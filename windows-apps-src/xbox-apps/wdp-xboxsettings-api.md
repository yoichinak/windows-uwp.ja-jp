---
title: Device Portal の Xbox 開発者向け設定 API のリファレンス
description: Xbox デバイスポータル REST API を使用して、開発に役立つ Xbox 1 設定にアクセスする方法について説明します。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 6ab12b99-2944-49c9-92d9-f995efc4f6ce
ms.localizationpriority: medium
ms.openlocfilehash: 0aceb7afdce9cc76eab3ee330f0018fdc7ccd1bb
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89168986"
---
# <a name="developer-settings-api-reference"></a>開発者向け設定 API のリファレンス

この API を使用して、開発に役立つ Xbox One 設定にアクセスできます。

## <a name="get-all-developer-settings-at-once"></a>すべての開発者向け設定を一度に取得する

**Request**

次の要求を使用して、すべての開発者向け設定を 1 つの要求で取得できます。

Method      | 要求 URI
:------     | :-----
GET | /ext/settings

**URI パラメーター**

- なし

**要求ヘッダー**

- なし

**要求本文**

- なし

**応答**   
応答は、すべての設定を含む設定の JSON 配列です。 設定オブジェクトには、それぞれ次のフィールドが含まれています。

* Name: (文字列) 設定の名前。
* Value: (文字列) 設定の値。
* RequiresReboot: ("Yes" | "No") このフィールドは、設定を有効にするために再起動する必要があるかどうかを示します。
* Disabled - ("Yes" | "No") このフィールドは、設定が無効であるかどうかと編集不可であるかを示します。
* Category: (文字列) 設定のカテゴリ。
* Type - ("Text" | "Number" | "Bool" | "Select") このフィールドは、設定の型を示します。テキスト入力、ブール値 ("true" または "false")、最小値と最大値を持つ数値、値の特定のリストを持つ選択のいずれかです。

設定が数値の場合:

* Min-(Number) このフィールドは、設定の最小の数値を示します。
* Max-(Number) このフィールドは、設定の数値の最大値を示します。

設定が [選択] の場合:

* オプション変数-("Yes" |"No") このフィールドは、再起動せずに有効なオプションを変更できる場合に、設定オプションが変数かどうかを示します。
* Options - 有効な選択オプションを文字列として含む JSON 配列。

**status code**

この API では次の状態コードが返される可能性があります。

HTTP 状態コード      | 説明
:------     | :-----
200 | 要求は成功しました
4XX | エラー コード
5XX | エラー コード

## <a name="get-settings-one-at-a-time"></a>設定を一度に 1 つ取得する

設定は個別に取得することもできます。

**Request**

次の要求を使って、個別の設定に関する情報を取得できます。

Method      | 要求 URI
:------     | :-----
GET | /ext/\<setting name\>

**URI パラメーター**

- なし

**要求ヘッダー**

- なし

**要求本文**

- なし

**応答**   
応答は、次のフィールドを含む JSON オブジェクトです。

* Name: (文字列) 設定の名前。
* Value: (文字列) 設定の値。
* RequiresReboot: ("Yes" | "No") このフィールドは、設定を有効にするために再起動する必要があるかどうかを示します。
* Disabled - ("Yes" | "No") このフィールドは、設定が無効であるかどうかと編集不可であるかを示します。
* Category: (文字列) 設定のカテゴリ。
* Type - ("Text" | "Number" | "Bool" | "Select") このフィールドは、設定の型を示します。テキスト入力、ブール値 ("true" または "false")、最小値と最大値を持つ数値、値の特定のリストを持つ選択のいずれかです。

設定が数値の場合:

* Min-(Number) このフィールドは、設定の最小の数値を示します。
* Max-(Number) このフィールドは、設定の数値の最大値を示します。

設定が [選択] の場合:

* オプション変数-("Yes" |"No") このフィールドは、再起動せずに有効なオプションを変更できる場合に、設定オプションが変数かどうかを示します。
* Options - 有効な選択オプションを文字列として含む JSON 配列。

**status code**

この API では次の状態コードが返される可能性があります。

HTTP 状態コード      | 説明
:------     | :-----
200 | 要求は成功しました
4XX | エラー コード
5XX | エラー コード

## <a name="set-the-value-of-a-setting"></a>設定の値を設定する

設定の値を設定できます。

**Request**

次の要求を使って、設定の値を設定できます。

Method      | 要求 URI
:------     | :-----
PUT | /ext/\<setting name\>

**URI パラメーター**

- なし

**要求ヘッダー**

- なし

**要求本文**   
要求本文は、次のフィールドを含む JSON オブジェクトです。   
Value: (文字列) 設定の新しい値。

**Response**   

- なし

**status code**

この API では次の状態コードが返される可能性があります。

HTTP 状態コード      | 説明
:------     | :-----
200 | 要求は成功しました
4XX | エラー コード
5XX | エラー コード

**利用可能なデバイス ファミリ**

* Windows Xbox