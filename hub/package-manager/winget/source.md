---
title: source コマンド
description: Windows パッケージ マネージャーによってアクセスされるリポジトリを管理します。
ms.date: 04/28/2020
ms.topic: overview
ms.localizationpriority: medium
ms.openlocfilehash: 08af76389627bb8c21bf7a4ddb856d09119dc917
ms.sourcegitcommit: 837ef4b2c2375d023ee85204f72a029f9ec8f4ee
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/15/2020
ms.locfileid: "92079277"
---
# <a name="source-command-winget"></a>source コマンド (winget)

[!INCLUDE [preview-note](../../includes/package-manager-preview.md)]

> [!NOTE]
> **source** コマンドは、現在内部でのみ使用されています。 追加のソースは、現時点ではサポートされていません。

[winget](index.md) ツールの **source** コマンドは、Windows パッケージ マネージャーによってアクセスされるリポジトリを管理します。 **source** コマンドを使用すると、リポジトリを**追加**、**削除**、**一覧表示**、および**更新**できます。

ソースは、アプリケーションを検出してインストールするためのデータを提供します。 新しいソースは、セキュリティで保護された場所としてそれを信頼できる場合にのみ追加してください。

## <a name="usage"></a>使用方法

`winget source \<sub command> \<options>`

![ソース画像](images\source.png)

## <a name="arguments"></a>引数

次の引数を使用できます。

| 引数  | 説明 |
|--------------|-------------|
| **-?、--help** |  このコマンドに関する追加のヘルプを取得します。 |

## <a name="sub-commands"></a>サブ コマンド

source では、ソースを操作するための次のサブ コマンドがサポートされています。

| サブ コマンド  | 説明 |
|--------------|-------------|
|  **add** |  新しいソースを追加します。 |
|  **list** | 有効なソースの一覧を列挙します。 |
|  **update** | ソースを更新します。 |
|  **remove** | ソースを削除します。 |
|  **reset** | **winget** を初期構成にリセットします。  |

## <a name="options"></a>オプション

**source** コマンドは、次のオプションをサポートしています。

| オプション  | 説明 |
|--------------|-------------|
|  **-n、--name** | ソースを識別するための名前。 |
|  **-a、--arg** | ソースの URL または UNC。 |
|  **-t、--type** | ソースの種類。 |
| **-?、--help** |  このコマンドに関する追加のヘルプを取得します。 |

## <a name="add"></a>追加

**add** サブ コマンドは、新しいソースを追加します。 このサブ コマンドには、 **--name** オプションと **name** 引数が必要です。

使用方法: `winget source add [-n, --name] \<name> [-a] \<url> [[-t] \<type>]`

例: `winget source add --name Contoso  https://www.contoso.com/cache`

**add** サブ コマンドでは、省略可能な **type** パラメーターもサポートされます。 **type** パラメーターは、接続先のリポジトリの種類をクライアントに伝えます。 次の種類がサポートされています。

| 種類  | 説明 |
|--------------|-------------|
| **Microsoft.PreIndexed.Package** | ソース \<default> の種類。 |

## <a name="list"></a>list

**list** サブ コマンドは、現在有効になっているソースを列挙します。 このサブ コマンドは、特定のソースの詳細も提供します。

使用方法: `winget source list [-n, --name] \<name>`

### <a name="list-all"></a>list all

**list** サブコマンドは、単独で、サポートされているソースの完全一覧を表示します。 たとえば、次のように入力します。

```CMD
> C:\winget source list
> Name   Arg
> -----------------------------------------
> winget https://winget.azureedge.net/cache

```

### <a name="list-source-details"></a>list source details

ソースの詳細を取得するには、ソースを識別するために使用する名前を渡します。 たとえば、次のように入力します。

```CMD
> C:\winget source list --name contoso  
> Name   : contoso  
> Type   : Microsoft.PreIndexed.Package  
> Arg    : https://pkgmgr-int.azureedge.net/cache  
> Data   : AppInstallerSQLiteIndex-int_g4ype1skzj3jy  
> Updated: 2020-4-14 17:45:32.000
```

**Name** は、ソースを識別するための名前を表示します。
**Type** は、リポジトリの種類を表示します。
**Arg** は、ソースによって使用される URL またはパスを表示します。
**Data** は、必要に応じて使用される省略可能なパッケージ名を表示します。
**Updated** は、ソースが最後に更新された日時を表示します。

## <a name="update"></a>update

**update** サブ コマンドは、個々のソースまたはすべてに対して更新を強制します。

使用方法: `winget source update [-n, --name] \<name>`

### <a name="update-all"></a>update all

**update** サブ コマンドは、単独で、各リポジトリを要求し、更新します。 たとえば次のようになります。`C:\winget update`

### <a name="update-source"></a>更新ソース

**update** サブ コマンドと **--name** オプションを組み合わせて、個々のソースを指示して更新することができます。 次に例を示します。`C:\winget source update --name contoso`

## <a name="remove"></a>削除

**remove** サブ コマンドは、ソースを削除します。 このサブ コマンドでは、ソースを識別するために、 **--name** オプションと **name 引数**が必要です。

使用方法: `winget source remove [-n, --name] \<name>`

たとえば次のようになります。`winget source remove --name Contoso`

## <a name="reset"></a>reset

**reset** サブ コマンドは、クライアントを元の構成にリセットします。 **reset** サブ コマンドは、すべてのソースを削除し、ソースを既定値に設定します。 このサブ コマンドは、まれなケースでのみ使用してください。

使用方法: `winget source reset`

たとえば次のようになります。`winget source reset`

## <a name="default-repository"></a>既定のリポジトリ

Windows パッケージ マネージャーは、既定のリポジトリを指定します。 リポジトリを識別するには、**list** コマンドを使用できます。 たとえば次のようになります。`winget source list`

## <a name="related-topics"></a>関連トピック

* [winget ツールを使用したアプリケーションのインストールと管理](index.md)
