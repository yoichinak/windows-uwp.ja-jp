---
Description: カスタム入力方式エディター (IME) を開発します。ユーザーは、標準の QWERTY キーボードで簡単に表現できない言語のテキストを入力できます。
title: IME (Input Method Editor) の要件
label: Input Method Editor (IME) requirements
template: detail.hbs
keywords: ime、Input Method Editor、入力、対話
ms.date: 07/24/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 5a34c15826bff757b7c4277b87cc5fed53a6f109
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89160006"
---
# <a name="custom-input-method-editor-ime-requirements"></a>IME (カスタム入力方式エディター) の要件

これらのガイドラインと要件は、カスタム入力方式エディター (IME) を開発して、標準の QWERTY キーボードでは簡単に表現できない言語のテキストをユーザーが入力できるようにするために役立ちます。

Ime の概要については、「 [Input Method Editor (ime)](input-method-editors.md)」を参照してください。

## <a name="default-ime"></a>既定の IME

ユーザーは任意のアクティブな Ime を選択できます (**設定 > 時刻 & 言語-> 言語-> 優先 > 言語**-言語パック-オプション)。優先する言語の既定の ime になります。

:::image type="content" source="images/IMEs/ime-preferred-languages.png" alt-text="優先する言語設定":::

[言語オプション] の [設定] 画面で、優先する言語の既定のキーボードを選択します。

:::image type="content" source="images/IMEs/ime-preferred-languages-keyboard.png" alt-text="優先言語キーボード":::

> [!Important]
> カスタム IME の既定のキーボードを設定するために、レジストリに直接書き込むことはお勧めしません。

## <a name="compatibility-requirements"></a>互換性の要件

カスタム IME の基本的な互換性要件は次のとおりです。

### <a name="ime-must-be-compatible-with-windows-apps"></a>IME は Windows アプリと互換性がある必要があります

[テキストサービスフレームワーク (TSF)](/windows/win32/tsf/text-services-framework)を使用して、ime を実装します。 以前は、入力サービスに対して [入力方式マネージャー (IMM32)](/windows/win32/intl/input-method-manager) を使用するオプションがありました。 これで、入力方式マネージャー (IMM32) を使用して実装された Ime がシステムによってブロックされるようになりました。

アプリが起動すると、TSF は、ユーザーが現在選択している IME の IME DLL を読み込みます。 IME が読み込まれると、アプリと同じアプリコンテナーの制限が適用されます。 たとえば、アプリがマニフェストでインターネットアクセスを要求していない場合、IME はインターネットにアクセスできません。 この動作により、Ime がセキュリティコントラクトに違反しないようにします。

TSF は、アプリと IME の仲介です。 TSF は入力イベントを IME に伝え、ユーザーが文字を選択した後、IME から入力文字を受け取ります。

この動作は、以前のバージョンの Windows と同じですが、Windows アプリに読み込まれると、IME の機能に影響します。

IME が Windows アプリとデスクトップアプリ間で異なる機能または UI を提供する必要がある場合は、TSF によって読み込まれる DLL が、読み込まれているアプリの種類を確認するようにしてください。 IME で [Itfthreadmグリーン x:: GetActiveFlags](/windows/win32/api/msctf/nf-msctf-itfthreadmgrex-getactiveflags) メソッドを呼び出し、TF_TMF_IMMERSIVEMODE フラグをチェックします。これにより、結果に応じて、ime で異なるアプリケーションロジックがトリガーされます。

Windows アプリでは、表テキストサービス (TTS) の Ime はサポートされていません。

> [!NOTE]
> TTS Ime を生成するための一部のツールでは、Windows によってマルウェアとしてマークされた Ime が生成されます。

### <a name="ime-must-be-compatible-with-the-system-tray"></a>IME はシステムトレイと互換性がある必要があります

IME アイコンをホストする言語バーはありません。 代わりに、現在の入力オプションを示す入力インジケーターがシステムトレイに表示されます。 入力インジケーターには、現在実行中の IME を示す IME ブランド化アイコンのみが表示されます。 また、ime をオンまたはオフにするなど、最も一般的に使用される IME モードスイッチをユーザーが実行するために、IME ブランド化アイコンの左側に表示される IME モードアイコンが1つあります。

入力インジケーターには、互換性のある Ime に対してのみ、IME ブランドアイコンとモードアイコンが表示されます。 互換性のない Ime には、ブランドアイコンとモードアイコンがシステムトレイに表示されません。 代わりに、IME のブランド化アイコンではなく、言語の省略形が入力インジケーターに表示されます。

IME アイコンは、スタンドアロンの .ico ファイルではなく、DLL または EXE ファイルに格納します。 IME アイコンのデザインは、次の「UI デザインガイドライン」セクションで説明されているガイドラインに従う必要があります。

### <a name="ime-branding-icon"></a>IME のブランド化アイコン

入力インジケーターは、ime がシステムに登録されたときに ime によって定義されたリソース ID を使用して、ime DLL から IME ブランド化アイコンを取得します。

### <a name="ime-mode-icon"></a>IME モードアイコン

Ime モードアイコンを表示するには、システムトレイに表示されている入力インジケーターに依存している必要がある場合があります。 この場合、IME は GUID_LBI_INPUTMODE を使用して IME モードアイコンを入力インジケーターに渡します。

IME モードアイコンをシステムトレイの入力インジケーターに渡すと、IME モードアイコンの既定のサイズは16x16 ピクセルになります。 UI のスケーリングは DPI に従います。

IME モードアイコンを UAC の入力インジケーター (セキュリティで保護されたデスクトップのユーザーアカウント制御) に渡すと、IME モードアイコンの既定のサイズは20x20 ピクセルになります。 UAC の [IME モードの UI スケーリング] アイコンは、PPI に従います。

## <a name="ime-must-work-in-app-container"></a>アプリコンテナーで IME を使用する必要がある

一部の IME 関数は、アプリコンテナーで影響を受けます。

- **Dictionary ファイル** -多くの場合、ime には、ユーザー入力を特定の文字にマップするための読み取り専用の辞書ファイルがあります。 アプリコンテナー内からこれらのファイルにアクセスするには、プログラムファイルまたは Windows ディレクトリの下に IME を配置する必要があります。 既定では、これらのディレクトリはアプリコンテナーから読み取ることができるため、これらの場所に格納されているディクショナリファイルには、Ime がアクセスできます。 IME で辞書ファイルを別の場所に保存する必要がある場合は、辞書ファイルの [Access Control リスト (ACL)](/windows/win32/secauthz/access-control-lists) を明示的に操作して、アプリコンテナーからのアクセスを許可する必要があります。
- **インターネット更新** -IME でインターネットからのデータを使用してディクショナリを更新する必要がある場合、インターネットアクセスは常に許可されていないため、アプリコンテナー内では信頼できません。 代わりに、IME は、インターネットからのデータで辞書ファイルを更新する独立したデスクトッププロセスを実行する必要があります。
- **オンザフライの学習** -Ime がインターネットにアクセス可能なアプリコンテナーで実行されている場合、ime が通信できるエンドポイントに制限はありません。 この場合、IME はクラウドサーバーを使用して、オンザフライのラーニングサービスを提供できます。 一部の Ime は、ユーザーが入力している間にユーザー入力をダウンロードしてアップロードします。 インターネットアクセスはアプリコンテナーでは保証されないため、常に許可されるとは限りません。
- **プロセス間** での情報の共有-ime は、異なるアプリコンテナー内のアプリ間で、ユーザーの入力設定に関するデータを共有する必要がある場合があります。 Web サービスを使用して、アプリ間でデータを共有します。

> [!Important]
> アプリコンテナーのセキュリティ規則を回避しようとすると、IME はマルウェアとして扱われ、ブロックされる可能性があります。

## <a name="ime-and-touch-keyboard"></a>IME とタッチキーボード

IME は、候補ウィンドウの UI とその他の UI 要素がタッチキーボードの下に描画されないようにする必要があります。 タッチキーボードは、すべてのアプリよりも高い z オーダーバンドで表示され、IME UI は、アクティブになっているアプリと同じ z オーダーバンドで表示されます。 その結果、タッチキーボードは、IME UI を重複したり非表示にしたりすることができます。 ほとんどの場合、タッチキーボードを考慮して、アプリのウィンドウのサイズを変更する必要があります。 アプリのサイズが変更されない場合でも、IME は [Inputpane](/windows/win32/api/shobjidl_core/nn-shobjidl_core-iframeworkinputpane) API を使用してタッチキーボードの位置を取得できます。 IME は、 [Location](/windows/win32/api/shobjidl_core/nf-shobjidl_core-iframeworkinputpane-location) プロパティを照会するか、タッチキーボードの Show イベントと Hide イベントのハンドラーを登録します。 Show イベントは、現在タッチキーボードが表示されている場合でも、ユーザーが編集フィールドをタップするたびに発生します。 Ime は、この API を使用して、タッチキーボードで使用される画面領域を取得します。これにより、IME は、候補 (または他の) UI を描画したり、タッチキーボードの下に描画しないように Ime UI をリフローしたりします。

### <a name="specifying-the-preferred-touch-keyboard-layout"></a>優先するタッチキーボードレイアウトの指定

IME は、使用するタッチキーボードレイアウトを指定でき、IME はタッチに最適化されたレイアウトで動作するように有効になっています。 この機能は、韓国語、日本語、簡体字中国語、繁体字中国語の入力言語の Ime に限定されています。

タッチキーボードでサポートされているレイアウトは7つあります。3つはクラシックレイアウト、4つはタッチ最適化レイアウトです。 クラシックレイアウトは、物理キーボードのように見え、動作します。

次の3つのクラシックレイアウトは、繁体字中国語を異なる形式で入力するためのものです。

- 発音に基づく入力
- Changjie 入力
- Dayi 入力

従来のレイアウトに加えて、韓国語、日本語、簡体字中国語、および繁体字中国語の各入力言語に対して、タッチに最適化されたレイアウトが1つあります。

この機能を使用するには、IME で [ITfFnGetPreferredTouchKeyboardLayout](/windows/win32/api/ctffunc/nn-ctffunc-itffngetpreferredtouchkeyboardlayout) インターフェイスを実装する必要があります。これは、テキストサービスフレームワーク [Itff tionprovider](/windows/win32/api/msctf/nn-msctf-itffunctionprovider) API を使用して ime によってエクスポートされます。

IME で ITfFnGetPreferredTouchKeyboardLayout インターフェイスがサポートされていない場合、IME を使用すると、タッチキーボードによって表示される言語の既定のクラシックレイアウトが生成されます。

IME で、いずれかのクラシックレイアウトを優先レイアウトとして設定する必要がある場合、ITfFnGetPreferredTouchKeyboardLayout および Itff Tionprovider インターフェイスをサポートするだけでなく、IME 側で追加の作業を行う必要はありません。 ただし、タッチに最適化されたレイアウトを操作するには、IME で追加の作業が必要です。これについては、次のセクションで説明します。

### <a name="touch-optimized-layout"></a>タッチに最適化されたレイアウト

韓国語、日本語、簡体字中国語、および繁体字中国語入力言語用のタッチに最適化されたキーボードでは、IME でのさまざまなレイアウトと、IME オフ変換モードが表示されます。 タッチキーボードには、IME 変換モードをオンまたはオフに設定するためのキーがありますが、キーボードの IME モードも、エディットコントロール間でフォーカスが変更されると変更される可能性があります。

日本語、簡体字中国語、および繁体字中国語入力言語のタッチに最適化されたキーボードには、候補ページ間を移動するために IME が使用するキー (キー) が含まれています。 日本語および簡体字中国語の場合、タッチに最適化されたレイアウトで候補ページキーが表示されます。 繁体字中国語の場合は、前の候補ページと次の候補ページに個別のキーがあります。

これらのキーが押されると、タッチキーボードは [SendInput](/windows/win32/api/winuser/nf-winuser-sendinput) 関数を呼び出して、次の Unicode プライベート使用領域文字をフォーカスされたアプリケーションに送信します。これは、IME がインターセプトして操作できます。

- **次のページ (0xF003)** -候補ページキーが、日本語と簡体字中国語のタッチ最適化キーボードで押されたとき、または、繁体字中国語用タッチ最適化キーボードで次のページキーが押されたときに送信されます。
- **前のページ (0xF004)** -[候補ページ] キーが、日本語と簡体字中国語のタッチ最適化キーボードの Shift キーと同時に押されたとき、または前のページキーが繁体字中国語のタッチ最適化キーボードで押されたときに送信されます。

これらの文字は、Unicode 入力として送信されます。 次の段落では、テキストサービスフレームワーク IME によって受信される主要なイベントシンク通知中に文字情報を抽出する方法について詳しく説明します。 これらの文字値は、どのヘッダーファイルでも定義されていないため、コードで定義する必要があります。

キーボード入力を受け取るには、IME がキーイベントシンクとして登録されている必要があります。 SendInput 関数を使用して生成された Unicode 入力の場合、 [ITfKeyEventSink](/windows/win32/api/msctf/nn-msctf-itfkeyeventsink) コールバックの WPARAM パラメーター (OnKeyDown、OnKeyUp、OnTestKeyDown、Ontestkeydown) は常に仮想キー VK_PACKET を含み、文字を直接識別しません。

文字にアクセスするには、次の呼び出しシーケンスを実装します。

```cpp
// Keyboard state
BYTE abKbdState[256];
if (!GetKeyboardState(abKbdState))
{
   return 0;
}

// Map virtual key to character code
WCHAR wch;
if (ToUnicode(VK_PACKET, 0, abKbdState, &wch, 1, 0) == 1)
{
   return wch;
}
```

## <a name="ime-search-integration"></a>IME 検索の統合

検索機能を使用して検索機能をユーザーに提供し、検索ウィンドウと統合します。

:::image type="content" source="images/IMEs/ime-search-pane.png" alt-text="検索ウィンドウと IME の候補":::<br/>
*検索ウィンドウと IME の候補*

検索ウィンドウは、ユーザーがすべてのアプリで検索を実行できるようにするための一元的な場所です。 IME ユーザーの場合、Windows は、互換性のある Ime を Windows と統合して効率と使いやすさを向上させるための一意の検索エクスペリエンスを提供します。

検索と互換性のある IME を入力したユーザーには、次の2つの主な利点があります。

- IME と検索エクスペリエンスのシームレスな相互作用。 IME 候補は検索ボックスの下にインラインで表示され、occluding 検索候補は表示されません。 ユーザーはキーボードを使用して、検索ボックス、IME 変換候補、および検索候補の間をシームレスに移動できます。
- アプリケーションによって提供される、関連する結果や提案にすばやくアクセスできます。 アプリは、現在のすべての変換候補にアクセスして、より関連性の高い候補を提供します。 検索候補の優先順位を向上させるために、変換は関連性の高い順にアプリに与えられます。 ユーザーは、[発音] を入力するだけで、変換せずに必要な結果を検索して選択します。

IME は、次の条件を満たしている場合、統合された検索エクスペリエンスと互換性があります。

- Windows スタイルシェルと互換性があります。
- TSF UILess mode Api を実装します。 詳細については、「 [UILess モードの概要](/windows/win32/tsf/uiless-mode-overview)」を参照してください。
- TSF 検索統合 Api、 [ITfFnSearchCandidateProvider](/windows/win32/api/ctffunc/nn-ctffunc-itffnsearchcandidateprovider) 、および [ITfIntegratableCandidateListUIElement](/windows/win32/api/ctffunc/nn-ctffunc-itfintegratablecandidatelistuielement)を実装します。

検索ウィンドウでアクティブ化すると、互換性のある IME は UIless モードで配置され、UI を表示できません。 代わりに、前のスクリーンショットに示したように、変換候補を Windows に送信して、インライン候補リストコントロールに表示します。

また、IME は、現在の検索を実行するために使用する候補を送信します。 これらの候補は、変換の候補と同じにすることも、検索用にカスタマイズすることもできます。

適切な検索候補は、次の条件を満たしています。

- プレフィックスが重複していません。 たとえば、北京大学 and北京は、もう一方のプレフィックスであるため、冗長です。
- 重複候補はありません。 重複候補は、結果のフィルター処理に役立たないので、検索には役立ちません。 たとえば、北京大学に一致する結果も北京に一致します。
- 予測候補はありません。変換のみです。 たとえば、ユーザーが「be」と入力した場合、IME は北を候補として返すことができますが、北京大学は返しません。 通常、予測候補には制限があります。

条件を満たしていない Ime は、他のコントロールと同じように検索の表示と互換性がありません。また、UI の統合と検索候補を利用することもできません。 アプリは、ユーザーの作成が完了した後にのみクエリを受信します。

検索コントラクトをサポートするアプリがクエリを受け取ると、クエリイベントには "queryTextAlternatives" 配列が含まれます。この配列には、最も関連性の高いものから最も関連性が低いものから順に順位付けされた既知の代替候補がすべて含まれています。

代替手段が提供されている場合、アプリは各代替手段をクエリとして処理し、任意の代替候補に一致するすべての結果を返す必要があります。 アプリは、ユーザーが同時に複数のクエリを発行した場合と同じように動作し、結果を提供するサービスに対して "or" クエリを本質的に発行します。 パフォーマンス上の考慮事項として、アプリでは、最も関連性の高い代替手段のうちの 5 ~ 20 までの一致を制限することがよくあります。

## <a name="ui-design-guidelines"></a>UI デザインのガイドライン

すべての Ime は、「 [Windows アプリの設計とコーディング](../index.md)」で説明されているユーザーエクスペリエンスガイドラインに従う必要があります。

### <a name="dont-use-sticky-windows"></a>固定ウィンドウを使用しない

IME ウィンドウは、必要なときにのみ表示され、常に表示されるはずではありません。 ユーザーが入力する必要がない場合、IME ウィンドウは表示されません。 IME ウィンドウを全画面表示ウィンドウにすることはできません。 IME ウィンドウが互いに重ならないようにします。 Windows は、Windows スタイルで設計し、UI のスケーリングに従う必要があります。

### <a name="ime-icons"></a>IME アイコン

IME アイコンには、ブランドアイコンとモードアイコンの2種類があります。 すべての IME アイコンは、黒と白の色のみで設計する必要があります。 新しい IME アイコンは、システムトレイアイコンの glyphic の外観から借用します。 このスタイルは、すべての言語で、familial の外観を補完するために使用できるように作成されています。また、相互に区別することもできます。

IME アイコンのファイル形式は ICO です。 次のアイコンのサイズを指定する必要があります。

- 16x16 ピクセル
- 20x20 ピクセル
- 24 x 24 ピクセル
- 32x32 ピクセル
- 40 x 40 ピクセル
- 48 x 48 ピクセル

すべての解像度で、アルファチャネルを含む32ビットのアイコンが提供されていることを確認します。

IME ブランドアイコンは、最新のタイプフェイスでレンダリングされるタイポグラフィグリフが配置されている白いボックスで定義されます。 それぞれの定義グリフは、各言語チームによって選択されます。 グリフが黒です。 ボックスには、1ピクセルの外側のストロークが50% の不透明度で黒で表示されます。 "新しい" バージョンは、ボックスの左上に丸い角が付いています。

IME モードのアイコンは、50% の不透明度で黒で1ピクセルの外側のストロークを含む、最新のタイプフェイスのホワイトタイポグラフィグリフによって定義されます。

| アイコン | 説明 |
| --- | --- |
| :::image type="content" source="images/IMEs/ime-brand-icon-traditional-chinese.png" alt-text="繁体字中国語 ChangeJie のサンプル IME ブランドアイコン。"::: | 繁体字中国語 ChangeJie のサンプル IME ブランドアイコン。 |
| :::image type="content" source="images/IMEs/ime-brand-icon-traditional-chinese-new.png" alt-text="繁体字中国語の新しい ChangeJie のサンプル IME ブランドアイコン。"::: | 繁体字中国語 ChangeJie のサンプル IME ブランドアイコン。 |
| :::image type="content" source="images/IMEs/ime-mode-icon-chinese.png" alt-text="中国語モードアイコン"::: | IME モードの例のアイコン。 |

### <a name="owned-window"></a>所有ウィンドウ

候補となる UI を表示するには、IME がウィンドウを所有ウィンドウに設定する必要があります。これにより、現在実行中のアプリで表示できるようになります。 [Itfcontextview:: GetWnd](/windows/win32/api/msctf/nf-msctf-itfcontextview-getwnd)メソッドを使用して、所有するウィンドウを取得します。 GetWnd がエラーまたは NULLHWND を返した場合は、 [GetFocus](/windows/win32/api/msctf/nf-msctf-itfthreadmgr-getfocus) 関数を呼び出します。

`if (FAILED(pView->GetWnd(&parentWndHandle)) || (parentWndHandle == nullptr)) { parentWndHandle = GetFocus(); }`

### <a name="ime-candidate-window-interaction-with-light-dismiss-surfaces"></a>IME 候補ウィンドウとライト消去サーフェイスの相互作用

ポップアップウィンドウの無視モデルは、ユーザーが簡単にこのようなウィンドウを閉じることができるため、"ライトの破棄" と呼ばれます。 Ime を Windows 相互作用モデルで適切に機能させるには、IME ウィンドウを明るい破棄モデルに含める必要があります。

ライトの破棄モデルに参加するには、 [Notifywinevent](/windows/win32/api/winuser/nf-winuser-notifywinevent) 関数または同様の関数を使用して、3つの新しい Windows イベントを IME で発生させる必要があります。 これらの新しいイベントは次のとおりです。

- **EVENT_OBJECT_IME_SHOW** -IME が表示されたときに、このイベントを発生させます。
- **EVENT_OBJECT_IME_HIDE** -IME が非表示のときにこのイベントを発生させます。
- **EVENT_OBJECT_IME_CHANGE** -IME がサイズを移動または変更したときに、このイベントを発生させます。

### <a name="declaring-compatibility"></a>互換性の宣言

Ime は、 [ITfCategoryMgr:: RegisterCategory](/windows/win32/api/msctf/nf-msctf-itfcategorymgr-registercategory)を使用して、ime のカテゴリ GUID_TFCAT_TIPCAP_IMMERSIVESUPPORT を登録することによって、互換性があることを宣言します。

### <a name="set-the-default-ime-mode-to-on"></a>既定の IME モードをオンに設定します。

Ime に適した UX を提供しています。

## <a name="dpi-scaling-support-for-desktop-applications"></a>DPI スケーリングによるデスクトップアプリケーションのサポート

拡張された DPI スケーリングサポートにより、各デスクトッププロセスの宣言された DPI 認識レベルに対してクエリを実行し、UI を拡張する必要があるかどうかを判断できます。 マルチモニターのシナリオでは、各モニターのさまざまな DPI 設定に合わせて UI が適切にスケールされます。

IME は各アプリケーションのプロセスのコンテキストで実行されるため、IME の DPI 認識レベルを宣言しないでください。 これにより、IME は現在のプロセスの DPI 認識レベルで実行されます。

すべての IME UI 要素に、実行中のプロセスの UI 要素とのスケーリングパリティがあることを確認するには、異なる DPI 値に適切に応答する必要があります。

> [!NOTE]
> 新しいデスクトップアプリケーションとの同等性を確保するために、IME はモニターごとの DPI 認識をサポートする必要がありますが、認識のレベルを宣言しないでください。 各シナリオで、システムによって適切なスケーリング要件が決定されます。

デスクトップアプリケーションの DPI スケーリングサポート要件の詳細については、「 [高 dpi](/windows/win32/hidpi/high-dpi-desktop-application-development-on-windows)」を参照してください。

## <a name="ime-installation"></a>IME のインストール

Microsoft Visual Studio を使用して IME を構築する場合は、Flexera Software のようなサードパーティのインストーラーを使用して、IME のインストールエクスペリエンスを作成します。

次の手順は、InstallShield を使用して、IME DLL のセットアッププロジェクトを作成する方法を示しています。

- Visual Studio をインストールします。
- Visual Studio を起動します。
- [ **ファイル** ] メニューの [ **新規作成** ] をポイントし、[ **プロジェクト**] をクリックします。 [ **新しいプロジェクト** ] ダイアログボックスが開きます。
- 左側のウィンドウで、[ **テンプレート] > 他のプロジェクトの種類 > セットアップと配置**] に移動し、[ **InstallShield の制限付きエディションを有効に**する] をクリックして、[ **OK**] をクリックします。 インストール手順に従います。
- Visual Studio を再起動します。
- IME ソリューション (.sln) ファイルを開きます。
- ソリューションエクスプローラーで、ソリューションを右クリックして [ **追加**] をポイントし、[ **新しいプロジェクト**] をクリックします。 [ **新しいプロジェクトの追加** ] ダイアログボックスが表示されます。
- 左側のツリービューコントロールで、[ **テンプレート] > その他のプロジェクトの種類 > [InstallShield 限定版**] の順に移動します。
- 中央のウィンドウで、[ **InstallShield の制限付きエディションプロジェクト**] をクリックします。
- [ **名前** ] テキストボックスに「setupime」と入力し、[ **OK]** をクリックします。
- [ **Project Assistant** ] ダイアログボックスで、[ **アプリケーション情報**] をクリックします。
- 会社名とその他のフィールドを入力します。
- [ **アプリケーションファイル**] をクリックします。
- 左側のウィンドウで、 **[INSTALLDIR]** フォルダーを右クリックし、[ **新しいフォルダー**] を選択します。 フォルダーに "プラグイン" という名前を指定します。
- [ **ファイルの追加**] をクリックします。 IME DLL に移動し、[ **プラグイン** ] フォルダーに追加します。 IME 辞書に対してこの手順を繰り返します。
- IME DLL を右クリックし、[ **プロパティ**] を選択します。 [ **プロパティ** ] ダイアログボックスが表示されます。
- [ **プロパティ** ] ダイアログボックスで、[ **COM & .net の設定** ] タブをクリックします。
- [ **登録の種類**] で [ **自己登録** ] を選択し、[ **OK**] をクリックします。
- ソリューションをビルドします。 IME DLL が構築され、InstallShield によって、ユーザーが Windows に IME をインストールできるようにする setup.exe ファイルが作成されます。

独自のインストールエクスペリエンスを作成するには、 [Itfinputprocessorprofilemgr:: RegisterProfile](/windows/win32/api/msctf/nf-msctf-itfinputprocessorprofilemgr-registerprofile) メソッドを呼び出して、インストール時に IME を登録します。 レジストリエントリを直接記述しないでください。

インストール後すぐに IME を使用する必要がある場合は、 [InstallLayoutOrTip](/windows/win32/tsf/installlayoutortip) を呼び出して、psz パラメーターに次の形式を使用し、ユーザーが有効にした入力方法に ime を追加します。

`<LangID 1>:{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}`

## <a name="ime-accessibility"></a>IME のアクセシビリティ

Ime がアクセシビリティ要件に準拠し、ナレーターを操作できるようにするには、次の規則を実装します。 候補リストをアクセス可能にするには、Ime がこの規則に従う必要があります。

- 候補リストには、変換候補の一覧または予測候補の一覧の "IME_Prediction_Window" の **UIA_AutomationIdPropertyId** が "IME_Candidate_Window" と同じである必要があります。
- 候補リストが表示され、表示されなくなると、型 **UIA_MenuOpenedEventId** と **UIA_MenuClosedEventId**のイベントがそれぞれ生成されます。
- 現在選択されている候補が変更されると、候補リストによって **UIA_SelectionItem_ElementSelectedEventId**が発生します。 選択した要素には、 **TRUE**に等しい**UIA_SelectionItemIsSelectedPropertyId**プロパティが設定されている必要があります。
- 候補リストの各項目の **UIA_NamePropertyId** は、候補の名前である必要があります。 必要に応じて、 **UIA_HelpTextPropertyId**を通じて候補を明確にするための追加情報を提供できます。

## <a name="related-topics"></a>関連トピック

- [Input Method Editor (IME)](input-method-editors.md)
- [ITfFnGetPreferredTouchKeyboardLayout](/windows/win32/api/ctffunc/nn-ctffunc-itffngetpreferredtouchkeyboardlayout)
- [ITfCompartmentEventSink](/windows/win32/api/msctf/nn-msctf-itfcompartmenteventsink)
- [Itfthreadmグリーン x:: GetActiveFlags](/windows/win32/api/msctf/nf-msctf-itfthreadmgrex-getactiveflags)
- [ITfContextView:: GetWnd](/windows/win32/api/msctf/nf-msctf-itfcontextview-getwnd)
- [TF_INPUTPROCESSORPROFILE](/windows/win32/api/msctf/ns-msctf-tf_inputprocessorprofile)
- [ITfFnSearchCandidateProvider](/windows/win32/api/ctffunc/nn-ctffunc-itffnsearchcandidateprovider)
- [ITfIntegratableCandidateListUIElement](/windows/win32/api/ctffunc/nn-ctffunc-itfintegratablecandidatelistuielement)
- [SendInput](/windows/win32/api/winuser/nf-winuser-sendinput)
- [アクセシビリティ](../accessibility/accessibility.md)