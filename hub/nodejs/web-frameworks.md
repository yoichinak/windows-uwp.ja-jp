---
title: Windows での node.js web フレームワークの概要
description: Windows での node.js web フレームワークの使用を開始するためのガイドです。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: NodeJS、node.js、windows 10、microsoft、learning NodeJS、windows 上のノード、wsl のノード、windows 上の linux 上のノード、windows 上のノードのインストール、windows 上のノードのインストール、windows 上の NodeJS を使用した開発、windows 上のノードのインストール、WSL へのノードのインストール、Windows 上の NodeJSLinux 用サブシステム
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: a3c1cd980884dc50107c05207665d0c1ef88938e
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/15/2019
ms.locfileid: "72314956"
---
# <a name="get-started-with-nodejs-web-frameworks-on-windows"></a>Windows での node.js web フレームワークの概要

Node.js、Nuxt、Gatsby など、Windows での node.js web フレームワークの使用を開始するための手順を説明したガイドです。

## <a name="prerequisites"></a>前提条件

このガイドでは、次のよう[な WSL 2 を使用して node.js 開発環境を設定](./setup-on-wsl2.md)する手順を既に完了していることを前提としています。

- Windows 10 Insider Preview ビルド18932以降をインストールします。
- Windows で WSL 2 機能を有効にします。
- Linux ディストリビューションをインストールします (この例では Ubuntu 18.04)。 これを確認するには、次のようにします。 `wsl lsb_release -a`
- Ubuntu 18.04 のディストリビューションが WSL 2 モードで実行されていることを確認します。 (WSL では、v1 モードと v2 モードの両方でディストリビューションを実行できます)。これを確認するには、PowerShell を開き、次のように入力します。 `wsl -l -v`
- PowerShell を使用して、Ubuntu 18.04 を既定のディストリビューションとして設定します。 `wsl -s ubuntu 18.04`

## <a name="get-started-with-nextjs"></a>次の .js を使ってみる

次の js は、サーバー側でレンダリングされる JavaScript アプリを、.js、node.js、Webpack、および Babel の反応に基づいて作成するためのフレームワークです。 これは基本的に、ベストプラクティスに注意して対応したプロジェクト定型です。これにより、構成をほとんど必要とせずに、簡単かつ一貫した方法で "ユニバーサル" web アプリを作成できます。 これらの "ユニバーサル" サーバーでレンダリングされる web アプリは、"isomorphic" とも呼ばれ、クライアントとサーバー間でコードが共有されることを意味します。

次の .js プロジェクトを作成するには、次のようにします。

1. WSL ターミナル (ie を開きます。Ubuntu 18.04)。

2. 新しいプロジェクトフォルダーを作成します。 `mkdir NextProjects` で、そのディレクトリを入力します。 `cd NextProjects` です。

3. 次の .js をインストールし、プロジェクトを作成します ("my-app" はアプリの呼び出しに使用するものに置き換えます): `npm create next-app my-next-app`。

4. パッケージがインストールされたら、新しいアプリフォルダーにディレクトリを変更し、`cd my-next-app` をクリックしてから `code .` を使用して VS Code で次の .js プロジェクトを開きます。 これにより、アプリ用に作成された次の .js フレームワークを確認することができます。 (VS Code ウィンドウの左下にある緑のタブに示されているように) アプリが WSL-リモート環境で開かれている VS Code ます。 つまり、Windows OS での編集に VS Code を使用している場合でも、Linux OS でアプリを実行しています。

    ![WSL-リモート拡張](../images/wsl-remote-extension.png)

5. 次の3つのコマンドがインストールされていることを確認する必要があります。

    - ホットリロード、ファイル監視、およびタスクの再実行を使用して開発インスタンスを実行する場合は `npm run dev`。
    - プロジェクトをコンパイルするには、`npm run build` を使用します。
    - 実稼働モードでアプリを起動する場合は、`npm start` を使用します。

    VS Code (**View > terminal**) に統合された wsl ターミナルを開きます。 ターミナルパスがプロジェクトディレクトリ (ie. `~/NextProjects/my-next-app$`) を指していることを確認します。 次に、次を使用して新しい .js アプリの開発インスタンスを実行してみてください: `npm run dev`

6. ローカル開発サーバーが起動し、プロジェクトページのビルドが完了すると、ターミナルの " [http://localhost:3000](http://localhost:3000)で正常にコンパイルされました" と表示されます。 この localhost リンクを選択すると、web ブラウザーで新しい次の .js アプリが開きます。

    ![Localhost で実行されている次の .js アプリ: 3000](../images/next-app.png)

7. VS Code エディターで `pages/index.js` ファイルを開きます。 ページタイトル `<h1 className='title'>Welcome to Next.js!</h1>` を検索し、`<h1 className='title'>This is my new Next.js app!</h1>` に変更します。 Web ブラウザーで localhost: 3000 がまだ開いている状態で、変更を保存します。ホットリロード機能によって自動的にコンパイルされ、ブラウザーで変更が更新されます。

8. 次に、次の js がエラーを処理する方法を見てみましょう。 タイトルコードが次のようになるように、`</h1>` の終了タグを削除します。 `<h1 className='title'>This is my new Next.js app!` のようになります。 この変更を保存すると、ブラウザーに "コンパイルできませんでした" というエラーが表示され、ターミナルには `<h1>` の終了タグが必要であることがわかります。 @No__t 0 の終了タグを置き換えて保存し、ページを再読み込みします。

F5 キーを押すか、メニューバーの **[表示 > デバッグ]** (Ctrl + Shift + D) と**表示 > デバッグコンソール**(Ctrl + shift + Y) に移動して、次の .js アプリで VS Code のデバッガーを使用できます。 デバッグウィンドウで歯車アイコンを選択すると、デバッグセットアップの詳細を保存するための起動構成 (`launch.json`) ファイルが作成されます。 詳細については、「 [VS Code デバッグ](https://code.visualstudio.com/docs/editor/debugging#_launch-configurations)」を参照してください。

![VS Code デバッグウィンドウと起動. json 構成アイコン](../images/vscode-debug-launch-configuration.png)

次の .js の詳細については、[次の .js ドキュメント](https://nextjs.org/docs)を参照してください。

## <a name="get-started-with-nuxtjs"></a>Nuxt を使ってみる

Nuxt は、サーバー側でレンダリングされる JavaScript アプリを作成するためのフレームワークであり、Vue、node.js、Webpack、および Babel に基づいています。 これは次の .js によって行われました。 これは基本的に Vue のプロジェクト定型です。 次の .js と同じように、ベストプラクティスに注意して作成され、"ユニバーサル" な web アプリを簡単かつ一貫した方法で作成することができます。ほとんどの構成は必要ありません。 これらの "ユニバーサル" サーバーでレンダリングされる web アプリは、"isomorphic" とも呼ばれ、クライアントとサーバー間でコードが共有されることを意味します。

Nuxt プロジェクトを作成するには、インストールする統合サーバー側フレームワーク、UI フレームワーク、テストフレームワーク、モード、モジュール、およびリンターの種類に関する一連の質問への回答が含まれます。

1. WSL ターミナル (ie を開きます。Ubuntu 18.04)。

2. 新しいプロジェクトフォルダーを作成します。 `mkdir NuxtProjects` で、そのディレクトリを入力します。 `cd NuxtProjects` です。

3. Nuxt をインストールし、プロジェクトを作成します (' Nuxt ' は、アプリの呼び出しに使用するものに置き換えてください)。 `npm create nuxt-app my-nuxt-app`

4. Nuxt インストーラーでは、次の事項を確認するようになります。
    - プロジェクト名: nuxtjs-app
    - プロジェクトの説明:Nuxt アプリの説明。
    - 作成者名:GitHub のエイリアスを使用します。
    - パッケージマネージャーを選択します。Yarn または**npm** -例には npm を使用します。
    - UI フレームワークの選択:None、Ant Design、Bootstrap Vue など。 この例では**Vuetify**を選択してみましょう。 Vue コミュニティでは、[これらの UI フレームワークを比較](https://vue-community.org/guide/ecosystem/ui-libraries.html#summary-tldr)して、プロジェクトに最適なものを選択するのに役立つ概要を作成しました。
    - [カスタムサーバーフレームワーク] を選択します。None、AdonisJs、Express、Fastify など。 この例では **[なし]** を選択してみましょう。ただし、Dev.to サイトでは、 [2019-2020 server framework の比較](https://dev.to/santypk4/introducing-the-best-10-node-js-frameworks-for-2019-and-2020-mcm)を参照できます。
    - Nuxt モジュールを選択します ([space] を使用してモジュールを選択するか、不要な場合は入力します)。Axios (HTTP 要求を簡易化するため) または[PWA サポート](https://pwa.nuxtjs.org/)(サービスワーカー、manifest ファイルなどの追加用)。 この例のモジュールを追加しないでください。
    - 線ツールの選択:いい**ね、そして、そして**、そして、そして、糸のようにしています。 **[Eslint]** (コードを分析するためのツールであり、エラーの可能性があることを警告します) を選択してみましょう。
    - テストフレームワークを選択してください:**None**、JEST、ava。 このクイックスタートのテストについては説明しませんので、 **[なし]** を選択してください。
    - 表示モードの選択:**ユニバーサル (SSR)** またはシングルページアプリ (SPA)。 この例では**Universal (ssr)** を選択しますが、 [Nuxt のドキュメント](https://nuxtjs.org/guide#server-rendered-universal-ssr-)では、いくつかの相違点を指摘しています。これは、サーバーで実行されている node.js サーバーで、静的ホスティングのためにアプリと SPA をレンダリングする必要があるためです。
    - [開発ツール: jsconfig] を選択します (VS Code に推奨さ**れます。** Intellisense コード補完機能が動作します)。

5. プロジェクトが作成されたら、Nuxt プロジェクトディレクトリを入力するために-0 を @no__t、`code .` を入力して、VS Code WSL-リモート環境でプロジェクトを開きます。

    ![WSL-リモート拡張](../images/wsl-remote-extension.png)

6. Nuxt がインストールされると、次の3つのコマンドを知る必要があります。

    - ホットリロード、ファイル監視、およびタスクの再実行を使用して開発インスタンスを実行する場合は `npm run dev`。
    - プロジェクトをコンパイルするには、`npm run build` を使用します。
    - 実稼働モードでアプリを起動する場合は、`npm start` を使用します。

    VS Code (**View > terminal**) に統合された wsl ターミナルを開きます。 ターミナルパスがプロジェクトディレクトリ (ie. `~/NuxtProjects/my-nuxt-app$`) を指していることを確認します。 次に、を使用して、新しい Nuxt アプリの開発インスタンスを実行してみます。 `npm run dev`

6. ローカル開発サーバーが起動します (クライアントとサーバーがコンパイルするためのクールプログレスバーが表示されます)。 プロジェクトのビルドが完了すると、ターミナルに "コンパイルに成功しました" と、コンパイルに要した時間が表示されます。 Web ブラウザー[で http://localhost:3000](http://localhost:3000)をポイントして、新しい Nuxt アプリを開きます。

    ![Localhost で実行されている Nuxt アプリ: 3000](../images/nuxt-app.png)

7. VS Code エディターで `pages/index.vue` ファイルを開きます。 ページタイトル `<v-card-title class="headline">Welcome to the Vuetify + Nuxt.js template</v-card-title>` を検索し、`<v-card-title class="headline">This is my new Nuxt.js app!</v-card-title>` に変更します。 Web ブラウザーで localhost: 3000 がまだ開いている状態で、変更を保存します。ホットリロード機能によって自動的にコンパイルされ、ブラウザーで変更が更新されます。

8. Nuxt がエラーを処理する方法を見てみましょう。 タイトルコードが次のようになるように、`</v-card-title>` の終了タグを削除します。 `<v-card-title class="headline">This is my new Nuxt.js app!` のようになります。 この変更を保存すると、コンパイルエラーがブラウザーに表示され、ターミナルでは、`<v-card-title>` の終了タグがコード内で検出された行番号と共に表示されることを確認できます。 @No__t 0 の終了タグを置き換えて保存し、ページを再読み込みします。

F5 キーを押すか、メニューバーの **[表示 > デバッグ]** (Ctrl + Shift + D) と**表示 > デバッグコンソール**(Ctrl + shift + Y) に移動して、VS Code のデバッガーを Nuxt アプリで使用できます。 デバッグウィンドウで歯車アイコンを選択すると、デバッグセットアップの詳細を保存するための起動構成 (`launch.json`) ファイルが作成されます。 詳細については、「 [VS Code デバッグ](https://code.visualstudio.com/docs/editor/debugging#_launch-configurations)」を参照してください。

![VS Code デバッグウィンドウと起動. json 構成アイコン](../images/vscode-debug-launch-configuration.png)

Nuxt の詳細については、 [Nuxt ガイド](https://nuxtjs.org/guide)を参照してください。

## <a name="get-started-with-gatsbyjs"></a>Gatsby を使ってみる

Gatsby は、次の .js や Nuxt などのサーバーでレンダリングされるのではなく、応答の .js に基づく静的サイトジェネレーターフレームワークです。 静的サイトジェネレーターは、ビルド時に静的 HTML を生成します。 サーバーは必要ありません。 次の .js と Nuxt は、(新しい要求が送信されるたびに) 実行時に HTML を生成します。 サーバーを実行する必要があります。 また、Gatsby では、(GraphQL を使用して) アプリ内のデータを処理する方法も決定されます。一方、次の .js と Nuxt では、この決定はユーザーに任せます。

Gatsby プロジェクトを作成するには、次のようにします。

1. WSL ターミナル (ie を開きます。Ubuntu 18.04)。
2. 新しいプロジェクトフォルダーを作成します。 `mkdir GatsbyProjects` で、そのディレクトリを入力します。 `cd GatsbyProjects`
3. Npm を使用して Gatsby CLI をインストールします。 `npm install -g gatsby-cli`。 インストールが完了したら、`gatsby --version` を使用してバージョンを確認します。
4. Gatsby プロジェクトを作成します。 `gatsby new my-gatsby-app`
5. パッケージがインストールされたら、新しいアプリフォルダーにディレクトリを変更し、`cd my-gatsby-app` をクリックし、`code .` を使用して、VS Code で Gatsby プロジェクトを開きます。 これにより、VS Code のエクスプローラーを使用して、アプリ用に作成された Gatsby フレームワークを確認することができます。 (VS Code ウィンドウの左下にある緑のタブに示されているように) アプリが WSL-リモート環境で開かれている VS Code ます。 つまり、Windows OS での編集に VS Code を使用している場合でも、Linux OS でアプリを実行しています。

    ![WSL-リモート拡張](../images/wsl-remote-extension.png)

6. Gatsby をインストールすると、次の3つのコマンドを知る必要があります。

    - `gatsby develop`。ホットリロードを使用して開発インスタンスを実行します。
    - 実稼働ビルドを作成する場合は `gatsby build`。
    - 実稼働モードでアプリを起動する場合は、`gatsby serve` を使用します。

    VS Code (**View > terminal**) に統合された wsl ターミナルを開きます。 ターミナルパスがプロジェクトディレクトリ (ie. `~/GatsbyProjects/my-gatsby-app$`) を指していることを確認します。 次に、を使用して、新しいアプリの開発インスタンスを実行してみます。 `gatsby develop`

7. 新しい Gatsby プロジェクトのコンパイルが完了すると、ターミナルに "ブラウザーで Gatsby を表示できるようになりました。 [http://localhost:8000/](http://localhost:8000/). " Web ブラウザーでビルドされた新しいプロジェクトを表示するには、この localhost リンクを選択します。

> [!NOTE]
> ターミナル出力から、ブラウザー内 IDE で GraphiQL を表示して、サイトのデータとスキーマを調べることができることもわかります。 [http://localhost:8000/___graphql](http://localhost:8000/___graphql)です。 GraphQL は、Gatsby に組み込まれている自己ドキュメント型の IDE (GraphiQL) に Api を統合します。 サイトのデータとスキーマを探索するだけでなく、クエリ、変更、サブスクリプションなどの GraphQL 操作を実行できます。 詳細については、「 [GraphiQL の概要](https://www.gatsbyjs.org/docs/running-queries-with-graphiql/)」を参照してください。

8. VS Code エディターで `src/pages/index.js` ファイルを開きます。 ページタイトル `<h1 >Hi people</h1>` を検索し、`<h1 >Hi (Your Name)!</h1>` に変更します。 Web ブラウザーで localhost: 8000 が開いたままになっている状態で、変更を保存します。ホットリロード機能によって自動的にコンパイルされ、ブラウザーで変更が更新されます。

    ![Localhost で実行されている Gatsby アプリ: 3000](../images/gatsby-app.png)

9. 次に、次の js がエラーを処理する方法を見てみましょう。 タイトルコードが次のようになるように、`</h1>` の終了タグを削除します。 `<h1>Hi (Your Name)!` のようになります。 この変更を保存すると、ブラウザーに "コンパイルできませんでした" というエラーが表示され、ターミナルには `<h1>` の終了タグが必要であることがわかります。 @No__t 0 の終了タグを置き換えて保存し、ページを再読み込みします。

F5 キーを押すか、メニューバーの **[表示 > デバッグ]** (Ctrl + Shift + D) と**表示 > デバッグコンソール**(Ctrl + shift + Y) に移動して、次の .js アプリで VS Code のデバッガーを使用できます。 デバッグウィンドウで歯車アイコンを選択すると、デバッグセットアップの詳細を保存するための起動構成 (`launch.json`) ファイルが作成されます。 詳細については、「 [VS Code デバッグ](https://code.visualstudio.com/docs/editor/debugging#_launch-configurations)」を参照してください。

![VS Code デバッグウィンドウと起動. json 構成アイコン](../images/vscode-debug-launch-configuration.png)

Gatsby の詳細については、 [Gatsby のドキュメント](https://www.gatsbyjs.org/docs/)を参照してください。
