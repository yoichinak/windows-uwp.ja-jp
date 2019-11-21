---
ms.assetid: 60fc48dd-91a9-4dd6-a116-9292a7c1f3be
title: Windows Device Portal の概要
description: Windows Device Portal で、ネットワーク経由でリモートから、または USB 接続によって、デバイスの構成と管理を行うための方法を説明します。
ms.date: 04/09/2019
ms.topic: article
keywords: windows 10, uwp, device portal
ms.localizationpriority: medium
ms.openlocfilehash: 2292d97166d34905bb895aa3f53f864510a21f46
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/20/2019
ms.locfileid: "74254763"
---
# <a name="windows-device-portal-overview"></a>Windows Device Portal の概要

Windows Device Portal では、ネットワーク経由でリモートから、または USB 接続によって、デバイスの構成と管理を行えます。 It also provides advanced diagnostic tools to help you troubleshoot and view the real-time performance of your Windows device.

Windows Device Portal is a web server on your device that you can connect to from a web browser on a PC. If your device has a web browser, you can also connect locally with the browser on that device.

Windows Device Portal is available on each device family, but features and setup vary based on each device's requirements. ここでは、Device Portal の一般的な説明と、各デバイス ファミリに関するさらに詳細な情報が掲載されている記事へのリンクを示します。

The functionality of the Windows Device Portal is implemented with [REST APIs](device-portal-api-core.md) that you can use directly to access data and control your device programmatically.

## <a name="setup"></a>[セットアップ]

Device Portal への接続の具体的な手順はデバイスによって異なりますが、どのデバイスでも次の一般的な手順が必要です。

1. Enable Developer Mode and Device Portal on your device (configured in the Settings app).

2. Connect your device and PC through a local network or with USB.

3. ブラウザーでデバイス ポータルのページに移動します。 This table shows the ports and protocols used by each device family.

デバイス ファミリ | 既定でオンになっているか | HTTP | HTTPS | [USB]
--------------|----------------|------|-------|----
HoloLens | 開発者モードでオン | 80 (既定) | 443 (既定) | http://127.0.0.1:10080
IoT | 開発者モードでオン | 8080 | レジストリ キーで有効化する | 該当なし
Xbox | 開発者モードで有効化する | 無効 | 11443 | 該当なし
Desktop| 開発者モードで有効化する | 50080\* | 50043\* | 該当なし
Phone | 開発者モードで有効化する | 80| 443 | http://127.0.0.1:10080

\* This is not always the case, as Device Portal on desktop claims ports in the ephemeral range (>50,000) to prevent collisions with existing port claims on the device. 詳しくは、デスクトップに関する「[ポート番号を設定する](device-portal-desktop.md#registry-based-configuration-for-device-portal)」のセクションをご覧ください。  

デバイス固有のセットアップ手順については、以下をご覧ください。

- [HoloLens 用 Device Portal](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-hololens)
- [IoT 用 Device Portal](https://docs.microsoft.com/windows/iot-core/manage-your-device/DevicePortal)
- [モバイル用 Device Portal](device-portal-mobile.md)
- [Xbox 向けのデバイス ポータル](../xbox-apps/device-portal-xbox.md)
- [デスクトップ用 Device Portal](device-portal-desktop.md#set-up-device-portal-on-windows-desktop)

## <a name="features"></a>機能

### <a name="toolbar-and-navigation"></a>ツールバーとナビゲーション

The toolbar at the top of the page provides access to commonly used features.

- **Power**: Access power options.
  - **[Shutdown]** (シャットダウン): デバイスをオフにします。
  - **[Restart]** (再起動): デバイスの電源を入れ直します。
- **[Help]** (ヘルプ): ヘルプ ページを開きます。

ページの左側にあるナビゲーション ウィンドウのリンクを使用して、デバイスの管理と監視に利用可能なツールに移動します。

Tools that are common across device families are described here. デバイスによってはその他のオプションを利用できる場合があります。 For more info, see the specific page for your device type.

### <a name="apps-manager"></a>アプリ マネージャー

The Apps manager provides install/uninstall and management functionality for app packages and bundles on the host device.

![Device Portal Apps manager page](images/device-portal/WDP_AppsManager2.png)

* **Deploy apps**: Deploy packaged apps from local, network, or web hosts and register loose files from network shares.
* **Installed apps**: Use the dropdown menu to remove or start apps that are installed on the device.
* **Running apps**: Get information about the apps that are currently running and close them as necessary.

#### <a name="install-sideload-an-app"></a>Install (sideload) an app

You can sideload apps during development using Windows Device Portal:

1. When you've created an app package, you can remotely install it onto your device. Visual Studio でビルドすると、出力フォルダーが生成されます。

    ![アプリのインストール](images/device-portal/iot-installapp0.png)

2. In Windows Device Portal, navigate to the **Apps manager** page.

3. In the **Deploy apps** section, select **Local Storage**.

4. Under **Select the application package**, select **Choose File** and browse to the app package that you want to sideload.

5. Under **Select certificate file (.cer) used to sign app package**, select **Choose File** and browse to the certificate associated with that app package.

6. Check the respective boxes if you want to install optional or framework packages along with the app installation, and select **Next** to choose them.

7. Select **Install** to initiate the installation.

8. If the device is running Windows 10 in S mode, and it is the first time that the given certificate has been installed on the device, restart the device.

#### <a name="install-a-certificate"></a>Install a certificate

Alternatively, you can install the certificate via Windows Device Portal, and install the app through other means:

1. In Windows Device Portal, navigate to the **Apps manager** page.

2. In the **Deploy apps** section, select **Install Certificate**.

3. Under **Select certificate file (.cer) used to sign an app package**, select **Choose File** and browse to the certificate associated with the app package that you want to sideload.

4. Select **Install** to initiate the installation.

5. If the device is running Windows 10 in S mode, and it is the first time that the given certificate has been installed on the device, restart the device.

#### <a name="uninstall-an-app"></a>アプリをアンインストールする

1. アプリが実行中でないことを確認します。
2. If it is, go to **Running apps** and close it. If you attempt to uninstall while the app is running, it will cause issues when you attempt to reinstall the app.
3. Select the app from the dropdown and click **Remove**.

### <a name="running-processes"></a>Running processes

This page shows details about processes currently running on the host device. これには、アプリとシステムの両方のプロセスが含まれます。 On some platforms (Desktop, IoT, and HoloLens), you can terminate processes.

![Device Portal Running processes page](images/device-portal/mob-device-portal-processes.png)

### <a name="file-explorer"></a>エクスプローラー

This page allows you to view and manipulate files stored by any sideloaded apps. See the [Using the App File Explorer](https://blogs.windows.com/buildingapps/2016/06/08/using-the-app-file-explorer-to-see-your-app-data/) blog post to learn more about the File explorer and how to use it.

![Device Portal File explorer page](images/device-portal/mob-device-portal-AppFileExplorer.png)

### <a name="performance"></a>パフォーマンス

The Performance page shows real-time graphs of system diagnostic info like power usage, frame rate, and CPU load.

利用可能なメトリックを次に示します。

- **CPU**: Percent of total available CPU utilization
- **Memory**: Total, in use, available, committed, paged, and non-paged
- **I/O**: Read and write data quantities
- **Network**: Received and sent data
- **GPU**: Percent of total available GPU engine utilization

![Device Portal Performance page](images/device-portal/mob-device-portal-perf.png)

### <a name="event-tracing-for-windows-etw-logging"></a>Event Tracing for Windows (ETW) logging

The ETW logging page manages real-time Event Tracing for Windows (ETW) information on the device.

![Device Portal ETW logging page](images/device-portal/mob-device-portal-etw.png)

**[Hide Providers]** (プロバイダを非表示にする) チェックボックスをオンにすると、イベントの一覧のみが表示されます。

- **Registered providers**: Select the event provider and the tracing level. The tracing level is one of these values:
  1. 異常終了または終了
  2. 重大なエラー
  3. 警告
  4. エラーではない警告
  5. Detailed trace

  トレースを開始するには、 **[Enable]** (有効にする) をクリックまたはタップします。 **[Enabled Providers]** (有効なプロバイダー) ドロップダウン リストにプロバイダーが追加されます。
- **[Custom providers]** (カスタム プロバイダー): カスタム ETW プロバイダーとトレース レベルを選択します。 GUID を使用してプロバイダーを識別します。 Do not include brackets in the GUID.
- **Enabled providers**: This lists the enabled providers. ドロップダウンからプロバイダーを選択し、 **[Disable]** (無効にする) をクリックまたはタップしてトレースを停止します。 すべてのトレースを中断するには、 **[Stop All]** (すべて停止) をクリックまたはタップします。
- **Providers history**: This shows the ETW providers that were enabled during the current session. 無効になっているプロバイダーをアクティブ化するには、 **[Enable]** (有効にする) をクリックまたはタップします。 履歴をクリアするには、 **[Clear]** (クリア) をクリックまたはタップします。
- **Filters / Events**: The **Events** section lists ETW events from the selected providers in table format. The table is updated in real time. Use the **Filters** menu to set up custom filters for which events will be displayed. Click the **Clear** button to delete all ETW events from the table. これによってプロバイダーが無効になることはありません。 You can click **Save to file** to export the currently collected ETW events to a local CSV file.

For more details on using ETW logging, see the [Use Device Portal to view debug logs](https://blogs.windows.com/buildingapps/2016/06/10/using-device-portal-to-view-debug-logs-for-uwp/) blog post.

### <a name="performance-tracing"></a>パフォーマンス トレース

The Performance tracing page allows you for view the [Windows Performance Recorder (WPR)](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh448205(v=win.10)) traces from the host device.

![Device Portal performance tracing page](images/device-portal/mob-device-portal-perf-tracing.png)

- **[Available profiles]** (利用可能なプロファイル): ドロップダウン リストから WPR プロファイルを選択し、 **[Start]** (開始) をクリックまたはタップすると、トレースを開始できます。
- **[Custom profiles]** (カスタム プロファイル): **[Browse]** (参照) をクリックまたはタップして、PC から WPR プロファイルを選択します。 **[Upload and start]** (アップロードして開始) をクリックまたはタップすると、トレースが開始します。

トレースを停止するには、 **[Stop]** (停止) をクリックします。 Stay on this page until the trace file (.ETL) has finished downloading.

Captured .ETL files can be opened for analysis in the [Windows Performance Analyzer](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh448170(v=win.10)).

### <a name="device-manager"></a>デバイス マネージャー

The Device manager page enumerates all peripherals attached to your device. You can click the settings icons to view the properties of each.

![Device Portal Device manager page](images/device-portal/mob-device-portal-devices.png)

### <a name="networking"></a>のネットワーク

The Networking page manages network connections on the device. Unless you are connected to Device Portal through USB, changing these settings will likely disconnect you from Device Portal.

- **Available networks**: Shows the WiFi networks available to the device. ネットワークをクリックまたはタップすると、そのネットワークに接続し、必要に応じてパスキーを提供できます。 Device Portal does not yet support Enterprise Authentication. You can also use the **Profiles** dropdown to attempt to connect to any of the WiFi profiles known to the device.
- **IP configuration**: Shows address information about each of the host device's network ports.

![Device Portal Networking page](images/device-portal/mob-device-portal-network.png)

## <a name="service-features-and-notes"></a>Service features and notes

### <a name="dns-sd"></a>DNS-SD

Device Portal は DNS-SD を使用して、ローカル ネットワーク上でその存在をアドバタイズします Device Portal のすべてのインスタンスは、デバイスの種類に関係なく、"WDP._wdp._tcp.local" でアドバタイズします。 サービス インスタンスの TXT レコードは、次の情報を提供します。

Key | タスクバーの検索ボックスに | 説明
----|------|-------------
S | 整数 | Device Portal 用のセキュリティで保護されたポート。 0 (ゼロ) の場合、Device Portal は HTTPS 接続をリッスンしていません。
D | string | デバイスの種類。 This will be in the format "Windows.*", for example, Windows.Xbox or Windows.Desktop
確認が完了していないエイリアスの横には、 | string | デバイスのアーキテクチャ。 これは、ARM、x86、AMD64 のいずれかです。  
T | null 文字で区切られた string のリスト | ユーザーが適用したデバイスのタグ。 使い方については、タグの REST API に関する説明をご覧ください。 リストは 2 つの null で終了します。  

DNS-SD レコードでアドバタイズされる HTTP ポートですべてのデバイスがリッスンしているわけではないため、HTTPS ポートでの接続をお勧めします。

### <a name="csrf-protection-and-scripting"></a>CSRF に対する保護とスクリプト

[CSRF 攻撃](https://en.wikipedia.org/wiki/Cross-site_request_forgery)に対する保護のために、すべての非 GET 要求に一意のトークンが必要です。 このトークン、X-CSRF-Token 要求ヘッダーは、セッション Cookie、CSRF-Token から派生します。 Device Portal の Web UI では、CSRF-Token Cookie が、各要求の X-CSRF-Token にコピーされます。

> [!IMPORTANT]
> This protection prevents usages of the REST APIs from a standalone client (such as command-line utilities). これは 3 つの方法で解決できます。
> - Use an "auto-" username. クライアントはユーザー名の前に "auto-" を追加することによって、CSRF に対する保護を迂回できます。 このユーザー名は、ブラウザーから Device Portal にログインするために使用しないでください。サービスが CSRF 攻撃を受ける可能性があります。 例: Device Portal のユーザー名が "admin" である場合、CSRF に対する保護を迂回するために ```curl -u auto-admin:password <args>``` を使用します。
> - クライアントで cookie-to-header スキームを実装します。 そのためには、GET 要求でセッション Cookie を確立し、それ以降のすべての要求にヘッダーと Cookie の両方を含めます。
> - 認証を無効にして、HTTP を使用します。 CSRF に対する保護は HTTPS エンドポイントにのみ適用されるため、HTTP エンドポイントに接続する場合、上記のいずれの操作も必要ありません。

#### <a name="cross-site-websocket-hijacking-cswsh-protection"></a>クロスサイト WebSocket ハイジャック (CSWSH) に対する保護

[CSWSH 攻撃](https://www.christian-schneider.net/CrossSiteWebSocketHijacking.html)を防御するために、Device Portal に対して WebSocket 接続を開くすべてのクライアントは、Host ヘッダーと一致する Origin ヘッダーも提供する必要があります。 これにより、Device Portal に対して、要求が Device Portal の UI または有効なクライアント アプリケーションからの要求であることを証明します。 Origin ヘッダーがない場合、要求は拒否されます。
