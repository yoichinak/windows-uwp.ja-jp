---
title: Windows で NodeJS を使ってみる (初心者向け)
description: 初心者が Windows で Node.js 開発を始めるときに役立つガイド。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: NodeJS、Node.js、windows 10、microsoft、nodejs 学習、windows のノード、windows のノード (初心者向け)、windows でノード開発、windows で nodejs 開発
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: 433eb5701696f590f10d8b3276481098b9ec073d
ms.sourcegitcommit: 8f9cea69f33b06166fec22677eaa43466352c14d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/04/2020
ms.locfileid: "75657084"
---
# <a name="get-started-using-nodejs-on-windows-for-beginners"></a>Windows で Node.js を使ってみる (初心者向け)

Node.js を使用するのがまったく初めての場合、いくつかの基本事項から始めるとき、このガイドが役に立ちます。

## <a name="prerequisites"></a>前提条件

このガイドでは、読者が [ネイティブ Windows で Node.js 開発環境を設定する](./setup-on-windows.md)手順を既に完了していることを前提にしています。これには以下が含まれます。

- Node.js バージョン マネージャーをインストールします。
- Visual Studio Code をインストールします。

Windows に Node.js を直接インストールすることは、最小限の設定で基本的な Node.js 操作を始めるための最も簡単な方法です。

Node.js を使用し、実稼働用のアプリケーションを開発する (通常、Linux サーバーに展開する作業を含む) 準備ができたら、[WSL2 で Node.js 開発環境を設定する](./setup-on-wsl2.md)ことをお勧めします。 Windows サーバーに Web アプリを配置することもできますが、[Linux サーバーを利用して Node.js アプリをホストする](https://azure.microsoft.com/develop/nodejs/)方が一般的です。

## <a name="types-of-nodejs-applications"></a>Node.js アプリケーションの種類

Node.js は、Web アプリケーションの作成に主に使用される JavaScript ランタイムです。 別の言い方をすると、アプリケーションのバックエンドを記述するために使用される JavaScript をサーバー側で実装することになります。 (ただし、多くの Node.js フレームワークでは、フロントエンドも処理できます)Node.js で作成するものの例をいくつか挙げます。

- **単一ページのアプリ (SPA)** :Web アプリの中にはブラウザーの中で動作し、新しいデータを取得する目的でページを使用するたびにページを再読み込みする必要がないものがあります。 SPA の例としては、ソーシャル ネットワーク アプリ、メール アプリ、地図アプリ、オンライン テキスト、描画ツールなどがあります。
- **リアルタイム アプリ (RTA)** :Web アプリの中には、更新がないかユーザー (またはソフトウェア) がソースを定期的に確認しなくても、作成者が公開した直後にユーザーが情報を受け取れるものがあります。 RTA の例としては、インスタント メッセージング アプリ、チャット ルーム、ブラウザーでプレイできるオンライン マルチプレイヤー ゲーム、オンライン コラボレーション ドキュメント、コミュニティ ストレージ、ビデオ会議アプリなどがあります。
- **データ ストリーミング アプリ**:アプリ (またはサービス) の中には、到着した (または作成された) データまたはコンテンツを送信しながら接続を維持し、必要に応じてさらなるデータ、コンテンツ、コンポーネントを引き続きダウンロードするものがあります。 例としては、動画ストリーム配信アプリや音声ストリーム配信アプリなどがあります。
- **REST API**:このインターフェイスは、誰かの Web アプリでやりとりするためのデータを提供します。 たとえば、Calendar API サービスから、誰かのローカル イベント Web サイトで使用されうるコンサート会場の日時が提供されることがあります。
- **サーバー側でレンダリングされるアプリ (SSR)** :この Web アプリは、クライアント (ブラウザーまたはフロントエンド) とサーバー (バックエンド) の両方で実行できて、動的なページはコンテンツが既知であればそれを表示し (HTML を生成し)、既知ではないコンテンツはそれが利用可能になった瞬間に取得できます。 "isomorphic" または "universal" アプリケーションと呼ばれることがあります。 SSR では SPA メソッドが活用されます。このメソッドでは、使用するたびに再読み込みする必要がありません。 ただし、SSR には、ユーザーにとって重要かどうかわからない長所がいくつかあります。たとえば、サイトのコンテンツを Google の検索結果に表示することや、アプリのリンクが Twitter や Facebook などのソーシャル メディアに投稿されたとき、プレビュー画像を提供することなどです。 潜在的な欠点は、Node.js サーバーを常に実行する必要があることです。 たとえば、ユーザーが検索結果に表示させたいイベントやソーシャルメディアに対応しているソーシャル ネットワーキング アプリは SSR の利点を活用できるかもしれませんが、メール アプリは SPA でも問題ないでしょう。 SPA ではないがサーバー側でレンダリングするアプリを実行することもできます。これはたとえば、WordPress ブログのようなものです。 ご覧のとおり、複雑になる可能性があるため、何が重要なのかを判断する必要があります。
- **コマンド ライン ツール**:繰り返し作業を自動化し、広範囲の Node.js エコシステム全体にツールを配布できます。 コマンド ライン ツールの例としては cURL があります。これはクライアント URL という意味で、インターネット URL からコンテンツをダウンロードする目的で使用されます。 cURL は多くの場合、Node.js などをインストールするために使用されます。今回のような Node.js バージョン マネージャーもあります。
- **ハードウェア プログラミング**:Web アプリほどの人気はありませんが、Node.js は、センサー、ビーコン、トランスミッター、モーター、あるいは大量のデータを生成する何かからデータを収集するなど、IoT 用途で人気が上昇しています。 Node.js ではデータを収集し、そのデータを分析し、デバイスとサーバーの間で通信をやりとりし、分析に基づいて措置をとることができます。 NPM には、Arduino コントローラー、raspberry pi、Intel IoT Edison、さまざまなセンサー、Bluetooth デバイスのためのパッケージが 80 以上含まれています。

## <a name="try-using-nodejs-in-vs-code"></a>VS Code で Node.js を使用してみる

1. コマンド ライン (コマンド プロンプト、PowerShell、あるいは自分がよいと思うツール) を起動し、新しいディレクトリを作成し (`mkdir HelloNode`)、そのディレクトリに入ります (`cd HelloNode`)

2. "app.js" という名前の JavaScript ファイルを作成し、"msg" という名前の変数を中に入れます (`echo var msg > app.js`)。

3. VS Code でディレクトリと app.js ファイルを開きます (`code .`)。

4. 簡単な文字列変数 ("Hello World") を追加し、"app.js" ファイルに次を入力し、コンソールに文字列コンテンツを送信します。

    ```js
    var msg = 'Hello World';
    console.log(msg);
    ```

5. Node.js で "app.js" ファイルを実行するには、 **[表示]** から **[ターミナル]** を選択し (あるいは Ctrl を押しながらバックティック文字 ` を選択し)、VS Code 内でターミナルを起動します。 既定のターミナルを変更する必要がある場合、ドロップダウン メニューを選択し、 **[既定のシェルの選択]** を選択します。

6. ターミナルで「`node app.js`」と入力します。 次のように出力されるはずです。"Hello World"。

> [!NOTE]
> "app.js" ファイルに「`console`」と入力すると、[`console`](https://developer.mozilla.org/docs/Web/API/Console) オブジェクト関連でサポートされているオプションが VS Code に表示されることに注目してください。このオプションは IntelliSense 使用時に選択できます。 他の [JavaScript オブジェクト](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects)を使用し、Intellisense をいろいろ試してみてください。

> [!TIP]
> 複数のコマンド ライン (Ubuntu、PowerShell、Windows コマンド プロンプトなど) を使用する予定がある場合や、テキスト、背景色、キー バインド、複数のウィンドウ ペインなど、[ターミナルをカスタマイズする](https://github.com/microsoft/terminal/blob/master/doc/user-docs/UsingJsonSettings.md)場合は、新しい [Windows ターミナル](https://github.com/microsoft/terminal/blob/master/doc/user-docs/index.md)を試してみてください。

## <a name="set-up-a-basic-web-app-framework-by-using-express"></a>Express を使用し、基本的な Web アプリ フレームワークを設定する

Express は最小限で柔軟かつ合理化された Node.js フレームワークであり、GET、PUT、POST、DELETE など、さまざまな種類の要求を処理できる Web アプリを簡単に開発できます。 Express には、アプリのファイル アーキテクチャを自動的に作成するアプリケーション ジェネレーターが付属しています。

Express.js でプロジェクトを作成する方法:

1. コマンド ライン (コマンドプロンプト、PowerShell、あるいは自分がよいと思うツール) を開きます。
2. 新しいプロジェクト フォルダーを作成し (`mkdir ExpressProjects`)、そのディレクトリに移動します (`cd ExpressProjects`)。
3. Express を使用して HelloWorld プロジェクトテンプレートを作成します (`npx express-generator HelloWorld --view=pug`)。

>[!NOTE]
> 今回は `npx` コマンドを使用し、Express.js Node パッケージを実際にインストールすることなく実行しています (一時的にインストールしていると考えることもできます)。 `express` コマンドを使用するか、インストールされている Express のバージョンを確認してみると (`express --version`)、Express が見つからないという応答が届きます。 Express をグローバルにインストールして繰り返し使用する場合は、`npm install -g express-generator` を使用します。 `npm list` を使用すれば、インストールされているパッケージの一覧を表示できます。 深さ (奥の方に入れ子になっているディレクトリの数) 別に一覧表示されます。 自分でインストールしたパッケージの深度は 0 になります。 そのパッケージに依存するものの深度が 1 で、それにさらに依存するものの深度が 2 というふうに続きます。 詳細については、Stackoverflow の「[Difference between npx and npm?](https://stackoverflow.com/questions/50605219/difference-between-npx-and-npm)」 (npx と npm の違いとは?) を参照してください。

4. VS Code でプロジェクトを開き、Express で追加されたファイルとフォルダーを調べます (`code .`)。

   Express によって生成されたファイルからは Web アプリが生成されますが、そのアプリで使用されるアーキテクチャは、初めは少し圧倒的に見えるかもしれません。 次のファイルとフォルダーが生成されたことが VS Code **Explorer** ウィンドウでわかります (Ctrl + Shift + E で表示)。

   - `bin` の順にクリックします。 アプリを起動する実行可能ファイルが含まれます。 (代替が指定されていない場合、ポート 3000 で) サーバーが起動し、基本的なエラー処理がセットアップされます。 
   - `public` の順にクリックします。 JavaScript ファイル、CSS スタイルシート、フォント ファイル、画像、人が Web サイトに接続するときに必要となるその他のアセットなど、誰でもアクセスできるファイルがすべて含まれます。
   - `routes` の順にクリックします。 アプリケーションのルート ハンドラーがすべて含まれます。 このフォルダーには `index.js` と `users.js` という 2 つのファイルが自動生成され、アプリケーションのルート構成を区別する方法の例として役立ちます。
   - `views` の順にクリックします。 テンプレート エンジンによって使用されたファイルが含まれます。 Express は、レンダー メソッドが呼び出されたとき、一致するビューを探すように構成されています。 既定のテンプレート エンジンは Jade ですが、Jade は Pug を優先するために非推奨となっています。そのため、`--view` フラグを使用し、ビュー (テンプレート) エンジンを変更しました。 `express --help` を使用すると、`--view` フラグ オプションやその他が表示されます。
   - `app.js` の順にクリックします。 アプリの開始点。 すべてを読み込み、ユーザーの要求に応え始めます。 これは基本的にすべて部品をくっつける糊です。
   - `package.json` の順にクリックします。 プロジェクトの説明、スクリプト マネージャー、アプリ マニフェストが含まれます。 その主たる目的は、アプリの依存関係とそのそれぞれのバージョンを追跡記録することです。

5. 次に、HelloWorld Express アプリを構築して実行する目的で Express で使用される依存関係をインストールする必要があります (`package.json` ファイルに定義されているとおり、サーバーの実行などのタスクに使用されるパッケージ)。 VS Code 内で、 **[表示]** から **[ターミナル]** を選択し (あるいは Ctrl を押しながらバックティック文字 ` を選択し)、"HelloWorld" プロジェクト ディレクトリにまだいることを確認します。 次を使用して Express パッケージの依存関係をインストールします。

```bash
npm install
```

6. この時点で、API と HTTP の多様なユーティリティ メソッドとミドルウェアを利用できる、複数のページからなる Web アプリ用のフレームワークが設定されます。堅牢な API を簡単に作成できます。 次を入力し、仮想サーバー上で Express アプリを起動します。

```bash
npx cross-env DEBUG=HelloWorld:* npm start
```

> [!TIP]
> 上のコマンドの `DEBUG=myapp:*` 部分は、デバッグ目的でログをオンにすることを Node.js に伝えています。 "myapp" は必ず自分のアプリの名前に置換してください。 アプリ名は `package.json` ファイルの "name" プロパティの下で見つかります。 `npx cross-env` を使用すると、あらゆるターミナルで `DEBUG` 環境変数が設定されますが、お使いのターミナル固有の方法でも設定できます。 `npm start` コマンドでは、`package.json` ファイルでスクリプトを実行するように npm に指示します。

7. これで Web ブラウザーを起動し、**localhost:3000** に移動することで実行中のアプリを表示できます。

   ![ブラウザーで実行されている Express アプリのスクリーンショット](../images/express-app.png)

8. これで HelloWorld Express アプリがブラウザーでローカル実行されたので、プロジェクト ディレクトリで "views" フォルダーを開き、"index.pug" ファイルを選択し、変更を加えてみてください。 開いたら、`h1= title` を `h1= "Hello World!"` に変更し、 **[保存]** (Ctrl + S) を選択します。 Web ブラウザーで **localhost:3000** URL を更新し、変更内容を表示します。

9. Express アプリの実行を停止するには、ターミナルに **Ctrl + C** を入力します。

## <a name="try-using-a-nodejs-module"></a>Node.js モジュールを試す

Node.js には、サーバー側 Web アプリの開発に役立つツールがあります。組み込み済みのツールもあれば、npm から入手できるさまざまなツールもあります。 これらのモジュールはさまざまなタスクで役立ちます。

|ツール               |使用目的                                                                                                  |
|:----------------- |:---------------------------------------------------------------------------------------------------------|
|gm、sharp          |編集、サイズ変更、圧縮など、JavaScript コードで直接、画像を操作する |
|PDFKit             |PDF 生成                                                                                            |
|validator.js       |文字列検証                                                                                         |
|imagemin、UglifyJS2|縮小                                                                                              |
|spritesmith        |スプライト シート生成                                                                                   |
|winston            |ログ記録                                                                                                  |
|commander.js       |コマンド ライン アプリケーションの作成                                                                       |

組み込み OS モジュールを利用し、お使いのコンピューターのオペレーティング システムに関する情報を取得してみましょう。

1) コマンド ラインで、Node.js CLI を開きます。 `node` の入力後、`>` プロンプトが表示され、Node.js を使用していることがわかります。

2) 現在使用しているオペレーティング システムを特定するには (応答を返し、Windows に入っていることを知らせなければなりません)、`os.platform()` を入力します。

3) CPU のアーキテクチャを確認するには、`os.arch()` を入力します。

4) システムで使用可能な CPU を表示するには、`os.cpus()` を入力します。

5) `.exit` を入力するか、Ctrl + C を 2 回押して、Node.js CLI を終了します。

   > [!TIP]
   > Node.js OS モジュールを使用してプラットフォームを確認したり、プラットフォーム固有の変数を返したりすることができます(Windows 開発の Win32/.bat や Mac/Unix、Linux、SunOS の darwin/.sh など) を返したりすることができます (たとえば、`var isWin = process.platform === "win32";`)。

## <a name="next-steps"></a>次の手順

このガイドでは、Node.js でできる基本的なことをいくつか学習し、VS Code で Node.js コマンド ラインの使用を試し、Express.js で簡単な Web アプリを作成してそれを Web ブラウザーでローカル実行し、いくつかの組み込み Node.js モジュールの使用を試しました。 いくつかの人気のある Node.js Web フレームワークをインストールして使用する方法の詳細については、次のガイドに進んでください。Next.js (React を基盤とし、サーバー側でレンダリングする Web フレームワーク)、Nuxt.js (Vue を基盤とし、サーバー側でレンダリングする Web フレームワーク)、Gatsby (React を基盤とし、静的にレンダリングする Web フレームワーク) について取り上げています。 これを飛ばして、MongoDB、PostgreSQL データベース、または Docker コンテナーを使用する方法を学習することもできます。

- [Windows での Node.js Web フレームワークの概要](./web-frameworks.md)
- [Node.js アプリのデータベースへの接続の概要](./databases.md)
- [Node.js で Docker コンテナーを使ってみる](./containers.md)
