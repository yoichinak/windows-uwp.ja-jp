---
ms.assetid: C4DB495D-1F91-40EF-A55C-5CABBF3269A2
description: Windows.Media.Editing 名前空間の API を使うと、オーディオやビデオのソース ファイルからメディア コンポジションを作成するアプリを簡単に開発できます。
title: メディア コンポジションと編集
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 91b0418555e4d46c15bd43816ec6b01ebbd27d8b
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363875"
---
# <a name="media-compositions-and-editing"></a>メディア コンポジションと編集



この記事では、[**Windows.Media.Editing**](/uwp/api/Windows.Media.Editing) 名前空間の API を使って、オーディオとビデオのソース ファイルからメディア コンポジションを作成するアプリを開発する方法について説明します。 このフレームワークには、複数のビデオ クリップをまとめて追加したり、ビデオや画像のオーバーレイを追加したり、バックグラウンド オーディオを追加したり、オーディオとビデオの効果を追加したりする作業をプログラムから実行できる機能が備わっています。 作成したメディア コンポジションは、フラットなメディア ファイルにレンダリングして再生したり共有したりできるほか、コンポジションをディスクにシリアル化したりディスクから逆シリアル化したりすることもできるので、過去に作成されたコンポジションを読み込んで変更を加えるような機能をユーザーに提供することができます。 その機能はいずれも、使いやすい Windows ランタイムのインターフェイスとして用意されているため、低レベルの [Microsoft メディア ファンデーション](/windows/desktop/medfound/microsoft-media-foundation-sdk) API と比べて、これらの作業を実行するために必要なコードの量や複雑さは大幅に軽減されます。

## <a name="create-a-new-media-composition"></a>新しいメディア コンポジションを作成する

[**MediaComposition**](/uwp/api/Windows.Media.Editing.MediaComposition) クラスは、コンポジションの構成要素となるすべてのメディア クリップのコンテナーで、最終的なコンポジションのレンダリングや、ディスクからの読み込みとディスクへの保存、UI に表示するプレビュー ストリームの提供などの機能を担います。 **MediaComposition** をアプリで使うには、[**Windows.Media.Editing**](/uwp/api/Windows.Media.Editing) 名前空間に加え、関連する必要な API を含んだ [**Windows.Media.Core**](/uwp/api/Windows.Media.Core) 名前空間を追加する必要があります。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaEditing/cs/MainPage.xaml.cs" id="SnippetNamespace1":::


**MediaComposition** オブジェクトには、コードのいたるところからアクセスすることになるため、それを格納するためのメンバー変数を宣言するのが一般的です。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaEditing/cs/MainPage.xaml.cs" id="SnippetDeclareMediaComposition":::

**MediaComposition** のコンストラクターに引数はありません。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaEditing/cs/MainPage.xaml.cs" id="SnippetMediaCompositionConstructor":::

## <a name="add-media-clips-to-a-composition"></a>コンポジションにメディア クリップを追加する

メディア コンポジションは通常、ビデオ クリップを含んでいます。 ビデオ ファイルをユーザーに選択してもらうには、[**FileOpenPicker**](/uwp/schemas/appxpackage/appxmanifestschema/element-fileopenpicker) を使います。 ファイルが選択されたら、[**MediaClip.CreateFromFileAsync**](/uwp/api/windows.media.editing.mediaclip.createfromfileasync) を呼び出し、そのビデオ クリップを格納するための新しい [**MediaClip**](/uwp/api/Windows.Media.Editing.MediaClip) オブジェクトを作成します。 そのクリップを **MediaComposition** オブジェクトの [**Clips**](/uwp/api/windows.media.editing.mediacomposition.clips) リストに追加します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaEditing/cs/MainPage.xaml.cs" id="SnippetPickFileAndAddClip":::

-   **MediaComposition** には、[**Clips**](/uwp/api/windows.media.editing.mediacomposition.clips) リストと同じ順序でメディア クリップが出現します。

-   **MediaClip** をコンポジションに追加できるのは 1 回だけです。 既にコンポジションで使われている **MediaClip** を追加しようとすると、エラーが発生します。 コンポジションの中でビデオ クリップを複数回にわたって再利用するには、[**Clone**](/uwp/api/windows.media.editing.mediaclip.clone) を呼び出して新しい **MediaClip** オブジェクトを作成し、それをコンポジションに追加してください。

-   ユニバーサル Windows アプリには、ファイル システム全体にアクセスする権限がありません。 [**StorageApplicationPermissions**](/uwp/api/Windows.Storage.AccessCache.StorageApplicationPermissions) クラスの [**FutureAccessList**](/uwp/api/windows.storage.accesscache.storageapplicationpermissions.futureaccesslist) プロパティを使うと、ユーザーによって選択されたファイルの記録をアプリで保存し、ファイルにアクセスするための権限を維持することができます。 **FutureAccessList** の最大エントリ数は 1,000 件です。リストがあふれないようアプリ側で管理する必要があります。 過去に作成されたコンポジションの読み込みと変更をサポートする場合は、この点が特に重要となります。

-   **MediaComposition** は、MP4 形式のビデオ クリップをサポートしています。

-   ビデオ ファイルに複数のオーディオ トラックが埋め込まれている場合、コンポジションに使うオーディオ トラックを [**SelectedEmbeddedAudioTrackIndex**](/uwp/api/windows.media.editing.mediaclip.selectedembeddedaudiotrackindex) プロパティで選ぶことができます。

-   フレーム全体を単色で塗りつぶした **MediaClip** を作成するには、単一色とクリップの再生時間を指定して [**CreateFromColor**](/uwp/api/windows.media.editing.mediaclip.createfromcolor) を呼び出します。

-   **MediaClip** を画像ファイルから作成するには、画像ファイルとクリップの再生時間を指定して [**CreateFromImageFileAsync**](/uwp/api/windows.media.editing.mediaclip.createfromimagefileasync) を呼び出します。

-   **MediaClip** を [**IDirect3DSurface**](/uwp/api/Windows.Graphics.DirectX.Direct3D11.IDirect3DSurface) から作成するには、サーフェスとクリップの再生時間を指定して [**CreateFromSurface**](/uwp/api/windows.media.editing.mediaclip.createfromsurface) を呼び出します。

## <a name="preview-the-composition-in-a-mediaelement"></a>コンポジションを MediaElement でプレビューする

メディア コンポジションをユーザーが表示できるようにするには、UI を定義する XAML ファイルに [**MediaPlayerElement**](/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement) を追加します。

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaEditing/cs/MainPage.xaml" id="SnippetMediaElement":::

[**Mediastreamsource**](/uwp/api/Windows.Media.Core.MediaStreamSource)型のメンバー変数を宣言します。


:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaEditing/cs/MainPage.xaml.cs" id="SnippetDeclareMediaStreamSource":::

**MediaComposition** オブジェクトの [**GeneratePreviewMediaStreamSource**](/uwp/api/windows.media.editing.mediacomposition.generatepreviewmediastreamsource) メソッドを呼び出して、コンポジションの **MediaStreamSource** を作成します。 ファクトリ メソッド [**CreateFromMediaStreamSource**](/uwp/api/windows.media.core.mediasource.createfrommediastreamsource) を呼び出して、[**MediaSource**](/uwp/api/Windows.Media.Core.MediaSource) オブジェクトを作成し、それを **MediaPlayerElement** の [**Source**](/uwp/api/windows.ui.xaml.controls.mediaplayerelement.source) プロパティに割り当てます。 これでコンポジションを UI に表示することができます。


:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaEditing/cs/MainPage.xaml.cs" id="SnippetUpdateMediaElementSource":::

-   [**GeneratePreviewMediaStreamSource**](/uwp/api/windows.media.editing.mediacomposition.generatepreviewmediastreamsource) は、**MediaComposition** にメディア クリップが少なくとも 1 つは存在している状態で呼び出す必要があります。まったく存在しない場合、返されるオブジェクトは null になります。

-   コンポジションの変更を反映するために **MediaElement** のタイムラインが自動的に更新されることはありません。 コンポジションに一連の変更を行って UI の更新が必要になるたびに、**GeneratePreviewMediaStreamSource** を呼び出し、**MediaPlayerElement、** **Source** プロパティを設定することをお勧めします。

ユーザーがページから離れたときは、**MediaPlayerElement** の [**Source**](/uwp/api/windows.ui.xaml.controls.mediaelement.source) プロパティと **MediaStreamSource** オブジェクトを null に設定して、関連付けられているリソースを解放することをお勧めします。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaEditing/cs/MainPage.xaml.cs" id="SnippetOnNavigatedFrom":::

## <a name="render-the-composition-to-a-video-file"></a>コンポジションをビデオ ファイルにレンダリングする

メディア コンポジションをフラット ビデオ ファイルにレンダリングして他のデバイスで共有したり表示したりするためには、[**Windows.Media.Transcoding**](/uwp/api/Windows.Media.Transcoding) 名前空間の API が必要となります。 また、非同期操作の進行状況に応じて UI を更新するには、[**Windows.UI.Core**](/uwp/api/Windows.UI.Core) 名前空間の API が必要となります。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaEditing/cs/MainPage.xaml.cs" id="SnippetNamespace2":::

[**FileSavePicker**](/uwp/api/Windows.Storage.Pickers.FileSavePicker) でユーザーが出力ファイルを選べるようにしたら、**MediaComposition** オブジェクトの [**RenderToFileAsync**](/uwp/api/windows.media.editing.mediacomposition.rendertofileasync) を呼び出し、選択されたファイルにコンポジションをレンダリングします。 以下のコード例の残りの部分は、[**AsyncOperationWithProgress**](/previous-versions/br205807(v=vs.85)) の処理パターンを踏襲しているだけです。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaEditing/cs/MainPage.xaml.cs" id="SnippetRenderCompositionToFile":::

-   トランスコード処理の速度を優先させるか、隣接するメディア クリップのトリミング精度を優先させるかは、[**MediaTrimmingPreference**](/uwp/api/Windows.Media.Editing.MediaTrimmingPreference) で設定することができます。 **Fast** を選んだ場合は、トランスコード処理の速度が上がってトリミングの精度が低下します。一方、**Precise** を選んだ場合は、トランスコード処理の速度が下がってトリミングの精度が向上します。

## <a name="trim-a-video-clip"></a>ビデオ クリップをトリミングする

コンポジションにおけるビデオ クリップの再生時間をトリミングするには、[**MediaClip**](/uwp/api/Windows.Media.Editing.MediaClip) オブジェクトの [**TrimTimeFromStart**](/uwp/api/windows.media.editing.mediaclip.trimtimefromstart) プロパティまたは [**TrimTimeFromEnd**](/uwp/api/windows.media.editing.mediaclip.trimtimefromend) プロパティ、あるいはその両方を設定します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaEditing/cs/MainPage.xaml.cs" id="SnippetTrimClipBeforeCurrentPosition":::

-   任意の UI を使って、トリミングの開始と終了の値をユーザーに指定してもらうことができます。 上の例では、**MediaPlayerElement** に関連付けられた [**MediaPlaybackSession**](/uwp/api/Windows.Media.Playback.MediaPlaybackSession) の [**Position**](/uwp/api/windows.media.playback.mediaplaybacksession.position) プロパティを使い、[**StartTimeInComposition**](/uwp/api/windows.media.editing.mediaclip.starttimeincomposition) と [**EndTimeInComposition**](/uwp/api/windows.media.editing.mediaclip.endtimeincomposition) を確認することによって、コンポジションの現在位置で再生されている **MediaClip** を調べています。 次に、**Position** プロパティと **StartTimeInComposition** プロパティをもう一度使って、クリップの先頭からトリミングする時間を計算します。 **FirstOrDefault** メソッドは、**System.Linq** 名前空間の拡張メソッドです。リストから項目を選択するコードが、このメソッドによって単純化されます。
-   クリッピングが一切適用されていない状態のメディア クリップの再生時間は、**MediaClip** オブジェクトの [**OriginalDuration**](/uwp/api/windows.media.editing.mediaclip.originalduration) プロパティで確認できます。
-   トリミングを適用した後のメディア クリップの再生時間は、[**TrimmedDuration**](/uwp/api/windows.media.editing.mediaclip.trimmedduration) プロパティを使って確認できます。
-   クリップの元の再生時間を超えるトリミング値を指定してもエラーはスローされません。 ただし、コンポジションに含まれているクリップが 1 つだけであるときに、大きなトリミング値を指定したことによって長さがゼロにまでトリミングされた場合、それ以降の [**GeneratePreviewMediaStreamSource**](/uwp/api/windows.media.editing.mediacomposition.generatepreviewmediastreamsource) の呼び出しからは、一見コンポジションにクリップが存在しないかのように null が返されます。

## <a name="add-a-background-audio-track-to-a-composition"></a>コンポジションにバックグラウンド オーディオ トラックを追加する

コンポジションにバックグラウンド トラックを追加するには、オーディオ ファイルを読み込んだ後、ファクトリ メソッド [**BackgroundAudioTrack.CreateFromFileAsync**](/uwp/api/windows.media.editing.backgroundaudiotrack.createfromfileasync) を呼び出して [**BackgroundAudioTrack**](/uwp/api/Windows.Media.Editing.BackgroundAudioTrack) オブジェクトを作成します。 次に、**BackgroundAudioTrack** をコンポジションの [**BackgroundAudioTracks**](/uwp/api/windows.media.editing.mediacomposition.backgroundaudiotracks) プロパティに追加します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaEditing/cs/MainPage.xaml.cs" id="SnippetAddBackgroundAudioTrack":::

-   **MediaComposition** では、MP3、WAV、FLAC の各形式のバックグラウンド オーディオ トラックがサポートされます。

-   バックグラウンド オーディオ トラック。

-   ビデオ ファイルの場合と同様、[**StorageApplicationPermissions**](/uwp/api/Windows.Storage.AccessCache.StorageApplicationPermissions) クラスを使ってコンポジション内のファイルにアクセスする権限を維持することをお勧めします。

-   **MediaClip** の場合と同様、**BackgroundAudioTrack** をコンポジションに追加できるのは 1 回だけです。 既にコンポジションで使われている **BackgroundAudioTrack** を追加しようとすると、エラーが発生します。 コンポジションの中でオーディオ トラックを複数回にわたって再利用するには、[**Clone**](/uwp/api/windows.media.editing.mediaclip.clone) を呼び出して新しい **MediaClip** オブジェクトを作成し、それをコンポジションに追加してください。

-   既定では、コンポジションの開始時にバックグラウンド オーディオ トラックが再生されます。 複数のバックグラウンド トラックが存在する場合、コンポジションの開始時にすべてのトラックの再生が開始されます。 バックグラウンド オーディオ トラックの再生を他のタイミングで開始するには、[**Delay**](/uwp/api/windows.media.editing.backgroundaudiotrack.delay) プロパティに、目的のタイム オフセットを設定してください。

## <a name="add-an-overlay-to-a-composition"></a>コンポジションにオーバーレイを追加する

オーバーレイを使うと、コンポジションの複数のビデオ レイヤーを重ね合わせることができます。 コンポジションには、複数のオーバーレイ レイヤーを含めることができ、それぞれのオーバーレイ レイヤーには複数のオーバーレイを追加することができます。 [**MediaOverlay**](/uwp/api/Windows.Media.Editing.MediaOverlay) オブジェクトは、そのコンストラクターに **MediaClip** を渡すことによって作成します。 オーバーレイの位置と不透明度を設定したら、新しい [**MediaOverlayLayer**](/uwp/api/Windows.Media.Editing.MediaOverlayLayer) を作成し、その [**Overlays**](/windows/desktop/api/dxgi1_3/nf-dxgi1_3-idxgioutput2-supportsoverlays) リストに **MediaOverlay** を追加します。 最後に、その **MediaOverlayLayer** をコンポジションの [**OverlayLayers**](/uwp/api/windows.media.editing.mediacomposition.overlaylayers) リストに追加します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaEditing/cs/MainPage.xaml.cs" id="SnippetAddOverlay":::

-   レイヤーにおけるオーバーレイの Z オーダーは、そのオーバーレイを含んでいるレイヤーの **Overlays** リストにおける順序に基づいて決まります。 リストにおけるインデックスが大きいほど、手前にレンダリングされます。 コンポジションにおけるオーバーレイ レイヤーにも同じことが当てはまります。 コンポジションの **OverlayLayers** リストにおけるインデックスが大きいレイヤーほど、手前にレンダリングされます。

-   オーバーレイは、順に再生されるのではなく上に積み重ねられるため、既定ではコンポジションの開始と共にすべてのオーバーレイが再生されます。 オーバーレイの再生を他のタイミングで開始するには、[**Delay**](/uwp/api/windows.media.editing.mediaoverlay.delay) プロパティに、目的のタイム オフセットを設定してください。

## <a name="add-effects-to-a-media-clip"></a>メディア クリップに効果を追加する

コンポジションに含まれる各 **MediaClip** には、オーディオ効果とビデオ効果のリストがあって、そのリストに複数の効果を追加することができます。 これらの効果にはそれぞれ [**IAudioEffectDefinition**](/uwp/api/Windows.Media.Effects.IAudioEffectDefinition) または [**IVideoEffectDefinition**](/uwp/api/Windows.Media.Effects.IVideoEffectDefinition) が実装されている必要があります。 次の例は、現在の **MediaPlayerElement** 位置を使って表示されている **MediaClip** を選び、[**VideoStabilizationEffectDefinition**](/uwp/api/Windows.Media.Core.VideoStabilizationEffectDefinition) の新しいインスタンスを作成して、メディア クリップの [**VideoEffectDefinitions**](/uwp/api/windows.media.editing.mediaclip.videoeffectdefinitions) リストに追加しています。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaEditing/cs/MainPage.xaml.cs" id="SnippetAddVideoEffect":::

## <a name="save-a-composition-to-a-file"></a>コンポジションをファイルに保存する

メディア コンポジションは、後で変更を加えることができるようにファイルにシリアル化することができます。 出力ファイルを選んだ後、[**MediaComposition**](/uwp/api/Windows.Media.Editing.MediaComposition) の [**SaveAsync**](/uwp/api/windows.media.editing.mediacomposition.saveasync) メソッドを呼び出してコンポジションを保存します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaEditing/cs/MainPage.xaml.cs" id="SnippetSaveComposition":::

## <a name="load-a-composition-from-a-file"></a>コンポジションをファイルから読み込む

メディア コンポジションをファイルから逆シリアル化することによって、コンポジションを表示したり変更を加えたりする機能をユーザーに提供することができます。 コンポジション ファイルを選んだ後、[**MediaComposition**](/uwp/api/Windows.Media.Editing.MediaComposition) の [**LoadAsync**](/uwp/api/windows.media.editing.mediacomposition.loadasync) メソッドを呼び出してコンポジションを読み込みます。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaEditing/cs/MainPage.xaml.cs" id="SnippetOpenComposition":::

-   コンポジション内のメディア ファイルが、アプリがアクセスできる場所になく、アプリの [**StorageApplicationPermissions**](/uwp/api/Windows.Storage.AccessCache.StorageApplicationPermissions) クラスの [**FutureAccessList**](/uwp/api/windows.storage.accesscache.storageapplicationpermissions.futureaccesslist) プロパティに存在しない場合、コンポジションの読み込み時にエラーがスローされます。

 

 
