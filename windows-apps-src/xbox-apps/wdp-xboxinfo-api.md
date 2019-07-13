---
title: デバイス ポータル Xbox 情報 API リファレンス
description: Xbox デバイス情報にアクセスする方法について説明します。
ms.date: 04/18/2019
ms.topic: article
keywords: windows 10、uwp、xbox、デバイス ポータル
ms.localizationpriority: medium
ms.openlocfilehash: c6a8e595be9a0846df2af81ea0b7fc1605f62e5f
ms.sourcegitcommit: 139717a79af648a9231821bdfcaf69d8a1e6e894
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/10/2019
ms.locfileid: "67714076"
---
# <a name="xbox-info-api-reference"></a>Xbox 情報 API リファレンス   
この API を使うと、Xbox One デバイス情報にアクセスすることができます。

## <a name="get-xbox-one-device-information"></a>Xbox One デバイス情報を取得する

## <a name="request"></a>要求

Xbox One に関するデバイス情報を取得できます。

メソッド      | 要求 URI
:------     | :-----
GET | /ext/xbox/info

**URI パラメーター**

- なし

**要求ヘッダー**

- なし

**要求本文**

- なし

## <a name="response"></a>応答
次のフィールドを含む JSON オブジェクト。

* OsVersion - (文字列) OS のバージョン。
* OsEdition - (文字列) OS のエディション ("March 2017" や "March 2017 QFE 1" など)。
* ConsoleId - (文字列) コンソールの ID。
* DeviceId - (文字列) コンソールの Xbox Live デバイス ID。
* SerialNumber - (文字列) コンソールのシリアル番号。
* DevMode - (文字列) コンソールの現在の開発者モード ("None" や "Retail" など)。
* ConsoleType - (文字列) コンソールの種類 ("Xbox One" や "Xbox One S" など)。
* DevkitCertificateExpirationTime - (数値) コンソールの開発者キット証明書の有効期限が切れる UTC 時刻 (秒単位)。

**状態コード**

この API では次の状態コードが返される可能性があります。

HTTP 状態コード      | 説明
:------     | :-----
200 | 要求は成功しました
4XX | エラー コード
5XX | エラー コード

**使用可能なデバイス ファミリ**

* Windows Xbox
