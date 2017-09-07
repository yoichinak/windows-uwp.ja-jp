---
author: scottmill
ms.assetid: 03dd256f-78c0-e1b1-3d9f-7b3afab29b2f
title: "コンポジションのブラシ"
description: "ブラシは、その出力で Visual の領域を塗りつぶします。 さまざまなブラシで、出力の種類もさまざまです。"
ms.author: scotmi
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.openlocfilehash: 71ffd720dac0be29f003da965054db16ae8e39c0
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="composition-brushes"></a>コンポジションのブラシ

\[Windows 10 の UWP アプリ向けに更新。 Windows 8.x の記事については、[アーカイブ](http://go.microsoft.com/fwlink/p/?linkid=619132)をご覧ください。\]

ブラシは、その出力で [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858) の領域を塗りつぶします。 さまざまなブラシで、出力の種類もさまざまです。 コンポジション API には、次の 3 種類のブラシが用意されています。

-   [**CompositionColorBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589399) はビジュアルを単色で塗りつぶします。
-   [**CompositionSurfaceBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589415) はビジュアルをコンポジション サーフェスの内容で塗りつぶします。
-   [**CompositionEffectBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589406) はビジュアルをコンポジション効果の内容で塗りつぶします。

すべてのブラシは [**CompositionBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589398) を継承します。ブラシは、[**Compositor**](https://msdn.microsoft.com/library/windows/apps/Dn706789) によって直接的または間接的に作成され、デバイスに依存しないリソースです。 ブラシはデバイスに依存しませんが、[**CompositionSurfaceBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589415) と [**CompositionEffectBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589406) は、デバイスに依存するコンポジション サーフェスの内容で [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858) を塗りつぶします。

-   [前提条件](./composition-brushes.md#prerequisites)
-   [色の基本](./composition-brushes.md#color-basics)
    -   [アルファ モード](./composition-brushes.md#alpha-modes)
-   [色ブラシの使用](./composition-brushes.md#using-color-brush)
-   [サーフェス ブラシの使用](./composition-brushes.md#using-surface-brush)
-   [ストレッチと整列の構成](./composition-brushes.md#configuring-stretch-and-alignment)

## <a name="prerequisites"></a>前提条件

この概要では、「[コンポジション UI](visual-layer.md)」で説明されているように、基本的なコンポジション アプリケーションの構造を理解していることを前提としています。

## <a name="color-basics"></a>色の基本

[**CompositionColorBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589399) を使って塗りつぶす前に、色を選択する必要があります。 コンポジション API では、Windows ランタイムの構造体 Color を使って色を表します。 Color 構造体では、sRGB エンコーディングを使用します。 sRGB エンコードは、色をアルファ、赤、緑、青の 4 つのチャンネルに分割します。 各コンポーネントは、浮動小数点値の、通常は 0.0 ～ 1.0 の範囲で表されます。 値が 0.0 の場合はその色がまったく含まれないことを意味し、値が 1.0 の場合はその色が完全に表示されていることを意味します。 アルファ コンポーネントの場合、0.0 は完全に透明な色を表し、1.0 は完全に不透明な色を表します。

### <a name="alpha-modes"></a>アルファ モード

[**CompositionColorBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589399) の色の値は、常にストレート アルファとして解釈されます。

## <a name="using-color-brush"></a>色ブラシの使用

色ブラシを作成するには、Compositor.[**CreateColorBrush**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositor.createcolorbrush.aspx) メソッドを呼び出します。このメソッドは [**CompositionColorBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589399) を返します。 **CompositionColorBrush** の既定の色は \#00000000 です。 次の図とコードは、黒の色ブラシで描かれた四角形を、色の値が 0x9ACD32 である単色ブラシで塗りつぶす小規模なビジュアル ツリーを示しています。

![CompositionColorBrush](images/composition-compositioncolorbrush.png)
```cs
Compositor _compositor;
ContainerVisual _container;
SpriteVisual visual1, visual2;
CompositionColorBrush _blackBrush, _greenBrush; 

_compositor = new Compositor();
_container = _compositor.CreateContainerVisual();

_blackBrush = _compositor.CreateColorBrush(Colors.Black);
visual1 = _compositor.CreateSpriteVisual();
visual1.Brush = _blackBrush;
visual1.Size = new Vector2(156, 156);
visual1.Offset = new Vector3(0, 0, 0);

_ greenBrush = _compositor.CreateColorBrush(Color.FromArgb(0xff, 0x9A, 0xCD, 0x32));
Visual2 = _compositor.CreateSpriteVisual();
Visual2.Brush = _greenBrush;
Visual2.Size = new Vector2(150, 150);
Visual2.Offset = new Vector3(3, 3, 0);
```

他のブラシとは異なり、[**CompositionColorBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589399) の作成は比較的安価な操作です。 毎回ほとんどまたはまったくパフォーマンスに影響を与えずに、**CompositionColorBrush** オブジェクトを作成できます。

## <a name="using-surface-brush"></a>サーフェス ブラシの使用

[**CompositionSurfaceBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589415) は、コンポジション サーフェス ([**ICompositionSurface**](https://msdn.microsoft.com/library/windows/apps/Dn706819) オブジェクトで表される) を使ってビジュアルを塗りつぶします。 次の図は、D2D を使って **ICompositionSurface** にレンダリングされたリコリスのビットマップで塗りつぶされた正方形のビジュアルを示しています。

![CompositionSurfaceBrush](images/composition-compositionsurfacebrush.png) 最初の例では、ブラシで使用するためのコンポジション サーフェスを初期化します。 コンポジション サーフェスは、[**CompositionSurfaceBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589415) と Url を文字列として受け取るヘルパー メソッド LoadImage を使って作成されます。 このヘルパー メソッドは、Url から画像を読み込み、画像を [**ICompositionSurface**](https://msdn.microsoft.com/library/windows/apps/Dn706819) にレンダリングして、そのサーフェスを **CompositionSurfaceBrush** の内容として設定します。 **ICompositionSurface** はネイティブ コードでのみ公開されるため、LoadImage メソッドはネイティブ コードで実装されていることに注意してください。

```cs
LoadImage(Brush,
          "ms-appx:///Assets/liqorice.png");
```

サーフェス ブラシを作成するには、Compositor.[**CreateSurfaceBrush**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositor.createsurfacebrush.aspx) メソッドを呼び出します。 このメソッドは、[**CompositionSurfaceBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589415) オブジェクトを返します。 次のコードは、**CompositionSurfaceBrush** の内容を使ってビジュアルを塗りつぶすために使用できるコードを示しています。

```cs
Compositor _compositor;
ContainerVisual _container;
SpriteVisual visual;
CompositionSurfaceBrush _surfaceBrush;

_surfaceBrush = _compositor.CreateSurfaceBrush();
LoadImage(_surfaceBrush, "ms-appx:///Assets/liqorice.png");
visual.Brush = _surfaceBrush;
```

## <a name="configuring-stretch-and-alignment"></a>ストレッチと整列の構成

[**CompositionSurfaceBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589415) 用の [**ICompositionSurface**](https://msdn.microsoft.com/library/windows/apps/Dn706819) の内容が、描画されるビジュアルの領域を満たさない場合があります。 この場合、コンポジション API はブラシの [**HorizontalAlignmentRatio**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionsurfacebrush.horizontalalignmentratio.aspx)、[**VerticalAlignmentRatio**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionsurfacebrush.verticalalignmentratio)、および [**Stretch**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionsurfacebrush.stretch) モードの設定を使って、残りの領域を塗りつぶす方法を決定します。

-   [**HorizontalAlignmentRatio**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionsurfacebrush.horizontalalignmentratio.aspx) と [**VerticalAlignmentRatio**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionsurfacebrush.verticalalignmentratio) は float 型であり、ビジュアルの境界内でのブラシの配置を制御するために使用できます。
    -   値 0.0 は、ブラシの左上隅をビジュアルの左上隅に整列します。
    -   値 0.5 は、ブラシの中央をビジュアルの中央に整列します。
    -   値 1.0 は、ブラシの右下隅をビジュアルの右下隅に整列します。
-   [**Stretch**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionsurfacebrush.stretch) プロパティには、[**CompositionStretch**](https://msdn.microsoft.com/library/windows/apps/Dn706786) 列挙体で定義されている次の値を指定します。
    -   None: ブラシは拡大されず、ビジュアルの領域全体が塗りつぶされません。 この Stretch の設定には注意してください。ブラシがビジュアルの領域よりも大きい場合、ブラシの内容はクリップされます。 ビジュアルの領域を塗りつぶすために使用するブラシの部分は、[**HorizontalAlignmentRatio**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionsurfacebrush.horizontalalignmentratio.aspx) プロパティと [**VerticalAlignmentRatio**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionsurfacebrush.verticalalignmentratio) プロパティを使って制御できます。
    -   Uniform: ブラシはビジュアルの領域に合わせて拡大縮小されます。ブラシの縦横比は維持されます。 これは既定値です。
    -   UniformToFill: ブラシはビジュアルの領域が完全に塗りつぶされるように拡大縮小されます。ブラシの縦横比は維持されます。
    -   Fill: ブラシはビジュアルの領域に合わせて拡大縮小されます。 ブラシの高さと幅は個々に拡大縮小されるため、ブラシの元の縦横比は維持されません。 つまり、ビジュアルの領域を完全に塗りつぶすために、ブラシがゆがむことがあります。

 

## <a name="related-topics"></a>関連トピック
[BeginDraw と EndDraw によるコンポジションでの DirectX と Direct2D のネイティブ相互運用](composition-native-interop.md)



