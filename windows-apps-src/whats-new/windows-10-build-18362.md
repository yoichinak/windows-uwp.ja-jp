---
title: Windows 10 の開発者向け新着情報
description: Windows 10 が 18362 をビルドし、新しい開発者ツールは、ツール、機能、およびユニバーサル Windows プラットフォームを利用したエクスペリエンスを提供します。
keywords: 新機能については、何が起こって、新しい更新プログラム、更新プログラム、機能、新しい Windows 10 で、最新の開発者、18362、可能性があります
ms.date: 04/19/2019
ms.topic: article
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 935d7f787d0cc23965c0fd51747b7687adb80a3f
ms.sourcegitcommit: a4fe508e62827a10471e2359e81e82132dc2ac5a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/03/2019
ms.locfileid: "66468316"
---
# <a name="whats-new-in-windows-10-for-developers-build-18362"></a>開発者は、ビルド 18362 が追加された Windows 10 です。

Visual Studio 2019 での Windows 10 ビルド 18362 (とも呼ばれます SDK バージョンが 1903) の組み合わせでは、ツール、機能、および優れた Windows アプリを作成するエクスペリエンスを提供します。 Windows 10 の[ツールと SDK をインストール](https://go.microsoft.com/fwlink/?LinkId=821431)すると、[新しいユニバーサル Windows アプリを作成](../get-started/create-uwp-apps.md)したり、[Windows の既存のアプリ コード](../porting/index.md)がどのように使えるかを試したりすることができます。

ここには、Windows 開発者にとって重要なこのリリースの新機能、強化された機能、ガイダンスを集めました。 Windows SDK に追加された新しい名前空間の一覧については、次を参照してください。、 [Windows 10 ビルド API の変更点を 18362](windows-10-build-18362-api-diff.md)します。 Windows 10 での注目すべき機能について詳しくは、「[Windows 10 の優れた機能](https://go.microsoft.com/fwlink/?LinkId=823181)」をご覧ください。

## <a name="design--ui"></a>設計および UI

機能 | 説明
:------ | :------
AnimatedVisualPlayer | [AnimatedVisualPlayer](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.animatedvisualplayer) API をホストし、アプリでビジュアルをアニメーションの再生を制御します。 この API の使用を制御しなどのコンテンツを表示[Lottie](https://docs.microsoft.com/windows/communitytoolkit/animations/lottie)アプリケーションでネイティブに Adobe たびたびアニメーションをレンダリングするためのビジュアル。
CompactDensity | 有効にする[コンパクト モード](../design/style/spacing.md)アプリ コントロールの高密度、情報が豊富なグループを使用できます。 大量のページに表示されるコンテンツの最大化、コンテンツの参照を支援したり、ユーザーがポインターの入力を使用する場合、ナビゲーションや操作を支援できます。
項目を Repeater | [ItemsRepeater](../design/controls-and-patterns/items-repeater.md)コントロールが生き物、カスタム エクスペリエンスをユーザーにコレクションを表示することができます。 ItemsRepeater では、包括的なエンド ユーザー エクスペリエンス、または既定の UI は提供されません。 代わりに、独自の一意のコレクション ベースのエクスペリエンスとカスタム コントロールの作成に使用できる構成要素になります。
教育のヒント | A[教えるヒント](../design/controls-and-patterns/dialogs-and-flyouts/teaching-tip.md)は半永続的なと、豊富なコンテンツのポップアップで、コンテキスト情報を提供します。 通知、通知、および重要な新機能についてユーザーに知らせるには、このコントロールを使用できます。
UI コマンドを実行 | [コマンド実行の UWP アプリで](../design/controls-and-patterns/commanding.md)を使用して、 [XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand)と[StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand)を共有し、コマンドの間でさまざまな管理 (ICommand インターフェイス) と共にクラス使用されているデバイスと入力の種類に関係なく、型を制御します。
Windows UI ライブラリ | 最新の正式なバージョンの Windows UI ライブラリ – [WinUI 2.1](https://docs.microsoft.com/uwp/toolkits/winui/release-notes/winui-2.1) – Windows アプリの活気のある新しい XAML コントロールを提供します。 WinUI ライブラリの API は以前のバージョンの Windows 10 で実行できるため、最新 OS を使用していないユーザーのサポート用にバージョン チェックや条件付き XAML を含める必要はありません。
ビジュアル層でデスクトップ アプリ | できるようになりました[デスクトップ アプリケーションで UWP ビジュアル層の Api を使用して、](../composition/visual-layer-in-desktop-apps.md)します。 これらの Api は、グラフィックス、エフェクト、およびアニメーションの高パフォーマンス モードが再トレーニング API を提供し、UI の基盤を Windows デバイス間でします。
Z 深度とシャドウ | 使用[Z 深さとシャドウ](../design/layout/depth-shadow.md)UWP アプリで昇格を作成します。 これを使用すると、アプリの UI をスキャンしやすくの新機能と強化に専念するユーザーにとって重要な内容を伝達します。

## <a name="develop-windows-apps"></a>Windows アプリを開発

機能 | 説明
:------ | :------
マルウェア対策スキャン インターフェイス (AMSI) | について説明します[マルウェア対策スキャン インターフェイス (AMSI) を利用するマルウェアからの保護方法](https://docs.microsoft.com/windows/desktop/amsi/how-amsi-helps)、チェック アウトし、[サンプル コード](https://docs.microsoft.com/windows/desktop/amsi/dev-audience)にデスクトップ アプリでの実装方法について説明します。
C++/WinRT 2.0 | バージョン 2.0 のC++/WinRT がリリースされています。 チェック アウト[新C++/WinRT](../cpp-and-winrt-apis/news.md)のさまざまな種類のすべての新しい変更と追加します。
プラットフォームの選択 | 新しいデスクトップ アプリケーションの作成に興味はあるでしょうか。 チェック アウト、刷新[プラットフォームを選択](https://docs.microsoft.com/windows/desktop/choose-your-technology)詳細な説明と、UWP、WPF、および Windows フォームのプラットフォームと Win32 API の詳細についての比較ページ。
会話のエージェント | [Windows.ApplicationModel.ConversationalAgent](https://docs.microsoft.com/uwp/api/windows.applicationmodel.conversationalagent)名前空間では、Windows アプリには、Windows プラットフォームのエージェントのアクティブ化実行時 (AAR) でサポートされているデジタルの支援を追加することができます。
クラウドのファイルの API | *ファイル API をクラウド*できます[プレース ホルダー ファイルをサポートするクラウド同期エンジンを構築](https://docs.microsoft.com/windows/desktop/cfapi/build-a-cloud-file-sync-engine)します。
Direct 3D 12 | [Direct3d12 のレンダリング パス](/windows/desktop/direct3d12/direct3d-12-render-passes)パフォーマンスを向上できます、レンダラーの延期タイル ベースのレンダリング (TBDR) に基づく場合などのテクニックです。 GPU の効率を向上させるよりリソースの表示の要件とデータの依存関係の順序を識別するために、アプリケーションを有効にすると、このレンダラーをによりします。 これには、チップをメモリとの間のメモリ トラフィックが削減されます。
Direct Machine Learning (DirectML) | [DirectML](https://docs.microsoft.com/windows/desktop/direct3d12/dml)は machine learning の低レベル ハードウェア アクセラレータを使用した API です。 使い慣れたが (ネイティブC++、nano COM) プログラミング インターフェイスや DirectX 12 のスタイルのワークフロー。 Machine learning を統合することができます、ゲーム、エンジン、ミドルウェア、バックエンド、またはその他のアプリケーションにワークロードを推論します。 DirectML は、すべての DirectX 12 と互換性のあるハードウェアによってサポートされています。
DirectX HLSL | [HLSL シェーダー モデル 6.4](https://docs.microsoft.com/windows/desktop/direct3dhlsl/hlsl-shader-model-6-4-features-for-direct3d-12) DirectML で使用するための machine learning の新しい組み込み関数を提供します。
ドライバーの開発 | 新しい音声、カメラ、表示、ネットワーク、Windows ドライバー開発者向けのモバイル ブロード バンド、印刷、センサー、ストレージ、および wifi の機能が追加されました。 チェック アウト[ドライバーの開発における新](https://docs.microsoft.com/windows-hardware/drivers/what-s-new-in-driver-development#whats-new-in-windows-10-version-1903-latest)の詳細。
ファイル システム操作 | これは、[のベスト プラクティス ガイド](../files/best-practices-for-writing-to-files.md)最適使用、Windows.Storage.FileIO と Windows.Storage.PathIO クラスは、ファイル システム I/O 操作の実行を支援できます。
ゲームパッドとリモコンの操作 | 使用[ゲームパッドとリモート制御の相互作用](../design/input/gamepad-and-remote-interactions.md)操作のアクセスを使用して構築するため発生します。 これらのインタラクションをアプリケーションできます直感的なと 2 フィート離れた場所から使いやすい 10 フィート離れた場所からは。
日本語の時代 (年号) の変更 | 提供されています[手順](../design/globalizing/japanese-era-change.md)Windows アプリケーションは、日本語のことを確認する方法を説明する時代 (年号) は、2019 年 5 月 1 日に行われるように設定を変更します。 [このページは日本語で利用可能なも](https://docs.microsoft.com/ja-jp/windows/uwp/design/globalizing/japanese-era-change)します。
WPF、Windows フォーム、および WinUI のオープン ソース | GitHub のオープン ソース コントリビューションの WPF、Windows フォーム、および WinUI UX のフレームワークがあるようになりました。 詳細な情報とリンクについては、次を参照してください。、 [building Windows アプリのブログ](https://blogs.windows.com/buildingapps/2018/12/04/announcing-open-source-of-wpf-windows-forms-and-winui-at-microsoft-connect-2018/#OKZjJs1VVTrMMtkL.97)します。
プログレッシブ Web Apps for Xbox | [Xbox One の Web アプリをプログレッシブ](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/xbox-considerations)、web アプリケーションを拡張し、使用できるように Xbox One のアプリケーションとして Microsoft Store を使用して、既存のフレームワーク、CDN、およびサーバーのバックエンドを使用する継続しているときにすることができます。 ほとんどの場合、Windows の場合と同じ方法で Xbox One の PWA をパッケージすることができます。 このガイドは、プロセスについて説明し、主な違いを強調表示します。
Project Rome | プロジェクトのローマ SDK は Android と iOS のご利用いただけます。 グラフの通知を各プラットフォームに統合する方法について説明します。[Android](https://docs.microsoft.com/windows/project-rome/notifications/how-to-guide-for-android)と[iOS](https://docs.microsoft.com/windows/project-rome/notifications/how-to-guide-for-ios)します。
リモート カメラ | DeviceWatcher クラスを使用して[リモート カメラに接続する](../audio-video-camera/connect-to-remote-cameras.md)、し、Windows アプリにそのカメラからフレームを読み込みます。
デスクトップ アプリケーション (XAML 諸島) に UWP コントロール | WPF、Windows フォーム、UWP コントロールをホストするための Windows SDK の Api とC++Win32 デスクトップ アプリケーションでは不要になった developer preview。 詳細については、次を参照してください。[デスクトップ アプリケーションでの UWP コントロール](../xaml-platform/xaml-host-controls.md)します。
Visual Studio 2019 | 最新のツールや開発者、アプリ、またはプラットフォーム サービスと、visual Studio 2019 が離されました。 チェック アウト[新機能については Visual Studio 2019](https://docs.microsoft.com/visualstudio/ide/whats-new-visual-studio-2019?view=vs-2019)最新を開始します。
Win32 WebView | この[よく寄せられる質問](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webview#frequently-asked-questions-faqs)サンプルとその他のリソースへのリンクと同様に、デスクトップ アプリケーションで Microsoft Edge の WebView を使用する場合は、よく寄せられる質問に対する回答を提供します。
Windows コマンドライン | [コンソールの新機能](https://devblogs.microsoft.com/commandline/new-experimental-console-features/)スクロール、カーソルの形状カーソルの色の設定では、実験的なターミナル タブが含まれます。 詳細について、 [Windows コマンド ライン ツール開発者向けブログ](https://devblogs.microsoft.com/commandline/)します。
Windows  コミュニティ ツールキット | Windows コミュニティ Toolkit v5.1 は、アニメーション、リモート デバイス、イメージのトリミング、およびユーザー補助の魅力的な更新プログラムを提供します。 </br> • 新しい[Lottie Windows ライブラリ](https://docs.microsoft.com/windows/communitytoolkit/animations/lottie)Windows.UI.Composition Api を利用することで、Windows 10 (1809) で高品質のアニメーション サポートを提供でき、消費量の[Bodymovin](https://aescripts.com/bodymovin/) JSON ファイルまたはWindows アプリで再生するためのコードで生成されたクラスが最適化されています。 新しいお試しください[Lottie ビューアー アプリ](https://aka.ms/lottieviewer)アニメーションをテストし、Windows アプリの最適化されたコードを生成する Microsoft Store から。 </br> • 新しい[リモート デバイスの選択](https://docs.microsoft.com/windows/communitytoolkit/controls/remotedevicepicker)デバイスを選択できます (proximally またはクラウドにアクセスできる)、そのデバイスでアプリを起動またはリモート デバイスにアプリ サービスと通信します。 </br> • 新しい[ImageCropper コントロール](https://docs.microsoft.com/windows/communitytoolkit/controls/imagecropper)プロファイル写真の選択や写真編集ツールを使用して、トリミング機能を統合します。 </br> • さらに、されましたが、コントロールのアクセシビリティ機能改善を[Microsoft.Toolkit.Win32](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32) 6.0 WPF および WinForms より多くの機能では読み取ることができるパッケージの更新のプレビュー、[リリース ノート](https://github.com/windows-toolkit/WindowsCommunityToolkit/releases/tag/v5.1.0).
Windows Machine Learning | Windows の AI のドキュメント、3 つの領域に分割する再設計しました。Windows の Machine Learning の (WinML)、Windows、スキルのビジョンし、の直接の Machine Learning (DirectML)。 チェック アウト、新しい[ランディング ページ](https://docs.microsoft.com/windows/ai/) </br> •、 [ *MLGen*エクスペリエンス](https://docs.microsoft.com/windows/ai/mlgen)Visual Studio で変更されます。 Windows 10、バージョンが 1903 以降で*mlgen*は、Windows 10 SDK には含まれていません。 VS 2017 を使用している場合は代わりにダウンロードしてインストールする Visual Studio 拡張機能[Windows Machine Learning コード ジェネレーターの VS 2017](https://marketplace.visualstudio.com/items?itemName=WinML.mlgen)します。 Visual Studio 2019 を使用している場合をインストール、 [Windows Machine Learning のコード ジェネレーター](https://marketplace.visualstudio.com/items?itemName=WinML.mlgenv2)拡張機能。 </br> • もにお応えしてパッキング重みに対する新しいサポートを発表します。 開発者にはをとおして利用可能な重みの梱包と呼ばれる手法を使用して、ML モデルのディスク フット プリントを削減することができますようになりましたできます。、 [WinMLTools コンバーター](https://docs.microsoft.com/windows/ai/convert-model-winmltools)します。
WinRT の参照の統合 | 詳細な説明が追加されました、 [WinRT 型システム](https://docs.microsoft.com/uwp/winrt-cref/winrt-type-system)と[WinMD ファイル](https://docs.microsoft.com/uwp/winrt-cref/winmd-files)、WinRT Api の構造の定義に関する詳細な注記を提供します。
Windows Subsystem for Linux (WSL) | [WSL の最新のアップデート](https://devblogs.microsoft.com/commandline/whats-new-for-wsl-in-windows-10-version-1903/)wsl.exe と wslconfig.exe ファイル エクスプ ローラー、およびいくつかの新しいコマンドを使用して Windows から Linux ファイルにアクセスする機能が含まれます。
Windows Vision Skills | [Windows ビジョン スキル](https://docs.microsoft.com/windows/ai/windows-vision-skills)はできるようにする Api のセットが「スキル」顔の認識などを作成し、機械学習モデルを含めるもしなくても、その他のアプリで使用できる NuGet パッケージとしてパッケージ化します。

## <a name="publish--monetize-windows-apps"></a>Windows アプリを公開および収益化する

機能 | 説明
 :------ | :------
MSIX | [Windows 10 で MSIX サポート ビルド 1709 と 1803](https://docs.microsoft.com/windows/msix/msix-1709-and-1803-support) Windows 10、バージョンは 1809 より前に、のバージョンでサポートされるどの MSIX 機能について説明します。
MSIX パッケージ化とデプロイ | いくつか導入されています[の機能強化に関連する変更パッケージ](https://docs.microsoft.com/windows/msix/modification-package-insider-preview-build-18312)MSIX パッケージにパッケージのカスタマイズを容易にできるようにします。 これらの機能強化は、新しい**rescap6:ModificationPackage**内の要素、パッケージ マニフェスト、修正パッケージでは、メイン パッケージ内のファイルをオーバーライドする機能、およびファイル システムをパッケージ化する機能はプラグイン ベースMSIX 変更パッケージ。
MSIX パッケージ作成ツール | 追加しました •[リモート コンピューター上の変換を実行するためのサポート](https://docs.microsoft.com/windows/msix/packaging-tool/remote-conversion-setup)します。 また、 [MSIX パッケージ化ツール Insider Program](https://docs.microsoft.com/windows/msix/packaging-tool/insider-program)ツールの新機能にいち早くアクセスを提供します。 </br> • [MSIX パッケージ サポート 1709 以降](https://docs.microsoft.com/windows/msix/packaging-tool/support-on-1709-and-later)MSIX パッケージ化ツールを使用して、具体的には、Windows 10 バージョン 1709 および 1803 のパッケージを作成する方法のガイダンスを提供します。 </br> • [、HYPER-V の簡易作成でパッケージ化環境を MSIX](https://docs.microsoft.com/windows/msix/packaging-tool/quick-create-vm) MSIX パッケージ プロジェクト用の仮想環境を作成する方法を示します。 </br> •[バンドル MSIX パッケージ](https://docs.microsoft.com/windows/msix/packaging-tool/bundle-msix-packages)MSIX パッケージ化ツールを使用してパッケージのバンドルを作成する方法について説明します。 </br> • [Windows 10 バージョンは 1809 変更パッケージ](https://docs.microsoft.com/windows/msix/modification-package-1809-update)MSIX パッケージ化ツールと MakeApp.exe を使用して 1809 およびそれ以降のバージョンの Windows 10 のバージョンの変更のパッケージを作成するための手順について説明します。
MSIX SDK | [MSIX SDK を使用してクロスプラット フォームで使用するためのパッケージをビルド](https://docs.microsoft.com/windows/msix/msix-sdk/sdk-guidance)のパッケージを抽出するターゲット プラットフォームを指定する方法について説明します。

## <a name="microsoft-learn"></a>Microsoft がについて説明します

Microsoft の学習は、Microsoft の開発者に新しい実践的な学習とトレーニングの機会を提供します。

* Windows アプリを開発する方法を学習する場合は、チェック アウト[、新規のラーニング パス](https://docs.microsoft.com/learn/paths/develop-windows10-apps/)徹底的な概要については、プラットフォーム、ツール、および、最初のいくつかのアプリを記述する方法。

* Windows アプリに UI 機能を追加する方法について説明しますか? について説明する方法[UI 作成](https://docs.microsoft.com/learn/modules/create-ui-for-windows-10-apps/)、[ナビゲーションとメディアを UI に追加](https://docs.microsoft.com/learn/modules/enhance-ui-of-windows-10-app/)、または[データ バインディングを実装](https://docs.microsoft.com/learn/modules/implement-data-binding-in-windows-10-app/)。

* Web 開発に関心がある場合はチェック アウト[Visual Studio Code での web アプリケーションを開発](https://docs.microsoft.com/learn/modules/develop-web-apps-with-vs-code/)または[単純な web サイトの構築](https://docs.microsoft.com/learn/modules/build-simple-website/)します。

* または、自由に参照[Microsoft 学習上のすべての Windows 開発者モジュール](https://docs.microsoft.com/learn/browse/?products=windows&resource_type=module)します。

## <a name="videos"></a>ビデオ

### <a name="progressive-web-apps"></a>プログレッシブ Web アプリ

プログレッシブ Web アプリは、さまざまなブラウザーおよび Windows 10 デバイスのさまざまなネイティブ アプリのように機能する web サイトです。 [ビデオを見る](https://youtu.be/ugAewC3308Y)詳細については、し[ドキュメントのチェック アウト](https://aka.ms/Windows-PWA)を開始します。

### <a name="vs-code-series"></a>VS コード シリーズ

チェック アウト、 [Visual Studio Code での新しいビデオ シリーズ](https://www.youtube.com/playlist?list=PLlrxD0HtieHjQX77y-0sWH9IZBTmv1tTx)VSCode は、それを使用する方法との作成方法についてはします。

### <a name="mixed-reality-services"></a>実際にはサービスの混在

HoloLens 2 が最近発表しました。 この[複合現実でのビデオ シリーズ](https://www.youtube.com/watch?v=pdB7Ukf3u0I&list=PLlrxD0HtieHjh2Nt2BhcluIZeQg0uBZST)の最新情報、および参加して、開発を開始する方法。

### <a name="one-dev-question"></a>開発用の 1 つの質問

開発用の 1 つの質問のビデオ シリーズでは、マイクロソフトのベテランの開発者は、一連の Windows の開発、チームのカルチャ、および履歴に関する質問を説明します。

* [Windows 開発と履歴 Raymond Chen](https://www.youtube.com/playlist?list=PLWs4_NfqMtoxjy3LrIdf2oamq1coolpZ7)

* [Windows 開発と履歴の Larry Osterman](https://www.youtube.com/playlist?list=PLWs4_NfqMtoyPUkYGpJU0RzvY6PBSEA4K)

* [プログレッシブ Web Apps での Aaron グスタフソン](https://www.youtube.com/playlist?list=PLWs4_NfqMtoyPHoI-CIB71mEq-om6m35I)

* [Chris Heilmann webhint ツール](https://www.youtube.com/playlist?list=PLWs4_NfqMtow00LM-vgyECAlMDxx84Q2v)
