---
Description: NavigationView は、ご利用のアプリの最上位のナビゲーション パターンを実装するアダプティブ コントロールです。
title: ナビゲーション ビュー
template: detail.hbs
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: yulikl
design-contact: kimsea
dev-contact: ''
doc-status: Published
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 1a396377eb332052ae7f238a23865f2b7dc0aa16
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/13/2019
ms.locfileid: "65984181"
---
# <a name="navigation-view"></a>ナビゲーション ビュー

NavigationView コントロールでは、ご利用のアプリの最上位のナビゲーションが提供されます。 これは、さまざまな画面サイズに適応し、_上部_のナビゲーション スタイルと_左側_のナビゲーション スタイルの両方をサポートしています。

![上部のナビゲーション](images/nav-view-header.png)<br/>
_ナビゲーション ビューでは上部と左側の両方のナビゲーションのウィンドウまたはメニューがサポートされます_

> **プラットフォーム API**: [Windows.UI.Xaml.Controls.NavigationView クラス](/uwp/api/windows.ui.xaml.controls.navigationview)
>
> **Windows UI ライブラリ API**: [Microsoft.UI.Xaml.Controls.NavigationView クラス](/uwp/api/microsoft.ui.xaml.controls.navigationview)
>
> _上部_のナビゲーションなどの NavigationView の一部の機能には、Windows 10 バージョン 1809 ([SDK 17763 ](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) 以降、または [Windows UI ライブラリ](https://docs.microsoft.com/uwp/toolkits/winui/)が必要です。

## <a name="is-this-the-right-control"></a>適切なコントロールの選択

NavigationView は次の場合に役に立つアダプティブ ナビゲーション コントロールです。

- ご利用のアプリ全体で一貫性のあるナビゲーション エクスペリエンスを提供する。
- 小さいウィンドウの画面領域を節約する。
- 多くのナビゲーション カテゴリへのアクセスを整理する。

その他のナビゲーション パターンについては、[ナビゲーション デザインの基本](../basics/navigation-basics.md)に関するページを参照してください。

## <a name="examples"></a>例

<table>
<th align="left">XAML コントロール ギャラリー<th>
<tr>
<td><img src="images/XAML-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML コントロール ギャラリー</strong> アプリがインストールされている場合、こちらをクリックして<a href="xamlcontrolsgallery:/item/NavigationView">アプリを開き、NavigationView の動作を確認</a>してください。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML コントロール ギャラリー アプリを入手する (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">ソース コード (GitHub) を入手する</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="display-modes"></a>表示モード

> PaneDisplayMode プロパティには、Windows 10 バージョン 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) 以降、または [Windows UI ライブラリ](https://docs.microsoft.com/uwp/toolkits/winui/)が必要です。

PaneDisplayMode プロパティを使用すれば、NavigationView でさまざまなナビゲーション スタイルまたは表示モードを構成することができます。

:::row:::
    :::column:::
    ### <a name="top"></a>Top
    ウィンドウはコンテンツの上に配置されます。</br>
    `PaneDisplayMode="Top"`
    :::column-end:::
    :::column span="2":::
    ![上部のナビゲーションの例](images/displaymode-top.png)
    :::column-end:::
:::row-end:::

次の場合に_上部_ナビゲーションをお勧めします。

- 同じ重要度の最上位ナビゲーション カテゴリが 5 個以下であり、ドロップダウン オーバーフロー メニューとなる追加の最上位ナビゲーション カテゴリはいずれも、それほど重要ではないと考えられる。
- 画面上にナビゲーション オプションをすべて表示する必要がある。
- ご利用のアプリのコンテンツ用にスペースを追加したい。
- アイコンでは、ご利用のアプリのナビゲーション カテゴリを明確に説明できない。

:::row:::
    :::column:::
    ### <a name="left"></a>Left
    ウィンドウはコンテンツの左側に展開および配置されます。</br>
    `PaneDisplayMode="Left"`
    :::column-end:::
    :::column span="2":::
    ![展開された左側のナビゲーションのウィンドウの例](images/displaymode-left.png)
    :::column-end:::
:::row-end:::

次の場合に_左側_のナビゲーションをお勧めします。

- 同じ重要度の最上位ナビゲーション カテゴリが 5 から 10 個ある。
- ナビゲーション カテゴリ以外のアプリ コンテンツ用のスペースを少なくして、ナビゲーション カテゴリを非常に目立つようにする。

:::row:::
    :::column:::
    ### <a name="leftcompact"></a>LeftCompact
    ウィンドウは開かれるまでアイコンのみを表示し、ウィンドウはコンテンツの左側に配置されます。</br>
    `PaneDisplayMode="LeftCompact"`
    :::column-end:::
    :::column span="2":::
    ![コンパクトな左側のナビゲーションのウィンドウの例](images/displaymode-leftcompact.png)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
    ### <a name="leftminimal"></a>LeftMinimal
    ウィンドウが開かれるまで、メニュー ボタンのみが表示されます。 開かれると、コンテンツの左側に配置されます。</br>
    `PaneDisplayMode="LeftMinimal"`
    :::column-end:::
    :::column span="2":::
    ![最小化されている左側のナビゲーションのウィンドウの例](images/displaymode-leftminimal.png)
    :::column-end:::
:::row-end:::

### <a name="auto"></a>Auto

既定では、PaneDisplayMode は Auto に設定されます。自動モードでは、ナビゲーション ビューは、ウィンドウが狭いときは LeftMinimal または LeftCompact となり、ウィンドウが広くなると Left に切り替わります。 詳細については、「[アダプティブ動作](#adaptive-behavior)」セクションを参照してください。

![左側のナビゲーションの既定のアダプティブ動作](images/displaymode-auto.png)<br/>
_ナビゲーション ビューの既定のアダプティブ動作_

## <a name="anatomy"></a>構造

これらのイメージは、_上部_または_左側_のナビゲーションが構成されている場合の、ウィンドウ、ヘッダー、およびコントロールのコンテンツ領域のレイアウトを示します。

![上部のナビゲーション ビューのレイアウト](images/topnav-anatomy.png)<br/>
_上部のナビゲーションのレイアウト_

![左側のナビゲーション ビューのレイアウト](images/leftnav-anatomy.png)<br/>
_左側のナビゲーションのレイアウト_

### <a name="pane"></a>ウィンドウ

PaneDisplayMode プロパティを使用すると、コンテンツの上またはコンテンツの左側にウィンドウを配置できます。

NavigationView ウィンドウには、次のものを含めることができます。

- [NavigationViewItem](/uwp/api/windows.ui.xaml.controls.navigationviewitem) オブジェクト。 特定のページに移動するためのナビゲーション項目。
- [NavigationViewItemSeparator](/uwp/api/windows.ui.xaml.controls.navigationviewitemseparator) オブジェクト。 ナビゲーション項目をグループ化するための区切り記号。 [Opacity](/uwp/api/windows.ui.xaml.uielement.opacity) プロパティを 0 に設定して区切り記号を空白としてレンダリングします。
- [NavigationViewItemHeader](/uwp/api/windows.ui.xaml.controls.navigationviewitemheader) オブジェクト。 項目グループへのラベル付けのためのヘッダー。
- オプションの [AutoSuggestBox](auto-suggest-box.md) を含めると、アプリ レベルの検索が可能になります。 コントロールを [NavigationView.AutoSuggestBox](/uwp/api/windows.ui.xaml.controls.navigationview.autosuggestbox) プロパティに割り当てます。
- [アプリ設定](../app-settings/app-settings-and-data.md)のエントリ ポイント (オプション)。 設定項目を非表示にするには、[IsSettingsVisible](/uwp/api/windows.ui.xaml.controls.navigationview.IsSettingsVisible) プロパティを **false** に設定します。

また、左側のウィンドウには次のものが含まれています。

- ウィンドウの開閉を切り替えるメニュー ボタン。 [IsPaneToggleButtonVisible](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.IsPaneToggleButtonVisible) プロパティを使うと、ウィンドウが開いたとき、大きなアプリ ウィンドウで、このボタンを非表示にすることを選択できます。

ナビゲーション ビューには、ウィンドウの左上隅に配置された [戻る] ボタンがあります。 ただし、これを使用しても、後方ナビゲーションの処理と、バック スタックへのコンテンツの追加は自動的には行われません。 前に戻る処理を有効にするには、「[後方ナビゲーション](#backwards-navigation)」セクションを参照してください。

ウィンドウが上部または左側に配置された場合の詳細なウィンドウ構造を以下に示します。

#### <a name="top-navigation-pane"></a>上部のナビゲーションのウィンドウ

![ナビゲーション ビューの上部のウィンドウの構造](images/navview-pane-anatomy-horizontal.png)

1. ヘッダー
1. ナビゲーション項目
1. 区切り記号
1. AutoSuggestBox (オプション)
1. [設定] ボタン (オプション)

#### <a name="left-navigation-pane"></a>左側のナビゲーションのウィンドウ

![ナビゲーション ビューの左側のウィンドウの構造](images/navview-pane-anatomy-vertical.png)

1. メニュー ボタン
1. ナビゲーション項目
1. 区切り記号
1. ヘッダー
1. AutoSuggestBox (オプション)
1. [設定] ボタン (オプション)

#### <a name="pane-footer"></a>ウィンドウのフッター

[PaneFooter](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneFooter) プロパティに自由形式のコンテンツを追加すると、それをウィンドウのフッターに配置することができます。

:::row:::
    :::column:::
    ![ウィンドウのフッターの上部のナビゲーション](images/navview-freeform-footer-top.png)<br>
     _上部のウィンドウのフッター_<br>
    :::column-end:::
    :::column:::
    ![ウィンドウのフッターの左側のナビゲーション](images/navview-freeform-footer-left.png)<br>
    _左側のウィンドウのフッター_<br>
    :::column-end:::
:::row-end:::

#### <a name="pane-title-and-header"></a>ウィンドウのタイトルとヘッダー

[PaneTitle](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneTitle) プロパティを設定することで、ウィンドウのヘッダー領域にテキスト コンテンツを配置できます。 このプロパティは文字列を取り、メニュー ボタンの横にテキストを表示します。

画像やロゴなどのテキスト以外のコンテンツを追加するには、[PaneHeader](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneHeader) プロパティに追加することで、ウィンドウのヘッダーに任意の要素を配置できます。

PaneTitle と PaneHeader の両方を設定した場合、コンテンツはメニュー ボタンの横に水平に積み上げられ、PaneTitle はメニュー ボタンに最も近くなります。

:::row:::
    :::column:::
    ![ウィンドウのヘッダーの上部のナビゲーション](images/navview-freeform-header-top.png)<br>
     _上部のウィンドウのヘッダー_<br>
    :::column-end:::
    :::column:::
    ![ウィンドウのヘッダーの左側のナビゲーション](images/navview-freeform-header-left.png)<br>
    _左側のウィンドウのヘッダー_<br>
    :::column-end:::
:::row-end:::

#### <a name="pane-content"></a>ウィンドウのコンテンツ

自由形式のコンテンツは、[PaneCustomContent](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneCustomContent) プロパティに追加することでウィンドウに配置できます。

:::row:::
    :::column:::
    ![ウィンドウのカスタム コンテンツの上部のナビゲーション](images/navview-freeform-pane-top.png)<br>
     _上部のウィンドウのカスタム コンテンツ_<br>
    :::column-end:::
    :::column:::
    ![ウィンドウのカスタム コンテンツの左側のナビゲーション](images/navview-freeform-pane-left.png)<br>
    _左側のウィンドウのカスタム コンテンツ_<br>
    :::column-end:::
:::row-end:::

### <a name="header"></a>Header

[Header](/uwp/api/windows.ui.xaml.controls.navigationview.header) プロパティを設定することで、ページのタイトルを追加できます。

![ナビゲーション ビューのヘッダー領域の例](images/nav-header.png)<br/>
_ナビゲーション ビューのヘッダー_

ヘッダー領域は、左側のウィンドウ位置では移動ボタンともに垂直方向に揃えられ、上部のウィンドウ位置ではウィンドウの下に置かれます。 その高さは、52 ピクセルで固定です。 これは、選択されたナビゲーション カテゴリのページ タイトルを保持するためです。 ヘッダーはページ上部に固定され、コンテンツ領域のスクロール クリッピング ポイントとして機能します。

ヘッダーは、NavigationView が Minimal モードのときはいつも表示されます。 ウィンドウの幅をもっと広げて使用される他のモードでは、ヘッダーを非表示にすることもできます。 ヘッダーを非表示にするには、[AlwaysShowHeader](/uwp/api/windows.ui.xaml.controls.navigationview.AlwaysShowHeader) プロパティを **false** に設定します。

### <a name="content"></a>コンテンツ

![ナビゲーション ビューのコンテンツ領域の例](images/nav-content.png)<br/>
_ナビゲーション ビューのコンテンツ_

コンテンツ領域には、選んだナビゲーション カテゴリのほとんどの情報が表示されます。

NavigationView が**Minimal** モードの場合はコンテンツ領域に 12 ピクセルの余白を設定し、それ以外の場合は 24 ピクセルの余白を設定することをお勧めします。

## <a name="adaptive-behavior"></a>アダプティブ動作

既定では、ナビゲーション ビューは、利用可能な画面領域の大きさに基づいて自動的に表示モードが変わります。 [CompactModeThresholdWidth](/uwp/api/windows.ui.xaml.controls.navigationview.compactmodethresholdwidth) プロパティおよび [ExpandedModeThresholdWidth](/uwp/api/windows.ui.xaml.controls.navigationview.expandedmodethresholdwidth) プロパティでは、表示モードが変更されるブレークポイントが指定されます。 これらの値を変更することで、アダプティブ表示モードの動作をカスタマイズできます。

### <a name="default"></a>Default

PaneDisplayMode を既定値である **Auto** に設定すると、アダプティブ動作は次のようになります。

- ウィンドウ幅が広い場合 (1008 ピクセル以上)、展開された左側のウィンドウが表示されます。
- ウィンドウ幅が中くらいの場合 (641 ピクセルから 1007 ピクセル)、左側 (アイコンのみ) のナビゲーションのウィンドウ (LeftCompact) が表示されます。
- ウィンドウ幅が狭い場合 (640 ピクセル以下)、メニュー ボタン (LeftMinimal) のみが表示されます。

アダプティブ動作に対応するウィンドウ サイズの詳細については、「[画面のサイズとブレークポイント](../layout/screen-sizes-and-breakpoints-for-responsive-design.md)」を参照してください。

![左側のナビゲーションの既定のアダプティブ動作](images/displaymode-auto.png)<br/>
_ナビゲーション ビューの既定のアダプティブ動作_

### <a name="minimal"></a>最小

2 つ目の一般的なアダプティブ パターンは、ウィンドウ幅が広い場合は展開された左側のウィンドウを使用し、ウィンドウ幅が中くらいまたは狭い場合はメニュー ボタンのみを使用するというものです。

これは次の場合にお勧めします。

- ウィンドウ幅が小さい場合にアプリ コンテンツ用のスペースを広く確保したい。
- アイコンでは、ナビゲーション カテゴリを明確に表すことができない。

![左側のナビゲーションの最小アダプティブ動作](images/adaptive-behavior-minimal.png)<br/>
_ナビゲーション ビューの "最小" アダプティブ動作_

この動作を設定するには、ウィンドウを折りたたむときの幅を CompactModeThresholdWidth に設定します。 ここでは、既定の 640 から 1007 に変更されています。 また、値が競合しないように ExpandedModeThresholdWidth を設定する必要があります。

```xaml
<NavigationView CompactModeThresholdWidth="1007" ExpandedModeThresholdWidth="1007"/>
```

### <a name="compact"></a>コンパクト

3 つ目の一般的なアダプティブ パターンは、ウィンドウ幅が広い場合は展開された左側のウィンドウを使用し、ウィンドウ幅が中くらいの場合と狭い場合では LeftCompact (アイコンのみ) ナビゲーション ウィンドウを使用するというものです。

これは次の場合にお勧めします。

- 常にすべてのナビゲーション オプションを画面に表示することが重要。
- アイコンで、ナビゲーション カテゴリを明確に表すことができる。

![左側のナビゲーションのコンパクト アダプティブ動作](images/adaptive-behavior-compact.png)<br/>
_ナビゲーション ビューの "コンパクト" アダプティブ動作_

この動作を構成するには、CompactModeThresholdWidth を 0 に設定します。

```xaml
<NavigationView CompactModeThresholdWidth="0"/>
```

### <a name="no-adaptive-behavior"></a>アダプティブ動作なし

自動アダプティブ動作を無効にするには、PaneDisplayMode を Auto 以外の値に設定します。ここでは、LeftMinimal に設定されているので、ウィンドウの幅に関係なくメニュー ボタンのみが表示されます。

![左側のナビゲーションのアダプティブ動作なし](images/adaptive-behavior-none.png)<br/>
_PaneDisplayMode が LeftMinimal に設定されたナビゲーション ビュー_

```xaml
<NavigationView PaneDisplayMode="LeftMinimal" />
```

「_表示モード_」セクションで既に説明したように、ウィンドウが常に上部にある状態、常に展開された状態、常にコンパクトの状態、または常に最小の状態になるように設定することができます。 ご自分のアプリ コード内で表示モードを自身で管理することもできます。 この例については、次のセクションで示します。

### <a name="top-to-left-navigation"></a>上部のナビゲーションから左側のナビゲーションへ

ご利用のアプリで上部のナビゲーションを使用すると、ウィンドウ幅が狭くなるにつれてナビゲーション項目がオーバーフロー メニューに折りたたまれます。 ご利用のアプリのウィンドウ幅が狭い場合は、すべての項目をオーバーフロー メニューに折りたたむのではなく、PaneDisplayMode を Top ナビゲーションから LeftMinimal ナビゲーションに切り替える方がユーザー エクスペリエンスが向上します。

次の場合、ウィンドウ サイズが大きいときは上部のナビゲーションを使用し、ウィンドウ サイズが小さいときは左側のナビゲーションを使用することをお勧めします。

- 最上位ナビゲーション カテゴリのセット内の 1 つが画面に収まらない場合に、左側のナビゲーションに折りたたんで重要度を同じにして一緒に表示される必要のある同じ重要度のセットがある。
- 小さいウィンドウ サイズでできるだけ多くのコンテンツスペースを確保したい。

この例では、[VisualStateManager](/uwp/api/Windows.UI.Xaml.VisualStateManager) プロパティと [AdaptiveTrigger.MinWindowWidth](/uwp/api/windows.ui.xaml.adaptivetrigger.minwindowwidth) プロパティを使用して、Top ナビゲーションと LeftMinimal ナビゲーションを切り替える方法を示します。

![上部または左側のアダプティブ動作 1 の例](images/navigation-top-to-left.png)

```xaml
<Grid >
    <NavigationView x:Name="NavigationViewControl" >
        <NavigationView.MenuItems>
            <NavigationViewItem Content="A" x:Name="A" />
            <NavigationViewItem Content="B" x:Name="B" />
            <NavigationViewItem Content="C" x:Name="C" />
        </NavigationView.MenuItems>
    </NavigationView>

    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup>
            <VisualState>
                <VisualState.StateTriggers>
                    <AdaptiveTrigger
                        MinWindowWidth="{x:Bind NavigationViewControl.CompactModeThresholdWidth}" />
                </VisualState.StateTriggers>

                <VisualState.Setters>
                    <Setter Target="NavigationViewControl.PaneDisplayMode" Value="Top"/>
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Grid>

```

> [!TIP]
> AdaptiveTrigger.MinWindowWidth を使用すると、ウィンドウの幅が指定された最小幅よりも広くなったときに表示状態がトリガーされます。 つまり、既定の XAML によって狭いウィンドウが定義され、VisualState によってウィンドウが広くなったときに適用する変更が定義されます。 ナビゲーション ビューの既定の PaneDisplayMode は Auto です。したがって、ウィンドウの幅が CompactModeThresholdWidth 以下である場合は、LeftMinimal ナビゲーションが使用されます。 ウィンドウが広くなると、VisualState によって既定値がオーバーライドされ、Top ナビゲーションが使用されます。

## <a name="navigation"></a>ナビゲーション

ナビゲーション ビューではナビゲーション タスクは自動的に実行されません。 ユーザーがナビゲーション項目をタップすると、ナビゲーション ビューではその項目が選択済みとして表示され、[ItemInvoked](/uwp/api/windows.ui.xaml.controls.navigationview.ItemInvoked) イベントが発生します。 タップによって新しい項目が選択されると、[SelectionChanged](/uwp/api/windows.ui.xaml.controls.navigationview.SelectionChanged) イベントも発生します。

どちらのイベントを処理しても、要求されたナビゲーションに関連するタスクを実行できます。 どちらを処理する必要があるかは、ご利用のアプリに求める動作によって異なります。 通常は、要求されたページに移動し、これらのイベントに応じてナビゲーション ビューのヘッダーを更新します。

**ItemInvoked** は、ユーザーがナビゲーション項目をタップするたびに、それが既に選択されている場合でも発生します。 (項目は、マウス、キーボード、またはその他の入力を使用して同等の操作で呼び出すこともできます。 詳細については、「[入力と操作](../input/index.md)」を参照してください)。ItemInvoked ハンドラー内を移動すると、既定ではページが再読み込みされ、重複エントリがナビゲーション スタックに追加されます。 項目が呼び出されたときに移動する場合は、ページの再読み込みを禁止するか、またはページが再読み込みされるときにナビゲーション バックスタック内に重複エントリが決して作成されないようにする必要があります。 (コード例を参照してください)。

**SelectionChanged** は、現在選択されていない項目をユーザーが呼び出すことで発生させることも、選択された項目をプログラムで変更することによって発生させることもできます。 ユーザーが項目を呼び出したために選択の変更が発生した場合は、最初に ItemInvoked イベントが発生します。 選択の変更がプログラムによるものである場合、ItemInvoked は発生しません。

### <a name="backwards-navigation"></a>逆方向のナビゲーション

NavigationView には組み込みの [戻る] ボタンがありますが、前方ナビゲーションと同様に、後方ナビゲーションは自動的には実行されません。 ユーザーが [戻る] ボタンをタップすると、[BackRequested](/uwp/api/windows.ui.xaml.controls.navigationview.BackRequested) イベントが発生します。 このイベントを処理して、後方ナビゲーションを実行します。 詳細については、[ナビゲーション履歴と後方ナビゲーション](../basics/navigation-history-and-backwards-navigation.md)に関するページを参照してください。

Minimal モードまたは Compact モードでは、ナビゲーション ビュー ウィンドウはポップアップとして開きます。 この場合、[戻る] ボタンをクリックすると、ウィンドウが閉じられ、代わりに **PaneClosing** イベントが発生します。

以下のプロパティを設定することで、[戻る] ボタンを非表示または無効にすることができます。

- [IsBackButtonVisible](/uwp/api/windows.ui.xaml.controls.navigationview.IsBackButtonVisible): [戻る] ボタンを表示および非表示にするために使用します。 このプロパティは [NavigationViewBackButtonVisible](/uwp/api/windows.ui.xaml.controls.navigationviewbackbuttonvisible) 列挙体の値を取り、既定では **Auto** に設定されています。 ボタンが折りたたまれると、このボタン用のスペースはレイアウト内に確保されません。
- [IsBackEnabled](/uwp/api/windows.ui.xaml.controls.navigationview.IsBackEnabled): [戻る] ボタンを有効または無効にするために使用します。 このプロパティは、ご利用のナビゲーション フレームの [CanGoBack](/uwp/api/windows.ui.xaml.controls.frame.cangoback) プロパティにデータ バインドすることができます。 **IsBackEnabled** が **false** の場合、**BackRequested** は発生しません。

:::row:::
    :::column:::
        ![Navigation view back button in the left navigation pane](images/leftnav-back.png)<br/>
        _The back button in the left navigation pane_
    :::column-end:::
    :::column:::
        ![Navigation view back button in the top navigation pane](images/topnav-back.png)<br/>
        _The back button in the top navigation pane_
    :::column-end:::
:::row-end:::

## <a name="code-example"></a>コードの例

この例では、ウィンドウ サイズが大きい場合の上部のナビゲーション ウィンドウとウィンドウ サイズが小さい場合の左側のナビゲーション ウィンドウの両方で NavigationView を使用する方法を示します。 これは、VisualStateManager で_上部_のナビゲーション設定を削除することにより、左側のみのナビゲーションに適応させることができます。

この例は、一般的なシナリオの多くで機能するナビゲーション データを設定するための推奨方法を示しています。 また、NavigationView の [戻る] ボタンとキーボード ナビゲーションを使用して後方ナビゲーションを実装する方法についても示します。

このコードでは、移動先である次の名前を含むページが、ご利用のアプリに含まれていることを前提としています。_HomePage_、_AppsPage_、_GamesPage_、_MusicPage_、_MyContentPage_、および _SettingsPage_。 これらのページのコードは示されていません。

> [!IMPORTANT]
> アプリのページに関する情報は [ValueTuple](https://docs.microsoft.com/dotnet/api/system.valuetuple) に格納されています。 この構造体を使用するには、ご利用のアプリ プロジェクトの最小バージョンが SDK 17763 以上である必要があります。 前のバージョンの Windows 10 をターゲットとする NavigationView の WinUI バージョンを使用する場合は、代わりに [System.ValueTuple NuGet パッケージ](https://www.nuget.org/packages/System.ValueTuple/)を使用することができます。

> [!IMPORTANT]
> このコードでは、NavigationView の [Windows UI ライブラリ](https://docs.microsoft.com/uwp/toolkits/winui/) バージョンの使い方を示します。 プラットフォーム バージョンの NavigationView を代わりに使用する場合、アプリ プロジェクトの最小バージョンは SDK 17763 以上である必要があります。 プラットフォーム バージョンを使用するには、`muxc:` へのすべての参照を削除します。

```xaml
<!-- xmlns:muxc="using:Microsoft.UI.Xaml.Controls" -->
<Grid>
    <muxc:NavigationView x:Name="NavView"
                         Loaded="NavView_Loaded"
                         ItemInvoked="NavView_ItemInvoked"
                         BackRequested="NavView_BackRequested">
        <muxc:NavigationView.MenuItems>
            <muxc:NavigationViewItem Tag="home" Icon="Home" Content="Home"/>
            <muxc:NavigationViewItemSeparator/>
            <muxc:NavigationViewItemHeader x:Name="MainPagesHeader"
                                           Content="Main pages"/>
            <muxc:NavigationViewItem Tag="apps" Content="Apps">
                <muxc:NavigationViewItem.Icon>
                    <FontIcon FontFamily="Segoe MDL2 Assets" Glyph="&#xEB3C;"/>
                </muxc:NavigationViewItem.Icon>
            </muxc:NavigationViewItem>
            <muxc:NavigationViewItem Tag="games" Content="Games">
                <muxc:NavigationViewItem.Icon>
                    <FontIcon FontFamily="Segoe MDL2 Assets" Glyph="&#xE7FC;"/>
                </muxc:NavigationViewItem.Icon>
            </muxc:NavigationViewItem>
            <muxc:NavigationViewItem Tag="music" Icon="Audio" Content="Music"/>
        </muxc:NavigationView.MenuItems>

        <muxc:NavigationView.AutoSuggestBox>
            <!-- See AutoSuggestBox documentation for
                 more info about how to implement search. -->
            <AutoSuggestBox x:Name="NavViewSearchBox" QueryIcon="Find"/>
        </muxc:NavigationView.AutoSuggestBox>

        <ScrollViewer>
            <Frame x:Name="ContentFrame" Padding="12,0,12,24" IsTabStop="True"
                   NavigationFailed="ContentFrame_NavigationFailed"/>
        </ScrollViewer>
    </muxc:NavigationView>

    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup>
            <VisualState>
                <VisualState.StateTriggers>
                    <AdaptiveTrigger
                        MinWindowWidth="{x:Bind NavView.CompactModeThresholdWidth}"/>
                </VisualState.StateTriggers>
                <VisualState.Setters>
                    <!-- Remove the next 3 lines for left-only navigation. -->
                    <Setter Target="NavView.PaneDisplayMode" Value="Top"/>
                    <Setter Target="NavViewSearchBox.Width" Value="200"/>
                    <Setter Target="MainPagesHeader.Visibility" Value="Collapsed"/>
                    <!-- Leave the next line for left-only navigation. -->
                    <Setter Target="ContentFrame.Padding" Value="24,0,24,24"/>
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Grid>
```

> [!IMPORTANT]
> このコードでは、NavigationView の [Windows UI ライブラリ](https://docs.microsoft.com/uwp/toolkits/winui/) バージョンの使い方を示します。 プラットフォーム バージョンの NavigationView を代わりに使用する場合、アプリ プロジェクトの最小バージョンは SDK 17763 以上である必要があります。 プラットフォーム バージョンを使用するには、`muxc` へのすべての参照を削除します。

```csharp
// Add "using" for WinUI controls.
// using muxc = Microsoft.UI.Xaml.Controls;

private void ContentFrame_NavigationFailed(object sender, NavigationFailedEventArgs e)
{
    throw new Exception("Failed to load Page " + e.SourcePageType.FullName);
}

// List of ValueTuple holding the Navigation Tag and the relative Navigation Page
private readonly List<(string Tag, Type Page)> _pages = new List<(string Tag, Type Page)>
{
    ("home", typeof(HomePage)),
    ("apps", typeof(AppsPage)),
    ("games", typeof(GamesPage)),
    ("music", typeof(MusicPage)),
};

private void NavView_Loaded(object sender, RoutedEventArgs e)
{
    // You can also add items in code.
    NavView.MenuItems.Add(new muxc.NavigationViewItemSeparator());
    NavView.MenuItems.Add(new muxc.NavigationViewItem
    {
        Content = "My content",
        Icon = new SymbolIcon((Symbol)0xF1AD),
        Tag = "content"
    });
    _pages.Add(("content", typeof(MyContentPage)));

    // Add handler for ContentFrame navigation.
    ContentFrame.Navigated += On_Navigated;

    // NavView doesn't load any page by default, so load home page.
    NavView.SelectedItem = NavView.MenuItems[0];
    // If navigation occurs on SelectionChanged, this isn't needed.
    // Because we use ItemInvoked to navigate, we need to call Navigate
    // here to load the home page.
    NavView_Navigate("home", new EntranceNavigationTransitionInfo());

    // Add keyboard accelerators for backwards navigation.
    var goBack = new KeyboardAccelerator { Key = VirtualKey.GoBack };
    goBack.Invoked += BackInvoked;
    this.KeyboardAccelerators.Add(goBack);

    // ALT routes here
    var altLeft = new KeyboardAccelerator
    {
        Key = VirtualKey.Left,
        Modifiers = VirtualKeyModifiers.Menu
    };
    altLeft.Invoked += BackInvoked;
    this.KeyboardAccelerators.Add(altLeft);
}

private void NavView_ItemInvoked(muxc.NavigationView sender,
                                 muxc.NavigationViewItemInvokedEventArgs args)
{
    if (args.IsSettingsInvoked == true)
    {
        NavView_Navigate("settings", args.RecommendedNavigationTransitionInfo);
    }
    else if (args.InvokedItemContainer != null)
    {
        var navItemTag = args.InvokedItemContainer.Tag.ToString();
        NavView_Navigate(navItemTag, args.RecommendedNavigationTransitionInfo);
    }
}

<!-- NavView_SelectionChanged is not used in this example, but is shown for completeness.
     You will typically handle either ItemInvoked or SelectionChanged to perform navigation,
     but not both. -->
private void NavView_SelectionChanged(muxc.NavigationView sender,
                                      muxc.NavigationViewSelectionChangedEventArgs args)
{
    if (args.IsSettingsSelected == true)
    {
        NavView_Navigate("settings", args.RecommendedNavigationTransitionInfo);
    }
    else if (args.SelectedItemContainer != null)
    {
        var navItemTag = args.SelectedItemContainer.Tag.ToString();
        NavView_Navigate(navItemTag, args.RecommendedNavigationTransitionInfo);
    }
}

private void NavView_Navigate(string navItemTag, NavigationTransitionInfo transitionInfo)
{
    Type _page = null;
    if (navItemTag == "settings")
    {
        _page = typeof(SettingsPage);
    }
    else
    {
        var item = _pages.FirstOrDefault(p => p.Tag.Equals(navItemTag));
        _page = item.Page;
    }
    // Get the page type before navigation so you can prevent duplicate
    // entries in the backstack.
    var preNavPageType = ContentFrame.CurrentSourcePageType;

    // Only navigate if the selected page isn't currently loaded.
    if (!(_page is null) && !Type.Equals(preNavPageType, _page))
    {
        ContentFrame.Navigate(_page, null, transitionInfo);
    }
}

private void NavView_BackRequested(muxc.NavigationView sender,
                                   muxc.NavigationViewBackRequestedEventArgs args)
{
    On_BackRequested();
}

private void BackInvoked(KeyboardAccelerator sender,
                         KeyboardAcceleratorInvokedEventArgs args)
{
    On_BackRequested();
    args.Handled = true;
}

private bool On_BackRequested()
{
    if (!ContentFrame.CanGoBack)
        return false;

    // Don't go back if the nav pane is overlayed.
    if (NavView.IsPaneOpen &&
        (NavView.DisplayMode == muxc.NavigationViewDisplayMode.Compact ||
         NavView.DisplayMode == muxc.NavigationViewDisplayMode.Minimal))
        return false;

    ContentFrame.GoBack();
    return true;
}

private void On_Navigated(object sender, NavigationEventArgs e)
{
    NavView.IsBackEnabled = ContentFrame.CanGoBack;

    if (ContentFrame.SourcePageType == typeof(SettingsPage))
    {
        // SettingsItem is not part of NavView.MenuItems, and doesn't have a Tag.
        NavView.SelectedItem = (muxc.NavigationViewItem)NavView.SettingsItem;
        NavView.Header = "Settings";
    }
    else if (ContentFrame.SourcePageType != null)
    {
        var item = _pages.FirstOrDefault(p => p.Page == e.SourcePageType);

        NavView.SelectedItem = NavView.MenuItems
            .OfType<muxc.NavigationViewItem>()
            .First(n => n.Tag.Equals(item.Tag));

        NavView.Header =
            ((muxc.NavigationViewItem)NavView.SelectedItem)?.Content?.ToString();
    }
}
```

以下は、上記の C# コード例の **NavView_ItemInvoked** ハンドラーの [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/index) バージョンです。 C++/WinRT ハンドラーの手法では、移動先のページの完全な型名を ([**NavigationViewItem**](/uwp/api/windows.ui.xaml.controls.navigationviewitem) のタグに) 最初に格納する必要があります。 ハンドラーでは、その値のボックス化を解除し、それを [**Windows::UI::Xaml::Interop::TypeName**](/uwp/api/windows.ui.xaml.interop.typename) オブジェクトに変換し、それを使用して目的のページに移動します。 C# の例に示した `_pages` という名前のマッピング変数は必要ありません。また、ご利用のタグ内の値が有効な型であることを確認するための単体テストを作成することができます。 「[C++/WinRT を使用した IInspectable へのスカラー値のボックス化とボックス化解除](/windows/uwp/cpp-and-winrt-apis/boxing)」も参照してください。

```cppwinrt
void MainPage::NavView_ItemInvoked(Windows::Foundation::IInspectable const & /* sender */, Windows::UI::Xaml::Controls::NavigationViewItemInvokedEventArgs const & args)
{
    if (args.IsSettingsInvoked())
    {
        // Navigate to Settings.
    }
    else if (args.InvokedItemContainer())
    {
        Windows::UI::Xaml::Interop::TypeName pageTypeName;
        pageTypeName.Name = unbox_value<hstring>(args.InvokedItemContainer().Tag());
        pageTypeName.Kind = Windows::UI::Xaml::Interop::TypeKind::Primitive;
        ContentFrame().Navigate(pageTypeName, nullptr);
    }
}
```

## <a name="navigation-view-customization"></a>ナビゲーション ビューのカスタマイズ

### <a name="pane-backgrounds"></a>ウィンドウの背景

既定では、NavigationView ウィンドウでは表示モードに応じて次のようにさまざまな背景が使用されます。

- ウィンドウは、コンテンツと横並びに左側に展開されると灰色の単色になります (Left モード)。
- コンテンツの上部にオーバーレイとして開かれたウィンドウでは、アプリ内アクリルが使用されます (Top モード、Minimal モード、または Compact モード)。

ウィンドウの背景を変更するには、各モードで背景をレンダリングするのに使用される XAML テーマ リソースをオーバーライドします (各種表示モードに対してさまざまな背景をサポートするには、単一の PaneBackground プロパティではなくこの手法が使用されます)。

次の表に、各表示モードで使用されるテーマ リソースを示します。

| 表示モード | テーマ リソース |
| ------------ | -------------- |
| Left | NavigationViewExpandedPaneBackground |
| LeftCompact<br/>LeftMinimal | NavigationViewDefaultPaneBackground |
| Top | NavigationViewTopPaneBackground |

この例では、App.xaml 内でテーマ リソースをオーバーライドする方法を示します。 テーマ リソースをオーバーライドする場合、最低でも "Default" および "HighContrast" のリソース辞書は常に指定し、"Light" または "Dark" リソース用の辞書は必要に応じて指定します。 詳細については、[ResourceDictionary.ThemeDictionaries](/uwp/api/windows.ui.xaml.resourcedictionary.themedictionaries) に関するページを参照してください。

> [!IMPORTANT]
> このコードでは、AcrylicBrush の [Windows UI ライブラリ](https://docs.microsoft.com/uwp/toolkits/winui/) バージョンの使い方を示します。 プラットフォーム バージョンの AcrylicBrush を代わりに使用する場合、アプリ プロジェクトの最小バージョンは SDK 16299 以上である必要があります。 プラットフォーム バージョンを使用するには、`muxm:` へのすべての参照を削除します。

```xaml
<Application
    <!-- ... -->
    xmlns:muxm="using:Microsoft.UI.Xaml.Media">
    <Application.Resources>
        <ResourceDictionary>
            <ResourceDictionary.MergedDictionaries>
                <XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls"/>
                <ResourceDictionary>
                    <ResourceDictionary.ThemeDictionaries>
                        <ResourceDictionary x:Key="Default">
                            <!-- The "Default" theme dictionary is used unless a specific
                                 light, dark, or high contrast dictionary is provided. These
                                 resources should be tested with both the light and dark themes,
                                 and specific light or dark resources provided as needed. -->
                            <muxm:AcrylicBrush x:Key="NavigationViewDefaultPaneBackground"
                                   BackgroundSource="Backdrop"
                                   TintColor="LightSlateGray"
                                   TintOpacity=".6"/>
                            <muxm:AcrylicBrush x:Key="NavigationViewTopPaneBackground"
                                   BackgroundSource="Backdrop"
                                   TintColor="{ThemeResource SystemAccentColor}"
                                   TintOpacity=".6"/>
                            <LinearGradientBrush x:Key="NavigationViewExpandedPaneBackground"
                                     StartPoint="0.5,0" EndPoint="0.5,1">
                                <GradientStop Color="LightSlateGray" Offset="0.0" />
                                <GradientStop Color="White" Offset="1.0" />
                            </LinearGradientBrush>
                        </ResourceDictionary>
                        <ResourceDictionary x:Key="HighContrast">
                            <!-- Always include a "HighContrast" dictionary when you override
                                 theme resources. This empty dictionary ensures that the 
                                 default high contrast resources are used when the user
                                 turns on high contrast mode. -->
                        </ResourceDictionary>
                    </ResourceDictionary.ThemeDictionaries>
                </ResourceDictionary>
            </ResourceDictionary.MergedDictionaries>
        </ResourceDictionary>
    </Application.Resources>
</Application>
```

## <a name="related-topics"></a>関連トピック

- [NavigationView クラス](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview)
- [マスター/詳細](master-details.md)
- [ナビゲーションの基本](../basics/navigation-basics.md)
- [UWP 用 Fluent Design の概要](/windows/apps/fluent-design-system)
