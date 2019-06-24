---
Description: iOS、Android、Windows 10 のプラットフォーム機能を比較します。
title: Android と iOS 開発者向けの Windows アプリ概念マッピング
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 082736c8-2ac3-41b3-b246-e705edc23f34
ms.localizationpriority: medium
ms.openlocfilehash: ba17b8a63d169f2d232930b209bfadd5a2fbdff5
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/21/2019
ms.locfileid: "67319781"
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
<td align="left"><strong>言語を設計します。</strong><br><br>プラットフォーム上のアプリの外観と動作方法を規定する規則のセット。</td>
<td align="left"><strong>Android のマテリアル デザイン</strong> ガイドラインに、Android の設計者や開発者が従う視覚言語が示されています。</td>
<td align="left"><strong>ヒューマン インターフェイス ガイドライン</strong>に、iOS の設計者や開発者向けのアドバイスが示されています。</td>
<td align="left"><a href="https://developer.microsoft.com/en-us/windows/apps/design"><strong>UWP Windows アプリのデザイン</strong></a>次のすべての Windows 10 デバイスで画期的なアプリを作成する方法を示します。 ユーザー インターフェイス (UI) デザインの基本、レスポンシブ デザイン テクニック、詳細なガイドラインの完全な一覧が示されています。<br/></td>
</tr>
<tr class="even">
<td align="left"><strong>ユーザー インターフェイスのマークアップ言語。</strong> <br><br>UI とそのコンポーネントをレンダリングし、記述するマークアップ言語。 プラットフォームごとに、ビジュアル編集とマークアップ編集のためのエディターが提供されています。<br/></td>
<td align="left"><strong>XML レイアウト</strong>。<strong>Android Studio</strong> または <strong>Eclipse</strong> を使って編集します。</td>
<td align="left"><strong>XIB</strong> と<strong>ストーリーボード</strong>。Xcode 内の <strong>Interface Builder</strong> を使って編集します。</td>
<td align="left"><strong><a href="https://docs.microsoft.com/windows/uwp/xaml-platform/xaml-overview">XAML</a></strong>編集を使用して、 <strong><a href="https://visualstudio.microsoft.com/">Microsoft Visual Studio</a></strong>と <strong><a href="https://docs.microsoft.com/visualstudio/designers/creating-a-ui-by-using-blend-for-visual-studio?view=vs-2015">Blend for Visual Studio</a></strong>します。<br/><br/><a href="https://docs.microsoft.com/windows/uwp/xaml-platform/index">XAML プラットフォーム</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/design/basics/xaml-basics-ui">XAML を使った UI の作成</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/layout/layouts-with-xaml">XAML を使ったレイアウトの定義</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>組み込みのユーザー インターフェイスを制御します。</strong> <br><br>ボタン、リスト コントロール、テキスト コントロールなど、プラットフォームで提供される、再利用可能な UI 要素。</td>
<td align="left">ウィジェット、レイアウト、テキスト フィールド、コンテナー、日付/時刻コントロール、専門的なコントロールとして参照される、作成済みの<strong>ビュー</strong>と<strong>ビュー グループ</strong>のクラス。</td>
<td align="left">Xcode オブジェクトのライブラリにあり、UIKit ユーザー インターフェイス カタログに掲載された<strong>ビュー</strong>と<strong>コントロール</strong>。 ビューには、イメージ ビュー、ピッカー ビュー、スクロール ビューが含まれます。 コントロールには、ボタン、日付選択コントロール、テキスト フィールドが含まれます。</td>
<td align="left">XAML プラットフォームでは、ボタン、リスト コントロール、パネル、テキスト コントロール、コマンド バー、ファイル ピッカー、メディア、インク入力など、さまざまな<strong>ビルトイン コントロール</strong>が提供されます。<br/><br/><a href="https://docs.microsoft.com/windows/uwp/controls-and-patterns/controls-and-events-intro">コントロールの追加とイベントの処理</a></td>
</tr>
<tr class="even">
<td align="left"><strong>イベント処理を制御します。</strong> <br><br>UI コントロール内でイベントがトリガーされるときに実行されるロジックを定義します。</td>
<td align="left">XML またはプログラムで<strong>イベント ハンドラー</strong>と<strong>イベント リスナー</strong>を追加します。</td>
<td align="left">コントロールから<strong>ターゲット</strong>へ<strong>操作</strong>メッセージが送信されます。</td>
<td align="left">XAML ページに接続されている<strong>コード ビハインド ファイル</strong>に、XAML コントロールのイベントを処理するメソッドを定義できます。 <strong>イベント ハンドラー</strong>は常に、コードで記述します。 ただし、XAML マークアップまたはコードでそれらのハンドラーをイベントにフックすることができます。<br/><br/><a href="https://docs.microsoft.com/windows/uwp/controls-and-patterns/controls-and-events-intro">コントロールの追加とイベントの処理</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/xaml-platform/events-and-routed-events-overview">イベントとルーティング イベントの概要</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>データ バインディング。</strong> <br><br>アプリの UI でデータを表示し、必要に応じてそのデータと同じ状態を保つことができるソフトウェアの設計パターンです。</td>
<td align="left"><strong>データ バインディング ライブラリ</strong>が提供されています。ただし、まだベータ版です。</td>
<td align="left">iOS では、組み込みのバインディング システムは存在しません。 サード パーティのライブラリを使うか追加コードを記述すると、<strong>キー値監視</strong>を作成してデータ バインディングを実行できます。 コントロールは、データを取得するためにデリゲート/コールバックの方法を使います。</td>
<td align="left">UWP プラットフォームが<strong>データ バインディング</strong>を処理します。 <strong><a href="https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension">{x:Bind}</a></strong> マークアップ拡張を使ってバインディングのパフォーマンスを向上させるか、<strong><a href="https://docs.microsoft.com/windows/uwp/xaml-platform/binding-markup-extension">{Binding}</a></strong> を使ってより多くの機能を利用できます。 その後は、プラットフォームで<strong>一方向バインディング</strong>を使ってデータ ソースからの値を UI に表示するか、<strong>双方向バインディング</strong>で値の監視も行い、値が変わったら UI を更新するか、バインディングの構成方法を選ぶだけです。<br/><br/><a href="https://docs.microsoft.com/windows/uwp/data-binding/index">データ バインディング</a></td>
</tr>
<tr class="even">
<td align="left"><strong>UI オートメーションです。</strong> <br><br>プログラムによる UI 要素へのアクセスで、アプリをアクセシビリティ対応にして支援技術製品を利用できるようにして、自動テスト スクリプトで UI を操作できるようにします。</td>
<td align="left"><strong>テキストのラベル</strong>、<strong>contentDescription</strong>、<strong>ヒント</strong>の値を使って、オートメーションで UI 要素が見つかるようにします。 Android Studio で <strong>UI Automator</strong> と <strong>Espresso</strong> テスト フレームワークを使って UI のテストを作成できます。</td>
<td align="left"><strong>Automation インストルメント</strong>で、<strong>アクセシビリティ</strong>設定や<strong>要素階層</strong>内の要素の位置を使って要素を識別する、自動化された UI テスト スクリプトを記述できます。</td>
<td align="left"><strong><a href="https://docs.microsoft.com/windows/desktop/WinAuto/uiauto-uiautomationoverview">UI オートメーション</a></strong>を使って、UWP の組み込みの UI 要素にプログラムから既定でアクセスできます。<br/><strong><a href="https://docs.microsoft.com/windows/uwp/accessibility/custom-automation-peers">カスタムのオートメーション ピア</a></strong>独自のカスタム UI クラスのオートメーションのサポートを提供することです。 Visual Studio の<strong><a href="https://docs.microsoft.com/visualstudio/test/use-ui-automation-to-test-your-code?view=vs-2015">コード化された UI テスト プロジェクト</a></strong>で、UI を使ってアプリケーション全体を自動的にテストするか、UI を単独でテストできます。</td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>コントロールの外観を変更します。</strong> <br><br>サイズ、色、その他の属性を編集します。</td>
<td align="left">コントロールには<strong>プロパティ</strong>があり、デザイナー ツールを使って XML マークアップまたはプログラムで編集できます。</td>
<td align="left">コントロールには<strong>属性</strong>があり、<strong>属性インスペクター</strong>を使って Interface Builder またはプログラムで編集できます。</td>
<td align="left">Visual Studio と Blend for Visual Studio を使って、XAML マークアップまたはプログラムでコントロールの<strong>プロパティ</strong>を編集できます。<br/><br/><a href="https://docs.microsoft.com/windows/uwp/controls-and-patterns/controls-and-events-intro">コントロールの追加とイベントの処理</a></td>
</tr>
<tr class="even">
<td align="left"><strong>再利用可能な visual スタイル。</strong> <br><br>視覚的な変更を再利用可能な形式でいくつかのコントロールに適用します。</td>
<td align="left"><strong>XML スタイル</strong>は 1 つまたは複数のコントロールに適用されるプロパティのセットです。</td>
<td align="left">iOS では、既定では再利用可能な視覚スタイルをサポートしていませんが、UIAppearance プロトコルを使って複数のコントロールで共通の属性を共有できます。</td>
<td align="left">再利用可能な<strong><a href="https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Style">スタイル</a></strong>を作成して複数のコントロールに適用したり、簡単に再利用できるように <strong><a href="https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary">ResourceDictionary</a></strong> に格納したりできます。<br/><br/><a href="https://docs.microsoft.com/previous-versions/windows/apps/hh465381(v=win.10)">クイック スタート:コントロールのスタイル設定</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>コントロールの視覚的な構造を編集します。</strong> <br><br>チェックボックスの下にあるチェックボックスのテキストの移動など、プロパティまたは属性を変更する以外の、コントロールの視覚的構造をカスタマイズします。</td>
<td align="left">Android では、コントロールの視覚的構造を編集する簡単な方法は存在しません。</td>
<td align="left">iOS では、コントロールの視覚的構造を編集する簡単な方法は存在しません。</td>
<td align="left">コントロールの視覚的な構造をカスタマイズするには、XAML マークアップで<strong><a href="https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate">コントロール テンプレート</a></strong>をコピーして編集します。<br/><br/><a href="https://docs.microsoft.com/previous-versions/windows/apps/hh465374(v=win.10)">クイック スタート:コントロール テンプレート</a></td>
</tr>
<tr class="even">
<td align="left"><strong>組み込みのタッチ ジェスチャ。</strong> <br><br>ビューやコントロールでタップやダブルタップなどの高レベルの抽象化されたジェスチャ イベントを処理して、カスタマイズされたタッチをサポートします。</td>
<td align="left"><strong>ジェスチャ ディテクター</strong>が、スクロール、長押し、タップ、ダブルタップ、フリックなどの一般的なタッチ ジェスチャを検出します。</td>
<td align="left">UIKit フレームワークで提供される組み込みの<strong>ジェスチャ認識エンジン</strong>が、タップ、ピンチ、パン、スワイプ、回転、長押しなどのタッチ ジェスチャを検出します。</td>
<td align="left"><strong>UI 要素</strong>を使って、タップ、ダブルタップ、右タップ、ホールドなどの<strong>静的ジェスチャ イベント</strong>だけでなく、スライド、スワイプ、回転、ピンチ、ストレッチなどの<strong>操作ジェスチャ イベント</strong>を処理できます。 ジェスチャ イベントは<strong>ルーティング イベント</strong>であり、UIElement の子が含まれている親オブジェクトを使って処理できます。<br/><br/><a href="https://docs.microsoft.com/windows/uwp/input-and-devices/touch-interactions">タッチ操作</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/design/layout/index">カスタム ユーザー操作 - ジェスチャ、操作、対話式操作</a></td>
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
<td align="left"><strong>レイアウト。</strong> <br><br>レイアウトでは、ユーザー インターフェイスの構造を定義します。</td>
<td align="left">レイアウトは、他のビュー グループやビューを入れ子にできる <strong>LinearLayout</strong> や <strong>RelativeLayout</strong> などの<strong>ビュー グループ</strong>で構成されます。</td>
<td align="left">レイアウトは、入れ子にできる <strong>UIView</strong> が含まれる <strong>UIViewController</strong> で構成されます。</td>
<td align="left">XAML が、静的レイアウトやレスポンシブ レイアウト用の <strong><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.canvas">Canvas</a></strong>、<strong><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.grid">Grid</a></strong>、<strong><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.relativepanel">RelativePanel</a></strong>、<strong><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.stackpanel">StackPanel</a></strong> などの<strong>レイアウト パネル クラス</strong>から成る柔軟なレイアウト システムを提供します。 <strong><a href="https://docs.microsoft.com/visualstudio/ide/reference/properties-window?view=vs-2015">プロパティ</a></strong>要素の位置とサイズを制御するために使用します。<br/><br/><a href="https://docs.microsoft.com/windows/uwp/layout/layouts-with-xaml">XAML を使ったレイアウトの定義</a><br/></td>
</tr>
<tr class="even">
<td align="left"><strong>ピア ナビゲーション。</strong> <br><br>階層の重要度が同じページ間を移動する方法をユーザーに提供します。</td>
<td align="left"><strong>タブ</strong>、<strong>スワイプ ビュー</strong>、<strong>ナビゲーション ドロワー</strong>で<strong>水平方向ナビゲーション</strong>ができます。</td>
<td align="left"><strong>タブ バー コントローラー</strong>、<strong>分割ビュー コントローラー</strong>、<strong>ページ ビュー コントローラー</strong>で同じ階層のビューの間を移動できます。</td>
<td align="left"><strong><a href="https://docs.microsoft.com/windows/uwp/controls-and-patterns/tabs-pivot">タブ/ピボット</a></strong>を使って、コンテンツの上にあるリンクやタブの永続的な一覧を表示できます。 <strong><a href="https://docs.microsoft.com/windows/uwp/controls-and-patterns/split-view">ナビゲーション ウィンドウと分割ビュー</a></strong>で、コンテンツと共にリンクの一覧を表示することができます。<br/><br/><a href="https://docs.microsoft.com/windows/uwp/layout/navigation-basics">ナビゲーション</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/layout/navigate-between-two-pages">2 つのページ間を移動します。</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>階層間を移動します。</strong> <br><br>階層の親と子のページ間を移動します。</td>
<td align="left">他の<strong>アクティビティ</strong>を読み込む<strong>インテント</strong>と一緒に<strong>リスト</strong>、<strong>グリッド リスト</strong>、<strong>ボタン</strong>などのコントロールを使うと、<strong>子孫のナビゲーション</strong>ができます。</td>
<td align="left"><strong>ナビゲーション コントローラー</strong>を使うと、ユーザーが階層のレベルの間を移動できます。</td>
<td align="left"><strong><a href="https://docs.microsoft.com/windows/uwp/controls-and-patterns/hub">ハブ</a></strong>ユーザーにコンテンツの子ページに移動する選択された状態のプレビューを表示できます。 <strong><a href="https://docs.microsoft.com/windows/uwp/controls-and-patterns/master-details">マスター/詳細</a></strong>ユーザーが、対応する詳細セクションの横に表示される項目の概要の一覧から選択できるようにします。<br/><br/><a href="https://docs.microsoft.com/windows/uwp/layout/navigation-basics">ナビゲーション</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/layout/navigate-between-two-pages">2 つのページ間を移動します。</a></td>
</tr>
<tr class="even">
<td align="left"><strong>ボタンのナビゲーションをバックアップします。</strong> <br><br>アプリケーション内で元の画面に戻ります。</td>
<td align="left">アクション バーの <strong>[戻る]</strong> ボタンと <strong>[上へ]</strong> ボタンで、<strong>バック スタック</strong>を使って<strong>先祖</strong>ナビゲーションと<strong>一時的な</strong>ナビゲーションができます。</td>
<td align="left"><strong>ナビゲーション コントローラー</strong>に [戻る] ボタンを追加することができます。<br/></td>
<td align="left"><strong><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.backstack">バック スタック プロパティ</a></strong>を使ってソフトウェアまたはハードウェアの [戻る] ボタン入力を簡単に処理できます。ユーザーは<strong>ナビゲーション履歴</strong>を移動できます。<br/><br/><a href="https://docs.microsoft.com/windows/uwp/layout/navigation-history-and-backwards-navigation">[戻る] ボタンによるナビゲーション</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>スプラッシュ スクリーン。</strong> <br><br>アプリ起動時にイメージを表示します。主にブランディング目的です。</td>
<td align="left">スプラッシュ画面は既定では指定されていません。最初のアクティビティの<strong>テーマの背景</strong>を編集すると実装されます。</td>
<td align="left"><strong>静的起動イメージ</strong>または<strong>XIB/ストーリーボードの起動ファイル</strong>がアプリに必要です。</td>
<td align="left"><strong>イメージ</strong>とカラーの背景を使ってスプラッシュ画面を作成できます。 <a href="https://docs.microsoft.com/windows/uwp/launch-resume/create-a-customized-splash-screen">スプラッシュ画面の時間を延ばすことができます</a>。<br/><br/><a href="https://docs.microsoft.com/windows/uwp/launch-resume/add-a-splash-screen">スプラッシュ画面の追加</a></td>
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
<td align="left"><strong>音声。</strong> <br><br>音声入力に対する音声認識と、その他の音声機能。</td>
<td align="left"><strong>Google 音声検索</strong>などの <strong>RecognizerIntent</strong> を実装するアプリで音声入力を提供できます。 <strong>SpeechRecognizer</strong> クラスを使うと、アプリで Google の音声認識 API を使うことができます。</td>
<td align="left">アプリでは、<strong>SFSpeechRecognizer</strong> クラスを使用して音声入力と音声認識を実装することができます。</td>
<td align="left"><strong><a href="https://docs.microsoft.com/windows/uwp/input-and-devices/speech-recognition">音声認識</a></strong> API を使ってフォアグラウンドでアプリを操作できます。 音声認識に基づく <strong><a href="https://docs.microsoft.com/windows/uwp/input-and-devices/cortana-interactions">Cortana の操作</a></strong>で、フォアグラウンドまたはバックグラウンドでアプリを起動したり、バックグラウンドのアプリを操作したりできます。<br/><br/><a href="https://docs.microsoft.com/windows/uwp/input-and-devices/speech-interactions">音声操作</a></td>
</tr>
<tr class="even">
<td align="left"><strong>カスタム ユーザーの入力。</strong> <br><br>キーボード、マウス、スタイラスなどの入力を処理します。</td>
<td align="left"><strong>タッチ</strong>、<strong>タッチパッド</strong>、<strong>スタイラス</strong>、<strong>マウス</strong>、<strong>キーボード</strong>の操作がサポートされています。 移動と入力はタッチ操作と同じ方法で報告されますが、<strong>入力デバイス</strong>に関する詳しい情報を検出することができます。</td>
<td align="left"><strong>タッチ</strong>、<strong>Apple Pencil</strong>、ハードウェア <strong>キーボード</strong>がサポートされています。</td>
<td align="left"><strong><a href="https://docs.microsoft.com/windows/uwp/input-and-devices/touch-interactions">タッチ</a></strong>、<strong><a href="https://docs.microsoft.com/windows/uwp/input-and-devices/touchpad-interactions">タッチパッド</a></strong>、デジタル インクを使った<strong><a href="https://docs.microsoft.com/windows/uwp/input-and-devices/pen-and-stylus-interactions">ペン/スタイラス</a></strong>、<strong><a href="https://docs.microsoft.com/windows/uwp/input-and-devices/mouse-interactions">マウス</a></strong>、<strong><a href="https://docs.microsoft.com/windows/uwp/input-and-devices/keyboard-interactions">キーボード</a></strong>など、さまざまな操作がサポートされています。 どの入力デバイスが使われたかわからなくても、アプリがデータを処理し、必要に応じて未加工入力デバイス データにアクセスできます。<br/><br/><a href="https://docs.microsoft.com/windows/uwp/input-and-devices/handle-pointer-input">ポインター入力の処理</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/design/layout/index">カスタム ユーザー操作</a></td>
</tr>
</tbody>
</table>
<h2 id="data">データ</h2>
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
<td align="left"><strong>ローカルのアプリのデータ。</strong> <br><br>アプリに関連する設定とファイルをローカルに保存します。</td>
<td align="left"><strong>openFileOutput</strong> と <strong>openFileInput</strong> を使ってローカル ファイルを保存することができます。 <strong>getSharedPreferences</strong> を使って<strong>共有環境設定ファイル</strong>の設定にアクセスできます。</td>
<td align="left"><strong>NSFileManager</strong> クラスを使ってアクセスする<strong>アプリケーション サポート</strong> ディレクトリにローカル ファイルを保存できます。 <strong>NSUserDefaults</strong> クラスを使って<strong>環境設定</strong>ファイルの設定にアクセスできます。</td>
<td align="left"><strong><a href="https://docs.microsoft.com/uwp/api/Windows.Storage">Windows.Storage</a></strong> クラスは、統一された方法でローカルのデータ記憶域を処理します。 <strong><a href="https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.localsettings">ApplicationData.LocalSettings</a></strong> プロパティを使ってアクセスする <strong><a href="https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationDataContainer">ApplicationDataContainer</a></strong> オブジェクトとして設定を保存します。 <strong><a href="https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.localfolder">ApplicationData.LocalFolder</a></strong> プロパティを使ってアクセスする <strong><a href="https://docs.microsoft.com/uwp/api/windows.storage.storagefolder">StorageFolder</a></strong> オブジェクトにファイルを保存します。<br/><br/><a href="https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data">設定と他のアプリ データを保存して取得する</a></td>
</tr>
<tr class="even">
<td align="left"><strong>ローカル データベース ストレージ。</strong> <br><br>リレーショナル データベースにアプリ データを保存します。該当する場合はオブジェクト リレーショナル マッパー (ORM) も一緒に保存します。</td>
<td align="left"><strong>SQLite</strong> データベースが提供されます。 ORM は組み込みではありません。 <strong>SQLiteDatabase</strong> クラスを使って SQL のクエリが実行されます。</td>
<td align="left"><strong>SQLite</strong> データベースが提供されます。 <strong>CoreData</strong> は組み込みのオブジェクト グラフのフレームワークで、SQLite と一緒に使うことができ、ORM と同様の機能を提供します。</td>
<td align="left"><strong>SQLite</strong> を使ってデータを保存することができます。 <strong><a href="https://docs.microsoft.com/windows/uwp/data-access/entity-framework-7-with-sqlite-for-csharp-apps">Entity Framework</a></strong> は組み込みの ORM 大量のデータ アクセス コードを記述する必要はありませんし、SQL を記述することがなく、データベースを簡単に照会することができます。 <a href="https://docs.microsoft.com/windows/uwp/data-access/sqlite-databases">SQLite ライブラリ</a> で直接、SQL のクエリを実行できます。<br/><br/><a href="https://docs.microsoft.com/windows/uwp/data-access/index">データ アクセス</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>REST 用の HTTP ライブラリにアクセスします。</strong> <br><br>HTTP(S) を使って Web サービスや Web サーバーと通信できる組み込みのライブラリ。<br/></td>
<td align="left">HTTP ライブラリ <strong>HttpURLConnection</strong> と <strong>Volley</strong>。</td>
<td align="left"><strong>NSURLSession</strong>、<strong>NSURLConnection</strong>、<strong>NSURLDownload</strong>。</td>
<td align="left">組み込みの <strong><a href="https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient">HttpClient</a></strong> API を使って、GET、DELETE、PUT、POST、一般的な認証パターン、SSL、Cookie、進行状況情報などの一般的な HTTP 機能にアクセスできます。</td>
</tr>
<tr class="even">
<td align="left"><strong>クラウド バックアップ サービスです。</strong> <br><br>プラットフォームで提供される、アプリのデータ バックアップ サービス。</td>
<td align="left">Android の<strong>バックアップ マネージャー</strong>が Google の <strong>Android バックアップ サービス</strong>でアプリケーション データのバックアップを処理します。</td>
<td align="left">アプリ データなどのバックアップを処理するようにユーザーが <strong>iCloud バックアップ</strong>を構成できます。 iCloud と互換性のある<strong>コア データ</strong>、<strong>iCloud のキー値ストア</strong>、<strong>iCloud ドキュメント ストレージ</strong>を使うアプリ。</td>
<td align="left">ローミング <strong><a href="https://docs.microsoft.com/uwp/api/windows.storage.applicationdata">ApplicationData API</a></strong> (<strong><a href="https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingfolder">RoamingFolder</a></strong> と <a href="https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingsettings"><strong>RoamingSettings</strong></a> を含む) を使って保存したアプリ データは、自動的にクラウドおよびユーザーの他のデバイスと同じ状態に保たれます。 最新に保つ処理は、ユーザーの Microsoft アカウントで実行されます。<br/><br/><a href="https://docs.microsoft.com/windows/uwp/design/app-settings/store-and-retrieve-app-data">アプリのデータのローミングのガイドライン</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>HTTP ファイルをダウンロードします。</strong> <br><br>大小のファイルを HTTP 経由でダウンロードします。</td>
<td align="left"><strong>URLConnection</strong> と <strong>HTTPURLConnection</strong> を使って HTTP と FTP 経由でダウンロードを行います。また、システムの<strong>ダウンロード マネージャー</strong>を使ってバックグラウンドでダウンロードすることもできます。</td>
<td align="left"><strong>NSURLSession</strong> と <strong>NSURLConnection</strong> を使って HTTP と FTP 経由でファイルをダウンロードできます。</td>
<td align="left"><strong><a href="https://docs.microsoft.com/uwp/api/windows.networking.backgroundtransfer">バックグラウンド転送 API</a></strong> で、アプリの中断と接続の切断を考慮し、接続性とバッテリー残量に基づいて調整を行いながら、HTTP(S) と FTP 経由でファイルを確実に転送することができます。 小さなファイルに適した <strong><a href="https://docs.microsoft.com/uwp/api/windows.web.http.httpclient">HttpClient</a></strong> を使用することもできます。<br/><br/><a href="https://docs.microsoft.com/windows/uwp/networking/which-networking-technology">アプリに適したネットワーク テクノロジ</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/networking/background-transfers">バックグラウンド転送</a></td>
</tr>
<tr class="even">
<td align="left"><strong>ソケット。</strong> <br><br>独自のプロトコルを使って他のデバイスと通信するために、低レベルの UDP データグラムおよび TCP ソケットを作成します。</td>
<td align="left"><strong>Socket</strong> クラスが TCP ソケットを提供し、<strong>DatagramSocket</strong> クラスが UDP ソケットを提供します。</td>
<td align="left"><strong>NSStream</strong> と <strong>CFStream</strong> が TCP ソケットを提供し、<strong>CFSocket</strong> が UDP ソケットを提供します。</td>
<td align="left"><strong><a href="https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.DatagramSocket">DatagramSocket</a></strong> クラスを使って UDP データグラム ソケットで通信し、<strong><a href="https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamSocket">StreamSocket</a></strong> クラスを使って TCP または Bluetooth RFCOMM 経由で通信できます。<br/><br/><a href="https://docs.microsoft.com/windows/uwp/networking/networking-basics">ネットワークの基本</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/networking/which-networking-technology">アプリに適したネットワーク テクノロジ</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/networking/sockets">ソケットの概要</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Websocket です。</strong> <br><br>クライアントとサーバーの間の双方向通信を実現し、リアルタイムにデータを転送できるようにします。</td>
<td align="left">Android では、組み込みの WebSocket ライブラリは存在しません。</td>
<td align="left">iOS では、組み込みの WebSocket ライブラリは存在しません。</td>
<td align="left">受信通知付きの小さなメッセージ用には <strong><a href="https://docs.microsoft.com/uwp/api/windows.networking.sockets.messagewebsocket">MessageWebSocket</a></strong> クラス、セクションごとに読み取りできる大きなバイナリ ファイル転送用には <strong><a href="https://docs.microsoft.com/uwp/api/windows.networking.sockets.streamwebsocket">StreamWebSocket</a></strong> を使って、WebSocket をサポートしているサーバーへの接続をセキュリティ保護できます。<br/><br/><a href="https://docs.microsoft.com/windows/uwp/networking/networking-basics">ネットワークの基本</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/networking/which-networking-technology">アプリに適したネットワーク テクノロジ</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/networking/websockets">WebSocket の概要</a></td>
</tr>
<tr class="even">
<td align="left"><strong>OAuth のライブラリ。</strong> <br><br>OAuth のライブラリがサード パーティの OAuth プロバイダーと、プラットフォームに組み込まれているすべてのアカウント管理にアクセスできるようにします。</td>
<td align="left">汎用的な OAuth ライブラリは用意されていません。 Google Play サービスの OAuth 認証用に <strong>GoogleAuthUtil</strong> クラスが提供されています。<br/></td>
<td align="left">汎用的な OAuth ライブラリは用意されていません。 <strong>アカウント フレームワーク</strong>が、デバイスに既に保存されている Facebook や Twitter などのユーザー アカウントにアクセスできるようにします。</td>
<td align="left">汎用的な OAuth ライブラリの <strong><a href="https://docs.microsoft.com/windows/uwp/security/web-authentication-broker">Web 認証ブローカー</a></strong>を使うと、サード パーティの ID プロバイダー サービスに接続することができます。 <strong><a href="https://docs.microsoft.com/windows/uwp/security/credential-locker">資格情報保管ボックス</a></strong>を使うと、ユーザーはログインを保存して複数のデバイスで使うことができます。 <strong><a href="https://docs.microsoft.com/previous-versions/office/developer/onedrive-live-sdk-reference/dn896755(v=office.15)">Microsoft.live</a></strong> 名前空間を使うと、Live SDK の OAuth に簡単にアクセスして Microsoft サービスにアクセスできます。<br/><br/><a href="https://docs.microsoft.com/windows/uwp/security/authentication-and-user-identity">認証とユーザー ID</a><br/><br/><a href="https://docs.microsoft.com/uwp/api/windows.security.authentication.web">Windows.Security.Authentication.Web API ドキュメント</a><br/><br/><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/WebAuthenticationBroker">WebAuthenticationBroker のコード例</a></td>
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
<td align="left"><strong>IDE です。</strong> <br><br>アプリの作成に使うツールセット。</td>
<td align="left"><strong>Android Studio</strong> と <strong>Eclipse</strong>。Google は Android Studio を使うように開発者に推奨しています。</td>
<td align="left"><strong>Xcode</strong></td>
<td align="left"><strong><a href="https://visualstudio.microsoft.com/features/universal-windows-platform-vs">Visual Studio</a></strong> と<strong><a href="https://docs.microsoft.com/visualstudio/designers/creating-a-ui-by-using-blend-for-visual-studio?view=vs-2015">Blend for Visual Studio</a></strong>コード、設計、接続、デバッグ、分析、最適化および UWP アプリをテストする必要があるすべてのツールがあります。 Visual Studio では Windows 10 デバイス用の<strong><a href="https://docs.microsoft.com/windows/uwp/debug-test-perf/test-with-the-emulator">エミュレーター</a></strong>も提供されているため、さまざまなエミュレートされたデバイスでアプリをテストできます。<br/><br/><a href="https://developer.microsoft.com/windows/downloads">UWP 用のダウンロードとツール</a></td>
</tr>
<tr class="even">
<td align="left"><strong>コードの編成します。</strong> <br><br>初期テンプレートから作成されることが多い、アプリの基本的なフォルダー構造。</td>
<td align="left"><strong>AndroidManifest</strong> ファイル、ソース ファイルを含む <strong>java</strong> フォルダー、レイアウトや値などのリソースを持つ <strong>res</strong> フォルダー、Android Studio の <strong>Gradle</strong> ビルド スクリプト、Eclipse の <strong>Ant</strong> ビルド スクリプト。</td>
<td align="left">ソース ファイルと<strong>サポート ファイル</strong>、<strong>Info.plist</strong> ファイル、<strong>Main.storyboard</strong>、<strong>LaunchScreen.storyboard</strong>。 イメージは<strong>アセット ライブラリ</strong>に格納されます。</td>
<td align="left">UWP アプリには、Example.xaml や Example.xaml.cs というアプリ用の XAML とコード ファイル、<strong>Assets フォルダー</strong>のさまざまな画像、<strong>MainPage.xaml</strong> や <strong>MainPage.xaml.cs</strong> などのスタート ページ、マニフェストが含まれています。<br/><br/><a href="https://docs.microsoft.com/windows/uwp/get-started/create-a-hello-world-app-xaml-universal">"Hello, world" アプリを作成する</a></td>
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
<td align="left"><strong>アプリのライフ サイクルです。</strong> <br><br>アプリの起動時、中断時、再開時、終了時にイベントを処理し、アプリケーションの状態を保存/復元したり他のタスクを実行したりする機会を提供します。</td>
<td align="left">アクティビティのそれぞれに、<strong>再開</strong>などの状態を持つ独自の<strong>アクティビティ ライフサイクル</strong>があります。 <strong>ライフ サイクルのコールバック</strong>など<strong>onResume</strong>で実装されて、<strong>アクティビティ クラス</strong>します。</td>
<td align="left"><strong>アプリケーションのライフサイクル</strong>には<strong>中断</strong>などの状態があります。 <strong>applicationDidEnterBackground:</strong> などのメソッドを<strong>アプリケーションのデリゲート オブジェクト</strong>に実装して、状態が変わったときにコードを実行できます。</td>
<td align="left">アプリケーションには<strong>アプリの実行状態</strong> NotRunning、Activated、Running、Suspending、Suspended、Resuming があります。<br/><br/><strong><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.application">Application クラス</a></strong>のメソッド OnLaunched、OnActivated、Suspending、Resuming をアプリに実装して、状態が変わったときにコードを実行できます。<br/><br/><a href="https://docs.microsoft.com/windows/uwp/launch-resume/app-lifecycle">アプリのライフサイクル</a></td>
</tr>
<tr class="even">
<td align="left"><strong>バック グラウンド タスク。</strong> <br><br>バックグラウンド操作を実行し、アプリがフォアグラウンドではなくなったときに動作し続けるタスク。</td>
<td align="left">アプリは、アプリがフォアグラウンドではなくなったときにバックグラウンド操作を実行する<strong>サービス</strong>を起動できます。 サービスに独自の<strong>ライフサイクル</strong>があり、マニフェストに登録されています。</td>
<td align="left"><strong>バックグラウンドでの実行</strong>は、特定の種類のタスクのみで許可されます。<br/><br/>アプリは <strong>UIBackgroundModes</strong> を使って、Info.plist ファイルで<strong>サポートされるバックグラウンド タスク</strong>を宣言します。<br/><br/>システムでは、バックグラウンド タスクがいつどれくらいの長さで実行されるかが制御されます。</td>
<td align="left">バックグラウンド タスクを作成するには、<strong><a href="https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtask">IBackgroundTask</a></strong> インターフェイスを実装してアプリケーション マニフェストにタスクを登録します。 <a href="https://docs.microsoft.com/windows/uwp/launch-resume/run-a-background-task-on-a-timer-">  <strong>タイマー</strong></a>、<a href="https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.systemtriggertype"><strong>システム トリガー</strong></a>、<a href="https://docs.microsoft.com/windows/uwp/launch-resume/use-a-maintenance-trigger"><strong>メンテナンス トリガー</strong></a>でタスクをトリガーするように設定できます。<br/><br/><a href="https://docs.microsoft.com/windows/uwp/launch-resume/support-your-app-with-background-tasks">バックグラウンド タスクによるアプリのサポート</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task">バックグラウンド タスクの作成と登録</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/launch-resume/guidelines-for-background-tasks">バックグラウンド タスクのガイドライン</a></td>
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
<td align="left"><strong>パフォーマンスのベスト プラクティスです。</strong> <br><br>スタートアップ時間が短く、高速で応答性の高い、バッテリー残量に配慮したアプリを構築するためのガイドライン。</td>
<td align="left">Android では<strong>パフォーマンスに関するベスト プラクティス</strong>のトレーニング ガイドが提供されています。</td>
<td align="left">iOS では<strong>パフォーマンスの概要</strong>に関するドキュメントが提供されています。</td>
<td align="left">パフォーマンスの目標の設定、パフォーマンスの測定、メモリ管理、スムーズなアニメーション、効率的なファイル システムへのアクセス、プロファイリングとパフォーマンスのために使用できるツールなどのトピックが記載されているセクションを含む、詳しい<strong><a href="https://docs.microsoft.com/windows/uwp/debug-test-perf/performance-and-xaml-ui">パフォーマンス ガイド</a></strong>をご覧いただけます。</td>
</tr>
<tr class="even">
<td align="left"><strong>レスポンシブ UI の最適化を表示します。</strong> <br><br>表示を最適化してパフォーマンスを向上させます。</td>
<td align="left">階層ビューアー ツールを使った<strong>レイアウト階層</strong>の最適化、<strong>レイアウトの再利用</strong>、<strong>オンデマンドでのビュー</strong>の読み込みはすべて、UI スレッドの応答性を確保し、「アプリケーションが応答しない」(<strong>ANR</strong>) というダイアログを回避するための手法です。<br/></td>
<td align="left"><strong>コア アニメーション</strong> ツールを使って<strong>オフスクリーン レンダリング</strong>、<strong>ブレンド レイヤー</strong>、<strong>ラスタライズ</strong>で UI の問題を修正すると、UI スレッドの応答性を確保しやすくなります。</td>
<td align="left">いくつかの簡単な手順を実行すると、XAML の<strong>マークアップ</strong>と<strong>レイアウト</strong>を簡単に<strong>最適化</strong>できます。 レイアウト構造の簡素化、要素数の最小化、過剰な描画の最小化などの手法を利用できます。 <br/><br/><a href="https://docs.microsoft.com/windows/uwp/debug-test-perf/keep-the-ui-thread-responsive">UI スレッドの応答性の確保</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-xaml-loading">XAML マークアップの最適化</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-your-xaml-layout">XAML レイアウトの最適化</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>スレッド処理します。</strong> <br><br><strong>UI の応答性</strong>を確保し、複数<strong>タスクを並行</strong>して実行するためにスレッド処理を使用します。</td>
<td align="left">スレッド処理を行うには、<strong>Runnable</strong>、<strong>Handler</strong>、<strong>ThreadPoolExecutor</strong> の各クラスと、上位レベルの <strong>AsyncTask</strong> クラスを使います。</td>
<td align="left">スレッド処理を行うには、<strong>NSThread</strong>、<strong>Grand Central Dispatch</strong>、上位レベルの <strong>NSOperation</strong> を使います。</td>
<td align="left">スレッドを使用するには、<strong><a href="https://docs.microsoft.com/uwp/api/windows.system.threading.threadpool.runasync">RunAsync</a></strong> で<strong>作業項目</strong>を<strong>スレッド プール</strong>に送信します。 <strong><a href="https://docs.microsoft.com/uwp/api/windows.system.threading.threadpooltimer.createtimer">CreateTimer</a></strong> でタイマーを使って作業項目を送信したり、<strong><a href="https://docs.microsoft.com/uwp/api/windows.system.threading.threadpooltimer.createperiodictimer">CreatePeriodicTimer</a></strong> で繰り返しの作業項目を作成したりできます。<br/><br/><a href="https://docs.microsoft.com/windows/uwp/threading-async/submit-a-work-item-to-the-thread-pool">スレッド プールへの作業項目の送信</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/threading-async/use-a-timer-to-submit-a-work-item">タイマーを使った作業項目の送信</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/threading-async/create-a-periodic-work-item">定期的な作業項目の作成</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/threading-async/best-practices-for-using-the-thread-pool">スレッド プールを使うためのベスト プラクティス</a></td>
</tr>
<tr class="even">
<td align="left"><strong>非同期プログラミングします。</strong> <br><br>UI スレッドの応答性を確保するために、非同期プログラミング パターンを利用してスレッドが複雑にならないようにします。</td>
<td align="left">独自の非同期クラスを作成するには<strong>スレッド処理を使う必要があります</strong>。 一部の組み込みクラスは非同期です。</td>
<td align="left">独自の非同期クラスを作成するには<strong>スレッド処理を使う必要があります</strong>。 一部の組み込みクラスは非同期です。</td>
<td align="left">独自の API を作成するときにメイン スレッドをブロックしないように、非同期パターンを使うことができます。たとえば、C# と Visual Basic で <strong>async</strong> や <strong>await</strong> を使うことができます。 語が <strong>Async</strong> で終わる非同期の組み込み API を使うことができます。<br/><br/><a href="https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-universal-windows-platform-apps">非同期プログラミング</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/threading-async/call-asynchronous-apis-in-csharp-or-visual-basic">C# または Visual Basic での非同期 API の呼び出し</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>リスト ビューの最適化。</strong> <br><br>大量のデータを表示する必要がある場合にパフォーマンスを低下させることが多い、データの一覧の最適化を支援する組み込みのパターン</td>
<td align="left"><strong>ViewHolder</strong> デザイン パターンを使って複数のビュー参照を避け、再利用可能な UI 要素を使うことができます。</td>
<td align="left"><strong>UITableView</strong> のパフォーマンスを向上させるさまざまな最適化を行うことができます。組み込まれているものはありません。</td>
<td align="left">既定で <strong>UI の仮想化</strong> を提供する <a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview">ListView</a> と <a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.gridview">GridView</a> の各コントロールを使うと、スムーズなパンやスクロール、起動時間の短縮を実現できます。 <a href="https://docs.microsoft.com/dotnet/api/system.collections.ilist?redirectedfrom=MSDN">IList</a> と <a href="https://docs.microsoft.com/dotnet/api/system.collections.specialized.inotifycollectionchanged?redirectedfrom=MSDN">INotifyCollectionChanged</a> をデータ ソースに実装し、<strong>データ仮想化</strong>を行ってパフォーマンスをさらに改善することもできます。<br/><br/><a href="https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-gridview-and-listview">ListView と GridView の UI の最適化</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/debug-test-perf/listview-and-gridview-data-optimization">ListView と GridView のデータ仮想化</a></td>
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
<td align="left"><strong>アプリ内購入します。</strong> <br><br>ユーザーがアプリで購入を行うことができるプラットフォーム機能です。</td>
<td align="left"><strong>アプリ内課金</strong>は Google サービスで提供されています。 製品は <strong>Google Play デベロッパー コンソール</strong>に追加されます。 アプリ内購入を実装するには、<strong>Google Play Billing Library</strong> を使います。</td>
<td align="left">製品は <strong>iTunes Connect</strong> に追加されます。 アプリ内購入を実装するには、<strong>StoreKit</strong> フレームワークを使います。<br/><br/>製品を購入するには、<strong>SKMutablePayment</strong> と <strong>SKPaymentQueue</strong> を使います。</td>
<td align="left">アプリでアプリ内製品の購入を作成するには、<a href="https://docs.microsoft.com/windows/uwp/publish/iap-submissions">アプリに追加してストアに申請</a> します。 <br/><br/>アプリ内購入を定義するには、<strong><a href="https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp">CurrentApp クラス</a></strong>を使います。 <br/><br/><strong><a href="https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync">CurrentApp.RequestProductPurchaseAsync</a></strong> を使うと、ユーザーが製品を購入するための UI を表示できます。<br/><br/><a href="https://docs.microsoft.com/windows/uwp/monetize/enable-in-app-product-purchases">アプリ内製品購入を有効にする</a></td>
</tr>
<tr class="even">
<td align="left"><strong>アプリ内購入の消耗します。</strong> <br><br>購入して使った後でもう一度購入できるアプリ内製品。</td>
<td align="left">通常の購入後に <strong>consumePurchase</strong> で製品を使うと、コンシューマブルな購入が有効になり、購入して使った後でもう一度購入できます。</td>
<td align="left">コンシューマブルな製品は iTunes Connect で<strong>コンシューマブルな製品として定義</strong>されます。</td>
<td align="left">コンシューマブルをサポートするには、ストアに <a href="https://docs.microsoft.com/windows/uwp/publish/enter-iap-properties">申請するときに製品の種類をコンシューマブルと定義</a> します。 次に、ユーザーがコンシューマブルな購入を利用できるようになったら <strong><a href="https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.reportconsumablefulfillmentasync">CurrentApp.ReportConsumableFulfillmentAsync</a></strong> を呼び出します。<br/><br/><a href="https://docs.microsoft.com/windows/uwp/monetize/enable-consumable-in-app-product-purchases">コンシューマブルなアプリ内購入の有効化</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>アプリ内購入をテストします。</strong> <br><br>アプリをストアに配置せずに、アプリ内購入コードをテストできるようにします。</td>
<td align="left"><strong>アプリ内課金サンドボックス</strong>を使ってテストします。</td>
<td align="left"><strong>Sandbox テスター アカウント</strong>を使ってテストします。</td>
<td align="left">CurrentApp の代わりに <strong><a href="https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentappsimulator">CurrentAppSimulator</a></strong> クラスを使うだけでアプリ内購入をテストできます。<br/><br/></td>
</tr>
<tr class="even">
<td align="left"><strong>試用版。</strong> <br><br>簡単にコンテンツを制限したり、アプリの試用版に基づく広告を削除したりできるようにします。</td>
<td align="left">Google Play は<strong>アプリの試用版を公式にサポートしていません</strong>。 試用または広告の削除を行うには、アプリ内購入を作成し、購入の確認時に適切なコード パスを実行します。</td>
<td align="left">App Store は<strong>アプリの試用版を公式にサポートしていません</strong>。 試用または広告の削除を行うには、アプリ内購入を作成し、購入の確認時に適切なコード パスを実行します。</td>
<td align="left">アプリの無料試用版を提供するには、ストアにアプリを申請するときに <strong><a href="https://docs.microsoft.com/windows/uwp/publish/set-app-pricing-and-availability">'無料試用版' オプション</a></strong>を使います。 その後、<strong><a href="https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.licenseinformation.istrial">LicenseInformation.IsTrial</a></strong> を使ってアプリの試用の状態を確認し、それに応じて異なるコード パスを指定します。 アプリの実行中にユーザーが試用の状態を変更した場合に <a href="https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.licenseinformation.licensechanged">LicenseChanged イベント</a> が通知されるように登録できます。<br/><br/><a href="https://docs.microsoft.com/windows/uwp/monetize/exclude-or-limit-features-in-a-trial-version-of-your-app">試用版での機能の除外または制限</a></td>
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
<td align="left"><strong>アダプティブ UI: 柔軟なレイアウトです。</strong> <br><br>柔軟な高さと幅の、さまざまな画面サイズをサポートします。</td>
<td align="left">柔軟なレイアウトにするには、LinearLayout オブジェクトで <strong>wrap_content</strong> と <strong>match_parent</strong> の値を使うか、RelativeLayout オブジェクトを使って配置します。</td>
<td align="left">柔軟なレイアウトにするには、ユニバーサル ストーリーボードと一緒に<strong>アダプティブ モデル</strong>を使い、表示コントローラーに適用される horizontalSizeClass や displayScale などの<strong>制約</strong>や<strong>特性</strong>と一緒に<strong>自動レイアウト</strong>を利用します。</td>
<td align="left">柔軟なレイアウトを作成するには、固定サイズ指定と動的なサイズ指定を組み合わせて<strong>レイアウト プロパティ</strong>と<strong>パネル</strong>を使います。<br/><br/><a href="https://docs.microsoft.com/windows/uwp/layout/layouts-with-xaml">XAML を使ったレイアウトの定義 - レイアウト プロパティとパネル</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/layout/screen-sizes-and-breakpoints-for-responsive-design">レスポンシブ デザイン 101</a></td>
</tr>
<tr class="even">
<td align="left"><strong>アダプティブ UI: レイアウトを調整します。</strong> <br><br>別の対象となるレイアウトを使った、さまざまな画面サイズをサポートします。</td>
<td align="left"><strong>small</strong>、<strong>large</strong>、<strong>ldpi</strong>、<strong>hdpi</strong> などの<strong>構成修飾子</strong>を使ってリソース ディレクトリにさまざまな画面構成用の代替レイアウト ファイルを指定すると、さまざまなサイズと密度の画面にカスタム レイアウトを適用できます。</td>
<td align="left"><strong>iPhone と iPad で別々のストーリーボード</strong>を定義すると、ユニバーサルアプリで異なるデバイス ファミリに合わせてレイアウトを調整できます。</td>
<td align="left">カスタマイズされたレイアウトを作るには、デバイス ファミリごとに<strong>別の XAML マークアップ ファイル</strong>を定義します。<br/><br/><a href="https://docs.microsoft.com/windows/uwp/layout/layouts-with-xaml">XAML を使ったレイアウトの定義 - カスタマイズされたレイアウト</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>アダプティブ UI: 応答性の高いレイアウトです。</strong> <br><br>回転などの画面サイズの変更やウィンドウのサイズの変更へ反応します。</td>
<td align="left"><strong>LinearLayout</strong> や <strong>RelativeLayout</strong> と一緒に柔軟なレイアウトを使うか、異なる方向用の代替レイアウト ファイルを提供すると、レスポンシブ レイアウトを使うことができます。</td>
<td align="left">表示の<strong>サイズ</strong>や<strong>特性</strong>が変化すると、ストーリーボードに指定されている<strong>制約</strong>が適用されます。</td>
<td align="left"><strong><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.visualstate">VisualState</a></strong>、<strong><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.visualstatemanager">VisualStateManager</a></strong>、<strong><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.adaptivetrigger">AdaptiveTrigger</a></strong> を使い、ウィンドウ サイズの変更に基づいて実行時に UI のセクションの再配置、位置の変更、サイズの変更、表示、置換を簡単に行うことができます。<br/><br/><a href="https://docs.microsoft.com/windows/uwp/layout/layouts-with-xaml">XAML を使ったレイアウトの定義 - 表示状態と状態トリガー</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/layout/screen-sizes-and-breakpoints-for-responsive-design">レスポンシブ デザイン 101</a></td>
</tr>
<tr class="even">
<td align="left"><strong>別のデバイスの機能をサポートします。</strong> <br><br>高度なハードウェア機能に対応しながら、それらの機能を備えていないデバイスもサポートします。</td>
<td align="left"><strong>PackageManager.hasSystemFeature</strong> を使ってデバイス機能を実行時にテストすると、ハードウェア固有のコードを実行できるかどうかを判断することができます。</td>
<td align="left">実行時にデバイス機能をテストするために行える<strong>単一のチェックはありません</strong>。各機能を異なる方法でテストして、ハードウェア固有のコードを実行できるかどうかを判断します。</td>
<td align="left">電話、デスクトップ、IoT など、別のデバイス ファミリの追加機能を対象にした<strong>プラットフォーム拡張 SDK</strong> をパッケージに追加できます。 <strong><a href="https://docs.microsoft.com/uwp/api/windows.foundation.metadata.apiinformation">ApiInformation API</a></strong> を使って実行時に型とメンバーの存在をテストし、存在している場合のみ、それらの型とメンバーを呼び出すことができます。</td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>別のデバイスの機能をサポートします。</strong> <br><br>高度なハードウェア機能に対応しながら、それらの機能を備えていないデバイスもサポートします。</td>
<td align="left"><strong>Android サポート ライブラリ</strong>をアプリにパッケージして、以前のバージョンの Android ユーザーがいくつかの新しい API を利用できるようにします。 実行時に API レベルのテストを行うには、<strong>Build.Version.SDK_INT</strong> を使います。</td>
<td align="left">クラスが存在するかどうかを確認する <strong>class</strong> メソッドやクラスのメソッドを確認する <strong>respondsToSelector:</strong> など、標準的なランタイム チェックを使って API が利用できるかどうかを調べます。</td>
<td align="left"><strong><a href="https://docs.microsoft.com/uwp/api/windows.foundation.metadata.apiinformation.isapicontractpresent">ApiInformation.IsApiContractPresent</a></strong> を使って、指定したメジャー番号とマイナー番号の API コントラクトが存在するかどうかを特定できます。 また、<strong><a href="https://docs.microsoft.com/uwp/api/windows.foundation.metadata.apiinformation">ApiInformation API</a></strong> を使って実行時に型とメンバーの存在をテストし、存在している場合のみ、それらの型とメンバーを呼び出すことができます。</td>
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
<td align="left"><strong>アプリのウィジェット</strong>は、ホーム画面に埋め込むことができ、定期的な更新プログラムを受け取ることができる、アプリケーションのビューです。 Android には<strong>バッジ システム</strong>は存在しません。 タイルと同じシステムは存在しません。</td>
  <td align="left">iOS の<strong>ウィジェット</strong>は通知センターに表示され、<strong>アプリの拡張機能</strong>として実装されます。 ローカルまたはリモートの通知に基づいて変更できる数字付きで、<strong>バッジ</strong>をアイコンに追加することもできます。 タイル システムはありません。</td>
<td align="left">アプリにはスタート画面にピン留めできる<strong>タイル</strong>があり、装飾文字と数字付きで選択したテキスト、画像、<strong>バッジ</strong>を表示するために使うことができます。 プッシュ通知または定義済みのスケジュールに基づいて、アプリからタイルのコンテンツを更新することができます。 タイルはアダプティブにでき、表示される場所に従って変更できます。<br/><br/><a href="https://docs.microsoft.com/windows/uwp/controls-and-patterns/tiles-and-notifications-creating-tiles">タイルの作成</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/controls-and-patterns/tiles-and-notifications-create-adaptive-tiles">アダプティブ タイルの作成</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/controls-and-patterns/tiles-and-notifications-choosing-a-notification-delivery-method">通知配信方法の選択</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/controls-and-patterns/tiles-and-notifications-creating-tiles">タイルとバッジのガイドライン</a></td>
</tr>
<tr class="even">
<td align="left"><strong>通知を表示します。</strong> <br><br>表示できる通知の種類。</td>
<td align="left">通知を表示できるのは<strong>通知領域</strong>と<strong>通知ドロワー</strong>で、<strong>ヘッドアップ通知</strong>では小さなフローティング ウィンドウに通知が表示されます。 <strong>PendingIntent</strong> を定義すると、通知に操作を追加できます。</td>
<td align="left">ポップアップ通知は<strong>バナー</strong>または<strong>警告</strong>として表示されます。 <strong>UIMutableUserNotificationAction</strong> で定義する<strong>操作可能な通知</strong>にカスタム操作のボタンを追加できます。</td>
<td align="left"><strong>トースト通知</strong>と呼ばれるアダプティブ ポップアップ通知を作成できます。 視覚的なコンテンツ、ボタンや入力などの<strong>操作</strong>、オーディオを使って、XML でトースト通知を定義できます。<br/><br/><a href="https://docs.microsoft.com/windows/uwp/controls-and-patterns/tiles-and-notifications-adaptive-interactive-toasts">アダプティブ トースト通知と対話型トースト通知</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/controls-and-patterns/tiles-and-notifications-choosing-a-notification-delivery-method">通知配信方法の選択</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/controls-and-patterns/tiles-badges-notifications">トースト通知のガイドライン</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>ローカル通知をスケジュールします。</strong> <br><br>スケジュールされた時刻にアプリから送信されるローカル通知。</td>
<td align="left">通知とアクションは <strong>NotificationCompat.Builder</strong> を使って定義し、<strong>AlarmManager</strong> と <strong>BroadcastReceiver</strong> を使ってアプリ内でスケジュール設定したり処理したりできます。</td>
<td align="left">使用してローカル通知が作成された<strong>UILocalNotification</strong>、スケジュールを設定できますと<b>UILocalNotification.scheduleLocalNotification:<strong>|。使用してトースト通知をスケジュールする</strong> <a href="https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ScheduledToastNotification">ScheduledToastNotification</a><strong>します。タイル通知をアプリから送信するには</strong> <a href="https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileNotification">TileNotification クラス</a><strong> を使い、タイル通知をスケジュール設定するには <a href="https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ScheduledTileNotification">ScheduledTileNotification</a> を使います。<br/><br/><a href="https://docs.microsoft.com/windows/uwp/controls-and-patterns/tiles-and-notifications-adaptive-interactive-toasts">アダプティブ トースト通知と対話型トースト通知</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/controls-and-patterns/tiles-and-notifications-sending-a-local-tile-notification">ローカルのタイル通知を送信</a>| |</strong>プッシュ通知の送信。</b> プッシュ通知のサーバーから送信され、必要に応じてアプリで処理される通知。</td>
<td align="left"><strong>Google Cloud Messaging</strong> が Android のプッシュ通知のサポートを提供します。</td>
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
<td align="left"><strong>キャプチャ メディア。</strong> <br><br>オーディオとビジュアルのコンテンツを記録します。</td>
<td align="left">MediaStore.ACTION_VIDEO_CAPTURE などの<strong>インテント</strong>を使うと、既存のカメラ アプリでメディアをキャプチャできます。 <strong>android.hardware.camera2</strong> または <strong>camera</strong> ライブラリを使うと、カメラのカスタム インターフェイスを実装できます。 <strong>MediaRecorder</strong> API を使うと、オーディオをキャプチャできます。</td>
<td align="left"><strong>UIImagePickerController</strong> を使うと、システム UI でビデオと写真をキャプチャできます。 <strong>AVCaptureSession</strong> などの <strong>AVFoundation</strong> クラスを使うと、カメラへ直接アクセスできます。 <br/><strong>AVAudioRecorder</strong> クラスを使うと、オーディオを録音できます。</td>
<td align="left"><strong><a href="https://docs.microsoft.com/uwp/api/Windows.Media.Capture.CameraCaptureUI">CameraCaptureUI クラス</a></strong>を使うと、組み込みのカメラ UI を使いながら写真とビデオをキャプチャできます。 <strong><a href="https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture">MediaCapture API</a></strong> などの <strong><a href="https://docs.microsoft.com/uwp/api/Windows.Media.Capture">Windows.Media.Capture</a></strong> のクラスを使って、低レベルのカメラ操作でオーディオをキャプチャできます。 <br/><br/><a href="https://docs.microsoft.com/windows/uwp/audio-video-camera/capture-photos-and-video-with-cameracaptureui">CameraCaptureUI を使った写真とビデオのキャプチャ</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/audio-video-camera/capture-photos-and-video-with-mediacapture">MediaCapture を使った写真とビデオのキャプチャ</a></td>
</tr>
<tr class="even">
<td align="left"><strong>メディアの再生。</strong> <br><br>オーディオとビデオのファイルを再生します。</td>
<td align="left"><strong>MediaPlayer</strong> クラスと <strong>AudioManager</strong> クラスを使うと、オーディオとビデオのファイルを再生できます。</td>
<td align="left"><strong>AVKit フレームワーク</strong>、<strong>AVAudioPlayer</strong>、<strong>Media Player Framework</strong> を使うと、オーディオとビデオのファイルを再生できます。</td>
<td align="left"><strong><a href="https://docs.microsoft.com/uwp/api/Windows.Media.Core.MediaSource">MediaSource クラス</a></strong>、<strong><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaelement">MediaElement</a></strong>、<strong><a href="https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer">MediaPlayer</a></strong> クラスを使うと、ローカル ファイルやリモート ファイルなどのソースからオーディオとビデオを再生できます。<br/><br/><a href="https://docs.microsoft.com/windows/uwp/audio-video-camera/media-playback-with-mediasource">MediaSource を使ったメディアの再生</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>メディアを編集します。</strong> <br><br>既存の録音や録画から新しいメディア ファイルを作成し、特殊効果を適用します。</td>
<td align="left"><strong>MediaCodec</strong>、<strong>MediaMuxer</strong>、<strong>android.media.effect</strong> などの低レベルのクラスを使ってコンテンツを編集できます。</td>
<td align="left"><strong>AV Foundation</strong> フレームワークの <strong>AVMutableComposition</strong>、<strong>AVMutableVideoComposition</strong>、<strong>AVMutableAudioMix</strong> などのクラスを使ってコンテンツを編集できます。</td>
<td align="left"><strong><a href="https://docs.microsoft.com/uwp/api/windows.media.editing.mediacomposition">MediaComposition</a></strong>、<strong><a href="https://docs.microsoft.com/uwp/api/windows.media.editing.mediaclip">MediaClip</a></strong> などの <strong><a href="https://docs.microsoft.com/uwp/api/windows.media.editing">Windows.Media.Editing</a></strong> API を使って、オーディオやビデオのファイルからメディア コンポジションを作成できます。 ビデオと画像のオーバーレイの追加、ビデオ クリップの結合、バックグラウンド オーディオの追加、オーディオとビデオの効果の適用を行うことができます。<br/><br/><a href="https://docs.microsoft.com/windows/uwp/audio-video-camera/media-compositions-and-editing">メディア コンポジションと編集</a></td>
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
<td align="left"><strong>センサー。</strong> <br><br>デバイスの動き、位置、環境のプロパティを検出します。</td>
<td align="left"><strong>SensorManager</strong> や <strong>SensorEvent</strong> などのクラスを使うと、<strong>センサー フレームワーク</strong>を使ってハードウェアとソフトウェアのセンサーにアクセスできます。</td>
<td align="left"><strong>Core Motion フレームワーク</strong>を使うと、センサーの生データと処理されたデータにアクセスできます。</td>
<td align="left"><strong><a href="https://docs.microsoft.com/uwp/api/windows.devices.sensors">Windows.Devices.Sensors</a></strong> のクラスを使って、センサーの測定値にアクセスしたり、センサーから新しい測定データを受け取ったときにトリガーされたイベントにアクセスしたりできます。<br/><br/><a href="https://docs.microsoft.com/windows/uwp/devices-sensors/sensors">センサー</a></td>
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
<td align="left"><strong>場所。</strong> <br><br>デバイスの<strong>現在</strong>位置を検出し、<strong>変更</strong>を追跡します。</td>
<td align="left">Google Play サービスの位置情報 API では、<strong>getLastLocation</strong> メソッドと <strong>requestLocationUpdates</strong> メソッドを使って、<strong>Fused Location Provider</strong> が取得した<strong>最後の既知の位置情報</strong>への高レベルのアクセスを提供します。 低レベルのアクセスは、Android ライブラリの <strong>LocationManager</strong> で提供されます。</td>
<td align="left"><strong>Core 場所</strong> <strong>CLLocationManager</strong>クラスを使用してデバイスの場所の監視に使用されます<strong>startUpdatingLocation</strong>標準の場所のサービスと<strong>startMonitoringSignificantLocationChanges</strong>の<strong>大幅な変更</strong>ロケーション サービス。</td>
<td align="left">デバイスの位置情報を追跡するには、<strong><a href="https://docs.microsoft.com/uwp/api/windows.devices.geolocation">Windows.Devices.Geolocation</a></strong> のクラスを使います。 1 回限りの読み取りの場合、<strong><a href="https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.getgeopositionasync">Geolocator.GetGeopositionAsync</a></strong> を使います。 タイマーを使って位置情報を定期的に取得するか、位置が変わったときに通知を受けるには、<strong><a href="https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.positionchanged">Geolocator.PositionChanged</a></strong> を使います。<br/><br/><a href="https://docs.microsoft.com/windows/uwp/maps-and-location/get-location">ユーザーの位置情報の取得</a></td>
</tr>
<tr class="even">
<td align="left"><strong>マップを表示します。</strong> <br><br><strong>組み込みの対話式の地図</strong>を表示し、<strong>関心のあるポイント</strong>を追加します。</td>
<td align="left"><strong>Google Maps Android API</strong> の <strong>GoogleMap</strong>、<strong>MapFragment</strong>、<strong>MapView</strong> クラスで、アプリに地図を埋め込むことができます。 関心のあるポイントを表示するには、<strong>マーカー</strong>と、カスタマイズ可能な <strong>Marker</strong> クラスを使います。</td>
<td align="left">地図を iOS アプリに埋め込むには、<strong>MapKit フレームワーク</strong>の <strong>MKMapView</strong> クラスを使います。 <strong>MKPointAnnotation</strong> などのオブジェクト クラスや <strong>MKPinAnnotationView</strong> などのビュー クラスを使ってアプリに<strong>アノテーション</strong>を追加して、関心のあるポイントを表示できます。</td>
<td align="left">アプリに地図を埋め込むには、2D、3D、Streetside ビューを表示する組み込みの <strong><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol">MapControl</a></strong> XAML コントロールを使います。 プッシュピン、画像、図形を使って関心のあるポイントを追加するには、<strong><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapicon">MapIcon</a></strong>、<strong><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mappolygon">MapPolygon</a></strong>、<strong><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mappolyline">MapPolyline</a></strong> などのクラスを使います。<br/><br/><a href="https://docs.microsoft.com/windows/uwp/maps-and-location/display-maps">2D、3D、Streetside ビューでの地図の表示</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/maps-and-location/display-poi">関心のあるポイント (POI) の地図への表示</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Geofencing 機能します。</strong> <br><br>特定の地理的な地域への進入と退出を監視します。</td>
<td align="left">ジオフェンスを監視するには、Google Play Services SDK の<strong>位置情報サービス</strong>を使います。</td>
<td align="left">地域を監視するには <strong>CLCircularRegion</strong> クラスを使い、登録するには <strong>CLLocationManager.startMonitoringForRegion:</strong> を使います。</td>
<td align="left"><strong><a href="https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geofencing.geofence">Geofence</a></strong> クラスを使ってジオフェンスを作成し、地域への進入と退出など、<strong>監視する状態</strong>を定義できます。 ジオフェンス イベントをフォアグラウンドで処理するには <strong><a href="https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geofencing.geofencemonitor">GeofenceMonitor クラス</a></strong>、バックグラウンドで処理するには <strong><a href="https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.locationtrigger">LocationTrigger バックグラウンド クラス</a></strong>を使います。<br/><br/><a href="https://docs.microsoft.com/windows/uwp/maps-and-location/set-up-a-geofence">ジオフェンスのセットアップ</a></td>
</tr>
<tr class="even">
<td align="left"><strong>ジオコーディングと逆ジオコーディングします。</strong> <br><br>住所を地理的な位置に変換したり (ジオコーディング)、地理的な位置を住所に変換したりします (逆ジオコーディング)。<br/></td>
<td align="left">ジオコーディングや逆ジオコーディングには <strong>Geocoder</strong> クラスを使います。</td>
<td align="left">ジオコーディングには <strong>CLGeocoder</strong> クラスを使います。</td>
<td align="left">ジオコーディングを実行するには、<strong><a href="https://docs.microsoft.com/uwp/api/windows.services.maps">Windows.Services.Maps</a></strong> の <strong><a href="https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder">MapLocationFinder クラス</a></strong>を使います。 ジオコーディングには <strong><a href="https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsasync">FindLocationsAsync</a></strong>、逆ジオコーディングには <strong><a href="https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsatasync">FindLocationsAtAsync</a></strong> を使います。<br/><br/><a href="https://docs.microsoft.com/windows/uwp/maps-and-location/geocoding">ジオコーディングと逆ジオコーディングの実行</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>ルートと方向。</strong> <br><br>地理的な 2 つの場所の間のルート、距離、ルート案内を提供します。</td>
<td align="left">Google は、Android で使うことができる Web サービス <strong>Google Maps Directions API</strong> を提供しています。ただし、SDK は提供されていません。</td>
<td align="left">Map Kit で提供されている <strong>MKDirections</strong> API を使ってルートとルート案内を取得できます。</td>
<td align="left"><strong><a href="https://docs.microsoft.com/uwp/api/windows.services.maps">Windows.Services.Maps</a></strong> の <strong><a href="https://docs.microsoft.com/uwp/api/windows.services.maps.maproutefinder">MapRouteFinder</a></strong> クラスを使って、徒歩ルートや運転ルートを要求できます。 ルートは <strong><a href="https://docs.microsoft.com/uwp/api/windows.services.maps.maproute">MapRoute</a></strong> インスタンスとして返されるため、MapControl に簡単に表示できます。 ルート案内は <strong><a href="https://docs.microsoft.com/uwp/api/windows.services.maps.maproutemaneuver">MapRouteManeuver</a></strong> オブジェクト内に返されます。<br/><br/><a href="https://docs.microsoft.com/windows/uwp/maps-and-location/routes-and-directions">地図へのルートとルート案内の表示</a></td>
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
<td align="left"><strong>別のアプリを起動しています。</strong> <br><br>別のアプリを起動して、必要に応じてリンク、テキスト、写真、ビデオ、ファイルなどのデータを共有します。</td>
<td align="left"><strong>インテント</strong>で<strong>アクション</strong>と必要に応じてデータを定義し、<strong>startActivityForResult</strong> で呼び出すと、<strong>暗黙的インテント</strong>を使って別のアプリを起動できます。<br/></td>
<td align="left"><strong>アプリの拡張機能</strong>を使って、別のアプリにアプリ データへのアクセスを提供できます。 <strong>URL スキーム</strong>を使うと、URL を別のアプリに渡すことができます。</td>
<td align="left">URI が登録されている別のアプリを起動するには <strong><a href="https://docs.microsoft.com/uwp/api/windows.system.launcher.launchuriasync">Launcher.LaunchUriAsync</a></strong> を使います。結果を取得するためにアプリを起動し、起動したアプリから返されたデータを取得するには <strong><a href="https://docs.microsoft.com/uwp/api/windows.system.launcher.launchuriforresultsasync">Launcher.LaunchUriForResultsAsync</a></strong> を使います。 <strong><a href="https://docs.microsoft.com/uwp/api/windows.system.launcher.launchfileasync">Launcher.LaunchFileAsync</a></strong> を使うと、別のアプリに処理するファイルを渡すことができます。<br/><br/><strong>共有コントラクト</strong>を使って、アプリ間でデータを簡単に共有できます。<br/><br/><a href="https://docs.microsoft.com/windows/uwp/launch-resume/launch-default-app">URI に応じた既定のアプリの起動</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/launch-resume/how-to-launch-an-app-for-results">結果を取得するためのアプリの起動</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/launch-resume/launch-the-default-app-for-a-file">ファイルに応じた既定のアプリの起動</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/app-to-app/share-data">データの共有</a></td>
</tr>
<tr class="even">
<td align="left"><strong>アプリを呼び出すことができます。</strong> <br><br>アプリが別のアプリからの要求に応答できるようにします。</td>
<td align="left">アプリで<strong>インテント フィルター</strong>に<strong>インテント処理動作</strong>を登録し、別のアプリからの暗黙的インテントに対応します。</td>
<td align="left"><strong>アプリの拡張機能</strong>をパッケージ化すると、データを他のアプリと共有できます。 アプリで<strong>カスタム URL スキーム</strong>を登録するには、Info.plist で <strong>CFBundleURLTypes</strong> キーを使います。</td>
<td align="left">アプリを <strong>URI スキーム名</strong>の既定のハンドラーとして登録するには、パッケージ マニフェストに<strong><a href="https://docs.microsoft.com/uwp/api/windows.applicationmodel.activation.activationkind#Protocol">プロトコル</a></strong>を登録して <strong><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onactivated">Application.OnActivated</a></strong> イベント ハンドラーを更新し、必要に応じて結果を返すようにします。 同様に、アプリを特定のファイルの種類の既定のハンドラーとして登録するには、パッケージ マニフェストに宣言を追加して <strong><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onfileactivated">Application.OnFileActivated</a></strong> イベントを処理します。<br/><br/>共有コントラクトの要求を処理するには、マニフェストで共有ターゲットとしてアプリを登録し、<strong><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onsharetargetactivated">Application.OnShareTargetActivated</a></strong> イベントを処理します。<br/><br/><a href="https://docs.microsoft.com/windows/uwp/launch-resume/how-to-launch-an-app-for-results">結果を取得するためのアプリの起動</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/launch-resume/handle-file-activation">ファイルのアクティブ化の処理</a><br/><br/><a href="https://docs.microsoft.com/windows/uwp/app-to-app/receive-data">データの受信</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>コピーして貼り付けます。</strong> <br><br>アプリ間でテキストやその他のコンテンツをコピーしたり貼り付けしたりします。</td>
<td align="left"><strong>クリップボード フレームワーク</strong>を使って、<strong>ClipboardManager</strong> と <strong>ClipData</strong> の各クラスでコピーと貼り付けを実装できます。</td>
<td align="left"><strong>UIPasteboard</strong>、<strong>UIMenuController</strong>、<strong>UIResponderStandardEditActions</strong> を使って、コピーとペーストを実装できます。</td>
<td align="left">多くの既定の XAML コントロールは既にコピーと貼り付けをサポートしています。 自分でコピーと貼り付けを実装するには、<strong><a href="https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer">Windows.ApplicationModel.DataTransfer</a></strong> の <strong><a href="https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datapackage">DataPackage</a></strong> と <strong><a href="https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.Clipboard">Clipboard</a></strong> の各クラスを使います。<br/><br/><a href="https://docs.microsoft.com/windows/uwp/app-to-app/copy-and-paste">コピーと貼り付け</a></td>
</tr>
<tr class="even">
<td align="left"><strong>ドラッグ アンド ドロップします。</strong> <br><br>アプリ間でコンテンツをドラッグ アンド ドロップします。</td>
<td align="left">1 つのアプリケーションにドラッグ アンド ドロップを実装するには、<strong>Android ドラッグ/ドロップ フレームワーク</strong>を使います。</td>
<td align="left">iOS では高度なドラッグ アンド ドロップ API は提供されていません。</td>
<td align="left">アプリにドラッグ アンド ドロップを実装すると、アプリとアプリ、デスクトップとアプリ、アプリとデスクトップのドラッグ アンド ドロップ機能を有効にできます。 UIElement クラスにドラッグ アンド ドロップのサポートを実装するには、<strong><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.allowdrop">AllowDrop</a></strong> と <strong><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.candrag">CanDrag</a></strong> の各プロパティ、<strong><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.dragover">DragOver</a></strong> と <strong><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.drop">Drop</a></strong> の各イベントを使います。<br/><br/><a href="https://docs.microsoft.com/windows/uwp/app-to-app/drag-and-drop">ドラッグ アンド ドロップ</a></td>
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
<td align="left"><strong>ソフトウェアの設計パターン。</strong> <br><br>プラットフォーム向けの推奨パターンまたはよく使われるパターン。</td>
<td align="left">正式なパターンは、推奨されていないか、Android の開発用に提供されていません。ただし、ベータ版のデータ バインディング フレームワークでは、<strong>モデル - ビュー - ビューモデル (MVVM)</strong> パターンを現在よりも利用できるようになる可能性があります。 多くのサード パーティの記事やフレームワークでは、<strong>モデル ビュー プレゼンター (MVP)</strong> と <strong>MVVM</strong> を使った方法を推奨しています。</td>
<td align="left"><strong>モデル - ビュー - コントローラー (MVC)</strong> は iOS で使用される一般的なパターンであり、プラットフォームに統合されています。</td>
<td align="left">UWP 用のアプリ作成時、特定のパターンに制限されません。<br/><br/>組み込みの <a href="https://docs.microsoft.com/windows/uwp/data-binding/index">データ バインディング</a> パターンを使って、データの問題と UI の問題を明確に切り離すことができ、後でプロパティ値を更新する UI イベント ハンドラーをコード内に記述することを回避できます。<br/><br/>データ バインディングを拡張して<strong>モデル - ビュー - ビューモデル (MVVM)</strong> パターンに従うことができます。そのためには、<a href="https://archive.codeplex.com/?p=mvvmlight">MVVM Light Toolkit</a> などのサード パーティの MVVM ライブラリを使うか、独自に展開してロジックをコードビハインドから排除します。<br/><br/><a href="https://docs.microsoft.com/previous-versions/msp-n-p/hh848246(v=pandp.10)">MVVM パターン</a><br/><br/><a href="https://github.com/Windows-XAML/Template10/wiki">Template 10 Visual Studio プロジェクト テンプレート</a></td>
</tr>
</tbody>
</table>
