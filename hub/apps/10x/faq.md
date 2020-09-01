---
Description: Windows 10 に関する基本的な開発者の質問に対する回答について説明します。
title: Windows 10 開発者向け FAQ
ms.topic: article
ms.date: 06/02/2020
ms.localizationpriority: medium
ms.author: quradic
author: QuinnRadich
ms.openlocfilehash: f321815658a1b59d941f8b2c0e1fa5aa0142b4f7
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89157676"
---
# <a name="windows-10x-developer-faq"></a>Windows 10 開発者向け FAQ

> [!IMPORTANT]
> Microsoft では先日、Windows 10 および Windows 10X の優先順位に関するいくつかの変更を発表しました。
> これらの発表には、Windows 10X フォーム ファクターの優先順位の変更が含まれます。 [詳細をこちらでご確認ください。](https://blogs.windows.com/windowsexperience/2020/05/04/accelerating-innovation-in-windows-10-to-meet-customers-where-they-are/)

Windows 10 は、デュアル画面デバイスで使用できるように最適化された Windows ファミリの製品ラインです。 開発者は、アプリを Windows 10 に最適化することで、より広範な対象ユーザーに配信できます。これにより、モバイルおよびデュアルスクリーンの対象ユーザーに固有の新機能を利用できるだけでなく、同じ幅の Windows 10 機能と豊富なデスクトップサポートも利用できます。 [Windows 10 は、2019の遅延について発表](https://blogs.windows.com/windowsexperience/2019/10/02/introducing-windows-10x-enabling-dual-screen-pcs-in-2020/#6qxkItE2XMPu24uw.97)しました。今後、2020のリリースを予定しています。

![Windows 10 を実行しているデバイス](images/windows-10x-devices.png)
 
*[表示されるプレリリース製品、画面がシミュレートされ、変更される可能性があります]*

デュアルスクリーンエクスペリエンスと Windows 10 の構築の詳細については、 [Microsoft 365 の開発日](https://developer.microsoft.com/microsoft-365/virtual-events)または [デュアルスクリーンの開発者ドキュメント](/dual-screen/)から仮想セッションを確認してください。ひとめでわかるように、ここでは、いくつかの質問に対する回答を示します。

### <a name="how-is-this-different-from-developing-for-windows-10"></a>Windows 10 向けの開発とはどのように違いますか。

アプリケーションの大部分は、まったく異なるものではありません。 Windows 10 アプリの作成は、Windows 10 SDK を通じてサポートされています。 Windows 10 の式として、Windows 10 は Windows ランタイム (WinRT) Api をサポートし、ネイティブコンテナーを介して Win32 アプリを実行します。 次に、新規または既存の UWP または Win32 アプリからのデュアルスクリーンデバイス向けに設計された新しい Api を呼び出すことができます。これにより、新しいプラットフォームの機能やメリットにアクセスできるようになります。

### <a name="does-this-replace-desktop-windows-10"></a>これにより、デスクトップの Windows 10 が置換されるのですか。

不正解です。 Windows 10 は、Windows 10 のデスクトップリリースと並行してリリースされます。 Windows 10 のデスクトップリリースでは、最新のデスクトップアプリケーションのストーリーに対する拡張機能と機能強化が引き続き提供されます。 Windows 10 は、デュアルスクリーンプラットフォームをサポートするために最適化された別のプラットフォームです。

### <a name="when-will-windows-10x-be-released"></a>Windows 10 がリリースされるのはいつですか。

Windows 10 は、2020以降のサードパーティ製のデュアルスクリーンデバイスに付随するようにリリースされます。

### <a name="when-can-i-start-development-for-windows-10x"></a>Windows 10 の開発を開始するにはどうすればよいですか。

[Microsoft emulator と Windows 10 倍のエミュレーターイメージ](/dual-screen/windows/get-dev-tools)を今すぐダウンロードできます。 このエミュレーターは引き続き改善され、他の Windows 10 対応デバイスのサポートによって補完されます。 これらのエミュレーターは、プレリリース版の Windows SDK と組み合わせることで、最初のデュアルスクリーンデバイスが一般公開される前に、Windows 10 の開発を可能にします。

### <a name="will-my-universal-windows-platform-uwp-apps-run-on-windows-10x"></a>ユニバーサル Windows プラットフォーム (UWP) アプリは Windows 10 倍で動作しますか。

ほとんどの UWP アプリは Windows 10 で完全にサポートされており、Windows 10 を実行するデバイスでは変更なしで機能します。 すべての WinRT Api は、UWP アプリがアクセスできるその他のほとんどの機能としてサポートされています。 プレリリースの開発が続行されると、サポートされていないその他の機能の詳細についてのドキュメントをリリースします。

### <a name="will-my-win32-apps-run-on-windows-10x"></a>Win32 アプリは Windows 10 で実行されますか。

Windows 10 は、含まれている環境で Win32 アプリを実行するネイティブサポートを提供します。 ほとんどの Win32 アプリは、インシデントを発生させずに Windows 10 デバイスで実行およびデバッグできます。また、新しい Win32 Api を使用して、デュアルスクリーンサポートをアプリに追加することもできます。

### <a name="are-there-any-features-of-my-app-that-wont-work-on-windows-10x"></a>Windows 10 では動作しないアプリの機能はありますか。

Windows 10 がリリース前の開発を続けているため、特定の制限事項を強調表示するドキュメントをリリースします。 ただし、Win32 アプリの実行に使用されている環境には Windows シェルが含まれていないため、シェル拡張と同様の機能はサポートされません。 同様に、Windows 10 デバイスは、電源使用オプションなど、特定のシステム設定に関連する Api をサポートしていません。

### <a name="if-i-enhance-my-app-with-windows-10x-features-will-it-still-run-on-devices-running-desktop-windows-10"></a>Windows 10 の機能でアプリを拡張すると、デスクトップ Windows 10 を実行しているデバイスでも動作しますか。

Windows 10 用に設計されたアプリは、windows 10 のデスクトップバージョンを実行しているデバイスでも動作しますが、次のメジャーバージョンの更新まで、これらの新しい Windows Api は Windows 10 のデスクトップバージョンには追加されません。 複数のバージョンのデスクトップ Windows 10 でサポートされているアプリを開発している場合と同様に、適応型の [コーディングのベストプラクティス](/windows/uwp/debug-test-perf/version-adaptive-code) に従って、10倍とデスクトップの両方の windows 10 でアプリが正常に動作することを確認します。