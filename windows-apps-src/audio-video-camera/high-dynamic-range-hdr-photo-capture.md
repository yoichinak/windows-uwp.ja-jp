---
ms.assetid: 0186EA01-8446-45BA-A109-C5EB4B80F368
description: この記事では、AdvancedPhotoCapture クラスを使って、ハイ ダイナミック レンジ (HDR) とローライトの写真をキャプチャする方法について説明します。
title: ハイ ダイナミック レンジ (HDR) とローライトの写真のキャプチャ
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 2072f1e7fad5c9652200fe067de8abe0afaede2a
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/02/2020
ms.locfileid: "89362655"
---
# <a name="high-dynamic-range-hdr-and-low-light-photo-capture"></a>ハイ ダイナミック レンジ (HDR) とローライトの写真のキャプチャ



この記事では、[**AdvancedPhotoCapture**](/uwp/api/Windows.Media.Capture.AdvancedPhotoCapture) クラスを使って、ハイ ダイナミック レンジ (HDR) の写真をキャプチャする方法について説明します。 最終的な画像の処理が完了する前に、この API を使って、HDR キャプチャから参照フレームを取得することもできます。

HDR キャプチャに関連したその他の記事を以下に示します。

-   [**SceneAnalysisEffect**](/uwp/api/Windows.Media.Core.SceneAnalysisEffect) でメディア キャプチャのプレビュー ストリームの内容をシステムで評価し、HDR 処理によるキャプチャ結果の向上が期待できるかどうかを判断できます。 詳しくは、「[メディア キャプチャのシーン分析](scene-analysis-for-media-capture.md)」をご覧ください。

-   [**HdrVideoControl**](/uwp/api/Windows.Media.Devices.HdrVideoControl) で、Windows に組み込まれている HDR 処理アルゴリズムを使ってビデオをキャプチャします。 詳しくは、「[ビデオ キャプチャのためのキャプチャ デバイス コントロール](capture-device-controls-for-video-capture.md)」をご覧ください。

-   [**VariablePhotoSequenceCapture**](/uwp/api/Windows.Media.Capture.Core.VariablePhotoSequenceCapture) を使うと、キャプチャ設定がそれぞれ異なる一連の写真をキャプチャすることができます。HDR またはその他の処理アルゴリズムを独自に実装することが可能です。 詳細については、「 [可変フォトシーケンス](variable-photo-sequence.md)」を参照してください。



> [!NOTE] 
> Windows 10、バージョン 1709 以降では、ビデオ録画と **AdvancedPhotoCapture** の使用の同時実行がサポートされています。  これは、それより前のバージョンではサポートされていません。 この変更により、**[LowLagMediaRecording](/uwp/api/windows.media.capture.lowlagmediarecording)** と**[AdvancedPhotoCapture](/uwp/api/windows.media.capture.advancedphotocapture)** の準備を同時に実行できるようになりました。 **[MediaCapture.PrepareAdvancedPhotoCaptureAsync](/uwp/api/windows.media.capture.mediacapture.prepareadvancedphotocaptureasync)** と **[AdvancedPhotoCapture.FinishAsync](/uwp/api/windows.media.capture.advancedphotocapture.FinishAsync)** の呼び出しの間に、ビデオ録画を開始または停止できます。 またビデオの録画中に、**[AdvancedPhotoCapture.CaptureAsync](/uwp/api/windows.media.capture.advancedphotocapture.CaptureAsync)** を呼び出すこともできます。 ただし、ビデオの録画中の HDR 写真のキャプチャなど、一部の **AdvancedPhotoCapture** のシナリオでは、HDR キャプチャによって一部のビデオ フレームが変更され、好ましくないユーザー エクスペリエンスが生じることがあります。 このため、ビデオの録画中は、**[AdvancedPhotoControl.SupportedModes](/uwp/api/windows.media.devices.advancedphotocontrol.SupportedModes)** によって返されるモードが通常とは異なります。 ビデオ録画を開始または停止した場合は、直後にこの値をチェックして、目的のモードが現在のビデオ録画状態でサポートされていることを確認する必要があります。


> [!NOTE] 
> Windows 10、バージョン 1709 以降では、**AdvancedPhotoCapture** を HDR モードに設定すると、[**FlashControl.Enabled**](/uwp/api/windows.media.devices.flashcontrol.enabled) プロパティの設定が無視されるため、フラッシュが作動しません。 また他のキャプチャ モードでは、**FlashControl.Enabled** の場合、**AdvancedPhotoCapture** 設定が上書きされ、通常の写真がフラッシュを使ってキャプチャされます。 [**Auto**](/uwp/api/windows.media.devices.flashcontrol.auto) が true に設定されている場合、**AdvancedPhotoCapture** がフラッシュを使用できるかどうかは、現在のシーン条件に対してカメラのドライバーに設定された既定の動作に依存します。 以前のリリースでは、**AdvancedPhotoCapture** のフラッシュ設定が、常に **FlashControl.Enabled** の設定を上書きします。

> [!NOTE] 
> この記事の内容は、写真やビデオの基本的なキャプチャ機能を実装するための手順を紹介した「[MediaCapture を使った基本的な写真、ビデオ、およびオーディオのキャプチャ](basic-photo-video-and-audio-capture-with-MediaCapture.md)」で取り上げた概念やコードに基づいています。 そちらの記事で基本的なメディア キャプチャのパターンを把握してから、高度なキャプチャ シナリオに進むことをお勧めします。 この記事で紹介しているコードは、MediaCapture のインスタンスが既に作成され、適切に初期化されていることを前提としています。

**AdvancedPhotoCapture** クラスの使い方を示す、ユニバーサル Windows のサンプルがあります。コンテキスト内で API を使用する方法を確認したり、独自のアプリを作成し始めたりすることができます。 詳しくは、「[カメラの高度なキャプチャのサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraAdvancedCapture)」をご覧ください。

## <a name="advanced-photo-capture-namespaces"></a>高度な写真キャプチャの名前空間

この記事のコード例では、基本的なメディア キャプチャに必要な名前空間に加え、次の名前空間の API を使用します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetHDRPhotoUsing":::

## <a name="hdr-photo-capture"></a>HDR 写真キャプチャ

### <a name="determine-if-hdr-photo-capture-is-supported-on-the-current-device"></a>HDR 写真キャプチャが現在のデバイスでサポートされているかどうかを調べる

この記事で説明されている HDR キャプチャ手法には、[**AdvancedPhotoCapture**](/uwp/api/Windows.Media.Capture.AdvancedPhotoCapture) オブジェクトが使われています。 デバイスによっては、**AdvancedPhotoCapture** での HDR キャプチャがサポートされません。 現在アプリを実行しているデバイスが、この手法をサポートしているかどうかを調べるには、**MediaCapture** オブジェクトの [**VideoDeviceController**](/uwp/api/Windows.Media.Devices.VideoDeviceController) を取得し、その [**AdvancedPhotoControl**](/uwp/api/Windows.Media.Devices.AdvancedPhotoControl) プロパティを取得します。 ビデオ デバイス コントローラーの [**SupportedModes**](/uwp/api/windows.media.devices.advancedphotocontrol.supportedmodes)  コレクションに [**AdvancedPhotoMode.Hdr**](/uwp/api/Windows.Media.Devices.AdvancedPhotoMode) が含まれているかどうかを確認してください。コレクションに含まれている場合、**AdvancedPhotoCapture** を使った HDR キャプチャがサポートされています。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetHdrSupported":::

### <a name="configure-and-prepare-the-advancedphotocapture-object"></a>AdvancedPhotoCapture オブジェクトを構成して準備する

[**AdvancedPhotoCapture**](/uwp/api/Windows.Media.Capture.AdvancedPhotoCapture) インスタンスにはコード内の複数の場所からアクセスする必要があるため、そのオブジェクトを保持するメンバー変数を宣言する必要があります。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetDeclareAdvancedCapture":::

アプリのコードで、**MediaCapture** オブジェクトを初期化した後、[**AdvancedPhotoCaptureSettings**](/uwp/api/Windows.Media.Devices.AdvancedPhotoCaptureSettings) オブジェクトを作成し、そのモードを [**AdvancedPhotoMode.Hdr**](/uwp/api/Windows.Media.Devices.AdvancedPhotoMode) に設定します。作成した **AdvancedPhotoCaptureSettings** オブジェクトを [**AdvancedPhotoControl**](/uwp/api/Windows.Media.Devices.AdvancedPhotoControl) オフジェクトの [**Configure**](/uwp/api/windows.media.devices.advancedphotocontrol.configure) メソッドに渡して呼び出します。

**MediaCapture** オブジェクトの [**PrepareAdvancedPhotoCaptureAsync**](/uwp/api/windows.media.capture.mediacapture.prepareadvancedphotocaptureasync) を呼び出す際に [**ImageEncodingProperties**](/uwp/api/Windows.Media.MediaProperties.ImageEncodingProperties) オブジェクトを渡し、キャプチャで使うエンコードの種類を指定します。 **ImageEncodingProperties**クラスは、 **MediaCapture**でサポートされているイメージエンコーディングを作成するための静的メソッドを提供します。

**PrepareAdvancedPhotoCaptureAsync** からは [**AdvancedPhotoCapture**](/uwp/api/Windows.Media.Capture.AdvancedPhotoCapture) オブジェクトが返されます。写真キャプチャの初期化にはこのオブジェクトを使うことになります。 後ほど説明する [**OptionalReferencePhotoCaptured**](/uwp/api/windows.media.capture.advancedphotocapture.optionalreferencephotocaptured) と [**AllPhotosCaptured**](/uwp/api/windows.media.capture.advancedphotocapture.allphotoscaptured) のハンドラーは、このオブジェクトを使って登録することができます。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetCreateAdvancedCaptureAsync":::

### <a name="capture-an-hdr-photo"></a>HDR 写真をキャプチャする

HDR 写真をキャプチャするには、[**AdvancedPhotoCapture**](/uwp/api/Windows.Media.Capture.AdvancedPhotoCapture) オブジェクトの [**CaptureAsync**](/uwp/api/windows.media.capture.advancedphotocapture.captureasync) メソッドを呼び出します。 このメソッドから返される [**AdvancedCapturedPhoto**](/uwp/api/Windows.Media.Capture.AdvancedCapturedPhoto) オブジェクトの [**Frame**](/uwp/api/windows.media.capture.advancedcapturedphoto.frame) プロパティに、キャプチャされた写真が格納されています。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetCaptureHdrPhotoAsync":::

ほとんどの写真アプリは、他のアプリやデバイスで正しく表示できるように、キャプチャされた写真の回転情報を画像ファイルにエンコードします。 次の例は、**CameraRotationHelper** ヘルパー クラスを使用して、ファイルの正しい向きを計算する方法を示しています。 このクラスの全容については、「[**MediaCapture を使ってデバイスの向きを処理する**](handle-device-orientation-with-mediacapture.md)」で説明しています。

画像をディスクに保存する **SaveCapturedFrameAsync** ヘルパー メソッドについては、この記事で後述しています。

### <a name="get-optional-reference-frame"></a>必要に応じて参照フレームを取得する

HDR プロセスは複数のフレームをキャプチャします。そのすべてのフレームがキャプチャされると、それらが単一の画像として合成されます。 フレームがキャプチャされた後、HDR プロセス全体が完了する前に、[**OptionalReferencePhotoCaptured**](/uwp/api/windows.media.capture.advancedphotocapture.optionalreferencephotocaptured) イベントを処理することでそのフレームにアクセスすることができます。 HDR 写真の最終的な結果だけが目的であれば、この処理は不要です。

> [!IMPORTANT]
> ハードウェア HDR をサポートしていて参照フレームを生成しないデバイスでは、[**OptionalReferencePhotoCaptured**](/uwp/api/windows.media.capture.advancedphotocapture.optionalreferencephotocaptured) が発生しません。 アプリ側で、このイベントが生成されないケースに対処する必要があります。

参照フレームは **CaptureAsync** 呼び出しのコンテキストから離れて届くため、**OptionalReferencePhotoCaptured** ハンドラーにコンテキスト情報を渡すためのしくみが用意されています。 まず、コンテキスト情報を保持するオブジェクトを呼び出す必要があります。 このオブジェクトの名前と内容は自由に設定してください。 この例のオブジェクトには、キャプチャのファイル名とカメラの向きを追跡するためのメンバーが定義されています。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetAdvancedCaptureContext":::

コンテキスト オブジェクトの新しいインスタンスを作成して、そのメンバーにデータを設定した後、パラメーターとしてオブジェクトを受け取る [**CaptureAsync**](/uwp/api/windows.media.capture.advancedphotocapture.captureasync) のオーバーロードにそのオブジェクトを渡します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetCaptureWithContext":::

[**OptionalReferencePhotoCaptured**](/uwp/api/windows.media.capture.advancedphotocapture.optionalreferencephotocaptured) イベント ハンドラーで、[**OptionalReferencePhotoCapturedEventArgs**](/uwp/api/Windows.Media.Capture.OptionalReferencePhotoCapturedEventArgs) オブジェクトの [**Context**](/uwp/api/windows.media.capture.optionalreferencephotocapturedeventargs.context) プロパティを、先ほど定義したコンテキスト オブジェクト クラスにキャストします。 この例では、最終的な HDR 画像と区別するために参照フレーム画像のファイル名を変更した上で **SaveCapturedFrameAsync** ヘルパー メソッドを呼び出し、画像を保存しています。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetOptionalReferencePhotoCaptured":::

### <a name="receive-a-notification-when-all-frames-have-been-captured"></a>すべてのフレームがキャプチャされたときに通知を受け取る

HDR 写真のキャプチャには、2 つのステップがあります。 複数のフレームをキャプチャするステップと、その後、それらのフレームが最終的な HDR 画像に加工するステップです。 ソース HDR フレームのキャプチャ中に別のキャプチャを開始することはできませんが、すべてのフレームがキャプチャされた後であれば、HDR の後処理が完了していなくても、キャプチャを開始することができます。 HDR キャプチャが完了すると [**AllPhotosCaptured**](/uwp/api/windows.media.capture.advancedphotocapture.allphotoscaptured) イベントが発生するので、そのタイミングで別のキャプチャを開始することができます。 たとえば HDR キャプチャの開始時に UI のキャプチャ ボタンを無効にし、その後 **AllPhotosCaptured** が発生した時点で再度ボタンを有効にする、という使い方が考えられます。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetAllPhotosCaptured":::

### <a name="clean-up-the-advancedphotocapture-object"></a>AdvancedPhotoCapture オブジェクトのクリーンアップ

キャプチャが終了したら、**MediaCapture** オブジェクトを破棄する前に、[**FinishAsync**](/uwp/api/windows.media.capture.advancedphotocapture.finishasync) を呼び出し、メンバー変数を null に設定して [**AdvancedPhotoCapture**](/uwp/api/Windows.Media.Capture.AdvancedPhotoCapture) オブジェクトをシャットダウンする必要があります。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetCleanUpAdvancedPhotoCapture":::


## <a name="low-light-photo-capture"></a>ローライトの写真のキャプチャ
Windows 10 バージョン 1607 以降では、**AdvancedPhotoCapture** により、ローライトの設定でキャプチャされた写真の品質を高める組み込みアルゴリズムを使って、写真をキャプチャすることが可能です。 [**AdvancedPhotoCapture**](/uwp/api/Windows.Media.Capture.AdvancedPhotoCapture) クラスの低光量機能を使うと、システムは現在のシーンを評価し、必要に応じて、低光量の状況に合わせてアルゴリズムを適用します。 システムでアルゴリズムが必要ないと判断された場合は、通常のキャプチャが実行されます。

低光量写真のキャプチャを使用する前に、現在アプリを実行しているデバイスがこの手法をサポートしているかどうかを調べます。このためには、**MediaCapture** オブジェクトの [**VideoDeviceController**](/uwp/api/Windows.Media.Devices.VideoDeviceController) を取得し、その [**AdvancedPhotoControl**](/uwp/api/Windows.Media.Devices.AdvancedPhotoControl) プロパティを取得します。 ビデオ デバイス コントローラーの [**SupportedModes**](/uwp/api/windows.media.devices.advancedphotocontrol.supportedmodes) コレクションに [**AdvancedPhotoMode.LowLight**](/uwp/api/Windows.Media.Devices.AdvancedPhotoMode) が含まれているかどうかを確認してください。 コレクションに含まれている場合、**AdvancedPhotoCapture** を使ったローライトのキャプチャがサポートされています。 
:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetLowLightSupported1":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetLowLightSupported2":::

次に、**AdvancedPhotoCapture** オブジェクトを格納するためのメンバー変数を宣言します。 

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetDeclareAdvancedCapture":::

アプリのコードで、**MediaCapture** オブジェクトを初期化した後、[**AdvancedPhotoCaptureSettings**](/uwp/api/Windows.Media.Devices.AdvancedPhotoCaptureSettings) オブジェクトを作成し、そのモードを [**AdvancedPhotoMode.LowLight**](/uwp/api/Windows.Media.Devices.AdvancedPhotoMode) に設定します。 作成した **AdvancedPhotoCaptureSettings** オブジェクトを [**AdvancedPhotoControl**](/uwp/api/Windows.Media.Devices.AdvancedPhotoControl) オブジェクトの [**Configure**](/uwp/api/windows.media.devices.advancedphotocontrol.configure) メソッドに渡して呼び出します。

**MediaCapture** オブジェクトの [**PrepareAdvancedPhotoCaptureAsync**](/uwp/api/windows.media.capture.mediacapture.prepareadvancedphotocaptureasync) を呼び出す際に [**ImageEncodingProperties**](/uwp/api/Windows.Media.MediaProperties.ImageEncodingProperties) オブジェクトを渡し、キャプチャで使うエンコードの種類を指定します。 

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetCreateAdvancedCaptureLowLightAsync":::

写真をキャプチャするには、[**CaptureAsync**](/uwp/api/windows.media.capture.advancedphotocapture.captureasync) を呼び出します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetCaptureLowLight":::

前述の HDR の例のように、この例は **CameraRotationHelper** というヘルパー クラスを使って、画像にエンコードする必要がある回転値を特定し、他のアプリやデバイスで正しく表示できるようにします。 このクラスの全容については、「[**MediaCapture を使ってデバイスの向きを処理する**](handle-device-orientation-with-mediacapture.md)」で説明しています。

画像をディスクに保存する **SaveCapturedFrameAsync** ヘルパー メソッドについては、この記事で後述しています。

**AdvancedPhotoCapture** オブジェクトを再構成しなくても低光量の写真を複数キャプチャすることができますが、キャプチャが終わったら [**FinishAsync**](/uwp/api/windows.media.capture.advancedphotocapture.finishasync) を呼び出し、オブジェクトと、関連付けられているリソースをクリーンアップする必要があります。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetCleanUpAdvancedPhotoCapture":::

## <a name="working-with-advancedcapturedphoto-objects"></a>AdvancedCapturedPhoto オブジェクトを操作する
[**AdvancedPhotoCapture.CaptureAsync**](/uwp/api/windows.media.capture.advancedphotocapture.captureasync) は、キャプチャした写真を表す [**AdvancedCapturedPhoto**](/uwp/api/Windows.Media.Capture.AdvancedCapturedPhoto) オブジェクトを返します。 このオブジェクトが公開するのは、画像を表す [**CapturedFrame**](/uwp/api/Windows.Media.Capture.CapturedFrame) オブジェクトを返す、[**Frame**](/uwp/api/windows.media.capture.advancedcapturedphoto.frame) プロパティです。 [**OptionalReferencePhotoCaptured**](/uwp/api/windows.media.capture.advancedphotocapture.optionalreferencephotocaptured) イベントも、イベント引数で **CapturedFrame** オブジェクトを提供します。 この型のオブジェクトを取得した後は、[**SoftwareBitmap**](/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) の作成や、ファイルへの画像の保存など、多くのことを実行できるようになります。 

## <a name="get-a-softwarebitmap-from-a-capturedframe"></a>CapturedFrame から SoftwareBitmap を取得する
**SoftwareBitmap** を **CapturedFrame** オブジェクトから取得するのは、オブジェクトの [**SoftwareBitmap**](/uwp/api/windows.media.capture.capturedframe.softwarebitmap) プロパティにアクセスするだけなので、簡単です。 ただし、**AdvancedPhotoCapture** での **SoftwareBitmap** の使用は、ほとんどのエンコード形式においてサポートされていないため、使用する前にプロパティが null になっていないことを確認する必要があります。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetSoftwareBitmapFromCapturedFrame":::

現在のリリースで、**AdvancedPhotoCapture** での **SoftwareBitmap** の使用をサポートしている唯一のエンコード形式は、非圧縮形式の NV12 です。 したがってこの機能を使用する場合は、[**PrepareAdvancedPhotoCaptureAsync**](/uwp/api/windows.media.capture.mediacapture.prepareadvancedphotocaptureasync) を呼び出す際にそのエンコーディングを指定する必要があります。 

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetUncompressedNv12":::

もちろん、ファイルに画像を保存し、別個の手順で **SoftwareBitmap** にファイルを読み込むことも常に可能です。 **SoftwareBitmap** の操作について詳しくは、「[**ビットマップ画像の作成、編集、保存**](imaging.md)」をご覧ください。

## <a name="save-a-capturedframe-to-a-file"></a>CapturedFrame をファイルに保存する
[**CapturedFrame**](/uwp/api/Windows.Media.Capture.CapturedFrame) クラスは IInputStream インターフェイスを実装するので、[**BitmapDecoder**](/uwp/api/Windows.Graphics.Imaging.BitmapDecoder) への入力として使用できます。その後、[**BitmapEncoder**](/uwp/api/Windows.Graphics.Imaging.BitmapEncoder) を使えば画像データをディスクに書き込むことができます。

次の例では、ユーザーの画像ライブラリに新しいフォルダーが作成され、そのフォルダー内にファイルが作成されています。 このディレクトリにアクセスするためには、アプリが **Pictures Library** 機能をアプリ マニフェスト ファイルに含める必要があります。 すると、ファイル ストリームが指定のファイルに開かれます。 次に、**CapturedFrame** からデコーダーを作成するために [**BitmapDecoder.CreateAsync**](/uwp/api/windows.graphics.imaging.bitmapdecoder.createasync) を呼び出します。 その後 [**CreateForTranscodingAsync**](/uwp/api/windows.graphics.imaging.bitmapencoder.createfortranscodingasync) がファイル ストリームとデコーダーからエンコーダーを作成します。

次に、エンコーダーの [**BitmapProperties**](/uwp/api/windows.graphics.imaging.bitmapencoder.bitmapproperties) を使って画像ファイルに写真の向きをエンコードします。 画像をキャプチャする際の向きの処理について詳しくは、「[**MediaCapture を使ってデバイスの向きを処理する**](handle-device-orientation-with-mediacapture.md)」をご覧ください。

最後に、[**FlushAsync**](/uwp/api/windows.graphics.imaging.bitmapencoder.flushasync) を呼び出すことで、画像をファイルに書き込みます。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetSaveCapturedFrameAsync":::

## <a name="related-topics"></a>関連トピック

* [カメラ](camera.md)
* [MediaCapture を使った基本的な写真、ビデオ、およびオーディオのキャプチャ](basic-photo-video-and-audio-capture-with-MediaCapture.md)
