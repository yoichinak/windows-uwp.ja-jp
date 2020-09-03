---
title: Windows ドキュメントの最新情報、2019 年 1 月 - UWP アプリの開発
description: 2019 年 1 月版の Windows 10 開発者向けドキュメントには、新しい機能、ビデオ、開発者向けガイダンスが追加されました
keywords: 最新情報, 更新, 機能, 開発者向けガイダンス, Windows 10, 1 月
ms.date: 01/17/2019
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: f1811e3b99077a3be676c57dc3420e88658f1d73
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174366"
---
# <a name="whats-new-in-the-windows-developer-docs-in-january-2019"></a>Windows 開発者向けドキュメントの最新情報、2019 年 1 月

Windows 開発者向けドキュメントは、Windows プラットフォームで開発者に提供される新機能の情報を反映して継続的に更新されています。 次の機能概要、開発者向けガイダンス、およびビデオは 1 月に利用可能になりました。

Windows 10 の[ツールと SDK をインストール](https://developer.microsoft.com/windows/downloads#_blank)すると、[新しいユニバーサル Windows アプリを作成](../get-started/create-uwp-apps.md)したり、[Windows の既存のアプリ コード](../porting/index.md)がどのように使えるかを試したりすることができます。

## <a name="features"></a>機能

### <a name="windows-development-on-microsoft-learn"></a>Microsoft Learn での Windows 開発

Microsoft Learn は、新しい実践的な学習とトレーニングの機会を Microsoft 開発者に提供します。 Windows アプリの開発方法の学習に関心がある場合、プラットフォーム、ツール、およびご自分の最初のいくつかのアプリの作成方法については、[新しいラーニング パス](/learn/paths/develop-windows10-apps/)をご覧ください。

![Windows 開発のラーニング パスの画像](images/windows-learn.png)

### <a name="direct-3d-12"></a>Direct 3D 12

[Direct3D 12 のレンダリング パス](/windows/desktop/direct3d12/direct3d-12-render-passes)では、レンダラーがタイルベースの遅延レンダリング (TBDR) などの手法に基づいている場合、レンダラーのパフォーマンスを向上させることができます。 この手法では、ご利用のレンダラーで GPU の効率が改善されるように支援します。そのために、ご利用のアプリケーションがリソース レンダリングの順序付けの要件とデータの依存関係をより適切に識別できるようにして、オフチップ メモリとの間のメモリ トラフィックを削減します。

### <a name="msix-modification-packages"></a>MSIX 修正パッケージ

Windows 10 バージョン 1809 では、[MSIX 修正パッケージ](/windows/msix/modification-package-1809-update)のサポートが改良されました。 修正パッケージにはレジストリベースのプラグインとそれに関連するカスタマイズを含めることができ、これにより MSIX を介して配置されたアプリケーションは仮想レジストリを使用して期待どおりに実行できるようにようになります。

![MSIX 修正パッケージの作成](images/msix-modification-package.png)

### <a name="open-source-of-wpf-windows-forms-and-winui"></a>WPF、Windows フォーム、および WinUI のオープン ソース

WPF、Windows フォーム、WinUI UX フレームワークを、GitHub 上でのオープンソース共同作成に利用できるようになりました。 詳細とリンクについては、[Windows アプリの作成](https://blogs.windows.com/buildingapps/2018/12/04/announcing-open-source-of-wpf-windows-forms-and-winui-at-microsoft-connect-2018/#OKZjJs1VVTrMMtkL.97)に関するブログ記事を参照してください。

### <a name="progressive-web-apps-for-xbox"></a>Xbox 用のプログレッシブ Web アプリ

[Xbox One 用のプログレッシブ Web アプリ](/microsoft-edge/progressive-web-apps/xbox-considerations)を使用すれば、既存のフレームワーク、CDN、サーバー バックエンドを引き続き使用しながら、Web アプリケーションを拡張してそれを Microsoft Store 経由で Xbox One アプリとして使用可能にすることができます。 ほとんどのパーツについては、Windows の場合と同じ方法で、ご自分の Xbox One 用 PWA をパッケージ化することができます。ただし、いくつかの重要な違いがあり、このガイドではそれを順番に説明します。

### <a name="windows-machine-learning"></a>Windows Machine Learning

Microsoft は [WinML API のランディング ページ](/windows/ai/api-reference)を再構築し、WinML カスタム演算子とネイティブ API に関する新しいドキュメントを追加しました。

「[PyTorch を使ってモデルをトレーニングする](/windows/ai/train-model-pytorch)」には、PyTorch フレームワークを使用してローカルまたはクラウドのいずれかでモデルをトレーニングする方法に関するガイダンスが提供されています。 さらに、このモデルを ONNX ファイルとしてダウンロードし、ご利用の WinML アプリケーション内で使用することができます。

![WinML グラフィック](images/winml-graphic.png)

## <a name="developer-guidance"></a>開発者ガイド

### <a name="choose-your-platform"></a>プラットフォームの選択

新しいデスクトップ アプリケーションの作成に関心がありますか? UWP、WPF、Windows フォームの各プラットフォームの詳細な説明と比較、および Win32 API の詳細については、改訂された[プラットフォームの選択](/windows/desktop/choose-your-technology)に関するページをご覧ください。

### <a name="faqs-on-win32-webview"></a>Win32 WebView に関してよく寄せられる質問

[よく寄せられる質問](/windows/communitytoolkit/controls/wpf-winforms/webview#frequently-asked-questions-faqs)には、デスクトップ アプリケーションで Microsoft Edge WebView を使用する場合の一般的な質問に対する回答に加えて、サンプルおよびその他のリソースへのリンクが提供されています。

### <a name="japanese-era-change"></a>日本の元号の変更

「[アプリケーションの新元号対応](../design/globalizing/japanese-era-change.md)」では、2019 年 5 月 1 日に実施された日本の元号の変更にご利用の Windows アプリケーションが確実に対応できるようにする方法を示しています。 [このページは日本語でもご覧いただけます](../design/globalizing/japanese-era-change.md)。

## <a name="videos"></a>ビデオ

### <a name="progressive-web-apps"></a>プログレッシブ Web アプリ

プログレッシブ Web アプリは、さまざまなブラウザーおよび各種 Windows 10 デバイスでネイティブ アプリのように機能する Web サイトです。 [ビデオをご覧になって](https://youtu.be/ugAewC3308Y)詳細を学習してから、[ドキュメントを参照](https://developer.microsoft.com/windows/pwa)して使用を開始してください。

### <a name="vs-code-series"></a>VS Code シリーズ

VSCode とは何か、その使用方法、および作成方法については、[Visual Studio Code に関する新しいビデオ シリーズ](https://www.youtube.com/playlist?list=PLlrxD0HtieHjQX77y-0sWH9IZBTmv1tTx)をご覧ください。

### <a name="one-dev-question"></a>One Dev Question

One Dev Question ビデオ シリーズでは、ベテランの Microsoft 開発者が Windows 開発、チーム カルチャー、および歴史に関する一連の質問に答えています。 最新の質問に対してお答えした内容を以下に示します。

Raymond Chen: 

* [Program Files と Program Files (x86) があるのはなぜですか?](https://youtu.be/qRb6otsHG5c)
* [Microsoft でのあなたの最初のインタビューはどのようなものでしたか?](https://youtu.be/MfzzbNp8kfw)

Larry Osterman: 

* [COM はなぜそれほど複雑なのですか?](https://youtu.be/-gkXAV-StVA)
* [Microsoft に対するあなたの最初のインタビューはどのようなものでしたか?](https://youtu.be/N7o9eJpFYco)