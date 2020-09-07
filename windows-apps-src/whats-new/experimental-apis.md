---
title: 試験的な API
description: 開発者が試せるように、Windows Insider SDK を使用して試験的な API を外部でフライトする方法について説明します。
ms.date: 11/13/2017
ms.topic: article
keywords: windows 10, uwp, 試験的, api
ms.localizationpriority: medium
ms.openlocfilehash: 5a4813e7b4ae1e3dd16017066758aa8a35d0570a
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89170806"
---
# <a name="experimental-apis"></a>試験的な API

試験的な API は、設計の初期段階で使用され、所有者がフィードバックを反映したり、他のシナリオに対するサポートを追加したりする場合に変更される可能性があります。

これらの API は、[Windows Insider SDK](https://www.microsoft.com/software-download/windowsinsiderpreviewSDK) を利用して、外部でフライトされます。そのため、API が正式なプラットフォームに組み込まれる前に、開発者は API を試したり、フィードバックを提供したりすることができます。 これらの API がフライトされている場合は、安定性や問題への対応は保証されません。

## <a name="consuming-experimental-apis"></a>試験的な API の使用
Intellisense を利用すると、API が試験的なものであるかどうかを確認できます。 また、試験的な API を使用したときに、"評価のみを目的としており、今後の更新プログラムで変更または削除されることがあります" などのコンパイラの警告が表示されます。

これらの警告は、実稼働プロジェクトで試験的な API に対する依存関係を作成するのを防ぐ場合に役立ちます。 試験的なプロジェクトでは、これらの警告を無視したり、無効にしたりすることができます。

既定では、これらの API は実行時に無効になっており、これらの API を呼び出すとランタイム例外が発生します。 このもう一つのセーフガードにより、不注意による依存関係の作成や、試験的な API を使用するアプリの広範な配布を防ぐことができます。

これらの API を試験用に有効にするには、[Windows デバイス ポータル (WDP)](../debug-test-perf/device-portal.md) 機能のプラグインをターゲット デバイスで使用し、呼び出す API に対応する機能を有効にします。

特定の試験的な API に関するドキュメントは、API を所有するチームの裁量に任されています。

## <a name="providing-feedback"></a>フィードバックの提供

試験的な API を試し、フィードバックを提供する場合は、[Windows フィードバック Hub](https://support.microsoft.com/help/4021566/windows-10-send-feedback-to-microsoft-with-feedback-hub) の **"開発者向けプラットフォーム"** カテゴリをご利用ください。