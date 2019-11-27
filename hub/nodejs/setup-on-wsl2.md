---
title: WSL 2 で NodeJS を設定する
description: Windows Subsystem for Linux (WSL) で node.js 開発環境を設定するためのガイドです。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: NodeJS、node.js、windows 10、microsoft、learning NodeJS、windows 上のノード、wsl のノード、windows 上の linux 上のノード、windows 上のノードのインストール、windows 上のノードのインストール、windows 上の NodeJS を使用した開発、windows 上のノードのインストール、WSL へのノードのインストール、Windows 上の NodeJSLinux 用サブシステム
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: e5875f0bf7ce73d3615aa131d57c2384c73dd8a1
ms.sourcegitcommit: 60d2d15dd0d365f82e4e90e4bc34b40cf5b4a247
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/17/2019
ms.locfileid: "72517842"
---
# <a name="set-up-your-nodejs-development-environment-with-wsl-2"></a>WSL 2 を使用して node.js 開発環境を設定する

Windows Subsystem for Linux (WSL) を使用して node.js 開発環境をセットアップする方法については、次のステップバイステップガイドを参照してください。 現在、このガイドでは、 [Wsl 2](https://devblogs.microsoft.com/commandline/wsl-2-is-now-available-in-windows-insiders/)をインストールして使用するために、Windows Insider Preview ビルドをインストールして実行する必要があります。 WSL 2 では、特に node.js に関して、WSL 1 よりも大幅な速度とパフォーマンスの向上が図られています。 Node.js web 開発用の多くの npm モジュールとチュートリアルは、Linux ユーザー向けに記述されており、Linux ベースのパッケージツールとインストールツールを使用します。 ほとんどの web アプリも Linux にデプロイされるため、WSL 2 を使用すると、開発環境と運用環境の間で一貫性が確保されます。

> [!NOTE]
> Windows で node.js を直接使用することを検討している場合、または Windows Server 運用環境の使用を計画している場合は、 [windows で直接 node.js 開発環境を設定](./setup-on-windows.md)するためのガイドを参照してください。

## <a name="install-windows-10-insider-preview-build"></a>Windows 10 Insider Preview ビルドをインストールする

1. **[最新バージョンの Windows 10 をインストール](https://www.microsoft.com/software-download/windows10)** する: 更新アシスタントをダウンロードするには、 **[今すぐ更新]** を選択します。 ダウンロードが完了したら、update assistant を開いて、最新バージョンの Windows を現在実行しているかどうかを確認します。それ以外の場合は、アシスタントウィンドウ内で **[今すぐ更新]** を選択して、コンピューターを更新します。 *(Windows 10 の最新バージョンを実行している場合、この手順は省略可能です)。*

    ![Windows Update アシスタント](../images/windows-update-assistant2019.png)

2. Windows insider program ウィンドウ内の [ **[Start > Settings] >](ms-settings:windowsinsider)** windows insider program] の順に移動し、 **[開始]** 、 **[アカウントのリンク]** の順に選択します。

    ![Windows Insider プログラムの設定](../images/windows-insider-program-settings.png)

3. **[Windows insider として登録](https://insider.windows.com/getting-started/#register)** する: insider プログラムに登録していない場合は、 [Microsoft アカウント](https://account.microsoft.com/account)で登録する必要があります。

    ![Windows Insider の登録](../images/windows-insider-account.png)

4. **高速リング**更新を受信するか **、次の Windows リリースコンテンツにスキップ**するかを選択します。 確認し、**後で再起動**することを選択します。 再起動する前に、いくつかの追加設定を変更する必要があります。

    ![Windows Insider Fast リング](../images/windows-insider-fast.png)

## <a name="enable-windows-subsystem-for-linux-and-virtual-machine-platform"></a>Windows Subsystem for Linux と仮想マシンプラットフォームを有効にする

1. **Windows の設定**のままで、 **[windows の機能の有効化または無効化**] を検索します。
2. **[Windows の機能]** の一覧が表示されたら、 **[仮想マシンプラットフォーム]** と **[windows Subsystem for Linux]** をスクロールして、両方のチェックボックスがオンになっていることを確認し、[ **OK]** を選択します。
3. メッセージが表示されたら、コンピューターを再起動します。

    ![Windows の機能を有効にする](../images/windows-feature-settings.png)

## <a name="install-a-linux-distribution"></a>Linux ディストリビューションをインストールする

WSL で実行できる Linux ディストリビューションがいくつかあります。 お気に入りは、Microsoft Store で見つけてインストールできます。 [Ubuntu 18.04 LTS](https://www.microsoft.com/store/productId/9N9TNGVNDL3Q)は、現在、人気、およびサポートされているため、開始することをお勧めします。

1. この[Ubuntu 18.04 LTS](https://www.microsoft.com/store/productId/9N9TNGVNDL3Q)リンクを開き、Microsoft Store を開いて、 **[取得]** を選択します。 *(これはかなり大きなダウンロードであり、インストールに時間がかかる場合があります)。*

2. ダウンロードが完了したら、Microsoft Store から **[起動]** を選択するか、 **[スタート]** メニューに「Ubuntu 18.04 LTS」と入力して起動します。

3. 最初にディストリビューションを実行するときに、アカウント名とパスワードの作成を求められます。 この後、既定では、このユーザーとして自動的にサインインされます。 任意のユーザー名とパスワードを選択できます。 Windows ユーザー名には影響しません。

    ![Microsoft Store での Linux ディストリビューション](../images/store-linux-distros.png)

現在使用している Linux ディストリビューションを確認するには、`lsb_release -dc`を入力します。 Ubuntu ディストリビューションを更新するには、: `sudo apt update && sudo apt upgrade`を使用します。 最新のパッケージがあることを確認するために、定期的に更新することをお勧めします。 Windows は、この更新プログラムを自動的に処理しません。 Microsoft Store で利用可能なその他の Linux ディストリビューション、代替のインストール方法、トラブルシューティングのリンクについては、「windows [10 用 Windows Subsystem For Linux インストールガイド](https://docs.microsoft.com/windows/wsl/install-win10)」を参照してください。

## <a name="install-wsl-2"></a>WSL 2 のインストール

WSL 2 は、WSL の[アーキテクチャの新しいバージョン](https://docs.microsoft.com/windows/wsl/wsl2-about)です。 Linux ディストリビューションが Windows と対話する方法を変更し、パフォーマンスを向上させ、システムコールの完全な互換性を追加します。

1. PowerShell でコマンド: `wsl -l` を入力して、コンピューターにインストールされている WSL ディストリビューションの一覧を表示します。 これで、この一覧に Ubuntu-18.04 が表示されます。
2. ここで、コマンドを入力して、`wsl --set-version Ubuntu-18.04 2`、WSL 2 を使用するように Ubuntu のインストールを設定します。
3. インストールされている各ディストリビューションのバージョンが、with: `wsl --list --verbose` (または `wsl -l -v`) で使用されていることを確認します。

    ![Linux 用 Windows サブシステムのセットバージョン](../images/wsl-versions.png)

> [!TIP]
> 同じ手順 (PowerShell を使用) に従って、WSL 2 にインストールした Linux ディストリビューションを設定することができます。その場合は、"Ubuntu-18.04" を対象とするインストール済みのディストリビューションの名前に変更します。 WSL 1 に戻すには、上記と同じコマンドを実行しますが、' 2 ' を ' 1 ' に置き換えます。  また、次のように入力して、新しくインストールされたディストリビューションの既定値として WSL 2 を設定することもできます。 `wsl --set-default-version 2`します。

## <a name="install-nvm-nodejs-and-npm"></a>Nvm、node.js、nvm をインストールします。

Node.js をインストールするには、複数の方法があります。 バージョンが非常に迅速に変更されるため、バージョンマネージャーを使用することをお勧めします。 多くの場合、作業しているさまざまなプロジェクトのニーズに基づいて、複数のバージョンを切り替える必要があります。 Node.js の複数のバージョンをインストールする最も一般的な方法は、ノードバージョンマネージャー (nvm と呼ばれます) です。 ここでは、nvm をインストールする手順について説明した後、node.js と Node Package Manager (nvm) をインストールするために使用します。 次のセクションでは、[別のバージョンのマネージャー](#alternative-version-managers)についても考慮する必要があります。

> [!IMPORTANT]
> 異なる種類のインストールでバージョンマネージャーをインストールする前に、オペレーティングシステムから node.js または npm の既存のインストールをすべて削除することをお勧めします。これにより、予期しない競合が発生する可能性があります。 たとえば、Ubuntu の `apt-get` コマンドと共にインストールできるノードのバージョンは、現在古くなっています。 以前のインストールの削除については、「 [ubuntu から nodejs を削除する方法](https://askubuntu.com/questions/786015/how-to-remove-nodejs-from-ubuntu-16-04)」を参照してください。)

1. Ubuntu 18.04 のコマンドラインを開きます。
2. 次のコマンドを使用して、cURL (インターネットからコンテンツをダウンロードするために使用するツール) をインストールします。 `sudo apt-get install curl`
3. 次のものを使用して nvm をインストールし `curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.34.0/install.sh | bash`
4. インストールを確認するには、次のように入力します。 `command -v nvm`...' nvm ' が返されます。 "コマンドが見つかりません" または応答がない場合は、現在のターミナルを閉じて再度開き、もう一度やり直してください。 [詳細については、nvm github リポジトリを参照して](https://github.com/nvm-sh/nvm)ください。
5. 現在インストールされているノードのバージョンを一覧表示します (この時点では何も指定できません): `nvm ls`

    ![ノードバージョンが表示されていない NVM リスト](../images/nvm-no-node.png)

6. Node.js の現在のリリースをインストールします (最新の機能の改善をテストするために、問題が発生する可能性が高くなります)。 `nvm install node`
7. Node.js の最新の安定した LTS リリースをインストールする (推奨): `nvm install --lts`
8. インストールされているノードのバージョンを一覧表示する: `nvm ls`...これで、先ほどインストールした2つのバージョンが表示されます。

    ![LTS と現在のノードバージョンを示す NVM リスト](../images/nvm-node-installed.png)

9. Node.js がインストールされており、現在の既定のバージョンが: `node --version`であることを確認します。 次に、: `npm --version` (`which node` または `which npm` を使用して、既定のバージョンで使用されているパスを確認することもできます) の npm もご確認ください。
10. プロジェクトに使用する node.js のバージョンを変更するには、新しいプロジェクトディレクトリ `mkdir NodeTest`を作成し、`cd NodeTest`ディレクトリを入力します。次に `nvm use node` を入力して現在のバージョンに切り替えるか、または `nvm use --lts` を使用して LTS のバージョンに切り替えます。 また、`nvm use v8.2.1`のように、インストールしたその他のバージョンには特定の番号を使用することもできます。 (使用可能な node.js のすべてのバージョンを一覧表示するには、コマンド: `nvm ls-remote`) を使用します。

> [!TIP]
> NVM を使用して node.js と NVM をインストールする場合は、SUDO コマンドを使用して新しいパッケージをインストールする必要はありません。

> [!NOTE]
> 発行時に、NVM v 0.34.0 が使用可能な最新バージョンでした。 [GitHub プロジェクトページで NVM の最新リリース](https://github.com/nvm-sh/nvm)を確認し、上記のコマンドを調整して最新バージョンを含めることができます。
CURL を使用して新しいバージョンの NVM をインストールすると、古いものが置き換えられ、NVM で使用したノードのバージョンはそのまま残ります。 たとえば次のようになります。`curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.0/install.sh | bash`

## <a name="alternative-version-managers"></a>代替バージョンマネージャー

Nvm は現在最も人気のあるノードのバージョンマネージャーですが、いくつかの選択肢があります。

- [n](https://www.npmjs.com/package/n#installation)は、やや異なるコマンドを使用して同じことを実現し、bash スクリプトではなく `npm` を使用してインストールされる、長期的な `nvm` の代替手段です。
- [fnm](https://github.com/Schniz/fnm#using-a-script)は、新しいバージョンのマネージャーであり、`nvm`よりもはるかに高速であることが要求されています。 ( [Azure Pipelines](https://docs.microsoft.com/azure/devops/pipelines/get-started/what-is-azure-pipelines?view=azure-devops)も使用します)。
- [Volta](https://github.com/volta-cli/volta#installing-volta)は、より高速でクロスプラットフォームのサポートを要求する LinkedIn チームの新しいバージョンマネージャーです。
- [asdf-vm](https://asdf-vm.com/#/core-manage-asdf-vm)は、1つの ike gvm、nvm、rbenv & pyenv (その他) など、複数の言語用の単一の CLI です。
- [nvs](https://github.com/jasongin/nvs) (ノードバージョンスイッチャー) は、 [VS Code と統合](https://github.com/jasongin/nvs/blob/master/doc/VSCODE.md)できる、クロスプラットフォームの `nvm` 代替です。

## <a name="install-your-favorite-code-editor"></a>お気に入りのコードエディターをインストールする

Node.js プロジェクト用の**リモート WSL 拡張機能**で**Visual Studio Code**を使用することをお勧めします。 これにより、VS Code が "クライアント-サーバー" アーキテクチャに分割され、Windows コンピューター上で実行されるクライアント (ユーザーインターフェイス) とサーバー (コード、Git、プラグインなど) がリモートで実行されます。

- Linux ベースの Intellisense とインライン作成がサポートされています。
- プロジェクトは、Linux で自動的にビルドされます。
- Linux で実行されているすべての拡張機能 ([けば、NPM Intellisense、ES6 スニペット](https://marketplace.visualstudio.com/items?itemName=waderyan.nodejs-extension-pack)など) を使用できます。

ターミナルベースのテキストエディター (vim、emacs、nano) は、コンソール内ですぐに変更を加える場合にも役立ちます。 ([この記事](https://medium.com/linode-cube/emacs-nano-or-vim-choose-your-terminal-based-text-editor-wisely-8f3826c92a68)では、違いと、それぞれの使用方法について説明しています)。

> [!NOTE]
> 一部の GUI エディター (Atom、Sublime テキスト、Eclipse) は、WSL 共有ネットワークの場所 (\\wsl $ \Ubuntu\home\) にアクセスするときに問題が発生する可能性があります。また、Windows ツールを使用して Linux ファイルをビルドしようとしますが、これは不要な場合があります。 この互換性は、VS Code のリモート WSL 拡張機能によって処理されます。

VS Code とリモート WSL 拡張機能をインストールするには、次のようにします。

1. [Windows 用の VS Code をダウンロードしてインストール](https://code.visualstudio.com)します。 VS Code は Linux でも使用できますが、Windows Subsystem for Linux は GUI アプリをサポートしていないため、Windows にインストールする必要があります。 心配しなくても、リモートの WSL 拡張機能を使用して Linux コマンドラインやツールと統合できます。

2. VS Code に[リモート WSL 拡張機能](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl)をインストールします。 これにより、統合開発環境として WSL を使用し、互換性とパスを処理することができます。 [詳しくはこちらをご覧ください](https://code.visualstudio.com/docs/remote/remote-overview)。

> [!IMPORTANT]
> 既に VS Code がインストールされている場合は、[リモート WSL 拡張機能](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl)をインストールするために、 [1.35 がリリース](https://code.visualstudio.com/updates/v1_35)以降であることを確認する必要があります。 リモート WSL 拡張機能を使用せずに VS Code で WSL を使用することはお勧めしません。オートコンプリート、デバッグ、インライン処理などのサポートが失われるためです。楽しい事実: この WSL 拡張機能は $HOME/.vscode-server/extensions. にインストールされます。

### <a name="helpful-vs-code-extensions"></a>便利な VS Code 拡張機能

VS Code には、node.js の開発に関する多くの機能が用意されていますが、 [Node.js 拡張パック](https://marketplace.visualstudio.com/items?itemName=waderyan.nodejs-extension-pack)でのインストールを検討すると便利な拡張機能がいくつかあります。 これらのすべてをインストールするか、最も役に立つと思われるものを選択して選択します。

Node.js 拡張パックをインストールするには:

1. VS Code で **[拡張]** ウィンドウ (Ctrl + Shift + X) を開きます。

    [拡張] ウィンドウが3つのセクションに分割されました (リモート WSL 拡張機能がインストールされているため)。
    - "Local Installed": Windows オペレーティングシステムで使用するためにインストールされる拡張機能。
    - "WSL: Ubuntu-18.04-Installed": Ubuntu オペレーティングシステム (WSL) で使用するためにインストールされる拡張機能。
    - "推奨": 現在のプロジェクトのファイルの種類に基づいて、VS Code によって推奨される拡張機能。

    ![VS Code 拡張機能のローカルとリモートの比較](../images/vscode-extensions-local-remote.png)

2. [拡張機能] ウィンドウの上部にある [検索] ボックスに、**ノード拡張パック**(または目的の拡張機能の名前) を入力します。 この拡張機能は、現在のプロジェクトを開いている場所に応じて、VS Code のローカルインスタンスまたは WSL インスタンスのいずれかにインストールされます。 VS Code ウィンドウの左下隅にある [リモート] リンク (緑色) を選択するとわかります。 リモート接続を開いたり閉じたりするためのオプションが表示されます。 Node.js 拡張機能を "WSL: Ubuntu-18.04" 環境にインストールします。

    ![リモートリンクの VS Code](../images/wsl-remote-extension.png)

さらに、次のような拡張機能を使用することもできます。

- [Chrome のデバッガー](https://code.visualstudio.com/blogs/2016/02/23/introducing-chrome-debugger-for-vs-code): node.js を使用してサーバー側で開発を完了すると、クライアント側を開発してテストする必要があります。 この拡張機能により、VS Code エディターと Chrome ブラウザーのデバッグサービスが統合され、さらに効率的になります。
- [他のエディターからの Keymaps](https://marketplace.visualstudio.com/search?target=VSCode&category=Keymaps&sortBy=Downloads): これらの拡張機能は、別のテキストエディター (Atom、Sublime、Vim、EMacs、メモ帳 + + など) から移行している場合に、環境を自宅で使用しやすくするために役立ちます。
- [設定の同期](https://marketplace.visualstudio.com/items?itemName=Shan.code-settings-sync): GitHub を使用して、複数のインストール間で VS Code 設定を同期できます。 別のコンピューターで作業している場合は、これによって環境の整合性を保つことができます。

## <a name="install-windows-terminal-optional"></a>Windows ターミナルをインストールする (省略可能)

新しい Windows ターミナルでは、複数のタブが有効になり (コマンドプロンプト、PowerShell、または複数の Linux ディストリビューションをすばやく切り替えることができます)、カスタムキーバインド (タブを開いたり閉じたりするための独自のショートカットキーを作成する、コピーと貼り付けなど)、絵文字☺、カスタムテーマ (配色、フォントスタイルとサイズ、背景画像、 [詳しくはこちらをご覧ください](https://devblogs.microsoft.com/commandline/)。

1. [Microsoft Store で Windows ターミナル (プレビュー)](https://www.microsoft.com/store/apps/9n0dx20hk701)を取得する: ストアを使用してをインストールすることにより、更新プログラムは自動的に処理されます。

2. インストールが完了したら、Windows ターミナルを開き、 **[設定]** を選択して `profile.json` ファイルを使用してターミナルをカスタマイズします。 [詳細については、「Windows ターミナル設定の編集](https://github.com/microsoft/terminal/blob/master/doc/user-docs/UsingJsonSettings.md)」を参照してください。

    ![Windows ターミナルの設定](../images/windows-terminal-settings.png)

## <a name="set-up-git-optional"></a>Git を設定する (省略可能)

他のユーザーと共同作業を行う場合、または (GitHub などの) オープンソースサイトでプロジェクトをホストする場合、VS Code は[Git を使用したバージョン管理](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support)をサポートします。 VS Code の [ソース管理] タブでは、すべての変更が追跡され、UI には一般的な Git コマンド (add、commit、push、pull) が組み込まれています。

1. Git は、Windows Subsystem for Linux ディストリビューションと共にインストールされます。ただし、git 構成ファイルを設定する必要があります。 これを行うには、ターミナルで次のように入力して、`git config --global user.email "youremail@domain.com"``git config --global user.name "Your Name"` します。 まだ Git アカウントを持っていない場合は、 [GitHub でサインアップ](https://github.com/join)できます。 前に Git を使用したことがない場合は、 [GitHub のガイド](https://guides.github.com/)を参考にしてください。 Git 構成を編集する必要がある場合は、nano: `nano ~/.gitconfig`のような組み込みのテキストエディターを使用して実行できます。

2. ノードプロジェクトには、ファイルを追加することをお勧めし[ます](https://help.github.com/en/articles/ignoring-files)。 [Node.js の GitHub の既定のテンプレート](https://github.com/github/gitignore/blob/master/Node.gitignore)は次のとおりです。 [Github web サイトを使用して新しいリポジトリを作成](https://help.github.com/articles/create-a-repo)することを選択した場合は、リポジトリを初期化するためのチェックボックスがあります。このファイルは、node.js プロジェクト用にセットアップされています。また、必要に応じてライセンスを追加するためのオプションもあります。

## <a name="next-steps"></a>次のステップ

これで、node.js 開発環境がセットアップされました。 Node.js 環境の使用を開始するには、次のチュートリアルのいずれかを試すことを検討してください。

- [初心者向け node.js を使ってみる](./beginners.md)
- [Windows での node.js web フレームワークの概要](./web-frameworks.md)
- [データベースへの node.js アプリの接続の概要](./databases.md)
- [Node.js で Docker コンテナーを使ってみる](./containers.md)
