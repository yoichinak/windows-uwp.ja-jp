---
title: デバイス ポータル展開情報 API リファレンス
description: Xbox デバイスポータル REST API deployinfo を使用して、インストールされている1つ以上のパッケージの展開情報を要求する方法について説明します。
ms.localizationpriority: medium
ms.topic: article
ms.date: 02/08/2017
ms.openlocfilehash: 5260125625ced6c258a683bcfb9b552e57d07f06
ms.sourcegitcommit: 8e0e4cac79554e86dc7f035c4b32cb1f229142b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/26/2020
ms.locfileid: "88943002"
---
# <a name="requests-deployment-information-for-one-or-more-installed-packages"></a>1 つ以上のインストール パッケージの展開情報を要求します。

**Request**

Method      | 要求 URI
:------     | :------
POST | /ext/app/deployinfo
<br />
**URI パラメーター**

 - なし

**要求ヘッダー**

- なし

**要求本文**

次の形式の JSON 配列。

* DeployInfo
  * PackageFullName - 情報を要求するパッケージの名前。
  * OverlayFolder - この機能を使用する場合の、オーバーレイ フォルダーのパスへのオプションのパス。

###<a name="response"></a>応答

**応答本文**

次の形式での JSON 配列 (一部のフィールドは省略可能)。

* DeployInfo
  * PackageFullName - 情報を受け取るパッケージの名前。
  * DeployType - 展開の種類。
  * DeployPathOrSpecifiers - ルーズ展開の展開パスまたはパッケージ化された展開のインストール済み指定子。
  * DeployDrive - 該当する展開の種類のパッケージが展開されているドライブです。
  * DeploySizeInBytes - 該当する展開の種類のパッケージのサイズ (バイト単位) です。
  * OverlayFolder - この機能をサポートする展開のオーバーレイ フォルダーです。

**status code**

この API では次の状態コードが返される可能性があります。

HTTP 状態コード      | 説明
:------     | :-----
200 | Success
4XX | エラー コード
5XX | エラー コード
<br />

**利用可能なデバイス ファミリ**

* Windows Xbox