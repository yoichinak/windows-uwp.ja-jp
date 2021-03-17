---
title: Windows で Rust 用の開発環境を設定する
description: Rust を使用した Windows での開発に興味を持っている初心者に向けた、開発環境の設定。
author: stevewhims
ms.author: stwhi
manager: jken
ms.topic: article
keywords: rust、windows 10、 microsoft、 rust の学習、windows での rust (初心者向け)、vs code を使用した rust
ms.localizationpriority: medium
ms.date: 03/04/2021
ms.openlocfilehash: 5aac8dd9b9f760f6e1ed49ff0246e44c400d72c4
ms.sourcegitcommit: 85b9a5fc16f4486bc23b4ec8f4fae5ab6211a066
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/05/2021
ms.locfileid: "102194157"
---
# <a name="set-up-your-dev-environment-on-windows-for-rust"></a>Windows で Rust 用の開発環境を設定する

「[Rust を使用した Windows での開発の概要](overview.md)」トピックでは、Rust を紹介し、どのようなものであるかを説明しました。また、一部の主要動作コンポーネントについて説明しました。 このトピックでは、開発環境を設定します。

Rust 開発は Windows で行うことをお勧めします。 ただし、Linux でローカルにコンパイルしてテストする予定の場合は、[Linux 用 Windows サブシステム (WSL)](/windows/wsl/about) 上で Rust を使用して開発することも選択できます。

## <a name="install-visual-studio-recommended-or-the-microsoft-c-build-tools"></a>Visual Studio (推奨) または Microsoft C++ Build Tools をインストールする

Windows では、Rust 用に特定の C++ ビルド ツールが必要です。

[Microsoft C++ Build Tools](https://visualstudio.microsoft.com/visual-cpp-build-tools/) をダウンロードできます。または、[Microsoft Visual Studio](https://visualstudio.microsoft.com/downloads/) をインストールするだけでもかまいません (推奨)。

> [!NOTE]
> ここでは、Rust の統合開発環境 (IDE) として、Visual Studio ではなく Visual Studio Code を使用します。 しかし、それでも出費なく Visual Studio をインストールできます。 Community エディションが提供されており、これは、学生、オープンソース貢献者、個人には無料です。

Visual Studio をインストールするときに、&mdash; **[.NET デスクトップ開発]** 、 **[C++ によるデスクトップ開発]** 、および **[ユニバーサル Windows プラットフォーム開発]** を選択することをお勧めする Windows ワークロードがいくつかあります。 3 つすべてが必要とは思わないかもしれませんが、それらが必要な場所で何らかの依存関係が生じる可能性があるため、ただ 3 つすべてを選択するのがより簡単だと思われます。

新しい Rust プロジェクトでは既定で Git が使用されます。 そのため、個別コンポーネントの **Git for Windows** もミックスに追加します (検索ボックスを使用して、名前でそれを検索します)。

![.NET デスクトップ開発、C++ によるデスクトップ開発、ユニバーサル Windows プラットフォーム開発](../../images/rust-vs-workloads.png)

## <a name="install-rust"></a>Rust をインストールする

次に、[Rust Web サイトから Rust をインストールします](https://www.rust-lang.org/tools/install)。 Web サイトでは、Windows を実行していることが検出されます。また、Windows 用 `rustup` ツールの 64 ビットおよび 32 ビットのインストーラーと、[Linux 用 Windows サブシステム (WSL)](/windows/wsl/about) への Rust のインストールに関する手順が提供されます。

> [!TIP]
> Rust は Windows で非常にうまく機能します。そのため、WSL のルートに進む必要はありません (Linux でローカルにコンパイルしてテストする予定ではない場合)。 Windows を使用しているため、64 ビット Windows 用の `rustup` インストーラーのみを実行することをお勧めします。 その後、Rust を使用して Windows *のための* アプリを記述するようにすべて設定されます。

Rust のインストーラーが完了したら、Rust でプログラミングする準備が整います。 便利な IDE はまだありません (これは次のセクション「&mdash;[Visual Studio Code のインストール](#install-visual-studio-code)」で取り上げます)。 また、Windows API を呼び出すようにまだ設定されていません。 しかし、コマンド プロンプト (**VS 用の x64 Native Tools コマンド プロンプト** または任意の `cmd.exe`) を起動して、おそらく `cargo --version` コマンドを発行することもできます。 バージョン番号が出力されている場合は、それが、Rust が正しくインストールされていることの確認となります。

上記の `cargo` キーワードの使用に興味がある場合、*Cargo* は、プロジェクト (より適切には "*パッケージ*") とその依存関係の管理とビルドを行う、Rust 開発環境のツールの名前です。

また、この時点で (IDE の利便性を利用しなくても) どうしてもプログラミングに着手したい場合は、「[Hello, World!](https://doc.rust-lang.org/book/ch01-02-hello-world.html)」を読むことができます (Rust Web サイトの Rust プログラミング言語ブックの章です)。

## <a name="install-visual-studio-code"></a>Visual Studio Code をインストールする

テキスト エディターまたは統合開発環境 (IDE) として Visual Studio Code (VS Code) を使用すれば、コード補完、構文の強調表示、形式の指定、デバッグなどの言語サービスを活用できます。

VS Code には、コマンドライン引数を発行できる (たとえば、Cargo へのコマンドを発行する) [組み込みターミナル](https://code.visualstudio.com/docs/editor/integrated-terminal)も含まれています。

1. まず、[Windows 用 Visual Studio Code](https://code.visualstudio.com) をダウンロードしてインストールします。

2. VS Code をインストールしたら、**rust-analyzer** "*拡張機能*" をインストールします。 [Visual Studio Marketplace から rust-analyzer 拡張機能](https://marketplace.visualstudio.com/items?itemName=matklad.rust-analyzer)をインストールするか、VS Code を開き、拡張機能メニュー (Ctrl + Shift + X) で **rust-analyzer** を検索することができます。

3. デバッグのサポートのためには、**CodeLLDB** 拡張機能をインストールしてください。 [Visual Studio Marketplace から CodeLLDB 拡張機能](https://marketplace.visualstudio.com/items?itemName=vadimcn.vscode-lldb)をインストールするか、VS Code を開き、拡張機能メニュー (Ctrl + Shift + X) で **CodeLLDB** を検索することができます。

   > [!NOTE]
   > デバッグをサポートするための **CodeLLDB** 拡張機能の代わりとなるのは、Microsoft **C/C++** 拡張機能です。 **C/C++** 拡張機能は、**CodeLLDB** のようには IDE に統合されません。 しかし、その **C/C++** 拡張機能では、優れたデバッグ情報が提供されます。 そのため、それが必要な場合に準備しておくことをお勧めします。
   >
   > [Visual Studio Marketplace から C/C++ 拡張機能](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools)をインストールするか、VS Code を開き、拡張機能メニュー (Ctrl + Shift + X) で **C/C++** を検索することができます。

4. VS Code でターミナルを開く場合は、 **[表示]**  >  **[ターミナル]** を選択するか、ショートカット **Ctrl + `** を使用します (バックティック文字を使用)。 既定のターミナルは PowerShell です。

## <a name="hello-world-tutorial-rust-with-vs-code"></a>Hello, world! チュートリアル (VS Code を使用した Rust)

簡単な "Hello, world!" を使用して Rust アプリを試してみましょう。

1. 最初に、コマンド プロンプト (**VS 用の x64 Native Tools コマンド プロンプト** または任意の `cmd.exe`) を起動し、Rust プロジェクトを保持するフォルダーに `cd` で移動します。

2. 次に、次のコマンドを使用して、新しい Rust プロジェクトを作成するよう Cargo に指示します。

   ```console
   cargo new first_rust_project
   ```

   `cargo new` コマンドに渡す引数は、Cargo によって作成するプロジェクトの名前です。 ここでは、プロジェクト名は *first_rust_project* です。 Rust プロジェクトの名前を付けるときには、スネーク ケース (単語は小文字にして、各スペースをアンダースコアに置き換えます) を使用することをお勧めします。

   入力した名前を使用して、Cargo によってプロジェクトが自動的に作成されます。 そして、実際には、Cargo の新しいプロジェクトには、*Hello, world!* メッセージ (後で表示されます) を出力する非常に単純なアプリのソース コードが含まれています。 Cargo では、*first_rust_project* プロジェクトの作成に加え、*first_rust_project* という名前のフォルダーが作成されて、その中にプロジェクトのソース コード ファイルが配置されます。

3. では、ここで `cd` によってそのフォルダーに移動してから、そのフォルダーのコンテキスト内から VS Code を起動します。

   ```console
   cd first_rust_project
   code .
   ```

4. VS Code のエクスプローラーで、`src` > `main.rs` ファイルを開きます。これは、アプリのエントリ ポイント (**main** という名前の関数) が含まれる Rust ソース コード ファイルです。 次のように表示されます。

   ```rust
   // main.rs
   fn main() {
     println!("Hello, world!");
   }
   ```

   > [!NOTE]
   > VS Code で最初の `.rs` ファイルを開くと、一部の Rust コンポーネントがインストールされていないという通知が表示され、それらをインストールするかどうかをたずねられます。 **[はい]** をクリックすると、VS Code によって Rust 言語サーバーがインストールされます。

   `main.rs` のコードを見ると、**main** が関数定義であり、それが文字列 "Hello, world!" を出力することがわかります。 構文の詳細については、Rust Web サイトの「[Rust プログラムの構造](https://doc.rust-lang.org/book/ch01-02-hello-world.html#anatomy-of-a-rust-program)」を参照してください。

5. ここでは、デバッガーでアプリの実行を試みましょう。 2 行目にブレークポイントを置き、 **[実行]**  >  **[デバッグの開始]** をクリックします (または **F5** キーを押します)。 テキスト エディター内にも **[デバッグ]** と **[実行]** のコマンドが埋め込まれています。

   > [!NOTE]
   > デバッガーで初めてアプリを実行すると、"起動構成が提供されていないため、デバッグを開始できません" というダイアログ ボックスが表示されます。 **[OK]** をクリックすると、"このワークスペースで Cargo.toml が検出されました。 ターゲットの起動構成を生成しますか?" という 2 番目のダイアログ ボックスが表示されます。 **[はい]** をクリックします。 次に、launch.json ファイルを閉じて、もう一度デバッグを開始します。

6. ご覧のようにデバッガーは 2 行目で中断します。 **F5** キーを押して続行すると、アプリは完了まで実行されます。 **[ターミナル]** ペインに、予期した出力である "Hello, world!" が表示されます。

## <a name="rust-for-windows"></a>Windows 用 Rust

Windows "*で*" Rust を使用できるだけでなく、Rust を使用して Windows "*のための*" アプリを記述することもできます。 *Windows* クレートから、過去、現在、将来の任意の Windows API を呼び出すことができます。 それについては、「[Windows 用 Rust と windows クレート](rust-for-windows.md)」というトピックに、詳細とコード例が記載されています。

## <a name="related"></a>関連項目

* [Windows 用 Rust と windows クレート](rust-for-windows.md)
* [Windows Subsystem for Linux (WSL)](/windows/wsl/about)
* [Microsoft C++ Build Tools](https://visualstudio.microsoft.com/visual-cpp-build-tools/)
* [Microsoft Visual Studio](https://visualstudio.microsoft.com/downloads/)
* [Windows 用 Visual Studio Code](https://code.visualstudio.com)
* [rust-analyzer 拡張機能](https://marketplace.visualstudio.com/items?itemName=matklad.rust-analyzer)
* [CodeLLDB 拡張機能](https://marketplace.visualstudio.com/items?itemName=vadimcn.vscode-lldb)
* [C/C++ 拡張機能](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools)
