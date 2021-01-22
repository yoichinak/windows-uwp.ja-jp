---
ms.assetid: B071F6BC-49D3-4E74-98EA-0461A1A55EFB
description: アプリとアドオンのカタログがある場合は、Microsoft Store コレクション API および Microsoft Store 購入 API を使って、サービスからこれらの製品の所有権情報にアクセスできます。
title: サービスによる製品の権利の管理
ms.date: 01/21/2021
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store コレクション API, Microsoft Store 購入 API, 製品の表示, 製品の付与
ms.localizationpriority: medium
ms.openlocfilehash: 7674a9b966510d914850e1fc8b2c8ca531f64a20
ms.sourcegitcommit: 069f5ab4be85a7d638fc2a426afaed824e5dfeae
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/21/2021
ms.locfileid: "98668730"
---
# <a name="manage-product-entitlements-from-a-service"></a>サービスによる製品の権利の管理

アプリとアドオンのカタログがある場合は、*Microsoft Store コレクション API* と *Microsoft Store 購入 API* を使って、サービスからこれらの製品の権利の情報にアクセスできます。 "権利" とは、Microsoft Store を通じて公開されたアプリまたはアドオンを顧客が使用する権利を表します。

これらの API は、クロスプラットフォーム サービスでサポートされるアドオン カタログを持つ開発者向けに設計された REST メソッドで構成されています。 これらの API を使用すると、次の操作を実行できます。

-   Microsoft Store コレクション API: [ユーザーが所有するアプリを照会](query-for-products.md)し、[コンシューマブルな製品をフルフィルメント完了として報告](report-consumable-products-as-fulfilled.md)する。
-   Microsoft Store 購入 API: [無料のアプリをユーザーに付与する](grant-free-products.md)、[ユーザーのサブスクリプションを取得する](get-subscriptions-for-a-user.md)、[ユーザーのサブスクリプションに関する請求の状態を変更する](change-the-billing-state-of-a-subscription-for-a-user.md)。

> [!NOTE]
> Microsoft Store コレクション API と Microsoft Store 購入 API では、Azure Active Directory (Azure AD) 認証を使って顧客の所有権情報にアクセスします。 これらの API を使用するには、ユーザー (またはユーザーの組織) が、Azure AD ディレクトリと、そのディレクトリに対する[全体管理者](/azure/active-directory/users-groups-roles/directory-assign-admin-roles)のアクセス許可を持っている必要があります。 Microsoft 365 または Microsoft の他のビジネス サービスをすでに使用している場合、Azure AD ディレクトリをすでに所有しています。

## <a name="overview"></a>概要

次の手順は、Microsoft Store コレクション API および購入 API を使用するプロセス全体を表したものです。

1.  [Azure AD でアプリケーションを構成](#step-1)します。
2.  [Azure AD アプリケーション ID をパートナーセンターのアプリに関連付け](#step-2)ます。
3.  サービスで、発行元 ID を表す [Azure AD アクセス トークンを作成します](#step-3)。
4.  クライアントの Windows アプリで、現在のユーザーの id を表す [MICROSOFT STORE ID キーを作成](#step-4) し、このキーをサービスに戻します。

このエンドツーエンドのプロセスには、さまざまなタスクを実行する2つのソフトウェアコンポーネントが含まれます。

* **サービス**。 これは、ビジネス環境のコンテキストで安全に実行されるアプリケーションであり、選択した任意の開発プラットフォームを使用して実装できます。 サービスは、シナリオに必要な Azure AD アクセストークンを作成し、Microsoft Store collection API と purchase API の REST Uri を呼び出す役割を担います。
* **クライアントの Windows アプリ**。 これは、顧客の権利情報 (アプリのアドオンを含む) にアクセスして管理するアプリです。 このアプリは、サービスから Microsoft Store collection API と購入 API を呼び出すために必要な Microsoft Store ID キーを作成する役割を担います。

<span id="step-1"/>

## <a name="step-1-configure-an-application-in-azure-ad"></a>手順 1: Azure AD でアプリケーションを構成する

Microsoft Store collection API または purchase API を使用するには、Azure AD Web アプリケーションを作成し、アプリケーションのテナント ID とアプリケーション ID を取得して、キーを生成する必要があります。 Azure AD Web アプリケーションは、Microsoft Store collection API または purchase API の呼び出し元のサービスを表します。 API を呼び出すために必要な Azure AD アクセストークンを生成するには、テナント ID、アプリケーション ID、およびキーが必要です。

1.  まだ行っていない場合は、「アプリケーションを [Azure Active Directory と統合](/azure/active-directory/develop/active-directory-integrating-applications) して、 **WEB アプリ/API** アプリケーションを Azure AD に登録する」の手順に従ってください。
    > [!NOTE]
    > アプリケーションを登録するときに、アプリケーションの種類として [ **Web アプリ/API** ] を選択して、アプリケーションのキー ( *クライアントシークレット* とも呼ばれます) を取得できるようにする必要があります。 Microsoft Store コレクション API または購入 API を呼び出すには、後の手順で Azure AD からアクセス トークンを要求するときにクライアント シークレットを指定する必要があります。

2.  [Azure 管理ポータル](https://portal.azure.com/)で、 **Azure Active Directory** に移動します。 ディレクトリを選択し、左側のナビゲーションウィンドウで [ **アプリの登録** ] をクリックして、アプリケーションを選択します。
3.  アプリケーションのメイン登録ページが表示されます。 このページで、[ **アプリケーション ID** ] の値をコピーして後で使用します。
4.  後で必要になるキーを作成します (これは、 *クライアントシークレット* と呼ばれます)。 左側のウィンドウで、[ **設定** ]、[ **キー**] の順にクリックします。 このページで、 [キーを作成](/azure/active-directory/develop/active-directory-integrating-applications#to-add-application-credentials-or-permissions-to-access-web-apis)する手順を完了します。 後で使用するためにこのキーをコピーします。

<span id="step-2"/>

## <a name="step-2-associate-your-azure-ad-application-id-with-your-client-app-in-partner-center"></a>手順 2: パートナーセンターで Azure AD アプリケーション ID をクライアントアプリに関連付ける

Microsoft Store collection API または purchase API を使用してアプリまたはアドオンの所有権と購入を構成する前に、パートナーセンターで Azure AD アプリケーション ID をアプリ (またはアドオンを含むアプリ) に関連付ける必要があります。

> [!NOTE]
> この作業を行うのは一度だけです。 テナント ID、アプリケーション ID、およびクライアントシークレットを取得した後、新しい Azure AD アクセストークンを作成する必要がある場合は、いつでもこれらの値を再利用できます。

1.  [パートナーセンター](https://partner.microsoft.com/dashboard)にサインインし、アプリを選択します。
2.  **サービス** &gt; **製品コレクションと購入** ページにアクセスし、[使用可能な **クライアント id** ] フィールドのいずれかに Azure AD アプリケーション id を入力します。

<span id="step-3"/>

## <a name="step-3-create-azure-ad-access-tokens"></a>手順 3: Azure AD アクセス トークンを作成する

Microsoft Store ID キーを取得したり、Microsoft Store コレクション API または Microsoft Store 購入 API を呼び出したりする前に、発行元 ID を表すいくつかの Azure AD アクセス トークンをサービスで作成する必要があります。 各トークンは別々の API で使われます。 各トークンの有効期間は 60 分であり、有効期限を過ぎた場合は更新できます。

> [!IMPORTANT]
> Azure AD アクセス トークンは、アプリ内ではなく、サービスのコンテキスト内でのみ作成してください。 このアクセス トークンがアプリに送信されると、クライアント シークレットが侵害される可能性があります。

<span id="access-tokens" />

### <a name="understanding-the-different-tokens-and-audience-uris"></a>さまざまなトークンとオーディエンス URI を理解する

Microsoft Store コレクション API または購入 API で呼び出そうとしているメソッドに応じて、2 つまたは 3 つの異なるトークンを作成する必要があります。 各アクセストークンは、異なる対象ユーザーの URI に関連付けられています。

  * いずれの場合も、`https://onestore.microsoft.com` オーディエンス URI のトークンを 1 つ作成する必要があります。 このトークンは、後の手順で Microsoft Store コレクション API または購入 API のメソッドの **Authorization** ヘッダーに渡します。
      > [!IMPORTANT]
      > `https://onestore.microsoft.com` 対象ユーザーには、サービス内に安全に格納されたアクセス トークンのみを使用してください。 このオーディエンスのアクセス トークンがサービス外に公開されると、サービスがリプレイ攻撃に対して脆弱になる可能性があります。

  * Microsoft Store コレクション API のメソッドを呼び出して、[特定のユーザーが所有する製品を照会](query-for-products.md)したり、[コンシューマブルな製品をフルフィルメント完了として報告](report-consumable-products-as-fulfilled.md)したりする場合は、`https://onestore.microsoft.com/b2b/keys/create/collections` オーディエンス URI のトークンも作成する必要があります。 後の手順で、このトークンを Windows SDK のクライアント メソッドに渡し、Microsoft Store コレクション API で使用できる Microsoft Store ID キーを要求します。

  * Microsoft Store 購入 API でメソッドを呼び出して[無料の製品をユーザーに付与する](grant-free-products.md)、[ユーザーのサブスクリプションを取得する](get-subscriptions-for-a-user.md)、または[ユーザーのサブスクリプションに関する請求の状態を変更する](change-the-billing-state-of-a-subscription-for-a-user.md)場合、`https://onestore.microsoft.com/b2b/keys/create/purchase` 対象ユーザー URI を使ってトークンも作成する必要があります。 後の手順で、このトークンを Windows SDK のクライアント メソッドに渡し、Microsoft Store 購入 API で使用できる Microsoft Store ID キーを要求します。

<span />

### <a name="create-the-tokens"></a>トークンの作成

アクセス トークンを作成するには、「[クライアント資格情報を使用したサービス間の呼び出し](/azure/active-directory/azuread-dev/v1-oauth2-client-creds-grant-flow)」の手順に従って OAuth 2.0 API をサービスで使用し、HTTP POST を ```https://login.microsoftonline.com/<tenant_id>/oauth2/token``` エンドポイントに送信します。 要求の例を次に示します。

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

* [ *クライアント \_ id* ] と [ *クライアント \_ シークレット* ] のパラメーターには、 [Azure 管理ポータル](https://portal.azure.com/)から取得したアプリケーションのアプリケーション id とクライアントシークレットを指定します。 これらのパラメーターはいずれも、Microsoft Store コレクション API または購入 API で必要とされる認証のレベルに基づいてアクセス トークンを生成するために必要です。

* *resource* パラメーターには、作成するアクセス トークンの種類に応じて、[前のセクション](#access-tokens)に記載したいずれかのオーディエンス URI を指定します。

アクセス トークンの有効期限が切れた後は、[この](/azure/active-directory/azuread-dev/v1-protocols-oauth-code#refreshing-the-access-tokens)手順に従って更新できます。 アクセス トークンの構造について詳しくは、「[サポートされているトークンと要求の種類](/azure/active-directory/develop/id-tokens)」をご覧ください。

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

  * アプリで [Windows.Services.Store](/uwp/api/windows.services.store) 名前空間の [StoreContext](/uwp/api/Windows.Services.Store.StoreContext) クラスを使ってアプリ内購入を管理する場合は、[StoreContext.GetCustomerCollectionsIdAsync](/uwp/api/windows.services.store.storecontext.getcustomercollectionsidasync) メソッドを使用します。

  * アプリで [Windows.ApplicationModel.Store](/uwp/api/windows.applicationmodel.store) 名前空間の  [CurrentApp](/uwp/api/Windows.ApplicationModel.Store.CurrentApp) クラスを使ってアプリ内購入を管理する場合は、[CurrentApp.GetCustomerCollectionsIdAsync](/uwp/api/windows.applicationmodel.store.currentapp.getcustomercollectionsidasync) メソッドを使用します。

    メソッドの *serviceTicket* パラメーターに、Azure AD アクセス トークンを渡します。 現在のアプリの発行元として管理するサービスのコンテキストで匿名ユーザー Id を保持する場合は、 *publisherUserId* パラメーターにユーザー id を渡して、現在のユーザーと新しい Microsoft Store id キーを関連付けることもできます (ユーザー id はキーに埋め込まれます)。 それ以外の場合、ユーザー ID を Microsoft Store ID キーに関連付ける必要がない場合は、 *publisherUserId* パラメーターに任意の文字列値を渡すことができます。

3.  アプリで正しく Microsoft Store ID キーを作成したら、そのキーをサービスに渡します。

<span />

### <a name="to-create-a-microsoft-store-id-key-for-the-microsoft-store-purchase-api"></a>Microsoft Store 購入 API 用の Microsoft Store ID キーを作成するには

[無料の製品をユーザーに付与する](grant-free-products.md)、[ユーザーのサブスクリプションを取得する](get-subscriptions-for-a-user.md)、または[ユーザーのサブスクリプションの請求状態を変更する](change-the-billing-state-of-a-subscription-for-a-user.md)ために、Microsoft Store 購入 API と共に使うことができる Microsoft Store ID キーを作成するには、以下の手順に従います。

1.  オーディエンス URI 値 `https://onestore.microsoft.com/b2b/keys/create/purchase` を持つ Azure AD アクセス トークンを、サービスからクライアント アプリに渡します。 これは、[前述の手順 3](#step-3) で作成したトークンのいずれかです。

2.  アプリ コードで次のいずれかのメソッドを呼び出して、Microsoft Store ID キーを取得します。

  * アプリで [Windows.Services.Store](/uwp/api/windows.services.store) 名前空間の [StoreContext](/uwp/api/Windows.Services.Store.StoreContext) クラスを使ってアプリ内購入を管理する場合は、[StoreContext.GetCustomerPurchaseIdAsync](/uwp/api/windows.services.store.storecontext.getcustomerpurchaseidasync) メソッドを使用します。

  * アプリで [Windows.ApplicationModel.Store](/uwp/api/windows.applicationmodel.store) 名前空間の [CurrentApp](/uwp/api/Windows.ApplicationModel.Store.CurrentApp) クラスを使ってアプリ内購入を管理する場合は、[CurrentApp.GetCustomerPurchaseIdAsync](/uwp/api/windows.applicationmodel.store.currentapp.getcustomerpurchaseidasync) メソッドを使用します。

    メソッドの *serviceTicket* パラメーターに、Azure AD アクセス トークンを渡します。 現在のアプリの発行元として管理するサービスのコンテキストで匿名ユーザー Id を保持する場合は、 *publisherUserId* パラメーターにユーザー id を渡して、現在のユーザーと新しい Microsoft Store id キーを関連付けることもできます (ユーザー id はキーに埋め込まれます)。 それ以外の場合、ユーザー ID を Microsoft Store ID キーに関連付ける必要がない場合は、 *publisherUserId* パラメーターに任意の文字列値を渡すことができます。

3.  アプリで正しく Microsoft Store ID キーを作成したら、そのキーをサービスに渡します。

### <a name="diagram"></a>ダイアグラム

次の図は、Microsoft Store ID キーを作成するプロセスを示しています。

  ![Windows ストア ID キーを作成する](images/b2b-1.png)

## <a name="claims-in-a-microsoft-store-id-key"></a>Microsoft Store ID キー内の要求

Microsoft Store ID キーは、ユーザーの製品所有権情報にアクセスする場合にそのユーザーの ID を表す JSON Web トークン (JWT) です。 Base64 を使用してデコードされた Microsoft Store ID キーには、次の要求が含まれています。

* `iat`: &nbsp; &nbsp; &nbsp; キーが発行された時刻を示します。 この要求は、トークンの経過期間の判別に使用できます。 この値はエポック時間で表されます。
* `iss`: &nbsp; &nbsp; &nbsp; 発行者を識別します。 これは `aud` 要求と同じ値を持ちます。
* `aud`: &nbsp; &nbsp; &nbsp; 対象ユーザーを識別します。 `https://collections.mp.microsoft.com/v6.0/keys` または `https://purchase.mp.microsoft.com/v6.0/keys` のいずれかの値である必要があります。
* `exp`: キーを &nbsp; &nbsp; &nbsp; 更新する場合を除き、キーを処理するための有効期限を指定します。 この要求の値はエポック時間で表されます。
* `nbf`: &nbsp; &nbsp; &nbsp; トークンが処理に受け入れられる時刻を指定します。 この要求の値はエポック時間で表されます。
* `http://schemas.microsoft.com/marketplace/2015/08/claims/key/clientId`: &nbsp; &nbsp; &nbsp; 開発者を識別するクライアント ID。
* `http://schemas.microsoft.com/marketplace/2015/08/claims/key/payload`: &nbsp; &nbsp; &nbsp; Microsoft Store services での使用のみを目的とした情報を含む、不透明なペイロード (暗号化および Base64 エンコード)。
* `http://schemas.microsoft.com/marketplace/2015/08/claims/key/userId`: &nbsp; &nbsp; &nbsp; サービスのコンテキストで現在のユーザーを識別するユーザー ID。 これは、[キーの作成に使うメソッド](#step-4)の省略可能な *publisherUserId* パラメーターに渡す値と同じです。
* `http://schemas.microsoft.com/marketplace/2015/08/claims/key/refreshUri`: &nbsp; &nbsp; &nbsp; キーを更新するために使用できる URI。

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

* [製品の照会](query-for-products.md)
* [コンシューマブルな製品をフルフィルメント完了として報告する](report-consumable-products-as-fulfilled.md)
* [無料の製品の付与](grant-free-products.md)
* [ユーザーのサブスクリプションの取得](get-subscriptions-for-a-user.md)
* [ユーザーのサブスクリプションに関する請求の状態を変更する](change-the-billing-state-of-a-subscription-for-a-user.md)
* [Microsoft Store ID キーの更新](renew-a-windows-store-id-key.md)
* [Azure Active Directory とアプリケーションの統合](/azure/active-directory/develop/quickstart-register-app)
* [Azure Active Directory のアプリケーション マニフェストについて]( https://go.microsoft.com/fwlink/?LinkId=722500)
* [サポートされているトークンとクレームの種類](/azure/active-directory/develop/id-tokens)
