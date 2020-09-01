---
description: Xbox Live の同時使用状況データを取得するには、Microsoft Store 分析 API の以下のメソッドを使います。
title: Xbox Live の同時使用状況データの取得
ms.date: 06/04/2018
ms.topic: article
keywords: Windows 10, UWP, Store サービス, Microsoft Store 分析 API, Xbox Live 分析, 同時使用状況
ms.localizationpriority: medium
ms.openlocfilehash: 9e8d36ce9336d5fc65a73a19233b8a1f0a05a8bf
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167586"
---
# <a name="get-xbox-live-concurrent-usage-data"></a>Xbox Live の同時使用状況データの取得


[Xbox Live 対応ゲーム](/gaming/xbox-live/index.md)をプレイしたユーザーの平均数について、分単位、時間単位、または日単位でリアルタイムに近い使用状況データ (5 ～ 15 分の遅延) を取得するには、Microsoft Store 分析 API の以下のメソッドを使います。 この情報は、パートナーセンターの [Xbox analytics レポート](../publish/xbox-analytics-report.md) でも確認できます。

> [!IMPORTANT]
> このメソッドは、Xbox のゲームまたは Xbox Live サービスを使用するゲームのみサポートします。 これらのゲームは、[概念の承認プロセス](../gaming/concept-approval.md)を完了する必要があります。これには、[Microsoft パートナー](/gaming/xbox-live/developer-program-overview.md#microsoft-partners)が発行したゲームと [ID@Xbox プログラム](/gaming/xbox-live/developer-program-overview.md#id)を介して申請されたゲームが含まれます。 このメソッドでは、[Xbox Live クリエーターズ プログラム](/gaming/xbox-live/get-started-with-creators/get-started-with-xbox-live-creators.md)を介して発行されたゲームは現在サポートされていません。

## <a name="prerequisites"></a>前提条件

このメソッドを使うには、最初に次の作業を行う必要があります。

* Microsoft Store 分析 API に関するすべての[前提条件](access-analytics-data-using-windows-store-services.md#prerequisites)を満たします (前提条件がまだ満たされていない場合)。
* このメソッドの要求ヘッダーで使う [Azure AD アクセス トークンを取得](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token)します。 アクセス トークンを取得した後、アクセス トークンを使用できるのは、その有効期限が切れるまでの 60 分間です。 トークンの有効期限が切れたら新しいトークンを取得できます。

## <a name="request"></a>Request


### <a name="request-syntax"></a>要求の構文

| 認証方法 | 要求 URI       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics``` |


### <a name="request-header"></a>要求ヘッダー

| Header        | Type   | 説明                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 承認 | string | 必須。 **Bearer** &lt;*トークン*&gt; という形式の Azure AD アクセス トークン。 |


### <a name="request-parameters"></a>要求パラメーター


| パラメーター        | Type   |  説明      |  必須  
|---------------|--------|---------------|------|
| applicationId | string | Xbox Live の同時使用状況データを取得するゲームの [Store ID](in-app-purchases-and-trials.md#store-ids) です。  |  はい  |
| metricType | string | 取得する Xbox Live 分析データの種類を指定する文字列です。 このメソッドでは、値 **concurrency** を指定します。  |  はい  |
| startDate | 日付 | 取得する同時使用状況データの日付範囲の開始日です。 既定の動作については *aggregationLevel* の説明をご覧ください。 |  いいえ  |
| endDate | 日付 | 取得する同時使用状況データの日付範囲の終了日です。 既定の動作については *aggregationLevel* の説明をご覧ください。 |  いいえ  |
| aggregationLevel | string | 集計データを取得する時間範囲を指定します。 **minute**、**hour**、**day** のいずれかの文字列を指定できます。 指定しない場合、既定値は **day** です。 <p/><p/>*startDate* または *endDate* を指定しなかった場合、応答本文は既定で次のようになります。 <ul><li>**minute**: 利用可能なデータの最新の 60 レコード。</li><li>**hour**: 利用可能なデータの最新の 24 レコード</li><li>**day**: 利用可能なデータの最新の 7 レコード。</li></ul><p/>各集計レベルでは、返すことのできるレコード数にサイズ制限があります。 要求された期間が大きすぎる場合、レコードは切り捨てられます。 <ul><li>**minute**: 最大 1440 レコード (24 時間のデータ)。</li><li>**hour**: 最大 720 レコード (30 日間のデータ)。</li><li>**day**: 最大 60 レコード (60 日間のデータ)。</li></ul>  |  いいえ  |


### <a name="request-example"></a>要求の例

次の例では、Xbox Live 対応ゲームの同時使用状況データを取得する要求を示します。 この要求は、2018 年 2 月 1 日から 2018 年 2 月 2 日までの毎分のデータを取得します。 *applicationId* の値は、ゲームの Store ID に置き換えてください。

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics?applicationId=9NBLGGGZ5QDR&metrictype=concurrency&aggregationLevel=hour&startDate=2018-02-01&endData=2018-02-02 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>応答

応答本文にはオブジェクトの配列が含まれ、各要素のオブジェクトは、指定された分、時間、または日に対応する同時使用状況データのセットを 1 つ保持します。 各オブジェクトには次の値が含まれます。

| 値      | Type   | 説明                  |
|------------|--------|-------------------------------------------------------|
| Count      | number  | 指定された分、時間、または日に Xbox Live 対応ゲームをプレイしたユーザーの平均数です。 <p/><p/>**Note** &nbsp; メモ &nbsp;値0は、指定された期間中に同時実行ユーザーが存在しなかったか、または指定された期間にゲームの同時ユーザーデータを収集するときにエラーが発生したことを示します。 |
| Date  | string | 同時使用状況データが発生した分、時間、または日を指定する日付と時刻です。  |
| SeriesName | string    | この値は常に **UserConcurrency** になります。 |


### <a name="response-example"></a>応答の例

分単位のデータ集計要求に対する JSON 応答本文の例を次に示します。

```json
[   {
        "Count": 418.0,
        "Date": "2018-02-02T04:42:13.65Z",
        "SeriesName": "UserConcurrency"
    }, {
        "Count": 418.0,
        "Date": "2018-02-02T04:43:13.65Z",
        "SeriesName": "UserConcurrency"
    }, {
        "Count": 415.0,
        "Date": "2018-02-02T04:44:13.65Z",
        "SeriesName": "UserConcurrency"
    }, {
        "Count": 412.0,
        "Date": "2018-02-02T04:45:13.65Z",
        "SeriesName": "UserConcurrency"
    }, {
        "Count": 414.0,
        "Date": "2018-02-02T04:46:13.65Z",
        "SeriesName": "UserConcurrency"
    }
]
```

## <a name="related-topics"></a>関連トピック

* [Microsoft Store サービスを使った分析データへのアクセス](access-analytics-data-using-windows-store-services.md)
* [Xbox Live の分析データの取得](get-xbox-live-analytics.md)
* [Xbox Live の実績データの取得](get-xbox-live-achievements-data.md)
* [Xbox Live の正常性データの取得](get-xbox-live-health-data.md)
* [Xbox Live game hub データを取得する](get-xbox-live-game-hub-data.md)
* [Xbox Live クラブのデータの取得](get-xbox-live-club-data.md)
* [Xbox Live のマルチプレイヤー データの取得](get-xbox-live-multiplayer-data.md)