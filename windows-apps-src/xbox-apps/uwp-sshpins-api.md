---
title: Device Portal の SSH ピン API のリファレンス
description: /Ext/app/sshpins Xbox デバイスポータル REST API を使用して、すべての信頼された Secure Shell (SSH) pin をプログラムから削除する方法について説明します。
ms.localizationpriority: medium
ms.topic: article
ms.date: 02/08/2017
ms.openlocfilehash: 307af4cdd4e998832f4a2fe7a8f874615fe10ad7
ms.sourcegitcommit: cb5af00af05e838621c270173e7fde1c5d2168ef
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/28/2020
ms.locfileid: "89043534"
---
# <a name="ssh-pins-api-reference"></a>SSH ピン API リファレンス
この REST API を使用して、開発キットで信頼されているすべての SSH ピンを削除することができます。

## <a name="remove-trusted-ssh-pins"></a>信頼されている SSH ピンの削除

**Request**

Method      | 要求 URI
:------     | :-----
DELETE | /ext/app/sshpins
<br />
**URI パラメーター**

- なし

**要求ヘッダー**

- なし

**要求本文**   

- なし

**Response**   

- なし 

**status code**

この API では次の状態コードが返される可能性があります。

HTTP 状態コード      | 説明
:------     | :-----
204 | ピンをクリアする要求が成功しました。
4XX | エラー コード
5XX | エラー コード

<br />
**利用可能なデバイス ファミリ**

* Windows Xbox

