---
title: ファイルに応じた既定のアプリの起動
description: Windows.System の使用方法について説明します。ランチャー API を使用して、アプリ自体を処理できないファイルの既定のハンドラーを起動します。
ms.assetid: BB45FCAF-DF93-4C99-A8B5-59B799C7BD98
ms.date: 07/05/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 34aee5b2e2f04b7e5d72a7bc31d2cfafc1bcb6fb
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167846"
---
# <a name="launch-the-default-app-for-a-file"></a>ファイルに応じた既定のアプリの起動

**重要な API**

-   [**Windows.System.Launcher.LaunchFileAsync**](/uwp/api/windows.system.launcher.launchfileasync)

ファイルに応じて既定のアプリを起動する方法について説明します。 多くのアプリでは、アプリ自体で処理できないファイルを操作する必要が生じる場合があります。 たとえば、さまざまな種類のファイルを受け取るメール アプリは、これらのファイルを既定のハンドラーで起動する手段を備えている必要があります。 以下の手順では、[**Windows.System.Launcher**](/uwp/api/Windows.System.Launcher) API を使って、アプリがそれ自体で処理できないファイルの既定のハンドラーを起動する方法を示します。

## <a name="get-the-file-object"></a>ファイル オブジェクトを取得する

最初にファイルの [**Windows.Storage.StorageFile**](/uwp/api/Windows.Storage.StorageFile) オブジェクトを取得します。

ファイルがアプリのパッケージに含まれている場合は、[**Package.InstalledLocation**](/uwp/api/windows.applicationmodel.package.installedlocation) プロパティで [**Windows.Storage.StorageFolder**](/uwp/api/Windows.Storage.StorageFolder) オブジェクトを取得し、[**Windows.Storage.StorageFolder.GetFileAsync**](/uwp/api/windows.storage.storagefolder.getfileasync) メソッドで [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) オブジェクトを取得します。

ファイルが既知のフォルダー内にある場合には、[**Windows.Storage.KnownFolders**](/uwp/api/Windows.Storage.KnownFolders) クラスのプロパティで [**StorageFolder**](/uwp/api/Windows.Storage.StorageFolder) を取得し、[**GetFileAsync**](/uwp/api/windows.storage.storagefolder.getfileasync) メソッドで [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) オブジェクトを取得します。

## <a name="launch-the-file"></a>ファイルを起動する

Windows には、ファイルの既定のハンドラーを起動するためのいくつかの異なるオプションが用意されています。 これらのオプションについて、次の表とセクションで説明します。

| オプション | Method | 説明 |
|--------|--------|-------------|
| 既定の起動 | [**LaunchFileAsync(IStorageFile)**](/uwp/api/windows.system.launcher.launchfileasync) | 指定されたファイルを既定のハンドラーで起動します。 |
| [プログラムから開く] を使った起動 | [**LaunchFileAsync(IStorageFile, LauncherOptions)**](/uwp/api/windows.system.launcher.launchfileasync#Windows_System_Launcher_LaunchFileAsync_Windows_Storage_IStorageFile_Windows_System_LauncherOptions_) | 指定されたファイルを [プログラムから開く] ダイアログでユーザーによって選択されたハンドラーを使って起動します。 |
| 推奨されるアプリ フォールバックを使った起動 | [**LaunchFileAsync(IStorageFile, LauncherOptions)**](/uwp/api/windows.system.launcher.launchfileasync#Windows_System_Launcher_LaunchFileAsync_Windows_Storage_IStorageFile_Windows_System_LauncherOptions_) | 指定されたファイルを既定のハンドラーで起動します。 ハンドラーがシステムにインストールされていない場合は、ストアにあるアプリをユーザーに勧めます。 |
| 画面上に留まった適切なビューを使った起動 | [**LaunchFileAsync(IStorageFile, LauncherOptions)**](/uwp/api/windows.system.launcher.launchfileasync#Windows_System_Launcher_LaunchFileAsync_Windows_Storage_IStorageFile_Windows_System_LauncherOptions_) (Windows のみ) | 指定されたファイルを既定のハンドラーで起動します。 起動後も画面上に留まるように指定し、特定のウィンドウ サイズを要求します。 [**LauncherOptions.DesiredRemainingView**](/uwp/api/windows.system.launcheroptions.desiredremainingview) はモバイル デバイス ファミリではサポートされていません。 |

### <a name="default-launch"></a>既定の起動

既定のアプリを起動するには、[**Windows.System.Launcher.LaunchFileAsync(IStorageFile)**](/uwp/api/windows.system.launcher.launchfileasync) メソッドを呼び出します。 この例では [**Windows.Storage.StorageFolder.GetFileAsync**](/uwp/api/windows.storage.storagefolder.getfileasync) メソッドを使って、このアプリ パッケージに含まれる画像ファイル test.png を起動します。

```csharp
async void DefaultLaunch()
{
   // Path to the file in the app package to launch
   string imageFile = @"images\test.png";
   
   var file = await Windows.ApplicationModel.Package.Current.InstalledLocation.GetFileAsync(imageFile);
   
   if (file != null)
   {
      // Launch the retrieved file
      var success = await Windows.System.Launcher.LaunchFileAsync(file);

      if (success)
      {
         // File launched
      }
      else
      {
         // File launch failed
      }
   }
   else
   {
      // Could not find file
   }
}
```

```vb
async Sub DefaultLaunch()
   ' Path to the file in the app package to launch
   Dim imageFile = "images\test.png"
   Dim file = await Windows.ApplicationModel.Package.Current.InstalledLocation.GetFileAsync(imageFile)
   
   If file IsNot Nothing Then
      ' Launch the retrieved file
      Dim success = await Windows.System.Launcher.LaunchFileAsync(file)

      If success Then
         ' File launched
      Else
         ' File launch failed
      End If
   Else
      ' Could not find file
   End If
End Sub
```

```cppwinrt
Windows::Foundation::IAsyncAction MainPage::DefaultLaunch()
{
    auto installFolder{ Windows::ApplicationModel::Package::Current().InstalledLocation() };

    Windows::Storage::StorageFile file{ co_await installFolder.GetFileAsync(L"images\\test.png") };

    if (file)
    {
        // Launch the retrieved file
        bool success = co_await Windows::System::Launcher::LaunchFileAsync(file);
        if (success)
        {
            // File launched
        }
        else
        {
            // File launch failed
        }
    }
    else
    {
        // Could not find file
    }
}
```

```cpp
void MainPage::DefaultLaunch()
{
   auto installFolder = Windows::ApplicationModel::Package::Current->InstalledLocation;

   concurrency::task<Windows::Storage::StorageFile^getFileOperation(installFolder->GetFileAsync("images\\test.png"));
   getFileOperation.then([](Windows::Storage::StorageFile^ file)
   {
      if (file != nullptr)
      {
         // Launch the retrieved file
         concurrency::task<bool> launchFileOperation(Windows::System::Launcher::LaunchFileAsync(file));
         launchFileOperation.then([](bool success)
         {
            if (success)
            {
               // File launched
            }
            else
            {
               // File launch failed
            }
         });
      }
      else
      {
         // Could not find file
      }
   });
}
```

### <a name="open-with-launch"></a>[プログラムから開く] を使った起動

[**LauncherOptions.DisplayApplicationPicker**](/uwp/api/windows.system.launcheroptions.displayapplicationpicker) を **true** に設定して [**Windows.System.Launcher.LaunchFileAsync(IStorageFile, LauncherOptions)**](/uwp/api/windows.system.launcher.launchfileasync#Windows_System_Launcher_LaunchFileAsync_Windows_Storage_IStorageFile_Windows_System_LauncherOptions_) メソッドを呼び出して、ユーザーが **[プログラムから開く]** ダイアログ ボックスで選んだアプリを起動します。

ユーザーが特定のファイルに既定以外のアプリを選ぶ場合は、**[プログラムから開く]** ダイアログ ボックスを使うことをお勧めします。 たとえば、アプリで画像ファイルを起動できる場合、既定のハンドラーは多くの場合ビューアー アプリです。 場合によっては、ユーザーが画像の表示ではなく編集を行うこともあります。 **[プログラムから開く]** オプションを **AppBar** またはコンテキスト メニューで代替コマンドと共に使って、ユーザーが **[プログラムから開く]** ダイアログを開き、このようなシナリオでエディター アプリを選択できるようにします。

![.png ファイルの起動のための [プログラムから開く] ダイアログ。 このダイアログには、ユーザーによって選択されたアプリをすべての .png ファイルに使用するかまたはこの 1 つの .png ファイルのみに使用するかを指定するチェック ボックスがあります。 また、このダイアログには、ファイルの起動に関する 4 つのアプリ オプションと、[その他のオプション] リンクがあります。](images/checkboxopenwithdialog.png)

```csharp
async void DefaultLaunch()
{
   // Path to the file in the app package to launch
      string imageFile = @"images\test.png";
      
   var file = await Windows.ApplicationModel.Package.Current.InstalledLocation.GetFileAsync(imageFile);

   if (file != null)
   {
      // Set the option to show the picker
      var options = new Windows.System.LauncherOptions();
      options.DisplayApplicationPicker = true;

      // Launch the retrieved file
      bool success = await Windows.System.Launcher.LaunchFileAsync(file, options);
      if (success)
      {
         // File launched
      }
      else
      {
         // File launch failed
      }
   }
   else
   {
      // Could not find file
   }
}
```

```vb
async Sub DefaultLaunch()

   ' Path to the file in the app package to launch
   Dim imageFile = "images\test.png"

   Dim file = await Windows.ApplicationModel.Package.Current.InstalledLocation.GetFileAsync(imageFile)

   If file IsNot Nothing Then
      ' Set the option to show the picker
      Dim options = Windows.System.LauncherOptions()
      options.DisplayApplicationPicker = True

      ' Launch the retrieved file
      Dim success = await Windows.System.Launcher.LaunchFileAsync(file)

      If success Then
         ' File launched
      Else
         ' File launch failed
      End If
   Else
      ' Could not find file
   End If
End Sub
```

```cppwinrt
Windows::Foundation::IAsyncAction MainPage::DefaultLaunch()
{
    auto installFolder{ Windows::ApplicationModel::Package::Current().InstalledLocation() };

    Windows::Storage::StorageFile file{ co_await installFolder.GetFileAsync(L"images\\test.png") };

    if (file)
    {
        // Set the option to show the picker
        Windows::System::LauncherOptions launchOptions;
        launchOptions.DisplayApplicationPicker(true);

        // Launch the retrieved file
        bool success = co_await Windows::System::Launcher::LaunchFileAsync(file, launchOptions);
        if (success)
        {
            // File launched
        }
        else
        {
            // File launch failed
        }
    }
    else
    {
        // Could not find file
    }
}
```

```cpp
void MainPage::DefaultLaunch()
{
   auto installFolder = Windows::ApplicationModel::Package::Current->InstalledLocation;

   concurrency::task<Windows::Storage::StorageFile^> getFileOperation(installFolder->GetFileAsync("images\\test.png"));
   getFileOperation.then([](Windows::Storage::StorageFile^ file)
   {
      if (file != nullptr)
      {
         // Set the option to show the picker
         auto launchOptions = ref new Windows::System::LauncherOptions();
         launchOptions->DisplayApplicationPicker = true;

         // Launch the retrieved file
         concurrency::task<bool> launchFileOperation(Windows::System::Launcher::LaunchFileAsync(file, launchOptions));
         launchFileOperation.then([](bool success)
         {
            if (success)
            {
               // File launched
            }
            else
            {
               // File launch failed
            }
         });
      }
      else
      {
         // Could not find file
      }
   });
}
```

**推奨されるアプリ フォールバックを使った起動**

場合によっては、起動中のファイルを処理するためのアプリがインストールされていないこともあります。 既定では、Windows はストア上の適切なアプリを検索するリンクを表示して、これらのケースに対処します。 このシナリオで入手するアプリに関する特定の推奨事項を示す場合は、起動中のファイルと共に推奨事項を渡すことができます。 そのためには、[**LauncherOptions.PreferredApplicationPackageFamilyName**](/uwp/api/windows.system.launcheroptions.preferredapplicationpackagefamilyname) を推奨するストア内のアプリのパッケージのファミリ名に設定して、[**Windows.System.Launcher.launchFileAsync(IStorageFile, LauncherOptions)**](/uwp/api/windows.system.launcher.launchfileasync#Windows_System_Launcher_LaunchFileAsync_Windows_Storage_IStorageFile_Windows_System_LauncherOptions_) メソッドを呼び出します。 その後、[**LauncherOptions.PreferredApplicationDisplayName**](/uwp/api/windows.system.launcheroptions.preferredapplicationdisplayname) をそのアプリの名前に設定します。 Windows ではこの情報を使って、ストア内のアプリを検索する一般的なオプションを、ストアから推奨アプリを入手する固有のオプションに置き換えます。

> [!NOTE]
> アプリを推奨するには、これらのオプションの両方を設定する必要があります。 どちらか一方のみを設定した場合は、エラーになります。

![.contoso ファイルの起動のための [プログラムから開く] ダイアログ。 コンピューターには .contoso に対応するハンドラーがインストールされていないため、このダイアログには、ストア アイコンとストア上の適切なハンドラーをユーザーに通知するテキストを含むオプションが表示されます。 このダイアログには、[その他のオプション] リンクもあります。](images/howdoyouwanttoopen.png)

```csharp
async void DefaultLaunch()
{
   // Path to the file in the app package to launch
   string imageFile = @"images\test.contoso";

   // Get the image file from the package's image directory
   var file = await Windows.ApplicationModel.Package.Current.InstalledLocation.GetFileAsync(imageFile);

   if (file != null)
   {
      // Set the recommended app
      var options = new Windows.System.LauncherOptions();
      options.PreferredApplicationPackageFamilyName = "Contoso.FileApp_8wknc82po1e";
      options.PreferredApplicationDisplayName = "Contoso File App";

      // Launch the retrieved file pass in the recommended app
      // in case the user has no apps installed to handle the file
      bool success = await Windows.System.Launcher.LaunchFileAsync(file, options);
      if (success)
      {
         // File launched
      }
      else
      {
         // File launch failed
      }
   }
   else
   {
      // Could not find file
   }
}
```

```vb
async Sub DefaultLaunch()

   ' Path to the file in the app package to launch
   Dim imageFile = "images\test.contoso"

   ' Get the image file from the package's image directory
   Dim file = await Windows.ApplicationModel.Package.Current.InstalledLocation.GetFileAsync(imageFile)

   If file IsNot Nothing Then
      ' Set the recommended app
      Dim options = Windows.System.LauncherOptions()
      options.PreferredApplicationPackageFamilyName = "Contoso.FileApp_8wknc82po1e";
      options.PreferredApplicationDisplayName = "Contoso File App";

      ' Launch the retrieved file pass in the recommended app
      ' in case the user has no apps installed to handle the file
      Dim success = await Windows.System.Launcher.LaunchFileAsync(file)

      If success Then
         ' File launched
      Else
         ' File launch failed
      End If
   Else
      ' Could not find file
   End If
End Sub
```

```cppwinrt
Windows::Foundation::IAsyncAction MainPage::DefaultLaunch()
{
    auto installFolder{ Windows::ApplicationModel::Package::Current().InstalledLocation() };

    Windows::Storage::StorageFile file{ co_await installFolder.GetFileAsync(L"images\\test.png") };

    if (file)
    {
        // Set the recommended app
        Windows::System::LauncherOptions launchOptions;
        launchOptions.PreferredApplicationPackageFamilyName(L"Contoso.FileApp_8wknc82po1e");
        launchOptions.PreferredApplicationDisplayName(L"Contoso File App");

        // Launch the retrieved file, and pass in the recommended app
        // in case the user has no apps installed to handle the file.
        bool success = co_await Windows::System::Launcher::LaunchFileAsync(file, launchOptions);
        if (success)
        {
            // File launched
        }
        else
        {
            // File launch failed
        }
    }
    else
    {
        // Could not find file
    }
}
```

```cpp
void MainPage::DefaultLaunch()
{
   auto installFolder = Windows::ApplicationModel::Package::Current->InstalledLocation;

   concurrency::task<Windows::Storage::StorageFile^> getFileOperation(installFolder->GetFileAsync("images\\test.contoso"));
   getFileOperation.then([](Windows::Storage::StorageFile^ file)
   {
      if (file != nullptr)
      {
         // Set the recommended app
         auto launchOptions = ref new Windows::System::LauncherOptions();
         launchOptions->PreferredApplicationPackageFamilyName = "Contoso.FileApp_8wknc82po1e";
         launchOptions->PreferredApplicationDisplayName = "Contoso File App";
         
         // Launch the retrieved file pass, and in the recommended app
         // in case the user has no apps installed to handle the file.
         concurrency::task<bool> launchFileOperation(Windows::System::Launcher::LaunchFileAsync(file, launchOptions));
         launchFileOperation.then([](bool success)
         {
            if (success)
            {
               // File launched
            }
            else
            {
               // File launch failed
            }
         });
      }
      else
      {
         // Could not find file
      }
   });
}
```

### <a name="launch-with-a-desired-remaining-view-windows-only"></a>画面上に留まった適切なビューを使った起動 (Windows のみ)

[**LaunchFileAsync**](/uwp/api/windows.system.launcher.launchfileasync) を呼び出すソース アプリは、ファイルの起動後も画面上に留まることを要求できます。 既定では、利用可能なスペース全体がソース アプリとファイルを処理するターゲット アプリとで均等に共有されます。 ソース アプリでは、[**DesiredRemainingView**](/uwp/api/windows.system.launcheroptions.desiredremainingview) プロパティを使って、利用可能なスペースをソース アプリのウィンドウがどの程度占めるかをオペレーティング システムに指示できます。 この **DesiredRemainingView** では、ファイルの起動後にソース アプリが画面上に留まる必要がなく、ターゲット アプリに完全に置き換わってもよいことも示せます。 このプロパティは呼び出し元アプリの優先ウィンドウのサイズだけを指定します。 画面に同時に表示されている可能性のある他のアプリの動作は指定しません。

> [!NOTE]
> ソースアプリの最終的なウィンドウサイズ (ソースアプリの優先度、画面上のアプリの数、画面の向きなど) を決定するときに、Windows では複数の異なる要因が考慮されます。 [**DesiredRemainingView**](/uwp/api/windows.system.launcheroptions.desiredremainingview) を設定しても、ソース アプリの特定のウィンドウ動作が保証されるわけではありません。

**モバイルデバイスファミリ:  **[**DesiredRemainingView**](/uwp/api/windows.system.launcheroptions.desiredremainingview) は、モバイルデバイスファミリではサポートされていません。

```csharp
async void DefaultLaunch()
{
   // Path to the file in the app package to launch
   string imageFile = @"images\test.png";
   
   var file = await Windows.ApplicationModel.Package.Current.InstalledLocation.GetFileAsync(imageFile);

   if (file != null)
   {
      // Set the desired remaining view
      var options = new Windows.System.LauncherOptions();
      options.DesiredRemainingView = Windows.UI.ViewManagement.ViewSizePreference.UseLess;

      // Launch the retrieved file
      bool success = await Windows.System.Launcher.LaunchFileAsync(file, options);
      if (success)
      {
         // File launched
      }
      else
      {
         // File launch failed
      }
   }
   else
   {
      // Could not find file
   }
}
```

```cppwinrt
Windows::Foundation::IAsyncAction MainPage::DefaultLaunch()
{
    auto installFolder{ Windows::ApplicationModel::Package::Current().InstalledLocation() };

    Windows::Storage::StorageFile file{ co_await installFolder.GetFileAsync(L"images\\test.png") };

    if (file)
    {
        // Set the desired remaining view.
        Windows::System::LauncherOptions launchOptions;
        launchOptions.DesiredRemainingView(Windows::UI::ViewManagement::ViewSizePreference::UseLess);

        // Launch the retrieved file.
        bool success = co_await Windows::System::Launcher::LaunchFileAsync(file, launchOptions);
        if (success)
        {
            // File launched
        }
        else
        {
            // File launch failed
        }
    }
    else
    {
        // Could not find file
    }
}
```

```cpp
void MainPage::DefaultLaunch()
{
   auto installFolder = Windows::ApplicationModel::Package::Current->InstalledLocation;

   concurrency::task<Windows::Storage::StorageFile^> getFileOperation(installFolder->GetFileAsync("images\\test.png"));
   getFileOperation.then([](Windows::Storage::StorageFile^ file)
   {
      if (file != nullptr)
      {
         // Set the desired remaining view.
         auto launchOptions = ref new Windows::System::LauncherOptions();
         launchOptions->DesiredRemainingView = Windows::UI::ViewManagement::ViewSizePreference::UseLess;

         // Launch the retrieved file.
         concurrency::task<bool> launchFileOperation(Windows::System::Launcher::LaunchFileAsync(file, launchOptions));
         launchFileOperation.then([](bool success)
         {
            if (success)
            {
               // File launched
            }
            else
            {
               // File launch failed
            }
         });
      }
      else
      {
         // Could not find file
      }
   });
}
```

## <a name="remarks"></a>注釈

起動するアプリをアプリが選ぶことはできません。 どのアプリを起動するかはユーザーが決めます。 ユーザーは、ユニバーサル Windows プラットフォーム (UWP) アプリまたは Windows デスクトップ アプリを選択できます。

ファイルの起動時、アプリはユーザーに表示されるフォアグラウンド アプリである必要があります。 この要件は、ユーザーが制御を維持するのに役立ちます。 この要件を満たすために、すべてのファイル起動がアプリの UI に直接結び付けられていることを確認します。 ほとんどの場合、ファイル起動を開始するには、常にユーザーがなんらかの操作を行う必要があります。

.exe、.msi、.js ファイルなど、オペレーティング システムによって自動的に実行されるコードまたはスクリプトを含むファイルの種類を起動することはできません。 この制約により、オペレーティング システムを変更する可能性のある、悪意のあるファイルからユーザーを保護できます。 この方法では、.docx ファイルなど、スクリプトを分離するアプリによって実行されるスクリプトを含むファイルの種類を起動できます。 Microsoft Word などのアプリは、.docx ファイルのスクリプトがオペレーティング システムを変更しないようにします。

制限されている種類のファイルを起動しようとすると、起動は失敗し、エラー コールバックが呼び出されます。 アプリがさまざまな種類のファイルを処理するため、このエラーの発生が予想される場合は、ユーザーにフォールバックを提供することをお勧めします。 たとえば、ファイルをデスクトップに保存してそこで開けるようなオプションをユーザーに提供することができます。

## <a name="related-topics"></a>関連トピック

### <a name="tasks"></a>タスク

* [URI に応じた既定のアプリの起動](launch-default-app.md)
* [ファイルのアクティブ化の処理](handle-file-activation.md)

### <a name="guidelines"></a>ガイドライン

* [ファイルの種類と URI のガイドライン](../files/index.md)

### <a name="reference"></a>参照先

* [**Windows.Storage.StorageFile**](/uwp/api/Windows.Storage.StorageFile)
* [**Windows.System.Launcher.LaunchFileAsync**](/uwp/api/windows.system.launcher.launchfileasync)