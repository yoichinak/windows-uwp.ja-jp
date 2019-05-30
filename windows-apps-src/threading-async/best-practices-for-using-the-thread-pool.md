---
ms.assetid: 95CF7F3D-9E3A-40AC-A083-D8A375272181
title: スレッド プールを使うためのベスト プラクティス
description: このトピックでは、スレッド プールを使った操作のベスト プラクティスについて説明します。
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10、UWP、スレッド、スレッド プール
ms.localizationpriority: medium
ms.openlocfilehash: a498f685e7a810d19e2f1eb63ae112dd02587b84
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370695"
---
# <a name="best-practices-for-using-the-thread-pool"></a>スレッド プールを使うためのベスト プラクティス

このトピックでは、スレッド プールを使った操作のベスト プラクティスについて説明します。

## <a name="dos"></a>推奨


-   スレッド プールを使って、アプリの並列処理を実行します。

-   作業項目を使って、UI スレッドをブロックせずに広範なタスクを実行します。

-   有効期間が短く独立した作業項目を作成します。 作業項目は非同期に実行され、キューから任意の順番でプールに送ることができます。

-   [  **Windows.UI.Core.CoreDispatcher**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreDispatcher) を使って更新を UI スレッドにディスパッチします。

-   **Sleep** 関数ではなく [**ThreadPoolTimer.CreateTimer**](https://docs.microsoft.com/uwp/api/windows.system.threading.threadpooltimer.createtimer) 関数を使います。

-   独自のスレッド管理システムを作成するのではなく、スレッド プールを使います。 スレッド プールは、OS レベルで高度な機能を使って実行され、プロセス内およびシステム全体のデバイス リソースとアクティビティに従って動的にスケーリングされるように最適化されています。

-   C++ の場合、作業項目委任でアジャイル スレッド モデルを使うようにします (C++ 委任は既定でアジャイルです)。

-   使用の時点でリソース割り当ての失敗を許容できない場合は、あらかじめ割り当てられた作業項目を使用します。

## <a name="donts"></a>非推奨


-   *period* の値が 1 ミリ秒未満 (0 秒を含む) の定期タイマーを作成しないでください。 この場合、作業項目は 1 回限りのタイマーとして動作します。

-   *period* パラメーターで指定した時間よりも完了までに時間がかかる定期的な作業項目を送信しないでください。

-   バックグラウンド タスクからディスパッチされている作業項目から、UI 更新 (トーストと通知を除く) を送信しないでください。 代わりに、バックグラウンド タスクの進行ハンドラーと完了ハンドラー ([**IBackgroundTaskInstance.Progress**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtaskinstance.progress) など) を使います。

-   **async** キーワードを使用する作業項目ハンドラーを使用する場合は、ハンドラーのすべてのコードの実行が完了する前にスレッド プール作業項目が完了状態に設定される可能性があることに注意してください。 ハンドラー内の **await** キーワードに続くコードは、作業項目が完了状態に設定された後で実行される可能性があります。

-   あらかじめ割り当てられた作業項目を複数回実行する場合は、1 回実行するたびに再初期化してください。 [定期的な作業項目の作成](create-a-periodic-work-item.md)

## <a name="related-topics"></a>関連トピック


* [定期的な作業項目の作成](create-a-periodic-work-item.md)
* [スレッド プールへの作業項目の送信](submit-a-work-item-to-the-thread-pool.md)
* [タイマーを使った作業項目の送信](use-a-timer-to-submit-a-work-item.md)
