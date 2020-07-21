---
title: Node.js アプリのデータベースへの接続の概要
description: Windows で Node.js アプリのデータベースへの接続を開始します。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: NodeJS, Node.js, Windows 10, Microsoft, ラーニング NodeJS, Windows 上のノード, WSL 上のノード, Windows 上の Linux 上のノード, Windows 上のインストール ノード, NodeJS と VS Code, Windows 上のノードでの開発, Windows 上の NodeJS での開発, WSL 上のインストール ノード, Linux 用 Windows サブシステム上の NodeJS
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: 63c47107538d8744201f83ea1be24cfaf3193f4f
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/29/2020
ms.locfileid: "72517815"
---
# <a name="get-started-using-mongodb-or-postgresql-with-nodejs-on-windows"></a>Windows での Node.js に関する MongoDB または PostgreSQL の使用の概要

Node.js アプリケーションは多くの場合、データを永続化する必要があります。これは、ファイル、ローカル ストレージ、クラウド サービス、またはデータベース経由で行われる場合があります。 このステップ バイ ステップ ガイドは、Node.js アプリのデータベースへの接続を開始するために役立ちます。 焦点を当てることにした 2 つの一般的なオプションは、MongoDB と PostgreSQL です。

## <a name="prerequisites"></a>前提条件

このガイドでは、読者が [WSL 2 を使用して Node.js 開発環境を設定する](./setup-on-wsl2.md)手順を既に完了していることを前提にしています。これには以下が含まれます。

- Windows 10 Insider Preview ビルド 18932 以降をインストールします。
- Windows で WSL 2 機能を有効にします。
- Linux ディストリビューション (この例では Ubuntu 18.04) をインストールします。 これは `wsl lsb_release -a` で確認できます。
- Ubuntu 18.04 ディストリビューションが WSL 2 モードで実行されていることを確認します。 (WSL は、v1 または v2 モードの両方でディストリビューションを実行できます。)これは PowerShell を開き、「`wsl -l -v`」と入力することによって確認できます。
- PowerShell を使用して、`wsl -s ubuntu 18.04` で、Ubuntu 18.04 を既定のディストリビューションとして設定します。

## <a name="differences-between-mongodb-and-postgresql"></a>MongoDB と PostgreSQL の違い

[!INCLUDE [Postgres vs Mongo](../includes/postgres-v-mongo.md)]

## <a name="install-mongodb"></a>MongoDB のインストール

[!INCLUDE [Install and run Mongo](../includes/install-and-run-mongo.md)]

### <a name="vs-code-support-for-mongodb"></a>VS Code の MongoDB サポート

VS Code では、[Azure CosmosDB 拡張機能](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb)による MongoDB データベースの操作がサポートされており、VS Code 内から MongoDB データベースの作成、管理、クエリ実行を行うことができます。

詳細については、次に関する VS Code のドキュメントを参照してください:[MongoDB の操作](https://code.visualstudio.com/docs/azure/mongodb)。

詳細については、次の MongoDB のドキュメントを参照してください。

- [MongoDB の概要](https://docs.mongodb.com/manual/introduction/)
- [ユーザーの作成](https://docs.mongodb.com/manual/tutorial/create-users/)
- [リモート ホスト上の MongoDB インスタンスへの接続](https://docs.mongodb.com/manual/mongo/#mongodb-instance-on-a-remote-host)
- [CRUD: 作成、読み取り、更新、削除](https://docs.mongodb.com/manual/crud/)
- [リファレンス ドキュメント](https://docs.mongodb.com/manual/reference/)

## <a name="install-postgresql"></a>PostgreSQL のインストール

[!INCLUDE [Install and run PostgresQL](../includes/install-and-run-postgres.md)]

### <a name="vs-code-support-for-postgresql"></a>VS Code の PostgreSQL サポート

VS Code では、[PostgreSQL 拡張機能](https://marketplace.visualstudio.com/items?itemName=ms-ossdata.vscode-postgresql)を使用した PostgreSQL データベースの操作がサポートされており、VSCode 内から PostgreSQL データベースを作成し、データベースに接続し、管理し、クエリを実行することができます。

## <a name="set-up-profile-aliases"></a>プロファイルのエイリアスの設定

[!INCLUDE [Set up profile aliases](../includes/profile-aliases.md)]
