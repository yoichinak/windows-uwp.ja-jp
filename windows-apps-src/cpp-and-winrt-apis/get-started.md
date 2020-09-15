---
description: C++/WinRT の使用をすぐに開始できるように、このトピックでは、単純なコード例について説明します。
title: C++/WinRT の使用を開始する
ms.date: 04/18/2019
ms.topic: article
keywords: windows 10, uwp, 標準, c++, cpp, winrt, プロジェクション, 取得, 取得, 開始
ms.localizationpriority: medium
ms.openlocfilehash: d7dc6455219510d75307df02571fc506b909553c
ms.sourcegitcommit: 6009896ead442b378106d82870f249dc8b55b886
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/10/2020
ms.locfileid: "89643822"
---
# <a name="get-started-with-cwinrt"></a>C++/WinRT の使用を開始する

このトピックでは、[C++/WinRT](./intro-to-using-cpp-with-winrt.md) を使用して作業を短縮できるように、新しい **Windows コンソール アプリケーション (C++/WinRT)** プロジェクトに基づいた簡単なコード例を紹介します。 また、このトピックでは、[Windows デスクトップ アプリケーション プロジェクトに C++/WinRT サポートを追加する方法](#modify-a-windows-desktop-application-project-to-add-cwinrt-support)も示します。

> [!NOTE]
> Visual Studio 2017 (Version 15.8.0 以降) を使用し、Windows SDK Version 10.0.17134.0 (Windows 10 Version 1803) をターゲットにしている場合は、最新バージョンの Visual Studio と Windows SDK を使用して開発することをお勧めします。その後は、C++/WinRT プロジェクトを新しく作成すると、エラー " *'エラー C3861: 'from_abi': 識別子が見つかりませんでした*" と、*base.h* から発生する他のエラーでコンパイルに失敗することがあります。 これを解決するには、より新しい (より準拠した) バージョンの Windows SDK をターゲットにするか、プロジェクトのプロパティを **[C/C++]**  >  **[言語]**  >  **[準拠モード]:[いいえ]** に設定します (また、プロジェクトのプロパティの **[その他のオプション]** の **[C/C++]**  >  **[言語]**  >  **[コマンド ライン]** に **/permissive-** が表示される場合は、それを削除します)。

## <a name="a-cwinrt-quick-start"></a>C++/WinRT のクイックスタート

> [!NOTE]
> &mdash;C++/WinRT Visual Studio Extension (VSIX) と NuGet パッケージ (両者が連携してプロジェクト テンプレートとビルドをサポート) のインストールと使用など、&mdash;C++/WinRT 開発用に Visual Studio を設定する方法については、[Visual Studio での C++/WinRT のサポート](./intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)に関する記事を参照してください。

新しい **Windows コンソール アプリケーション (C++/WinRT)** プロジェクトを作成します。

`pch.h` と `main.cpp` を次のように編集します。

```cppwinrt
// pch.h
#pragma once
#include <winrt/Windows.Foundation.Collections.h>
#include <winrt/Windows.Web.Syndication.h>
#include <iostream>
```

```cppwinrt
// main.cpp
#include "pch.h"

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Web::Syndication;

int main()
{
    winrt::init_apartment();

    Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
    SyndicationClient syndicationClient;
    SyndicationFeed syndicationFeed = syndicationClient.RetrieveFeedAsync(rssFeedUri).get();
    for (const SyndicationItem syndicationItem : syndicationFeed.Items())
    {
        winrt::hstring titleAsHstring = syndicationItem.Title().Text();
        std::wcout << titleAsHstring.c_str() << std::endl;
    }
}
```

上の簡単なコード例を 1 つずつ参照し、各部分に何が起こっているかを説明します。

```cppwinrt
#include <winrt/Windows.Foundation.Collections.h>
#include <winrt/Windows.Web.Syndication.h>
```

既定のプロジェクト設定では、含まれているヘッダーは Windows SDK のフォルダー `%WindowsSdkDir%Include<WindowsTargetPlatformVersion>\cppwinrt\winrt` 内にあります。 Visual Studio には、その *IncludePath* マクロにそのパスが含まれています。 ただし、プロジェクトでは (`cppwinrt.exe` ツールを介して) プロジェクトの *$(GeneratedFilesDir)* フォルダーに同じヘッダーが生成されるため、Windows SDK にはそれほど依存していません。 他の場所に見つからない場合、またはプロジェクト設定を変更した場合は、そのフォルダーから読み込まれます。

このヘッダーには、C++/WinRT に投影された Windows API が含まれます。 つまり、Windows の種類ごとに、C++/WinRT は C++ 対応の同等の型 (*投影された型*と呼ばれます) を定義します。 投影された型には Windows の型と同じ完全修飾名がありますが、C++ **winrt** 名前空間に配置されます。 これらのインクルードをプリコンパイル済みヘッダーに配置すると、段階的なビルド時間が短縮されます。

> [!IMPORTANT]
> Windows 名前空間から型を使用する場合は、上に示すように、対応する C++/WinRT Windows 名前空間ヘッダー ファイルを `#include` する必要があります。 *対応する*ヘッダーは、その型の名前空間と同じ名前を持つヘッダーです。 たとえば、C++/WinRT プロジェクションを [**Windows::Foundation::Collections::PropertySet**](/uwp/api/windows.foundation.collections.propertyset) ランタイム クラスに使用するには、`winrt/Windows.Foundation.Collections.h` ヘッダーを含めます。
> 
> C++/WinRT プロジェクション ヘッダーに、その親の名前空間のヘッダー ファイルが自動的に含まれるのはよくあることです。 たとえば `winrt/Windows.Foundation.Collections.h` には `winrt/Windows.Foundation.h` が含まれます。 とはいえ、実装の詳細は時とともに変化するので、この挙動に依存するべきではありません。 必要なヘッダーはすべて明示的に含める必要があります。

```cppwinrt
using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Web::Syndication;
```

`using namespace` ディレクティブはオプションですが、便利です。 (**winrt** 名前空間で非修飾名による参照を許可する) ディレクティブに関する上記のパターンは、新しいプロジェクトを開始する場合に最適です。また、C++/WinRT はそのプロジェクト内で使用する唯一の言語プロジェクションです)。 一方、C++/WinRT コードを [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) および SDK アプリケーション バイナリ インターフェイス (ABI) コードと混在させている (これらのモデルのいずれかまたは両方から移植しているか、相互運用している) 場合は、「[C++/WinRT と C++/CX 間の相互運用](interop-winrt-cx.md)」、「[C++/CX から C++/WinRT への移動](move-to-winrt-from-cx.md)」、「[C++/WinRT と ABI 間の相互運用](interop-winrt-abi.md)」のトピックを参照してください。

```cppwinrt
winrt::init_apartment();
```

**winrt::init_apartment** を呼び出と、Windows ランタイムのスレッドが初期化されます。既定では、マルチスレッド アパートメントです。 この呼び出しによって COM も初期化されます。

```cppwinrt
Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
SyndicationClient syndicationClient;
```

2 つのオブジェクトをスタックに割り当てます。これらのオブジェクトは Windows ブログの URI と配信クライアントを表します。 単純なワイド文字列リテラルで URI を作成します (その他の文字列の操作方法については、「[C++/WinRT での文字列の処理](strings.md)」を参照してください)。

```cppwinrt
SyndicationFeed syndicationFeed = syndicationClient.RetrieveFeedAsync(rssFeedUri).get();
```

[**SyndicationClient::RetrieveFeedAsync**](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync) は、非同期 Windows ランタイム関数の例です。 コード例では、**RetrieveFeedAsync** から非同期操作オブジェクトを受け取り、呼び出しスレッドをブロックし、その結果 (この場合は配信フィード) を待機するように、このオブジェクトで **get** を呼び出します。 同時実行、および非ブロッキングの手法の詳細については、「[C++/WinRT での同時実行と非同期操作](concurrency.md)」を参照してください。

```cppwinrt
for (const SyndicationItem syndicationItem : syndicationFeed.Items()) { ... }
```

[**SyndicationFeed.Items**](/uwp/api/windows.web.syndication.syndicationfeed.items) は、**begin** および **end** 関数 (またはそれらの定数、逆、および定数逆バリアント) から返される反復子によって定義される範囲です。 このため、範囲ベースの `for` ステートメント、または **std::for_each** テンプレート関数とともに **Items** を列挙できます。 このように Windows Runtime コレクションを反復処理するたびに、`#include <winrt/Windows.Foundation.Collections.h>` が必要になります。

```cppwinrt
winrt::hstring titleAsHstring = syndicationItem.Title().Text();
std::wcout << titleAsHstring.c_str() << std::endl;
```

フィードのタイトル テキストを、[**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) オブジェクトとして取得します (詳細については「[C++/WinRT での文字列処理](strings.md)」を参照してください)。 次に **c_str** 関数で **hstring** が出力され、C++ 標準ライブラリの文字列で使用されるパターンが反映されます。

おわかりのように、C++/WinRT では、`syndicationItem.Title().Text()` などの、最新のクラスのような C++ の式を奨励しています。 これは、従来の COM プログラミングとは異なる、よりクリーンなプログラミング スタイルです。 直接 COM を初期化する必要も、COM ポインターを操作する必要もありません。

HRESULT リターン コードを処理する必要もありません。 C++/WinRT では、エラーの HRESULT を [**winrt::hresult-error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) のような自然かつ最新のプログラミング スタイルの例外に変換します。 エラー処理の詳細とコード例の詳細については、「[C++/WinRT でのエラー処理](error-handling.md)」を参照してください。

## <a name="modify-a-windows-desktop-application-project-to-add-cwinrt-support"></a>Windows デスクトップ アプリケーション プロジェクトを変更して C++/WinRT のサポートを追加する

このセクションでは、Windows デスクトップ アプリケーション プロジェクトに C++/WinRT のサポートを追加する方法を説明します。 既存の Windows デスクトップ アプリケーション プロジェクトがない場合は、まずそれを作成すると、これらの手順を実行できます。 たとえば、Visual Studio を開き、 **[Visual C++]** \> **[Windows デスクトップ]** \> **[Windows デスクトップ アプリケーション]** プロジェクトを作成します。

必要に応じて [C++/WinRT Visual Studio 拡張機能 (VSIX)](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264) と NuGet パッケージをインストールできます。 詳細については、[C++/WinRT の Visual Studio のサポート](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)に関する記事を参照してください。

### <a name="set-project-properties"></a>プロジェクト プロパティを設定する

プロジェクトのプロパティ **[全般]** \> **[Windows SDK バージョン]** に移動し、 **[すべての構成]** と **[すべてのプラットフォーム]** を選択します。 **[Windows SDK バージョン]** が 10.0.17134.0 (Windows 10 Version 1803) 以降に設定されていることを確認します。

「[新しいプロジェクトがコンパイルされないのはなぜですか?](./faq.md)」の影響を受けていないことを確認します。

C++/WinRT には C++17 標準の機能が使用されるので、プロジェクト プロパティの **[C/C++]**  >  **[言語]**  >  **[C++ 言語標準]** を *[ISO C++17 標準 (/std:c++17)]* に設定します。

### <a name="the-precompiled-header"></a>プリコンパイル済みヘッダー

既定のプロジェクト テンプレートでは、`framework.h` または `stdafx.h` という名前のプリコンパイル済みヘッダーが自動的に作成されます。 その名前を `pch.h` に変更します。 `stdafx.cpp` ファイルがある場合は、その名前を `pch.cpp` に変更します。 プロジェクトのプロパティ **[C/C++]**  >  **[プリコンパイル済みヘッダー]**  >  **[プリコンパイル済みヘッダー]** を *[作成 (/Yc)]* に、 **[プリコンパイル済みヘッダー ファイル]** を *pch.h* に設定します。

すべての `#include "framework.h"` (または `#include "stdafx.h"`) を検索して `#include "pch.h"` に置換します。

`pch.h` に `winrt/base.h` を含めます。

```cppwinrt
// pch.h
...
#include <winrt/base.h>
```

### <a name="linking"></a>リンク

C++/WinRT 言語のプロジェクションは、特定の Windows Runtime free (非メンバー) 関数と、[WindowsApp.lib](/uwp/win32-and-com/win32-apis) アンブレラ ライブラリへのリンクを必要とするエントリ ポイントに依存します。 このセクションでは、リンカーを満たす 3 つの方法について説明します。

1 つ目の選択肢は、Visual Studio プロジェクトに C++/WinRT MSBuild のプロパティとターゲットをすべて追加することです。 そのためには、[Microsoft.Windows.CppWinRT NuGet パッケージ](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/)をプロジェクトにインストールします。 Visual Studio でプロジェクトを開き、 **[プロジェクト]** \> **[NuGet パッケージの管理]** \> **[参照]** をクリックし、検索ボックスに「**Microsoft.Windows.CppWinRT**」を入力するか貼り付けます。検索結果の項目を選択し、 **[インストール]** をクリックして、そのプロジェクトのパッケージをインストールします。

プロジェクト リンク設定を使用して明示的に `WindowsApp.lib` をリンクすることもできます。 また、このようにソース コード (たとえば `pch.h` など) で実行することもできます。

```cppwinrt
#pragma comment(lib, "windowsapp")
```

これで、C++/WinRT コードをコンパイルしてリンクし、プロジェクトに追加できるようになりました (たとえば、前述の「[C++/C++/WinRT のクイックスタート](#a-cwinrt-quick-start)」セクションと同様のコード)。

## <a name="the-three-main-scenarios-for-cwinrt"></a>C++/WinRT の 3 つの主なシナリオ

C++/WinRT の使用に慣れていき、このドキュメントの残りの部分を完了していくに従って、以下のセクションで説明する 3 つの主なシナリオがあることに気付くでしょう。

### <a name="consuming-windows-runtime-apis-and-types"></a>Windows ランタイム API と型の使用

つまり、API の "*使用*" または "*呼び出し*" です。 たとえば、Bluetooth を使用して通信する、動画をストリーミングして表示する、Windows シェルと統合することなどを目的として、API を呼び出します。 C++/WinRT では、このカテゴリのシナリオが完全かつ徹底的にサポートされています。 詳細については、「[C++/WinRT での API の使用](./consume-apis.md)」を参照してください。

### <a name="authoring-windows-runtime-apis-and-types"></a>Windows ランタイム API と型の作成

つまり、API と型の "*作成*" です。 たとえば、前のセクションで説明した種類の API、グラフィックス API、ストレージとファイル システムの API、ネットワーク API などを生成します。 詳細については、「[C++/WinRT での API の作成](./author-apis.md)」を参照してください。

C++/WinRT を使用した API の作成では、API を実装する前に IDL を使用してその形状を定義する必要があるため、使用よりも少し複雑になります。 「[XAML コントロール: C++/WinRT プロパティへのバインド](./binding-property.md)」に、それを実行するチュートリアルがあります。

### <a name="xaml-applications"></a>XAML アプリケーション

このシナリオは、XAML UI フレームワークでのアプリケーションとコントロールのビルドに関するものです。 XAML アプリケーションでの作業は、使用と作成の組み合わせです。 ただし、XAML は、現在 Windows の主要な UI フレームワークであり、Windows ランタイムに対するその影響はそれに比例しているため、独自のカテゴリのシナリオになる価値があります。

XAML は、リフレクションを提供するプログラミング言語で最適に動作することに注意してください。 C++/WinRT では、XAML フレームワークと相互運用するために、多少の追加作業を実行しなければならない場合があります。 これらのすべてのケースをこのドキュメントで説明します。 開始するのに適しているのは、「[XAML コントロール: C++/WinRT プロパティへのバインド](./binding-property.md)」と「[C++/WinRT による XAML カスタム (テンプレート化) コントロール](./xaml-cust-ctrl.md)」です。

## <a name="sample-apps-written-in-cwinrt"></a>C++/WinRT で記述されたサンプル アプリ

「[C++/WinRT サンプル アプリはどこにありますか?](/windows/uwp/cpp-and-winrt-apis/faq#where-can-i-find-cwinrt-sample-apps)」を参照してください。

## <a name="important-apis"></a>重要な API
* [SyndicationClient::RetrieveFeedAsync メソッド](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)
* [SyndicationFeed.Items プロパティ](/uwp/api/windows.web.syndication.syndicationfeed.items)
* [winrt::hstring 構造体](/uwp/cpp-ref-for-winrt/hstring)
* [winrt::hresult-error 構造体](/uwp/cpp-ref-for-winrt/error-handling/hresult-error)

## <a name="related-topics"></a>関連トピック
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [C++/WinRT でのエラー処理](error-handling.md)
* [C++/WinRT と C++/CX 間の相互運用](interop-winrt-cx.md)
* [C++/WinRT と ABI 間の相互運用](interop-winrt-abi.md)
* [C++/CX から C++/WinRT への移行](move-to-winrt-from-cx.md)
* [C++/WinRT での文字列の処理](strings.md)