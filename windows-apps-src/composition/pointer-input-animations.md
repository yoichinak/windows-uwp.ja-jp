---
title: ポインター ベース アニメーション
description: ポインターの位置を使用して、動的な "カーソル追跡" エクスペリエンスを作成する方法ついて説明します。
ms.date: 03/23/2021
ms.topic: article
keywords: Windows 10, UWP, アニメーション
ms.localizationpriority: medium
ms.openlocfilehash: 80fd46af6f84d4a23329ad34fc77a14faf97b913
ms.sourcegitcommit: 34f532fd023af2849c3e975baf7aa6771d7e53b9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/23/2021
ms.locfileid: "104893862"
---
# <a name="pointer-based-animations"></a>ポインター ベース アニメーション

この記事では、ポインターの位置を使用して、動的な "カーソル追跡" エクスペリエンスを作成する方法ついて説明します。

## <a name="prerequisites"></a>前提条件

ここでは、以下の記事で説明されている概念を理解していることを前提とします。

- [入力駆動型アニメーション](input-driven-animations.md)
- [関係ベース アニメーション](relation-animations.md)

## <a name="why-create-pointer-position-driven-experiences"></a>ポインターの位置で駆動するエクスペリエンスを作成する理由

[Fluent デザイン言語](/windows/apps/fluent-design-system)では、UI と対話する唯一の方法ではありません。 UWP では複数のデバイス フォーム ファクターを対象としているため、エンド ユーザーは、マウスやペンなど、他の入力方式を使用してアプリとやり取りする場合もあります。 こうしたその他の入力方式から取得される位置データを利用することで、アプリに対するエンド ユーザーの使用率を高めることができます。

ポインターの位置で駆動するエクスペリエンスによって、ポインター入力方式での画面上の位置を活用して、アプリに対して追加のモーションや UI エクスペリエンスを作成することができます。 多くの場合、こうしたエクスペリエンスでは、UI の動作と構造に関する追加のコンテキストやフィードバックが、エンド ユーザーに提供されます。 現在ではエクスペリエンスは一方向のストリームではなく、双方向のストリームになりつつあります。双方向のストリームとは、エンド ユーザーが入力方式に基づいて入力を提供し、アプリの UI はその入力に応答するといったストリームです。

次に例をいくつか示します。

- カーソルの後に移動する [スポットライト](/uwp/api/windows.ui.composition.spotlight) の位置をアニメーション化する

    ![ポインター スポットライトの例](images/animation/spotlight-reveal.gif)

- ポインターの位置に基づいて画像を回転させる

    ![ポインターに基づく回転の例](images/animation/pointer-rotate.gif)

## <a name="using-pointerpositionpropertyset"></a>PointerPositionPropertySet の使用

このようなエクスペリエンスは、PointerPositionPropertySet を使用して作成することができます。 この PropertySet が作成されると、UIElement は、UIElement のヒット テストが適切に行われているときにポインターの位置を保持することができます。 位置の値は、UIElement の座標空間を基準とした相対的な値です (位置 <0,0> は UIElement の左上隅を表します)。 このエクスペリエンスが作成されると、このプロパティをアニメーションで利用して、別のプロパティのモーションを駆動することができます。

それぞれのポインター入力方式には、入力時の位置の変化 (ホバー、押下、押下して移動) に応じてさまざまな入力状態があります。 PointerPositionPropertySet は、マウスとペンのホバー時、押された状態、および移動された状態にあるポインターの位置だけを保持します。

作業を始める際の一般的な手順:

1. 追跡するポインターの位置を保持する UIElement を指定します。
1. ElementCompositionPreview を使用して PointerPositionPropertySet にアクセスします。
    - [ElementCompositionPreview](/uwp/api/windows.ui.xaml.hosting.elementcompositionpreview.getpointerpositionpropertyset)メソッドに UIElement を渡します。
1. PropertySet の Position プロパティを参照する ExpressionAnimation を作成します。
    - 参照パラメーターを忘れずに設定してください。
1. CompositionObject のプロパティを、式のアニメーションでターゲットにします。

> [!NOTE]
> GetPointerPositionPropertySet メソッドから返された PropertySet をクラス変数に割り当てることをお勧めします。 これにより、プロパティ セットがガベージ コレクションによってクリーンアップされなくなります。そのため、参照されている ExpressionAnimation は影響を受けません。 ExpressionAnimations は、式で使用されているどのオブジェクトに対しても強参照を保持しません。

## <a name="example"></a>例

マウスとペン入力モダリティのホバー位置を利用して、画像を動的に回転させる例を見てみましょう。

![ポインターに基づく回転の例](images/animation/pointer-rotate.gif)

このイメージは UIElement であるため、まず PointerPositionPropertySet への参照を取得しましょう。

```csharp
_pointerPositionPropSet = ElementCompositionPreview.GetPointerPositionPropertySet(UIElement element);
```

この例では、2 つの式を使用します。

- 画像の中心から離れた位置にあるポインターに基づいて画像を回転させる式。 中心から離れるに従って、回転が大きくなります。
- ポインターの位置に基づいて回転軸を変化させる式。 位置のベクトル対して垂直となる回転軸が必要です。

ここでは、2つの式を定義します。1つは RotationAngle プロパティを対象とし、もう1つは Matrix.rotationaxis プロパティを対象とします。 他の PropertySet と同様に、PointerPositionPropertySet を参照します。

この例では、式 [ビルダー](/windows/communitytoolkit/animations/expressions) クラスを使用して式を作成しています。

```csharp
// using EF = ExpressionBuilder.ExpressionFunctions;
// || DEFINE THE EXPRESSION FOR THE ROTATION ANGLE ||
var hoverPosition = _pointerPositionPropSet.GetSpecializedReference
<PointerPositionPropertySetReferenceNode>().Position;
var angleExpressionNode =
EF.Conditional(
 hoverPosition == new Vector3(0, 0, 0),
 ExpressionValues.CurrentValue.CreateScalarCurrentValue(),
 35 * ((EF.Clamp(EF.Distance(center, hoverPosition), 0, distanceToCenter) % distanceToCenter) / distanceToCenter));
_tiltVisual.StartAnimation("RotationAngleInDegrees", angleExpressionNode);

// || DEFINE THE EXPRESSION FOR THE ROTATION AXIS ||
var axisAngleExpressionNode = EF.Vector3(
-(hoverPosition.Y - center.Y) * EF.Conditional(hoverPosition.Y == center.Y, 0, 1),
 (hoverPosition.X - center.X) * EF.Conditional(hoverPosition.X == center.X, 0, 1),
 0);
_tiltVisual.StartAnimation("RotationAxis", axisAngleExpressionNode);
```

## <a name="see-also"></a>関連項目

- [ポインターの回転のサンプル](https://github.com/microsoft/WindowsCompositionSamples/tree/master/SampleGallery/Samples/SDK%2015063/PointerRotate)
- [ポインター Positionpropertysetreferencのクラス](/dotnet/api/microsoft.toolkit.uwp.ui.animations.expressions.pointerpositionpropertysetreferencenode?view=win-comm-toolkit-dotnet-7.0&preserve-view=true)