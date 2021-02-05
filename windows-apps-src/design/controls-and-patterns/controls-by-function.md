---
description: アプリで使うことができる一部のコントロールの機能別の一覧を示します。
title: 機能別コントロール
ms.assetid: 8DB4347B-91D6-4659-91F2-80ECF7BBB596
label: Controls by function
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c84595a8d5aadfb22f84a5fe780e93b9518490dc
ms.sourcegitcommit: 382ae62f9d9bf980399a3f654e40ef4f85eae328
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/04/2021
ms.locfileid: "99534481"
---
# <a name="controls-by-function"></a>機能別コントロール

Windows の XAML UI フレームワークには、UI 開発をサポートする多くのコントロールのライブラリが用意されています。 これらのコントロールの一部は視覚的に表示されますが、それ以外のコントロールは他のコントロールまたはコンテンツ (画像やメディアなど) のコンテナーとして機能します。 

[XAML UI の基本のサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics)をダウンロードすると、Windows UI コントロールの多くを実際に見ることができます。

<table>
<th align="left">XAML コントロール ギャラリー<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML コントロール ギャラリー</strong> アプリがインストールされている場合、こちらをクリックして<a href="xamlcontrolsgallery:/item/NavigationView">アプリを開き、NavigationView の動作を確認</a>してください。 </p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML コントロール ギャラリー アプリを入手する (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">ソース コード (GitHub) を入手する</a></li>
    </ul>
</td>
</tr>
</table>


アプリで使うことができる一般的な XAML コントロールの機能別の一覧を以下に示します。

## <a name="appbars-and-commands"></a>アプリ バーとコマンド

### <a name="app-bar"></a>アプリ バー
アプリ特有のコマンドを表示するツール バー。 「[コマンド バー](#command-bar)」をご覧ください。

参照 :[AppBar](/uwp/api/Windows.UI.Xaml.Controls.AppBar) 

### <a name="app-bar-button"></a>アプリ バーのボタン
アプリ バー スタイルを使ってコマンドを表示するボタン。

![アプリ バーのボタン アイコン](images/controls/app-bar-buttons.png) 

参照 :[AppBarButton](/uwp/api/Windows.UI.Xaml.Controls.AppBarButton)、[SymbolIcon](/uwp/api/Windows.UI.Xaml.Controls.SymbolIcon)、[BitmapIcon](/uwp/api/Windows.UI.Xaml.Controls.BitmapIcon)、[FontIcon](/uwp/api/Windows.UI.Xaml.Controls.FontIcon)、[PathIcon](/uwp/api/Windows.UI.Xaml.Controls.PathIcon) 

デザインと使い方: [アプリ バーとコマンド バーのコントロールのガイド](app-bars.md) 

サンプル コード: [XAML コマンド実行のサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlCommanding)

### <a name="app-bar-separator"></a>アプリ バーの区分線
コマンド バーのコマンドのグループを視覚的に区切ります。

参照 :[AppBarSeparator](/uwp/api/Windows.UI.Xaml.Controls.AppBarSeparator) 

サンプル コード: [XAML コマンド実行のサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlCommanding)

### <a name="app-bar-toggle-button"></a>アプリ バーのトグル ボタン
コマンド バーでコマンドを切り替えるボタン。

参照 :[AppBarToggleButton](/uwp/api/Windows.UI.Xaml.Controls.AppBarToggleButton) 

サンプル コード: [XAML コマンド実行のサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlCommanding)

### <a name="command-bar"></a>コマンド バー
アプリ バーのボタン要素のサイズ変更を処理する専用のアプリ バー。

![コマンド バー コントロール](images/command-bar-compact.png)

```xaml
<CommandBar>
    <AppBarButton Icon="Back" Label="Back" Click="AppBarButton_Click"/>
    <AppBarButton Icon="Stop" Label="Stop" Click="AppBarButton_Click"/>
    <AppBarButton Icon="Play" Label="Play" Click="AppBarButton_Click"/>
</CommandBar>
```
参照 :[CommandBar](/uwp/api/Windows.UI.Xaml.Controls.CommandBar) 

デザインと使い方: [アプリ バーとコマンド バーのコントロールのガイド](app-bars.md)

サンプル コード: [XAML コマンド実行のサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlCommanding)

## <a name="buttons"></a>ボタン

### <a name="button"></a>ボタン
ユーザーの入力に応答して **Click** イベントを発生させるコントロール。

![標準的なボタン](images/controls/button.png)

```xaml
<Button x:Name="button1" Content="Button" 
        Click="Button_Click" />
```

参照 :[ボタン](/uwp/api/Windows.UI.Xaml.Controls.Button) 

デザインと使い方: [ボタン コントロールのガイド](buttons.md) 

### <a name="hyperlink"></a>ハイパーリンク
「[ハイパーリンク ボタン](#hyperlink-button)」をご覧ください。

### <a name="hyperlink-button"></a>ハイパーリンク ボタン
ブラウザーでマークアップ テキストとして表示され、指定された URI を開くボタンです。

![ハイパーリンク ボタン](images/controls/hyperlink-button.png)

```xaml
<HyperlinkButton Content="www.microsoft.com" 
                 NavigateUri="https://www.microsoft.com"/>
```

参照 :[HyperlinkButton](/uwp/api/Windows.UI.Xaml.Controls.HyperlinkButton) 

デザインと使い方: [ハイパーリンク コントロールのガイド](hyperlinks.md)

### <a name="repeat-button"></a>繰り返しボタン
押されたときから離されるまでの間、繰り返し **Click** イベントを発生させるボタン。 

![繰り返しボタン コントロール](images/controls/repeat-button.png) 

```xaml
<RepeatButton x:Name="repeatButton1" Content="Repeat Button" 
              Click="RepeatButton_Click" />
```

参照 :[RepeatButton](/uwp/api/Windows.UI.Xaml.Controls.Primitives.RepeatButton) 

デザインと使い方: [ボタン コントロールのガイド](buttons.md) 

## <a name="collectiondata-controls"></a>コレクション コントロールとデータ コントロール

### <a name="flip-view"></a>フリップ ビュー
ユーザーが 1 つずつめくって表示することができる項目のコレクションを表示するコントロール。

```xaml
<FlipView x:Name="flipView1" SelectionChanged="FlipView_SelectionChanged">
    <Image Source="Assets/Logo.png" />
    <Image Source="Assets/SplashScreen.png" />
    <Image Source="Assets/SmallLogo.png" />
</FlipView>
```

参照 :[FlipView](/uwp/api/Windows.UI.Xaml.Controls.FlipView) 

デザインと使い方: [FlipView コントロール](flipview.md) 

### <a name="grid-view"></a>グリッド ビュー
縦方向にスクロールできる複数行と複数列で項目のコレクションを表示するコントロール。

```xaml
<GridView x:Name="gridView1" SelectionChanged="GridView_SelectionChanged">
    <x:String>Item 1</x:String>
    <x:String>Item 2</x:String>
</GridView>
```

参照 :[GridView](/uwp/api/Windows.UI.Xaml.Controls.GridView) 

デザインと使い方: [リスト](lists.md) 

サンプル コード: [ListView のサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlListView)

### <a name="items-control"></a>項目コントロール
データ テンプレートで指定された UI にある項目のコレクションを表示するコントロール。 

```xaml
<ItemsControl/>
```

参照 :[ItemsControl](/uwp/api/Windows.UI.Xaml.Controls.ItemsControl) 

### <a name="list-view"></a>リスト ビュー
縦方向にスクロールできるリストで項目のコレクションを表示するコントロール。

```xaml
<ListView x:Name="listView1" SelectionChanged="ListView_SelectionChanged">
    <x:String>Item 1</x:String>
    <x:String>Item 2</x:String>
</ListView>
```

参照 :[ListView](/uwp/api/Windows.UI.Xaml.Controls.ListView) 

デザインと使い方: [リスト](lists.md) 

サンプル コード: [ListView のサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlListView)

## <a name="date-and-time-controls"></a>日付と時刻コントロール

### <a name="calendar-date-picker"></a>カレンダーの日付の選択コントロール
ドロップダウン カレンダー表示を使って、ユーザーが日付を選択できるコントロール。

![カレンダー ビューが開いたカレンダーの日付の選択コントロール](images/controls/calendar-date-picker-open.png)

```xaml
<CalendarDatePicker/>
```

参照 :[CalendarDatePicker](/uwp/api/Windows.UI.Xaml.Controls.CalendarDatePicker) 

デザインと使い方: [カレンダー、日付、および時刻コントロール](date-and-time.md)
 
### <a name="calendar-view"></a>カレンダー ビュー
ユーザーが 1 つまたは複数の日付を選択できる、構成可能なカレンダー表示。

```xaml
<CalendarView/>
```

参照 :[CalendarView](/uwp/api/Windows.UI.Xaml.Controls.CalendarView) 

デザインと使い方: [カレンダー、日付、および時刻コントロール](date-and-time.md) 

### <a name="date-picker"></a>日付の選択コントロール
ユーザーが日付を選択できるコントロール。

![日付の選択コントロール](images/controls_datepicker_expand.png)

```xaml
<DatePicker Header="Arrival Date"/>
```

参照 :[DatePicker](/uwp/api/Windows.UI.Xaml.Controls.DatePicker) 

デザインと使い方: [カレンダー、日付、および時刻コントロール](date-and-time.md)
 
### <a name="time-picker"></a>時刻の選択コントロール
ユーザーが時間値を設定できるコントロール。

![TimePicker コントロール](images/controls_timepicker_expand.png)

```xaml
<TimePicker Header="Arrival Time"/>
```

参照 :[TimePicker](/uwp/api/Windows.UI.Xaml.Controls.TimePicker) 

デザインと使い方: [カレンダー、日付、および時刻コントロール](date-and-time.md)

## <a name="flyouts"></a>ポップアップ

### <a name="context-menu"></a>ショートカット メニュー
「[メニュー ポップアップ](#menu-flyout)」および「[ポップアップ メニュー](#popup-menu)」をご覧ください。

### <a name="flyout"></a>ポップアップ
ユーザーの操作が必要であることを示すメッセージを表示します (ダイアログでは他のユーザー操作がブロックされますが、ポップアップでは別のウィンドウが作成されず、操作もブロックされません)。

![ポップアップ コントロール](images/controls/flyout.png)

```xaml
<Flyout>
    <StackPanel>
        <TextBlock Text="All items will be removed. Do you want to continue?"/>
        <Button Click="DeleteConfirmation_Click" Content="Yes, empty my cart"/>
    </StackPanel>
</Flyout>
```

参照 :[Flyout](/uwp/api/Windows.UI.Xaml.Controls.Flyout) 

デザインと使い方: [ポップアップ](dialogs-and-flyouts/flyouts.md) 

### <a name="menu-flyout"></a>メニュー ポップアップ
ユーザーが現在行っている内容に関連するコマンドまたはオプションの一覧を一時的に表示します。

![メニュー ポップアップ コントロール](images/controls/menu-flyout.png) 

```xaml
<MenuFlyout>
    <MenuFlyoutItem Text="Reset" Click="Reset_Click"/>
    <MenuFlyoutSeparator/>
    <ToggleMenuFlyoutItem Text="Shuffle" 
                          IsChecked="{Binding IsShuffleEnabled, Mode=TwoWay}"/>
    <ToggleMenuFlyoutItem Text="Repeat" 
                          IsChecked="{Binding IsRepeatEnabled, Mode=TwoWay}"/>
</MenuFlyout>
```

参照 :[MenuFlyout](/uwp/api/Windows.UI.Xaml.Controls.MenuFlyout)、[MenuFlyoutItem](/uwp/api/Windows.UI.Xaml.Controls.MenuFlyoutItem)、[MenuFlyoutSeparator](/uwp/api/Windows.UI.Xaml.Controls.MenuFlyoutSeparator)、[ToggleMenuFlyoutItem](/uwp/api/Windows.UI.Xaml.Controls.ToggleMenuFlyoutItem) 

デザインと使い方: [メニューとショートカット メニュー](menus.md) 

サンプル コード: [XAML コンテキスト メニューのサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlContextMenu)

### <a name="popup-menu"></a>ポップアップ メニュー
指定したコマンドを表示するためのカスタム メニュー。

参照 :[PopupMenu](/uwp/api/Windows.UI.Popups.PopupMenu) 

デザインと使い方: [ダイアログ](dialogs-and-flyouts/dialogs.md) 

### <a name="tooltip"></a>ヒント
要素についての情報を表示するポップアップ ウィンドウ。 
 
![ヒント コントロール](images/controls/tool-tip.png)

```xaml
<Button Content="Button" Click="Button_Click" 
        ToolTipService.ToolTip="Click to perform action" />
```

参照 :[ToolTip](/uwp/api/Windows.UI.Xaml.Controls.ToolTip)、[ToolTipService](/uwp/api/Windows.UI.Xaml.Controls.ToolTipService) 

デザインと使い方: ヒントのガイドライン 

## <a name="images"></a>イメージ

### <a name="image"></a>イメージ
画像を表示するコントロール。

```xaml
<Image Source="Assets/Logo.png" />
```

参照 :[画像](/uwp/api/Windows.UI.Xaml.Controls.Image) 

デザインと使い方: [Image と ImageBrush](images-imagebrushes.md) 

サンプル コード: [XAML コントロール ギャラリー](/samples/microsoft/xaml-controls-gallery/xaml-controls-gallery/)

## <a name="graphics-and-ink"></a>グラフィックスとインク

### <a name="inkcanvas"></a>InkCanvas
インク ストロークを受信し、表示するコントロール。

```xaml
<InkCanvas/>
```

参照 :[InkCanvas](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) 

### <a name="shapes"></a>図形
楕円形、四角形、直線、ベジエ パスなどのように表示できる、保持モードの各種グラフィック オブジェクト。

![多角形](images/controls/shapes-polygon.png) 
![パス](images/controls/shapes-path.png) 

```xaml
<Ellipse/>
<Path/>
<Rectangle/>
```

参照 :[図形](/uwp/api/Windows.UI.Xaml.Shapes.Shape) 

手順[図形の描画](./shapes.md) 

サンプル コード: [XAML ベクターベース描画のサンプル](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/XAML%20vector-based%20drawing%20sample%20(Windows%208))

## <a name="layout-controls"></a>レイアウト コントロール

### <a name="border"></a>境界線
境界線、背景、またはこの両方を他のオブジェクトの周囲に描画するコンテナー コントロール。

![2 つの四角形の周囲の境界線](images/controls/border.png) 

```xaml
<Border BorderBrush="Blue" BorderThickness="4" 
        Height="108" Width="64" 
        Padding="8" CornerRadius="4">
    <Canvas>
        <Rectangle Fill="Orange"/>
        <Rectangle Fill="Green" Margin="0,44"/>
    </Canvas>
</Border>
```

参照 :[Border](/uwp/api/Windows.UI.Xaml.Controls.Border)

### <a name="canvas"></a>キャンバス
キャンバスの左上隅を基準とする子要素の絶対配置をサポートするレイアウト パネル。
 
![キャンバス レイアウト パネル](images/controls/canvas.png) 

```xaml
<Canvas Width="120" Height="120">
    <Rectangle Fill="Red"/>
    <Rectangle Fill="Blue" Canvas.Left="20" Canvas.Top="20"/>
    <Rectangle Fill="Green" Canvas.Left="40" Canvas.Top="40"/>
    <Rectangle Fill="Orange" Canvas.Left="60" Canvas.Top="60"/>
</Canvas>
```

参照 :[Canvas](/uwp/api/Windows.UI.Xaml.Controls.Canvas)
 
### <a name="grid"></a>グリッド
複数行と複数列での子要素の配置をサポートするレイアウト パネル。

![グリッド レイアウト パネル](images/controls/grid.png) 

```xaml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="50"/>
        <RowDefinition Height="50"/>
    </Grid.RowDefinitions>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="50"/>
        <ColumnDefinition Width="50"/>
    </Grid.ColumnDefinitions>
    <Rectangle Fill="Red"/>
    <Rectangle Fill="Blue" Grid.Row="1"/>
    <Rectangle Fill="Green" Grid.Column="1"/>
    <Rectangle Fill="Orange" Grid.Row="1" Grid.Column="1"/>
</Grid>
```

参照 :[Grid](/uwp/api/Windows.UI.Xaml.Controls.Grid)
 
### <a name="panning-scroll-viewer"></a>パン スクロール ビューアー
「[スクロール ビューアー](#scroll-viewer)」をご覧ください。

### <a name="relativepanel"></a>RelativePanel
互いまたは親パネルを基準にして、子オブジェクトの位置を決定し、調整できるパネル。

![RelativePanel レイアウト パネル](images/controls/relative-panel.png) 

```xaml
<RelativePanel>
    <TextBox x:Name="textBox1" RelativePanel.AlignLeftWithPanel="True"/>
    <Button Content="Submit" RelativePanel.Below="textBox1"/>
</RelativePanel>
```

参照 :[RelativePanel](/uwp/api/Windows.UI.Xaml.Controls.RelativePanel)

### <a name="scroll-bar"></a>スクロール バー
「[スクロール ビューアー](#scroll-viewer)」をご覧ください。 (ScrollBar は ScrollViewer の要素です。 通常、スタンドアロン コントロールとしては使用しません)。

参照 :[ScrollBar](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ScrollBar)
 
### <a name="scroll-viewer"></a>スクロール ビューアー
ユーザーが、コンテンツのパンとズームを実行できるようにするコンテナー コントロール。

```xaml
<ScrollViewer ZoomMode="Enabled" MaxZoomFactor="10" 
              HorizontalScrollMode="Enabled" 
              HorizontalScrollBarVisibility="Visible"
              Height="200" Width="200">
    <Image Source="Assets/Logo.png" Height="400" Width="400"/>
</ScrollViewer>
```

参照 :[ScrollViewer](/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer)

デザインと使い方: [スクロール コントロールとパン コントロールのガイド](scroll-controls.md) 

サンプル コード: [XAML のスクロール、パン、ズームのサンプル](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/XAML%20scrolling%2C%20panning%2C%20and%20zooming%20sample%20(Windows%208))

### <a name="stack-panel"></a>スタック パネル
子要素を水平方向または垂直方向の単一行に配置するレイアウト パネル。

![スタック パネル レイアウト コントロール](images/controls/stack-panel.png) 

```xaml
<StackPanel>
    <Rectangle Fill="Red"/>
    <Rectangle Fill="Blue"/>
    <Rectangle Fill="Green"/>
    <Rectangle Fill="Orange"/>
</StackPanel>
```

参照 :[StackPanel](/uwp/api/Windows.UI.Xaml.Controls.StackPanel)
 
### <a name="variablesizedwrapgrid"></a>VariableSizedWrapGrid
複数行と複数列での子要素の配置をサポートするレイアウト パネル。 各子要素を、複数の行と列に配置できます。

![可変サイズ折り返しグリッド レイアウト パネル](images/controls/variable-sized-wrap-grid.png) 

```xaml
<VariableSizedWrapGrid MaximumRowsOrColumns="3" ItemHeight="44" ItemWidth="44">
    <Rectangle Fill="Red"/>
    <Rectangle Fill="Blue" Height="80" 
               VariableSizedWrapGrid.RowSpan="2"/>
    <Rectangle Fill="Green" Width="80" 
               VariableSizedWrapGrid.ColumnSpan="2"/>
    <Rectangle Fill="Orange" Height="80" Width="80" 
               VariableSizedWrapGrid.RowSpan="2" 
               VariableSizedWrapGrid.ColumnSpan="2"/>
</VariableSizedWrapGrid>
```

参照 :[VariableSizedWrapGrid](/uwp/api/Windows.UI.Xaml.Controls.VariableSizedWrapGrid)

### <a name="viewbox"></a>Viewbox
コンテンツを指定されたサイズに拡大縮小するコンテナー コントロール。

![Viewbox コントロール](images/controls/view-box.png) 

```xaml
<Viewbox MaxWidth="25" MaxHeight="25">
    <Image Source="Assets/Logo.png"/>
</Viewbox>
<Viewbox MaxWidth="75" MaxHeight="75">
    <Image Source="Assets/Logo.png"/>
</Viewbox>
<Viewbox MaxWidth="150" MaxHeight="150">
    <Image Source="Assets/Logo.png"/>
</Viewbox>
```

参照 :[Viewbox](/uwp/api/Windows.UI.Xaml.Controls.Viewbox)
 
### <a name="zooming-scroll-viewer"></a>ズーム スクロール ビューアー
「[スクロール ビューアー](#scroll-viewer)」をご覧ください。

## <a name="media-controls"></a>メディア コントロール

### <a name="audio"></a>オーディオ

「[メディア プレーヤーの要素](#media-player-element)」をご覧ください。

### <a name="media-player-element"></a>メディア プレーヤーの要素

オーディオとビデオのコンテンツを再生するコントロール。

```xaml
<MediaPlayerElement x:Name="myMediaPlayerElement"/>
```

> [!Important]
> **MediaPlayerElement** は Windows 10 バージョン 1607 以降でのみ使用できます。 Windows 10 の以前のバージョン用にアプリを開発する場合は、代わりに [MediaElement](/uwp/api/Windows.UI.Xaml.Controls.MediaElement) コントロールを使用する必要があります。

参照 :[MediaPlayerElement](/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement)

デザインと使い方: [メディア再生](media-playback.md)

### <a name="mediatransportcontrols"></a>MediaTransportControls
MediaElement の再生コントロールを提供するコントロール。

![トランスポート コントロールを含むメディア要素](images/controls/media-transport-controls.png) 

```xaml
<MediaTransportControls MediaElement="myMediaElement"/>
```

参照 :[MediaTransportControls](/uwp/api/Windows.UI.Xaml.Controls.MediaTransportControls) 

デザインと使い方: [メディア再生](media-playback.md)

サンプル コード: [メディア トランスポート コントロールのサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlCustomMediaTransportControls)

### <a name="video"></a>ビデオ
「[メディア プレーヤーの要素](#media-player-element)」をご覧ください。

## <a name="navigation"></a>［ナビゲーション］

### <a name="navigationview"></a>NavigationView

左側のナビゲーション ウィンドウ、上部のナビゲーション、およびタブのパターンを実装する、適応性のあるコンテナーと柔軟なナビゲーションのモデルです。

参照 :[NavigationView](/uwp/api/windows.ui.xaml.controls.navigationview)

デザインと使い方: [NavigationView コントロールのガイド](navigationview.md)

### <a name="splitview"></a>SplitView

2 つのビューを持つコンテナー コントロール。1 つはメイン コンテンツ用で、もう 1 つは、通常、ナビゲーション メニューに使います。

![分割ビュー コントロール](images/controls/split-view.png) 

```xaml
<SplitView>
    <SplitView.Pane>
        <!-- Menu content -->
    </SplitView.Pane>
    <SplitView.Content>
        <!-- Main content -->
    </SplitView.Content>
</SplitView>
```

参照 :[SplitView](/uwp/api/Windows.UI.Xaml.Controls.SplitView) 

デザインと使い方: [分割ビュー コントロールのガイド](split-view.md)

### <a name="web-view"></a>Web ビュー

Web コンテンツをホストするコンテナー コントロール。

```xaml
<WebView x:Name="webView1" Source="https://developer.microsoft.com" 
         Height="400" Width="800"/>
```

参照 :[WebView](/uwp/api/Windows.UI.Xaml.Controls.WebView) 

デザインと使い方: Web ビューのガイドライン 

サンプル コード: [XAML WebView コントロールのサンプル](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/XAML%20WebView%20control%20sample%20(Windows%208))

### <a name="semantic-zoom"></a>セマンティック ズーム

ユーザーが、項目のコレクションの 2 つの異なる表示の間でズームを実行できるようにするコンテナー コントロール。

```xaml
<SemanticZoom>
    <ZoomedInView>
        <GridView></GridView>
    </ZoomedInView>
    <ZoomedOutView>
        <GridView></GridView>
    </ZoomedOutView>
</SemanticZoom>
```

参照 :[SemanticZoom](/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) 

デザインと使い方: [セマンティック ズーム コントロールのガイド](semantic-zoom.md)

サンプル コード: [XAML GridView のグループ化と SemanticZoom のサンプル](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/XAML%20GridView%20grouping%20and%20SemanticZoom%20sample%20(Windows%208))

## <a name="progress-controls"></a>プログレス コントロール

### <a name="progress-bar"></a>進行状況バー
バーを表示して進行状況を示すコントロール。

![進行状況バー コントロール](images/controls/progress-bar-determinate.png)

特定の値を表示する進行状況バー。

```xaml
<ProgressBar x:Name="progressBar1" Value="50" Width="100"/>
```

![進行状況不定バー コントロール](images/controls/progress-bar-indeterminate.gif)

進行状況が不定であることを表す進行状況バー。

```xaml
<ProgressBar x:Name="indeterminateProgressBar1" IsIndeterminate="True" Width="100"/>
```

参照 :[ProgressBar](/uwp/api/Windows.UI.Xaml.Controls.ProgressBar) 

デザインと使い方: [プログレス コントロールのガイド](progress-controls.md) 

### <a name="progress-ring"></a>進行状況リング
リングを表示して進行状況が不定であることを示すコントロール。 

![進行状況リング コントロール](images/controls/progress-ring.png) 

```xaml
<ProgressRing x:Name="progressRing1" IsActive="True"/>
```

参照 :[ProgressRing](/uwp/api/Windows.UI.Xaml.Controls.ProgressRing) 

デザインと使い方: [プログレス コントロールのガイド](progress-controls.md) 

## <a name="text-controls"></a>テキスト コントロール

### <a name="auto-suggest-box"></a>自動提案ボックス
ユーザーが入力するときに、候補のテキストを表示するテキスト入力ボックスです。

![検索の自動提案ボックス](images/controls/auto-suggest-box.png) 

参照 :[AutoSuggestBox](/uwp/api/Windows.UI.Xaml.Controls.AutoSuggestBox)

デザインと使い方: [テキスト コントロール](text-controls.md)、[自動提案ボックス コントロールのガイド](auto-suggest-box.md)

サンプル コード: [AutoSuggestBox の移行のサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlAutoSuggestBox)

### <a name="multi-line-text-box"></a>複数行テキスト ボックス
「[テキスト ボックス](#text-box)」をご覧ください。

### <a name="password-box"></a>パスワード ボックス
パスワードを入力するためのコントロール。

 ![パスワード ボックス](images/controls/password-box.png)

```xaml
<PasswordBox x:Name="passwordBox1" 
             PasswordChanged="PasswordBox_PasswordChanged" />
```

参照 :[PasswordBox](/uwp/api/Windows.UI.Xaml.Controls.PasswordBox) 

デザインと使い方: [テキスト コントロール](text-controls.md)、[パスワード ボックス コントロールのガイド](password-box.md) 

サンプル コード: [XAML テキスト表示のサンプル](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/XAML%20text%20display%20sample%20(Windows%208))、[XAML テキスト編集のサンプル](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/XAML%20text%20editing%20sample%20(Windows%208))

### <a name="rich-edit-box"></a>リッチ エディット ボックス
書式付きテキスト、ハイパーリンク、イメージなどを含んだリッチ テキスト ドキュメントを編集できるコントロール。

```xaml
<RichEditBox />
```

参照 :[RichEditBox](/uwp/api/Windows.UI.Xaml.Controls.RichEditBox) 

デザインと使い方: [テキスト コントロール](text-controls.md)、[リッチ エディット ボックス コントロールのガイド](rich-edit-box.md)

サンプル コード: [XAML テキストのサンプル](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/XAML%20text%20display%20sample%20(Windows%208))

### <a name="search-box"></a>検索ボックス
「[自動提案ボックス](#auto-suggest-box)」をご覧ください。

### <a name="single-line-text-box"></a>単一行テキスト ボックス
「[テキスト ボックス](#text-box)」をご覧ください。

### <a name="static-textparagraph"></a>静的テキスト/段落
「[テキスト ブロック](#text-block)」をご覧ください。

### <a name="text-block"></a>テキスト ブロック
テキストを表示するコントロール。

![テキスト ブロック コントロール](images/controls/text-block.png) 

```xaml
<TextBlock x:Name="textBlock1" Text="I am a TextBlock"/>
```

参照 :[TextBlock](/uwp/api/Windows.UI.Xaml.Controls.TextBlock)、[RichTextBlock](/uwp/api/Windows.UI.Xaml.Controls.RichTextBlock) 

デザインと使い方: [テキスト コントロール](text-controls.md)、[テキスト ブロック コントロールのガイド](text-block.md)、[リッチ テキスト ブロック コントロールのガイド](rich-text-block.md)

サンプル コード: [XAML テキストのサンプル](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/XAML%20text%20display%20sample%20(Windows%208))

### <a name="text-box"></a>テキスト ボックス
1 行または複数行のプレーンテキスト フィールド。

![テキスト ボックス コントロール](images/controls/text-box.png)

```xaml
<TextBox x:Name="textBox1" Text="I am a Text Box."
         TextChanged="TextBox_TextChanged"/>
```

参照 :[TextBox](/uwp/api/Windows.UI.Xaml.Controls.TextBox) 

デザインと使い方: [テキスト コントロール](text-controls.md)、[テキスト ボックス コントロールのガイド](text-box.md) 

サンプル コード: [XAML テキストのサンプル](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/XAML%20text%20display%20sample%20(Windows%208))

## <a name="selection-controls"></a>選択コントロール

### <a name="check-box"></a>チェック ボックス
ユーザーがオンまたはオフにできるコントロール。

![チェック ボックスの 3 状態](images/templates-checkbox-states-default.png)

```xaml
<CheckBox x:Name="checkbox1" Content="CheckBox" 
          Checked="CheckBox_Checked"/>
```

参照 :[CheckBox](/uwp/api/Windows.UI.Xaml.Controls.CheckBox) 

デザインと使い方: [チェック ボックス コントロールのガイド](checkbox.md) 

### <a name="combo-box"></a>コンボ ボックス
ユーザーが選択できる項目のドロップダウン リスト。

![開かれた状態のコンボ ボックス](images/controls/combo-box-open.png) 

```xaml
<ComboBox x:Name="comboBox1" Width="100"
          SelectionChanged="ComboBox_SelectionChanged">
    <x:String>Item 1</x:String>
    <x:String>Item 2</x:String>
    <x:String>Item 3</x:String>
</ComboBox>
```

参照 :[ComboBox](/uwp/api/Windows.UI.Xaml.Controls.ComboBox) 

デザインと使い方: [リスト](lists.md) 

### <a name="list-box"></a>リスト ボックス
ユーザーが選択できる項目のインライン リストを表示するコントロール。 

![リスト ボックス コントロール](images/controls/list-box.png)

```xaml
<ListBox x:Name="listBox1" Width="100"
         SelectionChanged="ListBox_SelectionChanged">
    <x:String>List item 1</x:String>
    <x:String>List item 2</x:String>
    <x:String>List item 3</x:String>
</ListBox>
```

参照 :[ListBox](/uwp/api/Windows.UI.Xaml.Controls.ListBox) 

デザインと使い方: [リスト](lists.md) 

### <a name="radio-button"></a>ラジオ ボタン
ユーザーがオプションのグループから 1 つのオプションを選択できるようにするコントロール。 グループ化されたラジオ ボタンは、それぞれ相互に排他的です。

![ラジオ ボタン コントロール](images/controls/radio-button.png)

```xaml
<RadioButton x:Name="radioButton1" Content="RadioButton 1" GroupName="Group1" 
             Checked="RadioButton_Checked"/>
<RadioButton x:Name="radioButton2" Content="RadioButton 2" GroupName="Group1" 
             Checked="RadioButton_Checked" IsChecked="True"/>
<RadioButton x:Name="radioButton3" Content="RadioButton 3" GroupName="Group1" 
             Checked="RadioButton_Checked"/>
```

参照 :[RadioButton](/uwp/api/Windows.UI.Xaml.Controls.RadioButton) 

デザインと使い方: [ラジオ ボタン コントロールのガイド](radio-button.md)
 
### <a name="slider"></a>スライダー
ユーザーがトラックに沿って Thumb コントロールを動かすことで値の範囲から選択できるようにするコントロール。

![スライダー コントロール](images/controls/slider.png)

```xaml
<Slider x:Name="slider1" Width="100" ValueChanged="Slider_ValueChanged" />
```

参照 :[スライダー](/uwp/api/Windows.UI.Xaml.Controls.Slider) 

デザインと使い方: [スライダー コントロールのガイド](slider.md) 

### <a name="toggle-button"></a>トグル ボタン
2 つの状態を切り替えることができるボタン。

```xaml
<ToggleButton x:Name="toggleButton1" Content="Button" 
              Checked="ToggleButton_Checked"/>
```

参照 :[ToggleButton](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton)

デザインと使い方: [トグル コントロールのガイド](toggles.md) 

### <a name="toggle-switch"></a>トグル スイッチ
2 つの状態を切り替えることができるスイッチ。

![トグル スイッチ コントロール](images/controls/toggle-switch.png) 

```xaml
<ToggleSwitch x:Name="toggleSwitch1" Header="ToggleSwitch" 
              OnContent="On" OffContent="Off" 
              Toggled="ToggleSwitch_Toggled"/>
```

参照 :[ToggleSwitch](/uwp/api/Windows.UI.Xaml.Controls.ToggleSwitch) 

デザインと使い方: [トグル コントロールのガイド](toggles.md)
