---
description: このチュートリアルでは、UWP XAML ユーザー インターフェイスを追加、MSIX パッケージを作成およびその他の最新コンポーネントを WPF アプリに組み込む方法を示します。
title: チュートリアル:WPF アプリを近代化します。
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: windows 10、uwp、windows フォーム、wpf、xaml islands
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 5e7179d4aeb66cad547e31e2456da2e8264ebbcd
ms.sourcegitcommit: 1eec0e4fd8a5ba82803fdce6e23fcd01b9488523
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/27/2019
ms.locfileid: "67420076"
---
# <a name="tutorial-modernize-a-wpf-app"></a>チュートリアル:WPF アプリを近代化します。 

さまざまな方法がある[最新化](index.md)既存に最新の Windows 機能を統合することで、既存のデスクトップ アプリのソース コードを最初からアプリの書き換えの代わりにします。 このチュートリアルではこれらの機能を使用して既存の WPF 基幹業務アプリを最新化するいくつかの方法について説明します。

* .NET Core 3
* XAML 諸島での UWP XAML コントロール
* Adaptive Cards および Windows 10 通知
* MSIX の展開

このチュートリアルでは、次の開発スキルが必要です。

* WPF での Windows デスクトップ アプリの開発で発生します。
* 基本的な知識C#と XAML。
* UWP の基本的な知識。

## <a name="overview"></a>概要

このチュートリアルでは、Contoso の経費をという名前の単純な WPF 基幹業務アプリのコードを提供します。 架空のチュートリアルのシナリオでは、Contoso の費用は内部アプリのレポートによって送信された経費を追跡する Contoso Corporation の管理者を使用します。 タッチ対応のデバイスで、管理者が備えているようになりましたし扱おうとするマウスやキーボードのない Contoso 経費アプリケーションを使用します。 残念ながら、アプリの現在のバージョンがタッチをいない表示します。

Contoso では、従業員が経費レポートをより効率的に作成を有効にする新規の Windows 機能でこのアプリを最新化します。 多くの機能は、新しい UWP アプリのビルドで簡単に実装する可能性があります。 ただし、既存のアプリは複雑で、何年にも異なるチームで開発の結果です。 そのため、新しいテクノロジを使用して最初から書き直すことによって、オプションはされていません。 チームは既存のコードベースに新機能を追加する最善の方法に探しています。

チュートリアルの先頭には、Contoso 経費は .NET Framework 4.7.2 を対象とし、次のサード パーティ製ライブラリを使用します。

* MVVM Light では、MVVM パターンの基本的な実装。
* Unity で依存関係の注入コンテナー。
* LiteDb、データを格納する組み込み型の NoSQL ソリューションです。
* 偽、仮のデータを生成するためのツール。

このチュートリアルで Windows の新機能と Contoso 経費を拡張します。

* 既存の WPF アプリを .NET Core 3.0 に移行します。 今後新しいと重要なシナリオを開くこれは。
* XAML Islands を使用して 、 Windows コミュニティのツールキットで提供される**InkCanvas**と**MapControl** ラップコントロールをホストします。
* XAML Islands Standard を使用して UWP XAML コントロールをホストする  ( **CalendardView**)。
* Adaptive Cards および Windows 10 の通知をアプリに統合します。
* パッケージが MSIX と、CI/CD のセットアップを使用してアプリを Azure DevOps のパイプラインのできるように、使用可能になるとすぐにテスト担当者とユーザーに新しいバージョンのアプリに提供することに自動的にことができます。

## <a name="prerequisites"></a>前提条件

このチュートリアルを実行するには、開発用コンピューターにインストールされているこれらの前提条件が必要です。

* Windows 10、バージョン 1903 (18362 ビルド) またはそれ以降のバージョン。
* [Visual Studio 2019](https://www.visualstudio.com)します。
* [.NET core 3 Preview SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0) (最新の利用可能なプレビュー バージョンをインストールする)。

Visual Studio 2019 で、次のワークロードと省略可能な機能をインストールすることを確認します。

* .NET デスクトップ開発
* ユニバーサル Windows プラットフォームの開発
* Windows 10 SDK (10.0.18362.0 またはそれ以降)

## <a name="get-the-contoso-expenses-sample-app"></a>Contoso の支出のサンプル アプリを入手します。

このチュートリアルを開始する前に、Contoso 経費アプリケーションのソース コードをダウンロードし、Visual Studio でコードをビルドするかどうかを確認します。

1. アプリのソース コードからのダウンロード、**リリース**のタブ、 [AppConsult WinAppsModernization ワーク ショップ リポジトリ](https://github.com/Microsoft/AppConsult-WinAppsModernizationWorkshop)。 直接リンクが[ https://aka.ms/wamwc](https://aka.ms/wamwc)します。
2. Zip ファイルを開き、すべてのコンテンツのルートを抽出、 **c:\\** ドライブ。 という名前のフォルダーが作成されます**C:\WinAppsModernizationWorkshop**します。
3. Visual Studio 2019 を開き、ダブルクリック、 **C:\WinAppsModernizationWorkshop\Lab\Exercise1\01-Start\ContosoExpenses\ContosoExpenses.sln**ソリューションを開くファイル。
4. 確認できるビルド、実行するには、キーを押して、Contoso 経費の WPF プロジェクトをデバッグ、**開始**ボタンまたは ctrl キーを押しながら f5 キー。

## <a name="get-started"></a>作業開始

後、Contoso の支出のサンプル アプリのソース コードがあり、Visual Studio で構築することを確認するには、チュートリアルを開始する準備ができました。

* [パート 1: Contoso の移行を .NET Core 3 経費アプリ](modernize-wpf-tutorial-1.md)
* [パート 2: XAML Islands を使用した UWP InkCanvas コントロールの追加](modernize-wpf-tutorial-2.md)
* [パート 3:XAML Islands を使用した UWP CalendarView コントロールの追加](modernize-wpf-tutorial-3.md)
* [パート 4:Windows 10 ユーザー アクティビティと通知の追加](modernize-wpf-tutorial-4.md)
* [パート 5:MSIX によるパッケージとデプロイ](modernize-wpf-tutorial-5.md)

## <a name="important-concepts"></a>重要な概念

次のセクションでは、このチュートリアルで説明されている主要な概念のいくつかの背景を提供します。 これらの概念を理解している場合は、このセクションをスキップできます。

### <a name="universal-windows-platform-uwp"></a>ユニバーサル Windows プラットフォーム (UWP)

Windows 8 では、Microsoft は、Windows Runtime (WinRT) と呼ばれる新しいフレームワークを導入しました。 WinRT とは異なり、.NET Framework の Api アプリに直接公開されているネイティブ レイヤーです。 WinRT 言語プロジェクションは、開発者などの言語を使用して、対話できるように、ランタイムの上に追加のレイヤーを導入もC#および JavaScript に加えてC++します。 プロジェクションには、同じを利用する WinRT の上にアプリを構築する開発者が有効にするC#と .NET Framework でのアプリの構築に取得する XAML の知識。 

Microsoft、Windows 10 で導入された、[ユニバーサル Windows プラットフォーム (UWP)](/windows/uwp/get-started/universal-application-platform-guide)、これは、WinRT 上に構築されます。 UWP の最も重要な機能は、すべてのデバイス プラットフォームで一般的な一連の Api を提供します。 いたとしても、デスクトップ、Xbox One で、または、HoloLens、アプリが実行されている場合、同じ Api を使用できるようにします。

機能は今後、最新の Windows 10 の機能のタイムライン、ローマのプロジェクト、および Windows こんにちはなどの WinRT Api を使用して公開されます。

### <a name="msix-packaging"></a>MSIX パッケージ化

[MSIX](http://aka.ms/msix) (旧称 AppX) は Windows アプリの最新のパッケージ化モデルです。 MSIX には、UWP アプリとデスクトップ アプリの Win32、WPF、Windows フォーム、Java、電子などのテクノロジを使用して構築がサポートされています。 MSIX パッケージ内のデスクトップ アプリをパッケージ化するときは、Microsoft Store にアプリを発行できます。 デスクトップ アプリは、インストールは、これにより、デスクトップ アプリをより広範な一連の WinRT Api を使用するときにもパッケージ id を取得します。

詳細については、次の記事を参照してください。

* [デスクトップ アプリケーションをパッケージ化](/windows/uwp/porting/desktop-to-uwp-root)
* [バック グラウンドでのパッケージ化されたデスクトップ アプリケーション](/windows/uwp/porting/desktop-to-uwp-behind-the-scenes)

### <a name="xaml-islands"></a>XAML 諸島

Windows 10 バージョン 1903 以降では、*XAML Islands* と呼ばれる機能を使用して UWP 以外のデスクトップ アプリで UWP コントロールをホストできます。 この機能では、UWP コントロールを介してのみ使用できる最新の Windows 10 の UI 機能を使用して既存のデスクトップ アプリの外観、操作性、および機能性を向上させることができます。 つまり、既存のWPF、Windowsフォーム、およびC++Win32アプリでは、Windows Ink などの UWP 機能と Fluent Design System をサポートするコントロールを使用できます。

詳細については、[デスクトップ アプリケーション (XAML Islands) の UWP コントロール](/windows/uwp/xaml-platform/xaml-host-controls) をご参照ください。 このチュートリアルでは、2 つの異なるタイプの XAML Islandsコントロールを使用するプロセスを案内します。

* [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)と[MapControl](https://docs.microsoft.com/en-us/windows/communitytoolkit/controls/wpf-winforms/mapcontrol) Windows コミュニティ ツールキット。 これらの WPF コントロールでは、インターフェイスとの対応する UWP コントロールの機能をラップして、Visual Studio デザイナーでその他の WPF コントロールのように使用できます。

* UWP[カレンダー ビュー](/windows/uwp/design/controls-and-patterns/calendar-view)コントロール。 これは、標準的な UWP コントロールを使用してホストする、 [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) Windows の Community Toolkit のコントロール。

### <a name="net-core-3"></a>.NET Core 3

[.NET core](https://docs.microsoft.com/dotnet/core/)は完全な .NET Framework のクロスプラット フォームで、軽量で簡単に拡張可能なバージョンを実装するオープン ソース フレームワークです。 完全な .NET Framework と比較して、.NET Core の起動時間がはるかに高速と、Api の多くに最適化されています。

最初のいくつかのリリースでは、.NET Core のフォーカスは、web アプリやバックエンド アプリをサポートするためでした。 .NET Core では、スケーラブルな web アプリまたは Windows、Linux、または Docker コンテナーなどのマイクロ サービス アーキテクチャでホストされる Api を簡単に作成できます。

.NET Core 3 は、.NET Core の次期メジャー リリースです。 このリリースのハイライトは、Windows フォーム、WPF アプリを含む Windows デスクトップ アプリのサポートです。 .NET Core 3 で新規および既存の Windows デスクトップ アプリを実行し、.NET Core で提供するすべての特典を利用できます。 [XAML Islands](xaml-islands.md)でホストされている UWP コントロールを、.NET Core 3 を対象とする Windows フォームや WPF アプリでも使用できます。

> [!NOTE]
> WPF と Windows フォームがないクロス プラットフォームになることと、WPF や Windows フォームは、Linux と MacOS で実行することはできません。 WPF と Windows フォームの UI コンポーネントは、Windows レンダリング システムに依存関係があります。

詳しくは、次の記事をご覧ください。

* [.NET Core 3.0 Preview 1 のお知らせ](https://devblogs.microsoft.com/dotnet/announcing-net-core-3-preview-1-and-open-sourcing-windows-desktop-frameworks/)
* [.NET Core 3.0 Preview 2 のお知らせ](https://devblogs.microsoft.com/dotnet/announcing-net-core-3-preview-2/)
* [.NET Core 3.0 Preview 2 のお知らせ](https://devblogs.microsoft.com/dotnet/announcing-net-core-3-preview-3/)
* [.NET Core 3.0 Preview 4 のお知らせ](https://devblogs.microsoft.com/dotnet/announcing-net-core-3-preview-4/)
* [.NET Core 3.0 の新着情報](https://docs.microsoft.com/dotnet/core/whats-new/dotnet-core-3-0)
