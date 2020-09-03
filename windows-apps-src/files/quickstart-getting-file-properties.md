---
ms.assetid: AC96F645-1BDE-4316-85E0-2FBDE0A0A62A
title: ファイルのプロパティの取得
description: StorageFile\# オブジェクトで表されるファイルのプロパティ\# (最上位、基本、拡張) を取得します。
ms.date: 12/19/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1812908319bed81ab776e8e66753f4f881a8728f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156546"
---
# <a name="get-file-properties"></a>ファイルのプロパティの取得

**重要な API**

-   [**StorageFile.GetBasicPropertiesAsync**](/uwp/api/windows.storage.storagefile.getbasicpropertiesasync)
-   [**StorageFile.Properties**](/uwp/api/windows.storage.storagefile.properties)
-   [**StorageItemContentProperties.RetrievePropertiesAsync**](/uwp/api/windows.storage.fileproperties.storageitemcontentproperties.retrievepropertiesasync)

[  **StorageFile**](/uwp/api/Windows.Storage.StorageFile) オブジェクトで表されるファイルのプロパティ (最上位、基本、拡張) を取得します。

> [!NOTE]
> 完全なサンプルについては、[ファイル アクセスのサンプルに関するページ](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/FileAccess)をご覧ください。

## <a name="prerequisites"></a>前提条件

-   **ユニバーサル Windows プラットフォーム (UWP) アプリの非同期プログラミングについての理解**

    C# や Visual Basic での非同期アプリの作成方法については、「[C# または Visual Basic での非同期 API の呼び出し](../threading-async/call-asynchronous-apis-in-csharp-or-visual-basic.md)」をご覧ください。 C++ での非同期アプリの作成方法については、「[C++ での非同期プログラミング](../threading-async/asynchronous-programming-in-cpp-universal-windows-platform-apps.md)」をご覧ください。

-   **場所へのアクセス許可**

    たとえば、これらの例のコードでは **picturesLibrary** 機能が必要ですが、場所によっては別の機能が必要であったり、機能をまったく必要としない場合もあります。 詳しくは、「[ファイル アクセス許可](file-access-permissions.md)」をご覧ください。

## <a name="getting-a-files-top-level-properties"></a>ファイルの最上位プロパティの取得

多くの最上位ファイル プロパティは、[**StorageFile**](/uwp/api/Windows.Storage.StorageFile) クラスのメンバーとしてアクセスできます。 これらのプロパティには、ファイル属性、コンテンツの種類、作成日、表示名、ファイルの種類などがあります。

> [!NOTE]
> 必ず **picturesLibrary** 機能を宣言してください。

この例では、画像ライブラリ内のすべてのファイルを列挙して、各ファイルの最上位プロパティの一部にアクセスします。

```csharp
// Enumerate all files in the Pictures library.
var folder = Windows.Storage.KnownFolders.PicturesLibrary;
var query = folder.CreateFileQuery();
var files = await query.GetFilesAsync();

foreach (Windows.Storage.StorageFile file in files)
{
    StringBuilder fileProperties = new StringBuilder();

    // Get top-level file properties.
    fileProperties.AppendLine("File name: " + file.Name);
    fileProperties.AppendLine("File type: " + file.FileType);
}
```

## <a name="getting-a-files-basic-properties"></a>ファイルの基本プロパティの取得

多くの基本ファイル プロパティは、最初に [**StorageFile.GetBasicPropertiesAsync**](/uwp/api/windows.storage.storagefile.getbasicpropertiesasync) メソッドを呼び出して取得します。 このメソッドは、項目 (ファイルまたはフォルダー) のサイズや最終変更日のプロパティを定義する [**BasicProperties**](/uwp/api/Windows.Storage.FileProperties.BasicProperties) オブジェクトを返します。

この例では、画像ライブラリ内のすべてのファイルを列挙して、各ファイルの基本プロパティの一部にアクセスします。

```csharp
// Enumerate all files in the Pictures library.
var folder = Windows.Storage.KnownFolders.PicturesLibrary;
var query = folder.CreateFileQuery();
var files = await query.GetFilesAsync();

foreach (Windows.Storage.StorageFile file in files)
{
    StringBuilder fileProperties = new StringBuilder();

    // Get file's basic properties.
    Windows.Storage.FileProperties.BasicProperties basicProperties =
        await file.GetBasicPropertiesAsync();
    string fileSize = string.Format("{0:n0}", basicProperties.Size);
    fileProperties.AppendLine("File size: " + fileSize + " bytes");
    fileProperties.AppendLine("Date modified: " + basicProperties.DateModified);
}
 ```

## <a name="getting-a-files-extended-properties"></a>ファイルの拡張プロパティの取得

最上位と基本ファイル プロパティのほかに、ファイルの内容に関連付けられている多くのプロパティがあります。 これらの拡張プロパティは、[**BasicProperties.RetrievePropertiesAsync**](/uwp/api/windows.storage.fileproperties.basicproperties.retrievepropertiesasync) メソッドを呼び出してアクセスします  ([**BasicProperties**](/uwp/api/Windows.Storage.FileProperties.BasicProperties) オブジェクトは、[**StorageFile.Properties**](/uwp/api/windows.storage.storagefile.properties) プロパティを呼び出して取得します)。最上位と基本ファイル プロパティは、クラス (それぞれ [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) と **BasicProperties**) のプロパティとしてアクセスできます。拡張プロパティは、取得するプロパティの名前を表す [String](/dotnet/api/system.string) オブジェクトの [IEnumerable](/dotnet/api/system.collections.ienumerable) コレクションを **BasicProperties.RetrievePropertiesAsync** メソッドに渡して取得します。 このメソッドは、[IDictionary](/dotnet/api/system.collections.idictionary) コレクションを返します。 各拡張プロパティは、コレクションから名前またはインデックスで取得します。

この例では、画像ライブラリ内のすべてのファイルを列挙し、[List](/dotnet/api/system.collections.generic.list-1) オブジェクトで目的のプロパティの名前 (**DataAccessed** と **FileOwner**) を指定して、その [List](/dotnet/api/system.collections.generic.list-1) オブジェクトを [**BasicProperties.RetrievePropertiesAsync**](/uwp/api/windows.storage.fileproperties.basicproperties.retrievepropertiesasync) に渡すことでそれらのプロパティを取得します。その後、返された [IDictionary](/dotnet/api/system.collections.idictionary) オブジェクトから名前でそれらのプロパティを取得します。

ファイルの拡張プロパティの完全な一覧については、「[Windows コア プロパティ](/windows/desktop/properties/core-bumper)」をご覧ください。

```csharp
const string dateAccessedProperty = "System.DateAccessed";
const string fileOwnerProperty = "System.FileOwner";

// Enumerate all files in the Pictures library.
var folder = KnownFolders.PicturesLibrary;
var query = folder.CreateFileQuery();
var files = await query.GetFilesAsync();

foreach (Windows.Storage.StorageFile file in files)
{
    StringBuilder fileProperties = new StringBuilder();

    // Define property names to be retrieved.
    var propertyNames = new List<string>();
    propertyNames.Add(dateAccessedProperty);
    propertyNames.Add(fileOwnerProperty);

    // Get extended properties.
    IDictionary<string, object> extraProperties =
        await file.Properties.RetrievePropertiesAsync(propertyNames);

    // Get date-accessed property.
    var propValue = extraProperties[dateAccessedProperty];
    if (propValue != null)
    {
        fileProperties.AppendLine("Date accessed: " + propValue);
    }

    // Get file-owner property.
    propValue = extraProperties[fileOwnerProperty];
    if (propValue != null)
    {
        fileProperties.AppendLine("File owner: " + propValue);
    }
}
```

 

 