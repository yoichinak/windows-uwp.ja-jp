---
title: Windows 10 での Web 開発
description: Microsoft 全体から、ツール、api、各種リソースへのリンクを含む、Windows 10 での Web 開発ガイドです。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: web 開発、web 開発、windows 上の web、api、エッジ
ms.date: 01/06/2021
ms.openlocfilehash: 98b5d1443869baec9dd46796088ff7b71521237d
ms.sourcegitcommit: 6661f4d564d45ba10e5253864ac01e43b743c560
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/23/2021
ms.locfileid: "104804486"
---
# <a name="web-development-on-windows-10"></a>Windows 10 での Web 開発

Microsoft では、Web 開発者向けにさまざまなリソースを提供しています。これには、Windows 10 を使用した Web 開発をサポートする新しいツールと機能が含まれます。 このガイドでは、使用可能なツールの多くについて説明し、Windows を Web 用の理想的な開発環境にするために[フィードバックを送信](../dev-environment/overview.md#additional-resources)することもできます。 API の一覧については、[Web 開発用の API](/apis.md) に関する記事を参照してください。 作業の開始に関する詳細については、「[Windows 10 で開発環境を設定する](../dev-environment/overview.md)」を参照してください。

## <a name="webview-devtools-pwas"></a>WebView、DevTools、PWA

:::row:::
    :::column:::
        [![WebView アイコン](../images/webview2.png)](https://developer.microsoft.com/microsoft-edge/webview2/)<br>
        **[WebView 2](https://developer.microsoft.com/microsoft-edge/webview2/)**<br>
        Microsoft Edge WebView2 を使用して、ネイティブ アプリケーションに Web コンテンツ (HTML、CSS、および JavaScript) を埋め込みます。
        [WebView 2 のダウンロード](https://developer.microsoft.com/microsoft-edge/webview2/#download-section)
    :::column-end:::
    :::column:::
        [![Microsoft Edge DevTools アイコン](../images/microsoftedge-devtools.png)](/microsoft-edge/devtools-guide-chromium/index.md)<br>
        **[Microsoft Edge DevTools](/microsoft-edge/devtools-guide-chromium/)**<br>
        Microsoft Edge 開発者ツールは、Microsoft Edge ブラウザーに直接組み込まれている検査ツールとデバッグ ツールのセットです。
    DevTools を開くには、Microsoft Edge を使用して次のようにします。
        - 右クリックして検査
        - `F12` キーを選択
        - `Ctrl` + `Shift` + `i`
    :::column-end:::
    :::column:::
       [![PWA アイコン](../images/pwa-icon.png)](/microsoft-edge/progressive-web-apps-chromium/)<br>
        **[Windows のプログレッシブ Web アプリ](/microsoft-edge/progressive-web-apps-chromium/)**<br>
        プログレッシブ Web アプリ (PWA) を使用すると、ユーザーに自分のデバイス用にカスタマイズされたネイティブなアプリのようなエクスペリエンスを提供できます。 これは、サポート プラットフォームでネイティブ アプリのように機能するよう徐々に拡張される Web サイトです。<br>
        [PWA の概要](/microsoft-edge/progressive-web-apps-chromium/get-started)
    :::column-end:::
:::row-end:::

## <a name="microsoft-edge-browser"></a>Microsoft Edge ブラウザー

:::row:::
    :::column:::
       [![Microsoft Edge アイコン](../images/microsoftedge.png)](https://www.microsoft.com/en-us/edge)<br>
        **[開発者向け Microsoft Edge](https://developer.microsoft.com/microsoft-edge/)**<br>
        新しい Microsoft Edge は Chromium に基づいています。これにより、Web 互換性が向上し、基になる Web プラットフォームの断片化が少なくなります。 2020 年 1 月 15 日にリリースされ、Windows、macOS、iOS、および Android でサポートされます。 <br>
        [新しい Microsoft Edge をインストールする](https://www.microsoft.com/edge)
    :::column-end:::
    :::column:::
        [![ビジネス向け Microsoft Edge](../images/microsoftedge-enterprise.png)](/deployedge/)<br>
        **[ビジネス向け Microsoft Edge](/deployedge/)**<br>
        Microsoft Edge は Chromium に基づいており、エンタープライズ サポートを提供しています。 使用可能な複数のチャネルを構成して展開する方法の手順について説明します。<br>
        [Microsoft Edge チャネルをダウンロードする](https://www.microsoft.com/edge/business/download)
    :::column-end:::
    :::column:::
        [![Microsoft Edge Insider アイコン](../images/microsoftedge-beta.png)](https://www.microsoftedgeinsider.com/whats-new)<br>
        **[Microsoft Edge Insider](https://www.microsoftedgeinsider.com/whats-new)**<br>
        Microsoft Edge では日々新しいものが構築されています。 最新の進行状況と、その使用方法をご覧ください。
        [Microsoft Edge ベータ版をダウンロードする](https://www.microsoftedgeinsider.com/)
    :::column-end:::
    :::column:::
        [![Microsoft Edge サポート アイコン](../images/microsoftedge-support.png)](https://support.microsoft.com/microsoft-edge)<br>
        **[Microsoft Edge のサポート](https://support.microsoft.com/microsoft-edge)**<br>
        ブラウザーのカスタマイズ、拡張機能の追加、追跡防止、トラブルシューティングなどのためのヘルプが用意されています。
        [Microsoft Edge に関するヘルプを表示する](https://support.microsoft.com/microsoft-edge)
    :::column-end:::
:::row-end:::

## <a name="debugging-testing-and-accessibility"></a>デバッグ、テスト、アクセシビリティ

:::row:::
    :::column:::
       [![VS Marketplace Edge デバッガー拡張機能](../images/visualstudio-edge-debugger.png)](https://marketplace.visualstudio.com/items?itemName=msjsdiag.debugger-for-edge)<br>
        **[VS Code: Microsoft Edge 用のデバッガー](https://marketplace.visualstudio.com/items?itemName=msjsdiag.debugger-for-edge/)**<br>
        この VS Code 拡張機能を使用して、Microsoft Edge ブラウザーで JavaScript コードをデバッグします。 Visual Studio の ASP.NET プロジェクトからも使用できます。<br>
        [VS Code: Microsoft Edge 用のデバッガーをインストールする](https://marketplace.visualstudio.com/items?itemName=msjsdiag.debugger-for-edge)
    :::column-end:::
    :::column:::
       [![[仮想マシン] アイコン](../images/virtualmachine.png)](https://developer.microsoft.com/microsoft-edge/tools/vms/)<br>
        **[テスト用の仮想マシン](https://developer.microsoft.com/microsoft-edge/tools/vms/)**<br>
        ローカルでダウンロードして管理する無料の Windows 10 仮想マシンを使用して、IE11 と Microsoft Edge 従来版をテストします。<br>
        [仮想マシンをダウンロードする](https://developer.microsoft.com/microsoft-edge/tools/vms/)
    :::column-end:::
    :::column:::
       [![WebHint アイコン](../images/webhint.png)](https://webhint.io/)<br>
        **[アクセシビリティのための WebHint](https://webhint.io/)**<br>
        ベスト プラクティスと一般的なエラーについてコードをチェックすることで、サイトのアクセシビリティ、速度、クロスブラウザーの互換性などの向上に役立つカスタマイズ可能なツール。<br>
        [VS Code 拡張機能をインストールする](https://webhint.io/docs/user-guide/extensions/vscode-webhint/)<br>
        [ブラウザー拡張機能をインストールする](https://webhint.io/docs/user-guide/extensions/extension-browser/)<br>
        [CLI のインストール](https://webhint.io/docs/user-guide/)
    :::column-end:::
    :::column:::
       [![WebDriver アイコン](../images/webdriver.png)](/microsoft-edge/webdriver-chromium/)<br>
        **[WebDriver](/microsoft-edge/webdriver-chromium/)**<br>
        Microsoft WebDriver を使用して Microsoft Edge で web サイトのテストを自動化することにより、開発者サイクルのループを閉じます。<br>
        [WebDriver をインストールする](https://developer.microsoft.com/microsoft-edge/tools/webdriver/)
    :::column-end:::
:::row-end:::

## <a name="visual-studio-code-editors"></a>Visual Studio コード エディター

:::row:::
    :::column:::
       [![VS Code アイコン](../images/Vscode.png)](https://code.visualstudio.com/docs)<br>
        **[VS Code](https://code.visualstudio.com/docs)**<br>
        JavaScript、TypeScript、Node.js、拡張機能のリッチなエコシステム (C++、C#、Java、Python、PHP、Go)、ランタイム (.NET や Unity など) のサポートが組み込まれている軽量のソース コード エディター。<br>
        [VS Code をインストールする](https://code.visualstudio.com/download)
    :::column-end:::
    :::column:::
       [![Visual Studio アイコン](../images/visualstudio.png)](/visualstudio/windows/)<br>
        **[Visual Studio (IDE)](/visualstudio/windows/)**<br>
        コンパイラ、IntelliSense コード補完、その他多くの機能が含まれ、コードの編集、デバッグ、ビルドと、アプリの発行を行うことができる統合開発環境。<br>
        [Visual Studio をインストールする](/visualstudio/install/install-visual-studio)
    :::column-end:::
    :::column:::
       [![VS Code マーケットプレース アイコン](../images/vs-code-marketplace.png)](https://marketplace.visualstudio.com/vscode)<br>
        **[拡張機能の VS Code Marketplace](https://marketplace.visualstudio.com/vscode)**<br>
        Visual Studio Code エディターをカスタマイズするために使用できるさまざまな拡張機能について説明します。<br>
        [拡張機能をインストールする](https://marketplace.visualstudio.com/vscode)
    :::column-end:::
    :::column:::
       [![Visual Studio マーケットプレース アイコン](../images/vs-marketplace.png)](https://marketplace.visualstudio.com/vs/)<br>
        **[拡張機能の Visual Studio Marketplace](https://marketplace.visualstudio.com/vs)**<br>
        Visual Studio 統合開発環境をカスタマイズするために使用できるさまざまな拡張機能について説明します。<br>
        [拡張機能をインストールする](https://marketplace.visualstudio.com/vs)
    :::column-end:::
:::row-end:::

## <a name="wsl-terminal-package-manager-docker-desktop"></a>WSL、ターミナル、パッケージ マネージャー、Docker Desktop

:::row:::
    :::column:::
       [![WSL アイコン](../images/windows-linux-dev-env.png)](/windows/wsl/)<br>
        **[Linux 用 Windows サブシステム](/windows/wsl/)**<br>
        Windows と完全に統合された好みの Linux ディストリビューションを使用します (デュアルブートの必要はもうありません)。<br>
        [WSL をインストールする](/windows/wsl/install-win10)
    :::column-end:::
    :::column:::
       [![Windows ターミナル アイコン](../images/terminal.png)](/windows/terminal/)<br>
        **[Windows ターミナル](/windows/terminal/)**<br>
        複数のコマンド ライン シェルで動作するようにターミナル環境をカスタマイズします。
        <br>
        [ターミナルをインストールする](https://www.microsoft.com/p/windows-terminal/9n0dx20hk701?rtc=1&activetab=pivot:overviewtab)
    :::column-end:::
    :::column:::
       [![Windows パッケージ マネージャー アイコン](../images/winget.png)](../package-manager/index.md)<br>
        **[Windows パッケージ マネージャー](../package-manager/index.md)**<br>
        winget.exe クライアントをコマンド ラインで使用して、Windows 10 にアプリをインストールします。<br>
        [Windows パッケージ マネージャー (パブリック プレビュー) のインストール](../package-manager/winget/index.md#install-winget)
    :::column-end:::
    :::column:::
       [![Docker Desktop for Windows のアイコン](../images/docker-icon.png)](../dev-environment/docker/overview.md)<br>
        **[Docker Desktop for Windows](../dev-environment/docker/overview.md)**<br>
        Visual Studio、VS Code、.NET、Linux 用 Windows サブシステムや様々な Azure サービスのサポートを活用して、リモート開発用コンテナーを作成します。<br>
        [Docker Desktop for Windows をインストールする](https://docs.docker.com/docker-for-windows/install/)
    :::column-end:::
:::row-end:::

## <a name="aspnet-typescript-xamarin"></a>ASP.NET、Typescript、Xamarin

:::row:::
    :::column:::
       [![ASP.NET アイコン](../images/aspnet.png)](https://dotnet.microsoft.com/apps/aspnet)<br>
        **[ASP.NET](/aspnet/)**<br>
        Web アプリとサービス、モノのインターネット (IoT) アプリ、または .NET と C# を使用したモバイル バックエンドを構築するためのクロスプラットフォーム フレームワーク。 Windows、macOS、Linux でお気に入りの開発ツールをご利用いただけます。 クラウドまたはオンプレミスに展開する。 [.NET Core] で実行する。<br>
        [ASP.NET をインストールする](https://dotnet.microsoft.com/download)
    :::column-end:::
    :::column:::
       [![TypeScript アイコン](../images/typescript-icon.png)](https://www.typescriptlang.org/)<br>
        **[TypeScript](https://www.typescriptlang.org/)**<br>
        TypeScript は、言語に型を追加することによって JavaScript を拡張します。 たとえば、JavaScript には、文字列、数値、オブジェクトなどの言語プリミティブが用意されていますが、それらを一貫して割り当てているかどうかはチェックされません。 TypeScript では、これが行われます。<br>
        [ブラウザーで試す](https://www.typescriptlang.org/play/) [ローカルにインストールする](https://www.typescriptlang.org/#installation)
    :::column-end:::
    :::column:::
       [![Xamarin リポジトリ アイコン](../images/xamarin-icon.png)](/xamarin/)<br>
        **[Xamarin](/xamarin/)**<br>
        Xamarin を使用すると、.NET のコードとプラットフォームに固有のユーザー インターフェイスを使用して、Android、iOS、および macOS 用のネイティブ アプリを構築できます。 Xamarin.Forms を使用すると、C# または XAML で記述された共有 UI コードを使用してネイティブ アプリを構築できます。
        <br>
        [Xamarin をインストールする](/xamarin/get-started/installation/)
    :::column-end:::
:::row-end:::

## <a name="open-source-contributions"></a>オープン ソースのコントリビューション

:::row:::
    :::column:::
       [![OpenSource アイコン](../images/opensource-icon.png)](https://opensource.microsoft.com/)<br>
        **[Microsoft のオープン ソース](https://opensource.microsoft.com/)**<br>
        数千もの Microsoft エンジニアが、オープン ソースを毎日使用、貢献、リリースしています。 人気のあるプロジェクトには、Visual Studio Code、TypeScript、.NET、ChakraCore などがあります。<br>
        [参加する](https://opensource.microsoft.com/collaborate)
    :::column-end:::
    :::column:::
       [![WinDev リポジトリ アイコン](../images/windev-repo.png)](https://github.com/microsoft/WinDev)<br>
        **[Windows 開発者パフォーマンスの問題のリポジトリ](https://github.com/microsoft/WinDev)**<br>
        Windows 用か Windows 上のどちらで開発しているかや、クロスプラットフォームの開発用コンピューターとして使用しているかどうかにかかわらず、問題の原因となっているパフォーマンス上の問題について話します。
        <br>
        [パフォーマンスの問題を提出する](https://github.com/microsoft/WinDev/issues)
    :::column-end:::
    :::column:::
       [![docs アイコン](../images/docs.png)](/contribute/)<br>
        **[ドキュメントに投稿する](/contribute/)**<br>
        Microsoft のドキュメント セットは、オープン ソースで GitHub 上でホストされているものがほとんどです。 問題をファイリングしたり、プル要求を作成したりして投稿します。
        <br>
        [方法](/contribute/)
    :::column-end:::
:::row-end:::

## <a name="cloud-development-with-azure"></a>Azure を使用したクラウド開発

:::row:::
    :::column:::
       [![Azure アイコン](../images/Azure.png)](/azure/guides/developer/azure-developer-guide)<br>
        **[Azure](/azure/guides/developer/azure-developer-guide)**<br>
        既存のアプリをホストし、新しい開発を効率化するための完全なクラウド プラットフォーム。 Azure サービスには、アプリの開発、テスト、デプロイ、管理に必要なすべてのものが統合されています。<br>
        [Azure アカウントを設定する](https://azure.microsoft.com/free/)
    :::column-end:::
    :::column:::
       [![Azure コグニティブ サービス アイコン](../images/azure-cognitive-services.png)](/azure/cognitive-services/what-are-cognitive-services)<br>
        **[Azure Cognitive Services](/azure/cognitive-services/what-are-cognitive-services)**<br>
        コグニティブなインテリジェンスを手軽にアプリケーションに組み込むことができる REST API とクライアント ライブラリ SDK が備わったクラウドベースのサービスです。<br>
        [コグニティブ サービスを試す](https://azure.microsoft.com/en-us/services/cognitive-services/)
    :::column-end:::
    :::column:::
       [![Azure 開発ガイド アイコン](../images/Azure.png)](/azure/guides/developer/azure-developer-guide)<br>
        **[Azure について学習](/azure/guides/developer/azure-developer-guide)**<br>
        既存のアプリをホストし、新しい開発を効率化するための完全なクラウド プラットフォーム。 Azure サービスには、アプリの開発、テスト、デプロイ、管理に必要なすべてのものが統合されています。<br>
        [Azure アカウントを設定する](https://azure.microsoft.com/free/)
    :::column-end:::
:::row-end:::

## <a name="addtional-resources"></a>その他のリソース

:::row:::
    :::column:::
       [![開発環境の設定アイコン](../images/dev-environment-icon.png)](../dev-environment/overview.md)<br>
        **[Windows 10 で開発環境を設定する](../dev-environment/overview.md)**<br>
        Python、NodeJS、C#、C、および C++ の使用、Android アプリのビルド、Windows デスクトップ アプリのビルド、Docker コンテナーのビルド、PowerShell スクリプトの実行などを行うための開発環境のセットアップに関するヘルプを確認できます。
        <br>
        [作業開始](../dev-environment/overview.md)
    :::column-end:::
    :::column:::
       [![Windows 用 React Native アイコン](../images/reactnative-windows.png)](https://microsoft.github.io/react-native-windows/)<br>
        **[Windows 用 React Native + macOS](https://microsoft.github.io/react-native-windows/)**<br>
        Windows 10 SDK および macOS 10.13 SDK に対するネイティブ サポートを機能させることができます。 JavaScript を使用して、PC、タブレット、2 in 1、Xbox、Mixed Reality デバイスなど、Windows 10 でサポートされているすべてのデバイス用のネイティブ Windows アプリと、macOS デスクトップおよびラップトップ エコシステムをビルドします。
        <br>
        [Windows 用 React Native をインストールする](https://microsoft.github.io/react-native-windows/docs/getting-started)<br>
        [macOS 用 React Native をインストールする](https://microsoft.github.io/react-native-windows/docs/rnm-getting-started)
    :::column-end:::
    :::column:::
       [![Learn アイコン](../images/learn-icon.png)](/learn/browse/?terms=web)<br>
        **[Web 開発に関連する Microsoft Learn コース](/learn/browse/?terms=web)**<br>
        Microsoft Learn には、各種の新しいスキルを学ぶことができる無料のオンライン コースが用意されており、ステップバイステップのガイダンスを使用して Microsoft の製品やサービスを見つけることができます。
        <br>
        [学習を開始する](/learn/browse/?terms=web)
    :::column-end:::
:::row-end:::

## <a name="transitioning-between-mac-and-windows"></a>Mac と Windows の間での移行

[Mac と Windows (または Linux 用 Windows サブシステム) 開発環境間の移行に関するガイド](../dev-environment/mac-to-windows.md)を参照してください。

- [キーボード ショートカット](../dev-environment/mac-to-windows.md#keyboard-shortcuts)
- [トラックパッドのショートカット](../dev-environment/mac-to-windows.md#trackpad-shortcuts)
- [ターミナルとシェルのツール](../dev-environment/mac-to-windows.md#command-line-shells-and-terminals)
- [アプリとユーティリティ](../dev-environment/mac-to-windows.md#apps-and-utilities)
- [Mac から Windows に切り替えた開発者のストーリー](../dev-environment/dev-stories.md)