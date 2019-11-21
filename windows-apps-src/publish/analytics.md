---
Description: Get detailed analytics for your Windows apps, in Partner Center or via other methods.
title: アプリのパフォーマンスの分析
ms.assetid: 3A3C6F10-0DB1-416D-B632-CD388EA66759
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, analytics, reports, dashboard, apps, data, metrics
ms.localizationpriority: medium
ms.openlocfilehash: 3f75f861aa4f6f828ff7bb1cf829f5205a8ddaed
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259039"
---
# <a name="analyze-app-performance"></a>アプリのパフォーマンスの分析

You can view detailed analytics for your apps in [Partner Center](https://partner.microsoft.com/dashboard). 統計情報とチャートでは、アプリの状況 (獲得したユーザーから、ユーザーによるアプリの使い方、ユーザーによるアプリの評判まで) を知ることができます。 アプリの正常性、広告の使用状況などに関するメトリックも確認できます。

You can view analytic reports right in Partner Center or [download the reports you need](download-analytic-reports.md) to analyze your data offline. We also provide several ways for you to [access your analytics data outside of Partner Center](#outside).

## <a name="view-key-analytics-for-all-your-apps"></a>すべてのアプリについての主要な分析を表示する

最もダウンロードされたアプリについての主要な分析を表示するには、 **[分析]** を展開し、 **[概要]** を選びます。 既定では、概要ページには、有効期間内の取得数が最も多い 5 つのアプリに関する情報が表示されます。 別の発行済みのアプリを選んで表示するには、 **[フィルター]** を選びます。

## <a name="view-individual-reports-for-each-app"></a>アプリごとに個別レポートを表示する

ここでは、次の各レポートに表示される情報の詳細情報を示します。

-   [取得レポート](acquisitions-report.md)
-   [アドオン取得レポート](add-on-acquisitions-report.md)
-   [利用状況レポート](usage-report.md)
-   [Health report](health-report.md)
-   [Ratings report](ratings-report.md)
-   [レビュー レポート](reviews-report.md)
-   [フィードバック レポート](feedback-report.md)
-   [Xbox analytics report](xbox-analytics-report.md)
-   [Insights report](insights-report.md)
-   [広告パフォーマンス レポート](advertising-performance-report.md)
-   [広告キャンペーン レポート](promote-your-app-report.md)


> [!NOTE]
> アプリの実際の機能や実装によっては、これらのレポートの一部にデータが含まれていないことがあります。

<span id="outside"/>

## <a name="access-analytics-data-outside-of-partner-center"></a>Access analytics data outside of Partner Center

In addition to viewing reports in Partner Center, you can access app analytics in other ways.

### <a name="microsoft-store-analytics-api"></a>Microsoft Store 分析 API

[Microsoft Store 分析 API](../monetize/access-analytics-data-using-windows-store-services.md) を使うと、アプリの分析データをプログラムで取得できます。 この REST API では、アプリおよびアドオンの入手数、エラー、アプリの評価とレビューに関するデータを取得できます。 この API は、Azure Active Directory (Azure AD) を使って、アプリまたはサービスからの呼び出しを認証します。

### <a name="windows-dev-center-content-pack-for-power-bi"></a>Power BI 用 Windows デベロッパー センター コンテンツ パック

Use the [Windows Dev Center content pack for Power BI](https://powerbi.microsoft.com/documentation/powerbi-content-pack-windows-dev-center/) to explore and monitor your Partner Center analytics data in Power BI. Power BI とは、ビジネス データの 1 つのビューを提供するクラウド ベースのビジネス分析サービスです。

Power BI を使って分析データにアクセスするには、まず、次のリソースをご覧ください。

* [Sign up for Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-self-service-signup-for-power-bi/)
* [Learn how to use Power BI](https://powerbi.microsoft.com/guided-learning/)
* [Learn how to use the Windows Dev Center content pack for Power BI to connect to your analytics data](https://powerbi.microsoft.com/documentation/powerbi-content-pack-windows-dev-center/)

> [!NOTE]
> To connect to the Windows Dev Center content pack for Power BI, we recommend that you specify credentials from an Azure AD directory that is associated with your Partner Center account. Microsoft アカウントの資格情報を使う場合は、Power BI の分析データが自動的に更新されないため、Power BI にサインインしてデータを更新する必要があります。 組織で Office 365 または Microsoft の他のビジネス サービスが既に使用されている場合は、既に Azure AD をお持ちです。 それ以外の場合は、[こちらから無料で入手](https://account.azure.com/organization)できます。 For more information about setting up the association, see [Associate Azure Active Directory with your Partner Center account](associate-azure-ad-with-dev-center.md).
