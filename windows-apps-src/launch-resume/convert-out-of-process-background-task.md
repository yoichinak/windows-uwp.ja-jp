---
title: アウトプロセスのバックグラウンド タスクをインプロセスのバックグラウンド タスクに移植する
description: フォアグラウンドアプリプロセス内で実行されるインプロセスバックグラウンドタスクに、アウトプロセスバックグラウンドタスクを移植します。
ms.date: 09/19/2018
ms.topic: article
keywords: windows 10、uwp、バックグラウンドタスク、app service
ms.assetid: 5327e966-b78d-4859-9b97-5a61c362573e
ms.localizationpriority: medium
ms.openlocfilehash: f500be50c8b57415f5ad2fe62c28cc21f4b57b68
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162656"
---
# <a name="port-an-out-of-process-background-task-to-an-in-process-background-task"></a>アウトプロセスのバックグラウンド タスクをインプロセスのバックグラウンド タスクに移植する

プロセス外 (OOP) のバックグラウンドアクティビティをインプロセスアクティビティに移植する最も簡単な方法は、 [Ibackgroundtask](/uwp/api/windows.applicationmodel.background.ibackgroundtask.run?f=255&MSPPError=-2147217396) を使用することです。アプリケーション内でメソッドコードを実行し、 [Onbackgroundactivated アクティブ](/uwp/api/windows.ui.xaml.application.onbackgroundactivated)にしてから開始します。 ここで説明する手法は、OOP バックグラウンドタスクからインプロセスバックグラウンドタスクに shim を作成することではありません。これは、OOP バージョンをインプロセスバージョンに書き直す (または移植する) ことです。

アプリに複数バックグラウンド タスクがある場合、[バックグラウンドのアクティブ化のサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BackgroundActivation) に、`BackgroundActivatedEventArgs.TaskInstance.Task.Name` を使って開始されるタスクを識別する方法が示されています。

現在、バックグラウンド プロセスとフォアグラウンド プロセスの間で通信している場合、その状態管理および通信コードを削除できます。

## <a name="background-tasks-and-trigger-types-that-cannot-be-converted"></a>変換できないバックグラウンド タスクとトリガーの種類

* インプロセスのバックグラウンド タスクでは、VoIP バックグラウンド タスクのアクティブ化がサポートされていません。
* インプロセス バックグラウンド タスクでは、[DeviceUseTrigger](/uwp/api/windows.applicationmodel.background.deviceusetrigger?f=255&MSPPError=-2147217396)、[DeviceServicingTrigger](/uwp/api/windows.applicationmodel.background.deviceservicingtrigger)、**IoTStartupTask** の各トリガーがサポートされていません。