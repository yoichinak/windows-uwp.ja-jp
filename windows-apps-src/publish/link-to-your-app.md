---
Description: You can help customers discover your app by linking to your app's listing in the Microsoft Store.
title: アプリへのリンク
ms.assetid: 5420B65C-7ECE-4364-8959-D1683684E146
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, リンク, windows store プロトコル, アプリにリンクする, アプリへのリンク
ms.localizationpriority: medium
ms.openlocfilehash: de22505cf42193932a5bbd951c983e02eea37bd7
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259987"
---
# <a name="link-to-your-app"></a>アプリへのリンク


You can help customers discover your app by linking to your app's listing in the Microsoft Store.

## <a name="getting-the-link-to-your-apps-store-listing"></a>ストアのアプリの内容へのリンク

アプリのストア登録情報の URL を取得するには、アプリの **[アプリ管理]** セクションの [[アプリ ID]](view-app-identity-details.md) ページに移動します。 URL の形式は **`https://www.microsoft.com/store/apps/<your app's Store ID>`** です。

ユーザーがこのリンクをクリックすると、アプリの Web ベースの登録情報ページが開きます。 Windows デバイスでは、ストア アプリも起動して、アプリの登録情報を表示します。


## <a name="linking-to-your-apps-store-listing-with-the-microsoft-store-badge"></a>Linking to your app's Store listing with the Microsoft Store badge

You can link directly to your app's listing with a custom badge to let customers know your app is in the Microsoft Store.

To create your badge, visit the [Microsoft Store badges](https://developer.microsoft.com/store/badges) page. バッジとリンクを生成するには、アプリの 12 文字の**ストア ID** が必要です。 アプリの**ストア ID** は、 **[アプリ管理]** セクションの [[アプリ ID]](view-app-identity-details.md) ページで確認できます。

> [!NOTE]
> See [App marketing guidelines](app-marketing-guidelines.md) for info and requirements related to your use of the Microsoft Store badge.


## <a name="linking-directly-to-your-app-in-the-microsoft-store"></a>Linking directly to your app in the Microsoft Store

You can create a link that launches the Microsoft Store and goes directly to your app's listing page without opening a browser by using the **ms-windows-store:** URI scheme.

ユーザーが Windows デバイスを使っていることがわかっていて、ストアの登録情報ページにユーザーが直接アクセスできるようにする場合は、このリンクが便利です。 たとえば、ブラウザーのユーザー エージェント文字列を調べてユーザーのオペレーティング システムがストアをサポートしていることを確認した後や、既に UWP アプリを使って通信している場合に、このリンクを利用できます。

To use this URI scheme to link directly to your app's Store listing, append your app's Store ID to this link:

`ms-windows-store://pdp/?ProductId=`

For more about using the Microsoft Store protocol, see [Launch the Microsoft app](../launch-resume/launch-store-app.md).

 

 




