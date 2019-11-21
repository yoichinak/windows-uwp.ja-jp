---
description: 宣言型 XAML マークアップ形式での UI の定義は、ユニバーサル 8.1 アプリからユニバーサル Windows プラットフォーム (UWP) アプリに適切に変換されます。
title: Windows ランタイム 8.x の XAML と UI の UWP への移植
ms.assetid: 78b86762-7359-474f-b1e3-c2d7cf9aa907
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 19e754fd6a52880c7bc636818acaeda815f9da16
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259107"
---
# <a name="porting-windows-runtime-8x-xaml-and-ui-to-uwp"></a>Windows ランタイム 8.x の XAML と UI の UWP への移植


前のトピックは、「[トラブルシューティング](w8x-to-uwp-troubleshooting.md)」でした。

宣言型 XAML マークアップ形式での UI の定義は、ユニバーサル 8.1 アプリからユニバーサル Windows プラットフォーム (UWP) アプリに適切に変換されます。 ほとんどのマークアップには互換性がありますが、場合によっては、使っているシステムのリソース キーやカスタム テンプレートを調整する必要があります。 ビュー モデルの命令型コードについては、ほとんどあるいはまったく変更する必要はありません。 プレゼンテーション層にあるほとんどのコード (UI 要素を操作するコード) も、簡単に移植できます。

## <a name="imperative-code"></a>命令型コード

プロジェクトのビルド段階に進むだけであれば、重要でないコードのコメントアウトやスタブの挿入を行うことができます。 次に、このセクションの以降のトピック (および前のトピック「[トラブルシューティング](w8x-to-uwp-troubleshooting.md)」) を参考にして、ビルドとランタイムの問題が解決して移植が完了するまで一度に 1 つの問題について反復作業を行います。

## <a name="adaptiveresponsive-ui"></a>アダプティブ/応答性の高い UI

アプリは、画面サイズと解像度が異なるさまざまなデバイスで実行できる場合があります。このため、最小限の手順でアプリを移植するだけでなく、各デバイスで最適な外観になるように UI を調整する必要があります。 アダプティブな Visual State Manager の機能を使って、ウィンドウのサイズを動的に検出し、それに応じてレイアウトを変更できます。その方法を示す例を、Bookstore2 ケース スタディの「[アダプティブ UI](w8x-to-uwp-case-study-bookstore2.md)」に示します。

## <a name="back-button-handling"></a>"戻る" ボタンの処理

For Universal 8.1 apps, Windows Runtime 8.x apps and Windows Phone Store apps have different approaches to the UI you show and the events you handle for the back button. But, for Windows 10 apps, you can use a single approach in your app. モバイル デバイスでは、このボタンはデバイス上の静電容量式のボタンまたはシェル内のボタンとして提供されます。 デスクトップ デバイスでは、アプリ内で戻るナビゲーションが可能な場合には常にアプリのクロムにボタンを追加します。このボタンは、ウィンドウ表示されたアプリのタイトル バーまたはタブレット モードのタスク バーに表示されます。 "戻る" ボタンのイベントはすべてのデバイス ファミリに共通するユニバーサルな概念であり、ハードウェアまたはソフトウェアに実装されるボタンは同じ [**BackRequested**](https://docs.microsoft.com/uwp/api/windows.ui.core.systemnavigationmanager.backrequested) イベントを発生させます。

次の例は、すべてのデバイス ファミリで動作し、同じ処理をすべてのページに適用する場合や、ナビゲーションを確認する必要がない場合 (保存していない変更に関する警告を表示する場合など) に適しています。

```csharp
   // app.xaml.cs

    protected override void OnLaunched(LaunchActivatedEventArgs e)
    {
        [...]

        Windows.UI.Core.SystemNavigationManager.GetForCurrentView().BackRequested += App_BackRequested;
        rootFrame.Navigated += RootFrame_Navigated;
    }

    private void RootFrame_Navigated(object sender, NavigationEventArgs e)
    {
        Frame rootFrame = Window.Current.Content as Frame;

        // Note: On device families that have no title bar, setting AppViewBackButtonVisibility can safely execute 
        // but it will have no effect. Such device families provide back button UI for you.
        if (rootFrame.CanGoBack)
        {
            Windows.UI.Core.SystemNavigationManager.GetForCurrentView().AppViewBackButtonVisibility = 
                Windows.UI.Core.AppViewBackButtonVisibility.Visible;
        }
        else
        {
            Windows.UI.Core.SystemNavigationManager.GetForCurrentView().AppViewBackButtonVisibility = 
                Windows.UI.Core.AppViewBackButtonVisibility.Collapsed;
        }
    }

    private void App_BackRequested(object sender, Windows.UI.Core.BackRequestedEventArgs e)
    {
        Frame rootFrame = Window.Current.Content as Frame;

        if (rootFrame.CanGoBack)
        {
            rootFrame.GoBack();
        }
    }
```

プログラムを使ったアプリの終了に関しても、すべてのデバイス ファミリに対する単一のアプローチがあります。

```csharp
   Windows.UI.Xaml.Application.Current.Exit();
```

## <a name="charms"></a>チャーム

You don't need to change any of your code that integrates with charms, but you do need to add some UI to your app to take the place of the Charms bar, which is not a part of the Windows 10 shell. A Universal 8.1 app running on Windows 10 has its own replacement UI provided by system-rendered chrome in the app's title bar.

## <a name="controls-and-control-styles-and-templates"></a>コントロールとコントロール スタイルおよびテンプレート

A Universal 8.1 app running on Windows 10 will retain the 8.1 appearance and behavior with respect to controls. But, when you port that app to a Windows 10 app, there are some differences in appearance and behavior to be aware of. The architecture and design of controls is essentially unchanged for Windows 10 apps, so the changes are mostly around the design language, simplification, and usability improvements.

**Note**   The PointerOver visual state is relevant in custom styles/templates in Windows 10 apps and in Windows Runtime 8.x apps, but not in Windows Phone Store apps. For this reason (and because of the system resource keys that are supported for Windows 10 apps), we recommend that you re-use the custom styles/templates from your Windows Runtime 8.x apps when you're porting your app to Windows 10.
If you want to be certain that your custom styles/templates are using the latest set of visual states, and are benefitting from performance improvements made to the default styles/templates, then edit a copy of the new Windows 10 default template and re-apply your customization to that. パフォーマンス向上の 1 つの例として、以前に **ContentPresenter** または Panel を囲んでいた **Border** が削除され、子要素が境界線を表示するようになりました。

以下に、コントロールの変更に関する具体的な例を示します。

| コントロール名 | [Change] |
|--------------|--------|
| **AppBar**   | If you are using the **AppBar** control ([**CommandBar**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBar) is recommended instead), then it is not hidden by default in a Windows 10 app. これを制御するには、[**AppBar.ClosedDisplayMode**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbar.closeddisplaymode) プロパティを使います。 |
| **AppBar**、[**CommandBar**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBar) | In a Windows 10 app, **AppBar** and [**CommandBar**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBar) have a **See more** button (the ellipsis). |
| [**CommandBar**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBar) | In a Windows Runtime 8.x app, the secondary commands of a [**CommandBar**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBar) are always visible. In a Windows Phone Store app, and in a Windows 10 app, the don't appear until the command bar opens. |
| [**CommandBar**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBar) | Windows Phone ストア アプリでは、[**CommandBar.IsSticky**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbar.issticky) の値は、バーの簡易非表示の動作に影響しません。 For a Windows 10 app, if **IsSticky** is set to true, then the **CommandBar** disregards a light dismiss gesture. |
| [**CommandBar**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBar) | In a Windows 10 app, [**CommandBar**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBar) does not handle the [**EdgeGesture.Completed**](https://docs.microsoft.com/uwp/api/windows.ui.input.edgegesture.completed) nor [**UIElement.RightTapped**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.righttapped) events. タップまたはスワイプにも応答しません。 ただし、これらのイベントを処理し、[**IsOpen**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbar.isopen) を設定するオプションがあります。 |
| [**DatePicker**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.DatePicker), [**TimePicker**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TimePicker) | [  **DatePicker**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.DatePicker) や [**TimePicker**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TimePicker) に加えられた視覚的な変化によってアプリの外観がどうなるかを確認してください。 For a Windows 10 app running on a mobile device, these controls no longer navigate to a selection page but instead use a light-dismissible popup. |
| [**DatePicker**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.DatePicker), [**TimePicker**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TimePicker) | In a Windows 10 app, you can't put [**DatePicker**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.DatePicker) or [**TimePicker**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TimePicker) inside a fly-out. If you want those controls to be displayed in a popup-type control, then you can use [**DatePickerFlyout**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.DatePickerFlyout) and [**TimePickerFlyout**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TimePickerFlyout). |
| **GridView**、**ListView** | **GridView**/**ListView** については、「[GridView と ListView の変更](#gridview-and-listview-changes)」をご覧ください。 |
| [**Hub**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Hub) | Windows Phone ストア アプリでは、[**Hub**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Hub) コントロールは最後のセクションから最初のセクションに折り返します。 In a Windows Runtime 8.x app, and in a Windows 10 app, hub sections do not wrap around. |
| [**Hub**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Hub) | Windows Phone ストア アプリでは、[**Hub**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Hub) コントロールの背景画像は、ハブ セクションに対する視差効果で移動します。 In a Windows Runtime 8.x app, and in a Windows 10 app, parallax is not used. |
| [**Hub**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Hub)  | ユニバーサル 8.1 アプリでは、[**HubSection.IsHeaderInteractive**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.hubsection.isheaderinteractive) プロパティにより、セクション ヘッダーとその横に表示される山形のグリフが対話型になります。 In a Windows 10 app, there is an interactive "See more" affordance beside the header, but the header itself is not interactive. **IsHeaderInteractive** により、操作で [**Hub.SectionHeaderClick**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.hub.sectionheaderclick) イベントが発生するかどうかが決まります。 |
| **MessageDialog** | **MessageDialog** を使っている場合は、柔軟性が向上した [**ContentDialog**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentDialog) の利用を検討してください。 [XAML UI の基本](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) のサンプルに関するページもご覧ください。 |
| **ListPickerFlyout**、**PickerFlyout**  | **ListPickerFlyout** and **PickerFlyout** are deprecated for a Windows 10 app. 単一選択ポップアップの場合は、[**MenuFlyout**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MenuFlyout) を使います。より複雑なエクスペリエンスの場合は、[**Flyout**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Flyout) を使います。 |
| [**PasswordBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.PasswordBox) | The [**PasswordBox.IsPasswordRevealButtonEnabled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.passwordbox.ispasswordrevealbuttonenabled) property is deprecated in a Windows 10 app, and setting it has no effect. Use [**PasswordBox.PasswordRevealMode**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.passwordbox.passwordrevealmode) instead, which defaults to **Peek** (in which an eye glyph is displayed, like in a Windows Runtime 8.x app). 「[パスワード ボックスのガイドライン](https://docs.microsoft.com/windows/uwp/controls-and-patterns/password-box)」もご覧ください。 |
| [**ピボット**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot) | [  **Pivot**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot) はユニバーサル コントロールとなり、モバイル デバイスでの利用のみに限定されていた制限が排除されました。 |
| [**SearchBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SearchBox) | ユニバーサル デバイス ファミリでは [**SearchBox**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.searchbox) が実装されていますが、モバイル デバイスでは部分的に機能しません。 「[AutoSuggestBox に使用されない SearchBox](#searchbox-deprecated-in-favor-of-autosuggestbox)」をご覧ください。 |
| **SemanticZoom** | **SemanticZoom** については、「[SemanticZoom に関する変更](#semanticzoom-changes)」をご覧ください。 |
| [**ScrollViewer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer)  | [  **ScrollViewer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer) の既定のプロパティの一部が変更されています。 [**HorizontalScrollMode**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.horizontalscrollmode) is **Auto**, [**VerticalScrollMode**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.verticalscrollmode) is **Auto**, and [**ZoomMode**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.zoommode) is **Disabled**. 新しい既定値がアプリに対して適切でない場合は、スタイルで変更するか、コントロール自体のローカル値として変更できます。  |
| [**TextBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox) | In a Windows Runtime 8.x app, spell-checking is off by default for a [**TextBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox). In a Windows Phone Store app, and in a Windows 10 app, it is on by default. |
| [**TextBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox) | [  **TextBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox) の既定のフォント サイズは 11 から 15 に変更されました。 |
| [**TextBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox) | [  **TextBox.TextReadingOrder**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.textreadingorder) の既定値は、**Default** から **DetectFromContent** に変更されました。 これが望ましくない場合は、**UseFlowDirection** を使ってください。 **Default** は使われなくなりました。 |
| Various | Accent color applies to a Windows Phone Store apps, and to Windows 10 apps, but not to Windows Runtime 8.x apps.  |

UWP アプリのコントロールについて詳しくは、「[機能別コントロール](https://docs.microsoft.com/windows/uwp/controls-and-patterns/controls-by-function)」、「[コントロールの一覧](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/)」、「[コントロールのガイドライン](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/index)」をご覧ください。

##  <a name="design-language-in-windows10"></a>Design language in Windows 10

There are some small but important differences in design language between Universal 8.1 apps and Windows 10 apps. 詳しくは、「[Design](https://developer.microsoft.com/en-us/windows/apps/design)」(UWP アプリの設計) をご覧ください。 デザイン言語に変更が加えられていますが、設計原則は維持されています。細部にまで注意を払いながら、簡潔さを追求しています。そのために、クロムよりもコンテンツを優先し、視覚要素を大幅に減らし、真のデジタル領域を常に意識しています。また、視覚的な階層の利用 (特に文字体裁に対して)、グリッド内でのデザイン、滑らかなアニメーションを使ったエクスペリエンスの実現も行っています。

## <a name="effective-pixels-viewing-distance-and-scale-factors"></a>有効ピクセル、視聴距離、スケール ファクター

これまで、表示ピクセルは、デバイスの実際の物理サイズと解像度から UI 要素のサイズとレイアウトを抽象化する方法でした。 現在では、表示ピクセルが有効ピクセルに変わりました。その用語の説明、有効ピクセルが何をするものなのか、および有効ピクセルで使うことができる追加の値について、以下に示します。

一般的な考えとは異なり、"解像度" という用語はピクセル密度の測定値を表しており、ピクセル数ではありません。 "有効解像度" は、画像またはグリフを構成する物理ピクセルを解決して、デバイスの視聴距離と物理ピクセル サイズでの目視による相違の度合を取得する方法です (物理ピクセル サイズの逆数であるピクセル密度)。 有効解像度は、ユーザー中心であるために、エクスペリエンスの構築に適したメトリックです。 すべての要因について理解し、UI 要素のサイズを制御することによって、ユーザーのエクスペリエンスを適切なものにすることができます。

デバイスによって、有効ピクセルの幅の値が異なります。その範囲は、320 epx (最小のデバイス) から 1024 epx (一般的なサイズのモニター)、またはそれ以上のさらに広い幅になります。 これまでと同様に、自動的にサイズ調整される要素と動的レイアウト パネルを引き続き使うことで十分に対応できます。 ただし、場合によっては、UI 要素のプロパティを XAML マークアップで固定サイズに設定することがあります。 スケール ファクターは、アプリが実行されているデバイスやユーザーが行った表示設定に応じて、アプリに自動的に適用されます。 スケール ファクターによって、さまざまな画面サイズでユーザーに対してほぼ一定サイズのタッチ (または読み取り) ターゲットを提示するように、すべての UI 要素を固定サイズで維持できます。 また、動的レイアウトと共に使うと、UI は単にさまざまなデバイスで光学的なスケーリングを行うだけではありません。 利用可能な領域に対して適切な量のコンテンツを表示するために必要な処理も実行します。

すべてのディスプレイで最適なアプリのエクスペリエンスが実現できるように、一連のサイズで各ビットマップ アセットを作成し、各アセットが特定のスケール ファクターに適合するように設定することをお勧めします。 ただし、100% スケール、200% スケール、および 400% スケール (この優先順位で) でアセットを作成するほうが、多くの場合、すべての中間スケール ファクターで適切な結果を得ることができます。

**Note**  If, for whatever reason, you cannot create assets in more than one size, then create 100%-scale assets. Microsoft Visual Studio では、UWP アプリの既定のプロジェクト テンプレートには 1 つのサイズのみのブランド アセット (タイル イメージとロゴ) が用意されていますが、これらは 100% のスケールではありません。 独自のアプリのアセットを作成する場合は、このセクションに示したガイドラインに従って、100%、200%、400% のサイズを用意し、アセット パックを使います。

複雑なアートワークがある場合は、さらに多くのサイズに対応したアセットが必要になることがあります。 ベクター アートを使って作業を始める場合は、どのようなスケール ファクターでも高品質なアセットを比較的簡単に生成できます。

We don't recommend that you try to support all of the scale factors, but the full list of scale factors for Windows 10 apps is 100%, 125%, 150%, 200%, 250%, 300%, and 400%. すべてのスケール ファクターのアセットを提供した場合、ストアでは、各デバイスに合った適切なサイズのアセットが選ばれ、それらのアセットのみがダウンロードされます。 ストアでは、デバイスの DPI に基づいて、ダウンロードするアセットが選ばれます。 You can re-use assets from your Windows Runtime 8.x app at scale factors such as 140% and 220%, but your app will run at one of the new scale factors and so some bitmap scaling will be unavoidable. 状況に合った最適な結果を得ることができるかどうかを確認するために、さまざまなデバイスでアプリをテストしてください。

You may be re-using XAML markup from a Windows Runtime 8.x app where literal dimension values are used in the markup (perhaps to size shapes or other elements, perhaps for typography). But, in some cases, a larger scale factor is used on a device for a Windows 10 app than for a Universal 8.1 app (for example, 150% is used where 140% was before, and 200% is used where 180% was). So, if you find that these literal values are now too big on Windows 10, then try multiplying them by 0.8. 詳しくは、「[UWP アプリ用レスポンシブ デザイン 101](https://docs.microsoft.com/windows/uwp/layout/screen-sizes-and-breakpoints-for-responsive-design)」をご覧ください。

## <a name="gridview-and-listview-changes"></a>GridView と ListView の変更

コントロールを縦方向へスクロールするために、[**GridView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView) の既定の style setter に対していくつかの変更が行われました (横方向へのスクロールではありません。これについては既定で対応しています)。 プロジェクトに含まれている既定のスタイルのコピーを編集した場合、そのコピーにはこれらの変更が適用されていないため、手動で変更を加える必要があります。 一連の変更を以下にまとめます。

-   [  **ScrollViewer.HorizontalScrollBarVisibility**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.horizontalscrollbarvisibility) の setter は、**Auto** から **Disabled** に変更されました。
-   [  **ScrollViewer.VerticalScrollBarVisibility**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.verticalscrollbarvisibility) の setter は、**Disabled** から **Auto** に変更されました。
-   [  **ScrollViewer.HorizontalScrollMode**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.horizontalscrollmode) の setter は、**Enabled** から **Disabled** に変更されました。
-   [  **ScrollViewer.VerticalScrollMode**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.verticalscrollmode) の setter は、**Disabled** から **Enabled** に変更されました。
-   [  **ItemsPanel**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemspanel) の setter では、[**ItemsWrapGrid.Orientation**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemswrapgrid.orientation) の値が **Vertical** から **Horizontal** に変更されました。

最後の変更 (**Orientation** に対する変更) は矛盾していると考えられますが、ここでは折り返しグリッドについて説明していることに注意してください。 横方向の折り返しグリッド (新しい値) は、テキストが横方向に表示され、ページの終端で次の行に改行される書記体系と類似しています。 そのようなテキストのページでは、縦方向にスクロールします。 これに対して、縦方向の折り返しグリッド (以前の値) は、テキストが縦方向に表示される書記体系と類似しています。このため、横方向にスクロールします。

Here are the aspects of [**GridView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView) and [**ListView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) that have change or are not supported in Windows 10.

-   The [**IsSwipeEnabled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.isswipeenabled) property (Windows Runtime 8.x apps only) is not supported for Windows 10 apps. API は存在しますが、API を設定しても効果はありません。 これまでのすべての選択ジェスチャがサポートされていますが、下方向へのスワイプと右クリックはサポートされていません。これは、下方向へのスワイプではデータが検出不可能と示されるためです。また、右クリックはコンテキスト メニューの表示用に予約されているためです。
-   The [**ReorderMode**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.reordermode) property (Windows Phone Store apps only) is not supported for Windows 10 apps. API は存在しますが、API を設定しても効果はありません。 代わりに、**GridView** や **ListView** に対して [**AllowDrop**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.allowdrop) と [**CanReorderItems**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.canreorderitems) を true に設定することで、ユーザーは、長押し (またはクリックしてドラッグ) のジェスチャを使って順序を変更することができます。
-   When developing for Windows 10, use [**ListViewItemPresenter**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ListViewItemPresenter) instead of [**GridViewItemPresenter**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.GridViewItemPresenter) in your item container style, both for [**ListView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) and for [**GridView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView). アイテム コンテナーの既定のスタイルをコピーして編集する場合は、適切な種類のスタイルを取得できます。
-   The selection visuals have changed for a Windows 10 app. [  **SelectionMode**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.selectionmode) を **Multiple** に設定すると、既定では、各項目に対してチェック ボックスが表示されます。 **ListView** 項目の既定の設定では、チェック ボックスが項目の横にインラインで配置されます。その結果、他の項目が占める領域が若干小さくなり、移動されます。 **GridView** 項目の場合、既定では、チェック ボックスが項目の上部に重なって表示されます。 ただしどちらの場合も、チェック ボックスのレイアウト (インラインまたはオーバーレイ) を制御できます ([**CheckMode**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.checkmode) プロパティを使う)。また、アイテム コンテナーのスタイル内にある [**ListViewItemPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter) 要素上にチェック ボックスをすべて表示するかどうかを制御することもできます ([**SelectionCheckMarkVisualEnabled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.selectioncheckmarkvisualenabled) プロパティを使う)。次に例を示します。
-   In Windows 10, the [**ContainerContentChanging**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.containercontentchanging) event is raised twice per item during UI virtualization: once for the reclaim, and once for the re-use. [  **InRecycleQueue**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.containercontentchangingeventargs.inrecyclequeue) の値が **true** で、特別な回収操作がない場合、同じ項目が再利用されるとき (**InRecycleQueue** が **false** になる場合) には再入力されることが確実であれば、すぐにイベント ハンドラーを終了することができます。

```xml
<Style x:Key="CustomItemContainerStyle" TargetType="ListViewItem|GridViewItem">
    ...
    <Setter.Value>
        <ControlTemplate TargetType="ListViewItem|GridViewItem">
            <ListViewItemPresenter CheckMode="Inline|Overlay" ... />
        </ControlTemplate>
    </Setter.Value>
    ...
</Style>
```

![インライン チェック ボックスを使った ListViewItemPresenter](images/w8x-to-uwp-case-studies/ui-listviewbase-cb-inline.jpg)

インライン チェック ボックスを使った ListViewItemPresenter

![重ねて表示されるチェック ボックスを使った ListViewItemPresenter](images/w8x-to-uwp-case-studies/ui-listviewbase-cb-overlay.jpg)

重ねて表示されるチェック ボックスを使った ListViewItemPresenter

-   選択する際の下方向へのスワイプと右クリックのジェスチャがサポートされなくなったため (理由については上記のとおり)、相互作用モデルが変更されました。その結果の 1 つとして、[**ItemClick**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.itemclick) イベントと [**SelectionChanged**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) イベントは相互に排他的ではなくなりました。 For your Windows 10 app, review your scenarios and decide whether to adopt the "selection" or the "invoke" interaction model. 詳しくは、「[操作モードを変更する方法](https://docs.microsoft.com/previous-versions/windows/apps/hh780625(v=win.10))」をご覧ください。
-   [  **ListViewItemPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter) のスタイルを指定する際に使うプロパティについて、変更がいくつかあります。 新しいプロパティは、[**CheckBoxBrush**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.checkboxbrush)、[**PressedBackground**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.pressedbackground)、[**SelectedPressedBackground**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.selectedpressedbackground)、[**FocusSecondaryBorderBrush**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.focussecondaryborderbrush) です。 Properties that are ignored for a Windows 10 app are [**Padding**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.padding) (use [**ContentMargin**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.contentmargin) instead), [**CheckHintBrush**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.checkhintbrush), [**CheckSelectingBrush**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.checkselectingbrush), [**PointerOverBackgroundMargin**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.pointeroverbackgroundmargin), [**ReorderHintOffset**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.reorderhintoffset), [**SelectedBorderThickness**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.selectedborderthickness), and [**SelectedPointerOverBorderBrush**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.selectedpointeroverborderbrush).

次の表では、[**ListViewItem**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListViewItem) コントロール テンプレートと [**GridViewItem**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridViewItem) コントロール テンプレートでの表示状態や表示状態グループに対する変更について説明します。

| 8.1                 |                         | Windows 10        |                     |
|---------------------|-------------------------|-------------------|---------------------|
| CommonStates        |                         | CommonStates      |                     |
|                     | 正常                  |                   | 正常              |
|                     | PointerOver             |                   | PointerOver         |
|                     | Pressed                 |                   | Pressed             |
|                     | PointerOverPressed      |                   | [利用不可]       |
|                     | 無効                |                   | [利用不可]       |
|                     | [利用不可]           |                   | PointerOverSelected |
|                     | [利用不可]           |                   | Selected            |
|                     | [利用不可]           |                   | PressedSelected     |
| [利用不可]       |                         | DisabledStates    |                     |
|                     | [利用不可]           |                   | 無効            |
|                     | [利用不可]           |                   | 有効             |
| SelectionHintStates |                         | [利用不可]     |                     |
|                     | VerticalSelectionHint   |                   | [利用不可]       |
|                     | HorizontalSelectionHint |                   | [利用不可]       |
|                     | NoSelectionHint         |                   | [利用不可]       |
| [利用不可]       |                         | MultiSelectStates |                     |
|                     | [利用不可]           |                   | MultiSelectDisabled |
|                     | [利用不可]           |                   | MultiSelectEnabled  |
| SelectionStates     |                         | [利用不可]     |                     |
|                     | Unselecting             |                   | [利用不可]       |
|                     | Unselected              |                   | [利用不可]       |
|                     | UnselectedPointerOver   |                   | [利用不可]       |
|                     | UnselectedSwiping       |                   | [利用不可]       |
|                     | 選択               |                   | [利用不可]       |
|                     | Selected                |                   | [利用不可]       |
|                     | SelectedSwiping         |                   | [利用不可]       |
|                     | SelectedUnfocused       |                   | [利用不可]       |

カスタムの [**ListViewItem**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListViewItem) コントロール テンプレートまたは [**GridViewItem**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridViewItem) コントロール テンプレートを使っている場合は、上記の変更を踏まえてテンプレートを確認してください。 新しい既定のテンプレートのコピーを編集し、そのコピーにカスタマイズを再適用して、コントロール テンプレートのカスタマイズをやり直すことをお勧めします。 こうした作業をできない理由があり、既にあるテンプレートの編集が必要になる場合は、既存のテンプレートを編集する方法に関する次の一般的なガイダンスを参考にしてください。

-   新しい MultiSelectStates 表示状態グループを追加します。
-   新しい MultiSelectDisabled 表示状態を追加します。
-   新しい MultiSelectEnabled 表示状態を追加します。
-   新しい DisabledStates 表示状態グループを追加します。
-   新しい Enabled 表示状態を追加します。
-   CommonStates 表示状態グループから PointerOverPressed 表示状態を削除します。
-   Disabled 表示状態を DisabledStates 表示状態グループに移動します。
-   新しい PointerOverSelected 表示状態を追加します。
-   新しい PressedSelected 表示状態を追加します。
-   SelectedHintStates 表示状態グループを削除します。
-   SelectionStates 表示状態グループの Selected 表示状態を CommonStates 表示状態グループに移動します。
-   SelectionStates 表示状態グループ全体を削除します。

## <a name="localization-and-globalization"></a>ローカリゼーションとグローバリゼーション

UWP アプリ プロジェクトで、ユニバーサル 8.1 プロジェクトの Resources.resw ファイルを再利用できます。 このファイルをコピーしてから、プロジェクトに追加し、 **[ビルド アクション]** を **[PRIResource]** に、 **[出力ディレクトリにコピー]** を **[コピーしない]** に設定します。 「[**ResourceContext.QualifierValues**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.resources.core.resourcecontext.qualifiervalues)」トピックでは、デバイス ファミリのリソースを選ぶ要因に基づいてデバイス ファミリ固有のリソースを読み込む方法について説明しています。

## <a name="play-to"></a>リモート再生

The APIs in the [**Windows.Media.PlayTo**](https://docs.microsoft.com/uwp/api/Windows.Media.PlayTo) namespace are deprecated for Windows 10 apps in favor of the [**Windows.Media.Casting**](https://docs.microsoft.com/uwp/api/Windows.Media.Casting) APIs.

## <a name="resource-keys-and-textblock-style-sizes"></a>リソース キー、および TextBlock スタイル サイズ

The design language has evolved for Windows 10 and consequently certain system styles have changed. 場合によっては、変更されたスタイルのプロパティにビューが適合するように、ビューのビジュアル デザインに戻る必要があります。

それ以外の場合、リソース キーはサポートされなくなりました。 Visual Studio の XAML マークアップ エディターでは、解決できないリソース キーへの参照が強調表示されます。 たとえば XAML マークアップ エディターでは、スタイル キー `ListViewItemTextBlockStyle` への参照の下に赤い波線が引かれます。 これを修正しない場合、エミュレーターかデバイスに展開しようとしたときにアプリが直ちに終了します。 したがって、XAML マークアップの正確性に関する作業に着手することが重要です。 また、そのような問題を検出するために Visual Studio が優れたツールであることがわかります。

まだサポートされているキーに関して、デザイン言語の変更は、一部のスタイルによって設定されるプロパティが変更されたことを意味します。 For example, `TitleTextBlockStyle` sets **FontSize** to 14.667px in a Windows Runtime 8.x app and 18.14px in a Windows Phone Store app. But, the same style sets **FontSize** to a much larger 24px in a Windows 10 app. デザインとレイアウトを確認し、適切なスタイルを適切な場所で使ってください。 詳しくは、「[フォントのガイドライン](https://docs.microsoft.com/windows/uwp/controls-and-patterns/fonts)」と「[UWP アプリの設計](https://developer.microsoft.com/en-us/windows/apps/design)」をご覧ください。

サポートされなくなったキーの完全な一覧を次に示します。

-   CheckBoxAndRadioButtonMinWidthSize
-   CheckBoxAndRadioButtonTextPaddingThickness
-   ComboBoxFlyoutListPlaceholderTextOpacity
-   ComboBoxFlyoutListPlaceholderTextThemeMargin
-   ComboBoxHighlightedBackgroundThemeBrush
-   ComboBoxHighlightedBorderThemeBrush
-   ComboBoxHighlightedForegroundThemeBrush
-   ComboBoxInlinePlaceholderTextForegroundThemeBrush
-   ComboBoxInlinePlaceholderTextThemeFontWeight
-   ComboBoxItemDisabledThemeOpacity
-   ComboBoxItemHighContrastBackgroundThemeMargin
-   ComboBoxItemMinHeightThemeSize
-   ComboBoxPlaceholderTextBlockStyle
-   ComboBoxPlaceholderTextThemeMargin
-   CommandBarBackgroundThemeBrush
-   CommandBarForegroundThemeBrush
-   ContentDialogButton1HostPadding
-   ContentDialogButton2HostPadding
-   ContentDialogButtonsMinHeight
-   ContentDialogContentLandscapeWidth
-   ContentDialogContentMinHeight
-   ContentDialogDimmingColor
-   ContentDialogTitleMinHeight
-   ControlContextualInfoTextBlockStyle
-   ControlHeaderContentPresenterStyle
-   ControlHeaderTextBlockStyle
-   FlyoutContentPanelLandscapeThemeMargin
-   FlyoutContentPanelPortraitThemeMargin
-   GrabberMargin
-   GridViewItemMargin
-   GridViewItemPlaceholderBackgroundThemeBrush
-   GroupHeaderTextBlockStyle
-   HeaderContentPresenterStyle
-   HighContrastBlack
-   HighContrastWhite
-   HubHeaderCharacterSpacing
-   HubHeaderFontSize
-   HubHeaderMarginThickness
-   HubSectionHeaderCharacterSpacing
-   HubSectionHeaderFontSize
-   HubSectionHeaderMarginThickness
-   HubSectionMarginThickness
-   InlineWindowPlayPauseMargin
-   ItemTemplate
-   LeftFullWindowMargin
-   LeftMargin
-   ListViewEmptyStaticTextBlockStyle
-   ListViewItemContentTextBlockStyle
-   ListViewItemContentTranslateX
-   ListViewItemMargin
-   ListViewItemMultiselectCheckBoxMargin
-   ListViewItemSubheaderTextBlockStyle
-   ListViewItemTextBlockStyle
-   MediaControlPanelAudioThemeBrush
-   MediaControlPanelPhoneVideoThemeBrush
-   MediaControlPanelVideoThemeBrush
-   MediaControlPanelVideoThemeColor
-   MediaControlPlayPauseThemeBrush
-   MediaControlTimeRowThemeBrush
-   MediaControlTimeRowThemeColor
-   MediaDownloadProgressIndicatorThemeBrush
-   MediaErrorBackgroundThemeBrush
-   MediaTextThemeBrush
-   MenuFlyoutBackgroundThemeBrush
-   MenuFlyoutBorderThemeBrush
-   MenuFlyoutLandscapeThemePadding
-   MenuFlyoutLeftLandscapeBorderThemeThickness
-   MenuFlyoutPortraitBorderThemeThickness
-   MenuFlyoutPortraitThemePadding
-   MenuFlyoutRightLandscapeBorderThemeThickness
-   MessageDialogContentStyle
-   MessageDialogTitleStyle
-   MinimalWindowMargin
-   PasswordBoxCheckBoxThemeMargin
-   PhoneAccentBrush
-   PhoneBackgroundBrush
-   PhoneBackgroundColor
-   PhoneBaseBlackColor
-   PhoneBaseHighColor
-   PhoneBaseLowColor
-   PhoneBaseLowSolidColor
-   PhoneBaseMediumHighColor
-   PhoneBaseMediumMidColor
-   PhoneBaseMediumMidSolidColor
-   PhoneBaseMidColor
-   PhoneBaseWhiteColor
-   PhoneBorderThickness
-   PhoneButtonBasePressedForegroundBrush
-   PhoneButtonContentPadding
-   PhoneButtonFontWeight
-   PhoneButtonMinHeight
-   PhoneButtonMinWidth
-   PhoneChromeBrush
-   PhoneChromeColor
-   PhoneControlBackgroundColor
-   PhoneControlDisabledColor
-   PhoneControlForegroundColor
-   PhoneDisabledBrush
-   PhoneDisabledColor
-   PhoneFontFamilyLight
-   PhoneFontFamilySemiBold
-   PhoneForegroundBrush
-   PhoneForegroundColor
-   PhoneHighContrastSelectedBackgroundThemeBrush
-   PhoneHighContrastSelectedForegroundThemeBrush
-   PhoneImagePlaceholderColor
-   PhoneLowBrush
-   PhoneMidBrush
-   PhonePageBackgroundColor
-   PhonePivotLockedTranslation
-   PhonePivotUnselectedItemOpacity
-   PhoneRadioCheckBoxBorderBrush
-   PhoneRadioCheckBoxBrush
-   PhoneRadioCheckBoxCheckBrush
-   PhoneRadioCheckBoxPressedBrush
-   PhoneStrokeThickness
-   PhoneTextHighColor
-   PhoneTextLowColor
-   PhoneTextMidColor
-   PhoneTextOverAccentColor
-   PhoneTouchTargetLargeOverhang
-   PhoneTouchTargetOverhang
-   PivotHeaderItemPadding
-   PlaceholderContentPresenterStyle
-   ProgressBarHighContrastAccentBarThemeBrush
-   ProgressBarIndeterminateRectagleThemeSize
-   ProgressBarRectangleStyle
-   ProgressRingActiveBackgroundOpacity
-   ProgressRingElipseThemeMargin
-   ProgressRingElipseThemeSize
-   ProgressRingTextForegroundThemeBrush
-   ProgressRingTextThemeMargin
-   ProgressRingThemeSize
-   RichEditBoxTextThemeMargin
-   RightFullWindowMargin
-   RightMargin
-   ScrollBarMinThemeHeight
-   ScrollBarMinThemeWidth
-   ScrollBarPanningThumbThemeHeight
-   ScrollBarPanningThumbThemeWidth
-   SliderThumbDisabledBorderThemeBrush
-   SliderTrackBorderThemeBrush
-   SliderTrackDisabledBorderThemeBrush
-   TextBoxBackgroundColor
-   TextBoxBorderColor
-   TextBoxDisabledHeaderForegroundThemeBrush
-   TextBoxFocusedBackgroundThemeBrush
-   TextBoxForegroundColor
-   TextBoxPlaceholderColor
-   TextControlHeaderMarginThemeThickness
-   TextControlHeaderMinHeightSize
-   TextStyleExtraExtraLargeFontSize
-   TextStyleExtraLargePlusFontSize
-   TextStyleMediumFontSize
-   TextStyleSmallFontSize
-   TimeRemainingElementMargin

## <a name="searchbox-deprecated-in-favor-of-autosuggestbox"></a>SearchBox に代わって使われる AutoSuggestBox

ユニバーサル デバイス ファミリでは [**SearchBox**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.searchbox) が実装されていますが、モバイル デバイスでは部分的に機能しません。 ユニバーサル検索エクスペリエンスには [**AutoSuggestBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AutoSuggestBox) を使います。 **AutoSuggestBox** を使った検索エクスペリエンスの通常の実装方法を次に示します。

入力を開始すると、**UserInput** という理由で **TextChanged** イベントが発生します。 次に、候補の一覧を設定し、[**AutoSuggestBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AutoSuggestBox) の **ItemsSource** を設定します。 ユーザーが一覧を移動すると、**SuggestionChosen** イベントが発生します (また、**TextMemberDisplayPath** を設定すると、指定されたプロパティに基づいてテキスト ボックスが自動的に入力されます)。 ユーザーが Enter キーで選択項目を送信すると、**QuerySubmitted** イベントが発生し、その時点で、選ばれた候補に対するアクションを実行できます (多くの場合、指定されたコンテンツの詳細を示す別のページに移動するというアクションが実行されます)。 **SearchBoxQuerySubmittedEventArgs** の **LinguisticDetails** プロパティと **Language** プロパティはサポートされなくなった点に注意してください (その機能をサポートする同等の API はあります)。 また、**KeyModifiers** もサポートされなくなりました。

[**AutoSuggestBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AutoSuggestBox) also has support for input method editors (IMEs). 必要に応じて "検索" アイコンを表示できます (このアイコンを操作すると **QuerySubmitted** イベントが発生します)。

```xml
   <AutoSuggestBox ... >
        <AutoSuggestBox.QueryIcon>
            <SymbolIcon Symbol="Find"/>
        </AutoSuggestBox.QueryIcon>
    </AutoSuggestBox>
```

[AutoSuggestBox の移植のサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlAutoSuggestBox)に関するページもご覧ください。

## <a name="semanticzoom-changes"></a>SemanticZoom に関する変更

[  **SemanticZoom**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) での縮小表示のジェスチャは、Windows Phone モデルで収束され、グループ ヘッダーをタップまたはクリックするという動作になりました (このため、デスクトップ コンピューターでは、縮小表示するためのマイナス記号のボタンのアフォーダンスは表示されなくなります)。 これで、デバイスに関係なく同一で一貫性のある動作を実現できます。 Windows Phone モデルと外観面で異なる点の 1 つは、縮小表示ビュー (ジャンプ リスト) が、拡大表示ビューをオーバーレイするのではなく、拡大表示ビューを置き換えることです。 このため、縮小表示ビューから半透明の背景を削除することができます。

In a Windows Phone Store app, the zoomed-out view expands to the size of the screen. In a Windows Runtime 8.x app, and in a Windows 10 app, the size of the zoomed-out view is constrained to the bounds of the **SemanticZoom** control.

Windows Phone ストア アプリでは、縮小表示ビューで背景に透明度が設定されている場合、そのビューの (Z オーダーに基づく) 背後のコンテンツが透けて表示されます。 In a Windows Runtime 8.x app, and in a Windows 10 app, nothing is visible behind the zoomed out view.

In a Windows Runtime 8.x app, when the app is deactivated and reactivated, the zoomed-out view is dismissed (if it was being shown) and the zoomed-in view is shown instead. In a Windows Phone Store app, and in a Windows 10 app, the zoomed-out view will remain showing if it was being shown.

In a Windows Phone Store app, and in a Windows 10 app, the zoomed-out view is dismissed when the back button is pressed. For a Windows Runtime 8.x app, there is no built-in back button processing, so the question doesn't apply.

## <a name="settings"></a>設定

The Windows Runtime 8.x **SettingsPane** class is not appropriate for Windows 10. 代わりに、[設定] ページを作成し、さらに、アプリ内から [設定] ページにアクセスする方法をユーザーに提供する必要があります。 このアプリの [設定] ページはトップ レベルで表示されるようにすることをお勧めしますが、ナビゲーション ウィンドウの最後のピン留めされた項目として、ここにはオプションの完全なセットがあります。

-   ナビゲーション ウィンドウ。 設定は選択肢のナビゲーション リストの最後の項目であり、下部にピン留めしている必要があります。
-   アプリ バー/ツール バー (タブ ビューまたはピボット レイアウト内)。 [設定] はアプリ バーやツール バーのメニューのポップアップで最後の項目であることが必要です。 [設定] をナビゲーション内のトップレベルのいずれかの項目にすることはお勧めしません。
-   ハブ。 [設定] はメニューのポップアップ内に配置する必要があります (ハブ レイアウトでのアプリ バー メニューやツール バー メニューからのポップアップ内など)。

また、マスター詳細ウィンドウ内に [設定] を配置することはお勧めしません。

[設定] ページはアプリのウィンドウ全体を埋め、このページには [バージョン情報] と [フィードバック] があることも必要です。 [設定] ページのデザインに関するガイダンスについては、「[アプリ設定のガイドライン](https://docs.microsoft.com/windows/uwp/app-settings/guidelines-for-app-settings)」をご覧ください。

## <a name="text"></a>テキスト

テキスト (または文字体裁) は UWP アプリの重要な要素です。移植するときには、ビューの視覚的なデザインが新しいデザイン言語に適合するように、ビューの視覚的なデザインを再検討することが必要になる場合があります。 次の図を使って、ユニバーサル Windows プラットフォーム (UWP) の利用可能な  **TextBlock** システム スタイルを見つけてください。 Find the ones that correspond to the Windows Phone Silverlight styles you used. Alternatively, you can create your own universal styles and copy the properties from the Windows Phone Silverlight system styles into those.

![Windows 10 アプリのシステム TextBlock スタイル](images/label-uwp10stylegallery.png) <br/>System TextBlock styles for Windows 10 apps

In Windows Runtime 8.x apps and Windows Phone Store apps, the default font family is Global User Interface. In a Windows 10 app, the default font family is Segoe UI. この結果、アプリでのフォント メトリックの表示が異なる可能性があります。 8\.1 のテキストの外観を再現する場合は、[**LineHeight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.lineheight) や [**LineStackingStrategy**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.linestackingstrategy) などのプロパティを使って、独自のメトリックを設定できます。

In Windows Runtime 8.x apps and Windows Phone Store apps, the default language for text is set to the language of the build, or to en-us. In a Windows 10 app, the default language is set to the top app language (font fallback). [  **FrameworkElement.Language**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.language) を明示的に設定できますが、そのプロパティの値を設定しないと、フォントの優れたフォールバック動作を得ることができます。

詳しくは、「[フォントのガイドライン](https://docs.microsoft.com/windows/uwp/controls-and-patterns/fonts)」と「[UWP アプリの設計](https://developer.microsoft.com/)」をご覧ください。 テキスト コントロールの変更については、上記の「[コントロール](#controls-and-control-styles-and-templates)」セクションもご覧ください。

## <a name="theme-changes"></a>テーマの変更

ユニバーサル 8.1 アプリでは、既定のテーマは濃色になっています。 For Windows 10 devices, the default theme has changed, but you can control the theme used by declaring a requested theme in App.xaml. たとえば、すべてのデバイスで濃色テーマを使うには、`RequestedTheme="Dark"` をルートの Application 要素に追加します。

## <a name="tiles-and-toasts"></a>タイルとトースト

For tiles and toasts, the templates you're currently using will continue to work in your Windows 10 app. ただし、新しいアダプティブ テンプレートを使うことができ、それらについては「 [通知、タイル、トースト、バッジ](https://docs.microsoft.com/windows/uwp/controls-and-patterns/tiles-badges-notifications)」で説明されています。

以前、デスクトップ コンピューターでは、トースト通知は一時的なメッセージでした。 表示が消えるため、見逃したり無視した場合はもう取得できなくなりました。 Windows phone では、トースト通知が無視されたり一時的に消された場合、通知はアクション センターに移されます。 アクション センターは、モバイル デバイス ファミリに限定されたものではなくなりました。

トースト通知を送るために機能を宣言する必要はなくなりました。

## <a name="window-size"></a>ウィンドウ サイズ

ユニバーサル 8.1 アプリでは、アプリ マニフェストの要素 [**ApplicationView**](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema2013/element-applicationview) を使って、ウィンドウの最小幅が宣言されます。 UWP アプリでは、命令型コードを使って最小サイズ (幅と高さ) を指定できます。 既定の最小サイズは 500 x 320 epx で、このサイズは受け入れられる最も小さいサイズでもあります。 受け入れられる最も大きいサイズは 500 x 500 epx です。

```csharp
   Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().SetPreferredMinSize
        (new Size { Width = 500, Height = 500 });
```

次のトピックは、「[入出力、デバイス、アプリ モデルの移植](w8x-to-uwp-input-and-sensors.md)」です。

