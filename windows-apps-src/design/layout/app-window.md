---
Description: アプリケーションのさまざまな部分を別のウィンドウで表示するには、AppWindow クラスを使用します。
title: AppWindow クラスを使用して、アプリのセカンダリウィンドウを表示する
ms.date: 07/19/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f9b247a4a8eb69fa390a9e250e0f88deff9311bf
ms.sourcegitcommit: 3cc6eb3bab78f7e68c37226c40410ebca73f82a9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/02/2019
ms.locfileid: "68730520"
---
# <a name="show-multiple-views-with-appwindow"></a>AppWindow での複数のビューの表示

[Appwindow](/uwp/api/windows.ui.windowmanagement.appwindow)とそれに関連する api は、各ウィンドウで同じ UI スレッドを操作しながら、セカンダリウィンドウにアプリのコンテンツを表示できるようにすることで、マルチウィンドウアプリの作成を簡略化します。

> [!NOTE]
> AppWindow は現在プレビューの段階です。 これは、AppWindow を使用するアプリをストアに送信できることを意味しますが、一部のプラットフォームおよびフレームワークコンポーネントは AppWindow で動作しないことがわかっています (「[制限事項](/uwp/api/windows.ui.windowmanagement.appwindow#limitations)」を参照してください)。

ここでは、という`HelloAppWindow`サンプルアプリを使用して、複数のウィンドウについていくつかのシナリオを紹介します。 サンプルアプリでは、次の機能を示しています。

- メインページからコントロールをドッキング解除し、新しいウィンドウで開きます。
- 新しいウィンドウでページの新しいインスタンスを開きます。
- プログラムによってアプリ内の新しいウィンドウのサイズを変更します。
- ContentDialog をアプリの適切なウィンドウに関連付けます。

![1つのウィンドウを持つサンプルアプリ](images/hello-app-window-single.png)
  
> _1つのウィンドウを持つサンプルアプリ_

![ドッキングが解除されたカラーピッカーとセカンダリウィンドウを使用したサンプルアプリ](images/hello-app-window-multi.png)

> _ドッキングが解除されたカラーピッカーとセカンダリウィンドウを使用したサンプルアプリ_

> **重要な API**:[Windows. UI. WindowManagement 名前空間](/uwp/api/windows.ui.windowmanagement)、 [appwindow クラス](/uwp/api/windows.ui.windowmanagement.appwindow)

## <a name="api-overview"></a>API の概要

[Windowmanagement](/uwp/api/windows.ui.windowmanagement)名前空間の[appwindow](/uwp/api/windows.ui.windowmanagement.appwindow)クラスおよびその他の api は、Windows 10 バージョン1903以降で使用できます (SDK 18362)。 アプリが以前のバージョンの Windows 10 を対象としている場合は、 [ApplicationView を使用してセカンダリウィンドウを作成](application-view.md)する必要があります。 WindowManagement Api はまだ開発中であり、API リファレンスドキュメントに記載されている[制限](/uwp/api/windows.ui.windowmanagement.appwindow#limitations)があります。

ここでは、AppWindow でコンテンツを表示するために使用する重要な Api の一部を示します。

### <a name="appwindow"></a>AppWindow

[Appwindow](/uwp/api/windows.ui.windowmanagement.appwindow)クラスを使用すると、Windows ランタイムアプリの一部をセカンダリウィンドウに表示できます。 概念は[Applicationview](/uwp/api/windows.ui.viewmanagement.applicationview)に似ていますが、動作と有効期間は同じではありません。 AppWindow の主な機能は、各インスタンスが、作成元の UI 処理スレッド (イベントディスパッチャーを含む) を共有することです。これにより、マルチウィンドウアプリが簡略化されます。

XAML コンテンツは AppWindow にのみ接続できます。ネイティブ DirectX または Holographic コンテンツはサポートされていません。 ただし、DirectX コンテンツをホストする XAML [SwapChainPanel](/uwp/api/windows.ui.xaml.controls.swapchainpanel)を表示することができます。

### <a name="windowingenvironment"></a>WindowingEnvironment

[WindowingEnvironment](/uwp/api/windows.ui.windowmanagement.windowingenvironment) API を使用すると、必要に応じてアプリを調整できるように、アプリが表示されている環境について知ることができます。 環境でサポートされているウィンドウの種類を説明します。たとえば、アプリ`Overlapped`が PC で実行されている場合や`Tiled` 、アプリが Xbox 上で実行されている場合などです。 また、アプリケーションが論理ディスプレイに表示される領域を記述する DisplayRegion オブジェクトのセットも提供します。

### <a name="displayregion"></a>DisplayRegion

[Displayregion](/uwp/api/windows.ui.windowmanagement.displayregion) API は、論理ディスプレイでユーザーにビューを表示できる領域について説明します。たとえば、デスクトップ PC では、完全な表示はタスクバーの領域を引いたものになります。 これは、必ずしもバッキングモニターの物理的な表示領域との1:1 マッピングではありません。 同じモニター内に複数のディスプレイ領域が存在する場合や、表示領域がすべての側面で同一である場合は複数のモニターにまたがるように構成できます。

### <a name="appwindowpresenter"></a>AppWindowPresenter

[Appwindowpresenter](/uwp/api/windows.ui.windowmanagement.appwindowpresenter) API を使用すると、や`FullScreen` `CompactOverlay`などの事前定義された構成に windows を簡単に切り替えることができます。 これらの構成により、ユーザーは、構成をサポートする任意のデバイスで一貫したエクスペリエンスを提供できます。

### <a name="uicontext"></a>UIContext

[Uicontext](/uwp/api/windows.ui.uicontext)は、アプリのウィンドウまたはビューの一意の識別子です。 これは自動的に作成され、 [UIElement. uicontext](/uwp/api/windows.ui.xaml.uielement.uicontext)プロパティを使用して uicontext を取得できます。 XAML ツリー内のすべての UIElement の UIContext は同じです。

 Uicontext が重要なのは、 [Window. Current](/uwp/api/Windows.UI.Xaml.Window.Current)と`GetForCurrentView`パターンなどの api は、操作対象のスレッドごとに1つの XAML ツリーを持つ単一の applicationview/corewindow を持つことに依存しているためです。 これは、AppWindow を使用する場合ではなく、UIContext を使用して特定のウィンドウを識別するためです。

### <a name="xamlroot"></a>XamlRoot

[Xamlroot](/uwp/api/windows.ui.xaml.xamlroot)クラスは、XAML 要素ツリーを保持し、ウィンドウホストオブジェクト (たとえば、 [Appwindow](/uwp/api/windows.ui.windowmanagement.appwindow)や[applicationview](/uwp/api/windows.ui.viewmanagement.applicationview)) に接続し、サイズや可視性などの情報を提供します。 XamlRoot オブジェクトを直接作成することはありません。 代わりに、XAML 要素を AppWindow にアタッチするときに1つのが作成されます。 その後、UIElement の[ルート](/uwp/api/windows.ui.xaml.uielement.xamlroot)プロパティを使用して、xamlroot を取得できます。

UIContext と XamlRoot の詳細については、「[ウィンドウホスト間でコードの移植性を確保](show-multiple-views.md#make-code-portable-across-windowing-hosts)する」を参照してください。

## <a name="show-a-new-window"></a>新しいウィンドウを表示する

新しい AppWindow でコンテンツを表示する手順を見てみましょう。

**新しいウィンドウを表示するには**

1. 静的な[appwindow. TryCreateAsync](/uwp/api/windows.ui.windowmanagement.appwindow.trycreateasync)メソッドを呼び出して、新しい[appwindow](/uwp/api/windows.ui.windowmanagement.appwindow)を作成します。

    ```csharp
    AppWindow appWindow = await AppWindow.TryCreateAsync();
    ```

1. ウィンドウコンテンツを作成します。

    通常は、XAML[フレーム](/uwp/api/Windows.UI.Xaml.Controls.Frame)を作成してから、アプリのコンテンツを定義した xaml[ページ](/uwp/api/Windows.UI.Xaml.Controls.Page)にフレームを移動します。 フレームとページの詳細については、「 [2 つのページ間のピアツーピアナビゲーション](../basics/navigate-between-two-pages.md)」を参照してください。

    ```csharp
    Frame appWindowContentFrame = new Frame();
    appWindowContentFrame.Navigate(typeof(AppWindowMainPage));
    ```

    ただし、すべての XAML コンテンツは、フレームやページだけでなく、AppWindow に表示することもできます。 たとえば、 [Colorpicker](/uwp/api/windows.ui.xaml.controls.colorpicker)などの1つのコントロールだけを表示することも、DirectX コンテンツをホストする[SwapChainPanel](/uwp/api/windows.ui.xaml.controls.swapchainpanel)を表示することもできます。

1. [ElementCompositionPreview](/api/windows.ui.xaml.hosting.elementcompositionpreview.setappwindowcontent)メソッドを呼び出して、XAML コンテンツを appwindow にアタッチします。

    ```csharp
    ElementCompositionPreview.SetAppWindowContent(appWindow, appWindowContentFrame);
    ```

    このメソッドを呼び出すと、 [xamlroot](/uwp/api/windows.ui.xaml.xamlroot)オブジェクトが作成され、指定した UIElement の[xamlroot](/uwp/api/windows.ui.xaml.uielement.xamlroot)プロパティとして設定されます。

    このメソッドは、AppWindow インスタンスごとに1回だけ呼び出すことができます。 コンテンツが設定されると、この AppWindow インスタンスの SetAppWindowContent への呼び出しは失敗します。 また、null の UIElement オブジェクトを渡して AppWindow コンテンツを切断しようとすると、呼び出しは失敗します。

1. 新しいウィンドウを表示するには、 [Appwindow. TryShowAsync](/uwp/api/windows.ui.windowmanagement.appwindow.tryshowasync)メソッドを呼び出します。

    ```csharp
    await appWindow.TryShowAsync();
    ```

## <a name="release-resources-when-a-window-is-closed"></a>ウィンドウが閉じられたときにリソースを解放する

XAML リソース (AppWindow コンテンツ) と AppWindow への参照を解放するには、常に[appwindow. Closed](/uwp/api/windows.ui.windowmanagement.appwindow.closed)イベントを処理する必要があります。

```csharp
appWindow.Closed += delegate
{
    appWindowContentFrame.Content = null;
    appWindow = null;
};
```

## <a name="track-instances-of-appwindow"></a>AppWindow のインスタンスの追跡

アプリでの複数のウィンドウの使用方法によっては、作成した AppWindow のインスタンスを追跡する必要がある場合と、ない場合があります。 この`HelloAppWindow`例は、通常、 [appwindow](/uwp/api/windows.ui.windowmanagement.appwindow)を使用するさまざまな方法を示しています。 ここでは、これらのウィンドウが追跡される理由とその方法について説明します。

### <a name="simple-tracking"></a>単純な追跡

[カラーピッカー] ウィンドウは1つの XAML コントロールをホストし、カラーピッカーと対話するためのコード`MainPage.xaml.cs`はすべてファイルに存在します。 [カラーピッカー] ウィンドウでは、1つのインスタンスのみが許可`MainWindow`され、基本的にはの拡張機能です。 1つのインスタンスのみが作成されるようにするために、[カラーピッカー] ウィンドウはページレベル変数で追跡されます。 新しいカラーピッカーウィンドウを作成する前に、インスタンスが存在するかどうかを確認し、存在する場合は、新しいウィンドウを作成する手順をスキップし、既存のウィンドウで[Tryshowasync](/uwp/api/windows.ui.windowmanagement.appwindow.tryshowasync)を呼び出します。

```csharp
AppWindow colorPickerAppWindow;

// ...

private async void DetachColorPickerButton_Click(object sender, RoutedEventArgs e)
{
    // Create the color picker window.
    if (colorPickerAppWindow == null)
    {
        // ...
        // Create a new window
        colorPickerAppWindow = await AppWindow.TryCreateAsync();
        // ...
    }
    // Show the window.
    await colorPickerAppWindow.TryShowAsync();
}
```

### <a name="track-an-appwindow-instance-in-its-hosted-content"></a>ホストされたコンテンツでの AppWindow インスタンスの追跡

この`AppWindowPage`ウィンドウは、完全な XAML ページをホストし、ページと対話するための`AppWindowPage.xaml.cs`コードはに存在します。 複数のインスタンスを許可し、それぞれが独立して機能します。

このページの機能を使用すると、ウィンドウを操作したり`FullScreen` 、 `CompactOverlay`またはに設定したり、 [appwindow](/uwp/api/windows.ui.windowmanagement.appwindow.changed)をリッスンして、ウィンドウに関する情報を表示したりすることができます。 これらの api を呼び出すため`AppWindowPage`に、は、それをホストしている appwindow インスタンスへの参照を必要とします。

これだけで必要な場合は、で`AppWindowPage`プロパティを作成し、作成時に[appwindow](/uwp/api/windows.ui.windowmanagement.appwindow)インスタンスを割り当てることができます。

**AppWindowPage.xaml.cs**

で`AppWindowPage`、appwindow 参照を保持するプロパティを作成します。

```csharp
public sealed partial class AppWindowPage : Page
{
    public AppWindow MyAppWindow { get; set; }

    // ...
}
```

**MainPage.xaml.cs**

で`MainPage`、ページインスタンスへの参照を取得し、新しく作成された appwindow をの`AppWindowPage`プロパティに割り当てます。

```csharp
private async void ShowNewWindowButton_Click(object sender, RoutedEventArgs e)
{
    // Create a new window.
    AppWindow appWindow = await AppWindow.TryCreateAsync();

    // Create a Frame and navigate to the Page you want to show in the new window.
    Frame appWindowContentFrame = new Frame();
    appWindowContentFrame.Navigate(typeof(AppWindowPage));

    // Get a reference to the page instance and assign the
    // newly created AppWindow to the MyAppWindow property.
    AppWindowPage page = (AppWindowPage)appWindowContentFrame.Content;
    page.MyAppWindow = appWindow;

    // ...
}
```

### <a name="tracking-app-windows-using-uicontext"></a>UIContext を使用してアプリウィンドウを追跡する

また、アプリの他の部分から[Appwindow](/uwp/api/windows.ui.windowmanagement.appwindow)インスタンスにアクセスできるようにすることもできます。 たとえば、[ `MainPage`すべて閉じる] ボタンを使用して、appwindow のすべての追跡対象インスタンスを閉じることができます。

この場合は、 [Uicontext](/uwp/api/windows.ui.uicontext)一意識別子を使用して、[ディクショナリ](/dotnet/api/system.collections.generic.dictionary-2?view=dotnet-uwp-10.0)内のウィンドウインスタンスを追跡する必要があります。

**MainPage.xaml.cs**

で`MainPage`、静的なプロパティとしてディクショナリを作成します。 次に、作成時にこのページをディクショナリに追加し、ページを閉じたときに削除します。 [ElementCompositionPreview](/api/windows.ui.xaml.hosting.elementcompositionpreview.setappwindowcontent)を呼び出した後、コンテンツ[フレーム](/uwp/api/Windows.UI.Xaml.Controls.Frame)(`appWindowContentFrame.UIContext`) から uicontext を取得できます。

```csharp
public sealed partial class MainPage : Page
{
    // Track open app windows in a Dictionary.
    public static Dictionary<UIContext, AppWindow> AppWindows { get; set; }
        = new Dictionary<UIContext, AppWindow>();

    // ...

    private async void ShowNewWindowButton_Click(object sender, RoutedEventArgs e)
    {
        // Create a new window.
        AppWindow appWindow = await AppWindow.TryCreateAsync();

        // Create a Frame and navigate to the Page you want to show in the new window.
        Frame appWindowContentFrame = new Frame();
        appWindowContentFrame.Navigate(typeof(AppWindowPage));

        // Attach the XAML content to the window.
        ElementCompositionPreview.SetAppWindowContent(appWindow, appWindowContentFrame);

        // Add the new page to the Dictionary using the UIContext as the Key.
        AppWindows.Add(appWindowContentFrame.UIContext, appWindow);
        appWindow.Title = "App Window " + AppWindows.Count.ToString();

        // When the window is closed, be sure to release
        // XAML resources and the reference to the window.
        appWindow.Closed += delegate
        {
            MainPage.AppWindows.Remove(appWindowContentFrame.UIContext);
            appWindowContentFrame.Content = null;
            appWindow = null;
        };

        // Show the window.
        await appWindow.TryShowAsync();
    }

    private async void CloseAllButton_Click(object sender, RoutedEventArgs e)
    {
        while (AppWindows.Count > 0)
        {
            await AppWindows.Values.First().CloseAsync();
        }
    }
    // ...
}
```

**AppWindowPage.xaml.cs**

`AppWindowPage`コードで[appwindow](/uwp/api/windows.ui.windowmanagement.appwindow)インスタンスを使用するには、ページの[uicontext](/uwp/api/windows.ui.uicontext)を使用して、の`MainPage`静的ディクショナリから取得します。 これは、UIContext が null でないように、コンストラクターではなく、ページの[読み込ま](/uwp/api/windows.ui.xaml.frameworkelement.loaded)れたイベントハンドラーで実行する必要があります。 UIContext は、ページ`this.UIContext`から取得できます。

```csharp
public sealed partial class AppWindowPage : Page
{
    AppWindow window;

    // ...
    public AppWindowPage()
    {
        this.InitializeComponent();

        Loaded += AppWindowPage_Loaded;
    }

    private void AppWindowPage_Loaded(object sender, RoutedEventArgs e)
    {
        // Get the reference to this AppWindow that was stored when it was created.
        window = MainPage.AppWindows[this.UIContext];

        // Set up event handlers for the window.
        window.Changed += Window_Changed;
    }
    // ...
}
```

> [!NOTE]
> この`HelloAppWindow`例では、の`AppWindowPage`ウィンドウを追跡する両方の方法を示していますが、通常はどちらか一方を使用します。

## <a name="request-window-size-and-placement"></a>要求ウィンドウのサイズと配置

[Appwindow](/uwp/api/windows.ui.windowmanagement.appwindow)クラスには、ウィンドウのサイズと配置を制御するために使用できるいくつかのメソッドがあります。 メソッド名によって示されるように、システムは環境要因に応じて、要求された変更を無視することがあります。

[Requestsize](/uwp/api/windows.ui.windowmanagement.appwindow.requestsize)を呼び出して、次のように、目的のウィンドウサイズを指定します。

```csharp
colorPickerAppWindow.RequestSize(new Size(300, 428));
```

ウィンドウの配置を管理するためのメソッドの名前は_Requestmove *_ :[RequestMoveAdjacentToCurrentView](/uwp/api/windows.ui.windowmanagement.appwindow.requestmoveadjacenttocurrentview)、 [RequestMoveAdjacentToWindow](/uwp/api/windows.ui.windowmanagement.appwindow.requestmoveadjacenttowindow)、 [RequestMoveRelativeToDisplayRegion](/uwp/api/windows.ui.windowmanagement.appwindow.requestmoverelativetodisplayregion)、 [requestmovetodisplayregion](/uwp/api/windows.ui.windowmanagement.appwindow.requestmovetodisplayregion)。

この例では、このコードによってウィンドウが、ウィンドウが生成されるメインビューの横に移動します。

```csharp
colorPickerAppWindow.RequestMoveAdjacentToCurrentView();
```

ウィンドウの現在のサイズと配置に関する情報を取得するには、 [Getplacement](/uwp/api/windows.ui.windowmanagement.appwindow.getplacement)を呼び出します。 これにより、ウィンドウの現在の[Displayregion](/uwp/api/windows.ui.windowmanagement.appwindowplacement.displayregion)、 [Offset](/uwp/api/windows.ui.windowmanagement.appwindowplacement.offset)、および[Size](/uwp/api/windows.ui.windowmanagement.appwindowplacement.size)を提供する[appwindowplacement](/uwp/api/windows.ui.windowmanagement.appwindowplacement)オブジェクトが返されます。

たとえば、このコードを呼び出して、ウィンドウを画面の右上隅に移動できます。 このコードは、ウィンドウが表示された後に呼び出す必要があります。それ以外の場合、GetPlacement の呼び出しによって返されるウィンドウサイズは 0, 0 になり、オフセットは正しくありません。

```csharp
DisplayRegion displayRegion = window.GetPlacement().DisplayRegion;
double displayRegionWidth = displayRegion.WorkAreaSize.Width;
double windowWidth = window.GetPlacement().Size.Width;
int horizontalOffset = (int)(displayRegionWidth - windowWidth);
window.RequestMoveRelativeToDisplayRegion(displayRegion, new Point(horizontalOffset, 0));
```

## <a name="request-a-presentation-configuration"></a>プレゼンテーション構成を要求する

[Appwindowpresenter](/uwp/api/windows.ui.windowmanagement.appwindowpresenter)クラスでは、表示されているデバイスに適した定義済みの構成を使用して、 [appwindow](/uwp/api/windows.ui.windowmanagement.appwindow)を表示できます。 [Appwindowプレゼンテーション構成](/uwp/api/windows.ui.windowmanagement.appwindowpresentationconfiguration)値を使用して、ウィンドウをまたは`FullScreen` `CompactOverlay`モードにすることができます。

この例では、次の操作を実行する方法を示します。

- 使用可能なウィンドウのプレゼンテーションが変更された場合に通知を受けるには、 [Appwindow. Changed](/uwp/api/windows.ui.windowmanagement.appwindow.changed)イベントを使用します。
- [Appwindow. プレゼンター](/uwp/api/windows.ui.windowmanagement.appwindow.presenter)プロパティを使用して、現在の[appwindowプレゼンター](/uwp/api/windows.ui.windowmanagement.appwindowpresenter)を取得します。
- 特定の[Appwindowwebservice](/uwp/api/windows.ui.windowmanagement.appwindowpresentationkind)がサポートされているかどうかを確認するには、 [Ispresentationsupported](/uwp/api/windows.ui.windowmanagement.appwindowpresenter.ispresentationsupported)を呼び出します。
- [GetConfiguration](/uwp/api/windows.ui.windowmanagement.appwindowpresenter.getconfiguration)を呼び出して、現在使用されている構成の種類を確認します。
- [Requestpresentation](/uwp/api/windows.ui.windowmanagement.appwindowpresenter.requestpresentation)を呼び出して、現在の構成を変更します。

```csharp
private void Window_Changed(AppWindow sender, AppWindowChangedEventArgs args)
{
    if (args.DidAvailableWindowPresentationsChange)
    {
        EnablePresentationButtons(sender);
    }

    if (args.DidWindowPresentationChange)
    {
        ConfigText.Text = window.Presenter.GetConfiguration().Kind.ToString();
    }

    if (args.DidSizeChange)
    {
        SizeText.Text = window.GetPlacement().Size.ToString();
    }
}

private void EnablePresentationButtons(AppWindow window)
{
    // Check whether the current AppWindowPresenter supports CompactOverlay.
    if (window.Presenter.IsPresentationSupported(AppWindowPresentationKind.CompactOverlay))
    {
        // Show the CompactOverlay button...
        compactOverlayButton.Visibility = Visibility.Visible;
    }
    else
    {
        // Hide the CompactOverlay button...
        compactOverlayButton.Visibility = Visibility.Collapsed;
    }

    // Check whether the current AppWindowPresenter supports FullScreen?
    if (window.Presenter.IsPresentationSupported(AppWindowPresentationKind.FullScreen))
    {
        // Show the FullScreen button...
        fullScreenButton.Visibility = Visibility.Visible;
    }
    else
    {
        // Hide the FullScreen button...
        fullScreenButton.Visibility = Visibility.Collapsed;
    }
}

private void CompactOverlayButton_Click(object sender, RoutedEventArgs e)
{
    if (window.Presenter.GetConfiguration().Kind != AppWindowPresentationKind.CompactOverlay)
    {
        window.Presenter.RequestPresentation(AppWindowPresentationKind.CompactOverlay);
        fullScreenButton.IsChecked = false;
    }
    else
    {
        window.Presenter.RequestPresentation(AppWindowPresentationKind.Default);
    }
}

private void FullScreenButton_Click(object sender, RoutedEventArgs e)
{
    if (window.Presenter.GetConfiguration().Kind != AppWindowPresentationKind.FullScreen)
    {
        window.Presenter.RequestPresentation(AppWindowPresentationKind.FullScreen);
        compactOverlayButton.IsChecked = false;
    }
    else
    {
        window.Presenter.RequestPresentation(AppWindowPresentationKind.Default);
    }
}
```

## <a name="reuse-xaml-elements"></a>XAML 要素の再利用

[Appwindow](/uwp/api/windows.ui.windowmanagement.appwindow)では、同じ UI スレッドを持つ複数の XAML ツリーを使用できます。 ただし、XAML 要素は XAML ツリーに一度だけ追加できます。 UI の一部を1つのウィンドウから別のウィンドウに移動する場合は、XAML ツリー内での配置を管理する必要があります。

この例では、[カラーピッカー](/uwp/api/windows.ui.xaml.controls.colorpicker)コントロールを再利用して、メインウィンドウとセカンダリウィンドウの間で移動する方法を示します。

カラーピッカーはの`MainPage`xaml で宣言され、 `MainPage` xaml ツリーに配置されます。

```xaml
<StackPanel x:Name="colorPickerContainer" Grid.Column="1" Background="WhiteSmoke">
    <Button Click="DetachColorPickerButton_Click" HorizontalAlignment="Right">
        <FontIcon FontFamily="Segoe MDL2 Assets" Glyph="&#xE2B4;" />
    </Button>
    <ColorPicker x:Name="colorPicker" Margin="12" Width="288"
                 IsColorChannelTextInputVisible="False"
                 ColorChanged="ColorPicker_ColorChanged"/>
</StackPanel>
```

新しい appwindow に配置するためにカラーピッカーをデタッチしたら、最初にその親コンテナーから削除する`MainPage`ことによって、それを XAML ツリーから削除する必要があります。 ただし、この例では必須ではありませんが、親コンテナーも非表示になります。

```csharp
colorPickerContainer.Children.Remove(colorPicker);
colorPickerContainer.Visibility = Visibility.Collapsed;
```

その後、新しい XAML ツリーに追加できます。 ここでは、まず ColorPicker の親コンテナーとなる[グリッド](/uwp/api/windows.ui.xaml.controls.grid)を作成し、グリッドの子として colorpicker を追加します。 (これにより、後でこの XAML ツリーから ColorPicker を簡単に削除できます)。次に、グリッドを新しいウィンドウの XAML ツリーのルートとして設定します。

```csharp
Grid appWindowRootGrid = new Grid();
appWindowRootGrid.Children.Add(colorPicker);

// Create a new window
colorPickerAppWindow = await AppWindow.TryCreateAsync();

// Attach the XAML content to our window
ElementCompositionPreview.SetAppWindowContent(colorPickerAppWindow, appWindowRootGrid);
```

[Appwindow](/uwp/api/windows.ui.windowmanagement.appwindow)が終了したら、プロセスを逆にします。 まず、[グリッド](/uwp/api/windows.ui.xaml.controls.grid)から[colorpicker](/uwp/api/windows.ui.xaml.controls.colorpicker)を削除し、で`MainPage` [StackPanel](/uwp/api/windows.ui.xaml.controls.stackpanel)の子として追加します。

```csharp
// When the window is closed, be sure to release XAML resources
// and the reference to the window.
colorPickerAppWindow.Closed += delegate
{
    appWindowRootGrid.Children.Remove(colorPicker);
    appWindowRootGrid = null;
    colorPickerAppWindow = null;

    colorPickerContainer.Children.Add(colorPicker);
    colorPickerContainer.Visibility = Visibility.Visible;
};
```

```csharp
private async void DetachColorPickerButton_Click(object sender, RoutedEventArgs e)
{
    ColorPickerContainer.Visibility = Visibility.Collapsed;

    // Create the color picker window.
    if (colorPickerAppWindow == null)
    {
        ColorPickerContainer.Children.Remove(colorPicker);

        Grid appWindowRootGrid = new Grid();
        appWindowRootGrid.Children.Add(colorPicker);

        // Create a new window
        colorPickerAppWindow = await AppWindow.TryCreateAsync();
        colorPickerAppWindow.RequestMoveAdjacentToCurrentView();
        colorPickerAppWindow.RequestSize(new Size(300, 428));
        colorPickerAppWindow.Title = "Color picker";

        // Attach the XAML content to our window
        ElementCompositionPreview.SetAppWindowContent(colorPickerAppWindow, appWindowRootGrid);

        // When the window is closed, be sure to release XAML resources
        // and the reference to the window.
        colorPickerAppWindow.Closed += delegate
        {
            appWindowRootGrid.Children.Remove(colorPicker);
            appWindowRootGrid = null;
            colorPickerAppWindow = null;

            ColorPickerContainer.Children.Add(colorPicker);
            ColorPickerContainer.Visibility = Visibility.Visible;
        };
    }
    // Show the window.
    await colorPickerAppWindow.TryShowAsync();
}
```

## <a name="show-a-dialog-box"></a>ダイアログボックスを表示する

既定では、コンテンツ ダイアログはルート [ApplicationView](/uwp/api/windows.ui.viewmanagement.applicationview) を基準としてモーダルに表示されます。 [Appwindow](/uwp/api/windows.ui.windowmanagement.appwindow)内で[contentdialog](/uwp/api/windows.ui.xaml.controls.contentdialog)を使用する場合は、ダイアログの xamlroot を XAML ホストのルートに手動で設定する必要があります。

これを行うには、ContentDialog の[xamlroot](/uwp/api/windows.ui.xaml.uielement.xamlroot)プロパティを、appwindow 内に既にある要素と同じ[xamlroot](/uwp/api/windows.ui.xaml.xamlroot)に設定します。 ここでは、このコードはボタンの[click](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click)イベントハンドラー内にあるため、_送信側_(クリックされたボタン) を使用して xamlroot を取得できます。

```csharp
if (ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 8))
{
    simpleDialog.XamlRoot = ((Button)sender).XamlRoot;
}
```

メインウィンドウ (ApplicationView) に加えて、1つまたは複数の AppWindows が開いている場合、モーダルダイアログボックスによってルートになっているウィンドウのみがブロックされるため、各ウィンドウはダイアログを開くことができます。 ただし、一度に1つのスレッドごとに開くことができる[Contentdialog](/uwp/api/windows.ui.xaml.controls.contentdialog)は1つだけです。 2 つの ContentDialog を開こうとすると、個別の AppWindow で開く場合でも、例外がスローされます。

これを管理するには、別のダイアログが既に開い`try/catch`ている場合に例外をキャッチするために、少なくともブロックでダイアログを開いておく必要があります。

```csharp
try
{
    ContentDialogResult result = await simpleDialog.ShowAsync();
}
catch (Exception)
{
    // The dialog didn't open, probably because another dialog is already open.
}
```

ダイアログを管理する別の方法として、現在開いているダイアログを追跡して、新しいダイアログを開く前に閉じます。 ここでは、この目的のため`MainPage`に`CurrentDialog` 、という静的プロパティを作成します。

```csharp
public sealed partial class MainPage : Page
{
    // Track the last opened dialog so you can close it if another dialog tries to open.
    public static ContentDialog CurrentDialog { get; set; } = null;

   // ...
}
```

次に、現在開いているダイアログがあるかどうかを確認し、存在する場合は、 [Hide](/uwp/api/windows.ui.xaml.controls.contentdialog.hide)メソッドを呼び出して閉じます。 最後に、新しいダイアログをに`CurrentDialog`割り当てて、表示しようとします。

```csharp
private async void DialogButton_Click(object sender, RoutedEventArgs e)
{
    ContentDialog simpleDialog = new ContentDialog
    {
        Title = "Content dialog",
        Content = "Dialog box for " + window.Title,
        CloseButtonText = "Ok"
    };

    if (MainPage.CurrentDialog != null)
    {
        MainPage.CurrentDialog.Hide();
    }
    MainPage.CurrentDialog = simpleDialog;

    // Use this code to associate the dialog to the appropriate AppWindow by setting
    // the dialog's XamlRoot to the same XamlRoot as an element that is already
    // present in the AppWindow.
    if (ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 8))
    {
        simpleDialog.XamlRoot = ((Button)sender).XamlRoot;
    }

    try
    {
        ContentDialogResult result = await simpleDialog.ShowAsync();
    }
    catch (Exception)
    {
        // The dialog didn't open, probably because another dialog is already open.
    }
}
```

プログラムによってダイアログを閉じることが望ましくない場合は、 `CurrentDialog`として割り当てないでください。 ここでは、をクリック`Ok`したときにのみ、無視する必要がある重要なダイアログを示します。`MainPage` は`CurrentDialog`として割り当てられていないため、プログラムで閉じようとすることはありません。

```csharp
public sealed partial class MainPage : Page
{
    // Track the last opened dialog so you can close it if another dialog tries to open.
    public static ContentDialog CurrentDialog { get; set; } = null;

    // ...
    private async void DialogButton_Click(object sender, RoutedEventArgs e)
    {
        ContentDialog importantDialog = new ContentDialog
        {
            Title = "Important dialog",
            Content = "This dialog can only be dismissed by clicking Ok.",
            CloseButtonText = "Ok"
        };

        if (MainPage.CurrentDialog != null)
        {
            MainPage.CurrentDialog.Hide();
        }
        // Do not track this dialog as the MainPage.CurrentDialog.
        // It should only be closed by clicking the Ok button.
        MainPage.CurrentDialog = null;

        try
        {
            ContentDialogResult result = await importantDialog.ShowAsync();
        }
        catch (Exception)
        {
            // The dialog didn't open, probably because another dialog is already open.
        }
    }
    // ...
}
```

## <a name="complete-code"></a>コードを完成させる

### <a name="mainpagexaml"></a>MainPage.xaml

```xaml
<Page
    x:Class="HelloAppWindow.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:HelloAppWindow"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d"
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    <Grid>
        <Grid.ColumnDefinitions>
            <ColumnDefinition/>
            <ColumnDefinition Width="Auto"/>
        </Grid.ColumnDefinitions>
        <StackPanel VerticalAlignment="Center" HorizontalAlignment="Center">
            <Button x:Name="NewWindowButton" Content="Open new window" 
                    Click="ShowNewWindowButton_Click" Margin="0,12"/>
            <Button Content="Open dialog" Click="DialogButton_Click" 
                    HorizontalAlignment="Stretch"/>
            <Button Content="Close all" Click="CloseAllButton_Click" 
                    Margin="0,12" HorizontalAlignment="Stretch"/>
        </StackPanel>

<StackPanel x:Name="colorPickerContainer" Grid.Column="1" Background="WhiteSmoke">
    <Button Click="DetachColorPickerButton_Click" HorizontalAlignment="Right">
        <FontIcon FontFamily="Segoe MDL2 Assets" Glyph="&#xE2B4;" />
    </Button>
            <ColorPicker x:Name="colorPicker" Margin="12" Width="288"
                 IsColorChannelTextInputVisible="False"
                 ColorChanged="ColorPicker_ColorChanged"/>
        </StackPanel>
    </Grid>
</Page>

```

### <a name="mainpagexamlcs"></a>MainPage.xaml.cs

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Windows.Foundation;
using Windows.UI;
using Windows.UI.WindowManagement;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Hosting;
using Windows.UI.Xaml.Media;

// The Blank Page item template is documented at https://go.microsoft.com/fwlink/?LinkId=402352&clcid=0x409

namespace HelloAppWindow
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    public sealed partial class MainPage : Page
    {
        AppWindow colorPickerAppWindow;

        // Track open app windows in a Dictionary.
        public static Dictionary<UIContext, AppWindow> AppWindows { get; set; }
            = new Dictionary<UIContext, AppWindow>();

        // Track the last opened dialog so you can close it if another dialog tries to open.
        public static ContentDialog CurrentDialog { get; set; } = null;

        public MainPage()
        {
            this.InitializeComponent();
        }

        private async void ShowNewWindowButton_Click(object sender, RoutedEventArgs e)
        {
            // Create a new window.
            AppWindow appWindow = await AppWindow.TryCreateAsync();

            // Create a Frame and navigate to the Page you want to show in the new window.
            Frame appWindowContentFrame = new Frame();
            appWindowContentFrame.Navigate(typeof(AppWindowPage));

            // Get a reference to the page instance and assign the
            // newly created AppWindow to the MyAppWindow property.
            AppWindowPage page = (AppWindowPage)appWindowContentFrame.Content;
            page.MyAppWindow = appWindow;
            page.TextColorBrush = new SolidColorBrush(colorPicker.Color);

            // Attach the XAML content to the window.
            ElementCompositionPreview.SetAppWindowContent(appWindow, appWindowContentFrame);

            // Add the new page to the Dictionary using the UIContext as the Key.
            AppWindows.Add(appWindowContentFrame.UIContext, appWindow);
            appWindow.Title = "App Window " + AppWindows.Count.ToString();

            // When the window is closed, be sure to release XAML resources
            // and the reference to the window.
            appWindow.Closed += delegate
            {
                MainPage.AppWindows.Remove(appWindowContentFrame.UIContext);
                appWindowContentFrame.Content = null;
                appWindow = null;
            };

            // Show the window.
            await appWindow.TryShowAsync();
        }

        private async void DialogButton_Click(object sender, RoutedEventArgs e)
        {
            ContentDialog importantDialog = new ContentDialog
            {
                Title = "Important dialog",
                Content = "This dialog can only be dismissed by clicking Ok.",
                CloseButtonText = "Ok"
            };

            if (MainPage.CurrentDialog != null)
            {
                MainPage.CurrentDialog.Hide();
            }
            // Do not track this dialog as the MainPage.CurrentDialog.
            // It should only be closed by clicking the Ok button.
            MainPage.CurrentDialog = null;

            try
            {
                ContentDialogResult result = await importantDialog.ShowAsync();
            }
            catch (Exception)
            {
                // The dialog didn't open, probably because another dialog is already open.
            }
        }

        private async void DetachColorPickerButton_Click(object sender, RoutedEventArgs e)
        {
            // Create the color picker window.
            if (colorPickerAppWindow == null)
            {
                colorPickerContainer.Children.Remove(colorPicker);
                colorPickerContainer.Visibility = Visibility.Collapsed;

                Grid appWindowRootGrid = new Grid();
                appWindowRootGrid.Children.Add(colorPicker);

                // Create a new window
                colorPickerAppWindow = await AppWindow.TryCreateAsync();
                colorPickerAppWindow.RequestMoveAdjacentToCurrentView();
                colorPickerAppWindow.RequestSize(new Size(300, 428));
                colorPickerAppWindow.Title = "Color picker";

                // Attach the XAML content to our window
                ElementCompositionPreview.SetAppWindowContent(colorPickerAppWindow, appWindowRootGrid);

                // Make sure to release the reference to this window, 
                // and release XAML resources, when it's closed
                colorPickerAppWindow.Closed += delegate
                {
                    appWindowRootGrid.Children.Remove(colorPicker);
                    appWindowRootGrid = null;
                    colorPickerAppWindow = null;

                    colorPickerContainer.Children.Add(colorPicker);
                    colorPickerContainer.Visibility = Visibility.Visible;
                };
            }
            // Show the window.
            await colorPickerAppWindow.TryShowAsync();
        }

        private void ColorPicker_ColorChanged(ColorPicker sender, ColorChangedEventArgs args)
        {
            NewWindowButton.Background = new SolidColorBrush(args.NewColor);
        }

        private async void CloseAllButton_Click(object sender, RoutedEventArgs e)
        {
            while (AppWindows.Count > 0)
            {
                await AppWindows.Values.First().CloseAsync();
            }
        }
    }
}

```

### <a name="appwindowpagexaml"></a>AppWindowPage .xaml

```xaml
<Page
    x:Class="HelloAppWindow.AppWindowPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:HelloAppWindow"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d"
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    <Grid>
        <TextBlock x:Name="TitleTextBlock" Text="Hello AppWindow!" FontSize="24" HorizontalAlignment="Center" Margin="24"/>

        <StackPanel VerticalAlignment="Center" HorizontalAlignment="Center">
            <Button Content="Open dialog" Click="DialogButton_Click"
                    Width="200" Margin="0,4"/>
            <Button Content="Move window" Click="MoveWindowButton_Click"
                    Width="200" Margin="0,4"/>
            <ToggleButton Content="Compact Overlay" x:Name="compactOverlayButton" Click="CompactOverlayButton_Click"
                          Width="200" Margin="0,4"/>
            <ToggleButton Content="Full Screen" x:Name="fullScreenButton" Click="FullScreenButton_Click"
                          Width="200" Margin="0,4"/>
            <Grid>
                <TextBlock Text="Size:"/>
                <TextBlock x:Name="SizeText" HorizontalAlignment="Right"/>
            </Grid>
            <Grid>
                <TextBlock Text="Presentation:"/>
                <TextBlock x:Name="ConfigText" HorizontalAlignment="Right"/>
            </Grid>
        </StackPanel>
    </Grid>
</Page>
```

### <a name="appwindowpagexamlcs"></a>AppWindowPage.xaml.cs

```csharp
using System;
using Windows.Foundation;
using Windows.Foundation.Metadata;
using Windows.UI;
using Windows.UI.WindowManagement;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Media;

// The Blank Page item template is documented at https://go.microsoft.com/fwlink/?LinkId=234238

namespace HelloAppWindow
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    public sealed partial class AppWindowPage : Page
    {
        AppWindow window;

        public AppWindow MyAppWindow { get; set; }

        public SolidColorBrush TextColorBrush { get; set; } = new SolidColorBrush(Colors.Black);

        public AppWindowPage()
        {
            this.InitializeComponent();

            Loaded += AppWindowPage_Loaded;
        }

        private void AppWindowPage_Loaded(object sender, RoutedEventArgs e)
        {
            // Get the reference to this AppWindow that was stored when it was created.
            window = MainPage.AppWindows[this.UIContext];

            // Set up event handlers for the window.
            window.Changed += Window_Changed;

            TitleTextBlock.Foreground = TextColorBrush;
        }

        private async void DialogButton_Click(object sender, RoutedEventArgs e)
        {
            ContentDialog simpleDialog = new ContentDialog
            {
                Title = "Content dialog",
                Content = "Dialog box for " + window.Title,
                CloseButtonText = "Ok"
            };

            if (MainPage.CurrentDialog != null)
            {
                MainPage.CurrentDialog.Hide();
            }
            MainPage.CurrentDialog = simpleDialog;

            // Use this code to associate the dialog to the appropriate AppWindow by setting
            // the dialog's XamlRoot to the same XamlRoot as an element that is already 
            // present in the AppWindow.
            if (ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 8))
            {
                simpleDialog.XamlRoot = ((Button)sender).XamlRoot;
            }

            try
            {
                ContentDialogResult result = await simpleDialog.ShowAsync();
            }
            catch (Exception)
            {
                // The dialog didn't open, probably because another dialog is already open.
            }
        }

        private void Window_Changed(AppWindow sender, AppWindowChangedEventArgs args)
        {
            if (args.DidAvailableWindowPresentationsChange)
            {
                EnablePresentationButtons(sender);
            }

            if (args.DidWindowPresentationChange)
            {
                ConfigText.Text = window.Presenter.GetConfiguration().Kind.ToString();
            }

            if (args.DidSizeChange)
            {
                SizeText.Text = window.GetPlacement().Size.ToString();
            }
        }

        private void EnablePresentationButtons(AppWindow window)
        {
            // Check whether the current AppWindowPresenter supports CompactOverlay.
            if (window.Presenter.IsPresentationSupported(AppWindowPresentationKind.CompactOverlay))
            {
                // Show the CompactOverlay button...
                compactOverlayButton.Visibility = Visibility.Visible;
            }
            else
            {
                // Hide the CompactOverlay button...
                compactOverlayButton.Visibility = Visibility.Collapsed;
            }

            // Check whether the current AppWindowPresenter supports FullScreen?
            if (window.Presenter.IsPresentationSupported(AppWindowPresentationKind.FullScreen))
            {
                // Show the FullScreen button...
                fullScreenButton.Visibility = Visibility.Visible;
            }
            else
            {
                // Hide the FullScreen button...
                fullScreenButton.Visibility = Visibility.Collapsed;
            }
        }

        private void CompactOverlayButton_Click(object sender, RoutedEventArgs e)
        {
            if (window.Presenter.GetConfiguration().Kind != AppWindowPresentationKind.CompactOverlay)
            {
                window.Presenter.RequestPresentation(AppWindowPresentationKind.CompactOverlay);
                fullScreenButton.IsChecked = false;
            }
            else
            {
                window.Presenter.RequestPresentation(AppWindowPresentationKind.Default);
            }
        }

        private void FullScreenButton_Click(object sender, RoutedEventArgs e)
        {
            if (window.Presenter.GetConfiguration().Kind != AppWindowPresentationKind.FullScreen)
            {
                window.Presenter.RequestPresentation(AppWindowPresentationKind.FullScreen);
                compactOverlayButton.IsChecked = false;
            }
            else
            {
                window.Presenter.RequestPresentation(AppWindowPresentationKind.Default);
            }
        }

        private void MoveWindowButton_Click(object sender, RoutedEventArgs e)
        {
            DisplayRegion displayRegion = window.GetPlacement().DisplayRegion;
            double displayRegionWidth = displayRegion.WorkAreaSize.Width;
            double windowWidth = window.GetPlacement().Size.Width;
            int horizontalOffset = (int)(displayRegionWidth - windowWidth);
            window.RequestMoveRelativeToDisplayRegion(displayRegion, new Point(horizontalOffset, 0));
        }
    }
}
```

## <a name="related-topics"></a>関連トピック

- [複数のビューを表示する](show-multiple-views.md)
- [ApplicationView を使用して複数のビューを表示する](application-view.md)
