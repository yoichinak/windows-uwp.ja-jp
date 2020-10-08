---
description: RelativePanel、StackPanel、Grid、Canvas などの組み込みの XAML レイアウト パネルを使用して、アプリの UI 要素を配置およびグループ化する方法を説明します。
title: Windows アプリのレイアウト パネル
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 3fd8d57db92b8a59f32935586edfc247ec3c281f
ms.sourcegitcommit: 39fb8c0dff1b98ededca2f12e8ea7977c2eddbce
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/06/2020
ms.locfileid: "91750458"
---
# <a name="layout-panels"></a>レイアウト パネル

レイアウト パネルは、アプリの UI 要素を配置およびグループ化するためのコンテナーです。 組み込みの XAML レイアウト パネルには、[**RelativePanel**](/uwp/api/Windows.UI.Xaml.Controls.RelativePanel)、[**StackPanel**](/uwp/api/Windows.UI.Xaml.Controls.StackPanel)、[**Grid**](/uwp/api/Windows.UI.Xaml.Controls.Grid)、[**VariableSizedWrapGrid**](/uwp/api/Windows.UI.Xaml.Controls.VariableSizedWrapGrid)、[**Canvas**](/uwp/api/Windows.UI.Xaml.Controls.Canvas) があります。 ここでは、各パネルについて説明し、パネルを使って XAML UI 要素をレイアウトする方法を示します。

レイアウト パネルを選ぶときに考慮する事項がいくつかあります。
- パネルに子要素がどのように配置されるか。
- パネルで子要素のサイズがどのように変更されるか。
- 重複する子要素をお互いに重ねる方法 (z オーダー)。
- 目的のレイアウトの作成に必要な、入れ子になったパネル要素の数と複雑さ。

## <a name="examples"></a>例

<table>
<th align="left">XAML コントロール ギャラリー<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML コントロール ギャラリー</strong> アプリがインストールされている場合は、<a href="xamlcontrolsgallery:/item/RelativePanel">RelativePanel</a>、<a href="xamlcontrolsgallery:/item/StackPanel">StackPanel</a>、<a href="xamlcontrolsgallery:/item/Grid">Grid</a>、<a href="xamlcontrolsgallery:/item/VariableSizedWrapGrid">VariableSizedWrapGrid</a>、<a href="xamlcontrolsgallery:/item/Canvas">Canvas</a> の動作を参照してください。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML コントロール ギャラリー アプリを入手する (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">ソース コード (GitHub) を入手する</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="panel-properties"></a>パネル プロパティ

個々のパネルについて説明する前に、すべてのパネルにあるいくつかの一般的なプロパティを見ていきましょう。 

### <a name="panel-attached-properties"></a>パネルの添付プロパティ

多くの XAML レイアウト パネルでは添付プロパティを使用して、子要素がどのように UI に表示される必要があるかについて親パネルに通知できるようにしています。 添付プロパティでは、*AttachedPropertyProvider.PropertyName* という構文が使用されます。 パネルを他のパネルに入れ子にする場合、親に対するレイアウト属性を指定する UI 要素の添付プロパティは、最も近い親パネルによってのみ解釈されます。

XAML で Button コントロールの [**Canvas.Left**](/dotnet/api/system.windows.controls.canvas.left) 添付プロパティを設定する方法の例を以下に示します。 これにより、親 Canvas に、Button を Canvas の左端から 50 有効ピクセルの位置に配置する必要があることを通知します。

```xaml
<Canvas>
  <Button Canvas.Left="50">Hello</Button>
</Canvas>
```

添付プロパティについて詳しくは、「[添付プロパティの概要](../../xaml-platform/attached-properties-overview.md)」をご覧ください。

### <a name="panel-borders"></a>パネルの境界線

RelativePanel、StackPanel、Grid の各パネルには境界線プロパティが定義されており、別の Border 要素でラップすることなく、パネルの周囲に境界線を描画できます。 境界線プロパティには、**BorderBrush**、**BorderThickness**、**CornerRadius**、**Padding** があります。

Grid で境界線プロパティを設定する例を以下に示します。

```xaml
<Grid BorderBrush="Blue" BorderThickness="12" CornerRadius="12" Padding="12">
    <TextBlock Text="Hello World!"/>
</Grid>
```

![境界線を持つグリッド](images/layout-panel-grid-border.png)

組み込みの境界線のプロパティを使うことによって、XAML 要素の数を減らし、アプリの UI のパフォーマンスを向上させることができます。 レイアウト パネルと UI のパフォーマンスについて詳しくは、「[XAML レイアウトの最適化](../../debug-test-perf/optimize-your-xaml-layout.md)」をご覧ください。

## <a name="relativepanel"></a>RelativePanel

[**RelativePanel**](/uwp/api/Windows.UI.Xaml.Controls.RelativePanel) を使用すると、他の要素やパネルを基準として UI 要素を配置する場所を指定することにより、これをレイアウトすることができます。 既定では、要素はパネルの左上隅に配置されます。 RelativePanel を、[**VisualStateManager**](/uwp/api/Windows.UI.Xaml.VisualStateManager) や [**AdaptiveTrigger**](/uwp/api/Windows.UI.Xaml.AdaptiveTrigger) と共に使用して、さまざまなウィンドウ サイズに合わせて UI を配置し直すことができます。

次の表に、パネルまたは他の要素を基準として要素を揃えて配置するために使用できる添付プロパティを示します。

パネルの配置 | 兄弟の配置 | 兄弟の位置
----------------|-------------------|-----------------
[**AlignTopWithPanel**](/uwp/api/windows.ui.xaml.controls.relativepanel.aligntopwithpanelproperty) | [**AlignTopWith**](/uwp/api/windows.ui.xaml.controls.relativepanel.aligntopwithproperty) | [**Above**](/uwp/api/windows.ui.xaml.controls.relativepanel)  
[**AlignBottomWithPanel**](/uwp/api/windows.ui.xaml.controls.relativepanel.alignbottomwithpanelproperty) | [**AlignBottomWith**](/uwp/api/windows.ui.xaml.controls.relativepanel.alignbottomwithproperty) | [**Below**](/uwp/api/windows.ui.xaml.controls.relativepanel.belowproperty)  
[**AlignLeftWithPanel**](/uwp/api/windows.ui.xaml.controls.relativepanel) | [**AlignLeftWith**](/uwp/api/windows.ui.xaml.controls.relativepanel.getalignleftwith) | [**LeftOf**](/uwp/api/windows.ui.xaml.controls.relativepanel.leftofproperty)  
[**AlignRightWithPanel**](/uwp/api/windows.ui.xaml.controls.relativepanel.alignrightwithpanelproperty) | [**AlignRightWith**](/uwp/api/windows.ui.xaml.controls.relativepanel.alignrightwithproperty) | [**RightOf**](/uwp/api/windows.ui.xaml.controls.relativepanel.setrightof)  
[**AlignHorizontalCenterWithPanel**](/uwp/api/windows.ui.xaml.controls.relativepanel.alignhorizontalcenterwithpanelproperty) | [**AlignHorizontalCenterWith**](/uwp/api/windows.ui.xaml.controls.relativepanel.alignhorizontalcenterwithproperty) | &nbsp;   
[**AlignVerticalCenterWithPanel**](/uwp/api/windows.ui.xaml.controls.relativepanel.alignverticalcenterwithpanelproperty) | [**AlignVerticalCenterWith**](/uwp/api/windows.ui.xaml.controls.relativepanel.alignverticalcenterwithproperty) | &nbsp;   

 
次の XAML は、RelativePanel で要素を配置する方法を示しています。

```xaml
<RelativePanel BorderBrush="Gray" BorderThickness="1">
    <Rectangle x:Name="RedRect" Fill="Red" Height="44" Width="44"/>
    <Rectangle x:Name="BlueRect" Fill="Blue"
               Height="44" Width="88"
               RelativePanel.RightOf="RedRect" />

    <Rectangle x:Name="GreenRect" Fill="Green" 
               Height="44"
               RelativePanel.Below="RedRect" 
               RelativePanel.AlignLeftWith="RedRect" 
               RelativePanel.AlignRightWith="BlueRect"/>
    <Rectangle Fill="Orange"
               RelativePanel.Below="GreenRect" 
               RelativePanel.AlignLeftWith="BlueRect" 
               RelativePanel.AlignRightWithPanel="True"
               RelativePanel.AlignBottomWithPanel="True"/>
</RelativePanel>
```

結果は次のようになります。 

![RelativePanel](images/layout-panel-relative-panel.png)

長方形のサイズについて、注意が必要な点をいくつか示します。
- 赤色の長方形には、明示的なサイズとして 44 x 44 が指定されています。 この要素はパネルの左上隅に配置されます。これは既定の位置です。
- 緑色の四角形には、明示的な高さとして 44 が指定されています。 この長方形の左端は赤色の長方形に揃えられ、その右端は青色の長方形と揃えられています。これにより、この長方形の幅が決まります。
- オレンジ色の長方形には、明示的なサイズは指定されていません。 この長方形の左端は青色の長方形に揃えられます。 右端と下端はパネルの端に揃えられます。 この長方形のサイズはこれらの配置によって決まり、パネルのサイズが変更されると、長方形のサイズも変更されます。

## <a name="stackpanel"></a>StackPanel

[**StackPanel**](/uwp/api/Windows.UI.Xaml.Controls.StackPanel) を使用すると、その子要素が水平方向または垂直方向の単一行に配置されます。 StackPanel は通常、ページ上に UI の小さいサブセクションを配置するために使用されます。

子要素を並べる向きを指定するには、[**Orientation**](/uwp/api/windows.ui.xaml.controls.stackpanel.orientation) プロパティを使います。 既定の向きは [**Vertical**](/uwp/api/Windows.UI.Xaml.Controls.Orientation) です。

次の XAML は、項目の垂直方向の StackPanel を作成する方法を示しています。

```xaml
<StackPanel>
    <Rectangle Fill="Red" Height="44"/>
    <Rectangle Fill="Blue" Height="44"/>
    <Rectangle Fill="Green" Height="44"/>
    <Rectangle Fill="Orange" Height="44"/>
</StackPanel>
```


結果は次のようになります。

![スタック パネル](images/layout-panel-stack-panel.png)

StackPanel では、子要素のサイズを明示的に設定しない場合、利用可能な幅 (Orientation が **Horizontal** の場合は高さ) いっぱいに拡大されます。 この例では、長方形の幅は設定されていません。 長方形は、StackPanel の幅いっぱいに拡張されています。

## <a name="grid"></a>グリッド

[  **Grid**](/uwp/api/Windows.UI.Xaml.Controls.Grid) パネルは、柔軟なレイアウトをサポートし、複数行および段組レイアウトでのコントロールの配置を可能にします。 [  **RowDefinitions**](/uwp/api/windows.ui.xaml.controls.grid.rowdefinitions) プロパティと [**ColumnDefinitions**](/uwp/api/windows.ui.xaml.controls.grid.columndefinitions) プロパティを使うことによって、グリッドの行と列を指定します。

オブジェクトをグリッドの特定のセルに配置するには、[**Grid.Column**](/dotnet/api/system.windows.controls.grid.column) 添付プロパティと [**Grid.Row**](/dotnet/api/system.windows.controls.grid.row) 添付プロパティを使います。

複数の行や列をまたいでコンテンツを表示するには、[**Grid.RowSpan**](/previous-versions/windows/silverlight/dotnet-windows-silverlight/ms605035(v=vs.95)) 添付プロパティと [**Grid.ColumnSpan**](/dotnet/api/system.windows.controls.grid.columnspan) 添付プロパティを使います。

次の XAML の例では、2 つの行と 2 つの列を持つ Grid を作成する方法を示しています。

```xaml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition/>
        <RowDefinition Height="44"/>
    </Grid.RowDefinitions>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="Auto"/>
        <ColumnDefinition/>
    </Grid.ColumnDefinitions>
    <Rectangle Fill="Red" Width="44"/>
    <Rectangle Fill="Blue" Grid.Row="1"/>
    <Rectangle Fill="Green" Grid.Column="1"/>
    <Rectangle Fill="Orange" Grid.Row="1" Grid.Column="1"/>
</Grid>
```


結果は次のようになります。

![グリッド](images/layout-panel-grid.png)

この例では、サイズ設定は次のように行われます。 
- 2 番目の行には、明示的に 44 有効ピクセルの高さが指定されます。 既定では、最初の行の高さは、残っているスペースいっぱいになります。
- 最初の列の幅は **Auto** に設定されているため、その子に必要な幅になります。 この例では、赤色の長方形の幅に対応するために、その幅は 44 有効ピクセルになります。
- 長方形に対してその他のサイズの制約はないため、各長方形は配置されているグリッドのセルいっぱいに拡大されます。

**Auto** サイズ変更またはスター サイズ指定を使うと、列または行内でスペースを分散できます。 UI 要素がコンテンツや親コンテナーに合わせてサイズ変更できるようにするには、自動サイズ変更を使います。 グリッドの行と列を使って、自動サイズ変更を行うこともできます。 自動サイズ変更を使うには、UI 要素の Height や Width を **Auto** に設定します。

比例サイズ変更 (*スター サイズ指定*とも呼ばれる) を使うと、使用可能なスペースが加重比率によりグリッドの行と列の間で分散されます。 XAML では、スター値は \* (重み付きのスター サイズ指定の場合は *n*\*) として表されます。 たとえば、2 段組レイアウトで 1 つの列と、幅が 5 倍の列とを指定するには、[**ColumnDefinition**](/uwp/api/Windows.UI.Xaml.Controls.ColumnDefinition) 要素の [**Width**](/uwp/api/windows.ui.xaml.controls.columndefinition.width) プロパティで "5\*" と "\*" を使います。

次の例では、4 つの列を含む [**Grid**](/uwp/api/Windows.UI.Xaml.Controls.Grid) で、固定、自動、比例サイズ指定を組み合わせています。

| 列 | サイズ変更 | 説明 |
| ------ | ------ | ----------- |
Column_1 | **自動** | 列は、コンテンツが収まるようにサイズ変更されます。
Column_2 | * | [自動] 列の計算後、この列は残りの幅の一部を取得します。 Column_2 の幅は Column_4 の半分になります。
Column_3 | **44** | 列の幅は 44 ピクセルに設定されます。
Column_4 | **2**\* | [自動] 列の計算後、この列は残りの幅の一部を取得します。 Column_4 の幅は Column_2 の 2 倍になります。

既定の列の幅は "*" であるため、2 つ目の列については、この値を明示的に設定する必要はありません。

```xaml
<Grid>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="Auto"/>
        <ColumnDefinition/>
        <ColumnDefinition Width="44"/>
        <ColumnDefinition Width="2*"/>
    </Grid.ColumnDefinitions>
    <TextBlock Text="Column 1 sizes to its conent." FontSize="24"/>
</Grid>
```

Visual Studio の XAML デザイナーでは、結果は次のように表示されます。

![Visual Studio デザイナーでの 4 列のグリッド](images/xaml-layout-grid-in-designer.png)

## <a name="variablesizedwrapgrid"></a>VariableSizedWrapGrid

[**VariableSizedWrapGrid**](/uwp/api/Windows.UI.Xaml.Controls.VariableSizedWrapGrid) は、[**MaximumRowsOrColumns**](/uwp/api/windows.ui.xaml.controls.variablesizedwrapgrid.maximumrowsorcolumns) 値に達すると複数行、複数列が新しい行または列に自動的に折り返して配置される、グリッド スタイルのレイアウト パネルです。 

[  **Orientation**](/uwp/api/windows.ui.xaml.controls.variablesizedwrapgrid.orientation) プロパティは、折り返す前にグリッドの行と列のどちらの向きに項目を追加するかを指定します。 既定の向きは **Vertical** で、グリッドの項目は上から下へ列がいっぱいになるまで追加された後、新しい列に折り返されます。 この値が **Horizontal** の場合は、グリッドの項目は左から右に追加され、新しい行に折り返されます。

セルのサイズは、[**ItemHeight**](/uwp/api/windows.ui.xaml.controls.variablesizedwrapgrid.itemheight) と [**ItemWidth**](/uwp/api/windows.ui.xaml.controls.variablesizedwrapgrid.itemwidth) で指定します。 各セルは同じサイズです。 ItemHeight または ItemWidth が指定されていない場合、最初のセルのサイズがそのコンテンツに合わせて変更され、その他の各セルは最初のセルのサイズになります。

子要素が配置される隣接セルの数を指定するには、[**VariableSizedWrapGrid.ColumnSpan**](/uwp/api/windows.ui.xaml.controls.variablesizedwrapgrid) 添付プロパティと [**VariableSizedWrapGrid.RowSpan**](/uwp/api/windows.ui.xaml.controls.variablesizedwrapgrid.getrowspan) 添付プロパティを使います。

XAML での VariableSizedWrapGrid の使い方を以下に示します。

```xaml
<VariableSizedWrapGrid MaximumRowsOrColumns="3" ItemHeight="44" ItemWidth="44">
    <Rectangle Fill="Red"/>
    <Rectangle Fill="Blue" 
               VariableSizedWrapGrid.RowSpan="2"/>
    <Rectangle Fill="Green" 
               VariableSizedWrapGrid.ColumnSpan="2"/>
    <Rectangle Fill="Orange" 
               VariableSizedWrapGrid.RowSpan="2" 
               VariableSizedWrapGrid.ColumnSpan="2"/>
</VariableSizedWrapGrid>
```


結果は次のようになります。

![VariableSizedWrapGrid](images/layout-panel-variable-size-wrap-grid.png)

この例では、各列の行の最大数は 3 です。 青色の長方形は 2 行にまたがるため、最初の列には項目が 2 つだけ (赤色と青色の四角形) が含まれています。 緑色の四角形は、次の列の先頭に折り返されています。

## <a name="canvas"></a>キャンバス

[  **Canvas**](/uwp/api/Windows.UI.Xaml.Controls.Canvas) パネルでは、その子要素が固定座標点を使って配置されます。柔軟なレイアウトはサポートされません。 個々の子要素の位置を指定するには、要素ごとに [**Canvas.Left**](/dotnet/api/system.windows.controls.canvas.left) 添付プロパティと [**Canvas.Top**](/dotnet/api/system.windows.controls.canvas.top) 添付プロパティを設定します。 親 Canvas は、レイアウトの [Arrange](/uwp/api/windows.ui.xaml.uielement.arrange) パス時に子のこれらの添付プロパティ値を読み取ります。

Canvas 内のオブジェクトは重ね合わせることができます。この場合、1 つのオブジェクトが別のオブジェクトの上に描画されます。 既定では、Canvas は、宣言されている順序で子要素をレンダリングするため、最後の子が一番上に表示されます (各要素の既定の z-index は 0 です)。 これは他の組み込みパネルでも同じです。 ただし、Canvas では、子要素にそれぞれ設定できる [**Canvas.ZIndex**](/previous-versions/windows/silverlight/dotnet-windows-silverlight/cc190397(v=vs.95)) 添付プロパティもサポートされています。 コードでこのプロパティを設定することにより。実行時に要素の描画順序を変更することができます。 Canvas.ZIndex 値が最大である要素は最後に描画されるため、同じ領域を共有するか、重なり合っている他の要素の上に描画されます。 アルファ値 (透明度) が優先されるため、要素が重なる場合でも、一番上の要素のアルファ値が最大でないと、重なる領域に表示されるコンテンツがブレンドされることがあります。

Canvas では、子のサイズ変更は行われません。 各要素でそのサイズを指定する必要があります。

XAML での Canvas の例を以下に示します。

```xaml
<Canvas Width="120" Height="120">
    <Rectangle Fill="Red" Height="44" Width="44"/>
    <Rectangle Fill="Blue" Height="44" Width="44" Canvas.Left="20" Canvas.Top="20"/>
    <Rectangle Fill="Green" Height="44" Width="44" Canvas.Left="40" Canvas.Top="40"/>
    <Rectangle Fill="Orange" Height="44" Width="44" Canvas.Left="60" Canvas.Top="60"/>
</Canvas>
```

結果は次のようになります。

![キャンバス](images/layout-panel-canvas.png)

Canvas パネルは慎重に使用する必要があります。 UI 要素の位置を正確に制御できるのは便利ですが、シナリオによっては、固定配置されるレイアウト パネルにより UI の領域がアプリ全体のウィンドウ サイズの変更に適応しなくなることがあります。 アプリのウィンドウのサイズ変更は、デバイスの向きの変更、アプリのウィンドウの分割、モニターの変更を始めとする多くのユーザー シナリオによって発生する場合があります。

## <a name="panels-for-itemscontrol"></a>ItemsControl 用のパネル

[  **ItemsControl**](/uwp/api/Windows.UI.Xaml.Controls.ItemsControl) に項目を表示するための [**ItemsPanel**](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemspanel) としてのみ使用できる特殊な用途のパネルがいくつかあります。 このようなパネルには、[**ItemsStackPanel**](/uwp/api/Windows.UI.Xaml.Controls.ItemsStackPanel)、[**ItemsWrapGrid**](/uwp/api/Windows.UI.Xaml.Controls.ItemsWrapGrid)、[**VirtualizingStackPanel**](/uwp/api/Windows.UI.Xaml.Controls.VirtualizingStackPanel)、[**WrapGrid**](/uwp/api/Windows.UI.Xaml.Controls.WrapGrid) があります。 一般的な UI のレイアウトに、これらのパネルを使うことはできません。

## <a name="get-the-sample-code"></a>サンプル コードの入手

- [XAML コントロール ギャラリー サンプル](https://github.com/Microsoft/Xaml-Controls-Gallery) - インタラクティブな形で XAML コントロールのすべてを参照できます。
