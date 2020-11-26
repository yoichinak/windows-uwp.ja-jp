---
title: Windows 10 ビルド 17763 の新着情報
description: Windows 10 ビルド 17763 と新しい開発者ツールでは、ユニバーサル Windows プラットフォームによって強化されたツール、機能、エクスペリエンスを利用できます。
keywords: Windows 10, 17763, 1809
ms.date: 10/03/2018
ms.topic: article
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 416e3947678de0ba70687d9070c245fdfb4376df
ms.sourcegitcommit: 67c4d4ecda4ffe5f1a233de5e8555ca2228e8489
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/19/2020
ms.locfileid: "94933187"
---
# <a name="whats-new-in-windows-10-for-developers-build-17763"></a>Windows 10 ビルド 17763 の開発者向け新着情報

Windows 10 ビルド 17763 (October 2018 Update またはバージョン 1809 とも呼ばれます) では、Visual Studio 2019 や更新された SDK と組み合わせて使うことで、優れたユニバーサル Windows プラットフォーム アプリを作成するためのツール、機能、エクスペリエンスが利用可能になります。 Windows 10 の[ツールと SDK をインストール](https://developer.microsoft.com/windows/downloads#_blank)すると、[新しいユニバーサル Windows アプリを作成](../get-started/create-uwp-apps.md)したり、[Windows の既存のアプリ コード](../porting/index.md)がどのように使えるかを試したりすることができます。

ここには、Windows 開発者にとって重要なこのリリースの新機能、強化された機能、ガイダンスを集めました。 Windows SDK に追加されたすべての新しい名前空間の一覧については、「[Windows 10 ビルド 17763 API の変更点](windows-10-build-17763-api-diff.md)」をご覧ください。 Windows 10 での注目すべき機能について詳しくは、「[Windows 10 の優れた機能](https://developer.microsoft.com/windows/windows-10-for-developers)」をご覧ください。 また、Windows プラットフォームに過去に追加された機能と今後追加される機能の概要については、[Windows 開発者向けプラットフォーム機能](https://developer.microsoft.com/windows/platform/features)に関するページをご覧ください。

## <a name="design--ui"></a>設計および UI

機能 | 説明
 :------ | :------
アプリのアイコンとロゴ | [アプリのアイコンとロゴ ページ](../design/style/app-icons-and-logos.md)が書き換えられ、最新の Visual Studio アイコン ツールが表示され、Microsoft Store のアプリ一覧へのイメージ追加に関する情報が提供されるようになりました。
設計ランディング ページ | [更新された設計ランディング ページ](https://developer.microsoft.com/windows/apps/design)には、UWP デザイン領域の概要と、Fluent Design への最新の追加に関する情報が表示されます。
Fluent Design のコントロール | Fluent Design System を強化してアプリの外観を向上させるために、次の新しい UI コントロールが追加されました。 </br> * [CommandBarFlyout](../design/controls-and-patterns/command-bar-flyout.md) を使用すると、UI キャンバス上の項目のコンテキストで一般的なユーザー タスクを表示できます。 </br> * [DropDownButton](../design/controls-and-patterns/buttons.md#create-a-drop-down-button)、[SplitButton](../design/controls-and-patterns/buttons.md#create-a-split-button)、および [ToggleSplitButton](../design/controls-and-patterns/buttons.md#create-a-toggle-split-button) は、アプリのユーザー インターフェイスを強化するための特別な機能を持つボタン コントロールを提供します。 </br> * [MenuBar](../design/controls-and-patterns/menus.md) を使用すると、横一列に複数のトップレベル メニューを表示できます。 </br> * [NavigationView](../design/controls-and-patterns/navigationview.md) は、アプリのナビゲーション オプションの数が少なく、コンテンツにより多くの領域が必要な場合のために、上部ナビゲーションをサポートするようになりました。 </br> * [TreeView](../design/controls-and-patterns/tree-view.md) が拡張されて、データ バインディング、項目テンプレート、ドラッグ アンド ドロップをサポートするようになりました。
Fluent Design の更新 | 以下の Fluent Design のページに視覚的な更新と軽微な変更が加えられました。 </br> * [配置、埋め込み、余白](../design/layout/alignment-margin-padding.md) </br> * [色](../design/style/color.md) </br> * [Windows アプリ用の Fluent Design](/windows/apps/fluent-design-system) </br> * [アプリ設計の概要](../design/basics/design-and-ui-intro.md) </br> * [ナビゲーションの基本](../design/basics/navigation-basics.md) </br> * [レスポンシブ デザインの手法](../design/layout/responsive-design.md) </br> * [画面のサイズとブレークポイント](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md) </br> * [スタイルの概要](../design/style/index.md) </br> * [記述スタイル](../design/style/writing-style.md) </br> さらに、以下のページは、そのコンテンツ領域に関するまったく新しい情報に書き換えられました。 </br> 「* [アイコン](../design/style/icons.md)」には、アイコンを使用してそれをクリック可能にするための実践的な推奨事項が示されています。 </br> 「* [Typography](../design/style/typography.md)」(文字体裁) では類似の記事からの情報を統合し、最新のガイダンスと図を使ってすべてを 1 か所にまとめました。
視線入力と操作 | [視線の操作](../design/input/gaze-interactions.md)では、アプリがユーザーの目の場所と動きに基づいてユーザーの視線、注意、およびプレゼンスを追跡することができます。 この機能は支援技術として使用でき、従来の入力デバイスが利用できないゲームやその他の対話型シナリオのための機会を提供します。
手書きビュー | [HandwritingView](../design/controls-and-patterns/text-handwriting-view.md) は、TextBox および RichEditBox のための新しいインク入力画面です。 ユーザーは、ペンでテキスト コントロールをタップすることで、コントロールを書き込み画面に拡張することができます。 このガイダンスでは、アプリケーション内で HandwritingView を管理およびカスタマイズする方法について説明します。
Fluent Design でのモーション | Fluent Design System でのモーションの使用が進化しています。モーションを構築する基本要素は、タイミング、イージング、方向性、重力です。 これらの基本要素を適用すると、アプリ全体を通してユーザーを誘導することに役立ち、自然な世界を反映することでユーザーをデジタル エクスペリエンスに結び付けることができます。 詳細はこれらの記事をご覧ください。 </br> 「* [The Motion overview](../design/motion/index.md)」(モーションの概要) は、これらの基本要素を反映するように更新されました。 </br> 「* [Motion-in-practice](../design/motion/motion-in-practice.md)」(実践的なモーション) では、アプリ内でこれらの基本要素を適用する方法の例が示されています。 また、これには暗黙的なアニメーションに関する情報も含まれています。これにより、XAML 要素のプロパティが変更されたときに、古い値と新しい値の間の補間を簡単に行うことができます。 </br> 「* [方向性と重力](../design/motion/directionality-and-gravity.md)」では、自作のアプリのユーザーのメンタル モデルを強固にします。 </br> 「* [タイミングとイージング](../design/motion/timing-and-easing.md)」では、自作のアプリ内のモーションに現実感を与えます。 </br> * [XAML プロパティ アニメーション](../design/motion/xaml-property-animations.md)により、基になるコンポジションの Visual と対話することなく、XAML 要素のプロパティを直接アニメーション化できます。
ページ切り替え効果 | [ページ切り替え効果](../design/motion/page-transitions.md)により、ユーザーはアプリ内のページ間を移動します。 ナビゲーション階層のどこにいるのかをユーザーが理解し、ページ間の関係についてフィードバックを提供するのに役立ちます。
テキストの拡大縮小 | 新しい[テキストの拡大縮小ガイダンス](../design/input/text-scaling.md)では、新しいテキストの拡大縮小の動作に対応するようアプリケーションを更新する方法について説明します。これにより、ユーザーは OS と個々のアプリケーションの両方で相対的なフォント サイズを変更できます。 拡大鏡アプリ (これは通常、画面の領域内のすべてを拡大するだけであり、独自のユーザビリティの問題が発生する) を使用したり、ディスプレイの解像度を変更したり、DPI スケール (これは、ディスプレイと標準的な表示距離に基づいてすべてのサイズを変更する) に依存したりする代わりに、ユーザーはテキストだけを 100% (既定のサイズ) から最大 225% までの範囲でサイズ変更するための設定にすばやくアクセスできます。
ツールキット | [Adobe XD および Adobe Illustrator ツールキット](../design/downloads/index.md)が更新されて新機能が追加されました。 これらの設計ツールキットは、UWP アプリを設計するためのコントロールとレイアウトのテンプレートを提供します。
UI コマンド処理 | [UWP コマンド処理インフラストラクチャ](../design/basics/commanding-basics.md)の更新には、コマンド オブジェクト (動作、ラベル、アイコン、キーボード アクセラレータ、アクセス キー、および説明) のカプセル化の改善と、切り取り、コピー、貼り付け、終了などの一般的なコマンドの標準セットが含まれており、これらのプロパティを手動で設定する必要がなくなりました。 </br> 新しい [XamlUICommand](/uwp/api/windows.ui.xaml.input.xamluicommand) クラスは、呼び出されたときにアクションを実行する対話型の UI 要素のコマンドの動作を定義するための基本クラスを提供します。 これは、定義済みのプロパティを持つ標準プラットフォームのコマンドのセットを公開する [StandardUICommand](/uwp/api/windows.ui.xaml.input.standarduicommand) の親クラスです。 
Windows UI ライブラリ | [Windows UI ライブラリ](/uwp/toolkits/winui/)は、UWP アプリ用のコントロールとその他のユーザー インターフェイス要素を提供する NuGet パッケージのセットです。 これらのパッケージは Windows 10 の以前のバージョンにも対応しているため、ユーザーが最新の OS を持っていない場合でも、アプリは動作します。 </br> Windows UI ライブラリの内容の詳細については、[NuGet パッケージに含まれる API 名前空間を示すこちらの一覧](/windows/winui/api/)を参照してください。

## <a name="develop-windows-apps"></a>Windows アプリの開発

機能 | 説明
 :------ | :------
バーコード スキャナー | [バーコード スキャナー](../devices-sensors/pos-barcodescanner.md)のドキュメントが再編成され、さらに詳しい情報とコード スニペットを使用して改善されています。 さらに、新しいトピック「[バーコード データの取得と理解](../devices-sensors/pos-barcodescanner-scan-data.md)」が追加され、バーコード スキャナーからデータを取得して操作する方法が説明されています。
C++/WinRT | [C++/WinRT](../cpp-and-winrt-apis/index.md) には、このリリースに関する多くの新機能、変更、および修正が含まれています。 ユーザーが独自の[コレクションのプロパティおよびコレクション型](../cpp-and-winrt-apis/collections.md)を実装するのをサポートする新しい関数および基本クラスがあります。また、[{binding}](../xaml-platform/binding-markup-extension.md) XAML マークアップ拡張機能を C++/WinRT ランタイム クラスとともに使用できるようになりました (コード例については、[データ バインディングの概要](../data-binding/data-binding-quickstart.md)に関する記事を参照してください)。 このリリースのすべての新機能および変更された機能の詳細な説明については、「[C++/WinRT の新機能](../cpp-and-winrt-apis/news.md)」を参照してください。</br></br>C++/WinRT に関するその他の新しい内容には、[XAML カスタム コントロール](../cpp-and-winrt-apis/xaml-cust-ctrl.md)、[COM コンポーネントの作成](../cpp-and-winrt-apis/author-coclasses.md)、[値のカテゴリ](../cpp-and-winrt-apis/cpp-value-categories.md)、[強参照と弱参照](../cpp-and-winrt-apis/weak-references.md)などがあります。
C++/WinRT コード例 | 既存の C++/CX コード例とともに、250 件の C++/WinRT コード リストをドキュメントのトピックに追加しました。
共同作成に関するガイダンス | Microsoft の UWP ドキュメントを対象にした[共同作成のガイダンス](https://github.com/MicrosoftDocs/windows-uwp/blob/docs/CONTRIBUTING.md)を更新しました。 この新しいガイダンスでは、Microsoft のドキュメントに対する外部貢献のワークフローとその貢献に対して期待することを明確にしています。
DirectX グラフィックス インフラストラクチャ (DXGI) | 不足している DXGI API についての新しいドキュメントが追加され、Windows 10 上で動作するときのベスト プラクティスに関する記事が提供されています。 </br> * [最適なパフォーマンスを得るには、DXGI フリップ モデルを使用してください](/windows/desktop/direct3ddxgi/for-best-performance--use-dxgi-flip-model)。最新バージョンの Windows でプレゼンテーション スタックのパフォーマンスと効率を最大化する方法について説明します。 </br> * [IDXGIOutput6::CheckHardwareCompositionSupport メソッド](/windows/desktop/api/dxgi1_6/nf-dxgi1_6-idxgioutput6-checkhardwarecompositionsupport):ハードウェアの拡大がサポートされていることをアプリケーションに通知します。 </br> * [DXGI_HARDWARE_COMPOSITION_SUPPORT_FLAGS 列挙](/windows/desktop/api/dxgi1_6/ne-dxgi1_6-dxgi_hardware_composition_support_flags):サポートされているハードウェア構成のレベルについて説明します。
開始 | 「[はじめに](../get-started/index.md)」のコンテンツが新しいトピックによって改訂されており、Windows 10 を初めて使用する開発者が次の一般的なタスクを実行するための方法についての情報とガイダンスが示されます。 </br> * [フォームの作成](../get-started/construct-form-learning-track.md) </br> * [一覧での顧客の表示](../get-started/display-customers-in-list-learning-track.md) </br> * [設定の保存と読み込み](../get-started/settings-learning-track.md) </br> * [ファイルの操作](../get-started/fileio-learning-track.md)
マップ スタイル シート エディター | 新しい[マップ スタイル シート エディター](https://www.microsoft.com/p/map-style-sheet-editor/9nbhtcjt72ft?rtc=1#activetab=pivot:overviewtab) アプリケーションを使用して、アプリケーションに追加するマップの外観を対話形式でカスタマイズできます。
Microsoft Learn | 新しい [Microsoft Learn サイト](https://www.microsoft.com/learning/default.aspx)は、新しい実践的な学習とトレーニングの機会を Microsoft 開発者に提供します。 現在、Microsoft Learn では Microsoft 365、Microsoft Azure、Windows Server についてのトレーニングと認定を提供しています。
メモ帳 | [メモ帳が更新](https://blogs.windows.com/windowsexperience/2018/07/11/announcing-windows-10-insider-preview-build-17713/)されており、ズーム、折り返し検索/置換、および Unix/Linux と Mac の行の末尾 (それぞれ LF および CR) のサポートが追加されました。
Project Rome | [Project Rome](/windows/project-rome/) では、サポートされるすべてのプラットフォームおよび SDK にわたって一貫性のあるプログラミング エクスペリエンスが提供されるようになりました。 </br>  新しい [Microsoft Graph 通知](/graph/notifications-concept-overview)では、Project Rome を使用して、アプリのためのユーザーを中心としたクロスプラットフォーム通知プラットフォームが提供されます。
画面切り取り | 新しい [URI スキーム](../launch-resume/launch-screen-snipping.md)によって、アプリで新しい切り取り領域をプログラムで開いたり、注釈用の特定のイメージとともに切り取り & スケッチ アプリを起動したりできます。
デスクトップ アプリケーションの UWP コントロール | Windows 10 では、WPF、Windows フォーム、および C++ Win32 デスクトップ アプリケーションで UWP コントロールを使用できるようになりました。 つまり、Windows Ink や Fluent Design System をサポートするコントロールなど、UWP コントロールでのみ利用可能な最新の Windows 10 UI 機能で、既存のデスクトップ アプリケーションの外観、操作性、機能を拡張することができます。 この機能を *XAML Islands* といいます。 </br> お使いのアプリケーション プラットフォームに応じて、アプリケーションで XAML Islands を使用するいくつかの方法を紹介します。 WPF および Windows Forms アプリケーションでは、デザイナー指向の開発エクスペリエンスを提供する[Windows コミュニティ ツールキット](/windows/uwpcommunitytoolkit/)内の一連のコントロールを使用できます。 C++ Win32 アプリケーションは、[Windows.UI.Xaml.Hosting](/uwp/api/windows.ui.xaml.hosting) 名前空間の *UWP XAML ホスティング API* を使用する必要があります。 詳細については、[デスクトップ アプリケーションでの UWP コントロール](/windows/apps/desktop/modernize/xaml-islands)に関するページを参照してください。 </br> **注:** XAML Islands を有効にする API およびコントロールは、開発者プレビューとして現在使用できます。 ご自身のプロトタイプ コードでこれらを試すことはお勧めしますが、現時点では運用コードで使用することはお勧めしません。
Windows Machine Learning | [Windows Machine Learning](/windows/ai/) が正式に発表され、最先端の機械学習モデルのサポートと高速な評価などの機能が提供されます。 これをアプリケーションに統合する開発者をサポートするために、いくつかの新規および更新リソースが含まれた、新しいドキュメント サイトが作成されました。 </br> * [チュートリアル:Windows Machine Learning のデスクトップ アプリケーションの作成 (C++)](/windows/ai/get-started-desktop):このチュートリアルでは、デスクトップ向けの単純な Windows ML アプリケーションを構築する方法を示します。 </br> * [チュートリアル:Windows Machine Learning の UWP アプリケーションの作成 (C#)](/windows/ai/get-started-uwp):このステップ バイ ステップ チュートリアルを使って、Windows ML で初めての UWP アプリケーションを作成します。 </br> * [Windows.AI.MachineLearning 名前空間](/uwp/api/windows.ai.machinelearning):API リファレンスが Windows 10 SDK の最新リリース用に更新され、開発者は Win32 と UWP の両方のアプリケーションに対してこの API を使用できるようになりました。
Windows Mixed Reality | ディスプレイ ハードウェアでサポートされている場合、開発者はハードウェアで保護されているバックバッファー テクスチャを要求できるようになりました。これによりアプリケーションは PlayReady などのソースから、ハードウェアで保護されているコンテンツを使用することができます。 ハードウェア保護のサポートおよび設定は、一次レイヤーの場合は新しいプロパティ [Windows.Graphics.Holographic.HolographicCamera](/uwp/api/windows.graphics.holographic.holographiccamera) を使用して、クアッド レイヤーの場合は [Windows.Graphics.Holographic.HolographicQuadLayerUpdateParameters](/uwp/api/windows.graphics.holographic.holographicquadlayerupdateparameters) を介して利用できます。

## <a name="iot-core"></a>IoT Core

機能 | 説明
 :------ | :------
AssignedAccessSettings | [AssignedAccessSettings クラス](/uwp/api/windows.system.userprofile.assignedaccesssettings)を使用すると、特定のデバイスに対するユーザーの割当て済みアクセス設定にアクセスするための、さまざまなメソッドおよびプロパティの呼び出しが可能になります。
既定アプリの概要 | [Windows 10 IoT Core の既定アプリ](/windows/iot-core/develop-your-app/iotcoredefaultapp)が更新され、天気、手描き入力、オーディオなどの新機能が追加されました。
ダッシュボード | [Windows 10 Iot Core ダッシュボード](/windows/iot-core/tutorials/quickstarter/devicesetup)では、Dragonboard 410 C または NXP を使用する開発者は、カスタム FFU を自分のデバイス上にフラッシュできるようになりました。
スクリーン キーボード | [IoT デバイス用のスクリーン キーボード](/windows/iot-core/develop-your-app/onscreenkeyboard)では、Windows のデスクトップ エディションと同じタッチ キーボード コンポーネントが使用されるようになりました。 これにより、ディクテーション モード、IME サポート、入力スコープの完全なセットなどの機能が有効になります。
サインイン ダイアログのためのタイトル バー | Windows 10 IoT Core では、[システム ダイアログ ボックスのためのタイトル バー](/windows/iot-core/develop-your-app/signindialogtitlebars)を構成するオプションが提供されるようになりました。
Wake On Touch | [Wake On Touch](/windows/iot-core/learn-about-hardware/wakeontouch) によって、使用していないときはデバイスの画面の電源がオフになり、ユーザーが画面に触れるとすぐにオンになります。
Windows.System.Update | 新しい [Windows.System.Update 名前空間](/uwp/api/windows.system.update)によって、システム更新の対話型コントロールが可能になります。 この名前空間は Windows 10 IoT Core でのみ利用可能です。

## <a name="web-development"></a>Web 開発

機能 | 説明
 :------ | :------
EdgeHTML 18 | Windows 10 October 2018 Update には、Microsoft Edge ブラウザーと UWP アプリ用 JavaScript エンジンの最新の更新である [EdgeHTML 18](/microsoft-edge/dev-guide) が付属しています。 EdgeHTML 18 では、Web Authentication API や新しい WebView コントロール機能などのサポートが最新化および拡張されています。 ツール側では、EdgeHTML 18 は WebDriver の新しい機能と自動更新、および Edge DevTools と Edge DevTools Protocol の拡張機能を提供します。 詳しくは、[EdgeHTML 18 の新機能](/microsoft-edge/dev-guide)および[最新の Windows 10 の更新 (EdgeHTML 18) の DevTools](/microsoft-edge/devtools-guide/whats-new) に関するページを確認してください。
プログレッシブ Web アプリ | Windows 10 の JavaScript アプリ (*WWAHost.exe* プロセスで実行されている Web アプリ) では、任意のビューがアクティブ化される前に開始されて処理中に実行される、オプションの [アプリケーション別のバックグラウンド スクリプト](/microsoft-edge/dev-guide#progressive-web-apps)がサポートされるようになりました。 これにより、ナビゲーションの監視および変更、ナビゲーション全体での状態の追跡、ナビゲーション エラーの監視、およびコードの実行を、ビューがアクティブ化される前に行うことができます。 [アプリ マニフェスト](/uwp/schemas/appxpackage/appx-package-manifest)内で [`StartPage`](/uwp/schemas/appxpackage/appxmanifestschema2010-v2/element-application) として指定する場合、アプリの各ビュー (ウィンドウ) は新しい [`WebUIView`](/uwp/api/windows.ui.webui.webuiview) クラスのインスタンスとしてスクリプトに公開され、一般的な (Win32) [WebView](/uwp/api/windows.web.ui.iwebviewcontrol) と同じイベント、プロパティ、およびメソッドを提供します。
Web API の拡張機能 | クロスブラウザー Web 開発用の Mozilla Developer Network ドキュメントに、[従来の Microsoft API 拡張機能](https://developer.mozilla.org/docs/Web/API/Microsoft_API_extensions)の一覧が追加されました。 これらの API 拡張機能は Internet Explorer または Microsoft Edge に固有のもので、MDN の Web ドキュメントの互換性とブラウザーのサポートに関する既存の情報を補足します。従来の Microsoft [CSS 拡張機能](https://developer.mozilla.org/docs/Web/CSS/Microsoft_Extensions)と [JavaScript 拡張機能](https://developer.mozilla.org/docs/Web/JavaScript/Microsoft_JavaScript_extensions)も利用でき、リッチな Web API 情報を、[Visual Studio Code](https://code.visualstudio.com/updates/v1_25#_new-css-pseudo-selectors-and-pseudo-elements-from-mdn) で直接表示される MDN から見つけることができます。
WebVR | 「[WebVR Developer's Guide](/microsoft-edge/webvr/)」を大幅に更新し、ホームページの完全な再設計や目次の再編成などを行いました。 また、以下に示すいくつかの新しいトピックを作成しました。 </br> * [WebVR とは](/microsoft-edge/webvr/what-is-webvr) WebVR とは何か、それを使用する理由、およびそれに対する開発を開始する方法について説明します。 </br> * [プログレッシブ Web アプリでの WebVR](/microsoft-edge/webvr/webvr-in-pwas):プログレッシブ Web アプリ (PWA) に WebVR を追加する方法について説明します。 </br> * [WebView の WebVR](/microsoft-edge/webvr/webvr-in-webview):Windows 10 のアプリケーションで WebView コントロールに WebVR を追加する方法について説明します。 </br> * [WebVR のデモ](/microsoft-edge/webvr/demos):Microsoft Edge と Windows Mixed Reality イマーシブ ヘッドセットを使用して、WebVR のいくつかのデモをご覧ください。

## <a name="publish--monetize-windows-apps"></a>Windows アプリを公開および収益化する

機能 | 説明
 :------ | :------
MSIX | [MSIX](/windows/msix/overview) は、あらゆる Windows アプリに最新のパッケージ化エクスペリエンスを提供する新しい Windows アプリ パッケージ形式です。 オープン ソースの MSIX 形式により、既存のパッケージ機能を維持しつつ、最新の展開機能を有効にすることができます。
MSIX パッケージ作成ツール | 新しい [MSIX パッケージ化ツール](/windows/msix/mpt-overview)を使用すると、既存のデスクトップ アプリケーションを、そのソース コードにアクセスできない場合でも、MSIX 形式で再パッケージ化することができます。 これはコマンドラインまたはその対話型 UI を使用して実行できます。
MSIX のための Desktop App Converter サポート | [Desktop App Converter](/windows/msix/desktop/source-code-overview) で、`-MakeMSIX` パラメーターを使用することによって、MSIX パッケージを出力できます。
MSIX のための MakeAppx.exe ツール サポート | MakeAppx.exe ツールを使用して、UWP アプリまたは従来のデスクトップ アプリケーションの MSIX パッケージを作成できます。 このツールは、Windows 10 SDK に含まれており、コマンド プロンプトまたはスクリプト ファイルから使用できます。 </br> UWP アプリの場合は、「[MakeAppx.exe ツールを使ったアプリ パッケージの作成](/windows/msix/package/create-app-package-with-makeappx-tool)」をご覧ください。 </br> デスクトップ アプリケーションの場合は、「[デスクトップ アプリケーションを手動でパッケージ化する](/windows/msix/desktop/desktop-to-uwp-manual-conversion)」をご覧ください。
パッケージ サポート フレームワーク | [パッケージ サポート フレームワーク](/windows/msix/package-support-framework-overview)は、ソース コードにアクセスできない場合に既存のデスクトップ アプリケーションに修正を適用して MSIX コンテナー内で実行できるようにするのに役立つオープン ソース キットです。
Store 分析 API | [Microsoft Store 分析 API](../monetize/access-analytics-data-using-windows-store-services.md) には以下の新しいメソッドが含まれています。 </br> * [UWP アプリのインサイト データの取得](../monetize/get-insights-data-for-your-app.md) </br> * [デスクトップ アプリケーションのインサイト データの取得](../monetize/get-insights-data-for-your-desktop-app.md) </br>* [デスクトップ アプリケーションのアップグレード ブロックの取得](../monetize/get-desktop-block-data.md) </br> * [デスクトップ アプリケーションのアップグレード ブロックの詳細情報の取得](../monetize/get-desktop-block-data-details.md)

## <a name="videos"></a>ビデオ

Fall Creators Update 以降、以下のビデオが公開されました。Windows 10 の開発者向け新機能および強化された機能が紹介されています。

### <a name="cwinrt"></a>C++/WinRT

C++/WinRT は、Windows ランタイム API を作成および使用するための新しい方法です。 ヘッダー ファイルに単独で実装され、最新のアプリ機能への最適なアクセスを提供するように設計されています。 [ビデオを視聴して](https://www.youtube.com/watch?v=TLSul1XxppA&feature=youtu.be)そのしくみを確認したうえで、詳細については[開発者向けドキュメントをご覧ください](../cpp-and-winrt-apis/index.md)。

### <a name="get-started-for-devs-create-and-customize-a-form-on-windows-10"></a>開発者向けスタートアップ ガイド:Windows 10 でフォームを作成してカスタマイズする

Windows 開発者向けの[スタートアップ ガイド](../get-started/index.md)では、基本的なアプリ開発タスクを実際に体験できるようになりました。 このビデオは、これらのトピックの 1 つを順を追って説明するもので、アプリでのフォーム UI 作成の基本について扱います。 [ビデオを見て](https://www.youtube.com/watch?v=AgngKzq4hKI&feature=youtu.be)動作中のコードを理解したうえで、[トピックを自分で確認してください。](../get-started/construct-form-learning-track.md)

### <a name="enhance-your-bot-with-project-personality-chat"></a>Project Personality Chat でボットを強化する

Project Personality Chat により、カスタマイズ可能なペルソナをチャット ボットに追加することができます。 Microsoft Bot Framework SDK を統合することで、より会話的な方法で顧客と対話するための複数のスモールトーク機能を追加できます。 [ビデオを見て](https://www.youtube.com/watch?v=5C_uD8g2QKg&feature=youtu.be)これを実装する方法を理解したうえで、[対話的なデモを通して](https://www.microsoft.com/research/project/personality-chat/)実際に体験してみてください。

### <a name="multi-instance-uwp-apps"></a>マルチインスタンスの UWP アプリ

Windows で UWP アプリの複数のインスタンスを実行できるようになりました。各インスタンスは分離された個別のプロセスで実行されます。 この機能をサポートする新しいアプリの作成方法を[ビデオを見て](https://www.youtube.com/watch?v=clnnf4cigd0&feature=youtu.be)確認し、この機能を使用する方法と理由の詳細については、[開発者向けドキュメントをご覧ください](../launch-resume/multi-instance-uwp.md)。

### <a name="xbox-live-unity-plugin"></a>Xbox Live Unity プラグイン

Unity 用の Xbox Live プラグインには、Xbox Live の署名、統計、フレンド リスト、クラウド ストレージ、およびランキングをタイトルに追加するためのサポートが含まれています。 [ビデオを見て](https://youtu.be/fVQZ-YgwNpY)詳細を確認したうえで、[GitHub パッケージをダウンロードして](/gaming/xbox-live/get-started/setup-ide/creators/unity-win10/live-cr-unity-win10-nav?WT.mc_id=windowsdocs-twi)開始してください。

### <a name="one-dev-question"></a>One Dev Question

One Dev Question ビデオ シリーズでは、ベテランの Microsoft 開発者が Windows 開発、チーム カルチャー、および歴史に関する一連の質問に答えています。

* [Raymond Chen: Windows 開発および歴史 ](https://www.youtube.com/playlist?list=PLWs4_NfqMtoxjy3LrIdf2oamq1coolpZ7)

* [Larry Osterman: Windows 開発および歴史](https://www.youtube.com/playlist?list=PLWs4_NfqMtoyPUkYGpJU0RzvY6PBSEA4K)

* [Aaron Gustafson: プログレッシブ Web アプリ](https://www.youtube.com/playlist?list=PLWs4_NfqMtoyPHoI-CIB71mEq-om6m35I)

* [Chris Heilmann: webhint ツール](https://www.youtube.com/playlist?list=PLWs4_NfqMtow00LM-vgyECAlMDxx84Q2v)

## <a name="samples"></a>サンプル

### <a name="customer-orders-database"></a>顧客注文データベース

[顧客注文データベース サンプル](https://github.com/Microsoft/Windows-appsample-customers-orders-database)が更新されて、[DataGrid](/windows/communitytoolkit/controls/datagrid)、[NavigationView](/uwp/api/windows.ui.xaml.controls.navigationview)、[Expander](/windows/communitytoolkit/controls/expander) のような新しいコントロールが使用されるようになりました。

### <a name="customer-database-tutorial"></a>顧客データベース チュートリアル

[顧客データベース チュートリアル](../enterprise/customer-database-tutorial.md)では、顧客の一覧を管理するための基本的な UWP アプリを作成し、エンタープライズ開発の概念と実践を紹介しています。 UI 要素の実装とローカル SQLite データベースに対する操作の追加を順を追って説明し、さらに希望する場合のために、リモート REST データベースへの接続の大まかなガイダンスを示します。

### <a name="photo-editor-cwinrt"></a>フォト エディター C++/WinRT

[フォト エディター サンプル アプリ](https://github.com/Microsoft/Windows-appsample-photo-editor)は、[C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) 言語プロジェクションでの開発を紹介します。 このアプリを使用すると、**画像** ライブラリから写真を取得し、選択した画像を関連する写真効果で編集できます。

### <a name="windows-machine-learning"></a>Windows Machine Learning

[Windows Machine Learning](https://github.com/Microsoft/Windows-Machine-Learning) リポジトリが最新の Windows 10 SDK と連動するように更新され、C#、C++、および JavaScript で記述されたサンプルが含まれています。

### <a name="xaml-hosting-api"></a>XAML ホスティング API

[XAML ホスティング API サンプル](https://github.com/Microsoft/Windows-appsample-Xaml-Hosting)は、UWP XAML ホスティング API (XAML Islands とも呼ばれる) を使用したさまざまなシナリオにスポットを当てる Win32 デスクトップ アプリです。 プロジェクトには、Windows Ink、Media Player、およびナビゲーション ビューのコントロールがギャラリー スタイル表示で組み込まれています。 一般的なコントロールの使用のほかに、サンプルでは XAML とネイティブの Windows イベント/メッセージの処理、および基本的な XAML データ バインディングも例示します。