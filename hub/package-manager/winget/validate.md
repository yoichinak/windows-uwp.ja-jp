---
title: winget の validate コマンド
description: GitHub にある Microsoft コミュニティのパッケージ マニフェスト リポジトリにソフトウェアを送信するためのマニフェスト ファイルを検証します。
ms.date: 04/28/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: ec1f15ef9086c1083430c9bbe55ea52827ae4bfb
ms.sourcegitcommit: 4df8c04fc6c22ec76cdb7bb26f327182f2dacafa
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/24/2020
ms.locfileid: "85334462"
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
