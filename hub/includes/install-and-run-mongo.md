---
ms.topic: include
author: mattwojo
ms.author: mattwoj
ms.date: 10/04/2019
ms.openlocfilehash: f594600991f08a7dfda784ae127be2e6438dacbd
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/15/2019
ms.locfileid: "72314886"
---
MongoDB をインストールするには:

1. WSL ターミナルを開きます (Ubuntu 18.04)。
2. `sudo apt update` を実行して Ubuntu パッケージを更新します。
3. パッケージが更新されたら、`sudo apt-get install mongodb` を実行して MongoDB をインストールします。
4. `mongod --version` を実行してインストールを確認し、バージョン番号を取得します。

MongoDB をインストールした後は、次の 3 つのコマンドについて覚えておく必要があります。

1. データベースの状態を確認する `sudo service mongodb status`
2. データベースの実行を開始する `sudo service mongodb start`
3. データベースの実行を停止する `sudo service mongodb stop`

> [!NOTE]
> チュートリアルや記事で、`sudo systemctl status mongodb` コマンドが使用されている場合があります。 軽量状態を維持するために、WSL には `systemd` (Linux のサービス管理システム) は含まれていません。 代わりに、SysVinit を使用してマシン上のサービスを開始します。 違いはほとんどありませんが、チュートリアルで `sudo systemctl` が使用されている場合は、代わりに `sudo /etc/init.d/` を使用します。 たとえば、`sudo /etc/inid.d/mongodb status` は WSL では `sudo systemctl status mongodb` になります。または、`sudo service mongodb status` を使用することもできます。

### <a name="run-your-mongo-database-in-a-local-server"></a>ローカル サーバーで Mongo データベースを実行する

1. データベースの状態を確認します。`sudo service mongodb status` を実行すると、データベースを既に開始している場合を除き、[Fail] 応答が表示されます。

2. データベースを起動します。`sudo service mongodb start` を実行すると、[OK] の応答が表示されるようになります。

3. データベース サーバーに接続し、診断コマンドを実行して確認します。`mongo --eval 'db.runCommand({ connectionStatus: 1 })'` を実行すると、現在のデータベースのバージョン、サーバーのアドレスとポート、および status コマンドの結果が出力されます。 応答の "ok" フィールドに `1` の値は、サーバーが動作していることを示しています。

4. MongoDB のサービスの実行を停止するには、`sudo service mongodb stop` と入力します。

> [!NOTE]
> MongoDB には、データを /data/db に格納する、ポート 27017 で実行するなど、いくつかの既定のパラメーターがあります。 また、`mongod` はデーモン (データベースのホスト プロセス) であり、`mongo` は `mongod` の特定のインスタンスに接続するコマンドライン シェルです。
