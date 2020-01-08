---
description: 現在の Mac コンピューターを使用して、Windows 用アプリを開発します。
title: Windows 10 を使用するための Mac のセットアップ
ms.assetid: 6D520610-5DE0-476E-A792-AA57E002D309
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: b8ff6d6b3b15992f0598bfb371e6bfb1023d6dc1
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/06/2020
ms.locfileid: "75684998"
---
# <a name="setting-up-your-mac-with-windows-10"></a>Windows 10 を使用するための Mac のセットアップ


現在の Mac コンピューターを使用して、Windows 用アプリを開発します。

## <a name="run-windows-on-your-mac-and-use-visual-studio"></a>Mac で Windows を実行し、Visual Studio を使う

ユニバーサル Windows アプリの開発を始める準備は整っているのに、PC が手元にない、そういう方でも 大丈夫です。Mac を使うことができます。 Apple Boot Camp、Oracle VirtualBox、VMware Fusion、および like Desktop などの一般的なサードパーティ製ソリューションを使用して、Windows 10 と Microsoft Visual Studio を Apple コンピューターにインストールできます。

**注**  ディスクまたは USB フラッシュドライブ上に Windows 10 の起動可能なイメージが必要です。 MSDN サブスクライバーである場合は、MSDN サブスクライバー ダウンロード センターからインストール イメージをダウンロードできます。 サブスクライバーでない場合は、 [Microsoft Store](https://www.microsoft.com/store/apps)からインストーラーを購入できます。 [この場所](https://www.microsoft.com/software-download/windows10)からダウンロードすることもできます。これは、Windows を既に実行中でありアップグレードする場合に便利です。

Windows を実行した後、 [windows 10 用の開発者向けダウンロード](https://developer.microsoft.com/windows/downloads)から Visual Studio の最新リリースをインストールし、アプリの作成を開始できます。

**注**  Visual Studio デバイスエミュレーターを使用する場合は、64ビット (x64) バージョンの Windows 10 Pro 以上をインストールする**必要があり**ます。 ただし、以前の Mac では 64 ビット版の Windows を実行できない場合があります。 この [Apple サポート ページ](https://support.apple.com/kb/HT5634)で、お使いのハードウェアに互換性があるかどうかを確認してください。

## <a name="apple-boot-camp"></a>Apple Boot Camp

ブートキャンプアシスタントアプリは、最近のすべての Mac にプレインストールされています。起動すると、Windows 10 をインストールする手順が表示されます。 必要なのもは、上記のソースからダウンロードした Windows のコピーと 30 Gb 以上の空きディスク領域だけです。 インストールしたら、Mac OSX と Windows 10 のどちらを起動するかを選択できます。 詳しくは、Apple の [Boot Camp に関するページ](https://support.apple.com/HT201468)をご覧ください。

## <a name="parallels-desktop"></a>Parallels Desktop

Parallels Desktop 11 を使用すると、Visual Studio と Cortana など、Windows アプリと既存の Mac アプリケーションを並べて実行できます。 強化されたデバッグや Docker と Jenkins のサポートなど、開発者向けの追加機能を含むプロ バージョンを利用できます。 詳しい情報、および無料試用版については、[Parallels Desktop に関するページ](https://www.parallels.com/download/desktop/)をご覧ください。

## <a name="vmware-fusion"></a>VMWare Fusion

VMWare の Fusion 8 を使うと、Mac デスクトップで Visual Studio を実行できます。 vSphere サポートなど、いくつかの高度な機能を開発者に提供するプロ バージョンを利用できます。 詳しい情報、および無料試用版については、「[VMware Fusion](http://www.vmware.com/products/fusion/)」をご覧ください。

## <a name="oracle-virtualbox"></a>Oracle VirtualBox

VirtualBox は、お使いのコンピューター上で仮想マシンを実行するための無料アプリケーションであり、Mac での Windows の実行をサポートしています。 余分な機能を省いたバージョンですが、価格は魅力的です。 詳しくは、「[VirtualBox](https://www.virtualbox.org/wiki/Downloads)」をご覧ください。

