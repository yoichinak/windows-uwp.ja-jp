---
Description: このトピックでは、ローテーション用の新しい Windows UI について説明し、UWP アプリでこの新しい対話機構を使用するときに考慮する必要があるユーザーエクスペリエンスガイドラインを示します。
title: 回転
ms.assetid: f098bc05-35b3-46b2-9e9b-9ff292d067ca
label: Rotation
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 71a09d304b0d68bf01012166173360ec6304fb2c
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/20/2019
ms.locfileid: "74257927"
---
# <a name="rotation"></a>回転


この記事では、新しい Windows UI の回転について説明し、UWP アプリでこの新しい操作のメカニズムを使うときに考慮する必要があるユーザー エクスペリエンスのガイドラインを示します。

> **重要な API**: [**Windows.UI.Input**](https://docs.microsoft.com/uwp/api/Windows.UI.Input)、[**Windows.UI.Xaml.Input**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input)

## <a name="dos-and-donts"></a>推奨と非推奨

-   ユーザーが直接 UI 要素を回転できるように回転を使います。

## <a name="additional-usage-guidance"></a>その他の使い方のガイダンス


**ローテーションの概要**

回転は UWP アプリで使われるタッチ操作に最適な手法であり、ユーザーがオブジェクトを回転 (時計回りまたは反時計回り) できるようにします。

入力デバイスに応じて回転操作は次のように実行されます。

-   マウスまたはアクティブなペン/スタイラスを使って、選んだオブジェクトの回転グリッパーを移動する。
-   タッチまたはパッシブなペン/スタイラスを使って、回転ジェスチャによって任意の方向にオブジェクトを回転させる。

**回転を使用する場合**

ユーザーが直接 UI 要素を回転できるように回転を使います。 次の図は、サポートされる回転操作の指の配置をいくつか示しています。

![回転がサポートされる異なる指の配置を示す図](images/ux-rotate-positions.png)

わかりやすいように、ほとんどの場合、この2つのタッチポイントの1つは、ユーザーが連絡先ポイントに関連しない回転ポイント (たとえば、描画またはレイアウトのアプリケーション) を指定できない場合に限ります **。  ** 以下の図では、回転の中心点がこのような制約を受けない場合に、どのようにユーザー エクスペリエンスが低下するかについて説明します。

1 番目の図は、最初のタッチ ポイント (親指) と 2 番目のタッチ ポイント (人差し指) を示します。人差し指は木に、親指は丸太にタッチしています。

回転ジェスチャの2つの初期タッチポイントを示す ![イメージ。](images/ux-rotate-points1.png)
2 番目の図では、最初のタッチ ポイント (親指) の周りで回転が行われています。 回転の後で、人差し指は相変わらず木の幹にタッチし、親指は相変わらず丸太 (回転の中心点) にタッチしています。

回転ポイントが2つの初期タッチポイントのいずれかに制限されている回転した画像を表示する ![イメージ。](images/ux-rotate-points2.png)
3 番目の図では、回転の中心がアプリによって絵の中心点に定義されています (またはユーザーによって設定されています)。 回転の後で、絵が指の 1 つの周りで回転しなかったために、直接操作の画像が失われます (ユーザーがこの設定を選んだ場合を除きます)。

回転ポイントが2つの初期タッチポイントではなく、画像の中心に制限されている回転した画像を表示する画像を ![します。](images/ux-rotate-points3.png)
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
<th align="left">種類</th>
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
<strong>注</strong>  ユーザーインターフェイスのレールは、ターゲットの周囲の領域が特定の値または場所への移動を制限して、その選択に影響を与える機能です。
</div>
<div>
 
</div></td>
</tr>
</tbody>
</table>

 

## <a name="related-topics"></a>関連トピック


**サンプル**
* [基本的な入力サンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
* [低待機時間入力サンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
* [ユーザー操作モードのサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode)
* [フォーカスの視覚効果のサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals)

**サンプルのアーカイブ**
* [入力: XAML ユーザー入力イベントのサンプル](https://code.msdn.microsoft.com/windowsapps/Input-3dff271b)
* [入力: デバイス機能のサンプル](https://code.msdn.microsoft.com/windowsapps/Input-device-capabilities-31b67745)
* [入力: タッチヒットテストのサンプル](https://code.msdn.microsoft.com/windowsapps/Touch-Hit-Testing-sample-5e35c690)
* [XAML のスクロール、パン、ズームのサンプル](https://code.msdn.microsoft.com/windowsapps/xaml-scrollviewer-pan-and-949d29e9)
* [入力: 簡略化されたインクのサンプル](https://code.msdn.microsoft.com/windowsapps/Input-simplified-ink-sample-11614bbf)
* [入力: GestureRecognizer でのジェスチャと操作](https://docs.microsoft.com/samples/browse/?redirectedfrom=MSDN-samples)
* [入力: 操作とジェスチャ (C++) のサンプル](https://code.msdn.microsoft.com/windowsapps/Manipulations-and-gestures-362b6b59)
* [DirectX タッチ入力のサンプル](https://code.msdn.microsoft.com/windowsapps/Simple-Direct3D-Touch-f98db97e)
 

 




