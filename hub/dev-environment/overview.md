---
title: Windows 10 で開発環境を設定する
description: Windows で開発環境を設定し、好みのツールとコード言語をインストールするためのガイドです。 Python、NodeJS、VS Code、Git、Bash、Linux のツールとコマンド、Android Studio のいずれを使用するかに関わらず、Windows ターミナルや WSL などの優れた新しいツールを活用できます。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
ms.technology: windows-nodejs
keywords: Windows の設定, 開発環境, 開発ツール, 開発パス, Microsoft, Windows, 開発者, ヒント, パフォーマンス, WSL, ターミナル, nodejs, Python
ms.localizationpriority: medium
ms.date: 07/24/2020
ms.openlocfilehash: e62ca938a23910290a8c63682fc2fde77ec0ea92
ms.sourcegitcommit: 1e06168ada5ce6013b1d07c428548f084464a286
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/28/2020
ms.locfileid: "87363734"
---
# <a name="set-up-your-development-environment-on-windows-10"></a>Windows 10 で開発環境を設定する

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
        **[Windows デスクトップの概要](https://docs.microsoft.com/windows/apps/)**<br>
        UWP、Win32、WPF、Windows フォームを使用して Windows 10 用のデスクトップ アプリの構築を始めるか、MSIX と XAML Island を使用して既存のデスクトップ アプリの更新とデプロイを始めます。
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
       [![C / C++](../images/c-logo.png)](https://docs.microsoft.com/cpp/)<br>
        **[C++ と C の概要](https://docs.microsoft.com/cpp/)**<br>
        C++、C、アセンブリを使用して、アプリ、サービス、ツールを開発します。
    :::column-end:::
    :::column:::
       [![C#](../images/csharp-logo.png)](https://docs.microsoft.com/dotnet/csharp/)<br>
        **[C# の概要](https://docs.microsoft.com/dotnet/csharp/)**<br>
        C# と .NET Core を使用してアプリを構築します。
    :::column-end:::
    :::column:::
       [![Java 向け Azure](../images/java-logo.png)](https://docs.microsoft.com/azure/developer/java/)<br>
        **[Azure での Java の概要](https://docs.microsoft.com/azure/developer/java/)**<br>
        Java 開発者向けのこれらのチュートリアルとツールを使用して、クラウド向けのアプリを構築します。
    :::column-end:::
    :::column:::
       [![PowerShell](../images/powershell.png)](https://docs.microsoft.com/powershell/)<br>
        **[PowerShell の概要](https://docs.microsoft.com/powershell/)**<br>
        PowerShell、コマンドライン シェル、スクリプト言語を使用して、クロスプラットフォームのタスク自動化と構成管理を行います。
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
        [WinGet のインストール (パブリック プレビュー)](https://docs.microsoft.com/windows/package-manager/winget/#install-winget)
    :::column-end:::
    :::column:::
       [![PowerToys](../images/powertoys.png)](https://github.com/microsoft/PowerToys)<br>
        **[Windows PowerToys](https://github.com/microsoft/PowerToys)**<br>
        この一連のパワー ユーザー ユーティリティを使用して、生産性が向上するように Windows のエクスペリエンスを調整して合理化します。<br>
        [PowerToys のインストール (パブリック プレビュー)](https://github.com/microsoft/PowerToys#installing-and-running-microsoft-powertoys)
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

## <a name="run-windows-and-linux"></a>Windows と Linux を実行する

Linux 用 Windows サブシステム (WSL) を使用することにより、開発者は Windows と並列で Linux オペレーティング システムを実行できるようになります。 これらは同じハード ドライブを使用するため (お互いのファイルにアクセスすることが可能)、クリップボードではこれらの間での自然なコピーと貼り付けがサポートされ、デュアル ブートは必要ありません。 WSL では BASH を使用することができ、Mac ユーザーにとって慣れ親しんだ環境が提供されます。
- 詳細については、[WSL ドキュメント](https://docs.microsoft.com/windows/wsl)か [Channel 9 の WSL についての動画](https://channel9.msdn.com/Search?term=wsl&lang-en=true)を参照してください。

> [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/What-can-I-do-with-WSL--One-Dev-Question/player?format=ny]

また Windows ターミナルを使用して、あらゆる好みのコマンド ライン ツールを、複数タブを使用する同じウィンドウで、または複数のペインで開くこともできます。それには、PowerShell、Windows コマンド プロンプト、Ubuntu、Debian、Azure CLI、Oh-my-Zsh、Git Bash が含まれ、これらすべてを開くこともできます。

- 詳細については、[Windows ターミナルのドキュメント](https://docs.microsoft.com/windows/terminal)や [Channel 9 の WT についての動画](https://channel9.msdn.com/Search?term=windows%20terminal&lang-en=true)を参照してください。

> [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/What-are-the-main-features-of-the-new-Terminal--One-Dev-Question/player?format=ny]

## <a name="transitioning-between-mac-and-windows"></a>Mac と Windows の間での移行

[Mac と Windows (または Linux 用 Windows サブシステム) 開発環境間の移行に関するガイド](https://docs.microsoft.com/windows/dev-environment/mac-to-windows)を参照してください。 次のような違いをマップするのに役立ちます。

* [キーボード ショートカット](https://docs.microsoft.com/windows/dev-environment/mac-to-windows#keyboard-shortcuts)
* [トラックパッドのショートカット](https://docs.microsoft.com/windows/dev-environment/mac-to-windows#trackpad-shortcuts)
* [ターミナルとシェルのツール](https://docs.microsoft.com/windows/dev-environment/mac-to-windows#terminal-and-shell)
* [アプリとユーティリティ](https://docs.microsoft.com/windows/dev-environment/mac-to-windows#apps-and-utilities)

![オフィスの画像](../images/flashy-office3.png)

## <a name="additional-resources"></a>その他の資料

* [ワークフローを改善するためのヒント](./tips.md)
* [Mac から Windows に切り替えた開発者のストーリー](./dev-stories.md)
* [人気のあるチュートリアル、コース、コード サンプル](./tutorials.md)
* [Microsoft Game Stack のドキュメント](https://docs.microsoft.com/gaming/)
