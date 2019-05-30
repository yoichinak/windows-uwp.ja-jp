---
title: アウトプロセスのバックグラウンド タスクをインプロセスのバックグラウンド タスクに移植する
description: フォア グラウンド アプリのプロセス内で実行されるプロセス内のバック グラウンド タスクに、プロセス外のバック グラウンド タスクを移植します。
ms.date: 09/19/2018
ms.topic: article
keywords: アプリ サービス、windows 10、uwp、バック グラウンド タスク
ms.assetid: 5327e966-b78d-4859-9b97-5a61c362573e
ms.localizationpriority: medium
ms.openlocfilehash: 42aaa5600b30924acc84aa61c2a15dbb2a320c10
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/29/2019
ms.locfileid: "66366259"
---
# <a name="port-an-out-of-process-background-task-to-an-in-process-background-task"></a>アウトプロセスのバックグラウンド タスクをインプロセスのバックグラウンド タスクに移植する

プロセス アクティビティには、アウト プロセス (OOP) バック グラウンド アクティビティを移植する最も簡単な方法を取り込む、 [IBackgroundTask.Run](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtask.run?f=255&MSPPError=-2147217396)メソッドは、アプリケーション内のコードし、開始から[OnBackgroundActivated](/uwp/api/windows.ui.xaml.application.onbackgroundactivated). ここで説明されている手法が、OOP のバック グラウンド タスクから、プロセス内のバック グラウンド タスクに shim を作成する方法について書き換え (または移植) をプロセス内のバージョンに、OOP バージョン。

アプリに複数バックグラウンド タスクがある場合、[バックグラウンドのアクティブ化のサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BackgroundActivation) に、`BackgroundActivatedEventArgs.TaskInstance.Task.Name` を使って開始されるタスクを識別する方法が示されています。

現在、バックグラウンド プロセスとフォアグラウンド プロセスの間で通信している場合、その状態管理および通信コードを削除できます。

## <a name="background-tasks-and-trigger-types-that-cannot-be-converted"></a>変換できないバックグラウンド タスクとトリガーの種類

* インプロセスのバックグラウンド タスクでは、VoIP バックグラウンド タスクのアクティブ化がサポートされていません。
* プロセス内のバック グラウンド タスクは、次のトリガーをサポートしません。[DeviceUseTrigger](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.deviceusetrigger?f=255&MSPPError=-2147217396)、 [DeviceServicingTrigger](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.deviceservicingtrigger)と**IoTStartupTask**
