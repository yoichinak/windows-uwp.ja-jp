---
Description: パートナーセンターまたは他の方法を使用して、Windows アプリの詳細な分析を取得します。
title: アプリのパフォーマンスの分析
ms.assetid: 3A3C6F10-0DB1-416D-B632-CD388EA66759
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10、uwp、分析、レポート、ダッシュボード、アプリ、データ、メトリック
ms.localizationpriority: medium
ms.openlocfilehash: 9b14ee776b31e21fbb8a0ee1d66b933648549b69
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162086"
---
# <a name="analyze-app-performance"></a>アプリのパフォーマンスの分析

[パートナーセンター](https://partner.microsoft.com/dashboard)で、アプリの詳細な分析を表示できます。 統計情報とチャートでは、アプリの状況 (獲得したユーザーから、ユーザーによるアプリの使い方、ユーザーによるアプリの評判まで) を知ることができます。 アプリの正常性、広告の使用状況などに関するメトリックも確認できます。

パートナーセンターで分析レポートを直接表示することも、データをオフラインで分析するために [必要なレポートをダウンロード](download-analytic-reports.md) することもできます。 また、 [パートナーセンター外で分析データにアクセス](#outside)するためのいくつかの方法も用意されています。

## <a name="view-key-analytics-for-all-your-apps"></a>すべてのアプリについての主要な分析を表示する

最もダウンロードされたアプリについての主要な分析を表示するには、**[分析]** を展開し、**[概要]** を選びます。 既定では、概要ページには、有効期間内の取得数が最も多い 5 つのアプリに関する情報が表示されます。 別の発行済みのアプリを選んで表示するには、**[フィルター]** を選びます。

## <a name="view-individual-reports-for-each-app"></a>アプリごとに個別レポートを表示する

ここでは、次の各レポートに表示される情報の詳細情報を示します。

-   [[取得] レポート](acquisitions-report.md)
-   [アドオン取得レポート](add-on-acquisitions-report.md)
-   [利用状況レポート](usage-report.md)
-   [状態レポート](health-report.md)
-   [レーティング レポート](ratings-report.md)
-   [レビュー レポート](reviews-report.md)
-   [フィードバック レポート](feedback-report.md)
-   [Xbox 分析レポート](xbox-analytics-report.md)
-   [インサイト レポート](insights-report.md)
-   [広告パフォーマンス レポート](advertising-performance-report.md)
-   [広告キャンペーン レポート](/windows/uwp/publish/ad-campaign-report)


> [!NOTE]
> アプリの実際の機能や実装によっては、これらのレポートの一部にデータが含まれていないことがあります。

<span id="outside"/>

## <a name="access-analytics-data-outside-of-partner-center"></a>パートナーセンター外での分析データへのアクセス

パートナーセンターでレポートを表示するだけでなく、他の方法でもアプリ分析にアクセスできます。

### <a name="microsoft-store-analytics-api"></a>Microsoft Store 分析 API

[Microsoft Store 分析 API](../monetize/access-analytics-data-using-windows-store-services.md) を使うと、アプリの分析データをプログラムで取得できます。 この REST API では、アプリおよびアドオンの入手数、エラー、アプリの評価とレビューに関するデータを取得できます。 この API は、Azure Active Directory (Azure AD) を使って、アプリまたはサービスからの呼び出しを認証します。

### <a name="windows-dev-center-content-pack-for-power-bi"></a>Power BI 用 Windows デベロッパー センター コンテンツ パック

[Power BI 用の Windows デベロッパーセンターコンテンツパック](https://powerbi.microsoft.com/documentation/powerbi-content-pack-windows-dev-center/)を使用して、Power BI でパートナーセンターの分析データを探索および監視します。 Power BI とは、ビジネス データの 1 つのビューを提供するクラウド ベースのビジネス分析サービスです。

Power BI を使って分析データにアクセスするには、まず、次のリソースをご覧ください。

* [Power BI にサインアップする](https://powerbi.microsoft.com/documentation/powerbi-service-self-service-signup-for-power-bi/)
* [Power BI の使用方法に関する詳細](https://powerbi.microsoft.com/guided-learning/)
* [Power BI 用 Windows デベロッパー センター コンテンツ パックを使って分析データに接続する方法](https://powerbi.microsoft.com/documentation/powerbi-content-pack-windows-dev-center/)

> [!NOTE]
> Power BI 用に Windows デベロッパーセンターコンテンツパックに接続するには、パートナーセンターアカウントに関連付けられている Azure AD ディレクトリから資格情報を指定することをお勧めします。 Microsoft アカウントの資格情報を使う場合は、Power BI の分析データが自動的に更新されないため、Power BI にサインインしてデータを更新する必要があります。 組織で既に Microsoft の Microsoft 365 またはその他のビジネスサービスを使用している場合は、既に Azure AD があります。 それ以外の場合は、[こちらから無料で入手](https://account.azure.com/organization)できます。 アソシエーションの設定の詳細については、「 [Azure Active Directory とパートナーセンターアカウントの関連付け](./associate-azure-ad-with-partner-center.md)」を参照してください。