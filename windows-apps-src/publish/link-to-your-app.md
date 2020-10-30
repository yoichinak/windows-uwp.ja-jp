---
description: Microsoft Store のアプリの一覧にリンクすることで、お客様がアプリを見つけられるようにすることができます。
title: アプリへのリンク
ms.assetid: 5420B65C-7ECE-4364-8959-D1683684E146
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, リンク, windows store プロトコル, アプリにリンクする, アプリへのリンク
ms.localizationpriority: medium
ms.openlocfilehash: 916f82714feb65e3d9d4db48703c831b8128021c
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2020
ms.locfileid: "93033915"
---
# <a name="link-to-your-app"></a>アプリへのリンク


Microsoft Store のアプリの一覧にリンクすることで、お客様がアプリを見つけられるようにすることができます。

## <a name="getting-the-link-to-your-apps-store-listing"></a>アプリのストア登録情報へのリンク

アプリのストア登録情報の URL を取得するには、アプリの **[アプリ管理]** セクションの [[アプリ ID]](view-app-identity-details.md) ページに移動します。 URL の形式は **`https://www.microsoft.com/store/apps/<your app's Store ID>`** です。

ユーザーがこのリンクをクリックすると、アプリの Web ベースの登録情報ページが開きます。 Windows デバイスでは、ストア アプリも起動して、アプリの登録情報を表示します。


## <a name="linking-to-your-apps-store-listing-with-the-microsoft-store-badge"></a>Microsoft Store バッジを使用したアプリのストアの一覧へのリンク

カスタムバッジを使ってアプリの一覧に直接リンクして、お客様にアプリが Microsoft Store にあることを知らせることができます。

バッジを作成するには、 [Microsoft Store バッジ](https://developer.microsoft.com/store/badges) のページにアクセスしてください。 バッジとリンクを生成するには、アプリの 12 文字の **ストア ID** が必要です。 アプリの **ストア ID** は、 **[アプリ管理]** セクションの [[アプリ ID]](view-app-identity-details.md) ページで確認できます。

> [!NOTE]
> Microsoft Store バッジの使用に関する情報と要件については、「 [アプリマーケティングのガイドライン](app-marketing-guidelines.md) 」を参照してください。


## <a name="linking-directly-to-your-app-in-the-microsoft-store"></a>Microsoft Store でアプリに直接リンクする

Microsoft Store を起動するリンクを作成し、 **ms windows ストア:** URI スキームを使用してブラウザーを開かずに、アプリの一覧ページに直接移動することができます。

ユーザーが Windows デバイスを使っていることがわかっていて、ストアの登録情報ページにユーザーが直接アクセスできるようにする場合は、このリンクが便利です。 たとえば、ブラウザーのユーザー エージェント文字列を調べてユーザーのオペレーティング システムがストアをサポートしていることを確認した後や、既に UWP アプリを使って通信している場合に、このリンクを利用できます。

この URI スキームを使用してアプリのストアの一覧に直接リンクするには、アプリのストア ID をこのリンクに追加します。

`ms-windows-store://pdp/?ProductId=`

Microsoft Store プロトコルの使用の詳細については、「 [Microsoft アプリを起動する](../launch-resume/launch-store-app.md)」を参照してください。

 

 




