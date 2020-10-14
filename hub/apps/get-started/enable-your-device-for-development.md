---
ms.assetid: 54973C62-9669-4988-934E-9273FB0425FD
title: デバイスを開発用に有効にする
description: Visual Studio で開発者モードをアクティブ化することにより、開発およびデバッグ用に Windows 10 デバイスを有効にする方法について説明します。
keywords: 開発者用 Visual Studio での作業の開始, 開発者用ライセンス対応デバイス
ms.date: 10/13/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 642dd2ac6db5509e9bc4ea6ff8cb756312c3e90b
ms.sourcegitcommit: 56e9cab45d1c6e54841d61fdf23044fa01f50c43
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/13/2020
ms.locfileid: "92011904"
---
# <a name="enable-your-device-for-development"></a>デバイスを開発用に有効にする

## <a name="activate-developer-mode-sideload-apps-and-access-other-developer-features"></a>開発者モードを有効にし、アプリのサイドローディングを行い、その他の開発者向け機能にアクセスする

![デバイスを開発用に有効にする](images/developer-poster.png)

> [!IMPORTANT]
> PC で独自のアプリケーションを作成していない場合は、開発者モードを有効にする必要はありません。 コンピューターの問題を解決しようとしている場合は、[Windows ヘルプ](https://support.microsoft.com/hub/4338813/windows-help?os=windows-10)に関するページを確認してください。 初めて開発している場合は、必要なツールをダウンロードして、[準備](get-set-up.md)することもできます。

コンピューターをゲーム、Web 閲覧、メール、Office アプリなどの日々の通常の用途に使用している場合は、開発者モードを有効にする*必要はなく*、また有効にしないことをお勧めします。 その場合は、このページの情報の残りの部分は、関連のない情報ですので、 元の作業にお戻りください。

コンピューターで初めて Visual Studio を使用してソフトウェアを作成する場合は、開発用 PC とコードのテスト用デバイスの両方で、開発者モードを有効にする*必要*があります。 開発者モードが有効になっていない状態で UWP プロジェクトを開くと、 **[開発者向け]** 設定ページが開くか、Visual Studio に次のダイアログ ボックスが表示されます。

![Visual Studio で表示される、開発者モードを有効にするためのダイアログ](images/latestenabledialog.png)

このダイアログが表示された場合は、 **[開発者向け設定]** をクリックして **[開発者向け]**  設定ページを開きます。

> [!NOTE]
> **[開発者向け]** ページにいつでも移動し、開発者モードの有効/無効を切り替えることができます。その場合には、タスク バーの Cortana 検索ボックスに「開発者向け」と入力するだけです。

## <a name="accessing-settings-for-developers"></a>開発者向け設定にアクセスする

開発者モードを有効にするには、またはその他の設定にアクセスするには:

1.  **[開発者向け]** 設定ダイアログ ボックスで、必要なアクセスのレベルを選択します。
2.  選択した設定の免責事項を読み、 **[はい]** をクリックして変更を受け入れます。

> [!NOTE]
> 開発者モードを有効にするには、管理者のアクセス権が必要です。 組織所有のデバイスの場合は、このオプションが無効になっていることもあります。

### <a name="developer-mode-features"></a>開発者モードの機能

開発者モードは、開発者用ライセンスに対する Windows 8.1 の要件に置き換わるものです。  サイドローディングだけでなく、開発者モードの設定でデバッグおよび追加の展開オプションを有効にできます。 デバイスを展開先にできるようにする SSH サービスの開始も含まれます。 このサービスを停止するためには、開発者モードを無効にしなければなりません。

デスクトップで開発者モードを有効にすると、次のような機能のパッケージがインストールされます。
- Windows Device Portal。 Device Portal が有効になり、ファイアウォール規則が構成されるのは、 **[デバイス ポータルを有効にする]** オプションがオンの場合のみです。
- アプリのリモート インストールを可能にする SSH サービスのファイアウォール規則がインストールされ、構成されます。 **[デバイスの検出]** を有効にすると、SSH サーバーが有効になります。

これらの機能の詳細については、またはインストールプロセスで問題が発生した場合は、「 [開発者モードの機能とデバッグ](developer-mode-features-and-debugging.md)」を参照してください。

## <a name="see-also"></a>参照

* [Windows アカウントのサインアップ](sign-up.md)
* [開発者モードの機能とデバッグ](developer-mode-features-and-debugging.md)。