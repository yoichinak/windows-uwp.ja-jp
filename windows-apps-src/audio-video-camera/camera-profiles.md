---
ms.assetid: 42A06423-670F-4CCC-88B7-3DCEEDDEBA57
description: この記事では、カメラ プロファイルを使ってさまざまなビデオ キャプチャ デバイスの機能を検出および管理する方法について説明します。 これには、特定の解像度やフレーム レートをサポートするプロファイル、複数のカメラへの同時アクセスをサポートするプロファイル、HDR をサポートするプロファイルを選ぶなどのタスクが含まれます。
title: カメラ プロファイルを使用したカメラ機能の検出と選択
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f42b58794b62753ff325cb4fc23202e5a48047d4
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/02/2020
ms.locfileid: "89364025"
---
# <a name="discover-and-select-camera-capabilities-with-camera-profiles"></a>カメラ プロファイルを使用したカメラ機能の検出と選択



この記事では、カメラ プロファイルを使ってさまざまなビデオ キャプチャ デバイスの機能を検出および管理する方法について説明します。 これには、特定の解像度やフレーム レートをサポートするプロファイル、複数のカメラへの同時アクセスをサポートするプロファイル、HDR をサポートするプロファイルを選ぶなどのタスクが含まれます。

> [!NOTE] 
> この記事の内容は、写真やビデオの基本的なキャプチャ機能を実装するための手順を紹介した「[MediaCapture を使った基本的な写真、ビデオ、およびオーディオのキャプチャ](basic-photo-video-and-audio-capture-with-MediaCapture.md)」で取り上げた概念やコードに基づいています。 そちらの記事で基本的なメディア キャプチャのパターンを把握してから、高度なキャプチャ シナリオに進むことをお勧めします。 この記事で紹介しているコードは、MediaCapture のインスタンスが既に作成され、適切に初期化されていることを前提としています。

 

## <a name="about-camera-profiles"></a>カメラ プロファイルについて

カメラが搭載されているデバイスによって、キャプチャ解像度、ビデオ キャプチャのフレーム レート、HDR または可変フレーム レート キャプチャなど、サポートされている機能も異なります。 ユニバーサル Windows プラットフォーム (UWP) メディア キャプチャ フレームワークでは、この機能セットが [**MediaCaptureVideoProfileMediaDescription**](/uwp/api/Windows.Media.Capture.MediaCaptureVideoProfileMediaDescription) に格納されます。 [**MediaCaptureVideoProfile**](/uwp/api/Windows.Media.Capture.MediaCaptureVideoProfile) オブジェクトで表されるカメラ プロファイルには、メディア記述のコレクションが 3 つ含まれています。1 つは写真のキャプチャ用、1 つはビデオ キャプチャ用、もう 1 つはビデオ プレビュー用です。

[MediaCapture](./index.md) オブジェクトを初期化する前に、現在のデバイスのキャプチャ デバイスを照会して、サポートされているプロファイルを確認することができます。 サポートされているプロファイルを選択すると、機能、プロファイルのメディア記述に含まれているすべての機能がすべてキャプチャ デバイスでサポートされることがわかります。 これにより、特定のデバイスでどのような組み合わせの機能がサポートされているか確認するために試行錯誤する必要がなくなります。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetBasicInitExample":::

この記事のコード例では、最小限の初期化が、さまざまな機能をサポートするカメラ プロファイルの検出に置き換わっています。検出されたプロファイルは、メディア キャプチャ デバイスの初期化に使用されます。

## <a name="find-a-video-device-that-supports-camera-profiles"></a>カメラ プロファイルをサポートするビデオ デバイスを検出する

サポートされているカメラ プロファイルを検索する前に、カメラ プロファイルの使用がサポートされるキャプチャ デバイスを検出する必要があります。 次の例で定義されている **GetVideoProfileSupportedDeviceIdAsync** ヘルパー メソッドでは、[**DeviceInformaion.FindAllAsync**](/uwp/api/windows.devices.enumeration.deviceinformation.findallasync) メソッドを使って、すべての利用可能なビデオ キャプチャ デバイスの一覧を取得しています。 個々のデバイスについて静的メソッド [**IsVideoProfileSupported**](/uwp/api/windows.media.capture.mediacapture.isvideoprofilesupported) を呼び出し、ビデオ プロファイルがサポートされるかどうかを確認する処理が、一覧内のすべてのデバイスに対してループで実行されます。 また、各デバイスの [**EnclosureLocation.Panel**](/uwp/api/windows.devices.enumeration.enclosurelocation.panel) プロパティで、使用するカメラがデバイスの前面にあるか背面にあるかを指定することができます。

指定された面に、カメラ プロファイルをサポートするデバイスが見つかった場合は、デバイスの ID 文字列が含まれている [**Id**](/uwp/api/windows.devices.enumeration.deviceinformation.id) 値が返されます。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetGetVideoProfileSupportedDeviceIdAsync":::

**GetVideoProfileSupportedDeviceIdAsync** ヘルパー メソッドから返されたデバイス ID が null または空の文字列であれば、指定された面にはカメラ プロファイルをサポートするデバイスがありません。 この場合は、プロファイルを使用せずにメディア キャプチャ デバイスを初期化する必要があります。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetGetDeviceWithProfileSupport":::

## <a name="select-a-profile-based-on-supported-resolution-and-frame-rate"></a>サポートされている解像度とフレーム レートに基づいてプロファイルを選択する

特定の解像度やフレーム レートを確保できるなど、特定の機能が含まれたプロファイルを選択するには、先ほど定義したヘルパー メソッドをまず呼び出して、カメラ プロファイルの使用をサポートするキャプチャ デバイスの ID を取得する必要があります。

選択されたデバイス ID を渡して、新しい [**MediaCaptureInitializationSettings**](/uwp/api/Windows.Media.Capture.MediaCaptureInitializationSettings) オブジェクトを作成します。 次に、静的メソッド [**MediaCapture.FindAllVideoProfiles**](/uwp/api/windows.media.capture.mediacapture.findallvideoprofiles) を呼び出して、デバイスでサポートされているすべてのカメラ プロファイルの一覧を取得します。

この例では、**System.Linq** 名前空間に含まれている Linq クエリの手法を使用して、[**Width**](/uwp/api/windows.media.capture.mediacapturevideoprofilemediadescription.width)、[**Height**](/uwp/api/windows.media.capture.mediacapturevideoprofilemediadescription.height)、[**FrameRate**](/uwp/api/windows.media.capture.mediacapturevideoprofilemediadescription.framerate) の各プロパティが要求値に一致する [**SupportedRecordMediaDescription**](/uwp/api/windows.media.capture.mediacapturevideoprofile.supportedrecordmediadescription) オブジェクトが含まれたプロパティを選択しています。 一致が見つかった場合、**MediaCaptureInitializationSettings** の [**VideoProfile**](/uwp/api/windows.media.capture.mediacaptureinitializationsettings.videoprofile) と [**RecordMediaDescription**](/uwp/api/windows.media.capture.mediacaptureinitializationsettings.recordmediadescription) が、Linq クエリから返された匿名型の値に設定されます。 一致が見つからなかった場合は、既定のプロパティが使われます。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetFindWVGA30FPSProfile":::

目的のカメラ プロファイルを **MediaCaptureInitializationSettings** に設定した後、メディア キャプチャ オブジェクトで [**InitializeAsync**](/uwp/api/windows.media.capture.mediacapture.initializeasync) を呼び出して、目的のプロファイルに構成します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetInitCaptureWithProfile":::

## <a name="use-media-frame-source-groups-to-get-profiles"></a>メディア フレーム ソース グループを使用してプロファイルを取得する

Windows 10、バージョン 1803 以降では、[**MediaFrameSourceGroup**](/uwp/api/windows.media.capture.frames.mediaframesourcegroup) クラスを使用して、特定の機能を備えたカメラ プロファイルを取得した後に、**MediaCapture** オブジェクトを初期化できます。 デバイス メーカーは、フレーム ソース グループを使用して、一連のセンサー機能やキャプチャ機能を 1 つの仮想デバイスとして表すことができます。 これにより、深度とカラー カメラを組み合わせるなどのコンピュテーショナル フォトグラフィー (計算写真学) のシナリオが可能になるだけでなく、単純なキャプチャのシナリオでカメラ プロファイルを選択するためにも使用できます。 **MediaFrameSourceGroup** の使用方法について詳しくは、「[MediaFrameReader を使ったメディア フレームの処理](process-media-frames-with-mediaframereader.md)」をご覧ください。

次のサンプル メソッドでは、**MediaFrameSourceGroup** オブジェクトを使用して、既知のビデオ プロファイルをサポートしているカメラ プロファイル (HDR や可変の写真シーケンスをサポートするカメラ プロファイルなど) を検索する方法を示しています。 まず、[**MediaFrameSourceGroup.FindAllAsync**](/uwp/api/windows.media.capture.frames.mediaframesourcegroup.findallasync) を呼び出して、現在のデバイス上で利用可能なすべてのメディア フレーム ソース グループの一覧を取得します。 ループ処理によって各ソース グループで [**MediaCapture.FindKnownVideoProfiles**](/uwp/api/windows.media.capture.mediacapture.findknownvideoprofiles) を呼び出し、現在のソース グループについて、指定したプロファイル (この例では HDR/WCG 写真) をサポートしているすべてのビデオ プロファイルの一覧を取得します。 条件に適合するプロファイルが見つかった場合、新しい **MediaCaptureInitializationSettings** オブジェクトが作成され、**VideoProfile** が選択したプロファイルに設定されると共に、**VideoDeviceId** が現在のメディア フレーム ソース グループの **Id** プロパティに設定されます。 これにより、たとえば **KnownVideoProfile.HdrWithWcgVideo** をこのメソッドに渡すと、HDR ビデオをサポートしているメディア キャプチャ設定を取得できます。 また **KnownVideoProfile.VariablePhotoSequence** を渡すと、加変の写真シーケンスをサポートしている設定を取得できます。

 :::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetFindKnownVideoProfile":::

## <a name="use-known-profiles-to-find-a-profile-that-supports-hdr-video-legacy-technique"></a>既知のプロファイルを使用して HDR ビデオをサポートするプロファイルを検出する (従来の手法)

> [!NOTE] 
> このセクションで説明されている API は、Windows 10、バージョン 1803 以降では非推奨です。 上記の「**メディア フレーム ソース グループを使用してプロファイルを取得する**」をご覧ください。

HDR をサポートするプロファイルの選択も、他のシナリオと同じように始まります。 **MediaCaptureInitializationSettings**と、キャプチャデバイス ID を保持する文字列を作成します。 HDR ビデオがサポートされているかどうかを追跡するためのブール変数を追加します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetGetHdrProfileSetup":::

上で定義した **GetVideoProfileSupportedDeviceIdAsync** ヘルパー メソッドを使って、カメラ プロファイルをサポートしているキャプチャ デバイスのデバイス ID を取得します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetFindDeviceHDR":::

静的メソッド [**MediaCapture.FindKnownVideoProfiles**](/uwp/api/windows.media.capture.mediacapture.findknownvideoprofiles) により、指定の [**KnownVideoProfile**](/uwp/api/Windows.Media.Capture.KnownVideoProfile) 値で分類された指定のデバイスでサポートされるカメラ プロファイルが返されます。 このシナリオでは、ビデオ録画をサポートするカメラ プロファイルのみが返されるように、**VideoRecording** 値が指定されています。

返されたカメラ プロファイルの一覧をループ処理します。 各カメラ プロファイルで、プロファイル内の各 [**VideoProfileMediaDescription**](/uwp/api/Windows.Media.Capture.MediaCaptureVideoProfileMediaDescription) をループ処理して、[**IsHdrVideoSupported**](/uwp/api/windows.media.capture.mediacapturevideoprofilemediadescription.ishdrvideosupported) プロパティの値が true かどうかをチェックします。 適切なメディア記述が見つかったら、ループを抜けて、プロファイルと記述のオブジェクトを **MediaCaptureInitializationSettings** オブジェクトに割り当てます。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetFindHDRProfile":::

## <a name="determine-if-a-device-supports-simultaneous-photo-and-video-capture"></a>写真とビデオの同時キャプチャがデバイスでサポートされているかどうかを確認する

多くのデバイスでは、写真とビデオの同時キャプチャがサポートされます。 キャプチャ デバイスでこの動作がサポートされているかどうかを確認するには、[**MediaCapture.FindAllVideoProfiles**](/uwp/api/windows.media.capture.mediacapture.findallvideoprofiles) を呼び出して、デバイスでサポートされているすべてのカメラ プロファイルを取得します。 Linq クエリを使って、[**SupportedPhotoMediaDescription**](/uwp/api/windows.media.capture.mediacapturevideoprofile.supportedphotomediadescription) と [**SupportedRecordMediaDescription**](/uwp/api/windows.media.capture.mediacapturevideoprofile.supportedrecordmediadescription) に 1 つ以上のエントリがある (プロファイルで同時キャプチャがサポートされていることを意味する) プロファイルを検出します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetGetPhotoAndVideoSupport":::

このクエリを調整して、同時ビデオ録画以外にも、特定の解像度やその他の機能をサポートするプロファイルを検出することもできます。 また、[**MediaCapture.FindKnownVideoProfiles**](/uwp/api/windows.media.capture.mediacapture.findknownvideoprofiles) を使って [**BalancedVideoAndPhoto**](/uwp/api/Windows.Media.Capture.KnownVideoProfile) 値を指定し、同時キャプチャをサポートするプロファイルを取得することもできますが、すべてのプロファイルを照会する方が完全な結果を得ることができます。

## <a name="related-topics"></a>関連トピック

* [カメラ](camera.md)
* [MediaCapture を使った基本的な写真、ビデオ、およびオーディオのキャプチャ](basic-photo-video-and-audio-capture-with-MediaCapture.md)
 

 
