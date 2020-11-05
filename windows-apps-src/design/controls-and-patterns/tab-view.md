---
description: TabView は、動的なタブに複数のドキュメントを整理するための柔軟な方法です
title: タブ ビュー
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: f34e2a882746ac833d2b78373a96496c1f079864
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034775"
---
# <a name="tabview"></a>TabView

TabView コントロールは、一連のタブとそれぞれの内容を表示するための手段です。 TabView は、ユーザーに新しいタブを再配置したり、開いたり、閉じたりする機能を提供しながら、コンテンツの複数のページ (またはドキュメント) を表示する場合に便利です。

![TabView の例](images/tabview/tab-introduction.png)

**Windows UI ライブラリを入手する**

:::row:::
   :::column:::
      ![WinUI ロゴ](images/winui-logo-64x64.png)
   :::column-end:::
   :::column span="3":::
      **TabView** コントロールでは、Windows アプリのための新しいコントロールと UI 機能を含む NuGet パッケージである Windows UI ライブラリが必要になります。 インストール手順などについて詳しくは、「[Windows UI Library (Windows UI ライブラリ)](/uwp/toolkits/winui/)」をご覧ください。
   :::column-end:::
   :::column:::

   :::column-end:::
:::row-end:::

> **Windows UI ライブラリ API** : [TabView クラス](/uwp/api/microsoft.ui.xaml.controls.tabview)、 [TabViewItem クラス](/uwp/api/microsoft.ui.xaml.controls.tabviewitem)

> [!TIP]
> このドキュメントでは、XAML で **muxc** エイリアスを使って、プロジェクトに含めた Windows UI Library API を表します。 [Page](/uwp/api/windows.ui.xaml.controls.page) 要素にこれを追加しました。`xmlns:muxc="using:Microsoft.UI.Xaml.Controls"`
>
>コードビハインドでは、C# でも **muxc** エイリアスを使って、プロジェクトに含めた Windows UI Library API を表します。 この **using** ステートメントは、ファイルの先頭に追加されています。`using muxc = Microsoft.UI.Xaml.Controls;`

## <a name="is-this-the-right-control"></a>これは適切なコントロールですか?

一般に、タブ付き UI は、機能と外観が異なる 2 種類のスタイルのいずれかで提供されます。 **静的タブ** は、設定ウィンドウでよく見られるタブの種類です。 通常、内容があらかじめ定義されている固定順序の複数のページが含まれます。
**ドキュメント タブ** は、Microsoft Edge のようなブラウザーで見られるタブの種類です。 ユーザーは、タブの作成、削除、再配置、ウィンドウ間でのタブの移動、タブの内容の変更を行うことができます。

[TabView](/uwp/api/microsoft.ui.xaml.controls.tabview) では、UWP アプリ用のドキュメント タブが提供されています。 次のような場合は TabView を使用します。

- ユーザーが、タブを動的に開いたり、閉じたり、再配置したりできる。
- ユーザーが、ドキュメントや Web ページを直接タブで開くことができる。
- ユーザーが、ウィンドウ間でタブをドラッグ アンド ドロップできる。

TabView がアプリに適していない場合は、[Pivot](./pivot.md) や [NavigationView](./navigationview.md) などのコントロールの使用を検討してください。

## <a name="anatomy"></a>構造

下の図では、[TabView](/uwp/api/microsoft.ui.xaml.controls.tabview) コントロールの各部分を示します。 TabStrip にはヘッダーとフッターがありますが、ドキュメントとは異なり、TabStrip のヘッダーとフッターはそれぞれストリップの左端と右端にあります。

![TabView コントロールの構造](images/tabview/tab-view-anatomy.png)

次の図では、[TabViewItem](/uwp/api/microsoft.ui.xaml.controls.tabviewitem) コントロールの各部分を示します。 コンテンツは TabView コントロールの内部に表示されますが、コンテンツは実際には TabViewItem の一部であることに注意してください。

![TabViewItem コントロールの構造](images/tabview/tab-control-anatomy.png)

### <a name="create-a-tab-view"></a>タブ ビューを作成する

この例では、簡単な [TabView](/uwp/api/microsoft.ui.xaml.controls.tabview) と、タブの開閉をサポートするためのイベント ハンドラーを作成します。

```xaml
 <muxc:TabView AddTabButtonClick="TabView_AddTabButtonClick"
               TabCloseRequested="TabView_TabCloseRequested"/>
```

```csharp
// Add a new Tab to the TabView
private void TabView_AddTabButtonClick(muxc.TabView sender, object args)
{
    var newTab = new muxc.TabViewItem();
    newTab.IconSource = new muxc.SymbolIconSource() { Symbol = Symbol.Document };
    newTab.Header = "New Document";

    // The Content of a TabViewItem is often a frame which hosts a page.
    Frame frame = new Frame();
    newTab.Content = frame;
    frame.Navigate(typeof(Page1));

    sender.TabItems.Add(newTab);
}

// Remove the requested tab from the TabView
private void TabView_TabCloseRequested(muxc.TabView sender, muxc.TabViewTabCloseRequestedEventArgs args)
{
    sender.TabItems.Remove(args.Tab);
}
```

## <a name="behavior"></a>動作

[TabView](/uwp/api/microsoft.ui.xaml.controls.tabview) の機能を利用または拡張する方法は多数あります。

### <a name="bind-tabitemssource-to-a-tabviewitemcollection"></a>TabItemsSource を TabViewItemCollection にバインドする

```xaml
<muxc:TabView TabItemsSource="{x:Bind TabViewItemCollection}" />
```

### <a name="display-tabview-tabs-in-a-windows-titlebar"></a>ウィンドウのタイトル バーに TabView のタブを表示する

ウィンドウのタイトル バーの下にタブ専用の行を挿入するのではなく、2 つを同じ領域に統合できます。 これにより、コンテンツの縦方向の領域が節約され、最新のアプリの感じになります。

ユーザーはウィンドウをタイトル バーでドラッグしてウィンドウの位置を変更できるため、タイトル バーをタブで完全に埋めないことが重要です。 そのため、タイトル バーにタブを表示する場合は、ドラッグ可能な領域として確保するタイトル バーの部分を指定する必要があります。 ドラッグ可能な領域を指定しないと、タイトル バー全体がドラッグ可能になり、タブが入力イベントを受信できなくなります。 TabView をウィンドウのタイトルバーに表示する場合は、常に [TabStripFooter](/uwp/api/microsoft.ui.xaml.controls.tabview.tabstripfooter) を [TabView](/uwp/api/microsoft.ui.xaml.controls.tabview) に含めて、それをドラッグ可能な領域としてマークする必要があります。

詳しくは、「[タイトル バーのカスタマイズ](../shell/title-bar.md)」をご覧ください

![タイトル バーのタブ](images/tabview/tab-extend-to-title.png)

```xaml
<muxc:TabView HorizontalAlignment="Stretch" VerticalAlignment="Stretch">
    <muxc:TabViewItem Header="Home" IsClosable="False">
        <muxc:TabViewItem.IconSource>
            <muxc:SymbolIconSource Symbol="Home" />
        </muxc:TabViewItem.IconSource>
    </muxc:TabViewItem>
    <muxc:TabViewItem Header="Document 1">
        <muxc:TabViewItem.IconSource>
            <muxc:SymbolIconSource Symbol="Document" />
        </muxc:TabViewItem.IconSource>
    </muxc:TabViewItem>
    <muxc:TabViewItem Header="Document 2">
        <muxc:TabViewItem.IconSource>
            <muxc:SymbolIconSource Symbol="Document" />
        </muxc:TabViewItem.IconSource>
    </muxc:TabViewItem>
    <muxc:TabViewItem Header="Document 3">
        <muxc:TabViewItem.IconSource>
            <muxc:SymbolIconSource Symbol="Document" />
        </muxc:TabViewItem.IconSource>
    </muxc:TabViewItem>

    <muxc:TabView.TabStripHeader>
        <Grid x:Name="ShellTitlebarInset" Background="Transparent" />
    </muxc:TabView.TabStripHeader>
    <muxc:TabView.TabStripFooter>
        <Grid x:Name="CustomDragRegion" Background="Transparent" />
    </muxc:TabView.TabStripFooter>
</muxc:TabView>
```

```csharp
public MainPage()
{
    this.InitializeComponent();

    var coreTitleBar = CoreApplication.GetCurrentView().TitleBar;
    coreTitleBar.ExtendViewIntoTitleBar = true;
    coreTitleBar.LayoutMetricsChanged += CoreTitleBar_LayoutMetricsChanged;

    Window.Current.SetTitleBar(CustomDragRegion);
}

private void CoreTitleBar_LayoutMetricsChanged(CoreApplicationViewTitleBar sender, object args)
{
    if (FlowDirection == FlowDirection.LeftToRight)
    {
        CustomDragRegion.MinWidth = sender.SystemOverlayRightInset;
        ShellTitlebarInset.MinWidth = sender.SystemOverlayLeftInset;
    }
    else
    {
        CustomDragRegion.MinWidth = sender.SystemOverlayLeftInset;
        ShellTitlebarInset.MinWidth = sender.SystemOverlayRightInset;
    }

    CustomDragRegion.Height = ShellTitlebarInset.Height = sender.Height;
}
```

>[!NOTE]
> タイトル バーのタブがシェル コンテンツによって詰まってしまわないようにするには、左と右のオーバーレイを考慮する必要があります。 LTR レイアウトでは、右側の挿入部にキャプション ボタンとドラッグ領域が含まれます。 RTL では逆になります。 SystemOverlayLeftInset と SystemOverlayRightInset の値は物理的な左と右を意味するので、RTL ではこれらも逆にします。

### <a name="control-overflow-behavior"></a>コントロールのオーバーフロー動作

タブ バーがタブでいっぱいになったら、[TabView.TabWidthMode](/uwp/api/microsoft.ui.xaml.controls.tabview.tabwidthmode) を設定して、タブの表示方法を制御できます。

| TabWidthMode の値 | 動作                                                                                                                                                    |
|--------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Equal              | 新しいタブが追加されたら、非常に小さい最小幅になるまで、すべてのタブを横方向に縮小します。                                                       |
| SizeToContent      | タブは、常に、アイコンとヘッダーを表示するために必要な最小サイズである "自然サイズ" になります。 タブが追加されたり閉じられたりしても、拡張または縮小されません。 |

どちらの値を選択した場合でも、最終的にタブの数が多すぎてタブ ストリップに表示できなくなる可能性があります。 この場合は、スクロール バンパーが表示され、ユーザーは TabStrip を左右にスクロールできます。

### <a name="guidance-for-tab-selection"></a>タブの選択に関するガイダンス

ほとんどのユーザーは、Web ブラウザーを使用して簡単にドキュメント タブを使用した経験があります。 アプリでドキュメント タブを使用するとき、ユーザーは経験からタブがどのように動作するかを予期します。

ユーザーがドキュメント タブのセットを操作する方法に関係なく、常にアクティブなタブが必要になります。ユーザーが選択したタブを閉じたり、選択したタブを別のウィンドウに分割したりした場合は、別のタブがアクティブになる必要があります。[TabView](/uwp/api/microsoft.ui.xaml.controls.tabview) では、次のタブを自動的に選択することで、これが試みられます。タブが選択されていない TabView をアプリで許可する十分な理由がある場合、TabView のコンテンツ領域は単に空白になります。

## <a name="keyboard-navigation"></a>キーボード ナビゲーション

[TabView](/uwp/api/microsoft.ui.xaml.controls.tabview) では、既定で多くの一般的なキーボード ナビゲーション シナリオがサポートされています。 このセクションでは、組み込みの機能について説明し、一部のアプリで役立つ追加機能についての推奨事項を示します。

### <a name="tab-and-cursor-key-behavior"></a>タブとカーソル キーの動作

フォーカスが _TabStrip_ 領域に移動すると、選択された [TabViewItem](/uwp/api/microsoft.ui.xaml.controls.tabviewitem) がフォーカスを得ます。 ユーザーは左右の方向キーを使用して、TabStrip 内の他のタブにフォーカスを移動できます (選択はされません)。 矢印フォーカスはタブ ストリップ内でトラップされ、ある場合はタブ追加 [+] ボタンが表示されます。 Tab キーを押すとフォーカスを TabStrip 領域の外に移動することができ、フォーカスは次のフォーカス可能な要素に移動します。

Tab キーを使用してフォーカスを移動する

![Tab キーを使用してフォーカスを移動する](images/tabview/tab-keyboard-behavior-1.png)

方向キーではフォーカスは循環しない

![方向キーではフォーカスは循環しない](images/tabview/tab-keyboard-behavior-3.png)

### <a name="selecting-a-tab"></a>タブの選択

TabViewItem にフォーカスがあるときに、Space キーまたは Enter キーを押すと、その TabViewItem が選択されます。

方向キーを使用してフォーカスを移動し、Space キーを押してタブを選択します。

![Space キーでタブを選択する](images/tabview/tab-keyboard-behavior-2.png)

### <a name="shortcuts-for-selecting-adjacent-tabs"></a>隣接するタブを選択するためのショートカット

Ctrl + Tab キーを押すと、次の [TabViewItem](/uwp/api/microsoft.ui.xaml.controls.tabviewitem) が選択されます。 Ctrl + Shift + Tab キーを押すと、前の TabViewItem が選択されます。 これらの目的のため、タブ リストは "ループ" になっているので、最後のタブが選択されているときに次のタブを選択すると、最初のタブが選択されます。

### <a name="closing-a-tab"></a>タブを閉じる

Ctrl + F4 キーを押すと、[TabCloseRequested](/uwp/api/microsoft.ui.xaml.controls.tabview.tabcloserequested) イベントが発生します。 イベントを処理し、必要に応じてタブを閉じます。

### <a name="keyboard-guidance-for-app-developers"></a>アプリ開発者向けのキー ボードガイダンス

アプリケーションによっては、より高度なキーボード制御が必要になる場合があります。 アプリに適している場合は、次のショートカットを実装することを検討してください。

> [!WARNING]
> 既存のアプリに [TabView](/uwp/api/microsoft.ui.xaml.controls.tabview) を追加する場合は、推奨される TabView キーボード ショートカットのキーの組み合わせに対応するキーボード ショートカットが、既に作成されている可能性があります。 この場合は、既存のショートカットを保持するか、ユーザーに直感的なタブ エクスペリエンスを提供するかを、検討する必要があります。

- Ctrl + T キーが押されたら、新しいタブを開く必要があります。通常、このタブは、定義済みのドキュメントを設定されるか、またはコンテンツを選択する簡単な方法を備えて空で作成されます。 ユーザーが新しいタブのコンテンツを選択する必要がある場合は、コンテンツ選択コントロールに入力フォーカスを設定することを検討します。
- Ctrl + W キーが押されたら、選択されているタブを閉じる必要があります。TabView では次のタブが自動的に選択されることを思い出してください。
- Ctrl + Shift + T キーが押されたら、最近閉じられたタブを開く必要があります (より正確には、最近閉じられタブと同じコンテンツで新しいタブを開きます)。 最後に閉じられたタブから始めて、ショートカットが呼び出されるたびに、時間を遡って移動します。 これには、最近閉じられたタブのリストを保持する必要があることに注意してください。
- Ctrl + 1 キーが押されたら、タブ リストの最初のタブを選択します。 同様に、Ctrl + 2 キーが押されたら 2 番目のタブを選択し、Ctrl + 3 キーが押されたら 3 番目のタブを選択します。Ctrl + 8 キーマで同様です。
- Ctrl + 9 キーが押されたら、リスト内のタブの数に関係なく、タブ リストの最後のタブを選択します。
- タブで "閉じる" 以外のコマンド (タブの複製やピン留めなど) も提供する場合は、コンテキスト メニューを使用して、タブで実行できるすべての操作を表示します。

### <a name="implement-browser-style-keyboarding-behavior"></a>ブラウザー スタイルのキーボード動作を実装する

この例では、[TabView](/uwp/api/microsoft.ui.xaml.controls.tabview) に関する上記の推奨事項がいくつか実装されています。 具体的には、この例では Ctrl + T、Ctrl + W、Ctrl + 1-8、Ctrl + 9 が実装されています。

```xaml
<muxc:TabView x:Name="TabRoot">
    <muxc:TabView.KeyboardAccelerators>
        <KeyboardAccelerator Key="T" Modifiers="Control" Invoked="NewTabKeyboardAccelerator_Invoked" />
        <KeyboardAccelerator Key="W" Modifiers="Control" Invoked="CloseSelectedTabKeyboardAccelerator_Invoked" />
        <KeyboardAccelerator Key="Number1" Modifiers="Control" Invoked="NavigateToNumberedTabKeyboardAccelerator_Invoked" />
        <KeyboardAccelerator Key="Number2" Modifiers="Control" Invoked="NavigateToNumberedTabKeyboardAccelerator_Invoked" />
        <KeyboardAccelerator Key="Number3" Modifiers="Control" Invoked="NavigateToNumberedTabKeyboardAccelerator_Invoked" />
        <KeyboardAccelerator Key="Number4" Modifiers="Control" Invoked="NavigateToNumberedTabKeyboardAccelerator_Invoked" />
        <KeyboardAccelerator Key="Number5" Modifiers="Control" Invoked="NavigateToNumberedTabKeyboardAccelerator_Invoked" />
        <KeyboardAccelerator Key="Number6" Modifiers="Control" Invoked="NavigateToNumberedTabKeyboardAccelerator_Invoked" />
        <KeyboardAccelerator Key="Number7" Modifiers="Control" Invoked="NavigateToNumberedTabKeyboardAccelerator_Invoked" />
        <KeyboardAccelerator Key="Number8" Modifiers="Control" Invoked="NavigateToNumberedTabKeyboardAccelerator_Invoked" />
        <KeyboardAccelerator Key="Number9" Modifiers="Control" Invoked="NavigateToNumberedTabKeyboardAccelerator_Invoked" />
    </muxc:TabView.KeyboardAccelerators>
    <!-- ... some tabs ... -->
</muxc:TabView>
```

```csharp
private void NewTabKeyboardAccelerator_Invoked(KeyboardAccelerator sender, KeyboardAcceleratorInvokedEventArgs args)
{
    // Create new tab.
    var newTab = new muxc.TabViewItem();
    newTab.IconSource = new muxc.SymbolIconSource() { Symbol = Symbol.Document };
    newTab.Header = "New Document";

    // The Content of a TabViewItem is often a frame which hosts a page.
    Frame frame = new Frame();
    newTab.Content = frame;
    frame.Navigate(typeof(Page1));

    TabRoot.TabItems.Add(newTab);
}

private void CloseSelectedTabKeyboardAccelerator_Invoked(KeyboardAccelerator sender, KeyboardAcceleratorInvokedEventArgs args)
{
    // Only remove the selected tab if it can be closed.
    if (((muxc.TabViewItem)TabRoot.SelectedItem).IsClosable)
    {
        TabRoot.TabItems.Remove(TabRoot.SelectedItem);
    }
}

private void NavigateToNumberedTabKeyboardAccelerator_Invoked(KeyboardAccelerator sender, KeyboardAcceleratorInvokedEventArgs args)
{
    int tabToSelect = 0;

    switch (sender.Key)
    {
        case Windows.System.VirtualKey.Number1:
            tabToSelect = 0;
            break;
        case Windows.System.VirtualKey.Number2:
            tabToSelect = 1;
            break;
        case Windows.System.VirtualKey.Number3:
            tabToSelect = 2;
            break;
        case Windows.System.VirtualKey.Number4:
            tabToSelect = 3;
            break;
        case Windows.System.VirtualKey.Number5:
            tabToSelect = 4;
            break;
        case Windows.System.VirtualKey.Number6:
            tabToSelect = 5;
            break;
        case Windows.System.VirtualKey.Number7:
            tabToSelect = 6;
            break;
        case Windows.System.VirtualKey.Number8:
            tabToSelect = 7;
            break;
        case Windows.System.VirtualKey.Number9:
            // Select the last tab
            tabToSelect = TabRoot.TabItems.Count - 1;
            break;
    }

    // Only select the tab if it is in the list
    if (tabToSelect < TabRoot.TabItems.Count)
    {
        TabRoot.SelectedIndex = tabToSelect;
    }
}
```

## <a name="related-articles"></a>関連記事

- [MasterDetails](./master-details.md)
- [NavigationView](./navigationview.md)
- [ピボット](./pivot.md)
