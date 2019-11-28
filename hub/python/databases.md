---
title: Windows で Python を使ってみる (データベース)
description: Windows で PostgreSQL または MongoDB と Python を初めて使用するときに役立つガイドです。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: python, windows 10, postgresql, mongodb, postgres, mongo, microsoft, windows での python, postgresql を windows にインストール, mongodb を windows にインストール, python で postgresql を使用, python で mongodb を使用, WSL での postgresql, WSL での mongodb
ms.localizationpriority: medium
ms.date: 07/19/2019
ms.openlocfilehash: 9b1bdea86739f3d58b39cf7f0e6b8090474886f3
ms.sourcegitcommit: 60d2d15dd0d365f82e4e90e4bc34b40cf5b4a247
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/17/2019
ms.locfileid: "72517779"
---
# <a name="get-started-using-postgresql-or-mongodb-with-python-on-windows"></a>Windows で PostgreSQL または MongoDB と Python を初めて使用する

このステップバイステップ ガイドは、Python アプリを初めてデータベースに接続するときに役立ちます。 焦点を当てることにした 2 つの一般的なオプションは、PostgreSQL と MongoDB です。

## <a name="differences-between-mongodb-and-postgresql"></a>MongoDB と PostgreSQL の違い

[!INCLUDE [Postgres vs Mongo](../includes/postgres-v-mongo.md)]

> [!NOTE]
> 使用しているフレームワークやツールと、特定のデータベース システムの統合の相性を検討することが必要な場合もあります。 [Django Web フレームワーク](./web-frameworks.md#hello-world-tutorial-for-django)は、PostgreSQL との統合の方が適していると考えられます ([Django のドキュメント](https://docs.djangoproject.com/en/2.2/ref/contrib/postgres/)および [psycopg2](https://github.com/psycopg/psycopg2) を参照)。 [Flask Web フレームワーク](./web-frameworks.md#hello-world-tutorial-for-flask)は、MongoDB との統合の方が適していると考えられます ([MongoEngine](https://github.com/MongoEngine/flask-mongoengine) および [PyMongo](https://github.com/dcrosta/flask-pymongo) を参照)。

## <a name="install-postgresql"></a>PostgreSQL のインストール

[!INCLUDE [Install and run PostgresQL](../includes/install-and-run-postgres.md)]

### <a name="vs-code-support-for-postgresql"></a>VS Code の PostgreSQL サポート

VS Code では、[PostgreSQL 拡張機能](https://marketplace.visualstudio.com/items?itemName=ms-ossdata.vscode-postgresql)を使用した PostgreSQL データベースの操作がサポートされており、VSCode 内から PostgreSQL データベースを作成し、データベースに接続し、管理し、クエリを実行することができます。

## <a name="install-mongodb"></a>MongoDB のインストール

[!INCLUDE [Install and run Mongo](../includes/install-and-run-mongo.md)]

### <a name="vs-code-support-for-mongodb"></a>VS Code の MongoDB サポート

VS Code では、[Azure CosmosDB 拡張機能](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb)を使用した MongoDB データベースの操作がサポートされており、VSCode 内から MongoDB データベースを作成し、データベースに接続し、管理し、クエリを実行することができます。

詳細については、次に関する VS Code のドキュメントを参照してください:[MongoDB の操作](https://code.visualstudio.com/docs/azure/mongodb)。

## <a name="set-up-profile-aliases"></a>プロファイルのエイリアスの設定

[!INCLUDE [Set up profile aliases](../includes/profile-aliases.md)]
