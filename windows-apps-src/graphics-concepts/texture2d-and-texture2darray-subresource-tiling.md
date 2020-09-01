---
title: Texture2D と Texture2DArray のサブリソースのタイル表示
description: 以下の表に、Texture2D および Texture2DArray サブリソースがどのようにタイル表示されるかを示します。
ms.assetid: 2DC14DFC-5299-44D9-895F-5A223D3FD530
keywords:
- Texture2D と Texture2DArray のサブリソースのタイル表示
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 28152f39983f4831a9efa981efcb85fb65fa0204
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156176"
---
# <a name="texture2d-and-texture2darray-subresource-tiling"></a>Texture2D と Texture2DArray のサブリソースのタイル表示


次の表に、[**Texture2D**](/windows/desktop/direct3dhlsl/sm5-object-texture2d) および [**Texture2DArray**](/windows/desktop/direct3dhlsl/sm5-object-texture2darray) サブリソースがどのようにタイル表示されるかを示します。 これらの表の値は、テール ミップ パッキングをカウントしていません。

## <a name="span-idsubresources-with-multisample-counts-of-1spanspan-idsubresources-with-multisample-counts-of-1spanspan-idsubresources-with-multisample-counts-of-1spansubresources-with-multisample-counts-of-1"></a><span id="Subresources-with-multisample-counts-of-1"></span><span id="subresources-with-multisample-counts-of-1"></span><span id="SUBRESOURCES-WITH-MULTISAMPLE-COUNTS-OF-1"></span>マルチサンプリング数が1のサブリソース


次の表に、マルチサンプル数が 1 の [**Texture2D**](/windows/desktop/direct3dhlsl/sm5-object-texture2d) および [**Texture2DArray**](/windows/desktop/direct3dhlsl/sm5-object-texture2darray) サブリソースがどのようにタイル表示されるかを示します。

| ビット/ピクセル (1 サンプル/ピクセル) | タイルの寸法 (ピクセル、W x H) |
|-----------------------------|-------------------------------|
| 8                           | 256 x 256                       |
| 16                          | 256 x 128                       |
| 32                          | 128 x 128                       |
| 64                          | 128 x 64                        |
| 128                         | 64 x 64                         |
| BC1、4                       | 512 x 256                       |
| BC2、3、5、6、7                 | 256 x 256                       |

 

ストリーミングリソースでサポートされていない形式のビットカウントは、96 bpp 形式、ビデオ形式、DXGI \_ フォーマット \_ R1 \_ UNORM、dxgi \_ 形式の \_ R8G8 \_ B8G8 \_ unorm、および dxgi \_ 形式 \_ R8R8 \_ G8B8 \_ unorm です。

## <a name="span-idsubresources-with-various-multisample-countsspanspan-idsubresources-with-various-multisample-countsspanspan-idsubresources-with-various-multisample-countsspansubresources-with-various-multisample-counts"></a><span id="Subresources-with-various-multisample-counts"></span><span id="subresources-with-various-multisample-counts"></span><span id="SUBRESOURCES-WITH-VARIOUS-MULTISAMPLE-COUNTS"></span>さまざまなマルチサンプリング数を持つサブリソース


次の表に、さまざまなマルチサンプル数の [**Texture2D**](/windows/desktop/direct3dhlsl/sm5-object-texture2d) および [**Texture2DArray**](/windows/desktop/direct3dhlsl/sm5-object-texture2darray) サブリソースがどのようにタイル表示されるかを示します。

| ビット/ピクセル (1 サンプル/ピクセル) | タイルの寸法 (ピクセル、W x H) |
|-----------------------------|-------------------------------|
| 1                           | 1 x 1                           |
| 2                           | 2 x 1                           |
| 4                           | 2 x 2                           |
| 8                           | 4 x 2                           |
| 16                          | 4x4                           |

 

サンプル数 1 および 4 だけが、ストリーミング リソースにサポート (および許可) される必要があります。 ストリーミング リソースは現在、2、8、および 16 をサポートしていません (表示されていますが)。

実装では、ストリーミング リソースでサポートされていない場合でも、非ストリーミング リソースで 2、8、または 16 サンプルのマルチサンプル アンチエイリアシング (MSAA) モードをサポートすることを選択できます。

サンプル数が 1 より大きいストリーミング リソースは、128 bpp 形式を使うことはできません。

サポートされているサンプルの数と形式に関する制約は、仕様とハードウェアの不一致が原因です。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>関連トピック


[ストリーミング リソースの領域をタイル表示する方法](how-a-streaming-resource-s-area-is-tiled.md)

 

 