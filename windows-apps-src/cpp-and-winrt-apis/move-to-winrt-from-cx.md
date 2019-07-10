---
description: このトピックでは、C++/CX コードを C++/WinRT の同等のコードに移植する方法について説明します。
title: C++/CX から C++/WinRT への移行
ms.date: 01/17/2019
ms.topic: article
keywords: windows 10, uwp, 標準, c++, cpp, winrt, プロジェクション, 移植, 移行, C++/CX
ms.localizationpriority: medium
ms.openlocfilehash: 7fbe10e41da1b330d6f5042bea109a8a0e04f8ad
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/13/2019
ms.locfileid: "66360159"
---
# <a name="move-to-cwinrt-from-ccx"></a>C++/CX から C++/WinRT への移行

このトピックでは、[C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) プロジェクト内のコードを [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) の同等のコードに移植する方法について説明します。

## <a name="porting-strategies"></a>移植方針

C++/CX コードを徐々に C++/WinRT に移植したい場合、それは可能です。 C++/CX コードと C++/WinRT コードは同じプロジェクト内に共存できます。ただし、XAML コンパイラーのサポートと Windows ランタイム コンポーネントについては例外となります。 この 2 つの例外の場合は、同じプロジェクト内で C++/CX または C++/WinRT のいずれかをターゲットにする必要があります。

> [!IMPORTANT]
> ご利用のプロジェクトで XAML アプリケーションを構築する場合、Microsoft では 1 つのワークフローとして、C++/WinRT プロジェクト テンプレートのいずれかを使用して Visual Studio でまず新しいプロジェクトを作成することをお勧めします ([Visual Studio での C++/WinRT のサポート](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)に関するページを参照)。 次に、C++/CX プロジェクトからソース コードとマークアップのコピーを開始します。 新しい XAML ページを追加するには、 **[プロジェクト]** \> **[新しい項目の追加]** \> **[Visual C++]**  >  **[空白のページ] (C++/WinRT)** の順に選択します。
>
> または、Windows ランタイム コンポーネントを使用して、移植時に XAML C++/CX プロジェクトからコードを除外することもできます。 できる限り多くの C++/CX コードをコンポーネントに移動してから、XAML プロジェクトを C++/WinRT に変更します。 または、XAML プロジェクトは C++/CX のままとし、新しい C++/WinRT コンポーネントを作成して、XAML プロジェクトからコンポーネントへの C++/CX コードの移植をします。 また、同じソリューション内に C++/CX コンポーネント プロジェクトと C++/WinRT コンポーネント プロジェクトを一緒に用意し、ご利用のアプリケーション プロジェクトから両方を参照し、一方からもう一方に徐々に移植することもできます。 同じプロジェクト内で 2 つの言語プロジェクションを使用する方法の詳細については、「[C++/WinRT と C++/CX 間の相互運用](interop-winrt-cx.md)」を参照してください。

> [!NOTE]
> [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) と Windows SDK の両方で、ルート名前空間 **Windows** で型を宣言します。 C++/WinRT に投影された Windows 型は Windows 型と同じ完全修飾名を持ちますが、 C++ **winrt** 名前空間に配置されます。 これらの異なる名前空間では、独自のペースで C++/CX から C++/WinRT へ移植できます。

上記の例外を念頭に置いて、C++/WinRT への C++/CX プロジェクトの移植の最初の手順は、それに C++/WinRT サポートを手動で追加することです ([C++/WinRT の Visual Studio サポート](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)に関するページを参照してください)。 そのためには、[Microsoft.Windows.CppWinRT NuGet パッケージ](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/)をご利用のプロジェクトにインストールします。 Visual Studio でプロジェクションを開き、 **[プロジェクト]** \> **[NuGet パッケージの管理]** \> **[参照]** の順にクリックし、検索ボックスに「**Microsoft.Windows.CppWinRT**」を入力するか貼り付けます。検索結果の項目を選択し、 **[インストール]** をクリックしてそのプロジェクトのパッケージをインストールします。 その変更による 1 つの効果は、C++/CX のサポートがプロジェクトで無効になることです。 ビルド メッセージが C++/CX のすべての依存関係の検索および移植に役立つように、サポートを無効にしたままにすることをお勧めします。または、サポートをもう一度有効にして (プロジェクトのプロパティで、 **[C/C++]** \> **[全般]** \> **[Windows ランタイム拡張機能の使用]** \> **[はい (/ZW)]** の順に選択)、徐々に移行することもできます。

プロジェクトのプロパティ ( **[全般]** \> **[ターゲット プラットフォーム バージョン]** ) が 10.0.17134.0 (Windows 10 バージョン 1803) 以上に確実に設定されるようにします。

プリコンパイル済みヘッダー ファイル (通常は `pch.h`) で、`winrt/base.h` を含めます。

```cppwinrt
#include <winrt/base.h>
```

C++/WinRT の投影された Windows API ヘッダー (たとえば、`winrt/Windows.Foundation.h`) を含める場合は、それが自動的に含められるため、このように明示的に `winrt/base.h` を含める必要はありません。

プロジェクトで [Windows ランタイム C++ テンプレート ライブラリ (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl) 型も使用している場合は、「[WRL から C++/WinRT への移行](move-to-winrt-from-wrl.md)」を参照してください。

## <a name="parameter-passing"></a>パラメーターの引き渡し
C++/CX ソース コードを記述するときは、ハット (\^) 参照のように C++/CX 型を関数パラメーターとして渡します。

```cppcx
void LogPresenceRecord(PresenceRecord^ record);
```

C++/WinRT では、同期関数に既定で `const&` パラメーターを使用する必要があります。 これにより、コピーとインター ロック オーバーヘッドが回避されます。 ただし、コルーチンが値による呼び出しをして有効期間の問題を回避するように、値渡しを使用する必要があります (詳細については、「[C++/WinRT を使用した同時実行と非同期操作](concurrency.md)」を参照してください)。

```cppwinrt
void LogPresenceRecord(PresenceRecord const& record);
IASyncAction LogPresenceRecordAsync(PresenceRecord const record);
```

C++/WinRT オブジェクトは、基本的に Windows ランタイムのバッキング オブジェクトへのインターフェイス ポインターを保持する値です。 C++/WinRT オブジェクトをコピーすると、コンパイラはカプセル化されたインターフェイス ポインターをコピーし、その参照カウントをインクリメントします。 コピーの最終的なデストラクションには、参照カウントのデクリメントも含まれます。 そのため、必要な場合にのみ、コピーのオーバーヘッドが発生します。

## <a name="variable-and-field-references"></a>変数とフィールドの参照
C++/CX ソース コードを記述するときに、ハット (\^) 変数を使用して Windows ランタイム オブジェクトを参照し、矢印 (-&gt;) 演算子を使用してハット変数を逆参照します。

```cppcx
IVectorView<User^>^ userList = User::Users;

if (userList != nullptr)
{
    for (UINT32 iUser = 0; iUser < userList->Size; ++iUser)
    ...
```

同等の C++/WinRT コードに移植する場合は、ハットを取り除き、矢印演算子 (-&gt;) をドット演算子 (.) に変更することにより、開始できるようになります。 C++/WinRT のプロジェクションが実行された型は値であり、ポインターではありません。

```cppwinrt
IVectorView<User> userList = User::Users();

if (userList != nullptr)
{
    for (UINT32 iUser = 0; iUser < userList.Size(); ++iUser)
    ...
```

C++/CX ハット ポインター用の既定のコンストラクターにより、それを null に初期化されます。 次に示したのは、正しい型の変数/フィールドを作成するための C++/CX コード例でが、初期化は行われません。 つまり、それによって最初は **TextBlock** は参照されません。後で参照を割り当てるつもりです。

```cppcx
TextBlock^ textBlock;

class MyClass
{
    TextBlock^ textBlock;
};
```

C++/WinRT での同等のものについては、「[初期化の遅延](consume-apis.md#delayed-initialization)」を参照してください。

## <a name="properties"></a>プロパティ
C++/CX 言語拡張機能には、プロパティの概念が含まれています。 C++/CX ソース コードを記述するときに、フィールドと同様にプロパティにアクセスできます。 標準 C++ にはプロパティの概念がないため、C++/WinRT では、Get 関数と Set 関数を呼び出します。

次の例では、**XboxUserId**、**UserState**、**PresenceDeviceRecords**、**Size** はすべてプロパティです。

### <a name="retrieving-a-value-from-a-property"></a>プロパティからの値の取得
C++/CX でプロパティの値を取得する方法は次のとおりです。

```cppcx
void Sample::LogPresenceRecord(PresenceRecord^ record)
{
    auto id = record->XboxUserId;
    auto state = record->UserState;
    auto size = record->PresenceDeviceRecords->Size;
}
```

同等の C++/WinRT ソース コードは、プロパティと同じ名前を持ち、パラメーターのない関数を呼び出します。

```cppwinrt
void Sample::LogPresenceRecord(PresenceRecord const& record)
{
    auto id = record.XboxUserId();
    auto state = record.UserState();
    auto size = record.PresenceDeviceRecords().Size();
}
```

**PresenceDeviceRecords** 関数は、それ自体が **Size** 関数を持つ Windows ランタイム オブジェクトを返します。 返されたオブジェクトは C++/WinRT の投影された型でもあるため、ドット演算子を使用して逆参照し、**Size** を呼び出します。

### <a name="setting-a-property-to-a-new-value"></a>新しい値へのプロパティの設定
新しい値にプロパティを設定するには、同様のパターンに従います。 まず、C++/CX で次の操作を行います。

```cppcx
record->UserState = newValue;
```

C++/WinRT で対応する操作を行うには、プロパティと同じ名前の関数を呼び出し、引数を渡します。

```cppwinrt
record.UserState(newValue);
```

## <a name="creating-an-instance-of-a-class"></a>クラスのインスタンスの作成
C++/CX オブジェクトは、それに対するハンドル (通常はハット (\^) 参照と呼ばれます) を介して操作します。 `ref new` キーワードにより新しいオブジェクトを作成します。これにより、[**RoActivateInstance**](https://docs.microsoft.com/windows/desktop/api/roapi/nf-roapi-roactivateinstance) が呼び出され、ランタイム クラスの新しいインスタンスがアクティブ化されます。

```cppcx
using namespace Windows::Storage::Streams;

class Sample
{
private:
    Buffer^ m_gamerPicBuffer = ref new Buffer(MAX_IMAGE_SIZE);
};
```

C++/WinRT オブジェクトは値であるため、スタックに割り当てるか、オブジェクトのフィールドとして割り当てることができます。 C++/WinRT オブジェクトを割り当てるために、`ref new` (または `new`) は使用*しません*。 バックグラウンドで **RoActivateInstance** は引き続き呼び出されています。

```cppwinrt
using namespace winrt::Windows::Storage::Streams;

struct Sample
{
private:
    Buffer m_gamerPicBuffer{ MAX_IMAGE_SIZE };
};
```

リソースを初期化するコストが高い場合は、実際に必要になるまで初期化を遅延するのが一般的です。

```cppcx
using namespace Windows::Storage::Streams;

class Sample
{
public:
    void DelayedInit()
    {
        // Allocate the actual buffer.
        m_gamerPicBuffer = ref new Buffer(MAX_IMAGE_SIZE);
    }

private:
    Buffer^ m_gamerPicBuffer;
};
```

同じコードが C++/WinRT に移植されました。 `nullptr` コンストラクターを使っていることに注目してください。 コンストラクターの詳細については、「[C++/WinRT での API の使用](consume-apis.md)」を参照してください。

```cppwinrt
using namespace winrt::Windows::Storage::Streams;

struct Sample
{
    void DelayedInit()
    {
        // Allocate the actual buffer.
        m_gamerPicBuffer = Buffer(MAX_IMAGE_SIZE);
    }

private:
    Buffer m_gamerPicBuffer{ nullptr };
};
```

## <a name="converting-from-a-base-runtime-class-to-a-derived-one"></a>基本ランタイム クラスから派生クラスへの変換
派生型のオブジェクトを参照する既知の reference-to-base を使用することは一般的です。 C++/CX では、`dynamic_cast` を使用して、reference-to-base を reference-to-derived に "*キャスト*" します。 `dynamic_cast` は、実際には [**QueryInterface**](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_)) の隠された呼び出しです。 次に示すのは典型的な例です。依存関係プロパティの変更イベントを処理しており、**DependencyObject** から、依存関係プロパティを所有する実際の型にキャストし直す必要があります。

```cppcx
void BgLabelControl::OnLabelChanged(Windows::UI::Xaml::DependencyObject^ d, Windows::UI::Xaml::DependencyPropertyChangedEventArgs^ e)
{
    BgLabelControl^ theControl{ dynamic_cast<BgLabelControl^>(d) };

    if (theControl != nullptr)
    {
        // succeeded ...
    }
}
```

同等の C++/WinRT コードでは、`dynamic_cast` が [**IUnknown :: try_as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntry_as-function) 関数 (**QueryInterface** をカプセル化する) の呼び出しによって置き換えられます。 代わりに [**IUnknown :: as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function) を呼び出すオプションもあります。このオプションでは、必須のインターフェイス (要求している型の既定のインターフェイス) に対するクエリが返されない場合に例外がスローされます。 次に C++/WinRT コード例を示します。

```cppwinrt
void BgLabelControl::OnLabelChanged(Windows::UI::Xaml::DependencyObject const& d, Windows::UI::Xaml::DependencyPropertyChangedEventArgs const& e)
{
    if (BgLabelControlApp::BgLabelControl theControl{ d.try_as<BgLabelControlApp::BgLabelControl>() })
    {
        // succeeded ...
    }

    try
    {
        BgLabelControlApp::BgLabelControl theControl{ d.as<BgLabelControlApp::BgLabelControl>() };
        // succeeded ...
    }
    catch (winrt::hresult_no_interface const&)
    {
        // failed ...
    }
}
```

## <a name="event-handling-with-a-delegate"></a>デリゲートを使用したイベント処理
この場合はデリゲートとしてラムダ関数を使用して、C++/CX でイベントを処理する典型的な例を次に示します。

```cppcx
auto token = myButton->Click += ref new RoutedEventHandler([=](Platform::Object^ sender, RoutedEventArgs^ args)
{
    // Handle the event.
    // Note: locals are captured by value, not reference, since this handler is delayed.
});
```

C++/WinRT で対応するコードは次のとおりです。

```cppwinrt
auto token = myButton().Click([=](IInspectable const& sender, RoutedEventArgs const& args)
{
    // Handle the event.
    // Note: locals are captured by value, not reference, since this handler is delayed.
});
```

ラムダ関数の代わりに、デリゲートを自由関数として実装するか、またはメンバー関数へのポインターとして実装するかを選択できます。 詳細については、「[C++/WinRT でのデリゲートを使用したイベントの処理](handle-events.md)」を参照してください。

イベントとデリゲートが内部的に使用される C++/CX コードベース (バイナリではなく) から移植する場合は、[**winrt::delegate**](/uwp/cpp-ref-for-winrt/delegate) を使用すると、C++/WinRT でそのパターンを複製できます。 「[パラメーター化されたデリゲート、単純なシグナル、およびプロジェクト内でのコールバック](author-events.md#parameterized-delegates-simple-signals-and-callbacks-within-a-project)」も参照してください。

## <a name="revoking-a-delegate"></a>デリゲートの取り消し
C++/CX では、`-=` 演算子を使用して前のイベント登録を取り消します。

```cppcx
myButton->Click -= token;
```

C++/WinRT で対応するコードは次のとおりです。

```cppwinrt
myButton().Click(token);
```

詳細とオプションについては、「[登録済みデリゲートの取り消し](handle-events.md#revoke-a-registered-delegate)」を参照してください。

## <a name="mapping-ccx-platform-types-to-cwinrt-types"></a>C++/WinRT 型への C++/CX **Platform** 型のマッピング
C++/CX は **Platform** 名前空間でいくつかのデータ型を提供します。 これらの型は標準 C++ ではないため、Windows ランタイム言語拡張機能を有効にしたとき (Visual Studio プロジェクトのプロパティの **[C/C++]**  >  **[全般]**  >  **[Windows ランタイム拡張機能の使用]**  >  **[はい (/ZW)]** ) にのみ使用できます。 下の表は、**Platform** 型を C++/WinRT の同等の型に移植するときに役立ちます。 完了したら、C++/WinRT は標準 C++ であるため、`/ZW` オプションをオフにすることができます。

| C++/CX | C++/WinRT |
| ---- | ---- |
| **Platform::Agile\^** | [**winrt::agile_ref**](/uwp/cpp-ref-for-winrt/agile-ref) |
| **Platform::Array\^** | 「[**Platform::Array\^** を移植する](#port-platformarray)」を参照してください。 |
| **Platform::Exception\^** | [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) |
| **Platform::InvalidArgumentException\^** | [**winrt::hresult_invalid_argument**](/uwp/cpp-ref-for-winrt/error-handling/hresult-invalid-argument) |
| **Platform::Object\^** | **winrt::Windows::Foundation::IInspectable** |
| **Platform::String\^** | [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) |

### <a name="port-platformagile-to-winrtagileref"></a>**Platform::Agile\^** を **winrt::agile_ref** に移植する
C++/CX の **Platform::agile\^** 型は、任意のスレッドからアクセスできる Windows ランタイム クラスとなります。 C++/WinRT の同等の型は [**winrt::agile_ref**](/uwp/cpp-ref-for-winrt/agile-ref) です。

C++/CX で、次の操作を行います。

```cppcx
Platform::Agile<Windows::UI::Core::CoreWindow> m_window;
```

C++/WinRT で、次の操作を行います。

```cppwinrt
winrt::agile_ref<Windows::UI::Core::CoreWindow> m_window;
```

### <a name="port-platformarray"></a>**Platform::Array\^** を移植する
オプションには、初期化子リスト、**std::array**、または **std::vector** の使用が含まれます。 詳細およびコード例については、「[標準的な初期化子リスト](/windows/uwp/cpp-and-winrt-apis/std-cpp-data-types#standard-initializer-lists)」および「[標準的な配列とベクトル](/windows/uwp/cpp-and-winrt-apis/std-cpp-data-types#standard-arrays-and-vectors)」を参照してください。

### <a name="port-platformexception-to-winrthresulterror"></a>**Platform::Exception\^** を **winrt::hresult_error** に移植する
Windows ランタイム API から S\_OK HRESULT 以外が返された場合、**Platform::Exception\^** 型が C++/CX で生成されます。 C++/WinRT の同等の型は [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) です。

C++/WinRT に移植するには、**Platform::Exception\^** を使用するすべてのコードを、**winrt::hresult_error** を使用するように変更します。

C++/CX で、次の操作を行います。

```cppcx
catch (Platform::Exception^ ex)
```

C++/WinRT で、次の操作を行います。

```cppwinrt
catch (winrt::hresult_error const& ex)
```

C++/WinRT には、これらの例外クラスが用意されています。

| 例外の型 | 基本クラス | HRESULT |
| ---- | ---- | ---- |
| [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) | | [  **hresult_error::to_abi**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error#hresult_errorto_abi-function) 呼び出し |
| [**winrt::hresult_access_denied**](/uwp/cpp-ref-for-winrt/error-handling/hresult-access-denied) | **winrt::hresult_error** | E_ACCESSDENIED |
| [**winrt::hresult_canceled**](/uwp/cpp-ref-for-winrt/error-handling/hresult-canceled) | **winrt::hresult_error** | ERROR_CANCELLED |
| [**winrt::hresult_changed_state**](/uwp/cpp-ref-for-winrt/error-handling/hresult-changed-state) | **winrt::hresult_error** | E_CHANGED_STATE |
| [**winrt::hresult_class_not_available**](/uwp/cpp-ref-for-winrt/error-handling/hresult-class-not-available) | **winrt::hresult_error** | CLASS_E_CLASSNOTAVAILABLE |
| [**winrt::hresult_illegal_delegate_assignment**](/uwp/cpp-ref-for-winrt/error-handling/hresult-illegal-delegate-assignment) | **winrt::hresult_error** | E_ILLEGAL_DELEGATE_ASSIGNMENT |
| [**winrt::hresult_illegal_method_call**](/uwp/cpp-ref-for-winrt/error-handling/hresult-illegal-method-call) | **winrt::hresult_error** | E_ILLEGAL_METHOD_CALL |
| [**winrt::hresult_illegal_state_change**](/uwp/cpp-ref-for-winrt/error-handling/hresult-illegal-state-change) | **winrt::hresult_error** | E_ILLEGAL_STATE_CHANGE |
| [**winrt::hresult_invalid_argument**](/uwp/cpp-ref-for-winrt/error-handling/hresult-invalid-argument) | **winrt::hresult_error** | E_INVALIDARG |
| [**winrt::hresult_no_interface**](/uwp/cpp-ref-for-winrt/error-handling/hresult-no-interface) | **winrt::hresult_error** | E_NOINTERFACE |
| [**winrt::hresult_not_implemented**](/uwp/cpp-ref-for-winrt/error-handling/hresult-not-implemented) | **winrt::hresult_error** | E_NOTIMPL |
| [**winrt::hresult_out_of_bounds**](/uwp/cpp-ref-for-winrt/error-handling/hresult-out-of-bounds) | **winrt::hresult_error** | E_BOUNDS |
| [**winrt::hresult_wrong_thread**](/uwp/cpp-ref-for-winrt/error-handling/hresult-wrong-thread) | **winrt::hresult_error** | RPC_E_WRONG_THREAD |

各クラスは (**hresult_error** 基本クラスを介して)、エラーの HRESULT を返す [**to_abi**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error#hresult_errorto_abi-function) 関数を指定し、その HRESULT の文字列表現を返す [**message**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error#hresult_errormessage-function) 関数を指定します。

C++/CX で例外をスローする例を次に示します。

```cppcx
throw ref new Platform::InvalidArgumentException(L"A valid User is required");
```

また、C++/WinRT での同等のコードは次のとおりです。

```cppwinrt
throw winrt::hresult_invalid_argument{ L"A valid User is required" };
```

### <a name="port-platformobject-to-winrtwindowsfoundationiinspectable"></a>**Platform::Object\^** を **winrt::Windows::Foundation::IInspectable** に移植する
すべての C++/WinRT 型と同様に、**winrt::Windows::Foundation::IInspectable** は値の型です。 その型の変数を null に初期化する方法は次のとおりです。

```cppwinrt
winrt::Windows::Foundation::IInspectable var{ nullptr };
```

### <a name="port-platformstring-to-winrthstring"></a>**Platform::String\^** を **winrt::hstring** に移植する
**Platform::String\^\^** は Windows ランタイム HSTRING ABI 型と同等です。 C++/WinRT では、同等の型は [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) です。 ただし、C++/WinRT では、**std::wstring** などの C++ 標準ライブラリのワイド文字列型、およびワイド文字列リテラルを使用して Windows ランタイム API を呼び出すことができます。 詳細とコード例については、「[C++/WinRT での文字列の処理](strings.md)」を参照してください。

C++/CX では、[**Platform::String::Data**](https://docs.microsoft.com/en-us/cpp/cppcx/platform-string-class#data) プロパティにアクセスして、C スタイルの **const wchar_t\*** 配列 (たとえば、それを **std::wcout** に渡すために) として文字列を取得できます。

```cppcx
auto var{ titleRecord->TitleName->Data() };
```

C++/WinRT で同じ操作を行うには、[**hstring::c_str**](/uwp/api/windows.foundation.uri.-ctor#Windows_Foundation_Uri__ctor_System_String_) 関数を使用して null で終了する C スタイルの文字列バージョンを取得します。これは **std::wstring** から取得する場合と同様です。

```cppwinrt
auto var{ titleRecord.TitleName().c_str() };
```

文字列を取るかまたは文字列を返す API の実装に関しては、通常、**Platform::String\^** を使用する C++/CX コードを変更して、代わりに **winrt::hstring** を使用します。

文字列を取る C++/CX API の例を次に示します。

```cppcx
void LogWrapLine(Platform::String^ str);
```

C++/WinRT では、次のようにその API を [MIDL 3.0](/uwp/midl-3) で宣言できます。

```idl
// LogType.idl
void LogWrapLine(String str);
```

次に C++/WinRT ツール チェーンは次のようなソース コードを生成します。

```cppwinrt
void LogWrapLine(winrt::hstring const& str);
```

#### <a name="tostring"></a>ToString()

C++/CX には [Object::ToString](/cpp/cppcx/platform-object-class?view=vs-2017#tostring) メソッドが用意されています。

```cppcx
int i{ 2 };
auto s{ i.ToString() }; // s is a Platform::String^ with value L"2".
```

C++/WinRT ではこの機能は直接提供されていませんが、代替機能があります。

```cppwinrt
int i{ 2 };
auto s{ std::to_wstring(i) }; // s is a std::wstring with value L"2".
```

## <a name="important-apis"></a>重要な API
* [winrt::delegate 構造体テンプレート](/uwp/cpp-ref-for-winrt/delegate)
* [winrt::hresult_error 構造体](/uwp/cpp-ref-for-winrt/error-handling/hresult-error)
* [winrt::hstring 構造体](/uwp/cpp-ref-for-winrt/hstring)
* [winrt 名前空間](/uwp/cpp-ref-for-winrt/winrt)

## <a name="related-topics"></a>関連トピック
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [C++/WinRT でのイベントの作成](author-events.md)
* [C++/WinRT を使用した同時実行操作と非同期操作](concurrency.md)
* [C++/WinRT で API を使用する](consume-apis.md)
* [C++/WinRT でのデリゲートを使用したイベントの処理](handle-events.md)
* [C++/WinRT と C++/CX 間の相互運用](interop-winrt-cx.md)
* [Microsoft インターフェイス定義言語 3.0 リファレンス](/uwp/midl-3)
* [WRL から C++/WinRT への移行](move-to-winrt-from-wrl.md)
* [C++/WinRT での文字列の処理](strings.md)
