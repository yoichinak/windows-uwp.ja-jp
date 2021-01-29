---
description: NavigationView は、ご利用のアプリの最上位のナビゲーション パターンを実装するアダプティブ コントロールです。
title: ナビゲーション ビュー
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
pm-contact: yulikl
design-contact: kimsea
dev-contact: ''
doc-status: Published
ms.localizationpriority: medium
ms.custom: RS5
dev_langs:
- csharp
- cppwinrt
ms.openlocfilehash: af52032a0ebdf60e72f8bad0dd853696b717a05a
ms.sourcegitcommit: 9bd23e0e08ed834accebde4db96fc87f921d983d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2021
ms.locfileid: "98949145"
---
# <a name="navigation-view"></a>ナビゲーション ビュー

NavigationView コントロールでは、ご利用のアプリの最上位のナビゲーションが提供されます。 これは、さまざまな画面サイズに適応し、_上部_ のナビゲーション スタイルと _左側_ のナビゲーション スタイルの両方をサポートしています。

![上部のナビゲーション](images/nav-view-header.png)<br/>
_ナビゲーション ビューでは上部と左側の両方のナビゲーションのウィンドウまたはメニューがサポートされます_

**Windows UI ライブラリを入手する**

:::row:::
   :::column:::
      ![WinUI ロゴ](images/winui-logo-64x64.png)
   :::column-end:::
   :::column span="3":::
      **NavigationView** コントロールは、Windows アプリのための新しいコントロールと UI 機能を含む NuGet パッケージである Windows UI ライブラリの一部として含まれています。 インストール手順などの詳細については、[Windows UI ライブラリの概要](/uwp/toolkits/winui/)に関するページを参照してください。
   :::column-end:::
   :::column:::

   :::column-end:::
:::row-end:::

> **プラットフォーム API**: [Windows.UI.Xaml.Controls.NavigationView クラス](/uwp/api/windows.ui.xaml.controls.navigationview)
>
> **Windows UI ライブラリ API**: [Microsoft.UI.Xaml.Controls.NavigationView クラス](/uwp/api/microsoft.ui.xaml.controls.navigationview)
>
> "_上部_" や "_階層型_" のナビゲーションなど、NavigationView の一部の機能には、Windows 10 バージョン 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) 以降、または [Windows UI ライブラリ](/uwp/toolkits/winui/)が必要です。

## <a name="is-this-the-right-control"></a>これは適切なコントロールですか?

NavigationView は次の場合に役に立つアダプティブ ナビゲーション コントロールです。

- ご利用のアプリ全体で一貫性のあるナビゲーション エクスペリエンスを提供する。
- 小さいウィンドウの画面領域を節約する。
- 多くのナビゲーション カテゴリへのアクセスを整理する。

その他のナビゲーション パターンについては、[ナビゲーション デザインの基本](../basics/navigation-basics.md)に関するページを参照してください。

## <a name="examples"></a>例

<table>
<th align="left">XAML コントロール ギャラリー<th>
<tr>
<td><img src="images/XAML-controls-gallery-app-icon-sm.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML コントロール ギャラリー</strong> アプリがインストールされている場合、こちらをクリックして<a href="xamlcontrolsgallery:/item/NavigationView">アプリを開き、NavigationView の動作を確認</a>してください。</p>
    <ul>
    <li><a href="https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt">XAML コントロール ギャラリー アプリを入手する (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">ソース コード (GitHub) を入手する</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="display-modes"></a>表示モード

> PaneDisplayMode プロパティには、Windows 10 バージョン 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) 以降、または [Windows UI ライブラリ](/uwp/toolkits/winui/)が必要です。

PaneDisplayMode プロパティを使用すれば、NavigationView でさまざまなナビゲーション スタイルまたは表示モードを構成することができます。

:::row:::
    :::column:::
    ### <a name="top"></a>上
    ウィンドウはコンテンツの上に配置されます。</br>
    `PaneDisplayMode="Top"`
    :::column-end:::
    :::column span="2":::
    ![上部のナビゲーションの例](images/displaymode-top.png)
    :::column-end:::
:::row-end:::

次の場合に _上部_ ナビゲーションをお勧めします。

- 同じ重要度の最上位ナビゲーション カテゴリが 5 個以下であり、ドロップダウン オーバーフロー メニューとなる追加の最上位ナビゲーション カテゴリはいずれも、それほど重要ではないと考えられる。
- 画面上にナビゲーション オプションをすべて表示する必要がある。
- ご利用のアプリのコンテンツ用にスペースを追加したい。
- アイコンでは、ご利用のアプリのナビゲーション カテゴリを明確に説明できない。

:::row:::
    :::column:::
    ### <a name="left"></a>左
    ウィンドウはコンテンツの左側に展開および配置されます。</br>
    `PaneDisplayMode="Left"`
    :::column-end:::
    :::column span="2":::
    ![展開された左側のナビゲーションのウィンドウの例](images/displaymode-left.png)
    :::column-end:::
:::row-end:::

次の場合に _左側_ のナビゲーションをお勧めします。

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

### <a name="auto"></a>自動

既定では、PaneDisplayMode は Auto に設定されます。自動モードでは、ナビゲーション ビューは、ウィンドウが狭いときは LeftMinimal または LeftCompact となり、ウィンドウが広くなると Left に切り替わります。 詳細については、「[アダプティブ動作](#adaptive-behavior)」セクションを参照してください。

![左側のナビゲーションの既定のアダプティブ動作](images/displaymode-auto.png)<br/>
_ナビゲーション ビューの既定のアダプティブ動作_

## <a name="anatomy"></a>構造

これらのイメージは、_上部_ または _左側_ のナビゲーションが構成されている場合の、ウィンドウ、ヘッダー、およびコントロールのコンテンツ領域のレイアウトを示します。

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
- [アプリ設定](../app-settings/guidelines-for-app-settings.md)のエントリ ポイント (オプション)。 設定項目を非表示にするには、[IsSettingsVisible](/uwp/api/windows.ui.xaml.controls.navigationview.IsSettingsVisible) プロパティを **false** に設定します。

また、左側のウィンドウには次のものが含まれています。

- ウィンドウの開閉を切り替えるメニュー ボタン。 [IsPaneToggleButtonVisible](/uwp/api/windows.ui.xaml.controls.navigationview.IsPaneToggleButtonVisible) プロパティを使うと、ウィンドウが開いたとき、大きなアプリ ウィンドウで、このボタンを非表示にすることを選択できます。

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

#### <a name="footer-menu-items"></a>フッター メニュー項目
[FooterMenuItems](/uwp/api/microsoft.ui.xaml.controls.navigationview.FooterMenuItems) を使用すると、ナビゲーション ペインの最後にナビゲーション項目を配置できるのに対し、[MenuItems](/uwp/api/microsoft.ui.xaml.controls.navigationview.MenuItems) プロパティを使用すると、ペインの先頭に項目が配置されます。

FooterMenuItems は、既定では Settings 項目の前に表示されます。 Settings 項目は、[`IsSettingsVisible`](/uwp/api/microsoft.ui.xaml.controls.navigationview.IsSettingsVisible) プロパティを使用して切り替えることもできます。

FooterMenuItems には Navigation 項目だけを配置する必要があります。ペインのフッターと位置を合わせる必要がある他のコンテンツは、[PaneFooter](/uwp/api/microsoft.ui.xaml.controls.navigationview.PaneFooter) に配置する必要があります。

NavigationView に FooterMenuItems を追加する方法の例については、「[FooterMenuItems クラス](/uwp/api/microsoft.ui.xaml.controls.navigationview.FooterMenuItems)」を参照してください。 

次に示す画像は、フッター メニューに [アカウント]、[カート]、[ヘルプ] の各ナビゲーション項目が含まれる NavigationView です。 

![FooterMenuItems が含まれる NavigationView](images/footermenu-leftmode.png)

#### <a name="pane-footer"></a>ウィンドウのフッター

[PaneFooter](/uwp/api/microsoft.ui.xaml.controls.navigationview.PaneFooter) プロパティに自由形式のコンテンツを追加すると、それをウィンドウのフッターに配置することができます。

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

[PaneTitle](/uwp/api/windows.ui.xaml.controls.navigationview.PaneTitle) プロパティを設定することで、ウィンドウのヘッダー領域にテキスト コンテンツを配置できます。 このプロパティは文字列を取り、メニュー ボタンの横にテキストを表示します。

画像やロゴなどのテキスト以外のコンテンツを追加するには、[PaneHeader](/uwp/api/windows.ui.xaml.controls.navigationview.PaneHeader) プロパティに追加することで、ウィンドウのヘッダーに任意の要素を配置できます。

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

自由形式のコンテンツは、[PaneCustomContent](/uwp/api/windows.ui.xaml.controls.navigationview.PaneCustomContent) プロパティに追加することでウィンドウに配置できます。

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

NavigationView が **Minimal** モードの場合はコンテンツ領域に 12 ピクセルの余白を設定し、それ以外の場合は 24 ピクセルの余白を設定することをお勧めします。

## <a name="adaptive-behavior"></a>アダプティブ動作

既定では、ナビゲーション ビューは、利用可能な画面領域の大きさに基づいて自動的に表示モードが変わります。 [CompactModeThresholdWidth](/uwp/api/windows.ui.xaml.controls.navigationview.compactmodethresholdwidth) プロパティおよび [ExpandedModeThresholdWidth](/uwp/api/windows.ui.xaml.controls.navigationview.expandedmodethresholdwidth) プロパティでは、表示モードが変更されるブレークポイントが指定されます。 これらの値を変更することで、アダプティブ表示モードの動作をカスタマイズできます。

### <a name="default"></a>既定

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
<Grid>
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

## <a name="navigation"></a>［ナビゲーション］

ナビゲーション ビューではナビゲーション タスクは自動的に実行されません。 ユーザーがナビゲーション項目をタップすると、ナビゲーション ビューではその項目が選択済みとして表示され、[ItemInvoked](/uwp/api/windows.ui.xaml.controls.navigationview.ItemInvoked) イベントが発生します。 タップによって新しい項目が選択されると、[SelectionChanged](/uwp/api/windows.ui.xaml.controls.navigationview.SelectionChanged) イベントも発生します。

どちらのイベントを処理しても、要求されたナビゲーションに関連するタスクを実行できます。 どちらを処理する必要があるかは、ご利用のアプリに求める動作によって異なります。 通常は、要求されたページに移動し、これらのイベントに応じてナビゲーション ビューのヘッダーを更新します。

**ItemInvoked** は、ユーザーがナビゲーション項目をタップするたびに、それが既に選択されている場合でも発生します。 (項目は、マウス、キーボード、またはその他の入力を使用して同等の操作で呼び出すこともできます。 詳細については、「[入力と操作](../input/index.md)」を参照してください)。ItemInvoked ハンドラー内を移動すると、既定ではページが再読み込みされ、重複エントリがナビゲーション スタックに追加されます。 項目が呼び出されたときに移動する場合は、ページの再読み込みを禁止するか、またはページが再読み込みされるときにナビゲーション バックスタック内に重複エントリが決して作成されないようにする必要があります。 (コード例を参照してください)。

**SelectionChanged** は、現在選択されていない項目をユーザーが呼び出すことで発生させることも、選択された項目をプログラムで変更することによって発生させることもできます。 ユーザーが項目を呼び出したために選択の変更が発生した場合は、最初に ItemInvoked イベントが発生します。 選択の変更がプログラムによるものである場合、ItemInvoked は発生しません。

すべてのナビゲーション項目は、[MenuItems](/uwp/api/microsoft.ui.xaml.controls.navigationview.MenuItems) または [FooterMenuItems](/uwp/api/microsoft.ui.xaml.controls.navigationview.FooterMenuItems) の一部であるかどうかにかかわらず、同じ選択モデルの一部です。 選択できるナビゲーション項目は、一度に 1 つだけです。

### <a name="backwards-navigation"></a>逆方向のナビゲーション

NavigationView には組み込みの [戻る] ボタンがありますが、前方ナビゲーションと同様に、後方ナビゲーションは自動的には実行されません。 ユーザーが [戻る] ボタンをタップすると、[BackRequested](/uwp/api/windows.ui.xaml.controls.navigationview.BackRequested) イベントが発生します。 このイベントを処理して、後方ナビゲーションを実行します。 詳細については、[ナビゲーション履歴と後方ナビゲーション](../basics/navigation-history-and-backwards-navigation.md)に関するページを参照してください。

Minimal モードまたは Compact モードでは、ナビゲーション ビュー ウィンドウはポップアップとして開きます。 この場合、[戻る] ボタンをクリックすると、ウィンドウが閉じられ、代わりに **PaneClosing** イベントが発生します。

以下のプロパティを設定することで、[戻る] ボタンを非表示または無効にすることができます。

- [IsBackButtonVisible](/uwp/api/windows.ui.xaml.controls.navigationview.IsBackButtonVisible): [戻る] ボタンを表示および非表示にするために使用します。 このプロパティは [NavigationViewBackButtonVisible](/uwp/api/windows.ui.xaml.controls.navigationviewbackbuttonvisible) 列挙体の値を取り、既定では **Auto** に設定されています。 ボタンが折りたたまれると、このボタン用のスペースはレイアウト内に確保されません。
- [IsBackEnabled](/uwp/api/windows.ui.xaml.controls.navigationview.IsBackEnabled): [戻る] ボタンを有効または無効にするために使用します。 このプロパティは、ご利用のナビゲーション フレームの [CanGoBack](/uwp/api/windows.ui.xaml.controls.frame.cangoback) プロパティにデータ バインドすることができます。 **IsBackEnabled** が **false** の場合、**BackRequested** は発生しません。

:::row:::
    :::column:::
        ![左側のナビゲーション ウィンドウのナビゲーション ビューの [戻る] ボタン](images/leftnav-back.png)<br/>
        _左側のナビゲーション ウィンドウの [戻る] ボタン_
    :::column-end:::
    :::column:::
        ![上部ナビゲーション ウィンドウのナビゲーション ビューの [戻る] ボタン](images/topnav-back.png)<br/>
        _上部ナビゲーション ウィンドウの [戻る] ボタン_
    :::column-end:::
:::row-end:::

## <a name="code-example"></a>コードの例

> [!IMPORTANT]
> Windows UI (WinUI) ライブラリ ツールキットを利用するプロジェクトの場合、同じ準備段階のセットアップの手順を実行します。 背景、セットアップ、およびサポート情報の詳細については、「[Windows UI ライブラリの概要](/uwp/toolkits/winui/getting-started)」を参照してください。

この例では、ウィンドウ サイズが大きい場合の上部のナビゲーション ウィンドウとウィンドウ サイズが小さい場合の左側のナビゲーション ウィンドウの両方で **NavigationView** を使用する方法を示します。 これは、**VisualStateManager** で *上部* のナビゲーション設定を削除することにより、左側のみのナビゲーションに適応させることができます。

この例は、一般的なシナリオの多くで機能するナビゲーション データを設定するための推奨方法を示しています。 また、**NavigationView** の [戻る] ボタンとキーボード ナビゲーションを使用して後方ナビゲーションを実装する方法についても示します。

このコードでは、移動先である次の名前を含むページが、ご利用のアプリに含まれていることを前提としています。*HomePage*、*AppsPage*、*GamesPage*、*MusicPage*、*MyContentPage*、および *SettingsPage*。 これらのページのコードは示されていません。

> [!IMPORTANT]
> アプリのページに関する情報は [ValueTuple](/dotnet/api/system.valuetuple) に格納されています。 この構造体を使用するには、ご利用のアプリ プロジェクトの最小バージョンが SDK 17763 以上である必要があります。 前のバージョンの Windows 10 をターゲットとする NavigationView の WinUI バージョンを使用する場合は、代わりに [System.ValueTuple NuGet パッケージ](https://www.nuget.org/packages/System.ValueTuple/)を使用することができます。

> [!IMPORTANT]
> このコードでは、NavigationView の [Windows UI ライブラリ](/uwp/toolkits/winui/) バージョンの使い方を示します。 プラットフォーム バージョンの NavigationView を代わりに使用する場合、アプリ プロジェクトの最小バージョンは SDK 17763 以上である必要があります。 プラットフォーム バージョンを使用するには、`muxc:` へのすべての参照を削除します。

```xaml
<Page ... xmlns:muxc="using:Microsoft.UI.Xaml.Controls" ... >
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
                        MinWindowWidth="{x:Bind NavViewCompactModeThresholdWidth}"/>
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
</Page>
```

> [!IMPORTANT]
> このコードでは、NavigationView の [Windows UI ライブラリ](/uwp/toolkits/winui/) バージョンの使い方を示します。 プラットフォーム バージョンの NavigationView を代わりに使用する場合、アプリ プロジェクトの最小バージョンは SDK 17763 以上である必要があります。 プラットフォーム バージョンを使用するには、`muxc` へのすべての参照を削除します。

```csharp
// Add "using" for WinUI controls.
// using muxc = Microsoft.UI.Xaml.Controls;

private double NavViewCompactModeThresholdWidth { get { return NavView.CompactModeThresholdWidth; } }

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
    NavView_Navigate("home", new Windows.UI.Xaml.Media.Animation.EntranceNavigationTransitionInfo());

    // Listen to the window directly so the app responds
    // to accelerator keys regardless of which element has focus.
    Window.Current.CoreWindow.Dispatcher.AcceleratorKeyActivated +=
        CoreDispatcher_AcceleratorKeyActivated;

    Window.Current.CoreWindow.PointerPressed += CoreWindow_PointerPressed;

    SystemNavigationManager.GetForCurrentView().BackRequested += System_BackRequested;
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

// NavView_SelectionChanged is not used in this example, but is shown for completeness.
// You will typically handle either ItemInvoked or SelectionChanged to perform navigation,
// but not both.
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

private void NavView_Navigate(
    string navItemTag,
    Windows.UI.Xaml.Media.Animation.NavigationTransitionInfo transitionInfo)
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
    TryGoBack();
}

private void CoreDispatcher_AcceleratorKeyActivated(CoreDispatcher sender, AcceleratorKeyEventArgs e)
{
    // When Alt+Left are pressed navigate back
    if (e.EventType == CoreAcceleratorKeyEventType.SystemKeyDown
        && e.VirtualKey == VirtualKey.Left
        && e.KeyStatus.IsMenuKeyDown == true
        && !e.Handled)
    {
        e.Handled = TryGoBack();
    }
}

private void System_BackRequested(object sender, BackRequestedEventArgs e)
{
    if (!e.Handled)
    {
        e.Handled = TryGoBack();
    }
}

private void CoreWindow_PointerPressed(CoreWindow sender, PointerEventArgs e)
{
    // Handle mouse back button.
    if (e.CurrentPoint.Properties.IsXButton1Pressed)
    {
        e.Handled = TryGoBack();
    }
}

private bool TryGoBack();
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

> [!NOTE]
> このコード例の [C++/WinRT](../../cpp-and-winrt-apis/index.md) バージョンについては、最初に **空のアプリ (C++/WinRT)** プロジェクト テンプレートに基づいて新しいプロジェクトを作成してから、登録情報内のコードを指定されたソース コード ファイルに追加します。 登録情報に示されているとおりにソース コードを使用するには、新しいプロジェクトに *NavigationViewCppWinRT* という名前を付けます

```cppwinrt
// MainPage.idl
runtimeclass MainPage : Windows.UI.Xaml.Controls.Page
{
    ...
    Double NavViewCompactModeThresholdWidth{ get; };
}

// pch.h
...
#include "winrt/Windows.UI.Xaml.Input.h"
#include "winrt/Windows.UI.Xaml.Media.Animation.h"
#include "winrt/Microsoft.UI.Xaml.Controls.h"
#include "winrt/Microsoft.UI.Xaml.XamlTypeInfo.h"
#include <winrt/Windows.UI.Core.h>
#include "winrt/Windows.UI.Input.h"


// MainPage.h
#pragma once

#include "MainPage.g.h"

namespace muxc
{
    using namespace winrt::Microsoft::UI::Xaml::Controls;
};

namespace wuxc
{
    using namespace winrt::Windows::UI::Xaml::Controls;
};

namespace winrt::NavigationViewCppWinRT::implementation
{
    struct MainPage : MainPageT<MainPage>
    {
        MainPage();

        double NavViewCompactModeThresholdWidth();
        void ContentFrame_NavigationFailed(
            Windows::Foundation::IInspectable const& /* sender */,
            Windows::UI::Xaml::Navigation::NavigationFailedEventArgs const& args);
        void NavView_Loaded(
            Windows::Foundation::IInspectable const& /* sender */,
            Windows::UI::Xaml::RoutedEventArgs const& /* args */);
        void NavView_ItemInvoked(
            Windows::Foundation::IInspectable const& /* sender */,
            muxc::NavigationViewItemInvokedEventArgs const& args);

        // NavView_SelectionChanged is not used in this example, but is shown for completeness.
        // You'll typically handle either ItemInvoked or SelectionChanged to perform navigation,
        // but not both.
        void NavView_SelectionChanged(
            muxc::NavigationView const& /* sender */,
            muxc::NavigationViewSelectionChangedEventArgs const& args);
        void NavView_Navigate(
            std::wstring navItemTag,
            Windows::UI::Xaml::Media::Animation::NavigationTransitionInfo const& transitionInfo);
        void NavView_BackRequested(
            muxc::NavigationView const& /* sender */,
            muxc::NavigationViewBackRequestedEventArgs const& /* args */);
        void On_Navigated(
            Windows::Foundation::IInspectable const& /* sender */,
            Windows::UI::Xaml::Navigation::NavigationEventArgs const& args);
        void CoreDispatcher_AcceleratorKeyActivated(
            Windows::UI::Core::CoreDispatcher const& /* sender */,
            Windows::UI::Core::AcceleratorKeyEventArgs const& args);
        void CoreWindow_PointerPressed(
            Windows::UI::Core::CoreWindow const& /* sender */,
            Windows::UI::Core::PointerEventArgs const& args);
        void System_BackRequested(
            Windows::Foundation::IInspectable const& /* sender */,
            Windows::UI::Core::BackRequestedEventArgs const& args);
        bool TryGoBack();

    private:
        // Vector of std::pair holding the Navigation Tag and the relative Navigation Page.
        std::vector<std::pair<std::wstring, Windows::UI::Xaml::Interop::TypeName>> m_pages;
    };
}

namespace winrt::NavigationViewCppWinRT::factory_implementation
{
    struct MainPage : MainPageT<MainPage, implementation::MainPage>
    {
    };
}

// MainPage.cpp
#include "pch.h"
#include "MainPage.h"
#include "MainPage.g.cpp"

namespace winrt::NavigationViewCppWinRT::implementation
{
    MainPage::MainPage()
    {
        InitializeComponent();
        m_pages.push_back(std::make_pair<std::wstring, Windows::UI::Xaml::Interop::TypeName>
            (L"home", winrt::xaml_typename<NavigationViewCppWinRT::HomePage>()));
        m_pages.push_back(std::make_pair<std::wstring, Windows::UI::Xaml::Interop::TypeName>
            (L"apps", winrt::xaml_typename<NavigationViewCppWinRT::AppsPage>()));
        m_pages.push_back(std::make_pair<std::wstring, Windows::UI::Xaml::Interop::TypeName>
            (L"games", winrt::xaml_typename<NavigationViewCppWinRT::GamesPage>()));
        m_pages.push_back(std::make_pair<std::wstring, Windows::UI::Xaml::Interop::TypeName>
            (L"music", winrt::xaml_typename<NavigationViewCppWinRT::MusicPage>()));
    }

    double MainPage::NavViewCompactModeThresholdWidth()
    {
        return NavView().CompactModeThresholdWidth();
    }

    void MainPage::ContentFrame_NavigationFailed(
        Windows::Foundation::IInspectable const& /* sender */,
        Windows::UI::Xaml::Navigation::NavigationFailedEventArgs const& args)
    {
        throw winrt::hresult_error(
            E_FAIL, winrt::hstring(L"Failed to load Page ") + args.SourcePageType().Name);
    }

    // List of ValueTuple holding the Navigation Tag and the relative Navigation Page
    std::vector<std::pair<std::wstring, Windows::UI::Xaml::Interop::TypeName>> m_pages;

    void MainPage::NavView_Loaded(
        Windows::Foundation::IInspectable const& /* sender */,
        Windows::UI::Xaml::RoutedEventArgs const& /* args */)
    {
        // You can also add items in code.
        NavView().MenuItems().Append(muxc::NavigationViewItemSeparator());
        muxc::NavigationViewItem navigationViewItem;
        navigationViewItem.Content(winrt::box_value(L"My content"));
        navigationViewItem.Icon(wuxc::SymbolIcon(static_cast<wuxc::Symbol>(0xF1AD)));
        navigationViewItem.Tag(winrt::box_value(L"content"));
        NavView().MenuItems().Append(navigationViewItem);
        m_pages.push_back(
            std::make_pair<std::wstring, Windows::UI::Xaml::Interop::TypeName>(
                L"content", winrt::xaml_typename<NavigationViewCppWinRT::MyContentPage>()));

        // Add handler for ContentFrame navigation.
        ContentFrame().Navigated({ this, &MainPage::On_Navigated });

        // NavView doesn't load any page by default, so load home page.
        NavView().SelectedItem(NavView().MenuItems().GetAt(0));
        // If navigation occurs on SelectionChanged, then this isn't needed.
        // Because we use ItemInvoked to navigate, we need to call Navigate
        // here to load the home page.
        NavView_Navigate(L"home",
            Windows::UI::Xaml::Media::Animation::EntranceNavigationTransitionInfo());

        // Listen to the window directly so the app responds
        // to accelerator keys regardless of which element has focus.
        winrt::Windows::UI::Xaml::Window::Current().CoreWindow().Dispatcher().
            AcceleratorKeyActivated({ this, &MainPage::CoreDispatcher_AcceleratorKeyActivated });
 
        winrt::Windows::UI::Xaml::Window::Current().CoreWindow().
            PointerPressed({ this, &MainPage::CoreWindow_PointerPressed });
 
        Windows::UI::Core::SystemNavigationManager::GetForCurrentView().
            BackRequested({ this, &MainPage::System_BackRequested });

    }

    void MainPage::NavView_ItemInvoked(
        Windows::Foundation::IInspectable const& /* sender */,
        muxc::NavigationViewItemInvokedEventArgs const& args)
    {
        if (args.IsSettingsInvoked())
        {
            NavView_Navigate(L"settings", args.RecommendedNavigationTransitionInfo());
        }
        else if (args.InvokedItemContainer())
        {
            NavView_Navigate(
                winrt::unbox_value_or<winrt::hstring>(
                    args.InvokedItemContainer().Tag(), L"").c_str(),
                args.RecommendedNavigationTransitionInfo());
        }
    }

    // NavView_SelectionChanged is not used in this example, but is shown for completeness.
    // You will typically handle either ItemInvoked or SelectionChanged to perform navigation,
    // but not both.
    void MainPage::NavView_SelectionChanged(
        muxc::NavigationView const& /* sender */,
        muxc::NavigationViewSelectionChangedEventArgs const& args)
    {
        if (args.IsSettingsSelected())
        {
            NavView_Navigate(L"settings", args.RecommendedNavigationTransitionInfo());
        }
        else if (args.SelectedItemContainer())
        {
            NavView_Navigate(
                winrt::unbox_value_or<winrt::hstring>(
                    args.SelectedItemContainer().Tag(), L"").c_str(),
                args.RecommendedNavigationTransitionInfo());
        }
    }

    void MainPage::NavView_Navigate(
        std::wstring navItemTag,
        Windows::UI::Xaml::Media::Animation::NavigationTransitionInfo const& transitionInfo)
    {
        Windows::UI::Xaml::Interop::TypeName pageTypeName;
        if (navItemTag == L"settings")
        {
            pageTypeName = winrt::xaml_typename<NavigationViewCppWinRT::SettingsPage>();
        }
        else
        {
            for (auto&& eachPage : m_pages)
            {
                if (eachPage.first == navItemTag)
                {
                    pageTypeName = eachPage.second;
                    break;
                }
            }
        }
        // Get the page type before navigation so you can prevent duplicate
        // entries in the backstack.
        Windows::UI::Xaml::Interop::TypeName preNavPageType =
            ContentFrame().CurrentSourcePageType();

        // Navigate only if the selected page isn't currently loaded.
        if (pageTypeName.Name != L"" && preNavPageType.Name != pageTypeName.Name)
        {
            ContentFrame().Navigate(pageTypeName, nullptr, transitionInfo);
        }
    }

    void MainPage::NavView_BackRequested(
        muxc::NavigationView const& /* sender */,
        muxc::NavigationViewBackRequestedEventArgs const& /* args */)
    {
        TryGoBack();
    }

    void MainPage::CoreDispatcher_AcceleratorKeyActivated(
        Windows::UI::Core::CoreDispatcher const& /* sender */,
        Windows::UI::Core::AcceleratorKeyEventArgs const& args)
    {
        // When Alt+Left are pressed navigate back
        if (args.EventType() == Windows::UI::Core::CoreAcceleratorKeyEventType::SystemKeyDown
            && args.VirtualKey() == Windows::System::VirtualKey::Left
            && args.KeyStatus().IsMenuKeyDown
            && !args.Handled())
        {
            args.Handled(TryGoBack());
        }
    }
 
    void MainPage::CoreWindow_PointerPressed(
        Windows::UI::Core::CoreWindow const& /* sender */,
        Windows::UI::Core::PointerEventArgs const& args)
    {
        // Handle mouse back button.
        if (args.CurrentPoint().Properties().IsXButton1Pressed())
        {
            args.Handled(TryGoBack());
        }
    }
 
    void MainPage::System_BackRequested(
        Windows::Foundation::IInspectable const& /* sender */,
        Windows::UI::Core::BackRequestedEventArgs const& args)
    {
        if (!args.Handled())
        {
            args.Handled(TryGoBack());
        }
    }
 
    bool MainPage::TryGoBack()
    {
        if (!ContentFrame().CanGoBack())
            return false;
        // Don't go back if the nav pane is overlayed.
        if (NavView().IsPaneOpen() &&
            (NavView().DisplayMode() == muxc::NavigationViewDisplayMode::Compact ||
                NavView().DisplayMode() == muxc::NavigationViewDisplayMode::Minimal))
            return false;
        ContentFrame().GoBack();
        return true;
    }  

    void MainPage::On_Navigated(
        Windows::Foundation::IInspectable const& /* sender */,
        Windows::UI::Xaml::Navigation::NavigationEventArgs const& args)
    {
        NavView().IsBackEnabled(ContentFrame().CanGoBack());

        if (ContentFrame().SourcePageType().Name ==
            winrt::xaml_typename<NavigationViewCppWinRT::SettingsPage>().Name)
        {
            // SettingsItem is not part of NavView.MenuItems, and doesn't have a Tag.
            NavView().SelectedItem(NavView().SettingsItem().as<muxc::NavigationViewItem>());
            NavView().Header(winrt::box_value(L"Settings"));
        }
        else if (ContentFrame().SourcePageType().Name != L"")
        {
            for (auto&& eachPage : m_pages)
            {
                if (eachPage.second.Name == args.SourcePageType().Name)
                {
                    for (auto&& eachMenuItem : NavView().MenuItems())
                    {
                        auto navigationViewItem =
                            eachMenuItem.try_as<muxc::NavigationViewItem>();
                        {
                            if (navigationViewItem)
                            {
                                winrt::hstring hstringValue =
                                    winrt::unbox_value_or<winrt::hstring>(
                                        navigationViewItem.Tag(), L"");
                                if (hstringValue == eachPage.first)
                                {
                                    NavView().SelectedItem(navigationViewItem);
                                    NavView().Header(navigationViewItem.Content());
                                }
                            }
                        }
                    }
                    break;
                }
            }
        }
    }
}
```

### <a name="alternative-cwinrt-implementation"></a>別の C++ /WinRT 実装

上で示した C# および C++/WinRT のコードは、両方のバージョンで同じ XAML マークアップを使用できるように設計されています。 ただし、このセクションに記載されている C++/WinRT バージョンを実装する別の方法があり、これが適切である場合もあります。

次に示すのは、**NavView_ItemInvoked** ハンドラーの別のバージョンです。 このバージョンのハンドラーの手法では、移動先のページの完全な型名を ([**NavigationViewItem**](/uwp/api/windows.ui.xaml.controls.navigationviewitem) のタグに) 最初に格納する必要があります。 ハンドラーでは、その値のボックス化を解除し、それを [**Windows::UI::Xaml::Interop::TypeName**](/uwp/api/windows.ui.xaml.interop.typename) オブジェクトに変換し、それを使用して目的のページに移動します。 上記の例に示した `_pages` という名前のマッピング変数は必要ありません。また、ご利用のタグ内の値が有効な型であることを確認するための単体テストを作成することができます。 「[C++/WinRT を使用した IInspectable へのスカラー値のボックス化とボックス化解除](../../cpp-and-winrt-apis/boxing.md)」も参照してください。

```cppwinrt
void MainPage::NavView_ItemInvoked(
    Windows::Foundation::IInspectable const & /* sender */,
    Windows::UI::Xaml::Controls::NavigationViewItemInvokedEventArgs const & args)
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

## <a name="hierarchical-navigation"></a>階層型ナビゲーション
アプリによっては、ナビゲーション項目の単純なリストだけでは対応しきれない、より複雑な階層構造が存在する場合があります。 最上位レベルのナビゲーション項目を使用してページのカテゴリを表示し、特定のページを子項目で表示することができます。 これは、他のページにリンクされているだけのハブスタイルのページがある場合にも便利です。 このような場合には、階層型の NavigationView を作成する必要があります。

入れ子になったナビゲーション項目の階層型リストをペインに表示するには、**NavigationViewItem** の [MenuItems](/uwp/api/microsoft.ui.xaml.controls.navigationviewitem.menuitems) プロパティか [MenuItemsSource](/uwp/api/microsoft.ui.xaml.controls.navigationviewitem.menuitemssource) プロパティを使用します。
各 NavigationViewItem には、他の NavigationViewItems と整理のための要素 (項目ヘッダーや区切り記号など) 含めることができます。 `MenuItemsSource` を使用するときに階層型リストを表示するには、`ItemTemplate` が NavigationViewItem になるように設定し、その `MenuItemsSource` プロパティを階層の次のレベルにバインドします。

NavigationViewItem には入れ子になったレベルをいくつでも含めることができますが、アプリのナビゲーション階層は浅く保つことをお勧めします。 使いやすさと理解しやすさを考慮すると、レベルは 2 つが理想的でしょう。

NavigationView では、Top、Left、LeftCompact のペイン表示モードで階層が表示されます。 各ペイン表示モードで展開されたサブツリーは次のようになります。

![階層構造を備えた NavigationView](images/navigation-view-hierarchy-labeled.png)

### <a name="adding-a-hierarchy-of-items-in-markup"></a>マークアップでの項目の階層の追加
マークアップでアプリのナビゲーション階層を宣言します。

```Xaml
<!-- xmlns:muxc="using:Microsoft.UI.Xaml.Controls" -->
<muxc:NavigationView>
    <muxc:NavigationView.MenuItems>
        <muxc:NavigationViewItem Content="Home" Icon="Home" ToolTipService.ToolTip="Home"/>
        <muxc:NavigationViewItem Content="Collections" Icon="Keyboard" ToolTipService.ToolTip="Collections">
            <muxc:NavigationViewItem.MenuItems>
                <muxc:NavigationViewItem Content="Notes" Icon="Page" ToolTipService.ToolTip="Notes"/>
                <muxc:NavigationViewItem Content="Mail" Icon="Mail" ToolTipService.ToolTip="Mail"/>
            </muxc:NavigationViewItem.MenuItems>
        </muxc:NavigationViewItem>
    </muxc:NavigationView.MenuItems>
</muxc:NavigationView>
```

### <a name="adding-a-hierarchy-of-items-using-data-binding"></a>データ バインディングを使用した項目の階層の追加

メニュー項目の階層を NavigationView に追加する方法 
* MenuItemsSource プロパティを階層型データにバインドする
* 項目テンプレートを NavigationViewMenuItem として定義し、その内容をメニュー項目のラベルに設定し、その MenuItemsSource プロパティを階層の次のレベルにバインドする

この例では、[Expanding](/uwp/api/microsoft.ui.xaml.controls.navigationview.expanding) イベントと [Collapsed](/uwp/api/microsoft.ui.xaml.controls.navigationview.collapsed) イベントも示されています。 これらのイベントは、子があるメニュー項目に対して発生します。

```xaml
<Page ... xmlns:muxc="using:Microsoft.UI.Xaml.Controls" ... >
    <Page.Resources>
        <DataTemplate x:Key="NavigationViewMenuItem" x:DataType="local:Category">
            <muxc:NavigationViewItem Content="{x:Bind Name}" MenuItemsSource="{x:Bind Children}"/>
        </DataTemplate>
    </Page.Resources>

    <Grid>
        <muxc:NavigationView x:Name="navview" 
    MenuItemsSource="{x:Bind Categories, Mode=OneWay}" 
    MenuItemTemplate="{StaticResource NavigationViewMenuItem}" 
    ItemInvoked="{x:Bind OnItemInvoked}" 
    Expanding="OnItemExpanding" 
    Collapsed="OnItemCollapsed" 
    PaneDisplayMode="Left">
            <StackPanel Margin="10,10,0,0">
                <TextBlock Margin="0,10,0,0" x:Name="ExpandingItemLabel" Text="Last Expanding: N/A"/>
                <TextBlock x:Name="CollapsedItemLabel" Text="Last Collapsed: N/A"/>
            </StackPanel>
        </muxc:NavigationView>
    </Grid>
</Page>
```

```csharp
public class Category
{
    public String Name { get; set; }
    public String CategoryIcon { get; set; }
    public ObservableCollection<Category> Children { get; set; }
}

public sealed partial class HierarchicalNavigationViewDataBinding : Page
{
    public HierarchicalNavigationViewDataBinding()
    {
        this.InitializeComponent();
    }  

    public ObservableCollection<Category> Categories = new ObservableCollection<Category>()
    {
        new Category(){
            Name = "Menu item 1",
            CategoryIcon = "Icon",
            Children = new ObservableCollection<Category>() {
                new Category(){
                    Name = "Menu item 2",
                    CategoryIcon = "Icon",
                    Children = new ObservableCollection<Category>() {
                        new Category() {
                            Name  = "Menu item 3",
                            CategoryIcon = "Icon",
                            Children = new ObservableCollection<Category>() {
                                new Category() { Name  = "Menu item 4", CategoryIcon = "Icon" },
                                new Category() { Name  = "Menu item 5", CategoryIcon = "Icon" }
                            }
                        }
                    }
                }
            }
        },
        new Category(){
            Name = "Menu item 6",
            CategoryIcon = "Icon",
            Children = new ObservableCollection<Category>() {
                new Category(){
                    Name = "Menu item 7",
                    CategoryIcon = "Icon",
                    Children = new ObservableCollection<Category>() {
                        new Category() { Name  = "Menu item 8", CategoryIcon = "Icon" },
                        new Category() { Name  = "Menu item 9", CategoryIcon = "Icon" }
                    }
                }
            }
        },
        new Category(){ Name = "Menu item 10", CategoryIcon = "Icon" }
    };

    private void OnItemInvoked(object sender, NavigationViewItemInvokedEventArgs e)
    {
        var clickedItem = e.InvokedItem;
        var clickedItemContainer = e.InvokedItemContainer;
    }
    private void OnItemExpanding(object sender, NavigationViewItemExpandingEventArgs e)
    {
        var nvib = e.ExpandingItemContainer;
        var name = "Last expanding: " + nvib.Content.ToString();
        ExpandingItemLabel.Text = name;
    }
    private void OnItemCollapsed(object sender, NavigationViewItemCollapsedEventArgs e)
    {
        var nvib = e.CollapsedItemContainer;
        var name = "Last collapsed: " + nvib.Content;
        CollapsedItemLabel.Text = name;
    }
}
```

```cppwinrt
// Category.idl
namespace HierarchicalNavigationViewDataBinding
{
    runtimeclass Category
    {
        String Name;
        String CategoryIcon;
        Windows.Foundation.Collections.IObservableVector<Category> Children;
    }
}

// Category.h
#pragma once
#include "Category.g.h"

namespace winrt::HierarchicalNavigationViewDataBinding::implementation
{
    struct Category : CategoryT<Category>
    {
        Category();
        Category(winrt::hstring name,
            winrt::hstring categoryIcon,
            Windows::Foundation::Collections::
                IObservableVector<HierarchicalNavigationViewDataBinding::Category> children);

        winrt::hstring Name();
        void Name(winrt::hstring const& value);
        winrt::hstring CategoryIcon();
        void CategoryIcon(winrt::hstring const& value);
        Windows::Foundation::Collections::
            IObservableVector<HierarchicalNavigationViewDataBinding::Category> Children();
        void Children(Windows::Foundation::Collections:
            IObservableVector<HierarchicalNavigationViewDataBinding::Category> const& value);

    private:
        winrt::hstring m_name;
        winrt::hstring m_categoryIcon;
        Windows::Foundation::Collections::
            IObservableVector<HierarchicalNavigationViewDataBinding::Category> m_children;
    };
}

// Category.cpp
#include "pch.h"
#include "Category.h"
#include "Category.g.cpp"

namespace winrt::HierarchicalNavigationViewDataBinding::implementation
{
    Category::Category()
    {
        m_children = winrt::single_threaded_observable_vector<HierarchicalNavigationViewDataBinding::Category>();
    }

    Category::Category(
        winrt::hstring name,
        winrt::hstring categoryIcon,
        Windows::Foundation::Collections::
            IObservableVector<HierarchicalNavigationViewDataBinding::Category> children)
    {
        m_name = name;
        m_categoryIcon = categoryIcon;
        m_children = children;
    }

    hstring Category::Name()
    {
        return m_name;
    }

    void Category::Name(hstring const& value)
    {
        m_name = value;
    }

    hstring Category::CategoryIcon()
    {
        return m_categoryIcon;
    }

    void Category::CategoryIcon(hstring const& value)
    {
        m_categoryIcon = value;
    }

    Windows::Foundation::Collections::IObservableVector<HierarchicalNavigationViewDataBinding::Category>
        Category::Children()
    {
        return m_children;
    }

    void Category::Children(
        Windows::Foundation::Collections::IObservableVector<HierarchicalNavigationViewDataBinding::Category>
            const& value)
    {
        m_children = value;
    }
}

// MainPage.idl
import "Category.idl";

namespace HierarchicalNavigationViewDataBinding
{
    [default_interface]
    runtimeclass MainPage : Windows.UI.Xaml.Controls.Page
    {
        MainPage();
        Windows.Foundation.Collections.IObservableVector<Category> Categories{ get; };
    }
}

// MainPage.h
#pragma once

#include "MainPage.g.h"

namespace muxc
{
    using namespace winrt::Microsoft::UI::Xaml::Controls;
};

namespace winrt::HierarchicalNavigationViewDataBinding::implementation
{
    struct MainPage : MainPageT<MainPage>
    {
        MainPage();

        Windows::Foundation::Collections::IObservableVector<HierarchicalNavigationViewDataBinding::Category>
            Categories();

        void OnItemInvoked(muxc::NavigationView const& sender, muxc::NavigationViewItemInvokedEventArgs const& args);
        void OnItemExpanding(
            muxc::NavigationView const& sender,
            muxc::NavigationViewItemExpandingEventArgs const& args);
        void OnItemCollapsed(
            muxc::NavigationView const& sender,
            muxc::NavigationViewItemCollapsedEventArgs const& args);

    private:
        Windows::Foundation::Collections::
            IObservableVector<HierarchicalNavigationViewDataBinding::Category> m_categories;
    };
}

namespace winrt::HierarchicalNavigationViewDataBinding::factory_implementation
{
    struct MainPage : MainPageT<MainPage, implementation::MainPage>
    {
    };
}

// MainPage.cpp
#include "pch.h"
#include "MainPage.h"
#include "MainPage.g.cpp"

#include "Category.h"

namespace winrt::HierarchicalNavigationViewDataBinding::implementation
{
    MainPage::MainPage()
    {
        InitializeComponent();

        m_categories = 
            winrt::single_threaded_observable_vector<HierarchicalNavigationViewDataBinding::Category>();

        auto menuItem10 = winrt::make_self<HierarchicalNavigationViewDataBinding::implementation::Category>
            (L"Menu item 10", L"Icon", nullptr);

        auto menuItem9 = winrt::make_self<HierarchicalNavigationViewDataBinding::implementation::Category>
            (L"Menu item 9", L"Icon", nullptr);
        auto menuItem8 = winrt::make_self<HierarchicalNavigationViewDataBinding::implementation::Category>
            (L"Menu item 8", L"Icon", nullptr);
        auto menuItem7Children = 
            winrt::single_threaded_observable_vector<HierarchicalNavigationViewDataBinding::Category>();
        menuItem7Children.Append(*menuItem9);
        menuItem7Children.Append(*menuItem8);

        auto menuItem7 = winrt::make_self<HierarchicalNavigationViewDataBinding::implementation::Category>
            (L"Menu item 7", L"Icon", menuItem7Children);
        auto menuItem6Children = 
            winrt::single_threaded_observable_vector<HierarchicalNavigationViewDataBinding::Category>();
        menuItem6Children.Append(*menuItem7);

        auto menuItem6 = winrt::make_self<HierarchicalNavigationViewDataBinding::implementation::Category>
            (L"Menu item 6", L"Icon", menuItem6Children);

        auto menuItem5 = winrt::make_self<HierarchicalNavigationViewDataBinding::implementation::Category>
            (L"Menu item 5", L"Icon", nullptr);
        auto menuItem4 = winrt::make_self<HierarchicalNavigationViewDataBinding::implementation::Category>
            (L"Menu item 4", L"Icon", nullptr);
        auto menuItem3Children =
            winrt::single_threaded_observable_vector<HierarchicalNavigationViewDataBinding::Category>();
        menuItem3Children.Append(*menuItem5);
        menuItem3Children.Append(*menuItem4);

        auto menuItem3 = winrt::make_self<HierarchicalNavigationViewDataBinding::implementation::Category>
            (L"Menu item 3", L"Icon", menuItem3Children);
        auto menuItem2Children = 
            winrt::single_threaded_observable_vector<HierarchicalNavigationViewDataBinding::Category>();
        menuItem2Children.Append(*menuItem3);

        auto menuItem2 = winrt::make_self<HierarchicalNavigationViewDataBinding::implementation::Category>
            (L"Menu item 2", L"Icon", menuItem2Children);
        auto menuItem1Children = 
            winrt::single_threaded_observable_vector<HierarchicalNavigationViewDataBinding::Category>();
        menuItem1Children.Append(*menuItem2);

        auto menuItem1 = winrt::make_self<HierarchicalNavigationViewDataBinding::implementation::Category>
            (L"Menu item 1", L"Icon", menuItem1Children);

        m_categories.Append(*menuItem1);
        m_categories.Append(*menuItem6);
        m_categories.Append(*menuItem10);
    }

    Windows::Foundation::Collections::IObservableVector<HierarchicalNavigationViewDataBinding::Category>
        MainPage::Categories()
    {
        return m_categories;
    }

    void MainPage::OnItemInvoked(
        muxc::NavigationView const& /* sender */,
        muxc::NavigationViewItemInvokedEventArgs const& args)
    {
        auto clickedItem = args.InvokedItem();
        auto clickedItemContainer = args.InvokedItemContainer();
    }

    void MainPage::OnItemExpanding(
        muxc::NavigationView const& /* sender */,
        muxc::NavigationViewItemExpandingEventArgs const& args)
    {
        auto nvib = args.ExpandingItemContainer();
        auto name = L"Last expanding: " + winrt::unbox_value<winrt::hstring>(nvib.Content());
        ExpandingItemLabel().Text(name);
    }

    void MainPage::OnItemCollapsed(
        muxc::NavigationView const& /* sender */,
        muxc::NavigationViewItemCollapsedEventArgs const& args)
    {
        auto nvib = args.CollapsedItemContainer();
        auto name = L"Last collapsed: " + winrt::unbox_value<winrt::hstring>(nvib.Content());
        CollapsedItemLabel().Text(name);
    }
}
```

### <a name="selection"></a>選択

既定では、すべての項目に対して子を含めることができるほか、呼び出し、選択を行えます。

ナビゲーション オプションの階層型ツリーをユーザーに提供する場合、親項目を選択不可にすることができます。たとえば、アプリにその親項目に関連付けられたリンク先ページが存在しない場合などです。 親項目が選択可能 "_である_" 場合は、左展開または Top のペイン表示モードの使用をお勧めします。 LeftCompact モードでは、その呼び出しのたびにユーザーが子サブツリーを開くために親項目に移動することになります。

項目を選択すると、選択インジケーターが Left モードの場合は左端に沿って、Top モードの場合は下端に合わせて表示されます。 親項目が選択されている Left モードおよび Top モードの NavigationViews を次に示します。

![親が選択された状態の Left モードの NavigationView](images/navigation-view-selection.png)

![親が選択された状態の Top モードの NavigationView](images/navigation-view-selection-top.png)

選択した項目は、常に表示されたままとは限りません。 折りたたまれた、または展開されていないサブツリー内の子が選択された場合、最初に表示される先祖が選択済みとして表示されます。 サブツリーが展開されている場合または展開された場合、選択インジケーターは選択された項目に戻ります。

たとえば、上の図では、ユーザーがカレンダー項目を選択した後、そのサブツリーを折りたたむことができます。 この場合、カレンダーで最初に表示される先祖はアカウントなので、選択インジケーターはアカウント項目の下に表示されます。 ユーザーがサブツリーを再度展開すると、選択インジケーターはカレンダー項目に戻ります。 

NavigationView では、選択インジケーターは全体で 1 つだけ表示されます。

Top と Left どちらのモードも、NavigationViewItems の矢印をクリックすると、サブツリーが展開されるか折りたたまれます。 NavigationViewItem の "_別の場所_" をクリックまたはタップすると、`ItemInvoked` イベントがトリガーされ、サブツリーも折りたたまれるか展開されます。

項目が呼び出されたときに選択インジケーターが表示されないようにするには、その項目の [SelectsOnInvoked](/uwp/api/microsoft.ui.xaml.controls.navigationviewitem.selectsoninvoked) プロパティを次のように False に設定します。

```xaml
<Page ... xmlns:muxc="using:Microsoft.UI.Xaml.Controls" ... >
    <Page.Resources>
        <DataTemplate x:Key="NavigationViewMenuItem" x:DataType="local:Category">
            <muxc:NavigationViewItem Content="{x:Bind Name}"
            MenuItemsSource="{x:Bind Children}"
            SelectsOnInvoked="{x:Bind IsLeaf}"/>
        </DataTemplate>
    </Page.Resources>

    <Grid>
        <muxc:NavigationView x:Name="navview" 
    MenuItemsSource="{x:Bind Categories, Mode=OneWay}" 
    MenuItemTemplate="{StaticResource NavigationViewMenuItem}">
        </muxc:NavigationView>
    </Grid>
</Page>
```

```csharp
public class Category
{
    public String Name { get; set; }
    public String CategoryIcon { get; set; }
    public ObservableCollection<Category> Children { get; set; }
    public bool IsLeaf { get; set; }
}

public sealed partial class HierarchicalNavigationViewDataBinding : Page
{
    public HierarchicalNavigationViewDataBinding()
    {
        this.InitializeComponent();
    }  

    public ObservableCollection<Category> Categories = new ObservableCollection<Category>()
    {
        new Category(){
            Name = "Menu item 1",
            CategoryIcon = "Icon",
            Children = new ObservableCollection<Category>() {
                new Category(){
                    Name = "Menu item 2",
                    CategoryIcon = "Icon",
                    Children = new ObservableCollection<Category>() {
                        new Category() {
                            Name  = "Menu item 3",
                            CategoryIcon = "Icon",
                            Children = new ObservableCollection<Category>() {
                                new Category() { Name  = "Menu item 4", CategoryIcon = "Icon", IsLeaf = true },
                                new Category() { Name  = "Menu item 5", CategoryIcon = "Icon", IsLeaf = true }
                            }
                        }
                    }
                }
            }
        },
        new Category(){
            Name = "Menu item 6",
            CategoryIcon = "Icon",
            Children = new ObservableCollection<Category>() {
                new Category(){
                    Name = "Menu item 7",
                    CategoryIcon = "Icon",
                    Children = new ObservableCollection<Category>() {
                        new Category() { Name  = "Menu item 8", CategoryIcon = "Icon", IsLeaf = true },
                        new Category() { Name  = "Menu item 9", CategoryIcon = "Icon", IsLeaf = true }
                    }
                }
            }
        },
        new Category(){ Name = "Menu item 10", CategoryIcon = "Icon", IsLeaf = true }
    };
}
```

```cppwinrt
// Category.idl
namespace HierarchicalNavigationViewDataBinding
{
    runtimeclass Category
    {
        ...
        Boolean IsLeaf;
    }
}

// Category.h
...
struct Category : CategoryT<Category>
{
    ...
    Category(winrt::hstring name,
        winrt::hstring categoryIcon,
        Windows::Foundation::Collections::IObservableVector<HierarchicalNavigationViewDataBinding::Category> children,
        bool isleaf = false);
    ...
    bool IsLeaf();
    void IsLeaf(bool value);

private:
    ...
    bool m_isleaf;
};

// Category.cpp
...
Category::Category(winrt::hstring name,
    winrt::hstring categoryIcon,
    Windows::Foundation::Collections::IObservableVector<HierarchicalNavigationViewDataBinding::Category> children,
    bool isleaf) : m_name(name), m_categoryIcon(categoryIcon), m_children(children), m_isleaf(isleaf) {}
...
bool Category::IsLeaf()
{
    return m_isleaf;
}

void Category::IsLeaf(bool value)
{
    m_isleaf = value;
}

// MainPage.h and MainPage.cpp
// Delete OnItemInvoked, OnItemExpanding, and OnItemCollapsed.

// MainPage.cpp
...
MainPage::MainPage()
{
    InitializeComponent();

    m_categories = winrt::single_threaded_observable_vector<HierarchicalNavigationViewDataBinding::Category>();

    auto menuItem10 = 
        winrt::make_self<HierarchicalNavigationViewDataBinding::implementation::Category>
        (L"Menu item 10", L"Icon", nullptr, true);

    auto menuItem9 = 
        winrt::make_self<HierarchicalNavigationViewDataBinding::implementation::Category>
        (L"Menu item 9", L"Icon", nullptr, true);
    auto menuItem8 = 
        winrt::make_self<HierarchicalNavigationViewDataBinding::implementation::Category>
        (L"Menu item 8", L"Icon", nullptr, true);
    auto menuItem7Children = 
        winrt::single_threaded_observable_vector<HierarchicalNavigationViewDataBinding::Category>();
    menuItem7Children.Append(*menuItem9);
    menuItem7Children.Append(*menuItem8);

    auto menuItem7 = 
        winrt::make_self<HierarchicalNavigationViewDataBinding::implementation::Category>
        (L"Menu item 7", L"Icon", menuItem7Children);
    auto menuItem6Children = 
        winrt::single_threaded_observable_vector<HierarchicalNavigationViewDataBinding::Category>();
    menuItem6Children.Append(*menuItem7);

    auto menuItem6 = 
        winrt::make_self<HierarchicalNavigationViewDataBinding::implementation::Category>
        (L"Menu item 6", L"Icon", menuItem6Children);

    auto menuItem5 = 
        winrt::make_self<HierarchicalNavigationViewDataBinding::implementation::Category>
        (L"Menu item 5", L"Icon", nullptr, true);
    auto menuItem4 = 
        winrt::make_self<HierarchicalNavigationViewDataBinding::implementation::Category>
        (L"Menu item 4", L"Icon", nullptr, true);
    auto menuItem3Children = 
        winrt::single_threaded_observable_vector<HierarchicalNavigationViewDataBinding::Category>();
    menuItem3Children.Append(*menuItem5);
    menuItem3Children.Append(*menuItem4);

    auto menuItem3 = 
        winrt::make_self<HierarchicalNavigationViewDataBinding::implementation::Category>
        (L"Menu item 3", L"Icon", menuItem3Children);
    auto menuItem2Children = 
        winrt::single_threaded_observable_vector<HierarchicalNavigationViewDataBinding::Category>();
    menuItem2Children.Append(*menuItem3);

    auto menuItem2 = 
        winrt::make_self<HierarchicalNavigationViewDataBinding::implementation::Category>
        (L"Menu item 2", L"Icon", menuItem2Children);
    auto menuItem1Children = 
        winrt::single_threaded_observable_vector<HierarchicalNavigationViewDataBinding::Category>();
    menuItem1Children.Append(*menuItem2);

    auto menuItem1 = 
        winrt::make_self<HierarchicalNavigationViewDataBinding::implementation::Category>
        (L"Menu item 1", L"Icon", menuItem1Children);

    m_categories.Append(*menuItem1);
    m_categories.Append(*menuItem6);
    m_categories.Append(*menuItem10);
}
...
```

### <a name="keyboarding-within-hierarchical-navigationview"></a>階層型 NavigationView 内でのキーボード操作

ユーザーは、[キーボード](../input/keyboard-interactions.md)を使用してナビゲーション ビューの周囲にフォーカスを移動できます。 方向キーはペイン内の "内部ナビゲーション" を表示し、[ツリー ビュー](./tree-view.md)でのアクションに従います。 キー アクションは、NavigationView 内を移動するとき、または HierarchicalNavigationView の Top および LeftCompact モードで表示されるポップアップ メニュー内を移動するときに変更されます。 階層型の NavigationView で各キーが実行できる特定のアクションを次に示します。

| キー      |      Left モード      |  Top モード | ポップアップ  |
|----------|------------------------|--------------|------------|
| ［上へ］ |現在フォーカスされている項目のすぐ上の項目にフォーカスを移動します。 | 何も実行しません。 |現在フォーカスされている項目のすぐ上の項目にフォーカスを移動します。|
| ［下へ］|現在フォーカスされている項目のすぐ下にフォーカスを移動します。* | 何も実行しません。 | 現在フォーカスされている項目のすぐ下にフォーカスを移動します。* |
| 権限 |何も実行しません。  |現在フォーカスされている項目の右隣の項目にフォーカスを移動します。 |何も実行しません。|
| 左 |何も実行しません。 | 現在フォーカスされている項目の左隣の項目にフォーカスを移動します。  |何も実行しません。 |
| Space または Enter |項目に子がある場合、フォーカスを変更せずに項目を展開するか折りたたみます。   | 項目に子がある場合、子をポップアップとして展開し、ポップアップの最初の項目にフォーカスを置きます。 | 項目を呼び出すか選択して、ポップアップを閉じます。 |
| Esc | 何も実行しません。 | 何も実行しません。 | ポップアップを閉じます。|

Space または Enter キーでは常に項目の呼び出しまたは選択を行います。

\* 項目が視覚的に隣接している必要はありません。ペインの一覧の最後の項目にあるフォーカスは設定項目に移動します。 

## <a name="navigation-view-customization"></a>ナビゲーション ビューのカスタマイズ

### <a name="pane-backgrounds"></a>ウィンドウの背景

既定では、NavigationView ウィンドウでは表示モードに応じて次のようにさまざまな背景が使用されます。

- ウィンドウは、コンテンツと横並びに左側に展開されると灰色の単色になります (Left モード)。
- コンテンツの上部にオーバーレイとして開かれたウィンドウでは、アプリ内アクリルが使用されます (Top モード、Minimal モード、または Compact モード)。

ウィンドウの背景を変更するには、各モードで背景をレンダリングするのに使用される XAML テーマ リソースをオーバーライドします (各種表示モードに対してさまざまな背景をサポートするには、単一の PaneBackground プロパティではなくこの手法が使用されます)。

次の表に、各表示モードで使用されるテーマ リソースを示します。

| 表示モード | テーマ リソース |
| ------------ | -------------- |
| 左 | NavigationViewExpandedPaneBackground |
| LeftCompact<br/>LeftMinimal | NavigationViewDefaultPaneBackground |
| 上 | NavigationViewTopPaneBackground |

この例では、App.xaml 内でテーマ リソースをオーバーライドする方法を示します。 テーマ リソースをオーバーライドする場合、最低でも "Default" および "HighContrast" のリソース辞書は常に指定し、"Light" または "Dark" リソース用の辞書は必要に応じて指定します。 詳細については、[ResourceDictionary.ThemeDictionaries](/uwp/api/windows.ui.xaml.resourcedictionary.themedictionaries) に関するページを参照してください。

> [!IMPORTANT]
> このコードでは、AcrylicBrush の [Windows UI ライブラリ](/uwp/toolkits/winui/) バージョンの使い方を示します。 プラットフォーム バージョンの AcrylicBrush を代わりに使用する場合、アプリ プロジェクトの最小バージョンは SDK 16299 以上である必要があります。 プラットフォーム バージョンを使用するには、`muxm:` へのすべての参照を削除します。

```xaml
<Application ... xmlns:muxm="using:Microsoft.UI.Xaml.Media" ...>
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

### <a name="top-whitespace"></a>先頭の空白

> `IsTitleBarAutoPaddingEnabled` プロパティには、[Windows UI ライブラリ](/uwp/toolkits/winui/) 2.2 以降が必要です。

一部のアプリでは、[そのウィンドウのタイトル バーをカスタマイズ](../shell/title-bar.md)するように選択できるため、そのアプリのコンテンツがタイトル バー領域に拡張される可能性があります。 NavigationView が、 **[ExtendViewIntoTitleBar](/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar.extendviewintotitlebar) API を使用** してタイトル バーに拡張されるアプリ内のルート要素である場合、[ドラッグ可能な領域](../shell/title-bar.md#draggable-regions)との重なりを避けるためその対話型要素の位置が自動的に調整されます。

![アプリのタイトル バーへの拡張](images/navigation-view-with-titlebar-padding.png)

ドラッグ可能な領域が [Window.SetTitleBar](/uwp/api/windows.ui.xaml.window.settitlebar) メソッドを呼び出すことによってアプリで指定されている場合、戻るボタンやメニュー ボタンをアプリ ウィンドウの上部近くに配置するには、[IsTitleBarAutoPaddingEnabled](/uwp/api/microsoft.ui.xaml.controls.navigationview.istitlebarautopaddingenabled) を **false** に設定します。

![余白なしのアプリのタイトル バーへの拡張](images/navigation-view-no-titlebar-padding.png)

```Xaml
<muxc:NavigationView x:Name="NavView" IsTitleBarAutoPaddingEnabled="False">
```

#### <a name="remarks"></a>コメント
NavigationView のヘッダー領域の位置をさらに調整するには、Page リソース内などの *NavigationViewHeaderMargin* XAML テーマ リソースをオーバーライドします。

```Xaml
<Page.Resources>
    <Thickness x:Key="NavigationViewHeaderMargin">12,0</Thickness>
</Page.Resources>
```

このテーマ リソースによって、[NavigationView.Header](/uwp/api/windows.ui.xaml.controls.navigationview.header) の周囲の余白が変更されます。

## <a name="related-topics"></a>関連トピック

- [NavigationView クラス](/uwp/api/windows.ui.xaml.controls.navigationview)
- [マスター/詳細](master-details.md)
- [ナビゲーションの基本](../basics/navigation-basics.md)
- [Fluent Design の概要](/windows/apps/fluent-design-system)
