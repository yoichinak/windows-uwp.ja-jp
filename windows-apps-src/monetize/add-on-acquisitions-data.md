---
description: Microsoft Store analytics API でこのメソッドを使用して、XDP Analytics パートナーセンターのダッシュボードで使用できる UWP アプリおよび Xbox One ゲーム用の JSON 形式で、集計されたアドオン取得データを取得します。
title: ゲームとアプリのアドオン入手データを取得する
ms.date: 03/06/2019
ms.topic: article
keywords: Windows 10、UWP、広告ネットワーク、アプリのメタデータ
ms.localizationpriority: medium
ms.openlocfilehash: 3e1349582515ce66232ea8266efc588610faae98
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493567"
---
# <a name="get-add-on-acquisitions-data-for-your-games-and-apps"></a>ゲームとアプリのアドオン入手データを取得する 
Microsoft Store analytics API でこのメソッドを使用して、XDP Analytics パートナーセンターのダッシュボードで使用できる UWP アプリおよび Xbox One ゲーム用の JSON 形式で、集計されたアドオン取得データを取得します。 

## <a name="prerequisites"></a>前提条件
このメソッドを使うには、最初に次の作業を行う必要があります。 

* Microsoft Store 分析 API に関するすべての[前提条件](access-analytics-data-using-windows-store-services.md)を満たします (前提条件がまだ満たされていない場合)。 
* このメソッドの要求ヘッダーで使う [Azure AD アクセス トークンを取得](access-analytics-data-using-windows-store-services.md)します。 アクセス トークンを取得した後、アクセス トークンを使用できるのは、その有効期限が切れるまでの 60 分間です。 トークンの有効期限が切れたら新しいトークンを取得できます。 

> [!NOTE]
> この API では、2016年10月1日より前に日単位の集計データは提供されません。 

## <a name="request"></a>Request

### <a name="request-syntax"></a>要求の構文
| 認証方法 | 要求 URI |
| --- | --- | 
| GET | `https://manage.devcenter.microsoft.com/v1.0/my/analytics/addonacquisitions` |

### <a name="request-header"></a>要求ヘッダー 
| Header | 種類 | 説明 | 
| --- | --- | --- |
| 承認 | string | 必須。 **Bearer** `<token>` という形式の Azure AD アクセス トークン。 |

### <a name="request-parameters"></a>要求パラメーター
*ApplicationId*または*addonProductId*パラメーターが必要です。 アプリに登録されたすべてのアドオンの入手データを取得するには、*applicationId* パラメーターを指定します。 1つのアドオンの取得データを取得するには、 *addonProductId*パラメーターを指定します。 両方を指定した場合、*applicationId* パラメーターは無視されます。 

| パラメーター | 種類 | 説明 | 必須 | 
| --- | --- | --- | --- |
| applicationId | string | 取得データを取得する Xbox One ゲームの*productId* 。 ゲームの*productid*を取得するには、XDP Analytics プログラムでゲームに移動し、URL から*productid*を取得します。 または、パートナーセンター分析レポートから取得データをダウンロードする場合は、 *productId*が .tsv ファイルに含まれています。 | はい |
| addonProductId | string | 取得データを取得する対象となるアドオンの*productId* 。 | はい |
| startDate | date | 取得するアドオン入手データの日付範囲の開始日です。 既定値は現在の日付です。 | いいえ |
| endDate | 日付 | 取得するアドオン入手データの日付範囲終了日です。 既定値は現在の日付です。 | いいえ |
| top | INT | 要求で返すデータの行数です。 最大値および指定しない場合の既定値は 10000 です。 クエリにこれを上回る行がある場合は、応答本文に次リンクが含まれ、そのリンクを使ってデータの次のページを要求できます。 | いいえ |
| skip | int | クエリでスキップする行数です。 大きなデータ セットを操作するには、このパラメーターを使用します。 たとえば、top=10000 と skip=0 を指定すると、データの最初の 10,000 行が取得され、top=10000 と skip=10000 を指定すると、データの次の 10,000 行が取得されます。 | いいえ |
| filter | string | 応答内の行をフィルター処理する 1 つまたは複数のステートメントです。 各ステートメントには、応答本文からのフィールド名、および eq 演算子または ne 演算子と関連付けられる値が含まれており、and や or を使用してステートメントを組み合わせることができます。 filter パラメーターでは、文字列値を単一引用符で囲む必要があります。 たとえば、「filter=market eq 'US' and gender eq 'm'」のように指定します。 <br/> 応答本文から次のフィールドを指定することができます。 <ul><li>**acquisitionType**</li><li>**変更**</li><li>**storeClient**</li><li>**gender**</li><li>**マーケティング**</li><li>**osVersion**</li><li>**deviceType**</li><li>**sandboxId**</li></ul> | いいえ |
| aggregationLevel | string | 集計データを取得する時間範囲を指定します。 次のいずれかの文字列を指定できます。**day**、**week**、または **month**。 指定しない場合、既定値は **day** です。 | いいえ |
| orderby | string | それぞれのアドオン入手数について結果データ値の順序を指定するステートメントです。 構文は、 *orderby = field [order]、field [order],...**Field*パラメーターには、次のいずれかの文字列を指定できます。 <ul><li>**date**</li><li>**acquisitionType**</li><li>**変更**</li><li>**storeClient**</li><li>**gender**</li><li>**マーケティング**</li><li>**osVersion**</li><li>**deviceType**</li><li>**orderName**</li></ul> order パラメーターは省略可能であり、**asc** または **desc** を指定して、各フィールドを昇順または降順にすることができます。 既定値は**asc**です。 <br/> *orderby* 文字列の例: *orderby=date,market* | いいえ |
| groupby | string | 指定したフィールドのみにデータ集計を適用するステートメントです。 次のフィールドを指定できます。 <ul><li>**date**</li><li>**applicationName**</li><li>**addonProductName**</li> <li>**acquisitionType**</li><li>**変更**</li> <li>**storeClient**</li><li>**gender**</li> <li>**マーケティング**</li> <li>**osVersion**</li><li>**deviceType**</li><li>**paymentInstrumentType**</li><li>**sandboxId**</li><li>**xboxTitleIdHex**</li></ul> 返されるデータ行には、*groupby* パラメーターに指定したフィールドと次の値が含まれます。 <ul><li>**date**</li><li>**applicationId**</li><li>**addonProductId**</li><li>**acquisitionQuantity**</li></ul> groupby パラメーターは、*aggregationLevel* パラメーターと同時に使用できます。 例: *&groupby = age、market&aggregationLevel = week* | いいえ |

### <a name="request-example"></a>要求の例
アドオン入手データを取得するためのいくつかの要求の例を次に示します。 *AddonProductId*と*applicationId*の値を、アドオンまたはアプリの適切なストア ID に置き換えます。 

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/addonacquisitions?applicationId=9WZDNCRFJ314&startDate=1/1/2015&endDate=2/1/2015&skip=0 HTTP/1.1 

Authorization: Bearer <your access token> 

 

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/addonacquisitions?applicationId=9WZDNCRFJ314&startDate=1/1/2015&endDate=2/1/2015&skip=0&filter=market eq 'GB' and gender eq 'm' HTTP/1.1 

Authorization: Bearer <your access token>
```

## <a name="response"></a>[応答]

### <a name="response-body"></a>応答本文

| 値 | 種類 | 説明 |
| --- | --- | --- |
| 値 | array | 集計アドオン入手データが含まれるオブジェクトの配列です。 各オブジェクトのデータについて詳しくは、次の「[アドオン入手値](#add-on-acquisition-values)」セクションをご覧ください。 |
| @nextLink | string | データの追加ページがある場合、この文字列には、データの次のページを要求するために使用できる URI が含まれます。 たとえば、要求の **top** パラメーターが 10000 に設定されたが、クエリのアドオン入手データに 10,000 を超える行が含まれている場合に、この値が返されます。 |
| TotalCount | INT | クエリの結果データ内の行の総数です。 |

### <a name="add-on-acquisition-values"></a>アドオン入手値
Value 配列の要素には、次の値が含まれます。

| 値 | 種類 | 説明 | 
| --- | --- | --- |
| date | string | 入手データの日付範囲の最初の日付です。 要求に日付を指定した場合、この値はその日付になります。 要求に週、月、またはその他の日付範囲を指定した場合、この値はその日付範囲の最初の日付になります。 |
| addonProductId | string | 取得データを取得しているアドオンの*productId* 。 |
| addonProductName | string | アドオンの表示名です。 *Groupby*パラメーターで**addonProductName**フィールドを指定しない限り、この値は、 *aggregationLevel*パラメーターが**day**に設定されている場合にのみ、応答データに表示されます。 |
| applicationId | string | アドオン取得データを取得する対象のアプリの*productId* 。 |
| applicationName | string | ゲームの表示名です。 |
| deviceType | string | 入手が完了したデバイスの種類を指定する、以下のいずれかの文字列です。 <ul><li>PC</li><li>ダイヤル</li><li>"Console-Xbox One"</li><li>"コンソール-Xbox シリーズ X"</li><li>IoT</li><li>Server</li><li>Table</li><li>Holographic</li><li>知ら</li></ul> |
| storeClient | string | 入手が発生した Store のバージョンを示す、以下のいずれかの文字列です。 <ul><li>"Windows Phone ストア (クライアント)"</li><li>"Microsoft Store (クライアント)" (2018 年3月23日より前にデータを照会する場合は "Windows ストア (クライアント)")</li><li>"Microsoft Store (web)" (2018 年3月23日より前にデータを照会する場合は "Windows ストア (web)")</li><li>"組織別のボリューム購入"</li><li>他の</li></ul> |
| osVersion | string | 入手が発生した OS のバージョンです。 このメソッドでは、この値は常に "Windows 10" です。 |
| market | string | 入手が発生した市場の ISO 3166 国コードです。 |
| gender | string | 入手したユーザーの性別を指定する、以下のいずれかの文字列です。 <ul><li>"m"</li><li>"f"</li><li>知ら</li></ul> |
| age | string | 入手したユーザーの年齢グループを示す、以下のいずれかの文字列です。 <ul><li>"13 未満"</li><li>"13-17"</li><li>"18-24"</li><li>"25-34"</li><li>"35-44"</li><li>"44-55"</li><li>"55 より大きい"</li><li>知ら</li></ul> |
| acquisitionType | string | 入手の種類を示す、以下のいずれかの文字列です。 <ul><li>空け </li><li>サブスクリプション </li><li>貢物</li><li>"プロモーションコード" </li><li>Iap</li><li>"Subscription Iap"</li><li>"プライベートユーザー"</li><li>"事前の順序"</li><li>"Xbox ゲームパス" (2018 年3月23日より前にデータを照会する場合は "ゲームパス")</li><li>ディスク</li><li>"前払いコード"</li><li>"課金される前の注文"</li><li>"取り消された事前注文"</li><li>"失敗した事前順序"</li></ul> |
| acquisitionQuantity | 整数 (integer) | 発生した入手の数です。 |
| inAppProductId | string | このアドオンが使用される製品の製品 ID。  |
| inAppProductName | string | このアドオンが使用される製品の製品名。  |
| paymentInstrumentType | string | 取得に使用する支払い方法の種類。  |
| sandboxId | string | ゲーム用に作成されたサンドボックス ID。 この値には、**小売**の値またはプライベートサンドボックス ID を指定できます。  |
| xboxTitleId | string | XDP からの製品の Xbox タイトル ID (該当する場合)。  |
| localCurrencyCode | string | パートナーセンターアカウントの国に基づく現地通貨コード。  |
| xboxProductId | string | XDP からの製品の Xbox 製品 ID (該当する場合)。  |
| availabilityId | string | XDP からの製品の可用性 ID (該当する場合)。  |
| skuId | string | XDP からの製品の SKU ID (該当する場合)。  |
| skuDisplayName | string | XDP からの製品の SKU 表示名 (該当する場合)。  |
| xboxParentProductId | string | XDP から製品の Xbox 親製品 ID (該当する場合)。  |
| parentProductName | string | XDP からの製品の親製品名 (該当する場合)。  |
| productTypeName | string | XDP からの製品の種類名 (該当する場合)。  |
| purchaseTaxType | string | XDP から製品の税の種類を購入します (該当する場合)。  |
| purchasePriceUSDAmount | number | このアドオンに関して顧客が支払った金額を、米国ドルに換算します。  |
| purchasePriceLocalAmount | number | アドオンに適用された税額。  |
| purchaseTaxUSDAmount | number | アドオンに適用された税金額を米国ドルに変換した値。  |
| purchaseTaxLocalAmount | number | 該当する場合は、XDP から製品の税の現地金額を購入します。  |

### <a name="response-example"></a>応答の例
この要求の JSON 返信の本文の例を次に示します。 

```JSON
{ 
  "Value": [ 
    { 
            "inAppProductId": "9NBLGGH1864K", 
            "inAppProductName": "866879", 
            "addonProductId": "9NBLGGH1864K", 
            "addonProductName": "866879", 
            "date": "2017-11-05", 
            "applicationId": "9WZDNCRFJ314", 
            "applicationName": "Tetris Blitz", 
            "acquisitionType": "Iap", 
            "age": "35-49", 
            "deviceType": "Phone", 
            "gender": "m", 
            "market": "US", 
            "osVersion": "Windows Phone 8.1", 
            "paymentInstrumentType": "Credit Card", 
            "sandboxId": "RETAIL", 
            "storeClient": "Windows Phone Store (client)", 
            "xboxTitleId": "", 
            "localCurrencyCode": "USD", 
            "xboxProductId": "00000000-0000-0000-0000-000000000000", 
            "availabilityId": "", 
            "skuId": "", 
            "skuDisplayName": "Full", 
            "xboxParentProductId": "", 
            "parentProductName": "Tetris Blitz", 
            "productTypeName": "Add-On", 
            "purchaseTaxType": "", 
            "acquisitionQuantity": 1, 
            "purchasePriceUSDAmount": 1.08, 
            "purchasePriceLocalAmount": 0.09, 
            "purchaseTaxUSDAmount": 1.08, 
            "purchaseTaxLocalAmount": 0.09 
        } 
    ], 

    "@nextLink": null, 
    
    "TotalCount": 7601 
} 
```