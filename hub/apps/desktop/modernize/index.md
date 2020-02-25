---
Description: 最新の XAML ユーザー インターフェイスを追加し、MSIX パッケージを作成し、その他のモダン コンポーネントをお使いのデスクトップ アプリケーションに組み込みます。
title: Windows 用デスクトップ アプリの現代化
ms.topic: article
ms.date: 04/17/2019
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: 0c86290c9765eba5186e777f8de7b3b86967be9e
ms.sourcegitcommit: 0a319e2e69ef88b55d472b009b3061a7b82e3ab1
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/21/2020
ms.locfileid: "77521343"
---
# <a name="modernize-your-desktop-apps"></a>デスクトップ アプリの現代化

Windows 10 とユニバーサル Windows プラットフォーム (UWP) に用意された多数の機能を使用して、デスクトップ アプリでモダンなエクスペリエンスを実現できます。 これらのほとんどの機能は、独自のペースでデスクトップ アプリに採用できるモジュラー コンポーネントとして利用できます。別のプラットフォーム用にアプリケーションを書き換える必要がありません。 既存のデスクトップ アプリを拡張するには、Windows 10 と UWP のどの部分を採用するかを選択します。

この記事では、今すぐデスクトップ アプリで使用できる Windows 10 および UWP 機能について説明します。 既存のアプリを現代化してこの記事で説明されている多くの機能を使用する方法を説明したチュートリアルについては、「[WPF アプリの現代化](modernize-wpf-tutorial.md)」チュートリアルをご覧ください。

> [!NOTE]
> デスクトップ アプリを Windows 10 に移行するのにサポートが必要な場合 [Desktop App Assure](https://docs.microsoft.com/FastTrack/win-10-desktop-app-assure) サービスでは、自身のアプリを Windows 10 に移植する開発者に直接無償のサポートが提供されます。 このプログラムは、すべての ISV と条件に適合する企業が使用できます。 ご利用資格とプログラム自体について詳しくは、[https://docs.microsoft.com/fasttrack/win-10-app-assure-assistance-offered](https://docs.microsoft.com/fasttrack/win-10-app-assure-assistance-offered) をご覧ください。 すぐに開始するには、[リクエストを送信](https://fasttrack.microsoft.com/dl/daa)してください。

## <a name="msix-packages"></a>MSIX パッケージ

MSIX は、UWP、WPF、Windows フォーム、Win32 アプリを含む、あらゆる Windows アプリ用のユニバーサル パッケージ化エクスペリエンスを提供するモダンな Windows アプリ パッケージ形式です。 MSIX は、MSI、.appx、App-V、ClickOnce インストール テクノロジの優れた部分を組み合わせたもので、最新で信頼性の高いパッケージ化エクスペリエンスを提供します。

MSIX パッケージにデスクトップ Windows アプリをパッケージ化することで、堅牢なインストール、更新エクスペリエンス、柔軟な機能システムによる管理されたセキュリティ モデル、Microsoft Store のサポート、エンタープライズ管理、および多くのカスタム配布モデルにアクセスできます。

詳しくは、MSIX ドキュメントの「[デスクトップ アプリケーションのパッケージ化](/windows/msix/desktop/desktop-to-uwp-root)」をご覧ください。

## <a name="net-core-3"></a>.NET Core 3

.NET Core 3 は、.NET Core の最新のメジャー リリースです。 このリリースのハイライトは、Windows フォーム、WPF アプリを含む Windows デスクトップ アプリのサポートです。 .NET Core 3 で新規および既存の Windows デスクトップ アプリを実行し、.NET Core で提供されるすべての特典を利用することができます。 [XAML Islands](xaml-islands.md)でホストされている UWP コントロールを、.NET Core 3 を対象とする Windows フォームや WPF アプリでも使用できます。

詳細については、「[.NET Core 3.0 の新機能](https://docs.microsoft.com/dotnet/core/whats-new/dotnet-core-3-0)」をご覧ください。

## <a name="uwp-apis"></a>UWP API

多くの UWP API を WPF、Windows フォーム、または C++ Win32 デスクトップ アプリで直接呼び出して、Windows 10 ユーザーの利便性を高めるモダン エクスペリエンスを統合できます。 たとえば、UWP API を呼び出して、トースト通知をデスクトップ アプリに追加できます。

詳細については、「[Use UWP APIs in desktop apps (デスクトップ アプリでの UWP API の使用)](desktop-to-uwp-enhance.md)」を参照してください。

## <a name="host-uwp-controls-xaml-islands"></a>UWP コントロールのホスト (XAML Islands)

Windows 10、バージョン 1903 以降では、ウィンドウ ハンドル (HWND) に関連付けられた WPF、Windows フォーム、または C++ Win32 アプリ内の任意の UI 要素に直接 [UWP XAML コントロール](/windows/uwp/design/controls-and-patterns/controls-by-function)を追加できます。 つまり、[Windows Ink](/windows/uwp/design/input/pen-and-stylus-interactions)、[Fluent Design System](/windows/uwp/design/fluent-design-system/index) をサポートするコントロールなど最新の UWP 機能を、デスクトップ アプリのウィンドウやその他のディスプレイ サーフェスに完全に統合できます。 この開発者シナリオは "*XAML Islands*" とも呼ばれます。

詳細については、「[UWP controls in desktop apps (デスクトップ アプリでの UWP コントロール)](xaml-islands.md)」を参照してください。

## <a name="use-the-visual-layer-in-desktop-apps"></a>デスクトップ アプリでのビジュアル レイヤーの使用

UWP 以外のデスクトップ アプリで UWP API を使用して WPF、Windows フォーム、C++ Win32 アプリの外観や機能を高めたり、UWP でのみ利用可能な最新の Windows 10 UI 機能を活用したりできるようになりました。 これは、XAML  Islands を使用してホストできる組み込みの UWP コントロールだけにとどまらないカスタム エクスペリエンスを作成する必要がある場合に便利です。

詳しくは、「[Modernize your desktop app using the Visual layer (ビジュアル レイヤーを使用したデスクトップ アプリの現代化)](visual-layer-in-desktop-apps.md)」をご覧ください。

## <a name="additional-features-available-to-apps-with-package-identity"></a>パッケージ ID を持つアプリで利用できる追加機能

一部の最新の Windows 10 エクスペリエンスは、[パッケージ ID](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) を持つデスクトップ アプリでのみ利用可能です。 これらの機能には、特定の UWP API、パッケージ拡張機能、UWP コンポーネントなどがあります。 詳しくは、「[パッケージ ID が必要な機能](modernize-packaged-apps.md)」を参照してください。

デスクトップ アプリに ID を付与する方法はいくつかあります。

* [MSIX パッケージ](/windows/msix/desktop/desktop-to-uwp-root)でパッケージ化します。 MSIX は、すべての Windows アプリ、WPF、Windows フォーム、Win32 アプリ用のユニバーサルなパッケージ化エクスペリエンスを提供するモダンなアプリ パッケージ形式です。 これは、堅牢なインストールと更新のエクスペリエンス、柔軟な機能システムによる管理されたセキュリティ モデル、Microsoft Store のサポート、エンタープライズ管理、および多くのカスタム配布モデルを提供します。 詳しくは、MSIX ドキュメントの「[デスクトップ アプリケーションのパッケージ化](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-root)」をご覧ください。
* デスクトップ アプリを配置するために MSIX パッケージを導入できない場合、Windows 10 Insider Preview Build 10.0.19000.0 以降では、パッケージ マニフェストのみを含む "*スパース MSIX パッケージ*" を作成することでパッケージ ID を付与できます。 詳細については、「[パッケージ化されていないデスクトップ アプリに ID を付与する](grant-identity-to-nonpackaged-apps.md)」を参照してください。

<a id="desktop-uwp-controls"/>

## <a name="uwp-controls-optimized-for-desktop-apps"></a>デスクトップ アプリ用に最適化された UWP コントロール

デスクトップ デバイス ファミリを排他的にターゲットとする UWP アプリを構築する場合でも、WPF、Windows フォーム、または C++ Win32 デスクトップ アプリで UWP コントロールを使用する場合でも、次の新しい、または更新された UWP コントロールは [Fluent Design System](/windows/uwp/design/fluent-design-system/index) を使用してデスクトップ最適化されたエクスペリエンスを提供するように設計されています。 これらのコントロールは、Windows 10、バージョン 1809 (October 2018 Update、またはバージョン 10.0.17763) で導入されました。

| Control |  説明 |
|------ |--------------|
| [MenuBar](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/menus#create-a-menu-bar) | **CommandBar** で許容されるよりも多くの組織またはグループ化が必要になる可能性があるアプリ用のコマンド セットを公開する簡単でシンプルな方法を提供します。 |
| [DropDownButton](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/buttons#create-a-drop-down-button) | その他のオプションを含むアタッチされたポップアップのある視覚インジケータとして山かっこを示します。  |
| [SplitButton](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/buttons#create-a-split-button) | 個別に呼び出せる 2 つのパーツを持つボタンを指定します。 1 つのパーツは標準のボタンのように動作し、即座にアクションを呼び出します。 もう一方のパーツは、ユーザーが選択できる追加オプションを含むポップアップを呼び出します。|
| [ToggleSplitButton](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/buttons#create-a-toggle-split-button) | 個別に呼び出せる 2 つのパーツを持つボタンを指定します。 1 つのパーツは、オンまたはオフにできるトグル ボタンのように動作します。 もう一方のパーツは、ユーザーが選択できる追加オプションを含むポップアップを呼び出します。 |
| [CommandBarFlyout](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/command-bar-flyout) |  UI キャンバス上の項目のコンテキストで一般的なユーザー タスクを示すことができます。 |
| [ComboBox](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/combo-box#make-a-combo-box-editable) | コンボ ボックスを、ユーザーがコントロール内に一覧表示されていない値を入力できるように、編集可能にできるようになりました。  |
| [TreeView](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/tree-view) | データ バインディング、項目テンプレート、ドラッグ アンド ドロップを有効にするようにツリー ビューを構成できるようになりました。  |
| [DataGridView](https://docs.microsoft.com/windows/communitytoolkit/controls/datagrid) |   行と列に柔軟にデータ コレクションを表示できます。 このコントロールは、[Windows コミュニティ ツールキット](https://docs.microsoft.com/windows/uwpcommunitytoolkit/)で入手できます。  |

## <a name="windows-ui-library"></a>Windows UI ライブラリ

Windows UI ライブラリは、UWP アプリ用の新しいコントロールとその他のユーザー インターフェイス要素を提供する NuGet パッケージのセットです。 Windows UI ライブラリの API は以前のバージョンの Windows 10 で動作するため、ユーザーが最新バージョンの Windows 10 を実行していなくてもアプリは動作します。 これにより、新しいコントロールが Windows UI ライブラリでリリースされたときにこれらを導入することができます。バージョン チェックや条件付き XAML を含める必要がありません。

「[Windows UI ライブラリ](https://docs.microsoft.com/uwp/toolkits/winui/)」をご覧ください。

## <a name="other-technologies-for-modern-desktop-apps"></a>モダン デスクトップ アプリ向けのその他のテクノロジ

### <a name="microsoft-graph"></a>Microsoft Graph

Microsoft Graph は、数百万に及ぶユーザーのデータとやり取りする組織やコンシューマーがアプリの構築に使用できる API のコレクションです。 Microsoft Graph では、次のデータにアクセスするための REST API とクライアント ライブラリが公開されます。
* Azure Active Directory
* Office 365 サービス:SharePoint、OneDrive、Outlook/Exchange、Microsoft Teams、OneNote、Planner、Excel
* Enterprise Mobility および Security サービス:Identity Manager、Intune、Advanced Threat Analytics、Advanced Threat Protection
* Windows 10 サービス: アクティビティとデバイス

詳細については、[Microsoft Graph のドキュメント](https://developer.microsoft.com/graph/docs/concepts/overview)をご覧ください。

### <a name="adaptive-cards"></a>アダプティブ カード

アダプティブ カードは、オープンなクロスプラットフォーム フレームワークで、これにより、さまざまなデバイスやプラットフォーム間で一貫性のある共通の方法によりカードベースの UI コンテンツを交換できます。

詳しくは、[アダプティブ カードのドキュメント](https://docs.microsoft.com/adaptive-cards/)をご覧ください。
