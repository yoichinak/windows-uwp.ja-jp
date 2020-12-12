---
description: このメソッドを Microsoft Store analytics API で使用して、UWP アプリと Xbox One ゲームの JSON 形式で集計データを取得します。これは、Xbox Developer ポータル (XDP) によって取り込まれたされ、XDP Analytics ダッシュボードで利用できます。
title: ゲームとアプリの入手データを取得する
ms.date: 03/06/2019
ms.topic: article
keywords: Windows 10、UWP、広告ネットワーク、アプリのメタデータ
ms.localizationpriority: medium
ms.openlocfilehash: 371f855e644a990191cd8f8b9365553bc2df9693
ms.sourcegitcommit: 368753aea2792984857f6a57a22daed1035f1a33
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/11/2020
ms.locfileid: "97349698"
---
# <a name="get-acquisitions-data-for-your-games-and-apps"></a>ゲームとアプリの入手データを取得する 
このメソッドを Microsoft Store analytics API で使用して、UWP アプリと Xbox One ゲームの JSON 形式で集計データを取得します。これは、Xbox Developer ポータル (XDP) によって取り込まれたされ、XDP Analytics ダッシュボードで利用できます。 

> [!NOTE]
> この API では、2016年10月1日より前に日単位の集計データは提供されません。 

## <a name="prerequisites"></a>前提条件
このメソッドを使うには、最初に次の作業を行う必要があります。 

* Microsoft Store 分析 API に関するすべての[前提条件](access-analytics-data-using-windows-store-services.md)を満たします (前提条件がまだ満たされていない場合)。 
* このメソッドの要求ヘッダーで使う [Azure AD アクセス トークンを取得](access-analytics-data-using-windows-store-services.md)します。 アクセス トークンを取得した後、アクセス トークンを使用できるのは、その有効期限が切れるまでの 60 分間です。 トークンの有効期限が切れたら新しいトークンを取得できます。 

## <a name="request"></a>Request
### <a name="request-syntax"></a>要求の構文

| 認証方法 | 要求 URI |
| --- | --- |
| GET | `https://manage.devcenter.microsoft.com/v1.0/my/analytics/acquisitions` |

### <a name="request-header"></a>要求ヘッダー

| Header | Type | 説明 |
| --- | --- | --- |
| 承認 | string | 必須。 **Bearer** `<token>` という形式の Azure AD アクセス トークン。 |

### <a name="request-parameters"></a>要求パラメーター

| パラメーター | Type | 説明 | 必須 |
| --- | --- | --- | --- |
| applicationId | string | 入手データを取得する Xbox One ゲームの製品 ID です。 ゲームの製品 ID を取得するには、XDP Analytics プログラムでゲームに移動し、URL から製品 ID を取得します。 また、パートナーセンター分析レポートから購入データをダウンロードする場合は、製品 ID が .tsv ファイルに含まれています。  | はい |
| startDate | 日付 | 取得する入手データの日付範囲の開始日です。 既定値は現在の日付です。  | いいえ |
| endDate | 日付 | 取得する入手データの日付範囲終了日です。 既定値は現在の日付です。  | いいえ |
| filter | string | 応答内の行をフィルター処理する 1 つまたは複数のステートメントです。 各ステートメントには、応答本文からのフィールド名、および **eq** 演算子または **ne** 演算子と関連付けられる値が含まれており、**and** や **or** を使用してステートメントを組み合わせることができます。 filter パラメーターでは、文字列値を単一引用符で囲む必要があります。 たとえば、「*filter=market eq 'US' and gender eq 'm'*」のように指定します。  <br/> 応答本文から次のフィールドを指定することができます。 <ul><li>**acquisitionType**</li><li>**変更**</li><li>**storeClient**</li><li>**gender**</li><li>**マーケティング**</li><li>**osVersion**</li><li>**deviceType**</li><li>**sandboxId**</li></ul> | いいえ |
| aggregationLevel | string | 集計データを取得する時間範囲を指定します。 次のいずれかの文字列を指定できます。**day**、**week**、または **month**。 指定しない場合、既定値は **day** です。  | いいえ |
| orderby | string | それぞれの入手数について結果データ値の順序を指定するステートメントです。 構文は、 *orderby = field [order]、field [order],...**Field* パラメーターには、次のいずれかの文字列を指定できます。 <ul><li>**date**</li><li>**acquisitionType**</li><li>**変更**</li><li>**storeClient**</li><li>**gender**</li><li>**マーケティング**</li><li>**osVersion**</li><li>**deviceType**</li><li>**paymentInstrumentType**</li><li>**sandboxId**</li><li>**xboxTitleIdHex**</li></ul> *order* パラメーターは省略可能であり、**asc** または **desc** を指定して、各フィールドを昇順または降順にすることができます。 既定値は **asc** です。 *orderby* 文字列の例: *orderby=date,market*  | いいえ |
| groupby | string | 指定したフィールドのみにデータ集計を適用するステートメントです。 次のフィールドを指定できます。 <ul><li>**date**</li><li>**applicationName**</li><li>**acquisitionType**</li><li>**ageGroup**</li><li>**storeClient**</li><li>**gender**</li><li>**マーケティング**</li><li>**osVersion**</li><li>**deviceType**</li><li>**paymentInstrumentType**</li><li>**sandboxId**</li><li>**xboxTitleIdHex**</li></ul> 返されるデータ行には、*groupby* パラメーターに指定したフィールドと次の値が含まれます。 <ul><li>**date**</li><li>**applicationId**</li><li>**acquisitionQuantity**</li></ul> *groupby* パラメーターは、aggregationLevel パラメーターと同時に使用できます。 例: *&groupby = ageGroup、market&aggregationLevel = week*  | いいえ |

### <a name="request-example"></a>要求の例
Xbox One ゲームの入手データを取得するための要求の例を、いくつか次に示します。 *ApplicationId* の値をゲームの製品 ID に置き換えます。  

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/acquisitions?applicationId=9WZDNCRFHXHT&startDate=1/1/2017&endDate=2/1/2019&top=10&skip=0 HTTP/1.1 
Authorization: Bearer <your access token> 

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/acquisitions?applicationId=9WZDNCRFHXHT&startDate=1/1/2017&endDate=2/1/2019&skip=0&filter=market eq 'US' and gender eq 'm' HTTP/1.1 
Authorization: Bearer <your access token> 
```

## <a name="response"></a>[応答]

### <a name="response-body"></a>応答本文
| 値 | Type | 説明 |
| --- | --- | --- |
| 値 | array | ゲームに関する集計入手データを含むオブジェクトの配列です。 各オブジェクト内のデータについて詳しくは、後の「[入手値](#acquisition-values)」セクションをご覧ください。 |
| TotalCount | 整数 (integer) | クエリの結果データ内の行の総数です。 |

### <a name="acquisition-values"></a>入手値 
*Value* 配列の要素には、次の値が含まれます。 

| 値 | Type | 説明 |
| --- | --- | --- |
| date | string | 入手データの日付範囲の最初の日付です。 要求に日付を指定した場合、この値はその日付になります。 要求に週、月、またはその他の日付範囲を指定した場合、この値はその日付範囲の最初の日付になります。 |
| applicationId | string | 入手データを取得する Xbox One ゲームの製品 ID です。 |
| applicationName | string | ゲームの表示名です。 |
| acquisitionType | string | 入手の種類を示す、以下のいずれかの文字列です。  <ul><li>**Free**</li><li>**試用版**</li><li>**有料**</li><li>**プロモーションコード**</li><li>**Iap**</li><li>**サブスクリプション Iap**</li><li>**プライベート対象ユーザー**</li><li>**前の順序**</li><li>**Xbox Game Pass** (または、2018 年 3 月 23 日より前のデータを照会した場合は **Game Pass**)</li><li>**ディスク**</li><li>**Prepaid Code**</li><li>**課金される前の注文**</li><li>**キャンセル前の順序**</li><li>**失敗した事前順序**</li></ul> |
| age | string | 入手したユーザーの年齢グループを示す、以下のいずれかの文字列です。 <ul><li>**13未満**</li><li>**13-17**</li><li>**18-24**</li><li>**25-34**</li><li>**35-44**</li><li>**44-55**</li><li>**55より大きい**</li><li>**Unknown**</li></ul> |
| deviceType | string | 入手が完了したデバイスの種類を指定する、以下のいずれかの文字列です。 <ul><li>**PC**</li><li>**Phone**</li><li>**コンソール-Xbox One**</li><li>**コンソール-Xbox シリーズ X**</li><li>**IoT**</li><li>**サーバー**</li><li>**タブレット**</li><li>**Holographic**</li><li>**Unknown**</li></ul> |
| gender | string | 入手したユーザーの性別を指定する、以下のいずれかの文字列です。 <ul><li>**m**</li><li>**f**</li><li>**Unknown**</li></ul> |
| market | string | 入手が発生した市場の ISO 3166 国コードです。 |
| osVersion | string | 入手が発生した OS のバージョンです。 このメソッドでは、この値は常に **Windows 10** になります。 |
| paymentInstrumentType | string | 入手に使われた支払い方法を示す、以下のいずれかの文字列です。 <ul><li>**クレジット カード**</li><li>**Direct Debit Card**</li><li>**Inferred Purchase**</li><li>**MS Balance**</li><li>**携帯電話会社**</li><li>**Online Bank Transfer**</li><li>**PayPal**</li><li>**Split Transaction**</li><li>**Token Redemption**</li><li>**Zero Amount Paid**</li><li>**eWallet**</li><li>**Unknown**</li></ul> |
| sandboxId | string | ゲーム用に作成されたサンドボックス ID です。 この値には、 **小売** の値またはプライベートサンドボックス ID を指定できます。 |
| storeClient | string | 入手が発生した Store のバージョンを示す、以下のいずれかの文字列です。 <ul><li>**Windows Phone ストア (クライアント)**</li><li>**Microsoft Store (client)** (または、2018 年 3 月 23 日より前のデータを照会した場合は **Windows Store (client)**) </li><li>**Microsoft Store (web)** (または、2018 年 3 月 23 日より前のデータを照会した場合は **Windows Store (web)**) </li><li>**Volume purchase by organizations**</li><li>**その他**</li></ul> |
| xboxTitleId | string | Xbox デベロッパー ポータル (XDP) によって Xbox Live 対応ゲームに割り当てられた Xbox Live タイトル ID (16 進数表記) です。 |
| acquisitionQuantity | number | 指定された集計レベルで発生した入手の数です。 |
| purchasePriceUSDAmount | number | 入手時にユーザーが支払った金額を、月ごとの為替レートを使って米国ドルに換算した値です。 |
| purchaseTaxUSDAmount | number | 入手時に適用された税額を米国ドルに換算した値です。 |
| localCurrencyCode | string | パートナーセンターアカウントの国に基づく現地通貨コード。  |
| xboxProductId | string | XDP からの製品の Xbox 製品 ID (該当する場合)。  |
| availabilityId | string | XDP からの製品の可用性 ID (該当する場合)。  |
| skuId | string | XDP からの製品の SKU ID (該当する場合)。  |
| skuDisplayName  | string | XDP からの製品の SKU 表示名 (該当する場合)。  |
| xboxParentProductId | string | XDP から製品の Xbox 親製品 ID (該当する場合)。  |
| parentProductName | string | XDP からの製品の親製品名 (該当する場合)。  |
| productTypeName | string | XDP からの製品の種類名 (該当する場合)。  |
| purchaseTaxType | string | XDP から製品の税の種類を購入します (該当する場合)。  |
| purchasePriceLocalAmount | number | XDP からの製品の購入価格 (該当する場合)。  |
| purchaseTaxLocalAmount | number | 該当する場合は、XDP から製品の税の現地金額を購入します。  |

### <a name="response-example"></a>応答の例
この要求の JSON 返信の本文の例を次に示します。 

```JSON
{ 
    "Value": [ 
        { 
            "date": "2019-01-15T01:00:00.0000000Z", 
            "applicationId": "9WZDNCRFHXHT", 
            "applicationName": null, 
            "acquisitionType": "Paid", 
            "age": null, 
            "deviceType": "Phone", 
            "gender": null, 
            "market": "US", 
            "osVersion": "Windows 10", 
            "paymentInstrumentType": null, 
            "sandboxId": "RETAIL", 
            "storeClient": "Microsoft Store (client)", 
            "xboxTitleId": null, 
            "localCurrencyCode": "USD", 
            "xboxProductId": null, 
            "availabilityId": "B42LRTSZ2MCJ", 
            "skuId": "0010", 
            "skuDisplayName": null, 
            "xboxParentProductId": null, 
            "parentProductName": null, 
            "productTypeName": "Game", 
            "purchaseTaxType": "TaxesNotIncluded", 
            "acquisitionQuantity": 1, 
            "purchasePriceUSDAmount": 3.08, 
            "purchasePriceLocalAmount": 3.08, 
            "purchaseTaxUSDAmount": 0.09, 
            "purchaseTaxLocalAmount": 0.09 
        } 
    ], 

    "@nextLink": null,
    
    "TotalCount": 12221 
} 
```

 
