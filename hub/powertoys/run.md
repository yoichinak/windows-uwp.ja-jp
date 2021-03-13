---
title: Windows 10 用の Powertoy Run ユーティリティ
description: パフォーマンスを犠牲にすることなく、いくつかの追加機能を含むパワーユーザー向けのクイックランチャーです。
ms.date: 12/02/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: ce71ac5f4667952be8beb790b0890aadd0d8eb54
ms.sourcegitcommit: a1b251971f7ac574275d53bbe3e9ef4a3a9dc15c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/12/2021
ms.locfileid: "103417113"
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

## <a name="settings"></a>Settings

次の実行オプションは、[Powertoy の設定] メニューで使用できます。

  | **[設定]** |**操作** |
  | --- | --- |
  | Powertoy の実行を開く | Powertoy の実行を開く/非表示にするキーボードショートカットを定義する |
  | 全画面モードでショートカットを無視する |  全画面表示 (F11) では、を実行してもショートカットが表示されません。 |
  | 結果の最大数 |  スクロールせずに表示される結果の最大数 |
  | 起動時に上記のクエリをクリアする | 起動すると、以前の検索は強調表示されません |
  | ドライブ検出の無効化の警告 | すべてのドライブのインデックスが作成されていない場合、警告は表示されなくなります。 |

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

これらの既定のアクティブ化フレーズは、Powertoy をターゲットプラグインのみに強制的に実行します。

  | **アクションキー** | **操作** |
  | --- | --- |
  | `=` | 電卓のみ。 例 `=2+2` |
  | `?` | ファイルの検索のみ。 `?road`検索する例`roadmap.txt` |
  | `.` | インストールされているプログラムのみ。 `.code`Visual Studio Code を取得する例。 プログラムの起動時にパラメーターを追加する方法については、「 [プログラムパラメーター](#program-parameters) 」を参照してください。 |
  | `//` | Url のみ。 `//docs.microsoft.com`既定のブラウザーを使用する例https://docs.microsoft.com |
  | `<` | 実行中のプロセスのみ。 `<outlook`Outlook を含むすべてのプロセスを検索する例 |
  | `>` | Shell コマンドのみ。 `>ping localhost`Ping クエリを実行する例 |
  | `:` | レジストリキーのみ。 `:hkcu`HKEY_CURRENT_USER レジストリキーを検索する例 |
  | `!` | Windows サービスのみ。 `!alg`開始または停止するアプリケーションレイヤーゲートウェイサービスを検索する例 |

## <a name="system-commands"></a>システムコマンド

Powertoy を実行すると、実行可能な一連のシステムレベルのアクションが有効になります。

  | **アクションキー**   |   **操作** |
  | ------------------ | ---------------------------------------------------------------------------------|
  | `Shutdown` | コンピューターをシャットダウンします。 |
  | `Restart` | コンピューターを再起動します |
  | `Sign Out` | 現在のユーザーをサインアウトします |
  | `Lock` | コンピューターをロックします |
  | `Sleep` | コンピューターをスリープ状態にする |
  | `Hibernate` | コンピューターを休止状態にする |
  | `Empty Recycle Bin` | ごみ箱を空にします |

## <a name="plugin-manager"></a>プラグインマネージャー

Powertoy v 0.33 とをオンにすると、[Powertoy 実行設定] メニューには、現在使用可能なさまざまなプラグインを有効または無効にするプラグインマネージャーが含まれます。 セクションを選択して展開することで、各プラグインで使用されるアクティベーション語句をカスタマイズできます。 また、プラグインがグローバル結果に表示されるかどうかを選択したり、使用可能な場合は追加のプラグインオプションを設定したりすることもできます。 

## <a name="program-parameters"></a>プログラムのパラメーター

Powertoy v 0.33 以降では、Powertoy Run プログラムプラグインを使用して、アプリケーションを起動するときにプログラム引数を追加できます。 プログラムの引数は、プログラムのコマンドラインインターフェイスで定義されているように、必要な形式に従う必要があります。

たとえば、Visual Studio Code を起動するときに、次のようにして開くフォルダーを指定できます。

`Visual Studio Code -- C:\myFolder`

また Visual Studio Code は、 [コマンドラインパラメーター](https://code.visualstudio.com/docs/editor/command-line)のセットもサポートしています。このパラメーターは、powertoy の対応する引数と共に使用できます。たとえば、ファイル間の違いを確認できます。

`Visual Studio Code -d C:\foo.txt C:\bar.txt` 

プログラムプラグインの [グローバルな結果に含める] オプションが選択されていない場合は、 `.` 既定では、プラグインの動作を呼び出すためにアクティベーション語句を含めてください。

`.Visual Studio Code -- C:\myFolder`

## <a name="windows-search-settings"></a>Windows Search の設定

Windows Search プラグインがすべてのドライブをカバーするように設定されていない場合は、次の警告が表示されます。

![Powertoy 実行インデクサー警告](../images/pt-run-warning.png)

Windows Search の Powertoy の [プラグインマネージャーの実行] オプションで警告をオフにするか、警告を選択して、インデックスを作成するドライブを拡張することができます。 警告を選択すると、Windows 10 の設定の [Windows の検索] オプションメニューが開きます。

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
