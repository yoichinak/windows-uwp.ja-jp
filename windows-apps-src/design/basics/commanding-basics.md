---
description: Windows アプリのコマンド要素は、ユーザーがメール送信、項目の削除、フォームの送信などのアクションを実行できる対話型の UI 要素です。
title: Windows アプリのコマンド デザインの基本
ms.assetid: 1DB48285-07B7-4952-80EF-02B57D4469F2
label: Command design basics
template: detail.hbs
op-migration-status: ready
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 5b6bfac1c27a857b38c2995200590afd81576d0d
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2020
ms.locfileid: "93032455"
---
# <a name="command-design-basics-for-windows-apps"></a>Windows アプリのコマンド デザインの基本

Windows アプリの " *コマンド要素* " は、ユーザーがメール送信、項目の削除、フォームの送信などのアクションを実行できる対話型の UI 要素です。 " *コマンド インターフェイス* " は、共通のコマンド要素、それをホストするコマンド サーフェス、サポートされている対話、提供されているエクスペリエンスで構成されます。

## <a name="provide-the-best-command-experience"></a>最善のコマンド エクスペリエンスを提供する

コマンド インターフェイスの最も重要な側面は、ユーザーが実行できるようにすることです。 アプリの機能を計画するときは、それらのタスクを実現するために必要な手順と、有効にするユーザー エクスペリエンスを検討します。 これらのエクスペリエンスの最初のドラフトが完成した後は、それらを実装するためのツールと相互作用を決定できます。

一般的なコマンド エクスペリエンスを次に示します。

- 情報の送信または提出
- 設定とオプションの選択
- コンテンツの検索とフィルター処理
- ファイルを開く、保存する、削除する
- コンテンツの編集または作成

クリエイティブにコマンド エクスペリエンスを設計してください。 アプリでサポートする入力デバイスと、各デバイスに対するアプリでの対応方法を選択します。 幅広い機能と設定をサポートすることにより、アプリの使いやすさ、移植性、アクセシビリティを最大限に引き出せます (詳しくは、[Windows アプリ向けのコマンド デザイン](../controls-and-patterns/commanding.md)に関する記事をご覧ください)。



<!--
When designing a command interface, the most important decision is choosing what a user can do. To plan the right type of interactions, focus on your app - consider the user experiences you want to enable, and what steps users will need to take. Once you decide what you want users to accomplish, then you can provide them the tools to do so.
-->

## <a name="choose-the-right-command-elements"></a>適切なコマンド要素を選択する

適切な要素をコマンド インターフェイスで使うことが、直感的で使いやすいアプリとなるか、使いにくくてややこしいアプリとなるかの分かれ目になります。 Windows アプリでは、包括的なコマンド要素のセットを使用できます。 最も一般的な UWP のコマンド要素を次に示します。

:::row:::
    :::column:::
![ボタンの画像](images/commanding/thumbnail-button.svg)
    :::column-end:::
    :::column span="2":::
<b>ボタン</b>

<a href="../controls-and-patterns/buttons.md" style="text-decoration:none">ボタン</a>は、即時アクションをトリガーします。 メールの送信、フォーム データの送信、ダイアログでのアクションの確認などの操作です。
:::row-end:::

:::row:::
    :::column:::
![リストの画像](images/commanding/thumbnail-list.svg)
    :::column-end:::
    :::column span="2":::
<b>リスト</b>

<a href="../controls-and-patterns/lists.md" style="text-decoration:none">リスト</a>は、対話型のリストまたはグリッド内に項目を表示します。 通常、オプションや表示項目が多い場合に使用されます。 例として、ドロップダウン リスト、リスト ボックス、リスト ビューとグリッド ビューなどがあります。
:::row-end:::

:::row:::
    :::column:::
![選択コントロールの画像](images/commanding/thumbnail-selection.svg)
    :::column-end:::
    :::column span="2":::
<b>選択コントロール</b>

アンケートに入力するときや、アプリ設定を構成するときなどに、ユーザーがいくつかのオプションから選択できるようにします。 例として、<a href="../controls-and-patterns/checkbox.md">チェック ボックス</a>、<a href="../controls-and-patterns/radio-button.md">ラジオ ボタン</a>、<a href="../controls-and-patterns/toggles.md">トグル スイッチ</a>などがあります。
:::row-end:::

:::row:::
    :::column:::
![カレンダーの画像](images/commanding/thumbnail-calendar.svg)
    :::column-end:::
    :::column span="2":::
<b>カレンダー、日付、および時刻の選択コントロール</b>

<a href="../controls-and-patterns/date-and-time.md">カレンダー、日付、および時刻の選択コントロール</a>は、イベントを作成するときや、アラームを設定するときなどに、ユーザーが日時情報を表示して変更できるようにします。 例として、カレンダーの日付の選択コントロール、カレンダー ビュー、日付の選択コントロール、時刻の選択コントロールなどがあります。
:::row-end:::

:::row:::
    :::column:::
![テキスト予測入力の画像](images/commanding/thumbnail-autosuggest.svg)
    :::column-end:::
    :::column span="2":::
<b>テキスト予測入力</b>

データを入力したりクエリを実行したりするときに、ユーザーが入力するにつれて入力候補を表示します。 例として、<a href="../controls-and-patterns/auto-suggest-box.md">自動提案ボックス</a>などがあります。<br>
:::row-end:::

全一覧については、「[コントロールと UI 要素](../controls-and-patterns/index.md)」をご覧ください。

## <a name="place-commands-on-the-right-surface"></a>適切なサーフェスへのコマンドの配置

アプリのキャンバスや、コマンド バー、コマンド バー ポップアップ、メニュー バー、ダイアログといった特殊なコマンド コンテナーなど、アプリ内の多くのサーフェスに、コマンド要素を配置できます。

常に、コンテンツに対して作用するコマンドを介してではなく、コンテンツを直接ユーザーが操作できるようにします。たとえば、リストのアイテムの並べ替えは、上下のコマンド ボタンではなく、ドラッグ アンド ドロップで行えるようにします。 

ただし、特定の入力デバイスの場合、または特定のユーザー機能や設定に対応するときは、これが不可能なことがあります。 このような場合は、できるだけ多くのコマンド アフォーダンスを提供し、これらのコマンド要素をアプリのコマンド サーフェスに配置します。

最も一般的ないくつかのコマンド サーフェスの一覧を次に示します。

:::row:::
    :::column:::
![アプリのキャンバスの画像](images/commanding/thumbnail-canvas.svg)
    :::column-end:::
    :::column span="2":::
<b>アプリのキャンバス (コンテンツ領域)</b>

ユーザーがコア シナリオを完了するためにあるコマンドが常に必要な場合は、そのコマンドをキャンバスに配置できます。 コマンドは影響を与えるオブジェクトの近く (またはその上) に配置できるため、キャンバスにコマンドを配置すると使い方がわかりやすくなります。 ただし、キャンバスに配置するコマンドは慎重に選んでください。 アプリのキャンバスにコマンドが多すぎると、貴重な画面のスペースがなくなり、ユーザーを困惑させる可能性があります。 それほど頻繁に使わないコマンドの場合、別のコマンド サーフェスに配置することを検討してください。
:::row-end:::

:::row:::
    :::column:::
![コマンド バーの画像](images/commanding/thumbnail-commandbar.svg)
    :::column-end:::
    :::column span="2":::
<b>コマンド バーとメニュー バー</b>

<a href="../controls-and-patterns/app-bars.md">コマンド バー</a>を使うと、コマンドを整理しやすくなり、アクセスしやすくなります。 コマンド バーは画面の上部または画面の下部、あるいは画面の上部と下部の両方に配置できます (アプリの機能がコマンド バーに対して複雑すぎる場合は<a href="../controls-and-patterns/menus.md#create-a-menu-bar">メニュー バー</a>も使用できます)。
:::row-end:::

:::row:::
    :::column:::
![コンテキスト メニューの画像](images/commanding/thumbnail-contextmenu.svg)
    :::column-end:::
    :::column span="2":::
<b>メニューとショートカット メニュー</b>

<p>メニューとコンテキスト メニューは、コマンドを整理してユーザーに要求されるまで非表示にすることによって、スペースを節約します。 通常、ユーザーはボタンをクリックするか、コントロールを右クリックして、メニューまたはコンテキスト メニューにアクセスします。</p> 

<p><a href="../controls-and-patterns/command-bar-flyout.md">コマンド バーのポップアップ</a>は、コマンド バーとコンテキスト メニューの利点を単一のコントロールに結合するコンテキスト メニューの一種です。 これにより、よく使うアクションへのショートカットが提供され、クリップボードやカスタム コマンドなど、特定のコンテキストにのみ関連するセカンダリ コマンドにアクセスできます。</p>

<p>UWP には、従来のメニューとコンテキスト メニューのセットも用意されています。詳細については、<a href="../controls-and-patterns/menus.md">メニューとコンテキスト メニューの概要</a>に関する記事を参照してください。</p>
:::row-end:::

## <a name="provide-command-feedback"></a>コマンドのフィードバックを提供する 

コマンドのフィードバックでは、操作やコマンドが検出されたこと、コマンドがどのように解釈および処理されたか、コマンドが成功したかどうかを、ユーザーに伝えます。 これは、自分が実行したこと、そして次に実行できることを、ユーザーが理解するのに役立ちます。 フィードバックが UI に自然に統合されていて、ユーザーの介在が不要であるか、どうしても必要な場合以外は他の操作が不要であることが理想的です。

> [!NOTE]
> 必要なときにのみ、そして他の場所では得られない場合にだけ、フィードバックを提供します。 価値が加わる場合を除き、アプリケーションの UI は無駄がなく整然としたものに保ちます。

アプリでフィードバックを提供する方法をいくつか示します。

:::row:::
    :::column:::
![コマンド バー コンテンツ領域の画像](images/commanding/thumbnail-commandbar2.svg)
    :::column-end:::
    :::column span="2":::
<b>コマンド バー</b>

<a href="../controls-and-patterns/app-bars.md">コマンド バー</a>のコンテンツ領域は、ユーザーがフィードバックを確認したい場合にユーザーに状態を伝えるための直感的な場所です。
:::row-end:::

:::row:::
    :::column:::
![ポップアップの画像](images/commanding/thumbnail-flyout.svg)
    :::column-end:::
    :::column span="2":::
<b>ポップアップ</b>

       <a href="../controls-and-patterns/dialogs-and-flyouts/index.md">フライアウト</a>は、その外側をタップまたはクリックして閉じることができる、軽量な状況依存のポップアップです。
:::row-end:::

:::row:::
    :::column:::
![ダイアログの画像](images/commanding/thumbnail-dialog.svg)
    :::column-end:::
    :::column span="2":::
<b>ダイアログ コントロール</b>

<a href="../controls-and-patterns/dialogs-and-flyouts/index.md">ダイアログ コントロール</a>は、状況依存のアプリ情報を表示するモーダル UI オーバーレイです。 ほとんどの場合、ダイアログは明示的に閉じられまでアプリ ウィンドウの操作を妨げます。また、多くの場合、ユーザーに操作を要求します。 ダイアログは、煩わしく感じることがあるため、特定の状況でのみ使用してください。 詳しくは、「[アクションを確認または元に戻すタイミング](#when-to-confirm-or-undo-actions)」をご覧ください。
    :::column-end:::
:::row-end:::

> [!TIP]
> アプリで使う確認ダイアログの量に注意してください。ユーザーが間違えたときはとても役に立ちますが、ユーザーが意図的にアクションを実行しようとしているときは邪魔になります。

### <a name="when-to-confirm-or-undo-actions"></a>アクションを確認または元に戻すタイミング

アプリケーションの UI がどれほど適切に設計されていたとしても、すべてのユーザーが望んだとおりにアクションを実行できることはありません。 アクションの確認を求めたり、最近のアクションを元に戻す方法を用意したりすることにより、アプリでこのような状況に対処できます。

:::row:::
    :::column:::
![推奨の画像](images/do.svg)

元に戻すことができず、実行結果が重大な操作の場合は、確認ダイアログ ボックスの使用をお勧めします。 このような操作の例は、次のとおりです。
-   ファイルを上書きする
-   ファイルを保存せずに終了する
-   ファイルやデータを完全に削除することを確認する
-   購入する (確認メッセージを表示しないことをユーザーが選択した場合を除く)
-   何かへのサインアップなどのフォームを送信する
    :::column-end:::
    :::column:::
![推奨の画像](images/do.svg)

元に戻すことができる操作の場合は、通常、単純な "元に戻す" コマンドを提供すれば十分です。 このような操作の例は、次のとおりです。
-   ファイルを削除する
-   メールを削除する (完全には削除しない)
-   コンテンツを変更する、またはテキストを編集する
ファイル名を変更する
:::row-end:::

##  <a name="optimize-for-specific-input-types"></a>特定の入力タイプの最適化

特定の入力の種類やデバイスを中心としたユーザー エクスペリエンスの最適化について詳しくは、「[操作の基本情報](../input/index.md)」をご覧ください。

