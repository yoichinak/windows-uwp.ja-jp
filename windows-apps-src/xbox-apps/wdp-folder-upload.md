---
title: Device Portal のフォルダー アップロード API のリファレンス
description: Xbox デバイスポータル REST API を使用して、開発ディレクトリにフォルダーをアップロードする方法について説明します。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: e1a2c7f0-0040-4ce7-94de-17224736e20b
ms.localizationpriority: medium
ms.openlocfilehash: b71f60350bf5c8318adb2a4741bb1a275a4b0276
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89157756"
---
# <a name="upload-a-folder-to-the-development-directory"></a>開発ディレクトリにフォルダーをアップロードする

**Request**

フォルダー全体を DevelopmentFiles の既知のフォルダー ID (またはそのフォルダーのサブフォルダー) に一度にアップロードできます。

Method      | 要求 URI
:------     | :------
POST | /api/app/packagemanager/upload 

**URI パラメーター**

次の追加パラメーターを要求 URI に指定できます。

URI パラメーター      | 説明
:------     | :-----
destinationFolder (必須) | アップロードするフォルダーのターゲット フォルダー名です。 このフォルダーは、本体の d:\developmentfiles\LooseApps に配置されます。 フォルダーが LooseApps の下のサブフォルダーである場合、フォルダー名にパスの区切り文字が含まれる可能性があるため、このフォルダー名は base64 でエンコードされている必要があります。


**要求ヘッダー**

- なし

**要求本文**

- ディレクトリ コンテンツの原則に従ったマルチパートの http 本文。

**Response**

**status code**

この API では次の状態コードが返される可能性があります。

HTTP 状態コード      | 説明
:------     | :-----
200 | Success
4XX | エラー コード
5XX | エラー コード

**利用可能なデバイス ファミリ**

* Windows Xbox

