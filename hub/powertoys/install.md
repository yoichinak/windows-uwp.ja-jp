---
title: PowerToys をインストールする
description: 実行可能ファイルまたはパッケージマネージャー (WinGet、Chocolatey、つい) を使用して、Windows 10 をカスタマイズするための一連のユーティリティである Powertoy をインストールします。
ms.date: 12/02/2020
ms.topic: quickstart
ms.localizationpriority: medium
ms.openlocfilehash: 0f051d44609653029a1bab40d2978eca91a4499c
ms.sourcegitcommit: 5dac88ad541b71ebe85b78951e6b357a3db176cc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/16/2020
ms.locfileid: "97612282"
---
# <a name="install-powertoys"></a>PowerToys をインストールする

Powertoy をインストールする方法は複数あります。

- **[Windows の実行可能ファイル](#install-with-windows-executable-file)** *(推奨)*
- [Windows パッケージマネージャー](#install-with-windows-package-manager-preview) *(プレビュー)*
- [コミュニティ主導のインストールツール](#community-driven-install-tools) *(公式にはサポートされていません)*

## <a name="requirements"></a>必要条件

- Windows 10 1803 (ビルド 17134) 以降。
- [.Net Core 3.1 デスクトップランタイム](https://dotnet.microsoft.com/download/dotnet-core/thank-you/runtime-desktop-3.1.4-windows-x64-installer)。 この要件は、Powertoy インストーラーによって処理されます。

コンピューターがこれらの要件を満たしていることを確認するには、windows 10 のバージョンとビルド番号を確認します。そのためには、 **⊞ Win** *(Windows キー)*  +  **D** を選択し、「 **winver**」と入力して、[ **OK]** を選択します。 (または、Windows コマンド プロンプトで `ver` コマンドを入力します)。 [**設定**] メニューでは、[最新の Windows バージョンに更新](ms-settings:windowsupdate)できます。

## <a name="install-with-windows-executable-file"></a>Windows 実行可能ファイルと共にインストールする

Windows 実行可能ファイルを使用して Powertoy をインストールするには、次のようにします。

1. [Microsoft Powertoy GitHub のリリースページ](https://github.com/microsoft/PowerToys/releases/)を参照してください。
2. 使用できる Powertoy の安定したバージョンと試験的なバージョンの一覧を参照します。
3. [ **アセット** ] ドロップダウンメニューを選択して、リリースのファイルを表示します。
4. ファイルを選択して、 `PowerToysSetup-0.##.#-x64.exe` powertoy 実行可能ファイルのインストーラーをダウンロードします。
5. ダウンロードが完了したら、実行可能ファイルを開き、インストールのプロンプトに従います。

**現在、このインストール方法をお勧めします。**

## <a name="install-with-windows-package-manager-preview"></a>Windows パッケージマネージャー (プレビュー) を使用してインストールする

Windows パッケージマネージャー (WinGet) プレビューを使用して Powertoy をインストールするには、次のようにします。

1. [Windows パッケージマネージャー](https://github.com/microsoft/winget-cli/releases)から powertoy をダウンロードします。
2. コマンドラインまたは PowerShell から次のコマンドを実行します。

```powershell
WinGet install powertoys
```

## <a name="community-driven-install-tools"></a>コミュニティ主導のインストールツール

これらのコミュニティ主導の代替インストール方法は公式にはサポートされておらず、Powertoy チームはこれらのパッケージを更新または管理しません。

### <a name="install-with-chocolatey"></a>Chocolatey を使用してインストールする

[Chocolatey](https://chocolatey.org/)を使用して powertoy をインストールするには、コマンドラインまたは PowerShell から次のコマンドを実行します。

```powershell
choco install powertoys
```

Powertoy をアップグレードするには、次のように実行します。

```powershell
choco upgrade powertoys
```

をインストールまたはアップグレードするときに問題が発生した場合は、 [Chocolatey.org の powertoy パッケージ](https://chocolatey.org/packages/powertoys) にアクセスして、 [Chocolatey のトリアージプロセス](https://chocolatey.org/docs/package-triage-process)に従います。

### <a name="install-with-scoop"></a>ついを使用してインストールする

[つい](https://scoop.sh/)を使用して powertoy をインストールするには、コマンドラインまたは PowerShell から次のコマンドを実行します。

```powershell
scoop install powertoys
```

Powertoy を更新するには、コマンドラインまたは PowerShell から次のコマンドを実行します。

```powershell
scoop update powertoys
```

のインストールまたは更新時に問題が発生した場合は、 [GitHub のついリポジトリ](https://github.com/lukesampson/scoop/issues)に問題を報告してください。
