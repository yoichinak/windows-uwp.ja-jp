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
ms.openlocfilehash: 1d94944037caf7cd909ea4799867f83bd4a6f887
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89157586"
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

## <a name="command-line-shells-and-terminals"></a>コマンドライン シェルとターミナル

Windows はいくつかのコマンドライン シェルとターミナルをサポートしていますが、Mac の BASH シェルや、Terminal や iTerm などのターミナル エミュレーター アプリとは動作が少し異なることがあります。

### <a name="windows-shells"></a>Windows のシェル

Windows には、2 つの主なコマンドライン シェルがあります。

1. **[PowerShell](/powershell/scripting/overview?view=powershell-7)** - PowerShell は、クロスプラットフォームのタスク自動化および構成管理フレームワークであり、.NET で構築されたコマンドライン シェルとスクリプト言語で構成されています。 PowerShell を使用することにより、管理者、開発者、パワー ユーザーは、複雑なプロセスを管理するタスクや、それが実行されている環境やオペレーティング システムのさまざまな側面を、迅速に制御および自動化することができます。 PowerShell は[完全にオープンソース](https://github.com/powershell/powershell)であり、またクロスプラットフォームであるため、[Mac と Linux でも利用できます](/powershell/scripting/install/installing-powershell?view=powershell-7)。

    **Mac および Linux の BASH シェル ユーザー**:PowerShell は、使い慣れた多くのコマンド エイリアスもサポートしています。 たとえば、次のように入力します。
    - 現在のディレクトリの内容を一覧表示する: `ls`
    - ファイルを移動する： `mv`
    - 新しいディレクトリに移動する: `cd <path>`

    コマンドと引数のいくらかの相違点が、PowerShell とBASH の間に存在します。 詳細については、PowerShell で [`get-help`](/powershell/scripting/learn/ps101/02-help-system?view=powershell-7) と入力するか、ドキュメントの中に含まれている、[互換性のあるエイリアス](/powershell/scripting/samples/appendix-1---compatibility-aliases?view=powershell-7)についての記事を参照してください。

    管理者として PowerShell を実行するには、Windows スタート メニューで "PowerShell" と入力し、[管理者として実行] を選択します。

2. **Windows コマンド ライン (Cmd)** :Windows には今でも従来のコマンド プロンプト (およびコンソール。下をご覧ください) が搭載されており、現在および従来の MS-DOS 互換コマンドとバッチ ファイルとの互換性が保たれています。 Cmd は、既存のまたは前のバッチ ファイルやコマンドラインの操作を実行する際に便利ですが、Cmd は今ではメンテナンスの状態にあり、将来的に改善や新しい機能が加えられることはないため、通常は、PowerShell を学んで使用するようユーザーにお勧めします。

### <a name="linux-shells"></a>Linux のシェル

Linux 用 Windows サブシステム (WSL) をインストールして、Windows 内で Linux シェルを実行できるようになりました。 このことは、選択した特定の Linux ディストリビューションを使用して、Windows 内で統合された **bash** を実行できることを意味しています。 WSL を使用すると、Mac ユーザーに最もなじみがある種類の環境を提供できます。 たとえば、現在のディレクトリ内のファイルを一覧表示するには、従来の Windows Cmd シェルで使用する **dir** ではなく、**ls** を実行できます。 WSL のインストールと使用の詳細については、「[Windows 10 用 Windows Subsystem for Linux のインストール ガイド](/windows/wsl/install-win10)」を参照してください。 WSL で Windows にインストールできる Linux ディストリビューションには、次のものがあります。

1. [Ubuntu 20.04 LTS](https://www.microsoft.com/store/apps/9n6svws3rx71)
2. [Kali Linux](https://www.microsoft.com/store/apps/9PKR34TNCV07)
3. [Debian GNU/Linux](https://www.microsoft.com/store/apps/9MSVKQC78PK6)
4. [openSUSE Leap 15.1](https://www.microsoft.com/store/apps/9NJFZK00FGKV)
5. [SUSE Linux Enterprise Server 15 SP1](https://www.microsoft.com/store/apps/9PN498VPMF3Z)

これは一部にすぎません。 詳細については [WSL のインストール ドキュメント](/windows/wsl/install-win10#install-your-linux-distribution-of-choice)を参照して、[Microsoft Store](https://www.microsoft.com/search/shop/apps?q=linux&category=Developer+tools) から直接インストールしてください。

## <a name="windows-terminals"></a>Windows のターミナル

多くのサード パーティ製品に加えて、Microsoft から 2 つの "ターミナル" が提供されています。コマンドラインのシェルとアプリケーションへのアクセスを提供する GUI アプリケーションです。

1. **[Windows ターミナル](/windows/terminal/)** :Windows ターミナルは高度な構成が可能な最新のコマンドライン ターミナル アプリケーションであり、高パフォーマンス、低遅延のコマンドライン ユーザー エクスペリエンス、複数のタブ、分割されたウィンドウ ペイン、カスタムのテーマとスタイル、異なるシェルまたはコマンドライン アプリ用の複数の "プロファイル"、さらにコマンドライン ユーザー エクスペリエンスのさまざまな側面を構成してカスタマイズするための非常に多くの機能を特徴としています。

    Windows ターミナルを使用して、PowerShell、WSL シェル (Ubuntu や Debian)、従来の Windows コマンド プロンプト、その他のコマンドライン アプリ (SSH、Azure CLI、Git Bash など) に接続されたタブを開くことができます。

2. **[コンソール](/windows/console/)** :Mac や Linux では、ユーザーは通常、まず好みのターミナル アプリケーションを開き、そこでユーザーの既定のシェル (BASH など) を作成して接続します。

    しかし、歴史の気まぐれのせいで、Windows ユーザーは伝統的に、シェルを開始して、その後 Windows から自動的に GUI コンソール アプリを開始して接続していました。

    今でもシェルを直接起動して従来の Windows コンソールを使用することは可能ですが、ユーザーには、Windows ターミナルをインストールして、極めて高速で生産性が高い、コマンドラインのベスト エクスペリエンスを活用することを強くお勧めします。

## <a name="apps-and-utilities"></a>アプリとユーティリティ

 **アプリ** | **Mac** | **Windows** |
|---------------|--------------------|---------------------|
| 設定と基本設定 | システム設定 | Settings |
| タスク マネージャー | アクティビティ モニター | タスク マネージャー |
| ディスク フォーマット | ディスク ユーティリティ | ディスクの管理 |
| テキスト編集 | TextEdit | メモ帳 |
| イベント表示 | コンソール | イベント ビューアー |
| ファイル/アプリの検索 | Command + Space | Windows キー |