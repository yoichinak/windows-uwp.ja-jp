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

申請は、アプリが完成して公開する準備ができたときに開始できます。または、1 行のコードを記述する前でも情報を入力し始めることができます。 提出物に対して行った更新は保存されるので、準備ができたらいつでも戻って作業することができます。

> [!NOTE]
> Microsoft Store にアプリを送信するには、[パートナーセンター](https://partner.microsoft.com/dashboard)にアクティブな[開発者アカウント](https://developer.microsoft.com/store/register)が必要です。

アプリが発行されたら、パートナーセンターで別の送信を作成して、更新されたバージョンを発行できます。 新しい申請を作成すると、新しいパッケージをアップロードするときでも、価格やカテゴリなどの情報を変更するだけのときでも、必要な変更を加えて公開することができます。 発行されたアプリの新しい送信を作成するには、 **[概要]** ページに表示されている最新の送信の横にある **[更新]** をクリックします。 また、必要に応じて[ストアからアプリを削除](guidance-for-app-package-management.md#removing-an-app-from-the-store)することもできます (必要に応じて、後で再度使用できるようにします)。

> [!NOTE]
> ドキュメントのこのセクションでは、パートナーセンターでアプリの送信を作成する方法について説明します。 ここで説明する方法以外に、[Microsoft Store 申請 API](../monetize/create-and-manage-submissions-using-windows-store-services.md) を使用してアプリ申請を自動化することもできます。

> [!IMPORTANT]
> 2018年10月31日の時点で、新しく作成された製品には、Windows 8.x/Windows Phone 8.x 以前を対象とするパッケージを含めることはできません。 詳細については、こちらの[ブログ投稿](https://blogs.windows.com/windowsdeveloper/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store)を参照してください。

## <a name="app-submission-checklist"></a>アプリの申請用チェック リスト

ここでは、アプリの申請を作成するときに提供できる詳細情報と、それぞれの詳細へのリンクを示します。

提供または指定する情報には、必須のものと 省略可能なものがあります。提供されている既定値は必要に応じて変更できます。 ここに記載されている順序でこれらのセクションを使用する必要はありません。

### <a name="pricing-and-availability-page"></a>[価格と使用可能状況] ページ
| フィールド名                    | 説明                                       | 詳しい情報                                                             |
|-------------------------------|---------------------------------------------|---------------------------------------------------------------------------|
| **新た**                   | 既定値: 対象となるすべての市場  | [価格と市場の選択を定義する](define-pricing-and-market-selection.md)         |
| **オーディエンス**                | 既定値: パブリック対象ユーザー | [オーディエンス](choose-visibility-options.md#audience) |
| **やすく**                | 既定値: この製品を Microsoft Store で提供し、検索可能にします | [やすく](choose-visibility-options.md#discoverability) |
| **予定**                  | 既定値: 最短でリリース        | [正確なリリーススケジュールの構成](configure-precise-release-scheduling.md) |
| **基本料金**                | 必須                                    | [アプリの価格を設定してスケジュールする](set-and-schedule-app-pricing.md)              |
| **無料試用版**                | 既定値: 無料の試用版なし                      | [無料試用版](set-app-pricing-and-availability.md#free-trial)              |
| **セール価格**              | 省略可能                                    | [アプリとアドオンを販売する](put-apps-and-add-ons-on-sale.md)           |
| **組織のライセンス**    | 既定値: 組織単位でのボリューム購入を許可する | [組織のライセンスオプション](organizational-licensing.md)        |
      |


### <a name="properties-page"></a>[プロパティ] ページ

| フィールド名                    | 説明                                       | 詳しい情報                                                             |
|-------------------------------|---------------------------------------------|---------------------------------------------------------------------------|
| **カテゴリとサブカテゴリ**  | 必須                                    | [カテゴリおよびサブカテゴリテーブル](category-and-subcategory-table.md)       |
| **プライバシーポリシーの URL**            | 多くのアプリでは必須。 「[アプリ開発者契約](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement)」と「[Microsoft Store ポリシー](store-policies.md#105-personal-information)」をご覧ください | [プライバシーポリシーの URL](enter-app-properties.md#privacy-policy-url)        |
| **用**                   | 省略可能                                    | [用](enter-app-properties.md#website)                   |
| **サポートの連絡先情報**      | 製品が Xbox で使用可能な場合は必須。それ以外の場合は省略可能 (ただし推奨)                                   | [サポートの連絡先情報](enter-app-properties.md#support-contact-info)              |
| **ゲームの設定**             | 省略可能 (ゲームにのみ適用)         | [ゲームの設定](enter-app-properties.md#game-settings) |
| **表示モード**             | 省略可能                   | [表示モード](enter-app-properties.md#display-mode) |
| **製品宣言**          | 既定値: ユーザーは、代替ドライブやリムーバブル ストレージにこのアプリをインストールできます。Windows はこのアプリのデータを OneDrive に自動的にバックアップできます | [製品宣言](app-declarations.md) |
| **システム要件**      | 省略可能                                    | [システム要件](enter-app-properties.md#system-requirements)      |

<span/>

### <a name="age-ratings-page"></a>[年齢区分] ページ

| フィールド名                    | 説明                                       | 詳しい情報                          |
|-------------------------------|---------------------------------------------|----------------------------------------|
| **年齢区分**               | 必須                                    | [年齢区分](age-ratings.md)          |

<span/>

### <a name="packages-page"></a>[パッケージ] ページ

| フィールド名                    | 説明                                  | 詳しい情報                          |
|-------------------------------|----------------------------------------|----------------------------------------|
| **パッケージアップロードコントロール**    | 必須 (パッケージが 1 つ以上)        | [アプリパッケージのアップロード](upload-app-packages.md) |
| **デバイスファミリの可用性** | 既定値: パッケージに基づく       | [デバイスファミリの可用性](device-family-availability.md) |
| **段階的パッケージのロールアウト**   | 省略可能 (更新プログラムのみ)            | [段階的パッケージのロールアウト](gradual-package-rollout.md) |
| **必須の更新**          | 省略可能 (更新プログラムのみ)            | [必須の更新](upload-app-packages.md#mandatory-update)


### <a name="store-listings"></a>ストア登録情報

必須情報のすべてを、アプリでサポートする言語のうち、少なくとも 1 つの言語で用意する必要があります。 [Store 登録情報](create-app-store-listings.md)は、アプリでサポートするすべての言語で用意することをお勧めします。また[追加の言語で Store 登録情報を用意](create-app-store-listings.md#store-listing-languages)することもできます。 同じ製品の複数の登録情報を管理しやすくするため、[Store 登録情報をインポートおよびエクスポート](import-and-export-store-listings.md)することができます。

| フィールド名                    | 説明                                       | 詳しい情報                                                     |
|-------------------------------|---------------------------------------------|-------------------------------------------------------------------|
| **説明**               | 必須                                    | [優れたアプリの説明を書く](write-a-great-app-description.md) |
| **このバージョンの新機能**   | 省略可能                                 | [リリース ノート](create-app-store-listings.md#whats-new-in-this-version)       |
| **アプリの機能**              | 省略可能                                    | [製品の機能](create-app-store-listings.md#product-features)         |
| **ショット**               | 必須 (スクリーンショット 1 つ以上。4 つ以上を推奨)          | [ショット](app-screenshots-and-images.md#screenshots)          |
| **ロゴを格納する**               | 推奨。一部の OS バージョンで必須 | [ロゴを格納する](app-screenshots-and-images.md#store-logos)             |
| **トレーラー**                  | 省略可能                                    | [トレーラー](app-screenshots-and-images.md#trailers)                | 
| **Windows 10 と Xbox イメージ (16:9 のスーパーヒーローアート)**     | 推奨        | [Windows 10 と Xbox イメージ (16:9 のスーパーヒーローアート)](app-screenshots-and-images.md#windows-10-and-xbox-image-169-super-hero-art) |
| **Xbox イメージ**     | Xbox に発行した場合に適切に表示するために必要です        | [Xbox イメージ](app-screenshots-and-images.md#xbox-images) |
| **補足フィールド**  | 省略可能                                    | [補足フィールド](create-app-store-listings.md#supplemental-fields) 
| **検索語句**              | 省略可能                                    | [検索語句](create-app-store-listings.md#search-terms)         |
| **著作権および商標情報** | 省略可能                                 | [著作権および商標情報](create-app-store-listings.md#copyright-and-trademark-info) |
| **追加のライセンス条項**  | 省略可能                                    | [追加のライセンス条項](create-app-store-listings.md#additional-license-terms) |
| **開発者**              | 省略可能                                    | [開発者](create-app-store-listings.md#developed-by)                   |


<span/>

### <a name="submission-options-page"></a>申請オプション ページ

| フィールド名                    | 説明                                       | 詳しい情報                                                     |
|-------------------------------|---------------------------------------------|-------------------------------------------------------------------|
| **パブリッシュ保留オプション**     | 既定: この申請が認定されたら、すぐに申請を公開します (または [スケジュール] セクションで選択した各日付で公開します)      | [パブリッシュ保留オプション](manage-submission-options.md#publishing-hold-options)    
| **認定のメモ**     | 推奨          | [認定のメモ](notes-for-certification.md)             |
| **制限された機能**     | 製品で制限された[機能](../packaging/app-capability-declarations.md#restricted-capabilities)を宣言する場合は必須    | [制限された機能](manage-submission-options.md#publishing-hold-options)       

<span/>

> [!NOTE]
> 基幹業務 (LOB) アプリを企業に直接公開する方法については、「[LOB アプリの企業への配布](distribute-lob-apps-to-enterprises.md)」をご覧ください。
