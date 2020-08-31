---
title: Texture3D サブリソースのタイル表示
description: Texture2D タイルのピクセルあたりのビット数に基づいて、Texture3D サブリソースがどのように並べて表示されるかを示す表をご覧ください。
ms.assetid: 210D03E4-CF12-47E0-BA2F-C8D059B17D3E
keywords:
- Texture3D サブリソースのタイル表示
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 9ee4ff5c87022f9fd303b1331665a2551704cb93
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167946"
---
# <a name="texture3d-subresource-tiling"></a>Texture3D サブリソースのタイル表示


次の表に、[**Texture3D**](/windows/desktop/direct3dhlsl/sm5-object-texture3d) サブリソースがどのようにタイル表示されるかを示します。 この表の値は、テール ミップ パッキングをカウントしていません。

次の表では、[**Texture2D**](/windows/desktop/direct3dhlsl/sm5-object-texture2d) のタイル表示を受け取り、x/y 次元をそれぞれ 4 で割り、16 レイヤーの深度を追加します。 最初の平面 (最初の 16 レイヤーの深度を定義するタイルの 2D 平面) のすべてのタイルが、後続の平面の前に表示されます。

**注** ストリーミング リソースでの   [**Texture3D**](/windows/desktop/direct3dhlsl/sm5-object-texture3d) のサポートは、ストリーミング リソースの最初の実装では公開されませんが、今後のリリースでのサポートに備えて必要なタイル形状をここに示します。

 

| ビット/ピクセル (1 サンプル/ピクセル) | タイルの寸法 (ピクセル、W x H x D) |
|-----------------------------|---------------------------------|
| 8                           | 64 x 32 x 32                        |
| 16                          | 32 x 32 x 32                        |
| 32                          | 32 x 32 x 16                        |
| 64                          | 32 x 16 x 16                        |
| 128                         | 16 x 16 x 16                        |
| BC1、4                       | 128 x 64 x 16                       |
| BC2、3、5、6、7                 | 64 x 64 x 16                        |

 

ストリーミングリソースでサポートされていない形式のビットカウントは、96 bpp 形式、ビデオ形式、DXGI \_ フォーマット \_ R1 \_ UNORM、dxgi \_ 形式の \_ R8G8 \_ B8G8 \_ unorm、および dxgi \_ 形式 \_ R8R8 \_ G8B8 \_ unorm です。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>関連トピック


[ストリーミング リソースの領域をタイル表示する方法](how-a-streaming-resource-s-area-is-tiled.md)

 

 