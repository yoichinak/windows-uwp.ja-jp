---
Description: このトピックでは、テキスト、イメージ、およびコントロールを選択して操作するための新しい Windows UI について説明します。また、Windows アプリで新しい選択と操作のメカニズムを使用するときに考慮する必要があるユーザーエクスペリエンスのガイドラインを提供します。
title: テキストと画像の選択
ms.assetid: d973ffd8-602e-47b5-ab0b-4b2a964ec53d
label: Selecting text and images
template: detail.hbs
keywords: キーボード, テキスト, 入力, ユーザーの操作
ms.date: 09/24/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 179eeffd014cb614fb5314826068d9690fc29807
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/24/2020
ms.locfileid: "91216975"
---
# <a name="selecting-text-and-images"></a>テキストと画像の選択


この記事では、テキスト、画像、コントロールの選択と操作について説明し、アプリでこれらのメカニズムを使うときに考慮する必要があるユーザー エクスペリエンスのガイドラインを示します。

> **重要な API**: [**Windows.UI.Xaml.Input**](/uwp/api/Windows.UI.Xaml.Input)、[**Windows.UI.Input**](/uwp/api/Windows.UI.Input)
 


## <a name="dos-and-donts"></a>推奨と非推奨


-   独自のグリッパー UI を実装する場合は、フォント グリフを使います。 グリッパーは、システム全体で利用できる 2 つの Segoe UI フォントを組み合わせたものです。 フォント リソースを使うと、さまざまな dpi におけるレンダリングの問題が軽減され、さまざまな UI 表示スケール プラトーに対応できます。 独自のグリッパーを実装する場合は、どのグリッパーにも次の UI の特性を持たせてください。

    -   円の図形
    -   背景に隠れず表示される
    -   一貫したサイズ
-   グリッパー UI が収まるように、選択可能なコンテンツの周囲に余白を設けます。 パンとスクロールに対応していない領域でテキストを選択できる場合は、テキスト領域の左右に 1/2 のグリッパー余白を設け、テキスト領域の上下に 1 つのグリッパーの高さを設けます (次の図を参照)。 こうすることで、グリッパー UI 全体がユーザーに表示されるため、端に基づく他の UI が誤って操作されるのを防ぐことができます。

    ![テキスト選択グリッパーの余白](images/textselection-gripper-margins.png)

-   対話式操作中はグリッパー UI を非表示にします。 対話式操作中にグリッパーによって領域がふさがれないようにします。 これは、グリッパーが指で完全に隠れない場合や、テキスト選択グリッパーが複数存在する場合に有効です。 これにより、子ウィンドウを表示しているときは、視覚的なアーティファクトが取り除かれます。

-   コントロール、ラベル、画像、独自のコンテンツなどの UI 要素は選択できないようにします。 通常、Windows アプリケーションでは、特定のコントロール内でのみ選択できます。 ボタン、ラベル、ロゴなどのコントロールは選択できません。 選択がアプリにとって問題になるかどうかを評価し、問題になる場合は、選択を禁止する UI 領域を特定します。 

## <a name="additional-usage-guidance"></a>その他の使い方のガイダンス


テキストの選択と操作は、タッチ操作で導入されたユーザー エクスペリエンスの問題の影響を特に受けやすくなっています。 マウス、ペン/スタイラス、キーボード入力は非常に細かく制御されます。1 回のマウスのクリックまたはペン/スタイラスの接触は 1 ピクセルにマッピングされ、キーは押されるか、放されます。 タッチ入力は細かく制御されません。指先の表面全体を、画面上の特定の x-y の位置にマッピングしてテキスト キャレットを正確に配置することは困難です。

**考慮事項と推奨事項**

Windows の言語フレームワークによって公開されるビルトイン コントロールを利用して、選択や操作の動作など、完全なプラットフォームのユーザー操作エクスペリエンスを実現するアプリを作成してください。 Windows アプリの大部分に対して十分な組み込みコントロールの対話機能を見つけることができます。

標準の Windows テキストコントロールを使用する場合、このトピックで説明する選択動作とビジュアルはカスタマイズできません。

**テキストの選択**

アプリにテキストの選択をサポートするカスタム UI を実装する必要がある場合は、ここで説明する Windows の選択動作に従うことをお勧めします。

**編集可能なコンテンツと編集不可のコンテンツ**


タッチでは、選択操作は主に挿入カーソルの設定や単語の選択を行うタップ、選択範囲の変更を行うスライドなどのジェスチャを通じて実行されます。 他の Windows タッチ操作と同様に、時間制限のある対話式操作は情報 UI を表示するための長押しジェスチャに制限されます。 詳しくは、「[視覚的なフィードバックのガイドライン](guidelines-for-visualfeedback.md)」をご覧ください。

Windows では、選択操作のために "編集可能" と "編集不可" の 2 つの状態が認識され、その状態に合わせて選択 UI、フィードバック、機能が調整されます。

**編集可能なコンテンツ**

単語内の左半分をタップすると、単語のすぐ左にカーソルが配置されます。単語内の右半分をタップすると、単語のすぐ右にカーソルが配置されます。

次の図は、単語の先頭または終わりの近くでタップして、グリッパーを持つ最初の挿入カーソルを配置する方法を示しています。

![単語の左側をタップ (または長押し) するとその単語の先頭にキャレットとグリッパーが配置されます。 単語の右側をタップ (または長押し) するとその単語の終わりにキャレットとグリッパーが配置されます。](images/textselection-place-caret.png)

次の図は、グリッパーをドラッグして選択範囲を調整する方法を示しています。

![グリッパーをいずれかの方向にドラッグして選択範囲を調整します (最初のグリッパーは固定されたままで、2 つ目のグリッパーが表示されます)。 いずれかのグリッパーをドラッグし、以降の調整を行います。](images/adjust-selection.png)

次の図は、選択範囲内またはグリッパー上でタップしてコンテキスト メニューを呼び出す方法を示しています (長押しを使うこともできます)。

![選択範囲内またはグリッパー上でタップ (または長押し) してコンテキスト メニューを呼び出します。](images/textselection-show-context.png)

**メモ**   単語のスペルが間違っている場合、これらの相互作用は多少異なります。 綴りに誤りがあるとしてマークされている単語をタップすると、単語全体が強調表示されて、スペル候補のコンテキスト メニューが呼び出されます。

 

**編集不可のコンテンツ**

次の図は、単語内でタップして単語を選ぶ方法を示しています (最初の選択にスペースは含まれていません)。

![単語内でタップしてその単語を選びます (最初の選択にスペースは含まれていません)。](images/select-word.png)

編集可能なテキストと同じ手順に従って、選択範囲を調整し、コンテキスト メニューを表示します。

**オブジェクトの操作**

Windows アプリでカスタムオブジェクト操作を実装する場合、可能な限り、同じ (または類似した) グリッパーリソースをテキスト選択として使用します。 そうすれば、プラットフォーム間で操作エクスペリエンスの一貫性が保たれます。

たとえば、次の図に示すように、グリッパーは、サイズ変更とトリミングをサポートする画像処理アプリや、調節可能なプログレス バーを備えたメディア プレーヤー アプリでも利用できます。

![プログレス グリッパーを備えたメディア プレーヤー](images/gripper-mediaplayer.png)

*調節可能なプログレス バーを備えたメディア プレーヤーです。*

![トリミング グリッパーが表示された図](images/gripper-imagemanip.png)

*トリミング グリッパーが表示された画像エディターです。*

## <a name="related-articles"></a>関連記事

### <a name="for-developers"></a>開発者向け

- [カスタム ユーザー操作](../layout/index.md)

### <a name="samples"></a>サンプル

- [基本的な入力のサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
- [待機時間が短い入力のサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
- [ユーザー操作モードのサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode)
- [フォーカスの視覚効果のサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals)

### <a name="archive-samples"></a>サンプルのアーカイブ

- [入力: XAML ユーザー入力イベントのサンプルに関するページ](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20XAML%20user%20input%20events%20sample)
- [入力: デバイス機能のサンプルに関するページ](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Input%20Device%20capabilities%20sample%20(Windows%208))
- [入力: タッチのヒット テストのサンプルに関するページ](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20desktop%20samples/%5BC%2B%2B%5D-Windows%208%20desktop%20samples/C%2B%2B/Windows%208%20desktop%20samples/Input%20Touch%20hit%20testing%20sample)
- [XAML のスクロール、パン、ズームのサンプル](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Universal%20Windows%20app%20samples/111487-Universal%20Windows%20app%20samples/XAML%20scrolling%2C%20panning%2C%20and%20zooming%20sample)
- [入力: 簡略化されたインクのサンプル](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20Simplified%20ink%20sample)
- [入力: Windows 8 のジェスチャのサンプルに関するページ](/samples/browse/?redirectedfrom=MSDN-samples)
- [入力: 操作とジェスチャのサンプル](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20Gestures%20and%20manipulations%20with%20GestureRecognizer)
- [DirectX タッチ入力のサンプル](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%2B%2B%5D-Windows%208%20app%20samples/C%2B%2B/Windows%208%20app%20samples/DirectX%20touch%20input%20sample%20(Windows%208))
