---
title: Windows での Python の使用 (初心者向け)
description: Windows で Python を使用することが初めての場合に役立つガイドです。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: python、windows 10、microsoft、python の学習、初心者向けの python、microsoft store を使用した python のインストール、vs code を使用した python、pykinect on windows
ms.localizationpriority: medium
ms.date: 07/19/2019
ms.openlocfilehash: d4c1cb6d65eb38a93e8bf9f0c34afd9e28f20129
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/15/2019
ms.locfileid: "72314926"
---
# <a name="get-started-using-python-on-windows-for-beginners"></a>Windows での Python の使用開始 (初心者向け)

以下は、Windows 10 を使用した Python の学習に関心を持つ初心者向けのステップバイステップガイドです。

## <a name="set-up-your-development-environment"></a>開発環境を設定する

Python を初めて使用する初心者にとっては[、Microsoft Store から python をインストール](https://www.microsoft.com/en-us/p/python-37/9nj46sx7x90p?activetab=pivot:overviewtab)することをお勧めします。 Microsoft Store を介してをインストールすると、基本的な Python3 インタープリターが使用されますが、自動更新を提供するだけでなく、現在のユーザーのパス設定 (管理者アクセス権の必要性を回避する) の設定が処理されます。 これは、コンピューター上のアクセス許可または管理アクセスを制限する教育環境または組織の一部である場合に特に役立ちます。

Windows で**web 開発**用に Python を使用している場合は、開発環境用に別の設定を行うことをお勧めします。 Windows に直接インストールするのではなく、Windows Subsystem for Linux を使用して Python をインストールし、使用することをお勧めします。 詳細については、以下を参照してください。[Windows での web 開発用の Python の使用を開始](./web-frameworks.md)します。 お使いのオペレーティングシステムでの一般的なタスクの自動化に関心がある場合は、次のガイドを参照してください。[Windows での Python の使用を開始し、スクリプト作成と自動化を実現](./scripting.md)します。 いくつかの高度なシナリオ (Python のインストールされたファイルにアクセスして変更する、バイナリのコピーを作成する、Python Dll を直接使用するなど) については、 [python.org](https://www.python.org/downloads/)から特定の Python リリースを直接ダウンロードすることを検討するか、インストールを検討することをお勧めします。Anaconda、Jython、PyPy、WinPython、IronPython などの[代替手段](https://www.python.org/download/alternatives)。これは、別の実装を選択する具体的な理由を持つより高度な Python プログラマの場合にのみお勧めします。

## <a name="install-python"></a>Python をインストールする

Microsoft Store を使用して Python をインストールするには:

1. **[スタート]** メニュー (左下の Windows アイコン) に移動し、「Microsoft Store」と入力し、リンクを選択してストアを開きます。

2. ストアが開いたら、右上のメニューから **[検索]** を選択し、「Python」と入力します。 [アプリ] の下の結果から "Python 3.7" を開きます。 **[取得]** を選択します。

3. Python がダウンロードとインストールのプロセスを完了したら、 **[スタート]** メニュー (左下の windows アイコン) を使用して windows PowerShell を開きます。 PowerShell が開いたら、`Python --version` を入力して、Python3 がコンピューターにインストールされていることを確認します。

4. Python の Microsoft Store インストールには、標準のパッケージマネージャーである**pip**が含まれています。 Pip を使用すると、Python 標準ライブラリに含まれていない追加のパッケージをインストールして管理することができます。 また、パッケージのインストールと管理に pip が使用可能であることを確認するには、`pip --version` を入力します。

## <a name="install-visual-studio-code"></a>Visual Studio Code のインストール

テキストエディター/統合開発環境 (IDE) として VS Code を使用すると、 [IntelliSense](https://code.visualstudio.com/docs/editor/intellisense) (コード補完機能) を利用して[リンティング](https://code.visualstudio.com/docs/python/linting) (コードのエラーを回避するのに役立ちます)、[デバッグサポート](https://code.visualstudio.com/docs/python/debugging)(エラーの検出に役立ちます) を利用できます。実行後のコード、[コードスニペット](https://code.visualstudio.com/docs/editor/userdefinedsnippets)(小さい再利用可能なコードブロック用のテンプレート)、[単体テスト](https://code.visualstudio.com/docs/python/unit-testing)(さまざまな種類の入力によるコードのインターフェイスのテスト) があります。

また VS Code には、Windows コマンドプロンプト、PowerShell、または任意のものを使用して Python コマンドラインを開くことができる[組み込みのターミナル](https://code.visualstudio.com/docs/editor/integrated-terminal)も含まれており、コードエディターとコマンドラインの間でシームレスなワークフローを確立できます。

1. VS Code をインストールするには、Windows 用の VS Code をダウンロードしてください: [https://code.visualstudio.com](https://code.visualstudio.com)。

2. Python は解釈された言語であり、Python コードを実行するには、どのインタープリターを使用するか VS Code に指示する必要があります。 別のものを選択する特別な理由がない限り、Python 3.7 を使用することをお勧めします。 **コマンドパレット**(Ctrl + Shift + P) を開いて python 3 インタープリターを選択し、コマンド **python の入力を開始します。[インタープリター @ no__t-0] を選択して検索し、コマンドを選択します。 下部のステータスバーで **[Python 環境の選択**] オプションを使用することもできます (選択したインタープリターが既に表示されている場合があります)。 コマンドを実行すると、仮想環境を含む、自動的に検出 VS Code れる使用可能なインタープリターの一覧が表示されます。 目的のインタープリターが表示されない場合は、「 [Python 環境の構成](https://code.visualstudio.com/docs/python/environments)」を参照してください。

    ![VS Code で Python インタープリターを選択します](../images/interpreterselection.gif)

3. VS Code でターミナルを開くには、[ **View** > **ターミナル**] を選択するか、またはショートカット**Ctrl + '** (バックティック文字を使用) を使用します。 既定のターミナルは PowerShell です。

4. VS Code ターミナル内で、次のコマンドを入力するだけで Python を開きます。 `python`

5. 次のように入力して、Python インタープリターを試してみてください: `print("Hello World")`。 Python からステートメント "Hello World" が返されます。

    ![VS Code の Python コマンドライン](../images/python-in-vscode.png)

## <a name="install-git-optional"></a>Git のインストール (省略可能)

Python コードで他のユーザーと共同作業を行う場合、またはオープンソースサイト (GitHub など) でプロジェクトをホストする場合、VS Code は[Git を使用したバージョン管理](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support)をサポートします。 VS Code の [ソース管理] タブでは、すべての変更が追跡され、UI には一般的な Git コマンド (add、commit、push、pull) が組み込まれています。 最初に Git をインストールして、ソース管理パネルを電源にする必要があります。

1. Git [-scm web サイト](https://git-scm.com/download/win)から Git for Windows をダウンロードしてインストールします。

2. インストールウィザードには、Git インストールの設定に関する一連の質問が記載されています。 何らかの変更に特に理由がない限り、すべての既定の設定を使用することをお勧めします。

3. 前に Git を使用したことがない場合は、 [GitHub のガイド](https://guides.github.com/)を参考にしてください。

## <a name="hello-world-tutorial-for-some-python-basics"></a>いくつかの Python の基本に関する Hello World チュートリアル

Python は、その creator Guido van Rossum に従って、"高レベルなプログラミング言語" であり、コードの読みやすさや構文を使用することで、プログラマは数行のコードで概念を表現できるようになります。

Python は解釈された言語です。 コンピューターのプロセッサで実行するために記述するコードをコンピューターコードに変換する必要があるコンパイル済みの言語とは異なり、Python コードはインタープリターに直接渡され、直接実行されます。 コードを入力して実行するだけです。 試してみましょう。

1. PowerShell コマンドラインを開いた状態で、`python` を入力して Python 3 インタープリターを実行します。 (一部の手順では `py` または `python3` のコマンドを使用しますが、これらも機能します)。 > > は、3つ以上の記号が表示され > プロンプトが表示されるので、成功したことがわかります。

2. Python で文字列を変更できる組み込みメソッドがいくつかあります。 次のようにして変数を作成します。 `variable = 'Hello World!'` を使用します。 新しい行に対して enter キーを押します。

3. 次の値を使用して変数を出力します: `print(variable)`。 "Hello World!" というテキストが表示されます。

4. 文字列変数の長さと使用されている文字数を、`len(variable)` で確認します。 これにより、12文字が使用されていることが表示されます。 (合計長の文字としてカウントされた空白の空白文字であることに注意してください)。

5. 文字列変数を大文字に変換します: `variable.upper()`。 ここで、文字列変数を小文字に変換します。 `variable.lower()` です。

6. 文字列変数で文字 "l" が使用されている回数をカウントします: `variable.count("l")`。

7. 文字列変数内の特定の文字を検索し、次のように使用して感嘆符を見つけてみましょう。 `variable.find("!")`。 これにより、文字列の11番目の位置文字に感嘆符があることが表示されます。

8. 感嘆符を疑問符 (`variable.replace("!", "?")`) に置き換えます。

9. Python を終了するには、`exit()`、`quit()` を入力するか、Ctrl + Z キーを押します。

![このチュートリアルの PowerShell スクリーンショット](../images/hello-world-basics.png)

Python に組み込まれている文字列の変更方法を使用して楽しいことを願っています。 次に、Python プログラムファイルを作成し、VS Code で実行します。

## <a name="hello-world-tutorial-for-using-python-with-vs-code"></a>VS Code で Python を使用するための Hello World チュートリアル

VS Code はじめにチームは、python のチュートリアル[で](https://code.visualstudio.com/docs/python/python-tutorial#_start-vs-code-in-a-project-workspace-folder)、Hello World プログラムを作成する方法、プログラムファイルを実行する方法、デバッガーを構成して実行する方法、 *matplotlib* *のようなパッケージをインストールする方法について説明します。numpy*は、仮想環境内にグラフィカルプロットを作成します。

1. PowerShell を開き、"hello" という名前の空のフォルダーを作成し、このフォルダーに移動して VS Code で開きます。

    ```console
    mkdir hello
    cd hello
    code .
    ```

2. VS Code が開いたら、左側の **[エクスプローラー]** ウィンドウに新しい*hello*フォルダーを表示し、 **Ctrl + '** (バックティック文字を使用) または **[表示]** を選択して VS Code の下部パネルにあるコマンドラインウィンドウを開き  > **を押します。ターミナル**。 フォルダー内の VS Code を開始すると、そのフォルダーが "ワークスペース" になります。 VS Code は、そのワークスペースに固有の設定を vscode/settings. json で格納します。この設定は、グローバルに保存されているユーザー設定とは別のものです。

3. VS Code のドキュメントのチュートリアルを続行します。[Python Hello World ソースコードファイルを作成](https://code.visualstudio.com/docs/python/python-tutorial#_create-a-python-hello-world-source-code-file)します。

## <a name="create-a-simple-game-with-pygame"></a>Pygame を使用してシンプルなゲームを作成する

![Pykinect サンプルゲームの実行](../images/pygame-shmup.jpg)

Pykinect は、ゲームの奨励者を作成するための一般的な Python パッケージであり、楽しいものを作成しながら、プログラミングについて学習できます。 Pykinect は新しいウィンドウにグラフィックスを表示するため、WSL のコマンドラインのみのアプローチでは動作しません。 ただし、このチュートリアルで説明されているように Microsoft Store を使用して Python をインストールした場合は、問題なく動作します。

1. Python をインストールしたら、コマンドライン (または VS Code 内のターミナル) から、「`python -m pip install -U pygame --user`」と入力して、pykinect をインストールします。

2. サンプルゲームを実行してインストールをテストする: `python -m pygame.examples.aliens`

3. そうですね、ゲームによってウィンドウが開きます。 再生が終了したら、ウィンドウを閉じます。

独自のゲームの作成を開始する方法を次に示します。

1. PowerShell (または Windows コマンドプロンプト) を開き、"バウンス" という名前の空のフォルダーを作成します。 このフォルダーに移動し、"bounce.py" という名前のファイルを作成します。 VS Code でフォルダーを開きます。

    ```powershell
    mkdir bounce
    cd bounce
    new-item bounce.py
    code .
    ```

2. VS Code を使用して、次の Python コードを入力します (またはコピーして貼り付けます)。

    ```python
    import sys, pygame

    pygame.init()

    size = width, height = 640, 480
    dx = 1
    dy = 1
    x= 163
    y = 120
    black = (0,0,0)
    white = (255,255,255)

    screen = pygame.display.set_mode(size)

    while 1:

        for event in pygame.event.get():
            if event.type == pygame.QUIT: sys.exit()

        x += dx
        y += dy

        if x < 0 or x > width:   
            dx = -dx

        if y < 0 or y > height:
            dy = -dy

        screen.fill(black)

        pygame.draw.circle(screen, white, (x,y), 8)

        pygame.display.flip()
    ```

3. @No__t-0 として保存します。

4. PowerShell ターミナルから、次のように入力して実行します。 `python bounce.py`

    ![次の大きな pykinect を実行しています](../images/pygame.jpg)

数値の一部を調整して、バウンスボールにどのような効果があるかを確認してみてください。

Pykinect でのゲームの作成については、 [pygame.org](http://www.pygame.org)を参照してください。

## <a name="resources-for-continued-learning"></a>継続的な学習のためのリソース

Windows での Python 開発の詳細については、次のリソースを参照することをお勧めします。

### <a name="online-courses-for-learning-python"></a>Python を学習するためのオンラインコース

- [Microsoft Learn での Python の概要](https://docs.microsoft.com/en-us/learn/modules/intro-to-python/):対話型 Microsoft Learn プラットフォームを試して、このモジュールを完成させるためのエクスペリエンスポイントを獲得してください。基本的な Python コードを記述し、変数を宣言し、コンソールの入力と出力を操作する方法の基本について説明します。 対話型サンドボックス環境を使用すると、Python 開発環境がセットアップされていない人にとって、この作業を開始することができます。

- Pluralsight での @no__t 0Python:8コース、29時間 @ no__t:Pluralsight の Python learning パスでは、スキルを測定してギャップを見つけるためのツールなど、Python に関連するさまざまなトピックを網羅したオンラインコースを提供しています。

- [LearnPython.org のチュートリアル](https://www.learnpython.org/):DataCamp のスタッフから、これらの無料の対話型 Python チュートリアルをインストールしたり設定したりしなくても、Python の学習を開始できます。

- [Python.org のチュートリアル](https://docs.python.org/3/tutorial/index.html):Python 言語とシステムの基本的な概念と機能について説明します。

- [Lynda.com での Python の学習](https://www.lynda.com/Python-tutorials/Learning-Python/661773-2.html):Python の基本的な概要です。

### <a name="working-with-python-in-vs-code"></a>VS Code での Python の使用

- [VS Code での Python の編集](https://code.visualstudio.com/docs/python/editing):Behvior をカスタマイズする方法など、Python の VS Code のオートコンプリートと IntelliSense のサポートを活用する方法については、こちらを参照してください。または、オフにしてください。

- [Python](https://code.visualstudio.com/docs/python/linting):Linting は、潜在的なエラーのコードを分析するプログラムを実行するプロセスです。 Python とその設定方法については、さまざまな形式のサポート VS Code について説明します。

- [Python のデバッグ](https://code.visualstudio.com/docs/python/debugging):デバッグは、コンピュータープログラムからエラーを識別して削除するプロセスです。 この記事では、VS Code を使用して Python のデバッグを初期化して構成する方法、ブレークポイントを設定して検証する方法、ローカルスクリプトをアタッチする方法、さまざまなアプリの種類またはリモートコンピューターでデバッグを実行する方法、およびいくつかの基本的なトラブルシューティングを行う方法について説明します。

- [Python の単体テスト](https://code.visualstudio.com/docs/python/unit-testing):単体テストの意味、チュートリアルの例、テストフレームワークの有効化、テストの作成と実行、テストのデバッグ、構成設定のテストなど、いくつかの背景について説明します。 
