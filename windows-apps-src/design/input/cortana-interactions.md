---
description: Windows アプリケーションで1つのアクションを起動して実行する音声コマンドを使用して、 **Cortana** の基本的な機能を拡張します。
title: Windows アプリにおける Cortana のやり取り
ms.assetid: 4C11A7CF-DA26-4CA1-A9B9-FE52670101F5
label: Cortana
template: detail.hbs
keywords: Cortana, Cortana のキャンバス, Cortana の設計, ユーザー インターフェイス, 音声コマンド, VCD
ms.date: 01/27/2021
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 6187022f4fcce254d142c61f1c2fe222c11bd8a5
ms.sourcegitcommit: 8fe992f3a6d8f7975af4911ad88e855bee50083e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/05/2021
ms.locfileid: "99606047"
---
# <a name="cortana-interactions-in-windows-apps"></a>Windows アプリにおける Cortana のやり取り

>[!WARNING]
> この機能は、Windows 10 2020 年5月の更新プログラム (バージョン2004、コードネーム "20H1") ではサポートされなくなりました。
>
> Cortana が最新の生産性エクスペリエンスを変革する方法については [、Microsoft 365 の cortana](/microsoft-365/admin/misc/cortana-integration) を参照してください。

Windows アプリケーションで1つのアクションを起動して実行する音声コマンドを使用して、 **Cortana** の基本的な機能を拡張します。

ターゲット アプリは、操作の複雑さに応じて、フォアグラウンドで起動したり (アプリがフォーカスを取得し、**Cortana** は消えます)、バックグラウンドでアクティブ化されたりします (**Cortana** がフォーカスを維持しますが、アプリからの結果を表示します)。 一般に、追加のコンテキストやユーザー入力を必要とする音声コマンドは、フォアグラウンドアプリで処理することをお勧めします。一方、基本的なコマンドは、バックグラウンドアプリを使用して **Cortana** で処理できます。

アプリの基本的な機能を統合し、ユーザーが直接アプリを開かずにほとんどのタスクを実行できる中心的エントリ ポイントを提供することで、**Cortana** はアプリとユーザーの仲介役となります。 アプリの機能へのこのショートカットを提供し、アプリを切り替える必要性を減らすことで、ユーザーの時間と労力を大幅に節約できます。

> [!NOTE]
> 音声コマンドは、音声コマンド定義 (VCD) ファイルで定義されている特定のインテントを持つ単一の (発話) であり、インストールされているアプリに **Cortana** 経由で送信されます。
>
> VCD ファイルでは、1 つ以上の音声コマンドが定義されており、各音声コマンドは固有の目的を持っています。
>
> 音声コマンドの定義は、複雑さによって異なります。 1つの制約された (発話) から、より柔軟な自然言語発話のコレクションに至るまで、あらゆることをサポートできます。これはすべて同じ目的を意味します。

## <a name="other-speech-and-conversation-components"></a>その他の音声および会話コンポーネント

### <a name="speech-voice-and-conversation-in-windows-10"></a>Windows 10 の音声、音声、および会話

Windows アプリケーションを構築する開発者に対する音声認識、音声合成、およびメッセージ交換のサポートについては、「 [windows 10 での音声、音声、および会話](/windows/apps/speech) 」を参照してください。

### <a name="cortana-skills-kit"></a>Cortana Skills Kit

Cortana を使用してユーザーが **サービス** と対話できるようにするスキルを追加して cortana を拡張する場合は、Cortana の [スキルキット](/cortana/skills/)を参照してください。 [**廃止に関する注意:** cortana を Microsoft 365 に埋め込むことによって最新の生産性向上エクスペリエンスを変革することの目標の一環として、お客様向けの cortana スキルキット (developer platform) と、このプラットフォーム上に構築されたすべてのスキルを廃止しています。]

## <a name="related-articles"></a>関連記事

- [VCD 要素および属性 v1.2](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)
- [Cortana のデザインガイドライン](cortana-design-guidelines.md)
- [Cortana 音声コマンドのサンプル](https://go.microsoft.com/fwlink/p/?LinkID=619899)
