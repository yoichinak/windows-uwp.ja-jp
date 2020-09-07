---
description: アプリの独立した部分を別々のウィンドウで表示できるようにして、ユーザーの生産性を高めます。
title: アプリの複数のビューの表示
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 7b58841420c93f3fee02b0f283012fe45c468618
ms.sourcegitcommit: b0cfbab1ed8749ef572ba6971e6b206717d12c12
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89219135"
---
# <a name="show-multiple-views-for-an-app"></a>アプリの複数のビューの表示

![複数のウィンドウでアプリを表示するワイヤーフレーム](images/multi-view.gif)

アプリの独立した部分を別々のウィンドウで表示できるようにすることは、ユーザーが生産性を高めるために役立ちます。 アプリに複数のウィンドウを作成すると、タスクバーに各ウィンドウが別々に表示されます。 ユーザーはアプリ ウィンドウの移動、サイズ変更、表示、非表示を個別に行うことができます。また、個別のアプリの場合と同じように各アプリ ウィンドウを切り替えることができます。

> **重要な API**:[Windows.UI.ViewManagement namespace](/uwp/api/windows.ui.viewmanagement)、[Windows.UI.WindowManagement namespace](/uwp/api/windows.ui.windowmanagement)

## <a name="when-should-an-app-use-multiple-views"></a>アプリが複数のビューを使用する場合

複数のビューによるメリットを活用できる、さまざまなシナリオがあります。 以下は例です。

- 受信したメッセージの一覧を表示しながら、新しいメールを作成できるメール アプリ
- 複数の連絡先情報を並列に表示して比較できるアドレス帳アプリ
- 再生中の曲の情報を表示しながら、その他の利用可能な曲のリストを閲覧できるミュージック プレイヤー アプリ
- ノートの 1 つのページから別のページに情報をコピーできるメモ アプリ
- すべてのヘッドライン概要に目を通しながら、後から読むための記事を複数開くことができる閲覧アプリ

各アプリのレイアウトはそれぞれ異なりますが、「新しいウィンドウ」ボタンを予測可能な場所に含めることを推奨します。たとえば、新しいウィンドウで開くことができるコンテンツの右上隅などです。 さらに、[新しいウィンドウで開く] ための[コンテキスト メニュー](../controls-and-patterns/menus.md) オプションを含めることを検討します。

アプリの別のインスタンス (同じインスタンスの別のウィンドウではなく) を作成するには、[マルチインスタンスの Windows アプリの作成](../../launch-resume/multi-instance-uwp.md)に関するページをご覧ください。

## <a name="windowing-hosts"></a>ウィンドウ化ホスト

アプリ内で Windows コンテンツをホストできるさまざまな方法があります。

- [CoreWindow](/uwp/api/windows.ui.core.corewindow)/[ApplicationView](/uwp/api/windows.ui.viewmanagement.applicationview)

     アプリのビューは、スレッドとウィンドウが 1:1 で対応したもので、アプリがコンテンツの表示に使います。 アプリの起動時に最初に作成されるビューは、*メイン ビュー*と呼ばれます。 各 CoreWindow/ApplicationView は、独自のスレッドで動作します。 さまざまな UI スレッドを操作する必要がある場合、マルチウィンドウ アプリが複雑になる可能性があります。

    アプリのメイン ビューは、常に ApplicationView でホストされます。 セカンダリ ウィンドウ内のコンテンツは、ApplicationView または AppWindow でホストできます。

    ApplicationView を使用して、アプリでセカンダリ ウィンドウを表示する方法については、「[ApplicationView の使用](application-view.md)」を参照してください。
- [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow)

    AppWindow は作成元と同じ UI スレッド上で動作するので、これを使用することでマルチウィンドウ Windows アプリの作成が簡単になります。

    [WindowManagement](/uwp/api/windows.ui.windowmanagement) 名前空間内の AppWindow クラスやその他の API は、Windows 10 バージョン 1903 (SDK 18362) 以降で使用できます。 アプリで以前のバージョンの Windows 10 を対象としている場合は、ApplicationView を使用して、セカンダリ ウィンドウを作成する必要があります。

    AppWindow を使用して、アプリでセカンダリ ウィンドウを表示する方法については、「[AppWindow の使用](app-window.md)」を参照してください。

    > [!NOTE]
    > AppWindow は現在、プレビュー段階です。 つまり、AppWindow を使用するアプリを Store に送信することはできますが、一部のプラットフォームおよびフレームワーク コンポーネントが AppWindow では動作しないことがわかっています (「[制限事項](/uwp/api/windows.ui.windowmanagement.appwindow#limitations)」を参照)。
- [DesktopWindowXamlSource](/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) (XAML Islands)

     Win32 アプリ内の UWP XAML コンテンツ (HWND を使用) は、XAML Islands とも呼ばれ、DesktopWindowXamlSource でホストされます。

    XAML Islands の詳細については、[デスクトップ アプリケーションでの UWP XAML ホスティング API の使用](/windows/apps/desktop/modernize/using-the-xaml-hosting-api)に関するページを参照してください

### <a name="make-code-portable-across-windowing-hosts"></a>ウィンドウ化ホスト間でコードを移植可能にする

XAML コンテンツが [CoreWindow](/uwp/api/windows.ui.core.corewindow) に表示される場合、常に [ApplicationView](/uwp/api/windows.ui.viewmanagement.applicationview) および XAML [Window](/uwp/api/windows.ui.xaml.window) が関連付けられています。 これらのクラスで API を使用すると、ウィンドウの境界などの情報を取得できます。 これらのクラスのインスタンスを取得するには、静的 [CoreWindow.GetForCurrentThread](/uwp/api/windows.ui.core.corewindow.getforcurrentthread) メソッド、[ApplicationView.GetForCurrentView](/uwp/api/windows.ui.viewmanagement.applicationview.getforcurrentview) メソッド、または [Window.Current](/uwp/api/windows.ui.xaml.window.current) プロパティを使用します。 また、`GetForCurrentView` パターンを使用して、[DisplayInformation.GetForCurrentView](/uwp/api/windows.graphics.display.displayinformation.getforcurrentview) などのクラスのインスタンスを取得するクラスも多数あります。

これらの API は、CoreWindow/ApplicationView の XAML コンテンツのツリーが 1 つしかなく、XAML には、それがホストされているコンテキストがその CoreWindow/ApplicationView であることがわかっているため、機能します。

XAML コンテンツが AppWindow または DesktopWindowXamlSource 内で実行している場合、複数の XAML コンテンツのツリーを同じスレッドで同時に実行させることができます。 この場合、コンテンツが現在の CoreWindow/ApplicationView (および XAML ウィンドウ) 内で実行されなくなるため、これらの API から適切な情報が得られません。

すべてのウィンドウ化ホストでコードが正常に動作するようにするには、[CoreWindow](/uwp/api/windows.ui.core.corewindow)、[ApplicationView](/uwp/api/windows.ui.viewmanagement.applicationview)、および [Window](/uwp/api/windows.ui.xaml.window) を [XamlRoot](/uwp/api/windows.ui.xaml.xamlroot) クラスからコンテキストを取得する新しい API に置き換える必要があります。
XamlRoot クラスは、XAML コンテンツのツリー、およびそれがホストされているコンテキストに関する情報を表します。それが CoreWindow、AppWindow、DesktopWindowXamlSource のいずれでも同じです。 この抽象化レイヤーを使用すると、XAML が実行されるウィンドウ化ホストに関係なく、同じコードを記述できます。

この表では、ウィンドウ化ホスト間で正常に機能しないコードと、それと置換できる新しい移植可能コード、および変更する必要のない API を示しています。

| 使用していた場合... | 置換先... |
| - | - |
| CoreWindow.GetForCurrentThread().[Bounds](/uwp/api/windows.ui.core.corewindow.bounds) | _uiElement_.XamlRoot.[Size](/uwp/api/windows.ui.xaml.xamlroot.size) |
| CoreWindow.GetForCurrentThread().[SizeChanged](/uwp/api/windows.ui.core.corewindow.sizechanged) | _uiElement_.XamlRoot.[Changed](/uwp/api/windows.ui.xaml.xamlroot.changed) |
| CoreWindow.[Visible](/uwp/api/windows.ui.core.corewindow.visible) | _uiElement_.XamlRoot.[IsHostVisible](/uwp/api/windows.ui.xaml.xamlroot.ishostvisible) |
| CoreWindow.[VisibilityChanged](/uwp/api/windows.ui.core.corewindow.visibilitychanged) | _uiElement_.XamlRoot.[Changed](/uwp/api/windows.ui.xaml.xamlroot.changed) |
| CoreWindow.GetForCurrentThread().[GetKeyState](/uwp/api/windows.ui.core.corewindow.getkeystate) | 変更なし。 これは AppWindow と DesktopWindowXamlSource でサポートされています。 |
| CoreWindow.GetForCurrentThread().[GetAsyncKeyState](/uwp/api/windows.ui.core.corewindow.getasynckeystate) | 変更なし。 これは AppWindow と DesktopWindowXamlSource でサポートされています。 |
| Window.[Current](/uwp/api/windows.ui.xaml.window.current) | 現在の CoreWindow に緊密にバインドされているメイン XAML Window オブジェクトを返します。 この表の後の「注」を参照してください。 |
| Window.Current.[Bounds](/uwp/api/windows.ui.xaml.window.bounds) | _uiElement_.XamlRoot.[Size](/uwp/api/windows.ui.xaml.xamlroot.size) |
| Window.Current.[Content](/uwp/api/windows.ui.xaml.window.content) | UIElement root =  _uiElement_.XamlRoot.[Content](/uwp/api/windows.ui.xaml.xamlroot.content) |
| Window.Current.[Compositor](/uwp/api/windows.ui.xaml.window.compositor) | 変更なし。 これは AppWindow と DesktopWindowXamlSource でサポートされています。 |
| VisualTreeHelper.[FindElementsInHostCoordinates](/uwp/api/windows.ui.xaml.media.visualtreehelper.findelementsinhostcoordinates)<br>UIElement パラメーターは省略可能ですが、Island でホストされているときに UIElement が指定されていないと、メソッドは例外を発生させます。 | UIElement として _uiElement_.XamlRoot を指定し、空のままにしないでください。 |
| VisualTreeHelper.[GetOpenPopups](/uwp/api/windows.ui.xaml.media.visualtreehelper.getopenpopups)<br/>XAML Islands アプリでは、これによってエラーがスローされます。 AppWindow アプリでは、これによって、メインウィンドウに開いているポップアップが返されます。 | VisualTreeHelper.[GetOpenPopupsForXamlRoot](/uwp/api/windows.ui.xaml.media.visualtreehelper.getopenpopupsforxamlroot)(_uiElement_.XamlRoot) |
| FocusManager.[GetFocusedElement](/uwp/api/windows.ui.xaml.input.focusmanager.getfocusedelement) | FocusManager.[GetFocusedElement](/uwp/api/windows.ui.xaml.input.focusmanager.getfocusedelement#Windows_UI_Xaml_Input_FocusManager_GetFocusedElement_Windows_UI_Xaml_XamlRoot_)(_uiElement_.XamlRoot) |
| contentDialog.ShowAsync() | contentDialog.[XamlRoot](/uwp/api/windows.ui.xaml.uielement.xamlroot) = _uiElement_.XamlRoot;<br/>contentDialog.ShowAsync(); |
| menuFlyout.ShowAt(null, new Point(10, 10)); | menuFlyout.[XamlRoot](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.xamlroot) = _uiElement_.XamlRoot;<br/>menuFlyout.ShowAt(null, new Point(10, 10)); |

> [!NOTE]
> DesktopWindowXamlSource の XAML コンテンツの場合、スレッドに CoreWindow/Window が存在しますが、常に非表示で、サイズが 1x1 になります。 アプリから引き続きアクセスできますが、意味のある境界や表示対象は返されません。
>
>AppWindow の XAML コンテンツの場合、同じスレッドには常に 1 つの CoreWindow が存在します。 `GetForCurrentView` または `GetForCurrentThread` API を呼び出すと、その API からは、そのスレッドで実行されている可能性のある AppWindows ではなく、スレッドの CoreWindow の状態を反映するオブジェクトが返されます。


## <a name="dos-and-donts"></a>推奨と非推奨

- 「新しいウィンドウを開く」のグリフを利用することにより、セカンダリ ビューへの明確なエントリ ポイントを提供します。
- セカンダリ ビューの目的をユーザーに伝えます。
- アプリが単一のビューで完全に機能することを確認します。ユーザーは利便性のためにのみ、セカンダリ ビューを開きます。
- 通知やその他の一時的な視覚効果を提供するためにセカンダリ ビューを使用しないようにします。

## <a name="related-topics"></a>関連トピック

- [AppWindow の使用](app-window.md)
- [ApplicationView の使用](application-view.md)
- [ApplicationViewSwitcher](/uwp/api/Windows.UI.ViewManagement.ApplicationViewSwitcher)
- [CreateNewView](/uwp/api/windows.applicationmodel.core.coreapplication.createnewview)
