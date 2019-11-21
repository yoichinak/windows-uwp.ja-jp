---
Description: 音声認識に使われるインストール済みの言語を選ぶ方法について説明します。
title: 音声認識エンジンの言語の指定
ms.assetid: 4C463A1B-AF6A-46FD-A839-5D6724955B38
label: Specify the speech recognizer language
template: detail.hbs
keywords: スピーチ, 音声, 音声認識, 自然言語, ディクテーション, 入力, ユーザーの操作
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 200fe265390d10a12a8e1b3a1abf7cd8164238d6
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258240"
---
# <a name="specify-the-speech-recognizer-language"></a>音声認識エンジンの言語の指定


音声認識に使われるインストール済みの言語を選ぶ方法について説明します。

> **重要な API**: [**SupportedTopicLanguages**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.supportedtopiclanguages)、[**SupportedGrammarLanguages**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.supportedgrammarlanguages)、[**Language**](https://docs.microsoft.com/uwp/api/Windows.Globalization.Language)


ここでは、システムにインストールされている言語を列挙し、どの言語が既定の言語であるかを指定します。また、音声認識用に別の言語を選びます。

**Prerequisites:**

このトピックは、「[音声認識](speech-recognition.md)」に基づいています。

音声認識と認識の制約についての基本的な知識が必要です。

ユニバーサル Windows プラットフォーム (UWP) アプリを開発するのが初めての場合は、以下のトピックに目を通して、ここで説明されているテクノロジをよく理解できるようにしてください。

-   [初めてのアプリの作成](https://docs.microsoft.com/windows/uwp/get-started/your-first-app)
-   「[イベントとルーティング イベントの概要](https://docs.microsoft.com/windows/uwp/xaml-platform/events-and-routed-events-overview)」に記載されているイベントの説明

**User experience guidelines:**

魅力的な音声認識対応アプリの設計に役立つ便利なヒントについては、「[音声機能の設計ガイドライン](https://docs.microsoft.com/windows/uwp/input-and-devices/speech-interactions)」をご覧ください。

## <a name="identify-the-default-language"></a>既定の言語を指定する


音声認識エンジンでは、システムの音声認識の言語を既定の認識言語として使います。 この言語は、デバイスで [設定] &gt; [システム] &gt; [音声認識] &gt; [音声認識の言語] の順に移動し、画面上でユーザーが設定します。

[  **SystemSpeechLanguage**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.systemspeechlanguage) 静的プロパティを調べて、既定の言語を特定します。

```CSharp
var language = SpeechRecognizer.SystemSpeechLanguage; 
```

## <a name="confirm-an-installed-language"></a>インストールされている言語を確認する


インストールされている言語はデバイスによって異なる場合があります。 特定の制約を使う際にある言語に依存する場合は、その言語が存在するかどうかを確認してください。

**Note**  A reboot is required after a new language pack is installed. An exception with error code SPERR\_NOT\_FOUND (0x8004503a) is raised if the specified language is not supported or has not finished installing.

 

[  **SpeechRecognizer**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer) クラスの 2 つの静的プロパティのいずれかを調べて、デバイスでサポートされる言語を特定します。

-   [**SupportedTopicLanguages**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.supportedtopiclanguages)—The collection of [**Language**](https://docs.microsoft.com/uwp/api/Windows.Globalization.Language) objects used with predefined dictation and web search grammars.

-   [**SupportedGrammarLanguages**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.supportedgrammarlanguages)—The collection of [**Language**](https://docs.microsoft.com/uwp/api/Windows.Globalization.Language) objects used with a list constraint or a Speech Recognition Grammar Specification (SRGS) file.

## <a name="specify-a-language"></a>言語を指定する


言語を指定するには、[**SpeechRecognizer**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer) コンストラクターで [**Language**](https://docs.microsoft.com/uwp/api/Windows.Globalization.Language) オブジェクトを渡します。

ここでは、認識言語として "en-US" を指定します。


```CSharp
var language = new Windows.Globalization.Language("en-US"); 
var recognizer = new SpeechRecognizer(language); 
```

## <a name="remarks"></a>注釈


トピック制約を構成するには、[**SpeechRecognitionTopicConstraint**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionTopicConstraint) を [**SpeechRecognizer**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer) の [**Constraints**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.constraints) コレクションに追加して、[**CompileConstraintsAsync**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.compileconstraintsasync) を呼び出します。 サポートされているトピックの言語で認識エンジンが初期化されていない場合は、**TopicLanguageNotSupported** の [**SpeechRecognitionResultStatus**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResultStatus) が返されます。

一覧の制約を構成するには、[**SpeechRecognitionListConstraint**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionListConstraint) を [**SpeechRecognizer**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer) の [**Constraints**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.constraints) コレクションに追加して、[**CompileConstraintsAsync**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.compileconstraintsasync) を呼び出します。 カスタム一覧の言語を直接指定することはできません。 代わりに、認識エンジンの言語を使って一覧が処理されます。

SRGS 文法は、[**SpeechRecognitionGrammarFileConstraint**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionGrammarFileConstraint) クラスによって表されるオープン スタンダードの XML 形式です。 カスタム一覧とは異なり、SRGS マークアップで文法の言語を指定できます。 [**CompileConstraintsAsync**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.compileconstraintsasync) fails with a [**SpeechRecognitionResultStatus**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResultStatus) of **TopicLanguageNotSupported** if the recognizer is not initialized to the same language as the SRGS markup.

## <a name="related-articles"></a>関連記事

**開発者向け**

* [音声操作](speech-interactions.md)

**デザイナー向け**

* [音声認識のデザイン ガイドライン](https://docs.microsoft.com/windows/uwp/input-and-devices/speech-interactions)

**サンプル**

* [Speech recognition and speech synthesis sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SpeechRecognitionAndSynthesis)
 

 




