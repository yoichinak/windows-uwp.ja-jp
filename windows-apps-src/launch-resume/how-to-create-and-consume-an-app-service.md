---
title: アプリ サービスの作成と利用
description: 他のユニバーサル Windows プラットフォーム (UWP) アプリにサービスを提供できる UWP アプリを作成する方法と、それらのサービスを利用する方法について説明します。
ms.assetid: 6E48B8B6-D3BF-4AE2-85FB-D463C448C9D3
keywords: アプリ間通信, プロセス間通信, IPC, バックグラウンドメッセージング, バックグラウンド通信, アプリからアプリ, app service
ms.date: 01/16/2019
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: bd67c8a94ae7fc46956f5a6f7c3358e0e28965d2
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89158826"
---
# <a name="create-and-consume-an-app-service"></a>アプリ サービスの作成と利用

アプリ サービスは、他の UWP アプリにサービスを提供する UWP アプリです。 これは、デバイス上にある Web サービスのようなものです。 アプリ サービスは、バック グラウンド タスクとしてホスト アプリで実行され、そのサービスを他のアプリに提供することができます。 たとえば、アプリ サービスによって、他のアプリで使用できるバー コード スキャナー サービスが提供される場合があります。 また、アプリのエンタープライズ スイートに共通のスペル チェック アプリ サービスを備えておき、そのサービスを同じスイート内の他のアプリから利用可能にする場合もあるでしょう。  アプリ サービスでは、同じデバイス上のアプリから呼び出せる UI を持たないサービスを作成できます。また、Windows 10 バージョン 1607 以降では、リモート デバイスからも呼び出せます。

Windows 10 バージョン 1607 以降では、ホスト アプリと同じプロセスで実行されるアプリ サービスを作成できます。 この記事では、別のバックグラウンド プロセスで実行されるアプリ サービスの作成と利用に重点を置いて説明します。 プロバイダーと同じプロセスでアプリ サービスを実行する詳細については、「[ホスト アプリと同じプロセスで実行するようにアプリ サービスを変換する](convert-app-service-in-process.md)」をご覧ください。

アプリ サービスのコード サンプルについては、「[ユニバーサル Windows プラットフォーム (UWP) アプリのサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AppServices)」を参照してください。

## <a name="create-a-new-app-service-provider-project"></a>新しいアプリ サービス プロバイダー プロジェクトの作成

このトピックでは、わかりやすくするためにすべてを 1 つのソリューションで作成します。

1. Visual Studio 2015 以降で、新しい UWP アプリプロジェクトを作成し、その名前を「 **Appserviceprovider**」にします。
    1. **ファイル > 新しい > プロジェクトの**選択... 
    2. [ **新しいプロジェクトの作成** ] ダイアログボックスで、[ **空のアプリ (ユニバーサル Windows)] C#** を選択します。 これは、他の UWP アプリがアプリ サービスを利用できるアプリとなります。
    3. [ **次へ**] をクリックし、プロジェクトに「 **appserviceprovider**」という名前を指定して、場所を選択し、[ **作成**] をクリックします。

2. プロジェクトの **ターゲット** と **最小バージョン** を選択するよう求められたら、少なくとも **10.0.14393**を選択します。 新しい **supports複数インスタンス** の属性を使用する場合は、visual studio 2017 または visual studio 2019 を使用し、target **10.0.15063** (**Windows 10**の作成者の更新) 以降を使用する必要があります。

<span id="appxmanifest"/>

## <a name="add-an-app-service-extension-to-packageappxmanifest"></a>Package.appxmanifest に app service 拡張機能を追加する

**Appserviceprovider**プロジェクトで、 **package.appxmanifest**ファイルをテキストエディターで開きます。 

1. **ソリューションエクスプローラー**で右クリックします。 
2. [ **ファイルを開くアプリケーションの**選択] を選びます。 
3. **XML (テキスト) エディター**を選択します。 

`AppService`要素内に次の拡張機能を追加し `<Application>` ます。 この例では、`com.microsoft.inventory` サービスをアドバタイズし、このアプリをアプリ サービス プロバイダーとして識別します。 実際のサービスは、バックグラウンド タスクとして実装されます。 アプリ サービスのプロジェクトは、サービスを他のアプリに公開します。 サービス名には逆のドメイン名スタイルを使うことをお勧めします。

`xmlns:uap4` 名前空間のプレフィックスと `uap4:SupportsMultipleInstances` 属性は、Windows SDK バージョン 10.0.15063 以降をターゲットとしている場合のみ有効です。 以前のバージョンの SDK をターゲットとしている場合には、それらは安全に削除できます。

``` xml
<Package
    ...
    xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
    xmlns:uap4="http://schemas.microsoft.com/appx/manifest/uap/windows10/4"
    ...
    <Applications>
        <Application Id="AppServiceProvider.App"
          Executable="$targetnametoken$.exe"
          EntryPoint="AppServiceProvider.App">
          ...
          <Extensions>
            <uap:Extension Category="windows.appService" EntryPoint="MyAppService.Inventory">
              <uap3:AppService Name="com.microsoft.inventory" uap4:SupportsMultipleInstances="true"/>
            </uap:Extension>
          </Extensions>
          ...
        </Application>
    </Applications>
```

属性は、 `Category` このアプリケーションを app service プロバイダーとして識別します。

属性は、 `EntryPoint` 次に実装するサービスを実装する名前空間修飾クラスを識別します。

属性は、 `SupportsMultipleInstances` app service が呼び出されるたびに、新しいプロセスで実行する必要があることを示します。 この機能は必須ではありませんが、10.0.15063 SDK (Windows 10 の作成者の**更新プログラム**) 以降を対象としている場合に使用できます。 また、先頭に `uap4` 名前空間を付ける必要があります。

## <a name="create-the-app-service"></a>アプリ サービスの作成

1.  アプリ サービスは、バックグラウンド タスクとして実装できます。 これにより、フォアグラウンド アプリケーションは、別のアプリケーションのアプリ サービスを呼び出すことができます。 バックグラウンドタスクとして app service を作成するには、 **MyAppService**という名前の新しい Windows ランタイムコンポーネントプロジェクトをソリューションに追加します ([ファイル] [** &gt; &gt; 新しいプロジェクトの追加**])。 [ **新しいプロジェクトの追加** ] ダイアログボックスで、[ **インストール済み > Visual C# > Windows ランタイムコンポーネント (ユニバーサル Windows)**] を選択します。
2.  **Appserviceprovider**プロジェクトで、新しい**MyAppService**プロジェクトにプロジェクト間参照を追加します (**ソリューションエクスプローラー**で、 **appserviceprovider**プロジェクトを右クリックし > [参照プロジェクトの**追加**]  >  **Reference**  >  **Projects**  >  **ソリューション**を選択し、[ **MyAppService**  >  **OK]** を選択します)。 参照を追加しない場合でも、アプリ サービスは実行時に接続されないため、この手順は重要です。
3.  **MyAppService**プロジェクトで、 **Class1.cs**の先頭に次の**using**ステートメントを追加します。
    ```cs
    using Windows.ApplicationModel.AppService;
    using Windows.ApplicationModel.Background;
    using Windows.Foundation.Collections;
    ```

4.  **Class1.cs**の名前を**Inventory.cs**に変更し、 **Class1**のスタブコードを**Inventory**という名前の新しい background task クラスに置き換えます。

    ```cs
    public sealed class Inventory : IBackgroundTask
    {
        private BackgroundTaskDeferral backgroundTaskDeferral;
        private AppServiceConnection appServiceconnection;
        private String[] inventoryItems = new string[] { "Robot vacuum", "Chair" };
        private double[] inventoryPrices = new double[] { 129.99, 88.99 };

        public void Run(IBackgroundTaskInstance taskInstance)
        {
            // Get a deferral so that the service isn't terminated.
            this.backgroundTaskDeferral = taskInstance.GetDeferral();

            // Associate a cancellation handler with the background task.
            taskInstance.Canceled += OnTaskCanceled;

            // Retrieve the app service connection and set up a listener for incoming app service requests.
            var details = taskInstance.TriggerDetails as AppServiceTriggerDetails;
            appServiceconnection = details.AppServiceConnection;
            appServiceconnection.RequestReceived += OnRequestReceived;
        }

        private async void OnRequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
        {
            // This function is called when the app service receives a request.
        }

        private void OnTaskCanceled(IBackgroundTaskInstance sender, BackgroundTaskCancellationReason reason)
        {
            if (this.backgroundTaskDeferral != null)
            {
                // Complete the service deferral.
                this.backgroundTaskDeferral.Complete();
            }
        }
    }
    ```

    このクラスは、アプリ サービスが作業を実行する場所です。

    **Run** は、バックグラウンドタスクが作成されるときに呼び出されます。 バックグラウンド タスクは **Run** が完了すると終了するため、バックグラウンド タスクが要求に引き続き対応できるように、コードは保留されます。 バックグラウンド タスクとして実装されたアプリ サービスは、呼び出しを受け取った後、約 30 秒間に再度呼び出されない限り、または保留されない限り、約 30 秒間有効になっています。アプリ サービスが同じプロセスで呼び出し元として実装されると、アプリ サービスの有効期間は呼び出し元の有効期間に関連付けられます。

    アプリ サービスの有効期間は、呼び出し元に依存します。

    * 呼び出し元がフォアグラウンドの場合、app service の有効期間は呼び出し元と同じになります。
    * 呼び出し元がバックグラウンドの場合、app service は実行に30秒かかります。 保留されると、1 回 5 秒追加されます。

    **Ontaskcanceled** は、タスクが取り消されたときに呼び出されます。 クライアントアプリが [AppServiceConnection](/uwp/api/Windows.ApplicationModel.AppService.AppServiceConnection)を破棄するか、クライアントアプリが中断されるか、os がシャットダウンまたはスリープ状態になるか、os がタスクを実行するためにリソース不足になると、タスクは取り消されます。

## <a name="write-the-code-for-the-app-service"></a>アプリ サービスのコードを記述する

**Onrequestreceived** は、app service のコードが存在する場所です。 **MyAppService**の**Inventory.cs**で**受信したスタブ onrequestreceived** 、この例のコードで置き換えます。 このコードは、インベントリの項目のインデックスを取得し、コマンド文字列と共にサービスに渡して、指定したインベントリ項目の名前と価格を取得します。 独自のプロジェクトには、エラー処理コードを追加します。

```cs
private async void OnRequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
{
    // Get a deferral because we use an awaitable API below to respond to the message
    // and we don't want this call to get canceled while we are waiting.
    var messageDeferral = args.GetDeferral();

    ValueSet message = args.Request.Message;
    ValueSet returnData = new ValueSet();

    string command = message["Command"] as string;
    int? inventoryIndex = message["ID"] as int?;

    if (inventoryIndex.HasValue &&
        inventoryIndex.Value >= 0 &&
        inventoryIndex.Value < inventoryItems.GetLength(0))
    {
        switch (command)
        {
            case "Price":
            {
                returnData.Add("Result", inventoryPrices[inventoryIndex.Value]);
                returnData.Add("Status", "OK");
                break;
            }

            case "Item":
            {
                returnData.Add("Result", inventoryItems[inventoryIndex.Value]);
                returnData.Add("Status", "OK");
                break;
            }

            default:
            {
                returnData.Add("Status", "Fail: unknown command");
                break;
            }
        }
    }
    else
    {
        returnData.Add("Status", "Fail: Index out of range");
    }

    try
    {
        // Return the data to the caller.
        await args.Request.SendResponseAsync(returnData);
    }
    catch (Exception e)
    {
        // Your exception handling code here.
    }
    finally
    {
        // Complete the deferral so that the platform knows that we're done responding to the app service call.
        // Note for error handling: this must be called even if SendResponseAsync() throws an exception.
        messageDeferral.Complete();
    }
}
```

この例では[Sendresponseasync](/uwp/api/windows.applicationmodel.appservice.appservicerequest.sendresponseasync)に対して待機可能メソッド呼び出しを行うため、 **onrequestreceived**は**async**であることに注意してください。

サービスが**Onrequestreceived**ハンドラーで**非同期**メソッドを使用できるように、遅延が発生します。 これにより、メッセージの処理が完了するまで、 **Onrequestreceived** の呼び出しが完了しません。  [SendResponseAsync](/uwp/api/windows.applicationmodel.appservice.appservicerequest.sendresponseasync) が結果を呼び出し元に送ります。 **SendResponseAsync** は、呼び出しの完了時に通知しません。 **OnRequestReceived** が完了したことを [SendMessageAsync](/uwp/api/windows.applicationmodel.appservice.appserviceconnection.sendmessageasync) に通知するのは、保留の完了時です。 Sendresponseasync の呼び出しは try/finally ブロックで **ラップされ** ます。これは、 **sendresponseasync** が例外をスローした場合でも遅延を完了する必要があるためです。

App services は、 [Valueset](/uwp/api/Windows.Foundation.Collections.ValueSet) オブジェクトを使用して情報を交換します。 渡すことができるデータのサイズは、システム リソースによってのみ制限されます。 **ValueSet** で使うことができる定義済みのキーはありません。 アプリ サービスのプロトコルの定義に使うキーの値を決定する必要があります。 呼び出し元は、そのプロトコルを念頭に置いて記述する必要があります。 この例では、アプリ サービスがインベントリ項目またはその価格の名前を提供するかどうかを示す値を持つ、`Command` という名前のキーを選びました。 インベントリ名のインデックスは、`ID` キーに保存されています。 戻り値は `Result` キーに保存されます。

[AppServiceClosedStatus](/uwp/api/Windows.ApplicationModel.AppService.AppServiceClosedStatus) 列挙体が呼び出し元に返され、アプリ サービスの呼び出した成功したか失敗したかを示します。 アプリ サービスへの呼び出しが失敗する例として、OS がサービスのエンドポイントを中止した、リソースが超過したなどがあります。 [ValueSet](/uwp/api/Windows.Foundation.Collections.ValueSet) を通じて、さらにエラー情報を取得することができます。 この例では、`Status` という名前のキーを使って、より詳細なエラー情報を呼び出し元に返します。

[SendResponseAsync](/uwp/api/windows.applicationmodel.appservice.appservicerequest.sendresponseasync) の呼び出しからは、[ValueSet](/uwp/api/Windows.Foundation.Collections.ValueSet) が呼び出し元に返されます。

## <a name="deploy-the-service-app-and-get-the-package-family-name"></a>サービス アプリを展開して、パッケージ ファミリ名を取得する

App service プロバイダーは、クライアントから呼び出す前に展開する必要があります。 これをデプロイするには、Visual Studio で [ **ビルド > 配置** ] を選択します。

また、それを呼び出すためには、app service プロバイダーのパッケージファミリ名も必要です。 これを取得するには、デザイナービューで **Appserviceprovider** プロジェクトの **package.appxmanifest** ファイルを開きます ( **ソリューションエクスプローラー**でダブルクリックします)。 [ **パッケージング** ] タブを選択し、[ **パッケージファミリ名**] の横の値をコピーして、メモ帳などの場所に貼り付けます。

## <a name="write-a-client-to-call-the-app-service"></a>アプリ サービスを呼び出すクライアントを作成する

1.  **[ファイル] &gt; [追加] &gt; [新しいプロジェクト]** で、新しい空の Windows ユニバーサル アプリ プロジェクトをソリューションに追加します。 [ **新しいプロジェクトの追加** ] ダイアログボックスで、[ **インストール済み > VISUAL C#] > [空のアプリ (ユニバーサル Windows)** ] の順に選択し、 **ClientApp**という名前を指定します。

2.  **ClientApp**プロジェクトで、 **MainPage.xaml.cs**の先頭に次の**using**ステートメントを追加します。
    ```cs
    using Windows.ApplicationModel.AppService;
    ```

3.  **TextBox**という名前のテキストボックスと、 **mainpage.xaml**にボタンを追加します。

4.  **Button_Click**というボタンのボタンクリックハンドラーを追加し、ボタンハンドラーのシグネチャに**async**キーワードを追加します。

5. ボタンのクリック ハンドラーのスタブを、次のコードで置き換えます。 必ず、`inventoryService` フィールドの宣言を含めます。
    ```cs
   private AppServiceConnection inventoryService;

   private async void button_Click(object sender, RoutedEventArgs e)
   {
       // Add the connection.
       if (this.inventoryService == null)
       {
           this.inventoryService = new AppServiceConnection();

           // Here, we use the app service name defined in the app service 
           // provider's Package.appxmanifest file in the <Extension> section.
           this.inventoryService.AppServiceName = "com.microsoft.inventory";

           // Use Windows.ApplicationModel.Package.Current.Id.FamilyName 
           // within the app service provider to get this value.
           this.inventoryService.PackageFamilyName = "Replace with the package family name";

           var status = await this.inventoryService.OpenAsync();

           if (status != AppServiceConnectionStatus.Success)
           {
               textBox.Text= "Failed to connect";
               this.inventoryService = null;
               return;
           }
       }

       // Call the service.
       int idx = int.Parse(textBox.Text);
       var message = new ValueSet();
       message.Add("Command", "Item");
       message.Add("ID", idx);
       AppServiceResponse response = await this.inventoryService.SendMessageAsync(message);
       string result = "";

       if (response.Status == AppServiceResponseStatus.Success)
       {
           // Get the data  that the service sent to us.
           if (response.Message["Status"] as string == "OK")
           {
               result = response.Message["Result"] as string;
           }
       }

       message.Clear();
       message.Add("Command", "Price");
       message.Add("ID", idx);
       response = await this.inventoryService.SendMessageAsync(message);

       if (response.Status == AppServiceResponseStatus.Success)
       {
           // Get the data that the service sent to us.
           if (response.Message["Status"] as string == "OK")
           {
               result += " : Price = " + response.Message["Result"] as string;
           }
       }

       textBox.Text = result;
   }
   ```
    
    行 `this.inventoryService.PackageFamilyName = "Replace with the package family name";` のパッケージ ファミリ名を、「[サービス アプリを展開して、パッケージ ファミリ名を取得する](#deploy-the-service-app-and-get-the-package-family-name)」で得た **AppServiceProvider** プロジェクトのパッケージ ファミリ名に置き換えます。

    > [!NOTE]
    > 文字列リテラルは、変数に配置するのではなく、必ず貼り付けてください。 変数を使用する場合は機能しません。

    最初に、コードによってアプリ サービスとの接続が確立されます。 接続は、`this.inventoryService` を破棄するまで開いたままになります。 App service の名前は、 `AppService` `Name` **appserviceprovider** プロジェクトの **package.appxmanifest** ファイルに追加した要素の属性と一致している必要があります。 この例では `<uap3:AppService Name="com.microsoft.inventory"/>` です。

    という名前の [Valueset](/uwp/api/Windows.Foundation.Collections.ValueSet) `message` が作成され、app service に送信するコマンドを指定します。 この例のアプリ サービスでは、2 つのアクションのどちらを実行するかをコマンドが示すことを想定しています。 クライアントアプリのテキストボックスからインデックスを取得し、コマンドを使用してサービスを呼び出し、 `Item` 項目の説明を取得します。 その後、`Price` コマンドで呼び出しを行い、項目の価格を取得します。 ボタンのテキストは結果に設定されています。

    オペレーティング システムがアプリ サービスに呼び出しを接続できたかどうかを示すのは [AppServiceResponseStatus](/uwp/api/Windows.ApplicationModel.AppService.AppServiceResponseStatus) のみです。このため、アプリ サービスが要求を満たすことができたことを確認するために、アプリ サービスから受け取る [ValueSet](/uwp/api/Windows.Foundation.Collections.ValueSet) の `Status` キーをチェックします。

6. **ClientApp**プロジェクトをスタートアッププロジェクトとして設定し ( **Solution Explorer**  >  **スタートアッププロジェクトとして設定**ソリューションエクスプローラーで右クリック)、ソリューションを実行します。 数値 1 をテキスト ボックスに入力し、ボタンをクリックします。 サービスから "Chair : Price = 88.99" が返されます。

    ![Chair : price = 88.99 を表示するサンプル アプリ](images/appserviceclientapp.png)

App service の呼び出しが失敗した場合は、 **ClientApp** プロジェクトで次のことを確認します。

1.  Inventory service 接続に割り当てられているパッケージファミリ名が、 **Appserviceprovider** アプリのパッケージファミリ名と一致していることを確認します。 「 **」の行を \_ 参照して** ください `this.inventoryService.PackageFamilyName = "...";` 。
2.  ** \_ [ボタン**] で、inventory service 接続に割り当てられている app service 名が、 **appserviceprovider**の**package.appxmanifest**ファイルの app service 名と一致していることを確認します。 `this.inventoryService.AppServiceName = "com.microsoft.inventory";` をご覧ください。
3.  **Appserviceprovider**アプリがデプロイされていることを確認します。 ( **ソリューションエクスプローラー**で、ソリューションを右クリックし、[ **ソリューションの配置**] を選択します)。

## <a name="debug-the-app-service"></a>アプリ サービスのデバッグ

1.  サービスを呼び出す前にアプリ サービス プロバイダーのアプリが配置される必要があるため、ソリューションがデバッグする前に展開されるようにします。 (Visual Studio で、**[ビルド] &gt; [ソリューションの配置]** の順にクリックします)。
2.  **ソリューションエクスプローラー**で、 **appserviceprovider**プロジェクトを右クリックし、[**プロパティ**] を選択します。 **[デバッグ]** タブで、**[開始動作]** を **[起動しないが、開始時にマイ コードをデバッグする]** に変更します。 (C++ を使ってアプリ サービス プロバイダーを実装した場合、**[デバッグ]** タブから **[アプリケーションの起動]** を **[いいえ]** に変更します)。
3.  **MyAppService**プロジェクトの**Inventory.cs**ファイルで、 **onrequestreceived**にブレークポイントを設定します。
4.  **Appserviceprovider**プロジェクトをスタートアッププロジェクトとして設定し、 **F5**キーを押します。
5.  (Visual Studio からではなく) [スタート] メニューから **ClientApp** を起動します。
6.  数値 1 をテキスト ボックスに入力し、ボタンを押します。 デバッガーは、アプリ サービス内のブレークポイントでアプリ サービスの呼び出しを停止します。

## <a name="debug-the-client"></a>クライアントのデバッグ

1.  前の手順に従って、アプリ サービスを呼び出すクライアントをデバッグします。
2.  [スタート] メニューから **ClientApp** を起動します。
3.  デバッガーを **ClientApp.exe** プロセスにアタッチします ( **ApplicationFrameHost.exe** プロセスではありません)。 (Visual Studio で、**[デバッグ] &gt; [プロセスにアタッチ]** の順に選びます)。
4.  **ClientApp**プロジェクトで、**ボタンを \_ クリック**してブレークポイントを設定します。
5.  **ClientApp**のテキストボックスに数字1を入力し、ボタンをクリックすると、クライアントと app service の両方のブレークポイントがヒットするようになります。

## <a name="general-app-service-troubleshooting"></a>一般的なアプリ サービスのトラブルシューティング

App service に接続しようとした後に **Appunavailable できない** 状態が発生した場合は、次のことを確認してください。

- アプリ サービス プロバイダー プロジェクトとアプリ サービス プロジェクトが展開されていることを確認します。 クライアントを実行する前に、両方が展開されている必要があります。展開されていない場合、クライアントには接続先がありません。 **ビルド**  >  **配置ソリューション**を使用して、Visual Studio から配置できます。
- **ソリューションエクスプローラー**で、app service プロバイダープロジェクトに、app service を実装するプロジェクトへのプロジェクト間参照があることを確認します。
- `<Extensions>`「 [App service 拡張機能を Package.appxmanifest に追加](#appxmanifest)する」で前述したように、エントリとその子要素が、app service プロバイダープロジェクトに属する**package.appxmanifest**ファイルに追加されていることを確認します。
- App service プロバイダーを呼び出すクライアントの [AppServiceConnection](/uwp/api/windows.applicationmodel.appservice.appserviceconnection.appservicename) 文字列が、 `<uap3:AppService Name="..." />` app service プロバイダープロジェクトの **package.appxmanifest** ファイルに指定されていると一致していることを確認します。
- [AppServiceConnection Efamilyname](/uwp/api/windows.applicationmodel.appservice.appserviceconnection.packagefamilyname)が、前述の「 [app service 拡張機能を package.appxmanifest に追加](#appxmanifest)する」で指定した、app service プロバイダーコンポーネントのパッケージファミリ名と一致していることを確認します。
- この例のようなアウトプロセスアプリサービスでは、 `EntryPoint` `<uap:Extension ...>` app service プロバイダープロジェクトの **package.appxmanifest** ファイルの要素で指定されたが、App Service プロジェクトの [ibackgroundtask](/uwp/api/windows.applicationmodel.background.ibackgroundtask) を実装するパブリッククラスの名前空間とクラス名と一致していることを確認します。

### <a name="troubleshoot-debugging"></a>デバッグのトラブルシューティング

アプリ サービス プロバイダーまたはアプリ サービス プロジェクトのブレークポイントでデバッガーが停止しない場合は、以下を確認します。

- アプリ サービス プロバイダー プロジェクトとアプリ サービス プロジェクトが展開されていることを確認します。 クライアントを実行する前に、両方が展開されている必要があります。 これらは、**ビルド**  >  **配置ソリューション**を使用して Visual Studio から配置できます。
- デバッグするプロジェクトがスタートアッププロジェクトとして設定されていること、およびそのプロジェクトのデバッグプロパティが **F5 キー** が押されたときにプロジェクトを実行しないように設定されていることを確認します。 プロジェクトを右クリックし、**[プロパティ]**、**[デバッグ]** (または C++ では **[デバッグ]**) の順にクリックします。 C# では、**[開始動作]** を **[起動しないが、開始時にマイ コードをデバッグする]** に設定します。 C++ では、**[アプリケーションの起動]** を **[いいえ]** に設定します。

## <a name="remarks"></a>注釈

この例では、バックグラウンド タスクとして実行されるアプリ サービスを作成して、それを別のアプリから呼び出す概要を示しています。 注意すべき重要な点は次のとおりです。

* App service をホストするバックグラウンドタスクを作成します。
* `windows.appService`拡張機能を app service プロバイダーの**package.appxmanifest**ファイルに追加します。
* クライアントアプリからアプリに接続できるように、app service プロバイダーのパッケージファミリ名を取得します。
* App service プロバイダープロジェクトから app service プロジェクトへのプロジェクト間参照を追加します。
* [AppService](/uwp/api/Windows.ApplicationModel.AppService.AppServiceConnection)を使用してサービスを呼び出します。

## <a name="full-code-for-myappservice"></a>MyAppService の完全なコード

```cs
using System;
using Windows.ApplicationModel.AppService;
using Windows.ApplicationModel.Background;
using Windows.Foundation.Collections;

namespace MyAppService
{
    public sealed class Inventory : IBackgroundTask
    {
        private BackgroundTaskDeferral backgroundTaskDeferral;
        private AppServiceConnection appServiceconnection;
        private String[] inventoryItems = new string[] { "Robot vacuum", "Chair" };
        private double[] inventoryPrices = new double[] { 129.99, 88.99 };

        public void Run(IBackgroundTaskInstance taskInstance)
        {
            // Get a deferral so that the service isn't terminated.
            this.backgroundTaskDeferral = taskInstance.GetDeferral();

            // Associate a cancellation handler with the background task.
            taskInstance.Canceled += OnTaskCanceled;

            // Retrieve the app service connection and set up a listener for incoming app service requests.
            var details = taskInstance.TriggerDetails as AppServiceTriggerDetails;
            appServiceconnection = details.AppServiceConnection;
            appServiceconnection.RequestReceived += OnRequestReceived;
        }

        private async void OnRequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
        {
            // Get a deferral because we use an awaitable API below to respond to the message
            // and we don't want this call to get canceled while we are waiting.
            var messageDeferral = args.GetDeferral();

            ValueSet message = args.Request.Message;
            ValueSet returnData = new ValueSet();

            string command = message["Command"] as string;
            int? inventoryIndex = message["ID"] as int?;

            if (inventoryIndex.HasValue &&
                 inventoryIndex.Value >= 0 &&
                 inventoryIndex.Value < inventoryItems.GetLength(0))
            {
                switch (command)
                {
                    case "Price":
                        {
                            returnData.Add("Result", inventoryPrices[inventoryIndex.Value]);
                            returnData.Add("Status", "OK");
                            break;
                        }

                    case "Item":
                        {
                            returnData.Add("Result", inventoryItems[inventoryIndex.Value]);
                            returnData.Add("Status", "OK");
                            break;
                        }

                    default:
                        {
                            returnData.Add("Status", "Fail: unknown command");
                            break;
                        }
                }
            }
            else
            {
                returnData.Add("Status", "Fail: Index out of range");
            }

            // Return the data to the caller.
            await args.Request.SendResponseAsync(returnData);

            // Complete the deferral so that the platform knows that we're done responding to the app service call.
            // Note for error handling: this must be called even if SendResponseAsync() throws an exception.
            messageDeferral.Complete();
        }


        private void OnTaskCanceled(IBackgroundTaskInstance sender, BackgroundTaskCancellationReason reason)
        {
            if (this.backgroundTaskDeferral != null)
            {
                // Complete the service deferral.
                this.backgroundTaskDeferral.Complete();
            }
        }
    }
}
```

## <a name="related-topics"></a>関連トピック

* [ホスト アプリと同じプロセスで実行するようにアプリ サービスを変換する](convert-app-service-in-process.md)
* [バックグラウンド タスクによるアプリのサポート](support-your-app-with-background-tasks.md)
* [アプリ サービスのコード サンプル (C#、C++、および VB)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AppServices)