---
ms.assetid: 8BDDE64A-77D2-4F9D-A1A0-E4C634BCD890
title: ピッカーによるファイルの保存
description: FileSavePicker を使って、アプリで保存するファイルの名前とその保存場所をユーザーが指定できるようにします。
ms.date: 12/19/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e46393d303ac186a3a96c3dfaeb0eea335fbadad
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89168346"
---
# <a name="save-a-file-with-a-picker"></a>ピッカーによるファイルの保存

**重要な API**

-   [**FileSavePicker**](/uwp/api/Windows.Storage.Pickers.FileSavePicker)
-   [**StorageFile**](/uwp/api/Windows.Storage.StorageFile)

[  **FileSavePicker**](/uwp/api/Windows.Storage.Pickers.FileSavePicker) を使ってユーザーがアプリで保存するファイルの名前とその保存場所を指定できるようにします。

> [!NOTE]
> 完全なサンプルについては、[ファイル ピッカーのサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/FilePicker)に関するページをご覧ください。

 

## <a name="prerequisites"></a>必要条件


-   **ユニバーサル Windows プラットフォーム (UWP) アプリの非同期プログラミングについての理解**

    C# や Visual Basic での非同期アプリの作成方法については、「[C# または Visual Basic での非同期 API の呼び出し](../threading-async/call-asynchronous-apis-in-csharp-or-visual-basic.md)」をご覧ください。 C++ での非同期アプリの作成方法については、「[C++ での非同期プログラミング](../threading-async/asynchronous-programming-in-cpp-universal-windows-platform-apps.md)」をご覧ください。

-   **場所へのアクセス許可**

    「[ファイル アクセス許可](file-access-permissions.md)」をご覧ください。

## <a name="filesavepicker-step-by-step"></a>FileSavePicker: 手順

[  **FileSavePicker**](/uwp/api/Windows.Storage.Pickers.FileSavePicker) を使って、ユーザーが保存するファイルの名前、種類、場所を指定できるようにします。 ファイル ピッカー オブジェクトを作成、カスタマイズ、および表示し、選ばれたファイルを表す返された [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) オブジェクトを使ってデータを保存します。

1.  **FileSavePicker を作成してカスタマイズする**

    ```cs
    var savePicker = new Windows.Storage.Pickers.FileSavePicker();
    savePicker.SuggestedStartLocation =
        Windows.Storage.Pickers.PickerLocationId.DocumentsLibrary;
    // Dropdown of file types the user can save the file as
    savePicker.FileTypeChoices.Add("Plain Text", new List<string>() { ".txt" });
    // Default file name if the user does not type one in or select a file to replace
    savePicker.SuggestedFileName = "New Document";
    ```

ファイル ピッカー オブジェクトの、ユーザーとアプリに関連するプロパティを設定します。 この例では、3 つのプロパティ [**SuggestedStartLocation**](/uwp/api/windows.storage.pickers.filesavepicker.suggestedstartlocation)、[**FileTypeChoices**](/uwp/api/windows.storage.pickers.filesavepicker.filetypechoices)、および [**SuggestedFileName**](/uwp/api/windows.storage.pickers.filesavepicker.suggestedfilename) を設定します。
     
- このサンプルでは [**LocalFolder**](/uwp/api/windows.storage.applicationdata.localfolder) を使って、ドキュメントまたはテキスト ファイルを保存する場所として [**SuggestedStartLocation**](/uwp/api/windows.storage.pickers.filesavepicker.suggestedstartlocation) をアプリのローカル フォルダーに設定しています。 [  **SuggestedStartLocation**](/uwp/api/windows.storage.pickers.fileopenpicker.suggestedstartlocation) を保存するファイルの種類 (音楽、画像、ビデオ、ドキュメントなど) に適切な場所に設定します。 ユーザーは、開始場所から別の場所に移動できます。

- サンプルでは、保存したファイルを確実にアプリから開くことができるように、サポートするファイルの種類 (Microsoft Word 文書とテキスト ファイル) を [**FileTypeChoices**](/uwp/api/windows.storage.pickers.filesavepicker.filetypechoices) を使って指定しています。 指定したすべてのファイルの種類をアプリはサポートする必要があります。 ユーザーは、ファイルの種類を指定して保存できます。 また、別のファイルの種類を選んで、ファイルの種類を変更することもできます。 既定では、リストの先頭にあるファイルの種類が選択されます。これを制御するには、[**DefaultFileExtension**](/uwp/api/windows.storage.pickers.filesavepicker.defaultfileextension) プロパティを設定します。

    > [!NOTE]
    > また、ファイル ピッカーでは、現在選ばれているファイルの種類を使って表示されるファイルがフィルター処理され、選ばれているファイルの種類に一致するファイルの種類だけがユーザーに表示されます。

- ユーザーの入力の手間を多少なりとも軽減するために、この例では [**SuggestedFileName**](/uwp/api/windows.storage.pickers.filesavepicker.suggestedfilename) を設定しています。 提示するファイル名は、ユーザーが保存するファイルにできる限り関係のあるものにします。 たとえば、Word のように、ファイルが既にある場合はその名前を提示し、まだ名前のないファイルを保存している場合はドキュメントの 1 行目を提示します。

> [!NOTE]
> [**FileSavePicker**](/uwp/api/Windows.Storage.Pickers.FileSavePicker) オブジェクトでは、[**PickerViewMode.List**](/uwp/api/Windows.Storage.Pickers.PickerViewMode) 表示モードを使ってファイル ピッカーが表示されます。

2.  **FileSavePicker を表示して選ばれたファイルに保存する**

    ファイル ピッカーを表示するには、[**PickSaveFileAsync**](/uwp/api/windows.storage.pickers.filesavepicker.picksavefileasync) メソッドを呼び出します。 ユーザーが名前、ファイルの種類、場所を指定し、ファイルの保存を確定すると、**PickSaveFileAsync** は保存するファイルを表す [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) オブジェクトを返します。 これでファイルの読み取りおよび書き込みアクセスが付与されるため、このファイルをキャプチャして処理できます。

    ```cs
    Windows.Storage.StorageFile file = await savePicker.PickSaveFileAsync();
        if (file != null)
        {
            // Prevent updates to the remote version of the file until
            // we finish making changes and call CompleteUpdatesAsync.
            Windows.Storage.CachedFileManager.DeferUpdates(file);
            // write to file
            await Windows.Storage.FileIO.WriteTextAsync(file, file.Name);
            // Let Windows know that we're finished changing the file so
            // the other app can update the remote version of the file.
            // Completing updates may require Windows to ask for user input.
            Windows.Storage.Provider.FileUpdateStatus status =
                await Windows.Storage.CachedFileManager.CompleteUpdatesAsync(file);
            if (status == Windows.Storage.Provider.FileUpdateStatus.Complete)
            {
                this.textBlock.Text = "File " + file.Name + " was saved.";
            }
            else
            {
                this.textBlock.Text = "File " + file.Name + " couldn't be saved.";
            }
        }
        else
        {
            this.textBlock.Text = "Operation cancelled.";
        }
    ```

この例では、ファイルが有効であることをチェックし、ファイルにそのファイル名を書き込みます。 また、「[ファイルの作成、書き込み、および読み取り](quickstart-reading-and-writing-files.md)」もご覧ください。

> [!TIP]
> その他の処理を実行する前に、必ず保存されたファイルをチェックしてファイルの有効性を確認する必要があります。 その後、アプリに適したコンテンツをファイルに保存できるほか、選ばれたファイルが有効でない場合は適切な動作を実行できます。