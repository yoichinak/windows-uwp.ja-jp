---
description: 現在の Mac コンピューターを使用して、Windows 用アプリを開発します。
title: Windows 10 を使用するための Mac のセットアップ
ms.assetid: 6D520610-5DE0-476E-A792-AA57E002D309
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 55165e0369c6bda64c19dc384c5c2addf224b8ba
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259112"
---
# <a name="setting-up-your-mac-with-windows-10"></a>Windows 10 を使用するための Mac のセットアップ


現在の Mac コンピューターを使用して、Windows 用アプリを開発します。

## <a name="run-windows-on-your-mac-and-use-visual-studio"></a>Mac で Windows を実行し、Visual Studio を使う

ユニバーサル Windows アプリの開発を始める準備は整っているのに、PC が手元にない、そういう方でも 大丈夫です。Mac を使うことができます。 With popular third-party solutions like Apple Boot Camp, Oracle VirtualBox, VMware Fusion, and Parallels Desktop, you can install Windows 10 and Microsoft Visual Studio on your Apple computer.

**Note**  You will need a Windows 10 bootable image on disk or USB flash drive. MSDN サブスクライバーである場合は、MSDN サブスクライバー ダウンロード センターからインストール イメージをダウンロードできます。 If you aren't a subscriber, the installer can be purchased from the [Microsoft Store](https://www.microsoft.com/store/apps). [この場所](https://www.microsoft.com/software-download/windows10)からダウンロードすることもできます。これは、Windows を既に実行中でありアップグレードする場合に便利です。

Once you have Windows running, you can then install the latest release of Visual Studio from [Developer downloads for Windows 10](https://developer.microsoft.com/en-us/windows/downloads) and start writing apps!

**Note**  If you plan to use the Visual Studio device emulators, you **must** install a 64-bit (x64) version of Windows 10 Pro or better. ただし、以前の Mac では 64 ビット版の Windows を実行できない場合があります。 この [Apple サポート ページ](https://support.apple.com/kb/HT5634)で、お使いのハードウェアに互換性があるかどうかを確認してください。

## <a name="apple-boot-camp"></a>Apple Boot Camp

The Boot Camp Assistant app is pre-installed on every recent Mac, and launching it will walk you through the process of installing Windows 10. 必要なのもは、上記のソースからダウンロードした Windows のコピーと 30 Gb 以上の空きディスク領域だけです。 インストールしたら、Mac OSX と Windows 10 のどちらを起動するかを選択できます。 詳しくは、Apple の [Boot Camp に関するページ](https://support.apple.com/HT201468)をご覧ください。

## <a name="parallels-desktop"></a>Parallels Desktop

Parallels Desktop 11 を使用すると、Visual Studio と Cortana など、Windows アプリと既存の Mac アプリケーションを並べて実行できます。 強化されたデバッグや Docker と Jenkins のサポートなど、開発者向けの追加機能を含むプロ バージョンを利用できます。 詳しい情報、および無料試用版については、[Parallels Desktop に関するページ](https://www.parallels.com/download/desktop/)をご覧ください。

## <a name="vmware-fusion"></a>VMWare Fusion

VMWare の Fusion 8 を使うと、Mac デスクトップで Visual Studio を実行できます。 vSphere サポートなど、いくつかの高度な機能を開発者に提供するプロ バージョンを利用できます。 詳しい情報、および無料試用版については、「[VMware Fusion](http://www.vmware.com/products/fusion/)」をご覧ください。

## <a name="oracle-virtualbox"></a>Oracle VirtualBox

VirtualBox は、お使いのコンピューター上で仮想マシンを実行するための無料アプリケーションであり、Mac での Windows の実行をサポートしています。 余分な機能を省いたバージョンですが、価格は魅力的です。 詳しくは、「[VirtualBox](https://www.virtualbox.org/wiki/Downloads)」をご覧ください。

