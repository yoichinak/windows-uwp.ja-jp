---
title: "リッチ プレゼンスのプログラミング"
author: KevinAsgari
description: "Xbox Live メンバーのオンライン プレゼンス状態を設定するコード例について説明します。"
ms.assetid: 7e6e7b69-d7c3-42fa-bcc4-6d68947f6fdb
ms.author: kevinasg
ms.date: 04-04-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "xbox live, xbox, ゲーム, uwp, windows 10, xbox one, リッチ プレゼンス"
ms.openlocfilehash: 2d29f58e0e58c9dbcc0264aea54f35d225d17ece
ms.sourcegitcommit: 90fbdc0e25e0dff40c571d6687143dd7e16ab8a8
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/06/2017
---
# <a name="programming-rich-presence"></a>リッチ プレゼンスのプログラミング

リッチ プレゼンスは、プレイヤーの現在のアクティビティを他のプレイヤーに通知するための機能を提供します。 詳細については、「[リッチ プレゼンス文字列 - 概要](rich-presence-strings-overview.md)」を参照してください。

```cpp

XboxLiveContext^ xboxLiveContext = NULL;

// Set the XboxLiveContext for a user *once* otherwise you may encounter unpredictable behavior.
void OneTimeInit()
{
  // Get the XboxLiveContext.  This should only be done once per user after signing in.
  xboxLiveContext = ref new Microsoft::Xbox::Services::XboxLiveContext(User::Users->GetAt(0));
}

void Example_PresenceService_SetPresenceAsync()
{
    // The config ID below is an example.
    // The real ID to use is defined when you set up the game on Xbox Development Portal.
    String^ aServiceConfigurationId = "C1BA92A9-0000-0000-0000-000000000000";

    PresenceData^ presenceData = ref new PresenceData(
        aServiceConfigurationId,
        "mainMenuPresence"
        );

    IAsyncAction^ setPresenceAction = xboxLiveContext->PresenceService->SetPresenceAsync(
        xboxLiveContext->User->XboxLiveId,
        true, // isUserActive
        presenceData    // richPresenceData is optional
        );

    create_task( setPresenceAction )
    .then([] ()
    {
        LogComment( L"SetPresenceAsync Done" );
    })
    .wait();
}
```