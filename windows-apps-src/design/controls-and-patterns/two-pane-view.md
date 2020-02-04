---
Description: TwoPaneView は、2 つの個別のコンテンツ領域を持つアプリの表示を管理するために役立つレイアウト コントロールです。
title: 2 つのペインからなるビュー
template: detail.hbs
ms.date: 01/22/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 67b97aec970cc655700729743f10c63c666ab0a6
ms.sourcegitcommit: 09571e1c6a01fabed773330aa7ead459a47d94f7
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/01/2020
ms.locfileid: "76929262"
---
# <a name="two-pane-view"></a>2 つのペインからなるビュー

[TwoPaneView](/uwp/api/microsoft.ui.xaml.controls.twopaneview) は、マスター ビューと詳細ビューなど、2 つの異なる領域のコンテンツがあるアプリの表示を管理するために役立つレイアウト コントロールです。

> [!IMPORTANT]
> この記事では、パブリック プレビュー段階であり、一般公開前に大幅に変更される可能性がある機能とガイダンスについて説明します。 本書に記載された情報について、Microsoft は明示または黙示を問わずいかなる保証をするものでもありません。

TwoPaneView コントロールは、すべての Windows デバイス上で動作します。さらに、特別なコーディングを必要とせずに、デュアルスクリーン デバイスを自動的に最大限に活用できるように設計されています。 デュアルスクリーン デバイスでは、2 ペイン ビューを使うことで、ユーザー インターフェイス (UI) が画面間のすき間にまたがる場合でもきれいに分割され、コンテンツがすき間の両側に表示されます。

> **注:** "_デュアルスクリーン デバイス_" は、固有の機能を持つ特殊な種類のデバイスです。 これは、複数のモニターが搭載されたデスクトップ デバイスとは同じではありません。 デュアルスクリーン デバイスの詳細については、「[デュアルスクリーン デバイスの概要](/dual-screen/introduction)」を参照してください。
>
>この記事では、デュアルスクリーン デバイスではない任意のデバイスを示すために、単一モニターとマルチモニター セットアップの一部のいずれであっても、"_シングルスクリーン デバイス_" または "_シングルスクリーン ディスプレイ_" という用語を使用します。 TwoPaneView コントロールは、他の XAML コントロールと同じように 1 つの画面で動作します。 複数のモニターに合わせてアプリを最適化する方法の詳細については、[複数のビューの表示](/windows/uwp/design/layout/show-multiple-views)に関する記事を参照してください。

| **Windows UI ライブラリを入手する** |
| - |
| このコントロールは、Windows UI ライブラリの NuGet パッケージの一部として組み込まれており、パッケージには、UWP アプリの新しいコントロールと UI 機能が含まれています。 インストール手順などの詳細については、[Windows UI ライブラリの概要](/uwp/toolkits/winui/)に関するページを参照してください。 |

| **プラットフォーム API** | **Windows UI ライブラリ API** |
| - | - |
| [TwoPaneView クラス](/uwp/api/windows.ui.xaml.controls.twopaneview) | [TwoPaneView クラス](/uwp/api/microsoft.ui.xaml.controls.twopaneview) |

このドキュメントでは、XAML で **muxc** エイリアスを使って、プロジェクトに含めた Windows UI Library API を表します。 [Page](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page) 要素にこれを追加しました。

```xaml
xmlns:muxc="using:Microsoft.UI.Xaml.Controls"
```

コードビハインドでは、C# でも **muxc** エイリアスを使って、プロジェクトに含めた Windows UI Library API を表します。 この **using** ステートメントは、ファイルの先頭に追加されています。

```csharp
using muxc = Microsoft.UI.Xaml.Controls;
```

## <a name="is-this-the-right-control"></a>これは適切なコントロールですか?

コンテンツに 2 つの異なる領域があり、次に該当する場合は 2 ペイン ビューを使用します。

- ウィンドウに最適なサイズに合わせて、コンテンツの再配置とサイズ変更が自動的に行われます。
- コンテンツの 2 つ目の領域は、使用できる空間に応じて表示または非表示にする必要があります。
- コンテンツは、デュアルスクリーン デバイスの 2 つの画面の間できれいに分かれる必要があります。

## <a name="examples"></a>例

これらの画像は、シングルスクリーン デバイスとデュアルスクリーン デバイスで実行されるアプリを示します。 2 ペイン ビューを使うと、アプリの UI を各デバイスのさまざまなペイン構成に適応させることができます。

![tpv-single.png](images/two-pane-view/tpv-single.png)

"_シングルスクリーン デバイス上のアプリ_"。

![tpv-dual-wide.png](images/two-pane-view/tpv-dual-wide.png)

"_横長モードでデュアルスクリーン デバイス全体に表示されるアプリ_"。

![tpv-dual-tall.png](images/two-pane-view/tpv-dual-tall.png)

"_縦長モードでデュアルスクリーン デバイス全体に表示されるアプリ_"。

## <a name="how-it-works"></a>しくみ

2 ペイン ビューには、コンテンツが配置される 2 つのペインがあります。 ウィンドウに使用できる空間に応じて、ペインのサイズと配置が調整されます。 使用できるペインのレイアウトは、[TwoPaneViewMode](/uwp/api/microsoft.ui.xaml.controls.twopaneviewmode) 列挙体によって定義されます。

- **SinglePane** - [PanePriority](/uwp/api/microsoft.ui.xaml.controls.twopaneview.panepriority) プロパティで指定されているように、1 つのペインのみが表示されます。
- **Wide** - [WideModeConfiguration](/uwp/api/microsoft.ui.xaml.controls.twopaneview.widemodeconfiguration) プロパティの指定に従い、ペインが横並びで表示されるか、単一のペインが表示されます。
- **Tall** - [TallModeConfiguration](/uwp/api/microsoft.ui.xaml.controls.twopaneview.tallmodeconfiguration) プロパティの指定に従い、ペインが上下に表示されるか、単一のペインが表示されます。

2 ペイン ビューを構成するには、[PanePriority](/uwp/api/microsoft.ui.xaml.controls.twopaneview.panepriority) を設定して、1 つのペインのみに空間がある場合に表示するペインを指定します。 次に、Pane1 を縦長ウィンドウの上または下に表示するか、横長ウィンドウの左または右に表示するかを指定します。

2 ペイン ビューでは、ペインのサイズと配置が処理されますが、ペイン内のコンテンツをサイズと向きの変更に適応させる必要があります。 アダプティブ UI の作成の詳細については、「[XAML でのレスポンシブ レイアウト](/windows/uwp/design/layout/layouts-with-xaml)」と「[レイアウト パネル](/windows/uwp/design/layout/layout-panels)」を参照してください。

2 ペイン ビューを使うと、アプリが実行されているデバイスの種類に基づいてペインの表示を管理できます。

- デュアルスクリーン デバイスの場合

    2 ペイン ビューは、デュアルスクリーン デバイス用に UI を簡単に最適化できるように設計されています。 ウィンドウは、画面上の使用できるすべての空間を使用するようにサイズが自動調整されます。 アプリがデバイスの画面の 1 つのみに表示される場合、[PanePriority](/uwp/api/microsoft.ui.xaml.controls.twopaneview.panepriority) プロパティの指定に従い、1 つのペインが表示されます。

    アプリがデュアルスクリーン デバイスの両方の画面にまたがる場合、各画面にはいずれかのペインのコンテンツが表示され、すき間にまたがってコンテンツが適切に広がります。 2 ペイン ビューを使用すると、スパニング対応が組み込まれます。 縦長または横長の構成を設定するだけで、どの画面にどのペインを表示するかを指定できます。 2 ペイン ビューで残りの処理が行われます。


- シングルスクリーン デバイスの場合

    ラップトップやデスクトップ PC などのシングルスクリーン デバイス上で実行している場合、2 ペイン ビューでは、XAML コントロールから想定される動作が提供されます。 ウィンドウのサイズが変更されると、2 ペイン ビューによって、ウィンドウ サイズに基づいてペインのサイズと位置が調整されます。 1 つの画面上でサイズ変更可能なウィンドウに表示される場合に、設定してコントロールの動作を定義できる追加のプロパティがあります。 このようなプロパティについては、次のセクションで詳しく説明します。

## <a name="how-to-use-the-two-pane-view-control"></a>2 ペイン ビュー コントロールの使用方法

[TwoPaneView](/uwp/api/microsoft.ui.xaml.controls.twopaneview) は、ページ レイアウトのルート要素である必要はありません。 実際、アプリの全体的なナビゲーションを提供する [NavigationView](/uwp/api/microsoft.ui.xaml.controls.navigationview) コントロール内で使用することがよくあります。 TwoPaneView は、XAML ツリー内の場所に関係なく、適切に適応できます。ただし、TwoPaneView を別の TwoPaneView 内に入れ子にしないことをお勧めします。

### <a name="add-content-to-the-panes"></a>ペインにコンテンツを追加する

2 ペイン ビューの各ペインには、1 つの XAML UIElement を保持できます。 コンテンツを追加するには、通常、各ペインに XAML レイアウト パネルを配置し、他のコントロールとコンテンツをパネルに追加します。 ペインのサイズを変更し、横長モードと縦長モードを切り替えることができるため、各ペインのコンテンツがこのような変更に適応できることを確認する必要があります。 アダプティブ UI の作成の詳細については、「[XAML でのレスポンシブ レイアウト](/windows/uwp/design/layout/layouts-with-xaml)」と「[レイアウト パネル](/windows/uwp/design/layout/layout-panels)」を参照してください。

この例では、ここに示す単純な画像または情報アプリ UI を作成します。 2 つのペインに空間がある場合、画像と情報は別々のペインに表示されます (1 つのペインにのみ空間がある場合、Pane2 のコンテンツを Pane1 に移動し、ユーザー スクロールで非表示のコンテンツを表示できるようにします。 このコードについては、後述する「_モードの変更への対応_」を参照してください)。

![tpv-add-content.png](images/two-pane-view/tpv-add-content.png)

```xaml
<muxc:TwoPaneView
    Pane1Length="*"
    ModeChanged="TwoPaneView_ModeChanged">

    <muxc:TwoPaneView.Pane1>
        <Grid x:Name="Pane1Root">
            <Grid.RowDefinitions>
                <RowDefinition Height="Auto" MinHeight="40"/>
                <RowDefinition Height="*"/>
            </Grid.RowDefinitions>

            <CommandBar x:Name="MyCommandBar" DefaultLabelPosition="Right">
                <AppBarButton x:Name="Share" Icon="Share" Label="Share"/>
                <AppBarButton x:Name="Print" Icon="Print" Label="Print"/>
            </CommandBar>

            <ScrollViewer Grid.Row="1">
                <StackPanel x:Name="Pane1StackPanel">
                    <Image x:Name="TheImage" Source="Assets\LandscapeImage8.jpg"
                           VerticalAlignment="Top" HorizontalAlignment="Center" 
                           Margin="16,0"/>
                </StackPanel>
            </ScrollViewer>

        </Grid>
    </muxc:TwoPaneView.Pane1>

    <muxc:TwoPaneView.Pane2>
        <Grid x:Name="Pane2Root">
            <Grid.RowDefinitions>
                <RowDefinition Height="Auto" MinHeight="40"/>
                <RowDefinition Height="*"/>
            </Grid.RowDefinitions>
            <StackPanel x:Name="DetailsContent" Grid.Row="1"
                Orientation="Vertical" Padding="16">

                <TextBlock Text="Mountain.jpg" Margin="0,0,0,12"
                   FontWeight="SemiBold" FontSize="18"/>

                <TextBlock Text="Date Taken:" FontWeight="SemiBold"/>
                <TextBlock Text="8/29/2019 9:55am" Margin="0,0,0,12"/>

                <TextBlock Text="Dimensions:" FontWeight="SemiBold"/>
                <TextBlock Text="1000x750" Margin="0,0,0,12"/>

                <TextBlock Text="Resolution:" FontWeight="SemiBold"/>
                <TextBlock Text="96 dpi" Margin="0,0,0,12"/>

                <TextBlock Text="Description:" FontWeight="SemiBold"/>
                <TextBlock TextWrapping="Wrap" 
                           Text="Lorem ipsum dolor sit amet."/>
            </StackPanel>
        </Grid>
    </muxc:TwoPaneView.Pane2>
</muxc:TwoPaneView>
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

シングルスクリーン デバイスのペインのサイズは、[Pane1Length](/uwp/api/microsoft.ui.xaml.controls.twopaneview.pane1length) および [Pane2Length](/uwp/api/microsoft.ui.xaml.controls.twopaneview.pane2length) プロパティによって決まります。 これらには、_auto_ および _star_(\*) のサイズ設定をサポートする [GridLength](/uwp/api/windows.ui.xaml.gridlength) 値が使用されます。 自動サイズ設定と比例サイズ設定の詳細については、「[XAML でのレスポンシブ レイアウト](/windows/uwp/design/layout/layouts-with-xaml#layout-properties)」の「_レイアウト プロパティ_」を参照してください。

既定で、Pane1Length は **Auto** に設定されており、コンテンツに合わせてサイズが調整されます。 Pane2Length は * に設定されており、残っているすべての空間が使用されます。

![tpv-size-default.png](images/two-pane-view/tpv-size-default.png)

_既定のサイズ設定のペイン_

既定値は、Pane1 に項目の一覧があり、Pane2 に多数の詳細がある一般的なマスターと詳細のレイアウトに役立ちます。 ただし、コンテンツに応じて、異なる方法で空間を分割することをお勧めします。 ここでは、Pane1Length が 2* に設定されているため、Pane2 の 2 倍の空間が確保されます。

```xaml
<muxc:TwoPaneView x:Name="MyTwoPaneView" Pane1Length="2*">
```

![tpv-size-2.png](images/two-pane-view/tpv-size-2.png)

_ペインサイズ 2* と *_

> **注:** 前述のように、デュアルスクリーン デバイスではこれらのプロパティは無視され、デバイスの "_向き_" に基づいてペインのサイズと配置が自動的に変更されます。

自動サイズ設定を使用するようにペインを設定した場合、ペインのコンテンツを保持する Panel の高さと幅を設定することにより、サイズを制御できます。 この場合、ModeChanged イベントを処理し、現在のモードに合わせてコンテンツの高さと幅の制約を設定する必要があります。

### <a name="display-in-wide-or-tall-mode"></a>横長モードまたは縦長モードで表示する

デスクトップ ディスプレイでは、2 ペイン ビューのディスプレイの [Mode](/uwp/api/microsoft.ui.xaml.controls.twopaneview.mode) は [MinWideModeWidth](/uwp/api/microsoft.ui.xaml.controls.twopaneview.minwidemodewidth) プロパティと [MinTallModeHeight](/uwp/api/microsoft.ui.xaml.controls.twopaneview.mintallmodeheight) プロパティによって決まります。 どちらのプロパティも、既定値は [NavigationView.CompactThresholdWidth](/uwp/api/windows.ui.xaml.controls.navigationview.compactmodethresholdwidth) と同じ 641 px です。

ウィンドウが以下の場合:

- MinWideModeWidth よりも広い場合、**Wide** モードが使用されます。
- MinWideModeWidth よりも狭く、MinTallModeHeight よりも高い場合、**Tall** モードが使用されます。
- MinWideModeWidth よりも狭く、MinTallModeHeight よりも低い場合、**SinglePane** モードが使用されます。

> **注:** 前述のように、デュアルスクリーン デバイスではこれらのプロパティは無視され、デバイスの "_向き_" に基づいてペインのサイズと配置が自動的に変更されます。

#### <a name="wide-configuration-options"></a>横長構成のオプション

MinWideModeWidth プロパティよりも幅の広いシングル ディスプレイがある場合、2 ペイン ビューは横長モードに切り替わります。 MinWideModeWidth を使うと、2 ペイン ビューが横長モードに切り替わるタイミングを制御できます。 既定値は 641 px ですが、任意の値に変更できます。 一般的に、このプロパティは、ペインの最小の幅として使用する値に設定することをお勧めします。

2 ペイン ビューが横長モードの場合、WideModeConfiguration プロパティによって表示される内容が決まります。

- **SinglePane** - 単一ペイン (PanePriority によって決まります)。 ペインは、TwoPaneView のサイズ全体を占有します (つまり、両方向に比例サイズ設定です)。
- **LeftRight** - 左側に Pane1、右側に Pane2。 両方のペインは縦方向に比例サイズ設定、Pane1 の幅は自動サイズ設定、Pane2 の幅は比例サイズ設定です。
- **RightLeft** - 右側に Pane1、左側に Pane2。 両方のペインは縦方向に比例サイズ設定、Pane2 の幅は自動サイズ設定、Pane1 の幅は比例サイズ設定です。

既定の設定は **LeftRight** です。

| LeftRight | RightLeft |
| - | - |
| ![tpv-left-right.png](images/two-pane-view/tpv-left-right.png)  | ![tpv-right-left.png](images/two-pane-view/tpv-right-left.png)  |

> **ヒント:** デバイスで右から左 (RTL) の言語を使用している場合、2 ペイン ビューでは順序が自動的に入れ替えられます。RightLeft は LeftRight としてレンダリングされ、LeftRight は RightLeft としてレンダリングされます。

#### <a name="tall-configuration-options"></a>縦長構成のオプション

MinWideModeWidth よりも狭く、MinTallModeHeight よりも高いシングル ディスプレイがある場合、2 ペイン ビューは縦長モードに切り替わります。 既定値は 641 px ですが、任意の値に変更できます。 一般的に、このプロパティは、ペインの最小の高さとして使用する値に設定することをお勧めします。

2 ペイン ビューが横長モードの場合、TallLayout プロパティによって表示される内容が決まります。

- **SinglePane** - 単一ペイン (PanePriority によって決まります)。 ペインは、TwoPaneView のサイズ全体を占有します (つまり、両方向に比例サイズ設定です)。
- **TopBottom** - 上側に Pane1、下側に Pane2。 両方のペインは横方向に比例サイズ設定、Pane1 の高さは自動サイズ設定、Pane2 の高さは比例サイズ設定です。
- **TopBottom** - 下側に Pane1、上側に Pane2。 両方のペインは横方向に比例サイズ設定、Pane2 の高さは自動サイズ設定、Pane1 の高さは比例サイズ設定です。

既定値は **TopBottom** です。

| TopBottom | BottomTop |
| - | - |
| ![tpv-top-bottom.png](images/two-pane-view/tpv-top-bottom.png)  | ![tpv-bottom-top.png](images/two-pane-view/tpv-bottom-top.png)  |

#### <a name="special-values-for-minwidemodewidth-and-mintallmodeheight"></a>MinWideModeWidth と MinTallModeHeight の特別な値

MinWideModeWidth プロパティを使用して、2 ペイン ビューが Wide モードに切り替わらないようにすることができます。MinWideModeWidth を [Double.PositiveInfinity](/dotnet/api/system.double.positiveinfinity?view=dotnet-uwp-10.0) に設定するだけです。

MinTallModeHeight を [Double.PositiveInfinity](/dotnet/api/system.double.positiveinfinity?view=dotnet-uwp-10.0) に設定すると、2 ペイン ビューは Tall モードに切り替わらなくなります。

MinTallModeHeight を 0 に設定すると、2 ペイン ビューは SinglePane モードに切り替わらなくなります。

#### <a name="responding-to-mode-changes"></a>モードの変更への対応

読み取り専用の [Mode](/uwp/api/microsoft.ui.xaml.controls.twopaneview.mode) プロパティを使用して現在の表示モードを取得できます。 2 ペイン ビューに表示されるペインが変わるたびに、更新されたコンテンツがレンダリングされる前に [ModeChanged](/uwp/api/microsoft.ui.xaml.controls.twopaneview.modechanged) イベントが発生します。 このイベントを処理して、表示モードの変更に対応できます。

このイベントを使用する方法の 1 つは、ユーザーが SinglePane モードですべてのコンテンツを表示できるようにアプリの UI を更新することです。 たとえば、サンプル アプリにはプライマリ ペイン (画像) と情報ペインがあります。

![tpv-add-content.png](images/two-pane-view/tpv-add-content.png)

_Wide モード_

1 つのペインを表示するだけの空間しかない場合は、ユーザーがスクロールしてすべてのコンテンツを表示できるように、Pane2 のコンテンツを Pane1 に移動します。 次のようになります。

![tpv-mode-change.png](images/two-pane-view/tpv-mode-change.png)

_SinglePane モード_

```csharp
 private void TwoPaneView_ModeChanged(Microsoft.UI.Xaml.Controls.TwoPaneView sender, object args)
 {
     ((Panel)DetailsContent.Parent).Children.Remove(DetailsContent);
     ((Panel)MyCommandBar.Parent).Children.Remove(MyCommandBar);

     // Single pane
     if (sender.Mode == Microsoft.UI.Xaml.Controls.TwoPaneViewMode.SinglePane)
     {
         // Add the command bar and details content to Pane1.
         Pane1StackPanel.Children.Add(DetailsContent);
         Pane1Root.Children.Add(MyCommandBar);
     }
     // Dual pane.
     else
     {
         // Wide mode.
         if (sender.Mode == Microsoft.UI.Xaml.Controls.TwoPaneViewMode.Wide)
         {
             // Put the command bar in Pane2.
             Pane2Root.Children.Add(MyCommandBar);
         }
         // Tall mode.
         else if (sender.Mode == Microsoft.UI.Xaml.Controls.TwoPaneViewMode.Tall)
         {
             // Put the command bar in Pane1
             Pane1Root.Children.Add(MyCommandBar);
         }

         // Put details content in Pane2.
         Pane2Root.Children.Add(DetailsContent);
     }
 }
```

## <a name="dos-and-donts"></a>推奨と非推奨

- アプリでデュアルスクリーン ディスプレイと大画面を活用できるように、できる限り 2 ペイン ビューを使用してください。
- 別の 2 ペイン ビュー内に 2 ペイン ビューを配置しないでください。

## <a name="related-articles"></a>関連記事

- [レイアウトの概要](../layout/index.md)
- [デュアルスクリーンの開発](/dual-screen)
- [デュアルスクリーン デバイスの概要](/dual-screen/introduction)
