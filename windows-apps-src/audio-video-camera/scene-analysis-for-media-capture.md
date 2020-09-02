---
ms.assetid: B5D915E4-4280-422C-BA0E-D574C534410B
description: この記事では、SceneAnalysisEffect と FaceDetectionEffect を使ってメディア キャプチャのプレビュー ストリームの内容を分析する方法について説明します。
title: カメラ フレームの分析の効果
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 56f828268980eb7ccc63a84729365bb5d17a609c
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363755"
---
# <a name="effects-for-analyzing-camera-frames"></a>カメラ フレームの分析の効果



この記事では、 [**SceneAnalysisEffect**](/uwp/api/Windows.Media.Core.SceneAnalysisEffect) と [**FaceDetectionEffect**](/uwp/api/Windows.Media.Core.FaceDetectionEffect) を使用して、media capture プレビューストリームのコンテンツを分析する方法について説明します。

## <a name="scene-analysis-effect"></a>シーン分析効果

[**SceneAnalysisEffect**](/uwp/api/Windows.Media.Core.SceneAnalysisEffect) は、メディア キャプチャのプレビュー ストリームに含まれるビデオ フレームを分析し、キャプチャ結果を向上させるための処理オプションを推奨します。 現時点では、ハイ ダイナミック レンジ (HDR) 処理を使用してキャプチャを向上できるかどうかの検出がサポートされています。

HDR の使用が推奨された場合は、次の方法で実行できます。

-   [**AdvancedPhotoCapture**](/uwp/api/Windows.Media.Capture.AdvancedPhotoCapture) クラスで、Windows に組み込まれている HDR 処理アルゴリズムを使って写真をキャプチャします。 詳細については、「 [高ダイナミックレンジ (HDR) の写真キャプチャ](high-dynamic-range-hdr-photo-capture.md)」を参照してください。

-   [**HdrVideoControl**](/uwp/api/Windows.Media.Devices.HdrVideoControl) で、Windows に組み込まれている HDR 処理アルゴリズムを使ってビデオをキャプチャします。 詳しくは、「[ビデオ キャプチャのためのキャプチャ デバイス コントロール](capture-device-controls-for-video-capture.md)」をご覧ください。

-   [**VariablePhotoSequenceControl**](/uwp/api/Windows.Media.Devices.Core.VariablePhotoSequenceController) でフレームのシーケンスをキャプチャし、カスタム HDR 実装を使って合成します。 詳細については、「 [可変フォトシーケンス](variable-photo-sequence.md)」を参照してください。

### <a name="scene-analysis-namespaces"></a>シーン分析の名前空間

シーン分析を使うには、基本的なメディア キャプチャに必要な名前空間に加え、次の名前空間をアプリに追加する必要があります。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetSceneAnalysisUsing":::

### <a name="initialize-the-scene-analysis-effect-and-add-it-to-the-preview-stream"></a>シーン分析効果を初期化してプレビュー ストリームに追加する

ビデオ効果は、2 つの API を使って実装されます。1 つは効果の定義であり、キャプチャ デバイスで効果を初期化するために必要となる設定を提供します。もう 1 つは効果のインスタンスであり、効果を制御するために使用できます。 効果のインスタンスには、コードのいたるところからアクセスする必要があるので、通常はそのオブジェクトを保持するメンバー変数を宣言する必要があります。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetDeclareSceneAnalysisEffect":::

アプリで、**MediaCapture** オブジェクトを初期化した後、[**SceneAnalysisEffectDefinition**](/uwp/api/Windows.Media.Core.SceneAnalysisEffectDefinition) の新しいインスタンスを作成します。

**MediaCapture** オブジェクトに対して [**AddVideoEffectAsync**](/uwp/api/windows.media.capture.mediacapture.addvideoeffectasync) を呼び出すことで、効果をキャプチャ デバイスに登録します。このとき、**SceneAnalysisEffectDefinition** を指定し、さらに [**MediaStreamType.VideoPreview**](/uwp/api/Windows.Media.Capture.MediaStreamType) を指定することで、効果をキャプチャ ストリームではなくビデオ プレビュー ストリームに適用することを示します。 **AddVideoEffectAsync** は追加されたエフェクトのインスタンスを返します。 このメソッドは複数の種類の効果について使用できるため、返されたインスタンスは [**SceneAnalysisEffect**](/uwp/api/Windows.Media.Core.SceneAnalysisEffect) オブジェクトにキャストする必要があります。

シーン分析の結果を受け取るには、[**SceneAnalyzed**](/uwp/api/windows.media.core.sceneanalysiseffect.sceneanalyzed) イベントのハンドラーを登録する必要があります。

現時点では、シーン分析効果にはハイ ダイナミック レンジ アナライザーのみが含まれます。 効果の [**HighDynamicRangeControl.Enabled**](/uwp/api/windows.media.core.highdynamicrangecontrol.enabled) を true に設定して、HDR 分析を有効にします。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetCreateSceneAnalysisEffectAsync":::

### <a name="implement-the-sceneanalyzed-event-handler"></a>SceneAnalyzed イベント ハンドラーを実装する

シーン分析の結果は、**SceneAnalyzed** イベント ハンドラーに返されます。 ハンドラーに渡された [**SceneAnalyzedEventArgs**](/uwp/api/Windows.Media.Core.SceneAnalyzedEventArgs) オブジェクトには、[**SceneAnalysisEffectFrame**](/uwp/api/Windows.Media.Core.SceneAnalysisEffectFrame) オブジェクトが含まれ、これには [**HighDynamicRangeOutput**](/uwp/api/Windows.Media.Core.HighDynamicRangeOutput) オブジェクトが含まれています。 ハイ ダイナミック レンジ出力の [**Certainty**](/uwp/api/windows.media.core.highdynamicrangeoutput.certainty) プロパティの値は、0 ～ 1.0 となります。0 は、HDR 処理によってキャプチャの結果が向上しないことを示します。1.0 は、HDR 処理によって結果が向上することを示します。 HDR を使うかどうかのしきい値を決めておくか、またはユーザーに結果を表示してユーザーが決めるようにすることもできます。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetSceneAnalyzed":::

ハンドラーに渡された [**HighDynamicRangeOutput**](/uwp/api/Windows.Media.Core.HighDynamicRangeOutput) オブジェクトには、[**FrameControllers**](/uwp/api/windows.media.core.highdynamicrangeoutput.framecontrollers) プロパティもあります。このプロパティは、HDR 処理用の可変の写真シーケンスをキャプチャするために推奨されるフレーム コントローラーを指定します。 詳細については、「 [可変フォトシーケンス](variable-photo-sequence.md)」を参照してください。

### <a name="clean-up-the-scene-analysis-effect"></a>シーン分析効果をクリーンアップする

キャプチャが終了したら、**MediaCapture** オブジェクトを破棄する前に、効果の [**HighDynamicRangeAnalyzer.Enabled**](/uwp/api/windows.media.core.highdynamicrangecontrol.enabled) プロパティを false に設定してシーン分析効果を無効にし、[**SceneAnalyzed**](/uwp/api/windows.media.core.sceneanalysiseffect.sceneanalyzed) イベント ハンドラーの登録を解除する必要があります。 効果が追加されたのはビデオ プレビュー ストリームであるため、ビデオ プレビュー ストリームを指定して [**MediaCapture.ClearEffectsAsync**](/uwp/api/windows.media.capture.mediacapture.cleareffectsasync) を呼び出します。 最後に、メンバー変数を null に設定します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetCleanUpSceneAnalysisEffectAsync":::

## <a name="face-detection-effect"></a>顔検出効果

[**FaceDetectionEffect**](/uwp/api/Windows.Media.Core.FaceDetectionEffect) は、メディア キャプチャのプレビュー ストリーム内で顔の位置を特定します。 この効果によって、プレビュー ストリーム内で顔が検出されたときに通知を受け取ることができ、プレビュー フレーム内で検出された顔ごとに境界ボックスが表示されます。 サポートされているデバイスでは、シーン内の最も重要な顔に対して露出の強化やフォーカスが行われます。

### <a name="face-detection-namespaces"></a>顔検出の名前空間

顔検出を使うには、基本的なメディア キャプチャに必要な名前空間に加え、次の名前空間をアプリに追加する必要があります。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetFaceDetectionUsing":::

### <a name="initialize-the-face-detection-effect-and-add-it-to-the-preview-stream"></a>顔検出効果を初期化してプレビュー ストリームに追加する

ビデオ効果は、2 つの API を使って実装されます。1 つは効果の定義であり、キャプチャ デバイスで効果を初期化するために必要となる設定を提供します。もう 1 つは効果のインスタンスであり、効果を制御するために使用できます。 効果のインスタンスには、コードのいたるところからアクセスする必要があるので、通常はそのオブジェクトを保持するメンバー変数を宣言する必要があります。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetDeclareFaceDetectionEffect":::

アプリで、**MediaCapture** オブジェクトを初期化した後、[**FaceDetectionEffectDefinition**](/uwp/api/Windows.Media.Core.FaceDetectionEffectDefinition) の新しいインスタンスを作成します。 [**DetectionMode**](/uwp/api/windows.media.core.facedetectioneffectdefinition.detectionmode) プロパティを設定して、より高速な顔検出とより正確な顔検出のどちらを優先するかを決めます。 顔検出が完了するまで待機して着信フレームが遅延すると、プレビューがぎくしゃくした感じになる場合があるため、[**SynchronousDetectionEnabled**](/uwp/api/windows.media.core.facedetectioneffectdefinition.synchronousdetectionenabled) を設定して、顔検出を待機しないよう指定します。

**MediaCapture** オブジェクトに対して [**AddVideoEffectAsync**](/uwp/api/windows.media.capture.mediacapture.addvideoeffectasync) を呼び出すことで、効果をキャプチャ デバイスに登録します。このとき、**FaceDetectionEffectDefinition** を指定し、さらに [**MediaStreamType.VideoPreview**](/uwp/api/Windows.Media.Capture.MediaStreamType) を指定することで、効果をキャプチャ ストリームではなくビデオ プレビュー ストリームに適用することを示します。 **AddVideoEffectAsync** は追加されたエフェクトのインスタンスを返します。 このメソッドは複数の種類の効果について使用できるため、返されたインスタンスは [**FaceDetectionEffect**](/uwp/api/Windows.Media.Core.FaceDetectionEffect) オブジェクトにキャストする必要があります。

[**FaceDetectionEffect.Enabled**](/uwp/api/windows.media.core.facedetectioneffect.enabled) プロパティを設定して、効果を有効または無効にします。 [**FaceDetectionEffect.DesiredDetectionInterval**](/uwp/api/windows.media.core.facedetectioneffect.desireddetectioninterval) プロパティを設定して、フレームを分析する頻度を調整します。 これらのプロパティはいずれも、メディア キャプチャの進行中に調整できます。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetCreateFaceDetectionEffectAsync":::

### <a name="receive-notifications-when-faces-are-detected"></a>顔が検出されたときに通知を受け取る

顔が検出されたときに、ビデオ プレビュー内で検出された顔の周りにボックスを描画するなど、特定の操作を実行するには、[**FaceDetected**](/uwp/api/windows.media.core.facedetectioneffect.facedetected) イベントに登録します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetRegisterFaceDetectionHandler":::

イベントのハンドラーで、[**FaceDetectedEventArgs**](/uwp/api/Windows.Media.Core.FaceDetectedEventArgs) の [**FaceDetectionEffectFrame.DetectedFaces**](/uwp/api/windows.media.core.facedetectioneffectframe.detectedfaces) プロパティにアクセスすることにより、フレームで検出されたすべての顔の一覧を取得できます。 [**FaceBox**](/uwp/api/windows.media.faceanalysis.detectedface.facebox) プロパティは、プレビュー ストリームのサイズを基準とした単位で検出された顔を含む四角形を描画する [**BitmapBounds**](/uwp/api/Windows.Graphics.Imaging.BitmapBounds) 構造体です。 プレビュー ストリームの座標を画面座標に変換するサンプル コードについては、「[顔検出 UWP のサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraFaceDetection)」をご覧ください。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetFaceDetected":::

### <a name="clean-up-the-face-detection-effect"></a>顔検出効果をクリーンアップする

キャプチャが終了したら、**MediaCapture** オブジェクトを破棄する前に、[**FaceDetectionEffect.Enabled**](/uwp/api/windows.media.core.facedetectioneffect.enabled) で顔検出効果を無効にし、[**FaceDetected**](/uwp/api/windows.media.core.facedetectioneffect.facedetected) イベント ハンドラーを登録していた場合は登録を解除する必要があります。 効果が追加されたのはビデオ プレビュー ストリームであるため、ビデオ プレビュー ストリームを指定して [**MediaCapture.ClearEffectsAsync**](/uwp/api/windows.media.capture.mediacapture.cleareffectsasync) を呼び出します。 最後に、メンバー変数を null に設定します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetCleanUpFaceDetectionEffectAsync":::

### <a name="check-for-focus-and-exposure-support-for-detected-faces"></a>検出された顔に対するフォーカスや露出のサポートを確認する

すべてのデバイスに、検出された顔に基づいてフォーカスや露出を調整できるキャプチャ デバイスが搭載されているとは限りません。 顔検出はデバイス リソースを消費するため、キャプチャを強化する機能を使用できるデバイスでのみ顔検出を有効にすることもできます。 顔に基づくキャプチャの最適化が利用可能かどうかを確認するには、初期化された [MediaCapture](./index.md) に対する [**VideoDeviceController**](/uwp/api/Windows.Media.Devices.VideoDeviceController) を取得してから、ビデオ デバイス コントローラーの [**RegionsOfInterestControl**](/uwp/api/Windows.Media.Devices.RegionsOfInterestControl) を取得します。 [**MaxRegions**](/uwp/api/windows.media.devices.regionsofinterestcontrol.maxregions) で少なくとも 1 つの領域がサポートされているかどうかを確認します。 次に、[**AutoExposureSupported**](/uwp/api/windows.media.devices.regionsofinterestcontrol.autoexposuresupported) または [**AutoFocusSupported**](/uwp/api/windows.media.devices.regionsofinterestcontrol.autofocussupported) のいずれかが true であるかどうかを確認します。 これらの条件が満たされている場合は、デバイスで顔検出を利用してキャプチャを強化できます。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetAreFaceFocusAndExposureSupported":::

## <a name="related-topics"></a>関連トピック

* [カメラ](camera.md)
* [MediaCapture を使った基本的な写真、ビデオ、およびオーディオのキャプチャ](basic-photo-video-and-audio-capture-with-MediaCapture.md)
 

 
