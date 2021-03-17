---
title: RSS リーダー チュートリアル (VS Code を使用した Windows 用 Rust)
description: RSS フィードからブログ投稿のタイトルをダウンロードする簡単なアプリを作成するチュートリアル。
author: stevewhims
ms.author: stwhi
manager: jken
ms.topic: article
keywords: rust、windows 10、microsoft、rust の学習、windows での rust (初心者向け)、vs code を使用した rust、windows 用の rust
ms.localizationpriority: medium
ms.date: 03/04/2021
ms.openlocfilehash: b55faf6d44395989cb7eec39fb9cdd1ee7128aa7
ms.sourcegitcommit: bcdec8bda3106cd5588464531e582101d52dcc80
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/06/2021
ms.locfileid: "102254548"
---
# <a name="rss-reader-tutorial-rust-for-windows-with-vs-code"></a>RSS リーダー チュートリアル (VS Code を使用した Windows 用 Rust)

前のトピックでは、[Windows 用 Rust と windows クレート](rust-for-windows.md)について紹介しました。

ここでは、Really Simple Syndication (RSS) フィードからブログ投稿のタイトルをダウンロードする単純なアプリを記述することで、Windows 用 Rust を試してみましょう。

1. コマンド プロンプト (**VS 用の x64 Native Tools コマンド プロンプト** または任意の `cmd.exe`) を起動し、Rust プロジェクトを保持するフォルダーに `cd` で移動します。

2. 次に、Cargo を介して *rss_reader* という名前の新しい Rust プロジェクトを作成し、プロジェクトの新しく作成されたフォルダーに `cd` で移動します。

   ```console
   cargo new rss_reader
   cd rss_reader
   ```

3. ここで、再度 Cargo を介して、*bindings* という名前の新しいサブプロジェクトを作成します。 下のコマンドでわかるように、この新しいプロジェクトはライブラリであり、呼び出したい Windows API にバインドするときに使う手段として機能することになります。 ビルド時に、ライブラリのサブプロジェクト *bindings* は、"*クレート*" (Rust のバイナリまたはライブラリを表す用語) に組み込まれます。 後で見るように、*rss_reader* プロジェクト内からそのクレートを利用することになります。

   ```console
   cargo new --lib bindings
   ```

   *bindings* を入れ子になったクレートにすることは、*rss_reader* をビルドすると、インポートするすべてのバインドの結果を *bindings* でキャッシュできるようになることを意味します。

4. 次に、VS Code で *rss_reader* プロジェクトを開きます。

   ```console
   code .
   ```

5. まず、*bindings* ライブラリの作業を行いましょう。

   VS Code のエクスプローラーで、`bindings` > `Cargo.toml` ファイルを開きます。

   ![プロジェクト作成後に VS Code で開いた Cargo.toml ファイル](../../images/rust-rss-reader-1.png)

   `Cargo.toml` ファイルは、Rust プロジェクトを、そこにあるすべての依存関係を含めて記述しているテキスト ファイルです。

   現時点では、`dependencies` セクションは空白です。 そのため、ここでそのセクションを編集します (`[build-dependencies]` セクションも追加します)。そうすると、次のようになります。

   ```toml
   # bindings\Cargo.toml
   ...

   [dependencies]
   windows="0.3.1"

   [build-dependencies]
   windows="0.3.1"
   ```

   *bindings* ライブラリとビルド スクリプトの両方のために、*windows* クレートに依存関係を追加したところです。 それにより Cargo で、Windows サポートをパッケージとしてダウンロードし、ビルドしてキャッシュすることができます。 バージョン番号は、いくつであるかを問わず最新バージョンに設定します。これは、[windows クレート](https://crates.io/crates/windows)の Web ページで確認できます。

6. これで、ビルド スクリプト自体を追加できます。そこが、最終的に依存することになるバインドを生成する場所です。 VS Code で、 *[bindings]* フォルダーを右クリックし、 **[新しいファイル]** をクリックします。 「*build.rs*」という名前を入力し、**Enter** キーを押します。 `build.rs` を次のように編集します。

   ```rust
   // bindings\build.rs
   fn main() { 
       windows::build!(windows::web::syndication::SyndicationClient);
   }
   ```

   `windows::build!` マクロは、`.winmd` ファイルの形式となっているすべての依存関係を解決し、選択された型のバインディングをメタデータから直接生成する処理を行います。 名前空間全体を要求することも *できました* (`windows::web::syndication::SyndicationClient` ではなく `windows::web::syndication::*` を指定)。 しかしここでは、[**SyndicationClient**](/uwp/api/windows.web.syndication.syndicationclient) 型に対してのみバインドを生成するように要求しています。 このようにすると、必要なだけ少なくまたは多くインポートし、必要になることは決してないもののために、コードの生成とコンパイルが行われるのを待機せずにすみます。
   
   また、*build* マクロでは、明示的に示された型のすべての依存関係が自動的に見つけられます。 ここでは、**SyndicationClient** に、[**Uri**](/uwp/api/windows.foundation.uri) 型のパラメーターを必要とするメソッドが含まれています。 そのため *build* マクロには、そのメソッドを呼び出すことができるように **Uri** の定義が含まれています。 その他の型は、**windows** クレート自体の一部となっています。 たとえば **windows::Result** は、*windows* クレートによって定義されているので、常に使用することができます。 [**Windows.Foundation**](/uwp/api/windows.foundation) 名前空間の必要なもののほとんどは、自動的に含められることがわかります。

7. `bindings` > `src` > `lib.rs` ソース コード ファイルを開きます。 前の手順で生成されたバインドを含めるには、`lib.rs` にある既定のコードを、次と置き換えます。

   ```rust
   // bindings\src\lib.rs
   ::windows::include_bindings!();
   ```

   `windows::include_bindings!` マクロには、前の手順でビルド スクリプトによって生成されたソース コードが含まれています。 これで、追加の API にアクセスする必要がある場合はいつでも、それらをビルド スクリプト (`build.rs`) 内に列挙するだけで済みます。

8. ここで、メインの *rss_reader* プロジェクトを実装しましょう。 まず、プロジェクトのルートにある `Cargo.toml` ファイルを開き、内側の *bindings* クレートに次の依存関係を追加します。

   ```toml
   # Cargo.toml
   ...

   [dependencies] 
   bindings = { path = "bindings" }
   ```

9. 最後に、*rss_reader* プロジェクトの `src` > `main.rs` ソース コード ファイルを開きます。 そこには、*Hello, world!* メッセージを出力する簡単なコードがあります。 メッセージで応答します。 このコードを `main.rs` の先頭に追加します。

   ```rust
   // src\main.rs
   use bindings::{ 
       windows::foundation::Uri,
       windows::web::syndication::SyndicationClient,
       windows::Result,
   };

   fn main() {
       println!("Hello, world!");
   }
   ```

   `use` 宣言によって、使おうとしている型へのパスが短縮されます。 前に触れた **Uri** 型があります。 そして **windows::Result** は、エラーの伝達と簡潔なエラー処理を行う助けとなります。

10. 新しい [**Uri**](/uwp/api/windows.foundation.uri) を作成するには、このコードを **main** 関数に追加します。

   ```rust
   // src\main.rs
   ...

   fn main() -> Result<()> {
       let uri = Uri::create_uri("https://blogs.windows.com/feed")?;

       Ok(())
   }
   ```

   **main** 関数の戻り値の型として、**windows::Result** を使用していることに注意してください。 これによって物事がより容易になります。オペレーティング システム (OS) API からのエラーを処理するのが一般的であるためです。

   **Uri** を作成するコード行の末尾に、疑問符の演算子があることがわかります。 入力量を減らすため、それを行って、Rust のエラー伝達と短絡論理を利用しています。 これは、この簡単な例では、手動のエラー処理を多数行う必要がないことを意味します。 Rust のこの機能の詳細については、「[より簡単なエラー処理のための ? 演算子](https://doc.rust-lang.org/edition-guide/rust-2018/error-handling-and-panics/the-question-mark-operator-for-easier-error-handling.html)」を参照してください。

11. この RSS フィードをダウンロードするには、新しい **SyndicationClient** オブジェクトを作成します。

   ```rust
   // src\main.rs
   ...

   fn main() -> Result<()> {
       let uri = Uri::create_uri("https://blogs.windows.com/feed")?;
       let client = SyndicationClient::new()?;

       Ok(())
   }
   ```

   **new** 関数は、Rust の既定のコンストラクターに相当するものです。

12. これで、**SyndicationClient** オブジェクトを使用してフィードを取得できます。

   ```rust
   // src\main.rs
   ...

   fn main() -> Result<()> {
       let uri = Uri::create_uri("https://blogs.windows.com/feed")?;
       let client = SyndicationClient::new()?;
       let feed = client.retrieve_feed_async(uri)?.get()?;

       Ok(())
   }
   ```

[**RetrieveFeedAsync**](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync) は非同期 API であるため、ブロックする **get** 関数を (上に示すように) 使用できます。 または、C# や C++ で行うのと同様に (結果を協調的に待機するため) `async` 関数内で `await` 演算子を使用できます。

13. これで、結果として得られる項目を単純に反復処理できるので、タイトルだけを出力しましょう。

   ```rust
   // src\main.rs
   ...

   fn main() -> Result<()> {
       let uri = Uri::create_uri("https://blogs.windows.com/feed")?;
       let client = SyndicationClient::new()?;
       let feed = client.retrieve_feed_async(uri)?.get()?;

       for item in feed.items()? {
           println!("{}", item.title()?.text()?);
       }

       Ok(())
   }
   ```

14. ここでは、 **[実行]**  >  **[デバッグなしで実行]** をクリックして (または **Ctrl + F5** キーを押して)、ビルドと実行が可能であることを確認しましょう。 テキスト エディター内にも **[デバッグ]** と **[実行]** のコマンドが埋め込まれています。 または、コマンド プロンプトから `cargo run` コマンドを送信できます (最初に `cd` で `rss_reader` フォルダーに移動します)。このコマンドが、ビルドを行ってから実行します。

   ![テキスト エディター内に埋め込まれているデバッグと実行のコマンド](../../images/rust-rss-reader-2.png)

   **[ターミナル]** ペインの下方では、Cargo によって、**windows** クレートのダウンロードとコンパイルが正常に行われ、結果がキャッシュされて、それを使用して後続のビルドがより短い時間で完了したことを確認できます。 その後サンプルをビルドして実行し、ブログ投稿のタイトルの一覧を表示しています。

   ![ブログ投稿のタイトルの一覧](../../images/rust-rss-reader-3.png)

Windows のために Rust をプログラミングすることは、これほど簡単です。 ただし内部的には、Rust でコンパイル時に [ECMA-335](https://www.ecma-international.org/publications-and-standards/standards/ecma-335/) (共通言語基盤すなわち CLI) に基づく `.winmd` ファイルを解析すること、および安全性と効率性の両方を考慮に入れて実行時に COM ベースのアプリケーション バイナリ インターフェイス (ABI) を忠実に遵守することの両方が可能なように、ツールの構築に多くの労力が注がれています。

## <a name="related"></a>関連項目

* [Windows 用 Rust と windows クレート](rust-for-windows.md)
* [ECMA-335](https://www.ecma-international.org/publications-and-standards/standards/ecma-335/)
* [より簡単なエラー処理のための ? 演算子](https://doc.rust-lang.org/edition-guide/rust-2018/error-handling-and-panics/the-question-mark-operator-for-easier-error-handling.html)
