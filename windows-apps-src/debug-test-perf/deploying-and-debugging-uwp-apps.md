---
ms.assetid: 9322B3A3-8F06-4329-AFCB-BE0C260C332C
description: この記事では、さまざまな展開およびデバッグのターゲットを指定する手順について説明します。
title: ユニバーサル Windows プラットフォーム (UWP) アプリの展開とデバッグ
ms.date: 04/08/2019
ms.topic: article
keywords: Windows 10, UWP, デバッグ, テスト, パフォーマンス
ms.localizationpriority: medium
ms.openlocfilehash: cdfcdfddb2b595a589c70d1facc24559c63b98da
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/20/2019
ms.locfileid: "74254793"
---
# <a name="deploying-and-debugging-uwp-apps"></a>UWP アプリの展開とデバッグ

この記事では、さまざまな展開およびデバッグのターゲットを指定する手順について説明します。

Microsoft Visual Studio allows you to deploy and debug your Universal Windows Platform (UWP) apps on a variety of Windows 10 devices. Visual Studio は、ターゲット デバイスにアプリを展開して登録するプロセスを処理します。

## <a name="picking-a-deployment-target"></a>展開ターゲットの選択

ターゲットを選択するには、 **[デバッグの開始]** ボタンの横にあるデバッグ ターゲットのドロップダウンに移動し、アプリの展開先のターゲットを選択します。 ターゲットを選択した後で、そのターゲットに展開してデバッグする場合は **[デバッグの開始 (F5)]** を選択し、単にそのターゲットに展開する場合は **Ctrl + F5** キーを押します。

![デバイスのターゲットの一覧のデバッグ](images/debug-device-target-list.png)

- **[シミュレーター]** は、現在の開発コンピューター上のシミュレートされた環境にアプリを展開します。 このオプションは、アプリの **[ターゲット プラットフォームの最小バージョン]** が開発コンピューターのオペレーティング システム以下である場合にのみ使用できます。
- **[ローカル コンピューター]** は、現在の開発コンピューターにアプリを展開します。 このオプションは、アプリの **[ターゲット プラットフォームの最小バージョン]** が開発コンピューターのオペレーティング システム以下である場合にのみ使用できます。
- **[リモート コンピューター]** では、アプリを展開するリモート ターゲットを指定できます。 リモート コンピューターへの展開について詳しくは、「[リモート デバイスの指定](#specifying-a-remote-device)」をご覧ください。
- **[デバイス]** は、USB 接続のデバイスにアプリを展開します。 デバイスが開発者によりロック解除され、画面がロック解除されている必要があります。
- **[エミュレーター]** ターゲットが起動し、名前で指定された構成のエミュレーターにアプリが展開されます。 Emulators are only available on Hyper-V enabled machines running Windows 8.1 or beyond.

## <a name="debugging-deployed-apps"></a>展開されているアプリのデバッグ

Visual Studio では、 **[デバッグ]** 、 **[プロセスにアタッチ]** の順に選ぶことによって、実行中の任意の UWP アプリ プロセスにアタッチすることもできます。 実行中のプロセスにアタッチする場合、元の Visual Studio プロジェクトは必要ありませんが、プロセスの[シンボル](#symbols)を読み込むことは、元のコードを持たないプロセスのデバッグ時に非常に役立ちます。  

さらに、 **[デバッグ]** 、 **[その他]** 、 **[インストールされているアプリケーション パッケージのデバッグ]** を選択することによって、インストール済みのアプリ パッケージにアタッチしてデバッグすることができます。

![[インストールされているアプリケーション パッケージのデバッグ] ダイアログ ボックス](images/gs-debug-uwp-apps-002.png)

**[起動はしないが、開始時にコードをデバッグする]** を選択すると、Visual Studio デバッガーは、UWP アプリが独自のタイミングで起動したときに、UWP アプリにアタッチします。 これは、カスタム パラメーターを使ったプロトコルのアクティブ化など、[さまざまな起動方法](../xbox-apps/automate-launching-uwp-apps.md)からの制御パスをデバッグするのに効果的な方法です。  

UWP アプリは、Windows 8.1 以降で開発してコンパイルすることができますが、実行するには Windows 10 が必要です。 Windows 8.1 PC で UWP アプリを開発している場合、別の Windows 10 デバイスで実行されている UWP アプリをリモートでデバッグできます。ただし、ホストとターゲットの両方のコンピューターが同じ LAN に接続されている必要があります。 これを行うには、両方のコンピューターに [Remote Tools for Visual Studio](https://visualstudio.microsoft.com/downloads/) をダウンロードしてインストールします。 インストールするバージョンはインストール済みの Visual Studio の既存のバージョンと一致している必要があり、選択するアーキテクチャ (x86、x64) もターゲット アプリのアーキテクチャと一致している必要があります。

## <a name="package-layout"></a>パッケージのレイアウト

As of Visual Studio 2015 Update 3, we have added the option for developers to specify the layout path for their UWP apps. これにより、アプリをビルドするときの、パッケージのレイアウトのディスク上でのコピー先を指定します。 既定では、このプロパティは、プロジェクトのルート ディレクトリに相対的に設定されます。 このプロパティを変更しない場合には、動作は Visual Studio の以前のバージョンと同じです。

このプロパティは、プロジェクトの **デバッグ** プロパティで変更できます。

アプリのパッケージを作成するときに、パッケージにすべてのレイアウト ファイルを含める場合には、プロジェクト プロパティ `<IncludeLayoutFilesInPackage>true</IncludeLayoutFilesInPackage>` を追加する必要があります

このプロパティを追加するには

1. プロジェクトを右クリックし、 **[プロジェクトのアンロード]** をクリックします。
2. プロジェクトを右クリックし、 **[[プロジェクト名].xxproj の編集]** を選択します (.xxproj は、プロジェクトの言語に応じて変わります)。
3. プロパティを追加し、プロジェクトを再度読み込みます。

## <a name="specifying-a-remote-device"></a>リモート デバイスの指定

### <a name="c-and-microsoft-visual-basic"></a>C# および Microsoft Visual Basic

C# または Microsoft Visual Basic のアプリのリモート コンピューターを指定するには、デバッグ ターゲットのドロップダウンで **[リモート コンピューター]** を選択します。 **[リモート接続]** ダイアログが表示され、IP アドレスを指定するか、または検出されたデバイスを選択できます。 既定では、 **[ユニバーサル]** 認証モードが選択されます。 使用する認証モードを決定するには、「[認証モード](#authentication-modes)」をご覧ください。

![リモート接続ダイアログ ボックス](images/debug-remote-connections.png)

To return to this dialog, you can open project properties and go to the **Debug** tab. From there, select **Find** next to **Remote machine:**

![デバッグ タブ](images/debug-remote-machine-config.png)

Creators Update より前のリモート PC にアプリを展開するには、Visual Studio リモート ツールをターゲット PC にダウンロードしてインストールする必要もあります。 詳しい手順については、「[リモート PC の手順](#remote-pc-instructions)」をご覧ください。  ただし、Creators Update の PC ではリモート展開もサポートされます。  

### <a name="c-and-javascript"></a>C++ および JavaScript

To specify a remote machine target for a C++ or JavaScript UWP app:

1. **[ソリューション エクスプローラー]** で、プロジェクトを右クリックし、 **[プロパティ]** をクリックします。
2. **[デバッグ]** 設定に移動し、 **[起動するデバッガー]** の下で **[リモート コンピューター]** を選択します。
3. **[コンピューター名]** を入力 (または **[検索…]** をクリックして検出) し、 **[認証の種類]** プロパティを設定します。

![デバッグ プロパティ ページ](images/debug-property-pages.png)

コンピューターを指定した後で、デバッグ ターゲットのドロップダウンで **[リモート コンピューター]** を選択し、その指定コンピューターに戻ることができます。 一度に選ぶことができるリモート コンピューターは 1 つだけです。

### <a name="remote-pc-instructions"></a>リモート PC の手順

> [!NOTE]
> これらの手順は、以前のバージョンの Windows 10 でのみ必要です。  Creators Update の PC は、Xbox と同じように扱うことができます。  つまり、PC の開発者モード メニューでデバイスの検出を有効にし、ユニバーサル認証を使って PIN でペアリングして、PC に接続します。

Creators Update より前のリモート PC に展開するには、ターゲット PC に Visual Studio リモート ツールがインストールされている必要があります。 また、リモート PC がアプリの **[ターゲット プラットフォームの最小バージョン]** プロパティ以上の Windows バージョンを実行している必要もあります。 リモート ツールをインストールしたら、ターゲット PC でリモート デバッガーを起動する必要があります。

これを行うには、 **[スタート]** メニューで **[リモート デバッガー]** を探して開き、プロンプトが表示されたらデバッガーがファイアウォール設定を構成できるようにします。 既定では、デバッガーは Windows 認証を使用して起動します。 両方の PC でサインイン ユーザーが同じでない場合、これにはユーザー資格情報が必要になります。

To change it to **no authentication**, in the **Remote Debugger**, go to **Tools** -&gt; **Options**, and then set it to **No Authentication**. リモート デバッガーを設定したら、ホスト デバイスが[開発者モード](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development)に設定されていることを確認する必要があります。 その後、開発コンピューターから展開できます。

詳しくは、[Visual Studio ダウンロード センター](https://visualstudio.microsoft.com/downloads/)のページをご覧ください。

## <a name="passing-command-line-debug-arguments"></a>デバッグのコマンド ライン引数を渡す

In Visual Studio 2019, you can pass command line debug arguments when you start debugging UWP applications. デバッグのコマンド ライン引数には、[**Application**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.application) クラスの **OnLaunched** メソッドで *args* パラメーターからアクセスすることができます デバッグのコマンド ライン引数を指定するには、プロジェクトのプロパティを開き、 **[デバッグ]** タブに移動します。

> [!NOTE]
> これは、Visual Studio 2017 (Version 15.1) で C#、VB、C++ について利用できます。 JavaScript is available in later versions. デバッグのコマンド ライン引数は、シミュレーターを除くすべての種類の展開で利用できます。

C# と VB の UWP プロジェクトでは、 **[開始オプション]** に **[コマンド ライン引数]** フィールドが表示されます。

![コマンド ライン引数](images/command-line-arguments.png)

C++ と JS の UWP プロジェクトでは、 **[デバッグ プロパティ]** のフィールドとして **[コマンド ライン引数]** が表示されます。

![C++ と JS でのコマンド ライン引数](images/command-line-arguments-cpp.png)

コマンド ライン引数を指定すると、アプリの **OnLaunched** メソッドで引数の値にアクセスすることができます。 [  **LaunchActivatedEventArgs**](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs) オブジェクト *args* は、値が **[コマンド ライン引数]** フィールドのテキストに設定された **Arguments** プロパティを持ちます。

![C++ と JS でのコマンド ライン引数](images/command-line-arguments-debugging.png)

## <a name="authentication-modes"></a>認証モード

リモート コンピューターへの展開用に 3 つの認証モードがあります。

- **[ユニバーサル (暗号化されていないプロトコル)]** : リモート デバイスに展開するときは、必ずこの認証モードを使います。 これは現在、IoT デバイス、Xbox デバイス、HoloLens デバイスと、Creators Update 以降を搭載した PC を対象としています。 ユニバーサル (暗号化されていないプロトコル) は、信頼されたネットワークで使う必要があります。 デバッグ接続は、開発マシンとリモート マシンとの間で渡されるデータを傍受して変更できる悪意のあるユーザーに対して脆弱です。
- **[Windows]** : この認証モードは、Visual Studio リモート ツールを実行中のリモート PC (デスクトップまたはノート PC) にのみ使うように想定されています。 ターゲット コンピューターのサインイン ユーザーの資格情報にアクセスできる場合は、この認証モードを使用します。 これは、リモート展開用の最も安全なチャネルです。
- **[なし]** : この認証モードは、Visual Studio リモート ツールを実行中のリモート PC (デスクトップまたはノート PC) にのみ使うように想定されています。 テスト アカウントがサインインしていて資格情報を入力できない環境にテスト コンピューターがセットアップされている場合は、この認証モードを使用します。 リモート デバッガーの設定が、認証を受け入れないように設定されていることを確認してください。

## <a name="advanced-remote-deployment-options"></a>リモート展開の詳細オプション

As of the release of Visual Studio 2015 Update 3, and the Windows 10 Anniversary Update, there are new advanced remote deployment options for certain Windows 10 devices. リモート展開の詳細オプションは、プロジェクト プロパティの **[デバッグ]** メニューにあります。

新しいプロパティには、次のものが含まれています。

- 展開の種類
- パッケージの登録パス
- レイアウトの一部でなくなっているものも含め、デバイスのすべてのファイルを保持する

### <a name="requirements"></a>要件

リモート展開の詳細オプションを利用するには、次の要件を満たす必要があります。

- Have Visual Studio 2015 Update 3 or some later Visual Studio release installed with Windows 10 Tools 1.4.1 or later(which includes the Windows 10 Anniversary Update SDK) We recommend that you use the latest version of Visual Studio with updates to ensure you get all the newest development and security features.
- Windows 10 Anniversary Update の Xbox リモート デバイスまたは Windows 10 Creators Update の PC をターゲットにする
- ユニバーサル認証モードを使う

### <a name="properties-pages"></a>プロパティ ページ

C# または Visual Basic の UWP アプリでは、[プロパティ] ページは、次のようになります。

![CS または VB プロパティ](images/advanced-remote-deploy-cs.png)

C++ UWP アプリでは、[プロパティ] ページは、次のようになります。

![Cpp プロパティ](images/advanced-remote-deploy-cpp.png)

### <a name="copy-files-to-device"></a>デバイスにファイルをコピーする

**[デバイスにファイルをコピーする]** ファイルをリモート デバイスにネットワークを介して物理的に転送します。 **レイアウト フォルダー パス**にビルドされたパッケージ レイアウトをコピーして登録します。 Visual Studio は、Visual Studio プロジェクト内のファイルとの同期してデバイスにコピーされるファイルを保持します。ただし、 **[レイアウトの一部でなくなっているものも含め、デバイスのすべてのファイルを保持する]** のオプションがあります。 このオプションを選択すると、リモート デバイスに以前にコピーされたが、プロジェクトの一部ではなくなったファイルも、リモート デバイス上で維持されます。

**[デバイスにファイルをコピーする]** で指定する**パッケージ登録パス**は、コピー先リモート デバイスの物理的な場所です。 このパスは、任意の相対パスとして指定することができます。 ファイルが展開される場所は、開発ファイルのルートからの相対的な場所となり、ターゲット デバイスによって異なります。 複数の開発者が同じデバイスを共有し、いくつかの異なるビルドのパッケージに作業を行っている場合に、このパスを指定すると便利です。

> [!NOTE]
> **[デバイスにファイルをコピーする]** は現在、Windows 10 Anniversary Update を搭載した Xbox と、Windows 10 Creators Update を搭載した PC でサポートされています。

On the remote device, the layout gets copied to the following default location: `\\MY-DEVKIT\DevelopmentFiles\PACKAGE-REGISTRATION-PATH`

### <a name="register-layout-from-network"></a>ネットワークからレイアウトを登録する

ネットワークからレイアウトを登録する場合には、パッケージ レイアウトをネットワーク共有にビルドでき、ネットワークから直接レイアウトをリモート デバイスに登録できます。 これには、リモート デバイスからアクセス可能なレイアウト フォルダー パス (ネットワーク共有) を指定する必要があります。 **[レイアウト フォルダー パス]** プロパティは、Visual Studio を実行している PC からの相対パスで、 **[パッケージ登録パス]** プロパティは、同じパスですが、リモート デバイスからの相対パスです。

ネットワークからレイアウトを正常に登録するには、まず **[レイアウト フォルダー パス]** を共有ネットワーク フォルダーにする必要があります。 これを行うには、エクスプローラーでフォルダーを右クリックして、 **[共有] > [特定のユーザー]** を選択し、フォルダーを共有するユーザーを選択します。 ネットワークからレイアウトを登録する場合、共有へのアクセスを持つユーザーとして登録することを確認するため、資格情報を求められます。

これについては、次の例をご覧ください。

- 例 1 (ネットワーク共有としてアクセス可能な、ローカル レイアウト フォルダー):
  - **Layout folder path** = `D:\Layouts\App1`
  - **Package registration path** = `\\NETWORK-SHARE\Layouts\App1`

- 例 2 (ネットワーク レイアウト フォルダー):
  - **Layout folder path** = `\\NETWORK-SHARE\Layouts\App1`
  - **Package registration path** = `\\NETWORK-SHARE\Layouts\App1`

最初にネットワークからレイアウトを登録するときに、ターゲット デバイスに資格情報がキャッシュされるため、繰り返しサインインする必要はありません。 キャッシュされた資格情報を削除するには、Windows 10 SDK の [WinAppDeployCmd.exe ツール](https://docs.microsoft.com/windows/uwp/packaging/install-universal-windows-apps-with-the-winappdeploycmd-tool) と **deletecreds**コマンド を使用できます。

ネットワークからレイアウトを登録する場合には、ファイルは物理的にリモート デバイスにコピーされないため、 **[デバイスのすべてのファイルを保持する]** を選択することはできません。

> [!NOTE]
> **[ネットワークからレイアウトを登録する]** は現在、Windows 10 Anniversary Update を搭載した Xbox と、Windows 10 Creators Update を搭載した PC でサポートされています。

On the remote device, the layout gets registered to the following default location depending on the device family: `Xbox: \\MY-DEVKIT\DevelopmentFiles\XrfsFiles` - this is a symlink to the **package registration path** PC does not use a symlink and instead directly registers the **package registration path**

## <a name="debugging-options"></a>デバッグのオプション

On Windows 10, the startup performance of UWP apps is improved by proactively launching and then suspending apps in a technique called [prelaunch](https://docs.microsoft.com/windows/uwp/launch-resume/handle-app-prelaunch). 多くのアプリはこのモードで動作するために特別に何もする必要はありませんが、一部のアプリでは動作を調整する必要があります。 これらのコード パスの問題をデバッグするために、事前起動モードで Visual Studio からアプリのデバッグを開始できます。

Debugging is supported both from a Visual Studio project (**Debug** -&gt; **Other Debug Targets** -&gt; **Debug Universal Windows App Prelaunch**), and for apps already installed on the machine (**Debug** -&gt; **Other Debug Targets** -&gt; **Debug Installed App Package** by selecting the **Activate app with Prelaunch** check box). 詳しくは、「[事前起動 UWP をデバッグする](https://blogs.msdn.com/b/visualstudioalm/archive/2015/11/30/debug-uwp-prelaunch-with-vs2015.aspx)」をご覧ください。

スタートアップ プロジェクトの **[デバッグ]** プロパティ ページで、次の展開オプションを設定できます。

- **Allow local network loopback**

  セキュリティ上の理由で、標準的な方法でインストールされた UWP アプリでは、それがインストールされているデバイスに対してネットワーク呼び出しを実行することは許可されません。 既定では、Visual Studio の展開では、展開されたアプリについてこの規則が除外されます。 この除外により、単一コンピューター上で通信手順をテストできます。 Before submitting your app to the Microsoft Store, you should test your app without the exemption.

  ネットワーク ループバックに関する除外をアプリから除去するには:

  - On the C# and Visual Basic **Debug** property page, clear the **Allow local network loopback** check box.
  - JavaScript および C++ の **[デバッグ]** プロパティ ページで、 **[ローカル ネットワーク ループバックの許可]** の値を **[いいえ]** に設定します

- **Do not launch, but debug my code when it starts / Launch Application**

  アプリ起動時にデバッグ セッションが自動的に開始されるように展開を構成するには:

  - On the C# and Visual Basic **Debug** property page, select the **Do not launch, but debug my code when it starts** check box.
  - JavaScript および C++ の **[デバッグ]** プロパティ ページで、 **[アプリの起動]** 値を **[はい]** に設定します。

## <a name="symbols"></a>シンボル

シンボル ファイルには、変数、関数名、エントリ ポイントのアドレスなど、コードをデバッグするときに非常に便利な値が格納されており、例外とコールスタックの実行順序を把握することができます。 ほとんどの種類の Windows のシンボルは、[Microsoft シンボル サーバー](https://msdl.microsoft.com/download/symbols)から利用することも、高速にオフラインで参照できるように [Windows シンボル パッケージのダウンロード サイト](https://docs.microsoft.com/windows-hardware/drivers/debugger/debugger-download-symbols)からダウンロードすることもできます。

Visual Studio のシンボル オプションを設定するには、 **[ツール] の [オプション]** を選択し、ダイアログ ウィンドウで **[デバッグ]、[シンボル]** の順に移動します。

![オプション ダイアログ ボックス](images/gs-debug-uwp-apps-004.png)

[WinDbg](#windbg) を使ってデバッグ セッションでシンボルを読み込むには、**sympath** 変数をシンボル パッケージの場所に設定します。 たとえば、次のコマンドを実行すると、Microsoft シンボル サーバーからシンボルが読み込まれ、C:\Symbols ディレクトリにキャッシュされます。

```cmd
.sympath SRV*C:\Symbols*http://msdl.microsoft.com/download/symbols
.reload
```

区切り文字 `‘;’` を使用して複数のパスを追加したり、`.sympath+` コマンドを使用することもできます。 WinDbg を使用する高度なシンボル操作については、「[パブリック シンボルとプライベート シンボル](https://docs.microsoft.com/windows-hardware/drivers/debugger/public-and-private-symbols)」をご覧ください。

## <a name="windbg"></a>WinDbg

WinDbg は、[Windows SDK](https://developer.microsoft.com/) に含まれる、Debugging Tools for Windows の一部として出荷される強力なデバッガーです。 Windows SDK のインストールでは、スタンドアロン製品として Debugging Tools for Windows をインストールすることができます。 ネイティブ コードのデバッグには非常に便利ですが、マネージ コードや HTML5 で記述されたアプリについては WinDbg の使用をお勧めできません。

UWP アプリで WinDbg を使用するには、まず PLMDebug を使用して、アプリ パッケージのプロセス ライフタイム管理 (PLM) を無効にする必要があります。これについては [プロセス ライフタイム管理 (PLM) のテスト ツールとデバッグ ツール](testing-debugging-plm.md) で説明されています。

```cmd
plmdebug /enableDebug [PackageFullName] ""C:\Program Files\Debugging Tools for Windows (x64)\WinDbg.exe\" -server npipe:pipe=test"
```

Visual Studio とは対照的に、WinDbg のコア機能の多くは、コマンド ウィンドウにコマンドを入力する必要があります。 入力したコマンドによって、実行状態の表示、ユーザー モードのクラッシュ ダンプの調査、さまざまなモードでのデバッグを行うことができます。

WinDbg で最もよく使用されるコマンドの 1 つが `!analyze -v` であり、次のように、現在の例外に関する詳細な情報を取得するために使用されます。

- FAULTING_IP: 障害発生時の命令ポインター
- EXCEPTION_RECORD: 現在の例外のアドレス、コード、フラグ
- STACK_TEXT: 例外の前のスタック トレース

WinDbg のすべてのコマンドの一覧については、[デバッガー コマンドに関するページ](https://docs.microsoft.com/windows-hardware/drivers/debugger/debugger-commands)をご覧ください。

## <a name="related-topics"></a>関連トピック

- [プロセス ライフタイム管理 (PLM) のテスト ツールとデバッグ ツール](testing-debugging-plm.md)
- [デバッグ、テスト、パフォーマンス](index.md)
