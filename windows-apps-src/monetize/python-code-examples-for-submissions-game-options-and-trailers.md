---
description: このセクションの Python コード例を使用して、Microsoft Store 申請 API を使用したゲーム オプションおよびトレーラーの申請方法をご確認ください。
title: 'Python のコード例: ゲーム オプションおよびトレーラーを含むアプリの申請'
ms.date: 07/10/2017
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store 申請 API, コード例, ゲーム オプション, トレーラー, 詳細な登録情報, python
ms.localizationpriority: medium
ms.openlocfilehash: 26a2062ff1d8e0f03ff8507cab89bed942012143
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/02/2020
ms.locfileid: "89364075"
---
# <a name="python-sample-app-submission-with-game-options-and-trailers"></a>Python のコード例: ゲーム オプションおよびトレーラーを含むアプリの申請

この記事では、次のタスクで [Microsoft Store 申請 API](create-and-manage-submissions-using-windows-store-services.md) を使用する方法を示す Python コード例を提供します。

* Microsoft Store 申請 API で使用する Azure AD アクセス トークンを取得します。
* アプリの申請の作成
* [ゲーム](manage-app-submissions.md#gaming-options-object)と[トレーラー](manage-app-submissions.md#trailer-object)の高度な登録情報のオプションを含む、アプリの申請用のストア登録情報データを構成します。
* アプリの申請用のパッケージ、登録情報の画像、トレーラー ファイルが含まれた ZIP ファイルをアップロードします。
* アプリの申請をコミットします。

<span id="create-app-submission" />

## <a name="create-an-app-submission"></a>アプリの申請の作成

このコードでは、他のサンプル クラスと関数を呼び出して、Microsoft Store 申請 API を使ってゲーム オプションとトレーラーを含むアプリの申請を作成し、コミットします。 このコードを採用するには、次の手順を実行してください。

* `tenant` 変数をアプリのテナント ID に割り当てて、`client` 変数と `secret` 変数をアプリのクライアント ID とキーに割り当てます。 詳細については、「 [Azure AD アプリケーションをパートナーセンターアカウントに関連付ける方法](create-and-manage-submissions-using-windows-store-services.md#how-to-associate-an-azure-ad-application-with-your-partner-center-account)」を参照してください。
* `application_id` 変数を、申請を作成するアプリの[ストア ID](in-app-purchases-and-trials.md#store-ids) に割り当てます。

> [!div class="tabbedCodeSnippets"]
:::code language="python" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_SubmissionAdvancedListings/python/CreateAndSubmitAppSubmissionExample.py" range="1-74":::

<span id="token" />

## <a name="obtain-an-azure-ad-access-token-and-invoke-the-submission-api"></a>Azure AD アクセス トークンを取得して申請 API を呼び出す

次の例では、以下に示すクラスを定義します。

* `DevCenterAccessTokenClient` クラスでは、指定された `tenantId`、`clientId`、`clientSecret` の値を使って、Microsoft Store 申請 API で使用する Azure AD アクセス トークンを作成するヘルパー メソッドを定義します。
* `DevCenterClient` クラスでは、Microsoft Store 申請 API のさまざまなメソッドを呼び出して、アプリの申請用のパッケージ、登録情報の画像、トレーラー ファイルを含む ZIP ファイルをアップロードするヘルパー メソッドを定義します。

> [!div class="tabbedCodeSnippets"]
:::code language="python" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_SubmissionAdvancedListings/python/devcenterclient.py" range="1-126":::

<span id="token" />

## <a name="get-app-submission-listing-data"></a>アプリの申請の登録情報データを取得する

次の例では、新しいサンプル アプリの申請用の JSON 形式の登録情報データを返すヘルパー関数を定義します。

> [!div class="tabbedCodeSnippets"]
:::code language="python" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_SubmissionAdvancedListings/python/submissiondatasamples.py" range="1-170":::

## <a name="related-topics"></a>関連トピック

* [Microsoft Store サービスを使用した申請の作成と管理](create-and-manage-submissions-using-windows-store-services.md)
