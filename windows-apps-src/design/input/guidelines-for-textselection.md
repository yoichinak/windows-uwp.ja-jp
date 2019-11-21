---
Description: This topic describes the new Windows UI for selecting and manipulating text, images, and controls and provides user experience guidelines that should be considered when using these new selection and manipulation mechanisms in your UWP app.
title: テキストと画像の選択
ms.assetid: d973ffd8-602e-47b5-ab0b-4b2a964ec53d
label: Selecting text and images
template: detail.hbs
keywords: キーボード, テキスト, 入力, ユーザーの操作
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 56f09f2c903354159c63fa7226007cd65e57e69e
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/20/2019
ms.locfileid: "74257919"
---
# <a name="selecting-text-and-images"></a>テキストと画像の選択


この記事では、テキスト、画像、コントロールの選択と操作について説明し、アプリでこれらのメカニズムを使うときに考慮する必要があるユーザー エクスペリエンスのガイドラインを示します。

> **重要な API**: [**Windows.UI.Xaml.Input**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input)、[**Windows.UI.Input**](https://docs.microsoft.com/uwp/api/Windows.UI.Input)
 


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

**Considerations and recommendations**

Use the built-in controls exposed through the language frameworks in Windows to build apps that provide the full platform user interaction experience, including selection and manipulation behaviors. ビルトイン コントロールの対話式操作の機能は、大部分の UWP アプリにとって十分なものです。

標準の UWP テキスト コントロールを使う場合、このトピックで説明した選択の動作と視覚効果はカスタマイズできません。

**テキストの選択**

If your app requires a custom UI that supports text selection, we recommend that you follow the Windows selection behaviors described here.

**Editable and non-editable content**


タッチでは、選択操作は主に挿入カーソルの設定や単語の選択を行うタップ、選択範囲の変更を行うスライドなどのジェスチャを通じて実行されます。 As with other Windows touch interactions, timed interactions are limited to the press and hold gesture to display informational UI. 詳しくは、「[視覚的なフィードバックのガイドライン](guidelines-for-visualfeedback.md)」をご覧ください。

Windows recognizes two possible states for selection interactions, editable and non-editable, and adjusts selection UI, feedback, and functionality accordingly.

**Editable content**

単語内の左半分をタップすると、単語のすぐ左にカーソルが配置されます。単語内の右半分をタップすると、単語のすぐ右にカーソルが配置されます。

次の図は、単語の先頭または終わりの近くでタップして、グリッパーを持つ最初の挿入カーソルを配置する方法を示しています。

![単語の左側をタップ (または長押し) するとその単語の先頭にキャレットとグリッパーが配置されます。 単語の右側をタップ (または長押し) するとその単語の終わりにキャレットとグリッパーが配置されます。](images/textselection-place-caret.png)

次の図は、グリッパーをドラッグして選択範囲を調整する方法を示しています。

![グリッパーをいずれかの方向にドラッグして選択範囲を調整します (最初のグリッパーは固定されたままで、2 つ目のグリッパーが表示されます)。 いずれかのグリッパーをドラッグし、以降の調整を行います。](images/adjust-selection.png)

次の図は、選択範囲内またはグリッパー上でタップしてコンテキスト メニューを呼び出す方法を示しています (長押しを使うこともできます)。

![選択範囲内またはグリッパー上でタップ (または長押し) してコンテキスト メニューを呼び出します。](images/textselection-show-context.png)

**Note**  These interactions vary somewhat in the case of a misspelled word. 綴りに誤りがあるとしてマークされている単語をタップすると、単語全体が強調表示されて、スペル候補のコンテキスト メニューが呼び出されます。

 

**Non-editable content**

次の図は、単語内でタップして単語を選ぶ方法を示しています (最初の選択にスペースは含まれていません)。

![単語内でタップしてその単語を選びます (最初の選択にスペースは含まれていません)。](images/select-word.png)

編集可能なテキストと同じ手順に従って、選択範囲を調整し、コンテキスト メニューを表示します。

**Object manipulation**

UWP アプリでカスタム オブジェクト操作を実装する場合は、できる限り、テキストの選択と同じ (類似する) グリッパー リソースを使います。 そうすれば、プラットフォーム間で操作エクスペリエンスの一貫性が保たれます。

たとえば、次の図に示すように、グリッパーは、サイズ変更とトリミングをサポートする画像処理アプリや、調節可能なプログレス バーを備えたメディア プレーヤー アプリでも利用できます。

![プログレス グリッパーを備えたメディア プレーヤー](images/gripper-mediaplayer.png)

*Media player with adjustable progress bar.*

![トリミング グリッパーが表示された図](images/gripper-imagemanip.png)

*Image editor with cropping grippers.*

## <a name="related-articles"></a>関連記事



**開発者向け**
* [カスタム ユーザー操作](https://docs.microsoft.com/windows/uwp/design/layout/index)

**サンプル**
* [Basic input sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
* [Low latency input sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
* [ユーザー操作モードのサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode)
* [フォーカスの視覚効果のサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals)

**サンプルのアーカイブ**
* [Input: XAML user input events sample](https://code.msdn.microsoft.com/windowsapps/Input-3dff271b)
* [Input: Device capabilities sample](https://code.msdn.microsoft.com/windowsapps/Input-device-capabilities-31b67745)
* [Input: Touch hit testing sample](https://code.msdn.microsoft.com/windowsapps/Touch-Hit-Testing-sample-5e35c690)
* [XAML scrolling, panning, and zooming sample](https://code.msdn.microsoft.com/windowsapps/xaml-scrollviewer-pan-and-949d29e9)
* [Input: Simplified ink sample](https://code.msdn.microsoft.com/windowsapps/Input-simplified-ink-sample-11614bbf)
* [Input: Windows 8 gestures sample](https://docs.microsoft.com/samples/browse/?redirectedfrom=MSDN-samples)
* [Input: Manipulations and gestures (C++) sample](https://code.msdn.microsoft.com/windowsapps/Manipulations-and-gestures-362b6b59)
* [DirectX touch input sample](https://code.msdn.microsoft.com/windowsapps/Simple-Direct3D-Touch-f98db97e)
 

 




