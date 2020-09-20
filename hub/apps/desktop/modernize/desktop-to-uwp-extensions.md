---
description: 拡張機能を使用すると、あらかじめ定義された方法で Windows 10 にパッケージ デスクトップ アプリを統合できます。
title: デスクトップ ブリッジを使用して既存のデスクトップ アプリを最新化する
ms.date: 09/11/2020
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 0a8cedac-172a-4efd-8b6b-67fd3667df34
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: e0a8a7bf38fbf44fd3544d7912729bbd42672f34
ms.sourcegitcommit: 7c49f789f5b382b5b12efed6a81cbb4a25d44bd9
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/11/2020
ms.locfileid: "90026327"
---
# <a name="integrate-your-desktop-app-with-windows-10-and-uwp"></a>Windows 10 と UWP にデスクトップ アプリを統合する

デスクトップ アプリに[パッケージ ID](modernize-packaged-apps.md) がある場合は、[パッケージ マニフェスト](/uwp/schemas/appxpackage/uapmanifestschema/schema-root)内で定義済みの拡張機能を使用することによって、拡張機能を使用してアプリを Windows 10 と統合できます。

たとえば、ファイアウォール例外を作成する、アプリを特定のファイルの種類の既定アプリケーションにする、アプリをスタート タイルの参照先に指定する、などの操作を拡張機能で行うことができます。 拡張機能は、アプリのパッケージ マニフェスト ファイルに XML を追加するだけで使用できます。 コードは必要ありません。

この記事では、これらの拡張機能について、また、拡張機能を使って実行できるタスクについて説明します。

> [!NOTE]
> この記事で説明されている機能を使用するには、[デスクトップ アプリを MSIX パッケージにパッケージ化する](/windows/msix/desktop/desktop-to-uwp-root)か、または[スパース パッケージを使用してアプリ ID を付与する](grant-identity-to-nonpackaged-apps.md)ことで、デスクトップ アプリに[パッケージ ID ](modernize-packaged-apps.md)がある状態にする必要があります。

## <a name="transition-users-to-your-app"></a>ユーザーをアプリに移行する

ユーザーによってパッケージ アプリが使用されるように、移行を促します。

* [既存のスタート タイルとタスク バー ボタンの参照先をパッケージ アプリに設定する](#point)
* [デスクトップ アプリではなくパッケージ アプリケーションによってファイルが開かれるように設定する](#make)
* [パッケージ アプリケーションを一連のファイルの種類に関連付ける](#associate)
* [特定の種類のファイルのコンテキスト メニューにオプションを追加する](#add)
* [URL を使用して特定の種類のファイルを直接開く](#open)

<a id="point"></a>

### <a name="point-existing-start-tiles-and-taskbar-buttons-to-your-packaged-app"></a>既存のスタート タイルとタスク バー ボタンの参照先をパッケージ アプリに設定する

ユーザーによって、デスクトップ アプリがタスク バーまたはスタート メニューにピン留めされている可能性があります。 これらのショートカットの参照先を新しいパッケージ アプリに変更できます。

#### <a name="xml-namespace"></a>XML 名前空間

`http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities/3`

#### <a name="elements-and-attributes-of-this-extension"></a>この拡張機能の要素と属性

```XML
<Extension Category="windows.desktopAppMigration">
    <DesktopAppMigration>
        <DesktopApp AumId="[your_app_aumid]" />
        <DesktopApp ShortcutPath="[path]" />
    </DesktopAppMigration>
</Extension>
```

完全なスキーマ リファレンスについては、[こちら](/uwp/schemas/appxpackage/uapmanifestschema/element-rescap3-desktopappmigration)をご覧ください。

|名前 | 説明 |
|-------|-------------|
|カテゴリ |常に ``windows.desktopAppMigration`` です。
|AumID |パッケージ アプリのアプリケーション ユーザー モデル ID。 |
|ShortcutPath |アプリのデスクトップ バージョンを起動する .lnk ファイルへのパス。 |

#### <a name="example"></a>例

```XML
<Package
  xmlns:rescap3="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities/3"
  IgnorableNamespaces="rescap3">
  <Applications>
    <Application>
      <Extensions>
        <rescap3:Extension Category="windows.desktopAppMigration">
          <rescap3:DesktopAppMigration>
            <rescap3:DesktopApp AumId="[your_app_aumid]" />
            <rescap3:DesktopApp ShortcutPath="%USERPROFILE%\Desktop\[my_app].lnk" />
            <rescap3:DesktopApp ShortcutPath="%APPDATA%\Microsoft\Windows\Start Menu\Programs\[my_app].lnk" />
            <rescap3:DesktopApp ShortcutPath="%PROGRAMDATA%\Microsoft\Windows\Start Menu\Programs\[my_app_folder]\[my_app].lnk"/>
         </rescap3:DesktopAppMigration>
        </rescap3:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

#### <a name="related-sample"></a>関連するサンプル

[WPF picture viewer with transition/migration/uninstallation](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DesktopAppTransition)

<a id="make"></a>

### <a name="make-your-packaged-application-open-files-instead-of-your-desktop-app"></a>デスクトップ アプリではなくパッケージ アプリケーションによってファイルが開かれるように設定する

ユーザーが特定の種類のファイルを開くときに、アプリのデスクトップ バージョンではなく、新しいパッケージ アプリケーションが既定で開くように設定できます。

これを行うには、ファイルの関連付けを継承するために、関連付けされている各アプリケーションの[プログラム識別子 (ProgID)](/windows/desktop/shell/fa-progids) を指定します。

#### <a name="xml-namespaces"></a>XML 名前空間

* `http://schemas.microsoft.com/appx/manifest/uap/windows10/3`
* `http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities/3`

#### <a name="elements-and-attributes-of-this-extension"></a>この拡張機能の要素と属性

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[Name]">
         <MigrationProgIds>
            <MigrationProgId>"[ProgID]"</MigrationProgId>
        </MigrationProgIds>
    </FileTypeAssociation>
</Extension>
```

完全なスキーマ リファレンスについては、[こちら](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation)をご覧ください。

|名前 |説明 |
|-------|-------------|
|カテゴリ |常に ``windows.fileTypeAssociation`` です。
|名前 |ファイルの種類の関連付けの名前。 この名前を使用して、ファイルの種類を整理およびグループ化することができます。 名前は、すべて小文字でスペースを含まないようにする必要があります。 |
|MigrationProgId |ファイルの関連付けを継承するための、元のデスクトップ アプリケーションの用途、コンポーネント、およびバージョンを示す[プログラム識別子 (ProgID)](/windows/desktop/shell/fa-progids)。|

#### <a name="example"></a>例

```XML
<Package
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  xmlns:rescap3="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities/3"
  IgnorableNamespaces="uap3, rescap3">
  <Applications>
    <Application>
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
          <uap3:FileTypeAssociation Name="myfiletypes">
            <rescap3:MigrationProgIds>
              <rescap3:MigrationProgId>Foo.Bar.1</rescap3:MigrationProgId>
              <rescap3:MigrationProgId>Foo.Bar.2</rescap3:MigrationProgId>
            </rescap3:MigrationProgIds>
          </uap3:FileTypeAssociation>
        </uap:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

#### <a name="related-sample"></a>関連するサンプル

[WPF picture viewer with transition/migration/uninstallation](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DesktopAppTransition)

<a id="associate"></a>

### <a name="associate-your-packaged-application-with-a-set-of-file-types"></a>パッケージ アプリケーションを一連のファイルの種類に関連付ける

パッケージ アプリケーションは、ファイルの種類を表す拡張子に関連付けることができます。 ユーザーがファイルを右クリックして **[プログラムから開く]** オプションを選んだ場合、候補の一覧にアプリケーションが表示されます。

#### <a name="xml-namespaces"></a>XML 名前空間

* `http://schemas.microsoft.com/appx/manifest/uap/windows10`
* `http://schemas.microsoft.com/appx/manifest/uap/windows10/3`

#### <a name="elements-and-attributes-of-this-extension"></a>この拡張機能の要素と属性

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[Name]">
        <SupportedFileTypes>
            <FileType>"[file extension]"</FileType>
        </SupportedFileTypes>
    </FileTypeAssociation>
</Extension>
```

完全なスキーマ リファレンスについては、[こちら](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation)をご覧ください。

|名前 |説明 |
|-------|-------------|
|カテゴリ |常に ``windows.fileTypeAssociation`` です。
|名前 | ファイルの種類の関連付けの名前。 この名前を使用して、ファイルの種類を整理およびグループ化することができます。 名前は、すべて小文字でスペースを含まないようにする必要があります。   |
|FileType |アプリでサポートされているファイル拡張子。 |

#### <a name="example"></a>例

```XML
<Package
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  IgnorableNamespaces="uap, uap3">
  <Applications>
    <Application>
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
          <uap3:FileTypeAssociation Name="mediafiles">
            <uap:SupportedFileTypes>
            <uap:FileType>.avi</uap:FileType>
            </uap:SupportedFileTypes>
          </uap3:FileTypeAssociation>
        </uap:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

#### <a name="related-sample"></a>関連するサンプル

[WPF picture viewer with transition/migration/uninstallation](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DesktopAppTransition)

<a id="add"></a>

### <a name="add-options-to-the-context-menus-of-files-that-have-a-certain-file-type"></a>特定の種類のファイルのコンテキスト メニューにオプションを追加する

ほとんどの場合、ユーザーはファイルをダブルクリックして開きます。 ユーザーがファイルを右クリックすると、さまざまなオプションが表示されます。

このメニューには、オプションを追加できます。 これらのオプションを使用すると、ファイルの印刷、編集、プレビューなど、ファイルの操作を別の方法で行うことができます。

#### <a name="xml-namespaces"></a>XML 名前空間

* `http://schemas.microsoft.com/appx/manifest/uap/windows10`
* `http://schemas.microsoft.com/appx/manifest/uap/windows10/2`
* `http://schemas.microsoft.com/appx/manifest/uap/windows10/3`

#### <a name="elements-and-attributes-of-this-extension"></a>この拡張機能の要素と属性

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[Name]">
        <SupportedVerbs>
           <Verb Id="[ID]" Extended="[Extended]" Parameters="[parameters]">"[verb label]"</Verb>
        </SupportedVerbs>
    </FileTypeAssociation>
</Extension>
```

完全なスキーマ リファレンスについては、[こちら](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation)をご覧ください。

|名前 |説明 |
|-------|-------------|
|カテゴリ | 常に ``windows.fileTypeAssociation`` です。
|名前 |ファイルの種類の関連付けの名前。 この名前を使用して、ファイルの種類を整理およびグループ化することができます。 名前は、すべて小文字でスペースを含まないようにする必要があります。 |
|Verb |エクスプローラーのコンテキスト メニューに表示される名前です。 この文字列は、```ms-resource``` を使用してローカライズできます。|
|Id |動詞の一意の ID。 これは、アプリケーションが UWP アプリである場合に、アクティブ化イベント引数の一部としてアプリに渡されます。これにより、アプリでユーザーの選択内容を適切に処理できます。 アプリケーションが完全信頼のパッケージ アプリである場合は、代わりにパラメーターを受け取ります (次の項目をご覧ください)。 |
|パラメーター |動詞に関連付けられている引数のパラメーターと値のリスト。 アプリケーションが完全信頼のパッケージ アプリである場合は、アプリケーションがアクティブ化されたときに、これらのパラメーターがイベント引数としてアプリケーションに渡されます。 アプリケーションの動作は、さまざまなアクティブ化の動詞に基づいてカスタマイズできます。 変数にファイル パスが含まれる可能性がある場合は、パラメーター値を引用符で囲みます。 これにより、パスにスペースが含まれている場合に発生する問題を回避できます。 アプリケーションが UWP アプリの場合、パラメーターを渡すことはできません。 アプリは、代わりに ID を受け取ります (前の項目を参照してください)。|
|Extended |ユーザーが **Shift** キーを押しながらファイルを右クリックすることでコンテキスト メニューを表示した場合にのみ表示される動詞を指定します。 この属性は省略可能であり、指定されていない場合の既定値は **False** (たとえば常に動詞を表示する) です。 この動作は各動詞について個別に指定します ("開く" は例外で、常に **False**)。|

#### <a name="example"></a>例

```XML
<Package
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap2="http://schemas.microsoft.com/appx/manifest/uap/windows10/2"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"

  IgnorableNamespaces="uap, uap2, uap3">
  <Applications>
    <Application>
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
          <uap3:FileTypeAssociation Name="myfiletypes">
            <uap2:SupportedVerbs>
              <uap3:Verb Id="Edit" Parameters="/e &quot;%1&quot;">Edit</uap3:Verb>
              <uap3:Verb Id="Print" Extended="true" Parameters="/p &quot;%1&quot;">Print</uap3:Verb>
            </uap2:SupportedVerbs>
          </uap3:FileTypeAssociation>
        </uap:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

#### <a name="related-sample"></a>関連するサンプル

[WPF picture viewer with transition/migration/uninstallation](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DesktopAppTransition)

<a id="open"></a>

### <a name="open-certain-types-of-files-directly-by-using-a-url"></a>URL を使用して特定の種類のファイルを直接開く

ユーザーが特定の種類のファイルを開くときに、アプリのデスクトップ バージョンではなく、新しいパッケージ アプリケーションが既定で開くように設定できます。

#### <a name="xml-namespaces"></a>XML 名前空間

* `http://schemas.microsoft.com/appx/manifest/uap/windows10`
* `http://schemas.microsoft.com/appx/manifest/uap/windows10/3`

#### <a name="elements-and-attributes-of-this-extension"></a>この拡張機能の要素と属性

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[Name]" UseUrl="true" Parameters="%1">
        <SupportedFileTypes>
            <FileType>"[FileExtension]"</FileType>
        </SupportedFileTypes>
    </FileTypeAssociation>
</Extension>
```

完全なスキーマ リファレンスについては、[こちら](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation)をご覧ください。

|名前 |説明 |
|-------|-------------|
|カテゴリ |常に ``windows.fileTypeAssociation`` です。
|名前 |ファイルの種類の関連付けの名前。 この名前を使用して、ファイルの種類を整理およびグループ化することができます。 名前は、すべて小文字でスペースを含まないようにする必要があります。 |
|UseUrl |URL ターゲットから直接ファイルを開くかどうかを示します。 この値が設定されていない場合にアプリケーションで URL を使用してファイルを開こうとすると、まずシステムによってファイルがローカルにダウンロードされます。 |
|パラメーター | 省略可能なパラメーター。 |
|FileType |関連するファイル拡張子。 |

#### <a name="example"></a>例

```XML
<Package
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  IgnorableNamespaces="uap, uap3">
  <Applications>
      <Application>
        <Extensions>
          <uap:Extension Category="windows.fileTypeAssociation">
            <uap3:FileTypeAssociation Name="myfiletypes" UseUrl="true" Parameters="%1">
              <uap:SupportedFileTypes>
                <uap:FileType>.txt</uap:FileType>
                <uap:FileType>.doc</uap:FileType>
              </uap:SupportedFileTypes>
            </uap3:FileTypeAssociation>
          </uap:Extension>
        </Extensions>
      </Application>
    </Applications>
</Package>
```

## <a name="perform-setup-tasks"></a>セットアップ タスクを実行する

* [アプリのファイアウォール例外を作成する](#rules)
* [DLL ファイルをパッケージの任意のフォルダーに配置する](#load-paths)

<a id="rules"></a>

### <a name="create-firewall-exception-for-your-app"></a>アプリのファイアウォール例外を作成する

アプリケーションでポートを経由して通信する必要がある場合は、アプリケーションをファイアウォール例外の一覧に追加します。

#### <a name="xml-namespace"></a>XML 名前空間

`http://schemas.microsoft.com/appx/manifest/desktop/windows10/2`

#### <a name="elements-and-attributes-of-this-extension"></a>この拡張機能の要素と属性

```XML
<Extension Category="windows.firewallRules">
  <FirewallRules Executable="[executable file name]">
    <Rule
      Direction="[Direction]"
      IPProtocol="[Protocol]"
      LocalPortMin="[LocalPortMin]"
      LocalPortMax="LocalPortMax"
      RemotePortMin="RemotePortMin"
      RemotePortMax="RemotePortMax"
      Profile="[Profile]"/>
  </FirewallRules>
</Extension>
```

完全なスキーマ リファレンスについては、[こちら](/uwp/schemas/appxpackage/uapmanifestschema/element-desktop2-firewallrules)をご覧ください。

|名前 |説明 |
|-------|-------------|
|カテゴリ |常に ``windows.firewallRules`` です。|
|実行可能ファイル |ファイアウォールの例外の一覧に追加する実行可能ファイルの名前。 |
|方向 |規則が受信規則か送信規則かを示します。 |
|IPProtocol |通信プロトコル。 |
|LocalPortMin |ローカル ポート番号の範囲を示すポート番号の下限。 |
|LocalPortMax |ローカル ポート番号の範囲を示すポート番号の上限。 |
|RemotePortMax |リモート ポート番号の範囲を示すポート番号の下限。 |
|RemotePortMax |リモート ポート番号の範囲を示すポート番号の上限。 |
|[プロファイル] |ネットワークの種類。 |

#### <a name="example"></a>例

```XML
<Package
  xmlns:desktop2="http://schemas.microsoft.com/appx/manifest/desktop/windows10/2"
  IgnorableNamespaces="desktop2">
  <Extensions>
    <desktop2:Extension Category="windows.firewallRules">
      <desktop2:FirewallRules Executable="Contoso.exe">
          <desktop2:Rule Direction="in" IPProtocol="TCP" Profile="all"/>
          <desktop2:Rule Direction="in" IPProtocol="UDP" LocalPortMin="1337" LocalPortMax="1338" Profile="domain"/>
          <desktop2:Rule Direction="in" IPProtocol="UDP" LocalPortMin="1337" LocalPortMax="1338" Profile="public"/>
          <desktop2:Rule Direction="out" IPProtocol="UDP" LocalPortMin="1339" LocalPortMax="1340" RemotePortMin="15"
                         RemotePortMax="19" Profile="domainAndPrivate"/>
          <desktop2:Rule Direction="out" IPProtocol="GRE" Profile="private"/>
      </desktop2:FirewallRules>
  </desktop2:Extension>
</Extensions>
</Package>
```

<a id="load-paths"></a>

### <a name="place-your-dll-files-into-any-folder-of-the-package"></a>DLL ファイルをパッケージの任意のフォルダーに配置します。

[uap6:LoaderSearchPathOverride](/uwp/schemas/appxpackage/uapmanifestschema/element-uap6-loadersearchpathoverride) 拡張機能を使用すると、アプリ パッケージで最大 5 つのフォルダー パスを、アプリ パッケージのルート パスから見た相対パスとして宣言できます。これらは、アプリのプロセスのローダー検索パスで使用されます。

パッケージに実行権限がある場合、Windows アプリの [DLL 検索順序](/windows/win32/dlls/dynamic-link-library-search-order)には、パッケージの依存関係グラフのパッケージが含まれます。 既定で、これにはメイン、オプション、フレームワークの各パッケージが含まれます。ただし、これはパッケージ マニフェストの [uap6:AllowExecution](/uwp/schemas/appxpackage/uapmanifestschema/element-uap6-allowexecution) 要素によって上書きされる場合があります。

DLL 検索順序に含まれるパッケージには、既定で、その "*有効パス*" が含まれます。 有効パスの詳細については、[EffectivePath](/uwp/api/windows.applicationmodel.package.effectivepath) プロパティ (WinRT) および [PackagePathType](/windows/win32/api/appmodel/ne-appmodel-packagepathtype) 列挙 (Win32) を参照してください。

パッケージで [uap6:LoaderSearchPathOverride](/uwp/schemas/appxpackage/uapmanifestschema/element-uap6-loadersearchpathoverride) が指定されている場合、パッケージの有効パスの代わりにこの情報が使用されます。

各パッケージには、1 つの [uap6:LoaderSearchPathOverride](/uwp/schemas/appxpackage/uapmanifestschema/element-uap6-loadersearchpathoverride) 拡張機能のみを含めることができます。 つまり、1 つをメイン パッケージに追加し、他の拡張機能は[オプション パッケージと関連するセット](/windows/msix/package/optional-packages)それぞれに 1 つずつ追加できます。

#### <a name="xml-namespace"></a>XML 名前空間

`http://schemas.microsoft.com/appx/manifest/uap/windows10/6`

#### <a name="elements-and-attributes-of-this-extension"></a>この拡張機能の要素と属性

アプリケーション マニフェストのパッケージ レベルでこの拡張機能を宣言します。

```XML
<Extension Category="windows.loaderSearchPathOverride">
  <LoaderSearchPathOverride>
    <LoaderSearchPathEntry FolderPath="[path]"/>
  </LoaderSearchPathOverride>
</Extension>

```

|名前 | 説明 |
|-------|-------------|
|カテゴリ |常に ``windows.loaderSearchPathOverride``。
|FolderPath | DLL ファイルが格納されているフォルダーのパス。 パッケージのルート フォルダーの相対パスを指定します。 1 つの拡張機能で最大 5 つのパスを指定できます。 システムがパッケージのルート フォルダーにあるファイルを検索するようにする場合、これらのパスのいずれかに空の文字列を使用します。 重複するパスを含めないでください。また、パスの先頭と末尾にスラッシュやバックスラッシュを使用しないでください。 <br><br> システムはサブフォルダーを検索しないため、システムが読み込む DLL ファイルが含まれている各フォルダーを明示的に一覧表示してください。|

#### <a name="example"></a>例

```XML
<Package
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/6"
  IgnorableNamespaces="uap6">
  ...
    <Extensions>
      <uap6:Extension Category="windows.loaderSearchPathOverride">
        <uap6:LoaderSearchPathOverride>
          <uap6:LoaderSearchPathEntry FolderPath=""/>
          <uap6:LoaderSearchPathEntry FolderPath="folder1/subfolder1"/>
          <uap6:LoaderSearchPathEntry FolderPath="folder2/subfolder2"/>
        </uap6:LoaderSearchPathOverride>
      </uap6:Extension>
    </Extensions>
...
</Package>
```

## <a name="integrate-with-file-explorer"></a>エクスプローラーに統合する

ユーザーが慣れた方法でファイルを整理し操作できるようになります。

* [ユーザーが複数のファイルを同時に選択して開いた場合のアプリケーションの動作を定義する](#define)
* [エクスプ ローラーでサムネイル画像のファイル内容を表示する](#show)
* [エクスプローラーのプレビュー ウィンドウにファイル内容を表示する](#preview)
* [ユーザーがエクスプローラーの [種類] 列を使用してファイルをグループ化できるようにする](#enable)
* [ファイルのプロパティを検索、インデックス、プロパティ ダイアログ、詳細ウィンドウに利用できるようにする](#make-file-properties)
* [ファイルの種類のコンテキスト メニュー ハンドラーを指定する](#context-menu)
* [クラウド サービスのファイルがエクスプローラーに表示されるようにする](#cloud-files)

<a id="define"></a>

### <a name="define-how-your-application-behaves-when-users-select-and-open-multiple-files-at-the-same-time"></a>ユーザーが複数のファイルを同時に選択して開いた場合のアプリケーションの動作を定義する

ユーザーが同時に複数のファイルを開いたときに、アプリケーションがどのように動作するかを指定します。

#### <a name="xml-namespaces"></a>XML 名前空間

* `http://schemas.microsoft.com/appx/manifest/uap/windows10`
* `http://schemas.microsoft.com/appx/manifest/uap/windows10/2`
* `http://schemas.microsoft.com/appx/manifest/uap/windows10/3`

#### <a name="elements-and-attributes-of-this-extension"></a>この拡張機能の要素と属性

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[Name]" MultiSelectModel="[SelectionModel]">
        <SupportedVerbs>
            <Verb Id="Edit" MultiSelectModel="[SelectionModel]">Edit</Verb>
        </SupportedVerbs>
        <SupportedFileTypes>
            <FileType>"[FileExtension]"</FileType>
        </SupportedFileTypes>
</Extension>
```

完全なスキーマ リファレンスについては、[こちら](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation)をご覧ください。

|名前 |説明 |
|-------|-------------|
|カテゴリ |常に ``windows.fileTypeAssociation``。
|名前 |ファイルの種類の関連付けの名前。 この名前を使用して、ファイルの種類を整理およびグループ化することができます。 名前は、すべて小文字でスペースを含まないようにする必要があります。 |
|MultiSelectModel |下記参照 |
|FileType |関連するファイル拡張子。 |

**MultiSelectModel**

パッケージ デスクトップ アプリには、通常のデスクトップ アプリと同じ 3 つのオプションがあります。

* ``Player``:アプリケーションは、1 回アクティブ化されます。 選択されているすべてのファイルが、引数パラメーターとしてアプリケーションに渡されます。
* ``Single``:アプリケーションは、最初に選択されたファイルに対して 1 回アクティブ化されます。 その他のファイルは無視されます。
* ``Document``:選択された各ファイルについて、アプリケーションの新しい独立したインスタンスがアクティブ化されます。

 ファイルの種類やアクションごとに、さまざまな環境設定項目を設定できます。 たとえば、*Documents* は *Document* モードで開き、*Images* は *Player* モードで開くことができます。

#### <a name="example"></a>例

```XML
<Package
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap2="http://schemas.microsoft.com/appx/manifest/uap/windows10/2"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  IgnorableNamespaces="uap, uap2, uap3">
  <Applications>
    <Application>
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
          <uap3:FileTypeAssociation Name="myfiletypes" MultiSelectModel="Document">
            <uap2:SupportedVerbs>
              <uap3:Verb Id="Edit" MultiSelectModel="Player">Edit</uap3:Verb>
              <uap3:Verb Id="Preview" MultiSelectModel="Document">Preview</uap3:Verb>
            </uap2:SupportedVerbs>
            <uap:SupportedFileTypes>
              <uap:FileType>.txt</uap:FileType>
            </uap:SupportedFileTypes>
        </uap:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

ユーザーが 15 個以下のファイルを開いた場合、**MultiSelectModel** 属性の既定値は *Player* になります。 それ以外の場合、既定値は *Document* です。 UWP アプリは常に *Player* として起動されます。

<a id="show"></a>

### <a name="show-file-contents-in-a-thumbnail-image-within-file-explorer"></a>エクスプ ローラーでサムネイル画像のファイル内容を表示する

ファイルが中アイコン、大アイコン、特大アイコンで表示された場合に、ファイル内容のサムネイル画像をユーザーが確認できるようにします。

#### <a name="xml-namespace"></a>XML 名前空間

* `http://schemas.microsoft.com/appx/manifest/uap/windows10`
* `http://schemas.microsoft.com/appx/manifest/uap/windows10/2`
* `http://schemas.microsoft.com/appx/manifest/uap/windows10/3`
* `http://schemas.microsoft.com/appx/manifest/desktop/windows10/2`

#### <a name="elements-and-attributes-of-this-extension"></a>この拡張機能の要素と属性

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[Name]">
        <SupportedFileTypes>
            <FileType>"[FileExtension]"</FileType>
        </SupportedFileTypes>
        <ThumbnailHandler
            Clsid  ="[Clsid  ]" />
    </FileTypeAssociation>
</Extension>
```

完全なスキーマ リファレンスについては、[こちら](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation)をご覧ください。

|名前 |説明 |
|-------|-------------|
|カテゴリ |常に ``windows.fileTypeAssociation``。
|名前 |ファイルの種類の関連付けの名前。 この名前を使用して、ファイルの種類を整理およびグループ化することができます。 名前は、すべて小文字でスペースを含まないようにする必要があります。 |
|FileType |関連するファイル拡張子。 |
|Clsid   |アプリのクラス ID。 |

#### <a name="example"></a>例

```XML
<Package
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap2="http://schemas.microsoft.com/appx/manifest/uap/windows10/2"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  xmlns:desktop2="http://schemas.microsoft.com/appx/manifest/desktop/windows10/2"
  IgnorableNamespaces="uap, uap2, uap3, desktop2">
  <Applications>
    <Application>
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
          <uap3:FileTypeAssociation Name="myfiletypes">
            <uap2:SupportedFileTypes>
              <uap:FileType>.bar</uap:FileType>
            </uap2:SupportedFileTypes>
            <desktop2:ThumbnailHandler
              Clsid  ="20000000-0000-0000-0000-000000000001"  />
            </uap3:FileTypeAssociation>
         </uap::Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

<a id="preview"></a>

### <a name="show-file-contents-in-the-preview-pane-of-file-explorer"></a>エクスプローラーのプレビュー ウィンドウにファイル内容を表示する

エクスプローラーのプレビュー ウィンドウで、ユーザーがファイルの内容をプレビューできるようにします。

#### <a name="xml-namespace"></a>XML 名前空間

* `http://schemas.microsoft.com/appx/manifest/uap/windows10`
* `http://schemas.microsoft.com/appx/manifest/uap/windows10/2`
* `http://schemas.microsoft.com/appx/manifest/uap/windows10/3`
* `http://schemas.microsoft.com/appx/manifest/desktop/windows10/2`

#### <a name="elements-and-attributes-of-this-extension"></a>この拡張機能の要素と属性

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[Name]">
        <SupportedFileTypes>
            <FileType>"[FileExtension]"</FileType>
        </SupportedFileTypes>
        <DesktopPreviewHandler Clsid  ="[Clsid  ]" />
    </FileTypeAssociation>
</Extension>
```

完全なスキーマ リファレンスについては、[こちら](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation)をご覧ください。

|名前 |説明 |
|-------|-------------|
|カテゴリ |常に ``windows.fileTypeAssociation``。
|名前 |ファイルの種類の関連付けの名前。 この名前を使用して、ファイルの種類を整理およびグループ化することができます。 名前は、すべて小文字でスペースを含まないようにする必要があります。 |
|FileType |関連するファイル拡張子。 |
|Clsid   |アプリのクラス ID。 |

#### <a name="example"></a>例

```XML
<Package
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap2="http://schemas.microsoft.com/appx/manifest/uap/windows10/2"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  xmlns:desktop2="http://schemas.microsoft.com/appx/manifest/desktop/windows10/2"
  IgnorableNamespaces="uap, uap2, uap3, desktop2">
  <Applications>
    <Application>
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
          <uap3:FileTypeAssociation Name="myfiletypes">
            <uap2SupportedFileTypes>
              <uap:FileType>.bar</uap:FileType>
                </uap2SupportedFileTypes>
              <desktop2:DesktopPreviewHandler Clsid ="20000000-0000-0000-0000-000000000001" />
           </uap3:FileTypeAssociation>
        </uap:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

<a id="enable"></a>

### <a name="enable-users-to-group-files-by-using-the-kind-column-in-file-explorer"></a>ユーザーがエクスプローラーの [種類] 列を使用してファイルをグループ化できるようにする

ファイルの種類に関する 1 つまたは複数の定義済みの値を **Kind** フィールドに関連付けることができます。

ユーザーはエクスプローラーでこのフィールドを使用して、ファイルをグループ化できます。 また、このフィールドは、システム コンポーネントによって、インデックス作成などのさまざまな目的にも使用されます。

**Kind** フィールドの詳細と、このフィールドに使用できる値については、「[種類名の使用](/windows/desktop/properties/building-property-handlers-user-friendly-kind-names)」をご覧ください。

#### <a name="xml-namespaces"></a>XML 名前空間

* `http://schemas.microsoft.com/appx/manifest/uap/windows10`
* `http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities/3`

#### <a name="elements-and-attributes-of-this-extension"></a>この拡張機能の要素と属性

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[Name]">
        <SupportedFileTypes>
            <FileType>"[FileExtension]"</FileType>
        </SupportedFileTypes>
        <KindMap>
            <Kind value="[KindValue]">
        </KindMap>
    </FileTypeAssociation>
</Extension>
```

完全なスキーマ リファレンスについては、[こちら](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation)をご覧ください。

|名前 |説明 |
|-------|-------------|
|カテゴリ |常に ``windows.fileTypeAssociation``。
|名前 |ファイルの種類の関連付けの名前。 この名前を使用して、ファイルの種類を整理およびグループ化することができます。 名前は、すべて小文字でスペースを含まないようにする必要があります。 |
|FileType |関連するファイル拡張子。 |
|value |有効な [Kind 値](/windows/desktop/properties/building-property-handlers-user-friendly-kind-names)。 |

#### <a name="example"></a>例

```XML
<Package
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities/3"
  IgnorableNamespaces="uap, rescap">
  <Applications>
    <Application>
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
           <uap:FileTypeAssociation Name="mediafiles">
             <uap:SupportedFileTypes>
               <uap:FileType>.m4a</uap:FileType>
               <uap:FileType>.mta</uap:FileType>
             </uap:SupportedFileTypes>
             <rescap:KindMap>
               <rescap:Kind value="Item">
               <rescap:Kind value="Communications">
               <rescap:Kind value="Task">
             </rescap:KindMap>
          </uap:FileTypeAssociation>
      </uap:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

<a id="make-file-properties"></a>

### <a name="make-file-properties-available-to-search-index-property-dialogs-and-the-details-pane"></a>ファイルのプロパティを検索、インデックス、プロパティ ダイアログ、詳細ウィンドウに利用できるようにする

#### <a name="xml-namespace"></a>XML 名前空間

* `http://schemas.microsoft.com/appx/manifest/uap/windows10`
* `http://schemas.microsoft.com/appx/manifest/uap/windows10/3`
* `http://schemas.microsoft.com/appx/manifest/desktop/windows10/2`

#### <a name="elements-and-attributes-of-this-extension"></a>この拡張機能の要素と属性

```XML
<uap:Extension Category="windows.fileTypeAssociation">
    <uap:FileTypeAssociation Name="[Name]">
        <SupportedFileTypes>
            <FileType>.bar</FileType>
        </SupportedFileTypes>
        <DesktopPropertyHandler Clsid ="[Clsid]"/>
    </uap:FileTypeAssociation>
</uap:Extension>
```

完全なスキーマ リファレンスについては、[こちら](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation)をご覧ください。

|名前 |説明 |
|-------|-------------|
|カテゴリ |常に ``windows.fileTypeAssociation``。
|名前 |ファイルの種類の関連付けの名前。 この名前を使用して、ファイルの種類を整理およびグループ化することができます。 名前は、すべて小文字でスペースを含まないようにする必要があります。 |
|FileType |関連するファイル拡張子。 |
|Clsid  |アプリのクラス ID。 |

#### <a name="example"></a>例

```XML
<Package
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  xmlns:desktop2="http://schemas.microsoft.com/appx/manifest/desktop/windows10/2"
  IgnorableNamespaces="uap, uap3, desktop2">
  <Applications>
    <Application>
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
          <uap3:FileTypeAssociation Name="myfiletypes">
            <uap:SupportedFileTypes>
              <uap:FileType>.bar</uap:FileType>
            </uap:SupportedFileTypes>
            <desktop2:DesktopPropertyHandler Clsid ="20000000-0000-0000-0000-000000000001"/>
          </uap3:FileTypeAssociation>
        </uap:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

<a id="context-menu"></a>

### <a name="specify-a-context-menu-handler-for-a-file-type"></a>ファイルの種類のコンテキスト メニュー ハンドラーを指定する

デスクトップ アプリケーションで[コンテキスト メニュー ハンドラー](/windows/desktop/shell/context-menu-handlers)が定義されている場合は、この拡張機能を使用してメニュー ハンドラーを登録します。

#### <a name="xml-namespaces"></a>XML 名前空間

* `http://schemas.microsoft.com/appx/manifest/foundation/windows10`
* `http://schemas.microsoft.com/appx/manifest/desktop/windows10/4`

#### <a name="elements-and-attributes-of-this-extension"></a>この拡張機能の要素と属性

```XML
<Extensions>
    <com:Extension Category="windows.comServer">
        <com:ComServer>
            <com:SurrogateServer AppId="[AppID]" DisplayName="[DisplayName]">
                <com:Class Id="[Clsid]" Path="[Path]" ThreadingModel="[Model]"/>
            </com:SurrogateServer>
        </com:ComServer>
    </com:Extension>
    <desktop4:Extension Category="windows.fileExplorerContextMenus">
        <desktop4:FileExplorerContextMenus>
            <desktop4:ItemType Type="[Type]">
                <desktop4:Verb Id="[ID]" Clsid="[Clsid]" />
            </desktop4:ItemType>
        </desktop4:FileExplorerContextMenus>
    </desktop4:Extension>
</Extensions>
```

詳細なスキーマ リファレンスについては、[com:ComServer](/uwp/schemas/appxpackage/uapmanifestschema/element-com-comserver) と [desktop4:FileExplorerContextMenus](/uwp/schemas/appxpackage/uapmanifestschema/element-desktop4-fileexplorercontextmenus) を参照してください。

#### <a name="instructions"></a>Instructions

コンテキスト メニュー ハンドラーを登録するには、次の手順に従います。

1. デスクトップ アプリケーションで、[IExplorerCommand](/windows/desktop/api/shobjidl_core/nn-shobjidl_core-iexplorercommand) または [IExplorerCommandState](/windows/desktop/api/shobjidl_core/nn-shobjidl_core-iexplorercommandstate) インターフェイスを実装することによって、[コンテキスト メニュー ハンドラー](/windows/desktop/shell/context-menu-handlers)を実装します。 サンプルについては、[ExplorerCommandVerb](https://github.com/microsoft/Windows-classic-samples/tree/master/Samples/Win7Samples/winui/shell/appshellintegration/ExplorerCommandVerb) のコード サンプルを参照してください。 各実装オブジェクトのクラス GUID を必ず定義します。 たとえば、次のコードでは [IExplorerCommand](/windows/desktop/api/shobjidl_core/nn-shobjidl_core-iexplorercommand) の実装のクラス ID を定義しています。

    ```cpp
    class __declspec(uuid("d0c8bceb-28eb-49ae-bc68-454ae84d6264")) CExplorerCommandVerb;
    ```

2. パッケージ マニフェストで、COM サロゲート サーバーとコンテキスト メニュー ハンドラーの実装のクラス ID を登録する [com:ComServer](/uwp/schemas/appxpackage/uapmanifestschema/element-com-comserver) アプリケーション拡張機能を指定します。

    ```xml
    <com:Extension Category="windows.comServer">
        <com:ComServer>
            <com:SurrogateServer AppId="d0c8bceb-28eb-49ae-bc68-454ae84d6264" DisplayName="ContosoHandler">
                <com:Class Id="d0c8bceb-28eb-49ae-bc68-454ae84d6264" Path="ExplorerCommandVerb.dll" ThreadingModel="STA"/>
            </com:SurrogateServer>
        </com:ComServer>
    </com:Extension>
    ```

2. パッケージ マニフェストで、コンテキスト メニュー ハンドラーの実装を登録する [desktop4:FileExplorerContextMenus](/uwp/schemas/appxpackage/uapmanifestschema/element-desktop4-fileexplorercontextmenus) アプリケーション拡張機能を指定します。

    ```xml
    <desktop4:Extension Category="windows.fileExplorerContextMenus">
        <desktop4:FileExplorerContextMenus>
            <desktop4:ItemType Type=".rar">
                <desktop4:Verb Id="Command1" Clsid="d0c8bceb-28eb-49ae-bc68-454ae84d6264" />
            </desktop4:ItemType>
        </desktop4:FileExplorerContextMenus>
    </desktop4:Extension>
    ```

#### <a name="example"></a>例

```XML
<Package
  xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
  xmlns:desktop4="http://schemas.microsoft.com/appx/manifest/desktop/windows10/4"
  IgnorableNamespaces="desktop4">
  <Applications>
    <Application>
      <Extensions>
        <com:Extension Category="windows.comServer">
          <com:ComServer>
            <com:SurrogateServer AppId="d0c8bceb-28eb-49ae-bc68-454ae84d6264" DisplayName="ContosoHandler"">
              <com:Class Id="Id="d0c8bceb-28eb-49ae-bc68-454ae84d6264" Path="ExplorerCommandVerb.dll" ThreadingModel="STA"/>
            </com:SurrogateServer>
          </com:ComServer>
        </com:Extension>
        <desktop4:Extension Category="windows.fileExplorerContextMenus">
          <desktop4:FileExplorerContextMenus>
            <desktop4:ItemType Type=".contoso">
              <desktop4:Verb Id="Command1" Clsid="d0c8bceb-28eb-49ae-bc68-454ae84d6264" />
            </desktop4:ItemType>
          </desktop4:FileExplorerContextMenus>
        </desktop4:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

<a id="cloud-files"></a>

### <a name="make-files-from-your-cloud-service-appear-in-file-explorer"></a>クラウド サービスのファイルがエクスプローラーに表示されるようにする

アプリに実装するハンドラーを登録する ユーザーがエクスプローラーでクラウド ベースのファイルを右クリックしたときに表示されるコンテキスト メニュー オプションを追加することもできます。

#### <a name="xml-namespace"></a>XML 名前空間

* `http://schemas.microsoft.com/appx/manifest/desktop/windows10`

#### <a name="elements-and-attributes-of-this-extension"></a>この拡張機能の要素と属性

```XML
<Extension Category="windows.cloudfiles" >
    <CloudFiles IconResource="[Icon]">
        <CustomStateHandler Clsid ="[Clsid]"/>
        <ThumbnailProviderHandler Clsid ="[Clsid]"/>
        <ExtendedPropertyhandler Clsid ="[Clsid]"/>
        <CloudFilesContextMenus>
            <Verb Id ="Command3" Clsid= "[GUID]">[Verb Label]</Verb>
        </CloudFilesContextMenus>
    </CloudFiles>
</Extension>

```

|名前 |説明 |
|-------|-------------|
|カテゴリ |常に ``windows.cloudfiles``。
|iconResource |クラウド ファイル プロバイダー サービスを表すアイコン。 このアイコンは、エクスプローラーのナビゲーション ウィンドウに表示されます。  ユーザーは、このアイコンを選んでクラウド サービスのファイルを表示します。 |
|CustomStateHandler Clsid |CustomStateHandler を実装するアプリケーションのクラス ID。 システムは、このクラス ID を使ってクラウド ファイルのカスタム状態と列を要求します。 |
|ThumbnailProviderHandler Clsid |ThumbnailProviderHandler を実装するアプリケーションのクラス ID。 システムは、このクラス ID を使ってクラウド ファイルの縮小版イメージを要求します。 |
|ExtendedPropertyHandler Clsid |ExtendedPropertyHandler を実装するアプリケーションのクラス ID。  システムは、このクラス ID を使ってクラウド ファイルの拡張プロパティを要求します。 |
|動詞 |クラウド サービスによって提供されるファイルのエクスプローラー コンテキスト メニューに表示される名前です。 |
|Id |動詞の一意の ID。 |

#### <a name="example"></a>例

```XML
<Package
    xmlns:desktop="http://schemas.microsoft.com/appx/manifest/desktop/windows10"
    IgnorableNamespaces="desktop">
  <Applications>
    <Application>
      <Extensions>
        <Extension Category="windows.cloudfiles" >
            <CloudFiles IconResource="images\Wide310x150Logo.png">
                <CustomStateHandler Clsid ="20000000-0000-0000-0000-000000000001"/>
                <ThumbnailProviderHandler Clsid ="20000000-0000-0000-0000-000000000001"/>
                <ExtendedPropertyhandler Clsid ="20000000-0000-0000-0000-000000000001"/>
                <desktop:CloudFilesContextMenus>
                    <desktop:Verb Id ="keep" Clsid=
                       "20000000-0000-0000-0000-000000000001">
                       Always keep on this device</desktop:Verb>
                </desktop:CloudFilesContextMenus>
            </CloudFiles>
          </Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

<a id="start"></a>

## <a name="start-your-application-in-different-ways"></a>さまざまな方法でアプリケーションを起動する

* [プロトコルを使用してアプリケーションを起動する](#protocol)
* [エイリアスを使用してアプリケーションを起動する](#alias)
* [ユーザーが Windows にログオンしたときに実行可能ファイルを起動する](#executable)
* [ユーザーがデバイスを自分の PC に接続したときにアプリケーションを起動できるようにする](#autoplay)
* [Microsoft Store から更新プログラムを受信した後、自動的に再起動する](#updates)

<a id="protocol"></a>

### <a name="start-your-application-by-using-a-protocol"></a>プロトコルを使用してアプリケーションを起動する

プロトコルの関連付けによって、他のプログラムやシステム コンポーネントがパッケージ アプリと相互運用できるようにします。 プロトコルを使用してパッケージ アプリケーションを起動する場合、アプリケーションが適切に動作できるように、特定のパラメーターを指定してアプリケーションのアクティブ化イベント引数に渡すことができます。 パラメーターは、完全に信頼できるパッケージ アプリでのみサポートされています。 UWP アプリでは、パラメーターを使用できません。

#### <a name="xml-namespace"></a>XML 名前空間

`http://schemas.microsoft.com/appx/manifest/uap/windows10/3`

#### <a name="elements-and-attributes-of-this-extension"></a>この拡張機能の要素と属性

```XML
<Extension
    Category="windows.protocol">
  <Protocol
      Name="[Protocol name]"
      Parameters="[Parameters]" />
</Extension>
```

完全なスキーマ リファレンスについては、[こちら](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-protocol)をご覧ください。

|名前 |説明 |
|-------|-------------|
|カテゴリ |常に ``windows.protocol``。
|名前 |プロトコルの名前。 |
|パラメーター |アプリケーションがアクティブ化されたときにイベント引数としてアプリケーションに渡すパラメーターや値のリスト。 変数にファイル パスが含まれる可能性がある場合は、パラメーター値を引用符で囲みます。 これにより、パスにスペースが含まれている場合に発生する問題を回避できます。 |

### <a name="example"></a>例

```XML
<Package
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  xmlns:desktop="http://schemas.microsoft.com/appx/manifest/desktop/windows10"
  IgnorableNamespaces="uap3, desktop">
  <Applications>
    <Application>
      <Extensions>
        <uap3:Extension
          Category="windows.protocol">
          <uap3:Protocol
            Name="myapp-cmd"
            Parameters="/p &quot;%1&quot;" />
        </uap3:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

<a id="alias"></a>

### <a name="start-your-application-by-using-an-alias"></a>エイリアスを使用してアプリケーションを起動する

ユーザーと他のプロセスは、アプリの完全パスを指定することなく、エイリアスを使用してアプリケーションを起動できます。 そのエイリアス名を指定できます。

#### <a name="xml-namespaces"></a>XML 名前空間

* `http://schemas.microsoft.com/appx/manifest/uap/windows10/3`
* `http://schemas.microsoft.com/appx/manifest/desktop/windows10`

#### <a name="elements-and-attributes-of-this-extension"></a>この拡張機能の要素と属性

```XML
<Extension
    Category="windows.appExecutionAlias"
    Executable="[ExecutableName]"
    EntryPoint="Windows.FullTrustApplication">
    <AppExecutionAlias>
        <desktop:ExecutionAlias Alias="[AliasName]" />
    </AppExecutionAlias>
</Extension>
```

|名前 |説明 |
|-------|-------------|
|カテゴリ |常に ``windows.appExecutionAlias``。
|[実行可能ファイル] |エイリアスが呼び出されたときに起動する実行可能ファイルの相対パス。 |
|エイリアス |アプリの短い名前。 常に、拡張子 ".exe" で終わっている必要があります。 パッケージ内のアプリケーションごとにアプリの実行エイリアスは 1 つだけ指定できます。 複数のアプリで同じエイリアスが登録されている場合、システムは最後に登録されたアプリを呼び出します。したがって、他のアプリが上書きする可能性が低い一意のエイリアスを選んでください。
|

#### <a name="example"></a>例

```XML
<Package
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  IgnorableNamespaces="uap3">
  <Applications>
    <Application>
      <Extensions>
         <uap3:Extension
                Category="windows.appExecutionAlias"
                Executable="exes\launcher.exe"
                EntryPoint="Windows.FullTrustApplication">
            <uap3:AppExecutionAlias>
                <desktop:ExecutionAlias Alias="Contoso.exe" />
            </uap3:AppExecutionAlias>
        </uap3:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

完全なスキーマ リファレンスについては、[こちら](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation)をご覧ください。

<a id="executable"></a>

### <a name="start-an-executable-file-when-users-log-into-windows"></a>ユーザーが Windows にログオンしたときに実行可能ファイルを起動する

スタートアップ タスクによって、ユーザーがログオンするたびにアプリケーションで自動的に実行可能ファイルを実行できます。

> [!NOTE]
> このスタートアップ タスクを登録するには、ユーザーが少なくとも 1 回アプリケーションを起動する必要があります。

アプリケーションでは、複数のスタートアップ タスクを宣言できます。 各タスクは独立して起動されます。 すべてのスタートアップ タスクは、タスク マネージャーの **[スタートアップ]** タブに、アプリのマニフェストで指定した名前とアプリのアイコンを使って表示されます。 タスク マネージャーによって、タスクの起動への影響が自動的に分析されます。

ユーザーは、タスク マネージャーを使用して、アプリのスタートアップ タスクを手動で無効にすることができます。 ユーザーがタスクを無効にした場合、プログラムでタスクを再度有効にすることはできません。

#### <a name="xml-namespace"></a>XML 名前空間

`http://schemas.microsoft.com/appx/manifest/desktop/windows10`

#### <a name="elements-and-attributes-of-this-extension"></a>この拡張機能の要素と属性

```XML
<Extension
    Category="windows.startupTask"
    Executable="[ExecutableName]"
    EntryPoint="Windows.FullTrustApplication">
  <StartupTask
      TaskId="[TaskID]"
      Enabled="true"
      DisplayName="[DisplayName]" />
</Extension>
```

|名前 |説明 |
|-------|-------------|
|カテゴリ |常に ``windows.startupTask``。|
|[実行可能ファイル] |起動する実行可能ファイルへの相対パス。 |
|TaskId |タスクの一意の識別子。 この識別子を使用してアプリケーションで [Windows.ApplicationModel.StartupTask](/uwp/api/Windows.ApplicationModel.StartupTask) クラスの API を呼び出し、スタートアップ タスクをプログラムで有効または無効にすることができます。 |
|Enabled |初めて起動したタスクを有効にするか、無効にするかを指定します。 有効になっているタスクは、(ユーザーが無効にしていない限り) 次回ユーザーがログオンするときに実行されます。 |
|DisplayName |タスク マネージャーに表示されるタスクの名前。 この文字列は、```ms-resource``` を使用してローカライズできます。 |

#### <a name="example"></a>例

```XML
<Package
  xmlns:desktop="http://schemas.microsoft.com/appx/manifest/desktop/windows10"
  IgnorableNamespaces="desktop">
  <Applications>
    <Application>
      <Extensions>
        <desktop:Extension
          Category="windows.startupTask"
          Executable="bin\MyStartupTask.exe"
          EntryPoint="Windows.FullTrustApplication">
        <desktop:StartupTask
          TaskId="MyStartupTask"
          Enabled="true"
          DisplayName="My App Service" />
        </desktop:Extension>
      </Extensions>
    </Application>
  </Applications>
 </Package>
```

<a id="autoplay"></a>

### <a name="enable-users-to-start-your-application-when-they-connect-a-device-to-their-pc"></a>ユーザーがデバイスを自分の PC に接続したときにアプリケーションを起動できるようにする

ユーザーが自分の PC にデバイスを接続したときに、自動再生のオプションとしてアプリケーションを表示できます。

#### <a name="xml-namespace"></a>XML 名前空間

`http://schemas.microsoft.com/appx/manifest/desktop/windows10/3`

#### <a name="elements-and-attributes-of-this-extension"></a>この拡張機能の要素と属性

```XML
<Extension Category="windows.autoPlayHandler">
  <AutoPlayHandler>
    <InvokeAction ActionDisplayName="[action string]" ProviderDisplayName="[name of your app/service]">
      <Content ContentEvent="[Content event]" Verb="[any string]" DropTargetHandler="[Clsid]" />
      <Content ContentEvent="[Content event]" Verb="[any string]" Parameters="[Initialization parameter]"/>
      <Device DeviceEvent="[Device event]" HWEventHandler="[Clsid]" InitCmdLine="[Initialization parameter]"/>
    </InvokeAction>
  </AutoPlayHandler>
```

|名前 |説明 |
|-------|-------------|
|カテゴリ |常に ``windows.autoPlayHandler``。
|ActionDisplayName |ユーザーが PC に接続したときにデバイスで実行できるアクションを表す文字列 (例: "ファイルのインポート" や "ビデオの再生")。 |
|ProviderDisplayName | アプリケーションまたはサービスを表す文字列 (例: "Contoso ビデオ プレーヤー")。 |
|ContentEvent |ユーザーに ``ActionDisplayName`` と ``ProviderDisplayName`` をプロンプト表示する原因となるコンテンツ イベントの名前。 コンテンツ イベントは、カメラのメモリ カード、サム ドライブ、DVD などのボリューム デバイスが PC に挿入されたときに発生します。 これらのイベントの詳しい一覧については、[ここ](/windows/uwp/launch-resume/auto-launching-with-autoplay#autoplay-event-reference)をご覧ください。  |
|動詞 |[動詞] 設定では、選択されたオプションに応じてアプリケーションに渡される値を指定します。 自動再生のイベントの起動アクションは複数指定できます。また、[動詞] 設定を使って、ユーザーがアプリで選んだアクションを確認できます。 アプリに渡される起動イベント引数の verb プロパティを調べることでユーザーが選んだオプションを確認できます。 [動詞] 設定には任意の値を使うことができます。ただし、予約されている open を除きます。 |
|DropTargetHandler |[IDropTarget](/dotnet/api/microsoft.visualstudio.ole.interop.idroptarget) インターフェイスを実装するアプリケーションのクラス ID。 リムーバブル メディアのファイルは、[IDropTarget](/dotnet/api/microsoft.visualstudio.ole.interop.idroptarget) 実装の [Drop](/dotnet/api/microsoft.visualstudio.ole.interop.idroptarget.drop#Microsoft_VisualStudio_OLE_Interop_IDropTarget_Drop_Microsoft_VisualStudio_OLE_Interop_IDataObject_System_UInt32_Microsoft_VisualStudio_OLE_Interop_POINTL_System_UInt32__) メソッドに渡されます。  |
|パラメーター |すべてのコンテンツ イベントで [IDropTarget](/dotnet/api/microsoft.visualstudio.ole.interop.idroptarget) インターフェイスを実装する必要はありません。 どのコンテンツ イベントにも、[IDropTarget](/dotnet/api/microsoft.visualstudio.ole.interop.idroptarget) インターフェイスを実装する代わりにコマンド ライン パラメーターを指定することができます。 このようなイベントでは、これらのコマンド ライン パラメーターを使うことで自動再生によってアプリケーションが起動します。 アプリの初期化コードでそれらのパラメーターを解析して、自動再生によって起動したかどうかを判断し、カスタム実装を提供することができます。 |
|DeviceEvent |ユーザーに ``ActionDisplayName`` と ``ProviderDisplayName`` をプロンプト表示する原因となるデバイス イベントの名前。 デバイス イベントは、デバイスが PC に接続されると発生します。 デバイス イベントの先頭は文字列 ``WPD`` です。一覧については[ここ](/windows/uwp/launch-resume/auto-launching-with-autoplay#autoplay-event-reference)をご覧ください。 |
|HWEventHandler |[IHWEventHandler](/windows/desktop/api/shobjidl/nn-shobjidl-ihweventhandler) インターフェイスを実装するアプリケーションのクラス ID。 |
|InitCmdLine |[IHWEventHandler](/windows/desktop/api/shobjidl/nn-shobjidl-ihweventhandler) インターフェイスの [Initialize](/windows/desktop/api/shobjidl/nf-shobjidl-ihweventhandler-initialize) メソッドに渡す文字列パラメーター。 |

### <a name="example"></a>例

```XML
<Package
  xmlns:desktop3="http://schemas.microsoft.com/appx/manifest/desktop/windows10/3"
  IgnorableNamespaces="desktop3">
  <Applications>
    <Application>
      <Extensions>
        <desktop3:Extension Category="windows.autoPlayHandler">
          <desktop3:AutoPlayHandler>
            <desktop3:InvokeAction ActionDisplayName="Import my files" ProviderDisplayName="ms-resource:AutoPlayDisplayName">
              <desktop3:Content ContentEvent="ShowPicturesOnArrival" Verb="show" DropTargetHandler="CD041BAE-0DEA-4472-9B7B-C98043D26EA8"/>
              <desktop3:Content ContentEvent="PlayVideoFilesOnArrival" Verb="play" Parameters="%1" />
              <desktop3:Device DeviceEvent="WPD\ImageSource" HWEventHandler="CD041BAE-0DEA-4472-9B7B-C98043D26EA8" InitCmdLine="/autoplay"/>
            </desktop3:InvokeAction>
          </desktop3:AutoPlayHandler>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

<a id="updates"></a>

### <a name="restart-automatically-after-receiving-an-update-from-the-microsoft-store"></a>Microsoft Store から更新プログラムを受信した後、自動的に再起動する

ユーザーが更新プログラムをインストールするときにアプリケーションが開いている場合は、アプリケーションが終了します。

更新の完了後にアプリケーションを再起動させる場合は、再起動するすべてのプロセスで [RegisterApplicationRestart](/windows/desktop/api/winbase/nf-winbase-registerapplicationrestart) 関数を呼び出します。

[WM_QUERYENDSESSION](/windows/desktop/Shutdown/wm-queryendsession) メッセージを受け取るアプリケーションの各アクティブ ウィンドウ。 この時点で、アプリケーションで [RegisterApplicationRestart](/windows/desktop/api/winbase/nf-winbase-registerapplicationrestart) 関数を再度呼び出して、必要に応じてコマンド ラインを更新することができます。

アプリケーションの各アクティブ ウィンドウで [WM_ENDSESSION](/windows/desktop/Shutdown/wm-endsession) メッセージを受け取ったら、アプリケーションでデータを保存してシャットダウンする必要があります。

>[!NOTE]
> また、アプリケーションで [WM_ENDSESSION](/windows/desktop/Shutdown/wm-endsession) メッセージが処理されない場合は、アクティブ ウィンドウには [WM_CLOSE](/windows/desktop/winmsg/wm-close) メッセージも届きます。

この時点で、アプリケーションのプロセスがアプリケーション自体によって 30 秒内に終了されない場合、プロセスはプラットフォームによって強制終了されます。

更新が完了したら、アプリケーションが再起動されます。

## <a name="work-with-other-applications"></a>他のアプリケーションと連携する

他のアプリとの統合、他のプロセスの開始、情報の共有が可能です。

* [印刷をサポートするアプリケーションで自分のアプリケーションが印刷先として表示されるようにする](#printing)
* [他の Windows アプリケーションとフォントを共有する](#fonts)
* [ユニバーサル Windows プラットフォーム (UWP) アプリから Win32 プロセスを開始する](#win32-process)

<a id="printing"></a>

### <a name="make-your-application-appear-as-the-print-target-in-applications-that-support-printing"></a>印刷をサポートするアプリケーションで自分のアプリケーションが印刷先として表示されるようにする

メモ帳など別のアプリケーションからデータを印刷できるようにするには、そのアプリで利用できる印刷先の一覧に、印刷先として自分のアプリケーションが表示されるように設定できます。

印刷データを XML Paper Specification (XPS) 形式で受信できるように、アプリケーションを変更する必要があります。

#### <a name="xml-namespaces"></a>XML 名前空間

`http://schemas.microsoft.com/appx/manifest/desktop/windows10/2`

#### <a name="elements-and-attributes-of-this-extension"></a>この拡張機能の要素と属性

```XML
<Extension Category="windows.appPrinter">
    <AppPrinter
        DisplayName="[DisplayName]"
        Parameters="[Parameters]" />
</Extension>
```

完全なスキーマ リファレンスについては、[こちら](/uwp/schemas/appxpackage/uapmanifestschema/element-desktop2-appprinter)をご覧ください。

|名前 |説明 |
|-------|-------------|
|カテゴリ |常に ``windows.appPrinter``。
|DisplayName |アプリの印刷先一覧に表示する名前。 |
|パラメーター |要求を正しく処理するためにアプリケーションが必要とするパラメーター。 |

#### <a name="example"></a>例

```XML
<Package
  xmlns:desktop2="http://schemas.microsoft.com/appx/manifest/desktop/windows10/2"
  IgnorableNamespaces="desktop2">
  <Applications>
  <Application>
    <Extensions>
      <desktop2:Extension Category="windows.appPrinter">
        <desktop2:AppPrinter
          DisplayName="Send to Contoso"
          Parameters="/insertdoc %1" />
      </desktop2:Extension>
    </Extensions>
  </Application>
</Applications>
</Package>
```

この拡張機能を使用するサンプルについては、[こちら](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/PrintToPDF)をご覧ください。

<a id="fonts"></a>

### <a name="share-fonts-with-other-windows-applications"></a>他の Windows アプリケーションとフォントを共有する

他の Windows アプリケーションとカスタム フォントを共有できます。

> [!NOTE]
> この拡張機能を使用するアプリを Microsoft Store に送信するには、まず Microsoft Store チームから承認を得る必要があります。 承認を得るには、[https://aka.ms/storesupport](https://aka.ms/storesupport) にアクセスし、 **[お問い合わせ先]** をクリックして、ダッシュボードへのアプリの送信に関連するオプションを選択します。 この承認プロセスにより、アプリによってインストールされたフォントと、OS と共にインストールされたフォントの間で競合が発生しないようにすることができます。 承認を得ていない場合は、アプリを送信するときに次のようなエラーが表示されます。「パッケージ受領の検証エラー:このアカウントでは拡張機能 windows.sharedFonts を使用できません。 この拡張機能を使うためのアクセス許可を申請する場合は、サポート チームにお問い合わせください。」

#### <a name="xml-namespaces"></a>XML 名前空間

`http://schemas.microsoft.com/appx/manifest/desktop/windows10/2`

#### <a name="elements-and-attributes-of-this-extension"></a>この拡張機能の要素と属性

```XML
<Extension Category="windows.sharedFonts">
    <SharedFonts>
      <Font File="[FontFile]" />
    </SharedFonts>
  </Extension>
```

完全なスキーマ リファレンスについては、[こちら](/uwp/schemas/appxpackage/uapmanifestschema/element-uap4-sharedfonts)をご覧ください。

|名前 |説明 |
|-------|-------------|
|カテゴリ |常に ``windows.sharedFonts``。
|ファイル |共有するフォントが格納されたファイル。 |

#### <a name="example"></a>例

```XML
<Package
  xmlns:uap4="http://schemas.microsoft.com/appx/manifest/uap/windows10/4"
  IgnorableNamespaces="uap4">
  <Applications>
    <Application>
      <Extensions>
        <uap4:Extension Category="windows.sharedFonts">
          <uap4:SharedFonts>
            <uap4:Font File="Fonts\JustRealize.ttf" />
            <uap4:Font File="Fonts\JustRealizeBold.ttf" />
          </uap4:SharedFonts>
        </uap4:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

<a id="win32-process"></a>

### <a name="start-a-win32-process-from-a-universal-windows-platform-uwp-app"></a>ユニバーサル Windows プラットフォーム (UWP) アプリから Win32 プロセスを開始する

完全信頼で実行される Win32 プロセスを開始します。

#### <a name="xml-namespaces"></a>XML 名前空間

`http://schemas.microsoft.com/appx/manifest/desktop/windows10`

#### <a name="elements-and-attributes-of-this-extension"></a>この拡張機能の要素と属性

```XML
<Extension Category="windows.fullTrustProcess" Executable="[executable file]">
  <FullTrustProcess>
    <ParameterGroup GroupId="[GroupID]" Parameters="[Parameters]"/>
  </FullTrustProcess>
</Extension>
```

|名前 |説明 |
|-------|-------------|
|カテゴリ |常に ``windows.fullTrustProcess``。
|GroupID |実行可能ファイルに渡すパラメーターのセットを識別するための文字列。 |
|パラメーター |実行可能ファイルに渡すパラメーター。 |

#### <a name="example"></a>例

```XML
<Package xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
         xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
         xmlns:rescap=
"http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
         xmlns:desktop="http://schemas.microsoft.com/appx/manifest/desktop/windows10">
  ...
  <Capabilities>
      <rescap:Capability Name="runFullTrust"/>
  </Capabilities>
  <Applications>
    <Application>
      <Extensions>
          <desktop:Extension Category="windows.fullTrustProcess" Executable="fulltrustprocess.exe">
              <desktop:FullTrustProcess>
                  <desktop:ParameterGroup GroupId="SyncGroup" Parameters="/Sync"/>
                  <desktop:ParameterGroup GroupId="OtherGroup" Parameters="/Other"/>
              </desktop:FullTrustProcess>
           </desktop:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

この拡張機能は、すべてのデバイスで実行できるユニバーサル Windows プラットフォームのユーザー インターフェイスを作成する一方で、Win32 アプリケーションのコンポーネントについては完全信頼での実行を継続する場合に便利です。

Win32 アプリ向けに Windows アプリ パッケージを作成します。 そのうえで、この拡張機能を UWP アプリのパッケージ ファイルに追加してください。 この拡張機能は、Windows アプリ パッケージで実行可能ファイルを開始することを示します。  UWP アプリと Win32 アプリの間でやり取りを行うには、1 つまたは複数の[アプリ サービス](/windows/uwp/launch-resume/app-services)を設定します。 このシナリオについては詳しくは、[こちら](/archive/blogs/appconsult/desktop-bridge-the-migrate-phase-invoking-a-win32-process-from-a-uwp-app)をご覧ください。

## <a name="next-steps"></a>次のステップ

ご質問があるでしょうか。 Stack Overflow でお問い合わせください。 Microsoft のチームでは、これらの[タグ](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)をチェックしています。 [こちら](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)から質問することもできます。