---
description: デスクトップ アプリケーションのエラーの CAB ファイルをダウンロードするには、Microsoft Store 分析 API の以下のメソッドを使います。
title: デスクトップ アプリケーションのエラーの CAB ファイルをダウンロードする
ms.date: 03/06/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store 分析 API, CAB のダウンロード, デスクトップ アプリケーション
ms.localizationpriority: medium
ms.openlocfilehash: 98a0d6931a59e037bb15679a20fae11eb82dae32
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162496"
---
# <a name="download-the-cab-file-for-an-error-in-your-desktop-application"></a>デスクトップ アプリケーションのエラーの CAB ファイルをダウンロードする

[Windows デスクトップ アプリケーション プログラム](/windows/desktop/appxpkg/windows-desktop-application-program)に追加したデスクトップ アプリケーションの特定のエラーに関連する CAB ファイルをダウンロードするには、Microsoft Store 分析 API の以下のメソッドを使います。 このメソッドでダウンロードできるのは、過去 30 日以内に発生したアプリのエラーに関する CAB ファイルのみです。 CAB ファイルのダウンロードは、パートナーセンターのデスクトップアプリケーションの [状態レポート](/windows/desktop/appxpkg/windows-desktop-application-program) でも利用できます。

このメソッドを使用するには、事前に「[デスクトップ アプリケーションのエラーに関する詳細情報の取得](get-details-for-an-error-in-your-desktop-application.md)」のメソッドを使って、ダウンロードする CAB ファイルの ID ハッシュを取得しておく必要があります。

## <a name="prerequisites"></a>前提条件


このメソッドを使うには、最初に次の作業を行う必要があります。

* Microsoft Store 分析 API に関するすべての[前提条件](access-analytics-data-using-windows-store-services.md#prerequisites)を満たします (前提条件がまだ満たされていない場合)。
* このメソッドの要求ヘッダーで使う [Azure AD アクセス トークンを取得](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token)します。 アクセス トークンを取得した後、アクセス トークンを使用できるのは、その有効期限が切れるまでの 60 分間です。 トークンの有効期限が切れたら新しいトークンを取得できます。
* ダウンロードする CAB ファイルの ID ハッシュを取得します。 この値を取得するには、「[デスクトップ アプリケーションのエラーに関する詳細情報の取得](get-details-for-an-error-in-your-desktop-application.md)」のメソッドを使ってアプリの特定のエラーに関する詳細情報を取得し、そのメソッドの応答本文に含まれる **cabIdHash** 値を使用します。

## <a name="request"></a>Request


### <a name="request-syntax"></a>要求の構文

| 認証方法 | 要求 URI                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/cabdownload``` |


### <a name="request-header"></a>要求ヘッダー

| Header        | Type   | 説明                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 承認 | string | 必須。 **Bearer** &lt;*トークン*&gt; という形式の Azure AD アクセス トークン。 |


### <a name="request-parameters"></a>要求パラメーター

| パラメーター        | Type   |  説明      |  必須  |
|---------------|--------|---------------|------|
| applicationId | string | CAB ファイルをダウンロードするデスクトップ アプリケーションの製品 ID です。 デスクトップアプリケーションの製品 ID を取得するには、デスクトップアプリケーション (**状態レポート**など)[のパートナーセンター分析レポート](/windows/desktop/appxpkg/windows-desktop-application-program)を開き、URL から製品 id を取得します。 |  はい  |
| cabIdHash | string | ダウンロードする CAB ファイルの一意の ID ハッシュです。 この値を取得するには、「[デスクトップ アプリケーションのエラーに関する詳細情報の取得](get-details-for-an-error-in-your-desktop-application.md)」のメソッドを使ってアプリケーションの特定のエラーに関する詳細情報を取得し、そのメソッドの応答本文に含まれる **cabIdHash** 値を使用します。 |  はい  |


### <a name="request-example"></a>要求の例

次の例は、このメソッドを使って CAB ファイルをダウンロードする方法を示しています。 *applicationId* パラメーターと *cabIdHash* パラメーターは、デスクトップ アプリケーションに合わせて適切な値に置き換えてください。

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/cabdownload?applicationId=10238467886765136388&cabIdHash=54ffb83a-e159-41d2-8158-f36f306cc01e HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>応答

このメソッドは 302 (リダイレクト) 応答コードを返します。また、応答に含まれる **Location** ヘッダーは、CAB ファイルの Shared Access Signature (SAS) URI に割り当てられます。 呼び出し元はこの URI にリダイレクトされ、CAB ファイルが自動的にダウンロードされます。

## <a name="related-topics"></a>関連トピック

* [状態レポート](../publish/health-report.md)
* [Microsoft Store サービスを使った分析データへのアクセス](access-analytics-data-using-windows-store-services.md)
* [デスクトップ アプリケーションのエラー報告データの取得](get-desktop-application-error-reporting-data.md)
* [デスクトップ アプリケーションのエラーに関する詳細情報の取得](get-details-for-an-error-in-your-desktop-application.md)
* [デスクトップ アプリケーションのエラーに関するスタック トレースの取得](get-the-stack-trace-for-an-error-in-your-desktop-application.md)