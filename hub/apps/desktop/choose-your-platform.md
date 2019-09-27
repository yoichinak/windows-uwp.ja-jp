---
Description: 新しいデスクトップアプリを作成する場合、最初に行う必要があるのは、Win32 と COM API または .NET のどちらを使用するかということです。
ms.assetid: 82705644-F1F0-40F3-99B1-7A97BFB32831
title: アプリ プラットフォームの選択
ms.topic: article
ms.date: 03/18/2019
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: ff32e20c42f613bd9ac3dba9eada2cced0baa64c
ms.sourcegitcommit: f34deba1d4460d85ed08fe9648999fe03ff6a3dd
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/26/2019
ms.locfileid: "71316990"
---
# <a name="choose-your-app-platform"></a>アプリ プラットフォームの選択

Windows Pc 用の新しいデスクトップアプリケーションを作成する場合は、まず、どのアプリケーションプラットフォームを使用するかを決定します。 Windows には、次の4つの主要なアプリケーションプラットフォームが用意されており、それぞれに長所があります。

* [ユニバーサル Windows プラットフォーム (UWP)](#uwp)
* [WPF (.NET)](#wpf)
* [Windows フォーム (.NET)](#windows-forms)
* [32](#win32)

これらのアプリケーションプラットフォームでは、従来の Windows デスクトップで実行される Word、Excel、Photoshop などのデスクトップアプリを作成し、その環境固有の機能を最大限に活用することができます。 ただし、これらのプラットフォームの一部はいくつかの特徴を共有し、特定の種類のアプリケーションに適しています。

* **UWP、WPF、および Windows フォーム**。 これらのプラットフォームは、特に開発者の生産性、洗練されたカスタマイズ可能な UI、アプリケーションのセキュリティの領域において、マネージランタイム環境 (UWP 用の Windows ランタイムと Windows フォームと WPF 用の .NET) を提供します。 これらのフレームワークは、UI を迅速に作成するためのビジュアルデザイナーと UI マークアップをサポートしているため、基幹業務アプリケーションに特に適しています。

* **Win32 API**。 Win32 API (Windows API とも呼ばれます) は、Windows およびハードウェアへのC++直接アクセスを必要とするネイティブ C/windows アプリケーションの元のプラットフォームです。 .NET や WinRT などのマネージランタイム環境に依存することなく、ファーストクラスの開発環境を提供します。 これにより、最高レベルのパフォーマンスとシステムハードウェアへの直接アクセスを必要とするアプリケーションに適したプラットフォームが Win32 API されます。

この記事では、これらのプラットフォームについて詳しく説明し、アプリケーションに最適なプラットフォームを決定するのに役立ちます。 

> [!NOTE]
> どのアプリプラットフォームを選択しても、ユニバーサル Windows プラットフォーム (UWP) の多くの機能を使用して、Windows 10 でアプリの最新のエクスペリエンスを提供できます。 たとえば、デスクトップアプリが WPF、Windows フォーム、または Win32 API を使用して構築されている場合でも、MSIX パッケージ配置や UWP XAML コントロールなど、UWP で初めて導入された多くの機能を使用できます。 詳細については、「[デスクトップアプリの](modernize/index.md)最新化」を参照してください。

## <a name="uwp"></a>UWP

UWP は、Windows 10 アプリケーションとゲームの最先端のプラットフォームです。 これは、XAML マークアップを使用してコード (ビジネスロジック) から UX (プレゼンテーション) を分離する、高度にカスタマイズ可能なプラットフォームです。 UWP は、高度な UI、スタイルのカスタマイズ、およびグラフィックス集中型のシナリオを必要とするデスクトップアプリケーションに適しています。 UWP には、既定の UX エクスペリエンスのための[Fluent デザインシステム](/windows/uwp/design/fluent-design-system/)のサポートが組み込まれており、 [Windows ランタイム (WinRT) api](/windows/uwp/get-started/universal-application-platform-guide#how-the-universal-windows-platform-relates-to-windows-runtime-apis)にアクセスできます。 Fluent を導入することにより、一般的な入力方式 (インク、タッチ、ゲームパッド、キーボード、マウスなど) が自動的にサポートされます。

UWP を使用して Windows Pc 用のデスクトップアプリケーションを作成できるだけでなく、UWP は、Xbox、HoloLens、および Surface Hub アプリケーションで唯一サポートされているプラットフォームでもあります。 UWP は最新の最先端のアプリケーションプラットフォームです。

UWP の詳細については、「 [Windows 10 アプリの概要](/windows/uwp/get-started/)」を参照してください。

## <a name="wpf"></a>WPF

WPF は、完全な .NET Framework にアクセスするマネージ Windows アプリケーション用に確立されたプラットフォームであり、XAML マークアップを使用して、UX をコードから分離します。 このプラットフォームは、高度な UI、スタイルのカスタマイズ、およびグラフィックス集中型のシナリオを必要とするデスクトップアプリケーション向けに設計されています。 WPF 開発スキルは UWP 開発スキルに似ているため、WPF から UWP アプリへの移行は Windows フォームからの移行よりも簡単です。

WPF の詳細については、「[はじめに (wpf)](https://docs.microsoft.com/dotnet/framework/wpf/getting-started/)」を参照してください。

## <a name="windows-forms"></a>Windows フォーム

Windows フォームは、簡易 UI モデルを使用したマネージ Windows アプリケーションの元のプラットフォームであり、完全な .NET Framework にアクセスできます。 開発者は、プラットフォームを初めて使用する開発者にとっても、アプリケーションの構築をすぐに開始できるようになります。 これは、ビジュアルおよび非ビジュアルのドラッグアンドドロップコントロールの大きな組み込みコレクションを備えた、フォームベースの迅速なアプリケーション開発プラットフォームです。 Windows フォームでは XAML を使用しないため、後でアプリケーションを UWP に拡張するために、UI を完全に再記述する必要があります。

Windows フォームの詳細については、「 [Windows フォームの概要](https://docs.microsoft.com/dotnet/framework/winforms/getting-started-with-windows-forms)」を参照してください。

## <a name="platform-comparison-uwp-wpf-and-windows-forms"></a>プラットフォームの比較:UWP、WPF、および Windows フォーム

次の表は、Windows フォーム、WPF、UWP のさまざまな特性を詳細に比較したものです。

| 機能またはシナリオ  |    UWP     |      WPF     |   Windows フォーム  |
|--------|--------|--------|--------|
| **サポートされるバージョン**      |  Windows 10   |  Windows 7 以降 |  Windows 7 以降  |
| **言語**      |   C\#、 C++/WinRT、 C++/cx、VB、JavaScript   |  C\#、 C++/cli (のC++マネージ拡張)、F\#、VB |  C\#、 C++/cli (のC++マネージ拡張)、F\#、VB   |
| **UI ランタイム** |    ネイティブ (C++WinRT およびC++/cx) とマネージ (.NET ネイティブ)  |  マネージド (.NET Framework および .NET Core 3) |   マネージド (.NET Framework および .NET Core 3)   |
| **オープンソース** | [はい (Windows UI ライブラリのみ)](https://github.com/Microsoft/microsoft-ui-xaml)  |  [はい (.NET Core のみ)](https://github.com/dotnet/wpf) | [はい (.NET Core のみ)](https://github.com/dotnet/winforms)  |
| **XAML をサポートする** |   はい   |  [はい]  |   いいえ   |
| **力**  |  <ul><li>UI の XAML マークアップ</li><li>豊富でカスタマイズ可能な UX</li><li>既存のコードベースは .NET Standard 準拠しています</li><li>高 DPI サポート</li><li>Windows デバイス間での複数の入力の種類のサポート (タッチ、ペン、ゲームパッド、マウス、キーボードなど)</li><li>Xbox、HoloLens、IoT、または Surface Hub のサポート</li><li>ネイティブのサポートC++</li><li>バッテリ寿命の最適化</li><li>新しいアクセシビリティサポート (スクリーンリーダーなど)</li><li>リッチテキストデータ機能 (組み込みのスペルチェックなど)</li><li>インクサポート</li><li>アプリケーションコンテナーによるセキュリティで保護された実行 (たとえば、信頼されていないコンテンツがサンドボックス化される)</li></ul>  |  <ul><li>UI の XAML マークアップ</li><li>豊富でカスタマイズ可能な UX</li><li>Microsoft とパートナーのコントロールの大規模なコレクション</li><li>高密度 UI</li><li>Windows 7 のサポート</li><li>入力検証のプラットフォームサポート</li></ul> | <ul><li>迅速なアプリケーション開発</li><li>UI を構築するための WYSIWYG エディター</li><li>Microsoft とパートナーのコントロールの大規模なコレクション</li><li>高密度 UI</li><li>Windows 7 のサポート</li><li>キーボードとマウスの入力</li></ul>          |
| **サポートが制限されているシナリオ** |  <ul><li>高密度 UI (高密度 UI の作成にはカスタムスタイルが必要)<sup>1</sup></li><li>複数のウィンドウのサポート<sup>1</sup></li><li>入力検証のプラットフォームサポート<sup>1</sup></li><li>Windows 7 はサポートされていません</li><li>一部の UWP Api では、Windows 10 の特定の最小バージョンが必要です</li><li>完全なプラットフォームのサポートとシェルの統合 (たとえば、現在、UWP はシステムトレイの統合またはすべてのデバイスへのフルアクセスをサポートしていません)</li><li>ディスク上のすべてのファイルへの直接アクセス</li><li>ADO.NET</li><li>Non-.NET 標準または Windows 以外のアプリ認定キットに準拠した Api を使用する既存のコードベースクラスライブラリ</li><li>ローカルネットワークループバックのサポート (つまり、アプリがターゲットデバイスにループバックの除外を作成せずに localhost と通信する必要がある場合)</li><li>集中型のファイル i/o</li></ul>     |  <ul><li>高 DPI サポート<sup>2</sup></li><li>タッチ入力<sup>2</sup></li></ul>  |  <ul><li>高 DPI サポート<sup>2</sup></li><li>タッチ入力<sup>2</sup></li><li>カスタマイズ可能な UI</li><li>豊富なグラフィックスとユーザーエクスペリエンス (タッチやアニメーションなど)</li><li>ビューとデータモデルの豊富な抽象化</li></ul>    |   |

<sup>1</sup> Windows 10 の今後のリリースでは、このシナリオに対応する機能が公開されています。

<sup>2</sup>プラットフォームでは、このシナリオに対するファーストクラスの API のサポートが不足していますが、開発者は回避策を使用してこのシナリオをサポートできます。

## <a name="win32"></a>Win32

でC++ Win32 API を使用すると、WinRT や .net などのマネージランタイム環境では実現できないアンマネージコードを使用してターゲットプラットフォームの制御を強化することで、最高レベルのパフォーマンスと効率性を実現できます。 ただし、アプリケーションの実行に対してこのようなレベルの制御を行うには、十分な注意と注意が必要です。また、実行時のパフォーマンスについては、開発の生産性を向上させる必要があります。

ここでは、高パフォーマンスのアプリケーションを構築C++するために Win32 API とについて説明します。

-   ハードウェアレベルの最適化。リソース割り当て、オブジェクトの有効期間、データレイアウト、アラインメント、バイトパッキングなどの厳密な制御を含みます。
-   組み込み関数による SSE や AVX などのパフォーマンス指向の命令セットへのアクセス。
-   テンプレートを使用した、効率的でタイプセーフな汎用プログラミング。
-   効率的で安全なコンテナーとアルゴリズム。
-   DirectX (特に Direct3D および DirectCompute) では、UWP は DirectX 相互運用機能も備えています。
-   C++UTILITY.

詳細については、「Win32 API と[デスクトップアプリテクノロジ](/windows/desktop/desktop-app-technologies)を[使用するデスクトップ Windows アプリの概要](/windows/desktop/desktop-programming)」を参照してください。

### <a name="win32-and-c-for-traditional-desktop-applications"></a>Win32 とC++従来のデスクトップアプリケーション

でC++デスクトップアプリケーションを作成するときは、UI に Win32 または MFC を選択するか、Windows 以外のプラットフォームもサポートするサードパーティ製のアプリケーションフレームワークのホストを選択できます。

-   **32**これは、Windows プラットフォームのハンドルベースの C 言語 API です。たとえば、ウィンドウ、描画、UI コントロールなどの UI 機能に限定されません。 これは、ハンドルに基づく低レベルの C 言語 API なので、UI を大量に消費する最新のアプリを作成するための頻度が低くなります。 ただし、Windows プラットフォームとの対話に必要な基本的な Api が用意されています。これは、単純な UI 要件を持つアプリや、Windows UI をできるだけ多くのゲームなどの目的で使用できるようにするだけのアプリに適した選択肢です。
-   **MFC (Microsoft Foundation Class ライブラリ):** これは、1992以降に Windows 開発者が提供した、venerable アプリケーションフレームワークおよび UI ライブラリです。 これは、ハンドルC++ベースの C 言語 Win32 API に対するシンラッパーであり、定義済みのウィンドウ、一般的なコントロール、およびその他の windows オブジェクトの多くに対してオブジェクト指向インターフェイスを提供します。 .NET エコシステムの最新の UI フレームワークの多くは、MFC を使いやすくなっていますが、多くC++の開発者にとって、Windows デスクトップ用のアプリケーションを作成するためのネイティブ ui フレームワークとしても適しています。
-   **サードパーティ製のアプリケーションフレームワーク:** はC++さまざまなプラットフォームで実行でき、Windows または .net ランタイムには関連していないため、サードパーティは、豊富なユーザーインターフェイスC++を備えたクロスプラットフォームアプリケーションの開発を容易にするために、用の新しいアプリケーションおよび UI フレームワークを開発しました。 これらのフレームワークの中には、独自のルック & 感覚が用意されていますが、wxWidgets ジェットや Qt などの他のプラットフォームでは、プラットフォームのネイティブ制御セットが使用されるか、エミュレートされます。 これらのライブラリを使用すると、Windows または OSX や Linux などの他のプラットフォームで実行されているアプリケーションのバージョン間で、ほぼすべてのアプリケーションのソースコードを共有することができます。

## <a name="other-app-platforms"></a>その他のアプリプラットフォーム

### <a name="progressive-web-apps-pwas"></a>プログレッシブ Web Apps (PWAs)

PWAs を使用すると、開発者は web サイトコードをパッケージ化して、Windows 10 Pc 上のアプリケーションのようにインストールして実行することができます。 詳細については、「[プログレッシブ Web Apps](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/get-started)」を参照してください。

### <a name="xamarin"></a>Xamarin

Xamarin を使用して、iOS および Android でも実行できる Windows 10 用のクロスプラットフォームアプリケーションをビルドします。 詳細については、「 [Xamarin](https://docs.microsoft.com/xamarin/xamarin-forms/get-started/index)」を参照してください。
