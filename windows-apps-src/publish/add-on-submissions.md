---
Description: Add-ons (or in-app products) are published through Partner Center.
title: アドオンの申請
ms.assetid: E175AF9E-A1D4-45DF-B353-5E24E573AE67
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, iap, アプリ内購入, アプリ内製品, iap の申請
ms.localizationpriority: medium
ms.openlocfilehash: 87e5298f677a078eb1d6f89b72af3ea15d44b75f
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259057"
---
# <a name="add-on-submissions"></a>アドオンの申請

アドオン (アプリ内製品とも呼ばれる) は、お客様が購入可能なアプリの補助アイテムです。 An add-on can be a fun new feature, a new game level, or anything else you think will keep users engaged. アドオンは収益を得るためだけでなく、お客様との意見交換や顧客エンゲージメントの獲得を促すためにも役立ちます。

Add-ons are published through [Partner Center](https://partner.microsoft.com/dashboard), and require you to have an active [developer account](https://developer.microsoft.com/store/register). また、アプリのコードで [アドオンを有効にする](../monetize/in-app-purchases-and-trials.md)ことも必要です。

The first step in the add-on submission process is to create the add-on in Partner Center by [defining its product type and product ID](set-your-add-on-product-id.md). After that, you'll create a submission so that your add-on can be purchased via the Microsoft Store. [アプリの申請](app-submissions.md)と同時にまたは別々にアドオンを申請できます。 アプリがストアに公開された後は、アプリを再び申請することなく、アドオンを[更新](#updating-an-add-on-after-publication)できます。

> [!NOTE]
> This section of the documentation describes how to submit add-ons in Partner Center. ここで説明する方法以外に、[Microsoft Store 申請 API](../monetize/create-and-manage-submissions-using-windows-store-services.md) を使用してアドオン申請を自動化することもできます。


## <a name="checklist-for-submitting-an-add-on"></a>アドオンの申請用チェック リスト

アドオンの申請を作成するときに提供する情報の一覧を次に示します。 提供が必須である情報については、次の一覧に示してあります。 また、省略可能な情報もあります。既に提供されている既定値は必要に応じて変更できます。


### <a name="create-a-new-add-on-page"></a>新しいアドオン ページを作成する

| フィールド名                    | 注意                            |
|-------------------------------|----------------------------------|
| [**Product type**](set-your-add-on-product-id.md#product-type)      | 必須かどうか |  
| [**Product ID**](set-your-add-on-product-id.md#product-id)          | 必須かどうか |        


### <a name="properties-page"></a>[プロパティ] ページ

| フィールド名                    | 注意                              |   
|-------------------------------|------------------------------------|
| [**Product lifetime**](enter-add-on-properties.md#product-lifetime)  | 製品の種類が **[永続的]** である場合は必須です。 他の種類の製品には適用されません。 |
| [**Quantity**](enter-add-on-properties.md#quantity)  | 製品の種類が **[ストアで管理されるコンシューマブル]** の場合は必須です。 他の種類の製品には適用されません。 |
| [**Subscription period**](enter-add-on-properties.md#subscription-period)          | 製品の種類が **[サブスクリプション]** である場合は必須です。 他の種類の製品には適用されません。       |  
| [**Free trial**](enter-add-on-properties.md#free-trial)          | 製品の種類が **[サブスクリプション]** である場合は必須です。 他の種類の製品には適用されません。       |
| [**Content type**](enter-add-on-properties.md#content-type)          | 必須かどうか    |               
| [**Keywords**](enter-add-on-properties.md#keywords)                  | 省略可能 (最大で 10 個まで。それぞれ 30 文字以内)。 |
| [**Custom developer data**](enter-add-on-properties.md#custom-developer-data)   | 省略可能 (3,000 文字以内)。            |


### <a name="pricing-and-availability-page"></a>[価格と使用可能状況] ページ

| フィールド名                    | 注意                                       |
|-------------------------------|---------------------------------------------|
| [**Markets**](set-add-on-pricing-and-availability.md#markets)  | 既定値: 対象となるすべての市場 |
| [**Visibility**](set-add-on-pricing-and-availability.md#visibility)   | 既定値: 購入可能。 アプリのリストに表示されます |
| [**Schedule**](set-add-on-pricing-and-availability.md#schedule)    | 既定値: 最短でリリース
| [**Pricing**](set-add-on-pricing-and-availability.md#pricing)                | 必須かどうか                                    |
| [**Sale pricing**](put-apps-and-add-ons-on-sale.md)               | オプション                    |


### <a name="store-listings"></a>ストア登録情報

1 つのストア登録情報が必要です。 アプリがサポートする各[言語](create-add-on-store-listings.md#store-listing-languages)でストア登録情報を提供することをお勧めします。

| フィールド名                    | 注意                                       |
|-------------------------------|---------------------------------------------|
| [**Title**](create-add-on-store-listings.md#title)                    | 必須 (100 文字以内)           |
| [**Description**](create-add-on-store-listings.md#description)       | 省略可能 (200 文字以内)            |
| [**Icon**](create-add-on-store-listings.md#icon)                    | 省略可能 (.png、300 x 300 ピクセル)            |


これらの情報の入力が完了したら、 **[ストアに提出]** をクリックします。 ほとんどの場合、認定プロセスは約 1 時間かかります。 その後、アドオンはストアに公開され、お客様が購入できるようになります。

> [!NOTE]
> アドオンは、アプリのコードでも実装する必要があります。 詳しくは、「[アプリ内購入と試用版](../monetize/in-app-purchases-and-trials.md)」をご覧ください。


## <a name="updating-an-add-on-after-publication"></a>公開後のアドオンの更新

公開したアドオンはいつでも変更できます。 Add-on changes are submitted and published independently of your app, so you generally don't need to update the entire app in order to make changes to an add-on such as updating its price or description.

To submit updates, go to the add-on's page in Partner Center and click **Update**. This will create a new submission for the add-on, using the info from your previous submission as a starting point. Make the changes you'd like, and then click **Submit to the Store**.

既に提供されているアドオンを削除する場合は、新しい申請を作成して、[[分布と認知度]](set-add-on-pricing-and-availability.md) オプションを **[ストアに表示しない]** に変更し、 **[購入の停止]** オプションを選択します。 Be sure to update your app's code as needed to also remove references to the add-on (especially if your previously-published app supports Windows 8.1 earlier; this visibility setting won't apply to those customers).

> [!IMPORTANT]
> If your previously-published app is available to customers on Windows 8.x, you will need to create and publish a new app submission in order to make the add-on updates visible to those customers. 同様に、アプリを公開した後で、Windows 8.x を対象とする新しいアドオンをアプリに追加する場合は、アドオンを参照するようにアプリのコードを更新して、アプリを再申請する必要があります。 それ以外の場合、新しいアドオンは、Windows 8.x のユーザーには表示されません。
