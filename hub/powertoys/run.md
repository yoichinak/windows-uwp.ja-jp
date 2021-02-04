---
title: Windows 10 用の Powertoy Run ユーティリティ
description: パフォーマンスを犠牲にすることなく、いくつかの追加機能を含むパワーユーザー向けのクイックランチャーです。
ms.date: 12/02/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 126c38cd98f0d8ff1102c7f53f14cb95ec7e38c5
ms.sourcegitcommit: 382ae62f9d9bf980399a3f654e40ef4f85eae328
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/04/2021
ms.locfileid: "99534401"
---
# <a name="powertoys-run-utility"></a>Powertoy 実行ユーティリティ

Powertoy の実行は、パフォーマンスを犠牲にせずにいくつかの追加機能を含むパワーユーザー向けのクイックランチャーです。 これはオープン ソースであり、追加のプラグイン用のモジュラーです。

Powertoy の実行を使用するには、[ <kbd>Alt</kbd>Space] を選択し、「 + <kbd></kbd> 」と入力します。

*そのショートカットが気に入っていない場合は、設定で完全に構成できます。*

![Powertoy でアプリを開くデモを実行する](../images/pt-powerrun-demo.gif)

## <a name="requirements"></a>必要条件

- Windows 10 バージョン 1903 以降
- をインストールした後、このユーティリティが動作するには、Powertoy が有効になっていて、バックグラウンドで実行されている必要があります。

## <a name="features"></a>特徴

Powertoy の実行機能は次のとおりです。

- アプリケーション、フォルダー、またはファイルの検索

- 実行中のプロセスを検索します (以前は [Windowwalker](https://github.com/betsegaw/windowwalker/)と呼ばれていました)

- キーボードショートカットが表示されているクリック可能なボタン ( *[管理者として開く* ] や *[フォルダーを含むフォルダーを開く*] など)

- を使用してシェルプラグインを起動し `>`  ます (たとえば、 `> Shell:startup` Windows スタートアップフォルダーを開きます)

- 電卓を使用して簡単な計算を実行する

## <a name="settings"></a>設定

次の実行オプションは、[Powertoy の設定] メニューで使用できます。

  | **[設定]** |**操作** |
  | --- | --- |
  | Powertoy の実行を開く | Powertoy の実行を開く/非表示にするキーボードショートカットを定義する |
  | 全画面モードでショートカットを無視する |  全画面表示 (F11) では、を実行してもショートカットが表示されません。 |
  | 結果の最大数 |  スクロールせずに表示される結果の最大数 |
  | 起動時に上記のクエリをクリアする | 起動すると、以前の検索は強調表示されません |
  | ドライブ検出の無効化の警告 | すべてのドライブのインデックスが作成されていない場合、警告が表示されなくなります。 |

## <a name="keyboard-shortcuts"></a>キーボード ショートカット

  | **ショートカット** | **操作** |
  | --- | --- |
  | Alt + Space | Powertoy の実行を開く、または非表示にする |
  | Esc | Powertoy の実行の非表示 |
  | Ctrl + Shift + Enter | (アプリケーションにのみ適用)選択したアプリケーションを管理者として開きます |
  | Ctrl + Shift + E | (アプリケーションとファイルにのみ適用)エクスプローラーでフォルダーを開く |
  | Ctrl+C | (フォルダーとファイルにのみ適用)パスの場所のコピー |
  | タブ | 検索結果とコンテキストメニューボタンの間を移動します。 |

## <a name="action-key"></a>アクションキー

これにより、Powertoy はターゲットプラグインのみに強制的に実行されます。

  | **アクションキー** | **操作** |
  | --- | --- |
  | `=` | 電卓のみ。 例 `=2+2` |
  | `?` | ファイルの検索のみ。 `?road`検索する例`roadmap.txt` |
  | `.` | インストールされているアプリの検索のみ。 `.code`Visual Studio Code を取得する例 |
  | `//` | Url のみ。 `//docs.microsoft.com`既定のブラウザーを使用する例https://docs.microsoft.com |
  | `<` | 実行中のプロセスのみ。 `<outlook`Outlook を含むすべてのプロセスを検索する例 |
  | `>` | Shell コマンドのみ。 `>ping localhost`Ping クエリを実行する例 |
  | `:` | レジストリキーのみ。 `:hkcu`HKEY_CURRENT_USER レジストリキーを検索する例 |
  | `!` | Windows サービスのみ。 `!alu`開始または停止するアプリケーションレイヤーゲートウェイサービスを検索する例 |

## <a name="system-commands"></a>システムコマンド

Powertoy v 0.31 および on では、現在実行できるシステムレベルのアクションがあります。

  | **アクションキー**   |   **操作** |
  | ------------------ | ---------------------------------------------------------------------------------|
  | `Shutdown` | コンピューターをシャットダウンします。 |
  | `Restart` | コンピューターを再起動します |
  | `Sign Out` | 現在のユーザーをサインアウトします |
  | `Lock` | コンピューターをロックします |
  | `Sleep` | コンピューターをスリープ状態にする |
  | `Hibernate` | コンピューターを休止状態にする |
  | `Empty Recycle Bin` | ごみ箱を空にします |

## <a name="indexer-settings"></a>インデクサーの設定

インデクサー設定がすべてのドライブに対応するように設定されていない場合は、次の警告が表示されます。

![Powertoy 実行インデクサー警告](../images/pt-run-warning.png)

[Powertoy] 設定の警告をオフにするか、警告を選択して、インデックスを作成するドライブを拡張することができます。 警告を選択すると、Windows 10 の設定の [Windows の検索] オプションが開きます。

![インデックス設定](../images/pt-run-indexing.png)

この [Windows の検索] メニューでは、次のことができます。

- Windows 10 コンピューターのすべてのドライブでインデックスを作成できるようにするには、[拡張] モードを選択します。
- 除外するフォルダーのパスを指定します。
- 詳細なインデックス設定の設定、検索場所の追加または削除、暗号化されたファイルのインデックス作成などを行うには、メニューオプションの下部の近くにある [高度な検索インデクサー設定] を選択します。

![詳細なインデックス設定](../images/pt-run-indexing-advanced.png)

## <a name="known-issues"></a>既知の問題

既知の問題と提案事項の一覧については、 [GitHub の「powertoy 製品リポジトリに関する問題](https://github.com/microsoft/PowerToys/issues?q=is%3Aopen+is%3Aissue+label%3AProduct-Launcher)」を参照してください。

## <a name="attribution"></a>Attribution

- [Wox](https://github.com/Wox-launcher/Wox/)

- [ベータ Tadele のウィンドウウォーカー](https://github.com/betsegaw/windowwalker)
