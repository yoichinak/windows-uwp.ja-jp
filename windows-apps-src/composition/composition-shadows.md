---
title: コンポジションの影
description: Shadow Api を使用すると、動的にカスタマイズ可能なシャドウを UI コンテンツに追加できます。
ms.date: 07/16/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 29ca04ac3faf3a2884bcc2346177f49222cf786e
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89166426"
---
# <a name="shadows-in-windows-ui"></a>Windows UI の影

[Dropshadow](/uwp/api/Windows.UI.Composition.DropShadow)クラスは、 [SpriteVisual](/uwp/api/windows.ui.composition.spritevisual)または[レイヤービジュアル](/uwp/api/windows.ui.composition.layervisual)(ビジュアルのサブツリー) に適用できる構成可能なシャドウを作成するための手段を提供します。 ビジュアルレイヤー内のオブジェクトの慣例と同様に、CompositionAnimations を使用して DropShadow のすべてのプロパティをアニメーション化できます。

## <a name="basic-drop-shadow"></a>基本ドロップシャドウ

基本的な影を作成するには、新しい DropShadow を作成し、ビジュアルに関連付けるだけです。 既定では、影は四角形です。 標準のプロパティセットを使用して、影の外観を調整することができます。

```cs
var basicRectVisual = _compositor.CreateSpriteVisual();
basicRectVisual.Brush = _compositor.CreateColorBrush(Colors.Blue);
basicRectVisual.Offset = new Vector3(100, 100, 20);
basicRectVisual.Size = new Vector2(300, 300);

var basicShadow = _compositor.CreateDropShadow();
basicShadow.BlurRadius = 25f;
basicShadow.Offset = new Vector3(20, 20, 20);

basicRectVisual.Shadow = basicShadow;
```

![基本的な DropShadow を使用した四角形ビジュアル](images/rectangular-dropshadow.png)

## <a name="shaping-the-shadow"></a>影を整える

DropShadow の形状を定義するには、いくつかの方法があります。

- **既定値を使用する** -既定では、dropshadow 図形は CompositionDropShadowSourcePolicy の ' 既定 ' モードで定義されます。 SpriteVisual の場合、マスクが指定されていない限り、既定値は四角形です。 レイヤービジュアルの場合、既定では、ビジュアルのブラシのアルファを使用してマスクが継承されます。
- **マスクを設定** する– [マスク](/uwp/api/windows.ui.composition.dropshadow.mask) プロパティを設定して、影の不透明度マスクを定義できます。
- [**継承マスクを使用する**] を指定します。 [CompositionDropShadowSourcePolicy](/uwp/api/windows.ui.composition.compositiondropshadowsourcepolicy)を使用するには、 [sourcepolicy](/uwp/api/windows.ui.composition.dropshadow.sourcepolicy)プロパティを設定します。 InheritFromVisualContent は、ビジュアルのブラシのアルファから生成されたマスクを使用します。

## <a name="masking-to-match-your-content"></a>コンテンツに一致するマスク

影がビジュアルのコンテンツと一致するようにするには、[シャドウマスク] プロパティにビジュアルのブラシを使用するか、コンテンツからマスクを自動的に継承するように影を設定します。 レイヤービジュアルを使用している場合、影は既定でマスクを継承します。

```cs
var imageSurface = LoadedImageSurface.StartLoadFromUri(new Uri("ms-appx:///Assets/myImage.png"));
var imageBrush = _compositor.CreateSurfaceBrush(imageSurface);

var imageSpriteVisual = _compositor.CreateSpriteVisual();
imageSpriteVisual.Size = new Vector2(400,400);
imageSpriteVisual.Offset = new Vector3(100, 500, 20);
imageSpriteVisual.Brush = imageBrush;

var shadow = _compositor.CreateDropShadow();
shadow.Mask = imageBrush;
// or use shadow.SourcePolicy = CompositionDropShadowSourcePolicy.InheritFromVisualContent;
shadow.BlurRadius = 25f;
shadow.Offset = new Vector3(20, 20, 20);

imageSpriteVisual.Shadow = shadow;
```

![マスクされたドロップシャドウを使用した接続された web イメージ](images/ms-brand-web-dropshadow.png)

## <a name="using-an-alternative-mask"></a>代替マスクの使用

場合によっては、ビジュアルの内容と一致しないように影を整形することが必要になることがあります。 この効果を実現するには、アルファでブラシを使用して Mask プロパティを明示的に設定する必要があります。

次の例では、2つのサーフェイスを読み込みます。1つはビジュアルコンテンツ用で、もう1つはシャドウマスク用です。

```cs
var imageSurface = LoadedImageSurface.StartLoadFromUri(new Uri("ms-appx:///Assets/myImage.png"));
var imageBrush = _compositor.CreateSurfaceBrush(imageSurface);

var circleSurface = LoadedImageSurface.StartLoadFromUri(new Uri("ms-appx:///Assets/myCircleImage.png"));
var customMask = _compositor.CreateSurfaceBrush(circleSurface);

var imageSpriteVisual = _compositor.CreateSpriteVisual();
imageSpriteVisual.Size = new Vector2(400,400);
imageSpriteVisual.Offset = new Vector3(100, 500, 20);
imageSpriteVisual.Brush = imageBrush;

var shadow = _compositor.CreateDropShadow();
shadow.Mask = customMask;
shadow.BlurRadius = 25f;
shadow.Offset = new Vector3(20, 20, 20);

imageSpriteVisual.Shadow = shadow;
```

![サークルマスクドロップシャドウを使用した接続された web イメージ](images/ms-brand-web-masked-dropshadow.png)

## <a name="animating"></a>化

ビジュアルレイヤーの標準と同様、DropShadow プロパティはコンポジションアニメーションを使用してアニメーション化できます。 次に、上記の sprinkles サンプルからコードを変更して、影のぼかし半径をアニメーション化します。

```cs
ScalarKeyFrameAnimation blurAnimation = _compositor.CreateScalarKeyFrameAnimation();
blurAnimation.InsertKeyFrame(0.0f, 25.0f);
blurAnimation.InsertKeyFrame(0.7f, 50.0f);
blurAnimation.InsertKeyFrame(1.0f, 25.0f);
blurAnimation.Duration = TimeSpan.FromSeconds(4);
blurAnimation.IterationBehavior = AnimationIterationBehavior.Forever;
shadow.StartAnimation("BlurRadius", blurAnimation);
```

## <a name="shadows-in-xaml"></a>XAML の影

より複雑なフレームワーク要素に影を追加する場合は、XAML とコンポジションの間のシャドウとの相互運用にいくつかの方法があります。

1. Windows Community Toolkit で利用可能な [DropShadowPanel](https://github.com/windows-toolkit/WindowsCommunityToolkit/blob/master/Microsoft.Toolkit.Uwp.UI.Controls/DropShadowPanel/DropShadowPanel.Properties.cs) を使用します。 使用方法の詳細については、 [DropShadowPanel のドキュメント](/windows/uwpcommunitytoolkit/controls/DropShadowPanel) を参照してください。
1. シャドウホストとして使用するビジュアルを作成して、XAML 配付資料ビジュアルに関連付ける & ます。
1. コンポジションサンプルギャラリーの [SamplesCommon](https://github.com/microsoft/WindowsCompositionSamples/tree/master/SamplesCommon/SamplesCommon) custom CompositionShadow コントロールを使用します。 使用方法については、こちらの例を参照してください。

## <a name="performance"></a>パフォーマンス

ビジュアルレイヤーには、効果を効率的かつ使用可能にするために多くの最適化が行われていますが、設定したオプションによっては、影の生成が比較的負荷のかかる操作になることがあります。 次に、さまざまな種類の影の高レベルの "コスト" を示します。 特定の影が高価になることもありますが、特定のシナリオでは控えめに使用するのが適切な場合があります。

シャドウ特性| コスト
------------- | -------------
方形    | 低
Shadow. Mask      | 高
CompositionDropShadowSourcePolicy の内容 | 高
静的なぼかし半径 | 低
アニメーション化 (ぼかし半径を) | 高

## <a name="additional-resources"></a>その他のリソース

- [コンポジション DropShadow API](/uwp/api/Windows.UI.Composition.DropShadow)
- [WindowsUIDevLabs GitHub リポジトリ](https://github.com/microsoft/WindowsCompositionSamples)