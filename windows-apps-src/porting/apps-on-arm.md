---
title: ARM 版 Windows 10
description: この記事では、ARM でエクスペリエンスとアプリがどのように実行されるかの概要、どのような制限事項があるか、詳しい情報を参照できる場所について説明します。
ms.date: 05/22/2020
ms.topic: article
keywords: windows 10 s, 常時接続, ARM, ARM64, x86 エミュレーション
ms.localizationpriority: medium
ms.openlocfilehash: 39ff5b2aa6c72feaeaea0a7a61100196c109257c
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162296"
---
# <a name="windows-10-on-arm"></a>ARM 版 Windows 10
もともと、Windows 10 (Windows 10 Mobile とは区別されます) は、x86 および x64 プロセッサを搭載した PC でのみ実行できました。 現在、Windows 10 desktop は、ARM64 プロセッサを搭載したコンピューターで実行できます。 ARM CPU アーキテクチャが持つ省電力の性質により、これらの PC のバッテリーが終日持つようになり、モバイル データ ネットワークがサポートされるようになります。 これらの PC にはアプリケーションの互換性が十分に備わっており、既存の x86 win32 アプリケーションを変更せずに実行できます。 詳細やデモについては、「 [常時接続されている PC の Channel 9 ビデオ](https://channel9.msdn.com/Events/Build/2017/P4171)」を参照してください。

ここでは、*"ARM"* という用語を、ARM64 (一般に *AArch64* とも呼ばれます) プロセッサで Windows 10 のデスクトップ バージョンを実行する PC の省略形として使っています。  ここでは、*"ARM32"* という用語を、32 ビット ARM アーキテクチャ (他のドキュメントでは一般に *ARM* と呼ばれます) の省略形として使っています。

## <a name="apps-and-experiences-on-arm"></a>ARM でのアプリとエクスペリエンス

### <a name="built-in-windows-10-experiences-apps-and-drivers"></a>組み込みの Windows 10 エクスペリエンス, アプリとドライバー
Edge、Cortana、スタートメニュー、エクスプローラーなどの組み込みの Windows 10 エクスペリエンスはすべてネイティブで、ARM64 として実行されます。 これには、グラフィックス、ネットワーク、ハードディスクなど、すべてのデバイスドライバーも含まれます。 これにより、Qualcomm Snapdragon プロセッサの完全なネイティブ速度で実行されているデバイスから、最適なユーザーエクスペリエンスとバッテリ寿命を得ることができます。

### <a name="universal-windows-platform-uwp-apps"></a>ユニバーサル Windows プラットフォーム (UWP) アプリ
ARM 上の Windows 10 は、Microsoft Store からすべての x86、ARM32、および ARM64 [UWP アプリ](../get-started/universal-application-platform-guide.md) を実行します。 ARM32 アプリと ARM64 アプリはエミュレーションなしでネイティブに実行されますが、x86 アプリはエミュレーションで実行されます。 UWP 開発者の場合、デバイスの最適なユーザー エクスペリエンスを提供するため、必ずアプリの ARM パッケージを提出してください。 詳しくは、「[アプリ パッケージのアーキテクチャ](/windows/msix/package/device-architecture)」をご覧ください。

>[!NOTE]
> ARM64 プラットフォームをネイティブでターゲットとする UWP アプリケーションをビルドするには、Visual Studio 2017 バージョン15.9 以降、または Visual Studio 2019 が必要です。 詳細については、[このブログ投稿](https://blogs.windows.com/buildingapps/2018/11/15/official-support-for-windows-10-on-arm-development)を参照してください。


>[!IMPORTANT]
> ARM 上の Windows 10 は、ARM64 デバイス上のストアからの x86、ARM32、ARM64 UWP アプリをサポートしています。 ユーザーが ARM64 デバイスに UWP アプリをダウンロードすると、使用可能なアプリの最適なバージョンが OS によって自動的にインストールされます。 アプリの x86、ARM32、ARM64 バージョンをストアに送信すると、アプリの ARM64 バージョンが OS によって自動的にインストールされます。 アプリの x86 と ARM32 のバージョンのみを送信する場合、OS は ARM32 バージョンをインストールします。 アプリの x86 バージョンのみを送信する場合は、OS によってそのバージョンがインストールされ、エミュレーション下で実行されます。 アーキテクチャについて詳しくは、「[アプリ パッケージのアーキテクチャ](/windows/msix/package/device-architecture)」をご覧ください。

### <a name="win32-apps"></a>Win32 アプリ
UWP アプリに加えて、ARM 上の Windows 10 では、PC と同じように、優れたパフォーマンスとシームレスなユーザーエクスペリエンスで、x86 Win32 アプリを変更せずに実行することもできます。 これらの x86 Win32 アプリは ARM 用に再コンパイルする必要がなく、ARM プロセッサで実行されていることを認識していません。 64ビットの x64 Win32 アプリはサポートされていませんが、ほとんどのアプリには x86 バージョンが用意されています。  アプリのアーキテクチャを選択した場合は、ARM PC 上の Windows 10 でアプリを実行するために、32ビットの x86 バージョンを選択するだけです。

## <a name="downloads"></a>ダウンロード

Visual Studio 2019 では、ARM 上の Windows 10 用のツールをいくつかダウンロードできます。 Visual Studio 2017 を使用するユーザー stil は、インストーラーを使用して、同等のツールとパッケージを検索してインストールできます。 これらの手順を実行するには、Visual Studio 2019 を使用している必要があることに注意してください。

### <a name="visual-c-redistributable"></a>Visual C++ 再頒布可能パッケージ

Visual C++ Redist パッケージは、ARM アプリで使用できます。 [Visual studio のダウンロードページ](https://visualstudio.microsoft.com/downloads/)にアクセスして、[**すべてのダウンロード**] を表示し、**他のツールとフレームワーク**を開き、[ **Microsoft Visual C++ 2019 の再頒布可能パッケージ**] に移動します。 **ARM64**ラジオボタンを選択し、[**ダウンロード**] をクリックします。

### <a name="remote-tools"></a>リモート ツール

ARM アプリでは Remote Tools for Visual Studio を使用できます。 [Visual studio のダウンロードページ](https://visualstudio.microsoft.com/downloads/)にアクセスして、[**すべてのダウンロード**] にスクロールし、[ **Tools for Visual Studio 2019**] を開いて、 **Remote Tools for Visual Studio 2019**エントリに移動します。 **ARM64* オプションボタンを選択し、[ **ダウンロード**] をクリックします。


## <a name="in-this-section"></a>このセクションの内容
|トピック | 説明 |
|-----|-----|
|[ARM での x86 エミュレーションのしくみ](apps-on-arm-x86-emulation.md)|x86 アプリが ARM でどのようにエミュレートされるかの概要。|
|[ARM における x86 アプリのトラブルシューティング](apps-on-arm-troubleshooting-x86.md)|ARM で実行する際の x86 アプリの一般的な問題とその解決方法。 |
|[ARM での ARM アプリのトラブルシューティング](apps-on-arm-troubleshooting-arm32.md)|ARM での実行時の ARM32 アプリと ARM64 アプリの一般的な問題とその修正方法。 |
|[プログラム互換性のトラブルシューティング ツール (ARM)](apps-on-arm-program-compat-troubleshooter.md)|アプリが ARM で正しく動作しない場合に互換性の設定を調整するためのガイダンス。 |

## <a name="related-topics"></a>関連トピック
|トピック | 説明 |
|-----|-----|
|[WDK を使った ARM64 ドライバーのビルド](/windows-hardware/drivers/develop/building-arm64-drivers)|ARM64 ドライバーをビルドするための手順。 |
| [ARM における x86 アプリのデバッグ](/windows-hardware/drivers/debugger/debugging-arm64) | ARM で x86 アプリをデバッグするためのガイダンス。 |