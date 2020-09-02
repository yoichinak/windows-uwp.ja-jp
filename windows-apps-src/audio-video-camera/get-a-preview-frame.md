---
ms.assetid: 05E418B4-5A62-42BD-BF66-A0762216D033
description: このトピックでは、メディア キャプチャのプレビュー ストリームから単一のプレビュー フレームを取得する方法について説明します。
title: プレビュー フレームの取得
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1a688ade1e8907cb0de0683df0751d1eebef4ed7
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/02/2020
ms.locfileid: "89362625"
---
# <a name="get-a-preview-frame"></a>プレビュー フレームの取得


このトピックでは、メディア キャプチャのプレビュー ストリームから単一のプレビュー フレームを取得する方法について説明します。

> [!NOTE] 
> この記事の内容は、写真やビデオの基本的なキャプチャ機能を実装するための手順を紹介した「[MediaCapture を使った基本的な写真、ビデオ、およびオーディオのキャプチャ](basic-photo-video-and-audio-capture-with-MediaCapture.md)」で取り上げた概念やコードに基づいています。 そちらの記事で基本的なメディア キャプチャのパターンを把握してから、高度なキャプチャ シナリオに進むことをお勧めします。 この記事で紹介しているコードは、MediaCapture のインスタンスが既に作成され適切に初期化されていることと、アクティブなビデオ プレビュー ストリームを含んだ [**CaptureElement**](/uwp/api/Windows.UI.Xaml.Controls.CaptureElement) が存在することを前提としています。

プレビュー フレームをキャプチャするためには、基本的なメディア キャプチャに必要な名前空間に加え、次の名前空間が必要となります。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetPreviewFrameUsing":::

プレビュー フレームを要求するとき、フレームの受信に使う形式を指定するには、必要な形式を指定して [**VideoFrame**](/uwp/api/Windows.Media.VideoFrame) オブジェクトを作成します。 この例では、[**VideoDeviceController.GetMediaStreamProperties**](/uwp/api/windows.media.devices.videodevicecontroller.getmediastreamproperties) の呼び出しで [**MediaStreamType.VideoPreview**](/uwp/api/Windows.Media.Capture.MediaStreamType) を指定し、プレビュー ストリームのプロパティを要求することで、プレビュー ストリームと同じ解像度でビデオ フレームを作成します。 プレビュー ストリームの幅と高さを使って新しいビデオ フレームを作成しています。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetCreateFormatFrame":::

[**MediaCapture**](/uwp/api/Windows.Media.Capture.MediaCapture) オブジェクトが初期化されていてアクティブなプレビュー ストリームが存在する場合、[**GetPreviewFrameAsync**](/uwp/api/windows.media.capture.mediacapture.getpreviewframeasync) を呼び出してプレビュー ストリームを取得します。 引数には、最後のステップで作成したビデオ フレームを渡し、取得するフレームの形式を指定します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetGetPreviewFrameAsync":::

プレビュー フレームの [**SoftwareBitmap**](/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) 表現は、[**VideoFrame**](/uwp/api/Windows.Media.VideoFrame) オブジェクトの [**SoftwareBitmap**](/uwp/api/windows.media.videoframe.softwarebitmap) プロパティにアクセスして取得します。 ソフトウェア ビットマップの保存、読み込み、変更については、「[イメージング](imaging.md)」をご覧ください。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetGetPreviewBitmap":::

Direct3D API で画像を扱う場合は、プレビュー フレームの [**IDirect3DSurface**](/uwp/api/Windows.Graphics.DirectX.Direct3D11.IDirect3DSurface) 表現を取得することもできます。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetGetPreviewSurface":::

> [!IMPORTANT]
> **GetPreviewFrameAsync** の呼び出し方法と、アプリが実行されているデバイスによっては、返される **VideoFrame** の [**SoftwareBitmap**](/uwp/api/windows.media.videoframe.softwarebitmap) プロパティまたは [**Direct3DSurface**](/uwp/api/windows.media.videoframe.direct3dsurface) プロパティのどちらかが null になることがあります。

> - **VideoFrame** 引数を受け入れる [**GetPreviewFrameAsync**](/uwp/api/windows.media.capture.mediacapture.getpreviewframeasync) のオーバーロードを呼び出した場合、返される **VideoFrame** の **SoftwareBitmap** は null 以外になり、**Direct3DSurface** プロパティは null になります。
> - Direct3D サーフェスを使ってフレームを内部で表すデバイスで引数のない [**GetPreviewFrameAsync**](/uwp/api/windows.media.capture.mediacapture.getpreviewframeasync) のオーバーロードを呼び出した場合、**Direct3DSurface** プロパティは null 以外になり、**SoftwareBitmap** プロパティは null になります。
> - Direct3D サーフェスを使ってフレームを内部で表すデバイスで引数のない [**GetPreviewFrameAsync**](/uwp/api/windows.media.capture.mediacapture.getpreviewframeasync) のオーバーロードを呼び出した場合、**SoftwareBitmap** プロパティは null 以外になり、**Direct3DSurface** プロパティは null になります。

アプリは、**SoftwareBitmap** プロパティまたは **Direct3DSurface** プロパティによって返されるオブジェクトで動作を試みる前に、必ず null 値をチェックする必要があります。

プレビュー フレームが不要になったら必ず、その [**Close**](/uwp/api/windows.media.videoframe.close) メソッド (C# 内で Dispose に投影される) を呼び出して、フレームによって使われているリソースを解放してください。 または、**using** パターンを使ってもかまいません。その場合は、オブジェクトが自動的に破棄されます。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetCleanUpPreviewFrame":::

## <a name="related-topics"></a>関連トピック

* [カメラ](camera.md)
* [MediaCapture を使った基本的な写真、ビデオ、およびオーディオのキャプチャ](basic-photo-video-and-audio-capture-with-MediaCapture.md)
 

 
