---
title: Cortana でのバックグラウンドアプリとの対話-Cortana UWP の設計と開発
description: 音声コマンドの実行中に、Cortana キャンバスでの音声認識とテキストの入力により、バック グラウンド アプリへのユーザー操作が可能になります。
ms.assetid: e42917dc-aece-4880-813f-80b897f9126c
ms.date: 01/28/2021
ms.topic: article
keywords: Cortana
ms.openlocfilehash: 835a2f60d2b86e5bef49195d4f937fa844f4d921
ms.sourcegitcommit: 8fe992f3a6d8f7975af4911ad88e855bee50083e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/05/2021
ms.locfileid: "99606077"
---
# <a name="interact-with-a-background-app-in-cortana"></a>Cortana でのバックグラウンド アプリの操作

>[!WARNING]
> この機能は、Windows 10 2020 年5月の更新プログラム (バージョン2004、コードネーム "20H1") ではサポートされなくなりました。
>
> Cortana が最新の生産性エクスペリエンスを変革する方法については [、Microsoft 365 の cortana](/microsoft-365/admin/misc/cortana-integration) を参照してください。

音声コマンドの実行中に、**Cortana** キャンバスでの音声認識とテキストの入力により、バック グラウンド アプリへのユーザー操作が可能になります。

> [!NOTE]
> **重要な API**
>
> - [**Windows.ApplicationModel.VoiceCommands**](/uwp/api/Windows.ApplicationModel.VoiceCommands)
> - [**VCD 要素および属性 v1.2**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)

Cortana は、アプリでの完全なターン バイ ターン方式のワークフローをサポートします。 このワークフローはアプリによって定義され、次の機能をサポートできます。 

- 正常に完了しました
- ハンドオフ
- 進捗状況
- 確認
- 曖昧性の除去
- エラー

## <a name="composing-feedback-strings"></a>フィードバック文字列の作成

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

**Cortana** が表示または会話するフィードバック文字列を構成します。

[Cortana のデザインガイドライン](cortana-design-guidelines.md)では、 **cortana** の文字列の作成に関する推奨事項を提供します。

コンテンツ カードは、ユーザーに追加のコンテキストを示し、フィードバック文字列を簡潔に保つのに役立ちます。

**Cortana** では、次のコンテンツ カード テンプレートがサポートされます (完了画面で使うことができるテンプレートは 1 つだけです)。

- タイトルのみ
- 最大 3 行のテキストを含むタイトル
- イメージを含むタイトル
- イメージを含むタイトルと最大3行のテキスト

以下の画像を使うことができます。

- 幅 68 x 高さ 68
- 幅 68 x 高さ 92
- 幅 280 x 高さ 140

ユーザーのカードやアプリへのテキスト リンクをクリックして、アプリをフォアグラウンドで起動できるようにすることもできます。

## <a name="completion-screen"></a>完了画面

完了画面には、完了した音声コマンド タスクに関する情報が表示されます。

アプリの応答に 500 ミリ秒未満しかかからず、ユーザーからの追加情報を必要としないタスクは、それ以上の **Cortana** とのやり取りなしで完了します。 Cortana は、単に完了画面を表示します。

ここでは、**Adventure Works** アプリを使って、ロンドンへの旅行予定を表示して、音声コマンドを要求する完了画面を表示します。

:::image type="content" source="images/cortana/cortana-completion-screen-upcomingtrip-small.png" alt-text="今後の旅行における Cortana バックグラウンドアプリの完了のスクリーンショット":::

音声コマンドは AdventureWorksCommands.xml で定義されています。

```xml
<Command Name="whenIsTripToDestination">
  <Example> When is my trip to Las Vegas?</Example>
  <ListenFor RequireAppName="BeforeOrAfterPhrase"> when is [my] trip to {destination}</ListenFor>
  <ListenFor RequireAppName="ExplicitlySpecified"> when is [my] {builtin:AppName} trip to {destination} </ListenFor>
  <Feedback> Looking for trip to {destination}</Feedback>
  <VoiceCommandService Target="AdventureWorksVoiceCommandService"/>
</Command>
```

AdventureWorksVoiceCommandService.cs には、完了メッセージのメソッドが含まれています。

```csharp
/// <summary>
/// Show details for a single trip, if the trip can be found. 
/// This demonstrates a simple response flow in Cortana.
/// </summary>
/// <param name="destination">The destination, expected to be in the phrase list.</param>
private async Task SendCompletionMessageForDestination(string destination)
{
    // If this operation is expected to take longer than 0.5 seconds, the task must
    // supply a progress response to Cortana before starting the operation, and
    // updates must be provided at least every 5 seconds.
    string loadingTripToDestination = string.Format(
               cortanaResourceMap.GetValue("LoadingTripToDestination", cortanaContext).ValueAsString,
               destination);
    await ShowProgressScreen(loadingTripToDestination);
    Model.TripStore store = new Model.TripStore();
    await store.LoadTrips();

    // Query for the specified trip. 
    // The destination should be in the phrase list. However, there might be  
    // multiple trips to the destination. We pick the first.
    IEnumerable<Model.Trip> trips = store.Trips.Where(p => p.Destination == destination);

    var userMessage = new VoiceCommandUserMessage();
    var destinationsContentTiles = new List<VoiceCommandContentTile>();
    if (trips.Count() == 0)
    {
        string foundNoTripToDestination = string.Format(
               cortanaResourceMap.GetValue("FoundNoTripToDestination", cortanaContext).ValueAsString,
               destination);
        userMessage.DisplayMessage = foundNoTripToDestination;
        userMessage.SpokenMessage = foundNoTripToDestination;
    }
    else
    {
        // Set plural or singular title.
        string message = "";
        if (trips.Count() > 1)
        {
            message = cortanaResourceMap.GetValue("PluralUpcomingTrips", cortanaContext).ValueAsString;
        }
        else
        {
            message = cortanaResourceMap.GetValue("SingularUpcomingTrip", cortanaContext).ValueAsString;
        }
        userMessage.DisplayMessage = message;
        userMessage.SpokenMessage = message;

        // Define a tile for each destination.
        foreach (Model.Trip trip in trips)
        {
            int i = 1;
            
            var destinationTile = new VoiceCommandContentTile();

            destinationTile.ContentTileType = VoiceCommandContentTileType.TitleWith68x68IconAndText;
            destinationTile.Image = await StorageFile.GetFileFromApplicationUriAsync(new Uri("ms-appx:///AdventureWorks.VoiceCommands/Images/GreyTile.png"));

            destinationTile.AppLaunchArgument = trip.Destination;
            destinationTile.Title = trip.Destination;
            if (trip.StartDate != null)
            {
                destinationTile.TextLine1 = trip.StartDate.Value.ToString(dateFormatInfo.LongDatePattern);
            }
            else
            {
                destinationTile.TextLine1 = trip.Destination + " " + i;
            }

            destinationsContentTiles.Add(destinationTile);
            i++;
        }
    }

    var response = VoiceCommandResponse.CreateResponse(userMessage, destinationsContentTiles);

    if (trips.Count() > 0)
    {
        response.AppLaunchArgument = destination;
    }

    await voiceServiceConnection.ReportSuccessAsync(response);
}
```

## <a name="hand-off-screen"></a>ハンドオフ画面

音声コマンドが認識されると、 **cortana** は ReportSuccessAsync を呼び出す必要があります。また、app service が500ミリ秒内の音声コマンドによって指定されたアクションを完了できない場合、 **cortana** は、アプリが ReportSuccessAsync を呼び出すか、最大5秒間、表示されるハンドオフ画面を表示します。

アプリ サービスが ReportSuccessAsync を呼び出さないか、または他の VoiceCommandServiceConnection メソッドを呼び出さない場合、エラー メッセージが表示されてアプリ サービス呼び出しがキャンセルされます。

**Adventure Works** アプリのハンドオフ画面の例を次に示します。 この例では、ユーザーは **Cortana** に旅行の予定を照会しました。 ハンドオフ画面には、アプリ サービス名を使ってカスタマイズされたメッセージ、アイコン、**フィードバック** 文字列が表示されます。 

[!NOTE] **フィードバック** 文字列は、VCD ファイル内で宣言できます。 この文字列は、Cortana のキャンバスに表示される UI テキストには影響しません。**Cortana** によって読み上げられるテキストにのみ影響します。

:::image type="content" source="images/cortana/cortana-backgroundapp-progress-result.png" alt-text="Cortana バックグラウンドアプリのハンドオフ画面のスクリーンショット":::

## <a name="progress-screen"></a>進行状況画面

アプリ サービスが ReportSuccessAsync を呼び出すために 500 ミリ秒より多くの時間がかかる場合、**Cortana** は進行状況画面をユーザーに表示します。 アプリ アイコンが表示されます。タスクがアクティブに処理されていることを示すため、GUI と TTS の両方の進行状況文字列を提供する必要があります。

**Cortana** には、最大 5 秒間進行状況画面が表示されます。 5 秒後、**Cortana** はユーザーにエラー メッセージを表示し、アプリ サービスが終了します。 アプリ サービスがアクションを完了するのに 5 秒以上必要とする場合、進行状況画面を表示して **Cortana** の更新を続けます。

**Adventure Works** アプリの進行状況画面の例を次に示します。 この例では、ユーザーはラスベガス旅行をキャンセルしました。 進行状況画面には、アクションに合わせてカスタマイズされたメッセージ、アイコン、および取り消す旅行についての情報を含むコンテンツ タイルが表示されます。

:::image type="content" source="images/cortana/cortana-progress-screen.png" alt-text="バックグラウンドアプリの進行状況を示す Cortana のスクリーンショット":::

AdventureWorksVoiceCommandService.cs には、次の進行状況メッセージ メソッドが含まれています。このメソッドは [**ReportProgressAsync**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection) を呼び出して、**Cortana** に進行状況画面を表示します。

```    CSharp
/// <summary>
/// Show a progress screen. These should be posted at least every 5 seconds for a 
/// long-running operation.
/// </summary>
/// <param name="message">The message to display, relating to the task being performed.</param>
/// <returns></returns>
private async Task ShowProgressScreen(string message)
{
    var userProgressMessage = new VoiceCommandUserMessage();
    userProgressMessage.DisplayMessage = userProgressMessage.SpokenMessage = message;

    VoiceCommandResponse response = VoiceCommandResponse.CreateResponse(userProgressMessage);
    await voiceServiceConnection.ReportProgressAsync(response);
}
```

## <a name="confirmation-screen"></a>確認画面

音声コマンドにより指定されたアクションが、元に戻すことができないアクション、大きな影響を与えるアクション、認識の信頼性が高くないアクションの場合、アプリ サービスは確認を要求できます。

**Adventure Works** アプリの確認画面の例を次に示します。 この例では、**Cortana** を使って、アプリ サービスに対してラスベガス旅行を取り消すように指示しました。 アプリ サービスは、旅行を取り消す前にユーザーに「はい」または「いいえ」の回答を求める確認画面を **Cortana** に提供しました。

ユーザーが「はい」または「いいえ」以外のことを言った場合、**Cortana** は質問に対する回答を特定できません。 この場合、**Cortana** はアプリ サービスによって提供されたものと同様の質問をユーザーに表示します。

2 つ目のプロンプトでもユーザーが「はい」または「いいえ」と言わない場合、**Cortana** はお詫びの言葉を前に添えて 3 度目の同じ質問をユーザーに表示します。 それでもユーザーが「はい」または「いいえ」と言わない場合、**Cortana** は音声入力の待機をやめ、代わりにボタンのタップをユーザーに求めます。

確認画面には、アクションに合わせてカスタマイズされたメッセージ、アイコン、および取り消す旅行についての情報を含むコンテンツ タイルが表示されます。

:::image type="content" source="images/cortana/cortana-confirmation-screen.png" alt-text="バックグラウンドアプリの確認画面が表示される Cortana のスクリーンショット":::

AdventureWorksVoiceCommandService.cs には、次の旅行取り消しメソッドが含まれています。このメソッドは [**RequestConfirmationAsync**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection) を呼び出して、**Cortana** に確認画面を表示します。

```    CSharp
/// <summary>
/// Handle the Trip Cancellation task. This task demonstrates how to prompt a user
/// for confirmation of an operation, show users a progress screen while performing
/// a long-running task, and show a completion screen.
/// </summary>
/// <param name="destination">The name of a destination.</param>
/// <returns></returns>
private async Task SendCompletionMessageForCancellation(string destination)
{
    // Begin loading data to search for the target store. 
    // Consider inserting a progress screen here, in order to prevent Cortana from timing out. 
    string progressScreenString = string.Format(
        cortanaResourceMap.GetValue("ProgressLookingForTripToDest", cortanaContext).ValueAsString,
        destination);
    await ShowProgressScreen(progressScreenString);

    Model.TripStore store = new Model.TripStore();
    await store.LoadTrips();

    IEnumerable<Model.Trip> trips = store.Trips.Where(p => p.Destination == destination);
    Model.Trip trip = null;
    if (trips.Count() > 1)
    {
        // If there is more than one trip, provide a disambiguation screen.
        // However, if a significant number of items are returned, you might want to 
        // just display a link to your app and provide a deeper search experience.
        string disambiguationDestinationString = string.Format(
            cortanaResourceMap.GetValue("DisambiguationWhichTripToDest", cortanaContext).ValueAsString,
            destination);
        string disambiguationRepeatString = cortanaResourceMap.GetValue("DisambiguationRepeat", cortanaContext).ValueAsString;
        trip = await DisambiguateTrips(trips, disambiguationDestinationString, disambiguationRepeatString);
    }
    else
    {
        trip = trips.FirstOrDefault();
    }

    var userPrompt = new VoiceCommandUserMessage();
    
    VoiceCommandResponse response;
    if (trip == null)
    {
        var userMessage = new VoiceCommandUserMessage();
        string noSuchTripToDestination = string.Format(
            cortanaResourceMap.GetValue("NoSuchTripToDestination", cortanaContext).ValueAsString,
            destination);
        userMessage.DisplayMessage = userMessage.SpokenMessage = noSuchTripToDestination;

        response = VoiceCommandResponse.CreateResponse(userMessage);
        await voiceServiceConnection.ReportSuccessAsync(response);
    }
    else
    {
        // Prompt the user for confirmation that this is the correct trip to cancel.
        string cancelTripToDestination = string.Format(
            cortanaResourceMap.GetValue("CancelTripToDestination", cortanaContext).ValueAsString,
            destination);
        userPrompt.DisplayMessage = userPrompt.SpokenMessage = cancelTripToDestination;
        var userReprompt = new VoiceCommandUserMessage();
        string confirmCancelTripToDestination = string.Format(
            cortanaResourceMap.GetValue("ConfirmCancelTripToDestination", cortanaContext).ValueAsString,
            destination);
        userReprompt.DisplayMessage = userReprompt.SpokenMessage = confirmCancelTripToDestination;
        
        response = VoiceCommandResponse.CreateResponseForPrompt(userPrompt, userReprompt);

        var voiceCommandConfirmation = await voiceServiceConnection.RequestConfirmationAsync(response);

        // If RequestConfirmationAsync returns null, Cortana has likely been dismissed.
        if (voiceCommandConfirmation != null)
        {
            if (voiceCommandConfirmation.Confirmed == true)
            {
                string cancellingTripToDestination = string.Format(
               cortanaResourceMap.GetValue("CancellingTripToDestination", cortanaContext).ValueAsString,
               destination);
                await ShowProgressScreen(cancellingTripToDestination);

                // Perform the operation to remove the trip from app data. 
                // As the background task runs within the app package of the installed app,
                // we can access local files belonging to the app without issue.
                await store.DeleteTrip(trip);

                // Provide a completion message to the user.
                var userMessage = new VoiceCommandUserMessage();
                string cancelledTripToDestination = string.Format(
                    cortanaResourceMap.GetValue("CancelledTripToDestination", cortanaContext).ValueAsString,
                    destination);
                userMessage.DisplayMessage = userMessage.SpokenMessage = cancelledTripToDestination;
                response = VoiceCommandResponse.CreateResponse(userMessage);
                await voiceServiceConnection.ReportSuccessAsync(response);
            }
            else
            {
                // Confirm no action for the user.
                var userMessage = new VoiceCommandUserMessage();
                string keepingTripToDestination = string.Format(
                    cortanaResourceMap.GetValue("KeepingTripToDestination", cortanaContext).ValueAsString,
                    destination);
                userMessage.DisplayMessage = userMessage.SpokenMessage = keepingTripToDestination;

                response = VoiceCommandResponse.CreateResponse(userMessage);
                await voiceServiceConnection.ReportSuccessAsync(response);
            }
        }
    }
}
```

## <a name="disambiguation-screen"></a>不明瞭解消画面

音声コマンドにより指定されたアクションに考えれる結果が複数ある場合、アプリ サービスはユーザーに詳細情報を要求できます。

**Adventure Works** アプリの不明瞭解消画面の例を次に示します。 この例では、**Cortana** を使って、アプリ サービスに対してラスベガス旅行を取り消すように指示しました。 ただし、ユーザーには異なる日付のラスベガス旅行が 2 つあるため、アプリ サービスはユーザーが目的の旅行を選ばないとアクションを完了できません。

アプリ サービスは、取り消す前に、一致する旅行の一覧から選ぶように求める不明瞭解消画面を **Cortana** に提供します。

この場合、**Cortana** はアプリ サービスによって提供されたものと同様の質問をユーザーに表示します。

2 つ目のプロンプトでもユーザーが選択内容を特定できる内容を言わない場合、**Cortana** はお詫びの言葉を前に添えて 3 度目の同じ質問をユーザーに表示します。 それでもユーザーが選択内容を特定できる内容を言わない場合、**Cortana** は音声入力の待機をやめ、代わりにボタンのタップをユーザーに求めます。

不明瞭解消画面には、アクションに合わせてカスタマイズされたメッセージ、アイコン、および取り消す旅行についての情報を含むコンテンツ タイルが表示されます。

:::image type="content" source="images/cortana/cortana-disambiguation-screen.png" alt-text="バックグラウンドアプリの非不明瞭画面を含む Cortana のスクリーンショット":::

AdventureWorksVoiceCommandService.cs には、次の旅行取り消しメソッドが含まれています。このメソッドは [**RequestDisambiguationAsync**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection) を呼び出して、**Cortana** に不明瞭解消画面を表示します。

```csharp
/// <summary>
/// Provide the user with a way to identify which trip to cancel. 
/// </summary>
/// <param name="trips">The set of trips</param>
/// <param name="disambiguationMessage">The initial disambiguation message</param>
/// <param name="secondDisambiguationMessage">Repeat prompt retry message</param>
private async Task<Model.Trip> DisambiguateTrips(IEnumerable<Model.Trip> trips, string disambiguationMessage, string secondDisambiguationMessage)
{
    // Create the first prompt message.
    var userPrompt = new VoiceCommandUserMessage();
    userPrompt.DisplayMessage =
        userPrompt.SpokenMessage = disambiguationMessage;

    // Create a re-prompt message if the user responds with an out-of-grammar response.
    var userReprompt = new VoiceCommandUserMessage();
    userReprompt.DisplayMessage =
        userReprompt.SpokenMessage = secondDisambiguationMessage;

    // Create card for each item. 
    var destinationContentTiles = new List<VoiceCommandContentTile>();
    int i = 1;
    foreach (Model.Trip trip in trips)
    {
        var destinationTile = new VoiceCommandContentTile();

        destinationTile.ContentTileType = VoiceCommandContentTileType.TitleWith68x68IconAndText;
        destinationTile.Image = await StorageFile.GetFileFromApplicationUriAsync(new Uri("ms-appx:///AdventureWorks.VoiceCommands/Images/GreyTile.png"));
        
        // The AppContext can be any arbitrary object.
        destinationTile.AppContext = trip;
        string dateFormat = "";
        if (trip.StartDate != null)
        {
            dateFormat = trip.StartDate.Value.ToString(dateFormatInfo.LongDatePattern);
        }
        else
        {
            // The app allows a trip to have no date.
            // However, the choices must be unique so they can be distinguished.
            // Here, we add a number to identify them.
            dateFormat = string.Format("{0}", i);
        } 

        destinationTile.Title = trip.Destination + " " + dateFormat;
        destinationTile.TextLine1 = trip.Description;

        destinationContentTiles.Add(destinationTile);
        i++;
    }

    // Cortana handles re-prompting if no valid response.
    var response = VoiceCommandResponse.CreateResponseForPrompt(userPrompt, userReprompt, destinationContentTiles);

    // If cortana is dismissed in this operation, null is returned.
    var voiceCommandDisambiguationResult = await
        voiceServiceConnection.RequestDisambiguationAsync(response);
    if (voiceCommandDisambiguationResult != null)
    {
        return (Model.Trip)voiceCommandDisambiguationResult.SelectedItem.AppContext;
    }

    return null;
}
```

## <a name="error-screen"></a>エラー画面

音声コマンドにより指定されたアクションを完了できない場合、アプリ サービスにエラー画面を表示できます。

**Adventure Works** アプリのエラー画面の例を次に示します。 この例では、**Cortana** を使って、アプリ サービスに対してラスベガス旅行を取り消すように指示しました。 ただし、ユーザーには予定されているラスベガス旅行がありません。

アプリ サービスは、アクションに合わせてカスタマイズされたメッセージ、アイコン、特定のエラー メッセージが表示されるエラー画面を **Cortana** に提供します。

[**ReportFailureAsync**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection) を呼び出して、**Cortana** にエラー画面を表示します。

```csharp
var userMessage = new VoiceCommandUserMessage();
    userMessage.DisplayMessage = userMessage.SpokenMessage = 
      "Sorry, you don't have any trips to Las Vegas";
                
    var response = VoiceCommandResponse.CreateResponse(userMessage);

    response.AppLaunchArgument = "showUpcomingTrips";
    await voiceServiceConnection.ReportFailureAsync(response);
```

## <a name="related-articles"></a>関連記事

- [Windows アプリにおける Cortana のやり取り](cortana-interactions.md)
- [VCD 要素および属性 v1.2](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)
- [Cortana のデザインガイドライン](cortana-design-guidelines.md)
- [Cortana 音声コマンドのサンプル](https://go.microsoft.com/fwlink/p/?LinkID=619899)
