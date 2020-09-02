---
title: 環境光
description: アンビエント照明がシーンに対して一定の光源を提供し、C++ を使用して Direct3D でアンビエント照明を設定する方法について説明します。
ms.assetid: C34FA65A-3634-4A4B-B183-4CDA89F4DC95
keywords:
- 環境光
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: c21a674b0961836752c879bcea681b568f31053c
ms.sourcegitcommit: 5481bb34def681bc60fbfa42d9779053febec468
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/02/2020
ms.locfileid: "89304464"
---
# <a name="ambient-lighting"></a>環境光

環境光は、シーンに一定の照明を生成します。 頂点法線、光の方向、光の位置、減衰などの他の照明要素に依存していないため、すべてのオブジェクト頂点を同じように照らします。 環境光は、すべての方向において一定であり、オブジェクトのすべてのピクセルが同じように色付けされます。 計算は高速ですが、オブジェクトはフラットに見え、リアル感が劣ります。

環境光は最も短時間で実現できるライティングですが、リアル感は最も劣ります。 Direct3D には、ライトを作成せずに使用できる 1 つのグローバル環境光プロパティがあります。 または、任意のライト オブジェクトを設定して、環境光を実現することもできます。

シーンの環境光は、次の式で表されます。

アンビエント照明 = C ₐ \* \[ g ₐ + sum (<sub>私の</sub> \* スポットライト<sub>i</sub> \* L<sub>ai</sub>)\]

各値の説明:

| パラメーター         | 既定値 | 型          | Description                                                                                                       |
|-------------------|---------------|---------------|-------------------------------------------------------------------------------------------------------------------|
| Cₐ                | (0,0,0,0)     | D3DCOLORVALUE | マテリアルのアンビエント色                                                                                            |
| Gₐ                | (0,0,0,0)     | D3DCOLORVALUE | グローバル アンビエント色                                                                                              |
| Atten<sub>i</sub> | (0,0,0,0)     | D3DCOLORVALUE | i 番目のライトの減衰。 「[減衰とスポットライト係数](attenuation-and-spotlight-factor.md)」を参照してください。 |
| Spot<sub>i</sub>  | (0,0,0,0)     | D3DVECTOR     | i 番目のライトのスポットライト係数。 「[減衰とスポットライト係数](attenuation-and-spotlight-factor.md)」を参照してください。  |
| Sum               | 該当なし           | 該当なし           | 環境光の合計                                                                                          |
| L<sub>ai</sub>    | (0,0,0,0)     | D3DVECTOR     | i 番目のライトのアンビエント色                                                                              |

 

Cₐ の値は、次のいずれかになります。

-   AMBIENTMATERIALSOURCE = D3DMCS color1 の場合は頂点 color1、頂点 \_ 宣言では最初の頂点の色が指定されます。
-   AMBIENTMATERIALSOURCE = D3DMCS color2 の場合は頂点 color2、 \_ 2 番目の頂点の色は頂点宣言で指定されます。
-   マテリアルのアンビエント色。

**メモ**   いずれかの AMBIENTMATERIALSOURCE オプションが使用されており、頂点の色が指定されていない場合は、素材のアンビエント色が使用されます。

 

マテリアルのアンビエント色を使用するには、下記のコード例に示すように、SetMaterial を使用します。

Gₐ は、グローバル アンビエント色です。 これは、SetRenderState (D3DRS アンビエント) を使用して設定され \_ ます。 Direct3D のシーンには、グローバル アンビエント色が 1 つあります。 このパラメーターは、Direct3D のライト オブジェクトには対応していません。

L<sub>ai</sub> は、シーンの i 番目のアンビエント色です。 各 Direct3D のライトには一連のプロパティがあり、その 1 つがアンビエント色です。 sum(L<sub>ai</sub>) の項は、シーン内のすべてのアンビエント色の合計です。

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>よう


この例では、オブジェクトの色は、シーンの環境光と素材のアンビエント色を使用しています。

```cpp
#define GRAY_COLOR  0x00bfbfbf

Ambient.r = 0.75f;
Ambient.g = 0.0f;
Ambient.b = 0.0f;
Ambient.a = 0.0f;
```

この方程式では、生成されるオブジェクトの頂点の色は、マテリアルの色と光の色の組み合わせになります。

次の 2 つのイメージは、マテリアルの色がグレーで、ライトの色が明るい赤であることを示しています。

![灰色の球体の図](images/amb1.jpg)![赤の球体の図](images/lightred.jpg)

結果として作成されるシーンは、次のようになります。 シーン内の唯一のオブジェクトは球です。 環境光は、すべてのオブジェクトの頂点を同じ色で照らします。 頂点の法線やライトの方向には依存しません。 その結果、オブジェクトのサーフェスの周囲にシェーディングの差が生じないため、球体は 2D の円のように見えます。

![環境光のある球体の図](images/lighta.jpg)

オブジェクトをよりリアルに見せるには、環境光に加えて、ディフューズ ライティングやスペキュラ ライティングを適用します。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>関連トピック


[光源の計算](mathematics-of-lighting.md)

 

 




