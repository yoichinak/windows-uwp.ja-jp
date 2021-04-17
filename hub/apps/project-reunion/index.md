---
description: Project Reunion と、これによって開発者が受ける利点、開発者に用意されているもの、およびフィードバックの提供方法について説明します。
title: Project Reunion 0.5 を使用してデスクトップ Windows アプリをビルドする
ms.topic: article
ms.date: 03/19/2021
keywords: windows win32, デスクトップ開発, project reunion
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: 6674b18c0a042b857b14972c55e7e57f01acc03d
ms.sourcegitcommit: df14e7768acdb243190e3418db5afa5d65c5ff88
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/16/2021
ms.locfileid: "107574617"
---
# <a name="build-desktop-windows-apps-with-project-reunion-05"></a>Project Reunion 0.5 を使用してデスクトップ Windows アプリをビルドする

Project Reunion は、Windows アプリ開発プラットフォームの次世代の新しい開発者向けコンポーネントとツールのセットです。 Project Reunion によって提供される API とツールの統合セットは、対象の幅広い Windows 10 OS バージョン上のどのデスクトップ アプリからでも一貫した方法で使用できます。

Project Reunion は、既存のデスクトップ Windows アプリプラット フォームや .NET (Windows フォームと WPF を含む)、C++/Win32 などのフレームワークから置き換わるものではありません。 これは、開発者がこれらのプラットフォーム間で利用できる API とツールの共通セットにより、これらの既存のプラットフォームを補完するものです。 詳細については、[Project Reunion のベネフィット](#benefits-of-project-reunion-for-windows-developers)に関する記事を参照してください。

> [!NOTE]
> Project Reunion 0.5 では、実稼働環境での MSIX パッケージ デスクトップ アプリ (C#/.NET 5 または C++/Win32) での使用がサポートされています。 Project Reunion 0.5 を使用するパッケージ デスクトップ アプリは、Microsoft Store に発行できます。 UWP アプリの場合、Project Reunion 0.5 はプレビューとしてのみ使用できます。 このリリースは、実稼働環境で使用されている UWP アプリには対応していません。
>
>**Project Reunion** は、今後のリリースで変更される可能性のあるコード ネームです。

## <a name="overview"></a>概要

Project Reunion には、新しいプロジェクトで Project Reunion コンポーネントを使用するように構成されたプロジェクト テンプレートを含む Visual Studio 2019 の拡張機能が用意されています。 Project Reunion ライブラリは、既存のプロジェクトにインストールできる NuGet パッケージを使用して入手することもできます。 詳細については、「[Project Reunion の概要](get-started-with-project-reunion.md)」を参照してください。

Project Reunion を使用するアプリをビルドしたら、他のコンピューターに展開できます。 詳細については、「[Project Reunion を使用するアプリを展開する](deploy-apps-that-use-project-reunion.md)」を参照してください。

Project Reunion 0.5 には、アプリで使用できる次の API とコンポーネントのセットが含まれています。 他のコンポーネントを Project Reunion に取り込むための将来的な計画について詳しくは、[こちら](https://github.com/microsoft/ProjectReunion/blob/master/docs/README.md)をご覧ください。

| コンポーネント | 説明 |
|---------|-------------|
| [Windows UI ライブラリ 3](../winui/winui3/index.md) | Windows UI ライブラリ (WinUI) 3 は、Windows アプリに対応する次世代の Windows ユーザー エクスペリエンス (UX) プラットフォームです。 このリリースには、[WinUI ベースのユーザー インターフェイスを備えたアプリのビルド](..\winui\winui3\winui-project-templates-in-visual-studio.md)を始めるときに役立つ Visual Studio プロジェクト テンプレートと、WinUI ライブラリが収められた NuGet パッケージが含まれます。  |
| [MRT Core を使用してリソースを管理する](mrtcore/mrtcore-overview.md) | MRT Core から、アプリで使用されるリソースを読み込んで管理するための API が提供されます。 MRT Core は、最新の [Windows リソース管理システム](/windows/uwp/app-resources/resource-management-system)の簡素化されたバージョンです。 |
| [DWriteCore を使用してテキストを表示する](dwritecore.md) | DWriteCore を使用すると、デバイスに依存しないテキスト レイアウト システム、ハードウェアアクセラレーション テキスト、マルチフォーマット テキスト、および幅広い言語サポートなど、テキスト レンダリングの現在のすべての DirectWrite 機能にアクセスできます。  |

## <a name="benefits-of-project-reunion-for-windows-developers"></a>Windows 開発者にとっての Project Reunion のベネフィット

Project Reunion は、OS から切り離され、NuGet パッケージを開始て開発者にリリースされる、さまざまな Windows API のセットを提供します。 Project Reunion は、Windows SDK に代わるものではありません。 Windows SDK は引き続き機能します。また、OS および Windows SDK リリースを通じて配信される API 経由で引き続き進化する Windows のコア コンポーネントが多数あります。 開発者は Project Reunion を自身のペースで採用することをお勧めします。

### <a name="unified-api-surface-across-desktop-app-platforms"></a>デスクトップ アプリ プラットフォームの Unified API サーフェス

デスクトップ Windows アプリを作成する開発者は、複数のアプリ プラットフォームおよびフレームワークから選択する必要があります。 各プラットフォームには、他のプラットフォームを使用して構築されるアプリで使用できる多くの機能と API が用意されていますが、一部の機能と API は特定のプラットフォームでのみ使用できます。 Project Reunion により、すべてのデスクトップ Windows 10 アプリの Windows API へのアクセスが統合されます。 どのアプリ モデルを選択しても、Project Reunion で利用できる同じ Windows API セットにアクセスできます。

Microsoft は徐々に、異なるアプリ モデル間の相違をなくすように Project Reunion に投資する予定です。 Project Reunion には、WinRT API とネイティブ C API の両方が含まれます。

### <a name="consistent-support-across-windows-10-versions"></a>Windows 10 バージョン間での一貫したサポート

Windows API が新しい OS バージョンにより進化し続けるのに伴い、開発者は[バージョン アダプティブ コード](/windows/uwp/debug-test-perf/version-adaptive-code)などの手法を使用して、バージョンでのすべての相違を考慮して、アプリケーションの対象ユーザーにアピールする必要があります。 これにより、コードと開発エクスペリエンスが複雑になる可能性があります。

Project Reunion API は、Windows 10 バージョン 1809 以降のすべてのバージョンの Windows 10 で動作します。 つまり、お客様が Windows 10 バージョン 1809 以降のバージョンを使用している限り、Project Reunion の新しい API と機能をリリースと同時に使用でき、バージョンに適応したコードを記述する必要がありません。

### <a name="faster-release-cadence"></a>短いリリース サイクル

新しい Windows API と機能は、通常、年に 1 回または 2 回のリリース サイクルで発生する OS リリースに関連付けられています。 Project Reunion の更新プログラムはさらに短いサイクルで提供されるため、Windows 開発プラットフォームでイノベーションが起きるとすぐに、より早く、より迅速に利用できるようになります。

## <a name="limitations-and-known-issues"></a>制限事項と既知の問題

通常、次の制限事項と既知の問題が Project Reunion 0.5 に適用されます。

- **デスクトップ アプリ (C#/.NET 5 または C++/Win32)** : Project Reunion 0.5 は、パッケージされていないデスクトップ アプリ (C#/.NET 5 または C++/Win32) では使用できません。 このリリースは、MSIX のパッケージ デスクトップ アプリでのみサポートされています。
- **UWP アプリ**: Project Reunion 0.5 は、実稼働環境で使用されている UWP アプリには対応していません。 UWP アプリで Project Reunion を使用するには、実稼働環境でサポートされていない、Project Reunion 0.5 拡張機能のプレビュー バージョンを使用する必要があります。 プレビュー拡張機能のインストールの詳細については、「[開発環境を設定する](get-started-with-project-reunion.md#set-up-your-development-environment)」を参照してください。
- [WinUI 3 開発者ツールの制限](..\winui\winui3\index.md#developer-tools)は、Project Reunion 0.5 を使用するどのプロジェクトにも適用されます。

次の制限事項と既知の問題は、特定の開発者のシナリオに適用されます。

### <a name="using-the-project-reunion-nuget-package-in-existing-projects"></a>Project Reunion NuGet パッケージを既存のプロジェクトで使用する

[Project Reunion 0.5 NuGet パッケージを既存のプロジェクトで使用する](get-started-with-project-reunion.md#use-project-reunion-in-an-existing-project)には、次の制限に注意してください。

- Project Reunion 0.5 NuGet パッケージは、実稼働環境でのデスクトップ (C#/.NET 5 および C++/WinRT) プロジェクトでの使用がサポートされています。 これは、UWP プロジェクトの開発者プレビューとして提供されており、実稼働環境での UWP プロジェクトでの使用はサポートされていません。
- (**Microsoft.ProjectReunion** と呼ばれる) Project Reunion 0.5 NuGet パッケージには、WinUI、MRT Core、DWriteCore などのコンポーネントの実装を含む他のサブパッケージ (**Microsoft.ProjectReunion.Foundation**、**Microsoft.ProjectReunion.WinUI** など) が含まれています。 これらのサブパッケージは、プロジェクト内の特定のコンポーネントのみを参照するように個別にインストールすることはできません。 すべてのコンポーネントが含まれている、**Microsoft.ProjectReunion** パッケージをインストールする必要があります。  
- Project Reunion 0.5 NuGet パッケージを既存のプロジェクトにインストールする場合、プロジェクトでの Project Reunion に含まれる WinUI 3 以外のコンポーネントのみを使用できます。 WinUI 3 を使用するには、前のセクションで説明したように、WinUI 3 プロジェクト テンプレートのいずれかを使用して新しいプロジェクトを作成する必要があります。


#### <a name="asta-to-sta-threading-model"></a>ASTA から STA へのスレッド モデル

既存の UWP アプリから Project Reunion を使用する新しい C#/.NET 5 または C++/Win32 WinUI 3 プロジェクトにコードを移行する場合は、新しいプロジェクトで、UWP アプリで使用される[アプリケーション STA (ASTA)](https://devblogs.microsoft.com/oldnewthing/20210224-00/?p=104901) スレッド モデルではなく、[シングルスレッド アパートメント (STA)](/windows/win32/com/single-threaded-apartments) スレッド モデルが使用されることに注意してください。 コードで ASTA スレッドモデルの再入不可能な動作が想定されている場合、コードが予測どおりに動作しない可能性があります。

## <a name="developer-roadmap"></a>開発者向けロードマップ

最新の Project Reunion プランについては、[ロードマップ](https://github.com/microsoft/ProjectReunion/blob/main/docs/roadmap.md)を参照してください。

## <a name="give-feedback-and-contribute"></a>フィードバックと投稿の送信

Microsoft は、Project Reunion をオープン ソース プロジェクトとしてビルドしています。 Microsoft が Project Reunion をどのようにビルドしているか、そして開発プロセスに加わるための方法について詳しくは、[GitHub のページ](https://github.com/microsoft/ProjectReunion)をご覧ください。 質問、ディスカッションの開始、機能の提案については、[投稿のガイド](https://github.com/microsoft/ProjectReunion/blob/master/docs/contributor-guide.md)をご覧ください。 Project Reunion により開発者の皆さんが最大のメリットを得られることを見届けたいと思っています。

## <a name="related-topics"></a>関連トピック

- [Project Reunion を使用してデスクトップ Windows アプリをビルドする](index.md)
- [Project Reunion の概要](get-started-with-project-reunion.md)
- [Project Reunion を使用するアプリを展開する](deploy-apps-that-use-project-reunion.md)
- [Windows UI ライブラリ 3](../winui/winui3/index.md)
- [MRT Core を使用してリソースを管理する](mrtcore/mrtcore-overview.md)
- [DWriteCore を使用してテキストを表示する](dwritecore.md)
