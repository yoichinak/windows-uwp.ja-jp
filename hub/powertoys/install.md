---
title: PowerToys をインストールする
description: 実行可能ファイルまたはパッケージマネージャー (WinGet、Chocolatey、つい) を使用して、Windows 10 をカスタマイズするための一連のユーティリティである Powertoy をインストールします。
ms.date: 12/02/2020
ms.topic: quickstart
ms.localizationpriority: medium
ms.openlocfilehash: 2e6a585a6acd6bc9ec209b07e101daf8d844be34
ms.sourcegitcommit: 77af97719a439f5e73a6109b42fd3110bcb2843b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/08/2021
ms.locfileid: "107218125"
---
# <a name="install-powertoys"></a>PowerToys をインストールする

> [!WARNING]
> Powertoy v 0.37 以降では、Windows 10 v1903 以上が必要になります。 以前のバージョンの Windows をサポートする v1 の設定は、v 0.37 で削除される予定です。

次にリンクされている Windows 実行可能ボタンを使用して Powertoy をインストールすることをお勧めしますが、パッケージマネージャーを使用する場合は、別のインストール方法も示しています。

## <a name="install-with-windows-executable-file"></a>Windows 実行可能ファイルと共にインストールする

> [!div class="nextstepaction"]
> [PowerToys をインストールする](https://aka.ms/installpowertoys)

Windows 実行可能ファイルを使用して Powertoy をインストールするには、次のようにします。

1. [Microsoft Powertoy GitHub のリリースページ](https://github.com/microsoft/PowerToys/releases/)を参照してください。
2. 使用できる Powertoy の安定したバージョンと試験的なバージョンの一覧を参照します。
3. [ **アセット** ] ドロップダウンメニューを選択して、リリースのファイルを表示します。
4. ファイルを選択して、 `PowerToysSetup-0.##.#-x64.exe` powertoy 実行可能ファイルのインストーラーをダウンロードします。
5. ダウンロードが完了したら、実行可能ファイルを開き、インストールのプロンプトに従います。

## <a name="requirements"></a>要件

- Windows 10 1803 (ビルド 17134) 以降。
- [.Net Core 3.1 デスクトップランタイム](https://dotnet.microsoft.com/download/dotnet-core/thank-you/runtime-desktop-3.1.4-windows-x64-installer)。 この要件は、Powertoy インストーラーによって処理されます。
- 現在、x64 アーキテクチャがサポートされています。 ARM および x86 のサポートは、今後ご利用いただけるようになりました。

コンピューターがこれらの要件を満たしていることを確認するには、windows 10 のバージョンとビルド番号を確認します。そのためには、 **⊞ Win** *(Windows キー)*  +  **R** を選択し、「 **winver**」と入力して、[ **OK]** を選択します。 (または、Windows コマンド プロンプトで `ver` コマンドを入力します)。 [**設定**] メニューでは、[最新の Windows バージョンに更新](ms-settings:windowsupdate)できます。

## <a name="alternative-install-methods"></a>別のインストール方法

<!--  - **[Windows executable .exe file](#install-with-windows-executable-file)** *(Recommended)* -->
- [Windows パッケージマネージャー](#install-with-windows-package-manager-preview) *(プレビュー)*
- [コミュニティ主導のインストールツール](#community-driven-install-tools) *(公式にはサポートされていません)*

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
scoop bucket add extras
scoop install powertoys
```

Powertoy を更新するには、コマンドラインまたは PowerShell から次のコマンドを実行します。

```powershell
scoop update powertoys
```

のインストールまたは更新時に問題が発生した場合は、 [GitHub のついリポジトリ](https://github.com/lukesampson/scoop/issues)に問題を報告してください。

## <a name="post-install"></a>インストール後

Powertoy を正常にインストールすると、[概要] ウィンドウが表示され、使用可能な各ユーティリティの概要が示されます。
