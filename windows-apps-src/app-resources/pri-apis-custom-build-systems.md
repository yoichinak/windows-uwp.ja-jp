---
description: パッケージ リソース インデックス (PRI) API を使用すると、UWP アプリのリソース用にカスタム ビルド システムを開発することができます。 ビルド システムでは、UWP アプリで必要となる複雑さのレベルにかかわらず、PRI ファイルの作成やバージョン管理を行ったり、PRI ファイルをダンプしたりすることができます。
title: パッケージリソースのインデックス作成とカスタムビルドシステム
template: detail.hbs
ms.date: 05/07/2018
ms.topic: article
keywords: Windows 10, UWP, リソース, 画像, アセット, MRT, 修飾子
ms.localizationpriority: medium
ms.openlocfilehash: 84b6a05927e2106df136741a262c3af5ef08a575
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2020
ms.locfileid: "93031665"
---
# <a name="package-resource-indexing-pri-apis-and-custom-build-systems"></a>パッケージ リソース インデックス (PRI) API とカスタム ビルド システム
[パッケージ リソース インデックス (PRI) API](/windows/desktop/menurc/pri-indexing-reference) を使用すると、UWP アプリのリソース用にカスタム ビルド システムを開発することができます。 ビルド システムでは、UWP アプリが必要とする複雑さのレベルにかかわらず、パッケージ リソース インデックス (PRI) ファイルを (XML として) 作成、バージョン管理、ダンプすることができます。 現在 MakePri.exe コマンド ライン ツールを使用しているカスタム ビルド システムがある場合 (「[MakePri.exe を使用して手動でリソースをコンパイルする](makepri-exe-command-options.md)」を参照)、パフォーマンスと制御を向上させるために、MakePri.exe の呼び出しではなく、PRI API の呼び出しに切り替えることをお勧めします。

PRI API は、Windows 10 向け Windows SDK のバージョン 1803 で導入されました。 API は、Win32 Windows API の形式になります。つまり、それらを呼び出すオプションがいくつかあります。 Win32 アプリから直接それらを呼び出すか、または .NET アプリ、さらには UWP アプリからでも [platform invoke](/dotnet/framework/interop/consuming-unmanaged-dll-functions?branch=live) を介してそれらを呼び出すことができます。

このトピックのシナリオでは、Win32 Visual C++ Windows Console Application プロジェクトからの PRI API の呼び出しを示します。 背景情報については、「[リソース管理システム](resource-management-system.md)」を参照してください。

> [!NOTE]
> おそらくカスタム ビルド システム アプリを Microsoft Store に提出する必要はないため、この注意事項が問題になることはあまりありません。 ただし、UWP アプリの形式でカスタム ビルド システムを開発するオプションを選択する場合、そのアプリは Microsoft Store に提出することができないという点で通常とは異なる UWP アプリになります。 これは、プラットフォーム呼び出しを使用する UWP アプリが Microsoft Store 認定に失敗するためです。 この場合、プラットフォームの起動呼び出しは、配布する UWP アプリ (PRI ファイルを作成しているアプリ) *ではなく* 、 *カスタム ビルド システムでのみ行われる* 点に注意してください。

## <a name="scenario-walkthroughs"></a>シナリオのチュートリアル
|トピック|説明|
|-|-|
|[シナリオ 1: 文字列リソースとアセット ファイルから PRI ファイルを生成する](pri-apis-scenario-1.md)|このシナリオでは、カスタム ビルド システムを表す新しいアプリを作成します。 リソース インデクサーを作成し、文字列とその他の種類のリソースを追加します。 次に、PRI ファイルを生成してダンプします。|

## <a name="important-apis"></a>重要な API
* [パッケージ リソース インデックス (PRI) リファレンス](/windows/desktop/menurc/pri-indexing-reference)

## <a name="related-topics"></a>関連トピック
* [MakePri.exe を使用して手動でリソースをコンパイルする](makepri-exe-command-options.md)
* [アンマネージ DLL 関数の処理](/dotnet/framework/interop/consuming-unmanaged-dll-functions?branch=live)
* [リソース管理システム](resource-management-system.md)
