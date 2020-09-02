---
ms.assetid: 7DBEE5E2-C3EC-4305-823D-9095C761A1CD
description: この記事では、可変の写真シーケンスをキャプチャする方法について説明します。これによって、画像を複数のフレームとして次々とキャプチャし、各フレームに別々のフォーカス、フラッシュ、ISO、露出、露出補正の設定を適用することができます。
title: 可変の写真シーケンス
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 376d5cb011ab4a72d7715a36a1522ab91303c1f9
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363715"
---
# <a name="variable-photo-sequence"></a>可変の写真シーケンス



この記事では、可変の写真シーケンスをキャプチャする方法について説明します。これによって、画像を複数のフレームとして次々とキャプチャし、各フレームに別々のフォーカス、フラッシュ、ISO、露出、露出補正の設定を適用することができます。 この機能により、ハイ ダイナミック レンジ (HDR) 画像を作成するなどのシナリオが実現できます。

HDR 画像をキャプチャするときに、独自の処理アルゴリズムを実装しない場合は、[**AdvancedPhotoCapture**](/uwp/api/Windows.Media.Capture.AdvancedPhotoCapture) API を使って、Windows に組み込まれた HDR 機能を利用できます。 詳細については、「 [高ダイナミックレンジ (HDR) の写真キャプチャ](high-dynamic-range-hdr-photo-capture.md)」を参照してください。

> [!NOTE] 
> この記事の内容は、写真やビデオの基本的なキャプチャ機能を実装するための手順を紹介した「[MediaCapture を使った基本的な写真、ビデオ、およびオーディオのキャプチャ](basic-photo-video-and-audio-capture-with-MediaCapture.md)」で取り上げた概念やコードに基づいています。 そちらの記事で基本的なメディア キャプチャのパターンを把握してから、高度なキャプチャ シナリオに進むことをお勧めします。 この記事で紹介しているコードは、MediaCapture のインスタンスが既に作成され、適切に初期化されていることを前提としています。

## <a name="set-up-your-app-to-use-variable-photo-sequence-capture"></a>可変の写真シーケンス キャプチャを使うようにアプリを設定する

可変の写真シーケンス キャプチャを実装するためには、基本的なメディア キャプチャに必要な名前空間に加え、次の名前空間が必要となります。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetVPSUsing":::

写真シーケンス キャプチャを開始するために使用される [**VariablePhotoSequenceCapture**](/uwp/api/Windows.Media.Capture.Core.VariablePhotoSequenceCapture) オブジェクトを格納するためのメンバー変数を宣言します。 シーケンス内でキャプチャされた各画像を格納するための [**SoftwareBitmap**](/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) オブジェクトの配列を宣言します。 また、各フレームの [**CapturedFrameControlValues**](/uwp/api/Windows.Media.Capture.CapturedFrameControlValues) オブジェクトを格納するための配列も宣言します。 この配列は、画像処理アルゴリズムで、各フレームのキャプチャにどのような設定が使用されたかを確認するために使うことができます。 最後に、シーケンス内で現在キャプチャされているのがどの画像かを追跡するために使用される、インデックスを宣言します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetVPSMemberVariables":::

## <a name="prepare-the-variable-photo-sequence-capture"></a>可変の写真シーケンス キャプチャを準備する

[MediaCapture](./index.md) を初期化した後は、可変の写真シーケンスが現在のデバイスでサポートされていることを確認します。そのためには、[**VariablePhotoSequenceController**](/uwp/api/Windows.Media.Devices.Core.VariablePhotoSequenceController) のインスタンスをメディア キャプチャの [**VideoDeviceController**](/uwp/api/Windows.Media.Devices.VideoDeviceController) から取得し、[**Supported**](/uwp/api/windows.media.devices.core.variablephotosequencecontroller.supported) プロパティを調べます。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetIsVPSSupported":::

可変の写真シーケンスのコントローラーから [**FrameControlCapabilities**](/uwp/api/Windows.Media.Devices.Core.FrameControlCapabilities) オブジェクトを取得します。 このオブジェクトには、写真シーケンスのフレームごとに構成できるすべての設定のプロパティが含まれています。 これには以下が含まれます。

-   [**見る**](/uwp/api/windows.media.devices.core.framecontrolcapabilities.exposure)
-   [**ExposureCompensation**](/uwp/api/windows.media.devices.core.framecontrolcapabilities.exposurecompensation)
-   [**点滅**](/uwp/api/windows.media.devices.core.framecontrolcapabilities.flash)
-   [**対象**](/uwp/api/windows.media.devices.core.framecontrolcapabilities.focus)
-   [**IsoSpeed**](/uwp/api/windows.media.devices.core.framecontrolcapabilities.isospeed)
-   [**PhotoConfirmation**](/uwp/api/windows.media.devices.core.framecontrolcapabilities.photoconfirmationsupported)

この例では、フレームごとに異なる露出補正の値を設定します。 現在のデバイスの写真シーケンスで露出補正がサポートされているかどうかを確認するには、**ExposureCompensation** プロパティからアクセスできる [**FrameExposureCompensationCapabilities**](/uwp/api/Windows.Media.Devices.Core.FrameExposureCompensationCapabilities) オブジェクトの [**Supported**](/uwp/api/windows.media.devices.exposurecompensationcontrol.supported) プロパティを調べます。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetIsExposureCompensationSupported":::

キャプチャする各フレームに対して新しい [**FrameController**](/uwp/api/Windows.Media.Devices.Core.FrameController) オブジェクトを作成します。 この例では、3 つのフレームをキャプチャします。 フレームごとに変更するコントロールの値を設定します。 次に、**VariablePhotoSequenceController** の [**DesiredFrameControllers**](/uwp/api/windows.media.devices.core.variablephotosequencecontroller.desiredframecontrollers) コレクションをクリアし、コレクションに各フレーム コントローラーを追加します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetInitFrameControllers":::

キャプチャした画像に対して使用するエンコードを設定するための [**ImageEncodingProperties**](/uwp/api/Windows.Media.MediaProperties.ImageEncodingProperties) オブジェクトを作成します。 静的メソッド [**MediaCapture.PrepareVariablePhotoSequenceCaptureAsync**](/uwp/api/windows.media.capture.mediacapture.preparevariablephotosequencecaptureasync) を呼び出し、エンコード プロパティを渡します。 このメソッドは、[**VariablePhotoSequenceCapture**](/uwp/api/Windows.Media.Capture.Core.VariablePhotoSequenceCapture) オブジェクトを返します。 最後に、[**PhotoCaptured**](/uwp/api/windows.media.capture.core.variablephotosequencecapture.photocaptured) イベントと [**Stopped**](/uwp/api/windows.media.capture.core.variablephotosequencecapture.stopped) イベントのイベント ハンドラーを登録します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetPrepareVPS":::

## <a name="start-the-variable-photo-sequence-capture"></a>可変の写真シーケンス キャプチャを開始する

可変の写真シーケンスのキャプチャを開始するには、[**VariablePhotoSequenceCapture.StartAsync**](/uwp/api/windows.media.capture.core.variablephotosequencecapture.startasync) を呼び出します。 必ず、キャプチャした画像とフレーム コントロールの値を格納するための配列を初期化し、現在のインデックスを 0 に設定してください。 アプリの記録状態の変数を設定し、このキャプチャの進行中は別のキャプチャを開始できないように UI を更新します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetStartVPSCapture":::

## <a name="receive-the-captured-frames"></a>キャプチャされたフレームを受け取る

[**PhotoCaptured**](/uwp/api/windows.media.capture.core.variablephotosequencecapture.photocaptured) イベントは、キャプチャされたフレームごとに発生します。 フレーム コントロールの値とフレームのキャプチャされた画像を保存してから、現在のフレームのインデックスを増分します。 次の例は、各フレームの [**SoftwareBitmap**](/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) 表現を取得する方法を示しています。 **SoftwareBitmap** の使用方法について詳しくは、「[イメージング](imaging.md)」をご覧ください。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetOnPhotoCaptured":::

## <a name="handle-the-completion-of-the-variable-photo-sequence-capture"></a>可変の写真シーケンスのキャプチャの完了を処理する

[**Stopped**](/uwp/api/windows.media.capture.core.variablephotosequencecapture.stopped) イベントは、シーケンス内のすべてのフレームがキャプチャされると発生します。 アプリの記録状態を更新し、ユーザーが新しいキャプチャを開始できるように UI を更新します。 この時点で、キャプチャされた画像とフレーム コントロールの値を画像処理コードに渡すことができます。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetOnStopped":::

## <a name="update-frame-controllers"></a>フレーム コントローラーを更新する

フレームごとの設定を変更して、可変の写真シーケンス キャプチャを新たに実行する場合、**VariablePhotoSequenceCapture** を完全に再初期化する必要はありません。 [**DesiredFrameControllers**](/uwp/api/windows.media.devices.core.variablephotosequencecontroller.desiredframecontrollers) コレクションをクリアして新しいフレーム コントローラーを追加するか、または既存のフレーム コントローラーの値を変更できます。 次の例では、[**FrameFlashCapabilities**](/uwp/api/Windows.Media.Devices.Core.FrameFlashCapabilities) オブジェクトを調べて、現在のデバイスが可変の写真シーケンス フレームに対してフラッシュとフラッシュの電源をサポートしているかどうかを確認します。 サポートしている場合は、100% の電力でフラッシュを有効にするよう各フレームが更新されます。 各フレームに対して前の手順で設定した露出補正の値は、引き続き有効です。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetUpdateFrameControllers":::

## <a name="clean-up-the-variable-photo-sequence-capture"></a>可変の写真シーケンス キャプチャをクリーンアップする

可変の写真シーケンスのキャプチャが完了するか、アプリが中断された場合は、[**FinishAsync**](/uwp/api/windows.media.capture.core.variablephotosequencecapture.finishasync) を呼び出して、可変の写真シーケンスのオブジェクトをクリーンアップします。 オブジェクトのイベント ハンドラーの登録を解除し、null に設定します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetCleanUpVPS":::

## <a name="related-topics"></a>関連トピック

* [カメラ](camera.md)
* [MediaCapture を使った基本的な写真、ビデオ、およびオーディオのキャプチャ](basic-photo-video-and-audio-capture-with-MediaCapture.md)
 

 
