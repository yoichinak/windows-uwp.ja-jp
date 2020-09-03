---
Description: カスタム Panel クラスのコードの記述、ArrangeOverride メソッドと MeasureOverride メソッドの実装、Children プロパティの使用について説明します。
MS-HAID: dev\_ctrl\_layout\_txt.boxpanel\_example\_custom\_panel
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: BoxPanel、カスタム パネルの例 (Windows アプリ)
ms.assetid: 981999DB-81B1-4B9C-A786-3025B62B74D6
label: BoxPanel, an example custom panel
template: detail.hbs
op-migration-status: ready
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 5b763f6d7e2b2451008f14ebfc2cc6b154d93bba
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89165526"
---
# <a name="boxpanel-an-example-custom-panel"></a>BoxPanel、カスタム パネルの例

 

カスタム [**Panel**](/uwp/api/Windows.UI.Xaml.Controls.Panel) クラスのコードの記述、[**ArrangeOverride**](/uwp/api/windows.ui.xaml.frameworkelement.arrangeoverride) メソッドと [**MeasureOverride**](/uwp/api/windows.ui.xaml.frameworkelement.measureoverride) メソッドの実装、[**Children**](/uwp/api/windows.ui.xaml.controls.panel.children) プロパティの使用について説明します。 

> **重要な API**:[**Panel**](/uwp/api/Windows.UI.Xaml.Controls.Panel)、[**ArrangeOverride**](/uwp/api/windows.ui.xaml.frameworkelement.arrangeoverride)、[**MeasureOverride**](/uwp/api/windows.ui.xaml.frameworkelement.measureoverride) 

コード例ではカスタム パネルの実装を示しますが、さまざまなレイアウト シナリオのパネルのカスタマイズ方法に影響を与えるレイアウトの概念については、詳しく説明していません。 このようなレイアウトの概念や、自分の特定のレイアウト シナリオへの適用方法に関する詳細情報が必要な場合は、「[XAML カスタム パネルの概要](custom-panels-overview.md)」をご覧ください。

*パネル*は、XAML レイアウト システムが実行されて、アプリの UI が表示されるときに、含まれている子要素のレイアウト動作を提供するオブジェクトです。 [  **Panel**](/uwp/api/Windows.UI.Xaml.Controls.Panel) クラスからカスタム クラスを派生させて、XAML レイアウトのカスタム パネルを定義できます。 パネルの動作は、[**ArrangeOverride**](/uwp/api/windows.ui.xaml.frameworkelement.arrangeoverride) メソッドと [**MeasureOverride**](/uwp/api/windows.ui.xaml.frameworkelement.measureoverride) メソッドをオーバーライドすることで子要素を評価して配置するロジックを提供して実行します。 この例は、**Panel** から派生しています。 **Panel** から開始した場合、**ArrangeOverride** メソッドと **MeasureOverride** メソッドには起動動作がありません。 コードが提供するゲートウェイによって、子要素が XAML レイアウト システムに認識され、UI に表示されます。 したがって、コードがすべての子要素について説明し、レイアウト システムが想定しているパターンに従うことが実際に重要です。

## <a name="your-layout-scenario"></a>レイアウト シナリオ

カスタム パネルを定義することは、レイアウト シナリオを定義することです。

レイアウト シナリオは、次によって表現されます。

-   子要素が作成されたときのパネルの動作
-   パネル自体のスペースが制約されるタイミング
-   最終的に子要素の UI レイアウトとして描画される測定値、配置、サイズのすべてを、パネルのロジックが決定する方法

この点を考慮して、特定のシナリオが使用する `BoxPanel` を次に示します。 この例でコードを最優先するために、ここではシナリオを詳しくは説明しません。その代わり、必要な手順とコーディング パターンについて重点的に説明します。 最初にシナリオについて詳しく知りたい場合は、この後にある「[`BoxPanel` のシナリオ](#the-scenario-for-boxpanel)」を参照した後、コードの説明に戻ってください。

## <a name="start-by-deriving-from-panel"></a>**Panel** からの派生で開始する

まず、[**Panel**](/uwp/api/Windows.UI.Xaml.Controls.Panel) からカスタム クラスを派生させます。 このために最も簡単と思われる方法は、このクラスのための別のコード ファイルを定義することです。これには、Microsoft Visual Studio の**ソリューション エクスプローラー**でプロジェクトに対してコンテキスト メニューの **[追加]**  |  **[新しい項目]**  |  **[クラス]** をクリックします。 このクラス (とファイル) に、`BoxPanel` という名前を付けます。

クラスのテンプレート ファイルは、特に Windows アプリ用ではないため、多くの **using** ステートメントで始まることはありません。 まず、**using** ステートメントを追加します。 また、テンプレート ファイルはいくつかの **using** ステートメントで始まっていますが、おそらく不要と思われるため、削除することができます。 次に示すのは、一般的なカスタム パネル コードに必要となる型を解決できる **using** ステートメントの候補の一覧です。

```CSharp
using System;
using System.Collections.Generic; // if you need to cast IEnumerable for iteration, or define your own collection properties
using Windows.Foundation; // Point, Size, and Rect
using Windows.UI.Xaml; // DependencyObject, UIElement, and FrameworkElement
using Windows.UI.Xaml.Controls; // Panel
using Windows.UI.Xaml.Media; // if you need Brushes or other utilities
```

これで [**Panel**](/uwp/api/Windows.UI.Xaml.Controls.Panel) を解決できるので、これを `BoxPanel` の基底クラスにします。 また、`BoxPanel` を公開します。

```CSharp
public class BoxPanel : Panel
{
}
```

クラス レベルでは、複数の論理関数で共有される **int** 値と **double** 値をいくつか定義しますが、これらは、パブリック API として公開する必要はありません。 例では、これらの名前は `maxrc`、`rowcount`、`colcount`、`cellwidth`、`cellheight`、`maxcellheight`、`aspectratio` です。

これを行った後、コード ファイル全体は次のようになります (ここにある理由はわかっているので、**using** のコメントは削除します)。

```CSharp
using System;
using System.Collections.Generic;
using Windows.Foundation;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Media;

public class BoxPanel : Panel 
{
    int maxrc, rowcount, colcount;
    double cellwidth, cellheight, maxcellheight, aspectratio;
}
```

これ以降は、メソッドのオーバーライド、依存関係プロパティなどのサポートするものなどのメンバー定義を 1 つずつ示します。 これらは、上に示したスケルトンに任意の順序で追加できます。最終的なコードを示すまで、スニペットでは、**using** ステートメントとクラス スコープの定義のいずれも再び示すことはありません。

## <a name="measureoverride"></a>**MeasureOverride**


```CSharp
protected override Size MeasureOverride(Size availableSize)
{
    Size returnSize;
    // Determine the square that can contain this number of items.
    maxrc = (int)Math.Ceiling(Math.Sqrt(Children.Count));
    // Get an aspect ratio from availableSize, decides whether to trim row or column.
    aspectratio = availableSize.Width / availableSize.Height;

    // Now trim this square down to a rect, many times an entire row or column can be omitted.
    if (aspectratio > 1)
    {
        rowcount = maxrc;
        colcount = (maxrc > 2 && Children.Count < maxrc * (maxrc - 1)) ? maxrc - 1 : maxrc;
    } 
    else 
    {
        rowcount = (maxrc > 2 && Children.Count < maxrc * (maxrc - 1)) ? maxrc - 1 : maxrc;
        colcount = maxrc;
    }

    // Now that we have a column count, divide available horizontal, that's our cell width.
    cellwidth = (int)Math.Floor(availableSize.Width / colcount);
    // Next get a cell height, same logic of dividing available vertical by rowcount.
    cellheight = Double.IsInfinity(availableSize.Height) ? Double.PositiveInfinity : availableSize.Height / rowcount;
           
    foreach (UIElement child in Children)
    {
        child.Measure(new Size(cellwidth, cellheight));
        maxcellheight = (child.DesiredSize.Height > maxcellheight) ? child.DesiredSize.Height : maxcellheight;
    }
    return LimitUnboundedSize(availableSize);
}
```

[  **MeasureOverride**](/uwp/api/windows.ui.xaml.frameworkelement.measureoverride) の実装に必要なパターンは、[**Panel.Children**](/uwp/api/windows.ui.xaml.controls.panel.children) の各要素のループ処理です。 これらの要素のそれぞれで、[**Measure**](/uwp/api/windows.ui.xaml.uielement.measure) メソッドを必ず呼び出します。 **Measure** には、型 [**Size**](/uwp/api/Windows.Foundation.Size) のパラメーターがあります。 ここで渡しているのは、この特定の子要素が表示できるようにパネルがコミットしているサイズです。 したがって、ループ処理を行い、**Measure** の呼び出しを開始する前に、各セルが使用可能なスペースの量を知る必要があります。 **MeasureOverride** メソッド自体には、*availableSize* 値があります。 これは、最初に呼び出されたこの **MeasureOverride** のトリガーであった **Measure** を呼び出したときにパネルの親が使用したサイズです。 そのため、一般的なロジックは、各子要素がパネルの *availableSize* 全体のスペースを分割するためのスキームを作成することです。 そして、サイズの各部分を各子要素の **Measure** に渡します。

`BoxPanel` でのサイズの分割方法は、非常に簡単です。多数のボックスにスペースを分割しますが、これは、主に項目の数で制御されます。 ボックスのサイズは、行と列の数、および使用可能なサイズに基づいて設定されます。 正方形の 1 行または 1 列は不要な場合があるため、破棄され、行と列の割合から見ると、パネルは正方形ではなく四角形になります。 このロジックに到達する過程の詳細については、この後の[「BoxPanel のシナリオ」](#the-scenario-for-boxpanel)をご覧ください。

それでは、測定パスでは何が行われるのでしょうか。 ここでは、[**Measure**](/uwp/api/windows.ui.xaml.uielement.measure) が呼び出された各要素に読み取り専用の [**DesiredSize**](/uwp/api/windows.ui.xaml.uielement.desiredsize) プロパティの値が設定されます。 **DesiredSize** 値があることは、配置パスに到達した後に重要になる可能性があります。なぜなら、**DesiredSize** によって、配置の際や最終的な描画で可能または必要なサイズが伝えられるためです。 自分のロジックで **DesiredSize** を使用しない場合でも、システムでは必要になります。

このパネルが、*availableSize* の高さコンポーネントが無限である場合に使われる可能性があります。 これに該当する場合、パネルには、分割するための既知の高さがありません。 この場合、測定パスのロジックは、有限の高さがまだないことを各子要素に知らせます。 知らせるには、[**Size.Height**](/uwp/api/windows.foundation.size.height) が無限である子の [**Measure**](/uwp/api/windows.ui.xaml.uielement.measure) 呼び出しに [**Size**](/uwp/api/Windows.Foundation.Size) を渡します。 これは適正な動作です。 **Measure** が呼び出されるときのロジックは、[**DesiredSize**](/uwp/api/windows.ui.xaml.uielement.desiredsize) が、**Measure** に渡されたものの最小値、または、明示的に設定された [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) と [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) などの要因からのその要素の自然なサイズの最小値として設定されていることです。

> [!NOTE]
> [**StackPanel**](/uwp/api/Windows.UI.Xaml.Controls.StackPanel) の内部ロジックにも、この動作があります。**StackPanel** は、子の [**Measure**](/uwp/api/windows.ui.xaml.uielement.measure) に無限サイズの値を渡します。これは、子には、向きのサイズの制約がないことを示します。 **StackPanel** は、通常、動的にサイズ設定され、そのサイズ内で拡大されるスタックにすべての子が配置されます。

ただし、パネル自体は、[**MeasureOverride**](/uwp/api/windows.ui.xaml.frameworkelement.measureoverride) から、無限値を持つ [**Size**](/uwp/api/Windows.Foundation.Size) を返すことができません。返すと、レイアウト時に例外がスローされます。 したがって、ロジックの一部は、子が要求する最大の高さを調べ、それが既にパネル自体のサイズ制約によるものでない場合は、その高さをセルの高さとして使うことです。 次に示すのは、前のコードで参照されるヘルパー関数 `LimitUnboundedSize` です。これは、このセルの最大の高さを受け取り、これを使って、返すことができる有限の高さをパネルに与えます。また、配置パスの開始前に `cellheight` が有限数であることを確認します。

```CSharp
// This method is called only if one of the availableSize dimensions of measure is infinite.
// That can happen to height if the panel is close to the root of main app window.
// In this case, base the height of a cell on the max height from desired size
// and base the height of the panel on that number times the #rows.

Size LimitUnboundedSize(Size input)
{
    if (Double.IsInfinity(input.Height))
    {
        input.Height = maxcellheight * colcount;
        cellheight = maxcellheight;
    }
    return input;
}
```

## <a name="arrangeoverride"></a>**ArrangeOverride**

```CSharp
protected override Size ArrangeOverride(Size finalSize)
{
     int count = 1
     double x, y;
     foreach (UIElement child in Children)
     {
          x = (count - 1) % colcount * cellwidth;
          y = ((int)(count - 1) / colcount) * cellheight;
          Point anchorPoint = new Point(x, y);
          child.Arrange(new Rect(anchorPoint, child.DesiredSize));
          count++;
     }
     return finalSize;
}
```

[  **ArrangeOverride**](/uwp/api/windows.ui.xaml.frameworkelement.arrangeoverride) の実装に必要なパターンは、[**Panel.Children**](/uwp/api/windows.ui.xaml.controls.panel.children) の各要素のループ処理です。 これらの要素のそれぞれで、[**Arrange**](/uwp/api/windows.ui.xaml.uielement.arrange) メソッドを必ず呼び出します。

[  **MeasureOverride**](/uwp/api/windows.ui.xaml.frameworkelement.measureoverride) の場合ほど、計算が多くないことに注意してください。これが一般的です。 子のサイズは、パネル自体の **MeasureOverride** ロジックから、または測定パスで設定された各子要素の [**DesiredSize**](/uwp/api/windows.ui.xaml.uielement.desiredsize) 値から既にわかっています。 ただし、各子要素が表示されるパネル内の場所を決定する必要がまだあります。 一般的なパネルでは、各子要素が別の場所に描画されます。 要素の重なりを作成するパネルは、一般的なシナリオとして好ましくありません (ただし、実際に意図したシナリオである場合は、意図的な重なりがあるパネルを作成することは問題外ではありません)。

このパネルは、行と列の概念で配置されます。 行と列の数は既に計算されています (測定値に必要であったため)。 したがって、行と列の図形、および各セルの既知のサイズが、このパネルに含まれる各要素の描画位置 (`anchorPoint`) の定義のロジックに使用されます。 [  **Point**](/uwp/api/Windows.Foundation.Point) は、測定により既にわかっている [**Size**](/uwp/api/Windows.Foundation.Size) と共に、[**Rect**](/uwp/api/Windows.Foundation.Rect) を作成する 2 つのコンポーネントとして使われます。 **Rect** は [**Arrange**](/uwp/api/windows.ui.xaml.uielement.arrange) の入力タイプです。

パネルでは、そのコンテンツのクリップが必要な場合があります。 クリップが必要な場合、クリップされたサイズは、[**DesiredSize**](/uwp/api/windows.ui.xaml.uielement.desiredsize) にあるサイズです。これは、[**Measure**](/uwp/api/windows.ui.xaml.uielement.measure) ロジックがこのサイズを、**Measure** に渡された最小値、またはその他の自然なサイズの要因として設定するためです。 したがって、[**Arrange**](/uwp/api/windows.ui.xaml.uielement.arrange) では、特にクリップを確認する必要はありません。クリップは、各 **Arrange** 呼び出しを介して **DesiredSize** を渡すことに基づいて発生するだけです。

描画位置を定義するために必要なすべての情報が他の方法でわかっている場合は、ループ処理中に常に数を数える必要はありません。 たとえば、[**Canvas**](/uwp/api/Windows.UI.Xaml.Controls.Canvas) レイアウト ロジックで、[**Children**](/uwp/api/windows.ui.xaml.controls.panel.children) コレクションでの位置は重要ではありません。 **Canvas** の各要素の位置を決定するために必要なすべての情報は、配置ロジックの一部として子の [**Canvas.Left**](/dotnet/api/system.windows.controls.canvas.left) 値と [**Canvas.Top**](/dotnet/api/system.windows.controls.canvas.top) 値を読み取ることで得られるためです。 ただし、`BoxPanel` のロジックでは、新しい行の開始と、*y* 値のオフセットのタイミングを知るために、数を数えて *colcount* と比較する必要があります。

入力 *finalSize* と、[**ArrangeOverride**](/uwp/api/windows.ui.xaml.frameworkelement.arrangeoverride) の実装から返す [**Size**](/uwp/api/Windows.Foundation.Size) が同じであることは一般的です。 その理由について詳しくは、「[XAML カスタム パネルの概要](custom-panels-overview.md)」の「**ArrangeOverride**」セクションをご覧ください。

## <a name="a-refinement-controlling-the-row-vs-column-count"></a>改良: 行と列の数の制御

このパネルは、コンパイルして、そのまま使用できます。 ただし、もう 1 つ改良を加えます。 ここで示したコードで、ロジックは、縦横比で最も長い側に、追加の行または列を設定しています。 ただし、セルの形状をさらに制御するには、パネル自体の縦横比が "縦長" であっても、3×4 ではなく、4×3 のセル セットを選択する方が適切である場合があります。 そのため、その動作を制御するためにパネルのユーザーが設定できる、オプションの依存関係プロパティを追加します。 この依存関係プロパティ定義は、次に示すように、非常に基本的です。

```CSharp
public static readonly DependencyProperty UseOppositeRCRatioProperty =
   DependencyProperty.Register("UseOppositeRCRatio", typeof(bool), typeof(BoxPanel), null);

public bool UseSquareCells
{
    get { return (bool)GetValue(UseOppositeRCRatioProperty); }
    set { SetValue(UseOppositeRCRatioProperty, value); }
}
```

次に、`UseOppositeRCRatio` の使用が測定ロジックに与える影響を説明します。 実際に行われていることは、`rowcount` と `colcount` が、`maxrc` と実際の縦横比から派生しているしくみの変更だけです。このために、対応するサイズの違いが各セルにあります。 `UseOppositeRCRatio` が **true** である場合は、行と列を数えるために使う前に実際の縦横比の値が逆になります。

```CSharp
if (UseOppositeRCRatio) { aspectratio = 1 / aspectratio;}
```

## <a name="the-scenario-for-boxpanel"></a>BoxPanel のシナリオ

`BoxPanel` の特定のシナリオは、子項目の数がわかっており、パネルで使用できるとわかっているスペースを分割することが、スペースの分割方法の主な決定要因の 1 つであるパネルです。 パネルの形状は本質的に四角形です。 多くのパネルは、その四角形のスペースをさらに四角形に分割して動作します。これは、セルに対する [**Grid**](/uwp/api/Windows.UI.Xaml.Controls.Grid) の動作です。 **Grid** の場合は、セルのサイズが [**ColumnDefinition**](/uwp/api/Windows.UI.Xaml.Controls.ColumnDefinition) と [**RowDefinition**](/uwp/api/Windows.UI.Xaml.Controls.RowDefinition) の値によって設定され、これらの値が使用される正確なセルが要素によって、[**Grid.Row**](/dotnet/api/system.windows.controls.grid.row) 添付プロパティと [**Grid.Column**](/dotnet/api/system.windows.controls.grid.column) 添付プロパティで宣言されます。 **Grid** から適切なレイアウトを取得するには、通常、子要素の数を事前に知っている必要があります。これは、セルの数が十分であり、各子要素がそのセル サイズに収まるように自身の添付プロパティを設定する必要があるためです。

では、子の数が動的な場合はどうでしょうか。 これは、確実にあり得ます。アプリ コードは、UI を更新する価値があるだけ重要であると考えられる動的ランタイム状態に対応して、コレクションに項目を追加できます。 コレクション/ビジネス オブジェクトのバッキングにデータ バインドを使っている場合は、このような更新プログラムの取得と UI の更新が自動的に処理されます。これは、多くの場合、優先して使われる手法です (「[データ バインディングの詳細](../../data-binding/data-binding-in-depth.md)」をご覧ください)。

ただし、アプリのすべてのシナリオがデータ バインディングに対応しているわけではありません。 場合によっては、新しい UI 要素を実行時に作成し、表示されるようにする必要があります。 `BoxPanel` は、このようなシナリオで役立ちます。 子項目の数が変わることは、`BoxPanel` では問題になりません。子の数を使って計算し、既存と新規の両方の子要素がすべて、新しいレイアウトに収まるように調整するためです。

`BoxPanel` をさらに拡張する高度なシナリオ (ここでは示されていません) では、動的な子に対応すると同時に、より強力な要因として子の [**DesiredSize**](/uwp/api/windows.ui.xaml.uielement.desiredsize) を使って個々のセルのサイズを設定することができます。 そして、可変サイズの行または列、または非グリッド形状を使って、「無駄な」スペースを減らすことができます。 これには、黄金比と最小サイズの両方の場合に、さまざまなサイズと縦横比の複数の四角形をすべて、それらを含む四角形に収めるための方法が必要です。 `BoxPanel` では、このような方法ではなく、スペースを分割するための単純な手法を使用しています。 `BoxPanel` の手法は、子の数より多い、最小の正方形を決定することです。 たとえば、9 個の項目は、3×3 の正方形に収まります。 10 個の項目には、4×4 の正方形が必要です。 ただし、多くの場合、項目を収めると同時に、最初の正方形の 1 行または 1 列を削除して、スペースを節約できます。 10 個の項目の例では、4×3 または 3×4 の長方形に収まります。

10 項目の場合にパネルが、ちょうど収まる 5×2 を選択しないのは不思議に思われます。 ただし、実際には、向きがはっきりした縦横比の四角形としてパネルがサイズ設定されることは稀です。 最小正方形の手法は、サイズ設定ロジックを偏らせて、一般的なレイアウトの図形を適切に処理し、セルの形状が極端な縦横比になるサイズ設定を防ぐための 1 つの方法です。

## <a name="related-topics"></a>関連トピック

**リファレンス**

* [**FrameworkElement.ArrangeOverride**](/uwp/api/windows.ui.xaml.frameworkelement.arrangeoverride)
* [**FrameworkElement.MeasureOverride**](/uwp/api/windows.ui.xaml.frameworkelement.measureoverride)
* [**Panel**](/uwp/api/Windows.UI.Xaml.Controls.Panel)

**概念**

* [配置、余白、およびパディング](alignment-margin-padding.md)