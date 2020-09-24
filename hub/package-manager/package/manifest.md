---
title: パッケージ マニフェストを作成する
description: ソフトウェア パッケージを Windows パッケージ マネージャー リポジトリに送信する場合は、まずパッケージ マニフェストを作成します。
ms.date: 04/29/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: a3a1acebf2b48e767fbd16998967145976305434
ms.sourcegitcommit: 94841d1d59703897b42b11597c28a9d966626f47
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/23/2020
ms.locfileid: "91110567"
---
# <a name="create-your-package-manifest"></a>パッケージ マニフェストを作成する

[!INCLUDE [preview-note](../../includes/package-manager-preview.md)]

ソフトウェア パッケージを [Windows パッケージ マネージャー リポジトリ](repository.md)に送信する場合は、まずパッケージ マニフェストを作成します。 マニフェストは、インストールするアプリケーションについて説明する YAML ファイルです。

この記事では、Windows パッケージ マネージャーのパッケージ マニフェストの内容について説明します。

## <a name="yaml-basics"></a>YAML の基本

パッケージ マニフェストに YAML 形式が選択された理由は、人間が比較的簡単に読みやすく、他の Microsoft 開発ツールと一貫性があるためです。 YAML 構文に詳しくない場合は、[YAML を Y 分で学習する](https://learnxinyminutes.com/docs/yaml/)ためのページで基本を学習できます。

> [!NOTE]
> 現時点では、Windows パッケージ マネージャーのマニフェストでは一部の YAML 機能がサポートされていません。 サポートされていない YAML の機能には、アンカー、複合キー、セットなどがあります。

## <a name="conventions"></a>表記規則

この記事では、次の表記規則を使用します。

* `:` の左側は、マニフェストの定義で使用されるリテラル キーワードです。
* `:` の右側は、データ型です。 データ型は、**string** などのプリミティブ型の場合と、この記事の別の場所で定義されているリッチ構造への参照の場合があります。
* 表記 `[` *datatype* `]` は、言及したデータ型の配列を示します。 たとえば、`[ string ]` は文字列の配列です。
* 表記 `{` *datatype* `:` *datatype* `}` は、あるデータ型から別のデータ型へのマッピングです。 たとえば、`{ string: string }` は文字列から文字列へのマッピングです。

## <a name="manifest-contents"></a>マニフェストの内容

パッケージ マニフェストには、一連の必須項目が含まれている必要があります。また、ソフトウェア インストールのカスタマー エクスペリエンスを向上させることができる追加のオプション項目を含めることもできます。 このセクションでは、必要なマニフェスト スキーマと完全なマニフェスト スキーマについて簡単にまとめ、それぞれの例を示します。

マニフェスト ファイルの各項目は、パスカル ケースである必要があり、重複させることはできません。

マニフェスト内の項目の完全な一覧と説明については、[https://github.com/microsoft/winget-cli](https://github.com/microsoft/winget-cli) リポジトリの[マニフェスト仕様](https://github.com/microsoft/winget-cli/blob/master/doc/ManifestSpecv0.1.md)を参照してください。

### <a name="minimal-required-schema"></a>最低限必要なスキーマ

#### <a name="minimal-required-schema"></a>[最低限必要なスキーマ](#tab/minschema/)

```yaml
Id: string # Publisher.package format.
Publisher: string # The name of the publisher.
Name: string # The name of the application.
Version: string # Version numbering format.
License: string # The open source license or copyright.
InstallerType: string # Enumeration of supported installer types (exe, msi, msix, inno, wix, nullsoft, appx).
Installers:
  - Arch: string # Enumeration of supported architectures.
    Url: string # Path to download installation file.
    Sha256: string # SHA256 calculated from installer.
ManifestVersion: 0.1.0
```

#### <a name="example"></a>[例](#tab/minexample/)

```yaml
Id: microsoft.teams
Publisher: Microsoft Corporation
Name: Microsoft Teams
Version: 1.3.0.4461
License: Copyright (c) Microsoft Corporation. All rights reserved.
InstallerType: exe
Installers:
  - Arch: x64
    Url: https://statics.teams.cdn.office.net/production-windows-x64/1.3.00.4461/Teams_windows_x64.exe
    Sha256: 712f139d71e56bfb306e4a7b739b0e1109abb662dfa164192a5cfd6adb24a4e1
ManifestVersion: 0.1.0
```

* * *

### <a name="complete-schema"></a>完全なスキーマ

#### <a name="complete-schema"></a>[完全なスキーマ](#tab/compschema/)

```yaml
Id: string # Publisher.package format.
Publisher: string # The name of the publisher.
Name: string # The name of the application.
AppMoniker: string # The common name someone may use to search for the package.
Version: string # Version numbering format for package version.
Channel: string # A string representing the flight ring.
License: string # The open source license or copyright.
LicenseUrl: string # Valid secure URL to license.
MinOSVersion: string # Version numbering format for minimum version of Windows supported.
Description: string # Description of the package.
Homepage: string # Valid secure URL for the package.
Tags: list # Additional strings a user would use to search for the package.
FileExtensions: list # List of file extensions the package could support.
Protocols: list # List of protocols the package provides a handler for.
Commands: list # List of commands or aliases the user would use to run the package.
InstallerType: string # Enumeration of supported installer types (exe, msi, msix, inno, wix, nullsoft, appx).
Switches: # These can be used to change the install behavior if supported by the InstallerType.
  Custom: string # Custom switches passed to the installer.
  Silent: string # Switches passed to the installer for silent installation.
  SilentWithProgress: string # Switches passed to the installer for non-interactive install.
  Interactive: string # Experimental.
  Language: string # Experimental.
  Log: string # Specifies log redirection switches and path.
  InstallLocation: string # Specifies alternate location to install package.
Installers: # Nested map of keys for specific installer.
  - Arch: string # Enumeration of supported architectures.
    Url: string # Path to download installation file.
    Sha256: string # SHA256 calculated from installer.
    SignatureSha256: string # SHA256 calculated from signature file's hash of MSIX file.
    Switches: # Collection of entries to override root keys. The primary supported values are: Custom, Silent, SilentWithProgress, Interactive. For a complete list see the specification at https://github.com/microsoft/winget-cli/blob/master/doc/ManifestSpecv0.1.md.
    Scope: string # Experimental.
    SystemAppId: string # Experimental.
Localization: # Nested map of keys for localization.
  - Language: string # Locale for display fields and localized URLs.
ManifestVersion: string # Version number format for manifest version.
```

#### <a name="good-example"></a>[良い例](#tab/good/)

```yaml
Id: microsoft.teams
Publisher: Microsoft Corporation
Name: Microsoft Teams
Version: 1.3.0.4461
License: Copyright (c) Microsoft Corporation. All rights reserved.
LicenseUrl: https://docs.microsoft.com/en-us/MicrosoftTeams/assign-teams-licenses
InstallerType: exe
Installers:
  - Arch: x64
    Url: https://statics.teams.cdn.office.net/production-windows-x64/1.3.00.4461/Teams_windows_x64.exe
    Sha256: 712f139d71e56bfb306e4a7b739b0e1109abb662dfa164192a5cfd6adb24a4e1
ManifestVersion: 0.1.0
```

#### <a name="better-example"></a>[より良い例](#tab/better/)

```yaml
Id: microsoft.teams
Publisher: Microsoft Corporation
Name: Microsoft Teams
Version: 1.3.0.4461
AppMoniker: teams
MinOSVersion: 10.0.0.0
Description: The hub for teamwork in Microsoft 365
Homepage: https://www.microsoft.com/microsoft/teams
License: Copyright (c) Microsoft Corporation. All rights reserved.
LicenseUrl: https://docs.microsoft.com/en-us/MicrosoftTeams/assign-teams-licenses
InstallerType: exe
Installers:
  - Arch: x64
    Url: https://statics.teams.cdn.office.net/production-windows-x64/1.3.00.4461/Teams_windows_x64.exe
    Sha256: 712f139d71e56bfb306e4a7b739b0e1109abb662dfa164192a5cfd6adb24a4e1
ManifestVersion: 0.1.0
```

* * *

> [!NOTE]
> インストーラーが .exe で、Nullsoft または Inno を使用してビルドされている場合は、これらの値を代わりに指定できます。 Nullsoft または Inno が指定されている場合、クライアントは、インストーラーに対してサイレントおよびサイレントの進行状況のインストール動作を自動的に設定します。

## <a name="installer-switches"></a>インストーラーのスイッチ

多くの場合、コマンド ラインからインストーラーに `-?` を渡すことにより、インストーラーで使用できるサイレント `Switches` を判別することができます。 さまざまなインストーラーの種類で使用できる一般的なサイレント `Swtiches` の一部を次に示します。

| インストーラー | コマンド  | ドキュメント |  
| :--- | :-- | :--- |  
| MSI | `/q` | [MSI のコマンドライン オプション](/windows/win32/msi/command-line-options) |
| InstallShield | `/s`  | [InstallShield のコマンドライン パラメーター](https://docs.flexera.com/installshield19helplib/helplibrary/IHelpSetup_EXECmdLine.htm) |
| Inno Setup | `/SILENT or /VERYSILENT` | [Inno Setup のドキュメント](https://jrsoftware.org/ishelp/) |
| Nullsoft | `/S` | [Nullsoft サイレント インストーラー/アンインストーラー](https://nsis.sourceforge.io/Docs/Chapter4.html#silent) |

## <a name="tips-and-best-practices"></a>ヒントとベスト プラクティス

* ソフトウェアを検索してインストールするときの最良のカスタマー エクスペリエンスを実現するために、必要なスキーマ以外にも任意のオプション項目を可能な限り多く含めることをお勧めします。 たとえば、`AppMoniker` フィールドはオプションです。 ただし、このフィールドを含めると、顧客が [search](../winget/search.md) コマンドを実行したときに `AppMoniker` 値に関連付けられた結果が表示されます (例: **Visual Studio Code** の **vscode**)。 指定された `AppMoniker` 値を持つアプリが 1 つしかない場合、顧客は完全修飾 ID ではなく、モニカーを指定してアプリケーションをインストールできます。
* `Id` は一意である必要があります。 同じパッケージ ID で複数の送信を行うことはできません。 スペースを使用すると、ユーザーが [winget](../index.md) クライアントを使用するときに `Id` の前後に引用符を入力しなければならないため、避けてください。
* 複数の発行元フォルダーを作成しないでください。 たとえば、"Contoso" というフォルダーが既にある場合に、"Contoso Ltd" というフォルダーを作成しないでください。 また、フォルダーを作成するときもスペースを使用しないようにします。
* 可能であれば、すべてのパッケージをサイレント インストールで送信する必要があります。 サイレント インストールをサポートしない実行可能ファイルがある場合、ユーザー エクスペリエンスが低下します。
* マニフェストでは、改行前の文字列の長さを 100 文字に制限します。
* 指定したバージョンのパッケージに、複数のインストーラーの種類がある場合は、それぞれの `Installers` の下に `InstallerType` のインスタンスを配置できます。
