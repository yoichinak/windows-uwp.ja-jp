---
ms.assetid: 1abcbb13-80f0-4bf1-a812-649ee8bd1915
title: アプリのパッケージ化
description: このセクションには、ユニバーサル Windows プラットフォーム (UWP) アプリのパッケージ化に関する記事または記事へのリンクが記載されています。
ms.date: 07/22/2019
ms.topic: article
keywords: Windows 10, UWP, パッケージ化
ms.localizationpriority: medium
ms.openlocfilehash: d067511e4ceb5aaf072aabf8f74ccf186a7787dc
ms.sourcegitcommit: 350d6e6ba36800df582f9715c8d21574a952aef1
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/31/2019
ms.locfileid: "68682673"
---
# <a name="packaging-apps"></a>アプリのパッケージ化

このセクションには、デプロイとインストールに使用するための、MSIX および AppX アプリ パッケージでのユニバーサル Windows プラットフォーム (UWP) アプリのパッケージ化に関する記事が含まれているか、リンクされています。 これらのリンクの一部は、[MSIX ドキュメント](https://docs.microsoft.com/windows/msix/)の関連記事にアクセスします。

> [!NOTE]
> Windows 10 の UWP アプリの元のアプリ パッケージ形式は、AppX という名前でした。 Windows 10 バージョン 1809 以降では、このパッケージ形式は MSIX という名前に変更され、.NET や C++/Win32 デスクトップ アプリを含むすべての種類の Windows アプリをサポートするように拡張されました。 MSIX のサポートは、以前のバージョンの Windows にも拡張されています。 詳細については、[MSIX のドキュメント](https://docs.microsoft.com/windows/msix/)を参照してください。

| トピック | 説明 |
|-------|-------------|
| [Visual Studio で UWP アプリをパッケージ化する](/windows/msix/package/packaging-uwp-apps) | ユニバーサル Windows プラットフォーム (UWP) アプリを配布または販売するには、そのアプリのアプリ パッケージを作成する必要があります。 |
| [手動でのアプリのパッケージ化](/windows/msix/package/manual-packaging-root) | アプリ パッケージを作成して署名するときに、Visual Studio を使ってアプリを開発していない場合は、手動でのアプリのパッケージ化ツールを使用する必要があります。 |
| [アプリ パッケージのアーキテクチャ](/windows/msix/package/device-architecture) | アプリ パッケージを構築するときにどのプロセッサ アーキテクチャを使用するべきかについて説明します。 |
| [UWP アプリ ストリーミング インストール](/windows/msix/package/streaming-install) | アプリ ストリーミング インストールでは、Microsoft Store からアプリのどの部分を最初にダウンロードするかを指定できます。 アプリの基本的なファイルを先にダウンロードすると、残りの部分のダウンロードをバックグラウンドで完了している間に、ユーザーはアプリを起動して操作できます。 |
| [オプション パッケージと関連セットの作成](/windows/msix/package/optional-packages) | オプション パッケージには、メイン パッケージに統合できるコンテンツが格納されます。 オプション パッケージは、ダウンロード可能なコンテンツ (DLC) 用や、サイズ制約に対応して大規模アプリを分割する場合、元のアプリから分離して追加コンテンツを出荷する場合に便利です。 |
| [実行可能コードによるオプション パッケージ](/windows/msix/package/optional-packages-with-executable-code) | Visual Studio を使用して、実行可能コードでオプション パッケージを作成する方法について説明します。 |
| [アプリ インストーラーで Windows 10 アプリをインストールする](/windows/msix/app-installer/app-installer-root) | アプリ インストーラーを使用すると、アプリ パッケージをダブルクリックして Windows 10 アプリをインストールできます。 |
| [WinAppDeployCmd.exe ツールを使ったアプリのインストール](install-universal-windows-apps-with-the-winappdeploycmd-tool.md) | Windows アプリケーションの展開ツール (WinAppDeployCmd.exe) は、Windows 10 コンピューターから Windows 10 Mobile デバイスに UWP アプリを展開するために使うことができるコマンド ライン ツールです。 このツールを使うと、Windows 10 Mobile デバイスが USB で接続されているか同じサブネットにあれば、アプリ パッケージを展開できます。Microsoft Visual Studio やそのアプリ用のソリューションは不要です。 この記事では、このツールを使って UWP アプリをインストールする方法について説明します。 |
| [UWP アプリの自動ビルドを設定する](auto-build-package-uwp-apps.md) | このトピックでは、自動ビルド プロセスの一環としてアプリをパッケージ化する場合に、Visual Studio Team Services (VSTS) を使用して実行する方法を説明します。 |
| [アプリ機能の宣言](app-capability-declarations.md) | 特定の API、画像や音楽などのリソース、カメラやマイクなどデバイスにアクセスするには、機能をアプリの[パッケージ マニフェスト](https://docs.microsoft.com/uwp/schemas/appxpackage/appx-package-manifest)で宣言する必要があります。 |
| [パッケージの更新プログラムを Microsoft Store からダウンロードしてインストールする](self-install-package-updates.md) | UWP アプリでは、プログラムによてパッケージの更新を確認して、インストールできます。 また、パートナー センターで必須としてマークされているパッケージを照会し、必須の更新がインストールされるまで機能を無効にすることもできます。  |
