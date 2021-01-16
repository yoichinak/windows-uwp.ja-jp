---
title: メディア キャプチャ API のリファレンス
description: Xbox デバイスポータル REST API を使用して、現在の画面の PNG 表現をキャプチャする方法について説明します。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 3f92c8fd-4096-4972-97da-01ae5db6423c
ms.localizationpriority: medium
ms.openlocfilehash: 5d5cd87b95f923fc0303d5cd4ed7ecbff70b8209
ms.sourcegitcommit: b0a82c2a132212eb5fb72b67f0789cac1014642f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/16/2021
ms.locfileid: "98254197"
---
# <a name="media-capture-api-reference"></a>メディア キャプチャ API のリファレンス

## <a name="request"></a>Request

次の要求形式を使用して、現在の画面の PNG 画像を取得できます。

| Method        | 要求 URI     |
| ------------- |-----------------|
| GET           | /ext/screenshot |

**URI パラメーター**

次の追加パラメーターを要求 URI に指定できます。

| URI パラメーター       | 説明     |
| ------------------- |-----------------|
| download (省略可能) | ホスト ブラウザーでスクリーンショットをブラウザーにレンダリングするのではなく添付ファイルとしてダウンロードする必要があることを、HTTP 応答ヘッダーで設定する必要があるかどうかを示すブール値。 |

**要求ヘッダー**

* なし

**要求本文**

* なし

## <a name="response"></a>Response

**状態コード**

この API では次の状態コードが返される可能性があります。

| HTTP 状態コード   | 説明     |
| ------------------ |-----------------|
| 200                | スクリーンショットの要求が成功し、キャプチャが返される |
| 5XX                | 予期しないエラーのエラー コード |

**利用可能なデバイス ファミリ**

* Windows Xbox
