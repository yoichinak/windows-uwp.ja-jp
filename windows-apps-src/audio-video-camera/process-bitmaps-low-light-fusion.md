---
description: この記事では、LowLightFusion クラスを使用してビットマップを処理する方法について説明します。
title: 低光量 Fusion API を使用したビットマップの処理
ms.date: 03/22/2018
ms.topic: article
keywords: Windows 10, UWP, 低光量 Fusion, ビットマップ, 画像処理
ms.localizationpriority: medium
ms.openlocfilehash: 4e82eb780efe83125a09417f349f84ee9451c1f0
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363835"
---
# <a name="process-bitmaps-with-the-lowlightfusion-api"></a>LowLightFusion を使用したビットマップの処理

低光量画像は高画質でキャプチャするのが難しい画像です。絞りとセンサー サイズが固定されているモバイル デバイスでは、特にキャプチャするのが難しくなります。 低光量を補正するために、デバイスでは露出時間を長くしたり、センサー ゲインを上げたりする場合がありますが、これにより、モーション ブラーが発生したり、画像のノイズが増えたりする可能性があります。 

[LowLightFusion class](/uwp/api/windows.media.core.lowlightfusion) を使用すると、時間的に非常に近接している複数のフレームからのピクセル情報をサンプリングすることによって、低光量画像の画質が向上します。つまり、短時間のバースト画像によって、ノイズとモーション ブラーが減少します。 これは、写真編集アプリなどに追加できる便利な機能です。

この機能は、[AdvancedPhotoCapture クラス](/uwp/api/Windows.Media.Capture.AdvancedPhotoCapture)を介して利用することもできます。このクラスでは、必要に応じて、画像のキャプチャの直後に低光量 Fusion アルゴリズムを一連の画像に適用します。 この機能の実装方法については、「[低光量写真](./high-dynamic-range-hdr-photo-capture.md#low-light-photo-capture)のキャプチャ」をご覧ください。

## <a name="prepare-the-images-for-processing"></a>処理する画像を準備する

この例では、[LowLightFusion クラス](/uwp/api/windows.media.core.lowlightfusion) および [FileOpenPicker](/uwp/api/Windows.Storage.Pickers.FileOpenPicker) を使用して、ユーザーが低光量 Fusion を実行する複数の画像を選択できるようにする方法について説明します。

最初に、アルゴリズムで受け入れる画像の枚数 (フレームとも呼ばれます) を決定し、これらのフレームを格納するリストを作成する必要があります。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/LowLightFusionSample/cs/MainPage.xaml.cs" id="SnippetGetMaxLLFFrames":::

低光量 Fusion アルゴリズムで受け入れるフレームの枚数を決定したら、[FileOpenPicker](/uwp/api/Windows.Storage.Pickers.FileOpenPicker) を使用して、アルゴリズムで利用される画像をユーザーが選択できるようにすることができます。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/LowLightFusionSample/cs/MainPage.xaml.cs" id="SnippetGetFrames":::

適切な枚数のフレームが選択されているので、次に、フレームを [SoftwareBitmaps](/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) にデコードし、SoftwareBitmaps が LowLightFusion の正しい形式になっていることを確認する必要があります。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/LowLightFusionSample/cs/MainPage.xaml.cs" id="SnippetDecodeFrames":::


## <a name="fuse-the-bitmaps-into-a-single-bitmap"></a>複数のビットマップを 1 つのビットマップに合成する

適切な枚数のフレームが受け付け可能な形式になっているので、次に、**[FuseAsync](/uwp/api/windows.media.core.lowlightfusion.fuseasync)** メソッドを使用して、低光量 Fusion アルゴリズムを適用します。 結果として生成される処理済みの画像は、鮮明度が向上しており、SoftwareBitmap の形式になります。 

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/LowLightFusionSample/cs/MainPage.xaml.cs" id="SnippetFuseFrames":::

最後に、ユーザーが使い慣れた "通常" の画像形式 (作業を開始したときと同様の画像形式) にエンコードし保存することによって、生成された SoftwareBitmap をクリーンアップします。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/LowLightFusionSample/cs/MainPage.xaml.cs" id="SnippetEncodeFrame":::


## <a name="before-and-after"></a>適用前と適用後

1 つの入力画像と、低光量 Fusion アルゴリズムを適用した結果の出力画像の例を示します。

> [!div class="mx-tableFixed"] 
| 入力フレーム | 低光量 Fusion による出力 | 
|-------------|-------------------------|
| ![低光量 Fusion アルゴリズムへの入力フレーム](./images/LLF-Input.png) | ![低光量 Fusion アルゴリズムを適用した結果のフレーム](./images/LLF-Output.png) |

照明やバナーの周囲の影の鮮明さが向上したことが、入力フレームから確認できます。

## <a name="related-topics"></a>関連トピック 
[LowLightFusion クラス](/uwp/api/windows.media.core.lowlightfusion)  
[LowLightFusionResult クラス](/uwp/api/windows.media.core.lowlightfusionresult)
