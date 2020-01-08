---
title: Node.js で Docker コンテナーを使ってみる
description: Docker コンテナーを node.js アプリで使い始める際に役立つステップバイステップガイドです。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: ''
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: 9467224814b1e26f18031662f5e8d994a8fae1ac
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/06/2020
ms.locfileid: "75683675"
---
# <a name="get-started-using-docker-containers-with-nodejs"></a>Node.js で Docker コンテナーを使ってみる

Docker コンテナーを node.js アプリで使い始める際に役立つステップバイステップガイドです。

## <a name="prerequisites"></a>必要条件

このガイドでは、次のよう[な WSL 2 を使用して node.js 開発環境を設定](./setup-on-wsl2.md)する手順を既に完了していることを前提としています。

- Windows 10 Insider Preview ビルド18932以降をインストールします。
- Windows で WSL 2 機能を有効にします。
- Linux ディストリビューションをインストールします (この例では Ubuntu 18.04)。 これを確認するには、`wsl lsb_release -a`を使用します。
- Ubuntu 18.04 のディストリビューションが WSL 2 モードで実行されていることを確認します。 (WSL では、v1 モードと v2 モードの両方でディストリビューションを実行できます)。これを確認するには、PowerShell を開き、「`wsl -l -v`」と入力します。
- PowerShell を使用して、Ubuntu 18.04 を既定のディストリビューションとして設定します。 `wsl -s ubuntu 18.04`を使用します。

## <a name="overview-of-docker-containers"></a>Docker コンテナーの概要

**Docker**は、コンテナーを使用してアプリケーションを作成、デプロイ、および実行するために使用されるツールです。 コンテナーを使用すると、開発者は必要なすべてのパーツ (ライブラリ、フレームワーク、依存関係など) を使用してアプリをパッケージ化し、すべてを1つのパッケージとして出荷できます。 コンテナーを使用すると、アプリのコードの記述とテストに使用されたコンピューターとは異なる場合がある、カスタマイズされた設定や以前にインストールされたライブラリに関係なく、アプリが同じように動作することが保証されます。 これにより、開発者は、コードを実行するシステムについて心配することなく、コードの記述に集中できます。

Docker コンテナーは仮想マシンに似ていますが、仮想オペレーティングシステム全体を作成するわけではありません。 代わりに、Docker では、アプリが実行されているシステムと同じ Linux カーネルを使用することができます。 これにより、アプリケーションパッケージはホストコンピューター上にまだ存在しない部分のみを必要とし、パッケージサイズを減らし、パフォーマンスを向上させることができます。

[Kubernetes](https://docs.microsoft.com/azure/aks/)のようなツールで Docker コンテナーを使用することは、コンテナーの人気を高めるもう1つの理由です。 これにより、複数のバージョンのアプリコンテナーを異なるタイミングで作成できます。 更新またはメンテナンスのためにシステム全体を停止するのではなく、各コンテナー (および特定のマイクロサービス) をすぐに置き換えることができます。 すべての更新プログラムを使用して新しいコンテナーを準備し、運用環境用のコンテナーを設定して、準備ができたら新しいコンテナーをポイントするだけです。 また、コンテナーを使用して異なるバージョンのアプリをアーカイブし、必要に応じて安全フォールバックとして実行し続けることもできます。

## <a name="install-docker-desktop-wsl-2-tech-preview"></a>Docker Desktop WSL 2 Tech Preview のインストール

以前は、WSL 1 では Docker デーモンを直接実行できませんでしたが、WSL 2 で変更され、WSL 2 用の Docker Desktop により速度とパフォーマンスが大幅に向上しました。

Docker Desktop WSL 2 Tech Preview をインストールして実行するには、次のようにします。

1. [Docker Desktop WSL 2 Tech Preview インストーラー](https://download.docker.com/win/edge/36883/Docker%20Desktop%20Installer.exe)をダウンロードします。 (必要に応じて、[インストーラードキュメント](https://docs.docker.com/docker-for-windows/wsl-tech-preview/)を参照できます)。

2. ダウンロードした Docker インストーラーを開きます。 インストールウィザードでは、linux コンテナーではなく Windows コンテナーを使用するかどうかを確認するメッセージが表示されます。 Linux サブシステムを使用するため、このチェックをオフのままにします。 Docker は、既定の WSL 2 ディストリビューションの管理対象ディレクトリにインストールされ、Docker デーモン、CLI、および作成 CLI を含みます。

    ![Docker デスクトップの起動](../images/install-docker-1.png)

3. まだ Docker ID を持っていない場合は、「 [https://hub.docker.com/signup](https://hub.docker.com/signup)」にアクセスして設定する必要があります。 ID は、すべて小文字の英数字にする必要があります。

4. インストールが完了したら、デスクトップ上のショートカットアイコンを選択するか、Windows の [スタート] メニューで、Docker Desktop を起動します。 Docker アイコンは、タスクバーの [非表示アイコン] メニューに表示されます。 アイコンを右クリックして [Docker コマンド] メニューを表示し、[WSL 2 Tech Preview] を選択します。

5. Tech preview ウィンドウが開いたら、 **[開始]** を選択して、wsl 2 で Docker デーモン (バックグラウンドプロセス) の実行を開始します。 WSL 2 docker デーモンが開始されると、docker CLI コンテキストが自動的に作成されます。

    ![Docker デスクトップの起動](../images/start-docker.gif)

6. Docker がインストールされていることを確認し、バージョン番号を表示するには、コマンドライン (WSL または PowerShell) を開き、次のように入力し `docker --version`

7. 単純な組み込みの Docker イメージを実行することで、インストールが正しく機能することをテストします。 `docker run hello-world`

理解しておく必要がある Docker コマンドをいくつか次に示します。

- 次のように入力して、Docker CLI で使用できるコマンドを一覧表示し `docker`
- 特定のコマンドの情報を一覧表示: `docker <COMMAND> --help`
- 次のように、コンピューター上の docker イメージ (この時点では hello world イメージ) を一覧表示し `docker image ls --all`
- 次のようにして、コンピューター上のコンテナーを一覧表示します: `docker container ls --all`
- 次のように、WSL 2 コンテキストで使用可能な Docker システムの統計とリソース (CPU & メモリ) を一覧表示します。 `docker info`
- Docker が現在実行されている場所を表示します。次のものがあります: `docker context ls`

Docker が実行されているコンテキストは、`default` (クラシック Docker デーモン) と `wsl` (tech preview を使用した推奨事項) の2つであることがわかります。 (また、`ls` コマンドは `list` に対して短く、同じ意味で使用できます)。

![Powershell での Docker 表示コンテキスト](../images/docker-context.png)

> [!TIP]
> [Docker Hub でこのチュートリアル](https://hub.docker.com/?overlay=onboarding)を使用して、サンプルの docker イメージを作成してみてください。 Docker Hub には、コンテナー化するアプリの種類に一致する可能性のある多数のオープンソースイメージも含まれています。 この[Gatsby framework コンテナー](https://hub.docker.com/r/gatsbyjs/gatsby-dev-builds)やこの[Nuxt framework コンテナー](https://hub.docker.com/r/hobord/nuxtexpress)などのイメージをダウンロードし、独自のアプリケーションコードで拡張することができます。 コマンドラインまたは[Docker Hub web サイト](https://hub.docker.com/search/?type=image)[から docker](https://docs.docker.com/engine/reference/commandline/search/)を使用して、レジストリを検索できます。

## <a name="install-the-docker-extension-on-vs-code"></a>VS Code に Docker 拡張機能をインストールする

Docker 拡張機能を使用すると、コンテナー化されたアプリケーションのビルド、管理、および Visual Studio Code からのデプロイを簡単に行うことができます。

1. VS Code で **[拡張機能]** ウィンドウ (Ctrl + Shift + X) を開き、 **Docker**を検索します。

2. [Microsoft Docker 拡張機能](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker)を選択し、を**インストール**します。 拡張機能を有効にするには、をインストールした後に VS Code を再読み込みする必要があります。

    ![リモート-WSL の VS Code の Docker 拡張機能](../images/docker-vscode-extension.png)

VS Code に Docker 拡張機能をインストールすることにより、次のセクションで使用する `Dockerfile` のコマンドの一覧をショートカットで表示できるようになりました。 `Ctrl+Space`

[Docker の操作の詳細については、VS Code を](https://code.visualstudio.com/docs/azure/docker)参照してください。

## <a name="create-a-container-image-with-dockerfile"></a>DockerFile を使用してコンテナー イメージを作成する

**コンテナーイメージ**には、アプリケーションコード、ライブラリ、構成ファイル、環境変数、およびランタイムが格納されます。 イメージを使用すると、コンテナー内の環境が標準化され、アプリケーションのビルドと実行に必要なもののみが含まれるようになります。

**Dockerfile**には、新しいコンテナーイメージを作成するために必要な手順が含まれています。 つまり、このファイルは、どこからでも再現できるように、アプリの環境を定義するコンテナーイメージをビルドします。

[Web フレームワーク](./web-frameworks.md)ガイドで設定した次の .js アプリを使用して、コンテナーイメージを作成してみましょう。

1. VS Code で次の .js アプリを開きます (リモート WSL 拡張機能が実行されていることを確認するために、左下の緑色のタブに示されています)。 VS Code に統合された WSL ターミナル ( **> ターミナルを表示**) を開き、ターミナルパスが次の .js プロジェクトディレクトリ (ie. `~/NextProjects/my-next-app$`) を指していることを確認します。

2. 次の .js プロジェクトのルートに `Dockerfile` という名前の新しいファイルを作成し、次の内容を追加します。

    ```docker
    # Specifies where to get the base image (Node v12 in our case) and creates a new container for it
    FROM node:12
    
    # Set working directory. Paths will be relative this WORKDIR.
    WORKDIR /usr/src/app
    
    # Install dependencies
    COPY package*.json ./
    RUN npm install
    
    # Copy source files from host computer to the container
    COPY . .
    
    # Build the app
    RUN npm run build
    
    # Specify port app runs on
    EXPOSE 3000

    # Run the app
    CMD [ "npm", "start" ]
    ```

3. Docker イメージをビルドするには、プロジェクトのルートから次のコマンドを実行します (`<your_docker_username>` は、Docker Hub で作成したユーザー名に置き換えます)。 `docker build -t <your_docker_username>/my-nextjs-app .`

> [!NOTE]
> このコマンドを機能させるには、Docker が WSL Tech Preview を使用して実行されている必要があります。 Docker を開始する方法の詳細については、「install」セクションの「[手順 #4](#install-docker-desktop-wsl-2-tech-preview) 」を参照してください。 `-t` フラグは、作成するイメージの名前を指定します。ここでは "my nextjs-app: v1" を指定します。 イメージを作成するときは、常に[タグ名にバージョン # を使用](https://medium.com/@mccode/the-misunderstood-docker-tag-latest-af3babfd6375)することをお勧めします。 コマンドの最後にピリオドを含めるようにしてください。これは、現在の作業ディレクトリを使用して、次の .js アプリのビルドファイルを検索し、コピーすることを指定します。

4. 次の .js アプリの新しい docker イメージをコンテナー内で実行するには、コマンドを入力します: `docker run -d -p 3333:3000 <your_docker_username>/my-nextjs-app:v1`

5. `-p` フラグによって、コンピューター上のポート ' 3000 ' (アプリがコンテナー内で実行されているポート) がローカルポート ' 3333 ' にバインドされます。これにより、web ブラウザーで[http://localhost:3333](http://localhost:3333)をポイントし、サーバー側でレンダリングされる次の .js アプリケーションを Docker コンテナーイメージとして表示できるようになります。

> [!TIP]
> Docker Hub に格納されている node.js バージョン12の既定のイメージを参照する `FROM node:12` を使用して、コンテナーイメージを構築しています。 この既定の node.js イメージは Debian/Ubuntu Linux システムに基づいており、さまざまな node.js イメージから選択できます。また、より軽量であるか、ニーズに合わせて調整することを検討する必要があります。 詳細については、 [Docker Hub の Node.js イメージレジストリ](https://hub.docker.com/_/node/)を参照してください。

## <a name="upload-your-container-image-to-a-repository"></a>コンテナー イメージをリポジトリにアップロードする

コンテナー**リポジトリ**は、コンテナーイメージをクラウドに格納します。 多くの場合、コンテナーリポジトリには、さまざまなバージョンなどの関連するイメージのコレクションが含まれており、これらはすべて、簡単なセットアップや迅速なデプロイに使用できます。 通常は、セキュリティで保護された HTTPs エンドポイント経由でコンテナーリポジトリのイメージにアクセスできます。これにより、任意のシステム、ハードウェア、または VM インスタンスを使用してイメージをプル、プッシュ、または管理できます。

一方、**コンテナーレジストリ**には、リポジトリのコレクションに加えて、インデックス、アクセス制御規則、および API パスが格納されます。 これらは、パブリックまたはプライベートにホストできます。 [Docker Hub](https://hub.docker.com/)はオープンソースの docker レジストリで、`docker push` および `docker pull` コマンドを実行するときに既定で使用されます。 パブリックリポジトリでは無料で、プライベートリポジトリの料金が必要です。

Docker Hub でホストされているリポジトリに新しいコンテナーイメージをアップロードするには、次のようにします。

1. Docker Hub にログインします。 インストール手順で、Docker Hub アカウントの作成に使用したユーザー名とパスワードを入力するように求められます。 ターミナルで Docker にログインするには、次のように入力します。 `docker login`

2. コンピューターに作成した docker コンテナーイメージの一覧を取得するには、次のように入力します。 `docker image ls --all`

3. 次のコマンドを使用して、コンテナーイメージを Docker Hub までプッシュし、そこに新しいリポジトリを作成します。 `docker push <your_docker_username>/my-nextjs-app:v1`

4. Docker Hub でリポジトリを表示し、説明を入力し、(必要に応じて) GitHub アカウントをリンクすることで、次の情報にアクセスできるようになりました。 https://cloud.docker.com/repository/list

5. また、: `docker container ls` (または `docker ps`) を使用して、アクティブな Docker コンテナーの一覧を表示することもできます。

6. "My nextjs-app: v1" コンテナーがポート 3333-> 3000/tcp でアクティブになっていることを確認します。 ここには、"コンテナー ID" も表示されます。 コンテナーの実行を停止するには、次のコマンドを入力します: `docker stop <container ID>`

7. 通常、コンテナーが停止すると、削除もされるはずです。 コンテナーを削除すると、残されたすべてのリソースがクリーンアップされます。 コンテナーを削除すると、そのイメージファイルシステム内で行った変更は完全に失われます。 変更を反映するには、新しいイメージを作成する必要があります。 コンテナーを削除するには、次のコマンドを使用します: `docker rm <container ID>`

[Docker でコンテナー化された web アプリケーションを構築する](https://docs.microsoft.com/learn/modules/intro-to-containers/)方法について説明します。

## <a name="deploy-to-azure-container-registry"></a>Azure コンテナー レジストリへのデプロイ

[**Azure Container Registry**](https://azure.microsoft.com/services/container-registry/) (ACR) を使用すると、コンテナーイメージを保存、管理し、プライベートで認証されたリポジトリに安全に保管することができます。 標準の Docker コマンドと互換性があるため、ACR はコンテナーの正常性の監視とメンテナンスを行うための重要なタスクを処理し、 [Kubernetes](https://docs.microsoft.com/azure/aks/intro-kubernetes)と組み合わせてスケーラブルなオーケストレーションシステムを作成できます。 必要に応じてビルドするか､またはソースコードのコミットやベース イメージの更新などのトリガーでビルドを完全に自動化します｡ また、ACR は、大規模な Azure クラウドネットワークを活用して、ネットワーク待ち時間やグローバルデプロイを管理し、 [Azure App Service](https://docs.microsoft.com/azure/app-service/) (web ホスティング、モバイルバックエンド、REST api など) を使用するすべてのユーザーにシームレスなネイティブエクスペリエンスを作成[します。](https://azure.microsoft.com/product-categories/containers/)

> [!IMPORTANT]
> コンテナーを Azure にデプロイするには、独自の Azure サブスクリプションが必要です。料金が発生する場合があります。 Azure サブスクリプションをお持ちでない場合は、開始する前に[無料アカウントを作成](https://azure.microsoft.com/free/)してください。

Azure Container Registry を作成してアプリコンテナーイメージをデプロイする方法の詳細については、「演習: [Azure Container Instance への Docker イメージのデプロイ](https://docs.microsoft.com/learn/modules/intro-to-containers/7-exercise-deploy-docker-image-to-container-instance)」を参照してください。

## <a name="additional-resources"></a>その他の資料

- [Azure での Node.js](https://azure.microsoft.com/develop/nodejs/)
- クイックスタート: [Azure で node.js web アプリを作成する](https://docs.microsoft.com/azure/app-service/app-service-web-get-started-nodejs)
- オンラインコース: [Azure でコンテナーを管理](https://docs.microsoft.com/learn/paths/administer-containers-in-azure/)する
- VS Code の使用: [Docker の操作](https://code.visualstudio.com/docs/azure/docker)
- Docker ドキュメント: [Docker Desktop WSL 2 Tech Preview](https://docs.docker.com/docker-for-windows/wsl-tech-preview/)
