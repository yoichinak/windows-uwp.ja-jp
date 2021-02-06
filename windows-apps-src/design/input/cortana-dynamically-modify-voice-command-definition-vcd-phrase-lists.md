---
title: Cortana を動的に変更する VCD 語句の一覧-Cortana UWP の設計と開発
description: 音声認識の結果を使って、音声コマンド定義 (VCD) ファイルに含まれているサポート対象語句の一覧 (PhraseList 要素) にアクセスし、この一覧を実行時に更新することができます。
ms.assetid: b497145b-c7a0-454a-8329-6bc1228953bb
ms.date: 01/28/2021
ms.topic: article
keywords: Cortana
ms.openlocfilehash: 1f61a08e9eeb66371ed39b44eb39dacbc1bf3cf5
ms.sourcegitcommit: 8fe992f3a6d8f7975af4911ad88e855bee50083e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/05/2021
ms.locfileid: "99606037"
---
# <a name="dynamically-modify-cortana-vcd-phrase-lists"></a>Cortana を動的に変更する VCD 語句の一覧

>[!WARNING]
> この機能は、Windows 10 2020 年5月の更新プログラム (バージョン2004、コードネーム "20H1") ではサポートされなくなりました。
>
> Cortana が最新の生産性エクスペリエンスを変革する方法については [、Microsoft 365 の cortana](/microsoft-365/admin/misc/cortana-integration) を参照してください。

音声認識の結果を使って、音声コマンド定義 (VCD) ファイルに含まれているサポート対象語句の一覧 (**PhraseList** 要素) にアクセスし、この一覧を実行時に更新することができます。

> [!NOTE]
> 音声コマンドは、音声コマンド定義 (VCD) ファイルで定義されている特定のインテントを持つ単一の (発話) であり、インストールされているアプリに **Cortana** 経由で送信されます。
>
> VCD ファイルでは、1 つ以上の音声コマンドが定義されており、各音声コマンドは固有の目的を持っています。
>
> 音声コマンドの定義は、複雑さによって異なります。 1つの制約された (発話) から、より柔軟な自然言語発話のコレクションに至るまで、あらゆることをサポートできます。これはすべて同じ目的を意味します。

音声コマンドから実行されるタスクにユーザー定義のアプリ データや一時的なアプリ データが関係する場合は、実行時に動的に語句一覧を変更できると便利です。

> [!NOTE]
> **重要な API**
>
> - [**Windows.ApplicationModel.VoiceCommands**](/uwp/api/Windows.ApplicationModel.VoiceCommands)
> - [**VCD 要素および属性 v1.2**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)

例として、ユーザーが宛先を入力できる旅行アプリがあるとします。ユーザーがアプリを起動できるようにするには、アプリケーション名の後に「宛先へのトリップを表示」と入力し &lt; &gt; ます。 この場合、**ListenFor** 要素自体は `<ListenFor> Show trip to {destination}  </ListenFor>` のように指定します。ここで、"destination" は **PhraseList** の **Label** 属性の値です。

実行時に語句一覧を更新すれば、考えられる目的地ごとに別々の **ListenFor** 要素を作成する必要がなくなります。 代わりに、ユーザーが旅程の入力時に指定した目的地を動的に **PhraseList** に設定できます。

**PhraseList** とその他の VCD 要素について詳しくは、[**VCD 要素および属性 v1.2**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2) のリファレンスをご覧ください。

> [!TIP]
> **前提条件**
>
> ユニバーサル Windows プラットフォーム (UWP) アプリを開発するのが初めての場合は、以下のトピックに目を通して、ここで説明されているテクノロジをよく理解できるようにしてください。
>
> - [初めてのアプリの作成](/windows/uwp/get-started/your-first-app)
> - 「[イベントとルーティング イベントの概要](/windows/uwp/xaml-platform/events-and-routed-events-overview)」に記載されているイベントの説明
>
> **ユーザーエクスペリエンスのガイドライン**
>
> **Cortana** と [音声](speech-interactions.md)による対話とアプリを統合する方法については、 [cortana のデザインガイドライン](cortana-design-guidelines.md)に関する情報を参照してください。

## <a name="identify-the-command-and-update-the-phrase-list"></a>コマンドの識別と語句一覧の更新

VCD ファイルの例を次に示します。このファイルでは、**Command** "showTripToDestination" と、目的地を表す 3 つのオプションを含む **PhraseList** を **Adventure Works** 旅行アプリに定義します。 ユーザーがアプリで目的地を保存したり削除したりすると、アプリは **PhraseList** のオプションを更新します。

```XML
<?xml version="1.0" encoding="utf-8"?>
<VoiceCommands xmlns="https://schemas.microsoft.com/voicecommands/1.1">
  <CommandSet xml:lang="en-us" Name="AdventureWorksCommandSet_en-us">
    <AppName> Adventure Works, </AppName>
    <Example> Show trip to London </Example>

    <Command Name="showTripToDestination">
      <Example> show trip to London  </Example>
      <ListenFor> show trip to {destination} </ListenFor>
      <Feedback> Showing trip to {destination} </Feedback>
      <Navigate/>
    </Command>

    <PhraseList Label="destination">
      <Item> London </Item>
      <Item> Dallas </Item>
      <Item> New York </Item>
    </PhraseList>

  </CommandSet>

<!-- Other CommandSets for other languages -->

</VoiceCommands>
```

VCD ファイルの **PhraseList** 要素を更新するには、語句一覧を含む **CommandSet** 要素を取得します。 **CommandSet** 要素の **Name** 属性 (**Name** は VCD ファイル内で重複しないようにします) をキーとして [**VoiceCommandManager.InstalledCommandSets**](/uwp/api/Windows.Media.SpeechRecognition.VoiceCommandManager) プロパティにアクセスし、[**VoiceCommandSet**](/uwp/api/Windows.Media.SpeechRecognition.VoiceCommandSet) の参照を取得します。

コマンド セットを識別したら、変更する語句一覧への参照を取得し、[**SetPhraseListAsync**](/uwp/api/Windows.Media.SpeechRecognition.VoiceCommandSet) メソッドを呼び出します。このとき、**PhraseList** 要素の **Label** 属性と、語句一覧の新しいコンテンツとなる文字列の配列を指定します。

> [!NOTE]
> 語句リストを変更すると、語句リスト全体が置換されます。 語句一覧に新しい項目を追加する場合は、既にある項目と新しい項目の両方を指定して [**SetPhraseListAsync**](/uwp/api/Windows.Media.SpeechRecognition.VoiceCommandSet) を呼び出す必要があります。

次の例では、前の例で示した **PhraseList** を更新して、Phoenix という目的地を追加する方法を示します。

```CSharp
Windows.ApplicationModel.VoiceCommands.VoiceCommandDefinition.VoiceCommandSet commandSetEnUs;

if (Windows.ApplicationModel.VoiceCommands.VoiceCommandDefinitionManager.
      InstalledCommandSets.TryGetValue(
        "AdventureWorksCommandSet_en-us", out commandSetEnUs))
{
  await commandSetEnUs.SetPhraseListAsync(
    "destination", new string[] {“London”, “Dallas”, “New York”, “Phoenix”});
}
```

## <a name="remarks"></a>解説

**PhraseList** を使った認識の制約は、比較的少ないセットや単語に適しています。 単語セットが大きすぎ (数百語など) たり、まったく制約しない場合は、**PhraseTopic** 要素と **Subject** 要素を使って音声認識結果の関連性を絞り込み、スケーラビリティを高めます。

この例では、" **PhraseTopic** " という **シナリオ** で、"City 州" の **サブジェクト** によってさらに洗練された "検索" というシナリオがあり \\ ます。

```XML
<?xml version="1.0" encoding="utf-8"?>
<VoiceCommands xmlns="https://schemas.microsoft.com/voicecommands/1.1">
  <CommandSet xml:lang="en-us" Name="AdventureWorksCommandSet_en-us">
    <AppName> Adventure Works, </AppName>
    <Example> Show trip to London </Example>

    <Command Name="showTripToDestination">
      <Example> show trip to London  </Example>
      <ListenFor> show trip to {destination} </ListenFor>
      <Feedback> Showing trip to {destination} </Feedback>
      <Navigate/>
    </Command>

    <PhraseList Label="destination">
      <Item> London </Item>
      <Item> Dallas </Item>
      <Item> New York </Item>
    </PhraseList>

    <PhraseTopic Label="destination" Scenario="Search">
      <Subject>City/State</Subject>
    </PhraseTopic>

  </CommandSet>
```

## <a name="related-articles"></a>関連記事

- [Windows アプリにおける Cortana のやり取り](cortana-interactions.md)
- [VCD 要素および属性 v1.2](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)
- [Cortana を使用して音声コマンドでフォアグラウンドアプリをアクティブ化する](cortana-launch-a-foreground-app-with-voice-commands.md)
- [音声コマンドを使用して Cortana でバックグラウンドアプリをアクティブ化する](cortana-launch-a-background-app-with-voice-commands.md)
- [Cortana のデザインガイドライン](cortana-design-guidelines.md)
- [Cortana 音声コマンドのサンプル](https://go.microsoft.com/fwlink/p/?LinkID=619899)
