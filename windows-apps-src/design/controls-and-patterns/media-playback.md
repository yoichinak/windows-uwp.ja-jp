---
Description: ビデオ、オーディオ、および画像を表示したり聴いたりするには、メディア プレーヤーを使います。
title: メディア プレーヤー
ms.assetid: 9AABB5DE-1D81-4791-AB47-7F058F64C491
dev.assetid: AF2F2008-9B53-430C-BBC3-8888F631B0B0
label: Media player
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 89890c0657a69535099faf8bb1e591cc1e45b6da
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172586"
---
# <a name="media-player"></a>メディア プレーヤー



ビデオやオーディオを表示したり聴いたりするには、メディア プレーヤーを使います。 メディアはインラインで (ページに埋め込むか、その他のコントロールのグループを使う) 再生するか、専用の全画面表示で再生できます。 プレーヤーのボタン セットやコントロール バーの背景を変更したり、必要に応じてレイアウトを整理したりできます。 ユーザーが必要とするのは基本的なコントロール セット (再生/一時停止、巻き戻し、早送り) です。

![トランスポート コントロールを含むメディア プレーヤー要素](images/controls/mtc_double_video_inprod.png)

> **重要な API**:[MediaPlayerElement クラス](/uwp/api/windows.ui.xaml.controls.mediaplayerelement)、[MediaTransportControls クラス](/uwp/api/windows.ui.xaml.controls.mediatransportcontrols)


> [!NOTE]
> **MediaPlayerElement** は Windows 10 バージョン 1607 以降でのみ使用できます。 Windows 10 の以前のバージョン用にアプリを開発する場合は、代わりに [MediaElement](/uwp/api/Windows.UI.Xaml.Controls.MediaElement) を使用する必要があります。 このページの推奨事項はすべて MediaElement にも適用されます。

## <a name="is-this-the-right-control"></a>これは適切なコントロールですか?

アプリでオーディオまたはビデオを再生する場合は、メディア プレーヤーを使います。 画像のコレクションを表示するには、[フリップ ビュー](flipview.md)を使います。

## <a name="examples"></a>例

<table>
<th align="left">XAML コントロール ギャラリー<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML コントロール ギャラリー</strong> アプリがインストールされている場合、こちらをクリックしてアプリを開き、<a href="xamlcontrolsgallery:/item/MediaPlayerElement">MediaPlayerElement</a> または <a href="xamlcontrolsgallery:/item/MediaPlayer">MediaPlayer</a> の動作を確認してください。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML コントロール ギャラリー アプリを入手する (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">ソース コード (GitHub) を入手する</a></li>
    </ul>
</td>
</tr>
</table>

Windows 10 Get Started アプリのメディア プレイヤー。

![Windows 10 Get Started アプリのメディア要素](images/control-examples/mtc_getstarted_example.png)

## <a name="create-a-media-player"></a>メディア プレーヤーの作成
XAML で [MediaPlayerElement](/uwp/api/windows.ui.xaml.controls.mediaplayerelement) オブジェクトを作成してアプリにメディアを追加し、オーディオやビデオ ファイルを指定する [MediaSource](/uwp/api/windows.media.core.mediasource) に [Source](/uwp/api/windows.ui.xaml.controls.mediaplayerelement.source) を設定します。

この XAML は [MediaPlayerElement](/uwp/api/windows.ui.xaml.controls.mediaplayerelement) を作成し、その [Source](/uwp/api/windows.ui.xaml.controls.mediaplayerelement.source) プロパティをアプリのローカルにあるビデオ ファイルの URI に設定するコードを示します。 ページが読み込まれると、**MediaPlayerElement** によって再生が開始します。 メディアがすぐに再生されないようにするには、[AutoPlay](/uwp/api/windows.ui.xaml.controls.mediaplayerelement.autoplay) プロパティを **false** に設定します。

```xaml
<MediaPlayerElement x:Name="mediaSimple"
                    Source="ms-appx:///Videos/video1.mp4"
                    Width="400" AutoPlay="True"/>
```

この XAML は、組み込みのトランスポート コントロールを有効化し、さらに [AutoPlay](/uwp/api/windows.ui.xaml.controls.mediaplayerelement.autoplay) プロパティを **false** に設定した [MediaPlayerElement](/uwp/api/windows.ui.xaml.controls.mediaplayerelement) を作成します。


```xaml
<MediaPlayerElement x:Name="mediaPlayer"
                    Source="ms-appx:///Videos/video1.mp4"
                    Width="400"
                    AutoPlay="False"
                    AreTransportControlsEnabled="True"/>
```

### <a name="media-transport-controls"></a>メディア トランスポート コントロール
[MediaPlayerElement](/uwp/api/windows.ui.xaml.controls.mediaplayerelement) には、再生、停止、一時停止、音量、ミュート、シーク (進行状況)、字幕、オーディオ トラックの選択を処理する組み込みのトランスポート コントロールがあります。 これらのコントロールを有効にするには、[AreTransportControlsEnabled](/uwp/api/windows.ui.xaml.controls.mediaplayerelement.AreTransportControlsEnabled) を **true** に設定します。 これらのコントロールを無効にするには、**AreTransportControlsEnabled** を **false** に設定します。 トランスポート コントロールは、[MediaTransportControls](/uwp/api/Windows.UI.Xaml.Controls.MediaTransportControls) クラスで表されます。 トランスポート コントロールは、そのまま使用することも、さまざまな方法でカスタマイズすることもできます。 詳しくは、[MediaTransportControls](/uwp/api/Windows.UI.Xaml.Controls.MediaTransportControls) クラスのリファレンスと「[Create custom transport controls](custom-transport-controls.md)」をご覧ください。

トランスポート コントロールは 1 行および 2 行のレイアウトをサポートします。 最初の例は、メディアのタイムラインの左側に再生/一時停止ボタンを配置した 1 行のレイアウトです。 このレイアウトは、インライン メディア再生とコンパクトな画面に適しています。

![1 行の MTC コントロールの例](images/controls/mtc_single_inprod_02.png)

ほとんどの使用シナリオ (特に大きな画面) では、2 行のコントロールのレイアウト (下の図) をお勧めします。 このレイアウトでは、コントロールの領域がより多く確保されており、ユーザーが簡単にタイムラインを操作できます。

![2 行の MTC コントロールの例](images/controls/mtc_double_inprod.png)

**システム メディア トランスポート コントロール**

[MediaPlayerElement](/uwp/api/windows.ui.xaml.controls.mediaplayerelement) は、システム メディア トランスポート コントロールと自動的に統合されます。 システム メディア トランスポート コントロールは、キーボードのメディア ボタンなどのハードウェア メディア キーを押すとポップアップするコントロールです。 詳しくは、[SystemMediaTransportControls](/uwp/api/Windows.Media.SystemMediaTransportControls) をご覧ください。

> **注**&nbsp;&nbsp; [MediaElement](/uwp/api/Windows.UI.Xaml.Controls.MediaElement) は、システム メディア トランスポート コントロールと自動的に統合されないため、自分で接続する必要があります。 詳しくは、「[システム メディア トランスポート コントロール](../../audio-video-camera/system-media-transport-controls.md)」をご覧ください。


### <a name="set-the-media-source"></a>メディア ソースを設定する
ネットワーク上のファイルまたはアプリに埋め込まれたファイルを再生する場合は、ファイルのパスを使用して [Source](/uwp/api/windows.ui.xaml.controls.mediaplayerelement.source) プロパティを [MediaSource](/uwp/api/windows.media.core.mediasource) に設定します。

**ヒント**  インターネットからファイルを開くには、アプリのマニフェスト (Package.appxmanifest) で **Internet (Client)** 機能を宣言する必要があります。 機能の宣言について詳しくは、「[アプリ機能の宣言](../../packaging/app-capability-declarations.md)」をご覧ください。

 

次のコードでは、XAML で定義した [MediaPlayerElement](/uwp/api/windows.ui.xaml.controls.mediaplayerelement) の [Source](/uwp/api/windows.ui.xaml.controls.mediaplayerelement.source) プロパティを、[TextBox](/uwp/api/Windows.UI.Xaml.Controls.TextBox) に入力したファイルのパスに設定してみます。

```xaml
<TextBox x:Name="txtFilePath" Width="400"
         FontSize="20"
         KeyUp="TxtFilePath_KeyUp"
         Header="File path"
         PlaceholderText="Enter file path"/>
```

```csharp
private void TxtFilePath_KeyUp(object sender, KeyRoutedEventArgs e)
{
    if (e.Key == Windows.System.VirtualKey.Enter)
    {
        TextBox tbPath = sender as TextBox;

        if (tbPath != null)
        {
            LoadMediaFromString(tbPath.Text);
        }
    }
}

private void LoadMediaFromString(string path)
{
    try
    {
        Uri pathUri = new Uri(path);
        mediaPlayer.Source = MediaSource.CreateFromUri(pathUri);
    }
    catch (Exception ex)
    {
        if (ex is FormatException)
        {
            // handle exception.
            // For example: Log error or notify user problem with file
        }
    }
}
```

メディア ソースをアプリに埋め込まれたメディア ファイルに設定するには、**ms-appx:///** で始まるパスで [Uri](/uwp/api/windows.foundation.uri) を初期化し、その Uri で [MediaSource](/uwp/api/windows.media.core.mediasource) を作成してから、[Source](/uwp/api/windows.ui.xaml.controls.mediaplayerelement.source) をその Uri に設定します。 たとえば、**Videos** サブフォルダーにある **video1.mp4** というファイルのパスは、**ms-appx:///Videos/video1.mp4** のようになります。

次のコードは、XAML で以前に定義した [MediaPlayerElement](/uwp/api/windows.ui.xaml.controls.mediaplayerelement) の [Source](/uwp/api/windows.ui.xaml.controls.mediaplayerelement.source) プロパティを **ms-appx:///Videos/video1.mp4** に設定します。

```csharp
private void LoadEmbeddedAppFile()
{
    try
    {
        Uri pathUri = new Uri("ms-appx:///Videos/video1.mp4");
        mediaPlayer.Source = MediaSource.CreateFromUri(pathUri);
    }
    catch (Exception ex)
    {
        if (ex is FormatException)
        {
            // handle exception.
            // For example: Log error or notify user problem with file
        }
    }
}
```

### <a name="open-local-media-files"></a>ローカル メディア ファイルを開く
ローカル システムや OneDrive のファイルを開くには、[FileOpenPicker](/uwp/api/Windows.Storage.Pickers.FileOpenPicker) を使ってファイルを取得し、[Source](/uwp/api/windows.ui.xaml.controls.mediaplayerelement.source) を使ってメディア ソースを設定します。または、プログラムによってユーザーのメディア フォルダーにアクセスすることもできます。

アプリがユーザーの操作なしで、**Music** または **Video** フォルダーにアクセスする必要がある場合、たとえばユーザーのコレクションのすべての音楽ファイルやビデオ ファイルを列挙し、アプリで表示する場合は、**音楽ライブラリ**および**ビデオ ライブラリ**機能を宣言する必要があります。 詳しくは、「[ミュージック、画像、およびビデオ ライブラリのファイルとフォルダー](../../files/quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md)」をご覧ください。

ユーザーはどのファイルにアクセスしているかを完全に制御できるので、[FileOpenPicker](/uwp/api/Windows.Storage.Pickers.FileOpenPicker) には、ユーザーの **Music** または **Video** フォルダーなど、ローカル ファイル システム上のファイルにアクセスするための特別な機能は必要ありません。 セキュリティとプライバシーの観点から、アプリで使用する機能の数は最小限にすることをお勧めします。

**FileOpenPicker を使用してローカル メディア開くには**

1.  ユーザーがメディア ファイルを選べるようにするには、[FileOpenPicker](/uwp/api/Windows.Storage.Pickers.FileOpenPicker) を呼び出します。

    [FileOpenPicker](/uwp/api/Windows.Storage.Pickers.FileOpenPicker) クラスを使って、メディア ファイルを選びます。 **FileOpenPicker** が表示するファイルの種類を指定する [FileTypeFilter](/uwp/api/windows.storage.pickers.fileopenpicker.filetypefilter) を設定します。 [PickSingleFileAsync](/uwp/api/windows.storage.pickers.fileopenpicker.picksinglefileasync) を呼び出して、ファイル ピッカーを起動し、ファイルを取得します。

2.  [MediaSource](/uwp/api/windows.media.core.mediasource) を使用して、選んだメディア ファイルを [MediaPlayerElement.Source](/uwp/api/windows.ui.xaml.controls.mediaplayerelement.source) として設定します。

    [FileOpenPicker](/uwp/api/Windows.Storage.Pickers.FileOpenPicker) から返された [StorageFile](/uwp/api/Windows.Storage.StorageFile) を使用するには、[MediaSource](/uwp/api/windows.media.core.mediasource) で [CreateFromStorageFile](/uwp/api/windows.media.core.mediasource.createfromstoragefile) メソッドを呼び出して、それを [MediaPlayerElement](/uwp/api/windows.ui.xaml.controls.mediaplayerelement) の [Source](/uwp/api/windows.ui.xaml.controls.mediaplayerelement.source) として設定する必要があります。 その後、[MediaPlayerElement.MediaPlayer](/uwp/api/windows.ui.xaml.controls.mediaplayerelement.mediaplayer) で [Play](/uwp/api/windows.media.playback.mediaplayer.play) を呼び出して、メディアを開始します。


この例は、[FileOpenPicker](/uwp/api/Windows.Storage.Pickers.FileOpenPicker) を使ってファイルを選び、そのファイルを [MediaPlayerElement](/uwp/api/windows.ui.xaml.controls.mediaplayerelement) の [Source](/uwp/api/windows.ui.xaml.controls.mediaplayerelement.source) に設定する方法を示しています。

```xaml
<MediaPlayerElement x:Name="mediaPlayer"/>
...
<Button Content="Choose file" Click="Button_Click"/>
```

```csharp
private async void Button_Click(object sender, RoutedEventArgs e)
{
    await SetLocalMedia();
}

async private System.Threading.Tasks.Task SetLocalMedia()
{
    var openPicker = new Windows.Storage.Pickers.FileOpenPicker();

    openPicker.FileTypeFilter.Add(".wmv");
    openPicker.FileTypeFilter.Add(".mp4");
    openPicker.FileTypeFilter.Add(".wma");
    openPicker.FileTypeFilter.Add(".mp3");

    var file = await openPicker.PickSingleFileAsync();

    // mediaPlayer is a MediaPlayerElement defined in XAML
    if (file != null)
    {
        mediaPlayer.Source = MediaSource.CreateFromStorageFile(file);

        mediaPlayer.MediaPlayer.Play();
    }
}
```

### <a name="set-the-poster-source"></a>ポスター ソースを設定する
[PosterSource](/uwp/api/windows.ui.xaml.controls.mediaplayerelement.PosterSource) プロパティを使って、メディアの読み込みが終わるまで  [MediaPlayerElement](/uwp/api/windows.ui.xaml.controls.mediaplayerelement) に視覚的な表示を提供することができます。 **PosterSource** は、スクリーン ショットや映画のポスターなど、メディアの代わりに表示される画像です。 **PosterSource** は、次のような状況で表示されます。

-   有効なソースが設定されていないとき。 たとえば、[Source](/uwp/api/windows.ui.xaml.controls.mediaplayerelement.source) が設定されていないとき、**Source** が **Null** に設定されているとき、またはソースが無効であるとき ([MediaFailed](/uwp/api/windows.media.playback.mediaplayer.mediafailed) イベントが発生したときと同様) です。
-   メディアの読み込み中。 たとえば、有効なソースが設定されていても、[MediaOpened](/uwp/api/windows.media.playback.mediaplayer.mediaopened) イベントがまだ発生していないときです。
-   別のデバイスにメディアをストリーミングしているとき。
-   メディアがオーディオのみであるとき。

[Source](/uwp/api/windows.ui.xaml.controls.mediaplayerelement.source) がアルバムのトラックに設定され、[PosterSource](/uwp/api/windows.ui.xaml.controls.mediaplayerelement.PosterSource) がアルバムの表紙の画像を設定された [MediaPlayerElement](/uwp/api/windows.ui.xaml.controls.mediaplayerelement) を以下に示します。

```xaml
<MediaPlayerElement Source="ms-appx:///Media/Track1.mp4" PosterSource="Media/AlbumCover.png"/>
```

### <a name="keep-the-devices-screen-active"></a>デバイスの画面をアクティブに維持する
通常、ユーザーがいないときはバッテリーを節約するために画面が暗くなり、最終的には電源がオフになりますが、ビデオ アプリでは、ユーザーがビデオを見られるように画面をオンのままにしておく必要があります。 アプリでビデオを再生しているときなど、無操作状態が検出されてもディスプレイの電源が切れないようにするには、[DisplayRequest.RequestActive](/uwp/api/windows.system.display.displayrequest.requestactive) を呼び出します。 [DisplayRequest](/uwp/api/Windows.System.Display.DisplayRequest) クラスを使うと、ユーザーがビデオを見られるように画面をオンのままにするよう Windows に指示することができます。

消費電力とバッテリーの駆動時間を節約するため、不要になったら、[DisplayRequest.RequestRelease](/uwp/api/windows.system.display.displayrequest.requestrelease) を呼び出して表示要求を解放してください。 Windows は、アプリが画面から消されると自動的にアプリのアクティブな表示要求を非アクティブ化し、アプリがフォアグラウンドに戻ると再びアクティブ化します。

表示要求を解放する必要があるのは、次のような場合です。

-   ユーザーの操作、バッファリング、限られた帯域幅のための調整などでビデオの再生が一時停止になる。
-   再生が停止する。 たとえば、ビデオの再生が完了したり、プレゼンテーションが終了したりする。
-   再生エラーが発生した。 たとえば、ネットワーク接続の問題や破損したファイル。

> **注**&nbsp;&nbsp;[MediaPlayerElement.IsFullWindow](/uwp/api/windows.ui.xaml.controls.mediaplayerelement.IsFullWindow) が true に設定されていて、メディアが再生中である場合、ディスプレイは自動的に非アクティブ化されなくなります。

**画面をアクティブに維持するには**

1.  [DisplayRequest](/uwp/api/Windows.System.Display.DisplayRequest) グローバル変数を作成します。 null に初期化します。
```csharp
// Create this variable at a global scope. Set it to null.
private DisplayRequest appDisplayRequest = null;
```

2.  [RequestActive](/uwp/api/windows.system.display.displayrequest.requestactive) を呼び出して、アプリで表示をオンのままにする必要があることを Windows に通知します。

3.  ビデオの再生が再生エラーによって停止、一時停止、中断したときには必ず、[RequestRelease](/uwp/api/windows.system.display.displayrequest.requestrelease) を呼び出して表示要求を解放します。 アプリにアクティブな表示要求がなくなった場合、Windows は、デバイスが使われていないときには表示を暗くし、最終的には電源をオフにしてバッテリーを節約します。

    各 [MediaPlayerElement.MediaPlayer](/uwp/api/windows.ui.xaml.controls.mediaplayerelement.mediaplayer) には、[PlaybackRate](/uwp/api/windows.media.playback.mediaplaybacksession.playbackrate)、[PlaybackState](/uwp/api/windows.media.playback.mediaplaybacksession.playbackstate)、[Position](/uwp/api/windows.media.playback.mediaplaybacksession.position) など、メディア再生のさまざまな側面を制御する [MediaPlaybackSession](/uwp/api/windows.media.playback.mediaplaybacksession) 型の [PlaybackSession](/uwp/api/windows.media.playback.mediaplayer.playbacksession) があります。 ここでは、[MediaPlayer.PlaybackSession](/uwp/api/windows.media.playback.mediaplayer.playbacksession) で [PlaybackStateChanged](/uwp/api/windows.media.playback.mediaplaybacksession.playbackstatechanged) イベントを使って、表示要求を解放する必要がある状況を検出します。 次に、[NaturalVideoHeight](/uwp/api/windows.media.playback.mediaplaybacksession.naturalvideoheight) プロパティを使って、オーディオ ファイルとビデオ ファイルのどちらが再生されているかを確認し、ビデオが再生されている場合にのみ画面をアクティブなままにします。

    ```xaml
    <MediaPlayerElement x:Name="mpe" Source="ms-appx:///Media/video1.mp4"/>
    ```

    ```csharp
    protected override void OnNavigatedTo(NavigationEventArgs e)
    {
        mpe.MediaPlayer.PlaybackSession.PlaybackStateChanged += MediaPlayerElement_CurrentStateChanged;
        base.OnNavigatedTo(e);
    }

    private void MediaPlayerElement_CurrentStateChanged(object sender, RoutedEventArgs e)
    {
        MediaPlaybackSession playbackSession = sender as MediaPlaybackSession;
        if (playbackSession != null && playbackSession.NaturalVideoHeight != 0)
        {
            if(playbackSession.PlaybackState == MediaPlaybackState.Playing)
            {
                if(appDisplayRequest == null)
                {
                    // This call creates an instance of the DisplayRequest object
                    appDisplayRequest = new DisplayRequest();
                    appDisplayRequest.RequestActive();
                }
            }
            else // PlaybackState is Buffering, None, Opening or Paused
            {
                if(appDisplayRequest != null)
                {
                      // Deactivate the displayr request and set the var to null
                      appDisplayRequest.RequestRelease();
                      appDisplayRequest = null;
                }
            }
        }

    }
    ```

### <a name="control-the-media-player-programmatically"></a>プログラムでメディア プレーヤーを制御する
[MediaPlayerElement](/uwp/api/windows.ui.xaml.controls.mediaplayerelement) には、[MediaPlayerElement.MediaPlayer](/uwp/api/windows.ui.xaml.controls.mediaplayerelement.mediaplayer) プロパティを介してオーディオやビデオの再生を制御するプロパティ、メソッド、イベントが多数用意されています。 プロパティ、メソッド、イベントの完全な一覧については、[MediaPlayer](/uwp/api/windows.media.playback.mediaplayer) のリファレンス ページをご覧ください。

### <a name="advanced-media-playback-scenarios"></a>高度なメディア再生のシナリオ
プレイリストを再生するような複雑なメディア再生のシナリオでは、オーディオ言語間を切り替えたり、カスタム メタデータ トラックを作成したりするため、[MediaPlayerElement.Source](/uwp/api/windows.ui.xaml.controls.mediaplayerelement.source) を [MediaPlaybackItem](/uwp/api/windows.media.playback.mediaplaybackitem) または [MediaPlaybackList](/uwp/api/windows.media.playback.mediaplaybacklist) に設定します。 さまざまな高度なメディア機能を有効にする方法について詳しくは、[メディアの再生](../../audio-video-camera/media-playback-with-mediasource.md)に関するページをご覧ください。

### <a name="enable-full-window-video-rendering"></a>フル ウィンドウのビデオ レンダリングを有効にする

フル ウィンドウのレンダリングを有効または無効にするには、[IsFullWindow](/uwp/api/windows.ui.xaml.controls.mediaplayerelement.isfullwindow) プロパティを設定します。 プログラムを使ってアプリにフル ウィンドウのレンダリングを設定する場合、手動で行う代わりに **IsFullWindow** を常に使う必要があります。 **IsFullWindow** により、システム レベルの最適化が実行され、パフォーマンスとバッテリーの寿命が向上します。 フル ウィンドウのレンダリングが正しく設定されていない場合、これらの最適化が有効になっていない可能性があります。

フル ウィンドウのレンダリングを切り替える [AppBarButton](/uwp/api/Windows.UI.Xaml.Controls.AppBarButton) を作成するコードを次に示します。

```xaml
<AppBarButton Icon="FullScreen"
              Label="Full Window"
              Click="FullWindow_Click"/>>
```

```csharp
private void FullWindow_Click(object sender, object e)
{
    mediaPlayer.IsFullWindow = !media.IsFullWindow;
}
```

### <a name="resize-and-stretch-video"></a>ビデオのサイズを変更し、拡大する

[Stretch](/uwp/api/windows.ui.xaml.controls.mediaplayerelement.stretch) プロパティを使って、コンテナー内でのビデオ コンテンツや [PosterSource](/uwp/api/windows.ui.xaml.controls.mediaplayerelement.postersource) のサイズを変更します。 この要素は、[Stretch](/uwp/api/Windows.UI.Xaml.Media.Stretch) の値に応じてビデオのサイズ変更と拡大を行います。 **Stretch** 状態は、多くのテレビ セットの画像サイズの設定に似ています。 ボタンにフックしてユーザーが好みの設定を選ぶことができるようにします。

-   [None](/uwp/api/Windows.UI.Xaml.Media.Stretch) は、元のサイズでコンテンツのネイティブの解像度を表示します。
-   [Uniform](/uwp/api/Windows.UI.Xaml.Media.Stretch) は、縦横比、画像コンテンツを維持したままスペースを最大限に使用します。 これにより、ビデオの端に水平方向または垂直方向の黒いバーが表示されることがあります。 これはワイドスクリーン モードに似ています。
-   [UniformToFill](/uwp/api/Windows.UI.Xaml.Media.Stretch) は、縦横比を維持したままスペース全体を使用します。 これにより、画像の一部がトリミングされることがあります。 これは全画面モードに似ています。
-   [Fill](/uwp/api/Windows.UI.Xaml.Media.Stretch) は、縦横比を維持せずに、スペース全体を使用します。 画像はトリミングされませんが、拡大されることがあります。 これはストレッチ モードに似ています。

![Stretch 列挙値](images/Image_Stretch.jpg)

ここでは、[AppBarButton](/uwp/api/Windows.UI.Xaml.Controls.AppBarButton) を使って、[Stretch](/uwp/api/Windows.UI.Xaml.Media.Stretch) オプションを順に切り替えます。 **switch** ステートメントは、[Stretch](/uwp/api/windows.ui.xaml.controls.mediaelement.stretch) プロパティの現在の状態をチェックし、**Stretch** 列挙で次の値を設定します。 これにより、ユーザーはさまざまな拡大の状態を順番に表示することができます。

```xaml
<AppBarButton Icon="Switch"
              Label="Resize Video"
              Click="PictureSize_Click" />
```

```csharp
private void PictureSize_Click(object sender, RoutedEventArgs e)
{
    switch (mediaPlayer.Stretch)
    {
        case Stretch.Fill:
            mediaPlayer.Stretch = Stretch.None;
            break;
        case Stretch.None:
            mediaPlayer.Stretch = Stretch.Uniform;
            break;
        case Stretch.Uniform:
            mediaPlayer.Stretch = Stretch.UniformToFill;
            break;
        case Stretch.UniformToFill:
            mediaPlayer.Stretch = Stretch.Fill;
            break;
        default:
            break;
    }
}
```

### <a name="enable-low-latency-playback"></a>待機時間が短い再生を可能にする

[RealTimePlayback](/uwp/api/windows.media.playback.mediaplayer.realtimeplayback) プロパティを **true** に設定すると、[MediaPlayerElement.MediaPlayer](/uwp/api/windows.ui.xaml.controls.mediaplayerelement.mediaplayer) の再生の最初の待機時間を短くすることができます。 これは双方向通信アプリには重要で、ゲームのシナリオにも適用できる場合があります。 このモードでは、リソースがより多く消費され、電力効率が低下する点に注意してください。

この例では、[MediaPlayerElement](/uwp/api/windows.ui.xaml.controls.mediaplayerelement) を作って、[RealTimePlayback](/uwp/api/windows.media.playback.mediaplayer.realtimeplayback) を **true** に設定します。


```csharp
MediaPlayerElement mp = new MediaPlayerElement();
mp.MediaPlayer.RealTimePlayback = true;
```

## <a name="recommendations"></a>推奨事項

メディア プレイヤーは淡色テーマと濃色テーマの両方をサポートしていますが、ほとんどのエンターテインメント シナリオでは、濃色テーマを使用することでエクスペリエンスが向上します。 暗い背景を使うと、(特に高感度条件では) コントラストが強調され、表示エクスペリエンスに影響を及ぼすコントロール バーが制限されます。

ビデオ コンテンツを再生する場合、インライン モードよりも全画面表示モードを促進することにより、専用の表示エクスペリエンスを使うことをお勧めします。 全画面表示エクスペリエンスが最適であり、インライン モードではオプションが制限されます。

画面領域がある場合や、10 フィート エクスペリエンス向けに設計する場合は、2 行のレイアウトを採用します。 このレイアウトでは、コンパクトな 1 行のレイアウトよりもコントロールの領域が多く確保され、10 フィート環境ではゲームパッドによる移動が簡単になります。

> **注**&nbsp;&nbsp; アプリケーションを 10 フィート エクスペリエンスに最適化する方法について詳しくは、「[Xbox およびテレビ向け設計](../devices/designing-for-tv.md)」の記事をご覧ください。

既定のコントロールはメディア再生に最適化されていますが、アプリに最適なエクスペリエンスを実現するために、必要なカスタム オプションをメディア プレーヤーに追加できます。 カスタム コントロールの追加について詳しくは、「[カスタム トランスポート コントロールを作成する](custom-transport-controls.md)」をご覧ください。

## <a name="get-the-sample-code"></a>サンプル コードの入手

- [XAML コントロール ギャラリー サンプル](https://github.com/Microsoft/Xaml-Controls-Gallery) - インタラクティブな形で XAML コントロールのすべてを参照できます。

## <a name="related-articles"></a>関連記事

- [Windows アプリのコマンド デザインの基本](../basics/commanding-basics.md)
- [Windows アプリのコンテンツ デザインの基本](../basics/content-basics.md)