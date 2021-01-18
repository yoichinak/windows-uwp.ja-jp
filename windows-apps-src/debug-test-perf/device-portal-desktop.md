---
ms.assetid: 5c34c78e-9ff7-477b-87f6-a31367cd3f8b
title: デスクトップ用 Windows デバイス ポータル
description: Windows デバイス ポータルによってデスクトップ PC での設定、診断、自動化の機能が提供される方法について説明します。
ms.date: 01/08/2021
ms.topic: article
ms.custom: contperf-fy21q3
keywords: windows 10, uwp, デバイス ポータル
ms.localizationpriority: medium
ms.openlocfilehash: 11bfc81f045887dfdbba9d380eedd6b9ff40709a
ms.sourcegitcommit: 02d220ef0ec0ecd7ed733086ba164ee9653d9602
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/09/2021
ms.locfileid: "98055985"
---
# <a name="windows-device-portal-for-desktop"></a>デスクトップ用 Windows デバイス ポータル

Windows デバイス ポータル (WDP) はデバイスの管理およびデバッグ ツールであり、それを使用すると、Web ブラウザーから HTTP 経由で、デバイスの設定を構成および管理し、診断情報を表示することができます。 他のデバイスでの WDP の詳細については、「[Windows Device Portal の概要](device-portal.md)」を参照してください。

次のことに WDP を使用できます。

- デバイスの設定を管理する (**Windows 設定** アプリに似ています)
- 実行されているプロセスの一覧を確認して操作する
- アプリをインストール、削除、起動、および終了する
- Wi-Fi プロファイルの変更、信号強度の表示、ipconfig の詳細の確認を行う
- CPU、メモリ、I/O、ネットワーク、および GPU の使用率のライブ グラフを表示する
- プロセス ダンプを収集する
- ETW トレースを収集する
- サイドローディングされたアプリの分離ストレージを操作する

## <a name="set-up-windows-device-portal-on-a-desktop-device"></a>デスクトップ デバイスで Windows デバイス ポータルをセットアップする

### <a name="turn-on-developer-mode"></a>開発者モードをオンにする

Windows 10 バージョン 1607 以降では、デスクトップ用の新しい機能の一部は開発者モードが有効なときだけ利用できます。 開発者モードを有効にする方法については、「[デバイスを開発用に有効にする](/windows/apps/get-started/enable-your-device-for-development)」をご覧ください。

> [!IMPORTANT]
> ネットワークや互換性の問題により、お使いのデバイスに開発者モードが正しくインストールされないことがあります。 これらの問題のトラブルシューティングについては、「[デバイスを開発用に有効にする](/windows/apps/get-started/enable-your-device-for-development#failure-to-install-developer-mode-package)」の関連セクションをご覧ください。

### <a name="turn-on-windows-device-portal"></a>Windows デバイス ポータルを有効にする

**[設定]** の **[開発者向け]** セクションで、WDP を有効にすることができます。 Device Portal を有効にするときは、対応するユーザー名とパスワードも作成する必要があります。 Microsoft アカウントやその他の Windows の資格情報を使わないでください。

![設定アプリの [Windows Device Portal]\(Windows デバイス ポータル\) セクション](images/device-portal/device-portal-desk-settings.png)

WDP が有効になると、セクション下部に Web リンクが表示されます。 表示される URL の末尾に付加されたポート番号を記録しておきます。このポート番号は、WDP が有効になるとランダムに生成されるものですが、デスクトップを再起動するまで同じ番号を使う必要があります。

これらのリンクから、ローカル ネットワーク (VPN を含む) 経由、またはローカル ホスト経由のいずれかの方法で WDP に接続できます。 接続すると、次のように表示されます。

![Windows デバイス ポータル](images/device-portal/device-portal-example.png)

### <a name="turn-off-windows-device-portal"></a>Windows デバイス ポータルを無効にする

**[Windows の設定]** の **[開発者向け]** セクションで、WDP を無効にすることができます。

### <a name="connect-to-windows-device-portal"></a>Windows デバイス ポータルに接続する

ローカル ホスト経由で接続するには、ブラウザー ウィンドウを開き、次に示す URI のいずれかを入力します (使用している接続の種類に基づいて)。

- Localhost: `http://127.0.0.1:<PORT>` または `http://localhost:<PORT>`
- Local Network: `https://<IP address of the desktop>:<PORT>`

認証とセキュリティで保護された通信には HTTPS が必要です。

テスト ラボなど、保護された環境で WDP を使用している場合、ローカル ネットワーク上のすべてのユーザーを信頼していて、デバイス上に個人情報が保存されておらず、固有の要件があるのであれば、この認証オプションを無効にできます。 これにより、暗号化されていない通信が有効化され、コンピューターの IP アドレスを知っているすべてのユーザーが接続して制御できるようになります。

## <a name="windows-device-portal-content"></a>Windows デバイス ポータルの内容

WDP には、次の一連のページが用意されています。

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

## <a name="using-windows-device-portal-to-test-and-debug-msix-apps"></a>Windows デバイス ポータルを使用した MSIX アプリのテストとデバッグ


> [!VIDEO https://www.youtube.com/embed/PdgXeOMt4hk]


## <a name="more-windows-device-portal-options"></a>Windows デバイス ポータルのその他のオプション

### <a name="registry-based-configuration"></a>レジストリ ベースの構成

WDP のポート番号 (80、443 など) を選択する場合は、次のレジストリ キーを設定することができます。

- `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\WebManagement\Service` の下
    - `UseDynamicPorts`: 必須の DWORD。 選択したポート番号を保持するには、これを 0 に設定します。
    - `HttpPort`: 必須の DWORD。 WDP で HTTP 接続がリッスンされるポート番号が含まれます。
    - `HttpsPort`: 必須の DWORD。 WDP で HTTPS 接続がリッスンされるポート番号が含まれます。
    
同じレジストリ キー パスの下で、認証要件をオフにすることもできます。
- `UseDefaultAuthorizer` - `0` (無効)、`1` (有効)。  
    - これによって、各接続の基本認証要件と HTTP から HTTPS へのリダイレクトの両方が制御されます。  
    
### <a name="command-line-options-for-windows-device-portal"></a>Windows デバイス ポータル用のコマンド ライン オプション

管理コマンド プロンプトから、WDP の各部分を有効にして構成することができます。 ビルドでサポートされている最新のコマンド セットを表示するには、`webmanagement /?` を実行することができます。

- `sc start webmanagement` または `sc stop webmanagement`
    - サービスをオンまたはオフにします。 ここでも、開発者モードを有効にする必要があります。 
- `-Credentials <username> <password>` 
    - WDP 用のユーザー名とパスワードを設定します。 ユーザー名は基本認証の標準に準拠している必要があるため、コロン (:) を含めることはできません。また、ブラウザーではすべての文字セットが標準的な方法で解析されるわけではないため、標準の ASCII 文字 (例: [a-zA-Z0-9]) で構成されている必要があります。  
- `-DeleteSSL` 
    - このオプションを指定すると、HTTPS 接続に使用される SSL 証明書のキャッシュがリセットされます。 予期される証明書の警告ではなく、バイパスすることができない TLS 接続エラーが発生した場合は、このオプションで問題が解決される可能性があります。 
- `-SetCert <pfxPath> <pfxPassword>`
    - 詳細については、「[カスタムの SSL 証明書で Device Portal のプロビジョニングを行う](./device-portal-ssl.md)」を参照してください。  
    - このオプションを指定すると、独自の SSL 証明書をインストールして、通常 WDP に表示される SSL 警告ページを修正することができます。 
- `-Debug <various options for authentication, port selection, and tracing level>`
    - 特定の構成と視覚的なデバッグ メッセージを使用して、WDP のスタンドアロン バージョンを実行します。 これは、[パッケージ プラグイン](./device-portal-plugin.md)を構築するときに最も役立ちます。 
    - これをシステムとして実行して、パッケージ プラグインを完全にテストする方法について詳しくは、[MSDN Magazine の記事](/archive/msdn-magazine/2017/october/windows-device-portal-write-a-windows-device-portal-packaged-plug-in)をご覧ください。

## <a name="troubleshooting"></a>トラブルシューティング

Windows デバイス ポータルの設定時に発生する可能性のある一般的なエラーを次に示します。

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
* [Windows デバイス ポータル コア API リファレンス](./device-portal-api-core.md)