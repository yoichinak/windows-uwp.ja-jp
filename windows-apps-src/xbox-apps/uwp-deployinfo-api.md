---
title: デバイス ポータル展開情報 API リファレンス
description: Xbox デバイスポータル REST API deployinfo を使用して、インストールされている1つ以上のパッケージの展開情報を要求する方法について説明します。
ms.localizationpriority: medium
ms.topic: article
ms.date: 02/08/2017
ms.openlocfilehash: 2819b21e12d25aca941808e1feeb8a7539750a91
ms.sourcegitcommit: b0a82c2a132212eb5fb72b67f0789cac1014642f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/16/2021
ms.locfileid: "98254227"
---
# <a name="requests-deployment-information-for-one-or-more-installed-packages"></a>1 つ以上のインストール パッケージの展開情報を要求します。

**Request**

| Method | 要求 URI |
|--------|-------------|
| POST | /ext/app/deployinfo |

**URI パラメーター**

 - なし

**要求ヘッダー**

- なし

**要求本文**

次の形式の JSON 配列。

* DeployInfo
  * PackageFullName - 情報を要求するパッケージの名前。
  * OverlayFolder - この機能を使用する場合の、オーバーレイ フォルダーのパスへのオプションのパス。

### <a name="response"></a>Response

**応答本文**

次の形式での JSON 配列 (一部のフィールドは省略可能)。

* DeployInfo
  * PackageFullName - 情報を受け取るパッケージの名前。
  * DeployType - 展開の種類。
  * DeployPathOrSpecifiers - ルーズ展開の展開パスまたはパッケージ化された展開のインストール済み指定子。
  * DeployDrive - 該当する展開の種類のパッケージが展開されているドライブです。
  * DeploySizeInBytes - 該当する展開の種類のパッケージのサイズ (バイト単位) です。
  * OverlayFolder - この機能をサポートする展開のオーバーレイ フォルダーです。

**状態コード**

この API では次の状態コードが返される可能性があります。

| HTTP 状態コード | 説明 |
|------------------|-------------|
| 200 | Success |
| 4XX | エラー コード |
| 5XX | エラー コード |

**利用可能なデバイス ファミリ**

* Windows Xbox