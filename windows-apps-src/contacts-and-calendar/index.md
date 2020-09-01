---
description: コンテンツ、メール、カレンダー情報、メッセージを共有するために、ユーザーが連絡先や予定にアクセスできるようにする方法について説明します。
title: 連絡先とカレンダー
ms.assetid: b7e53ab5-2828-4fb7-8656-2bec70b3467f
ms.date: 05/18/2018
ms.topic: article
keywords: windows 10, uwp, 連絡先, カレンダー, 予定, メール メッセージ
ms.localizationpriority: medium
ms.openlocfilehash: 49fe277aa18627e304d2d6f27c023a13c85dccd0
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174056"
---
# <a name="contacts-my-people-and-calendar"></a>連絡先、マイ連絡先、カレンダー


ユーザーどうしがコンテンツ、メール、カレンダー情報、メッセージを共有したり、提供された機能を活用したりできるように、連絡先と予定へのアクセスを有効にします。

アプリで連絡先と予定にアクセスする方法については、次のトピックをご覧ください。

| トピック | 説明 |
|-------|-------------|
| [連絡先の選択](selecting-contacts.md) | [  <strong>Windows.ApplicationModel.Contacts</strong>](/uwp/api/Windows.ApplicationModel.Contacts) 名前空間では、複数の方法で連絡先を選ぶことができます。 ここでは、1 つまたは複数の連絡先を選ぶ方法について説明します。また、アプリで必要な連絡先情報だけを取得するように連絡先ピッカーを構成する方法についても説明します。 |
| [メールの送信](sending-email.md) | メールの作成ダイアログを起動して、ユーザーがメール メッセージを送信できるようにする方法について説明します。 ダイアログを表示する前に、メールの各フィールドにデータを設定することができます。 メッセージは、ユーザーが送信ボタンをタップするまで送信されません。 |
| [SMS メッセージの送信](sending-an-sms-message.md) | このトピックでは、SMS の作成ダイアログを起動して、ユーザーが SMS メッセージを送信できるようにする方法について説明します。 ダイアログを表示する前に、SMS の各フィールドにデータを設定することができます。 メッセージは、ユーザーが送信ボタンをタップするまで送信されません。 |
| [予定の管理](managing-appointments.md) | [  <strong>Windows.ApplicationModel.Appointments</strong>](/uwp/api/Windows.ApplicationModel.Appointments) 名前空間を使うと、ユーザーのカレンダー アプリで予定の作成と管理を行うことができます。 ここでは、予定を作成してカレンダー アプリに追加し、カレンダー アプリで置換して、カレンダー アプリから削除する方法を示します。 また、カレンダー アプリの一定の期間を表示し、予定の繰り返しオブジェクトを作る方法も示します。 |
| [アプリを連絡先カードの操作に接続する](integrating-with-contacts.md) | アプリを連絡先カードまたはミニ連絡先カードの操作の横に表示する方法を説明します。 ユーザーは、プロファイル ページを開く、通話を行う、メッセージを送信するなど、操作を実行するアプリを選ぶことができます。 |
| [アプリケーションにマイ連絡先のサポートを追加する](my-people-support.md) | アプリケーションにマイ連絡先のサポートを追加する方法と、タスク バーで連絡先をピン留めする方法およびピン留めを外す方法について説明します。 |
| [マイ連絡先の共有](my-people-sharing.md) | マイ連絡先の共有のサポートを追加する方法について説明します。マイ連絡先の共有によりユーザーは、ファイルをエクスプローラーからピン留めされたマイ連絡先にドラッグして、ピン留めされた連絡先とコンテンツを共有できます。 |
| [マイ連絡先の通知](my-people-notifications.md) | マイ連絡先の通知を作成して使用する方法について説明します。マイ連絡先の通知は、ピン留めした連絡先から送信される新しい種類のトースト通知です。 |

 

## <a name="related-topics"></a>関連トピック

* [予定 API のサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Appointments)
* [連絡先マネージャー API のサンプル](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/master/Official%20Windows%20Platform%20Sample/Contact%20manager%20API%20sample)
* [連絡先ピッカー アプリのサンプル](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/ContactPicker)
* [連絡先に関連する操作の処理のサンプル](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/master/Official%20Windows%20Platform%20Sample/Windows%208.1%20Store%20app%20samples/99866-Windows%208.1%20Store%20app%20samples/Handling%20Contact%20Actions)
* [連絡先カードの統合のサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ContactCardIntegration)