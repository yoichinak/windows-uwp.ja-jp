---
description: コマンド バー コントロールを使用して、ユーザーがアプリの最も一般的なコマンドとタスクに簡単にアクセスできるようにする方法について説明します。
title: コマンド バー
label: App bars/command bars
template: detail.hbs
op-migration-status: ready
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 868b4145-319b-4a97-82bd-c98d966144db
pm-contact: yulikl
design-contact: ksulliv
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 2a84dcc209fa0fcd897668293cb136a5448e7254
ms.sourcegitcommit: 4f032d7bb11ea98783db937feed0fa2b6f9950ef
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/08/2020
ms.locfileid: "91829517"
---
# <a name="command-bar"></a>コマンド バー

コマンド バーを使うと、ユーザーはアプリの最も一般的なタスクに簡単にアクセスできます。 コマンド バーを使うと、アプリ レベルまたはページに固有のコマンドにアクセスできます。これは、ナビゲーション パターンと共に使用することもできます。

> **プラットフォーム API:** [CommandBar クラス](/uwp/api/windows.ui.xaml.controls.commandbar)、[AppBarButton クラス](/uwp/api/windows.ui.xaml.controls.appbarbutton)、[AppBarToggleButton クラス](/uwp/api/windows.ui.xaml.controls.appbartogglebutton)、[AppBarSeparator クラス](/uwp/api/windows.ui.xaml.controls.appbarseparator)

![アイコンを含むコマンド バーの例](images/controls_appbar_icons.png)

## <a name="is-this-the-right-control"></a>これは適切なコントロールですか?

CommandBar コントロールは、汎用的で柔軟、軽量なコントロールです。画像やテキスト ブロックなどの複雑なコンテンツも、[AppBarButton](/uwp/api/windows.ui.xaml.controls.appbarbutton)、[AppBarToggleButton](/uwp/api/windows.ui.xaml.controls.appbartogglebutton)、[AppBarSeparator](/uwp/api/windows.ui.xaml.controls.appbarseparator) コントロールなどの単純なコマンドも表示できます。

> [!NOTE]
> XAML では、[AppBar](/uwp/api/windows.ui.xaml.controls.appbar) コントロールと [CommandBar](/uwp/api/windows.ui.xaml.controls.commandbar) コントロールの両方が提供されます。 AppBar を使うユニバーサル Windows 8 アプリをアップグレードする場合にのみ、AppBar を使ってください。また、変更は最小限に抑える必要があります。 Windows 10 の新しいアプリでは、代わりに CommandBar コントロールを使うことをお勧めします。 このドキュメントでは、CommandBar コントロールを使うことを前提としています。

## <a name="examples"></a>例

<table>
<th align="left">XAML コントロール ギャラリー<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML コントロール ギャラリー</strong> アプリがインストールされている場合、こちらをクリックして<a href="xamlcontrolsgallery:/item/CommandBar">アプリを開き、CommandBar の動作を確認</a>してください。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML コントロール ギャラリー アプリを入手する (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">ソース コード (GitHub) を入手する</a></li>
    </ul>
</td>
</tr>
</table>

展開されたコマンド バー。

![展開されたコマンド バー](images/control-examples/command-bar-photos.png)

## <a name="anatomy"></a>構造

既定では、コマンド バーには、一連のアイコン ボタンとオプションの [その他] ボタン (省略記号の \[•••\]) が表示されます。 後で示すコード例を使って作成されたコマンド バーを次に示します。 コマンド バーは、閉じたコンパクトな状態で表示されます。

![閉じたコマンド バーを示すスクリーンショット。](images/command-bar-compact.png)

コマンド バーは、次のように、閉じた最小の状態で表示することもできます。 詳しくは、「[開いた状態と閉じた状態](#open-and-closed-states)」をご覧ください。

![閉じた最小状態のコマンド バーを示すスクリーンショット。](images/command-bar-minimal.png)

同じコマンド バーが開いている状態を次に示します。 ラベルは、コントロールのメイン部分を識別します。

![開いた状態のコマンド バーを示すスクリーンショット。](images/commandbar_anatomy_open.png)

コマンド バーは、4 つの主な領域に分かれています。
- コンテンツ領域はバーの左側に配置されます。 これは、[Content](/uwp/api/windows.ui.xaml.controls.contentcontrol.content) プロパティが指定されている場合に表示されます。
- 基本コマンド領域はバーの右側に配置されます。 これは、[PrimaryCommands](/uwp/api/windows.ui.xaml.controls.commandbar.primarycommands) プロパティが指定されている場合に表示されます。  
- [その他] (\[•••\]) ボタンはバーの右側に表示されます。 [その他] (\[•••\]) ボタンを押すと、プライマリ コマンドのラベルが表示され、セカンダリ コマンドが存在する場合はオーバーフロー メニューが開きます。 このボタンは、プライマリ コマンド ラベルもセカンダリ ラベルもない場合には表示されません。 既定の動作を変更するには、[OverflowButtonVisibility](/uwp/api/windows.ui.xaml.controls.commandbar.overflowbuttonvisibility) プロパティを使います。
- オーバーフロー メニューは、コマンド バーが開いていて、[SecondaryCommands](/uwp/api/windows.ui.xaml.controls.commandbar.secondarycommands) プロパティが指定されている場合にのみ表示されます。 スペースが限られている場合に、プライマリ コマンドは SecondaryCommands 領域に移動されます。 既定の動作を変更するには、[IsDynamicOverflowEnabled](/uwp/api/windows.ui.xaml.controls.commandbar.isdynamicoverflowenabled) プロパティを使います。

[FlowDirection](/uwp/api/windows.ui.xaml.frameworkelement.flowdirection) が **RightToLeft** のときは、レイアウトが逆になります。

## <a name="create-a-command-bar"></a>コマンド バーの作成
次の例では、上に示したコマンド バーが作成されます。

```xaml
<CommandBar>
    <AppBarToggleButton Icon="Shuffle" Label="Shuffle" Click="AppBarButton_Click" />
    <AppBarToggleButton Icon="RepeatAll" Label="Repeat" Click="AppBarButton_Click"/>
    <AppBarSeparator/>
    <AppBarButton Icon="Back" Label="Back" Click="AppBarButton_Click"/>
    <AppBarButton Icon="Stop" Label="Stop" Click="AppBarButton_Click"/>
    <AppBarButton Icon="Play" Label="Play" Click="AppBarButton_Click"/>
    <AppBarButton Icon="Forward" Label="Forward" Click="AppBarButton_Click"/>

    <CommandBar.SecondaryCommands>
        <AppBarButton Label="Like" Click="AppBarButton_Click"/>
        <AppBarButton Label="Dislike" Click="AppBarButton_Click"/>
    </CommandBar.SecondaryCommands>

    <CommandBar.Content>
        <TextBlock Text="Now playing..." Margin="12,14"/>
    </CommandBar.Content>
</CommandBar>
```

## <a name="commands-and-content"></a>コマンドとコンテンツ
CommandBar コントロールには、[PrimaryCommands](/uwp/api/windows.ui.xaml.controls.commandbar.primarycommands)、[SecondaryCommands](/uwp/api/windows.ui.xaml.controls.commandbar.secondarycommands)、[Content](/uwp/api/windows.ui.xaml.controls.contentcontrol.content) の 3 つのプロパティがあり、コマンドとコンテンツを追加するために使うことができます。


### <a name="commands"></a>コマンド

既定では、コマンド バーの項目は **PrimaryCommands** コレクションに追加されます。 最も重要なコマンドを常に表示できるように、重要度順にコマンドを追加する必要があります。 ユーザーによるアプリ ウィンドウのサイズ変更などでコマンド バーの幅が変化した場合、プライマリ コマンドはブレークポイントを境としてコマンド バーとオーバーフロー メニューの間を動的に移動します。 この既定の動作を変更するには、[IsDynamicOverflowEnabled](/uwp/api/windows.ui.xaml.controls.commandbar.isdynamicoverflowenabled) プロパティを使います。 

最小画面 (幅 320 epx) には、最大 4 つのプライマリ コマンドを、コマンド バーに配置できます。 

**SecondaryCommands** コレクションにコマンドを追加することもできます。これらは、オーバーフロー メニューに表示されます。

![[その他] 領域とアイコンがあるコマンド バーの例](images/appbar_rs2_overflow_icons.png)

必要に応じて、プログラムを使って PrimaryCommands と SecondaryCommands の間でコマンドを移動できます。

- *複数のページで一貫して表示されるコマンドがある場合は、一貫した場所にそのコマンドを配置することをお勧めします。*
- *また、[Accept] (承諾）、[Yes] (はい)、[OK] (OK) コマンドは、[Reject] (拒否)、[No] (いいえ)、[Cancel] (キャンセル) コマンドの左に配置することをお勧めします。一貫性があることで、ユーザーは安心してシステム内を移動でき、アプリのナビゲーションに関する知識をさまざまなアプリで利用することができます。*

### <a name="app-bar-buttons"></a>アプリ バーのボタン

PrimaryCommands と SecondaryCommands には、どちらも [AppBarButton](/uwp/api/Windows.UI.Xaml.Controls.AppBarButton)、[AppBarToggleButton](/uwp/api/Windows.UI.Xaml.Controls.AppBarToggleButton)、[AppBarSeparator](/uwp/api/Windows.UI.Xaml.Controls.AppBarSeparator) の各コマンド要素のみを入力できます。 

アプリ バーのボタン コントロールは、アイコンとテキスト ラベルによって特徴付けられます。 これらのコントロールは、コマンド バーで使うように最適化されており、コマンド バーとオーバーフロー メニューのどちらで使うかに応じて外観が変化します。

オーバーフロー メニューのアイコンのサイズは 16 x 16 ピクセルで、基本コマンド領域のアイコン (20 x 20 ピクセル) よりも小さくなります。 SymbolIcon、FontIcon、または PathIcon を使うと、コマンドがセカンダリ コマンド領域に表示されるときに、忠実さを失うことなく、適切なサイズに自動的に調整されます。 

### <a name="button-labels"></a>ボタンのラベル
AppBarButton [IsCompact](/uwp/api/windows.ui.xaml.controls.appbarbutton.IsCompact) プロパティはラベルが表示されるかどうかを決定します。 CommandBar コントロールで、コマンド バーの開閉に応じてコマンド バーがボタンの IsCompact プロパティを自動的に上書きします。

アプリ バー ボタンのラベルを配置するには、CommandBar の [DefaultLabelPosition](/uwp/api/windows.ui.xaml.controls.commandbar.defaultlabelposition) プロパティを使用します。

```xaml
<CommandBar DefaultLabelPosition="Right">
    <AppBarToggleButton Icon="Shuffle" Label="Shuffle"/>
    <AppBarToggleButton Icon="RepeatAll" Label="Repeat"/>
</CommandBar>
```

![ラベルが右に配置されたコマンド バー](images/app-bar-labels-on-right.png)

比較的大きなウィンドウの場合は、ラベルを見やすくするためにアプリ バーのボタン アイコンの右に移動することを検討します。 ラベルを下に配置すると、コマンド バーを開かないとラベルが見えませんが、ラベルを右に配置すると、コマンド バーが閉じていてもラベルが見えます。

オーバーフロー メニューで、ラベルは既定ではアイコンの右側に配置され、**LabelPosition** は無視されます。 スタイルを調整するには、[CommandBarOverflowPresenterStyle](/uwp/api/Windows.UI.Xaml.Controls.CommandBar.CommandBarOverflowPresenterStyle) プロパティを、[CommandBarOverflowPresenter](/uwp/api/windows.ui.xaml.controls.commandbaroverflowpresenter) をターゲットにする Style に設定します。 

ボタンのラベルは、短く、可能であれば 1 つの単語にすることをお勧めします。 アイコンの下の長いラベルは複数の行に折り返されるため、開いたコマンド バーの全体的な高さが増します。 ラベルのテキストにソフト ハイフン文字 (0x00AD) を含めると、テキストの中で単語を分割する位置を示すことができます。 XAML でこの処理を行うには、次のようなエスケープ シーケンスを使います。

```xaml
<AppBarButton Icon="Back" Label="Areally&#x00AD;longlabel"/>
```

指定した場所でラベルが折り返されると、次のようになります。

![ラベルが折り返されたアプリ バーのボタン](images/app-bar-button-label-wrap.png)

### <a name="command-bar-flyouts"></a>コマンド バーのポップアップ

コマンドは論理的にグループ化することを検討します。たとえば、[返信]、[全員に返信]、[転送] を [応答] メニューに配置します。 通常、アプリ バー ボタンは単一のコマンドをアクティブ化しますが、アプリ バー ボタンを使用して、カスタム コンテンツを持つ [MenuFlyout](/uwp/api/windows.ui.xaml.controls.menuflyout) または [Flyout](/uwp/api/windows.ui.xaml.controls.flyout) を表示できます。

![コマンド バーでのポップアップの例](images/AppbarGuidelines_Flyouts.png)

### <a name="other-content"></a>その他のコンテンツ

XAML 要素をコンテンツ領域に追加するには、**Content** プロパティを設定します。 複数の要素を追加する場合は、それらの要素をパネル コンテナーに配置し、パネルを Content プロパティの唯一の子にする必要があります。

動的オーバーフローが有効になっている場合は、プライマリ コマンドがオーバーフロー メニューに移動するため、コンテンツはクリップされません。 それ以外の場合は、プライマリ コマンドが優先されるため、コンテンツがクリップされる可能性があります。

[ClosedDisplayMode](/uwp/api/windows.ui.xaml.controls.appbar.closeddisplaymode) が **Compact** の場合、コンパクト サイズのコマンド バーよりも大きいコンテンツはクリップされる可能性があります。 UI の一部がクリップされないようにするには、コンテンツ領域で UI の各部分を表示または非表示にするように [Opening](/uwp/api/windows.ui.xaml.controls.appbar.opening) と [Closed](/uwp/api/windows.ui.xaml.controls.appbar.closed) の各イベントを処理する必要があります。 詳しくは、「[開いた状態と閉じた状態](#open-and-closed-states)」をご覧ください。


## <a name="open-and-closed-states"></a>開いた状態と閉じた状態

コマンド バーは、開いたり閉じたりできます。 開いているときは、テキスト ラベル付きのプライマリ コマンド ボタンが表示され、(セカンダリ コマンドが存在する場合は) オーバーフロー メニューが開きます。
コマンドバーはオーバーフロー メニューを上方向 (プライマリ コマンドの上) または下方向 (プライマリ コマンドの下) に開きます。 既定の方向は上方向ですが、オーバーフロー メニューを開くための十分な領域が上方向にない場合、コマンド バーはこれを下方向に開きます。 

ユーザーは、[その他] (\[•••\]) ボタンを押してこれらの状態を切り替えることができます。 プログラムで切り替えるには、[IsOpen](/uwp/api/windows.ui.xaml.controls.appbar.isopen) プロパティを設定します。 

[Opening](/uwp/api/windows.ui.xaml.controls.appbar.opening)、[Opened](/uwp/api/windows.ui.xaml.controls.appbar.opened)、[Closing](/uwp/api/windows.ui.xaml.controls.appbar.closing)、[Closed](/uwp/api/windows.ui.xaml.controls.appbar.closed) の各イベントを使うと、コマンド バーの開閉に対応できます。  
- Opening イベントと Closing イベントが発生するのは、切り替えアニメーションの開始前です。
- Opened イベントと Closed イベントが発生するのは、切り替えの完了後です。

次の例では、Opening イベントと Closing イベントを使ってコマンド バーの不透明度を変更します。 コマンド バーが閉じているときは、アプリの背景が見えるようにコマンド バーが半透明になります。 コマンド バーが開いているときは、ユーザーがコマンドに集中できるようにコマンド バーが不透明になります。

```xaml
<CommandBar Opening="CommandBar_Opening"
            Closing="CommandBar_Closing">
    <AppBarButton Icon="Accept" Label="Accept"/>
    <AppBarButton Icon="Edit" Label="Edit"/>
    <AppBarButton Icon="Save" Label="Save"/>
    <AppBarButton Icon="Cancel" Label="Cancel"/>
</CommandBar>
```

```csharp
private void CommandBar_Opening(object sender, object e)
{
    CommandBar cb = sender as CommandBar;
    if (cb != null) cb.Background.Opacity = 1.0;
}

private void CommandBar_Closing(object sender, object e)
{
    CommandBar cb = sender as CommandBar;
    if (cb != null) cb.Background.Opacity = 0.5;
}

```

### <a name="issticky"></a>IsSticky

コマンド バーが開いているときにユーザーがアプリの他の部分とやり取りすると、コマンド バーは自動的に閉じます。 これは*簡易非表示*と呼ばれます。 簡易非表示動作を制御するには、[IsSticky](/uwp/api/windows.ui.xaml.controls.appbar.issticky) プロパティを設定します。 `IsSticky="true"` の場合、ユーザーが [その他] (\[•••\]) ボタンを押すか、オーバーフロー メニューから項目を選ぶまで、バーは開いたままになります。 

固定のコマンド バーは、[簡易非表示およびキーボード フォーカス](./menus.md#light-dismiss)というユーザーが期待する動作と一致しないため、使わないようにすることをお勧めします。

### <a name="display-mode"></a>表示モード

コマンド バーが閉じた状態でどのように表示されるか制御するには、[ClosedDisplayMode](/uwp/api/windows.ui.xaml.controls.appbar.closeddisplaymode) プロパティを設定します。 3 つのクローズド表示モードから選ぶことができます。
- **コンパクト**:既定のモード。 コンテンツ、プライマリ コマンドのアイコン (ラベルなし)、[その他] (\[•••\]) ボタンが表示されます。
- **最小**:[その他] (\[•••\]) ボタンとして機能する細いバーのみが表示されます。 ユーザーはバーの任意の場所を押してバーを開くことができます。
- **非表示**:コマンド バーを閉じたとき、コマンド バーは表示されません。 このモードは、インライン コマンド バーでコンテキスト依存コマンドを表示するときに便利な場合があります。 この場合は、コマンド バーをプログラムで開く必要があります。この操作を行うには、**IsOpen** プロパティを設定するか、ClosedDisplayMode を **Minimal** または **Compact** に変更します。

以下では、コマンド バーを使って [RichEditBox](/uwp/api/Windows.UI.Xaml.Controls.RichEditBox) に単純な書式設定コマンドを保持しています。 編集ボックスにフォーカスがないときには、書式設定コマンドが煩わしくないように非表示にします。 編集ボックスを使っているときは、コマンド バーの ClosedDisplayMode を Compact に変更して書式設定コマンドを表示します。

```xaml
<StackPanel Width="300"
            GotFocus="EditStackPanel_GotFocus"
            LostFocus="EditStackPanel_LostFocus">
    <CommandBar x:Name="FormattingCommandBar" ClosedDisplayMode="Hidden">
        <AppBarButton Icon="Bold" Label="Bold" ToolTipService.ToolTip="Bold"/>
        <AppBarButton Icon="Italic" Label="Italic" ToolTipService.ToolTip="Italic"/>
        <AppBarButton Icon="Underline" Label="Underline" ToolTipService.ToolTip="Underline"/>
    </CommandBar>
    <RichEditBox Height="200"/>
</StackPanel>
```

```csharp
private void EditStackPanel_GotFocus(object sender, RoutedEventArgs e)
{
    FormattingCommandBar.ClosedDisplayMode = AppBarClosedDisplayMode.Compact;
}

private void EditStackPanel_LostFocus(object sender, RoutedEventArgs e)
{
    FormattingCommandBar.ClosedDisplayMode = AppBarClosedDisplayMode.Hidden;
}
```

>**注**&nbsp;&nbsp;この例では編集コマンドの実装については取り上げません。 詳しくは、「[RichEditBox](rich-edit-box.md)」をご覧ください。

Minimal モードと Hidden モードが役に立つ場合もありますが、すべてのアクションを非表示にするとユーザーが混乱する可能性があることに注意してください。

ClosedDisplayMode を変更してユーザーにヒントを表示すると、周囲にある要素のレイアウトが影響を受けます。 これに対し、CommandBar の開閉を切り替えても他の要素のレイアウトには影響しません。

## <a name="placement"></a>配置
コマンド バーは、```Grid.row``` などのレイアウト コントロールに組み込むことにより、アプリ ウィンドウの上部、アプリ ウィンドウの下部、またはインラインに配置できます。

![アプリ バーの配置の例 1](images/AppbarGuidelines_Placement1.png)

-   小型の携帯デバイスでは、コマンド バーを画面の下部に配置して、操作しやすくことをお勧めします。
-   大きな画面を持つデバイスでは、では、コマンド バーは、ウィンドウの上端近くに配置して、気付きやすくします。

物理的な画面サイズを調べるには、[DiagonalSizeInInches](/uwp/api/windows.graphics.display.displayinformation.diagonalsizeininches) API を使います。

コマンド バーは、単一ビュー画面 (左側の例) と複数ビュー画面 (右側の例) の次の画面領域に配置できます。 インラインのコマンド バーは、アクション領域の任意の場所に配置できます。

![アプリ バーの配置の例 2](images/AppbarGuidelines_Placement2.png)

>**タッチ デバイス**:タッチ キーボード、つまりソフト入力パネル (SIP) が表示されているときに、コマンド バーをユーザーに対して表示したままにする必要がある場合、コマンド バーをページの [BottomAppBar](/uwp/api/windows.ui.xaml.controls.page.bottomappbar) プロパティに割り当てると、SIP の表示中はコマンド バーが移動して表示されたままになります。 それ以外の場合は、コマンド バーをインラインおよびアプリのコンテンツに対して相対的に配置します。

## <a name="get-the-sample-code"></a>サンプル コードの入手

- [XAML コントロール ギャラリー サンプル](https://github.com/Microsoft/Xaml-Controls-Gallery) - インタラクティブな形で XAML コントロールのすべてを参照できます。
- [XAML コマンド実行のサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlCommanding)

## <a name="related-articles"></a>関連記事

* [Windows アプリのコマンド デザインの基本](../basics/commanding-basics.md)
* [CommandBar クラス](/uwp/api/Windows.UI.Xaml.Controls.CommandBar)
