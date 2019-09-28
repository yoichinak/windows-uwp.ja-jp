---
ms.assetid: 9BA3F85A-970F-411C-ACB1-B65768B8548A
description: この記事では、ユニバーサル Windows プラットフォーム (UWP) アプリで XAML ページ内にカメラ プレビュー ストリームをすばやく表示する方法について説明します。
title: カメラ プレビューの表示
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1f35cbab511912bd9cf6616330f3e9e7737189fd
ms.sourcegitcommit: a20457776064c95a74804f519993f36b87df911e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/27/2019
ms.locfileid: "71339741"
---
# <a name="display-the-camera-preview"></a>カメラ プレビューの表示


この記事では、ユニバーサル Windows プラットフォーム (UWP) アプリで XAML ページ内にカメラ プレビュー ストリームをすばやく表示する方法について説明します。 カメラを使って写真やビデオをキャプチャするアプリを作成するには、デバイスとカメラの向きの処理や、キャプチャされたファイルのエンコーディング オプションの設定などのタスクを実行する必要があります。 アプリ シナリオによっては、このような他の考慮事項について考えなくても、カメラからプレビュー ストリームをそのまま表示することができます。 この記事では、最小限のコードでそれを行う方法を示します。 プレビュー ストリームの操作が完了したら、必ず以下の手順に従ってプレビュー ストリームを正しくシャットダウンする必要がある点に注意してください。

写真やビデオをキャプチャするカメラ アプリの作成方法について詳しくは、「[MediaCapture を使った基本的な写真、ビデオ、およびオーディオのキャプチャ](basic-photo-video-and-audio-capture-with-MediaCapture.md)」をご覧ください。

## <a name="add-capability-declarations-to-the-app-manifest"></a>アプリ マニフェストに機能宣言を追加する

アプリからデバイスのカメラにアクセスするには、アプリでデバイス機能 (*webcam* と *microphone*) の使用を宣言する必要があります。 

**アプリマニフェストに機能を追加する**

1.  Microsoft Visual Studio の**ソリューション エクスプローラー**で、**package.appxmanifest** 項目をダブルクリックしてアプリケーション マニフェストのデザイナーを開きます。
2.  **[機能]** タブをクリックします。
3.  **[Web カメラ]** のボックスと **[マイク]** のボックスをオンにします。

## <a name="add-a-captureelement-to-your-page"></a>ページに CaptureElement コントロールを追加する

[  **CaptureElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CaptureElement) を使って、XAML ページ内にプレビュー ストリームを表示します。

[!code-xml[CaptureElement](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml#SnippetCaptureElement)]



## <a name="use-mediacapture-to-start-the-preview-stream"></a>MediaCapture を使ってプレビュー ストリームを開始する

[  **MediaCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture) オブジェクトは、デバイスのカメラに対するアプリのインターフェイスです。 このクラスは、Windows.Media.Capture 名前空間のメンバーです。 この記事の例では、既定のプロジェクト テンプレートに含まれている API に加えて、[**Windows.ApplicationModel**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel) 名前空間と [System.Threading.Tasks](https://docs.microsoft.com/dotnet/api/system.threading.tasks) 名前空間の API も使われます。

ページの .cs ファイルに次の名前空間を含めるには using ディレクティブを追加します。

[!code-cs[SimpleCameraPreviewUsing](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetSimpleCameraPreviewUsing)]

**MediaCapture** オブジェクトのクラス メンバー変数と、カメラが現在プレビューを表示しているかどうかを追跡するブール値を宣言します。 

[!code-cs[DeclareMediaCapture](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetDeclareMediaCapture)]

プレビューの実行中にディスプレイがオフになっていないことを確認するために使用する、[**DisplayRequest**](https://docs.microsoft.com/uwp/api/Windows.System.Display.DisplayRequest) 型の変数を宣言します。

[!code-cs[DeclareDisplayRequest](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetDeclareDisplayRequest)]

カメラのプレビューを起動するヘルパー メソッド (この例では **StartPreviewAsync**) を作成します。 アプリのシナリオによって、ページが読み込まれるときに呼び出される **OnNavigatedTo** イベント ハンドラーからこれを呼び出すことも、UI イベントへの応答でプレビューを待機して起動することもできます。

**MediaCapture** クラスの新しいインスタンスを作成し、[**InitializeAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.initializeasync) を呼び出してキャプチャ デバイスを初期化します。 カメラがないデバイスなどではこのメソッドが失敗することがあるため、**try** ブロック内から呼び出してください。 ユーザーがデバイスのプライバシー設定でカメラへのアクセスを無効にしている場合、カメラを初期化しようとすると **UnauthorizedAccessException** がスローされます。 この例外は、開発中、アプリ マニフェストに適切な機能を追加し忘れた場合も表示されます。

**重要:** 一部のデバイス ファミリでは、アプリがデバイスのカメラへのアクセスを付与される前に、ユーザー同意のプロンプトがユーザーに表示されます。 このため、[**MediaCapture.InitializeAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.initializeasync) のみをメイン UI スレッドから呼び出す必要があります。 別のスレッドからカメラを初期化しようとすると、初期化エラーになる可能性があります。

[  **Source**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.captureelement.source) プロパティを設定して、**MediaCapture** を **CaptureElement** に接続します。 [  **StartPreviewAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.startpreviewasync) を呼び出してプレビューを開始します。 別のアプリがキャプチャ デバイスを排他的に制御している場合、このメソッドは **FileLoadException** をスローします。 排他的制御での変更をリッスンについては、次のセクションを参照してください。

[  **RequestActive**](https://docs.microsoft.com/uwp/api/windows.system.display.displayrequest.requestactive) を呼び出して、プレビューの実行中にデバイスがスリープ状態にならないことを確認します。 最後に、[**DisplayInformation.AutoRotationPreferences**](https://docs.microsoft.com/uwp/api/windows.graphics.display.displayinformation.autorotationpreferences) プロパティを [**Landscape**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Display.DisplayOrientations) に設定して、ユーザーがデバイスの向きを変更したときに UI と **CaptureElement** が回転することを防ぎます。 デバイスの向きの変更処理について詳しくは、「[**MediaCapture を使ってデバイスの向きを処理する**](handle-device-orientation-with-mediacapture.md)」をご覧ください。  

[!code-cs[StartPreviewAsync](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetStartPreviewAsync)]

## <a name="handle-changes-in-exclusive-control"></a>排他的制御での変更を処理する
前のセクションで説明したように、別のアプリがキャプチャ デバイスを排他的に制御している場合、**StartPreviewAsync** は **FileLoadException** をスローします。 Windows 10 Version 1703 以降では、デバイスの排他的制御の状態が変化するたびに発生する [MediaCapture.CaptureDeviceExclusiveControlStatusChanged](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture.CaptureDeviceExclusiveControlStatusChanged) イベントのハンドラーを登録できます。 このイベントのハンドラーで、[MediaCaptureDeviceExclusiveControlStatusChangedEventArgs.Status](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapturedeviceexclusivecontrolstatuschangedeventargs.Status) プロパティを調べて、現在の状態を確認します。 新しい状態が **SharedReadOnlyAvailable** である場合、現在プレビューを開始できないことがわかり、UI を更新してユーザーに警告することができます。 新しい状態が **ExclusiveControlAvailable** である場合は、カメラのプレビューを再試行することができます。

[!code-cs[ExclusiveControlStatusChanged](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetExclusiveControlStatusChanged)]

## <a name="shut-down-the-preview-stream"></a>プレビュー ストリームをシャットダウンする

プレビュー ストリームを使い終わったら、必ずストリームをシャットダウンして関連するリソースを適切に破棄し、デバイスで他のアプリがカメラを使うことができるようにしてください。 プレビュー ストリームをシャットダウンするために必要な手順は次のとおりです。

-   現在、カメラがプレビューを表示中の場合は、[**StopPreviewAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.stoppreviewasync) を呼び出してプレビュー ストリームを停止します。 プレビューが実行されていないときに **StopPreviewAsync** を呼び出すと、例外がスローされます。
-   **CaptureElement** の [**Source**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.captureelement.source) プロパティを null に設定します。 [  **CoreDispatcher.RunAsync**](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.runasync) を使用して、この呼び出しが UI スレッドで実行されることを確認します。
-   **MediaCapture** オブジェクトの [**Dispose**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.dispose) メソッドを呼び出してオブジェクトを解放します。 再度、[**CoreDispatcher.RunAsync**](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.runasync) を使用して、この呼び出しが UI スレッドで実行されることを確認します。
-   **MediaCapture** メンバー変数を null に設定します。
-   [  **RequestRelease**](https://docs.microsoft.com/uwp/api/windows.system.display.displayrequest.requestrelease) を呼び出して、アクティブでないときに画面をオフにできるようにします。

[!code-cs[CleanupCameraAsync](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetCleanupCameraAsync)]

[  **OnNavigatedFrom**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page.onnavigatedfrom) メソッドをオーバーライドすることで、ユーザーがページから離れるときにプレビュー ストリームをシャットダウンする必要があります。

[!code-cs[OnNavigatedFrom](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetOnNavigatedFrom)]

アプリの中断中もプレビュー ストリームを適切にシャットダウンする必要があります。 これを行うには、ページのコンストラクターで [**Application.Suspending**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.suspending) イベントのハンドラーを登録します。

[!code-cs[RegisterSuspending](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetRegisterSuspending)]

**Suspending** イベント ハンドラーで、まずページの種類と [**CurrentSourcePageType**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.currentsourcepagetype) プロパティを比較することで、アプリケーションの [**Frame**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame) にページが表示されることを確認します。 ページが現在表示されていない場合、**OnNavigatedFrom** イベントが既に発生しており、プレビュー ストリームはシャットダウンしています。 現在ページが表示されている場合、ハンドラーに渡されるイベント引数から [**SuspendingDeferral**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.SuspendingDeferral) オブジェクトを取得し、プレビュー ストリームがシャットダウンするまでシステムがアプリを中断しないことを確認します。 ストリームをシャットダウンした後、保留の [**Complete**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.suspendingdeferral.complete) メソッドを呼び出し、システムがアプリの中断を続行できるようにします。

[!code-cs[SuspendingHandler](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetSuspendingHandler)]


## <a name="related-topics"></a>関連トピック

* [カメラ](camera.md)
* [MediaCapture を使用した基本的な写真、ビデオ、オーディオキャプチャ](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [プレビューフレームを取得する](get-a-preview-frame.md)
