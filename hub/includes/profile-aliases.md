---
ms.topic: include
author: mattwojo
ms.author: mattwoj
ms.date: 10/04/2019
ms.openlocfilehash: 2f7a57f1652ecab81a70c39faa1b70c42ed6a3de
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/29/2020
ms.locfileid: "72314906"
---
`sudo service mongodb start`、`sudo service postgres start`、`sudo -u postgrest psql` などの入力は面倒な場合があります。  これらのコマンドをすばやく使えるように、また覚えやすくするために、WSL 上の `.profile` ファイルでエイリアスを設定することを検討してください。 

これらのコマンドを実行するためのカスタム エイリアス (ショートカット) を設定するには、次の手順を実行します。

1. WSL ターミナルを開き、`cd ~` と入力してルート ディレクトリに移動します。
2. `.profile` と入力して、ターミナルの設定を制御する `sudo nano .profile` ファイルを、ターミナルのテキスト エディター Nano で開きます。
3. ファイルの最後に次の内容を追加します (`# set PATH` 設定は変更しないでください)。

    ```bash
    # My Aliases
    alias start-pg='sudo service postgresql start'
    alias run-pg='sudo -u postgres psql'
    ```

以後、`start-pg` と入力して postgresql サービスの実行を開始し、`run-pg` と入力して psql シェルを開くことができます。 `start-pg` および `run-pg` は任意の名前に変更できますが、postgres が既に使用しているコマンドを上書きしないよう注意してください。

4. 新しいエイリアスを追加したら、**Ctrl + X** キーを使用して Nano テキスト エディターを終了し、保存の確認に対して `Y` (はい) を選択して Enter キーを押します (ファイル名は `.profile` のままにします)。
5. WSL ターミナルを閉じて開き直してから、新しいエイリアス コマンドを試してみてください。
