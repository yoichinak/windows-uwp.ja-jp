---
ms.assetid: 5c34c78e-9ff7-477b-87f6-a31367cd3f8b
title: Windows デスクトップ用 Device Portal
description: Windows デスクトップで Windows Device Portal の診断と自動化を利用する方法について説明します。
ms.date: 02/06/2019
ms.topic: article
keywords: windows 10、uwp、デバイスポータル
ms.localizationpriority: medium
ms.openlocfilehash: 73f7e827c0ec8ca289d3523da06601de978a91d2
ms.sourcegitcommit: 0426013dc04ada3894dd41ea51ed646f9bb17f6d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/06/2020
ms.locfileid: "78853429"
---
# <a name="device-portal-for-windows-desktop"></a>Windows デスクトップ用 Device Portal

Windows Device Portal では、診断情報を表示し、ブラウザー ウィンドウから HTTP 経由でデスクトップを操作することができます。 Device Portal を使用すると、次の操作を実行できます。
- 実行されているプロセスの一覧を確認して操作する
- アプリをインストール、削除、起動、および終了する
- Wi-Fi プロファイルの変更、シグナルの強さの表示、ipconfig の確認を行う
- CPU、メモリ、I/O、ネットワーク、および GPU の使用率のライブ グラフを表示する
- プロセス ダンプを収集する
- ETW トレースを収集する 
- サイドローディングされたアプリの分離ストレージを操作する

## <a name="set-up-device-portal-on-windows-desktop"></a>Windows デスクトップで Device Portal をセットアップする

### <a name="turn-on-developer-mode"></a>開発者モードをオンにする

Windows 10 バージョン 1607 以降では、デスクトップ用の新しい機能の一部は開発者モードが有効なときだけ利用できます。 開発者モードを有効にする方法については、「[デバイスを開発用に有効にする](../get-started/enable-your-device-for-development.md)」をご覧ください。

> [!IMPORTANT]
> ネットワークや互換性の問題により、お使いのデバイスに開発者モードが正しくインストールされないことがあります。 これらの問題のトラブルシューティングについては、「[デバイスを開発用に有効にする](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development#failure-to-install-developer-mode-package)」の関連セクションをご覧ください。

### <a name="turn-on-device-portal"></a>Device Portal をオンにする

**[設定]** の **[開発者向け]** セクションで、Device Portal を有効にすることができます。 Device Portal を有効にするときは、対応するユーザー名とパスワードも作成する必要があります。 Microsoft アカウントやその他の Windows の資格情報を使わないでください。 

![設定アプリの [Device Portal] セクション](images/device-portal/device-portal-desk-settings.png) 

Device Portal が有効になると、セクション下部に Web リンクが表示されます。 表示される URL の末尾に付加されたポート番号をメモします。このポート番号は、Device Portal が有効になるとランダムに生成されるものですが、デスクトップを再起動するまで同じ番号を使う必要があります。 

これらのリンクから、ローカル ネットワーク (VPN を含む) 経由、またはローカル ホスト経由のいずれかの方法で Device Portal に接続できます。

### <a name="connect-to-device-portal"></a>Device Portal に接続する

ローカル ホスト経由で接続するには、ブラウザー ウィンドウを開き、使用している接続の種類に関して次に示すアドレスを入力します。

* Localhost: `http://127.0.0.1:<PORT>` または `http://localhost:<PORT>`
* Local Network: `https://<IP address of the desktop>:<PORT>`

認証とセキュリティで保護された通信には HTTPS が必要です。

テスト ラボなど、保護された環境で Device Portal を使っている場合、ローカル ネットワーク上のすべてのユーザーを信頼していて、デバイス上に個人情報が保存されておらず、固有の要件もない場合は、[認証] オプションを無効にできます。 これにより、暗号化されていない通信が有効化され、コンピューターの IP アドレスを知っているすべてのユーザーが接続して制御できるようになります。

## <a name="device-portal-content-on-windows-desktop"></a>Windows デスクトップ上の Device Portal のコンテンツ

Windows デスクトップの Device Portal では、標準のページのセットが提供されます。 これらの詳しい説明については、「[Windows Device Portal の概要](device-portal.md)」をご覧ください。

- アプリ マネージャー
- エクスプローラー
- 実行中のプロセス
- パフォーマンス テスト
- デバッグ
- Windows イベント トレーシング (ETW)
- パフォーマンス トレース
- デバイス マネージャー
- ネットワーク
- クラッシュ データ
- 機能
- Mixed Reality
- ストリーミング インストール デバッガー
- 場所
- スクラッチ

## <a name="more-device-portal-options"></a>Device Portal のその他のオプション

### <a name="registry-based-configuration-for-device-portal"></a>Device Portal のレジストリ ベースの構成

デバイス ポータルのポート番号 (80、443 など) を選択する場合は、次のレジストリ キーを設定することができます。

- `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\WebManagement\Service`
    - `UseDynamicPorts`: 必須の DWORD です。 選択したポート番号を保持するには、これを 0 に設定します。
    - `HttpPort`: 必須の DWORD です。 Device Portal が HTTP 接続をリッスンするポート番号を指定します。    
    - `HttpsPort`: 必須の DWORD です。 Device Portal が HTTPS 接続をリッスンするポート番号を指定します。
    
同じレジストリ キー パスの下で、認証要件をオフにすることもできます。
- 無効にするには `UseDefaultAuthorizer` - `0` を、有効にするには `1` します。  
    - これによって、各接続の基本認証要件と HTTP から HTTPS へのリダイレクトの両方が制御されます。  
    
### <a name="command-line-options-for-device-portal"></a>Device Portal のコマンド ライン オプション
管理コマンド プロンプトから、Device Portal の各部分を有効にして構成することができます。 ビルドでサポートされている最新のコマンドセットを確認するには、を実行し `webmanagement /?`

- `sc start webmanagement` または `sc stop webmanagement` 
    - サービスをオンまたはオフにします。 ここでも、開発者モードを有効にする必要があります。 
- `-Credentials <username> <password>` 
    - Device Portal のユーザー名とパスワードを設定します。 ユーザー名は基本認証標準に準拠している必要があるため、コロン (:) を含めることはできませんとは標準の ASCII 文字から構築する必要があります。たとえば、ブラウザーは標準の方法で完全な文字セットを解析しませんが、zA-A-za-z0-9 のようになります。  
- `-DeleteSSL` 
    - このオプションを指定すると、HTTPS 接続に使用される SSL 証明書のキャッシュがリセットされます。 予期される証明書の警告ではなく、バイパスすることができない TLS 接続エラーが発生した場合は、このオプションで問題が解決される可能性があります。 
- `-SetCert <pfxPath> <pfxPassword>`
    - 詳しくは、「[カスタムの SSL 証明書で Device Portal をプロビジョニングする](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-ssl)」をご覧ください。  
    - このオプションを指定すると、独自の SSL 証明書をインストールして、通常 Device Portal に表示される SSL 警告ページを修正することができます。 
- `-Debug <various options for authentication, port selection, and tracing level>`
    - 特定の構成と視覚的なデバッグ メッセージを使用して、Device Portal のスタンドアロン バージョンを実行します。 これは、[パッケージ プラグイン](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-plugin)を構築するときに最も役立ちます。 
    - これをシステムとして実行して、パッケージ プラグインを完全にテストする方法について詳しくは、[MSDN Magazine の記事](https://msdn.microsoft.com/magazine/mt826332.aspx)をご覧ください。

## <a name="common-errors-and-issues"></a>一般的なエラーと問題

デバイスポータルの設定時に発生する可能性のある一般的なエラーを次に示します。

### <a name="windowsupdatesearch-returns-invalid-number-of-updates-0x800f0950-cbs_e_invalid_windows_update_count"></a>WindowsUpdateSearch は無効な数の更新プログラム (0x800f0950 CBS_E_INVALID_WINDOWS_UPDATE_COUNT) を返します

Windows 10 のプレリリースビルドで開発者パッケージをインストールしようとすると、このエラーが発生することがあります。 これらの機能オンデマンド (レ d) パッケージは Windows Update でホストされ、プレリリースビルドでダウンロードするには、flighting を選択する必要があります。 適切なビルドとリングの組み合わせに対してフライトを選択していない場合、ペイロードはダウンロードできません。 次の点を確認します。

1. [設定] に移動して **& セキュリティ > Windows Insider program に更新**し、 **[windows insider account]** セクションに正しいアカウント情報が記載されていることを確認 > ます。 このセクションが表示されない場合は、[ **Windows insider account をリンク**する] を選択し、電子メールアカウントを追加して、 **windows insider アカウント**の見出しの下に表示されることを確認します (新しく追加したアカウントを実際にリンクするには、[ **windows Insider アカウント**をもう一度リンクする] を選択する必要があります)。
 
2. **[どのような種類のコンテンツを受信しますか?]** で、 **[Windows のアクティブな開発]** が選択されていることを確認します。
 
3. **[新しいビルドを取得するペース]** を指定してください で、 **[Windows Insider Fast]** が選択されていることを確認します。
 
4. これで、ディレクトリをインストールできるようになります。 Windows Insider Fast を使用していても、まだインストールできないことを確認した場合は、フィードバックを提供し、 **C:\Windows\Logs\CBS**の下にログファイルを添付してください。

### <a name="sc-startservice-openservice-failed-1060-the-specified-service-does-not-exist-as-an-installed-service"></a>SCStartService: OpenService が失敗しました 1060: 指定されたサービスは、インストールされたサービスとして存在しません

開発者パッケージがインストールされていない場合は、このエラーが表示されることがあります。 開発者パッケージを使用しない場合、web 管理サービスはありません。 開発者パッケージをもう一度インストールしてみてください。

### <a name="cbs-cannot-start-download-because-the-system-is-on-metered-network-cbs_e_metered_network"></a>システムが従量制ネットワーク (CBS_E_METERED_NETWORK) 上にあるため、CBS をダウンロードできません

従量制インターネット接続を使用している場合は、このエラーが表示されることがあります。 従量制課金接続で開発者パッケージをダウンロードすることはできません。

## <a name="see-also"></a>参照

* [Windows デバイスポータルの概要](device-portal.md)
* [デバイスポータルコア API リファレンス](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-api-core)
