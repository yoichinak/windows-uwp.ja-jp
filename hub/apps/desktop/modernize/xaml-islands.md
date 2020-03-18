---
description: このガイドは、WPF および Windows フォーム アプリケーションで直接 Fluent ベースの UWP UI を作成するのに役立ちます。
title: デスクトップ アプリの UWP コントロール
ms.date: 01/24/2020
ms.topic: article
keywords: windows 10、uwp、windows フォーム、wpf、xaml islands
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: high
ms.custom: 19H1
ms.openlocfilehash: 061ad7a3f63fc92dd2f865f8870c7de5edf862af
ms.sourcegitcommit: 756217c559155e172087dee4d762d328c6529db6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2020
ms.locfileid: "78935355"
---
# <a name="host-uwp-xaml-controls-in-desktop-apps-xaml-islands"></a>デスクトップ アプリで UWP XAML コントロールをホストする (XAML Islands)

Windows 10 バージョン 1903 以降では、"*XAML Islands*" という機能を使用して、UWP 以外のデスクトップ アプリケーションで UWP コントロールをホストできるようになりました。 この機能を使用すると、既存の WPF、Windows フォーム、およびC++ Win32 アプリケーションの外観、操作性、および機能を、UWP コントロールでのみ使用できる最新の Windows 10 UI 機能を使用して強化することができます。 つまり、既存の WPF、Windows フォーム、C++ Win32 アプリケーションで [Fluent Design System](/windows/uwp/design/fluent-design-system/index) をサポートする [Windows Ink](/windows/uwp/design/input/pen-and-stylus-interactions) やコントロールなどの UWP 機能を使用できます。

次のような [Windows.UI.Xaml.UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) から派生した任意の UWP コントロールをホストできます。

* Windows SDK または WinUI ライブラリで提供されるファーストパーティ UWP コントロール。
* 任意のカスタム UWP コントロール (たとえば、連携して動作する複数の UWP コントロールで構成されるユーザー コントロール)。 アプリケーションと共にコンパイルできるように、カスタム コントロールのソース コードを用意する必要があります。

基本的に、XAML Islands は *UWP XAML ホスティング API* を使用して作成されています。 この API は、Windows 10 バージョン 1903 SDK で導入されたいくつかの Windows ランタイム クラスと COM インターフェイスで構成されています。 また、[Windows Community Toolkit](https://docs.microsoft.com/windows/uwpcommunitytoolkit/) には、UWP XAML ホスティング API を内部的に使用する一連の XAML Island .NET コントロールも用意されており、WPF と Windows フォーム アプリの開発環境がより便利になります。

XAML Island の使用方法は、アプリケーションの種類と、ホストする UWP コントロールの種類によって異なります。

> [!NOTE]
> XAML Islands に関するフィードバックがある場合は、[Microsoft.Toolkit.Win32 リポジトリ](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/issues)に新しい問題を作成し、そこにコメントを残してください。 個人的にフィードバックを送信したい場合は、XamlIslandsFeedback@microsoft.com に送信できます。 お客様の洞察とシナリオは弊社にとって非常に重要です。

## <a name="wpf-and-windows-forms-applications"></a>WPF と Windows フォーム アプリケーション

WPF および Windows フォーム アプリケーションでは、Windows Community Toolkit で利用できる XAML Island .NET コントロールを使用することをお勧めします。 これらのコントロールには、対応する UWP コントロールのプロパティ、メソッド、およびイベントを模倣する (またはそれにアクセスできるようになる) オブジェクト モデルが用意されています。 また、キーボード ナビゲーションやレイアウトの変更などの動作も処理されます。

WPF および Windows フォーム アプリケーション用の XAML Island コントロールには、"*ラップされたコントロール*" と"*ホスト コントロール*" という 2 つのセットがあります。 

### <a name="wrapped-controls"></a>ラップされたコントロール

WPF および Windows フォーム アプリケーションには、特定の UWP コントロールのインターフェイスと機能をラップする XAML Island コントロールの一部を使用できます。 これらのコントロールを WPF または Windows フォーム プロジェクトのデザイン サーフェイスに直接追加し、デザイナーの他の WPF や Windows フォーム コントロールと同様に使用できます。

現在、次のラップされた UWP コントロールを Windows Community Toolkit で使用できます。 

| Control | サポートされる最小 OS | 説明 |
|-----------------|-------------------------------|-------------|
| [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)<br>[InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) | Windows 10 バージョン 1903 | Windows フォームまたは WPF デスクトップ アプリケーションで Windows Ink ベースのユーザーとの対話に使用する、画面と関連するツールバーを提供します。 |
| [MediaPlayerElement](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/mediaplayerelement) | Windows 10 バージョン 1903 | Windows フォームまたは WPF デスクトップ アプリケーションでビデオなどのメディア コンテンツをストリーミングおよびレンダリングするビューを埋め込みます。 |
| [MapControl](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/mapcontrol) | Windows 10 バージョン 1903 | Windows フォームまたは WPF デスクトップ アプリケーションで、シンボリックまたはフォト マップを表示できるようにします。 |

ラップされた UWP コントロールの使用方法を示すチュートリアルについては、[WPF アプリでの標準 UWP コントロールのホスト](host-standard-control-with-xaml-islands.md)に関する記事を参照してください。

### <a name="host-controls"></a>ホスト コントロール

使用できるラップされたコントロールに含まれていないカスタム コントロールやその他のシナリオでは、Windows Community Toolkit で使用できる [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) コントロールを WPF および Windows フォーム アプリケーションに使用することもできます。

| Control | サポートされる最小 OS | 説明 |
|-----------------|-------------------------------|-------------|
| [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) | Windows 10 バージョン 1903 | [Windows.UI.Xaml.UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) から派生した UWP コントロールをホストできます。たとえば、Windows SDK で提供されるファーストパーティ UWP コントロールや、カスタム コントロールなどです。 |

**WindowsXamlHost** コントロールの使用方法を示すチュートリアルについては、[WPF アプリでの標準 UWP コントロールのホスト](host-standard-control-with-xaml-islands.md)に関する記事と「[XAML アイランドを使用した WPF アプリでのカスタム UWP コントロールのホスト](host-custom-control-with-xaml-islands.md)」を参照してください。

> [!NOTE]
> **WindowsXamlHost** コントロールを使用したカスタム UWP コントロールのホストは、.NET Core 3 をターゲットとする WPF および Windows フォーム アプリでのみサポートされています。 Windows SDK で提供されるファーストパーティ UWP コントロールのホストは、.NET Framework または .NET Core 3 をターゲットとするアプリでサポートされています。

<span id="requirements" />

### <a name="configure-your-project-to-use-the-xaml-island-net-controls"></a>XAML Island .NET コントロールを使用するようにプロジェクトを構成する

XAML Island .NET コントロールには、Windows 10 バージョン 1903 以降が必要です。 これらのコントロールを使用するには、次に示す NuGet パッケージのいずれかをインストールします。 これらのパッケージには、XAML Island のラップされたコントロールとホスト コントロールを使用するために必要なすべてのものが用意され、必要な他の関連する NuGet パッケージも含まれています。

| コントロールの種類 | NuGet パッケージ  | 関連記事 |
|-----------------|----------------|---------------------|
| [ラップされたコントロール](#wrapped-controls) | バージョン 6.0.0 以降のパッケージ: <ul><li>WPF:[Microsoft.Toolkit.Wpf.UI.Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls)</li><li>Windows フォーム:[Microsoft.Toolkit.Forms.UI.Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls)</li></ul>  | [WPF アプリでの標準 UWP コントロールのホスト](host-standard-control-with-xaml-islands.md)  |
| [ホスト コントロール](#host-controls) | バージョン 6.0.0 以降のパッケージ: <ul><li>WPF:[Microsoft.Toolkit.Wpf.UI.XamlHost](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.XamlHost)</li><li>Windows フォーム:[Microsoft.Toolkit.Forms.UI.XamlHost](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.XamlHost)</li></ul>  | [WPF アプリでの標準 UWP コントロールのホスト](host-standard-control-with-xaml-islands.md)<br/>[WPF アプリでのカスタム UWP コントロールのホスト](host-custom-control-with-xaml-islands.md)  |

次の詳細に注意してください。

* ホスト コントロール パッケージは、ラップされたコントロール パッケージにも含まれています。 ラップされたコントロール パッケージをインストールすると、両方のコントロール セットを使用できます。

* カスタム UWP コントロールをホストしている場合、WPF または Windows フォーム プロジェクトは .NET Core 3 をターゲットにする必要があります。 .NET Framework をターゲットとするアプリでは、カスタム UWP コントロールのホストはサポートされません。 また、カスタム コントロールを参照するには、いくつかの追加の手順を実行する必要があります。 詳細については、「[XAML アイランドを使用した WPF アプリでのカスタム UWP コントロールのホスト](host-custom-control-with-xaml-islands.md)」を参照してください。

### <a name="web-view-controls"></a>Web ビュー コントロール

Windows Community Toolkit には、WPF および Windows フォーム アプリケーションで Web コンテンツをホストするための次の .NET コントロールも用意されています。 これらのコントロールは、XAML Island コントロールと同様のデスクトップ アプリの最新化シナリオでよく使用され、XAML Island コントロールと同じ [Microsoft.Toolkit.Win32 リポジトリ](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32) リポジトリで保守されています。

| Control | サポートされる最小 OS | 説明 |
|-----------------|-------------------------------|-------------|
| [WebView](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webview) | Windows 10 バージョン 1803 | Microsoft Edge レンダリング エンジンを使用して、Web コンテンツを表示します。 |
| [WebViewCompatible](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webviewcompatible) | Windows 7 | より多くの OS バージョンと互換性がある **WebView** のバージョンが用意されています。 このコントロールには、Windows 10 バージョン 1803 以降で Web コンテンツを表示するために Microsoft Edge レンダリング エンジンが使用され、Windows 10、Windows 8.x、および Windows 7 の以前のバージョンで Web コンテンツを表示するために Internet Explorer レンダリング エンジンが使用されます。 |

これらのコントロールを使用するには、次のいずれかの NuGet パッケージをインストールします。

* WPF:[Microsoft.Toolkit.Wpf.UI.Controls.WebView](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls.WebView)
* Windows フォーム:[Microsoft.Toolkit.Forms.UI.Controls.WebView](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls.WebView)

## <a name="c-win32-applications"></a>C++ Win32 アプリケーション

XAML Island .NET コントロールは、C++ Win32 アプリケーションではサポートされていません。 これらのアプリケーションでは、Windows 10 SDK (バージョン 1903 以降) で提供される *UWP XAML ホスティング API* を代わりに使用する必要があります。

UWP XAML ホスティング API は、いくつかの Windows ランタイム クラスと COM インターフェイスで構成されています。C++ Win32 アプリケーションではこれらを使用して [Windows.UI.Xaml.UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) から派生した任意の UWP コントロールをホストできます。 関連するウィンドウ ハンドル (HWND) を持つアプリケーションの任意の UI 要素で UWP コントロールをホストできます。 前提条件など、この API の詳細については、「[UWP XAML を使用した C++ Win32 アプリでの API のホスト](using-the-xaml-hosting-api.md)」を参照してください。

> [!NOTE]
> Windows Community Toolkit のラップされたコントロールとホスト コントロールには、UWP XAML ホスティング API が内部的に使用され、キーボード ナビゲーションやレイアウトの変更など、UWP XAML ホスティング API を直接使用した場合には独自に処理する必要があるすべての動作が実装されています。 WPF および Windows フォーム アプリケーションの場合、UWP XAML ホスティング API ではなく、これらのコントロールを使用することを強くお勧めします。これは、API に関する実装の詳細の多くを抽象化できるためです。

## <a name="architecture-of-xaml-islands"></a>XAML Islands のアーキテクチャ

ここでは、UWP XAML ホスティング API の上に、どのような種類の XAML Island コントロールが構造的に構成されているかを簡単に説明します。

![ホスト コントロール アーキテクチャ](images/xaml-islands/host-controls.png)

この図の下部に表示されている API は、Windows SDK に付属しています。 ラップされたコントロールとホスト コントロールは、Windows Community Toolkit の NuGet パッケージを介して使用できます。

## <a name="window-host-context-for-xaml-islands"></a>XAML Islands のウィンドウ ホスト コンテキスト

デスクトップ アプリで XAML Islands をホストすると、XAML コンテンツの複数のツリーを同じスレッド上で同時に実行できます。 XAML Island で XAML コンテンツのツリーのルート要素にアクセスし、それがホストされているコンテキストに関する関連情報を取得するには、[XamlRoot](https://docs.microsoft.com/uwp/api/windows.ui.xaml.xamlroot) クラスを使用します。 [CoreWindow](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow)、[ApplicationView](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationview)、および [Window](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window) クラスでは、XAML Islands に関する正しい情報が提供されません。 [CoreWindow](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow) および [Window](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window) オブジェクトはスレッドに存在し、アプリからアクセスできますが、意味のある境界と表示は返されません (常に非表示で、サイズは 1x1 です)。 詳細については、[ウィンドウ化ホスト](/windows/uwp/design/layout/show-multiple-views#windowing-hosts)に関するページを参照してください。

たとえば、XAML Island でホストされている UWP コントロールが含まれるウィンドウの境界を示す四角形を取得するには、コントロールの [XamlRoot.Size](https://docs.microsoft.com/uwp/api/windows.ui.xaml.xamlroot.size) プロパティを使用します。 XAML Island でホストできるすべての UWP コントロールは、[Windows.UI.Xaml.UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) から派生するため、コントロールの [XamlRoot](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.xamlroot) プロパティを使用して、**XamlRoot** オブジェクトにアクセスできます。

```csharp
Size windowSize = myUWPControl.XamlRoot.Size;
```

境界を示す四角形を取得するために [CoreWindows.Bounds](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.bounds) プロパティを使用しないでください。

```csharp
// This will return incorrect information for a UWP control that is hosted in a XAML Island.
Rect windowSize = CoreWindow.GetForCurrentThread().Bounds;
```

XAML Islands のコンテキストで避ける必要がある一般的なウィンドウ関連 API の表と、推奨される [XamlRoot](https://docs.microsoft.com/uwp/api/windows.ui.xaml.xamlroot) の置換については、[こちらのセクション](/windows/uwp/design/layout/show-multiple-views#make-code-portable-across-windowing-hosts)の表を参照してください。

## <a name="feature-roadmap"></a>機能ロードマップ

ここでは、Windows 10、バージョン 1903 および Windows Community Toolkit バージョン 6.0 のリリース時点での XAML Islands 関連機能の現在の状態について説明します。

* **C++ Win32 アプリ:** Windows 10 バージョン 1903 時点の UWP XAML ホスティング API はバージョン 1.0 と見なされます。
* **.NET Framework 4.6.2 以降をターゲットとするマネージド アプリ:** .NET Framework 4.6.2 以降をターゲットとするアプリ用の[バージョン 6.0.0 NuGet パッケージ](#configure-your-project-to-use-the-xaml-island-net-controls)で使用できる XAML Island コントロールはバージョン 1.0 と見なされます。
* **.NET Core 3.0 以降をターゲットとするマネージド アプリ:** .NET Core 3.0 以降をターゲットとするアプリ用の[バージョン 6.0.0 NuGet パッケージで使用できるコントロール](#configure-your-project-to-use-the-xaml-island-net-controls)は、まだ開発者プレビュー段階です。 .NET Core 3.0 以降のこれらのコントロールのバージョン 1.0 リリースは、今後のリリースで予定されています。

## <a name="additional-resources"></a>その他の資料

XAML Islands の使用に関する背景情報とチュートリアルの詳細については、次の記事とリソースを参照してください。

* [WPF アプリの最新化のチュートリアル:](modernize-wpf-tutorial.md)このチュートリアルでは、Windows Community Toolkit でラップされたコントロールとホスト コントロールを使用して、UWP コントロールを既存の WPF 基幹業務アプリケーションに追加する手順が説明されています。 このチュートリアルには、WPF アプリケーションの完全なコードと、プロセスの各手順の詳細な手順も含まれています。
* [XAML Islands コード サンプル](https://github.com/microsoft/Xaml-Islands-Samples):このリポジトリには、XAML Islands の使用方法を示す Windows フォーム、WPF、C++/Win32 のサンプルが含まれています。
* [XAML Islandss v1 - 更新プログラムとロードマップ](https://blogs.windows.com/windowsdeveloper/2019/06/13/xaml-islands-v1-updates-and-roadmap):このブログ投稿では、XAML Islands についてよく寄せられる質問が説明され、詳細な開発ロードマップが掲載されています。
