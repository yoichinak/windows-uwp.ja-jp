---
ms.assetid: CC0D6E9B-128D-488B-912F-318F5EE2B8D3
description: この記事では、 [**CameraCaptureUI**](/uwp/api/windows.media.capture.cameracaptureui) クラスを使用して、Windows に組み込まれているカメラ UI を使用して写真やビデオをキャプチャする方法について説明します。
title: Windows の組み込みカメラ UI を使った写真とビデオのキャプチャ
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
ms.openlocfilehash: 5b5e1369e37fc683a3a09c8f404b1998ee06bdab
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/02/2020
ms.locfileid: "89364005"
---
# <a name="capture-photos-and-video-with-the-windows-built-in-camera-ui"></a>Windows の組み込みカメラ UI を使った写真とビデオのキャプチャ

この記事では、 [**CameraCaptureUI**](/uwp/api/windows.media.capture.cameracaptureui) クラスを使用して、Windows に組み込まれているカメラ UI を使用して写真やビデオをキャプチャする方法について説明します。 この機能は簡単に使用できます。 これにより、わずか数行のコードで、ユーザーがキャプチャした写真またはビデオをアプリで取得できるようになります。

独自のカメラ UI を提供する場合、またはシナリオでキャプチャ操作をより堅牢で低レベルで制御する必要がある場合は、 [**MediaCapture**](/uwp/api/Windows.Media.Capture.MediaCapture) クラスを使用して、独自のキャプチャエクスペリエンスを実装する必要があります。 詳しくは、「[MediaCapture を使った基本的な写真、ビデオ、およびオーディオのキャプチャ](basic-photo-video-and-audio-capture-with-MediaCapture.md)」をご覧ください。

> [!NOTE]
> アプリで**CameraCaptureUI**のみを使用する場合は、アプリマニフェストファイルで**web カメラ**や**マイク**機能を指定しないでください。 この操作を行うと、デバイスのカメラのプライバシー設定にアプリが表示されますが、ユーザーがアプリへのカメラアクセスを拒否しても、 **CameraCaptureUI** がメディアをキャプチャできなくなることはありません。 <p>これは、Windows の組み込みのカメラ アプリが、写真、音声、ビデオのキャプチャをボタンを押して開始する必要がある、信頼されているファースト パーティ アプリであるためです。 **CameraCaptureUI**を唯一の写真キャプチャメカニズムとして使用する場合に、web カメラまたはマイクの機能を指定すると、アプリが Microsoft Store に送信されたときに Windows アプリケーション認定キットの認定に失敗する可能性があります。<p>
**MediaCapture**を使用してオーディオ、写真、またはビデオをプログラムでキャプチャする場合は、アプリマニフェストファイルで**webcam**または**マイク**の機能を指定する必要があります。

## <a name="capture-a-photo-with-cameracaptureui"></a>CameraCaptureUI を使った写真のキャプチャ

カメラ キャプチャ UI を使うには、プロジェクトに [**Windows.Media.Capture**](/uwp/api/Windows.Media.Capture) 名前空間を含めます。 返された画像ファイルでファイル操作を行うには、[**Windows.Storage**](/uwp/api/Windows.Storage) を含めます。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cs/MainPage.xaml.cs" id="SnippetUsingCaptureUI":::
:::code language="cppwinrt" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cppwinrt/MainPage.h" id="SnippetUsingCaptureUI":::

写真をキャプチャするには、新しい [**CameraCaptureUI**](/uwp/api/Windows.Media.Capture.CameraCaptureUI) オブジェクトを作成します。 オブジェクトの [**Photosettings**](/uwp/api/windows.media.capture.cameracaptureui.photosettings) プロパティを使用して、写真の画像形式など、返された写真のプロパティを指定できます。 既定では、カメラのキャプチャ UI では、画像が返される前にトリミングがサポートされています。 これは、 [**Allowcropping**](/uwp/api/windows.media.capture.cameracaptureuiphotocapturesettings.allowcropping) プロパティを使用して無効にすることができます。 この例では、[**CroppedSizeInPixels**](/uwp/api/windows.media.capture.cameracaptureuiphotocapturesettings.croppedsizeinpixels) を設定して、返される画像のサイズが 200 x 200 ピクセルになるよう要求しています。

> [!NOTE]
> **CameraCaptureUI**でのイメージのトリミングは、モバイルデバイスファミリのデバイスではサポートされていません。 アプリがこれらのデバイスで実行されている場合、[**AllowCropping**](/uwp/api/windows.media.capture.cameracaptureuiphotocapturesettings.allowcropping) プロパティの値は無視されます。

写真をキャプチャすることを指定するには、[**CaptureFileAsync**](/uwp/api/windows.media.capture.cameracaptureui.capturefileasync) を呼び出して、[**CameraCaptureUIMode.Photo**](/uwp/api/Windows.Media.Capture.CameraCaptureUIMode) を指定します。 キャプチャに成功すると、このメソッドは、画像が格納された [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) インスタンスを返します。 ユーザーがキャプチャを取り消した場合、返されるオブジェクトは null になります。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cs/MainPage.xaml.cs" id="SnippetCapturePhoto":::
:::code language="cppwinrt" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cppwinrt/MainPage.cpp" id="SnippetCapturePhoto":::

キャプチャした写真が格納されている **StorageFile** は、動的に生成された名前が付けられて、アプリのローカル フォルダーに保存されます。 キャプチャした写真をより効果的に整理するために、ファイルを別のフォルダーに移動できます。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cs/MainPage.xaml.cs" id="SnippetCopyAndDeletePhoto":::
:::code language="cppwinrt" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cppwinrt/MainPage.cpp" id="SnippetCopyAndDeletePhoto":::

アプリで写真を使用するために、[**SoftwareBitmap**](/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap)オブジェクトを作成できます。このオブジェクトは、ユニバーサル Windows アプリのさまざまな機能で使用できます。

最初に、プロジェクトに [**Windows**](/uwp/api/Windows.Graphics.Imaging) の画像の名前空間を含めます。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cs/MainPage.xaml.cs" id="SnippetUsingSoftwareBitmap":::
:::code language="cppwinrt" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cppwinrt/MainPage.h" id="SnippetUsingSoftwareBitmap":::

画像ファイルからストリームを取得するには、[**OpenAsync**](/uwp/api/windows.storage.istoragefile.openasync) を呼び出します。 ストリームのビットマップ デコーダーを取得するには、[**BitmapDecoder.CreateAsync**](/uwp/api/windows.graphics.imaging.bitmapdecoder.createasync) を呼び出します。 次に、 [**Get$ bitmap**](/uwp/api/windows.graphics.imaging.bitmapdecoder.getsoftwarebitmapasync) を呼び出して、イメージの **ソフトウェアビットマップ** 表現を取得します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cs/MainPage.xaml.cs" id="SnippetSoftwareBitmap":::
:::code language="cppwinrt" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cppwinrt/MainPage.cpp" id="SnippetSoftwareBitmap":::

UI に画像を表示するには、XAML ページで [**Image**](/uwp/api/Windows.UI.Xaml.Controls.Image) コントロールを宣言します。

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cs/MainPage.xaml" id="SnippetImageControl":::
:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cppwinrt/MainPage.xaml" id="SnippetImageControl":::

XAML ページでソフトウェア ビットマップを使用するには、[**Windows.UI.Xaml.Media.Imaging**](/uwp/api/Windows.UI.Xaml.Media.Imaging) 名前空間の using 指定を含めます。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cs/MainPage.xaml.cs" id="SnippetUsingSoftwareBitmapSource":::
:::code language="cppwinrt" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cppwinrt/MainPage.h" id="SnippetUsingSoftwareBitmapSource":::

**イメージ**コントロールを使用するには、イメージソースの BGRA8 形式が、前乗算されたアルファまたはアルファなしである必要があります。 静的メソッドの software [**bitmap. Convert**](/uwp/api/windows.graphics.imaging.softwarebitmap.convert) を呼び出して、目的の形式の新しいソフトウェアビットマップを作成します。 次に、新しい software [**bitmapsource**](/uwp/api/Windows.UI.Xaml.Media.Imaging.SoftwareBitmapSource) オブジェクトを作成し、 [**setbitmapasync**](/uwp/api/windows.ui.xaml.media.imaging.softwarebitmapsource.setbitmapasync) を呼び出して、ソフトウェアビットマップをソースに割り当てます。 最後に、キャプチャした写真を UI に表示できるように、**Image** コントロールの [**Source**](/uwp/api/windows.ui.xaml.controls.image.source) プロパティを設定します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cs/MainPage.xaml.cs" id="SnippetSetImageSource":::
:::code language="cppwinrt" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cppwinrt/MainPage.cpp" id="SnippetSetImageSource":::

## <a name="capture-a-video-with-cameracaptureui"></a>CameraCaptureUI を使ったビデオのキャプチャ

ビデオをキャプチャするには、新しい [**CameraCaptureUI**](/uwp/api/Windows.Media.Capture.CameraCaptureUI) オブジェクトを作成します。 オブジェクトの [**Videosettings**](/uwp/api/windows.media.capture.cameracaptureui.videosettings) プロパティを使用して、ビデオの形式など、返されるビデオのプロパティを指定できます。

[**CaptureFileAsync**](/uwp/api/windows.media.capture.cameracaptureui.capturefileasync)を呼び出し、ビデオをキャプチャする[**ビデオ**](/uwp/api/windows.media.capture.cameracaptureui.videosettings)を指定します。 キャプチャに成功すると、このメソッドは、ビデオが格納された [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) インスタンスを返します。 キャプチャをキャンセルした場合、返されるオブジェクトは null になります。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cs/MainPage.xaml.cs" id="SnippetCaptureVideo":::
:::code language="cppwinrt" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cppwinrt/MainPage.cpp" id="SnippetCaptureVideo":::

キャプチャしたビデオ ファイルをどのように使うかは、アプリのシナリオによって異なります。 この記事の残りの部分では、キャプチャした 1 つ以上のビデオからメディア コンポジションをすばやく作成し、UI に表示する方法を説明します。

まず、XAML ページにビデオコンポジションが表示される [**MediaPlayerElement**](/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement) コントロールを追加します。

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cs/MainPage.xaml" id="SnippetMediaElement":::

カメラキャプチャ UI からビデオファイルが返されたら、 **[CreateFromStorageFile](/uwp/api/windows.media.core.mediasource.createfromstoragefile)** を呼び出して新しい[**mediasource**](/uwp/api/windows.media.core.mediasource)を作成します。 **MediaPlayerElement** に関連付けられている既定の **[MediaPlayer](/uwp/api/windows.media.playback.mediaplayer)** の **[Play](/uwp/api/windows.media.playback.mediaplayer.Play)** メソッドを呼び出してビデオを再生します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cs/MainPage.xaml.cs" id="SnippetPlayVideo":::
:::code language="cppwinrt" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cppwinrt/MainPage.cpp" id="SnippetPlayVideo":::

## <a name="related-topics"></a>関連トピック

* [カメラ](camera.md)
* [MediaCapture を使った基本的な写真、ビデオ、およびオーディオのキャプチャ](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [CameraCaptureUI](/uwp/api/Windows.Media.Capture.CameraCaptureUI)
