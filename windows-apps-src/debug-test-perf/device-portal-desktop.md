---
ms.assetid: 5c34c78e-9ff7-477b-87f6-a31367cd3f8b
title: Windows デスクトップ用 Device Portal
description: Windows デスクトップで Windows Device Portal の診断と自動化を利用する方法について説明します。
ms.date: 08/20/2020
ms.topic: article
ms.custom: contperfq1
keywords: windows 10, uwp, デバイス ポータル
ms.localizationpriority: medium
ms.openlocfilehash: f06a3c933060a7309604ae8dec49455ac3bd02ab
ms.sourcegitcommit: 41dbee78d827107c224a9136c26f90be4dfe12ad
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/21/2020
ms.locfileid: "90845571"
---
# <a name="device-portal-for-windows-desktop"></a>Windows デスクトップ用 Device Portal

Windows デバイス ポータルは、診断情報の表示や、Web ブラウザーを使用して HTTP 経由でデスクトップ PC 操作を行えるようにするデバッグ ツールです。 他のデバイスをデバッグするには、「[Windows デバイス ポータルの概要](device-portal.md)」を参照してください。


Device Portal を使用すると、次の操作を実行できます。
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
> ネットワークや互換性の問題により、お使いのデバイスに開発者モードが正しくインストールされないことがあります。 これらの問題のトラブルシューティングについては、「[デバイスを開発用に有効にする](../get-started/enable-your-device-for-development.md#failure-to-install-developer-mode-package)」の関連セクションをご覧ください。

### <a name="turn-on-device-portal"></a>Device Portal をオンにする

**[設定]** の **[開発者向け]** セクションで、Device Portal を有効にすることができます。 Device Portal を有効にするときは、対応するユーザー名とパスワードも作成する必要があります。 Microsoft アカウントやその他の Windows の資格情報を使わないでください。

![設定アプリの [Device Portal] セクション](images/device-portal/device-portal-desk-settings.png)

Device Portal が有効になると、セクション下部に Web リンクが表示されます。 表示される URL の末尾に付加されたポート番号をメモします。このポート番号は、Device Portal が有効になるとランダムに生成されるものですが、デスクトップを再起動するまで同じ番号を使う必要があります。

これらのリンクから、ローカル ネットワーク (VPN を含む) 経由、またはローカル ホスト経由のいずれかの方法で Device Portal に接続できます。 接続すると、次のように表示されます。

![デバイス ポータル](images/device-portal/device-portal-example.png)


### <a name="turn-off-device-portal"></a>デバイス ポータルをオフにする

**[設定]** の **[開発者向け]** セクションで、デバイス ポータルを無効にすることができます。

### <a name="connect-to-device-portal"></a>Device Portal に接続する

ローカル ホスト経由で接続するには、ブラウザー ウィンドウを開き、使用している接続の種類に応じて次に示すアドレスを入力します。

* Localhost: `http://127.0.0.1:<PORT>` または `http://localhost:<PORT>`
* Local Network: `https://<IP address of the desktop>:<PORT>`

認証とセキュリティで保護された通信には HTTPS が必要です。

テスト ラボなど、保護された環境でデバイス ポータルを使用している場合、ローカル ネットワーク上のすべてのユーザーを信頼していて、デバイス上に個人情報が保存されておらず、固有の要件があるのであれば、この認証オプションを無効にできます。 これにより、暗号化されていない通信が有効化され、コンピューターの IP アドレスを知っているすべてのユーザーが接続して制御できるようになります。

## <a name="device-portal-content-on-windows-desktop"></a>Windows デスクトップ上の Device Portal のコンテンツ

Windows デスクトップのデバイス ポータルには、「[Windows デバイス ポータルの概要](device-portal.md)」で説明されている一連のページが表示されます。

- アプリ マネージャー
- Xbox Live
- エクスプローラー
- 実行中のプロセス
- パフォーマンス
- デバッグ
- ETW (Windows イベント トレーシング) ログ
- パフォーマンス トレース
- デバイス マネージャー
- Bluetooth
- ネットワーク
- クラッシュ データ
- 特徴
- Mixed Reality
- ストリーミング インストール デバッガー
- 場所
- スクラッチ

## <a name="using-device-portal-for-windows-desktop-to-test-and-debug-msix-apps"></a>Windows デスクトップ用のデバイス ポータルを使用した MSIX アプリのテストとデバッグ


> [!VIDEO https://www.youtube.com/embed/PdgXeOMt4hk]


## <a name="more-device-portal-options"></a>Device Portal のその他のオプション

### <a name="registry-based-configuration-for-device-portal"></a>Device Portal のレジストリ ベースの構成

デバイス ポータルのポート番号 (80、443 など) を選択する場合は、次のレジストリ キーを設定することができます。

- `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\WebManagement\Service` の下
    - `UseDynamicPorts`: 必須の DWORD。 選択したポート番号を保持するには、これを 0 に設定します。
    - `HttpPort`: 必須の DWORD。 Device Portal が HTTP 接続をリッスンするポート番号を指定します。    
    - `HttpsPort`: 必須の DWORD。 Device Portal が HTTPS 接続をリッスンするポート番号を指定します。
    
同じレジストリ キー パスの下で、認証要件をオフにすることもできます。
- `UseDefaultAuthorizer` - `0` (無効)、`1` (有効)。  
    - これによって、各接続の基本認証要件と HTTP から HTTPS へのリダイレクトの両方が制御されます。  
    
### <a name="command-line-options-for-device-portal"></a>Device Portal のコマンド ライン オプション
管理コマンド プロンプトから、Device Portal の各部分を有効にして構成することができます。 ビルドでサポートされている最新のコマンド セットを表示するには、`webmanagement /?` を実行することができます。

- `sc start webmanagement` または `sc stop webmanagement` 
    - サービスをオンまたはオフにします。 ここでも、開発者モードを有効にする必要があります。 
- `-Credentials <username> <password>` 
    - Device Portal のユーザー名とパスワードを設定します。 ユーザー名は基本認証の標準に準拠している必要があるため、コロン (:) を含めることはできません。また、ブラウザーではすべての文字セットが標準的な方法で解析されるわけではないため、標準の ASCII 文字 (例: [a-zA-Z0-9]) で構成されている必要があります。  
- `-DeleteSSL` 
    - このオプションを指定すると、HTTPS 接続に使用される SSL 証明書のキャッシュがリセットされます。 予期される証明書の警告ではなく、バイパスすることができない TLS 接続エラーが発生した場合は、このオプションで問題が解決される可能性があります。 
- `-SetCert <pfxPath> <pfxPassword>`
    - 詳しくは、「[カスタムの SSL 証明書で Device Portal をプロビジョニングする](./device-portal-ssl.md)」をご覧ください。  
    - このオプションを指定すると、独自の SSL 証明書をインストールして、通常 Device Portal に表示される SSL 警告ページを修正することができます。 
- `-Debug <various options for authentication, port selection, and tracing level>`
    - 特定の構成と視覚的なデバッグ メッセージを使用して、Device Portal のスタンドアロン バージョンを実行します。 これは、[パッケージ プラグイン](./device-portal-plugin.md)を構築するときに最も役立ちます。 
    - これをシステムとして実行して、パッケージ プラグインを完全にテストする方法について詳しくは、[MSDN Magazine の記事](/archive/msdn-magazine/2017/october/windows-device-portal-write-a-windows-device-portal-packaged-plug-in)をご覧ください。

## <a name="troubleshooting"></a>トラブルシューティング

デバイス ポータルの設定時に発生する可能性のある一般的なエラーを次に示します。

### <a name="windowsupdatesearch-returns-invalid-number-of-updates-0x800f0950-cbs_e_invalid_windows_update_count"></a>WindowsUpdateSearch から無効な更新プログラムの数が返されました (0x800f0950 CBS_E_INVALID_WINDOWS_UPDATE_COUNT)

Windows 10 のプレリリース ビルドで開発者パッケージをインストールしようとすると、このエラーが発生することがあります。 これらのオンデマンド機能 (FoD) パッケージは、Windows Update でホストされ、プレリリース ビルドでダウンロードするには、フライティングをオプトインする必要があります。 インストールで適切なビルドとリングの組み合わせのフライティングをオプトインしていない場合、ペイロードはダウンロードできません。 以下を再確認してください。

1. **[設定] > [更新とセキュリティ] > [Windows Insider Program]** に移動して、 **[Windows Insider アカウント]** セクションに正しいアカウント情報が含まれていることを確認します。 このセクションが表示されない場合は、 **[Windows Insider アカウントをリンクする]** を選択し、電子メール アカウントを追加して、 **[Windows Insider アカウント]** の見出しの下にそれが表示されていることを確認します (新しく追加したアカウントを実際にリンクするために、 **[Windows Insider アカウントをリンクする]** をもう一度選択する必要がある場合があります)。
 
2. **[どのようなコンテンツの受け取りを希望されますか?]** の下で、 **[Active development of Windows]\(開発中の Windows\)** が選択されていることを確認します。
 
3. **[新しいビルドを取得する頻度はどの程度を希望されますか?]** の下で、 **[Windows Insider Fast]\(Windows Insider - 高速\)** が選択されていることを確認します。
 
4. これで FoD をインストールできるようになります。 "Windows Insider - 高速" を使用していて、FoD を引き続きインストールできないことを確認した場合は、フィードバックを提供し、**C:\Windows\Logs\CBS** の下のログ ファイルを添付してください。

### <a name="sc-startservice-openservice-failed-1060-the-specified-service-does-not-exist-as-an-installed-service"></a>[SC] StartService: OpenService に失敗しました 1060: 指定されたサービスは、インストール済みのサービスとして存在しません

開発者パッケージがインストールされていない場合、このエラーが表示されることがあります。 開発者パッケージがなければ、Web 管理サービスはありません。 開発者パッケージをもう一度インストールしてみてください。

### <a name="cbs-cannot-start-download-because-the-system-is-on-metered-network-cbs_e_metered_network"></a>システムが従量制課金接続 (CBS_E_METERED_NETWORK) 上にあるため、CBS のダウンロードを開始できません

従量制課金接続を使用している場合は、このエラーが表示されることがあります。 従量制課金接続で開発者パッケージをダウンロードすることはできません。

## <a name="see-also"></a>関連項目

* [Windows Device Portal の概要](device-portal.md)
* [デバイス ポータル コア API リファレンス](./device-portal-api-core.md)