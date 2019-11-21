---
description: UI の背後には、ビジネス レイヤーとデータ レイヤーがあります。
title: Silverlight のビジネス層とデータ層 Windows Phone UWP に移植する
ms.assetid: 27c66759-2b35-41f5-9f7a-ceb97f4a0e3f
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 25d8bba5e1b26613185017642d63128cc2b1f7f6
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259090"
---
#  <a name="porting-windowsphone-silverlight-business-and-data-layers-to-uwp"></a>Silverlight のビジネス層とデータ層 Windows Phone UWP に移植する


前のトピックは、「[入出力、デバイス、アプリ モデルの移植](wpsl-to-uwp-input-and-sensors.md)」でした。

UI の背後には、ビジネス レイヤーとデータ レイヤーがあります。 こうしたレイヤーのコードでは、オペレーティング システムと .NET Framework API (たとえば、バックグラウンド処理、場所、カメラ、ファイル システム、ネットワーク、その他のデータ アクセスなど) を呼び出します。 その大半が[ユニバーサル Windows プラットフォーム (UWP) アプリ](https://docs.microsoft.com/previous-versions/windows/br211369(v=win.10))で利用可能であり、したがって変更なくこのコードの大半を移植できると考えられます。

## <a name="asynchronous-methods"></a>非同期メソッド

ユニバーサル Windows プラットフォーム (UWP) で優先されることの 1 つは、真に一貫して応答性が高いアプリを構築できるようにすることです。 アニメーションは常にスムーズで、パンやスワイプなどのタッチ操作は遅延なく瞬時であり、UI が指に密着するように感じさせます。 これを実現するために、50 ミリ秒以内の完了が保証できないすべての UWP API は非同期になり、名前の後に **Async** が付加されています。 UI スレッドは、**Async** メソッドの呼び出し後に直ちに戻り、別のスレッドで処理を実行します。 **Async** メソッドの使用は、構文的に非常に簡単であり、C# の **await** 演算子、JavaScript promise オブジェクト、C++ 後続タスクを使います。 詳しくは、「[非同期プログラミング](https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-universal-windows-platform-apps)」をご覧ください。

## <a name="background-processing"></a>バックグラウンド処理

Silverlight アプリ Windows Phone は、アプリがフォアグラウンドにないときに、マネージ**Scheduledtaskagent**オブジェクトを使用してタスクを実行できます。 UWP アプリでは、同様の方法でバックグラウンド タスクを作成、登録するために [**BackgroundTaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) クラスを使います。 バックグラウンド タスクの処理を実装するクラスを定義します。 システムでは、バックグラウンド タスクを定期的に実行し、処理を実行するクラスの [**Run**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtask.run) メソッドを呼び出します。 UWP アプリでは、アプリ パッケージ マニフェストで**バックグラウンド タスク**の宣言を忘れずに設定します。 詳しくは、「[バックグラウンド タスクによるアプリのサポート](https://docs.microsoft.com/windows/uwp/launch-resume/support-your-app-with-background-tasks)」をご覧ください。

バックグラウンドで大きなデータファイルを転送するには、Windows Phone Silverlight アプリで**Backgroundtransferservice**クラスを使用します。 UWP アプリは、このために [**Windows.Networking.BackgroundTransfer**](https://docs.microsoft.com/uwp/api/Windows.Networking.BackgroundTransfer) 名前空間の API を使います。 機能は同様のパターンを使って転送を開始しますが、新しい API では機能と性能が向上しています。 詳しくは、「[バックグラウンドでのデータの転送](https://docs.microsoft.com/previous-versions/windows/apps/hh452975(v=win.10))」をご覧ください。

Windows Phone Silverlight アプリでは、アプリがフォアグラウンドにないときに、 **Microsoft の Tel オーディオ**名前空間のマネージクラスを使用してオーディオを再生します。 UWP は Windows Phone ストア アプリ モデルを使います。詳しくは、「[バックグラウンド オーディオ](https://docs.microsoft.com/windows/uwp/audio-video-camera/background-audio)」および[バックグラウンド オーディオ](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundAudio)のサンプルをご覧ください。

## <a name="cloud-services-networking-and-databases"></a>クラウド サービス、ネットワーク、データベース

Azure を使ってクラウドでデータとアプリ サービスをホストできます。 「[Mobile Services の使用](https://azure.microsoft.com/documentation/articles/mobile-services-windows-store-get-started/)」をご覧ください。 オンラインおよびオフラインの両データを必要とするソリューションの場合は、「[Mobile Services でのオフライン データの同期の使用](https://azure.microsoft.com/documentation/articles/mobile-services-windows-store-dotnet-get-started-offline-data/)」をご覧ください。

UWP は **System.Net.HttpWebRequest** クラスを部分的にサポートしていますが、**System.Net.WebClient** クラスはサポートされていません。 お勧めできる将来的な代替案は、[**Windows.Web.Http.HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) クラス (または、.NET をサポートする他のプラットフォームにコードを移植可能にする場合は [System.Net.Http.HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))) です。 これらの API では、[System.Net.Http.HttpRequestMessage](https://docs.microsoft.com/previous-versions/visualstudio/hh159020(v=vs.118)) を使って HTTP 要求を表します。

UWP アプリには、現在、大量のデータ アクセスを実行するシナリオ (基幹業務 (LOB) シナリオなど) を対象としたサポートは組み込まれていません。 ただし、ローカル トランザクション データベース サービスのために SQLite を使うことができます。 詳しくは、「[SQLite](https://marketplace.visualstudio.com/items?itemName=SQLiteDevelopmentTeam.SQLiteforUniversalWindowsPlatform)」をご覧ください。

相対 URI ではなく絶対 URI を Windows ランタイム型に渡します。 「[Windows ランタイムへの URI の引き渡し](https://docs.microsoft.com/dotnet/standard/cross-platform/passing-a-uri-to-the-windows-runtime)」をご覧ください。

## <a name="launchers-and-choosers"></a>ランチャーとセレクター

ランチャーとセレクターを使用して ( **Microsoft. Phone. Tasks**名前空間にあります)、Windows Phone Silverlight アプリはオペレーティングシステムと対話して、電子メールの作成、写真の選択、他のアプリとの特定の種類のデータの共有などの一般的な操作を実行できます。 「 [Silverlight を Windows 10 の名前空間とクラスのマッピングに Windows Phone](wpsl-to-uwp-namespace-and-class-mappings.md) 」のトピックの「 **Microsoft Phone のタスク**」を検索して、同等の UWP の種類を見つけます。 これには、ランチャーおよびピッカーと呼ばれる同様のメカニズムから、アプリ間でデータを共有するコントラクトの実装に至るまで、一連の型が含まれます。

Windows Phone Silverlight アプリを休止状態にしたり、写真選択タスクなどを使用して廃棄したりすることができます。 UWP アプリは、[**FileOpenPicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker) クラスの使用中はアクティブで実行中のままになります。

## <a name="monetization-trial-mode-and-in-app-purchases"></a>収益化 (試用モードとアプリ内購入)

Windows Phone Silverlight アプリでは、コードを移植する必要がないように、試用モードやアプリ内購入機能のほとんどに UWP [**Currentapp**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp)クラスを使用できます。 しかし、Windows Phone Silverlight アプリは MarketplaceDetailTask を呼び出して、アプリに購入を提供します **。**

```csharp
    private void Buy()
    {
        MarketplaceDetailTask marketplaceDetailTask = new MarketplaceDetailTask();
        marketplaceDetailTask.ContentType = MarketplaceContentType.Applications;
        marketplaceDetailTask.Show();
    }
```

UWP の  [**RequestAppPurchaseAsync**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestapppurchaseasync) メソッドを呼び出すコードを移植します。

```csharp
    private async void Buy()
    {
        await Windows.ApplicationModel.Store.CurrentApp.RequestAppPurchaseAsync(false);
    }
```

テスト目的のためにアプリ購入機能およびアプリ内購入機能をシミュレートするコードがある場合は、代わりに [**CurrentAppSimulator**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentAppSimulator) クラスを使うようにこのコードを移植できます。

## <a name="notifications-for-tile-or-toast-updates"></a>タイルまたはトーストの更新通知

通知は Windows Phone Silverlight アプリのプッシュ通知モデルの拡張機能です。 Windows プッシュ通知サービス (WNS) から通知を受信した場合、タイル更新またはトーストにより UI にこの情報を提示できます。 通知機能の UI 側の移植については、「[タイルとトースト](w8x-to-uwp-porting-xaml-and-ui.md)」をご覧ください。

UWP アプリで通知を使う方法について詳しくは、「[トースト通知の送信](https://docs.microsoft.com/previous-versions/windows/apps/hh868266(v=win.10))」をご覧ください。

C++、C#、または Visual Basic を使った Windows ランタイム アプリでのタイル、トースト、バッジ、バナー、通知の使用についての情報とチュートリアルは、「[タイル、バッジ、トースト通知の操作](https://docs.microsoft.com/previous-versions/windows/apps/hh868259(v=win.10))」をご覧ください。

## <a name="storage-file-access"></a>ストレージ (ファイル アクセス)

アプリケーション設定を分離ストレージにキーと値のペアとして格納する Silverlight コードは、簡単に移植 Windows Phone ます。 次に、前と後の例を示します。最初に Windows Phone Silverlight バージョンを示します。

```csharp
    var propertySet = IsolatedStorageSettings.ApplicationSettings;
    const string key = "favoriteAuthor";
    propertySet[key] = "Charles Dickens";
    propertySet.Save();
    string myFavoriteAuthor = propertySet.Contains(key) ? (string)propertySet[key] : "<none>";
```

次に相当する UWP の要素を示します。

```csharp
    var propertySet = Windows.Storage.ApplicationData.Current.LocalSettings.Values;
    const string key = "favoriteAuthor";
    propertySet[key] = "Charles Dickens";
    string myFavoriteAuthor = propertySet.ContainsKey(key) ? (string)propertySet[key] : "<none>";
```

IsolatedStorageFile 名前空間のサブセットを使用**できますが**、多くの Windows Phone Silverlight アプリは、クラスを使用してファイル i/o を実行します。これは、より長い期間サポートされているためです。 **IsolatedStorageFile**が使用されていることを前提として、ファイルの書き込みと読み取りの前と後の例を次に示します。まず、Silverlight のバージョンを Windows Phone します。

```csharp
    const string filename = "FavoriteAuthor.txt";
    using (var store = IsolatedStorageFile.GetUserStoreForApplication())
    {
        using (var streamWriter = new StreamWriter(store.CreateFile(filename)))
        {
            streamWriter.Write("Charles Dickens");
        }
        using (var StreamReader = new StreamReader(store.OpenFile(filename, FileMode.Open, FileAccess.Read)))
        {
            string myFavoriteAuthor = StreamReader.ReadToEnd();
        }
    }
```

そして、UWP を使う同じ機能です。

```csharp
    const string filename = "FavoriteAuthor.txt";
    var store = Windows.Storage.ApplicationData.Current.LocalFolder;
    Windows.Storage.StorageFile file = await store.CreateFileAsync(filename, Windows.Storage.CreationCollisionOption.ReplaceExisting);
    await Windows.Storage.FileIO.WriteTextAsync(file, "Charles Dickens");
    file = await store.GetFileAsync(filename);
    string myFavoriteAuthor = await Windows.Storage.FileIO.ReadTextAsync(file);
```

Windows Phone Silverlight アプリには、オプションの SD カードへの読み取り専用アクセス権があります。 UWP アプリは、オプションの SD カードに読み取り専用のアクセスを行います。 詳しくは、「[SD カードへのアクセス](https://docs.microsoft.com/windows/uwp/files/access-the-sd-card)」をご覧ください。

UWP アプリでの写真、音楽、ビデオ ファイルへのアクセスについて詳しくは、「[ミュージック、画像、およびビデオ ライブラリのファイルとフォルダー](https://docs.microsoft.com/windows/uwp/files/quickstart-managing-folders-in-the-music-pictures-and-videos-libraries)」をご覧ください。

詳しくは、「[ファイル、フォルダー、およびライブラリ](https://docs.microsoft.com/windows/uwp/files/index)」をご覧ください。

次のトピックは、「[フォーム ファクターと UX の移植](wpsl-to-uwp-form-factors-and-ux.md)」です。

## <a name="related-topics"></a>関連トピック

* [名前空間とクラスのマッピング](wpsl-to-uwp-namespace-and-class-mappings.md)
 

