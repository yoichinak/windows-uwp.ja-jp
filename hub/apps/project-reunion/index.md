---
description: Project Reunion と、これによって開発者が受ける利点、開発者に用意されているもの、およびフィードバックの提供方法について説明します。
title: Project Reunion
ms.topic: article
ms.date: 03/09/2021
keywords: windows win32, デスクトップ開発, project reunion
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: 9e6ff54d8605457dd5b29735f29405e9d0302fb7
ms.sourcegitcommit: dacbb7eef2cfffd7a8639e3a24ebda7b4eefae38
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/26/2021
ms.locfileid: "105616795"
---
# <a name="build-desktop-windows-apps-with-project-reunion-05-preview-march-2021"></a>Project Reunion 0.5 Preview を使用してデスクトップ Windows アプリをビルドする (2021 年 3 月)

Project Reunion は、Windows アプリ開発プラットフォームの次世代の新しい開発者向けコンポーネントとツールのセットです。 Project Reunion によって提供される API とツールの統合セットは、対象の幅広い Windows 10 OS バージョン上のどのデスクトップ アプリからでも一貫した方法で使用できます。 

Project Reunion は、既存のデスクトップ Windows アプリプラット フォームや .NET (Windows フォームと WPF を含む)、C++/Win32 などのフレームワークから置き換わるものではありません。 これは、開発者がこれらのプラットフォーム間で利用できる API とツールの共通セットにより、これらの既存のプラットフォームを補完するものです。

> [!NOTE]
> Project Reunion 0.5 Preview は、開発者向けのプレビューです。 このリリースをご自身の開発環境で試すことをお勧めします。 ただし、Project Reunion は、現在から 1.0 リリースまでの間にさまざまな方法で変更されることに注意してください。 Project Reunion 0.5 Preview は、実稼働環境で使用されているアプリには対応していません。 **Project Reunion** は、今後のリリースで変更される可能性のあるコード ネームです。

## <a name="benefits-of-project-reunion-for-windows-app-developers"></a>Windows アプリ開発者にとっての Project Reunion の利点

Project Reunion は、OS から切り離され、NuGet パッケージを開始て開発者にリリースされる、さまざまな Windows API のセットを提供します。 Project Reunion は、Windows SDK に代わるものではありません。 Windows SDK は引き続き機能します。また、OS および Windows SDK リリースを通じて配信される API 経由で引き続き進化する Windows のコア コンポーネントが多数あります。 開発者は Project Reunion を自身のペースで採用することをお勧めします。

#### <a name="unified-api-surface-across-desktop-app-platforms"></a>デスクトップ アプリ プラットフォームの Unified API サーフェス

デスクトップ Windows アプリを作成する開発者は、複数のアプリ プラットフォームおよびフレームワークから選択する必要があります。 各プラットフォームには、他のプラットフォームを使用して構築されるアプリで使用できる多くの機能と API が用意されていますが、一部の機能と API は特定のプラットフォームでのみ使用できます。 Project Reunion により、すべてのデスクトップ Windows 10 アプリの Windows API へのアクセスが統合されます。 どのアプリ モデルを選択しても、Project Reunion で利用できる同じ Windows API セットにアクセスできます。

Microsoft は徐々に、異なるアプリ モデル間の相違をなくすように Project Reunion に投資する予定です。 Project Reunion には、WinRT API とネイティブ C API の両方が含まれます。

#### <a name="consistent-support-across-windows-10-versions"></a>Windows 10 バージョン間での一貫したサポート

Windows API が新しい OS バージョンにより進化し続けるのに伴い、開発者は[バージョン アダプティブ コード](/windows/uwp/debug-test-perf/version-adaptive-code)などの手法を使用して、バージョンでのすべての相違を考慮して、アプリケーションの対象ユーザーにアピールする必要があります。 これにより、コードと開発エクスペリエンスが複雑になる可能性があります。

Project Reunion API は、Windows 10 バージョン 1809 以降のすべてのバージョンの Windows 10 で動作します。 つまり、お客様が Windows 10 バージョン 1809 以降のバージョンを使用している限り、Project Reunion の新しい API と機能をリリースと同時に使用でき、バージョンに適応したコードを記述する必要がありません。

#### <a name="faster-release-cadence"></a>短いリリース サイクル

新しい Windows API と機能は、通常、年に 1 回または 2 回のリリース サイクルで発生する OS リリースに関連付けられています。 Project Reunion の更新プログラムはさらに短いサイクルで提供されるため、Windows 開発プラットフォームでイノベーションが起きるとすぐに、より早く、より迅速に利用できるようになります。

## <a name="components-available-with-project-reunion"></a>Project Reunion で使用可能なコンポーネント

Project Reunion 0.5 Preview には、次のコンポーネントが含まれます。

| コンポーネント | 説明 |
|---------|-------------|
| [Windows UI ライブラリ 3](../winui/winui3/index.md) | Windows UI ライブラリ (WinUI) 3 は、デスクトップ (.NET と C++/Win32) と UWP アプリの両方に対する次世代の Windows ユーザー エクスペリエンス (UX) プラットフォームです。 このリリースには、[WinUI ベースのユーザー インターフェイスを備えたアプリのビルド](..\winui\winui3\index.md#create-winui-projects)を始めるときに役立つ Visual Studio プロジェクト テンプレートと、WinUI ライブラリが収められた NuGet パッケージが含まれます。  |
| [MRT Core を使用してリソースを管理する](mrtcore/mrtcore-overview.md) | MRT Core から、アプリで使用されるリソースを読み込んで管理するための API が提供されます。 MRT Core は、最新の [Windows リソース管理システム](/windows/uwp/app-resources/resource-management-system)の簡素化されたバージョンです。 |
| [DWriteCore を使用してテキストを表示する](dwritecore.md) | DWriteCore を使用すると、デバイスに依存しないテキスト レイアウト システム、ハードウェアアクセラレーション テキスト、マルチフォーマット テキスト、および幅広い言語サポートなど、テキスト レンダリングの現在のすべての DirectWrite 機能にアクセスできます。  |

他のコンポーネントを Project Reunion に取り込むための将来的な計画について詳しくは、[こちら](https://github.com/microsoft/ProjectReunion/blob/master/docs/README.md)をご覧ください。

## <a name="set-up-your-development-environment"></a>開発環境を設定する

1. 開発用コンピューターに Windows 10 Version 1809 (ビルド 17763) 以降の OS バージョンがインストールされていることを確認します。

2. まだインストールしていない場合は、[Visual Studio 2019 バージョン 16.10 Preview](https://visualstudio.microsoft.com/vs/preview/) (またはそれ以降) をインストールします。 

    > [!NOTE]
    > Visual Studio 2019 バージョン 16.9 では Project Reunion もサポートされていますが、WinUI 3 ツール機能はサポートされていません。 WinUI 3 ツールのサポートの詳細については、「Windows UI ライブラリ 3 - Project Reunion 0.5 Preview (2021 年 3 月)」を参照してください。

    Visual Studio をインストールする際、次のコンポーネントを含める必要があります。
    - インストール ダイアログの **[ワークロード]** タブで、 **[ユニバーサル Windows プラットフォーム開発]** が選択されていることを確認します。
    - インストール ダイアログの **[個別のコンポーネント]** タブで、 **[SDK、ライブラリ、およびフレームワーク]** セクションで **[Windows 10 SDK (10.0.19041.0)]** が選択されていることを確認します。

    .NET アプリをビルドするには、次のコンポーネントも含める必要があります。
    - インストール ダイアログの **[ワークロード]** タブで、 **[.NET デスクトップ開発]** が選択されていることを確認します。

    C++ アプリをビルドするには、次のコンポーネントも含める必要があります。
    - インストール ダイアログの **[ワークロード]** タブで、 **[C++ によるデスクトップ開発]** が選択されていることを確認します。
    - インストール ダイアログの右側の **[インストールの詳細]** ウィンドウの **[ユニバーサル Windows プラットフォーム開発]** セクションで、 **[C++ (v142) ユニバーサル Windows プラットフォーム ツール]** オプション コンポーネントが選択されていることを確認します。

3. 以前の WinUI 3 プレビュー リリースで、既に [WinUI 3 Preview 拡張機能](https://marketplace.visualstudio.com/items?itemName=Microsoft-WinUI.WinUIProjectTemplates)をインストールしていた場合は、拡張機能をアンインストールします。 拡張機能をアンインストールする方法の詳細については、[Visual Studio の拡張機能の管理](/visualstudio/ide/finding-and-using-visual-studio-extensions)に関するページを参照してください。

4. **nuget.org** に対して NuGet パッケージ ソースがシステムで有効になっていることを確認します。詳細については、「[一般的な NuGet 構成](/nuget/consume-packages/configuring-nuget-behavior)」を参照してください。

5. Visual Studio 2019 で、 **[拡張機能]**  >  **[拡張機能の管理]** をクリックし、 **[Project Reunion]** を探して、**Project Reunion** 拡張機能をインストールします。 または、[Project Reunion 拡張機能](https://marketplace.visualstudio.com/items?itemName=ProjectReunion.MicrosoftProjectReunion) を Visual Studio Marketplace から直接ダウンロードしてインストールすることもできます。 VSIX パッケージを Visual Studio に追加する方法については、「[Visual Studio の拡張機能を管理する](/visualstudio/ide/finding-and-using-visual-studio-extensions)」を参照してください。

6. ライブ ビジュアル ツリー、ホット リロード、ライブ プロパティ エクスプローラーなどの WinUI 3 のツールを使用するには、[こちらの手順](https://github.com/microsoft/microsoft-ui-xaml/issues/4140)で説明されているように、Visual Studio のプレビュー機能で WinUI 3 ツールを有効にする必要があります。

## <a name="get-started-developing-with-project-reunion"></a>Project Reunion を使用して開発を開始する

Project Reunion 0.5 Preview は、Project Reunion 拡張機能に含まれている WinUI 3 プロジェクト テンプレートを使用して作成される新しいアプリで使用できます。また、既存のプロジェクトでは、NuGet パッケージをインストールすると、Project Reunion コンポーネントを使用できます。

> [!NOTE]
> Project Reunion 0.5 Preview では、MSIX パッケージ化されたアプリのみがサポートされます。

#### <a name="create-a-new-project-that-uses-project-reunion"></a>Project Reunion を使用して新しいプロジェクトを作成する

Visual Studio 2019 の Project Reunion 0.5 Preview 拡張機能には、WinUI 3 ベースの UI レイヤーでプロジェクトを生成し、他のすべての Project Reunion API へのアクセスを提供するプロジェクト テンプレートが含まれます。 これらのテンプレートを使用して、MSIX パッケージ化されたデスクトップ アプリ (C#/.NET 5 または C++/WinRT)、または Project Reunion を使用する UWP アプリをビルドできます。 使用可能なプロジェクト テンプレートの詳細については、「[WinUI プロジェクトを作成する](..\winui\winui3\index.md#create-winui-projects)」を参照してください。

Project Reunion 0.5 Preview を使用する新しいプロジェクトを作成するには:

1. 次の記事の手順に従います。

    - [デスクトップ アプリ用の WinUI 3 の概要](..\winui\winui3\get-started-winui3-for-desktop.md)
    - [UWP アプリ用の WinUI 3 の概要](..\winui\winui3\get-started-winui3-for-uwp.md)

2. プロジェクトを作成した後、通常はデスクトップ アプリと UWP アプリで使用できる他のすべての Windows および .NET API に加えて、次のプロジェクトの Project Reunion API とコンポーネントにアクセスできます。

    - [Windows UI ライブラリ 3](../winui/winui3/index.md)
    - [リソース MRT Core を管理する](mrtcore/mrtcore-overview.md)
    - [DWriteCore を使用してテキストを表示する](dwritecore.md)

新しいプロジェクトで Project Reunion を使用していることを確認するには、**ソリューション エクスプローラー** でプロジェクトの **[依存関係]**  >  **[パッケージ]** ノードを展開します。 次の図のように、このノードの下に複数の **Microsoft.ProjectReunion** パッケージが表示されるはずです。

![[ソリューション エクスプローラー] ペインの Project Reunion パッケージのスクリーンショット](images/reunion-packages.png)

#### <a name="use-project-reunion-in-an-existing-project"></a>既存のプロジェクトで Project Reunion を使用する

Project Reunion を使用する既存のデスクトップ プロジェクト (C#/.NET 5 または C++/WinRT) がある場合は、Project Reunion 0.5 Preview NuGet パッケージをプロジェクトにインストールできます。 

> [!NOTE]
> このシナリオでは、プロジェクトの Project Reunion に含まれる WinUI 3 以外のコンポーネントのみを使用できます。 WinUI 3 を使用するには、前のセクションで説明したように、WinUI 3 プロジェクト テンプレートのいずれかを使用して新しいプロジェクトを作成する必要があります。

1. Visual Studio 2019 で、既存のデスクトップ プロジェクト (C#/.NET 5 または C++/WinRT) または UWP プロジェクトを開きます。

2. [パッケージ参照](/nuget/consume-packages/package-references-in-project-files)が有効になっていることを確認します。

    1. Visual Studio で、 **[ツール] -> [NuGet パッケージ マネージャー] -> [パッケージ マネージャー設定]** の順にクリックします。
    2. **[既定のパッケージ管理形式]** に **[PackageReference]** が選択されていることを確認します。

3. **ソリューション エクスプローラー** でプロジェクトを右クリックし、 **[NuGet パッケージの管理]** を選択します。

4. **[NuGet パッケージ マネージャー]** ウィンドウで、 **[参照]** タブを選択して、`Microsoft.ProjectReunion` を検索します。

5. **Microsoft.ProjectReunion** パッケージが見つかったら、 **[NuGet パッケージ マネージャー]** ウィンドウの右側ペインで、 **[インストール]** をクリックします。

6. パッケージをインストールした後、プロジェクトで次の Project Reunion API とコンポーネントを使用することができます。

    - [リソース MRT Core を管理する](mrtcore/mrtcore-overview.md)
    - [DWriteCore を使用してテキストを表示する](dwritecore.md)

#### <a name="deploying-apps-that-use-project-reunion"></a>Project Reunion を使用するアプリを展開する

現在、Project Reunion を使用するアプリは、[MSIX](/windows/msix) を使用してパッケージ化する必要があります。 既定では、Visual Studio の Project Reunion 拡張機能で提供される WinUI プロジェクト テンプレートのいずれかを使用してプロジェクトを作成すると、アプリを MSIX パッケージ化するように構成された [Windows アプリケーション パッケージ プロジェクト](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)がプロジェクトに含まれます。 このプロジェクトを構成して、アプリの MSIX パッケージを作成する方法の詳細については、「[Visual Studio でのデスクトップまたは UWP アプリのパッケージ化](/windows/msix/package/packaging-uwp-apps)」を参照してください。

アプリの MSIX パッケージを作成したら、他のコンピューターに展開するためのオプションがいくつかあります。 詳細については、「[MSIX の展開を管理する](/windows/msix/desktop/managing-your-msix-deployment-overview)」を参照してください。

> [!NOTE]
> Project Reunion 0.5 Preview を使用するアプリを Microsoft Store に発行することはできません。

## <a name="samples"></a>サンプル

現在、次の Project Reunion サンプルを利用できます。

- [DWriteCore ギャラリーのサンプル](https://github.com/microsoft/Project-Reunion-Samples/tree/main/DWriteCore/DWriteCoreGallery):このサンプル アプリケーションでは、[DWriteCore](dwritecore.md) API が示されています。
- [MRT Core サンプル](https://github.com/microsoft/Project-Reunion-Samples/tree/main/MrtCore):このサンプル アプリケーションでは、[MRT Core](mrtcore/mrtcore-overview.md) API が示されています。
- [Hello World サンプル](https://github.com/microsoft/Project-Reunion-Samples/tree/main/HelloWorld/reunioncppdesktopsampleapp):このサンプルでは、Project Reunion NuGet パッケージとの基本的な統合を示します。

## <a name="limitations-and-known-issues"></a>制限事項と既知の問題

- このリリースは、実稼働環境で使用されているアプリには対応していません。 バグ、制限事項、およびその他の問題があることを想定してください。
- このリリースは、MSIX パッケージ化されたデスクトップ アプリ (C#/.NET 5 または C++/Win32) でのみ使用できます。 パッケージ化されていないデスクトップ アプリでは使用できません。
- また、[WinUI 3 のツールの制限](..\winui\winui3\index.md#developer-tools)は、Project Reunion 0.5 Preview を使用するどのプロジェクトにも適用されます。

## <a name="developer-roadmap"></a>開発者向けロードマップ

最新の Project Reunion プランについては、[GitHub のページ](https://github.com/microsoft/ProjectReunion)を参照してください。

## <a name="give-feedback-and-contribute"></a>フィードバックと投稿の送信

Microsoft は、Project Reunion をオープン ソース プロジェクトとしてビルドしています。 Microsoft が Project Reunion をどのように実現しようとしているかについて詳しくは、[GitHub のページ](https://github.com/microsoft/ProjectReunion)をご覧ください。開発プロセスの一部を覗けるよう案内しています。 質問、ディスカッションの開始、機能の提案については、[投稿のガイド](https://github.com/microsoft/ProjectReunion/blob/master/docs/contributor-guide.md)をご覧ください。 Project Reunion により開発者の皆さんが最大のメリットを得られることを見届けたいと思っています。
