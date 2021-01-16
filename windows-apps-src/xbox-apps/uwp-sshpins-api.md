---
title: Device Portal の SSH ピン API のリファレンス
description: /Ext/app/sshpins Xbox デバイスポータル REST API を使用して、すべての信頼された Secure Shell (SSH) pin をプログラムから削除する方法について説明します。
ms.localizationpriority: medium
ms.topic: article
ms.date: 02/08/2017
ms.openlocfilehash: 999a77051b0eebe6ae5d8be9e4c2640709431b6d
ms.sourcegitcommit: b0a82c2a132212eb5fb72b67f0789cac1014642f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/16/2021
ms.locfileid: "98254207"
---
# <a name="ssh-pins-api-reference"></a>SSH ピン API リファレンス

この REST API を使用して、開発キットで信頼されているすべての SSH ピンを削除することができます。

## <a name="remove-trusted-ssh-pins"></a>信頼されている SSH ピンの削除

**Request**

| Method | 要求 URI |
|--------|-------------|
| DELETE | /ext/app/sshpins |

**URI パラメーター**

- なし

**要求ヘッダー**

- なし

**要求本文**

- なし

**Response**

- なし

**状態コード**

この API では次の状態コードが返される可能性があります。

| HTTP 状態コード | 説明 |
|------------------|-------------|
| 204 | ピンをクリアする要求が成功しました。 |
| 4XX | エラー コード |
| 5XX | エラー コード |

**利用可能なデバイス ファミリ**

* Windows Xbox
