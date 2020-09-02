---
title: レベル 1
description: Direct3D のリソースのストリーミング機能について、階層1のサポートに影響する一般的な制限事項について説明します。
ms.assetid: 153DA429-0C99-4691-AEB4-124FD9B17BE2
keywords:
- レベル 1
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 2ff90c75e3f4aefa28ddad5528d39d2f2d541bb0
ms.sourcegitcommit: 5481bb34def681bc60fbfa42d9779053febec468
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/02/2020
ms.locfileid: "89304644"
---
# <a name="tier-1"></a>レベル 1


このセクションでは、階層 1 のサポートについて説明します。

## <a name="span-idtier_1_general_limitationsspanspan-idtier_1_general_limitationsspanspan-idtier_1_general_limitationsspantier-1-general-limitations"></a><span id="Tier_1_general_limitations"></span><span id="tier_1_general_limitations"></span><span id="TIER_1_GENERAL_LIMITATIONS"></span>階層1の一般的な制限事項


-   機能レベル 11.0 以上のハードウェア。
-   キルティングのサポートがない。
-   Texture1D または Texture3D のサポートがない。
-   2、8、または 16 サンプルのマルチサンプル アンチエイリアシング (MSAA) のサポートがない。 4 倍のみ必要、128 bpp 形式でないものを除く。
-   標準の スウィズル パターンがない (64 KB のタイルとテール ミップ パッキング内のレイアウトはハードウェア ベンダーにより異なる)。
-   重複するマッピングがあるときにタイルにアクセスする方法に関する制限。 「[重複するマッピングを含むタイルのアクセス制限](tile-access-limitations-with-duplicate-mappings.md)」をご覧ください。

## <a name="span-idspecific_limitations_affecting_tier_1_onlyspanspan-idspecific_limitations_affecting_tier_1_onlyspanspan-idspecific_limitations_affecting_tier_1_onlyspanspecific-limitations-affecting-tier-1-only"></a><span id="Specific_limitations_affecting_tier_1_only"></span><span id="specific_limitations_affecting_tier_1_only"></span><span id="SPECIFIC_LIMITATIONS_AFFECTING_TIER_1_ONLY"></span>階層 1 のみに影響を与える特定の制限事項


### <a name="span-idreading_writing_to_streaming_resources_that_have_null_mappingsspanspan-idreading_writing_to_streaming_resources_that_have_null_mappingsspanspan-idreading_writing_to_streaming_resources_that_have_null_mappingsspanreadingwriting-to-streaming-resources-that-have-null-mappings"></a><span id="Reading_writing_to_streaming_resources_that_have_NULL_mappings"></span><span id="reading_writing_to_streaming_resources_that_have_null_mappings"></span><span id="READING_WRITING_TO_STREAMING_RESOURCES_THAT_HAVE_NULL_MAPPINGS"></span>NULL マッピングのあるストリーミングリソースへの読み取り/書き込み

ストリーミング リソースに **NULL** マッピングを含めることはできますが、それらのリソースからの読み取りまたはリソースへの書き込みでは、デバイス削除などの定義されていない結果が生じます。 アプリケーションは、すべての空の領域に 1 つのダミー ページをマッピングすることで、この問題を回避できます。 複数のレンダー ターゲットの場所にマップされているページに書き込んだりレンダリングしたりする場合は、書き込みの順序が未定義になるため注意してください。

### <a name="span-idno_shader_instructions_for_clamping_lod_and_mapped_status_feedbackspanspan-idno_shader_instructions_for_clamping_lod_and_mapped_status_feedbackspanspan-idno_shader_instructions_for_clamping_lod_and_mapped_status_feedbackspanno-shader-instructions-for-clamping-lod-and-mapped-status-feedback"></a><span id="No_shader_instructions_for_clamping_LOD_and_mapped_status_feedback"></span><span id="no_shader_instructions_for_clamping_lod_and_mapped_status_feedback"></span><span id="NO_SHADER_INSTRUCTIONS_FOR_CLAMPING_LOD_AND_MAPPED_STATUS_FEEDBACK"></span>LOD とマップされた状態のフィードバックのシェーダー命令がありません

LOD のクランプとマップされた状態のフィードバックのためのシェーダー命令は利用できません。 「[HLSL ストリーミング リソースの露出](hlsl-streaming-resources-exposure.md)」をご覧ください。

### <a name="span-idalignment_constraints_for_standard_tile_shapesspanspan-idalignment_constraints_for_standard_tile_shapesspanspan-idalignment_constraints_for_standard_tile_shapesspanalignment-constraints-for-standard-tile-shapes"></a><span id="Alignment_constraints_for_standard_tile_shapes"></span><span id="alignment_constraints_for_standard_tile_shapes"></span><span id="ALIGNMENT_CONSTRAINTS_FOR_STANDARD_TILE_SHAPES"></span>標準のタイル形状に関する配置の制約

寸法がすべて標準タイル サイズの倍数であるミップ (最も細いものから開始) が、標準のタイル形状をサポートし、個々のタイルを任意にマッピング/マッピング解除できることだけが保証されます。 ストリーミング リソース内で寸法が標準タイル サイズの倍数ではない最初のミップマップは、すべてのより粗いミップマップと共に、標準ではないタイル形状を持つことができ、この一連のミップ用の N 個の 64 KB のタイルに一度に適合させることができます (N はアプリケーションに報告される)。 これらの N 個のタイルは 1 つのユニットとしてパックされたと見なされ、常にアプリケーションによって完全にマッピングされるか、完全にマッピング解除される必要があります。ただし、N 個のタイルそれぞれのマッピングは、タイル プール内の分離された任意の場所に配置することができます。

### <a name="span-idarray_of_mipmaps_that_aren_t_a_multiple_of_standard_tile_sizespanspan-idarray_of_mipmaps_that_aren_t_a_multiple_of_standard_tile_sizespanspan-idarray_of_mipmaps_that_aren_t_a_multiple_of_standard_tile_sizespanarray-of-mipmaps-that-arent-a-multiple-of-standard-tile-size"></a><span id="Array_of_mipmaps_that_aren_t_a_multiple_of_standard_tile_size"></span><span id="array_of_mipmaps_that_aren_t_a_multiple_of_standard_tile_size"></span><span id="ARRAY_OF_MIPMAPS_THAT_AREN_T_A_MULTIPLE_OF_STANDARD_TILE_SIZE"></span>標準のタイル サイズの倍数でないミップマップの配列

すべての次元で標準のタイル サイズの倍数でないミップマップが含まれるストリーミング リソースは、1 より大きい配列サイズを持つことはできません。

### <a name="span-idswitching_between_referencing_tiles_in_a_tile_pool_via_a_buffer_and_texture_resourcespanspan-idswitching_between_referencing_tiles_in_a_tile_pool_via_a_buffer_and_texture_resourcespanspan-idswitching_between_referencing_tiles_in_a_tile_pool_via_a_buffer_and_texture_resourcespanswitching-between-referencing-tiles-in-a-tile-pool-via-a-buffer-and-texture-resource"></a><span id="Switching_between_referencing_tiles_in_a_tile_pool_via_a_Buffer_and_Texture_resource"></span><span id="switching_between_referencing_tiles_in_a_tile_pool_via_a_buffer_and_texture_resource"></span><span id="SWITCHING_BETWEEN_REFERENCING_TILES_IN_A_TILE_POOL_VIA_A_BUFFER_AND_TEXTURE_RESOURCE"></span>バッファーおよびテクスチャ リソースを介してタイル プール内で参照しているタイルの切り替え

[バッファー](introduction-to-buffers.md)リソースを使用してタイルプール内のタイルを参照して、[テクスチャ](introduction-to-textures.md)リソースを介して同じタイルを参照するか、またはその逆の切り替えを行うために、タイルプールタイルへのマッピングを定義するタイルマッピングの更新またはタイルマッピングのコピーが、タイルに \* アクセスするために使用されるリソースディメンションと同じリソースディメンション (バッファーとテクスチャ) そうでない場合、デバイスのリセットの可能性も含めて動作が未定義になります。

たとえば、タイル マッピングを更新してバッファーのタイル マッピングを定義した後、[**Texture2D**](/windows/desktop/direct3dhlsl/sm5-object-texture2d) リソースを介してタイル プール内の同じタイルへのタイル マッピングを更新し、バッファを介してそれらのタイルにアクセスすることは正しくありません。 これを回避する操作は、タイルを共有するバッファーとテクスチャの間で切り替えるときにリソースのタイル マッピングを再定義するか、バッファー リソースとテクスチャ リソースの間でタイル プール内のタイルを共有しないことです。

### <a name="span-idmin_max_reduction_filteringspanspan-idmin_max_reduction_filteringspanspan-idmin_max_reduction_filteringspanminmax-reduction-filtering"></a><span id="Min_Max_reduction_filtering"></span><span id="min_max_reduction_filtering"></span><span id="MIN_MAX_REDUCTION_FILTERING"></span>最小/最大削減フィルター処理

最小/最大除去フィルタリングはサポートされません。 「[ストリーミング リソース テクスチャ サンプリング機能](streaming-resources-texture-sampling-features.md)」をご覧ください。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>関連トピック


[ストリーミング リソース機能の階層](streaming-resources-features-tiers.md)

 

 