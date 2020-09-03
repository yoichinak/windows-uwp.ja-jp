---
ms.assetid: 60fc48dd-91a9-4dd6-a116-9292a7c1f3be
title: Windows Device Portal の概要
description: Windows Device Portal で、ネットワーク経由でリモートから、または USB 接続によって、デバイスの構成と管理を行うための方法を説明します。
ms.date: 04/09/2019
ms.topic: article
keywords: windows 10, uwp, デバイス ポータル
ms.localizationpriority: medium
ms.openlocfilehash: a4fc5cc5b8bc99e830d3c31604e581f8e57c1007
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173636"
---
# <a name="windows-device-portal-overview"></a>Windows Device Portal の概要

Windows Device Portal では、ネットワーク経由でリモートから、または USB 接続によって、デバイスの構成と管理を行えます。 また、Windows デバイスのトラブルシューティングを行ったり、リアルタイムのパフォーマンスを表示するための高度な診断ツールも用意されています。

Windows デバイス ポータルは、PC 上の Web ブラウザーから接続することができるデバイス上の Web サーバーです。 デバイスに Web ブラウザーがある場合、そのデバイス上のブラウザーにローカル接続することもできます。

各デバイス ファミリで Windows デバイス ポータルを利用できますが、機能とセットアップは各デバイスの要件によって異なります。 ここでは、Device Portal の一般的な説明と、各デバイス ファミリに関するさらに詳細な情報が掲載されている記事へのリンクを示します。

Windows デバイス ポータルの機能は、[REST API](device-portal-api-core.md) を使用して実装されています。REST API は、プログラムを使用してデータに直接アクセスしてデバイスを制御するために使用できます。

## <a name="setup"></a>セットアップ

Device Portal への接続の具体的な手順はデバイスによって異なりますが、どのデバイスでも次の一般的な手順が必要です。

1. デバイスで開発者モードとデバイス ポータルを有効にします (設定アプリで構成)。

2. デバイスと PC をローカル ネットワーク経由または USB を使用して接続します。

3. ブラウザーでデバイス ポータルのページに移動します。 次の表は、各デバイス ファミリで使用されるポートとプロトコルを示しています。

デバイス ファミリ | 既定でオンになっているか | HTTP | HTTPS | USB
--------------|----------------|------|-------|----
HoloLens | 開発者モードでオン | 80 (既定) | 443 (既定) | http://127.0.0.1:10080
IoT | 開発者モードでオン | 8080 | レジストリ キーで有効化する | なし
Xbox | 開発者モードで有効化する | 無効 | 11443 | なし
デスクトップ| 開発者モードで有効化する | 50080\* | 50043\* | なし
電話 | 開発者モードで有効化する | 80| 443 | http://127.0.0.1:10080

\* デスクトップ上のデバイス ポータルが、デバイス上の既存のポート要求との競合を避ける目的でエフェメラル範囲 (>50,000) にあるポートを要求するため、この表が当てはまらない場合があります。 詳しくは、デスクトップに関する「[ポート番号を設定する](device-portal-desktop.md#registry-based-configuration-for-device-portal)」のセクションをご覧ください。  

デバイス固有のセットアップ手順については、以下をご覧ください。

- [HoloLens 用 Device Portal](./device-portal-hololens.md)
- [IoT 用 Device Portal](/windows/iot-core/manage-your-device/DevicePortal)
- [モバイル用 Device Portal](device-portal-mobile.md)
- [Xbox 向けのデバイス ポータル](../xbox-apps/device-portal-xbox.md)
- [デスクトップ用 Device Portal](device-portal-desktop.md#set-up-device-portal-on-windows-desktop)

## <a name="features"></a>機能

### <a name="toolbar-and-navigation"></a>ツールバーとナビゲーション

ページの最上部にあるツール バーでは、よく使われる機能にアクセスできます。

- **[電源]** :電源オプションにアクセスします。
  - **[Shutdown]\(シャットダウン\)** : デバイスをオフにします。
  - **[Restart]\(再起動\)** : デバイスの電源を入れ直します。
- **[Help]\(ヘルプ\)** : ヘルプ ページを開きます。

ページの左側にあるナビゲーション ウィンドウのリンクを使用して、デバイスの管理と監視に利用可能なツールに移動します。

ここでは、すべてのデバイス ファミリに共通のツールについて説明します。 デバイスによってはその他のオプションを利用できる場合があります。 詳しくは、お使いのデバイスの種類用のページをご覧ください。

### <a name="apps-manager"></a>アプリ マネージャー

アプリ マネージャーでは、ホスト デバイス上でアプリ パッケージとバンドルのインストール/アンインストールおよび管理を行う機能が提供されます。

![デバイス ポータル アプリ マネージャーのページ](images/device-portal/WDP_AppsManager2.png)

* **[Deploy apps]\(アプリのデプロイ\)** :ローカル、ネットワーク、または Web ホストからパッケージ アプリをデプロイし、ネットワーク共有からルーズ ファイルを登録します。
* **[Installed apps]\(インストール済みのアプリ\)** : ドロップダウン メニューを使用して、デバイスにインストールされているアプリを削除または開始します。
* **[Running apps]\(実行中のアプリ\)** : 現在実行中のアプリに関する情報を取得し、必要に応じて閉じます。

#### <a name="install-sideload-an-app"></a>アプリのインストール (サイドロード)

Windows デバイス ポータルを使用して、開発中にアプリをサイドロードできます。

1. アプリ パッケージを作成したら、デバイス上にリモートでインストールできます。 Visual Studio でビルドすると、出力フォルダーが生成されます。

    ![アプリのインストール](images/device-portal/iot-installapp0.png)

2. Windows デバイス ポータルで、 **[Apps manager]\(アプリ マネージャー\)** ページに移動します。

3. **[Deploy apps]\(アプリのデプロイ\)** セクションで、 **[ローカル ストレージ]** を選択します。

4. **[Select the application package]\(アプリケーション パッケージの選択\)** で、 **[ファイルの選択]** を選択し、サイドロードするアプリ パッケージを参照します。

5. **[Select certificate file (.cer) used to sign app package]\(アプリ パッケージに署名するために使用する証明書ファイル (.cer) の選択\)** で、 **[ファイルの選択]** を選択し、そのアプリ パッケージに関連付けられている証明書を参照します。

6. アプリのインストールと共にオプションまたはフレームワークのパッケージをインストールする場合は、それぞれのボックスをオンにし、 **[次へ]** を選択して選択します。

7. **[インストール]** を選択すると、インストールが開始されます。

8. デバイス上で Windows 10 が S モードで実行されていて、特定の証明書がそのデバイスに初めてインストールされた場合は、デバイスを再起動します。

#### <a name="install-a-certificate"></a>証明書をインストールする

または、Windows デバイス ポータルを使用して証明書をインストールし、他の方法でアプリをインストールすることもできます。

1. Windows デバイス ポータルで、 **[Apps manager]\(アプリ マネージャー\)** ページに移動します。

2. **[Deploy apps]\(アプリのデプロイ\)** セクションで、 **[証明書のインストール]** を選択します。

3. **[Select certificate file (.cer) used to sign app package]\(アプリ パッケージに署名するために使用する証明書ファイル (.cer) の選択\)** で、 **[ファイルの選択]** を選択し、サイドロードするアプリ パッケージに関連付けられている証明書を参照します。

4. **[インストール]** を選択すると、インストールが開始されます。

5. デバイス上で Windows 10 が S モードで実行されていて、特定の証明書がそのデバイスに初めてインストールされた場合は、デバイスを再起動します。

#### <a name="uninstall-an-app"></a>アプリをアンインストールする

1. アプリが実行中でないことを確認します。
2. その場合、 **[Running apps]\(実行中のアプリ\)** に移動してそのアプリを閉じます。 アプリの実行中にアンインストールすると、そのアプリを再インストールしようとするときに問題が発生することがあります。
3. ドロップダウンからアプリを選択し、 **[削除]** をクリックします。

### <a name="running-processes"></a>実行中のプロセス

このページには、ホスト デバイス上で現在実行中のプロセスの詳細が表示されます。 これには、アプリとシステムの両方のプロセスが含まれます。 一部のプラットフォーム (Desktop、IoT、および HoloLens) では、プロセスを終了することができます。

![デバイス ポータルの [実行中のプロセス] ページ](images/device-portal/mob-device-portal-processes.png)

### <a name="file-explorer"></a>エクスプローラー

このページでは、サイドロードされた任意のアプリによって保存されたファイルを表示および操作できます。 エクスプローラーとその使用方法について詳しくは、[アプリ エクスプローラーの使用](https://blogs.windows.com/buildingapps/2016/06/08/using-the-app-file-explorer-to-see-your-app-data/)に関するブログ記事をご覧ください。

![デバイス ポータルの [エクスプローラー] ページ](images/device-portal/mob-device-portal-AppFileExplorer.png)

### <a name="performance"></a>[パフォーマンス]

[パフォーマンス] ページには、電力消費、フレーム レート、CPU 負荷など、システムの診断情報のグラフがリアルタイムで表示されます。

利用可能なメトリックを次に示します。

- **[CPU]** :使用可能な CPU 使用量の合計に対するパーセント
- **[Memory]\(メモリ\)** : 合計メモリ、使用中のメモリ、使用可能なメモリ、コミットされたメモリ、ページ メモリ、および非ページ メモリ
- **[I/O]** : データの読み取りと書き込みの量
- **ネットワーク**:受信済みおよび送信済みデータ
- **[GPU]** : 使用可能な GPU エンジン使用量の合計に対するパーセント

![デバイス ポータルの [パフォーマンス] ページ](images/device-portal/mob-device-portal-perf.png)

### <a name="event-tracing-for-windows-etw-logging"></a>Windows イベント トレーシング (ETW) ログ

[ETW logging]\(ETW ログ\) ページでは、デバイス上のリアルタイム Windows イベント トレーシング (ETW) 情報を管理します。

![デバイス ポータルの [ETW logging]\(ETW ログ\) ページ](images/device-portal/mob-device-portal-etw.png)

**[Hide Providers]** (プロバイダを非表示にする) チェックボックスをオンにすると、イベントの一覧のみが表示されます。

- **[Registered providers]\(登録済みプロバイダー\)** : イベント プロバイダーとトレース レベルを選択します。 トレース レベルは次のいずれかの値になります。
  1. 異常終了または終了
  2. 重大なエラー
  3. 警告
  4. エラーではない警告
  5. 詳細なトレース

  トレースを開始するには、 **[Enable]** (有効にする) をクリックまたはタップします。 **[Enabled Providers]** (有効なプロバイダー) ドロップダウン リストにプロバイダーが追加されます。
- **[Custom providers]\(カスタム プロバイダー\)** : カスタム ETW プロバイダーとトレース レベルを選択します。 GUID を使用してプロバイダーを識別します。 GUID にはかっこを含めないでください。
- **[Enabled providers]\(有効なプロバイダー\)** : 有効なプロバイダーを一覧表示します。 ドロップダウンからプロバイダーを選択し、 **[Disable]** (無効にする) をクリックまたはタップしてトレースを停止します。 すべてのトレースを中断するには、 **[Stop All]** (すべて停止) をクリックまたはタップします。
- **[Providers history]\(プロバイダー履歴\)** : 現在のセッション中に有効になった ETW プロバイダーを表示します。 無効になっているプロバイダーをアクティブ化するには、 **[Enable]** (有効にする) をクリックまたはタップします。 履歴をクリアするには、 **[Clear]** (クリア) をクリックまたはタップします。
- **[フィルター]/[イベント]** : **[イベント]** セクションには、選択したプロバイダーの ETW イベントが表形式で一覧表示されます。 この表はリアルタイムで更新されます。 **[フィルター]** メニューを使用して、表示するイベントのカスタム フィルターを設定します。 すべての ETW イベントを表から削除するには、 **[クリア]** ボタンをクリックします。 これによってプロバイダーが無効になることはありません。 **[ファイルに保存]** をクリックすると、現在収集されている ETW イベントをローカルの CSV ファイルにエクスポートできます。

ETW ログの使用について詳しくは、[デバイス ポータルを使用したデバッグ ログの表示](https://blogs.windows.com/buildingapps/2016/06/10/using-device-portal-to-view-debug-logs-for-uwp/)に関するブログ記事をご覧ください。

### <a name="performance-tracing"></a>パフォーマンス トレース

[パフォーマンスのトレース] ページでは、ホスト デバイスからの [Windows パフォーマンス レコーダー (WPR)](/previous-versions/windows/it-pro/windows-8.1-and-8/hh448205(v=win.10)) トレースを表示できます。

![デバイス ポータルの [パフォーマンスのトレース] ページ](images/device-portal/mob-device-portal-perf-tracing.png)

- **[Available profiles]\(利用可能なプロファイル\)** : ドロップダウン リストから WPR プロファイルを選択し、 **[Start]\(開始\)** をクリックまたはタップすると、トレースを開始できます。
- **[Custom profiles]\(カスタム プロファイル\)** : **[Browse]\(参照\)** をクリックまたはタップして、PC から WPR プロファイルを選択します。 **[Upload and start]** (アップロードして開始) をクリックまたはタップすると、トレースが開始します。

トレースを停止するには、 **[Stop]** (停止) をクリックします。 トレース ファイル (.ETL) のダウンロードが完了するまで、このページを閉じないでください。

キャプチャした .ETL ファイルは、[Windows Performance Analyzer](/previous-versions/windows/it-pro/windows-8.1-and-8/hh448170(v=win.10)) で開いて分析に使用できます。

### <a name="device-manager"></a>デバイス マネージャー

[デバイス マネージャー] ページでは、デバイスに接続されているすべての周辺機器が列挙されます。 設定アイコンをクリックして、それぞれのプロパティを表示できます。

![デバイス ポータルの [デバイス マネージャー] ページ](images/device-portal/mob-device-portal-devices.png)

### <a name="networking"></a>ネットワーク

[ネットワーク] ページでは、デバイス上のネットワーク接続を管理します。 デバイス ポータルに USB 経由で接続している場合を除き、これらの設定を変更するとデバイス ポータルとの接続が切断される可能性があります。

- **[Available networks]\(使用可能なネットワーク\)** : デバイスで利用可能な Wi-Fi ネットワークが表示されます。 ネットワークをクリックまたはタップすると、そのネットワークに接続し、必要に応じてパスキーを提供できます。 デバイス ポータルではエンタープライズ認証はまだサポートされていません。 **[プロファイル]** ドロップダウンを使用して、デバイスで認識されている任意の Wi-Fi プロファイルへの接続を試すこともできます。
- **[IP configuration]\(IP 構成\)** : 各ホスト デバイスのネットワーク ポートについてのアドレス情報が表示されます。

![デバイス ポータルの [ネットワーク] ページ](images/device-portal/mob-device-portal-network.png)

## <a name="service-features-and-notes"></a>サービスの機能と注意事項

### <a name="dns-sd"></a>DNS-SD

Device Portal は DNS-SD を使用して、ローカル ネットワーク上でその存在をアドバタイズします Device Portal のすべてのインスタンスは、デバイスの種類に関係なく、"WDP._wdp._tcp.local" でアドバタイズします。 サービス インスタンスの TXT レコードは、次の情報を提供します。

キー | 種類 | 説明
----|------|-------------
S | int | Device Portal 用のセキュリティで保護されたポート。 0 (ゼロ) の場合、Device Portal は HTTPS 接続をリッスンしていません。
D | string | デバイスの種類。 これは "Windows.*" の形式 (Windows.Xbox、Windows.Desktop など) になります
A | string | デバイスのアーキテクチャ。 これは、ARM、x86、AMD64 のいずれかです。  
T | null 文字で区切られた string のリスト | ユーザーが適用したデバイスのタグ。 使い方については、タグの REST API に関する説明をご覧ください。 リストは 2 つの null で終了します。  

DNS-SD レコードでアドバタイズされる HTTP ポートですべてのデバイスがリッスンしているわけではないため、HTTPS ポートでの接続をお勧めします。

### <a name="csrf-protection-and-scripting"></a>CSRF に対する保護とスクリプト

[CSRF 攻撃](https://en.wikipedia.org/wiki/Cross-site_request_forgery)に対する保護のために、すべての非 GET 要求に一意のトークンが必要です。 このトークン、X-CSRF-Token 要求ヘッダーは、セッション Cookie、CSRF-Token から派生します。 Device Portal の Web UI では、CSRF-Token Cookie が、各要求の X-CSRF-Token にコピーされます。

> [!IMPORTANT]
> この保護によって、スタンドアロン クライアント (コマンドライン ユーティリティなど) から REST API を使用できなくなります。 これは 3 つの方法で解決できます。
> - "auto-" ユーザー名を使用します。 クライアントはユーザー名の前に "auto-" を追加することによって、CSRF に対する保護を迂回できます。 このユーザー名は、ブラウザーから Device Portal にログインするために使用しないでください。サービスが CSRF 攻撃を受ける可能性があります。 例:デバイス ポータルのユーザー名が "admin" である場合、CSRF に対する保護を迂回するために ```curl -u auto-admin:password <args>``` を使用する必要があります。
> - クライアントで cookie-to-header スキームを実装します。 そのためには、GET 要求でセッション Cookie を確立し、それ以降のすべての要求にヘッダーと Cookie の両方を含めます。
> - 認証を無効にして、HTTP を使用します。 CSRF に対する保護は HTTPS エンドポイントにのみ適用されるため、HTTP エンドポイントに接続する場合、上記のいずれの操作も必要ありません。

#### <a name="cross-site-websocket-hijacking-cswsh-protection"></a>クロスサイト WebSocket ハイジャック (CSWSH) に対する保護

[CSWSH 攻撃](https://www.christian-schneider.net/CrossSiteWebSocketHijacking.html)を防御するために、Device Portal に対して WebSocket 接続を開くすべてのクライアントは、Host ヘッダーと一致する Origin ヘッダーも提供する必要があります。 これにより、Device Portal に対して、要求が Device Portal の UI または有効なクライアント アプリケーションからの要求であることを証明します。 Origin ヘッダーがない場合、要求は拒否されます。