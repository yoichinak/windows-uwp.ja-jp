---
title: Windows 10 の開発環境をセットアップする
description: Windows 開発環境をセットアップして最適化するためのガイドです。 Windows または Linux 用 Windows サブシステムを使用して開発するために必要な言語とツールのインストールを開始します。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
ms.technology: windows-nodejs
keywords: ''
ms.localizationpriority: medium
ms.date: 07/01/2020
ROBOTS: NOINDEX
ms.openlocfilehash: 237c3e8f58e41007840cf72aa1fd65efdc5763a5
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493829"
---
# <a name="set-up-your-windows-10-development-environment"></a>Windows 10 の開発環境をセットアップする

このガイドでは、Windows または Linux 用 Windows サブシステムを使用して開発するために必要な言語とツールのインストールとセットアップを始められるようにします。

## <a name="development-paths"></a>開発パス

:::row:::
    :::column:::
       [![JavaScrip/NodeJS](../images/nodejs-logo.png)](https://docs.microsoft.com/windows/nodejs)<br>
        **[NodeJS の概要](https://docs.microsoft.com/windows/nodejs)**<br>
        Windows または Linux 用 Windows サブシステムで、NodeJS をインストールし、開発環境をセットアップします。
    :::column-end:::
    :::column:::
       [![Python](../images/python-logo.png)](https://docs.microsoft.com/windows/python)<br>
        **[Python の概要](https://docs.microsoft.com/windows/python)**<br>
        Windows または Linux 用 Windows サブシステムで、Python をインストールし、開発環境をセットアップします。
    :::column-end:::
    :::column:::
       [![Android](../images/android-logo.png)](https://docs.microsoft.com/windows/android)<br>
        **[Android の概要](https://docs.microsoft.com/windows/android)**<br>
        Android Studio をインストールするか、Xamarin、React、Cordova などのクロスプラットフォーム ソリューションを選択して、Windows で開発環境をセットアップします。
    :::column-end:::
    :::column:::
       [![Windows デスクトップ](../images/windows-logo.png)](https://docs.microsoft.com/windows/apps/)<br>
        **[Windows の概要](https://docs.microsoft.com/windows/apps/)**<br>
        UWP、Win32、WPF、Windows フォームを使用して Windows 10 用のデスクトップ アプリの構築を始めるか、MSIX と XAML Island を使用して既存のデスクトップ アプリの更新とデプロイを始めます。
    :::column-end:::
:::row-end:::

## <a name="tools-and-platforms"></a>ツールとプラットフォーム

:::row:::
    :::column:::
       [![WSL](../images/windows-linux-dev-env.png)](https://docs.microsoft.com/windows/wsl/)<br>
        **[Linux 用 Windows サブシステム](https://docs.microsoft.com/windows/wsl/)**<br>
        Windows と完全に統合された好みの Linux ディストリビューションを使用します (デュアルブートの必要はもうありません)。<br>
        [WSL をインストールする](https://docs.microsoft.com/windows/wsl/install-win10)
    :::column-end:::
    :::column:::
       [![Windows ターミナル](../images/terminal.png)](https://docs.microsoft.com/windows/terminal/)<br>
        **[Windows ターミナル](https://docs.microsoft.com/windows/terminal/)**<br>
        複数のコマンド ライン シェルで動作するようにターミナル環境をカスタマイズします。
        <br>
        [ターミナルをインストールする](https://www.microsoft.com/p/windows-terminal/9n0dx20hk701?rtc=1&activetab=pivot:overviewtab)
    :::column-end:::
    :::column:::
       [![Windows パッケージ マネージャー](../images/winget.png)](https://docs.microsoft.com/windows/package-manager/)<br>
        **[Windows パッケージ マネージャー](https://docs.microsoft.com/windows/package-manager/)**<br>
        包括的なパッケージ マネージャーである WinGet をコマンド ラインで使用して、Windows 10 にアプリケーションをインストールします。<br>
        [WinGet をインストールする](https://docs.microsoft.com/windows/package-manager/winget/#install-winget)
    :::column-end:::
    :::column:::
       [![PowerToys](../images/powertoys.png)](https://github.com/microsoft/PowerToys)<br>
        **[Windows PowerToys](https://github.com/microsoft/PowerToys)**<br>
        この一連のパワー ユーザー ユーティリティを使用して、生産性が向上するように Windows のエクスペリエンスを調整して合理化します。<br>
        [PowerToys をインストールする](https://github.com/microsoft/PowerToys#installing-and-running-microsoft-powertoys)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
       [![VS Code](../images/Vscode.png)](https://code.visualstudio.com/docs)<br>
        **[VS Code](https://code.visualstudio.com/docs)**<br>
        JavaScript、TypeScript、Node.js、拡張機能のリッチなエコシステム (C++、C#、Java、Python、PHP、Go)、ランタイム (.NET や Unity など) のサポートが組み込まれている軽量のソース コード エディター。<br>
        [VS Code をインストールする](https://code.visualstudio.com/download)
    :::column-end:::
    :::column:::
       [![Visual Studio](../images/visualstudio.png)](https://docs.microsoft.com/visualstudio/windows/)<br>
        **[Visual Studio](https://docs.microsoft.com/visualstudio/windows/)**<br>
        コンパイラ、IntelliSense コード補完、その他多くの機能が含まれ、コードの編集、デバッグ、ビルドと、アプリの発行を行うことができる統合開発環境。<br>
        [Visual Studio をインストールする](https://docs.microsoft.com/visualstudio/install/install-visual-studio)
    :::column-end:::
    :::column:::
       [![Azure](../images/Azure.png)](https://docs.microsoft.com/azure/guides/developer/azure-developer-guide)<br>
        **[Azure](https://docs.microsoft.com/azure/guides/developer/azure-developer-guide)**<br>
        既存のアプリをホストし、新しい開発を効率化するための完全なクラウド プラットフォーム。 Azure サービスには、アプリの開発、テスト、デプロイ、管理に必要なすべてのものが統合されています。<br>
        [Azure アカウントを設定する](https://azure.microsoft.com/free/)
    :::column-end:::
    :::column:::
       [![.NET](../images/net.png)](https://dotnet.microsoft.com/)<br>
        **[.NET](https://docs.microsoft.com/dotnet/standard/get-started/)**<br>
        Web、モバイル、デスクトップ、ゲーム、IoT、クラウド、マイクロサービスなど、あらゆる種類のアプリを構築するためのツールとライブラリが含まれるオープンソースの開発プラットフォーム。<br>
        [.NET をインストールする](https://dotnet.microsoft.com/download)
    :::column-end:::
:::row-end:::

<br>

---

<br>

![フィラー イメージ](../images/flashy-office.png)

## <a name="tips-for-improving-your-workflow"></a>ワークフローを改善するためのヒント

ワークフローをいっそう効率的で楽しいものにするのに役立ついくつかのヒントをまとめました。 共有したいヒントが他にもありますか。 上の [編集] ボタンを使用して Pull Request を提出するか、下の [フィードバック] ボタンを使用して問題を報告していただけば、こちらでそれを一覧に追加します。

* Linux 用 Windows サブシステムは、内部開発ループの一部として使用することが意図されています。 たとえば、CI/CD パイプラインを作成し、WSL 2 を使用して Windows コンピューターに Ubuntu をインストールして、実際の Linux インスタンスでローカルに開発するワークフローをお勧めします。 正常に動作していることを確認したら、Docker コンテナーに格納し、そのコンテナーをクラウド インスタンスにプッシュして、運用対応の Ubuntu VM で実行することにより、その CI/CD パイプラインをクラウドにプッシュできます。 WSL を使用する他の方法については、[WSL 2 でのタブとスペースの比較のエピソード](https://channel9.msdn.com/Shows/Tabs-vs-Spaces/WSL2-Code-faster-on-the-Windows-Subsystem-for-Linux)に関する動画をご覧ください。

* Windows と Linux 用 Windows サブシステムの両方を使用している場合は、次の 2 つのファイル システムがインストールされます: NTSF (Windows) と WSL (お使いの Linux ディストリビューション)。 パフォーマンスを向上させるには、使用しているツールと同じシステムにプロジェクト ファイルを格納してください。 詳細については、[パフォーマンスを向上させるための適切なファイル システムの選択](https://docs.microsoft.com/windows/wsl/compare-versions#use-the-linux-file-system-for-faster-performance)に関するページを参照してください。

* Windows Defender の設定を更新し、セキュリティ脅威のスキャンを避けるのに十分であると信じられるプロジェクト フォルダーまたはファイルの種類の除外を追加することで、ビルドの速度を向上させることができます。 詳細については、「[パフォーマンスを向上させるための Windows Defender 設定の更新](https://docs.microsoft.com/windows/android/defender-settings)」を参照してください。

![Windows Defender のスクリーンショット](../images/windows-defender-exclusions.png)

* `code .` コマンドを使用し、コマンド ラインから VS Code を起動してプロジェクを開くことができます。または、Windows または WSL ディストリビューションから `explorer.exe .` を使用して、エクスプローラーでコマンド ラインからプロジェクト ディレクトリを開くことができます。 これが既定で動作しない場合は、VS Code の実行可能ファイルを PATH 環境変数に追加することが必要な場合があります。 詳細については、「[コマンド ラインからの起動](https://code.visualstudio.com/docs/editor/command-line#_launching-from-command-line)」を参照してください。

![エクスプローラーのスクリーンショット](../images/wsl-file-explorer.png)

* バージョン管理とコラボレーションに Git を使用している場合は、[Git Credential Manager を設定](https://docs.microsoft.com/windows/wsl/tutorials/wsl-git#git-credential-manager-setup)してトークンを Windows 資格情報マネージャーに格納することで、認証プロセスを効率化できます。 また、プロジェクトに [.gitignore ファイルを追加する](https://docs.microsoft.com/windows/wsl/tutorials/wsl-git#adding-a-git-ignore-file)ことをお勧めします。

* [Windows ターミナルのコマンド ライン引数](https://docs.microsoft.com/windows/terminal/command-line-arguments?tabs=powershell#multiple-panes)を使用することで、PowerShell、Ubuntu、Azure CLI など、複数のコマンド ラインをすべて、単一のウィンドウの複数のペインで起動することができます。 [Windows ターミナル](https://docs.microsoft.com/windows/terminal/get-started)、[WSL/Ubuntu](https://docs.microsoft.com/windows/wsl/install-win10)、[Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) をインストールした後、PowerShell で次のコマンドを入力して、新しい複数ペイン ウィンドウで 3 つをすべて開きます。

    ```powershell
    wt -p "Command Prompt" `; split-pane -p "Windows PowerShell" `; split-pane -H wsl.exe
    ```

![フィラー イメージ](../images/flashy-office2.png)

## <a name="transitioning-between-mac-and-windows"></a>Mac と Windows の間での移行

[Mac と Windows (または Linux 用 Windows サブシステム) 開発環境間の移行に関するガイド](https://docs.microsoft.com/windows/dev-environment/mac-to-windows)を参照してください。 次のような違いをマップするのに役立ちます。

* [キーボード ショートカット](https://docs.microsoft.com/windows/dev-environment/mac-to-windows#keyboard-shortcuts)
* [トラックパッドのショートカット](https://docs.microsoft.com/windows/dev-environment/mac-to-windows#trackpad-shortcuts)
* [ターミナルとシェルのツール](https://docs.microsoft.com/windows/dev-environment/mac-to-windows#terminal-and-shell)
* [アプリとユーティリティ](https://docs.microsoft.com/windows/dev-environment/mac-to-windows#apps-and-utilities)

## <a name="stories-from-developers-who-have-switched"></a>切り替えた開発者からのストーリー

他の開発者から、Mac と Windows の間での開発環境エクスペリエンスの切り替えについて話を聞くと役に立つと思われます。 多くは、プロセスが非常にシンプルであると感じ、使い慣れた Linux やオープンソース ツールを使用できる一方で、[Microsoft Office](https://www.microsoft.com/microsoft-365/products-apps-services)、[Outlook](https://www.microsoft.com/microsoft-365/outlook/email-and-calendar-software-microsoft-outlook)、[Teams](https://www.microsoft.com/microsoft-365/microsoft-teams/group-chat-software) などの Windows の生産性向上ツールへのアクセスも統合されていることを、喜んでいました。 いくつかの記事とブログ エントリを紹介しておきます。

* Ken Wang、「[違うことを考える — Mac から Windows に切り替えるソフトウェア開発者](https://medium.com/@kenwang_57215/software-developer-switching-from-mac-to-windows-66773d331910)」
* Owen Williams、「[2019 の Mac から Windows への切り替えの状態](https://char.gd/blog/2019/the-state-of-switching-to-windows-from-mac-in-2019)」
* Brent Rose、「[私が Mac から Windows に切り替えたときに何が起こったか](https://www.wired.com/story/rant-switching-from-mac-to-windows/)」
* Jack Franklin、「[フロントエンド Web 開発への Windows 10 と WSL の使用](https://www.jackfranklin.co.uk/blog/frontend-development-with-windows-10/)」
* Aaron Schlesinger、「[Mac から Windows と WSL 2 に](https://arschles.com/blog/coming-from-a-mac-to-windows-wsl-2/)」
* David Heinemeier Hansson、「[20 年後に Windows に戻る](https://m.signalvnoise.com/back-to-windows-after-twenty-years/)」
* Ray Elenteny、「[私が Windows に戻った理由](https://dzone.com/articles/why-i-returned-to-windows)」

## <a name="tutorials-courses-and-code-samples"></a>チュートリアル、コース、コード サンプル

いくつかの一般的な作業シナリオを始めるときに役立つ、チュートリアル、コース、コード サンプルを以下に示します。

* [React と Azure Cosmos DB を使って MongoDB アプリを作成する](https://docs.microsoft.com/azure/cosmos-db/tutorial-develop-mongodb-react)

* [ドラッグ アンド ドロップ機能を備えた Android デュアル スクリーン アプリを構築する](https://docs.microsoft.com/dual-screen/android/samples)

* [Xamarin.Forms を使用して To Do リストのクロス プラットフォーム アプリを構築する](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/todo/)

* [Google Play 開発者サービスを利用して Google Maps API をデモする Xamarin.Android アプリを構築する](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/todo/)

* [Azure App Service で PostgreSQL を使用して Python (Django) Web アプリをデプロイする](https://docs.microsoft.com/azure/app-service/containers/tutorial-python-postgresql-app?tabs=bash)

* [Blazor を使用して最初の ASP.Net Core Web アプリを構築する](https://docs.microsoft.com/aspnet/core/tutorials/build-your-first-blazor-app?view=aspnetcore-3.1)

* [Microsoft Graph を使って Java アプリを構築する](https://docs.microsoft.com/graph/tutorials/java)

* [Azure AD V2 を使用して WPF アプリケーションから ASP.NET Core Web API を呼び出す](https://docs.microsoft.com/samples/azure-samples/active-directory-dotnet-native-aspnetcore-v2/calling-an-aspnet-core-web-api-from-a-wpf-application-using-azure-ad-v2/?view=aspnetcore-3.1)

* [クラウドネイティブの ASP.NET Core マイクロサービスを作成してデプロイする](https://docs.microsoft.com/learn/modules/microservices-aspnet-core/?view=aspnetcore-3.1)

* [Microsoft Learn の無料オンライン コースを調べる](https://docs.microsoft.com/learn/browse/)

![フィラー イメージ](../images/flashy-office3.png)

## <a name="additional-resources"></a>その他の資料

* [Microsoft Edge Web ブラウザーのドキュメント](https://docs.microsoft.com/microsoft-edge/)
* [Web サイトの品質向上のために WebHint を試す](https://webhint.io/)
* [Microsoft Game Stack のドキュメント](https://docs.microsoft.com/gaming/)
