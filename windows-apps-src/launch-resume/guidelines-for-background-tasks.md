---
title: バックグラウンド タスクのガイドライン
description: アプリがバックグラウンド タスクを実行するための要件を満たしていることを確認します。
ms.assetid: 18FF1104-1F73-47E1-9C7B-E2AA036C18ED
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10、uwp、バックグラウンドタスク
ms.localizationpriority: medium
ms.openlocfilehash: 9c66cbccbdeed1258a25d3da03ed34a64995cd48
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/20/2019
ms.locfileid: "74260496"
---
# <a name="guidelines-for-background-tasks"></a>バックグラウンド タスクのガイドライン


アプリがバックグラウンド タスクを実行するための要件を満たしていることを確認します。

## <a name="background-task-guidance"></a>バックグラウンド タスクのガイダンス

バックグラウンド タスクの開発時とアプリの公開前に、次のガイダンスについて検討します。

バックグラウンド タスクを使ってバックグラウンドでメディアを再生する場合、Windows 10 バージョン 1607 で簡単に行うことができる機能強化について、「[バックグラウンドでのメディアの再生](https://docs.microsoft.com/windows/uwp/audio-video-camera/background-audio)」をご覧ください。

**インプロセス バックグラウンド タスクとアウトプロセス バックグラウンド タスク:** Windows 10 バージョン 1607 には、フォアグラウンド アプリと同じプロセスでバックグラウンド コードを実行できる[インプロセス バックグラウンド タスク](create-and-register-an-inproc-background-task.md) が導入されました。 インプロセス バックグラウンド タスクとアウトプロセス バックグラウンド タスクのどちらを使用するかを決定するときは、以下の事項を検討してください。

|考慮事項 | 影響 |
|--------------|--------|
|復元性   | バックグラウンド プロセスが別のプロセスで実行されている場合、バックグラウンド プロセスでクラッシュが発生してもフォアグラウンド アプリケーションがダウンしません。 さらに、実行時間制限を過ぎて実行された場合、アプリ内からでもバックグラウンド アクティビティを終了できます。 フォアグラウンド プロセスとバックグラウンド プロセスが互いに通信する必要がない場合は、バックグラウンド処理をフォアグラウンド アプリとは別のタスクに分離することをお勧めします (インプロセス バックグラウンド タスクの主な利点の 1 つは、プロセス間通信が不要になることであるためです)。 |
|シンプルさ    | インプロセス バックグラウンド タスクでは、プロセス間通信が不要のため、記述内容は複雑になりません。  |
|使用可能なトリガー | インプロセス バックグラウンド タスクでは、[DeviceUseTrigger](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.deviceusetrigger?f=255&MSPPError=-2147217396)、[DeviceServicingTrigger](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.deviceservicingtrigger)、**IoTStartupTask** の各トリガーがサポートされていません。 |
|VoIP | インプロセス バックグラウンド タスクでは、アプリケーション内での VoIP バックグラウンド タスクのアクティブ化がサポートされていません。 |  

**トリガー インスタンスの数の制限:** アプリが登録できる一部のトリガー インスタンスの数には制限があります。 アプリが [ApplicationTrigger](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.ApplicationTrigger)、[MediaProcessingTrigger](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.mediaprocessingtrigger)、および [DeviceUseTrigger](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.deviceusetrigger?f=255&MSPPError=-2147217396) を登録できるのは、アプリのインスタンスごとに 1 回のみです。 アプリでこの制限を超えると、登録で例外がスローされます。

**CPU の割り当て:** バックグラウンド タスクは、トリガーの種類に基づいて取得するウォールクロック時間の長さによって使用が制限されます。 ほとんどのトリガーは、使用時間がウォールクロック時間で 30 秒に制限されますが、負荷の高いタスクを完了するために最大 10 分実行できるトリガーもあります。 バッテリの寿命を長くし、フォアグラウンド アプリのユーザー エクスペリエンスを高めるため、バックグラウンド タスクは軽量にしてください。 バックグラウンド タスクに適用されるリソースの制約については、「[バックグラウンド タスクによるアプリのサポート](support-your-app-with-background-tasks.md)」をご覧ください。

**バックグラウンド タスクを管理する:** アプリでは、登録済みのバックグラウンド タスクの一覧を取得し、進行状況ハンドラーと完了ハンドラーを登録して、各イベントを適切に処理する必要があります。 バックグラウンド タスク クラスでは、進行状況、キャンセル、完了を報告する必要があります。 詳しくは、「[取り消されたバックグラウンド タスクの処理](handle-a-cancelled-background-task.md)」と「[バックグラウンド タスクの進捗状況と完了の監視](monitor-background-task-progress-and-completion.md)」をご覧ください。

**Use [BackgroundTaskDeferral](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskDeferral)を使用する:** バックグラウンド タスク クラスで非同期コードを実行する場合は、保留を使ってください。 そうしないと、 [Run](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtask.run)メソッドから制御が戻ったとき (または、インプロセスバックグラウンドタスクの場合は[onbackgroundactivated 化](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onbackgroundactivated)されたメソッド)、バックグラウンドタスクが途中で終了することがあります。 詳しくは、「[アウトプロセス バックグラウンド タスクの作成と登録](create-and-register-a-background-task.md)」をご覧ください

別の方法として、保留を 1 回要求し、**async/await** を使って、非同期メソッドの呼び出しを完了させることもできます。 **await** メソッドを呼び出した後、保留を閉じます。

**アプリケーション マニフェストを更新する:** アウトプロセスで実行されるバックグラウンド タスクの場合、アプリケーション マニフェストで、各バックグラウンド タスクを、バックグラウンド タスクで使用されるトリガーの種類と共に宣言します。 この宣言がないと、アプリでは実行時にバックグラウンド タスクを登録できません。

複数のバック グラウンド タスクがある場合は、同じホスト プロセスで実行する必要があるか、または別のホスト プロセスに分離する必要があるかどうかを検討してください。 1 つのバック グラウンド タスクでエラーが発生したときに別のバック グラウンド タスクが停止することが心配な場合は、別のホスト プロセスに配置します。  マニフェスト デザイナーの **リソース グループ** エントリを使用してバックグラウンド タスクを別のホスト プロセスにグループ化します。 

**リソース グループ**を設定するには、Package.appxmanifest デザイナーを開き、 **[宣言]** を選択し、 **[アプリ サービス]** 宣言を追加します。

![リソース グループの設定](images/resourcegroup.png)

リソース グループの設定の詳細については、「[アプリケーション スキーマ リファレンス](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-application)」を参照してください。

フォアグラウンド アプリと同じプロセスで実行されるバックグラウンド タスクは、アプリケーション マニフェストでの自身を宣言する必要はありません。 アウトプロセスで実行されるバックグラウンド タスクをマニフェストで宣言する方法について詳しくは、「[アプリケーション マニフェストでのバックグラウンド タスクの宣言](declare-background-tasks-in-the-application-manifest.md)」をご覧ください。

**アプリの更新を準備する:** アプリを更新する場合は、**ServicingComplete** バックグラウンド タスク (「SystemTriggerType[](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType)」をご覧ください) を作って登録し、以前のバージョンのアプリのバックグラウンド タスクを登録解除して新しいバージョンのバックグラウンド タスクを登録します。 これは、フォアグラウンドで実行中のコンテキストの外で必要となる可能性があるアプリの更新を実行するのに適したタイミングです。

**バックグラウンドタスクの実行要求:**

> **重要**  Windows 10 以降では、バックグラウンドタスクを実行するための前提条件として、アプリがロック画面に表示される必要がなくなりました。

ユニバーサル Windows プラットフォーム (UWP) アプリは、ロック画面にピン留めしなくても、サポートされているすべての種類のタスクを実行できます。 ただし、どの種類のバックグラウンド タスクを登録する場合でも、その前にアプリが [**RequestAccessAsync**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync) を呼び出す必要があります。 ユーザーがデバイス設定でバックグラウンド タスクに対するアプリのアクセス許可を明示的に拒否した場合、このメソッドは [**BackgroundAccessStatus.DeniedByUser**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundAccessStatus) を返します。 バックグラウンド アクティビティとバッテリー節約機能についてのユーザーの選択について詳しくは、「[バックグラウンド アクティビティの最適化](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-background-activity)」をご覧ください。 
## <a name="background-task-checklist"></a>バックグラウンド タスクのチェック リスト

*インプロセスとアウトプロセスのバックグラウンドタスクの両方に適用されます。*

-   バックグラウンド タスクを適切なトリガーに関連付けます。
-   条件を追加して、バックグラウンド タスクが適切に実行されるようにします。
-   バックグラウンド タスクの進行、完了、取り消しを処理します。
-   アプリ起動時にバックグラウンド タスクを再登録します。 これにより、初めてアプリを起動したときにそれらが登録されるようになります。 また、ユーザーがバックグラウンド タスク実行機能を無効にしたかどうかを検出する方法も提供されます (登録に失敗した場合)。
-   バックグラウンド タスクの登録エラーを確認します。 必要に応じて、別のパラメーター値でバックグラウンド タスクをもう一度登録してみます。
-   デスクトップ以外のすべてのデバイス ファミリでは、デバイスのメモリが少なくなった場合、バックグラウンド タスクが終了することがあります。 メモリ不足の例外が検出されないか、検出されてもアプリによって処理されない場合、バックグラウンド タスクは、警告や OnCanceled イベントの発生なしに終了します。 こうすることで、フォアグラウンドのアプリのユーザー エクスペリエンスが保証されます。 バックグラウンド タスクは、このシナリオを処理できるように設計する必要があります。

*プロセス外のバックグラウンドタスクにのみ適用されます*

-   Windows ランタイムコンポーネントでバックグラウンドタスクを作成します。
-   バックグラウンド タスクでは、トースト、タイル、バッジの更新以外の UI は表示しません。
-   [Run](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtask.run) メソッドでは、各非同期メソッド呼び出しに対して保留を要求し、メソッドが終了した時点で閉じます。 または、**async/await** で保留を 1 回使用します。
-   固定ストレージを使って、バックグラウンド タスクとアプリ間でデータを共有します。
-   アプリケーション マニフェストで、各バックグラウンド タスクと共に、バックグラウンド タスクで使うトリガーの種類を宣言します。 エントリ ポイントとトリガーの種類が正しいことを確認します。
-   アプリと同じコンテキストで実行する必要があるトリガー ([**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger) など) を使う場合を除き、マニフェストでは Executable 要素を指定しないでください。

*インプロセスバックグラウンドタスクにのみ適用されます*

- タスクをキャンセルするとき、キャンセルが発生するか、プロセス全体が終了する前に `BackgroundActivated` イベント ハンドラーが終了したことを確認してください。
-   バックグラウンド タスクの存続期間は短くします。 バックグラウンド タスクに使用できる時間は、ウォールクロック時間で 30 秒間に制限されています。
-   バックグラウンド タスクでのユーザー操作に依存することはできません。

## <a name="related-topics"></a>関連トピック

* [インプロセス バックグラウンド タスクの作成と登録](create-and-register-an-inproc-background-task.md)
* [アウトプロセス バックグラウンド タスクの作成と登録](create-and-register-a-background-task.md)
* [アプリケーション マニフェストでのバックグラウンド タスクの宣言](declare-background-tasks-in-the-application-manifest.md)
* [バックグラウンドでメディアを再生する](https://docs.microsoft.com/windows/uwp/audio-video-camera/background-audio)
* [取り消されたバックグラウンド タスクの処理](handle-a-cancelled-background-task.md)
* [バックグラウンド タスクの進捗状況と完了の監視](monitor-background-task-progress-and-completion.md)
* [バックグラウンド タスクの登録](register-a-background-task.md)
* [バックグラウンド タスクによるシステム イベントへの応答](respond-to-system-events-with-background-tasks.md)
* [バックグラウンド タスクを実行するための条件の設定](set-conditions-for-running-a-background-task.md)
* [バックグラウンド タスクのライブ タイルの更新](update-a-live-tile-from-a-background-task.md)
* [メンテナンス トリガーの使用](use-a-maintenance-trigger.md)
* [タイマーでのバックグラウンド タスクの実行](run-a-background-task-on-a-timer-.md)
* [バックグラウンド タスクのデバッグ](debug-a-background-task.md)
* [UWP アプリで中断イベント、再開イベント、およびバックグラウンドイベントをトリガーする方法 (デバッグ時)](https://msdn.microsoft.com/library/windows/apps/hh974425(v=vs.110).aspx)

 

 
