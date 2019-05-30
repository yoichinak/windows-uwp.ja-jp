---
Description: 新しいデスクトップ アプリを作成するときに、Win32 および COM API または .NET を使用するかどうかが最初に判断します。
ms.assetid: 82705644-F1F0-40F3-99B1-7A97BFB32831
title: アプリ プラットフォームの選択
ms.topic: article
ms.date: 03/18/2019
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: 996bbaf4dd05ce5b24e536459c6d7d009a53fa19
ms.sourcegitcommit: f167775291cbc566b72b0859ae6b426d848c5c89
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/28/2019
ms.locfileid: "66266404"
---
# <a name="choose-your-app-platform"></a>アプリ プラットフォームの選択

Windows Pc の新しいデスクトップ アプリケーションを作成する場合は、最初に決定することを使用するには、どのアプリケーション プラットフォームです。 Windows では、それぞれさまざまな長所を持つ 4 つのメインのアプリケーション プラットフォームを提供します。

* [ユニバーサル Windows プラットフォーム (UWP)](#uwp)
* [WPF (.NET)](#wpf)
* [Windows フォーム (.NET)](#windows-forms)
* [Win32](#win32)

すべてのこれらのアプリケーション プラットフォームでは、クラシック Windows デスクトップおよび take の利点を最大限その環境内の特定の機能で実行されている Word、Excel、および Photoshop などのデスクトップ アプリを作成できます。 ただし、これらのプラットフォームの一部いくつかの特性を共有し、特定の種類のアプリケーションはやりやすくなります。

* **UWP、WPF、および Windows フォーム**します。 これらのプラットフォームでは、特に開発者の生産性向上、高度でカスタマイズ可能な UI、およびアプリケーションのセキュリティの面で、多くの利点をマネージ ランタイム環境 (UWP、および .NET の Windows フォームと WPF の Windows ランタイム) を提供します。 これらのフレームワークは、UI を迅速に作成するためのビジュアル デザイナーと UI のマークアップをサポート、ためには、基幹業務アプリケーションに特に適しています。

* **Win32 API**します。 (Windows API とも呼ばれます)、Win32 API は、元のプラットフォームのネイティブ C/C++ Windows とハードウェアに直接アクセスを必要とする Windows アプリケーション。 .NET および WinRT のようなマネージ ランタイム環境に応じてせずファースト クラスの開発エクスペリエンスを提供します。 これにより、Win32 API は、最高レベルのパフォーマンスとシステムのハードウェアに直接アクセスする必要があるアプリケーションに最適なプラットフォームです。

この記事では、これらのプラットフォームについて詳しく説明し、アプリケーションの最適なものを決定するのに役立ちます。 

> [!NOTE]
> 選択したアプリ プラットフォームに関係なく、Windows 10 上のアプリで、最新のエクスペリエンスを提供するのにユニバーサル Windows プラットフォーム (UWP) の多くの機能を使用できます。 たとえば、デスクトップ アプリでは、WPF、Windows フォーム、または Win32 API を使用してビルドされたが、たとえ MSIX パッケージの配置と UWP XAML コントロールなど、UWP で初めて導入された多くの機能を使用できます。 詳細については、次を参照してください。 [、デスクトップ アプリを最新化](modernize/index.md)します。

## <a name="uwp"></a>UWP

UWP は、Windows 10 のアプリケーションやゲームの最先端のプラットフォームです。 コード (ビジネス ロジック) から UX (プレゼンテーション) を分離する XAML マークアップを使用する高度にカスタマイズ可能なプラットフォームになります。 UWP では、高度な UI、スタイルのカスタマイズ、およびグラフィックを多用するシナリオを必要とするデスクトップ アプリケーションに適しています。 UWP の組み込みサポートにも、 [Fluent Design System](/windows/uwp/design/fluent-design-system/) 、既定の UX が発生してへのアクセスを提供します、 [Windows ランタイム (WinRT) Api](/windows/uwp/get-started/universal-application-platform-guide#how-the-universal-windows-platform-relates-to-windows-runtime-apis)します。 UWP には、Fluent を採用することにより、インク、タッチ、ゲームパッド、キーボード、マウスなどの一般的な入力方法に自動的にサポートしています。

UWP を使用する Windows pc、デスクトップ アプリケーションを作成するだけでなく UWP、Xbox、HoloLens、Surface Hub などのアプリケーションのサポートされている唯一のプラットフォームでも。 UWP は、マイクロソフトの最新、最先端のアプリケーション プラットフォームです。

UWP の詳細については、次を参照してください。 [Windows 10 アプリの概要](/windows/uwp/get-started/)します。

## <a name="wpf"></a>WPF

WPF は .NET Framework にアクセス権を持つ管理対象の Windows アプリケーションのプラットフォームが確立され、UX をコードから分離する XAML マークアップを使用します。 このプラットフォームは、高度な UI、スタイルのカスタマイズ、およびグラフィックを多用するシナリオを必要とするデスクトップ アプリケーションに適しています。 WPF から UWP アプリへの移行は Windows フォームからの移行をより簡単には、WPF 開発スキルを UWP 開発のスキルに似ています。

WPF の詳細については、次を参照してください。[概要 (WPF)](https://docs.microsoft.com/dotnet/framework/wpf/getting-started/)します。

## <a name="windows-forms"></a>Windows フォーム

Windows フォームは、完全な .NET Framework に軽量の UI モデルへのアクセスと管理対象の Windows アプリケーションの元のプラットフォームです。 これは、開発者はすぐに開始する新しいプラットフォームに開発者の場合でも、アプリケーションの構築に優れています。 これは、ビジュアルと非ビジュアルのドラッグ アンド ドロップのコントロールの場合は、大規模な組み込みコレクションを使用して、フォーム ベースの迅速なアプリケーション開発プラットフォームです。 Windows フォームでは、全面的な書き直し、UI のでは、後で決定する uwp アプリケーションを拡張するため、XAML では使用しません。

Windows フォームの詳細については、次を参照してください。 [Windows フォームの概要](https://docs.microsoft.com/dotnet/framework/winforms/getting-started-with-windows-forms)します。

## <a name="platform-comparison-uwp-wpf-and-windows-forms"></a>プラットフォームの比較:UWP、WPF、および Windows フォーム

次の表は、詳細に Windows フォーム、WPF、および UWP のさまざまな特性を比較します。

| 機能またはシナリオ  |    UWP     |      WPF     |   Windows フォーム  |
|--------|--------|--------|--------|
| **サポートされているバージョン**      |  Windows 10   |  Windows 7 以降 |  Windows 7 以降  |
| **言語**      |   C\#、 C++/WinRT、 C++/CX、VB、JavaScript   |  C\#、 C++/CLI (のマネージ拡張C++)、F\#、VB |  C\#、 C++/CLI (のマネージ拡張C++)、F\#、VB   |
| **UI のランタイム** |    ネイティブ (C++/WinRT とC++/CX) および管理 (.NET ネイティブ)  |  マネージ (.NET Framework)<br/><br/>.NET Core 3 のサポートは近日公開予定  |   マネージ (.NET Framework)<br/><br/>.NET Core 3 のサポートは近日公開予定    |
| **オープン ソース** | [[はい] (Windows UI ライブラリのみ)](https://github.com/Microsoft/microsoft-ui-xaml)  |  [[はい] (.NET Core のみ)](https://github.com/dotnet/wpf) | [[はい] (.NET Core のみ)](https://github.com/dotnet/winforms)  |
| **XAML をサポートしています** |   〇   |  〇  |   X   |
| **長所**  |  <ul><li>UI の XAML マークアップ</li><li>機能が豊富でカスタマイズ可能なユーザー エクスペリエンス</li><li>既存のコード ベースは .NET Standard 準拠</li><li>高 DPI のサポート</li><li>Windows のデバイス (タッチ、ペン、ゲームパッド、マウス、キーボードを含む) の間で複数の入力型のサポート</li><li>Xbox、HoloLens、IoT、または Surface Hub のサポート</li><li>ネイティブのサポートC++</li><li>最適化されたバッテリの寿命</li><li>スクリーン リーダー) などの最新のユーザー補助のサポート</li><li>(組み込みのスペル チェック機能) などのリッチ テキスト データの機能</li><li>手描き入力のサポート</li><li>アプリケーション コンテナーを使用して実行をセキュリティで保護された (コンテンツがセキュリティで保護された信頼されていないなど)</li></ul>  |  <ul><li>UI の XAML マークアップ</li><li>機能が豊富でカスタマイズ可能なユーザー エクスペリエンス</li><li>Microsoft とパートナーからのコントロールの大規模なコレクション</li><li>高密度の UI</li><li>Windows 7 のサポート</li><li>プラットフォームは、入力の検証をサポートします。</li></ul> | <ul><li>迅速なアプリケーション開発</li><li>UI を構築するための WYSIWYG エディター</li><li>Microsoft とパートナーからのコントロールの大規模なコレクション</li><li>高密度の UI</li><li>Windows 7 のサポート</li><li>キーボードとマウス入力</li></ul>          |
| **サポートが限られているシナリオ** |  <ul><li>密度の高い UI (密度の高い UI の作成、カスタム スタイルが必要です)<sup>1</sup></li><li>複数のウィンドウのサポート<sup>1</sup></li><li>入力の検証のプラットフォーム サポート<sup>1</sup></li><li>Windows 7 はサポートされていません</li><li>一部の UWP Api が特定の最小バージョンの Windows 10 が必要です。</li><li>完全なプラットフォームのサポートとシェル統合 (たとえば、UWP 現在サポートしていませんシステム トレイの統合またはすべてのデバイスへのフル アクセス)</li><li>ディスク上のすべてのファイルに直接アクセスします。</li><li>ADO.NET</li><li>.NET Standard 以外を使用して、既存のコード ベースのクラス ライブラリまたは以外の Windows アプリ認定キットの準拠している Api</li><li>ローカル ネットワーク ループバックのサポート (アプリは、ターゲット デバイスでループバックの免除を作成せずに localhost と通信する必要があります) の場合は、</li><li>処理を要するファイル I/O</li></ul>     |  <ul><li>高 DPI のサポート<sup>2</sup></li><li>タッチ入力<sup>2</sup></li></ul>  |  <ul><li>高 DPI のサポート<sup>2</sup></li><li>タッチ入力<sup>2</sup></li><li>カスタマイズ可能な UI</li><li>(タッチやアニメーション) などのリッチ グラフィックスとユーザー エクスペリエンスします。</li><li>ビューとデータ モデルの高度な抽象化</li></ul>    |   |

<sup>1</sup> Windows 10 の将来のリリースでは、このシナリオに対処する機能が発表したパブリックにします。

<sup>2</sup>とその回避策は、このシナリオは、プラットフォームでは、このシナリオのファースト クラスの API のサポートがない、開発者はサポートできません。

## <a name="win32"></a>Win32

Win32 API を使用してC++アンマネージ コードとターゲット プラットフォームの WinRT、.NET などのマネージ ランタイム環境より詳細に制御を実行して、最高レベルのパフォーマンスと効率性を実現するためにできるようなります。 ただし、このようなアプリケーションの実行制御のレベルを行使する大きい注意し、すぐに注意を必要とし、実行時のパフォーマンスの開発の生産性と引き換え。

Win32 API のいくつかのハイライトをここでは、C++高パフォーマンス アプリケーションを構築するために提供します。

-   リソースの割り当て、オブジェクトの有効期間、データ レイアウト、配置、梱包、バイトを厳密に制御を含む、ハードウェア レベルの最適化など。
-   パフォーマンス指向命令へのアクセスは、組み込み関数を使用して、SSE および AVX のように設定します。
-   テンプレートを使用して汎用プログラミングを効率的なタイプ セーフです。
-   効率的かつ安全なコンテナーとアルゴリズム。
-   DirectX では、特定の Direct3D と DirectCompute (注 UWP は、DirectX の相互運用機能も用意されています)。
-   C++AMP します。

詳細については、次を参照してください。 [Win32 API を使用するデスクトップの Windows アプリの概要](/windows/desktop/desktop-programming)と[デスクトップ アプリ テクノロジ](/windows/desktop/desktop-app-technologies)します。

### <a name="win32-and-c-for-traditional-desktop-applications"></a>Win32 とC++の従来のデスクトップ アプリケーション

デスクトップ アプリケーションを記述するときC++、UI、または Windows 以外のプラットフォームをサポートするサード パーティ製のアプリケーション フレームワークのホストの Win32 または MFC を選択できます。

-   **Win32:** これは、コントロールを含むウィンドウ、描画、および UI などの UI 機能に限定されませんが、Windows プラットフォームのハンドルに基づく、C 言語 API です。 ハンドルに基づく低レベルで C 言語 API であるため、UI を大量に消費する最新のアプリを作成するための低頻度の選択肢となります。 ただし、Windows プラットフォームと対話するために必要な基本的な Api を提供しはアプリ UI のシンプルな要件またはゲーム、たとえば、可能な限り邪魔を維持する Windows UI をたいに適した選択肢です。
-   **MFC (Microsoft Foundation Class ライブラリ):** これは、古くからあるアプリケーション フレームワークと 1992 年から Windows 開発者が処理した UI ライブラリです。 シンC++ハンドルに基づく、C 言語の Win32 API のラッパーと、定義済みの windows、一般的なコントロール、およびその他の Windows のオブジェクトの多くのオブジェクト指向のインターフェイスを提供します。 多くの任意のネイティブ UI フレームワークでは引き続き、.NET エコシステムの多くの最新 UI フレームワークでは、利便性のために MFC を上回りがC++開発者向け Windows デスクトップ アプリケーションを作成します。
-   **サード パーティのアプリケーション フレームワーク:** C++さまざまなプラットフォームで実行できるし、関連付けられていない Windows または .NET ランタイムでは、サード パーティが開発して、新しいアプリケーションの UI フレームワークC++豊富なユーザー インターフェイスを持つクロス プラットフォーム アプリケーションの開発を容易にします。 これらのフレームワークの一部は提供 wxWidgets または Qt などの他のユーザーを使用して、またはプラットフォームのネイティブ コントロールのセットをエミュレート、独自のルック アンド フィールをします。 これらのライブラリを使用することはほぼすべての Windows または os X や Linux などの他のプラットフォームで実行されるアプリケーションのバージョン間で、アプリケーションのソース コードを共有します。

## <a name="other-app-platforms"></a>その他のアプリ プラットフォーム

### <a name="progressive-web-apps-pwas"></a>プログレッシブ Web アプリ (Pwa)

Pwa は、開発者パッケージがインストールされ、アプリケーションのように Windows 10 Pc 上で実行できるように企業の web サイトのコードを使用できます。 詳細については、次を参照してください。[プログレッシブ Web アプリ](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/get-started)します。

### <a name="xamarin"></a>Xamarin

IOS でも実行する Windows 10 および Android 用クロスプラット フォーム対応のアプリケーションを構築するのにには、Xamarin を使用します。 詳細については、次を参照してください。 [Xamarin](https://docs.microsoft.com/xamarin/xamarin-forms/get-started/index)します。
