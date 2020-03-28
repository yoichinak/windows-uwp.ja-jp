---
title: プロセス間通信 (IPC)
description: このトピックでは、ユニバーサル Windows プラットフォーム (UWP) アプリケーションと Win32 アプリケーションの間でプロセス間通信 (IPC) を実行するさまざまな方法について説明します。
ms.date: 03/23/2020
ms.topic: article
keywords: windows 10, uwp
ms.openlocfilehash: 7a41c72ee57f7c87278576cfb135a96651456214
ms.sourcegitcommit: 84c46591a32bf0613efc72d7e7c40cc7b4c51062
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/28/2020
ms.locfileid: "80377981"
---
# <a name="interprocess-communication-ipc"></a>プロセス間通信 (IPC)

このトピックでは、ユニバーサル Windows プラットフォーム (UWP) アプリケーションと Win32 アプリケーションの間でプロセス間通信 (IPC) を実行するさまざまな方法について説明します。

## <a name="app-services"></a>アプリ サービス

App services を使用すると、バックグラウンドでプリミティブ ([**Valueset**](/uwp/api/Windows.Foundation.Collections.ValueSet)) のプロパティバッグを受け入れて返すサービスをアプリで公開できます。 リッチオブジェクトは、[シリアル化](https://stackoverflow.com/questions/46367985/how-to-make-a-class-that-can-be-added-to-the-windows-foundation-collections-valu)されている場合は渡すことができます。

App services は、バックグラウンドタスクとして、またはフォアグラウンドアプリ内の[プロセスで](/windows/uwp/launch-resume/convert-app-service-in-process)、[アウトプロセス](/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service)で実行できます。

App services は、ほぼリアルタイムの待機時間を必要としない、少量のデータを共有する場合に最適です。

## <a name="com"></a>COM

[COM](/windows/win32/com/component-object-model--com--portal)は、相互作用および通信を可能にするバイナリソフトウェアコンポーネントを作成するための分散オブジェクト指向システムです。 開発者は、COM を使用して、アプリケーションの再利用可能なソフトウェアコンポーネントとオートメーションレイヤーを作成します。 COM コンポーネントは、プロセス内またはプロセス外に配置でき、[クライアントとサーバー](/windows/win32/com/com-clients-and-servers)のモデルを介して通信できます。 アウトプロセス COM サーバーは、[オブジェクト間通信](/windows/win32/com/inter-object-communication)の手段として長期間使用されています。

[Runfulltrust](/windows/uwp/packaging/app-capability-declarations#restricted-capabilities)機能を備えたパッケージアプリケーションでは、[パッケージマニフェスト](/uwp/schemas/appxpackage/uapmanifestschema/element-com-extension)を使用して IPC 用のアウトプロセス COM サーバーを登録できます。 これは、[パッケージ COM](https://blogs.windows.com/windowsdeveloper/2017/04/13/com-server-ole-document-support-desktop-bridge/)と呼ばれます。

## <a name="filesystem"></a>ファイル システム

### <a name="broadfilesystemaccess"></a>BroadFileSystemAccess

パッケージ化されたアプリケーションは、 [broadFileSystemAccess](/windows/uwp/files/file-access-permissions#accessing-additional-locations)制限機能を宣言することで、広範なファイルシステムを使用して IPC を実行できます。

既定では、パッケージアプリケーションのファイルシステムを介した IPC は、このセクションで説明する他のメカニズムに限定されます。

### <a name="publishercachefolder"></a>PublisherCacheFolder

[PublisherCacheFolder](/uwp/api/windows.storage.applicationdata.getpublishercachefolder)を使用すると、パッケージアプリケーションでマニフェスト内のフォルダーを宣言し、同じ発行元によって他のパッケージと共有できるようになります。

共有ストレージフォルダーには、次の要件と制限があります。

* 共有ストレージフォルダー内のデータは、バックアップまたはローミングされません。 また、ユーザーは共有ストレージフォルダーの内容を消去できます。
* この機能を使用して、さまざまな発行元のアプリ間でデータを共有することはできません。
* この機能を使用して、異なるユーザー間でデータを共有することはできません。
* 共有ストレージフォルダーには、バージョン管理がありません。

複数のアプリを発行し、それらの間でデータを共有するための簡単なメカニズムを探している場合は、PublisherCacheFolder は単純なファイルシステムベースのオプションです。

### <a name="sharedaccessstoragemanager"></a>SharedAccessStorageManager

[Sharedaccessstoragemanager](/uwp/api/Windows.ApplicationModel.DataTransfer.SharedStorageAccessManager)は、アプリサービス、プロトコルのアクティブ化 (たとえば Launchurifor等 async) などと共に使用して、トークンを介して storagefiles を共有します。

## <a name="fulltrustprocesslauncher"></a>FullTrustProcessLauncher

[Runfulltrust](/windows/uwp/packaging/app-capability-declarations#restricted-capabilities)機能を使用すると、パッケージアプリケーションは同じパッケージ内で[完全信頼プロセスを起動](/uwp/api/Windows.ApplicationModel.FullTrustProcessLauncher)できます。

パッケージの制限が負担になるシナリオや、IPC オプションがない場合は、アプリで完全信頼プロセスをプロキシとして使用してシステムとのインターフェイスを設定し、App services またはその他の適切なサポートされている IPC メカニズムを使用して完全信頼プロセス自体を IPC することができます。

## <a name="launchuriforresultsasync"></a>LaunchUriForResultsAsync

[Launchuriforresults async](/windows/uwp/launch-resume/how-to-launch-an-app-for-results)は、 [protocolforresults](/windows/uwp/launch-resume/how-to-launch-an-app-for-results#step-2-override-applicationonactivated-in-the-app-that-youll-launch-for-results)アクティブ化コントラクトを実装する他のパッケージアプリケーションとの単純な ([valueset](/uwp/api/Windows.Foundation.Collections.ValueSet)) データ交換に使用されます。 通常はバックグラウンドで実行される App services とは異なり、ターゲットアプリはフォアグラウンドで起動されます。

ファイルは、 [Storagestorageaccessmanager](/uwp/api/Windows.ApplicationModel.DataTransfer.SharedStorageAccessManager)トークンを valueset 経由でアプリに渡すことによって共有できます。

## <a name="loopback"></a>ループバック

ループバックは、localhost (ループバックアドレス) でリッスンしているネットワークサーバーと通信するプロセスです。

セキュリティとネットワークの分離を維持するために、パッケージアプリで IPC のループバック接続が既定でブロックされます。 信頼されたパッケージアプリ間でのループバック接続は、[機能](/previous-versions/windows/apps/hh770532(v=win.10))と[マニフェストのプロパティ](/uwp/schemas/appxpackage/uapmanifestschema/element-uap4-loopbackaccessrules)を使用して有効にすることができます。

* ループバック接続に参加しているすべてのパッケージアプリは、[パッケージマニフェスト](/uwp/schemas/appxpackage/uapmanifestschema/element-capability)で `privateNetworkClientServer` 機能を宣言する必要があります。
* パッケージマニフェスト内で[LoopbackAccessRules](/uwp/schemas/appxpackage/uapmanifestschema/element-uap4-loopbackaccessrules)を宣言することで、2つのパッケージアプリがループバック経由で通信できるようになります。 各アプリは、LoopbackAccessRules 内の他のアプリを一覧表示する必要があります。 クライアントはサーバーに対して "out" 規則を宣言し、サーバーはサポートされているクライアントの "in" 規則を宣言します。

> [!NOTE]
> これらの規則でアプリを識別するために必要なパッケージファミリ名は、開発時に Visual Studio のパッケージマニフェストエディターを使用するか、Microsoft Store で公開されたアプリの[パートナーセンター](/windows/uwp/publish/view-app-identity-details)を通じて、または既にインストールされているアプリの[get-appxpackage](/powershell/module/appx/get-appxpackage?view=win10-ps) PowerShell コマンドを使用して見つけることができます。

パッケージ化されていないアプリとサービスには、パッケージ id がないため、 [LoopbackAccessRules](/uwp/schemas/appxpackage/uapmanifestschema/element-uap4-loopbackaccessrules)で宣言することはできません。 [CheckNetIsolation](/previous-versions/windows/apps/hh780593(v=win.10))を使用して、パッケージ化されていないアプリやサービスとループバック経由で接続するようにパッケージアプリを構成できますが、これは、コンピューターへのローカルアクセスがあるサイドロードまたはデバッグシナリオでのみ可能で、管理者特権があります。
* ループバック接続に参加しているすべてのパッケージアプリは、[パッケージマニフェスト](/uwp/schemas/appxpackage/uapmanifestschema/element-capability)で `privateNetworkClientServer` 機能を宣言する必要があります。
* パッケージアプリが、パッケージ化されていないアプリまたはサービスに接続している場合は、`CheckNetIsolation.exe LoopbackExempt -a -n=<PACKAGEFAMILYNAME>` を実行して、パッケージアプリのループバックの除外を追加します。
* パッケージ化されていないアプリまたはサービスがパッケージアプリに接続している場合は、`CheckNetIsolation.exe LoopbackExempt -is -n=<PACKAGEFAMILYNAME>` を実行して、パッケージアプリが受信ループバック接続を受信できるようにします。 パッケージアプリが接続をリッスンしている間は、 [CheckNetIsolation](/previous-versions/windows/apps/hh780593(v=win.10))が連続して実行されている必要があります。 `-is` フラグは、Windows 10 バージョン1607で導入されました (10.0;ビルド 14393)。

> [!NOTE]
> [CheckNetIsolation](/previous-versions/windows/apps/hh780593(v=win.10))の `-n` フラグに必要なパッケージファミリ名は、開発時に Visual Studio のパッケージマニフェストエディターを使用するか、Microsoft Store を通じて発行されたアプリの[パートナーセンター](/windows/uwp/publish/view-app-identity-details)を通じて、または既にインストールされているアプリの[get-appxpackage](/powershell/module/appx/get-appxpackage?view=win10-ps) PowerShell コマンドを使用して見つけることができます。

[CheckNetIsolation](/previous-versions/windows/apps/hh780593(v=win.10))は、ネットワークの分離の[問題をデバッグ](/previous-versions/windows/apps/hh780593(v=win.10)#debug-network-isolation-issues)する場合にも役立ちます。

## <a name="pipes"></a>パイプ

[パイプを使用する](/windows/win32/ipc/pipes)と、パイプサーバーと1つ以上のパイプクライアントとの間で簡単な通信を行うことができます。

[匿名パイプ](/windows/win32/ipc/anonymous-pipes)と[名前付きパイプ](/windows/win32/ipc/named-pipes)は、次の制約でサポートされています。

* パッケージアプリケーションの名前付きパイプは、プロセスが完全に信頼されていない限り、同じパッケージ内のプロセス間でのみサポートされます。
* パッケージ化されたアプリケーションの名前付きパイプでは、パイプ名に `\\.\pipe\LOCAL\` 構文を使用する必要があります。

## <a name="registry"></a>［レジストリ］

IPC の[レジストリ](/windows/win32/sysinfo/registry-functions)の使用は一般に推奨されていませんが、既存のコードではサポートされています。 パッケージ化されたアプリケーションは、アクセス許可のあるレジストリキーのみにアクセスできます。

[Msix としてパッケージ化](/windows/msix/desktop/desktop-to-uwp-root)されたデスクトップアプリでは、[レジストリの仮想化](/windows/msix/desktop/desktop-to-uwp-behind-the-scenes#registry)を利用して、グローバルレジストリの書き込みが msix パッケージ内のプライベート hive に含まれるようにします。 これにより、グローバルレジストリへの影響を最小限に抑えながらソースコードの互換性を確保し、同じパッケージ内のプロセス間で IPC に使用できます。 レジストリを使用する必要がある場合は、このモデルを使用してグローバルレジストリを操作することをお勧めします。

## <a name="rpc"></a>RPC

パッケージアプリケーションに RPC エンドポイントの Acl と一致する適切な機能がある場合、 [rpc](/windows/win32/rpc/rpc-start-page)を使用して、パッケージ化されたアプリケーションを Win32 RPC エンドポイントに接続することができます。

カスタム機能を使用すると、Oem および Ihv は[任意の機能を定義](/windows-hardware/drivers/devapps/hardware-support-app--hsa--steps-for-driver-developers#reserving-a-custom-capability)し、[それらの機能と共に RPC エンドポイントに ACL](/windows-hardware/drivers/devapps/hardware-support-app--hsa--steps-for-driver-developers#allowing-access-to-an-rpc-endpoint-to-a-uwp-app-using-the-custom-capability)を設定して、承認された[クライアントアプリにこれらの機能を付与](/windows-hardware/drivers/devapps/hardware-support-app--hsa--steps-for-driver-developers#preparing-the-signed-custom-capability-descriptor-sccd-file)できます。 完全なサンプルアプリケーションについては、 [Customcapability ビリティ](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CustomCapability)のサンプルを参照してください。

RPC エンドポイントは、特定のパッケージアプリケーションを利用してエンドポイントへのアクセスを制限することもできます。カスタム機能の管理オーバーヘッドは必要ありません。 [DeriveAppContainerSidFromAppContainerName](/windows/win32/api/userenv/nf-userenv-deriveappcontainersidfromappcontainername) API を使用してパッケージファミリ名から sid を取得し、 [customcapability ビリティ](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/CustomCapability/Service/Server/RpcServer.cpp)サンプルに示されているように、sid を使用して RPC エンドポイントに ACL を指定することができます。

## <a name="shared-memory"></a>共有メモリ

[ファイルマッピング](/windows/win32/memory/sharing-files-and-memory)は、同じパッケージ内の2つ以上のプロセス間でファイルまたはメモリを共有するために使用できます。
