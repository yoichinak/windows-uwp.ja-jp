---
title: "構成 BLOB の読み取り"
author: KevinAsgari
description: "Xbox Live タイトル ストレージ内の構成 BLOB の読み取りについて説明します。"
ms.assetid: ee62d221-69b9-4f52-9b5d-5a44d04de548
ms.author: kevinasg
ms.date: 04-04-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "xbox live, xbox, ゲーム, uwp, windows 10, xbox one"
ms.openlocfilehash: 8b917d1aa65f7c1706c99d893c3dbc6b03aa7643
ms.sourcegitcommit: 90fbdc0e25e0dff40c571d6687143dd7e16ab8a8
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/06/2017
---
# <a name="reading-a-configuration-blob-in-xbox-live-title-storage"></a>Xbox Live タイトル ストレージ内の構成 BLOB の読み取り

*構成 BLOB* は、ゲーム データを保持している Xbox Live タイトル ストレージ内のファイルです。 データは仮想ノードを含む JSON オブジェクトであり、ゲームに配信される前にフィルターを適用できます。 構成 BLOB の詳細については、「**タイトル ストレージ URI**」を参照してください。

### <a name="to-read-a-configuration-blob"></a>構成 BLOB を読み取るには

1.  タイトル ストレージからデータを読み取るには、次のメソッドを使用して要求を送信します。

        GET https://titlestorage.xboxlive.com/global/scids/{scid}/data/config.json,config              
        Content-Type: application/octet-stream
        x-xbl-contract-version: 1
        Authorization: XBL3.0 x=<userHash>;<STSTokenString>
        Connection: Keep-Alive


-   更新するには、ユーザーはそのセッション内にいなければなりません。
-   STSTokenString は、簡潔にするためのプレースホルダーであり、認証要求から返されるトークンで置き換える必要があります。

#### <a name="reference"></a>参照先

**/global/scids/{scid}/data/{pathAndFileName},{type}**
**GET (/global/scids/{scid}/data/{pathAndFileName},{type})**