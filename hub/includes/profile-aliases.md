---
ms.topic: include
author: mattwojo
ms.author: mattwoj
ms.date: 10/04/2019
ms.openlocfilehash: 2f7a57f1652ecab81a70c39faa1b70c42ed6a3de
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/15/2019
ms.locfileid: "72314906"
---
@No__t-0 または `sudo service postgres start` と `sudo -u postgrest psql` を入力すると、面倒になる可能性があります。  ただし、WSL 上の `.profile` ファイルにエイリアスを設定して、これらのコマンドを使用して覚えやすくすることを検討できます。 

次のコマンドを実行するための独自のカスタムエイリアスまたはショートカットを設定するには、次の手順を実行します。

1. WSL ターミナルを開き、`cd ~` を入力して、ルートディレクトリにいることを確認します。
2. ターミナルテキストエディタを使用してターミナルの設定を制御する `.profile` ファイルを開きます。 Nano: `sudo nano .profile`
3. ファイルの末尾に (@no__t 0 設定を変更しないでください)、次の内容を追加します。

    ```bash
    # My Aliases
    alias start-pg='sudo service postgresql start'
    alias run-pg='sudo -u postgres psql'
    ```

これにより、`start-pg` を入力して postgresql サービスの実行を開始し、`run-pg` を使用して psql シェルを開くことができます。 @No__t-0 および `run-pg` を任意の名前に変更できます。 postgres が既に使用しているコマンドを上書きしないように注意してください。

4. 新しいエイリアスを追加したら、 **Ctrl + X キーを押し**て [`Y` (はい)] をクリックし、保存して入力するように求められたら、[はい] を選択します。このとき、ファイル名は `.profile` のままにします。
5. WSL ターミナルを閉じて開き直してから、新しいエイリアスコマンドを試してください。
