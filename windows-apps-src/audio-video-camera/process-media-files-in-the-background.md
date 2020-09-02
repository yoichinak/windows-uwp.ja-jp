---
ms.assetid: B5E3A66D-0453-4D95-A3DB-8E650540A300
description: この記事では、MediaProcessingTrigger とバックグラウンド タスクを使って、バックグラウンドでメディア ファイルを処理する方法について説明します。
title: バックグラウンドでのメディア ファイルの処理
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b7f726365a2e476e650f4b66d484840c0efabda5
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363825"
---
# <a name="process-media-files-in-the-background"></a>バックグラウンドでのメディア ファイルの処理



この記事では、 [**Mediaprocessingtrigger**](/uwp/api/Windows.ApplicationModel.Background.MediaProcessingTrigger) とバックグラウンドタスクを使用して、バックグラウンドでメディアファイルを処理する方法について説明します。

この記事で説明するサンプル アプリを使うと、ユーザーは入力メディア ファイルを選んでコード変換し、コード変換結果の出力ファイルを指定できます。 次に、バックグラウンド タスクが起動してコード変換操作が実行されます。 [**MediaProcessingTrigger**](/uwp/api/Windows.ApplicationModel.Background.MediaProcessingTrigger) は、コード変換だけでなくさまざまなメディア処理シナリオ (ディスクへのメディア コンポジションのレンダリング、処理の完了後の処理済みメディア ファイルのアップロードなど) をサポートすることを目的としています。

このサンプルで利用されているさまざまなユニバーサル Windows アプリ機能について詳しくは、次をご覧ください。

-   [メディア ファイルのコード変換](transcode-media-files.md)
-   [再開タスクとバックグラウンドタスクの起動](../launch-resume/index.md)
-   [バッジと通知をタイルする](../design/shell/tiles-and-notifications/index.md)

## <a name="create-a-media-processing-background-task"></a>メディア処理のバックグラウンド タスクの作成

Microsoft Visual Studio で既存のソリューションにバックグラウンド タスクを追加するには、コンポーネントの名前を入力します。

1.  **[ファイル]** メニューの **[追加]** をクリックし、**[新しいプロジェクト]** をクリックします。
2.  プロジェクトの種類として **[Windows ランタイム コンポーネント (ユニバーサル アプリ)]** を選びます。
3.  新しいコンポーネント プロジェクトの名前を入力します。 この例では、プロジェクト名として **MediaProcessingBackgroundTask** を使います。
4.  [OK] をクリックします。

**ソリューション エクスプ ローラー**で、既定で作成された "Class1.cs" ファイルのアイコンを右クリックし、**[名前の変更]** をクリックします。 ファイル名を "MediaProcessingTask.cs" に変更します。 Visual Studio に、このクラスへのすべての参照の名前を変更するかどうかを確認するメッセージが表示されたら、**[はい]** をクリックします。

名前が変更されたクラス ファイルで、次の **using** ディレクティブを追加してこれらの名前空間をプロジェクトに含めます。
                                  
:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs" id="SnippetBackgroundUsing":::

クラス宣言を更新して、クラスが [**Ibackgroundtask**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask)から継承されるようにします。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs" id="SnippetBackgroundClass":::

次のメンバー変数をクラスに追加します。

-   [**IBackgroundTaskInstance**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTaskInstance)。バックグラウンド タスクの進行状況によってフォアグラウンド アプリを更新するために使われます。
-   [**BackgroundTaskDeferral**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskDeferral)。メディアのコード変換が非同期的に実行されている間も、システムがバックグラウンド タスクをシャットダウンしないようにします。
-   **CancellationTokenSource** オブジェクト。非同期コード変換操作を取り消すために使うことができます。
-   [**MediaTranscoder**](/uwp/api/Windows.Media.Transcoding.MediaTranscoder) オブジェクト。メディア ファイルのコード変換に使われます。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs" id="SnippetBackgroundMembers":::

タスクが起動すると、システムはバックグラウンド タスクの [**Run**](/uwp/api/windows.applicationmodel.background.ibackgroundtask.run) メソッドを呼び出します。 メソッドに渡される [**IBackgroundTask**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask) オブジェクトを、対応するメンバー変数に設定します。 システムがバックグラウンド タスクをシャットダウンする必要がある場合に発生する [**Canceled**](/uwp/api/windows.applicationmodel.background.ibackgroundtaskinstance.canceled) イベントのハンドラーを登録します。 次に、[**Progress**](/uwp/api/windows.applicationmodel.background.ibackgroundtaskinstance.progress) プロパティを 0 に設定します。

次に、バックグラウンド タスク オブジェクトの [**GetDeferral**](/uwp/api/windows.applicationmodel.background.ibackgroundtaskinstance.getdeferral) メソッドを呼び出して保留を取得します。 非同期操作の実行中のため、タスクをシャットダウンしないようにシステムに指示されます。

次に、次のセクションで定義するヘルパー メソッド **TranscodeFileAsync** を呼び出します。 正常に完了した場合、ヘルパー メソッドが呼び出され、コード変換が完了したことをユーザーに通知するトースト通知が起動されます。

**Run** メソッドの最後に、保留オブジェクトで [**Complete**](/uwp/api/windows.applicationmodel.background.backgroundtaskdeferral.complete) を呼び出して、バックグラウンド タスクが完了したため終了できることをシステムが認識できるようにします。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs" id="SnippetRun":::

**TranscodeFileAsync** ヘルパー メソッドで、コード変換操作の入力ファイルと出力ファイルのファイル名がアプリの [**LocalSettings**](/uwp/api/windows.storage.applicationdata.localsettings) から取得されます。 これらの値は、フォアグラウンド アプリによって設定されます。 入力ファイルと出力ファイルの [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) オブジェクトを作成し、コード変換に使うエンコード プロファイルを作成します。

入力ファイル、出力ファイル、エンコード プロファイルを渡して [**PrepareFileTranscodeAsync**](/uwp/api/windows.media.transcoding.mediatranscoder.preparefiletranscodeasync) を呼び出します。 この呼び出しから返される [**PrepareTranscodeResult**](/uwp/api/Windows.Media.Transcoding.PrepareTranscodeResult) オブジェクトにより、コード変換を実行できるかどうかを把握できます。 [**CanTranscode**](/uwp/api/windows.media.transcoding.preparetranscoderesult.cantranscode) プロパティが true の場合、[**TranscodeAsync**](/uwp/api/windows.media.transcoding.preparetranscoderesult.transcodeasync) を呼び出してコード変換操作を実行します。

**AsTask** メソッドを使うと、非同期操作の進行状況を追跡したり、取り消したりできます。 必要な進行状況の単位と、タスクの現在の進行状況について通知するために呼び出されるメソッドの名前を指定して、新しい **Progress** オブジェクトを作成します。 タスクの取り消しを可能にするキャンセル トークンと共に、**Progress** オブジェクトを **AsTask** メソッドに渡します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs" id="SnippetTranscodeFileAsync":::

前の手順で Progress オブジェクトを作成するために使ったメソッド **Progress** で、バックグラウンド タスク インスタンスの進行状況を設定します。 これにより、進行状況がフォアグラウンド アプリ (実行されている場合) に渡されます。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs" id="SnippetProgress":::

**SendToastNotification** ヘルパー メソッドは、テキスト コンテンツしかないトーストのテンプレート XML ドキュメントを取得することによって新しいトースト通知を作成します。 トースト XML のテキスト要素が設定された後、XML ドキュメントから新しい [**ToastNotification**](/uwp/api/Windows.UI.Notifications.ToastNotification) オブジェクトが作成されます。 最後に、 [**Toastnotifier. Show**](/uwp/api/windows.ui.notifications.toastnotifier.show)を呼び出して、トーストをユーザーに表示します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs" id="SnippetSendToastNotification":::

システムがバック グラウンド タスクをキャンセルしたときに呼び出される [**Canceled**](/uwp/api/windows.applicationmodel.background.ibackgroundtaskinstance.canceled) イベントのハンドラーでは、利用統計情報を取集するためにログにエラーを記録することができます。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs" id="SnippetOnCanceled":::

## <a name="register-and-launch-the-background-task"></a>バックグラウンド タスクの登録と起動

フォアグラウンド アプリからバックグラウンド タスクを起動するには、フォアグラウンド アプリの Package.appmanifest ファイルを更新して、アプリがバックグラウンド タスクを使っていることをシステムが認識できるようにする必要があります。

1.  **ソリューション エクスプローラー**で、Package.appmanifest アイコンをダブルクリックしてマニフェスト エディターを開きます。
2.  **[宣言]** タブをクリックします。
3.  [ **使用可能な宣言**] から [ **バックグラウンドタスク** ] を選択し、[ **追加**] をクリックします。
4.  **[サポートされる宣言]** で、**[バックグラウンド タスク]** 項目が選択されていることを確認します。 **[プロパティ]** で、**[Media processing]** (メディア処理) チェック ボックスをオンにします。
5.  **[エントリ ポイント]** ボックスで、バックグラウンド テストの名前空間とクラス名を、ピリオドで区切って指定します。 この例では、エントリは次のとおりです。
   ```csharp
   MediaProcessingBackgroundTask.MediaProcessingTask
   ```
次に、フォアグラウンド アプリにバックグラウンド タスクへの参照を追加する必要があります。
1.  **ソリューション エクスプローラー**のフォアグラウンド アプリ プロジェクトで、**[参照]** フォルダーを右クリックし、**[参照の追加]** を選択します。
2.  **[プロジェクト]** ノードを展開し、**[ソリューション]** を選択します。
3.  バックグラウンド プロジェクトの横のボックスをオンにし、**[OK]** をクリックします。

この例のコードの残りの部分をフォアグラウンド アプリに追加する必要があります。 まず、次の名前空間をプロジェクトに追加する必要があります。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaProcessingTriggerWin10/cs/MediaProcessingTriggerWin10/MainPage.xaml.cs" id="SnippetForegroundUsing":::

次に、バックグラウンド タスクの登録に必要な次のメンバー変数を追加します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaProcessingTriggerWin10/cs/MediaProcessingTriggerWin10/MainPage.xaml.cs" id="SnippetForegroundMembers":::

**PickFilesToTranscode** ヘルパー メソッドは、[**FileOpenPicker**](/uwp/api/Windows.Storage.Pickers.FileOpenPicker) と [**FileSavePicker**](/uwp/api/Windows.Storage.Pickers.FileSavePicker) を使ってコード変換用の入力ファイルと出力ファイルを開きます。 アプリがアクセスできない場所にあるファイルをユーザーが選ぶ可能性があります。 バックグラウンド タスクがファイルを開けるようにするには、ファイルをアプリの [**FutureAccessList**](/uwp/api/windows.storage.accesscache.storageapplicationpermissions.futureaccesslist) に追加します。

最後に、アプリの [**LocalSettings**](/uwp/api/windows.storage.applicationdata.localsettings) で入力ファイルと出力ファイルの名前のエントリを設定します。 バックグラウンド タスクは、この場所からファイル名を取得します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaProcessingTriggerWin10/cs/MediaProcessingTriggerWin10/MainPage.xaml.cs" id="SnippetPickFilesToTranscode":::

バックグラウンド タスクを登録するには、新しい [**MediaProcessingTrigger**](/uwp/api/Windows.ApplicationModel.Background.MediaProcessingTrigger) と新しい [**BackgroundTaskBuilder**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) を作成します。 後で識別できるようにバックグラウンド タスク ビルダーの名前を設定します。 [**TaskEntryPoint**](/uwp/api/windows.applicationmodel.background.backgroundtaskbuilder.taskentrypoint) を、マニフェスト ファイルで使ったのと同じ名前空間とクラス名文字列に設定します。 [**Trigger**](/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.trigger) プロパティを **MediaProcessingTrigger** インスタンスに設定します。

タスクを登録する前に、[**AllTasks**](/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.alltasks) コレクションをループ処理し、[**BackgroundTaskBuilder.Name**](/uwp/api/windows.applicationmodel.background.backgroundtaskbuilder.name) プロパティで指定した名前を持つすべてのタスクで [**Unregister**](/uwp/api/windows.applicationmodel.background.ibackgroundtaskregistration.unregister) を呼び出すことにより、以前に登録したタスクを必ず登録解除してください。

[**Register**](/uwp/api/windows.applicationmodel.background.backgroundtaskbuilder.register) を呼び出してバックグラウンド タスクを登録します。 [**Completed**](/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.completed) イベントと [**Progress**](/uwp/api/windows.applicationmodel.background.ibackgroundtaskregistration.progress) イベントのハンドラーを登録します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaProcessingTriggerWin10/cs/MediaProcessingTriggerWin10/MainPage.xaml.cs" id="SnippetRegisterBackgroundTask":::

通常のアプリは、 **OnNavigatedTo** イベントなどで、アプリが最初に起動されたときにバックグラウンドタスクを登録します。

**MediaProcessingTrigger** オブジェクトの [**RequestAsync**](/uwp/api/windows.applicationmodel.background.mediaprocessingtrigger.requestasync) メソッドを呼び出してバックグラウンド タスクを起動します。 このメソッドによって返される [**MediaProcessingTriggerResult**](/uwp/api/Windows.ApplicationModel.Background.MediaProcessingTriggerResult) オブジェクトにより、バックグラウンド タスクが正常に起動されたかどうかを把握することができます。正常に起動されなかった場合は、バックグラウンド タスクが起動しなかった理由を把握できます。 

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaProcessingTriggerWin10/cs/MediaProcessingTriggerWin10/MainPage.xaml.cs" id="SnippetLaunchBackgroundTask":::

一般的なアプリは、UI コントロールの **クリック** イベントなどのユーザーの操作に応じてバックグラウンドタスクを起動します。

バックグラウンド タスクが操作の進行状況を更新すると、**OnProgress** イベント ハンドラーが呼び出されます。 この機会を使って、進行状況情報によって UI を更新することができます。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaProcessingTriggerWin10/cs/MediaProcessingTriggerWin10/MainPage.xaml.cs" id="SnippetOnProgress":::

バックグラウンド タスクの実行が完了すると、**OnCompleted** イベント ハンドラーが呼び出されます。 ここでも、UI を更新してユーザーに状態情報を示すことができます。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaProcessingTriggerWin10/cs/MediaProcessingTriggerWin10/MainPage.xaml.cs" id="SnippetOnCompleted":::


 

 
