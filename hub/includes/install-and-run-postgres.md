---
ms.topic: include
author: mattwojo
ms.author: mattwoj
ms.date: 10/04/2019
ms.openlocfilehash: aa1da2a65f95d92e895533ed37426b5e454c255b
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/29/2020
ms.locfileid: "72314896"
---
PostgreSQL をインストールするには:

1. WSL ターミナルを開きます (Ubuntu 18.04)。
2. `sudo apt update` を実行して Ubuntu パッケージを更新します。
3. パッケージが更新されたら、`sudo apt install postgresql postgresql-contrib` を実行して PostgreSQL (およびいくつかの便利なユーティリティが含まれている -contrib パッケージ) をインストールします。
4. `psql --version` を実行してインストールを確認し、バージョン番号を取得します。

PostgreSQL をインストールした後は、次の 3 つのコマンドについて覚えておく必要があります。

1. データベースの状態を確認する `sudo service postgresql status`
2. データベースの実行を開始する `sudo service postgresql start`
3. データベースの実行を停止する `sudo service postgresql stop`

### <a name="postgresql-user-setup"></a>PostgreSQL ユーザー セットアップ

既定の管理者ユーザー `postgres` には、データベースに接続するパスワードを割り当てる必要があります。 パスワードを設定するには:

1. コマンド `sudo passwd postgres` を入力します。
2. 新しいパスワードを入力するように求めるメッセージが表示されます。
3. ターミナルを閉じ、開き直します。

### <a name="run-postgresql-with-psql-shell"></a>psql シェルを使用した PostgreSQL の実行

[psql](https://www.postgresql.org/docs/10/app-psql.html) は、PostgreSQL に対するターミナルベースのフロントエンドです。 これにより、クエリを対話形式で入力して PostgreSQL に発行し、クエリ結果を確認できます。 または、ファイルからの入力を使用することもできます。 さらに、スクリプトの記述やさまざまなタスクの自動化を容易にするために、多くのメタコマンドと、シェルに似たさまざまな機能が用意されています。

psql シェルを開始するには:

1. `sudo service postgresql start` を実行して postgres サービスを開始します。
2. `sudo -u postgres psql` を実行して postgres サービスに接続し、psql シェルを開きます。

正常に psql シェルに入ると、コマンドラインが `postgres=#` のように変わります。

> [!NOTE]
> または、`su - postgres` を使用して postgres ユーザーに切り替え、コマンド `psql` を入力することで psql シェルを開くことができます。

postgres を終了するには、`\q` を入力するか、次のショートカット キーを使用します。Ctrl + D

PostgreSQL のインストールで作成されたユーザー アカウントを確認するには、WSL ターミナルから `psql -c "\du"` を使用します。または、psql シェルが開いている場合は、`\du` を実行します。 このコマンドを実行すると、アカウント ユーザー名、ロールの属性の一覧、およびロール グループのメンバーの各列が表示されます。 `q` を入力すると、終了してコマンド ラインに戻ります。
