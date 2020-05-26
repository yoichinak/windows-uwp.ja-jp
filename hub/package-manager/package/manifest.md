---
title: パッケージ マニフェストを作成する
description: ''
author: denelon
ms.author: denelon
ms.date: 04/29/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 054c8cf7fc104b78f0397f4d1536e1130668f4f8
ms.sourcegitcommit: 645cb099128072cfc905ec80b38bd280b98c9037
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/22/2020
ms.locfileid: "83825143"
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

マニフェスト内の項目の完全な一覧と説明については、[https://github.com/microsoft/winget-cli](https://github.com/microsoft/winget-cli) リポジトリのマニフェスト仕様を参照してください。

### <a name="minimal-required-schema"></a>最低限必要なスキーマ

#### <a name="minimal-required-schema"></a>[最低限必要なスキーマ](#tab/minschema/)

```yaml
Id: string # publisher.package format
Publisher: string # the name of the publisher
Name: string # the name of the application
Version: string # version numbering format
License: string # the open source license or copyright
InstallerType: string # enumeration of supported installer types (exe, msi, msix, inno, wix, nullsoft, appx)
Installers:
  - Arch: string # enumeration of supported architectures
  - URL: string # path to download installation file
  - Sha256: string # SHA256 calculated from installer
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
Id: string # publisher.package format
Publisher: string # the name of the publisher
Name: string # the name of the application
AppMoniker: string # the common name someone may use to search for the package
Version: string # version numbering format for package version
Channel: string # a string representing the flight ring
License: string # the open source license or copyright
LicenseUrl: string # valid secure URL to license
MinOSVersion: string # version numbering format for minimum version of Windows supported
Description: string # description of the package
Homepage: string # valid secure URL for the package
Tags: list # additional strings a user would use to search for the package
FileExtensions: list # list of file extensions the package could support
Protocols: list # list of protocols the package provides a handler for
Commands: list # list of commands or aliases the user would use to run the package
InstallerType: string # enumeration of supported installer types (exe, msi, msix)
Custom: string # custom switches passed to the installer
Silent: string # switches passed to the installer for silent installation
SilentWithProgress: string # switches passed to the installer for non-interactive install
Interactive: string # experimental
Language: string # experimental
Log: string # specifies log redirection switches and path
InstallLocation: string # specifies alternate location to install package
Installers: # nested map of keys for specific installer
  - Arch: string # enumeration of supported architectures
  - URL: string # path to download installation file
  - Sha256: string # SHA256 calculated from installer
  - SignatureSha256: string # SHA256 calculated from signature file's hash of MSIX file
  - Switches: # collection of entries to override root keys
  - Scope: string # experimental
  - SystemAppId: string # experimental
Localization: # nested map of keys for localization
  - Language: string # locale for display fields and localized URLs
ManifestVersion: string # version number format for manifest version
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

## <a name="tips-and-best-practices"></a>ヒントとベスト プラクティス

* ソフトウェアを検索してインストールするときの最良のカスタマー エクスペリエンスを実現するために、必要なスキーマ以外にも任意のオプション項目を可能な限り多く含めることをお勧めします。 たとえば、`AppMoniker` フィールドはオプションです。 ただし、このフィールドを含めると、顧客が [search](../winget/search.md) コマンドを実行したときに `AppMoniker` 値に関連付けられた結果が表示されます (例: **Visual Studio Code** の **vscode**)。 指定された `AppMoniker` 値を持つアプリが 1 つしかない場合、顧客は完全修飾 ID ではなく、モニカーを指定してアプリケーションをインストールできます。
* `Id` は一意である必要があります。 同じパッケージ ID で複数の送信を行うことはできません。 スペースを使用すると、ユーザーが [winget](../index.md) クライアントを使用するときに `Id` の前後に引用符を入力しなければならないため、避けてください。
* 複数の発行元フォルダーを作成しないでください。 たとえば、"Contoso" というフォルダーが既にある場合に、"Contoso Ltd" というフォルダーを作成しないでください。 また、フォルダーを作成するときもスペースを使用しないようにします。
* 可能であれば、すべてのパッケージをサイレント インストールで送信する必要があります。 サイレント インストールをサポートしない実行可能ファイルがある場合、ユーザー エクスペリエンスが低下します。
* マニフェストでは、改行前の文字列の長さを 100 文字に制限します。
* 指定したバージョンのパッケージに、複数のインストーラーの種類がある場合は、それぞれの `Installers` の下に `InstallerType` のインスタンスを配置できます。
