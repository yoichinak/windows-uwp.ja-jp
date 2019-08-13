---
title: XAML プロパティのアニメーション
description: 合成アニメーションを XAML 要素をアニメーション化します。
ms.date: 09/13/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: jeffarn
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 183a5433553ff6fdfcb09f6960f6a642f2c8bc08
ms.sourcegitcommit: cc0ef75f314658b14376eb60ef8e5bb4d7726e04
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/08/2019
ms.locfileid: "65444155"
---
# <a name="animating-xaml-elements-with-composition-animations"></a>合成アニメーションを XAML 要素をアニメーション化

この記事では、合成アニメーションのパフォーマンスと XAML のプロパティの設定の容易さと XAML UIElement をアニメーション化できる新しいプロパティについて説明します。

Windows 10、バージョンは 1809、以前は、UWP アプリでのアニメーションを作成する 2 つの選択肢がありました。

- などの XAML の構成要素を使用して、[アニメーションを再検討](storyboarded-animations.md)、または _* ThemeTransition_と _* ThemeAnimation_クラス、 [Windows.UI.Xaml.Media.Animation](/uwp/api/windows.ui.xaml.media.animation)名前空間。
- -「[XAML でのビジュアル レイヤーの使用](../../composition/using-the-visual-layer-with-xaml.md)」の説明に従って、合成アニメーションを使用する。

ビジュアル レイヤーを使用すると、XAML コンストラクトを使用した場合よりパフォーマンスが向上します。 ただし、[ElementCompositionPreview](/uwp/api/Windows.UI.Xaml.Hosting.ElementCompositionPreview) を使用して要素の基になるコンポジションの [Visual](/uwp/api/windows.ui.composition.visual) オブジェクトを取得してから、合成アニメーションでビジュアルをアニメーション化する方が、使用が複雑になります。

Windows 10、バージョンは 1809、以降のプロパティを基になる合成ビジュアルを取得する必要がなく、合成アニメーションを使用して直接 UIElement をアニメーション化できます。

> [!NOTE]
> UIElement にこれらのプロパティを使用するには、UWP プロジェクトのターゲット バージョンは 1809 以降にする必要があります。 プロジェクトのバージョンを構成する方法の詳細については、次を参照してください。[バージョン アダプティブ アプリ](../../debug-test-perf/version-adaptive-apps.md)します。

## <a name="examples"></a>例

<table>
<th align="left">XAML コントロール ギャラリー<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>ある場合、 <strong style="font-weight: semi-bold">XAML コントロール ギャラリー</strong>アプリをインストールするには、ここをクリックして<a href="xamlcontrolsgallery:/item/XamlCompInterop">アプリを開き、アニメーションの相互運用機能の動作を参照してください。</a>します。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML コントロール ギャラリー アプリを入手する (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">ソース コード (GitHub) を入手する</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="new-rendering-properties-replace-old-rendering-properties"></a>新しいレンダリング プロパティは、古いレンダリング プロパティを置き換えます

この表でアニメーション化もできる、UIElement のレンダリングを変更に使用できるプロパティを示します、 [CompositionAnimation](/uwp/api/windows.ui.composition.compositionanimation)します。

| プロパティ | 種類 | 説明 |
| -- | -- | -- |
| [不透明度](/uwp/api/windows.ui.xaml.uielement.opacity) | Double | オブジェクトの不透明度 |
| [翻訳](/uwp/api/windows.ui.xaml.uielement.translation) | Vector3 | 要素の X、Y、/Z 位置をシフトします。 |
| [TransformMatrix](/uwp/api/windows.ui.xaml.uielement.transformmatrix) | Matrix4x4 | 要素に適用する変換行列 |
| [スケール](/uwp/api/windows.ui.xaml.uielement.scale) | Vector3 | スケール、要素中心点の中央に配置 |
| [Rotation](/uwp/api/windows.ui.xaml.uielement.rotation) | Float | RotationAxis と CenterPoint の周りの要素を回転させる |
| [RotationAxis](/uwp/api/windows.ui.xaml.uielement.rotationaxis) | Vector3 | 回転の軸 |
| [CenterPoint](/uwp/api/windows.ui.xaml.uielement.centerpoint) | Vector3 | 拡大縮小や回転の中心点 |

TransformMatrix プロパティの値は、次の順序でスケール、回転、および変換のプロパティと組み合わせます。Scale、Rotation、Translation と平行移動します。

これらのプロパティは、要素のレイアウトに影響はありません、ためこれらのプロパティを変更するは発生しません新しい[メジャー](/uwp/api/windows.ui.xaml.uielement.measure)/[配置](/uwp/api/windows.ui.xaml.uielement.arrange)渡します。

これらのプロパティとしてコンポジションに似た名前の付いたプロパティの目的と動作が同じである[Visual](/uwp/api/windows.ui.composition.visual)クラス (を除く翻訳は、ビジュアルではありません)。

### <a name="example-setting-the-scale-property"></a>以下に例を示します。スケールのプロパティの設定

この例では、ボタンのスケールのプロパティを設定する方法を示します。

```xaml
<Button Scale="2,2,1" Content="I am a large button" />
```

```csharp
var button = new Button();
button.Content = "I am a large button";
button.Scale = new Vector3(2.0f,2.0f,1.0f);
```

### <a name="mutual-exclusivity-between-new-and-old-properties"></a>新規および既存のプロパティの間で相互に排他的

> [!NOTE]
> **Opacity** プロパティでは、このセクションで説明されている相互排他性は強制されません。 XAML または合成アニメーションを使用するかどうかに関係なく、同じ Opacity プロパティを使用します。

CompositionAnimation をアニメーション化できるプロパティは、いくつかの既存の UIElement プロパティの置き換えです。

- [RenderTransform](/uwp/api/windows.ui.xaml.uielement.rendertransform)
- [RenderTransformOrigin](/uwp/api/windows.ui.xaml.uielement.rendertransformorigin)
- [Projection](/uwp/api/windows.ui.xaml.uielement.projection)
- [Transform3D](/uwp/api/windows.ui.xaml.uielement.transform3d)

または設定すると (アニメーション化する)、新しいプロパティのいずれか、古いプロパティを使用することはできません。 逆に、設定 (またはアニメーション化する) 場合、古いプロパティのいずれかは、新しいプロパティを使用することはできません。

使用することもできない新しいプロパティを取得し、これらのメソッドを使用して自分でビジュアルを管理する ElementCompositionPreview を使用する場合。

- [ElementCompositionPreview.GetElementVisual](/uwp/api/windows.ui.xaml.hosting.elementcompositionpreview.getelementvisual)
- [ElementCompositionPreview.SetIsTranslationEnabled](/uwp/api/windows.ui.xaml.hosting.elementcompositionpreview.setistranslationenabled)

> [!IMPORTANT]
> 2 つのプロパティのセットを併用しようとして失敗し、エラー メッセージを生成するには、API 呼び出しになります。

わかりやすくするためにはお勧めしませんが、オフにすると、プロパティの 1 つのセットから切り替えるになります。 プロパティは、DependencyProperty でバックアップされている場合 (たとえば、UIElement.Projection 支え UIElement.ProjectionProperty)、「未使用」の状態に復元する ClearValue を呼び出します。 (たとえば、スケールのプロパティ)、それ以外の場合は、既定値に、プロパティを設定します。

## <a name="animating-uielement-properties-with-compositionanimation"></a>CompositionAnimation の UIElement のプロパティをアニメーション化

CompositionAnimation での表に表示プロパティをアニメーション化することができます。 これらのプロパティを参照することも、 [ExpressionAnimation](/uwp/api/windows.ui.composition.expressionanimation)します。

使用して、 [StartAnimation](/uwp/api/windows.ui.xaml.uielement.startanimation)と[StopAnimation](/uwp/api/windows.ui.xaml.uielement.stopanimation) UIElement UIElement プロパティをアニメーション化するメソッド。

### <a name="example-animating-the-scale-property-with-a-vector3keyframeanimation"></a>以下に例を示します。Vector3KeyFrameAnimation とスケールのプロパティをアニメーション化

この例では、ボタンの小数点以下桁数をアニメーション化する方法を示します。

```csharp
var compositor = Window.Current.Compositor;
var animation = compositor.CreateVector3KeyFrameAnimation();

animation.InsertKeyFrame(1.0f, new Vector3(2.0f,2.0f,1.0f));
animation.Duration = TimeSpan.FromSeconds(1);
animation.Target = "Scale";

button.StartAnimation(animation);
```

### <a name="example-animating-the-scale-property-with-an-expressionanimation"></a>以下に例を示します。ExpressionAnimation とスケールのプロパティをアニメーション化

ページには、2 つのボタンがあります。 2 番目のボタンを 2 倍に (スケール) を使用してアニメーションの最初のボタンとして。

```xaml
<Button x:Name="sourceButton" Content="Source"/>
<Button x:Name="destinationButton" Content="Destination"/>
```

```csharp
var compositor = Window.Current.Compositor;
var animation = compositor.CreateExpressionAnimation("sourceButton.Scale*2");
animation.SetExpressionReferenceParameter("sourceButton", sourceButton);
animation.Target = "Scale";
destinationButton.StartAnimation(animation);
```

## <a name="related-topics"></a>関連トピック

- [ストーリーボードに設定されたアニメーション](storyboarded-animations.md)
- [XAML でのビジュアル レイヤーの使用](../../composition/using-the-visual-layer-with-xaml.md)
- [変換の概要](../layout/transforms.md)
