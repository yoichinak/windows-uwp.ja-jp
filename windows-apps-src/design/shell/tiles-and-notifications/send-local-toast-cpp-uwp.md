---
description: C++ UWP アプリからローカルトースト通知を送信し、トーストをクリックしたユーザーを処理する方法について説明します。
title: C++ UWP アプリからローカルトースト通知を送信する
ms.assetid: E9AB7156-A29E-4ED7-B286-DA4A6E683638
label: Send a local toast notification from a C++ UWP app
template: detail.hbs
ms.date: 11/17/2020
ms.topic: article
keywords: windows 10、uwp、送信トースト通知、通知、送信通知、トースト通知、方法、クイックスタート、作業の開始、コードサンプル、チュートリアル、cpp、c++、uwp
ms.localizationpriority: medium
ms.openlocfilehash: 5960d6f2a63ea2aa0aea26392db5958825aebf2b
ms.sourcegitcommit: 6661f4d564d45ba10e5253864ac01e43b743c560
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/23/2021
ms.locfileid: "104804966"
---
# <a name="send-a-local-toast-notification-from-c-uwp-apps"></a>C++ UWP アプリからローカルトースト通知を送信する

[!INCLUDE [intro](includes/send-toast-intro.md)]

> [!IMPORTANT]
> C++ 以外の UWP アプリを作成している場合は、 [C++ WRL](send-local-toast-desktop-cpp-wrl.md) のドキュメントを参照してください。 C# アプリを作成する場合は、 [C# のドキュメント](send-local-toast.md)を参照してください。



## <a name="step-1-install-nuget-package"></a>手順 1: NuGet パッケージをインストールする

[!INCLUDE [nuget package](includes/nuget-package.md)]

このサンプルコードでは、このパッケージを使用します。 このパッケージを使用すると、XML を使用せずにトースト通知を作成できます。


## <a name="step-2-add-namespace-declarations"></a>手順 2: 名前空間宣言を追加する

```cpp
using namespace Microsoft::Toolkit::Uwp::Notifications;
```


## <a name="step-3-send-a-toast"></a>手順 3: トーストを送信する

[!INCLUDE [basic toast intro](includes/send-toast-basic-toast-intro.md)]

```cpp
// Construct the content and show the toast!
(ref new ToastContentBuilder())
    ->AddArgument("action", "viewConversation")
    ->AddArgument("conversationId", 9813)
    ->AddText("Andrew sent you a picture")
    ->AddText("Check this out, The Enchantments in Washington!")
    ->Show();
```


## <a name="step-4-handling-activation"></a>手順 4: アクティブ化の処理

ユーザーが通知 (またはフォアグラウンドアクティベーション付きの通知のボタン) をクリックすると、アプリの **App.xaml** **onactivated** が呼び出されます。

**App.xaml.cpp**

```cpp
void App::OnActivated(IActivatedEventArgs^ e)
{
    // Handle notification activation
    if (e->Kind == ActivationKind::ToastNotification)
    {
        ToastNotificationActivatedEventArgs^ toastActivationArgs = (ToastNotificationActivatedEventArgs^)e;

        // Obtain the arguments from the notification
        ToastArguments^ args = ToastArguments::Parse(toastActivationArgs->Argument);

        // Obtain any user input (text boxes, menu selections) from the notification
        auto userInput = toastActivationArgs->UserInput;
 
        // TODO: Show the corresponding content
    }
}
```

[!INCLUDE [OnLaunched warning](includes/onlaunched-warning.md)]


## <a name="activation-in-depth"></a>アクティブ化の詳細

通知を実行可能にするための最初の手順は、通知に起動引数をいくつか追加することです。これにより、ユーザーが通知をクリックしたときに起動する内容をアプリが認識できるようになります (この例では、メッセージ交換を開く必要があり、どのようなメッセージ交換が開くかがわかります)。

```cpp
// Construct the content and show the toast!
(ref new ToastContentBuilder())

    // Arguments returned when user taps body of notification
    ->AddArgument("action", "viewConversation")
    ->AddArgument("conversationId", 9813)

    ->AddText("Andrew sent you a picture")
    ->Show();
```



## <a name="adding-images"></a>追加 (イメージを)

豊富なコンテンツを通知に追加できます。 インラインイメージとプロファイル (アプリロゴのオーバーライド) イメージを追加します。

[!INCLUDE [images note](includes/images-note.md)]

<img alt="Toast with images" src="images/send-toast-02.png" width="364"/>

```cpp
// Construct the content and show the toast!
(ref new ToastContentBuilder())
    ...

    // Inline image
    ->AddInlineImage(ref new Uri("https://picsum.photos/360/202?image=883"))

    // Profile (app logo override) image
    ->AddAppLogoOverride(ref new Uri("ms-appdata:///local/Andrew.jpg"), ToastGenericAppLogoCrop::Circle)
    
    ->Show();
```



## <a name="adding-buttons-and-inputs"></a>ボタンと入力の追加

ボタンと入力を追加して、通知を対話形式にすることができます。 ボタンを使用すると、フォアグラウンドアプリ、プロトコル、またはバックグラウンドタスクを起動できます。 応答テキストボックス、"Like" ボタン、および [表示] ボタンを追加して、画像を開きます。

<img src="images/toast-notification.png" width="628" alt="Screenshot of a toast notification with inputs and buttons"/>

```cpp
// Construct the content
(ref new ToastContentBuilder())
    ->AddArgument("conversationId", 9813)
    ...

    // Text box for replying
    ->AddInputTextBox("tbReply", "Type a response")

    // Buttons
    ->AddButton((ref new ToastButton())
        ->SetContent("Reply")
        ->AddArgument("action", "reply")
        ->SetBackgroundActivation())

    ->AddButton((ref new ToastButton())
        ->SetContent("Like")
        ->AddArgument("action", "like")
        ->SetBackgroundActivation())

    ->AddButton((ref new ToastButton())
        ->SetContent("View")
        ->AddArgument("action", "view"))
    
    ->Show();
```

フォアグラウンドボタンのアクティブ化は、メインのトースト本文 (アプリの .xaml OnActivated が呼び出される) と同じ方法で処理されます。



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


次に、app.xaml で OnBackgroundActivated 化されたメソッドをオーバーライドします。 その後、事前に定義された引数とユーザー入力を取得できます。これは、フォアグラウンドアクティベーションと同様です。

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

```cpp
// Create toast content and show the toast!
(ref new ToastContentBuilder())
    ->AddText("Expires in 2 days...")
    ->Show(toast =>
    {
        toast->ExpirationTime = DateTime::Now->AddDays(2);
    });
```


## <a name="provide-a-primary-key-for-your-toast"></a>トーストの主キーを提供する

送信した通知をプログラムで削除するか差し替える必要がある場合、Tag プロパティ (および必要に応じて Group プロパティ) を使って通知の主キーを提供する必要があります。 そうすると、今後この主キーを使って、通知の削除や差し替えができるようになります。

既に配信されたトースト通知の差し替えと削除の方法について詳しくは、「[クイック スタート: アクション センターでのトースト通知の管理 (XAML)](/previous-versions/windows/apps/dn631260(v=win.10))」をご覧ください。

Tag と Group を組み合わせると、復号主キーとして機能します。 Group はより汎用的な ID で、"wallPosts"、"messages"、"friendRequests" などのグループを割り当てることができます。Tag はグループ内から通知自体を一意に識別する必要があります。 汎用グループを使うことで、[RemoveGroup API](/uwp/api/Windows.UI.Notifications.ToastNotificationHistory#Windows_UI_Notifications_ToastNotificationHistory_RemoveGroup_System_String_) を使ってそのグループからすべての通知を削除できます。

```cpp
// Create toast content and show the toast!
(ref new ToastContentBuilder())
    ->AddText("New post on your wall!")
    ->Show(toast =>
    {
        toast.Tag = "18365";
        toast.Group = "wallPosts";
    });
```



## <a name="clear-your-notifications"></a>通知を消去する

アプリは、独自の通知を削除してクリアする役割を担います。 アプリを起動したときに、通知が自動的に消去されることはありません。

Windows では、ユーザーが明示的に通知をクリックした場合のみ、通知を自動的に削除します。

メッセージング アプリの処理の例を以下に示します。

1. ユーザーが会話の中で新しいメッセージに関する複数のトーストを受け取る
2. ユーザーがそれらのトーストのいずれかをタップして会話を開く
3. アプリが会話を開き、その会話のすべてのトーストを消去する (その会話用にアプリが提供するグループで [RemoveGroup](/uwp/api/Windows.UI.Notifications.ToastNotificationHistory#Windows_UI_Notifications_ToastNotificationHistory_RemoveGroup_System_String_) を使う)
4. ユーザーのアクション センターが通知の状態を正しく反映して、その会話の古い通知がアクション センターに残らないように処理する

すべての通知を消去する方法または特定の通知を削除する方法については、「[クイック スタート: アクション センターでのトースト通知の管理 (XAML)](/previous-versions/windows/apps/dn631260(v=win.10))」をご覧ください。

```cpp
ToastNotificationManagerCompat::History->Clear();
```



## <a name="resources"></a>リソース

* [GitHub での完全なコード サンプル](https://github.com/WindowsNotifications/quickstart-sending-local-toast-win10)
* [トースト コンテンツのドキュメント](adaptive-interactive-toasts.md)
* [ToastNotification クラス](/uwp/api/Windows.UI.Notifications.ToastNotification)
* [ToastNotificationActivatedEventArgs クラス](/uwp/api/Windows.ApplicationModel.Activation.ToastNotificationActivatedEventArgs)