---
title: Windows アプリでのコマンド処理
description: XamlUICommand クラスと StandardUICommand クラスを (ICommand インターフェイスと共に) 使用し、使用しているデバイスや入力の種類に関係なく、さまざまな型のコントロール型でコマンドを共有し、管理する方法。
ms.service: ''
ms.topic: overview
ms.date: 09/13/2019
ms.openlocfilehash: 767172fe3384fc74687b239768b277b6147c0fd4
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89160636"
---
# <a name="commanding-in-windows-apps-using-standarduicommand-xamluicommand-and-icommand"></a>StandardUICommand、XamlUICommand、ICommand を使用する Windows アプリでのコマンド処理

このトピックでは、Windows アプリケーションでのコマンド処理について説明します。 具体的には、[XamlUICommand](/uwp/api/windows.ui.xaml.input.xamluicommand) クラスと [StandardUICommand](/uwp/api/windows.ui.xaml.input.standarduicommand) クラスを (ICommand インターフェイスと共に) 使用し、使用しているデバイスや入力の種類に関係なく、さまざまな型のコントロールでコマンドを共有し、管理する方法について説明します。

![共有コマンドで共通する使用方法を表す図: 複数の UI サーフェスと "お気に入り" コマンド](images/commanding/generic-commanding.png)

*デバイスや入力の種類に関係なく、さまざまなコントロールでコマンドを共有する*

## <a name="important-apis"></a>重要な API

- [Windows.UI.Xaml.Input.ICommand](/uwp/api/windows.ui.xaml.input.icommand) と [System.Windows.Input.ICommand](/dotnet/api/system.windows.input.icommand)
- [XamlUICommand](/uwp/api/windows.ui.xaml.input.xamluicommand)
- [StandardUICommand](/uwp/api/windows.ui.xaml.input.standarduicommand)

## <a name="overview"></a>概要

<!-- See https://blogs.msdn.microsoft.com/jebarson/2017/07/26/writing-an-asynchronous-relaycommand-implementing-icommand/ -->

<!-- A command describes an action but not its implementation (in other words, the what, but not the how). For example, the "Paste" command indicates that the user wants to copy something from the clipboard to a target control, but does not specify how. -->

コマンドは、ボタンをクリックする、コンテキスト メニューから項目を選択するなど、UI 操作で直接呼び出すことができます。 キーボード アクセラレータ、ジェスチャ、音声認識、自動化/アクセシビリティ ツールなど、入力装置から間接的に呼び出すこともできます。 呼び出し後、コマンドは、コントロール (編集コントロールのテキスト ナビゲーション)、ウィンドウ (戻るナビゲーション)、またはアプリケーション (終了) で処理できます。

コマンドは、テキストを削除する、アクションを元に戻すなど、アプリ内の特定のコンテキストで操作できます。あるいは、音声を消す、明るさを調整するなど、コンテキストがない場合もあります。

次の画像で確認できる 2 つのコマンド インターフェイス ([CommandBar](app-bars.md) とフローティング コンテキスト [CommandBarFlyout](command-bar-flyout.md)) では、同じコマンドがいくつか共有されています。

![展開されたコマンド バー](images/control-examples/command-bar-photos.png)<br>*コマンド バー*

![Microsoft フォト ギャラリーのコンテキスト メニュー](images/ContextMenu_example.png)<br>*Microsoft フォト ギャラリーのコンテキスト メニュー*

## <a name="command-interactions"></a>コマンド操作

さまざまなデバイス、入力の種類、UI サーフェスがコマンドの呼び出し方法に影響を与えるため、可能な限りたくさんのコマンド処理サーフェスを介してコマンドを公開することをお勧めします。 たとえば、[Swipe](swipe.md)、[MenuBar](menus.md)、[CommandBar](app-bars.md)、[CommandBarFlyout](command-bar-flyout.md)、従来の[コンテキスト メニュー](menus.md)を組み合わせることができます。

**重要なコマンドの場合、入力固有のアクセラレータを使用してください。** 入力アクセラレータを使用すると、使用している入力装置によっては、ユーザーはアクションをよりすばやく実行できます。

さまざまな入力の種類で共通する入力アクセラレータ:

- **ポインター** - マウスとペンのホバー ボタン
- **キーボード** - ショートカット (アクセス キーとアクセラレータ キー)
- **タッチ** - スワイプ
- **タッチ** - 引いてデータを更新する

アプリケーションの機能を誰でも使えるようにするには、入力の種類と使い勝手を検討する必要があります。 たとえば、コレクション (特に、ユーザーが編集できるコレクション) には通常、入力装置によって実行方法がかなり異なるコマンドがたくさん含まれています。

次の表は、典型的なコレクション コマンドとそれを公開する方法をまとめたものです。

| コマンド          | 入力方法を問わない | マウス アクセラレータ | キーボード アクセラレータ | タッチ アクセラレータ |
| ---------------- | -------------- | ----------------- | -------------------- | ----------------- |
| 項目の削除      | ショートカット メニュー   | ホバー ボタン      | DEL キー              | スワイプして削除   |
| フラグの設定        | ショートカット メニュー   | ホバー ボタン      | Ctrl + Shift + G         | スワイプしてフラグを設定     |
| データの更新     | ショートカット メニュー   | なし               | F5 キー               | 引っ張って更新   |
| お気に入りに追加 | ショートカット メニュー   | ホバー ボタン      | F、Ctrl + S            | スワイプしてお気に入りに追加 |

**コンテキスト メニューを常に提供する** 従来のコンテキスト コマンドまたは CommandBarFlyout に関連するすべてのコンテキスト コマンドを含めることをお勧めします。いずれも、入力の種類を問わず、サポートされています。 たとえば、ポインターのホバー イベント中にのみコマンドが公開される場合、タッチ専用デバイスでは利用できません。

## <a name="commands-in-windows-applications"></a>Windows アプリケーションのコマンド

Windows アプリケーションのコマンド実行エクスペリエンスは、いくつかの方法で共有および管理できます。 分離コードで、クリックなど、標準的な操作にイベント ハンドラーを定義したり (UI の複雑度によっては、かなり非効率的になることがあります)、標準的な操作のイベント リスナーを共有ハンドラーにバインドしたり、コマンド ロジックを表す ICommand 実装にコントロールの Command プロパティをバインドしたりできます。

コマンド サーフェス全体で、機能が豊富で包括的なユーザー エクスペリエンスを効率的かつ、コード重複を最小限に抑えて提供するには、このトピックで説明するコマンド バインド機能を利用することをお勧めします (標準的なイベント処理については、個々のイベント トピックをご覧ください)。

共有コマンド リソースにコントロールをバインドするには、ICommand インターフェイスを自分で実装したり、[XamlUICommand](/uwp/api/windows.ui.xaml.input.xamluicommand) ベース クラスまたは [StandardUICommand](/uwp/api/windows.ui.xaml.input.standarduicommand) 派生クラスによって定義されるプラットフォーム コマンドの 1 つからコマンドをビルドしたりできます。

- ICommand インターフェイス ([Windows.UI.Xaml.Input.ICommand](/uwp/api/windows.ui.xaml.input.icommand) または [System.Windows.Input.ICommand](/dotnet/api/system.windows.input.icommand)) を利用すると、完全にカスタマイズされた、再利用可能なコマンドをアプリ全体で作成できます。
- [XamlUICommand](/uwp/api/windows.ui.xaml.input.xamluicommand) からもこの機能が与えられますが、コマンドの動作、キーボード ショートカット (アクセス キーとアクセラレータ キー)、アイコン、ラベル、説明など、一連の組み込みコマンド プロパティを公開するため、開発が簡単になります。
- [StandardUICommand](/uwp/api/windows.ui.xaml.input.standarduicommand) では、プロパティが事前定義されている一連の標準プラットフォーム コマンドから選択できるため、さらに簡単になります。

> [!Important]
> UWP アプリケーションでは、コマンドは、[Windows.UI.Xaml.Input.ICommand](/uwp/api/windows.ui.xaml.input.icommand) (C++) または [System.Windows.Input.ICommand](/dotnet/api/system.windows.input.icommand) (C#) インターフェイスの実装になります。選択した言語フレームワークによって決まります。

## <a name="command-experiences-using-the-standarduicommand-class"></a>StandardUICommand クラスを使用したコマンド エクスペリエンス

[XamlUiCommand](/uwp/api/windows.ui.xaml.input.xamluicommand) (derived from [Windows.UI.Xaml.Input.ICommand](/uwp/api/windows.ui.xaml.input.icommand) for C++ または [System.Windows.Input.ICommand](/dotnet/api/system.windows.input.icommand) for C#) から派生した [StandardUICommand](/uwp/api/windows.ui.xaml.input.standarduicommand) クラスからは、アイコン、キーボード アクセラレータ、説明など、プロパティが事前定義された一連の標準プラットフォーム コマンドが公開されます。

[StandardUICommand](/uwp/api/windows.ui.xaml.input.standarduicommand) の場合、`Save` や `Delete` など、共通のコマンドを一貫性のある方法で簡単に定義できます。 必要な作業は、execute 関数と canExecute 関数を指定することだけです。

### <a name="example"></a>例

![StandardUICommand サンプル](images/commanding/StandardUICommandSampleOptimized.gif)

*StandardUICommandSample*

| この例のコードをダウンロードする |
| -------------------- |
| [UWP コマンド処理サンプル (StandardUICommand)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-commanding-standarduicommand.zip) |

この例では、[StandardUICommand](/uwp/api/windows.ui.xaml.input.standarduicommand) クラスを介して実装された項目削除コマンドで基本の [ListView](listview-and-gridview.md) を機能強化し、同時に、[MenuBar](menus.md)、[Swipe](swipe.md) コントロール、ホバー ボタン、[コンテキスト メニュー](menus.md)を利用し、さまざまな種類の入力方法を最適化する方法を確認できます。

> [!NOTE]
> このサンプルは Microsoft.UI.Xaml.Controls NuGet パッケージを必要としますが、これは [Microsoft Windows UI ライブラリ](/uwp/toolkits/winui/)に含まれています。

**Xaml:**

サンプル UI には、5 つの項目の [ListView](/uwp/api/windows.ui.xaml.controls.listview) が含まれています。 Delete [StandardUICommand](/uwp/api/windows.ui.xaml.input.standarduicommand) は [MenuBarItem](/uwp/api/microsoft.ui.xaml.controls.menubaritem)、[SwipeItem](/uwp/api/microsoft.ui.xaml.controls.swipeitem)、[AppBarButton](/uwp/api/windows.ui.xaml.controls.appbarbutton)、[ContextFlyout メニュー](/uwp/api/windows.ui.xaml.uielement.contextflyout)にバインドされています。

``` xaml
<Page
    x:Class="StandardUICommandSample.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:StandardUICommandSample"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    xmlns:muxcontrols="using:Microsoft.UI.Xaml.Controls"
    mc:Ignorable="d"
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    <Page.Resources>
        <Style x:Key="HorizontalSwipe" 
               TargetType="ListViewItem" 
               BasedOn="{StaticResource ListViewItemRevealStyle}">
            <Setter Property="Height" Value="60"/>
            <Setter Property="Padding" Value="0"/>
            <Setter Property="HorizontalContentAlignment" Value="Stretch"/>
            <Setter Property="VerticalContentAlignment" Value="Stretch"/>
            <Setter Property="BorderThickness" Value="0"/>
        </Style>
    </Page.Resources>

    <Grid Loaded="ControlExample_Loaded">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>

        <StackPanel Grid.Row="0" 
                    Padding="10" 
                    BorderThickness="0,0,0,1" 
                    BorderBrush="LightBlue"
                    Background="AliceBlue">
            <TextBlock Style="{StaticResource HeaderTextBlockStyle}">
                StandardUICommand sample
            </TextBlock>
            <TextBlock Style="{StaticResource SubtitleTextBlockStyle}" Margin="0,0,0,10">
                This sample shows how to use the StandardUICommand class to 
                share a platform command and consistent user experiences 
                across various controls.
            </TextBlock>
            <TextBlock Style="{StaticResource SubtitleTextBlockStyle}" Margin="0,0,0,0">
                Specifically, we define a standard delete command and add it 
                to a variety of command surfaces, all of which share a common 
                icon, label, keyboard accelerator, and description.
            </TextBlock>
        </StackPanel>

        <muxcontrols:MenuBar Grid.Row="1" Padding="10">
            <muxcontrols:MenuBarItem Title="File">
            </muxcontrols:MenuBarItem>
            <muxcontrols:MenuBarItem Title="Edit">
                <MenuFlyoutItem x:Name="DeleteFlyoutItem"/>
            </muxcontrols:MenuBarItem>
            <muxcontrols:MenuBarItem Title="Help">
            </muxcontrols:MenuBarItem>
        </muxcontrols:MenuBar>

        <ListView x:Name="ListViewRight" Grid.Row="2" 
                  Loaded="ListView_Loaded" 
                  IsItemClickEnabled="True" 
                  SelectionMode="Single" 
                  SelectionChanged="ListView_SelectionChanged" 
                  ItemContainerStyle="{StaticResource HorizontalSwipe}">
            <ListView.ItemTemplate>
                <DataTemplate x:DataType="local:ListItemData">
                    <UserControl PointerEntered="ListViewSwipeContainer_PointerEntered" 
                                 PointerExited="ListViewSwipeContainer_PointerExited">
                        <UserControl.ContextFlyout>
                            <MenuFlyout>
                                <MenuFlyoutItem 
                                    Command="{x:Bind Command}" 
                                    CommandParameter="{x:Bind Text}" />
                            </MenuFlyout>
                        </UserControl.ContextFlyout>
                        <Grid AutomationProperties.Name="{x:Bind Text}">
                            <VisualStateManager.VisualStateGroups>
                                <VisualStateGroup x:Name="HoveringStates">
                                    <VisualState x:Name="HoverButtonsHidden" />
                                    <VisualState x:Name="HoverButtonsShown">
                                        <VisualState.Setters>
                                            <Setter Target="HoverButton.Visibility" 
                                                    Value="Visible" />
                                        </VisualState.Setters>
                                    </VisualState>
                                </VisualStateGroup>
                            </VisualStateManager.VisualStateGroups>
                            <SwipeControl x:Name="ListViewSwipeContainer" >
                                <SwipeControl.RightItems>
                                    <SwipeItems Mode="Execute">
                                        <SwipeItem x:Name="DeleteSwipeItem" 
                                                   Background="Red" 
                                                   Command="{x:Bind Command}" 
                                                   CommandParameter="{x:Bind Text}"/>
                                    </SwipeItems>
                                </SwipeControl.RightItems>
                                <Grid VerticalAlignment="Center">
                                    <TextBlock Text="{x:Bind Text}" 
                                               Margin="10" 
                                               FontSize="18" 
                                               HorizontalAlignment="Left" 
                                               VerticalAlignment="Center"/>
                                    <AppBarButton x:Name="HoverButton" 
                                                  IsTabStop="False" 
                                                  HorizontalAlignment="Right" 
                                                  Visibility="Collapsed" 
                                                  Command="{x:Bind Command}" 
                                                  CommandParameter="{x:Bind Text}"/>
                                </Grid>
                            </SwipeControl>
                        </Grid>
                    </UserControl>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </Grid>
</Page>
```

**分離コード**

1. まず、テキスト文字列と ListView の ListViewItem 別の ICommand を含む `ListItemData` クラスを定義します。

```csharp
public class ListItemData
{
    public String Text { get; set; }
    public ICommand Command { get; set; }
}
```

2. MainPage クラスで、[ListView](/uwp/api/windows.ui.xaml.controls.listview) [ItemTemplate](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate) の [DataTemplate](/uwp/api/windows.ui.xaml.datatemplate) に `ListItemData` オブジェクトのコレクションを定義します。 次に、5 つの項目の最初のコレクションをそれに入力します (テキストと関連する [StandardUICommand](/uwp/api/windows.ui.xaml.input.standarduicommand) Delete)。

```csharp
/// <summary>
/// ListView item collection.
/// </summary>
ObservableCollection<ListItemData> collection = 
    new ObservableCollection<ListItemData>();

/// <summary>
/// Handler for the layout Grid control load event.
/// </summary>
/// <param name="sender">Source of the control loaded event</param>
/// <param name="e">Event args for the loaded event</param>
private void ControlExample_Loaded(object sender, RoutedEventArgs e)
{
    // Create the standard Delete command.
    var deleteCommand = new StandardUICommand(StandardUICommandKind.Delete);
    deleteCommand.ExecuteRequested += DeleteCommand_ExecuteRequested;

    DeleteFlyoutItem.Command = deleteCommand;

    for (var i = 0; i < 5; i++)
    {
        collection.Add(
            new ListItemData {
                Text = "List item " + i.ToString(),
                Command = deleteCommand });
    }
}

/// <summary>
/// Handler for the ListView control load event.
/// </summary>
/// <param name="sender">Source of the control loaded event</param>
/// <param name="e">Event args for the loaded event</param>
private void ListView_Loaded(object sender, RoutedEventArgs e)
{
    var listView = (ListView)sender;
    // Populate the ListView with the item collection.
    listView.ItemsSource = collection;
}
```

3. 次に、ICommand ExecuteRequested ハンドラーを定義します。このハンドラーに項目の削除コマンドを実装します。

``` csharp
/// <summary>
/// Handler for the Delete command.
/// </summary>
/// <param name="sender">Source of the command event</param>
/// <param name="e">Event args for the command event</param>
private void DeleteCommand_ExecuteRequested(
    XamlUICommand sender, ExecuteRequestedEventArgs args)
{
    // If possible, remove specfied item from collection.
    if (args.Parameter != null)
    {
        foreach (var i in collection)
        {
            if (i.Text == (args.Parameter as string))
            {
                collection.Remove(i);
                return;
            }
        }
    }
    if (ListViewRight.SelectedIndex != -1)
    {
        collection.RemoveAt(ListViewRight.SelectedIndex);
    }
}
```

4. 最後に、[PointerEntered](/uwp/api/windows.ui.xaml.uielement.pointerentered)、[PointerExited](/uwp/api/windows.ui.xaml.uielement.pointerexited)、[SelectionChanged](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) イベントなど、さまざま ListView イベントのハンドラーを定義します。 ポインターのイベント ハンドラーは、各項目の [削除] ボタンを表示するか、非表示にする目的で使用されます。

```csharp
/// <summary>
/// Handler for the ListView selection changed event.
/// </summary>
/// <param name="sender">Source of the selection changed event</param>
/// <param name="e">Event args for the selection changed event</param>
private void ListView_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    if (ListViewRight.SelectedIndex != -1)
    {
        var item = collection[ListViewRight.SelectedIndex];
    }
}

/// <summary>
/// Handler for the pointer entered event.
/// Displays the delete item "hover" buttons.
/// </summary>
/// <param name="sender">Source of the pointer entered event</param>
/// <param name="e">Event args for the pointer entered event</param>
private void ListViewSwipeContainer_PointerEntered(
    object sender, PointerRoutedEventArgs e)
{
    if (e.Pointer.PointerDeviceType == 
        Windows.Devices.Input.PointerDeviceType.Mouse || 
        e.Pointer.PointerDeviceType == 
        Windows.Devices.Input.PointerDeviceType.Pen)
    {
        VisualStateManager.GoToState(
            sender as Control, "HoverButtonsShown", true);
    }
}

/// <summary>
/// Handler for the pointer exited event.
/// Hides the delete item "hover" buttons.
/// </summary>
/// <param name="sender">Source of the pointer exited event</param>
/// <param name="e">Event args for the pointer exited event</param>

private void ListViewSwipeContainer_PointerExited(
    object sender, PointerRoutedEventArgs e)
{
    VisualStateManager.GoToState(
        sender as Control, "HoverButtonsHidden", true);
}
```

## <a name="command-experiences-using-the-xamluicommand-class"></a>XamlUICommand クラスを使用したコマンド エクスペリエンス

[StandardUICommand](/uwp/api/windows.ui.xaml.input.standarduicommand) クラスで定義されないコマンドを作成する必要がある場合、あるいはコマンドの外見をもっと調整する場合、[ICommand](/uwp/api/windows.ui.xaml.input.icommand) から派生する[XamlUiCommand](/uwp/api/windows.ui.xaml.input.xamluicommand) クラスでさまざまな UI プロパティ (アイコン、ラベル、説明、キーボード ショートカットなど)、メソッド、イベントを追加し、カスタム コマンドの UI と動作を簡単に定義できます。

[XamlUICommand](/uwp/api/windows.ui.xaml.input.xamluicommand) を利用すると、個別のプロパティを設定することなく、アイコン、ラベル、説明、キーボード ショートカット (アクセス キーとキーボード アクセラレータの両方) などの UI をコントロール バインドで指定できます。

### <a name="example"></a>例

![XamlUICommand サンプル](images/commanding/XamlUICommandSampleOptimized.gif)

*XamlUICommandSample*

| この例のコードをダウンロードする |
| -------------------- |
| [UWP コマンド処理サンプル (XamlUICommand)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-commanding-xamluicommand.zip) |

この例では、前の [StandardUICommand](/uwp/api/windows.ui.xaml.input.standarduicommand) の例と削除機能が同じくしていますが、[XamlUICommand](/uwp/api/windows.ui.xaml.input.xamluicommand) クラスを利用することで、独自のフォント アイコン、ラベル、キーボード アクセラレータ、説明でカスタムの削除アイコンを定義できることを示しています。 [StandardUICommand](/uwp/api/windows.ui.xaml.input.standarduicommand) の例と同様に、基本の [ListView](listview-and-gridview.md) を [XamlUICommand](/uwp/api/windows.ui.xaml.input.xamluicommand) クラス経由で実装された項目削除コマンドで機能強化し、同時に、[MenuBar](menus.md)、[Swipe](swipe.md) コントロール、ホバー ボタン、[コンテキスト メニュー](menus.md)を利用し、さまざまな種類の入力方法を最適化しています。

前のセクションの StandardUICommand の例で見られたように、多くのプラットフォーム コントロールでは XamlUICommand プロパティがこっそり使用されています。 

> [!NOTE]
> このサンプルは Microsoft.UI.Xaml.Controls NuGet パッケージを必要としますが、これは [Microsoft Windows UI ライブラリ](/uwp/toolkits/winui/)に含まれています。

**Xaml:**

サンプル UI には、5 つの項目の [ListView](/uwp/api/windows.ui.xaml.controls.listview) が含まれています。 カスタムの [XamlUICommand](/uwp/api/windows.ui.xaml.input.xamluicommand) Delete は [MenuBarItem](/uwp/api/microsoft.ui.xaml.controls.menubaritem)、[SwipeItem](/uwp/api/microsoft.ui.xaml.controls.swipeitem)、[AppBarButton](/uwp/api/windows.ui.xaml.controls.appbarbutton)、[ContextFlyout メニュー](/uwp/api/windows.ui.xaml.uielement.contextflyout)にバインドされています。

``` xaml
<Page
    x:Class="XamlUICommand_Sample.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:XamlUICommand_Sample"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    xmlns:muxcontrols="using:Microsoft.UI.Xaml.Controls"
    mc:Ignorable="d"
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    <Page.Resources>
        <XamlUICommand x:Name="CustomXamlUICommand" 
                       ExecuteRequested="DeleteCommand_ExecuteRequested"
                       Description="Custom XamlUICommand" 
                       Label="Custom XamlUICommand">
            <XamlUICommand.IconSource>
                <FontIconSource FontFamily="Wingdings" Glyph="&#x4D;"/>
            </XamlUICommand.IconSource>
            <XamlUICommand.KeyboardAccelerators>
                <KeyboardAccelerator Key="D" Modifiers="Control"/>
            </XamlUICommand.KeyboardAccelerators>
        </XamlUICommand>

        <Style x:Key="HorizontalSwipe" 
               TargetType="ListViewItem" 
               BasedOn="{StaticResource ListViewItemRevealStyle}">
            <Setter Property="Height" Value="70"/>
            <Setter Property="Padding" Value="0"/>
            <Setter Property="HorizontalContentAlignment" Value="Stretch"/>
            <Setter Property="VerticalContentAlignment" Value="Stretch"/>
            <Setter Property="BorderThickness" Value="0"/>
        </Style>
        
    </Page.Resources>

    <Grid Loaded="ControlExample_Loaded" Name="MainGrid">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        
        <StackPanel Grid.Row="0" 
                    Padding="10" 
                    BorderThickness="0,0,0,1" 
                    BorderBrush="LightBlue"
                    Background="AliceBlue">
            <TextBlock Style="{StaticResource HeaderTextBlockStyle}">
                XamlUICommand sample
            </TextBlock>
            <TextBlock Style="{StaticResource SubtitleTextBlockStyle}" Margin="0,0,0,10">
                This sample shows how to use the XamlUICommand class to 
                share a custom command with consistent user experiences 
                across various controls.
            </TextBlock>
            <TextBlock Style="{StaticResource SubtitleTextBlockStyle}" Margin="0,0,0,0">
                Specifically, we define a custom delete command and add it 
                to a variety of command surfaces, all of which share a common 
                icon, label, keyboard accelerator, and description.
            </TextBlock>
        </StackPanel>

        <muxcontrols:MenuBar Grid.Row="1">
            <muxcontrols:MenuBarItem Title="File">
            </muxcontrols:MenuBarItem>
            <muxcontrols:MenuBarItem Title="Edit">
                <MenuFlyoutItem x:Name="DeleteFlyoutItem" 
                                Command="{StaticResource CustomXamlUICommand}"/>
            </muxcontrols:MenuBarItem>
            <muxcontrols:MenuBarItem Title="Help">
            </muxcontrols:MenuBarItem>
        </muxcontrols:MenuBar>

        <ListView x:Name="ListViewRight" Grid.Row="2" 
                  Loaded="ListView_Loaded" 
                  IsItemClickEnabled="True"
                  SelectionMode="Single" 
                  SelectionChanged="ListView_SelectionChanged" 
                  ItemContainerStyle="{StaticResource HorizontalSwipe}">
            <ListView.ItemTemplate>
                <DataTemplate x:DataType="local:ListItemData">
                    <UserControl PointerEntered="ListViewSwipeContainer_PointerEntered"
                                 PointerExited="ListViewSwipeContainer_PointerExited">
                        <UserControl.ContextFlyout>
                            <MenuFlyout>
                                <MenuFlyoutItem 
                                    Command="{x:Bind Command}" 
                                    CommandParameter="{x:Bind Text}" />
                            </MenuFlyout>
                        </UserControl.ContextFlyout>
                        <Grid AutomationProperties.Name="{x:Bind Text}">
                            <VisualStateManager.VisualStateGroups>
                                <VisualStateGroup x:Name="HoveringStates">
                                    <VisualState x:Name="HoverButtonsHidden" />
                                    <VisualState x:Name="HoverButtonsShown">
                                        <VisualState.Setters>
                                            <Setter Target="HoverButton.Visibility" 
                                                    Value="Visible" />
                                        </VisualState.Setters>
                                    </VisualState>
                                </VisualStateGroup>
                            </VisualStateManager.VisualStateGroups>
                            <SwipeControl x:Name="ListViewSwipeContainer">
                                <SwipeControl.RightItems>
                                    <SwipeItems Mode="Execute">
                                        <SwipeItem x:Name="DeleteSwipeItem"
                                                   Background="Red" 
                                                   Command="{x:Bind Command}" 
                                                   CommandParameter="{x:Bind Text}"/>
                                    </SwipeItems>
                                </SwipeControl.RightItems>
                                <Grid VerticalAlignment="Center">
                                    <TextBlock Text="{x:Bind Text}" 
                                               Margin="10" 
                                               FontSize="18" 
                                               HorizontalAlignment="Left"       
                                               VerticalAlignment="Center"/>
                                    <AppBarButton x:Name="HoverButton" 
                                                  IsTabStop="False" 
                                                  HorizontalAlignment="Right" 
                                                  Visibility="Collapsed" 
                                                  Command="{x:Bind Command}" 
                                                  CommandParameter="{x:Bind Text}"/>
                                </Grid>
                            </SwipeControl>
                        </Grid>
                    </UserControl>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </Grid>
</Page>
```

**分離コード**

1. まず、テキスト文字列と ListView の ListViewItem 別の ICommand を含む `ListItemData` クラスを定義します。

```csharp
public class ListItemData
{
    public String Text { get; set; }
    public ICommand Command { get; set; }
}
```

2. MainPage クラスで、[ListView](/uwp/api/windows.ui.xaml.controls.listview) [ItemTemplate](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate) の [DataTemplate](/uwp/api/windows.ui.xaml.datatemplate) に `ListItemData` オブジェクトのコレクションを定義します。 次に、5 つの項目の最初のコレクションをそれに入力します (テキストと関連する [XamlUICommand](/uwp/api/windows.ui.xaml.input.xamluicommand))。

```csharp
ObservableCollection<ListItemData> collection = new ObservableCollection<ListItemData>();

private void ControlExample_Loaded(object sender, RoutedEventArgs e)
{
    for (var i = 0; i < 5; i++)
    {
        collection.Add(
           new ListItemData { Text = "List item " + i.ToString(), Command = CustomXamlUICommand });
    }
}

private void ListView_Loaded(object sender, RoutedEventArgs e)
{
    var listView = (ListView)sender;
    listView.ItemsSource = collection;
}
```

3. 次に、ICommand ExecuteRequested ハンドラーを定義します。このハンドラーに項目の削除コマンドを実装します。

``` csharp
private void DeleteCommand_ExecuteRequested(
   XamlUICommand sender, ExecuteRequestedEventArgs args)
{
    if (args.Parameter != null)
    {
        foreach (var i in collection)
        {
            if (i.Text == (args.Parameter as string))
            {
                collection.Remove(i);
                return;
            }
        }
    }
    if (ListViewRight.SelectedIndex != -1)
    {
        collection.RemoveAt(ListViewRight.SelectedIndex);
    }
}
```

4. 最後に、[PointerEntered](/uwp/api/windows.ui.xaml.uielement.pointerentered)、[PointerExited](/uwp/api/windows.ui.xaml.uielement.pointerexited)、[SelectionChanged](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) イベントなど、さまざま ListView イベントのハンドラーを定義します。 ポインターのイベント ハンドラーは、各項目の [削除] ボタンを表示するか、非表示にする目的で使用されます。

```csharp
private void ListView_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    if (ListViewRight.SelectedIndex != -1)
    {
        var item = collection[ListViewRight.SelectedIndex];
    }
}

private void ListViewSwipeContainer_PointerEntered(object sender, PointerRoutedEventArgs e)
{
    if (e.Pointer.PointerDeviceType == 
        Windows.Devices.Input.PointerDeviceType.Mouse || 
        e.Pointer.PointerDeviceType == Windows.Devices.Input.PointerDeviceType.Pen)
    {
        VisualStateManager.GoToState(sender as Control, "HoverButtonsShown", true);
    }
}

private void ListViewSwipeContainer_PointerExited(object sender, PointerRoutedEventArgs e)
{
    VisualStateManager.GoToState(sender as Control, "HoverButtonsHidden", true);
}
```

## <a name="command-experiences-using-the-icommand-interface"></a>ICommand インターフェイスを使用したコマンド エクスペリエンス

標準の UWP コントロール (ボタン、リスト、選択、暦、予測テキスト) からは、さまざまな共通コマンド操作の基本が提供されます。 コントロールの種類の完全な一覧については、[Windows アプリのコントロールとパターン](index.md)に関するページをご覧ください。

構造化されたコマンド処理をサポートする最も基本的な方法は、ICommand インターフェイスの実装を定義することです ([Windows.UI.Xaml.Input.ICommand](/uwp/api/windows.ui.xaml.input.icommand) for C++ または [System.Windows.Input.ICommand](/dotnet/api/system.windows.input.icommand) for C#)。  この ICommand インスタンスはその後、ボタンなどのコントロールにバインドできます。

> [!NOTE]
> 場合によっては、メソッドを Click イベントにバインドし、プロパティを IsEnabled プロパティにバインドするのと同じくらい効率的になります。

#### <a name="example"></a>例

![コマンド インターフェイスの例](images/commanding/icommand.gif)

*ICommand 例*

| この例のコードをダウンロードする |
| -------------------- |
| [UWP コマンド処理サンプル (ICommand)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-commanding-icommand.zip) |

この基本例では、ボタン クリック、キーボード アクセラレータ、マウス ホイールの回転で 1 つのコマンドを呼び出す方法を実演します。

[ListViews](/uwp/api/windows.ui.xaml.controls.listview) を 2 つ使用しますが、1 つには 5 つの項目を入力し、もう 1 つは空にします。また、ボタンを 2 つ使用しますが、1 つは左の ListView から右の ListView に項目を移動させるためのものであり、もう 1 つは右から左に項目を移動させるためのものです。 各ボタンは対応するコマンド (それぞれ、ViewModel.MoveRightCommand と ViewModel.MoveLeftCommand) にバインドされており、関連する ListView の項目数に基づいて自動的に有効または無効になります。

**次の XAML コードでは、今回の例の UI が定義されます。**

```xaml
<Page
    x:Class="UICommand1.View.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:vm="using:UICommand1.ViewModel"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d"
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    <Page.Resources>
        <vm:OpacityConverter x:Key="opaque" />
    </Page.Resources>

    <Grid Name="ItemGrid"
          Background="AliceBlue"
          PointerWheelChanged="Page_PointerWheelChanged">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*"/>
            <ColumnDefinition Width="2*"/>
            <ColumnDefinition Width="*"/>
        </Grid.ColumnDefinitions>
        <ListView Grid.Column="0" VerticalAlignment="Center"
                  x:Name="CommandListView" 
                  ItemsSource="{x:Bind Path=ViewModel.ListItemLeft}" 
                  SelectionMode="None" IsItemClickEnabled="False" 
                  HorizontalAlignment="Right">
            <ListView.ItemTemplate>
                <DataTemplate x:DataType="vm:ListItemData">
                    <Grid VerticalAlignment="Center">
                        <AppBarButton Label="{x:Bind ListItemText}">
                            <AppBarButton.Icon>
                                <SymbolIcon Symbol="{x:Bind ListItemIcon}"/>
                            </AppBarButton.Icon>
                        </AppBarButton>
                    </Grid>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
        <Grid Grid.Column="1" Margin="0,0,0,0"
              HorizontalAlignment="Center" 
              VerticalAlignment="Center">
            <Grid.RowDefinitions>
                <RowDefinition Height="*"/>
                <RowDefinition Height="*"/>
                <RowDefinition Height="*"/>
            </Grid.RowDefinitions>
            <StackPanel Grid.Row="1">
                <FontIcon FontFamily="{StaticResource SymbolThemeFontFamily}" 
                          FontSize="40" Glyph="&#xE893;" 
                          Opacity="{x:Bind Path=ViewModel.ListItemLeft.Count, 
                                        Mode=OneWay, Converter={StaticResource opaque}}"/>
                <Button Name="MoveItemRightButton"
                        Margin="0,10,0,10" Width="120" HorizontalAlignment="Center"
                        Command="{x:Bind Path=ViewModel.MoveRightCommand}">
                    <Button.KeyboardAccelerators>
                        <KeyboardAccelerator 
                            Modifiers="Control" 
                            Key="Add" />
                    </Button.KeyboardAccelerators>
                    <StackPanel>
                        <SymbolIcon Symbol="Next"/>
                        <TextBlock>Move item right</TextBlock>
                    </StackPanel>
                </Button>
                <Button Name="MoveItemLeftButton" 
                            Margin="0,10,0,10" Width="120" HorizontalAlignment="Center"
                            Command="{x:Bind Path=ViewModel.MoveLeftCommand}">
                    <Button.KeyboardAccelerators>
                        <KeyboardAccelerator 
                            Modifiers="Control" 
                            Key="Subtract" />
                    </Button.KeyboardAccelerators>
                    <StackPanel>
                        <SymbolIcon Symbol="Previous"/>
                        <TextBlock>Move item left</TextBlock>
                    </StackPanel>
                </Button>
                <FontIcon FontFamily="{StaticResource SymbolThemeFontFamily}" 
                          FontSize="40" Glyph="&#xE892;"
                          Opacity="{x:Bind Path=ViewModel.ListItemRight.Count, 
                                        Mode=OneWay, Converter={StaticResource opaque}}"/>
            </StackPanel>
        </Grid>
        <ListView Grid.Column="2" 
                  x:Name="CommandListViewRight" 
                  VerticalAlignment="Center" 
                  IsItemClickEnabled="False" 
                  SelectionMode="None"
                  ItemsSource="{x:Bind Path=ViewModel.ListItemRight}" 
                  HorizontalAlignment="Left">
            <ListView.ItemTemplate>
                <DataTemplate x:DataType="vm:ListItemData">
                    <Grid VerticalAlignment="Center">
                        <AppBarButton Label="{x:Bind ListItemText}">
                            <AppBarButton.Icon>
                                <SymbolIcon Symbol="{x:Bind ListItemIcon}"/>
                            </AppBarButton.Icon>
                        </AppBarButton>
                    </Grid>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </Grid>
</Page>
```

**前の UI の分離コードは次のようになります。**

分離コードでは、コマンド コードが含まれるビュー モデルに接続します。 また、やはりコマンド コードをつなげる、マウス ホイールからの入力のためのハンドラーを定義します。

```csharp
using Windows.UI.Xaml;
using Windows.UI.Xaml.Input;
using Windows.UI.Xaml.Controls;
using UICommand1.ViewModel;
using Windows.System;
using Windows.UI.Core;

namespace UICommand1.View
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    public sealed partial class MainPage : Page
    {
        // Reference to our view model.
        public UICommand1ViewModel ViewModel { get; set; }

        // Initialize our view and view model.
        public MainPage()
        {
            this.InitializeComponent();
            ViewModel = new UICommand1ViewModel();
        }

        /// <summary>
        /// Handle mouse wheel input and assign our
        /// commands to appropriate direction of rotation.
        /// </summary>
        /// <param name="sender"></param>
        /// <param name="e"></param>
        private void Page_PointerWheelChanged(object sender, PointerRoutedEventArgs e)
        {
            var props = e.GetCurrentPoint(sender as UIElement).Properties;

            // Require CTRL key and accept only vertical mouse wheel movement 
            // to eliminate accidental wheel input.
            if ((Window.Current.CoreWindow.GetKeyState(VirtualKey.Control) != 
                CoreVirtualKeyStates.None) && !props.IsHorizontalMouseWheel)
            {
                bool delta = props.MouseWheelDelta < 0 ? true : false;

                switch (delta)
                {
                    case true:
                        ViewModel.MoveRight();
                        break;
                    case false:
                        ViewModel.MoveLeft();
                        break;
                    default:
                        break;
                }
            }
        }
    }
}
```

**ビュー モデルからのコードは次のようになります。**

今回のビュー モデルでは、アプリに 2 つのコマンドの実行詳細を定義し、1 つの ListView に入力し、各 ListView の項目数に基づき、一部の追加 UI の表示/非表示を決定するための不透明度コンバーターを提供します。

```csharp
using System;
using System.Collections.ObjectModel;
using System.ComponentModel;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Data;

namespace UICommand1.ViewModel
{
    /// <summary>
    /// UI properties for our list items.
    /// </summary>
    public class ListItemData
    {
        /// <summary>
        /// Gets and sets the list item content string.
        /// </summary>
        public string ListItemText { get; set; }
        /// <summary>
        /// Gets and sets the list item icon.
        /// </summary>
        public Symbol ListItemIcon { get; set; }
    }

    /// <summary>
    /// View Model that sets up a command to handle invoking the move item buttons.
    /// </summary>
    public class UICommand1ViewModel
    {
        /// <summary>
        /// The command to invoke when the Move item left button is pressed.
        /// </summary>
        public RelayCommand MoveLeftCommand { get; private set; }

        /// <summary>
        /// The command to invoke when the Move item right button is pressed.
        /// </summary>
        public RelayCommand MoveRightCommand { get; private set; }

        // Item collections
        public ObservableCollection<ListItemData> ListItemLeft { get; } = 
           new ObservableCollection<ListItemData>();
        public ObservableCollection<ListItemData> ListItemRight { get; } = 
           new ObservableCollection<ListItemData>();

        public ListItemData listItem;

        /// <summary>
        /// Sets up a command to handle invoking the move item buttons.
        /// </summary>
        public UICommand1ViewModel()
        {
            MoveLeftCommand = 
               new RelayCommand(new Action(MoveLeft), CanExecuteMoveLeftCommand);
            MoveRightCommand = 
               new RelayCommand(new Action(MoveRight), CanExecuteMoveRightCommand);

            LoadItems();
        }

        /// <summary>
        ///  Populate our list of items.
        /// </summary>
        public void LoadItems()
        {
            for (var x = 0; x <= 4; x++)
            {
                listItem = new ListItemData();
                listItem.ListItemText = "Item " + (ListItemLeft.Count + 1).ToString();
                listItem.ListItemIcon = Symbol.Emoji;
                ListItemLeft.Add(listItem);
            }
        }

        /// <summary>
        /// Move left command valid when items present in the list on right.
        /// </summary>
        /// <returns>True, if count is greater than 0.</returns>
        private bool CanExecuteMoveLeftCommand()
        {
            return ListItemRight.Count > 0;
        }

        /// <summary>
        /// Move right command valid when items present in the list on left.
        /// </summary>
        /// <returns>True, if count is greater than 0.</returns>
        private bool CanExecuteMoveRightCommand()
        {
            return ListItemLeft.Count > 0;
        }

        /// <summary>
        /// The command implementation to execute when the Move item right button is pressed.
        /// </summary>
        public void MoveRight()
        {
            if (ListItemLeft.Count > 0)
            {
                listItem = new ListItemData();
                ListItemRight.Add(listItem);
                listItem.ListItemText = "Item " + ListItemRight.Count.ToString();
                listItem.ListItemIcon = Symbol.Emoji;
                ListItemLeft.RemoveAt(ListItemLeft.Count - 1);
                MoveRightCommand.RaiseCanExecuteChanged();
                MoveLeftCommand.RaiseCanExecuteChanged();
            }
        }

        /// <summary>
        /// The command implementation to execute when the Move item left button is pressed.
        /// </summary>
        public void MoveLeft()
        {
            if (ListItemRight.Count > 0)
            {
                listItem = new ListItemData();
                ListItemLeft.Add(listItem);
                listItem.ListItemText = "Item " + ListItemLeft.Count.ToString();
                listItem.ListItemIcon = Symbol.Emoji;
                ListItemRight.RemoveAt(ListItemRight.Count - 1);
                MoveRightCommand.RaiseCanExecuteChanged();
                MoveLeftCommand.RaiseCanExecuteChanged();
            }
        }

        /// <summary>
        /// Views subscribe to this event to get notified of property updates.
        /// </summary>
        public event PropertyChangedEventHandler PropertyChanged;

        /// <summary>
        /// Notify subscribers of updates to the named property
        /// </summary>
        /// <param name="propertyName">The full, case-sensitive, name of a property.</param>
        protected void NotifyPropertyChanged(string propertyName)
        {
            PropertyChangedEventHandler handler = this.PropertyChanged;
            if (handler != null)
            {
                PropertyChangedEventArgs args = new PropertyChangedEventArgs(propertyName);
                handler(this, args);
            }
        }
    }

    /// <summary>
    /// Convert a collection count to an opacity value of 0.0 or 1.0.
    /// </summary>
    public class OpacityConverter : IValueConverter
    {
        /// <summary>
        /// Converts a collection count to an opacity value of 0.0 or 1.0.
        /// </summary>
        /// <param name="value">The count passed in</param>
        /// <param name="targetType">Ignored.</param>
        /// <param name="parameter">Ignored</param>
        /// <param name="language">Ignored</param>
        /// <returns>1.0 if count > 0, otherwise returns 0.0</returns>
        public object Convert(object value, Type targetType, object parameter, string language)
        {
            return ((int)value > 0 ? 1.0 : 0.0);
        }

        /// <summary>
        /// Not used, converter is not intended for two-way binding. 
        /// </summary>
        /// <param name="value">Ignored</param>
        /// <param name="targetType">Ignored</param>
        /// <param name="parameter">Ignored</param>
        /// <param name="language">Ignored</param>
        /// <returns></returns>
        public object ConvertBack(object value, Type targetType, object parameter, string language)
        {
            throw new NotImplementedException();
        }
    }
}
```

**最後になりますが、ICommand インターフェイスの実装は次のようになります。**

ここで、[ICommand](/uwp/api/windows.ui.xaml.input.icommand) インターフェイスを実装し、その機能性を他のオブジェクトにリレーするコマンドを定義します。

```csharp
using System;
using System.Windows.Input;

namespace UICommand1
{
    /// <summary>
    /// A command whose sole purpose is to relay its functionality 
    /// to other objects by invoking delegates. 
    /// The default return value for the CanExecute method is 'true'.
    /// <see cref="RaiseCanExecuteChanged"/> needs to be called whenever
    /// <see cref="CanExecute"/> is expected to return a different value.
    /// </summary>
    public class RelayCommand : ICommand
    {
        private readonly Action _execute;
        private readonly Func<bool> _canExecute;

        /// <summary>
        /// Raised when RaiseCanExecuteChanged is called.
        /// </summary>
        public event EventHandler CanExecuteChanged;

        /// <summary>
        /// Creates a new command that can always execute.
        /// </summary>
        /// <param name="execute">The execution logic.</param>
        public RelayCommand(Action execute)
            : this(execute, null)
        {
        }

        /// <summary>
        /// Creates a new command.
        /// </summary>
        /// <param name="execute">The execution logic.</param>
        /// <param name="canExecute">The execution status logic.</param>
        public RelayCommand(Action execute, Func<bool> canExecute)
        {
            if (execute == null)
                throw new ArgumentNullException("execute");
            _execute = execute;
            _canExecute = canExecute;
        }

        /// <summary>
        /// Determines whether this <see cref="RelayCommand"/> can execute in its current state.
        /// </summary>
        /// <param name="parameter">
        /// Data used by the command. If the command does not require 
        /// data to be passed, this object can be set to null.
        /// </param>
        /// <returns>true if this command can be executed; otherwise, false.</returns>
        public bool CanExecute(object parameter)
        {
            return _canExecute == null ? true : _canExecute();
        }

        /// <summary>
        /// Executes the <see cref="RelayCommand"/> on the current command target.
        /// </summary>
        /// <param name="parameter">
        /// Data used by the command. If the command does not require 
        /// data to be passed, this object can be set to null.
        /// </param>
        public void Execute(object parameter)
        {
            _execute();
        }

        /// <summary>
        /// Method used to raise the <see cref="CanExecuteChanged"/> event
        /// to indicate that the return value of the <see cref="CanExecute"/>
        /// method has changed.
        /// </summary>
        public void RaiseCanExecuteChanged()
        {
            var handler = CanExecuteChanged;
            if (handler != null)
            {
                handler(this, EventArgs.Empty);
            }
        }
    }
}
```

## <a name="summary"></a>要約

ユニバーサル Windows プラットフォームからは、コントロールの種類、デバイス、入力の種類を問わず、コマンドを共有し、管理するアプリを構築できる堅牢かつ柔軟なコマンド処理システムが与えられます。

Windows アプリのコマンドを構築するときは、次の手法を使用してください。

- XAML/分離コードのイベントを待ち受け、処理する
- クリックなど、イベント処理メソッドにバインドする
- 独自の ICommand 実装を定義する
- 事前定義された一連のプロパティに独自の値を指定し、XamlUICommand オブジェクトを定義する
- 事前定義された一連のプロパティや値を利用し、StandardUICommand オブジェクトを作成する

## <a name="next-steps"></a>次の手順

[XamlUICommand](/uwp/api/windows.ui.xaml.input.xamluicommand) と [StandardUICommand](/uwp/api/windows.ui.xaml.input.standarduicommand) の実装を実演する完全な例については、[XAML コントロール ギャラリー](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) サンプルをご覧ください。

## <a name="see-also"></a>関連項目

[Windows アプリのコントロールとパターン](index.md)

### <a name="samples"></a>サンプル

#### <a name="topic-samples"></a>トピックのサンプル

- [UWP コマンド処理サンプル (StandardUICommand)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-commanding-standarduicommand.zip)
- [UWP コマンド処理サンプル (XamlUICommand)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-commanding-xamluicommand.zip)
- [UWP コマンド処理サンプル (ICommand)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-commanding-icommand.zip)

#### <a name="other-samples"></a>その他のサンプル

- [ユニバーサル Windows プラットフォームのサンプル (C# と C++)](https://github.com/Microsoft/Windows-universal-samples/tree/b78d95134ce2d57c848e0a8dc339fc362748fb9c/Samples/RadialController)
- [XAML コントロール ギャラリー](https://github.com/Microsoft/Xaml-Controls-Gallery)