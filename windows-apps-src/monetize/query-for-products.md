---
ms.assetid: D1F233EC-24B5-4F84-A92F-2030753E608E
description: Azure AD クライアント ID に関連付けられているアプリでユーザーが所有しているすべての製品を取得するには、Microsoft Store コレクション API の以下のメソッドを使います。 スコープを指定して特定の製品を照会することができ、また他のフィルターを使用することもできます。
title: 製品の照会
ms.date: 03/26/2021
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store コレクション API, 製品の表示
ms.localizationpriority: medium
ms.openlocfilehash: 25d7e5a67d35287433be56bcab6d4e9142f68084
ms.sourcegitcommit: 80ea62d6c0ee25d73750437fe1e37df5224d5797
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/26/2021
ms.locfileid: "105619278"
---
# <a name="query-for-products"></a>製品の照会


Azure AD クライアント ID に関連付けられているアプリでユーザーが所有しているすべての製品を取得するには、Microsoft Store コレクション API の以下のメソッドを使います。 スコープを指定して特定の製品を照会することができ、また他のフィルターを使用することもできます。

このメソッドは、アプリからのメッセージに対する応答としてサービスから呼び出されるように設計されています。 サービスで、スケジュールに従って定期的にすべてのユーザーをポーリングしないでください。

## <a name="prerequisites"></a>前提条件


このメソッドを使用するための要件:

* 対象ユーザー URI の値が `https://onestore.microsoft.com` の Azure AD アクセス トークン。
* 取得対象の製品を所有するユーザーの ID を表す Microsoft Store ID キー。

詳しくは、「[サービスによる製品の権利の管理](view-and-grant-products-from-a-service.md)」をご覧ください。

## <a name="request"></a>要求

### <a name="request-syntax"></a>要求の構文

| 認証方法 | 要求 URI                                                 |
|--------|-------------------------------------------------------------|
| POST   | ```https://collections.mp.microsoft.com/v6.0/collections/query``` |


### <a name="request-header"></a>要求ヘッダー

| Header         | Type   | 説明                                                                                           |
|----------------|--------|-------------------------------------------------------------------------------------------------------|
| 承認  | string | 必須。 ベアラートークン形式の Azure AD アクセストークン &lt;  &gt; 。                           |
| Host           | string | 値 **collections.mp.microsoft.com** に設定する必要があります。                                            |
| Content-Length | number | 要求本文の長さです。                                                                       |
| Content-Type   | string | 要求と応答の種類を指定します。 現時点では、サポートされている唯一の値は **application/json** です。 |


### <a name="request-body"></a>要求本文

| パラメーター         | Type         | 説明         | 必須 |
|-------------------|--------------|---------------------|----------|
| beneficiaries     | useridentity の一覧表示 &lt;&gt; | 製品のクエリを行うユーザーを表す UserIdentity オブジェクトの一覧。 詳細については、次の表をご覧ください。    | はい      |
| continuationToken | string       | 製品のセットが複数ある場合、ページ制限に達すると、応答本文で継続トークンが返されます。 残りの製品を取得する後続の呼び出しで、この継続トークンを指定します。       | No       |
| maxPageSize       | number       | 1 つの応答で返す製品の最大数。 既定値および最大値は 100 です。                 | No       |
| modifiedAfter     | DATETIME     | 指定した場合、この日付以降に変更された製品だけがサービスから返されます。        | No       |
| parentProductId   | string       | 指定した場合、指定されたアプリに対応するアドオンだけがサービスから返されます。      | No       |
| productSkuIds     | list&lt;ProductSkuId&gt; | 指定した場合、指定された製品/SKU のペアに該当する製品だけがサービスから返されます。 詳細については、次の表をご覧ください。      | No       |
| productTypes      | リスト &lt; 文字列&gt;       | クエリ結果で返す製品の種類を指定します。 サポートされている製品の種類は、 **Application**、 **耐久性**、 **Game**、および **UnmanagedConsumable** です。     | はい       |
| validityType      | string       | **All** に設定した場合、有効期限が切れた項目を含む、ユーザーのすべての製品が返されます。 **有効** に設定すると、この時点で有効な製品のみが返されます (つまり、アクティブな状態、開始日が指定され、終了日がになってい &lt; &gt; ます)。 | No       |


UserIdentity オブジェクトには以下のパラメーターが含まれています。

| パラメーター            | Type   |  説明      | 必須 |
|----------------------|--------|----------------|----------|
| identityType         | string | 文字列値 **b2b** を指定します。    | はい      |
| identityValue        | string | 照会する製品のユーザーの ID を表す [Microsoft Store ID キー](view-and-grant-products-from-a-service.md#step-4)。  | はい      |
| localTicketReference | string | 返された製品で必要な識別子。 応答本文で返された項目には、一致する *localTicketReference* があります。 Microsoft Store ID キーの *userId* 要求と同じ値を使用することをお勧めします。 | はい      |


ProductSkuId オブジェクトには以下のパラメーターが含まれています。

| パラメーター | Type   | 説明          | 必須 |
|-----------|--------|----------------------|----------|
| productId | string | Microsoft Store カタログ内の[製品](in-app-purchases-and-trials.md#products-skus-and-availabilities)の [Store ID](in-app-purchases-and-trials.md#store-ids)。 製品のストア ID の例は、9NBLGGH42CFD です。 | はい      |
| skuID     | string | Microsoft Store カタログ内の製品の [SKU](in-app-purchases-and-trials.md#products-skus-and-availabilities) の [Store ID](in-app-purchases-and-trials.md#store-ids)。 SKU の Store ID の例は、0010 です。       | はい      |


### <a name="request-example"></a>要求の例

```syntax
POST https://collections.mp.microsoft.com/v6.0/collections/query HTTP/1.1
Authorization: Bearer eyJ0eXAiOiJKV1Q…….
Host: collections.mp.microsoft.com
Content-Length: 2531
Content-Type: application/json

{
  "maxPageSize": 100,
  "beneficiaries": [
    {
      "localTicketReference": "1055521810674918",
      "identityValue": "eyJ0eXAiOiJ……",
      "identityType": "b2b"
    }
  ],
  "modifiedAfter": "\/Date(-62135568000000)\/",
  "productSkuIds": [
    {
      "productId": "9NBLGGH5WVP6",
      "skuId": "0010"
    }
  ],
  "productTypes": [
    "UnmanagedConsumable"
  ],
  "validityType": "All"
}
```

## <a name="response"></a>[応答]


### <a name="response-body"></a>応答本文

| パラメーター         | Type                     | 説明          | 必須 |
|-------------------|--------------------------|-----------------------|----------|
| continuationToken | string                   | 製品のセットが複数ある場合、ページ制限に達すると、このトークンが返されます。 残りの製品を取得する後続の呼び出しで、この継続トークンを指定できます。 | No       |
| items             | CollectionItemContractV6 | 指定したユーザーの製品の配列。 詳細については、次の表をご覧ください。        | No       |


CollectionItemContractV6 オブジェクトには以下のパラメーターが含まれています。

| パラメーター            | Type               | 説明            | 必須 |
|----------------------|--------------------|-------------------------|----------|
| acquiredDate         | DATETIME           | ユーザーが項目を取得した日付。                  | はい      |
| campaignId           | string             | この項目の購入時に提供されたキャンペーン ID。                  | No       |
| devOfferId           | string             | アプリ内購入からのプラン ID。              | いいえ       |
| endDate              | DATETIME           | 項目の終了日。              | はい      |
| fulfillmentData      | 表<string>       | 該当なし         | いいえ       |
| inAppOfferToken      | string             | パートナーセンターの項目に割り当てられている、開発者が指定した製品 ID 文字列。 たとえば、製品 ID は *product123* です。 | No       |
| itemId               | string             | ユーザーが所有する他の項目からこのコレクション項目を識別する ID。 この ID は製品ごとに一意です。   | はい      |
| localTicketReference | string             | 要求本文の *localTicketReference* で指定された ID。                  | はい      |
| modifiedDate         | DATETIME           | この項目が最後に更新された日付。              | はい      |
| orderId              | string             | 存在する場合、この項目が取得された注文 ID。              | No       |
| orderLineItemId      | string             | 存在する場合、この項目が取得された特定の注文の行項目。              | No       |
| ownershipType        | string             | 文字列 *OwnedByBeneficiary*。   | はい      |
| productId            | string             | Microsoft Store カタログ内の[製品](in-app-purchases-and-trials.md#products-skus-and-availabilities)の [Store ID](in-app-purchases-and-trials.md#store-ids)。 製品のストア ID の例は、9NBLGGH42CFD です。          | はい      |
| productType          | string             | **Application**、**Durable**、および **UnmanagedConsumable** の製品タイプのいずれか。        | はい      |
| purchasedCountry     | string             | 該当なし   | いいえ       |
| purchaser            | IdentityContractV6 | 存在する場合、項目の購入者の ID を表します。 下記に示すこのオブジェクトの詳細を参照してください。        | No       |
| 数量             | number             | 項目の数量。 現在、これは常に 1 になります。      | No       |
| skuId                | string             | Microsoft Store カタログ内の製品の [SKU](in-app-purchases-and-trials.md#products-skus-and-availabilities) の [Store ID](in-app-purchases-and-trials.md#store-ids)。 SKU の Store ID の例は、0010 です。     | はい      |
| skuType              | string             | SKU のタイプ。 可能な値は **Trial**、**Full**、および **Rental** です。        | はい      |
| startDate            | DATETIME           | 項目の有効期間の開始日。       | はい      |
| status               | string             | 項目の状態。 可能な値は **Active**、**Expired**、**Revoked**、および **Banned** です。    | はい      |
| tags                 | list<string>       | 該当なし    | はい      |
| transactionId        | guid               | この項目の購入の結果としてのトランザクション ID。 フルフィルメント完了として項目を報告するのに使用できます。      | はい      |


IdentityContractV6 オブジェクトには以下のパラメーターが含まれています。

| パラメーター     | Type   | 説明                                                                        | 必須 |
|---------------|--------|------------------------------------------------------------------------------------|----------|
| identityType  | string | 値 *pub* を格納します。                                                      | はい      |
| identityValue | string | 指定された Microsoft Store ID キーの *publisherUserId* の文字列値。 | はい      |


### <a name="response-example"></a>応答の例

```syntax
HTTP/1.1 200 OK
Content-Length: 7241
Content-Type: application/json
MS-CorrelationId: 699681ce-662c-4841-920a-f2269b2b4e6c
MS-RequestId: a9988cf9-652b-4791-beba-b0e732121a12
MS-CV: xu2HW6SrSkyfHyFh.0.1
MS-ServerId: 020022359
Date: Tue, 22 Sep 2015 20:28:18 GMT

{
  "items" : [
    {
      "acquiredDate" : "2015-09-22T19:22:51.2068724+00:00",
      "devOfferId" : "f9587c53-540a-498b-a281-8a349491ed47",
      "endDate" : "9999-12-31T23:59:59.9999999+00:00",
      "fulfillmentData" : [],
      "inAppOfferToken" : "consumable2",
      "itemId" : "4b8fbb13127a41f299270ea668681c1d",
      "localTicketReference" : "1055521810674918",
      "modifiedDate" : "2015-09-22T19:22:51.2513155+00:00",
      "orderId" : "4ba5960d-4ec6-4a81-ac20-aafce02ddf31",
      "ownershipType" : "OwnedByBeneficiary",
      "productId" : "9NBLGGH5WVP6",
      "productType" : "UnmanagedConsumable",
      "purchaser" : {
        "identityType" : "pub",
        "identityValue" : "user123"
      },
      "skuId" : "0010",
      "skuType" : "Full",
      "startDate" : "2015-09-22T19:22:51.2068724+00:00",
      "status" : "Active",
      "tags" : [],
      "transactionId" : "4ba5960d-4ec6-4a81-ac20-aafce02ddf31"
    }
  ]
}
```

## <a name="related-topics"></a>関連トピック

* [サービスによる製品の権利の管理](view-and-grant-products-from-a-service.md)
* [コンシューマブルな製品をフルフィルメント完了として報告する](report-consumable-products-as-fulfilled.md)
* [無料の製品の付与](grant-free-products.md)
* [Microsoft Store ID キーの更新](renew-a-windows-store-id-key.md)
