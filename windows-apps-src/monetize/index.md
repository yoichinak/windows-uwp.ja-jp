---
ms.assetid: 4e8cc0c0-b14c-472c-9e1c-4601d10289d2
description: Windows SDK、Microsoft Advertising SDK、Microsoft Store Services SDK、Microsoft Store は、アプリによる収益の向上と顧客エンゲージメントによるユーザー獲得のための多くの機能を提供します。
title: 収益化、エンゲージメント、Microsoft Store サービス
ms.date: 11/29/2017
ms.topic: article
keywords: Windows 10, UWP, 収益化, エンゲージメント, プロモーション, ストア サービス
ms.localizationpriority: medium
ms.openlocfilehash: 29dcc02375358c7f16f38748ca92dbaeaf3ebdb7
ms.sourcegitcommit: afc4ff2c89f148d32073ab1cc42063ccdc573a8c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/11/2021
ms.locfileid: "98104703"
---
# <a name="monetization-engagement-and-store-services"></a>収益化、エンゲージメント、Microsoft Store サービス

Windows SDK、Microsoft Advertising SDK、Microsoft Store Services SDK、Microsoft Store は、アプリによる収益の向上と顧客エンゲージメントによるユーザー獲得のための機能を提供します。 このセクションのトピックでは、これらの機能をアプリに組み込む方法について説明します。

Microsoft Store の手数料に関する説明とアプリの収益を受け取る方法について詳しくは、「[支払いの受け取り](/partner-center/marketplace-get-paid)」をご覧ください。

## <a name="choose-a-pricing-model"></a>価格モデルを選ぶ

:::row:::
    :::column:::
        ![アプリの価格を請求する](images/pricing-charge-price.png)
    :::column-end:::
    :::column span="2":::
**アプリの価格を請求する**

アプリの価格を前払いで請求できます。 市場ごとの価格を設定するオプションを含めて、包括的な価格帯範囲をサポートしています。 特売のスケジュールを設定し、一定の時間アプリの価格を値下げすることもできます。

[アプリの価格を設定する](../publish/set-app-pricing-and-availability.md)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![無料試用版](images/pricing-free-trial.png)
    :::column-end:::
    :::column span="2":::
**無料試用版**

アプリの無料試用版を提供して、より多くのお客様に試用してもらうことができます。 通常版の購入を促すために、試用版での機能を制限したり (たとえば、ゲームの最初のレベルのみにする) 広告を表示したり、期限付きの試用版を指定できます。

[試用版を提供する](in-app-purchases-and-trials.md)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![アプリ内購入](images/pricing-in-app-purchases.png)
    :::column-end:::
    :::column span="2":::
**アプリ内購入**

アプリの価格を請求する場合でも、無料で提供する場合でも、アプリでアプリ内購入を使って、継続的な収益を実現できます。 アプリ内購入を使うと、お客様にアプリの無料版から有料版へのアップグレードを提供したり、継続的アドオンやコンシューマブル アドオンをアプリ内で販売したりすることができます。

[アプリ内購入を使用する](in-app-purchases-and-trials.md)
    :::column-end:::
:::row-end:::

## <a name="monetize-your-app-with-ads"></a>広告によるアプリの収益の獲得

:::row:::
    :::column:::
        ![各コンテキストに合わせた広告](images/monetize-ads-every-context.png)
    :::column-end:::
    :::column span="2":::
**各コンテキストに合わせた広告**

バナー広告、スポット広告 (バナーおよびビデオ)、リニア ビデオ広告、再生可能広告、ネイティブ広告など、さまざまな広告をサポートし、ほとんどのニーズに対応しています。 マイクロソフトのプラットフォームは、OpenRTB、VAST 2.x、MRAID 2、および VPAID 3 の各標準に準拠しており、MOAT および IAS と互換性があります。

[広告オプションを探す]()
[広告 SDK をインストールする](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![広告仲介サービス](images/monetize-ad-mediation-service.png)
    :::column-end:::
    :::column span="2":::
**広告仲介サービス**

複数の人気のある広告ネットワークから広告を提供する、マイクロソフトの広告仲介サービスを使って、アプリでの広告収益を最大化します。 コード行を変更せずに、パートナー センターで、仲介の設定を構成できます。 マイクロソフトに仲介を設定させる場合、マイクロソフトの機械学習アルゴリズムによって、お客様のアプリがサポートしている市場の広告収益を最大化できます。

[広告サービスを使用する](https://blogs.windows.com/windowsdeveloper/2017/05/08/announcing-microsofts-ad-mediation-service/)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![Analytics](images/monetize-analytics-pie-chart.png)
    :::column-end:::
    :::column span="2":::
**Analytics**

詳細な分析レポートを使って、アプリ内広告のパフォーマンスを確認でき、広告の収益を最大化するのに必要な情報が得られます。 このデータをプログラムで取得するために使える RESTful API も提供します。

[パフォーマンスを確認する](../publish/advertising-performance-report.md)
    :::column-end:::
:::row-end:::

## <a name="other-monetization-opportunities"></a>その他の収益機会

![その他の収益機会](images/monetize-other-opportunities.png)

収益を増やす他の方法については、 次のオプションを検討してください。

 トピック                | 説明                 |
|--------------------|-----------------------------|
| [マイクロソフトのアフィリエイト プログラム](https://www.microsoftaffiliates.com/)。 | アプリ、ブログ、Web ページなどのコミュニケーション ツールから Microsoft 製品にリンクして、手数料を獲得できます。 リンクの対象として使うことができるのは、Microsoft Store で販売されているアプリ、ゲーム、音楽、ムービー、ハードウェア、アクセサリなどの商品があります。
| [A/B テスト](./run-app-experiments-with-a-b-testing.md) | アプリの機能の変更をすべてのユーザー向けに有効にする前に、アプリで A/B テストを実行し、一部のユーザーを対象として機能の変更の有効性を測定します。
| [Microsoft Store Services SDK を使ってユーザーとの関係を深める](microsoft-store-services-sdk.md) | Microsoft Store Services SDK が提供するライブラリとツールを利用すると、顧客エンゲージメントの獲得を図る機能をアプリに追加できます。 これらの機能には、ターゲット通知、A/B テスト、アプリからのフィードバック Hub の起動が含まれます。
| [アプリからのフィードバック Hub の起動](launch-feedback-hub-from-your-app.md) | UWP アプリにコードを追加して、Windows 10 ユーザーをフィードバック Hub に誘導して、ユーザーが問題、提案、賛成票を送信できるようにします。 次に、パートナー センターの[フィードバック レポート](../publish/feedback-report.md)でこのフィードバックを管理します。 この機能を使うには、Microsoft Store Services SDK が必要です。 
| [パートナー センターのプッシュ通知を受信するようにアプリを設定する](configure-your-app-to-receive-dev-center-notifications.md) | UWP アプリの通知チャンネルを登録して、[パートナー センターのプッシュ通知](../publish/send-push-notifications-to-your-apps-customers.md)を受信し、プッシュ通知の結果としてのアプリの起動率を追跡できるようにします。 この機能を使うには、Microsoft Store Services SDK が必要です。
| [パートナー センターのカスタム イベントをログに記録する](log-custom-events-for-dev-center.md) | UWP アプリからのカスタム イベントをログに記録し、パートナー センターの[利用状況レポート](../publish/usage-report.md)のイベントを確認します。 この機能を使うには、Microsoft Store Services SDK が必要です。
| [評価とレビューを求める](request-ratings-and-reviews.md) | プログラムで評価とレビュー UI を表示して、ユーザーにアプリの評価やレビューを求めます。
| [Microsoft Store サービス](using-windows-store-services.md) | RESTful API を使用して、ストアへの申請の自動化、アプリの分析データへのアクセス、ストアに関連するその他のタスクの自動化を行う方法を説明します。
| [アプリへの市販デモ (RDX) 機能の追加](retail-demo-experience.md) | 販売フロアで PC やデバイスを試しているユーザーがすぐに開始できるように、Windows アプリに小売デモ モードを含めます。

## <a name="monetization-analytics"></a>収益の分析

![収益の分析](images/monetize-analytics.png)

これらのレポート使って、Microsoft Store でのアプリのパフォーマンスを監視します。

- [支払いの概要](/partner-center/payout-statement)
- [[取得] レポート](../publish/acquisitions-report.md)
- [アドオン取得レポート](../publish/add-on-acquisitions-report.md)
- [広告パフォーマンス レポート](../publish/advertising-performance-report.md)
- [REST API を使った分析データの取得](access-analytics-data-using-windows-store-services.md)
- [ユーザー セグメントを作成する](../publish/create-customer-segments.md)
- [フィードバック レポート](../publish/feedback-report.md)
- [利用状況レポート](../publish/usage-report.md)