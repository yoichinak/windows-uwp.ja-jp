---
Description: デスクトップ Win32 アプリがトースト通知を送信するためのさまざまなオプションについて説明します。
title: デスクトップ アプリからのトースト通知
label: Toast notifications from desktop apps
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10、uwp、win32、デスクトップ、トースト通知、デスクトップブリッジ、msix、スパースパッケージ、toasts の送信オプション、com サーバー、com アクティベーター、com、フェイク com、com なし、com を使用しない com、送信トースト
ms.localizationpriority: medium
ms.openlocfilehash: 478c478fa6892e4b61ac1a7d6e22089720e96ca7
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/24/2020
ms.locfileid: "91220074"
---
# <a name="toast-notifications-from-desktop-apps"></a>デスクトップ アプリからのトースト通知

デスクトップアプリ (パッケージ化された [Msix](/windows/msix/desktop/source-code-overview) アプリ、 [スパースパッケージ](/windows/apps/desktop/modernize/grant-identity-to-nonpackaged-apps) を使用してパッケージ id を取得するアプリ、および従来のパッケージ化されていない Win32 アプリを含む) は、Windows アプリと同様に対話型のトースト通知を送信できます。 ただし、異なるアクティブ化スキームのために、いくつかの異なるデスクトップ アプリのオプションがあります。

この記事では、Windows 10 でトースト通知の送信に使用するためのオプションを一覧表示します。 すべてのオプションでは、以下が完全にサポートされています。

* アクション センター内での保持
* ポップアップとアクション センター内の両方からアクティブ化可能
* EXE が実行されていないときにアクティブ化可能

## <a name="all-options"></a>すべてのオプション

次の表は、デスクトップ アプリ内のトーストをサポートするためのオプション、および対応するサポートされる機能を示しています。 この表を使用してシナリオに最適なオプションを選択します。<br/><br/>

| オプション | 視覚化 | アクション | 入力 | プロセス内でのアクティブ化 |
| -- | -- | -- | -- | -- |
| [COM アクティベーター](#preferred-option---com-activator) | ✔️ | ✔️ | ✔️ | ✔️ |
| [COM なし / Stub CLSID](#alternative-option---no-com--stub-clsid) | ✔️ | ✔️ | ❌ | ❌ |


## <a name="preferred-option---com-activator"></a>推奨されるオプション - COM アクティベーター

これは、デスクトップアプリで使用できる推奨オプションであり、すべての通知機能をサポートしています。 "COM アクティベーター" について心配することはありません。以前に COM サーバーを記述したことがない場合でも、これを非常に簡単にするライブラリ [C#](send-local-toast-desktop.md) および [C++ アプリ](send-local-toast-desktop-cpp-wrl.md)があります。<br/><br/>

| 視覚化 | アクション | 入力 | プロセス内でのアクティブ化 |
| -- | -- | -- | -- |
| ✔️ | ✔️ | ✔️ | ✔️ |

COM アクティベーター オプションでは、アプリで次の通知テンプレートとライセンス認証の種類を使用できます。<br/><br/>

| テンプレートとライセンス認証の種類 | MSIX/スパースパッケージ | 従来の Win32 |
| -- | -- | -- |
| ToastGeneric フォアグラウンド | ✔️ | ✔️ |
| ToastGeneric バックグラウンド | ✔️ | ✔️ |
| ToastGeneric プロトコル | ✔️ | ✔️ |
| レガシ テンプレート | ✔️ | ❌ |

> [!NOTE]
> 既存の MSIX/スパースパッケージアプリに COM アクティベーターを追加すると、フォアグラウンド/Background および従来の通知のアクティブ化によって、コマンドラインではなく、COM アクティベーターがアクティブになります。

このオプションを使用する方法については、「 [デスクトップ C# アプリからローカルトースト通知を送信](send-local-toast-desktop.md) する」または「 [デスクトップ C++ wrl アプリからローカルトースト通知を送信](send-local-toast-desktop-cpp-wrl.md)する」を参照してください。


## <a name="alternative-option---no-com--stub-clsid"></a>代替オプション - COM なし / Stub CLSID

これは、COM アクティベーターを実装できない場合の代替オプションです。 ただし、入力サポート (トーストでのテキスト ボックス) やプロセス内でのアクティブ化など、いくつかの機能が犠牲になります。<br/><br/>

| 視覚化 | アクション | 入力 | プロセス内でのアクティブ化 |
| -- | -- | -- | -- |
| ✔️ | ✔️ | ❌ | ❌ |

このオプションでは、従来の Win32 をサポートする場合、次に示すように、使用できる通知テンプレートとライセンス認証の種類がはるかに制限されます。<br/><br/>

| テンプレートとライセンス認証の種類 | MSIX/スパースパッケージ | 従来の Win32 |
| -- | -- | -- |
| ToastGeneric フォアグラウンド | ✔️ | ❌ |
| ToastGeneric バックグラウンド | ✔️ | ❌ |
| ToastGeneric プロトコル | ✔️ | ✔️ |
| レガシ テンプレート | ✔️ | ❌ |

パッケージ化された [Msix](/windows/msix/desktop/source-code-overview) アプリと [スパースパッケージ](/windows/apps/desktop/modernize/grant-identity-to-nonpackaged-apps)を使用するアプリの場合は、UWP アプリのようなトースト通知を送信するだけです。 ユーザーがトーストをクリックすると、アプリは、トーストで指定した起動引数でコマンド ラインから起動されます。

従来の Win32 アプリでは、トースト通知を送信し、ショートカットで CLSID を指定できるように AUMID を設定します。 ランダムな GUID を指定できます。 COM サーバー/アクティベーターを追加しないでください。 "stub" COM CLSID を追加することで、アクション センターで通知が保持されます。 スタブ CLSID は他のトーストのアクティブ化を中断するため、プロトコルのアクティブ化のトーストのみを使用できる点に注意してください。 そのため、プロトコルのアクティブ化をサポートするようにアプリを更新し、トースト プロトコルで各自のアプリをアクティブ化する必要があります。


## <a name="resources"></a>リソース

* [デスクトップ C# アプリからのローカル トースト通知の送信](send-local-toast-desktop.md)
* [デスクトップ C++ WRL アプリからのローカル トースト通知の送信](send-local-toast-desktop-cpp-wrl.md)
* [トースト コンテンツのドキュメント](adaptive-interactive-toasts.md)
