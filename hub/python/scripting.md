---
title: Windows での Python によるスクリプト作成と自動化の概要
description: Windows でスクリプト作成、自動化、およびシステム管理のために Python の使用を開始する方法について説明します。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: python, windows 10, microsoft, python システム管理, python ファイル自動化, windows での python スクリプト, windows での python セットアップ, windows での python 開発環境, windows での python 開発環境, powershell を使用した python, ファイル システム タスク用の python スクリプト
ms.localizationpriority: medium
ms.date: 07/19/2019
ms.openlocfilehash: a8f13243f3501b2af42d38c13bff580be2e5b42a
ms.sourcegitcommit: 8040760f5520bd1732c39aedc68144c4496319df
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/22/2021
ms.locfileid: "98691327"
---
# <a name="get-started-using-python-on-windows-for-scripting-and-automation"></a>Windows で Python を使用してスクリプト作成と自動化を開始する

以下に示すのは、開発者環境を設定し、Windows で Python を使用し、ファイル システム操作のスクリプト作成と自動化を開始するためのステップ バイ ステップ ガイドです。

> [!NOTE]
> この記事では、Python の便利なライブラリの一部を使用するように環境をセットアップする方法について説明します。これにより、ファイル システムの検索、インターネットへのアクセス、ファイルの種類の解析など、Windows 中心のアプローチからプラットフォーム間でタスクを自動化することができます。 Windows 固有の操作の場合は、Python 用の C 互換の外部関数ライブラリである [ctypes](https://docs.python.org/3/library/ctypes.html)、Windows レジストリ API を Python に公開する機能である [winreg](https://docs.python.org/3/library/winreg.html)、Python から Windows ランタイム API にアクセスできるようにする [Python/WinRT](https://pypi.org/project/winrt/) を確認してください。

## <a name="set-up-your-development-environment"></a>開発環境を設定する

Python を使用してファイル システムの操作を実行するスクリプトを記述する場合は、[Microsoft Store から Python をインストールする](https://www.microsoft.com/p/python-37/9nj46sx7x90p?activetab=pivot:overviewtab)ことをお勧めします。 Microsoft Store を介してインストールすると、基本的な Python3 インタープリターが使用されますが、自動更新が提供されるだけでなく、現在のユーザーの PATH 設定 (管理者アクセス権は不要) も行われます。

Windows で Python を使用して **Web 開発** を行う場合は、Windows Subsystem for Linux を使用して別のセットアップを行うことをお勧めします。 ガイドのチュートリアル「[Windows での Web 開発用の Python の使用を開始する](./web-frameworks.md)」をご覧ください。 Python を初めて使用する場合は、[初心者向けの Windows での Python の使用](./beginners.md)に関する記事をご覧ください。 一部の高度なシナリオ (Python のインストール ファイルへのアクセスや修正が必要な場合、バイナリのコピーを作成する場合、Python DLL を直接使用する場合など) では、[python.org](https://www.python.org/downloads/) から特定の Python リリースを直接ダウンロードすることを検討するか、Anaconda、Jython、PyPy、WinPython、IronPython などの[代替手段をインストール](https://www.python.org/download/alternatives)することを検討してください。これは、別の実装を選択する具体的な理由がある、より高度な Python プログラマの場合にのみお勧めします。

## <a name="install-python"></a>Python のインストール

Microsoft Store を使用して Python をインストールするには:

1. **スタート** メニュー (左下の Windows アイコン) に移動し、「Microsoft Store」と入力してリンクを選択し、ストアを開きます。

2. ストアが開いたら、右上のメニューから **[検索]** を選択し、「Python」と入力します。 [アプリ] の下の結果から "Python 3.7" を開きます。 **[取得]** を選択します。

3. Python がダウンロードとインストールのプロセスを完了したら、**スタート** メニュー (左下の Windows アイコン) を使用して Windows PowerShell を開きます。 PowerShell が開いたら、`Python --version` を入力して、マシンに Python3 がインストールされていることを確認します。

4. Python の Microsoft Store インストールには、標準のパッケージ マネージャーである **pip** が含まれています。 pip を使用すると、Python 標準ライブラリに含まれていない追加のパッケージをインストールして管理することができます。 また、パッケージのインストールと管理に pip が使用できることを確認するには、`pip --version` と入力します。

## <a name="install-visual-studio-code"></a>Visual Studio Code をインストールする

テキスト エディターおよび統合開発環境 (IDE) として VS Code を使用すると、[IntelliSense](https://code.visualstudio.com/docs/editor/intellisense) (コード補完支援)、[Linting](https://code.visualstudio.com/docs/python/linting) (コードにエラーが発生しないようにする)、[デバッグ サポート](https://code.visualstudio.com/docs/python/debugging) (実行後にコードのエラーを検出)、[コード スニペット](https://code.visualstudio.com/docs/editor/userdefinedsnippets) (小さい再利用可能なコード ブロック用のテンプレート)、[単体テスト](https://code.visualstudio.com/docs/python/unit-testing) (さまざまな種類の入力を使用したコードのインターフェイスのテスト) を活用できます。

[https://code.visualstudio.com](https://code.visualstudio.com) から Windows 用の VS Code をダウンロードし、インストール手順に従います。

## <a name="install-the-microsoft-python-extension"></a>Microsoft Python 拡張機能をインストールする

VS Code のサポート機能を利用するには、Microsoft Python 拡張機能をインストールする必要があります。 [詳しくはこちらをご覧ください](https://code.visualstudio.com/docs/languages/python)。

1. **Ctrl + Shift + X** を入力して VS Code 拡張機能ウィンドウを開きます (または、メニューを使用して **[表示]**  >  **[拡張機能]** に移動します)。

2. 上部の **[Marketplace で拡張機能を検索する]** ボックスに、次のように入力します: **Python**。

3. **Python (ms-python.python) by Microsoft** 拡張機能を探し、緑色の **[インストール]** ボタンを選択します。

## <a name="open-the-integrated-powershell-terminal-in-vs-code"></a>VS Code で統合 PowerShell ターミナルを開きます。

また、VS Code に含まれる[組み込みターミナル](https://code.visualstudio.com/docs/editor/integrated-terminal)では、PowerShell を使用して Python コマンド ラインを開き、コード エディターとコマンド ラインの間でシームレスなワークフローを確立することができます。

1. **[表示]**  >  **[ターミナル]** を選択するか、ショートカット **Ctrl + `** (バックティック文字を使用) を使用して VS Code でターミナルを開きます。

    > [!NOTE]
    > 既定のターミナルは PowerShell であるはずですが、変更する必要がある場合は **Ctrl + Shift + P** を使用してコマンド パレットを入力します。 **Terminal: Select Default Shell** と入力すると、PowerShell、コマンド プロンプト、WSL などのターミナル オプションの一覧が表示されます。使用するものを選択し、**Ctrl+Shift+`** (バックティック文字を使用) キーを押して新しいターミナルを作成します。

2. VS Code ターミナル内で、`python` と入力して Python を開きます。

3. `print("Hello World")` と入力して、Python インタープリターを試してみます。 Python から "Hello World" 文字列が返されます。

    ![VS Code 内の Python コマンド ライン](../images/python-in-vscode.png)

4. Python を終了するには、`exit()` または `quit()` と入力するか、Ctrl - Z キーを押します。

## <a name="install-git-optional"></a>Git のインストール (省略可能)

共同作業で Python コードを開発する場合や、(GitHub のような) オープンソース サイトでプロジェクトをホストする場合のために、VS Code では [Git によるバージョン管理](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support)がサポートされています。 VS Code の [ソース管理] タブでは、すべての変更が追跡され、一般的な Git コマンド (追加、コミット、プッシュ、プル) が UI に組み込まれています。 ソース管理パネルを使用するには、まず Git をインストールする必要があります。

1. Windows 用の Git を [git-scm Web サイト](https://git-scm.com/download/win)からダウンロードしてインストールします。

2. インストール ウィザードで、Git インストールの設定に関する一連の質問に答えます。 何かを変更する特別な理由がない限り、すべて既定の設定を使用することをお勧めします。

3. 以前に Git を使用したことがない場合、入門用の [GitHub ガイド](https://guides.github.com/)が役に立ちます。

## <a name="example-script-to-display-the-structure-of-your-file-system-directory"></a>ファイル システム ディレクトリの構造を表示するスクリプトの例

一般的なシステム管理タスクにはかなりの時間がかかることがありますが、Python スクリプトを使用すると、これらのタスクを自動化することができ、時間がかかることはありません。 たとえば、Python では、コンピューターのファイル システムの内容を読み取って、ファイルやディレクトリの概要を印刷したり、フォルダーを別のディレクトリに移動したり、数百のファイルの名前を変更したりするなどの操作を実行できます。 通常、このようなタスクは、手動で実行すると時間がかかることがあります。 代わりに Python スクリプトを使用してください。

まず、ディレクトリ ツリーをたどってディレクトリ構造を表示する単純なスクリプトを見てみましょう。

1. **スタート** メニュー (左下の Windows アイコン) を使用して PowerShell を開きます。

2. `mkdir python-scripts` でプロジェクトのディレクトリを作成し、`cd python-scripts` でそのディレクトリを開きます。

3. サンプル スクリプトで使用するいくつかのディレクトリを作成します。

    ```powershell
    mkdir food, food\fruits, food\fruits\apples, food\fruits\oranges, food\vegetables
    ```

4. 次のスクリプトで使用するために、これらのディレクトリ内にいくつかのファイルを作成します。

    ```powershell
    new-item food\fruits\banana.txt, food\fruits\strawberry.txt, food\fruits\blueberry.txt, food\fruits\apples\honeycrisp.txt, food\fruits\oranges\mandarin.txt, food\vegetables\carrot.txt
    ```

5. python-scripts ディレクトリに新しい python ファイルを作成します。

    ```powershell
    mkdir src
    new-item src\list-directory-contents.py
    ```

6. `code .` と入力して VS Code でプロジェクトを開きます。

7. **Ctrl+Shift+E** キーを押して VS Code ファイル エクスプローラー ウィンドウを開きます (または、メニューを使用して **[表示]**  >  **[エクスプローラー]** ) に移動し、作成した list-directory-contents.py ファイルを選択します。 Microsoft Python 拡張機能は、Python インタープリターを自動的に読み込みます。 VS Code ウィンドウの下部で読み込まれたインタープリターを確認できます。

    > [!NOTE]
    > Python はインタープリター言語であり、物理コンピューターをエミュレートする仮想マシンとして機能します。 使用できる Python インタープリターには、Python 2、Python 3、Anaconda、PyPy などの種類があります。Python コードを実行して Python IntelliSense を活用するには、使用するインタープリターを VS Code に指示する必要があります。 別のものを選択する特別な理由がない限り、VS Code が既定で選択するインタープリター (この場合は Python 3) を使用することをお勧めします。 Python インタープリターを変更するには、VS Code ウィンドウの下部にある青いバーに現在表示されているインタープリターを選択するか、**コマンド パレット** (Ctrl + Shift + P) を開いて **Python: Select Interpreter** コマンドを入力します。 これにより、現在インストールされている Python インタープリターの一覧が表示されます。 [Python 環境の構成の詳細については、こちらを参照してください](https://code.visualstudio.com/docs/python/environments)。

    ![VS Code で Python インタープリターを選択する](../images/interpreterselection.gif)

8. list-directory-contents.py ファイルに次のコードを貼り付け、 **[保存]** を選択します。

    ```python
    import os

    root = os.path.join('..', 'food')
    for directory, subdir_list, file_list in os.walk(root):
        print('Directory:', directory)
        for name in subdir_list:
            print('Subdirectory:', name)
        for name in file_list:
            print('File:', name)
        print()
    ```

9. VS Code 統合ターミナル (**Ctrl+`** 、バックティック文字を使用) を開き、Python スクリプトを保存した src ディレクトリを入力します。

    ```powershell
    cd src
    ```

10. PowerShell でスクリプトを実行します。

    ```powershell
    python3 .\list-directory-contents.py
    ```

    アウトプットは次のようになります。

    ```powershell
    Directory: ..\food
    Subdirectory: fruits
    Subdirectory: vegetables

    Directory: ..\food\fruits
    Subdirectory: apples
    Subdirectory: oranges
    File: banana.txt
    File: blueberry.txt
    File: strawberry.txt

    Directory: ..\food\fruits\apples
    File: honeycrisp.txt

    Directory: ..\food\fruits\oranges
    File: mandarin.txt

    Directory: ..\food\vegetables
    File: carrot.txt
    ```

11. `python3 list-directory-contents.py > food-directory.txt` コマンドを入力し、Python を使用して PowerShell ターミナルからファイル システム ディレクトリの出力を直接テキスト ファイルに出力します。

お疲れさまでした。 作成したディレクトリとファイルを読み取り、Python を使用してディレクトリ構造を表示して独自のテキスト ファイルに出力する、自動化されたシステム管理スクリプトを作成しました。

> [!NOTE]
> Microsoft Store から Python 3 をインストールできない場合は、このサンプル スクリプトのパスを処理する方法の例について、こちらの[イシュー](https://github.com/MicrosoftDocs/windows-uwp/issues/2901)を参照してください。

## <a name="example-script-to-modify-all-files-in-a-directory"></a>ディレクトリ内のすべてのファイルを変更するスクリプトの例

この例では、先ほど作成したファイルとディレクトリを使用して、ファイルの最終更新日をファイル名の先頭に追加することで、各ファイルの名前を変更します。

1. **python-scripts** ディレクトリの **src** フォルダー内に、スクリプト用の新しい Python ファイルを作成します。

    ```powershell
    new-item update-filenames.py
    ```

2. update-filenames.py ファイルを開き、次のコードをファイルに貼り付けて保存します。

    > [!NOTE]
    > os.getmtime は、秒単位のタイムスタンプを読みにくい形式で返します。 最初に標準の datetime 文字列に変換する必要があります。

    ```python
    import datetime
    import os

    root = os.path.join('..', 'food')
    for directory, subdir_list, file_list in os.walk(root):
        for name in file_list:
            source_name = os.path.join(directory, name)
            timestamp = os.path.getmtime(source_name)
            modified_date = str(datetime.datetime.fromtimestamp(timestamp)).replace(':', '.')
            target_name = os.path.join(directory, f'{modified_date}_{name}')

            print(f'Renaming: {source_name} to: {target_name}')

            os.rename(source_name, target_name)
    ```

3. `python3 update-filenames.py` で update-filenames.py スクリプトを実行してテストし、次に `python3 list-directory-contents.py` で list-directory-contents.py スクリプトを再実行します。

4. 出力は次のようになります。

    ```powershell
    Renaming: ..\food\fruits\banana.txt to: ..\food\fruits\2019-07-18 12.24.46.385185_banana.txt
    Renaming: ..\food\fruits\blueberry.txt to: ..\food\fruits\2019-07-18 12.24.46.391170_blueberry.txt
    Renaming: ..\food\fruits\strawberry.txt to: ..\food\fruits\2019-07-18 12.24.46.389174_strawberry.txt
    Renaming: ..\food\fruits\apples\honeycrisp.txt to: ..\food\fruits\apples\2019-07-18 12.24.46.395160_honeycrisp.txt
    Renaming: ..\food\fruits\oranges\mandarin.txt to: ..\food\fruits\oranges\2019-07-18 12.24.46.398151_mandarin.txt
    Renaming: ..\food\vegetables\carrot.txt to: ..\food\vegetables\2019-07-18 12.24.46.402496_carrot.txt

    PS C:\src\python-scripting\src> python3 .\list-directory-contents.py
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

5. `python3 list-directory-contents.py > food-directory-last-modified.txt` コマンドは、Python を使用して PowerShell ターミナルから、最終更新日時のタイプスタンプが先頭に付いた新しいファイル システム ディレクトリの名前を直接テキスト ファイルに出力します。

基本的なシステム管理タスクを自動化するための Python スクリプトの使用について、いくつかのおもしろいことを学習できました。 もちろん、知るべきことはさらにたくさんありますが、これは適切な開始点となったはずです。 学習を続けるために、いくつかのリソースを共有します。

## <a name="additional-resources"></a>その他のリソース

- [Python ドキュメント:ファイルとディレクトリへのアクセス](https://docs.python.org/3.7/library/filesys.html):ファイル システムの操作、ファイルのプロパティの読み取り、移植可能な方法でのパスの操作、一時ファイルの作成に関する Python ドキュメントです。
- [Python について学習する:文字列フォーマット チュートリアル](https://www.learnpython.org/en/String_Formatting):文字列の書式設定に "%" 演算子を使用する方法の詳細については、こちらを参照してください。
- [知っておくべき 10 個の Python ファイル システム メソッド](https://towardsdatascience.com/10-python-file-system-methods-you-should-know-799f90ef13c2):`os` と `shutil` を使用したファイルとフォルダーの操作に関する Medium の記事です。
- [Python ヒッチハイク ガイド:システム管理](https://docs.python-guide.org/scenarios/admin/):Python 関連のトピックに関する概要とベスト プラクティスを提供する「こだわりのガイド」です。 このセクションでは、システム管理ツールとフレームワークについて説明しています。 このガイドは GitHub にホストされているため、問題の報告や投稿を行うことができます。
