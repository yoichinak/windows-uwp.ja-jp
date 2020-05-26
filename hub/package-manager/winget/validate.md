---
title: winget の validate コマンド
description: GitHub にある Microsoft コミュニティのパッケージ マニフェスト リポジトリにソフトウェアを送信するためのマニフェスト ファイルを検証します。
author: KevinLaMS
ms.author: kevinla
ms.date: 04/28/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: a5772e144a4226be9fbb4a4949aaac1e3d4408e6
ms.sourcegitcommit: d0f479f1955881afb62c2af249db5d0b053b63e5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/19/2020
ms.locfileid: "83824933"
---
# <a name="validate-command-winget"></a>validate コマンド (winget)

[!INCLUDE [preview-note](../../includes/package-manager-preview.md)]

[winget](index.md) ツールの **validate** コマンドは、GitHub にある **Microsoft コミュニティのパッケージ マニフェスト リポジトリ**にソフトウェアを送信するための[マニフェスト ファイル](../package/manifest.md)を検証します。 マニフェストは、[仕様](https://github.com/microsoft/winget-pkgs/YamlSpec.md)に従った YAML ファイルである必要があります。

## <a name="usage"></a>使用方法

`winget validate [--manifest] \<manifest>`

## <a name="arguments"></a>引数

次の引数を使用できます。

| 引数  | 説明 |
|--------------|-------------|
| **--manifest** |  検証するマニフェストへのパス。 |
| **-?、--help** |  このコマンドに関する追加のヘルプを取得します |

## <a name="related-topics"></a>関連トピック

* [winget ツールを使用したアプリケーションのインストールと管理](index.md)
* [Windows パッケージ マネージャーへのパッケージの送信](../package/index.md)
