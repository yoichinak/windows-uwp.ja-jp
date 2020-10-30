---
description: イベント ビューアーを使用して、Microsoft テストのイベントとエラーをトラブルシューティングします。
title: イベント ビューアーを使用して、Microsoft テストをトラブルシューティングします。
ms.assetid: 9218e542-f520-4616-98fc-b113d5a08e0f
ms.date: 10/06/2017
ms.topic: article
keywords: windows 10, uwp, 教育
ms.localizationpriority: medium
ms.openlocfilehash: cd30d54f1bff5fd43fbeb6e286e327fed9f8a585
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2020
ms.locfileid: "93031485"
---
# <a name="troubleshoot-microsoft-take-a-test-with-the-event-viewer"></a>イベント ビューアーを使用して、Microsoft テストをトラブルシューティングします

イベント ビューアーを使用して、テストのイベントとエラーを表示することができます。 テストでは、ロックダウン要求を受け取ったとき、デバイスの登録が成功したとき、ロックダウン ポリシーが正常に適用されたときなどに、イベントがログに記録されます。

イベント ビューアーでイベントの表示を有効にするには:
1. を開きます。 `Event Viewer`
2. `Applications and Services Logs > Microsoft > Windows > Management-SecureAssessment` に移動します
3. 右クリック `Operational` して選択 `Enable Log`

イベント ログを保存するには:
1. 右クリック `Operational`
2. [`Save All Events As…`] をクリックします。
