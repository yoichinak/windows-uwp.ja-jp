---
title: データベースへの node.js アプリの接続の概要
description: Windows 上のデータベースへの node.js アプリの接続を開始します。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: NodeJS、node.js、windows 10、microsoft、learning NodeJS、windows 上のノード、wsl のノード、windows 上の linux 上のノード、windows 上のノードのインストール、windows 上のノードのインストール、windows 上の NodeJS を使用した開発、windows 上のノードのインストール、WSL へのノードのインストール、Windows 上の NodeJSLinux 用サブシステム
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: 63c47107538d8744201f83ea1be24cfaf3193f4f
ms.sourcegitcommit: 60d2d15dd0d365f82e4e90e4bc34b40cf5b4a247
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/17/2019
ms.locfileid: "72517815"
---
# <a name="get-started-using-mongodb-or-postgresql-with-nodejs-on-windows"></a>Windows 上の node.js で MongoDB または PostgreSQL を使ってみる

Node.js アプリケーションでは、多くの場合、ファイル、ローカルストレージ、クラウドサービス、またはデータベースを通じてデータを永続化する必要があります。 このステップバイステップガイドでは、node.js アプリをデータベースに接続する方法について説明します。 ここでは、MongoDB と PostgreSQL という2つの一般的なオプションに注目します。

## <a name="prerequisites"></a>前提条件

このガイドでは、次のよう[な WSL 2 を使用して node.js 開発環境を設定](./setup-on-wsl2.md)する手順を既に完了していることを前提としています。

- Windows 10 Insider Preview ビルド18932以降をインストールします。
- Windows で WSL 2 機能を有効にします。
- Linux ディストリビューションをインストールします (この例では Ubuntu 18.04)。 これを確認するには、次のようにします。 `wsl lsb_release -a`
- Ubuntu 18.04 のディストリビューションが WSL 2 モードで実行されていることを確認します。 (WSL では、v1 モードと v2 モードの両方でディストリビューションを実行できます)。これを確認するには、PowerShell を開き、次のように入力します。 `wsl -l -v`
- PowerShell を使用して、Ubuntu 18.04 を既定のディストリビューションとして設定します。 `wsl -s ubuntu 18.04`

## <a name="differences-between-mongodb-and-postgresql"></a>MongoDB と PostgreSQL の違い

[!INCLUDE [Postgres vs Mongo](../includes/postgres-v-mongo.md)]

## <a name="install-mongodb"></a>MongoDB のインストール

[!INCLUDE [Install and run Mongo](../includes/install-and-run-mongo.md)]

### <a name="vs-code-support-for-mongodb"></a>MongoDB の VS Code サポート

VS Code は、 [Azure CosmosDB 拡張機能](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb)を使用した mongodb データベースの操作をサポートしており、VS Code 内から mongodb データベースを作成、管理、クエリできます。

詳細については、「VS Code ドキュメント: [MongoDB の](https://code.visualstudio.com/docs/azure/mongodb)使用」を参照してください。

詳細については、MongoDB のドキュメントを参照してください。

- [MongoDB の使用の概要](https://docs.mongodb.com/manual/introduction/)
- [ユーザーの作成](https://docs.mongodb.com/manual/tutorial/create-users/)
- [リモートホスト上の MongoDB インスタンスに接続する](https://docs.mongodb.com/manual/mongo/#mongodb-instance-on-a-remote-host)
- [CRUD: 作成、読み取り、更新、削除](https://docs.mongodb.com/manual/crud/)
- [リファレンスドキュメント](https://docs.mongodb.com/manual/reference/)

## <a name="install-postgresql"></a>PostgreSQL のインストール

[!INCLUDE [Install and run PostgresQL](../includes/install-and-run-postgres.md)]

### <a name="vs-code-support-for-postgresql"></a>PostgreSQL の VS Code サポート

Postgresql[拡張](https://marketplace.visualstudio.com/items?itemName=ms-ossdata.vscode-postgresql)機能を使用した postgresql データベースの操作をサポート VS Code は、VS Code 内から postgresql データベースを作成、接続、管理、照会することができます。

## <a name="set-up-profile-aliases"></a>プロファイルのエイリアスを設定する

[!INCLUDE [Set up profile aliases](../includes/profile-aliases.md)]
