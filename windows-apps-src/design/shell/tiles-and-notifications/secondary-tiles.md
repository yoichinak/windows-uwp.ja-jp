---
author: anbare
Description: Secondary tiles allow users to pin specific content and deep links from your app onto their Start menu, providing easy future access to the content within your app.
title: セカンダリ タイル
label: Secondary tiles
template: detail.hbs
ms.author: wdg-dev-content
ms.date: 05/25/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP, セカンダリ タイル
ms.localizationpriority: medium
ms.openlocfilehash: 269951e0e03758a614d14561a9504d0768049c5c
ms.sourcegitcommit: f9a4854b6aecfda472fb3f8b4a2d3b271b327800
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/12/2017
ms.locfileid: "1394181"
---
# <a name="secondary-tiles"></a>セカンダリ タイル


セカンダリ タイルを使うと、ユーザーは特定のコンテンツやディープ リンクをアプリからスタート メニューにピン留めできます。これにより、その後はアプリ内の特定のコンテンツに簡単にアクセスできます。

![セカンダリ タイルのスクリーン ショット](images/secondarytiles.png)

たとえば、ユーザーは、さまざまな特定の場所の天気をスタート メニューにピン留めできます。これにより、(1) ライブ タイルを使って、容易にひとめでわかる最新の天気の情報を表示でき、(2) 詳しく知りたい特定の都市の天気にすばやくアクセスできます。 ユーザーは、特定の株価情報、ニュース記事、およびその他の必要なアイテムもピン留めできます。

アプリにセカンダリ タイルを追加すると、すばやく効率的にユーザーとの再エンゲージメントを行えます。セカンダリ タイルによる容易なアクセスのため、より高い頻度でのアプリの再利用を促すことができます。

**セカンダリ タイルをピン留めできるのはユーザーだけです。アプリでプログラムによってユーザーの承認なくピン留めすることはできません**。 ユーザーは明示的にアプリ内から "ピン留め" ボタンをクリックする必要があります。これにより、API を使ってセカンダリ タイルの作成を要求します。システムによりダイアログ ボックスが表示され、ユーザーはタイルのピン留めの確認を求められます。

## <a name="quick-links"></a>クイック リンク

| 記事 | 説明 |
| --- | --- |
| [セカンダリ タイルのガイダンス](secondary-tiles-guidance.md) | セカンダリ タイルを使用する必要がある場合について説明します。 |
| [セカンダリ タイルをピン留めする](secondary-tiles-pinning.md) | セカンダリ タイルをピン留めする方法について説明します。 |
| [デスクトップ アプリケーションからピン留めする](secondary-tiles-desktop-pinning.md) | Windows デスクトップ アプリケーションでは、デスクトップ ブリッジを利用して、セカンダリ タイルをピン留めできます。 |


## <a name="secondary-tiles-in-relation-to-primary-tiles"></a>セカンダリ タイルとプライマリ タイルの関係

セカンダリ タイルは、1 つの親アプリに関連付けられます。 ユーザーは、セカンダリ タイルをスタート メニューにピン留めしておくことで、よく使う親アプリの領域を一貫した方法で効率的に直接起動できます。 対象となる領域は、頻繁に更新されるコンテンツを含んだ親アプリの総合サブセクションか、またはアプリの特定の領域へのディープ リンクです。

セカンダリ タイルを使うシナリオには次のようなものがあります。

* 天気予報アプリでの特定の都市の天気予報の更新情報
* カレンダー アプリでの予定イベントの要約
* ソーシャル アプリでの大事な連絡先のステータスと更新情報
* RSS リーダーでの特定のフィード
* 音楽の再生リスト
* ブログ

ユーザーが注目しているコンテンツが頻繁に変更される場合も、セカンダリ タイルが役立ちます。 セカンダリ タイルをピン留めすると、ユーザーはタイルから更新情報をひとめで確認できるようになるほか、タイルから親アプリを直接起動できます。

セカンダリ タイルは多くの点でプライマリ タイルに似ています。

* タイル通知を使用して、リッチ コンテンツを表示できます。
* 既定のタイル コンテンツ用の 150 x 150 ピクセルのロゴを入れる必要があります。
* オプションとして、より大きなタイル サイズを有効にするために他のロゴ サイズを含めることができます。
* 通知とバッジを表示できます。
* スタート メニューで並べ替えることができます。
* アプリがアンインストールされると自動的に削除されます。
* ロック画面に、バッジとロックの詳細情報テキストを表示できます。

一方で、セカンダリ タイルにはプライマリ タイルと大きく異なる点があります。

* ユーザーは親アプリを削除することなくいつでもセカンダリ タイルを削除できます。
* セカンダリ タイルは実行時に作成できます。 アプリ タイルはインストール時にしか作成できません。
* セカンダリ タイルを追加する前に、ポップアップでユーザーへの確認が行われます。
* プログラムでユーザーへの要求を使ってロック画面にセカンダリ タイルを選択することはできません。 セカンダリ タイルは、ユーザーが [PC 設定] の [パーソナル設定] ページを使って手動で追加する必要があります。

通知の送信のため、セカンダリ タイルで使われるタイル/バッジ アップデーターとプッシュ通知チャネル用に特定のメソッドが用意されています。 これらは、プライマリ タイルで使われるバージョンと似ています。 たとえば、CreateBadgeUpdaterForApplication に対して CreateBadgeUpdaterForSecondaryTile があります。


## <a name="guidance-on-secondary-tiles"></a>セカンダリ タイルのガイダンス
セカンダリ タイルを使う場合についての説明や、その他の使用のガイダンスについては、「[セカンダリ タイルのガイダンス](secondary-tiles-guidance.md)」をご覧ください。


## <a name="pinning-secondary-tiles"></a>セカンダリ タイルのピン留め
セカンダリ タイルをピン留めする方法については、「[セカンダリ タイルをピン留めする](secondary-tiles-pinning.md)」をご覧ください。


## <a name="desktop-applications-and-secondary-tiles"></a>デスクトップ アプリケーションとセカンダリ タイル
デスクトップ ブリッジを使って、デスクトップ アプリケーションからセカンダリ タイルを使用する方法については、「[デスクトップ アプリケーションからセカンダリ タイルをピン留めする](secondary-tiles-desktop-pinning.md)」をご覧ください。