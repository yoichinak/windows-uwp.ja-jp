---
title: Windows パッケージ マネージャーにパッケージを送信する
description: アプリケーションを含むソフトウェア パッケージの配布チャネルとして、Windows パッケージ マネージャーを使用できます。
ms.date: 04/29/2020
ms.topic: overview
ms.localizationpriority: medium
ms.openlocfilehash: b2bde5a55d22d0541724cd777c315b2751cf1d36
ms.sourcegitcommit: 3153ef4838c35084a64173c7ed88719c8864f8cc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/07/2020
ms.locfileid: "96755279"
---
# <a name="submit-packages-to-windows-package-manager"></a>Windows パッケージ マネージャーにパッケージを送信する

[!INCLUDE [preview-note](../../includes/package-manager-preview.md)]

独立系ソフトウェア ベンダー (ISV) の場合は、アプリケーションを含むソフトウェア パッケージの配布チャネルとして Windows パッケージ マネージャーを使用できます。 Windows パッケージ マネージャーでは、現在、次の形式のインストーラーがサポートされています。MSIX、MSI、および EXE。

ソフトウェア パッケージを Windows パッケージ マネージャーに送信するには、次の手順を実行します。

1. [アプリケーションに関する情報を提供するパッケージ マニフェストを作成します](manifest.md)。 マニフェストは、Windows パッケージ マネージャー スキーマに従う YAML ファイルです。
2. [マニフェストを Windows パッケージ マネージャー リポジトリに送信します](repository.md)。 これは、**winget** ツールがアクセスできるマニフェストのコレクションを含む GitHub のオープン ソース リポジトリです。

## <a name="related-topics"></a>関連トピック

* [Winget ツールを使用する](../winget/index.md)
* [パッケージ マニフェストを作成する](manifest.md)
* [リポジトリにマニフェストを送信する](repository.md)
