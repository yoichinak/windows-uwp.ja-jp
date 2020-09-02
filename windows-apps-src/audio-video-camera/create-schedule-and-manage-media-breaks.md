---
ms.assetid: 0309c7a1-8e4c-4326-813a-cbd9f8b8300d
description: この記事では、メディア再生アプリ用にメディアの中断を作成、スケジュール、管理する方法について説明します。
title: メディアの中断の作成、スケジュール、管理
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 4c3d9743b3c82b935029eeb0ee4c5b9347251fc2
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363975"
---
# <a name="create-schedule-and-manage-media-breaks"></a>メディアの中断の作成、スケジュール、管理

この記事では、メディア再生アプリ用にメディアの中断を作成、スケジュール、管理する方法について説明します。 通常、メディアの中断は、オーディオ広告やビデオ広告をメディア コンテンツに挿入する目的で使います。 Windows 10 バージョン 1607 以降では、[**MediaBreakManager**](/uwp/api/Windows.Media.Playback.MediaBreakManager) クラスを使って、[**MediaPlayer**](/uwp/api/Windows.Media.Playback.MediaPlayer) で再生する任意の [**MediaPlaybackItem**](/uwp/api/Windows.Media.Playback.MediaPlaybackItem) にメディアの中断を簡単かつ迅速に追加できます。


メディアの中断を 1 つ以上スケジュールすると、再生中の指定した時間に、システムがメディア コンテンツを自動的に再生します。 **MediaBreakManager** では、ユーザーがメディアを中断、終了、またはスキップしたときにアプリが反応できるように、イベントが提供されています。 [**MediaPlaybackSession**](/uwp/api/Windows.Media.Playback.MediaPlaybackSession) にアクセスしてメディアの中断を確認し、ダウンロードや進行状況の更新のバッファリングなどのイベントを監視することもできます。

## <a name="schedule-media-breaks"></a>メディアの中断のスケジュール
各 **MediaPlaybackItem** オブジェクトには、アイテムの再生時に再生されるメディアの中断の構成に使う独自の [**MediaBreakSchedule**](/uwp/api/Windows.Media.Playback.MediaBreakSchedule) があります。 アプリでメディアの中断を使うための最初の手順は、メイン再生コンテンツ用の [**MediaPlaybackItem**](/uwp/api/Windows.Media.Playback.MediaPlaybackItem) の作成です。 

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaBreaks_RS1/cs/MainPage.xaml.cs" id="SnippetMoviePlaybackItem":::

**MediaPlaybackItem**、[**MediaPlaybackList**](/uwp/api/Windows.Media.Playback.MediaPlaybackList)、他の基本的なメディアの再生 API の操作の参照については、「[メディア項目、プレイリスト、トラック](media-playback-with-mediasource.md)」を参照してください。

次の例は、**MediaPlaybackItem** にプリロール区切りを追加する方法を示しています。これは、区切りが属する再生アイテムを再生する前に、システムがメディアの中断を再生することを意味します。 まず、新しい [**MediaBreak**](/uwp/api/Windows.Media.Playback.MediaBreak) オブジェクトがインスタンス化されます。 この例では、コンストラクター [**MediaBreakInsertionMethod.Interrupt**](/uwp/api/Windows.Media.Playback.MediaBreakInsertionMethod) と共に呼び出されます。つまり、区切りコンテンツの再生中はメイン コンテンツが一時停止されます。 

次に、区切り中に再生されるコンテンツ (広告など) 用に新しい **MediaPlaybackItem** が作成されます。 この再生アイテムの [**CanSkip**](/uwp/api/windows.media.playback.mediaplaybackitem.canskip) プロパティは false に設定されます。 つまり、ユーザーは組み込みのメディア コントロールを使ってアイテムをスキップできません。 ただし、[**SkipCurrentBreak**](/uwp/api/windows.media.playback.mediabreakmanager.skipcurrentbreak) を呼び出すことで、広告をプログラムによりスキップすることを選ぶことはできます。 

メディアの中断の [**PlaybackList**](/uwp/api/windows.media.playback.mediabreak.playbacklist) プロパティは、複数のメディア アイテムをプレイリストとして再生できるようにする [**MediaPlaybackList**](/uwp/api/Windows.Media.Playback.MediaPlaybackList) です。 一覧の **Items** コレクションから 1 つまたは複数の **MediaPlaybackItem** オブジェクトを追加し、メディアの中断のプレイリストに含めます。

最後に、メイン コンテンツ再生アイテムの [**BreakSchedule**](/uwp/api/windows.media.playback.mediaplaybackitem.breakschedule) プロパティを使ってメディアの中断をスケジュールします。 プリロール区切りにする中断を、スケジュール オブジェクトの [**PrerollBreak**](/uwp/api/windows.media.playback.mediabreakschedule.prerollbreak) プロパティに割り当てることで指定します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaBreaks_RS1/cs/MainPage.xaml.cs" id="SnippetPreRollBreak":::

これで、メイン メディア アイテムを再生できるようになりました。作成したメディアの中断は、メイン コンテンツの前に再生されます。 新しい [**MediaPlayer**](/uwp/api/Windows.Media.Playback.MediaPlayer) オブジェクトを作成し、オプションで [**AutoPlay**](/uwp/api/windows.media.playback.mediaplayer.autoplay) プロパティをtrue に設定して再生を自動的に開始します。 **MediaPlayer** の [**Source**](/uwp/api/windows.media.playback.mediaplayer.source) プロパティをメイン コンテンツ再生アイテムに設定します。 必須ではありませんが、**MediaPlayer** を [**MediaPlayerElement**](/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement) に割り当てて XAML ページでメディアをレンダリングできます。 **MediaPlayer** の使い方について詳しくは、「[MediaPlayer を使ったオーディオとビデオの再生](play-audio-and-video-with-mediaplayer.md)」をご覧ください。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaBreaks_RS1/cs/MainPage.xaml.cs" id="SnippetPlay":::

プリロール区切りを同じ手法を使って、メイン コンテンツを含む **MediaPlaybackItem** の再生が終わったら再生されるポストロール区切りを追加します。ただし、**MediaBreak** オブジェクトを[**PostrollBreak**](/uwp/api/windows.media.playback.mediabreakschedule.postrollbreak) プロパティに再生する点が異なります。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaBreaks_RS1/cs/MainPage.xaml.cs" id="SnippetPostRollBreak":::

メイン コンテンツの再生中の指定した時点で再生されるミッドロール区切りを 1 つ以上スケジュールすることもできます。 次の例では、メイン メディア アイテムの再生中に区切りが生成される時間を指定する **TimeSpan** オブジェクトを受け入れるコンストラクター オーバーロードを使って [**MediaBreak**](/uwp/api/Windows.Media.Playback.MediaBreak) が作成されます。 ここでも、[**MediaBreakInsertionMethod.Interrupt**](/uwp/api/Windows.Media.Playback.MediaBreakInsertionMethod) が指定され、区切りの生成中にメイン コンテンツの再生が一時停止されることが示されます。 [**InsertMidrollBreak**](/uwp/api/windows.media.playback.mediabreakschedule.insertmidrollbreak) を呼び出すことで、ミッドロール区切りがスケジュールに追加されます。 [**MidrollBreaks**](/uwp/api/windows.media.playback.mediabreakschedule.midrollbreaks) プロパティにアクセスすることで、スケジュールに含まれている現在のミッドロール区切りの読み取り専用一覧を取得できます。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaBreaks_RS1/cs/MainPage.xaml.cs" id="SnippetMidrollBreak":::

次のミッドロール区切りの例では、[**MediaBreakInsertionMethod.Replace**](/uwp/api/Windows.Media.Playback.MediaBreakInsertionMethod) 挿入メソッドを使います。つまり、区切りの再生中もメイン コンテンツの再生が続けられます。 このオプションは通常、広告の再生中にライブ ストリームを一時停止して遅れを生じさせたくないライブ ストリーミング メディア アプリにより使われます。 

この例では、2 つの [**TimeSpan**](/uwp/api/Windows.Foundation.TimeSpan) パラメーターを受け入れる [**MediaPlaybackItem**](/uwp/api/Windows.Media.Playback.MediaPlaybackItem) コンストラクターのオーバーロードも使います。 最初のパラメーターは、メディアの中断アイテム内の、再生が開始される開始点を指定します。 2 番目のパラメーターは、メディアの中断アイテムが再生される時間の長さを指定します。 したがって、次の例では、20 分の時点でメイン コンテンツに対して **MediaBreak** が再生を開始します。 再生されると、区切りメディア アイテムの先頭から 30 秒後にメディア アイテムが開始され、15 秒間再生されてからメイン メディア コンテンツが再生を再開します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaBreaks_RS1/cs/MainPage.xaml.cs" id="SnippetMidrollBreak2":::

## <a name="skip-media-breaks"></a>メディアの中断のスキップ
この記事で既に説明したように、**MediaPlaybackItem** の [**CanSkip**](/uwp/api/windows.media.playback.mediaplaybackitem.canskip) プロパティを設定し、ユーザーが組み込みコントロールを使ってコンテンツをスキップできないようにすることができます。 ただし、いつでもコードから [**SkipCurrentBreak**](/uwp/api/windows.media.playback.mediabreakmanager.skipcurrentbreak) を呼び出すと現在の中断をスキップできます。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaBreaks_RS1/cs/MainPage.xaml.cs" id="SnippetSkipButtonClick":::

## <a name="handle-mediabreak-events"></a>MediaBreak イベントの処理

メディアの中断の状態の変化に基づいてアクションを実行するために登録できる、メディアの中断に関連するいくつかのイベントがあります。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaBreaks_RS1/cs/MainPage.xaml.cs" id="SnippetRegisterMediaBreakEvents":::

[**BreakStarted**](/uwp/api/windows.media.playback.mediabreakmanager.breakstarted) は、メディアの中断が開始すると発生します。 メディア コンテンツを再生していることがユーザーにわかるように UI を更新できます。 この例では、ハンドラーに渡された [**MediaBreakStartedEventArgs**](/uwp/api/Windows.Media.Playback.MediaBreakStartedEventArgs) を使って、開始されたメディアの中断への参照を取得します。 次に、[**CurrentItemIndex**](/uwp/api/windows.media.playback.mediaplaybacklist.currentitemindex) プロパティを使って、メディアの中断のプレイリストにある再生メディア アイテムが決定されます。 その後、UI が更新され、現在の広告インデックスと中断中の残りの広告数がユーザーに表示されます。 UI の更新は UI スレッドに加える必要があるため、[**RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync) の呼び出し内で呼び出しを行う必要がある点に注意してください。 

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaBreaks_RS1/cs/MainPage.xaml.cs" id="SnippetBreakStarted":::

[**BreakEnded**](/uwp/api/windows.media.playback.mediabreakmanager.breakended) は、中断内のすべてのメディア アイテムが再生を終えるか、スキップされたときに発生します。 このイベントのハンドラーを使って UI を更新し、メディアの中断コンテンツがそれ以上再生されないことを示すことができます。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaBreaks_RS1/cs/MainPage.xaml.cs" id="SnippetBreakEnded":::

**BreakSkipped** イベントは、[**CanSkip**](/uwp/api/windows.media.playback.mediaplaybackitem.canskip) が true になっているアイテムの再生中ユーザーが組み込み UI の *[次へ]* ボタンを押したとき、または [**SkipCurrentBreak**](/uwp/api/windows.media.playback.mediabreakmanager.skipcurrentbreak) を呼び出すことでコードで中断をスキップしたときに発生します。

次の例では、**MediaPlayer** の [**Source**](/uwp/api/windows.media.playback.mediaplayer.source) プロパティを使って、メイン コンテンツのメディア アイテムへの参照を取得します。 スキップされたメディアの中断は、このアイテムの中断スケジュールに属しています。 次に、スキップされたメディアの中断がスケジュールの [**PrerollBreak**](/uwp/api/windows.media.playback.mediabreakschedule.prerollbreak) プロパティに設定されたメディアの中断と同じであるかどうかをコードがチェックします。 同じである場合、プリロール区切りがスキップされた中断であることを意味します。この場合、新しいミッドロール区切りが作成され、メイン コンテンツの 10 分の時点で再生されるようにスケジュールされます。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaBreaks_RS1/cs/MainPage.xaml.cs" id="SnippetBreakSkipped":::

[**BreaksSeekedOver**](/uwp/api/windows.media.playback.mediabreakmanager.breaksseekedover) は、メイン メディア アイテムの再生位置が、1 つ以上のメディアの中断のスケジュールされた時間を超えたときに発生します。 次の例では、1 つ以上のメディアの中断が要求されたかどうか、再生位置が先に移動されたかどうか、先に移動された時間が 10 分未満であるかどうかがチェックされます。 その場合、要求された最初の中断 (イベント引数により開示された [**SeekedOverBreaks**](/uwp/api/windows.media.playback.mediabreakseekedovereventargs.seekedoverbreaks) コレクションから取得されます) が、**MediaPlayer.BreakManager** の [**PlayBreak**](/uwp/api/windows.media.playback.mediabreakmanager.playbreak) メソッドを呼び出すことですぐに再生されます。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaBreaks_RS1/cs/MainPage.xaml.cs" id="SnippetBreakSeekedOver":::


## <a name="access-the-current-playback-session"></a>現在の再生セッションへのアクセス
[**MediaPlaybackSession**](/uwp/api/Windows.Media.Playback.MediaPlaybackSession) オブジェクトは、**MediaPlayer** クラスを使って、現在再生中のメディア コンテンツに関連するデータとイベントを提供します。 [**MediaBreakManager**](/uwp/api/Windows.Media.Playback.MediaBreakManager) にも、再生中のメディアの中断コンテンツに特に関連するデータとイベントを取得するためにアクセスできる **MediaPlaybackSession** があります。 再生セッションから入手できる情報には、現在の再生状態、再生中か一時停止中か、コンテンツ内の現在の再生位置などがあります。 メディアの中断コンテンツの縦横比がメイン コンテンツと異なる場合、[**NaturalVideoWidth**](/uwp/api/windows.media.playback.mediaplaybacksession.naturalvideowidth) プロパティおよび [**NaturalVideoHeight**](/uwp/api/windows.media.playback.mediaplaybacksession.naturalvideoheight) プロパティと [**NaturalVideoSizeChanged**](/uwp/api/windows.media.playback.mediaplaybacksession.naturalvideosizechanged) を使って、ビデオ UI を調整することができます。 アプリのパフォーマンスに関する貴重な利用統計情報を示す、[**BufferingStarted**](/uwp/api/windows.media.playback.mediaplaybacksession.bufferingstarted)、[**BufferingEnded**](/uwp/api/windows.media.playback.mediaplaybacksession.bufferingended)、[**DownloadProgressChanged**](/uwp/api/windows.media.playback.mediaplaybacksession.downloadprogresschanged) などのイベントを受け取ることもできます。

次の例では、**BufferingProgressChanged イベント**のハンドラーを登録します。イベント ハンドラーでは、UI が更新されて現在のバッファリングの進行状況が表示されます。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaBreaks_RS1/cs/MainPage.xaml.cs" id="SnippetRegisterBufferingProgressChanged":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaBreaks_RS1/cs/MainPage.xaml.cs" id="SnippetBufferingProgressChanged":::

## <a name="related-topics"></a>関連トピック
* [メディア再生](media-playback.md)
* [MediaPlayer を使ったオーディオとビデオの再生](play-audio-and-video-with-mediaplayer.md)
* [システム メディア トランスポート コントロールの手動制御](system-media-transport-controls.md)

 

 
