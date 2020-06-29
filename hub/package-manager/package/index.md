---
title: Windows パッケージ マネージャーにパッケージを送信する
description: アプリケーションを含むソフトウェア パッケージの配布チャネルとして、Windows パッケージ マネージャーを使用できます。
ms.date: 04/29/2020
ms.topic: overview
ms.localizationpriority: medium
ms.openlocfilehash: ae9c9039154e2a576a691a01d64abcf8c9029c1c
ms.sourcegitcommit: 4df8c04fc6c22ec76cdb7bb26f327182f2dacafa
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/24/2020
ms.locfileid: "85334605"
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