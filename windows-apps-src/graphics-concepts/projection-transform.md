---
title: "射影トランスフォーム"
description: "射影トランスフォームは、カメラの内部を制御します。つまり、カメラのレンズを選ぶことと似ています。 このトランスフォームは、3 種類のトランスフォームの中で最も複雑です。"
ms.assetid: 378F205D-3800-4477-9820-5EBE6528B14A
keywords:
- "射影トランスフォーム"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 83679e9a41adcad68f1341328de4c03b10db08e5
ms.lasthandoff: 02/07/2017

---

# <a name="projection-transform"></a>射影トランスフォーム


*射影トランスフォーム*は、カメラの内部を制御します。つまり、カメラのレンズを選ぶことと似ています。 このトランスフォームは、3 種類のトランスフォームの中で最も複雑です。

射影行列とは、通常、スケーリングおよび遠近法による射影です。 射影トランスフォームでは、視錐台を立方体に変換します。 視錐台の近くの端は遠くの端よりも小さいので、このトランスフォームにはカメラの近くのオブジェクトを拡大するという効果があり、これによってシーンに遠近感が生まれます。

[視錘台](viewports-and-clipping.md)では、ビュー トランスフォーム空間の原点とカメラの間の距離が任意の値 D として定義され、射影行列は次のようになります。

![射影行列の図](images/projmat1.png)

ビュー行列は、z 方向に - D だけ平行移動することによって、カメラを原点に平行移動します。平行移動行列は、次のようになります。

![平行移動行列の図](images/projmat2.png)

平行移動行列に射影行列を乗算 (T\*P) すると、それらを合成した射影行列ができます。これは、次のようになります。

![合成した射影行列の図](images/projmat3.png)

次の図は、パースペクティブ (遠近投影) トランスフォームで視錐台を新しい座標空間に変換する方法を示しています。 視錐台が立方体になること、およびシーンの右上角にあった原点が中心に移動することに注目してください。

![パースペクティブ トランスフォームで視錐台を新しい座標空間に変更する方法の図](images/cuboid.png)

パースペクティブ トランスフォームでは、x 方向と y 方向の限界は -1 と 1 です。 z 方向の限界は、前方面については 0 で、後方面については 1 です。

この行列は、カメラから付近のクリップ面まで、指定された距離に基づいてオブジェクトを平行移動およびスケーリングします。しかし、この行列は視野 (fov) を考慮せず、遠くのオブジェクトに対して生成する z 値はほとんど同じになる可能性があるので、深度比較が困難になります。 この問題を解決するために、次の行列は、ビューポートのアスペクト比を考慮して頂点を調整し、アスペクト比をパースペクティブ射影に合わせます。

![パースペクティブ射影の行列の図](images/prjmatx1.png)

この行列では、Zₙ は付近のクリップ面の z 値です。 変数 w、h、および Q には、次のような意味があります。 fov<sub>w</sub> および fovₖ は、ビューポートの水平方向および垂直方向の視野を表します (ラジアン単位)。

![変数の意味の公式](images/prjmatx2.png)

アプリケーションでは、視野角度を使って x と y のスケール係数を定義することは、ビューポートの水平寸法と垂直寸法 (カメラ空間の) を使用するのに比べて不便な場合があります。 数値演算として、次に示す w と h の 2 つの式ではビューポートの寸法を使用します。これらの式は上の公式と同等です。

![w および h 変数の意味の公式](images/prjmatx3.png)

これらの式では、Zₙ は付近のクリップ面の位置を表し、変数 V<sub>w</sub> と Vₕ はカメラ空間でのビューポートの幅と高さを表しています。

どの式を使用する場合でも、必ず、Zₙ をできるだけ大きな値に設定します。カメラに極端に近い z 値は大幅には変化しないからです。 これにより、16 ビットの z バッファーを使う深度比較は多少複雑になります。

## <a name="span-idawfriendlyprojectionmatrixspanspan-idawfriendlyprojectionmatrixspanspan-idawfriendlyprojectionmatrixspana-w-friendly-projection-matrix"></a><span id="A_W_Friendly_Projection_Matrix"></span><span id="a_w_friendly_projection_matrix"></span><span id="A_W_FRIENDLY_PROJECTION_MATRIX"></span>w バッファーに有効な射影行列


Direct3D では、ワールド行列、ビュー行列、および射影行列によって変換された頂点の ｗ 成分を利用して、深度バッファーまたはフォグ エフェクトの計算を深度をベースに実行できます。 このような計算では、射影行列で w を正規化して、ワールド空間の z と等価にする必要があります。 つまり、射影行列に 1 ではない (3,4) 係数が含まれる場合、(3,4) 係数の逆数を使ってすべての係数をスケーリングすることで、適切な行列を作成しなければなりません。 対応していない行列を使用すると、フォグ エフェクトと深度バッファーが正しく適用されません。

次の図は、対応していない射影行列、および同じ行列をスケーリングして視点との相対フォグが有効になるようにした行列を示しています。

![対応していない射影行列、および視点との相対フォグが有効になっている行列の図](images/eyerlmx.png)

上の図では、すべての変数がゼロ以外の値であるとします。 w ベースの深度バッファリングについて詳しくは、「[深度バッファー](depth-buffers.md)」を参照してください。

Direct3D では、現在設定されている射影行列を使って w ベース深度の計算を実行します。 したがって、トランスフォームで Direct3D を使用しない場合であっても、アプリケーションでは、目的の w ベース機能を取得するために適切な射影行列を設定しておく必要があります。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>関連項目


[トランスフォーム](transforms.md)

 

 




