---
title: ユニバーサル Windows プラットフォームを使用してコンソール アプリを作成する
description: このトピックでは、コンソールウィンドウで実行される UWP アプリを記述する方法について説明します。
keywords: console uwp
ms.date: 08/02/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: cab52a84e631404fc4cbd682d6cedef7e799d387
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162666"
---
# <a name="create-a-universal-windows-platform-console-app"></a>ユニバーサル Windows プラットフォームを使用してコンソール アプリを作成する

このトピックでは、 [c++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) または c++/cx ユニバーサル WINDOWS プラットフォーム (UWP) コンソールアプリを作成する方法について説明します。

Windows 10 バージョン1803以降では、コンソールウィンドウ (DOS、PowerShell コンソールウィンドウなど) で実行される C++/WinRT または C++/CX の UWP コンソールアプリを作成できます。 コンソールアプリは、入力と出力にコンソールウィンドウを使用し、 **printf**や**Getchar**などの[ユニバーサル C ランタイム](/cpp/c-runtime-library/reference/crt-alphabetical-function-reference)関数を使用できます。 Microsoft Store には、UWP コンソール アプリを公開することができます。 それらのアプリは、アプリのリストにエントリがあり、スタート メニューに固定することができるプライマリ タイルがあります。 UWP コンソールアプリは、[スタート] メニューから起動できますが、通常はコマンドラインから起動します。

1つの動作を確認するために、UWP コンソールアプリの作成に関するビデオをご覧ください。

> [!VIDEO https://www.youtube.com/embed/bwvfrguY20s]

## <a name="use-a-uwp-console-app-template"></a>UWP コンソール アプリ テンプレートを使用する 

UWP コンソール アプリを作成するには、まず [Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=AndrewWhitechapelMSFT.ConsoleAppUniversal) から入手できる**コンソール アプリ (ユニバーサル) プロジェクト テンプレート**をインストールします。 インストールされたテンプレートは、[**新しいプロジェクト**] [  >  **インストールされている**  >  **他の言語] の**下で利用可能に  >  **Visual C++**  >  なります。**windows ユニバーサル**は、**コンソールアプリ c++/WinRT (ユニバーサル windows)** と**コンソールアプリ c++/cx (ユニバーサル windows**) Visual C++ ます。

## <a name="add-your-code-to-main"></a>Main() にコードを追加します。

テンプレートは **Program.cpp** を追加します。これには `main()` 関数が含まれています。 これは、UWP コンソール アプリで実行が開始される場所です。 `__argc` および `__argv` パラメーターでコマンドライン引数にアクセスします。 制御が `main()` から返ってくると、UWP コンソール アプリは終了します。

次の **プログラム .cpp** の例は、 **コンソールアプリ C++/WinRT** テンプレートによって追加されています。

```cppwinrt
#include "pch.h"

using namespace winrt;

// This example code shows how you could implement the required main function
// for a Console UWP Application. You can replace all the code inside main
// with your own custom code.

int __cdecl main()
{
    // You can get parsed command-line arguments from the CRT globals.
    wprintf(L"Parsed command-line arguments:\n");
    for (int i = 0; i < __argc; i++)
    {
        wprintf(L"__argv[%d] = %S\n", i, __argv[i]);
    }

    // Keep the console window alive in case you want to see console output when running from within Visual Studio
      wprintf(L"Press 'Enter' to continue: ");
    getchar();
}
```

## <a name="uwp-console-app-behavior"></a>UWP コンソール アプリの動作

UWP コンソール アプリは、実行されているディレクトリ、およびその下のファイル システムにアクセスできます。 テンプレートは [AppExecutionAlias](/uwp/schemas/appxpackage/uapmanifestschema/element-uap5-appexecutionalias) 拡張機能をアプリの Package.appxmanifest ファイルに追加するため、これが可能になります。 この拡張機能を使用すると、コンソール ウィンドウからエイリアスを入力してアプリを起動することもできます。 アプリを起動するためにシステムパスに入る必要はありません。

さらに、[ファイル アクセス許可](../files/file-access-permissions.md) で説明されているように、制限された機能 `broadFileSystemAccess` を追加することで、ファイルシステムへの広範なアクセスを UWP コンソール アプリに付与することができます。 この機能は、[**Windows.Storage**](/uwp/api/Windows.Storage) 名前空間の API で動作します。

テンプレートによって [SupportsMultipleInstances](multi-instance-uwp.md) 機能がアプリの Package.appxmanifest ファイルに追加されるため、複数の UWP コンソール アプリのインスタンスを同時に実行できます。

テンプレートは Package.appxmanifest ファイルに `Subsystem="console"` の機能も追加します。これは、この UWP アプリがコンソール アプリであることを示しています。 `desktop4` と iot2 `namespace` プレフィックスに注意してください。 UWP コンソール アプリは、デスクトップやモノのインターネット (IoT) プロジェクトでのみサポートされています。

```xml
<Package
  ...
  xmlns:desktop4="http://schemas.microsoft.com/appx/manifest/desktop/windows10/4" 
  xmlns:iot2="http://schemas.microsoft.com/appx/manifest/iot/windows10/2" 
  IgnorableNamespaces="uap mp uap5 desktop4 iot2">
  ...
  <Applications>
    <Application Id="App"
      ...
      desktop4:Subsystem="console" 
      desktop4:SupportsMultipleInstances="true" 
      iot2:Subsystem="console" 
      iot2:SupportsMultipleInstances="true"  >
      ...
      <Extensions>
          <uap5:Extension 
            Category="windows.appExecutionAlias" 
            Executable="YourApp.exe" 
            EntryPoint="YourApp.App">
            <uap5:AppExecutionAlias desktop4:Subsystem="console">
              <uap5:ExecutionAlias Alias="YourApp.exe" />
            </uap5:AppExecutionAlias>
          </uap5:Extension>
      </Extensions>
    </Application>
  </Applications>
    ...
</Package>
```

## <a name="additional-considerations-for-uwp-console-apps"></a>UWP コンソール アプリに関するその他の考慮事項

- コンソールアプリとして使用できるのは、C++/WinRT および C++/CX UWP アプリだけです。
- UWP コンソール アプリはデスクトップまたは IoT プロジェクト タイプをターゲットにする必要があります。
- UWP コンソールアプリでは、ウィンドウを作成できない場合があります。 メッセージボックス ()、場所 ()、またはユーザーの同意プロンプトなど、何らかの理由でウィンドウを作成する可能性のあるその他の API を使用することはできません。
- UWP コンソール アプリは、バックグラウンド タスクを消費したり、バックグラウンド タスクとして機能したりしない場合があります。
- [コマンド ラインのアクティブ化](https://blogs.windows.com/buildingapps/2017/07/05/command-line-activation-universal-windows-apps/#5YJUzjBoXCL4MhAe.97) を除き、UWP コンソール アプリは、ファイルの関連付け、プロトコルの関連付けなどのアクティブ化をサポートしていません。
- UWP コンソール アプリは複数インスタンスをサポートしていますが、[複数インスタンスのリダイレクト](multi-instance-uwp.md) はサポートしていません
- UWP アプリで使用できる Win32 API の一覧については、「[UWP アプリ用の Win32 API と COM API](/uwp/win32-and-com/win32-and-com-for-uwp-apps)」をご覧ください。