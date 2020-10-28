---
description: ここでは、パスの形状を XAML 属性値として指定するために使うことのできる、移動と描画のコマンド (ミニ言語) について説明します。
title: 移動と描画のコマンド構文
ms.assetid: 7772BC3E-A631-46FF-9940-3DD5B9D0E0D9
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a5942dfbcd2f72d456ac352785bce73e75454330
ms.sourcegitcommit: aa88679989ef3c8b726e1bf5a0ed17c1206a414f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/27/2020
ms.locfileid: "92687781"
---
# <a name="move-and-draw-commands-syntax"></a>移動と描画のコマンド構文


ここでは、パスの形状を XAML 属性値として指定するために使うことのできる、移動と描画のコマンド (ミニ言語) について説明します。 移動と描画のコマンドは、ベクター グラフィックや図形の出力に対応するさまざまなデザイン ツールやグラフィックス ツールで、シリアル化形式や交換形式として使われます。

## <a name="properties-that-use-move-and-draw-command-strings"></a>移動と描画のコマンド文字列を使うプロパティ

移動と描画のコマンド構文は、XAML の内部型コンバーターによってサポートされます。コンバーターはコマンドを解析し、実行時にグラフィックス表現を生成します。 この表現は、基本的には完成したベクター セットであり、そのまま表示することができます。 ただし、ベクター自体では表現の詳細までは定義されないため、他の値を要素に設定する必要もあります。 [**Path**](/uwp/api/Windows.UI.Xaml.Shapes.Path) オブジェクトについては、 [**Fill**](/uwp/api/Windows.UI.Xaml.Shapes.Shape.Fill) や [**Stroke**](/uwp/api/windows.ui.xaml.shapes.shape.stroke) などのプロパティに値を設定してから、その **Path** を何らかの方法でビジュアル ツリーに関連付ける必要もあります。 [**PathIcon**](/uwp/api/Windows.UI.Xaml.Controls.PathIcon) オブジェクトでは、 [**Foreground**](/uwp/api/windows.ui.xaml.controls.iconelement.foreground) プロパティを設定します。

Windows ランタイムには、移動と描画のコマンドを表す文字列を使うことのできるプロパティとして、 [**Path.Data**](/uwp/api/windows.ui.xaml.shapes.path.data) と [**PathIcon.Data**](/uwp/api/windows.ui.xaml.controls.pathicon.data) の 2 つがあります。 通常、これらのプロパティに移動と描画のコマンドを指定するときは、その要素に必要な他の属性と共に、コマンドを XAML 属性値として設定します。 単純な例としては次のようになります。

```xml
<Path x:Name="Arrow" Fill="White" Height="11" Width="9.67"
  Data="M4.12,0 L9.67,5.47 L4.12,10.94 L0,10.88 L5.56,5.47 L0,0.06" />
```

[**PathGeometry.Figures**](/uwp/api/windows.ui.xaml.media.pathgeometry.figures) でも移動と描画のコマンドを使うことができます。 移動と描画のコマンドを使う [**PathGeometry**](/uwp/api/Windows.UI.Xaml.Media.PathGeometry) オブジェクトは、 [**GeometryGroup**](/uwp/api/Windows.UI.Xaml.Media.GeometryGroup) オブジェクトに含まれる他の [**Geometry**](/uwp/api/Windows.UI.Xaml.Media.Geometry) 型と結合して、 [**Path.Data**](/uwp/api/windows.ui.xaml.shapes.path.data) の値として使うこともできます。 ただし、この使い方は、属性定義のデータで移動と描画のコマンドを使う方法ほど一般的ではありません。

## <a name="using-move-and-draw-commands-versus-using-a-pathgeometry"></a>移動と描画のコマンドの使用と **PathGeometry** の使用

Windows ランタイム XAML では、移動と描画のコマンドにより、単一の [**PathFigure**](/uwp/api/Windows.UI.Xaml.Media.PathFigure) オブジェクトと [**Figures**](/uwp/api/windows.ui.xaml.media.pathgeometry.figures) プロパティの値を持つ [**PathGeometry**](/uwp/api/Windows.UI.Xaml.Media.PathGeometry) が生成されます。 各描画コマンドは、その単一の **PathFigure** の [**Segments**](/uwp/api/windows.ui.xaml.media.pathfigure.segments) コレクションに [**PathSegment**](/uwp/api/Windows.UI.Xaml.Media.PathSegment) 派生クラスを生成します。移動コマンドは [**StartPoint**](/uwp/api/windows.ui.xaml.media.pathfigure.startpoint) を変更します。終了コマンドがある場合は、 [**IsClosed**](/uwp/api/windows.ui.xaml.media.pathfigure.isclosed) が **true** に設定されます。 実行時に **Data** の値を調べると、この構造をオブジェクト モデルとしてたどることができます。

## <a name="the-basic-syntax"></a>基本構文

移動と描画のコマンド構文を簡単にまとめると、次のようになります。

1.  まず、オプションの塗りつぶしルールを指定します。 通常、これを指定するのは、既定の **EvenOdd** では望ましくない場合だけです  ( **EvenOdd** については後ほど詳しく説明します)。
2.  移動コマンドを 1 つだけ指定します。
3.  1 つ以上の描画コマンドを指定します。
4.  終了コマンドを指定します。 終了コマンドは省略することもできますが、その場合は図が開いたままになります (これは一般的ではありません)。

この構文の一般的な規則は次のとおりです。

-   各コマンドは 1 文字で表されます。
-   コマンドの文字は大文字または小文字で指定できます。 後で説明するように、大文字と小文字は区別されます。
-   通常、終了コマンド以外の各コマンドには 1 つ以上の数値が続きます。
-   1 つのコマンドに複数の数値を指定する場合は、コンマまたはスペースで区切ります。

**\[**_Fillrule_ **\]**_movecommand_ _drawcommand_ **\[** _drawcommand_ **\*\]** **\[** _closeCommand_**\]**

描画コマンドの多くでは点が使われますが、これは _x,y_ 値として指定します。 ポイントのプレースホルダーが表示さ \* _points_ れている場合は、ポイントの _x、y_ 値に2つの10進値を指定することを前提としています。

空白がなくても結果があいまいにならない場合は、空白を省略できます。 すべての数値 (ポイントとサイズ) の区切り文字をコンマにすると、空白をすべて省略することができます。 たとえば、`F1M0,58L2,56L6,60L13,51L15,53L6,64z` という使い方は正当です。 ただし、読みやすくするために、コマンドの間には空白を含めるのが一般的です。

コンマを 10 進数の小数点として使わないでください。コマンド文字列は XAML によって解釈され、 **en-us** ロケール以外で使われるカルチャ固有の数値形式の規則は考慮されません。

## <a name="syntax-specifics"></a>構文仕様

**塗りつぶしルール**

オプションの塗りつぶしルールとして指定できる値には、 **F0** と **F1** の 2 つがあります  ( **F** は常に大文字です)。 **F0** が既定のルールです。これは **EvenOdd** の塗りつぶし動作になるので、通常は指定しません。 **Nonzero** の塗りつぶし動作を有効にするには、 **F1** を使います。 これらの塗りつぶしの値は、 [**FillRule**](/uwp/api/Windows.UI.Xaml.Media.FillRule) 列挙体の値と対応しています。

**Move コマンド**

新しい図形の始点を指定します。

| 構文 |
|--------|
| `M ` _startPoint_ <br/>または<br/>`m` _startPoint_|

| 用語 | 説明 |
|------|-------------|
| _startPoint_ | [**ポイント**](/uwp/api/Windows.Foundation.Point) <br/>新しい図形の始点。|

大文字の **M** は *startPoint* が絶対座標であることを示し、小文字の **m** は、 *startPoint* が前の点からのオフセットか、前の点がない場合は (0,0) からのオフセットであることを示します。

**注**  移動コマンドに続けて複数の点を指定することもできます。 これらの点の間には、直線コマンドを指定した場合と同様に直線が描画されます。 ただし、これは推奨されるスタイルではありません。代わりに専用の直線コマンドを使ってください。

**描画コマンド**

描画コマンドはいくつかの図形コマンドから構成されます。図形コマンドには、直線、水平線、垂直線、三次ベジエ曲線、二次ベジエ曲線、平滑三次ベジエ曲線、平滑二次ベジエ曲線、楕円円弧があります。

すべての描画コマンドで大文字と小文字が区別されます。 大文字は絶対座標を示し、小文字は前のコマンドからの相対座標を示します。

セグメントの制御点は、前のセグメントの終点からの相対値で表されます。 同じ種類のコマンドを複数回続けて入力するときは、重複するコマンドの入力を省略できます。 たとえば、`L 100,200 300,400` は、`L 100,200 L 300,400` と同じです。

**Line コマンド**

現在の点と指定された終点の間に直線を作成します。 たとえば、`l 20 30` や `L 20,30` は有効な直線コマンドです。 [**LineGeometry**](/uwp/api/Windows.UI.Xaml.Media.LineGeometry) オブジェクトと同等の結果が定義されます。

| 構文 |
|--------|
| `L` _endPoint_ <br/>または<br/>`l` _endPoint_ |

| 用語 | 説明 |
|------|-------------|
| endPoint | [**ポイント**](/uwp/api/Windows.Foundation.Point)<br/>線の終点。|

**水平線コマンド**

現在の点と指定された x 座標の間に水平線を作成します。 `H 90` は、有効な水平線コマンドの例です。

| 構文 |
|--------|
| `H ` _x_ <br/> - または - <br/>`h ` _x_ |

| 用語 | 説明 |
|------|-------------|
| x | [**小数**](/dotnet/api/system.double) <br/> 直線の終点の x 座標。 |

**垂直線コマンド**

現在の点と指定された y 座標の間に垂直線を作成します。 `v 90` は、有効な垂直線コマンドの例です。

| 構文 |
|--------|
| `V `_y_ <br/> - または - <br/> `v `_y_ |

| 用語 | 説明 |
|------|-------------|
| *y* | [**小数**](/dotnet/api/system.double) <br/> 直線の終点の y 座標。 |

**三次ベジエ曲線コマンド**

指定した 2 つの制御点 ( *controlPoint1* と *controlPoint2* ) を使って、現在の点と指定した終点の間に三次ベジエ曲線を作成します。 `C 100,200 200,400 300,200` は、有効な曲線コマンドの例です。 [**BezierSegment**](/uwp/api/Windows.UI.Xaml.Media.BezierSegment) オブジェクトを持つ [**PathGeometry**](/uwp/api/Windows.UI.Xaml.Media.PathGeometry) オブジェクトと同等の結果が定義されます。

| 構文 |
|--------|
| `C ` *controlPoint1* *controlPoint2* *endPoint* <br/> - または - <br/> `c ` *controlPoint1* *controlPoint2* *endPoint* |

| 用語 | 説明 |
|------|-------------|
| *controlPoint1* | [**ポイント**](/uwp/api/Windows.Foundation.Point) <br/> 曲線の 1 つ目の制御点。曲線の前半の接線を決定します。 |
| *controlPoint2* | [**ポイント**](/uwp/api/Windows.Foundation.Point) <br/> 曲線の 2 つ目の制御点。曲線の後半の接線を決定します。 |
| *endPoint* | [**ポイント**](/uwp/api/Windows.Foundation.Point) <br/> 曲線が描画される点。 | 

**二次ベジエ曲線コマンド**

指定した制御点 ( *controlPoint* ) を使って、現在の点と指定した終点の間に二次ベジエ曲線を作成します。 たとえば、`q 100,200 300,200` は有効な二次ベジエ曲線コマンドです。 [**QuadraticBezierSegment**](/uwp/api/Windows.UI.Xaml.Media.QuadraticBezierSegment) を持つ [**PathGeometry**](/uwp/api/Windows.UI.Xaml.Media.PathGeometry) と同等の結果が定義されます。

| 構文 |
|--------|
| `Q ` *controlPoint endPoint* <br/> - または - <br/> `q ` *controlPoint endPoint* |

| 用語 | 説明 |
|------|-------------|
| *controlPoint* | [**ポイント**](/uwp/api/Windows.Foundation.Point) <br/> 曲線の制御点。曲線の前半と後半の接線を決定します。 |
| *endPoint* | [**ポイント**](/uwp/api/Windows.Foundation.Point)<br/> 曲線が描画される点。 |

**平滑三次ベジエ曲線コマンド**

現在の点と指定した終点の間に三次ベジエ曲線を作成します。 1 つ目の制御点は、現在の点に対する前のコマンドの 2 つ目の制御点のリフレクションと見なされます。 前のコマンドがない場合や、前のコマンドが三次ベジエ曲線コマンドまたは平滑三次ベジエ曲線コマンドでない場合、1 つ目の制御点は現在の点と一致すると見なされます。 2 つ目の制御点 (曲線の終端の制御点) は、 *controlPoint2* によって指定します。 たとえば、 `S 100,200 200,300` は有効な平滑三次ベジエ曲線コマンドです。 このコマンドは、前に曲線セグメントがある場合の、 [**BezierSegment**](/uwp/api/Windows.UI.Xaml.Media.BezierSegment) を持つ [**PathGeometry**](/uwp/api/Windows.UI.Xaml.Media.PathGeometry) と同等の結果を定義します。

| 構文 |
|--------|
| `S` *controlPoint2* *endPoint* <br/> - または - <br/>`s` *controlPoint2 endPoint* |

| 用語 | 説明 |
|------|-------------|
| *controlPoint2* | [**ポイント**](/uwp/api/Windows.Foundation.Point) <br/> 曲線の制御点。曲線の後半の接線を決定します。 |
| *endPoint* | [**ポイント**](/uwp/api/Windows.Foundation.Point)<br/> 曲線が描画される点。 |

**平滑二次ベジエ曲線コマンド**

現在の点と指定した終点の間に二次ベジエ曲線を作成します。 制御点は、現在の点に対する前のコマンドの制御点のリフレクションと見なされます。 前のコマンドがない場合や、前のコマンドが二次ベジエ曲線コマンドまたは平滑二次ベジエ曲線コマンドでない場合、制御点は現在の点と一致します。 このコマンドは、前に曲線セグメントがある場合の、 [**QuadraticBezierSegment**](/uwp/api/Windows.UI.Xaml.Media.QuadraticBezierSegment) を持つ [**PathGeometry**](/uwp/api/Windows.UI.Xaml.Media.PathGeometry) と同等の結果を定義します。

| 構文 |
|--------|
| `T` *controlPoint* *endPoint* <br/> - または - <br/> `t` *controlPoint* *endPoint* |

| 用語 | 説明 |
|------|-------------|
| *controlPoint* | [**ポイント**](/uwp/api/Windows.Foundation.Point)<br/> 曲線の制御点。曲線の前半の接線を決定します。 |
| *endPoint* | [**ポイント**](/uwp/api/Windows.Foundation.Point)<br/> 曲線が描画される点。 |

**楕円の円弧コマンド**

現在の点と指定された終点の間に楕円の円弧を作成します。 [**ArcSegment**](/uwp/api/Windows.UI.Xaml.Media.ArcSegment) を持つ [**PathGeometry**](/uwp/api/Windows.UI.Xaml.Media.PathGeometry) と同等の結果が定義されます。

| 構文 |
|--------|
| `A ` *size* *rotationAngle* *isLargeArcFlag* *sweepDirectionFlag* *endPoint* <br/> - または - <br/>`a ` *size* *rotationAngle* *isLargeArcFlag* *sweepDirectionFlag* *endPoint* |

| 用語 | 説明 |
|------|-------------|
| *size* | [**サイズ**](/uwp/api/Windows.Foundation.Size)<br/>円弧の x 半径と y 半径。 |
| *rotationAngle* | [**小数**](/dotnet/api/system.double) <br/> 楕円の回転 (度単位)。 |
| *isLargeArcFlag* | 円弧の角度を 180 度以上にする場合は 1 に設定します。それ以外の場合は 0 に設定します。 |
| *sweepDirectionFlag* | 円弧が正の角度の方向に描画される場合は 1 に設定します。それ以外の場合は 0 に設定します。 |
| *endPoint* | [**ポイント**](/uwp/api/Windows.Foundation.Point) <br/> 円弧が描画される点。 |

**コマンドを閉じる**

現在の図形を終了し、現在の点と図の開始点を結ぶ線を作成します。 このコマンドは、図形の最初のセグメントと最後のセグメントの間に線結合 (コーナー) を作成します。

| 構文 |
|--------|
| `Z` <br/> または <br/> `z ` |

**Point 構文**

点の x 座標と y 座標を記述します。 [**Point**](/uwp/api/Windows.Foundation.Point) もご覧ください。

| 構文 |
|--------|
| *x* 、 *y*<br/> - または - <br/>*x* *y* |

| 用語 | 説明 |
|------|-------------|
| *x* | [**小数**](/dotnet/api/system.double) <br/> 点の x 座標。 |
| *y* | [**小数**](/dotnet/api/system.double) <br/> 点の y 座標。 |

**その他のメモ**

標準的な数値ではなく、次の特殊な値を使用することもできます。 これらの値では、大文字と小文字が区別されます。

-   **Infinity** : **PositiveInfinity** を表します。
-   **\- 無限大** : **.negativeinfinity** を表します。
-   **NaN** : **NaN** を表します。

10 進数や整数を使う代わりに、指数表記を使うこともできます。 たとえば、`+1.e17` は有効な値です。

## <a name="design-tools-that-produce-move-and-draw-commands"></a>移動と描画のコマンドを生成するデザイン ツール

Blend for Microsoft Visual Studio 2015 で **ペン** ツールやその他の描画ツールを使うと、通常、 [**Path**](/uwp/api/Windows.UI.Xaml.Shapes.Path) オブジェクトが移動と描画のコマンドと共に生成されます。

Windows ランタイムのコントロール用の既定の XAML テンプレートを見ると、定義されているコントロールのパーツの一部に、移動と描画のコマンドのデータが含まれていることに気付くことがあります。 たとえば、一部のコントロールで使われる [**PathIcon**](/uwp/api/Windows.UI.Xaml.Controls.PathIcon) では、データが移動と描画のコマンドとして定義されています。

その他のよく使われるベクター グラフィックス デザイン ツールにも、ベクターを XAML 形式で出力できるエクスポーターやプラグインがあります。 これらは通常、レイアウト コンテナーに [**Path**](/uwp/api/Windows.UI.Xaml.Shapes.Path) オブジェクトを作成し、 [**Path.Data**](/uwp/api/windows.ui.xaml.shapes.path.data) に移動と描画のコマンドを設定します。 XAML には、別々のブラシを適用できるように複数の **Path** 要素が含まれている場合があります。 これらのエクスポーターやプラグインの多くは、本来は Windows Presentation Foundation (WPF) の XAML や Silverlight 用に作成されたものですが、XAML のパス構文は Windows ランタイム XAML と同じです。 通常、エクスポーターからの XAML の大部分を Windows ランタイムの XAML ページに直接貼り付けることができます  (ただし、変換後の XAML に **RadialGradientBrush** が含まれている場合、このブラシは Windows ランタイム XAML でサポートされないため、使うことはできません)。

## <a name="related-topics"></a>関連トピック

* [図形の描画](../design/controls-and-patterns/shapes.md)
* [ブラシの使用](../design/style/brushes.md)
* [**Path.Data**](/uwp/api/windows.ui.xaml.shapes.path.data)
* [**PathIcon**](/uwp/api/Windows.UI.Xaml.Controls.PathIcon)