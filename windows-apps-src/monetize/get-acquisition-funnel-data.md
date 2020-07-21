---
description: 日付範囲やその他のオプション フィルターを指定して、アプリケーションの入手に関するファネル データを取得するには、Microsoft Store 分析 API の以下のメソッドを使います。
title: アプリの入手に関するファネル データの取得
ms.date: 08/04/2017
ms.topic: article
keywords: Windows 10, UWP, Store サービス, Microsoft Store 分析 API, 入手, ファネル
ms.localizationpriority: medium
ms.openlocfilehash: 817ad85bae642d6b09cf77bb5e7b4bf71b08f594
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493557"
---
# <a name="get-app-acquisition-funnel-data"></a>アプリの入手に関するファネル データの取得

日付範囲やその他のオプション フィルターを指定して、アプリケーションの入手に関するファネル データを取得するには、Microsoft Store 分析 API の以下のメソッドを使います。 この情報は、パートナーセンターの[購入レポート](../publish/acquisitions-report.md#acquisition-funnel)でも確認できます。

## <a name="prerequisites"></a>前提条件


このメソッドを使うには、最初に次の作業を行う必要があります。

* Microsoft Store 分析 API に関するすべての[前提条件](access-analytics-data-using-windows-store-services.md#prerequisites)を満たします (前提条件がまだ満たされていない場合)。
* このメソッドの要求ヘッダーで使う [Azure AD アクセス トークンを取得](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token)します。 アクセス トークンを取得した後、アクセス トークンを使用できるのは、その有効期限が切れるまでの 60 分間です。 トークンの有効期限が切れたら新しいトークンを取得できます。

## <a name="request"></a>Request


### <a name="request-syntax"></a>要求の構文

| 認証方法 | 要求 URI       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/funnel``` |


### <a name="request-header"></a>要求ヘッダー

| Header        | 種類   | 説明                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 承認 | string | 必須。 **Bearer** &lt;*トークン*&gt; という形式の Azure AD アクセス トークン。 |


### <a name="request-parameters"></a>要求パラメーター

| パラメーター        | 種類   |  説明      |  必須  
|---------------|--------|---------------|------|
| applicationId | string | 入手に関するファネル データを取得するアプリの [ストア ID](in-app-purchases-and-trials.md#store-ids) です。 ストア ID は、たとえば 9WZDNCRFJ3Q8 のような文字列です。 |  はい  |
| startDate | date | 取得する入手に関するファネル データの日付範囲開始日です。 既定値は現在の日付です。 |  いいえ  |
| endDate | 日付 | 取得する入手に関するファネル データの日付範囲終了日です。 既定値は現在の日付です。 |  いいえ  |
| filter | string  | 応答内の行をフィルター処理する 1 つまたは複数のステートメントです。 詳しくは、次の「[フィルター フィールド](#filter-fields)」セクションをご覧ください。 | いいえ   |

 
### <a name="filter-fields"></a>フィルター フィールド

要求の *filter* パラメーターには、応答内の行をフィルター処理する 1 つまたは複数のステートメントが含まれます。 各ステートメントには **eq** 演算子または **ne** 演算子と関連付けられるフィールドと値が含まれ、**and** または **or** を使ってステートメントを組み合わせることができます。

以下のフィルター フィールドがサポートされています。 *filter* パラメーターでは、文字列値を単一引用符で囲む必要があります。

| フィールド        |  説明        |
|---------------|-----------------|
| campaignId | 入手に関連付けられている[カスタム アプリ プロモーション キャンペーン](../publish/create-a-custom-app-promotion-campaign.md)の ID 文字列です。 |
| market | 入手が発生した市場の ISO 3166 国コードを含む文字列です。 |
| deviceType | 入手が発生したデバイスの種類を指定する、以下のいずれかの文字列です。<ul><li><strong>PC</strong></li><li><strong>ダイヤル</strong></li><li><strong>コンソール-Xbox One</strong></li><li><strong>コンソール-Xbox シリーズ X</strong></li><li><strong>IoT</strong></li><li><strong>Holographic</strong></li><li><strong>Unknown</strong></li></ul> |
| ageGroup | 入手を完了したユーザーの年齢グループを指定する、以下のいずれかの文字列です。<ul><li><strong>0 – 17</strong></li><li><strong>18 – 24</strong></li><li><strong>25 – 34</strong></li><li><strong>35 – 49</strong></li><li><strong>50 or over</strong></li><li><strong>Unknown</strong></li></ul> |
| gender | 入手を完了したユーザーの性別を指定する、以下のいずれかの文字列です。<ul><li><strong>M</strong></li><li><strong>F</strong></li><li><strong>Unknown</strong></li></ul> |


### <a name="request-example"></a>要求の例

アプリの入手に関するファネル データを取得するための要求の例を、いくつか次に示します。 *applicationId* 値を、目的のアプリのストア ID に置き換えてください。

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/funnel?applicationId=9NBLGGGZ5QDR&startDate=1/1/2017&endDate=2/1/2017  HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/funnel?applicationId=9NBLGGGZ5QDR&startDate=8/1/2016&endDate=8/31/2016&filter=market eq 'US' and gender eq 'm'  HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>[応答]


### <a name="response-body"></a>応答本文

| 値      | 型   | 説明                  |
|------------|--------|-------------------------------------------------------|
| 値      | array  | アプリに関する入手のファネル データが含まれるオブジェクトの配列です。 各オブジェクトのデータについて詳しくは、次の「[ファネル値](#funnel-values)」セクションをご覧ください。                  |
| TotalCount | INT    | *Value* 配列のオブジェクトの合計数です。        |


### <a name="funnel-values"></a>ファネル値

*Value* 配列のオブジェクトには、次の値が含まれます。

| 値               | 種類   | 説明                           |
|---------------------|--------|-------------------------------------------|
| MetricType                | string | このオブジェクトに含まれる[ファネル データの種類](../publish/acquisitions-report.md#acquisition-funnel)を指定する以下のいずれかの文字列です。<ul><li><strong>ページビュー</strong></li><li><strong>取得</strong></li><li><strong>インストール</strong></li><li><strong>使用方法</strong></li></ul> |
| UserCount       | string | *MetricType* 値によって指定されたファネル手順を実行したユーザーの数です。             |


### <a name="response-example"></a>応答の例

この要求の JSON 返信の本文の例を次に示します。

```json
{
  "Value": [
    {
      "MetricType": "PageView",
      "UserCount": 100
    },
    {
      "MetricType": "Acquisition",
      "UserCount": 80
    },
    {
      "MetricType": "Install",
      "UserCount": 50
    },
    {
      "MetricType": "Usage",
      "UserCount": 10
    }
  ],
  "TotalCount": 4
}
```

## <a name="related-topics"></a>関連トピック

* [取得レポート](../publish/acquisitions-report.md)
* [Microsoft Store サービスを使った分析データへのアクセス](access-analytics-data-using-windows-store-services.md)
* [アプリの入手数の取得](get-app-acquisitions.md)
