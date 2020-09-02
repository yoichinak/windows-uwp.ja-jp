---
ms.assetid: E0189423-1DF3-4052-AB2E-846EA18254C4
description: このトピックでは、カメラのプレビューおよび録画中のビデオ ストリームに効果を適用する方法と、ビデオ手ブレ補正効果を使用する方法について説明します。
title: ビデオ キャプチャの効果
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ffb379110a42579cd5bb2427f9c851637ff191be
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/02/2020
ms.locfileid: "89362675"
---
# <a name="effects-for-video-capture"></a>ビデオ キャプチャの効果


このトピックでは、カメラのプレビューおよび録画中のビデオ ストリームに効果を適用する方法と、ビデオ手ブレ補正効果を使用する方法について説明します。

> [!NOTE] 
> この記事の内容は、写真やビデオの基本的なキャプチャ機能を実装するための手順を紹介した「[MediaCapture を使った基本的な写真、ビデオ、およびオーディオのキャプチャ](basic-photo-video-and-audio-capture-with-MediaCapture.md)」で取り上げた概念やコードに基づいています。 そちらの記事で基本的なメディア キャプチャのパターンを把握してから、高度なキャプチャ シナリオに進むことをお勧めします。 この記事で紹介しているコードは、MediaCapture のインスタンスが既に作成され、適切に初期化されていることを前提としています。

## <a name="adding-and-removing-effects-from-the-camera-video-stream"></a>カメラのビデオ ストリームに対する効果の追加と削除
デバイスのカメラからビデオをプレビューまたはキャプチャするには、「[MediaCapture を使った基本的な写真、ビデオ、およびオーディオのキャプチャ](basic-photo-video-and-audio-capture-with-MediaCapture.md)」で説明されているように、[**MediaCapture**](/uwp/api/Windows.Media.Capture.MediaCapture) オブジェクトを使用します。 **MediaCapture** オブジェクトを初期化した後、プレビュー ストリームやキャプチャ ストリームに 1 つまたは複数のビデオ効果を追加できます。そのためには、[**AddVideoEffectAsync**](/uwp/api/windows.media.capture.mediacapture.addvideoeffectasync) を呼び出して、追加する効果を表す [**IVideoEffectDefinition**](/uwp/api/Windows.Media.Effects.IVideoEffectDefinition) オブジェクトと、カメラのプレビュー ストリームとレコード ストリームのどちらに効果を追加するかを示す [**MediaStreamType**](/uwp/api/Windows.Media.Capture.MediaStreamType) 列挙のメンバーを渡します。

> [!NOTE]
> 一部のデバイスでは、プレビュー ストリームとキャプチャ ストリームが同じである場合があります。これは、**AddVideoEffectAsync** を呼び出すときに **MediaStreamType.VideoPreview** または **MediaStreamType.VideoRecord** を指定すると、プレビュー ストリームとレコード ストリームの両方に効果が適用されることを意味します。 現在のデバイスでプレビュー ストリームとレコード ストリームが同じであるかどうかを確認するには、**MediaCapture** オブジェクトの [**MediaCaptureSettings**](/uwp/api/windows.media.capture.mediacapture.mediacapturesettings) の [**VideoDeviceCharacteristic**](/uwp/api/windows.media.capture.mediacapturesettings.videodevicecharacteristic) プロパティを調べます。 このプロパティの値が **VideoDeviceCharacteristic.AllStreamsIdentical** または **VideoDeviceCharacteristic.PreviewRecordStreamsIdentical** である場合、ストリームは同じであり、一方に対して適用したすべての効果はもう一方にも影響します。

次の例では、カメラのプレビュー ストリームとレコード ストリームの両方に効果を追加します。 この例では、レコード ストリームとプレビュー ストリームが同じかどうかを確認する方法を示します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs" id="SnippetBasicAddEffect":::

**AddVideoEffectAsync** は、追加されたビデオ効果を表す [**IMediaExtension**](/uwp/api/Windows.Media.IMediaExtension) を実装するオブジェクトを返します。 一部の効果では、[**PropertySet**](/uwp/api/Windows.Foundation.Collections.PropertySet) を [**SetProperties**](/uwp/api/windows.media.imediaextension.setproperties) メソッドに渡すことによって、効果の設定を変更できます。

Windows 10 バージョン 1607 では、**AddVideoEffectAsync** によって返されるオブジェクトを [**RemoveEffectAsync**](/uwp/api/windows.media.capture.mediacapture.removeeffectasync) に渡すことによって、ビデオ パイプラインから効果を削除することもできます。 **RemoveEffectAsync** は、効果オブジェクトのパラメーターがプレビュー ストリームとレコード ストリームのどちらに追加されたかを自動的に特定するため、呼び出しの際にストリームの種類を指定する必要はありません。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs" id="SnippetRemoveOneEffect":::

また、[**ClearEffectsAsync**](/uwp/api/windows.media.capture.mediacapture.cleareffectsasync) を呼び出し、すべての効果を削除するストリームを指定して、プレビュー ストリームやキャプチャ ストリームからすべての効果を削除することもできます。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs" id="SnippetClearAllEffects":::

## <a name="video-stabilization-effect"></a>ビデオ手ブレ補正効果

ビデオ手ブレ補正効果は、キャプチャ デバイスを手で支えることによって生じる動きを極力目立たなくするために、ビデオ ストリームのフレームを操作します。 この手法を適用するとピクセルが上下左右に移動されますが、ビデオ フレームのすぐ外側に何があるのかは、手ブレ補正効果のロジックにはわからないため、手ブレ補正後は元のビデオがわずかにトリミングされた状態となります。 ここで紹介しているユーティリティ関数を使いビデオ エンコードの設定を調整することで、手ブレ補正効果によるトリミングをベストな状態に制御することができます。

デバイス側でサポートされていれば、Optical Image Stabilization (OIS) がキャプチャ デバイスを機械的に操作することでビデオのブレが抑えられるため、ビデオ フレームの端をトリミングする必要はありません。 詳しくは、「[ビデオ キャプチャのためのキャプチャ デバイス コントロール](capture-device-controls-for-video-capture.md)」をご覧ください。

### <a name="set-up-your-app-to-use-video-stabilization"></a>ビデオの手ブレ補正を行うようにアプリを設定する

ビデオ手ブレ補正効果を使うためには、基本的なメディア キャプチャに必要な名前空間に加え、次の名前空間が必要となります。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs" id="SnippetVideoStabilizationEffectUsing":::

[**VideoStabilizationEffect**](/uwp/api/Windows.Media.Core.VideoStabilizationEffect) オブジェクトを格納するためのメンバー変数を宣言します。 効果の実装の一部として、キャプチャしたビデオをエンコードするために使うエンコードのプロパティを変更します。 後で効果が無効にされたときに入出力のエンコード プロパティを復元できるよう、初期状態のバックアップ コピーを格納するための 2 つの変数を宣言します。 最後に、[**MediaEncodingProfile**](/uwp/api/Windows.Media.MediaProperties.MediaEncodingProfile) 型のメンバー変数を宣言します。メンバー変数として宣言しているのは、コードのいたるところからこのオブジェクトにアクセスすることになるためです。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs" id="SnippetDeclareVideoStabilizationEffect":::

このシナリオでは、後からアクセスできるようメディア エンコード プロファイル オブジェクトをメンバー変数に代入する必要があります。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs" id="SnippetEncodingProfileMember":::

### <a name="initialize-the-video-stabilization-effect"></a>ビデオ手ブレ補正効果を初期化する

**MediaCapture** オブジェクトの初期化後、[**VideoStabilizationEffectDefinition**](/uwp/api/Windows.Media.Core.VideoStabilizationEffectDefinition) オブジェクトの新しいインスタンスを作成します。 効果をビデオ パイプラインに追加し、[**VideoStabilizationEffect**](/uwp/api/Windows.Media.Core.VideoStabilizationEffect) クラスのインスタンスを取得するには、[**MediaCapture.AddVideoEffectAsync**](/uwp/api/windows.media.capture.mediacapture.addvideoeffectasync) を呼び出します。 [**MediaStreamType.VideoRecord**](/uwp/api/Windows.Media.Capture.MediaStreamType) を指定すると、ビデオ レコード ストリームに効果を適用する、という意味になります。

[**EnabledChanged**](/uwp/api/windows.media.core.videostabilizationeffect.enabledchanged) イベントのハンドラーを登録し、ヘルパー メソッド **SetUpVideoStabilizationRecommendationAsync** を呼び出します。詳細については後で説明します。 最後に、[**Enabled**](/uwp/api/windows.media.core.videostabilizationeffect.enabled) プロパティを true に設定して効果を有効にします。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs" id="SnippetCreateVideoStabilizationEffect":::

### <a name="use-recommended-encoding-properties"></a>推奨エンコード プロパティを使う

先ほども触れましたが、ビデオ手ブレ補正効果に使われている手法では必然的に、補正後、わずかですが元のビデオがトリミングされます。 これは手ブレ補正効果の制限です。ビデオ エンコード プロパティを調整して、できるだけ適切に対処するために、以下のヘルパー関数をコードの中で定義してください。 このステップはビデオ手ブレ補正効果を使ううえで必須ではありませんが、これを省略した場合、最終的に得られるビデオが若干アップスケーリングされるために、ビジュアルの再現性がわずかに低下します。

VideoStabilizationEffect のインスタンスの [**GetRecommendedStreamConfiguration**](/uwp/api/windows.media.core.videostabilizationeffect.getrecommendedstreamconfiguration) を呼び出します。このメソッドは、ビデオ補正効果に対して現在の入力ストリームのエンコード プロパティを伝える [**VideoDeviceController**](/uwp/api/Windows.Media.Devices.VideoDeviceController) オブジェクトと、現在の出力エンコード プロパティを伝える **MediaEncodingProfile** とを引数として受け取ります。 このメソッドの戻り値である [**VideoStreamConfiguration**](/uwp/api/Windows.Media.Capture.VideoStreamConfiguration) オブジェクトには、入力ストリームと出力ストリームの最新の推奨エンコード プロパティが格納されます。

効果のトリミング適用後に失われる解像度を最小限にするために、入力の推奨エンコード プロパティは、デバイスによってサポートされる範囲で、指定した初期設定よりも解像度が高くなります。

新しいエンコード プロパティを設定するには、[**VideoDeviceController.SetMediaStreamPropertiesAsync**](/uwp/api/windows.media.devices.videodevicecontroller.setmediastreampropertiesasync) を呼び出します。 効果を無効にしたときに設定を元に戻すことができるよう、新しいプロパティを設定する前にメンバー変数を使って初期エンコード プロパティを保存します。

ビデオ手ブレ補正効果で出力ビデオをトリミングする必要がある場合、出力の推奨エンコード プロパティは、トリミングされたビデオのサイズになります。 つまり出力の解像度は、トリミングされたビデオのサイズと一致します。 推奨される出力プロパティを使わなかった場合、最初の出力サイズに合わせてビデオがアップスケーリングされるため、ビジュアルの再現性が低下します。

**MediaEncodingProfile** オブジェクトの [**Video**](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.video) プロパティを設定します。 効果を無効にしたときに設定を元に戻すことができるよう、新しいプロパティを設定する前にメンバー変数を使って初期エンコード プロパティを保存します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs" id="SnippetSetUpVideoStabilizationRecommendationAsync":::

### <a name="handle-the-video-stabilization-effect-being-disabled"></a>ビデオ手ブレ補正効果の無効化イベントを処理する

ピクセル スループットが高すぎて効果の処理が追い付かない場合や、効果の実行に時間がかかっていることをシステムが検出した場合、ビデオ手ブレ補正効果がシステムによって自動的に無効化されます。 このように状態が変化した場合、EnabledChanged イベントが発生します。 最新の効果の状態 (有効または無効) は、*sender* パラメーターに格納された **VideoStabilizationEffect** のインスタンスによって知ることができます。 [**VideoStabilizationEffectEnabledChangedEventArgs**](/uwp/api/Windows.Media.Core.VideoStabilizationEffectEnabledChangedEventArgs) に格納される [**VideoStabilizationEffectEnabledChangedReason**](/uwp/api/Windows.Media.Core.VideoStabilizationEffectEnabledChangedReason) の値は、効果が有効または無効にされた理由を示しています。 このイベントは、プログラムから効果を有効または無効にした場合にも発生します。この場合の理由は **Programmatic** になります。

通常、このイベントを使ってアプリの UI を調整し、ビデオ手ブレ補正の現在の状態を示します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs" id="SnippetVideoStabilizationEnabledChanged":::

### <a name="clean-up-the-video-stabilization-effect"></a>ビデオ手ブレ補正効果をクリーンアップする

ビデオ手ブレ補正効果をクリーンアップするには、[**RemoveEffectAsync**](/uwp/api/windows.media.capture.mediacapture.removeeffectasync) を呼び出してビデオ パイプラインから効果を削除します。 初期エンコード プロパティを格納しているメンバー変数が null 以外であれば、それらの変数を使ってエンコード プロパティを復元します。 最後に、**EnabledChanged** イベント ハンドラーを削除し、効果を null に設定します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs" id="SnippetCleanUpVisualStabilizationEffect":::

## <a name="related-topics"></a>関連トピック

* [カメラ](camera.md)
* [MediaCapture を使った基本的な写真、ビデオ、およびオーディオのキャプチャ](basic-photo-video-and-audio-capture-with-MediaCapture.md)
 

 
