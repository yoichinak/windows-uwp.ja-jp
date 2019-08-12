---
title: Windows での Python を使用した Web 開発
description: Windows での web 開発用の Python の使用を開始する方法 (Flask や Django などのフレームワークのセットアップを含む)。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
ms.prod: windows
ms.technology: hub
keywords: python、windows 10、microsoft、python on windows、python web with wsl、windows subsystem for linux を使用した python web アプリ、windows 上の python web 開発、windows 上の flask アプリ、django app on windows、python web、flask web dev on windows、django web dev on windows、windows web dev with python、vs code python web dev、remote wsl extension、ubuntu、wsl、ベンダー、pip、microsoft python extension、windows での python の実行、windows での python の使用、windows での python のビルド
ms.localizationpriority: medium
ms.date: 07/19/2019
ms.openlocfilehash: eafe85ac7e954d1a76708b059a191c14526afff8
ms.sourcegitcommit: 210034519678ba1a59744bc3a0b613b000921537
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/24/2019
ms.locfileid: "68473692"
---
# <a name="get-started-using-python-for-web-development-on-windows"></a>Windows での web 開発用の Python の使用を開始する

Windows Subsystem for Linux (WSL) を使用して、Windows での web 開発用の Python の使用を開始するためのステップバイステップガイドを次に示します。

## <a name="set-up-your-development-environment"></a>開発環境の設定

Web アプリケーションを構築するときは、WSL に Python をインストールすることをお勧めします。 Python web 開発のチュートリアルと手順の多くは、Linux ユーザー向けに記述されており、Linux ベースのパッケージツールとインストールツールを使用します。 ほとんどの web アプリも Linux にデプロイされるため、開発環境と運用環境の間で一貫性が確保されます。

Web 開発以外に Python を使用している場合は、Microsoft Store を使用して、Windows 10 に Python を直接インストールすることをお勧めします。 WSL は、GUI デスクトップやアプリケーション (Pykinect、Gnome、KDE など) をサポートしていません。 このような場合は、Python を Windows に直接インストールして使用します。 Python を初めて使用する場合は、次のガイドを参照してください。[初心者向け Windows での Python の使用を開始しましょう](./python-for-education.md)。 お使いのオペレーティングシステムでの一般的なタスクの自動化に関心がある場合は、次のガイドを参照してください。[Windows での Python の使用を開始し、スクリプト作成と自動化を実現](./python-for-scripting.md)します。 一部の高度なシナリオでは、 [python.org](https://www.python.org/downloads/windows/)から特定の Python リリースを直接ダウンロードすることを検討するか、Anaconda、Jython、PyPy、Winpython、IronPython などの[代替](https://www.python.org/download/alternatives)のインストールを検討することをお勧めします。これは、別の実装を選択する具体的な理由を持つより高度な Python プログラマの場合にのみお勧めします。

## <a name="enable-windows-subsystem-for-linux"></a>Linux 用 Windows サブシステムを有効にする

WSL を使用すると、ほとんどのコマンドラインツール、ユーティリティ、アプリケーションを含む GNU/Linux 環境を Windows 上で直接実行できます。また、Windows ファイルシステムや Visual Studio Code などのお気に入りのツールと完全に統合されています。 WSL を有効にする前に、[最新バージョンの Windows 10](https://www.microsoft.com/software-download/windows10)がインストールされていることを確認してください。

コンピューターで WSL を有効にするには、次のことを行う必要があります。

1. [**スタート**] メニュー (左下の windows アイコン) に移動し、「windows の機能の有効化または無効化」と入力して、**コントロールパネル**へのリンクを選択し、[ **windows の機能**] ポップアップメニューを開きます。 一覧で "Windows Subsystem for Linux" を探し、チェックボックスをオンにして機能を有効にします。

2. メッセージが表示されたら、コンピューターを再起動します。

## <a name="install-a-linux-distribution"></a>Linux ディストリビューションをインストールする

WSL で実行できる Linux ディストリビューションがいくつかあります。 お気に入りは、Microsoft Store で見つけてインストールできます。 [Ubuntu 18.04 LTS](https://www.microsoft.com/store/productId/9N9TNGVNDL3Q)は、現在、人気、およびサポートされているため、開始することをお勧めします。

1. この[Ubuntu 18.04 LTS](https://www.microsoft.com/store/productId/9N9TNGVNDL3Q)リンクを開き、Microsoft Store を開いて、[**取得**] を選択します。 *(これはかなり大きなダウンロードであり、インストールに時間がかかる場合があります)。*

2. ダウンロードが完了したら、[Microsoft Store から**起動**] を選択するか、[**スタート**] メニューに「Ubuntu 18.04 LTS」と入力して起動します。

3. 最初にディストリビューションを実行するときに、アカウント名とパスワードの作成を求められます。 この後、既定では、このユーザーとして自動的にサインインされます。 任意のユーザー名とパスワードを選択できます。 Windows ユーザー名には影響しません。

現在使用している Linux ディストリビューションについては、「 `lsb_release -d`」を入力して確認できます。 Ubuntu ディストリビューションを更新するには、 `sudo apt update && sudo apt upgrade`を使用します。 最新のパッケージがあることを確認するために、定期的に更新することをお勧めします。 Windows は、この更新プログラムを自動的に処理しません。 Microsoft Store で利用可能なその他の Linux ディストリビューション、代替のインストール方法、トラブルシューティングのリンクについては、「windows [10 用 Windows Subsystem For Linux インストールガイド](https://docs.microsoft.com/windows/wsl/install-win10)」を参照してください。

## <a name="set-up-visual-studio-code"></a>Visual Studio Code の設定

VS Code を使用して、 [IntelliSense](https://code.visualstudio.com/docs/editor/intellisense)、インライン[処理、](https://code.visualstudio.com/docs/python/linting)[デバッグサポート](https://code.visualstudio.com/docs/python/debugging)、[コードスニペット](https://code.visualstudio.com/docs/editor/userdefinedsnippets)、[単体テスト](https://code.visualstudio.com/docs/python/unit-testing)を活用できます。 VS Code は、Windows Subsystem for Linux に適切に統合されており、[組み込みのターミナル](https://code.visualstudio.com/docs/editor/integrated-terminal)を使用して、コードエディターとコマンドラインの間でシームレスなワークフローを確立します。また、共通の git による[バージョン管理のために git](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support)をサポートしています。コマンド (追加、コミット、プッシュ、プル) が UI に組み込まれています。

1. [Windows 用の VS Code をダウンロードしてインストール](https://code.visualstudio.com)します。 VS Code は Linux でも使用できますが、Windows Subsystem for Linux は GUI アプリをサポートしていないため、Windows にインストールする必要があります。 心配しなくても、リモートの WSL 拡張機能を使用して Linux コマンドラインやツールと統合できます。

2. VS Code に[リモート WSL 拡張機能](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl)をインストールします。 これにより、統合開発環境として WSL を使用し、互換性とパスを処理することができます。 [詳しくはこちらをご覧ください](https://code.visualstudio.com/docs/remote/remote-overview)。

> [!IMPORTANT]
> 既に VS Code がインストールされている場合は、[リモート WSL 拡張機能](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl)をインストールするために、 [1.35 がリリース](https://code.visualstudio.com/updates/v1_35)以降であることを確認する必要があります。 リモート WSL 拡張機能を使用せずに VS Code で WSL を使用することはお勧めしません。オートコンプリート、デバッグ、インライン処理などのサポートが失われるためです。おもしろい事実:この WSL 拡張機能は $HOME/.vscode-server/extensions. にインストールされています

## <a name="create-a-new-project"></a>新しいプロジェクトを作成する

ここでは、linux (Ubuntu) ファイルシステム上に新しいプロジェクトディレクトリを作成し、VS Code を使用して Linux アプリとツールを操作します。

1. [**スタート**] メニュー (左下の Windows アイコン) に移動して、次のように入力して、VS Code を閉じて Ubuntu 18.04 (wsl コマンドライン) を開きます。"Ubuntu 18.04"。

2. Ubuntu コマンドラインで、プロジェクトを配置する場所に移動し、その`mkdir HelloWorld`ディレクトリを作成します。

![Ubuntu ターミナル](../../images/ubuntu-terminal.png)

> [!TIP]
> Windows Subsystem for Linux (WSL) を使用するときに重要なことは、次の**2 つの異なるファイルシステム間で作業**していることです。1) Windows ファイルシステム、2) Linux ファイルシステム (WSL)。この例では Ubuntu です。 パッケージをインストールしてファイルを保存する場所に注意を払う必要があります。 Windows ファイルシステムには、ツールまたはパッケージの1つのバージョンをインストールできます。 Linux ファイルシステムには、まったく異なるバージョンをインストールできます。 Windows ファイルシステムのツールを更新しても、Linux ファイルシステムのツールには影響しません。その逆も同様です。 Wsl は、コンピューターの固定ドライブを Linux ディストリビューションの<drive> /mnt/フォルダーの下にマウントします。 たとえば、Windows C: ドライブはにマウント`/mnt/c/`されています。 Ubuntu ターミナルから Windows ファイルにアクセスし、それらのファイルで Linux アプリとツールを使用できます。また、その逆も可能です。 Python web 開発用に Linux ファイルシステムで作業することをお勧めします。これは、ほとんどの web ツールが Linux 向けに作成され、Linux 運用環境にデプロイされているためです。 また、ファイルシステムのセマンティクスが混在しないようにします (ファイル名については、Windows のように大文字と小文字が区別されます)。 しかし、WSL では、Linux ファイルシステムと Windows ファイルシステムの間でのジャンプがサポートされるようになったので、どちらかでファイルをホストすることができます。 [詳しくはこちらをご覧ください](https://devblogs.microsoft.com/commandline/do-not-change-linux-files-using-windows-apps-and-tools/)。 また、 [WSL2 が Windows に近](https://devblogs.microsoft.com/commandline/wsl-2-is-now-available-in-windows-insiders/)日公開され、大幅な改善が加えられています。 これは[、Windows insider build 18917 で試す](https://docs.microsoft.com/windows/wsl/wsl2-install)ことができます。

## <a name="install-python-pip-and-venv"></a>Python、pip、およびベンダーをインストールする

Ubuntu 18.04 LTS には、Python 3.6 が既にインストールされていますが、他の Python のインストールによって取得されることが予想されるモジュールの一部が付属していません。 また、 **pip**、Python 用標準パッケージマネージャー、および軽量仮想環境の作成と管理に使用される標準モジュールである**ベンダー**をインストールする必要もあります。  

1. Ubuntu ターミナルを開き、「 `python3 --version`」と入力して、Python3 が既にインストールされていることを確認します。 これにより、Python のバージョン番号が返されます。 Python のバージョンを更新する必要がある場合は、まず次のよう`sudo apt update && sudo apt upgrade`に入力して Ubuntu バージョンを更新し、次にを使用`sudo apt upgrade python3`して python を更新します。

2. 次のよう`sudo apt install python3-pip`に入力して pip をインストールします。 Pip を使用すると、Python 標準ライブラリに含まれていない追加のパッケージをインストールして管理することができます。

3. 「`sudo apt install python3-venv`」と入力して、**ベンダー**をインストールします。

## <a name="create-a-virtual-environment"></a>仮想環境を作成する

Python 開発プロジェクトのベストプラクティスとして、仮想環境を使用することをお勧めします。 仮想環境を作成することにより、プロジェクトツールを分離し、他のプロジェクトのツールとのバージョン管理の競合を回避できます。 たとえば、Django 1.2 web フレームワークを必要とする古い web プロジェクトを管理している場合に、Django 2.2 を使用して新しいプロジェクトを作成することができます。 仮想環境の外部で Django をグローバルに更新する場合は、後でバージョン管理の問題が発生する可能性があります。 仮想環境では、誤ったバージョン管理の競合を防ぐだけでなく、管理者特権なしでパッケージをインストールして管理することができます。

1. ターミナルを開き、 *HelloWorld*プロジェクトフォルダー内で次のコマンドを使用して、 **...** `python3 -m venv .venv`という名前の仮想環境を作成します。

2. 仮想環境をアクティブ化するには`source .venv/bin/activate`、「」と入力します。 正常に機能した場合は、コマンドプロンプトの前に **(...)** が表示されます。 これで、コードを記述してパッケージをインストールするための、自己完結型の環境が準備できました。 仮想環境の使用が完了したら、次のコマンドを入力して非`deactivate`アクティブ化します。

    ![仮想環境を作成する](../../images/wsl-venv.png)

> [!TIP]
> プロジェクトを作成するディレクトリ内に仮想環境を作成することをお勧めします。 各プロジェクトは個別のディレクトリにする必要があるため、それぞれに独自の仮想環境があるため、一意の名前を付ける必要はありません。 ここでは、Python 規則に従うために、と**いう名前を**使用することを提案します。 プロジェクトディレクトリにをインストールすると、一部のツール (pipenv など) も既定でこの名前になります。 環境変数の定義ファイルと競合するため、 **env**は使用しません。 通常、ディレクトリが存在することを常に示す必要が`ls`ないため、ドットではない名前を使用することはお勧めしません。 また、お使いの gifile**に追加する**ことをお勧めします。 (ここでは、 [Python 用の GitHub の既定の .gitignore](https://github.com/github/gitignore/blob/50e42aa1064d004a5c99eaa72a2d8054a0d8de55/Python.gitignore#L99-L106) for リファレンスを示しています)。VS Code での仮想環境の操作の詳細については、「 [VS Code での Python 環境の使用](https://code.visualstudio.com/docs/python/environments)」を参照してください。

## <a name="open-a-wsl---remote-window"></a>WSL-リモートウィンドウを開く

VS Code は、前にインストールしたリモート WSL 拡張機能を使用して、Linux サブシステムをリモートサーバーとして扱います。 これにより、統合開発環境として WSL を使用できるようになります。 [詳しくはこちらをご覧ください](https://code.visualstudio.com/docs/remote/wsl)。 

1. 次のよう`code .`に入力して、Ubuntu ターミナルから VS Code でプロジェクトフォルダーを開きます ("." は、現在のフォルダーを開くように VS Code に指示します)。

2. Windows Defender からセキュリティの警告がポップアップ表示され、[アクセスを許可する] を選択します。 VS Code を開くと、左下隅にリモート接続ホストインジケーターが表示され、wsl で**編集中であることがわかります。Ubuntu-18.04**。

    ![VS Code リモート接続ホストインジケーター](../../images/wsl-remote-extension.png)

3. Ubuntu ターミナルを閉じます。 今後は、VS Code に統合された WSL 端末を使用します。

4. Ctrl キーを押し**ながら '** (バックティック文字を使用) を押すか、[**ターミナル**の**表示** > ] を選択して、VS Code で wsl ターミナルを開きます。 これにより、Ubuntu ターミナルで作成したプロジェクトフォルダーパスに対して開かれた bash (WSL) コマンドラインが開きます。

    ![WSL ターミナルを使用した VS Code](../../images/vscode-bash-remote.png)

## <a name="install-the-microsoft-python-extension"></a>Microsoft Python 拡張機能をインストールする

リモート WSL の VS Code 拡張機能をインストールする必要があります。 VS Code にローカルにインストールされている拡張機能は、自動的には使用できません。 [詳しくはこちらをご覧ください](https://code.visualstudio.com/docs/remote/wsl#_managing-extensions)。

1. **Ctrl + Shift + X キーを押し**て [VS Code 拡張機能] ウィンドウを開きます (または、メニューを使用して**ビュー** > の**拡張機能**に移動します)。

2. [ **Marketplace の検索拡張機能**] ボックスに、次のように入力します。**Python**。

3. Microsoft 拡張機能**によって python (ms python. python)** を検索し、緑色の [**インストール**] ボタンを選択します。

4. 拡張機能のインストールが完了したら、青色の [**再読み込みが必要**] ボタンを選択する必要があります。 これにより VS Code が再読み込み**され、wsl が表示されます。UBUNTU-18.04-Python**拡張機能がインストールされていることを示す [VS Code 拡張機能] ウィンドウにインストールされたセクション。

## <a name="run-a-simple-python-program"></a>単純な Python プログラムを実行する

Python は解釈された言語であり、さまざまな種類の解釈 (Python2、Anaconda、PyPy など) をサポートしています。 VS Code は、プロジェクトに関連付けられているインタープリターを既定値にする必要があります。 変更する理由がある場合は、VS Code ウィンドウの下部にある青いバーに現在表示されているインタープリターを選択するか、**コマンドパレット**(Ctrl + Shift + P) を開い**て、コマンド Python を入力します。[インタープリター**] を選択します。 これにより、現在インストールされている Python インタープリターの一覧が表示されます。 [詳細については、Python 環境の構成に関する](https://code.visualstudio.com/docs/python/environments)ページをご覧ください。

単純な Python プログラムを作成してテストとして実行し、正しい Python インタープリターが選択されていることを確認しましょう。

1. **Ctrl + Shift + E キーを押し**て VS Code ファイルエクスプローラーウィンドウを開きます (または、メニューを使用して**表示** > **エクスプローラー**に移動します)。

2. まだ開いていない場合は、 **Ctrl + Shift + ' キーを押し**て統合 wsl ターミナルを開き、 **HelloWorld** python プロジェクトフォルダーが選択されていることを確認します。

3. 次のよう`touch test.py`に入力して、python ファイルを作成します。 先ほど作成したファイルが [エクスプローラー] ウィンドウの [プロジェクトディレクトリに既に存在する] フォルダーの下に表示されます。

4. エクスプローラーウィンドウで作成した**test.py**ファイルを選択して、VS Code で開きます。 ファイル名の .py は、これが Python ファイルであること VS Code 通知するため、以前に読み込んだ Python の拡張機能は、VS Code ウィンドウの下部に表示される Python インタープリターを自動的に選択して読み込みます。

    ![VS Code で Python インタープリターを選択します](../../images/interpreterselection.gif)

5. この Python コードを test.py ファイルに貼り付け、ファイルを保存します (Ctrl + S): 

    ```python
    print("Hello World")
    ```

6. 先ほど作成した Python の "Hello World" プログラムを実行するには、[VS Code エクスプローラー] ウィンドウで**test.py**ファイルを選択し、ファイルを右クリックして、[オプション] メニューを表示します。 [**ターミナルで Python ファイルを実行**する] を選択します。 または、統合された wsl ターミナルウィンドウで`python test.py` 、次のように入力して、"Hello World" プログラムを実行します。 Python インタープリターは、ターミナルウィンドウに "Hello World" を出力します。

テストは成功です。 Python プログラムを作成して実行するための設定がすべて整いました。 次に、最も一般的な2つの Python web フレームワークを使用して、Hello World アプリを作成してみましょう。Flask と Django。

## <a name="hello-world-tutorial-for-flask"></a>Flask の Hello World チュートリアル

[Flask](http://flask.pocoo.org/)は Python 用の web アプリケーションフレームワークです。 この簡単なチュートリアルでは、VS Code と WSL を使用して小さな "Hello World" Flask アプリを作成します。

1. [**スタート**] メニュー (左下の Windows アイコン) に移動して、次のように入力して、Ubuntu 18.04 (wsl コマンドライン) を開きます。"Ubuntu 18.04"。

2. プロジェクト`mkdir HelloWorld-Flask`のディレクトリを作成します。次`cd HelloWorld-Flask`に、ディレクトリを入力します。

3. 仮想環境を作成して、プロジェクトツールをインストールします。`python3 -m venv .venv`

4. コマンドを入力して、VS Code で**HelloWorld-Flask**プロジェクトを開きます。`code .`

5. VS Code 内で、 **Ctrl + Shift + '** ( **HelloWorld-flask**プロジェクトフォルダーは既に選択されている必要があります) を入力して、統合された Wsl ターミナル (Bash) を開きます。 *前の VS Code と統合された WSL ターミナルで作業するため、Ubuntu コマンドラインを閉じます。*

6. 「」の手順で作成した仮想環境をアクティブ化し、Bash ターミナル`source .venv/bin/activate`を使用して VS Code: を #3 します。 正常に機能した場合は、コマンドプロンプトの前に (...) が表示されます。

7. 次のよう`python3 -m pip install flask`に入力して、仮想環境に flask をインストールします。 次`python3 -m flask --version`のように入力して、インストールされていることを確認します。

8. Python コード用の新しいファイルを作成します。`touch app.py`

9. VS Code のエクスプローラー (`Ctrl+Shift+E`) で app.py ファイルを開き、app.py ファイルを選択します。 これにより、インタープリターを選択するために Python 拡張機能がアクティブ化されます。 既定値は**Python 3.6.8 64-bit ('...)** です。 仮想環境も検出されていることに注意してください。

    ![アクティブ化された仮想環境](../../images/virtual-environment.png)

10. **App.py**で、flask をインポートするコードを追加し、flask オブジェクトのインスタンスを作成します。

    ```python
    from flask import Flask
    app = Flask(__name__)
    ```

11. また、 **app.py**では、コンテンツ (この場合は単純な文字列) を返す関数を追加します。 Flask のアプリを使用して、URL ルート "/" をその関数にマップします **。**

    ```python
    @app.route("/")
    def home():
        return "Hello World! I'm using Flask."
    ```

    > [!TIP]
    > 同じ関数に複数のデコレーターを使用することができます。同じ関数にマップするルートの数に応じて、1行につき1つの関数を使用できます。

12. **App.py**ファイルを保存します (**Ctrl + S**)。

13. ターミナルで、次のコマンドを入力してアプリを実行します。

    ```python
    python3 -m flask run
    ```

    Flask 開発サーバーが実行されます。 開発サーバーは、既定で**app.py**を検索します。 Flask を実行すると、次のような出力が表示されます。

    ```bash
    (env) user@USER:/mnt/c/Projects/HelloWorld$ python3 -m flask run
     * Environment: production
       WARNING: This is a development server. Do not use it in a production deployment.
       Use a production WSGI server instead.
     * Debug mode: off
     * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
    ```

14. 表示されたページで既定の web ブラウザーを開き、 **Ctrl キーを押しながら**ターミナルの http://127.0.0.1:5000/ URL をクリックします。 ブラウザーに次のメッセージが表示されます。

    ![こんにちは、Flask!](../../images/hello-flask.png)

15. "/" のような URL にアクセスすると、HTTP 要求を示すメッセージがデバッグターミナルに表示されることを確認します。

    ```bash
    127.0.0.1 - - [19/Jun/2019 13:36:56] "GET / HTTP/1.1" 200 -
    ```

16. ターミナルで**Ctrl + C キーを押し**てアプリを停止します。

> [!TIP]
> **Program.py**などの**app.py**とは異なるファイル名を使用する場合は、 **FLASK_APP**という名前の環境変数を定義し、その値を選択したファイルに設定します。 Flask の開発サーバーは、既定のファイル**app.py**ではなく、 **FLASK_APP**の値を使用します。 詳細については、 [Flask のコマンドラインインターフェイス](http://flask.pocoo.org/docs/1.0/cli/)に関するドキュメントを参照してください。

これで、Visual Studio Code と Windows Subsystem for Linux を使用して Flask web アプリケーションが作成されました。 VS Code と Flask を使用した詳細なチュートリアルについては、 [Visual Studio Code の Flask チュートリアル](https://code.visualstudio.com/docs/python/tutorial-flask)を参照してください。

## <a name="hello-world-tutorial-for-django"></a>Django のチュートリアルの Hello World

[Django](https://www.djangoproject.com)は、Python 用の web アプリケーションフレームワークです。 この簡単なチュートリアルでは、VS Code と WSL を使用して小さな "Hello World" Django アプリを作成します。

1. [**スタート**] メニュー (左下の Windows アイコン) に移動して、次のように入力して、Ubuntu 18.04 (wsl コマンドライン) を開きます。"Ubuntu 18.04"。

2. プロジェクト`mkdir HelloWorld-Django`のディレクトリを作成します。次`cd HelloWorld-Django`に、ディレクトリを入力します。

3. 仮想環境を作成して、プロジェクトツールをインストールします。`python3 -m venv .venv`

4. コマンドを入力して VS Code で**HelloWorld-DJango**プロジェクトを開きます。`code .`

5. VS Code 内で、 **Ctrl + Shift + '** ( **Django**プロジェクトフォルダーは既に選択されている必要があります) を入力して、統合された Wsl ターミナル (Bash) を開きます。 *前の VS Code と統合された WSL ターミナルで作業するため、Ubuntu コマンドラインを閉じます。*

6. 「」の手順で作成した仮想環境をアクティブ化し、Bash ターミナル`source .venv/bin/activate`を使用して VS Code: を #3 します。 正常に機能した場合は、コマンドプロンプトの前に (...) が表示されます。

7. コマンドを使用して、仮想環境に Django `python3 -m pip install django`をインストールします。 次`python3 -m django --version`のように入力して、インストールされていることを確認します。

8. 次に、次のコマンドを実行して、Django プロジェクトを作成します。

    ```bash
    django-admin startproject web_project .
    ```

    この`startproject`コマンドは、現在の`.`フォルダーがプロジェクトフォルダーであることを前提としており、その中に次のものを作成します。

    - `manage.py` :プロジェクトの Django コマンドライン管理ユーティリティ。 を使用して`python manage.py <command> [options]`、プロジェクトの管理コマンドを実行します。

    - という名前`web_project`のサブフォルダー。次のファイルが含まれています。
        - `__init__.py`: Python にこのフォルダーが Python パッケージであることを示す空のファイル。
        - `wsgi.py`: WSGI と互換性のある web サーバーをプロジェクトに提供するためのエントリポイント。 通常、このファイルは実稼働 web サーバーのフックを提供するので、そのままにしておきます。
        - `settings.py`: Django プロジェクトの設定が含まれています。これは、web アプリを開発する過程で変更します。
        - `urls.py`: Django プロジェクトの目次が含まれています。このテーブルは、開発の過程でも変更できます。

9. Django プロジェクトを確認するには、コマンド`python3 manage.py runserver`を使用して Django の開発サーバーを起動します。 サーバーは既定のポート8000で実行され、ターミナルウィンドウに次の出力のような出力が表示されます。

    ```output
    Performing system checks...

    System check identified no issues (0 silenced).

    June 20, 2019 - 22:57:59
    Django version 2.2.2, using settings 'web_project.settings'
    Starting development server at http://127.0.0.1:8000/
    Quit the server with CONTROL-C.
    ```

    サーバーを初めて実行すると、ファイル`db.sqlite3`に既定の SQLite データベースが作成されます。このデータベースは開発目的で使用されますが、低ボリュームの web アプリの運用環境で使用できます。 また、Django の組み込み web サーバーは、ローカルでの開発*のみ*を目的としています。 ただし、web ホストに配置する場合、Django では、ホストの web サーバーが代わりに使用されます。 Django プロジェクトのモジュールでは、運用サーバーへのフックが処理されます。 `wsgi.py`

    既定の8000と`python3 manage.py runserver 5000`は異なるポートを使用する場合は、コマンドラインでなどのポート番号を指定します。

10. `Ctrl+click`既定のブラウザーでそのアドレスを開くためのターミナル出力ウィンドウ内のURL。`http://127.0.0.1:8000/` Django が正しくインストールされ、プロジェクトが有効な場合は、既定のページが表示されます。 VS Code ターミナル出力ウィンドウには、サーバーログも表示されます。

11. 完了したら、ブラウザーウィンドウを閉じ、ターミナル出力ウィンドウに示されて`Ctrl+C`いるようにを使用して VS Code でサーバーを停止します。

12. 次に、Django アプリを作成するために、プロジェクトフォルダー `startapp` (の場所`manage.py` ) で管理ユーティリティのコマンドを実行します。

    ```bash
    python3 manage.py startapp hello
    ```

    このコマンドは、多数の`hello`コードファイルと1つのサブフォルダーを含むという名前のフォルダーを作成します。 これらの中で、(web `views.py`アプリ内のページを定義する関数を含む) と`models.py` (データオブジェクトを定義するクラスを含む) を頻繁に使用します。 この`migrations`フォルダーは、このチュートリアルで後ほど説明するように、Django の管理ユーティリティでデータベースのバージョンを管理するために使用されます。 また、ファイル`apps.py` (アプリ構成`admin.py` )、(管理インターフェイスを作成するための)、および`tests.py` (テスト用) もあります。これらについては、ここでは説明しません。

13. 次`hello/views.py`のコードと一致するようにを変更します。これにより、アプリのホームページのビューが1つ作成されます。

    ```python
    from django.http import HttpResponse

    def home(request):
        return HttpResponse("Hello, Django!")
    ```

14. 以下の内容を`hello/urls.py`含むファイルを作成します。 ファイル`urls.py`では、パターンを指定して、異なる url を適切なビューにルーティングします。 次のコードには、アプリのルート URL (`""`) を追加`hello/views.py`した`views.home`関数にマップするルートが1つ含まれています。

    ```python
    from django.urls import path
    from hello import views

    urlpatterns = [
        path("", views.home, name="home"),
    ]
    ```

15. フォルダー `web_project`には、URL `urls.py`ルーティングが実際に処理されるファイルも含まれます。 を`web_project/urls.py`開き、次のコードに一致するように変更します (必要に応じて、指示に従ってコメントを保持できます)。 このコードは`hello/urls.py`を使用して`django.urls.include`アプリのを取得します。これにより、アプリ内に含まれるルートが保持されます。 この分離は、プロジェクトに複数のアプリが含まれている場合に便利です。

    ```python
    from django.contrib import admin
    from django.urls import include, path

    urlpatterns = [
        path("", include("hello.urls")),
    ]
    ```

16. 変更されたファイルをすべて保存します。

17. VS Code ターミナルで、を使用`python manage.py runserver`して開発サーバーを実行し、 `http://127.0.0.1:8000/`ブラウザーを開き、"Hello, Django" と表示されるページを表示します。

これで、VS Code と Windows Subsystem for Linux を使用して Django web アプリケーションが作成されました。 VS Code と Django を使用した詳細なチュートリアルについては、 [Visual Studio Code の「Django チュートリアル](https://code.visualstudio.com/docs/python/tutorial-django)」を参照してください。

## <a name="additional-resources"></a>その他の資料

- [VS Code を使用した Python チュートリアル](https://code.visualstudio.com/docs/python/python-tutorial):Python 環境として VS Code するための入門チュートリアルです。主にコードを編集、実行、デバッグする方法を説明します。
- [VS Code での Git のサポート](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support):VS Code で Git バージョン管理の基本を使用する方法について説明します。  
- [WSL 2 で近日公開予定の更新について説明](https://docs.microsoft.com/windows/wsl/wsl2-index)します。この新しいバージョンでは、Linux ディストリビューションが Windows とどのように連携し、ファイルシステムのパフォーマンスが向上し、システムコールの完全な互換性が追加されるかを変更します。
- [Windows での複数の Linux ディストリビューションの使用](https://docs.microsoft.com/windows/wsl/wsl-config):Windows コンピューターで複数の異なる Linux ディストリビューションを管理する方法について説明します。
