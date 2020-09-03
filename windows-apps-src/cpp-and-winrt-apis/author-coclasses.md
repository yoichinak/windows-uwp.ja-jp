---
description: C++/WinRT は、Windows Runtime クラスを作成するのに役立つのと同様に、従来の COM コンポーネントを作成するのに役立ちます。
title: C++/WinRT での COM コンポーネントの作成
ms.date: 04/24/2019
ms.topic: article
keywords: windows 10, uwp, 標準, c++, cpp, winrt, プロジェクション, 作成者, COM, コンポーネント
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 83ea8b5cea95f034b5cdfe4f1750a0ffd0166f49
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89154576"
---
# <a name="author-com-components-with-cwinrt"></a>C++/WinRT での COM コンポーネントの作成

[C++/WinRT](./intro-to-using-cpp-with-winrt.md) は、Windows Runtime クラスを作成するのに役立つのと同様に、従来のコンポーネント オブジェクト モデル (COM) コンポーネント (またはコクラス) を作成するのに役立ちます。 このトピックでは方法を説明します。

## <a name="how-cwinrt-behaves-by-default-with-respect-to-com-interfaces"></a>COM インターフェイスに関する C++/WinRT の既定の動作

C++/WinRT の [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements) テンプレートは、ランタイム クラスとアクティベーション ファクトリの直接的または間接的な派生元です。

既定では、**winrt::implements** では [**IInspectable**](/windows/win32/api/inspectable/nn-inspectable-iinspectable) ベースのインターフェイスのみがサポートされ、従来の COM インターフェイスは警告なしで無視されます。 したがって、従来の COM インターフェイスに対するすべての **QueryInterface** (QI) の呼び出しは、**E_NOINTERFACE** で失敗します。

そのような状況に対処する方法は後で説明しますが、次に示すのは既定での動作を示すコード例です。

```idl
// Sample.idl
runtimeclass Sample
{
    Sample();
    void DoWork();
}

// Sample.h
#include "pch.h"
#include <shobjidl.h> // Needed only for this file.

namespace winrt::MyProject
{
    struct Sample : implements<Sample, IInitializeWithWindow>
    {
        IFACEMETHOD(Initialize)(HWND hwnd);
        void DoWork();
    }
}
```

次に示すのは、**Sample** クラスを使用するクライアントのコードです。

```cppwinrt
// Client.cpp
Sample sample; // Construct a Sample object via its projection.

// This next line crashes, because the QI for IInitializeWithWindow fails.
sample.as<IInitializeWithWindow>()->Initialize(hwnd); 
```

さいわい、**winrt::implements** で従来の COM インターフェイスをサポートするために必要なのは、C++/WinRT のヘッダーをインクルードする前に `unknwn.h` をインクルードすることだけです。

それを明示的に行うことも、`ole2.h` などの他のヘッダー ファイルをインクルードすることで間接的に行うこともできます。 推奨される 1 つの方法は、`wil\cppwinrt.h` ヘッダー ファイルをインクルードすることです。これは、[Windows 実装ライブラリ (WIL)](https://github.com/Microsoft/wil) に含まれます。 `wil\cppwinrt.h` ヘッダー ファイルを使うと、`winrt/base.h` の前に `unknwn.h` が確実にインクルードされるだけでなく、C++/WinRT と WIL が相互の例外とエラー コードを認識するように設定されます。

## <a name="a-simple-example-of-a-com-component"></a>COM コンポーネントの簡単な例

C++/WinRT を使って記述された COM コンポーネントの簡単な例を次に示します。 これは小さいアプリケーションの完全なリストなので、新しい **Windows コンソール アプリケーション (C++/WinRT)** プロジェクトの `pch.h` と `main.cpp` に貼り付けて、コードを試してみることができます。

```cppwinrt
// pch.h
#pragma once
#include <unknwn.h>
#include <winrt/Windows.Foundation.h>

// main.cpp : Defines the entry point for the console application.
#include "pch.h"

struct __declspec(uuid("ddc36e02-18ac-47c4-ae17-d420eece2281")) IMyComInterface : ::IUnknown
{
    virtual HRESULT __stdcall Call() = 0;
};

using namespace winrt;
using namespace Windows::Foundation;

int main()
{
    winrt::init_apartment();

    struct MyCoclass : winrt::implements<MyCoclass, IPersist, IStringable, IMyComInterface>
    {
        HRESULT __stdcall Call() noexcept override
        {
            return S_OK;
        }

        HRESULT __stdcall GetClassID(CLSID* id) noexcept override
        {
            *id = IID_IPersist; // Doesn't matter what we return, for this example.
            return S_OK;
        }

        winrt::hstring ToString()
        {
            return L"MyCoclass as a string";
        }
    };

    auto mycoclass_instance{ winrt::make<MyCoclass>() };
    CLSID id{};
    winrt::check_hresult(mycoclass_instance->GetClassID(&id));
    winrt::check_hresult(mycoclass_instance.as<IMyComInterface>()->Call());
}
```

「[C++/WinRT での COM コンポーネントの使用](consume-com.md)」も参照してください。

## <a name="a-more-realistic-and-interesting-example"></a>より現実的な興味深い例

このトピックの残りの部分では、C++/WinRT を使用して基本的なコクラス (COM コンポーネント、または COM クラス) とクラス ファクトリを実装する、最小限のコンソール アプリケーション プロジェクトを作成する手順について説明します。 アプリケーション例では、コールバック ボタン付きのトースト通知を配信する方法を示しています。コクラス (**INotificationActivationCallback** COM インターフェイスを実装する) によって、アプリケーションが起動され、ユーザーがトーストでそのボタンをクリックしたときにコールバックされます。

トースト通知機能領域の詳細な背景情報については、「[ローカル トースト通知の送信](../design/shell/tiles-and-notifications/send-local-toast.md)」を参照してください。 ただし、ドキュメントのそのセクションにあるコード例では C++/WinRT を使用していないため、このトピックに示すコードを使用することをお勧めします。

## <a name="create-a-windows-console-application-project-toastandcallback"></a>Windows コンソール アプリケーション プロジェクト (ToastAndCallback) を作成する

まず、Microsoft Visual Studio で、新しいプロジェクトを作成します。 **Windows コンソール アプリケーション (C++/WinRT)** プロジェクトを作成し、*ToastAndCallback* という名前を付けます。

`pch.h` を開き、任意の C++/WinRT ヘッダーのインクルードの前に `#include <unknwn.h>` を追加します。 結果を次に示します。`pch.h` の内容をこのリストで置き換えることができます。

```cppwinrt
// pch.h
#pragma once
#include <unknwn.h>
#include <winrt/Windows.Foundation.h>
```

`main.cpp` を開き、プロジェクト テンプレートによって生成された using ディレクティブを削除します。 代わりに、次のコード (ライブラリ、ヘッダー、必要な型名を指定する) を挿入します。 結果を次に示します。使用する `main.cpp` の内容をこのリストで置き換えることができます (以下のリストでは `main` のコードも削除されています。これは、その関数を後で置き換えるためです)。

```cppwinrt
// main.cpp : Defines the entry point for the console application.

#include "pch.h"

#pragma comment(lib, "advapi32")
#pragma comment(lib, "ole32")
#pragma comment(lib, "shell32")

#include <iomanip>
#include <iostream>
#include <notificationactivationcallback.h>
#include <propkey.h>
#include <propvarutil.h>
#include <shlobj.h>
#include <winrt/Windows.UI.Notifications.h>
#include <winrt/Windows.Data.Xml.Dom.h>

using namespace winrt;
using namespace Windows::Data::Xml::Dom;
using namespace Windows::UI::Notifications;

int main() { }
```

プロジェクトはまだビルドできません。コードを追加し終わった後、ビルドして実行するように求められます。

## <a name="implement-the-coclass-and-class-factory"></a>コクラスとクラス ファクトリを実装する

C++/WinRT では、コクラスとクラス ファクトリを [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements) 基本構造体から派生させることによって実装します。 上に示した 3 つの using ディレクティブの直後 (かつ、`main` の前) にこのコードを貼り付けて、トースト通知 COM アクティベーター コンポーネントを実装します。

```cppwinrt
static constexpr GUID callback_guid // BAF2FA85-E121-4CC9-A942-CE335B6F917F
{
    0xBAF2FA85, 0xE121, 0x4CC9, {0xA9, 0x42, 0xCE, 0x33, 0x5B, 0x6F, 0x91, 0x7F}
};

std::wstring const this_app_name{ L"ToastAndCallback" };

struct callback : winrt::implements<callback, INotificationActivationCallback>
{
    HRESULT __stdcall Activate(
        LPCWSTR app,
        LPCWSTR args,
        [[maybe_unused]] NOTIFICATION_USER_INPUT_DATA const* data,
        [[maybe_unused]] ULONG count) noexcept final
    {
        try
        {
            std::wcout << this_app_name << L" has been called back from a notification." << std::endl;
            std::wcout << L"Value of the 'app' parameter is '" << app << L"'." << std::endl;
            std::wcout << L"Value of the 'args' parameter is '" << args << L"'." << std::endl;
            return S_OK;
        }
        catch (...)
        {
            return winrt::to_hresult();
        }
    }
};

struct callback_factory : implements<callback_factory, IClassFactory>
{
    HRESULT __stdcall CreateInstance(
        IUnknown* outer,
        GUID const& iid,
        void** result) noexcept final
    {
        *result = nullptr;

        if (outer)
        {
            return CLASS_E_NOAGGREGATION;
        }

        return make<callback>()->QueryInterface(iid, result);
    }

    HRESULT __stdcall LockServer(BOOL) noexcept final
    {
        return S_OK;
    }
};
```

上記のコクラスの実装は、「[C++/WinRT での API の作成](./author-apis.md#if-youre-not-authoring-a-runtime-class)」に示されているのと同じパターンに従っています。 そのため、同じ手法を使用して、COM インターフェイスと Windows ランタイム インターフェイスを実装できます。 COM コンポーネントと Windows ランタイム クラスの機能は、インターフェイスを介して公開されます。 すべての COM インターフェイスは、最終的に [**IUnknown インターフェイス**](/windows/desktop/api/unknwn/nn-unknwn-iunknown) から派生します。 Windows ランタイムは COM に基づいています。1 つの違いは、Windows ランタイム インターフェイスは最終的に [**IInspectable インターフェイス**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable) から派生することです (**IInspectable** は **IUnknown** から派生)。

上記のコードのコクラスで、**INotificationActivationCallback::Activate** メソッドを実装しています。これは、ユーザーがトースト通知のコールバック ボタンをクリックしたときに呼び出される関数です。 しかし、その関数を呼び出す前に、コクラスのインスタンスを作成する必要があります。これを行うのは **IClassFactory::CreateInstance** 関数です。

今実装したコクラスは、通知の *COM アクティベーター*として知られており、上に示すように `callback_guid` 識別子 (**GUID** 型) という形式のクラス ID (CLSID) が付いています。 その識別子は、スタート メニューのショートカットおよび Windows レジストリ エントリの形式で後から使用します。 COM アクティベーターの CLSID、およびそれに関連付けられた COM サーバーへのパス (ここでビルドしている実行可能ファイルへのパス) は、コールバック ボタンがクリックされたときに、どのインスタンスを作成するか (通知がアクション センターでクリックされたかどうか) をトースト通知に認識させるためのメカニズムです。

## <a name="best-practices-for-implementing-com-methods"></a>COM メソッドを実装するためのベスト プラクティス

エラー処理とリソース管理の手法は密接に関連しています。 エラー コードよりも例外を使用する方が便利で実用的です。 リソース取得は初期化である (RAII) という考え方を採用すれば、エラー コードを明示的にチェックしてからリソースを明示的に解放することを回避できます。 このような明示的なチェックによってコードは必要以上に複雑になり、隠れたバグが多数の場所に発生しやすくなります。 そうではなく、RAII を使用し、例外をスローおよびキャッチします。 そうすることで、リソースの割り当ては例外セーフとなり、コードがシンプルになります。

ただし、COM メソッドの実装をエスケープする例外を許可することはできません。 COM メソッドで `noexcept` 指定子を使用することにより、それを保証することができます。 メソッドが終了する前に例外を処理する限り、メソッドの呼び出し先内のどこで例外がスローされてもかまいません。 `noexcept` を使用していても、メソッドをエスケープする例外を許可すると、アプリケーションは終了します。

## <a name="add-helper-types-and-functions"></a>ヘルパーの型と関数を追加する

このステップでは、コードの残りの部分で使用するヘルパーの型と関数をいくつか追加します。 まず、`main` の直前に以下を追加します。

```cppwinrt
struct prop_variant : PROPVARIANT
{
    prop_variant() noexcept : PROPVARIANT{}
    {
    }

    ~prop_variant() noexcept
    {
        clear();
    }

    void clear() noexcept
    {
        WINRT_VERIFY_(S_OK, ::PropVariantClear(this));
    }
};

struct registry_traits
{
    using type = HKEY;

    static void close(type value) noexcept
    {
        WINRT_VERIFY_(ERROR_SUCCESS, ::RegCloseKey(value));
    }

    static constexpr type invalid() noexcept
    {
        return nullptr;
    }
};

using registry_key = winrt::handle_type<registry_traits>;

std::wstring get_module_path()
{
    std::wstring path(100, L'?');
    uint32_t path_size{};
    DWORD actual_size{};

    do
    {
        path_size = static_cast<uint32_t>(path.size());
        actual_size = ::GetModuleFileName(nullptr, path.data(), path_size);

        if (actual_size + 1 > path_size)
        {
            path.resize(path_size * 2, L'?');
        }
    } while (actual_size + 1 > path_size);

    path.resize(actual_size);
    return path;
}

std::wstring get_shortcut_path()
{
    std::wstring format{ LR"(%ProgramData%\Microsoft\Windows\Start Menu\Programs\)" };
    format += (this_app_name + L".lnk");

    auto required{ ::ExpandEnvironmentStrings(format.c_str(), nullptr, 0) };
    std::wstring path(required - 1, L'?');
    ::ExpandEnvironmentStrings(format.c_str(), path.data(), required);
    return path;
}
```

## <a name="implement-the-remaining-functions-and-the-wmain-entry-point-function"></a>残りの関数と wmain エントリ ポイント関数を実装する

`main` 関数を削除し、代わりにこのコード リストを貼り付けます。これには、コクラスを登録してから、アプリケーションをコールバックできるトーストを配信するコードが含まれています。

```cppwinrt
void register_callback()
{
    DWORD registration{};

    winrt::check_hresult(::CoRegisterClassObject(
        callback_guid,
        make<callback_factory>().get(),
        CLSCTX_LOCAL_SERVER,
        REGCLS_SINGLEUSE,
        &registration));
}

void create_shortcut()
{
    auto link{ winrt::create_instance<IShellLink>(CLSID_ShellLink) };
    std::wstring module_path{ get_module_path() };
    winrt::check_hresult(link->SetPath(module_path.c_str()));

    auto store = link.as<IPropertyStore>();
    prop_variant value;
    winrt::check_hresult(::InitPropVariantFromString(this_app_name.c_str(), &value));
    winrt::check_hresult(store->SetValue(PKEY_AppUserModel_ID, value));
    value.clear();
    winrt::check_hresult(::InitPropVariantFromCLSID(callback_guid, &value));
    winrt::check_hresult(store->SetValue(PKEY_AppUserModel_ToastActivatorCLSID, value));

    auto file{ store.as<IPersistFile>() };
    std::wstring shortcut_path{ get_shortcut_path() };
    winrt::check_hresult(file->Save(shortcut_path.c_str(), TRUE));

    std::wcout << L"In " << shortcut_path << L", created a shortcut to " << module_path << std::endl;
}

void update_registry()
{
    std::wstring key_path{ LR"(SOFTWARE\Classes\CLSID\{????????-????-????-????-????????????})" };
    ::StringFromGUID2(callback_guid, key_path.data() + 23, 39);
    key_path += LR"(\LocalServer32)";
    registry_key key;

    winrt::check_win32(::RegCreateKeyEx(
        HKEY_CURRENT_USER,
        key_path.c_str(),
        0,
        nullptr,
        0,
        KEY_WRITE,
        nullptr,
        key.put(),
        nullptr));
    ::RegDeleteValue(key.get(), nullptr);

    std::wstring path{ get_module_path() };

    winrt::check_win32(::RegSetValueEx(
        key.get(),
        nullptr,
        0,
        REG_SZ,
        reinterpret_cast<BYTE const*>(path.c_str()),
        static_cast<uint32_t>((path.size() + 1) * sizeof(wchar_t))));

    std::wcout << L"In " << key_path << L", registered local server at " << path << std::endl;
}

void create_toast()
{
    XmlDocument xml;

    std::wstring toastPayload
    {
        LR"(
<toast>
  <visual>
    <binding template='ToastGeneric'>
      <text>)"
    };
    toastPayload += this_app_name;
    toastPayload += LR"(
      </text>
    </binding>
  </visual>
  <actions>
    <action content='Call back )";
    toastPayload += this_app_name;
    toastPayload += LR"(
' arguments='the_args' activationKind='Foreground' />
  </actions>
</toast>)";
    xml.LoadXml(toastPayload);

    ToastNotification toast{ xml };
    ToastNotifier notifier{ ToastNotificationManager::CreateToastNotifier(this_app_name) };
    notifier.Show(toast);
    ::Sleep(50); // Give the callback chance to display.
}

void LaunchedNormally(HANDLE, INPUT_RECORD &, DWORD &);
void LaunchedFromNotification(HANDLE, INPUT_RECORD &, DWORD &);

int wmain(int argc, wchar_t * argv[], wchar_t * /* envp */[])
{
    winrt::init_apartment();

    register_callback();

    HANDLE consoleHandle{ ::GetStdHandle(STD_INPUT_HANDLE) };
    INPUT_RECORD buffer{};
    DWORD events{};
    ::FlushConsoleInputBuffer(consoleHandle);

    if (argc == 1)
    {
        LaunchedNormally(consoleHandle, buffer, events);
    }
    else if (argc == 2 && wcscmp(argv[1], L"-Embedding") == 0)
    {
        LaunchedFromNotification(consoleHandle, buffer, events);
    }
}

void LaunchedNormally(HANDLE consoleHandle, INPUT_RECORD & buffer, DWORD & events)
{
    try
    {
        bool runningAsAdmin{ ::IsUserAnAdmin() == TRUE };
        std::wcout << this_app_name << L" is running" << (runningAsAdmin ? L" (administrator)." : L" (NOT as administrator).") << std::endl;

        if (runningAsAdmin)
        {
            create_shortcut();
            update_registry();
        }

        std::wcout << std::endl << L"Press 'T' to display a toast notification (press any other key to exit)." << std::endl;

        ::ReadConsoleInput(consoleHandle, &buffer, 1, &events);
        if (towupper(buffer.Event.KeyEvent.uChar.UnicodeChar) == L'T')
        {
            create_toast();
        }
    }
    catch (winrt::hresult_error const& e)
    {
        std::wcout << L"Error: " << e.message().c_str() << L" (" << std::hex << std::showbase << std::setw(8) << static_cast<uint32_t>(e.code()) << L")" << std::endl;
    }
}

void LaunchedFromNotification(HANDLE consoleHandle, INPUT_RECORD & buffer, DWORD & events)
{
    ::Sleep(50); // Give the callback chance to display its message.
    std::wcout << std::endl << L"Press any key to exit." << std::endl;
    ::ReadConsoleInput(consoleHandle, &buffer, 1, &events);
}
```

## <a name="how-to-test-the-example-application"></a>アプリケーション例をテストする方法

アプリケーションをビルドし、それを管理者として少なくとも 1 回実行して、登録とその他のセットアップのコードを実行します。 それを行う 1 つの方法として、Visual Studio を管理者として実行し、その後、Visual Studio からアプリを実行します。 タスク バーの Visual Studio を右クリックしてジャンプ リストを表示し、ジャンプ リストの Visual Studio を右クリックして、 **[管理者として実行]** をクリックします。 プロンプトに同意し、プロジェクトを開きます。 アプリケーションを実行するとき、アプリケーションが管理者として実行されているかどうかを示すメッセージが表示されます。 そうなっていない場合は、登録とその他のセットアップは実行されません。 アプリケーションの正常な動作のためには、その登録とその他のセットアップが少なくとも 1 回は実行される必要があります。

管理者としてアプリケーションを実行しているかどうかにかかわらず、トーストを表示させるには T キーを押します。 次に、ポップアップ表示されるトースト通知から直接またはアクション センターから **ToastAndCallback コールバック** ボタンをクリックすると、トースト通知が起動され、コクラスがインスタンス化されて、**INotificationActivationCallback::Activate** メソッドが実行されます。

## <a name="in-process-com-server"></a>インプロセス COM サーバー

上記の *ToastAndCallback* のアプリ例は、ローカル (またはプロセス外の) COM サーバーとして機能します。 これは、コクラスの CLSID を登録するために使用する Windows レジストリ キー [LocalServer32](/windows/desktop/com/localserver32) によって示されます。 ローカル COM サーバーによって、実行可能バイナリ (`.exe`) 内にコクラスがホストされます。

別の方法 (そして、おそらく可能性の高い方法) として、ダイナミック リンク ライブラリ (`.dll`) 内にコクラスをホストする選択もできます。 DLL 形式の COM サーバーは、インプロセス COM サーバーと呼ばれ、Windows レジストリ キー [InprocServer32](/windows/desktop/com/inprocserver32) を使用して登録される CLSID によって示されます。

### <a name="create-a-dynamic-link-library-dll-project"></a>ダイナミック リンク ライブラリ (DLL) プロジェクトを作成する

Microsoft Visual Studio で新しいプロジェクトを作成することによって、インプロセス COM サーバーの作成作業を開始できます。 **[Visual C++]**  >  **[Windows デスクトップ]**  >  **[ダイナミック リンク ライブラリ (DLL)]** プロジェクトを作成します。

新しいプロジェクトに C++WinRT サポートを追加するには、「[Windows デスクトップ アプリケーション プロジェクトを変更して C++/WinRT のサポートを追加する](./get-started.md#modify-a-windows-desktop-application-project-to-add-cwinrt-support)」に説明されている手順に従います。

### <a name="implement-the-coclass-class-factory-and-in-proc-server-exports"></a>コクラス、クラス ファクトリ、インプロセス サーバーのエクスポートを実装する

`dllmain.cpp` を開き、下に示すコード リストをそれに追加します。

C++/WinRT の Windows ランタイム クラスを実装する DLL が既にある場合は、下に示す **DllCanUnloadNow** 関数が既にあります。 その DLL にコクラスを追加する場合は、**DllGetClassObject** 関数を追加することができます。

互換性を維持したい既存の [Windows ランタイム C++ テンプレート ライブラリ (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl) コードがない場合は、示されているコードから WRL の部分を削除することができます。

```cppwinrt
// dllmain.cpp

struct MyCoclass : winrt::implements<MyCoclass, IPersist>
{
    HRESULT STDMETHODCALLTYPE GetClassID(CLSID* id) noexcept override
    {
        *id = IID_IPersist; // Doesn't matter what we return, for this example.
        return S_OK;
    }
};

struct __declspec(uuid("85d6672d-0606-4389-a50a-356ce7bded09"))
    MyCoclassFactory : winrt::implements<MyCoclassFactory, IClassFactory>
{
    HRESULT STDMETHODCALLTYPE CreateInstance(IUnknown *pUnkOuter, REFIID riid, void **ppvObject) noexcept override
    {
        try
        {
            return winrt::make<MyCoclass>()->QueryInterface(riid, ppvObject);
        }
        catch (...)
        {
            return winrt::to_hresult();
        }
    }

    HRESULT STDMETHODCALLTYPE LockServer(BOOL fLock) noexcept override
    {
        // ...
        return S_OK;
    }

    // ...
};

HRESULT __stdcall DllCanUnloadNow()
{
#ifdef _WRL_MODULE_H_
    if (!::Microsoft::WRL::Module<::Microsoft::WRL::InProc>::GetModule().Terminate())
    {
        return S_FALSE;
    }
#endif

    if (winrt::get_module_lock())
    {
        return S_FALSE;
    }

    winrt::clear_factory_cache();
    return S_OK;
}

HRESULT __stdcall DllGetClassObject(GUID const& clsid, GUID const& iid, void** result)
{
    try
    {
        *result = nullptr;

        if (clsid == __uuidof(MyCoclassFactory))
        {
            return winrt::make<MyCoclassFactory>()->QueryInterface(iid, result);
        }

#ifdef _WRL_MODULE_H_
        return ::Microsoft::WRL::Module<::Microsoft::WRL::InProc>::GetModule().GetClassObject(clsid, iid, result);
#else
        return winrt::hresult_class_not_available().to_abi();
#endif
    }
    catch (...)
    {
        return winrt::to_hresult();
    }
}
```

### <a name="support-for-weak-references"></a>弱参照のサポート

「[C++/WinRT の弱参照](weak-references.md#weak-references-in-cwinrt)」も参照してください。

C++/WinRT (具体的には、[**winrt::implements**](/uwp/cpp-ref-for-winrt/implements) 基本構造体テンプレート) では、使用している型で [**IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable) (または **IInspectable** から派生する任意のインターフェイス) を実装する場合、自動的に [**IWeakReferenceSource**](/windows/desktop/api/weakreference/nn-weakreference-iweakreferencesource) が実装されます。

これは、**IWeakReferenceSource** と [**IWeakReference**](/windows/desktop/api/weakreference/nn-weakreference-iweakreference) が Windows ランタイム型用に設計されているためです。 したがって、**winrt::Windows::Foundation::IInspectable** (または **IInspectable** から派生するインターフェイス) を実装に追加するだけで、コクラスに対する弱参照サポートを有効にすることができます。

```cppwinrt
struct MyCoclass : winrt::implements<MyCoclass, IMyComInterface, winrt::Windows::Foundation::IInspectable>
{
    //  ...
};
```

## <a name="important-apis"></a>重要な API
* [IInspectable インターフェイス](/windows/desktop/api/inspectable/nn-inspectable-iinspectable)
* [IUnknown インターフェイス](/windows/desktop/api/unknwn/nn-unknwn-iunknown)
* [winrt::implements 構造体テンプレート](/uwp/cpp-ref-for-winrt/implements)

## <a name="related-topics"></a>関連トピック
* [C++/WinRT で API を作成する](./author-apis.md)
* [C++/WinRT での COM コンポーネントの使用](consume-com.md)
* [ローカル トースト通知の送信](../design/shell/tiles-and-notifications/send-local-toast.md)