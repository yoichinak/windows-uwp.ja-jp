---
title: Windows 10 ビルド 19041 の新着情報
description: Windows 10 ビルド 18362 と新しい開発者ツールでは、Windows 10 によって強化されたツール、機能、エクスペリエンスを利用できます。
keywords: 新着情報, 新機能, Windows, Windows 10, 更新, 更新プログラム, 機能, 新規, 最新, 開発者, 19041, 可能性
ms.date: 05/12/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 63a6138841a19629523b452eab2f3e7b5d125c37
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174426"
---
# <a name="whats-new-for-developers-in-windows-10-build-19041"></a>Windows 10 ビルド 19041 の開発者向け新着情報

Windows 10 ビルド 19041 (SDK バージョン 2004 とも呼ばれる) は、Visual Studio 2019 および関連するツールや機能と組み合わせて使用することにより、優れた Windows アプリを作成するのに必要なものすべてが利用可能になります。 Windows 10 の[ツールと SDK をインストール](https://developer.microsoft.com/windows/downloads#_blank)すると、[新しいユニバーサル Windows アプリを作成](../get-started/create-uwp-apps.md)したり、[Windows の既存のアプリ コード](../porting/index.md)がどのように使えるかを試したりすることができます。

ここには、Windows 開発者にとって重要なこのリリースの新機能、強化された機能、ガイダンスを集めました。 Windows SDK に追加されたすべての新しい名前空間の一覧については、[Windows 10 ビルド 19041 API の変更点](windows-10-build-19041-api-diff.md)に関するページをご覧ください。 Windows 10 での注目すべき機能について詳しくは、「[Windows 10 の優れた機能](https://developer.microsoft.com/windows/windows-10-for-developers)」をご覧ください。

## <a name="windows-10-apps"></a>Windows 10 アプリ

機能 | 説明
:------ | :------
Bluetooth オーディオ再生 | 「[Bluetooth に接続されたリモート デバイスからオーディオ再生を有効にする](../audio-video-camera/enable-remote-audio-playback.md)」では、[AudioPlaybackConnection](/uwp/api/windows.media.audio.audioplaybackconnection) を使用して、ローカル コンピューター上のオーディオを Bluetooth に接続されたリモート デバイスで再生できるようにする方法を説明しています。これにより、PC が Bluetooth スピーカーのように動作する構成や、ユーザーが自分の電話でオーディオを聞けるようにするなどのシナリオを実現できます。
C# アプリの移植 | C# アプリケーションを C++/WinRT に移植するプロセスのドキュメントが用意されました。 [C# から C++/WinRT への Clipboard サンプルの移植](../cpp-and-winrt-apis/clipboard-to-winrt-from-csharp.md)に関するページは、コンテキスト形式のドキュメントで、実際の特定の移植エクスペリエンスに基づいています。 関連トピックの「[C# から C++/WinRT への移行](../cpp-and-winrt-apis/move-to-winrt-from-csharp.md)」は、百科事典的なドキュメントで、移植に関する技術的な詳細と手順について説明しています。 
C++/WinRT | Visual C++ コンパイラ チームとの連携により、C++/WinRT の更新プログラムでビルド時と実行時のパフォーマンス向上が実現しました。詳細については、[最新の機能強化および追加のロールアップ](../cpp-and-winrt-apis/news.md#rollup-of-recent-improvementsadditions-as-of-march-2020)に関するページをご覧ください。 </br> C++/WinRT については、[C++/CX からの移植](../cpp-and-winrt-apis/move-to-winrt-from-cx.md#boxing-and-unboxing)に関するページ、[C# からの移植](../cpp-and-winrt-apis/move-to-winrt-from-csharp.md#boxing-and-unboxing)に関するページ、「[単純な C++/WinRT Windows UI ライブラリの例](../cpp-and-winrt-apis/simple-winui-example.md)」、[コンカレンシー](../cpp-and-winrt-apis/concurrency.md)に関するページ、[get_unknown()](/uwp/cpp-ref-for-winrt/get-unknown)に関するページ、「[C++/WinRT による XAML カスタム (テンプレート化) コントロール](../cpp-and-winrt-apis/xaml-cust-ctrl.md)」に、詳細な情報が追加されています。
DirectX | Creators Update から Windows 10 バージョン 1903 までの過去のいくつかの Windows リリースに関して、DirectX 関連の「新機能」トピックの一部を最新のものに更新しました。 「[DirectWrite の新機能](/windows/win32/directwrite/what-s-new-in-directwrite-for-windows-8-consumer-preview)」、「[DXGI 1.6 の改良点](/windows/win32/direct3ddxgi/dxgi-1-6-improvements)」、「[Direct3D 12 の新機能](/windows/win32/direct3d12/new-releases)」。
DirectXMath | 2 つのマトリックス構造とそのメンバー関数および free 関数について説明した、21 の新しい DirectXMath トピックを公開しました。 「[XMFLOAT3X4 構造](/windows/win32/api/directxmath/ns-directxmath-xmfloat3x4)」はその一例です。
Direct3D | 「[ハイ ダイナミック レンジ ディスプレイと高度な色で DirectX を使用する](/windows/win32/direct3darticles/high-dynamic-range)」では、Windows ハイ ダイナミック レンジ アプリのベスト プラクティスをまとめて説明しています。 </br> 新しい [ID3D11On12Device2](/windows/win32/api/d3d11on12/nn-d3d11on12-id3d11on12device2) インターフェイスとそのメソッドを使用することにより、Direct3D 11 API を使用して作成されたリソースを取得して、Direct3D 12 で使用できるようになりました。
Direct3D 12 | [Direct3D 12 Core 1.0 機能レベル](/windows/win32/direct3d12/core-feature-levels)が追加され、"*コンピューティング専用*" デバイスで使用できるようになりました。 </br> [ID3D12Debug3 インターフェイス](/windows/win32/api/d3d12sdklayers/nn-d3d12sdklayers-id3d12debug3)に関する新しいトピックが追加されました。
Direct ML | WinML の構築基盤となる低レベルのハードウェア アクセラレータ対応 API である DirectML に、18 個の演算子が追加されました。 [DML_ACTIVATION_SHRINK_OPERATOR_DESC 構造](/windows/win32/api/directml/ns-directml-dml_activation_shrink_operator_desc)はその一例です。
エラー報告 | RoFailFastWithErrorContextInternal2 関数が Win32 に追加されました。これにより、発生した例外に追加のエラー コンテキストが含まれる場合があります。
Machine Learning | Windows Machine Learning で、[ONNX バージョン 1.4 および opset 9 がサポート](/windows/ai/windows-ml/release-notes)されるようになりました。 </br>  [CloseModelOnSessionCreation](/uwp/api/windows.ai.machinelearning.learningmodelsessionoptions.closemodelonsessioncreation?view=winrt-19041) API を使用すると、不要になった学習モデルを自動的に閉じることにより、メモリを節約できます。
Wi-Fi | [WlanDeviceServiceCommand function](/windows/win32/api/wlanapi/nf-wlanapi-wlandeviceservicecommand) など、新しいネイティブ WiFi 関数と構造がいくつか追加されました。
Wi-Fi ホットスポット 2 | 「[Web サイトを使用した Wi-Fi プロファイルのプロビジョニング](/windows/win32/nativewifi/prov-wifi-profile-via-website)」では、Wi-Fi ホットスポット 2 の新機能について説明しています。
Windows Holographic 相互運用 | 17 個の Win32 API に加えて、[`windows.graphics.holographic.interop.h`](/windows/win32/api/windows.graphics.holographic.interop) ヘッダーが追加されました。 この API は、Win32 と Windows ランタイムの間で相互運用を行うためのものです。 API は Windows 10 ビルド 18362 で追加されていましたが、ヘッダーはビルド 19041 で新たに追加されました。
Windows ソケット | Windows ソケット 2 SPI コンテンツの機能が強化されました。 [LPWSPEVENTSELECT コールバック関数](/windows/win32/api/ws2spi/nc-ws2spi-lpwspeventselect)に関するトピックは、機能の強化と拡張がなされた多くのトピックの一例です。
XAML Islands - 基本 | XAML Islands を使用すると、UWP XAMl コントロールをデスクトップ Windows アプリでホストできます。 [WPF アプリでの標準 UWP コントロールのホスト](/windows/apps/desktop/modernize/host-standard-control-with-xaml-islands)に関するページおよび「[C++ Win32 アプリで標準 UWP コントロールをホストする](/windows/apps/desktop/modernize/host-standard-control-with-xaml-islands-cpp)」で方法をご確認ください。
XAML Islands - カスタム コントロール | [Microsoft.Toolkit.Win32.UI.XamlApplication](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication) および [Microsoft.Toolkit.Win32.UI.SDK](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.SDK) NuGet パッケージを使用すると、カスタム UWP XAML コントロールを .NET アプリや C++ Win32 アプリでホストしやすくなります。 </br> 詳しい手順については、[WPF アプリでのカスタム UWP コントロールのホスト](/windows/apps/desktop/modernize/host-custom-control-with-xaml-islands)に関するページおよび「[C++ Win32 アプリでカスタム UWP コントロールをホストする](/windows/apps/desktop/modernize/host-custom-control-with-xaml-islands-cpp)」を参照してください。 </br> また、さらに複雑な C++ Win32 シナリオでのガイダンスについては、[XAML Islands の高度なシナリオ](/windows/apps/desktop/modernize/advanced-scenarios-xaml-islands-cpp)に関するページを参照してください。

## <a name="build-with-windows"></a>Windows でビルドする

機能 | 説明
:------ | :------
Windows の開発環境 | 「[Windows の開発環境](/windows/dev-environment/)」と題するドキュメントで、Windows を使用してさまざまなプラットフォームで開発を進め、開発に関する目標を達成するためのリソースが提供されています。
Windows での Python | [Windows での Python](/windows/python/)に関するセクションでは、Python 言語を初めて使用する開発者向けの情報や、Windows で利用できる他のツールを使って Python 環境を最適化することに関心のある開発者向けの情報が提供されています。 [Web 開発](/windows/python/web-frameworks)や[データベースの対話式操作](/windows/python/databases)のために Python 環境を設定する方法をご確認ください。
Windows での NodeJS | [Node.js 開発環境での推奨されている設定](/windows/nodejs/setup-on-wsl2)に関するページでは、Linux サーバーにデプロイする上級開発者向けの詳細なガイドラインが提供されています。 また、[一般的な Node.js Web フレームワーク](/windows/nodejs/web-frameworks)、[データベースの対話式操作](/windows/nodejs/databases)、[Docker コンテナー](/windows/nodejs/containers)のセットアップ手順も説明されています。
Mac-to-Windows | [開発環境を変更するためのガイド](/windows/dev-environment/mac-to-windows)は、開発プラットフォームを Mac から Windows に移行するユーザー向けに準備されたもので、ショートカットや開発ユーティリティを比較できる対応表が用意されています。
Windows ターミナル | コマンド プロンプト、PowerShell、Linux 用 Windows サブシステム (WSL) などのコマンド ライン ツールとシェルのユーザー向けの[最新のターミナル アプリケーション](https://www.microsoft.com/p/windows-terminal/9n0dx20hk701?activetab=pivot:overviewtab)です。 主な機能には、複数のタブ、ペイン、Unicode および UTF-8 文字のサポート、GPU で高速化されたテキスト レンダリング エンジン、独自のテーマを作成したり、テキスト、色、背景、およびショートカット キーのバインドをカスタマイズしたりする機能があります。
WSL 2 | [新しいバージョンの Linux 用 Windows サブシステム (WSL)](/windows/wsl/wsl2-about) が利用できるようになりました。 WSL 2 の機能では、Windows 上で実際の Linux カーネルを実行するためのアーキテクチャが再構成されており、ファイル システムのパフォーマンス向上と、システム コールの完全な互換性の追加が実現されています。 この新しいアーキテクチャによって、Linux バイナリと Windows やお使いのコンピューターのハードウェアとの対話方法は変わりますが、ユーザー エクスペリエンスについては以前のバージョンの WSL の場合と同じになっています。 個々の Linux ディストリビューションは、WSL1 または WSL2 ディストリビューションとして実行することも、並列実行することもでき、いつでも変更できます。 </br> 使用を開始するには、[WSL 2 をインストール](/windows/wsl/wsl2-install)します。 </br> 詳細については、[WSL 1 と WSL 2 の間のユーザー エクスペリエンスの変更](/windows/wsl/wsl2-ux-changes)に関するページを参照してください。 </br> [WSL 2 に関するよく寄せられる質問](/windows/wsl/wsl2-faq)に関するページもご確認ください。

## <a name="msix-packaging-and-deployment"></a>MSIX、パッケージ作成、デプロイ

機能 | 説明
:------ | :------
MSIX | Windows 10 SDK の前回のリリース以降、[MSIX パッケージ形式](/windows/msix/overview)が大幅に更新されました。 
サービスを含むパッケージ作成 | MSIX および MSIX パッケージ作成ツールで、[サービスを含むアプリ パッケージがサポート](/windows/msix/packaging-tool/convert-an-installer-with-services)されるようになりました。
MSIX パッケージ内でのスクリプト | [パッケージ サポート フレームワーク (PSF) を使用して MSIX アプリ パッケージ内でスクリプトを実行する](/windows/msix/psf/run-scripts-with-package-support-framework)ことができます。このようにすると、IT 技術者は、MSIX を使用してパッケージ化した後に、アプリケーションをユーザーの環境に合わせて動的にカスタマイズできます。
パッケージの整合性の適用 | パッケージ マニフェストで [uap10:PackageIntegrity 要素](/uwp/schemas/appxpackage/uapmanifestschema/element-uap10-packageintegrity)を使用することにより、MSIX パッケージのコンテンツにパッケージの整合性を適用できるようになりました。 また、MSIX パッケージ作成ツールを使用して MSIX パッケージを作成するときにも、パッケージの整合性を適用できます。
スパース パッケージ | アプリで "*スパース パッケージ*" をビルドして登録することで、[MSIX パッケージにパッケージ化されていないデスクトップ アプリにパッケージ ID を付与](/windows/apps/desktop/modernize/grant-identity-to-nonpackaged-apps)できます。 この機能により、デプロイのために MSIX パッケージをまだ導入できないデスクトップ アプリで、パッケージ ID を必要とする Windows 10 拡張機能を使用できるようになります。
ホステッド アプリ | [ホステッド アプリを作成する](../launch-resume/hosted-apps.md)ことができるようになりました。 ホステッド アプリは、親ホスト アプリと同じ実行可能ファイルと定義を共有しますが、両者の外観と動作はシステム上では別のアプリのようになります。 ホステッド アプリは、コンポーネント (実行可能ファイルやスクリプト ファイルなど) がスタンドアロンの Windows 10 アプリのように動作する必要があるものの、そのコンポーネントを実行するためにホスト プロセスが必要な場合に役立ちます。 ホステッド アプリには、独自のスタート タイルや ID を指定できるほか、バックグラウンド タスク、通知、タイル、共有ターゲットなどの Windows 10 の機能と緊密に統合することもできます。

## <a name="windows-ui-library-winui"></a>Windows UI ライブラリ (WinUI)

機能 | 説明
:------ | :------
WinUI 2.4 | [WinUI 2.4](/uwp/toolkits/winui/release-notes/winui-2.4) は、Windows UI ライブラリの最新のパブリック リリースです。 どのバージョンの WinUI も、Windows アプリ用のさまざまな公式 UI コントロールを提供しており、Windows SDK とは独立した NuGet パッケージとして提供されているので、以前のバージョンの Windows 10 で動作します。 [こちらの手順に従って](/uwp/toolkits/winui) WinUI をインストールします。
RadialGradientBrush | WinUI 2.4 で新しく利用できるようになった [RadialGradientBrush](../design/style/brushes.md#radial-gradient-brushes) は、Center、RadiusX、RadiusY プロパティによって定義される楕円内に描画されます。 グラデーションの色は楕円の中心から始まり、半径の位置で終了します。
ProgressRing | WinUI 2.4 で新しく利用できるようになった [ProgressRing コントロール](../design/controls-and-patterns/progress-controls.md) はモーダル操作向けに使われ、ProgressRing が消えるまでユーザーはブロックされます。 ある操作で、その操作が完了するまで、アプリとのほとんどのやり取りを中断する必要がある場合は、このコントロールを使用します。
TabView | [TabView コントロール](../design/controls-and-patterns/tab-view.md)の更新プログラムにより、タブの表示方法をより詳細に制御できるようになります。 選択されていないタブの幅を設定し、アイコンのみを表示して画面領域を節約することや、選択されていないタブの [閉じる] ボタンを、ユーザーがタブにカーソルを合わせるまで非表示にすることができます。
TextBox コントロール | ダーク テーマが有効になっている場合、テキストの挿入時に TextBox ファミリ コントロールの背景色が既定でダークのままになります。 影響を受けるコントロールは、[TextBox](/uwp/api/windows.ui.xaml.controls.textbox)、[RichEditBox](/uwp/api/windows.ui.xaml.controls.richtextblock)、[PasswordBox](/uwp/api/windows.ui.xaml.controls.passwordbox)、[Editable ComboBox](/uwp/api/windows.ui.xaml.controls.combobox)、[AutoSuggestBox](/uwp/api/windows.ui.xaml.controls.autosuggestbox) です。
NavigationView | [NavigationView](/uwp/api/microsoft.ui.xaml.controls.navigationview) コントロールで、階層型ナビゲーションがサポートされるようになりました。このコントロールには、Left、Top、LeftCompact の表示モードが含まれています。 階層構造の NavigationView は、ページのカテゴリの表示、関連する子ページを含むページの識別、またはハブ スタイルのページが他の多くのページにリンクしているアプリ内での使用に役立ちます。
Windows UI ギャラリー | 各 WinUI 機能の例については、XAML コントロール ギャラリーでご確認いただけます。 [Microsoft Store](https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt) でダウンロードするか、[Github でソース コードを参照](https://github.com/Microsoft/Xaml-Controls-Gallery)できます。
以前のバージョン | Windows 10 SDK の前回のメジャー リリース以降、[WinUI 2.3](/uwp/toolkits/winui/release-notes/winui-2.3) および [WinUI 2.2](/uwp/toolkits/winui/release-notes/winui-2.2) もリリースされ、Windows 開発者向けの新しい UI 機能がさらに提供されました。

## <a name="samples"></a>サンプル

次のサンプル アプリは、Windows 10 ビルド 19041 を対象として更新されました。

* [リモート セッション (クイズ ゲーム)](https://github.com/microsoft/Windows-appsample-remote-system-sessions)
* [顧客注文データベース](https://github.com/Microsoft/Windows-appsample-customers-orders-database)
* [RSS リーダー](https://github.com/Microsoft/Windows-appsample-rssreader)
* [Marble Maze](https://github.com/Microsoft/Windows-appsample-marble-maze)
* [フォト エディター](https://github.com/Microsoft/Windows-appsample-photo-editor)
* [ランチ スケジューラ](https://github.com/Microsoft/Windows-appsample-lunch-scheduler)
* [塗り絵帳](https://github.com/Microsoft/Windows-appsample-coloringbook)
* [Hue ライト コントローラー](https://github.com/Microsoft/Windows-appsample-huelightcontroller)
* [写真ラボ](https://github.com/Microsoft/Windows-appsample-photo-lab)
* [Family Notes](https://github.com/Microsoft/Windows-appsample-familynotes)

## <a name="videos"></a>ビデオ

### <a name="windows-terminal-the-secret-to-command-line-happiness"></a>Windows ターミナル: コマンド ライン活用の秘訣

ワークフローに合わせて Windows ターミナルをカスタマイズする方法について説明し、機能を実際にデモンストレーションします。 [ビデオをご覧ください](https://www.youtube.com/watch?v=2dsnwlnNBzs)。詳細については、[ドキュメントでご確認](https://github.com/microsoft/terminal#terminal--console-overview)いただけます。

### <a name="wsl2-code-faster-on-the-windows-subsystem-for-linux"></a>WSL2:Linux 用 Windows サブシステムの高速コード

新しいバージョンの Linux 用 Windows サブシステムである WSL2 の詳細、およびパフォーマンス向上のためになされた変更について説明します。 [ビデオをご覧ください](https://www.youtube.com/watch?v=MrZolfGm8Zk)。詳細については、[ドキュメントでご確認](/windows/wsl/wsl2-about)いただけます。

### <a name="msix-package-desktop-apps-for-windows-10-replace-outdated-installers"></a>MSIX:Windows 10 用デスクトップ アプリをパッケージ化し、 古いインストーラーを置き換える

Windows アプリをインストールするためのパッケージ形式である MSIX について説明します。Visual Studio で既存のコードをパッケージ化する方法や、アプリをデプロイして配布する方法も説明します。 [ビデオをご覧ください](https://www.youtube.com/watch?v=yhOnClQrvBk)。詳細については、[ドキュメントでご確認](/windows/msix/)いただけます。