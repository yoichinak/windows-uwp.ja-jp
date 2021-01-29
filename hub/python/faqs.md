---
title: Windows での Python の使用についてよく寄せられる質問
description: 開発のための Windows での Python の使用についてよく寄せられる質問 (FAQ) に対する回答を確認することにより、役立つ情報が得られます。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: python, windows 10, microsoft, pip, py.exe, ファイル パス, PYTHONPATH, python 開発, python パッケージ化
ms.localizationpriority: medium
ms.date: 07/19/2019
ms.openlocfilehash: c1cada0fef5968846100f66bb41b3dd70ea5b59a
ms.sourcegitcommit: 8040760f5520bd1732c39aedc68144c4496319df
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/22/2021
ms.locfileid: "98691307"
---
# <a name="frequently-asked-questions-about-using-python-on-windows"></a>Windows での Python の使用についてよく寄せられる質問

## <a name="trouble-installing-a-package-with-pip-install"></a>pip インストールを使用したパッケージのインストールに関する問題

インストールが失敗する理由は多数あります。多くの場合、パッケージ開発者に連絡することが適切な解決策です。

問題のよくある原因は、変更のアクセス許可がない場所にインストールしようとしていることです。 たとえば、既定のインストール場所に管理者特権が要求される場合がありますが、既定では Python にその特権がありません。 最適な解決策は、[仮想環境](./web-frameworks.md#create-a-virtual-environment)を作成してそこにインストールすることです。

一部のパッケージには、インストールするために C または C++ コンパイラが必要なネイティブ コードが含まれています。 一般的には、パッケージ開発者はプリコンパイル済みのバージョンを公開するべきですが、そのようにしない場合も多々あります。 これらのパッケージの一部については、[Visual Studio 用のビルド ツールをインストール](https://visualstudio.microsoft.com/downloads/#build-tools-for-visual-studio-2019)して C++ オプションを選択するとうまくいく場合がありますが、ほとんどの場合はパッケージ開発者に問い合わせる必要があります。

[StackOverflow に関するディスカッションをフォローしてください](https://stackoverflow.com/questions/4750806/how-do-i-install-pip-on-windows/12476379)

### <a name="trouble-installing-pip-with-wsl"></a>WSL での pip のインストールに関する問題

Linux 用 Windows サブシステム (WSL または WSL2) で pip を使用してパッケージ (Flask など) をインストールする場合 (例: `python3 -m pip install flask`)、次のようなエラーが発生することがあります。

```bash
WARNING: Retrying (Retry(total=4, connect=None, read=None, redirect=None, status=None))
after connection broken by 'NewConnectionError('<urllib3.connection.VerifiedHTTPSConnection
object at 0x7f655471da30>: Failed to establish a new connection: [Errno -3]
Temporary failure in name resolution')': /simple/flask/
```

この問題を調査するときに陥る可能性のある陥穽がいくつかあり、そのどれもが WSL Linux のディストリビューションでは特に非生産的なものです。 (警告: WSL で `resolv.conf` を編集しようとしないでください。このファイルはシンボリック リンクであり、変更すると厄介な問題を引き起こす可能性があります)。 カスタマイズしたファイアウォールを実行しているのでない限り、可能性のある解決策は単に pip を再インストールすることです。

```bash
sudo apt -y purge python3-pip
sudo python3 -m pip uninstall pip
sudo apt -y install python3-pip --fix-missing
```

* *詳細については、[GitHub の WSL 製品リポジトリ](https://github.com/microsoft/WSL/issues/4020)を参照してください。ドキュメントへの[このイシューに関する寄与](https://github.com/MicrosoftDocs/windows-uwp/issues/2679)について、ユーザー コミュニティに感謝します。*

## <a name="what-is-pyexe"></a>py.exe とは何ですか?

さまざまな種類の Python プロジェクトを扱っていると、複数のバージョンの Python がコンピューターにインストールされる場合があります。 これらはすべて `python` コマンドを使用するため、どのバージョンの Python を使用しているのかはっきりしない場合があります。 標準として、`python3` コマンド (または、特定のバージョンを選択するには `python3.7`) を使用することをお勧めします。

[py.exe ランチャー](https://docs.python.org/3/using/windows.html#launcher)は、インストールされている中で最も新しいバージョンの Python を自動的に選択します。 `py -3.7` などのコマンドを使用して特定のバージョンを選択したり、使用できるバージョンを `py --list` を使用して確認したりすることもできます。 **ただし**、py.exe ランチャーが正しく動作するのは、[python.org](https://www.python.org/downloads/windows/) からインストールしたバージョンの Python を使用している場合だけです。Microsoft Store から Python をインストールする場合、`py` コマンドは **含まれません**。 Linux、macOS、WSL、および Microsoft Store バージョンの Python の場合、`python3` (または `python3.7`) コマンドを使用する必要があります。

## <a name="why-does-running-pythonexe-open-the-microsoft-store"></a>python.exe を実行すると Microsoft Store が開くのはなぜですか?

新しいユーザーが Python の適切なインストールを見つけられるよう、Microsoft Store で公開されているコミュニティのパッケージの最新バージョンに直結したショートカットを Windows に追加しました。 このパッケージは、管理者のアクセス許可がなくても簡単にインストールでき、既定の `python` および `python3` コマンドを実際のものに置き換えます。

コマンドライン引数を指定してショートカットの実行可能ファイルを実行すると、Python がインストールされていないことを示すエラー コードが返されます。 これは、意図していない場合にバッチ ファイルおよびスクリプトによって Store アプリが開かれるのを防ぐためです。

[python.org](https://www.python.org/downloads/windows/) のインストーラーを使用して Python をインストールし、"PATH に追加" オプションを選択した場合、新しい `python` コマンドがショートカットよりも優先されます。 他のインストーラーは、組み込みのショートカットよりも _低い_ 優先度で `python` を追加する場合があることに注意してください。

Python をインストールせずにショートカットを無効にするには、[スタート] から [Manage app execution aliases] (アプリ実行エイリアスの管理) を開き、"App Installer" (アプリ インストーラー) Python エントリを見つけて "オフ" に切り替えます。

## <a name="why-dont-file-paths-work-in-python-when-i-copy-paste-them"></a>ファイル パスをコピーして貼り付けても Python で正しく動作しないのはなぜですか?

Python の文字列では、特殊文字に "エスケープ" を使用します。 たとえば、改行文字を文字列に挿入するには「`\n`」と入力します。 Windows のファイル パスでは円記号が使用されるため、一部の箇所が特殊文字に変換される場合があります。

Python でパスを文字列として貼り付けるには、`r` プレフィックスを追加します。 これは、貼り付けるものが `raw` 文字列であり、"\" 以外のエスケープ文字が使用されないことを示します (パス内の最後の円記号を削除することが必要な場合があります)。 そのため、パスは次のようになります。`r"C:\Users\MyName\Documents\Document.txt"`

Python でパスを扱うときは、標準の pathlib モジュールを使用することをお勧めします。 これにより、スラッシュと円記号のどちらを使用する場合でも一貫したパス操作を実行できるリッチな Path オブジェクトに文字列を変換できるため、オペレーティング システム間でのコードの可搬性が向上します。

## <a name="what-is-pythonpath"></a>PYTHONPATH とは何ですか?

PYTHONPATH 環境変数は、モジュールのインポート元にすることができるディレクトリの一覧を指定するために Python によって使用されます。 実行中に `sys.path` 変数を調べると、何かをインポートするときに検索されるディレクトリを確認できます。

コマンド プロンプトからこの変数を設定するには、`set PYTHONPATH=list;of;paths` を使用します。

PowerShell からこの変数を設定するには、Python を起動する直前に `$env:PYTHONPATH=’list;of;paths’` を使用します。

この変数は、使用する予定の Python だけでなく、すべてのバージョンの Python によって使用される可能性があるため、**[環境変数]** 設定でこの変数をグローバルに設定することは **非推奨** です。

## <a name="where-can-i-find-help-with-packaging-and-deployment"></a>パッケージ化と配置のヘルプはどこにありますか?

[Docker](https://code.visualstudio.com/docs/azure/docker):[VSCode 拡張機能](https://code.visualstudio.com/docs/azure/docker)を使用すると、Dockerfile および docker-compose.yml テンプレートを使用して迅速にパッケージ化および配置する (プロジェクトに適した Docker ファイルを生成する) ことができます。

[Azure Kubernetes Service (AKS)](/azure/aks/) を使用すると、コンテナー化されたアプリケーションを配置および管理しながら、必要に応じてリソースを規模拡張できます。

## <a name="what-if-i-need-to-work-across-different-machines"></a>複数のコンピューターにまたがって作業する必要がある場合はどうすればよいですか?

[[Settings Sync]\(設定の同期\)](https://marketplace.visualstudio.com/items?itemName=Shan.code-settings-sync) を使用すると、異なるインストール間で GitHub を使用して VS Code の設定を同期することができます。 複数のコンピューターで作業する場合、この機能によってコンピューター間で環境の一貫性を保つことができます。

## <a name="what-if-im-used-to-using-pycharm-atom-sublime-text-emacs-or-vim"></a>PyCharm、Atom、Sublime Text、Emacs、または Vim を使い慣れている場合、どうすればよいですか?

VSCode 拡張機能の [Keymaps](https://marketplace.visualstudio.com/search?target=VSCode&category=Keymaps&sortBy=Downloads) は、環境を自分好みにカスタマイズするために役立ちます。

## <a name="how-do-mac-shortcut-keys-map-to-windows-shortcut-keys"></a>Mac のショートカット キーは Windows のショートカット キーにどのように対応しますか?

Windows コンピューターと Macintosh では、いくつかのキーボード ボタンとシステム ショートカットに微妙な違いがあります。 こちらの [Mac から Windows への移行ガイド](../dev-environment/mac-to-windows.md)で基本を説明しています。
