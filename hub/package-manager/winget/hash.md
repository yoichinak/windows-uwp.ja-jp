---
title: winget の hash コマンド
description: インストーラーの SHA256 ハッシュを生成します。
ms.date: 04/28/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 4b6599a2b538829c6d9107b20f5f22d22f646542
ms.sourcegitcommit: 4df8c04fc6c22ec76cdb7bb26f327182f2dacafa
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/24/2020
ms.locfileid: "85334561"
---
# <a name="hash-command-winget"></a>hash コマンド (winget)

[!INCLUDE [preview-note](../../includes/package-manager-preview.md)]

[winget](index.md) ツールの **hash** コマンドは、インストーラーの SHA256 ハッシュを生成します。 このコマンドは、GitHub にある **Microsoft コミュニティのパッケージ マニフェスト リポジトリ**にソフトウェアを送信するための[マニフェスト ファイル](../package/manifest.md)を作成する必要がある場合に使用します。 また、**hash** コマンドでは、MSIX ファイル用の SHA256 証明書ハッシュの生成もサポートされています。

## <a name="usage"></a>使用方法

`winget hash [-f] \<file> [\<options>]`

**hash** サブ コマンドは、ローカル ファイルでのみ実行できます。 **hash** サブ コマンドを使用するには、インストーラーを既知の場所にダウンロードします。 次に、ファイル パスを引数として **hash** サブ コマンドに渡します。

## <a name="arguments"></a>引数

次の引数を使用できます。

| 引数  | 説明 |
|--------------|-------------|
| **-f、--file** |  ハッシュするファイルへのパス。 |
| **-m、--msix**  | hash コマンドで、MSIX インストーラーで使用する SHA 256 SignatureSha256 も作成することを指定します。 |
| **-?、--help** |  このコマンドに関する追加のヘルプを取得します。 |

## <a name="related-topics"></a>関連トピック

* [winget ツールを使用したアプリケーションのインストールと管理](index.md)
* [Windows パッケージ マネージャーへのパッケージの送信](../package/index.md)
