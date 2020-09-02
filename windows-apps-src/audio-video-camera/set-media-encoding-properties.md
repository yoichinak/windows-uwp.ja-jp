---
ms.assetid: 09BA9250-A476-4803-910E-52F0A51704B1
description: この記事では、IMediaEncodingProperties インターフェイスを使用して、カメラのプレビュー ストリームとキャプチャした写真/ビデオの解像度およびフレーム レートを設定する方法を説明します。
title: MediaCapture の形式、解像度、およびフレーム レートの設定
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 2b847b7162de19b81c83be2f3769042a5acc8a3a
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363795"
---
# <a name="set-format-resolution-and-frame-rate-for-mediacapture"></a>MediaCapture の形式、解像度、およびフレーム レートの設定



この記事では、 [**IMediaEncodingProperties**](/uwp/api/Windows.Media.MediaProperties.IMediaEncodingProperties) インターフェイスを使用して、カメラのプレビューストリームとキャプチャされた写真やビデオの解像度とフレームレートを設定する方法について説明します。 プレビュー ストリームの縦横比をキャプチャしたメディアの縦横比と一致させる方法についても説明します。

カメラ プロファイルは、カメラのストリーム プロパティを検出および設定するための高度な方法ですが、すべてのデバイスではサポートされていません。 詳しくは、「[カメラ プロファイル](camera-profiles.md)」をご覧ください。

この記事のコードは、[CameraResolution サンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraResolution)を基にしています。 このサンプルをダウンロードし、該当するコンテキストで使用されているコードを確認することも、サンプルを独自のアプリの開始点として使用することもできます。

> [!NOTE] 
> この記事の内容は、写真やビデオの基本的なキャプチャ機能を実装するための手順を紹介した「[MediaCapture を使った基本的な写真、ビデオ、およびオーディオのキャプチャ](basic-photo-video-and-audio-capture-with-MediaCapture.md)」で取り上げた概念やコードに基づいています。 そちらの記事で基本的なメディア キャプチャのパターンを把握してから、高度なキャプチャ シナリオに進むことをお勧めします。 この記事で紹介しているコードは、MediaCapture のインスタンスが既に作成され、適切に初期化されていることを前提としています。

## <a name="a-media-encoding-properties-helper-class"></a>メディア エンコード プロパティのヘルパー クラス

[**IMediaEncodingProperties**](/uwp/api/Windows.Media.MediaProperties.IMediaEncodingProperties) インターフェイスの機能をラップする単純なヘルパー クラスを作成すると、特定の条件を満たす一連のエンコード プロパティを容易に選択できます。 特に、エンコード プロパティの機能の次のような動作に対して、このヘルパー クラスが便利です。

**警告**   [**GetAvailableMediaStreamProperties**](/uwp/api/windows.media.devices.videodevicecontroller.getavailablemediastreamproperties)メソッドは、 **VideoRecord**や**Photo**などの[**mediastreamtype**](/uwp/api/Windows.Media.Capture.MediaStreamType)列挙体のメンバーを取得し、キャプチャした写真やビデオの解像度など、ストリームエンコード設定を伝える[**ImageEncodingProperties**](/uwp/api/Windows.Media.MediaProperties.ImageEncodingProperties)または[**VideoEncodingProperties**](/uwp/api/Windows.Media.MediaProperties.VideoEncodingProperties)オブジェクトの一覧を返します。 指定された **MediaStreamType** 値に関係なく、**GetAvailableMediaStreamProperties** を呼び出した結果には、**ImageEncodingProperties** または **VideoEncodingProperties** が含まれている可能性があります。 このため、いずれかのプロパティ値にアクセスする前に、常に各戻り値の型を確認し、適切な型にキャストする必要があります。

次に示すヘルパー クラスでは、型の確認と [**ImageEncodingProperties**](/uwp/api/Windows.Media.MediaProperties.ImageEncodingProperties) または [**VideoEncodingProperties**](/uwp/api/Windows.Media.MediaProperties.VideoEncodingProperties) へのキャストを処理しています。これにより、アプリのコードでは、2 つの型を区別する必要がなくなります。 これに加えて、ヘルパー クラスは、プロパティの縦横比、フレーム レート (ビデオ エンコード プロパティの場合のみ)、アプリの UI でエンコード プロパティをわかりやすく表示するためのフレンドリ名を使用できるように、プロパティを公開します。

ヘルパー クラスのソース ファイルには、[**Windows.Media.MediaProperties**](/uwp/api/Windows.Media.MediaProperties) 名前空間を含める必要があります。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetMediaEncodingPropertiesUsing":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/StreamPropertiesHelper.cs" id="SnippetStreamPropertiesHelper":::

## <a name="determine-if-the-preview-and-capture-streams-are-independent"></a>プレビュー ストリームとキャプチャ ストリームの独立性の判断

デバイスによっては、プレビュー ストリームとキャプチャ ストリームに同じハードウェア ピンが使用されることがあります。 このようなデバイスでは、一方のエンコード プロパティを設定すると、他方も設定されます。 キャプチャとプレビューに別々のハードウェア ピンが使用されるデバイスでは、ストリームごとのプロパティを個々に設定できます。 プレビュー ストリームとキャプチャ ストリームが独立しているかどうかを判断するには、次のコードを使用します。 このテストの結果に基づいて UI を調整し、ストリームの設定を個々に有効化または無効化する必要があります。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetCheckIfStreamsAreIdentical":::

## <a name="get-a-list-of-available-stream-properties"></a>利用可能なストリーム プロパティのリストの取得

キャプチャ デバイスについて利用可能なストリーム プロパティのリストを取得するには、アプリの [MediaCapture](./index.md) オブジェクトの [**VideoDeviceController**](/uwp/api/Windows.Media.Devices.VideoDeviceController) を取得し、[**GetAvailableMediaStreamProperties**](/uwp/api/windows.media.devices.videodevicecontroller.getavailablemediastreamproperties) を呼び出して、いずれか 1 つの [**MediaStreamType**](/uwp/api/Windows.Media.Capture.MediaStreamType) 値 (**VideoPreview**、**VideoRecord**、**Photo**) を渡します。 この例では、**GetAvailableMediaStreamProperties** から返されたそれぞれの [**IMediaEncodingProperties**](/uwp/api/Windows.Media.MediaProperties.IMediaEncodingProperties) 値に対し、この記事で定義した **StreamPropertiesHelper** オブジェクトのリストを Linq 構文で作成しています。 この例では、まず Linq 拡張メソッドを使用して、返されたプロパティを解像度順のフレーム レート順に並べ替えます。

アプリに、解像度またはフレーム レートに関する特定の要件がある場合は、メディア エンコード プロパティのセットをプログラムで選択できます。 一般的なカメラ アプリでは、目的の設定をユーザーが選択できるように、利用可能なプロパティのリストが UI で公開されます。 **StreamPropertiesHelper** オブジェクトのリストにある各項目に対して、**ComboBoxItem** が作成されます。 コンテンツは、ヘルパー クラスから返されたフレンドリ名に設定されています。タグは、関連付けられているエンコード プロパティを後で取得できるように、ヘルパー クラス自体に設定されています。 次に、メソッドに渡された **ComboBox** に、各 **ComboBoxItem** が追加されます。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetPopulateStreamPropertiesUI":::

## <a name="set-the-desired-stream-properties"></a>目的のストリーム プロパティを設定する

目的のエンコード プロパティを使用するようにビデオ デバイスのコントローラーに指示するには、[**SetMediaStreamPropertiesAsync**](/uwp/api/windows.media.devices.videodevicecontroller.setmediastreampropertiesasync) を呼び出します。このとき、写真、ビデオ、プレビューのうち、どのプロパティを設定するかを示す **MediaStreamType** 値を渡します。 この例では、**PopulateStreamPropertiesUI** ヘルパー メソッドによって設定されたいずれかの **ComboBox** オブジェクトからユーザーが項目を選択すると、要求されたエンコード プロパティが設定されます。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetPreviewSettingsChanged":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetPhotoSettingsChanged":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetVideoSettingsChanged":::

## <a name="match-the-aspect-ratio-of-the-preview-and-capture-streams"></a>プレビューとキャプチャ ストリームの縦横比を一致させる

一般的なカメラ アプリでは、ユーザーがビデオや写真のキャプチャ解像度を選択するための UI を提供することも、プログラムによってプレビュー解像度を設定することもあります。 アプリの最適なプレビュー ストリーム解像度を選択するための方法はいくつかあります。

-   UI フレームワークで必要なプレビューのスケーリングを実行できるように、利用可能な最高のプレビュー解像度を選択します。

-   最終的にキャプチャされたメディアに最も近い表現がプレビューに表示されるように、キャプチャ解像度に最も近いプレビュー解像度を選択します。

-   必要以上のピクセルがプレビュー ストリーム パイプラインを通過しないように、[**CaptureElement**](/uwp/api/Windows.UI.Xaml.Controls.CaptureElement) のサイズに最も近いプレビュー解像度を選択します。

**重要**   一部のデバイスでは、カメラのプレビューストリームとキャプチャストリームに異なる縦横比を設定することができます。 この不一致によってフレームのトリミングが生じた場合、プレビューで表示されなかったコンテンツがキャプチャしたメディアに存在するという結果を招く可能性があり、これは否定的なユーザー エクスペリエンスにつながります。 プレビュー ストリームとキャプチャ ストリームには、微小な公差範囲内で同一の縦横比を使用することを強くお勧めします。 縦横比がほぼ一致していれば、キャプチャとプレビューにまったく異なる解像度を有効にしても問題ありません。


写真やビデオのキャプチャ ストリームをプレビュー ストリームの縦横比に一致させるために、この例では [**VideoDeviceController.GetMediaStreamProperties**](/uwp/api/windows.media.devices.videodevicecontroller.getmediastreamproperties) を呼び出し、**VideoPreview** 列挙値を渡して、プレビュー ストリームの現在のストリーム プロパティを要求しています。 次に、プレビュー ストリームとまったく同じでなくても、近似値であれば、その縦横比を許容できるように、縦横比の微小な公差範囲を定義しています。 次に、プレビュー ストリームについて定義済みの公差範囲に縦横比が含まれるような **StreamPropertiesHelper** オブジェクトだけを選択できるように、Linq 拡張メソッドが使用されています。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetMatchPreviewAspectRatio":::

 

 
