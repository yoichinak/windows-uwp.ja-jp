---
title: テクスチャ リソース
description: Direct3D テクスチャリソースを使用したレンダリングについて、またテクスチャステージを使用して複数のテクスチャブレンドをサポートする方法について説明します。
ms.assetid: 016F6CDA-D361-4E6B-BA99-49E915A19364
keywords:
- テクスチャ リソース
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: d82a5525601c98812d6aab97f5f5d4399ceddc91
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156186"
---
# <a name="texture-resources"></a>テクスチャ リソース


テクスチャは、レンダリングに使われるリソースの一種です。

## <a name="span-idrendering_with_texture_resourcesspanspan-idrendering_with_texture_resourcesspanspan-idrendering_with_texture_resourcesspanrendering-with-texture-resources"></a><span id="Rendering_with_Texture_Resources"></span><span id="rendering_with_texture_resources"></span><span id="RENDERING_WITH_TEXTURE_RESOURCES"></span>テクスチャ リソースを使ったレンダリング


Direct3D は、テクスチャ ステージの概念による複数のテクスチャ ブレンドをサポートしています。 各テクスチャ ステージには、テクスチャとそのテクスチャに対して実行できる操作が含まれています。 テクスチャ ステージ内のテクスチャが現在のテクスチャのセットを構成します。 「[テクスチャ ブレンド](texture-blending.md)」をご覧ください。 各テクスチャの状態は、そのテクスチャ ステージにカプセル化されます。

また、アプリケーションは、テクスチャの視点とテクスチャ フィルタリングの状態を設定することもできます。 「[テクスチャ フィルタリング](texture-filtering.md)」をご覧ください。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>関連トピック


[テクスチャ](textures.md)

 

 




