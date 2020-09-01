---
description: Microsoft Store analytics API でこのメソッドを使用して、デスクトップアプリケーションの洞察データを取得します。
title: デスクトップ アプリケーションのインサイト データの取得
ms.date: 07/31/2018
ms.topic: article
keywords: windows 10、uwp、ストアサービス、Microsoft Store analytics API、洞察
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: bd60425a5ec3c040417aded818c766db80eb59eb
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172896"
---
# <a name="get-insights-data-for-your-desktop-application"></a>デスクトップ アプリケーションのインサイト データの取得

[Windows デスクトップアプリケーションプログラム](/windows/desktop/appxpkg/windows-desktop-application-program)に追加したデスクトップアプリケーションの正常性メトリックに関連する洞察データを取得するには、MICROSOFT STORE analytics API でこのメソッドを使用します。 このデータは、パートナーセンターのデスクトップアプリケーションの [状態レポート](/windows/desktop/appxpkg/windows-desktop-application-program#health-report) でも使用できます。

## <a name="prerequisites"></a>前提条件

このメソッドを使うには、最初に次の作業を行う必要があります。

* Microsoft Store 分析 API に関するすべての[前提条件](access-analytics-data-using-windows-store-services.md#prerequisites)を満たします (前提条件がまだ満たされていない場合)。
* このメソッドの要求ヘッダーで使う [Azure AD アクセス トークンを取得](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token)します。 アクセス トークンを取得した後、アクセス トークンを使用できるのは、その有効期限が切れるまでの 60 分間です。 トークンの有効期限が切れたら新しいトークンを取得できます。

## <a name="request"></a>Request


### <a name="request-syntax"></a>要求の構文

| 認証方法 | 要求 URI       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/insights``` |


### <a name="request-header"></a>要求ヘッダー

| Header        | Type   | 説明                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 承認 | string | 必須。 **Bearer** &lt;*トークン*&gt; という形式の Azure AD アクセス トークン。 |


### <a name="request-parameters"></a>要求パラメーター

| パラメーター        | Type   |  説明      |  必須  
|---------------|--------|---------------|------|
| applicationId | string | 洞察データを取得するデスクトップアプリケーションの製品 ID。 デスクトップアプリケーションの製品 ID を取得するには、パートナーセンター (**正常性レポート**など) の[デスクトップアプリケーションの分析レポート](/windows/desktop/appxpkg/windows-desktop-application-program)を開き、URL から製品 id を取得します。 このパラメーターを指定しない場合、応答本文には、アカウントに登録されているすべてのアプリの分析情報データが含まれます。  |  いいえ  |
| startDate | 日付 | 取得するインサイトデータの日付範囲の開始日。 既定値は、現在の日付の 30 日前です。 |  いいえ  |
| endDate | 日付 | 取得するインサイトデータの日付範囲の終了日。 既定値は現在の日付です。 |  いいえ  |
| filter | string  | 応答内の行をフィルター処理する 1 つまたは複数のステートメントです。 各ステートメントには、応答本文からのフィールド名、および **eq** 演算子または **ne** 演算子と関連付けられる値が含まれており、**and** や **or** を使用してステートメントを組み合わせることができます。 *filter* パラメーターでは、文字列値を単一引用符で囲む必要があります。 たとえば、 *filter = dataType eq ' 取得 '* です。 <p/><p/>現在、このメソッドはフィルターの **正常性**のみをサポートしています。  | いいえ   |

### <a name="request-example"></a>要求の例

次の例は、洞察データを取得するための要求を示しています。 *ApplicationId*の値を、使用するデスクトップアプリケーションの適切な値に置き換えます。

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/insights?applicationId=10238467886765136388&startDate=6/1/2018&endDate=6/15/2018&filter=dataType eq 'health' HTTP/1.1
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
| applicationId       | string | Insights データを取得したデスクトップアプリケーションの製品 ID。     |
| insightDate                | string | 特定のメトリックの変更を特定した日付。 この日付は、メトリックがその前の週と比較して大幅に増加または減少したことが検出された週の終わりを表します。 |
| dataType     | string | この洞察によって通知される一般的な分析領域を指定する文字列。 現在、このメソッドは **正常性**のみをサポートしています。    |
| insightDetail          | array | 現在の洞察の詳細を表す1つ以上の [InsightDetail 値](#insightdetail-values) 。    |


### <a name="insightdetail-values"></a>InsightDetail の値

| 値               | Type   | 説明                           |
|---------------------|--------|-------------------------------------------|
| FactName           | string | 現在の洞察または現在のディメンションによって記述されるメトリックを示す文字列。 現在、このメソッドでは値の **ヒットカウント**のみがサポートされています。  |
| サブディメンション         | array |  洞察の1つのメトリックを記述する1つ以上のオブジェクト。   |
| PercentChange            | string |  顧客ベース全体でメトリックが変更された割合。  |
| DimensionName           | string |  現在のディメンションに記述されているメトリックの名前。 例として、 **EventType**、 **Market**、 **(devicetype**、 **PackageVersion**などがあります。   |
| DimensionValue              | string | 現在のディメンションに記述されているメトリックの値。 たとえば、 **DimensionName** が **EventType**の場合、 **dimensionvalue** が **クラッシュ** または **ハング**する可能性があります。   |
| FactValue     | string | 洞察が検出された日付のメトリックの絶対値。  |
| 方向 | string |  変更の方向 (**正** または **負**)。   |
| Date              | string |  現在の洞察または現在のディメンションに関連する変更を特定した日付。   |

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

* [Windows デスクトップアプリケーションプログラム](/windows/desktop/appxpkg/windows-desktop-application-program)
* [状態レポート](/windows/desktop/appxpkg/windows-desktop-application-program#health-report)
* [Microsoft Store サービスを使った分析データへのアクセス](access-analytics-data-using-windows-store-services.md)