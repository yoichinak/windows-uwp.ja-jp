---
ms.assetid: AE98C22B-A071-4206-ABBB-C0F0FB7EF33C
description: この記事では、ユニバーサル Windows プラットフォーム (UWP) アプリにアダプティブ ストリーミング マルチメディア コンテンツの再生を追加する方法について説明します。 現在、この機能では、HTTP ライブ ストリーミング (HLS) と Dynamic Adaptive Streaming over HTTP (DASH) コンテンツの再生がサポートされています。
title: アダプティブ ストリーミング
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 11e960699c64b005324d99b89b61dc8ee97961f7
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363005"
---
# <a name="adaptive-streaming"></a>アダプティブ ストリーミング


この記事では、ユニバーサル Windows プラットフォーム (UWP) アプリにアダプティブ ストリーミング マルチメディア コンテンツの再生を追加する方法について説明します。 この機能では、HTTP ライブ ストリーミング (HLS) と Dynamic Adaptive Streaming over HTTP (DASH) コンテンツの再生がサポートされています。 Windows 10 バージョン 1803 以降では、**[AdaptiveMediaSource](/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSource)** によってスムーズ ストリーミングがサポートされています。

サポートされている HLS プロトコル タグの一覧については、「[HLS タグのサポート](hls-tag-support.md)」をご覧ください。 

サポートされている DASH プロファイルの一覧については、「[DASH プロファイルのサポート](dash-profile-support.md)」をご覧ください。 

> [!NOTE] 
> この記事のコードは、UWP の[アダプティブ ストリーミングのサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/AdaptiveStreaming)を基にしています。

## <a name="simple-adaptive-streaming-with-mediaplayer-and-mediaplayerelement"></a>MediaPlayer と MediaPlayerElement を使った簡単なアダプティブ ストリーミング

UWP アプリでアダプティブ ストリーミング メディアを再生するには、DASH または HLS のマニフェスト ファイルを指す **Uri** オブジェクトを作成します。 [**MediaPlayer**](/uwp/api/Windows.Media.Playback.MediaPlayer) クラスのインスタンスを作成します。 [**MediaSource.CreateFromUri**](/uwp/api/windows.media.core.mediasource.createfromuri) を呼び出して新しい **MediaSource** オブジェクトを作成し、それを **MediaPlayer** の [**Source**](/uwp/api/windows.media.playback.mediaplayer.source) プロパティに設定します。 [**Play**](/uwp/api/windows.media.playback.mediaplayer.play) を呼び出してメディア コンテンツの作成を開始します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs" id="SnippetDeclareMediaPlayer":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs" id="SnippetManifestSourceNoUI":::

上の例では、メディア コンテンツのオーディオが再生されますが、UI 内のコンテンツは自動的にレンダリングされません。 ビデオ コンテンツを再生するほとんどのアプリは、XAML ページでコンテンツをレンダリングします。  これを行うには、XAML ページに [**MediaPlayerElement**](/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement) コントロールを追加します。

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/AdaptiveStreaming_RS1/cs/MainPage.xaml" id="SnippetMediaPlayerElementXAML":::

[**MediaSource.CreateFromUri**](/uwp/api/windows.media.core.mediasource.createfromuri) を呼び出して、DASH や HLS のマニフェスト ファイルの URI から **MediaSource** を作成します。 その後、**MediaPlayerElement** の [**Source**](/uwp/api/windows.ui.xaml.controls.mediaelement.sourceproperty) プロパティを設定します。 **MediaPlayerElement**によって、コンテンツの新しい **MediaPlayer** オブジェクトが自動的に作成されます。 **MediaPlayer** で **Play** を呼び出して、コンテンツの再生を開始できます。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs" id="SnippetManifestSource":::

> [!NOTE] 
> Windows 10 バージョン 1607 以降、メディア項目の再生には、**MediaPlayer** クラスを使うことをお勧めします。 **MediaPlayerElement** は、XAML ページの **MediaPlayer** コンテンツをレンダリングするために使われる軽量の XAML コントロールです。 下位互換性を確保するため、**MediaElement**コントロールも引き続きサポートされています。 **MediaPlayer** と **MediaPlayerElement** を使ってメディア コンテンツを再生する方法について詳しくは、「[MediaPlayer を使ったオーディオとビデオの再生](play-audio-and-video-with-mediaplayer.md)」をご覧ください。 **MediaSource** と関連の API を使ってメディア コンテンツを操作する方法について詳しくは、「[メディア項目、プレイリスト、トラック](media-playback-with-mediasource.md)」をご覧ください。

## <a name="adaptive-streaming-with-adaptivemediasource"></a>AdaptiveMediaSource を使ったアダプティブ ストリーミング

アプリで、より高度なアダプティブ ストリーミング機能 (カスタム HTTP ヘッダーの指定、現在のダウンロードや再生のビットレートの監視、アダプティブ ストリーミングのビットレートをシステムで切り替えるタイミングを決定する比率の調整など) を必要とする場合は、**[AdaptiveMediaSource](/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSource)** オブジェクトを使います。

アダプティブ ストリーミング API は、[**Windows.Media.Streaming.Adaptive**](/uwp/api/Windows.Media.Streaming.Adaptive) 名前空間にあります。 この記事の例には、以下の名前空間の API が使われています。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs" id="SnippetAdaptiveStreamingUsing":::

## <a name="initialize-an-adaptivemediasource-from-a-uri"></a>URI から AdaptiveMediaSource を初期化する

[**CreateFromUriAsync**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.createfromuriasync) を呼び出し、アダプティブ ストリーミング マニフェスト ファイルの URI で、**AdaptiveMediaSource** を初期化します。 このメソッドから返される [**AdaptiveMediaSourceCreationStatus**](/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSourceCreationStatus) の値を利用して、メディア ソースが正しく作成されたかどうかを確認できます。 正しく作成されている場合、オブジェクトを **MediaPlayer** のストリーム ソースとして設定できます。そのためには、[**MediaSource.CreateFromAdaptiveMediaSource**](/uwp/api/Windows.Media.Core.MediaSource.AdaptiveMediaSource) を呼び出して **MediaSource** オブジェクトを作成し、このオブジェクトをメディア プレーヤーの [**Source**](/uwp/api/windows.media.playback.mediaplayer.Source) プロパティに割り当てます。 この例では、[**AvailableBitrates**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.availablebitrates) プロパティを照会することによって、このストリームで利用できる最大ビットレートを特定し、その値が初期ビットレートとして設定されます。 またこの例では、**AdaptiveMediaSource** のいくつかのイベントのハンドラーも登録します。これらのイベントについては、この記事の後半で説明します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs" id="SnippetInitializeAMS":::

## <a name="initialize-an-adaptivemediasource-using-httpclient"></a>HttpClient を使用して AdaptiveMediaSource を初期化する

マニフェスト ファイルを取得するためにカスタム HTTP ヘッダーを設定する場合は、[**HttpClient**](/uwp/api/Windows.Web.Http.HttpClient) オブジェクトを作成し、目的のヘッダーを設定してから、そのオブジェクトを **CreateFromUriAsync** のオーバーロードに渡すことができます。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs" id="SnippetInitializeAMSWithHttpClient":::

[**DownloadRequested**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.downloadrequested) イベントは、システムがサーバーからリソースの取得を試みるときに発生します。 イベント ハンドラーに渡される [**AdaptiveMediaSourceDownloadRequestedEventArgs**](/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSourceDownloadRequestedEventArgs) によって、要求されているリソースに関する情報 (リソースの種類や URI など) を提供するプロパティが公開されます。

## <a name="modify-resource-request-properties-using-the-downloadrequested-event"></a>DownloadRequested イベントを使ってリソース要求プロパティを変更する

**DownloadRequested** イベント ハンドラーを使って、リソース要求を変更することができます。この場合、イベント引数によって提供される [**AdaptiveMediaSourceDownloadResult**](/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSourceDownloadResult) オブジェクトのプロパティを更新します。 次の例では、結果オブジェクトの [**ResourceUri**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcedownloadresult.resourceuri) プロパティを更新して、リソースの取得元となる URI を変更します。 メディア セグメントのバイト範囲オフセットや長さを書き換えたり、次の例に示すようにリソース URI を変更して完全なリソースをダウンロードし、バイト範囲のオフセットと長さを null に設定することもできます。

結果オブジェクトの [**Buffer**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcedownloadresult.buffer) プロパティや [**InputStream**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcedownloadresult.inputstream) プロパティを設定することによって、要求したリソースの内容を置き換えることができます。 次の例では、**Buffer** プロパティを設定することによって、マニフェスト リソースの内容が置き換わります。 非同期的に取得したデータを使ってリソース要求を更新する場合は (リモート サーバーや非同期ユーザー認証からデータを取得する場合など)、[**AdaptiveMediaSourceDownloadRequestedEventArgs.GetDeferral**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcedownloadrequestedeventargs.getdeferral) を呼び出して遅延を取得する必要があります。その後、操作が完了して、ダウンロード要求操作が継続可能なことをシステムに通知するときに、[**Complete**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcedownloadrequesteddeferral.complete) を呼び出してください。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs" id="SnippetAMSDownloadRequested":::

## <a name="use-bitrate-events-to-manage-and-respond-to-bitrate-changes"></a>ビットレート イベントを使用して、ビットレートの変更を管理し変更に応答する

**AdaptiveMediaSource** オブジェクトは、ダウンロードや再生のビットレートが変わったときに対応できるようにするイベントを提供します。 この例では、現在のビットレートが UI で簡単に更新されます。 また、アダプティブ ストリーミングのビットレートをシステムで切り替えるタイミングを決定する比率を変更できることに注意してください。 詳しくは、[**AdvancedSettings**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.advancedsettings) プロパティをご覧ください。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs" id="SnippetAMSBitrateEvents":::

## <a name="handle-download-completion-and-failure-events"></a>ダウンロードの完了と失敗イベントを処理する
**AdaptiveMediaSource** オブジェクトは、要求されたリソースのダウンロードが失敗したときに、[**DownloadFailed**](/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSource.DownloadFailed) イベントを発生させます。 このイベントを使用して、エラーに応じて UI を更新できます。 このイベントを使用して、ダウンロード操作とエラーに関する統計情報をログに記録することもできます。 

イベント ハンドラーに渡される [**AdaptiveMediaSourceDownloadFailedEventArgs**](/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSourceDownloadFailedEventArgs) オブジェクトには、リソースの種類、リソースの URI、ストリーム内のエラーが発生した位置など、失敗したリソースのダウンロードに関するメタデータが含まれています。 [**RequestId**](/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSourceDownloadFailedEventArgs.RequestId) は、複数のイベント間で、個々の要求に関する状態情報を関連付けるために使用できる、システムで生成された一意の識別子を取得します。

[**Statistics**](/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSourceDownloadFailedEventArgs.Statistics) プロパティは、[**AdaptiveMediaSourceDownloadStatistics**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcedownloadstatistics) オブジェクトを返します。このオブジェクトは、イベントの発生時や、ダウンロード操作のさまざまなマイルストーンのタイミングで受信したバイト数に関する詳細情報を提供します。 この情報は、アダプティブ ストリーミングの実装でパフォーマンスの問題を識別するためにろぐに記録できます。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs" id="SnippetAMSDownloadFailed":::


[**DownloadCompleted**](/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSource.DownloadCompleted) イベントは、リソースのダウンロードが完了したときに発生し、**DownloadFailed** イベントと同様のデータを提供します。 ここでも、1 つの要求のイベントを関連付けるために **RequestId** が提供されます。 また、ダウンロード統計情報のログ記録を有効にするために、**AdaptiveMediaSourceDownloadStatistics** オブジェクトが提供されます。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs" id="SnippetAMSDownloadCompleted":::

## <a name="gather-adaptive-streaming-telemetry-data-with-adaptivemediasourcediagnostics"></a>AdaptiveMediaSourceDiagnostics によってアダプティブ ストリーミングの利用統計情報を収集する
**AdaptiveMediaSource** は、[**AdaptiveMediaSourceDiagnostics**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcediagnostics) オブジェクトを返す [**Diagnostics**](/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSource) プロパティを公開します。 このオブジェクトを使って、[**DiagnosticAvailable**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcediagnostics.DiagnosticAvailable) イベントを登録します。 このイベントは、利用統計情報の収集に使用することを目的としており、実行時にアプリの動作を変更するために使用することはできません。 この診断イベントは、さまざまな理由で発生します。 イベントが発生した理由を特定するには、イベントに渡される [**AdaptiveMediaSourceDiagnosticAvailableEventArgs**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcediagnosticavailableeventargs) オブジェクトの [**DiagnosticType**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcediagnosticavailableeventargs.DiagnosticType) プロパティを確認します。 潜在的な理由には、要求されたリソースへのアクセス時のエラーや、ストリーミング マニフェスト ファイルの解析時のエラーが含まれます。 診断イベントをトリガーできる状況の一覧については、[**AdaptiveMediaSourceDiagnosticType**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcediagnostictype) を参照してください。 他のアダプティブ ストリーミング イベントの引数と同様に、**AdaptiveMediaSourceDiagnosticAvailableEventArgs** は、さまざまなイベントの間で要求情報を関連付けるための **RequestId** プロパティを提供します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs" id="SnippetAMSDiagnosticAvailable":::

## <a name="defer-binding-of-adaptive-streaming-content-for-items-in-a-playback-list-by-using-mediabinder"></a>MediaBinder を使用して、再生リスト内の項目のアダプティブ ストリーミング コンテンツのバインドを延期する
[**MediaBinder**](/uwp/api/Windows.Media.Core.MediaBinder) クラスによって、[**MediaPlaybackList**](/uwp/api/Windows.Media.Playback.MediaPlaybackList) 内のメディア コンテンツのバインドを延期することができます。 Windows 10 Version 1703 以降では、バインドされるコンテンツとして [**AdaptiveMediaSource**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasource) を指定できます。 アダプティブ メディア ソースの遅延バインドのプロセスの大部分は、「[メディア項目、プレイリスト、トラック](media-playback-with-mediasource.md)」で説明されている、他の種類のメディアのバインドと同じです。 

**MediaBinder** インスタンスを作成し、アプリで定義された、バインドされるコンテンツを識別するための [**Token**](/uwp/api/Windows.Media.Core.MediaBinder.Token) 文字列を設定して、[**Binding**](/uwp/api/Windows.Media.Core.MediaBinder.Binding) イベントに登録します。 [**MediaSource.CreateFromMediaBinder**](/uwp/api/windows.media.core.mediasource.createfrommediabinder) を呼び出すことによって、**Binder** から **MediaSource** を作成します。 次に、**MediaSource** から **MediaPlaybackItem** を作成し、再生リストに追加します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetInitMediaBinder":::

**Binding** イベント ハンドラーでは、トークン文字列を使用してバインドされるコンテンツを識別し、**[CreateFromStreamAsync](/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.createfromstreamasync)** または **[CreateFromUriAsync](/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.createfromuriasync)** のオーバーロードを呼び出すことによってアダプティブ メディア ソースを作成します。 これらは非同期メソッドであるため、最初に [**MediaBindingEventArgs.GetDeferral**](/uwp/api/windows.media.core.mediabindingeventargs.GetDeferral) メソッドを呼び出して、操作が完了するまで待機してから続行するようにシステムに指示する必要があります。  **[SetAdaptiveMediaSource](/uwp/api/windows.media.core.mediabindingeventargs.setadaptivemediasource)** を呼び出すことによって、バインドされるコンテンツとして、アダプティブ メディア ソースを設定します。 最後に、操作が完了した後、[**Deferral.Complete**](/uwp/api/windows.foundation.deferral.Complete) を呼び出して、システムに続行を指示します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetBinderBindingAMS":::

バインドされているアダプティブ メディア ソースのイベント ハンドラーを登録する場合は、**MediaPlaybackList** の [**CurrentItemChanged**](/uwp/api/windows.media.playback.mediaplaybacklist.CurrentItemChanged) イベントのハンドラーでこれを実行できます。 [**CurrentMediaPlaybackItemChangedEventArgs.NewItem**](/uwp/api/windows.media.playback.currentmediaplaybackitemchangedeventargs.NewItem) プロパティには、リスト内で現在再生中の新しい **MediaPlaybackItem** が含まれます。 新しい項目を表す **AdaptiveMediaSource** インスタンスを取得するには、**MediaPlaybackItem** の [**Source**](/uwp/api/Windows.Media.Playback.MediaPlaybackItem.Source) プロパティにアクセスし、次にメディア ソースの [**AdaptiveMediaSource**](/uwp/api/windows.media.core.mediasource.AdaptiveMediaSource) プロパティにアクセスします。 新しい再生項目が **AdaptiveMediaSource** ではない場合、このプロパティは null になるため、このオブジェクトのイベントのハンドラーを登録する前に、これが null であることをテストする必要があります。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetAMSBindingCurrentItemChanged":::

## <a name="related-topics"></a>関連トピック
* [メディア再生](media-playback.md)
* [HLS タグのサポート](hls-tag-support.md) 
* [ダッシュプロファイルのサポート](dash-profile-support.md) 
* [MediaPlayer を使ったオーディオとビデオの再生](play-audio-and-video-with-mediaplayer.md)
* [バックグラウンドでのメディアの再生](background-audio.md) 
