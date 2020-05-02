---
title: WSL 2 上で NodeJS を設定する
description: Linux 用 Windows サブシステム (WSL) 上で Node.js 開発環境を設定するのに役立つガイド。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: NodeJS, Node.js, Windows 10, Microsoft, ラーニング NodeJS, Windows 上のノード, WSL 上のノード, Windows 上の Linux 上のノード, Windows 上のインストール ノード, NodeJS と VS Code, Windows 上のノードでの開発, Windows 上の NodeJS での開発, WSL 上のインストール ノード, Linux 用 Windows サブシステム上の NodeJS
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: c987f5bea387c630a1b9ef23c928d7a1bb8fadfc
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/29/2020
ms.locfileid: "75835380"
---
# <a name="set-up-your-nodejs-development-environment-with-wsl-2"></a>WSL 2 を使用して Node.js 開発環境を設定する

以下に示すのは、Linux 用 Windows サブシステム (WSL) を使用して Node.js 開発環境を設定するのに役立つステップバイステップ ガイドです。 現在、このガイドでは [WSL 2](https://devblogs.microsoft.com/commandline/wsl-2-is-now-available-in-windows-insiders/) をインストールして使用するために、Windows Insider Preview ビルドをインストールして実行する必要があります。 WSL 2 の速度とパフォーマンスは、特に Node.js に関して、WSL 1 よりも大幅に向上しています。 Node.js Web 開発用の npm モジュールとチュートリアルの多くは Linux ユーザー向けに記述されており、Linux ベースのパッケージ ツールとインストール ツールを使用しています。 ほとんどの Web アプリも Linux に配置されるため、WSL 2 を使用することで、開発環境と運用環境の間で一貫性が確保されます。

> [!NOTE]
> Windows 上で Node.js を直接使用することに取り組んでいる場合、または Windows Server 運用環境の使用を計画している場合は、[ご利用の Node.js 開発環境を Windows 上で直接設定する](./setup-on-windows.md)ためのガイドを参照してください。

## <a name="install-windows-10-insider-preview-build"></a>Windows 10 Insider Preview ビルドをインストールする

1. **[Windows 10 の最新バージョンをインストールします](https://www.microsoft.com/software-download/windows10)** : **[今すぐ更新]** を選択して、更新アシスタントをダウンロードします。 ダウンロードが完了したら、更新アシスタントを開いて、最新バージョンの Windows が現在実行されているかどうかを確認します。そのようになっていない場合は、アシスタント ウィンドウ内の **[今すぐ更新]** を選択して、ご利用のコンピューターを更新します。 " *(Windows 10 の最新バージョンを実行している場合、この手順は省略可能です)。* "

    ![Windows 更新アシスタント](../images/windows-update-assistant2019.png)

2. **[[スタート] > [設定] > [Windows Insider Program] の順に移動します](ms-settings:windowsinsider)** : [Windows Insider Program] ウィンドウで、 **[開始]** 、 **[アカウントをリンクする]** の順に選択します。

    ![Windows Insider Program の設定](../images/windows-insider-program-settings.png)

3. **[Windows Insider として登録します](https://insider.windows.com/getting-started/#register)** : 自分が Insider プログラムに登録されていない場合は、[Microsoft アカウント](https://account.microsoft.com/account) を使用してそれを行う必要があります。

    ![Windows Insider の登録](../images/windows-insider-account.png)

4. **[ファスト リング]** 更新プログラムまたは、 **[Skip ahead to the next Windows release]\(次の Windows リリースへスキップ\)** コンテンツの受け取りを選択します。 確定し、 **[後で再起動]** を選択します。 再起動する前に、いくつかの追加設定を変更する必要があります。

    ![Windows Insider ファスト リング](../images/windows-insider-fast.png)

## <a name="enable-windows-subsystem-for-linux-and-virtual-machine-platform"></a>Linux 用 Windows サブシステムと仮想マシン プラットフォームを有効にする

1. 引き続き **[Windows の設定]** において、 **[Windows の機能の有効化または無効化]** を検索します。
2. **[Windows の機能]** リストが表示されたら、スクロールして **[仮想マシンプラットフォーム]** と **[Linux 用 Windows サブシステム]** を見つけます。確実に両方とも有効にするためにチェックボックスをオンにしてから **[OK]** を選択します。
3. メッセージが表示されたら、コンピューターを再起動します。

    ![Windows の機能を有効にする](../images/windows-feature-settings.png)

## <a name="install-a-linux-distribution"></a>Linux ディストリビューションをインストールする

WSL 上で実行できる Linux ディストリビューションは複数あります。 Microsoft Store でお気に入りのものを探してインストールできます。 最新であり、広く普及しており、サポートが充実している [Ubuntu 18.04 LTS](https://www.microsoft.com/store/productId/9N9TNGVNDL3Q) から始めることをお勧めします。

1. この [Ubuntu 18.04 LTS](https://www.microsoft.com/store/productId/9N9TNGVNDL3Q) リンクを開き、Microsoft Store を開いて、 **[入手]** を選択します。 *(ダウンロードのサイズが大きいため、インストールに時間がかかる場合があります。)*

2. ダウンロードが完了したら、Microsoft Store から **[起動]** を選択するか、 **[スタート]** メニューに「Ubuntu 18.04 LTS」と入力して起動します。

3. ディストリビューションを初めて実行すると、アカウント名とパスワードの作成を求められます。 これ以降、既定でこのユーザーとして自動的にサインインします。 任意のユーザー名とパスワードを選択できます。 これらはご自分の Windows ユーザー名とは関係ありません。

    ![Microsoft Store での Linux ディストリビューション](../images/store-linux-distros.png)

現在使用している Linux ディストリビューションは、`lsb_release -dc` と入力することで確認できます。 使用中の Ubuntu ディストリビューションを更新するには、`sudo apt update && sudo apt upgrade` を使用します。 パッケージを常に最新にするために、定期的な更新をお勧めします。 Windows はこの更新を自動的に処理しません。 Microsoft Store から入手できる他の Linux ディストリビューションへのリンク、別のインストール方法、またはトラブルシューティングについては、「[Windows 10 用 Windows Subsystem for Linux のインストール ガイド](https://docs.microsoft.com/windows/wsl/install-win10)」を参照してください。

## <a name="install-wsl-2"></a>WSL 2 のインストール

WSL 2 とは、WSL の[新しいバージョンのアーキテクチャ](https://docs.microsoft.com/windows/wsl/wsl2-about)です。Linux ディストリビューションと Windows のやりとりを変更することで、パフォーマンスが向上し、システム コールの完全な互換性が追加されています。

1. PowerShell で、コマンド `wsl -l` を入力して、ご利用のコンピューターにインストールされている WSL ディストリビューションの一覧を表示します。 この一覧に Ubuntu-18.04 が表示されているのがわかるはずです。
2. ここで、コマンド `wsl --set-version Ubuntu-18.04 2` を入力して、WSL 2 を使用するように Ubuntu のインストールを設定します。
3. `wsl --list --verbose` (または `wsl -l -v`) を使用して、インストールされた各ディストリビューションで使用されている WSL のバージョンを確認します。

    ![Linux 用 Windows サブシステムのセット バージョン](../images/wsl-versions.png)

> [!TIP]
> 同じ手順 (PowerShell を使用) に従って、WSL 2 にインストールした Linux ディストリビューションを設定することができます。その場合は、"Ubuntu-18.04" をターゲットとするインストール済みのディストリビューションの名前に変更するだけです。 WSL 1 に戻すには、上記と同じコマンドを実行しますが、"2" を "1" に置き換えます。  また、次のように入力すれば、新しくインストールされたディストリビューションの既定値として WSL 2 を設定することもできます: `wsl --set-default-version 2`

## <a name="install-nvm-nodejs-and-npm"></a>nvm、node.js、および npm をインストールする

Node.js をインストールするには、複数の方法があります。 バージョンの変更は非常に早いため、バージョン マネージャーを使用することをお勧めします。 多くの場合、作業しているさまざまなプロジェクトのニーズに基づいて、複数のバージョンを切り替える必要があります。 ノード バージョン マネージャー (一般的には nvm と呼ばれることが多い) は、Node.js の複数のバージョンをインストールするための最も一般的な方法です。 ここでは、nvm をインストールしてからそれを使用して Node.js と Node Package Manager (npm) をインストールする手順について説明します。 [代替のバージョン マネージャー](#alternative-version-managers)についても検討する必要があります。次のセクションで説明します。

> [!IMPORTANT]
> バージョン マネージャーをインストールする前に、ご利用のオペレーティング システムから Node.js または npm の既存のインストールを削除することをお勧めします。インストールの種類が異なると、奇妙で混乱を招く競合が発生する可能性があるためです。 たとえば、Ubuntu の `apt-get` コマンドを使用してインストールできる Node のバージョンは、現在期限切れになっています。 以前のインストールの削除に関するヘルプについては、[ubuntu から nodejs を削除する方法](https://askubuntu.com/questions/786015/how-to-remove-nodejs-from-ubuntu-16-04)) に関するページを参照してください。

1. ご利用の Ubuntu 18.04 コマンドラインを開きます。
2. `sudo apt-get install curl` を使用して cURL をインストールします (コマンドラインでインターネットからコンテンツをダウンロードするために使用するツール)。
3. `curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.2/install.sh | bash` を使用して nvm をインストールします。
4. インストールを確認するには、`command -v nvm` を入力します。これによって "nvm" が返されるはずです。"コマンドが見つかりません" というメッセージが返される場合または何も応答がない場合は、現在のターミナルを閉じてから再度開き、もう一度やり直してください。 [nvm GitHub リポジトリで詳細を確認してください](https://github.com/nvm-sh/nvm)。
5. 現在インストールされている Node のバージョンを一覧表示します (この時点では何もないはずです): `nvm ls`

    ![Node のバージョンが表示されていない NVM リスト](../images/nvm-no-node.png)

6. Node.js の現在のリリースをインストールします (最新の機能強化をテストするためですが、問題が発生する可能性が高くなります): `nvm install node`
7. Node.js の最新の安定した LTS リリースをインストールします (推奨): `nvm install --lts`
8. インストールされている Node のバージョンを一覧表示します: `nvm ls`。先ほどインストールした 2 つのバージョンが表示されるはずです。

    ![LTS と現在の Node バージョンを表示した NVM リスト](../images/nvm-node-installed.png)

9. Node.js がインストールされており、現在の既定のバージョンであることを確認します: `node --version` を使用 次に、npm があることも確認します: `npm --version` を使用。(`which node` または `which npm` を使用して、既定のバージョンで使用されているパスを確認することもできます)。
10. プロジェクトに使用する Node.js のバージョンを変更するには、新しいプロジェクト ディレクトリ `mkdir NodeTest` を作成し、ディレクトリ `cd NodeTest` を入力します。次に `nvm use node` を入力して現在のバージョンに切り替えるか、または `nvm use --lts` を入力して LTS のバージョンに切り替えます。 また、`nvm use v8.2.1` のように、インストールした追加のバージョンに固有の番号を使用することもできます  (使用可能な Node.js のバージョンをすべて一覧表示するには、コマンド `nvm ls-remote` を使用します)。

> [!TIP]
> NVM を使用して Node.js と NPM をインストールする場合は、SUDO コマンドを使用して新しいパッケージをインストールする必要はありません。

> [!NOTE]
> 発行時、最新バージョンとして NVM v0.35.2 が提供されていました。 [GitHub プロジェクト ページで NVM の最新リリース](https://github.com/nvm-sh/nvm)を確認し、最新バージョンを含むように上記のコマンドを調整することができます。
cURL を使用してより新しいバージョンの NVM をインストールすると、古いものが置き換えられ、NVM を使用してインストールした Node のバージョンはそのまま残ります。 たとえば次のようになります。`curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.36.0/install.sh | bash`

## <a name="alternative-version-managers"></a>代替のバージョン マネージャー

nvm は現在、最も一般的なノード用バージョン マネージャーですが、考慮すべき代替がいくつかあります。

- [n](https://www.npmjs.com/package/n#installation) は従来から用いられてきた `nvm` の代替です。同じことを実現しますがコマンドが若干異なります。また、Bash スクリプトではなく、`npm` を介してインストールされます。
- [fnm](https://github.com/Schniz/fnm#using-a-script) はより新しいバージョン マネージャーであり、`nvm` よりもはるかに高速であると言われています  ([Azure Pipelines](https://docs.microsoft.com/azure/devops/pipelines/get-started/what-is-azure-pipelines?view=azure-devops) も使用されます)。
- [Volta](https://github.com/volta-cli/volta#installing-volta) は LinkedIn チームからの新しいバージョン マネージャーであり、速度の向上とクロスプラットフォーム サポートを特長として挙げています。
- [asdf-vm](https://asdf-vm.com/#/core-manage-asdf-vm) は、ike gvm、nvm、rbenv、pyenv (その他多数) などの複数の言語を 1 つにまとめた単一の CLI です。
- [nvs](https://github.com/jasongin/nvs) (Node Version Switcher) は、クロスプラットフォームの `nvm` の代わりであり、[VS Code との統合](https://github.com/jasongin/nvs/blob/master/doc/VSCODE.md)が可能です。

## <a name="install-your-favorite-code-editor"></a>お気に入りのコード エディターをインストールする

Node.js プロジェクトには、**Visual Studio Code** と **Remote WSL 拡張機能**を併用することをお勧めします。 これにより、VS Code が "クライアントサーバー" アーキテクチャに分割され、クライアント (ユーザー インターフェイス) はご利用の Windows コンピューター上で実行され、サーバー (ご利用のコード、Git、プラグインなど) はリモートで実行されます。

- Linux ベースの Intellisense とリンティングがサポートされています。
- プロジェクトは、Linux で自動的にビルドされます。
- Linux で実行されるご自分のすべての拡張機能を使用できます ([ES Lint、NPM Intellisense、ES6 スニペットなど](https://marketplace.visualstudio.com/items?itemName=waderyan.nodejs-extension-pack))。

ご利用のコンソール内からすばやく変更を加える場合は、ターミナルベースのテキスト エディター (vim、emacs、nano) も役に立ちます  ([この記事](https://medium.com/linode-cube/emacs-nano-or-vim-choose-your-terminal-based-text-editor-wisely-8f3826c92a68)では、相違点について説明すると共に、それぞれの使用方法について簡単に説明します)。

> [!NOTE]
> 一部の GUI エディター (Atom、Sublime Text、Eclipse) では、WSL 共有ネットワークの場所 (\\wsl$\Ubuntu\home\) にアクセスする際に問題が発生する可能性があります。さらに、Windows ツールを使用して Linux ファイルのビルドが試みられますが、これは不要な場合があります。 この互換性は、VS Code の Remote-WSL 拡張機能によって自動的に処理されます。

VS Code と Remote-WSL 拡張機能をインストールするには、次のようにします。

1. [Windows 用の VS Code をダウンロードしてインストールします](https://code.visualstudio.com)。 VS Code は Linux でも使用できますが、Linux 用 Windows サブシステムは GUI アプリをサポートしていないため、Windows にインストールする必要があります。 心配しなくても、Remote - WSL 拡張機能を使用すれば、将来、お使いの Linux コマンド ラインやツールとの統合は引き続き可能です。

2. [Remote - WSL 拡張機能](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl)を VS Code にインストールします。 これにより、統合開発環境として WSL を使用できるようになり、互換性とパスが自動的に処理されます。 [詳しくはこちらをご覧ください](https://code.visualstudio.com/docs/remote/remote-overview)。

> [!IMPORTANT]
> VS Code が既にインストールされている場合、[Remote - WSL 拡張機能](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl)をインストールするためには、[1.35 May リリース](https://code.visualstudio.com/updates/v1_35)以降がインストールされていることを確認する必要があります。 オートコンプリート、デバッグ、lint などのサポートが失われるため、VS Code において、Remote - WSL 拡張機能なしで WSL を使用することはお勧めしません。豆知識: この WSL 拡張機能は $HOME/.vscode-server/extensions にインストールされます。

### <a name="helpful-vs-code-extensions"></a>便利な VS Code 拡張機能

VS Code には Node.js 開発用のすぐに利用できる機能が多数付属している一方、[Node.js Extension Pack](https://marketplace.visualstudio.com/items?itemName=waderyan.nodejs-extension-pack) にはインストールを検討すべき便利ないくつかの拡張機能が用意されています。 これらをすべてインストールするか、または最も役に立つと思われるものを選択します。

Node.js 拡張パックをインストールするには、次の手順を行います。

1. VS Code で **[拡張機能]** ウィンドウを開きます (Ctrl + Shift + X)。

    [拡張機能] ウィンドウは現在 3 つのセクションに分割されています (Remote-WSL 拡張機能をインストールしたため)。
    - "ローカル - インストール済み": ご利用の Windows オペレーティングシステムで使用するためにインストールされた拡張機能。
    - "WSL:Ubuntu-18.04-Installed": ご利用の Ubuntu オペレーティング システムで使用するためにインストールされた拡張機能 (WSL)。
    - "推奨": 現在ご利用のプロジェクト内のファイルの種類に基づいて、VS Code から推奨される拡張機能。

    ![VS Code 拡張機能のローカルとリモートの比較](../images/vscode-extensions-local-remote.png)

2. [拡張機能] ウィンドウの上部にある [検索] ボックスに、次のように入力します。**Node Extension Pack** (または、お探しの拡張機能の名前)。 拡張機能は、現在のプロジェクトが開かれている場所に応じて、VS Code のローカル インスタンスまたは WSL インスタンスのいずれかにインストールされます。 VS Code ウィンドウの左下隅にあるリモート リンク (緑色) を選択するとわかります。 リモート接続を開く、または閉じるためのオプションが表示されます。 使用する Node.js 拡張機能を "WSL:Ubuntu-18.04" 環境にインストールします。

    ![VS Code のリモート リンク](../images/wsl-remote-extension.png)

他にも検討をお勧めする拡張機能として、次のようなものがあります。

- [Debugger for Chrome](https://code.visualstudio.com/blogs/2016/02/23/introducing-chrome-debugger-for-vs-code):Node.js を使用してサーバー側で開発を完了したら、クライアント側を開発してテストする必要があります。 この拡張機能により、ご利用の VS Code エディターと Chrome ブラウザーのデバッグ サービスが統合され、効率がより向上します。
- [他のエディターからのキーマップ](https://marketplace.visualstudio.com/search?target=VSCode&category=Keymaps&sortBy=Downloads): これらの拡張機能は、別のテキスト エディター (Atom、Sublime、Vim、eMacs、Notepad++ など) から移行する場合に、ご利用の環境を快適に保つのに役立ちます。
- [設定の同期](https://marketplace.visualstudio.com/items?itemName=Shan.code-settings-sync): GitHub を使用して、異なるインストール間で VS Code の設定を同期させることができます。 複数のコンピューターで作業する場合、この機能によってコンピューター間で環境の一貫性を保つことができます。

## <a name="install-windows-terminal-optional"></a>Windows ターミナルをインストールする (省略可能)

新しい Windows ターミナルでは、複数のタブ (コマンド プロンプト、PowerShell、複数の Linux ディストリビューション間をすばやく切り替える)、カスタム キー バインド (タブを開くまたは閉じる、コピーと貼り付けを行うなどのための独自のショートカット キーを作成する)、絵文字 ☺、カスタム テーマ (配色、フォント スタイルとサイズ、背景画像/ぼかし/透明度) を有効にすることができます。 [詳しくはこちらをご覧ください](https://devblogs.microsoft.com/commandline/)。

1. [Microsoft Store で Windows ターミナル (プレビュー)](https://www.microsoft.com/store/apps/9n0dx20hk701) を取得します: ストアを介してインストールすると、更新プログラムが自動的に処理されます。

2. インストールが完了したら、Windows ターミナルを開き、 **[設定]** を選択して、`profile.json` ファイルによってターミナルをカスタマイズします。 [Windows ターミナルの [設定] の編集方法の詳細を確認してください](https://github.com/microsoft/terminal/blob/master/doc/user-docs/UsingJsonSettings.md)。

    ![Windows ターミナルの設定](../images/windows-terminal-settings.png)

## <a name="set-up-git-optional"></a>Git を設定する (省略可能)

共同作業で開発する場合や、(GitHub のような) オープンソース サイトでプロジェクトをホストする場合のために、VS Code では [Git によるバージョン管理](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support)がサポートされています。 VS Code の [ソース管理] タブでは、すべての変更が追跡され、一般的な Git コマンド (追加、コミット、プッシュ、プル) が UI に組み込まれています。

1. Git は Linux 用 Windows サブシステム ディストリビューションと共にインストールされが、git 構成ファイルを設定する必要があります。 これを行うには、ターミナルで `git config --global user.name "Your Name"` を入力してから、`git config --global user.email "youremail@domain.com"` を入力します。 まだ Git アカウントを持っていない場合は、[GitHub でそれにサインアップ](https://github.com/join)することができます。 以前に Git を使用したことがない場合、入門用の [GitHub ガイド](https://guides.github.com/)が役に立ちます。 git 構成を編集する必要がある場合は、nano のような組み込みのテキスト エディターを使用してそれを行うことができます: `nano ~/.gitconfig`

2. Node プロジェクトに [.gitignore ファイル](https://help.github.com/en/articles/ignoring-files)を追加することをお勧めします。 GitHub の既定の Node.js 用 gitignore テンプレートは、[こちら](https://github.com/github/gitignore/blob/master/Node.gitignore)です。 [GitHub Web サイトを使用して新しいリポジトリを作成](https://help.github.com/articles/create-a-repo)することを選択した場合は、リポジトリを初期化する場合に利用できるチェックボックスがあります。README ファイル、Node.js プロジェクトに合わせて設定された .gitignore ファイル、必要に応じてライセンスを追加するためのオプションが対象になります。

## <a name="next-steps"></a>次の手順

これで、Node.js 開発環境が設定されました。 Node.js 環境の使用を開始するには、次のチュートリアルのいずれかを試すことを検討してください。

- [初心者向けの Node.js 概要](./beginners.md)
- [Windows での Node.js Web フレームワークの概要](./web-frameworks.md)
- [Node.js アプリのデータベースへの接続の概要](./databases.md)
- [Node.js で Docker コンテナーを使ってみる](./containers.md)
