---
title: Windows ドキュメントの最新情報、2018 年 5 月 - UWP アプリの開発
description: 2018 年 5 月版および Microsoft Build カンファレンスの Windows 10 開発者向けドキュメントには、新しい機能、ビデオ、開発者向けガイダンスが追加されました。
keywords: 最新情報, 更新, 機能, 開発者向けガイダンス, Windows 10, 5 月, build
ms.date: 05/07/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: d9864c59a8bb8569861e9c239710a09602ffdcba
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174356"
---
# <a name="whats-new-in-the-windows-developer-docs-in-may-2018"></a>Windows 開発者向けドキュメントの最新情報、2018 年 5 月

Windows 開発者向けドキュメントは、Windows プラットフォームで開発者に提供される新機能の情報を反映して継続的に更新されています。 ここに示す機能概要、開発者向けガイダンス、ビデオ、サンプルは、[Microsoft Build 2018](https://www.microsoft.com/build/) 開発者カンファレンスに合わせて 5 月に利用可能になりました。

Windows 10 の[ツールと SDK をインストール](https://developer.microsoft.com/windows/downloads#_blank)すると、[新しいユニバーサル Windows アプリを作成](../get-started/create-uwp-apps.md)したり、[Windows の既存のアプリ コード](../porting/index.md)がどのように使えるかを試したりすることができます。

## <a name="features"></a>機能

### <a name="motion-in-fluent-design"></a>Fluent Design でのモーション

Fluent Design System でのモーションの使用が進化しています。モーションを構築する基本要素は、タイミング、イージング、方向性、重力です。 これらの基本要素を適用すると、アプリ全体を通してユーザーを誘導することに役立ち、自然な世界を反映することでユーザーをデジタル エクスペリエンスに結び付けることができます。 詳細はこちらの記事をご覧ください。

* 「[モーションの概要](../design/motion/index.md)」は、これらの基本要素を反映するように更新されました。
* 「[実践的なモーション](../design/motion/motion-in-practice.md)」では、アプリ内でこれらの基本要素を適用する方法の例が示されています。
* 「[方向性と重力](../design/motion/directionality-and-gravity.md)」では、自作のアプリのユーザーのメンタル モデルを強固にします。
* 「[タイミングとイージング](../design/motion/timing-and-easing.md)」では、自作のアプリ内のモーションに現実感を与えます。

![実際の動作を見る](../design/motion/images/contextual.gif)

### <a name="fluent-design-updates"></a>Fluent Design の更新

以下の Fluent Design のページに視覚的な更新と軽微な変更が加えられました。

* [配置、埋め込み、余白](../design/layout/alignment-margin-padding.md)
* [色](../design/style/color.md)
* [コマンドの基本](../design/basics/commanding-basics.md)
* [Windows アプリ用の Fluent Design](/windows/apps/fluent-design-system)
* [アプリ設計の概要](../design/basics/design-and-ui-intro.md)
* [ナビゲーションの基本](../design/basics/navigation-basics.md)
* [レスポンシブ デザインの手法](../design/layout/responsive-design.md)
* [画面のサイズとブレークポイント](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md)
* [スタイルの概要](../design/style/index.md)
* [記述スタイル](../design/style/writing-style.md)

さらに、以下のページはコンテンツ領域に関するまったく新しい情報に書き換えられました。

* 「[アイコン](../design/style/icons.md)」には、アイコンの使用とこれらをクリック可能にするための実践的な推奨事項が示されています。
* 「[文字体裁](../design/style/typography.md)」では類似の記事からの情報を統合し、最新のガイダンスと図を使ってすべてを 1 か所にまとめました。

![カラー パレットの画像](../design/style/images/color/accent-color-palette.svg)

### <a name="app-installer-files-in-visual-studio"></a>Visual Studio でのアプリ インストーラー ファイル

Visual Studio 2017 Update 15.7 以降のバージョンでは、アプリ インストーラー ファイルを作成できるようになりました。 [Visual Studio を使ってアプリ インストーラー ファイルを作成する方法](/windows/msix/app-installer/create-appinstallerfile-vs)と、自動更新を有効にする方法をご確認ください。 問題が発生した場合は、「[アプリ インストーラー ファイルを使ったインストールに関する問題のトラブルシューティング](/windows/msix/app-installer/troubleshoot-appinstaller-issues)」を参照して一般的な問題と解決方法を確認してください。

### <a name="edge-webview-control-for-windows-forms-and-wpf-applications"></a>Windows フォームと WPF アプリケーション用の Edge の WebView コントロール

WebView コントロールを使用して、Web コンテンツをデスクトップ アプリケーションに表示できます (以前は UWP アプリケーションでのみ使用可能でした)。 このコントロールによって、Microsoft Edge レンダリング エンジンを使用して、リモート Web サーバーにあるリッチな書式設定の HTML コンテンツ、動的に生成されたコード、またはコンテンツ ファイルをレンダリングするビューを埋め込むことができます。 [Windows Community Toolkit](/windows/uwpcommunitytoolkit/) の最新リリースで WebView コントロールを参照できます。

Windows Community Toolkit の今後のリリースで WebView のような他のコントロールを確認してください。 詳細については、[WPF および Windows フォーム アプリケーションでの UWP コントロールのホスト](/windows/apps/desktop/modernize/xaml-islands)に関するページを参照してください。

### <a name="gaze-input-and-interactions"></a>視線入力と操作

[ユーザーの視線、注意、および場所とユーザーの目の動きに基づくプレゼンスを追跡します。](../design/input/gaze-interactions.md) UWP アプリの使用と操作のためのこの強力な新しい方法は、特に支援技術として有用です。 また、視線入力によって、ゲーム (ターゲットの取得や追跡など) と、従来の入力デバイス (キーボード、マウス、タッチ) を利用できない他の対話型シナリオの両方にとって魅力的な機会が提供されます。

### <a name="msix-packaging-format"></a>MSIX パッケージ形式

Microsoft Build 2018 カンファレンスで発表された MSIX は、Win32、Windows フォーム、WPF、UWP を含むすべての Windows アプリケーションに適用される、新しいコンテナー化パッケージの形式です。 この新しい形式には、UWP の優れた機能が継承されています。

* 堅牢なインストールと更新。 
* 柔軟な機能システムを持つマネージド セキュリティ モデル。
* Microsoft Store、エンタープライズ管理、多くのカスタム配布モデルのサポート。

これらのパッケージを作成するためのツールは、Visual Studio と Windows SDK の将来のリリースで利用可能になる予定です。

MSIX パッケージ形式は、Microsoft のパートナーが自身のツールとソリューションで MSIX エコシステムをより簡単にサポートできるようにするオープン ソース形式です。 MSIX パッケージ形式の詳細については、[MSIX SDK](https://github.com/Microsoft/msix-packaging) をご覧ください。 

![MSIX パッケージ作成の画像](images/msix.png)

### <a name="optional-packages-with-executable-code"></a>実行可能コードによるオプション パッケージ

アプリ内のオプション パッケージに実行可能な C# コードを含めることができるようになりました。 [Visual Studio を使用して、メイン アプリ パッケージをサポートするオプションのアドオン パッケージを構成する方法については、こちらをご覧ください。](/windows/msix/package/optional-packages)

### <a name="page-transitions"></a>ページ切り替え効果

[ページ切り替え効果](../design/motion/page-transitions.md)により、ユーザーはアプリ内のページ間を移動します。 ナビゲーション階層のどこにいるのかをユーザーが理解し、ページ間の関係についてフィードバックを提供するのに役立ちます。

### <a name="project-rome"></a>Project Rome

Project Rome チームは、iOS と Android の SDK を徹底的に見直しました。ユーザー アクティビティなどの新機能を追加し、さまざまな SDK 間で一貫したプログラミング エクスペリエンスを提供するために、コードの大部分をリファクタリングしました。 [まったく新しい API リファレンスと使い方のドキュメント](/windows/project-rome/)が、Build 2018 開発者カンファレンス中に公開されます。

### <a name="sets"></a>Sets

Sets 機能は、Windows Insider のプレビュー ビルドで利用できます。 Sets 機能を使用すると、他のアプリと共有される可能性があるウィンドウにアプリが描画され、タイトル バーには各アプリの専用のタブが表示されます。 

## <a name="developer-guidance"></a>開発者ガイド

### <a name="get-started"></a>開始

「はじめに」の内容に新しい学習トラックを追加して更新しました。 これらの新しいトピックは、新たな Windows 10 開発者に、実現できる一般的なタスクに関する情報を提供することを目的としています。 これらはチュートリアルではなく、コンパクトな解説を提供するものでもありません。代わりに、既存のドキュメントがある場所とその使用方法を示しています。 改善された「[コーディングの開始](../get-started/create-uwp-apps.md)」ページを確認するか、個々の学習トラックをそれぞれ確認してください。

* [フォームの作成](../get-started/construct-form-learning-track.md)
* [一覧での顧客の表示](../get-started/display-customers-in-list-learning-track.md)
* [設定の保存と読み込み](../get-started/settings-learning-track.md)
* [ファイルの操作](../get-started/fileio-learning-track.md)

![作業の開始アイコン](../get-started/images/build-your-app.png)

### <a name="advertising-performance-report"></a>広告パフォーマンス レポート

パートナー センターの [[広告パフォーマンス] レポート](../publish/advertising-performance-report.md)に、視認性のメトリックが含まれるようになりました。 また、「[広告ユニットの視認性の最適化](../monetize/optimize-ad-unit-viewability.md)」の記事を追加し、広告の視認性を最適化するための推奨事項を提供しています。

### <a name="targeted-push-notifications"></a>ターゲット プッシュ通知

パートナーセンターの[通知](../publish/send-push-notifications-to-your-apps-customers.md)ページに、すべての通知に関する追加の分析データが、グラフ ビューとワールド マップ ビューで表示されるようになりました。

## <a name="videos"></a>ビデオ

### <a name="cwinrt"></a>C++/WinRT

C++/WinRT は、Windows ランタイム API を作成および使用するための新しい方法です。 ヘッダー ファイルに単独で実装され、最新のアプリ機能への最適なアクセスを提供するように設計されています。 [ビデオを視聴して](https://www.youtube.com/watch?v=TLSul1XxppA&feature=youtu.be)そのしくみを確認したうえで、詳細については[開発者向けドキュメントをご覧ください](../cpp-and-winrt-apis/index.md)。

### <a name="multi-instance-uwp-apps"></a>マルチインスタンスの UWP アプリ

Windows で UWP アプリの複数のインスタンスを実行できるようになりました。各インスタンスは分離された個別のプロセスで実行されます。 この機能をサポートする新しいアプリの作成方法を[ビデオを見て](https://www.youtube.com/watch?v=clnnf4cigd0&feature=youtu.be)確認し、この機能を使用する方法と理由の詳細については、[開発者向けドキュメントをご覧ください](../launch-resume/multi-instance-uwp.md)。

## <a name="samples"></a>サンプル

### <a name="customer-database-tutorial"></a>顧客データベース チュートリアル

このチュートリアルでは、顧客の一覧を管理するための基本的な UWP アプリを作成し、エンタープライズ開発の概念と実践を紹介しています。 UI 要素の実装とローカル SQLite データベースに対する操作の追加を順を追って説明し、さらに希望する場合のために、リモート REST データベースへの接続の大まかなガイダンスを示します。 [チュートリアルはこちらからご覧ください](../enterprise/customer-database-tutorial.md)