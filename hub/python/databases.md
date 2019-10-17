---
title: データベースを使用した Windows での Python の使用の開始
description: Windows 上の Python で PostgreSQL または MongoDB の使用を開始する際に役立つガイドです。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: python、windows 10、postgresql、mongodb、postgres、mongo、microsoft、windows での python、windows での postgresql のインストール、windows への postgresql のインストール、python での postgresql の使用、python での postgresql の使用、wsl 上の postgresql の使用
ms.localizationpriority: medium
ms.date: 07/19/2019
ms.openlocfilehash: 9b1bdea86739f3d58b39cf7f0e6b8090474886f3
ms.sourcegitcommit: 60d2d15dd0d365f82e4e90e4bc34b40cf5b4a247
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/17/2019
ms.locfileid: "72517779"
---
# <a name="get-started-using-postgresql-or-mongodb-with-python-on-windows"></a>Windows 上の Python で PostgreSQL または MongoDB の使用を開始する

このステップバイステップガイドでは、データベースへの Python アプリの接続を開始する方法について説明します。 PostgreSQL と MongoDB の2つの一般的なオプションに焦点を当てることを選択しました。

## <a name="differences-between-mongodb-and-postgresql"></a>MongoDB と PostgreSQL の違い

[!INCLUDE [Postgres vs Mongo](../includes/postgres-v-mongo.md)]

> [!NOTE]
> また、使用しているフレームワークとツールを特定のデータベースシステムと統合する方法を検討することもできます。 [Django web framework](./web-frameworks.md#hello-world-tutorial-for-django)は、PostgreSQL との統合が強化されているようです ( [Django docs](https://docs.djangoproject.com/en/2.2/ref/contrib/postgres/)と[psycopg2](https://github.com/psycopg/psycopg2)を参照してください)。 [Flask web フレームワーク](./web-frameworks.md#hello-world-tutorial-for-flask)は、MongoDB との統合が強化されているようです ( [MongoEngine](https://github.com/MongoEngine/flask-mongoengine)と[PyMongo](https://github.com/dcrosta/flask-pymongo)を参照してください)。

## <a name="install-postgresql"></a>PostgreSQL のインストール

[!INCLUDE [Install and run PostgresQL](../includes/install-and-run-postgres.md)]

### <a name="vs-code-support-for-postgresql"></a>PostgreSQL の VS Code サポート

Postgresql[拡張](https://marketplace.visualstudio.com/items?itemName=ms-ossdata.vscode-postgresql)機能を使用した postgresql データベースの操作をサポート VS Code は、VS Code 内から postgresql データベースを作成、接続、管理、照会することができます。

## <a name="install-mongodb"></a>MongoDB のインストール

[!INCLUDE [Install and run Mongo](../includes/install-and-run-mongo.md)]

### <a name="vs-code-support-for-mongodb"></a>MongoDB の VS Code サポート

VS Code は、 [Azure CosmosDB 拡張機能](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb)を使用した mongodb データベースの操作をサポートしており、VS Code 内から mongodb データベースを作成、接続、管理、照会することができます。

詳細については、「VS Code ドキュメント: [MongoDB の](https://code.visualstudio.com/docs/azure/mongodb)使用」を参照してください。

## <a name="set-up-profile-aliases"></a>プロファイルのエイリアスを設定する

[!INCLUDE [Set up profile aliases](../includes/profile-aliases.md)]
