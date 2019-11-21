---
Description: The Feedback report in Partner Center lets you see the problems, suggestions, and upvotes that your Windows 10 customers have submitted through Feedback Hub.
title: フィードバック レポート
ms.assetid: 9EA8B456-CA57-40CE-A55B-7BFDC55CA8A8
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 47eb494ac1b61caac0549f89254ae5d60a7ddf4c
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259015"
---
# <a name="feedback-report"></a>フィードバック レポート

The **Feedback report** in Partner Center lets you see the problems, suggestions, and upvotes that your Windows 10 customers have submitted through Feedback Hub. You can view this data in Partner Center, or export the data to view offline.

> [!NOTE]
> またフィードバックを確認していることを顧客に伝えるため、このレポートから直接[フィードバックに返信](respond-to-customer-feedback.md)できます。

アプリについてのフィードバックをユーザーに送信してもらうように促すのは、ユーザーにとって重要な問題や機能について知ることができる、よい方法です。 ユーザーは、アプリの作成者に直接フィードバックを送ることができるとわかると、否定的なレビューをフィードバックとしてストアに残す可能性が低くなることが考えられます。

[Microsoft Store Services SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftStoreServicesSDK) のフィードバック API を使って、ユーザーが直接[アプリからフィードバック Hub を起動](../monetize/launch-feedback-hub-from-your-app.md)するようにできます。 フィードバック Hub をサポートする Windows 10 デバイスにアプリをダウンロードしたユーザーはだれでも、フィードバック Hub アプリを使ってフィードバックを残すことができます。 Because of this, you may see customer feedback in this report even if you have not specifically asked for feedback from within your app.

Feedback can also be helpful when using [package flighting](package-flights.md), since the **Feedback** report shows you the specific package that each customer had installed on their device when they left the feedback.

> [!TIP]
> For a quick look at the reviews, ratings, and user feedback across all of your apps in the last 30 days, expand **Engage** in the left navigation menu and select **Reviews and feedback.** 


## <a name="apply-filters"></a>フィルターの適用

ページの上部で、データを表示する期間を選択できます。 既定では **[有効期間]** が選択されていますが、30 日間、3 か月間、6 か月間、12 か月間のデータを表示することもできます。

また、 **[フィルター]** を展開して、このページのすべてのデータを次のオプションを使ってフィルター処理できます。

- **[フィードバックの種類]** : 既定の設定は **[すべて]** です。 **[問題]** または **[提案]** を選択して、表示するフィードバックの種類を選択できます。
- **[デバイスの種類]** : 既定の設定は **[すべてのデバイス]** です。 このページにその種類のデバイスを使っているユーザーによるフィードバックのみを表示する場合は、特定のデバイスの種類を選ぶことができます。
- **[パッケージ バージョン]** : 既定の設定は **[すべてのパッケージ]** です。 パッケージの 1 つを選択して、ユーザーがフィードバックを行ったときに使っていた特定のパッケージのみのフィードバックを表示できます。
- **[市場]** : 既定の設定は **[すべての市場]** です。 特定の市場でのユーザーからフィードバックのみを表示することができます。
- **[グループ]** : 既定の設定は **[すべて]** です。 [Windows Insider](https://insider.windows.com) から送信されたフィードバックのみを表示することもできます。

> [!TIP]
> このページにフィードバックが表示されない場合は、フィルターですべてのフィードバックを除外していないかどうかを確認してください。 たとえば、アプリがサポートされていない**デバイスの種類**でフィルター処理する場合、フィードバックは表示されません。


## <a name="viewing-feedback-details"></a>フィードバックの詳細の表示

このレポートには、ユーザーからの個々のフィードバックが表示されます。 各項目のフィードバックのテキストの左には、フィードバック Hub で他のユーザーがそのフィードバックに投じた賛成票の回数が表示されます。 3 つの方法で、フィードバックを並べ替えることができます。

- **賛成票** (既定): 他のユーザーから最も多くの賛成票を受けたフィードバックを先頭に、賛成票を受けたフィードバックを表示します。
- **人気上昇中**: 最も直近に受けたフィードバックを先頭に、最近 7 日間に他のユーザーから賛成票を受けたフィードバックを表示します。
- **最近のフィードバック**: 最も直近のフィードバックを先頭に、すべてのフィードバックを表示します。

各コメントの横には、フィードバックを受けた日付と、フィードバックの種類が表示されます。 You’ll also see the customer’s market, the specific package that was installed on the device they were using when they left the feedback, the type of that device, and **Windows Insider** if the customer submitting the feedback is a member of the Windows Insider program.

ここには、[フィードバックに返信](respond-to-customer-feedback.md)するためのオプションも表示されます。


## <a name="translating-feedback"></a>フィードバックの翻訳

By default, feedback that was not written in your preferred language is translated for you. 必要に応じて、右上の、ページ フィルターの近くにある **[フィードバックを翻訳する]** チェック ボックスをオフにして、フィードバックの翻訳を無効にすることができます。

フィードバックは自動翻訳システムによって翻訳されるため、翻訳の結果が正確でない場合があることに注意してください。 元のテキストと翻訳を比較する場合や、元のテキストを他の方法で翻訳する場合は、元のテキストが提供されます。


## <a name="launching-feedback-hub-directly-from-your-app"></a>アプリから直フィードバック Hub を起動する

前に説明したように、フィードバック Hub への直接のリンクをアプリ内に組み込んで、ユーザーにフィードバックの提供を促すことをお勧めします。 詳しくは、「[アプリからのフィードバック Hub の起動](../monetize/launch-feedback-hub-from-your-app.md)」をご覧ください。
