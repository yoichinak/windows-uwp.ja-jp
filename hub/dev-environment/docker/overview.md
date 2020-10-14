---
title: コンテナーを使用するリモート開発のための Docker の概要
description: Windows または WSL で Docker Desktop を使用するためのガイド。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
ms.technology: windows-nodejs
keywords: Microsoft, Windows, Docker, WSL, リモート開発, コンテナー, Docker Desktop, Windows と WSL の比較
ms.date: 09/24/2020
ms.openlocfilehash: b3fc2509aa6a623bebd9f4566f3b8e75301251db
ms.sourcegitcommit: c65f62bda57563f6196691e7b9c25cbf5a8b16e5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/07/2020
ms.locfileid: "91780558"
---
# <a name="overview-of-docker-remote-development-on-windows"></a>Windows での Docker リモート開発の概要

リモート開発のためにコンテナーを使用し、アプリケーションを Docker プラットフォームでデプロイすることはとても一般的なソリューションであり、多くの利点があります。 Linux 用 Windows サブシステム (WSL)、Visual Studio、Visual Studio Code、.NET、幅広い Azure サービスを含む Microsoft ツールとサービスによって可能になる、様々なサポートについて説明します。

## <a name="docker-on-windows-10"></a>Windows 10 上の Docker

:::row:::
    :::column:::
       [![Docker ドキュメントのアイコン](../../images/docker-docs-icon.png)](https://docs.docker.com/docker-for-windows/install/)<br>
        **[Install Docker Desktop for Windows](https://docs.docker.com/docker-for-windows/install/)**<br>
        インストールの手順、システム要件、インストーラーに含まれるもの、アンインストールの方法、安定バージョンとエッジ バージョンの違い、Windows コンテナーと Linux コンテナーを切り替える方法について説明します。
    :::column-end:::
    :::column:::
       [![実行中の Docker のスクリーンショット](../../images/docker-running-screenshot.png)](https://docs.docker.com/get-started/)<br>
        **[Docker の概要](https://docs.docker.com/get-started/)**<br>
        初めて使用するためのステップバイステップの手順を含む、Docker のオリエンテーションと設定に関するドキュメント (ビデオ チュートリアルも含まれます)。
    :::column-end:::
    :::column:::
       [![Microsoft Learn Docker コースのスクリーンショット](../../images/docker-learn-course.png)](/learn/modules/intro-to-docker-containers/)<br>
        **[MS Learn コース:Docker コンテナーの紹介](/learn/modules/intro-to-docker-containers/)**<br>
        Microsoft Learn には、Docker コンテナーに関する無料のイントロダクション コースを始め、Docker の概要や Azure サービスとの接続についての[様々なコース](/learn/browse/?terms=docker)が用意されています。
    :::column-end:::
    :::column:::
       [![Docker Desktop WSL2 メニューのスクリーンショット](../../images/docker-wsl2.png)](/windows/wsl/tutorials/wsl-containers)<br>
        **[WSL 2 での Docker リモート コンテナーの概要](/windows/wsl/tutorials/wsl-containers)**<br>
        WSL 2 (Linux 用 Windows サブシステム、バージョン 2) で Linux コマンド ライン (Ubuntu、Debian、SUSE など) を使用するために Docker Desktop for Windows を設定する方法について説明します。
    :::column-end:::
:::row-end:::

## <a name="vs-code-and-docker"></a>VS Code と Docker

:::row:::
    :::column:::
       [![VS Code のリモート コンテナーの図](../../images/vscode-remote-containers.png)](https://code.visualstudio.com/docs/remote/create-dev-container)<br>
        **[VS Code を使用して Docker コンテナーを作成する](https://code.visualstudio.com/docs/remote/containers-tutorial)**<br>
        [Remote - Containers 拡張機能](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)を使用してコンテナー内ですべての機能を備えた開発環境を設定する方法について説明します。また、[NodeJS コンテナー](https://code.visualstudio.com/docs/containers/quickstart-node)、[Python コンテナー](https://code.visualstudio.com/docs/containers/quickstart-python)、[ASP.NET Core コンテナー](https://code.visualstudio.com/docs/containers/quickstart-aspnet-core) の設定についてのチュートリアルもあります。
    :::column-end:::
    :::column:::
       [![VSCode での Docker のアタッチのスクリーンショット](../../images/vscode-attach-docker.png)](https://code.visualstudio.com/docs/remote/attach-container)<br>
        **[VS Code を Docker コンテナーにアタッチする](https://code.visualstudio.com/docs/remote/attach-container)**<br>
        Visual Studio Code を既に実行されている Docker コンテナーまたは [Kubernetes クラスター内のコンテナー](https://code.visualstudio.com/docs/remote/attach-container#_attach-to-a-container-in-a-kubernetes-cluster) にアタッチする方法について説明します。
    :::column-end:::
    :::column:::
       [![VSCode コンテナー メニューのスクリーンショット](../../images/vscode-advanced-docker.png)](https://code.visualstudio.com/docs/remote/containers-advanced)<br>
        **[高度なコンテナーの構成](https://code.visualstudio.com/docs/remote/containers-advanced)**<br>
        Visual Studio Code で Docker コンテナーを使用するための高度な設定のシナリオについて説明します。または、VS Code でのデバッグのために[コンテナーを検証する](https://code.visualstudio.com/blogs/2019/10/31/inspecting-containers)方法についての記事をご覧ください。
    :::column-end:::
    :::column:::
       [![VSCode Docker Desktop と WSL の使用のスクリーンショット](../../images/vscode-docker-wsl.png)](https://code.visualstudio.com/blogs/2020/07/01/containers-wsl)<br>
        **[WSL 2 でリモート コンテナーを使用する](https://code.visualstudio.com/blogs/2020/07/01/containers-wsl)**<br>
        WSL 2 (Linux 用 Windows サブシステム、バージョン 2) での Docker コンテナーの使用、および VS Code ですべての設定を行う方法について説明します。 [機能のしくみ](https://code.visualstudio.com/blogs/2020/03/02/docker-in-wsl2#_how-it-works)についても説明されています。
    :::column-end:::
:::row-end:::

## <a name="visual-studio-and-docker"></a>Visual Studio と Docker

:::row:::
    :::column:::
       [![Visual Studio アイコン](../../images/visualstudio.png)](/visualstudio/containers/overview#docker-support-in-visual-studio-1)<br>
        **[Visual Studio での Docker サポート](/visualstudio/containers/overview#docker-support-in-visual-studio-1)**<br>
        Visual Studio での ASP.NET プロジェクト、ASP.NET Core プロジェクト、.NET Core および .NET Framework コンソール プロジェクトで利用できる Docker サポートについて、さらにコンテナー オーケストレーションのサポートについて説明します。
    :::column-end:::
    :::column:::
       [![Visual Studio Docker のメニュー](../../images/visualstudio-docker-menu.png)](/visualstudio/containers/container-tools)<br>
        **[クイック スタート: Visual Studio での Docker](/visualstudio/containers/container-tools)**<br>
        Visual Studio を使用して、コンテナー化された .NET、ASP.NET、ASP.NET Core アプリの構築、デバッグ、実行を行う方法と、それらを Azure Container Registry (ACR)、Docker Hub、Azure App Service、独自のコンテナー レジストリに公開する方法について説明します。
    :::column-end:::
    :::column:::
       [![VS チュートリアルのスクリーンショット](../../images/visualstudio-tutorial.png)](/visualstudio/containers/tutorial-multicontainer)<br>
        **[チュートリアル: Docker Compose を使用して複数コンテナーのアプリを作成する](/visualstudio/containers/tutorial-multicontainer)**<br>
        Visual Studio のコンテナー ツールを使用して複数のコンテナーを管理し、それらの間で通信を行う方法について説明します。 [React シングルページ アプリで Docker を使用する](/visualstudio/containers/container-tools-react)方法などについてのチュートリアルのリンクも含まれています。
    :::column-end:::
    :::column:::
       [![VS コンテナーのリンク](../../images/visualstudio-container-links.png)](/visualstudio/containers)<br>
        **[Visual Studio のコンテナー ツール](/visualstudio/containers)**<br>
        Visual Studio を使用して、コンテナーでのビルド ツールの実行、[Docker アプリのデバッグ](/visualstudio/containers/edit-and-refresh)、開発ツールのトラブルシューティング、Docker コンテナーのデプロイ、Kubernetes のブリッジを行う方法が紹介されています。
    :::column-end:::
:::row-end:::

![コンテナー、イメージ、レジストリについての基本的な Docker の分類を表す説明画像](../../images/taxonomy-of-docker-terms-and-concepts.png)

## <a name="net-core-and-docker"></a>.NET Core と Docker

:::row:::
    :::column:::
       [![.NET マイクロサービス ガイドの表紙](../../images/dotnet-microservice-guide.png)](/dotnet/architecture/microservices/)<br>
        **[.NET ガイド:マイクロサービス アプリとコンテナー](/dotnet/architecture/microservices/)**<br>
        コンテナーで管理されるマイクロサービス ベース アプリについての概要ガイド。
    :::column-end:::
    :::column:::
       [![Docker の説明画像](../../images/dotnet-docker-infographic.png)](/dotnet/architecture/microservices/container-docker-introduction/docker-defined)<br>
        **[Docker とは](/dotnet/architecture/microservices/container-docker-introduction/docker-defined)**<br>
        「[Docker コンテナーと仮想マシンの比較](/dotnet/architecture/microservices/container-docker-introduction/docker-defined#comparing-docker-containers-with-virtual-machines)」や、コンテナー、イメージ、レジストリの違いを説明する [Docker の用語と概念の基本的な分類](/dotnet/architecture/microservices/container-docker-introduction/docker-containers-images-registries)を含む、Docker コンテナーの基本について説明します。
    :::column-end:::
    :::column:::
       [![Docker の分類の説明画像](../../images/taxonomy-of-docker-terms-and-concepts.png)](/dotnet/core/docker/build-container?tabs=windows)<br>
        **[チュートリアル:.NET Core アプリをコンテナー化する](/dotnet/core/docker/build-container?tabs=windows)**<br>
        Docker を使用して .NET Core アプリケーションをコンテナー化する方法について説明します。Dockerfile の作成、必須コマンド、リソースのクリーンアップの説明も含まれます。
    :::column-end:::
    :::column:::
       [![Docker を使用する内部ループ開発ワークフローの説明画像](../../images/dotnet-docker-workflow.png)](/dotnet/architecture/microservices/docker-application-development-process/docker-app-development-workflow)<br>
        **[Docker アプリの開発ワークフロー](/dotnet/architecture/microservices/docker-application-development-process/docker-app-development-workflow)**<br>
        Docker コンテナーベース アプリケーションの内部ループ開発ワークフローについて説明します。
    :::column-end:::
:::row-end:::

## <a name="azure-container-services"></a>Azure Container Service

:::row:::
    :::column:::
       [![Azure Container Instances のスクリーンショット](../../images/azure-container-instances.png)](/azure/container-instances/)<br>
        **[Azure Container Instances](/azure/container-instances/)**<br>
        サーバーレスのマネージド Azure 環境で、Docker コンテナーをオンデマンドで実行する方法について説明します。Docker CLI、ARM、Azure portal を使用してデプロイする方法、複数コンテナー グループの作成、コンテナー間でのデータの共有、仮想ネットワークへの接続などについても説明します。
    :::column-end:::
    :::column:::
       [![Azure Container Registry のスクリーンショット](../../images/azure-container-registry-icon.png)](/azure/container-registry)<br>
        **[Azure Container Registry](/azure/container-registry)**<br>
        あらゆる種類のコンテナー デプロイのためのプライベート レジストリで、コンテナー イメージと成果物のビルド、保存、管理を行う方法について説明します。 既存のコンテナー開発とデプロイ パイプラインのための Azure コンテナー レジストリを作成し、自動化タスクを設定します。geo レプリケーションとベスト プラクティスを含むレジストリの管理方法についても説明します。
    :::column-end:::
    :::column:::
       [![Azure Service Fabric のスクリーンショット](../../images/azure-service-fabric.png)](/azure/service-fabric)<br>
        **[Azure Service Fabric](/azure/service-fabric)**<br>
        スケーラブルで信頼性の高いマイクロサービスとコンテナーのパッケージ化、デプロイ、管理のための分散システム プラットフォームである、Azure Service Fabric について説明します。
    :::column-end:::
    :::column:::
       [![Azure App Service のスクリーンショット](../../images/azure-app-service.png)](/azure/app-service)<br>
        **[Azure App Service](/azure/app-service)**<br>
        インフラストラクチャを管理することなく、好みのプログラミング言語を使用して、Web アプリ、モバイル バック エンド、RESTful API を構築してホストする方法について説明します。 [MS Learn の Azure App Service コース](/learn/modules/deploy-run-container-app-service)を試して、Docker イメージに基づいて Web アプリをデプロイし、継続的デプロイを構成しましょう。
    :::column-end:::
:::row-end:::

[コンテナーをサポートするこれら以外の Azure サービス](https://azure.microsoft.com/overview/containers/)についてもご確認ください。

## <a name="docker-containers-explainer-video"></a>Docker コンテナーの紹介ビデオ

> [!VIDEO https://www.youtube.com/embed/0oEsMwSxBsk]

## <a name="kubernetes-and-container-orchestration-explainer-video"></a>Kubernetes とコンテナーのオーケストレーションに関する紹介ビデオ

> [!VIDEO https://www.youtube.com/embed/3RTvoI-A7UQ]

## <a name="containers-on-windows"></a>Windows のコンテナー

:::row:::
    :::column:::
       [![Windows Server コンテナーのアイコン](../../images/windows-server-containers.png)](/virtualization/windowscontainers)<br>
        **[Windows のコンテナーに関するドキュメント](/virtualization/windowscontainers)**<br>
        アプリを依存関係と共にパッケージ化し、オペレーティング システム レベルの仮想化を利用して、完全に分離された高速な環境を単一のシステム上に実現します。 [Windows のコンテナー](/virtualization/windowscontainers/about)に関する記事をご覧ください。クイック スタート、デプロイ ガイド、サンプルが含まれます。
    :::column-end:::
    :::column:::
       [![FAQ アイコン](../../images/faq.png)](/virtualization/windowscontainers/about/faq)<br>
        **[Windows コンテナーに関する FAQ](/virtualization/windowscontainers/about/faq)**<br>
        コンテナーについてよく寄せられる質問をご覧ください。 [「What's the difference between Docker for Windows and Docker on Windows?」\(Docker for Windows と Windows 上の Docker の違いは何ですか?\)](https://stackoverflow.com/questions/38464724/whats-the-difference-between-docker-for-windows-and-docker-on-windows/40320748) で、StackOverflow でのその説明もご確認ください。
    :::column-end:::
    :::column:::
       [![Windows コンテナーのアイコン](../../images/windows-container.png)](/virtualization/windowscontainers/quick-start/set-up-environment?tabs=Windows-10-Client)<br>
        **[環境を設定する](/virtualization/windowscontainers/quick-start/set-up-environment?tabs=Windows-10-Client)**<br>
        コンテナーの作成、実行、デプロイを行うために Windows 10 または Windows Server を設定する方法について説明します。前提条件、Docker のインストール、[Windows コンテナーの基本イメージ](/virtualization/windowscontainers/manage-containers/container-base-images)を使用する方法についても説明します。
    :::column-end:::
    :::column:::
       [![AKS アイコン](../../images/kubernettes.png)](/azure/aks/windows-container-cli)<br>
        **[Azure Kubernetes Service (AKS) 上に Windows Server コンテナーを作成する](/azure/aks/windows-container-cli)**<br>
        Azure CLI を使用して Windows Server コンテナーの ASP.NET サンプル アプリを AKS クラスターにデプロイする方法について説明します。
    :::column-end:::
:::row-end:::
