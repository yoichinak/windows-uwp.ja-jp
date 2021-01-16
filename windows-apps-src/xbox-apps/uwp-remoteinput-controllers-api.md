---
title: デバイス ポータル コントローラー API リファレンス
description: 接続された物理コントローラーの数を取得し、プログラムでオフにする方法について説明します。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 535846d7dbeb2d29328b5c5d01b06d4449a53790
ms.sourcegitcommit: b0a82c2a132212eb5fb72b67f0789cac1014642f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/16/2021
ms.locfileid: "98254187"
---
# <a name="controller-api-reference"></a>コントローラー API のリファレンス

接続された物理コントローラーの数を取得し、REST API を使用してオフにすることができます。

## <a name="determine-the-number-of-attached-physical-controllers"></a>接続された物理コントローラーの数の決定

**Request**

次の要求を使用して、デバイス上に接続された物理コントローラーの数を確認することができます。

Method | 要求 URI |
-------|-------------|
| GET | /ext/remoteinput/controllers |

**URI パラメーター**

- なし

**要求ヘッダー**

- なし

**要求本文**   

- なし

**Response**   

- 接続された物理コントローラーの数を指定する JSON 数値プロパティ ConnectedControllerCount です。

**状態コード**

この API では次の状態コードが返される可能性があります。

| HTTP 状態コード | 説明 |
|------------------|-------------|
| 200 | Success |
| 4XX | エラー コード |
| 5XX | エラー コード |

## <a name="disconnect-all-physical-controllers-on-the-devkit"></a>開発キット上のすべての物理コントローラーの切断

**Request**

次の要求を使って、デバイス上のすべての物理コントローラーを切断することができます。

| Method | 要求 URI |
|--------|-------------|
| DELETE | /ext/remoteinput/controllers |

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
| 204 | コントローラーを接続する要求が成功しました。 |
| 4XX | エラー コード |
| 5XX | エラー コード |

**利用可能なデバイス ファミリ**

* Windows Xbox
