---
title: Windows での Node.js Web フレームワークの概要
description: Windows で Node.js Web フレームワークの使用を開始するために役立つガイド。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: NodeJS, Node.js, Windows 10, Microsoft, ラーニング NodeJS, Windows 上のノード, WSL 上のノード, Windows 上の Linux 上のノード, Windows 上のインストール ノード, NodeJS と VS Code, Windows 上のノードでの開発, Windows 上の NodeJS での開発, WSL 上のインストール ノード, Linux 用 Windows サブシステム上の NodeJS
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: a8ce1d08136a74504e1b3bad26feadd61b72068f
ms.sourcegitcommit: 60d2d15dd0d365f82e4e90e4bc34b40cf5b4a247
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/17/2019
ms.locfileid: "72517789"
---
# <a name="get-started-with-nodejs-web-frameworks-on-windows"></a>Windows での Node.js Web フレームワークの概要

Windows で Node.js Web フレームワーク (Next.js、Nuxt.js、Gatsby を含む) の使用を開始するために役立つステップ バイ ステップ ガイド。

## <a name="prerequisites"></a>前提条件

このガイドでは、読者が [WSL 2 を使用して Node.js 開発環境を設定する](./setup-on-wsl2.md)手順を既に完了していることを前提にしています。これには、次のような操作が含まれます。

- Windows 10 Insider Preview ビルド 18932 以降をインストールします。
- Windows で WSL 2 機能を有効にします。
- Linux ディストリビューション (この例では Ubuntu 18.04) をインストールします。 これは `wsl lsb_release -a` で確認できます。
- Ubuntu 18.04 ディストリビューションが WSL 2 モードで実行されていることを確認します。 (WSL は、v1 または v2 モードの両方でディストリビューションを実行できます。)これは PowerShell を開き、「`wsl -l -v`」と入力することによって確認できます。
- PowerShell を使用して、`wsl -s ubuntu 18.04` で、Ubuntu 18.04 を既定のディストリビューションとして設定します。

## <a name="get-started-with-nextjs"></a>Next.js の概要

Next.js は、React.js、Node.js、Webpack、Babel.js に基づいて、サーバー レンダリング JavaScript アプリを作成するためのフレームワークです。 これは基本的に、ベスト プラクティスに注意して作成された React 用のプロジェクト定型文であり、"ユニバーサル" Web アプリを簡単かつ一貫した方法で、構成をほとんど必要とせずに作成できるようにします。 これらの "ユニバーサル" サーバー レンダリング Web アプリは "isomorphic" とも呼ばれることがあります。つまり、コードがクライアントとサーバーの間で共有されます。

Next.js プロジェクトを作成するには (これには、next、react、react-dom のインストールが含まれます):

1. WSL ターミナルを開きます (Ubuntu 18.04)。

2. 新しいプロジェクト フォルダーを作成し (`mkdir NextProjects`)、そのディレクトリに移動します (`cd NextProjects`)。

3. Next.js をインストールし、プロジェクトを作成します ('my-next-app' をアプリに付ける名前に置き換えます) (`npm create next-app my-next-app`)。

4. パッケージがインストールされたら、新しいアプリ フォルダーにディレクトリを変更してから (`cd my-next-app`)、`code .` を使用して VS Code で Next.js プロジェクトを開きます。 これにより、アプリ用に作成された Next.js フレームワークを確認できます。 VS Code によって、アプリが WSL-Remote 環境で開かれたことに注意してください (VS Code ウィンドウの左下にある緑色のタブに示されています)。 これは、Windows OS で編集のために VS Code を使用している間、アプリが引き続き Linux OS 上で実行されていることを示します。

    ![WSL-Remote の拡張](../images/wsl-remote-extension.png)

5. Next.js がインストールされたら、次の 3 つのコマンドを覚えておく必要があります。

    - `npm run dev`。開発インスタンスをホット リロード、ファイル監視、タスク再実行で実行します。
    - `npm run build`。プロジェクトをコンパイルします。
    - `npm start`。アプリを実稼働モードで起動します。

    VS Code に統合された WSL ターミナルを開きます ( **[表示] > [ターミナル]** )。 ターミナル パスがプロジェクト ディレクトリ (`~/NextProjects/my-next-app$`) を指していることを確認してください。 その後、`npm run dev` を使用して、新しい Next.js アプリの開発インスタンスを実行してみてください。

6. ローカル開発サーバーが起動します。プロジェクト ページのビルドが完了すると、ターミナルに "正常にコンパイルされました - 準備完了 [http://localhost:3000](http://localhost:3000)" が表示されます。 この localhost リンクを選択して、Web ブラウザーで新しい Next.js アプリを開きます。

    ![localhost:3000 で実行されている Next.js アプリ](../images/next-app.png)

7. VS Code エディターで `pages/index.js` ファイルを開きます。 ページ タイトル `<h1 className='title'>Welcome to Next.js!</h1>` を見つけ、それを `<h1 className='title'>This is my new Next.js app!</h1>` に変更します。 Web ブラウザーを引き続き localhost:3000 で開いたまま、変更を保存し、ホット リロード機能が自動的にコンパイルされたことに注意して、ブラウザーで変更を更新します。

8. Next.js でのエラーの処理方法を見てみましょう。 `</h1>` の終了タグを削除して、タイトル コードが `<h1 className='title'>This is my new Next.js app!` となるようにします。 この変更を保存した後、ブラウザーに "コンパイルに失敗しました" というエラーが表示され、ターミナルでは `<h1>` の終了タグが予期されていたことが通知される点に注意してください。 `</h1>` の終了タグを置き換えて保存すると、ページが再度読み込まれます。

Next.js アプリで VS Code のデバッガーを使用するには、F5 キーを押すか、またはメニュー バーの **[表示] > [デバッグ]** (Ctrl+Shift+D) および **[表示] > [デバッグ コンソール]** (Ctrl+Shift+Y) に移動します。 デバッグ ウィンドウで歯車アイコンを選択すると、デバッグ セットアップの詳細を保存するための起動構成 (`launch.json`) ファイルが作成されます。 詳細については、[VS Code のデバッグ](https://code.visualstudio.com/docs/nodejs/nodejs-debugging)に関するページを参照してください。

![VS Code のデバッグ ウィンドウと launch.json の構成アイコン](../images/vscode-debug-launch-configuration.png)

Next.js の詳細については、[Next.js のドキュメント](https://nextjs.org/docs)を参照してください。

## <a name="get-started-with-nuxtjs"></a>Nuxt.js の概要

Nuxt.js は、Vue.js、Node.js、Webpack、Babel.js に基づいて、サーバー レンダリング JavaScript アプリを作成するためのフレームワークです。 これは Next.js から着想が得られました。 これは基本的に、Vue 用のプロジェクト定型文です。 Next.js と同様に、これはベスト プラクティスに注意して作成されており、"ユニバーサル" Web アプリを簡単かつ一貫した方法で、構成をほとんど必要とせずに作成できるようにします。 これらの "ユニバーサル" サーバー レンダリング Web アプリは "isomorphic" とも呼ばれることがあります。つまり、コードがクライアントとサーバーの間で共有されます。

Nuxt.js プロジェクトを作成するには (これには、どのような種類の統合サーバー側フレームワーク、UI フレームワーク、テスト フレームワーク、モード、モジュール、リンターをインストールするかに関する一連の質問への回答が含まれます):

1. WSL ターミナルを開きます (Ubuntu 18.04)。

2. 新しいプロジェクト フォルダーを作成し (`mkdir NuxtProjects`)、そのディレクトリに移動します (`cd NuxtProjects`)。

3. Nuxt.js をインストールし、プロジェクトを作成します ('my-nuxt-app' をアプリに付ける名前に置き換えます) (`npm create nuxt-app my-nuxt-app`)。

4. Nuxt.js インストーラーから次の項目をたずねられます。
    - プロジェクト名: my-nuxtjs-app
    - プロジェクトの説明: Nuxt.js アプリの説明。
    - 作成者名: GitHub のエイリアスを使用します。
    - パッケージ マネージャーの選択: Yarn または **Npm** - この例では NPM を使用します。
    - UI フレームワークの選択: なし、Ant Design Vue、Bootstrap Vue など。 この例では **[Vuetify]** を選択しましょう。ただし、Vue コミュニティが、プロジェクトに最も適したものを選択するために役立つ[これらの UI フレームワークを比較した素晴らしいサマリー](https://vue-community.org/guide/ecosystem/ui-libraries.html#summary-tldr)を作成しました。
    - カスタム サーバー フレームワークの選択: なし、AdonisJs、Express、Fastify など。 この例では **[なし]** を選択しましょう。ただし、Dev.to のサイトで [2019-2020 サーバー フレームワークの比較](https://dev.to/santypk4/introducing-the-best-10-node-js-frameworks-for-2019-and-2020-mcm)を見つけることができます。
    - Nuxt.js モジュールの選択 (Space キーを使用してモジュールを選択します。何も選択しない場合は単に Enter キーを押します): Axios (HTTP 要求の簡略化のため) または [PWA サポート](https://pwa.nuxtjs.org/) (サービス ワーカー、manifest.json ファイルなどの追加のため)。 この例では、モジュールを追加しないようにしましょう。
    - リンティング ツールの選択: **ESLint**、Prettier、Lint のステージングされたファイル。 **[ESLint]** (コードを分析し、潜在的なエラーを警告するためのツール) を選択しましょう。
    - テスト フレームワークの選択: **なし**、Jest、AVA。 このクイックスタートではテストを行わないため、 **[なし]** を選択しましょう。
    - レンダリング モードの選択: **ユニバーサル (SSR)** またはシングル ページ アプリ (SPA)。 この例では **[Universal (SSR)] (ユニバーサル (SSR))** を選択しましょう。ただし、[Nuxt.js のドキュメント](https://nuxtjs.org/guide#server-rendered-universal-ssr-)にいくつかの違いが指摘されています。つまり、SSR にはアプリのサーバー レンダリングのための Node.js サーバーが必要であり、SPA は静的ホスティング用です。
    - 開発ツールの選択: **jsconfig.json** (Intellisense コード補完が機能するように VS Code に推奨されます)

5. プロジェクトが作成されたら、`cd my-nuxtjs-app` を入力して Nuxt.js プロジェクトのディレクトリに移動し、「`code .`」と入力して VS Code WSL-Remote 環境でプロジェクトを開きます。

    ![WSL-Remote の拡張](../images/wsl-remote-extension.png)

6. Nuxt.js がインストールされたら、次の 3 つのコマンドを覚えておく必要があります。

    - `npm run dev`。開発インスタンスをホット リロード、ファイル監視、タスク再実行で実行します。
    - `npm run build`。プロジェクトをコンパイルします。
    - `npm start`。アプリを実稼働モードで起動します。

    VS Code に統合された WSL ターミナルを開きます ( **[表示] > [ターミナル]** )。 ターミナル パスがプロジェクト ディレクトリ (`~/NuxtProjects/my-nuxt-app$`) を指していることを確認してください。 その後、`npm run dev` を使用して、新しい Nuxt.js アプリの開発インスタンスを実行してみてください。

6. ローカル開発サーバーが起動します (クライアントとサーバーのコンパイルのための何種類かの見栄えのよい進行状況バーが表示されます)。 プロジェクトのビルドが完了すると、ターミナルに "正常にコンパイルされました" のメッセージとコンパイルにかかった時間が表示されます。 Web ブラウザーで [http://localhost:3000](http://localhost:3000) に移動して、新しい Nuxt.js アプリを開きます。

    ![localhost:3000 で実行されている Nuxt.js アプリ](../images/nuxt-app.png)

7. VS Code エディターで `pages/index.vue` ファイルを開きます。 ページ タイトル `<v-card-title class="headline">Welcome to the Vuetify + Nuxt.js template</v-card-title>` を見つけ、それを `<v-card-title class="headline">This is my new Nuxt.js app!</v-card-title>` に変更します。 Web ブラウザーを引き続き localhost:3000 で開いたまま、変更を保存し、ホット リロード機能が自動的にコンパイルされたことに注意して、ブラウザーで変更を更新します。

8. Nuxt.js でのエラーの処理方法を見てみましょう。 `</v-card-title>` の終了タグを削除して、タイトル コードが `<v-card-title class="headline">This is my new Nuxt.js app!` となるようにします。 この変更を保存した後、ブラウザーにコンパイル エラーが表示され、ターミナルでは `<v-card-title>` の終了タグが欠けていることと、コード内でエラーが見つかる行番号が通知される点に注意してください。 `</v-card-title>` の終了タグを置き換えて保存すると、ページが再度読み込まれます。

Nuxt.js アプリで VS Code のデバッガーを使用するには、F5 キーを押すか、またはメニュー バーの **[表示] > [デバッグ]** (Ctrl+Shift+D) および **[表示] > [デバッグ コンソール]** (Ctrl+Shift+Y) に移動します。 デバッグ ウィンドウで歯車アイコンを選択すると、デバッグ セットアップの詳細を保存するための起動構成 (`launch.json`) ファイルが作成されます。 詳細については、[VS Code のデバッグ](https://code.visualstudio.com/docs/nodejs/nodejs-debugging)に関するページを参照してください。

![VS Code のデバッグ ウィンドウと launch.json の構成アイコン](../images/vscode-debug-launch-configuration.png)

Nuxt.js の詳細については、[Nuxt.js のガイド](https://nuxtjs.org/guide)を参照してください。

## <a name="get-started-with-gatsbyjs"></a>Gatsby.js の概要

Gatsby.js は、React.js に基づいた静的サイト ジェネレーター フレームワークであり、Next.js や Nuxt.js などのサーバー レンダリングとは異なります。 静的サイト ジェネレーターでは、ビルド時に静的な HTML が生成されます。 サーバーは必要ありません。 Next.js や Nuxt.js では、実行時に (新しい要求が来るたびに) HTML が生成されます。 それには、サーバーが実行されている必要があります。 Gatsby ではまた、アプリで (GraphQL を使用して) データを処理する方法も規定されています。それに対して、Next.js や Nuxt.js ではその決定がユーザーに任されています。

Gatsby.js プロジェクトを作成するには:

1. WSL ターミナルを開きます (Ubuntu 18.04)。
2. 新しいプロジェクト フォルダーを作成し (`mkdir GatsbyProjects`)、そのディレクトリに移動します (`cd GatsbyProjects`)。
3. npm を使用して Gatsby CLI をインストールします (`npm install -g gatsby-cli`)。 インストールされたら、`gatsby --version` を使用してバージョンを確認します。
4. Gatsby.js プロジェクトを作成します (`gatsby new my-gatsby-app`)。
5. パッケージがインストールされたら、新しいアプリ フォルダーにディレクトリを変更してから (`cd my-gatsby-app`)、`code .` を使用して VS Code で Gatsby プロジェクトを開きます。 これにより、VS Code のエクスプローラーを使用して、アプリ用に作成された Gatsby.js フレームワークを確認できます。 VS Code によって、アプリが WSL-Remote 環境で開かれたことに注意してください (VS Code ウィンドウの左下にある緑色のタブに示されています)。 これは、Windows OS で編集のために VS Code を使用している間、アプリが引き続き Linux OS 上で実行されていることを示します。

    ![WSL-Remote の拡張](../images/wsl-remote-extension.png)

6. Gatsby がインストールされたら、次の 3 つのコマンドを覚えておく必要があります。

    - `gatsby develop`。開発インスタンスをホット リロードで実行します。
    - `gatsby build`。製品版ビルドを作成します。
    - `gatsby serve`。アプリを実稼働モードで起動します。

    VS Code に統合された WSL ターミナルを開きます ( **[表示] > [ターミナル]** )。 ターミナル パスがプロジェクト ディレクトリ (`~/GatsbyProjects/my-gatsby-app$`) を指していることを確認してください。 その後、`gatsby develop` を使用して、新しいアプリの開発インスタンスを実行してみてください。

7. 新しい Gatsby プロジェクトのコンパイルが完了すると、ターミナルに "ブラウザーに gatsby-starter-default を表示できるようになりました。 [http://localhost:8000/](http://localhost:8000/)" が表示されます。 この localhost リンクを選択して、Web ブラウザーに組み込まれた新しいプロジェクトを表示します。

> [!NOTE]
> ターミナル出力からは、"ブラウザー内 IDE である GraphiQL を表示して、サイトのデータやスキーマを調査する: [http://localhost:8000/___graphql](http://localhost:8000/___graphql)" こともできると通知されることがわかります。 GraphQL では、Gatsby に組み込まれた自己文書化 IDE (GraphiQL) に API が統合されます。 サイトのデータやスキーマの調査に加えて、クエリ、変異、サブスクリプションなどの GraphQL 操作を実行できます。 詳細については、[GraphiQL の概要](https://www.gatsbyjs.org/docs/running-queries-with-graphiql/)に関するページを参照してください。

8. VS Code エディターで `src/pages/index.js` ファイルを開きます。 ページ タイトル `<h1 >Hi people</h1>` を見つけ、それを `<h1 >Hi (Your Name)!</h1>` に変更します。 Web ブラウザーを引き続き localhost:8000 で開いたまま、変更を保存し、ホット リロード機能が自動的にコンパイルされたことに注意して、ブラウザーで変更を更新します。

    ![localhost:3000 で実行されている Gatsby.js アプリ](../images/gatsby-app.png)

9. Next.js でのエラーの処理方法を見てみましょう。 `</h1>` の終了タグを削除して、タイトル コードが `<h1>Hi (Your Name)!` となるようにします。 この変更を保存した後、ブラウザーに "コンパイルに失敗しました" というエラーが表示され、ターミナルでは `<h1>` の終了タグが予期されていたことが通知される点に注意してください。 `</h1>` の終了タグを置き換えて保存すると、ページが再度読み込まれます。

Next.js アプリで VS Code のデバッガーを使用するには、F5 キーを押すか、またはメニュー バーの **[表示] > [デバッグ]** (Ctrl+Shift+D) および **[表示] > [デバッグ コンソール]** (Ctrl+Shift+Y) に移動します。 デバッグ ウィンドウで歯車アイコンを選択すると、デバッグ セットアップの詳細を保存するための起動構成 (`launch.json`) ファイルが作成されます。 詳細については、[VS Code のデバッグ](https://code.visualstudio.com/docs/nodejs/nodejs-debugging)に関するページを参照してください。

![VS Code のデバッグ ウィンドウと launch.json の構成アイコン](../images/vscode-debug-launch-configuration.png)

Gatsby の詳細については、[Gatsby.js のドキュメント](https://www.gatsbyjs.org/docs/)を参照してください。
