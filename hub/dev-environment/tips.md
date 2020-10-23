---
title: Windows 10 での開発ワークフローを改善するためのヒント
description: Windows 10 での開発ワークフローを改善するためのヒント。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
ms.technology: windows-nodejs
keywords: Microsoft, Windows, 開発者, ヒント, パフォーマンス, WSL
ms.localizationpriority: medium
ms.date: 07/24/2020
ms.openlocfilehash: 7d02e3b46d6938532bbc7024e8840b976b2715a6
ms.sourcegitcommit: 861c381a31e4a5fd75f94ca19952b2baaa2b72df
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/19/2020
ms.locfileid: "92171153"
---
# <a name="tips-for-improving-performance-and-development-workflows"></a>パフォーマンスと開発ワークフローを改善するためのヒント

ワークフローをいっそう効率的で楽しいものにするのに役立ついくつかのヒントをまとめました。 共有したいヒントが他にもありますか。 上の [編集] ボタンを使用して Pull Request を提出するか、下の [フィードバック] ボタンを使用して問題を報告していただけば、こちらでそれを一覧に追加します。

> [!NOTE]
> Windows 10 上での開発に関連して、以下に挙げるいずれかのパフォーマンス上の問題が発生している場合:
> - Windows 上での開発ツール (コンパイラ、リンカーなど) の実行速度が予想より遅い。
> - Windows 上でのランタイム プラットフォーム (Node、.NET、Python など) の実行速度が他のプラットフォームより遅い。
> - アプリでファイル IO、ネットワーク、プロセス作成に関連するパフォーマンス上の問題が発生している。 
> 
> [Windows Developer (WinDev) Issues リポジトリ](https://github.com/microsoft/WinDev)で問題を登録して、お知らせください。

## <a name="use-shortcuts-to-open-a-project-in-vs-code-or-windows-file-explorer"></a>ショートカットを使用して、プロジェクトを VS Code または Windows エクスプローラーで開く

`code .` コマンドを使用し、コマンド ラインから VS Code を起動してプロジェクを開くことができます。または、Windows または WSL ディストリビューションから `explorer.exe .` を使用して、エクスプローラーでコマンド ラインからプロジェクト ディレクトリを開くことができます。 これが既定で動作しない場合は、VS Code の実行可能ファイルを PATH 環境変数に追加することが必要な場合があります。 詳細については、「[コマンド ラインからの起動](https://code.visualstudio.com/docs/editor/command-line#_launching-from-command-line)」を参照してください。

![エクスプローラーのスクリーンショット](../images/wsl-file-explorer.png)

## <a name="use-the-credential-manager-to-your-streamline-authentication-process"></a>資格情報マネージャーを使用して、認証プロセスを効率化する

バージョン管理とコラボレーションに Git を使用している場合は、[Git Credential Manager を設定](/windows/wsl/tutorials/wsl-git#git-credential-manager-setup)してトークンを Windows 資格情報マネージャーに格納することで、認証プロセスを効率化できます。 また、プロジェクトに [.gitignore ファイルを追加する](/windows/wsl/tutorials/wsl-git#adding-a-git-ignore-file)ことをお勧めします。

## <a name="use-wsl-for-testing-your-production-pipeline-before-deploying-to-the-cloud"></a>クラウドにデプロイする前に、WSL を使用して運用パイプラインをテストする

Linux 用 Windows サブシステムを使用すると、開発者は、従来の仮想マシンまたはデュアルブート セットアップのオーバーヘッドなしで、ほとんどのコマンド ライン ツール、ユーティリティ、アプリケーションを含む GNU/Linux 環境を変更せずそのまま Windows 上で直接実行できます。

WSL は開発者を対象としており、内部開発ループの一部としての使用を目的としています。 たとえば、Sam が CI/CD パイプライン (継続的インテグレーションおよび継続的デリバリー) を作成しており、クラウドにデプロイする前に、まずローカル コンピューター (ノート PC) でそれをテストしたいと思っているとします。 Sam は WSL (速度とパフォーマンスを向上させるには WSL 2) を有効にして、ローカル (ノート PC) で本物の Linux Ubuntu インスタンスと、任意の Bash コマンドとツールを使用することができます。 開発パイプラインをローカルで検証したら、Sam はその CI/CD パイプラインをクラウド (Azure) にプッシュできます。そうするには、これを Docker コンテナーに格納し、そのコンテナーをクラウド インスタンスにプッシュします。こうして、運用に対応した Ubuntu VM 上でこれを実行できます。

WSL を使用する他の方法については、[WSL 2 でのタブとスペースの比較のエピソード](https://channel9.msdn.com/Shows/Tabs-vs-Spaces/WSL2-Code-faster-on-the-Windows-Subsystem-for-Linux)に関する動画をご覧ください。

## <a name="improve-performance-speed-for-wsl-by-not-crossing-over-file-systems"></a>ファイル システムをまたがないことで、WSL のパフォーマンスの速度を改善する

Windows と Linux 用 Windows サブシステムの両方を使用している場合は、次の 2 つのファイル システムがインストールされます: NTSF (Windows) と WSL (お使いの Linux ディストリビューション)。 パフォーマンスを向上させるには、使用しているツールと同じシステムにプロジェクト ファイルを格納してください。 詳細については、[パフォーマンスを向上させるための適切なファイル システムの選択](/windows/wsl/compare-versions#use-the-linux-file-system-for-faster-performance)に関するページを参照してください。

## <a name="improve-build-speeds-by-adding-windows-defender-exclusions"></a>Windows Defender の除外を追加して、ビルドの速度を改善する

Windows Defender の設定を更新し、セキュリティ脅威のスキャンを避けるのに十分であると信じられるプロジェクト フォルダーまたはファイルの種類の除外を追加することで、ビルドの速度を向上させることができます。 詳細については、「[パフォーマンスを向上させるための Windows Defender 設定の更新](../android/defender-settings.md)」を参照してください。

![Windows Defender のスクリーンショット](../images/windows-defender-exclusions.png)

## <a name="launch-all-your-command-lines-in-windows-terminal-at-once"></a>すべてのコマンド ラインを Windows ターミナルで一度に起動する

* [Windows ターミナルのコマンド ライン引数](/windows/terminal/command-line-arguments?tabs=powershell#multiple-panes)を使用することで、PowerShell、Ubuntu、Azure CLI など、複数のコマンド ラインをすべて、単一のウィンドウの複数のペインで起動することができます。 [Windows ターミナル](/windows/terminal/get-started)、[WSL/Ubuntu](/windows/wsl/install-win10)、[Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest) をインストールした後、PowerShell で次のコマンドを入力して、新しい複数ペイン ウィンドウで 3 つをすべて開きます。

    ```powershell
    wt -p "Command Prompt" `; split-pane -p "Windows PowerShell" `; split-pane -H wsl.exe
    ```

## <a name="share-your-tips"></a>ヒントの共有

Windows を使用している他の開発者がワークフローを改善できるヒントをお持ちですか。 特定のトピックについてのヒントを追加したい場合は、[pull request を送信して](https://github.com/MicrosoftDocs/windows-uwp/edit/docs/hub/dev-environment/overview.md)ページにヒントを追加するか、[イシューを報告](https://github.com/MicrosoftDocs/windows-uwp/issues/new?title=&body=%0A%0A%5BEnter%20feedback%20here%5D%0A%0A%0A---%0A%23%23%23%23%20Document%20Details%0A%0A%E2%9A%A0%20*Do%20not%20edit%20this%20section.%20It%20is%20required%20for%20docs.microsoft.com%20%E2%9E%9F%20GitHub%20issue%20linking.*%0A%0A*%20ID%3A%207779352b-7b4e-dad8-7c1b-b9aba2c5e561%0A*%20Version%20Independent%20ID%3A%20a5b81b80-87a1-b6e2-8936-baf6c1a0b9c5%0A*%20Content%3A%20%5BSet%20up%20your%20Windows%2010%20development%20environment%5D(https%3A%2F%2Fdocs.microsoft.com%2Fen-us%2Fwindows%2Fdev-environment%2Foverview)%0A*%20Content%20Source%3A%20%5Bhub%2Fdev-environment%2Foverview.md%5D(https%3A%2F%2Fgithub.com%2FMicrosoftDocs%2Fwindows-uwp%2Fblob%2Fdocs%2Fhub%2Fdev-environment%2Foverview.md)%0A*%20Product%3A%20**dev-environment**%0A*%20Technology%3A%20**windows-nodejs**)してください。

対応が必要なパフォーマンス関連の問題がありますか。 新しい [WinDev の問題についてのリポジトリ](https://github.com/microsoft/windev)に報告してください。

開発者の皆さん、ありがとうございます。 フィードバックを参考にして、皆さんのエクスペリエンスを向上すべく努力しています。
