---
ms.assetid: ''
description: この記事では、Open Source Computer Vision Library (OpenCV) と MediaFrameReader クラスを使用する方法について説明します。
title: OpenCV と MediaFrameReader の使用
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, OpenCV
ms.localizationpriority: medium
ms.openlocfilehash: f78197b6108a81f3335dc202585127105bd94339
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363724"
---
# <a name="use-the-open-source-computer-vision-library-opencv-with-mediaframereader"></a>Open Source Computer Vision Library (OpenCV) と MediaFrameReader の使用

この記事では、さまざまな画像処理アルゴリズムを提供するネイティブ コード ライブラリである Open Source Computer Vision Library (OpenCV) を、複数のソースから同時にメディア フレームを読み取ることができる [**MediaFrameReader**](/uwp/api/Windows.Media.Capture.Frames.MediaFrameReader) クラスと共に使用する方法について説明します。 この記事のコード例では、カラー センサーからフレームを取得し、OpenCV ライブラリを使用して各フレームをぼかして、処理された画像を XAML の **Image** コントロールに表示する方法を示します。 

>[!NOTE]
>ImgProc は定期的に更新されず、ストアの準拠チェックにも合格しないため、これらのパッケージは実験のみを目的としています。

この記事は、他の 2 つの記事の内容に基づいています。

* [MediaFrameReader を使ったメディア フレームの処理](process-media-frames-with-mediaframereader.md): この記事では、**MediaFrameReader** を使用して 1 つまたは複数のメディア フレーム ソースからフレームを取得する方法について詳細な情報を提供し、この記事のサンプル コードの多くについて詳しく説明しています。 具体的には、「**MediaFrameReader を使ったメディア フレームの処理**」では、ヘルパー クラス **FrameRenderer** のコード一覧を示します。このクラスは、XAML の **Image** 要素でのメディア フレームのプレゼンテーションを処理します。 この記事のサンプル コードでもこのヘルパー クラスを使用します。

* [Opencv でソフトウェアビットマップを処理](process-software-bitmaps-with-opencv.md)する-この記事では、ネイティブコード Windows ランタイムコンポーネント**OpenCVBridge**を作成する手順について説明します。これは、 **MediaFrameReader**によって使用される software **bitmap**オブジェクトと OpenCV ライブラリで使用される**台紙**の種類との間の変換に役立ちます。 この記事のサンプル コードでは、**OpenCVBridge**コンポーネントを UWP アプリ ソリューションに追加する手順を実行していることを前提としています。

これらの記事に加えて、この記事で説明したシナリオの完全でエンド ツー エンドの実用的なサンプルを表示およびダウンロードするには、Windows ユニバーサル サンプル GitHub リポジトリにある[カメラ フレームと OpenCV のサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraOpenCV)をご覧ください。

開発を迅速に開始するために、NuGet パッケージを使用して UWP アプリプロジェクトに OpenCV ライブラリを含めることができますが、ストアにアプリを送信するときにこれらのパッケージがアプリのアプリケーションを通過しない場合があります。そのため、アプリを送信する前に、OpenCV ライブラリのソースコードをダウンロードし、バイナリをビルドすることをお勧め OpenCV を使用した開発については、「」を参照してください。 [https://opencv.org](https://opencv.org)


## <a name="implement-the-opencvhelper-native-windows-runtime-component"></a>OpenCVHelper ネイティブ Windows ランタイムコンポーネントを実装する
「 [Opencv でソフトウェアビットマップを処理](process-software-bitmaps-with-opencv.md) する」の手順に従って、opencv ヘルパー Windows ランタイムコンポーネントを作成し、コンポーネントプロジェクトへの参照を UWP アプリソリューションに追加します。

## <a name="find-available-frame-source-groups"></a>利用可能なフレーム ソース グループを検索する
最初に、メディア フレームを取得するメディア フレーム ソース グループを見つける必要があります。 **[MediaFrameSourceGroup.FindAllAsync](/uwp/api/windows.media.capture.frames.mediaframesourcegroup.FindAllAsync)** を呼び出して、現在のデバイスで利用可能なソース グループの一覧を取得します。 アプリのシナリオに必要なセンサーの種類を指定するソース グループを選択します。 この例では、RGB カメラからのフレームを提供するソース グループのみが必要です。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.OpenCV.xaml.cs" id="SnippetOpenCVFrameSourceGroups":::

## <a name="initialize-the-mediacapture-object"></a>MediaCapture オブジェクトを初期化する
次に、前の手順で **MediaCaptureInitializationSettings** の **[SourceGroup](/uwp/api/windows.media.capture.mediacaptureinitializationsettings.SourceGroup)** プロパティを設定することによって選択したフレーム ソース グループを使うように、**MediaCapture** オブジェクトを初期化する必要があります。

> [!NOTE] 
> 「[OpenCV でのソフトウェア ビットマップの処理](process-software-bitmaps-with-opencv.md)」で解説する OpenCVHelper コンポーネントで使用されている手法では、処理される画像データが GPU メモリではなく CPU メモリに格納されている必要があります。 そのため、**MediaCaptureInitializationSettings** の **[MemoryPreference](/uwp/api/windows.media.capture.mediacaptureinitializationsettings.MemoryPreference)** フィールドに **MemoryPreference.CPU** を指定する必要があります。

**MediaCapture** オブジェクトが初期化された後、**[MediaCapture.FrameSources](/uwp/api/windows.media.capture.mediacapture.FrameSources)** プロパティにアクセスして、RGB フレーム ソースへの参照を取得します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.OpenCV.xaml.cs" id="SnippetOpenCVInitMediaCapture":::

## <a name="initialize-the-mediaframereader"></a>MediaFrameReader を初期化する
次に、前の手順で取得した RGB フレーム ソースの [**MediaFrameReader**](/uwp/api/Windows.Media.Capture.Frames.MediaFrameReader) を作成します。 適切なフレーム レートを維持するために、センサーの解像度よりも低い解像度のフレームを処理する場合があります。 この例では、省略可能な **[BitmapSize](/uwp/api/windows.graphics.imaging.bitmapsize)** 引数を、**[MediaCapture.CreateFrameReaderAsync](/uwp/api/windows.media.capture.mediacapture.createframereaderasync)** メソッドに対して指定して、フレーム リーダーによって提供されるフレームのサイズを 640 x 480 ピクセルに変更するよう要求しています。

フレーム リーダーを作成した後、**[FrameArrived](/uwp/api/windows.media.capture.frames.mediaframereader.FrameArrived)** イベントのハンドラーを登録します。 次に、新しい **[SoftwareBitmapSource](/uwp/api/windows.ui.xaml.media.imaging.softwarebitmapsource)** オブジェクトを作成します。これは、**FrameRenderer** ヘルパー クラスが処理済みの画像を表示するために使用します。 次に、**FrameRenderer** のコンストラクターを呼び出します。 OpenCVBridge Windows ランタイムコンポーネントで定義されている **OpenCVHelper** クラスのインスタンスを初期化します。 このヘルパー クラスは、各フレームを処理するために **FrameArrived** ハンドラーで使用されます。 最後に、**[StartAsync](/uwp/api/windows.media.capture.frames.mediaframereader.StartAsync)** を呼び出して、フレーム リーダーを開始します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.OpenCV.xaml.cs" id="SnippetOpenCVFrameReader":::


## <a name="handle-the-framearrived-event"></a>FrameArrived イベントを処理する
フレーム リーダーからの新しいフレームが利用可能になると、**FrameArrived** イベントが発生します。 フレームが存在する場合は、**[TryAcquireLatestFrame](/uwp/api/windows.media.capture.frames.mediaframereader.TryAcquireLatestFrame)** を呼び出してフレームを取得します。 **[MediaFrameReference](/uwp/api/windows.media.capture.frames.mediaframereference)** から **SoftwareBitmap** を取得します。 この例で使用されている **CVHelper** クラスでは、画像がプリマルチプライ済みアルファを含む  BRGA8 ピクセル形式を使用している必要があります。 イベントに渡されたフレームが別の形式である場合は、**SoftwareBitmap** を正しい形式に変換します。 次に、ぼかし操作のターゲットとして使用される **SoftwareBitmap** を作成します。 一致する形式のビットマップを作成するために、ソース画像のプロパティがコンストラクターの引数として使用されます。 ヘルパー クラスの **Blur** メソッドを呼び出してフレームを処理します。 最後に、ぼかし操作の出力画像を **PresentSoftwareBitmap** メソッドに渡します。これは、画像が初期化された XAML **Image** コントロールに画像を表示する **FrameRenderer** ヘルパー クラスのメソッドです。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.OpenCV.xaml.cs" id="SnippetOpenCVFrameArrived":::

## <a name="related-topics"></a>関連トピック

* [カメラ](camera.md)
* [MediaCapture を使った基本的な写真、ビデオ、およびオーディオのキャプチャ](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [MediaFrameReader を使ったメディア フレームの処理](process-media-frames-with-mediaframereader.md)
* [OpenCV でのソフトウェア ビットマップの処理](process-software-bitmaps-with-opencv.md)
* [カメラ フレームのサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraFrames)
* [カメラ フレームと OpenCV のサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraOpenCV)
 

 
