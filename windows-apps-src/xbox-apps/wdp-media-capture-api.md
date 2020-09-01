---
title: メディア キャプチャ API のリファレンス
description: Xbox デバイスポータル REST API を使用して、現在の画面の PNG 表現をキャプチャする方法について説明します。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 3f92c8fd-4096-4972-97da-01ae5db6423c
ms.localizationpriority: medium
ms.openlocfilehash: ee1ccba3fe2a3f83a95c3538cb267730f7770c4c
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172736"
---
# <a name="media-capture-api-reference"></a>メディア キャプチャ API のリファレンス #

## <a name="request"></a>Request

次の要求形式を使用して、現在の画面の PNG 画像を取得できます。

| Method        | 要求 URI     | 
| ------------- |-----------------|
| GET           | /ext/screenshot |


**URI パラメーター**

次の追加パラメーターを要求 URI に指定できます。


| URI パラメーター      | 説明     | 
| ------------------ |-----------------|
| download (省略可能)| ホスト ブラウザーでスクリーンショットをブラウザーにレンダリングするのではなく添付ファイルとしてダウンロードする必要があることを、HTTP 応答ヘッダーで設定する必要があるかどうかを示すブール値。  |

**要求ヘッダー**

* なし

**要求本文**

* なし

## <a name="response"></a>応答

**status code**

この API では次の状態コードが返される可能性があります。

| HTTP 状態コード   | 説明     | 
| ------------------ |-----------------|
| 200                | スクリーンショットの要求が成功し、キャプチャが返される |
| 5XX                | 予期しないエラーのエラー コード |
<br>

**利用可能なデバイス ファミリ**

* Windows Xbox

