---
description: UWP アプリからローカルトースト通知を送信し、ユーザーがトーストをクリックするのを処理する方法について説明します。
title: UWP アプリからローカルトースト通知を送信する
ms.assetid: E9AB7156-A29E-4ED7-B286-DA4A6E683638
label: Send a local toast notification from UWP apps
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp, トースト通知の送信, 通知, 通知の送信, トースト通知, 方法, クイックスタート, 作業の開始, コード サンプル, チュートリアル
ms.localizationpriority: medium
ms.openlocfilehash: 0a2e8c25aa7efcb96166b741a073122e3c077c08
ms.sourcegitcommit: 2a23972e9a0807256954d6da5cf21d0bbe7afb0a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/19/2020
ms.locfileid: "94941828"
---
# <a name="send-a-local-toast-notification-from-uwp-apps"></a>UWP アプリからローカルトースト通知を送信する


トースト通知は、ユーザーが現在アプリ内にいないときに、アプリが作成してユーザーに配信できるメッセージです。 このクイック スタートでは、新しいアダプティブ テンプレートと対話型の操作を使って Windows 10 のトースト通知を作成、配信、表示する手順について紹介します。 これらの操作をローカル通知を使って説明します。これは、最も簡単に実装できる通知です。

> [!IMPORTANT]
> デスクトップアプリケーション (パッケージ化された [Msix](/windows/msix/desktop/source-code-overview) アプリ、 [スパースパッケージ](/windows/apps/desktop/modernize/grant-identity-to-nonpackaged-apps) を使用してパッケージ id を取得するアプリ、および従来のパッケージ化されていないデスクトップアプリを含む) は、通知を送信したり、アクティブ化を処理したりするためのさまざまな手順があります。 トーストを実装する方法については、「[デスクトップ アプリ](toast-desktop-apps.md)」のドキュメントを参照してください。

> **重要な API**: [ToastNotification クラス](/uwp/api/Windows.UI.Notifications.ToastNotification)、[ToastNotificationActivatedEventArgs クラス](/uwp/api/Windows.ApplicationModel.Activation.ToastNotificationActivatedEventArgs)



## <a name="step-1-install-nuget-package"></a>手順 1: NuGet パッケージをインストールする

[Microsoft. Toolkit. 通知 NuGet パッケージ](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)をインストールします。 このサンプルコードでは、このパッケージを使用します。 この記事の最後には、NuGet パッケージを使用しない "plain" コードスニペットが用意されています。 このパッケージを使用すると、XML を使用せずにトースト通知を作成できます。


## <a name="step-2-add-namespace-declarations"></a>手順 2: 名前空間宣言を追加する

`Windows.UI.Notifications` トースト Api が含まれています。

```csharp
using Windows.UI.Notifications;
using Microsoft.Toolkit.Uwp.Notifications; // Notifications library
```


## <a name="step-3-send-a-toast"></a>手順 3: トーストを送信する

Windows 10 では、トースト通知のコンテンツは、通知の外観にすばらしい柔軟性を持たせることができるアダプティブ言語を使って表されます。 詳しくは、[トースト コンテンツのドキュメント](adaptive-interactive-toasts.md)をご覧ください。

単純なテキストベースの通知から始めます。 通知ライブラリを使用して通知コンテンツを作成し、通知を表示します。

<img alt="Simple text notification" src="images/send-toast-01.png" width="364"/>

```csharp
// Construct the content
var content = new ToastContentBuilder()
    .AddToastActivationInfo("picOfHappyCanyon", ToastActivationType.Foreground)
    .AddText("Andrew sent you a picture")
    .AddText("Check this out, Happy Canyon in Utah!")
    .GetToastContent();

// Create the notification
var notif = new ToastNotification(content.GetXml());

// And show it!
ToastNotificationManager.CreateToastNotifier().Show(notif);
```

## <a name="step-4-handling-activation"></a>手順 4: アクティブ化の処理

ユーザーが通知 (またはフォアグラウンドアクティベーション付きの通知のボタン) をクリックすると、アプリの **App.xaml.cs** **onactivated** が呼び出されます。

**App.xaml.cs**

```csharp
protected override void OnActivated(IActivatedEventArgs e)
{
    // Handle notification activation
    if (e is ToastNotificationActivatedEventArgs toastActivationArgs)
    {
        // Obtain the arguments from the notification
        string args = toastActivationArgs.Argument;

        // Obtain any user input (text boxes, menu selections) from the notification
        ValueSet userInput = toastActivationArgs.UserInput;
 
        // TODO: Show the corresponding content
    }
}
```

> [!IMPORTANT]
> **OnLaunched** コードと同様に、フレームを初期化してウィンドウをアクティブ化する必要があります。 **OnLaunched は、ユーザーがトーストをクリックしても呼び出されません**。アプリが閉じられてから初めて起動している場合も同様です。 通常は、**OnLaunched** と **OnActivated** を組み合わせて独自の `OnLaunchedOrActivated` メソッドにまとめることをお勧めします。これは、両方で同じ初期化を実行する必要があるためです。


## <a name="activation-in-depth"></a>アクティブ化の詳細

通知を実行可能にするための最初の手順は、通知に起動引数をいくつか追加することです。これにより、ユーザーが通知をクリックしたときに起動する内容をアプリが認識できるようになります (この例では、メッセージ交換を開く必要があり、どのようなメッセージ交換が開くかがわかります)。

次に示すように、 [QueryString.NET](https://www.nuget.org/packages/QueryString.NET/) NuGet パッケージをインストールして、通知引数のクエリ文字列の構築と解析を支援することをお勧めします。

```csharp
using Microsoft.QueryStringDotNET; // QueryString.NET

int conversationId = 384928;

// Construct the content
var content = new ToastContentBuilder()

    // Arguments returned when user taps body of notification
    .AddToastActivationInfo(new QueryString() // Using QueryString.NET
    {
        { "action", "viewConversation" },
        { "conversationId", conversationId.ToString() }
    }.ToString(), ToastActivationType.Foreground)

    .AddText("Andrew sent you a picture")
    ...
```


アクティブ化を処理するより複雑な例を次に示します。

**App.xaml.cs**

```csharp
protected override void OnActivated(IActivatedEventArgs e)
{
    // Get the root frame
    Frame rootFrame = Window.Current.Content as Frame;
 
    // TODO: Initialize root frame just like in OnLaunched
 
    // Handle toast activation
    if (e is ToastNotificationActivatedEventArgs toastActivationArgs)
    {            
        // Parse the query string (using QueryString.NET)
        QueryString args = QueryString.Parse(toastActivationArgs.Argument);
 
        // See what action is being requested 
        switch (args["action"])
        {
            // Open the image
            case "viewImage":
 
                // The URL retrieved from the toast args
                string imageUrl = args["imageUrl"];
 
                // If we're already viewing that image, do nothing
                if (rootFrame.Content is ImagePage && (rootFrame.Content as ImagePage).ImageUrl.Equals(imageUrl))
                    break;
 
                // Otherwise navigate to view it
                rootFrame.Navigate(typeof(ImagePage), imageUrl);
                break;
                             
 
            // Open the conversation
            case "viewConversation":
 
                // The conversation ID retrieved from the toast args
                int conversationId = int.Parse(args["conversationId"]);
 
                // If we're already viewing that conversation, do nothing
                if (rootFrame.Content is ConversationPage && (rootFrame.Content as ConversationPage).ConversationId == conversationId)
                    break;
 
                // Otherwise navigate to view it
                rootFrame.Navigate(typeof(ConversationPage), conversationId);
                break;
        }
 
        // If we're loading the app for the first time, place the main page on
        // the back stack so that user can go back after they've been
        // navigated to the specific page
        if (rootFrame.BackStack.Count == 0)
            rootFrame.BackStack.Add(new PageStackEntry(typeof(MainPage), null, null));
    }
 
    // TODO: Handle other types of activation
 
    // Ensure the current window is active
    Window.Current.Activate();
}
```


## <a name="adding-images"></a>追加 (イメージを)

豊富なコンテンツを通知に追加できます。 インラインイメージとプロファイル (アプリロゴのオーバーライド) イメージを追加します。

> [!NOTE]
> 画像は、アプリのパッケージ、アプリのローカル ストレージ、または Web から使用できます。 Fall Creators Update の時点で、Web 画像の上限は通常の接続で 3 MB、従量制課金接続で 1 MB です。 まだ Fall Creators Update を実行していないデバイスでは、Web イメージは 200 KB を上限とします。

> [!IMPORTANT]
> Http イメージは、マニフェストにインターネット機能を持つ UWP/MSIX/スパースアプリでのみサポートされています。 デスクトップの非 MSIX/スパースアプリでは、http イメージはサポートされません。ローカルアプリデータにイメージをダウンロードし、ローカルで参照する必要があります。

<img alt="Toast with images" src="images/send-toast-02.png" width="364"/>

```csharp
// Construct the content
var content = new ToastContentBuilder()
    ...

    // Inline image
    .AddInlineImage(new Uri("https://picsum.photos/360/202?image=883"))

    // Profile (app logo override) image
    .AddAppLogoOverride(new Uri("ms-appdata:///local/Andrew.jpg"), ToastGenericAppLogoCrop.Circle)
    
    .GetToastContent();
    
...
```



## <a name="adding-buttons-and-inputs"></a>ボタンと入力の追加

ボタンと入力を追加して、通知を対話形式にすることができます。 ボタンを使用すると、フォアグラウンドアプリ、プロトコル、またはバックグラウンドタスクを起動できます。 応答テキストボックス、"Like" ボタン、および [表示] ボタンを追加して、画像を開きます。

<img alt="Toast with images and buttons" src="images/send-toast-03.png" width="364"/>

```csharp
int conversationId = 384928;

// Construct the content
var content = new ToastContentBuilder()
    ...

    // Text box for replying
    .AddInputTextBox("tbReply", placeHolderContent: "Type a response")

    // Reference the text box's ID in order to place this button next to the text box
    .AddButton("tbReply", "Reply", ToastActivationType.Background, new QueryString()
    {
        { "action", "reply" },
        { "conversationId", conversationId.ToString() }
    }.ToString(), imageUri: new Uri("Assets/Reply.png", UriKind.Relative))

    .AddButton("Like", ToastActivationType.Background, new QueryString()
    {
        { "action", "like" },
        { "conversationId", conversationId.ToString() }
    }.ToString())

    .AddButton("View", ToastActivationType.Foreground, new QueryString()
    {
        { "action", "viewImage" },
        { "imageUrl", image.ToString() }
    }.ToString())
    
    .GetToastContent();
    
...
```

フォアグラウンドボタンのアクティブ化は、メインのトースト本文と同じ方法で処理されます (App.xaml.cs OnActivated が呼び出されます)。



## <a name="handling-background-activation"></a>バックグラウンドのアクティブ化を処理する

トースト (またはトースト内のボタンで) でバックグラウンドのアクティブ化を指定すると、フォアグラウンド アプリがアクティブ化されるのではなく、バックグラウンド タスクが実行されます。

バックグラウンド タスクについて詳しくは、「[バックグラウンド タスクによるアプリのサポート](../../../launch-resume/support-your-app-with-background-tasks.md)」をご覧ください。

ビルド 14393 以降をターゲットとしている場合、インプロセス バックグラウンド タスクを使用できるため、大幅に簡略化できます。 インプロセス バックグラウンド タスクは古いバージョンの Windows では実行できないことに注意してください。 次のコード サンプルでは、インプロセス バックグラウンド タスクを使います。

```csharp
const string taskName = "ToastBackgroundTask";

// If background task is already registered, do nothing
if (BackgroundTaskRegistration.AllTasks.Any(i => i.Value.Name.Equals(taskName)))
    return;

// Otherwise request access
BackgroundAccessStatus status = await BackgroundExecutionManager.RequestAccessAsync();

// Create the background task
BackgroundTaskBuilder builder = new BackgroundTaskBuilder()
{
    Name = taskName
};

// Assign the toast action trigger
builder.SetTrigger(new ToastNotificationActionTrigger());

// And register the task
BackgroundTaskRegistration registration = builder.Register();
```


次に、App.xaml.cs で、OnBackgroundActivated 化されたメソッドをオーバーライドします。 その後、事前に定義された引数とユーザー入力を取得できます。これは、フォアグラウンドアクティベーションと同様です。

**App.xaml.cs**

```csharp
protected override async void OnBackgroundActivated(BackgroundActivatedEventArgs args)
{
    var deferral = args.TaskInstance.GetDeferral();
 
    switch (args.TaskInstance.Task.Name)
    {
        case "ToastBackgroundTask":
            var details = args.TaskInstance.TriggerDetails as ToastNotificationActionTriggerDetail;
            if (details != null)
            {
                string arguments = details.Argument;
                var userInput = details.UserInput;

                // Perform tasks
            }
            break;
    }
 
    deferral.Complete();
}
```


## <a name="set-an-expiration-time"></a>有効期限を設定する

Windows 10 では、ユーザーが閉じるか無視したすべてのトースト通知はアクション センター に送られるため、ユーザーはポップアップが消えた後も通知を表示できます。

ただし、通知に含まれているメッセージが一定期間だけ関係する場合は、トースト通知に有効期限を設定して、アプリからユーザーに古い情報が表示されないようにする必要があります。 たとえば、12 時間限りのキャンペーンの場合は、有効期限を 12 時間に設定します。 以下のコードでは、有効期限が 2 日間に設定されています。

> [!NOTE]
> ローカル トースト通知の既定の最長有効期限は 3 日間です。

```csharp
// Create toast content
var content = new ToastContentBuilder()
    .AddText("Expires in 2 days...")
    .GetToastContent();

// Set expiration time
var notif = new ToastNotification(content.GetXml())
{
    ExpirationTime = DateTime.Now.AddDays(2)
};

// And show it!
ToastNotificationManager.CreateToastNotifier().Show(notif);
```


## <a name="provide-a-primary-key-for-your-toast"></a>トーストの主キーを提供する

送信した通知をプログラムで削除するか差し替える必要がある場合、Tag プロパティ (および必要に応じて Group プロパティ) を使って通知の主キーを提供する必要があります。 そうすると、今後この主キーを使って、通知の削除や差し替えができるようになります。

既に配信されたトースト通知の差し替えと削除の方法について詳しくは、「[クイック スタート: アクション センターでのトースト通知の管理 (XAML)](/previous-versions/windows/apps/dn631260(v=win.10))」をご覧ください。

Tag と Group を組み合わせると、復号主キーとして機能します。 Group はより汎用的な ID で、"wallPosts"、"messages"、"friendRequests" などのグループを割り当てることができます。Tag はグループ内から通知自体を一意に識別する必要があります。 汎用グループを使うことで、[RemoveGroup API](/uwp/api/Windows.UI.Notifications.ToastNotificationHistory#Windows_UI_Notifications_ToastNotificationHistory_RemoveGroup_System_String_) を使ってそのグループからすべての通知を削除できます。

```csharp
// Create toast content
var content = new ToastContentBuilder()
    .AddText("New post on your wall!")
    .GetToastContent();

// Set tag/group
new ToastNotification(content.GetXml())
{
    Tag = "18365",
    Group = "wallPosts"
};

// And show it!
ToastNotificationManager.CreateToastNotifier().Show(notif);
```



## <a name="clear-your-notifications"></a>通知を消去する

UWP アプリが独自の通知の削除と消去を行います。 アプリを起動したときに、通知が自動的に消去されることはありません。

Windows では、ユーザーが明示的に通知をクリックした場合のみ、通知を自動的に削除します。

メッセージング アプリの処理の例を以下に示します。

1. ユーザーが会話の中で新しいメッセージに関する複数のトーストを受け取る
2. ユーザーがそれらのトーストのいずれかをタップして会話を開く
3. アプリが会話を開き、その会話のすべてのトーストを消去する (その会話用にアプリが提供するグループで [RemoveGroup](/uwp/api/Windows.UI.Notifications.ToastNotificationHistory#Windows_UI_Notifications_ToastNotificationHistory_RemoveGroup_System_String_) を使う)
4. ユーザーのアクション センターが通知の状態を正しく反映して、その会話の古い通知がアクション センターに残らないように処理する

すべての通知を消去する方法または特定の通知を削除する方法については、「[クイック スタート: アクション センターでのトースト通知の管理 (XAML)](/previous-versions/windows/apps/dn631260(v=win.10))」をご覧ください。

```csharp
ToastNotificationManager.History.Clear();
```


## <a name="plain-code-snippets"></a>プレーンコードスニペット

NuGet の通知ライブラリを使用していない場合は、以下に示すように手動で XML を構築して [Toastnotification](/uwp/api/Windows.UI.Notifications.ToastNotification)を作成できます。

```csharp
using Windows.UI.Notifications;
using Windows.Data.Xml.Dom;

// In a real app, these would be initialized with actual data
string title = "Andrew sent you a picture";
string content = "Check this out, Happy Canyon in Utah!";
string image = "http://blogs.msdn.com/cfs-filesystemfile.ashx/__key/communityserver-blogs-components-weblogfiles/00-00-01-71-81-permanent/2727.happycanyon1_5B00_1_5D00_.jpg";
string logo = "ms-appdata:///local/Andrew.jpg";
 
// TODO: all values need to be XML escaped
 
// Construct the visuals of the toast
string toastVisual =
$@"<visual>
  <binding template='ToastGeneric'>
    <text>{title}</text>
    <text>{content}</text>
    <image src='{image}'/>
    <image src='{logo}' placement='appLogoOverride' hint-crop='circle'/>
  </binding>
</visual>";

// In a real app, these would be initialized with actual data
int conversationId = 384928;
 
// Generate the arguments we'll be passing in the toast
string argsReply = $"action=reply&conversationId={conversationId}";
string argsLike = $"action=like&conversationId={conversationId}";
string argsView = $"action=viewImage&imageUrl={Uri.EscapeDataString(image)}";
 
// TODO: all args need to be XML escaped
 
string toastActions =
$@"<actions>
 
  <input
      type='text'
      id='tbReply'
      placeHolderContent='Type a response'/>
 
  <action
      content='Reply'
      arguments='{argsReply}'
      activationType='background'
      imageUri='Assets/Reply.png'
      hint-inputId='tbReply'/>
 
  <action
      content='Like'
      arguments='{argsLike}'
      activationType='background'/>
 
  <action
      content='View'
      arguments='{argsView}'/>
 
</actions>";

// Now we can construct the final toast content
string argsLaunch = $"action=viewConversation&conversationId={conversationId}";
 
// TODO: all args need to be XML escaped
 
string toastXmlString =
$@"<toast launch='{argsLaunch}'>
    {toastVisual}
    {toastActions}
</toast>";
 
// Parse to XML
XmlDocument toastXml = new XmlDocument();
toastXml.LoadXml(toastXmlString);
 
// Generate toast
var toast = new ToastNotification(toastXml);
```


## <a name="resources"></a>リソース

* [GitHub での完全なコード サンプル](https://github.com/WindowsNotifications/quickstart-sending-local-toast-win10)
* [トースト コンテンツのドキュメント](adaptive-interactive-toasts.md)
* [ToastNotification クラス](/uwp/api/Windows.UI.Notifications.ToastNotification)
* [ToastNotificationActivatedEventArgs クラス](/uwp/api/Windows.ApplicationModel.Activation.ToastNotificationActivatedEventArgs)