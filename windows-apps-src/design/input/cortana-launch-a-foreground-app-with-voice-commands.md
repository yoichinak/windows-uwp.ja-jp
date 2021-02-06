---
title: Cortana を使用して音声コマンドでフォアグラウンドアプリをアクティブ化する-Cortana UWP の設計と開発
description: 音声コマンドを使用して、アプリをフォアグラウンドでアクティブ化し、アプリ内でアクションまたはコマンドを実行します。
ms.assetid: e4bf3714-6f62-466f-9e7c-3b03ee86a117
ms.date: 01/28/2021
ms.topic: article
keywords: Cortana
ms.openlocfilehash: 6e4e1b30d7d6e259ff66cdccebeed325ded57c18
ms.sourcegitcommit: 8fe992f3a6d8f7975af4911ad88e855bee50083e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/05/2021
ms.locfileid: "99606017"
---
# <a name="activate-a-foreground-app-with-voice-commands-through-cortana"></a>Cortana の音声コマンドを使ったフォアグラウンド アプリのアクティブ化

>[!WARNING]
> この機能は、Windows 10 2020 年5月の更新プログラム (バージョン2004、コードネーム "20H1") ではサポートされなくなりました。
>
> Cortana が最新の生産性エクスペリエンスを変革する方法については [、Microsoft 365 の cortana](/microsoft-365/admin/misc/cortana-integration) を参照してください。

**Cortana** 内で音声コマンドを使ってシステム機能にアクセスするだけでなく、アプリの機能によって **Cortana** を拡張することもできます。 音声コマンドを使用して、アプリをフォアグラウンドでアクティブ化し、アプリ内で操作やコマンドを実行できます。

> [!NOTE]
> **重要な API**
>
> - [**Windows.ApplicationModel.VoiceCommands**](/uwp/api/Windows.ApplicationModel.VoiceCommands)
> - [**VCD 要素および属性 v1.2**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)

アプリがフォアグラウンドで音声コマンドを処理するときに、フォーカスを取得すると、Cortana が閉じられます。 必要に応じて、アプリをバックグラウンド タスクとしてアクティブ化し、コマンドを実行できます。 この場合、Cortana はフォーカスを保持し、アプリは、**Cortana** キャンバスと **Cortana** 音声を介して、すべてのフィードバックと結果を返します。

追加のコンテキストやユーザー入力 (特定の連絡先へのメッセージの送信など) が必要な音声コマンドはフォアグラウンド アプリで処理するのが最適ですが、基本的なコマンド (旅行の予定の一覧表示など) はバックグラウンド アプリを介して **Cortana** で処理できます。

音声コマンドを使用してバックグラウンドでアプリをアクティブ化する場合は、 [音声コマンドを使用した Cortana でのバックグラウンドアプリのアクティブ化](cortana-launch-a-background-app-with-voice-commands.md)に関するページを参照してください。

> [!NOTE]
> 音声コマンドは、音声コマンド定義 (VCD) ファイルで定義されている特定のインテントを持つ単一の (発話) であり、インストールされているアプリに **Cortana** 経由で送信されます。
>
> VCD ファイルでは、1 つ以上の音声コマンドが定義されており、各音声コマンドは固有の目的を持っています。
>
> 音声コマンドの定義は、複雑さによって異なります。 1つの制約された (発話) から、より柔軟な自然言語発話のコレクションに至るまで、あらゆることをサポートできます。これはすべて同じ目的を意味します。

フォアグラウンド アプリの機能を示すために、[Cortana 音声コマンドのサンプル](https://go.microsoft.com/fwlink/p/?LinkID=619899)の **Adventure Works** という旅行の計画および管理アプリを使用します。

**Cortana** を使わないで新しい **Adventure Works** の旅行を作るには、アプリを起動して **[New trip]** (新しい旅行) ページに移動します。 既存の旅行を表示するには、**[Upcoming trips]** (今後の旅行) ページに移動して旅行を選びます。

**Cortana** を通じて音声コマンドを使うと、ユーザーは代わりに「Adventure Works の旅行を追加します」や「Adventure Works で旅行を追加します」と言うだけでアプリを起動し、**[New trip]** (新しい旅行) ページに移動できます。 次に、「Adventure Works、ロンドン旅行を表示してください」と言うと、次のように、アプリが起動し、**[Trip]** (旅行) 詳細ページに移動します。

:::image type="content" source="images/cortana/cortana-foreground-with-adventureworks.png" alt-text="Cortana 起動時アプリのスクリーンショット":::

以下は、音声コマンド機能を追加し、音声またはキーボード入力を使うアプリに Cortana を統合する基本的な手順です。

1. VCD ファイルを作ります。 これは、アプリをアクティブ化して操作を開始するかコマンドを呼び出すためにユーザーが発声できる音声コマンドをすべて定義する XML ドキュメントです。 「 [**VCD の要素と属性**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)v1.0」を参照してください。
2. アプリが起動したら、VCD ファイル内のコマンド セットを登録します。
3. 音声コマンドによるアクティブ化、アプリ内でのナビゲーション、コマンドの実行を処理します。

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

## <a name="create-a-new-solution-with-project-in-visual-studio"></a>Visual Studio でプロジェクトを使って新しいソリューションを作成する

1. Microsoft Visual Studio 2015 を起動します。

    Visual Studio 2015 のスタート画面が表示されます。

2. **[ファイル]** メニューで、 **[新規作成]**  >  **[プロジェクト]** の順に選択します

    **[新しいプロジェクト]** ダイアログ ボックスが表示されます。 ダイアログの左側のウィンドウで、表示するテンプレートの種類を選択できます。

3. 左側のウィンドウで、[ **インストールされている > テンプレート > Visual C \# > Windows**] を展開し、[ **ユニバーサル** テンプレート] グループを選択します。 ユニバーサル Windows プラットフォーム (UWP) アプリで使うことができるプロジェクト テンプレートの一覧がダイアログの中央のウィンドウに表示されます。
4. 中央のウィンドウで、**[空白のアプリ (ユニバーサル Windows)]** プロジェクト テンプレートを選びます。

    **[空白のアプリ]** テンプレートは、コンパイルして実行できる最小限の UWP アプリを作成しますが、ユーザー インターフェイス コントロールやデータは含まれていません。 コントロールは、このチュートリアルの途中でアプリに追加します。

5. **[名前]** ボックスで、プロジェクト名を入力します。 この例では、"AdventureWorks" を使用します。
6. **[OK]** をクリックしてプロジェクトを作成します。

    Microsoft Visual Studio によってプロジェクトが作られ、**ソリューション エクスプローラー** に表示されます。

## <a name="add-image-assets-to-project-and-specify-them-in-the-app-manifest"></a>画像アセットをプロジェクトに追加してアプリ マニフェストで指定する

UWP アプリでは、特定の設定とデバイス機能 (ハイ コントラスト、有効ピクセル、ロケールなど) に基づいて最適な画像を自動的に選択できます。 必要な作業は、画像を提供し、リソースのバージョンごとに、アプリ プロジェクト内で適切な名前付け規則とフォルダー構造を使用していることを確認することだけです。 推奨されるリソースのバージョンが提供されない場合、ユーザーの基本設定、身体能力、デバイスの種類、場所によって、アクセシビリティ、ローカライズ、画像の品質が影響を受ける可能性があります。

ハイコントラストとスケールファクターのイメージリソースの詳細については、「 [タイルとアイコン資産のガイドライン](/windows/uwp/app-resources/images-tailored-for-scale-theme-contrast)」を参照してください。

修飾子を使ってリソースに名前を付けます。 リソース修飾子は、リソースの特定のバージョンが使われるコンテキストを識別するフォルダーとファイル名の修飾子です。

標準の命名規則は、`foldername/qualifiername-value[_qualifiername-value]/filename.qualifiername-value[_qualifiername-value].ext` です。 たとえば、`images/logo.scale-100_contrast-white.png` は、コード内ではルート フォルダーとファイル名を使用して単に `images/logo.png` と参照されます。 「[修飾子を使ってリソースに名前を付ける方法](/previous-versions/windows/apps/hh965324(v=win.10))」をご覧ください。

ローカライズされたリソースや複数の解像度のリソースの提供を現在計画していない場合でも、文字列リソース ファイルに既定の言語をマークし (`en-US\resources.resw` など)、画像に既定のスケール ファクターをマークする (`logo.scale-100.png` など) ことをお勧めします。 ただし、100、200、400 の倍率のアセットを提供することをお勧めします。

> [!IMPORTANT]
> **Cortana** キャンバスのタイトル領域で使用されるアプリアイコンは、"package.appxmanifest" ファイルに指定されている Square44x44Logo アイコンです。

## <a name="create-a-vcd-file"></a>VCD ファイルの作成

1. Visual Studio で、プライマリ プロジェクト名を右クリックし、**[追加]、[新しい項目]** の順にクリックします。 **XML ファイル** を追加します。
2. [**VCD**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2) ファイルの名前 (この例では、"AdventureWorksCommands.xml") を入力し、[追加] をクリックします。
3. **ソリューション エクスプローラー** で、[**VCD**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2) ファイルを選びます。
4. [ **プロパティ** ] ウィンドウで、[ **ビルドアクション** ] を [ **コンテンツ**] に設定し、[ **出力ディレクトリにコピー** ] を [ **新しい場合はコピー** する] に設定します。

## <a name="edit-the-vcd-file"></a>VCD ファイルの編集

`https://schemas.microsoft.com/voicecommands/1.2` を指す **xmlns** 属性を持つ **VoiceCommands** 要素を追加します。

1. アプリがサポートする各言語に対して、アプリがサポートする音声コマンドを含む [**CommandSet**](/previous-versions/windows/dn722331(v=win.10)) 要素を作成します。

   複数の [**CommandSet**](/previous-versions/windows/dn722331(v=win.10)) 要素を宣言し、それぞれに異なる [**xml:lang**](/previous-versions/windows/dn722331(v=win.10)) 属性を指定することで、アプリをさまざまな市場で使うことができるようにします。 たとえば、米国のアプリには、英語の [**Commandset**](/previous-versions/windows/dn722331(v=win.10)) とスペイン語の [**commandset**](/previous-versions/windows/dn722331(v=win.10)) がある場合があります。

   > [!CAUTION]
   > 音声コマンドを使ってアプリをアクティブ化して操作を開始するには、ユーザーがデバイスで選んだ音声の言語に一致する言語を含む [**CommandSet**](/previous-versions/windows/dn722331(v=win.10)) を格納している VCD ファイルをアプリで登録する必要があります。 音声認識言語は、[ **設定 > システム > 音声 > 音声言語**] にあります。

2. サポートする各コマンドの **Command** 要素を追加します。 [**VCD**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)ファイルで宣言される各 **コマンド** には、次の情報が含まれている必要があります。

   - アプリケーションが実行時に音声コマンドを識別するために使用する **AppName** 属性。
   - ユーザーがコマンドを呼び出す方法を説明する語句を含む **Example** 要素。 ユーザーが「何と言ったらよいですか」、「ヘルプ」と言ったり、**[もっと見る]** をタップしたときに、**Cortana** にこの例が表示されます。
   - アプリがコマンドとして認識する単語または語句を含む **ListenFor** 要素。 各 **ListenFor** 要素には、コマンドに関連する特定の単語を含む 1 つまたは複数の **PhraseList** 要素への参照を格納できます。
  
> [!NOTE]
> **ListenFor** 要素はプログラムで変更できません。 ただし、**ListenFor** 要素に関連付けられた **PhraseList** 要素はプログラムで変更できます。 アプリケーションは、ユーザーがアプリを使うときに生成されたデータ セットに基づいて実行時に **PhraseList** の内容を変更する必要があります。 「 [Cortana の VCD 語句リストを動的に変更](cortana-dynamically-modify-voice-command-definition-vcd-phrase-lists.md)する」を参照してください。

アプリケーションが起動したときに **Cortana** に表示されて読み上げられるテキストを含む **Feedback** 要素。

**Navigate** 要素は、音声コマンドにアプリをフォアグラウンドでアクティブ化するように指示します。 この例では、`showTripToDestination` コマンドはフォアグラウンド タスクです。

**VoiceCommandService** 要素は、音声コマンドにアプリをバックグラウンドでアクティブ化するように指示します。 この要素の **Target** 属性の値は、package.appxmanifest ファイルにある [**uap:AppService**](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-appservice) 要素の **Name** 属性の値に一致する必要があります。 この例では、`whenIsTripToDestination` コマンドと `cancelTripToDestination` コマンドは、アプリ サービス名を "AdventureWorksVoiceCommandService" として指定するバックグラウンド タスクです。

詳しくは、「[**VCD 要素および属性 v1.2**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)」をご覧ください。

**Adventure Works** アプリ向けの en-us 音声コマンドを定義する [**VCD**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2) ファイルの一部は次のとおりです。

```xml
<?xml version="1.0" encoding="utf-8" ?>
<VoiceCommands xmlns="https://schemas.microsoft.com/voicecommands/1.2">
  <CommandSet xml:lang="en-us" Name="AdventureWorksCommandSet_en-us">
    <AppName> Adventure Works </AppName>
    <Example> Show trip to London </Example>

    <Command Name="showTripToDestination">
      <Example> Show trip to London </Example>
      <ListenFor RequireAppName="BeforeOrAfterPhrase"> show [my] trip to {destination} </ListenFor>
      <ListenFor RequireAppName="ExplicitlySpecified"> show [my] {builtin:AppName} trip to {destination} </ListenFor>
      <Feedback> Showing trip to {destination} </Feedback>
      <Navigate />
    </Command>

    <Command Name="whenIsTripToDestination">
      <Example> When is my trip to Las Vegas?</Example>
      <ListenFor RequireAppName="BeforeOrAfterPhrase"> when is [my] trip to {destination}</ListenFor>
      <ListenFor RequireAppName="ExplicitlySpecified"> when is [my] {builtin:AppName} trip to {destination} </ListenFor>
      <Feedback> Looking for trip to {destination}</Feedback>
      <VoiceCommandService Target="AdventureWorksVoiceCommandService"/>
    </Command>
    
    <Command Name="cancelTripToDestination">
      <Example> Cancel my trip to Las Vegas </Example>
      <ListenFor RequireAppName="BeforeOrAfterPhrase"> cancel [my] trip to {destination}</ListenFor>
      <ListenFor RequireAppName="ExplicitlySpecified"> cancel [my] {builtin:AppName} trip to {destination} </ListenFor>
      <Feedback> Cancelling trip to {destination}</Feedback>
      <VoiceCommandService Target="AdventureWorksVoiceCommandService"/>
    </Command>

    <PhraseList Label="destination">
      <Item>London</Item>
      <Item>Las Vegas</Item>
      <Item>Melbourne</Item>
      <Item>Yosemite National Park</Item>
    </PhraseList>
  </CommandSet>
```

## <a name="install-the-vcd-commands"></a>VCD コマンドのインストール

VCD をインストールするには、アプリを 1 回実行する必要があります。

> [!NOTE]
> 音声コマンド データは、アプリのインストールで保持されません。 アプリの音声コマンド データをそのまま保持する場合は、アプリを起動またはアクティブ化するたびに VCD ファイルを初期化するか、VCD が現在インストールされているかどうかを示す設定を保持することを考慮してください。

"app.xaml.cs" ファイルで、次の操作を実行します。

1. 次の using ディレクティブを追加します。

    ```csharp
    using Windows.Storage;
    ```

1. "OnLaunched" メソッドを async 修飾子でマークします。  

    ```csharp
    protected async override void OnLaunched(LaunchActivatedEventArgs e)
    ```

1. [**OnLaunched**](/uwp/api/Windows.UI.Xaml.Application) ハンドラーの [**InstallCommandDefinitionsFromStorageFileAsync**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandDefinitionManager) を呼び出し、システムが認識する必要のある音声コマンドを登録します。

  Adventure Works のサンプルで、まず [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) オブジェクトを定義します。

  次に、[**GetFileAsync**](/uwp/api/Windows.Storage.StorageFolder) を呼び出し、"AdventureWorksCommands.xml" ファイルを使って初期化します。

  この [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) オブジェクトは、 [**InstallCommandDefinitionsFromStorageFileAsync**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandDefinitionManager)に渡されます。

```csharp
try
{
  // Install the main VCD. 
  StorageFile vcdStorageFile = 
  await Package.Current.InstalledLocation.GetFileAsync(
  @"AdventureWorksCommands.xml");

  await Windows.ApplicationModel.VoiceCommands.VoiceCommandDefinitionManager.
InstallCommandDefinitionsFromStorageFileAsync(vcdStorageFile);

  // Update phrase list.
  ViewModel.ViewModelLocator locator = App.Current.Resources["ViewModelLocator"] as ViewModel.ViewModelLocator;
  if(locator != null)
  {
     await locator.TripViewModel.UpdateDestinationPhraseList();
  }
}
catch (Exception ex)
{
  System.Diagnostics.Debug.WriteLine("Installing Voice Commands Failed: " + ex.ToString());
}
```

## <a name="handle-activation-and-execute-voice-commands"></a>アクティブ化の処理と音声コマンドの実行

アプリが少なくとも 1 回起動されて音声コマンド セットがインストールされた後、それ以降の音声コマンドのアクティブ化にアプリがどのように応答するかを指定します。

1. アプリが音声コマンドによってアクティブになったことを確認します。

    [**Application.OnActivated**](/uwp/api/Windows.UI.Xaml.Application) イベントをオーバーライドし、[**IActivatedEventArgs**](/uwp/api/Windows.ApplicationModel.Activation.IActivatedEventArgs).[**Kind**](/uwp/api/Windows.ApplicationModel.Activation.IActivatedEventArgs) が [**VoiceCommand**](/uwp/api/Windows.ApplicationModel.Activation.ActivationKind) かどうかを確認します。

2. コマンドの名前と内容を判断します。

    [**VoiceCommandActivatedEventArgs**](/uwp/api/Windows.ApplicationModel.Activation.VoiceCommandActivatedEventArgs) オブジェクトへの参照を [**IActivatedEventArgs**](/uwp/api/Windows.ApplicationModel.Activation.IActivatedEventArgs) から取得し、[**SpeechRecognitionResult**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResult) オブジェクトの [**Result**](/uwp/api/Windows.ApplicationModel.Activation.VoiceCommandActivatedEventArgs) プロパティを照会します。

    ユーザーが声に出した内容を判断するには、[**Text**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResult) の値か、[**SpeechRecognitionSemanticInterpretation**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionSemanticInterpretation) ディクショナリ内の認識された語句のセマンティクス プロパティを確認します。

3. アプリで適切なアクションを実行します (希望するページに移動するなど)。

この例では、手順 3 の「VCD ファイルの編集」の VCD について思い出す必要があります。

音声コマンドの音声認識の結果を取得したら、[**RulePath**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResult) 配列の最初の値からコマンド名を取得します。 VCD ファイルでは考えられる複数の音声コマンドが定義されたため、その値を VCD 内のコマンド名と比較し、適切なアクションを実行する必要があります。

アプリケーションが実行できる最も一般的なアクションは、音声コマンドのコンテキスト関連するコンテキストを含むページへの移動です。 この例では、**TripPage** ページに移動し、音声コマンドの値、コマンドがどのように入力されたか、および認識された "destination" 語句 (該当する場合) を渡します。 または、ページに移動するときにアプリがナビゲーション パラメーターを [**SpeechRecognitionResult**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResult) に送ることができます。

アプリを起動した音声コマンドが実際に発声されたかどうか、または **commandMode** キーを使って [**SpeechRecognitionSemanticInterpretation.Properties**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionSemanticInterpretation) ディクショナリからテキストとして入力されたかどうかを確認できます。 このキーの値は、"voice"、"text" のいずれかです。 キーの値が "voice" の場合、音声合成 ([**Windows.Media.SpeechSynthesis**](/uwp/api/Windows.Media.SpeechSynthesis)) を使って、発声されたフィードバックをアプリでユーザーに示すことを検討してください。

[**SpeechRecognitionSemanticInterpretation.Properties**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionSemanticInterpretation) を使って、**ListenFor** 要素の **PhraseList** 制約または **PhraseTopic** 制約で発声されたコンテンツを調べます。 ディクショナリのキーは、**PhraseList** 要素または **PhraseTopic** 要素の **Label** 属性値です。 **{destination}** 語句の値にアクセスする方法を次に示します。

```csharp
/// <summary>
/// Entry point for an application activated by some means other than normal launching. 
/// This includes voice commands, URI, share target from another app, and so on. 
/// 
/// NOTE:
/// A previous version of the VCD file might remain in place 
/// if you modify it and update the app through the store. 
/// Activations might include commands from older versions of your VCD. 
/// Try to handle these commands gracefully.
/// </summary>
/// <param name="args">Details about the activation method.</param>
protected override void OnActivated(IActivatedEventArgs args)
{
    base.OnActivated(args);

    Type navigationToPageType;
    ViewModel.TripVoiceCommand? navigationCommand = null;

    // Voice command activation.
    if (args.Kind == ActivationKind.VoiceCommand)
    {
        // Event args can represent many different activation types. 
        // Cast it so we can get the parameters we care about out.
        var commandArgs = args as VoiceCommandActivatedEventArgs;

        Windows.Media.SpeechRecognition.SpeechRecognitionResult speechRecognitionResult = commandArgs.Result;

        // Get the name of the voice command and the text spoken. 
        // See VoiceCommands.xml for supported voice commands.
        string voiceCommandName = speechRecognitionResult.RulePath[0];
        string textSpoken = speechRecognitionResult.Text;

        // commandMode indicates whether the command was entered using speech or text.
        // Apps should respect text mode by providing silent (text) feedback.
        string commandMode = this.SemanticInterpretation("commandMode", speechRecognitionResult);
        
        switch (voiceCommandName)
        {
            case "showTripToDestination":
                // Access the value of {destination} in the voice command.
                string destination = this.SemanticInterpretation("destination", speechRecognitionResult);

                // Create a navigation command object to pass to the page. 
                navigationCommand = new ViewModel.TripVoiceCommand(
                    voiceCommandName,
                    commandMode,
                    textSpoken,
                    destination);

                // Set the page to navigate to for this voice command.
                navigationToPageType = typeof(View.TripDetails);
                break;
            default:
                // If we can't determine what page to launch, go to the default entry point.
                navigationToPageType = typeof(View.TripListView);
                break;
        }
    }
    // Protocol activation occurs when a card is clicked within Cortana (using a background task).
    else if (args.Kind == ActivationKind.Protocol)
    {
        // Extract the launch context. In this case, we're just using the destination from the phrase set (passed
        // along in the background task inside Cortana), which makes no attempt to be unique. A unique id or 
        // identifier is ideal for more complex scenarios. We let the destination page check if the 
        // destination trip still exists, and navigate back to the trip list if it doesn't.
        var commandArgs = args as ProtocolActivatedEventArgs;
        Windows.Foundation.WwwFormUrlDecoder decoder = new Windows.Foundation.WwwFormUrlDecoder(commandArgs.Uri.Query);
        var destination = decoder.GetFirstValueByName("LaunchContext");

        navigationCommand = new ViewModel.TripVoiceCommand(
                                "protocolLaunch",
                                "text",
                                "destination",
                                destination);

        navigationToPageType = typeof(View.TripDetails);
    }
    else
    {
        // If we were launched via any other mechanism, fall back to the main page view.
        // Otherwise, we'll hang at a splash screen.
        navigationToPageType = typeof(View.TripListView);
    }

    // Repeat the same basic initialization as OnLaunched() above, taking into account whether
    // or not the app is already active.
    Frame rootFrame = Window.Current.Content as Frame;

    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active.
    if (rootFrame == null)
    {
        // Create a frame to act as the navigation context and navigate to the first page.
        rootFrame = new Frame();
        App.NavigationService = new NavigationService(rootFrame);

        rootFrame.NavigationFailed += OnNavigationFailed;

        // Place the frame in the current window.
        Window.Current.Content = rootFrame;
    }

    // Since we're expecting to always show a details page, navigate even if 
    // a content frame is in place (unlike OnLaunched).
    // Navigate to either the main trip list page, or if a valid voice command
    // was provided, to the details page for that trip.
    rootFrame.Navigate(navigationToPageType, navigationCommand);

    // Ensure the current window is active
    Window.Current.Activate();
}

/// <summary>
/// Returns the semantic interpretation of a speech result. 
/// Returns null if there is no interpretation for that key.
/// </summary>
/// <param name="interpretationKey">The interpretation key.</param>
/// <param name="speechRecognitionResult">The speech recognition result to get the semantic interpretation from.</param>
/// <returns></returns>
private string SemanticInterpretation(string interpretationKey, SpeechRecognitionResult speechRecognitionResult)
{
  return speechRecognitionResult.SemanticInterpretation.Properties[interpretationKey].FirstOrDefault();
}
```

## <a name="related-articles"></a>関連記事

- [Windows アプリにおける Cortana のやり取り](cortana-interactions.md)
- [音声コマンドを使用して Cortana でバックグラウンドアプリをアクティブ化する](cortana-launch-a-background-app-with-voice-commands.md)
- [VCD 要素および属性 v1.2](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)
- [Cortana のデザインガイドライン](cortana-design-guidelines.md)
- [Cortana 音声コマンドのサンプル](https://go.microsoft.com/fwlink/p/?LinkID=619899)
