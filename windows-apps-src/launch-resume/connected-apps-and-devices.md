---
author: PatrickFarley
title: 接続されるアプリやデバイス ("Rome" プロジェクト)
description: このセクションでは、Remote Systems プラットフォームを使って、リモート デバイスの検出、リモート デバイスでのアプリの起動、リモート デバイス上のアプリ サービスとの通信を行う方法について説明します。
ms.author: pafarley
ms.date: 06/08/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10、uwp、接続されているデバイス、リモート システム、ローマ、プロジェクト ローマ
ms.assetid: 7f39d080-1fff-478c-8c51-526472c1326a
ms.localizationpriority: medium
ms.openlocfilehash: d3efb7e094ce1464028dadaa14c6f0bfb3f3b214
ms.sourcegitcommit: 753dfcd0f9fdfc963579dd0b217b445c4b110a18
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/27/2018
ms.locfileid: "2860383"
---
# <a name="connected-apps-and-devices-project-rome"></a>接続されるアプリやデバイス ("Rome" プロジェクト)

このセクションでは、Rome プロジェクトを使ってデバイスとプラットフォーム間でアプリを接続する方法について説明します。 リモート デバイスの検出、リモート デバイス上でのアプリの起動、リモート デバイス上のアプリ サービスとの通信を行う方法について説明します。

ほとんどのユーザーは複数のデバイスを持っており、あるデバイスでアクティビティを始めてデバイスで終えることがよくあります。 これに対応するため、アプリはデバイスとプラットフォームにまたがる必要があります。

Windows 10 バージョン 1607 に導入された[リモート システム API](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems) を使うと、ユーザーがあるデバイスでタスクを開始して別のデバイスで終えることができるアプリを記述できます。 タスクは中央のフォーカスに残り、ユーザーは最も便利なデバイスで作業を行うことができます。 たとえば、ユーザーが車に乗りながら電話でラジオを聴きますが、帰宅したらホーム ステレオ システムに搭載した Xbox One に再生を転送することがあります。

コンパニオン デバイスに "Rome" プロジェクトを使う (つまり、リモート制御シナリオ) こともできます。 アプリ サービス メッセージング API を使って 2 つのデバイス間にアプリ チャネルを作り、カスタム メッセージを送受信します。 たとえば、テレビの再生を制御する電話用アプリや、別のアプリで視聴しているテレビ番組のキャラクターに関する情報を表示するコンパニオン アプリを記述することができます。  

デバイスは、Bluetooth やワイヤレスを経由して近くで接続したり、クラウドを通じてリモートで接続したりすることができます。それらのデバイスは、デバイスを使っているユーザーの Microsoft アカウント (MSA) によってリンクされます。

リモート システムを検出する方法、リモート システムでアプリを起動する方法、アプリ サービスを使って 2 つのシステム上で実行しているアプリ間でメッセージを送信する方法の例については、[リモート システム UWP のサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/RemoteSystems )を参照してください。

クロス プラットフォーム統合のリソースを含む、Project Rome 全般の詳細については、[aka.ms/project-rome](https://aka.ms/project-rome) を参照してください。

| トピック | 説明 |
|-------|-------------|
| [リモート デバイスでのアプリの起動](launch-a-remote-app.md) | リモート デバイスでアプリを起動する方法について説明します。 このトピックでは、最も単純な使用事例と準備段階のセットアップについて説明します。  |
| [リモート デバイスの検出](discover-remote-devices.md)  | 接続できるデバイスを検出する方法について説明します。 |
| [リモート アプリ サービスとの通信](communicate-with-a-remote-app-service.md) | リモート デバイスのアプリを操作する方法について説明します。 |
| [リモート セッションでデバイスを接続する](remote-sessions.md) | リモート セッションで複数のデバイスを結合することにより、これらのデバイス間で共有エクスペリエンスを作成します。 |
| [デバイス間でもユーザーのアクティビティを継続する](useractivities.md)| ユーザーが何でアプリでは、複数のデバイスにわたってを再開するのに役立ちます。|
| [ユーザーのアクティビティのベスト プラクティス](useractivities-best-practices.md)| 作成して、ユーザーのアクティビティの更新についての推奨事項をについて説明します。|