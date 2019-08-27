---
Description: 別のウィンドウでアプリのさまざまな部分を表示します。
title: アプリの複数のビューの表示
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ee49b5fe5b5956e9069ea196c4d2e029b3a15763
ms.sourcegitcommit: 3cc6eb3bab78f7e68c37226c40410ebca73f82a9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/02/2019
ms.locfileid: "68729519"
---
# <a name="show-multiple-views-for-an-app"></a>アプリの複数のビューの表示

![複数のウィンドウでアプリを表示するワイヤーフレーム](images/multi-view.gif)

アプリの独立した部分を別々のウィンドウで表示できるようにすることは、ユーザーが生産性を高めるために役立ちます。 アプリ用に複数のウィンドウを作成すると、タスクバーに各ウィンドウが個別に表示されます。 ユーザーはアプリ ウィンドウの移動、サイズ変更、表示、非表示を個別に行うことができます。また、個別のアプリの場合と同じように各アプリ ウィンドウを切り替えることができます。

> **重要な API**:[Windows. ui. ViewManagement 名前](/uwp/api/windows.ui.viewmanagement)空間、 [windows. Ui. windowmanagement 名前空間](/uwp/api/windows.ui.windowmanagement)

## <a name="when-should-an-app-use-multiple-views"></a>アプリが複数のビューを使用する場合

複数のビューを活用できるさまざまなシナリオがあります。 以下は例です。

- 受信したメッセージの一覧を表示しながら、新しいメールを作成できるメール アプリ
- 複数の連絡先情報を並列に表示して比較できるアドレス帳アプリ
- 再生中の曲の情報を表示しながら、その他の利用可能な曲のリストを閲覧できるミュージック プレイヤー アプリ
- ノートの 1 つのページから別のページに情報をコピーできるメモ アプリ
- すべてのヘッドライン概要に目を通しながら、後から読むための記事を複数開くことができる閲覧アプリ

各アプリのレイアウトはそれぞれ異なりますが、「新しいウィンドウ」ボタンを予測可能な場所に含めることを推奨します。たとえば、新しいウィンドウで開くことができるコンテンツの右上隅などです。 また、[新しいウィンドウで開く] に[コンテキストメニュー](../controls-and-patterns/menus.md)オプションを含めることも検討してください。

(同じインスタンスに個別のウィンドウではなく) アプリの個別のインスタンスを作成するには、「[複数インスタンスの UWP アプリを作成](../../launch-resume/multi-instance-uwp.md)する」を参照してください。

## <a name="windowing-hosts"></a>ウィンドウホスト

UWP コンテンツは、アプリ内でホストできるさまざまな方法があります。

- [Corewindow](/uwp/api/windows.ui.core.corewindow)/[applicationview](/uwp/api/windows.ui.viewmanagement.applicationview)

     アプリのビューは、スレッドとウィンドウが 1:1 で対応したもので、アプリがコンテンツの表示に使います。 アプリの起動時に最初に作成されるビューは、*メイン ビュー*と呼ばれます。 各 CoreWindow/ApplicationView は、独自のスレッドで動作します。 さまざまな UI スレッドで作業すると、マルチウィンドウアプリが複雑になる可能性があります。

    アプリのメインビューは、常に ApplicationView でホストされます。 セカンダリウィンドウ内のコンテンツは、ApplicationView または AppWindow でホストできます。

    アプリケーションで ApplicationView を使用してセカンダリウィンドウを表示する方法については、「 [ApplicationView を使用](application-view.md)する」を参照してください。
- [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow)

    AppWindow は、作成元と同じ UI スレッド上で動作するため、マルチウィンドウ UWP アプリの作成を簡略化します。

    [Windowmanagement](/uwp/api/windows.ui.windowmanagement)名前空間の appwindow クラスおよびその他の api は、Windows 10 バージョン1903以降で使用できます (SDK 18362)。 アプリが以前のバージョンの Windows 10 を対象としている場合は、ApplicationView を使用してセカンダリウィンドウを作成する必要があります。

    AppWindow を使用してアプリにセカンダリウィンドウを表示する方法については、「 [AppWindow を使用](app-window.md)する」を参照してください。

    > [!NOTE]
    > AppWindow は現在プレビューの段階です。 これは、AppWindow を使用するアプリをストアに送信できることを意味しますが、一部のプラットフォームおよびフレームワークコンポーネントは AppWindow で動作しないことがわかっています (「[制限事項](/uwp/api/windows.ui.windowmanagement.appwindow#limitations)」を参照してください)。
- [Desktopwindowxamlsource](/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource)(XAML Islands)

     Win32 アプリ内の UWP XAML コンテンツ (HWND を使用) は、XAML Islands とも呼ばれ、DesktopWindowXamlSource でホストされます。

    XAML Islands の詳細については、「[デスクトップアプリケーションでの UWP XAML ホスティング API の使用](/windows/apps/desktop/modernize/using-the-xaml-hosting-api)」を参照してください。

### <a name="make-code-portable-across-windowing-hosts"></a>ウィンドウ内のホスト間でコードの移植性を確保する

XAML コンテンツが[Corewindow](/uwp/api/windows.ui.core.corewindow)に表示される場合は、常に、関連付けられている[APPLICATIONVIEW](/uwp/api/windows.ui.viewmanagement.applicationview)および xaml[ウィンドウ](/uwp/api/windows.ui.xaml.window)があります。 これらのクラスで Api を使用すると、ウィンドウの境界などの情報を取得できます。 これらのクラスのインスタンスを取得するには、静的な[Corewindow](/uwp/api/windows.ui.core.corewindow.getforcurrentthread)メソッド、 [GetForCurrentView](/uwp/api/windows.ui.viewmanagement.applicationview.getforcurrentview)メソッド、または[Window. Current](/uwp/api/windows.ui.xaml.window.current)プロパティを使用します。 また、[DisplayInformation.GetForCurrentView](/uwp/api/windows.graphics.display.displayinformation.getforcurrentview) などのクラスのインスタンスを取得するために `GetForCurrentView` パターンを使用するクラスも多数あります。

これらの Api は、CoreWindow/ApplicationView に対する XAML コンテンツのツリーが1つだけであるため機能します。そのため、XAML は、ホストされているコンテキストが CoreWindow/ApplicationView であることを認識しています。

XAML コンテンツが AppWindow または DesktopWindowXamlSource 内で実行されている場合、同じスレッドで同時に実行されている XAML コンテンツの複数のツリーを持つことができます。 この場合、コンテンツは現在の CoreWindow/ApplicationView (および XAML ウィンドウ) 内で実行されなくなるため、これらの Api は適切な情報を提供しません。

すべてのウィンドウホストでコードが正しく動作するようにするには、 [Corewindow](/uwp/api/windows.ui.core.corewindow)、 [applicationview](/uwp/api/windows.ui.viewmanagement.applicationview)、および[Window](/uwp/api/windows.ui.xaml.window)に依存する api を、 [xamlroot](/uwp/api/windows.ui.xaml.xamlroot)クラスからコンテキストを取得する新しい api に置き換える必要があります。
XamlRoot クラスは、XAML コンテンツのツリー、および XAML コンテンツがホストされているコンテキストに関する情報を表します。これは、CoreWindow、AppWindow、DesktopWindowXamlSource のいずれでも同じです。 この抽象化レイヤーを使用すると、XAML が実行されているウィンドウホストに関係なく、同じコードを記述できます。

この表は、ウィンドウホスト間で正常に機能しないコードと、新しい移植可能なコードを示しています。このコードは、変更する必要のない Api と共に使用できます。

| 使用した場合... | 置換後の文字列... |
| - | - |
| CoreWindow.GetForCurrentThread().[Bounds](/uwp/api/windows.ui.core.corewindow.bounds) | _uiElement_.XamlRoot.[Size](/uwp/api/windows.ui.xaml.xamlroot.size) |
| CoreWindow.GetForCurrentThread().[SizeChanged](/uwp/api/windows.ui.core.corewindow.sizechanged) | _uiElement_.XamlRoot.[Changed](/uwp/api/windows.ui.xaml.xamlroot.changed) |
| CoreWindow.[Visible](/uwp/api/windows.ui.core.corewindow.visible) | _uiElement_.XamlRoot.[IsHostVisible](/uwp/api/windows.ui.xaml.xamlroot.ishostvisible) |
| CoreWindow.[VisibilityChanged](/uwp/api/windows.ui.core.corewindow.visibilitychanged) | _uiElement_.XamlRoot.[Changed](/uwp/api/windows.ui.xaml.xamlroot.changed) |
| CoreWindow.GetForCurrentThread().[GetKeyState](/uwp/api/windows.ui.core.corewindow.getkeystate) | Unchanged. これは、AppWindow と DesktopWindowXamlSource でサポートされています。 |
| CoreWindow.GetForCurrentThread().[GetAsyncKeyState](/uwp/api/windows.ui.core.corewindow.getasynckeystate) | Unchanged. これは、AppWindow と DesktopWindowXamlSource でサポートされています。 |
| Window.[Current](/uwp/api/windows.ui.xaml.window.current) | 現在の CoreWindow に密接にバインドされているメイン XAML ウィンドウオブジェクトを返します。 この表の後の注を参照してください。 |
| Window.Current.[Bounds](/uwp/api/windows.ui.xaml.window.bounds) | _uiElement_.XamlRoot.[Size](/uwp/api/windows.ui.xaml.xamlroot.size) |
| Window.Current.[Content](/uwp/api/windows.ui.xaml.window.content) | UIElement root =  _uiElement_.XamlRoot.[Content](/uwp/api/windows.ui.xaml.xamlroot.content) |
| Window.Current.[Compositor](/uwp/api/windows.ui.xaml.window.compositor) | Unchanged. これは、AppWindow と DesktopWindowXamlSource でサポートされています。 |
| VisualTreeHelper.[GetOpenPopups](/uwp/api/windows.ui.xaml.media.visualtreehelper.getopenpopups)<br/>XAML Islands アプリでは、これによってエラーがスローされます。 AppWindow アプリでは、メインウィンドウに開いているポップアップが返されます。 | VisualTreeHelper.[GetOpenPopupsForXamlRoot](/uwp/api/windows.ui.xaml.media.visualtreehelper.getopenpopupsforxamlroot)(_uiElement_.XamlRoot) |
| System.windows.input.focusmanager>.[System.windows.input.focusmanager.getfocusedelement](/uwp/api/windows.ui.xaml.input.focusmanager.getfocusedelement) | System.windows.input.focusmanager>.[System.windows.input.focusmanager.getfocusedelement](/uwp/api/windows.ui.xaml.input.focusmanager.getfocusedelement#Windows_UI_Xaml_Input_FocusManager_GetFocusedElement_Windows_UI_Xaml_XamlRoot_)(_uiElement_。XamlRoot) |
| contentDialog.ShowAsync() | contentDialog.[XamlRoot](/uwp/api/windows.ui.xaml.uielement.xamlroot) = _uiElement_.XamlRoot;<br/>contentDialog. ShowAsync (); |
| menuFlyout.ShowAt(null, new Point(10, 10)); | menuFlyout.[XamlRoot](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.xamlroot) = _uiElement_.XamlRoot;<br/>menuFlyout.ShowAt(null, new Point(10, 10)); |

> [!NOTE]
> DesktopWindowXamlSource の XAML コンテンツの場合、スレッドに CoreWindow/Window が存在しますが、常に非表示になり、サイズが1x1 になります。 アプリには引き続きアクセスできますが、意味のある境界や可視性は返されません。
>
>AppWindow の XAML コンテンツの場合、同じスレッド上に常に1つの CoreWindow が存在します。 Api `GetForCurrentView`または api を`GetForCurrentThread`呼び出すと、その api は、そのスレッドで実行されている可能性のある appwindows ではなく、スレッドの corewindow の状態を反映するオブジェクトを返します。


## <a name="dos-and-donts"></a>推奨と非推奨

- 「新しいウィンドウを開く」のグリフを利用することにより、セカンダリ ビューへの明確なエントリ ポイントを提供します。
- セカンダリ ビューの目的をユーザーに伝えます。
- アプリが1つのビューで完全に機能していることを確認します。ユーザーは、便宜上、セカンダリビューのみを開きます。
- 通知やその他の一時的な視覚効果を提供するためにセカンダリ ビューを使用しないようにします。

## <a name="related-topics"></a>関連トピック

- [AppWindow を使用する](app-window.md)
- [ApplicationView を使用する](application-view.md)
- [ApplicationViewSwitcher](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.ApplicationViewSwitcher)
- [CreateNewView](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.createnewview)
