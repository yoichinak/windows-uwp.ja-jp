---
description: タッチに最適化されているが、入力デバイス間で機能的に一貫性がある、直感的で特徴的なユーザー操作エクスペリエンスを備えた Windows アプリを作成します。
title: タッチ操作
ms.assetid: DA6EBC88-EB18-4418-A98A-457EA1DEA88A
label: Touch interactions
template: detail.hbs
keywords: タッチ, ポインター, 入力, ユーザーの操作
ms.date: 09/24/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: f173c90d43b7be795b8a87fe14dd3de9d6284da1
ms.sourcegitcommit: da44cb95946440cd06ff36254d42ecefcdd87ce2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2020
ms.locfileid: "93063014"
---
# <a name="touch-interactions"></a>タッチ操作


タッチがユーザーの主な入力方法になるという想定でアプリを設計します。 UWP コントロールを使う場合は、タッチパッド、マウス、ペン/スタイラスをサポートするために追加のプログラミングを行う必要はありません。UWP アプリでは、それらが無料で提供されます。

ただし、タッチ用に最適化された UI が従来の UI よりも常に優れているとは限らないことに留意してください。 どちらの UI にも、テクノロジとアプリに固有の長所と短所があります。 タッチによる最初の UI への移行では、タッチ、タッチパッド、ペン/スタイラス、マウス、キーボード入力の主な違いを理解することが重要です。

> **重要な API** : [**Windows.UI.Xaml.Input**](/uwp/api/Windows.UI.Xaml.Input)、 [**Windows.UI.Core**](/uwp/api/Windows.UI.Core)、 [**Windows.Devices.Input**](/uwp/api/Windows.Devices.Input)


多くのデバイスに、1 本または複数の指 (つまりタッチ接触) を入力として使うことをサポートするマルチタッチ画面が搭載されています。 タッチ接触とその移動は、さまざまなユーザー操作をサポートするタッチ ジェスチャや操作として解釈されます。

Windows アプリには、タッチ入力を処理するためのさまざまなメカニズムが用意されており、ユーザーが自信を持って探索できるイマーシブエクスペリエンスを作成できます。 ここでは、Windows アプリでのタッチ入力の基本的な使用方法について説明します。

タッチ操作には、次の 3 つが必要です。

- タッチ パネル ディスプレイ。
- 1 本以上の指による、そのディスプレイへの直接的な (ディスプレイに近接センサーがあり、ホバー検知がサポートされている場合は近接的な) 接触
- タッチ接触の動き (または、時間のしきい値に基づく動きの欠落)。

タッチ センサーから提供される入力データは、次のように使うことができます。

- 1 つ以上の UI 要素に直接的な操作 (パン、回転、サイズ変更、移動など) を行う物理的なジェスチャとして解釈する。 プロパティ ウィンドウ、ダイアログ ボックス、その他の UI アフォーダンスを通じた要素との対話的操作は、間接的な操作と見なされます。
- マウス、ペンなどの代替の入力方式として見なす。
- 他の入力方法の外観を補完するために使う (ペンで描画したインク ストロークをこするなど)。

タッチ入力では、通常、画面上の要素を直接操作します。 要素は、そのヒット テスト領域内でのタッチ接触をすぐに応答し、削除など、タッチ接触の後続の動きに適切に反応します。

カスタム タッチ ジェスチャと操作は、慎重に設計する必要があります。 直感的で、応答性が高く、見つけやすい必要があり、ユーザーが安心してアプリを操作できる必要があります。

アプリの機能がサポートされているすべての入力デバイスの種類で一貫して公開されていることを確認します。 必要に応じて、キーボード操作に対応するテキスト入力、またはマウスやペンの UI アフォーダンスなど、なんらかの形で間接的な入力モードを用意します。

多くのユーザーにとって従来型入力デバイス (マウスやキーボードなど) が親しみやすく魅力的であることを念頭に置いてください。 従来型入力デバイスは、速度、正確さ、触覚的なフィードバックという点で、タッチ操作より優れている場合があります。

すべての入力デバイスに固有なそれぞれの対話式操作のエクスペリエンスにより、多様な機能や基本設定をサポートし、できるだけ幅広い対象者の関心を引くことで、アプリのユーザーを増やすことができます。

## <a name="compare-touch-interaction-requirements"></a>タッチ操作の要件の比較

次の表に、タッチに最適化された Windows アプリを設計する際に考慮する必要がある入力デバイスの違いをいくつか示します。

<table>
<tbody><tr><th>要素</th><th>タッチ操作</th><th>マウス、キーボード、ペン/スタイラス操作</th><th>Touchpad</th></tr>
<tr><td rowspan="3">Precision</td><td>指先が接触する領域は単一の XY 座標よりも広いので、意図していないコマンドがアクティブ化される可能性が高くなります。</td><td>マウスとペン/スタイラスを使うと正確な XY 座標を指定できます。</td><td>マウスと同じです。</td></tr>
<tr><td>接触する領域の形は移動を通じて変化します。  </td><td>マウスの移動とペン/スタイラスのストロークによって正確な XY 座標を指定できます。 キーボード フォーカスは明示的です。</td><td>マウスと同じです。</td></tr>
<tr><td>ターゲット設定に役立つマウス カーソルはありません。</td><td>マウス カーソル、ペン/スタイラス カーソル、キーボード フォーカスはすべてターゲット設定に役立ちます。</td><td>マウスと同じです。</td></tr>
<tr><td rowspan="3">人体構造</td><td>1 本または複数の指で直線移動を行うのは困難なので、指先の動きは正確さに欠けます。 これは、手関節が曲がることや動きに関係する関節の数が原因です。</td><td>マウスまたはペン/スタイラスを制御する手の物理的な移動距離は、画面上のカーソルの移動距離よりも短いので、マウスまたはペン/スタイラスで直線移動を行う方が簡単です。</td><td>マウスと同じです。</td></tr>
<tr><td>ディスプレイ デバイスのタッチ画面には、指の位置とユーザーのデバイスの持ち方が原因で届きにくくなる領域もあります。</td><td>マウスとペン/スタイラスは画面のどの部分にも届き、キーボードではタブ オーダーによってどのコントロールにもアクセスできます。 </td><td>指の位置と持ち方が問題になることがあります。</td></tr>
<tr><td>オブジェクトは、1 本以上の指先またはユーザーの手で隠れる場合があります。 これをオクルージョンと呼びます。</td><td>間接的な入力デバイスでは、オクルージョンは発生しません。</td><td>マウスと同じです。</td></tr>
<tr><td>オブジェクトの状態</td><td>タッチでは、ディスプレイ デバイスのタッチ画面がタッチされているか (オン) タッチされていないか (オフ) の 2 状態モデルが使われます。 追加の視覚的なフィードバックをトリガーできるホバー状態はありません。</td><td>
<p>マウス、ペン/スタイラス、キーボードはすべて、離した状態 (オフ)、押した状態 (オン)、ホバー状態 (フォーカス) の 3 状態モデルを公開します。</p>
<p>ホバーすると、UI 要素に関連付けられたヒントを調べて参考にすることができます。 ホバーまたはフォーカスを合わせたときの効果によって操作可能なオブジェクトがわかり、ターゲット設定にも役立ちます。 
</p>
</td><td>マウスと同じです。</td></tr>
<tr><td rowspan="2">豊富な操作</td><td>タッチ画面において複数の入力ポイント (指先) で操作できるマルチタッチをサポートします。</td><td>単一の入力ポイントをサポートします。</td><td>タッチと同じです。</td></tr>
<tr><td>タップ、ドラッグ、スライド、ピンチ、回転などのジェスチャによるオブジェクトの直接操作をサポートします。</td><td>マウス、ペン/スタイラス、キーボードは間接的な入力デバイスなので、直接操作はサポートされません。</td><td>マウスと同じです。</td></tr>
</tbody></table>

> [!NOTE]
> 間接的な入力には、25 年以上の改良を経ているという利点があります。 ホバーすると表示されるヒントなどの機能は、タッチパッド、マウス、ペン/スタイラス、キーボード入力での UI の操作を解決するために特別に設計されています。 このような UI 機能は、他のデバイスのユーザー エクスペリエンスを損なうことなく、タッチ入力で充実したエクスペリエンスを提供するために再設計されました。

 

## <a name="use-touch-feedback"></a>タッチのフィードバックの使用

アプリの対話的操作中に視覚的なフィードバックが適切に表示されると、その対話的操作がアプリと Windows プラットフォームの両方でどのように解釈されるかに関する認識、学習、適応に役立ちます。 視覚的なフィードバックの用途は、対話的操作の成功の表示、システム状態の中継、コントロール感の向上、エラーの低減、システムと入力デバイスに関するユーザーの理解の支援、対話的操作の促進などです。

位置に基づく正確性が求められる操作をタッチ入力で行う場合は、視覚的なフィードバックが重要です。 タッチ入力が検出された場所に必ずフィードバックを表示して、アプリとそのコントロールで定義されたカスタム ターゲット設定規則をユーザーが把握できるようにします。


## <a name="targeting"></a>ターゲット

ターゲット設定は、次の要素によって最適化します。

- タッチ ターゲットのサイズ

    明確なサイズのガイドラインによって、ターゲット設定しやすいオブジェクトとコントロールが含まれる快適な UI を備えたアプリになるようにします。

- 接触形状

    指が接触する領域全体によって、意図された可能性が最も高いターゲット オブジェクトを特定します。

- スクラブ

    グループ内の項目 (ラジオ ボタンなど) 間で指をドラッグすると、それらの項目を簡単にターゲット設定し直すことができます。 指を離すと現在の項目がアクティブ化されます。

- 揺らす

    密集した複数の項目 (ハイパーリンクなど) を指で押してスライドせずに前後に揺らすと、それらの項目を簡単にターゲット設定し直すことができます。 オクルージョンが原因で、現在の項目はヒントまたはステータス バーで特定され、指を離すとアクティブ化されます。

## <a name="accuracy"></a>精度

以下を使って、対話的操作が雑な場合に備えて設計します。

- コンテンツの操作時に目的の位置で簡単に停止できるようにするスナップ位置。
- 手をわずかに曲げて動かした場合でも垂直方向または水平方向のパンを実行できる方向 "レール"。詳しくは、「[パンのガイドライン](guidelines-for-panning.md)」をご覧ください。

## <a name="occlusion"></a>オクルージョン

指と手のオクルージョンは、次の要素によって回避します。

- UI のサイズと配置

    UI 要素を十分に大きくして、指先が接触する領域で完全にふさぐことができないようにします。

    メニューとポップアップは、できる限り接触する領域の上に配置します。

- ツールヒント

    指がオブジェクトに接触し続けているときは、ヒントを表示します。 オブジェクトの機能について説明する場合に便利です。 ヒントが呼び出されないようにするには、ユーザーは指先をオブジェクトの外にドラッグします。

    小さいオブジェクトの場合は、ヒントをずらして、指先が接触する領域でふさがれないようにします。 これはターゲット設定に役立ちます。

- 正確さを確保するためのハンドル

    正確さが要求される場合 (テキスト選択など)、正確さを向上させるためにオフセットされる選択ハンドルを用意します。 詳しくは、「[テキストと画像の選択のガイドライン (Windows ランタイム アプリ)](guidelines-for-textselection.md)」をご覧ください。

## <a name="timing"></a>タイミング

直接操作を行うために、時間制限のあるモードの変更を避けます。 直接操作は、オブジェクトに対するリアルタイムで物理的な直接処理をシミュレートします。 オブジェクトは指の動きに合わせて反応します。

一方、時間制限のある操作は、タッチ操作の後に発生します。 通常、時間制限のある操作では、時間、距離、速度などの見えないしきい値に基づいて実行するコマンドが決定されます。 システムで操作が実行されるまで、時間制限のある操作では視覚的なフィードバックは返されません。

直接操作には、時間制限のある対話式操作と比べて、いくつかの利点があります。

- 対話式操作中に視覚的なフィードバックがすばやく返されるので、ユーザーはより集中して、すべてを制御しているという安心感を持って操作できます。
- 直接操作は元に戻すことができるので、安心してシステムを使うことができます。ユーザーは、論理的かつ直観的な方法で操作を簡単にさかのぼることができます。
- オブジェクトに直接影響して実際の対話式操作を模倣する操作は、より直観的で見つけやすく覚えやすい対話式操作です。 わかりにくい抽象的な対話式操作には依存しません。
- 時間制限のある対話式操作は、任意の見えないしきい値に達する必要があるので、実行が難しい場合があります。

また、次の点を考慮することを強くお勧めします。

- 操作は、使う指の数で区別しないでください。
- 複合操作をサポートしてください。 たとえば、ピンチによるズームを行いながら指をドラッグしてパンできるようにします。
- 対話式操作を時間で区別しないでください。 実行にかかる時間に関係なく、同じ対話式操作を行うと同じ結果が得られるようにします。 時間ベースのアクティブ化では、ユーザーは遅延を強いられるので、直接操作のイマーシブの特性が損なわれ、システムの応答性が低く感じられるようになります。

    > [!NOTE]
    > この例外が発生するのは、特定の時間指定の対話を使用して、学習と探索 (たとえば、プレスアンドホールド) を支援する場合です。

- 適切な説明と視覚的な合図を使うと、高度な対話式操作を非常に効果的に使用できます。


## <a name="app-views"></a>アプリ ビュー


アプリのビューのパン/スクロールとズームの設定を使って、ユーザー操作エクスペリエンスを調整します。 アプリ ビューによって、ユーザーがアプリとそのコンテンツにアクセスして操作する方法が決まります。 ビューは、慣性、コンテンツ境界の跳ね返り、スナップ位置などの動作も提供します。

[**ScrollViewer**](/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer) コントロールのパンとスクロールの設定により、ビューのコンテンツがビューポートに収まらない場合に、単一のビュー内でユーザーがどのように移動するかが決まります。 単一のビューは、たとえば雑誌や本のページ、コンピューターのフォルダー構造、ドキュメントのライブラリ、フォト アルバムなどです。

ズームの設定は、光学式ズーム ( [**ScrollViewer**](/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer) コントロールによってサポートされる) と [**Semantic Zoom**](/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) コントロールの両方に適用されます。 セマンティック ズームは、タッチに最適化された手法の 1 つであり、関連する大量のデータやコンテンツを 1 つのビュー内に表示してナビゲートします。 この機能では、2 つの分類モード (ズーム レベル) が使われます。 これは、1 つのビュー内でのパンとスクロールに似ています。 パンとスクロールは、セマンティック ズームと組み合わせて使うことができます。

パン/スクロールとズーム動作を変更するには、アプリのビューとイベントを使います。 こうすることで、ポインターとジェスチャ イベントを処理する場合よりもスムーズな操作エクスペリエンスを提供できます。

アプリビューの詳細については、「 [コントロール、レイアウト、およびテキスト](../basics/index.md)」を参照してください。

## <a name="custom-touch-interactions"></a>カスタム タッチ操作


独自の対話式操作サポートを実装する場合は、ユーザーはアプリの UI 要素を直接操作できる直感的なエクスペリエンスを期待しているということを心に留めておいてください。 プラットフォーム コントロール ライブラリでカスタムの対話式操作をモデル化し、一貫性と見つけやすさを維持することをお勧めします。 これらのライブラリのコントロールでは、標準的な対話式操作、アニメーション化された物理的効果、視覚的フィードバック、アクセシビリティなど、完全なユーザー操作エクスペリエンスが提供されます。 はっきりとした明確に定義されている要件があり、基本的な対話式操作ではシナリオがサポートされない場合のみ、カスタムの対話式操作を作ってください。

カスタマイズされたタッチ サポートを提供するために、さまざまな [**UIElement**](/uwp/api/Windows.UI.Xaml.UIElement) イベントを処理できます。 これらのイベントは、次の 3 つのレベルのアブストラクションにグループ化されます。

- 静的ジェスチャ イベントは、対話式操作が完了した後に発生します。 ジェスチャ イベントには、 [**Tapped**](/uwp/api/windows.ui.xaml.uielement.tapped)、 [**DoubleTapped**](/uwp/api/windows.ui.xaml.uielement.doubletapped)、 [**RightTapped**](/uwp/api/windows.ui.xaml.uielement.righttapped)、 [**Holding**](/uwp/api/windows.ui.xaml.uielement.holding) があります。

    特定の要素のジェスチャイベントを無効にするには、 [**Istapenabled**](/uwp/api/windows.ui.xaml.uielement.istapenabled)、 [**IsDoubleTapEnabled**](/uwp/api/windows.ui.xaml.uielement.isdoubletapenabled)、 [**IsRightTapEnabled**](/uwp/api/windows.ui.xaml.uielement.isrighttapenabled)、および [**IsHoldingEnabled**](/uwp/api/windows.ui.xaml.uielement.isholdingenabled) を **false** に設定します。

- ポインタ [**押さ**](/uwp/api/windows.ui.xaml.uielement.pointerpressed) れたポインターや [**移動**](/uwp/api/windows.ui.xaml.uielement.pointermoved) されたポインターなどのポインターイベントは、ポインターの動きや、プレスイベントとリリースイベントを区別する機能など、各タッチコンタクトの下位レベルの詳細を提供します。

    ポインターは、統合イベント メカニズムを持つ一般的な入力の種類です。 アクティブな入力ソース (タッチ、タッチパッド、マウス、またはペン) についての画面位置などの基本的な情報を公開します。

- [**ManipulationStarted**](/uwp/api/windows.ui.xaml.uielement.manipulationstarted) などの操作ジェスチャ イベントは継続的な対話式操作を示します。 操作ジェスチャ イベントはユーザーが要素にタッチしたときに発生し、ユーザーが指を離すか操作が取り消されるまで続きます。

    操作イベントには、ズーム、パン、回転などのマルチタッチ操作や、ドラッグなどの慣性と速度データを使った操作の場合は、操作イベントを使います。 操作イベントで提供される情報は、実行された操作のフォームを識別するのではなく、位置、変換デルタ、速度などのタッチ データを含みます。 このタッチ データを使って、実行された操作の種類を確認できます。

UWP でサポートされている基本的なタッチ ジェスチャのセットを以下に示します。

| Name           | Type                 | [説明]                                                                            |
|----------------|----------------------|----------------------------------------------------------------------------------------|
| タップ            | 静的ジェスチャ       | 1 本の指で画面をタッチし、その指を上げます。                                            |
| 長押し | 静的ジェスチャ       | 1 本の指で画面をタッチし、そのまま押し続けます。                                      |
| スライド          | 操作ジェスチャ | 1 本または複数の指で画面をタッチし、同じ方向に動かします。                   |
| Swipe          | 操作ジェスチャ | 1 本または複数の指で画面をタッチし、同じ方向に少しだけ動かします。  |
| 回転           | 操作ジェスチャ | 2 本以上の指で画面をタッチし、時計回りまたは反時計回りに動かします。 |
| ピンチ          | 操作ジェスチャ | 2 本以上の指で画面をタッチし、それらの指を近づけていきます。                         |
| Stretch        | 操作ジェスチャ | 2 本以上の指で画面をタッチし、それらの指を離していきます。                           |

 

<!-- mijacobs: Removing for now. We don't have a real page to link to yet. 
For more info about gestures, manipulations, and interactions, see [Custom user interactions](custom-user-input-portal.md).
-->

## <a name="gesture-events"></a>ジェスチャ イベント


個々のコントロールの詳細については、「 [コントロールリスト](../controls-and-patterns/index.md)」を参照してください。

## <a name="pointer-events"></a>ポインター イベント


ポインター イベントは、タッチ、タッチパッド、ペン、マウスなど、さまざまなアクティブな入力ソースによって生成されます (従来のマウス イベントに代わるものです)。

ポインター イベントは、単一の入力ポイント (指、ペンの先端、マウス カーソル) に基づき、速度ベースの操作をサポートしません。

ポインター イベントと、関連するイベント引数の一覧を示します。

| イベント/クラス                                                       | [説明]                                                   |
|----------------------------------------------------------------------|---------------------------------------------------------------|
| [**PointerPressed**](/uwp/api/windows.ui.xaml.uielement.pointerpressed)             | 1 本の指で画面をタッチしたときに発生します。               |
| [**PointerReleased**](/uwp/api/windows.ui.xaml.uielement.pointerreleased)           | その同じタッチによる接触が離れたときに発生します。                |
| [**PointerMoved**](/uwp/api/windows.ui.xaml.uielement.pointermoved)                 | 画面上でポインターがドラッグされたときに発生します。         |
| [**PointerEntered**](/uwp/api/windows.ui.xaml.uielement.pointerentered)             | ポインターが要素のヒット テスト領域内に入ったときに発生します。 |
| [**PointerExited**](/uwp/api/windows.ui.xaml.uielement.pointerexited)               | ポインターが要素のヒット テスト領域から出たときに発生します。  |
| [**PointerCanceled**](/uwp/api/windows.ui.xaml.uielement.pointercanceled)           | タッチによる接触が異常に失われたときに発生します。               |
| [**PointerCaptureLost**](/uwp/api/windows.ui.xaml.uielement.pointercapturelost)     | 別の要素でポインター キャプチャが行われたときに発生します。    |
| [**PointerWheelChanged**](/uwp/api/windows.ui.xaml.uielement.pointerwheelchanged)   | マウスホイールのデルタ値が変化したときと、タッチパッドが pinched たときに発生します。         |
| [**PointerRoutedEventArgs**](/uwp/api/Windows.UI.Xaml.Input.PointerRoutedEventArgs) | すべてのポインター イベントのデータを提供します。                         |

 

次の例に、 [**PointerPressed**](/uwp/api/windows.ui.xaml.uielement.pointerpressed)、 [**PointerReleased**](/uwp/api/windows.ui.xaml.uielement.pointerreleased)、 [**PointerExited**](/uwp/api/windows.ui.xaml.uielement.pointerexited) の各イベントを使って [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) オブジェクトに対するタップ操作を処理する方法を示します。

最初に、XAML (Extensible Application Markup Language) で、`touchRectangle` という名前の [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) コントロールが作成されます。

```XAML
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Rectangle Name="touchRectangle"
           Height="100" Width="200" Fill="Blue" />
</Grid>
```
次に、 [**PointerPressed**](/uwp/api/windows.ui.xaml.uielement.pointerpressed)、 [**PointerReleased**](/uwp/api/windows.ui.xaml.uielement.pointerreleased)、 [**PointerExited**](/uwp/api/windows.ui.xaml.uielement.pointerexited) の各イベントのリスナーが指定されます。

```cpp
MainPage::MainPage()
{
    InitializeComponent();

    // Pointer event listeners.
    touchRectangle->PointerPressed += ref new PointerEventHandler(this, &MainPage::touchRectangle_PointerPressed);
    touchRectangle->PointerReleased += ref new PointerEventHandler(this, &MainPage::touchRectangle_PointerReleased);
    touchRectangle->PointerExited += ref new PointerEventHandler(this, &MainPage::touchRectangle_PointerExited);
}
```

```cs
public MainPage()
{
    this.InitializeComponent();

    // Pointer event listeners.
    touchRectangle.PointerPressed += touchRectangle_PointerPressed;
    touchRectangle.PointerReleased += touchRectangle_PointerReleased;
    touchRectangle.PointerExited += touchRectangle_PointerExited;
}
```

```vb
Public Sub New()

    ' This call is required by the designer.
    InitializeComponent()

    ' Pointer event listeners.
    AddHandler touchRectangle.PointerPressed, AddressOf touchRectangle_PointerPressed
    AddHandler touchRectangle.PointerReleased, AddressOf Me.touchRectangle_PointerReleased
    AddHandler touchRectangle.PointerExited, AddressOf touchRectangle_PointerExited

End Sub
```

最後に、 [**PointerPressed**](/uwp/api/windows.ui.xaml.uielement.pointerpressed) イベント ハンドラーが [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) の [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) と [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) を大きくし、 [**PointerReleased**](/uwp/api/windows.ui.xaml.uielement.pointerreleased) と [**PointerExited**](/uwp/api/windows.ui.xaml.uielement.pointerexited) の各イベント ハンドラーは **Height** と **Width** を開始値に戻します。

```cpp
// Handler for pointer exited event.
void MainPage::touchRectangle_PointerExited(Object^ sender, PointerRoutedEventArgs^ e)
{
    Rectangle^ rect = (Rectangle^)sender;

    // Pointer moved outside Rectangle hit test area.
    // Reset the dimensions of the Rectangle.
    if (nullptr != rect)
    {
        rect->Width = 200;
        rect->Height = 100;
    }
}

// Handler for pointer released event.
void MainPage::touchRectangle_PointerReleased(Object^ sender, PointerRoutedEventArgs^ e)
{
    Rectangle^ rect = (Rectangle^)sender;

    // Reset the dimensions of the Rectangle.
    if (nullptr != rect)
    {
        rect->Width = 200;
        rect->Height = 100;
    }
}

// Handler for pointer pressed event.
void MainPage::touchRectangle_PointerPressed(Object^ sender, PointerRoutedEventArgs^ e)
{
    Rectangle^ rect = (Rectangle^)sender;

    // Change the dimensions of the Rectangle.
    if (nullptr != rect)
    {
        rect->Width = 250;
        rect->Height = 150;
    }
}
```

```cs
// Handler for pointer exited event.
private void touchRectangle_PointerExited(object sender, PointerRoutedEventArgs e)
{
    Rectangle rect = sender as Rectangle;

    // Pointer moved outside Rectangle hit test area.
    // Reset the dimensions of the Rectangle.
    if (null != rect)
    {
        rect.Width = 200;
        rect.Height = 100;
    }
}
// Handler for pointer released event.
private void touchRectangle_PointerReleased(object sender, PointerRoutedEventArgs e)
{
    Rectangle rect = sender as Rectangle;

    // Reset the dimensions of the Rectangle.
    if (null != rect)
    {
        rect.Width = 200;
        rect.Height = 100;
    }
}

// Handler for pointer pressed event.
private void touchRectangle_PointerPressed(object sender, PointerRoutedEventArgs e)
{
    Rectangle rect = sender as Rectangle;

    // Change the dimensions of the Rectangle.
    if (null != rect)
    {
        rect.Width = 250;
        rect.Height = 150;
    }
}
```

```vb
' Handler for pointer exited event.
Private Sub touchRectangle_PointerExited(sender As Object, e As PointerRoutedEventArgs)
    Dim rect As Rectangle = CType(sender, Rectangle)

    ' Pointer moved outside Rectangle hit test area.
    ' Reset the dimensions of the Rectangle.
    If (rect IsNot Nothing) Then
        rect.Width = 200
        rect.Height = 100
    End If
End Sub

' Handler for pointer released event.
Private Sub touchRectangle_PointerReleased(sender As Object, e As PointerRoutedEventArgs)
    Dim rect As Rectangle = CType(sender, Rectangle)

    ' Reset the dimensions of the Rectangle.
    If (rect IsNot Nothing) Then
        rect.Width = 200
        rect.Height = 100
    End If
End Sub

' Handler for pointer pressed event.
Private Sub touchRectangle_PointerPressed(sender As Object, e As PointerRoutedEventArgs)
    Dim rect As Rectangle = CType(sender, Rectangle)

    ' Change the dimensions of the Rectangle.
    If (rect IsNot Nothing) Then
        rect.Width = 250
        rect.Height = 150
    End If
End Sub
```

## <a name="manipulation-events"></a>操作イベント


アプリで複数の指を使った操作や速度データを必要とする操作をサポートする必要がある場合は、操作イベントを使います。

操作イベントを使うと、ドラッグ、ズーム、長押しなどの操作を検出できます。

> [!NOTE]
> タッチパッドは操作イベントを発生させません。 代わりに、タッチパッド入力に対してポインターイベントが発生します。

操作イベントと、関連するイベント引数の一覧を示します。

| イベント/クラス                                                                                               | [説明]                                                                                                                               |
|--------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------|
| [**ManipulationStarting イベント**](/uwp/api/windows.ui.xaml.uielement.manipulationstarting)                                   | 操作プロセッサを初めて作成するときに発生します。                                                                                  |
| [**ManipulationStarted イベント**](/uwp/api/windows.ui.xaml.uielement.manipulationstarted)                                     | 入力デバイスが [**UIElement**](/uwp/api/Windows.UI.Xaml.UIElement) に対する操作を開始すると発生します。                                            |
| [**ManipulationDelta イベント**](/uwp/api/windows.ui.xaml.uielement.manipulationdelta)                                         | 操作中に入力デバイスが位置を変更したときに発生します。                                                                      |
| [**ManipulationInertiaStarting イベント**](/uwp/api/windows.ui.xaml.uielement.manipulationinertiastartingevent)                | 操作中に入力デバイスが [**UIElement**](/uwp/api/Windows.UI.Xaml.UIElement) オブジェクトとのコンタクトを失ったときと慣性が開始したときに発生します。 |
| [**ManipulationCompleted イベント**](/uwp/api/windows.ui.xaml.uielement.manipulationcompleted)                                 | [**UIElement**](/uwp/api/Windows.UI.Xaml.UIElement) に対する操作と慣性が完了すると発生します。                                          |
| [**ManipulationStartingRoutedEventArgs**](/uwp/api/Windows.UI.Xaml.Input.ManipulationStartingRoutedEventArgs)               | [**ManipulationStarting**](/uwp/api/windows.ui.xaml.uielement.manipulationstarting) イベントのデータを指定します。                                         |
| [**ManipulationStartedRoutedEventArgs**](/uwp/api/Windows.UI.Xaml.Input.ManipulationStartedRoutedEventArgs)                 | [**ManipulationStarted**](/uwp/api/windows.ui.xaml.uielement.manipulationstarted) イベントのデータを指定します。                                           |
| [**ManipulationDeltaRoutedEventArgs**](/uwp/api/Windows.UI.Xaml.Input.ManipulationDeltaRoutedEventArgs)                     | [**ManipulationDelta**](/uwp/api/windows.ui.xaml.uielement.manipulationdelta) イベントのデータを指定します。                                               |
| [**ManipulationInertiaStartingRoutedEventArgs**](/uwp/api/Windows.UI.Xaml.Input.ManipulationInertiaStartingRoutedEventArgs) | [**ManipulationInertiaStarting**](/uwp/api/windows.ui.xaml.uielement.manipulationinertiastarting) イベントのデータを指定します。                           |
| [**ManipulationVelocities**](/uwp/api/Windows.UI.Input.ManipulationVelocities)                                              | 操作の実行速度を指定します。                                                                                         |
| [**ManipulationCompletedRoutedEventArgs**](/uwp/api/Windows.UI.Xaml.Input.ManipulationCompletedRoutedEventArgs)             | [**ManipulationCompleted**](/uwp/api/windows.ui.xaml.uielement.manipulationcompleted) イベントのデータを指定します。                                       |

 

ジェスチャは、一連の操作イベントで構成されます。 ユーザーが画面をタッチしたときなど、各ジェスチャは [**ManipulationStarted**](/uwp/api/windows.ui.xaml.uielement.manipulationstarted) イベントから始まります。

次に、1 つ以上の [**ManipulationDelta**](/uwp/api/windows.ui.xaml.uielement.manipulationdelta) イベントが発生します。 たとえば、画面をタッチして画面上で指をドラッグした場合です。 最後に、対話的操作が完了すると [**ManipulationCompleted**](/uwp/api/windows.ui.xaml.uielement.manipulationcompleted) イベントが発生します。

> [!NOTE]
> タッチスクリーンモニターがない場合は、マウスとマウスホイールインターフェイスを使用して、シミュレーターで操作イベントコードをテストできます。

 

次の例に、 [**ManipulationDelta**](/uwp/api/windows.ui.xaml.uielement.manipulationdelta) イベントを使って、 [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) に対するスライド操作を処理し、画面上でオブジェクトを移動する方法を示します。

まず、XAML で [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) と [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) が 200 の `touchRectangle` という名前の [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) を作成します。

```XAML
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Rectangle Name="touchRectangle"
               Width="200" Height="200" Fill="Blue" 
               ManipulationMode="All"/>
</Grid>
```

次に、 [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) を移動するための `dragTranslation` という名前のグローバルな [**TranslateTransform**](/uwp/api/Windows.UI.Xaml.Media.TranslateTransform) を作成します。 [**Rectangle** で **ManipulationDelta**](/uwp/api/windows.ui.xaml.uielement.manipulationdelta) イベント リスナーが指定され、`dragTranslation` が [**Rectangle** の **RenderTransform**](/uwp/api/windows.ui.xaml.uielement.rendertransform) に追加されます。

```cpp
// Global translation transform used for changing the position of 
// the Rectangle based on input data from the touch contact.
Windows::UI::Xaml::Media::TranslateTransform^ dragTranslation;
```

```cs
// Global translation transform used for changing the position of 
// the Rectangle based on input data from the touch contact.
private TranslateTransform dragTranslation;
```

```vb
' Global translation transform used for changing the position of 
' the Rectangle based on input data from the touch contact.
Private dragTranslation As TranslateTransform
```

```cpp
MainPage::MainPage()
{
    InitializeComponent();

    // Listener for the ManipulationDelta event.
    touchRectangle->ManipulationDelta += 
        ref new ManipulationDeltaEventHandler(
            this, 
            &MainPage::touchRectangle_ManipulationDelta);
    // New translation transform populated in 
    // the ManipulationDelta handler.
    dragTranslation = ref new TranslateTransform();
    // Apply the translation to the Rectangle.
    touchRectangle->RenderTransform = dragTranslation;
}
```

```cs
public MainPage()
{
    this.InitializeComponent();

    // Listener for the ManipulationDelta event.
    touchRectangle.ManipulationDelta += touchRectangle_ManipulationDelta;
    // New translation transform populated in 
    // the ManipulationDelta handler.
    dragTranslation = new TranslateTransform();
    // Apply the translation to the Rectangle.
    touchRectangle.RenderTransform = this.dragTranslation;
}
```

```vb
Public Sub New()

    ' This call is required by the designer.
    InitializeComponent()

    ' Listener for the ManipulationDelta event.
    AddHandler touchRectangle.ManipulationDelta,
        AddressOf testRectangle_ManipulationDelta
    ' New translation transform populated in 
    ' the ManipulationDelta handler.
    dragTranslation = New TranslateTransform()
    ' Apply the translation to the Rectangle.
    touchRectangle.RenderTransform = dragTranslation

End Sub
```

最後に、 [**ManipulationDelta**](/uwp/api/windows.ui.xaml.uielement.manipulationdelta) イベント ハンドラーで、 [**TranslateTransform**](/uwp/api/Windows.UI.Xaml.Media.TranslateTransform) を [**Delta**](/uwp/api/windows.ui.xaml.input.manipulationdeltaroutedeventargs.delta) プロパティで使って [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) の位置を更新します。

```cpp
// Handler for the ManipulationDelta event.
// ManipulationDelta data is loaded into the
// translation transform and applied to the Rectangle.
void MainPage::touchRectangle_ManipulationDelta(Object^ sender,
    ManipulationDeltaRoutedEventArgs^ e)
{
    // Move the rectangle.
    dragTranslation->X += e->Delta.Translation.X;
    dragTranslation->Y += e->Delta.Translation.Y;
    
}
```

```cs
// Handler for the ManipulationDelta event.
// ManipulationDelta data is loaded into the
// translation transform and applied to the Rectangle.
void touchRectangle_ManipulationDelta(object sender,
    ManipulationDeltaRoutedEventArgs e)
{
    // Move the rectangle.
    dragTranslation.X += e.Delta.Translation.X;
    dragTranslation.Y += e.Delta.Translation.Y;
}
```

```vb
' Handler for the ManipulationDelta event.
' ManipulationDelta data Is loaded into the
' translation transform And applied to the Rectangle.
Private Sub testRectangle_ManipulationDelta(
    sender As Object,
    e As ManipulationDeltaRoutedEventArgs)

    ' Move the rectangle.
    dragTranslation.X = (dragTranslation.X + e.Delta.Translation.X)
    dragTranslation.Y = (dragTranslation.Y + e.Delta.Translation.Y)

End Sub
```

## <a name="routed-events"></a>ルーティング イベント

ここに記載されたポインター イベント、ジェスチャ イベント、操作イベントはすべて、 *ルーティング イベント* として実装されます。 つまりこのイベントは、最初にイベントを発生したオブジェクト以外のオブジェクトによって処理される可能性があります。 [**UIElement**](/uwp/api/Windows.UI.Xaml.UIElement) の親コンテナーや、アプリのルート [**Page**](/uwp/api/Windows.UI.Xaml.Controls.Page) などのオブジェクト ツリーの一連の親は、元の要素が存在しなくても、これらのイベントを処理することを選択できます。 逆に、イベントを処理するどのオブジェクトも、親要素に達しないように、処理済みイベントをマークできます。 ルーティングイベントの概念と、ルーティングイベントのハンドラーの作成方法に対する影響の詳細については、「 [イベントとルーティングイベントの概要](/previous-versions/windows/apps/hh758286(v=win.10))」を参照してください。

> [!Important]
> スクロール可能なビュー (ScrollViewer や ListView など) で [**uielement**](/uwp/api/Windows.UI.Xaml.UIElement)のポインターイベントを処理する必要がある場合は、uielement を呼び出して、ビュー内の要素の操作イベントのサポートを明示的に無効にする必要があり [ます。](/uwp/api/windows.ui.xaml.uielement.canceldirectmanipulations) ビューで操作イベントを再び有効にするには、 [UIElement. TryStartDirectManipulation ()](/uwp/api/windows.ui.xaml.uielement.trystartdirectmanipulation)を呼び出します。

## <a name="dos-and-donts"></a>Dos といいえ

- 期待される主な入力方法としてタッチ操作を使うアプリを設計します。
- あらゆる種類 (タッチ、ペン、スタイラス、マウスなど) の操作に対する視覚的なフィードバックを提供します。
- タッチ ターゲットのサイズ、接触形状、スクラブ、揺らす操作を調整してターゲット設定を最適化します。
- スナップ位置と方向 "レール" を使って精度を最適化します。
- 密集した UI 項目のタッチの精度を高めるためにヒントとハンドルを用意します。
- できるだけ時間制限のある操作を使わないようにします (適切な使用例: 長押し)。
- できる限り、操作の区別に使われた数の指は使わないようにします。

## <a name="related-articles"></a>関連記事

- [ポインター入力の処理](handle-pointer-input.md)
- [入力デバイスの識別](identify-input-devices.md)

### <a name="samples"></a>サンプル

- [基本的な入力のサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
- [待機時間が短い入力のサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
- [ユーザー操作モードのサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode)
- [フォーカスの視覚効果のサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals)

### <a name="archive-samples"></a>サンプルのアーカイブ

- [入力: デバイス機能のサンプルに関するページ](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Input%20Device%20capabilities%20sample%20(Windows%208))
- [入力: XAML ユーザー入力イベントのサンプルに関するページ](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20XAML%20user%20input%20events%20sample)
- [XAML のスクロール、パン、ズームのサンプル](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Universal%20Windows%20app%20samples/111487-Universal%20Windows%20app%20samples/XAML%20scrolling%2C%20panning%2C%20and%20zooming%20sample)
- [入力: GestureRecognizer によるジェスチャと操作](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20Gestures%20and%20manipulations%20with%20GestureRecognizer)
