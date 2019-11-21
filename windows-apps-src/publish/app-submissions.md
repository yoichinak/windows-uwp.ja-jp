---
Description: 名前を予約してアプリを作成したら、そのアプリを公開するための作業を開始できます。 まず、申請を作成します。
title: アプリの申請
ms.assetid: 363BB9E4-4437-4238-A80F-ABDFC70D96E4
keywords: チェックリスト, windows, uwp, 申請, 提出, ゲーム, アプリ, 送信
ms.date: 10/31/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: bed7f232c8ec59771c6ae80a48b12bab1307ad68
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/20/2019
ms.locfileid: "74260022"
---
# <a name="app-submissions"></a>アプリの申請


[名前を予約してアプリを作成](create-your-app-by-reserving-a-name.md)したら、そのアプリを公開するための作業を開始できます。 まず、**申請**を作成します。

申請は、アプリが完成して公開する準備ができたときに開始できます。または、1 行のコードを記述する前でも情報を入力し始めることができます。 Updates you make to your submission are saved, so you can come back and work on it whenever you're ready.

> [!NOTE]
> You must have an active [developer account](https://developer.microsoft.com/store/register) in [Partner Center](https://partner.microsoft.com/dashboard) in order to submit apps to the Microsoft Store.

After your app is published, you can publish an updated version by creating another submission in Partner Center. 新しい申請を作成すると、新しいパッケージをアップロードするときでも、価格やカテゴリなどの情報を変更するだけのときでも、必要な変更を加えて公開することができます。 To create a new submission for a published app, click **Update** next to the most recent submission shown on its **Overview** page. You can also [remove an app from the Store](guidance-for-app-package-management.md#removing-an-app-from-the-store) if you need to do so (and then make it available again later, if you'd like).

> [!NOTE]
> This section of the documentation describes how to create an app submission in Partner Center. ここで説明する方法以外に、[Microsoft Store 申請 API](../monetize/create-and-manage-submissions-using-windows-store-services.md) を使用してアプリ申請を自動化することもできます。

> [!IMPORTANT]
> As of October 31, 2018, newly-created products cannot include packages targeting Windows 8.x/Windows Phone 8.x or earlier. For more info, see this [blog post](https://blogs.windows.com/windowsdeveloper/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store).

## <a name="app-submission-checklist"></a>アプリの申請用チェック リスト

ここで示しているのは、アプリの申請を作成するときに提供できる詳細情報と、詳細情報のリンクです。

提供または指定する情報は必須のものと、 省略可能なものがあります。提供されている既定値は必要に応じて変更できます。 You don't have to work on these sections in the order listed here.

### <a name="pricing-and-availability-page"></a>[価格と使用可能状況] ページ
| フィールド名                    | 注意                                       | 詳しい情報                                                             |
|-------------------------------|---------------------------------------------|---------------------------------------------------------------------------|
| **Markets**                   | 既定値: 対象となるすべての市場  | [Define pricing and market selection](define-pricing-and-market-selection.md)         |
| **オーディエンス**                | 既定値: パブリック対象ユーザー | [オーディエンス](choose-visibility-options.md#audience) |
| **Discoverability**                | 既定値: この製品を Microsoft Store で提供し、検索可能にします | [Discoverability](choose-visibility-options.md#discoverability) |
| **Schedule**                  | 既定値: 最短でリリース        | [Configure precise release scheduling](configure-precise-release-scheduling.md) |
| **Base price**                | 必須かどうか                                    | [Set and schedule app pricing](set-and-schedule-app-pricing.md)              |
| **無料試用版**                | 既定値: 無料の試用版なし                      | [無料試用版](set-app-pricing-and-availability.md#free-trial)              |
| **セール価格**              | オプション                                    | [アプリとアドオンを販売する](put-apps-and-add-ons-on-sale.md)           |
| **Organizational licensing**    | 既定値: このアプリの組織単位でのボリューム購入を許可する | [Organizational licensing options](organizational-licensing.md)        |
      |


### <a name="properties-page"></a>[プロパティ] ページ

| フィールド名                    | 注意                                       | 詳しい情報                                                             |
|-------------------------------|---------------------------------------------|---------------------------------------------------------------------------|
| **Category and subcategory**  | 必須かどうか                                    | [Category and subcategory table](category-and-subcategory-table.md)       |
| **Privacy policy URL**            | 多くのアプリでは必須。 「[アプリ開発者契約](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement)」と「[Microsoft Store ポリシー](store-policies.md#105-personal-information)」をご覧ください | [Privacy policy URL](enter-app-properties.md#privacy-policy-url)        |
| **Website**                   | オプション                                    | [Website](enter-app-properties.md#website)                   |
| **Support contact info**      | 製品が Xbox で使用可能な場合は必須。それ以外の場合は省略可能 (ただし推奨)                                   | [Support contact info](enter-app-properties.md#support-contact-info)              |
| **Game settings**             | 省略可能 (ゲームにのみ適用)         | [Game settings](enter-app-properties.md#game-settings) |
| **Display mode**             | オプション                   | [Display mode](enter-app-properties.md#display-mode) |
| **Product declarations**          | 既定値: ユーザーは、代替ドライブやリムーバブル ストレージにこのアプリをインストールできます。Windows はこのアプリのデータを OneDrive に自動的にバックアップできます | [Product declarations](app-declarations.md) |
| **システム要件**      | オプション                                    | [システム要件](enter-app-properties.md#system-requirements)      |

<span/>

### <a name="age-ratings-page"></a>[年齢区分] ページ

| フィールド名                    | 注意                                       | 詳しい情報                          |
|-------------------------------|---------------------------------------------|----------------------------------------|
| **Age ratings**               | 必須かどうか                                    | [Age ratings](age-ratings.md)          |

<span/>

### <a name="packages-page"></a>[パッケージ] ページ

| フィールド名                    | 注意                                  | 詳しい情報                          |
|-------------------------------|----------------------------------------|----------------------------------------|
| **Package upload control**    | 必須 (パッケージが 1 つ以上)        | [Upload app packages](upload-app-packages.md) |
| **Device family availability** | 既定値: パッケージに基づく       | [Device family availability](device-family-availability.md) |
| **Gradual package rollout**   | 省略可能 (更新プログラムのみ)            | [Gradual package rollout](gradual-package-rollout.md) |
| **Mandatory update**          | 省略可能 (更新プログラムのみ)            | [Mandatory update](upload-app-packages.md#mandatory-update)


### <a name="store-listings"></a>ストア登録情報

必須情報のすべてを、アプリでサポートする言語のうち、少なくとも 1 つの言語で用意する必要があります。 [ストア登録情報](create-app-store-listings.md)は、アプリでサポートするすべての言語で用意することをお勧めします。また[追加の言語でストア登録情報を用意](create-app-store-listings.md#store-listing-languages)することもできます。 同じ製品の複数の登録情報を管理しやすくするため、[Store 登録情報をインポートおよびエクスポート](import-and-export-store-listings.md)することができます。

| フィールド名                    | 注意                                       | 詳しい情報                                                     |
|-------------------------------|---------------------------------------------|-------------------------------------------------------------------|
| **説明**               | 必須かどうか                                    | [Write a great app description](write-a-great-app-description.md) |
| **What's new in this version**   | オプション                                 | [リリース ノート](create-app-store-listings.md#whats-new-in-this-version)       |
| **App features**              | オプション                                    | [Product features](create-app-store-listings.md#product-features)         |
| **Screenshots**               | 必須 (スクリーンショット 1 つ以上。4 つ以上を推奨)          | [Screenshots](app-screenshots-and-images.md#screenshots)          |
| **Store logos**               | 推奨。一部の OS バージョンで必須 | [Store logos](app-screenshots-and-images.md#store-logos)             |
| **Trailers**                  | オプション                                    | [Trailers](app-screenshots-and-images.md#trailers)                | 
| **Windows 10 and Xbox image (16:9 Super hero art)**     | おすすめ        | [Windows 10 and Xbox image (16:9 Super hero art)](app-screenshots-and-images.md#windows-10-and-xbox-image-169-super-hero-art) |
| **Xbox images**     | Required for proper display if you publish to Xbox        | [Xbox images](app-screenshots-and-images.md#xbox-images) |
| **Supplemental fields**  | オプション                                    | [Supplemental fields](create-app-store-listings.md#supplemental-fields) 
| **Search terms**              | オプション                                    | [Search terms](create-app-store-listings.md#search-terms)         |
| **Copyright and trademark info** | オプション                                 | [Copyright and trademark info](create-app-store-listings.md#copyright-and-trademark-info) |
| **Additional license terms**  | オプション                                    | [Additional license terms](create-app-store-listings.md#additional-license-terms) |
| **Developed by**              | オプション                                    | [Developed by](create-app-store-listings.md#developed-by)                   |


<span/>

### <a name="submission-options-page"></a>申請オプション ページ

| フィールド名                    | 注意                                       | 詳しい情報                                                     |
|-------------------------------|---------------------------------------------|-------------------------------------------------------------------|
| **Publishing hold options**     | 既定: この申請が認定されたら、すぐに申請を公開します (または [スケジュール] セクションで選択した各日付で公開します)      | [Publishing hold options](manage-submission-options.md#publishing-hold-options)    
| **Notes for certification**     | おすすめ          | [Notes for certification](notes-for-certification.md)             |
| **Restricted capabilities**     | Required if your product declares any [restricted capabilities](../packaging/app-capability-declarations.md#restricted-capabilities)    | [Restricted capabilities](manage-submission-options.md#publishing-hold-options)       

<span/>

> [!NOTE]
> 基幹業務 (LOB) アプリを企業に直接公開する方法については、「[LOB アプリの企業への配布](distribute-lob-apps-to-enterprises.md)」をご覧ください。
