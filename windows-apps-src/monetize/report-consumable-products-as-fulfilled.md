---
ms.assetid: E9BEB2D2-155F-45F6-95F8-6B36C3E81649
description: 特定のユーザーについてコンシューマブルな製品をフルフィルメント完了として報告するには、Microsoft Store コレクション API の以下のメソッドを使います。 ユーザーがコンシューマブルな製品を再購入するには、アプリまたはサービスが、そのユーザーについてコンシューマブルな製品をフルフィルメント完了と報告している必要があります。
title: コンシューマブルな製品をフルフィルメント完了として報告する
ms.date: 03/19/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store コレクション API, フルフィルメント, コンシューマブル
ms.localizationpriority: medium
ms.openlocfilehash: 88ff4f9bd2c490c8fae4deb2cfa4cbf5c74956c8
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174946"
---
# <a name="report-consumable-products-as-fulfilled"></a>コンシューマブルな製品をフルフィルメント完了として報告する

特定のユーザーについてコンシューマブルな製品をフルフィルメント完了として報告するには、Microsoft Store コレクション API の以下のメソッドを使います。 ユーザーがコンシューマブルな製品を再購入するには、アプリまたはサービスが、そのユーザーについてコンシューマブルな製品をフルフィルメント完了と報告している必要があります。

このメソッドを使用してコンシューマブルな製品をフルフィルメント完了として報告するには、次の 2 つの方法があります。

* コンシューマブルの項目 ID ([製品の照会](query-for-products.md)で **itemId** パラメーターとして返されるもの)、およびユーザー指定の一意の追跡 ID を指定します。 複数の試行で同じ追跡 ID が使用される場合、項目が既に消費されている場合でも同じ結果が返されます。 消費要求が成功したかどうかがわからない場合は、サービスは同じ追跡 ID を使用して消費要求を再送信する必要があります。 追跡 ID は常にその消費要求に関連付けられており、無限に再送信できます。
* 製品 ID ([製品の照会](query-for-products.md)の **productId** パラメーターで返されるもの)、および以下の要求の本文セクションの **transactionId** パラメーターの記述にあるいずれかのソースから取得されるトランザクション ID を指定します。

## <a name="prerequisites"></a>前提条件


このメソッドを使用するための要件:

* 対象ユーザー URI の値が `https://onestore.microsoft.com` の Azure AD アクセス トークン。
* コンシューマブルな製品をフルフィルメント完了として報告するユーザーの ID を表す Microsoft Store ID キー。

詳しくは、「[サービスによる製品の権利の管理](view-and-grant-products-from-a-service.md)」をご覧ください。

## <a name="request"></a>Request


### <a name="request-syntax"></a>要求の構文

| 認証方法 | 要求 URI                                                   |
|--------|---------------------------------------------------------------|
| POST   | ```https://collections.mp.microsoft.com/v6.0/collections/consume``` |


### <a name="request-header"></a>要求ヘッダー

| Header         | Type   | 説明                                                                                           |
|----------------|--------|-------------------------------------------------------------------------------------------------------|
| 承認  | string | 必須。 ベアラートークン形式の Azure AD アクセストークン**Bearer** &lt; *token* &gt; 。                           |
| Host           | string | 値 **collections.mp.microsoft.com** に設定する必要があります。                                            |
| Content-Length | number | 要求本文の長さです。                                                                       |
| Content-Type   | string | 要求と応答の種類を指定します。 現時点では、サポートされている唯一の値は **application/json** です。 |


### <a name="request-body"></a>要求本文

| パラメーター     | Type         | 説明         | 必須 |
|---------------|--------------|---------------------|----------|
| beneficiary   | UserIdentity | この項目が使用されているユーザー。 詳細については、後の表を参照してください。        | はい      |
| itemId        | string       | [製品のクエリ](query-for-products.md)によって返される*itemId*値。 *TrackingId*でこのパラメーターを使用します。      | いいえ       |
| trackingId    | guid         | 開発者により指定される一意の追跡 ID。 このパラメーターは *itemId* と共に使用します。         | いいえ       |
| productId     | string       | [製品のクエリ](query-for-products.md)によって返される*productId*値。 このパラメーターを*Transactionid*と共に使用します。   | いいえ       |
| transactionId | guid         | 次のいずれかのソースから取得されるトランザクション ID 値。 このパラメーターは *productId* と共に使用します。<ul><li>[PurchaseResults](/uwp/api/Windows.ApplicationModel.Store.PurchaseResults) クラスの [TransactionID](/uwp/api/windows.applicationmodel.store.purchaseresults.transactionid) プロパティ。</li><li>[RequestProductPurchaseAsync](/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync)、[RequestAppPurchaseAsync](/uwp/api/windows.applicationmodel.store.currentapp.requestapppurchaseasync)、または [GetAppReceiptAsync](/uwp/api/windows.applicationmodel.store.currentapp.getappreceiptasync) から返されるアプリまたは製品の通知。</li><li>[製品のクエリ](query-for-products.md)によって返される*transactionid*パラメーター。</li></ul>   | いいえ       |


UserIdentity オブジェクトには以下のパラメーターが含まれています。

| パラメーター            | Type   | 説明       | 必須 |
|----------------------|--------|-------------------|----------|
| identityType         | string | 文字列値 **b2b** を指定します。    | はい      |
| identityValue        | string | コンシューマブルな製品をフルフィルメント完了として報告するユーザーの ID を表す [Microsoft Store ID キー](view-and-grant-products-from-a-service.md#step-4)。      | はい      |
| localTicketReference | string | 返された応答で必要な識別子。 Microsoft Store ID キーの *userId*  [要求](view-and-grant-products-from-a-service.md#claims-in-a-microsoft-store-id-key)と同じ値を使用することをお勧めします。 | はい      |


### <a name="request-examples"></a>要求の例

次の例では、*itemId* と *trackingId* を使用します。

```syntax
POST https://collections.mp.microsoft.com/v6.0/collections/consume HTTP/1.1
Authorization: Bearer eyJ0eXAiOiJKV1…..
Host: collections.mp.microsoft.com
Content-Length: 2050
Content-Type: application/json

{
    "beneficiary": {
        "localTicketReference": "testreference",
        "identityValue": "eyJ0eXAiOi…..",
        "identityType": "b2b"
    },
    "itemId": "44c26106-4979-457b-af34-609ae97a084f",
    "trackingId": "44db79ca-e31d-49e9-8896-fa5c7f892b40"
}
```

次の例では、*productId* と *transactionId* を使用します。

```syntax
POST https://collections.mp.microsoft.com/v6.0/collections/consume HTTP/1.1
Authorization: Bearer eyJ0eXAiOiJKV1……
Content-Length: 1880
Content-Type: application/json
Host: collections.md.mp.microsoft.com

{
    "beneficiary" : {
        "localTicketReference" : "testReference",
        "identityValue" : "eyJ0eXAiOiJ…..",
        "identitytype" : "b2b"
    },
    "productId" : "9NBLGGH5WVP6",
    "transactionId" : "08a14c7c-1892-49fc-9135-190ca4f10490"
}
```


## <a name="response"></a>応答

使用が正常に実行された場合、コンテンツは返されません。

### <a name="response-example"></a>応答の例

```syntax
HTTP/1.1 204 No Content
Content-Length: 0
MS-CorrelationId: 386f733d-bc66-4bf9-9b6f-a1ad417f97f0
MS-RequestId: e488cd0a-9fb6-4c2c-bb77-e5100d3c15b1
MS-CV: 5.1
MS-ServerId: 030011326
Date: Tue, 22 Sep 2015 20:40:55 GMT
```

## <a name="error-codes"></a>エラー コード


| コード | エラー        | 内部エラー コード           | 説明           |
|------|--------------|----------------------------|-----------------------|
| 401  | 権限がありません | AuthenticationTokenInvalid | Azure AD アクセス トークンが無効です。 場合によっては、ServiceError の詳細に追加情報が含まれていることがあります (トークンの有効期限切れや *appid* 要求の欠落など)。 |
| 401  | 権限がありません | PartnerAadTicketRequired   | Azure AD アクセス トークンが承認ヘッダーでサービスに渡されませんでした。                                                                                                   |
| 401  | 権限がありません | InconsistentClientId       | 要求本文の Microsoft Store ID の *clientId* 要求と承認ヘッダーの Azure AD アクセス トークンの *appid* 要求が一致しません。                     |

<span/> 

## <a name="related-topics"></a>関連トピック

* [サービスによる製品の権利の管理](view-and-grant-products-from-a-service.md)
* [製品の照会](query-for-products.md)
* [無料の製品の付与](grant-free-products.md)
* [Microsoft Store ID キーの更新](renew-a-windows-store-id-key.md)