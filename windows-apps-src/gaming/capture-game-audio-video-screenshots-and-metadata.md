---
ms.assetid: ''
description: ゲームのビデオ、オーディオ、スクリーンショットをキャプチャする方法、およびシステムによってキャプチャされて放送メディアに埋め込まれるメタデータを送信する方法について説明します。
title: ゲームのオーディオ、ビデオ、スクリーンショット、メタデータのキャプチャ
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, ゲーム, キャプチャ, オーディオ, ビデオ, メタデータ
ms.localizationpriority: medium
ms.openlocfilehash: ea01139d4945d1e1b7e9a49078cd93725388e946
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/02/2020
ms.locfileid: "89364145"
---
# <a name="capture-game-audio-video-screenshots-and-metadata"></a>ゲームのオーディオ、ビデオ、スクリーンショット、メタデータのキャプチャ
この記事では、ゲームのビデオ、オーディオ、スクリーン ショットをキャプチャする方法や、キャプチャおよびブロードキャストされるメディアにシステムが埋め込むメタデータを送信して、アプリや他のユーザーがゲームプレイのイベントに同期する動的なエクスペリエンスを作成できるようにする方法について説明します。 

UWP アプリでゲームプレイをキャプチャするには、2 つの方法があります。 ユーザーは、組み込みのシステム UI を使用してキャプチャを開始できます。 この手法を使用してキャプチャされたメディアは、Microsoft ゲームエコシステムに取り込まれたされ、Xbox アプリなどのファーストパーティエクスペリエンスで表示したり共有したりすることができ、アプリやユーザーに直接提供されることはありません。 この記事の最初のセクションでは、システムに実装されたアプリのキャプチャを有効または無効にする方法と、アプリのキャプチャが開始または停止されたときに通知を受信する方法を示します。

メディアをキャプチャするもう 1 つの方法は、**[Windows.Media.AppRecording](/uwp/api/windows.media.apprecording)** 名前空間の API を使用する方法です。 デバイスでキャプチャが有効になっている場合、アプリはゲームプレイのキャプチャを開始し、しばらく時間が経過した後、キャプチャを停止できます。その時点で、メディアはファイルに書き込まれます。 ユーザーが履歴のキャプチャを有効にしている場合、過去の開始時刻と記録の継続時間を指定することによって、既に発生したゲームプレイも記録できます。 これらのいずれの手法でも、アプリでアクセスできるビデオ ファイルが生成されます。また、ファイルを保存するために選択した場所によっては、ユーザーがアクセスできるビデオ ファイルが生成されます。 この記事の中ほどのセクションでは、これらのシナリオを実装する手順について説明します。

**[Windows.Media.Capture](/uwp/api/windows.media.capture)** 名前空間は、キャプチャまたはブロードキャストされるゲームプレイを説明するメタデータを作成するための API を提供します。 これには、テキストや数値を、各データ項目を識別するテキスト ラベルと共に含めることができます。 メタデータは、1 つの時点で発生する "イベント" (ユーザーがレーシング ゲームでコースを走り終えたときなど) を表すことができます。また、一定期間保持される "状態" (ユーザーがプレイしている現在のゲーム マップなど) を表すこともできます。 メタデータは、システムによってアプリ用に割り当てられて管理されるキャッシュに書き込まれます。 メタデータは、組み込みのシステム キャプチャとカスタム アプリによるキャプチャのいずれの手法でも、ブロードキャスト ストリームやキャプチャしたビデオ ファイルに埋め込まれます。 この記事の最後のセクションでは、ゲームプレイのメタデータを書き込む方法について説明します。

> [!NOTE] 
> ゲームプレイのメタデータは、潜在的に、ネットワーク上でユーザーの制御の範囲外で共有できるメディア ファイルに埋め込まれる可能性があるため、個人を特定できる情報やその他の潜在的な機密データをメタデータに含めないでください。


## <a name="enable-and-disable-system-app-capture"></a>システムによるアプリのキャプチャを有効または無効にする
システムによるアプリのキャプチャは、ユーザーによって、組み込みのシステム UI を使用して開始されます。 これらのファイルは、Windows ゲームエコシステムによって取り込まれたされており、アプリやユーザーは使用できません。ただし、Xbox アプリのようなファーストパーティエクスペリエンスは除きます。 アプリでは、システムによって開始されるアプリのキャプチャを無効または有効にすることができ、ユーザーが特定のコンテンツやゲームプレイをキャプチャできないように設定できます。 

システムによるアプリのキャプチャを有効または無効にするには、静的メソッド **[AppCapture.SetAllowedAsync](/uwp/api/windows.media.capture.appcapture.setallowedasync)** を呼び出して、キャプチャを無効にする場合は **false** を、キャプチャを有効にする場合は **true** を渡すだけです。

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetSetAppCaptureAllowed":::


## <a name="receive-notifications-when-system-app-capture-starts-and-stops"></a>システムによるアプリのキャプチャが開始および停止したときに通知を受信する
システムによるアプリのキャプチャが開始または終了されたときに通知を受信するには、最初に、ファクトリ メソッド **[GetForCurrentView](/uwp/api/windows.media.capture.appcapture.GetForCurrentView)** を呼び出すことによって、**[AppCapture](/uwp/api/windows.media.capture.appcapture)** クラスのインスタンスを取得します。 次に、**[CapturingChanged](/uwp/api/windows.media.capture.appcapture.CapturingChanged)** イベントのハンドラーを登録します。

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetRegisterCapturingChanged":::

**CapturingChanged** イベントのハンドラーで、**[IsCapturingAudio](/uwp/api/windows.media.capture.appcapture.IsCapturingAudio)** プロパティと **[IsCapturingVideo](/uwp/api/windows.media.capture.appcapture.IsCapturingVideo)** プロパティを調べて、それぞれオーディオまたはビデオがキャプチャされているかどうかを確認できます。 現在のキャプチャの状態を示すために、アプリの UI を更新することが必要になる場合があります。

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetOnCapturingChanged":::

## <a name="add-the-windows-desktop-extensions-for-the-uwp-to-your-app"></a>UWP 用の Windows デスクトップ拡張機能をアプリに追加する
アプリから直接、オーディオとビデオを記録するための API やスクリーンショットをキャプチャするための API は、**[Windows.Media.AppRecording](/uwp/api/windows.media.apprecording)** 名前空間にあり、ユニバーサル API コントラクトには含まれていません。 この API にアクセスするには、次の手順に従って、UWP 用の Windows デスクトップ拡張機能への参照をアプリに追加する必要があります。

1. Visual Studio の**ソリューション エクスプローラー**で、UWP プロジェクトを展開し、**[参照]** を右クリックして、**[参照の追加]** を選択します。 
2. **[ユニバーサル Windows]** ノードを展開し、**[拡張機能]** を選択します。
3. 拡張機能の一覧で、プロジェクトのターゲット ビルドに一致する **[Windows Desktop Extensions for the UWP]** エントリの横にあるチェック ボックスをオンにします。 アプリのブロードキャスト機能を使用するには、バージョンが 1709 以上である必要があります。
4. **[OK]** をクリックします。

## <a name="get-an-instance-of-apprecordingmanager"></a>AppRecordingManager のインスタンスを取得する
**[AppRecordingManager](/uwp/api/windows.media.apprecording.apprecordingmanager)** クラスは、アプリの記録を管理するために使用する中心的な API です。 ファクトリ メソッド **[GetDefault](/uwp/api/windows.media.apprecording.apprecordingmanager.GetDefault)** を呼び出すことによって、このクラスのインスタンスを取得します。 **Windows.Media.AppRecording**名前空間の API のいずれかを使用する前に、現在のデバイスでこれらが存在することを確認する必要があります。 この API は、Windows 10 バージョン 1709 より前のバージョンの OS を実行しているデバイスでは利用できません。 特定の OS バージョンを確認するのではなく、**[ApiInformation.IsApiContractPresent](/uwp/api/windows.foundation.metadata.apiinformation.isapicontractpresent)** メソッドで、*Windows.Media.AppBroadcasting.AppRecordingContract* バージョン 1.0 を照会します。 このコントラクトが存在する場合は、デバイスで記録 API を利用できます。 この記事のコード例では、API を 1 回確認し、それ以降の操作の前に、**AppRecordingManager** が null であるかどうかを確認します。

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetGetAppRecordingManager":::

## <a name="determine-if-your-app-can-currently-record"></a>アプリが現在記録できるかどうかを確認する
現在、アプリでオーディオやビデオをキャプチャできない場合は、いくつかの原因があります。たとえば、現在のデバイスが記録のハードウェア要件を満たしていない場合や、別のアプリが現在ブロードキャストしている場合です。 記録を開始する前に、アプリが現在記録できるかどうかを確認できます。 **AppRecordingManager** オブジェクトの **[GetStatus](/uwp/api/windows.media.apprecording.apprecordingmanager.GetStatus)** メソッドを呼び出して、返された **[AppRecordingStatus](/uwp/api/windows.media.apprecording.apprecordingstatus)** オブジェクトの **[CanRecord](/uwp/api/windows.media.apprecording.apprecordingstatus.CanRecord)** プロパティを確認します。 **Canrecord**が**false**を返した場合、アプリで現在レコードが記録されていないことを意味します。**[詳細](/uwp/api/windows.media.apprecording.apprecordingstatus.Details)** プロパティを確認して、理由を特定できます。 理由に応じて、ユーザーに対してステータスを表示したり、アプリの記録を有効にするための手順を示したりすることができます。



:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetCanRecord":::

## <a name="manually-start-and-stop-recording-your-app-to-a-file"></a>ファイルへのアプリの記録を手動で開始および停止する

アプリが記録できることを確認した後、**AppRecordingManager** オブジェクトの **[StartRecordingToFileAsync](/uwp/api/windows.media.apprecording.apprecordingmanager.startrecordingtofileasync)** メソッドを呼び出すことによって、新しい記録を開始することができます。

次の例で、最初の **then** ブロックは、非同期タスクが失敗したときに実行されます。 2 番目の **then** ブロックは、タスクの結果へのアクセスを試行します。結果が null の場合、タスクは完了しています。 いずれの場合も、次に示す **OnRecordingComplete** ヘルパー メソッドが呼び出されて、結果が処理されます。 

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetStartRecordToFile":::

記録操作が完了したら、返された **[AppRecordingResult](/uwp/api/windows.media.apprecording.apprecordingresult)** オブジェクトの **[Succeeded](/uwp/api/windows.media.apprecording.apprecordingresult.Succeeded)** プロパティを確認して、記録操作が成功したかどうかを判断します。 成功している場合、**[IsFileTruncated](/uwp/api/windows.media.apprecording.apprecordingresult.IsFileTruncated)** プロパティを確認して、記憶域上の理由から、システムがキャプチャされたファイルを強制的に切り詰めたかどうかを判断できます。 **[Duration](/uwp/api/windows.media.apprecording.apprecordingresult.Duration)** プロパティを確認して、記録されたファイルの実際の継続時間を検出できます。ファイルが切り詰められている場合、実際の継続時間は記録操作の継続時間よりも短い場合があります。

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetOnRecordingComplete":::

次の例では、前の例で示した記録操作を開始および停止するためのいくつかの基本的なコードを示します。

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetCallStartRecordToFile":::

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetFinishRecordToFile":::

## <a name="record-a-historical-time-span-to-a-file"></a>履歴の期間をファイルに記録する
ユーザーが、システムの設定でアプリの履歴の記録を有効にしている場合、既に経過したゲームプレイの期間を記録できます。 この記事の前の例では、アプリが現在ゲームプレイを記録できることを確認する方法を示しました。 履歴のキャプチャが有効になっているかどうかを判断するには、追加の確認を行います。 もう一度 **[GetStatus](/uwp/api/windows.media.apprecording.apprecordingmanager.GetStatus)** を呼び出して、返された **AppRecordingStatus** オブジェクトの **[CanRecordTimeSpan](/uwp/api/windows.media.apprecording.apprecordingstatus.CanRecordTimeSpan)** プロパティを確認します。 この例では、**AppRecordingStatus** の **[HistoricalBufferDuration](/uwp/api/windows.media.apprecording.apprecordingstatus.HistoricalBufferDuration)** プロパティも返されます。このプロパティは、記録操作の有効な開始時刻を確認するために使用されます。

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetCanRecordTimeSpan":::

履歴の期間をキャプチャするには、記録の開始時刻と継続時間を指定する必要があります。 開始時刻は、**[DateTime](/uwp/api/windows.foundation.datetime)** 構造体として提供されます。 開始時刻は、記録バッファーの長さの範囲内で、現在の時刻よりも前の時刻である必要があります。 この例では、バッファーの長さは、前のコード例に示されている、履歴の記録が有効になっているかどうかの確認の一環として取得されます。 履歴の記録の継続時間は、**[TimeSpan](/uwp/api/windows.foundation.timespan)** 構造体として提供されます。これも、履歴のバッファーの継続時間以下である必要があります。 目的の開始時刻と継続時間を確認したら、**[RecordTimeSpanToFileAsync](/uwp/api/windows.media.apprecording.apprecordingmanager.recordtimespantofileasync)** を呼び出して、記録操作を開始します。

手動で記録を開始および停止する場合と同様に、履歴の記録が完了したときに、返された **[AppRecordingResult](/uwp/api/windows.media.apprecording.apprecordingresult)** オブジェクトの **[Succeeded](/uwp/api/windows.media.apprecording.apprecordingresult.Succeeded)** プロパティを確認して、記録操作が成功したかどうかを判断できます。また、**[IsFileTruncated](/uwp/api/windows.media.apprecording.apprecordingresult.IsFileTruncated)** プロパティと **[Duration](/uwp/api/windows.media.apprecording.apprecordingresult.Duration)** プロパティを確認して、記録されたファイルの実際の継続時間を検出できます。ファイルが切り詰められている場合、実際の継続時間は要求された継続時間よりも短い場合があります。

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetRecordTimeSpanToFile":::

次の例では、前の例で示した履歴の記録操作を開始するためのいくつかの基本的なコードを示します。

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetCallRecordTimeSpanToFile":::

## <a name="save-screenshot-images-to-files"></a>スクリーンショットの画像をファイルに保存する
アプリでスクリーンショットのキャプチャを開始できます。アプリのウィンドウの現在の内容を、1 つの画像ファイルに保存することも、異なる画像エンコードで複数の画像ファイルに保存することもできます。 使用する画像エンコードを指定するには、それぞれが画像の種類を表す文字列の一覧を作成します。 **[ImageEncodingSubtypes](/uwp/api/windows.media.mediaproperties.mediaencodingsubtypes)** のプロパティは、サポートされている画像の種類ごとに、適切な文字列 (**MediaEncodingSubtypes.Png**、**MediaEncodingSubtypes.JpegXr** など) を提供します。

画面キャプチャを開始するには、**AppRecordingManager** オブジェクトの **[SaveScreenshotToFilesAsync](/uwp/api/windows.media.apprecording.apprecordingmanager.savescreenshottofilesasync)** メソッドを呼び出します。 このメソッドの最初のパラメーターは、画像ファイルの保存場所を示す **StorageFolder** です。 2 番目のパラメーターは、システムが、保存される各画像の種類の拡張子 (".png" など) を追加する、ファイル名のプレフィックスです。

**SaveScreenshotToFilesAsync** の 3 番目のパラメーターは、キャプチャされる現在のウィンドウが HDR コンテンツを表示している場合に、システムが適切な色空間変換を実行できるようにするために必要です。 HDR コンテンツが存在する場合は、このパラメーターを **AppRecordingSaveScreenshotOption.HdrContentVisible** に設定する必要があります。 それ以外の場合は、**AppRecordingSaveScreenshotOption.None** を使用します。 このメソッドの最後のパラメーターは、画面をキャプチャするか画像形式の一覧です。

**SaveScreenshotToFilesAsync** の非同期呼び出しが完了すると、**[AppRecordingSavedScreenshotInfo](/uwp/api/windows.media.apprecording.apprecordingsavedscreenshotinfo)** オブジェクトが返されます。このオブジェクトは、**StorageFile** と、保存された各画像の種類を示す **MediaEncodingSubtypes** 値を提供します。

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetSaveScreenShotToFiles":::

次の例では、前の例で示したスクリーンショットの操作を開始するためのいくつかの基本的なコードを示します。

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetCallSaveScreenShotToFiles":::

## <a name="add-game-metadata-for-system-and-app-initiated-capture"></a>システムやアプリによるキャプチャのゲーム メタデータを追加する
この記事の以下のセクションでは、キャプチャまたはブロードキャストされるゲームプレイの MP4 ストリームにシステムが埋め込むメタデータを提供する方法について説明します。 メタデータは、組み込みのシステム UI を使用してキャプチャされたメディアや、**AppRecordingManager** を使用してアプリでキャプチャされたメディアに埋め込むことができます。 このメタデータを、メディアの再生中にアプリや他のアプリで抽出して、キャプチャまたはブロードキャストされたゲームプレイと同期する、コンテキストに対応したエクスペリエンスを提供することができます。

### <a name="get-an-instance-of-appcapturemetadatawriter"></a>AppCaptureMetadataWriter のインスタンスを取得する
アプリのキャプチャ メタデータを管理するためのプライマリ クラスは、**[AppCaptureMetadataWriter](/uwp/api/windows.media.capture.appcapturemetadatawriter)** です。 このクラスのインスタンスを初期化する前に、**[ApiInformation.IsApiContractPresent](/uwp/api/windows.foundation.metadata.apiinformation.isapicontractpresent)** メソッドを使用して、*Windows.Media.Capture.AppCaptureMetadataContract* バージョン 1.0 を照会し、この API が現在のデバイスで利用できることを確認します。

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetGetMetadataWriter":::

### <a name="write-metadata-to-the-system-cache-for-your-app"></a>アプリのシステム キャッシュにメタデータを書き込む
各メタデータ項目には 1 つのラベルがあり、メタデータ項目、関連するデータ値 (文字列、整数、または double 型の値)、データ項目の相対的な優先順位を示す **[AppCaptureMetadataPriority](/uwp/api/windows.media.capture.appcapturemetadatapriority)** 列挙の値が識別されます。 メタデータ項目は、ある時点で発生する "イベント" またはある時間枠で値を維持する "状態" と見なすことができます。 メタデータは、システムによってアプリ用に割り当てられて管理されるメモリ キャッシュに書き込まれます。 システムは、メタデータのメモリ キャッシュにサイズ制限を適用し、制限に達すると、各メタデータに書き込まれている優先順位に基づいてデータを削除します。 この記事の次のセクションでは、アプリのメタデータのメモリ割り当てを管理する方法を示します。

一般的なアプリでは、キャプチャ セッションの最初に特定のメタデータを書き込んで、以降のデータのコンテキストを提供することを選択できます。 このシナリオでは、瞬間的な "イベント" データを使用することをお勧めします。 この例では、**[AddStringEvent](/uwp/api/windows.media.capture.appcapturemetadatawriter.addstringevent)**、**[AddDoubleEvent](/uwp/api/windows.media.capture.appcapturemetadatawriter.adddoubleevent)**、および**[AddInt32Event](/uwp/api/windows.media.capture.appcapturemetadatawriter.addint32event)** を呼び出して、各データ型の瞬間的な値を設定します。

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetStartSession":::

一定時間継続する "状態" データを使用する一般的なシナリオとして、プレイヤーが現在プレイしているゲーム マップの追跡があります。 この例では、**[StartStringState](/uwp/api/windows.media.capture.appcapturemetadatawriter.startstringstate)** を呼び出して状態の値を設定します。 

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetStartMap":::

特定の状態が終了したことを記録するには、**[StopState](/uwp/api/windows.media.capture.appcapturemetadatawriter.stopstate)** を呼び出します。

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetEndMap":::

状態は、既存の状態ラベルを持つ新しい値を設定して上書きできます。

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetLevelUp":::

**[StopAllStates](/uwp/api/windows.media.capture.appcapturemetadatawriter.StopAllStates)** を呼び出すことによって、現在開いているすべての状態を終了できます。

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetRaceComplete":::

### <a name="manage-metadata-cache-storage-limit"></a>メタデータ キャッシュの記憶域の制限を管理する
**AppCaptureMetadataWriter** を使用して書き込むメタデータは、関連付けられているメディア ストリームに書き込まれるまで、システムによってキャッシュされます。 システムでは、各アプリのメタデータ キャッシュのサイズ制限を定義しています。 キャッシュのサイズ制限に達すると、システムはキャッシュされたメタデータの削除を開始します。 AppCaptureMetadataPriority を使用してメタデータを削除する前に、 **[AppCaptureMetadataPriority](/uwp/api/windows.media.capture.appcapturemetadatapriority)** 優先度値を使用して書き込まれたメタデータが削除され**[ます。](/uwp/api/windows.media.capture.appcapturemetadatapriority)**

いつでも、**[RemainingStorageBytesAvailable](/uwp/api/windows.media.capture.appcapturemetadatawriter.RemainingStorageBytesAvailable)** を呼び出すことによって、アプリのメタデータ キャッシュで利用可能なバイト数を確認できます。 独自のアプリで定義されたしきい値を設定することを選択し、その後でキャッシュに書き込むメタデータの量を減らすことを選択できます。 次の例は、このパターンの簡単な実装を示しています。

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetCheckMetadataStorage":::

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetComboExecuted":::

### <a name="receive-notifications-when-the-system-purges-metadata"></a>システムがメタデータを削除するときに通知を受け取る
システムが **[Metadatapurged](/uwp/api/windows.media.capture.appcapturemetadatawriter.MetadataPurged)** 推奨イベントのハンドラーを登録することで、アプリのメタデータの削除を開始したときに通知を受け取るように登録できます。

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetRegisterMetadataPurged":::

**MetadataPurged** イベントのハンドラーで、優先順位の低い状態を終了してメタデータ キャッシュの空き容量を増やすことも、キャッシュに書き込むメタデータの量を削減するためにアプリで定義されたロジックを実装することも、何もせずに書き込み時の優先順位に基づいてシステムによるキャッシュの削除を継続することもできます。

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetOnMetadataPurged":::

## <a name="related-topics"></a>関連トピック

* [Gaming](index.md)
 

 
