---
title: show コマンド
description: アプリケーションのソースの詳細およびアプリケーションに関連付けられたメタデータを含む、指定されたアプリケーションの詳細を表示します。
author: KevinLaMS
ms.author: kevinla
ms.date: 04/28/2020
ms.topic: overview
ms.localizationpriority: medium
ms.openlocfilehash: 1df5a5287b6c7a1321025182f7b3f24ed896e76d
ms.sourcegitcommit: d0f479f1955881afb62c2af249db5d0b053b63e5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/19/2020
ms.locfileid: "83824983"
---
# <a name="show-command-winget"></a>show コマンド (winget)

[!INCLUDE [preview-note](../../includes/package-manager-preview.md)]

[winget](index.md) ツールの **show** コマンドは、アプリケーションのソースの詳細およびアプリケーションに関連付けられたメタデータを含む、指定されたアプリケーションの詳細を表示します。

**show** コマンドは、アプリケーションで送信されたメタデータのみを表示します。 送信されたアプリケーションで一部のメタデータが除外されている場合、データは表示されません。

## <a name="usage"></a>使用方法

`winget show [[-q] \<query>] [\<options>]`

![show コマンド](images\show.png)

## <a name="arguments"></a>引数

次の引数を使用できます。

| 引数  | 説明 |
|--------------|-------------|
| **-q、--query** |  アプリケーションを検索するために使用するクエリ。 |
| **-?、--help** |  このコマンドに関する追加のヘルプを取得します。 |

## <a name="options"></a>オプション

次のオプションを使用できます。

| オプション  | 説明 |
|--------------|-------------|
| **-m、--manifest** | インストールするアプリケーションのマニフェストへのパス。 |
| **--id**         |  ID で結果をフィルター処理します。 |
| **--name**   |      名前で結果をフィルター処理します。 |
| **--moniker**   |  アプリケーション モニカーで結果をフィルター処理します。 |
| **-v、--version** |  指定されたバージョンを使用します。 既定値は最新バージョンです。 |
| **-s、--source** |   指定された [ソース](source.md)を使用してアプリケーションを検索します。 |
| **-e、--exact**     | 完全一致を使用してアプリケーションを検索します。 |
| **--versions**    | アプリケーションの使用可能なバージョンを表示します。 |

## <a name="multiple-selections"></a>複数選択

**winget** に指定されたクエリの結果が 1 つのアプリケーションでない場合、**winget** は検索の結果を表示します。 これにより、検索を絞り込むために必要な追加データが提供されます。

## <a name="results-of-show"></a>show の結果

1 つのアプリケーションが検出されると、以下のデータが表示されます。

### <a name="metadata"></a>メタデータ

| 値  | 説明 |
|--------------|-------------|
| **Id**   | アプリケーションの ID。 |
| **名前**  | アプリケーションの名前です。 |
| **Publisher** | アプリケーションの発行元。 |
| **バージョン** | アプリケーションのバージョン。 |
| **Author**  | アプリケーションの作成者。 |
| **AppMoniker** | アプリケーションの AppMoniker。 |
| **説明** | アプリケーションの説明。 |
| **ライセンス**  | アプリケーションのライセンス。 |
| **LicenseUrl** | アプリケーションのライセンス ファイルの URL。 |
| **Homepage**  | アプリケーションのホームページ。 |
| **Tags** | 検索を支援するために提供されるタグ。  |
| **コマンド** | アプリケーションでサポートされているコマンド。 |
| **Channel**  | アプリケーションがプレビューとリリースのどちらであるかについての詳細。  |
| **Minimum OS Version** | アプリケーションでサポートされている最小 OS バージョン。 |

### <a name="installer-details"></a>インストーラーの詳細

| 値  | 説明 |
|--------------|-------------|
| **Arch**   | インストーラーのアーキテクチャ。 |
| **言語**  | インストーラーの言語。 |
| **Installer Type**  | インストーラーの種類。 |
| **Download Url** | インストーラーの URL。 |
| **Hash** | インストーラーの Sha-256。  |
| **Scope** | インストーラーがコンピューターごとかユーザーごとかが表示されます。 |

## <a name="related-topics"></a>関連トピック

* [winget ツールを使用したアプリケーションのインストールと管理](index.md)
