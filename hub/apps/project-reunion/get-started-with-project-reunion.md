---
description: この記事では、開発用コンピューターに Visual Studio 2019 の Project レユニオン拡張機能をインストールし、新規または既存のプロジェクトで Project レユニオンを使用する手順について説明します。
title: Project Reunion の概要
ms.topic: article
ms.date: 03/19/2021
keywords: windows win32, デスクトップ開発, project reunion
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: e3a5c891bb6bbeafa59baf933a4d8886e8430a16
ms.sourcegitcommit: 2772d98e0c8f039e7356f03fd09054dca1022c3a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/21/2021
ms.locfileid: "107838694"
---
# <a name="get-started-with-project-reunion"></a>Project Reunion の概要

この記事では、開発用コンピューターに Visual Studio 2019 の Project レユニオン拡張機能をインストールし、新規または既存のプロジェクトで Project レユニオンを使用する手順について説明します。 Project レユニオンをインストールして使用する前に、 [制限事項と既知の問題](index.md#limitations-and-known-issues)を参照してください。

> [!NOTE]
> 以前の preview またはリリースバージョンの Project レユニオンまたは WinUI 3 を使用してプロジェクトを作成した場合は、 [最新のリリースを使用するようにプロジェクトを更新](update-existing-projects-to-the-latest-release.md)できます。

## <a name="set-up-your-development-environment"></a>開発環境を設定する

1. 開発用コンピューターに Windows 10 Version 1809 (ビルド 17763) 以降の OS バージョンがインストールされていることを確認します。

2. まだインストールしていない場合は、[Visual Studio 2019 バージョン 16.10 Preview](https://visualstudio.microsoft.com/vs/preview/) (またはそれ以降) をインストールします。

    > [!NOTE]
    > Visual Studio 2019、バージョン16.9 では、プロジェクトのレユニオンもサポートしていますが、すべての WinUI 3 ツール機能はサポートされていません。 WinUI 3 ツールのサポートの詳細については、「 [WINDOWS UI Library 3-Project レユニオン 0.5](../winui/winui3/index.md)」を参照してください。

    Visual Studio をインストールする際、次のコンポーネントを含める必要があります。
    - インストール ダイアログの **[ワークロード]** タブで、 **[ユニバーサル Windows プラットフォーム開発]** が選択されていることを確認します。
    - インストール ダイアログの **[個別のコンポーネント]** タブで、 **[SDK、ライブラリ、およびフレームワーク]** セクションで **[Windows 10 SDK (10.0.19041.0)]** が選択されていることを確認します。

    .NET アプリをビルドするには、次のコンポーネントも含める必要があります。
    - インストール ダイアログの **[ワークロード]** タブで、 **[.NET デスクトップ開発]** が選択されていることを確認します。

    C++ アプリをビルドするには、次のコンポーネントも含める必要があります。
    - インストール ダイアログの **[ワークロード]** タブで、 **[C++ によるデスクトップ開発]** が選択されていることを確認します。
    - インストール ダイアログの右側の **[インストールの詳細]** ウィンドウの **[ユニバーサル Windows プラットフォーム開発]** セクションで、 **[C++ (v142) ユニバーサル Windows プラットフォーム ツール]** オプション コンポーネントが選択されていることを確認します。

3. 以前 [に Visual Studio 用の WinUI 3 Preview 拡張機能](https://marketplace.visualstudio.com/items?itemName=Microsoft-WinUI.WinUIProjectTemplates)をインストールした場合は、拡張機能をアンインストールします。 拡張機能をアンインストールする方法の詳細については、[Visual Studio の拡張機能の管理](/visualstudio/ide/finding-and-using-visual-studio-extensions)に関するページを参照してください。

4. システムで、公式の NuGet サービスインデックスが有効になっている NuGet パッケージソースがにあることを確認し `https://api.nuget.org/v3/index.json` ます。 

    1. Visual Studio で、[**ツール**] [  ->  **NuGet パッケージマネージャー**] [  ->  **パッケージマネージャーの設定**] を選択し、[**オプション**] ダイアログを開きます。 
    2. [ **オプション** ] ダイアログボックスの左ペインで、[ **パッケージソース** ] タブを選択し、ソース URL としてを指す **nuget.org** のパッケージソースがあることを確認し `https://api.nuget.org/v3/index.json` ます。 詳細については、「 [Common NuGet の構成](/nuget/consume-packages/configuring-nuget-behavior)」を参照してください。

5. Project レユニオン 0.5 extension for Visual Studio をダウンロードしてインストールします。 拡張機能には、デスクトップ (C#/.NET 5 または C++/WinRT) アプリ用と UWP アプリ用の2つのバージョンがあります。

    Desktop (C#/.NET 5 または C++/WinRT) アプリで Project レユニオンを使用するには、次のようにします。
    - Visual Studio 2019 で、 **[拡張機能]**  >  **[拡張機能の管理]** をクリックし、 **[Project Reunion]** を探して、**Project Reunion** 拡張機能をインストールします。
    - または、Visual Studio Marketplace から直接 [Project レユニオン0.5 拡張機能](https://marketplace.visualstudio.com/items?itemName=ProjectReunion.MicrosoftProjectReunion) をダウンロードしてインストールすることもできます。

    UWP アプリで Project レユニオンを使用するには、運用環境でサポートされていない拡張機能のプレビューバージョンをインストールする必要があります。
    - Project レユニオン VSIX の既存のバージョンをアンインストールします。
    - Visual Studio 2019 で、[**拡張** 機能] [拡張機能の管理] の順にクリックし、左下  >  隅にある [**拡張機能の設定の変更**] をクリックします。 以前のバージョンをインストールする前に、すべてのユーザーにインストールされているパッケージの自動更新をオフにします。
    - [Project レユニオン 0.5 Preview 拡張機能](https://download.microsoft.com/download/9/9/8/9981a84b-8fd8-4645-9dce-c62761601f17/ProjectReunion.Extension.vsix)をダウンロードしてインストールします。

    Visual Studio に VSIX パッケージを追加する手順については、「 [Visual studio の拡張機能の管理](/visualstudio/ide/finding-and-using-visual-studio-extensions)」を参照してください。

    ![インストールされている Project レユニオン拡張機能のスクリーンショット](images/reunion-extension-install.png)

6. Live Visual Tree、ホットリロード、ライブプロパティエクスプローラー (Visual Studio 2019 16.10 Preview) などの WinUI 3 ツールを使用するには、Visual Studio Preview 機能で WinUI 3 ツールを有効にする必要があります。 手順については、「 [VS 16.9 Preview 4 の WinUI 3 の UI ツールを有効にする方法](https://github.com/microsoft/microsoft-ui-xaml/issues/4140)」を参照してください。

## <a name="create-a-new-project-that-uses-project-reunion"></a>Project Reunion を使用して新しいプロジェクトを作成する

Project レユニオン 0.5 extensions for Visual Studio 2019 (デスクトップアプリの拡張機能と UWP アプリのプレビュー拡張機能を含む) は、WinUI 3 ベースの UI レイヤーでプロジェクトを生成し、他のすべての Project レユニオン Api へのアクセスを提供するプロジェクトテンプレートを提供します。 使用可能なプロジェクトテンプレートの詳細については、「 [Visual Studio の WinUI 3 プロジェクトテンプレート](..\winui\winui3\winui-project-templates-in-visual-studio.md)」を参照してください。

> [!NOTE]
> デスクトップ (C#/.NET 5 および C++/WinRT) プロジェクトテンプレートは、運用環境での使用がサポートされています。 UWP プロジェクトテンプレートは開発者プレビューとしてのみ使用でき、運用環境用のアプリの構築には使用できません。

Project レユニオン0.5 を使用する新しいプロジェクトを作成するには、次のようにします。

1. 次の記事の手順に従います。

    - [デスクトップ アプリ用の WinUI 3 の概要](..\winui\winui3\get-started-winui3-for-desktop.md)
    - [UWP アプリ用の WinUI 3 の概要 (プレビュー)](..\winui\winui3\get-started-winui3-for-uwp.md)
    - [基本的な WinUI 3 デスクトップ アプリをビルドする](..\winui\winui3\desktop-build-basic-winui3-app.md)

2. プロジェクトを作成した後、通常はデスクトップ アプリと UWP アプリで使用できる他のすべての Windows および .NET API に加えて、次のプロジェクトの Project Reunion API とコンポーネントにアクセスできます。

    - [Windows UI ライブラリ 3](../winui/winui3/index.md)
    - [リソース MRT Core を管理する](mrtcore/mrtcore-overview.md)
    - [DWriteCore を使用してテキストを表示する](dwritecore.md)

新しいプロジェクトで Project Reunion を使用していることを確認するには、**ソリューション エクスプローラー** でプロジェクトの **[依存関係]**  >  **[パッケージ]** ノードを展開します。 次の図のように、このノードの下に複数の **Microsoft.ProjectReunion** パッケージが表示されるはずです。

![[ソリューション エクスプローラー] ペインの Project Reunion パッケージのスクリーンショット](images/reunion-packages.png)

## <a name="use-project-reunion-in-an-existing-project"></a>既存のプロジェクトで Project Reunion を使用する

Project レユニオンを使用する既存のプロジェクトがある場合は、project レユニオン 0.5 NuGet パッケージをプロジェクトにインストールできます。 このシナリオには [いくつかの制限](index.md#using-the-project-reunion-nuget-package-in-existing-projects)があります。

1. Visual Studio 2019 で、既存のデスクトップ プロジェクト (C#/.NET 5 または C++/WinRT) または UWP プロジェクトを開きます。

    > [!NOTE]
    > C#/.NET 5 desktop プロジェクトがある場合は、プロジェクトファイル内の **Targetframework** 要素が、 **NET 5.0-windows 10.0.19041.0** などの windows 10 固有の .net 5 モニカーに割り当てられていることを確認して Windows ランタイム api を呼び出すことができるようにします。 詳細については、[こちらのセクション](../../apps/desktop/modernize/desktop-to-uwp-enhance.md#net-5-use-the-target-framework-moniker-option)をご覧ください。

2. [パッケージ参照](/nuget/consume-packages/package-references-in-project-files)が有効になっていることを確認します。

    1. Visual Studio で、 **[ツール] -> [NuGet パッケージ マネージャー] -> [パッケージ マネージャー設定]** の順にクリックします。
    2. **[既定のパッケージ管理形式]** に **[PackageReference]** が選択されていることを確認します。

3. **ソリューション エクスプローラー** でプロジェクトを右クリックし、 **[NuGet パッケージの管理]** を選択します。

4. **[NuGet パッケージ マネージャー]** ウィンドウで、 **[参照]** タブを選択して、`Microsoft.ProjectReunion` を検索します。

5. **Microsoft.ProjectReunion** パッケージが見つかったら、 **[NuGet パッケージ マネージャー]** ウィンドウの右側ペインで、 **[インストール]** をクリックします。

    ![Project レユニオン NuGet パッケージがインストールされているスクリーンショット](images/reunion-nuget-install.png)

6. **C#/.NET 5 プロジェクトの場合のみ**: プロジェクトレユニオン0.5 の最新の安定したリリースからすべての修正プログラムを受け取るには、プロジェクトファイルを更新して、.net SDK を最新バージョンに明示的に設定する必要があります。 詳細については、「 [.NET SDK のリファレンス](index.md#net-sdk-references)」を参照してください。

7. Microsoft の実行後、プロジェクトでは、次のようにプロジェクトを共同で使用することができます **。**

    - [リソース MRT Core を管理する](mrtcore/mrtcore-overview.md)
    - [DWriteCore を使用してテキストを表示する](dwritecore.md)

## <a name="samples"></a>サンプル

現在、次の Project Reunion サンプルを利用できます。

- [DWriteCore ギャラリーのサンプル](https://github.com/microsoft/Project-Reunion-Samples/tree/main/DWriteCore/DWriteCoreGallery):このサンプル アプリケーションでは、[DWriteCore](dwritecore.md) API が示されています。
- [MRT Core サンプル](https://github.com/microsoft/Project-Reunion-Samples/tree/main/MrtCore):このサンプル アプリケーションでは、[MRT Core](mrtcore/mrtcore-overview.md) API が示されています。
- [Hello World サンプル](https://github.com/microsoft/Project-Reunion-Samples/tree/main/HelloWorld/reunioncppdesktopsampleapp):このサンプルでは、Project Reunion NuGet パッケージとの基本的な統合を示します。
- [Xaml コントロールギャラリー](https://aka.ms/winui3/xcg): これは、動作中のすべての WinUI 3 コントロールを示すサンプルアプリです。 

## <a name="related-topics"></a>関連トピック

- [Project Reunion を使用してデスクトップ Windows アプリをビルドする](index.md)
- [Project Reunion を使用するアプリを展開する](deploy-apps-that-use-project-reunion.md)
