---
ms.assetid: F37C2CEC-9ED1-4F9E-883D-9FBB082504D4
description: ユーザーのサブスクリプションの請求状態を変更するには、Microsoft Store 購入 API の以下のメソッドを使います。
title: ユーザーのサブスクリプションに関する請求の状態を変更する
ms.date: 08/01/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store 購入 API, サブスクリプション
ms.localizationpriority: medium
ms.openlocfilehash: 4d2558dec8422e198477c30097c20a875addeea4
ms.sourcegitcommit: 80ea62d6c0ee25d73750437fe1e37df5224d5797
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/26/2021
ms.locfileid: "105619498"
---
# <a name="change-the-billing-state-of-a-subscription-for-a-user"></a>ユーザーのサブスクリプションに関する請求の状態を変更する

特定のユーザーのサブスクリプション アドオンの請求状態を変更するには、Microsoft Store 購入 API の以下のメソッドを使います。 サブスクリプションのキャンセル、延長、払い戻し、または自動更新の無効化を行えます。

> [!NOTE]
> このメソッドは、ユニバーサル Windows プラットフォーム (UWP) アプリ用のサブスクリプション アドオンを作成できるように Microsoft がプロビジョニングした開発者アカウントでのみ使うことができます。 現在のところ、サブスクリプション アドオンはほとんどの開発者アカウントで使うことができません。

## <a name="prerequisites"></a>前提条件

このメソッドを使用するための要件:

* 対象ユーザー URI の値が `https://onestore.microsoft.com` の Azure AD アクセス トークン。
* 変更対象のサブスクリプションの権利を持つユーザーの ID を表す Microsoft Store ID キー。

詳しくは、「[サービスによる製品の権利の管理](view-and-grant-products-from-a-service.md)」をご覧ください。

## <a name="request"></a>要求


### <a name="request-syntax"></a>要求の構文

| 認証方法 | 要求 URI                                            |
|--------|--------------------------------------------------------|
| POST   | ```https://purchase.mp.microsoft.com/v8.0/b2b/recurrences/{recurrenceId}/change``` |


### <a name="request-header"></a>要求ヘッダー

| Header         | Type   | 説明   |
|----------------|--------|-------------|
| 承認  | string | 必須。 ベアラートークン形式の Azure AD アクセストークン &lt;  &gt; 。                           |
| Host           | string | 値 **purchase.mp.microsoft.com** に設定する必要があります。                                            |
| Content-Length | number | 要求本文の長さです。                                                                       |
| Content-Type   | string | 要求と応答の種類を指定します。 現時点では、サポートされている唯一の値は **application/json** です。 |


### <a name="request-parameters"></a>要求パラメーター

| 名前         | Type  | 説明   |  必須  |
|----------------|--------|-------------|-----------|
| recurrenceId | string | 変更するサブスクリプションの ID。 この ID を取得するには、 [ユーザーメソッドの get サブスクリプション](get-subscriptions-for-a-user.md) を呼び出し、変更するサブスクリプションアドオンを表す応答本文のエントリを特定して、エントリの **id** フィールドの値を使用します。     | はい      |


### <a name="request-body"></a>要求本文

| フィールド      | Type   | 説明   | 必須 |
|----------------|--------|---------------|----------|
| b2bKey         | string | 変更対象のサブスクリプションを所有するユーザーの ID を表す [Microsoft Store ID キー](view-and-grant-products-from-a-service.md#step-4)。     | はい      |
| changeType     | string |  加える変更の種類を識別する次のいずれかの文字列です。<ul><li>**Cancel**: サブスクリプションをキャンセルします。</li><li>**Extend**: サブスクリプションを延長します。 この値を指定する場合、*extensionTimeInDays* パラメーターも含める必要があります。</li><li>**Refund**: サブスクリプションを顧客に払い戻します。</li><li>**ToggleAutoRenew**: サブスクリプションの自動更新を無効にします。 サブスクリプションの自動更新が現在無効になっている場合、この値は何も行いません。</li></ul>   | はい      |
| extensionTimeInDays  | string  | *changeType* パラメーターの値が **Extend** の場合、このパラメーターはサブスクリプションを延長する日数を指定します。 |  *changeType* の値が **Extend** の場合は必須、それ以外の場合は必須ではありません。  |


### <a name="request-example"></a>要求の例

次の例は、このメソッドを使ってサブスクリプション期間を 5 日延長する方法を示しています。 *b2bKey* の値は、変更対象のサブスクリプションを持つユーザーの ID を表す [Microsoft Store ID キー](view-and-grant-products-from-a-service.md#step-4)に置き換えてください。

```json
POST https://purchase.mp.microsoft.com/v8.0/b2b/recurrences/mdr:0:bc0cb6960acd4515a0e1d638192d77b7:77d5ebee-0310-4d23-b204-83e8613baaac/change HTTP/1.1
Authorization: Bearer <your access token>
Content-Type: application/json
Host: https://purchase.mp.microsoft.com

{
  "b2bKey":  "eyJ0eXAiOiJ...",
  "changeType": "Extend",
  "extensionTimeInDays": "5"
}
```


## <a name="response"></a>Response

このメソッドは、変更されたすべてのフィールドを含む、変更されたサブスクリプション アドオンに関する情報を含む JSON 応答本文を返します。 このメソッドの応答本文を次の例に示します。

```json
{
  "items": [
    {
      "autoRenew":true,
      "beneficiary":"pub:gFVuEBiZHPXonkYvtdOi+tLE2h4g2Ss0ZId0RQOwzDg=",
      "expirationTime":"2017-06-16T03:07:49.2552941+00:00",
      "id":"mdr:0:bc0cb6960acd4515a0e1d638192d77b7:77d5ebee-0310-4d23-b204-83e8613baaac",
      "lastModified":"2017-01-10T21:08:13.1459644+00:00",
      "market":"US",
      "productId":"9NBLGGH52Q8X",
      "skuId":"0024",
      "startTime":"2017-01-10T21:07:49.2552941+00:00",
      "recurrenceState":"Active"
    }
  ]
}
```


### <a name="response-body"></a>応答本文

応答本文には、次のデータが含まれています。

| 値        | Type   | 説明                                                                 |
|---------------|--------|-----------------------------------------------|
| autoRenew | Boolean |  現在のサブスクリプション期間の終了時にサブスクリプションが自動的に更新されるように構成されているかどうかを示します。   |
| beneficiary | string |  このサブスクリプションに関連付けられている権利の受益者 ID。   |
| expirationTime | string | サブスクリプションの有効期限が切れる日時 (ISO 8601形式)。 このフィールドは、サブスクリプションが特定の状態のときのみ利用可能です。 有効期限は通常、現在の状態の有効期限がいつ切れるかを示します。 たとえば、アクティブなサブスクリプションの場合、有効期限日は次の自動更新がいつ行われるかを示します。    |
| expirationTimeWithGrace | string | 猶予期間 (ISO 8601 形式) を含むサブスクリプションの有効期限が切れる日付と時刻。 この値は、サブスクリプションが自動的に更新されなかった後に、ユーザーがサブスクリプションにアクセスできなくなったことを示します。    |
| id | string |  サブスクリプションの ID です。 [ユーザーのサブスクリプションの請求状態を変更する](change-the-billing-state-of-a-subscription-for-a-user.md)メソッドを呼び出すときに変更するサブスクリプションを指定するには、この値を使います。    |
| isTrial | Boolean |  サブスクリプションが試用版であるかどうかを示します。     |
| lastModified | string |  サブスクリプションが前回変更された日時 (ISO 8601形式)。      |
| market | string | ユーザーがサブスクリプションを取得した国コード (2 文字の ISO 3166-1 alpha-2 形式)。      |
| productId | string | Microsoft Store カタログ内のサブスクリプション アドオンを表す[製品](in-app-purchases-and-trials.md#products-skus-and-availabilities)の [Store ID](in-app-purchases-and-trials.md#store-ids)。 製品のストア ID の例は、9NBLGGH42CFD です。     |
| skuId | string |  Microsoft Store カタログ内のサブスクリプション アドオンを表す [SKU](in-app-purchases-and-trials.md#products-skus-and-availabilities) の [Store ID](in-app-purchases-and-trials.md#store-ids)。 SKU の Store ID の例は、0010 です。    |
| startTime | string |  サブスクリプションの開始日時 (ISO 8601 形式)。     |
| recurrenceState | string  |  次のいずれかの値です。<ul><li>**None**: &nbsp; &nbsp; 永続的なサブスクリプションを示します。</li><li>[**アクティブ**]: &nbsp; &nbsp; サブスクリプションがアクティブで、ユーザーはそのサービスを使用する権利を持っています。</li><li>[**非アクティブ**]: &nbsp; &nbsp; サブスクリプションが有効期限を過ぎており、ユーザーがサブスクリプションの自動更新オプションを無効にしています。</li><li>[**キャンセル**]: サブスクリプションは、 &nbsp; &nbsp; 有効期限の前に意図的に終了されました。返金はありません。</li><li>**InDunning**: &nbsp; &nbsp; サブスクリプションは *催促* にあります (つまり、サブスクリプションの有効期限が近づいていて、Microsoft はサブスクリプションを自動的に更新するために資金を取得しようとしています)。</li><li>**失敗**: &nbsp; &nbsp; 催促期間が超過し、いくつかの試行後にサブスクリプションを更新できませんでした。</li></ul><p>**注:**</p><ul><li>**非アクティブ** /**キャンセル** / 済み **失敗** はターミナルの状態です。 サブスクリプションがこれらのいずれかの状態になると、ユーザーはサブスクリプションを再購入してもう一度アクティブ化する必要があります。 ユーザーは、これらの状態でサービスを使うことはできません。</li><li>サブスクリプションが **Canceled** になると、キャンセルの日時によって expirationTime が更新されます。</li><li>サブスクリプションの ID は、有効期限が終了するまで同じままです。 自動更新オプションがオンでもオフでも変更されません。 終了状態に達した後ユーザーがサブスクリプションを再購入した場合、新しいサブスクリプション ID が作成されます。</li><li>サブスクリプションの ID は、個々のサブスクリプションで任意の操作を実行するために使ってください。</li><li>ユーザーがサブスクリプションをキャンセルまたは中止した後に再購入した場合、ユーザーの結果を照会すると、終了状態の古いサブスクリプション ID を含むエントリとアクティブ状態の新しいサブスクリプション ID を含むエントリの 2 つが返されます。</li><li>必ず、recurrenceState と expirationTime の両方を確認することをお勧めします。recurrenceState の更新は数分 (場合によって数時間) 遅れることがあるためです。       |
| cancellationDate | string   |  ユーザーのサブスクリプションがキャンセルされた日時 (ISO 8601 形式)。     |


## <a name="related-topics"></a>関連トピック


* [サービスによる製品の権利の管理](view-and-grant-products-from-a-service.md)
* [ユーザーのサブスクリプションの取得](get-subscriptions-for-a-user.md)
* [製品の照会](query-for-products.md)
* [コンシューマブルな製品をフルフィルメント完了として報告する](report-consumable-products-as-fulfilled.md)
* [Microsoft Store ID キーの更新](renew-a-windows-store-id-key.md)
