---
title: インプロセス バックグラウンド タスクの作成と登録
description: フォアグラウンド アプリと同じプロセスで実行されるインプロセスのタスクを作成して登録します。
ms.date: 11/03/2017
ms.topic: article
keywords: windows 10、uwp、バックグラウンドタスク
ms.assetid: d99de93b-e33b-45a9-b19f-31417f1e9354
ms.localizationpriority: medium
ms.openlocfilehash: 489de52a3c592bac9d715b679470b84c2af7e621
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155996"
---
# <a name="create-and-register-an-in-process-background-task"></a>インプロセス バックグラウンド タスクの作成と登録

**重要な API**

-   [**IBackgroundTask**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask)
-   [**BackgroundTaskBuilder**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder)
-   [**BackgroundTaskCompletedEventHandler**](/uwp/api/windows.applicationmodel.background.backgroundtaskcompletedeventhandler)

このトピックでは、アプリと同じプロセスで実行されるバックグラウンド タスクを作成して登録する方法について説明します。

インプロセスのバック グラウンド タスクはアウトプロセスのバック グラウンド タスクよりも実装が簡単です。 ただし、インプロセスのバックグラウンド タスクでは回復力が低下します。 インプロセスのバックグラウンド タスクで実行中のコードがクラッシュすると、アプリがダウンします。 また、[DeviceUseTrigger](/uwp/api/windows.applicationmodel.background.deviceusetrigger)、[DeviceServicingTrigger](/uwp/api/windows.applicationmodel.background.deviceservicingtrigger)、**IoTStartupTask** はインプロセス モデルで使用できないことに注意してください。 アプリケーション内で VoIP バックグラウンド タスクをアクティブ化することもできません。 これらのトリガーとタスクは、アウトプロセスのバックグラウンド タスク モデルを使用してサポートされています。

バックグラウンド アクティビティは、実行時間制限を超えて実行された場合に、アプリのフォアグラウンド プロセス内で実行されているときでも終了できます。 特定の目的では、別のプロセスで実行するバックグラウンド タスクに作業を分離した場合の回復性が有用です。 バックグラウンドの作業をフォアグラウンド アプリケーションとは別のタスクとして保持することは、フォアグラウンド アプリケーションとの通信を必要としない作業にとって最適なオプションである可能性があります。

## <a name="fundamentals"></a>基礎

インプロセス モデルでは、アプリがフォアグラウンドまたはバックグラウンドで実行されるときの改善された通知によってアプリケーションのライフサイクルが強化されます。 これらの切り替え用に、Application オブジェクトから [**EnteredBackground**](/uwp/api/windows.applicationmodel.core.coreapplication.enteredbackground) と [**LeavingBackground**](/uwp/api/windows.applicationmodel.core.coreapplication.leavingbackground) という 2 つの新しいイベントを使用できます。 これらのイベントは、アプリケーションの表示状態に基づくアプリケーションのライフサイクルに適合します。これらのイベントの詳細とアプリケーションのライフサイクルに与える影響については、[アプリのライフサイクル](app-lifecycle.md)をご覧ください。

大まかに言うと、**EnteredBackground** イベントは、アプリがバックグラウンドで実行されている間に実行されるコードを実行します。また、**LeavingBackground** イベントは、アプリがフォアグラウンドに移動したことを知るために処理します。

## <a name="register-your-background-task-trigger"></a>バックグラウンド タスク トリガーを登録する

インプロセスのバックグラウンド アクティビティの登録は、アウトプロセスのバックグラウンド アクティビティを登録する方法とほぼ同じです。 すべてのバックグラウンド トリガーは、[BackgroundTaskBuilder](/uwp/api/windows.applicationmodel.background.backgroundtaskbuilder?f=255&MSPPError=-2147217396) を使用した登録から始まります。 ビルダーを使用すると、必要なすべての値を 1 か所で設定できるため、簡単にバックグラウンド タスクを登録できます。

> [!div class="tabbedCodeSnippets"]
> ```cs
> var builder = new BackgroundTaskBuilder();
> builder.Name = "My Background Trigger";
> builder.SetTrigger(new TimeTrigger(15, true));
> // Do not set builder.TaskEntryPoint for in-process background tasks
> // Here we register the task and work will start based on the time trigger.
> BackgroundTaskRegistration task = builder.Register();
> ```

> [!NOTE]
> ユニバーサル Windows アプリは、どの種類のバックグラウンド トリガーを登録する場合でも、先に [**RequestAccessAsync**](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync) を呼び出す必要があります。
> 更新プログラムのリリース後にユニバーサル Windows アプリが引き続き適切に実行されるようにするには、更新後にアプリが起動する際に、[**RemoveAccess**](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.removeaccess)、[**RequestAccessAsync**](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync) の順に呼び出す必要があります。 詳しくは、「[バックグラウンド タスクのガイドライン](guidelines-for-background-tasks.md)」をご覧ください。

インプロセスのバックグラウンド アクティビティの場合は `TaskEntryPoint.` を設定しません。この値を設定しないと、[OnBackgroundActivated()](/uwp/api/windows.ui.xaml.application.onbackgroundactivated) と呼ばれる、Application オブジェクトの新しい保護されたメソッドが既定のエントリ ポイントとして有効になります。

登録したトリガーは、[SetTrigger](/uwp/api/windows.applicationmodel.background.backgroundtaskbuilder.settrigger) メソッドで設定されたトリガーの種類に基づいて発生します。 上記の例では、[TimeTrigger](/uwp/api/windows.applicationmodel.background.timetrigger) が使用されています。これは、登録された時点から 15 分後に発生します。

## <a name="add-a-condition-to-control-when-your-task-will-run-optional"></a>どのようなときにタスクを実行するかという条件を追加する (省略可能)

トリガー イベントの発生後、どのようなときにタスクを実行するかという条件を追加することもできます。 たとえば、ユーザーが存在するときにだけタスクを実行する場合、**UserPresent** という条件を使います。 指定できる条件の一覧については、「[**SystemConditionType**](/uwp/api/Windows.ApplicationModel.Background.SystemConditionType)」をご覧ください。

次のサンプル コードでは、"ユーザーの存在" を条件として割り当てています。

> [!div class="tabbedCodeSnippets"]
> ```cs
> builder.AddCondition(new SystemCondition(SystemConditionType.UserPresent));
> ```

## <a name="place-your-background-activity-code-in-onbackgroundactivated"></a>バックグラウンド アクティビティのコードを OnBackgroundActivated() に配置する

バックグラウンドのアクティビティコードを [Onbackgroundactivated アクティブ](/uwp/api/windows.ui.xaml.application.onbackgroundactivated) にして、バックグラウンドトリガーが起動したときに応答します。 **OnBackgroundActivated** は、[IBackgroundTask.Run](/uwp/api/windows.applicationmodel.background.ibackgroundtask.run?f=255&MSPPError=-2147217396) と同様に処理することができます。 メソッドには [BackgroundActivatedEventArgs](/uwp/api/windows.applicationmodel.activation.backgroundactivatedeventargs) パラメーターがあり、このパラメーターには **Run** メソッドが提供するすべてのものが含まれています。 たとえば、App.xaml.cs の場合は次のようになります。

``` cs
using Windows.ApplicationModel.Background;

...

sealed partial class App : Application
{
  ...

  protected override void OnBackgroundActivated(BackgroundActivatedEventArgs args)
  {
      base.OnBackgroundActivated(args);
      IBackgroundTaskInstance taskInstance = args.TaskInstance;
      DoYourBackgroundWork(taskInstance);  
  }
}
```

より充実した **Onbackgroundactivated 化** の例については、「 [ホストアプリと同じプロセスで実行されるように App service を変換する](convert-app-service-in-process.md)」を参照してください。

## <a name="handle-background-task-progress-and-completion"></a>バックグラウンド タスクの進捗状況と完了を処理する

タスクの進捗状況と完了は、マルチプロセスのバックグラウンド タスクの場合と同じ方法で監視できます (「[バックグラウンド タスクの進捗状況と完了の監視](monitor-background-task-progress-and-completion.md)」をご覧ください)。ただし、変数を使うと、より簡単にアプリで進捗状況または完了状態を追跡できます。 これは、アプリと同じプロセスでバックグラウンド アクティビティのコードを実行する利点の 1 つです。

## <a name="handle-background-task-cancellation"></a>バックグラウンド タスクの取り消しを処理する

インプロセスのバックグラウンド タスクは、アウトプロセスのバックグラウンド タスクの場合と同じ方法で取り消すことができます (「[取り消されたバックグラウンド タスクの処理](handle-a-cancelled-background-task.md)」をご覧ください)。 取り消しが発生する前に **BackgroundActivated** イベント ハンドラーを終了する必要があります。そうでないと、プロセス全体が終了します。 バックグラウンド タスクを取り消したときにフォアグラウンド アプリが予期せずに終了する場合は、取り消しが発生する前にハンドラーが終了していることを確認してください。

## <a name="the-manifest"></a>マニフェスト

アウトプロセスのバックグラウンド タスクとは異なり、インプロセスのバックグラウンド タスクを実行するためにパッケージ マニフェストにバックグラウンド タスクの情報を追加する必要はありません。

## <a name="summary-and-next-steps"></a>まとめと次のステップ

インプロセスのバックグラウンド タスクを作成する方法の基本を説明しました。

API リファレンス、バックグラウンド タスクの概念的ガイダンス、バックグラウンド タスクを使ったアプリの作成に関する詳しい説明については、次の関連するトピックをご覧ください。

## <a name="related-topics"></a>関連トピック

**バックグラウンド タスクに関する手順を詳しく説明するトピック**

* [アウトプロセスのバックグラウンド タスクをインプロセスのバックグラウンド タスクへ変換](convert-out-of-process-background-task.md)
* [アウトプロセス バックグラウンド タスクの作成と登録](create-and-register-a-background-task.md)
* [バックグラウンドでのメディアの再生](../audio-video-camera/background-audio.md)
* [バックグラウンド タスクによるシステム イベントへの応答](respond-to-system-events-with-background-tasks.md)
* [バックグラウンド タスクの登録](register-a-background-task.md)
* [バックグラウンド タスクを実行するための条件の設定](set-conditions-for-running-a-background-task.md)
* [メンテナンス トリガーの使用](use-a-maintenance-trigger.md)
* [取り消されたバックグラウンド タスクの処理](handle-a-cancelled-background-task.md)
* [バックグラウンド タスクの進捗状況と完了の監視](monitor-background-task-progress-and-completion.md)
* [タイマーでのバックグラウンド タスクの実行](run-a-background-task-on-a-timer-.md)

**バックグラウンド タスクのガイダンス**

* [バックグラウンド タスクのガイドライン](guidelines-for-background-tasks.md)
* [バックグラウンド タスクのデバッグ](debug-a-background-task.md)
* [UWP アプリで一時停止イベント、再開イベント、バックグラウンド イベントをトリガーする方法 (デバッグ時)](/previous-versions/hh974425(v=vs.110))

**バックグラウンド タスクの API リファレンス**

* [**Windows.ApplicationModel.Background**](/uwp/api/Windows.ApplicationModel.Background)