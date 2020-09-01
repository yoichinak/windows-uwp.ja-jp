---
title: はじめに
description: このクイックスタートガイドに従って、Windows または Xbox のゲーム開発をすぐに開始する方法について説明します。
ms.assetid: 40490837-6c7f-4f82-96b5-14f6858982b3
ms.date: 01/25/2018
ms.topic: article
keywords: windows 10、uwp、ゲーム、はじめに
localizationpriority: medium
ms.openlocfilehash: 7d3e50f5d32cd8c6a497ff3de67005972d3a0f7f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175316"
---
# <a name="getting-started"></a>はじめに

この記事は、Windows または Xbox でゲームを開発するための入門ガイドです。 

次に、必要な情報の検索に役立ついくつかの質問を示します。
* ゲーム開発者が経験していて、すべての情報を必要としていますか。 「 [Windows 10 ゲーム開発ガイド](e2e.md)」を参照してください。
* コーディングについてはまったく新しいことがありますか。 [コードチュートリアルの Minecraft](https://code.org/minecraft)のような楽しいものがあります。
* 遊びたいゲームを探しているだけです。 [Microsoft Store](https://www.microsoft.com/store)を確認します。
* Windows または Xbox の優れたゲーム開発を始める準備はできましたか?  お客様は正しい場所にあります。

## <a name="quick-start-guide"></a>クイック スタート ガイド

ゲームをすぐに開発するための手順です。

### <a name="step-1-get-the-software-and-tools"></a>手順 1: ソフトウェアとツールを入手する

デバイスに Windows 10 がインストールされており、最新の更新プログラムがインストールされていることを確認します。

Visual Studio のような適切な IDE をインストールします。 Visual Studio Community 2017 は無料でダウンロードできます。 詳細については、「 [Visual Studio のダウンロード](https://visualstudio.microsoft.com/downloads/)」を参照してください。

ゲームエンジンとその他のミドルウェアの使用を計画している場合は、「 [Windows 10 ゲーム開発ガイド](e2e.md)」の「[ブリッジ、ゲームエンジン、ミドルウェア](e2e.md#bridges-game-engines-and-middleware)」セクションを参照してください。 特定のゲームエンジンを使用して Windows および Xbox ゲームを開発する方法の詳細については、ゲームエンジンのドキュメントを参照してください。

### <a name="step-2-prepare-your-hardware-for-development"></a>手順 2: 開発用にハードウェアを準備する

初めて開発を行っている場合は、デバイスで開発者モードを有効にする必要があります。 詳細については、「[デバイスを開発用に有効にする](../get-started/enable-your-device-for-development.md)」を参照してください。

リテール Xbox コンソールを使用して Xbox ゲームの開発を計画しているユーザーについては、開発者モードをアクティブ化して有効にする必要もあります。 詳細については、「 [Xbox One Developer Mode activation](../xbox-apps/devkit-activation.md) 」と「 [XBOX での UWP アプリ開発の](../xbox-apps/getting-started.md)概要」を参照してください。 

> [!Note]
> Xbox コンソールで開発者モードを有効にするには、 [パートナーセンター](https://partner.microsoft.com/dashboard)  アカウントにサインアップする必要があります。 パートナーセンターアカウントへのサインアップの詳細については、以下の [手順 5](#step-5-sign-up-for-a-partner-center-account) . を参照してください。

### <a name="step-3-run-a-sample-and-see-how-it-works"></a>手順 3: サンプルを実行し、その動作を確認する

UWP DirectX 開発を開始するには、「 [DirectX を使用した簡単な uwp ゲームの作成](tutorial--create-your-first-uwp-directx-game.md)」を参照してください。 バッファーの内容などの DirectX の概念を単に読んで理解したい場合は、「 [Direct3D graphics の概念](../graphics-concepts/index.md)」を参照してください。

その他のサンプルについては、「 [Game samples](e2e.md#game-samples)」を参照してください。

### <a name="step-4-consider-joining-a-program"></a>手順 4: プログラムに参加することを検討する

Xbox ゲームを開発する場合、またはゲームで Xbox Live 機能を使用する場合は、 [Xbox live](https://developer.microsoft.com/games/xbox/xboxlive/creator) 作成者プログラムまたはプログラムのいずれかに参加し [ID@Xbox](https://www.xbox.com/Developers/id) ます。 

各プログラムで使用できる Xbox Live 機能の詳細については、「 [Feature Table](/gaming/xbox-live/developer-program-overview.md#feature-table)」を参照してください。 詳細については、「 [開発者プログラム](e2e.md#developer-programs)」を参照してください。

> [!Note]
> Xbox Live クリエータープログラムは、すべての開発者が利用できます。 **だれ** でも Xbox ゲームを発行できます。 Xbox Live 作成者プログラムのタイトル部分を作成するには、パートナーセンターからこのオプションを有効にするだけです。 パートナーセンターアカウントへのサインアップの詳細については、以下の [手順 5](#step-5-sign-up-for-a-partner-center-account) . を参照してください。

### <a name="step-5-sign-up-for-a-partner-center-account"></a>手順 5: パートナーセンターアカウントにサインアップする

パートナーセンターアカウントを使用すると、 [パートナーセンター](https://partner.microsoft.com/dashboard)にアクセスして、Windows デバイス用のすべてのアプリとゲームを1か所で管理し、送信することができます。

Windows ゲーム開発では、パートナーセンターへのアクセスが必要になるまで、またはゲームで Xbox Live 機能を使用する場合は、待機することを選択できます。

Xbox ゲーム開発では、開発用のリテール Xbox を設定するために必要なため、パートナーセンターアカウントにサインアップする必要があります。 詳細については、 [手順 2](#step-2-prepare-your-hardware-for-development) . を参照してください。

詳細については、「[Publish Windows apps and games](../publish/index.md)」(Windows のアプリとゲームを公開する) を参照してください。

## <a name="useful-links"></a>便利なリンク

* [Windows 10 ゲーム開発ガイド](e2e.md)
* [UWP アプリとは](../get-started/universal-application-platform-guide.md)
* [UWP ゲーム用のクラウド サービスの使用](cloud-for-games.md)
* [ゲームをアクセシビリティ対応にする](accessibility-for-games.md)