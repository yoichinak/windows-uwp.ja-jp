---
Description: パッケージ化されたデスクトップアプリケーションの配布 (デスクトップブリッジ)
title: パッケージ化されたデスクトップアプリケーションを Microsoft Store に発行するか、1つまたは複数のデバイスにサイドロードします。
ms.date: 05/18/2018
ms.topic: article
keywords: windows 10, uwp
ms.assetid: edff3787-cecb-4054-9a2d-1fbefa79efc4
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: 597a283fd28b571ed968255312059c7049f3f700
ms.sourcegitcommit: 350d6e6ba36800df582f9715c8d21574a952aef1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/31/2019
ms.locfileid: "68682561"
---
# <a name="distribute-your-packaged-desktop-app"></a>パッケージ化されたデスクトップアプリを配布する

[デスクトップアプリを MSIX パッケージでパッケージ化](/windows/msix/desktop/desktop-to-uwp-root)する場合は、パッケージ化されたアプリケーションを Microsoft Store に発行するか、1つまたは複数のデバイスにサイドロードすることができます。

> [!NOTE]
> パッケージ化されたアプリケーションにユーザーを移行する方法を計画していますか。 アプリを配布する前に、このガイドの「[パッケージ アプリにユーザーを移行する](#transition-users)」セクションを参照して、アイデアを得てください。

## <a name="distribute-your-application-by-publishing-it-to-the-microsoft-store"></a>アプリケーションを Microsoft Store に公開して配布する

[Microsoft Store](https://www.microsoft.com/store/apps) は、お客様がアプリを取得する場合に最も便利な方法です。

アプリケーションを Microsoft Store に発行して、最も広範な対象ユーザーに届けることができます。 また、組織のお客様は、アプリケーションを入手して、 [Microsoft Store For Business](https://businessstore.microsoft.com/store)を通じて組織に内部で配布することができます。

Microsoft Store への公開を計画している場合は、申請プロセスの一部としていくつかの追加の質問をされます。 これは、パッケージ マニフェストが **runFullTrust** という名前の制限付き機能を宣言し、弊社でアプリケーションによるその機能の使用を承認する必要があるためです。 この要件の詳細については、こちらを参照してください。[制限付きの機能](/windows/uwp/packaging/app-capability-declarations#restricted-capabilities)。

ストアに送信する前に、アプリケーションに署名する必要はありません。

>[!IMPORTANT]
> アプリケーションを Microsoft Store に発行する場合は、Windows 10 S を実行しているデバイスでアプリケーションが正しく動作することを確認してください。これは、ストアの要件です。 「[Windows アプリの Windows 10 S 対応をテストする](/windows/msix/desktop/desktop-to-uwp-test-windows-s)」をご覧ください。

<a id="side-load" />

## <a name="distribute-your-application-without-placing-it-onto-the-microsoft-store"></a>Microsoft Store に配置せずにアプリケーションを配布する

ストアを使用せずにアプリケーションを配布する場合は、アプリを1つまたは複数のデバイスに手動で配布できます。

この方法は、配布エクスペリエンスをきめ細かく制御する必要がある場合や、Microsoft Store の認定プロセスへの関与が望ましくない場合などに有効です。

ストアに配置せずにアプリケーションを他のデバイスに配布するには、証明書を取得し、その証明書を使用してアプリケーションに署名してから、そのデバイスにアプリケーションをサイドロードする必要があります。

[証明書を作成](/windows/msix/package/create-certificate-package-signing)することも、[Verisign](https://www.verisign.com/) などのポピュラーなベンダーから取得することもできます。

Windows 10 を実行するデバイスにアプリケーションを配布する場合は、アプリケーションを Microsoft Store で署名する必要があります。そのため、アプリケーションをこれらのデバイスに配布する前に、ストアの送信プロセスを行う必要があります。

証明書を作成する場合は、アプリを実行する各デバイスの証明書ストア ("**信頼されたルート**" または "**信頼されたユーザー**") にインストールする必要があります。 ポピュラーなベンダーから証明書を取得する場合、システムにはアプリの他に何もインストールする必要はありません。  

> [!IMPORTANT]
> 証明書の発行元名がアプリの発行者名と一致することを確認してください。

証明書を使用してアプリケーションに署名する方法については、「 [SignTool を使用してアプリケーションパッケージに署名](/windows/msix/package/sign-app-package-using-signtool)する」を参照してください。

アプリケーションを他のデバイスにサイドロードには、「[サイドロード LOB apps In Windows 10](/windows/application-management/sideload-apps-in-windows-10)」を参照してください。

<a id="transition-users" />

## <a name="transition-users-to-your-packaged-app"></a>パッケージ アプリへのユーザーの移行

ユーザーによってパッケージ アプリが使用されるようにするには、アプリを配布する前に、パッケージ マニフェストにいくつかの拡張機能を追加することを検討してください。 次のようなことができます。

* 既存のスタート タイルとタスク バー ボタンの参照先をパッケージ アプリに設定する。
* パッケージ化されたアプリケーションをファイルの種類のセットに関連付けます。
* 既定では、パッケージ化されたアプリケーションが特定の種類のファイルを開くようにします。

拡張機能の完全な一覧と使用方法のガイダンスについては、「[アプリにユーザーを移行する](desktop-to-uwp-extensions.md#transition-users-to-your-app)」を参照してください。

また、次のタスクを実行するパッケージアプリケーションにコードを追加することを検討してください。

* デスクトップアプリケーションに関連付けられているユーザーデータを、パッケージアプリの適切なフォルダーの場所に移行します。
* アプリのデスクトップ バージョンをアンインストールするためのオプションをユーザーに示します。

これらのタスクについて、それぞれ説明します。 ユーザー データの移行から開始します。

### <a name="migrate-user-data"></a>ユーザー データの移行

ユーザーデータを移行するコードを追加する場合は、アプリケーションが最初に起動されたときにのみそのコードを実行することをお勧めします。 ユーザー データを移行する前に、ユーザーに対してダイアログ ボックスを表示して、何が起こっているか、なぜ移行が推奨されるのか、既存のデータにどのような影響があるかを説明します。

例として、.NET ベースのパッケージ アプリでの方法を次に示します。

```csharp
private void MigrateUserData()
{
    String sourceDir = Environment.GetFolderPath
        (Environment.SpecialFolder.ApplicationData) + "\\AppName";

    if (sourceDir != null)
    {
        DialogResult migrateResult = MessageBox.Show
            ("Would you like to migrate your data from the previous version of this app?",
             "Data Migration", MessageBoxButtons.YesNo);

        if (migrateResult.Equals(DialogResult.Yes))
        {
            String destinationDir =
                Windows.Storage.ApplicationData.Current.LocalFolder.Path + "\\AppName";

            Process process = new Process();
            process.StartInfo.FileName = "robocopy.exe";
            process.StartInfo.Arguments = "%LOCALAPPDATA%\\AppName " + destinationDir + " /move";
            process.StartInfo.CreateNoWindow = true;
            process.Start();
            process.WaitForExit();

            if (process.ExitCode > 1)
            {
                //Migration was unsuccessful -- you can choose to block/retry/other action
            }
        }
    }
}
```

### <a name="uninstall-the-desktop-version-of-your-app"></a>アプリのデスクトップ バージョンをアンインストールする

ユーザーのデスクトップアプリケーションは、最初にアクセス許可を求めずにアンインストールしないことをお勧めします。 ユーザーに許可を求めるには、そのためのダイアログ ボックスを表示します。 ユーザーによって、アプリのデスクトップ バージョンをアンインストールしないように指定されることも考えられます。 その場合は、デスクトップアプリケーションの使用をブロックするか、両方のアプリの並列使用をサポートするかを決定する必要があります。

例として、.NET ベースのパッケージ アプリでの方法を次に示します。

このスニペットの完全なコンテキストを確認するには、「[WPF picture viewer with transition/migration/uninstallation](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DesktopAppTransition)」というサンプルの **MainWindow.cs** ファイルを参照してください。

```csharp
private void RemoveDesktopApp()
{              
    //Typically, you can find your uninstall string at this location.
    String uninstallString = (String)Microsoft.Win32.Registry.GetValue
        (@"HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\Windows\CurrentVersion" +
         @"\Uninstall\{7AD02FB8-B85E-44BC-8998-F4803BA5A0E3}\", "UninstallString", null);

    //Detect if the previous version of the Desktop application is installed.
    if (uninstallString != null)
    {
        DialogResult uninstallResult = MessageBox.Show
            ("To have the best experience, consider uninstalling the "
              + " previous version of this app. Would you like to do that now?",
              "Uninstall the previous version", MessageBoxButtons.YesNo);

        if (uninstallResult.Equals(DialogResult.Yes))
        {
                    string[] uninstallArgs = uninstallString.Split(' ');

            Process process = new Process();
            process.StartInfo.FileName = uninstallArgs[0];
            process.StartInfo.Arguments = uninstallArgs[1];
            process.StartInfo.CreateNoWindow = true;

            process.Start();
            process.WaitForExit();

            if (process.ExitCode != 0)
            {
                //Uninstallation was unsuccessful - You can choose to block the application here.
            }
        }
    }

}
```

## <a name="next-steps"></a>次の手順

**質問に対する回答を検索する**

ご質問がある場合は、 Stack Overflow でお問い合わせください。 Microsoft のチームでは、これらの[タグ](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)をチェックしています。 [こちら](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)から質問することもできます。

Microsoft Store へのアプリの公開で問題が発生した場合は、この[ブログ投稿](https://blogs.msdn.microsoft.com/appconsult/2017/09/25/preparing-a-desktop-bridge-application-for-the-store-submission/)で役に立つヒントを参照できます。

**フィードバックの提供または機能に関する提案**

[UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial) のページをご覧ください。
