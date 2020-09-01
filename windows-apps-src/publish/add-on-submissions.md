---
description: アプリのアドオン (アプリ内製品とも呼ばれます) を、顧客が購入できるアプリの補助項目として送信する方法について説明します。
title: アドオンの申請
ms.assetid: E175AF9E-A1D4-45DF-B353-5E24E573AE67
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, iap, アプリ内購入, アプリ内製品, iap の申請
ms.localizationpriority: medium
ms.openlocfilehash: 25246d192f40bae096c33fae00feb1bfc2a4c530
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162148"
---
# <a name="add-on-submissions"></a>アドオンの申請

アドオン (アプリ内製品とも呼ばれる) は、お客様が購入可能なアプリの補助アイテムです。 アドオンには、楽しい新しい機能、新しいゲームレベル、またはユーザーの関与を続けると思われるその他のものがあります。 アドオンは収益を得るためだけでなく、お客様との意見交換や顧客エンゲージメントの獲得を促すためにも役立ちます。

アドオンは [パートナーセンター](https://partner.microsoft.com/dashboard)を通じて公開され、アクティブな [開発者アカウント](https://developer.microsoft.com/store/register)を持っている必要があります。 また、アプリのコードで [アドオンを有効にする](../monetize/in-app-purchases-and-trials.md)ことも必要です。

アドオンの送信プロセスの最初の手順は、パートナーセンターでアドオンを作成することです。そのためには、 [製品の種類と製品 ID を定義](set-your-add-on-product-id.md)します。 その後、Microsoft Store を使用してアドオンを購入できるように、送信を作成します。 [アプリの申請](app-submissions.md)と同時にまたは別々にアドオンを申請できます。 アプリがストアに公開された後は、アプリを再び申請することなく、アドオンを[更新](#updating-an-add-on-after-publication)できます。

> [!NOTE]
> ドキュメントのこのセクションでは、パートナーセンターでアドオンを送信する方法について説明します。 ここで説明する方法以外に、[Microsoft Store 申請 API](../monetize/create-and-manage-submissions-using-windows-store-services.md) を使用してアドオン申請を自動化することもできます。


## <a name="checklist-for-submitting-an-add-on"></a>アドオンの申請用チェック リスト

アドオンの申請を作成するときに提供する情報の一覧を次に示します。 提供が必須である情報については、次の一覧に示してあります。 また、省略可能な情報もあります。既に提供されている既定値は必要に応じて変更できます。


### <a name="create-a-new-add-on-page"></a>新しいアドオン ページを作成する

| フィールド名                    | Notes                            |
|-------------------------------|----------------------------------|
| [**製品の種類**](set-your-add-on-product-id.md#product-type)      | 必須 |  
| [**Product ID**](set-your-add-on-product-id.md#product-id)          | 必須 |        


### <a name="properties-page"></a>[プロパティ] ページ

| フィールド名                    | Notes                              |   
|-------------------------------|------------------------------------|
| [**製品の有効期限**](enter-add-on-properties.md#product-lifetime)  | 製品の種類が **[永続的]** である場合は必須です。 他の種類の製品には適用されません。 |
| [**Quantity**](enter-add-on-properties.md#quantity)  | 製品の種類が **[ストアで管理されるコンシューマブル]** の場合は必須です。 他の種類の製品には適用されません。 |
| [**サブスクリプション期間**](enter-add-on-properties.md#subscription-period)          | 製品の種類が **[サブスクリプション]** である場合は必須です。 他の種類の製品には適用されません。       |  
| [**無料試用版**](enter-add-on-properties.md#free-trial)          | 製品の種類が **[サブスクリプション]** である場合は必須です。 他の種類の製品には適用されません。       |
| [**コンテンツの種類**](enter-add-on-properties.md#content-type)          | 必須    |               
| [**Keywords**](enter-add-on-properties.md#keywords)                  | 省略可能 (10 キーワードまで、それぞれ 30 文字以内)。 |
| [**カスタムの開発者データ**](enter-add-on-properties.md#custom-developer-data)   | 省略可能 (3,000 文字以内)。            |


### <a name="pricing-and-availability-page"></a>[価格と使用可能状況] ページ

| フィールド名                    | Notes                                       |
|-------------------------------|---------------------------------------------|
| [**市場**](set-add-on-pricing-and-availability.md#markets)  | 既定値: 対象となるすべての市場 |
| [**視程**](set-add-on-pricing-and-availability.md#visibility)   | 既定値: 購入可能。 アプリのリストに表示されます |
| [**スケジュール**](set-add-on-pricing-and-availability.md#schedule)    | 既定値: 最短でリリース
| [**価格**](set-add-on-pricing-and-availability.md#pricing)                | 必須                                    |
| [**販売価格**](put-apps-and-add-ons-on-sale.md)               | オプション                    |


### <a name="store-listings"></a>ストア登録情報

1 つのストア登録情報が必要です。 アプリでサポートされている [言語](create-add-on-store-listings.md#store-listing-languages) ごとにストアの一覧を提供することをお勧めします。

| フィールド名                    | Notes                                       |
|-------------------------------|---------------------------------------------|
| [**タイトル**](create-add-on-store-listings.md#title)                    | 必須 (100 文字以内)           |
| [**説明**](create-add-on-store-listings.md#description)       | 省略可能 (200 文字以内)            |
| [**表す**](create-add-on-store-listings.md#icon)                    | 省略可能 (.png、300 x 300 ピクセル)            |


これらの情報の入力が完了したら、**[Submit to the Store]** (ストアに提出) をクリックします。 ほとんどの場合、認定プロセスは約 1 時間かかります。 その後、アドオンはストアに公開され、お客様が購入できるようになります。

> [!NOTE]
> アドオンは、アプリのコードでも実装する必要があります。 詳しくは、「[アプリ内購入と試用版](../monetize/in-app-purchases-and-trials.md)」をご覧ください。


## <a name="updating-an-add-on-after-publication"></a>公開後のアドオンの更新

公開したアドオンはいつでも変更できます。 アドオンの変更は、アプリとは別に送信および発行されるので、通常、価格や説明の更新などのアドオンに変更を加えるために、アプリ全体を更新する必要はありません。

更新プログラムを送信するには、パートナーセンターのアドオンのページにアクセスし、[ **更新**] をクリックします。 これにより、前の送信の情報を出発点として使用して、アドオンの新しい送信が作成されます。 必要な変更を行い、[ **ストアに送信**] をクリックします。

既に提供されているアドオンを削除する場合は、新しい申請を作成して、[[分布と認知度]](set-add-on-pricing-and-availability.md) オプションを **[ストアに表示しない]** に変更し、**[購入の停止]** オプションを選択します。 必要に応じてアプリのコードを更新して、アドオンへの参照も削除するようにしてください (特に、以前に発行されたアプリが以前に Windows 8.1 をサポートしている場合は、この可視性の設定はこれらの顧客に適用されません)。

> [!IMPORTANT]
> 以前に発行されたアプリを Windows 8.x のお客様が利用できる場合は、新しいアプリの提出を作成して発行する必要があります。これにより、アドオンの更新が顧客に表示されるようになります。 同様に、アプリを公開した後で、Windows 8.x を対象とする新しいアドオンをアプリに追加する場合は、アドオンを参照するようにアプリのコードを更新して、アプリを再申請する必要があります。 それ以外の場合、新しいアドオンは、Windows 8.x のユーザーには表示されません。
