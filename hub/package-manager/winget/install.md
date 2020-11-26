---
title: install コマンド
description: 指定されたアプリケーションをインストールします。
ms.date: 04/28/2020
ms.topic: overview
ms.localizationpriority: medium
ms.openlocfilehash: 5daae6dabee1201dd9df0b83dc56f98b06b15487
ms.sourcegitcommit: 4df27104a9e346d6b9fb43184812441fe5ea3437
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/25/2020
ms.locfileid: "96025175"
---
# <a name="install-command-winget"></a>install コマンド (winget)

[!INCLUDE [preview-note](../../includes/package-manager-preview.md)]

[winget](index.md) ツールの **install** コマンドは、指定されたアプリケーションをインストールします。 インストールするアプリケーションを特定するには、[**search**](search.md) コマンドを使用します。  

**install** コマンドでは、インストールする正確な文字列を指定する必要があります。 あいまいさがある場合は、**install** コマンドを正確なアプリケーションに絞り込むように求めるプロンプトが表示されます。

## <a name="usage"></a>使用方法

`winget install [[-q] \<query>] [\<options>]`

![search コマンド](images\install.png)

## <a name="arguments"></a>引数

次の引数を使用できます。

| 引数      | 説明 |
|-------------|-------------|  
| **-q、--query**  |  アプリを検索するために使用するクエリ。 |
| **-?、--help** |  このコマンドに関する追加のヘルプを取得します。 |

## <a name="options"></a>オプション

オプションを使用すると、インストール エクスペリエンスをニーズに合わせてカスタマイズできます。

| オプション      | 説明 |
|-------------|-------------|  
| **-m、--manifest** |   この後にマニフェスト (YAML) ファイルのパスを指定する必要があります。 マニフェストを使用して、[ローカル YAML ファイル](#local-install)からインストール エクスペリエンスを実行できます。 |
| **--id**    |  インストールをアプリケーションの ID に限定します。   |  
| **--name**   |  検索をアプリケーションの名前に限定します。 |  
| **--moniker**   | 検索をアプリケーション用に一覧表示されているモニカーに限定します。 |  
| **-v、--version**  |  インストールする正確なバージョンを指定できます。 指定しない場合、latest によって最新バージョンのアプリケーションがインストールされます。 |  
| **-s、--source**   |  検索を、指定されたソース名に制限します。 この後にソース名を指定する必要があります。 |  
| **-e、--exact**   |   大文字小文字の区別の検査を含め、クエリで正確な文字列を使用します。 部分文字列の既定の動作は使用されません。 |  
| **-i、--interactive** |  対話型モードでインストーラーを実行します。 既定のエクスペリエンスでは、インストーラーの進行状況が表示されます。 |  
| **-h、--silent** |  サイレント モードでインストーラーを実行します。 これにより、すべての UI が抑制されます。 既定のエクスペリエンスでは、インストーラーの進行状況が表示されます。 |  
| **-o、--log**  |  ログ記録をログ ファイルに送信します。 書き込み権限を持っているファイルへのパスを指定する必要があります。 |
| **--override** | インストーラーに直接渡される文字列。    |
| **-l、--location** |    インストールする場所 (サポートされている場合)。 |

### <a name="example-queries"></a>クエリの例

次の例では、アプリケーションの特定のバージョンをインストールします。

```CMD
winget install powertoys --version 0.15.2
```

次の例では、アプリケーションをその ID からインストールします。

```CMD
winget install --id Microsoft.PowerToys
```

次の例では、バージョンと ID によってアプリケーションをインストールします。

```CMD
winget install --id Microsoft.PowerToys --version 0.15.2
```

## <a name="multiple-selections"></a>複数選択

**winget** に指定されたクエリの結果が 1 つのアプリケーションでない場合、**winget** は検索の結果を表示します。 これにより、正しいインストールのために検索を絞り込むうえで必要な追加データが提供されます。

選択を 1 つのファイルに限定するには、**正確な** クエリ オプションと組み合わせてアプリケーションの **ID** を使用するのが最善です。  たとえば、次のように入力します。

```CMD
winget install --id Git.Git -e 
```

## <a name="local-install"></a>ローカル インストール

**manifest** オプションを使用すると、YAML ファイルをクライアントに直接渡すことによってアプリケーションをインストールできます。 **manifest** オプションには次の使用法があります。

使用法: `winget install --manifest \<file>`

| オプション  | 説明 |
|-------------|-------------|  
|  **-m、--manifest** | インストールするアプリケーションのマニフェストへのパス。 |

### <a name="log-files"></a>ログ ファイル

リダイレクトされない限り、winget のログ ファイルは次のフォルダーに配置されます: **\%temp%\\AICLI\\*.log**

## <a name="related-topics"></a>関連トピック

* [winget ツールを使用したアプリケーションのインストールと管理](index.md)
