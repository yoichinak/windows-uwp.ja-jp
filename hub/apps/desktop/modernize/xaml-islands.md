---
description: このガイドは、WPF および Windows フォーム アプリケーションで直接 Fluent ベースの UWP UI を作成するのに役立ちます。
title: デスクトップアプリの UWP コントロール
ms.date: 08/20/2019
ms.topic: article
keywords: windows 10、uwp、windows フォーム、wpf、xaml islands
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: bd49417d110759dc9fec4ff4c9003e842bf1d7bb
ms.sourcegitcommit: 6bb794c6e309ba543de6583d96627fbf1c177bef
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/20/2019
ms.locfileid: "69643356"
---
# <a name="host-uwp-xaml-controls-in-desktop-apps-xaml-islands"></a>デスクトップ アプリ でUWP XAMLコントロールをホストする(XAML Islands)

Windows 10 バージョン1903以降では、*XAML Islands* と呼ばれる機能を使用して、UWP 以外のデスクトップアプリケーションでUWPコントロールをホストできます。 この機能を使用すると、既存の WPF、Windows フォーム、およびC++ Win32 アプリケーションの外観、操作性、および機能を、UWP コントロールでのみ使用できる最新の WINDOWS 10 UI 機能を使用して強化することができます。 つまり、既存の WPF、Windowsフォーム、およびC ++ Win32 アプリケーションで [Windows Ink](/windows/uwp/design/input/pen-and-stylus-interactions)などの UWP 機能と [Fluent Design System](/windows/uwp/design/fluent-design-system/index) をサポートするコントロールを使用できます。

次のような、 [Windows の UI](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)から派生した UWP コントロールをホストできます。

* Windows SDK または WinUI ライブラリによって提供される、すべてのファーストパーティ UWP コントロール。
* 任意のカスタム UWP コントロール (連携して動作する複数の UWP コントロールで構成されるユーザーコントロールなど)。 アプリケーションでコンパイルできるように、カスタムコントロールのソースコードが必要です。

基本的に、XAML アイランドは*UWP xaml ホスティング API*を使用して作成されます。 この API は、Windows 10 バージョン 1903 SDK で導入されたいくつかの Windows ランタイムクラスと COM インターフェイスで構成されています。 また、 [Windows Community Toolkit](https://docs.microsoft.com/windows/uwpcommunitytoolkit/)には、UWP XAML ホスティング API を内部的に使用する一連の xaml アイランド .net コントロールが用意されており、WPF と Windows フォームアプリの開発環境がより簡単になります。

XAML アイランドの使用方法は、アプリケーションの種類とホストする UWP コントロールの種類によって異なります。

> [!NOTE]
> XAML Islands に関するフィードバックがある場合は、 [Microsoft.Toolkit.Win32 リポジトリ](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/issues) に新しい問題を作成し、そこにコメントを残してください。 個人的にフィードバックを送信したい場合は、XamlIslandsFeedback@microsoft.com に送信できます。 あなたの洞察とシナリオは弊社にとって非常に重要です。

## <a name="wpf-and-windows-forms-applications"></a>WPF と Windows フォームアプリケーション

WPF および Windows フォームアプリケーションでは、Windows Community Toolkit で利用できる XAML アイランド .NET コントロールを使用することをお勧めします。 これらのコントロールは、対応する UWP コントロールのプロパティ、メソッド、およびイベントを模倣 (またはそのアクセスを提供) するオブジェクトモデルを提供します。 また、キーボードナビゲーションやレイアウトの変更などの動作も処理します。

WPF および Windows フォームアプリケーション用の XAML アイランドコントロールには、ラップされた*コントロール*と*ホストコントロール*の2つのセットがあります。 Windows 10 バージョン1903では、これらのコントロールは[開発者プレビューとして入手でき](#feature-roadmap)ます。

### <a name="wrapped-controls"></a>ラップされたコントロール

WPF および Windows フォームアプリケーションは、特定の UWP コントロールのインターフェイスと機能をラップする選択した XAML アイランドコントロールを使用できます。 これらのコントロールを WPF または Windows フォームプロジェクトのデザインサーフェイスに直接追加し、デザイナーの他の WPF や Windows フォームコントロールと同様に使用できます。

次のラップされた UWP コントロールは、現在 Windows Community Toolkit で使用できます。 

| コントロール | サポートされる最小 OS | 説明 |
|-----------------|-------------------------------|-------------|
| [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)<br>[InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) | Windows 10 バージョン 1903 | Windows フォームまたは WPF デスクトップアプリケーションで Windows Ink ベースのユーザーとの対話に使用する、画面と関連するツールバーを提供します。 |
| [MediaPlayerElement](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/mediaplayerelement) | Windows 10 バージョン 1903 | Windows フォームまたは WPF デスクトップアプリケーションでビデオなどのメディアコンテンツをストリーミングおよびレンダリングするビューを埋め込みます。 |
| [MapControl](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/mapcontrol) | Windows 10 バージョン 1903 | Windows フォームまたは WPF デスクトップアプリケーションで、シンボリックまたはフォトマップを表示できるようにします。 |

ラップされた UWP コントロールの使用方法を示すチュートリアルについては、「 [WPF アプリでの標準 UWP コントロールのホスト](host-standard-control-with-xaml-islands.md)」を参照してください。

### <a name="host-controls"></a>ホストコントロール

ラップされた使用可能なコントロールに含まれているもの以外のシナリオでは、WPF および Windows フォームアプリケーションで、Windows Community Toolkit で利用可能な[Windowsxamlhost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)コントロールを使用することもできます。

| コントロール | サポートされる最小 OS | 説明 |
|-----------------|-------------------------------|-------------|
| [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) | Windows 10 バージョン 1903 | は、Windows SDK によって提供されるファーストパーティ UWP コントロールやカスタムコントロールなど、 [Windows の UI](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)から派生したすべての uwp コントロールをホストできます。 |

**Windowsxamlhost**コントロールの使用方法を示すチュートリアルについては、「 [wpf アプリでの標準 Uwp コントロールのホスト](host-standard-control-with-xaml-islands.md)」および「 [XAML アイランドを使用した WPF アプリでのカスタム uwp コントロールのホスト](host-custom-control-with-xaml-islands.md)」を参照してください。

> [!NOTE]
> **Windowsxamlhost**コントロールを使用したカスタム UWP コントロールのホストは、WPF と、.net Core 3 を対象とする Windows フォームアプリでのみサポートされています。 Windows SDK によって提供されるファーストパーティ UWP コントロールのホストは、.NET Framework または .NET Core 3 を対象とするアプリでサポートされています。

<span id="requirements" />

### <a name="configure-your-project-to-use-the-xaml-island-net-controls"></a>XAML アイランド .NET コントロールを使用するようにプロジェクトを構成する

XAML アイランド .NET コントロールには、Windows 10 バージョン1903以降のバージョンが必要です。 これらのコントロールを使用するには、次に示すいずれかの NuGet パッケージをインストールします。 これらのパッケージは、XAML アイランドのラップされたコントロールとホストコントロールを使用するために必要なすべてのものを提供します。また、必要な他の関連する NuGet パッケージが含まれています。

| コントロールの種類 | NuGet パッケージ  | 関連記事 |
|-----------------|----------------|---------------------|
| [ラップされたコントロール](#wrapped-controls) | バージョン 6.0.0-preview7 以降のパッケージ: <ul><li>WPF[Microsoft. Toolkit. UI. コントロール](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls)</li><li>Windows フォーム:[Microsoft. Toolkit. UI. コントロール](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls)</li></ul>  | [WPF アプリで標準の UWP コントロールをホストする](host-standard-control-with-xaml-islands.md)  |
| [ホストコントロール](#host-controls) | バージョン 6.0.0-preview7 以降のパッケージ: <ul><li>WPF[Microsoft. Toolkit. UI. XamlHost](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.XamlHost)</li><li>Windows フォーム:[Microsoft. Toolkit. UI. XamlHost](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.XamlHost)</li></ul>  | [WPF アプリで標準の UWP コントロールをホストする](host-standard-control-with-xaml-islands.md)<br/>[WPF アプリでカスタム UWP コントロールをホストする](host-custom-control-with-xaml-islands.md)  |

次の詳細に注意してください。

* ホストコントロールパッケージは、ラップされたコントロールパッケージにも含まれています。 両方のコントロールセットを使用する場合は、ラップされたコントロールパッケージをインストールできます。

* カスタム UWP コントロールをホストしている場合は、WPF または Windows フォームプロジェクトで .NET Core 3 をターゲットにする必要があります。 カスタム UWP コントロールのホストは、.NET Framework を対象とするアプリではサポートされていません。 また、カスタムコントロールを参照するために、いくつかの追加手順を実行する必要があります。 詳細については、「 [XAML アイランドを使用した WPF アプリでのカスタム UWP コントロールのホスト](host-custom-control-with-xaml-islands.md)」を参照してください。

* 以前のバージョンの手順では、 `maxversiontested` WPF または Windows フォームプロジェクトのアプリケーションマニフェストに要素を追加しました。 上記の NuGet パッケージの最新のプレビューバージョンを使用している限り、この要素をマニフェストに追加する必要はありません。

### <a name="architecture-of-xaml-island-net-controls"></a>XAML アイランド .NET コントロールのアーキテクチャ

ここでは、さまざまな種類の XAML アイランドコントロールが UWP XAML ホスティング API の上に構造的にどのように構成されているかを簡単に説明します。

![ホスト コントロール アーキテクチャ](images/xaml-islands/host-controls.png)

この図の下部に表示されている API は、Windows SDK に付属しています。 ラップされたコントロールとホストコントロールは、Windows Community Toolkit の NuGet パッケージを通じて入手できます。

### <a name="web-view-controls"></a>Web ビューコントロール

Windows Community Toolkit には、WPF および Windows フォームアプリケーションで web コンテンツをホストするための次の .NET コントロールも用意されています。 これらのコントロールは、多くの場合、XAML アイランドコントロールと同様のデスクトップアプリの近代化シナリオで使用され、XAML アイランドコントロールと同じ[Microsoft Toolkit (Win32 リポジトリ](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32)) リポジトリに保持されます。

| コントロール | サポートされる最小 OS | 説明 |
|-----------------|-------------------------------|-------------|
| [WebView](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webview) | Windows 10 バージョン 1803 | Microsoft Edge レンダリングエンジンを使用して、web コンテンツを表示します。 |
| [WebViewCompatible](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webviewcompatible) | Windows 7 | には、より多くの OS バージョンと互換性がある**WebView**のバージョンが用意されています。 このコントロールは、Microsoft Edge レンダリングエンジンを使用して Windows 10 バージョン1803以降の web コンテンツを表示し、Internet Explorer レンダリングエンジンを使用して、以前のバージョンの Windows 10、Windows 8.x、Windows 7 に web コンテンツを表示します。 |

これらのコントロールを使用するには、次のいずれかの NuGet パッケージをインストールします。

* WPF[Microsoft. Toolkit. UI. WebView](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls.WebView)
* Windows フォーム:[Microsoft. Toolkit. UI. WebView](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls.WebView)

## <a name="c-win32-applications"></a>C++Win32 アプリケーション

XAML アイランド .NET コントロールは、Win32 アプリケーションでC++はサポートされていません。 代わりに、これらのアプリケーションでは、Windows 10 SDK (バージョン1903以降) によって提供される*UWP XAML ホスティング API*を使用する必要があります。

UWP XAML ホスティング API は、いくつかの Windows ランタイムクラスと COM インターフェイスでC++構成されています。このインターフェイスは、Win32 アプリケーションで、 [Windows の UI. .xaml. UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)から派生した UWP コントロールをホストするために使用できます。 UWP コントロールは、ウィンドウハンドル (HWND) が関連付けられているアプリケーション内の任意の UI 要素でホストできます。 この API の前提条件などの詳細については、「 [ C++ Win32 アプリでの UWP XAML ホスティング API の使用](using-the-xaml-hosting-api.md)」を参照してください。

> [!NOTE]
> Windows コミュニティツールキットのラップされたコントロールとホストコントロールは、UWP XAML ホスティング API を内部的に使用し、UWP XAML ホスティング API を直接使用した場合 (キーボードナビゲーションを含む)、独自に処理する必要のあるすべての動作を実装します。およびレイアウトが変更されます。 WPF と Windows フォーム アプリケーションでは、UWP XAML ホスティング API の代わりにこれらのコントロールを直接使用することを強くお勧めします。これらのコントロールは、API の使用に関する実装の詳細の多くを抽象化しているためです。

## <a name="feature-roadmap"></a>機能のロードマップ

Windows 10 バージョン1903のリリース時点では、Windows Community Toolkit の XAML アイランド .NET コントロールは、バージョン1.0 のコントロールが使用可能になるまで、developer preview のままです。

* .NET Framework 4.6.2 以降のコントロールのバージョン1.0 は、[ツールキットの6.0 リリース](https://github.com/windows-toolkit/WindowsCommunityToolkit/milestones)でリリースされる予定です。
* .NET Core 3 のコントロールのバージョン1.0 は、ツールキットの今後のリリースで予定されています。
* .NET Framework と .NET Core 3 について、これらのコントロールのバージョン1.0 リリースの最新のプレビューを試す場合は、 [UWP Community Toolkit](https://dotnet.myget.org/gallery/uwpcommunitytoolkit)ギャラリーにあるバージョン 6.0.0-preview7 (またはそれ以降) の NuGet パッケージを参照してください。

詳細については、[このブログの投稿](https://blogs.windows.com/windowsdeveloper/2019/06/13/xaml-islands-v1-updates-and-roadmap)を参照してください。

## <a name="additional-resources"></a>その他の資料

詳細な背景情報と XAML Islandsを使用したチュートリアルについては、次の記事やリソースを参照してください。

* [WPF アプリの最新化のチュートリアル](modernize-wpf-tutorial.md):このチュートリアルでは、Windows Community Toolkit のラップされたコントロールとホストコントロールを使用して、既存の WPF 基幹業務アプリケーションに UWP コントロールを追加する手順について説明します。 このチュートリアルでは、WPF アプリケーションの完全なコードと、プロセスの各手順の詳細な手順について説明します。
* [XAML Islands v1 - 更新プログラムとロードマップ](https://blogs.windows.com/windowsdeveloper/2019/06/13/xaml-islands-v1-updates-and-roadmap):このブログの投稿では、XAML Islands に関する多くの一般的な質問を説明し、開発の詳細なロードマップを提供します。
