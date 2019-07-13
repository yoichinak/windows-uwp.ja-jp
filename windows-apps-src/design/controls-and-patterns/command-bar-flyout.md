---
Description: コマンド バーのポップアップを使用すると、ユーザーはアプリの最も一般的なタスクにインラインでアクセスできます。
title: コマンド バーのポップアップ
label: Command bar flyout
template: detail.hbs
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: abarlow
design-contact: ksulliv
dev-contact: llongley
doc-status: Draft
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: d5774b5301f7e8ce0616df72cfbf4fc81d0d0cf7
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/13/2019
ms.locfileid: "66363253"
---
# <a name="command-bar-flyout"></a>コマンド バーのポップアップ

コマンド バーのポップアップを使用すると、UI キャンバス上の要素に関連した移動可能なツールバーにコマンドを表示することによって、一般的なタスクへの簡単なアクセスをユーザーに提供できます。

![展開されたテキスト コマンド バーのポップアップ](images/command-bar-flyout-header.png)

> CommandBarFlyout には、Windows 10 Version 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) 以降、または [Windows UI ライブラリ](https://docs.microsoft.com/uwp/toolkits/winui/)が必要です。

> - **プラットフォーム API**: [CommandBarFlyout クラス](/uwp/api/windows.ui.xaml.controls.commandbarflyout)、[TextCommandBarFlyout クラス](/uwp/api/windows.ui.xaml.controls.textcommandbarflyout)、[AppBarButton クラス](/uwp/api/windows.ui.xaml.controls.appbarbutton)、[AppBarToggleButton クラス](/uwp/api/windows.ui.xaml.controls.appbartogglebutton)、[AppBarSeparator クラス](/uwp/api/windows.ui.xaml.controls.appbarseparator)
>- **Windows UI ライブラリ API**: [CommandBarFlyout クラス](/uwp/api/microsoft.ui.xaml.controls.commandbarflyout)、[TextCommandBarFlyout クラス](/uwp/api/microsoft.ui.xaml.controls.textcommandbarflyout)

[CommandBar](app-bars.md) と同様に、CommandBarFlyout には、コマンドを追加するために使用できる **PrimaryCommands** プロパティと **SecondaryCommands** プロパティがあります。 コマンドは、どちらかまたは両方のコレクションに配置できます。 プライマリとセカンダリのコマンドが、どのような場合にどのような方法で表示されるかは、表示モードによって異なります。

コマンド バーのポップアップには、"*折りたたみ*" と "*展開*" の 2 つの表示モードがあります。

- 折りたたみモードでは、プライマリ コマンドのみが表示されます。 コマンド バーのポップアップにプライマリとセカンダリの両方のコマンドが含まれる場合は、省略記号 \[•••\] によって表される "see more" (詳細表示) ボタンが表示されます。 これにより、ユーザーは展開モードに切り替えることでセカンダリ コマンドにアクセスできます。
- 展開モードでは、プライマリとセカンダリの両方のコマンドが表示されます (コントロールにセカンダリ項目のみが含まれる場合、それらの項目は MenuFlyout コントロールと同様の方法で表示されます)。

## <a name="is-this-the-right-control"></a>適切なコントロールの選択

アプリ キャンバス上の要素のコンテキスト内でボタンやメニュー項目などのコマンドのコレクションをユーザーに表示するには、CommandBarFlyout コントロールを使用します。

TextCommandBarFlyout では、TextBox、TextBlock、RichEditBox、RichTextBlock、および PasswordBox コントロールにテキスト コマンドが表示されます。 コマンドは、現在のテキスト選択に応じて適切に、自動的に構成されます。 テキスト コントロールの既定のテキスト コマンドを置き換えるには、CommandBarFlyout を使用します。

リスト アイテムのコンテキスト コマンドを表示するには、「[コレクションとリストのコンテキスト コマンドの実行](collection-commanding.md)」にあるガイダンスに従います。

### <a name="commandbarflyout-vs-menuflyout"></a>CommandBarFlyout と MenuFlyout

コンテキスト メニューにコマンドを表示するには、CommandBarFlyout または MenuFlyout を使用できます。 MenuFlyout よりも多くの機能が提供されているため、CommandBarFlyout をお勧めします。 セカンダリ コマンドのみを含む CommandBarFlyout を使用して MenuFlyout の動作と外観を得ることも、プライマリとセカンダリの両方のコマンドを含む完全なコマンド バーのポップアップを使用することもできます。

> 関連する情報については、「[ポップアップ](../controls-and-patterns/dialogs-and-flyouts/flyouts.md)」、「[メニューとショートカット メニュー](menus.md)」、「[コマンド バー](app-bars.md)」を参照してください。

## <a name="examples"></a>例

<table>
<th align="left">XAML コントロール ギャラリー<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML コントロール ギャラリー</strong> アプリがインストールされている場合、こちらをクリックして<a href="xamlcontrolsgallery:/item/CommandBarFlyout">アプリを開き、CommandBarFlyout の動作を確認</a>してください。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML コントロール ギャラリー アプリを入手する (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">ソース コード (GitHub) を入手する</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="proactive-vs-reactive-invocation"></a>事前と事後の呼び出し

UI キャンバス上の要素に関連付けられたポップアップまたはメニューを呼び出すには、通常、2 つの方法があります。_事前呼び出し_と_事後呼び出し_です。

事前呼び出しでは、コマンドは、コマンドが関連付けられた項目をユーザーが操作するときに自動的に表示されます。 たとえば、テキストの書式設定コマンドは、ユーザーがテキスト ボックス内のテキストを選択したときにポップアップ表示することができます。 この場合、コマンド バーのポップアップにフォーカスは移りません。 そうではなく、ユーザーが操作している項目の近くに関連するコマンドが表示されます。 ユーザーがコマンドを操作しない場合、コマンドは閉じられます。

事後呼び出しでは、コマンドは、コマンドを要求する明示的なユーザー操作 (たとえば右クリック) に対する応答として表示されます。 これは、従来の[コンテキスト メニュー](menus.md)の概念に対応します。

CommandBarFlyout は、どちらの方法でも使用できます。または、2 つの方法を混合することもできます。

## <a name="create-a-command-bar-flyout"></a>コマンド バーのポップアップの作成

この例では、コマンド バーのポップアップを作成し、事前および事後に使用する方法を示します。 画像がタップされたとき、ポップアップが折りたたみモードで表示されます。 コンテキスト メニューとして表示されるとき、ポップアップは展開モードで表示されます。 どちらの場合も、ユーザーはポップアップが開いた後に、ポップアップを展開するか折りたたむことができます。

![折りたたまれたコマンド バーのポップアップの例](images/command-bar-flyout-img-collapsed.png)

> _折りたたまれたコマンド バーのポップアップ_

![展開されたコマンド バーのポップアップの例](images/command-bar-flyout-img-expanded.png)

> _展開されたコマンド バーのポップアップ_

```xaml
<Grid>
    <Grid.Resources>
        <CommandBarFlyout x:Name="ImageCommandsFlyout">
            <AppBarButton Icon="OutlineStar" ToolTipService.ToolTip="Favorite"/>
            <AppBarButton Icon="Copy" ToolTipService.ToolTip="Copy"/>
            <AppBarButton Icon="Share" ToolTipService.ToolTip="Share"/>
            <CommandBarFlyout.SecondaryCommands>
                <AppBarButton Label="Select all"/>
                <AppBarButton Label="Delete" Icon="Delete"/>
            </CommandBarFlyout.SecondaryCommands>
        </CommandBarFlyout>
    </Grid.Resources>

    <Image Source="Assets/image1.png" Width="300"
           Tapped="Image_Tapped" FlyoutBase.AttachedFlyout="{x:Bind ImageCommandsFlyout}"
           ContextFlyout="{x:Bind ImageCommandsFlyout}"/>
</Grid>
```

```csharp
private void Image_Tapped(object sender, TappedRoutedEventArgs e)
{
    var flyout = FlyoutBase.GetAttachedFlyout((FrameworkElement)sender);
    var options = new FlyoutShowOptions()
    {
        // Position shows the flyout next to the pointer.
        // "Transient" ShowMode makes the flyout open in its collapsed state.
        Position = e.GetPosition((FrameworkElement)sender),
        ShowMode = FlyoutShowMode.Transient
    };
    flyout?.ShowAt((FrameworkElement)sender, options);
}
```

### <a name="show-commands-proactively"></a>コマンドの事前表示

コンテキストに応じたコマンドを事前に表示するとき、既定ではプライマリ コマンドのみが表示されます (コマンド バーのポップアップは折りたたまれます)。 最も重要なコマンドをプライマリ コマンドのコレクションに配置し、従来のコンテキスト メニューに表示されるその他のコマンドをセカンダリ コマンドのコレクションに配置します。

コマンドを事前に表示するには、通常、[Click](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) または [Tapped](/uwp/api/windows.ui.xaml.uielement.tapped) イベントを処理してコマンド バーのポップアップを表示します。 ポップアップの [ShowMode](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.showmode) を **Transient** または **TransientWithDismissOnPointerMoveAway** に設定して、フォーカスを移さずに折りたたみモードでポップアップを開きます。

Windows 10 Insider Preview 以降、テキスト コントロールに **SelectionFlyout** プロパティが用意されています。 このプロパティにポップアップを割り当てると、テキストが選択されたときにポップアップが自動的に表示されます。

### <a name="show-commands-reactively"></a>コマンドの事後表示

コンテキスト コマンドをコンテキスト メニューとして事後表示すると、セカンダリ コマンドが既定で表示されます (コマンド バーのポップアップは展開されます)。 この場合、コマンド バーのポップアップには、プライマリとセカンダリの両方のコマンドが含まれる場合と、セカンダリ コマンドのみが含まれる場合があります。

コンテキスト メニューにコマンドを表示するには、通常、UI 要素の [ContextFlyout](/uwp/api/windows.ui.xaml.uielement.contextflyout) プロパティにポップアップを割り当てます。 この方法では、ポップアップを開く処理がその要素によって行われ、それ以上何もする必要がありません。

ポップアップの表示を (たとえば [RightTapped](/uwp/api/windows.ui.xaml.uielement.righttapped) イベント発生時に) 自分で処理する場合は、ポップアップの [ShowMode](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.showmode) を **Standard** に設定して、展開モードでポップアップを開いてフォーカスを移します。

> [!TIP]
> ポップアップを表示するときのオプションおよびポップアップの配置を制御する方法の詳細については、「[ポップアップ](../controls-and-patterns/dialogs-and-flyouts/flyouts.md)」を参照してください。

## <a name="commands-and-content"></a>コマンドとコンテンツ

CommandBarFlyout コントロールには、コマンドおよびコンテンツを追加するために使用できるプロパティが 2 つあります。[PrimaryCommands](/uwp/api/windows.ui.xaml.controls.commandbarflyout.primarycommands) と [SecondaryCommands](/uwp/api/windows.ui.xaml.controls.commandbarflyout.secondarycommands) です。

既定では、コマンド バーの項目は **PrimaryCommands** コレクションに追加されます。 これらのコマンドはコマンド バーに表示され、折りたたみモードと展開モードの両方で表示されます。 CommandBar とは異なり、プライマリ コマンドがセカンダリ コマンドに自動的にオーバーフローされることはなく、プライマリ コマンドは切り捨てられる可能性があります。

**SecondaryCommands** コレクションにコマンドを追加することもできます。 セカンダリ コマンドは、コントロールのメニューの部分に表示され、展開モードでのみ表示されます。

### <a name="app-bar-buttons"></a>アプリ バーのボタン

PrimaryCommands と SecondaryCommands には、[AppBarButton](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBarButton)、[AppBarToggleButton](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBarToggleButton)、[AppBarSeparator](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBarSeparator) の各コントロールを直接入力できます。

アプリ バーのボタン コントロールは、アイコンとテキスト ラベルによって特徴付けられます。 これらのコントロールは、コマンド バーで使うように最適化されており、コマンド バーとオーバーフロー メニューのどちらに表示されるかに応じて外観が変化します。

- プライマリ コマンドとして使用されるアプリ バーのボタンは、アイコンだけがコマンド バーに表示されます。テキスト ラベルは表示されません。 コマンドの説明テキストを表示するには、次に示すようにヒントを使用することをお勧めします。
    ```xaml
    <AppBarButton Icon="Copy" ToolTipService.ToolTip="Copy"/>
    ```
- セカンダリ コマンドとして使用されるアプリ バー ボタンは、ラベルとアイコンの両方がメニューに表示されます。

### <a name="other-content"></a>その他のコンテンツ

他のコントロールを AppBarElementContainer にラップすることによって、コマンド バーのポップアップに追加できます。 これにより、[DropDownButton](buttons.md) や [SplitButton](buttons.md) などのコントロールを追加したり、[StackPanel](buttons.md) のようなコンテナーを追加したりして、より複雑な UI を作成できます。

コマンド バーのポップアップの、プライマリまたはセカンダリのコマンドのコレクションに要素を追加するには、要素で [ICommandBarElement](/uwp/api/windows.ui.xaml.controls.icommandbarelement) インターフェイスを実装する必要があります。 AppBarElementContainer は、このインターフェイスを実装するラッパーです。これにより、要素自体にインターフェイスが実装されていない場合でも、コマンド バーに要素を追加できます。

ここでは、AppBarElementContainer を使用してコマンド バーのポップアップに追加の要素を加えています。 色の選択を可能にするために、SplitButton がプライマリ コマンドに追加されます。 ズーム コントロールのより複雑なレイアウトを可能にするために、StackPanel がセカンダリ コマンドに追加されます。

> [!TIP]
> 既定では、アプリ キャンバス用に設計された要素は、コマンド バーで正しく表示されない可能性があります。 AppBarElementContainer を使用して要素を追加するときには、その要素を他のコマンド バー要素と一致させるために実行する必要がある、いくつかの手順があります。
>
> - 既定のブラシを [lightweight styling](/windows/uwp/design/controls-and-patterns/xaml-styles#lightweight-styling) でオーバーライドして、要素の背景と境界線をアプリ バーのボタンと一致させます。
> - 要素のサイズと位置を調整します。
> - 16 ピクセルの Width と Height を持つ Viewbox にアイコンをラップします。

> [!NOTE]
> この例では、コマンド バーのポップアップ UI のみを示しています。示されているコマンドはどれも実装されていません。 コマンドの実装の詳細については、「[ボタン](buttons.md)」および[コマンド設計の基本](../basics/commanding-basics.md)に関するページを参照してください。

![分割ボタンを使用したコマンド バーのポップアップ](images/command-bar-flyout-split-button.png)

> _開いた SplitButton を持つ折りたたまれたコマンド バーのポップアップ_

![複雑な UI を使用したコマンド バーのポップアップ](images/command-bar-flyout-custom-ui.png)

> _メニューにカスタム ズーム UI が表示された、展開されたコマンド バーのポップアップ_


```xaml
<CommandBarFlyout>
    <AppBarButton Icon="Cut" ToolTipService.ToolTip="Cut"/>
    <AppBarButton Icon="Copy" ToolTipService.ToolTip="Copy"/>
    <AppBarButton Icon="Paste" ToolTipService.ToolTip="Paste"/>
    <!-- Alignment controls -->
    <AppBarElementContainer>
        <SplitButton ToolTipService.ToolTip="Alignment">
            <SplitButton.Resources>
                <!-- Override default brushes to make the SplitButton 
                     match other command bar elements. -->
                <Style TargetType="SplitButton">
                    <Setter Property="Height" Value="38"/>
                </Style>
                <SolidColorBrush x:Key="SplitButtonBackground"
                                 Color="Transparent"/>
                <SolidColorBrush x:Key="SplitButtonBackgroundPressed"
                                 Color="{ThemeResource SystemListMediumColor}"/>
                <SolidColorBrush x:Key="SplitButtonBackgroundPointerOver"
                                 Color="{ThemeResource SystemListLowColor}"/>
                <SolidColorBrush x:Key="SplitButtonBorderBrush" Color="Transparent"/>
                <SolidColorBrush x:Key="SplitButtonBorderBrushPointerOver"
                                 Color="Transparent"/>
                <SolidColorBrush x:Key="SplitButtonBorderBrushChecked"
                                 Color="Transparent"/>
            </SplitButton.Resources>
            <SplitButton.Content>
                <Viewbox Width="16" Height="16" Margin="0,2,0,0">
                    <SymbolIcon Symbol="AlignLeft"/>
                </Viewbox>
            </SplitButton.Content>
            <SplitButton.Flyout>
                <MenuFlyout>
                    <MenuFlyoutItem Icon="AlignLeft" Text="Align left"/>
                    <MenuFlyoutItem Icon="AlignCenter" Text="Center"/>
                    <MenuFlyoutItem Icon="AlignRight" Text="Align right"/>
                </MenuFlyout>
            </SplitButton.Flyout>
        </SplitButton>
    </AppBarElementContainer>
    <!-- end Alignment controls -->
    <CommandBarFlyout.SecondaryCommands>
        <!-- Zoom controls -->
        <AppBarElementContainer>
            <AppBarElementContainer.Resources>
                <!-- Override default brushes to make the Buttons 
                     match other command bar elements. -->
                <SolidColorBrush x:Key="ButtonBackground"
                                 Color="Transparent"/>
                <SolidColorBrush x:Key="ButtonBackgroundPressed"
                                 Color="{ThemeResource SystemListMediumColor}"/>
                <SolidColorBrush x:Key="ButtonBackgroundPointerOver"
                                 Color="{ThemeResource SystemListLowColor}"/>
                <SolidColorBrush x:Key="ButtonBorderBrush"
                                 Color="Transparent"/>
                <SolidColorBrush x:Key="ButtonBorderBrushPointerOver"
                                 Color="Transparent"/>
                <SolidColorBrush x:Key="ButtonBorderBrushChecked"
                                 Color="Transparent"/>
                <Style TargetType="TextBlock">
                    <Setter Property="VerticalAlignment" Value="Center"/>
                </Style>
                <Style TargetType="Button">
                    <Setter Property="Height" Value="40"/>
                    <Setter Property="Width" Value="40"/>
                </Style>
            </AppBarElementContainer.Resources>
            <Grid Margin="12,-4">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="Auto"/>
                    <ColumnDefinition Width="76"/>
                    <ColumnDefinition Width="Auto"/>
                </Grid.ColumnDefinitions>
                <Viewbox Width="16" Height="16" Margin="0,2,0,0">
                    <SymbolIcon Symbol="Zoom"/>
                </Viewbox>
                <TextBlock Text="Zoom" Margin="10,0,0,0" Grid.Column="1"/>
                <StackPanel Orientation="Horizontal" Grid.Column="2">
                    <Button ToolTipService.ToolTip="Zoom out">
                        <Viewbox Width="16" Height="16">
                            <SymbolIcon Symbol="ZoomOut"/>
                        </Viewbox>
                    </Button>
                    <TextBlock Text="50%" Width="40"
                               HorizontalTextAlignment="Center"/>
                    <Button ToolTipService.ToolTip="Zoom in">
                        <Viewbox Width="16" Height="16">
                            <SymbolIcon Symbol="ZoomIn"/>
                        </Viewbox>
                    </Button>
                </StackPanel>
            </Grid>
        </AppBarElementContainer>
        <!-- end Zoom controls -->
        <AppBarSeparator/>
        <AppBarButton Label="Undo" Icon="Undo"/>
        <AppBarButton Label="Redo" Icon="Redo"/>
        <AppBarButton Label="Select all" Icon="SelectAll"/>
    </CommandBarFlyout.SecondaryCommands>
</CommandBarFlyout>
```

## <a name="create-a-context-menu-with-secondary-commands-only"></a>セカンダリ コマンドのみを含むコンテキスト メニューの作成

セカンダリ コマンドのみを含む CommandBarFlyout を[コンテキスト メニュー](menus.md)として MenuFlyout の代わりに使用できます。

![セカンダリ コマンドのみを含むコマンド バーのポップアップ](images/command-bar-flyout-context-menu.png)

> _コンテキスト メニューとしてのコマンド バーのポップアップ_

```xaml
<Grid>
    <Grid.Resources>
        <!-- A command bar flyout with only secondary commands. -->
        <CommandBarFlyout x:Name="ContextMenu">
            <CommandBarFlyout.SecondaryCommands>
                <AppBarButton Label="Copy" Icon="Copy"/>
                <AppBarButton Label="Save" Icon="Save"/>
                <AppBarButton Label="Print" Icon="Print"/>
                <AppBarSeparator />
                <AppBarButton Label="Properties"/>
            </CommandBarFlyout.SecondaryCommands>
        </CommandBarFlyout>
    </Grid.Resources>

    <Image Source="Assets/image1.png" Width="300"
           ContextFlyout="{x:Bind ContextMenu}"/>
</Grid>
```

CommandBarFlyout を DropDownButton と共に使用して、標準メニューを作成することもできます。

![ドロップダウン ボタン メニューを使用したコマンド バーのポップアップ](images/command-bar-flyout-dropdown.png)

> _コマンド バーのポップアップでのドロップダウン ボタン メニュー_

```xaml
<CommandBarFlyout>
    <AppBarButton Icon="Placeholder"/>
    <AppBarElementContainer>
        <DropDownButton Content="Mail">
            <DropDownButton.Resources>
                <!-- Override default brushes to make the DropDownButton 
                     match other command bar elements. -->
                <Style TargetType="DropDownButton">
                    <Setter Property="Height" Value="38"/>
                </Style>
                <SolidColorBrush x:Key="ButtonBackground"
                                 Color="Transparent"/>
                <SolidColorBrush x:Key="ButtonBackgroundPressed"
                                 Color="{ThemeResource SystemListMediumColor}"/>
                <SolidColorBrush x:Key="ButtonBackgroundPointerOver"
                                 Color="{ThemeResource SystemListLowColor}"/>

                <SolidColorBrush x:Key="ButtonBorderBrush"
                                 Color="Transparent"/>
                <SolidColorBrush x:Key="ButtonBorderBrushPointerOver"
                                 Color="Transparent"/>
                <SolidColorBrush x:Key="ButtonBorderBrushChecked"
                                 Color="Transparent"/>
            </DropDownButton.Resources>
            <DropDownButton.Flyout>
                <CommandBarFlyout Placement="BottomEdgeAlignedLeft">
                    <CommandBarFlyout.SecondaryCommands>
                        <AppBarButton Icon="MailReply" Label="Reply"/>
                        <AppBarButton Icon="MailReplyAll" Label="Reply all"/>
                        <AppBarButton Icon="MailForward" Label="Forward"/>
                    </CommandBarFlyout.SecondaryCommands>
                </CommandBarFlyout>
            </DropDownButton.Flyout>
        </DropDownButton>
    </AppBarElementContainer>
    <AppBarButton Icon="Placeholder"/>
    <AppBarButton Icon="Placeholder"/>
</CommandBarFlyout>
```

## <a name="command-bar-flyouts-for-text-controls"></a>テキスト コントロール用のコマンド バーのポップアップ

[TextCommandBarFlyout](/uwp/api/microsoft.ui.xaml.controls.textcommandbarflyout) は、テキストを編集するためのコマンドを含む特殊なコマンド バーのポップアップです。 各テキスト コントロールでは、コンテキスト メニュー (右クリック) として、またはテキストが選択されたときに、自動的に TextCommandBarFlyout が表示されます。 テキスト コマンド バーのポップアップでは、テキストの選択に適応して、関連するコマンドのみが表示されます。

![折りたたまれたテキスト コマンド バーのポップアップ](images/command-bar-flyout-text-selection.png)

> _テキスト選択時のテキスト コマンド バーのポップアップ_

![展開されたテキスト コマンド バーのポップアップ](images/command-bar-flyout-text-full.png)

> _展開されたテキスト コマンド バーのポップアップ_


### <a name="available-commands"></a>使用可能なコマンド

次の表に、TextCommandBarFlyout に含まれているコマンドと、各コマンドがどのような場合に表示されるかを示します。

| コマンド | 表示される場合 |
| ------- | -------- |
| Bold | テキスト コントロールが読み取り専用でない場合 (RichEditBox のみ)。 |
| Italic | テキスト コントロールが読み取り専用でない場合 (RichEditBox のみ)。 |
| Underline | テキスト コントロールが読み取り専用でない場合 (RichEditBox のみ)。 |
| Proofing | IsSpellCheckEnabled が **true** で、スペル ミスのテキストが選択されている場合。 |
| 切り取り | テキスト コントロールが読み取り専用ではなく、テキストが選択されている場合。 |
| コピー | テキストが選択されている場合。 |
| Paste | テキスト コントロールが読み取り専用ではなく、クリップボードが空でない場合。 |
| 元に戻す | 元に戻すことができるアクションがある場合。 |
| すべて選択 | テキストを選択できる場合。 |

### <a name="custom-text-command-bar-flyouts"></a>カスタム テキスト コマンド バーのポップアップ

TextCommandBarFlyout はカスタマイズできず、各テキスト コントロールによって自動的に管理されます。 ただし、既定の TextCommandBarFlyout をカスタム コマンドで置き換えることができます。

- テキスト選択時に表示される既定の TextCommandBarFlyout を置き換えるために、カスタム CommandBarFlyout (またはその他の種類のポップアップ) を作成して、**SelectionFlyout** プロパティに割り当てることができます。 SelectionFlyout を **null** に設定した場合、選択時にコマンドは表示されません。
- コンテキスト メニューとして表示される既定の TextCommandBarFlyout を置き換えるには、カスタムの CommandBarFlyout (またはその他の種類のポップアップ) をテキスト コントロールの **ContextFlyout** プロパティに割り当てます。 ContextFlyout を **null** に設定すると、以前のバージョンのテキスト コントロールに表示されるメニュー ポップアップが、TextCommandBarFlyout の代わりに表示されます。

## <a name="get-the-sample-code"></a>サンプル コードを入手する

- [XAML コントロール ギャラリー サンプル](https://github.com/Microsoft/Xaml-Controls-Gallery) - インタラクティブな形で XAML コントロールのすべてを参照できます。
- [XAML コマンド実行のサンプル](https://go.microsoft.com/fwlink/p/?LinkId=620019)

## <a name="related-articles"></a>関連記事

- [UWP アプリのコマンド設計の基本](../basics/commanding-basics.md)
- [CommandBar クラス](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CommandBar)
