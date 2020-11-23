---
title: バックグラウンド タスクのガイドライン
description: アプリケーションでのインプロセスおよびアウトプロセスのバックグラウンドタスクの開発と実行に関する詳細なガイダンスを表示します。
ms.assetid: 18FF1104-1F73-47E1-9C7B-E2AA036C18ED
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10、uwp、バックグラウンドタスク
ms.localizationpriority: medium
ms.openlocfilehash: b73568c5fb4bae6392051fedcd6ca3dea078a98d
ms.sourcegitcommit: 4491da3f509b1126601990a816c6eb301d35ecc6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2020
ms.locfileid: "95416619"
---
# <a name="guidelines-for-background-tasks"></a>バックグラウンド タスクのガイドライン


アプリがバックグラウンド タスクを実行するための要件を満たしていることを確認します。

## <a name="background-task-guidance"></a>バックグラウンド タスクのガイダンス

バックグラウンド タスクの開発時とアプリの公開前に、次のガイダンスについて検討します。

バックグラウンド タスクを使ってバックグラウンドでメディアを再生する場合、Windows 10 バージョン 1607 で簡単に行うことができる機能強化について、「[バックグラウンドでのメディアの再生](../audio-video-camera/background-audio.md)」をご覧ください。

**インプロセス バックグラウンド タスクとアウトプロセス バックグラウンド タスク:** Windows 10 バージョン 1607 には、フォアグラウンド アプリと同じプロセスでバックグラウンド コードを実行できる [インプロセス バックグラウンド タスク](create-and-register-an-inproc-background-task.md) が導入されました。 インプロセス バックグラウンド タスクとアウトプロセス バックグラウンド タスクのどちらを使用するかを決定するときは、以下の事項を検討してください。

|考慮事項 | 影響 |
|--------------|--------|
|回復力   | バックグラウンド プロセスが別のプロセスで実行されている場合、バックグラウンド プロセスでクラッシュが発生してもフォアグラウンド アプリケーションがダウンしません。 さらに、実行時間制限を過ぎて実行された場合、アプリ内からでもバックグラウンド アクティビティを終了できます。 フォアグラウンド プロセスとバックグラウンド プロセスが互いに通信する必要がない場合は、バックグラウンド処理をフォアグラウンド アプリとは別のタスクに分離することをお勧めします (インプロセス バックグラウンド タスクの主な利点の 1 つは、プロセス間通信が不要になることであるためです)。 |
|シンプルさ    | インプロセス バックグラウンド タスクでは、プロセス間通信が不要のため、記述内容は複雑になりません。  |
|使用可能なトリガー | インプロセスバックグラウンドタスクは、 [DeviceUseTrigger](/uwp/api/windows.applicationmodel.background.deviceusetrigger?f=255&MSPPError=-2147217396)、 [DeviceServicingTrigger](/uwp/api/windows.applicationmodel.background.deviceservicingtrigger) 、および **iotstartuptask** というトリガーをサポートしていません。 |
|VoIP | インプロセス バックグラウンド タスクでは、アプリケーション内での VoIP バックグラウンド タスクのアクティブ化がサポートされていません。 |  

**トリガー インスタンスの数の制限:** アプリが登録できる一部のトリガー インスタンスの数には制限があります。 アプリが [ApplicationTrigger](/uwp/api/Windows.ApplicationModel.Background.ApplicationTrigger)、[MediaProcessingTrigger](/uwp/api/windows.applicationmodel.background.mediaprocessingtrigger)、および [DeviceUseTrigger](/uwp/api/windows.applicationmodel.background.deviceusetrigger?f=255&MSPPError=-2147217396) を登録できるのは、アプリのインスタンスごとに 1 回のみです。 アプリでこの制限を超えると、登録で例外がスローされます。

**CPU クォータ:** バックグラウンドタスクは、トリガーの種類に基づいて取得されるウォールクロックの使用時間によって制限されます。 ほとんどのトリガーは、使用時間がウォールクロック時間で 30 秒に制限されますが、負荷の高いタスクを完了するために最大 10 分実行できるトリガーもあります。 バッテリの寿命を長くし、フォアグラウンド アプリのユーザー エクスペリエンスを高めるため、バックグラウンド タスクは軽量にしてください。 バックグラウンド タスクに適用されるリソースの制約については、「[バックグラウンド タスクによるアプリのサポート](support-your-app-with-background-tasks.md)」をご覧ください。

**バックグラウンドタスクの管理:** アプリは、登録されたバックグラウンドタスクの一覧を取得し、進行状況と完了ハンドラーを登録して、それらのイベントを適切に処理する必要があります。 バックグラウンド タスク クラスでは、進行状況、キャンセル、完了を報告する必要があります。 詳細については、「キャンセルされ [たバックグラウンドタスクの処理](handle-a-cancelled-background-task.md)」と「 [バックグラウンドタスクの進行状況と完了の監視](monitor-background-task-progress-and-completion.md)」を参照してください。

**[Backgroundtaskdeferral](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskDeferral)を使用します。** バックグラウンドタスククラスで非同期コードを実行する場合は、必ず遅延を使用してください。 そうしないと、 [Run](/uwp/api/windows.applicationmodel.background.ibackgroundtask.run) メソッドから制御が戻ったとき (または、インプロセスバックグラウンドタスクの場合は [onbackgroundactivated 化](/uwp/api/windows.ui.xaml.application.onbackgroundactivated) されたメソッド)、バックグラウンドタスクが途中で終了することがあります。 詳しくは、「[アウトプロセス バックグラウンド タスクの作成と登録](create-and-register-a-background-task.md)」をご覧ください

別の方法として、保留を 1 回要求し、**async/await** を使って、非同期メソッドの呼び出しを完了させることもできます。 **await** メソッドを呼び出した後、保留を閉じます。

**アプリケーション マニフェストを更新する:** アウトプロセスで実行されるバックグラウンド タスクの場合、アプリケーション マニフェストで、各バックグラウンド タスクを、バックグラウンド タスクで使用されるトリガーの種類と共に宣言します。 この宣言がないと、アプリでは実行時にバックグラウンド タスクを登録できません。

複数のバック グラウンド タスクがある場合は、同じホスト プロセスで実行する必要があるか、または別のホスト プロセスに分離する必要があるかどうかを検討してください。 1 つのバック グラウンド タスクでエラーが発生したときに別のバック グラウンド タスクが停止することが心配な場合は、別のホスト プロセスに配置します。  マニフェスト デザイナーの **リソース グループ** エントリを使用してバックグラウンド タスクを別のホスト プロセスにグループ化します。 

**リソース グループ** を設定するには、Package.appxmanifest デザイナーを開き、**[宣言]** を選択し、**[アプリ サービス]** 宣言を追加します。

![リソース グループの設定](images/resourcegroup.png)

リソース グループの設定の詳細については、「[アプリケーション スキーマ リファレンス](/uwp/schemas/appxpackage/uapmanifestschema/element-application)」を参照してください。

フォアグラウンド アプリと同じプロセスで実行されるバックグラウンド タスクは、アプリケーション マニフェストでの自身を宣言する必要はありません。 アウトプロセスで実行されるバックグラウンド タスクをマニフェストで宣言する方法について詳しくは、「[アプリケーション マニフェストでのバックグラウンド タスクの宣言](declare-background-tasks-in-the-application-manifest.md)」をご覧ください。

**アプリの更新を準備する:** アプリを更新する場合は、**ServicingComplete** バックグラウンド タスク (「SystemTriggerType [](/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType)」をご覧ください) を作って登録し、以前のバージョンのアプリのバックグラウンド タスクを登録解除して新しいバージョンのバックグラウンド タスクを登録します。 これは、フォアグラウンドで実行中のコンテキストの外で必要となる可能性があるアプリの更新を実行するのに適したタイミングです。

**バックグラウンド タスクを実行する要求:**

> **重要**  Windows 10 以降、バックグラウンド タスクを実行する前提条件として、アプリをロック画面に配置する必要はなくなりました。

ユニバーサル Windows プラットフォーム (UWP) アプリは、ロック画面にピン留めしなくても、サポートされているすべての種類のタスクを実行できます。 ただし、アプリは [**Getaccessstate**](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.getaccessstatus) を呼び出し、アプリがバックグラウンドで実行されることを拒否されていないことを確認する必要があります。 **Getaccessstatus** が、拒否された [**backgroundaccessstatus**](/uwp/api/windows.applicationmodel.background.backgroundaccessstatus)列挙体のいずれかを返さないことを確認します。 たとえば、ユーザーがデバイスの設定でアプリのバックグラウンドタスクのアクセス許可を明示的に拒否している場合、このメソッドは DeniedByUser を返します **。**

アプリがバックグラウンドでの実行を拒否された場合、アプリは [**Requestaccessasync**](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync) を呼び出して、バックグラウンドタスクを登録する前に応答が拒否されないようにする必要があります。

バックグラウンド アクティビティとバッテリー節約機能についてのユーザーの選択について詳しくは、「[バックグラウンド アクティビティの最適化](../debug-test-perf/optimize-background-activity.md)」をご覧ください。 
## <a name="background-task-checklist"></a>バックグラウンド タスクのチェック リスト

*インプロセス バックグラウンド タスクとアウトプロセス バックグラウンド タスクの両方に適用されます*

-   バックグラウンド タスクを適切なトリガーに関連付けます。
-   条件を追加して、バックグラウンド タスクが適切に実行されるようにします。
-   バックグラウンド タスクの進行、完了、取り消しを処理します。
-   アプリ起動時にバックグラウンド タスクを再登録します。 これにより、初めてアプリを起動したときにそれらが登録されるようになります。 また、ユーザーがバックグラウンド タスク実行機能を無効にしたかどうかを検出する方法も提供されます (登録に失敗した場合)。
-   バックグラウンド タスクの登録エラーを確認します。 必要に応じて、別のパラメーター値でバックグラウンド タスクをもう一度登録してみます。
-   デスクトップ以外のすべてのデバイス ファミリでは、デバイスのメモリが少なくなった場合、バックグラウンド タスクが終了することがあります。 メモリ不足の例外が検出されないか、検出されてもアプリによって処理されない場合、バックグラウンド タスクは、警告や OnCanceled イベントの発生なしに終了します。 こうすることで、フォアグラウンドのアプリのユーザー エクスペリエンスが保証されます。 バックグラウンド タスクは、このシナリオを処理できるように設計する必要があります。

*アウトプロセス バックグラウンド タスクにのみ適用されます*

-   Windows ランタイムコンポーネントでバックグラウンドタスクを作成します。
-   バックグラウンド タスクでは、トースト、タイル、バッジの更新以外の UI は表示しません。
-   [Run](/uwp/api/windows.applicationmodel.background.ibackgroundtask.run) メソッドでは、各非同期メソッド呼び出しに対して保留を要求し、メソッドが終了した時点で閉じます。 または、 **async/await** で1つの遅延を使用します。
-   固定ストレージを使って、バックグラウンド タスクとアプリ間でデータを共有します。
-   アプリケーション マニフェストで、各バックグラウンド タスクと共に、バックグラウンド タスクで使うトリガーの種類を宣言します。 エントリ ポイントとトリガーの種類が正しいことを確認します。
-   アプリケーションと同じコンテキストで実行する必要があるトリガー ( [**Controlchanneltrigger**](/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger)など) を使用している場合を除き、マニフェストに実行可能な要素を指定しないでください。

*インプロセス バックグラウンド タスクにのみ適用されます*

- タスクをキャンセルするとき、キャンセルが発生するか、プロセス全体が終了する前に `BackgroundActivated` イベント ハンドラーが終了したことを確認してください。
-   バックグラウンド タスクの存続期間は短くします。 ほとんどのバックグラウンドタスクは、最大30秒に制限されています。


*回避事項*
- COM または RPC を介したプロセス間通信の使用を最小限に抑えます。
-   通信しようとしているプロセスが実行中の状態ではない可能性があり、ハングする可能性があります。
-   プロセス間の通信を容易にするためにかなりの時間がかかり、バックグラウンドタスクの実行に割り当てられた時間としてカウントされます。
- バックグラウンド タスクでのユーザー操作に依存することはできません。


## <a name="related-topics"></a>関連トピック

* [インプロセス バックグラウンド タスクの作成と登録](create-and-register-an-inproc-background-task.md)
* [アウトプロセス バックグラウンド タスクの作成と登録](create-and-register-a-background-task.md)
* [winmain COM バックグラウンド タスクの作成と登録](create-and-register-a-winmain-background-task.md)
* [アプリケーション マニフェストでのバックグラウンド タスクの宣言](declare-background-tasks-in-the-application-manifest.md)
* [バックグラウンドでメディアを再生する](../audio-video-camera/background-audio.md)
* [取り消されたバックグラウンド タスクの処理](handle-a-cancelled-background-task.md)
* [バックグラウンド タスクの進捗状況と完了の監視](monitor-background-task-progress-and-completion.md)
* [バックグラウンド タスクの登録](register-a-background-task.md)
* [バックグラウンド タスクによるシステム イベントへの応答](respond-to-system-events-with-background-tasks.md)
* [バックグラウンド タスクを実行するための条件の設定](set-conditions-for-running-a-background-task.md)
* [バックグラウンド タスクのライブ タイルの更新](update-a-live-tile-from-a-background-task.md)
* [メンテナンス トリガーの使用](use-a-maintenance-trigger.md)
* [タイマーでのバックグラウンド タスクの実行](run-a-background-task-on-a-timer-.md)
* [バックグラウンド タスクのデバッグ](debug-a-background-task.md)
* [UWP アプリで一時停止イベント、再開イベント、バックグラウンド イベントをトリガーする方法 (デバッグ時)](/previous-versions/hh974425(v=vs.110))

 

 
