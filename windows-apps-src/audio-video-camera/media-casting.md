---
ms.assetid: 40B97E0C-EB1B-40C2-A022-1AB95DFB085E
description: この記事では、ユニバーサル Windows アプリからリモート デバイスにメディアをキャストする方法について説明します。
title: メディアのキャスト
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 50f588caaf36d9a2a74222029e17785663cf3953
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/21/2019
ms.locfileid: "67318298"
---
# <a name="media-casting"></a>メディアのキャスト



この記事では、ユニバーサル Windows アプリからリモート デバイスにメディアをキャストする方法について説明します。

## <a name="built-in-media-casting-with-mediaplayerelement"></a>MediaPlayerElement を使った組み込みのメディア キャスト機能

ユニバーサル Windows アプリからメディアをキャストする方法としては、[**MediaPlayerElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement) コントロールの組み込みのキャスト機能を使うのが最も簡単です。

**MediaPlayerElement** コントロールで再生するビデオ ファイルをユーザーが開くことができるようにするには、以下の名前空間をプロジェクトに追加します。

[!code-cs[BuiltInCastingUsing](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetBuiltInCastingUsing)]

アプリの XAML ファイルに **MediaPlayerElement** を追加し、[**AreTransportControlsEnabled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaelement.aretransportcontrolsenabled) を true に設定します。

[!code-xml[MediaElement](./code/MediaCasting_RS1/cs/MainPage.xaml#SnippetMediaElement)]

ユーザーがファイルを選択するときに使うボタンを追加します。

[!code-xml[OpenButton](./code/MediaCasting_RS1/cs/MainPage.xaml#SnippetOpenButton)]

このボタンの [**Click**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) イベント ハンドラーで [**FileOpenPicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker) の新しいインスタンスを作成し、ビデオ ファイルの種類を [**FileTypeFilter**](https://docs.microsoft.com/uwp/api/windows.storage.pickers.fileopenpicker.filetypefilter) コレクションに追加して、開始位置をユーザーのビデオ ライブラリに設定します。

[  **PickSingleFileAsync**](https://docs.microsoft.com/uwp/api/windows.storage.pickers.fileopenpicker.picksinglefileasync) を呼び出して、ファイル ピッカー ダイアログを起動します。 このメソッドからは、ビデオ ファイルを表す [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) オブジェクトが返されます。 ファイル選択操作をユーザーが取り消した場合は null が返されます。返されたファイルが null ではないことを確認してください。 ファイルの [**IRandomAccessStream**](https://docs.microsoft.com/uwp/api/Windows.Storage.Streams.IRandomAccessStream) を取得するには、そのファイルの [**OpenAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.openasync) メソッドを呼び出します。 最後に、[**CreateFromStorageFile**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.createfromstoragefile) を呼び出して選択したファイルから新しい **MediaSource** オブジェクトを作成し、そのオブジェクトを **MediaPlayerElement** オブジェクトの [**Source**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.source) プロパティに割り当てて、そのビデオ ファイルをコントロールのビデオ ソースに設定します。

[!code-cs[OpenButtonClick](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetOpenButtonClick)]

**MediaPlayerElement** にビデオを読み込んだら、ユーザーがトランスポート コントロールのキャスト ボタンを押すだけで、組み込みのダイアログを起動し、読み込まれているメディアのキャスト先となるデバイスを選択できます。

![mediaelement casting button](images/media-element-casting-button.png)

> [!NOTE] 
> Windows 10 バージョン 1607 以降、メディア項目の再生には、**MediaPlayer** クラスを使うことをお勧めします。 **MediaPlayerElement** は、XAML ページの **MediaPlayer** コンテンツをレンダリングするために使われる軽量の XAML コントロールです。 下位互換性を確保するため、**MediaElement**コントロールも引き続きサポートされています。 **MediaPlayer** と **MediaPlayerElement** を使ってメディア コンテンツを再生する方法について詳しくは、「[MediaPlayer を使ったオーディオとビデオの再生](play-audio-and-video-with-mediaplayer.md)」をご覧ください。 **MediaSource** と関連の API を使ってメディア コンテンツを操作する方法について詳しくは、「[メディア項目、プレイリスト、トラック](media-playback-with-mediasource.md)」をご覧ください。

## <a name="media-casting-with-the-castingdevicepicker"></a>CastingDevicePicker を使ったメディアのキャスト

メディアをデバイスにキャストするもう 1 つの方法は、[**CastingDevicePicker**](https://docs.microsoft.com/uwp/api/Windows.Media.Casting.CastingDevicePicker) を使うことです。 このクラスを使うには、プロジェクトに [**Windows.Media.Casting**](https://docs.microsoft.com/uwp/api/Windows.Media.Casting) 名前空間を追加します。

[!code-cs[CastingNamespace](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetCastingNamespace)]

**CastingDevicePicker** オブジェクトのメンバー変数を宣言します。

[!code-cs[DeclareCastingPicker](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetDeclareCastingPicker)]

対象のページが初期化されたタイミングで CastingDevicePicker の新しいインスタンスを作成します。さらに、ビデオをサポートするデバイスだけをキャスト先としてピッカーに一覧表示するために、[**Filter**](https://docs.microsoft.com/uwp/api/windows.media.casting.castingdevicepicker.filter) を [**SupportsVideo**](https://docs.microsoft.com/uwp/api/Windows.Media.Casting.CastingDevicePickerFilter) プロパティに設定します。 [  **CastingDeviceSelected**](https://docs.microsoft.com/uwp/api/windows.media.casting.castingdevicepicker.castingdeviceselected) イベントのハンドラーを登録します。これは、ユーザーがキャスト先のデバイスを選んだときに発生するイベントです。

[!code-cs[InitCastingPicker](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetInitCastingPicker)]

ユーザーがピッカーを起動するためのボタンを XAML ファイルに追加します。

[!code-xml[CastPickerButton](./code/MediaCasting_RS1/cs/MainPage.xaml#SnippetCastPickerButton)]

ボタンの **Click** イベント ハンドラーで [**TransformToVisual**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.transformtovisual) を呼び出し、別の要素を基準とした UI 要素の変換を取得します。 この例の変換は、アプリケーション ウィンドウの表示ルートを基準としたキャスト ピッカー ボタンの位置です。 [  **CastingDevicePicker**](https://docs.microsoft.com/uwp/api/Windows.Media.Casting.CastingDevicePicker) オブジェクトの [**Show**](https://docs.microsoft.com/uwp/api/windows.media.casting.castingdevicepicker.show) メソッドを呼び出して、キャスト先の選択ダイアログを起動します。 ユーザーによって押されたボタンから飛び出すようにダイアログを表示させるため、キャスト ピッカー ボタンの位置とサイズを指定します。

[!code-cs[CastPickerButtonClick](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetCastPickerButtonClick)]

**CastingDeviceSelected** イベント ハンドラーで、イベント引数の [**SelectedCastingDevice**](https://docs.microsoft.com/uwp/api/windows.media.casting.castingdeviceselectedeventargs.selectedcastingdevice) プロパティ (ユーザーが選択したキャスト先デバイスを表す) の [**CreateCastingConnection**](https://docs.microsoft.com/uwp/api/windows.media.casting.castingdevice.createcastingconnection) メソッドを呼び出します。 [  **ErrorOccurred**](https://docs.microsoft.com/uwp/api/windows.media.casting.castingconnection.erroroccurred) イベントと [**StateChanged**](https://docs.microsoft.com/uwp/api/windows.media.casting.castingconnection.statechanged) イベントのハンドラーを登録します。 最後に、[**RequestStartCastingAsync**](https://docs.microsoft.com/uwp/api/windows.media.casting.castingconnection.requeststartcastingasync) を呼び出すとキャストが開始されます。キャストするメディアが **MediaPlayerElement** に関連付けられた **MediaPlayer** のコンテンツであることを指定するために、このメソッドの引数には、**MediaPlayerElement** コントロールの **MediaPlayer** オブジェクトの [**GetAsCastingSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaelement.getascastingsource) メソッドの結果を渡します。

> [!NOTE] 
> キャスト接続は UI スレッドで開始する必要があります。 **CastingDeviceSelected** は UI スレッドでは呼び出されないため、上に挙げた一連の呼び出しを、[**CoreDispatcher.RunAsync**](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.runasync) の呼び出しの中に置いて、UI スレッドで呼び出されるようにする必要があります。

[!code-cs[CastingDeviceSelected](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetCastingDeviceSelected)]

**ErrorOccurred** イベントと **StateChanged** イベントのハンドラーでは、UI を更新して、現在のキャスト ステータスをユーザーに通知する必要があります。 これらのイベントについては、次のセクションで、カスタムのキャスト先デバイス ピッカーを作成する方法の中で詳しく説明します。

[!code-cs[EmptyStateHandlers](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetEmptyStateHandlers)]

## <a name="media-casting-with-a-custom-device-picker"></a>カスタム デバイス ピッカーを使ったメディアのキャスト

次のセクションでは、独自のコードでキャスト先デバイスを列挙し、接続を開始することによって、キャスト先デバイス ピッカーの UI を作成する方法を説明します。

利用可能なキャスト先デバイスを列挙するには、[**Windows.Devices.Enumeration**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration) 名前空間をプロジェクトに追加します。

[!code-cs[EnumerationNamespace](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetEnumerationNamespace)]

この例の基本的な UI を実装するために、以下のコントロールを XAML ページに追加します。

-   利用可能なキャスト先デバイスを探すデバイス ウォッチャーの起動ボタン。
-   キャスト先を列挙する処理の進行状況をユーザーにフィードバックする [**ProgressRing**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ProgressRing) コントロール。
-   検出されたキャスト先デバイスを一覧表示する [**ListBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListBox)。 キャスト先デバイス オブジェクトを直接コントロールに割り当てたうえで、さらに [**FriendlyName**](https://docs.microsoft.com/uwp/api/windows.media.casting.castingdevice.friendlyname) プロパティを表示できるようにコントロールの [**ItemTemplate**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate) を定義します。
-   キャスト先デバイスとの接続をユーザーが切断するためのボタン。

[!code-xml[CustomPickerXAML](./code/MediaCasting_RS1/cs/MainPage.xaml#SnippetCustomPickerXAML)]

分離コードには、[**DeviceWatcher**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher) と [**CastingConnection**](https://docs.microsoft.com/uwp/api/Windows.Media.Casting.CastingConnection) のメンバー変数を宣言します。

[!code-cs[DeclareDeviceWatcher](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetDeclareDeviceWatcher)]

*startWatcherButton* の **Click** ハンドラーでまず行うのは UI の更新です。デバイスの列挙中はボタンを無効にし、プログレス リングをアクティブにします。 キャスト先デバイスのリスト ボックスをクリアします。

次に、[**DeviceInformation.CreateWatcher**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.createwatcher) を呼び出してデバイス ウォッチャーを作成します。 このメソッドを使うと、さまざまな種類のデバイスを監視することができます。 ビデオ キャストに対応しているデバイスを監視対象として指定するには、[**CastingDevice.GetDeviceSelector**](https://docs.microsoft.com/uwp/api/windows.media.casting.castingdevice.getdeviceselector) から返されるデバイスのセレクター文字列を使います。

最後に、[**Added**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.added)、[**Removed**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.removed)、[**EnumerationCompleted**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.enumerationcompleted)、[**Stopped**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.stopped) の各イベントのハンドラーを登録します。

[!code-cs[StartWatcherButtonClick](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetStartWatcherButtonClick)]

**Added** は、ウォッチャーで新しいデバイスが検出されたときに発生するイベントです。 このイベントのハンドラーで [**CastingDevice.FromIdAsync**](https://docs.microsoft.com/uwp/api/windows.media.casting.castingdevice.fromidasync) を呼び出して新しい [**CastingDevice**](https://docs.microsoft.com/uwp/api/Windows.Media.Casting.CastingDevice) オブジェクトを作成します。このメソッドの引数には、検出されたキャスト先デバイスの ID を指定します。ID は、ハンドラーに渡された **DeviceInformation** オブジェクトに格納されています。

この **CastingDevice** をキャスト先デバイスの **ListBox** に追加して、ユーザーが選択できるようにします。 リスト ボックスの項目テキストには、XAML で定義した [**ItemTemplate**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate) により、[**FriendlyName**](https://docs.microsoft.com/uwp/api/windows.media.casting.castingdevice.friendlyname) プロパティが使われます。 このイベント ハンドラーは UI スレッドで呼び出されないため、UI の更新は、[**CoreDispatcher.RunAsync**](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.runasync) の呼び出しの中で行う必要があります。

[!code-cs[WatcherAdded](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetWatcherAdded)]

キャスト先デバイスがもう存在しないことをウォッチャーが検出すると、**Removed** イベントが発生します。 ハンドラーに渡された **Added** オブジェクトの ID プロパティと、リスト ボックスの [**Items**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.items) コレクションに含まれている各 **Added** の ID とを比較します。 ID が一致する場合は、そのオブジェクトをコレクションから削除します。 UI が更新中であるため、先ほどと同様、この呼び出しは **RunAsync** の呼び出しの中で行う必要があります。

[!code-cs[WatcherRemoved](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetWatcherRemoved)]

ウォッチャーがデバイスの検出を完了すると、**EnumerationCompleted** イベントが発生します。 このイベントのハンドラーで UI を更新して、デバイスの列挙処理が完了したことをユーザーに知らせ、[**Stop**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.stop) を呼び出してデバイス ウォッチャーを停止します。

[!code-cs[WatcherEnumerationCompleted](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetWatcherEnumerationCompleted)]

デバイス ウォッチャーの停止処理が完了すると Stopped イベントが発生します。 このイベントのハンドラーで [**ProgressRing**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ProgressRing) コントロールを停止したうえで、デバイス列挙処理をユーザーが再開できるように *startWatcherButton* を再び有効にします。

[!code-cs[WatcherStopped](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetWatcherStopped)]

リスト ボックスからいずれかのキャスト先デバイスをユーザーが選択すると、[**SelectionChanged**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) イベントが発生します。 このハンドラーの中でキャスト接続を作成し、キャストを開始することになります。

まず、デバイスの列挙処理がメディアのキャスト処理に干渉しないよう、デバイス ウォッチャーが停止していることを確認します。 キャスト接続を作成するには、ユーザーが選択した **CastingDevice** オブジェクトの [**CreateCastingConnection**](https://docs.microsoft.com/uwp/api/windows.media.casting.castingdevice.createcastingconnection) を呼び出します。 [  **StateChanged**](https://docs.microsoft.com/uwp/api/windows.media.casting.castingconnection.statechanged) イベントおよび [**ErrorOccurred**](https://docs.microsoft.com/uwp/api/windows.media.casting.castingconnection.erroroccurred) イベントに対応するイベント ハンドラーを追加します。

メディアのキャストを開始するには、[**RequestStartCastingAsync**](https://docs.microsoft.com/uwp/api/windows.media.casting.castingconnection.requeststartcastingasync) を呼び出します。その際、引数には、**MediaPlayer** の [**GetAsCastingSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaelement.getascastingsource) メソッドを呼び出して取得したキャスト ソースを指定します。 最後に、メディアのキャストをユーザーが停止するための切断ボタンを表示状態にします。

[!code-cs[SelectionChanged](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetSelectionChanged)]

StateChanged ハンドラーで実行する操作は、キャスト接続の新しい状態によって異なります。

-   状態が **Connected** または **Rendering** である場合は、**ProgressRing** コントロールを非アクティブにし、切断ボタンを表示します。
-   状態が **Disconnected** である場合は、現在のキャスト先デバイスの選択をリスト ボックスで解除し、**ProgressRing** コントロールを非アクティブにして、切断ボタンを非表示にします。
-   状態が **Connecting** である場合は、**ProgressRing** コントロールをアクティブにして、切断ボタンを非表示にします。
-   状態が **Disconnecting** である場合は、**ProgressRing** コントロールをアクティブにして、切断ボタンを非表示にします。

[!code-cs[StateChanged](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetStateChanged)]

**ErrorOccurred** イベントのハンドラーでは、UI を更新してキャスト エラーの発生をユーザーに知らせると共に、リスト ボックスで現在の **CastingDevice** オブジェクトの選択を解除します。

[!code-cs[ErrorOccurred](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetErrorOccurred)]

最後に、切断ボタンのハンドラーを実装します。 メディアのキャストを停止してキャスト先デバイスから切断するために、**CastingConnection** オブジェクトの [**DisconnectAsync**](https://docs.microsoft.com/uwp/api/windows.media.casting.castingconnection.disconnectasync) メソッドを呼び出します。 この呼び出しは、[**CoreDispatcher.RunAsync**](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.runasync) を呼び出して UI スレッドにディスパッチする必要があります。

[!code-cs[DisconnectButton](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetDisconnectButton)]

 

 




