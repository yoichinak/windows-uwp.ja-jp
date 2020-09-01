---
title: XAML プロパティアニメーション
description: ユニバーサル Windows プラットフォーム (UWP) コンポジションアニメーションを使用して、XAML UIElement のプロパティを直接アニメーション化する方法について説明します。
ms.date: 09/13/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: jeffarn
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 0a7ab119df407b45fb391aebf50df5d85f89ec0f
ms.sourcegitcommit: e273e5901bfa6596dfef4cc741bb1c42614c25ab
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/01/2020
ms.locfileid: "89238297"
---
# <a name="animating-xaml-elements-with-composition-animations"></a>アニメーション化 (アニメーションを使用して XAML 要素を)

この記事では、合成アニメーションのパフォーマンスと xaml プロパティの簡単な設定を使用して XAML UIElement をアニメーション化できる新しいプロパティについて説明します。

Windows 10 バージョン1809より前では、UWP アプリでアニメーションを作成するには2つの選択肢がありました。

- [Storyboarded アニメーション](storyboarded-animations.md)などの XAML コンストラクト、または ThemeTransition[名前空間の](/uwp/api/windows.ui.xaml.media.animation) _*_ クラスと _* ThemeAnimation_クラスを使用します。
- 「 [XAML でのビジュアルレイヤーの使用](../../composition/using-the-visual-layer-with-xaml.md)」で説明されているように、コンポジションアニメーションを使用します。

ビジュアルレイヤーを使用すると、XAML コンストラクトを使用する場合よりも優れたパフォーマンスが得られます。 しかし、 [ElementCompositionPreview](/uwp/api/Windows.UI.Xaml.Hosting.ElementCompositionPreview) を使用して要素の基になる合成 [ビジュアル](/uwp/api/windows.ui.composition.visual) オブジェクトを取得し、コンポジションアニメーションを使用してビジュアルをアニメーション化する方がより複雑になります。

Windows 10 バージョン1809以降では、基になるコンポジションビジュアルを取得する必要がなく、合成アニメーションを使用して、UIElement のプロパティを直接アニメーション化できます。

> [!NOTE]
> これらのプロパティを UIElement で使用するには、UWP プロジェクトのターゲットバージョンが1809以降である必要があります。 プロジェクトバージョンの構成の詳細については、「 [バージョンアダプティブアプリ](../../debug-test-perf/version-adaptive-apps.md)」を参照してください。

## <a name="examples"></a>例

<table>
<th align="left">XAML コントロール ギャラリー<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML コントロールギャラリー</strong>アプリがインストールされている場合は、ここをクリックして<a href="xamlcontrolsgallery:/item/XamlCompInterop">アプリを開き、アニメーションの相互運用に</a>関するページを参照してください。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML コントロール ギャラリー アプリを入手する (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">ソース コード (GitHub) を入手する</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="new-rendering-properties-replace-old-rendering-properties"></a>新しい表示プロパティが古い表示プロパティに置き換わる

次の表は、UIElement のレンダリングを変更するために使用できるプロパティを示しています。これは、 [CompositionAnimation](/uwp/api/windows.ui.composition.compositionanimation)を使用してアニメーション化することもできます。

| プロパティ | Type | 説明 |
| -- | -- | -- |
| [Opacity](/uwp/api/windows.ui.xaml.uielement.opacity) | Double | オブジェクトの不透明度 |
| [翻訳](/uwp/api/windows.ui.xaml.uielement.translation) | Vector3 | 要素の X/Y/Z 位置をシフトします |
| [TransformMatrix](/uwp/api/windows.ui.xaml.uielement.transformmatrix) | Matrix4x4 | 要素に適用する変換行列。 |
| [スケール](/uwp/api/windows.ui.xaml.uielement.scale) | Vector3 | 小惑星の中央に要素をスケーリングします。 |
| [回転](/uwp/api/windows.ui.xaml.uielement.rotation) | Float | 要素を Matrix.rotationaxis と小惑星の周りで回転させます |
| [RotationAxis](/uwp/api/windows.ui.xaml.uielement.rotationaxis) | Vector3 | 回転の軸 |
| [CenterPoint](/uwp/api/windows.ui.xaml.uielement.centerpoint) | Vector3 | スケールと回転の中心点 |

TransformMatrix プロパティ値は、次の順序でスケール、回転、および翻訳のプロパティと組み合わせて使用されます。 TransformMatrix、Scale、Rotation、Translation。

これらのプロパティは要素のレイアウトに影響しないため、これらのプロパティを変更しても、新しい[メジャー](/uwp/api/windows.ui.xaml.uielement.measure) / [配置](/uwp/api/windows.ui.xaml.uielement.arrange)パスは発生しません。

これらのプロパティの目的と動作は、合成 [ビジュアル](/uwp/api/windows.ui.composition.visual) クラスの似た名前のプロパティと同じです (ただし、ビジュアルではない平行移動は除きます)。

### <a name="example-setting-the-scale-property"></a>例: Scale プロパティの設定

この例では、ボタンの Scale プロパティを設定する方法を示します。

```xaml
<Button Scale="2,2,1" Content="I am a large button" />
```

```csharp
var button = new Button();
button.Content = "I am a large button";
button.Scale = new Vector3(2.0f,2.0f,1.0f);
```

### <a name="mutual-exclusivity-between-new-and-old-properties"></a>New プロパティと old プロパティ間の相互排他性

> [!NOTE]
> **Opacity**プロパティでは、このセクションで説明する相互排他性は適用されません。 XAML またはコンポジションアニメーションを使用しているかどうかにかかわらず、同じ Opacity プロパティを使用します。

CompositionAnimation を使用してアニメーション化できるプロパティは、いくつかの既存の UIElement プロパティの置換です。

- [RenderTransform](/uwp/api/windows.ui.xaml.uielement.rendertransform)
- [System.windows.uielement.rendertransformorigin](/uwp/api/windows.ui.xaml.uielement.rendertransformorigin)
- [射影](/uwp/api/windows.ui.xaml.uielement.projection)
- [Transform3D](/uwp/api/windows.ui.xaml.uielement.transform3d)

新しいプロパティを設定 (またはアニメーション化) するときに、古いプロパティを使用することはできません。 逆に、古いプロパティを設定 (またはアニメーション化) した場合、新しいプロパティを使用することはできません。

ElementCompositionPreview を使用して、これらのメソッドを使用して自分でビジュアルを取得して管理する場合は、新しいプロパティを使用することもできません。

- [ElementCompositionPreview.GetElementVisual](/uwp/api/windows.ui.xaml.hosting.elementcompositionpreview.getelementvisual)
- [ElementCompositionPreview.SetIsTranslationEnabled](/uwp/api/windows.ui.xaml.hosting.elementcompositionpreview.setistranslationenabled)

> [!IMPORTANT]
> 2つのプロパティセットの使用を混在させようとすると、API 呼び出しが失敗し、エラーメッセージが生成されます。

プロパティをクリアすることで、1つのプロパティセットから切り替えることができます。ただし、わかりやすくするためにはお勧めしません。 プロパティが DependencyProperty によってサポートされている場合 (たとえば、UIElement で ProjectionProperty がサポートされている場合)、ClearValue を呼び出して "未使用" 状態に復元します。 それ以外 (たとえば、Scale プロパティ) の場合は、プロパティを既定値に設定します。

## <a name="animating-uielement-properties-with-compositionanimation"></a>CompositionAnimation を使用した UIElement プロパティのアニメーション化

CompositionAnimation を使用して、表に示されている表示プロパティをアニメーション化できます。 これらのプロパティは、 [式のアニメーション](/uwp/api/windows.ui.composition.expressionanimation)によって参照することもできます。

Uielement で [startanimation](/uwp/api/windows.ui.xaml.uielement.startanimation) メソッドと [stopanimation](/uwp/api/windows.ui.xaml.uielement.stopanimation) メソッドを使用して、uielement のプロパティをアニメーション化します。

### <a name="example-animating-the-scale-property-with-a-vector3keyframeanimation"></a>例: Vector3KeyFrameAnimation を使用して Scale プロパティをアニメーション化する

この例では、ボタンのスケールをアニメーション化する方法を示します。

```csharp
var compositor = Window.Current.Compositor;
var animation = compositor.CreateVector3KeyFrameAnimation();

animation.InsertKeyFrame(1.0f, new Vector3(2.0f,2.0f,1.0f));
animation.Duration = TimeSpan.FromSeconds(1);
animation.Target = "Scale";

button.StartAnimation(animation);
```

### <a name="example-animating-the-scale-property-with-an-expressionanimation"></a>例: 式を使用して Scale プロパティをアニメーション化する

ページには2つのボタンがあります。 2番目のボタンは、最初のボタンとして2倍の大きさ (スケールを使用) にアニメーション化されます。

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
