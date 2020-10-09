---
description: UI 内で入力、終了、または移動するオブジェクトの動きを自然にするためのタイミングとイージングの重要性について説明します。
title: タイミングとイージング
label: Timing and easing
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: jeffarn
doc-status: Draft
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 336b49c999d135908fb54490b7b33057d4527079
ms.sourcegitcommit: d786d084dafee5da0268ebb51cead1d8acb9b13e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/09/2020
ms.locfileid: "91860153"
---
# <a name="timing-and-easing"></a>タイミングとイージング

モーションが現実世界に基づいている場合、私たちは速度とパフォーマンスへの期待を伴うデジタル メディアでもあります。

## <a name="examples"></a>例

<table>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML コントロールギャラリー</strong>アプリがインストールされている場合は、ここをクリックして<a href="xamlcontrolsgallery:/item/EasingFunction">アプリを開き、イージング関数の動作を確認</a>します。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML コントロール ギャラリー アプリを入手する (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">ソース コード (GitHub) を入手する</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="how-fluent-motion-uses-time"></a>Fluent モーションが時間を使用する方法

タイミングは、UI 内で出入りしたり、移動したりするオブジェクトのモーションを自然に感じさせる重要な要素です。

1. ビューに入るオブジェクトまたはシーンはあっという間ですが、美しく彩ります。 これらのアニメーションは、通常、シーンの階層的なビルドアップを可能にするために、シーンから出るときより継続時間が長くなります。
1. ビューを出るオブジェクトまたはシーンは、非常にあっという間です。 ユーザーは、UI がどこに行ったかを理解できる必要があります。 ただし、UI が閉じたら、それが邪魔にならないようにする必要があります。
1. シーンを移動するオブジェクトは、移動距離に適した時間が必要です。

## <a name="timing-in-fluent-motion"></a>Fluent モーションのタイミング

Fluent のモーションのタイミングでは、ベースラインとして 500 ミリ秒 (0.5 秒) を使用します。これはユーザーが瞬間に認識する最長時間です。

![垂直方向に積み重ねられた3つの円を表示する短いビデオ。速度は150ミリ秒、300ミリ秒、500ミリ秒です。](images/time.gif)

### <a name="150ms-exit"></a>**150ミリ秒** (終了)

:::row:::
    :::column:::
シーンを終了または終了しているオブジェクトまたはページに対して使用します。
終了する UI の非常に速い方向性フィードバックを可能にします。ここでは、スムーズなアニメーションを実現するためにタイミングがフレーム レートを妨げることはありません。
    :::column-end:::
    :::column:::
        ![150ミリ秒モーション](images/150msAlt.gif)
    :::column-end:::
:::row-end:::

### <a name="300ms-enter"></a>**300 ミリ秒** (入力)

:::row:::
    :::column:::
シーンに入ったり開いたりするオブジェクトまたはページに対して使用します。
シーンに入るときにコンテンツを彩るために適切な時間が確保されます。
    :::column-end:::
    :::column:::
        ![300 ミリ秒のモーション](images/300ms.gif)
    :::column-end:::
:::row-end:::

### <a name="500ms-move"></a>**≤500 ミリ秒** (移動)

:::row:::
    :::column:::
1つのシーンまたは複数のシーンで平行移動するオブジェクトに対して使用します。 
    :::column-end:::
    :::column:::
        ![500ミリ秒モーション](images/500ms.gif)
    :::column-end:::
:::row-end:::

## <a name="easing-in-fluent-motion"></a>Fluent モーションのイージング

イージングは、移動時のオブジェクトの速度を操作する方法です。 Fluent モーションのすべてのエクスペリエンスを結び付ける接着剤です。 極端ですが、システムで使用されるイージングは、システム内で移動するオブジェクトの物理的な操作感を統合することができます。 これは、現実世界を模倣し、移動中のオブジェクトがその環境に属しているように感じさせる 1 つの方法です。

![フレームの右下隅から、フレームの左上隅近くにある円を示す短いビデオが表示されます。](images/easing.gif)

## <a name="apply-easing-to-motion"></a>モーションへのイージングの適用

これらのイージングは、より自然な操作感を得るのに役立ち、Fluent モーションのために使用するベースラインとなります。

コード例では、推奨されるイージング値を Storyboard アニメーション (XAML) または Composition アニメーション (C#) に適用する方法を示しています。

### <a name="accelerate-exit"></a>**高速化**(終了)

:::row:::
    :::column:::
シーンを終了する UI またはオブジェクトに対して使用します。

オブジェクトは、エスケープ速度に達するまで電力を供給し、勢いを獲得します。
結果として得られる感覚は、オブジェクトがユーザーのやり方にとって最も難しく、新しいコンテンツのためのスペースを作ることです。
    :::column-end:::
    :::column:::
        ![イージングを加速する](images/accelEase.gif)
    :::column-end:::
:::row-end:::

```
cubic-bezier(0.7 , 0 , 1 , 0.5)
```

```xaml
<!-- Use for XAML Storyboard animations. -->
<Storyboard x:Name="Storyboard">
    <DoubleAnimation Storyboard.TargetName="Translation" Storyboard.TargetProperty="X" From="0" To="200" Duration="0:0:0.15">
        <DoubleAnimation.EasingFunction>
            <ExponentialEase Exponent="4.5" EasingMode="EaseIn" />
        </DoubleAnimation.EasingFunction>
    </DoubleAnimation>
</Storyboard>
```

```csharp
// Use for Composition animations.
CubicBezierEasingFunction accelerate =
    _compositor.CreateCubicBezierEasingFunction(new Vector2(0.7f, 0.0f), new Vector2(1.0f, 0.5f));
_exitAnimation = _compositor.CreateScalarKeyFrameAnimation();
_exitAnimation.InsertKeyFrame(0.0f, _startValue);
_exitAnimation.InsertKeyFrame(1.0f, _endValue, accelerate);
_exitAnimation.Duration = TimeSpan.FromMilliseconds(150);
```

### <a name="decelerate-enter"></a>**減速**(入力)

:::row:::
    :::column:::
シーンを開始するオブジェクトまたは UI に使用します。移動または生成します。

シーンを使用すると、オブジェクトは極端な摩擦で満たされ、オブジェクトの残りの速度が低下します。
結果として得られる感覚は、オブジェクトが長い距離から移動し、極端な速度で入力されたか、またはすぐに rest 状態に戻ることです。

無応答の前にある場合でも、受信オブジェクトの速度は、速度と応答性が低下します。
    :::column-end:::
    :::column:::
        ![イージングを減速する](images/decelEase.gif)
    :::column-end:::
:::row-end:::

```
cubic-bezier(0.1 , 0.9 , 0.2 , 1)
```

```xaml
<!-- Use for XAML Storyboard animations. -->
<Storyboard x:Name="Storyboard">
    <DoubleAnimation Storyboard.TargetName="Translation" Storyboard.TargetProperty="X" From="0" To="200" Duration="0:0:0.3">
        <DoubleAnimation.EasingFunction>
            <ExponentialEase Exponent="7" EasingMode="EaseOut" />
        </DoubleAnimation.EasingFunction>
    </DoubleAnimation>
</Storyboard>
```

```csharp
// Use for Composition animations.
CubicBezierEasingFunction decelerate =
    _compositor.CreateCubicBezierEasingFunction(new Vector2(0.1f, 0.9f), new Vector2(0.2f, 1.0f));
_enterAnimation = _compositor.CreateScalarKeyFrameAnimation();
_enterAnimation.InsertKeyFrame(0.0f, _startValue);
_enterAnimation.InsertKeyFrame(1.0f, _endValue, decelerate);
_enterAnimation.Duration = TimeSpan.FromMilliseconds(300);
```

### <a name="standard-easing-move"></a>**標準的なイージング**(移動)

:::row:::
    :::column:::
これは、システム内でアニメーション化されたパラメーターを変更するための、基準となるイージングです。
単純な位置変更など、画面上で状態ごとに変化するオブジェクトに標準的なイージングを使用します。 また、拡大するオブジェクトのように、シーン内でモーフィングするオブジェクトに使用します。

結果として得られる感覚は、状態を A から B に変更するオブジェクトが、自然な力によって解決されていることです。
    :::column-end:::
    :::column:::
        ![標準のイージング](images/standardEase.gif)
    :::column-end:::
:::row-end:::

```
cubic-bezier(0.8 , 0 , 0.2 , 1)
```

```xaml
<!-- Use for XAML Storyboard animations. -->
<Storyboard x:Name="Storyboard">
    <DoubleAnimation Storyboard.TargetName="Translation" Storyboard.TargetProperty="X" From="0" To="200" Duration="0:0:0.5">
        <DoubleAnimation.EasingFunction>
            <CircleEase EasingMode="EaseInOut" />
        </DoubleAnimation.EasingFunction>
    </DoubleAnimation>
</Storyboard>
```

```csharp
// Use for Composition animations.
CubicBezierEasingFunction standard =
    _compositor.CreateCubicBezierEasingFunction(new Vector2(0.8f, 0.0f), new Vector2(0.2f, 1.0f));
 _moveAnimation = _compositor.CreateScalarKeyFrameAnimation();
 _moveAnimation.InsertKeyFrame(0.0f, _startValue);
 _moveAnimation.InsertKeyFrame(1.0f, _endValue, standard);
 _moveAnimation.Duration = TimeSpan.FromMilliseconds(500);
```

## <a name="related-articles"></a>関連記事

- [モーションの概要](index.md)
- [方向性と重力](directionality-and-gravity.md)
