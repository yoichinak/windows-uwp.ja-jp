---
description: 新しい Windows デスクトップ アプリを作成する場合、最初に決める必要があるのは、Win32 と COM API または .NET のどちらを使用するかということです。
ms.assetid: 82705644-F1F0-40F3-99B1-7A97BFB32831
title: Windows アプリ プラットフォームの選択
ms.topic: article
ms.date: 02/03/2021
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
keywords: windows win32, デスクトップ開発
ms.openlocfilehash: 62567b36d16e01fc6091f9514137c60dc352942a
ms.sourcegitcommit: 382ae62f9d9bf980399a3f654e40ef4f85eae328
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/04/2021
ms.locfileid: "99534381"
---
# <a name="choose-your-windows-app-platform"></a>Windows アプリ プラットフォームの選択

Windows PC 用の新しいデスクトップ アプリケーションを作成する場合は、まず、どのアプリケーション プラットフォームを使用するかを決定します。 Windows には、次の 4 つの主要なアプリケーション プラットフォームが用意されており、それぞれに長所があります。

* [ユニバーサル Windows プラットフォーム (UWP)](#uwp): このプラットフォームには、Windows 10 を実行するすべてのデバイスに共通の型システム、API、およびアプリケーション モデルが用意されています。 UWP アプリケーションは、ネイティブでもマネージドでもかまいません。
* [WPF](#wpf) と [Windows フォーム](#windows-forms): これらの .NET ベースのプラットフォームには、マネージド アプリケーションに共通の型システム、API、およびアプリケーション モデルが用意されています。
* [Win32](#win32): これは、Windows とハードウェアへの直接アクセスを必要とするネイティブ C/C++ の Windows アプリケーション向けの最初のプラットフォームです。 これにより、Win32 API 最高レベルのパフォーマンスとシステム ハードウェアへの直接アクセスを必要とするアプリケーションに適したプラットフォームになっています。

これらの各プラットフォームには、一連の UI フレームワークおよび UI コントロールが備わっています。それを使用すると、従来の Windows デスクトップで実行される Word、Excel、Photoshop などのデスクトップ アプリを作成し、その環境固有の機能を最大限に活用できます。 Windows 10 では、この各プラットフォームで [Windows UI ライブラリ (WinUI)](#use-the-windows-ui-library-with-windows-apps) を使用したユーザー インターフェイスの作成がサポートされています。

これらのプラットフォームの一部はいくつかの特徴を共有し、特定の種類のアプリケーションにより適しています。 たとえば、UWP と .NET はどちらも Visual Studio と緊密に統合されています。 これには、特に開発者の生産性、洗練されたカスタマイズ可能な UI、アプリケーションのセキュリティの領域において、多くの利点があります。 これらのフレームワークは、UI を迅速に作成するためのビジュアル デザイナーと UI マークアップをサポートしているため、基幹業務アプリケーションに特に適しています。

> [!NOTE]
> どのアプリ プラットフォームを選択しても、Windows 10 の多くの機能を使用して、アプリで最新のエクスペリエンスを実現できます。 たとえば、デスクトップ アプリが WPF、Windows フォーム、または Win32 API を使用して構築されている場合でも、MSIX パッケージの展開を引き続き利用できます。 デスクトップ アプリを現代化するためのすべての方法の詳細については、「[デスクトップ アプリの現代化](modernize/index.md)」を参照してください。

## <a name="uwp"></a>UWP

UWP は、Windows 10 アプリケーションおよびゲーム用の最先端のプラットフォームです。 XAML マークアップを使用してコード (ビジネス ロジック) から UI (プレゼンテーション) を分離する、高度にカスタマイズ可能なプラットフォームです。 UWP は、高度な UI、スタイルのカスタマイズ、およびグラフィックス集中型のシナリオを必要とするデスクトップ アプリケーションに適しています。 また、UWP には、既定の UX エクスペリエンスのための [Fluent Design System](/windows/uwp/design/fluent-design-system/) の組み込みサポートもあり、[Windows ランタイム (WinRT) API](/windows/uwp/get-started/universal-application-platform-guide#how-the-universal-windows-platform-relates-to-windows-runtime-apis) へのアクセスを提供します。 Fluent を導入することにより、UWP では一般的な入力方式 (インク、タッチ、ゲームパッド、キーボード、マウスなど) が自動的にサポートされます。

UWP は、Windows PC 用のデスクトップ アプリケーションを作成できるだけでなく、Xbox、HoloLens、および Surface Hub アプリケーションで唯一サポートされているプラットフォームでもあります。 UWP は最新で最先端のアプリケーション プラットフォームです。

UWP の詳細については、次の記事を参照してください。

* [作業開始](/windows/uwp/get-started/)
* [プロジェクト テンプレート](visual-studio-templates.md#uwp-templates)
* [設計と UI](/windows/uwp/design/)
* [テクノロジと機能](/windows/uwp/develop/)
* [API リファレンス](/uwp/)
* [サンプル](https://github.com/Microsoft/Windows-universal-samples)

## <a name="wpf"></a>WPF

WPF は、.NET Core にアクセスできるか、.NET Framework にフル アクセスできるマネージド Windows アプリケーション用に確立されたプラットフォームであり、XAML マークアップを使用してコードから UI を分離します。 このプラットフォームは、高度な UI、スタイルのカスタマイズ、およびグラフィックス集中型のシナリオを必要とするデスクトップ アプリケーションのために設計されています。 WPF の開発スキルは UWP の開発スキルに似ているため、WPF から UWP アプリへの移行は Windows フォームからの移行よりも簡単です。

WPF の詳細については、次の記事を参照してください。

* [概要 (WPF)](/dotnet/framework/wpf/getting-started/)
* [プロジェクト テンプレート](visual-studio-templates.md#net-templates)
* [初めてのアプリの作成 (.NET Core)](/visualstudio/get-started/csharp/tutorial-wpf/)
* [初めてのアプリの作成 (.NET Framework)](/dotnet/framework/wpf/getting-started/walkthrough-my-first-wpf-desktop-application/)
* [WPF アプリを .NET Core に移行する](/dotnet/desktop-wpf/migration/convert-project-from-net-framework/)
* [API リファレンス (.NET)](/dotnet/api/index)
* [サンプル](https://github.com/Microsoft/WPF-Samples)

## <a name="windows-forms"></a>Windows フォーム

Windows フォームは、軽量 UI モデルや、.NET Core へのアクセスと .NET Framework へのフル アクセスを備え、Windows マネージド アプリケーションのために用意された最初のプラットフォームです。 プラットフォームを初めて使用する場合であっても、開発者がアプリケーションの構築をすぐに開始できる点で優れています。 これは、ビジュアルおよび非ビジュアルのドラッグアンドドロップ コントロールの大規模な組み込みコレクションを備えた、フォームベースの高速なアプリケーション開発プラットフォームです。 Windows フォームでは XAML を使用しないため、後でアプリケーションを UWP に拡張する場合は、UI を完全に再記述する必要があります。

Windows フォームの詳細については、次の記事を参照してください。

* [Windows フォームについて](/dotnet/framework/winforms/getting-started-with-windows-forms)
* [プロジェクト テンプレート](visual-studio-templates.md#net-templates)
* [初めての Windows フォーム アプリの作成](/dotnet/framework/winforms/creating-a-new-windows-form)
* [チュートリアル: ピクチャ ビューアーの作成](/visualstudio/ide/tutorial-1-create-a-picture-viewer)
* [API リファレンス (.NET)](/dotnet/api/index)
* [Windows フォーム アプリの拡張](/dotnet/framework/winforms/advanced/)

## <a name="win32"></a>Win32

C++ で Win32 API を使用すると、WinRT や .NET などのマネージド ランタイム環境以上のターゲット プラットフォームの制御の強化をアンマネージド コードを使用して行うことで、最高レベルのパフォーマンスと効率を実現できます。 ただし、アプリケーションの実行に対してこのようなレベルの制御を行うには、正しく行うための十分な慎重さと注意が必要です。また、実行時のパフォーマンスと開発の生産性が引き換えになります。

ここでは、高パフォーマンスのアプリケーションの構築を可能にする、Win32 API と C++ が提供するものについて説明します。

* リソース割り当て、オブジェクトの有効期間、データ レイアウト、アラインメント、バイト パッキングなどの厳密な制御を含むハードウェア レベルの最適化。
* 組み込み関数による SSE や AVX などのパフォーマンス指向の命令セットへのアクセス。
* テンプレートを使用した、効率的でタイプセーフな汎用プログラミング。
* 効率的で安全なコンテナーとアルゴリズム。
* DirectX、特に Direct3D および DirectCompute (UWP は DirectX 相互運用機能も提供)。

詳細については、以下の記事を参照してください。

* [作業開始](/windows/win32/desktop-programming/)
* [プロジェクト テンプレート](visual-studio-templates.md#cwin32-templates)
* [初めての Win32 および C++ アプリの作成](/windows/win32/learnwin32/learn-to-program-for-windows/)
* [テクノロジと機能](/windows/win32/desktop-app-technologies)
* [API リファレンス](/windows/win32/apiindex/windows-api-list/)
* [サンプル](https://github.com/Microsoft/Windows-classic-samples)

## <a name="platform-comparison-uwp-wpf-and-windows-forms"></a>プラットフォームの比較:UWP、WPF、Windows フォーム

次の表は、Windows フォーム、WPF、UWP のさまざまな特性を詳細に比較したものです。

| 機能またはシナリオ  |    UWP     |      WPF     |   Windows フォーム  |
|--------|--------|--------|--------|
| **サポートされているバージョン**      |  Windows 10   |  Windows 7 以降 |  Windows 7 以降  |
| **言語**      |   C\#、 C++/WinRT、 C++/CX、VB、JavaScript   |  C\#、 C++/CLI (C++ マネージド拡張)、F\#、VB |  C\#、 C++/CLI (C++ マネージド拡張)、F\#、VB   |
| **UI ランタイム** |    ネイティブ (C++WinRT および C++/CX) とマネージド (.NET ネイティブ)  |  マネージド (.NET Framework および .NET Core 3) |   マネージド (.NET Framework および .NET Core 3)   |
| **オープン ソース** | [はい (Windows UI ライブラリのみ)](https://github.com/Microsoft/microsoft-ui-xaml)  |  [はい (.NET Core のみ)](https://github.com/dotnet/wpf) | [はい (.NET Core のみ)](https://github.com/dotnet/winforms)  |
| **XAML をサポート** |   はい   |  はい  |   いいえ   |
| **特長**  |  <ul><li>UI の XAML マークアップ</li><li>豊富でカスタマイズ可能な UX</li><li>既存のコードベースは .NET Standard 対応</li><li>高 DPI サポート</li><li>Windows デバイス間での複数の入力の種類のサポート (タッチ、ペン、ゲームパッド、マウス、キーボードなど)</li><li>Xbox、HoloLens、IoT、または Surface Hub のサポート</li><li>オプションの高密度 (コンパクト) UI</li><li>ネイティブ C++ のサポート</li><li>最適化されたバッテリー寿命</li><li>最新のアクセシビリティ サポート (スクリーン リーダーなど)</li><li>リッチ テキスト データ機能 (組み込みのスペル チェックなど)</li><li>手描き入力のサポート</li><li>アプリケーション コンテナーによるセキュリティで保護された実行 (たとえば、信頼されていないコンテンツのサンドボックス化)</li></ul>  |  <ul><li>UI の XAML マークアップ</li><li>豊富でカスタマイズ可能な UX</li><li>Microsoft とパートナーのコントロールの大規模なコレクション</li><li>高密度 UI</li><li>Windows 7 のサポート</li><li>入力検証のプラットフォームのサポート</li></ul> | <ul><li>迅速なアプリケーション開発</li><li>UI を構築するための WYSIWYG エディター</li><li>Microsoft とパートナーのコントロールの大規模なコレクション</li><li>高密度 UI</li><li>Windows 7 のサポート</li><li>キーボードとマウス入力</li></ul>          |
| **サポートが限定されているシナリオ** |  <ul><li>複数のウィンドウのサポート <sup>1</sup></li><li>入力検証のプラットフォームのサポート <sup>1</sup></li><li>Windows 7 はサポートされません</li><li>一部の Windows ランタイム API では、Windows 10 の特定の最小バージョンが必要です</li><li>完全なプラットフォームのサポートとシェルの統合 (たとえば、現在、UWP はシステム トレイの統合またはすべてのデバイスへのフル アクセスをサポートしていません)</li><li>ディスク上のすべてのファイルへの直接アクセス</li><li>ADO.NET</li><li>.NET Standard 以外または Windows アプリ認定キット以外に準拠した API を使用する既存のコードベース クラス ライブラリ</li><li>ローカル ネットワーク ループバックのサポート (つまり、アプリがターゲット デバイスにループバックの除外を作成せずに localhost と通信する必要がある場合)</li><li>集中型のファイル I/O</li></ul>     |  <ul><li>高 DPI サポート <sup>2</sup></li><li>タッチ入力 <sup>2</sup></li></ul>  |  <ul><li>高 DPI サポート <sup>2</sup></li><li>タッチ入力 <sup>2</sup></li><li>カスタマイズ可能な UI</li><li>豊富なグラフィックスとユーザー エクスペリエンス (タッチやアニメーションなど)</li><li>ビューとデータ モデルの豊富な抽象化</li></ul>    |   |

<sup>1</sup> Windows 10 の今後のリリースでこのシナリオに対処する機能が公開されています。

<sup>2</sup> プラットフォームには、このシナリオに対する最上級の API のサポートがありませんが、開発者はこのシナリオを回避策によりサポートできます。

## <a name="use-the-windows-ui-library-with-windows-apps"></a>Windows アプリで Windows UI ライブラリを使用する

Windows のメインのアプリ プラットフォームを補うために、アプリで [Windows UI ライブラリ (WinUI)](../winui/index.md) を使用することもできます。 WinUI は、ダウンレベル バージョンの Windows 10 を対象とする UWP アプリ用の WinRT コントロールの新しいバージョンと更新バージョンを提供するツールキットとして開始されました。 WinUI 3 (まだプレビュー段階) の時点で、WinUI は、UWP、.NET、および Win32 アプリ プラットフォームにわたって Windows 10 アプリのプレミア ネイティブ ユーザー インターフェイス (UI) フレームワークになるように範囲が拡大しています。

Windows アプリでは、次の方法で WinUI を使用できます。

* [WinUI 2.x](../winui/winui2/index.md):
  * UWP アプリでは、Windows SDK によって提供される WinRT コントロールの代わりに、WinUI 2.x コントロールを使用できます。 WinUI のこれらのリリースには、刷新されたコントロールと、Windows SDK からの既存のコントロールの更新されたバージョンの両方が含まれています。
  * WPF、Windows フォーム、および C++/Win32 の既存のアプリを更新し、[XAML Islands](modernize/xaml-islands.md) を使用して WinUI 2.x コントロールをホストできます。

* [WinUI 3 (プレビュー段階)](../winui/winui3/index.md):
  * WinUI 3 以降では、WinUI ベースの UI を全面的に使用する [.NET アプリと C++/Win32 アプリ](../winui/winui3/get-started-winui3-for-desktop.md)および [UWP アプリ](../winui/winui3/get-started-winui3-for-uwp.md)を作成できます。 このリリースには、これらのアプリを作成するために必要なすべてを提供する Visual Studio プロジェクト テンプレートが含まれています。

> [!NOTE]
> WinUI 3 はまだプレビュー段階であり、運用環境のアプリでは使用しないでください。

## <a name="other-app-platforms"></a>その他のアプリ プラットフォーム

### <a name="progressive-web-apps-pwas"></a>プログレッシブ Web アプリ (PWA)

PWA により、開発者は Web サイト コードをパッケージ化し、Windows 10 の PC 上のアプリケーションのようにインストールして実行できるようになります。 詳細については、[プログレッシブ Web アプリ](/microsoft-edge/progressive-web-apps-chromium/get-started)に関する記事を参照してください。

### <a name="xamarin"></a>Xamarin

Xamarin は、Windows 10 用でありながら iOS および Android でも実行できるクロスプラットフォーム アプリケーションの作成に使用します。 詳細については、[Xamarin](/xamarin/xamarin-forms/get-started/index)に関する記事を参照してください。

### <a name="uno-platform"></a>Uno Platform

Uno Platform は、Windows UWP ベースのコード (C# および XAML) を iOS、Android、macOS、Linux、WebAssembly で実行できるようにします。 [Windows 10 2004 (19041)](/windows/uwp/whats-new/windows-10-build-19041) での UWP に対する完全な API 定義と、[Windows.UI.Xaml](/uwp/api/windows.ui.xaml.documents) などの UWP API のパーツの実装を提供し、UWP アプリケーションをこれらのプラットフォームで実行できるようにします。 詳しくは、[Uno Platform に関するドキュメント](https://platform.uno/docs/articles/intro.html)をご覧ください。
