---
title: Cortana のより自然な音声コマンドをサポートする-Cortana UWP の設計と開発
description: ユーザーがコマンド内の任意の場所にアプリ名を含めることができる柔軟で自然な音声コマンドで Cortana を拡張します。
author: kbridge
label: Conceptual
ms.assetid: c2959c1b-c2f2-4a8d-8f3e-79585f69afcf
ms.date: 01/28/2021
ms.topic: article
keywords: Cortana
ms.openlocfilehash: 7716d4d623653c6b2d943135f2e2cf1ac9a40343
ms.sourcegitcommit: 8fe992f3a6d8f7975af4911ad88e855bee50083e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/05/2021
ms.locfileid: "99605997"
---
# <a name="support-natural-language-voice-commands-in-cortana"></a>Cortana での自然言語音声コマンドのサポート

>[!WARNING]
> この機能は、Windows 10 2020 年5月の更新プログラム (バージョン2004、コードネーム "20H1") ではサポートされなくなりました。
>
> Cortana が最新の生産性エクスペリエンスを変革する方法については [、Microsoft 365 の cortana](/microsoft-365/admin/misc/cortana-integration) を参照してください。

ユーザーがコマンド内の任意の場所にアプリ名を含めることができる柔軟で自然な音声コマンドで **Cortana** を拡張します。

> [!NOTE]
> **重要な API**
>
> - [**Windows.ApplicationModel.VoiceCommands**](/uwp/api/Windows.ApplicationModel.VoiceCommands)
> - [**VCD 要素および属性 v1.2**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)

音声コマンドを使ってアプリの機能で Cortana を拡張する場合、ユーザーはアプリと実行するコマンドや機能の両方を指定する必要があります。 通常、これは音声コマンドの最初または最後にアプリ名を発音することによって行います。 たとえば、「Adventure Works、新しいラスベガス旅行を追加します」などです。

ただし、このような方法でアプリケーション名を指定すると、不自然であったり、堅苦しくなる可能性がありますし、意味をなさないことすらあります。 多くの場合、コマンドの別の場所でアプリ名を言うことができた方が使いやすく自然なため、操作がより直感的になり、ユーザーにとって魅力的です。 前の例の「Adventure Works、新しいラスベガス旅行を追加します」は次のように言い換えることができます。 「新しい Adventure Works のラスベガス旅行を追加します」 または「Adventure Works を使って、新しいラスベガス旅行を追加します」

アプリ名が以下の位置でサポートされるように音声コマンドを設定することができます。

- プレフィックス - コマンド語句の前
- インフィックス - コマンド語句の途中
- サフィックス - コマンド語句の後

> [!TIP]
> **前提条件**
>
> このトピックで [は、音声コマンドを使用して Cortana でバックグラウンドアプリをアクティブ化する方法](cortana-launch-a-background-app-with-voice-commands.md)について説明します。 ここでは、引き続き **Adventure Works** という旅行の計画および管理アプリを使って機能について説明します。
>
> ユニバーサル Windows プラットフォーム (UWP) アプリを開発するのが初めての場合は、以下のトピックに目を通して、ここで説明されているテクノロジをよく理解できるようにしてください。
>
> - [初めてのアプリの作成](/windows/uwp/get-started/your-first-app)
> - 「[イベントとルーティング イベントの概要](/windows/uwp/xaml-platform/events-and-routed-events-overview)」に記載されているイベントの説明
>
> **ユーザーエクスペリエンスのガイドライン**
>
> **Cortana** と [音声](speech-interactions.md)による対話とアプリを統合する方法については、 [cortana のデザインガイドライン](cortana-design-guidelines.md)に関する情報を参照してください。

## <a name="specify-an-appname-element-in-the-vcd"></a>VCD での **AppName** 要素の指定

**AppName** 要素は、音声コマンドでアプリのわかりやすい名前を指定するために使います。

```XML
<AppName>Adventure Works</AppName>
```

## <a name="specify-where-the-app-name-can-be-spoken-in-the-voice-command"></a>アプリ名を言うことができる音声コマンド内の場所を指定します。

**ListenFor** 要素には、アプリ名を言うことができる音声コマンド内の場所を指定する **RequireAppName** 属性があります。 この属性では、次の 4 つの値がサポートされます。

1. **BeforePhrase**

   既定値。

   ユーザーがコマンド語句の前にアプリ名を言う必要があることを指定します。

   ここでは、Cortana は「Adventure Works、次のラスベガス旅行はいつですか」という内容を待機します。

   ```xml
   <ListenFor RequireAppName="BeforePhrase"> show [my] trip to {destination} </ListenFor>
   ```

2. **AfterPhrase**

    ユーザーがコマンド語句の後にアプリ名を言う必要があることを指定します。

    前置詞付きの接続詞のローカライズされた語句一覧がシステムによって提供されます。 これには、"を使って"、"によって"、"で" などの語句が含まれます。

    ここでは、Cortana は「次のラスベガス旅行を Adventure Works で表示してください」や「次のラスベガス旅行を、Adventure Works を使って表示してください」などのコマンドを待機します。

    ```xml
    <ListenFor RequireAppName="AfterPhrase">show [my] next trip to {destination} </ListenFor>
    ```

3. **BeforeOrAfterPhrase**

    ユーザーがコマンド語句の前または後にアプリ名を言う必要があることを指定します。

    サフィックスとして使われる場合については、前置詞付きの接続詞のローカライズされた語句一覧がシステムによって提供されます。 これには、"を使って"、"によって"、"で" などの語句が含まれます。

    ここでは、Cortana は「Adventure Works、次のラスベガス旅行を表示してください」や「次のラスベガス旅行を Adventure Works で表示してください」などのコマンドを待機します。

    ``` xml
    <ListenFor RequireAppName="BeforeOrAfterPhrase">show [my] next trip to {destination}</ListenFor>
    ```

4. **ExplicitlySpecified**

    ユーザーがコマンド語句の指定箇所でアプリ名を言う必要があることを指定します。 ユーザーは、語句の前後にアプリ名を言う必要はありません。

    **{builtin:AppName}** タグを使ってアプリ名を明示的に参照する必要があります。

    ここでは、Cortana は「Adventure Works、次のラスベガス旅行を表示してください」や「Adventure Works に登録されている次のラスベガス旅行を表示してください」などのコマンドを待機します。

    ```xml
    <ListenFor RequireAppName="ExplicitlySpecified">show [my] next {builtin:AppName} trip to {destination} </ListenFor>
    ```

## <a name="special-cases"></a>特殊なケース

**RequireAppName** が "AfterPhrase" または "ExplicitlySpecified" の **ListenFor** 要素を宣言するときは、次の特定の要件を満たす必要があります。

1. **RequireAppName** が "ExplicitlySpecified" の場合、**{builtin:AppName}** は 1 回だけ出現する必要があります。

    この値を使うと、システムはアプリ名が音声コマンドのどの場所に出現するかを推測できません。 場所を明示的に指定する必要があります。

2. 音声コマンドの先頭を **PhraseTopic** 要素にすることはできません。この要素は、通常語彙の多い音声認識に使われます。 少なくとも 1 つの単語を前に付ける必要があります。

    これは、コマンドの言葉のどこかにアプリ名やその一部が含まれている場合に **Cortana** によりアプリが起動される可能性を最小限に抑えるのに役立ちます。

    ユーザーが「Kinect のアドベンチャー ワークスのレビューを表示してください」のような内容を声に出した場合に、**Cortana** によって **Adventure Works** アプリが起動される可能性がある無効な宣言の例を次に示します。
  
    ```xml
    <ListenFor RequireAppName="ExplicitlySpecified">{searchPhrase} {builtin:AppName}</ListenFor>
    ```

3. アプリ名と **PhraseTopic** 要素への参照に加えて、**ListenFor** 文字列内に 2 単語以上が含まれている必要があります。

    ケース 2 と同様、アプリが意図せずに起動してしまう可能性を最小限に抑えるため、コマンドに十分な音声コンテンツが含まれるようにする必要があります。

    これは、ユーザーが「Kinect のアドベンチャー ワークスを検索します」などと言ったときにアプリケーションが間違って起動しないように、できる限り成功率の高い方法でアプリケーションを設定するのに役立ちます。

    ユーザーが「アドベンチャー ワークスさん」や「Kinect のアドベンチャー ワークスを検索します」のような内容を声に出した場合に、**Cortana** によって **Adventure Works** アプリが起動される可能性がある無効な宣言の例を次に示します。

    ```xml
    <ListenFor RequireAppName="ExplicitlySpecified">Hey {builtin:AppName}</ListenFor>
    <ListenFor RequireAppName="ExplicitlySpecified">Find {searchPhrase} {builtin:AppName}</ListenFor>
    ```

## <a name="remarks"></a>解説

**Cortana** でユーザーが声に出すことができる音声コマンドのバリエーションを増やすことによっても、アプリの全体的な使いやすさが高まります。

\[AppName として "アプリ名がこんにちは" にならないよう \] にしてください。  ユーザーは、音声によるライセンス認証を通じて Cortana を呼び出すように "コルタナさん" と言うことがよくあり \[ ます。また、(発話) に "アプリ名 \] " があると自然に聞こえません。 たとえば、「コルタナさん。次のラスベガス旅行を Adventure Works さんで表示してください」などです。

既存の音声コマンドにインフィックス/サフィックスのバリエーションを追加することを検討してください。 ここに示したように、既存の **ListenFor** 要素を追加してサフィックスの変化形をサポートするのに、多くの労力はかかりません。 「コルタナさん。Adventure Works、次のラスベガス旅行を表示してください」より「コルタナさん。次のラスベガス旅行を Adventure Works で表示してください」の方が自然です。

音声コマンドが **Cortana** の既存の機能 (通話、メッセージングなど) と競合する場合は、アプリ名をプレフィックスとして使うことを検討してください。 たとえば、"Adventure Works、メッセージトラベルエージェント (ラスベガス) を使用 \[ \] します。

## <a name="complete-example"></a>コード例全体

より自然な言語の音声コマンドを提供するさまざまな方法を示す VCD ファイルを次に示します。

> [!NOTE]
> 複数の要素 **に対し** て、それぞれ異なる **requireappname** 属性値を持つことが有効です。

```XML
<?xml version="1.0" encoding="utf-8"?>
<VoiceCommands xmlns="https://schemas.microsoft.com/voicecommands/1.1">
  <CommandSet xml:lang="en-us" Name="commandSet_en-us">
    <AppName>Adventure Works</AppName>
    <Example> When is my trip to Las Vegas? </Example>
    <Command Name="whenIsTripToDestination">
      <Example> When is my trip to Las Vegas?</Example>
      <ListenFor RequireAppName="BeforePhrase">
        when is my] trip to {destination} </ListenFor>

      <!-- This ListenFor command will set up Cortana to accept commands like 
           “Show my next trip to Las Vegas on Adventure Works”; “Show my next 
           trip to Las Vegas using Adventure Works” -->
      <ListenFor RequireAppName="AfterPhrase">
        show [my] next trip to {destination} </ListenFor>

      <!-- This ListenFor command will set up Cortana to accept commands when 
           the user specifies your app name either before or after the command. 
           “Adventure Works, show my next trip to Las Vegas”; 
           “Show my next trip to Last Vegas on Adventure works” -->
      <ListenFor RequireAppName="BeforeOrAfterPhrase">
        show [my] next trip to {destination} </ListenFor>

      <!-- This ListenFor command will set up Cortana to accept commands 
           when the user specifies your app name inline. 
           “Show my next Adventure Works trip to Las Vegas” -->
      <ListenFor RequireAppName="ExplicitlySpecified">
        show [my] next {builtin:AppName} trip to {destination} </ListenFor>

      <Feedback> Looking for trip to {destination} </Feedback>
      <Navigate />
    </Command>
    <PhraseList Label="destination">
      <Item> Las Vegas </Item>
      <Item> Dallas </Item>
      <Item> New York </Item>
    </PhraseList>
  </CommandSet>
  <!-- Other CommandSets for other languages -->
</VoiceCommands>
```

## <a name="related-articles"></a>関連記事

- [Windows アプリにおける Cortana のやり取り](cortana-interactions.md)
- [Cortana のデザインガイドライン](cortana-design-guidelines.md)
- [VCD 要素および属性 v1.2](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)
- [Cortana 音声コマンドのサンプル](https://go.microsoft.com/fwlink/p/?LinkID=619899)
