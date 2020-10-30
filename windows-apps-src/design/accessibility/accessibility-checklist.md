---
description: Windows アプリにアクセスできるかどうかを確認するためのチェックリストを提供します。
ms.assetid: BB8399E2-7013-4F77-AF2C-C1A0E5412856
title: アクセシビリティのチェック リスト
label: Accessibility checklist
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: fd16c93b3914987741a486e4f40d4b60e0b274ee
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2020
ms.locfileid: "93032715"
---
# <a name="accessibility-checklist"></a>アクセシビリティのチェック リスト

Windows アプリにアクセスできるかどうかを確認するためのチェックリストを提供します。

ここでは、アプリをアクセシビリティ対応にするときに使用できるチェック リストを示します。

1. コンテンツやアプリの対話型の UI 要素にアクセシビリティ対応の名前 (必須) と説明 (省略可能) を設定します。

    アクセシビリティ対応の名前とは、スクリーン リーダーが UI 要素を読み上げるときに使う短い説明の文字列です。 [**TextBlock**](/uwp/api/Windows.UI.Xaml.Controls.TextBlock) や [**TextBox**](/uwp/api/Windows.UI.Xaml.Controls.TextBox) などの一部の UI 要素では、既定のアクセシビリティ対応の名前としてテキスト コンテンツを昇格させるものがあります。「 [基本的なアクセシビリティ情報](basic-accessibility-information.md#name_from_inner_text)」をご覧ください。

    暗黙的なアクセシビリティ対応の名前として内部テキスト コンテンツを昇格させない画像などのコントロールに対し、明示的にアクセシビリティ対応の名前を設定する必要があります。 フォーム要素のラベルのテキストは、ラベルと入力を関連付けるために、Microsoft UI オートメーション モデルの [**LabeledBy**](/previous-versions/windows/silverlight/dotnet-windows-silverlight/ms591292(v=vs.95)) ターゲットとして使うことができるようにする必要があります。 ユーザーに、通常アクセシビリティ対応の名前に含まれているものよりも詳しい UI のガイダンスを提供する場合は、アクセシビリティ対応の説明やヒントを用意すると、UI の内容がわかりやすくなります。

    詳しくは、「[アクセシビリティ対応の名前](basic-accessibility-information.md#accessible_name)」と「[アクセシビリティ対応の説明](basic-accessibility-information.md)」をご覧ください。

2. キーボード アクセシビリティを実装します。

    * UI 用の既定のタブ インデックスの順序をテストします。 必要に応じてタブ インデックスの順序を調整します。このとき、特定のコントロールの有効化または無効化、一部の UI 要素の [**TabIndex**](/uwp/api/windows.ui.xaml.controls.control.tabindex) の既定値の変更が必要になる場合があります。
    * コンポジット要素に方向キーのナビゲーションをサポートするコントロールを使います。 既定のコントロールの場合、通常、方向キーのナビゲーションは既に実装されています。
    * キーボードのアクティブ化をサポートするコントロールを使います。 既定のコントロール、特に UI オートメーションの [**Invoke**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IInvokeProvider) パターンをサポートするものの場合は、基本的にキーボードのアクティブ化が利用できます。該当するコントロールの説明書をご覧ください。
    * 対話式操作をサポートする UI の一部に対するアクセス キーを設定するか、ショートカット キーを実装します。
    * UI で使うカスタム コントロールで、アクティブ化用に適切な [**AutomationPeer**](/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer) サポートを設定した状態でコントロールが実装され、アクティブ化キー、トラバーサル キー、アクセス キーまたはショートカット キーのサポートに必要なキー処理の上書きが定義されていることを確認します。

    詳細については、「 [キーボード操作](../input/keyboard-interactions.md)」を参照してください。

3. テキストが読み取り可能なサイズであることを確認する

    * Windows にはさまざまなユーザー補助ツールと設定が含まれており、ユーザーはこれを利用して、テキストを読み取るためのニーズや好みに合わせて調整できます。 次の設定があります。
        * 拡大鏡ツール。 UI の選択領域を拡大します。 アプリ内のテキストのレイアウトによって、拡大鏡を使用した読み取りが困難になることはありません。
        * [設定] のグローバルなスケールと解像度の設定 **->システム >表示->スケールとレイアウト** です。 使用できるサイズ変更オプションは、ディスプレイデバイスの機能によって異なります。
        * [設定] の [テキストサイズ] 設定 **->簡単なアクセス >表示** 。 [テキストのサイズを **大きく** する] 設定を調整して、すべてのアプリケーションと画面でのサポートコントロール内のテキストのサイズのみを指定します (すべての UWP テキストコントロールは、カスタマイズまたはテンプレートを使用しないテキストスケーリングエクスペリエンスをサポートします)。
        > [!NOTE]
        > [ **すべてを大きく** する] 設定を使用すると、ユーザーは、通常、テキストとアプリのサイズをプライマリ画面のみで指定できます。

4. テキスト コントラストが適切であること、ハイ コントラスト テーマで要素が正しくレンダリングされること、色が正しく使われていることを確認するため、UI を表示して検証します。

    * 色分析ツールを使って、視覚的なテキストのコントラスト比が 4.5:1 以上であることを検証します。
    * ハイ コントラスト テーマに切り替え、アプリの UI が読みやすく使いやすいことを確認します。
    * UI が情報を伝える唯一の手段として色を使っていないことを確認します。

    詳細については、「 [ハイコントラストテーマ](high-contrast-themes.md) と [アクセス可能なテキスト要件](accessible-text-requirements.md)」を参照してください。

5. アクセシビリティ ツールを実行し、報告された問題に対処して、画面の読み上げを確認します。

    [**Inspect**](/windows/desktop/WinAuto/inspect-objects) などのツールを使ってプログラムによるアクセスを検証し、 [**AccChecker**](/windows/desktop/WinAuto/ui-accessibility-checker) などの診断ツールを実行して一般的なエラーを見つけます。画面の読み上げの確認には、ナレーターを使います。

    詳しくは、「[アクセシビリティ テスト](accessibility-testing.md)」をご覧ください。

6. アプリ マニフェストの設定がアクセシビリティ ガイドラインに準拠しているかどうかを確認します。

7. Microsoft Store でアプリがアクセシビリティ対応であることを宣言します。

    アクセシビリティ サポートの基準を実装したら、Microsoft Store でアプリがアクセシビリティ対応であることを宣言することで、より多くのユーザーにアプリを提供し、さらに良い評価を得ることができます。

    詳しくは、「[ストア内のアクセシビリティ](accessibility-in-the-store.md)」をご覧ください。

## <a name="related-topics"></a>関連トピック  

* [アクセシビリティに対応したテキストの要件](accessible-text-requirements.md)
* [テキストの拡大縮小](../input/text-scaling.md)
* [アクセシビリティ](accessibility.md)
* [アクセシビリティのための設計](./accessibility-overview.md)
* [避ける事項](practices-to-avoid.md)
