---
title: Windows 上の NodeJS を初心者向けに使い始める
description: Windows での node.js 開発を開始するのに役立つガイドです。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: NodeJS、node.js、windows 10、microsoft、learning NodeJS、windows 上のノード、初心者向け windows 上のノード、windows 上のノードを使用した開発、windows 上の NodeJS を使用した開発
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: 24a2ea5288a627ed884d549ca3b6f9c6930ce34e
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/15/2019
ms.locfileid: "72315136"
---
# <a name="get-started-using-nodejs-on-windows-for-beginners"></a>Windows 上の node.js を初心者向けに使い始める

Node.js を初めて使用する場合は、このガイドを参考にして基本的な作業を開始できます。

## <a name="prerequisites"></a>前提条件

このガイドでは、次のよう[な WSL 2 を使用して node.js 開発環境を設定](./setup-on-wsl2.md)する手順を既に完了していることを前提としています。

- Windows 10 Insider Preview ビルド18932以降をインストールします。
- Windows で WSL 2 機能を有効にします。
- Linux ディストリビューションをインストールします (この例では Ubuntu 18.04)。 これを確認するには、次のようにします。 `wsl lsb_release -a`
- Ubuntu 18.04 のディストリビューションが WSL 2 モードで実行されていることを確認します。 (WSL では、v1 モードと v2 モードの両方でディストリビューションを実行できます)。これを確認するには、PowerShell を開き、次のように入力します。 `wsl -l -v`
- 次のように、PowerShell を使用して Ubuntu 18.04 を既定のディストリビューションとして設定します。 `wsl -s ubuntu 18.04`

> [!NOTE]
> Windows Subsystem for Linux を使用するための追加のセットアップ手順がいくつかありますが、WSL 2 を使用して VS Code とリモート WSL 拡張機能を使用すると、最も滑らかな node.js 開発ワークフローが提供され、ツールの大部分との連携が実現します。記事、チュートリアル、[デプロイ環境](https://docs.microsoft.com/en-us/azure/javascript/tutorial-vscode-azure-app-service-node-01)。 ただし、Windows で node.js を使用することに努めている場合は、 [windows で直接 node.js 開発環境を設定](./setup-on-windows.md)するためのガイドを参照してください。 Windows server で node.js アプリをホストする必要がある (まれな) 状況では、最も一般的なシナリオは[リバースプロキシを使用して](https://medium.com/intrinsic/why-should-i-use-a-reverse-proxy-if-node-js-is-production-ready-5a079408b2ca)いるように見えます。 これには 2 つの方法があります。1) [iisnode](https://harveywilliams.net/blog/installing-iisnode)またはを[直接](https://dev.to/petereysermans/hosting-a-node-js-application-on-windows-with-iis-as-reverse-proxy-397b)使用します。 これらのリソースは維持されず、 [Linux サーバーを使用して node.js アプリをホストする](https://azure.microsoft.com/en-us/develop/nodejs/)ことをお勧めします。

## <a name="types-of-nodejs-applications"></a>Node.js アプリケーションの種類

Node.js は、主に web アプリケーションの作成に使用される JavaScript ランタイムです。 もう1つの方法として、アプリケーションのバックエンドを記述するために使用される JavaScript のサーバー側の実装があります。 (ただし、多くの node.js フレームワークでフロントエンドを処理することもできます)。Node.js で作成する可能性のあるいくつかの例を次に示します。

- **シングルページアプリ (spa)** :これらはブラウザー内で動作する web アプリであり、新しいデータを取得するために使用するたびにページを再読み込みする必要はありません。 例として、ソーシャルネットワーキングアプリ、電子メールまたはマップアプリ、オンラインテキストまたは描画ツールなどがあります。
- **リアルタイムアプリ (rta)** :これらは、ユーザーが更新プログラムのソースを定期的にチェックするのではなく、作成者によって発行された情報をすぐに受け取ることができる web アプリです。 Rta の例としては、インスタントメッセージングアプリやチャットルーム、ブラウザーで再生できるオンラインのマルチプレイヤーゲーム、オンラインコラボレーションドキュメント、コミュニティストレージ、ビデオ会議アプリなどがあります。
- **データストリーミングアプリ**:これらのアプリ (またはサービス) は、接続を開いたままにして、必要に応じてさらにデータ、コンテンツ、またはコンポーネントのダウンロードを続行しながら、データ/コンテンツを受信 (または作成) して送信します。 一部の例には、ビデオとオーディオストリーミングアプリが含まれています。
- **REST api**:これらは、他のユーザーの web アプリが対話するためのデータを提供するインターフェイスです。 たとえば、Calendar API サービスは、他のユーザーのローカルイベント web サイトで使用される可能性があるコンサート会場の日付と時刻を提供できます。
- **サーバー側でレンダリングされるアプリ (SSRs)** :これらの web アプリは、(ブラウザー/フロントエンド内の) クライアントとサーバー (バックエンド) の両方で実行できるため、動的なページを使用して、どのようなコンテンツが認識されていても、使用可能ではないコンテンツを迅速に取得することができます。 これらは、"isomorphic" または "universal" アプリケーションと呼ばれることがよくあります。 SSRs は、使用するたびに再読み込みを必要としないので、SPA のメソッドを利用します。 ただし、お客様にとって重要であるとは限りません。たとえば、サイトのコンテンツを Google search の結果に表示し、アプリへのリンクが Twitter や Facebook などのソーシャルメディアで共有されている場合にプレビューイメージを提供するなど、いくつかの利点があります。 潜在的な欠点は、常に node.js サーバーが実行されている必要があることです。 例として、ユーザーが検索結果やソーシャルメディアに表示するイベントをサポートするソーシャルネットワークアプリは SSR の恩恵を受ける可能性がありますが、電子メールアプリは SPA として問題なく動作する可能性があります。 サーバーでレンダリングされた非 SPA アプリを実行することもできます。これは WordPress ブログのようなものです。 ご覧のとおり、複雑になる可能性があります。重要なことを判断するだけで済みます。
- **コマンドラインツール**:これにより、繰り返し実行されるタスクを自動化し、大規模な node.js エコシステム全体にツールを配布することができます。 コマンドラインツールの例としては、cURL があります。これはクライアントの URL 用であり、インターネット URL からコンテンツをダウンロードするために使用されます。 cURL は多くの場合、node.js や node.js バージョンマネージャーなどをインストールするために使用されます。
- **ハードウェアプログラミング**:Web アプリほど広く普及しているわけではありませんが、node.js は、センサー、ビーコン、送信機、モーター、または大量のデータを生成するすべてのデータを収集するなど、IoT の使用に関してますます普及しています。 Node.js では、データ収集を有効にし、そのデータを分析し、デバイスとサーバー間の通信を行い、分析に基づいてアクションを実行することができます。 NPM には、Arduino コントローラー、raspberry pi、Intel IoT Edison、さまざまなセンサー、および Bluetooth デバイスの80を超えるパッケージが含まれています。

## <a name="try-using-nodejs-in-vs-code"></a>VS Code で node.js を使用してみてください

1. Ubuntu ターミナルを開き、新しいディレクトリを作成します。 `mkdir HelloNode` の場合は、次のディレクトリを入力します: `cd HelloNode`

2. "App.config" という名前の空の JavaScript ファイルを作成します。 `touch app.js`

3. VS Code:: @no__t でディレクトリと空のファイルを開きます。

4. App.config に単純な文字列変数を作成し、' app.config ' ファイルに次のように入力して、文字列の内容をコンソールに送信します。

    ```js
    var msg = 'Hello World';
    console.log(msg);
    ```

5. Node.js で "app.config" ファイルを実行します。 [ **View** > **ターミナル**] (または、バックティック文字を使用して Ctrl + ' を選択) を選択して、VS Code 内で Ubuntu ターミナルを開きます。 既定のターミナルを変更する必要がある場合は、ドロップダウンメニューを選択し、[**既定のシェルを選択**する] を選択します。 次に、VS Code で使用する既定のターミナルシェルとして **[Wsl]** を選択します。

6. ターミナルで、「`node app.js`」と入力します。 次のような出力が表示されます。"Hello World"。

> [!NOTE]
> リモート WSL 拡張機能が既にインストールされているため、VS Code ウィンドウの左下にある緑色のタブで示されているように、Ubuntu Linux システムで実行されているリモート環境でディレクトリが開きます。 また、' app.config ' ファイルに `console` と入力すると、IntelliSense を使用して選択できる[`console`](https://developer.mozilla.org/docs/Web/API/Console)オブジェクトに関連するサポートされるオプションが VS Code に表示されることに注意してください。 他の[JavaScript オブジェクト](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects)を使用して、Intellisense を試してみてください。

> [!TIP]
> 複数のコマンドライン (Ubuntu、PowerShell、Windows コマンドプロンプトなど) を使用する場合や、[ターミナルをカスタマイズ](https://github.com/microsoft/terminal/blob/master/doc/user-docs/UsingJsonSettings.md)する場合は (テキスト、背景色、キーバインドなど)、新しい[Windows ターミナル](https://github.com/microsoft/terminal/blob/master/doc/user-docs/index.md)を試すことを検討してください。

## <a name="set-up-a-basic-web-app-framework-by-using-express"></a>Express を使用して基本的な web アプリフレームワークを設定する

Express は、最小、柔軟、かつ合理化された node.js フレームワークであり、GET、PUT、POST、DELETE などの複数の種類の要求を処理できる web アプリの開発を容易にします。 Express には、アプリのファイルアーキテクチャを自動的に作成するアプリケーションジェネレーターが付属しています。

プロジェクトを作成するには、次のように記述します。

1. WSL ターミナル (Ubuntu 18.04) を開きます。
2. 新しいプロジェクトフォルダーを作成します。 `mkdir ExpressProjects` で、そのディレクトリを入力します。 `cd ExpressProjects` です。
3. Express を使用して HelloWorld プロジェクトテンプレートを作成します。 `npx express-generator HelloWorld --view=pug`。

>[!NOTE]
> ここでは `npx` のコマンドを使用して、実際にはインストールせずに (または、その考え方に応じて一時的にインストールして)、Express js ノードパッケージを実行しています。 @No__t-0 コマンドを使用しようとした場合、または: `express --version` を使用してインストールされている Express のバージョンを確認しようとすると、Express が見つからないという応答が返されます。 Express をグローバルにインストールして、繰り返し使用する場合は、を使用します。 `npm install -g express-generator` を使用します。 Npm によってインストールされたパッケージの一覧を表示するには、`npm list` を使用します。 これらは、深さ (入れ子になったディレクトリの数) で一覧表示されます。 インストールしたパッケージは、深度0になります。 そのパッケージの依存関係は、深さ1、さらに深い依存関係が深さ2になります。 詳細については、Stackoverflow での[npx と npx の違い](https://stackoverflow.com/questions/50605219/difference-between-npx-and-npm)に関するページを参照してください。

4. VS Code でプロジェクトを開き、次のようにを使用して、含まれているファイルとフォルダーを確認します。 `code .`

   によって生成されるファイルによって、最初に少し圧倒されるアーキテクチャを使用する web アプリが作成されます。 次のファイルとフォルダーが生成されたことが、VS Code**エクスプローラー**ウィンドウに表示されます (Ctrl + Shift + E を参照)。

   - `bin`。 アプリを起動する実行可能ファイルが含まれます。 (代替手段が提供されていない場合はポート3000で) サーバーを起動し、基本的なエラー処理を設定します。 
   - `public`。 JavaScript ファイル、CSS スタイルシート、フォントファイル、画像、その他のユーザーが web サイトに接続するときに必要なその他のアセットを含む、パブリックにアクセスされるすべてのファイルが含まれます。
   - `routes`。 アプリケーションのすべてのルートハンドラーを含みます。 このフォルダーには、@no__t 0 と `users.js` の2つのファイルが自動的に生成され、アプリケーションのルート構成を分離する方法の例として機能します。
   - `views`。 テンプレートエンジンによって使用されるファイルが含まれます。 Express は、render メソッドが呼び出されたときに、ここで一致するビューを検索するように構成されています。 既定のテンプレートエンジンは Jade ですが、Pug を優先するために Jade は非推奨とされているので、`--view` フラグを使用してビュー (テンプレート) エンジンを変更しました。 @No__t-1 を使用すると、`--view` フラグオプションを表示できます。
   - `app.js`。 アプリの開始点。 すべてを読み込み、ユーザー要求の提供を開始します。 これは基本的に、すべてのパーツをまとめて保持するグルーです。
   - `package.json`。 プロジェクトの説明、スクリプトマネージャー、およびアプリケーションマニフェストが含まれています。 主な目的は、アプリの依存関係とそのバージョンを追跡することです。

5. HelloWorld Express アプリをビルドして実行するために、で使用する依存関係をインストールする必要があります (`package.json` ファイルで定義されているように、サーバーの実行などのタスクに使用されるパッケージ)。 VS Code で、[ **View** > **ターミナル**] (または Ctrl + ' を選択してバックティック文字を使用) を選択して wsl ターミナルを開き、まだ ' HelloWorld ' プロジェクトディレクトリにあることを確認します。 次のものを使用して、Express パッケージの依存関係をインストールします。

```bash
npm install
```

6. この時点で、さまざまな Api や HTTP ユーティリティメソッドとミドルウェアにアクセスできるマルチページ web アプリ用のフレームワークが設定されているため、堅牢な API を簡単に作成できます。 次のように入力して、仮想サーバーで Express アプリを起動します。

```bash
DEBUG=HelloWorld:* npm start
```

> [!TIP]
> 上のコマンドの @no__t 0 部分は、デバッグのためにログ記録を有効にするように node.js に指示することを意味します。 ' Myapp ' は実際のアプリ名に置き換えてください。 アプリケーション名は、"name" プロパティの下のパッケージの json ファイルにあります。 @No__t-0 コマンドは、パッケージの json ファイルでスクリプトを実行するように npm に指示します。

7. Web ブラウザーを開き、 **localhost: 3000**に移動して、実行中のアプリを表示できるようになりました。

   ![ブラウザーで実行されている Express アプリのスクリーンショット](../images/express-app.png)

8. HelloWorld Express アプリがブラウザーでローカルに実行されるようになったので、プロジェクトディレクトリで ' views ' フォルダーを開き、"index. pug" ファイルを選択して、変更を行ってみてください。 開いたら、`h1= title` を `h1= "Hello World!"` に変更し、 **[保存]** (Ctrl + S) を選択します。 Web ブラウザーで**localhost: 3000** URL を更新して変更内容を表示します。

9. お使いのターミナルで Express アプリの実行を停止するには、次のように入力します。**Ctrl + C**

## <a name="try-using-a-nodejs-module"></a>Node.js モジュールを使用してみる

Node.js には、サーバー側の web アプリを開発するのに役立つツールが用意されています。また、npm を使用して、さまざまな機能を利用できます。 これらのモジュールは、多くのタスクに役立ちます。

|ツール               |使用目的                                                                                                  |
|:----------------- |:---------------------------------------------------------------------------------------------------------|
|gm、シャープ          |JavaScript コード内で直接、編集、サイズ変更、圧縮などを含むイメージ操作 |
|PDFKit             |PDF の生成                                                                                            |
|検証コントロール       |文字列の検証                                                                                         |
|imagemin, UglifyJS2|縮小                                                                                              |
|spritesmith        |スプライトシートの生成                                                                                   |
|winston            |ログの記録                                                                                                  |
|コマンダー       |コマンドラインアプリケーションの作成                                                                       |

組み込みの OS モジュールを使用して、お使いのコンピューターのオペレーティングシステムに関する情報を取得してみましょう。

1) WSL (Ubuntu 18.04) ターミナルで、node.js CLI を開きます。 次のように入力した後に node.js を使用していることを知らせる `>` プロンプトが表示されます: `node`

2) 現在使用しているオペレーティングシステム (Windows を使用していることを知らせる応答を返す) を特定するには、次のように入力します。 `os.platform()`

3) CPU のアーキテクチャを確認するには、次のように入力します。 `os.arch()`

4) システムで使用可能な Cpu を表示するには、次のように入力します。 `os.cpus()`

5) @No__t-0 を入力するか、Ctrl + C キーを2回押して、node.js CLI をそのまま使用します。

   > [!TIP]
   > Node.js OS モジュールを使用して、プラットフォームを確認し、プラットフォーム固有の変数を返すなどの操作を行うことができます。Windows 開発用の Win32/.bat、Mac/unix 用のダーウィン/sh、Linux、SunOS など (たとえば、`var isWin = process.platform === "win32";`)。

## <a name="next-steps"></a>次の手順

このガイドでは、node.js で実行できることについていくつかの基本的なことを学習しました。 VS Code で node.js コマンドラインを使用して、簡単な web アプリを作成し、web ブラウザーでローカルに実行して、組み込みの node.js モジュールのいくつかを試してみました。 いくつかの一般的な node.js web フレームワークをインストールして使用する方法の詳細については、次のガイドに進んでください。次のガイドでは、サーバーによってレンダリングされた、Nuxt (Vue ベースの web フレームワーク)、Gatsby (a) について説明しています。応答に基づいて静的にレンダリングされた web フレームワーク。 また、MongoDB または PostgreSQL データベースまたは Docker コンテナーを操作する方法についても説明しません。

- [Windows での node.js web フレームワークの概要](./web-frameworks.md)
- [データベースへの node.js アプリの接続の概要](./databases.md)
- [Node.js で Docker コンテナーを使ってみる](./containers.md)
