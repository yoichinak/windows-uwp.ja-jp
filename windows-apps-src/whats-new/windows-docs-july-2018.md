---
title: Windows ドキュメントの最新情報、2018 年 7 月 - UWP アプリの開発
description: 2018 年 7 月版の Windows 10 開発者向けドキュメントには、新しい機能、ビデオ、サンプル、開発者向けガイダンスが追加されました。
keywords: 最新情報, 更新, 機能, 開発者向けガイダンス, Windows 10, 7 月
ms.date: 07/11/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 375cd903a0711a1493930567f557992c0bba46b7
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174346"
---
# <a name="whats-new-in-the-windows-developer-docs-in-july-2018"></a>Windows 開発者向けドキュメントの最新情報、2018 年 7 月

Windows 開発者向けドキュメントは、Windows プラットフォームで開発者に提供される新機能の情報を反映して継続的に更新されています。 次の機能概要、開発者向けガイダンス、ビデオ、およびサンプルは 7 月に公開されたものです。

Windows 10 の[ツールと SDK をインストール](https://developer.microsoft.com/windows/downloads#_blank)すると、[新しいユニバーサル Windows アプリを作成](../get-started/create-uwp-apps.md)したり、[Windows の既存のアプリ コード](../porting/index.md)がどのように使えるかを試したりすることができます。

## <a name="features"></a>機能

### <a name="progressive-web-apps-on-windows"></a>Windows のプログレッシブ Web アプリ

[プログレッシブ Web アプリ (PWA)](https://developer.microsoft.com/windows/pwa) は、サポートするプラットフォームおよびブラウザー エンジン上でネイティブ アプリのような機能 (ホーム画面からの起動、オフライン サポート、プッシュ通知など) を提供するように[プログレッシブに拡張された](https://www.wikipedia.org/wiki/Progressive_enhancement) Web アプリです。 Microsoft Edge (EdgeHTML) エンジンを搭載した Windows 10 では、PWA を [UWP アプリとしてブラウザー ウィンドウとは独立して実行できる](/microsoft-edge/progressive-web-apps/windows-features)という利点があります。

![動作中の PWA のイメージ](images/progressive-web-apps.jpg)

次の項目については、PWA ガイドを確認してください。

* [単純な Web アプリを PWA として構築する](/microsoft-edge/progressive-web-apps/get-started)
* [Windows ランタイムで PWA を強化する](/microsoft-edge/progressive-web-apps/windows-features)
* [PWA を Microsoft Store で公開する](/microsoft-edge/progressive-web-apps/microsoft-store)

### <a name="notepad"></a>メモ帳

Windows 10 Insider Preview ビルド 17713 で提供されている[メモ帳は、多数の新機能で更新されています](https://blogs.windows.com/windowsexperience/2018/07/11/announcing-windows-10-insider-preview-build-17713/)。 ズーム、折り返し検索/置換、および Unix/Linux と Mac の行の末尾 (それぞれ LF と CR) が、[Windows Insider](https://insider.windows.com/) でサポートされるようになりました。 

## <a name="developer-guidance"></a>開発者ガイド

### <a name="design-landing-page"></a>設計ランディング ページ

[更新された設計ランディング ページ](https://developer.microsoft.com/windows/apps/design)で、一目でわかる UWP デザイン領域の概要情報と、Fluent Design への最新の追加機能に関する情報を確認してください。

### <a name="design-toolkits"></a>設計ツールキット

Adobe XD および Adobe Illustrator ツールキットが更新されて新機能が追加されました。 これらの設計ツールキットは、UWP アプリを設計するためのコントロールおよびレイアウト テンプレートを提供します。 [これらについてはこちらをご覧ください](../design/downloads/index.md)。

### <a name="webvr"></a>WebVR

[WebVR のドキュメント](/microsoft-edge/webvr/)にいくつかの新しいトピックが追加されました。

* [WebVR とは](/microsoft-edge/webvr/what-is-webvr): WebVR とは何か、それを使用する理由、およびその開発の始め方について説明します。

* [プログレッシブ Web アプリでの WebVR](/microsoft-edge/webvr/webvr-in-pwas):プログレッシブ Web App (PWA) に WebVR を追加する方法について説明します。

* [WebView の WebVR](/microsoft-edge/webvr/webvr-in-webview):Windows 10 のアプリケーションで WebView コントロールに WebVR を追加する方法について説明します。

* [WebVR のデモ](/microsoft-edge/webvr/demos):Microsoft Edge と Windows Mixed Reality イマーシブ ヘッドセットを使用して、WebVR のいくつかのデモをご覧ください。

また、既存のページが一部更新されています。

* 目次が次の 4 つの項目で構成されるように改良されました: **基本事項**、**開発**、**リソース**、**デモ**。

* [WebVR 開発者向けガイド (ランディング ページ)](/microsoft-edge/webvr/):外観が更新されて、画像とアイコンが大きくなり、新しいデモが追加されました。

* [Microsoft Edge で WebVR を使用する](/microsoft-edge/webvr/webvr-with-edge):Windows 10 April 2018 Update に関する情報が追加されました。

## <a name="videos"></a>ビデオ

### <a name="get-started-for-devs-create-and-customize-a-form-on-windows-10"></a>開発者向けスタートアップ ガイド:Windows 10 でフォームを作成してカスタマイズする

Windows 開発者向けの[スタートアップ ガイド](../get-started/index.md)では、基本的なアプリ開発タスクを実際に体験できるようになりました。 このビデオは、これらのトピックの 1 つを順を追って説明するもので、アプリでのフォーム UI 作成の基本について扱います。 [ビデオを見て](https://www.youtube.com/watch?v=AgngKzq4hKI&feature=youtu.be)動作中のコードを理解したうえで、[トピックを自分で確認してください。](../get-started/construct-form-learning-track.md)

### <a name="enhance-your-bot-with-project-personality-chat"></a>Project Personality Chat でボットを強化する

Project Personality Chat により、カスタマイズ可能なペルソナをチャット ボットに追加することができます。 Microsoft Bot Framework SDK を統合することで、より会話的な方法で顧客と対話するための複数のスモールトーク機能を追加できます。 [ビデオを見て](https://www.youtube.com/watch?v=5C_uD8g2QKg&feature=youtu.be)これを実装する方法を理解したうえで、[対話的なデモを通して](https://www.microsoft.com/research/project/personality-chat/)実際に体験してみてください。

### <a name="one-dev-question"></a>One Dev Question

One Dev Question ビデオ シリーズでは、ベテランの Microsoft 開発者が Windows 開発、チーム カルチャ、および歴史に関する一連の質問に答えています。 最新の質問に対してお答えした内容を以下に示します。

Raymond Chen:

* [Microsoft に応募した理由](https://www.youtube.com/watch?v=oL8ymamkEMU&feature=youtu.be)

Larry Osterman:

* [開発者が既定のオーディオ デバイスを変更できない理由](https://www.youtube.com/watch?v=6aNUoVfbnmg&feature=youtu.be)
* [多くの UWP 関数が非同期である理由](https://www.youtube.com/watch?v=5M724QIy1Mk&feature=youtu.be)

## <a name="samples"></a>サンプル

### <a name="photo-editor-cwinrt"></a>フォト エディター C++/WinRT

フォト エディター サンプル アプリは、[C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) 言語プロジェクションでの開発を紹介します。 このアプリを使用すると、**画像**ライブラリから写真を取得し、選択した画像を関連する写真効果を使って編集できます。 [サンプルはこちらで複製またはダウンロードできます。](https://github.com/Microsoft/Windows-appsample-photo-editor)

![動作中のサンプルの例](images/photo-editor-banner.png)