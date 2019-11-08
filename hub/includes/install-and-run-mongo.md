---
ms.topic: include
author: mattwojo
ms.author: mattwoj
ms.date: 10/04/2019
ms.openlocfilehash: f594600991f08a7dfda784ae127be2e6438dacbd
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/15/2019
ms.locfileid: "72314886"
---
MongoDB をインストールするには:

1. WSL ターミナル (ie を開きます。Ubuntu 18.04)。
2. Ubuntu パッケージを更新する: `sudo apt update`
3. パッケージが更新されたら、次のように MongoDB をインストールします。 `sudo apt-get install mongodb`
4. インストールの確認とバージョン番号の取得: `mongod --version`

MongoDB をインストールすると、次の3つのコマンドを認識する必要があります。

1. データベースの状態を確認するには、`sudo service mongodb status` を使用します。
2. `sudo service mongodb start` を使用すると、データベースの実行が開始されます。
3. `sudo service mongodb stop` の場合は、データベースの実行を停止します。

> [!NOTE]
> チュートリアルや記事で使用されているコマンド `sudo systemctl status mongodb` が表示されることがあります。 軽量のままにするため、WSL には `systemd` (Linux のサービス管理システム) は含まれません。 代わりに、SysVinit を使用してコンピューターでサービスを開始します。 違いはありませんが、チュートリアルで `sudo systemctl` を使用することを推奨する場合は、次のように使用します。 `sudo /etc/init.d/` を使用します。 たとえば、WSL の `sudo systemctl status mongodb` は `sudo /etc/inid.d/mongodb status`...または、`sudo service mongodb status` を使用することもできます。

### <a name="run-your-mongo-database-in-a-local-server"></a>ローカルサーバーで Mongo データベースを実行する

1. データベースの状態を確認します。`sudo service mongodb status` データベースを既に開始している場合を除き、[Fail] の応答が表示されます。

2. データベースを起動します。@no__t 0 の場合は、[OK] の応答が表示されます。

3. データベースサーバーに接続し、診断コマンドを実行して確認します。`mongo --eval 'db.runCommand({ connectionStatus: 1 })'` にすると、現在のデータベースのバージョン、サーバーのアドレスとポート、および status コマンドの出力が出力されます。 応答の "ok" フィールドの値 `1` は、サーバーが動作していることを示します。

4. MongoDB サービスの実行を停止するには、次のように入力します。 `sudo service mongodb stop`

> [!NOTE]
> MongoDB には、データを/data/db に格納し、ポート27017で実行するなど、いくつかの既定のパラメーターがあります。 また、`mongod` はデーモン (データベースのホストプロセス) で、`mongo` は `mongod` の特定のインスタンスに接続するコマンドラインシェルです。
