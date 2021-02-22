---
description: この記事では、パッケージ拡張を使用して、パッケージ化されたデスクトップ アプリをエクスプローラーと統合するさまざまな方法について説明します。
title: パッケージ化されたデスクトップ アプリをエクスプローラーと統合する
ms.date: 02/08/2021
ms.topic: article
keywords: windows 10, uwp
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: e3e7aaffc86a152530c933291321c30c2e7c507d
ms.sourcegitcommit: 2b7f6fdb3c393f19a6ad448773126a053b860953
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/12/2021
ms.locfileid: "100335200"
---
# <a name="integrate-a-packaged-desktop-app-with-file-explorer"></a>パッケージ化されたデスクトップ アプリをエクスプローラーと統合する

一部の Windows アプリには、ユーザーがアプリに関連するオプションを実行できるようにするコンテキスト メニューのエントリを追加する、エクスプローラー拡張が定義されています。 MSI や ClickOnce などの以前の Windows アプリ展開テクノロジでは、レジストリを使用してエクスプローラー拡張を定義しています。 レジストリには、エクスプローラー拡張やその他の種類のシェル拡張を制御する一連のハイブがあります。 通常、コンテキスト メニューに含めるさまざまな項目を構成するために、これらのインストーラーによって一連のレジストリ キーが作成されます。

[MSIX](/windows/msix/) を使用して Windows アプリをパッケージ化する場合、レジストリは仮想化されるため、アプリではレジストリを使用してエクスプローラー拡張を登録することはできません。 代わりに、パッケージ拡張を使用してエクスプローラー拡張を定義する必要があります。これは、パッケージ マニフェストで定義します。 この記事では、これを行ういくつかの方法について説明します。

この記事で使用されている完全なサンプル コードは、[GitHub](https://github.com/microsoft/Windows-AppConsult-Samples-DesktopBridge/tree/main/Docs-ContextMenuSample) で見つかります。

## <a name="add-a-context-menu-entry-that-supports-startup-parameters"></a>起動パラメーターをサポートするコンテキスト メニューのエントリを追加する

エクスプローラーと統合する最も簡単な方法の 1 つは、ユーザーがエクスプローラーで特定の種類のファイルを右クリックしたときに、利用可能なアプリの一覧に対象のアプリを追加するパッケージ拡張をコンテキスト メニューに定義することです。 ユーザーがアプリを開くと、その拡張によってアプリにパラメーターが渡されます。

このシナリオにはいくつかの制限があります。

* これは、[ファイルの種類を関連付ける機能](/windows/uwp/launch-resume/handle-file-activation)と組み合わせることでのみ機能します。 メイン アプリに関連付けられているファイルの種類に対してのみ、コンテキスト メニューに追加のオプションを表示できます (例: アプリでファイルをエクスプローラーでダブルクリックして開くことができるようにする)。
* コンテキスト メニューのオプションは、アプリがそのファイルの種類の既定のアプリとして設定されている場合にのみ表示されます。
* サポートされているアクションは、そのアプリのメインの実行可能ファイル ([スタート] メニューのエントリに接続されている同じ実行可能ファイル) を起動することのみです。 ただし、アクションごとに異なるパラメーターが指定される場合があるため、それをアプリが開始されるときに使用することで、どのアクションによってその実行がトリガーされたかを把握し、異なるタスクを実行できます。

これらの制限事項はあるものの、多くのシナリオではこのアプローチで十分です。 たとえば、画像エディターを構築している場合、画像のサイズを変更するエントリをコンテキスト メニューに簡単に追加できます。これにより、画像エディターとサイズ変更プロセスを開始するウィザードを直接起動できます。

### <a name="implement-the-context-menu-entry"></a>コンテキスト メニューのエントリを実装する

このシナリオをサポートするには、カテゴリに `windows.fileTypeAssociation` が指定された [Extension](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-extension) 要素をパッケージ マニフェストに追加します。 この要素は、[Extensions](/uwp/schemas/appxpackage/uapmanifestschema/element-1-extensions) 要素の子として [Application](/uwp/schemas/appxpackage/uapmanifestschema/element-application) 要素の下に追加する必要があります。

次の例は、拡張子が `.foo` のファイルに対してコンテキスト メニューを有効にするアプリの登録方法を示します。 この例では `.foo` の拡張子が指定されていますが、その理由はこれが偽の拡張子であり、通常は他のアプリやコンピューターには登録されていないからです。 既に使用されている可能性があるファイルの種類 (.txt や jpg など) を管理する必要がある場合は、アプリがそのファイルの種類に対して既定として設定されるまで、そのオプションは表示されないことに注意してください。 この例は、GitHub 上の関連サンプルの [Package.appxmanifest](https://github.com/microsoft/Windows-AppConsult-Samples-DesktopBridge/blob/main/Docs-ContextMenuSample/ContextMenuSample.Package/Package.appxmanifest) ファイルからの抜粋です。

```xml
<Extensions>
  <uap3:Extension Category="windows.fileTypeAssociation">
    <uap3:FileTypeAssociation Name="foo" Parameters="&quot;%1&quot;">
      <uap:SupportedFileTypes>
        <uap:FileType>.foo</uap:FileType>
      </uap:SupportedFileTypes>
      <uap2:SupportedVerbs>
        <uap3:Verb Id="Resize" Parameters="&quot;%1&quot; /p">Resize file</uap3:Verb>
      </uap2:SupportedVerbs>
    </uap3:FileTypeAssociation>
  </uap3:Extension>
</Extensions>
```

この例では、マニフェスト内のルートの `<Package>` 要素で、次の名前空間とエイリアスが宣言されていることを前提としています。

```xml
<Package
  xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap2="http://schemas.microsoft.com/appx/manifest/uap/windows10/2"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
  IgnorableNamespaces="uap uap2 uap3 rescap">
  ...
</Package>
```

[FileTypeAssociation](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation) 要素により、アプリがサポートするファイルの種類と関連付けられます。 詳細については、「[パッケージ アプリケーションを一連のファイルの種類に関連付ける](desktop-to-uwp-extensions.md#associate-your-packaged-application-with-a-set-of-file-types)」を参照してください。 この要素に関連する最も重要な項目を次に示します。

| 属性または要素 | 説明 |
|----------------------|-------------|
| `Name` 属性 | 登録する拡張子の名前からドットを削除し、照合します (前の例では `foo`)。 |
| `Parameters` 属性 | ユーザーが指定の拡張子を持つファイルをダブルクリックしたときに、アプリケーションに渡すパラメーターが格納されます。 通常は、少なくとも `%1` を渡します。これは、選択されたファイルのパスが格納される特殊なパラメーターです。 この方法により、ユーザーがファイルをダブルクリックすると、アプリケーションによってその完全なパスが認識され、読み込まれます。 |
| [SupportedFileTypes](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-supportedfiletypes) 要素 | 登録する拡張子の名前をドットを含めて指定します (この例では `.foo`)。 複数の `<FileType>` エントリを指定して、より多くのファイルの種類をサポートできます。 |

コンテキスト メニューの統合を定義するには、[SupportedVerbs](/uwp/schemas/appxpackage/uapmanifestschema/element-uap2-supportedverbs) 子要素も追加する必要があります。 この要素には、ユーザーがエクスプローラーで拡張子が .foo のファイルを右クリックしたときに表示されるオプションを定義する、[Verb](/uwp/schemas/appxpackage/uapmanifestschema/element-uap3-verb) 要素が 1 つ以上格納されます。 詳細については、「[特定の種類のファイルのコンテキスト メニューにオプションを追加する](desktop-to-uwp-extensions.md#add-options-to-the-context-menus-of-files-that-have-a-certain-file-type)」を参照してください。 [Verb](/uwp/schemas/appxpackage/uapmanifestschema/element-uap3-verb) 要素に関連する最も重要な項目を次に示します。

| 属性または要素 | 説明 |
|----------------------|-------------|
| `Id` 属性 | そのアクションの一意の識別子を指定します。|
| `Parameters` 属性 | [FileTypeAssociation](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation) 要素と同様に、[Verb](/uwp/schemas/appxpackage/uapmanifestschema/element-uap3-verb) 要素のこの属性には、ユーザーがコンテキスト メニューのエントリをクリックしたときにアプリケーションに渡されるパラメーターが格納されます。 通常、選択したファイルのパスを取得するための特殊なパラメーター `%1` 以外に、1 つ以上のパラメーターを渡してコンテキストを取得します。 これにより、コンテキスト メニューのエントリから開かれたことがアプリで認識されます。  |
| 要素の値 | [Verb](/uwp/schemas/appxpackage/uapmanifestschema/element-uap3-verb) 要素の値には、コンテキスト メニューのエントリに表示されるラベルが格納されます (この例では、 **[Resize file]\(ファイルのサイズ変更\)** )。 |

### <a name="access-the-startup-parameters-in-your-app-code"></a>アプリ コードで起動パラメーターにアクセスする

アプリでパラメーターを受け取る方法は、作成したアプリの種類によって異なります。 たとえば、WPF アプリでは通常、`App` クラスの `OnStartup` メソッドで起動イベントの引数を処理します。 起動パラメーターがあるかどうかを確認し、その結果に基づいて最適な操作を実行できます (メインのウィンドウではなく、そのアプリケーションの特定のウィンドウを開くなど)。

```csharp
public partial class App : Application
{
    protected override void OnStartup(StartupEventArgs e)
    {
        if (e.Args.Contains("Resize"))
        {
            // Open a specific window of the app.
        }
        else
        {
            MainWindow main = new MainWindow();
            main.Show();
        }
    }
}
```

次のスクリーンショットは、前の例で作成したコンテキスト メニューの **[Resize file]\(ファイルのサイズ変更\)** エントリを示します。

![コンテキスト メニューの [Resize file]\(ファイルのサイズ変更\) コマンドのスクリーンショット](images/file-explorer/resize-file.png)

## <a name="support-generic-files-or-folders-and-perform-complex-tasks"></a>汎用のファイルまたはフォルダーをサポートして複雑なタスクを実行する

前のセクションで説明したように、多くのシナリオでは、パッケージ マニフェストで [FileTypeAssociation](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation) 拡張を使用すれば十分ですが、制限もあります。 大きな課題は 2 つあります。

* 関連付けたファイルの種類しか処理できない。 たとえば、汎用フォルダーは処理できません。
* 一連のパラメーターを使用する以外にアプリを起動できない。 別の実行可能ファイルを起動する、メイン アプリを開くことなくタスクを実行するなどの、高度なオプションは実行できません。

これらの目的を実現するには、[シェル拡張](/windows/win32/shell/shell-exts)を作成する必要があります。これにより、エクスプローラーと統合するより強力な方法が得られます。 このシナリオでは、ラベル、アイコン、状態、実行するタスクなど、ファイルのコンテキスト メニューを管理するために必要なすべてが格納された、DLL を作成します。 この機能は DLL 内に実装されているため、通常のアプリで実行できるほぼすべての処理を実行できます。 DLL を実装した後は、パッケージ マニフェストで定義した拡張を使用して登録する必要があります。

> [!NOTE]
> このセクションで説明するプロセスには、制限が 1 つあります。 その拡張が格納された MSIX パッケージが対象のコンピューターにインストールされた後は、シェル拡張を読み込む前に、エクスプローラーを再起動する必要があります。 これを実現するには、コンピューターを再起動するか、**タスク マネージャー** を使用して **explorer.exe** プロセスを再起動できます。

### <a name="implement-the-shell-extension"></a>シェル拡張を実装する

シェル拡張は、[COM (コンポーネント オブジェクト モデル)](/windows/win32/com/component-object-model--com--portal) をベースとします。 DLL によって、システム レジストリに登録されている 1 つ以上の COM オブジェクトが公開されます。 Windows では、これらの COM オブジェクトを検出して、対象の拡張をエクスプローラーと統合します。 Windows のシェルを使用してコードを統合しているため、パフォーマンスとメモリ フットプリントが重要です。 そのため、これらの種類の拡張は通常、C++ を使用して構築されています。

シェル拡張を実装する方法を示すサンプル コードについては、GitHub 上の関連サンプルの [ExplorerCommandVerb](https://github.com/microsoft/Windows-AppConsult-Samples-DesktopBridge/tree/main/Docs-ContextMenuSample/ExplorerCommandVerb) プロジェクトを参照してください。 このプロジェクトは、Windows デスクトップ サンプルの[こちらのサンプル](https://github.com/microsoft/Windows-classic-samples/tree/master/Samples/Win7Samples/winui/shell/appshellintegration/ExplorerCommandVerb)をベースとしています。サンプルには、Visual Studio の最新バージョンでより使いやすくなるように、いくつかのリビジョンが用意されています。

このプロジェクトには、動的メニューや静的メニュー、DLL の手動登録など、さまざまなタスクの定型コードが多数格納されています。 このコードの大部分は、MSIX を使用してアプリをパッケージ化する場合は必要ありません。パッケージング サポートによってこれらのタスクが自動的に処理されるためです。 [ExplorerCommandVerb.cpp](https://github.com/microsoft/Windows-AppConsult-Samples-DesktopBridge/blob/main/Docs-ContextMenuSample/ExplorerCommandVerb/ExplorerCommandVerb.cpp) ファイルにはコンテキスト メニューの実装が格納されています。これがこのチュートリアルでメインとなるコード ファイルです。

重要な関数は `CExplorerCommandVerb::Invoke` です。 これは、ユーザーがコンテキスト メニューのエントリをクリックしたときに呼び出される関数です。 サンプルでは、パフォーマンスへの影響を最小限に抑えるために、操作が別のスレッドで実行されるため、本物の実装は実際には `CExplorerCommandVerb::_ThreadProc` にあります。

```cpp
DWORD CExplorerCommandVerb::_ThreadProc()
{
    IShellItemArray* psia;
    HRESULT hr = CoGetInterfaceAndReleaseStream(_pstmShellItemArray, IID_PPV_ARGS(&psia));
    _pstmShellItemArray = NULL;
    if (SUCCEEDED(hr))
    {
        DWORD count;
        psia->GetCount(&count);

        IShellItem2* psi;
        HRESULT hr = GetItemAt(psia, 0, IID_PPV_ARGS(&psi));
        if (SUCCEEDED(hr))
        {
            PWSTR pszName;
            hr = psi->GetDisplayName(SIGDN_DESKTOPABSOLUTEPARSING, &pszName);
            if (SUCCEEDED(hr))
            {
                WCHAR szMsg[128];
                StringCchPrintf(szMsg, ARRAYSIZE(szMsg), L"%d item(s), first item is named %s", count, pszName);

                MessageBox(_hwnd, szMsg, L"ExplorerCommand Sample Verb", MB_OK);

                CoTaskMemFree(pszName);
            }

            psi->Release();
        }
        psia->Release();
    }

    return 0;
}
```

ユーザーがファイルまたはフォルダーを右クリックすると、この関数によって、メッセージ ボックスに選択したファイルまたはフォルダーの完全なパスが表示されます。 シェル拡張を他の方法でカスタマイズする必要がある場合は、サンプル内の次の関数を拡張できます。

- [GetTitle](/windows/win32/api/shobjidl_core/nf-shobjidl_core-iexplorercommand-gettitle) 関数を変更して、コンテキスト メニューのエントリのラベルをカスタマイズできます。
- [GetIcon](/windows/win32/api/shobjidl_core/nf-shobjidl_core-iexplorercommand-geticon) 関数を変更して、コンテキスト メニューのエントリの近くに表示されるアイコンをカスタマイズできます。
- [GetTooltip](/windows/win32/api/shobjidl_core/nf-shobjidl_core-iexplorercommand-gettooltip) 関数を変更して、コンテキスト メニューのエントリにマウス カーソルを合わせると表示されるツールヒントをカスタマイズできます。

### <a name="register-the-shell-extension"></a>シェル拡張を登録する

シェル拡張は COM がベースであるため、Windows ではエクスプローラーと統合できるように、実装用の DLL が COM サーバーとして公開されている必要があります。 通常、これは一意の ID (CLSID) を COM サーバーに割り当て、システム レジストリの特定のハイブの中に登録することで行われます。 [ExplorerCommandVerb](https://github.com/microsoft/Windows-AppConsult-Samples-DesktopBridge/tree/main/Docs-ContextMenuSample/ExplorerCommandVerb) プロジェクトでは、`CExplorerCommandVerb` 拡張の CLSID は、[Dll.h](https://github.com/microsoft/Windows-AppConsult-Samples-DesktopBridge/blob/main/Docs-ContextMenuSample/ExplorerCommandVerb/Dll.h) ファイルで定義されています。

```cpp
class __declspec(uuid("CC19E147-7757-483C-B27F-3D81BCEB38FE")) CExplorerCommandVerb;
```

シェル拡張 DLL をMSIX パッケージにパッケージ化するときも、同様の方法で行います。 ただし、[こちら](https://blogs.windows.com/windowsdeveloper/2017/04/13/com-server-ole-document-support-desktop-bridge/)で説明されているように、GUID はレジストリではなく、パッケージ マニフェスト内に登録する必要があります。

パッケージ マニフェストで、次の名前空間を **Package** 要素に追加することから始めます。

```xml
<Package
  xmlns:desktop="http://schemas.microsoft.com/appx/manifest/desktop/windows10"
  xmlns:desktop4="http://schemas.microsoft.com/appx/manifest/desktop/windows10/4"
  xmlns:desktop5="http://schemas.microsoft.com/appx/manifest/desktop/windows10/5"
  xmlns:com="http://schemas.microsoft.com/appx/manifest/com/windows10" 
  IgnorableNamespaces="desktop desktop4 desktop5 com">
    
    ...
</Package>
```

CLSID を登録するには、カテゴリに `windows.comServer` が指定された [com.Extension](/uwp/schemas/appxpackage/uapmanifestschema/element-com-comserver) 要素をパッケージ マニフェストに追加します。 この要素は、[Extensions](/uwp/schemas/appxpackage/uapmanifestschema/element-1-extensions) 要素の子として [Application](/uwp/schemas/appxpackage/uapmanifestschema/element-application) 要素の下に追加する必要があります。 この例は、GitHub 上の関連サンプルの [Package.appxmanifest](https://github.com/microsoft/Windows-AppConsult-Samples-DesktopBridge/blob/main/Docs-ContextMenuSample/ContextMenuSample.Package/Package.appxmanifest) ファイルからの抜粋です。

```xml
<com:Extension Category="windows.comServer">
  <com:ComServer>
    <com:SurrogateServer DisplayName="ContextMenuSample">
      <com:Class Id="CC19E147-7757-483C-B27F-3D81BCEB38FE" Path="ExplorerCommandVerb.dll" ThreadingModel="STA"/>
    </com:SurrogateServer>
  </com:ComServer>
</com:Extension>
```

[com:Class](/uwp/schemas/appxpackage/uapmanifestschema/element-com-surrogateserver-class) 要素では、2 つの重要な属性を構成する必要があります。

| 属性 | 説明 |
|----------------------|-------------|
| `Id` 属性 | これは、登録するオブジェクトの CLSID と一致している必要があります。 この例では、これは `CExplorerCommandVerb` クラスに関連付けられた `Dll.h` ファイルで宣言されている CLSID です。 |
| `Path` 属性 | これには、COM オブジェクトを公開する DLL の名前が格納されている必要があります。 この例では、パッケージのルートに DLL が格納されているため、`ExplorerCommandVerb` プロジェクトによって生成される DLL の名前を指定するだけで済みます。 |

次に、ファイルのコンテキスト メニューを登録する別の拡張を追加します。 これを行うには、カテゴリに `windows.fileExplorerContextMenus` が指定された [desktop4:Extension](/uwp/schemas/appxpackage/uapmanifestschema/element-desktop4-extension) 要素をパッケージ マニフェストに追加します。 また、この要素は [Extensions](/uwp/schemas/appxpackage/uapmanifestschema/element-1-extensions) の子として [Application](/uwp/schemas/appxpackage/uapmanifestschema/element-application) 要素の下に追加する必要があります。

```xml
<desktop4:Extension Category="windows.fileExplorerContextMenus">
  <desktop4:FileExplorerContextMenus>
    <desktop5:ItemType Type="Directory">
      <desktop5:Verb Id="Command1" Clsid="CC19E147-7757-483C-B27F-3D81BCEB38FE" />
    </desktop5:ItemType>
  </desktop4:FileExplorerContextMenus>
</desktop4:Extension>
```

[desktop4:Extension](/uwp/schemas/appxpackage/uapmanifestschema/element-desktop4-extension) 要素の下に、2 つの重要な属性を構成する必要があります。

| 属性または要素 | 説明 |
|----------------------|-------------|
| [desktop5:ItemType](/uwp/schemas/appxpackage/uapmanifestschema/element-desktop5-itemtype) の `Type` 属性 | これによって、コンテキスト メニューに関連付ける項目の種類が定義されます。 すべてのファイルで表示する場合はアスタリスク (`*`) にします。特定のファイル拡張子 (`.foo`) にすることも、フォルダー (`Directory`) でも利用できます。 |
| [desktop5:Verb](/uwp/schemas/appxpackage/uapmanifestschema/element-desktop5-verb) の `Clsid` 属性 | これは、以前に COM サーバーとしてパッケージ マニフェスト ファイルに登録した CLSID と一致している必要があります。 |

### <a name="configure-the-dll-in-the-package"></a>パッケージ内の DLL を構成する

シェル拡張を実装する DLL (このサンプルでは **ExplorerCommandVerb.dll**) を、MSIX パッケージのルートに実装します。 [Windows アプリケーション パッケージ プロジェクト](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)を使用している場合、最も簡単な解決策は、DLL をコピーしてそのプロジェクトに貼り付け、DLL ファイルのプロパティの **[出力ディレクトリにコピー]** オプションが **[新しい場合はコピーする]** に設定されていることを確認することです。

パッケージに常に最新バージョンの DLL が格納されるようにするには、シェル拡張プロジェクトに[ビルド後のイベント](/visualstudio/ide/specifying-custom-build-events-in-visual-studio)を追加して、ビルドするたびに DLL が Windows アプリケーション パッケージ プロジェクトにコピーされるように設定できます。

### <a name="restart-file-explorer"></a>エクスプローラーを再起動する

シェル拡張パッケージをインストールした後は、シェル拡張を読み込む前に、エクスプローラーを再起動する必要があります。 これは、MSIX パッケージによって展開されて登録された、シェル拡張の制限です。

シェル拡張をテストするには、PC を再起動するか、**タスク マネージャー** を使用して **explorer.exe** プロセスを再起動します。 この操作を実行すると、コンテキスト メニューにエントリが表示されます。

![コンテキスト メニューのカスタム エントリのスクリーンショット](images/file-explorer/folder-context-menu.png)

これをクリックすると、`CExplorerCommandVerb::_ThreadProc` が呼び出され、選択されたフォルダーへのパスがメッセージ ボックスに表示されます。

![カスタム ポップアップのスクリーンショット](images/file-explorer/popup.png)
