---
title: コンポジションの光源
description: コンポジション照明 Api を使用して、動的3D 光源をアプリケーションに追加できます。
ms.date: 07/16/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 5531382ef46346a40844a8eb5a5a77c0ad565fbb
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89154706"
---
# <a name="using-lights-in-windows-ui"></a>Windows UI でのライトの使用

Windows の UI の Api を使用すると、リアルタイムのアニメーションと効果を作成できます。 合成照明は、2D アプリケーションで3D 照明を使用できるようにします。 この概要では、コンポジションライトを設定する方法、各ライトを受信するためのビジュアルを識別する方法、および効果を使用してコンテンツの素材を定義する方法の機能を実行します。

> [!NOTE]
> [CompositionLights](/uwp/api/Windows.UI.Composition.CompositionLight)を適用して Xaml uielements を点灯さ[せる方法について](/uwp/api/windows.ui.xaml.media.xamllight)は、「 [xaml ライティング](xaml-lighting.md)」を参照してください。

コンポジション照明を使用すると、次のことを許可することで、興味深い UI を作成できます

- シーン内の他のオブジェクトから独立した光の変換。音楽再生シーンなどのイマーシブシナリオを実現します。
- オブジェクトとライトを組み合わせることにより、シーンの残りの部分とは無関係に移動し [、Fluent の](../design/style/reveal.md) 強調表示などのシナリオが可能になります。
- 素材と深度を作成するためのグループとしてのライトとシーン全体の変換。

コンポジション照明では、 **ライト**、 **ターゲット**、 **SceneLightingEffect**という3つの主要概念がサポートされています。

## <a name="light"></a>白

[CompositionLight](/uwp/api/windows.ui.composition.compositionlight) を使用すると、さまざまなライトを作成し、それらを座標空間に配置できます。 これらのライトは、ライトで点灯していることを示すビジュアルをターゲットにします。

### <a name="light-types"></a>ライトの種類

| Type | 説明 |
| --- | --- |
| [AmbientLight](/uwp/api/windows.ui.composition.ambientlight) | シーン内のすべての要素によって反射される非指向性ライトを出力する光源。 |
| [Distの薄い](/uwp/api/windows.ui.composition.distantlight) | 1つの方向に光を発する、無限に大きく離れた光源。 Sun と同様です。 |
| [PointLight](/uwp/api/windows.ui.composition.pointlight) | すべての方向に光を発する光源のポイントソース。 電球のようになります。 |
| [スポット](/uwp/api/windows.ui.composition.spotlight) | 光の内側と外側のコーンを出力する光源。 懐中電灯に似ています。 |

## <a name="targets"></a>対象サーバー

ライトがビジュアル ( [ターゲット](/uwp/api/windows.ui.composition.compositionlight.targets) リストに追加) を対象としている場合、ビジュアルとそのすべての子孫がこの光源を認識して応答します。 これは、ツリーのルートで PointLight source を設定することによって単純なものにすることができ、その下のすべてのビジュアルはポイントライトの方向のアニメーションに反応します。

**ExclusionsFromTargets** を使用すると、ターゲットを追加する場合と同様の方法で、ビジュアルまたはビジュアルのサブツリーの光源を削除できます。 除外されているビジュアルをルートとするツリー内の子は、結果として点灯しません。

### <a name="sample-targets"></a>サンプル (ターゲット)

次のサンプルでは、CompositionPointLight を使用して XAML TextBlock をターゲットにしています。

```cs
    _pointLight = _compositor.CreatePointLight();
    _pointLight.Color = Colors.White;
    _pointLight.CoordinateSpace = text; //set up co-ordinate space for offset
    _pointLight.Targets.Add(text); //target XAML TextBlock
```

ポイントライトのオフセットにアニメーションを追加することで、shimmering 効果を簡単に実現できます。

```cs
_pointLight.Offset = new Vector3(-(float)TextBlock.ActualWidth, (float)TextBlock.ActualHeight / 2, (float)TextBlock.FontSize);
```

詳細については、WindowUIDevLabs サンプルゲラにある complete [Text Shimmer](https://github.com/microsoft/WindowsCompositionSamples/tree/master/SampleGallery/Samples/SDK 14393/TextShimmer) サンプルを参照してください。

## <a name="restrictions"></a>制限事項

CompositionLight によってどのコンテンツがどのようになるかを決定する際に考慮する必要がある要素がいくつかあります。

概念 | 詳細
--- | ---
**環境光** | シーンに非アンビエント光を追加すると、既存のすべてのライトがオフになります。  非アンビエント光の対象になっていない項目は黒で表示されます。  光の対象になっていない周囲のビジュアルを自然な方法で点灯させるには、他のライトと組み合わせてアンビエントライトを使用します。
**ライトの数** | UI をターゲットにするには、任意の組み合わせで任意の2つの非アンビエントコンポジションライトを使用できます。 アンビエントライトは制限されていません。スポットライト、ポイントライト、遠くのライトがあります。
**有効期間** | CompositionLight では、有効期間の条件が発生する可能性があります (例: ガベージコレクターは、使用前に光源オブジェクトをリサイクルする場合があります)。  アプリケーションの有効期間を管理するために、メンバーとしてライトを追加することによって、ライトへの参照を保持することをお勧めします。
**変換** | ライトは、ビジュアル構造内の [パースペクティブ変換](../design/layout/3-d-perspective-effects.md) のような効果を使用して適切に描画されるように、UI の上のノードに配置する必要があります。
**ターゲットと座標空間** | CoordinateSpace は、すべてのライトプロパティを設定する必要がある視覚空間です。 CompositionLight は CoordinateSpace ツリー内にある必要があります。

## <a name="lighting-properties"></a>光源のプロパティ

使用されるライトの種類に応じて、光には減衰と領域のプロパティを含めることができます。 すべての種類の光源がすべてのプロパティを使うわけではありません。

プロパティ | 説明
--- | ---
**色** | ライトの [色](/uwp/api/windows.ui.color) 。 光源の色の値は、出力される色を定義する [D3D](../graphics-concepts/light-properties.md) 拡散、アンビエント、およびスペキュラによって定義されます。 光源は、ライトに RGBA 値を使用します。アルファカラーコンポーネントは使用されません。
**方向** | ライトの方向。 光源をポイントする方向は、 [CoordinateSpace](/uwp/api/windows.ui.composition.distantlight.coordinatespace) のビジュアルに対して相対的に指定されます。
**座標空間** | すべてのビジュアルには、3D 座標空間が暗黙的に含まれています。 X 方向は左から右です。 Y 方向は、上から下に向かっています。 Z 方向は、平面からのポイントです。 この座標の元の点はビジュアルの左上隅で、単位はデバイスに依存しないピクセル (DIP) です。 この座標で定義されたライトのオフセット。
**内部コーンと外側コーン** | スポットライトは、明るい内部コーンと外部コーンの 2 つの部分を持つ光のコーンを放射します。 コンポジションを使用すると、円錐の内側と外側の角度と色を制御できます。
**Offset** | 座標空間のビジュアルを基準とした光源のオフセット。

> [!NOTE]
> 複数のライトが同じビジュアルにヒットした場合、または薄い色の値が1.0 を超える大きさになった場合は、ライトカラーチャネルが固定されているため、光の色が変化することがあります。

### <a name="advanced-lighting-properties"></a>高度な照明のプロパティ

プロパティ | 説明
--- | ---
**明暗** | ライトの明るさを制御します。
**減衰** | 減衰は、範囲プロパティによって指定された最大距離にいたるまでに光の強さがどのように弱くなっていくかを制御します。  定数、Quadradic、および線形の減衰プロパティを使用できます。

## <a name="getting-started-with-lighting"></a>光源を使用したはじめに

ライトを追加するには、次の一般的な手順に従います。

- ライトを作成して配置します。ライトを作成し、指定した座標空間に配置します。
- ライトするオブジェクトを特定します。関連するビジュアルのターゲットライト。
- Optional個々のオブジェクトがライトにどのように反応するかを定義します。 SceneLightingEffect を EffectBrush と共に使用して、SpriteVisual を表示するための明るい反射をカスタマイズします。 反射の既定値では、ライトソースの CoordinateSpace の子の光源がサポートされます。  SceneLightingEffect を使用して描画されたビジュアルは、そのビジュアルの既定の光源を上書きします。

## <a name="scenelightingeffect"></a>SceneLightingEffect

[SceneLightingEffect](/uwp/api/Windows.UI.Composition.Effects.SceneLightingEffect)は、 [CompositionLight](/uwp/api/windows.ui.composition.compositionlight)の対象となる[SpriteVisual](/uwp/api/Windows.UI.Composition.SpriteVisual)のコンテンツに適用される既定の光源を変更するために使用されます。

[SceneLightingEffect](/uwp/api/Windows.UI.Composition.Effects.SceneLightingEffect) は、多くの場合、素材の作成に使用されます。 SceneLightingEffect は、画像の反射プロパティを有効にしたり、通常の地図で奥行を提供したりするなど、より複雑な処理を行う場合に使用される効果です。 SceneLightingEffect は、スペキュラや拡散量などの光源のプロパティを使用して UI をカスタマイズする機能を提供します。 その他の効果パイプラインを使用して照明効果をさらにカスタマイズできます。これにより、さまざまな照明の反応を個別にブレンドして、コンテンツと共に構成することができます。

> [!NOTE]
> シーンの光源は影を生成しません。これは、2D レンダリングに焦点を絞った効果です。  影を含む実際の照明モデルを含む3D 照明シナリオは考慮されません。


プロパティ | 説明
--- | ---
**法線マップ** | NormalMaps はテクスチャの効果を作成しますが、通常は光をポイントすると明るくなり、通常は点けがあります。 対象となるビジュアルに NormalMap を追加するには、LoadedImageSurface を使用する [CompositionSurfaceBrush](/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush) を使用して、normalmap アセットを読み込みます。
**環境光** | アンビエントプロパティは、ほとんどの場合、カラーリフレクション全体を制御するために使用されます。
**反射** | 反射反射は、オブジェクトにハイライトを作成し、光沢のあるものとして表示します。 反射のレベルと、反射のレベルを制御できます。  これらのプロパティは、shinny 金属や光沢紙などの素材効果を作成するために操作されます。
**拡散光** | 拡散されたリフレクションは、すべての方向に光を scatters ます。
**反射率モデル** | [反射率モデル](/uwp/api/windows.ui.composition.effects.scenelightingeffectreflectancemodel) では、 [Blinn phong](/visualstudio/designers/how-to-create-a-basic-phong-shader) 物理的に基づく Blinn phong 選択できます。  反射の光源を縮小する場合は、[物理的な Blinn] を選択します。

### <a name="sample-scenelightingeffect"></a>サンプル (SceneLightingEffect)

次のサンプルでは、通常のマップを SceneLightingEffect に追加する方法を示します。

```cs
CompositionBrush CreateNormalMapBrush(ICompositionSurface normalMapImage)
{
    var colorSourceEffect = new ColorSourceEffect()
    {
        Color = Colors.White
    };
    var sceneLightingEffect = new SceneLightingEffect()
    {
        NormalMapSource = new CompositionEffectSourceParameter("NormalMap")
    };

    var compositeEffect = new ArithmeticCompositeEffect()
    {
        Source1 = colorSourceEffect,
        Source2 = sceneLightingEffect,
    };

    var factory = _compositor.CreateEffectFactory(sceneLightingEffect);

    var normalMapBrush = _compositor.CreateSurfaceBrush();
    normalMapBrush.Surface = normalMapImage;
    normalMapBrush.Stretch = CompositionStretch.Fill;

    var brush = factory.CreateBrush();
    brush.SetSourceParameter("NormalMap", normalMapBrush);

    return brush;
}
```

## <a name="related-articles"></a>関連記事

- [ビジュアルレイヤーでの素材とライトの作成](https://blogs.windows.com/buildingapps/2017/08/04/creating-materials-lights-visual-layer/)
- [照明の概要](../graphics-concepts/lighting-overview.md)
- [CompositionCapabilities API](/uwp/api/windows.ui.composition.compositioncapabilities)
- [照明の数学](../graphics-concepts/mathematics-of-lighting.md)
- [SceneLightingEffect](/uwp/api/windows.ui.composition.effects.scenelightingeffect)
- [WindowsUIDevLabs GitHub リポジトリ](https://github.com/microsoft/WindowsCompositionSamples)