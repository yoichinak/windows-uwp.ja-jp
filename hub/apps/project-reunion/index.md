---
description: Project Reunion と、これによって開発者が受ける利点、開発者に用意されているもの、およびフィードバックの提供方法について説明します。
title: Project Reunion
ms.topic: article
ms.date: 01/07/2021
keywords: windows win32, デスクトップ開発, project reunion
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: de1115281859ba322a03e3bab7a8d0e4ef71440d
ms.sourcegitcommit: 044c75ea0c6fb3463a0150acdae1ff867dc05f29
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/07/2021
ms.locfileid: "97972126"
---
# <a name="build-windows-apps-with-project-reunion-prerelease"></a>Project Reunion による Windows アプリのビルド (プレリリース)

Project Reunion 0.1 プレリリースは、Windows アプリ開発プラットフォームの次世代の新しい開発者向けコンポーネントおよびツール セットのプレビュー版です。 Project Reunion により、さまざまな一連の対象の Windows 10 OS バージョン上のあらゆるアプリによって一貫した方法で使用できる、統合された API とツールのセットが提供されます。 Project Reunion は、UWP、ネイティブ Win32、.NET などの既存の Windows アプリ プラットフォームやフレームワークに代わるものではありません。 これは、開発者がこれらのプラットフォーム間で利用できる API とツールの共通セットにより、これらの既存のプラットフォームを補完するものです。

> [!NOTE]
> Project Reunion 0.1 プレリリースは、いち早く利用する開発者向けのプレビューです。 このリリースをご自身の開発環境で試すことをお勧めします。 ただし、Project Reunion は、最終リリースに向けて、今後さまざまな面で変更されることに注意してください。 Project Reunion 0.1 プレリリースは、実稼働環境で使用されているアプリには対応しません。 **Project Reunion** は、今後のリリースで変更される可能性のあるコード ネームです。

## <a name="goals-of-project-reunion"></a>Project Reunion の目的

Project Reunion は、OS から切り離され、NuGet パッケージを開始て開発者にリリースされる、さまざまな Windows API のセットを提供します。 Project Reunion は、Windows SDK に代わるものではありません。 Windows SDK は引き続き機能します。また、OS および Windows SDK リリースを通じて配信される API 経由で引き続き進化する Windows のコア コンポーネントが多数あります。 開発者は Project Reunion を自身のペースで採用することをお勧めします。

Project Reunion は、次の目標をサポートすることを目的としています。

#### <a name="unified-api-surface-across-different-types-of-windows-apps"></a>さまざまな種類の Windows API アプリ間で統一された API サーフェス

Windows アプリを作成する開発者は、複数のアプリ プラットフォームおよびフレームワークから選択する必要があります。 各プラットフォームには、他のプラットフォームを使用して構築されるアプリで使用できる多くの機能と API が用意されていますが、一部の機能と API は特定のプラットフォームでのみ使用できます。 Project Reunion は、すべての Windows 10 アプリの Windows API へのアクセスを統合します。 どのアプリ モデルを選択しても、Project Reunion で利用できる同じ Windows API セットにアクセスできます。

Microsoft は徐々に、異なるアプリ モデル間の相違をなくすように Project Reunion に投資する予定です。 Project Reunion には、WinRT API とネイティブ C API の両方が含まれます。

#### <a name="consistent-support-across-windows-10-versions"></a>Windows 10 バージョン間での一貫したサポート

Windows API が新しい OS バージョンにより進化し続けるのに伴い、開発者は[バージョン アダプティブ コード](/windows/uwp/debug-test-perf/version-adaptive-code)などの手法を使用して、バージョンでのすべての相違を考慮して、アプリケーションの対象ユーザーにアピールする必要があります。 これにより、コードと開発エクスペリエンスが複雑になる可能性があります。 Project Reunion を使用すると、さまざまな OS バージョンとすべての Windows 10 デバイス全体で Project Reunion API へのアクセスを統合できます。これにより、最新バージョンの Windows だけでなく、必要に応じて、より多くの開発者が更新プログラムを入手できるようにすることができます。 現在の計画では、Project Reunion は Windows 10 Version 1809 およびそれ以降のすべてのバージョンの Windows 10 に対応する予定です。

#### <a name="faster-release-cadence"></a>短いリリース サイクル

新しい Windows API と機能は、通常、年に 1 回または 2 回のリリース サイクルで発生する OS リリースに関連付けられています。 Project Reunion を使用すると、より頻繁かつアジャイルな方法で新しい実稼働品質の API と機能をリリースできます。

## <a name="get-started"></a>作業開始

Project Reunion 0.1 プレリリースには、次の機能領域用の新しい API が含まれています。

| 機能 | 説明 |
|---------|-------------|
| [MRT Core (Modern Resource Management System)](mrtcore/mrtcore-overview.md) | MRT Core は、Project Reunion の一部として配布される最新の [Windows リソース管理システム](/windows/uwp/app-resources/resource-management-system)の簡素化されたバージョンです。 |
| [DWriteCore](dwritecore.md) | DWriteCore は、Windows のバージョン (以前のバージョン Windows 8 まで) 上で実行される DirectWrite の一種で、クロスプラットフォームでの使用が可能になります。 |

他のコンポーネントを Project Reunion に取り込むための将来的な計画について詳しくは、[こちら](https://github.com/microsoft/ProjectReunion/blob/master/docs/README.md)をご覧ください。

> [!NOTE]
> Windows UI ライブラリ (WinUI)、MSIX、WebView2 など、他の特定の既存コンポーネントは既に OS から切り離されており、Project Reunion のガイドラインに従っています (たとえば、これらは Windows 10 Version 1809 以降の OS バージョンではサポートされています)。 ただし、これらの他のコンポーネントは、現在、Project Reunion NuGet パッケージには含まれていません。  

### <a name="set-up-your-development-environment"></a>開発環境を設定する

Project Reunion 0.1 プレリリースを試してみる場合、提供されている C++ サンプルのいずれかで始める必要があります。 これらのサンプルは、Project Reunion NuGet パッケージを使用するように事前構成されています。 このプレビューでは、独自のプロジェクトへの Project Reunion NuGet パッケージのインストールはサポートされていません。 サンプルのいずれかを使用するように開発環境を設定するには、次の手順に従います。

1. 開発用コンピューターに Windows 10 Version 1809 (ビルド 17763) 以降の OS バージョンがインストールされていることを確認します。

2. [Visual Studio 2019 バージョン 16.9 Preview 2](https://visualstudio.microsoft.com/vs/preview/) 以降をインストールします。 Visual Studio インストーラーで次の項目が選択されていることを確認します。
    - **[ワークロード]** タブで、次のワークロードが選択されていることを確認します。
        - **.NET デスクトップ開発**
        - **C++ によるデスクトップ開発**
        - **ユニバーサル Windows プラットフォーム開発** ( **[インストールの詳細]** ウィンドウで、このワークロードに対して **[C++ (v142) ユニバーサル Windows プラットフォーム ツール]** オプション コンポーネントが選択されていることも確認します)
    - **[個別のコンポーネント]** タブで、**SDK、ライブラリ、およびフレームワーク** セクションで **Windows 10 SDK (10.0.19041.0)** が選択されていることを確認します。

3. Visual Studio Marketplace から最新バージョンの [C++/WinRT Visual Studio 拡張機能 (VSIX)](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264) をインストールします。

4. **nuget.org** に対して NuGet パッケージ ソースがシステムで有効になっていることを確認します。詳細については、「[一般的な NuGet 構成](/nuget/consume-packages/configuring-nuget-behavior)」を参照してください。

5. [WinUI 3 Preview 3 VSIX パッケージ](https://aka.ms/winui3/preview3-download)をダウンロードしてインストールします。 この手順は、既に WinUI 3 を使用するように構成されている Hello World と MRT Core サンプルに対してのみ必要です。 VSIX パッケージを Visual Studio に追加する方法については、[Visual Studio 拡張機能を見つけて使用する方法](/visualstudio/ide/finding-and-using-visual-studio-extensions#install-without-using-the-manage-extensions-dialog-box)に関するページをご覧ください。

6. 次のサンプルを複製して調べます。
    - [DWriteCore ギャラリーのサンプル](https://github.com/microsoft/Project-Reunion-Samples/tree/main/DWriteCore/DWriteCoreGallery):このサンプル アプリケーションでは、[DWriteCore](dwritecore.md) API が示されています。
    - [MRT Core サンプル](https://github.com/microsoft/Project-Reunion-Samples/tree/main/MrtCore):このサンプル アプリケーションでは、[MRT Core](mrtcore/mrtcore-overview.md) API が示されています。
    - [Hello World サンプル](https://github.com/microsoft/Project-Reunion-Samples/tree/main/HelloWorld/reunioncppdesktopsampleapp):このサンプルでは、Project Reunion NuGet パッケージとの基本的な統合を示します。

## <a name="known-issues"></a>既知の問題

Project Reunion 0.1 プレリリースには、次の制限があります。

 - このリリースは、MSIX パッケージ C++/Win32 アプリにのみ対応しています。
 - このリリースでは、C# はサポートされていません。
 - このリリースでは、.NET 5 はサポートされていません。

## <a name="developer-roadmap"></a>開発者向けロードマップ

最新の Project Reunion プランについては、[GitHub のページ](https://github.com/microsoft/ProjectReunion)を参照してください。

## <a name="give-feedback-and-contribute"></a>フィードバックと投稿の送信

Microsoft は、Project Reunion をオープン ソース プロジェクトとしてビルドしています。 Microsoft が Project Reunion をどのように実現しようとしているかについて詳しくは、[GitHub のページ](https://github.com/microsoft/ProjectReunion)をご覧ください。開発プロセスの一部を覗けるよう案内しています。 質問、ディスカッションの開始、機能の提案については、[投稿のガイド](https://github.com/microsoft/ProjectReunion/blob/master/docs/contributor-guide.md)をご覧ください。 Project Reunion により開発者の皆さんが最大のメリットを得られることを見届けたいと思っています。
