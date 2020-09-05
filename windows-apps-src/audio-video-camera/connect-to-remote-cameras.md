---
ms.assetid: ''
description: この記事では、リモートカメラに接続し、MediaFrameSourceGroup を取得して各カメラからフレームを取得する方法について説明します。
title: リモート カメラへの接続
ms.date: 04/19/2019
ms.topic: article
ms.custom: 19H1
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1fae76aea28ffb63f6cb0ad5af8c5eb6e3fac6e4
ms.sourcegitcommit: 8171695ade04a762f19723f0b88e46e407375800
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/05/2020
ms.locfileid: "89494368"
---
# <a name="connect-to-remote-cameras"></a>リモート カメラへの接続

この記事では、1つまたは複数のリモートカメラに接続して、各カメラからフレームを読み取ることができる [**Mediaframesourcegroup**](/uwp/api/Windows.Media.Capture.Frames.MediaFrameSourceGroup) オブジェクトを取得する方法について説明します。 メディアソースからのフレームの読み取りの詳細については、「 [MediaFrameReader を使用したメディアフレームの処理](process-media-frames-with-mediaframereader.md)」を参照してください。 デバイスとのペアリングの詳細については、「 [ペアリングデバイス](../devices-sensors/pair-devices.md)」を参照してください。

> [!NOTE] 
> この記事で説明されている機能は、Windows 10 バージョン1903以降で使用できます。

## <a name="create-a-devicewatcher-class-to-watch-for-available-remote-cameras"></a>使用可能なリモートカメラを監視する DeviceWatcher クラスを作成する

[**Devicewatcher**](/uwp/api/windows.devices.enumeration.devicewatcher)クラスは、アプリで使用可能なデバイスを監視し、デバイスが追加または削除されたときにアプリに通知します。 Devicewatcher のインスタンス**DeviceWatcher**を取得するには、 [**Devicewatcher. createwatcher**](/uwp/api/windows.devices.enumeration.deviceinformation.createwatcher#Windows_Devices_Enumeration_DeviceInformation_CreateWatcher_System_String_)を呼び出して、監視するデバイスの種類を識別する Advanced Query 構文 (aqs) 文字列を渡します。 ネットワークカメラデバイスを指定する AQS 文字列は、次のとおりです。

```syntax
@"System.Devices.InterfaceClassGuid:=""{B8238652-B500-41EB-B4F3-4234F7F5AE99}"" AND System.Devices.InterfaceEnabled:=System.StructuredQueryType.Boolean#True"
```

> [!NOTE] 
> ヘルパーメソッド [**Mediaframesourcegroup. GetDeviceSelector**](/uwp/api/windows.media.capture.frames.mediaframesourcegroup.getdeviceselector) は、ローカルに接続されたリモートネットワークカメラを監視する aqs 文字列を返します。 ネットワークカメラだけを監視するには、上に示した AQS 文字列を使用する必要があります。

[**Start**](/uwp/api/windows.devices.enumeration.devicewatcher.start)メソッドを呼び出すことによって返された**devicewatcher**を開始すると、現在使用可能なすべてのネットワークカメラに対して[**追加さ**](/uwp/api/windows.devices.enumeration.devicewatcher.added)れたイベントが発生します。 [**Stop**](/uwp/api/windows.devices.enumeration.devicewatcher.stop)を呼び出してウォッチャーを停止するまで、**追加**されたイベントは、新しいネットワークカメラデバイスが使用可能になったときに発生し、カメラデバイスが使用できなくなったときに[**削除**](/uwp/api/windows.devices.enumeration.devicewatcher.removed)されたイベントが発生します。

**追加**および**削除**されたイベントハンドラーに渡されるイベント引数は、それぞれ[**Deviceinformation**](/uwp/api/Windows.Devices.Enumeration.DeviceInformation)または[**deviceinformationupdate**](/uwp/api/windows.devices.enumeration.deviceinformationupdate)オブジェクトです。 これらの各オブジェクトには、イベントが発生したネットワークカメラの識別子である **Id** プロパティがあります。 この ID を [**mediaframesourcegroup. FromIdAsync**](/uwp/api/windows.media.capture.frames.mediaframesourcegroup.fromidasync) メソッドに渡して、カメラからフレームを取得するために使用できる [**mediaframesourcegroup**](/uwp/api/windows.media.capture.frames.mediaframesourcegroup.fromidasync) オブジェクトを取得します。

## <a name="remote-camera-pairing-helper-class"></a>リモートカメラペアリングヘルパークラス

次の例は、 **Devicewatcher**を使用して**Mediaframesourcegroup**オブジェクトの**system.collections.objectmodel.observablecollection**を作成および更新し、カメラのリストへのデータバインドをサポートするヘルパークラスを示しています。 一般的なアプリでは、カスタムモデルクラスに **Mediaframesourcegroup** がラップされます。 ヘルパークラスは、アプリの [**CoreDispatcher**](/uwp/api/Windows.UI.Core.CoreDispatcher) への参照を保持し、 [**runasync**](/uwp/api/windows.ui.core.coredispatcher.runasync) の呼び出し内のカメラのコレクションを更新して、コレクションにバインドされた ui が ui スレッドで確実に更新されるようにします。

また、この例では、**追加**および**削除**されたイベントに加えて、 [**devicewatcher も更新さ**](/uwp/api/windows.devices.enumeration.devicewatcher.updated)れたイベントを処理します。 **更新さ**れたハンドラーで、関連付けられているリモートカメラデバイスがから削除され、コレクションに戻されます。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/RemoteCameraPairingHelper.cs" id="SnippetRemoteCameraPairingHelper":::

```cppwinrt
#include <winrt/Windows.Devices.Enumeration.h>
#include <winrt/Windows.Foundation.Collections.h>
#include <winrt/Windows.Media.Capture.Frames.h>
#include <winrt/Windows.UI.Core.h>
using namespace winrt;
using namespace winrt::Windows::Devices::Enumeration;
using namespace winrt::Windows::Foundation::Collections;
using namespace winrt::Windows::Media::Capture::Frames;
using namespace winrt::Windows::UI::Core;

struct RemoteCameraPairingHelper
{
    RemoteCameraPairingHelper(CoreDispatcher uiDispatcher) :
        m_dispatcher(uiDispatcher)
    {
        m_remoteCameraCollection = winrt::single_threaded_observable_vector<MediaFrameSourceGroup>();
        auto remoteCameraAqs =
            LR"(System.Devices.InterfaceClassGuid:=""{B8238652-B500-41EB-B4F3-4234F7F5AE99}"")"
            LR"(AND System.Devices.InterfaceEnabled:=System.StructuredQueryType.Boolean#True)";
        m_watcher = DeviceInformation::CreateWatcher(remoteCameraAqs);
        m_watcherAddedAutoRevoker = m_watcher.Added(winrt::auto_revoke, { this, &RemoteCameraPairingHelper::Watcher_Added });
        m_watcherRemovedAutoRevoker = m_watcher.Removed(winrt::auto_revoke, { this, &RemoteCameraPairingHelper::Watcher_Removed });
        m_watcherUpdatedAutoRevoker = m_watcher.Updated(winrt::auto_revoke, { this, &RemoteCameraPairingHelper::Watcher_Updated });
        m_watcher.Start();
    }
    ~RemoteCameraPairingHelper()
    {
        m_watcher.Stop();
    }
    IObservableVector<MediaFrameSourceGroup> FrameSourceGroups()
    {
        return m_remoteCameraCollection;
    }
    winrt::fire_and_forget Watcher_Added(DeviceWatcher /* sender */, DeviceInformation args)
    {
        co_await AddDeviceAsync(args.Id());
    }
    winrt::fire_and_forget Watcher_Removed(DeviceWatcher /* sender */, DeviceInformationUpdate args)
    {
        co_await RemoveDevice(args.Id());
    }
    winrt::fire_and_forget Watcher_Updated(DeviceWatcher /* sender */, DeviceInformationUpdate args)
    {
        co_await RemoveDevice(args.Id());
        co_await AddDeviceAsync(args.Id());
    }
    Windows::Foundation::IAsyncAction AddDeviceAsync(winrt::hstring id)
    {
        auto group = co_await MediaFrameSourceGroup::FromIdAsync(id);
        if (group)
        {
            co_await m_dispatcher;
            m_remoteCameraCollection.Append(group);
        }
    }
    Windows::Foundation::IAsyncAction RemoveDevice(winrt::hstring id)
    {
        co_await m_dispatcher;

        uint32_t ix{ 0 };
        for (auto const&& item : m_remoteCameraCollection)
        {
            if (item.Id() == id)
            {
                m_remoteCameraCollection.RemoveAt(ix);
                break;
            }
            ++ix;
        }
    }

private:
    CoreDispatcher m_dispatcher{ nullptr };
    DeviceWatcher m_watcher{ nullptr };
    IObservableVector<MediaFrameSourceGroup> m_remoteCameraCollection;
    DeviceWatcher::Added_revoker m_watcherAddedAutoRevoker;
    DeviceWatcher::Removed_revoker m_watcherRemovedAutoRevoker;
    DeviceWatcher::Updated_revoker m_watcherUpdatedAutoRevoker;
};
```

## <a name="related-topics"></a>関連項目

* [カメラ](camera.md)
* [MediaCapture を使った基本的な写真、ビデオ、およびオーディオのキャプチャ](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [カメラ フレームのサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraFrames)
* [MediaFrameReader を使ったメディア フレームの処理](process-media-frames-with-mediaframereader.md)
