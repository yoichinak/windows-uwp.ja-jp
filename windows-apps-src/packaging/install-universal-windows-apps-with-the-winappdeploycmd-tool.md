---
author: laurenhughes
ms.assetid: 6AA037C0-35ED-4B9C-80A3-5E144D7EE94B
title: WinAppDeployCmd.exe ツールを使ったアプリのインストール
description: Windows アプリケーションの展開ツール (WinAppDeployCmd.exe) は、Windows 10 PC から Windows 10 デバイスにユニバーサル Windows プラットフォーム (UWP) アプリを展開するために利用できるコマンド ライン ツールです。
ms.author: lahugh
ms.date: 03/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 664b2e92c40fa619150a2c3fd527194e8e983039
ms.sourcegitcommit: b8c77ac8e40a27cf762328d730c121c28de5fbc4
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/21/2018
ms.locfileid: "1673009"
---
# <a name="install-apps-with-the-winappdeploycmdexe-tool"></a>WinAppDeployCmd.exe ツールを使ったアプリのインストール


Windows アプリケーションの展開ツール (WinAppDeployCmd.exe) は、Windows 10 PC から Windows 10 デバイスにユニバーサル Windows プラットフォーム (UWP) アプリを展開するために利用できるコマンド ライン ツールです。 このツールを使うと、Windows 10 デバイスが USB で接続されているか同じサブネットにあれば、.appx パッケージを展開できます。Microsoft Visual Studio やそのアプリ用のソリューションは不要です。 最初にパッケージ化することなく、リモート PC や Xbox One にアプリを展開することもできます。 この記事では、このツールを使って UWP アプリをインストールする方法について説明します。

Windows 10 SDK がインストールされていれば、WinAppDeployCmd ツールをコマンド プロンプトまたはスクリプト ファイルから実行できます。 WinAppDeployCmd.exe でアプリをインストールすると、.appx ファイルや AppxManifest(ルーズ ファイルの場合) を使って Windows 10 デバイスにアプリがサイドローディングされます。 このコマンドによって、アプリに必要な証明書はインストールされません。 アプリを実行するには、Windows 10 デバイスが開発者モードになっているか、証明書が既にインストールされている必要があります。

モバイル デバイスに展開するには、最初にパッケージを作成する必要があります。 詳しくは、[こちら](https://msdn.microsoft.com/windows/uwp/packaging/packaging-uwp-apps)をご覧ください。

**WinAppDeployCmd.exe** ツールは、Windows 10 PC の **C:\\Program Files (x86)\\Windows Kits\\10\\bin\\<SDK Version>\\x86\\WinAppDeployCmd.exe** にあります (SDK のインストール パスに基づきます)。 
> [!NOTE]
> SDK のバージョン 15063 以降では、SDK はバージョン固有のフォルダー内にサイド バイ サイドでインストールされます。  以前の SDK (14393 以前) は、親フォルダーに直接書き込まれます。

まず、Windows 10 デバイスを同じサブネットに接続するか、USB 接続で Windows 10 コンピューターに直接接続します。 この記事ではその後、このコマンドの次の構文と例を使って UWP アプリを展開します。

## <a name="winappdeploycmd-syntax-and-options"></a>WinAppDeployCmd の構文とオプション

**WinAppDeployCmd.exe** で使用される一般的な構文を次に示します。
```syntax
WinAppDeployCmd command -option <argument>
```

さまざまなコマンドを使用する他の構文例を次に示します。
```syntax
WinAppDeployCmd devices
WinAppDeployCmd devices <x>
WinAppDeployCmd install -file <path> -ip <address>
WinAppDeployCmd install -file <path> -guid <address> -pin <p>
WinAppDeployCmd install -file <path> -ip <address> -dependency <a> <b> 
WinAppDeployCmd install -file <path> -guid <address> -dependency <a> <b>
WinAppDeployCmd uninstall -file <path>
WinAppDeployCmd uninstall -package <name>
WinAppDeployCmd update -file <path>
WinAppDeployCmd list -ip <address>
WinAppDeployCmd list -guid <address>
WinAppDeployCmd deployfiles -file <path> -remotedeploydir <remoterelativepath> -ip <address>
WinAppDeployCmd registerfiles -remotedeploydir <remoterelativepath> -ip <address>
WinAppDeployCmd addcreds -credserver <server> -credusername <username> -credpassword <password> -ip <address>
WinAppDeployCmd getcreds -credserver <server> -ip <address>
WinAppDeployCmd deletecreds -credserver <server> -ip <address>
```

ターゲット デバイスでアプリをインストール/アンインストールしたり、既にインストールされているアプリを更新したりできます。 既にインストールされているアプリによって保存されたデータや設定を保持するには、**install** オプションの代わりに **update** オプションを使います。

次の表では、**WinAppDeployCmd.exe** のコマンドについて説明します。


| **コマンド**  | **説明**                                                     |
|--------------|---------------------------------------------------------------------|
| devices      | 利用可能なネットワーク デバイスの一覧を表示します。                         |
| install      | ターゲット デバイスに UWP アプリ パッケージをインストールします。                     |
| update       | ターゲット デバイスに既にインストールされている UWP アプリを更新します。    |
| list         | ターゲット デバイスに既にインストールされている UWP アプリの一覧を表示します。 |
| uninstall    | 指定したアプリ パッケージをターゲット デバイスからアンインストールします。         |
| deployfiles  | ターゲット パスにあるルーズ ファイル アプリをデバイスのリモート相対パスにコピーします。|
| registerfiles| リモートの展開ディレクトリにルーズ ファイル アプリを登録します。         |
| addcreds     | Xbox に資格情報を追加して、アプリを登録するためにネットワークの場所にアクセスすることを許可します。|
| getcreds     | ネットワーク共有からアプリケーションを実行するときにターゲットが使用するネットワーク資格情報を取得します。|
| deletecreds  | ネットワーク共有からアプリケーションを実行するときにターゲットが使用するネットワーク資格情報を削除します。|


次の表では、**WinAppDeployCmd.exe** のオプションについて説明します。


| **コマンド**  | **説明**  |
|--------------|------------------|
| -h (-help)       | コマンド、オプション、引数を表示します。 |
| -ip              | ターゲット デバイスの IP アドレス。 |
| -g (-guid)       | ターゲット デバイスの一意の識別子。|
| -d (-dependency) | (省略可能) パッケージの依存関係のそれぞれの依存パスを指定します。 パスを指定しない場合、ツールはアプリ パッケージのルート ディレクトリと SDK のディレクトリで依存関係を探します。|
| -f (-file)       | インストール、更新、またはアンインストールするアプリ パッケージのファイル パス。|
| -p (-package)    | アンインストールするアプリ パッケージの完全なパッケージ名  (list コマンドを使って、デバイスに既にインストールされているパッケージの完全な名前を見つけることができます)。 |
| -pin             | ターゲット デバイスとの接続を確立するために求められた場合に指定する PIN。 (認証が必要な場合に -pin オプションを指定して再試行するように求められます) |
| -credserver      | ターゲットが使用するネットワーク資格情報のサーバー名。 |
| -credusername    | ターゲットが使用するネットワーク資格情報のユーザー名。 |
| -credpassword    | ターゲットが使用するネットワーク資格情報のパスワード。 |
| -connecttimeout  | デバイスに接続するときに使用されるタイムアウト (秒)。 |
| -remotedeploydir | リモート デバイス上のファイルのコピー先の相対ディレクトリ パスと名前。これは既知の自動的に決定されたリモート展開フォルダーになります。 |
| -deleteextrafile | ソース ディレクトリに一致するようにリモート ディレクトリ内の既存のファイルを消去する必要があるかどうかを指定するスイッチ。 |


次の表では、**WinAppDeployCmd.exe** のオプションについて説明します。

| **引数**           | **説明**                                                              |
|------------------------|------------------------------------------------------------------------------|
| &lt;x&gt;              | タイムアウト (秒単位)  (既定値は 10 です)                                          |
| &lt;address&gt;        | ターゲット デバイスの IP アドレスと一意の識別子。                        |
| &lt;a&gt;&lt;b&gt; ... | アプリ パッケージの依存関係のそれぞれの依存パス。                    |
| &lt;p&gt;              | 接続を確立するためのデバイス設定に示されている、英数字 PIN。 |
| &lt;path&gt;           | ファイル システム パス。                                                            |
| &lt;name&gt;           | アンインストールするアプリ パッケージの完全なパッケージ名。                          |
| &lt;server&gt;         | ファイル ネットワーク上のサーバー。                                                  |
| &lt;username&gt;       | ファイル ネットワーク上のサーバーにアクセスできる資格情報のユーザー名。      |
| &lt;password&gt;       | ファイル ネットワーク上のサーバーにアクセスできる資格情報のパスワード。 |
| &lt;remotedeploydir&gt;| 展開先の場所を基準としたデバイス上のディレクトリ。                      |

 
## <a name="winappdeploycmdexe-examples"></a>WinAppDeployCmd.exe の例

**WinAppDeployCmd.exe** の構文を使ってコマンド ラインから展開する方法の例を、次に示します。

展開できるデバイスを表示します。 コマンドは 3 秒でタイムアウトになります。

``` syntax
WinAppDeployCmd devices 3
```

PC のダウンロード ディレクトリにある MyApp.appx パッケージからアプリを、IP アドレス 192.168.0.1、PIN A1B2C3 の Windows 10 デバイスにインストールして、デバイスとの接続を確立します。

``` syntax
WinAppDeployCmd install -file "Downloads\MyApp.appx" -ip 192.168.0.1 -pin A1B2C3
```

指定したパッケージ (その完全な名前に基づく) を、IP アドレスが 192.168.0.1 の Windows 10 デバイスからアンインストールします。 list コマンドを使って、デバイスにインストールされているパッケージの完全な名前を見つけることができます。

``` syntax
WinAppDeployCmd uninstall -package Company.MyApp_1.0.0.1_x64__qwertyuiop -ip 192.168.0.1
```

指定した .appx パッケージを使って、IP アドレスが 192.168.0.1 の Windows 10 デバイスに既にインストールされているアプリを更新します。

``` syntax
WinAppDeployCmd update -file "Downloads\MyApp.appx" -ip 192.168.0.1
```

IP アドレスが 192.168.0.1 である PC または Xbox の展開パスの下にある app1_F5 ディレクトリに、AppxManifest と同じフォルダーにあるアプリのファイルを展開します。

``` syntax
WinAppDeployCmd deployfiles -file "C:\apps\App1\AppxManifest.xml" -remotedeploydir app1_F5 -ip 192.168.0.1
```

IP アドレスが 192.168.0.1 である PC または Xbox の展開パスの app1_F5 ディレクトリにあるアプリを登録します。

``` syntax
WinAppDeployCmd registerfiles -file app1_F5 -ip 192.168.0.1
```

## <a name="using-winappdeploycmd-to-set-up-run-from-pc-deployment-on-xbox-one"></a>WinAppDeployCmd を使用した PC からの Xbox One 展開の実行の設定

PC からの実行を利用すると、バイナリをコピーしなくても Xbox One に UWP アプリケーションを展開できます。バイナリは、Xbox と同じネットワーク上のネットワーク共有でホストされます。  そのためには、開発者によりロック解除された Xbox One と、ルーズ ファイル UWP アプリケーションを Xbox がアクセスできるネットワーク ドライブに用意する必要があります。

アプリを登録するには、次のコマンドを実行します。
``` syntax
WinAppDeployCmd registerfiles -ip <Xbox One IP> -remotedeploydir <location of app> -username <user for network> -password <password for user>

ex. WinAppDeployCmd register files -ip 192.168.0.1 -remotedeploydir \\driveA\myAppLocation -username admin -password A1B2C3
```