---
description: 目的がはっきりとしており、適切にデザインされたモーションは、アプリに強い印象を与え、精巧で完成度の高い操作性を感じさせます。 コンテキストの変化がわかりやすく、視覚的な切り替えがエクスペリエンスに結び付きます。
title: Windows アプリのモーション
ms.assetid: 21AA1335-765E-433A-85D8-560B340AE966
label: Motion
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: jeffarn
doc-status: Published
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 3850adfcb545bf5f21716cccb4fcda43e9c02efb
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034325"
---
# <a name="motion-for-windows-apps"></a>Windows アプリのモーション

![モーション アイコン](../images/motion-2x.png)

Fluent モーションはアプリで目的を果たします。 これは、ユーザーの動作に基づいてインテリジェントなフィードバックを提供し、UI にアクティブな印象を与え、アプリ内でユーザーのナビゲーションを誘導します。 Fluent モーションは、ユーザーとそのデジタル エクスペリエンス間の感情面の結び付きを生み出します。 マイクロソフトでは、ユーザーが既に現実世界から認識している自然な動きの基盤のうえに構築し、そこからシステムを拡張します。

## <a name="examples"></a>例

<table>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML コントロール ギャラリー</strong> アプリがインストールされている場合、こちらをクリックして<a href="xamlcontrolsgallery:/category/Motion">アプリを開き、モーションの動作を確認</a>してください。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML コントロール ギャラリー アプリを入手する (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">ソース コード (GitHub) を入手する</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="fluent-motion-principles"></a>Fluent モーションの原則

### <a name="physical"></a>運動能力

動作中のオブジェクトは、現実世界でのオブジェクトの動作を示します。 滑らかで応答性の高い動きによって、自然な操作性を感じさせるエクスペリエンスが実現され、感情面の結び付きが生みだされ、個性が追加されます。

![物理モーションの UI の例](images/Physical.gif)
> タッチで UI を操作すると、UI の動きが、操作の速度に直接関連します。 また、タッチは直接操作であるため、操作するオブジェクトが周囲のオブジェクトに影響します。

### <a name="functional"></a>機能性

モーションは目的を果たし、確信があります。 複雑さに関してユーザーをサポートし、階層を確立する手助けをします。 モーションは、パフォーマンスが向上した印象を与え、体感する待ち時間を隠蔽することでユーザー エクスペリエンスを最適化します。

![機能的なモーションの UI の例](images/functional.gif)
> ページ切り替えは、目的に特化されています。 ページが相互に関連する方法に関するヒントを提供します。 パフォーマンスが最適でない場合でも速く感じられるような方法で移動します。

### <a name="continuous"></a>継続

ポイントからポイントへの滑らか動きは、自然に目を引きつけ、ユーザーを誘導します。 適切にユーザーのタスクをまとめて、よりコンシューマブルで親しみやすく感じられるようにします。

![継続的なモーションの UI の例](images/continuous3.gif)
> オブジェクトは、シーン間を移動したり、シーン内でモーフィングして継続性を提供し、ユーザーがコンテキストを維持できるようにします。

### <a name="contextual"></a>状況依存

インテリジェントなモーションは、UI を操作する方法に沿った方法でユーザーにフィードバックを提供します。 操作はユーザーを中心にしています。 動きはフォーム ファクターに適切であると感じられ、シナリオに基づいて設計されています。 各ユーザーにとって快適である必要があります。

![状況依存のモーションの UI の例](images/Contextual.gif)
> アニメーションはユーザーの操作に沿ったものである必要があります。 コンテキスト メニューは、ユーザーがアクティブ化したポイントから展開されます。

## <a name="motion-articles"></a>モーションの記事

:::row:::
    :::column:::
### <a name="timing-and-easing"></a>[タイミングとイージング](timing-and-easing.md)
タイミングとイージングは、UI 内で出入りしたり、移動したりするオブジェクトのモーションを自然に感じさせる重要な要素です。
    :::column-end:::
    :::column:::
### <a name="directionality-and-gravity"></a>[方向性と重力](directionality-and-gravity.md)
方向指示は、ユーザーがエクスペリエンス全体を通じて行う取り組みの強固な概念的モデルを提供するのに役立ちます。 方向性は、重力などのエネルギーの影響を受け、これにより移動の自然な感覚が強化されます。
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
### <a name="page-transitions"></a>[ページ切り替え効果](page-transitions.md)
ページ切り替え効果により、ユーザーがアプリ内のページ間を移動するため、ページ間の関係がフィードバックされます。 ユーザーがナビゲーション階層内の場所を理解するのに役立ちます。
    :::column-end:::
    :::column:::
### <a name="connected-animation"></a>[接続型アニメーション](connected-animation.md)
接続型アニメーションを使用すると、2 つの異なるビューの間で要素が切り替わる様子をアニメーション化することによって、動的で魅力的なナビゲーション エクスペリエンスを作成できます。
    :::column-end:::
:::row-end:::
