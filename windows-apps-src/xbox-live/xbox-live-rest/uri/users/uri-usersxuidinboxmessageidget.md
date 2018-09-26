---
title: GET (/users/xuid({xuid})/inbox/{messageId})
assetID: d76563d0-2c74-0308-054b-762c80392a02
permalink: en-us/docs/xboxlive/rest/uri-usersxuidinboxmessageidget.html
author: KevinAsgari
description: " GET (/users/xuid({xuid})/inbox/{messageId})"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Xbox Live, Xbox, ゲーム, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 8e94396f86b235aafce2e8a65f93eedbdc96f46b
ms.sourcegitcommit: 232543fba1fb30bb1489b053310ed6bd4b8f15d5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/25/2018
ms.locfileid: "4175464"
---
# <a name="get-usersxuidxuidinboxmessageid"></a>GET (/users/xuid({xuid})/inbox/{messageId})
サービスの読み取りとしてマーク、特定のユーザーのメッセージの詳細なメッセージ テキストを取得します。
これらの Uri のドメインが`msg.xboxlive.com`します。

  * [注釈](#ID4EV)
  * [URI パラメーター](#ID4EEB)
  * [Authorization](#ID4ERB)
  * [要求本文](#ID4E3B)
  * [リソースのプライバシーの設定の効果](#ID4EJC)
  * [HTTP ステータス コード](#ID4EUC)
  * [JavaScript オブジェクト Notation (JSON) の応答](#ID4EUE)

<a id="ID4EV"></a>


## <a name="remarks"></a>注釈

Get 操作は、ユーザー、システム、および FriendRequest メッセージの種類でのみ実行できます。

この URI には、Xbox.com に更新が必要です。 現時点では、まで、ユーザーがサインアウトしたとに戻り、Xbox 360 は読み取り/未読メ状態を更新されません。

この API は、サポートのみコンテンツの種類は、"アプリケーション/json"、呼び出しごとの HTTP ヘッダーのために必要です。

<a id="ID4EEB"></a>


## <a name="uri-parameters"></a>URI パラメーター

| パラメーター| 型| 説明|
| --- | --- | --- |
| xuid | 64 ビットの符号なし整数 | Xbox ユーザー ID (XUID) の要求を行っているプレイヤーです。 |
| メッセージ Id | 文字列 [50] | 取得または削除されるメッセージの ID です。 |

<a id="ID4ERB"></a>


## <a name="authorization"></a>Authorization

独自のユーザーがユーザーのメッセージを取得する要求が必要です。

<a id="ID4E3B"></a>


## <a name="request-body"></a>要求本文

この要求の本文には、オブジェクトは送信されません。

<a id="ID4EJC"></a>


## <a name="effect-of-privacy-settings-on-resource"></a>リソースのプライバシーの設定の効果

だけユーザー メッセージを取得できます。

<a id="ID4EUC"></a>


## <a name="http-status-codes"></a>HTTP ステータス コード

サービスでは、このリソースには、この方法で行った要求に対する応答としてでは、このセクションで、状態コードのいずれかを返します。 Xbox Live サービスで使用される標準の HTTP ステータス コードの一覧は、[標準の HTTP ステータス コード](../../additional/httpstatuscodes.md)を参照してください。

| コード| 説明|
| --- | --- | --- | --- | --- |
| 200| 成功します。|
| 400| XUID を適切に変換することはできません。|
| 403| XUID に変換することはできませんか、有効な XUID クレームが見つかったことはできません。|
| 404| 有効な XUID が見つからないか、またはメッセージ ID が見つからないか、解析されます。|
| 500| サーバー側の一般的なエラー、またはメッセージの種類は、GET に対して有効ではありません。|

<a id="ID4EUE"></a>


## <a name="javascript-object-notation-json-response"></a>JavaScript オブジェクト Notation (JSON) の応答

正常に呼び出されると、サービスは結果データを JSON 形式で返します。 ルート オブジェクトは、UserMessageHeader オブジェクトです。

#### <a name="usermessageheader"></a>UserMessageHeader

| プロパティ| 型| 最大長| 注釈|
| --- | --- | --- | --- |
| header| ヘッダー|  | JSON オブジェクト|
| メッセージ テキスト| string| 256| UTF-8|

#### <a name="header"></a>ヘッダー

| プロパティ| 型| 最大長| 注釈|
| --- | --- | --- | --- |
| 送信| DateTime|  | メッセージが送信された日付と時刻。 (サービスによって提供されます)。|
| 有効期限| DateTime|  | 日付と時刻のメッセージの有効期限が切れます。 (すべてのメッセージによって、決定する、今後の最長有効期間がある)。|
| メッセージの種類| string| 13| メッセージの種類: ユーザー、システム、FriendRequest します。|
| senderXuid| ulong|  | 送信者の XUID です。|
| 送信者| string| 15| 送信者のゲーマータグです。|
| hasAudio| bool|  | かどうか、メッセージは、オーディオ (声) の添付ファイルがあります。|
| hasPhoto| bool|  | かどうか、メッセージは、写真の添付ファイルがあります。|
| hasText| bool|  | かどうか、メッセージには、テキストが含まれています。|

#### <a name="sample-response"></a>応答の例

```cpp
{
          "header":
          {
            "expiration":"2011-10-11T23:59:59.9999999",
            "messageType":"User",
            "senderXuid":"123456789",
            "sender":"Striker",
            "sent":"2011-05-08T17:30:00Z",
            "hasAudio":false,
            "hasPhoto":false,
            "hasText":true
          },
        "messageText":"random user text up to 256 characters"
        }

```

#### <a name="error-response"></a>エラー応答

エラーの場合、サービスはサービスの環境からの値が含まれている全て、errorResponse オブジェクトを返す可能性があります。

| プロパティ| 型| 説明|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| errorSource| string| エラーの発生元の項目を示します。|
| errorCode| int| (Null にすることができます) エラーに関連付けられている数値コードです。|
| エラー メッセージ| string| 詳細を表示するように構成する場合のエラーの説明します。|

<a id="ID4E3DAC"></a>


## <a name="see-also"></a>関連項目

<a id="ID4E5DAC"></a>


##### <a name="parent"></a>Parent  

[/users/xuid({xuid})/inbox](uri-usersxuidinbox.md)


<a id="ID4EMEAC"></a>


##### <a name="reference--standard-http-status-codesadditionalhttpstatuscodesmd"></a>参照[標準の HTTP ステータス コード](../../additional/httpstatuscodes.md)