---
title: ビデオに画面の取り込み
description: この記事では、画面からキャプチャされたフレームをビデオファイルにエンコードする方法について説明します。
ms.date: 07/28/2020
ms.topic: article
dev_langs:
- csharp
keywords: windows 10、uwp、画面キャプチャ、ビデオ
ms.localizationpriority: medium
ms.openlocfilehash: d8f70748d025d50d19dbf2cb184ae841cced7f8a
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/24/2020
ms.locfileid: "91218635"
---
# <a name="screen-capture-to-video"></a>ビデオに画面の取り込み

この記事では、画面からキャプチャされたフレームをビデオファイルにエンコードする方法について説明します。 静止画像のキャプチャの詳細については、「 [Screeen capture](./screen-capture.md)」を参照してください。

## <a name="overview-of-the-video-capture-process"></a>ビデオキャプチャプロセスの概要
この記事では、ビデオファイルにウィンドウの内容を記録するサンプルアプリのチュートリアルを提供します。 このシナリオを実装するために多くのコードが必要と思われるかもしれませんが、画面レコーダーアプリの高レベルの構造は非常に単純です。 画面キャプチャプロセスでは、次の3つの主要な UWP 機能を使用します。

- [GraphicsCapture](/uwp/api/windows.graphics.capture) api は、画面からピクセルを実際に取得する処理を実行します。 [GraphicsCaptureItem](/uwp/api/windows.graphics.capture.graphicscaptureitem)クラスは、キャプチャされるウィンドウまたは表示を表します。 [GraphicsCaptureSession](/uwp/api/windows.graphics.capture.graphicscapturesession) は、キャプチャ操作を開始および停止するために使用されます。 [Direct3D11CaptureFramePool](/uwp/api/windows.graphics.capture.direct3d11captureframepool)クラスは、画面の内容がコピーされるフレームのバッファーを保持します。
- [Mediastreamsource](/uwp/api/windows.media.core.mediastreamsource)クラスはキャプチャされたフレームを受け取り、ビデオストリームを生成します。
- [MediaTranscoder](/uwp/api/windows.media.transcoding.mediatranscoder)クラスは、 **mediastreamsource**によって生成されたストリームを受け取り、それをビデオファイルにエンコードします。

この記事に示されているコード例は、いくつかの異なるタスクに分類できます。

- **初期化** -これには、上記で説明した UWP クラスの構成、グラフィックスデバイスインターフェイスの初期化、キャプチャするウィンドウの選択、および解像度やフレームレートなどのエンコーディングパラメーターの設定が含まれます。
- **イベントハンドラーとスレッド処理**-メインキャプチャループの主要なドライバーは、 [SampleRequested](/uwp/api/windows.media.core.mediastreamsource.samplerequested)イベントを使用してフレームを定期的に要求する**mediastreamsource**です。 この例では、イベントを使用して、この例の異なるコンポーネント間の新しいフレームに対する要求を調整します。 同期は、フレームのキャプチャとエンコードを同時に行うことができるようにするために重要です。
- **フレームをコピー** すると、キャプチャフレームバッファーから別の Direct3D サーフェイスにコピーされます。これは **mediastreamsource** に渡すことができるため、エンコード中にリソースが上書きされることはありません。 このコピー操作を短時間で実行するには、Direct3D Api を使用します。 

### <a name="about-the-direct3d-apis"></a>Direct3D Api について
前述のように、キャプチャされた各フレームのコピーは、この記事で示されている実装の中で最も複雑な部分です。 低いレベルでは、この操作は Direct3D を使用して行われます。 この例では、 [SharpDX](http://sharpdx.org/) ライブラリを使用して、C# から Direct3D 操作を実行します。 このライブラリは正式にはサポートされていませんが、このシナリオでは低レベルのコピー操作のパフォーマンスに優れているため、選択されました。 これらのタスクのために独自のコードまたは他のライブラリを簡単に置き換えることができるように、Direct3D 操作をできるだけ不連続化しました。

## <a name="setting-up-your-project"></a>プロジェクトの設定
このチュートリアルのコード例は、Visual Studio 2019 の **空のアプリ (ユニバーサル Windows)** C# プロジェクトテンプレートを使用して作成されました。 アプリで **Package.appxmanifest api を** 使用するには、プロジェクトのパッケージのファイルに **グラフィックスキャプチャ** 機能を含める必要があります。 この例では、生成されたビデオファイルをデバイスのビデオライブラリに保存します。 このフォルダーにアクセスするには、[ **ビデオライブラリ** ] 機能を含める必要があります。

SharpDX Nuget パッケージをインストールするには、Visual Studio で [ **Nuget パッケージの管理**] を選択します。 [参照] タブで "SharpDX" パッケージを検索し、[ **インストール**] をクリックします。

この記事のコードリストのサイズを小さくするために、次のチュートリアルのコードでは、明示的な名前空間参照と、先頭にアンダースコア "_" を付けて名前が付けられた Mainpage.xaml クラスメンバー変数の宣言を省略しています。

## <a name="setup-for-encoding"></a>エンコード用のセットアップ

このセクションで説明されている **Setupencoding** メソッドは、ビデオフレームをキャプチャしてエンコードし、キャプチャしたビデオのエンコードパラメーターを設定するために使用される主要なオブジェクトの一部を初期化します。 このメソッドは、プログラムによって、またはボタンのクリックのようなユーザーの操作に応じて呼び出すことができます。 **Setupencoding**のコードリストを、初期化手順の説明の後に示します。

- **キャプチャのサポートを確認します。** キャプチャプロセスを開始する前に、GraphicsCaptureSession を呼び出して、現在のデバイスで画面キャプチャ機能がサポートされていることを確認する必要があり[ます。](/uwp/api/windows.graphics.capture.graphicscapturesession.issupported)

- **Direct3D インターフェイスを初期化します。** このサンプルでは、Direct3D を使用して、画面からキャプチャされたピクセルをビデオフレームとしてエンコードされたテクスチャにコピーします。 この記事では、Direct3D インターフェイスの初期化に使用されるヘルパーメソッドである**CreateD3DDevice と****を次**に示します。

- **GraphicsCaptureItem を初期化します。** [GraphicsCaptureItem](/uwp/api/windows.graphics.capture.graphicscaptureitem)は、ウィンドウまたは画面全体のいずれかでキャプチャされる画面上の項目を表します。 [GraphicsCapturePicker](/uwp/api/windows.graphics.capture.graphicscapturepicker)を作成し、Pick [singleitemasync](/uwp/api/windows.graphics.capture.graphicscapturepicker.picksingleitemasync)を呼び出して、キャプチャする項目をユーザーが選択できるようにします。

- **コンポジションテクスチャを作成します。** テクスチャリソースと、関連付けられているレンダーターゲットビューを作成します。このビューは、各ビデオフレームのコピーに使用されます。 **GraphicsCaptureItem**が作成され、そのディメンションがわかっているまで、このテクスチャを作成することはできません。 このコンポジションテクスチャの使用方法については、 **Waitfornewframe** の説明を参照してください。 このテクスチャを作成するためのヘルパーメソッドについては、この記事の後半でも説明します。

- **MediaEncodingProfile と VideoStreamDescriptor を作成します。** [Mediastreamsource](/uwp/api/windows.media.core.mediastreamsource)クラスのインスタンスは、画面からキャプチャされたイメージを取得し、ビデオストリームにエンコードします。 次に、 [MediaTranscoder](/uwp/api/windows.media.transcoding.mediatranscoder) クラスによってビデオストリームがビデオファイルにトランスコードされます。 [VideoStreamDecriptor](/uwp/api/windows.media.core.videostreamdescriptor)は、 **mediastreamsource**の解像度やフレームレートなどのエンコーディングパラメーターを提供します。 **MediaTranscoder**のビデオファイルエンコーディングパラメーターは、 [MediaEncodingProfile](/uwp/api/Windows.Media.MediaProperties.MediaEncodingProfile)で指定されます。 ビデオエンコードに使用されるサイズは、キャプチャされるウィンドウのサイズと同じである必要はありませんが、この例を単純なままにするために、エンコーディングの設定は、キャプチャ項目の実際の大きさを使用するようにハードコーディングされています。

- **MediaStreamSource オブジェクトと MediaTranscoder オブジェクトを作成します。** 前述のように、 **Mediastreamsource** オブジェクトは、個々のフレームをビデオストリームにエンコードします。 前の手順で作成した **MediaEncodingProfile** を渡して、このクラスのコンストラクターを呼び出します。 バッファー時間をゼロに設定し、 [開始](/uwp/api/windows.media.core.mediastreamsource.starting) イベントと [SampleRequested](/uwp/api/windows.media.core.mediastreamsource.samplerequested) イベントのハンドラーを登録します。このイベントは、この記事の後半で説明します。 次に、 **MediaTranscoder** クラスの新しいインスタンスを構築し、ハードウェアアクセラレータを有効にします。

- **出力ファイルを作成** するこのメソッドの最後の手順は、ビデオをトランスコードするファイルを作成することです。 この例では、デバイスの [ビデオライブラリ] フォルダーに一意の名前が付けられたファイルを作成します。 このフォルダーにアクセスするには、アプリでアプリマニフェストに "ビデオライブラリ" 機能を指定する必要があることに注意してください。 ファイルが作成されたら、読み取りと書き込みのためにファイルを開き、結果として得られるストリームを **EncodeAsync** メソッドに渡します。このメソッドは次のようになります。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/MainPage.xaml.cs" id="snippet_SetupEncoding":::

## <a name="start-encoding"></a>エンコードの開始
メインオブジェクトが初期化されたので、 **EncodeAsync** メソッドを実装してキャプチャ操作を開始します。 このメソッドは、まず、まだ記録されていないことを確認します。存在しない場合は、ヘルパーメソッド **Startcapture** を呼び出して、画面からフレームのキャプチャを開始します。 このメソッドについては、この記事の後半で説明します。 次に、 [PrepareMediaStreamSourceTranscodeAsync](/uwp/api/windows.media.transcoding.mediatranscoder.preparemediastreamsourcetranscodeasync)は、前のセクションで作成したエンコードプロファイルを使用して、 **mediastreamsource**オブジェクトによって生成されたビデオストリームを出力ファイルストリームに変換できる**MediaTranscoder**を取得するために呼び出されます。 Transcodeを準備したら、 [Transcodeasync](/uwp/api/windows.media.transcoding.preparetranscoderesult.transcodeasync) を呼び出して、トランスコードを開始します。 **MediaTranscoder**の使用方法の詳細については、「[メディアファイルのトランスコード](./transcode-media-files.md)」を参照してください。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/MainPage.xaml.cs" id="snippet_EncodeAsync":::

## <a name="handle-mediastreamsource-events"></a>MediaStreamSource イベントの処理
**Mediastreamsource**オブジェクトは、画面からキャプチャしたフレームを取得し、 **MediaTranscoder**を使用してファイルに保存できるビデオストリームに変換します。 オブジェクトのイベントのハンドラーを介して、フレームを **Mediastreamsource** に渡します。

[SampleRequested](/uwp/api/windows.media.core.mediastreamsource.samplerequested)イベントは、 **mediastreamsource**が新しいビデオフレームの準備が整ったときに発生します。 現在記録していることを確認したら、ヘルパーメソッド **Waitfornewframe** を呼び出して、画面からキャプチャされた新しいフレームを取得します。 このメソッドは、この記事の後半で示すように、キャプチャされたフレームを含む [ID3D11Surface](/uwp/api/Windows.Graphics.DirectX.Direct3D11.IDirect3DSurface) オブジェクトを返します。 この例では、フレームがキャプチャされたシステム時刻も格納するヘルパークラスに **IDirect3DSurface** インターフェイスをラップします。 フレームとシステム時刻の両方が[mediastreamsample. CreateFromDirect3D11Surface](/uwp/api/windows.media.core.mediastreamsample.createfromdirect3d11surface)ファクトリメソッドに渡され、結果の[Mediastreamsample](/uwp/api/windows.media.core.mediastreamsample)は[MediaStreamSourceSampleRequestedEventArgs](/uwp/api/windows.media.core.mediastreamsourcesamplerequestedeventargs)の[MediaStreamSourceSampleRequest](/uwp/api/windows.media.core.mediastreamsourcesamplerequest.sample)プロパティに設定されます。 これは、キャプチャしたフレームが **Mediastreamsource**に提供される方法です。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/MainPage.xaml.cs" id="snippet_OnMediaStreamSourceSampleRequested":::

[開始](/uwp/api/windows.media.core.mediastreamsource.starting)イベントのハンドラーでは、 **waitfornewframe**を呼び出しますが、フレームがキャプチャされた[MediaStreamSourceStartingRequest](/uwp/api/windows.media.core.mediastreamsourcestartingrequest.setactualstartposition)メソッドにはフレームがキャプチャされたシステム時刻のみが渡されます。これは、 **mediastreamsource**がそれ以降のフレームのタイミングを適切にエンコードするために使用します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/MainPage.xaml.cs" id="snippet_OnMediaStreamSourceStarting":::

## <a name="start-capturing"></a>キャプチャの開始
この手順で示されている **Startcapture** メソッドは、前の手順で示した **EncodeAsync** helper メソッドから呼び出されます。 まず、このメソッドは、キャプチャ操作のフローを制御するために使用される一連のイベントオブジェクトを初期化します。

- **_multithread** は、コピー中に他のスレッドが SharpDX テクスチャにアクセスしないようにするために使用される、SharpDX ライブラリの **マルチスレッド** オブジェクトをラップするヘルパークラスです。
- **_frameEvent**は、新しいフレームがキャプチャされ、 **mediastreamsource**に渡すことができることを通知するために使用されます。
- **_closedEvent** は、記録が停止したこと、および新しいフレームを待機する必要がないことを通知します。

フレームイベントと closed イベントが配列に追加されるため、キャプチャループ内のいずれかのイベントを待機できます。

**Startcapture**メソッドの残りの部分では、実際の画面キャプチャを実行するための Windows の取り込む api を設定します。 まず、 **CaptureItem** イベントにイベントが登録されます。 次に、 [Direct3D11CaptureFramePool](/uwp/api/windows.graphics.capture.direct3d11captureframepool) が作成されます。これにより、一度に複数のキャプチャされたフレームをバッファリングできます。 [Createfreethreaded](/uwp/api/windows.graphics.capture.direct3d11captureframepool.createfreethreaded)メソッドを使用してフレームプールを作成し、フレームに[到着](/uwp/api/windows.graphics.capture.direct3d11captureframepool.framearrived)したイベントがアプリのメインスレッドではなく、プールの独自のワーカースレッドで呼び出されるようにします。 次に、 **フレームに到着** したイベントに対してハンドラーが登録されます。 最後に、選択した**CaptureItem**に対して[GraphicsCaptureSession](/uwp/api/windows.graphics.capture.graphicscapturesession)が作成され、 [startcapture](/uwp/api/windows.graphics.capture.graphicscapturesession)を呼び出すことによってフレームのキャプチャが開始されます。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/MainPage.xaml.cs" id="snippet_StartCapture":::

## <a name="handle-graphics-capture-events"></a>グラフィックスキャプチャイベントの処理
前の手順では、グラフィックスキャプチャイベント用に2つのハンドラーを登録し、キャプチャループのフローを管理するためにいくつかのイベントを設定しました。

フレーム **到着** イベントは、 **Direct3D11CaptureFramePool** に使用可能な新しいキャプチャフレームがある場合に発生します。 このイベントのハンドラーで、送信側の [TryGetNextFrame](/uwp/api/windows.graphics.capture.direct3d11captureframepool.trygetnextframe) を呼び出して、次にキャプチャされたフレームを取得します。 フレームを取得した後、 **_frameEvent** を設定して、使用可能な新しいフレームがキャプチャループに認識されるようにします。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/MainPage.xaml.cs" id="snippet_OnFrameArrived":::

**Closed**イベントハンドラーでは、停止するタイミングをキャプチャループが認識できるように、 **_closedEvent**を通知します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/MainPage.xaml.cs" id="snippet_OnClosed":::

## <a name="wait-for-new-frames"></a>新しいフレームを待機する
このセクションで説明する **Waitfornewframe** のヘルパーメソッドは、キャプチャループが発生する場所です。 このメソッドは、 **Mediastreamsource**がビデオストリームに新しいフレームを追加する準備が整うと常に、 **OnMediaStreamSourceSampleRequested**イベントハンドラーから呼び出されます。 大まかに言えば、この関数は、画面にキャプチャされた各ビデオフレームを1つの Direct3D サーフェイスから別の Direct3D サーフェイスにコピーするだけで、新しいフレームがキャプチャされている間に、エンコードのために **Mediastreamsource** に渡すことができます。 この例では、SharpDX ライブラリを使用して、実際のコピー操作を実行します。

新しいフレームを待機する前に、メソッドは、クラス変数に格納されている前のフレームを破棄し、 **_currentFrame**、 **_frameEvent**をリセットします。 次に、メソッドは、 **_frameEvent** または **_closedEvent** がシグナル状態になるのを待機します。 Closed イベントが設定されている場合、アプリは、キャプチャリソースをクリーンアップするヘルパーメソッドを呼び出します。 このメソッドについては、この記事の後半で説明します。

フレームイベントが設定されている場合は、前の手順で定義されているフレームに **到着** したイベントハンドラーが呼び出されたことがわかります。キャプチャされたフレームデータを、 **mediastreamsource**に渡される Direct3D 11 サーフェイスにコピーする処理が開始されます。

この例では、ヘルパークラス **SurfaceWithInfo**を使用しています。これにより、ビデオフレームとフレームのシステム時刻の両方を、1つのオブジェクトとして、 **mediastreamsource** によって要求されたものとして渡すことができます。 フレームのコピー処理の最初の手順は、このクラスをインスタンス化し、システム時刻を設定することです。

次の手順は、特に SharpDX ライブラリに依存するこの例の一部です。 ここで使用されているヘルパー関数は、この記事の最後に定義されています。 まず、マルチスレッド **ロック** を使用して、コピーの作成中に他のスレッドがビデオフレームバッファーにアクセスしないようにします。 次に、ヘルパーメソッド **CreateSharpDXTexture2D** を呼び出して、ビデオフレームから SharpDX **Texture2D** オブジェクトを作成します。 これはコピー操作のソーステクスチャになります。 

次に、前の手順で作成した **Texture2D** オブジェクトから、プロセスで前に作成したコンポジションテクスチャにコピーします。 このコンポジションテクスチャはスワップバッファーとして機能します。これにより、エンコードプロセスは、次のフレームがキャプチャされている間にピクセルで動作できるようになります。 コピーを実行するには、コンポジションテクスチャに関連付けられているレンダーターゲットビューをクリアし、コピーするテクスチャ内の領域 (この場合はテクスチャ全体) を定義します。次に、 **CopySubresourceRegion** を呼び出して、ピクセルをコンポジションテクスチャに実際にコピーします。

ターゲットテクスチャを作成するときに使用するテクスチャの説明のコピーを作成しますが、説明は変更され、新しいテクスチャに書き込みアクセスが許可されるように **Bindflags** を **作り** に設定します。 Cpu の **Accessflags** を **None** に設定すると、システムはコピー操作を最適化できます。 テクスチャの説明は、新しいテクスチャリソースを作成するために使用されます。この新しいリソースには、 **copyresource**を呼び出すことによって、コンポジションテクスチャリソースがコピーされます。 最後に、 **CreateDirect3DSurfaceFromSharpDXTexture** を呼び出して、このメソッドから返される **IDirect3DSurface** オブジェクトを作成します。


:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/MainPage.xaml.cs" id="snippet_WaitForNewFrame":::

## <a name="stop-capture-and-clean-up-resources"></a>キャプチャの停止とリソースのクリーンアップ
**Stop**メソッドは、キャプチャ操作を停止する方法を提供します。 アプリは、プログラムによって、またはボタンのクリックなどのユーザーの操作に応じて、これを呼び出すことができます。 このメソッドは、単に **_closedEvent**を設定します。 前の手順で定義した **Waitfornewframe** メソッドは、このイベントを検索します。設定されている場合は、キャプチャ操作をシャットダウンします。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/MainPage.xaml.cs" id="snippet_Stop":::

**クリーンアップ**メソッドは、コピー操作中に作成されたリソースを正しく破棄するために使用されます。 これには次のものが含まれます

- キャプチャセッションによって使用される **Direct3D11CaptureFramePool** オブジェクト
- **GraphicsCaptureSession**と**GraphicsCaptureItem**
- Direct3D および SharpDX デバイス
- コピー操作で使用される SharpDX テクスチャおよびレンダーターゲットビュー。
- 現在のフレームを格納するために使用される **Direct3D11CaptureFrame** 。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/MainPage.xaml.cs" id="snippet_Cleanup":::

## <a name="helper-wrapper-classes"></a>ヘルパーラッパークラス
次のヘルパークラスは、この記事のコード例を参照するために定義されています。

**Multithreadlock**ヘルパークラスは、コピー中に他のスレッドがテクスチャリソースにアクセスしないようにするための、SharpDX**マルチスレッド**クラスをラップします。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/MainPage.xaml.cs" id="snippet_MultithreadLock":::

**SurfaceWithInfo**は、キャプチャされたフレームとキャプチャされた時間を表す**SystemRelativeTime**に**IDirect3DSurface**を関連付けるために使用されます。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/MainPage.xaml.cs" id="snippet_SurfaceWithInfo":::

## <a name="direct3d-and-sharpdx-helper-apis"></a>Direct3D と SharpDX helper Api
次のヘルパー Api は、Direct3D と SharpDX リソースの作成を抽象化するために定義されています。 これらのテクノロジの詳細については、この記事では説明しませんが、このチュートリアルで示したコード例を実装できるように、ここでコードを提供します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/Direct3D11Helpers.cs" id="snippet_Direct3D11Helpers":::

## <a name="see-also"></a>関連項目

* [Windows.Graphics.Capture 名前空間](/uwp/api/windows.graphics.capture)
* [画面の取り込み](screen-capture.md)