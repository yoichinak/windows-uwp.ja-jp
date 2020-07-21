---
title: Windows パッケージ マネージャー
description: Windows パッケージ マネージャーは、Windows 10 にアプリケーションをインストールするためのコマンド ライン ツールと一連のサービスで構成される、包括的なパッケージ マネージャー ソリューションです。
ms.date: 05/03/2020
ms.topic: overview
ms.localizationpriority: medium
ms.openlocfilehash: 5b25f2c651e11a5ff97a630bb802b236771f5441
ms.sourcegitcommit: 4df8c04fc6c22ec76cdb7bb26f327182f2dacafa
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/24/2020
ms.locfileid: "85334621"
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