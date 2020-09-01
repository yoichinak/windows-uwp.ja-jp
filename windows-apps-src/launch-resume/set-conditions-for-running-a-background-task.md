---
title: バックグラウンド タスクを実行するための条件の設定
description: バックグラウンド タスクをいつ実行するかを制御する条件の設定方法について説明します。
ms.assetid: 10ABAC9F-AA8C-41AC-A29D-871CD9AD9471
ms.date: 07/06/2018
ms.topic: article
keywords: windows 10、uwp、バックグラウンドタスク
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cpp
ms.openlocfilehash: 9339472d1f7d601accdc3791644ba328211e5191
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167736"
---
# <a name="set-conditions-for-running-a-background-task"></a>バックグラウンド タスクを実行するための条件の設定

**重要な API**

- [**SystemCondition**](/uwp/api/Windows.ApplicationModel.Background.SystemCondition)
- [**SystemConditionType**](/uwp/api/Windows.ApplicationModel.Background.SystemConditionType)
- [**BackgroundTaskBuilder**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder)

バックグラウンド タスクをいつ実行するかを制御する条件の設定方法について説明します。

場合によっては、バックグラウンド タスクを正しく実行するために、特定の条件を満たす必要があります。 バックグラウンド タスクを登録するときに、[**SystemConditionType**](/uwp/api/Windows.ApplicationModel.Background.SystemConditionType) で指定される条件を 1 つ以上指定することができます。 条件がチェックされるのは、トリガーが発生した後です。 バックグラウンド タスクは、キューに入れられますが、必要な条件がすべて満たされるまでは実行されません。

バックグラウンド タスクに条件を設定すると、タスクが不必要に実行されなくなるため、バッテリー残量と CPU 実行時間が節約できます。 たとえば、バックグラウンド タスクがタイマーで実行され、インターネット接続が必要な場合は、タスクを登録する前に、**InternetAvailable** 条件を [**TaskBuilder**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) に追加します。 これにより、タイマーの設定時間が経過し、*かつ*インターネットが利用可能な場合にのみバックグラウンド タスクが実行されるため、タスクがシステム リソースやバッテリー残量を無駄に消費することはなくなります。

また、同じ[**Taskbuilder**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder)で**addcondition**を複数回呼び出すことによって、複数の条件を組み合わせることもできます。 **UserPresent** や **UserNotPresent** など競合する条件を追加しないように注意してください。

## <a name="create-a-systemcondition-object"></a>SystemCondition オブジェクトを作る

ここでは、既にバックグラウンド タスクがアプリと関連付けられており、アプリでは、**taskBuilder** という [**BackgroundTaskBuilder**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) オブジェクトを作るためのコードが記述済みであることを前提とします。  最初にバックグラウンド タスクを作成する必要がある場合は、「[インプロセス バックグラウンド タスクの作成と登録](create-and-register-an-inproc-background-task.md)」または「[アウトプロセス バックグラウンド タスクの作成と登録](create-and-register-a-background-task.md)」をご覧ください。

このトピックの内容は、アウトプロセスで実行されるバックグラウンド タスク、およびフォアグラウンド アプリと同じプロセスで実行されるバックグラウンド タスクに適用されます。

条件を追加する前に、バックグラウンド タスクを実行するために有効にする必要のある条件を表す [**SystemCondition**](/uwp/api/Windows.ApplicationModel.Background.SystemCondition) オブジェクトを作ります。 コンストラクターで、[**SystemConditionType**](/uwp/api/Windows.ApplicationModel.Background.SystemConditionType) 列挙値を渡して、必要な条件を指定します。

次のコードでは、**InternetAvailable** 条件を指定する [**SystemCondition**](/uwp/api/Windows.ApplicationModel.Background.SystemCondition) オブジェクトを作成します。

```csharp
SystemCondition internetCondition = new SystemCondition(SystemConditionType.InternetAvailable);
```

```cppwinrt
Windows::ApplicationModel::Background::SystemCondition internetCondition{
    Windows::ApplicationModel::Background::SystemConditionType::InternetAvailable };
```

```cpp
SystemCondition ^ internetCondition = ref new SystemCondition(SystemConditionType::InternetAvailable);
```

## <a name="add-the-systemcondition-object-to-your-background-task"></a>SystemCondition オブジェクトをバックグラウンド タスクに追加する

条件を追加するには、[**BackgroundTaskBuilder**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) オブジェクトで [**AddCondition**](/uwp/api/windows.applicationmodel.background.backgroundtaskbuilder.addcondition) メソッドを呼び出して、[**SystemCondition**](/uwp/api/Windows.ApplicationModel.Background.SystemCondition) オブジェクトに渡します。

次のコードは、**taskBuilder** を使用して **InternetAvailable** 条件を追加します。

```csharp
taskBuilder.AddCondition(internetCondition);
```

```cppwinrt
taskBuilder.AddCondition(internetCondition);
```

```cpp
taskBuilder->AddCondition(internetCondition);
```

## <a name="register-your-background-task"></a>バックグラウンド タスクを登録する

これで、バックグラウンド タスクを [**Register**](/uwp/api/windows.applicationmodel.background.backgroundtaskbuilder.register) メソッドに登録できます。このバックグラウンド タスクは、指定した条件が満たされるまで、開始されません。

次のコードでは、タスクを登録し、生成される BackgroundTaskRegistration オブジェクトを保存します。

```csharp
BackgroundTaskRegistration task = taskBuilder.Register();
```

```cppwinrt
Windows::ApplicationModel::Background::BackgroundTaskRegistration task{ recurringTaskBuilder.Register() };
```

```cpp
BackgroundTaskRegistration ^ task = taskBuilder->Register();
```

> [!NOTE]
> バックグラウンド タスクの登録パラメーターは登録時に検証されます。 いずれかの登録パラメーターが有効でない場合は、エラーが返されます。 バックグラウンド タスクの登録が失敗するシナリオをアプリが適切に処理するようにします。タスクを登録しようとした後で、有効な登録オブジェクトを持っていることを前提として動作するアプリは、クラッシュする場合があります。

## <a name="place-multiple-conditions-on-your-background-task"></a>バックグラウンド タスクに複数の条件を設定する

複数の条件を追加するには、アプリから [**で**](/uwp/api/windows.applicationmodel.background.backgroundtaskbuilder.addcondition) メソッドを複数回呼び出します。 これらの呼び出しは、タスクの登録を有効にする前に行う必要があります。

> [!NOTE]
> バックグラウンドタスクに競合している条件を追加しないように注意してください。

次のスニペットは、バックグラウンドタスクの作成と登録のコンテキストでの複数の条件を示しています。

```csharp
// Set up the background task.
TimeTrigger hourlyTrigger = new TimeTrigger(60, false);

var recurringTaskBuilder = new BackgroundTaskBuilder();

recurringTaskBuilder.Name           = "Hourly background task";
recurringTaskBuilder.TaskEntryPoint = "Tasks.ExampleBackgroundTaskClass";
recurringTaskBuilder.SetTrigger(hourlyTrigger);

// Begin adding conditions.
SystemCondition userCondition     = new SystemCondition(SystemConditionType.UserPresent);
SystemCondition internetCondition = new SystemCondition(SystemConditionType.InternetAvailable);

recurringTaskBuilder.AddCondition(userCondition);
recurringTaskBuilder.AddCondition(internetCondition);

// Done adding conditions, now register the background task.

BackgroundTaskRegistration task = recurringTaskBuilder.Register();
```

```cppwinrt
// Set up the background task.
Windows::ApplicationModel::Background::TimeTrigger hourlyTrigger{ 60, false };

Windows::ApplicationModel::Background::BackgroundTaskBuilder recurringTaskBuilder;

recurringTaskBuilder.Name(L"Hourly background task");
recurringTaskBuilder.TaskEntryPoint(L"Tasks.ExampleBackgroundTaskClass");
recurringTaskBuilder.SetTrigger(hourlyTrigger);

// Begin adding conditions.
Windows::ApplicationModel::Background::SystemCondition userCondition{
    Windows::ApplicationModel::Background::SystemConditionType::UserPresent };
Windows::ApplicationModel::Background::SystemCondition internetCondition{
    Windows::ApplicationModel::Background::SystemConditionType::InternetAvailable };

recurringTaskBuilder.AddCondition(userCondition);
recurringTaskBuilder.AddCondition(internetCondition);

// Done adding conditions, now register the background task.
Windows::ApplicationModel::Background::BackgroundTaskRegistration task{ recurringTaskBuilder.Register() };
```

```cpp
// Set up the background task.
TimeTrigger ^ hourlyTrigger = ref new TimeTrigger(60, false);

auto recurringTaskBuilder = ref new BackgroundTaskBuilder();

recurringTaskBuilder->Name           = "Hourly background task";
recurringTaskBuilder->TaskEntryPoint = "Tasks.ExampleBackgroundTaskClass";
recurringTaskBuilder->SetTrigger(hourlyTrigger);

// Begin adding conditions.
SystemCondition ^ userCondition     = ref new SystemCondition(SystemConditionType::UserPresent);
SystemCondition ^ internetCondition = ref new SystemCondition(SystemConditionType::InternetAvailable);

recurringTaskBuilder->AddCondition(userCondition);
recurringTaskBuilder->AddCondition(internetCondition);

// Done adding conditions, now register the background task.
BackgroundTaskRegistration ^ task = recurringTaskBuilder->Register();
```

## <a name="remarks"></a>注釈

> [!NOTE]
> 必要なときにのみ実行されるようにバックグラウンドタスクの条件を選択します。そうしないと、実行されません。 バックグラウンド タスクの各条件については、「[**SystemConditionType**](/uwp/api/Windows.ApplicationModel.Background.SystemConditionType)」をご覧ください。

## <a name="related-topics"></a>関連トピック

* [アウトプロセス バックグラウンド タスクの作成と登録](create-and-register-a-background-task.md)
* [インプロセス バックグラウンド タスクの作成と登録](create-and-register-an-inproc-background-task.md)
* [アプリケーション マニフェストでのバックグラウンド タスクの宣言](declare-background-tasks-in-the-application-manifest.md)
* [取り消されたバックグラウンド タスクの処理](handle-a-cancelled-background-task.md)
* [バックグラウンド タスクの進捗状況と完了の監視](monitor-background-task-progress-and-completion.md)
* [バックグラウンド タスクの登録](register-a-background-task.md)
* [バックグラウンド タスクによるシステム イベントへの応答](respond-to-system-events-with-background-tasks.md)
* [バックグラウンド タスクのライブ タイルの更新](update-a-live-tile-from-a-background-task.md)
* [メンテナンス トリガーの使用](use-a-maintenance-trigger.md)
* [タイマーでのバックグラウンド タスクの実行](run-a-background-task-on-a-timer-.md)
* [バックグラウンド タスクのガイドライン](guidelines-for-background-tasks.md)
* [バックグラウンド タスクのデバッグ](debug-a-background-task.md)
* [UWP アプリで一時停止イベント、再開イベント、バックグラウンド イベントをトリガーする方法 (デバッグ時)](/previous-versions/hh974425(v=vs.110))