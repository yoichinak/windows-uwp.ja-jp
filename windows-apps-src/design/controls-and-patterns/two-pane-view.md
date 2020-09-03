---
Description: TwoPaneView は、2 つの個別のコンテンツ領域を持つアプリの表示を管理するために役立つレイアウト コントロールです。
title: 2 つのペインからなるビュー
template: detail.hbs
ms.date: 01/22/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9c2fd792b9652e38637810b4ccd0aee94075895b
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174696"
---
# <a name="two-pane-view"></a>2 つのペインからなるビュー

[TwoPaneView](/uwp/api/microsoft.ui.xaml.controls.twopaneview) は、マスター ビューと詳細ビューなど、2 つの異なる領域のコンテンツがあるアプリの表示を管理するために役立つレイアウト コントロールです。

> [!IMPORTANT]
> この記事では、パブリック プレビュー段階であり、一般公開前に大幅に変更される可能性がある機能とガイダンスについて説明します。 本書に記載された情報について、Microsoft は明示または黙示を問わずいかなる保証をするものでもありません。

TwoPaneView コントロールは、すべての Windows デバイス上で動作します。さらに、特別なコーディングを必要とせずに、デュアルスクリーン デバイスを自動的に最大限に活用できるように設計されています。 デュアルスクリーン デバイスでは、2 ペイン ビューを使うことで、ユーザー インターフェイス (UI) が画面間のすき間にまたがる場合でもきれいに分割され、コンテンツがすき間の両側に表示されます。

> [!NOTE]
> "_デュアルスクリーン デバイス_" は、固有の機能を持つ特殊な種類のデバイスです。 これは、複数のモニターが搭載されたデスクトップ デバイスとは同じではありません。 デュアルスクリーン デバイスの詳細については、「[デュアルスクリーン デバイスの概要](/dual-screen/introduction)」を参照してください。 (複数のモニターに合わせてアプリを最適化する方法の詳細については、「[Show multiple views](../layout/show-multiple-views.md)」 (複数のビューの表示) を参照してください。)

**Windows UI ライブラリを入手する**

|  |  |
| - | - |
| ![WinUI ロゴ](images/winui-logo-64x64.png) | **TwoPaneView** コントロールは、Windows アプリのための新しいコントロールと UI 機能を含む NuGet パッケージである Windows UI ライブラリの一部として含まれています。 インストール手順などについて詳しくは、「[Windows UI Library (Windows UI ライブラリ)](/uwp/toolkits/winui/)」をご覧ください。 |

> **Windows UI ライブラリ API:** [TwoPaneView クラス](/uwp/api/microsoft.ui.xaml.controls.twopaneview)

> [!TIP]
> このドキュメントでは、XAML で **muxc** エイリアスを使って、プロジェクトに含めた Windows UI Library API を表します。 [Page](/uwp/api/windows.ui.xaml.controls.page) 要素にこれを追加しました。`xmlns:muxc="using:Microsoft.UI.Xaml.Controls"`
>
>コードビハインドでは、C# でも **muxc** エイリアスを使って、プロジェクトに含めた Windows UI Library API を表します。 この **using** ステートメントは、ファイルの先頭に追加されています。`using muxc = Microsoft.UI.Xaml.Controls;`

## <a name="is-this-the-right-control"></a>これは適切なコントロールですか?

コンテンツに 2 つの異なる領域があり、次に該当する場合は 2 ペイン ビューを使用します。

- ウィンドウに最適なサイズに合わせて、コンテンツの再配置とサイズ変更が自動的に行われます。
- コンテンツの 2 つ目の領域は、使用できる空間に応じて表示または非表示にする必要があります。
- コンテンツは、デュアルスクリーン デバイスの 2 つの画面の間できれいに分かれる必要があります。

## <a name="examples"></a>例

これらの画像は、実行されるアプリがシングルスクリーンに表示されている場合とデュアルスクリーンにまたがって表示されている場合を示しています。 2 ペイン ビューを使うと、アプリの UI をさまざまな画面構成に適応させることができます。

![2 ペイン ビューのアプリをシングル スクリーンで使用](images/two-pane-view/tpv-single.png)

> "_シングルスクリーン上のアプリ_。"

![2 ペイン ビューのアプリをデュアルスクリーンの横長モードで使用](images/two-pane-view/tpv-dual-wide.png)

> "_横長モードでデュアルスクリーン デバイス全体に表示されるアプリ_"。

![2 ペイン ビューのアプリをデュアルスクリーンの縦長モードで使用](images/two-pane-view/tpv-dual-tall.png)

> "_縦長モードでデュアルスクリーン デバイス全体に表示されるアプリ_"。

## <a name="how-it-works"></a>しくみ

2 ペイン ビューには、コンテンツが配置される 2 つのペインがあります。 ウィンドウに使用できる空間に応じて、ペインのサイズと配置が調整されます。 使用できるペインのレイアウトは、[TwoPaneViewMode](/uwp/api/microsoft.ui.xaml.controls.twopaneviewmode) 列挙体によって定義されます。

| 列挙値&nbsp; | 説明 |
| - | - |
| `SinglePane` | [PanePriority](/uwp/api/microsoft.ui.xaml.controls.twopaneview.panepriority) プロパティの指定に従い、1 つのペインのみが表示されます。 |
| `Wide` | [WideModeConfiguration](/uwp/api/microsoft.ui.xaml.controls.twopaneview.widemodeconfiguration) プロパティの指定に従い、ペインが横並びで表示されるか、単一のペインが表示されます。 |
| `Tall` | [TallModeConfiguration](/uwp/api/microsoft.ui.xaml.controls.twopaneview.tallmodeconfiguration) プロパティの指定に従い、ペインが上下に表示されるか、単一のペインが表示されます。 |

2 ペイン ビューを構成するには、[PanePriority](/uwp/api/microsoft.ui.xaml.controls.twopaneview.panepriority) を設定して、1 つのペインのみに空間がある場合に表示するペインを指定します。 次に、`Pane1` を縦長ウィンドウの上または下に表示するか、横長ウィンドウの左または右に表示するかを指定します。

2 ペイン ビューでは、ペインのサイズと配置が処理されますが、ペイン内のコンテンツをサイズと向きの変更に適応させる必要があります。 アダプティブ UI の作成の詳細については、「[XAML でのレスポンシブ レイアウト](../layout/layouts-with-xaml.md)」と「[レイアウト パネル](../layout/layout-panels.md)」を参照してください。

[TwoPaneView](/uwp/api/microsoft.ui.xaml.controls.twopaneview) を使用して、アプリのスパン状態に基づいてペインの表示を管理します。

- シングルスクリーン上

    アプリがシングル スクリーンにのみ表示されている場合は、`TwoPaneView` を使用して、指定したプロパティの設定に基づいて、ペインのサイズと位置を調整します。 このようなプロパティについては、次のセクションで詳しく説明します。 デバイス間の唯一の相違点は、デスクトップ PC などの一部のデバイスでは、サイズ変更可能なウィンドウを許可するが、他のデバイスでは許可しないことです。

- デュアルスクリーンにまたがる

    `TwoPaneView` は、デュアルスクリーン デバイス用に UI を簡単に最適化できるように設計されています。 画面上の使用できるすべての空間を使用するようにウィンドウ サイズが自動調整されます。 アプリがデュアルスクリーン デバイスの両方の画面にまたがる場合、各画面にはいずれかのペインのコンテンツが表示され、すき間にまたがってコンテンツが適切に広がります。 2 ペイン ビューを使用すると、スパニング対応が組み込まれます。 縦長または横長の構成を設定するだけで、どの画面にどのペインを表示するかを指定できます。 2 ペイン ビューで残りの処理が行われます。

## <a name="how-to-use-the-two-pane-view-control"></a>2 ペイン ビュー コントロールの使用方法

[TwoPaneView](/uwp/api/microsoft.ui.xaml.controls.twopaneview) は、ページ レイアウトのルート要素である必要はありません。 実際、アプリの全体的なナビゲーションを提供する [NavigationView](/uwp/api/microsoft.ui.xaml.controls.navigationview) コントロール内で使用することがよくあります。 `TwoPaneView` は、XAML ツリー内の場所に関係なく、適切に適応できます。ただし、`TwoPaneView` を別の `TwoPaneView` 内に入れ子にしないことをお勧めします。 (そのようにした場合は、外側の `TwoPaneView` のみがスパンに対応します。)

### <a name="add-content-to-the-panes"></a>ペインにコンテンツを追加する

2 ペイン ビューの各ペインには、1 つの XAML `UIElement` を保持できます。 コンテンツを追加するには、通常、各ペインに XAML レイアウト パネルを配置し、他のコントロールとコンテンツをパネルに追加します。 ペインのサイズを変更し、横長モードと縦長モードを切り替えることができるため、各ペインのコンテンツがこのような変更に適応できることを確認する必要があります。 アダプティブ UI の作成の詳細については、「[XAML でのレスポンシブ レイアウト](../layout/layouts-with-xaml.md)」と「[レイアウト パネル](../layout/layout-panels.md)」を参照してください。

この例では、前述の「_例_」セクションで示したシンプルな画像と情報アプリ UI を作成します。 アプリがデュアルスクリーンにまたがっている場合、画像と情報は別々の画面に表示されます。 シングル スクリーン上では、使用可能な空間の大きさに応じて、コンテンツを 2 つのペインに表示するか、1 つのペインに結合することができます。 (1 つのペインにのみ空間がある場合、Pane2 のコンテンツを Pane1 に移動し、ユーザー スクロールで非表示のコンテンツを表示できるようにします。 このコードについては、後述する「_モードの変更への対応_」を参照してください)。

![デュアル スクリーンにまたがるサンプル アプリの小さい画像](images/two-pane-view/tpv-left-right.png)

```xaml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto" MinHeight="40"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>

    <CommandBar DefaultLabelPosition="Right">
        <AppBarButton x:Name="Share" Icon="Share" Label="Share" Click="Share_Click"/>
        <AppBarButton x:Name="Print" Icon="Print" Label="Print" Click="Print_Click"/>
    </CommandBar>

    <muxc:TwoPaneView
        x:Name="MyTwoPaneView"
        Grid.Row="1"
        MinWideModeWidth="959"
        MinTallModeHeight="863"
        ModeChanged="TwoPaneView_ModeChanged">

        <muxc:TwoPaneView.Pane1>
            <Grid x:Name="Pane1Root">
                <ScrollViewer>
                    <StackPanel x:Name="Pane1StackPanel">
                        <Image Source="Assets\LandscapeImage8.jpg"
                               VerticalAlignment="Top" HorizontalAlignment="Center"
                               Margin="16,0"/>
                    </StackPanel>
                </ScrollViewer>
            </Grid>
        </muxc:TwoPaneView.Pane1>

        <muxc:TwoPaneView.Pane2>
            <Grid x:Name="Pane2Root">
                <ScrollViewer x:Name="DetailsContent">
                    <StackPanel Padding="16">
                        <TextBlock Text="Mountain.jpg" MaxLines="1"
                                       Style="{ThemeResource HeaderTextBlockStyle}"/>
                        <TextBlock Text="Date Taken:"
                                       Style="{ThemeResource SubheaderTextBlockStyle}"
                                       Margin="0,24,0,0"/>
                        <TextBlock Text="8/29/2019 9:55am"
                                       Style="{ThemeResource SubtitleTextBlockStyle}"/>
                        <TextBlock Text="Dimensions:"
                                       Style="{ThemeResource SubheaderTextBlockStyle}"
                                       Margin="0,24,0,0"/>
                        <TextBlock Text="800x536"
                                       Style="{ThemeResource SubtitleTextBlockStyle}"/>
                        <TextBlock Text="Resolution:"
                                       Style="{ThemeResource SubheaderTextBlockStyle}"
                                       Margin="0,24,0,0"/>
                        <TextBlock Text="96 dpi"
                                       Style="{ThemeResource SubtitleTextBlockStyle}"/>
                        <TextBlock Text="Description:"
                                       Style="{ThemeResource SubheaderTextBlockStyle}"
                                       Margin="0,24,0,0"/>
                        <TextBlock Text="Lorem ipsum dolor sit amet, consectetuer adipiscing elit. Maecenas porttitor congue massa. Fusce posuere, magna sed pulvinar ultricies, purus lectus malesuada libero, sit amet commodo magna eros quis urna."
                                       Style="{ThemeResource SubtitleTextBlockStyle}"
                                       TextWrapping="Wrap"/>
                    </StackPanel>
                </ScrollViewer>
            </Grid>
        </muxc:TwoPaneView.Pane2>
    </muxc:TwoPaneView>

    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="TwoPaneViewStates">
            <VisualState x:Name="Normal"/>
            <VisualState x:Name="Wide">
                <VisualState.Setters>
                    <Setter Target="MyTwoPaneView.Pane1Length"
                            Value="2*"/>
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Grid>
```

### <a name="specify-which-pane-to-display"></a>表示するペインを指定する

2 ペイン ビューで表示できるペインが 1 つだけの場合、[PanePriority](/uwp/api/microsoft.ui.xaml.controls.twopaneview.panepriority) プロパティを使用して、表示するペインを決定します。 既定では、PanePriority は **Pane1** に設定されています。 XAML またはコードでこのプロパティを設定する方法を次に示します。

```xaml
<muxc:TwoPaneView x:Name="MyTwoPaneView" PanePriority="Pane2">
```

```csharp
MyTwoPaneView.PanePriority = Microsoft.UI.Xaml.Controls.TwoPaneViewPriority.Pane2;
```

### <a name="pane-sizing"></a>ペインのサイズ設定

シングル スクリーン上では、ペインのサイズは、[Pane1Length](/uwp/api/microsoft.ui.xaml.controls.twopaneview.pane1length) および [Pane2Length](/uwp/api/microsoft.ui.xaml.controls.twopaneview.pane2length) プロパティによって決まります。 これらには、_auto_ および _star_(\*) のサイズ設定をサポートする [GridLength](/uwp/api/windows.ui.xaml.gridlength) 値が使用されます。 自動サイズ設定と比例サイズ設定の詳細については、「[XAML でのレスポンシブ レイアウト](../layout/layouts-with-xaml.md#layout-properties)」の「_レイアウト プロパティ_」を参照してください。

既定では、`Pane1Length` は `Auto` に設定され、コンテンツに合わせてサイズが自動調整されます。 `Pane2Length` は `*` に設定され、残りのすべての空間が使用されます。

![ペインが既定のサイズに設定されている 2 ペイン ビュー](images/two-pane-view/tpv-size-default.png)

> _既定のサイズ設定のペイン_

この既定値は、`Pane1` に項目の一覧があり、`Pane2` の詳細項目の数が多い、マスターと詳細からなる一般的なレイアウトに役立ちます。 ただし、コンテンツに応じて、異なる方法で空間を分割することをお勧めします。 ここでは、`Pane1Length` が `2*` に設定されているため、`Pane2`の 2 倍の空間が使用されます。

```xaml
<muxc:TwoPaneView x:Name="MyTwoPaneView" Pane1Length="2*">
```

![画面の 3 分の 2 を使用するペイン 1 と、3 分の 1 を使用するペイン 2 の 2 ペイン ビュー](images/two-pane-view/tpv-size-2.png)

> _ペインサイズ 2* と *_

> [!NOTE]
> 前述したように、アプリがデュアル スクリーンにまたがっている場合、これらのプロパティは無視され、各ペインはいずれかの画面に表示されます。

自動サイズ設定を使用するようにペインを設定した場合、ペインのコンテンツを保持する Panel の高さと幅を設定することにより、サイズを制御できます。 この場合、ModeChanged イベントを処理し、現在のモードに合わせてコンテンツの高さと幅の制約を設定する必要があります。

### <a name="display-in-wide-or-tall-mode"></a>横長モードまたは縦長モードで表示する

シングル スクリーン上では、2 ペイン ビューのディスプレイの [Mode](/uwp/api/microsoft.ui.xaml.controls.twopaneview.mode) は [MinWideModeWidth](/uwp/api/microsoft.ui.xaml.controls.twopaneview.minwidemodewidth) および [MinTallModeHeight](/uwp/api/microsoft.ui.xaml.controls.twopaneview.mintallmodeheight) プロパティによって決まります。 どちらのプロパティも、既定値は [NavigationView.CompactThresholdWidth](/uwp/api/windows.ui.xaml.controls.navigationview.compactmodethresholdwidth) と同じ 641 px です。

次の表は、TwoPaneView の高さと幅によって、使用される表示モードを決定する方法を示しています。

| TwoPaneView 条件  | モード |
|---------|---------|
| `Width` > `MinWideModeWidth` | `Wide` モードが使用されます |
| `Width` <= `MinWideModeWidth`、かつ `Height` > `MinTallModeHeight` | `Tall` モードが使用されます |
| `Width` <= `MinWideModeWidth`、かつ `Height` <= `MinTallModeHeight` | `SinglePane` モードが使用されます |


> [!NOTE]
> 前述したように、アプリがデュアル スクリーンにまたがっている場合、これらのプロパティは無視され、表示モードはデバイスの "_配置_" に基づいて決定されます。

#### <a name="wide-configuration-options"></a>横長構成のオプション

`MinWideModeWidth` プロパティよりも幅の広いシングル ディスプレイがある場合、2 ペイン ビューは `Wide` モードに切り替わります。 `MinWideModeWidth` を使うと、2 ペイン ビューが横長モードに切り替わるタイミングを制御できます。 既定値は 641 px ですが、任意の値に変更できます。 一般的に、このプロパティは、ペインの最小の幅として使用する値に設定することをお勧めします。

2 ペイン ビューが横長モードの場合、[WideModeConfiguration](/uwp/api/microsoft.ui.xaml.controls.twopaneview.widemodeconfiguration) プロパティによって、表示される内容が決まります。

| [列挙&nbsp;値](/uwp/api/microsoft.ui.xaml.controls.twopaneviewwidemodeconfiguration) | 説明 |
|---------|---------|
| `SinglePane` | シングル ペイン (`PanePriority` によって決まります)。 ペインは、`TwoPaneView` のサイズ全体を占有します (つまり、両方向に比例サイズ設定です)。 |
| `LeftRight` | 左側に `Pane1`、右側に `Pane2`。 両方のペインは縦方向に比例サイズ設定、`Pane1` の幅は自動サイズ設定、`Pane2` の幅は比例サイズ設定です。 |
| `RightLeft` | 右側に `Pane1`、左側に `Pane2`。 両方のペインは縦方向に比例サイズ設定、`Pane2` の幅は自動サイズ設定、`Pane1` の幅は比例サイズ設定です。 |

既定の設定は `LeftRight` です。

| LeftRight | RightLeft |
| - | - |
| ![2 ペイン ビューで構成されるビュー (左、右)](images/two-pane-view/tpv-left-right.png)  | ![2 ペイン ビューで構成されるビュー (右、左)](images/two-pane-view/tpv-right-left.png)  |

> **ヒント:** デバイスで右から左 (RTL) に記述する言語を使用している場合、2 ペイン ビューでは順序が自動的に入れ替わり、`RightLeft` は `LeftRight` として表示され、`LeftRight` は `RightLeft` として表示されます。

#### <a name="tall-configuration-options"></a>縦長構成のオプション

幅が `MinWideModeWidth` よりも狭く、高さが `MinTallModeHeight` よりも高いシングル ディスプレイがある場合、2 ペイン ビューは `Tall` モードに切り替わります。 既定値は 641 px ですが、任意の値に変更できます。 一般的に、このプロパティは、ペインの最小の高さとして使用する値に設定することをお勧めします。

2 ペイン ビューが縦長モードの場合、[TallModeConfiguration](/uwp/api/microsoft.ui.xaml.controls.twopaneview.tallmodeconfiguration) プロパティによって、表示される内容が決まります。

| [列挙&nbsp;値](/uwp/api/microsoft.ui.xaml.controls.twopaneviewtallmodeconfiguration) | 説明 |
|---------|---------|
| `SinglePane` | シングル ペイン (`PanePriority` によって決まります)。 ペインは、`TwoPaneView` のサイズ全体を占有します (つまり、両方向に比例サイズ設定です)。 |
| `TopBottom` | 上側に `Pane1`、下側に `Pane2`。 両方のペインは横方向に比例サイズ設定、`Pane1` の高さは自動サイズ設定、`Pane2` の高さは比例サイズ設定です。 |
| `BottomTop` | 下側に `Pane1`、上側に `Pane2`。 両方のペインは横方向に比例サイズ設定、`Pane2` の高さは自動サイズ設定、`Pane1` の高さは比例サイズ設定です。 |

既定値は `TopBottom` です。

| TopBottom | BottomTop |
| - | - |
| ![2 ペイン ビューで構成されるビュー (上、下)](images/two-pane-view/tpv-top-bottom.png)  | ![2 ペイン ビューで構成されるビュー (下、上)](images/two-pane-view/tpv-bottom-top.png)  |

#### <a name="special-values-for-minwidemodewidth-and-mintallmodeheight"></a>MinWideModeWidth と MinTallModeHeight の特別な値

`MinWideModeWidth` プロパティを使用して、2 ペイン ビューが横長モードに切り替わらないようにできます。`MinWideModeWidth` を [ouble.PositiveInfinity](/dotnet/api/system.double.positiveinfinity?view=dotnet-uwp-10.0) に設定するだけです。

`MinTallModeHeight` を [Double.PositiveInfinity](/dotnet/api/system.double.positiveinfinity?view=dotnet-uwp-10.0) に設定すると、2 ペイン ビューは縦長モードに切り替わらなくなります。

`MinTallModeHeight` を 0 に設定すると、2 ペイン ビューが `SinglePane` モードになるのを防ぎます。

#### <a name="responding-to-mode-changes"></a>モードの変更への対応

読み取り専用の [Mode](/uwp/api/microsoft.ui.xaml.controls.twopaneview.mode) プロパティを使用して現在の表示モードを取得できます。 2 ペイン ビューに表示されるペインが変わるたびに、更新されたコンテンツがレンダリングされる前に [ModeChanged](/uwp/api/microsoft.ui.xaml.controls.twopaneview.modechanged) イベントが発生します。 このイベントを処理して、表示モードの変更に対応できます。

> [!TIP]
> `ModeChanged` イベントは、ページが最初に読み込まれるときには発生しないので、既定の XAML は最初に読み込まれたときに表示される UI を表す必要があります。

このイベントを使用する方法の 1 つは、ユーザーが `SinglePane` モードですべてのコンテンツを表示できるようにアプリの UI を更新することです。 たとえば、サンプル アプリにはプライマリ ペイン (画像) と情報ペインがあります。

![縦長モードでまたがって表示されるサンプル アプリの小さい画像](images/two-pane-view/tpv-top-bottom.png)

> "_縦長モード_"

1 つのペインを表示するだけの空間しかない場合は、ユーザーがスクロールしてすべてのコンテンツを表示できるように、`Pane2` のコンテンツを `Pane1` に移動します。 次のようになります。

![1 つの画面に表示され、シングル ペインですべてのコンテンツのスクロールができるサンプル アプリの画像](images/two-pane-view/tpv-single-pane.png)

> _SinglePane モード_

`MinWideModeWidth` および `MinTallModeHeight` プロパティによって、表示モードを変更するタイミングが決定するため、これらのプロパティの値を調整すると、ペイン間でコンテンツを移動するタイミングを変更できます。

`Pane1` と `Pane2` の間でコンテンツを移動する `ModeChanged` イベント ハンドラー コードを次に示します。 また、[VisualState](/uwp/api/windows.ui.xaml.visualstate) を設定して、横長モードの画像の幅を制限します。

```csharp
private void TwoPaneView_ModeChanged(Microsoft.UI.Xaml.Controls.TwoPaneView sender, object args)
{
    // Remove details content from it's parent panel.
    ((Panel)DetailsContent.Parent).Children.Remove(DetailsContent);
    // Set Normal visual state.
    Windows.UI.Xaml.VisualStateManager.GoToState(this, "Normal", true);

    // Single pane
    if (sender.Mode == Microsoft.UI.Xaml.Controls.TwoPaneViewMode.SinglePane)
    {
        // Add the details content to Pane1.
        Pane1StackPanel.Children.Add(DetailsContent);
    }
    // Dual pane.
    else
    {
        // Put details content in Pane2.
        Pane2Root.Children.Add(DetailsContent);

        // If also in Wide mode, set Wide visual state
        // to constrain the width of the image to 2*.
        if (sender.Mode == Microsoft.UI.Xaml.Controls.TwoPaneViewMode.Wide)
        {
            Windows.UI.Xaml.VisualStateManager.GoToState(this, "Wide", true);
        }
    }
}
```

## <a name="dos-and-donts"></a>推奨と非推奨

- アプリでデュアルスクリーンと大画面を活用できるように、できる限り 2 ペイン ビューを使用してください。
- 別の 2 ペイン ビュー内に 2 ペイン ビューを配置しないでください。

## <a name="related-articles"></a>関連記事

- [レイアウトの概要](../layout/index.md)
- [デュアルスクリーンの開発](/dual-screen)
- [デュアルスクリーン デバイスの概要](/dual-screen/introduction)