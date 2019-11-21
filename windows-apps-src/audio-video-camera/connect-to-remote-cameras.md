---
ms.assetid: ''
description: この記事では、リモートカメラに接続し、MediaFrameSourceGroup を取得して各カメラからフレームを取得する方法について説明します。
title: リモート カメラへの接続
ms.date: 04/19/2019
ms.topic: article
ms.custom: 19H1
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 253eea00ba6c4188197224111909c28a53932b88
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/20/2019
ms.locfileid: "74257350"
---
# <a name="connect-to-remote-cameras"></a>リモート カメラへの接続

この記事では、1つまたは複数のリモートカメラに接続して、各カメラからフレームを読み取ることができる[**Mediaframesourcegroup**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameSourceGroup)オブジェクトを取得する方法について説明します。 メディアソースからのフレームの読み取りの詳細については、「 [MediaFrameReader を使用したメディアフレームの処理](process-media-frames-with-mediaframereader.md)」を参照してください。 デバイスとのペアリングの詳細については、「[ペアリングデバイス](https://docs.microsoft.com/windows/uwp/devices-sensors/pair-devices)」を参照してください。

> [!NOTE] 
> この記事で説明されている機能は、Windows 10 バージョン1903以降でのみ使用できます。

## <a name="create-a-devicewatcher-class-to-watch-for-available-remote-cameras"></a>使用可能なリモートカメラを監視する DeviceWatcher クラスを作成する

[**Devicewatcher**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher)クラスは、アプリで使用可能なデバイスを監視し、デバイスが追加または削除されたときにアプリに通知します。 Devicewatcher のインスタンスを取得するには、 [**Devicewatcher. createwatcher**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.createwatcher#Windows_Devices_Enumeration_DeviceInformation_CreateWatcher_System_String_)を呼び出して、監視するデバイスの種類を識別する Advanced Query 構文 (aqs) 文字列を渡します。 ネットワークカメラデバイスを指定する AQS 文字列は、次のとおりです。

```
@"System.Devices.InterfaceClassGuid:=""{B8238652-B500-41EB-B4F3-4234F7F5AE99}"" AND System.Devices.InterfaceEnabled:=System.StructuredQueryType.Boolean#True"
```

> [!NOTE] 
> ヘルパーメソッド[**Mediaframesourcegroup. GetDeviceSelector**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourcegroup.getdeviceselector)は、ローカルに接続されたリモートネットワークカメラを監視する aqs 文字列を返します。 ネットワークカメラだけを監視するには、上に示した AQS 文字列を使用する必要があります。


[**Start**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.start)メソッドを呼び出すことによって返された**devicewatcher**を開始すると、現在使用可能なすべてのネットワークカメラに対して[**追加さ**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.added)れたイベントが発生します。 [**Stop**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.stop)を呼び出してウォッチャーを停止するまで、**追加**されたイベントは、新しいネットワークカメラデバイスが使用可能になったときに発生し、カメラデバイスが使用できなくなったときに[**削除**](https://docs.microsoft.com/en-us/uwp/api/windows.devices.enumeration.devicewatcher.removed)されたイベントが発生します。

**追加**および**削除**されたイベントハンドラーに渡されるイベント引数は、それぞれ[**Deviceinformation**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformation)または[**deviceinformationupdate**](https://docs.microsoft.com/en-us/uwp/api/windows.devices.enumeration.deviceinformationupdate)オブジェクトです。 これらの各オブジェクトには、イベントが発生したネットワークカメラの識別子である**Id**プロパティがあります。 この ID を[**mediaframesourcegroup. FromIdAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourcegroup.fromidasync)メソッドに渡して、カメラからフレームを取得するために使用できる[**mediaframesourcegroup**](https://docs.microsoft.com/en-us/uwp/api/windows.media.capture.frames.mediaframesourcegroup.fromidasync)オブジェクトを取得します。

## <a name="remote-camera-pairing-helper-class"></a>リモートカメラペアリングヘルパークラス

次の例は、 **Devicewatcher**を使用して**Mediaframesourcegroup**オブジェクトの**system.collections.objectmodel.observablecollection**を作成および更新し、カメラのリストへのデータバインドをサポートするヘルパークラスを示しています。 一般的なアプリでは、カスタムモデルクラスに**Mediaframesourcegroup**がラップされます。 ヘルパークラスは、アプリの[**CoreDispatcher**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreDispatcher)への参照を保持し、 [**runasync**](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.runasync)の呼び出し内のカメラのコレクションを更新して、コレクションにバインドされた ui が ui スレッドで確実に更新されるようにします。

また、この例では、**追加**および**削除**されたイベントに加えて、 [**devicewatcher も更新さ**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.updated)れたイベントを処理します。 **更新さ**れたハンドラーで、関連付けられているリモートカメラデバイスがから削除され、コレクションに戻されます。

[!code-cs[SnippetRemoteCameraPairingHelper](./code/Frames_Win10/Frames_Win10/RemoteCameraPairingHelper.cs#SnippetRemoteCameraPairingHelper)]


## <a name="related-topics"></a>関連トピック

* [カメラ](camera.md)
* [MediaCapture を使用した基本的な写真、ビデオ、オーディオキャプチャ](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [カメラフレームのサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraFrames)
* [MediaFrameReader を使用してメディアフレームを処理する](process-media-frames-with-mediaframereader.md)
 

 




