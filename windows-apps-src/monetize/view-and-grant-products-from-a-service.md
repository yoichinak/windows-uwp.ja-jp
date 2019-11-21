---
ms.assetid: B071F6BC-49D3-4E74-98EA-0461A1A55EFB
description: アプリとアドオンのカタログがある場合は、Microsoft Store コレクション API および Microsoft Store 購入 API を使って、サービスからこれらの製品の所有権情報にアクセスできます。
title: サービスから製品の権利を管理する
ms.date: 08/01/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store コレクション API, Microsoft Store 購入 API, 製品の表示, 製品の付与
ms.localizationpriority: medium
ms.openlocfilehash: 2d0df7780943717d8e01f0efcf1583efe05608af
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259217"
---
# <a name="manage-product-entitlements-from-a-service"></a>サービスから製品の権利を管理する

アプリとアドオンのカタログがある場合は、*Microsoft Store コレクション API* と *Microsoft Store 購入 API* を使って、サービスからこれらの製品の権利の情報にアクセスできます。 "権利" とは、Microsoft Store を通じて公開されたアプリまたはアドオンを顧客が使用する権利を表します。

これらの API は、クロスプラットフォーム サービスでサポートされるアドオン カタログを持つ開発者向けに設計された REST メソッドで構成されています。 これらの API を使用すると、次の操作を実行できます。

-   Microsoft Store コレクション API: [ユーザーが所有するアプリを照会](query-for-products.md)し、[コンシューマブルな製品をフルフィルメント完了として報告](report-consumable-products-as-fulfilled.md)する。
-   Microsoft Store 購入 API: [無料のアプリをユーザーに付与する](grant-free-products.md)、[ユーザーのサブスクリプションを取得する](get-subscriptions-for-a-user.md)、[ユーザーのサブスクリプションに関する請求の状態を変更する](change-the-billing-state-of-a-subscription-for-a-user.md)。

> [!NOTE]
> Microsoft Store コレクション API と Microsoft Store 購入 API では、Azure Active Directory (Azure AD) 認証を使って顧客の所有権情報にアクセスします。 これらの API を使用するには、ユーザー (またはユーザーの組織) が、Azure AD ディレクトリと、そのディレクトリに対する[全体管理者](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles)のアクセス許可を持っている必要があります。 Office 365 または Microsoft の他のビジネス サービスを既に使っている場合は、既に Azure AD ディレクトリをお持ちです。

## <a name="overview"></a>概要

次の手順は、Microsoft Store コレクション API および購入 API を使用するプロセス全体を表したものです。

1.  [Configure an application in Azure AD](#step-1).
2.  [Associate your Azure AD application ID with your app in Partner Center](#step-2).
3.  サービスで、発行元 ID を表す [Azure AD アクセス トークンを作成します](#step-3)。
4.  In your client Windows app, [create a Microsoft Store ID key](#step-4) that represents the identity of the current user, and pass this key back to your service.
5.  必要な Azure AD のアクセス トークンと Microsoft Store ID キーを取得した後、[サービスから Microsoft Store コレクション API または Microsoft Store 購入 API を呼び出します](#step-5)。

This end-to-end process involves two software components that perform different tasks:

* **Your service**. This is an application that runs securely in the context of your business environment, and it can be implemented using any development platform you choose. Your service is responsible for creating the Azure AD access tokens needed for the scenario and for calling the REST URIs for the Microsoft Store collection API and purchase API.
* **Your client Windows app**. This is the app for which you want to access and manage customer entitlement information (including add-ons for the app). This app is responsible for creating the Microsoft Store ID keys you need to call the Microsoft Store collection API and purchase API from your service.

<span id="step-1"/>

## <a name="step-1-configure-an-application-in-azure-ad"></a>Step 1: Configure an application in Azure AD

Before you can use the Microsoft Store collection API or purchase API, you must create an Azure AD Web application, retrieve the tenant ID and application ID for the application, and generate a key. The Azure AD Web application represents the service from which you want to call the Microsoft Store collection API or purchase API. You need the tenant ID, application ID and key to generate Azure AD access tokens that you need to call the API.

> [!NOTE]
> このセクションの作業は 1 回実行する必要があるだけです。 After you update your Azure AD application manifest and you have your tenant ID, application ID and client secret, you can reuse these values any time you need to create a new Azure AD access token.

1.  If you haven't done so already, follow the instructions in [Integrating Applications with Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications) to register a **Web app / API** application with Azure AD.
    > [!NOTE]
    > When you register your application, you must choose **Web app / API** as the application type so that you can retrieve a key (also called a *client secret*) for your application. Microsoft Store コレクション API または購入 API を呼び出すには、後の手順で Azure AD からアクセス トークンを要求するときにクライアント シークレットを指定する必要があります。

2.  In the [Azure Management Portal](https://portal.azure.com/), navigate to **Azure Active Directory**. Select your directory, click **App registrations** in the left navigation pane, and then select your application.
3.  You are taken to the application's main registration page. On this page, copy the **Application ID** value for use later.
4.  Create a key that you will need later (this is all called a *client secret*). In the left pane, click **Settings** and then **Keys**. On this page, complete the steps to [create a key](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#to-add-application-credentials-or-permissions-to-access-web-apis). Copy this key for later use.
5.  Add several required audience URIs to your [application manifest](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-manifest). In the left pane, click **Manifest**. Click **Edit**, replace the `"identifierUris"` section with the following text, and then click **Save**.

    ```json
    "identifierUris" : [                                
            "https://onestore.microsoft.com",
            "https://onestore.microsoft.com/b2b/keys/create/collections",
            "https://onestore.microsoft.com/b2b/keys/create/purchase"
        ],
    ```

    これらの文字列は、アプリケーションでサポートされる対象ユーザーを表します。 後の手順で、各対象ユーザー値に関連付けられた Azure AD アクセス トークンを作成します。

<span id="step-2"/>

## <a name="step-2-associate-your-azure-ad-application-id-with-your-client-app-in-partner-center"></a>Step 2: Associate your Azure AD application ID with your client app in Partner Center

Before you can use the Microsoft Store collection API or purchase API to configure the ownership and purchases for your app or add-on, you must associate your Azure AD application ID with the app (or the app that contains the add-on) in Partner Center.

> [!NOTE]
> この作業を行うのは一度だけです。

1.  Sign in to [Partner Center](https://partner.microsoft.com/dashboard) and select your app.
2.  Go to the **Services** &gt; **Product collections and purchases** page and enter your Azure AD application ID into one of the available **Client ID** fields.

<span id="step-3"/>

## <a name="step-3-create-azure-ad-access-tokens"></a>手順 3: Azure AD アクセス トークンを作成する

Microsoft Store ID キーを取得したり、Microsoft Store コレクション API または Microsoft Store 購入 API を呼び出したりする前に、発行元 ID を表すいくつかの Azure AD アクセス トークンをサービスで作成する必要があります。 各トークンは別々の API で使われます。 各トークンの有効期間は 60 分であり、有効期限が切れた場合は更新できます。

> [!IMPORTANT]
> Azure AD アクセス トークンは、アプリ内ではなく、サービスのコンテキスト内でのみ作成してください。 このアクセス トークンがアプリに送信されると、クライアント シークレットが侵害される可能性があります。

<span id="access-tokens" />

### <a name="understanding-the-different-tokens-and-audience-uris"></a>さまざまなトークンとオーディエンス URI を理解する

Microsoft Store コレクション API または購入 API で呼び出そうとしているメソッドに応じて、2 つまたは 3 つの異なるトークンを作成する必要があります。 各アクセス トークンは、別々のオーディエンス URI に関連付けられます (これらは、以前に Azure AD アプリケーション マニフェストの `"identifierUris"` セクションに追加した URI と同じです)。

  * いずれの場合も、`https://onestore.microsoft.com` オーディエンス URI のトークンを 1 つ作成する必要があります。 このトークンは、後の手順で Microsoft Store コレクション API または購入 API のメソッドの **Authorization** ヘッダーに渡します。
      > [!IMPORTANT]
      > `https://onestore.microsoft.com` 対象ユーザーには、サービス内に安全に格納されたアクセス トークンのみを使用してください。 このオーディエンスのアクセス トークンがサービス外に公開されると、サービスがリプレイ攻撃に対して脆弱になる可能性があります。

  * Microsoft Store コレクション API のメソッドを呼び出して、[特定のユーザーが所有する製品を照会](query-for-products.md)したり、[コンシューマブルな製品をフルフィルメント完了として報告](report-consumable-products-as-fulfilled.md)したりする場合は、`https://onestore.microsoft.com/b2b/keys/create/collections` オーディエンス URI のトークンも作成する必要があります。 後の手順で、このトークンを Windows SDK のクライアント メソッドに渡し、Microsoft Store コレクション API で使用できる Microsoft Store ID キーを要求します。

  * Microsoft Store 購入 API でメソッドを呼び出して[無料の製品をユーザーに付与する](grant-free-products.md)、[ユーザーのサブスクリプションを取得する](get-subscriptions-for-a-user.md)、または[ユーザーのサブスクリプションに関する請求の状態を変更する](change-the-billing-state-of-a-subscription-for-a-user.md)場合、`https://onestore.microsoft.com/b2b/keys/create/purchase` 対象ユーザー URI を使ってトークンも作成する必要があります。 後の手順で、このトークンを Windows SDK のクライアント メソッドに渡し、Microsoft Store 購入 API で使用できる Microsoft Store ID キーを要求します。

<span />

### <a name="create-the-tokens"></a>トークンの作成

アクセス トークンを作成するには、「[クライアント資格情報を使用したサービス間の呼び出し](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-service-to-service/)」の手順に従って OAuth 2.0 API をサービスで使用し、HTTP POST を ```https://login.microsoftonline.com/<tenant_id>/oauth2/token``` エンドポイントに送信します。 要求の例を次に示します。

``` syntax
POST https://login.microsoftonline.com/<tenant_id>/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded; charset=utf-8

grant_type=client_credentials
&client_id=<your_client_id>
&client_secret=<your_client_secret>
&resource=https://onestore.microsoft.com
```

各トークンについて、次のパラメーター データを指定します。

* For the *client\_id* and *client\_secret* parameters, specify the application ID and the client secret for your application that you retrieved from the [Azure Management Portal](https://portal.azure.com/). これらのパラメーターはいずれも、Microsoft Store コレクション API または購入 API で必要とされる認証のレベルに基づいてアクセス トークンを生成するために必要です。

* *resource* パラメーターには、作成するアクセス トークンの種類に応じて、[前のセクション](#access-tokens)に記載したいずれかのオーディエンス URI を指定します。

アクセス トークンの有効期限が切れた後は、[この](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-code/#refreshing-the-access-tokens)手順に従って更新できます。 アクセス トークンの構造について詳しくは、「[サポートされているトークンと要求の種類](https://docs.microsoft.com/azure/active-directory/develop/id-tokens)」をご覧ください。

<span id="step-4"/>

## <a name="step-4-create-a-microsoft-store-id-key"></a>手順 4: Microsoft Store ID キーを生成する

Microsoft Store コレクション API または Microsoft Store 購入 API のメソッドを呼び出すには、事前にアプリで Microsoft Store ID キーを作成し、サービスに送信する必要があります。 このキーは、アクセス対象の製品所有権情報を保持するユーザーの ID を表す JSON Web トークン (JWT) です。 このキーの要求について詳しくは、「[Microsoft Store ID キー内の要求](#claims-in-a-microsoft-store-id-key)」をご覧ください。

現時点では、Microsoft Store ID キーを作成する唯一の方法は、アプリ内のコードからユニバーサル Windows プラットフォーム (UWP) API を呼び出すことです。 生成されたキーは、デバイスで現在 Microsoft Store にサインインしているユーザーの ID を表します。

> [!NOTE]
> 各 Microsoft Store ID キーは 90 日間有効です。 キーの有効期限が切れた場合は、[キーを更新](renew-a-windows-store-id-key.md)できます。 新しい Microsoft Store ID キーを作成するのではなく、更新することをお勧めします。

<span />

### <a name="to-create-a-microsoft-store-id-key-for-the-microsoft-store-collection-api"></a>Microsoft Store コレクション API 用の Microsoft Store ID キーを作成するには

[特定のユーザーが所有する製品を照会](query-for-products.md)したり、[コンシューマブルな製品をフルフィルメント完了として報告](report-consumable-products-as-fulfilled.md)したりするために、Microsoft Store コレクション API で使用できる Microsoft Store ID キーを作成するには、次の手順に従います。

1.  オーディエンス URI 値 `https://onestore.microsoft.com/b2b/keys/create/collections` を持つ Azure AD アクセス トークンを、サービスからクライアント アプリに渡します。 これは、[前述の手順 3](#step-3) で作成したトークンのいずれかです。

2.  アプリ コードで次のいずれかのメソッドを呼び出して、Microsoft Store ID キーを取得します。

  * アプリで [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store) 名前空間の [StoreContext](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreContext) クラスを使ってアプリ内購入を管理する場合は、[StoreContext.GetCustomerCollectionsIdAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getcustomercollectionsidasync) メソッドを使用します。

  * アプリで [Windows.ApplicationModel.Store](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store) 名前空間の  [CurrentApp](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp) クラスを使ってアプリ内購入を管理する場合は、[CurrentApp.GetCustomerCollectionsIdAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.getcustomercollectionsidasync) メソッドを使用します。

    メソッドの *serviceTicket* パラメーターに、Azure AD アクセス トークンを渡します。 If you maintain anonymous user IDs in the context of services that you manage as the publisher of the current app, you can also pass a user ID to the *publisherUserId* parameter to associate the current user with the new Microsoft Store ID key (the user ID will be embedded in the key). Otherwise, if you don't need to associate a user ID with the Microsoft Store ID key, you can pass any string value to the *publisherUserId* parameter.

3.  アプリで正しく Microsoft Store ID キーを作成したら、そのキーをサービスに渡します。

<span />

### <a name="to-create-a-microsoft-store-id-key-for-the-microsoft-store-purchase-api"></a>Microsoft Store 購入 API 用の Microsoft Store ID キーを作成するには

[無料の製品をユーザーに付与する](grant-free-products.md)、[ユーザーのサブスクリプションを取得する](get-subscriptions-for-a-user.md)、または[ユーザーのサブスクリプションの請求状態を変更する](change-the-billing-state-of-a-subscription-for-a-user.md)ために、Microsoft Store 購入 API と共に使うことができる Microsoft Store ID キーを作成するには、以下の手順に従います。

1.  オーディエンス URI 値 `https://onestore.microsoft.com/b2b/keys/create/purchase` を持つ Azure AD アクセス トークンを、サービスからクライアント アプリに渡します。 これは、[前述の手順 3](#step-3) で作成したトークンのいずれかです。

2.  アプリ コードで次のいずれかのメソッドを呼び出して、Microsoft Store ID キーを取得します。

  * アプリで [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store) 名前空間の [StoreContext](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreContext) クラスを使ってアプリ内購入を管理する場合は、[StoreContext.GetCustomerPurchaseIdAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getcustomerpurchaseidasync) メソッドを使用します。

  * アプリで [Windows.ApplicationModel.Store](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store) 名前空間の [CurrentApp](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp) クラスを使ってアプリ内購入を管理する場合は、[CurrentApp.GetCustomerPurchaseIdAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.getcustomerpurchaseidasync) メソッドを使用します。

    メソッドの *serviceTicket* パラメーターに、Azure AD アクセス トークンを渡します。 If you maintain anonymous user IDs in the context of services that you manage as the publisher of the current app, you can also pass a user ID to the *publisherUserId* parameter to associate the current user with the new Microsoft Store ID key (the user ID will be embedded in the key). Otherwise, if you don't need to associate a user ID with the Microsoft Store ID key, you can pass any string value to the *publisherUserId* parameter.

3.  アプリで正しく Microsoft Store ID キーを作成したら、そのキーをサービスに渡します。

### <a name="diagram"></a>図

The following diagram illustrates the process of creating a Microsoft Store ID key.

  ![Create Windows Store ID key](images/b2b-1.png)

<span id="step-5"/>

## <a name="step-5-call-the-microsoft-store-collection-api-or-purchase-api-from-your-service"></a>手順 5: サービスから Microsoft Store コレクション API または購入 API を呼び出す

特定のユーザーの製品所有権情報にアクセスするための Microsoft Store ID キーをサービスで取得したら、次の手順に従って、サービスから Microsoft Store コレクション API または購入 API を呼び出すことができます。

* [Query for products](query-for-products.md)
* [Report consumable products as fulfilled](report-consumable-products-as-fulfilled.md)
* [Grant free products](grant-free-products.md)
* [Get subscriptions for a user](get-subscriptions-for-a-user.md)
* [Change the billing state of a subscription for a user](change-the-billing-state-of-a-subscription-for-a-user.md)

各シナリオについて、次の情報を API に渡します。

-   要求ヘッダーで、対象ユーザー URI 値 `https://onestore.microsoft.com` を持つ Azure AD アクセス トークンを渡します。 これは、[前述の手順 3](#step-3) で作成したトークンのいずれかです。 このトークンは発行元 ID を表します。
-   要求本文で、[前述の手順 4](#step-4) でアプリのクライアント側コードから取得した Microsoft Store ID キーを渡します。 このキーは、ユーザーの製品所有権情報にアクセスする場合にそのユーザーの ID を表します。

### <a name="diagram"></a>図

The following diagram describes the process of calling a method in the Microsoft Store collection API or purchase API from your service.

  ![Call collections or puchase API](images/b2b-2.png)

## <a name="claims-in-a-microsoft-store-id-key"></a>Microsoft Store ID キー内の要求

Microsoft Store ID キーは、ユーザーの製品所有権情報にアクセスする場合にそのユーザーの ID を表す JSON Web トークン (JWT) です。 Base64 を使用してデコードされた Microsoft Store ID キーには、次の要求が含まれています。

* `iat`:&nbsp;&nbsp;&nbsp;Identifies the time at which the key was issued. この要求は、トークンの経過期間を判別するために使用できます。 この値はエポック時間で表されます。
* `iss`:&nbsp;&nbsp;&nbsp;Identifies the issuer. これは `aud` 要求と同じ値を持ちます。
* `aud`:&nbsp;&nbsp;&nbsp;Identifies the audience. `https://collections.mp.microsoft.com/v6.0/keys` または `https://purchase.mp.microsoft.com/v6.0/keys` のいずれかの値である必要があります。
* `exp`:&nbsp;&nbsp;&nbsp;Identifies the expiration time on or after which the key will no longer be accepted for processing anything except for renewing keys. この要求の値はエポック時間で表されます。
* `nbf`:&nbsp;&nbsp;&nbsp;Identifies the time at which the token will be accepted for processing. この要求の値はエポック時間で表されます。
* `http://schemas.microsoft.com/marketplace/2015/08/claims/key/clientId`:&nbsp;&nbsp;&nbsp;The client ID that identifies the developer.
* `http://schemas.microsoft.com/marketplace/2015/08/claims/key/payload`:&nbsp;&nbsp;&nbsp;An opaque payload (encrypted and Base64 encoded) that contains information that is intended only for use by Microsoft Store services.
* `http://schemas.microsoft.com/marketplace/2015/08/claims/key/userId`:&nbsp;&nbsp;&nbsp;A user ID that identifies the current user in the context of your services. これは、[キーの作成に使うメソッド](#step-4)の省略可能な *publisherUserId* パラメーターに渡す値と同じです。
* `http://schemas.microsoft.com/marketplace/2015/08/claims/key/refreshUri`:&nbsp;&nbsp;&nbsp;The URI that you can use to renew the key.

デコードされた Microsoft Store ID キー ヘッダーの例を次に示します。

```json
{
    "typ":"JWT",
    "alg":"RS256",
    "x5t":"agA_pgJ7Twx_Ex2_rEeQ2o5fZ5g"
}
```

デコードされた Microsoft Store ID キー要求セットの例を次に示します。

```json
{
    "http://schemas.microsoft.com/marketplace/2015/08/claims/key/clientId": "1d5773695a3b44928227393bfef1e13d",
    "http://schemas.microsoft.com/marketplace/2015/08/claims/key/payload": "ZdcOq0/N2rjytCRzCHSqnfczv3f0343wfSydx7hghfu0snWzMqyoAGy5DSJ5rMSsKoQFAccs1iNlwlGrX+/eIwh/VlUhLrncyP8c18mNAzAGK+lTAd2oiMQWRRAZxPwGrJrwiq2fTq5NOVDnQS9Za6/GdRjeiQrv6c0x+WNKxSQ7LV/uH1x+IEhYVtDu53GiXIwekltwaV6EkQGphYy7tbNsW2GqxgcoLLMUVOsQjI+FYBA3MdQpalV/aFN4UrJDkMWJBnmz3vrxBNGEApLWTS4Bd3cMswXsV9m+VhOEfnv+6PrL2jq8OZFoF3FUUpY8Fet2DfFr6xjZs3CBS1095J2yyNFWKBZxAXXNjn+zkvqqiVRjjkjNajhuaNKJk4MGHfk2rZiMy/aosyaEpCyncdisHVSx/S4JwIuxTnfnlY24vS0OXy7mFiZjjB8qL03cLsBXM4utCyXSIggb90GAx0+EFlVoJD7+ZKlm1M90xO/QSMDlrzFyuqcXXDBOnt7rPynPTrOZLVF+ODI5HhWEqArkVnc5MYnrZD06YEwClmTDkHQcxCvU+XUEvTbEk69qR2sfnuXV4cJRRWseUTfYoGyuxkQ2eWAAI1BXGxYECIaAnWF0W6ThweL5ZZDdadW9Ug5U3fZd4WxiDlB/EZ3aTy8kYXTW4Uo0adTkCmdLibw=",
    "http://schemas.microsoft.com/marketplace/2015/08/claims/key/userId": "infusQMLaYCrgtC0d/SZWoPB4FqLEwHXgZFuMJ6TuTY=",
    "http://schemas.microsoft.com/marketplace/2015/08/claims/key/refreshUri": "https://collections.mp.microsoft.com/v6.0/b2b/keys/renew",
    "iat": 1442395542,
    "iss": "https://collections.mp.microsoft.com/v6.0/keys",
    "aud": "https://collections.mp.microsoft.com/v6.0/keys",
    "exp": 1450171541,
    "nbf": 1442391941
}
```

## <a name="related-topics"></a>関連トピック

* [Query for products](query-for-products.md)
* [Report consumable products as fulfilled](report-consumable-products-as-fulfilled.md)
* [Grant free products](grant-free-products.md)
* [Get subscriptions for a user](get-subscriptions-for-a-user.md)
* [Change the billing state of a subscription for a user](change-the-billing-state-of-a-subscription-for-a-user.md)
* [Renew a Microsoft Store ID key](renew-a-windows-store-id-key.md)
* [Integrating Applications with Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/quickstart-register-app)
* [Understanding the Azure Active Directory application manifest]( https://go.microsoft.com/fwlink/?LinkId=722500)
* [Supported Token and Claim Types](https://docs.microsoft.com/azure/active-directory/develop/id-tokens)
