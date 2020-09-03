---
ms.assetid: 3604524F-112A-474F-B0CA-0726DC8DB885
title: Microsoft OneDrive ファイルが利用可能かどうかの確認
description: StorageFile.IsAvailable プロパティを使って、Microsoft OneDrive ファイルが利用可能かどうかを確認します。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 8eda7226547706cb7a8a4ef69d04407d749a0c86
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89159446"
---
# <a name="determining-availability-of-microsoft-onedrive-files"></a>Microsoft OneDrive ファイルが利用可能かどうかの確認


**重要な API**

-   [**FileIO クラス**](/uwp/api/Windows.Storage.FileIO)
-   [**StorageFile クラス**](/uwp/api/Windows.Storage.StorageFile)
-   [**StorageFile.IsAvailable プロパティ**](/uwp/api/windows.storage.storagefile.isavailable)

[  **StorageFile.IsAvailable**](/uwp/api/windows.storage.storagefile.isavailable) プロパティを使って、Microsoft OneDrive ファイルが利用可能かどうかを確認します。

## <a name="prerequisites"></a>必要条件

-   **ユニバーサル Windows プラットフォーム (UWP) アプリの非同期プログラミングについての理解**

    C# や Visual Basic での非同期アプリの作成方法については、「[C# または Visual Basic での非同期 API の呼び出し](../threading-async/call-asynchronous-apis-in-csharp-or-visual-basic.md)」をご覧ください。 C++ での非同期アプリの作成方法については、「[C++ での非同期プログラミング](../threading-async/asynchronous-programming-in-cpp-universal-windows-platform-apps.md)」をご覧ください。

-   **アプリ機能の宣言**

    「[ファイル アクセス許可](file-access-permissions.md)」をご覧ください。

## <a name="using-the-storagefileisavailable-property"></a>StorageFile.IsAvailable プロパティの使用

ユーザーは、OneDrive ファイルを "オフラインで利用可能" (既定) または "オンラインのみ" とマークできます。 この機能を使うと、大容量のファイル (写真やビデオなど) を自分の OneDrive に移動し、オンラインのみとマークすることで、ディスク領域を節約できます (ローカルに保存されるのはメタデータ ファイルのみです)。

[**StorageFile.IsAvailable**](/uwp/api/windows.storage.storagefile.isavailable) は、ファイルが現在利用可能であるかどうかを判別するために使われます。 次の表に、さまざまなシナリオでの **StorageFile.IsAvailable** プロパティの値を示します。

| ファイルの種類                              | オンライン | 従量制課金接続        | オフライン |
|-------------------------------------------|--------|------------------------|---------|
| ローカル ファイル                                | True   | True                   | True    |
| オフラインで利用可能とマークされている OneDrive ファイル | True   | True                   | True    |
| オンラインのみとマークされている OneDrive ファイル       | True   | ユーザー設定に基づく | False   |
| ネットワーク ファイル                              | True   | ユーザー設定に基づく | False   |

 

次の手順では、ファイルが現在利用できるかどうかを判別する方法を示しています。

1.  アクセスするライブラリに適した機能を宣言します。
2.  [  **Windows.Storage**](/uwp/api/Windows.Storage) 名前空間を含めます。 この名前空間には、ファイル、フォルダー、アプリケーション設定を管理するための型が含まれています。 また、必要な [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) 型も含まれています。
3.  必要なファイルの [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) オブジェクトを取得します。 ライブラリを列挙する場合、通常、この手順は [**StorageFolder.CreateFileQuery**](/uwp/api/windows.storage.storagefolder.createfilequery) メソッドを呼び出し、結果の [**StorageFileQueryResult**](/uwp/api/Windows.Storage.Search.StorageFileQueryResult) オブジェクトの [**GetFilesAsync**](/uwp/api/windows.storage.storagefolder.getfilesasync) メソッドを呼び出して行います。 **GetFilesAsync** メソッドは、**StorageFile** オブジェクトの [IReadOnlyList](/dotnet/api/system.collections.generic.ireadonlylist-1) コレクションを返します。
4.  目的のファイルを表す [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) オブジェクトにアクセスできるようになると、[**StorageFile.IsAvailable**](/uwp/api/windows.storage.storagefile.isavailable) プロパティの値は、ファイルが利用できるかどうかを表します。

次の汎用的なメソッドは、フォルダーを列挙し、そのフォルダーの [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) オブジェクトのコレクションを返す方法を示しています。 その後、呼び出し元メソッドで、各ファイルの [**StorageFile.IsAvailable**](/uwp/api/windows.storage.storagefile.isavailable) プロパティを参照する返されたコレクションを反復処理します。

```cs
/// <summary>
/// Generic function that retrieves all files from the specified folder.
/// </summary>
/// <param name="folder">The folder to be searched.</param>
/// <returns>An IReadOnlyList collection containing the file objects.</returns>
async Task<System.Collections.Generic.IReadOnlyList<StorageFile>> GetLibraryFilesAsync(StorageFolder folder)
{
    var query = folder.CreateFileQuery();
    return await query.GetFilesAsync();
}

private async void CheckAvailabilityOfFilesInPicturesLibrary()
{
    // Determine availability of all files within Pictures library.
    var files = await GetLibraryFilesAsync(KnownFolders.PicturesLibrary);
    for (int i = 0; i < files.Count; i++)
    {
        StorageFile file = files[i];

        StringBuilder fileInfo = new StringBuilder();
        fileInfo.AppendFormat("{0} (on {1}) is {2}",
                    file.Name,
                    file.Provider.DisplayName,
                    file.IsAvailable ? "available" : "not available");
    }
}
```