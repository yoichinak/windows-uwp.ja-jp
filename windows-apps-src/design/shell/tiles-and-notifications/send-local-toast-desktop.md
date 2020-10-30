---
description: デスクトップの C# アプリがローカルトースト通知を送信し、トーストをクリックしたユーザーを処理する方法について説明します。
title: デスクトップ C# アプリからのローカル トースト通知の送信
ms.assetid: E9AB7156-A29E-4ED7-B286-DA4A6E683638
label: Send a local toast notification from desktop C# apps
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10、win32、デスクトップ、トースト通知、トーストの送信、ローカルトーストの送信、デスクトップブリッジ、msix、スパースパッケージ、C#、C シャープ、トースト通知、wpf、送信トースト通知 wpf、送信トースト通知 winforms、送信トースト通知 c#、送信通知 wpf、送信通知 c#、トースト通知 wpf、トースト通知 C#
ms.localizationpriority: medium
ms.openlocfilehash: cb91a76db38623b533a925ea1df4728bc0fead78
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034475"
---
# <a name="send-a-local-toast-notification-from-desktop-c-apps"></a>デスクトップ C# アプリからのローカル トースト通知の送信

デスクトップアプリ (パッケージ化された [Msix](/windows/msix/desktop/source-code-overview) アプリ、 [スパースパッケージ](/windows/apps/desktop/modernize/grant-identity-to-nonpackaged-apps) を使用してパッケージ id を取得するアプリ、およびクラシック非パッケージデスクトップアプリを含む) は、Windows アプリと同様に対話型のトースト通知を送信できます。 ただし、さまざまなライセンス認証スキームと、MSIX またはスパースパッケージを使用していない場合、パッケージ id が存在しない可能性があるため、デスクトップアプリにはいくつかの特別な手順があります。

> [!IMPORTANT]
> UWP アプリを作成している場合は、[UWP のドキュメント](send-local-toast.md) をご覧ください。 その他のデスクトップ言語については、 [Win32 C++ WRL](send-local-toast-desktop-cpp-wrl.md)に関する記述を参照してください。


## <a name="step-1-install-the-notifications-library"></a>手順 1: 通知ライブラリをインストールする

`Microsoft.Toolkit.Uwp.Notifications` [NuGet パッケージ](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)をプロジェクトにインストールします。

この [通知ライブラリ](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) には、デスクトップアプリからのトースト通知を操作するための互換性ライブラリコードが追加されています。 また、UWP Sdk を参照し、生の XML ではなく、C# を使用して通知を作成することもできます。 このクイックスタートの残りの部分は、通知ライブラリによって異なります。


## <a name="step-2-implement-the-activator"></a>手順 2: アクティベーターを実装する

トーストをアクティブ化するためのハンドラーを実装する必要があります。これにより、ユーザーがトーストをクリックすると、アプリで何らかの処理を実行できるようになります。 これは、アクション センターにトーストを継続的に表示するために必要です (トーストは、数日後、アプリが閉じているときにクリックされる可能性があります)。 このクラスは、プロジェクトの任意の位置に指定できます。

新しい **Mynotificationactivator** クラスを作成し、 **notificationactivator** クラスを拡張します。 以下に示す3つの属性を追加し、多数のオンライン GUID ジェネレーターのいずれかを使用して、アプリの一意の GUID CLSID を作成します。 アクション センターは、この CLSID (クラス識別子) に基づいて、COM アクティブ化するクラスを認識します。

**MyNotificationActivator.cs** (このファイルを作成する)

```csharp
// The GUID CLSID must be unique to your app. Create a new GUID if copying this code.
[ClassInterface(ClassInterfaceType.None)]
[ComSourceInterfaces(typeof(INotificationActivationCallback))]
[Guid("replaced-with-your-guid-C173E6ADF0C3"), ComVisible(true)]
public class MyNotificationActivator : NotificationActivator
{
    public override void OnActivated(string invokedArgs, NotificationUserInput userInput, string appUserModelId)
    {
        // TODO: Handle activation
    }
}
```


## <a name="step-3-register-with-notification-platform"></a>手順 3: notification platform に登録する

次に、通知プラットフォームに登録します。 MSIX/スパースパッケージとクラシックデスクトップのどちらを使用しているかによって、手順が異なります。 両方をサポートする場合は、両方の手順を行う必要があります (コードをフォークする必要はありません。ライブラリがすべて自動的に処理します)。


#### <a name="msixsparse-packages"></a>[MSIX/スパースパッケージ](#tab/msix-sparse)

[Msix](/windows/msix/desktop/source-code-overview)または [スパースパッケージ](/windows/apps/desktop/modernize/grant-identity-to-nonpackaged-apps)を使用している場合 (または、両方をサポートしている場合) は、 **package.appxmanifest** に次のように追加します。

1. **xmlns:com** のための宣言
2. **xmlns:desktop** のための宣言
3. **IgnorableNamespaces** 属性に **com** と **desktop** を追加
4. **com:** 手順 #2 の GUID を使用して、com アクティベーターの拡張機能。 トーストから起動されたことがわかるように、必ず、`Arguments="-ToastActivated"` を指定します。
5. **desktop:** **Windows. toastNotificationActivation** の拡張機能を使用して、トーストアクティベーター CLSID (手順 #2 の GUID) を宣言します。

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

        <!--Register COM CLSID LocalServer32 registry key-->
        <com:Extension Category="windows.comServer">
          <com:ComServer>
            <com:ExeServer Executable="YourProject\YourProject.exe" Arguments="-ToastActivated" DisplayName="Toast activator">
              <com:Class Id="replaced-with-your-guid-C173E6ADF0C3" DisplayName="Toast activator"/>
            </com:ExeServer>
          </com:ComServer>
        </com:Extension>

        <!--Specify which CLSID to activate when toast clicked-->
        <desktop:Extension Category="windows.toastNotificationActivation">
          <desktop:ToastNotificationActivation ToastActivatorCLSID="replaced-with-your-guid-C173E6ADF0C3" /> 
        </desktop:Extension>

      </Extensions>
    </Application>
  </Applications>
 </Package>
```


#### <a name="unpackaged"></a>[パッケージ化](#tab/classic)

MSIX/スパース (または両方をサポートする場合) を使用していない場合は、スタート画面のアプリのショートカットで、アプリケーションユーザーモデル ID (AUMID) とトーストアクティベーター CLSID (手順 #2 からの GUID) を宣言する必要があります。

デスクトップアプリを識別する一意の AUMID を選択します。 これは通常、[CompanyName].[AppName] の形式です。すべてのアプリを通じて、一意である必要があります (任意の数字を自由に追加できます)。

### <a name="step-31-wix-installer"></a>手順 3.1: WiX インストーラー

インストーラーに WiX を使用している場合は、以下に示すように **Product.wxs** ファイルを編集して、スタート メニューのショートカットに 2 つのショートカット プロパティを追加します。 次に示すように、手順 #2 からの GUID がに囲まれていることを確認してください `{}` 。

**Product.wxs**

```xml
<Shortcut Id="ApplicationStartMenuShortcut" Name="Wix Sample" Description="Wix Sample" Target="[INSTALLFOLDER]WixSample.exe" WorkingDirectory="INSTALLFOLDER">
                    
    <!--AUMID-->
    <ShortcutProperty Key="System.AppUserModel.ID" Value="YourCompany.YourApp"/>
    
    <!--COM CLSID-->
    <ShortcutProperty Key="System.AppUserModel.ToastActivatorCLSID" Value="{replaced-with-your-guid-C173E6ADF0C3}"/>
    
</Shortcut>
```

> [!IMPORTANT]
> 実際に通知を使用するためには、通常のデバッグ前に、アプリをインストーラー経由でインストールして、AUMID と CLSID を使用したスタート ショートカットを表示する必要があります。 スタート ショートカットが表示された後は、Visual Studio で F5 キーを使用してデバッグできます。


### <a name="step-32-register-aumid-and-com-server"></a>手順 3.2: AUMID と COM サーバーを登録する

次に、アプリのスタートアップコード (通知 Api を呼び出す前) で、インストーラーに関係なく、 **RegisterAumidAndComServer** メソッドを呼び出して、手順 #2 で通知アクティベータークラスを指定し、上で使用した AUMID を指定します。

```csharp
// Register AUMID and COM server (for MSIX/sparse package apps, this no-ops)
DesktopNotificationManagerCompat.RegisterAumidAndComServer<MyNotificationActivator>("YourCompany.YourApp");
```

MSIX/スパースパッケージと従来のデスクトップの両方をサポートしている場合は、に関係なく、このメソッドを自由に呼び出すことができます。 MSIX/スパースパッケージでを実行している場合、このメソッドはすぐに制御を戻します。 コードをフォークする必要はありません。

このメソッドを使用することで、AUMID を常に提供する必要なしに、compat API を呼び出して通知を送信および管理できます。 またこのメソッドによって、COM サーバーの LocalServer32 レジストリ キーが挿入されます。

---


## <a name="step-4-register-com-activator"></a>手順 4: COM アクティベーターを登録する

MSIX/スパースパッケージと従来のデスクトップアプリの両方について、トーストのアクティベーションを処理できるように、通知アクティベーターの種類を登録する必要があります。

アプリのスタートアップコードで、次の **registeractivator** メソッドを呼び出し、手順 #2 で作成した **notificationactivator** クラスの実装を渡します。 これにより、トーストのアクティブ化を受信できるようになります。

```csharp
// Register COM server and activator type
DesktopNotificationManagerCompat.RegisterActivator<MyNotificationActivator>();
```


## <a name="step-5-send-a-notification"></a>手順 5: 通知を送信する

通知を送信する手順は、 **DesktopNotificationManagerCompat** クラスを使用して **ToastNotifier** を作成することを除き、UWP アプリとまったく同じです。 互換ライブラリでは、MSIX/スパースパッケージと従来のデスクトップの違いが自動的に処理されるため、コードをフォークする必要がありません。 従来のデスクトップでは、互換ライブラリによって、 **RegisterAumidAndComServer** を呼び出すときに指定した AUMID がキャッシュされます。これにより、AUMID を提供するタイミングや、提供するタイミングについて心配する必要はありません。

> [!NOTE]
> [Notifications ライブラリ](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) をインストールすると、以下に示すように、生の XML ではなく、C# を使って通知を作成できます。

レガシ Windows 8.1 トースト通知テンプレートでは、手順 #2 で作成した COM 通知アクティベーターがアクティブにならないため、以下に示す **Toastcontent** (または Toastcontent テンプレート) を使用してください。

> [!IMPORTANT]
> Http イメージは、マニフェストにインターネット機能を持つ MSIX/スパースパッケージアプリでのみサポートされています。 従来のデスクトップアプリは http イメージをサポートしていません。ローカルアプリデータにイメージをダウンロードし、ローカルで参照する必要があります。

```csharp
// Construct the visuals of the toast (using Notifications library)
ToastContent toastContent = new ToastContentBuilder()
    .AddToastActivationInfo("action=viewConversation&conversationId=5", ToastActivationType.Foreground)
    .AddText("Hello world!")
    .GetToastContent();

// And create the toast notification
var toast = new ToastNotification(toastContent.GetXml());

// And then show it
DesktopNotificationManagerCompat.CreateToastNotifier().Show(toast);
```

> [!IMPORTANT]
> 従来のデスクトップアプリでは、レガシトーストテンプレート (ToastText02 など) を使用できません。 COM CLSID を指定すると、レガシ テンプレートのアクティブ化は失敗します。 上記のように Windows 10 ToastGeneric テンプレートを使用する必要があります。


## <a name="step-6-handling-activation"></a>手順 6: アクティブ化の処理

ユーザーがトーストをクリックすると、 **NotificationActivator** クラスの **OnActivated** メソッドが呼び出されます。

OnActivated メソッド内では、トーストで指定した引数を解析し、ユーザーが入力または選択したユーザー入力を取得したうえで、それに応じてアプリをアクティブ化できます。

> [!NOTE]
> **OnActivated** メソッドは UI スレッドでは呼び出されません。 UI スレッド操作を実行する場合は、`Application.Current.Dispatcher.Invoke(callback)` を呼び出す必要があります。

```csharp
// The GUID must be unique to your app. Create a new GUID if copying this code.
[ClassInterface(ClassInterfaceType.None)]
[ComSourceInterfaces(typeof(INotificationActivationCallback))]
[Guid("replaced-with-your-guid-C173E6ADF0C3"), ComVisible(true)]
public class MyNotificationActivator : NotificationActivator
{
    public override void OnActivated(string invokedArgs, NotificationUserInput userInput, string appUserModelId)
    {
        Application.Current.Dispatcher.Invoke(delegate
        {
            // Tapping on the top-level header launches with empty args
            if (arguments.Length == 0)
            {
                // Perform a normal launch
                OpenWindowIfNeeded();
                return;
            }

            // Parse the query string (using NuGet package QueryString.NET)
            QueryString args = QueryString.Parse(invokedArgs);

            // See what action is being requested 
            switch (args["action"])
            {
                // Open the image
                case "viewImage":

                    // The URL retrieved from the toast args
                    string imageUrl = args["imageUrl"];

                    // Make sure we have a window open and in foreground
                    OpenWindowIfNeeded();

                    // And then show the image
                    (App.Current.Windows[0] as MainWindow).ShowImage(imageUrl);

                    break;

                // Background: Quick reply to the conversation
                case "reply":

                    // Get the response the user typed
                    string msg = userInput["tbReply"];

                    // And send this message
                    SendMessage(msg);

                    // If there's no windows open, exit the app
                    if (App.Current.Windows.Count == 0)
                    {
                        Application.Current.Shutdown();
                    }

                    break;
            }
        });
    }

    private void OpenWindowIfNeeded()
    {
        // Make sure we have a window open (in case user clicked toast while app closed)
        if (App.Current.Windows.Count == 0)
        {
            new MainWindow().Show();
        }

        // Activate the window, bringing it to focus
        App.Current.Windows[0].Activate();

        // And make sure to maximize the window too, in case it was currently minimized
        App.Current.Windows[0].WindowState = WindowState.Normal;
    }
}
```

アプリが閉じている間の起動を適切にサポートするため、`App.xaml.cs` ファイル内で **OnStartup** メソッド (WPF アプリ用) を上書きして、トーストから起動しているかどうかを判定することができます。 トーストから起動している場合は、起動引数が "-ToastActivated" に指定されています。 この引数が指定されている場合、通常の起動アクティブ化コードの実行をすべて停止して、 **OnActivated** コードによる起動処理が完了するのを待つ必要があります。

```csharp
protected override async void OnStartup(StartupEventArgs e)
{
    // Register AUMID, COM server, and activator
    DesktopNotificationManagerCompat.RegisterAumidAndComServer<MyNotificationActivator>("YourCompany.YourApp");
    DesktopNotificationManagerCompat.RegisterActivator<MyNotificationActivator>();

    // If launched from a toast
    if (e.Args.Contains("-ToastActivated"))
    {
        // Our NotificationActivator code will run after this completes,
        // and will show a window if necessary.
    }

    else
    {
        // Show the window
        // In App.xaml, be sure to remove the StartupUri so that a window doesn't
        // get created by default, since we're creating windows ourselves (and sometimes we
        // don't want to create a window if handling a background activation).
        new MainWindow().Show();
    }

    base.OnStartup(e);
}
```


### <a name="activation-sequence-of-events"></a>イベントのアクティブ化シーケンス

WPF の場合、アクティブ化シーケンスは次のとおりです。

アプリが既に実行されている場合:

1. **NotificationActivator** で **OnActivated** が呼び出されます。

アプリが実行されていない場合:

1. `App.xaml.cs` で、 **Args** に "-ToastActivated" を指定して **OnStartup** が呼び出されます。
2. **NotificationActivator** で **OnActivated** が呼び出されます。


### <a name="foreground-vs-background-activation"></a>フォアグラウンドとバックグラウンドのアクティブ化
デスクトップ アプリでは、フォア グラウンドとバック グラウンドのアクティブ化はいずれも、COM アクティベーターの呼び出しという同じ手順で処理されます。 ウィンドウを表示するか、ウィンドウを表示せずに作業を行うだけで終了するかは、アプリのコードによって決定されます。 そのため、トーストコンテンツに **背景** の **ActivationType** を指定しても、動作は変わりません。


## <a name="step-7-remove-and-manage-notifications"></a>手順 7: 通知を削除して管理する

通知を削除および管理する手順は、UWP アプリと同じです。 ただし、従来のデスクトップを使用している場合は、AUMID の提供について心配する必要がないように、互換性ライブラリを使用して **Desktopnotificationhistory compat** を取得することをお勧めします。

```csharp
// Remove the toast with tag "Message2"
DesktopNotificationManagerCompat.History.Remove("Message2");

// Clear all toasts
DesktopNotificationManagerCompat.History.Clear();
```


## <a name="step-8-deploying-and-debugging"></a>手順 8: 配置とデバッグ

MSIX アプリをデプロイしてデバッグする方法については、「 [パッケージ化されたデスクトップアプリの実行、デバッグ、およびテスト](/windows/msix/desktop/desktop-to-uwp-debug)」を参照してください。

クラシックデスクトップアプリをデプロイしてデバッグするには、通常どおりにデバッグする前に、インストーラーを使用してアプリをインストールする必要があります。これにより、AUMID と CLSID のスタートショートカットが存在するようになります。 スタート ショートカットが表示された後は、Visual Studio で F5 キーを使用してデバッグできます。

通知が従来のデスクトップアプリに表示されない (例外がスローされない) 場合は、最初のショートカットが表示されない (インストーラーを使用してアプリをインストールする) か、コードで使用した AUMID がスタートショートカットの AUMID と一致しないことが考えられます。

通知は表示されるが、アクション センターに表示されたままにならない (ポップアップを無視すると表示されなくなる) 場合は、COM アクティベーターが正しく実装されていません。

MSIX/スパースパッケージと従来のデスクトップアプリの両方をインストールした場合、トーストのアクティブ化を処理するときに、MSIX/スパースパッケージアプリは従来のデスクトップアプリよりも優先されることに注意してください。 つまり、クリックすると、クラシックデスクトップアプリからのトーストは、msix/スパースパッケージアプリを起動します。 MSIX/スパースパッケージアプリをアンインストールすると、ライセンス認証が従来のデスクトップアプリに戻されます。


## <a name="known-issues"></a>既知の問題

**修正済み: トーストのクリック後、アプリがフォーカスされない** : ビルド 15063 以前では、COM サーバーをアクティブ化したときに、フォアグラウンドの権利がアプリケーションに移転されませんでした。 そのため、アプリをフォアグラウンドに移動しようとしても、点滅するのみで移動できませんでした。 この問題を解決する方法はありませんでした。 この問題は、16299 以降のビルドでは解決済みです。


## <a name="resources"></a>リソース

* [GitHub での完全なコード サンプル](https://github.com/WindowsNotifications/desktop-toasts)
* [デスクトップアプリからのトースト通知](toast-desktop-apps.md)
* [トースト コンテンツのドキュメント](adaptive-interactive-toasts.md)
