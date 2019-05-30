---
ms.assetid: 98BD79B3-F420-43C5-98D3-52EBDDB479A0
description: この記事では、BitmapEncoder で使用できるエンコーディング オプションを示します。
title: BitmapEncoder オプションのリファレンス
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 34e2ac1531b496f076abbe8e23ffa1c0cb318373
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/29/2019
ms.locfileid: "66359051"
---
# <a name="bitmapencoder-options-reference"></a>BitmapEncoder オプションのリファレンス


この記事では、[**BitmapEncoder**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapEncoder) で使用できるエンコーディング オプションを示します。 エンコーディング オプションは、対応する名前の文字列と特定のデータ型 ([**Windows.Foundation.PropertyType**](https://docs.microsoft.com/uwp/api/Windows.Foundation.PropertyType)) の値によって定義されます。 画像の操作について詳しくは、「[ビットマップ画像の作成、編集、保存](imaging.md)」をご覧ください。

| 名前                    | PropertyType | 使用上の注意                                                                                        | 有効な形式 |
|-------------------------|--------------|----------------------------------------------------------------------------------------------------|---------------|
| ImageQuality            | single       | 有効な値は 0 ～ 1.0 です。 値が大きいほど、画質が高くなります。                                 | JPEG、JPEG-XR |
| CompressionQuality      | single       | 有効な値は 0 ～ 1.0 です。 値が大きいほど、効率の高い (時間のかかる) 圧縮方式であることを示します。 | TIFF          |
| Lossless                | boolean      | true に設定すると、ImageQuality オプションが無視されます。                                        | JPEG-XR       |
| InterlaceOption         | boolean      | 画像をインターレースするかどうかを示します。                                                                    | PNG           |
| FilterOption            | uint8        | [  **PngFilterMode**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.PngFilterMode) 列挙値を使います。                                | PNG           |
| TiffCompressionMethod   | uint8        | [  **TiffCompressionMode**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.TiffCompressionMode) 列挙値を使います。                    | TIFF          |
| Luminance               | uint32Array  | 輝度の量子化定数を格納する 64 要素の配列です。                               | JPEG          |
| Chrominance             | uint32Array  | クロミナンスの量子化定数を格納する 64 要素の配列です。                             | JPEG          |
| JpegYCrCbSubsampling    | uint8        | [  **JpegSubsamplingMode**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.JpegSubsamplingMode) 列挙値を使います。                    | JPEG          |
| SuppressApp0            | boolean      | App0 メタデータ ブロックの作成を抑制するかどうかを示します。                                        | JPEG          |
| EnableV5Header32bppBGRA | boolean      | アルファをサポートするバージョン 5 BMP にエンコードするかどうかを示します。                                         | BMP           |

 

## <a name="related-topics"></a>関連トピック

* [ビットマップ画像の作成、編集、保存](imaging.md)
* [サポートされるコーデック](supported-codecs.md)

 




