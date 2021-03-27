---
ms.assetid: c5246681-82c7-44df-87e1-a84a926e6496
description: プロモーション用の広告キャンペーンのクリエイティブを管理するには、Microsoft Store プロモーション API の以下のメソッドを使います。
title: クリエイティブの管理
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store プロモーション API, 広告キャンペーン
ms.localizationpriority: medium
ms.openlocfilehash: 90a186a63104c0393cb871ceffee023a05a7d514
ms.sourcegitcommit: 80ea62d6c0ee25d73750437fe1e37df5224d5797
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/26/2021
ms.locfileid: "105619638"
---
# <a name="manage-creatives"></a>クリエイティブの管理

プロモーション用の広告キャンペーンで使う独自のカスタム クリエイティブをアップロードしたり、既存のクリエイティブを取得したりするには、Microsoft Store プロモーション API の以下のメソッドを使います。 クリエイティブは、常に同じアプリを表す場合は、同一の広告キャンペーンでなくても、1 つ以上の配信ラインに関連付けることができます。

クリエイティブと広告キャンペーン、配信ライン、ターゲット プロファイルとの関係について詳しくは、「[Microsoft Store サービスを使用した広告キャンペーンの実行](run-ad-campaigns-using-windows-store-services.md#call-the-windows-store-promotions-api)」をご覧ください。

> [!NOTE]
> この API を使って独自のクリエイティブをアップロードする場合、クリエイティブの最大許容サイズは 40 KB です。 これよりも大きいクリエイティブ ファイルを送信しても、この API からはエラーが返されず、キャンペーンは正しく作成されません。

## <a name="prerequisites"></a>前提条件

これらのメソッドを使うには、最初に次の作業を行う必要があります。

* Microsoft Store プロモーション API に関するすべての[前提条件](run-ad-campaigns-using-windows-store-services.md#prerequisites)を満たします (前提条件がまだ満たされていない場合)。
* これらのメソッドの要求ヘッダーで使う [Azure AD アクセス トークンを取得](run-ad-campaigns-using-windows-store-services.md#obtain-an-azure-ad-access-token)します。 アクセス トークンを取得した後、アクセス トークンを使用できるのは、その有効期限が切れるまでの 60 分間です。 トークンの有効期限が切れたら新しいトークンを取得できます。


## <a name="request"></a>要求

これらのメソッドでは、次の URL が使用されます。

| メソッドの型 | 要求 URI     |  説明  |
|--------|-----------------------------|---------------|
| POST   | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/creative``` |  新しいクリエイティブを作成します。  |
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/creative/{creativeId}``` |  *creativeId* で指定されたクリエイティブを取得します。  |

> [!NOTE]
> 現在、この API は PUT メソッドをサポートしていません。


### <a name="header"></a>ヘッダー

| ヘッダー        | Type   | 説明         |
|---------------|--------|---------------------|
| 承認 | string | 必須。 ベアラートークン形式の Azure AD アクセストークン &lt;  &gt; 。 |
| 追跡 ID   | GUID   | 省略可能。 呼び出しフローを追跡する ID。                                  |


### <a name="request-body"></a>要求本文

POST メソッドには、[クリエイティブ](#creative) オブジェクトの必須フィールドを含む JSON 要求本文が必要です。


### <a name="request-examples"></a>要求の例

次の例は、POST メソッドを呼び出してクリエイティブを作成する方法を示しています。 この例では、簡潔にするために *content* 値が短縮されています。

```json
POST https://manage.devcenter.microsoft.com/v1.0/my/promotion/creative HTTP/1.1
Authorization: Bearer <your access token>

{
  "name": "Contoso App Campaign - Creative 1",
  "content": "data:image/jpeg;base64,/9j/4AAQSkZJRgABAQEAAQABAAD/2wBDAAgGB...other base64 data shortened for brevity...",
  "height": 80,
  "width": 480,
  "imageAttributes":
  {
    "imageExtension": "PNG"
  }
}
```

次の例は、GET メソッドを呼び出してクリエイティブを取得する方法を示しています。

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/creative/106851  HTTP/1.1
Authorization: Bearer <your access token>
```


## <a name="response"></a>Response

これらのメソッドは、作成または取得されたクリエイティブに関する情報を含む[クリエイティブ](#creative) オブジェクトを持つ JSON 応答本文を返します。 これらのメソッドの応答本文を次の例に示します。 この例では、簡潔にするために *content* 値が短縮されています。

```json
{
    "Data": {
        "id": 106126,
        "name": "Contoso App Campaign - Creative 2",
        "content": "data:image/jpeg;base64,/9j/4AAQSkZJRgABAQEAAQABAAD/2wBDAAgGB...other base64 data shortened for brevity...",
        "height": 50,
        "width": 300,
        "format": "Banner",
        "imageAttributes":
        {
          "imageExtension": "PNG"
        },
        "storeProductId": "9nblggh42cfd"
    }
}
```


<span id="creative"/>

## <a name="creative-object"></a>クリエイティブ オブジェクト

これらのメソッドの要求本文と応答本文には、次のフィールドが含まれています。 この表は、読み取り専用のフィールド (つまり、PUT メソッドで変更できない) と POST メソッドの要求本文で必須のフィールドを示しています。

| フィールド        | Type   |  説明      |  読み取り専用  | Default  |  POST に必須かどうか |  
|--------------|--------|---------------|------|-------------|------------|
|  id   |  整数 (integer)   |  クリエイティブの ID です。     |   はい    |      |    いいえ   |       
|  name   |  string   |   クリエイティブの名前です。    |    いいえ   |      |  はい     |       
|  コンテンツ   |  string   |  クリエイティブ イメージのコンテンツです (Base64 でエンコードされた形式)。<br/><br/> &nbsp; メモ &nbsp;クリエイティブに許容される最大サイズは 40 KB です。 これよりも大きいクリエイティブ ファイルを送信しても、この API からはエラーが返されず、キャンペーンは正しく作成されません。     |  いいえ     |      |   はい    |       
|  height   |  整数 (integer)   |   クリエイティブの高さです。    |    いいえ    |      |   はい    |       
|  width   |  整数 (integer)   |  クリエイティブの幅です。     |  いいえ    |     |    はい   |       
|  landingUrl   |  string   |  AppsFlyer、Vungle などのキャンペーン追跡サービスを使用してアプリのインストール分析を測定する場合は、POST メソッドを呼び出すときに、このフィールドに追跡 URL を割り当てます (指定した場合、この値は有効な URI である必要があります)。 キャンペーン追跡サービスを使用していない場合、POST メソッドを呼び出すときには、この値を省略します (その場合、この URL は自動的に作成されます)。   |  いいえ    |     |   はい    |
|  format   |  string   |   広告形式です。 現時点では、サポートされている唯一の値は **Banner** です。    |   No    |  バナー   |  No     |       
|  imageAttributes   | [ImageAttributes](#image-attributes)    |   クリエイティブの属性を指定します。     |   いいえ    |      |   はい    |       
|  storeProductId   |  string   |   この広告キャンペーンが関連付けられているアプリの[ストア ID](in-app-purchases-and-trials.md#store-ids) です。 製品のストア ID の例は、9nblggh42cfd です。    |   いいえ    |    |  いいえ     |


<span id="image-attributes"/>

## <a name="imageattributes-object"></a>ImageAttributes オブジェクト

| フィールド        | Type   |  説明      |  読み取り専用  | 既定値  | POST に必須かどうか |  
|--------------|--------|---------------|------|-------------|------------|
|  imageExtension   |   string  |   **PNG** または **JPG** のいずれかの値です。    |    いいえ   |      |   はい    |


## <a name="related-topics"></a>関連トピック

* [Microsoft Store Services を使用して ad キャンペーンを実行する](run-ad-campaigns-using-windows-store-services.md)
* [広告キャンペーンの管理](manage-ad-campaigns.md)
* [広告キャンペーンの配信ラインの管理](manage-delivery-lines-for-ad-campaigns.md)
* [広告キャンペーンの対象プロファイルの管理](manage-targeting-profiles-for-ad-campaigns.md)
* [広告キャンペーンのパフォーマンス データの取得](get-ad-campaign-performance-data.md)
