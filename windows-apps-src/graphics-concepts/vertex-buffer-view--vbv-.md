---
title: 頂点バッファー ビュー (VBV) とインデックス バッファー ビュー (IBV)
description: Direct3D レンダリングで頂点のデータと整数インデックスを保持する、頂点バッファービュー (VBV) とインデックスバッファービュー (IBV) について説明します。
ms.assetid: 695115D2-9DA0-41F2-9416-33BFAB698129
keywords:
- 頂点バッファー ビュー (VBV)
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: a616f2bad8f478b2d20e96b183ba944950fef8a8
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156096"
---
# <a name="vertex-buffer-view-vbv-and-index-buffer-view-ibv"></a>頂点バッファー ビュー (VBV) とインデックス バッファー ビュー (IBV)


頂点バッファーには、頂点のリストのデータが保持されます。 各頂点のデータには、位置、色、法線ベクトル、テクスチャ座標などを含めることができます。 インデックス バッファーには、頂点バッファーへの整数インデックス (オフセット) が保持されます。インデックス バッファーは、頂点の完全なリストのサブセットから成るオブジェクトを定義してレンダリングするために使われます。

多くの場合、単独の頂点の定義はアプリケーションが独自に決定できます。たとえば、次のように定義することができます。

``` syntax
struct CUSTOMVERTEX { 
        FLOAT x, y, z;      // The position
        FLOAT nx, ny, nz;   // The normal
        DWORD color;        // RGBA color
        FLOAT tu, tv;       // The texture coordinates. 
}; 
```

その後、CUSTOMVERTEX の定義は、頂点バッファーの作成時にグラフィックス ドライバーに渡されます。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>関連トピック


[ビュー](views.md)

 

 




