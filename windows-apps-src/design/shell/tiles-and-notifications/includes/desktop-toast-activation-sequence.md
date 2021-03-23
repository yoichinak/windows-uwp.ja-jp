---
ms.openlocfilehash: 62470070b726aa7101b8299296dc1f4ef67f30e4
ms.sourcegitcommit: 5e718720d1032a7089dea46a7c5aefa6cda3385f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/12/2021
ms.locfileid: "104804356"
---
ユーザーが通知 (または通知のボタン) をクリックすると、次の処理が行われます。

**アプリが現在実行** されている場合...

1. **Toastnotificationmanagercompat. OnActivated 化** イベントがバックグラウンドスレッドで呼び出されます。

**アプリが現在閉じられている場合**...

1. アプリの EXE が起動され、 `ToastNotificationManagerCompat.WasCurrentProcessToastActivated()` 最新のアクティブ化によってプロセスが開始されたこと、およびイベントハンドラーが間もなく呼び出されることを示すために true が返されます。
1. その後、 **Toastnotificationmanagercompat. OnActivated 化** イベントがバックグラウンドスレッドで呼び出されます。