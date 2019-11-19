---
Description: 新しいデスクトップ アプリを作成する場合、最初に決める必要があるのは、Win32 と COM API または .NET のどちらを使用するかということです。
ms.assetid: 82705644-F1F0-40F3-99B1-7A97BFB32831
title: アプリ プラットフォームの選択
ms.topic: article
ms.date: 03/18/2019
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: 57f3101b1134b4fdceb6a38f392e54677d2b884d
ms.sourcegitcommit: f3dd633a3149d2e206981fa52ad424d408e5508c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/07/2019
ms.locfileid: "73799575"
---
# <a name="choose-your-app-platform"></a>アプリ プラットフォームの選択

Windows PC 用の新しいデスクトップ アプリケーションを作成する場合は、まず、どのアプリケーション プラットフォームを使用するかを決定します。 Windows には、次の 4 つの主要なアプリケーション プラットフォームが用意されており、それぞれに長所があります。

* [ユニバーサル Windows プラットフォーム (UWP)](#uwp)
* [WPF (.NET)](#wpf)
* [Windows フォーム (.NET)](#windows-forms)
* [Win32](#win32)

これらのアプリケーション プラットフォームでは、従来の Windows デスクトップで実行される Word、Excel、Photoshop などのデスクトップ アプリを作成し、その環境固有の機能を最大限に活用することができます。 ただし、これらのプラットフォームの一部はいくつかの特徴を共有し、特定の種類のアプリケーションにより適しています。

* **UWP、WPF、Windows フォーム**。 これらのプラットフォームは、特に開発者の生産性、洗練されたカスタマイズ可能な UI、アプリケーションのセキュリティの領域において、マネージド ランタイム環境 (UWP 用の Windows ランタイムと、Windows フォームと WPF 用の .NET) を提供します。 これらのフレームワークは、UI を迅速に作成するためのビジュアル デザイナーと UI マークアップをサポートしているため、基幹業務アプリケーションに特に適しています。

* **Win32 API**。 WIN32 API (Windows API とも呼ばれます) は、Windows とハードウェアへの直接アクセスを必要とするネイティブ C/C++ の Windows アプリケーションのために用意された最初のプラットフォームです。 .NET や WinRT などのマネージド ランタイム環境に依存することなく、最上級の開発環境を提供します。 これにより、Win32 API 最高レベルのパフォーマンスとシステム ハードウェアへの直接アクセスを必要とするアプリケーションに適したプラットフォームになっています。

この記事は、これらのプラットフォームについて詳しく説明し、アプリケーションに最適なプラットフォームを決定するのに役立ちます。 

> [!NOTE]
> どのアプリ プラットフォームを選択しても、ユニバーサル Windows プラットフォーム (UWP) の多くの機能を使用して、Windows 10 でアプリの最新のエクスペリエンスを提供できます。 たとえば、デスクトップ アプリが WPF、Windows フォーム、または Win32 API を使用して構築されている場合でも、MSIX パッケージの展開や UWP XAML コントロールなど、UWP で初めて導入された多くの機能を使用できます。 詳しくは、「[デスクトップ アプリの現代化](modernize/index.md)」を参照してください。

## <a name="uwp"></a>UWP

UWP は、Windows 10 アプリケーションおよびゲーム用の最先端のプラットフォームです。 XAML マークアップを使用してコード (ビジネス ロジック) から UI (プレゼンテーション) を分離する、高度にカスタマイズ可能なプラットフォームです。 UWP は、高度な UI、スタイルのカスタマイズ、およびグラフィックス集中型のシナリオを必要とするデスクトップ アプリケーションに適しています。 また、UWP には、既定の UX エクスペリエンスのための [Fluent Design System](/windows/uwp/design/fluent-design-system/) の組み込みサポートもあり、[Windows ランタイム (WinRT) API](/windows/uwp/get-started/universal-application-platform-guide#how-the-universal-windows-platform-relates-to-windows-runtime-apis) へのアクセスを提供します。 Fluent を導入することにより、UWP では一般的な入力方式 (インク、タッチ、ゲームパッド、キーボード、マウスなど) が自動的にサポートされます。

UWP は、Windows PC 用のデスクトップ アプリケーションを作成できるだけでなく、Xbox、HoloLens、および Surface Hub アプリケーションで唯一サポートされているプラットフォームでもあります。 UWP は最新で最先端のアプリケーション プラットフォームです。

UWP の詳細については、「[Windows 10 アプリの概要](/windows/uwp/get-started/)」を参照してください。

## <a name="wpf"></a>WPF

WPF は、.NET Framework にフル アクセスできるマネージド Windows アプリケーション用に確立されたプラットフォームであり、XAML マークアップを使用してコードから UI を分離します。 このプラットフォームは、高度な UI、スタイルのカスタマイズ、およびグラフィックス集中型のシナリオを必要とするデスクトップ アプリケーションのために設計されています。 WPF の開発スキルは UWP の開発スキルに似ているため、WPF から UWP アプリへの移行は Windows フォームからの移行よりも簡単です。

詳細については、「[はじめに (WPF)](https://docs.microsoft.com/dotnet/framework/wpf/getting-started/)」を参照してください。

## <a name="windows-forms"></a>Windows フォーム

Windows フォームは、軽量 UI モデルと .NET Framework へのフル アクセスを備え、Windows マネージド アプリケーションのために用意された最初のプラットフォームです。 プラットフォームを初めて使用する場合であっても、開発者がアプリケーションの構築をすぐに開始できる点で優れています。 これは、ビジュアルおよび非ビジュアルのドラッグアンドドロップ コントロールの大規模な組み込みコレクションを備えた、フォームベースの高速なアプリケーション開発プラットフォームです。 Windows フォームでは XAML を使用しないため、後でアプリケーションを UWP に拡張する場合は、UI を完全に再記述する必要があります。

Windows フォームの詳細については、「[Windows フォームについて](https://docs.microsoft.com/dotnet/framework/winforms/getting-started-with-windows-forms)」を参照してください。

## <a name="platform-comparison-uwp-wpf-and-windows-forms"></a>プラットフォームの比較:UWP、WPF、Windows フォーム

次の表は、Windows フォーム、WPF、UWP のさまざまな特性を詳細に比較したものです。

| 機能またはシナリオ  |    UWP     |      WPF     |   Windows フォーム  |
|--------|--------|--------|--------|
| **サポートされているバージョン**      |  Windows 10   |  Windows 7 以降 |  Windows 7 以降  |
| **言語**      |   C\#、 C++/WinRT、 C++/CX、VB、JavaScript   |  C\#、 C++/CLI (C++ マネージド拡張)、F\#、VB |  C\#、 C++/CLI (C++ マネージド拡張)、F\#、VB   |
| **UI ランタイム** |    ネイティブ (C++WinRT および C++/CX) とマネージド (.NET ネイティブ)  |  マネージド (.NET Framework および .NET Core 3) |   マネージド (.NET Framework および .NET Core 3)   |
| **オープン ソース** | [はい (Windows UI ライブラリのみ)](https://github.com/Microsoft/microsoft-ui-xaml)  |  [はい (.NET Core のみ)](https://github.com/dotnet/wpf) | [はい (.NET Core のみ)](https://github.com/dotnet/winforms)  |
| **XAML をサポート** |   〇   |  〇  |   X   |
| **特長**  |  <ul><li>UI の XAML マークアップ</li><li>豊富でカスタマイズ可能な UX</li><li>既存のコードベースは .NET Standard 対応</li><li>高 DPI サポート</li><li>Windows デバイス間での複数の入力の種類のサポート (タッチ、ペン、ゲームパッド、マウス、キーボードなど)</li><li>Xbox、HoloLens、IoT、または Surface Hub のサポート</li><li>オプションの高密度 (コンパクト) UI</li><li>ネイティブ C++ のサポート</li><li>最適化されたバッテリー寿命</li><li>最新のアクセシビリティ サポート (スクリーン リーダーなど)</li><li>リッチ テキスト データ機能 (組み込みのスペル チェックなど)</li><li>手描き入力のサポート</li><li>アプリケーション コンテナーによるセキュリティで保護された実行 (たとえば、信頼されていないコンテンツのサンドボックス化)</li></ul>  |  <ul><li>UI の XAML マークアップ</li><li>豊富でカスタマイズ可能な UX</li><li>Microsoft とパートナーのコントロールの大規模なコレクション</li><li>高密度 UI</li><li>Windows 7 のサポート</li><li>入力検証のプラットフォームのサポート</li></ul> | <ul><li>迅速なアプリケーション開発</li><li>UI を構築するための WYSIWYG エディター</li><li>Microsoft とパートナーのコントロールの大規模なコレクション</li><li>高密度 UI</li><li>Windows 7 のサポート</li><li>キーボードとマウス入力</li></ul>          |
| **サポートが限定されているシナリオ** |  <ul><li>複数のウィンドウのサポート <sup>1</sup></li><li>入力検証のプラットフォームのサポート <sup>1</sup></li><li>Windows 7 はサポートされません</li><li>一部の UWP API では、Windows 10 の特定の最小バージョンが必要です</li><li>完全なプラットフォームのサポートとシェルの統合 (たとえば、現在、UWP はシステム トレイの統合またはすべてのデバイスへのフル アクセスをサポートしていません)</li><li>ディスク上のすべてのファイルへの直接アクセス</li><li>ADO.NET</li><li>.NET Standard 以外または Windows アプリ認定キット以外に準拠した API を使用する既存のコードベース クラス ライブラリ</li><li>ローカル ネットワーク ループバックのサポート (つまり、アプリがターゲット デバイスにループバックの除外を作成せずに localhost と通信する必要がある場合)</li><li>集中型のファイル I/O</li></ul>     |  <ul><li>高 DPI サポート <sup>2</sup></li><li>タッチ入力 <sup>2</sup></li></ul>  |  <ul><li>高 DPI サポート <sup>2</sup></li><li>タッチ入力 <sup>2</sup></li><li>カスタマイズ可能な UI</li><li>豊富なグラフィックスとユーザー エクスペリエンス (タッチやアニメーションなど)</li><li>ビューとデータ モデルの豊富な抽象化</li></ul>    |   |

<sup>1</sup> Windows 10 の今後のリリースでこのシナリオに対処する機能が公開されています。

<sup>2</sup> プラットフォームには、このシナリオに対する最上級の API のサポートがありませんが、開発者はこのシナリオを回避策によりサポートできます。

## <a name="win32"></a>Win32

C++ で Win32 API を使用すると、WinRT や .NET などのマネージド ランタイム環境以上のターゲット プラットフォームの制御の強化をアンマネージド コードを使用して行うことで、最高レベルのパフォーマンスと効率を実現できます。 ただし、アプリケーションの実行に対してこのようなレベルの制御を行うには、正しく行うための十分な慎重さと注意が必要です。また、実行時のパフォーマンスと開発の生産性が引き換えになります。

ここでは、高パフォーマンスのアプリケーションの構築を可能にする、Win32 API と C++ が提供するものについて説明します。

-   リソース割り当て、オブジェクトの有効期間、データ レイアウト、アラインメント、バイト パッキングなどの厳密な制御を含むハードウェア レベルの最適化。
-   組み込み関数による SSE や AVX などのパフォーマンス指向の命令セットへのアクセス。
-   テンプレートを使用した、効率的でタイプセーフな汎用プログラミング。
-   効率的で安全なコンテナーとアルゴリズム。
-   DirectX、特に Direct3D および DirectCompute (UWP は DirectX 相互運用機能も提供)。
-   C++ AMP。

詳細については、「[Get started with desktop Windows apps that use the Win32 API](/windows/desktop/desktop-programming)」(Win32 API を使用するデスクトップ Windows アプリの概要) および[デスクトップ アプリ テクノロジ](/windows/desktop/desktop-app-technologies)に関する記事を参照してください。

### <a name="win32-and-c-for-traditional-desktop-applications"></a>従来のデスクトップ アプリケーション向けの Win32 と C++

C++ でデスクトップ アプリケーションを作成するときは、UI に Win32 または MFC を選択するか、Windows 以外のプラットフォームもサポートするサードパーティ製のアプリケーション フレームワークのホストを選択できます。

-   **Win32:** これは、Windows プラットフォームのハンドルベースの C 言語 API です。ウィンドウ、描画、UI コントロールなどの UI 機能を含みますがこれらに限定されません。 これは、ハンドルに基づく低レベルの C 言語 API なので、UI を大量に使用する最新のアプリの作成で選ばれることはあまりありません。 ただし、Windows プラットフォームとのやりとりに必要な基本的な API が用意されており、単純な UI 要件を持つアプリや、ゲームなどの Windows UI をできるだけ除外したいアプリに適しています。
-   **MFC (Microsoft Foundation Class ライブラリ):** これは、1992 年以降、Windows 開発者が使用してきた、歴史あるアプリケーション フレームワークおよび UI ライブラリです。 これは、ハンドルベースの C 言語 Win32 API に対する C++ の thin ラッパーであり、定義済みのウィンドウ、一般的なコントロール、およびその他の Windows オブジェクトの多くに対してオブジェクト指向インターフェイスを提供します。 .NET エコシステムの最新の UI フレームワークの多くは、MFC より使いやすくなっていますが、Windows デスクトップ用のアプリケーションを作成する多く C++ の開発者にとって、今でもネイティブ UI フレームワークとして選ばれています。
-   **サードパーティ アプリケーション フレームワーク:** C++ はさまざまなプラットフォームで実行でき、Windows または .NET ランタイムには関連していないため、サードパーティは、豊富なユーザー インターフェイスを備えたクロスプラットフォーム アプリケーションの開発を容易にするために、C++ 用の新しいアプリケーションおよび UI フレームワークを開発しました。 これらのフレームワークの中には、独自のルック アンド フィールを提供するものもありますが、wxWidgets や Qt などの他のプラットフォームでは、プラットフォームのネイティブ コントロール セットが使用されたり、エミュレートされたりします。 これらのライブラリを使用すると、Windows または OSX や Linux などの他のプラットフォームで実行されているアプリケーションのバージョン間で、ほぼすべてのアプリケーションのソースコードを共有することができます。

## <a name="other-app-platforms"></a>その他のアプリ プラットフォーム

### <a name="progressive-web-apps-pwas"></a>プログレッシブ Web アプリ (PWA)

PWA により、開発者は Web サイト コードをパッケージ化し、Windows 10 の PC 上のアプリケーションのようにインストールして実行できるようになります。 詳細については、[プログレッシブ Web アプリ](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/get-started)に関する記事を参照してください。

### <a name="xamarin"></a>Xamarin

Xamarin は、Windows 10 用でありながら iOS および Android でも実行できるクロスプラットフォーム アプリケーションの作成に使用します。 詳細については、[Xamarin](https://docs.microsoft.com/xamarin/xamarin-forms/get-started/index)に関する記事を参照してください。
