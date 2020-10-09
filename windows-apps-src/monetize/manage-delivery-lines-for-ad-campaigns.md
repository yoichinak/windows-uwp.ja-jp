---
ms.assetid: dc632a4c-ce48-400b-8e6e-1dddbd13afff
description: プロモーション用の広告キャンペーンの配信ラインを管理するには、Microsoft Store プロモーション API の以下のメソッドを使います。
title: 配信ラインの管理
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store プロモーション API, 広告キャンペーン
ms.localizationpriority: medium
ms.openlocfilehash: e7b370d8eea61033092d833cdf751f1dade86c6d
ms.sourcegitcommit: 5d84d8fe60e83647fa363b710916cf8b92c6e331
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/09/2020
ms.locfileid: "91878565"
---
# <a name="manage-delivery-lines"></a>配信ラインの管理

1 つ以上の*配信ライン*を作成してインベントリを購入し、プロモーション用の広告キャンペーンの広告を配信するには、Microsoft Store プロモーション API の以下のメソッドを使います。 配信ラインごとに、ターゲットと入札額を設定できます。また、予算と使用したいクリエイティブへのリンクを設定することで、支払い額を決定できます。

配信ラインと広告キャンペーン、ターゲット プロファイル、クリエイティブとの関係について詳しくは、「[Microsoft Store サービスを使用した広告キャンペーンの実行](run-ad-campaigns-using-windows-store-services.md#call-the-windows-store-promotions-api)」をご覧ください。

>**Note** &nbsp; メモ &nbsp;この API を使用して広告キャンペーンの配信明細行を正常に作成するには、まず、[パートナーセンターの [ **ad キャンペーン**] ページを使用して1つの有料広告キャンペーンを作成](./index.md)し、このページで少なくとも1つの支払い方法を追加する必要があります。 これを行うと、この API を使用して、広告キャンペーンの請求可能な配信ラインを正しく作成することができます。 API を使用して作成した ad キャンペーンは、パートナーセンターの [ **ad キャンペーン** ] ページで選択した既定の支払い方法に自動的に課金されます。

## <a name="prerequisites"></a>前提条件

これらのメソッドを使うには、最初に次の作業を行う必要があります。

* Microsoft Store プロモーション API に関するすべての[前提条件](run-ad-campaigns-using-windows-store-services.md#prerequisites)を満たします (前提条件がまだ満たされていない場合)。

  > [!NOTE]
  > 前提条件の一部として、 [パートナーセンターで少なくとも1つの有料広告キャンペーンを作成](./index.md) し、パートナーセンターで広告キャンペーンの少なくとも1つの支払い方法を追加するようにしてください。 この API を使用して作成した配信行は、パートナーセンターの [ **Ad キャンペーン** ] ページで選択した既定の支払い方法に自動的に課金されます。

* これらのメソッドの要求ヘッダーで使う [Azure AD アクセス トークンを取得](run-ad-campaigns-using-windows-store-services.md#obtain-an-azure-ad-access-token)します。 アクセス トークンを取得した後、アクセス トークンを使用できるのは、その有効期限が切れるまでの 60 分間です。 トークンの有効期限が切れたら新しいトークンを取得できます。

## <a name="request"></a>Request

これらのメソッドでは、次の URL が使用されます。

| メソッドの型 | 要求 URI         |  説明  |
|--------|---------------------------|---------------|
| POST   | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/line``` |  新しい配信ラインを作成します。  |
| PUT    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/line/{lineId}``` |  *lineId* により指定された配信ラインを編集します。  |
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/line/{lineId}``` |  *lineId* により指定された配信ラインを取得します。  |


### <a name="header"></a>Header

| Header        | Type   | 説明         |
|---------------|--------|---------------------|
| 承認 | string | 必須。 ベアラートークン形式の Azure AD アクセストークン**Bearer** &lt; *token* &gt; 。 |
| 追跡 ID   | GUID   | 任意。 呼び出しフローを追跡する ID。                                  |


### <a name="request-body"></a>要求本文

POST メソッドと PUT メソッドには、[配信ライン](#line) オブジェクトの必須フィールドと設定または変更する追加フィールドを持つ JSON 要求本文が必要です。


### <a name="request-examples"></a>要求の例

次の例は、POST メソッドを呼び出して配信ラインを作成する方法を示しています。

```json
POST https://manage.devcenter.microsoft.com/v1.0/my/promotion/line HTTP/1.1
Authorization: Bearer <your access token>

{
    "name": "Contoso App Campaign - Paid Line",
    "configuredStatus": "Active",
    "startDateTime": "2017-01-19T12:09:34Z",
    "endDateTime": "2017-01-31T23:59:59Z",
    "bidAmount": 0.4,
    "dailyBudget": 20,
    "targetProfileId": {
        "id": 310021746
    },
    "creatives": [
        {
            "id": 106851
        }
    ],
    "campaignId": 31043481,
    "minMinutesPerImp ": 1
}
```

次の例は、GET メソッドを呼び出して配信ラインを取得する方法を示しています。

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/line/31019990  HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>[応答]

これらのメソッドは、作成、更新、または取得された配信ラインに関する情報を含む[配信ライン](#line) オブジェクトを持つ JSON 応答本文を返します。 これらのメソッドの応答本文を次の例に示します。

```json
{
    "Data": {
        "id": 31043476,
        "name": "Contoso App Campaign - Paid Line",
        "configuredStatus": "Active",
        "effectiveStatus": "Active",
        "effectiveStatusReasons": [
            "{\"ValidationStatusReasons\":null}"
        ],
        "startDateTime": "2017-01-19T12:09:34Z",
        "endDateTime": "2017-01-31T23:59:59Z",
        "createdDateTime": "2017-01-17T10:28:34Z",
        "bidType": "CPM",
        "bidAmount": 0.4,
        "dailyBudget": 20,
        "targetProfileId": {
            "id": 310021746
        },
        "creatives": [
            {
                "id": 106126
            }
        ],
        "campaignId": 31043481,
        "minMinutesPerImp ": 1,
        "pacingType ": "SpendEvenly",
        "currencyId ": 732
    }
}

```


<span id="line"/>

## <a name="delivery-line-object"></a>配信ライン オブジェクト

これらのメソッドの要求本文と応答本文には、次のフィールドが含まれています。 この表は、読み取り専用のフィールド (つまり、PUT メソッドで変更できない) と POST メソッドまたは PUT メソッドの要求本文で必須のフィールドを示しています。

| フィールド        | Type   |  説明      |  読み取り専用  | Default  | POST/PUT に必須かどうか |   
|--------------|--------|---------------|------|-------------|------------|
|  id   |  整数 (integer)   |  配信ラインの ID です。     |   はい    |      |  いいえ      |    
|  name   |  string   |   配信ラインの名前です。    |    いいえ   |      |  POST     |     
|  configuredStatus   |  string   |  開発者により指定された配信ラインのステータスを指定する次のいずれかの値です。 <ul><li>**アクティブ**</li><li>**非アクティブ**</li></ul>     |  いいえ     |      |   POST    |       
|  effectiveStatus   |  string   |   システム検証に基づいて配信ラインの有効ステータスを指定する次のいずれかの値です。 <ul><li>**アクティブ**</li><li>**非アクティブ**</li><li>**処理中**</li><li>**Failed**</li></ul>    |    はい   |      |  いいえ      |      
|  effectiveStatusReasons   |  array   |  配信ラインの有効ステータスの理由を指定する次のうち 1 つ以上の値です。 <ul><li>**AdCreativesInactive**</li><li>**ValidationFailed**</li></ul>      |  はい     |     |    いいえ    |           
|  startDatetime   |  string   |  配信ラインの開始日時です (ISO 8601 形式)。 日時が過去の場合、この値を変更できません。     |    いいえ   |      |    POST、PUT     |
|  endDatetime   |  string   |  配信ラインの終了日時です (ISO 8601 形式)。 日時が過去の場合、この値を変更できません。     |   いいえ    |      |  POST、PUT     |
|  createdDatetime   |  string   |  配信ラインが作成された日時 (ISO 8601 形式)。      |    はい   |      |  いいえ      |
|  bidType   |  string   |  配信ラインの入札の種類を指定する値です。 現時点では、サポートされている唯一の値は **CPM** です。      |    いいえ   |  CPM    |  いいえ     |
|  bidAmount   |  decimal   |  広告要求の入札に使う入札額です。      |    いいえ   |  対象市場に基づく平均 CPM 値です (この値は定期的に変更されます)。    |    いいえ    |  
|  dailyBudget   |  decimal   |  配信ラインの 1 日あたりの予算です。 *dailyBudget* または *lifetimeBudget* を設定する必要があります。      |    いいえ   |      |   POST、PUT (*lifetimeBudget* が設定されていない場合)       |
|  lifetimeBudget   |  decimal   |   配信ラインの全体予算です。 lifetimeBudget* または *dailyBudget* を設定する必要があります。      |    いいえ   |      |   POST、PUT (*dailyBudget* が設定されていない場合)    |
|  targetingProfileId   |  object   |  この配信ラインを対象にするユーザー、地域、およびインベントリの種類を指定する[ターゲット プロファイル](manage-targeting-profiles-for-ad-campaigns.md#targeting-profile)を識別するオブジェクトです。 このオブジェクトは、ターゲット プロファイルの ID を指定する単一の *id* フィールドで構成されます。     |    いいえ   |      |  いいえ      |  
|  creatives   |  array   |  配信ラインに関連づけられた[クリエイティブ](manage-creatives-for-ad-campaigns.md#creative)を表す 1 つ以上のオブジェクトです。 このフィールド内の各オブジェクトは、クリエイティブの ID を指定する単一の *id* フィールドで構成されます。      |    いいえ   |      |   いいえ     |  
|  campaignId   |  整数 (integer)   |  親広告キャンペーンの ID です。      |    いいえ   |      |   いいえ     |  
|  minMinutesPerImp   |  整数 (integer)   |  この配信ラインから同じユーザーに表示される 2 つのインプレッション間の間隔 (分単位) を指定します。      |    いいえ   |  4000    |  いいえ      |  
|  pacingType   |  string   |  ペーシングの種類を指定する次のいずれかの値です。 <ul><li>**SpendEvenly**</li><li>**SpendAsFastAsPossible**</li></ul>      |    いいえ   |  SpendEvenly    |  いいえ      |
|  currencyId   |  整数 (integer)   |  キャンペーンの通貨の ID です。      |    はい   |  開発者アカウントの通貨 (POST または PUT 呼び出しではこのフィールドを指定する必要はありません)    |   いいえ     |      |


## <a name="related-topics"></a>関連トピック

* [Microsoft Store Services を使用して ad キャンペーンを実行する](run-ad-campaigns-using-windows-store-services.md)
* [広告キャンペーンの管理](manage-ad-campaigns.md)
* [広告キャンペーンの対象プロファイルの管理](manage-targeting-profiles-for-ad-campaigns.md)
* [広告キャンペーンのクリエイティブの管理](manage-creatives-for-ad-campaigns.md)
* [広告キャンペーンのパフォーマンス データの取得](get-ad-campaign-performance-data.md)