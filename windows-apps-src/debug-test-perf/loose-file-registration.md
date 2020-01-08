---
title: ルーズ ファイルの登録によるアプリの展開
description: このガイドでは、ルーズ ファイル レイアウトを使用して、Windows 10 アプリをパッケージ化することなく、検証および共有する方法を示します。
ms.date: 06/01/2018
ms.topic: article
keywords: windows 10、uwp、デバイスポータル、アプリマネージャー、デプロイ、sdk
ms.localizationpriority: medium
ms.openlocfilehash: 7bf3dab97be67a3b97aca4b3132bd9fe18691d15
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/06/2020
ms.locfileid: "75681933"
---
# <a name="deploy-an-app-through-loose-file-registration"></a>ルーズ ファイルの登録によるアプリの展開 

このガイドでは、ルーズ ファイル レイアウトを使用して、Windows 10 アプリをパッケージ化することなく、検証および共有する方法を示します。 ファイルレイアウトを厳密に登録すると、開発者はアプリをパッケージ化してインストールすることなく、アプリをすばやく検証することができます。 

## <a name="what-is-a-loose-file-layout"></a>圧縮されていないファイルレイアウトとは

疎ファイルレイアウトは、パッケージ化プロセスを実行するのではなく、フォルダーにアプリの内容を配置する操作です。 パッケージの内容は、フォルダーでは使用できず、パッケージ化されません。 

> [!WARNING]
> 厳密でないファイルレイアウト登録は、開発者とデザイナーがアクティブな開発中にアプリをすばやく検証できるようにするためのものです。 この方法を使用して、アプリを "社内リリース" またはフライトすることはできません。 最終的な検証は、信頼された証明書で署名されたパッケージアプリで実行することをお勧めします。 

## <a name="advantages-of-loose-file-registration"></a>緩いファイル登録の利点

- **クイック検証**-アプリファイルは既にアンパックされているため、ユーザーは簡単にファイルレイアウトを登録してアプリを起動できます。 通常のアプリと同じように、ユーザーはアプリを設計どおりに使用できます。 
- **ネットワーク内での簡単な配布**-圧縮されていないファイルがローカルドライブではなくネットワーク共有に配置されている場合、開発者はネットワーク共有の場所をネットワークにアクセスできる他のユーザーに送信できます。また、これらのユーザーは、圧縮されていないファイルレイアウトを登録してアプリを実行できます。 これにより、複数のユーザーが同時にアプリを検証することができます。 
- **コラボレーション**-厳密でないファイル登録により、開発者とデザイナーは、アプリの登録中にビジュアルアセットを操作し続けることができます。 ユーザーがアプリを起動すると、これらの変更が表示されます。 この方法で変更できるのは静的アセットのみであることに注意してください。 コードまたは動的に作成されたコンテンツを変更する必要がある場合は、アプリを再コンパイルする必要があります。

## <a name="how-to-register-a-loose-file-layout"></a>圧縮されるファイルレイアウトを登録する方法

Windows には、ローカルデバイスとリモートデバイスに疎ファイルレイアウトを登録するための複数の開発者ツールが用意されています。 `WinDeployAppCmd` (Windows SDK ツール)、Windows デバイスポータル、PowerShell、 [Visual Studio](https://docs.microsoft.com/windows/uwp/debug-test-perf/deploying-and-debugging-uwp-apps#register-layout-from-network)から選択できます。 以下では、これらのツールを使用して、圧縮されていないファイルを登録する方法について説明します。 ただし、まず、次のセットアップが完了していることを確認します。

- デバイスは、Windows 10 の作成者の更新プログラム (ビルド 14965) 以降である必要があります。
- すべてのデバイスで[開発者モード](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development)と[デバイスの検出](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development#device-discovery)を有効にする必要があります。

> [!IMPORTANT]
> 圧縮されていないファイルの登録は、ネットワーク共有 (SMB) プロトコルをサポートするデバイス (デスクトップと Xbox) でのみ使用できます。 

### <a name="register-with-windeployappcmd"></a>WinDeployAppCmd に登録する

Windows 10 の作成者の更新プログラム (ビルド 14965) 以降に対応する SDK ツールを使用している場合は、コマンドプロンプトで `WinDeployAppCmd` コマンドを使用できます。

```cmd
WinAppDeployCmd.exe registerfiles -remotedeploydir <Network Path> -ip <IP Address> -pin <target machine PIN>
```

**ネットワークパス**–アプリの圧縮されていないファイルへのパス。

**Ip アドレス**–ターゲットコンピューターの ip アドレス。

**ターゲットコンピューターの pin** –必要に応じて、ターゲットデバイスとの接続を確立するための pin です。 認証が必要な場合は、`-pin` オプションを使用して再試行するように求められます。 PIN を取得する方法については、「[デバイスの検出](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development#device-discovery)」を参照してください。

### <a name="windows-device-portal"></a>Windows Device Portal

Windows デバイスポータルは、すべての Windows 10 デバイスで使用でき、開発者が作業をテストおよび検証するために使用されます。 ブラウザー UX と REST エンドポイントを使用して、開発者コミュニティのすべてのユーザーに対応ます。 デバイスポータルの詳細については、「 [Windows デバイスポータルの概要](device-portal.md)」を参照してください。

デバイスポータルに圧縮されていないファイルレイアウトを登録するには、次の手順に従います。

1. [Windows デバイスポータルの概要](device-portal.md)に関するセクションの手順に従っ**て、デバイス**ポータルに接続します。
1. [アプリマネージャー] タブで、[**ネットワーク共有から登録**する] を選択します。
1. 圧縮されていないファイルレイアウトへのネットワーク共有パスを入力します。 
1. ホストデバイスにネットワーク共有へのアクセス権がない場合は、必要な資格情報を入力するように求められます。
1. 登録が完了したら、アプリを起動できます。

デバイスポータルの アプリマネージャー ページで、**オプションのパッケージを指定する** チェックボックスをオンにして、オプションのアプリのネットワーク共有パスを指定することで、メインアプリに対してオプションの緩いファイルレイアウトを登録することもできます。 

### <a name="powershell"></a>PowerShell 

また、Windows PowerShell では、ローカルデバイスにのみ、ルースファイルレイアウトを登録できます。 レイアウトをリモートデバイスに登録する必要がある場合は、他の方法のいずれかを使用する必要があります。 

圧縮されていないファイルレイアウトを登録するには、PowerShell を起動し、次のように入力します。

```PowerShell
Add-AppxPackage -Register <path to manifest file>
```

## <a name="troubleshooting"></a>[トラブルシューティング]

### <a name="mapped-network-drives"></a>マップ済みネットワーク ドライブ
現時点では、割り当てられたネットワークドライブは、緩いファイル登録ではサポートされていません。 ネットワーク共有パスが完全である、マップされたドライブを参照してください。

### <a name="registration-failure"></a>登録エラー
登録が行われているデバイスは、ファイルレイアウトにアクセスできる必要があります。 ファイルのレイアウトがネットワーク共有でホストされている場合は、デバイスにアクセス権があることを確認します。 

### <a name="modifications-to-visual-assets-arent-being-loaded-in-the-app"></a>ビジュアルアセットの変更がアプリに読み込まれていません 
アプリは起動時にビジュアルアセットを読み込みます。 アプリの起動後にビジュアルアセットに変更が加えられた場合、最新の変更を表示するには、アプリを再起動する必要があります。
