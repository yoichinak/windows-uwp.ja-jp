---
author: PatrickFarley
title: ユーザーのアクティビティのベスト プラクティス
description: このガイドでは、作成して、ユーザーのアクティビティの更新についての推奨事項について説明します。
keywords: ユーザー アクティビティ、ユーザー アクティビティ、タイムライン、Cortana の前回終了した位置から再開する、Cortana の前回終了した位置から再開する、Project Rome
ms.author: pafarley
ms.date: 08/23/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: b11151df981a4b5ce233458581d21e405be9844c
ms.sourcegitcommit: 753dfcd0f9fdfc963579dd0b217b445c4b110a18
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/27/2018
ms.locfileid: "2854985"
---
# <a name="user-activities-best-practices"></a>ユーザーのアクティビティのベスト プラクティス

このガイドでは、作成して、ユーザーのアクティビティの更新についての推奨事項について説明します。 Windows ユーザーのアクティビティの機能の概要については、[デバイスにわたって、続行ユーザーのアクティビティ](https://docs.microsoft.com/windows/uwp/launch-resume/useractivities)を参照してください。 または、その他の開発プラットフォーム アクティビティの実装のプロジェクト ローマの[ユーザーの操作] セクション](https://docs.microsoft.com/windows/project-rome/user-activities/)を参照してください。

## <a name="when-to-create-or-update-user-activities"></a>ユーザーの活動作成または更新します。

すべてのアプリケーションが異なるため、ユーザーのアクティビティにアプリ内での操作をマップする最善の方法を決定するには、各開発するです。 ユーザー アクティビティ、過去の訪問したコンテンツに戻るして増加のユーザーの生産性と効率に重点を置いた、Cortana とタイムラインの中です。

**一般的なガイドライン**

* **関連するユーザーの操作のグループの 1 つのアクティビティを記録します。** これは、音楽を再生または番組が特に: 1 つの活動は、ユーザーの進捗状況を反映するように定期的に更新できます。 この例では、複数の日または週契約の期間を表す複数の履歴項目を 1 つのユーザー アクティビティがあります。 ユーザーがアプリ内で段階的な進行状況を行うための文書ベースの活動にも当てはまります。
* **ユーザーのデータをクラウドに保存します。** デバイス間の活動をサポートする場合は、この活動を再利用するに必要なコンテンツがクラウドの場所に保存されているかどうかを確認する必要があります。 デバイスに固有のアクティビティが表示タイムライン上をデバイスにアクティビティが作成されたが他のデバイスでは表示されない場合があります。
* **アクションを再開するのには、ユーザーは必要はありませんの活動は作成されません。** 場合は、アプリケーションを使用すると、状態が保持されない単純な 1 回限りの操作を完了すると、可能性がありますユーザーの活動を作成する必要はありません。
* **他のユーザーが完了したアクションの活動は作成されません。** 外部アカウントがメッセージをユーザーに送信する場合、または@-mentionsは、アプリ内で必要があるいない活動を作成するこのします。 この種類の操作が方がアクション センター通知します。
  * グループ作業のシナリオが例外: 場合は、複数のユーザーで作業している同じアクティビティ化 (Word 文書) がある場合は、他のユーザーが変更を行った後、ユーザーにします。 この例では、文書に加えられた変更を反映するように、既存のアクティビティを更新することがあります。 これにより、新しい履歴項目を作成せずに、既存のユーザーのアクティビティのコンテンツ データの更新が含まれます。

**特定の種類のアプリのためのガイドライン**

すべてのアプリケーションをさまざまな、ほとんどのアプリケーションは、対話のパターンを次のいずれかに分類されます。
* **文書ベース アプリ**: 使用期間を反映した 1 つまたは複数の履歴アイテムがある文書ごとに 1 つのアクティビティを作成します。 文書に変更を加えるアクティビティを更新する重要です。
* **ゲーム**: 各ワールドまたは保存するゲームの 1 つの活動を作成します。 ゲームでは、1 つのレベルのシーケンスのみがサポートされている場合は、最新の進捗状況または成績を表示するコンテンツのデータを更新することができますに、時間の経過とともに同じアクティビティをもう一度公開ことができます。
* **ユーティリティ アプリ**: ユーザーのままにして再開する必要がありますアプリ内で何がある場合はユーザーのアクティビティを使用する必要はありません。 たとえば、電卓のような単純なアプリがします。
* **基幹業務アプリケーション**単純なタスクまたはワークフローを管理するために多くのアプリケーションが存在します。 アプリを使用してアクセス独立したワークフローごとに 1 つのアクティビティを作成する (たとえば、経費レポートをそれぞれ別のアクティビティであるとしているユーザーが特定のレポートが承認されたかどうかに表示するためのアクティビティ] をクリックし、)。
* **メディアの再生アプリ**: コンテンツ (再生リスト、プログラム、またはスタンドアロンのコンテンツ) などの論理グループごとの 1 つの活動を作成します。 アプリの開発者の基になる質問は、スタンドアロンのコンテンツまたはコレクションの一部として、それぞれのコンテンツ (テレビ エピソード、曲) をカウントするかどうか。 一般に、コレクションまたは連続したコンテンツを再生するユーザーを選んだ場合、コレクション全体のサイズはアクティビティです。 1 つのコンテンツを再生するオプションを選択すると、[コンテンツの 1 つはアクティビティです。 以下の具体的なガイドラインを参照してください。
  * **音楽: アルバム/アーティスト/ジャンル**: ユーザーは、アルバム、アーティスト、またはジャンルを選択し、**再生**ヒット、そのコレクションがアクティビティそれぞれの曲を別のアクティビティを作成できません。 1 つのアルバムのような短いコレクションまたはコレクションをランダムに再生される、する必要はありません、ユーザーの現在の位置を反映するように活動を更新します。 アルバムや再生リストなどの長い連続再生、アルバム内の位置を記録する必要があります。
  * **音楽: スマート プレイリスト**-をランダムに音楽を再生するアプリケーションは、その再生リストの 1 つのアクティビティを記録する必要があります。 ユーザーをもう一度再生リストが再生される場合、同じアクティビティの追加の履歴レコードを作成します。 順序がランダムなので、再生リスト内のユーザーの現在の位置を記録する必要はありません。
  * **テレビ シリーズ**-現在が完了した後は、次の回を再生するアプリを構成する場合は、テレビ シリーズの 1 つの活動を作成する必要があります。 複数表示セッションの間でさまざまな録画を再生する場合にシリーズでは、現在の位置を反映するように、アクティビティを更新して、履歴の複数のレコードが作成されます。
  * **ビデオ**: ビデオ コンテンツを 1 つ、独自の履歴を持つ必要があります。 ユーザーは、ムービー途中での監視を停止する場合は、その位置を記録することをお勧めします。 今後、再開するのには、希望アクティビティ可能性のある、中断、ムービーを再開するか再開または先頭から開始するかどうかは、ユーザーにも確認します。

## <a name="user-activity-design"></a>ユーザーのアクティビティのデザイン

3 つのコンポーネントで構成されるユーザーのアクティビティ: URI をアクティブ化、視覚的なデータとコンテンツ メタデータします。
* ライセンス認証 URI は、特定のコンテキストでアプリケーションを再開するのには、アプリケーションまたはエクスペリエンスに渡される URI です。 一般的に、これらのリンクは、プロトコル ハンドラー (たとえば、"my-app://page2?action=edit") のパターンのフォームを実行します。 開発者は、アプリでの URI パラメーターの処理方法を決定する必要があります。 詳細については、[処理の URI のライセンス認証](https://docs.microsoft.com/windows/uwp/launch-resume/handle-uri-activation)を参照してください。
* 一連のプロパティを必要と省略可能なで構成されている、視覚的なデータ (例: タイトル、説明、または適応カード要素)、アクティビティを視覚的に識別できるようにします。 アクティビティの適応カード視覚効果を作成する方法については、下を参照してください。
* コンテンツのメタデータは、JSON データをグループ化し、特定のコンテキストの活動を取得するために使用できます。 一般的に、次の形式のhttp://schema.orgデータ。 このデータを入力する方法については、下を参照してください。

### <a name="adaptive-card-design-guidelines"></a>適応カード デザインのガイドライン

タイムラインでのアクティビティが表示されたら、[適応カード フレームワーク](https://docs.microsoft.com/adaptive-cards/)を使用して表示されます。 開発者がアクティビティごとに、適応カードいない場合は、タイムラインはアプリの名前/アイコン、必要な [タイトル] フィールド、オプションの説明] フィールドに基づく単純なカードを自動的に作成されます。 

アプリの開発者は、単純な適応カード JSON スキーマを使用してユーザー設定のカードを提供することをお勧めします。 適応カードのオブジェクトを作成する方法の手順についてはテクニカル[適応カードのマニュアル](https://docs.microsoft.com/adaptive-cards/authoring-cards/getting-started)を参照してください。 下にあるユーザーのアクティビティの適応のカードをデザインするためのガイドラインを参照してください。
* 画像を使う
  * 可能であれば、アクティビティごとに一意の画像を使用します。 アプリケーションの名前とアイコンが自動的に表示されますアクティビティのカードの横にあります。その他の画像には、ユーザーが探している活動を見つけますが役立ちます。
  * 画像にはテキストが含まれていないユーザーが閲覧と予想されます。 このテキストは、ユーザー補助を必要とするできないし、検索することはできません。
  * 2:1 はトリミングすることができますイメージがテキストが含まれていない場合、背景画像として使用する必要があります。 これは、結果、タイムラインのいなければならないは太字のアクティビティ カード。 画像は、カードで、テキストが見えなくし、のみ小さいテキストが読みにくくなるよう、活動名をこの例では、使用することをお勧めしていることを確認するやや暗くされます。
  * 2:1 には、画像がトリミングされることはできません、アクティビティ カード内で配置する必要があります。  
    * 縦横比が正方形または縦向きの場合は、余白なしで、カードの右側にある画像を固定します。
    * 縦横比が横向きである場合は、カードの右上隅にイメージを固定します。
* それぞれのアクティビティを常に表示するか、アクティビティの名前を付ける必要があります。
  * この名前は、大きな太字オプションを使用して、カードの左上隅に表示する必要があります。 重要である、わかりやすいのみが Cortana のシナリオでアクティビティを表示すると一部のユーザーに表示されます。 タイムラインで同じ名前が表示されていると、ユーザーが多数のアクティビティを参照するやすくなります。
* タイムラインのユーザーがアプリのアクティビティを簡単に見つけられるように、すべてのアプリでは、アクティビティの同じ視覚スタイルを使用します。
  * たとえば、アクティビティ必要がありますすべて同じ背景色が使用します。
* テキストの補足的な情報を] は慎重に使用します。 
  * テキストのカードを防ぐし、のみで、右側のアクティビティを検索するユーザーを支援または (特定のタスクの現在の進捗状況) などの状態の情報を反映する補足情報を使用します。

### <a name="content-metadata-guidelines"></a>コンテンツのメタデータのガイドライン

ユーザーのアクティビティには、コンテンツのメタデータは、Windows および Cortana を使用して、アクティビティを分類する推定を生成も格納できます。 (この場合、ユーザーが別のアプリや web サイト全体で特定の製品買い物)、アクティビティを (この場合、ユーザーは休暇を調査して) の場所 (ユーザーでは何かを調査して) 場合は、オブジェクト、やアクションなど、特定のトピックをグループ化し、ことができます。 名詞とアクティビティに関連する動詞の両方を表すことをお勧めします。 

次の例では、JSON、次の標準の[Schema.org](https://schema.org/)、コンテンツのメタデータを表すシナリオ:「John 再生 angry と鳥 Steve」。

```json
// John played angry birds with Steve.
{
  "@context": "http://schema.org",
  "@type": "PlayAction",
  "agent": {
    "@type": "Person",
    "name": "John"
  },
  "object": {
    "@type": "MobileApplication",
    "name": "Angry Birds."
  },
  "participant": {
    "@type": "Person",
    "name": "Steve"
  }
}
```

## <a name="key-apis"></a>キー API

* [UserActivities 名前空間](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities)

## <a name="related-topics"></a>関連トピック

* [ユーザー アクティビティ (Project ローマ ドキュメント)](https://docs.microsoft.com/windows/project-rome/user-activities/)
* [アダプティブ カード](https://docs.microsoft.com/adaptive-cards/)
* [アダプティブ カード ビジュアライザー、サンプル](http://adaptivecards.io/)
* [URI のアクティブ化の処理](https://docs.microsoft.com/windows/uwp/launch-resume/handle-uri-activation)
* [Microsoft Graph、アクティビティ フィード、およびアダプティブ カードを使用して、どのプラットフォームでも顧客の関心を惹きつける](https://channel9.msdn.com/Events/Connect/2017/B111)
* [Microsoft Graph](https://developer.microsoft.com/graph/)