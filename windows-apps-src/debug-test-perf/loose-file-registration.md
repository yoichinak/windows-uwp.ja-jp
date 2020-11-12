---
title: ルーズ ファイルの登録によるアプリの展開
description: このガイドでは、ルーズ ファイル レイアウトを使用して、Windows 10 アプリをパッケージ化することなく、検証および共有する方法を示します。
ms.date: 06/01/2018
ms.topic: article
keywords: windows 10、uwp、デバイス ポータル、アプリ マネージャー、デプロイ、sdk
ms.localizationpriority: medium
ms.openlocfilehash: 7006d32777e7b3ece5c5b6ed066bd23265b0bbb7
ms.sourcegitcommit: aaa72ddeb01b074266f4cd51740eec8d1905d62d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/06/2020
ms.locfileid: "94339620"
---
# <a name="deploy-an-app-through-loose-file-registration"></a>ルーズ ファイルの登録によるアプリの展開 

このガイドでは、ルーズ ファイル レイアウトを使用して、Windows 10 アプリをパッケージ化することなく、検証および共有する方法を示します。 開発者はルーズ ファイル レイアウトを登録すると、アプリをパッケージ化してインストールする必要なく、アプリをすばやく検証できます。 

## <a name="what-is-a-loose-file-layout"></a>ルーズ ファイル レイアウトとは

ルーズ ファイル レイアウトとは、アプリをパッケージ化する代わりに、フォルダーにアプリの内容を単純に配置する操作です。 パッケージの内容は、パッケージ化されず、フォルダーから "ルーズ" に使用できます。 

> [!WARNING]
> ルーズ ファイル レイアウトの登録は、アプリを積極的に開発している開発者とデザイナーが、アプリをすばやく検証するために使用できます。 この方法は、"社内リリース" またはアプリをフライトするために使用すべきではありません。 最終的な検証は、信頼された証明書で署名されたパッケージ アプリに対して実行することをお勧めします。 

## <a name="advantages-of-loose-file-registration"></a>ルーズ ファイルの登録の利点

- **検証の迅速化** : アプリ ファイルはまだパッケージ化されていないため、ユーザーは迅速にファイル レイアウトを登録してアプリを起動できます。 通常のアプリと同様に、ユーザーはアプリを設計どおりに使用できます。 
- **ネットワーク内での容易な配布** : ルーズ ファイルがローカル ドライブではなくネットワーク共有にある場合、開発者がネットワーク共有の場所をネットワークにアクセスできる他のユーザーに送信して、それらのユーザーがルーズ ファイル レイアウトを登録してアプリを実行できるようにすることができます。 これにより、複数のユーザーが同時にアプリを検証できます。 
- **コラボレーション** : ルーズ ファイルの登録では、開発者とデザイナーは、アプリの登録中にビジュアル資産で作業し続けることができます。 ユーザーはこれらの変更を、アプリの起動後に確認できます。 静的な資産は、この方法でのみ変更できることにご注意ください。 コードまたは動的に作成されたコンテンツを変更する必要がある場合は、アプリを再コンパイルする必要があります。

## <a name="how-to-register-a-loose-file-layout"></a>ルーズ ファイル レイアウトを登録する方法

Windows には、ルーズ ファイル レイアウトをローカル デバイスやリモート デバイスに登録する開発者向けツールが複数用意されています。 選択肢には、`WinDeployAppCmd` (Windows SDK ツール)、Windows デバイス ポータル、PowerShell および [Visual Studio](./deploying-and-debugging-uwp-apps.md#register-layout-from-network) があります。 以下では、これらのツールを使用して、ルーズ ファイルを登録する方法について説明します。 しかし、まず次の設定を確実に完了させてください。

- お使いのデバイスは、Windows 10 Creators Update (ビルド 14965) 以降を使用している必要があります。
- すべてのデバイスで、[開発者モード](/windows/apps/get-started/enable-your-device-for-development)と[デバイスの検出](/windows/apps/get-started/enable-your-device-for-development#device-discovery)が有効になっている必要があります。

> [!IMPORTANT]
> ルーズ ファイルの登録は、ネットワーク共有 (SMB) プロトコルをサポートする次のデバイスでのみ使用できます。デスクトップと Xbox。 

### <a name="register-with-windeployappcmd"></a>WinDeployAppCmd を使用して登録する

Windows 10 Creators Update (ビルド 14965) 以降に対応する SDK ツールを使用している場合は、コマンド プロンプトで `WinDeployAppCmd` コマンドを使用できます。

```cmd
WinAppDeployCmd.exe registerfiles -remotedeploydir <Network Path> -ip <IP Address> -pin <target machine PIN>
```

**Network Path** : アプリのルーズ ファイルへのパス。

**IP Address** : ターゲット マシンの IP アドレス。

**target machine PIN** : ターゲット デバイスとの接続を確立するために求められた場合に指定する PIN。 認証が必要な場合には、`-pin` オプションを指定し、再試行が求められます。 PIN を取得する方法については、「[デバイスの検出](/windows/apps/get-started/enable-your-device-for-development#device-discovery)」を参照してください。

### <a name="windows-device-portal"></a>Windows デバイス ポータル

Windows デバイス ポータルは、すべての Windows 10 デバイスから使用でき、開発者が作業をテストおよび検証するために使用できます。 そのブラウザー UX と REST エンドポイントを使用し、開発者コミュニティのすべてのユーザーにサービスが提供できます。 デバイス ポータルについて詳しくは、「[Windows デバイス ポータルの概要](device-portal.md)」をご覧ください。

デバイス ポータルでルーズ ファイル レイアウトを登録するには、次の手順に従います。

1. 「 [Windows デバイス ポータルの概要](device-portal.md)」の「 **セットアップ** 」セクションの手順に従って、デバイス ポータルに接続します。
1. [Apps Manager]\(アプリ マネージャー\) タブで、 **[Register from Network Share]** \(ネットワーク共有から登録する\) を選択します。
1. ルーズ ファイル レイアウトにネットワーク共有のパスを入力します。 
1. ホスト デバイスにネットワーク共有に対するアクセス権がない場合は、必要な資格情報の入力を求めるプロンプトが表示されます。
1. 登録が完了したら、アプリを起動できます。

デバイス ポータルの [Apps Manager]\(アプリ マネージャー\) ページで **[I want to specify optional packages]** \(オプション パッケージを指定する\) チェックボックスをオンにし、オプションのアプリのネットワーク共有パスを指定することにより、お使いのメイン アプリにオプションのルーズ ファイル レイアウトを登録することもできます。 

### <a name="powershell"></a>PowerShell 

Windows PowerShell でも、ルーズ ファイル レイアウトを登録できますが、ローカル デバイスのみに対してです。 レイアウトをリモート デバイスに登録する必要がある場合は、他の方法のいずれかを使用する必要があります。 

ルーズ ファイル レイアウトを登録するには、PowerShell を起動し、次のように入力します。

```PowerShell
Add-AppxPackage -Register <path to manifest file>
```

## <a name="troubleshooting"></a>トラブルシューティング

### <a name="mapped-network-drives"></a>マップ済みネットワーク ドライブ
現時点では、マップ済みネットワーク ドライブでは、ルーズ ファイルの登録はサポートされていません。 完全なネットワーク共有パスを使用して、マップ済みドライブを参照してください。

### <a name="registration-failure"></a>登録エラー
登録を実行するデバイスには、ファイル レイアウトに対するアクセス権が必要です。 ファイル レイアウトがネットワーク共有にホストされている場合は、そのデバイスにアクセス権を確保します。 

### <a name="modifications-to-visual-assets-arent-being-loaded-in-the-app"></a>ビジュアル資産への変更がアプリに読み込まれない 
アプリは起動時にビジュアル資産を読み込みます。 アプリの起動後にビジュアル資産を変更した場合に最新の変更を表示するには、アプリを再起動する必要があります。