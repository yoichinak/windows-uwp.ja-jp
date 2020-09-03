---
description: このチュートリアルでは、UWP XAML ユーザー インターフェイスを追加し、MSIX パッケージを作成し、他の最新のコンポーネントを WPF アプリに組み込む方法について説明します。
title: 'チュートリアル: WPF アプリの現代化'
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: windows 10、uwp、windows フォーム、wpf、xaml islands
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: aa8991e7fd0bbb825ff5280f01693f092125f573
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89161376"
---
# <a name="tutorial-modernize-a-wpf-app"></a>チュートリアル: WPF アプリの現代化 

アプリを最初から書き換えるのではなく、最新の Windows 機能を既存のソース コードに統合することで、既存のデスクトップ アプリを[最新化する](index.md)方法は多数あります。 このチュートリアルでは、これらの機能を使用して、既存の WPF 基幹業務アプリを最新化するためのいくつかの方法について説明します。

* .NET Core 3
* XAML Islands を使用する UWP XAML コントロール
* アダプティブ カードと Windows 10 の通知
* MSIX の展開

このチュートリアルでは、以下の開発スキルが必要です。

* WPF を使用した Windows デスクトップ アプリの開発経験。
* C# と XAML の基本的な知識。
* UWP の基本的な知識。

## <a name="overview"></a>概要

このチュートリアルでは、Contoso Expenses という名前のシンプルな WPF 基幹業務アプリ用のコードを提供します。 このチュートリアルの架空のシナリオでは、Contoso Expenses は、レポートで送信される経費を追跡するために Contoso Corporation のマネージャーによって使用される内部アプリです。 現在、マネージャーはタッチ対応デバイスを備えており、マウスやキーボードを使用せずに Contoso Expenses アプリを使用したいと考えています。 残念ながら、現在のバージョンのアプリはタッチ対応ではありません。

Contoso では、従業員が経費レポートをより効率的に作成できるように、新しい Windows 機能でこのアプリを最新化したいと考えています。 機能の多くは、新しい UWP アプリをビルドすることで簡単に実装できます。 しかし、既存のアプリは複雑であり、これは長期にわたるさまざまなチームによる開発の結果です。 そのため、新しいテクノロジを使用して最初から書き換えることはできません。 チームは、既存のコードベースに新機能を追加するための最適な方法を探しています。

このチュートリアルの最初に、Contoso Expenses では .NET Framework 4.7.2 をターゲットとし、次のサードパーティ製ライブラリが使用されます。

* MVVM Light。MVVM パターンの基本実装。
* Unity。依存関係の注入コンテナー。
* LiteDb。データを格納するための埋め込みの NoSQL ソリューション。
* Bogus。仮のデータを生成するためのツール。

このチュートリアルでは、次のようにして、新しい Windows 機能で Contoso Expenses を強化します。

* 既存の WPF アプリを .NET Core 3.0 に移行する。 これにより、今後、新しい重要なシナリオが開かれます。
* XAML Islands を使用して、Windows Community Toolkit で提供される、ラップされたコントロールの **InkCanvas** と **MapControl** をホストする。
* XAML Islands を使用して、標準の UWP XAML コントロール (この場合は、**CalendardView**) をホストする。
* アダプティブ カードと Windows 10 の通知をアプリに統合する。
* アプリを MSIX でパッケージ化し、Azure DevOps に CI/CD パイプラインを設定して、使用可能になったらすぐに新しいバージョンのアプリをテスト担当者とユーザーに自動的に配信できるようにする。

## <a name="prerequisites"></a>前提条件

このチュートリアルを実行するには、開発用コンピューターにこれらの前提条件がインストールされている必要があります。

* Windows 10 バージョン 1903 (ビルド 18362) 以降のバージョン。
* [Visual Studio 2019](https://www.visualstudio.com)。
* [.NET Core 3 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0) (最新バージョンをインストールする)。

必ず、Visual Studio 2019 とともに以下のワークロードとオプション機能をインストールしてください。

* .NET デスクトップ開発
* ユニバーサル Windows プラットフォームの開発
* Windows 10 SDK (10.0.18362.0 以降)

## <a name="get-the-contoso-expenses-sample-app"></a>Contoso Expenses のサンプル アプリを取得する

チュートリアルを開始する前に、Contoso Expenses アプリのソース コードをダウンロードし、Visual Studio でコードをビルドできることを確認します。

1. [AppConsult WinAppsModernization ワークショップ リポジトリ](https://github.com/Microsoft/AppConsult-WinAppsModernizationWorkshop)の **[Releases]\(リリース\)** タブから、アプリのソース コードをダウンロードします。 直接リンクは [https://github.com/microsoft/AppConsult-WinAppsModernizationWorkshop/releases](https://github.com/microsoft/AppConsult-WinAppsModernizationWorkshop/releases) です。
2. zip ファイルを開き、すべてのコンテンツを **C:\\** ドライブのルートに抽出します。 **C:\WinAppsModernizationWorkshop** という名前のフォルダーが作成されます。
3. Visual Studio 2019 を開き、**C:\WinAppsModernizationWorkshop\Lab\Exercise1\01-Start\ContosoExpenses\ContosoExpenses.sln** ファイルをダブルクリックしてソリューションを開きます。
4. **[Start]\(開始\)** ボタンまたは CTRL + F5 キーを押して、Contoso Expenses の WPF プロジェクトをビルド、実行、およびデバッグできることを確認します。

## <a name="get-started"></a>開始

Contoso Expenses サンプル アプリ用のソース コードを取得し、それを Visual Studio でビルドできることが確認できたら、チュートリアルを開始する準備はできています。

* [パート 1: Contoso Expenses アプリを .NET Core 3 に移行する](modernize-wpf-tutorial-1.md)
* [パート 2: XAML Islands を使用して UWP InkCanvas コントロールを追加する](modernize-wpf-tutorial-2.md)
* [パート 3: XAML Islands を使用して UWP CalendarView コントロールを追加する](modernize-wpf-tutorial-3.md)
* [パート 4: Windows 10 ユーザー アクティビティと通知を追加する](modernize-wpf-tutorial-4.md)
* [パート 5: MSIX を使用してパッケージ化および配置する](modernize-wpf-tutorial-5.md)

## <a name="important-concepts"></a>重要な概念

以下のセクションでは、このチュートリアルで説明するいくつかの主な概念の背景を示します。 これらの概念について既によく理解している場合は、このセクションを省略してかまいません。

### <a name="universal-windows-platform-uwp"></a>ユニバーサル Windows プラットフォーム (UWP)

Microsoft では、Windows 8 で Windows ランタイム (WinRT) という新しいフレームワークを導入しました。 .NET Framework とは異なり、WinRT は、アプリに直接公開される API のネイティブ レイヤーです。 また、WinRT では、ランタイムの上に追加されたレイヤーである言語プロジェクションが導入されており、開発者は C# に加え、C# や JavaScript などの言語を使ってやり取りすることができます。 プロジェクションを使えば、開発者は .NET Framework でのアプリのビルドで得られたのと同じ C# と XAML の知識を活用するアプリを WinRT の上にビルドすることができます。 

Microsoft では、WinRT の上にビルドされた [ユニバーサル Windows プラットフォーム (UWP)](/windows/uwp/get-started/universal-application-platform-guide) を Windows 10 で導入しました。 UWP の最も重要な機能は、すべてのデバイス プラットフォームで共通の API セットを提供することです。アプリがデスクトップ、Xbox One、HoloLens のいずれで実行されているかに関係なく、同じ API を使用できます。

今後は、Windows 10 の新機能のほとんどが、タイムライン、Project Rome、Windows Hello などの機能を含め、WinRT API を介して公開されます。

### <a name="msix-packaging"></a>MSIX パッケージ作成ツール

[MSIX](/windows/msix/) は、Windows アプリ用の最新のパッケージング モデルです。 MSIX では UWP アプリだけでなく、Win32、WPF、Windows フォーム、Java、Electron などのテクノロジを使用するデスクトップ アプリのビルドもサポートされます。 MSIX パッケージでデスクトップ アプリをパッケージ化する場合、アプリを Microsoft Store に公開できます。 デスクトップ アプリでは、インストール時にパッケージ ID が取得されます。これにより、デスクトップ アプリでより広範な WinRT API を使用できるようになります。

詳しくは、次の記事をご覧ください。

* [デスクトップ アプリケーションのパッケージ化](/windows/uwp/porting/desktop-to-uwp-root)
* [パッケージ デスクトップ アプリケーションのしくみ](/windows/uwp/porting/desktop-to-uwp-behind-the-scenes)

### <a name="xaml-islands"></a>XAML Islands

Windows 10 バージョン 1903 以降では、*XAML Islands* という機能を使用して、UWP 以外のデスクトップ アプリで UWP コントロールをホストすることができます。 この機能を使用すると、既存のデスクトップ アプリの外観、操作性、および機能を、UWP コントロールでのみ使用できる最新の Windows 10 UI 機能で強化することができます。 つまり、既存の WPF、Windows フォーム、C++ Win32 アプリで Fluent Design System をサポートする、Windows Ink やコントロールなどの UWP 機能を使用できます。

詳細については、[デスクトップ アプリケーションの UWP コントロール (XAML Islands)](/windows/uwp/xaml-platform/xaml-host-controls) に関するページを参照してください。 このチュートリアルでは、異なる 2 種類の XAML Island コントロールを使用するプロセスについて説明します。

* Windows Community Toolkit の[InkCanvas](/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) と [MapControl](/windows/communitytoolkit/controls/wpf-winforms/mapcontrol)。 これらの WPF コントロールでは、対応する UWP コントロールのインターフェイスと機能をラップし、Visual Studio デザイナーの他の WPF コントロールと同様に使用できます。

* UWP の [カレンダー ビュー](/windows/uwp/design/controls-and-patterns/calendar-view) コントロール。 これは、Windows Community Toolkit の [WindowsXamlHost](/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) コントロールを使用してホストする、標準 UWP コントロールです。

### <a name="net-core-3"></a>.NET Core 3

[.NET Core](/dotnet/core/) は、完全な .NET Framework のクロスプラットフォームで軽量かつ容易に拡張可能なバージョンを実装するオープンソース フレームワークです。 完全な .NET Framework と比べ、.NET Core の起動時間は大幅に短縮されており、API の多くが最適化されています。

その最初のいくつかのリリースでは、.NET Core の焦点は、Web またはバックエンド アプリをサポートすることにありました。 .NET Core を使用すれば、Windows、Linux、または Docker コンテナーのようなマイクロサービス アーキテクチャでホストできる、スケーラブルな Web アプリまたは API を簡単にビルドできます。

.NET Core 3 は、.NET Core の最新リリースです。 このリリースのハイライトは、Windows フォーム、WPF アプリを含む Windows デスクトップ アプリのサポートです。 .NET Core 3 で新規および既存の Windows デスクトップ アプリを実行し、.NET Core で提供されるすべての特典を利用することができます。 [XAML Islands](xaml-islands.md)でホストされている UWP コントロールを、.NET Core 3 を対象とする Windows フォームや WPF アプリでも使用できます。

> [!NOTE]
> WPF と Windows フォームがクロスプラットフォームになっていないため、Linux および MacOS で WPF や Windows フォームを実行することはできません。 WPF と Windows フォームの UI コンポーネントは、引き続き Windows レンダリング システムに依存しています。

詳細については、「[.NET Core 3.0 の新機能](/dotnet/core/whats-new/dotnet-core-3-0)」をご覧ください。