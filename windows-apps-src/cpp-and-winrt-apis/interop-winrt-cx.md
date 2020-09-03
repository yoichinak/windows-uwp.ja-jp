---
description: このトピックでは、[C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) と [C++/WinRT](./intro-to-using-cpp-with-winrt.md) オブジェクト間の変換に使用できる 2 つのヘルパー関数について説明します。
title: C++/WinRT と C++/CX 間の相互運用
ms.date: 10/09/2018
ms.topic: article
keywords: Windows 10、uwp、標準、c++、cpp、winrt、プロジェクション、ポート、移行、相互運用、C++/CX
ms.localizationpriority: medium
ms.openlocfilehash: 8ef3b45222b5e9324dc76d7a81a8d096a569595d
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89157376"
---
# <a name="interop-between-cwinrt-and-ccx"></a>C++/WinRT と C++/CX 間の相互運用

このトピックを読む前に、「[C++/CX から C++/WinRT への移行](./move-to-winrt-from-cx.md)」のトピックの情報が必要です。 このトピックでは、[C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) プロジェクトを [C++/WinRT](./intro-to-using-cpp-with-winrt.md) に移植するための 2 つの主要な方法について説明します。

- プロジェクト全体を 1 回の動作で移植する。 プロジェクトが大きすぎない場合の最も簡単なオプションです。 Windows ランタイム コンポーネント プロジェクトがある場合は、この方法が唯一のオプションです。
- プロジェクトを段階的に移植する (コードベースのサイズまたは複雑さによってはこれが必要になる場合があります)。 しかしこの方法では、C++/CX と C++/WinRT のコードがしばらくの間同じプロジェクト内に共存する移植プロセスに従うことが必要になります。 XAML プロジェクトの場合、XAML ページの種類は常に、すべて C++/WinRT "*または*" すべて C++/CX の "*いずれか*" である必要があります。

この相互運用のトピックは、プロジェクトを段階的に移植する必要がある場合の "*2 番目の*" 方法に関連しています。 このトピックでは、同じプロジェクト内の C++/CX オブジェクトを C++/WinRT オブジェクトに (およびその逆に) 変換するために使用できる 2 つのヘルパー関数について説明します。

これらのヘルパー関数は、C++/CX から C++/WinRT にコードを段階的に移植するときに非常に便利です。 または、移植するかどうかにかかわらず、1 つのプロジェクトで C++/WinRT と C++/CX の両方の言語プロジェクションを使用し、これらのヘルパー関数を使用して 2 つの間で相互運用することができます。

このトピックを読んだ後、PPL タスクとコルーチンを同じプロジェクトで同時にサポートする方法を示す情報とコードの例 (タスク チェーンからコルーチンを呼び出す場合など) については、高度なトピック「[非同期性、および C++/WinRT と C++/CX 間の相互運用](./interop-winrt-cx-async.md)」を参照してください。

## <a name="the-from_cx-and-to_cx-functions"></a>**from_cx** および **to_cx** 関数

ここでは、`interop_helpers.h` という名前のヘッダー ファイルのソース コード リストを次に示します。このファイルには、2 つの変換ヘルパー関数が含まれています。 プロジェクトを段階的に移植すると、まだ C++/CX である部分と、既に C++/WinRT に移植された部分が存在することになります。 これらのヘルパー関数を使用すると、このような 2 つの部分の境界点で、プロジェクト内のオブジェクトを C++/CX と C++/WinRT との間で変換できます。

コード リストの後のセクションでは、この 2 つの関数と、プロジェクトでヘッダー ファイルを作成して使用する方法について説明します。

```cppwinrt
// interop_helpers.h
#pragma once

template <typename T>
T from_cx(Platform::Object^ from)
{
    T to{ nullptr };

    winrt::check_hresult(reinterpret_cast<::IUnknown*>(from)
        ->QueryInterface(winrt::guid_of<T>(), winrt::put_abi(to)));

    return to;
}

template <typename T>
T^ to_cx(winrt::Windows::Foundation::IUnknown const& from)
{
    return safe_cast<T^>(reinterpret_cast<Platform::Object^>(winrt::get_abi(from)));
}
```

### <a name="the-from_cx-function"></a>**from_cx** 関数

**from_cx** ヘルパー関数では、C++/CX オブジェクトを同等の C++/WinRT オブジェクトに変換します。 この関数は、C++/CX オブジェクトを基礎となる [**IUnknown**](/windows/win32/api/unknwn/nn-unknwn-iunknown) インターフェイス ポインターにキャストします。 次に、このポインター上で [**QueryInterface**](/windows/win32/api/unknwn/nf-unknwn-iunknown-queryinterface(q_)) を呼び出し、C++/WinRT オブジェクトの既定のインターフェイスを照会します。 **QueryInterface** は、C++/CX `safe_cast` 拡張と同等の Windows ランタイム アプリケーション バイナリ インターフェイス (ABI) です。 [**winrt::put_abi**](/uwp/cpp-ref-for-winrt/put-abi) 関数は、別の値に設定できるように C++/WinRT オブジェクトの基礎となる **IUnknown** インターフェイス ポインターのアドレスを取得します。

### <a name="the-to_cx-function"></a>**to_cx** 関数

**to_cx** ヘルパー関数では、C++/WinRT オブジェクトを同等の C++/CX オブジェクトに変換します。 [**winrt::get_abi**](/uwp/cpp-ref-for-winrt/get-abi) 関数は、C++/WinRT オブジェクトの基礎となる **IUnknown** インターフェイスのポインターを取得します。 この関数は、C++/CX `safe_cast` 拡張を使用して要求された C++/CX 型を照会する前に、このポインターを C++/CX オブジェクトにキャストします。

### <a name="the-interop_helpersh-header-file"></a>`interop_helpers.h` ヘッダー ファイル

プロジェクトで 2 つのヘルパー関数を使用するには、次の手順を実行します。

- 新しい**ヘッダー ファイル (.h)** 項目をプロジェクトに追加し、`interop_helpers.h` という名前を付けます。
- `interop_helpers.h` の内容を、上記のコード リストで置き換えます。
- これらのインクルードを `pch.h` に追加します。

```cppwinrt
// pch.h
...
#include <unknwn.h>
// Include C++/WinRT projected Windows API headers here.
...
#include <interop_helpers.h>
```

## <a name="taking-a-ccx-project-and-adding-cwinrt-support"></a>C++/CX プロジェクトの取得と C++/WinRT サポートの追加

このセクションでは、既存の C++/CX プロジェクトを取得し、C++/WinRT サポートをこれに追加し、そこで移植作業を行うことを決定した場合に行うことについて説明します。 [C++/WinRT の Visual Studio のサポート](./intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)に関する記事も参照してください。

プロジェクト内で **from_cx** および **to_cx** ヘルパー関数を使用するなど、C++/CX および C++/WinRT を C++/CX プロジェクト内で混在させるには、プロジェクトに C++/WinRT サポートを手動で追加する必要があります。

最初に、Visual Studio で C++/CX プロジェクトを開き、プロジェクトのプロパティの **[全般]** \> **[ターゲット プラットフォーム バージョン]** が 10.0.17134.0 (Windows 10 バージョン 1803) 以上に設定されていることを確認します。

### <a name="install-the-cwinrt-nuget-package"></a>C++/WinRT NuGet パッケージをインストールする

[Microsoft.Windows.CppWinRT NuGet パッケージ](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/)では、C++/WinRT ビルド サポート (MSBuild プロパティ/ターゲット) が提供されています。 これをインストールするには、メニュー項目の **[プロジェクト]** \> **[NuGet パッケージの管理...]** \> **[参照]** をクリックし、検索ボックスに「**Microsoft.Windows.CppWinRT**」を入力するか貼り付けます。検索結果の項目を選択し、 **[インストール]** をクリックして、そのプロジェクトのパッケージをインストールします。

> [!IMPORTANT]
> C++/WinRT NuGet パッケージをインストールすると、プロジェクトで C++/CX のサポートが無効になります。 1 回の動作で移植する場合は、そのサポートを無効のままにしておくことで、ビルド メッセージを使用して C++/CX のすべての依存関係を検索 (および移植) できるようにする (最終的には純粋な C++/CX プロジェクトだったものを純粋な C++/WinRT プロジェクトにする) ことをお勧めします。 ただし、再度有効にする方法については、次のセクションを参照してください。

### <a name="turn-ccx-support-back-on"></a>C++/CX サポートを再び有効にする

1 回の動作で移植する場合は、これを行う必要はありません。 しかし、段階的に移植する必要がある場合は、この時点でプロジェクトで C++/CX のサポートを再び有効にする必要があります。 プロジェクトのプロパティで、 **[C/C++]** \> **[全般]** \> **[Windows ランタイム拡張機能の使用]** \> **[はい (/ZW)]** を選択します。

別の方法として (または、XAML プロジェクトの場合は、それに加えて)、Visual Studio の C++/WinRT プロジェクト プロパティ ページを使用して、C++/CX サポートを追加することもできます。 プロジェクトのプロパティで、 **[共通プロパティ]** \> **[C++ /WinRT]** \> **[Project Language]\(プロジェクト言語\)** \> **[C++/CX]** の順に選択します。 これを行うと、次のプロパティが `.vcxproj` ファイルに追加されます。

```xml
  <PropertyGroup Label="Globals">
    <CppWinRTProjectLanguage>C++/CX</CppWinRTProjectLanguage>
  </PropertyGroup>
```

> [!IMPORTANT]
> **Midl ファイル (.idl)** の内容を処理してスタブ ファイルにするようビルドする必要がある場合は常に、 **[Project Language]\(プロジェクト言語\)** を **[C++/WinRT]** に戻す必要があります。 ビルドによってスタブが生成されたら、 **[Project Language]\(プロジェクト言語\)** を **[C++/CX]** に戻します。

同様のカスタマイズ オプション (`cppwinrt.exe` ツールの動作を微調整するオプション) の一覧については、Microsoft.Windows.CppWinRT NuGet パッケージの [readme](https://github.com/microsoft/cppwinrt/blob/master/nuget/readme.md#customizing) をご覧ください。

### <a name="include-cwinrt-header-files"></a>C++/WinRT ヘッダー ファイルを含める

少なくとも、次に示すようにプリコンパイル済みヘッダー ファイル (通常は `pch.h`) に `winrt/base.h` を含める必要があります。

```cppwinrt
// pch.h
...
#include <winrt/base.h>
...
```

しかし、ほとんどの場合、**winrt::Windows::Foundation** 名前空間の型が必要になります。 また、他にも必要な名前空間があることがわかっている場合もあります。 したがって、これらの名前空間に対応する C++/WinRT の投影された Windows API ヘッダーを以下のように含めます (`winrt/base.h` は自動的に含められるため、ここで明示的に含める必要はありません)。

```cppwinrt
// pch.h
...
#include <winrt/Windows.Foundation.h>
// Include any other C++/WinRT projected Windows API headers here.
...
```

また、名前空間のエイリアス `namespace cx` および `namespace winrt` を使用する手法については、次のセクション (「*C++/WinRT プロジェクトの取得と C++/CX サポートの追加*」) のコード例を参照してください。 この手法を使用すると、そうしなかった場合に発生する可能性がある C++/WinRT プロジェクションと C++/CX プロジェクションの間の名前空間の競合に対処できます。

### <a name="add-interop_helpersh-to-the-project"></a>プロジェクトに `interop_helpers.h` を追加する

これで、**from_cx** および **to_cx** 関数を C++/CX プロジェクトに追加できるようになります。 これを行うための方法については、上記の「[**from_cx** および **to_cx** 関数](#the-from_cx-and-to_cx-functions)」セクションを参照してください。

## <a name="taking-a-cwinrt-project-and-adding-ccx-support"></a>C++/WinRT プロジェクトの取得と C++/CX サポートの追加

このセクションでは、新しい C++/WinRT プロジェクトを作成し、そこで移植作業を行うことを決定した場合に行うことについて説明します。

プロジェクト内で **from_cx** および **to_cx** ヘルパー関数を使用するなど、C++/WinRT および C++/CX を C++/WinRT プロジェクト内で混在させるには、プロジェクトに C++/CX サポートを手動で追加する必要があります。

- いずれかの C++/WinRT プロジェクト テンプレートを使用して、Visual Studio で新しい C++/WinRT プロジェクトを作成します ([C++/WinRT の Visual Studio のサポート](./intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)に関する記事を参照してください)。
- C++/CX に対するプロジェクトのサポートを有効にします。 プロジェクトのプロパティで、**[C/C++]** \> **[全般]** \> **[Windows ランタイム拡張機能の使用]** \> **[はい (/ZW)]** を選択します。

### <a name="an-example-cwinrt-project-showing-the-two-helper-functions-in-use"></a>使用中の 2 つのヘルパー関数を示す C++/WinRT プロジェクトの例

このセクションでは、**from_cx** および **to_cx** の使用方法を示すサンプル C++/WinRT プロジェクトを作成できます。 ここでは、異なる断片コードの名前空間のエイリアスを使用して、C++/WinRT プロジェクションと C++/CX プロジェクション間で生じる可能性のある他の名前空間の競合を処理する方法についても説明します。

- **[Visual C++]** \> **[Windows ユニバーサル]** \> **Core アプリ (C++/WinRT)** プロジェクトを作成します。
- プロジェクトのプロパティで、**[C/C++]** \> **[全般]** \> **[Windows ランタイム拡張機能の使用]** \> **[はい (/ZW)]** を選択します。
- プロジェクトに `interop_helpers.h` を追加します。 これを行うための方法については、上記の「[**from_cx** および **to_cx** 関数](#the-from_cx-and-to_cx-functions)」セクションを参照してください。
- `App.cpp` の内容を、以下に示すコード リストで置き換え、ビルドして実行します。

`WINRT_ASSERT` はマクロ定義であり、[_ASSERTE](/cpp/c-runtime-library/reference/assert-asserte-assert-expr-macros) に展開されます。

```cppwinrt
// App.cpp
#include "pch.h"
#include <sstream>

namespace cx
{
    using namespace Windows::Foundation;
}

namespace winrt
{
    using namespace Windows;
    using namespace Windows::ApplicationModel::Core;
    using namespace Windows::Foundation;
    using namespace Windows::Foundation::Numerics;
    using namespace Windows::UI;
    using namespace Windows::UI::Core;
    using namespace Windows::UI::Composition;
}

struct App : winrt::implements<App, winrt::IFrameworkViewSource, winrt::IFrameworkView>
{
    winrt::CompositionTarget m_target{ nullptr };
    winrt::VisualCollection m_visuals{ nullptr };
    winrt::Visual m_selected{ nullptr };
    winrt::float2 m_offset{};

    winrt::IFrameworkView CreateView()
    {
        return *this;
    }

    void Initialize(winrt::CoreApplicationView const &)
    {
    }

    void Load(winrt::hstring const&)
    {
    }

    void Uninitialize()
    {
    }

    void Run()
    {
        winrt::CoreWindow window = winrt::CoreWindow::GetForCurrentThread();
        window.Activate();

        winrt::CoreDispatcher dispatcher = window.Dispatcher();
        dispatcher.ProcessEvents(winrt::CoreProcessEventsOption::ProcessUntilQuit);
    }

    void SetWindow(winrt::CoreWindow const & window)
    {
        winrt::Compositor compositor;
        winrt::ContainerVisual root = compositor.CreateContainerVisual();
        m_target = compositor.CreateTargetForCurrentView();
        m_target.Root(root);
        m_visuals = root.Children();

        window.PointerPressed({ this, &App::OnPointerPressed });
        window.PointerMoved({ this, &App::OnPointerMoved });

        window.PointerReleased([&](auto && ...)
        {
            m_selected = nullptr;
        });
    }

    void OnPointerPressed(IInspectable const &, winrt::PointerEventArgs const & args)
    {
        winrt::float2 const point = args.CurrentPoint().Position();

        for (winrt::Visual visual : m_visuals)
        {
            winrt::float3 const offset = visual.Offset();
            winrt::float2 const size = visual.Size();

            if (point.x >= offset.x &&
                point.x < offset.x + size.x &&
                point.y >= offset.y &&
                point.y < offset.y + size.y)
            {
                m_selected = visual;
                m_offset.x = offset.x - point.x;
                m_offset.y = offset.y - point.y;
            }
        }

        if (m_selected)
        {
            m_visuals.Remove(m_selected);
            m_visuals.InsertAtTop(m_selected);
        }
        else
        {
            AddVisual(point);
        }
    }

    void OnPointerMoved(IInspectable const &, winrt::PointerEventArgs const & args)
    {
        if (m_selected)
        {
            winrt::float2 const point = args.CurrentPoint().Position();

            m_selected.Offset(
            {
                point.x + m_offset.x,
                point.y + m_offset.y,
                0.0f
            });
        }
    }

    void AddVisual(winrt::float2 const point)
    {
        winrt::Compositor compositor = m_visuals.Compositor();
        winrt::SpriteVisual visual = compositor.CreateSpriteVisual();

        static winrt::Color colors[] =
        {
            { 0xDC, 0x5B, 0x9B, 0xD5 },
            { 0xDC, 0xED, 0x7D, 0x31 },
            { 0xDC, 0x70, 0xAD, 0x47 },
            { 0xDC, 0xFF, 0xC0, 0x00 }
        };

        static unsigned last = 0;
        unsigned const next = ++last % _countof(colors);
        visual.Brush(compositor.CreateColorBrush(colors[next]));

        float const BlockSize = 100.0f;

        visual.Size(
        {
            BlockSize,
            BlockSize
        });

        visual.Offset(
        {
            point.x - BlockSize / 2.0f,
            point.y - BlockSize / 2.0f,
            0.0f,
        });

        m_visuals.InsertAtTop(visual);

        m_selected = visual;
        m_offset.x = -BlockSize / 2.0f;
        m_offset.y = -BlockSize / 2.0f;
    }
};

int __stdcall wWinMain(HINSTANCE, HINSTANCE, PWSTR, int)
{
    winrt::init_apartment();

    winrt::Uri uri(L"http://aka.ms/cppwinrt");
    std::wstringstream wstringstream;
    wstringstream << L"C++/WinRT: " << uri.Domain().c_str() << std::endl;

    // Convert from a C++/WinRT type to a C++/CX type.
    cx::Uri^ cx = to_cx<cx::Uri>(uri);
    wstringstream << L"C++/CX: " << cx->Domain->Data() << std::endl;
    ::OutputDebugString(wstringstream.str().c_str());

    // Convert from a C++/CX type to a C++/WinRT type.
    winrt::Uri uri_from_cx = from_cx<winrt::Uri>(cx);
    WINRT_ASSERT(uri.Domain() == uri_from_cx.Domain());
    WINRT_ASSERT(uri == uri_from_cx);

    winrt::CoreApplication::Run(winrt::make<App>());
}
```

## <a name="important-apis"></a>重要な API
* [IUnknown インターフェイス](/windows/win32/api/unknwn/nn-unknwn-iunknown)
* [QueryInterface 関数](/windows/win32/api/unknwn/nf-unknwn-iunknown-queryinterface(q_))
* [winrt::get_abi 関数](/uwp/cpp-ref-for-winrt/get-abi)
* [winrt::put_abi 関数](/uwp/cpp-ref-for-winrt/put-abi)

## <a name="related-topics"></a>関連トピック
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [C++/CX から C++/WinRT への移行](./move-to-winrt-from-cx.md)
* [非同期性、および C++/WinRT と C++/CX 間の相互運用](./interop-winrt-cx-async.md)