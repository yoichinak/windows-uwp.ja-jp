---
description: このチュートリアルでは、UWP XAML ユーザーインターフェイスを追加する方法、MSIX パッケージを作成する方法、およびその他の最新のコンポーネントを WPF アプリに組み込む方法について説明します。
title: チュートリアル:WPF アプリの現代化
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: windows 10、uwp、windows フォーム、wpf、xaml islands
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 3e78bd81531750ac419c5ca3628e72a3e8505038
ms.sourcegitcommit: f34deba1d4460d85ed08fe9648999fe03ff6a3dd
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/26/2019
ms.locfileid: "71317081"
---
# <a name="tutorial-modernize-a-wpf-app"></a>チュートリアル:WPF アプリの現代化 

アプリを最初から書き換えるのではなく、最新の Windows 機能を既存のソースコードに[統合することによっ](index.md)て、既存のデスクトップアプリを最新化する方法は多数あります。 このチュートリアルでは、次の機能を使用して、既存の WPF 基幹業務アプリを最新化するためのいくつかの方法について説明します。

* .NET Core 3
* XAML Islands を使用した UWP XAML コントロール
* アダプティブカードと Windows 10 の通知
* MSIX の展開

このチュートリアルでは、次の開発スキルが必要です。

* WPF を使用した Windows デスクトップアプリの開発に関する経験があります。
* および XAML にC#関する基本的な知識。
* UWP の基本的な知識。

## <a name="overview"></a>概要

このチュートリアルでは、Contoso の経費という単純な WPF 基幹業務アプリのコードを提供します。 このチュートリアルの架空のシナリオでは、contoso 社の支出は、レポートによって送信された経費を追跡するために Contoso Corporation のマネージャーによって使用される内部アプリです。 これで、マネージャーはタッチ対応デバイスを装備しています。マウスやキーボードを使用せずに Contoso の経費アプリを使用したいと考えています。 残念ながら、現在のバージョンのアプリはタッチフレンドリではありません。

Contoso では、従業員が経費報告書を効率的に作成できるように、新しい Windows 機能でこのアプリを最新化したいと考えています。 多くの機能は、新しい UWP アプリを構築することによって簡単に実装できます。 ただし、既存のアプリは複雑であり、さまざまなチームによる多数の開発の結果となっています。 そのため、新しいテクノロジを使用して最初から書き換えることはできません。 チームは、既存のコードベースに新機能を追加するための最適な方法を探しています。

このチュートリアルの冒頭では、Contoso の経費が .NET Framework 4.7.2 をターゲットとし、次のサードパーティ製のライブラリを使用します。

* MVVM Light。 MVVM パターンの基本実装です。
* Unity (依存関係挿入コンテナー)。
* LiteDb は、データを格納する埋め込みの NoSQL ソリューションです。
* 偽のデータを生成するためのツール。

このチュートリアルでは、新しい Windows 機能で Contoso の支出を強化します。

* 既存の WPF アプリを .NET Core 3.0 に移行します。 これにより、今後新しい重要なシナリオが開きます。
* Windows Community Toolkit によって提供される**system.windows.controls.inkcanvas>** および**mapcontrol**のラップされたコントロールをホストするには、XAML Islands を使用します。
* XAML Islands を使用して、標準の UWP XAML コントロール (この場合は**CalendardView**) をホストします。
* アダプティブカードと Windows 10 の通知をアプリに統合します。
* アプリを MSIX でパッケージ化し、Azure DevOps に CI/CD パイプラインを設定して、使用可能になったらすぐに新しいバージョンのアプリをテスト担当者とユーザーに自動的に配信できるようにします。

## <a name="prerequisites"></a>前提条件

このチュートリアルを実行するには、開発用コンピューターに次の前提条件がインストールされている必要があります。

* Windows 10 バージョン 1903 (ビルド 18362) 以降のバージョン。
* [Visual Studio 2019](https://www.visualstudio.com)。
* [.Net Core 3 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0)(最新バージョンをインストールします)。

Visual Studio 2019 では、次のワークロードとオプション機能をインストールしていることを確認します。

* .NET デスクトップ開発
* ユニバーサル Windows プラットフォームの開発
* Windows 10 SDK (10.0.18362.0 以降)

## <a name="get-the-contoso-expenses-sample-app"></a>Contoso の経費のサンプルアプリを入手する

チュートリアルを開始する前に、Contoso の経費アプリのソースコードをダウンロードし、Visual Studio でコードをビルドできることを確認します。

1. [Appconsult WinAppsModernization ワークショップリポジトリ](https://github.com/Microsoft/AppConsult-WinAppsModernizationWorkshop)の **[リリース]** タブからアプリのソースコードをダウンロードします。 直接リンクは[https://aka.ms/wamwc](https://aka.ms/wamwc)です。
2. Zip ファイルを開き、すべてのコンテンツを**C:\\** ドライブのルートに抽出します。 **C:\WinAppsModernizationWorkshop**という名前のフォルダーが作成されます。
3. Visual Studio 2019 を開き、 **C:\WinAppsModernizationWorkshop\Lab\Exercise1\01-Start\ContosoExpenses\ContosoExpenses.sln**ファイルをダブルクリックしてソリューションを開きます。
4. **[スタート]** ボタンまたは CTRL キーを押しながら F5 キーを押して、Contoso の経費の WPF プロジェクトをビルド、実行、およびデバッグできることを確認します。

## <a name="get-started"></a>作業開始

Contoso の経費サンプルアプリのソースコードを取得し、それを Visual Studio でビルドできることを確認できるので、チュートリアルを開始する準備ができています。

* [パート 1: Contoso の経費アプリを .NET Core 3 に移行する](modernize-wpf-tutorial-1.md)
* [パート 2: XAML Islands を使用した UWP InkCanvas コントロールの追加](modernize-wpf-tutorial-2.md)
* [パート 3:XAML Islands を使用した UWP CalendarView コントロールの追加](modernize-wpf-tutorial-3.md)
* [パート 4:Windows 10 ユーザー アクティビティと通知の追加](modernize-wpf-tutorial-4.md)
* [パート 5:MSIX によるパッケージとデプロイ](modernize-wpf-tutorial-5.md)

## <a name="important-concepts"></a>重要な概念

以下のセクションでは、このチュートリアルで説明する主な概念の背景について説明します。 これらの概念を既に理解している場合は、このセクションを省略できます。

### <a name="universal-windows-platform-uwp"></a>ユニバーサル Windows プラットフォーム (UWP)

Windows 8 では、Windows ランタイム (WinRT) と呼ばれる新しいフレームワークが導入されました。 .NET Framework とは異なり、WinRT は、アプリに直接公開されている Api のネイティブ層です。 また、WinRT には、ランタイムの上に追加されたレイヤーである言語のプロジェクションも導入されておりC# 、開発者は、 C++やなどの言語を使用して、との対話を行うことができます。 プロジェクションを使用すると、開発者は、.NET Framework を使用しC#たアプリの構築で取得したものと同じおよび XAML の知識を活用して、WinRT 上にアプリを構築できます。 

Windows 10 では、WinRT の上に構築された[ユニバーサル Windows プラットフォーム (UWP)](/windows/uwp/get-started/universal-application-platform-guide)が導入されました。 UWP の最も重要な機能は、すべてのデバイスプラットフォームで共通の Api セットを提供することです。アプリがデスクトップで実行されている場合でも、Xbox One でも HoloLens でも、同じ Api を使用できます。

今後、Windows 10 の新機能のほとんどは、タイムライン、プロジェクトローマ、Windows Hello などの機能を含む、WinRT Api を介して公開されています。

### <a name="msix-packaging"></a>MSIX パッケージ化

[Msix](http://aka.ms/msix)(旧称 AppX) は、Windows アプリの最新のパッケージモデルです。 MSIX は UWP アプリだけでなく、Win32、WPF、Windows フォーム、Java、電子などのテクノロジを使用して構築されたデスクトップアプリもサポートしています。 MSIX パッケージでデスクトップアプリをパッケージ化する場合、アプリを Microsoft Store に発行できます。 デスクトップアプリでは、インストール時にパッケージ id を取得することもできます。これにより、デスクトップアプリでより広範な WinRT Api のセットを使用できるようになります。

詳細については、次の記事を参照してください。

* [パッケージデスクトップアプリケーション](/windows/uwp/porting/desktop-to-uwp-root)
* [パッケージ化されたデスクトップアプリケーションの背後](/windows/uwp/porting/desktop-to-uwp-behind-the-scenes)

### <a name="xaml-islands"></a>XAML Islands

Windows 10 バージョン 1903 以降では、*XAML Islands* と呼ばれる機能を使用して UWP 以外のデスクトップ アプリで UWP コントロールをホストできます。 この機能では、UWP コントロールを介してのみ使用できる最新の Windows 10 の UI 機能を使用して既存のデスクトップ アプリの外観、操作性、および機能性を向上させることができます。 つまり、既存のWPF、Windowsフォーム、およびC++Win32アプリでは、Windows Ink などの UWP 機能と Fluent Design System をサポートするコントロールを使用できます。

詳細については、[デスクトップ アプリケーション (XAML Islands) の UWP コントロール](/windows/uwp/xaml-platform/xaml-host-controls) をご参照ください。 このチュートリアルでは、2 つの異なるタイプの XAML Islandsコントロールを使用するプロセスを案内します。

* Windows Community Toolkit の[system.windows.controls.inkcanvas>](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)と[mapcontrol](https://docs.microsoft.com/en-us/windows/communitytoolkit/controls/wpf-winforms/mapcontrol) 。 これらの WPF コントロールは、対応する UWP コントロールのインターフェイスと機能をラップし、Visual Studio デザイナーの他の WPF コントロールと同様に使用できます。

* UWP[カレンダービュー](/windows/uwp/design/controls-and-patterns/calendar-view)コントロール。 これは、Windows Community Toolkit の[Windowsxamlhost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)コントロールを使用してホストする標準 UWP コントロールです。

### <a name="net-core-3"></a>.NET Core 3

[.Net Core](https://docs.microsoft.com/dotnet/core/)は、完全な .NET Framework のクロスプラットフォーム、軽量、および拡張可能なバージョンを実装するオープンソースフレームワークです。 完全な .NET Framework と比較すると、.NET Core の起動時間が大幅に短縮され、Api の多くが最適化されています。

最初のいくつかのリリースでは、web またはバックエンドアプリをサポートするために .NET Core の焦点を絞っていました。 .NET Core を使用すると、Windows、Linux、または Docker コンテナーのようなマイクロサービスアーキテクチャでホストできるスケーラブルな web アプリまたは Api を簡単に構築できます。

.NET Core 3 は .NET Core の最新リリースです。 このリリースの強調表示は、Windows フォームと WPF アプリを含む Windows デスクトップアプリをサポートしています。 .NET Core 3 で新規および既存の Windows デスクトップアプリを実行して、.NET Core が提供するすべての利点を活用することができます。 [XAML Islands](xaml-islands.md)でホストされている UWP コントロールを、.NET Core 3 を対象とする Windows フォームや WPF アプリでも使用できます。

> [!NOTE]
> WPF と Windows フォームがクロスプラットフォームになっていないため、Linux および MacOS で WPF や Windows フォームを実行することはできません。 WPF と Windows フォームの UI コンポーネントは、引き続き Windows レンダリングシステムに依存しています。

詳細については、「 [.Net Core 3.0 の新機能](https://docs.microsoft.com/dotnet/core/whats-new/dotnet-core-3-0)」を参照してください。
