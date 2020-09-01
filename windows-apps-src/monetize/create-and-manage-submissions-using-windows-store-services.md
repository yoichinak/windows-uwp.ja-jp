---
ms.assetid: 7CC11888-8DC6-4FEE-ACED-9FA476B2125E
description: パートナーセンターアカウントに登録されているアプリの送信をプログラムで作成および管理するには、Microsoft Store 送信 API を使用します。
title: 申請の作成と管理
ms.date: 06/04/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store 申請 API
ms.localizationpriority: medium
ms.openlocfilehash: af0d36f2fa76fe9bb5bd253436f3d434a860e7ec
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155626"
---
# <a name="create-and-manage-submissions"></a>申請の作成と管理

*Microsoft Store 送信 API*を使用して、または組織のパートナーセンターアカウントのアプリ、アドオン、およびパッケージフライトに対してプログラムでクエリを実行し、送信を作成します。 この API は、アカウントで多数のアプリやアドオンを管理していて、それらのアセットの申請プロセスを自動化して最適化する必要がある場合に役立ちます。 この API は、Azure Active Directory (Azure AD) を使って、アプリまたはサービスからの呼び出しを認証します。

この手順では、Microsoft Store 申請 API を使う場合のプロセスについて詳しく説明します。

1.  すべての[前提条件](#prerequisites)を完了したことを確認します。
3.  Microsoft Store 申請 API のメソッドを呼び出す前に、[Azure AD アクセス トークンを取得](#obtain-an-azure-ad-access-token)する必要があります。 トークンを取得した後、Microsoft Store 申請 API の呼び出しでこのトークンを使用できるのは、その有効期限が切れるまでの 60 分間です。 トークンの有効期限が切れた後は、新しいトークンを生成できます。
4.  [Microsoft Store 申請 API を呼び出します](#call-the-windows-store-submission-api)。

<span id="not_supported" />

> [!IMPORTANT]
> この API を使用してアプリ、パッケージフライト、またはアドオンの送信を作成する場合は、パートナーセンターではなく、API を使用してのみ、送信をさらに変更してください。 API を使用して最初に作成した送信をパートナーセンターを使用して変更した場合、API を使用してその送信を変更またはコミットすることはできなくなります。 場合によっては、申請がエラー状態のままになり、申請プロセスを進めることができなくなります。 この場合、申請を削除して新しい申請を作成する必要があります。

> [!IMPORTANT]
> この API を使って、[ビジネス向け Microsoft Store や教育機関向け Microsoft Store でのボリューム購入](../publish/organizational-licensing.md)の申請を公開したり、[LOB アプリ](../publish/distribute-lob-apps-to-enterprises.md)の申請を直接企業に発行したりすることはできません。 どちらのシナリオでも、「パートナーセンターでの送信の発行」を使用する必要があります。

> [!NOTE]
> この API は、アプリの必須更新プログラムや Store で管理されるコンシューマブルなアドオンを使うアプリまたはアドオンでは使用できません。 このような機能のいずれかを使用するアプリまたはアドオンで Microsoft Store 申請 API を使うと、API から 409 エラー コードが返されます。 この場合は、パートナーセンターを使用して、アプリまたはアドオンの送信を管理する必要があります。

<span id="prerequisites" />

## <a name="step-1-complete-prerequisites-for-using-the-microsoft-store-submission-api"></a>手順 1. Microsoft Store 申請 API を使うための前提条件を満たす

Microsoft Store 申請 API を呼び出すコードの作成を開始する前に、次の前提条件が満たされていることを確認します。

* 自分 (または自分の組織) に Azure AD ディレクトリがあり、自分がそのディレクトリに対する[グローバル管理者](/azure/active-directory/users-groups-roles/directory-assign-admin-roles)のアクセス許可を持っている必要があります。 Microsoft 365 または Microsoft の他のビジネス サービスをすでに使用している場合、Azure AD ディレクトリをすでに所有しています。 それ以外の場合は、追加料金なしに[パートナー センターで新しい Azure AD を作成](../publish/associate-azure-ad-with-partner-center.md#create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account)できます。

* [Azure AD アプリケーションをパートナー センター アカウントと関連付け](#associate-an-azure-ad-application-with-your-windows-partner-center-account)、テナント ID、クライアント ID、キーを取得する必要があります。 Azure AD アクセス トークンを取得するにはこれらの値が必要です。Microsoft Store 申請 API への呼び出しにこれらを使用します。

* Microsoft Store 申請 API で使うアプリを準備します。

  * アプリがパートナーセンターにまだ存在しない場合は、 [パートナーセンターでアプリの名前を予約することで、アプリを作成](../publish/create-your-app-by-reserving-a-name.md)する必要があります。 Microsoft Store 送信 API を使用して、パートナーセンターでアプリを作成することはできません。これを作成するには、パートナーセンターで作業する必要があります。その後、API を使用してアプリにアクセスし、プログラムによって送信を作成できます。 ただし、API を使うと、アプリの申請を作成する前に、アドオンとパッケージ フライトをプログラムで作成できます。

  * この API を使用して特定のアプリの送信を作成する前に、まず [パートナーセンターでアプリの送信を1つ作成](../publish/app-submissions.md)する必要があります ( [年齢](../publish/age-ratings.md) 区分のアンケートへの回答を含む)。 この操作の実行後、API を使ってこのアプリの新しい申請をプログラムで作成できるようになります。 アドオンの申請またはパッケージ フライトの申請を作成しなくても、このような申請に API を使うことができます。

  * アプリの申請を作成または更新するときにアプリ パッケージを追加する必要がある場合は、[アプリ パッケージを準備](../publish/app-package-requirements.md)します。

  * アプリの申請を作成または更新するときにストア登録情報用のスクリーンショットまたは画像を含める必要がある場合は、[アプリのスクリーンショットと画像を準備](../publish/app-screenshots-and-images.md)します。

  * アドオンの申請を作成または更新するときにアイコンを含める必要がある場合は、[アイコンを準備](../publish/create-add-on-store-listings.md)します。

<span id="associate-an-azure-ad-application-with-your-windows-partner-center-account" />

### <a name="how-to-associate-an-azure-ad-application-with-your-partner-center-account"></a>Azure AD アプリケーションをパートナー センター アカウントと関連付ける方法

Microsoft Store 送信 API を使用するには、Azure AD アプリケーションをパートナーセンターアカウントに関連付ける必要があります。その後、アプリケーションのテナント ID とクライアント ID を取得し、キーを生成する必要があります。 Azure AD アプリケーションは、Microsoft Store 申請 API の呼び出し元のアプリまたはサービスを表します。 API に渡す Azure AD アクセス トークンを取得するには、テナント ID、クライアント ID、キーが必要です。

> [!NOTE]
> この作業を行うのは一度だけです。 テナント ID、クライアント ID、キーがあれば、新しい Azure AD アクセス トークンを作成する必要がある度にそれらを再利用できます。

1.  パートナー センターで、[組織のパートナー センター アカウントを組織の Azure AD ディレクトリに関連付けます](../publish/associate-azure-ad-with-partner-center.md)。

2.  次に、パートナー センターの **[アカウント設定]** セクションの **[ユーザー]** ページから、パートナー センター アカウントの申請にアクセスするために使用するアプリまたはサービスを表す [Azure AD アプリケーションを追加](../publish/add-users-groups-and-azure-ad-applications.md#add-azure-ad-applications-to-your-partner-center-account)します。 このアプリケーションに**マネージャー** ロールを確実に割り当てます。 アプリケーションがまだ Azure AD ディレクトリに存在しない場合、[パートナー センターで新しい Azure AD アプリケーションを作成](../publish/add-users-groups-and-azure-ad-applications.md#create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account)できます。  

3.  **[ユーザー]** ページに戻り、Azure AD アプリケーションの名前をクリックしてアプリケーション設定に移動し、**テナント ID** と**クライアント ID** の値を書き留めます。

4. **[新しいキーを追加]** をクリックします。 次の画面で、**キー**の値を書き留めます。 このページを離れると、この情報にアクセスすることはできなくなります。 詳細については、「[Azure AD アプリケーションのキーを管理する](../publish/add-users-groups-and-azure-ad-applications.md#manage-keys)」を参照してください。

<span id="obtain-an-azure-ad-access-token" />

## <a name="step-2-obtain-an-azure-ad-access-token"></a>手順 2:Azure AD アクセス トークンを取得する

Microsoft Store 申請 API のいずれかのメソッドを呼び出す前に、まず API の各メソッドの **Authorization** ヘッダーに渡す Azure AD アクセス トークンを取得する必要があります。 アクセス トークンを取得した後、アクセス トークンを使用できるのは、その有効期限が切れるまでの 60 分間です。 トークンの有効期限が切れた後は、トークンを更新してそれ以降の API 呼び出しで引き続き使用できます。

アクセス トークンを取得するには、「[クライアント資格情報を使用したサービス間の呼び出し](/azure/active-directory/azuread-dev/v1-oauth2-client-creds-grant-flow)」の手順に従って、HTTP POST を ```https://login.microsoftonline.com/<tenant_id>/oauth2/token``` エンドポイントに送信します。 要求の例を次に示します。

```json
POST https://login.microsoftonline.com/<tenant_id>/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded; charset=utf-8

grant_type=client_credentials
&client_id=<your_client_id>
&client_secret=<your_client_secret>
&resource=https://manage.devcenter.microsoft.com
```

[POST URI] と [*クライアント \_ id* ] と [*クライアント \_ シークレット*] の [*テナント \_ id* ] の値について、前のセクションでパートナーセンターから取得したアプリケーションのテナント id、クライアント id、キーを指定します。 *resource* パラメーターには、```https://manage.devcenter.microsoft.com``` を指定します。

アクセス トークンの有効期限が切れた後は、[この](/azure/active-directory/azuread-dev/v1-protocols-oauth-code#refreshing-the-access-tokens)手順に従って更新できます。

C#、Java、または Python コードを使ってアクセス トークンを取得する方法を示す例については、Microsoft Store 申請 API の[コード例](#code-examples)をご覧ください。

<span id="call-the-windows-store-submission-api">

## <a name="step-3-use-the-microsoft-store-submission-api"></a>手順 3:Microsoft Store 申請 API を使用する

Azure AD アクセス トークンを取得したら、Microsoft Store 申請 API のメソッドを呼び出すことができます。 この API には、アプリ、アドオン、パッケージ フライトのシナリオにグループ化される多くのメソッドが含まれています。 申請を作成または更新するには、通常、Microsoft Store 申請 API の複数のメソッドを特定の順序で呼び出します。 各シナリオと各メソッドの構文について詳しくは、次の表の記事をご覧ください。

> [!NOTE]
> アクセス トークンを取得した後、Microsoft Store 申請 API のメソッドを呼び出すことができるのは、その有効期限が切れるまでの 60 分間です。

| シナリオ       | 説明                                                                 |
|---------------|----------------------------------------------------------------------|
| [アプリ] |  パートナーセンターアカウントに登録されているすべてのアプリのデータを取得し、アプリの送信を作成します。 これらのメソッドについて詳しくは、次の記事をご覧ください。 <ul><li>[アプリ データの入手](get-app-data.md)</li><li>[アプリの申請の管理](manage-app-submissions.md)</li></ul> |
| アドオン | アプリのアドオンを取得、作成、または削除した後、そのアドオンの申請を取得、作成、または削除します。 これらのメソッドについて詳しくは、次の記事をご覧ください。 <ul><li>[アドオンの管理](manage-add-ons.md)</li><li>[アドオンの申請の管理](manage-add-on-submissions.md)</li></ul> |
| パッケージ フライト | アプリのパッケージ フライトを取得、作成、または削除した後、パッケージ フライトの申請を取得、作成、または削除します。 これらのメソッドについて詳しくは、次の記事をご覧ください。 <ul><li>[パッケージ フライトの管理](manage-flights.md)</li><li>[パッケージ フライトの申請の管理](manage-flight-submissions.md)</li></ul> |

<span id="code-samples"/>

## <a name="code-examples"></a>コード例

次の記事では、さまざまなプログラミング言語で Microsoft Store 申請 API を使用する方法を示す詳しいコード例を紹介します。

* [C# のコード例: アプリ、アドオン、およびフライトの申請](csharp-code-examples-for-the-windows-store-submission-api.md)
* [C# のコード例: ゲーム オプションおよびトレーラーを含むアプリの申請](csharp-code-examples-for-submissions-game-options-and-trailers.md)
* [Java のコード例: アプリ、アドオン、およびフライトの申請](java-code-examples-for-the-windows-store-submission-api.md)
* [Java のコード例: ゲーム オプションおよびトレーラーを含むアプリの申請](java-code-examples-for-submissions-game-options-and-trailers.md)
* [Python のコード例: アプリ、アドオン、およびフライトの申請](python-code-examples-for-the-windows-store-submission-api.md)
* [Python のコード例: ゲーム オプションおよびトレーラーを含むアプリの申請](python-code-examples-for-submissions-game-options-and-trailers.md)

## <a name="storebroker-powershell-module"></a>StoreBroker PowerShell モジュール

Microsoft Store 申請 API を直接呼び出す代わりに、API の上にコマンド ライン インターフェイスを実装するオープンソースの PowerShell モジュールも用意されています。 このモジュールは、[StoreBroker](https://github.com/Microsoft/StoreBroker) と呼ばれています。 このモジュールを使うと、Microsoft Store 申請 API を直接呼び出さずに、コマンド ラインからアプリ、フライト、アドオンの申請を管理できます。また、ソースを参照して、この API を呼び出す方法の例を確認することもできます。 StoreBroker モジュールは、多くのファースト パーティ アプリケーションをストアに申請する主要な方法として Microsoft 内で積極的に使っています。

詳しくは、[GitHub の StoreBroker に関するページ](https://github.com/Microsoft/StoreBroker)をご覧ください。

## <a name="troubleshooting"></a>トラブルシューティング

| 問題      | 解決方法                                          |
|---------------|---------------------------------------------|
| PowerShell から Microsoft Store 申請 API を呼び出した後、[ConvertFrom-Json](/powershell/module/5.1/microsoft.powershell.utility/ConvertFrom-Json) コマンドレットを使って API の応答データを JSON 形式から PowerShell オブジェクトに変換し、[ConvertTo-Json](/powershell/module/5.1/microsoft.powershell.utility/ConvertTo-Json) コマンドレットを使ってもう一度 JSON 形式に変換すると、応答データが破損します。 |  既定では、[ConvertTo-Json](/powershell/module/5.1/microsoft.powershell.utility/ConvertTo-Json) コマンドレットの *-Depth* パラメーターは、2 レベルのオブジェクトに設定されます。これは、Microsoft Store 申請 API から返される JSON オブジェクトの大半にとって浅すぎます。 [ConvertTo-Json](/powershell/module/5.1/microsoft.powershell.utility/ConvertTo-Json) コマンドレットを呼び出すときは、*-Depth* パラメーターを大きい値 (たとえば 20) に設定します。 |

## <a name="additional-help"></a>追加のヘルプ

Microsoft Store 申請 API について質問がある場合や、この API を使った申請の管理に関してサポートが必要な場合は、次のリソースを使ってください。

* Microsoft の[フォーラム](https://social.msdn.microsoft.com/Forums/windowsapps/home?forum=wpsubmit)で質問します。
* [サポートページ](https://developer.microsoft.com/windows/support)にアクセスし、パートナーセンターのサポートオプションの1つをお問い合わせください。 問題の種類とカテゴリを選択するよう求められた場合は、**[App submission and certification]** (アプリの申請と認定) と **[Submitting an app]** (アプリの申請) をそれぞれ選択します。  

## <a name="related-topics"></a>関連トピック

* [アプリ データの入手](get-app-data.md)
* [アプリの申請の管理](manage-app-submissions.md)
* [アドオンの管理](manage-add-ons.md)
* [アドオンの申請の管理](manage-add-on-submissions.md)
* [パッケージ フライトの管理](manage-flights.md)
* [パッケージ フライトの申請の管理](manage-flight-submissions.md)