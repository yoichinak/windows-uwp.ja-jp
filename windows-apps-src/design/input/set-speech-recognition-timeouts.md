---
description: 音声認識エンジンが無音または認識できないサウンド (雑音) を無視し、音声入力を待機する時間の長さを設定します。
title: 音声認識のタイムアウトの設定
ms.assetid: 58F446AC-4A56-454D-8125-62A2C4DBFCC8
label: Speech recognition timeouts
template: detail.hbs
keywords: スピーチ, 音声, 音声認識, 自然言語, ディクテーション, 入力, ユーザーの操作
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 78c351941b1b6703c28f249afcd119cf267f3d0a
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2020
ms.locfileid: "93031355"
---
# <a name="set-speech-recognition-timeouts"></a>音声認識のタイムアウトの設定


音声認識エンジンが無音または認識できないサウンド (雑音) を無視し、音声入力を待機する時間の長さを設定します。

> **重要な API** : [**Timeouts**](/uwp/api/windows.media.speechrecognition.speechrecognizer.timeouts)、 [**SpeechRecognizerTimeouts**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizerTimeouts)

## <a name="set-a-timeout"></a>タイムアウトの設定


ここでは、さまざまな [**Timeouts**](/uwp/api/windows.media.speechrecognition.speechrecognizer.timeouts) 値を指定します。

-   InitialSilenceTimeout: SpeechRecognizer が (認識結果が生成されるまでの) 無音を検出し、音声入力が続かないと見なす時間の長さ。
-   BabbleTimeout: SpeechRecognizer が、認識できないサウンド (雑音) のリッスンを継続し、音声入力が終了したと見なし、認識処理を終了するまでの時間の長さ。
-   EndSilenceTimeout: SpeechRecognizer が (認識結果が生成された後の) 無音を検出し、音声入力が終了したと見なす時間の長さ。

**注**  タイムアウトは認識エンジンごとに設定できます。

 

```CSharp
// Set timeout settings.
recognizer.Timeouts.InitialSilenceTimeout = TimeSpan.FromSeconds(6.0);
recognizer.Timeouts.BabbleTimeout = TimeSpan.FromSeconds(4.0);
recognizer.Timeouts.EndSilenceTimeout = TimeSpan.FromSeconds(1.2);
```

## <a name="related-articles"></a>関連記事

* [音声操作](speech-interactions.md)

**サンプル**

* [音声認識と音声合成のサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SpeechRecognitionAndSynthesis)
