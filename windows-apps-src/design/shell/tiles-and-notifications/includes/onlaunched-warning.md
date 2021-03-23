---
ms.openlocfilehash: 9508a15492b2b2d6a27d042ddea323f142eb8103
ms.sourcegitcommit: 5e718720d1032a7089dea46a7c5aefa6cda3385f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/12/2021
ms.locfileid: "104804351"
---
> [!IMPORTANT]
> **OnLaunched** コードと同様に、フレームを初期化してウィンドウをアクティブ化する必要があります。 **OnLaunched は、ユーザーがトーストをクリックしても呼び出されません**。アプリが閉じられてから初めて起動している場合も同様です。 通常は、**OnLaunched** と **OnActivated** を組み合わせて独自の `OnLaunchedOrActivated` メソッドにまとめることをお勧めします。これは、両方で同じ初期化を実行する必要があるためです。