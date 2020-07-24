---
title: search コマンド
description: ソースに対して、インストールできる使用可能なアプリケーションに関するクエリを実行します
ms.date: 04/28/2020
ms.topic: overview
ms.localizationpriority: medium
ms.openlocfilehash: 7038f9b31c4c0446e3af56cac2d118598347d4d3
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493257"
---
# <a name="search-command-winget"></a>search コマンド (winget)

[!INCLUDE [preview-note](../../includes/package-manager-preview.md)]

[winget](index.md) ツールの **search** コマンドは、ソースに対して、インストールできる使用可能なアプリケーションに関するクエリを実行します。  

**search** コマンドでは、使用可能なすべてのアプリケーションを表示することも、フィルター処理して、特定のアプリケーションを検出することもできます。 **search** コマンドは、通常、特定のアプリケーションのインストールに使用する文字列を識別するために使用されます。

## <a name="usage"></a>使用方法

`winget search [[-q] \<query>] [\<options>]`

![[検索]](images\search.png)

## <a name="arguments"></a>引数

次の引数を使用できます。

| 引数  | 説明 |
 --------------|-------------|
| **-q、--query** |  アプリを検索するために使用するクエリ。 |
| **-?、--help** |  このコマンドに関する追加のヘルプを取得します。 |

## <a name="show-all"></a>すべて表示

search コマンドにフィルターやオプションが含まれていない場合は、既定のソースで使用可能なすべてのアプリケーションが表示されます。 また、**source** オプションのみを渡すと、別のソース内のすべてのアプリケーションを検索できます。

## <a name="search-strings"></a>検索文字列

検索文字列は、次のオプションを使用してフィルター処理することができます。

| オプション  | 説明 |
 --------------|-------------|
| **--id**        |   検索をアプリケーションの ID に限定します。 ID には、発行元とアプリケーション名が含まれます。 |
| **--name**      |  検索をアプリケーションの名前に限定します。 |
| **--moniker**  |    検索を指定されたモニカーに限定します。 |
| **--tag**    |  検索をアプリケーション用に一覧表示されているタグに限定します。 |
| **--command**   |   検索を、アプリケーション用に一覧表示されているコマンドに限定します。 |

文字列は部分文字列として扱われます。 また、既定の検索では大文字と小文字は区別されません。 たとえば、`winget search micro` では以下が返される可能性があります。

* Microsoft
* microscope
* MyMicro

## <a name="search-options"></a>［検索のオプション］

検索コマンドでは、結果を限定するのに役立つさまざまなオプションやフィルターがサポートされています。

| オプション  | 説明 |
 --------------|-------------|
| **-e、--exact**  |     大文字小文字の区別の検査を含め、クエリで正確な文字列を使用します。 部分文字列の既定の動作は使用されません。  |  
| **-n、--count**      |  表示の出力を、指定した数に制限します。 |
| **-s、--source**     |  検索を、指定した[ソース](source.md)名に制限します。  |

## <a name="related-topics"></a>関連トピック

* [winget ツールを使用したアプリケーションのインストールと管理](index.md)
