---
Description: 音声認識に使われるインストール済みの言語を選ぶ方法について説明します。
title: 音声認識エンジンの言語の指定
ms.assetid: 4C463A1B-AF6A-46FD-A839-5D6724955B38
label: Specify the speech recognizer language
template: detail.hbs
keywords: スピーチ, 音声, 音声認識, 自然言語, ディクテーション, 入力, ユーザーの操作
ms.date: 09/24/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: a19e4ec876ca5dfa313c56e5653b3a27a4155765
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/24/2020
ms.locfileid: "91219935"
---
# <a name="specify-the-speech-recognizer-language"></a>音声認識エンジンの言語の指定


音声認識に使われるインストール済みの言語を選ぶ方法について説明します。

> **重要な API**: [**SupportedTopicLanguages**](/uwp/api/windows.media.speechrecognition.speechrecognizer.supportedtopiclanguages)、[**SupportedGrammarLanguages**](/uwp/api/windows.media.speechrecognition.speechrecognizer.supportedgrammarlanguages)、[**Language**](/uwp/api/Windows.Globalization.Language)


ここでは、システムにインストールされている言語を列挙し、どの言語が既定の言語であるかを指定します。また、音声認識用に別の言語を選びます。

**前提条件:**

このトピックは、 [音声認識](speech-recognition.md)に基づいています。

音声認識と認識の制約についての基本的な知識が必要です。

Windows アプリの開発に慣れていない場合は、以下のトピックを参照して、ここで説明するテクノロジについて理解してください。

-   [最初のアプリの作成](../../get-started/your-first-app.md)
-   「[イベントとルーティング イベントの概要](../../xaml-platform/events-and-routed-events-overview.md)」に記載されているイベントの説明

**ユーザーエクスペリエンスのガイドライン:**

魅力的な音声認識対応アプリの設計に役立つ便利なヒントについては、「[音声機能の設計ガイドライン](./speech-interactions.md)」をご覧ください。

## <a name="identify-the-default-language"></a>既定の言語を指定する


音声認識エンジンでは、システムの音声認識の言語を既定の認識言語として使います。 この言語は、デバイスで [設定] &gt; [システム] &gt; [音声認識] &gt; [音声認識の言語] の順に移動し、画面上でユーザーが設定します。

[**SystemSpeechLanguage**](/uwp/api/windows.media.speechrecognition.speechrecognizer.systemspeechlanguage) 静的プロパティを調べて、既定の言語を特定します。

```CSharp
var language = SpeechRecognizer.SystemSpeechLanguage; 
```

## <a name="confirm-an-installed-language"></a>インストールされている言語を確認する


インストールされている言語はデバイスによって異なる場合があります。 特定の制約を使う際にある言語に依存する場合は、その言語が存在するかどうかを確認してください。

**メモ**   新しい言語パックをインストールすると、再起動が必要になります。 \_指定された言語がサポートされていない場合、またはインストールが完了していない場合は、エラーコード SPERR が見つからないという例外 \_ (0x8004503a) が発生します。

 

[**SpeechRecognizer**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer) クラスの 2 つの静的プロパティのいずれかを調べて、デバイスでサポートされる言語を特定します。

-   [**SupportedTopicLanguages**](/uwp/api/windows.media.speechrecognition.speechrecognizer.supportedtopiclanguages): 定義済みのディクテーション文法および Web 検索文法と共に使われる [**Language**](/uwp/api/Windows.Globalization.Language) オブジェクトのコレクションです。

-   [**SupportedGrammarLanguages**](/uwp/api/windows.media.speechrecognition.speechrecognizer.supportedgrammarlanguages): 一覧の制約または Speech Recognition Grammar Specification (SRGS) ファイルと共に使われる [**Language**](/uwp/api/Windows.Globalization.Language) オブジェクトのコレクションです。

## <a name="specify-a-language"></a>言語を指定する


言語を指定するには、[**SpeechRecognizer**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer) コンストラクターで [**Language**](/uwp/api/Windows.Globalization.Language) オブジェクトを渡します。

ここでは、認識言語として "en-US" を指定します。


```CSharp
var language = new Windows.Globalization.Language("en-US"); 
var recognizer = new SpeechRecognizer(language); 
```

## <a name="remarks"></a>解説


トピック制約を構成するには、[**SpeechRecognitionTopicConstraint**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionTopicConstraint) を [**SpeechRecognizer**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer) の [**Constraints**](/uwp/api/windows.media.speechrecognition.speechrecognizer.constraints) コレクションに追加して、[**CompileConstraintsAsync**](/uwp/api/windows.media.speechrecognition.speechrecognizer.compileconstraintsasync) を呼び出します。 サポートされているトピックの言語で認識エンジンが初期化されていない場合は、**TopicLanguageNotSupported** の [**SpeechRecognitionResultStatus**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResultStatus) が返されます。

一覧の制約を構成するには、[**SpeechRecognitionListConstraint**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionListConstraint) を [**SpeechRecognizer**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer) の [**Constraints**](/uwp/api/windows.media.speechrecognition.speechrecognizer.constraints) コレクションに追加して、[**CompileConstraintsAsync**](/uwp/api/windows.media.speechrecognition.speechrecognizer.compileconstraintsasync) を呼び出します。 カスタム一覧の言語を直接指定することはできません。 代わりに、認識エンジンの言語を使って一覧が処理されます。

SRGS 文法は、[**SpeechRecognitionGrammarFileConstraint**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionGrammarFileConstraint) クラスによって表されるオープン スタンダードの XML 形式です。 カスタム一覧とは異なり、SRGS マークアップで文法の言語を指定できます。 [**CompileConstraintsAsync**](/uwp/api/windows.media.speechrecognition.speechrecognizer.compileconstraintsasync)認識エンジンが SRGS マークアップと同じ言語に初期化されていない場合は、**TopicLanguageNotSupported** の [**SpeechRecognitionResultStatus**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResultStatus)  により失敗します。

## <a name="related-articles"></a>関連記事

* [音声操作](speech-interactions.md)

**サンプル**

* [音声認識と音声合成のサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SpeechRecognitionAndSynthesis)
 

 
