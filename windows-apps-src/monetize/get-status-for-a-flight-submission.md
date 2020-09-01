---
ms.assetid: C78176D6-47BB-4C63-92F8-426719A70F04
description: パッケージ フライトの申請の状態を取得するには、Microsoft Store 申請 API の以下のメソッドを使います。
title: パッケージ フライトの申請の状態の取得
ms.date: 04/17/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store 申請 API, フライトの申請, 状態
ms.localizationpriority: medium
ms.openlocfilehash: 28eb08ff5f1dd6a5c7887c100edfe2f38780f6fe
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175016"
---
# <a name="get-the-status-of-a-package-flight-submission"></a>パッケージ フライトの申請の状態の取得

パッケージ フライトの申請の状態を取得するには、Microsoft Store 申請 API の以下のメソッドを使います。 Microsoft Store 申請 API を使ったパッケージ フライトの申請の作成プロセスについて詳しくは、「[パッケージ フライトの申請の管理](manage-flight-submissions.md)」をご覧ください。

## <a name="prerequisites"></a>前提条件

このメソッドを使うには、最初に次の作業を行う必要があります。

* Microsoft Store 申請 API に関するすべての[前提条件](create-and-manage-submissions-using-windows-store-services.md#prerequisites)を満たします (前提条件がまだ満たされていない場合)。
* このメソッドの要求ヘッダーで使う [Azure AD アクセス トークンを取得](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token)します。 アクセス トークンを取得した後、アクセス トークンを使用できるのは、その有効期限が切れるまでの 60 分間です。 トークンの有効期限が切れたら新しいトークンを取得できます。
* アプリの1つに対して、パッケージのフライト送信を作成します。 これはパートナーセンターで行うことができます。また、[ [パッケージの作成] フライト送信](create-a-flight-submission.md) 方法を使用して行うこともできます。

## <a name="request"></a>要求

このメソッドの構文は次のとおりです。 ヘッダーと要求本文の使用例と説明については、次のセクションをご覧ください。

| 認証方法 | 要求 URI                                                      |
|--------|------------------------------------------------------------------|
| GET   | `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/status` |


### <a name="request-header"></a>要求ヘッダー

| Header        | Type   | 説明                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 承認 | string | 必須。 **Bearer** &lt;*トークン*&gt; という形式の Azure AD アクセス トークン。 |


### <a name="request-parameters"></a>要求パラメーター

| 名前        | Type   | 説明                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | string | 必須。 状態を取得するパッケージ フライトの申請が含まれているアプリのストア ID です。 ストア ID について詳しくは、「[アプリ ID の詳細の表示](../publish/view-app-identity-details.md)」をご覧ください。  |
| flightId | string | 必須。 状態を取得する申請が含まれているパッケージ フライトの ID です。 この ID は、[パッケージ フライトの作成](create-a-flight.md)要求と[アプリのパッケージ フライトの取得](get-flights-for-an-app.md)要求の応答データで確認できます。 パートナーセンターで作成されたフライトの場合、この ID は、パートナーセンターのフライトページの URL でも利用できます。  |
| submissionId | string | 必須。 状態を取得する申請の ID です。 この ID は、[パッケージ フライトの申請の作成](create-a-flight-submission.md)要求に対する応答データで確認できます。 パートナーセンターで作成された送信の場合、この ID はパートナーセンターの [送信] ページの URL でも利用できます。  |


### <a name="request-body"></a>[要求本文]

このメソッドでは要求本文を指定しないでください。

### <a name="request-example"></a>要求の例

次の例は、パッケージ フライトの申請の状態を取得する方法を示しています。

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights/43e448df-97c9-4a43-a0bc-2a445e736bcd/submissions/1152921504621243649/status HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>応答

次の例は、このメソッドが正常に呼び出された場合の JSON 応答本文を示しています。 応答本文には、指定された申請に関する情報が含まれています。 応答本文の値について詳しくは、次のセクションをご覧ください。

```json
{
  "status": "PendingCommit",
  "statusDetails": {
    "errors": [],
    "warnings": [],
    "certificationReports": []
  },
}
```

### <a name="response-body"></a>応答本文

| 値      | Type   | 説明                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| status           | string  | 申請の状態。 次のいずれかの値を指定できます。 <ul><li>なし</li><li>Canceled</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>発行</li><li>公開済み</li><li>PublishFailed</li><li>PreProcessing</li><li>PreProcessingFailed</li><li>認定</li><li>CertificationFailed</li><li>リリース</li><li>ReleaseFailed</li></ul>   |
| statusDetails           | object  |  エラーに関する情報など、申請の状態に関する追加詳細情報が含まれています。 詳しくは、[ステータスの詳細に関するリソース](manage-flight-submissions.md#status-details-object)をご覧ください。 |


## <a name="error-codes"></a>エラー コード

要求を正常に完了できない場合、次の HTTP エラー コードのいずれかが応答に含まれます。

| エラー コード |  説明   |
|--------|------------------|
| 404  | 申請は見つかりませんでした。 |
| 409  | このアプリでは、 [Microsoft Store 送信 API で現在サポート](create-and-manage-submissions-using-windows-store-services.md#not_supported)されていないパートナーセンター機能を使用しています。  |


## <a name="related-topics"></a>関連トピック

* [Microsoft Store サービスを使用した申請の作成と管理](create-and-manage-submissions-using-windows-store-services.md)
* [パッケージ フライトの申請の管理](manage-flight-submissions.md)
* [アプリの申請の取得](get-an-app-submission.md)
* [アプリの申請の作成](create-an-app-submission.md)
* [アプリの申請のコミット](commit-an-app-submission.md)
* [アプリの申請の更新](update-an-app-submission.md)
* [アプリの申請の削除](delete-an-app-submission.md)