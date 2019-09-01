---
ms.assetid: CC0D6E9B-128D-488B-912F-318F5EE2B8D3
description: この記事では、CameraCaptureUI クラスを使用して、Windows に組み込まれているカメラ UI を使用して写真やビデオをキャプチャする方法について説明します。
title: Windows ビルトインカメラ UI を使用して写真とビデオをキャプチャする
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d582d4815b4fb2168b187a1efff3795cc98aca02
ms.sourcegitcommit: 99595e4938213aafdb49635d684d8ba8eb3f697a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/15/2019
ms.locfileid: "69487805"
---
# <a name="capture-photos-and-video-with-the-windows-built-in-camera-ui"></a>Windows ビルトインカメラ UI を使用して写真とビデオをキャプチャする



この記事では、CameraCaptureUI クラスを使用して、Windows に組み込まれているカメラ UI を使用して写真やビデオをキャプチャする方法について説明します。 この機能は簡単に使用できます。 これにより、わずか数行のコードで、ユーザーがキャプチャした写真またはビデオをアプリで取得できるようになります。

独自のカメラ用 UI を用意する場合、またはキャプチャ操作に対してより堅牢で低レベルな制御が必要な場合は、[**MediaCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture) オブジェクトを使用して、独自のキャプチャ操作を実装する必要があります。 詳しくは、「[MediaCapture を使った基本的な写真、ビデオ、およびオーディオのキャプチャ](basic-photo-video-and-audio-capture-with-MediaCapture.md)」をご覧ください。

> [!NOTE]
> アプリで CameraCaptureUI のみを使用する場合は、アプリマニフェストファイルで**webcam**または**マイク**の機能を指定しないでください。 この操作を行うと、デバイスのカメラのプライバシー設定にアプリが表示されますが、ユーザーがアプリへのカメラアクセスを拒否しても、CameraCaptureUI がメディアをキャプチャできなくなることはありません。 <p>これは、Windows の組み込みのカメラ アプリが、写真、音声、ビデオのキャプチャをボタンを押して開始する必要がある、信頼されているファースト パーティ アプリであるためです。 CameraCaptureUI を唯一の写真キャプチャメカニズムとして使用する場合に、web カメラまたはマイクの機能を指定すると、アプリが Microsoft Store に送信されたときに Windows アプリケーション認定キットの認定に失敗する可能性があります。<p>
MediaCapture を使用してオーディオ、写真、またはビデオをプログラムでキャプチャする場合は、アプリマニフェストファイルで webcam またはマイクの機能を指定する必要があります。

## <a name="capture-a-photo-with-cameracaptureui"></a>CameraCaptureUI を使った写真のキャプチャ

カメラ キャプチャ UI を使うには、プロジェクトに [**Windows.Media.Capture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture) 名前空間を含めます。 返された画像ファイルでファイル操作を行うには、[**Windows.Storage**](https://docs.microsoft.com/uwp/api/Windows.Storage) を含めます。

[!code-cs[UsingCaptureUI](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetUsingCaptureUI)]

写真をキャプチャするには、新しい [**CameraCaptureUI**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.CameraCaptureUI) オブジェクトを作成します。 オブジェクトの[**Photosettings**](https://docs.microsoft.com/uwp/api/windows.media.capture.cameracaptureui.photosettings)プロパティを使用して、写真の画像形式など、返された写真のプロパティを指定できます。 既定では、カメラのキャプチャ UI では、画像が返される前にトリミングがサポートされています。 これは、 [**Allowcropping**](https://docs.microsoft.com/uwp/api/windows.media.capture.cameracaptureuiphotocapturesettings.allowcropping)プロパティを使用して無効にすることができます。 この例では、[**CroppedSizeInPixels**](https://docs.microsoft.com/uwp/api/windows.media.capture.cameracaptureuiphotocapturesettings.croppedsizeinpixels) を設定して、返される画像のサイズが 200 x 200 ピクセルになるよう要求しています。

> [!NOTE]
> **CameraCaptureUI**でのイメージのトリミングは、モバイルデバイスファミリのデバイスではサポートされていません。 アプリがこれらのデバイスで実行されている場合、[**AllowCropping**](https://docs.microsoft.com/uwp/api/windows.media.capture.cameracaptureuiphotocapturesettings.allowcropping) プロパティの値は無視されます。

写真をキャプチャすることを指定するには、[**CaptureFileAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.cameracaptureui.capturefileasync) を呼び出して、[**CameraCaptureUIMode.Photo**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.CameraCaptureUIMode) を指定します。 キャプチャに成功すると、このメソッドは、画像が格納された [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) インスタンスを返します。 ユーザーがキャプチャを取り消した場合、返されるオブジェクトは null になります。

[!code-cs[CapturePhoto](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetCapturePhoto)]

キャプチャした写真が格納されている **StorageFile** は、動的に生成された名前が付けられて、アプリのローカル フォルダーに保存されます。 キャプチャした写真をより効果的に整理するために、ファイルを別のフォルダーに移動できます。

[!code-cs[CopyAndDeletePhoto](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetCopyAndDeletePhoto)]

アプリで写真を使用するために、[**SoftwareBitmap**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap)オブジェクトを作成できます。このオブジェクトは、ユニバーサル Windows アプリのさまざまな機能で使用できます。

最初に、プロジェクトに[**Windows**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging)の画像の名前空間を含めます。

[!code-cs[UsingSoftwareBitmap](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetUsingSoftwareBitmap)]

画像ファイルからストリームを取得するには、[**OpenAsync**](https://docs.microsoft.com/uwp/api/windows.storage.istoragefile.openasync) を呼び出します。 ストリームのビットマップ デコーダーを取得するには、[**BitmapDecoder.CreateAsync**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapdecoder.createasync) を呼び出します。 次に、 [**Get$ bitmap**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapdecoder.getsoftwarebitmapasync)を呼び出して、イメージの**ソフトウェアビットマップ**表現を取得します。

[!code-cs[SoftwareBitmap](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetSoftwareBitmap)]

UI に画像を表示するには、XAML ページで [**Image**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image) コントロールを宣言します。

[!code-xml[ImageControl](./code/CameraCaptureUIWin10/cs/MainPage.xaml#SnippetImageControl)]

XAML ページでソフトウェア ビットマップを使用するには、[**Windows.UI.Xaml.Media.Imaging**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging) 名前空間の using 指定を含めます。

[!code-cs[UsingSoftwareBitmapSource](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetUsingSoftwareBitmapSource)]

**イメージ**コントロールを使用するには、イメージソースの BGRA8 形式が、前乗算されたアルファまたはアルファなしである必要があります。 静的メソッドの software [**bitmap. Convert**](/uwp/api/windows.graphics.imaging.softwarebitmap.convert)を呼び出して、目的の形式の新しいソフトウェアビットマップを作成します。 次に、新しい software [**bitmapsource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.SoftwareBitmapSource)オブジェクトを作成し、 [**setbitmapasync**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.softwarebitmapsource.setbitmapasync)を呼び出して、ソフトウェアビットマップをソースに割り当てます。 最後に、キャプチャした写真を UI に表示できるように、**Image** コントロールの [**Source**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image.source) プロパティを設定します。

[!code-cs[SetImageSource](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetSetImageSource)]

## <a name="capture-a-video-with-cameracaptureui"></a>CameraCaptureUI を使ったビデオのキャプチャ

ビデオをキャプチャするには、新しい [**CameraCaptureUI**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.CameraCaptureUI) オブジェクトを作成します。 オブジェクトの[**Videosettings**](https://docs.microsoft.com/uwp/api/windows.media.capture.cameracaptureui.videosettings)プロパティを使用して、ビデオの形式など、返されるビデオのプロパティを指定できます。

[**CaptureFileAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.cameracaptureui.capturefileasync)を呼び出し、ビデオをキャプチャする[**ビデオ**](https://docs.microsoft.com/uwp/api/windows.media.capture.cameracaptureui.videosettings)を指定します。 キャプチャに成功すると、このメソッドは、ビデオが格納された [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) インスタンスを返します。 キャプチャをキャンセルした場合、返されるオブジェクトは null になります。

[!code-cs[CaptureVideo](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetCaptureVideo)]

キャプチャしたビデオ ファイルをどのように使うかは、アプリのシナリオによって異なります。 この記事の残りの部分では、キャプチャした 1 つ以上のビデオからメディア コンポジションをすばやく作成し、UI に表示する方法を説明します。

まず、XAML ページにビデオコンポジションが表示される[**MediaPlayerElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement)コントロールを追加します。

[!code-xml[MediaElement](./code/CameraCaptureUIWin10/cs/MainPage.xaml#SnippetMediaElement)]


カメラキャプチャ UI からビデオファイルが返されたら、 **[CreateFromStorageFile](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.createfromstoragefile)** を呼び出して新しい[**mediasource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource)を作成します。 **MediaPlayerElement** に関連付けられている既定の **[MediaPlayer](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer)** の **[Play](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.Play)** メソッドを呼び出してビデオを再生します。

[!code-cs[PlayVideo](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetPlayVideo)]
 

## <a name="related-topics"></a>関連トピック

* [カメラ](camera.md)
* [MediaCapture を使用した基本的な写真、ビデオ、オーディオキャプチャ](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [**CameraCaptureUI**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.CameraCaptureUI) 
 

 




