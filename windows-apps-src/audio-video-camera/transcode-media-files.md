---
ms.assetid: A1A0D99A-DCBF-4A14-80B9-7106BEF045EC
description: Windows.Media.Transcoding API を使って、ビデオ ファイルをある形式から別の形式にコード変換できます。
title: メディア ファイルのコード変換
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 28ed15ab49f3ec33e382d28c14ffd43bb5c8779d
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363745"
---
# <a name="transcode-media-files"></a>メディア ファイルのコード変換



[**Windows のメディアトランスコード**](/uwp/api/Windows.Media.Transcoding)api を使用して、ビデオファイルをある形式から別の形式に変換できます。

*トランスコード* とは、ビデオやオーディオファイルなどのデジタルメディアファイルを、ある形式から別の形式に変換することです。 通常は、ファイルをデコードしてエンコードし直すことで行います。 たとえば、MP4 形式をサポートするポータブル デバイスで再生できるように、Windows Media ファイルを MP4 に変換できます。 また、高解像度のビデオ ファイルの解像度を下げることもできます。 この場合、再エンコードしたファイルは元のファイルと同じコーデックを使うことがありますが、エンコード プロファイルは異なります。

## <a name="set-up-your-project-for-transcoding"></a>プロジェクトをコード変換用にセットアップする

この記事のコードを使ってメディア ファイルのコード変換を行うには、既定のプロジェクト テンプレートによって参照される名前空間に加えて、これらの名前空間を参照する必要があります。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/TranscodeWin10/cs/MainPage.xaml.cs" id="SnippetUsing":::

## <a name="select-source-and-destination-files"></a>変換元ファイルと変換先ファイルを選択する

アプリでコード変換の変換元ファイルと変換先ファイルを判別する方法は、実装によって異なります。 この例では、[**FileOpenPicker**](/uwp/api/Windows.Storage.Pickers.FileOpenPicker) と [**FileSavePicker**](/uwp/api/Windows.Storage.Pickers.FileSavePicker) を使って、ユーザーが変換元ファイルと変換先ファイルを選べるようにします。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/TranscodeWin10/cs/MainPage.xaml.cs" id="SnippetTranscodeGetFile":::

## <a name="create-a-media-encoding-profile"></a>メディア エンコード プロファイルを作成する

エンコード プロファイルには、変換先ファイルのエンコード方法を決めるための設定が含まれています。 ファイルをコード変換するときのオプションが最も多いのは、このエンコード プロファイルです。

[**MediaEncodingProfile**](/uwp/api/Windows.Media.MediaProperties.MediaEncodingProfile) クラスには、あらかじめ定義されたエンコード プロファイルを作るための静的メソッドが用意されています。

### <a name="methods-for-creating-audio-only-encoding-profiles"></a>オーディオ専用のエンコード プロファイルを作成するメソッド

Method  |プロファイル  |
---------|---------|
[**CreateAlac**](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createalac)     |Apple Lossless Audio Codec (ALAC) オーディオ         |
[**CreateFlac**](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createflac)     |Free Lossless Audio Codec (FLAC) オーディオ         |
[**CreateM4a**](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createm4a)     |AAC オーディオ (M4A)         |
[**CreateMp3**](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createmp3)     |MP3 オーディオ         |
[**CreateWav**](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createwav)     |WAV オーディオ         |
[**CreateWmv**](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createwmv)     |Windows Media Audio (WMA)         |

### <a name="methods-for-creating-audio--video-encoding-profiles"></a>オーディオ/ビデオのエンコード プロファイルを作成するメソッド

Method  |プロファイル  |
---------|---------|
[**CreateAvi**](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createavi) |AVI |
[**CreateHevc**](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createhevc) |High Efficiency Video Coding (HEVC) ビデオ、H.265 ビデオとも呼ばれます |
[**CreateMp4**](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createmp4) |MP4 ビデオ (H.264 ビデオと AAC オーディオ) |
[**CreateWmv**](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createwmv) |Windows Media Video (WMV) |


次のコードでは、MP4 ビデオ用のプロファイルが作成されます。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/TranscodeWin10/cs/MainPage.xaml.cs" id="SnippetTranscodeMediaProfile":::

静的 [**CreateMp4**](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createmp4) メソッドは、MP4 エンコード プロファイルを作ります。 このメソッドのパラメーターで、ビデオのターゲット解像度を指定します。 この場合の [**VideoEncodingQuality.hd720p**](/uwp/api/Windows.Media.MediaProperties.VideoEncodingQuality) は、1280 x 720 ピクセルで 1 秒あたり 30 フレームであることを意味します  ("720p" は、プログレッシブ スキャン方式で 1 フレームあたり 720 本を処理することを表します)。あらかじめ定義されたプロファイルを作るその他のメソッドは、すべてこのパターンに従います。

別の方法として、[**MediaEncodingProfile.CreateFromFileAsync**](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createfromfileasync) メソッドを使って現在のメディア ファイルに一致するプロファイルを作成することもできます。 または、必要なエンコード設定が正確にわかれば、新しい [**MediaEncodingProfile**](/uwp/api/Windows.Media.MediaProperties.MediaEncodingProfile) オブジェクトを作成してプロファイルの詳細を入力できます。

## <a name="transcode-the-file"></a>ファイルのコード変換を行う

ファイルのコード変換を行うには、新しい [**MediaTranscoder**](/uwp/api/Windows.Media.Transcoding.MediaTranscoder) オブジェクトを作成し、[**MediaTranscoder.PrepareFileTranscodeAsync**](/uwp/api/windows.media.transcoding.mediatranscoder.preparefiletranscodeasync) メソッドを呼び出します。 ソース ファイル、出力先ファイル、エンコード プロファイルを渡します。 次に、非同期コード変換操作から返された [**PrepareTranscodeResult**](/uwp/api/Windows.Media.Transcoding.PrepareTranscodeResult) オブジェクト上の [**TranscodeAsync**](/uwp/api/windows.media.transcoding.preparetranscoderesult.transcodeasync) メソッドを呼び出します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/TranscodeWin10/cs/MainPage.xaml.cs" id="SnippetTranscodeTranscodeFile":::

## <a name="respond-to-transcoding-progress"></a>コード変換の進行状況に対して応答する

非同期の [**TranscodeAsync**](/uwp/api/windows.media.transcoding.preparetranscoderesult.transcodeasync) の進行状況が変化したときに応答するイベントを登録できます。 これらのイベントは、ユニバーサル Windows プラットフォーム (UWP) アプリの非同期プログラミング フレームワークの一部であり、コード変換 API に固有のものではありません。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/TranscodeWin10/cs/MainPage.xaml.cs" id="SnippetTranscodeCallbacks":::
