---
title: Cortana のバックグラウンドアプリから前景アプリへのディープリンク-Cortana UWP の設計と開発
description: '**Cortana** でバックグラウンド アプリからのディープ リンクを提供し、フォアグラウンドに特定の状態やコンテキストでアプリを起動します。'
ms.assetid: 6fe5fcc5-9ee4-4c04-92f4-7b1bf7ef5651
ms.date: 01/28/2021
ms.topic: article
keywords: Cortana
ms.openlocfilehash: d96e54604c5def61802a77625a6c18c556db909d
ms.sourcegitcommit: 8fe992f3a6d8f7975af4911ad88e855bee50083e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/05/2021
ms.locfileid: "99606057"
---
# <a name="deep-link-from-a-background-app-in-cortana-to-a-foreground-app"></a>Cortana のバックグラウンドアプリから前景アプリへのディープリンク

>[!WARNING]
> この機能は、Windows 10 2020 年5月の更新プログラム (バージョン2004、コードネーム "20H1") ではサポートされなくなりました。
>
> Cortana が最新の生産性エクスペリエンスを変革する方法については [、Microsoft 365 の cortana](/microsoft-365/admin/misc/cortana-integration) を参照してください。

**Cortana** でバックグラウンド アプリからのディープ リンクを提供し、フォアグラウンドに特定の状態やコンテキストでアプリを起動します。

> [!NOTE]
> フォアグラウンド アプリが起動すると、**Cortana** とバックグラウンド アプリ サービスはいずれも終了します。

既定では、ディープ リンクは、ここに示されているように **Cortana** の完了画面 ("AdventureWorks に移動") に表示されますが、他のさまざまな画面にもディープ リンクを表示できます。

:::image type="content" source="images/cortana/cortana-completion-screen-upcomingtrip-small.png" alt-text="今後の旅行における Cortana バックグラウンドアプリの完了のスクリーンショット":::

> [!NOTE]
> **重要な API**
>
> - [**Windows.ApplicationModel.VoiceCommands**](/uwp/api/Windows.ApplicationModel.VoiceCommands)
> - [**VCD 要素および属性 v1.2**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)

## <a name="overview"></a>概要

ユーザーは、次のような方法で **Cortana** 経由でアプリにアクセスできます。

- アプリをフォアグラウンド アプリとしてアクティブ化する (「[Cortana の音声コマンドを使ったフォアグラウンド アプリのアクティブ化](cortana-launch-a-foreground-app-with-voice-commands.md)」をご覧ください)。
- バックグラウンドアプリサービスとして特定の機能を公開する (「 [音声コマンドを使用した Cortana でのバックグラウンドアプリのアクティブ化](cortana-launch-a-background-app-with-voice-commands.md)」を参照してください)。
- 特定のページ、コンテンツ、状態やコンテキストへのディープ リンク。

ここでは、ディープ リンクについて説明します。

ディープ リンクは、(スタート メニューからアプリを起動することをユーザーに要求する代わりに) Cortana とアプリ サービスがすべての機能を備えたアプリへのゲートウェイとなっている場合や、Cortana 経由では不可能なアプリ内の充実した詳細情報や機能へのアクセスを提供する場合に便利です。 ディープ リンクは、操作性を向上させ、アプリを宣伝するためのもう 1 つの方法です。

ディープ リンクを提供するには、3 つの方法があります。

- さまざまな **Cortana** 画面にある "&lt;アプリ&gt; に移動" リンク。
- さまざまな **Cortana** 画面のコンテンツ タイルに埋め込まれているリンク。
- プログラムによるバックグラウンド アプリ サービスからのフォアグラウンド アプリの起動。

## <a name="go-to-ltappgt-deep-link"></a>"&lt;アプリ&gt; に移動" のディープ リンク

**Cortana** では、ほとんどの画面のコンテンツ カードの下に "&lt;アプリ&gt; に移動" ディープ リンクが表示されます。

:::image type="content" source="images/cortana/cortana-completion-screen.png" alt-text="バックグラウンドアプリの完了画面で Cortana の [アプリにアクセス] ディープリンクのスクリーンショット。":::

このリンクに、アプリ サービスと同様のコンテキストでアプリを開く起動引数を指定できます。 起動引数を指定しない場合、アプリはメイン画面で起動されます。

この例では、指定した目的地 (`destination`) の文字列をサンプルの **AdventureWorks** の AdventureWorksVoiceCommandService.cs から SendCompletionMessageForDestination メソッドに渡します。このメソッドは、一致するすべての旅行を受け取り、アプリへのディープ リンクを提供します。

まず、 [](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandUserMessage) ```userMessage``` **cortana** によって読み上げられ、 **Cortana** キャンバスに表示される VoiceCommandUserMessage () を作成します。 結果のカード コレクションをキャンバスに表示するための [**VoiceCommandContentTile**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandContentTile) リスト オブジェクトが作成されます。

これら 2 つのオブジェクトが [**VoiceCommandResponse**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandResponse) オブジェクトの [CreateResponse](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandResponse)メソッド (`response`) に渡されます。 次に、応答オブジェクトの [**AppLaunchArgument**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandResponse) プロパティの値を、この関数に渡された `destination` の値に設定します。 ユーザーが Cortana のキャンバス上のコンテンツ タイルをタップすると、パラメーターの値は応答オブジェクトを介してアプリに渡されます。

最後に、[**VoiceCommandServiceConnection**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection) の [**ReportSuccessAsync**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection) メソッドを呼び出します。

```csharp
/// <summary>
/// Show details for a single trip, if the trip can be found. 
/// This demonstrates a simple response flow in Cortana.
/// </summary>
/// <param name="destination">The destination specified in the voice command.</param>
private async Task SendCompletionMessageForDestination(string destination)
{
...
    IEnumerable<Model.Trip> trips = store.Trips.Where(p => p.Destination == destination);

    var userMessage = new VoiceCommandUserMessage();
    var destinationsContentTiles = new List<VoiceCommandContentTile>();
...
    var response = VoiceCommandResponse.CreateResponse(userMessage, destinationsContentTiles);

    if (trips.Count() > 0)
    {
        response.AppLaunchArgument = destination;
    }

    await voiceServiceConnection.ReportSuccessAsync(response);
}
```

## <a name="content-tile-deep-link"></a>コンテンツ タイルのディープ リンク

ディープ リンクは、さまざまな **Cortana** 画面にあるコンテンツ カードに追加できます。

ハンドオフ画面を :::image type="content" source="images/cortana/cortana-backgroundapp-progress-result.png" alt-text="使用したエンドツーエンド cortana のバックグラウンドアプリフローのスクリーンショット":::*[次回の旅行*] とハンドオフ画面

"&lt;アプリ&gt; に移動" のリンクと同様に、アプリ サービスと同様のコンテキストでアプリを開くために、起動引数を指定できます。 起動引数を指定しない場合、コンテンツ タイルはアプリにリンクされません。

この例では、指定した目的地をサンプルの **AdventureWorks** の AdventureWorksVoiceCommandService.cs から SendCompletionMessageForDestination メソッドに渡します。このメソッドは、一致するすべての旅行を受け取り、アプリへのディープ リンクをコンテンツ カードに提供します。

まず、 [](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandUserMessage) ```userMessage``` **cortana** によって読み上げられ、 **Cortana** キャンバスに表示される VoiceCommandUserMessage () を作成します。 結果のカード コレクションをキャンバスに表示するための [**VoiceCommandContentTile**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandContentTile) リスト オブジェクトが作成されます。 

これら 2 つのオブジェクトが [**VoiceCommandResponse**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandResponse) オブジェクトの [CreateResponse](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandResponse)メソッド (```response```) に渡されます。 次に、[**AppLaunchArgument**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandResponse) プロパティの値を音声コマンドの目的地の値に設定します。

最後に、[**VoiceCommandServiceConnection**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection) の [**ReportSuccessAsync**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection) メソッドを呼び出します。
ここでは、[**VoiceCommandServiceConnection**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection) オブジェクトの [**ReportSuccessAsync**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection) の呼び出しで使用される [**VoiceCommandContentTile**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandContentTile) リストに、異なる [**AppLaunchArgument**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandResponse) パラメーター値を持つ 2 つのコンテンツ タイルを追加します。

```csharp
/// <summary>
/// Show details for a single trip, if the trip can be found. 
/// This demonstrates a simple response flow in Cortana.
/// </summary>
/// <param name="destination">The destination specified in the voice command.</param>
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

## <a name="programmatic-deep-link"></a>プログラムによるディープ リンク

アプリ サービスと同様のコンテキストでアプリを開くために、起動引数を指定して、プログラムによってアプリを起動することもできます。 起動引数を指定しない場合、アプリはメイン画面で起動されます。

ここでは、[**VoiceCommandServiceConnection**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection) オブジェクトの [**RequestAppLaunchAsync**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection) の呼び出しで使用される [**VoiceCommandResponse**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandResponse) オブジェクトに、"Las Vegas" という値の [**AppLaunchArgument**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandResponse) パラメーターを追加します。

```CSharp
var userMessage = new VoiceCommandUserMessage();
userMessage.DisplayMessage = "Here are your trips.";
userMessage.SpokenMessage = 
  "You have one trip to Vegas coming up.";

response = VoiceCommandResponse.CreateResponse(userMessage);
response.AppLaunchArgument = “Las Vegas”;
await  VoiceCommandServiceConnection.RequestAppLaunchAsync(response);
```

## <a name="app-manifest"></a>アプリ マニフェスト

アプリへのディープ リンクを有効にするには、アプリ プロジェクトの Package.appxmanifest ファイルで、`windows.personalAssistantLaunch` 拡張機能を宣言する必要があります

ここでは、**Adventure Works** アプリの `windows.personalAssistantLaunch` 拡張機能を宣言します。

```XML
<Extensions>
  <uap:Extension Category="windows.appService" 
    EntryPoint="AdventureWorks.VoiceCommands.AdventureWorksVoiceCommandService">
    <uap:AppService Name="AdventureWorksVoiceCommandService"/>
  </uap:Extension>
  <uap:Extension Category="windows.personalAssistantLaunch"/> 
</Extensions>
```

## <a name="protocol-contract"></a>プロトコル コントラクト

アプリは、[**Protocol**](/uwp/api/Windows.ApplicationModel.Activation.ActivationKind) コントラクトを使用して、Uniform Resource Identifier (URI) アクティブ化によってフォアグラウンドで起動されます。 アプリは、アプリの [**OnActivated**](/uwp/api/Windows.UI.Xaml.Application) イベントをオーバーライドし、**Protocol** の **ActivationKind** を確認する必要があります。 詳しくは、「[URI のアクティブ化の処理](/windows/uwp/launch-resume/handle-uri-activation)」をご覧ください。

ここでは、[**ProtocolActivatedEventArgs**](/uwp/api/Windows.ApplicationModel.Activation.ProtocolActivatedEventArgs) によって提供される URI をデコードして、起動引数にアクセスします。 この例では、[**Uri**](/uwp/api/Windows.ApplicationModel.Activation.ProtocolActivatedEventArgs) は "windows.personalassistantlaunch:?LaunchContext=Las Vegas" に設定されています。

```CSharp
if (args.Kind == ActivationKind.Protocol)
  {
    var commandArgs = args as ProtocolActivatedEventArgs;
    Windows.Foundation.WwwFormUrlDecoder decoder = 
      new Windows.Foundation.WwwFormUrlDecoder(commandArgs.Uri.Query);
    var destination = decoder.GetFirstValueByName("LaunchContext");

    navigationCommand = new ViewModel.TripVoiceCommand(
      "protocolLaunch",
      "text",
      "destination",
      destination);

    navigationToPageType = typeof(View.TripDetails);

    rootFrame.Navigate(navigationToPageType, navigationCommand);

    // Ensure the current window is active.
    Window.Current.Activate();
  }
```

## <a name="related-articles"></a>関連記事

- [Windows アプリにおける Cortana のやり取り](cortana-interactions.md)
- [Cortana のデザインガイドライン](cortana-design-guidelines.md)
- [VCD 要素および属性 v1.2](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)
- [Cortana 音声コマンドのサンプル](https://go.microsoft.com/fwlink/p/?LinkID=619899)
