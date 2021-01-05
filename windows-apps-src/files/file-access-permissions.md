---
ms.assetid: 3A404CC0-A997-45C8-B2E8-44745539759D
title: ファイル アクセス許可
description: アプリは既定でファイル システムの特定の場所にアクセスできます。 また、ファイル ピッカーの使用や機能の宣言によって、その他の場所にアクセスすることもできます。
ms.date: 09/10/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cpp
- javascript
ms.custom: contperf-fy21q1
ms.openlocfilehash: fa37bc45ae85456aff177417a3875d724b0ebd54
ms.sourcegitcommit: 4cafc1c55511741dd1e5bfe4496d9950a9b4de1b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/04/2021
ms.locfileid: "97860257"
---
# <a name="file-access-permissions"></a>ファイル アクセス許可

ユニバーサル Windows プラットフォーム (UWP) アプリでは、既定で特定のファイル システムの場所にアクセスできます。 また、ファイル ピッカーの使用や機能の宣言によって、その他の場所にアクセスすることもできます。

## <a name="locations-that-all-apps-can-access"></a>すべてのアプリからアクセスできる場所

新しいアプリを作成すると、既定でファイル システムの次の場所にアクセスできます。

### <a name="application-install-directory"></a>アプリケーションのインストール ディレクトリ
ユーザーのシステムでアプリがインストールされているフォルダー。

アプリのインストール ディレクトリにあるファイルやフォルダーにアクセスする主な方法は 2 とおりあります。

1. 次のように、アプリのインストール ディレクトリを表す [**StorageFolder**](/uwp/api/Windows.Storage.StorageFolder) を取得できます。

    ```csharp
    Windows.Storage.StorageFolder installedLocation = Windows.ApplicationModel.Package.Current.InstalledLocation;
    ```
    
    ```javascript
    var installDirectory = Windows.ApplicationModel.Package.current.installedLocation;
    ```
    
    ```cppwinrt
    #include <winrt/Windows.Storage.h>
    ...
    Windows::Storage::StorageFolder installedLocation{ Windows::ApplicationModel::Package::Current().InstalledLocation() };
    ```
    
    ```cpp
    Windows::Storage::StorageFolder^ installedLocation = Windows::ApplicationModel::Package::Current->InstalledLocation;
    ```

    [  **StorageFolder**](/uwp/api/Windows.Storage.StorageFolder) メソッドを使って、ディレクトリ内のファイルとフォルダーにアクセスすることができます。 上の例では、この **StorageFolder** が `installDirectory` 変数に格納されています。 アプリ パッケージやインストール ディレクトリの操作について詳しくは、GitHub にある[アプリ パッケージ情報のサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Package)をご覧ください。

2. 次のように、アプリの URI を使って、アプリのインストール ディレクトリからファイルを直接取得できます。

    ```csharp
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
    
    ```cppwinrt
    Windows::Foundation::IAsyncAction ExampleCoroutineAsync()
    {
        Windows::Storage::StorageFile file{
            co_await Windows::Storage::StorageFile::GetFileFromApplicationUriAsync(Windows::Foundation::Uri{L"ms-appx:///file.txt"})
        };
        // Process file
    }
    ```
    
    ```cpp
    auto getFileTask = create_task(StorageFile::GetFileFromApplicationUriAsync(ref new Uri("ms-appx:///file.txt")));
    getFileTask.then([](StorageFile^ file) 
    {
        // Process file
    });
    ```

    [  **GetFileFromApplicationUriAsync**](/uwp/api/windows.storage.storagefile.getfilefromapplicationuriasync) が完了すると、アプリのインストール ディレクトリにある `file.txt` ファイル (この例では `file`) を表す [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) が返されます。
    
    URI の "ms-appx:///" プレフィックスは、アプリのインストール ディレクトリを参照します。 アプリの URI の使用について詳しくは、「[URI を使ってコンテンツを参照する方法](/previous-versions/windows/apps/hh781215(v=win.10))」をご覧ください。

さらに、他の場所とは異なり、[ユニバーサル Windows プラットフォーム (UWP) アプリの Win32 と COM](/uwp/win32-and-com/win32-and-com-for-uwp-apps) や [Microsoft Visual Studio の C/C++ 標準ライブラリ関数](/cpp/cpp/c-cpp-language-and-standard-libraries)を使ってアプリのインストール ディレクトリ内のファイルにアクセスすることもできます。

アプリのインストール ディレクトリは読み取り専用です。 インストール ディレクトリにファイル ピッカーでアクセスすることはできません。

### <a name="application-data-locations"></a>アプリケーション データの場所
アプリがデータを保存できるフォルダーです。 これらのフォルダー (ローカル、移動、一時) は、アプリのインストール時に作成されます。

アプリのデータの場所にあるファイルやフォルダーにアクセスする主な方法は 2 とおりあります。

1.  [  **ApplicationData**](/uwp/api/Windows.Storage.ApplicationData) プロパティを使ってアプリ データ フォルダーを取得します。

    たとえば、次のように、[**ApplicationData**](/uwp/api/Windows.Storage.ApplicationData).[**LocalFolder**](/uwp/api/windows.storage.applicationdata.localfolder) を使って、アプリのローカル フォルダーを表す [**StorageFolder**](/uwp/api/Windows.Storage.StorageFolder) を取得できます。
    
    ```csharp
    using Windows.Storage;
    StorageFolder localFolder = ApplicationData.Current.LocalFolder;
    ```
    
    ```javascript
    var localFolder = Windows.Storage.ApplicationData.current.localFolder;
    ```
    
    ```cppwinrt
    Windows::Storage::StorageFolder storageFolder{
        Windows::Storage::ApplicationData::Current().LocalFolder()
    };
    ```
    
    ```cpp
    using namespace Windows::Storage;
    StorageFolder^ storageFolder = ApplicationData::Current->LocalFolder;
    ```
    
    アプリの移動フォルダーまたは一時フォルダーにアクセスする場合は、[**RoamingFolder**](/uwp/api/windows.storage.applicationdata.roamingfolder) プロパティまたは [**TemporaryFolder**](/uwp/api/windows.storage.applicationdata.temporaryfolder) プロパティを使います。
    
    アプリ データの場所を表す [**StorageFolder**](/uwp/api/Windows.Storage.StorageFolder) を取得したら、**StorageFolder** メソッドを使って、その場所にあるファイルやフォルダーにアクセスできます。 上の例では、これらの **StorageFolder** オブジェクトが `localFolder` 変数に格納されています。 アプリ データの場所の使用について詳しくは、[ApplicationData クラス](/uwp/api/windows.storage.applicationdata)のページのガイダンスをご覧ください。また、GitHub から[アプリケーション データ サンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ApplicationData)をダウンロードしてご覧ください。

2. このように、アプリの URI を使って、アプリのローカル フォルダーからファイルを直接取得できます。
    
    ```csharp
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
    
    ```cppwinrt
    Windows::Storage::StorageFile file{
        co_await Windows::Storage::StorageFile::GetFileFromApplicationUriAsync(Windows::Foundation::Uri{ L"ms-appdata:///local/file.txt" })
    };
    // Process file
    ```
    
    ```cpp
    using Windows::Storage;
    auto getFileTask = create_task(StorageFile::GetFileFromApplicationUriAsync(ref new Uri("ms-appdata:///local/file.txt")));
    getFileTask.then([](StorageFile^ file) 
    {
        // Process file
    });
    ```
    
    [  **GetFileFromApplicationUriAsync**](/uwp/api/windows.storage.storagefile.getfilefromapplicationuriasync) が完了すると、アプリのローカル フォルダーにある `file.txt` ファイル (この例では `file`) を表す [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) が返されます。
    
    URI の "ms-appdata:///local/" プレフィックスは、アプリのローカル フォルダーを参照します。 アプリの移動フォルダーまたは一時フォルダーにあるファイルにアクセスするには、"ms-appdata:///roaming/" または "ms-appdata:///temporary/" を使います。 アプリの URI の使用について詳しくは、「[ファイル リソースを読み込む方法](/previous-versions/windows/apps/hh781229(v=win.10))」をご覧ください。

さらに、他の場所とは異なり、[UWP アプリの Win32 と COM](/uwp/win32-and-com/win32-and-com-for-uwp-apps) や Visual Studio の C/C++ 標準ライブラリ関数を使ってアプリ データの場所にあるファイルにアクセスすることもできます。

ローカル フォルダー、移動フォルダー、一時フォルダーにファイル ピッカーでアクセスすることはできません。

### <a name="removable-devices"></a>リムーバブル デバイス

さらに、接続されているデバイス上の一部のファイルに既定でアクセスできます。 これは、[自動再生拡張機能](/previous-versions/windows/apps/hh464906(v=win.10))を使って、ユーザーがデバイス (カメラや USB サム ドライブなど) をシステムに接続したときに自動的に起動されるようにする場合に使うことができます。 アプリでアクセスできるファイルの種類は、アプリ マニフェストのファイルの種類の関連付けの宣言で指定されたものだけに制限されます。

もちろん、ファイル ピッカー ([**FileOpenPicker**](/uwp/api/Windows.Storage.Pickers.FileOpenPicker) と [**FolderPicker**](/uwp/api/Windows.Storage.Pickers.FolderPicker)) を呼び出して、アプリでアクセスするファイルやフォルダーをユーザーが選べるようにすると、リムーバブル デバイス上のファイルやフォルダーにもアクセスできます。 ファイル ピッカーの使い方については、「[ピッカーでファイルやフォルダーを開く](quickstart-using-file-and-folder-pickers.md)」をご覧ください。

> [!NOTE]
> SD カードやその他のリムーバブル デバイスにアクセスする方法について詳しくは、「[SD カードへのアクセス](access-the-sd-card.md)」をご覧ください。

## <a name="locations-that-uwp-apps-can-access"></a>UWP アプリからアクセスできる場所

### <a name="users-downloads-folder"></a>ユーザーの Downloads フォルダー

ダウンロードされたファイルが保存される既定のフォルダーです。

既定では、ユーザーの "ダウンロード" フォルダーにあるファイルやフォルダーについては、そのアプリで作成したものにしかアクセスできません。 ただし、ファイル ピッカー ([**FileOpenPicker**](/uwp/api/Windows.Storage.Pickers.FileOpenPicker) または [**FolderPicker**](/uwp/api/Windows.Storage.Pickers.FolderPicker)) を呼び出して、アプリでアクセスするファイルやフォルダーをユーザーが参照して選べるようにすると、ユーザーの "ダウンロード" フォルダーにあるファイルやフォルダーにアクセスできるようになります。

- 次のように、ユーザーの "ダウンロード" フォルダーにファイルを作成できます。

    ```csharp
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
    
    ```cppwinrt
    Windows::Storage::StorageFile newFile{
        co_await Windows::Storage::DownloadsFolder::CreateFileAsync(L"file.txt")
    };
    // Process file
    ```
    
    ```cpp
    using Windows::Storage;
    auto createFileTask = create_task(DownloadsFolder::CreateFileAsync(L"file.txt"));
    createFileTask.then([](StorageFile^ newFile)
    {
        // Process file
    });
    ```

    [**DownloadsFolder**](/uwp/api/Windows.Storage.DownloadsFolder).[**CreateFileAsync**](/uwp/api/windows.storage.downloadsfolder.createfileasync) は、同じ名前のファイルが既に Downloads フォルダーにある場合に、システムで行う必要がある内容を指定できるようにオーバーロードされます。 これらのメソッドが完了すると、作成されたファイルを表す [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) が返されます。 上の例では、このファイルの名前は `newFile` です。

- 次のように、ユーザーの "ダウンロード" フォルダーにサブフォルダーを作成できます。

    ```csharp
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
    
    ```cppwinrt
    Windows::Storage::StorageFolder newFolder{
        co_await Windows::Storage::DownloadsFolder::CreateFolderAsync(L"New Folder")
    };
    // Process folder
    ```
    
    ```cpp
    using Windows::Storage;
    auto createFolderTask = create_task(DownloadsFolder::CreateFolderAsync(L"New Folder"));
    createFolderTask.then([](StorageFolder^ newFolder)
    {
        // Process folder
    });
    ```

    [**DownloadsFolder**](/uwp/api/Windows.Storage.DownloadsFolder).[**CreateFolderAsync**](/uwp/api/windows.storage.downloadsfolder.createfolderasync) は、同じ名前のサブフォルダーが既に Downloads フォルダーにある場合に、システムで行う必要がある内容を指定できるようにオーバーロードされます。 これらのメソッドが完了すると、作成されたサブフォルダーを表す [**StorageFolder**](/uwp/api/Windows.Storage.StorageFolder) が返されます。 上の例では、このファイルの名前は `newFolder` です。

## <a name="accessing-additional-locations"></a>その他の場所へのアクセス

アプリで、既定の場所以外にあるファイルやフォルダーにアクセスするには、[アプリ マニフェストで機能を宣言](../packaging/app-capability-declarations.md)するか、[ファイル ピッカーを呼び出して](quickstart-using-file-and-folder-pickers.md)アプリでアクセスするファイルやフォルダーをユーザーが選べるようにします。

[AppExecutionAlias](/uwp/schemas/appxpackage/uapmanifestschema/element-uap5-appexecutionalias) 拡張機能を宣言したアプリには、コンソール ウィンドウでアプリが起動されたディレクトリおよびその下位レベルのディレクトリに対する、ファイル システムのアクセス許可が与えられます。

### <a name="retaining-access-to-files-and-folders"></a>ファイルとフォルダーへのアクセスの保持

アプリでピッカー、ファイルのアクティブ化、ドラッグ アンド ドロップ操作などによりファイルまたはフォルダーが取得される場合、終了されるまで、アプリはそのファイルまたはフォルダーにのみアクセスできます。 将来自動的にファイルまたはフォルダーにアクセスできるようにしたい場合、[**FutureAccessList**](/uwp/api/windows.storage.accesscache.storageapplicationpermissions.futureaccesslist) に追加して、アプリが将来的にその項目に簡単にアクセスできるようにします。 また、[**MostRecentlyUsedList**](/uwp/api/windows.storage.accesscache.storageapplicationpermissions.mostrecentlyusedlist) を使用して、最近使用したファイルの一覧を簡単に管理することもできます。

### <a name="capabilities-for-accessing-other-locations"></a>他の場所にアクセスするための機能

次の表に、1 つ以上の機能の宣言や関連付けられた [**Windows.Storage**](/uwp/api/Windows.Storage) API の使用によってアクセスできるその他の場所を示します。

| 場所 | 機能 | Windows.Storage API |
|----------|------------|---------------------|
| ユーザーがアクセス権を持つすべてのファイル。 例: ドキュメント、画像、写真、ダウンロード、デスクトップ、OneDrive などです。 | **broadFileSystemAccess**<br><br>これは、制限付き機能です。 アクセスは、 **[設定]**  >  **[プライバシー]**  >  **[ファイル システム]** で構成できます。 ユーザーは **[設定]** でいつでもアクセスを許可または拒否できるため、アプリがこれらの変更に対して回復力があることを確認する必要があります。 アプリでアクセスできないことがわかった場合は、「[Windows 10 ファイル システムへのアクセスとプライバシー](https://support.microsoft.com/help/4468237/windows-10-file-system-access-and-privacy-microsoft-privacy)」の記事へのリンクを提供し、ユーザーに設定の変更を求めるように選択できます。 ユーザーはアプリを閉じ、設定を切り替え、アプリを再起動する必要があることに注意してください。 アプリの実行中に設定を切り替えると、状態を保存できるようにプラットフォームでアプリが中断され、その後、新しい設定を適用するためにアプリが強制的に終了されます。 2018 年 4 月の更新プログラムでは、アクセス許可の既定値はオンです。 2018 の年 10 月の更新プログラムでは、既定値はオフです。<br /><br />この機能を宣言するアプリを Microsoft Store に提出する場合、アプリでこの機能が必要となる理由およびこの機能の使用目的に関する追加の説明を提供する必要があります。<br/><br/>この機能は、[**Windows.Storage**](/uwp/api/Windows.Storage) 名前空間の API で動作します。 アプリでこの機能を有効にする方法の例については、この記事の最後の「**例**」セクションを参照してください。<br/><br/>**注:** この機能は、Xbox ではサポートされていません。 | 該当なし |
| Documents | **documentsLibrary**<br><br>注: アプリ マニフェストにファイルの種類の関連付けを追加し、この場所でアプリがアクセスできるファイルの種類を具体的に宣言する必要があります。 <br><br>この機能は、アプリが次の条件を満たす場合に使います。<br>- 有効な OneDrive URL またはリソース ID を使った、特定の OneDrive コンテンツへのクロスプラットフォーム オフライン アクセスを容易にする<br>- オフライン時に、開いているファイルをユーザーの OneDrive に自動的に保存する | [KnownFolders.DocumentsLibrary](/uwp/api/windows.storage.knownfolders.documentslibrary) |
| ミュージック     | **musicLibrary** <br>「[ミュージック、画像、およびビデオ ライブラリのファイルとフォルダー](quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md)」もご覧ください。 | [KnownFolders.MusicLibrary](/uwp/api/windows.storage.knownfolders.musiclibrary) |    
| ピクチャ  | **picturesLibrary**<br> 「[ミュージック、画像、およびビデオ ライブラリのファイルとフォルダー](quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md)」もご覧ください。 | [KnownFolders.PicturesLibrary](/uwp/api/windows.storage.knownfolders.pictureslibrary) |  
| ビデオ    | **videosLibrary**<br>「[ミュージック、画像、およびビデオ ライブラリのファイルとフォルダー](quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md)」もご覧ください。 | [KnownFolders.VideosLibrary](/uwp/api/windows.storage.knownfolders.videoslibrary) |   
| リムーバブル デバイス  | **removableStorage**  <br><br>注  アプリ マニフェストにファイルの種類の関連付けを追加し、この場所でアプリがアクセスできるファイルの種類を具体的に宣言する必要があります。 <br><br>「[SD カードへのアクセス](access-the-sd-card.md)」もご覧ください。 | [KnownFolders.RemovableDevices](/uwp/api/windows.storage.knownfolders.removabledevices) |  
| ホームグループ ライブラリ  | 次の機能が 1 つ以上必要です。 <br>- **musicLibrary** <br>- **picturesLibrary** <br>- **videosLibrary** | [KnownFolders.HomeGroup](/uwp/api/windows.storage.knownfolders.homegroup) |      
| メディア サーバー デバイス (DLNA) | 次の機能が 1 つ以上必要です。 <br>- **musicLibrary** <br>- **picturesLibrary** <br>- **videosLibrary** | [KnownFolders.MediaServerDevices](/uwp/api/windows.storage.knownfolders.mediaserverdevices) |
| 汎用名前付け規則 (UNC) フォルダー | 次の機能の組み合わせが必要です。 <br><br>ホーム ネットワークと社内ネットワークの機能: <br>- **privateNetworkClientServer** <br><br>インターネットとパブリック ネットワークの 1 つ以上の機能: <br>- **internetClient** <br>- **internetClientServer** <br><br>ドメイン資格情報の機能 (該当する場合):<br>- **enterpriseAuthentication** <br><br>**注:** アプリ マニフェストにファイルの種類の関連付けを追加し、この場所でアプリからアクセスできるファイルの種類を具体的に宣言する必要があります。 | フォルダーを取得する場合: <br>[StorageFolder.GetFolderFromPathAsync](/uwp/api/windows.storage.storagefolder.getfolderfrompathasync) <br><br>ファイルを取得する場合: <br>[StorageFile.GetFileFromPathAsync](/uwp/api/windows.storage.storagefile.getfilefrompathasync) |

### <a name="example"></a>例

この例では、制限された **broadFileSystemAccess** 機能を追加します。 機能を指定するだけでなく、`rescap` 名前空間を追加し、`IgnorableNamespaces` に追加する必要もあります。

```xaml
<Package
  ...
  xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
  IgnorableNamespaces="uap mp rescap">
...
<Capabilities>
    <rescap:Capability Name="broadFileSystemAccess" />
</Capabilities>
```

> [!NOTE]
> すべてのアプリ機能の一覧については、「[アプリ機能の宣言](../packaging/app-capability-declarations.md)」をご覧ください。
