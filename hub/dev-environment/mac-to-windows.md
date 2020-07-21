---
title: Mac (Unix) から Windows への移行のサポート
description: Mac (Unix) から Windows 開発環境への移行に役立つガイドで、ショートカット キーのマッピングや Mac と Windows で異なる概念の簡単な概要を示します。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
ms.technology: windows-nodejs
keywords: Mac から Windows、ショートカット キーのマッピング、Unix から Windows への移行、Mac から Windows への移行、MacBook から Surface への移行のサポート、Macintosh ユーザーの Windows の使用方法、Macintosh から Windows への切り替え、開発環境の変更のサポート、Mac OS X から Windows へ、Mac から PC への移行のサポート
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: 457abcec97247afcc0d63c983c8a6cda2de51c66
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/29/2020
ms.locfileid: "81643699"
---
# <a name="guide-for-changing-your-dev-environment-from-mac-to-windows"></a>開発環境を Mac から Windows に変更するためのガイド

次のヒントと同等のコントロールは、Mac と Windows (または WSL/Linux) 間の開発環境の移行で役立つはずです。

アプリ開発で、Xcode に最も近いものが [Visual Studio](https://visualstudio.microsoft.com) になります。 また、元に戻る必要があると思う場合は、[Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/) のバージョンもあります。 クロスプラットフォーム ソース コード編集 (および多数のプラグイン) には、[Visual Studio Code](https://code.visualstudio.com/?wt.mc_id=DX_841432) が最も一般的な選択肢です。

## <a name="keyboard-shortcuts"></a>キーボード ショートカット

| **操作** | **Mac** | **Windows** |
|---------------|--------------------|---------------------|
| コピー | Command + C | Ctrl+C |
| ［切り取り］ | Command + X | Ctrl + X |
| 貼り付け | Command + V | Ctrl + V |
| 元に戻す | Command + Z | Ctrl + Z |
| 上書き保存 | Command + S | Ctrl + S |
| 開く | Command + O | Ctrl + O |
| コンピューターのロック | Command + Control + Q | WindowsKey + L |
| デスクトップの表示 | Command + F3 | WindowsKey + D |
| ファイル ブラウザーを開く | Command + N | WindowsKey + E |
| ウィンドウの最小化 | Command + M | WindowsKey + M |
| 検索 | Command + Space | WindowsKey |
| アクティブ ウィンドウを閉じる | Command + W | Control + W |
| 現在のタスクの切り替え | Command + Tab | Alt + Tab |
| ウィンドウの全画面表示への最大化 | Control + Command + F | WindowsKey + Up |
| 画面の保存 (スクリーンショット) | Command + Shift + 3 | WindowsKey + Shift + S |
| ウィンドウの保存 | Command + Shift + 4 | WindowsKey + Shift + S |
| 項目の情報やプロパティの表示 | Command + I | Alt + Enter |
 | すべての項目の選択 | Command + A | Ctrl + A |
| リスト内の複数の項目の選択 (非連続) | Command、次に各項目をクリック | Control、次に各項目をクリック |
| 特殊文字の入力 | Option + 文字キー | Alt + 文字キー|

## <a name="trackpad-shortcuts"></a>トラックパッドのショートカット

注: これらのショートカットの一部では、Surface デバイスのトラックパッドやその他のサード パーティ製ノート PC など、"高精度のトラックパッド" が必要です。

 **操作** | **Mac** | **Windows** |
|---------------|--------------------|---------------------|
| Scroll | 2 本指での縦方向のスワイプ | 2 本指での縦方向のスワイプ |
| ズーム | 2 本指でのピンチインとピンチアウト | 2 本指でのピンチインとピンチアウト |
| ビューを前後にスワイプ | 2 本指での横方向のスワイプ | 2 本指での横方向のスワイプ |
| 仮想ワークスペースの切り替え | 4 本指での横方向のスワイプ | 4 本指での横方向のスワイプ |
| 現在開いているアプリの表示 | 4 本指での上方向のスワイプ | 3 本指での上方向のスワイプ |
| アプリの切り替え | なし | ゆっくりした 3 本指での横方向のスワイプ |
| デスクトップへの移動 | 4 本指を広げる | 3 本指での下方向へのスワイプ |
| Cortana/アクション センターを開く | 2 本指での右からのスライド | 3 本指でのタップ |
| 追加情報を開く | 3 本指でのタップ | なし |
|スタートパッドの表示/アプリの起動 | 4 本指でのピンチ | 4 本指でのタップ |

注: トラックパッド オプションは、両方のプラットフォームで構成できます。

## <a name="terminal-and-shell"></a>ターミナルとシェル

Windows には、Mac のターミナル エミュレーターに代わるいくつかの手段が用意されています。

1. Windows コマンド ライン

Windows コマンド ラインは DOS コマンドを受け付け、Windows で最も一般的に使われているコマンドライン ツールです。 それを開くには:**WindowsKey + R** を押して、 **[ファイル名を指定して実行]** ボックスを開き、「**cmd**」と入力して、 **[OK]** をクリックします。 管理者コマンド ラインを開くには、「**cmd**」と入力し、**Ctrl + Shift + Enter** キーを押します。

2. PowerShell

[Powershell](https://docs.microsoft.com/powershell/scripting/overview?view=powershell-6) は、"PowerShell は、.NET 上に構築されたタスクベースのコマンド ライン シェルおよびスクリプト言語です。 PowerShell は、システム管理者やパワーユーザーが、オペレーティング システムを管理するタスクを速やかに自動化するのに役立ちます"。 つまり、きわめて強力なコマンド ラインであり、特にシステム管理者に好まれています。

ちなみに、PowerShell は [Mac でも使用できます](https://docs.microsoft.com/powershell/scripting/install/installing-powershell-core-on-macos?view=powershell-6)。

3. Windows Subsystem for Linux (WSL)

WSL を使用すると、Windows 内で Linux シェルを実行できます。 つまり、選択またはインストールされている特定の Linux ディストリビューションに応じて、**bash** やその他のシェルを実行できます。 WSL を使用すると、Mac ユーザーに最もなじみがある種類の環境を提供できます。 たとえば、現在のディレクトリ内のファイルを一覧表示するには、Windows コマンド ラインでのように **dir** ではなく、**ls** を実行します。 WSL のインストールと使用の詳細については、「[Windows 10 用 Windows Subsystem for Linux のインストール ガイド](https://docs.microsoft.com/windows/wsl/install-win10)」を参照してください。

4. Windows ターミナル (プレビュー)

Windows ターミナルは、従来の Windows コマンド ライン、PowerShell、Linux 用 Windows サブシステムなど、さまざまなソースのコマンドライン ツールとシェルを組み合わせたアプリケーションです。 現在はまだプレビュー段階にありますが、複数のタブ、分割ペイン、カスタム テーマとスタイル、完全な Unicode のサポートなど、いくつかの便利な機能が既に含まれています。 Windows ターミナルは、[Windows 10 の Microsoft Store](https://www.microsoft.com/en-us/p/windows-terminal-preview/9n0dx20hk701?activetab=pivot:overviewtab) からインストールできます。

## <a name="apps-and-utilities"></a>アプリとユーティリティ

 **アプリ** | **Mac** | **Windows** |
|---------------|--------------------|---------------------|
| 設定と基本設定 | システム設定 | Settings |
| タスク マネージャー | アクティビティ モニター | タスク マネージャー |
| ディスク フォーマット | ディスク ユーティリティ | ディスクの管理 |
| テキスト編集 | TextEdit | メモ帳 |
| イベント表示 | コンソール | イベント ビューアー |
| ファイル/アプリの検索 | Command + Space | Windows キー |
