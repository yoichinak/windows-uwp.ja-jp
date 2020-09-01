---
title: テクスチャのブロック圧縮
description: Direct3D 11 では、テクスチャのブロック圧縮 (BC) サポートが拡張され、BC6H および BC7 アルゴリズムが組み込まれています。
ms.assetid: 63506C46-BF14-464B-B20C-8B8F359E7AFE
keywords:
- テクスチャのブロック圧縮
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 06c132f23d22dc688f82ff7976bcb93108f2ee1c
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89158816"
---
# <a name="texture-block-compression"></a>テクスチャのブロック圧縮


Direct3D 11 では、テクスチャのブロック圧縮 (BC) サポートが拡張され、BC6H および BC7 アルゴリズムが組み込まれています。 BC6H はハイ ダイナミック レンジのカラー ソース データをサポートし、BC7 は標準 RGB ソース データのアーティファクトを低減して標準よりも優れた品質の圧縮を提供します。

BC1 ～ BC5 形式のサポートなど、Direct3D 11 より前のブロック圧縮アルゴリズムのサポートに関する具体的な情報については、「[ブロック圧縮 (Direct3D 10)](/windows/desktop/direct3d10/d3d10-graphics-programming-guide-resources-block-compression)」をご覧ください。

**ファイル形式に関する注意:  ** BC6H と BC7 のテクスチャ圧縮形式では、圧縮されたテクスチャデータを格納するために DDS ファイル形式が使用されます。 詳しくは、「[DDS 用プログラミング ガイド](/windows/desktop/direct3ddds/dx-graphics-dds-pguide)」をご覧ください。

## <a name="span-idblock_compression_formats_supported_in_direct3d_11spanspan-idblock_compression_formats_supported_in_direct3d_11spanspan-idblock_compression_formats_supported_in_direct3d_11spanblock-compression-formats-supported-in-direct3d-11"></a><span id="Block_Compression_Formats_Supported_in_Direct3D_11"></span><span id="block_compression_formats_supported_in_direct3d_11"></span><span id="BLOCK_COMPRESSION_FORMATS_SUPPORTED_IN_DIRECT3D_11"></span>Direct3D 11 でサポートされている圧縮形式をブロックする


| ソース データ                                  | 最低限必要なデータ圧縮解像度                              | 推奨形式 | サポートされる最小機能レベル |
|----------------------------------------------|---------------------------------------------------------------------------|--------------------|---------------------------------|
| 3 チャネル カラーおよびアルファ チャネル       | 3 カラー チャネル (5 ビット:6 ビット:5 ビット)、および 0 または 1 ビットのアルファ  | BC1                | Direct3D 9.1                    |
| 3 チャネル カラーおよびアルファ チャネル       | 3 カラー チャネル (5 ビット:6 ビット:5 ビット)、および 4 ビットのアルファ         | BC2                | Direct3D 9.1                    |
| 3 チャネル カラーおよびアルファ チャネル       | 3 カラー チャネル (5 ビット:6 ビット:5 ビット)、および 8 ビットのアルファ          | BC3                | Direct3D 9.1                    |
| 1 チャネル カラー                            | 1 カラー チャネル (8 ビット)                                                | BC4                | Direct3D 10                     |
| 2 チャネル カラー                            | 2 カラー チャンネル (8 ビット:8 ビット)                                        | BC5                | Direct3D 10                     |
| 3 チャネル ハイ ダイナミック レンジ (HDR) カラー | "半分" の浮動小数点の3つのカラーチャネル (16 ビット:16 ビット:16 ビット)\* | BC6H               | Direct3D 11                     |
| 3 チャネル カラー、アルファ チャネルはオプション  | 3 カラー チャネル (チャネルあたり 4 ～ 7 ビット)、および 0 ～ 8 ビットのアルファ  | BC7                | Direct3D 11                     |

 

\*"ハーフ" 浮動小数点は、省略可能な符号ビット、5ビットバイアスの指数、および10または11ビットの仮数で構成される16ビット値です。
## <a name="span-idbc1__bc2__and_b3_formatsspanspan-idbc1__bc2__and_b3_formatsspanspan-idbc1__bc2__and_b3_formatsspanbc1-bc2-and-b3-formats"></a><span id="BC1__BC2__and_B3_Formats"></span><span id="bc1__bc2__and_b3_formats"></span><span id="BC1__BC2__AND_B3_FORMATS"></span>BC1、BC2、および B3 の形式


BC1、BC2、および BC3 形式は Direct3D 9 DXTn テクスチャ圧縮形式と等価で、対応する Direct3D 10 BC1、BC2、および BC3 形式と同じです。 これら3つの形式のサポートは、すべての機能レベルで必要となります (D3D \_ 機能 \_ レベル \_ 9 \_ 1、d3d \_ 機能 \_ レベル \_ 9 \_ 2、d3d \_ 機能 \_ レベル \_ 9 \_ 3、D3D \_ 機能 \_ レベル \_ 10 \_ 0、d3d \_ 機能 \_ レベル \_ 10 \_ 1、d3d \_ 機能 \_ レベル \_ 11 \_ 0)。

| ブロック圧縮形式 | DXGI 形式                                                                           | 相当する Direct3D 9 の形式                               | 4 x 4 のピクセル ブロックあたりのバイト数 |
|--------------------------|---------------------------------------------------------------------------------------|------------------------------------------------------------|---------------------------|
| BC1                      | DXGI \_ 形式 \_ BC1 \_ UNORM、dxgi \_ 形式 \_ BC1 \_ unorm \_ SRGB、dxgi \_ 形式 \_ BC1 \_ タイプレス | D3DFMT \_ dxt1、FourCC = "DXT1"                                | 8                         |
| BC2                      | DXGI \_ 形式 \_ BC2 \_ UNORM、dxgi \_ 形式 \_ BC2 \_ unorm \_ SRGB、dxgi \_ 形式 \_ BC2 \_ タイプレス | D3DFMT \_ DXT2 \* 、fourcc = "DXT2"、D3DFMT \_ DXT3、FOURCC = "DXT3" | 16                        |
| BC3                      | DXGI \_ 形式 \_ BC3 \_ UNORM、dxgi \_ 形式 \_ BC3 \_ unorm \_ SRGB、dxgi \_ 形式 \_ BC3 \_ タイプレス | D3DFMT \_ DXT4 \* 、fourcc = "DXT4"、D3DFMT \_ DXT5、FOURCC = "DXT5" | 16                        |

 

\*これらの圧縮方式 (DXT2 および DXT4) では、Direct3D 9 の事前乗算されたアルファ形式と標準のアルファ形式が区別されません。 これらの区別は、レンダリング時にプログラム可能なシェーダーで処理する必要があります。

## <a name="span-idbc4_and_bc5_formatsspanspan-idbc4_and_bc5_formatsspanspan-idbc4_and_bc5_formatsspanbc4-and-bc5-formats"></a><span id="BC4_and_BC5_Formats"></span><span id="bc4_and_bc5_formats"></span><span id="BC4_AND_BC5_FORMATS"></span>BC4 形式と BC5 形式


| ブロック圧縮形式 | DXGI 形式                                                                     | 相当する Direct3D 9 の形式 | 4 x 4 のピクセル ブロックあたりのバイト数 |
|--------------------------|---------------------------------------------------------------------------------|------------------------------|---------------------------|
| BC4                      | DXGI \_ 形式 \_ BC4 \_ UNORM、dxgi \_ 形式 \_ BC4 \_ snorm、dxgi \_ 形式 \_ BC4 \_ タイプレス | FourCC="ATI1"                | 8                         |
| BC5                      | DXGI \_ 形式 \_ BC5 \_ UNORM、dxgi \_ 形式 \_ BC5 \_ snorm、dxgi \_ 形式 \_ BC5 \_ タイプレス | FourCC="ATI2"                | 16                        |

 

## <a name="span-idbc6h_formatspanspan-idbc6h_formatspanspan-idbc6h_formatspanbc6h-format"></a><span id="BC6H_Format"></span><span id="bc6h_format"></span><span id="BC6H_FORMAT"></span>BC6H 形式


この形式については詳しくは、「[BC6H 形式](/windows/desktop/direct3d11/bc6h-format)」をご覧ください。

| ブロック圧縮形式 | DXGI 形式                                                                      | 相当する Direct3D 9 の形式 | 4 x 4 のピクセル ブロックあたりのバイト数 |
|--------------------------|----------------------------------------------------------------------------------|------------------------------|---------------------------|
| BC6H                     | DXGI \_ 形式 \_ BC6H \_ UF16、dxgi \_ 形式 \_ BC6H \_ SF16、dxgi \_ 形式 \_ BC6H \_ タイプレス | N/A                          | 16                        |

 

BC6H 形式では、4 x 4 のピクセル ブロックごとに異なるエンコード モードを選択できます。 全部で 14 種類のエンコード モードを利用でき、それぞれのモードには、表示されるテクスチャの画質に少しずつ異なるトレードオフがあります。 モードの選択によって、ソース コンテンツに合わせて品質レベルを選択または適合させたハードウェアで高速デコードできますが、検索領域の複雑さも大幅に増加します。

## <a name="span-idbc7_formatspanspan-idbc7_formatspanspan-idbc7_formatspanbc7-format"></a><span id="BC7_Format"></span><span id="bc7_format"></span><span id="BC7_FORMAT"></span>BC7 形式


この形式については詳しくは、「[BC7 形式](/windows/desktop/direct3d11/bc7-format)」をご覧ください。

| ブロック圧縮形式 | DXGI 形式                                                                           | 相当する Direct3D 9 の形式 | 4 x 4 のピクセル ブロックあたりのバイト数 |
|--------------------------|---------------------------------------------------------------------------------------|------------------------------|---------------------------|
| BC7                      | DXGI \_ 形式 \_ BC7 \_ UNORM、dxgi \_ 形式 \_ BC7 \_ unorm \_ SRGB、dxgi \_ 形式 \_ BC7 \_ タイプレス | N/A                          | 16                        |

 

BC7 形式では、4 x 4 のピクセル ブロックごとに異なるエンコード モードを選択できます。 全部で 8 種類のエンコード モードを利用でき、それぞれのモードには、表示されるテクスチャの画質に少しずつ異なるトレードオフがあります。 モードの選択によって、ソース コンテンツに合わせて品質レベルを選択または適合させたハードウェアで高速デコードできますが、検索領域の複雑さも大幅に増加します。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>関連トピック


[付録](appendix.md)

[テクスチャ](/windows/desktop/direct3d11/overviews-direct3d-11-resources-textures)

 

 