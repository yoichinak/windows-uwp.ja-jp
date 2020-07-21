---
ms.assetid: 370f2c14-4f1e-47b3-9197-24205ab255a3
description: この記事では、UWP アプリで使用可能なカメラ機能と、その使用方法を示すハウツー記事へのリンクを示します。
title: Camera
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d7d7fcdeb3740ac4c6851170796243392676d1d1
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/20/2019
ms.locfileid: "74254288"
---
# <a name="camera"></a>Camera

このセクションでは、カメラやマイクを使って写真、ビデオ、オーディオをキャプチャするユニバーサル Windows プラットフォーム (UWP) アプリの作成について説明します。

## <a name="use-the-windows-built-in-camera-ui"></a>Windows 組み込みのカメラ UI を使う

| トピック | 説明 |
|---------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Windows ビルトインカメラ UI を使用して写真とビデオをキャプチャする](capture-photos-and-video-with-cameracaptureui.md) | [  **CameraCaptureUI**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.CameraCaptureUI) クラスを使用して、Windows に組み込まれているカメラ UI で写真またはビデオをキャプチャする方法を説明します。 ユーザーが写真やビデオをキャプチャしてアプリに結果を返すだけでよい場合は、これが最も早くて簡単な方法です。  |

## <a name="basic-mediacapture-tasks"></a>基本的な MediaCapture タスク

| トピック | 説明 |
|---------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [カメラのプレビューを表示する](simple-camera-preview-access.md) | UWP アプリで XAML ページ内にカメラ プレビュー ストリームをすばやく表示する方法を示します。 |
| [MediaCapture を使用した基本的な写真、ビデオ、オーディオキャプチャ](basic-photo-video-and-audio-capture-with-MediaCapture.md) | [  **MediaCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture) クラスを使用して写真やビデオをキャプチャする最も簡単な方法を示します。 **MediaCapture** クラスは、キャプチャ パイプラインに対する低レベルの制御を提供し、高度なキャプチャ シナリオを実現する、堅牢な一連の API を公開しますが、この記事では基本的なメディア キャプチャをアプリにすばやく簡単に追加できるようにすることを目的としています。 |
| [モバイルデバイス向けのカメラ UI 機能](camera-ui-features-for-mobile-devices.md) | モバイル デバイス上にのみある特殊カメラの UI 機能を活用する方法を示します。  |
                                                                                                               
## <a name="advanced-mediacapture-tasks"></a>高度な MediaCapture タスク   
                                                                                                               
| トピック                                                                                             | 説明                                                                                                                                                                                                                                                                                    |
|---------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [MediaCapture を使用してデバイスと画面の向きを処理する](handle-device-orientation-with-mediacapture.md) | 写真とビデオをキャプチャするときに、ヘルパー クラスを使ってデバイスの向きを処理する方法について説明します。 | 
| [カメラのプロファイルでカメラの機能を検出して選択する](camera-profiles.md) | カメラ プロファイルを使ってさまざまなビデオ キャプチャ デバイスの機能を検出および管理する方法について説明します。 これには、特定の解像度やフレーム レートをサポートするプロファイル、複数のカメラへの同時アクセスをサポートするプロファイル、HDR をサポートするプロファイルを選ぶなどのタスクが含まれます。 |
| [MediaCapture の形式、解像度、およびフレームレートを設定します。](set-media-encoding-properties.md) | [  **IMediaEncodingProperties**](https://docs.microsoft.com/uwp/api/Windows.Media.MediaProperties.IMediaEncodingProperties) インターフェイスを使用して、カメラのプレビュー ストリームとキャプチャした写真/ビデオの解像度およびフレーム レートを設定する方法を説明します。 プレビュー ストリームの縦横比をキャプチャしたメディアの縦横比と一致させる方法についても説明します。 |
| [HDR と低光の写真キャプチャ](high-dynamic-range-hdr-photo-capture.md) | [  **AdvancedPhotoCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.AdvancedPhotoCapture) クラスを使って、ハイ ダイナミック レンジ (HDR) 写真と低光量写真をキャプチャする方法について説明します。 |
| [写真とビデオのキャプチャのための手動カメラコントロール](capture-device-controls-for-photo-and-video-capture.md) | 光学式手ブレ補正やスムーズ ズームなど、写真とビデオのキャプチャに関する拡張シナリオを可能にするために、手動デバイス制御を使う方法について説明します。 |
| [ビデオキャプチャの手動カメラコントロール](capture-device-controls-for-video-capture.md) | この記事では、ビデオ キャプチャの拡張シナリオ (HDR ビデオ、露出の優先順位など) が手動デバイス制御によってどのように有効になるかを示します。  |
| [ビデオキャプチャのためのビデオ安定化効果](effects-for-video-capture.md) | ビデオ手ブレ補正効果を使う方法について説明します。  |
| [MediaCapture のシーン anlysis](scene-analysis-for-media-capture.md) | [  **SceneAnalysisEffect**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.SceneAnalysisEffect) と [**FaceDetectionEffect**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.FaceDetectionEffect) を使ってメディア キャプチャのプレビュー ストリームの内容を分析する方法について説明します。  |
| [VariablePhotoSequence を使用して写真シーケンスをキャプチャする](variable-photo-sequence.md) | 可変の写真シーケンスをキャプチャする方法について説明します。これによって、画像を複数のフレームとして次々とキャプチャし、各フレームに別々のフォーカス、フラッシュ、ISO、露出、露出補正の設定を適用することができます。  |
| [MediaFrameReader を使用してメディアフレームを処理する](process-media-frames-with-mediaframereader.md) | [  **MediaCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameReader) と共に [**MediaFrameReader**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture) を使って、色、深度、赤外線カメラ、オーディオ デバイスなどの 1 つ以上の利用可能なソースや、スケルタル トラッキング フレームを生成するようなカスタム フレーム ソースから、メディア フレームを取得する方法を示します。 この機能は、拡張現実アプリや奥行きを検出するカメラ アプリなど、メディア フレームのリアルタイム処理を実行するアプリで使用するために設計されました。  |
| [プレビューフレームを取得する](get-a-preview-frame.md) | メディア キャプチャのプレビュー ストリームから単一のプレビュー フレームを取得する方法について説明します。  |                                                                                                   


## <a name="uwp-app-samples-for-camera"></a>カメラ用の UWP アプリ サンプル

* [カメラの顔検出のサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraFaceDetection)
* [カメラプレビューフレームのサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraGetPreviewFrame)
* [カメラの HDR サンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraAdvancedCapture)
* [カメラの手動コントロールのサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraManualControls)
* [カメラプロファイルのサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraProfile)
* [カメラの解像度のサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraResolution)
* [カメラスタートキット](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraStarterKit)
* [カメラビデオ安定化のサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraVideoStabilization)

## <a name="related-topics"></a>関連トピック

* [オーディオ、ビデオ、およびカメラ](index.md)
 

 




