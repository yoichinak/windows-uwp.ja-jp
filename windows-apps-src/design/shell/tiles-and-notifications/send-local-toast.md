---
description: C# アプリからローカルトースト通知を送信し、ユーザーがトーストをクリックする処理を行う方法について説明します。
title: C# アプリからローカルのトースト通知を送信する
ms.assetid: E9AB7156-A29E-4ED7-B286-DA4A6E683638
label: Send a local toast notification from C# apps
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10、uwp、トースト通知の送信、通知、送信通知、トースト通知、方法、クイックスタート、作業の開始、コードサンプル、チュートリアル、c#、csharp、win32、デスクトップ
ms.localizationpriority: medium
ms.openlocfilehash: 9e6130b703cc1f8a0163ea539ba97cf4a08388b5
ms.sourcegitcommit: 6661f4d564d45ba10e5253864ac01e43b743c560
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/23/2021
ms.locfileid: "104804359"
---
# <a name="send-a-local-toast-notification-from-c-apps"></a>C# アプリからローカルのトースト通知を送信する

[!INCLUDE [intro](includes/send-toast-intro.md)]

> [!IMPORTANT]
> C++ アプリを作成している場合は、 [C++ UWP](send-local-toast-cpp-uwp.md) または [c++ wrl](send-local-toast-desktop-cpp-wrl.md) のドキュメントを参照してください。


## <a name="step-1-install-nuget-package"></a>手順 1: NuGet パッケージをインストールする

[!INCLUDE [nuget package](includes/nuget-package.md)]

[!INCLUDE [nuget package .NET warnings](includes/nuget-package-dotnet-warnings.md)]

このサンプルコードでは、このパッケージを使用します。 このパッケージを使用すると、XML を使用せずにトースト通知を作成できます。また、デスクトップアプリでも、toasts を送信できます。


## <a name="step-2-send-a-toast"></a>手順 2: トーストを送信する

[!INCLUDE [basic toast intro](includes/send-toast-basic-toast-intro.md)]

```csharp
// Requires Microsoft.Toolkit.Uwp.Notifications NuGet package
new ToastContentBuilder()
    .AddArgument("action", "viewConversation")
    .AddArgument("conversationId", 9813)
    .AddText("Andrew sent you a picture")
    .AddText("Check this out, The Enchantments in Washington!")
    .Show();
```

このコードを実行すると、通知が表示されます。


## <a name="step-3-handling-activation"></a>手順 3: アクティブ化の処理

通知を表示した後、ユーザーが通知をクリックした場合、ユーザーがクリックしたときに特定のコンテンツを表示したり、アプリを一般に開いたり、ユーザーが通知をクリックしたときにアクションを実行したりすることが必要になる場合があります。

UWP、デスクトップ (MSIX)、およびデスクトップ (パッケージ化されていない) アプリでは、アクティベーションを処理する手順は異なります。


#### <a name="uwp"></a>[UWP](#tab/uwp)

ユーザーが通知 (またはフォアグラウンドアクティベーション付きの通知のボタン) をクリックすると、アプリの **app.xaml** の **onactivated** が呼び出され、追加した引数が返されます。

**App.xaml.cs**

```csharp
protected override void OnActivated(IActivatedEventArgs e)
{
    // Handle notification activation
    if (e is ToastNotificationActivatedEventArgs toastActivationArgs)
    {
        // Obtain the arguments from the notification
        ToastArguments args = ToastArguments.Parse(toastActivationArgs.Argument);

        // Obtain any user input (text boxes, menu selections) from the notification
        ValueSet userInput = toastActivationArgs.UserInput;
 
        // TODO: Show the corresponding content
    }
}
```

[!INCLUDE [OnLaunched warning](includes/onlaunched-warning.md)]

#### <a name="desktop-msix"></a>[デスクトップ (MSIX)](#tab/desktop-msix)

まず、 **package.appxmanifest** に次のように追加します。

1. **xmlns:com** のための宣言
1. **xmlns:desktop** のための宣言
1. **IgnorableNamespaces** 属性に **com** と **desktop** を追加
1. **desktop:** **Windows. toastNotificationActivation** の拡張機能を使用して、トーストアクティベーター CLSID を宣言します (任意の新しい GUID を使用します)。
1. MSIX のみ: com アクティベーターの com **: 拡張機能** は、手順 #4 の GUID を使用します。 発表が通知からのものであることを知らせるため、必ずを含めてください。 `Arguments="-ToastActivated"`

**Package.appxmanifest**

```xml
<!--Add these namespaces-->
<Package
  ...
  xmlns:com="http://schemas.microsoft.com/appx/manifest/com/windows10"
  xmlns:desktop="http://schemas.microsoft.com/appx/manifest/desktop/windows10"
  IgnorableNamespaces="... com desktop">
  ...
  <Applications>
    <Application>
      ...
      <Extensions>

        <!--Specify which CLSID to activate when toast clicked-->
        <desktop:Extension Category="windows.toastNotificationActivation">
          <desktop:ToastNotificationActivation ToastActivatorCLSID="replaced-with-your-guid-C173E6ADF0C3" /> 
        </desktop:Extension>

        <!--Register COM CLSID LocalServer32 registry key-->
        <com:Extension Category="windows.comServer">
          <com:ComServer>
            <com:ExeServer Executable="YourProject\YourProject.exe" Arguments="-ToastActivated" DisplayName="Toast activator">
              <com:Class Id="replaced-with-your-guid-C173E6ADF0C3" DisplayName="Toast activator"/>
            </com:ExeServer>
          </com:ComServer>
        </com:Extension>

      </Extensions>
    </Application>
  </Applications>
 </Package>
```

次に、 **アプリのスタートアップコード** (WPF 用の App.xaml onstartup) で、onstartup 化イベントをサブスクライブします。

[!INCLUDE [desktop toast activation sequence](includes/desktop-toast-activation-code.md)]


[!INCLUDE [desktop toast activation sequence](includes/desktop-toast-activation-sequence.md)]


#### <a name="desktop-unpackaged"></a>[デスクトップ (パッケージからの)](#tab/desktop)

[!INCLUDE [desktop toast activation sequence](includes/desktop-toast-activation-sequence.md)]

**アプリのスタートアップコード** (WPF 用の App.xaml onstartup) で、onstartup 化イベントをサブスクライブします。

[!INCLUDE [desktop toast activation sequence](includes/desktop-toast-activation-code.md)]

---


## <a name="step-4-handling-uninstallation"></a>手順 4: アンインストールの処理

#### <a name="uwp"></a>[UWP](#tab/uwp)

何もする必要はありません。 UWP アプリがアンインストールされると、すべての通知とその他の関連リソースが自動的にクリーンアップされます。

#### <a name="desktop-msix"></a>[デスクトップ (MSIX)](#tab/desktop-msix)

何もする必要はありません。 MSIX アプリがアンインストールされると、すべての通知とその他の関連リソースが自動的にクリーンアップされます。

#### <a name="desktop-unpackaged"></a>[デスクトップ (パッケージからの)](#tab/desktop)

アプリにアンインストーラーがある場合は、アンインストーラーでを呼び出す必要があり `ToastNotificationManagerCompat.Uninstall();` ます。 アプリがインストーラーを使用しない "ポータブルアプリ" である場合は、アプリの終了後もこのメソッドを呼び出すことを検討してください。

アンインストール方法では、スケジュールされた通知と現在の通知をクリーンアップし、関連するレジストリ値を削除して、ライブラリによって作成された関連する一時ファイルを削除します。

---


## <a name="adding-images"></a>追加 (イメージを)

豊富なコンテンツを通知に追加できます。 インラインイメージとプロファイル (アプリロゴのオーバーライド) イメージを追加します。

[!INCLUDE [images note](includes/images-note.md)]

> [!IMPORTANT]
> Http イメージは、マニフェストにインターネット機能を持つ UWP/MSIX/スパースアプリでのみサポートされています。 デスクトップの非 MSIX/スパースアプリでは、http イメージはサポートされません。ローカルアプリデータにイメージをダウンロードし、ローカルで参照する必要があります。

<img alt="Toast with images" src="images/send-toast-02.png" width="364"/>

```csharp
// Construct the content and show the toast!
new ToastContentBuilder()
    ...

    // Inline image
    .AddInlineImage(new Uri("https://picsum.photos/360/202?image=883"))

    // Profile (app logo override) image
    .AddAppLogoOverride(new Uri("ms-appdata:///local/Andrew.jpg"), ToastGenericAppLogoCrop.Circle)
    
    .Show();
```



## <a name="adding-buttons-and-inputs"></a>ボタンと入力の追加

ボタンと入力を追加して、通知を対話形式にすることができます。 ボタンを使用すると、フォアグラウンドアプリ、プロトコル、またはバックグラウンドタスクを起動できます。 応答テキストボックス、"Like" ボタン、および [表示] ボタンを追加して、画像を開きます。

<img src="images/toast-notification.png" width="628" alt="Screenshot of a toast notification with inputs and buttons"/>

```csharp
int conversationId = 384928;

// Construct the content
new ToastContentBuilder()
    .AddArgument("conversationId", conversationId)
    ...

    // Text box for replying
    .AddInputTextBox("tbReply", placeHolderContent: "Type a response")

    // Buttons
    .AddButton(new ToastButton()
        .SetContent("Reply")
        .AddArgument("action", "reply")
        .SetBackgroundActivation())

    .AddButton(new ToastButton()
        .SetContent("Like")
        .AddArgument("action", "like")
        .SetBackgroundActivation())

    .AddButton(new ToastButton()
        .SetContent("View")
        .AddArgument("action", "viewImage")
        .AddArgument("imageUrl", image.ToString()))
    
    .Show();
```

前面のボタンのアクティブ化は、メインのトーストの本文と同じ方法で処理されます (app.xaml の OnActivated が呼び出されます)。

上に示したように、ボタンが AddArgument API を使用している限り、トップレベルのトースト (メッセージ交換 ID など) に追加された引数も返されます (ボタンで引数を代入する場合、最上位レベルの引数は含まれません)。



## <a name="handling-background-activation"></a>バックグラウンドのアクティブ化を処理する

#### <a name="uwp"></a>[UWP](#tab/uwp)

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
                ToastArguments arguments = ToastArguments.Parse(details.Argument);
                var userInput = details.UserInput;

                // Perform tasks
            }
            break;
    }
 
    deferral.Complete();
}
```

#### <a name="desktop"></a>[デスクトップ](#tab/desktop-msix+desktop)

デスクトップアプリケーションの場合、バックグラウンドでのアクティブ化はフォアグラウンドアクティベーションと同じように処理されます ( **onactivated 化** されたイベントハンドラーがトリガーされます)。 アクティブ化を処理した後、UI を表示せずにアプリを閉じることを選択できます。

---


## <a name="set-an-expiration-time"></a>有効期限を設定する

Windows 10 では、ユーザーが閉じるか無視したすべてのトースト通知はアクション センター に送られるため、ユーザーはポップアップが消えた後も通知を表示できます。

ただし、通知に含まれているメッセージが一定期間だけ関係する場合は、トースト通知に有効期限を設定して、アプリからユーザーに古い情報が表示されないようにする必要があります。 たとえば、12 時間限りのキャンペーンの場合は、有効期限を 12 時間に設定します。 以下のコードでは、有効期限が 2 日間に設定されています。

> [!NOTE]
> ローカル トースト通知の既定の最長有効期限は 3 日間です。

```csharp
// Create toast content and show the toast!
new ToastContentBuilder()
    .AddText("Expires in 2 days...")
    .Show(toast =>
    {
        toast.ExpirationTime = DateTime.Now.AddDays(2);
    });
```


## <a name="provide-a-primary-key-for-your-toast"></a>トーストの主キーを提供する

送信した通知をプログラムで削除するか差し替える必要がある場合、Tag プロパティ (および必要に応じて Group プロパティ) を使って通知の主キーを提供する必要があります。 そうすると、今後この主キーを使って、通知の削除や差し替えができるようになります。

既に配信されたトースト通知の差し替えと削除の方法について詳しくは、「[クイック スタート: アクション センターでのトースト通知の管理 (XAML)](/previous-versions/windows/apps/dn631260(v=win.10))」をご覧ください。

Tag と Group を組み合わせると、復号主キーとして機能します。 Group はより汎用的な ID で、"wallPosts"、"messages"、"friendRequests" などのグループを割り当てることができます。Tag はグループ内から通知自体を一意に識別する必要があります。 汎用グループを使うことで、[RemoveGroup API](/uwp/api/Windows.UI.Notifications.ToastNotificationHistory#Windows_UI_Notifications_ToastNotificationHistory_RemoveGroup_System_String_) を使ってそのグループからすべての通知を削除できます。

```csharp
// Create toast content and show the toast!
new ToastContentBuilder()
    .AddText("New post on your wall!")
    .Show(toast =>
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

```csharp
ToastNotificationManagerCompat.History.Clear();
```



## <a name="resources"></a>リソース

* [GitHub での完全なコード サンプル](https://github.com/WindowsNotifications/quickstart-sending-local-toast-win10)
* [トースト コンテンツのドキュメント](adaptive-interactive-toasts.md)
* [ToastNotification クラス](/uwp/api/Windows.UI.Notifications.ToastNotification)
* [ToastNotificationActivatedEventArgs クラス](/uwp/api/Windows.ApplicationModel.Activation.ToastNotificationActivatedEventArgs)