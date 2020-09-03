---
ms.assetid: 02141F86-355E-4046-86EA-2A89D615B7DB
title: ブラシの使用
description: Brush オブジェクトは、コントロールの領域、テキスト、図形の内側または輪郭を塗りつぶすことで、その対象領域を UI 上で視覚的に確認できるようにする目的で使います。
ms.date: 04/28/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9e70c76f3ed659a46dd9834442049849dd3b7761
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175526"
---
# <a name="using-brushes-to-paint-backgrounds-foregrounds-and-outlines"></a>ブラシを使用して背景、前景、輪郭を描画する

[**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush) オブジェクトを使用して、XAML の図形、テキスト、コントロールの内側と輪郭を塗りつぶすことにより、アプリケーション UI で視覚的に確認できるようにします。

> **重要な API**:[Brush クラス](/uwp/api/Windows.UI.Xaml.Media.Brush)

## <a name="introduction-to-brushes"></a>ブラシ入門

アプリのキャンバスに表示される [**Shape**](/uwp/api/Windows.UI.Xaml.Shapes.Shape)、テキスト、[**Control**](/uwp/api/Windows.UI.Xaml.Controls.Control) の領域を塗りつぶすには、**Shape** の [**Fill**](/uwp/api/windows.ui.xaml.shapes.shape.fill) プロパティか、**Control** の [**Background**](/uwp/api/windows.ui.xaml.controls.control.background) および [**Foreground**](/uwp/api/windows.ui.xaml.controls.control.foreground) プロパティを **Brush** 値に設定します。

ブラシには、次のような種類があります。 
-   [**AcrylicBrush**](/uwp/api/windows.ui.xaml.media.acrylicbrush)
-   [**SolidColorBrush**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush)
-   [**LinearGradientBrush**](/uwp/api/Windows.UI.Xaml.Media.LinearGradientBrush) 
-   [**RadialGradientBrush**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush) 
-   [**ImageBrush**](/uwp/api/Windows.UI.Xaml.Media.ImageBrush)
-   [**WebViewBrush**](/uwp/api/Windows.UI.Xaml.Controls.WebViewBrush)
-   [**XamlCompositionBrushBase**](/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase)

## <a name="solid-color-brushes"></a>単色ブラシ

[**SolidColorBrush**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush) は、赤や青などの 1 つの [**Color**](/uwp/api/Windows.UI.Color) で領域を塗りつぶします。 これは、最も基本的なブラシです。 XAML で **SolidColorBrush** とそれで指定する色を定義するには、定義済みの色の名前、16 進数の色値、およびプロパティ要素構文という 3 つの方法があります。

### <a name="predefined-color-names"></a>定義済みの色の名前

[**Yellow**](/uwp/api/windows.ui.colors.yellow)、[**Magenta**](/uwp/api/windows.ui.colors.magenta) など、定義済みの色の名前を使うことができます。 名前付きの色は 256 個存在します。 XAML パーサーにより、色の名前が、正しいカラー チャネルを持つ [**Color**](/uwp/api/Windows.UI.Color) 構造体に変換されます。 256 個の名前付きの色は、カスケード スタイル シート レベル 3 (CSS3) 仕様の *X11* の色名が基になっているため、過去に Web 開発や Web デザインの経験があれば、この一連の色について既にご存じの方も多いと思います。

[**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) の [**Fill**](/uwp/api/windows.ui.xaml.shapes.shape.fill) プロパティを定義済みの色 [**Red**](/uwp/api/windows.ui.colors.red) に設定する例を次に示します。

```xaml
<Rectangle Width="100" Height="100" Fill="Red" />
```

![レンダリングされた SolidColorBrush](images/brushes-solidcolorbrush.jpg)

*四角形に適用された SolidColorBrush*

XAML ではなくコードを使って [**SolidColorBrush**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush) を定義する場合、名前付きの色はそれぞれ、[**Colors**](/uwp/api/windows.ui.colors) クラスの静的プロパティの値として利用できます。 たとえば、名前付きの色 "Orchid" を表す、**SolidColorBrush** の [**Color**](/uwp/api/windows.ui.xaml.media.solidcolorbrush.color) 値を宣言するには、**Color** 値を静的な [**Colors.Orchid**](/uwp/api/windows.ui.colors.orchid) 値に設定します。

### <a name="hexadecimal-color-values"></a>16 進数の色値

16 進数形式の文字列を使い、[**SolidColorBrush**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush) に対して、8 ビットのアルファ チャネルを備えた厳密な 24 ビットの色値を宣言できます。 16 進数文字列はコンポーネント値ごとに、0 - F の範囲の 2 文字で定義され、各コンポーネントの値がアルファ チャネル (不透明度)、赤色チャネル、緑色チャネル、青色チャネルの順に並びます (**ARGB**)。 たとえば、16 進数値 "\#FFFF0000" は、完全に不透明な赤色 (アルファ = "FF"、赤 = "FF"、緑 = "00"、青 = "00") を定義します。

以下の XAML では、名前付きの色 [**Colors.Red**](/uwp/api/windows.ui.colors.red) とまったく同じ結果となるように、[**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) の [**Fill**](/uwp/api/windows.ui.xaml.shapes.shape.fill) プロパティを 16 進数値 "\#FFFF0000" に設定しています。

```xml
<StackPanel>
  <Rectangle Width="100" Height="100" Fill="#FFFF0000" />
</StackPanel>
```

### <a name="property-element-syntax"></a>プロパティ要素構文

プロパティ要素構文を使って [**SolidColorBrush**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush) を定義できます。 この構文は、前の方法よりもやや複雑ですが、[**Opacity**](/uwp/api/windows.ui.xaml.media.brush.opacity) などのプロパティ値を要素に対して追加で指定できます。 プロパティ要素構文を含む XAML 構文について詳しくは、「[XAML の概要](../../xaml-platform/xaml-overview.md)」と「[XAML 構文のガイド](../../xaml-platform/xaml-syntax-guide.md)」をご覧ください。

これまでの例で作成したブラシは、多くの一般的なケースにおいて UI を簡潔に定義できるようする意識的な XAML 言語の省略表現の一環として、暗黙的かつ自動的に作成されました。 次の例では、[**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) を作成し、[**Rectangle.Fill**](/uwp/api/windows.ui.xaml.shapes.shape.fill) プロパティの要素値として [**SolidColorBrush**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush) を明示的に作成します。 **SolidColorBrush** の [**Color**](/uwp/api/windows.ui.xaml.media.solidcolorbrush.color) は [**Blue**](/uwp/api/windows.ui.colors.blue) に設定され、[**Opacity**](/uwp/api/windows.ui.xaml.media.brush.opacity) は 0.5 に設定されます。

```xml
<Rectangle Width="200" Height="150">
    <Rectangle.Fill>
        <SolidColorBrush Color="Blue" Opacity="0.5" />
    </Rectangle.Fill>
</Rectangle>
```

## <a name="linear-gradient-brushes"></a>線状グラデーション ブラシ

[**LinearGradientBrush**](/uwp/api/Windows.UI.Xaml.Media.LinearGradientBrush) は、直線に沿って定義されたグラデーションを使って、領域を塗りつぶします。 この直線は "*グラデーション軸*" と呼ばれます。 [**GradientStop**](/uwp/api/Windows.UI.Xaml.Media.GradientStop) オブジェクトを使って、グラデーションの色とグラデーション軸に沿った位置を指定します。 既定では、ブラシで塗りつぶす領域の左上隅から右下隅に向かってグラデーション軸がとられているため、明暗は斜め方向に適用されます。

[**GradientStop**](/uwp/api/Windows.UI.Xaml.Media.GradientStop) は、グラデーション ブラシの基本的な構成要素です。 グラデーション境界では、塗りつぶす領域に適用されたブラシの、グラデーション軸上の [**Offset**](/uwp/api/windows.ui.xaml.media.gradientstop.offset) 位置における [**Color**](/uwp/api/windows.ui.xaml.media.gradientstop.color) が指定されます。

グラデーション境界の [**Color**](/uwp/api/windows.ui.xaml.media.gradientstop.color) プロパティでは、グラデーション境界の色を指定します。 定義済みの色の名前を使うか、16 進数の **ARGB** 値を指定して、色を設定できます。

[**GradientStop**](/uwp/api/Windows.UI.Xaml.Media.GradientStop) の [**Offset**](/uwp/api/windows.ui.xaml.media.gradientstop.offset) プロパティでは、グラデーション軸に沿った各 **GradientStop** の位置を指定します。 **Offset** は、0 から 1 までの範囲の**倍精度浮動小数点数**です。 **Offset** が 0 のとき、**GradientStop** は、グラデーション軸の開始位置、つまり、[**StartPoint**](/uwp/api/windows.ui.xaml.media.lineargradientbrush.startpoint) 付近に配置されます。 **Offset** が 1 のとき、**GradientStop** は [**EndPoint**](/uwp/api/windows.ui.xaml.media.lineargradientbrush.endpoint) に配置されます。 実用上、[**LinearGradientBrush**](/uwp/api/Windows.UI.Xaml.Media.LinearGradientBrush) には少なくとも 2 つの **GradientStop** 値が必要であり、2 つの **GradientStop** は、それぞれ異なる [**Color**](/uwp/api/windows.ui.xaml.media.gradientstop.color) と、0 - 1 の範囲内の異なる **Offset** 値を持つ必要があります。

次の例では、4 色の線状グラデーションを作成し、それを使って [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) を塗りつぶします。

```xml
<!-- This rectangle is painted with a diagonal linear gradient. -->
<Rectangle Width="200" Height="100">
    <Rectangle.Fill>
        <LinearGradientBrush StartPoint="0,0" EndPoint="1,1">
            <GradientStop Color="Yellow" Offset="0.0" x:Name="GradientStop1"/>
            <GradientStop Color="Red" Offset="0.25" x:Name="GradientStop2"/>
            <GradientStop Color="Blue" Offset="0.75" x:Name="GradientStop3"/>
            <GradientStop Color="LimeGreen" Offset="1.0" x:Name="GradientStop4"/>
        </LinearGradientBrush>
    </Rectangle.Fill>
</Rectangle>
```

グラデーション境界の各点の色は、境界となる 2 つのグラデーション境界によって指定される色の組み合わせとして、直線的に補間されます。 次の図は、この例のグラデーション境界を示しています。 グラデーション境界の位置が円で示され、グラデーション軸が破線で示されています。

![グラデーション境界](images/linear-gradients-stops.png)

*隣接する 2 つのグラデーション境界によって指定される色の組み合わせ*

グラデーション境界が配置される直線は、[**StartPoint**](/uwp/api/windows.ui.xaml.media.lineargradientbrush.startpoint) プロパティと [**EndPoint**](/uwp/api/windows.ui.xaml.media.lineargradientbrush.endpoint) プロパティを、既定値である `(0,0)` と `(1,1)` 以外の値に設定することで変更できます。 **StartPoint** と **EndPoint** の座標値を変更すると、水平方向や垂直方向のグラデーションを作成したり、グラデーション方向を反転したりできるほか、塗りつぶす領域全体よりも小さくなるようにグラデーションの適用範囲を狭めることができます。 グラデーションの領域を狭めるには、**StartPoint** または **EndPoint** (あるいはその両方) の値を 0 - 1 の範囲で変更します。 たとえば、すべてのフェードがブラシの左半分で発生し、右半分は、最後の [**GradientStop**](/uwp/api/Windows.UI.Xaml.Media.GradientStop) の色まで単色であるような水平方向のグラデーションが必要な場合、**StartPoint** を `(0,0)` に、**EndPoint** を `(0.5,0)` に設定します。

### <a name="use-tools-to-make-gradients"></a>ツールによるグラデーションの作成

ここまで、線状グラデーションのしくみについて説明しました。次に、Visual Studio または Blend を使うと、これらのグラデーションの作成が簡単になります。 グラデーションを作成するには、デザイン サーフェイスまたは XAML ビューで、グラデーションを適用するオブジェクトを選択します。 **[ブラシ]** を展開し、 **[線状グラデーション]** タブをクリックします。

![Visual Studio での線状グラデーションの作成](images/tool-gradient-brush-1.png)

*Visual Studio での線状グラデーションの作成*

ここで、グラデーション境界の色を変更したり、下のバーを使って位置をずらしたりできます。 また、バーをクリックして新しいグラデーション境界を追加したり、グラデーション境界をバーの外側にドラッグして削除したりもできます (次のスクリーンショットをご覧ください)。

![プロパティ ウィンドウの下部にあるバーでグラデーション境界を制御](images/tool-gradient-brush-2.png)

*グラデーション設定スライダー*

## <a name="radial-gradient-brushes"></a>放射状グラデーション ブラシ

[**RadialGradientBrush**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush) は、[**Center**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.center)、[**RadiusX**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.radiusx)、[**RadiusY**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.radiusy) プロパティによって定義される楕円内に描画されます。 グラデーションの色は楕円の中心から始まり、半径の位置で終了します。

放射状グラデーションの色は、[**GradientStops**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.gradientstops) コレクション プロパティに追加された色の境界によって定義されます。 それぞれのグラデーション境界により、グラデーションの色とオフセットが決まります。

グラデーションの原点は既定では中央ですが、[**GradientOrigin**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.gradientorigin) プロパティを使用して指定することができます。

[MappingMode](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.mappingmode) は、[**Center**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.center)、[**RadiusX**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.radiusx)、[**RadiusY**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.radiusy)、[**GradientOrigin**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.gradientorigin) が相対座標と絶対座標のどちらを表すかを定めます。

[**MappingMode**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.mappingmode) が `RelativeToBoundingBox` に設定されている場合、3 つのプロパティの X および Y 値は要素境界に対する相対位置として扱われ、[**Center**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.center)、[**RadiusX**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.radiusx)、[**RadiusY**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.radiusy) プロパティでは、`(0,0)` は要素境界の左上部、`(1,1)` は右下部を表し、[**GradientOrigin**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.gradientorigin) プロパティでは、`(0,0)` は中央を表します。

[**MappingMode**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.mappingmode) が `Absolute` に設定されている場合、3 つのプロパティの X および Y 値は要素境界内の絶対座標として扱われます。

次の例では、4 色の線状グラデーションを作成し、それを使って [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) を塗りつぶします。

```xml
<!-- This rectangle is painted with a radial gradient. -->
<Rectangle Width="200" Height="200">
    <Rectangle.Fill>
        <media:RadialGradientBrush>
            <GradientStop Color="Blue" Offset="0.0" />
            <GradientStop Color="Yellow" Offset="0.2" />
            <GradientStop Color="LimeGreen" Offset="0.4" />
            <GradientStop Color="LightBlue" Offset="0.6" />
            <GradientStop Color="Blue" Offset="0.8" />
            <GradientStop Color="LightGray" Offset="1" />
        </media:RadialGradientBrush>
    </Rectangle.Fill>
</Rectangle>
```

グラデーション境界間の各点の色は、隣接する 2 つのグラデーション境界によって指定される色の組み合わせとして、放射状に補間されます。 次の図は、この例のグラデーション境界を示しています。 

![グラデーション境界](images/radial-gradient.png)

*グラデーション境界*

## <a name="image-brushes"></a>イメージ ブラシ

[**ImageBrush**](/uwp/api/Windows.UI.Xaml.Media.ImageBrush) では、イメージ ファイル ソースから取得した画像で領域を塗りつぶします。 [**ImageSource**](/uwp/api/Windows.UI.Xaml.Media.ImageSource) プロパティに、読み込む画像のパスを設定します。 通常、イメージ ソースは、アプリのリソースに含まれる **Content** 項目から取得します。

[**ImageBrush**](/uwp/api/Windows.UI.Xaml.Media.ImageBrush) では、既定で、描画する領域が完全に埋まるように画像が拡大されます。描画する領域と画像の縦横比が異なる場合、画像がゆがむ可能性があります。 この動作を変更するには、[**Stretch**](/uwp/api/windows.ui.xaml.media.tilebrush.stretch) プロパティを既定値の **Fill** から **None**、**Uniform**、または **UniformToFill** に変更します。

次の例では、[**ImageBrush**](/uwp/api/Windows.UI.Xaml.Media.ImageBrush) を作成し、[**ImageSource**](/uwp/api/Windows.UI.Xaml.Media.ImageSource) を licorice.jpg という画像に設定します。この画像は、アプリのリソースとして取り込んでおく必要があります。 この **ImageBrush** を使って、図形 [**Ellipse**](/uwp/api/Windows.UI.Xaml.Shapes.Ellipse) で定義される領域を塗りつぶします。

```xml
<Ellipse Height="200" Width="300">
   <Ellipse.Fill>
     <ImageBrush ImageSource="licorice.jpg" />
   </Ellipse.Fill>
</Ellipse>
```

![レンダリングされた ImageBrush。](images/brushes-imagebrush.jpg)

*レンダリングされた ImageBrush*

[**ImageBrush**](/uwp/api/Windows.UI.Xaml.Media.ImageBrush) と [**Image**](/uwp/api/Windows.UI.Xaml.Controls.Image) は、どちらも Uniform Resource Identifier (URI) でイメージ ソース ファイルを参照します。また、イメージ ソース ファイルに使うことができる画像形式には、さまざまなものがあります。 これらのイメージ ソース ファイルは、URI として指定されます。 イメージ ソースの指定、使用できる画像の形式、アプリへのパッケージ化について詳しくは、「[画像とイメージ ブラシ](../controls-and-patterns/images-imagebrushes.md)」をご覧ください。

## <a name="brushes-and-text"></a>ブラシとテキスト

ブラシを使って、テキスト要素にレンダリング特性を適用することもできます。 たとえば、[**TextBlock**](/uwp/api/Windows.UI.Xaml.Controls.TextBlock) の [**Foreground**](/uwp/api/windows.ui.xaml.controls.textblock.foreground) プロパティに対して、[**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush) を指定できます。 テキストには、ここで説明したすべてのブラシを適用できます。 ただし、背景にまで滲んでしまうブラシを使用するとテキストが読みにくくなる可能性があるため、テキストにどのブラシを適用するかに注意してください。 テキスト要素の装飾性を高めることが重要でなければ、ほとんどの場合は [**SolidColorBrush**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush) を使うと読みやすくなります。

単色を使う場合でも、テキストには、そのレイアウト コンテナーの背景色に対して十分なコントラストを持つ色を選ぶ必要があります。 テキストの前景とテキスト コンテナーの背景とのコントラスト レベルは、アクセシビリティにかかわる考慮事項です。

## <a name="webviewbrush"></a>WebViewBrush

[**WebViewBrush**](/uwp/api/Windows.UI.Xaml.Controls.WebViewBrush) は、[**WebView**](/uwp/api/Windows.UI.Xaml.Controls.WebView) コントロールに通常表示されるコンテンツにアクセスできる特殊なブラシです。 四角形の **WebView** コントロール領域にコンテンツをレンダリングする代わりに、**WebViewBrush** は、レンダリング サーフェイスに [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush) タイプのプロパティを持つ別の要素にコンテンツを描画します。 **WebViewBrush** は、必ずしもすべての用途に適したブラシではありませんが、**WebView** の切り替えで効果的に使うことができます。 詳しくは、「[**WebViewBrush**](/uwp/api/Windows.UI.Xaml.Controls.WebViewBrush)」をご覧ください。

## <a name="xamlcompositionbrushbase"></a>XamlCompositionBrushBase

[**XamlCompositionBrushBase**](/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase) は、[**CompositionBrush**](/uwp/api/Windows.UI.Composition.CompositionBrush) を使って XAML UI 要素を描画する、カスタム ブラシを作成するために使用する基底クラスです。

これにより、Windows.UI.Xaml と Windows.UI.Composition レイヤー間の "ドロップ ダウン " 相互運用を行えます。詳しくは、「[**ビジュアル レイヤーの概要**](../../composition/visual-layer.md)」をご覧ください。 

カスタム ブラシを作成するには、XamlCompositionBrushBase から継承する新しいクラスを作成し、必要なメソッドを実装します。

これにより、[**CompositionEffectBrush**](/uwp/api/Windows.UI.Composition.CompositionEffectBrush) を使って、[**効果**](/uwp/composition/composition-effects)を XAML UIElements に適用することができます。たとえば、[**XamlLight**](/uwp/api/windows.ui.xaml.media.xamllight) に照射される XAML UIElement の反射プロパティを制御する **GaussianBlurEffect** や [**SceneLightingEffect**](/uwp/api/Windows.UI.Composition.Effects.SceneLightingEffect) などの効果を適用できます。

コード例については、[**XamlCompositionBrushBase**](/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase) に関する記事を参照してください。

## <a name="brushes-as-xaml-resources"></a>XAML リソースとしてのブラシ

すべてのブラシは、XAML リソース ディクショナリに、キーを持つ XAML リソースとして宣言できます。 これにより、UI 内の複数の要素に適用する同じブラシの値を簡単に複製することができます。 その後、このブラシの値を共有し、XAML 内で [{StaticResource}](/uwp/xaml-platform/staticresource-markup-extension) 使用法としてブラシ リソースを参照するときに適用することができます。 たとえば、共有されたブラシを参照する XAML コントロール テンプレートがあるとき、そのコントロール テンプレート自体を、キーを持つ XAML リソースとして利用することができます。

## <a name="brushes-in-code"></a>コードを使ったブラシ

コードを使ってブラシを定義するよりも、XAML を使ってブラシを指定する方が一般的です。 これは、ブラシが通常は XAML リソースとして定義されるためであり、ブラシの値がデザイン ツールの出力結果である場合や、XAML UI 定義の一部としての出力結果である場合が多いためです。 ただし、コードを使ってブラシを定義する必要がある場合は、すべての種類の [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush) をコードのインスタンス化に使うことができます。

コードを使って [**SolidColorBrush**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush) を作成するには、[**Color**](/uwp/api/Windows.UI.Color) パラメーターを受け取るコンストラクターを使います。 次のように、[**Colors**](/uwp/api/windows.ui.colors) クラスの静的プロパティである値を渡します。

```cs
SolidColorBrush blueBrush = new SolidColorBrush(Windows.UI.Colors.Blue);
```

```vb
Dim blueBrush as SolidColorBrush = New SolidColorBrush(Windows.UI.Colors.Blue)
```

```cppwinrt
Windows::UI::Xaml::Media::SolidColorBrush blueBrush{ Windows::UI::Colors::Blue() };
```

```cpp
blueBrush = ref new SolidColorBrush(Windows::UI::Colors::Blue);
```

[**WebViewBrush**](/uwp/api/Windows.UI.Xaml.Controls.WebViewBrush) と [**ImageBrush**](/uwp/api/Windows.UI.Xaml.Media.ImageBrush) を UI プロパティに使う場合、その前に既定のコンストラクターを使って他の API を呼び出してください。

-   コードを使って [**ImageBrush**](/uwp/api/Windows.UI.Xaml.Media.ImageBrush) を定義する場合、[**ImageSource**](/uwp/api/windows.ui.xaml.media.imagebrush.imagesourceproperty) では [**BitmapImage**](/uwp/api/Windows.UI.Xaml.Media.Imaging.BitmapImage) (URI ではない) が必要です。 ソースがストリームである場合は、[**SetSourceAsync**](/uwp/api/windows.ui.xaml.media.imaging.bitmapsource.setsourceasync) メソッドを使って値を初期化します。 ソースが、**ms-appx** スキームまたは **ms-resource** スキームを使うアプリ内のコンテンツを含む URI である場合は、URI を受け取る [**BitmapImage**](/uwp/api/windows.ui.xaml.media.imaging.bitmapimage) コンストラクターを使います。 イメージ ソースが使えるようになるまで代替コンテンツを表示することが必要であるなど、イメージ ソースの取得やデコードについてタイミングの問題がある場合は、[**ImageOpened**](/uwp/api/windows.ui.xaml.media.imagebrush.imageopened) イベントを処理することも検討してください。
-   [**WebViewBrush**](/uwp/api/Windows.UI.Xaml.Controls.WebViewBrush) については、[**SourceName**](/uwp/api/windows.ui.xaml.controls.webviewbrush.sourcename) プロパティを最近リセットした場合、または [**WebView**](/uwp/api/Windows.UI.Xaml.Controls.WebView) のコンテンツをコードを使って変更した場合に、[**Redraw**](/uwp/api/windows.ui.xaml.controls.webviewbrush.redraw) の呼び出しが必要になる場合があります。

コード例については、[**WebViewBrush**](/uwp/api/Windows.UI.Xaml.Controls.WebViewBrush)、[**ImageBrush**](/uwp/api/Windows.UI.Xaml.Media.ImageBrush)、[**XamlCompositionBrushBase**](/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase) に関する記事を参照してください。
 

 