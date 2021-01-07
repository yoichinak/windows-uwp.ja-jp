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
ms.openlocfilehash: 578bc10b26dfe04384ca8e8174e36497c20896cb
ms.sourcegitcommit: 48702934676ae366fd46b7d952396c5e2fb2cbbe
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/06/2021
ms.locfileid: "97927815"
---
# <a name="set-up-your-development-environment-on-windows-10"></a>Windows 10 で開発環境を設定する

このガイドでは、Windows または Linux 用 Windows サブシステムを使用して開発するために必要な言語とツールのインストールとセットアップを始められるようにします。

## <a name="development-paths"></a>開発パス

:::row:::
    :::column:::
       [![JavaScrip NodeJS アイコン](../images/nodejs-logo.png)](../nodejs/index.yml)<br>
        **[NodeJS の概要](../nodejs/index.yml)**<br>
        Windows または Linux 用 Windows サブシステムで、NodeJS をインストールし、開発環境をセットアップします。
    :::column-end:::
    :::column:::
       [![Python アイコン](../images/python-logo.png)](../python/index.yml)<br>
        **[Python の概要](../python/index.yml)**<br>
        Windows または Linux 用 Windows サブシステムで、Python をインストールし、開発環境をセットアップします。
    :::column-end:::
    :::column:::
       [![Android アイコン](../images/android-logo.png)](/windows/android)<br>
        **[Android の概要](/windows/android)**<br>
        Android Studio をインストールするか、Xamarin、React、Cordova などのクロスプラットフォーム ソリューションを選択して、Windows で開発環境をセットアップします。
    :::column-end:::
    :::column:::
       [![Windows デスクトップ アイコン](../images/windows-logo.png)](../apps/index.yml)<br>
        **[Windows デスクトップの概要](../apps/index.yml)**<br>
        UWP、Win32、WPF、Windows フォームを使用して Windows 10 用のデスクトップ アプリの構築を始めるか、MSIX と XAML Island を使用して既存のデスクトップ アプリの更新とデプロイを始めます。
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
       [![C / C++](../images/c-logo.png)](/cpp/)<br>
        **[C++ と C の概要](/cpp/)**<br>
        C++、C、アセンブリを使用して、アプリ、サービス、ツールを開発します。
    :::column-end:::
    :::column:::
       [![C# アイコン](../images/csharp-logo.png)](/dotnet/csharp/)<br>
        **[C# の概要](/dotnet/csharp/)**<br>
        C# と .NET Core を使用してアプリを構築します。
    :::column-end:::
    :::column:::
       [![Docker Desktop for Windows のアイコン](../images/docker-logo.png)](../dev-environment/docker/overview.md)<br>
        **[Docker Desktop for Windows の概要](../dev-environment/docker/overview.md)**<br>
        Visual Studio、VS Code、.NET、Linux 用 Windows サブシステムや様々な Azure サービスのサポートを活用して、リモート開発用コンテナーを作成します。
    :::column-end:::
    :::column:::
       [![PowerShell アイコン](../images/powershell.png)](/powershell/)<br>
        **[PowerShell の概要](/powershell/)**<br>
        PowerShell、コマンドライン シェル、スクリプト言語を使用して、クロスプラットフォームのタスク自動化と構成管理を行います。
    :::column-end:::
:::row-end:::

## <a name="tools-and-platforms"></a>ツールとプラットフォーム

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
        包括的なパッケージ マネージャーである winget.exe クライアントをコマンド ラインで使用して、Windows 10 にアプリケーションをインストールします。<br>
        [Windows パッケージ マネージャー (パブリック プレビュー) のインストール](../package-manager/winget/index.md#install-winget)
    :::column-end:::
    :::column:::
       [![PowerToys アイコン](../images/powertoys.png)](https://github.com/microsoft/PowerToys)<br>
        **[Windows PowerToys](../PowerToys/index.md)**<br>
        この一連のパワー ユーザー ユーティリティを使用して、生産性が向上するように Windows のエクスペリエンスを調整して合理化します。<br>
        [PowerToys をインストールする](https://github.com/microsoft/PowerToys#installing-and-running-microsoft-powertoys)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
       [![VS Code アイコン](../images/Vscode.png)](https://code.visualstudio.com/docs)<br>
        **[VS Code](https://code.visualstudio.com/docs)**<br>
        JavaScript、TypeScript、Node.js、拡張機能のリッチなエコシステム (C++、C#、Java、Python、PHP、Go)、ランタイム (.NET や Unity など) のサポートが組み込まれている軽量のソース コード エディター。<br>
        [VS Code をインストールする](https://code.visualstudio.com/download)
    :::column-end:::
    :::column:::
       [![Visual Studio アイコン](../images/visualstudio.png)](/visualstudio/windows/)<br>
        **[Visual Studio](/visualstudio/windows/)**<br>
        コンパイラ、IntelliSense コード補完、その他多くの機能が含まれ、コードの編集、デバッグ、ビルドと、アプリの発行を行うことができる統合開発環境。<br>
        [Visual Studio をインストールする](/visualstudio/install/install-visual-studio)
    :::column-end:::
    :::column:::
       [![Azure アイコン](../images/Azure.png)](/azure/guides/developer/azure-developer-guide)<br>
        **[Azure](/azure/guides/developer/azure-developer-guide)**<br>
        既存のアプリをホストし、新しい開発を効率化するための完全なクラウド プラットフォーム。 Azure サービスには、アプリの開発、テスト、デプロイ、管理に必要なすべてのものが統合されています。<br>
        [Azure アカウントを設定する](https://azure.microsoft.com/free/)
    :::column-end:::
    :::column:::
       [![.NET アイコン](../images/net.png)](https://dotnet.microsoft.com/)<br>
        **[.NET](/dotnet/standard/get-started/)**<br>
        Web、モバイル、デスクトップ、ゲーム、IoT、クラウド、マイクロサービスなど、あらゆる種類のアプリを構築するためのツールとライブラリが含まれるオープンソースの開発プラットフォーム。<br>
        [.NET をインストールする](https://dotnet.microsoft.com/download)
    :::column-end:::
:::row-end:::

<br>

## <a name="run-windows-and-linux"></a>Windows と Linux を実行する

Linux 用 Windows サブシステム (WSL) を使用することにより、開発者は Windows と並列で Linux オペレーティング システムを実行できるようになります。 これらは同じハード ドライブを使用するため (お互いのファイルにアクセスすることが可能)、クリップボードではこれらの間での自然なコピーと貼り付けがサポートされ、デュアル ブートは必要ありません。 WSL では BASH を使用することができ、Mac ユーザーにとって慣れ親しんだ環境が提供されます。
- 詳細については、[WSL ドキュメント](/windows/wsl)か [Channel 9 の WSL についての動画](https://channel9.msdn.com/Search?term=wsl&lang-en=true)を参照してください。

> [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/What-can-I-do-with-WSL--One-Dev-Question/player?format=ny]

また Windows ターミナルを使用して、あらゆる好みのコマンド ライン ツールを、複数タブを使用する同じウィンドウで、または複数のペインで開くこともできます。それには、PowerShell、Windows コマンド プロンプト、Ubuntu、Debian、Azure CLI、Oh-my-Zsh、Git Bash が含まれ、これらすべてを開くこともできます。

詳細については、[Windows ターミナルのドキュメント](/windows/terminal)や [Channel 9 の Windows ターミナルについての動画](https://channel9.msdn.com/Search?term=windows%20terminal&lang-en=true)をご覧ください。

> [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/What-are-the-main-features-of-the-new-Terminal--One-Dev-Question/player?format=ny]

## <a name="transitioning-between-mac-and-windows"></a>Mac と Windows の間での移行

[Mac と Windows (または Linux 用 Windows サブシステム) 開発環境間の移行に関するガイド](./mac-to-windows.md)を参照してください。 次のような違いをマップするのに役立ちます。

- [キーボード ショートカット](./mac-to-windows.md#keyboard-shortcuts)
- [トラックパッドのショートカット](./mac-to-windows.md#trackpad-shortcuts)
- [ターミナルとシェルのツール](./mac-to-windows.md#command-line-shells-and-terminals)
- [アプリとユーティリティ](./mac-to-windows.md#apps-and-utilities)

![オフィスの画像](../images/flashy-office3.png)

## <a name="additional-resources"></a>その他の資料

- [ワークフローを改善するためのヒント](./tips.md)
- [Mac から Windows に切り替えた開発者のストーリー](./dev-stories.md)
- [人気のあるチュートリアル、コース、コード サンプル](./tutorials.md)
- [Microsoft Game Stack のドキュメント](/gaming/)
