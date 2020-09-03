---
title: Windows での Python を使用した Web 開発
description: Windows で Python を使用して Web 開発を始める方法。Flask や Django などのフレームワークのセットアップを含みます。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: python, windows 10, microsoft, windows での python , wsl を使用した python web , linux 用 windows サブシステムを使用した python web アプリ, windows での python web 開発, windows での flask アプリ, windows での django アプリ, python web, windows での flask web 開発, windows での django web 開発, python を使用した windows web 開発, vs code python web 開発, リモート wsl 拡張機能, ubuntu, wsl, venv, pip, microsoft python 拡張機能, windows での python の実行, windows での python の使用, windows での python を使用した構築
ms.localizationpriority: medium
ms.date: 07/19/2019
ms.openlocfilehash: fedfb42e4c1604b3570c2b4db21b12926bea3762
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174566"
---
# <a name="get-started-using-python-for-web-development-on-windows"></a>Windows で Web 開発に Python を使用する

以下は、Linux 用 Windows サブシステム (WSL) を使用して、Windows で Python を使用した Web 開発を始めるためのステップバイステップ ガイドです。

## <a name="set-up-your-development-environment"></a>開発環境を設定する

Web アプリケーションを構築するときは、WSL に Python をインストールすることをお勧めします。 Python Web 開発のチュートリアルと手順の多くは Linux ユーザー向けに記述されており、Linux ベースのパッケージ ツールとインストール ツールを使用しています。 ほとんどの Web アプリも Linux に配置されるため、これによって、開発環境と運用環境の間で一貫性が確保されます。

Web 開発以外に Python を使用している場合は、Microsoft Store を使用して Windows 10 に Python を直接インストールすることをお勧めします。 WSL は、GUI デスクトップまたはアプリケーション (PyGame、Gnome、KDE など) をサポートしていません。 このような場合は、Windows に直接 Python をインストールして使用してください。 Python を初めて使用する場合は、[初心者向けの Windows での Python の使用](./beginners.md)に関する記事をご覧ください。 お使いのオペレーティング システムでの一般的なタスクの自動化に関心がある場合は、次のガイドを参照してください:[Windows で Python を使用してスクリプト作成と自動化を開始する](./scripting.md)。 一部の高度なシナリオでは、[python.org](https://www.python.org/downloads/windows/) から特定の Python リリースを直接ダウンロードすることを検討するか、Anaconda、Jython、PyPy、WinPython、IronPython などの[代替手段をインストール](https://www.python.org/download/alternatives)することを検討してください。これは、別の実装を選択する具体的な理由がある、より高度な Python プログラマの場合にのみお勧めします。

## <a name="install-windows-subsystem-for-linux"></a>Linux 用 Windows サブシステムをインストールする

WSL を使用すると、Windows と、Visual Studio Code、Outlook などの使い慣れたツールと直接統合された GNU/Linux コマンド ライン環境を実行できます。

WSL (または WSL 2) を有効にしてインストールするには、[WSL インストール ドキュメント](/windows/wsl/install-win10)の手順に従います。 これらの手順には、Linux ディストリビューション (Ubuntu など) の選択が含まれています。

WSL と Linux ディストリビューションをインストールしたら、Linux ディストリビューション (Windows の [スタート] メニューにあります) を開き、コマンド `lsb_release -dc` を使用してバージョンとコードネームを確認します。

最新のパッケージであることを確認するために、インストールした直後も含めて、Linux ディストリビューションを定期的に更新することをお勧めします。 Windows はこの更新を自動的に処理しません。 使用中のディストリビューションを更新するには、コマンド `sudo apt update && sudo apt upgrade` を使用します。  

> [!TIP]
> 複数のタブ (コマンド プロンプト、PowerShell、複数の Linux ディストリビューション間をすばやく切り替える) の有効化、カスタム キー バインド (タブを開くまたは閉じる、コピーと貼り付けを行うなどのためのショートカット キー) の作成、検索機能の使用、カスタム テーマ (配色、フォント スタイルとサイズ、背景画像/ぼかし/透明度) の設定を行うために、[新しい Windows ターミナルを Microsoft Store からインストールする](https://www.microsoft.com/store/apps/9n0dx20hk701)ことを検討してください。 [詳しくはこちらをご覧ください](/windows/terminal)。

## <a name="set-up-visual-studio-code"></a>Visual Studio Code を設定する

VS Code を使用して、[IntelliSense](https://code.visualstudio.com/docs/editor/intellisense)、[lint](https://code.visualstudio.com/docs/python/linting)、[デバッグ サポート](https://code.visualstudio.com/docs/python/debugging)、[コード スニペット](https://code.visualstudio.com/docs/editor/userdefinedsnippets)、[単体テスト](https://code.visualstudio.com/docs/python/unit-testing)を利用します。 VS Code は Linux 用 Windows サブシステムと密接に統合され、[組み込みのターミナル](https://code.visualstudio.com/docs/editor/integrated-terminal)を提供してコード エディターとコマンド ライン間のシームレスなワークフローを確立し、[Git によるバージョン管理](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support)をサポートすることに加えて、一般的な Git コマンド (追加、コミット、プッシュ、プル) が UI に直接組み込まれています。

1. [Windows 用の VS Code をダウンロードしてインストールします](https://code.visualstudio.com)。 VS Code は Linux でも使用できますが、Linux 用 Windows サブシステムは GUI アプリをサポートしていないため、Windows にインストールする必要があります。 心配しなくても、Remote - WSL 拡張機能を使用すれば、将来、お使いの Linux コマンド ラインやツールとの統合は引き続き可能です。

2. [Remote - WSL 拡張機能](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl)を VS Code にインストールします。 これにより、統合開発環境として WSL を使用できるようになり、互換性とパスが自動的に処理されます。 [詳しくはこちらをご覧ください](https://code.visualstudio.com/docs/remote/remote-overview)。

> [!IMPORTANT]
> VS Code が既にインストールされている場合、[Remote - WSL 拡張機能](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl)をインストールするためには、[1.35 May リリース](https://code.visualstudio.com/updates/v1_35)以降がインストールされていることを確認する必要があります。 オートコンプリート、デバッグ、lint などのサポートが失われるため、VS Code において、Remote - WSL 拡張機能なしで WSL を使用することはお勧めしません。豆知識: この WSL 拡張機能は $HOME/.vscode-server/extensions にインストールされます。

## <a name="create-a-new-project"></a>新しいプロジェクトを作る

ここでは、Linux (Ubuntu) ファイル システムに新しいプロジェクト ディレクトリを作成し、Linux のアプリとツール、および VS Code を使用してこのディレクトリで作業します。

1. VS Code を閉じ、 **[スタート]** メニュー (左下の Windows アイコン) から次のように入力して Ubuntu 18.04 (WSL コマンド ライン) を開きます:「Ubuntu 18.04」。

2. Ubuntu コマンド ラインで、プロジェクトを配置する場所に移動し、プロジェクト用のディレクトリを作成します: `mkdir HelloWorld`。

![Ubuntu ターミナル](../images/ubuntu-terminal.png)

> [!TIP]
> Linux 用 Windows サブシステム (WSL) を使用する際に覚えておく必要がある重要な点は、**2 つの異なるファイル システムをまたいで作業している**ことです。1 つは Windows ファイル システム、もう 1 つは Linux ファイル システム (WSL)、この例では Ubuntu です。 パッケージをインストールおよびファイルを保存する場所に注意する必要があります。 あるバージョンのツールまたはパッケージを Windows ファイル システムにインストールし、まったく別のバージョンを Linux ファイル システムにインストールすることが可能です。 Windows ファイル システムでツールを更新しても Linux ファイル システムのツールには影響がなく、逆も同様です。 WSL は、お使いのコンピューター上の固定ドライブを、お使いの Linux ディストリビューションの `/mnt/<drive>` フォルダー配下にマウントします。 たとえば、Windows の C: ドライブは `/mnt/c/` 配下にマウントされます。 Ubuntu ターミナルから Windows のファイルにアクセスし、それらのファイルに対して Linux のアプリやツールを使用することが可能であり、逆も同様です。 多くの Web ツールは元々 Linux 向けに開発され、Linux の運用環境に配置されるため、Python Web 開発では Linux のファイル システムで作業することをお勧めします。 そうすることで、(Windows でファイル名の大文字と小文字が区別されないといった) ファイル システムのセマンティクスが混在することも回避できます。 とはいえ、WSL は Linux と Windows の両ファイル システム間の行き来をサポートするようになったため、どちらのファイル システムでもファイルをホストできます。 [詳しくはこちらをご覧ください](https://devblogs.microsoft.com/commandline/do-not-change-linux-files-using-windows-apps-and-tools/)。

## <a name="install-python-pip-and-venv"></a>Python、pip、venv をインストールする

Ubuntu 18.04 LTS には Python 3.6 が既にインストールされていますが、他の Python インストールで一般的なモジュールの一部が含まれていません。 Python の標準パッケージ マネージャー **pip** と、軽量な仮想環境の作成および管理に使用される標準モジュールの **venv** を、追加でインストールする必要があります。  

1. Ubuntu ターミナルを開き、`python3 --version` と入力して、Python3 が既にインストールされていることを確認します。 Python のバージョン番号が返されます。 Python のバージョンを更新する必要がある場合、`sudo apt update && sudo apt upgrade` と入力して Ubuntu のバージョンを更新した後、`sudo apt upgrade python3` を使用して Python を更新します。

2. `sudo apt install python3-pip` と入力して **pip** をインストールします。 pip を使用すると、Python 標準ライブラリに含まれていない追加のパッケージをインストールして管理することができます。

3. `sudo apt install python3-venv` と入力して **venv** をインストールします。

## <a name="create-a-virtual-environment"></a>仮想環境を作成する

仮想環境の使用は、Python 開発プロジェクトで推奨されるベスト プラクティスです。 仮想環境を作成することにより、プロジェクトのツールを分離し、他のプロジェクト用のツールとの間でバージョン管理上の競合を避けることができます。 たとえば、Django 1.2 Web フレームワークが必要な古い Web プロジェクトを保守している間に、Django 2.2 を使用するエキサイティングな新しいプロジェクトが登場したとします。 仮想環境の外で Django をグローバルに更新する場合、後でバージョン管理上の問題が起きる可能性があります。 仮想環境を使用すれば、予期しないバージョン管理上の競合を避けられるだけでなく、管理者特権なしでパッケージをインストールおよび管理することができます。

1. ターミナルを開き、*HelloWorld* プロジェクト フォルダー内で、次のコマンドを使用して **.venv** という名前の仮想環境を作成します: `python3 -m venv .venv`。

2. 仮想環境をアクティブにするには、`source .venv/bin/activate` と入力します。 成功した場合、コマンド プロンプトの前に **(.venv)** と表示されます。 これで、コードを記述してパッケージをインストールするための、自己完結型の環境が準備できました。 仮想環境での作業が完了したら、次のコマンドを入力して非アクティブ化します: `deactivate`。

    ![仮想環境を作成する](../images/wsl-venv.png)

> [!TIP]
> プロジェクトを配置する予定のディレクトリ内に仮想環境を作成することをお勧めします。 プロジェクトごとに独自の独立したディレクトリを設ける必要があり、各プロジェクトが独自の仮想環境を持つことになるため、一意の名前付けは必要ありません。 ここでは、Python の規則に従うために **.venv** という名前を使用することを提案します。 プロジェクト ディレクトリにインストールした場合、(pipenv などの) 一部のツールも既定でこの名前になります。 **.env** は環境変数の定義ファイルと競合するため、使用しないでください。 ディレクトリが存在することが `ls` から常に通知される必要はないため、一般的には、ドットで始まらない名前は推奨されません。 .gitignore ファイルに **.venv** を追加することもお勧めします。 (参考までに、[GitHub の既定の Python 用 gitignore テンプレート](https://github.com/github/gitignore/blob/50e42aa1064d004a5c99eaa72a2d8054a0d8de55/Python.gitignore#L99-L106)はこちらです。)VS Code での仮想環境を使用した作業の詳細については、[VS Code での Python 環境の使用](https://code.visualstudio.com/docs/python/environments)に関するページを参照してください。

## <a name="open-a-wsl---remote-window"></a>WSL - Remote ウィンドウを開く

VS Code では、(以前にインストールした) Remote - WSL 拡張機能を使用して、Linux サブシステムをリモート サーバーとして扱います。 これにより、WSL を統合開発環境として使用できるようになります。 [詳しくはこちらをご覧ください](https://code.visualstudio.com/docs/remote/wsl)。

1. `code .` と入力して、Ubuntu ターミナルから VS Code でプロジェクト フォルダーを開きます ("." は、現在のフォルダーを開くよう VS Code に指示します)。

2. Windows Defender のセキュリティ警告が表示されたら、[アクセスを許可] を選択します。 VS Code が開くと、リモート接続ホストのインジケーターが左下隅に表示され、**WSL: Ubuntu-18.04** で編集作業中であることを知らせます。

    ![VS Code のリモート接続ホストのインジケーター](../images/wsl-remote-extension.png)

3. Ubuntu ターミナルを閉じます。 これより後は、VS Code に統合された WSL ターミナルを使用します。

4. **Ctrl + `** (バックティック文字を使用) キーを押すか **[表示]**  >  **[ターミナル]** を選択して、VS Code で WSL ターミナルを開きます。 Ubuntu ターミナルで作成したプロジェクト フォルダーのパスで bash (WSL) コマンド ラインが開きます。

    ![VS Code 内の WSL ターミナル](../images/vscode-bash-remote.png)

## <a name="install-the-microsoft-python-extension"></a>Microsoft Python 拡張機能をインストールする

お使いの Remote - WSL 用の VS Code 拡張機能がある場合、インストールする必要があります。 VS Code にローカルで既にインストールされている拡張機能は、自動的には使用可能になりません。 [詳しくはこちらをご覧ください](https://code.visualstudio.com/docs/remote/wsl#_managing-extensions)。

1. **Ctrl + Shift + X** を入力して VS Code 拡張機能ウィンドウを開きます (または、メニューを使用して **[表示]**  >  **[拡張機能]** に移動します)。

2. 上部の **[Marketplace で拡張機能を検索する]** ボックスに、次のように入力します: **Python**。

3. **Python (ms-python.python) by Microsoft** 拡張機能を探し、緑色の **[インストール]** ボタンを選択します。

4. 拡張機能のインストールが完了したら、青色の **[Reload Required]\(再読み込みが必要\)** ボタンを選択する必要があります。 VS Code が再読み込みされ、**WSL: UBUNTU-18.04 - Installed** セクションが VS Code の [拡張機能] ウィンドウに表示されて、Python 拡張機能がインストールされたことを示します。

## <a name="run-a-simple-python-program"></a>単純な Python プログラムを実行する

Python はインタープリター言語であり、さまざまな種類のインタープリター (Python2、Anaconda、PyPy など) をサポートしています。 VS Code では、プロジェクトに関連付けられているインタープリターが既定で使用されます。 変更する理由がある場合、VS Code ウィンドウの下部にある青いバーに現在表示されているインタープリターを選択するか、**コマンド パレット** (Ctrl + Shift + P) を開いて **Python: Select Interpreter** コマンドを入力します。 これにより、現在インストールされている Python インタープリターの一覧が表示されます。 [Python 環境の構成の詳細については、こちらを参照してください](https://code.visualstudio.com/docs/python/environments)。

テストとして単純な Python プログラムを作成して実行し、正しい Python インタープリターが選択されていることを確認してみましょう。

1. **Ctrl + Shift + E** キーを押すか、メニューから **[表示]**  >  **[エクスプローラー]** を選択して、VS Code のファイル エクスプローラー ウィンドウを開きます。

2. まだ開いていない場合、**Ctrl + Shift + `** キーを押して統合 WSL ターミナルを開き、**HelloWorld** python プロジェクト フォルダーが選択されていることを確認します。

3. `touch test.py` と入力して python ファイルを作成します。 エクスプローラー ウィンドウで、先に作成したファイルが、プロジェクト ディレクトリに既に存在する .venv および .vscode フォルダーの下に表示されるはずです。

4. エクスプローラー ウィンドウで、先に作成した **test.py** ファイルを選択して、VS Code で開きます。 これが Python ファイルであることはファイル名の .py によって VS Code に認識されるため、以前に読み込んだ Python 拡張機能によって、Python インタープリターが自動的に選択されて読み込まれ、VS Code ウィンドウの最下部に表示されます。

    ![VS Code で Python インタープリターを選択する](../images/interpreterselection.gif)

5. この Python コードを test.py ファイルに貼り付けて、ファイルを保存します (Ctrl + S)。 

    ```python
    print("Hello World")
    ```

6. 先に作成した "Hello World" Python プログラムを実行するには、VS Code のエクスプローラー ウィンドウで **test.py** ファイルを選択し、ファイルを右クリックしてオプションのメニューを表示します。 **[Run Python File in Terminal]\(Python ファイルをターミナルで実行\)** を選択します。 または、統合 WSL ターミナル ウィンドウで `python test.py` と入力して "Hello World" プログラムを実行します。 Python インタープリターにより、ターミナル ウィンドウに "Hello World" と出力されます。

お疲れさまでした。 Python プログラムを作成して実行するための準備がすべて整いました。 次に、最も一般的な 2 つの Python Web フレームワークである Flask と Django を使用して Hello World アプリを作成してみましょう。

## <a name="hello-world-tutorial-for-flask"></a>Flask 用の Hello World チュートリアル

[Flask](http://flask.pocoo.org/) は Python 用の Web アプリケーション フレームワークです。 この簡単なチュートリアルでは、VS Code と WSL を使用して小さな "Hello World" Flask アプリを作成します。

1. **[スタート]** メニュー (左下の Windows アイコン) から次のように入力して Ubuntu 18.04 (WSL コマンド ライン) を開きます:「Ubuntu 18.04」。

2. `mkdir HelloWorld-Flask` でプロジェクトのディレクトリを作成し、`cd HelloWorld-Flask` でこのディレクトリに移動します。

3. プロジェクトのツールをインストールするための仮想環境を作成します: `python3 -m venv .venv`

4. 次のコマンドを入力して、VS Code で **HelloWorld-Flask** プロジェクトを開きます: `code .`

5. VS Code 内で **Ctrl + Shift + `** キーを押して統合 WSL ターミナル (別名 Bash) を開きます (**HelloWorld-Flask** プロジェクト フォルダーが既に選択されているはずです)。 *ここからは、VS Code に統合された WSL ターミナルで作業を進めるので、Ubuntu コマンド ラインを閉じます。*

6. VS Code 内の Bash ターミナルを使用して、手順 3 で作成した仮想環境をアクティブ化します: `source .venv/bin/activate`。 成功した場合、コマンド プロンプトの前に (.venv) と表示されます。

7. `python3 -m pip install flask` と入力して、Flask を仮想環境にインストールします。 `python3 -m flask --version` と入力して、インストールされていることを確認します。

8. Python コード用の新しいファイルを作成します: `touch app.py`

9. VS Code のファイル エクスプローラーで **app.py** ファイルを開きます (`Ctrl+Shift+E` キーを押してから app.py ファイルを選択します)。 インタープリターを選択するための Python 拡張機能がアクティブになります。 既定で **Python 3.6.8 64-bit ('.venv': venv)** が選択されるはずです。 仮想環境も検出されていることに注意してください。

    ![アクティブ化された仮想環境](../images/virtual-environment.png)

10. **app.py** で、Flask をインポートして Flask オブジェクトのインスタンスを作成するためのコードを追加します。

    ```python
    from flask import Flask
    app = Flask(__name__)
    ```

11. さらに **app.py** で、コンテンツ (この場合は単純な文字列) を返す関数を追加します。 Flask の **app.route** デコレーターを使用して、URL ルート "/" をその関数にマップします。

    ```python
    @app.route("/")
    def home():
        return "Hello World! I'm using Flask."
    ```

    > [!TIP]
    > 同じ関数にマップする異なったルートの数に応じて、同じ関数で複数のデコレーターを (1 行につき 1 つ) 使用できます。

12. **app.py** ファイルを保存します (**Ctrl + S**)。

13. ターミナルで、次のコマンドを入力してアプリを実行します。

    ```python
    python3 -m flask run
    ```

    Flask 開発サーバーが実行されます。 開発サーバーは既定で **app.py** を探します。 Flask を実行すると、次のような出力が表示されるはずです。

    ```bash
    (env) user@USER:/mnt/c/Projects/HelloWorld$ python3 -m flask run
     * Environment: production
       WARNING: This is a development server. Do not use it in a production deployment.
       Use a production WSGI server instead.
     * Debug mode: off
     * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
    ```

14. 既定の Web ブラウザーを開き、ターミナル内の http://127.0.0.1:5000/ URL を **Ctrl キーを押しながらクリック**して、レンダリングされたページを表示します。 次のメッセージがブラウザーに表示されるはずです。

    ![Hello, Flask!](../images/hello-flask.png)

15. "/" のような URL にアクセスすると、HTTP 要求を示すメッセージがデバッグ ターミナルに表示されることを確認します。

    ```bash
    127.0.0.1 - - [19/Jun/2019 13:36:56] "GET / HTTP/1.1" 200 -
    ```

16. ターミナルで **Ctrl + C** を使用してアプリを停止します。

> [!TIP]
> **app.py** 以外のファイル名 (**program.py** など) を使用する場合、**FLASK_APP** という名前の環境変数を定義し、選択したファイルにその値を設定します。 Flask の開発サーバーは、既定のファイル **app.py** の代わりに **FLASK_APP** の値を使用します。 詳細については、[Flask のコマンド ライン インターフェイスに関するドキュメント](http://flask.pocoo.org/docs/1.0/cli/)を参照してください。

以上、Visual Studio Code と Linux 用 Windows サブシステムを使用して Flask Web アプリケーションを作成しました。 VS Code と Flask を使用した、さらに詳しいチュートリアルについては、[Visual Studio Code の Flask チュートリアル](https://code.visualstudio.com/docs/python/tutorial-flask)を参照してください。

## <a name="hello-world-tutorial-for-django"></a>Django 用の Hello World チュートリアル

[Django](https://www.djangoproject.com) は Python 用の Web アプリケーション フレームワークです。 この簡単なチュートリアルでは、VS Code と WSL を使用して小さな "Hello World" Django アプリを作成します。

1. **[スタート]** メニュー (左下の Windows アイコン) から次のように入力して Ubuntu 18.04 (WSL コマンド ライン) を開きます:「Ubuntu 18.04」。

2. `mkdir HelloWorld-Django` でプロジェクトのディレクトリを作成し、`cd HelloWorld-Django` でこのディレクトリに移動します。

3. プロジェクトのツールをインストールするための仮想環境を作成します: `python3 -m venv .venv`

4. 次のコマンドを入力して、VS Code で **HelloWorld-DJango** プロジェクトを開きます: `code .`

5. VS Code 内で **Ctrl + Shift + `** キーを押して統合 WSL ターミナル (別名 Bash) を開きます (**HelloWorld-Django** プロジェクト フォルダーが既に選択されているはずです)。 *ここからは、VS Code に統合された WSL ターミナルで作業を進めるので、Ubuntu コマンド ラインを閉じます。*

6. VS Code 内の Bash ターミナルを使用して、手順 3 で作成した仮想環境をアクティブ化します: `source .venv/bin/activate`。 成功した場合、コマンド プロンプトの前に (.venv) と表示されます。

7. `python3 -m pip install django` コマンドを使用して、仮想環境に Django をインストールします。 `python3 -m django --version` と入力して、インストールされていることを確認します。

8. 次に、下記のコマンドを実行して Django プロジェクトを作成します。

    ```bash
    django-admin startproject web_project .
    ```

    `startproject` コマンドは (最後の `.` の使用によって) 現在のフォルダーがプロジェクト フォルダーであると想定し、その中に以下を作成します。

    - `manage.py`:プロジェクト用の Django コマンド ライン管理ユーティリティ。 `python manage.py <command> [options]` を使用してプロジェクトの管理コマンドを実行します。

    - `web_project` という名前のサブフォルダー。次のファイルが含まれます。
        - `__init__.py`: このフォルダーが Python パッケージであることを Python に指示する空のファイル。
        - `wsgi.py`: プロジェクトを実行する WSGI 互換 Web サーバーのエントリ ポイント。 通常、このファイルは実稼働 Web サーバーへのフックを提供するので、そのままにしておきます。
        - `settings.py`: Web アプリの開発中に変更する、Django プロジェクトの設定が含まれています。
        - `urls.py`: Django プロジェクトの目次が含まれ、これも開発中に変更します。

9. Django プロジェクトを検証するには、コマンド `python3 manage.py runserver` を使用して Django の開発サーバーを起動します。 サーバーは既定のポート 8000 で実行され、ターミナル ウィンドウに次のような出力が表示されるはずです。

    ```output
    Performing system checks...

    System check identified no issues (0 silenced).

    June 20, 2019 - 22:57:59
    Django version 2.2.2, using settings 'web_project.settings'
    Starting development server at http://127.0.0.1:8000/
    Quit the server with CONTROL-C.
    ```

    サーバーを初めて実行すると、サーバーは既定の SQLite データベースをファイル `db.sqlite3` に作成します。開発用途が想定されていますが、小規模な Web アプリであれば運用環境で使用できます。 また、Django の組み込み Web サーバーはローカル開発用途*のみ*を想定しています。 ただし、Web ホストに配置する場合、Django はホストの Web サーバーを代わりに使用します。 Django プロジェクトの `wsgi.py` モジュールは、実稼働サーバーへのフックを処理します。

    既定の 8000 以外のポートを使用する場合、`python3 manage.py runserver 5000` のようにコマンド ラインでポート番号を指定します。

10. ターミナル出力ウィンドウ内の `http://127.0.0.1:8000/` URL を `Ctrl+click`すると、既定のブラウザーでそのアドレスが開きます。 Django が正しくインストールされていてプロジェクトが有効な場合、既定のページが表示されます。 VS Code のターミナル出力ウィンドウには、サーバー ログも表示されます。

11. 終了したら、ブラウザー ウィンドウを閉じ、VS Code で、ターミナル出力ウィンドウの指示のとおり `Ctrl+C` を使用してサーバーを停止します。

12. 次に、Django アプリを作成するために、プロジェクト フォルダー (`manage.py` がある場所) で管理ユーティリティの `startapp` コマンドを実行します。

    ```bash
    python3 manage.py startapp hello
    ```

    このコマンドは、多数のコード ファイルと 1 つのサブフォルダーを含む `hello` という名前のフォルダーを作成します。 これらのうち、よく使用するのは、Web アプリ内のページを定義する関数が含まれる `views.py` と、データ オブジェクトを定義するクラスが含まれる `models.py` です。 `migrations` フォルダーは、このチュートリアルで追って説明するように、データベースのバージョンを管理するために Django の管理ユーティリティによって使用されます。 `apps.py` (アプリ構成)、`admin.py` (管理インターフェイスの作成用)、`tests.py` (テスト用) などのファイルもありますが、ここでは説明しません。

13. 次のコードに一致するように `hello/views.py` を変更します。このコードは、アプリのホーム ページの単一ビューを作成します。

    ```python
    from django.http import HttpResponse

    def home(request):
        return HttpResponse("Hello, Django!")
    ```

14. 次の内容のファイル `hello/urls.py` を作成します。 `urls.py` ファイルでは、さまざまな URL を適切なビューにルーティングするためのパターンを指定します。 下記のコードには、アプリのルート URL (`""`) を、先に `hello/views.py` に追加した `views.home` 関数にマップする 1 つのルートが含まれています。

    ```python
    from django.urls import path
    from hello import views

    urlpatterns = [
        path("", views.home, name="home"),
    ]
    ```

15. `web_project` フォルダーには `urls.py` ファイルも含まれます。これは、URL ルーティングが実際に処理される場所です。 `web_project/urls.py` を開き、次のコードに一致するように変更します (必要であれば、指示コメントを残しても構いません)。 このコードは、`django.urls.include` を使用してアプリの `hello/urls.py` を取得します。これにより、アプリのルートがアプリ内に保持されます。 この分離は、プロジェクトに複数のアプリが含まれる場合に役立ちます。

    ```python
    from django.contrib import admin
    from django.urls import include, path

    urlpatterns = [
        path("", include("hello.urls")),
    ]
    ```

16. 変更したすべてのファイルを保存します。

17. VS Code ターミナルで、`python3 manage.py runserver` を使用して開発サーバーを実行し、ブラウザーで `http://127.0.0.1:8000/` を開いて、ページの "Hello, Django" の表示を確認します。

以上、VS Code と Linux 用 Windows サブシステムを使用して Django Web アプリケーションを作成しました。 VS Code と Django を使用した、さらに詳しいチュートリアルについては、[Visual Studio Code の Django チュートリアル](https://code.visualstudio.com/docs/python/tutorial-django)を参照してください。

## <a name="additional-resources"></a>その他の資料

- [VS Code の Python チュートリアル](https://code.visualstudio.com/docs/python/python-tutorial):Python 環境としての VS Code の入門チュートリアル。コードを編集、実行、デバッグする方法が主な内容です。
- [VS Code の Git サポート](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support):VS Code で基本的な Git バージョン管理を使用する方法について説明します。  
- [近日提供の WSL 2 での更新内容について](/windows/wsl/wsl2-index):この新しいバージョンでは、Linux ディストリビューションと Windows の対話方法が変わり、ファイル システムのパフォーマンスが向上し、システム コールの完全な互換性が追加されます。
- [Windows で複数の Linux ディストリビューションを使用する](/windows/wsl/wsl-config):Windows コンピューターで複数の異なる Linux ディストリビューションを管理する方法について説明します。