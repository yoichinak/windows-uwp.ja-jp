---
description: この記事では、AudioPlaybackConnection を使用して、Bluetooth に接続されたリモートデバイスがローカルコンピューターでオーディオを再生できるようにする方法について説明します。
title: Bluetooth 接続のリモート デバイスからオーディオ再生を有効にする
ms.date: 05/03/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 8a23758612b3c595f808fe2ffe4f38e558cf0740
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89163966"
---
# <a name="enable-audio-playback-from-remote-bluetooth-connected-devices"></a>Bluetooth 接続のリモート デバイスからオーディオ再生を有効にする

この記事では、 [Audioplaybackconnection](/uwp/api/windows.media.audio.audioplaybackconnection) を使用して、Bluetooth に接続されたリモートデバイスがローカルコンピューターでオーディオを再生できるようにする方法について説明します。

Windows 10 以降では、バージョン2004のリモートオーディオソースで Windows デバイスにオーディオをストリーミングできるため、PC を Bluetooth スピーカーのように動作させ、ユーザーが電話から音声を聞くことができるようにするなどのシナリオを実現できます。 実装では、OS の Bluetooth コンポーネントを使用して受信オーディオデータを処理し、システムのシステムのオーディオエンドポイント (内蔵 PC スピーカーや有線ヘッドホンなど) で再生します。 基になる Bluetooth A2DP sink の有効化は、システムではなく、エンドユーザーのシナリオを担当するアプリによって管理されます。

[Audioplaybackconnection](/uwp/api/windows.media.audio.audioplaybackconnection)クラスは、リモートデバイスからの接続を有効または無効にしたり、接続を作成したりするために使用されます。これにより、リモートオーディオの再生を開始できます。

## <a name="add-a-user-interface"></a>ユーザー インターフェイスの追加

この記事の例では、次の単純な XAML UI を使用します。ここでは、使用可能なリモートデバイスを表示する **ListView** コントロール、接続状態を表示する **TextBlock** 、および接続を有効、無効、および開くための3つのボタンを定義しています。

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioPlaybackConnectionExample/cs/MainPage.xaml" id="snippet_AudioPlaybackConnectionXAML":::

## <a name="use-devicewatcher-to-monitor-for-remote-devices"></a>DeviceWatcher を使用してリモートデバイスを監視する

[Devicewatcher](/uwp/api/windows.devices.enumeration.devicewatcher)クラスを使用すると、接続されているデバイスを検出できます。 [Audioplaybackconnection. GetDeviceSelector](/uwp/api/windows.media.audio.audioplaybackconnection.getdeviceselector)メソッドは、監視するデバイスの種類をデバイス監視に示す文字列を返します。 この文字列を **Devicewatcher** コンストラクターに渡します。 

デバイス監視が開始されたときに接続されているデバイスごとに、デバイス監視の実行中に接続されているデバイスごとに、 [devicewatcher. Added](/uwp/api/windows.devices.enumeration.devicewatcher.added) イベントが発生します。 以前に接続されていたデバイスが切断されると、 [Devicewatcher. 削除](/uwp/api/windows.devices.enumeration.devicewatcher.removed) イベントが発生します。 

[Devicewatcher を呼び出します。開始](/uwp/api/windows.devices.enumeration.devicewatcher.start)して、オーディオ再生接続をサポートしている接続デバイスの監視を開始します。 この例では、UI のメイン **グリッド** コントロールが読み込まれるときに、デバイスマネージャーを起動します。 **Devicewatcher**の使用方法の詳細については、「[デバイスの列挙](../devices-sensors/enumerate-devices.md)」を参照してください。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioPlaybackConnectionExample/cs/MainPage.xaml.cs" id="snippet_MainGridLoaded":::


デバイスウォッチャーの **追加** イベントでは、検出された各デバイスは [deviceinformation](/uwp/api/Windows.Devices.Enumeration.DeviceInformation) オブジェクトによって表されます。 検出された各デバイスを、UI の **ListView** コントロールにバインドされている観測可能なコレクションに追加します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioPlaybackConnectionExample/cs/MainPage.xaml.cs" id="snippet_DeclareDevices":::


:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioPlaybackConnectionExample/cs/MainPage.xaml.cs" id="snippet_DeviceWatcher_Added":::


## <a name="enable-and-release-audio-playback-connections"></a>オーディオ再生接続の有効化と解放

デバイスとの接続を開く前に、接続を有効にする必要があります。 これは、リモートデバイスのオーディオを PC で再生する新しいアプリケーションがあることをシステムに通知しますが、接続が開かれるまでオーディオの再生は開始されません。これは後の手順で示されます。

[ **オーディオ再生接続を有効にする** ] ボタンのクリックハンドラーで、 **ListView** コントロールで現在選択されているデバイスに関連付けられているデバイス ID を取得します。 この例では、有効になっている **Audioplaybackconnection** オブジェクトのディクショナリを保持します。 このメソッドは、まず、選択したデバイスの辞書にエントリが存在するかどうかを確認します。 次に、メソッドは、 [Trycreatefromid](/uwp/api/windows.media.audio.audioplaybackconnection.trycreatefromid)を呼び出して選択したデバイス ID を渡すことで、選択したデバイスに対して**Audioplaybackconnection**を作成しようとします。 

接続が正常に作成された場合は、新しい **Audioplaybackconnection** オブジェクトをアプリのディクショナリに追加し、オブジェクトの [StateChanged](/uwp/api/windows.media.audio.audioplaybackconnection.statechanged) イベントのハンドラーを登録します。さらに、新しい接続が有効になったことをシステムに通知するには、[startasync](/uwp/api/windows.media.audio.audioplaybackconnection.startasync) 呼び出します。 

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioPlaybackConnectionExample/cs/MainPage.xaml.cs" id="snippet_DeclareConnections":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioPlaybackConnectionExample/cs/MainPage.xaml.cs" id="snippet_EnableAudioPlaybackConnection":::


## <a name="open-the-audio-playback-connection"></a>オーディオ再生接続を開く

前の手順では、オーディオ再生接続が作成されましたが、 [Open](/uwp/api/windows.media.audio.audioplaybackconnection.open) または [openasync](/uwp/api/windows.media.audio.audioplaybackconnection.openasync)を呼び出すことによって接続が開かれるまで、サウンドの再生は開始されません。 [ **オーディオ再生接続を開く** ] ボタンをクリックし、[ハンドラー] をクリックして現在選択されているデバイスを取得し、ID を使用してアプリの接続の辞書から **Audioplaybackconnection** を取得します。 **Openasync**への呼び出しを待機し、返された[Audioplaybackconnectionopenresultstatus](/uwp/api/windows.media.audio.audioplaybackconnectionopenresult)オブジェクトの**状態**の値を確認して、接続が正常に開かれたかどうかを確認し、存在する場合は接続状態のテキストボックスを更新します。


:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioPlaybackConnectionExample/cs/MainPage.xaml.cs" id="snippet_OpenAudioPlaybackConnectionButton":::

## <a name="monitor-audio-playback-connection-state"></a>オーディオ再生の接続状態を監視する

接続の状態が変化するたびに、 [ConnectionStateChanged](/uwp/api/windows.media.audio.audioplaybackconnection.statechanged) イベントが発生します。 この例では、このイベントのハンドラーによって状態テキストボックスが更新されます。 Ui スレッドで更新が行われるようにするには、必ず、 [Dispatcher](/uwp/api/windows.ui.core.coredispatcher.runasync) の呼び出し内の ui を更新してください。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioPlaybackConnectionExample/cs/MainPage.xaml.cs" id="snippet_ConnectionStateChanged":::

## <a name="release-connections-and-handle-removed-devices"></a>接続を解放し、削除されたデバイスを処理する

この例では、[ **オーディオ再生接続のリリース** ] ボタンを使用して、ユーザーがオーディオ再生接続を解放できるようにします。 このイベントのハンドラーでは、現在選択されているデバイスを取得し、デバイスの ID を使用して、辞書内の **Audioplaybackconnection** を検索します。 **Dispose**を呼び出して参照を解放し、関連付けられているすべてのリソースを解放して、ディクショナリから接続を削除します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioPlaybackConnectionExample/cs/MainPage.xaml.cs" id="snippet_ReleaseAudioPlaybackConnectionButton":::

接続が有効になっているとき、または開いている間にデバイスが削除された場合は、そのケースを処理する必要があります。 これを行うには、デバイスウォッチャーの [devicewatcher](/uwp/api/windows.devices.enumeration.devicewatcher.removed) イベントのハンドラーを実装します。 まず、削除されたデバイスの ID を使用して、アプリの **ListView** コントロールにバインドされている観測可能なコレクションからデバイスを削除します。 次に、このデバイスに関連付けられている接続がアプリのディクショナリ内にある場合は、 **Dispose** が呼び出されて、関連付けられているリソースが解放され、その後、接続がディクショナリから削除されます。 これらはすべて、ui の更新が UI スレッドで実行されるようにするために、Dispatcher の呼び出し内で行われ**ます。**

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioPlaybackConnectionExample/cs/MainPage.xaml.cs" id="snippet_DeviceWatcher_Removed":::

## <a name="related-topics"></a>関連トピック

[メディアの再生](media-playback.md)


 