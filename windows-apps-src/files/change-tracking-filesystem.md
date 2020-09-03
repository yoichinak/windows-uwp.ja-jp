---
title: ファイル システムの変更のバック グラウンドでの追跡
description: システム内でユーザーがファイルやフォルダーを移動したときに、それらの変更をバックグラウンドで追跡する方法を説明します。
ms.date: 12/19/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 15a1b05281215136afc75190bc3812811ee69ea8
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173236"
---
# <a name="track-file-system-changes-in-the-background"></a>ファイル システムの変更のバック グラウンドでの追跡

**重要な API**

-   [**StorageLibraryChangeTracker**](/uwp/api/Windows.Storage.StorageLibraryChangeTracker)
-   [**StorageLibraryChangeReader**](/uwp/api/windows.storage.storagelibrarychangereader)
-   [**StorageLibraryChangedTrigger**](/uwp/api/Windows.ApplicationModel.Background.StorageLibraryContentChangedTrigger)
-   [**StorageLibrary**](/uwp/api/windows.storage.storagelibrary)

アプリで [**StorageLibraryChangeTracker**](/uwp/api/Windows.Storage.StorageLibraryChangeTracker) クラスを使用すると、システム内でユーザーがファイルやフォルダーを移動したときに、それらの変更を追跡することができます。 アプリでは、**StorageLibraryChangeTracker** クラスを使用して以下を追跡できます。

- 追加、削除、変更を含むファイル操作。
- 名前変更や削除などのフォルダー操作。
- ドライブへのファイルやフォルダーの移動。

このガイドでは、変更トラッカーを操作するためのプログラミング モデルについて説明します。サンプル コードを参照し、**StorageLibraryChangeTracker** によって追跡されるさまざまなファイル操作の種類について理解してください。

**StorageLibraryChangeTracker** は、ユーザー ライブラリに対して、またはローカル コンピューター上の任意のフォルダーに対して機能します。 これには、セカンダリ ドライブやリムーバブル ドライブが含まれますが、NAS ドライブやネットワーク ドライブは含まれません。

## <a name="using-the-change-tracker"></a>変更トラッカーを使用する

変更トラッカーは、直前の *N* 個のファイル システム操作を格納する循環バッファーとして、システム上に実装されます。 アプリでは、バッファーから変更が読み取られ、読み取られた変更を処理して独自のエクスペリエンスに取り込むことができます。 アプリでは、変更が終了すると、変更を処理済みとしてマークされ、それらが再び表示されることはありません。

変更トラッカーをフォルダーに対して使用するには、次の手順に従います。

1. フォルダーに対して変更の追跡を有効にします。
2. 変更の発生を待機します。
3. 変更を読み取ります。
4. 変更を受け入れます。

次のセクションでは、いくつかのコード例を使用して各手順を説明します。 完全なコード サンプルをこの記事の最後に示します。

### <a name="enable-the-change-tracker"></a>変更トラッカーを有効にする

アプリで最初に行う必要があるのは、特定のライブラリを変更追跡の対象にすることをシステムに指示することです。 これは、対象のライブラリに対して変更トラッカーの [**Enable**](/uwp/api/windows.storage.storagelibrarychangetracker.enable) メソッドを呼び出すことによって行います。

```csharp
StorageLibrary videosLib = await StorageLibrary.GetLibraryAsync(KnownLibraryId.Videos);
StorageLibraryChangeTracker videoTracker = videosLib.ChangeTracker;
videoTracker.Enable();
```

いくつかの重要な注意:

- [**StorageLibrary**](/uwp/api/windows.storage.storagelibrary) オブジェクトを作成する前に、アプリに正しいライブラリへのアクセス許可があることをマニフェスト内で確認してください。 詳しくは、「[ファイル アクセス許可](./file-access-permissions.md)」をご覧ください。
- [**Enable**](/uwp/api/windows.storage.storagelibrarychangetracker.enable) はスレッド セーフであり、ポインターのリセットは発生せず、必要な回数だけ呼び出すことができます (これについての詳細は後述)。

![空の変更トラッカーを有効にする](images/changetracker-enable.png)

### <a name="wait-for-changes"></a>変更の発生を待機する

変更トラッカーが初期化された後、アプリが実行されていないときでも、ライブラリ内で発生するすべての操作の記録が開始されます。 [**StorageLibraryChangedTrigger**](/uwp/api/Windows.ApplicationModel.Background.StorageLibraryContentChangedTrigger) イベントに登録することによって、変更があった場合にアプリをアクティブ化するように登録できます。

![アプリによる変更の読み取りはなく、変更が変更トラッカーに追加される](images/changetracker-waiting.png)

### <a name="read-the-changes"></a>変更を読み取る

アプリでは、変更トラッカーに変更をポーリングし、最後のチェック以降の変更の一覧を受け取ることができます。 次のコードでは、変更トラッカーから変更の一覧を取得する方法を示します。

```csharp
StorageLibrary videosLibrary = await StorageLibrary.GetLibraryAsync(KnownLibraryId.Videos);
videosLibrary.ChangeTracker.Enable();
StorageLibraryChangeReader videoChangeReader = videosLibrary.ChangeTracker.GetChangeReader();
IReadOnlyList changeSet = await changeReader.ReadBatchAsync();
```

その後アプリでは、変更を処理し、必要に応じて独自のエクスペリエンスまたはデータベースに取り込みます。

![変更トラッカーからアプリのデータベースへの変更の読み込み](images/changetracker-reading.png)

> [!TIP]
> Enable の 2 回目の呼び出しは、アプリで変更を読み取り中にユーザーがライブラリに別のフォルダーを追加した場合に、競合状態が発生するのを防止するための呼び出しです。 **Enable** への追加呼び出しがない場合、ユーザーがライブラリ内でフォルダーを変更すると、コードは ecSearchFolderScopeViolation (0x80070490) で失敗します。

### <a name="accept-the-changes"></a>変更を受け入れる

アプリでは、変更の処理が終了したら、[**AcceptChangesAsync**](/uwp/api/windows.storage.storagelibrarychangereader.acceptchangesasync) メソッドを呼び出して、それらの変更が再び表示されないようにシステムに指示する必要があります。

```csharp
await changeReader.AcceptChangesAsync();
```

![変更を既読としてマークし二度と表示されないようにする](images/changetracker-accepting.png)

今後アプリで変更トラッカーを読み取るときは、新しい変更のみを受け取ります。

- [**ReadBatchAsync**](/uwp/api/windows.storage.storagelibrarychangereader.readbatchasync) と [AcceptChangesAsync](/uwp/api/windows.storage.storagelibrarychangereader.acceptchangesasync) の呼び出しの間に変更が発生した場合、ポインターは、アプリに表示された最新の変更にまでしか移されません。 その他の変更は、**ReadBatchAsync** を次回呼び出したときにまだ使用できます。
- 変更を受け入れなかった場合、アプリでの **ReadBatchAsync** の次回呼び出し時に、システムから同じ変更セットが返されます。

## <a name="important-things-to-remember"></a>重要な注意点

変更トラッカーを使用する場合、すべてが正しく機能していることを確認するために注意が必要なことがいくつかあります。

### <a name="buffer-overruns"></a>バッファーのオーバーラン

システム上で発生するすべての操作をアプリケーションで読み取り可能になるまで保持するために、変更トラッカーに十分なスペースを確保するようにしていますが、循環バッファーが上書きされる前にアプリで変更が読み取られなかったシナリオを想像するのは非常に簡単です。 特に、ユーザーがバックアップからデータを復元する場合や、カメラ付き携帯電話から大量の画像を同期する場合があります。

この場合、**ReadBatchAsync** からエラー コード [**StorageLibraryChangeType.ChangeTrackingLost**](/uwp/api/windows.storage.storagelibrarychangetype) が返されます。 アプリでこのエラー コードを受け取った場合、次のようなことが考えられます。

* 最後にバッファーを参照したとき以降にバッファーが上書きされました。 トラッカーからの情報はすべて不完全なものになるため、最善策はライブラリを再クロールすることです。
* [**Reset**](/uwp/api/windows.storage.storagelibrarychangetracker.reset) を呼び出すまで、変更トラッカーからはそれ以上の変更は返されません。 アプリで Reset を呼び出すと、ポインターが最新の変更に移動し、追跡が正常に再開されます。

まれなケースですが、ユーザーがディスク上で大量の数のファイルを移動するシナリオにおいて、変更トラッカーが膨張して保存スペースを使い過ぎることは望ましくありません。 これにより、Windows の顧客エクスペリエンスを損なうことなく、アプリで大規模なファイル システム操作に対応できるようになります。

### <a name="changes-to-a-storagelibrary"></a>StorageLibrary に対する変更

[**StorageLibrary**](/uwp/api/windows.storage.storagelibrary) クラスは、他のフォルダーを含むルート フォルダーからなる仮想グループとして存在します。 これをファイル システム変更トラッカーと調整するために、Microsoft では次のように選択しました。

- ルート ライブラリ フォルダーの下位で発生した変更は、変更トラッカーに表示されます。 ルート ライブラリ フォルダーは、[**Folders**](/uwp/api/windows.storage.storagelibrary.folders) プロパティを使用して見つけることができます。
- **StorageLibrary** ([**RequestAddFolderAsync**](/uwp/api/windows.storage.storagelibrary.requestaddfolderasync) および [**RequestRemoveFolderAsync**](/uwp/api/windows.storage.storagelibrary.requestremovefolderasync) を介して) からルート フォルダーを追加または削除すると、変更トラッカーにエントリは作成されません。 これらの変更は、[**DefinitionChanged**](/uwp/api/windows.storage.storagelibrary.definitionchanged) イベントを介して、または[**Folders**](/uwp/api/windows.storage.storagelibrary.folders) プロパティを使用してライブラリ内のルート フォルダーを列挙することによって、追跡できます。
- 既にその中に内容があるフォルダーがライブラリに追加された場合、変更通知または変更トラッカー エントリは生成されません。 そのフォルダーの下位で発生するその後のすべての変更によって、通知が生成されトラッカー エントリが変更されます。

### <a name="calling-the-enable-method"></a>Enable メソッドを呼び出す

アプリでは、ファイル システムの追跡を開始したらすぐに、変更のあらゆる列挙の前に、[**Enable**](/uwp/api/windows.storage.storagelibrarychangetracker.enable) を呼び出す必要があります。 これにより、すべての変更が変更トラッカーによって確実にキャプチャされます。  

## <a name="putting-it-together"></a>まとめる

ビデオ ライブラリでの変更に登録し、変更トラッカーからの変更の取得を開始するために使用されるコード全体を次に示します。

```csharp
private async void EnableChangeTracker()
{
    StorageLibrary videosLib = await StorageLibrary.GetLibraryAsync(KnownLibraryId.Videos);
    StorageLibraryChangeTracker videoTracker = videosLib.ChangeTracker;
    videoTracker.Enable();
}

private async void GetChanges()
{
    StorageLibrary videosLibrary = await StorageLibrary.GetLibraryAsync(KnownLibraryId.Videos);
    videosLibrary.ChangeTracker.Enable();
    StorageLibraryChangeReader videoChangeReader = videosLibrary.ChangeTracker.GetChangeReader();
    IReadOnlyList changeSet = await changeReader.ReadBatchAsync();


    //Below this line is for the blog post. Above the line is for the magazine
    foreach (StorageLibraryChange change in changeSet)
    {
        if (change.ChangeType == StorageLibraryChangeType.ChangeTrackingLost)
        {
            //We are in trouble. Nothing else is going to be valid.
            log("Resetting the change tracker");
            videosLibrary.ChangeTracker.Reset();
            return;
        }
        if (change.IsOfType(StorageItemTypes.Folder))
        {
            await HandleFileChange(change);
        }
        else if (change.IsOfType(StorageItemTypes.File))
        {
            await HandleFolderChange(change);
        }
        else if (change.IsOfType(StorageItemTypes.None))
        {
            if (change.ChangeType == StorageLibraryChangeType.Deleted)
            {
                RemoveItemFromDB(change.Path);
            }
        }
    }
    await changeReader.AcceptChangesAsync();
}
```