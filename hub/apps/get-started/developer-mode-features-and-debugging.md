---
title: 開発者モードの機能とデバッグ
description: Windows 10 の開発者モードの機能に関する詳細と、インストールエラーの詳細について説明します。
keywords: 開発者用 Visual Studio での作業の開始, 開発者用ライセンス対応デバイス
ms.date: 10/13/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 85c64cd4baed7be9edde0dfc008ab90c1d537ca4
ms.sourcegitcommit: d0eef123b167dc63f482a9f4432a237c1c6212db
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/29/2021
ms.locfileid: "99077230"
---
# <a name="developer-mode-features-and-debugging"></a>開発者モードの機能とデバッグ

アプリに開発者モードをインストールする方法の基本に関心がある場合は、「 [開発用にデバイスを有効](enable-your-device-for-development.md) にする」に記載されている手順に従って作業を開始することができます。 この記事では、開発者モードの高度な機能、以前のバージョンの Windows 10 の開発者モード、および開発者モードのインストールでのエラーのデバッグについて説明します。

## <a name="additional-developer-mode-features"></a>追加の開発者モード機能

各デバイス ファミリには、開発者向けの追加機能が用意されている場合があります。 これらの機能は、デバイスで開発者モードが有効になっている場合にのみ使用でき、OS バージョンによって異なる可能性があります。

この画像は、Windows 10 の開発者向け機能を示しています。

![開発者モードのオプション](images/devmode-mob-options.png)

### <a name="span-iddevice-discovery-and-pairingspandevice-portal"></a><span id="device-discovery-and-pairing"></span>Device Portal

Device Portal について詳しくは、「[Windows Device Portal の概要](/windows/uwp/debug-test-perf/device-portal.md)」をご覧ください。


デバイス固有のセットアップ手順については、以下をご覧ください。
- [デスクトップ用 Device Portal](/windows/uwp/debug-test-perf/device-portal-desktop)
- [HoloLens 用 Device Portal](/windows/mixed-reality/using-the-windows-device-portal)
- [IoT 用 Device Portal](/windows/iot-core/manage-your-device/deviceportal)
- [モバイル用 Device Portal](/windows/uwp/debug-test-perf/device-portal-mobile)
- [Xbox 向けのデバイス ポータル](/windows/uwp/xbox-apps/device-portal-xbox)

開発者モードの有効化または Device Portal について問題が発生した場合には、「[既知の問題](https://social.msdn.microsoft.com/Forums/en-US/home?forum=Win10SDKToolsIssues&sort=relevancedesc&brandIgnore=True&searchTerm=%22device+portal%22)」フォーラムで問題の回避策を見つけるか、または「[開発者モード パッケージのインストール エラー](#failure-to-install-developer-mode-package)」で、開発者モード パッケージをブロック解除するための WSUS サポート技術情報の追加の情報をご覧ください。

### <a name="sideload-apps"></a>アプリのサイドローディング

> [!IMPORTANT]
> 最新の Windows 10 更新プログラムでは、サイドローディングが既定で有効になっているため、この設定は表示されません。 以前のバージョンの Windows 10 を使用している場合、既定の設定では Microsoft Store からアプリを実行することのみが許可されており、Microsoft 以外のソースからアプリをインストールするにはサイドローディングを有効にする必要があります。

アプリのサイドロード設定は、通常、Microsoft Store を使わずにカスタム アプリを管理対象デバイスにインストールする必要がある会社や学校によって、または Microsoft 以外のソースからアプリを実行する必要があるユーザーによって、使用されます。 この場合、設定ページのイメージで以前に示したように、*UWP アプリ* 設定を無効にするポリシーを組織が適用していることはよくあります。 また、組織は、必要な証明書と、アプリをサイドローディングするインストール場所を提供します。 詳しくは、TechNet の記事「[Windows 10 でのアプリのサイド ローディング](/windows/deploy/sideload-apps-in-windows-10)」と「[Microsoft Intune の基本](/mem/intune/fundamentals/)」をご覧ください。

デバイス ファミリ固有の情報

-   デスクトップ デバイス ファミリの場合:パッケージ ("Add-AppDevPackage.ps1") と共に作成される Windows PowerShell スクリプトを実行して、アプリの実行に必要なアプリ パッケージ (.appx) と証明書をインストールできます。 詳しくは、「[UWP アプリのパッケージ化](/windows/msix/package/packaging-uwp-apps)」をご覧ください。

-   モバイル デバイス ファミリの場合:必要な証明書が既にインストールされている場合は、メールまたは SD カードで受け取ったファイルをタップして、.appx をインストールできます。


信頼できる証明書がないデバイスにアプリをインストールすることはできないため、**アプリのサイドローディング** は開発者モードよりも安全です。

> [!NOTE]
> アプリをサイドローディングする場合は、信頼できるソースからのみアプリをインストールしてください。 サイドローディングしたアプリ (Microsoft Store の認証を受けていないアプリ) をインストールする場合は、そのアプリをサイドローディングする際に必要なすべての権利をお客様が保持していること、およびそのアプリのインストールや実行の結果生じるすべての問題についてお客様が一切の責任を負うことに同意したものと見なされます。 この[プライバシーに関する声明](https://privacy.microsoft.com/privacystatement)の「Windows」&gt;「Microsoft Store」セクションをご覧ください。


### <a name="ssh"></a>SSH

デバイスでデバイスの検出を有効にすると、SSH サービスが有効になります。  デバイスが UWP アプリケーションのリモート展開ターゲットの場合にこれを使用します。   サービスの名前は、「SSH Server Broker」と「SSH Server Proxy」です。

> [!NOTE]
> これは Microsoft の OpenSSH の実装ではありません。それは [GitHub](https://github.com/PowerShell/Win32-OpenSSH) にあります。  

SSH サービスを利用するには、デバイスの検出を有効にして PIN のペアリングを許可できます。 別の SSH サービスを実行する予定の場合、別のポートにセットアップするか、開発者モードの SSH サービスを無効にできます。 SSH サービスを無効にするには、デバイスの検出を無効にします。  

SSH ログインは、認証用のパスワードを受け入れる "DevToolsUser" アカウントを通じて行われます。  このパスワードは、デバイスの検出の [ペアリング] ボタンを押した後にデバイスに表示される PIN であり、PIN が表示されている間のみ有効です。  Visual Studio からルーズ ファイル配置がインストールされる DevelopmentFiles フォルダーを手動で管理するために、SFTP サブシステムも有効になります。

#### <a name="caveats-for-ssh-usage"></a>SSH の使用に関する注意事項
Windows で使用される既存の SSH サーバーはまだプロトコルに準拠していないため、SFTP または SSH クライアントを使うには特殊な構成が必要になることがあります。  具体的には、SFTP サブシステムはバージョン 3 以下で実行されるため、中継するクライアントが古いサーバーを使うように構成する必要があります。  古いデバイスの SSH サーバーは、公開キー認証として `ssh-dss` が使用されます。これは、OpenSSH によって非推奨となりました。  このようなデバイスに接続するには、`ssh-dss` を受け入れるように SSH クライアントを手動で構成する必要があります。  

### <a name="device-discovery"></a>デバイスの検出

デバイスの検出を有効にすると、ネットワーク上の他のデバイスから mDNS を介してそのデバイスが表示できるようになります。  この機能では、デバイスの検出が有効になると一度だけ表示される [ペアリング] ボタンを押すことで、このデバイスをペアリングするための SSH PIN を取得することもできます。  この PIN プロンプトは、そのデバイスをターゲットとする最初の Visual Studio 展開を完了するために画面に表示する必要があります。  

![PIN のペアリング](images/devmode-pc-pinpair.PNG)

デバイスを展開ターゲットにする予定の場合にのみ、デバイスの検出を有効にする必要があります。 たとえば、Device Portal を使用してアプリを電話に展開してテストする場合、その電話でデバイスの検出を有効にする必要がありますが、開発用 PC では不要です。

### <a name="optimizations-for-windows-explorer-remote-desktop-and-powershell-desktop-only"></a>エクスプローラー、リモート デスクトップ、PowerShell の最適化 (デスクトップのみ)

 デスクトップ デバイス ファミリの場合、 **[開発者向け]** 設定ページには、開発タスク用 PC を最適化するために使用できる設定へのショートカットが備わっています。 それぞれの設定で、チェック ボックスを選択して **[適用]** をクリックするか、 **[設定の表示]** リンクをクリックして対象オプションの設定ページを開くことができます。


## <a name="notes"></a>メモ
以前のバージョンの Windows 10 Mobile では、クラッシュ ダンプ オプションが [開発者向け設定] メニューに存在していました。  これは、USB 経由だけではなくリモートでも使うことができるように[デバイス ポータル](/windows/uwp/debug-test-perf/device-portal.md)に移動しました。  

Windows 10 PC から Windows 10 デバイスへのアプリの展開に使用できるツールはいくつかあります。 デバイスは両方ともワイヤード (有線) またはワイヤレスでネットワークの同じサブネットに接続されているか、または 2 台のデバイスが USB で接続されている必要があります。 どちらの方法を使用しても、アプリ パッケージ (.appx/.appxbundle) のみがインストールされます。証明書はインストールされません。

-   Windows 10 アプリケーション展開 (WinAppDeployCmd) ツールを使います。 詳しくは、[WinAppDeployCmd ツール](/previous-versions/windows/apps/mt203806(v=vs.140))に関するページをご覧ください。
-   [デバイス ポータル](/windows/uwp/debug-test-perf/device-portal.md)を使用して、ブラウザーから、Windows 10 バージョン 1511 以降を実行しているモバイル デバイスに展開できます。 Device Portal の **[アプリ](/windows/uwp/debug-test-perf/device-portal.md#apps-manager)** ページを使用して、アプリ パッケージ (.appx) をアップロードしてデバイスにインストールします。

## <a name="failure-to-install-developer-mode-package"></a>開発者モード パッケージのインストール エラー
ネットワークや管理上の問題により、開発者モードが正しくインストールされないことがあります。 開発者モード パッケージは、この PC への **リモート** 展開に必要 (ブラウザーから Device Portal を使うか、またはデバイスの検出を使って SSH を有効化する) ですが、ローカル展開には必要ではありません。  これらの問題が発生した場合でも、Visual Studio を使用してローカルでアプリを展開できます。また、このデバイスから他のデバイスへ展開できます。

これらの問題に対する回避策を検索するには、[既知の問題](https://social.msdn.microsoft.com/Forums/en-US/home?forum=Win10SDKToolsIssues&sort=relevancedesc&brandIgnore=True&searchTerm=%22device+portal%22)フォーラムをご覧ください。

> [!NOTE]
> 開発者モードが正しくインストールされない場合、フィードバック要求を提出することをお勧めします。 **フィードバック Hub** アプリで、 **[新しいフィードバックの追加]** を選択し、 **[Developer Platform]** (開発者のプラットフォーム) カテゴリを選択して、 **[開発者モード]** サブカテゴリを選択します。 フィードバックを送信していただくと、発生している問題を Microsoft が解決するのに役立ちます。

### <a name="failed-to-locate-the-package"></a>パッケージ検索エラー

"Developer Mode package couldn’t be located in Windows Update. Error Code 0x80004005 Learn more"   

このエラーは、ネットワーク接続に問題がある場合、エンタープライズ設定になっている場合、またはパッケージが見つからない場合に発生することがあります。

この問題を解決するには:

1. お使いのコンピューターがインターネットに接続されていることを確認します。
2. ドメインに参加しているコンピューターの場合は、ネットワーク管理者に問い合わせます。 開発者モード パッケージは、すべてのオンデマンド機能と同様に、既定では WSUS でブロックされています。
2.1. 現在または以前のリリースで開発者モード パッケージのブロックを解除するためには、WSUS で次のサポート技術情報を許可する必要があります: 4016509、3180030、3197985  
3. [設定] > [更新とセキュリティ] > [Windows Update] で Windows の更新プログラムをチェックします。
4. [設定] > [システム] > [アプリと機能] > [オプション機能を管理する] に、Windows 開発者モード パッケージが存在することを確認します。 ない場合は、Windows はコンピューターの適切なパッケージを検出できません。

上記の手順のいずれかを実行後、修正を確認するために、開発者モードを無効にし、もう一度有効にします。


### <a name="failed-to-install-the-package"></a>パッケージ インストール エラー

"Developer Mode package failed to install. Error code 0x80004005  Learn more"

このエラーは、Windows のビルドと開発者モード パッケージの間に互換性の問題がある場合に発生します。

この問題を解決するには:

1. [設定] > [更新とセキュリティ] > [Windows Update] で Windows の更新プログラムをチェックします。
2. すべての更新プログラムを確実に適用するために、コンピューターを再起動します。


## <a name="use-group-policies-or-registry-keys-to-enable-a-device"></a>グループ ポリシーまたはレジストリ キーを使用してデバイスを有効にする

ほとんどの開発者のためには、設定アプリを使用して、デバイスでデバッグを有効にします。 自動化テストなど特定のシナリオでは、他の方法を使用して、Windows 10 デスクトップ デバイスで開発を有効にできます。  これらの手順では SSH サーバーが有効になったり、リモート展開およびデバッグ用のデバイスを対象にできるようになったりはしない点に注意してください。

Windows 10 Home をお持ちでない場合は、gpedit.msc を使って、グループ ポリシーを設定してデバイスを有効にできます。 Windows 10 Home をお持ちの場合は、regedit または PowerShell コマンドを使ってレジストリ キーを直接設定し、デバイスを有効にしてください。

**gpedit を使ってデバイスを有効にする**

1.  **Gpedit.msc** を実行します。
2.  [ローカル コンピューター ポリシー] &gt; [コンピューターの構成] &gt; [管理用テンプレート] &gt; [Windows コンポーネント] &gt; [アプリ パッケージの展開] の順に移動します。
3.  サイドローディングを有効にするには、ポリシーを編集して次を有効にします。

    -   **信頼できるすべてのアプリのインストールを許可する**

    または

    開発者モードを有効にするには、ポリシーを編集して次の両方を有効にします。

    -   **信頼できるすべてのアプリのインストールを許可する**
    -   **UWP アプリの開発と統合開発環境 (IDE) からのインストールを許可する**

4.  コンピューターを再起動します。

**regedit を使ってデバイスを有効にする**

1.  **regedit** を実行します。
2.  サイドローディングを有効にするには、この DWORD の値を 1 に設定します。

    -   `HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\AppModelUnlock\AllowAllTrustedApps`

    または

    開発者モードを有効にするには、この DWORD の値を 1 に設定します。

    -   `HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\AppModelUnlock\AllowDevelopmentWithoutDevLicense`

**PowerShell を使ってデバイスを有効にする**

1.  管理者特権で PowerShell を実行します。
2.  サイドローディングを有効にするには、このコマンドを実行します。

    ```powershell
    PS C:\WINDOWS\system32> reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\AppModelUnlock" /t REG_DWORD /f /v "AllowAllTrustedApps" /d "1"
    ```

    または

    開発者モードを有効にするには、このコマンドを実行します。

    ```powershell
    PS C:\WINDOWS\system32> reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\AppModelUnlock" /t REG_DWORD /f /v "AllowDevelopmentWithoutDevLicense" /d "1"
    ```

## <a name="upgrade-your-device-from-windows-81-to-windows-10"></a>Windows 8.1 から Windows 10 にデバイスをアップグレードする

Windows 8.1 デバイスでアプリを作成またはサイドローディングするときに、開発者用ライセンスをインストールする必要があります。 Windows 8.1 から Windows 10 にデバイスをアップグレードする場合は、この情報が維持されます。 アップグレードした Windows 10 デバイスからこの情報を削除するには、次のコマンドを実行します。 Windows 8.1 から Windows 10 バージョン 1511 以降に直接アップグレードする場合、この手順は必要ありません。

**開発者用ライセンスを登録解除するには**

1.  管理者特権で PowerShell を実行します。
2.  コマンド `unregister-windowsdeveloperlicense` を実行します。

その後、このトピックで説明されているように、開発用のデバイスを有効にする必要があります。これにより、このデバイスで開発を継続できます。 有効にしない場合、アプリをデバッグしたり、パッケージを作成しようとしたりすると、エラーが発生する可能性があります。 このエラーの例を次に示します。

エラー: DEP0700: アプリケーションの登録に失敗しました。