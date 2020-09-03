---
title: Windows で Python を使ってみる (初心者向け)
description: Windows で初めて Python を使用する場合に役立つスタート ガイドです。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: python, windows 10, microsoft, python の学習, windows での python (初心者向け), microsoft store から python をインストール, python と vs code, windows での pygame
ms.localizationpriority: medium
ms.date: 07/19/2019
ms.openlocfilehash: 076c62658431ba75bdbd7f385ced86f27fb2f7d0
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174146"
---
# <a name="get-started-using-python-on-windows-for-beginners"></a>Windows で Python を使ってみる (初心者向け)

以下は、Windows 10 を使用して Python を学ぶことに興味がある初心者向けのステップバイステップ ガイドです。

## <a name="set-up-your-development-environment"></a>開発環境を設定する

Python を初めて使用する初心者の方には、[Microsoft Store から Python をインストールする](https://www.microsoft.com/p/python-37/9nj46sx7x90p?activetab=pivot:overviewtab)ことをお勧めします。 Microsoft Store を介してインストールすると、基本的な Python3 インタープリターが使用されますが、自動更新が提供されるだけでなく、現在のユーザーの PATH 設定 (管理者アクセス権は不要) も行われます。 これは、教育現場や、コンピューターへのアクセス許可または管理アクセスに制限を設けている組織で特に役立ちます。

Windows で **Web 開発**に Python を使用している場合は、開発環境用に別のセットアップを用意することをお勧めします。 Windows に直接インストールするのではなく、Linux 用 Windows サブシステム経由で Python をインストールして使用することをお勧めします。 ヘルプについては、次を参照してください:「[Windows での Web 開発用の Python の使用を開始する](./web-frameworks.md)」をご覧ください。 お使いのオペレーティング システムでの一般的なタスクの自動化に関心がある場合は、次のガイドを参照してください:[Windows で Python を使用してスクリプト作成と自動化を開始する](./scripting.md)。 一部の高度なシナリオ (Python のインストール ファイルへのアクセスや修正が必要な場合、バイナリのコピーを作成する場合、Python DLL を直接使用する場合など) では、[python.org](https://www.python.org/downloads/) から特定の Python リリースを直接ダウンロードすることを検討するか、Anaconda、Jython、PyPy、WinPython、IronPython などの[代替手段をインストール](https://www.python.org/download/alternatives)することを検討してください。これは、別の実装を選択する具体的な理由がある、より高度な Python プログラマの場合にのみお勧めします。

## <a name="install-python"></a>Python のインストール

Microsoft Store を使用して Python をインストールするには:

1. **スタート** メニュー (左下の Windows アイコン) に移動し、「Microsoft Store」と入力してリンクを選択し、ストアを開きます。

2. ストアが開いたら、右上のメニューから **[検索]** を選択し、「Python」と入力します。 [アプリ] の下の結果から "Python 3.7" を開きます。 **[取得]** を選択します。

3. Python がダウンロードとインストールのプロセスを完了したら、**スタート** メニュー (左下の Windows アイコン) を使用して Windows PowerShell を開きます。 PowerShell が開いたら、`Python --version` を入力して、マシンに Python3 がインストールされていることを確認します。

4. Python の Microsoft Store インストールには、標準のパッケージ マネージャーである **pip** が含まれています。 pip を使用すると、Python 標準ライブラリに含まれていない追加のパッケージをインストールして管理することができます。 また、パッケージのインストールと管理に pip が使用できることを確認するには、`pip --version` と入力します。

## <a name="install-visual-studio-code"></a>Visual Studio Code をインストールする

テキスト エディターおよび統合開発環境 (IDE) として VS Code を使用すると、[IntelliSense](https://code.visualstudio.com/docs/editor/intellisense) (コード補完支援)、[Linting](https://code.visualstudio.com/docs/python/linting) (コードにエラーが発生しないようにする)、[デバッグ サポート](https://code.visualstudio.com/docs/python/debugging) (実行後にコードのエラーを検出)、[コード スニペット](https://code.visualstudio.com/docs/editor/userdefinedsnippets) (小さい再利用可能なコード ブロック用のテンプレート)、[単体テスト](https://code.visualstudio.com/docs/python/unit-testing) (さまざまな種類の入力を使用したコードのインターフェイスのテスト) を活用できます。

また、VS Code に含まれる[組み込みターミナル](https://code.visualstudio.com/docs/editor/integrated-terminal)では、Windows コマンド プロンプト、PowerShell、その他の環境を使用して Python コマンド ラインを開き、コード エディターとコマンド ラインの間でシームレスなワークフローを確立することができます。

1. VS Code をインストールするには、Windows 用の VS Code をダウンロードします: [https://code.visualstudio.com](https://code.visualstudio.com)。

2. VS Code がインストールされたら、Python 拡張機能もインストールする必要があります。 Python 拡張機能をインストールするには、[VS Code マーケットプレースへのリンク](https://marketplace.visualstudio.com/items?itemName=ms-python.python)を選択するか、VS Code を開いて拡張機能メニュー (Ctrl + Shift + X) で「**Python**」を検索します。

3. Python はインタープリター言語であり、Python コードを実行するには、使用するインタープリターを VS Code に指示する必要があります。 別のものを選択する特別な理由がない限り、Python 3.7 を使用することをお勧めします。 Python 拡張機能をインストールしたら、Python 3 インタープリターを選択します。**コマンド パレット**を開き (Ctrl + Shift + P)、検索するコマンド「**Python: Select Interpreter**」の入力を開始して、コマンドを選択します。 利用可能な場合、最下部のステータス バーにある **[Select Python Environment]\(Python 環境の選択\)** オプションを使用することもできます (選択したインタープリターが既に表示されている場合があります)。 コマンドを実行すると、仮想環境を含め、VS Code が自動的に検出できる使用可能なインタープリターの一覧が表示されます。 目的のインタープリターが表示されない場合は、[Python 環境の構成](https://code.visualstudio.com/docs/python/environments)に関するページを参照してください。

    ![VS Code で Python インタープリターを選択する](../images/interpreterselection.gif)

4. VS Code でターミナルを開くには、 **[表示]**  >  **[ターミナル]** を選択するか、ショートカット **Ctrl + `** を使用します (バックティック文字を使用)。 既定のターミナルは PowerShell です。

5. VS Code ターミナル内で、`python` コマンドを入力して Python を開きます。

6. `print("Hello World")` と入力して、Python インタープリターを試してみます。 Python から "Hello World" 文字列が返されます。

    ![VS Code 内の Python コマンド ライン](../images/python-in-vscode.png)

## <a name="install-git-optional"></a>Git のインストール (省略可能)

共同作業で Python コードを開発する場合や、(GitHub のような) オープンソース サイトでプロジェクトをホストする場合のために、VS Code では [Git によるバージョン管理](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support)がサポートされています。 VS Code の [ソース管理] タブでは、すべての変更が追跡され、一般的な Git コマンド (追加、コミット、プッシュ、プル) が UI に組み込まれています。 ソース管理パネルを使用するには、まず Git をインストールする必要があります。

1. Windows 用の Git を [git-scm Web サイト](https://git-scm.com/download/win)からダウンロードしてインストールします。

2. インストール ウィザードで、Git インストールの設定に関する一連の質問に答えます。 何かを変更する特別な理由がない限り、すべて既定の設定を使用することをお勧めします。

3. 以前に Git を使用したことがない場合、入門用の [GitHub ガイド](https://guides.github.com/)が役に立ちます。

## <a name="hello-world-tutorial-for-some-python-basics"></a>Python の基本を学ぶ Hello World チュートリアル

作者の Guido van Rossum 氏は Python について「高度なプログラミング言語であり、その中核をなす設計哲学は、コードの読みやすさと、プログラマが数行のコードで概念を表現できる構文を重視している」と述べています。

Python はインタープリター言語です。 コンピューターのプロセッサで実行するために、記述したコードをマシン コードに変換する必要があるコンパイラ言語とは異なり、Python のコードはインタープリターにそのまま渡され、直接実行されます。 コードを入力して実行するだけです。 試してみましょう。

1. PowerShell コマンド ラインを開いた状態で、`python` と入力して Python 3 インタープリターを実行します (`py` または `python3` コマンドの使用が優先される命令もあり、これらのコマンドでも機能します)。 3 つの大なり記号 (>>>) のプロンプトが表示されれば成功です。

2. Python には、文字列の変更に使用できる組み込みのメソッドが複数あります。 変数を作成するには、`variable = 'Hello World!'` のようにします。 Enter キーを押して改行します。

3. `print(variable)` で変数を出力します。 "Hello World!" というテキストが表示されます。

4. 文字列変数の長さ (使われている文字の数) を調べるには、`len(variable)` を使用します。 12 文字が使われていると表示されます。 (スペースも 1 文字としてカウントされ、全体の長さに含まれていることに注意してください。)

5. 文字列変数を大文字に変換するには、`variable.upper()` を使用します。 次に、`variable.lower()` を使用して、文字列変数を小文字に変換します。

6. 文字列変数の中で "l" が使われている回数を数えるには、`variable.count("l")` を使用します。

7. 文字列変数から特定の文字を探します。ここでは、`variable.find("!")` を使用して感嘆符を探します。 文字列の 11 文字目に感嘆符が見つかったと表示されます。

8. 感嘆符を疑問符に置き換えるには、`variable.replace("!", "?")` を使用します。

9. Python を終了するには、`exit()` または `quit()` と入力するか、Ctrl - Z キーを押します。

![このチュートリアルの PowerShell スクリーンショット](../images/hello-world-basics.png)

Python の組み込みの文字列変更メソッドを使用してみました。 次に、Python プログラム ファイルを作成し、VS Code で実行してみましょう。

## <a name="hello-world-tutorial-for-using-python-with-vs-code"></a>VS Code で Python を使用するための Hello World チュートリアル

VS Code チームが作成した[はじめての Python](https://code.visualstudio.com/docs/python/python-tutorial#_start-vs-code-in-a-project-workspace-folder) チュートリアルでは、Hello World プログラムを Python で作成し、プログラム ファイルを実行し、デバッガーを構成して実行し、*matplotlib* や *numpy* などのパッケージをインストールして仮想環境内でグラフィカル プロットを作成する方法を説明しています。

1. PowerShell を開き、"hello" という名前の空のフォルダーを作成し、このフォルダーに移動し、VS Code でこのフォルダーを開きます。

    ```console
    mkdir hello
    cd hello
    code .
    ```

2. VS Code が起動し、左側の **[エクスプローラー]** ウィンドウに新しい *hello* フォルダーが表示されたら、**Ctrl + `** (バックティック文字を使用) キーを押すか **[表示]**  >  **[ターミナル]** を選択して、VS Code の下部パネルにコマンド ライン ウィンドウを開きます。 フォルダー内で VS Code を起動すると、そのフォルダーが "ワークスペース" になります。 VS Code では、そのワークスペースに固有の設定は .vscode/settings.json に保存されます。この設定は、グローバルに保存されるユーザー設定とは別のものです。

3. VS Code のドキュメントのチュートリアルで、次の項目に進みます:[Hello World の Python ソース コード ファイルを作成する](https://code.visualstudio.com/docs/python/python-tutorial#_create-a-python-hello-world-source-code-file)。

## <a name="create-a-simple-game-with-pygame"></a>Pygame を使用してシンプルなゲームを作成する

![サンプル ゲームを実行する pygame](../images/pygame-shmup.jpg)

pygame は、ゲーム作成のための定番の Python パッケージであり、楽しいものを作りながらプログラミングを学ぶことができます。 pygame は新しいウィンドウにグラフィックスを表示するため、コマンド ラインのみの WSL 環境では動作しません。 ただし、このチュートリアルで説明しているように Microsoft Store から Python をインストールした場合は、問題なく動作します。

1. Python をインストールしたら、`python -m pip install -U pygame --user` と入力して、コマンド ライン (または VS Code 内のターミナル) から pygame をインストールします。

2. サンプル ゲームを実行してインストールをテストします: `python -m pygame.examples.aliens`

3. 問題がなければ、ゲームのウィンドウが開きます。 プレイを終了したらウィンドウを閉じます。

独自のゲームの作成を開始する方法を次に示します。

1. PowerShell (または Windows コマンド プロンプト) を開き、"bounce" という名前の空のフォルダーを作成します。 このフォルダーに移動し、"bounce.py" という名前のファイルを作成します。 VS Code でフォルダーを開きます。

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

3. `bounce.py` という名前で保存します。

4. PowerShell ターミナルから、`python bounce.py` と入力して実行します。

    ![ゲームを実行する pygame](../images/pygame.jpg)

いくつかの数値を調整して、ボールの跳ね返りにどのような効果があるか確認してみてください。

pygame でのゲームの作成の詳細については、[pygame.org](http://www.pygame.org) を参照してください。

## <a name="resources-for-continued-learning"></a>継続学習のためのリソース

Windows での Python 開発について学習を続けるために役立つ、以下のリソースをお勧めします。

### <a name="online-courses-for-learning-python"></a>オンラインの Python 学習コース

- [Microsoft Learn の Python 入門](/learn/modules/intro-to-python/):対話型の Microsoft Learn プラットフォームで、簡単な Python コードを記述し、変数を宣言し、コンソール入出力を操作する方法を基礎から学びましょう。このモジュールを完了すると経験ポイントを獲得できます。 Python 開発環境をまだセットアップしていない場合、対話型のサンドボックス環境は学習を始めるのに最適な場所です。

- [Pluralsight で Python を学ぶ:8 コース、29 時間](https://app.pluralsight.com/paths/skills/python):Pluralsight の Python ラーニング パスは、スキルを測定してギャップを見つけるためのツールなど、Python 関連のさまざまなトピックをカバーしたオンライン コースを提供します。

- [LearnPython.org のチュートリアル](https://www.learnpython.org/):DataCamp のスタッフが提供する、これらの無料の対話型 Python チュートリアルでは、インストールもセットアップも一切不要で Python の学習を始めることができます。

- [Python.org のチュートリアル](https://docs.python.org/3/tutorial/index.html):Python の言語とシステムの基本的な概念と機能について、わかりやすく解説しています。

- [Lynda.com で Python を学ぶ](https://www.lynda.com/Python-tutorials/Learning-Python/661773-2.html):Python の基礎入門です。

### <a name="working-with-python-in-vs-code"></a>VS Code での Python の使用

- [VS Code での Python の編集](https://code.visualstudio.com/docs/python/editing):VS Code のオートコンプリートおよび IntelliSense サポートを Python 開発で利用する方法 (動作をカスタマイズまたは無効化する方法など) について説明します。

- [Python での lint](https://code.visualstudio.com/docs/python/linting):lint は、コードを分析して潜在的なエラーを検出するプログラムを実行するプロセスです。 VS Code で Python 用に提供されるさまざまな形式の lint サポートと、その設定方法について説明します。

- [Python のデバッグ](https://code.visualstudio.com/docs/python/debugging):デバッグは、コンピューター プログラムのエラーを識別して除去するプロセスです。 この記事では、VS Code で Python のデバッグを初期化および構成する方法、ブレークポイントを設定および検証する方法、ローカル スクリプトをアタッチする方法、さまざまなアプリの種類に対して、またはリモート コンピューターでデバッグを実行する方法、および、いくつかの基本的なトラブルシューティングについて説明します。

- [Python の単体テスト](https://code.visualstudio.com/docs/python/unit-testing):単体テストの意味、チュートリアルの例、テスト フレームワークの有効化、テストの作成と実行、テストのデバッグ、構成設定のテストなど、いくつかの背景について説明します。