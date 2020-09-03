---
title: NodeJS をネイティブ Windows 上に設定する
description: Windows 上に Node.js 開発環境を設定するために役立つガイド。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: Node.js, windows 10, ネイティブ Windows, Windows 上に直接
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: 8c865610ba2678c1c5ab1b25ff7a2c7410d11f15
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89166586"
---
# <a name="set-up-your-nodejs-development-environment-directly-on-windows"></a>Node.js 開発環境を Windows 上に直接設定する

以下では、ネイティブの Windows 開発環境で Node.js の使用を開始するためのステップバイステップ ガイドを示します。

## <a name="install-nvm-windows-nodejs-and-npm"></a>Install nvm-windows のインストール, node.js, npm

Node.js をインストールするには、複数の方法があります。 バージョンの変更は非常に早いため、バージョン マネージャーを使用することをお勧めします。 多くの場合、作業しているさまざまなプロジェクトのニーズに基づいて、複数のバージョンを切り替える必要があります。 ノード バージョン マネージャー (一般的には nvm と呼ばれています) は、複数のバージョンの Node.js をインストールする場合の最も一般的な方法ですが、Mac または Linux にのみ使用でき、Windows ではサポートされていません。 代わりに、ここでは、nvm-windows をインストールし、それを使用して Node.js と Node Package Manager (npm) をインストールする手順について説明します。 [代替のバージョン マネージャー](#alternative-version-managers)についても検討する必要があります。次のセクションで説明します。

> [!IMPORTANT]
> バージョン マネージャーをインストールする前に、ご利用のオペレーティング システムから Node.js または npm の既存のインストールを削除することをお勧めします。インストールの種類が異なると、奇妙で混乱を招く競合が発生する可能性があるためです。 これには、残っている可能性がある既存の nodejs インストール ディレクトリ (例: "C:\Program Files\nodejs") の削除などが含まれます。 NVM によって生成される symlink は、既存のインストール ディレクトリを (空であっても) 上書きしません。 以前のインストールの削除については、「[How to completely remove node.js from Windows](https://stackoverflow.com/questions/20711240/how-to-completely-remove-node-js-from-windows)」 (Windows から node.js を完全に削除する方法) を参照してください。

1. ご使用のインターネット ブラウザーで [windows-nvm リポジトリ](https://github.com/coreybutler/nvm-windows#node-version-manager-nvm-for-windows)を開いて、 **[今すぐダウンロード]** を選択します。
2. 最新リリースの **nvm-setup.zip** をダウンロードします。
3. ダウンロードが完了したら、ZIP ファイルを開いて、**nvm-setup.exe** ファイルを開きます。
4. Setup-NVM-for-Windows インストール ウィザードの指示に従って、セットアップ手順を行います。たとえば、nvm-windows と Node.js の両方をインストールするディレクトリの選択などがあります。

    ![NVM for Windows インストール ウィザード](../images/install-nvm-for-windows-wizard.png)

5. インストールが完了したら、 PowerShell を開き、windows-nvm を使用して、現在インストールされている Node のバージョンを一覧表示してみます (この時点では何も表示されないはずです): `nvm ls`

    ![Node のバージョンが表示されていない NVM リスト](../images/windows-nvm-powershell-no-node.png)

6. Node.js の現在のリリースをインストールします (最新の機能強化をテストするためですが、LTS バージョンよりも問題が発生する可能性が高くなります): `nvm install latest`

7. まず、`nvm list available` を実行して LTS の現在のバージョン番号を調べた後、Node.js の最新の安定した LTS リリースをインストールします (推奨)。次に、`nvm install <version>`(`<version>` を、インストールするバージョン番号に置き換えます。たとえば、`nvm install 12.14.0`) を実行して、そのバージョン番号の LTS をインストールします。

    ![使用可能なバージョンの NVM リスト](../images/windows-nvm-list.png)

8. インストールされている Node のバージョンを一覧表示します: `nvm ls`。先ほどインストールした 2 つのバージョンが表示されるはずです。

    ![インストールされている Node のバージョンを示す NVM リスト](../images/windows-nvm-node-installs.png)

9. 必要なバージョン番号の Node.js をインストールしたら、`nvm use <version>` を入力して、使用するバージョンを選択します (`<version>` を番号で置き換え、`nvm use 12.9.0` のようにします)。

10. プロジェクトに使用する Node.js のバージョンを変更するには、新しいプロジェクト ディレクトリを作成し (`mkdir NodeTest`)、ディレクトリ `cd NodeTest` を入力します。次に、`<version>`を、使用するバージョン番号 (たとえば、v10.16.3) に置き換えて、`nvm use <version>` を入力します。

11. `npm --version` を実行して、インストールされている npm のバージョンを確認します。このバージョン番号は、Node.js の現在のバージョンに関連付けられている npm のバージョンに自動的に変更されます。

## <a name="alternative-version-managers"></a>代替のバージョン マネージャー

windows-nvm は、現在最も一般的なノード バージョン マネージャーですが、検討すべき代替マネージャーがいくつかあります。

- [nvs](https://github.com/jasongin/nvs) (Node Version Switcher) は、クロスプラットフォームの `nvm` の代わりであり、[VS Code との統合](https://github.com/jasongin/nvs/blob/master/doc/VSCODE.md)が可能です。

- [Volta](https://github.com/volta-cli/volta#installing-volta) は LinkedIn チームからの新しいバージョン マネージャーであり、速度の向上とクロスプラットフォーム サポートを特長として挙げています。

(windows-nvm ではなく) Volta をバージョン マネージャーとしてインストールするには、「[Getting Started guide](https://docs.volta.sh/guide/getting-started)」 (入門ガイド) の「**Windows Installation**」 (Windows へのインストール) セクションを参照し、セットアップ手順に従って Windows インストーラーをダウンロードして実行します。

> [!IMPORTANT]
> Volta をインストールする前に、Windows コンピューターで確実に [[開発者モード]](/windows/uwp/get-started/enable-your-device-for-development#accessing-settings-for-developers) を有効にする必要があります。

Volta を使用して複数のバージョンの Node.js を Windows にインストールする方法の詳細については、[Volta Docs](https://docs.volta.sh/guide/understanding#managing-your-toolchain) を参照してください。

## <a name="install-your-favorite-code-editor"></a>お気に入りのコード エディターをインストールする

Windows で Node.js を使用して開発するには、[VS Code](https://code.visualstudio.com)と [Node.js 拡張パック](https://marketplace.visualstudio.com/items?itemName=waderyan.nodejs-extension-pack) をインストールすることをお勧めします。 これらをすべてインストールするか、または最も役に立つと思われるものを選択します。

Node.js 拡張パックをインストールするには、次の手順を行います。

1. VS Code で **[拡張機能]** ウィンドウを開きます (Ctrl + Shift + X)。
2. [拡張機能] ウィンドウの上部にある [検索] ボックスに、次のように入力します。"Node Extension Pack" (または、お探しの拡張機能の名前)。
3. **[インストール]** を選択します。 インストールが完了すると、拡張機能が **[拡張機能]** ウィンドウの [有効] フォルダーに表示されます。 新しい拡張機能の説明の横にある歯車アイコンを選択して、設定を無効、アンインストール、または構成することができます。

他にも検討をお勧めする拡張機能として、次のようなものがあります。

- [Debugger for Chrome](https://code.visualstudio.com/blogs/2016/02/23/introducing-chrome-debugger-for-vs-code):Node.js を使用してサーバー側で開発を完了したら、クライアント側を開発してテストする必要があります。 この拡張機能により、ご利用の VS Code エディターと Chrome ブラウザーのデバッグ サービスが統合され、効率がより向上します。
- [他のエディターからのキーマップ](https://marketplace.visualstudio.com/search?target=VSCode&category=Keymaps&sortBy=Downloads): これらの拡張機能は、別のテキスト エディター (Atom、Sublime、Vim、eMacs、Notepad++ など) から移行する場合に、ご利用の環境を快適に保つのに役立ちます。
- [設定の同期](https://marketplace.visualstudio.com/items?itemName=Shan.code-settings-sync): GitHub を使用して、異なるインストール間で VS Code の設定を同期させることができます。 複数のコンピューターで作業する場合、この機能によってコンピューター間で環境の一貫性を保つことができます。

## <a name="install-git-optional"></a>Git のインストール (省略可能)

共同作業で開発する場合や、(GitHub のような) オープンソース サイトでプロジェクトをホストする場合のために、VS Code では [Git によるバージョン管理](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support)がサポートされています。 VS Code の [ソース管理] タブでは、すべての変更が追跡され、一般的な Git コマンド (追加、コミット、プッシュ、プル) が UI に組み込まれています。 ソース管理パネルを使用するには、まず Git をインストールする必要があります。

1. Windows 用の Git を [git-scm Web サイト](https://git-scm.com/download/win)からダウンロードしてインストールします。

2. インストール ウィザードで、Git インストールの設定に関する一連の質問に答えます。 何かを変更する特別な理由がない限り、すべて既定の設定を使用することをお勧めします。

3. 以前に Git を使用したことがない場合、入門用の [GitHub ガイド](https://guides.github.com/)が役に立ちます。

4. Node プロジェクトに [.gitignore ファイル](https://help.github.com/en/articles/ignoring-files)を追加することをお勧めします。 GitHub の既定の Node.js 用 gitignore テンプレートは、[こちら](https://github.com/github/gitignore/blob/master/Node.gitignore)です。

## <a name="use-windows-subsystem-for-linux-for-production"></a>Linux 用 Windows サブシステムを運用環境で使用する

Node.js を Windows で直接使用することは、何ができるかを学習し、体験するのに最適です。 通常は Linux ベースのサーバーに展開される運用環境対応の Web アプリを構築する準備ができたら、Linux 用 Windows サブシステムのバージョン 2 (WSL 2) を使用して Node.js Web アプリを開発することをお勧めします。 多くの Node.js パッケージおよびフレームワークは、*nix 環境を念頭に置いて作成され、ほとんどの Node.js アプリは Linux に展開されるため、WSL で開発すると、開発環境と運用環境の間で一貫性が確保されます。 WSL 開発環境を設定するには、「[WSL 2 を使用して node.js 開発環境を設定する](./setup-on-wsl2.md)」を参照してください。

> [!NOTE]
> Node.js アプリを Windows サーバーでホストする必要がある (まれな) 状況では、最も一般的なシナリオとして、[リバース プロキシの使用](https://medium.com/intrinsic/why-should-i-use-a-reverse-proxy-if-node-js-is-production-ready-5a079408b2ca)が考えられます。 これには、1) [iisnode を使用する](https://harveywilliams.net/blog/installing-iisnode) または [直接使用する](https://dev.to/petereysermans/hosting-a-node-js-application-on-windows-with-iis-as-reverse-proxy-397b)という 2 つの方法があります。 Microsoft では、これらのリソースを保持していないので、[Linux サーバーを使用して Node.js アプリをホストする](/azure/app-service/app-service-web-get-started-nodejs)ことをお勧めします。