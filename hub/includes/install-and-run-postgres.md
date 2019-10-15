---
ms.topic: include
author: mattwojo
ms.author: mattwoj
ms.date: 10/04/2019
ms.openlocfilehash: aa1da2a65f95d92e895533ed37426b5e454c255b
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/15/2019
ms.locfileid: "72314896"
---
PostgreSQL をインストールするには:

1. WSL ターミナル (ie を開きます。Ubuntu 18.04)。
2. Ubuntu パッケージを更新する: `sudo apt update`
3. パッケージが更新されたら、次のように PostgreSQL (およびいくつかの便利なユーティリティが含まれている-contrib パッケージ) をインストールし @no__t ます。
4. インストールの確認とバージョン番号の取得: `psql --version`

PostgreSQL をインストールすると、次の3つのコマンドを知る必要があります。

1. データベースの状態を確認するには、`sudo service postgresql status` を使用します。
2. `sudo service postgresql start` を使用すると、データベースの実行が開始されます。
3. `sudo service postgresql stop` の場合は、データベースの実行を停止します。

### <a name="postgresql-user-setup"></a>PostgreSQL ユーザーセットアップ

既定の管理者ユーザーである @no__t 0 では、データベースに接続するためにパスワードを割り当てる必要があります。 パスワードを設定するには:

1. 次のコマンドを入力します: `sudo passwd postgres`
2. 新しいパスワードを入力するように求めるメッセージが表示されます。
3. ターミナルを閉じてから開き直します。

### <a name="run-postgresql-with-psql-shell"></a>Psql シェルを使用した PostgreSQL の実行

[psql](https://www.postgresql.org/docs/10/app-psql.html)は、PostgreSQL に対するターミナルベースのフロントエンドです。 これにより、クエリを対話形式で入力し、PostgreSQL に発行し、クエリ結果を確認できます。 または、ファイルからの入力を使用することもできます。 さらに、スクリプトの記述やさまざまなタスクの自動化を容易にするために、多くのメタコマンドと、シェルに似たさまざまな機能が用意されています。

Psql シェルを開始するには、次のようにします。

1. Postgres サービスを開始します。 `sudo service postgresql start`
2. Postgres サービスに接続し、psql シェルを開きます。 `sudo -u postgres psql`

Psql シェルを正常に入力すると、次のようにコマンドラインが変更されていることがわかります。 `postgres=#`

> [!NOTE]
> または、次を使用して postgres ユーザーに切り替えて、psql シェルを開くこともできます。 `su - postgres` の場合は、コマンドを入力します。 `psql`

Postgres = # enter を終了するには、`\q` またはショートカットキーを使用します。Ctrl + D

PostgreSQL のインストールで作成されたユーザーアカウントを確認するには、WSL ターミナルからを使用します。 `psql -c "\du"`...または、psql シェルを開いている場合は、-1 を @no__t ます。 次のコマンドを実行すると、列が表示されます。アカウントのユーザー名、ロールの属性の一覧、およびロールグループのメンバー。 コマンドラインに戻るには、「`q`」と入力します。
