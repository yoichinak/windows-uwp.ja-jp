---
title: Windows 10 ビルド 18362 の新着情報
description: Windows 10 ビルド 18362 と新しい開発者ツールでは、ユニバーサル Windows プラットフォームによって強化されたツール、機能、エクスペリエンスを利用できます。
keywords: Windows 10, 18362, 1903
ms.date: 04/19/2019
ms.topic: article
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 3db6ce85e81f4478a9776cec6a4e38caee0a2c9c
ms.sourcegitcommit: a7d49538d7ad762d34d41579fdfad2fb4422d667
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/29/2021
ms.locfileid: "99061419"
---
# <a name="whats-new-in-windows-10-for-developers-build-18362"></a>Windows 10 ビルド 18362 の開発者向け新着情報

Windows 10 ビルド 18362 (SDK バージョン 1903 とも呼ばれる) を Visual Studio 2019 と組み合わせて使用すると、優れた Windows アプリを作成するためのツール、機能、およびエクスペリエンスが得られます。 Windows 10 の[ツールと SDK をインストール](https://developer.microsoft.com/windows/downloads#_blank)すると、[新しいユニバーサル Windows アプリを作成](../get-started/create-uwp-apps.md)したり、[Windows の既存のアプリ コード](../porting/index.md)がどのように使えるかを試したりすることができます。

ここには、Windows 開発者にとって重要なこのリリースの新機能、強化された機能、ガイダンスを集めました。 Windows SDK に追加されたすべての新しい名前空間の一覧については、「[Windows 10 ビルド 18362 API の変更点](windows-10-build-18362-api-diff.md)」をご覧ください。 Windows 10 での注目すべき機能について詳しくは、「[Windows 10 の優れた機能](https://developer.microsoft.com/windows/windows-10-for-developers)」をご覧ください。

## <a name="design--ui"></a>設計および UI

機能 | 説明
:------ | :------
AnimatedVisualPlayer | [AnimatedVisualPlayer](/uwp/api/microsoft.ui.xaml.controls.animatedvisualplayer) API では、ご利用のアプリ内でのアニメーション ビジュアルの再生がホストおよび制御されます。 この API は、[Lottie](/windows/communitytoolkit/animations/lottie) ビジュアル (ご利用のアプリケーション内で Adobe AfterEffects アニメーションをネイティブにレンダリングできます) などのコンテンツを制御および表示するために使用されます。
CompactDensity | ご利用のアプリ内で[コンパクト モード](../design/style/spacing.md)を有効にすると、密度が高く情報が豊富なコントロール グループが有効にされます。 大量のコンテンツのブラウズ、ページに表示されるコンテンツの最大化を支援したり、ユーザーがポインターの入力を使用する場合、ナビゲーションや操作を支援できます。
Items Repeater | [ItemsRepeater](../design/controls-and-patterns/items-repeater.md) コントロールでは、ユーザーにコレクションを表示するためのカスタム エクスペリエンスを作成することができます。 ItemsRepeater では、包括的なエンドユーザー エクスペリエンスまたは既定の UI は提供されません。 その代わり、この構成要素を使用することで、独自の一意のコレクション ベースのエクスペリエンスおよびカスタム コントロールを作成することができます。
教育のヒント | [教育のヒント](../design/controls-and-patterns/dialogs-and-flyouts/teaching-tip.md)は、コンテキスト情報を提供する半永続的で内容豊富なポップアップです。 ユーザーに対して新機能または重要な機能について通知したり、リマインドしたり、説明したりする場合に、このコントロールを使用できます。
UI コマンド処理 | [UWP アプリでのコマンド処理](../design/controls-and-patterns/commanding.md)では、使用しているデバイスおよび入力の種類に関係なく、[XamlUICommand](/uwp/api/windows.ui.xaml.input.standarduicommand) クラスおよび [StandardUICommand](/uwp/api/windows.ui.xaml.input.standarduicommand) クラスを (ICommand インターフェイスと共に) 使用してさまざまなコントロール型にわたりコマンドを共有および管理します。
Windows UI ライブラリ | Windows UI ライブラリの最新の公式バージョンである [WinUI 2.1](/uwp/toolkits/winui/release-notes/winui-2.1) には、ご利用の Windows アプリ向けにさまざまな新しい XAML コントロールが用意されています。 WinUI ライブラリの API は以前のバージョンの Windows 10 で実行できるため、最新 OS を使用していないユーザーのサポート用にバージョン チェックや条件付き XAML を含める必要はありません。
デスクトップ アプリでのビジュアル レイヤー | [デスクトップ アプリケーションで UWP ビジュアル レイヤー API を使用](/windows/apps/desktop/modernize/visual-layer-in-desktop-apps)できるようになりました。 これらの API は、グラフィックス、効果、およびアニメーション用の高パフォーマンスの保持モード API を提供し、Windows デバイス間で UI の基盤となります。
Z 深度とシャドウ | ご利用の UWP アプリで昇格を作成するには、[Z 深度とシャドウ](../design/layout/depth-shadow.md)を使用します。 これらの新機能を使用すると、ご利用のアプリの UI を見やすくし、ユーザーが集中すべき重要なこととは何かを適切に伝えることができます。

## <a name="develop-windows-apps"></a>Windows アプリの開発

機能 | 説明
:------ | :------
マルウェア対策スキャン インターフェイス (AMSI) | [マルウェアからの保護にマルウェア対策スキャン インターフェイス (AMSI) がどのように役立つのか](/windows/desktop/amsi/how-amsi-helps)を学んでから、[サンプル コード](/windows/desktop/amsi/dev-audience)を確認して、それをご利用のデスクトップ アプリに実装する方法を学んでください。
C++/WinRT 2.0 | C++/WinRT のバージョン 2.0 がリリースされています。 新しい変更および追加の詳細な要約については、「[what's new in C++/WinRT](../cpp-and-winrt-apis/news.md)」 (C++/WinRT の新機能) を参照してください。
プラットフォームの選択 | 新しいデスクトップ アプリケーションの作成に関心がありますか? UWP、WPF、Windows フォームの各プラットフォームの詳細な説明と比較、および Win32 API の詳細については、改訂された[プラットフォームの選択](/windows/desktop/choose-your-technology)に関するページをご覧ください。
メッセージ エージェント | [Windows.ApplicationModel.ConversationalAgent](/uwp/api/windows.applicationmodel.conversationalagent) 名前空間を使用すれば、Windows プラットフォームの Agent Activation Runtime (AAR) でサポートされている任意のデジタル アシスタントをご利用の Windows アプリに追加できます。
クラウド ファイル API | "*クラウド ファイル API* " を使用すると、[プレースホルダー ファイル](/windows/desktop/cfapi/build-a-cloud-file-sync-engine)をサポートするクラウド同期エンジンを構築できます。
Direct 3D 12 | [Direct3D 12 のレンダリング パス](/windows/desktop/direct3d12/direct3d-12-render-passes)では、レンダラーがタイルベースの遅延レンダリング (TBDR) などの手法に基づいている場合、レンダラーのパフォーマンスを向上させることができます。 この手法では、ご利用のレンダラーで GPU の効率が改善されるように支援します。そのために、ご利用のアプリケーションがリソース レンダリングの順序付けの要件とデータの依存関係をより適切に識別できるようにします。 これにより、オフチップ メモリとの間のメモリ トラフィックが減少します。
Direct Machine Learning (DirectML) | [DirectML](/windows/desktop/direct3d12/dml) は、機械学習向けの低レベルのハードウェア アクセラレータ対応 API です。 これは、DirectX 12 のスタイルの使い慣れた (ネイティブ C++、nano-COM) プログラミング インターフェイスとワークフローを備えています。 機械学習の推測ワークロードをゲーム、エンジン、ミドルウェア、バックエンド、およびその他のアプリケーションに統合できます。 DirectML は、すべての DirectX 12 と互換性のあるハードウェアによってサポートされています。
DirectX HLSL | [HLSL Shader Model 6.4](/windows/desktop/direct3dhlsl/hlsl-shader-model-6-4-features-for-direct3d-12) では、DirectML で使用するための新しい機械学習組み込みが提供されます。
ドライバーの開発 | Windows ドライバー開発者向けに、オーディオ、カメラ、ディスプレイ、ネットワーク、モバイル ブロードバンド、印刷、センサー、ストレージ、および WiFi の新しい機能が追加されました。 詳細については、「[ドライバー開発に関する最新情報](/windows-hardware/drivers/what-s-new-in-driver-development#whats-new-in-windows-10-version-1903-latest)」をご覧ください。
ファイル システムの操作 | この[ベスト プラクティス ガイド](../files/best-practices-for-writing-to-files.md)は、お客様が Windows.Storage.FileIO クラスおよび Windows.Storage.PathIO クラスを使用してファイル システム I/O 操作を実行するのに役立ちます。
ゲームパッドとリモコンの操作 | [ゲームパッドとリモコンの操作](../design/input/gamepad-and-remote-interactions.md)を使用すれば、使いやすくアクセスしやすい操作エクスペリエンスを構築できます。 これらの操作により、ご利用のアプリケーションを、2 ft (60 cm) 離れた場所からでも 10 ft (3 m) 離れた場所からでも同様に直感的で使いやすいものとすることができます。
日本の年号の変更 | 2019 年 5 月 1 日に実施された日本の年号の変更にご利用の Windows アプリケーションが確実に対応できるようにする方法を示すために、[こちらの手順](../design/globalizing/japanese-era-change.md)を用意しました。 このページは、日本語でも利用できます (記事の下部にある [言語] コントロールをクリックし、[日本語] を選択します)。
WPF、Windows フォーム、および WinUI のオープン ソース | WPF、Windows フォーム、WinUI UX フレームワークを、GitHub 上でのオープンソース共同作成に利用できるようになりました。 詳細とリンクについては、[Windows アプリの作成](https://blogs.windows.com/buildingapps/2018/12/04/announcing-open-source-of-wpf-windows-forms-and-winui-at-microsoft-connect-2018/#OKZjJs1VVTrMMtkL.97)に関するブログ記事を参照してください。
Xbox 用のプログレッシブ Web アプリ | [Xbox One 用のプログレッシブ Web アプリ](/microsoft-edge/progressive-web-apps/xbox-considerations)を使用すれば、既存のフレームワーク、CDN、およびサーバー バックエンドを引き続き使用しながら、Web アプリケーションを拡張してそれを Microsoft Store 経由で Xbox One アプリとして使用可能にすることができます。 ほとんどのパーツについては、Windows の場合と同じ方法で、ご自分の Xbox One 用 PWA をパッケージ化することができます。 このガイドでは、そのプロセスについて順を追って説明し、主な違いを強調表示します。
Project Rome | Project Rome SDK が Android および iOS で利用できるようになりました。 Graph 通知を次の各プラットフォームに統合する方法について説明します: [Android](/windows/project-rome/notifications/how-to-guide-for-android) および [iOS](/windows/project-rome/notifications/how-to-guide-for-ios)。
リモート カメラ | DeviceWatcher クラスを使用すれば、[リモート カメラに接続](../audio-video-camera/connect-to-remote-cameras.md)し、そのカメラからのフレームをご利用の Windows アプリに読み込むことができます。
デスクトップ アプリケーションの UWP コントロール (XAML Islands) | WPF、Windows フォーム、および C++ Win32 デスクトップ アプリケーションで UWP コントロールをホストするための Windows SDK の API は、開発者向けプレビューに表示されなくなりました。 詳細については、[デスクトップ アプリケーションでの UWP コントロール](/windows/apps/desktop/modernize/xaml-islands)に関するページを参照してください。
Visual Studio 2019 | すべての開発者、アプリ、またはプラットフォーム用の最新のツールとサービスを備えた Visual Studio 2019 がリリースされています。 最新情報および使い始める方法については、「[Visual Studio 2019 の新機能](/visualstudio/ide/whats-new-visual-studio-2019)」をご覧ください。
Win32 WebView | [よく寄せられる質問](/windows/communitytoolkit/controls/wpf-winforms/webview#frequently-asked-questions-faqs)では、デスクトップ アプリケーションで Microsoft Edge WebView を使用する際の一般的な質問に対する回答、サンプルおよびその他のリソースへのリンクが提供されています。
Windows コマンド ライン | [コンソールの新機能](https://devblogs.microsoft.com/commandline/new-experimental-console-features/)には、スクロール、カーソルの形状、およびカーソルの色に関する設定がある実験用の [ターミナル] タブが含まれています。 詳細については、[開発者向けの Windows コマンド ライン ツール](https://devblogs.microsoft.com/commandline/)に関するブログ記事を参照してください。
Windows コミュニティ ツールキット | Windows コミュニティ ツールキット v5.1 では、アニメーション、リモート デバイス、画像のトリミング、およびアクセシビリティに対して魅力的な更新プログラムが提供されています。 </br> • 新しい [Lottie-Windows ライブラリ](/windows/communitytoolkit/animations/lottie)では、Windows.UI.Composition API を利用することによって Windows 10 (1809) 上での高品質のアニメーションがサポートされ、さらに [Bodymovin](https://aescripts.com/bodymovin/) JSON ファイルの使用、またはご利用の Windows アプリで再生するための最適化されたコード生成クラスの使用が考慮されています。 アニメーションのテストを行い、ご利用の Windows アプリ用に最適化されたコードを生成するために、Microsoft Store からの新しい [Lottie Viewer アプリ](https://www.microsoft.com/p/lottie-viewer/9p7x9k692tmw)を試してみてください。 </br> • 新しい[リモート デバイス ピッカー](/windows/communitytoolkit/controls/remotedevicepicker)を使用すると、ユーザーはデバイス (近距離またはクラウドからアクセス可能) を選択したり、そのデバイス上でアプリを起動したり、リモート デバイス上のアプリ サービスと通信したりできます。 </br> • 新しい[ImageCropper コントロール](/windows/communitytoolkit/controls/imagecropper)では、プロフィール画像を選択するため、または写真編集ツールを使用するのトリミング機能が統合されています。 </br> • さらに、コントロールに対するアクセシビリティーの向上や WPF および WinForms 用の [Microsoft.Toolkit.Win32](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32) 6.0 プレビュー パッケージ更新プログラムの他にも、[リリース ノート](https://github.com/windows-toolkit/WindowsCommunityToolkit/releases/tag/v5.1.0)で確認できる機能があります。
Windows Machine Learning | Windows AI ドキュメントをデザインし直して次の 3 つの領域に分けました: Windows Machine Learning (WinML)、Windows Vision Skills、および Direct Machine Learning (DirectML)。 新しい[ランディング ページ](/windows/ai/)を確認してください </br> • [*MLGen* エクスペリエンス](/windows/ai/mlgen)は、Visual Studio で変更中です。 バージョン 1903 以降の Windows 10 では、*mlgen* が Windows 10 SDK に含まれなくなりました。 VS 2017 を使用している場合は、代わりに Visual Studio の拡張機能 [Windows Machine Learning Code Generator VS 2017](https://marketplace.visualstudio.com/items?itemName=WinML.mlgen) をダウンロードしてインストールする必要があります。 Visual Studio 2019 を使用している場合は、拡張機能 [Windows Machine Learning Code Generator](https://marketplace.visualstudio.com/items?itemName=WinML.mlgenv2) をインストールする必要があります。 </br> • また、新たにウェイト パッキング用のサポートを発表できることを誇りに思います。 開発者は、[WinMLTools コンバーター](/windows/ai/convert-model-winmltools)を介して利用可能になるウェイト パッキングと呼ばれる手法を使用して、ML モデルのディスク占有領域を削減できるようになりました。
WinRT 統合リファレンス | WinRT API の構造体の定義に関して特定の詳細なノートを提供するために、[WinRT 型システム](/uwp/winrt-cref/winrt-type-system)および [WinMD ファイル](/uwp/winrt-cref/winmd-files)の詳細な説明を追加しました。
Windows Subsystem for Linux (WSL) | [WSL に対する最近の更新プログラム](https://devblogs.microsoft.com/commandline/whats-new-for-wsl-in-windows-10-version-1903/)には、エクスプローラーを使用して Windows から Linux ファイルにアクセスする機能と、wsl.exe および wslconfig.exe 用のいくつかの新しいコマンドが含まれています。
Windows Vision Skills | [Windows Vision Skills](/windows/ai/windows-vision-skills) は API セットです。これを使用することで、顔認識などの "スキル" を作成してから、他のアプリで使用できる NuGet パッケージとしてそれらをパッケージ化することができ、そのために機械学習モデルを追加する必要はありません。

## <a name="publish--monetize-windows-apps"></a>Windows アプリを公開および収益化する

機能 | 説明
 :------ | :------
MSIX | [Windows 10 ビルド 1709 および 1803 での MSIX サポート](/windows/msix/msix-1709-and-1803-support)に関するページでは、Windows 10 バージョン 1809 より前のバージョンでサポートされている MSIX 機能について説明しています。
MSIX パッケージ化とデプロイ | MSIX パッケージでのパッケージのカスタマイズを容易にするために、[修正パッケージに関連する改良点](/windows/msix/modification-package-insider-preview-build-18312)をいくつか導入しました。 これらの改良点には、パッケージ マニフェスト内の新しい **rescap6:ModificationPackage** 要素、メイン パッケージ内のファイルを変更パッケージでオーバーライドする機能、およびファイル システム ベースのプラグインを MSIX 修正パッケージとしてパッケージ化する機能が含まれます。
MSIX パッケージ作成ツール | • [リモート コンピューター上で変換を実行するためのサポート](/windows/msix/packaging-tool/remote-conversion-setup)を追加しました。 また、ツールの新しい機能への早期のアクセスを可能にする [MSIX パッケージ作成ツールの Insider Program](/windows/msix/packaging-tool/insider-program) を導入しました。 </br> • [1709 以降の MSIX パッケージのサポート](/windows/msix/packaging-tool/support-on-1709-and-later)に関するページで、MSIX パッケージ作成ツールを使用して、Windows 10 バージョン 1709 および 1803 に専用のパッケージを構築する方法について説明しています。 </br> • 「[MSIX packaging environment on Hyper-V Quick Create](/windows/msix/packaging-tool/quick-create-vm)」 (Hyper-V クイック作成での MSIX パッケージ化) では、MSIX パッケージ プロジェクト用の仮想環境の作成方法を示しています。 </br> • 「[MSIX パッケージのバンドル](/windows/msix/packaging-tool/bundle-msix-packages)」では、MSIX パッケージ作成ツールを使用してパッケージ バンドルを作成するための手順を説明しています。 </br> • 「[Modification packages on Windows 10 version 1809](/windows/msix/modification-package-1809-update)」 (Windows 10 バージョン 1809 での修正パッケージ) には、MSIX パッケージ作成ツールおよび MakeApp.exe を使用して Windows 10 バージョン 1809 以降のバージョン用の修正パッケージを作成するための手順が含まれています。
MSIX SDK | [MSIX SDK を使用してクロスプラットフォーム用のパッケージを作成](/windows/msix/msix-sdk/sdk-guidance)して、パッケージの展開先とするターゲット プラットフォームを指定する方法を学んでください。

## <a name="microsoft-learn"></a>Microsoft Learn

Microsoft Learn は、新しい実践的な学習とトレーニングの機会を Microsoft 開発者に提供します。

* Windows アプリの開発方法の学習に関心がある場合、プラットフォーム、ツール、およびご自分の最初のいくつかのアプリの作成方法については、[新しい学習パス](/learn/paths/develop-windows10-apps/)をご覧ください。

* ご自分の Windows アプリに UI 機能を追加する方法を学びたいですか? [UI を作成](/learn/modules/create-ui-for-windows-10-apps/)する方法、[ナビゲーションとメディアを UI に追加](/learn/modules/enhance-ui-of-windows-10-app/)する方法、または[データ バインディングを実装](/learn/modules/implement-data-binding-in-windows-10-app/)する方法を学習してください。

* Web 開発に関心がある場合は、「[Visual Studio Code で Web アプリケーションを開発する](/learn/modules/develop-web-apps-with-vs-code/)」または[簡単な Web サイトの構築](/learn/modules/build-simple-website/)に関するページを参照してください。

* または、[Microsoft Learn 上にあるすべての Windows 開発者モジュール](/learn/browse/?products=windows&resource_type=module)を自由に参照してください。

## <a name="videos"></a>ビデオ

### <a name="progressive-web-apps"></a>プログレッシブ Web アプリ

プログレッシブ Web アプリは、さまざまなブラウザーおよび各種 Windows 10 デバイスでネイティブ アプリのように機能する Web サイトです。 [ビデオをご覧になって](https://youtu.be/ugAewC3308Y)詳細を学習してから、[ドキュメントを参照](https://developer.microsoft.com/windows/pwa)して使用を開始してください。

### <a name="vs-code-series"></a>VS Code シリーズ

VSCode とは何か、その使用方法、および作成方法については、[Visual Studio Code に関する新しいビデオ シリーズ](https://www.youtube.com/playlist?list=PLlrxD0HtieHjQX77y-0sWH9IZBTmv1tTx)をご覧ください。

### <a name="mixed-reality-services"></a>複合現実サービス

HoloLens 2 が最近発表されました。 最新の情報および開発に参加し開発を開始する方法については、[複合現実に関するビデオ シリーズ](https://www.youtube.com/watch?v=pdB7Ukf3u0I&list=PLlrxD0HtieHjh2Nt2BhcluIZeQg0uBZST)をご覧ください。

### <a name="one-dev-question"></a>One Dev Question

One Dev Question ビデオ シリーズでは、ベテランの Microsoft 開発者が Windows 開発、チーム カルチャー、および歴史に関する一連の質問に答えています。

* [Raymond Chen: Windows 開発および歴史 ](https://www.youtube.com/watch?v=teV0gjCacug&list=PLlrxD0HtieHge3_8Dm48C0Ns61I6bHThc)

* [Larry Osterman: Windows 開発および歴史](https://www.youtube.com/watch?v=_34CokLwodE&list=PLlrxD0HtieHhDTjMijDOd0BSJcpSFabfE)

* [Chris Heilmann: VS Code](https://www.youtube.com/watch?v=cYRn5ONWAqo&list=PLlrxD0HtieHjQX77y-0sWH9IZBTmv1tTx)

* [Aaron Gustafson: プログレッシブ Web アプリ](https://www.youtube.com/watch?v=ks3CYvPBO2k&ab_channel=MicrosoftDeveloper)