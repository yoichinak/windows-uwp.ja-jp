---
title: Device Portal の SMB API のリファレンス
description: Xbox デバイスポータル REST API/ext/smb/developerfolder を使用して、エクスプローラーを使用して Xbox の1つのコンソールで developer フォルダーにアクセスする方法について説明します。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 1f0eb76e-fe3e-4674-a27e-229beec7e63d
ms.localizationpriority: medium
ms.openlocfilehash: 80a49d324c27754a2686ba4d954b47e7529df330
ms.sourcegitcommit: eb725a47c700131f5975d737bd9d8a809e04943b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/27/2020
ms.locfileid: "88970210"
---
# <a name="developer-folder-api-reference"></a>開発者向けフォルダー API のリファレンス

標準的なエクスプローラーを使って、Xbox One の開発に関連するファイルにアクセスできます。 これにより、ファイルを簡単に表示したり、PC から本体に置き換えることができます。

**Request**

次の要求を使用して、開発者向けフォルダーにアクセスできます。 要求によって次の情報が返されます。

* ファイル共有の場所。 この場所は、エクスプローラーのアドレス バーに入力できます。
* ファイル共有へのアクセスで使うユーザー名。
* ファイル共有へのアクセスで使うパスワード。

Method      | 要求 URI
:------     | :-----
GET | /ext/smb/developerfolder

**URI パラメーター**

- なし

**要求ヘッダー**

- なし

**要求本文**

- なし

**応答**   
Path: 開発者向けファイル共有にあるファイルのパス。   
Username: 開発者向けファイル共有にアクセスするために必要なユーザー名。   
Password: 開発者向けファイル共有にアクセスするために必要なパスワード。   

**status code**

この API では次の状態コードが返される可能性があります。

HTTP 状態コード      | 説明
:------     | :-----
200 | ファイル共有の資格情報にアクセスする要求が許可されました。
4XX | エラー コード
5XX | エラー コード

**利用可能なデバイス ファミリ**

* Windows Xbox
