---
description: このガイドは、WPF および Windows フォーム アプリケーションで直接 Fluent ベースの UWP UI を作成するのに役立ちます。
title: デスクトップアプリの UWP コントロール
ms.date: 07/26/2019
ms.topic: article
keywords: windows 10、uwp、windows フォーム、wpf、xaml islands
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 765fefa0b489e1620d7a37fe75acd02acb8d5ae8
ms.sourcegitcommit: 3cc6eb3bab78f7e68c37226c40410ebca73f82a9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/02/2019
ms.locfileid: "68729475"
---
# <a name="host-uwp-xaml-controls-in-desktop-apps-xaml-islands"></a>デスクトップ アプリ でUWP XAMLコントロールをホストする(XAML Islands)

Windows 10 バージョン1903以降では、*XAML Islands* と呼ばれる機能を使用して、UWP 以外のデスクトップアプリケーションでUWPコントロールをホストできます。 この機能を使用すると、UWPコントロールを介してのみ利用可能な最新の Windows 10 UI機能を使用して、既存のデスクトップアプリケーションの外観、操作性、および機能性を向上させることができます。 つまり、既存の WPF、Windowsフォーム、およびC ++ Win32 アプリケーションで [Windows Ink](/windows/uwp/design/input/pen-and-stylus-interactions)などの UWP 機能と [Fluent Design System](/windows/uwp/design/fluent-design-system/index) をサポートするコントロールを使用できます。

XAML Islands で、Windows フォーム、WPF を使用するいくつかの方法を紹介し、C++テクノロジまたは使用しているフレームワークに応じて、Win32 アプリケーション。 

> [!NOTE]
> XAML Islands に関するフィードバックがある場合は、 [Microsoft.Toolkit.Win32 リポジトリ](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/issues) に新しい問題を作成し、そこにコメントを残してください。 個人的にフィードバックを送信したい場合は、XamlIslandsFeedback@microsoft.com に送信できます。 あなたの洞察とシナリオは弊社にとって非常に重要です。

## <a name="how-do-xaml-islands-work"></a>XAML Islands のしくみ

Windows フォーム、WPF の XAML Islands を使用する 2 つの方法を説明する Windows 10、バージョンが 1903 年以降とC++Win32 アプリケーション。

* Windows SDK には、アプリケーションが、 [**Windows の UI**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)から派生した UWP コントロールをホストするために使用できる、いくつかの Windows ランタイムクラスおよび COM インターフェイスが用意されています。 これらのクラスとインターフェイスをまとめて*UWP XAML ホスティング API*と呼びます。これにより、ウィンドウハンドル (HWND) が関連付けられているアプリケーション内の任意の UI 要素で uwp コントロールをホストできます。 この API の詳細については、[API をホストしている XAML を使用](using-the-xaml-hosting-api.md)を参照してください。

* [Windows Community Toolkit](https://docs.microsoft.com/windows/uwpcommunitytoolkit/)には、WPF および Windows フォーム用の追加の XAML アイランドコントロールも用意されています。 これらのコントロールは、UWP XAML ホスティング API を内部的に使用し、UWP XAML ホスティング API を直接使用した場合 (キーボードナビゲーションやレイアウトの変更など)、独自に処理する必要があるすべての動作を実装します。 WPF と Windows フォームアプリケーションでは、UWP XAML ホスティング API の代わりにこれらのコントロールを直接使用することを強くお勧めします。これは、API の使用に関する多くの実装の詳細を抽象化しているためです。 Windows 10 バージョン1903以降では、これらのコントロールは[開発者プレビューとして入手でき](#feature-roadmap)ます。

> [!NOTE]
> C++Win32 デスクトップアプリケーションでは、uwp コントロールをホストするために UWP XAML ホスティング API を使用する必要があります。 Windows Community Toolkit の XAML アイランドコントロールは、 C++ Win32 デスクトップアプリケーションでは使用できません。

WPF と Windows フォームアプリケーションの Windows Community Toolkit には、ラップされた*コントロール*と*ホストコントロール*という2種類の XAML アイランドコントロールが用意されています。 

### <a name="wrapped-controls"></a>ラップされたコントロール

WPF および Windows フォームアプリケーションは、 [Windows Community Toolkit](https://docs.microsoft.com/windows/uwpcommunitytoolkit/)で、ラップされた UWP コントロールを選択できます。 これらは、特定の UWP コントロールのインターフェイスと機能をラップするため、ラップされた*コントロール*と呼ばれます。 これらのコントロールを WPF または Windows フォームプロジェクトのデザインサーフェイスに直接追加し、デザイナーで他の WPF や Windows フォームコントロールと同様に使用できます。

XAML アイランドを実装するためにラップされた次の UWP コントロールは、現在 WPF で使用できます (「[Microsoft.Toolkit.Wpf.UI.Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls) パッケージ」を参照してください)。また Windows フォーム アプリケーション (「[Microsoft.Toolkit.Forms.UI.Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls) パッケージ」を参照してください)。

| コントロール | サポートされる最小 OS | 説明 |
|-----------------|-------------------------------|-------------|
| [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)<br>[InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) | Windows 10 バージョン 1903 | Windows フォームまたは WPF デスクトップアプリケーションで Windows Ink ベースのユーザーとの対話に使用する、画面と関連するツールバーを提供します。 |
| [MediaPlayerElement](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/mediaplayerelement) | Windows 10 バージョン 1903 | Windows フォームまたは WPF デスクトップアプリケーションでビデオなどのメディアコンテンツをストリーミングおよびレンダリングするビューを埋め込みます。 |
| [MapControl](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/mapcontrol) | Windows 10 バージョン 1903 | Windows フォームまたは WPF デスクトップアプリケーションで、シンボリックまたはフォトマップを表示できるようにします。 |

Windows Community Toolkit には、XAML アイランドに対してラップされたコントロールに加えて、WPF で web コンテンツをホストするための次のコントロールも用意されて[います](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls.WebView)(「」を参照してください)。また、アプリケーションを Windows フォームします (「」を参照してください)。[UI](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls.WebView)パッケージ)。

| コントロール | サポートされる最小 OS | 説明 |
|-----------------|-------------------------------|-------------|
| [WebView](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webview) | Windows 10 バージョン 1803 | Microsoft Edge レンダリングエンジンを使用して、web コンテンツを表示します。 |
| [WebViewCompatible](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webviewcompatible) | Windows 7 | には、より多くの OS バージョンと互換性がある**WebView**のバージョンが用意されています。 このコントロールは、Microsoft Edge レンダリングエンジンを使用して Windows 10 バージョン1803以降の web コンテンツを表示し、Internet Explorer レンダリングエンジンを使用して、以前のバージョンの Windows 10、Windows 8.x、Windows 7 に web コンテンツを表示します。 |

### <a name="host-controls"></a>ホストコントロール

ラップされた使用可能なコントロールに含まれているもの以外のシナリオでは、WPF および Windows フォームアプリケーションでも[Windows Community Toolkit](https://docs.microsoft.com/windows/uwpcommunitytoolkit/)の[windowsxamlhost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)コントロールを使用できます。 このコントロールは、Windows SDK によって提供される UWP コントロールやカスタムユーザーコントロールなど、 [**Windows の UI**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)から派生した uwp コントロールをホストできます。 このコントロールは、Windows 10 Insider Preview SDK ビルド17709以降のリリースをサポートしています。

これらのコントロール[は、(](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.XamlHost) wpf 用) および[microsoft. toolkit](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.XamlHost) .............. (Windows フォーム) のパッケージで使用できます。 これらのパッケージは、ラップされたコントロールが含まれている、 [Microsoft toolkit. ui](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls) . ui. ui. ui. ui... [ui パッケージに](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls)含まれています。

### <a name="architecture-overview"></a>アーキテクチャの概要

これらのコントロールがアーキテクチャ的に編成されるしくみの概要を次に示します。

![ホスト コントロール アーキテクチャ](images/xaml-islands/host-controls.png)

この図の下部に表示されている API は、Windows SDK に付属しています。 ラップされたコントロールとホストコントロールは、Windows Community Toolkit の Nuget パッケージを通じて入手できます。

<span id="requirements" />

## <a name="configure-your-project-to-use-xaml-islands"></a>XAML アイランドを使用するようにプロジェクトを構成する

XAML Islandsには、Windows 10、バージョン1903以降が必要です。 アプリケーションで XAML アイランドを使用するには、最初にプロジェクトを設定する必要があります。

1. Windows ランタイム Api を使用するようにプロジェクトを変更します。 手順については、こちらの[記事](desktop-to-uwp-enhance.md#set-up-your-project)を参照してください。
2. これらの NuGet パッケージの1つをプロジェクトにインストールします。 バージョン 6.0.0-preview7 またはそれ以降のバージョンのパッケージがインストールされていることを確認してください。
    * WPFMicrosoft Toolkit... [UI の](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls)インストール
    * Windows フォーム:[Microsoft. Toolkit. UI. コントロール](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls)
    * C++32[Microsoft. Toolkit. UI. XamlApplication](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication)

> [!NOTE]
> 以前のバージョンの手順では、 **maxversiontested**された要素をプロジェクトのアプリケーションマニフェストに追加しました。 NuGet パッケージの最新のプレビューバージョンの時点で、マニフェストにこの要素を追加する必要がなくなりました。

## <a name="feature-roadmap"></a>機能のロードマップ

Windows 10 バージョン1903のリリース時点では、Windows Community Toolkit のラップされたコントロールとホストコントロールは、コントロールのバージョン1.0 のリリースが利用可能になるまで、developer preview のままです。

* .NET Framework 4.6.2 以降のコントロールのバージョン1.0 は、[ツールキットの6.0 リリース](https://github.com/windows-toolkit/WindowsCommunityToolkit/milestones)でリリースされる予定です。
* .NET Core 3 のコントロールのバージョン1.0 は、ツールキットの今後のリリースで予定されています。
* .NET Framework と .NET Core 3 について、これらのコントロールのバージョン1.0 リリースの最新のプレビューを試す場合は、 [UWP Community Toolkit](https://dotnet.myget.org/gallery/uwpcommunitytoolkit)ギャラリーの**6.0.0-preview7** NuGet パッケージを参照してください。

詳細については、[このブログの投稿](https://blogs.windows.com/windowsdeveloper/2019/06/13/xaml-islands-v1-updates-and-roadmap)を参照してください。

## <a name="additional-resources"></a>その他の資料

詳細な背景情報と XAML Islandsを使用したチュートリアルについては、次の記事やリソースを参照してください。

* [WPF アプリの最新化のチュートリアル](modernize-wpf-tutorial.md):このチュートリアルでは、Windows Community Toolkit のラップされたコントロールとホストコントロールを使用して、既存の WPF 基幹業務アプリケーションに UWP コントロールを追加する手順について説明します。 このチュートリアルでは、WPF アプリケーションの完全なコードと、プロセスの各手順の詳細な手順について説明します。
* [XAML Islands v1 - 更新プログラムとロードマップ](https://blogs.windows.com/windowsdeveloper/2019/06/13/xaml-islands-v1-updates-and-roadmap):このブログの投稿では、XAML Islands に関する多くの一般的な質問を説明し、開発の詳細なロードマップを提供します。
