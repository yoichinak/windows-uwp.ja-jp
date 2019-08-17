---
title: Windows での Python を使用したスクリプトと自動化
description: Windows でスクリプト、オートメーション、およびシステム管理のための Python の使用を開始する方法について説明します。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: python、windows 10、microsoft、python システム管理、python ファイルオートメーション、windows 上の python スクリプト、windows での python 開発環境、windows 上の python 開発環境、windows 上の python 開発環境、powershell を使用した python、python スクリプトファイルシステムタスク
ms.localizationpriority: medium
ms.date: 07/19/2019
ms.openlocfilehash: 93fdea3347cc15aa6231ff90fb18eb2f7defb201
ms.sourcegitcommit: a28a32fff9d15ecf4a9d172cd0a04f4d993f9d76
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/12/2019
ms.locfileid: "68959068"
---
# <a name="get-started-using-python-on-windows-for-scripting-and-automation"></a>Windows での Python を使用したスクリプト作成と自動化の概要

次に示すのは、開発環境を設定し、Windows でのファイルシステム操作のスクリプト作成と自動化を行うための Python の使用を開始するためのステップバイステップガイドです。

## <a name="set-up-your-development-environment"></a>開発環境の設定

Python を使用してファイルシステム操作を実行するスクリプトを記述する場合[は、Microsoft Store から python をインストール](https://www.microsoft.com/en-us/p/python-37/9nj46sx7x90p?activetab=pivot:overviewtab)することをお勧めします。 Microsoft Store を介してをインストールすると、基本的な Python3 インタープリターが使用されますが、自動更新を提供するだけでなく、現在のユーザーのパス設定 (管理者アクセス権の必要性を回避する) の設定が処理されます。

Windows での**web 開発**に Python を使用している場合は、windows Subsystem for Linux を使用して別のセットアップを行うことをお勧めします。 このガイドのチュートリアルを検索します。[Windows での web 開発用の Python の使用を開始](./python-for-web.md)します。 Python を初めて使用する場合は、次のガイドをお試しください。[初心者向け Windows での Python の使用を開始しましょう](./python-for-education.md)。 いくつかの高度なシナリオ (Python のインストールされたファイルにアクセスして変更する、バイナリのコピーを作成する、Python Dll を直接使用するなど) については、 [python.org](https://www.python.org/downloads/)から特定の Python リリースを直接ダウンロードすることを検討するか、インストールを検討することをお勧めします。Anaconda、Jython、PyPy、WinPython、IronPython などの[代替手段](https://www.python.org/download/alternatives)。これは、別の実装を選択する具体的な理由を持つより高度な Python プログラマの場合にのみお勧めします。

## <a name="install-python"></a>Python をインストールする

Microsoft Store を使用して Python をインストールするには:

1. **[スタート]** メニュー (左下の Windows アイコン) に移動し、「Microsoft Store」と入力し、リンクを選択してストアを開きます。

2. ストアが開いたら、右上のメニューから **[検索]** を選択し、「Python」と入力します。 [アプリ] の下の結果から "Python 3.7" を開きます。 **[取得]** を選択します。

3. Python がダウンロードとインストールのプロセスを完了したら、 **[スタート]** メニュー (左下の windows アイコン) を使用して windows PowerShell を開きます。 PowerShell が開いたら、「 `Python --version` 」と入力して、Python3 がコンピューターにインストールされていることを確認します。

4. Python の Microsoft Store インストールには、標準のパッケージマネージャーである**pip**が含まれています。 Pip を使用すると、Python 標準ライブラリに含まれていない追加のパッケージをインストールして管理することができます。 パッケージをインストールして管理するための pip も使用できることを`pip --version`確認するには、「」と入力します。

## <a name="install-visual-studio-code"></a>Visual Studio Code のインストール

テキストエディター/統合開発環境 (IDE) として VS Code を使用すると、 [IntelliSense](https://code.visualstudio.com/docs/editor/intellisense) (コード補完機能) を利用して[リンティング](https://code.visualstudio.com/docs/python/linting) (コードのエラーを回避するのに役立ちます)、[デバッグサポート](https://code.visualstudio.com/docs/python/debugging)(エラーの検出に役立ちます) を利用できます。実行後のコード)、[コードスニペット](https://code.visualstudio.com/docs/editor/userdefinedsnippets)(小さい再利用可能なコードブロック用のテンプレート)、[単体テスト](https://code.visualstudio.com/docs/python/unit-testing)(さまざまな種類の入力によるコードのインターフェイスのテスト) があります。

Windows の VS Code をダウンロードし、インストール手順[https://code.visualstudio.com](https://code.visualstudio.com)に従います。

## <a name="install-the-microsoft-python-extension"></a>Microsoft Python 拡張機能をインストールする

VS Code サポート機能を利用するには、Microsoft Python 拡張機能をインストールする必要があります。 [詳しくはこちらをご覧ください](https://code.visualstudio.com/docs/languages/python)。

1. **Ctrl + Shift + X キーを押し**て [VS Code 拡張機能] ウィンドウを開きます (または、メニューを使用して**ビュー** > の**拡張機能**に移動します)。

2. **[Marketplace の検索拡張機能]** ボックスに、次のように入力します。**Python**。

3. Microsoft 拡張機能**によって python (ms python. python)** を検索し、緑色の **[インストール]** ボタンを選択します。

## <a name="open-the-integrated-powershell-terminal-in-vs-code"></a>VS Code で統合 PowerShell ターミナルを開きます。

VS Code には、PowerShell を使用して Python コマンドラインを開き、コードエディターとコマンドラインの間にシームレスなワークフローを確立できるようにする[組み込みのターミナル](https://code.visualstudio.com/docs/editor/integrated-terminal)が含まれています。

1. VS Code でターミナルを開き、[**ターミナル**の**表示** > ] を選択するか、またはショートカット**Ctrl + '** (バックティック文字を使用) を使用します。

    > [!NOTE]
    > 既定のターミナルは PowerShell である必要がありますが、変更する必要がある場合は、 **Ctrl + Shift + P キー**を使用してコマンドパレットを入力します。 ターミナル**を入力:[既定の**シェル] を選択すると、PowerShell、コマンドプロンプト、wsl などを含むターミナルオプションの一覧が表示されます。使用するものを選択し、 **Ctrl + Shift + ' キーを押し**て (バックティックを使用して) 新しいターミナルを作成します。

2. VS Code ターミナル内で、次のように入力して Python を開きます。`python`

3. 次`print("Hello World")`のように入力して、Python インタープリターを試してみてください。 Python からステートメント "Hello World" が返されます。

    ![VS Code の Python コマンドライン](../../images/python-in-vscode.png)

4. Python を`exit()`終了するには、 `quit()`、、または Ctrl + Z キーを押します。

## <a name="install-git-optional"></a>Git のインストール (省略可能)

Python コードで他のユーザーと共同作業を行う場合、またはオープンソースサイト (GitHub など) でプロジェクトをホストする場合、VS Code は[Git を使用したバージョン管理](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support)をサポートします。 VS Code の [ソース管理] タブでは、すべての変更が追跡され、UI には一般的な Git コマンド (add、commit、push、pull) が組み込まれています。 最初に Git をインストールして、ソース管理パネルを電源にする必要があります。

1. Git [-scm web サイト](https://git-scm.com/download/win)から Git for Windows をダウンロードしてインストールします。

2. インストールウィザードには、Git インストールの設定に関する一連の質問が記載されています。 何らかの変更に特に理由がない限り、すべての既定の設定を使用することをお勧めします。

3. 前に Git を使用したことがない場合は、 [GitHub のガイド](https://guides.github.com/)を参考にしてください。

## <a name="example-script-to-display-the-structure-of-your-file-system-directory"></a>ファイルシステムディレクトリの構造を表示するスクリプトの例

一般的なシステム管理タスクにはかなりの時間がかかることがありますが、Python スクリプトを使用すると、これらのタスクを自動化することができ、時間がかかることはありません。 たとえば、Python では、コンピューターのファイルシステムの内容を読み取って、ファイルやディレクトリの概要を印刷したり、フォルダーを別のディレクトリに移動したり、数百のファイルの名前を変更したりするなどの操作を実行できます。 通常、このようなタスクは、手動で実行すると、時間がかかることがあります。 代わりに Python スクリプトを使用してください。

まず、ディレクトリツリーを説明し、ディレクトリ構造を表示する単純なスクリプトを見てみましょう。

1. **[スタート]** メニュー (左下の Windows アイコン) を使用して PowerShell を開きます。

2. プロジェクト`mkdir python-scripts`のディレクトリを作成します。次に、その`cd python-scripts`ディレクトリを開きます。

3. サンプルスクリプトで使用するディレクトリをいくつか作成します。

    ```powershell
    mkdir food, food/fruits, food/fruits/apples, food/fruits/oranges, food/vegetables
    ```

4. 次のスクリプトで使用するために、これらのディレクトリ内にいくつかのファイルを作成します。

    ```powershell
    new-item food/fruits/banana.txt, food/fruits/strawberry.txt, food/fruits/blueberry.txt, food/fruits/apples/honeycrisp.txt, food/fruits/oranges/mandarin.txt, food/vegetables/carrot.txt
    ```

5. Python スクリプトディレクトリに新しい python ファイルを作成します。

    ```powershell
    mkdir src
    new-item src/list-directory-contents.py
    ```

6. 次のように入力して、VS Code でプロジェクトを開きます。`code .`

7. **Ctrl キーと Shift キーを押しながら E**キーを押して (またはメニューを使用して**表示** > **エクスプローラー**に移動し)、[VS Code エクスプローラー] ウィンドウを開き、先ほど作成した list-directory-contents.py ファイルを選択します。 Microsoft Python 拡張機能は、Python インタープリターを自動的に読み込みます。 VS Code ウィンドウの下部に読み込まれたインタープリターを確認できます。

    > [!NOTE]
    > Python は解釈された言語であり、物理コンピューターをエミュレートする仮想マシンとして機能します。 使用できる Python インタープリターには、次のような種類があります。Python 2、Python 3、Anaconda、PyPy などPython コードを実行して Python IntelliSense を取得するには、どのインタープリターを使用するか VS Code に指示する必要があります。 別のものを選択する特別な理由がない限り、既定で選択されているインタープリター (この場合は Python 3) には VS Code ないことをお勧めします。 Python インタープリターを変更するには、VS Code ウィンドウの下部にある青いバーに現在表示されているインタープリターを選択するか、**コマンドパレット**(Ctrl + Shift + P **) を開いて、コマンド Python を入力します。[インタープリター**] を選択します。 これにより、現在インストールされている Python インタープリターの一覧が表示されます。 [詳細については、Python 環境の構成に関する](https://code.visualstudio.com/docs/python/environments)ページをご覧ください。

    ![VS Code で Python インタープリターを選択します](../../images/interpreterselection.gif)

8. List-directory-contents.py ファイルに次のコードを貼り付け、 **[保存]** を選択します。

    ```python
    import os

    root = '%s%s%s' % ('..', os.path.sep, 'food')
    for directory, subdir_list, file_list in os.walk(root):
        print('Directory: ' + directory)
        for name in subdir_list:
            print ('Subdirectory: ' + name)
        for name in file_list:
            print('File: ' + name)
        print(os.linesep)
    ```

9. VS Code 統合ターミナル (**Ctrl + '** 、バックティック文字) を開き、Python スクリプトを保存した場所の src ディレクトリを入力します。

    ```powershell
    cd src
    ```

10. 次のものを使用して PowerShell でスクリプトを実行します。

    ```powershell
    python3 .\list-directory-contents.py
    ```

    次のような出力が表示されます。

    ```powershell
    Directory: ../food
    Subdirectory: fruits
    Subdirectory: vegetables

    Directory: ../food\fruits
    Subdirectory: apples
    Subdirectory: oranges
    File: banana.txt
    File: blueberry.txt
    File: strawberry.txt

    Directory: ../food\fruits\apples
    File: honeycrisp.txt

    Directory: ../food\fruits\oranges
    File: mandarin.txt

    Directory: ../food\vegetables
    File: carrot.txt
    ```

11. Python を使用して、このコマンドを PowerShell ターミナルに直接入力することで、そのファイルシステムディレクトリの出力を独自のテキストファイルに出力します。`python3 list-directory-contents.py > food-directory.txt`

おめでとうございます! 作成したディレクトリとファイルを読み取って Python を使用して、ディレクトリ構造を独自のテキストファイルに表示して印刷する、自動化された systems 管理スクリプトを作成しました。

## <a name="example-script-to-modify-all-files-in-a-directory"></a>ディレクトリ内のすべてのファイルを変更するスクリプトの例

この例では、作成したファイルとディレクトリを使用して、ファイルの最終更新日をファイル名の先頭に追加することで、各ファイルの名前を変更します。

1. **Python スクリプト**ディレクトリ内の**src**フォルダー内に、スクリプト用の新しい python ファイルを作成します。

    ```powershell
    new-item update-filenames.py
    ```

2. Update-filenames.py ファイルを開き、次のコードをファイルに貼り付けて保存します。

    > [!NOTE]
    > getmtime は、簡単に読み取ることができないタイムスタンプをタイマー刻みで返します。 最初に標準の datetime 文字列に変換する必要があります。

    ```python
    import datetime
    import os

    root = '%s%s%s' % ('..', os.path.sep, 'food')
    for directory, subdir_list, file_list in os.walk(root):
        for name in file_list:
            source_name = '%s%s%s' % (directory, os.path.sep, name)
            timestamp = os.path.getmtime(source_name)
            modified_date = str(datetime.datetime.fromtimestamp(timestamp)).replace(':', '.')
            target_name = '%s%s%s_%s' % (directory, os.path.sep, modified_date, name)

            print ('Renaming: %s to: %s' % (source_name, target_name))

            os.rename(source_name, target_name)
    ```

3. Update-filenames.py スクリプトを実行`python3 update-filenames.py`してテストし、list-directory-contents.py スクリプトをもう一度実行します。`python3 list-directory-contents.py`

4. 次のような出力が表示されます。

    ```powershell
    Renaming: ..\food\fruits\banana.txt to: ..\food\fruits\2019-07-18 12.24.46.385185_banana.txt
    Renaming: ..\food\fruits\blueberry.txt to: ..\food\fruits\2019-07-18 12.24.46.391170_blueberry.txt
    Renaming: ..\food\fruits\strawberry.txt to: ..\food\fruits\2019-07-18 12.24.46.389174_strawberry.txt
    Renaming: ..\food\fruits\apples\honeycrisp.txt to: ..\food\fruits\apples\2019-07-18 12.24.46.395160_honeycrisp.txt
    Renaming: ..\food\fruits\oranges\mandarin.txt to: ..\food\fruits\oranges\2019-07-18 12.24.46.398151_mandarin.txt
    Renaming: ..\food\vegetables\carrot.txt to: ..\food\vegetables\2019-07-18 12.24.46.402496_carrot.txt

    ~/src/python-scripting/src$ python3 .\list-directory-contents.py
    ..\food\
    Directory: ..\food
    Subdirectory: fruits
    Subdirectory: vegetables

    Directory: ..\food\fruits
    Subdirectory: apples
    Subdirectory: oranges
    File: 2019-07-18 12.24.46.385185_banana.txt
    File: 2019-07-18 12.24.46.389174_strawberry.txt
    File: 2019-07-18 12.24.46.391170_blueberry.txt

    Directory: ..\food\fruits\apples
    File: 2019-07-18 12.24.46.395160_honeycrisp.txt

    Directory: ..\food\fruits\oranges
    File: 2019-07-18 12.24.46.398151_mandarin.txt

    Directory: ..\food\vegetables
    File: 2019-07-18 12.24.46.402496_carrot.txt

    ```

5. Python を使用して、PowerShell ターミナルで次のコマンドを直接入力して、最後に変更されたタイムスタンプを持つ新しいファイルシステムディレクトリ名を、独自のテキストファイルの先頭に追加します。`python3 list-directory-contents.py > food-directory-last-modified.txt`

基本的なシステム管理タスクを自動化するための Python スクリプトの使用について、いくつかのおもしろいことを願っています。 もちろん、もっと多くのことを知りたいと思いますが、すぐに始められることを願っています。 以下の学習を続けるために、いくつかのリソースを共有しています。

## <a name="additional-resources"></a>その他の資料

- [Python ドキュメント:ファイルとディレクトリへ](https://docs.python.org/3.7/library/filesys.html)のアクセス:ファイルシステムの操作、ファイルのプロパティの読み取り、移植可能な方法でのパスの操作、一時ファイルの作成に関する Python ドキュメント。
- [Python について学習する:String_Formatting チュートリアル](https://www.learnpython.org/en/String_Formatting):文字列の書式設定に "%" 演算子を使用する方法の詳細については、こちらを参照してください。
- [10 個の Python ファイルシステムメソッドについて理解しておく必要があり](https://towardsdatascience.com/10-python-file-system-methods-you-should-know-799f90ef13c2)ます。`os` および`shutil`でのファイルとフォルダーの操作に関する中程度の記事。
- [Python の Hitchhikers ガイド:システム管理](https://docs.python-guide.org/scenarios/admin/):Python に関連するトピックに関する概要とベストプラクティスを提供する "こだわり guide"。 このセクションでは、システム管理ツールとフレームワークについて説明します。 このガイドは GitHub でホストされているため、問題を報告し、投稿を行うことができます。
