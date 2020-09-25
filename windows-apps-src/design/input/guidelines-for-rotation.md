---
Description: このトピックでは、ローテーション用の新しい Windows UI について説明します。また、この新しい対話機構を Windows アプリで使用する際に考慮する必要があるユーザーエクスペリエンスガイドラインについて説明します。
title: 回転
ms.assetid: f098bc05-35b3-46b2-9e9b-9ff292d067ca
label: Rotation
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 135f7773a94491e1e6470c84ad428265273bc79d
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/24/2020
ms.locfileid: "91217005"
---
# <a name="rotation"></a>回転


この記事では、ローテーション用の新しい Windows UI について説明します。また、この新しい対話機構を Windows アプリで使用する際に考慮する必要があるユーザーエクスペリエンスガイドラインについて説明します。

> **重要な API**: [**Windows.UI.Input**](/uwp/api/Windows.UI.Input)、[**Windows.UI.Xaml.Input**](/uwp/api/Windows.UI.Xaml.Input)

## <a name="dos-and-donts"></a>推奨と非推奨

-   ユーザーが直接 UI 要素を回転できるように回転を使います。

## <a name="additional-usage-guidance"></a>その他の使い方のガイダンス


**回転の概要**

ローテーションは、ユーザーがオブジェクトを円形 (時計回りまたは反時計回り) に変換できるようにするために、Windows アプリによって使用されるタッチ最適化された手法です。

入力デバイスに応じて回転操作は次のように実行されます。

-   マウスまたはアクティブなペン/スタイラスを使って、選んだオブジェクトの回転グリッパーを移動する。
-   タッチまたはパッシブなペン/スタイラスを使って、回転ジェスチャによって任意の方向にオブジェクトを回転させる。

**回転を使う状況**

ユーザーが直接 UI 要素を回転できるように回転を使います。 次の図は、サポートされる回転操作の指の配置をいくつか示しています。

![回転がサポートされる異なる指の配置を示す図](images/ux-rotate-positions.png)

**メモ**   直感的に言えば、ほとんどの場合、回転ポイントは2つのタッチポイントの1つです。ただし、ユーザーが連絡先ポイントに関連しない回転ポイント (たとえば、描画アプリケーションやレイアウトアプリケーションなど) を指定することはできません。 以下の図では、回転の中心点がこのような制約を受けない場合に、どのようにユーザー エクスペリエンスが低下するかについて説明します。

1 番目の図は、最初のタッチ ポイント (親指) と 2 番目のタッチ ポイント (人差し指) を示します。人差し指は木に、親指は丸太にタッチしています。

![回転ジェスチャのための最初の 2 つのタッチ ポイントを示す図](images/ux-rotate-points1.png)
2 番目の図では、最初のタッチ ポイント (親指) の周りで回転が行われています。 回転の後で、人差し指は相変わらず木の幹にタッチし、親指は相変わらず丸太 (回転の中心点) にタッチしています。

![回転の中心点が最初に 2 つタッチした点の 1 つに制約された状態で回転する絵を示す図](images/ux-rotate-points2.png)
3 番目の図では、回転の中心がアプリによって絵の中心点に定義されています (またはユーザーによって設定されています)。 回転の後で、絵が指の 1 つの周りで回転しなかったために、直接操作の画像が失われます (ユーザーがこの設定を選んだ場合を除きます)。

![回転の中心点が最初に 2 つタッチした点のどちらでもなく、絵の中心に制約された状態で回転する絵を示す図](images/ux-rotate-points3.png)
最後の図では、回転の中心がアプリによって絵の左端の中央の点に定義されています (またはユーザーによって設定されています)。 この場合も、ユーザーがこの設定を選んだ場合を除いて、直接操作の画像が失われます。

![回転の中心点が最初に 2 つタッチした点のどちらでもなく、絵の左端の中央に制約された状態で回転する絵を示す図](images/ux-rotate-points4.png)

 

Windows 10 では、3種類のローテーション (free、制約、および組み合わせ) がサポートされています。

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Type</th>
<th align="left">説明</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">自由回転</td>
<td align="left"><p>自由回転では、ユーザーはコンテンツを 360°の任意の位置に自由に回転できます。ユーザーがオブジェクトを離すと、オブジェクトは選んだ位置にとどまります。 自由回転は、Microsoft PowerPoint、Word、Visio、ペイントと Adobe Photoshop、Illustrator、Flash などの描画アプリやレイアウト アプリで便利です。</p></td>
</tr>
<tr class="even">
<td align="left">制約付き回転</td>
<td align="left"><p>制約付き回転は、操作中は自由回転をサポートしますが、離したときに 90°単位のスナップ位置が強制されます (0、90、180、270)。 ユーザーがオブジェクトを離すと、オブジェクトは自動的に最も近いスナップ位置まで回転します。</p>
<p>制約付き回転は回転の最も一般的な方法で、コンテンツのスクロールと同じように機能します。 スナップ位置があることで、ユーザーは操作が正確でなくても目標の位置に到達できます。 制約付きの回転は Web ブラウザーやフォト アルバムのようなアプリで便利です。</p></td>
</tr>
<tr class="odd">
<td align="left">複合回転</td>
<td align="left"><p>複合回転は自由回転をサポートしますが、(<a href="guidelines-for-panning.md">パン</a>におけるレールのように) 90°単位のスナップ位置のゾーンでは制約付き回転によって強制されます。 ユーザーが各 90° のゾーンの外でオブジェクトを離した場合にはオブジェクトはその位置にとどまりますが、それ以外の場合にはオブジェクトは自動的にスナップ位置まで回転します。</p>
<div class="alert">
<strong>メモ</strong>   ユーザーインターフェイスレールは、ターゲットの周囲の領域が特定の値または場所への移動を制限して、その選択に影響を与える機能です。
</div>
<div>
 
</div></td>
</tr>
</tbody>
</table>

## <a name="related-topics"></a>関連トピック

### <a name="samples"></a>サンプル

- [基本的な入力のサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
- [待機時間が短い入力のサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
- [ユーザー操作モードのサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode)
- [フォーカスの視覚効果のサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals)

### <a name="archive-samples"></a>サンプルのアーカイブ

- [入力: XAML ユーザー入力イベントのサンプルに関するページ](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20XAML%20user%20input%20events%20sample)
- [入力: デバイス機能のサンプルに関するページ](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Input%20Device%20capabilities%20sample%20(Windows%208))
- [入力: タッチのヒット テストのサンプルに関するページ](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20desktop%20samples/%5BC%2B%2B%5D-Windows%208%20desktop%20samples/C%2B%2B/Windows%208%20desktop%20samples/Input%20Touch%20hit%20testing%20sample)
- [XAML のスクロール、パン、ズームのサンプル](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Universal%20Windows%20app%20samples/111487-Universal%20Windows%20app%20samples/XAML%20scrolling%2C%20panning%2C%20and%20zooming%20sample)
- [入力: 簡略化されたインクのサンプル](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20Simplified%20ink%20sample)
- [入力: GestureRecognizer によるジェスチャと操作](/samples/browse/?redirectedfrom=MSDN-samples)
- [入力: 操作とジェスチャのサンプル](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20Gestures%20and%20manipulations%20with%20GestureRecognizer)
- [DirectX タッチ入力のサンプル](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%2B%2B%5D-Windows%208%20app%20samples/C%2B%2B/Windows%208%20app%20samples/DirectX%20touch%20input%20sample%20(Windows%208))
