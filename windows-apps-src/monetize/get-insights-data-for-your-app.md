---
description: アプリの洞察データを取得するには、Microsoft Store analytics API でこのメソッドを使用します。
title: 洞察データを取得する
ms.date: 07/31/2018
ms.topic: article
keywords: windows 10、uwp、ストアサービス、Microsoft Store analytics API、洞察
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: cebd415b00268c9a5e8febbe175347345d830c23
ms.sourcegitcommit: 368753aea2792984857f6a57a22daed1035f1a33
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/11/2020
ms.locfileid: "97349721"
---
# <a name="get-insights-data"></a>洞察データを取得する

Microsoft Store analytics API でこのメソッドを使用して、特定の日付範囲とその他のオプションのフィルターで、アプリの購入、正常性、および使用状況のメトリックに関する洞察データを取得します。 この情報は、パートナーセンターの [Insights レポート](../publish/insights-report.md) でも確認できます。

## <a name="prerequisites"></a>前提条件


このメソッドを使うには、最初に次の作業を行う必要があります。

* Microsoft Store 分析 API に関するすべての[前提条件](access-analytics-data-using-windows-store-services.md#prerequisites)を満たします (前提条件がまだ満たされていない場合)。
* このメソッドの要求ヘッダーで使う [Azure AD アクセス トークンを取得](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token)します。 アクセス トークンを取得した後、アクセス トークンを使用できるのは、その有効期限が切れるまでの 60 分間です。 トークンの有効期限が切れたら新しいトークンを取得できます。

## <a name="request"></a>Request


### <a name="request-syntax"></a>要求の構文

| 認証方法 | 要求 URI       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/insights``` |


### <a name="request-header"></a>要求ヘッダー

| Header        | Type   | 説明                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 承認 | string | 必須。 **Bearer** &lt;*トークン*&gt; という形式の Azure AD アクセス トークン。 |


### <a name="request-parameters"></a>要求パラメーター

| パラメーター        | Type   |  説明      |  必須  
|---------------|--------|---------------|------|
| applicationId | string | Insights データを取得するアプリの [ストア ID](in-app-purchases-and-trials.md#store-ids) 。 このパラメーターを指定しない場合、応答本文には、アカウントに登録されているすべてのアプリの分析情報データが含まれます。  |  いいえ  |
| startDate | 日付 | 取得するインサイトデータの日付範囲の開始日。 既定値は、現在の日付の 30 日前です。 |  いいえ  |
| endDate | 日付 | 取得するインサイトデータの日付範囲の終了日。 既定値は現在の日付です。 |  いいえ  |
| filter | string  | 応答内の行をフィルター処理する 1 つまたは複数のステートメントです。 各ステートメントには、応答本文からのフィールド名、および **eq** 演算子または **ne** 演算子と関連付けられる値が含まれており、**and** や **or** を使用してステートメントを組み合わせることができます。 *filter* パラメーターでは、文字列値を単一引用符で囲む必要があります。 たとえば、 *filter = dataType eq ' 取得 '* です。 <p/><p/>次のフィルターフィールドを指定できます。<p/><ul><li><strong>買収</strong></li><li><strong>ステータス</strong></li><li><strong>ユーセジリンク</strong></li></ul> | はい   |

### <a name="request-example"></a>要求の例

次の例は、洞察データを取得するための要求を示しています。 *applicationId* 値を、目的のアプリのストア ID に置き換えてください。

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/insights?applicationId=9NBLGGGZ5QDR&startDate=6/1/2018&endDate=6/15/2018&filter=dataType eq 'acquisition' or dataType eq 'health' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>[応答]

### <a name="response-body"></a>応答本文

| 値      | Type   | 説明                  |
|------------|--------|-------------------------------------------------------|
| 値      | array  | アプリの洞察データを格納するオブジェクトの配列。 各オブジェクトのデータの詳細については、以下の「 [洞察の値](#insight-values) 」セクションを参照してください。                                                                                                                      |
| TotalCount | INT    | クエリの結果データ内の行の総数です。                 |


### <a name="insight-values"></a>洞察の値

*Value* 配列の要素には、次の値が含まれます。

| 値               | Type   | 説明                           |
|---------------------|--------|-------------------------------------------|
| applicationId       | string | Insights データを取得するアプリのストア ID。     |
| insightDate                | string | 特定のメトリックの変更を特定した日付。 この日付は、メトリックがその前の週と比較して大幅に増加または減少したことが検出された週の終わりを表します。 |
| dataType     | string | この洞察で説明されている一般的な分析領域を指定する次のいずれかの文字列。<p/><ul><li><strong>買収</strong></li><li><strong>ステータス</strong></li><li><strong>ユーセジリンク</strong></li></ul>   |
| insightDetail          | array | 現在の洞察の詳細を表す1つ以上の [InsightDetail 値](#insightdetail-values) 。    |


### <a name="insightdetail-values"></a>InsightDetail の値

| 値               | Type   | 説明                           |
|---------------------|--------|-------------------------------------------|
| FactName           | string | **データ型** の値に基づいて、現在の洞察または現在のディメンションによって記述されるメトリックを示す、次のいずれかの値を指定します。<ul><li>**正常性** の場合、この値は常に **ヒットカウント** です。</li><li>**取得** の場合、この値は常に **iap** になります。</li><li>**使用** する場合、この値は次のいずれかの文字列になります。<ul><li><strong>DailyActiveUsers</strong></li><li><strong>EngagementDurationMinutes</strong></li><li><strong>DailyActiveDevices</strong></li><li><strong>DailyNewUsers</strong></li><li><strong>DailySessionCount</strong></li></ul></ul>  |
| サブディメンション         | array |  洞察の1つのメトリックを記述する1つ以上のオブジェクト。   |
| PercentChange            | string |  顧客ベース全体でメトリックが変更された割合。  |
| DimensionName           | string |  現在のディメンションに記述されているメトリックの名前。 例としては、 **EventType**、 **Market**、 **(devicetype**、 **PackageVersion**、 **AcquisitionType**、 **agegroup** 、 **性別** などがあります。   |
| DimensionValue              | string | 現在のディメンションに記述されているメトリックの値。 たとえば、 **DimensionName** が **EventType** の場合、 **dimensionvalue** が **クラッシュ** または **ハング** する可能性があります。   |
| FactValue     | string | 洞察が検出された日付のメトリックの絶対値。  |
| Direction | string |  変更の方向 (**正** または **負**)。   |
| 日付              | string |  現在の洞察または現在のディメンションに関連する変更を特定した日付。   |

### <a name="response-example"></a>応答の例

この要求の JSON 返信の本文の例を次に示します。

```json
{
  "Value": [
    {
      "applicationId": "9NBLGGGZ5QDR",
      "insightDate": "2018-06-03T00:00:00",
      "dataType": "health",
      "insightDetail": [
        {
          "FactName": "HitCount",
          "SubDimensions": [
            {
              "FactName:": "HitCount",
              "PercentChange": "21",
              "DimensionValue:": "DE",
              "FactValue": "109",
              "Direction": "Positive",
              "Date": "6/3/2018 12:00:00 AM",
              "DimensionName": "Market"
            }
          ],
          "DimensionValue": "crash",
          "Date": "6/3/2018 12:00:00 AM",
          "DimensionName": "EventType"
        },
        {
          "FactName": "HitCount",
          "SubDimensions": [
            {
              "FactName:": "HitCount",
              "PercentChange": "71",
              "DimensionValue:": "JP",
              "FactValue": "112",
              "Direction": "Positive",
              "Date": "6/3/2018 12:00:00 AM",
              "DimensionName": "Market"
            }
          ],
          "DimensionValue": "hang",
          "Date": "6/3/2018 12:00:00 AM",
          "DimensionName": "EventType"
        },
      ],
      "insightId": "9CY0F3VBT1AS942AFQaeyO0k2zUKfyOhrOHc0036Iwc="
    }
  ],
  "@nextLink": null,
  "TotalCount": 2
}
```

## <a name="related-topics"></a>関連トピック

* [インサイト レポート](../publish/insights-report.md)
* [Microsoft Store サービスを使った分析データへのアクセス](access-analytics-data-using-windows-store-services.md)
