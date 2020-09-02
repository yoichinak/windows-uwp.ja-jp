---
ms.assetid: EFCF84D0-2F4C-454D-97DA-249E9EAA806C
description: SystemMediaTransportControls クラスを使うと、Windows に組み込まれているシステム メディア トランスポート コントロールをアプリで使って、現在アプリで再生中のメディアに関してコントロールに表示されるメタデータを更新できるようになります。
title: システム メディア トランスポート コントロールの手動制御
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d592b516db32c2602c8b51d82f3ea56c037e5164
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363785"
---
# <a name="manual-control-of-the-system-media-transport-controls"></a>システム メディア トランスポート コントロールの手動制御


Windows 10 バージョン 1607 以降、[**MediaPlayer**](/uwp/api/Windows.Media.Playback.MediaPlayer) クラスを使ってメディアを再生する UWP アプリは、既定でシステム メディア トランスポート コントロール (SMTC) と自動的に統合されます。 これは、ほとんどのシナリオの SMTC と対話するための推奨される方法です。 SMTC の **MediaPlayer** との既定の統合のカスタマイズについて詳しくは、「[システム メディア トランスポート コントロールとの統合](integrate-with-systemmediatransportcontrols.md)」をご覧ください。

SMTC の手動コントロールの実装が必要になるシナリオがいくつかあります。 たとえば、[**MediaTimelineController**](/uwp/api/Windows.Media.MediaTimelineController) を使って 1 つ以上のメディア プレーヤーの再生を制御する場合です。 複数のメディア プレーヤーを使用していて、アプリ用に SMTC の 1 つのインスタンスだけが必要な場合もそうです。 [**MediaElement**](/uwp/api/Windows.UI.Xaml.Controls.MediaElement) を使ってメディアを再生する場合は、SMTC を手動で制御する必要があります。

## <a name="set-up-transport-controls"></a>トランスポート コントロールをセットアップする
**MediaPlayer** 使ってメディアを再生している場合は、[**MediaPlayer.SystemMediaTransportControls**](/uwp/api/windows.media.playback.mediaplayer.systemmediatransportcontrols) プロパティにアクセスして [**SystemMediaTransportControls**](/uwp/api/Windows.Media.SystemMediaTransportControls) のインスタンスを取得できます。 SMTC を手動で制御する場合は、[**CommandManager.IsEnabled**](/uwp/api/windows.media.playback.mediaplaybackcommandmanager.isenabled) プロパティを false に設定して、**MediaPlayer** によって提供される自動統合を無効にする必要があります。

> [!NOTE] 
> [**IsEnabled**](/uwp/api/windows.media.playback.mediaplaybackcommandmanager.isenabled) を false に設定して、[**MediaPlayer**](/uwp/api/Windows.Media.Playback.MediaPlayer) の [**MediaPlaybackCommandManager**](/uwp/api/Windows.Media.Playback.MediaPlaybackCommandManager) を無効にすると、**MediaPlayerElement** で提供される **MediaPlayer** と [**TransportControls**](/uwp/api/windows.ui.xaml.controls.mediaplayerelement.transportcontrols) の間のリンクが解除されます。このため組み込みトランスポート コントロールはプレーヤーの再生を自動的に制御しなくなります。 代わりに、独自のコントロールを実装して、**MediaPlayer** を制御する必要があります。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SMTCWin10/cs/MainPage.xaml.cs" id="SnippetInitSMTCMediaPlayer":::

[**GetForCurrentView**](/uwp/api/windows.media.systemmediatransportcontrols.getforcurrentview) を呼び出して、[**SystemMediaTransportControls**](/uwp/api/Windows.Media.SystemMediaTransportControls) のインスタンスを取得することもできます。 **MediaElement** を使ってメディアを再生している場合は、この方法でオブジェクトを取得する必要があります。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SMTCWin10/cs/MainPage.xaml.cs" id="SnippetInitSMTCMediaElement":::

**SystemMediaTransportControls** オブジェクトの対応する "is enabled" プロパティ ([**IsPlayEnabled**](/uwp/api/windows.media.systemmediatransportcontrols.isplayenabled)、[**IsPauseEnabled**](/uwp/api/windows.media.systemmediatransportcontrols.ispauseenabled)、[**IsNextEnabled**](/uwp/api/windows.media.systemmediatransportcontrols.isnextenabled)、[**IsPreviousEnabled**](/uwp/api/windows.media.systemmediatransportcontrols.ispreviousenabled) など) を設定することで、アプリで使うボタンを有効にします。 利用可能なすべてのコントロールの一覧については、**SystemMediaTransportControls** のリファレンス ドキュメントをご覧ください。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SMTCWin10/cs/MainPage.xaml.cs" id="SnippetEnableContols":::

ユーザーがボタンを押したときに通知を受信するために、[**ButtonPressed**](/uwp/api/windows.media.systemmediatransportcontrols.buttonpressed) イベントのハンドラーを登録します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SMTCWin10/cs/MainPage.xaml.cs" id="SnippetRegisterButtonPressed":::

## <a name="handle-system-media-transport-controls-button-presses"></a>システム メディア トランスポート コントロールの押されたボタンを処理する

[**ButtonPressed**](/uwp/api/windows.media.systemmediatransportcontrols.buttonpressed) イベントは、有効なボタンの 1 つが押されたときにシステム トランスポート コントロールで発生します。 イベント ハンドラーに渡される [**SystemMediaTransportControlsButtonPressedEventArgs**](/uwp/api/Windows.Media.SystemMediaTransportControlsButtonPressedEventArgs) の [**Button**](/uwp/api/windows.media.systemmediatransportcontrolsbuttonpressedeventargs.button) プロパティは、有効なボタンのうちどれが押されたかを示す [**SystemMediaTransportControlsButton**](/uwp/api/Windows.Media.SystemMediaTransportControlsButton) 列挙体のメンバーです。

[**ButtonPressed**](/uwp/api/windows.media.systemmediatransportcontrols.buttonpressed) イベント ハンドラーから UI スレッド上のオブジェクト ([**MediaElement**](/uwp/api/Windows.UI.Xaml.Controls.MediaElement) オブジェクトなど) を更新するには、[**CoreDispatcher**](/uwp/api/Windows.UI.Core.CoreDispatcher) を介して呼び出しをマーシャリングする必要があります。 これは、**ButtonPressed** イベント ハンドラーが UI スレッドから呼び出されず、UI を直接変更しようとすると例外が発生するためです。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SMTCWin10/cs/MainPage.xaml.cs" id="SnippetSystemMediaTransportControlsButtonPressed":::

## <a name="update-the-system-media-transport-controls-with-the-current-media-status"></a>現在のメディアの状態でシステム メディア トランスポート コントロールを更新する

システムで現在の状態を反映してコントロールを更新できるように、メディアの状態が変更されたときには、それを [**SystemMediaTransportControls**](/uwp/api/Windows.Media.SystemMediaTransportControls) に通知する必要があります。 そのためには、メディアの状態が変更されたときに発生する [**MediaElement**](/uwp/api/Windows.UI.Xaml.Controls.MediaElement) の [**CurrentStateChanged**](/uwp/api/windows.ui.xaml.controls.mediaelement.currentstatechanged) イベント内から、[**PlaybackStatus**](/uwp/api/windows.media.systemmediatransportcontrols.playbackstatus) プロパティを適切な [**MediaPlaybackStatus**](/uwp/api/Windows.Media.MediaPlaybackStatus) 値に設定します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SMTCWin10/cs/MainPage.xaml.cs" id="SnippetSystemMediaTransportControlsStateChange":::

## <a name="update-the-system-media-transport-controls-with-media-info-and-thumbnails"></a>メディアの情報と縮小表示でシステム メディア トランスポート コントロールを更新する

[**SystemMediaTransportControlsDisplayUpdater**](/uwp/api/Windows.Media.SystemMediaTransportControlsDisplayUpdater) クラスを使って、現在再生されているメディア項目の曲のタイトルやアルバム アートなど、トランスポート コントロールに表示されるメディア情報を更新します。 [**SystemMediaTransportControls.DisplayUpdater**](/uwp/api/windows.media.systemmediatransportcontrols.displayupdater) プロパティを使って、このクラスのインスタンスを取得します。 一般的なシナリオの場合、メタデータを渡すための推奨される方法は、現在再生中のメディア ファイルを渡して [**CopyFromFileAsync**](/uwp/api/windows.media.systemmediatransportcontrolsdisplayupdater.copyfromfileasync) を呼び出すことです。 表示アップデーターによって、ファイルからメタデータと縮小表示画像が自動的に抽出されます。

[**Update**](/uwp/api/windows.media.systemmediatransportcontrolsdisplayupdater.update) を呼び出すことで、システム メディア トランスポート コントロールの UI が新しいメタデータと縮小表示によって更新されます。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SMTCWin10/cs/MainPage.xaml.cs" id="SnippetSystemMediaTransportControlsUpdater":::

必要に応じて、[**DisplayUpdater**](/uwp/api/windows.media.systemmediatransportcontrols.displayupdater) クラスによって公開される [**MusicProperties**](/uwp/api/windows.media.systemmediatransportcontrolsdisplayupdater.musicproperties)、[**ImageProperties**](/uwp/api/windows.media.systemmediatransportcontrolsdisplayupdater.imageproperties)、または [**VideoProperties**](/uwp/api/windows.media.systemmediatransportcontrolsdisplayupdater.videoproperties) オブジェクトの値を設定することにより、システム メディア トランスポート コントロールに表示されるメタデータを手動で更新できます。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SMTCWin10/cs/MainPage.xaml.cs" id="SystemMediaTransportControlsUpdaterManual":::

> [!Note]
> アプリは、システムメディアトランスポートコントロールによって表示される他のメディアメタデータを提供していない場合でも、 [SystemMediaTransportControlsDisplayUpdater](/uwp/api/windows.media.systemmediatransportcontrolsdisplayupdater.type#Windows_Media_SystemMediaTransportControlsDisplayUpdater_Type
) プロパティの値を設定する必要があります。 この値は、再生中にスクリーンセーバーがアクティブ化されないようにするなど、メディアコンテンツを正しく処理するのに役立ちます。


## <a name="update-the-system-media-transport-controls-timeline-properties"></a>システム メディア トランスポート コントロールのタイムライン プロパティを更新する

システム トランスポート コントロールには、メディア項目の現在の再生位置、開始時刻、終了時刻など、現在再生中のメディア項目のタイムラインに関する情報が表示されます。 システム メディア トランスポート コントロールのタイムライン プロパティを更新するには、新しい [**SystemMediaTransportControlsTimelineProperties**](/uwp/api/Windows.Media.SystemMediaTransportControlsTimelineProperties) オブジェクトを作成します。 再生中のメディア項目の現在の状態を反映するように、オブジェクトのプロパティを設定します。 [**SystemMediaTransportControls.UpdateTimelineProperties**](/uwp/api/windows.media.systemmediatransportcontrols.updatetimelineproperties) を呼び出して、コントロールのタイムラインを更新します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SMTCWin10/cs/MainPage.xaml.cs" id="SnippetUpdateTimelineProperties":::

-   再生中の項目のタイムラインをシステム コントロールに表示するには、[**StartTime**](/uwp/api/windows.media.systemmediatransportcontrolstimelineproperties.starttime)、[**EndTime**](/uwp/api/windows.media.systemmediatransportcontrolstimelineproperties.endtime)、および [**Position**](/uwp/api/windows.media.systemmediatransportcontrols.playbackpositionchangerequested) の値を指定する必要があります。

-   [**MinSeekTime**](/uwp/api/windows.media.systemmediatransportcontrolstimelineproperties.minseektime) と [**MaxSeekTime**](/uwp/api/windows.media.systemmediatransportcontrolstimelineproperties.maxseektime) を使うと、ユーザーがシークできるタイムライン内の範囲を指定できます。 一般的なシナリオとしては、コンテンツ プロバイダーがメディアに広告を含める場合などがあります。

    [**PositionChangeRequest**](/uwp/api/windows.media.systemmediatransportcontrols.playbackpositionchangerequested) を発生させるには、[**MinSeekTime**](/uwp/api/windows.media.systemmediatransportcontrolstimelineproperties.minseektime) と [**MaxSeekTime**](/uwp/api/windows.media.systemmediatransportcontrolstimelineproperties.maxseektime) を設定する必要があります。

-   システム コントロールは、メディアの再生と同期させておくことをお勧めします。そのためには、再生中にこれらのプロパティを約 5 秒ごとに更新し、一時停止や新しい位置へのシークなど、再生に変更が加えられたときには、再度更新します。

## <a name="respond-to-player-property-changes"></a>プレーヤーのプロパティの変更に応答する

再生中のメディア項目の状態ではなく、メディア プレーヤー自体の現在の状態に関連した、一連のシステム トランスポート コントロール プロパティがあります。 これらの各プロパティは、ユーザーが関連するコントロールを調整したときに発生するイベントと対応しています。 これらのプロパティとイベントを次に示します。

| プロパティ                                                                  | Event                                                                                                   |
|---------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------|
| [**AutoRepeatMode**](/uwp/api/windows.media.systemmediatransportcontrols.autorepeatmode) | [**AutoRepeatModeChangeRequested**](/uwp/api/windows.media.systemmediatransportcontrols.autorepeatmodechangerequested) |
| [**PlaybackRate**](/uwp/api/windows.media.systemmediatransportcontrols.playbackrate)     | [**PlaybackRateChangeRequested**](/uwp/api/windows.media.systemmediatransportcontrols.playbackratechangerequested)     |
| [**ShuffleEnabled**](/uwp/api/windows.media.systemmediatransportcontrols.shuffleenabled) | [**ShuffleEnabledChangeRequested**](/uwp/api/windows.media.systemmediatransportcontrols.shuffleenabledchangerequested) |

 
これらのコントロールの 1 つに対するユーザーの操作を処理するには、最初に、関連付けられているイベントのハンドラーを登録します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SMTCWin10/cs/MainPage.xaml.cs" id="SnippetRegisterPlaybackChangedHandler":::

まず、イベントのハンドラーで、要求された値が有効な予想される範囲内にあることを確認します。 範囲内の場合は、[**MediaElement**](/uwp/api/Windows.UI.Xaml.Controls.MediaElement) の対応するプロパティを設定してから、[**SystemMediaTransportControls**](/uwp/api/Windows.Media.SystemMediaTransportControls) オブジェクトの対応するプロパティを設定します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SMTCWin10/cs/MainPage.xaml.cs" id="SnippetPlaybackChangedHandler":::

-   これらのプレーヤー プロパティ イベントの 1 つが発生するためには、プロパティの初期値を設定する必要があります。 たとえば、[**PlaybackRateChangeRequested**](/uwp/api/windows.media.systemmediatransportcontrols.playbackratechangerequested) は、[**PlaybackRate**](/uwp/api/windows.media.systemmediatransportcontrols.playbackrate) プロパティの値を少なくとも 1 回設定するまでは発生しません。

## <a name="use-the-system-media-transport-controls-for-background-audio"></a>バックグラウンド オーディオに対してシステム メディア トランスポート コントロールを使う

**MediaPlayer** によって提供される自動 SMTC 統合を使用していない場合は、SMTC と手動で統合してバック グラウンド オーディオを有効にする必要があります。 少なくとも、アプリで [**IsPlayEnabled**](/uwp/api/windows.media.systemmediatransportcontrols.isplayenabled) と [**IsPauseEnabled**](/uwp/api/windows.media.systemmediatransportcontrols.ispauseenabled) を true に設定して、再生ボタンと一時停止ボタンを有効にする必要があります。 アプリは、[**ButtonPressed**](/uwp/api/windows.media.systemmediatransportcontrols.buttonpressed) イベントも処理する必要があります。 アプリがこれらの要件を満たしていない場合、アプリがバックグラウンドに移動したときにオーディオ再生は停止します。

バックグラウンド オーディオ用に新しい 1 プロセスのモデルを使うアプリは、[**GetForCurrentView**](/uwp/api/windows.media.systemmediatransportcontrols.getforcurrentview) を呼び出して [**SystemMediaTransportControls**](/uwp/api/Windows.Media.SystemMediaTransportControls) のインスタンスを取得する必要があります。 バックグラウンド オーディオ用に従来の 2 プロセスのモデルを使うアプリは、[**BackgroundMediaPlayer.Current.SystemMediaTransportControls**](/uwp/api/windows.media.playback.mediaplayer.systemmediatransportcontrols) を使ってバックグラウンド プロセスから SMTC にアクセスする必要があります

バックグラウンドでのオーディオ再生について詳しくは、「[バックグラウンドでのメディアの再生](background-audio.md)」をご覧ください。

## <a name="related-topics"></a>関連トピック
* [メディア再生](media-playback.md)
* [システム メディア トランスポート コントロールとの統合](integrate-with-systemmediatransportcontrols.md) 
* [システム メディア トランスポートのサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/SystemMediaTransportControls) 

 
