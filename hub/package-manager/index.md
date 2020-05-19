---
title: Windows パッケージ マネージャー
description: ''
author: denelon
ms.author: denelon
ms.date: 05/03/2020
ms.topic: overview
ms.localizationpriority: medium
ms.openlocfilehash: 4f7a6533d3dea9c304e9be7d8e689ab537a46449
ms.sourcegitcommit: d0f479f1955881afb62c2af249db5d0b053b63e5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/19/2020
ms.locfileid: "83580099"
---
# <a name="windows-package-manager-preview"></a>Windows パッケージ マネージャー (プレビュー)

[!INCLUDE [preview-note](../includes/package-manager-preview.md)]

Windows パッケージ マネージャーは、Windows 10 にアプリケーションをインストールするためのコマンド ライン ツールと一連のサービスで構成される、包括的な[パッケージ マネージャー ソリューション](#understanding-package-managers)です。

## <a name="windows-package-manager-for-developers"></a>開発者向け Windows パッケージ マネージャー

開発者は **winget** コマンド ライン ツールを使用して、選別された一連のアプリケーションを検出、インストール、アップグレード、削除、および構成します。 インストールが完了すると、開発者は Windows ターミナル、PowerShell、またはコマンド プロンプトを使用して **winget** にアクセスできます。

詳細については、「[winget ツールを使用したアプリケーションのインストールと管理](winget/index.md)」を参照してください。

## <a name="windows-package-manager-for-isvs"></a>ISV 向け Windows パッケージ マネージャー

独立系ソフトウェア ベンダー (ISV) は、ツールとアプリケーションを含むソフトウェア パッケージの配布チャネルとして Windows パッケージ マネージャーを使用できます。 ソフトウェア パッケージ (.msix、.msi、または .exe インストーラーを含む) を Windows パッケージ マネージャーに送信するために、Mirosoft では GitHub にオープン ソースの **Microsoft コミュニティ パッケージ マニフェスト リポジトリ**を提供しています。ISV は、そこに[パッケージ マニフェスト](package/manifest.md)をアップロードし、Windows パッケージ マネージャーに自身のソフトウェア パッケージが含まれるよう検討してもらうことができます。 マニフェストは自動的に検証されますが、手動で確認される場合もあります。

詳細については、[Windows パッケージ マネージャーへのパッケージの送信](package/repository.md)に関するページを参照してください。

## <a name="understanding-package-managers"></a>パッケージ マネージャーについて

パッケージ マネージャーは、ソフトウェアのインストール、アップグレード、構成、および使用を自動化するために使用されるシステムまたは一連のツールです。 ほとんどのパッケージ マネージャーは、開発者ツールを検出およびインストールするために設計されています。

理想的には、開発者は、パッケージ マネージャーを使用して、特定のプロジェクトのソリューションを開発するために必要なツールの前提条件を指定します。 次に、パッケージ マネージャーは、宣言型の指示に従ってツールをインストールし、構成します。 パッケージ マネージャーを使用すると、環境の準備に費やす時間を短縮でき、確実にマシンに同じバージョンのパッケージをインストールすることができます。

サード パーティのパッケージ マネージャーは、[Microsoft コミュニティのパッケージ マニフェスト リポジトリ](package/repository.md)を利用して、ソフトウェア カタログのサイズを増やすことができます。

## <a name="related-topics"></a>関連トピック

* [winget ツールを使用したソフトウェア パッケージのインストールと管理](winget/index.md)
* [Windows パッケージ マネージャーへのパッケージの送信](package/index.md)