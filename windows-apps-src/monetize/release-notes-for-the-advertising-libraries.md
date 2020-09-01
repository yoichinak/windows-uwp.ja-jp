---
ms.assetid: ca92bed1-ad9e-4947-ad91-87d12de727c0
description: Windows 10、Windows 8.1、Windows Phone 8.1、Windows Phone 8 の XAML および JavaScript/HTML アプリをサポートする Microsoft 広告ライブラリのリリースノートを表示します。
title: Advertising ライブラリのリリース ノート
ms.date: 02/18/2020
ms.topic: article
keywords: Windows 10, UWP, 広告, 宣伝, リリース ノート
ms.localizationpriority: medium
ms.openlocfilehash: 8faf352040b9d7bdc9fc8bc79804d5903573d41d
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174966"
---
# <a name="release-notes-for-the-advertising-libraries"></a>Advertising ライブラリのリリース ノート

>[!WARNING]
> 2020年6月1日から、Microsoft Ad 収益化 platform for Windows UWP アプリがシャットダウンされます。 [詳細を表示](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

このセクションでは、Microsoft Advertising ライブラリの現在のリリースのリリース ノートを示します。 これらのライブラリは、Windows 10、Windows 8.1、Windows Phone 8.1、Windows Phone 8 用の XAML アプリと JavaScript/HTML アプリをサポートします。

## <a name="installation"></a>インストール


Microsoft Advertising ライブラリは、[Microsoft Advertising SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK) の一部として利用できるようになりました。 SDK のインストールについて詳しくは、「[Microsoft Advertising SDK のインストール](install-the-microsoft-advertising-libraries.md)」をご覧ください。

## <a name="uninstall-previous-versions"></a>以前のバージョンのアンインストール

最新の Microsoft Advertising SDK をインストールする前に、SDK の以前のインスタンスをすべてアンインストールすることを強くお勧めします。 詳しくは、「[Microsoft Advertising SDK のインストール](install-the-microsoft-advertising-libraries.md)」をご覧ください。

## <a name="target-architecture-specific-build-outputs"></a>ターゲット アーキテクチャ固有のビルドの出力

Microsoft Advertising ライブラリを使う場合、プロジェクトで **"Any CPU"** をターゲットにすることはできません。 プロジェクトでのターゲットを **Any CPU** プラットフォームに設定している場合は、Microsoft Advertising ライブラリに参照を追加した後で警告メッセージが表示される可能性があります。 この警告を解決するには、アーキテクチャ固有のビルド出力 (たとえば、**x86**) を使用するようにプロジェクトを更新します。 詳細については、「 [既知の問題](known-issues-for-the-advertising-libraries.md)」を参照してください。

## <a name="c-support"></a>C++ のサポート

Microsoft Advertising ライブラリ (**AdControl** クラスと **InterstitialAd** クラスが含まれる) は、Windows ランタイムの相互運用性 (*相互運用機能*) を使用して C++ および DirectX で記述されたアプリをサポートします。 XAML と C++ を使ったプログラミングの詳細情報と例については、「[型システム](/cpp/cppcx/type-system-c-cx)」をご覧ください。

## <a name="no-toolbox-control"></a>ツールボックス コントロールはない

[Microsoft Advertising SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK) に含まれる Microsoft Advertising ライブラリの現在のリリースには、**AdControl** や **InterstitialAd** をアプリのデザイン サーフェイスにドラッグするためのツールボックス コントロールはありません。 マークアップとコードでこれらのコントロールを追加する方法については、[開発者向けのチュートリアル](developer-walkthroughs.md)をご覧ください。

## <a name="latitude-and-longitude-properties-no-longer-available"></a>使用できなくなった緯度と経度のプロパティ

**AdControl**クラスに、UWP アプリ用の **Latitude** プロパティと **Longitude** プロパティは含まれなくなりました。 代わりに、広告コントロールに組み込まれているコードが、アプリに代わってこれらの値を検出し、広告サーバーに送信します。


 

 