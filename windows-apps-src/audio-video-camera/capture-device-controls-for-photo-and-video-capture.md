---
ms.assetid: 831123A7-1F40-4B74-AE9F-69AC9883B4AD
description: この記事では、光学式手ブレ補正やスムーズ ズームなど、写真とビデオのキャプチャに関する拡張シナリオを可能にするために、手動デバイス制御を使う方法について説明します。
title: 写真とビデオのキャプチャのための手動カメラ制御
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d7248b4f3fe515a164410305bac074f44cae53e6
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/02/2020
ms.locfileid: "89362865"
---
# <a name="manual-camera-controls-for-photo-and-video-capture"></a>写真とビデオのキャプチャのための手動カメラ制御



この記事では、光学式手ブレ補正やスムーズ ズームなど、写真とビデオのキャプチャに関する拡張シナリオを可能にするために、手動デバイス制御を使う方法について説明します。

この記事で説明するコントロールはすべて、同じパターンを使ってアプリに追加されます。 まず、アプリが実行されている現在のデバイスで、コントロールがサポートされているかどうかを確認します。 コントロールがサポートされている場合は、コントロールに対して必要なモードを設定します。 一般的に、現在のデバイスで特定のコントロールがサポートされていない場合は、ユーザーがその機能を有効にできるような UI 要素を無効または非表示にする必要があります。

この記事のコードは、[カメラの手動コントロール SDK のサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraManualControls)を基にしています。 このサンプルをダウンロードし、該当するコンテキストで使用されているコードを確認することも、サンプルを独自のアプリの開始点として使用することもできます。

> [!NOTE]
> この記事の内容は、写真やビデオの基本的なキャプチャ機能を実装するための手順を紹介した「[MediaCapture を使った基本的な写真、ビデオ、およびオーディオのキャプチャ](basic-photo-video-and-audio-capture-with-MediaCapture.md)」で取り上げた概念やコードに基づいています。 そちらの記事で基本的なメディア キャプチャのパターンを把握してから、高度なキャプチャ シナリオに進むことをお勧めします。 この記事で紹介しているコードは、MediaCapture のインスタンスが既に作成され、適切に初期化されていることを前提としています。

この記事で説明するデバイス制御 API はすべて、[**Windows.Media.Devices**](/uwp/api/Windows.Media.Devices) 名前空間のメンバーです。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetVideoControllersUsing":::

## <a name="exposure"></a>露出

[**ExposureControl**](/uwp/api/Windows.Media.Devices.ExposureControl) によって、写真やビデオのキャプチャ時に使用されるシャッター速度を設定できます。

この例では、[**スライダー**](/uwp/api/Windows.UI.Xaml.Controls.Slider) コントロールを使って現在の露出値を調整し、チェック ボックスを使って自動露出調整のオンとオフを切り替えます。

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml" id="SnippetExposureXAML":::

現在のキャプチャ デバイスで **ExposureControl** がサポートされているかどうかを確認するには、[**Supported**](/uwp/api/windows.media.devices.exposurecontrol.supported) プロパティを確認します。 コントロールがサポートされている場合は、この機能の UI を表示し、有効にすることができます。 自動露出調整が現在アクティブであるかどうかを示すために、チェック ボックスのオンの状態を [**Auto**](/uwp/api/windows.media.devices.exposurecontrol.auto) プロパティの値に設定します。

露出値は、デバイスでサポートされている範囲内である必要があり、サポートされているステップ サイズのインクリメントである必要があります。 現在のデバイスでサポートされている値を取得するには、[**Min**](/uwp/api/windows.media.devices.exposurecontrol.min)、[**Max**](/uwp/api/windows.media.devices.exposurecontrol.max)、および [**Step**](/uwp/api/windows.media.devices.exposurecontrol.step) プロパティを確認します。これらのプロパティは、対応するスライダー コントロールのプロパティを設定するために使用されます。

値が設定されたときにイベントがトリガーされないように、[**ValueChanged**](/uwp/api/windows.ui.xaml.controls.primitives.rangebase.valuechanged) イベント ハンドラーの登録を解除した後、スライダー コントロールの値を **ExposureControl** の現在値に設定します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetExposureControl":::

**ValueChanged** イベント ハンドラーで、コントロールの現在の値を取得し、[**SetValueAsync**](/uwp/api/windows.media.devices.exposurecontrol.setvalueasync) を呼び出して露出値を設定します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetExposureSlider":::

自動露出チェック ボックスの **CheckedChanged** イベント ハンドラーで、[**SetAutoAsync**](/uwp/api/windows.media.devices.exposurecontrol.setautoasync) を呼び出してブール値を渡すことにより、自動露出調整のオンとオフを切り替えます。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetExposureCheckBox":::

> [!IMPORTANT]
> 自動露出モードは、プレビュー ストリームが実行中であるときにのみサポートされます。 自動露出をオンにする前に、プレビュー ストリームが実行されていることを確認します。

## <a name="exposure-compensation"></a>露出補正

[**ExposureCompensationControl**](/uwp/api/Windows.Media.Devices.ExposureCompensationControl) によって、写真やビデオのキャプチャ時に使用される露出補正を設定できます。

この例では、[**スライダー**](/uwp/api/Windows.UI.Xaml.Controls.Slider) コントロールを使って、現在の露出補正値を調整します。

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml" id="SnippetEvXAML":::

現在のキャプチャ デバイスで **ExposureCompensationControl** がサポートされているかどうかを確認するには、[Supported](supported-codecs.md) プロパティを確認します。 コントロールがサポートされている場合は、この機能の UI を表示し、有効にすることができます。

露出補正値は、デバイスでサポートされている範囲内である必要があり、サポートされているステップ サイズのインクリメントである必要があります。 現在のデバイスでサポートされている値を取得するには、[**Min**](/uwp/api/windows.media.devices.exposurecompensationcontrol.min)、[**Max**](/uwp/api/windows.media.devices.exposurecompensationcontrol.max)、および [**Step**](/uwp/api/windows.media.devices.exposurecompensationcontrol.step) プロパティを確認します。これらのプロパティは、対応するスライダー コントロールのプロパティを設定するために使用されます。

値が設定されたときにイベントがトリガーされないように、[**ValueChanged**](/uwp/api/windows.ui.xaml.controls.primitives.rangebase.valuechanged) イベント ハンドラーの登録を解除した後、スライダー コントロールの値を **ExposureCompensationControl** の現在値に設定します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetEvControl":::

**ValueChanged** イベント ハンドラーで、コントロールの現在の値を取得し、[**SetValueAsync**](/uwp/api/windows.media.devices.exposurecompensationcontrol.setvalueasync) を呼び出して露出値を設定します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetEvValueChanged":::

## <a name="flash"></a>点滅

[**FlashControl**](/uwp/api/Windows.Media.Devices.FlashControl) によって、フラッシュを有効または無効にしたり、フラッシュを使うかどうかをシステムが動的に判断する、自動フラッシュを有効にしたりすることができます。 このコントロールによって、サポートされているデバイスで自動赤目軽減を有効にすることもできます。 これらの設定はすべて写真のキャプチャに適用されます。 [**TorchControl**](/uwp/api/Windows.Media.Devices.TorchControl) は、ビデオ キャプチャ時のトーチのオンとオフを切り替える別のコントロールです。

この例では、一連のラジオ ボタンを使って、ユーザーがオン、オフ、自動のフラッシュ設定を切り替えることができるようにします。 赤目軽減とビデオ トーチのオンとオフを切り替えるためのチェック ボックスも用意されています。

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml" id="SnippetFlashXAML":::

現在のキャプチャ デバイスで **FlashControl** がサポートされているかどうかを確認するには、[**Supported**](/uwp/api/windows.media.devices.focuscontrol.supported) プロパティを確認します。 コントロールがサポートされている場合は、この機能の UI を表示し、有効にすることができます。 **FlashControl** がサポートされている場合でも、自動赤目軽減はサポートされている場合とサポートされていない場合があるため、UI を有効にする前に [**RedEyeReductionSupported**](/uwp/api/windows.media.devices.flashcontrol.redeyereductionsupported) プロパティを確認します。 **TorchControl** はフラッシュ コントロールとは別であるため、使用する前にその [**Supported**](/uwp/api/windows.media.devices.torchcontrol.supported) プロパティも確認する必要があります。

フラッシュの各ラジオ ボタンの [**Checked**](/uwp/api/windows.ui.xaml.controls.primitives.togglebutton.checked) イベント ハンドラーで、適切な対応するフラッシュの設定を有効または無効にします。 フラッシュが常に使用されるように設定するには、[**Enabled**](/uwp/api/windows.media.devices.flashcontrol.enabled) プロパティを true に、[**Auto**](/uwp/api/windows.media.devices.flashcontrol.auto) プロパティを false に設定する必要があります。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetFlashControl":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetFlashRadioButtons":::

赤目軽減チェック ボックスのハンドラーで、[**RedEyeReduction**](/uwp/api/windows.media.devices.flashcontrol.redeyereduction) プロパティを適切な値に設定します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetRedEye":::

最後に、ビデオ トーチ チェック ボックスのハンドラーで、[**Enabled**](/uwp/api/windows.media.devices.torchcontrol.enabled) プロパティを適切な値に設定します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetTorch":::

> [!NOTE] 
>  一部のデバイスでは、[**TorchControl.Enabled**](/uwp/api/windows.media.devices.torchcontrol.enabled) が true に設定されている場合でも、デバイスがプレビュー ストリームを実行中で、アクティブにビデオをキャプチャ中ではない限り、トーチは発光しません。 推奨される処理の順序は、ビデオのプレビューを有効にし、**Enabled** を true に設定してトーチを有効にした後、ビデオ キャプチャを開始するという順序です。 一部のデバイスでは、プレビューを開始した後もトーチが点灯しません。 その他のデバイスでは、トーチはビデオのキャプチャを開始するまで点灯しません。

## <a name="focus"></a>対象

[**FocusControl**](/uwp/api/Windows.Media.Devices.FocusControl) オブジェクトでは、カメラのフォーカスを調整するためによく使用される 3 種類の方法である、連続オート フォーカス、タップしてフォーカス、手動フォーカスがサポートされています。 カメラ アプリでこれらの 3 つの方法がすべてをサポートされている可能性がありますが、分かりやすくするために、この記事ではそれぞれの手法を個別に説明します。 このセクションでは、フォーカス アシスト ライトを有効にする方法も説明します。

### <a name="continuous-autofocus"></a>連続オート フォーカス

連続オート フォーカスを有効にすると、カメラに対して、動的にフォーカスを調整して、写真やビデオの被写体にフォーカスを合わせ続けるように指示されます。 この例では、ラジオ ボタンを使用して連続オート フォーカスのオンとオフを切り替えます。

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml" id="SnippetCAFXAML":::

現在のキャプチャ デバイスで **FocusControl** がサポートされているかどうかを確認するには、[**Supported**](/uwp/api/windows.media.devices.flashcontrol.supported) プロパティを確認します。 次に、連続オート フォーカスがサポートされている場合は、[**SupportedFocusModes**](/uwp/api/windows.media.devices.focuscontrol.supportedfocusmodes) の一覧を確認して値 [**FocusMode.Continuous**](/uwp/api/Windows.Media.Devices.FocusMode) が含まれていることを確認します。この値が含まれている場合は、連続オート フォーカスのラジオ ボタンを表示します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetCAF":::

連続オート フォーカス ラジオ ボタンの [**Checked**](/uwp/api/windows.ui.xaml.controls.primitives.togglebutton.checked) イベント ハンドラーで、[**VideoDeviceController.FocusControl**](/uwp/api/windows.media.devices.videodevicecontroller.focuscontrol) プロパティを使ってコントロールのインスタンスを取得します。 アプリが以前に [**LockAsync**](/uwp/api/windows.media.devices.focuscontrol.lockasync) を呼び出して他のフォーカス モードのいずれかを有効にしていた場合は、[**UnlockAsync**](/uwp/api/windows.media.devices.focuscontrol.unlockasync) を呼び出してコントロールのロックを解除します。

新しい [**FocusSettings**](/uwp/api/Windows.Media.Devices.FocusSettings) オブジェクトを作成し、[**Mode**](/uwp/api/windows.media.devices.focussettings.mode) プロパティを **Continuous** に設定します。 [**AutoFocusRange**](/uwp/api/windows.media.devices.focussettings.autofocusrange) プロパティをアプリのシナリオに適した値またはユーザーが UI で選択した値に設定します。 **FocusSettings**オブジェクトを[**Configure**](/uwp/api/windows.media.devices.focuscontrol.configure)メソッドに渡し、 [**FocusAsync**](/uwp/api/windows.media.devices.focuscontrol.focusasync)を呼び出して継続的自動フォーカスを開始します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetCafFocusRadioButton":::

> [!IMPORTANT]
> オート フォーカス モードは、プレビュー ストリームが実行中であるときにのみサポートされます。 連続オート フォーカスをオンにする前に、プレビュー ストリームが実行されていることを確認します。

### <a name="tap-to-focus"></a>タップしてフォーカス

タップしてフォーカスの手法では、[**FocusControl**](/uwp/api/Windows.Media.Devices.FocusControl) と [**RegionsOfInterestControl**](/uwp/api/Windows.Media.Devices.RegionsOfInterestControl) を使って、キャプチャ デバイスでフォーカスを合わせるキャプチャ フレームのサブ領域を指定します。 フォーカスの領域は、プレビュー ストリームが表示されている画面をユーザーがタップすることによって決定されます。

この例では、ラジオ ボタンを使って、タップしてフォーカス モードを有効または無効にします。

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml" id="SnippetTapFocusXAML":::

現在のキャプチャ デバイスで **FocusControl** がサポートされているかどうかを確認するには、[**Supported**](/uwp/api/windows.media.devices.flashcontrol.supported) プロパティを確認します。 この手法を使うには、**RegionsOfInterestControl** がサポートされており、少なくとも 1 つの領域をサポートしている必要があります。 [**AutoFocusSupported**](/uwp/api/windows.media.devices.regionsofinterestcontrol.autofocussupported) および [**MaxRegions**](/uwp/api/windows.media.devices.regionsofinterestcontrol.maxregions) プロパティを確認して、タップしてフォーカスのラジオ ボタンを表示するか、非表示にするかを決定します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetTapFocus":::

タップしてフォーカスのラジオ ボタンの [**Checked**](/uwp/api/windows.ui.xaml.controls.primitives.togglebutton.checked) イベント ハンドラーで、[**VideoDeviceController.FocusControl**](/uwp/api/windows.media.devices.videodevicecontroller.focuscontrol) プロパティを使ってコントロールのインスタンスを取得します。 アプリが以前に [**UnlockAsync**](/uwp/api/windows.media.devices.focuscontrol.unlockasync) を呼び出して連続オート フォーカスを有効にしていた場合は、[**LockAsync**](/uwp/api/windows.media.devices.focuscontrol.lockasync) を呼び出してコントロールをロックし、ユーザーが画面をタップしてフォーカスを変更するまで待機します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetTapFocusRadioButton":::

この例は、ユーザーが画面をタップすると領域にフォーカスを合わせ、ユーザーがもう一度タップすると、トグルのように、その領域からフォーカスを削除します。 現在のトグルの状態を追跡するには、ブール変数を使います。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetIsFocused":::

次の手順では、ユーザーが画面をタップしたときのイベントをリッスンします。そのためには、現在キャプチャ プレビュー ストリームを表示している [**CaptureElement**](/uwp/api/Windows.UI.Xaml.Controls.CaptureElement) の [**Tapped**](/uwp/api/windows.ui.xaml.uielement.tapped) イベントを処理します。 カメラが現在プレビューを表示していない場合や、タップしてフォーカス モードが無効である場合は、何もせずにハンドラーから制御を戻します。

追跡変数* \_ isfocused*が false に切り替わり、カメラが現在フォーカスされていない場合 ( **FocusControl**の[**FocusState**](/uwp/api/windows.media.devices.focuscontrol.focusstate)プロパティによって決まります)、タップツーフォーカスプロセスを開始します。 ハンドラーに渡されるイベント引数から、ユーザーのタップの位置を取得します。 また、この例では、この機会を利用して、フォーカスが設定される領域のサイズを取得します。 この場合、サイズは、キャプチャ要素の最小サイズの 1/4 です。 タップの位置と領域のサイズを、次のセクションで定義される **TapToFocus** ヘルパー メソッドに渡します。

* \_ Isfocused*トグルが true に設定されている場合、ユーザータップは前の領域からフォーカスを削除する必要があります。 これは、次に示す **TapUnfocus** ヘルパー メソッドで行われます。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetTapFocusPreviewControl":::

**TapToFocus** helper メソッドでは、最初に* \_ isfocused*トグルを true に設定して、次の画面タップでタップされた領域からフォーカスが解放されるようにします。

このヘルパー メソッドの次のタスクでは、フォーカス コントロールに割り当てられるプレビュー ストリーム内の四角形を決定します。 これには 2 つのステップが必要です。 最初のステップは、[**CaptureElement**](/uwp/api/Windows.UI.Xaml.Controls.CaptureElement) コントロール内でプレビュー ストリームが占有する四角形を特定することです。 これは、プレビュー ストリームのサイズとデバイスの向きに依存します。 ヘルパー メソッド **GetPreviewStreamRectInControl** (このセクションの最後に示されている) は、このタスクを実行し、プレビュー ストリームが含まれている四角形を返します。

**TapToFocus** の次のタスクは、**CaptureElement.Tapped** イベント ハンドラーで特定された、タップの位置と目的のフォーカスの四角形のサイズを、キャプチャ ストリーム内の座標に変換することです。 **ConvertUiTapToPreviewRect** ヘルパー メソッド (このセクションで後で説明されている) は、フォーカスが要求されている四角形をキャプチャ ストリームの座標で返します。

ターゲットの四角形が取得されたら、[**Bounds**](/uwp/api/windows.media.devices.regionofinterest.bounds) プロパティを前の手順で取得されたターゲットの四角形に設定して、新しい [**RegionOfInterest**](/uwp/api/Windows.Media.Devices.RegionOfInterest) オブジェクトを作成します。

キャプチャ デバイスの [**FocusControl**](/uwp/api/Windows.Media.Devices.FocusControl) を取得します。 新しい [**FocusSettings**](/uwp/api/Windows.Media.Devices.FocusSettings) オブジェクトを作成し、**FocusControl** でサポートされていることを確認した後、[**Mode**](/uwp/api/windows.media.devices.focuscontrol.mode) と [**AutoFocusRange**](/uwp/api/windows.media.devices.focussettings.autofocusrange) を目的の値に設定します。 最後に、**FocusControl** で [**Configure**](/uwp/api/windows.media.devices.focuscontrol.configure) を呼び出して、設定をアクティブにし、指定された領域へのフォーカスを開始するようにデバイスに通知します。

次に、キャプチャ デバイスの [**RegionsOfInterestControl**](/uwp/api/Windows.Media.Devices.RegionsOfInterestControl) を取得し、[**SetRegionsAsync**](/uwp/api/windows.media.devices.regionsofinterestcontrol.setregionsasync) を呼び出してアクティブな領域を設定します。 サポートされているデバイスでは複数の対象領域を設定できますが、この例では単一の領域のみを設定します。

最後に、**FocusControl** で [**FocusAsync**](/uwp/api/windows.media.devices.focuscontrol.focusasync) を呼び出して、フォーカス設定を開始します。

> [!IMPORTANT]
> タップによるフォーカスを実装する場合、操作の順序が重要になります。 これらの API は、次の順序で呼び出す必要があります。
>
> 1. [**FocusControl.Configure**](/uwp/api/windows.media.devices.focuscontrol.configure)
> 2. [**RegionsOfInterestControl.SetRegionsAsync**](/uwp/api/windows.media.devices.regionsofinterestcontrol.setregionsasync)
> 3. [**FocusControl.FocusAsync**](/uwp/api/windows.media.devices.focuscontrol.focusasync)

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetTapToFocus":::

**TapUnfocus** ヘルパー メソッドで、**RegionsOfInterestControl** を取得し、[**ClearRegionsAsync**](/uwp/api/windows.media.devices.regionsofinterestcontrol.clearregionsasync) を呼び出して、**TapToFocus** ヘルパー メソッドでコントロールに登録された領域をクリアします。 次に、**FocusControl** を取得し、[**FocusAsync**](/uwp/api/windows.media.devices.focuscontrol.focusasync) を呼び出して、対象領域を指定せずにデバイスで再びフォーカスを設定できるようにします。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetTapUnfocus":::

**GetPreviewStreamRectInControl** ヘルパー メソッドは、プレビュー ストリームの解像度とデバイスの向きを使い、コントロールがストリームの縦横比を維持するために提供する可能性があるレターボックス化されたパディングをトリミングして、プレビュー ストリームを含むプレビュー要素内の四角形を特定します。 このメソッドは、「[MediaCapture を使った基本的な写真、ビデオ、およびオーディオのキャプチャ](basic-photo-video-and-audio-capture-with-MediaCapture.md)」に示されている基本的なメディア キャプチャのコード例で定義されているクラス メンバー変数を使います。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetGetPreviewStreamRectInControl":::

**ConvertUiTapToPreviewRect** ヘルパー メソッドは、引数として、タップ イベントの場所、目的のフォーカス領域のサイズ、**GetPreviewStreamRectInControl** ヘルパー メソッドで取得したプレビュー ストリームを含む四角形を受け取ります。 このメソッドは、これらの値とデバイスの現在の向きを使って、目的の地域を含むプレビュー ストリーム内の四角形を計算します。 ここでも、このメソッドは、「[MediaCapture を使った写真とビデオのキャプチャ](./index.md)」に示されている基本的なメディア キャプチャのコード例で定義されているクラス メンバー変数を使います。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetConvertUiTapToPreviewRect":::

### <a name="manual-focus"></a>手動フォーカス

手動フォーカスの手法では、**スライダー** コントロールを使って、キャプチャ デバイスの現在フォーカスの深度を設定します。 ラジオ ボタンを使って、手動フォーカスのオンとオフを切り替えます。

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml" id="SnippetManualFocusXAML":::

現在のキャプチャ デバイスで **FocusControl** がサポートされているかどうかを確認するには、[**Supported**](/uwp/api/windows.media.devices.focuscontrol.supported) プロパティを確認します。 コントロールがサポートされている場合は、この機能の UI を表示し、有効にすることができます。

フォーカス値は、デバイスでサポートされている範囲内である必要があり、サポートされているステップ サイズのインクリメントである必要があります。 現在のデバイスでサポートされている値を取得するには、[**Min**](/uwp/api/windows.media.devices.focuscontrol.min)、[**Max**](/uwp/api/windows.media.devices.focuscontrol.max)、および [**Step**](/uwp/api/windows.media.devices.focuscontrol.step) プロパティを確認します。これらのプロパティは、対応するスライダー コントロールのプロパティを設定するために使用されます。

値が設定されたときにイベントがトリガーされないように、[**ValueChanged**](/uwp/api/windows.ui.xaml.controls.primitives.rangebase.valuechanged) イベント ハンドラーの登録を解除した後、スライダー コントロールの値を **FocusControl** の現在値に設定します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetFocus":::

アプリが以前に [**UnlockAsync**](/uwp/api/windows.media.devices.focuscontrol.unlockasync) を呼び出してフォーカスのロックを解除していた場合は、手動フォーカスのラジオ ボタンの **Checked** イベント ハンドラーで、**FocusControl** オブジェクトを取得し、[**LockAsync**](/uwp/api/windows.media.devices.focuscontrol.lockasync) を呼び出します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetManualFocusChecked":::

手動フォーカス スライダーの **ValueChanged** イベント ハンドラーで、コントロールの現在の値を取得し、[**SetValueAsync**](/uwp/api/windows.media.devices.focuscontrol.setvalueasync) を呼び出してフォーカス値を設定します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetFocusSlider":::

### <a name="enable-the-focus-light"></a>フォーカス ライトの有効化

サポートされているデバイスで、デバイスのフォーカスを支援するフォーカス アシスト ライトを有効にすることができます。 この例では、チェック ボックスを使って、フォーカス アシスト ライトを有効または無効にします。

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml" id="SnippetFocusLightXAML":::

現在のキャプチャ デバイスで **FlashControl** がサポートされているかどうかを確認するには、[**Supported**](/uwp/api/windows.media.devices.flashcontrol.supported) プロパティを確認します。 また、[**AssistantLightSupported**](/uwp/api/windows.media.devices.flashcontrol.assistantlightsupported) を確認してアシスト ライトがサポートされていることも確認します。 これらがいずれもサポートされている場合は、この機能の UI を表示し、有効にすることができます。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetFocusLight":::

**CheckedChanged** イベント ハンドラーで、キャプチャ デバイスの [**FlashControl**](/uwp/api/Windows.Media.Devices.FlashControl) オブジェクトを取得します。 [**AssistantLightEnabled**](/uwp/api/windows.media.devices.flashcontrol.assistantlightenabled) プロパティを設定して、フォーカス ライトを有効または無効にします。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetFocusLightCheckBox":::

## <a name="iso-speed"></a>ISO 速度

[**IsoSpeedControl**](/uwp/api/Windows.Media.Devices.IsoSpeedControl) によって、写真やビデオのキャプチャ時に使用される ISO 速度を設定できます。

この例では、[**スライダー**](/uwp/api/Windows.UI.Xaml.Controls.Slider) コントロールを使って現在の露出補正値を調整し、チェック ボックスを使って自動 ISO 速度調整のオンとオフを切り替えます。

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml" id="SnippetIsoXAML":::

現在のキャプチャ デバイスで **IsoSpeedControl** がサポートされているかどうかを確認するには、[**Supported**](/uwp/api/windows.media.devices.isospeedcontrol.supported) プロパティを確認します。 コントロールがサポートされている場合は、この機能の UI を表示し、有効にすることができます。 自動 ISO 速度調整が現在アクティブであるかどうかを示すために、チェック ボックスのオンの状態を [**Auto**](/uwp/api/windows.media.devices.isospeedcontrol.auto) プロパティの値に設定します。

ISO 速度値は、デバイスでサポートされている範囲内である必要があり、サポートされているステップ サイズのインクリメントである必要があります。 現在のデバイスでサポートされている値を取得するには、[**Min**](/uwp/api/windows.media.devices.isospeedcontrol.min)、[**Max**](/uwp/api/windows.media.devices.isospeedcontrol.max)、および [**Step**](/uwp/api/windows.media.devices.isospeedcontrol.step) プロパティを確認します。これらのプロパティは、対応するスライダー コントロールのプロパティを設定するために使用されます。

値が設定されたときにイベントがトリガーされないように、[**ValueChanged**](/uwp/api/windows.ui.xaml.controls.primitives.rangebase.valuechanged) イベント ハンドラーの登録を解除した後、スライダー コントロールの値を **IsoSpeedControl** の現在値に設定します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetIsoControl":::

**ValueChanged** イベント ハンドラーで、コントロールの現在の値を取得し、[**SetValueAsync**](/uwp/api/windows.media.devices.isospeedcontrol.setvalueasync) を呼び出して ISO 速度を設定します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetIsoSlider":::

自動 ISO 速度チェック ボックスの **CheckedChanged** イベント ハンドラーで、[**SetAutoAsync**](/uwp/api/windows.media.devices.isospeedcontrol.setautoasync) を呼び出すことによって、自動 ISO 速度調整をオンにします。 自動 ISO 速度調整をオフにするには、[**SetValueAsync**](/uwp/api/windows.media.devices.isospeedcontrol.setvalueasync) を呼び出して、スライダー コントロールの現在の値を渡します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetIsoCheckBox":::

## <a name="optical-image-stabilization"></a>光学式手ブレ補正

光学式手ブレ補正 (OIS) は、ハードウェア キャプチャ デバイスを物理的に操作することで、キャプチャしたビデオ ストリームを補正します。これにより、デジタル補正より優れた結果が生まれます。 OIS がサポートされていないデバイスでは、VideoStabilizationEffect を使用して、キャプチャしたビデオにデジタル手ブレ補正を行うことができます。 詳しくは、「[ビデオ キャプチャの効果](effects-for-video-capture.md)」をご覧ください。

現在のデバイスで OIS がサポートされているかどうかを確認するには、[**OpticalImageStabilizationControl.Supported**](/uwp/api/windows.media.devices.opticalimagestabilizationcontrol.supported) プロパティをチェックします。

OIS コントロールでは、3 つのモード (オン、オフ、自動) がサポートされています。自動モードでは、OIS によってメディア キャプチャが改善されるかどうかをデバイスが動的に判断し、改善される場合は OIS が有効になります。 現在のデバイスで特定のモードがサポートされているかどうかを確認するには、[**OpticalImageStabilizationControl.SupportedModes**](/uwp/api/windows.media.devices.opticalimagestabilizationcontrol.supportedmodes) コレクションに目的のモードが含まれているかどうかをチェックします。

OIS を有効または無効にするには、[**OpticalImageStabilizationControl.Mode**](/uwp/api/Windows.Media.Devices.OpticalImageStabilizationMode) を目的のモードに設定します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetSetOpticalImageStabilizationMode":::

## <a name="powerline-frequency"></a>電源周波数
一部のカメラ デバイスでは、現在の環境の AC 電源周波数を認識し、それに応じたアンチフリッカー処理をサポートします。 電源周波数の自動認識をサポートするデバイスもあれば、周波数を手動で設定する必要があるデバイスもあります。 次のコード例は、デバイスの電源周波数のサポートを判別し、必要に応じて、周波数を手動で設定する方法を示します。 

まず [**PowerlineFrequency**](/uwp/api/Windows.Media.Capture.PowerlineFrequency) 型の出力パラメーターを渡して **VideoDeviceController** メソッド [**TryGetPowerlineFrequency**](/uwp/api/windows.media.devices.videodevicecontroller.trygetpowerlinefrequency) を呼び出します。この呼び出しが失敗した場合、現在のデバイスでは電源周波数の制御がサポートされていません。 機能がサポートされている場合、自動モードの設定を試みることで、自動モードがサポートされているかどうかを確認できます。 これを行うには、 [**TrySetPowerlineFrequency**](/uwp/api/windows.media.devices.videodevicecontroller.trysetpowerlinefrequency) を呼び出し、値 **Auto**を渡します。呼び出しが成功した場合は、自動 powerline の周波数がサポートされていることを意味します。 デバイスで電源周波数の制御はサポートされているが、周波数の自動検出はサポートされていない場合、**TrySetPowerlineFrequency** を使って周波数を手動で設定できます。 次の例で、**MyCustomFrequencyLookup** は、デバイスの現在の場所における正しい周波数を判定するために実装するカスタム メソッドです。 

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetPowerlineFrequency":::

## <a name="white-balance"></a>ホワイト バランス

[**WhiteBalanceControl**](/uwp/api/windows.media.devices.videodevicecontroller.whitebalancecontrol) によって、写真やビデオのキャプチャ時に使用されるホワイト バランスを設定できます。

この例では、[**ComboBox**](/uwp/api/Windows.UI.Xaml.Controls.ComboBox) コントロールを使って組み込みの色温度のプリセットを選択し、[**スライダー**](/uwp/api/Windows.UI.Xaml.Controls.Slider) コントロールを使って手動でホワイト バランスを調整します。

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml" id="SnippetWhiteBalanceXAML":::

現在のキャプチャ デバイスで **WhiteBalanceControl** がサポートされているかどうかを確認するには、[**Supported**](/uwp/api/windows.media.devices.whitebalancecontrol.supported) プロパティを確認します。 コントロールがサポートされている場合は、この機能の UI を表示し、有効にすることができます。 コンボ ボックスの項目を、[**ColorTemperaturePreset**](/uwp/api/Windows.Media.Devices.ColorTemperaturePreset) 列挙体の値に設定します。 また、選ばれた項目を、[**Preset**](/uwp/api/windows.media.devices.whitebalancecontrol.preset) プロパティの現在の値に設定します。

手動コントロールの場合、ホワイト バランス値は、デバイスでサポートされている範囲内である必要があり、サポートされているステップ サイズのインクリメントである必要があります。 現在のデバイスでサポートされている値を取得するには、[**Min**](/uwp/api/windows.media.devices.whitebalancecontrol.min)、[**Max**](/uwp/api/windows.media.devices.whitebalancecontrol.max)、および [**Step**](/uwp/api/windows.media.devices.whitebalancecontrol.step) プロパティを確認します。これらのプロパティは、対応するスライダー コントロールのプロパティを設定するために使用されます。 手動コントロールを有効にする前に、サポートされている最小値と最大値の間の範囲がステップ サイズよりも大きいことを確認します。 このようになっていない場合、手動コントロールは、現在のデバイスではサポートされません。

値が設定されたときにイベントがトリガーされないように、[**ValueChanged**](/uwp/api/windows.ui.xaml.controls.primitives.rangebase.valuechanged) イベント ハンドラーの登録を解除した後、スライダー コントロールの値を **WhiteBalanceControl** の現在値に設定します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetWhiteBalance":::

色温度プリセットのコンボ ボックスの [**SelectionChanged**](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) イベント ハンドラーで、現在選択されているプリセットを取得し、[**SetPresetAsync**](/uwp/api/windows.media.devices.whitebalancecontrol.setpresetasync) を呼び出してコントロールの値を設定します。 選択されたプリセット値が **Manual** でない場合は、手動のホワイト バランス スライダーを無効にします。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetWhiteBalanceComboBox":::

**ValueChanged** イベント ハンドラーで、コントロールの現在の値を取得し、[**SetValueAsync**](/uwp/api/windows.media.devices.exposurecontrol.setvalueasync) を呼び出してホワイト バランス値を設定します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetWhiteBalanceSlider":::

> [!IMPORTANT]
> ホワイト バランスの調整は、プレビュー ストリームが実行中であるときにのみサポートされます。 ホワイト バランス値またはプリセットを設定する前に、プレビュー ストリームが実行されていることを確認します。

> [!IMPORTANT]
> **ColorTemperaturePreset.Auto** プリセット値は、ホワイト バランス レベルを自動調整するようにシステムに指示します。 ホワイト バランス レベルが各フレームで同じである必要がある写真シーケンスのキャプチャなど、一部のシナリオでは、現在の自動値にコントロールをロックすることもできます。 これを行うには、[**SetPresetAsync**](/uwp/api/windows.media.devices.whitebalancecontrol.setpresetasync) を呼び出して、**Manual** プリセットを指定します。[**SetValueAsync**](/uwp/api/windows.media.devices.whitebalancecontrol.setvalueasync) を使って、コントロールの値を設定しないでください。 これにより、デバイスは現在の値にロックされます。 現在のコントロールの値を読み取って、返された値を **SetValueAsync** に渡すことはしないでください。この値が正しいことは保証されないためです。

## <a name="zoom"></a>Zoom

[**ZoomControl**](/uwp/api/Windows.Media.Devices.ZoomControl) によって、写真やビデオのキャプチャ時に使用されるズーム レベルを設定できます。

この例では、[**スライダー**](/uwp/api/Windows.UI.Xaml.Controls.Slider) コントロールを使って、現在のズーム レベルを調整します。 次のセクションでは、画面上のピンチ ジェスチャに基づいてズームを調整する方法を示します。

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml" id="SnippetZoomXAML":::

現在のキャプチャ デバイスで **ZoomControl** がサポートされているかどうかを確認するには、[**Supported**](/uwp/api/windows.media.devices.zoomcontrol.supported) プロパティを確認します。 コントロールがサポートされている場合は、この機能の UI を表示し、有効にすることができます。

ズーム レベル値は、デバイスでサポートされている範囲内である必要があり、サポートされているステップ サイズのインクリメントである必要があります。 現在のデバイスでサポートされている値を取得するには、[**Min**](/uwp/api/windows.media.devices.zoomcontrol.min)、[**Max**](/uwp/api/windows.media.devices.zoomcontrol.max)、および [**Step**](/uwp/api/windows.media.devices.zoomcontrol.step) プロパティを確認します。これらのプロパティは、対応するスライダー コントロールのプロパティを設定するために使用されます。

値が設定されたときにイベントがトリガーされないように、[**ValueChanged**](/uwp/api/windows.ui.xaml.controls.primitives.rangebase.valuechanged) イベント ハンドラーの登録を解除した後、スライダー コントロールの値を **ZoomControl** の現在値に設定します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetZoomControl":::

**ValueChanged** イベント ハンドラーで、[**Value**](/uwp/api/windows.media.devices.zoomsettings.value) プロパティをズーム スライダー コントロールの現在の値に設定して、[**ZoomSettings**](/uwp/api/Windows.Media.Devices.ZoomSettings) クラスの新しいインスタンスを作成します。 **ZoomControl** の [**SupportedModes**](/uwp/api/windows.media.devices.zoomcontrol.supportedmodes) プロパティに [**ZoomTransitionMode.Smooth**](/uwp/api/Windows.Media.Devices.ZoomTransitionMode) が含まれている場合、デバイスがズーム レベルのスムーズな切り替えをサポートしていることを意味します。 このモードではユーザー エクスペリエンスが向上するため、通常、**ZoomSettings** オブジェクトの [**Mode**](/uwp/api/windows.media.devices.zoomsettings.mode) プロパティにはこの値を使います。

最後に、**ZoomSettings** オブジェクトを、**ZoomControl** オブジェクトの [**Configure**](/uwp/api/windows.media.devices.zoomcontrol.configure) メソッドに渡すことによって、現在のズーム設定を変更します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetZoomSlider":::

### <a name="smooth-zoom-using-pinch-gesture"></a>ピンチ ジェスチャを使ったスムーズなズーム

前のセクションで説明したように、スムーズ ズームがサポートされているキャプチャ デバイスでスムーズ ズーム モードを使用すると、異なるデジタル ズーム レベル間での切り替えがスムーズになります。これによりユーザーは、キャプチャ操作中にズーム レベルを動的に調整することができます。切り替えを個々に行う必要がなく、不快感もありません。 このセクションでは、ピンチ ジェスチャに応答してズーム レベルを調整する方法について説明します。

まず、[**ZoomControl.Supported**](/uwp/api/windows.media.devices.zoomcontrol.supported) プロパティをチェックして、現在のデバイスでデジタル ズーム コントロールがサポートされているかどうかを確認します。 次に、[**ZoomControl.SupportedModes**](/uwp/api/windows.media.devices.zoomcontrol.supportedmodes) の値として [**ZoomTransitionMode.Smooth**](/uwp/api/Windows.Media.Devices.ZoomTransitionMode) が含まれているかどうかをチェックすることによって、スムーズ ズーム モードが利用可能かどうかを確認します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetIsSmoothZoomSupported":::

マルチタッチ操作対応デバイスでは、2 本指でのピンチ ジェスチャに基づいてズーム倍率を調整するのが一般的です。 ピンチ ジェスチャを有効にするには、[**CaptureElement**](/uwp/api/Windows.UI.Xaml.Controls.CaptureElement) コントロールの [**ManipulationMode**](/uwp/api/windows.ui.xaml.uielement.manipulationmode) プロパティを [**ManipulationModes.Scale**](/uwp/api/Windows.UI.Xaml.Input.ManipulationModes) に設定します。 次に、ピンチ ジェスチャでサイズ変更されたときに発生する [**ManipulationDelta**](/uwp/api/windows.ui.xaml.uielement.manipulationdelta) イベントを登録します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetRegisterPinchGestureHandler":::

**ManipulationDelta** イベント用のハンドラーでは、ユーザーのピンチ ジェスチャの変化に基づいてズーム倍率を更新します。 [**ManipulationDelta.Scale**](/uwp/api/Windows.UI.Input.ManipulationDelta) 値は、ピンチ ジェスチャによるズーム倍率の変化を表します。たとえば、ピンチ サイズがわずかに大きくなった場合は 1.0 よりわずかに大きい数値、ピンチ サイズがわずかに小さくなった場合は 1.0 よりわずかに小さい数値になります。 この例では、ズーム コントロールの現在の値にスケール デルタを掛けています。

ズーム倍率を設定する前に、デバイスでサポートされている ([**ZoomControl.Min**](/uwp/api/windows.media.devices.zoomcontrol.min) プロパティで示されている) 最小値より小さい値になっていないか確認する必要があります。 また、値が [**ZoomControl.Max**](/uwp/api/windows.media.devices.zoomcontrol.max) 値以下であることを確認します。 最後に、[ [**ステップ**](/uwp/api/windows.media.devices.zoomcontrol.step) ] プロパティで示されているように、デバイスでサポートされているズームステップのサイズの倍数であることを確認する必要があります。 ズーム倍率がこれらの要件を満たしていない場合は、キャプチャ デバイスでズーム レベルを設定しようとすると、例外がスローされます。

キャプチャ デバイスでズーム レベルを設定するには、新しい [**ZoomSettings**](/uwp/api/Windows.Media.Devices.ZoomSettings) オブジェクトを作成します。 [**Mode**](/uwp/api/windows.media.devices.zoomsettings.mode) プロパティを [**ZoomTransitionMode.Smooth**](/uwp/api/Windows.Media.Devices.ZoomTransitionMode) に設定し、[**Value**](/uwp/api/windows.media.devices.zoomsettings.value) プロパティを目的のズーム倍率に設定します。 最後に、[**ZoomControl.Configure**](/uwp/api/windows.media.devices.zoomcontrol.configure) を呼び出して、デバイスの新しいズーム値を設定します。 デバイスは新しいズーム値への切り替えをスムーズに行います。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetManipulationDelta":::

## <a name="related-topics"></a>関連トピック

* [カメラ](camera.md)
* [MediaCapture を使った基本的な写真、ビデオ、およびオーディオのキャプチャ](basic-photo-video-and-audio-capture-with-MediaCapture.md)
