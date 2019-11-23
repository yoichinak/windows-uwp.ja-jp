---
title: ネイティブ Windows での NodeJS の設定
description: Node.js 開発環境を Windows に直接設定するためのガイドです。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: Node.js、windows 10、ネイティブウィンドウ、windows 上で直接
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: 18a8d07f790c391a6e10577ff512347106e1cf21
ms.sourcegitcommit: 60d2d15dd0d365f82e4e90e4bc34b40cf5b4a247
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/17/2019
ms.locfileid: "72517827"
---
# <a name="set-up-your-nodejs-development-environment-directly-on-windows"></a>Node.js 開発環境を Windows 上に直接セットアップする

ネイティブ Windows 開発環境で node.js の使用を開始するためのステップバイステップガイドを次に示します。

> [!NOTE]
> Windows での node.js の使用は確かに有効なオプションですが、通常は、node.js web アプリを開発するために Windows Subsystem for Linux (WSL) を使用することをお勧めします。 多くの node.js パッケージとフレームワークは * nix 環境を念頭に置いて作成されており、ほとんどの node.js アプリは Linux にデプロイされているため、WSL での開発では、開発環境と運用環境の間の一貫性が確保されます。 WSL dev 環境を設定するには、「 [wsl 2 を使用して node.js 開発環境を設定](./setup-on-wsl2.md)する」を参照してください。

## <a name="install-nvm-windows-nodejs-and-npm"></a>Nvm (windows、node.js、nvm) をインストールします。

Node.js をインストールするには、複数の方法があります。 バージョンが非常に迅速に変更されるため、バージョンマネージャーを使用することをお勧めします。 多くの場合、作業しているさまざまなプロジェクトのニーズに基づいて、複数のバージョンを切り替える必要があります。 ノードバージョンマネージャーは、一般に nvm と呼ばれていますが、node.js の複数のバージョンをインストールする最も一般的な方法ですが、Mac/Linux でのみ使用でき、Windows ではサポートされていません。 代わりに、nvm-windows をインストールし、それを使用して node.js および Node Package Manager (nvm) をインストールする手順について説明します。 次のセクションでは、[別のバージョンのマネージャー](#alternative-version-managers)についても考慮する必要があります。

> [!IMPORTANT]
> 異なる種類のインストールでバージョンマネージャーをインストールする前に、オペレーティングシステムから node.js または npm の既存のインストールをすべて削除することをお勧めします。これにより、予期しない競合が発生する可能性があります。 これには、残っている可能性のある既存の nodejs インストールディレクトリ (例: "C:\Program Files\nodejs") の削除が含まれます。 NVM が生成したシンボリックリンクは、既存の (空である) インストールディレクトリを上書きしません。 以前のインストールの削除については、「 [Windows から node.js を完全に削除する方法](https://stackoverflow.com/questions/20711240/how-to-completely-remove-node-js-from-windows)」を参照してください。)

1. インターネットブラウザーで[windows-nvm リポジトリ](https://github.com/coreybutler/nvm-windows#node-version-manager-nvm-for-windows)を開き、 **[今すぐダウンロード]** リンクを選択します。
2. 最新のリリースの**nvm-setup**ファイルをダウンロードします。
3. ダウンロードが完了したら、zip ファイルを開き、 **nvm-setup**ファイルを開きます。
4. セットアップ-NVM-windows インストールウィザードでは、nvm と node.js の両方がインストールされるディレクトリを選択するなど、セットアップ手順を実行します。

    ![NVM for Windows インストールウィザード](../images/install-nvm-for-windows-wizard.png)

5. インストールが完了したら、 PowerShell を開き、windows-nvm を使用して、現在インストールされているノードのバージョンを一覧表示します (現時点では、この時点では none にする必要があります)。 `nvm ls`

    ![ノードバージョンが表示されていない NVM リスト](../images/windows-nvm-powershell-no-node.png)

6. Node.js の現在のリリースをインストールします (最新の機能の改善をテストするために、LTS バージョンよりも問題が発生する可能性が高く `nvm install latest` なります)。
7. Node.js の最新の安定した LTS リリース (推奨) をインストールします。最初に現在の LTS バージョン番号を参照してください `nvm list available`。そのためには、次のようにして、LTS バージョン番号を: `nvm install <version>` でインストールします (`<version>` を数字に置き換えます。 ie: `nvm install 10.16.3`)。

    ![使用可能なバージョンの NVM リスト](../images/windows-nvm-list.png)

8. インストールされているノードのバージョンを一覧表示する: `nvm ls`...これで、先ほどインストールした2つのバージョンが表示されます。

    ![インストールされているノードバージョンを示す NVM リスト](../images/windows-nvm-node-installs.png)

9. 現在既定である node.js のバージョンを確認するには、次のように入力します: `node --version`
10. プロジェクトに使用する node.js のバージョンを変更するには、新しいプロジェクトディレクトリ `mkdir NodeTest`を作成し、`cd NodeTest`ディレクトリを入力して、`<version>` を使用するバージョン番号 (ie v 10.16.3 ') に置き換え `nvm use <version>` を入力します。
11. インストールされている npm のバージョンを確認してください: `npm --version`、このバージョン番号は、現在のバージョンの node.js に関連付けられている npm のバージョンに自動的に変更されます。

## <a name="alternative-version-managers"></a>代替バージョンマネージャー

Windows-nvm は現在最も人気のあるノードのバージョンマネージャーですが、次の点を考慮することをお勧めします。

- [nvs](https://github.com/jasongin/nvs) (ノードバージョンスイッチャー) は、 [VS Code と統合](https://github.com/jasongin/nvs/blob/master/doc/VSCODE.md)できる、クロスプラットフォームの `nvm` 代替です。
- 
- [Volta](https://github.com/volta-cli/volta#installing-volta)は、より高速でクロスプラットフォームのサポートを要求する LinkedIn チームの新しいバージョンマネージャーです。

(Windows-nvm ではなく) バージョンマネージャーとして Volta をインストールするには、[はじめにガイド](https://docs.volta.sh/guide/getting-started)の「 **windows インストール**」セクションにアクセスし、windows インストーラーをダウンロードして実行します。セットアップの手順に従ってください。

> [!IMPORTANT]
> Volta をインストールする前に、Windows コンピューターで[開発者モード](https://docs.microsoft.com/en-us/windows/uwp/get-started/enable-your-device-for-development#accessing-settings-for-developers)が有効になっていることを確認する必要があります。

Volta を使用して Windows に複数のバージョンの node.js をインストールする方法の詳細については、 [Volta のドキュメント](https://docs.volta.sh/guide/understanding#managing-your-toolchain)を参照してください。

## <a name="install-your-favorite-code-editor"></a>お気に入りのコードエディターをインストールする

Windows で node.js を使用して開発する場合は、VS Code、および[Node.js 拡張パック](https://marketplace.visualstudio.com/items?itemName=waderyan.nodejs-extension-pack)を[インストール](https://code.visualstudio.com)することをお勧めします。 これらのすべてをインストールするか、最も役に立つと思われるものを選択して選択します。

Node.js 拡張パックをインストールするには:

1. VS Code で **[拡張]** ウィンドウ (Ctrl + Shift + X) を開きます。
2. [拡張機能] ウィンドウの上部にある [検索] ボックスに、「Node Extension Pack」 (または目的の拡張機能の名前) と入力します。
3. **[インストール]** を選択します。 インストールが完了すると、拡張機能が **拡張機能** ウィンドウの 有効 フォルダーに表示されます。 新しい拡張機能の説明の横にある歯車アイコンを選択して、設定を無効にしたり、アンインストールしたり、構成したりすることができます。

さらに、次のような拡張機能を使用することもできます。

- [Chrome のデバッガー](https://code.visualstudio.com/blogs/2016/02/23/introducing-chrome-debugger-for-vs-code): node.js を使用してサーバー側で開発を完了すると、クライアント側を開発してテストする必要があります。 この拡張機能により、VS Code エディターと Chrome ブラウザーのデバッグサービスが統合され、さらに効率的になります。
- [他のエディターからの Keymaps](https://marketplace.visualstudio.com/search?target=VSCode&category=Keymaps&sortBy=Downloads): これらの拡張機能は、別のテキストエディター (Atom、Sublime、Vim、EMacs、メモ帳 + + など) から移行している場合に、環境を自宅で使用しやすくするために役立ちます。
- [設定の同期](https://marketplace.visualstudio.com/items?itemName=Shan.code-settings-sync): GitHub を使用して、複数のインストール間で VS Code 設定を同期できます。 別のコンピューターで作業している場合は、これによって環境の整合性を保つことができます。

## <a name="install-git-optional"></a>Git のインストール (省略可能)

他のユーザーと共同作業を行う場合、または (GitHub などの) オープンソースサイトでプロジェクトをホストする場合、VS Code は[Git を使用したバージョン管理](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support)をサポートします。 VS Code の [ソース管理] タブでは、すべての変更が追跡され、UI には一般的な Git コマンド (add、commit、push、pull) が組み込まれています。 最初に Git をインストールして、ソース管理パネルを電源にする必要があります。

1. Git [-scm web サイト](https://git-scm.com/download/win)から Git for Windows をダウンロードしてインストールします。

2. インストールウィザードには、Git インストールの設定に関する一連の質問が記載されています。 何らかの変更に特に理由がない限り、すべての既定の設定を使用することをお勧めします。

3. 前に Git を使用したことがない場合は、 [GitHub のガイド](https://guides.github.com/)を参考にしてください。

4. ノードプロジェクトには、ファイルを追加することをお勧めし[ます](https://help.github.com/en/articles/ignoring-files)。 [Node.js の GitHub の既定のテンプレート](https://github.com/github/gitignore/blob/master/Node.gitignore)は次のとおりです。
