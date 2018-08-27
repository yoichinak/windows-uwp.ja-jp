---
author: laurenhughes
ms.assetid: 3A404CC0-A997-45C8-B2E8-44745539759D
title: ファイル アクセス許可
description: アプリは既定でファイル システムの特定の場所にアクセスできます。 また、ファイル ピッカーの使用や機能の宣言によって、その他の場所にアクセスすることもできます。
ms.author: lahugh
ms.date: 3/30/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: dc4f3532f2aebd84c8194586b3fdaad45146ff8d
ms.sourcegitcommit: cceaf2206ec53a3e9155f97f44e4795a7b6a1d78
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/03/2018
ms.locfileid: "1701168"
---
# <a name="file-access-permissions"></a>ファイル アクセス許可


ユニバーサル Windows アプリ (アプリ) は、既定でファイル システムの特定の場所にアクセスできます。 また、ファイル ピッカーの使用や機能の宣言によって、その他の場所にアクセスすることもできます。

## <a name="the-locations-that-all-apps-can-access"></a>すべてのアプリからアクセスできる場所

新しいアプリを作成すると、既定でファイル システムの次の場所にアクセスできます。

-   **アプリケーションのインストール ディレクトリ**。 ユーザーのシステムでアプリがインストールされているフォルダーです。

    アプリのインストール ディレクトリにあるファイルやフォルダーにアクセスする主な方法は 2 とおりあります。

    1.  次のように、アプリのインストール ディレクトリを表す [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230) を取得できます。
        > [!div class="tabbedCodeSnippets"]
        ```cs
        Windows.Storage.StorageFolder installedLocation = Windows.ApplicationModel.Package.Current.InstalledLocation;
        ```
        ```javascript
        var installDirectory = Windows.ApplicationModel.Package.current.installedLocation;
        ```
        ```cpp
        Windows::Storage::StorageFolder^ installedLocation = Windows::ApplicationModel::Package::Current->InstalledLocation;
        ```

       [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230) メソッドを使って、ディレクトリ内のファイルとフォルダーにアクセスすることができます。 上の例では、この **StorageFolder** が `installDirectory` 変数に格納されています。 アプリ パッケージやインストール ディレクトリの操作について詳しくは、GitHub にある[アプリ パッケージ情報のサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Package)をご覧ください。

    2.  次のように、アプリの URI を使って、アプリのインストール ディレクトリからファイルを直接取得できます。
        > [!div class="tabbedCodeSnippets"]
        ```cs
        using Windows.Storage;            
        StorageFile file = await StorageFile.GetFileFromApplicationUriAsync(new Uri("ms-appx:///file.txt"));
        ```
        ```javascript
        Windows.Storage.StorageFile.getFileFromApplicationUriAsync("ms-appx:///file.txt").done(
            function(file) {
                // Process file
            }
        );
        ```
        ```cpp
        auto getFileTask = create_task(StorageFile::GetFileFromApplicationUriAsync(ref new Uri("ms-appx:///file.txt")));
        getFileTask.then([](StorageFile^ file) 
        {
            // Process file
        });
        ```

        [**GetFileFromApplicationUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701741) が完了すると、アプリのインストール ディレクトリにある `file.txt` ファイル (この例では `file`) を表す [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) が返されます。

        URI の "ms-appx:///" プレフィックスは、アプリのインストール ディレクトリを参照します。 アプリの URI の使用について詳しくは、「[URI を使ってコンテンツを参照する方法](https://msdn.microsoft.com/library/windows/apps/hh781215)」をご覧ください。

    さらに、他の場所とは異なり、[ユニバーサル Windows プラットフォーム (UWP) アプリの Win32 と COM](https://msdn.microsoft.com/library/windows/apps/br205757) や [Microsoft Visual Studio の C/C++ 標準ライブラリ関数](http://msdn.microsoft.com/library/hh875057.aspx)を使ってアプリのインストール ディレクトリ内のファイルにアクセスすることもできます。

    アプリのインストール ディレクトリは読み取り専用です。 インストール ディレクトリにファイル ピッカーでアクセスすることはできません。

-   **アプリケーション データの場所。** アプリがデータを保存できるフォルダーです。 これらのフォルダー (ローカル、移動、一時) は、アプリのインストール時に作成されます。

    アプリ データの場所にあるファイルやフォルダーにアクセスする主な方法は 2 とおりあります。

    1.  [**ApplicationData**](https://msdn.microsoft.com/library/windows/apps/br241587) プロパティを使ってアプリ データ フォルダーを取得します。

        たとえば、次のように、[**ApplicationData**](https://msdn.microsoft.com/library/windows/apps/br241587).[**LocalFolder**](https://msdn.microsoft.com/library/windows/apps/br241621) を使って、アプリのローカル フォルダーを表す [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230) を取得できます。
        > [!div class="tabbedCodeSnippets"]
        ```cs
        using Windows.Storage;
        StorageFolder localFolder = ApplicationData.Current.LocalFolder;
        ```
        ```javascript
        var localFolder = Windows.Storage.ApplicationData.current.localFolder;
        ```
        ```cpp
        using namespace Windows::Storage;
        StorageFolder^ storageFolder = ApplicationData::Current->LocalFolder;
        ```

        アプリの移動フォルダーまたは一時フォルダーにアクセスする場合は、[**RoamingFolder**](https://msdn.microsoft.com/library/windows/apps/br241623) プロパティまたは [**TemporaryFolder**](https://msdn.microsoft.com/library/windows/apps/br241629) プロパティを使います。

        アプリ データの場所を表す [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230) を取得したら、**StorageFolder** メソッドを使って、その場所にあるファイルやフォルダーにアクセスできます。 上の例では、これらの **StorageFolder** オブジェクトが `localFolder` 変数に格納されています。 アプリ データの場所の使用について詳しくは、[ApplicationData クラス](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata)のページのガイダンスをご覧ください。また、GitHub から[アプリケーション データ サンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ApplicationData)をダウンロードしてご覧ください。

    2.  たとえば、次のように、アプリの URI を使って、アプリのローカル フォルダーからファイルを直接取得できます。
        > [!div class="tabbedCodeSnippets"]
        ```cs
        using Windows.Storage;
        StorageFile file = await StorageFile.GetFileFromApplicationUriAsync(new Uri("ms-appdata:///local/file.txt"));
        ```
        ```javascript
        Windows.Storage.StorageFile.getFileFromApplicationUriAsync("ms-appdata:///local/file.txt").done(
            function(file) {
                // Process file
            }
        );
        ```
        ```cpp
        using Windows::Storage;
        auto getFileTask = create_task(StorageFile::GetFileFromApplicationUriAsync(ref new Uri("ms-appdata:///local/file.txt")));
        getFileTask.then([](StorageFile^ file) 
        {
            // Process file
        });
        ```

        [**GetFileFromApplicationUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701741) が完了すると、アプリのローカル フォルダーにある `file.txt` ファイル (この例では `file`) を表す [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) が返されます。

        URI の "ms-appdata:///local/" プレフィックスは、アプリのローカル フォルダーを参照します。 アプリの移動フォルダーまたは一時フォルダーにあるファイルにアクセスするには、"ms-appdata:///roaming/" または "ms-appdata:///temporary/" を使います。 アプリの URI の使用について詳しくは、「[ファイル リソースを読み込む方法](https://msdn.microsoft.com/library/windows/apps/hh781229)」をご覧ください。

    さらに、他の場所とは異なり、[UWP アプリの Win32 と COM](https://msdn.microsoft.com/library/windows/apps/br205757) や Visual Studio の C/C++ 標準ライブラリ関数を使ってアプリ データの場所にあるファイルにアクセスすることもできます。

    ローカル フォルダー、移動フォルダー、一時フォルダーにファイル ピッカーでアクセスすることはできません。

-   **リムーバブル デバイス。** さらに、接続されているデバイス上の一部のファイルに既定でアクセスできます。 これは、[自動再生拡張機能](https://msdn.microsoft.com/library/windows/apps/xaml/hh464906.aspx#autoplay)を使って、ユーザーがデバイス (カメラや USB サム ドライブなど) をシステムに接続したときに自動的に起動されるようにする場合に使うことができます。 アプリでアクセスできるファイルの種類は、アプリ マニフェストのファイルの種類の関連付けの宣言で指定されたものだけに制限されます。

    もちろん、ファイル ピッカー ([**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) と [**FolderPicker**](https://msdn.microsoft.com/library/windows/apps/br207881)) を呼び出して、アプリでアクセスするファイルやフォルダーをユーザーが選べるようにすると、リムーバブル デバイス上のファイルやフォルダーにもアクセスできます。 ファイル ピッカーの使い方については、「[ピッカーでファイルやフォルダーを開く](quickstart-using-file-and-folder-pickers.md)」をご覧ください。

    > [!NOTE]
    > SD カードやその他のリムーバブル デバイスにアクセスする方法について詳しくは、「[SD カードへのアクセス](access-the-sd-card.md)」をご覧ください。

     

## <a name="locations-uwp-apps-can-access"></a>UWP アプリからアクセスできる場所

-   **ユーザーの "ダウンロード" フォルダー。** ダウンロードされたファイルが保存される既定のフォルダーです。

    既定では、ユーザーの "ダウンロード" フォルダーにあるファイルやフォルダーについては、そのアプリで作成したものにしかアクセスできません。 ただし、ファイル ピッカー ([**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) または [**FolderPicker**](https://msdn.microsoft.com/library/windows/apps/br207881)) を呼び出して、アプリでアクセスするファイルやフォルダーをユーザーが参照して選べるようにすると、ユーザーの "ダウンロード" フォルダーにあるファイルやフォルダーにアクセスできるようになります。

    -   次のように、ユーザーの "ダウンロード" フォルダーにファイルを作成できます。
        > [!div class="tabbedCodeSnippets"]
        ```cs
        using Windows.Storage;
        StorageFile newFile = await DownloadsFolder.CreateFileAsync("file.txt");
        ```
        ```javascript
        Windows.Storage.DownloadsFolder.createFileAsync("file.txt").done(
            function(newFile) {
                // Process file
            }
        );
        ```
        ```cpp
        using Windows::Storage;
        auto createFileTask = create_task(DownloadsFolder::CreateFileAsync(L"file.txt"));
        createFileTask.then([](StorageFile^ newFile)
        {
            // Process file
        });
        ```

        [**DownloadsFolder**](https://msdn.microsoft.com/library/windows/apps/br241632).[**CreateFileAsync**](https://msdn.microsoft.com/library/windows/apps/hh996761) は、同じ名前のファイルが既に "ダウンロード" フォルダーにある場合の処理を指定できるようにオーバーロードされます。 これらのメソッドが完了すると、作成されたファイルを表す [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) が返されます。 上の例では、このファイルの名前は `newFile` です。

    -   次のように、ユーザーの "ダウンロード" フォルダーにサブフォルダーを作成できます。
        > [!div class="tabbedCodeSnippets"]
        ```cs
        using Windows.Storage;
        StorageFolder newFolder = await DownloadsFolder.CreateFolderAsync("New Folder");
        ```
        ```javascript
        Windows.Storage.DownloadsFolder.createFolderAsync("New Folder").done(
            function(newFolder) {
                // Process folder
            }
        );
        ```
        ```cpp
        using Windows::Storage;
        auto createFolderTask = create_task(DownloadsFolder::CreateFolderAsync(L"New Folder"));
        createFolderTask.then([](StorageFolder^ newFolder)
        {
            // Process folder
        });
        ```

        [**DownloadsFolder**](https://msdn.microsoft.com/library/windows/apps/br241632).[**CreateFolderAsync**](https://msdn.microsoft.com/library/windows/apps/hh996763) は、同じ名前のサブフォルダーが既に "ダウンロード" フォルダーにある場合の処理を指定できるようにオーバーロードされます。 これらのメソッドが完了すると、作成されたサブフォルダーを表す [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230) が返されます。 上の例では、このファイルの名前は `newFolder` です。

    "ダウンロード" フォルダーにファイルやフォルダーを作成する場合は、以降に簡単にアクセスできるように、その項目をアプリの [**FutureAccessList**](https://msdn.microsoft.com/library/windows/apps/br207457) に追加することをお勧めします。

## <a name="accessing-additional-locations"></a>その他の場所へのアクセス

アプリで、既定の場所以外にあるファイルやフォルダーにアクセスするには、アプリ マニフェストで機能を宣言するか、ファイル ピッカーを呼び出してアプリでアクセスするファイルやフォルダーをユーザーが選べるようにします。詳しくは、「[アプリ機能の宣言](https://msdn.microsoft.com/library/windows/apps/mt270968)」または「[ピッカーでファイルやフォルダーを開く](quickstart-using-file-and-folder-pickers.md)」をご覧ください。

[AppExecutionAlias](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap5-appexecutionalias) 拡張機能を宣言したアプリには、コンソール ウィンドウでアプリが起動されたディレクトリおよびその下位レベルのディレクトリに対する、ファイル システムのアクセス許可が与えられます。

次の表に、機能の宣言や関連する [**Windows.Storage**](https://msdn.microsoft.com/library/windows/apps/br227346) API の使用によってアクセスできるその他の場所を示します。

| 場所 | 機能 | Windows.Storage API |
|----------|------------|---------------------|
| ユーザーがアクセス権を持つすべてのファイル。 例: ドキュメント、画像、写真、ダウンロード、デスクトップ、OneDrive などです。 | broadFileSystemAccess<br><br>これは、制限付き機能です。 最初に使うとき、システムはユーザーにアクセスを許可するかどうかを要求します。 アクセスは、[設定] > [プライバシー] > [ファイル システム] で構成できます。 この機能を宣言するアプリを Microsoft Store に提出する場合、アプリでこの機能が必要となる理由およびこの機能の使用目的に関する追加の説明を提供する必要があります。<br>この機能は、[**Windows.Storage**](https://msdn.microsoft.com/library/windows/apps/BR227346) 名前空間の API で動作します。 | なし |
| ドキュメント | DocumentsLibrary <br><br>注: アプリ マニフェストにファイルの種類の関連付けを追加し、この場所でアプリがアクセスできるファイルの種類を具体的に宣言する必要があります。 <br><br>この機能は、アプリが次の条件を満たす場合に使います。<br>- 有効な OneDrive URL またはリソース ID を使った、特定の OneDrive コンテンツへのクロスプラットフォーム オフライン アクセスを容易にする<br>- オフライン時に、開いているファイルをユーザーの OneDrive に自動的に保存する | [KnownFolders.DocumentsLibrary](https://msdn.microsoft.com/library/windows/apps/br227152) |
| ミュージック     | MusicLibrary <br>「[ミュージック、画像、およびビデオ ライブラリのファイルとフォルダー](quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md)」もご覧ください。 | [KnownFolders.MusicLibrary](https://msdn.microsoft.com/library/windows/apps/br227155) |    
| 画像  | PicturesLibrary<br> 「[ミュージック、画像、およびビデオ ライブラリのファイルとフォルダー](quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md)」もご覧ください。 | [KnownFolders.PicturesLibrary](https://msdn.microsoft.com/library/windows/apps/br227156) |  
| ビデオ    | VideosLibrary<br>「[ミュージック、画像、およびビデオ ライブラリのファイルとフォルダー](quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md)」もご覧ください。 | [KnownFolders.VideosLibrary](https://msdn.microsoft.com/library/windows/apps/br227159) |   
| リムーバブル デバイス  | RemovableDevices <br><br>注  アプリ マニフェストにファイルの種類の関連付けを追加し、この場所でアプリがアクセスできるファイルの種類を具体的に宣言する必要があります。 <br><br>「[SD カードへのアクセス](access-the-sd-card.md)」もご覧ください。 | [KnownFolders.RemovableDevices](https://msdn.microsoft.com/library/windows/apps/br227158) |  
| ホームグループ ライブラリ  | 次の機能が 1 つ以上必要です。 <br>- MusicLibrary <br>- PicturesLibrary <br>- VideosLibrary | [KnownFolders.HomeGroup](https://msdn.microsoft.com/library/windows/apps/br227153) |      
| メディア サーバー デバイス (DLNA) | 次の機能が 1 つ以上必要です。 <br>- MusicLibrary <br>- PicturesLibrary <br>- VideosLibrary | [KnownFolders.MediaServerDevices](https://msdn.microsoft.com/library/windows/apps/br227154) |
| 汎用名前付け規則 (UNC) フォルダー | 次の機能の組み合わせが必要です。 <br><br>ホーム ネットワークと社内ネットワークの機能: <br>- PrivateNetworkClientServer <br><br>インターネットとパブリック ネットワークの 1 つ以上の機能: <br>- InternetClient <br>- InternetClientServer <br><br>ドメイン資格情報の機能 (該当する場合):<br>- EnterpriseAuthentication <br><br>注: アプリ マニフェストにファイルの種類の関連付けを追加し、この場所でアプリがアクセスできるファイルの種類を具体的に宣言する必要があります。 | フォルダーを取得する場合: <br>[StorageFolder.GetFolderFromPathAsync](https://msdn.microsoft.com/library/windows/apps/br227278) <br><br>ファイルを取得する場合: <br>[StorageFile.GetFileFromPathAsync](https://msdn.microsoft.com/library/windows/apps/br227206) |

**例**

この例では、制限付きの `broadFileSystemAccess` 機能を追加します。 機能を指定するだけでなく、`rescap` 名前空間を追加し、`IgnorableNamespaces` に追加する必要もあります。

``` xaml
<Package
  ...
  xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
  IgnorableNamespaces="uap mp uap5 rescap">

...
<Capabilities>
    <rescap:Capability Name="broadFileSystemAccess" />
</Capabilities>
```

> [!NOTE]
> すべてのアプリ機能の一覧については、「[アプリ機能の宣言](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations)」をご覧ください。