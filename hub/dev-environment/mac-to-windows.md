---
title: Mac (Unix) から Windows への移行に関するヘルプ
description: Mac (Unix) から Windows 開発環境への移行に役立つガイドです。これには、ショートカットキーマッピングや、Mac と Windows で異なる概念の簡単な概要が含まれます。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
ms.technology: windows-nodejs
keywords: Mac から Windows、ショートカットキーのマッピング、Unix から Windows への移行、Mac から Windows への移行、Macintosh ユーザー用の Windows の使用、開発環境の変更、Windows への Mac OS X、ヘルプを参照してください。Mac から PC への移行
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: 8c23fa3e6791a3cd78d259b40e68606a30fd9395
ms.sourcegitcommit: c660def841abc742600fbcf6ed98e1f4f7beb8cc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/24/2020
ms.locfileid: "80218442"
---
# <a name="guide-for-changing-your-dev-environment-from-mac-to-windows"></a>開発環境を Mac から Windows に変更するためのガイド

次のヒントと制御に相当するものが、Mac と Windows (または WSL/Linux) 開発環境の移行に役立ちます。

アプリの開発では、Xcode と最も近いものが[Visual Studio](https://visualstudio.microsoft.com)になります。 また、前に戻る必要があると思われる場合は、 [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/)のバージョンもあります。 クロスプラットフォームのソースコード編集 (および多数のプラグイン) の場合[Visual Studio Code](https://code.visualstudio.com/?wt.mc_id=DX_841432)は最も一般的な選択肢です。

## <a name="keyboard-shortcuts"></a>キーボード ショートカット

| **運用** | **Mac** | **Windows** |
|---------------|--------------------|---------------------|
| コピー | コマンド + C | Ctrl+C |
| 切り取り | コマンド + X | Ctrl+X |
| 貼り付け | コマンド + V | Ctrl+V |
| 元に戻す | コマンド + Z | Ctrl+Z |
| 保存 | コマンド + S | Ctrl + S |
| 開く | コマンド + O | Ctrl + O |
| コンピューターのロック | コマンド + コントロール + Q | WindowsKey + L |
| デスクトップを表示する | Command + F3 | WindowsKey + D |
| ファイルブラウザーを開く | コマンド + N | WindowsKey + E |
| ウィンドウを最小化する | コマンド + M | WindowsKey + M |
| 検索 | コマンド + スペース | WindowsKey |
| アクティブウィンドウを閉じる | コマンド + W | Ctrl + W |
| 現在のタスクの切り替え | コマンド + Tab | Alt + Tab |
| ウィンドウを全画面表示に最大化 | Ctrl + Command + F | WindowsKey + Up |
| 画面の保存 (スクリーンショット) | コマンド + Shift + 3 | WindowsKey + Shift + S |
| ウィンドウの保存 | コマンド + Shift + 4 | WindowsKey + Shift + S |
| 項目の情報またはプロパティを表示する | コマンド + I | Alt + Enter |
 | [すべての項目を選択] | コマンド + A | Ctrl&lt;/localizedText&gt; + &lt;localizedText&gt;A |
| リスト内の複数の項目を選択する (非連続) | をクリックし、各項目をクリックします。 | コントロールをクリックし、各項目をクリックします。 |
| 特殊文字の入力 | Option + 文字キー | Alt + 文字キー|

## <a name="trackpad-shortcuts"></a>トラックパッドのショートカット

注: これらのショートカットの中には、Surface デバイスのトラックパッドや他のサードパーティ製のラップトップなど、"Precision トラックパッド" が必要です。

 **運用** | **Mac** | **Windows** |
|---------------|--------------------|---------------------|
| Scroll | 2本の指の垂直方向のスワイプ | 2本の指の垂直方向のスワイプ |
| ズーム | 2本の指のピンチインと out | 2本の指のピンチインと out |
| ビューを前後にスワイプする | 2本の横方向のスワイプ | 2本の横方向のスワイプ |
| 仮想ワークスペースの切り替え | 4本の横方向のスワイプ | 4本の横方向のスワイプ |
| 現在開いているアプリを表示する | 4本の上向きスワイプ | 3本の上向きスワイプ |
| アプリを切り替える | N/A | 3本の横方向のスワイプが遅い |
| デスクトップにアクセス | 4本の指を拡散する | 指を下に3本スワイプ |
| Cortana/アクションセンターを開く | 右から2本の指スライド | 3本の指タップ |
| 追加情報を開く | 3本の指タップ | N/A |
|スタートパッドの表示/アプリの開始 | 4本の指を使用したピンチ | 4本の指でタップ |

注: トラックパッドオプションは、両方のプラットフォームで構成できます。

## <a name="terminal-and-shell"></a>ターミナルとシェル

Windows には、Mac のターミナルエミュレーターに代わるいくつかの代替手段が用意されています。

1. Windows コマンドライン

Windows のコマンドラインは DOS コマンドを受け取り、Windows で最も一般的に使用されるコマンドラインツールです。 開くには、 **Windowskey + R**キーを押して [ファイル名を選択して**実行**] ボックスを開き、「 **cmd** 」と入力して、[ **OK]** をクリックします。 管理者のコマンドラインを開くには、「 **cmd** 」と入力し、 **ctrl + Shift + enter**キーを押します。

2. PowerShell

[Powershell は](https://docs.microsoft.com/powershell/scripting/overview?view=powershell-6)、"powershell は、.net 上に構築されたタスクベースのコマンドラインシェルおよびスクリプト言語です。 PowerShell を使用すると、システム管理者やパワーユーザーは、オペレーティングシステムを管理するタスクを迅速に自動化できます。 つまり、非常に強力なコマンドラインであり、特にシステム管理者にとっては非常に便利です。

ちなみに、Mac で[も PowerShell を使用でき](https://docs.microsoft.com/powershell/scripting/install/installing-powershell-core-on-macos?view=powershell-6)ます。

3. Windows Subsystem for Linux (WSL)

WSL を使用すると、Linux シェルを Windows 内で実行できます。 これは、インストールされている特定の Linux ディストリビューションに応じて、 *bash** またはその他のシェルを実行できることを意味します。 WSL を使用すると、Mac ユーザーに最もなじみのある環境を提供できます。 たとえば、Windows のコマンドラインの場合**と同じ**ように、現在のディレクトリ内のファイルを一覧**表示すること**ができます。 WSL のインストールと使用の詳細については、windows [10 用 Windows Subsystem For Linux のインストール](https://docs.microsoft.com/windows/wsl/install-win10)に関するガイドを参照してください。

4. Windows ターミナル (プレビュー)

Windows ターミナルは、従来の Windows コマンドライン、PowerShell、Windows Subsystem for Linux など、さまざまなソースのコマンドラインツールとシェルを組み合わせるアプリケーションです。 現在はプレビュー段階にありますが、複数のタブ、分割ペイン、カスタムテーマとスタイル、完全な Unicode のサポートなど、いくつかの便利な機能が alreaedy に含まれています。 Windows ターミナルは、 [windows 10 の Microsoft Store](https://www.microsoft.com/en-us/p/windows-terminal-preview/9n0dx20hk701?activetab=pivot:overviewtab)からインストールできます。

## <a name="apps-and-utilities"></a>アプリとユーティリティ

 **アプリケーション** | **Mac** | **Windows** |
|---------------|--------------------|---------------------|
| 設定と基本設定 | システム設定 | 設定 |
| タスク マネージャー | アクティビティ モニター | タスク マネージャー |
| ディスクのフォーマット | ディスクユーティリティ | ディスクの管理 |
| テキスト編集 | TextEdit | メモ帳 |
| イベントの表示 | Console | イベント ビューアー |
| ファイル/アプリの検索 | コマンド + スペース | Windows キー |
