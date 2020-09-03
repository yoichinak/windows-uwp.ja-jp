---
title: Node.js で Docker コンテナーを使ってみる
description: Node.js アプリで Docker コンテナーの使用を開始する際に役立つステップ バイ ステップ ガイドです。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: ''
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: a1bd1b0f2916ccf44cc79d83f0335f55cf3863e4
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89166626"
---
# <a name="get-started-using-docker-containers-with-nodejs"></a>Node.js で Docker コンテナーを使ってみる

Node.js アプリで Docker コンテナーの使用を開始する際に役立つステップ バイ ステップ ガイドです。

## <a name="prerequisites"></a>前提条件

このガイドでは、読者が [WSL 2 を使用して Node.js 開発環境を設定する](./setup-on-wsl2.md)手順を既に完了していることを前提にしています。これには以下が含まれます。

- Windows 10 Insider Preview ビルド 18932 以降をインストールします。
- Windows で WSL 2 機能を有効にします。
- Linux ディストリビューション (この例では Ubuntu 18.04) をインストールします。 これは `wsl lsb_release -a` で確認できます。
- Ubuntu 18.04 ディストリビューションが確実に WSL 2 モードで実行されるようにします。 (WSL は、v1 または v2 モードの両方でディストリビューションを実行できます)。これは PowerShell を開き、「`wsl -l -v`」と入力することによって確認できます。
- PowerShell を使用して、`wsl -s ubuntu 18.04` で、Ubuntu 18.04 を既定のディストリビューションとして設定します。

## <a name="overview-of-docker-containers"></a>Docker コンテナーの概要

**Docker** は、コンテナーを使用してアプリケーションを作成、展開、および実行するために使用されるツールです。 コンテナーを使用すると、開発者はアプリを必要なすべての部分 (ライブラリ、フレームワーク、依存関係など) とともにパッケージ化し、すべてを 1 つのパッケージとして出荷できます。 コンテナーを使用すると、カスタマイズされた設定や、アプリを実行しているコンピューターに以前にインストールされていたライブラリが、アプリのコードの記述とテストに使用されたマシンと異なる可能性がある場合でも、アプリが同じように実行されることが保証されます。 これにより、開発者は、コードが実行されるシステムの心配をすることなく、コードの記述に集中できます。

Docker コンテナーは仮想マシンに似ていますが、仮想オペレーティング システム全体を作成するわけではありません。 代わりに、Docker により、アプリが実行されているシステムと同じ Linux カーネルを使用することができます。 これにより、アプリ パッケージにはホスト コンピューター上にまだ存在しない部分だけが必要になるため、パッケージ サイズが減り、パフォーマンスを向上させることができます。

[Kubernetes](/azure/aks/) などのツールで Docker コンテナーを使用する、継続的な可用性が、コンテナーの人気のもう 1 つの理由です。 これにより、複数のバージョンのアプリ コンテナーを異なるタイミングで作成できます。 更新またはメンテナンスのためにシステム全体を停止するのではなく、各コンテナー (およびその特定のマイクロサービス) をすぐに置き換えることができます。 すべての更新プログラムが含まれた新しいコンテナーを準備し、運用環境用にコンテナーを設定して、新しいコンテナーの準備ができたら、それをポイントするだけです。 また、コンテナーを使用して異なるバージョンのアプリをアーカイブし、必要に応じて安全のフォールバックとしてそれらを実行し続けることもできます。

## <a name="install-docker-desktop-wsl-2-tech-preview"></a>Docker Desktop WSL 2 Tech Preview のインストール

以前は、WSL 1 では Docker デーモンを直接実行できませんでしたが、WSL 2 で変更され、Docker Desktop for WSL 2 により速度とパフォーマンスが大幅に向上しました。

Docker Desktop WSL 2 Tech Preview をインストールして実行するには、次の手順を行います。

1. [Docker Desktop WSL 2 Tech Preview インストーラー](https://download.docker.com/win/edge/36883/Docker%20Desktop%20Installer.exe)をダウンロードします (必要に応じて、[インストーラー ドキュメント](https://docs.docker.com/docker-for-windows/wsl-tech-preview/)を参照できます)。

2. ダウンロードした Docker インストーラーを開きます。 インストール ウィザードでは、[Use Windows containers instead of Linux containers]\(Linux コンテナーではなく Windows コンテナーを使用する\) かどうかが問われますが、ここでは Linux サブシステムを使用するため、これはオフのままにします。 Docker は、既定の WSL 2 ディストリビューションの管理対象ディレクトリにインストールされ、Docker デーモン、CLI、および Compose CLI が含まれます。

    ![Docker Desktop の起動](../images/install-docker-1.png)

3. まだ Docker ID をお持ちでない場合は、[https://hub.docker.com/signup](https://hub.docker.com/signup) にアクセスして設定する必要があります。 ID は、すべて小文字の英数字にする必要があります。

4. インストールが完了したら、デスクトップ上のショートカット アイコンを選択するか、Windows の [スタート] メニューで検索して、Docker Desktop を起動します。 Docker アイコンは、タスクバーの隠れているインジケーターのメニューに表示されます。 アイコンを右クリックして Docker コマンド メニューを表示し、[WSL 2 Tech Preview] を選択します。

5. Tech Preview ウィンドウが開いたら、 **[Start]\(開始\)** を選択して、WSL 2 で Docker デーモンの実行 (バックグラウンド プロセス) を開始します。 WSL 2 Docker デーモンが開始されると、それに対して Docker CLI コンテキストが自動的に作成されます。

    ![Docker Desktop の起動](../images/start-docker.gif)

6. Docker がインストールされていることを確認し、バージョン番号を表示するには、コマンドライン (WSL または PowerShell) を開き、`docker --version` を入力します。

7. 単純な組み込みの Docker イメージを実行して、インストールが正しく機能することをテストします。 `docker run hello-world`

理解しておくべき Docker コマンドをいくつか次に示します。

- `docker` を入力して、Docker CLI で使用できるコマンドを一覧表示します。
- `docker <COMMAND> --help` を使用して、特定のコマンドの情報を一覧表示します。
- `docker image ls --all` を使用して、お使いのマシン上の docker イメージを一覧表示します (この時点では hello world イメージのみ)。
- `docker container ls --all` を使用して、お使いのマシン上のコンテナーを一覧表示します。
- `docker info` を使用して、WSL 2 コンテキストで使用可能な Docker システムの統計とリソース (CPU とメモリ) を一覧表示します。
- `docker context ls` を使用して、docker が現在実行されている場所を表示します。

Docker が実行されているコンテキストには、`default` (クラシック Docker デーモン) と `wsl` (Tech Preview を使用したこの推奨事項) の 2 つがあることがわかります。 (また、`ls` コマンドは `list` の省略形で、同じ意味で使用できます)。

![PowerShell での Docker 表示コンテキスト](../images/docker-context.png)

> [!TIP]
> [Docker Hub でこのチュートリアル](https://hub.docker.com/?overlay=onboarding)を使用して、サンプルの Docker イメージを作成してみてください。 Docker Hub には、多数のオープンソース イメージも含まれており、コンテナー化したいアプリの種類に一致するものが見つかる可能性があります。 この [Gatsby.js framework container](https://hub.docker.com/r/gatsbyjs/gatsby-dev-builds) や、この [Nuxt.js framework container](https://hub.docker.com/r/hobord/nuxtexpress) などのイメージをダウンロードし、独自のアプリケーション コードで拡張することができます。 [コマンド ラインから Docker](https://docs.docker.com/engine/reference/commandline/search/) を使用して、または [Docker Hub の Web サイト](https://hub.docker.com/search/?type=image)で、レジストリを検索できます。

## <a name="install-the-docker-extension-on-vs-code"></a>VS Code に Docker 拡張機能をインストールする

Docker 拡張機能を使用すると、コンテナー化されたアプリケーションのビルド、管理、および Visual Studio Code からの展開を簡単に行うことができます。

1. VS Code で **[拡張機能]** ウィンドウを開き (Ctrl + Shift + X)、**Docker** を検索します。

2. [Microsoft Docker 拡張機能](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker)を選択し、**インストール**します。 拡張機能を有効にするには、インストールした後に VS Code を再読み込みする必要があります。

    ![Remote-WSL の VS Code での Docker 拡張機能](../images/docker-vscode-extension.png)

VS Code に Docker 拡張機能をインストールすることにより、次のセクションで使用する `Dockerfile` のコマンドの一覧を、ショートカット キー `Ctrl+Space` を使用して表示できるようになります。

VS Code での Docker の使用に関する詳細は、[こちら](https://code.visualstudio.com/docs/azure/docker)をご覧ください。

## <a name="create-a-container-image-with-dockerfile"></a>DockerFile を使用してコンテナー イメージを作成する

**コンテナー イメージ**には、アプリケーション コード、ライブラリ、構成ファイル、環境変数、ランタイムが格納されます。 イメージを使用することで、コンテナー内の環境が標準化され、アプリケーションのビルドと実行に必要なものだけが含まれるようになります。

**DockerFile** には、新しいコンテナー イメージをビルドするために必要な命令が含まれています。 つまり、このファイルにより、アプリの環境どこでも再現できるように定義するコンテナーイ メージがビルドされます。

[Web フレームワーク](./web-frameworks.md) ガイドで設定した Next.js アプリを使用して、コンテナー イメージをビルドしてみましょう。

1. VS Code で Next.js アプリを開きます (左下の緑色のタブに示されているように、Remote-WSL 拡張機能が実行されていることを確認します)。 VS Code に統合されている WSL ターミナルを開き ( **[View]\(\表示) > [Terminal]\(ターミナル\)** )、ターミナル パスで Next.js プロジェクト ディレクトリ (`~/NextProjects/my-next-app$`) がポイントされていることを確認します。

2. Next.js プロジェクトのルートに `Dockerfile` という名前の新しいファイルを作成し、次を追加します。

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

3. Docker イメージをビルドするには、プロジェクトのルートから次のコマンドを実行します (ただし、`<your_docker_username>` は、Docker Hub で作成したユーザー名に置き換えます)。`docker build -t <your_docker_username>/my-nextjs-app .`

> [!NOTE]
> このコマンドを機能させるには、Docker が WSL Tech Preview を使用して実行されている必要があります。 Docker の起動方法を思い出すには、インストール セクションの[手順 #4](#install-docker-desktop-wsl-2-tech-preview) を参照してください。 `-t` フラグは、作成されるイメージの名前を指定します。この場合は "my nextjs-app: v1" です。 イメージの作成時には、常に[タグ名にバージョン番号を使用](https://medium.com/@mccode/the-misunderstood-docker-tag-latest-af3babfd6375)することをお勧めします。 コマンドの最後には必ずピリオドを含めるようにしてください。これにより、Next.js アプリのビルド ファイルの検索およびコピーに使用する必要がある現在の作業ディレクトリが指定されます。

4. コンテナー内の Next.js アプリのこの新しい docker イメージを実行するには、次のコマンドを入力します。`docker run -d -p 3333:3000 <your_docker_username>/my-nextjs-app:v1`

5. `-p` フラグにより、ポート '3000' (コンテナー内でアプリが実行されているポート) がお使いのマシン上のローカル ポート '3333' にバインドされるため、Web ブラウザーで [http://localhost:3333](http://localhost:3333) をポイントして、Docker コンテナー イメージとして実行されている、サーバー側のレンダリングされた Next.js アプリケーションを確認できるようになります。

> [!TIP]
> このコンテナー イメージは、Docker Hub に格納されている Node.js バージョン 12 の既定のイメージを参照する `FROM node:12` を使用してビルドしました。 この既定の Node.js イメージは、Debian/Ubuntu Linux システムに基づいており、さまざまな Node.js イメージから選択することができますが、さらに軽量なものや、ニーズに合わせて調整されたものを使用することも検討できます。 詳細については、[Docker Hub の Node.js イメージ レジストリ](https://hub.docker.com/_/node/)を参照してください。

## <a name="upload-your-container-image-to-a-repository"></a>コンテナー イメージをリポジトリにアップロードする

**コンテナー リポジトリ**では、コンテナー イメージがクラウドに格納されます。 多くの場合、コンテナー リポジトリには、関連するイメージ (バージョン違いなど) のコレクションが実際に含まれており、これらはすべて、簡単なセットアップや迅速なデプロイに使用できます。 通常は、セキュリティで保護された HTTPS エンドポイント経由でコンテナー リポジトリのイメージにアクセスできます。これにより、任意のシステム、ハードウェア、または VM インスタンスを使用してイメージをプル、プッシュ、または管理できます。

一方、**コンテナー リポジトリ**では、リポジトリのコレクションに加えて、インデックス、アクセス制御ルール、および API パスが格納されます。 これらは、パブリックまたはプライベートにホストできます。 [Docker Hub](https://hub.docker.com/) はオープン ソースの Docker レジストリで、`docker push` コマンドと `docker pull` コマンドを実行するときに既定で使用されます。 これはパブリック リポジトリには無料で、プライベート リポジトリには料金がかかります。

Docker Hub でホストされているリポジトリに新しいコンテナー イメージをアップロードするには、次の手順を行います。

1. Docker Hub にログインします。 インストール手順で、Docker Hub アカウントの作成に使用したユーザー名とパスワードを入力するように求められます。 ターミナルで Docker にログインするには、`docker login` を入力します。

2. ご使用のマシンで作成した docker コンテナー イメージの一覧を取得するには、`docker image ls --all` を入力します。

3. 次のコマンドを使用して、コンテナー イメージを Docker Hub にプッシュし、そこに新しいリポジトリを作成します。`docker push <your_docker_username>/my-nextjs-app:v1`

4. これで Docker Hub にリポジトリが表示されます。 https://cloud.docker.com/repository/list にアクセスして、説明を入力し、GitHub アカウントをリンクすることができます (必要な場合)。

5. また、`docker container ls` (または `docker ps`) を使用して、アクティブな Docker コンテナーの一覧を表示することもできます。

6. "my-nextjs-app:v1" コンテナーがポート 3333-> 3000/tcp でアクティブになっていることを確認してください。 ここには、自分の "CONTAINER ID" も一覧表示されます。 コンテナーの実行を停止するには、次のコマンドを入力します。`docker stop <container ID>`

7. 通常、コンテナーが停止すると、削除もされるはずです。 コンテナーを削除すると、残されたすべてのリソースがクリーンアップされます。 コンテナーを削除すると、そのイメージ ファイルシステム内で加えられた変更はすべて完全に失われます。 変更を反映するには、新しいイメージを作成する必要があります。 コンテナーを削除するには、`docker rm <container ID>` コマンドを使用します。

Docker を使用してコンテナー化された Web アプリケーションをビルドする詳細については、[こちら](/learn/modules/intro-to-containers/)をご覧ください。

## <a name="deploy-to-azure-container-registry"></a>Azure コンテナー レジストリへのデプロイ

[**Azure Container Registry**](https://azure.microsoft.com/services/container-registry/) (ACR) を使用すると、プライベートの認証されたリポジトリでコンテナー イメージを安全に格納、管理、保管することができます。 ACR は、標準の Docker コマンドと互換性があるため、コンテナーの正常性の監視、メンテナンス、[Kubernetes](/azure/aks/intro-kubernetes) とのペアリングなどの重要なタスクを処理して、スケーラブルなオーケストレーション システムを作成することができます。 必要に応じてビルドするか､またはソースコードのコミットやベース イメージの更新などのトリガーでビルドを完全に自動化します｡ また、ACR は、膨大な Azure クラウド ネットワークを活用してネットワーク待機時間、グローバル デプロイを管理し、[Azure App Service](/azure/app-service/) (Web ホスティング、モバイル バックエンド、REST API) や[その他の Azure Cloud Services](https://azure.microsoft.com/product-categories/containers/) を使用するすべての人にとってシームレスなネイティブ エクスペリエンスを作成します。

> [!IMPORTANT]
> コンテナーを Azure にデプロイするには、独自の Azure サブスクリプションが必要です。料金が発生する場合があります。 Azure サブスクリプションをお持ちでない場合は、開始する前に[無料アカウントを作成](https://azure.microsoft.com/free/)してください。

Azure Container Registry を作成してアプリ コンテナー イメージをデプロイするためのヘルプについては、演習「[Azure コンテナー インスタンスに Docker イメージをデプロイする](/learn/modules/intro-to-containers/7-exercise-deploy-docker-image-to-container-instance)」を参照してください。

## <a name="additional-resources"></a>その他のリソース

- [Azure での Node.js](https://azure.microsoft.com/develop/nodejs/)
- クイック スタート:[Azure で Node.js Web アプリを作成する](/azure/app-service/app-service-web-get-started-nodejs)
- オンライン コース:[Azure でコンテナーを管理する](/learn/paths/administer-containers-in-azure/)
- VS Code の使用:[Docker を使用する](https://code.visualstudio.com/docs/azure/docker)
- Docker ドキュメント:[Docker Desktop WSL 2 Tech Preview](https://docs.docker.com/docker-for-windows/wsl-tech-preview/)