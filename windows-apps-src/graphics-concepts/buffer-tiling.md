---
title: バッファーのタイル表示
description: バッファー リソースは 64 KB のタイルに分割されます。サイズが 64 KB の倍数でない場合は、最後のタイルに空きが生じます。
ms.assetid: 577DC6B0-F373-4748-AD80-2784C597C366
keywords:
- バッファーのタイル表示
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: fdb78dc2cff6ccedec58acbc946068e14511fdc2
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156356"
---
# <a name="buffer-tiling"></a>バッファーのタイル表示


[バッファー ](introduction-to-buffers.md) リソースは 64 KB のタイルに分割されます。サイズが 64 KB の倍数でない場合は、最後のタイルに空きが生じます。

構造化バッファーでは、タイリングのストライドに制約があってはなりません。 しかし、[**StructuredBuffers**](/windows/desktop/direct3dhlsl/sm5-object-structuredbuffer) を使用するためのハードウェアで可能となるパフォーマンスの最適化が、最初にタイリングを行うことにより損なわれる場合があります。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>関連トピック


[ストリーミング リソースの領域をタイル表示する方法](how-a-streaming-resource-s-area-is-tiled.md)

 

 