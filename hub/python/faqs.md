---
title: Windows での Python の使用に関してよく寄せられる質問
description: Windows での Python の使用に関してよく寄せられる質問
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: python、windows 10、microsoft、pip、.py、ファイルパス、python パス、python デプロイ、python パッケージ
ms.localizationpriority: medium
ms.date: 07/19/2019
ms.openlocfilehash: 4132ef0089ee707367666b4d6340333e538b1130
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/15/2019
ms.locfileid: "72313377"
---
# <a name="frequently-asked-questions-about-using-python-on-windows"></a>Windows での Python の使用に関してよく寄せられる質問

## <a name="why-cant-i-pip-install-a-certain-package"></a>特定のパッケージを "pip インストール" できないのはなぜですか。

インストールが失敗する理由は多数あります。ほとんどの場合、パッケージ開発者に連絡することが適切なソリューションです。

問題の最も一般的な原因として、変更するアクセス許可がない場所にをインストールしようとしていることが考えられます。 たとえば、既定のインストール場所には管理者特権が必要になる場合がありますが、既定では Python には含まれません。 最適なソリューションは、仮想環境を作成してインストールすることです。

一部のパッケージには、のインストールに C C++またはコンパイラを必要とするネイティブコードが含まれています。 一般に、パッケージ開発者はプリコンパイル済みのバージョンを発行する必要がありますが、多くの場合はそうではありません。 [Build Tools For Visual Studio をインストール](https://visualstudio.microsoft.com/downloads/#build-tools-for-visual-studio-2019)してC++オプションを選択すると、これらのパッケージの一部が機能する場合がありますが、ほとんどの場合、パッケージ開発者に連絡する必要があります。

[StackOverflow に関する説明に従っ](https://stackoverflow.com/questions/4750806/how-do-i-install-pip-on-windows/12476379)てください。

## <a name="what-is-pyexe"></a>.Py とは何ですか。

さまざまな種類の Python プロジェクトで作業しているため、コンピューターに複数のバージョンの Python がインストールされている可能性があります。 これらはすべて `python` コマンドを使用するため、使用している Python のバージョンがわからないことがあります。 標準として、`python3` コマンドを使用することをお勧めします (または `python3.7` で特定のバージョンを選択します)。

[.Py ランチャー](https://docs.python.org/3/using/windows.html#launcher)によって、インストールした Python の最新バージョンが自動的に選択されます。 また、`py -3.7` のようなコマンドを使用して特定のバージョンを選択することも、-1 を @no__t して、使用できるバージョンを確認することもできます。 **ただし**、.py ランチャーは、 [python.org](https://www.python.org/downloads/windows/)からインストールされたバージョンの Python を使用している場合にのみ機能します。Microsoft Store から Python をインストールする場合、`py` コマンドは**含まれません**。 Linux、macOS、WSL、および Microsoft Store バージョンの Python では、`python3` (または `python3.7`) コマンドを使用する必要があります。

## <a name="why-does-running-pythonexe-open-the-microsoft-store"></a>Microsoft Store を開いているのはなぜですか。

新しいユーザーが適切に Python をインストールできるように、Windows へのショートカットを追加しました。これにより、Microsoft Store で公開されているコミュニティのパッケージの最新バージョンに直接移動できます。 このパッケージは、管理者のアクセス許可がなくても簡単にインストールでき、既定の `python` および `python3` のコマンドは実際のものに置き換えられます。

コマンドライン引数を指定してショートカット実行可能ファイルを実行すると、Python がインストールされていないことを示すエラーコードが返されます。 これは、意図していない場合に、バッチファイルやスクリプトがストアアプリを開けないようにするためです。

[Python.org](https://www.python.org/downloads/windows/)のインストーラーを使用して Python をインストールし、[パスに追加] オプションを選択すると、新しい `python` コマンドがショートカットよりも優先されます。 他のインストーラーでは、組み込みのショートカットよりも_低い_優先順位で `python` が追加される場合があることに注意してください。

Python をインストールせずにショートカットを無効にするには、スタートから "アプリの実行エイリアスの管理" を開き、"アプリインストーラー" Python エントリを検索して "Off" に切り替えます。

## <a name="why-dont-file-paths-work-in-python-when-i-copy-paste-them"></a>コピーして貼り付けると Python でファイルパスが動作しないのはなぜですか。

Python 文字列では、特殊文字に "エスケープ" を使用します。 たとえば、文字列に新しい行文字を挿入するには、「`\n`」と入力します。 Windows 上のファイルパスでは円記号が使用されるため、一部の部分は特殊文字に変換される場合があります。

Python でパスを文字列として貼り付けるには、@no__t 0 プレフィックスを追加します。 これは @no__t 0 の文字列であることを示し、\ "以外のエスケープ文字は使用されません (パスの最後の円記号を削除することが必要になる場合があります)。 この場合、パスは r "C:\Users\MyName\Documents\Document.txt" のようになります。

Python でパスを操作する場合は、標準の pathlib モジュールを使用することをお勧めします。 これにより、文字列をリッチパスオブジェクトに変換し、スラッシュまたは円記号を使用するかどうかにかかわらずパス操作を一貫させることができます。これにより、コードがさまざまなオペレーティングシステムで動作しやすくなります。

## <a name="what-is-pythonpath"></a>PYTHON パスとは

Python PATH 環境変数は、モジュールをインポートできるディレクトリの一覧を指定するために Python によって使用されます。 を実行している場合は、@no__t 0 の変数を調べて、何かをインポートするときに検索されるディレクトリを確認できます。

この変数をコマンドプロンプトから設定するには、: `set PYTHONPATH=list;of;paths` を使用します。

この変数を PowerShell から設定するには、Python を起動する直前に: `$env:PYTHONPATH=’list;of;paths’` を使用します。

**環境変数**の設定を使用してグローバルにこの変数を設定することは推奨され**ません**。これは、使用する予定のバージョンではなく、任意のバージョンの Python で使用される可能性があるためです。

## <a name="where-can-i-find-help-with-packaging-and-deployment"></a>パッケージ化と展開に関するヘルプはどこで入手できますか。

[Docker](https://code.visualstudio.com/docs/azure/docker):[Vscode 拡張機能](https://code.visualstudio.com/docs/azure/docker)を使用すると、Dockerfile と docker-compose.yml テンプレートを使用して簡単にパッケージ化して配置することができます (プロジェクトの適切な docker ファイルを生成します)。

[Azure Kubernetes Service (AKS)](https://docs.microsoft.com/azure/aks/)を使用すると、必要に応じてリソースを拡張しながら、コンテナー化されたアプリケーションをデプロイして管理することができます。

## <a name="what-if-i-need-to-work-across-different-machines"></a>複数のコンピューターで作業する必要がある場合はどうすればよいですか。

[設定の同期](https://marketplace.visualstudio.com/items?itemName=Shan.code-settings-sync)を使用すると、GitHub を使用して、さまざまなインストール間で VS Code 設定を同期できます。 別のコンピューターで作業している場合は、これによって環境の整合性を保つことができます。

## <a name="what-if-im-used-to-using-pycharm-atom-sublime-text-emacs-or-vim"></a>PyCharm、Atom、Sublime テキスト、Emacs、Vim を使用する場合はどうすればよいですか。

VSCode 拡張機能の[Keymaps](https://marketplace.visualstudio.com/search?target=VSCode&category=Keymaps&sortBy=Downloads)を使用すると、環境をホームにすることができます。

## <a name="how-do-mac-shortcut-keys-map-to-windows-shortcut-keys"></a>Mac のショートカットキーを Windows ショートカットキーにマップするにはどうすればいいですか。

一部のキーボードボタンとシステムショートカットは、Windows コンピューターと Macintosh では若干異なります。 この[Mac から Windows への移行ガイド](../dev-environment/mac-to-windows.md)では、基本について説明します。
