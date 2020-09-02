---
ms.assetid: 8e6c3d3d-0120-40f4-9f90-0b0518188a1a
description: Microsoft Store 昇格 API を使用して、または組織のパートナーセンターアカウントに登録されているアプリのプロモーション広告キャンペーンをプログラムで管理します。
title: ストア サービスを使用した広告キャンペーンの実行
ms.date: 06/04/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store プロモーション API, 広告キャンペーン
ms.localizationpriority: medium
ms.openlocfilehash: 74afbda1cc93aa0602618d6d94efe6baadf59ecb
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363705"
---
# <a name="run-ad-campaigns-using-store-services"></a>ストア サービスを使用した広告キャンペーンの実行

*Microsoft Store 昇格 API*を使用して、または組織のパートナーセンターアカウントに登録されているアプリのプロモーション広告キャンペーンをプログラムで管理します。 この API を使用して、広告キャンペーンや、ターゲット設定、クリエイティブなど、その他の関連アセットを作成、更新、および監視できます。 この API は、大量のキャンペーンを作成する開発者や、パートナーセンターを使用せずに実行する開発者に特に役立ちます。 この API は、Azure Active Directory (Azure AD) を使って、アプリまたはサービスからの呼び出しを認証します。

次の手順で、このプロセスについて詳しく説明しています。

1.  すべての[前提条件](#prerequisites)を完了したことを確認します。
2.  Microsoft Store プロモーション API のメソッドを呼び出す前に、[Azure AD アクセス トークンを取得](#obtain-an-azure-ad-access-token)する必要があります。 トークンを取得した後、Microsoft Store プロモーション API の呼び出しでこのトークンを使用できるのは、その有効期限が切れるまでの 60 分間です。 トークンの有効期限が切れた後は、新しいトークンを生成できます。
3.  [Microsoft Store プロモーション API を呼び出します](#call-the-windows-store-promotions-api)。

また、パートナーセンターを使用して ad キャンペーンを作成して管理することもできます。また、Microsoft Store 昇格 API を使用してプログラムで作成した ad キャンペーンは、パートナーセンターでもアクセスできます。 パートナーセンターで ad キャンペーンを管理する方法の詳細については、「 [アプリの ad キャンペーンを作成](../publish/create-an-ad-campaign-for-your-app.md)する」を参照してください。

> [!NOTE]
> パートナーセンターアカウントを持つ開発者は、Microsoft Store 昇格 API を使用して、アプリの ad キャンペーンを管理できます。 メディア エージェンシーもこの API へのアクセスを要求して、広告主の代わりに広告キャンペーンを実行できます。 お客様がメディア エージェンシーで、この API について詳しい情報を希望される場合、またはこの API へのアクセスを要求される場合は、storepromotionsapi@microsoft.com までリクエストをお送りください。

<span id="prerequisites" />

## <a name="step-1-complete-prerequisites-for-using-the-microsoft-store-promotions-api"></a>手順 1: Microsoft Store プロモーション API を使うための前提条件を満たす

Microsoft Store プロモーション API を呼び出すコードの作成を開始する前に、次の前提条件が満たされていることを確認します。

* この API を使用して ad キャンペーンを正常に作成して開始するには、まず、 [パートナーセンターの [ **ad キャンペーン** ] ページを使用して1つの有料広告キャンペーンを作成](../publish/create-an-ad-campaign-for-your-app.md)し、このページで少なくとも1つの支払い方法を追加する必要があります。 これを行うと、この API を使用して、広告キャンペーンの請求可能な配信ラインを正しく作成することができます。 この API を使用して作成した広告キャンペーンの配信行は、パートナーセンターの [ **ad キャンペーン** ] ページで選択した既定の支払い方法に自動的に課金されます。

* 自分 (または自分の組織) に Azure AD ディレクトリがあり、自分がそのディレクトリに対する[グローバル管理者](/azure/active-directory/users-groups-roles/directory-assign-admin-roles)のアクセス許可を持っている必要があります。 Microsoft 365 または Microsoft の他のビジネス サービスをすでに使用している場合、Azure AD ディレクトリをすでに所有しています。 それ以外の場合は、追加料金なしに[パートナー センターで新しい Azure AD を作成](../publish/associate-azure-ad-with-partner-center.md#create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account)できます。

* Azure AD アプリケーションをパートナーセンターアカウントに関連付けて、アプリケーションのテナント ID とクライアント ID を取得し、キーを生成する必要があります。 Azure AD アプリケーションは、Microsoft Store プロモーション API の呼び出し元のアプリまたはサービスを表します。 API に渡す Azure AD アクセス トークンを取得するには、テナント ID、クライアント ID、キーが必要です。
    > [!NOTE]
    > この作業を行うのは一度だけです。 テナント ID、クライアント ID、キーがあれば、新しい Azure AD アクセス トークンを作成する必要がある度にそれらを再利用できます。

Azure AD アプリケーションをパートナーセンターアカウントに関連付け、必要な値を取得するには、次のようにします。

1.  パートナー センターで、[組織のパートナー センター アカウントを組織の Azure AD ディレクトリに関連付けます](../publish/associate-azure-ad-with-partner-center.md)。

2.  次に、パートナーセンターの [**アカウントの設定**] セクションの [**ユーザー** ] ページで、パートナーセンターアカウントのプロモーションキャンペーンを管理するために使用するアプリまたはサービスを表す[Azure AD アプリケーションを追加](../publish/add-users-groups-and-azure-ad-applications.md#add-azure-ad-applications-to-your-partner-center-account)します。 このアプリケーションに**マネージャー** ロールを確実に割り当てます。 アプリケーションがまだ Azure AD ディレクトリに存在しない場合、[パートナー センターで新しい Azure AD アプリケーションを作成](../publish/add-users-groups-and-azure-ad-applications.md#create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account)できます。 

3.  **[ユーザー]** ページに戻り、Azure AD アプリケーションの名前をクリックしてアプリケーション設定に移動し、**テナント ID** と**クライアント ID** の値を書き留めます。

4. **[新しいキーを追加]** をクリックします。 次の画面で、**キー**の値を書き留めます。 このページを離れると、この情報にアクセスすることはできなくなります。 詳細については、「[Azure AD アプリケーションのキーを管理する](../publish/add-users-groups-and-azure-ad-applications.md#manage-keys)」を参照してください。

<span id="obtain-an-azure-ad-access-token" />

## <a name="step-2-obtain-an-azure-ad-access-token"></a>手順 2:Azure AD アクセス トークンを取得する

Microsoft Store プロモーション API のいずれかのメソッドを呼び出す前に、まず API の各メソッドの **Authorization** ヘッダーに渡す Azure AD アクセス トークンを取得する必要があります。 アクセス トークンを取得した後、アクセス トークンを使用できるのは、その有効期限が切れるまでの 60 分間です。 トークンの有効期限が切れた後は、トークンを更新してそれ以降の API 呼び出しで引き続き使用できます。

アクセス トークンを取得するには、「[クライアント資格情報を使用したサービス間の呼び出し](/azure/active-directory/azuread-dev/v1-oauth2-client-creds-grant-flow)」の手順に従って、HTTP POST を ```https://login.microsoftonline.com/<tenant_id>/oauth2/token``` エンドポイントに送信します。 要求の例を次に示します。

```syntax
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

<span id="call-the-windows-store-promotions-api" />

## <a name="step-3-call-the-microsoft-store-promotions-api"></a>手順 3: Microsoft Store プロモーション API を呼び出す

Azure AD アクセス トークンを取得したら、Microsoft Store プロモーション API を呼び出すことができます。 各メソッドの **Authorization** ヘッダーにアクセス トークンを渡す必要があります。

Microsoft Store プロモーション API では、キャンペーンについての概要情報を保持する*キャンペーン* オブジェクトと、広告キャンペーンの*配信ライン*、*ターゲット プロファイル*、および*クリエイティブ*を表すその他のオブジェクトで構成されるものを広告キャンペーンとします。 この API には、これらのオブジェクトの種類ごとにグループ化されたメソッドのセットが含まれます。 キャンペーンを作成するには、通常、これらのオブジェクトごとに別の POST メソッドを呼び出します。 この API には、任意のオブジェクトの取得に使用できる GET メソッドと、キャンペーン、配信ライン、およびターゲット プロファイル オブジェクトの編集に使用できる PUT メソッドも用意されています。

これらオブジェクトと関連するメソッドについての詳細は、以下の表を参照してください。


| Object       | 説明   |
|---------------|-----------------|
| キャンペーン |  このオブジェクトは広告キャンペーンを表し、広告キャンペーンのオブジェクト モデル階層の最上位に置かれます。 このオブジェクトは、実行するキャンペーンの種類 (有料、自社、またはコミュニティ)、キャンペーン目標、広告キャンペーンの配信ライン、その他の詳細情報を示します。 1 つのキャンペーンに関連づけることができるのは、1 つのアプリのみです。<br/><br/>このオブジェクトについて詳しくは、「[広告キャンペーンの管理](manage-ad-campaigns.md)」をご覧ください。<br/><br/>**Note** &nbsp; メモ &nbsp;Ad キャンペーンを作成した後、 [Microsoft Store ANALYTICS API](access-analytics-data-using-windows-store-services.md)の [ [ad キャンペーンのパフォーマンスデータを取得](get-ad-campaign-performance-data.md)] メソッドを使用して、キャンペーンのパフォーマンスデータを取得できます。  |
| 配信ライン | キャンペーンごとに、インベントリの購入と広告の配信に使用する配信ラインが 1 つ以上あります。 配信ラインごとに、ターゲットと入札額を設定できます。また、予算と使用したいクリエイティブへのリンクを設定することで、支払い額を決定できます。<br/><br/>このオブジェクトについて詳しくは、「[広告キャンペーンの配信ラインの管理](manage-delivery-lines-for-ad-campaigns.md)」をご覧ください。 |
| ターゲット プロファイル | 配信ラインごとに、1 つのターゲット プロファイルを用意します。ターゲット プロファイルでは、対象にするユーザー、地域、およびインベントリの種類を指定します。 ターゲット プロファイルは、テンプレートとして作成し、複数の配信ライン間で共有できます。<br/><br/>このオブジェクトについて詳しくは、「[広告キャンペーンのターゲット プロファイルの管理](manage-targeting-profiles-for-ad-campaigns.md)」をご覧ください。 |
| クリエイティブ | 配信ラインごとに、キャンペーンの一環でお客様に表示される広告を表すクリエイティブが 1 つ以上あります。 クリエイティブは、常に同じアプリを表す場合は、同一の広告キャンペーンでなくても、1 つ以上の配信ラインに関連付けることができます。<br/><br/>このオブジェクトについて詳しくは、「[広告キャンペーンのクリエイティブの管理](manage-creatives-for-ad-campaigns.md)」をご覧ください。 |


次の図は、キャンペーン、配信ライン、ターゲット プロファイル、クリエイティブ間の関係を表しています。

![広告キャンペーンの階層](images/ad-campaign-hierarchy.png)

## <a name="code-example"></a>コードの例

次のコード例は、Azure AD アクセス トークンを取得し、C# コンソール アプリから Microsoft Store プロモーション API を呼び出す方法を示しています。 このコード例を使う場合は、変数 *tenantId*、*clientId*、*clientSecret*、および *appID* を自分のシナリオに合った適切な値に割り当ててください。 この例では、Microsoft Store プロモーション API から返される JSON データを逆シリアル化するときに、Newtonsoft の [Json.NET パッケージ](https://www.newtonsoft.com/json)が必要になります。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Promotions/cs/Program.cs" id="PromotionsApiExample":::

## <a name="related-topics"></a>関連トピック

* [広告キャンペーンの管理](manage-ad-campaigns.md)
* [広告キャンペーンの配信ラインの管理](manage-delivery-lines-for-ad-campaigns.md)
* [広告キャンペーンの対象プロファイルの管理](manage-targeting-profiles-for-ad-campaigns.md)
* [広告キャンペーンのクリエイティブの管理](manage-creatives-for-ad-campaigns.md)
* [広告キャンペーンのパフォーマンス データの取得](get-ad-campaign-performance-data.md)


 
