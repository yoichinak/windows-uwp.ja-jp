---
title: iOS、Android、Windows 10 のプラットフォーム機能を比較します。
description: IOS、Android、および Windows 10 でのユニバーサル Windows プラットフォーム (UWP) 間の開発概念とプラットフォーム機能の詳細な比較をご覧ください。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 082736c8-2ac3-41b3-b246-e705edc23f34
ms.localizationpriority: medium
ms.openlocfilehash: 21cb4c105cc4c95c3a14c4c5bd0049265682f91c
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/24/2020
ms.locfileid: "91220585"
---
# <a name="windows-apps-concept-mapping-for-android-and-ios-developers"></a>Android と iOS 開発者向けの Windows アプリ概念マッピング

このリソースには、Android や iOS のスキルとコードを持つ開発者が Windows 10 とユニバーサル Windows プラットフォーム (UWP) に移行する場合に、それら 3 つのプラットフォーム間でプラットフォームの機能と知識を関連付けるために必要なすべての情報が含まれています。

[iOS から UWP への移行](ios-to-uwp-root.md) の移植に関するコンテンツもご覧ください。 このドキュメントは、[ダウンロード](https://www.microsoft.com/download/details.aspx?id=52041)することもできます。

## <a name="user-interface-ui"></a>ユーザー インターフェイス (UI)


<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>一般的な概念</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>デザイン言語。</strong><br><br>プラットフォーム上のアプリの外観と動作方法を規定する規則のセット。</td>
<td align="left"><strong>Android のマテリアル設計</strong> ガイドラインでは、android のデザイナーや開発者が従うための視覚的な言語を提供します。</td>
<td align="left"><strong>ヒューマンインターフェイスのガイドライン</strong> では、iOS の設計者や開発者向けのアドバイスを提供しています。</td>
<td align="left">「<a href="https://developer.microsoft.com/windows/apps/design"><strong>UWP アプリのデザイン</strong></a>」で、すべての Windows 10 デバイスで優れた外観を持つアプリを作成する方法について説明しています。 ユーザー インターフェイス (UI) デザインの基本、レスポンシブ デザイン テクニック、詳細なガイドラインの完全な一覧が示されています。<br/></td>
</tr>
<tr class="even">
<td align="left"><strong>ユーザーインターフェイスマークアップ言語。</strong> <br><br>UI とそのコンポーネントをレンダリングし、記述するマークアップ言語。 プラットフォームごとに、ビジュアル編集とマークアップ編集のためのエディターが提供されています。<br/></td>
<td align="left"><strong>XML レイアウト</strong>。<strong>Android Studio</strong> または <strong>Eclipse</strong> を使って編集します。</td>
<td align="left"><strong>XIB</strong> と <strong>ストーリーボード</strong> は、Xcode 内の <strong>Interface Builder</strong> を使用して編集されます。</td>
<td align="left"><strong><a href="/windows/uwp/xaml-platform/xaml-overview">XAML</a></strong>。<strong><a href="https://visualstudio.microsoft.com/">Microsoft Visual Studio</a></strong> と <strong><a href="/visualstudio/designers/creating-a-ui-by-using-blend-for-visual-studio?view=vs-2015">Blend for Visual Studio</a></strong> を使って編集します。<br/><br/><a href="/windows/uwp/xaml-platform/index">XAML プラットフォーム</a><br/><br/><a href="/windows/uwp/design/basics/xaml-basics-ui">XAML を使った UI の作成</a><br/><br/><a href="/windows/uwp/layout/layouts-with-xaml">XAML を使ったレイアウトの定義</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>組み込みのユーザーインターフェイスコントロール。</strong> <br><br>ボタン、リスト コントロール、テキスト コントロールなど、プラットフォームで提供される、再利用可能な UI 要素。</td>
<td align="left">ウィジェット、レイアウト、テキスト フィールド、コンテナー、日付/時刻コントロール、専門的なコントロールとして参照される、作成済みの<strong>ビュー</strong>と<strong>ビュー グループ</strong>のクラス。</td>
<td align="left">Xcode オブジェクトライブラリにあり、UIKit ユーザーインターフェイスカタログに表示されている<strong>ビュー</strong>および<strong>コントロール</strong>。 ビューには、イメージ ビュー、ピッカー ビュー、スクロール ビューが含まれます。 コントロールには、ボタン、日付選択コントロール、テキスト フィールドが含まれます。</td>
<td align="left">XAML プラットフォームでは、ボタン、リスト コントロール、パネル、テキスト コントロール、コマンド バー、ファイル ピッカー、メディア、インク入力など、さまざまな<strong>ビルトイン コントロール</strong>が提供されます。<br/><br/><a href="/windows/uwp/controls-and-patterns/controls-and-events-intro">コントロールを追加し、イベントを処理する</a></td>
</tr>
<tr class="even">
<td align="left"><strong>イベント処理を制御します。</strong> <br><br>UI コントロール内でイベントがトリガーされるときに実行されるロジックを定義します。</td>
<td align="left"><strong>イベントハンドラー</strong> と <strong>イベントリスナー</strong> は、XML またはプログラムによって追加されます。</td>
<td align="left">コントロールから<strong>ターゲット</strong>へ<strong>操作</strong>メッセージが送信されます。</td>
<td align="left">XAML ページに接続されている<strong>コード ビハインド ファイル</strong>に、XAML コントロールのイベントを処理するメソッドを定義できます。 <strong>イベント ハンドラー</strong>は常に、コードで記述します。 ただし、XAML マークアップまたはコードでそれらのハンドラーをイベントにフックすることができます。<br/><br/><a href="/windows/uwp/controls-and-patterns/controls-and-events-intro">コントロールを追加し、イベントを処理する</a><br/><br/><a href="/windows/uwp/xaml-platform/events-and-routed-events-overview">イベントとルーティング イベントの概要</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>データバインディング。</strong> <br><br>アプリの UI でデータを表示し、必要に応じてそのデータと同じ状態を保つことができるソフトウェアの設計パターンです。</td>
<td align="left"><strong>データ バインディング ライブラリ</strong>が提供されています。ただし、まだベータ版です。</td>
<td align="left">iOS では、組み込みのバインディング システムは存在しません。 <strong>キー値の観察</strong> は、サードパーティのライブラリを使用するか、追加のコードを記述することで、データバインディングを実行するために構築できます。 コントロールは、データを取得するためにデリゲート/コールバックの方法を使います。</td>
<td align="left">UWP プラットフォームが<strong>データ バインディング</strong>を処理します。 <strong><a href="/windows/uwp/xaml-platform/x-bind-markup-extension">{x:Bind}</a></strong> マークアップ拡張を使ってバインディングのパフォーマンスを向上させるか、<strong><a href="/windows/uwp/xaml-platform/binding-markup-extension">{Binding}</a></strong> を使ってより多くの機能を利用できます。 その後は、プラットフォームで<strong>一方向バインディング</strong>を使ってデータ ソースからの値を UI に表示するか、<strong>双方向バインディング</strong>で値の監視も行い、値が変わったら UI を更新するか、バインディングの構成方法を選ぶだけです。<br/><br/><a href="/windows/uwp/data-binding/index">データ バインディング</a></td>
</tr>
<tr class="even">
<td align="left"><strong>UI オートメーション。</strong> <br><br>プログラムによる UI 要素へのアクセスで、アプリをアクセシビリティ対応にして支援技術製品を利用できるようにして、自動テスト スクリプトで UI を操作できるようにします。</td>
<td align="left"><strong>テキストのラベル</strong>、<strong>contentDescription</strong>、<strong>ヒント</strong>の値を使って、オートメーションで UI 要素が見つかるようにします。 Android Studio で <strong>UI Automator</strong> と <strong>Espresso</strong> テスト フレームワークを使って UI のテストを作成できます。</td>
<td align="left"><strong>Automation インストルメント</strong>で、<strong>アクセシビリティ</strong>設定や<strong>要素階層</strong>内の要素の位置を使って要素を識別する、自動化された UI テスト スクリプトを記述できます。</td>
<td align="left"><strong><a href="/windows/desktop/WinAuto/uiauto-uiautomationoverview">Ui オートメーション</a></strong>によってすぐに使用できる組み込みの ui 要素にプログラムでアクセスできるようになります。<br/><strong><a href="/windows/uwp/accessibility/custom-automation-peers">カスタム オートメーション ピア</a></strong>で、独自のカスタム UI クラスのオートメーション サポートを提供できます。 Visual Studio の<strong><a href="/visualstudio/test/use-ui-automation-to-test-your-code?view=vs-2015">コード化された UI テスト プロジェクト</a></strong>で、UI を使ってアプリケーション全体を自動的にテストするか、UI を単独でテストできます。</td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>コントロールの外観を変更する。</strong> <br><br>サイズ、色、その他の属性を編集します。</td>
<td align="left">コントロールには<strong>プロパティ</strong>があり、デザイナー ツールを使って XML マークアップまたはプログラムで編集できます。</td>
<td align="left">コントロールには<strong>属性</strong>があり、<strong>属性インスペクター</strong>を使って Interface Builder またはプログラムで編集できます。</td>
<td align="left">Visual Studio と Blend for Visual Studio を使って、XAML マークアップまたはプログラムでコントロールの<strong>プロパティ</strong>を編集できます。<br/><br/><a href="/windows/uwp/controls-and-patterns/controls-and-events-intro">コントロールを追加し、イベントを処理する</a></td>
</tr>
<tr class="even">
<td align="left"><strong>再利用可能な視覚スタイル。</strong> <br><br>視覚的な変更を再利用可能な形式でいくつかのコントロールに適用します。</td>
<td align="left"><strong>XML スタイル</strong>は 1 つまたは複数のコントロールに適用されるプロパティのセットです。</td>
<td align="left">iOS では、既定では再利用可能な視覚スタイルをサポートしていませんが、UIAppearance プロトコルを使って複数のコントロールで共通の属性を共有できます。</td>
<td align="left">再利用を容易にするために、再利用可能な <strong><a href="/uwp/api/Windows.UI.Xaml.Style">スタイル</a></strong>を作成して、複数のコントロールに適用し、 <strong><a href="/uwp/api/Windows.UI.Xaml.ResourceDictionary">ResourceDictionary</a></strong> に格納することができます。<br/><br/><a href="/previous-versions/windows/apps/hh465381(v=win.10)">クイック スタート: コントロールのスタイル</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>コントロールの視覚的な構造を編集する。</strong> <br><br>プロパティまたは属性を変更するだけでなく、コントロールのビジュアル構造をカスタマイズします。たとえば、チェックボックスの下のテキストを移動できます。</td>
<td align="left">Android では、コントロールの視覚的構造を編集する簡単な方法は存在しません。</td>
<td align="left">iOS では、コントロールの視覚的構造を編集する簡単な方法は存在しません。</td>
<td align="left">コントロールのビジュアル構造をカスタマイズするには、XAML マークアップで <strong><a href="/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate">コントロールテンプレート</a></strong> をコピーして編集します。<br/><br/><a href="/previous-versions/windows/apps/hh465374(v=win.10)">クイックスタート: コントロールテンプレート</a></td>
</tr>
<tr class="even">
<td align="left"><strong>組み込みのタッチジェスチャ。</strong> <br><br>ビューやコントロールでタップやダブルタップなどの高レベルの抽象化されたジェスチャ イベントを処理して、カスタマイズされたタッチをサポートします。</td>
<td align="left"><strong>ジェスチャ ディテクター</strong>が、スクロール、長押し、タップ、ダブルタップ、フリックなどの一般的なタッチ ジェスチャを検出します。</td>
<td align="left">UIKit フレームワークで提供される組み込みの<strong>ジェスチャ認識エンジン</strong>が、タップ、ピンチ、パン、スワイプ、回転、長押しなどのタッチ ジェスチャを検出します。</td>
<td align="left"><strong>UI 要素</strong> を使用すると、タップ、ダブルタップ、右タップして保持するなどの <strong>静的なジェスチャイベント</strong> や、スライド、スワイプ、ターン、ピンチ、伸縮などの <strong>操作ジェスチャイベント</strong> を処理できます。 ジェスチャ イベントは<strong>ルーティング イベント</strong>であり、UIElement の子が含まれている親オブジェクトを使って処理できます。<br/><br/><a href="/windows/uwp/input-and-devices/touch-interactions">タッチ操作</a><br/><br/><a href="/windows/uwp/design/layout/index">カスタム ユーザー操作 - ジェスチャ、操作、対話式操作</a></td>
</tr>
</tbody>
</table>
<h2 id="navigation-and-app-structure">ナビゲーションとアプリの構造</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>一般的な概念</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>レイアウト.</strong> <br><br>レイアウトでは、ユーザー インターフェイスの構造を定義します。</td>
<td align="left">レイアウトは、他のビュー グループやビューを入れ子にできる <strong>LinearLayout</strong> や <strong>RelativeLayout</strong> などの<strong>ビュー グループ</strong>で構成されます。</td>
<td align="left">レイアウトは、入れ子にできる <strong>UIView</strong> が含まれる <strong>UIViewController</strong> で構成されます。</td>
<td align="left">XAML が、静的レイアウトやレスポンシブ レイアウト用の <strong><a href="/uwp/api/windows.ui.xaml.controls.canvas">Canvas</a></strong>、<strong><a href="/uwp/api/windows.ui.xaml.controls.grid">Grid</a></strong>、<strong><a href="/uwp/api/windows.ui.xaml.controls.relativepanel">RelativePanel</a></strong>、<strong><a href="/uwp/api/windows.ui.xaml.controls.stackpanel">StackPanel</a></strong> などの<strong>レイアウト パネル クラス</strong>から成る柔軟なレイアウト システムを提供します。 <strong><a href="/visualstudio/ide/reference/properties-window?view=vs-2015">プロパティ</a></strong> は、要素のサイズと位置を制御するために使用されます。<br/><br/><a href="/windows/uwp/layout/layouts-with-xaml">XAML を使用したレイアウトの定義</a><br/></td>
</tr>
<tr class="even">
<td align="left"><strong>ピアナビゲーション。</strong> <br><br>階層の重要度が同じページ間を移動する方法をユーザーに提供します。</td>
<td align="left"><strong>タブ</strong>、 <strong>スワイプビュー</strong> 、および <strong>ナビゲーション引き出し</strong> は横方向の <strong>ナビゲーション</strong>を提供します。</td>
<td align="left"><strong>タブ バー コントローラー</strong>、<strong>分割ビュー コントローラー</strong>、<strong>ページ ビュー コントローラー</strong>で同じ階層のビューの間を移動できます。</td>
<td align="left"><strong><a href="/windows/uwp/controls-and-patterns/tabs-pivot">タブ/ピボット</a></strong>を使って、コンテンツの上にあるリンクやタブの永続的な一覧を表示できます。 <strong><a href="/windows/uwp/controls-and-patterns/split-view">ナビゲーションウィンドウ/分割ビュー</a></strong>を使用すると、コンテンツと共にリンクの一覧を表示できます。<br/><br/><a href="/windows/uwp/layout/navigation-basics">ナビゲーション</a><br/><br/><a href="/windows/uwp/layout/navigate-between-two-pages">2 つのページ間の移動</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>階層ナビゲーション。</strong> <br><br>階層の親と子のページ間を移動します。</td>
<td align="left">他の<strong>アクティビティ</strong>を読み込む<strong>インテント</strong>と一緒に<strong>リスト</strong>、<strong>グリッド リスト</strong>、<strong>ボタン</strong>などのコントロールを使うと、<strong>子孫のナビゲーション</strong>ができます。</td>
<td align="left"><strong>ナビゲーションコントローラー</strong> を使用すると、ユーザーは階層のレベル間を移動できます。</td>
<td align="left"><strong><a href="/windows/uwp/controls-and-patterns/hub">ハブ</a></strong> を使用すると、ユーザーにコンテンツのプレビューを表示できます。これを選択すると、子ページに移動できます。 <strong><a href="/windows/uwp/controls-and-patterns/master-details">マスター/詳細</a></strong>を使うと、ユーザーは対応する [詳細] セクションの横に表示される項目の概要の一覧から項目を選ぶことができます。<br/><br/><a href="/windows/uwp/layout/navigation-basics">ナビゲーション</a><br/><br/><a href="/windows/uwp/layout/navigate-between-two-pages">2 つのページ間の移動</a></td>
</tr>
<tr class="even">
<td align="left"><strong>戻るボタンのナビゲーション。</strong> <br><br>アプリケーション内で元の画面に戻ります。</td>
<td align="left">アクション バーの <strong>[戻る]</strong> ボタンと <strong>[上へ]</strong> ボタンで、<strong>バック スタック</strong>を使って<strong>先祖</strong>ナビゲーションと<strong>一時的な</strong>ナビゲーションができます。</td>
<td align="left"><strong>ナビゲーション コントローラー</strong>に [戻る] ボタンを追加することができます。<br/></td>
<td align="left">[ <strong><a href="/uwp/api/windows.ui.xaml.controls.frame.backstack">バックスタック] プロパティ</a></strong> を使用して、ユーザーが <strong>ナビゲーション履歴</strong>を走査できるようにすることで、ソフトウェアまたはハードウェアの戻るボタンの押下を簡単に処理できます。<br/><br/><a href="/windows/uwp/layout/navigation-history-and-backwards-navigation">[戻る] ボタンによるナビゲーション</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>スプラッシュスクリーン。</strong> <br><br>アプリ起動時にイメージを表示します。主にブランディング目的です。</td>
<td align="left">スプラッシュ画面は既定では指定されていません。最初のアクティビティの<strong>テーマの背景</strong>を編集すると実装されます。</td>
<td align="left"><strong>静的起動イメージ</strong>または<strong>XIB/ストーリーボードの起動ファイル</strong>がアプリに必要です。</td>
<td align="left"><strong>イメージ</strong>とカラーの背景を使ってスプラッシュ画面を作成できます。 <a href="/windows/uwp/launch-resume/create-a-customized-splash-screen">スプラッシュスクリーンの時間は延長でき</a>ます。<br/><br/><a href="/windows/uwp/launch-resume/add-a-splash-screen">スプラッシュ画面の追加</a></td>
</tr>
</tbody>
</table>
<h2 id="custom-inputs">カスタム入力</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>一般的な概念</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>ナレーション.</strong> <br><br>音声入力に対する音声認識と、その他の音声機能。</td>
<td align="left"><strong>Google 音声検索</strong>などの <strong>RecognizerIntent</strong> を実装するアプリで音声入力を提供できます。 <strong>SpeechRecognizer</strong> クラスを使うと、アプリで Google の音声認識 API を使うことができます。</td>
<td align="left">アプリでは、<strong>SFSpeechRecognizer</strong> クラスを使用して音声入力と音声認識を実装することができます。</td>
<td align="left"><strong><a href="/windows/uwp/input-and-devices/speech-recognition">音声認識</a></strong>API を使用して、フォアグラウンドでアプリと対話することができます。 音声ベースの <strong><a href="/windows/uwp/input-and-devices/cortana-interactions">Cortana 対話</a></strong> 機能を使用して、フォアグラウンドまたはバックグラウンドでアプリを起動したり、バックグラウンドアプリと対話したりすることができます。<br/><br/><a href="/windows/uwp/input-and-devices/speech-interactions">音声操作</a></td>
</tr>
<tr class="even">
<td align="left"><strong>カスタムユーザー入力。</strong> <br><br>キーボード、マウス、スタイラスなどの入力を処理します。</td>
<td align="left"><strong>タッチ</strong>、<strong>タッチパッド</strong>、<strong>スタイラス</strong>、<strong>マウス</strong>、<strong>キーボード</strong>の操作がサポートされています。 移動と入力はタッチ操作と同じ方法で報告されますが、<strong>入力デバイス</strong>に関する詳しい情報を検出することができます。</td>
<td align="left"><strong>タッチ</strong>、<strong>Apple Pencil</strong>、ハードウェア <strong>キーボード</strong>がサポートされています。</td>
<td align="left"><strong><a href="/windows/uwp/input-and-devices/touch-interactions">タッチ</a></strong>、<strong><a href="/windows/uwp/input-and-devices/touchpad-interactions">タッチパッド</a></strong>、<strong><a href="/windows/uwp/input-and-devices/pen-and-stylus-interactions">ペン/スタイラス</a></strong>、デジタルインク、<strong><a href="/windows/uwp/input-and-devices/mouse-interactions">マウス</a></strong>、<strong><a href="/windows/uwp/input-and-devices/keyboard-interactions">キーボード</a></strong>などのさまざまなやり取りがサポートされています。 どの入力デバイスが使われたかわからなくても、アプリがデータを処理し、必要に応じて未加工入力デバイス データにアクセスできます。<br/><br/><a href="/windows/uwp/input-and-devices/handle-pointer-input">ポインター入力の処理</a><br/><br/><a href="/windows/uwp/design/layout/index">カスタム ユーザー操作</a></td>
</tr>
</tbody>
</table>
<h2 id="data">Data</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>一般的な概念</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>ローカルアプリデータ。</strong> <br><br>アプリに関連する設定とファイルをローカルに保存します。</td>
<td align="left"><strong>openFileOutput</strong> と <strong>openFileInput</strong> を使ってローカル ファイルを保存することができます。 <strong>getSharedPreferences</strong> を使って<strong>共有環境設定ファイル</strong>の設定にアクセスできます。</td>
<td align="left"><strong>NSFileManager</strong> クラスを使ってアクセスする<strong>アプリケーション サポート</strong> ディレクトリにローカル ファイルを保存できます。 <strong>NSUserDefaults</strong> クラスを使って<strong>環境設定</strong>ファイルの設定にアクセスできます。</td>
<td align="left"><strong><a href="/uwp/api/Windows.Storage">Windows のストレージ</a></strong>クラスは、統合された方法でローカルデータストレージを処理します。 設定は <strong><a href="/uwp/api/Windows.Storage.ApplicationDataContainer">windows.storage.applicationdatacontainer</a></strong> オブジェクトとして保存され、 <strong><a href="/uwp/api/windows.storage.applicationdata.localsettings">LocalSettings</a></strong> プロパティを介してアクセスされます。 <strong><a href="/uwp/api/windows.storage.applicationdata.localfolder">ApplicationData.LocalFolder</a></strong> プロパティを使ってアクセスする <strong><a href="/uwp/api/windows.storage.storagefolder">StorageFolder</a></strong> オブジェクトにファイルを保存します。<br/><br/><a href="/windows/uwp/app-settings/store-and-retrieve-app-data">設定と他のアプリ データを保存して取得する</a></td>
</tr>
<tr class="even">
<td align="left"><strong>ローカルデータベースストレージ。</strong> <br><br>リレーショナル データベースにアプリ データを保存します。該当する場合はオブジェクト リレーショナル マッパー (ORM) も一緒に保存します。</td>
<td align="left"><strong>SQLite</strong> データベースが提供されます。 ORM は組み込みではありません。 <strong>SQLiteDatabase</strong> クラスを使って SQL のクエリが実行されます。</td>
<td align="left"><strong>SQLite</strong> データベースが提供されます。 <strong>Coredata</strong> は、SQLite と共に使用できる組み込みのオブジェクトグラフフレームワークであり、ORM と同等の機能を提供します。</td>
<td align="left"><strong>SQLite</strong> を使ってデータを保存することができます。 <strong><a href="/windows/uwp/data-access/entity-framework-7-with-sqlite-for-csharp-apps">Entity Framework</a></strong> は、大量のデータアクセスコードを記述する必要がなくなり、SQL を記述しなくてもデータベースを簡単に照会できるようにする組み込みの ORM です。 <a href="/windows/uwp/data-access/sqlite-databases">SQLite ライブラリ</a> で直接、SQL のクエリを実行できます。<br/><br/><a href="/windows/uwp/data-access/index">データ アクセス</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>REST アクセス用の HTTP ライブラリ。</strong> <br><br>HTTP(S) を使って Web サービスや Web サーバーと通信できる組み込みのライブラリ。<br/></td>
<td align="left">HTTP ライブラリ <strong>HttpURLConnection</strong> と <strong>Volley</strong>。</td>
<td align="left"><strong>NSURLSession</strong>、<strong>NSURLConnection</strong>、<strong>NSURLDownload</strong>。</td>
<td align="left">組み込みの <strong><a href="/uwp/api/Windows.Web.Http.HttpClient">Httpclient</a></strong> API を使用して、GET、DELETE、PUT、POST、common authentication PATTERNS、SSL、cookies、progress info などの一般的な HTTP 機能にアクセスできます。</td>
</tr>
<tr class="even">
<td align="left"><strong>クラウドバックアップサービス。</strong> <br><br>プラットフォームで提供される、アプリのデータ バックアップ サービス。</td>
<td align="left">Android の<strong>バックアップ マネージャー</strong>が Google の <strong>Android バックアップ サービス</strong>でアプリケーション データのバックアップを処理します。</td>
<td align="left"><strong>ICloud バックアップ</strong> は、ユーザーがアプリデータを含むバックアップを処理するように構成できます。 iCloud と互換性のある<strong>コア データ</strong>、<strong>iCloud のキー値ストア</strong>、<strong>iCloud ドキュメント ストレージ</strong>を使うアプリ。</td>
<td align="left">ローミング <strong><a href="/uwp/api/windows.storage.applicationdata">Applicationdata api</a></strong> ( <strong><a href="/uwp/api/windows.storage.applicationdata.roamingfolder">RoamingFolder</a></strong> と <a href="/uwp/api/windows.storage.applicationdata.roamingsettings"><strong>RoamingSettings</strong></a>を含む) を使用して保存したすべてのアプリデータは、クラウドとユーザーの他のデバイスに自動的に同期されます。 最新に保つ処理は、ユーザーの Microsoft アカウントで実行されます。<br/><br/><a href="/windows/uwp/design/app-settings/store-and-retrieve-app-data">アプリのデータのローミングのガイドライン</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>HTTP ファイルのダウンロード。</strong> <br><br>大小のファイルを HTTP 経由でダウンロードします。</td>
<td align="left"><strong>Urlconnection</strong> と <strong>httの LCONNECTION</strong> は、HTTP および FTP 経由でのダウンロードに使用されます。システム <strong>ダウンロードマネージャー</strong> を使用して、バックグラウンドでダウンロードすることもできます。</td>
<td align="left">HTTP および FTP 経由でファイルをダウンロードするには、 <strong>Nの Lsession</strong>と<strong>Nの lconnection</strong>を使用します。</td>
<td align="left"><strong><a href="/uwp/api/windows.networking.backgroundtransfer">バックグラウンド転送 API</a></strong>を使用すると、HTTP (S) および FTP 経由でファイルを確実に転送できます。その際、アプリの中断、接続の切断、接続とバッテリの寿命に基づく調整が行われます。 また、より小さなファイルに最適な <strong><a href="/uwp/api/windows.web.http.httpclient">Httpclient</a></strong> を使用することもできます。<br/><br/><a href="/windows/uwp/networking/which-networking-technology">アプリに適したネットワーク テクノロジ</a><br/><br/><a href="/windows/uwp/networking/background-transfers">バックグラウンド転送</a></td>
</tr>
<tr class="even">
<td align="left"><strong>P.</strong> <br><br>独自のプロトコルを使って他のデバイスと通信するために、低レベルの UDP データグラムおよび TCP ソケットを作成します。</td>
<td align="left"><strong>Socket</strong> クラスは TCP ソケットを提供し、 <strong>DATAGRAMSOCKET</strong> クラスは UDP ソケットを提供します。</td>
<td align="left"><strong>Nsstream</strong> と <strong>CFSTREAM</strong> は TCP ソケットを提供し、 <strong>cfstream</strong> は UDP ソケットを提供します。</td>
<td align="left"><strong><a href="/uwp/api/Windows.Networking.Sockets.DatagramSocket">DatagramSocket</a></strong> クラスを使って UDP データグラム ソケットで通信し、<strong><a href="/uwp/api/Windows.Networking.Sockets.StreamSocket">StreamSocket</a></strong> クラスを使って TCP または Bluetooth RFCOMM 経由で通信できます。<br/><br/><a href="/windows/uwp/networking/networking-basics">ネットワークの基本</a><br/><br/><a href="/windows/uwp/networking/which-networking-technology">アプリに適したネットワーク テクノロジ</a><br/><br/><a href="/windows/uwp/networking/sockets">ソケットの概要</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Websocket.</strong> <br><br>クライアントとサーバーの間の双方向通信を実現し、リアルタイムにデータを転送できるようにします。</td>
<td align="left">Android では、組み込みの WebSocket ライブラリは存在しません。</td>
<td align="left">iOS では、組み込みの WebSocket ライブラリは存在しません。</td>
<td align="left">Websocket をサポートするサーバーへのセキュリティで保護された接続は、 <strong><a href="/uwp/api/windows.networking.sockets.messagewebsocket">Messagewebsocket</a></strong> クラスを使用して、受信通知を含む小さなメッセージと、セクションで読み取ることができる大規模なバイナリファイル転送の <strong><a href="/uwp/api/windows.networking.sockets.streamwebsocket">streamwebsocket</a></strong> で作成できます。<br/><br/><a href="/windows/uwp/networking/networking-basics">ネットワークの基本</a><br/><br/><a href="/windows/uwp/networking/which-networking-technology">アプリに適したネットワーク テクノロジ</a><br/><br/><a href="/windows/uwp/networking/websockets">WebSocket の概要</a></td>
</tr>
<tr class="even">
<td align="left"><strong>OAuth ライブラリ。</strong> <br><br>OAuth のライブラリがサード パーティの OAuth プロバイダーと、プラットフォームに組み込まれているすべてのアカウント管理にアクセスできるようにします。</td>
<td align="left">汎用的な OAuth ライブラリは用意されていません。 Google Play サービスの OAuth 認証用に <strong>GoogleAuthUtil</strong> クラスが提供されています。<br/></td>
<td align="left">汎用的な OAuth ライブラリは用意されていません。 <strong>アカウント フレームワーク</strong>が、デバイスに既に保存されている Facebook や Twitter などのユーザー アカウントにアクセスできるようにします。</td>
<td align="left">汎用 OAuth ライブラリ <strong><a href="/windows/uwp/security/web-authentication-broker">Web authentication broker</a></strong> を使用すると、サードパーティの id プロバイダーサービスに接続できます。 <strong><a href="/windows/uwp/security/credential-locker">資格情報保管ボックス</a></strong>を使うと、ユーザーはログインを保存して複数のデバイスで使うことができます。 <strong><a href="/previous-versions/office/developer/onedrive-live-sdk-reference/dn896755(v=office.15)">Microsoft.live</a></strong> 名前空間を使うと、Live SDK の OAuth に簡単にアクセスして Microsoft サービスにアクセスできます。<br/><br/><a href="/windows/uwp/security/authentication-and-user-identity">認証とユーザー ID</a><br/><br/><a href="/uwp/api/windows.security.authentication.web">Windows.Security.Authentication.Web API ドキュメント</a><br/><br/><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/WebAuthenticationBroker">WebAuthenticationBroker のコード例</a></td>
</tr>
</tbody>
</table>
<h2 id="tooling">ツール</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>一般的な概念</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Netbeans.</strong> <br><br>アプリの作成に使うツールセット。</td>
<td align="left"><strong>Android Studio</strong> と <strong>Eclipse</strong>は、Android Studio の使用に向けて Google が開発者にプッシュします。</td>
<td align="left"><strong>Xcode</strong></td>
<td align="left"><strong><a href="https://visualstudio.microsoft.com/features/universal-windows-platform-vs">Visual Studio</a></strong> と <strong><a href="/visualstudio/designers/creating-a-ui-by-using-blend-for-visual-studio?view=vs-2015">Blend for Visual Studio</a></strong> には、UWP アプリをコーディング、設計、接続、デバッグ、分析、最適化、およびテストするために必要なすべてのツールが用意されています。 Visual Studio には、Windows 10 デバイス用の <strong><a href="/windows/uwp/debug-test-perf/test-with-the-emulator">エミュレーター</a></strong> も用意されているので、エミュレートされたデバイスの範囲を越えてアプリをテストすることができます。<br/><br/><a href="https://developer.microsoft.com/windows/downloads">UWP 用のダウンロードとツール</a></td>
</tr>
<tr class="even">
<td align="left"><strong>コード編成。</strong> <br><br>初期テンプレートから作成されることが多い、アプリの基本的なフォルダー構造。</td>
<td align="left"><strong>Androidmanifest</strong> ファイル、ソースファイルを含む <strong>java</strong> フォルダー、レイアウトと値を含むリソースを含む <strong>res</strong> フォルダー、Android Studio でのビルドスクリプトの <strong>Gradle</strong> 、Eclipse での <strong>Ant</strong> ビルドスクリプト。</td>
<td align="left">ソース ファイルと<strong>サポート ファイル</strong>、<strong>Info.plist</strong> ファイル、<strong>Main.storyboard</strong>、<strong>LaunchScreen.storyboard</strong>。 イメージは<strong>アセット ライブラリ</strong>に格納されます。</td>
<td align="left">UWP アプリには、Example.xaml や Example.xaml.cs というアプリ用の XAML とコード ファイル、<strong>Assets フォルダー</strong>のさまざまな画像、<strong>MainPage.xaml</strong> や <strong>MainPage.xaml.cs</strong> などのスタート ページ、マニフェストが含まれています。<br/><br/><a href="/windows/uwp/get-started/create-a-hello-world-app-xaml-universal">Hello world アプリを作成する</a></td>
</tr>
</tbody>
</table>
<h2 id="app-lifecycle">アプリのライフサイクル</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>一般的な概念</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>アプリのライフサイクル。</strong> <br><br>アプリの起動時、中断時、再開時、終了時にイベントを処理し、アプリケーションの状態を保存/復元したり他のタスクを実行したりする機会を提供します。</td>
<td align="left">アクティビティのそれぞれに、<strong>再開</strong>などの状態を持つ独自の<strong>アクティビティ ライフサイクル</strong>があります。 <strong>Onresume</strong>などの<strong>ライフサイクルコールバック</strong>は、<strong>アクティビティクラス</strong>に実装されます。</td>
<td align="left"><strong>アプリケーションのライフサイクル</strong>には<strong>中断</strong>などの状態があります。 <strong>applicationDidEnterBackground:</strong> などのメソッドを<strong>アプリケーションのデリゲート オブジェクト</strong>に実装して、状態が変わったときにコードを実行できます。</td>
<td align="left">アプリケーションには<strong>アプリの実行状態</strong> NotRunning、Activated、Running、Suspending、Suspended、Resuming があります。<br/><br/>状態が変化したときにコードを実行するために、アプリで onlaunched、Onlaunched 化、中断、または再開する <strong><a href="/uwp/api/windows.ui.xaml.application">アプリケーションクラス</a></strong> のメソッドを実装できます。<br/><br/><a href="/windows/uwp/launch-resume/app-lifecycle">アプリのライフサイクル</a></td>
</tr>
<tr class="even">
<td align="left"><strong>バックグラウンドタスク。</strong> <br><br>バックグラウンド操作を実行し、アプリがフォアグラウンドではなくなったときに動作し続けるタスク。</td>
<td align="left">アプリは、アプリがフォアグラウンドではなくなったときにバックグラウンド操作を実行する<strong>サービス</strong>を起動できます。 サービスに独自の<strong>ライフサイクル</strong>があり、マニフェストに登録されています。</td>
<td align="left"><strong>バックグラウンド実行</strong> は、特定のタスクの種類に対してのみ許可されます。<br/><br/>アプリは <strong>UIBackgroundModes</strong> を使って、Info.plist ファイルで<strong>サポートされるバックグラウンド タスク</strong>を宣言します。<br/><br/>システムでは、バックグラウンド タスクがいつどれくらいの長さで実行されるかが制御されます。</td>
<td align="left">バックグラウンドタスクを作成するには、 <strong><a href="/uwp/api/windows.applicationmodel.background.ibackgroundtask">Ibackgroundtask</a></strong> インターフェイスを実装し、アプリケーションマニフェストにタスクを登録します。 <a href="/windows/uwp/launch-resume/run-a-background-task-on-a-timer-"><strong>タイマー</strong></a>、<a href="/uwp/api/windows.applicationmodel.background.systemtriggertype"><strong>システム トリガー</strong></a>、<a href="/windows/uwp/launch-resume/use-a-maintenance-trigger"><strong>メンテナンス トリガー</strong></a>でタスクをトリガーするように設定できます。<br/><br/><a href="/windows/uwp/launch-resume/support-your-app-with-background-tasks">バックグラウンド タスクによるアプリのサポート</a><br/><br/><a href="/windows/uwp/launch-resume/create-and-register-a-background-task">バックグラウンド タスクの作成と登録</a><br/><br/><a href="/windows/uwp/launch-resume/guidelines-for-background-tasks">バックグラウンド タスクのガイドライン</a></td>
</tr>
</tbody>
</table>
<h2 id="performance">パフォーマンス</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>一般的な概念</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>パフォーマンスのベストプラクティス。</strong> <br><br>スタートアップ時間が短く、高速で応答性の高い、バッテリー残量に配慮したアプリを構築するためのガイドライン。</td>
<td align="left">Android では<strong>パフォーマンスに関するベスト プラクティス</strong>のトレーニング ガイドが提供されています。</td>
<td align="left">iOS では<strong>パフォーマンスの概要</strong>に関するドキュメントが提供されています。</td>
<td align="left">パフォーマンスの目標の設定、パフォーマンスの測定、メモリ管理、スムーズなアニメーション、効率的なファイル システムへのアクセス、プロファイリングとパフォーマンスのために使用できるツールなどのトピックが記載されているセクションを含む、詳しい<strong><a href="/windows/uwp/debug-test-perf/performance-and-xaml-ui">パフォーマンス ガイド</a></strong>をご覧いただけます。</td>
</tr>
<tr class="even">
<td align="left"><strong>応答性の高い UI の最適化を表示します。</strong> <br><br>表示を最適化してパフォーマンスを向上させます。</td>
<td align="left">階層ビューアーツールを使用した <strong>レイアウト階層</strong> の最適化、レイアウトの再 <strong>利用</strong> 、および <strong>必要に</strong> 応じたビューの読み込みは、UI スレッドの応答性を維持し、 &quot; アプリケーションに応答しない &quot; ダイアログ (<strong>ANR</strong>) を回避するためのすべての手法です。<br/></td>
<td align="left"><strong>コア アニメーション</strong> ツールを使って<strong>オフスクリーン レンダリング</strong>、<strong>ブレンド レイヤー</strong>、<strong>ラスタライズ</strong>で UI の問題を修正すると、UI スレッドの応答性を確保しやすくなります。</td>
<td align="left">いくつかの簡単な手順を実行すると、XAML の<strong>マークアップ</strong>と<strong>レイアウト</strong>を簡単に<strong>最適化</strong>できます。 レイアウト構造の簡素化、要素数の最小化、過剰な描画の最小化などの手法を利用できます。 <br/><br/><a href="/windows/uwp/debug-test-perf/keep-the-ui-thread-responsive">UI スレッドの応答性の確保</a><br/><br/><a href="/windows/uwp/debug-test-perf/optimize-xaml-loading">XAML マークアップの最適化</a><br/><br/><a href="/windows/uwp/debug-test-perf/optimize-your-xaml-layout">XAML レイアウトの最適化</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>おける.</strong> <br><br><strong>UI の応答性</strong>を確保し、複数<strong>タスクを並行</strong>して実行するためにスレッド処理を使用します。</td>
<td align="left">スレッド処理を行うには、<strong>Runnable</strong>、<strong>Handler</strong>、<strong>ThreadPoolExecutor</strong> の各クラスと、上位レベルの <strong>AsyncTask</strong> クラスを使います。</td>
<td align="left">スレッド処理を行うには、<strong>NSThread</strong>、<strong>Grand Central Dispatch</strong>、上位レベルの <strong>NSOperation</strong> を使います。</td>
<td align="left"><strong><a href="/uwp/api/windows.system.threading.threadpool.runasync">Runasync</a></strong>を使用して<strong>threadpool</strong>に<strong>作業項目</strong>を送信することで、スレッドを操作できます。 <strong><a href="/uwp/api/windows.system.threading.threadpooltimer.createtimer">CreateTimer</a></strong> でタイマーを使って作業項目を送信したり、<strong><a href="/uwp/api/windows.system.threading.threadpooltimer.createperiodictimer">CreatePeriodicTimer</a></strong> で繰り返しの作業項目を作成したりできます。<br/><br/><a href="/windows/uwp/threading-async/submit-a-work-item-to-the-thread-pool">スレッド プールへの作業項目の送信</a><br/><br/><a href="/windows/uwp/threading-async/use-a-timer-to-submit-a-work-item">タイマーを使った作業項目の送信</a><br/><br/><a href="/windows/uwp/threading-async/create-a-periodic-work-item">定期的な作業項目の作成</a><br/><br/><a href="/windows/uwp/threading-async/best-practices-for-using-the-thread-pool">スレッド プールを使うためのベスト プラクティス</a></td>
</tr>
<tr class="even">
<td align="left"><strong>非同期プログラミング。</strong> <br><br>UI スレッドの応答性を確保するために、非同期プログラミング パターンを利用してスレッドが複雑にならないようにします。</td>
<td align="left">独自の非同期クラスを作成するには<strong>スレッド処理を使う必要があります</strong>。 一部の組み込みクラスは非同期です。</td>
<td align="left">独自の非同期クラスを作成するには<strong>スレッド処理を使う必要があります</strong>。 一部の組み込みクラスは非同期です。</td>
<td align="left">非同期パターンを使用すると、独自の Api を作成するときにメインスレッドがブロックされないようにすることができます。たとえば、C# では <strong>async</strong> と <strong>await</strong> を使用し、Visual Basic します。 語が <strong>Async</strong> で終わる非同期の組み込み API を使うことができます。<br/><br/><a href="/windows/uwp/threading-async/asynchronous-programming-universal-windows-platform-apps">非同期プログラミング</a><br/><br/><a href="/windows/uwp/threading-async/call-asynchronous-apis-in-csharp-or-visual-basic">C# または Visual Basic での非同期 API の呼び出し</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>リストビューの最適化。</strong> <br><br>大量のデータを表示する必要がある場合にパフォーマンスを低下させることが多い、データの一覧の最適化を支援する組み込みのパターン</td>
<td align="left"><strong>ViewHolder</strong> デザイン パターンを使って複数のビュー参照を避け、再利用可能な UI 要素を使うことができます。</td>
<td align="left"><strong>UITableView</strong> のパフォーマンスを向上させるさまざまな最適化を行うことができます。組み込まれているものはありません。</td>
<td align="left">既定で <strong>UI の仮想化</strong> を提供する <a href="/uwp/api/windows.ui.xaml.controls.listview">ListView</a> と <a href="/uwp/api/windows.ui.xaml.controls.gridview">GridView</a> の各コントロールを使うと、スムーズなパンやスクロール、起動時間の短縮を実現できます。 <a href="/dotnet/api/system.collections.ilist">IList</a> と <a href="/dotnet/api/system.collections.specialized.inotifycollectionchanged">INotifyCollectionChanged</a> をデータ ソースに実装し、<strong>データ仮想化</strong>を行ってパフォーマンスをさらに改善することもできます。<br/><br/><a href="/windows/uwp/debug-test-perf/optimize-gridview-and-listview">ListView と GridView の UI の最適化</a><br/><br/><a href="/windows/uwp/debug-test-perf/listview-and-gridview-data-optimization">ListView と GridView のデータ仮想化</a></td>
</tr>
</tbody>
</table>
<h2 id="monetization">収益化</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>一般的な概念</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>アプリ内購入。</strong> <br><br>ユーザーがアプリで購入を行うことができるプラットフォーム機能です。</td>
<td align="left"><strong>アプリ内課金</strong> は Google Services によって提供されます。 製品は <strong>Google Play デベロッパー コンソール</strong>に追加されます。 アプリ内購入を実装するには、<strong>Google Play Billing Library</strong> を使います。</td>
<td align="left">製品は <strong>iTunes Connect</strong> に追加されます。 アプリ内購入を実装するには、<strong>StoreKit</strong> フレームワークを使います。<br/><br/>製品を購入するには、<strong>SKMutablePayment</strong> と <strong>SKPaymentQueue</strong> を使います。</td>
<td align="left">アプリでアプリ内製品の購入を作成するには、<a href="/windows/uwp/publish/iap-submissions">アプリに追加してストアに申請</a> します。 <br/><br/><strong><a href="/uwp/api/windows.applicationmodel.store.currentapp">Currentapp クラス</a></strong>を使用して、アプリ内購入を定義します。 <br/><br/><strong><a href="/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync">RequestProductPurchaseAsync</a></strong>を使用して、顧客が製品を購入できるようにするための UI を表示します。<br/><br/><a href="/windows/uwp/monetize/enable-in-app-product-purchases">アプリ内製品購入の有効化</a></td>
</tr>
<tr class="even">
<td align="left"><strong>アプリ内購入を利用できます。</strong> <br><br>購入して使った後でもう一度購入できるアプリ内製品。</td>
<td align="left">通常の購入後に <strong>consumePurchase</strong> で製品を使うと、コンシューマブルな購入が有効になり、購入して使った後でもう一度購入できます。</td>
<td align="left">コンシューマブルな製品は iTunes Connect で<strong>コンシューマブルな製品として定義</strong>されます。</td>
<td align="left">コンシューマブルをサポートするには、ストアに <a href="/windows/uwp/publish/enter-iap-properties">申請するときに製品の種類をコンシューマブルと定義</a> します。 次に、顧客がアクセスできるようにするために、使用可能な購入が行われた後に <strong><a href="/uwp/api/windows.applicationmodel.store.currentapp.reportconsumablefulfillmentasync">ReportConsumableFulfillmentAsync</a></strong> を呼び出します。<br/><br/><a href="/windows/uwp/monetize/enable-consumable-in-app-product-purchases">コンシューマブルなアプリ内購入の有効化</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>アプリ内購入のテスト。</strong> <br><br>アプリをストアに配置せずに、アプリ内購入コードをテストできるようにします。</td>
<td align="left"><strong>アプリ内課金サンドボックス</strong>を使ってテストします。</td>
<td align="left"><strong>サンドボックステスターアカウント</strong> は、テストに使用されます。</td>
<td align="left"><strong><a href="/uwp/api/windows.applicationmodel.store.currentappsimulator">Currentappsimulator</a></strong>クラスを currentapp の代わりに使用するだけで、アプリ内購入をテストできます。<br/><br/></td>
</tr>
<tr class="even">
<td align="left"><strong>試用版.</strong> <br><br>簡単にコンテンツを制限したり、アプリの試用版に基づく広告を削除したりできるようにします。</td>
<td align="left">Google Play は<strong>アプリの試用版を公式にサポートしていません</strong>。 試用または広告の削除を行うには、アプリ内購入を作成し、購入の確認時に適切なコード パスを実行します。</td>
<td align="left">App Store は<strong>アプリの試用版を公式にサポートしていません</strong>。 試用または広告の削除を行うには、アプリ内購入を作成し、購入の確認時に適切なコード パスを実行します。</td>
<td align="left">アプリの無料試用版を提供するには、アプリをストアに送信するときに <strong><a href="/windows/uwp/publish/set-app-pricing-and-availability">[無料試用</a></strong> 版] オプションを使用します。 その後、 <strong><a href="/uwp/api/windows.applicationmodel.store.licenseinformation.istrial">IsTrial</a></strong> を使用してアプリの試用状態を確認し、それに応じて異なるコードパスを提示することができます。 アプリの実行中にユーザーが試用の状態を変更した場合に <a href="/uwp/api/windows.applicationmodel.store.licenseinformation.licensechanged">LicenseChanged イベント</a> が通知されるように登録できます。<br/><br/><a href="/windows/uwp/monetize/exclude-or-limit-features-in-a-trial-version-of-your-app">試用版での機能の除外または制限</a></td>
</tr>
</tbody>
</table>
<h2 id="adapting-to-multiple-platforms">複数のプラットフォームへの対応</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>一般的な概念</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>アダプティブ UI: 柔軟なレイアウト。</strong> <br><br>柔軟な高さと幅の、さまざまな画面サイズをサポートします。</td>
<td align="left">柔軟なレイアウトにするには、LinearLayout オブジェクトで <strong>wrap_content</strong> と <strong>match_parent</strong> の値を使うか、RelativeLayout オブジェクトを使って配置します。</td>
<td align="left">柔軟なレイアウトにするには、ユニバーサル ストーリーボードと一緒に<strong>アダプティブ モデル</strong>を使い、表示コントローラーに適用される horizontalSizeClass や displayScale などの<strong>制約</strong>や<strong>特性</strong>と一緒に<strong>自動レイアウト</strong>を利用します。</td>
<td align="left">柔軟なレイアウトを作成するには、固定サイズ指定と動的なサイズ指定を組み合わせて<strong>レイアウト プロパティ</strong>と<strong>パネル</strong>を使います。<br/><br/><a href="/windows/uwp/layout/layouts-with-xaml">XAML を使ったレイアウトの定義 - レイアウト プロパティとパネル</a><br/><br/><a href="/windows/uwp/layout/screen-sizes-and-breakpoints-for-responsive-design">レスポンシブ デザイン 101</a></td>
</tr>
<tr class="even">
<td align="left"><strong>アダプティブ UI: 調整したレイアウト。</strong> <br><br>別の対象となるレイアウトを使った、さまざまな画面サイズをサポートします。</td>
<td align="left"><strong>small</strong>、<strong>large</strong>、<strong>ldpi</strong>、<strong>hdpi</strong> などの<strong>構成修飾子</strong>を使ってリソース ディレクトリにさまざまな画面構成用の代替レイアウト ファイルを指定すると、さまざまなサイズと密度の画面にカスタム レイアウトを適用できます。</td>
<td align="left"><strong>iPhone と iPad で別々のストーリーボード</strong>を定義すると、ユニバーサルアプリで異なるデバイス ファミリに合わせてレイアウトを調整できます。</td>
<td align="left">カスタマイズされたレイアウトを作るには、デバイス ファミリごとに<strong>別の XAML マークアップ ファイル</strong>を定義します。<br/><br/><a href="/windows/uwp/layout/layouts-with-xaml">XAML を使ったレイアウトの定義 - カスタマイズされたレイアウト</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>アダプティブ UI: 応答性の高いレイアウト。</strong> <br><br>回転などの画面サイズの変更やウィンドウのサイズの変更へ反応します。</td>
<td align="left"><strong>LinearLayout</strong> や <strong>RelativeLayout</strong> と一緒に柔軟なレイアウトを使うか、異なる方向用の代替レイアウト ファイルを提供すると、レスポンシブ レイアウトを使うことができます。</td>
<td align="left">表示の<strong>サイズ</strong>や<strong>特性</strong>が変化すると、ストーリーボードに指定されている<strong>制約</strong>が適用されます。</td>
<td align="left"><strong><a href="/uwp/api/windows.ui.xaml.visualstate">Visualstate</a></strong>、 <strong><a href="/uwp/api/windows.ui.xaml.visualstatemanager">VisualStateManager</a></strong> 、および<strong><a href="/uwp/api/windows.ui.xaml.adaptivetrigger">AdaptiveTrigger</a></strong>を使用したウィンドウサイズの変更に応じて、実行時に UI のセクションを簡単にリフロー、位置変更、サイズ変更、表示、または置換できます。<br/><br/><a href="/windows/uwp/layout/layouts-with-xaml">XAML を使ったレイアウトの定義 - 表示状態と状態トリガー</a><br/><br/><a href="/windows/uwp/layout/screen-sizes-and-breakpoints-for-responsive-design">レスポンシブ デザイン 101</a></td>
</tr>
<tr class="even">
<td align="left"><strong>さまざまなデバイスの機能をサポートします。</strong> <br><br>高度なハードウェア機能に対応しながら、それらの機能を備えていないデバイスもサポートします。</td>
<td align="left"><strong>PackageManager.hasSystemFeature</strong> を使ってデバイス機能を実行時にテストすると、ハードウェア固有のコードを実行できるかどうかを判断することができます。</td>
<td align="left">実行時にデバイス機能をテストするために行える<strong>単一のチェックはありません</strong>。各機能を異なる方法でテストして、ハードウェア固有のコードを実行できるかどうかを判断します。</td>
<td align="left">電話、デスクトップ、IoT など、別のデバイス ファミリの追加機能を対象にした<strong>プラットフォーム拡張 SDK</strong> をパッケージに追加できます。 <strong><a href="/uwp/api/windows.foundation.metadata.apiinformation">ApiInformation API</a></strong> を使って実行時に型とメンバーの存在をテストし、存在している場合のみ、それらの型とメンバーを呼び出すことができます。</td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>さまざまなデバイスの機能をサポートします。</strong> <br><br>高度なハードウェア機能に対応しながら、それらの機能を備えていないデバイスもサポートします。</td>
<td align="left"><strong>Android サポート ライブラリ</strong>をアプリにパッケージして、以前のバージョンの Android ユーザーがいくつかの新しい API を利用できるようにします。 実行時に API レベルのテストを行うには、<strong>Build.Version.SDK_INT</strong> を使います。</td>
<td align="left">クラスが存在するかどうかを確認する <strong>class</strong> メソッドやクラスのメソッドを確認する <strong>respondsToSelector:</strong> など、標準的なランタイム チェックを使って API が利用できるかどうかを調べます。</td>
<td align="left"><strong><a href="/uwp/api/windows.foundation.metadata.apiinformation.isapicontractpresent">IsApiContractPresent</a></strong>を使用して、指定されたメジャーとマイナー番号を持つ API コントラクトが存在するかどうかを識別できます。 また、 <strong><a href="/uwp/api/windows.foundation.metadata.apiinformation">Apiinformation API</a></strong> を使用して、実行時に型とメンバーの存在をテストし、それらの型とメンバーが存在する場合にのみ呼び出すことができます。</td>
</tr>
</tbody>
</table>
<h2 id="notifications">通知</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>一般的な概念</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>タイルとバッジ。</strong> <br><br>ホーム画面上でユーザーに更新プログラムを表示します。</td>
<td align="left"><strong>アプリウィジェット</strong> は、アプリケーション上のビューで、ホーム画面に埋め込むことができ、定期的な更新プログラムを受け取ることができます。 Android にバッジシステムが存在し<strong>ません</strong>。 タイルと同じシステムは存在しません。</td>
  <td align="left">iOS の<strong>ウィジェット</strong>は通知センターに表示され、<strong>アプリの拡張機能</strong>として実装されます。 ローカルまたはリモートの通知に基づいて変更できる数字付きで、<strong>バッジ</strong>をアイコンに追加することもできます。 タイル システムはありません。</td>
<td align="left">アプリにはスタート画面にピン留めできる<strong>タイル</strong>があり、装飾文字と数字付きで選択したテキスト、画像、<strong>バッジ</strong>を表示するために使うことができます。 プッシュ通知または定義済みのスケジュールに基づいて、アプリからタイルのコンテンツを更新することができます。 タイルはアダプティブにでき、表示される場所に従って変更できます。<br/><br/><a href="/windows/uwp/controls-and-patterns/tiles-and-notifications-creating-tiles">タイルの作成</a><br/><br/><a href="/windows/uwp/controls-and-patterns/tiles-and-notifications-create-adaptive-tiles">アダプティブ タイルの作成</a><br/><br/><a href="/windows/uwp/controls-and-patterns/tiles-and-notifications-choosing-a-notification-delivery-method">通知配信方法の選択</a><br/><br/><a href="/windows/uwp/controls-and-patterns/tiles-and-notifications-creating-tiles">タイルとバッジのガイドライン</a></td>
</tr>
<tr class="even">
<td align="left"><strong>通知を表示しています。</strong> <br><br>表示できる通知の種類。</td>
<td align="left">通知を表示できるのは<strong>通知領域</strong>と<strong>通知ドロワー</strong>で、<strong>ヘッドアップ通知</strong>では小さなフローティング ウィンドウに通知が表示されます。 <strong>PendingIntent</strong> を定義すると、通知に操作を追加できます。</td>
<td align="left">ポップアップ通知は<strong>バナー</strong>または<strong>警告</strong>として表示されます。 <strong>UIMutableUserNotificationAction</strong> で定義する<strong>操作可能な通知</strong>にカスタム操作のボタンを追加できます。</td>
<td align="left"><strong>トースト通知</strong>と呼ばれるアダプティブ ポップアップ通知を作成できます。 視覚的なコンテンツ、ボタンや入力などの<strong>操作</strong>、オーディオを使って、XML でトースト通知を定義できます。<br/><br/><a href="/windows/uwp/controls-and-patterns/tiles-and-notifications-adaptive-interactive-toasts">アダプティブ トースト通知と対話型トースト通知</a><br/><br/><a href="/windows/uwp/controls-and-patterns/tiles-and-notifications-choosing-a-notification-delivery-method">通知配信方法の選択</a><br/><br/><a href="/windows/uwp/controls-and-patterns/tiles-badges-notifications">トースト通知のガイドライン</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>ローカル通知をスケジュールします。</strong> <br><br>スケジュールされた時刻にアプリから送信されるローカル通知。</td>
<td align="left">通知とアクションは、 <strong>Notificationcompat</strong> を使用して定義され、 <strong>AlarmManager</strong> と <strong>BroadcastReceiver</strong>を使用して、アプリ内でスケジュールおよび処理することができます。</td>
<td align="left">ローカル通知は <strong>UILocalNotification</strong> を使って作成し、<b>UILocalNotification.scheduleLocalNotification<strong> でスケジュール設定できます。 | トースト通知は </strong><a href="/uwp/api/Windows.UI.Notifications.ScheduledToastNotification">ScheduledToastNotification</a><strong> を使ってスケジュール設定できます。タイル通知をアプリから送信するには </strong><a href="/uwp/api/Windows.UI.Notifications.TileNotification">TileNotification クラス</a><strong>を使い、タイル通知をスケジュール設定するには <a href="/uwp/api/Windows.UI.Notifications.ScheduledTileNotification">ScheduledTileNotification</a> を使います。<br/><br/><a href="/windows/uwp/controls-and-patterns/tiles-and-notifications-adaptive-interactive-toasts">アダプティブ トースト通知と対話型トースト通知</a><br/><br/><a href="/windows/uwp/controls-and-patterns/tiles-and-notifications-sending-a-local-tile-notification">ローカルタイルの通知を送信する</a> | | </strong>プッシュ通知を送信しています。</b> プッシュ通知のサーバーから送信され、必要に応じてアプリで処理される通知。</td>
<td align="left"><strong>Google Cloud Messaging</strong> は、Android のプッシュ通知をサポートしています。</td>
</tr>
</tbody>
</table>
<h2 id="media-capture-and-rendering">メディアのキャプチャとレンダリング</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>一般的な概念</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>メディアをキャプチャしています。</strong> <br><br>オーディオとビジュアルのコンテンツを記録します。</td>
<td align="left">MediaStore.ACTION_VIDEO_CAPTURE などの<strong>インテント</strong>を使うと、既存のカメラ アプリでメディアをキャプチャできます。 <strong>android.hardware.camera2</strong> または <strong>camera</strong> ライブラリを使うと、カメラのカスタム インターフェイスを実装できます。 <strong>Mediarecorder</strong> Api を使用してオーディオをキャプチャできます。</td>
<td align="left"><strong>UIImagePickerController</strong> を使うと、システム UI でビデオと写真をキャプチャできます。 <strong>AVCaptureSession</strong> などの <strong>AVFoundation</strong> クラスを使うと、カメラへ直接アクセスできます。 <br/><strong>AVAudioRecorder</strong> クラスを使うと、オーディオを録音できます。</td>
<td align="left"><strong><a href="/uwp/api/Windows.Media.Capture.CameraCaptureUI">CameraCaptureUI クラス</a></strong>で組み込みのカメラ UI を使用して、写真やビデオをキャプチャできます。 <strong><a href="/uwp/api/Windows.Media.Capture.MediaCapture">MediaCapture API</a></strong> などの <strong><a href="/uwp/api/Windows.Media.Capture">Windows.Media.Capture</a></strong> のクラスを使って、低レベルのカメラ操作でオーディオをキャプチャできます。 <br/><br/><a href="/windows/uwp/audio-video-camera/capture-photos-and-video-with-cameracaptureui">CameraCaptureUI を使った写真とビデオのキャプチャ</a><br/><br/><a href="/windows/uwp/audio-video-camera/capture-photos-and-video-with-mediacapture">MediaCapture を使った写真とビデオのキャプチャ</a></td>
</tr>
<tr class="even">
<td align="left"><strong>メディアの再生。</strong> <br><br>オーディオとビデオのファイルを再生します。</td>
<td align="left"><strong>MediaPlayer</strong> クラスと <strong>AudioManager</strong> クラスを使うと、オーディオとビデオのファイルを再生できます。</td>
<td align="left"><strong>AVKit フレームワーク</strong>、<strong>AVAudioPlayer</strong>、<strong>Media Player Framework</strong> を使うと、オーディオとビデオのファイルを再生できます。</td>
<td align="left"><strong><a href="/uwp/api/Windows.Media.Core.MediaSource">Mediasource クラス</a></strong>、 <strong><a href="/uwp/api/windows.ui.xaml.controls.mediaelement">MediaElement</a></strong>、および<strong><a href="/uwp/api/windows.media.playback.mediaplayer">MediaPlayer</a></strong>クラスを使用して、ローカルファイルやリモートファイルなどのソースからオーディオとビデオを再生できます。<br/><br/><a href="/windows/uwp/audio-video-camera/media-playback-with-mediasource">MediaSource を使ったメディアの再生</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>メディアを編集しています。</strong> <br><br>既存の録音や録画から新しいメディア ファイルを作成し、特殊効果を適用します。</td>
<td align="left"><strong>MediaCodec</strong>、<strong>MediaMuxer</strong>、<strong>android.media.effect</strong> などの低レベルのクラスを使ってコンテンツを編集できます。</td>
<td align="left"><strong>AV Foundation</strong> フレームワークの <strong>AVMutableComposition</strong>、<strong>AVMutableVideoComposition</strong>、<strong>AVMutableAudioMix</strong> などのクラスを使ってコンテンツを編集できます。</td>
<td align="left"><strong><a href="/uwp/api/windows.media.editing.mediacomposition">MediaComposition</a></strong>、<strong><a href="/uwp/api/windows.media.editing.mediaclip">MediaClip</a></strong> などの <strong><a href="/uwp/api/windows.media.editing">Windows.Media.Editing</a></strong> API を使って、オーディオやビデオのファイルからメディア コンポジションを作成できます。 ビデオと画像のオーバーレイの追加、ビデオ クリップの結合、バックグラウンド オーディオの追加、オーディオとビデオの効果の適用を行うことができます。<br/><br/><a href="/windows/uwp/audio-video-camera/media-compositions-and-editing">メディア コンポジションと編集</a></td>
</tr>
</tbody>
</table>
<h2 id="sensors">センサー</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>一般的な概念</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>センサー.</strong> <br><br>デバイスの動き、位置、環境のプロパティを検出します。</td>
<td align="left"><strong>SensorManager</strong> や <strong>SensorEvent</strong> などのクラスを使うと、<strong>センサー フレームワーク</strong>を使ってハードウェアとソフトウェアのセンサーにアクセスできます。</td>
<td align="left"><strong>Core Motion フレームワーク</strong>を使うと、センサーの生データと処理されたデータにアクセスできます。</td>
<td align="left"><strong><a href="/uwp/api/windows.devices.sensors">Windows.Devices.Sensors</a></strong> のクラスを使って、センサーの測定値にアクセスしたり、センサーから新しい測定データを受け取ったときにトリガーされたイベントにアクセスしたりできます。<br/><br/><a href="/windows/uwp/devices-sensors/sensors">センサー</a></td>
</tr>
</tbody>
</table>
<h2 id="location-and-mapping">位置情報とマッピング</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>一般的な概念</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>設置.</strong> <br><br>デバイスの<strong>現在</strong>位置を検出し、<strong>変更</strong>を追跡します。</td>
<td align="left">Google Play サービスの位置情報 API では、<strong>getLastLocation</strong> メソッドと <strong>requestLocationUpdates</strong> メソッドを使って、<strong>Fused Location Provider</strong> が取得した<strong>最後の既知の位置情報</strong>への高レベルのアクセスを提供します。 低レベルのアクセスは、Android ライブラリの <strong>LocationManager</strong> で提供されます。</td>
<td align="left"><strong>Core Location の</strong> <strong>CLLocationManager</strong> クラスを使ってデバイスの位置を監視します。<strong>startUpdatingLocation</strong> で標準位置情報サービス、<strong>startMonitoringSignificantLocationChanges</strong> で<strong>大幅変更</strong>位置情報サービスを利用できます。</td>
<td align="left">デバイスの位置情報を追跡するには、<strong><a href="/uwp/api/windows.devices.geolocation">Windows.Devices.Geolocation</a></strong> のクラスを使います。 1回限りの読み取りには、 <strong><a href="/uwp/api/windows.devices.geolocation.geolocator.getgeopositionasync">Geolocator. GetGeopositionAsync</a></strong> を使用します。 <strong><a href="/uwp/api/windows.devices.geolocation.geolocator.positionchanged">Geolocator</a></strong>を使用して、タイマーを使用して定期的に場所を取得するか、場所が変更されたときに通知を受けるようにします。<br/><br/><a href="/windows/uwp/maps-and-location/get-location">ユーザーの位置情報の取得</a></td>
</tr>
<tr class="even">
<td align="left"><strong>マップを表示しています。</strong> <br><br><strong>組み込みの対話式の地図</strong>を表示し、<strong>関心のあるポイント</strong>を追加します。</td>
<td align="left"><strong>Google Maps Android API</strong> の <strong>GoogleMap</strong>、<strong>MapFragment</strong>、<strong>MapView</strong> クラスで、アプリに地図を埋め込むことができます。 関心のあるポイントを表示するには、<strong>マーカー</strong>と、カスタマイズ可能な <strong>Marker</strong> クラスを使います。</td>
<td align="left">地図を iOS アプリに埋め込むには、<strong>MapKit フレームワーク</strong>の <strong>MKMapView</strong> クラスを使います。 <strong>MKPointAnnotation</strong> などのオブジェクト クラスや <strong>MKPinAnnotationView</strong> などのビュー クラスを使ってアプリに<strong>アノテーション</strong>を追加して、関心のあるポイントを表示できます。</td>
<td align="left">アプリに地図を埋め込むには、2D、3D、Streetside ビューを表示する組み込みの <strong><a href="/uwp/api/windows.ui.xaml.controls.maps.mapcontrol">MapControl</a></strong> XAML コントロールを使います。 <strong><a href="/uwp/api/windows.ui.xaml.controls.maps.mapicon">Mapicon</a></strong>、 <strong><a href="/uwp/api/windows.ui.xaml.controls.maps.mappolygon">MapPolygon</a></strong> 、 <strong><a href="/uwp/api/windows.ui.xaml.controls.maps.mappolyline">MapPolyline</a></strong>などのクラスを使用して、プッシュピン、画像、または図形に関心のあるポイントを追加できます。<br/><br/><a href="/windows/uwp/maps-and-location/display-maps">2D、3D、Streetside ビューでの地図の表示</a><br/><br/><a href="/windows/uwp/maps-and-location/display-poi">関心のあるポイント (POI) の地図への表示</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>ジオフェンシング.</strong> <br><br>特定の地理的な地域への進入と退出を監視します。</td>
<td align="left">ジオフェンスを監視するには、Google Play Services SDK の<strong>位置情報サービス</strong>を使います。</td>
<td align="left">地域を監視するには <strong>CLCircularRegion</strong> クラスを使い、登録するには <strong>CLLocationManager.startMonitoringForRegion:</strong> を使います。</td>
<td align="left"><strong><a href="/uwp/api/windows.devices.geolocation.geofencing.geofence">Geofence</a></strong> クラスを使ってジオフェンスを作成し、地域への進入と退出など、<strong>監視する状態</strong>を定義できます。 ジオフェンス イベントをフォアグラウンドで処理するには <strong><a href="/uwp/api/windows.devices.geolocation.geofencing.geofencemonitor">GeofenceMonitor クラス</a></strong>、バックグラウンドで処理するには <strong><a href="/uwp/api/windows.applicationmodel.background.locationtrigger">LocationTrigger バックグラウンド クラス</a></strong>を使います。<br/><br/><a href="/windows/uwp/maps-and-location/set-up-a-geofence">ジオフェンスのセットアップ</a></td>
</tr>
<tr class="even">
<td align="left"><strong>ジオコーディングと reverse ジオコーディング。</strong> <br><br>住所を地理的な位置に変換したり (ジオコーディング)、地理的な位置を住所に変換したりします (逆ジオコーディング)。<br/></td>
<td align="left">ジオコーディングや逆ジオコーディングには <strong>Geocoder</strong> クラスを使います。</td>
<td align="left">ジオコーディングには <strong>CLGeocoder</strong> クラスを使います。</td>
<td align="left">ジオコーディングは、 <strong><a href="/uwp/api/windows.services.maps.maplocationfinder">MapLocationFinder のクラス</a></strong>を使用して実行<strong><a href="/uwp/api/windows.services.maps">できます。</a></strong> ジオコーディングには <strong><a href="/uwp/api/windows.services.maps.maplocationfinder.findlocationsasync">FindLocationsAsync</a></strong> を、reverse ジオコーディングには <strong><a href="/uwp/api/windows.services.maps.maplocationfinder.findlocationsatasync">Findlocationsatasync</a></strong> を使用します。<br/><br/><a href="/windows/uwp/maps-and-location/geocoding">ジオコーディングと逆ジオコーディングの実行</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>ルートと方向。</strong> <br><br>地理的な 2 つの場所の間のルート、距離、ルート案内を提供します。</td>
<td align="left">Google は、Android で使うことができる Web サービス <strong>Google Maps Directions API</strong> を提供しています。ただし、SDK は提供されていません。</td>
<td align="left">Map Kit で提供されている <strong>MKDirections</strong> API を使ってルートとルート案内を取得できます。</td>
<td align="left"><strong><a href="/uwp/api/windows.services.maps">Windows. service. Maps</a></strong>で<strong><a href="/uwp/api/windows.services.maps.maproutefinder">maproutefinder</a></strong>クラスを使用して、ウォーキングまたは運転ルートを要求できます。 ルートは <strong><a href="/uwp/api/windows.services.maps.maproute">MapRoute</a></strong> インスタンスとして返されるため、MapControl に簡単に表示できます。 方向は、 <strong><a href="/uwp/api/windows.services.maps.maproutemaneuver">MapRouteManeuver</a></strong> オブジェクト内で返されます。<br/><br/><a href="/windows/uwp/maps-and-location/routes-and-directions">地図へのルートとルート案内の表示</a></td>
</tr>
</tbody>
</table>
<h2 id="app-to-app-communication">アプリ間通信</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>一般的な概念</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>別のアプリを呼び出しています。</strong> <br><br>別のアプリを起動して、必要に応じてリンク、テキスト、写真、ビデオ、ファイルなどのデータを共有します。</td>
<td align="left"><strong>インテント</strong>で<strong>アクション</strong>と必要に応じてデータを定義し、<strong>startActivityForResult</strong> で呼び出すと、<strong>暗黙的インテント</strong>を使って別のアプリを起動できます。<br/></td>
<td align="left">アプリ<strong>拡張機能</strong>を使用して、別のアプリへのアプリデータへのアクセスを提供できます。 <strong>URL スキーム</strong>を使うと、URL を別のアプリに渡すことができます。</td>
<td align="left"><strong><a href="/uwp/api/windows.system.launcher.launchuriasync">LaunchUriAsync</a></strong>を使用して URI に登録されている別のアプリを起動するか、<strong><a href="/uwp/api/windows.system.launcher.launchuriforresultsasync">ランチャー</a></strong>を起動して結果を表示し、起動したアプリからデータを取得することができます。 <strong><a href="/uwp/api/windows.system.launcher.launchfileasync">LaunchFileAsync</a></strong>を使用して、ファイルを別のアプリに渡して処理することができます。<br/><br/><strong>共有コントラクト</strong>を使って、アプリ間でデータを簡単に共有できます。<br/><br/><a href="/windows/uwp/launch-resume/launch-default-app">URI に応じた既定のアプリの起動</a><br/><br/><a href="/windows/uwp/launch-resume/how-to-launch-an-app-for-results">結果を取得するためのアプリの起動</a><br/><br/><a href="/windows/uwp/launch-resume/launch-the-default-app-for-a-file">ファイルに応じた既定のアプリの起動</a><br/><br/><a href="/windows/uwp/app-to-app/share-data">データの共有</a></td>
</tr>
<tr class="even">
<td align="left"><strong>アプリを呼び出すことができるようにします。</strong> <br><br>アプリが別のアプリからの要求に応答できるようにします。</td>
<td align="left">アプリで<strong>インテント フィルター</strong>に<strong>インテント処理動作</strong>を登録し、別のアプリからの暗黙的インテントに対応します。</td>
<td align="left"><strong>アプリの拡張機能</strong>をパッケージ化すると、データを他のアプリと共有できます。 アプリで<strong>カスタム URL スキーム</strong>を登録するには、Info.plist で <strong>CFBundleURLTypes</strong> キーを使います。</td>
<td align="left">アプリを <strong>URI スキーム名</strong>の既定のハンドラーとして登録するには、パッケージ マニフェストに<strong><a href="/uwp/api/windows.applicationmodel.activation.activationkind#Protocol">プロトコル</a></strong>を登録して <strong><a href="/uwp/api/windows.ui.xaml.application.onactivated">Application.OnActivated</a></strong> イベント ハンドラーを更新し、必要に応じて結果を返すようにします。 同様に、アプリを特定のファイルの種類の既定のハンドラーとして登録するには、パッケージ マニフェストに宣言を追加して <strong><a href="/uwp/api/windows.ui.xaml.application.onfileactivated">Application.OnFileActivated</a></strong> イベントを処理します。<br/><br/>共有コントラクト要求を処理するには、アプリをマニフェストで共有ターゲットとして登録し、 <strong><a href="/uwp/api/windows.ui.xaml.application.onsharetargetactivated">OnShareTargetActivated</a></strong> イベントを処理します。<br/><br/><a href="/windows/uwp/launch-resume/how-to-launch-an-app-for-results">結果を取得するためのアプリの起動</a><br/><br/><a href="/windows/uwp/launch-resume/handle-file-activation">ファイルのアクティブ化の処理</a><br/><br/><a href="/windows/uwp/app-to-app/receive-data">データの受信</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>コピーして貼り付けます。</strong> <br><br>アプリ間でテキストやその他のコンテンツをコピーしたり貼り付けしたりします。</td>
<td align="left"><strong>クリップボード フレームワーク</strong>を使って、<strong>ClipboardManager</strong> と <strong>ClipData</strong> の各クラスでコピーと貼り付けを実装できます。</td>
<td align="left"><strong>UIPasteboard</strong>、<strong>UIMenuController</strong>、<strong>UIResponderStandardEditActions</strong> を使って、コピーとペーストを実装できます。</td>
<td align="left">多くの既定の XAML コントロールは既にコピーと貼り付けをサポートしています。 自分でコピーと貼り付けを実装するには、<strong><a href="/uwp/api/Windows.ApplicationModel.DataTransfer">Windows.ApplicationModel.DataTransfer</a></strong> の <strong><a href="/uwp/api/windows.applicationmodel.datatransfer.datapackage">DataPackage</a></strong> と <strong><a href="/uwp/api/Windows.ApplicationModel.DataTransfer.Clipboard">Clipboard</a></strong> の各クラスを使います。<br/><br/><a href="/windows/uwp/app-to-app/copy-and-paste">コピーと貼り付け</a></td>
</tr>
<tr class="even">
<td align="left"><strong>ドラッグアンドドロップ。</strong> <br><br>アプリ間でコンテンツをドラッグ アンド ドロップします。</td>
<td align="left">1 つのアプリケーションにドラッグ アンド ドロップを実装するには、<strong>Android ドラッグ/ドロップ フレームワーク</strong>を使います。</td>
<td align="left">iOS では高度なドラッグ アンド ドロップ API は提供されていません。</td>
<td align="left">アプリにドラッグ アンド ドロップを実装すると、アプリとアプリ、デスクトップとアプリ、アプリとデスクトップのドラッグ アンド ドロップ機能を有効にできます。 UIElement クラスでドラッグアンドドロップのサポートを実装するには、 <strong><a href="/uwp/api/windows.ui.xaml.uielement.allowdrop">system.windows.uielement.allowdrop</a></strong>、 <strong><a href="/uwp/api/windows.ui.xaml.uielement.candrag">candrag</a></strong> プロパティ、および <strong><a href="/uwp/api/windows.ui.xaml.uielement.dragover">system.windows.dragdrop.dragover></a></strong>、および <strong><a href="/uwp/api/windows.ui.xaml.uielement.drop">drop</a></strong> イベントを使用します。<br/><br/><a href="/windows/uwp/app-to-app/drag-and-drop">ドラッグ アンド ドロップ</a></td>
</tr>
</tbody>
</table>
<h2 id="software-design">ソフトウェアの設計</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>一般的な概念</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>ソフトウェア設計パターン。</strong> <br><br>プラットフォーム向けの推奨パターンまたはよく使われるパターン。</td>
<td align="left">正式なパターンは、推奨されていないか、Android の開発用に提供されていません。ただし、ベータ版のデータ バインディング フレームワークでは、<strong>モデル - ビュー - ビューモデル (MVVM)</strong> パターンを現在よりも利用できるようになる可能性があります。 多くのサード パーティの記事やフレームワークでは、<strong>モデル ビュー プレゼンター (MVP)</strong> と <strong>MVVM</strong> を使った方法を推奨しています。</td>
<td align="left"><strong>モデルビューコントローラー (MVC)</strong> は、iOS で使用される一般的なパターンであり、プラットフォームに統合されています。</td>
<td align="left">UWP 用のアプリ作成時、特定のパターンに制限されません。<br/><br/>組み込みの <a href="/windows/uwp/data-binding/index">データ バインディング</a> パターンを使って、データの問題と UI の問題を明確に切り離すことができ、後でプロパティ値を更新する UI イベント ハンドラーをコード内に記述することを回避できます。<br/><br/>データ バインディングを拡張して<strong>モデル - ビュー - ビューモデル (MVVM)</strong> パターンに従うことができます。そのためには、<a href="https://archive.codeplex.com/?p=mvvmlight">MVVM Light Toolkit</a> などのサード パーティの MVVM ライブラリを使うか、独自に展開してロジックをコードビハインドから排除します。<br/><br/><a href="/previous-versions/msp-n-p/hh848246(v=pandp.10)">MVVM パターン</a><br/><br/><a href="https://github.com/Windows-XAML/Template10/wiki">Template 10 Visual Studio プロジェクト テンプレート</a></td>
</tr>
</tbody>
</table>