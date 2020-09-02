---
Description: 最新の XAML ユーザー インターフェイスを追加し、MSIX パッケージを作成し、その他のモダン コンポーネントをお使いのデスクトップ アプリケーションに組み込みます。
title: Windows 用デスクトップ アプリの現代化
ms.topic: article
ms.date: 04/17/2019
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: d2ae73cc32fd4e3717fe40b8a6ec8c3397b40619
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89161536"
---
# <a name="modernize-your-desktop-apps"></a>デスクトップ アプリの現代化

Windows 10 とユニバーサル Windows プラットフォーム (UWP) に用意された多数の機能を使用して、デスクトップ アプリでモダンなエクスペリエンスを実現できます。 これらのほとんどの機能は、独自のペースでデスクトップ アプリに採用できるモジュラー コンポーネントとして利用できます。別のプラットフォーム用にアプリケーションを書き換える必要がありません。 既存のデスクトップ アプリを拡張するには、Windows 10 と UWP のどの部分を採用するかを選択します。

この記事では、今すぐデスクトップ アプリで使用できる Windows 10 および UWP 機能について説明します。 既存のアプリを現代化してこの記事で説明されている多くの機能を使用する方法を説明したチュートリアルについては、「[WPF アプリの現代化](modernize-wpf-tutorial.md)」チュートリアルをご覧ください。

> [!NOTE]
> デスクトップ アプリを Windows 10 に移行するのにサポートが必要な場合 [Desktop App Assure](/FastTrack/win-10-desktop-app-assure) サービスでは、自身のアプリを Windows 10 に移植する開発者に直接無償のサポートが提供されます。 このプログラムは、すべての ISV と条件に適合する企業が使用できます。 適格性とプログラム自体の詳細については、[/fasttrack/win-10-app-assure-assistance-offered](/fasttrack/win-10-app-assure-assistance-offered) をご覧ください。 すぐに開始するには、[リクエストを送信](https://fasttrack.microsoft.com/dl/daa)してください。

## <a name="windows-ui-library"></a>Windows UI ライブラリ

Windows UI ライブラリは、Windows 10 アプリ用のコントロールとその他のユーザー インターフェイス要素を提供する NuGet パッケージのセットです。 WinUI は、ダウンレベル バージョンの Windows 10 を対象とする UWP アプリ用の UWP コントロールの新しいバージョンと更新バージョンを提供するツールキットとして開始されました。 WinUI のスコープが拡張されて、UWP、.NET、Win32 において Windows 10 アプリ向けの最新のネイティブ ユーザー インターフェイス (UI) プラットフォームになりました。

デスクトップ アプリでは、次の方法で WinUI を使用できます。

* WPF、Windows フォーム、および C++/Win32 の既存のアプリを更新して、これらのアプリで [XAML Islands](xaml-islands.md) を使用して WinUI 2.x コントロールをホストできます。
* [WinUi 3.0 Preview 1](../../winui/winui3/index.md) からは、[全面的に WinUI ベースの UI を使用する .NET アプリと C++/Win32 アプリ](../../winui/winui3/get-started-winui3-for-desktop.md)を作成できます。

「[Windows UI (WinUI) ライブラリ](../../winui/index.md)」をご覧ください。

## <a name="msix-packages"></a>MSIX パッケージ

MSIX は、UWP、WPF、Windows フォーム、Win32 アプリを含む、あらゆる Windows アプリ用のユニバーサル パッケージ化エクスペリエンスを提供するモダンな Windows アプリ パッケージ形式です。 MSIX は、MSI、.appx、App-V、ClickOnce インストール テクノロジの優れた部分を組み合わせたもので、最新で信頼性の高いパッケージ化エクスペリエンスを提供します。

MSIX パッケージにデスクトップ Windows アプリをパッケージ化することで、堅牢なインストール、更新エクスペリエンス、柔軟な機能システムによる管理されたセキュリティ モデル、Microsoft Store のサポート、エンタープライズ管理、および多くのカスタム配布モデルにアクセスできます。

詳しくは、MSIX ドキュメントの「[デスクトップ アプリケーションのパッケージ化](/windows/msix/desktop/desktop-to-uwp-root)」をご覧ください。

## <a name="net-core-3"></a>.NET Core 3

.NET Core 3 は、.NET Core の最新のメジャー リリースです。 このリリースのハイライトは、Windows フォーム、WPF アプリを含む Windows デスクトップ アプリのサポートです。 .NET Core 3 で新規および既存の Windows デスクトップ アプリを実行し、.NET Core で提供されるすべての特典を利用することができます。 [XAML Islands](xaml-islands.md)でホストされている UWP コントロールを、.NET Core 3 を対象とする Windows フォームや WPF アプリでも使用できます。

詳細については、「[.NET Core 3.0 の新機能](/dotnet/core/whats-new/dotnet-core-3-0)」をご覧ください。

## <a name="windows-runtime-apis"></a>Windows ランタイム API

多くの Windows ランタイム API を WPF、Windows フォーム、または C++ Win32 デスクトップ アプリで直接呼び出して、Windows 10 ユーザーの利便性を高めるモダン エクスペリエンスを統合できます。 たとえば、Windows ランタイム API を呼び出して、トースト通知をデスクトップ アプリに追加できます。

詳しくは、[デスクトップ アプリでの Windows ランタイム API の使用](desktop-to-uwp-enhance.md)に関する記事をご覧ください。

## <a name="host-uwp-controls-xaml-islands"></a>UWP コントロールのホスト (XAML Islands)

Windows 10、バージョン 1903 以降では、ウィンドウ ハンドル (HWND) に関連付けられた WPF、Windows フォーム、または C++ Win32 アプリ内の任意の UI 要素に直接 [UWP XAML コントロール](/windows/uwp/design/controls-and-patterns/controls-by-function)を追加できます。 つまり、[Windows Ink](/windows/uwp/design/input/pen-and-stylus-interactions)、[Fluent Design System](/windows/uwp/design/fluent-design-system/index) をサポートするコントロールなど最新の UWP 機能を、デスクトップ アプリのウィンドウやその他のディスプレイ サーフェスに完全に統合できます。 この開発者シナリオは "*XAML Islands*" とも呼ばれます。

詳細については、「[UWP controls in desktop apps (デスクトップ アプリでの UWP コントロール)](xaml-islands.md)」を参照してください。

## <a name="use-the-visual-layer-in-desktop-apps"></a>デスクトップ アプリでのビジュアル レイヤーの使用

UWP 以外のデスクトップ アプリで Windows ランタイム API を使用して WPF、Windows フォーム、C++ Win32 アプリの外観や機能を高めたり、UWP でのみ利用可能な最新の Windows 10 UI 機能を活用したりできるようになりました。 これは、XAML  Islands を使用してホストできる組み込みの UWP コントロールだけにとどまらないカスタム エクスペリエンスを作成する必要がある場合に便利です。

詳しくは、「[Modernize your desktop app using the Visual layer (ビジュアル レイヤーを使用したデスクトップ アプリの現代化)](visual-layer-in-desktop-apps.md)」をご覧ください。

## <a name="additional-features-available-to-apps-with-package-identity"></a>パッケージ ID を持つアプリで利用できる追加機能

一部の最新の Windows 10 エクスペリエンスは、[パッケージ ID](/uwp/schemas/appxpackage/uapmanifestschema/element-identity) を持つデスクトップ アプリでのみ利用可能です。 これらの機能には、特定の Windows ランタイム API、パッケージ拡張機能、UWP コンポーネントなどがあります。 詳しくは、「[パッケージ ID が必要な機能](modernize-packaged-apps.md)」を参照してください。

デスクトップ アプリに ID を付与する方法はいくつかあります。

* [MSIX パッケージ](/windows/msix/desktop/desktop-to-uwp-root)でパッケージ化します。 MSIX は、すべての Windows アプリ、WPF、Windows フォーム、Win32 アプリ用のユニバーサルなパッケージ化エクスペリエンスを提供するモダンなアプリ パッケージ形式です。 これは、堅牢なインストールと更新のエクスペリエンス、柔軟な機能システムによる管理されたセキュリティ モデル、Microsoft Store のサポート、エンタープライズ管理、および多くのカスタム配布モデルを提供します。 詳しくは、MSIX ドキュメントの「[デスクトップ アプリケーションのパッケージ化](/windows/msix/desktop/desktop-to-uwp-root)」をご覧ください。
* デスクトップ アプリを配置するために MSIX パッケージを導入できない場合、Windows 10 バージョン 2004 以降では、パッケージ マニフェストのみを含む "*スパース MSIX パッケージ*" を作成することでパッケージ ID を付与できます。 詳細については、「[パッケージ化されていないデスクトップ アプリに ID を付与する](grant-identity-to-nonpackaged-apps.md)」を参照してください。

<a id="desktop-uwp-controls"></a>

## <a name="uwp-controls-optimized-for-desktop-apps"></a>デスクトップ アプリ用に最適化された UWP コントロール

デスクトップ デバイス ファミリを排他的にターゲットとする UWP アプリを構築する場合でも、WPF、Windows フォーム、または C++ Win32 デスクトップ アプリで UWP コントロールを使用する場合でも、次の新しい、または更新された UWP コントロールは [Fluent Design System](/windows/uwp/design/fluent-design-system/index) を使用してデスクトップ最適化されたエクスペリエンスを提供するように設計されています。 これらのコントロールは、Windows 10、バージョン 1809 (October 2018 Update、またはバージョン 10.0.17763) で導入されました。

| Control |  説明 |
|------ |--------------|
| [MenuBar](/windows/uwp/design/controls-and-patterns/menus#create-a-menu-bar) | **CommandBar** で許容されるよりも多くの組織またはグループ化が必要になる可能性があるアプリ用のコマンド セットを公開する簡単でシンプルな方法を提供します。 |
| [DropDownButton](/windows/uwp/design/controls-and-patterns/buttons#create-a-drop-down-button) | その他のオプションを含むアタッチされたポップアップのある視覚インジケータとして山かっこを示します。  |
| [SplitButton](/windows/uwp/design/controls-and-patterns/buttons#create-a-split-button) | 個別に呼び出せる 2 つのパーツを持つボタンを指定します。 1 つのパーツは標準のボタンのように動作し、即座にアクションを呼び出します。 もう一方のパーツは、ユーザーが選択できる追加オプションを含むポップアップを呼び出します。|
| [ToggleSplitButton](/windows/uwp/design/controls-and-patterns/buttons#create-a-toggle-split-button) | 個別に呼び出せる 2 つのパーツを持つボタンを指定します。 1 つのパーツは、オンまたはオフにできるトグル ボタンのように動作します。 もう一方のパーツは、ユーザーが選択できる追加オプションを含むポップアップを呼び出します。 |
| [CommandBarFlyout](/windows/uwp/design/controls-and-patterns/command-bar-flyout) |  UI キャンバス上の項目のコンテキストで一般的なユーザー タスクを示すことができます。 |
| [ComboBox](/windows/uwp/design/controls-and-patterns/combo-box#make-a-combo-box-editable) | コンボ ボックスを、ユーザーがコントロール内に一覧表示されていない値を入力できるように、編集可能にできるようになりました。  |
| [TreeView](/windows/uwp/design/controls-and-patterns/tree-view) | データ バインディング、項目テンプレート、ドラッグ アンド ドロップを有効にするようにツリー ビューを構成できるようになりました。  |
| [DataGridView](/windows/communitytoolkit/controls/datagrid) |   行と列に柔軟にデータ コレクションを表示できます。 このコントロールは、[Windows コミュニティ ツールキット](/windows/uwpcommunitytoolkit/)で入手できます。  |

## <a name="other-technologies-for-modern-desktop-apps"></a>モダン デスクトップ アプリ向けのその他のテクノロジ

### <a name="microsoft-graph"></a>Microsoft Graph

Microsoft Graph は、数百万に及ぶユーザーのデータとやり取りする組織やコンシューマーがアプリの構築に使用できる API のコレクションです。 Microsoft Graph では、次のデータにアクセスするための REST API とクライアント ライブラリが公開されます。
* Azure Active Directory
* Microsoft 365 Office アプリ:SharePoint、OneDrive、Outlook/Exchange、Microsoft Teams、OneNote、Planner、Excel
* Enterprise Mobility および Security サービス:Identity Manager、Intune、Advanced Threat Analytics、Advanced Threat Protection
* Windows 10 サービス: アクティビティとデバイス

詳細については、[Microsoft Graph のドキュメント](/graph/overview)をご覧ください。

### <a name="adaptive-cards"></a>アダプティブ カード

アダプティブ カードは、オープンなクロスプラットフォーム フレームワークで、これにより、さまざまなデバイスやプラットフォーム間で一貫性のある共通の方法によりカードベースの UI コンテンツを交換できます。

詳しくは、[アダプティブ カードのドキュメント](/adaptive-cards/)をご覧ください。