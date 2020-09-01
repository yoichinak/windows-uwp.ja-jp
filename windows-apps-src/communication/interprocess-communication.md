---
title: プロセス間通信 (IPC)
description: このトピックでは、ユニバーサル Windows プラットフォーム (UWP) アプリケーションと Win32 アプリケーションの間でプロセス間通信 (IPC) を実行するさまざまな方法について説明します。
ms.date: 03/23/2020
ms.topic: article
keywords: windows 10, uwp
ms.openlocfilehash: 0aa3c62100ecbb30e136c52cee3a6862cf15bef2
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175626"
---
# <a name="interprocess-communication-ipc"></a>プロセス間通信 (IPC)

このトピックでは、ユニバーサル Windows プラットフォーム (UWP) アプリケーションと Win32 アプリケーションの間でプロセス間通信 (IPC) を実行するさまざまな方法について説明します。

## <a name="app-services"></a>App Services

App services を使用すると、アプリケーションは、プリミティブ ([**Valueset**](/uwp/api/Windows.Foundation.Collections.ValueSet)) のプロパティバッグを受け取って返すサービスをバックグラウンドで公開できます。 リッチオブジェクトは、 [シリアル化](https://stackoverflow.com/questions/46367985/how-to-make-a-class-that-can-be-added-to-the-windows-foundation-collections-valu)されている場合は渡すことができます。

App services は、バックグラウンドタスクとして、またはフォアグラウンドアプリケーション内の[プロセスで](../launch-resume/convert-app-service-in-process.md)、[アウトプロセス](../launch-resume/how-to-create-and-consume-an-app-service.md)で実行できます。

App services は、ほぼリアルタイムの待機時間を必要としない、少量のデータを共有する場合に最適です。

## <a name="com"></a>COM (COM)

[COM](/windows/win32/com/component-object-model--com--portal) は、相互作用および通信を可能にするバイナリソフトウェアコンポーネントを作成するための分散オブジェクト指向システムです。 開発者は、COM を使用して、アプリケーションの再利用可能なソフトウェアコンポーネントとオートメーションレイヤーを作成します。 COM コンポーネントは、プロセス内またはプロセス外に配置でき、 [クライアントとサーバー](/windows/win32/com/com-clients-and-servers) のモデルを介して通信できます。 アウトプロセス COM サーバーは、 [オブジェクト間通信](/windows/win32/com/inter-object-communication)の手段として長期間使用されています。

[Runfulltrust](../packaging/app-capability-declarations.md#restricted-capabilities)機能を備えたパッケージアプリケーションでは、[パッケージマニフェスト](/uwp/schemas/appxpackage/uapmanifestschema/element-com-extension)を使用して IPC 用のアウトプロセス COM サーバーを登録できます。 これは、 [パッケージ COM](https://blogs.windows.com/windowsdeveloper/2017/04/13/com-server-ole-document-support-desktop-bridge/)と呼ばれます。

## <a name="filesystem"></a>ファイルシステム

### <a name="broadfilesystemaccess"></a>BroadFileSystemAccess

パッケージ化されたアプリケーションは、 [broadFileSystemAccess](../files/file-access-permissions.md#accessing-additional-locations) 制限機能を宣言することで、広範なファイルシステムを使用して IPC を実行できます。 この機能により、 [Windows の Storage](/uwp/api/Windows.Storage) Api と [xxx Fromapp](/previous-versions/windows/desktop/legacy/mt846585(v=vs.85)) Win32 api が広範なファイルシステムにアクセスできるようになります。

既定では、パッケージアプリケーションのファイルシステムを介した IPC は、このセクションで説明する他のメカニズムに限定されます。

### <a name="publishercachefolder"></a>PublisherCacheFolder

[PublisherCacheFolder](/uwp/api/windows.storage.applicationdata.getpublishercachefolder)を使用すると、パッケージアプリケーションでマニフェスト内のフォルダーを宣言し、同じ発行元によって他のパッケージと共有できるようになります。

共有ストレージフォルダーには、次の要件と制限があります。

* 共有ストレージフォルダー内のデータは、バックアップまたはローミングされません。
* ユーザーは、共有ストレージフォルダーの内容を消去できます。
* 共有ストレージフォルダーを使用して、さまざまなパブリッシャーのアプリケーション間でデータを共有することはできません。
* 共有ストレージフォルダーを使用して、異なるユーザー間でデータを共有することはできません。
* 共有ストレージフォルダーには、バージョン管理がありません。

複数のアプリケーションを発行し、それらの間でデータを共有するための簡単なメカニズムを探している場合、PublisherCacheFolder は単純なファイルシステムベースのオプションです。

### <a name="sharedaccessstoragemanager"></a>SharedAccessStorageManager

[Sharedaccessstoragemanager](/uwp/api/Windows.ApplicationModel.DataTransfer.SharedStorageAccessManager) は、アプリサービス、プロトコルのアクティブ化 (たとえば Launchurifor等 async) などと共に使用して、トークンを介して storagefiles を共有します。

## <a name="fulltrustprocesslauncher"></a>FullTrustProcessLauncher

[Runfulltrust](../packaging/app-capability-declarations.md#restricted-capabilities)機能を使用すると、パッケージアプリケーションは同じパッケージ内で[完全信頼プロセスを起動](/uwp/api/Windows.ApplicationModel.FullTrustProcessLauncher)できます。

パッケージの制限が負担になるシナリオや、IPC オプションがない場合は、アプリケーションで完全信頼プロセスをプロキシとして使用してシステムとのインターフェイスを設定し、App services またはその他の適切なサポートされている IPC メカニズムを使用して完全信頼プロセス自体を IPC することができます。

## <a name="launchuriforresultsasync"></a>LaunchUriForResultsAsync

[Launchuriforresults async](../launch-resume/how-to-launch-an-app-for-results.md)は、 [protocolforresults](../launch-resume/how-to-launch-an-app-for-results.md#step-2-override-applicationonactivated-in-the-app-that-youll-launch-for-results)アクティブ化コントラクトを実装する他のパッケージアプリケーションとの単純な ([valueset](/uwp/api/Windows.Foundation.Collections.ValueSet)) データ交換に使用されます。 通常はバックグラウンドで実行される App services とは異なり、ターゲットアプリケーションはフォアグラウンドで起動されます。

ファイルは、 [Storagestorageaccessmanager](/uwp/api/Windows.ApplicationModel.DataTransfer.SharedStorageAccessManager) トークンを valueset 経由でアプリケーションに渡すことによって共有できます。

## <a name="loopback"></a>ループバック

ループバックは、localhost (ループバックアドレス) でリッスンしているネットワークサーバーと通信するプロセスです。

セキュリティとネットワークの分離を維持するために、IPC のループバック接続は、パッケージアプリケーションでは既定でブロックされます。 信頼されたパッケージアプリケーション間のループバック接続は、 [機能](/previous-versions/windows/apps/hh770532(v=win.10)) と [マニフェストのプロパティ](/uwp/schemas/appxpackage/uapmanifestschema/element-uap4-loopbackaccessrules)を使用して有効にすることができます。

* ループバック接続に参加するすべてのパッケージアプリケーションは、 `privateNetworkClientServer` [パッケージマニフェスト](/uwp/schemas/appxpackage/uapmanifestschema/element-capability)で機能を宣言する必要があります。
* パッケージマニフェスト内で [LoopbackAccessRules](/uwp/schemas/appxpackage/uapmanifestschema/element-uap4-loopbackaccessrules) を宣言することで、2つのパッケージ化されたアプリケーションがループバック経由で通信できるようになります。
    * 各アプリケーションは、 [LoopbackAccessRules](/uwp/schemas/appxpackage/uapmanifestschema/element-uap4-loopbackaccessrules)内の他のアプリケーションを一覧表示する必要があります。 クライアントはサーバーに対して "out" 規則を宣言し、サーバーはサポートされているクライアントの "in" 規則を宣言します。

> [!NOTE]
> これらの規則でアプリケーションを識別するために必要なパッケージファミリ名は、開発時に Visual Studio のパッケージマニフェストエディターを使用するか、Microsoft Store で公開されたアプリケーションの [パートナーセンター](../publish/view-app-identity-details.md) を通じて、または既にインストールされているアプリケーションの [get-appxpackage](/powershell/module/appx/get-appxpackage?view=win10-ps) PowerShell コマンドを使用して見つけることができます。

パッケージ化されていないアプリケーションやサービスには、パッケージ id がないため、 [LoopbackAccessRules](/uwp/schemas/appxpackage/uapmanifestschema/element-uap4-loopbackaccessrules)で宣言することはできません。 パッケージ化されたアプリケーションを [CheckNetIsolation.exe](/previous-versions/windows/apps/hh780593(v=win.10))経由でアンパッケージされたアプリケーションやサービスと共にループバック経由で接続するように構成できますが、これが可能なのは、コンピューターへのローカルアクセスを持つサイドロードまたはデバッグのシナリオと、管理者特権がある場合のみです。

* ループバック接続に参加するすべてのパッケージアプリケーションは、 `privateNetworkClientServer` [パッケージマニフェスト](/uwp/schemas/appxpackage/uapmanifestschema/element-capability)で機能を宣言する必要があります。
* パッケージ化されたアプリケーションが、パッケージ化されていないアプリケーションまたはサービスに接続している場合は、を実行して、 `CheckNetIsolation.exe LoopbackExempt -a -n=<PACKAGEFAMILYNAME>` パッケージアプリケーションのループバックの除外対象を追加します。
* パッケージ化されていないアプリケーションまたはサービスがパッケージアプリケーションに接続している場合は、を実行し `CheckNetIsolation.exe LoopbackExempt -is -n=<PACKAGEFAMILYNAME>` て、パッケージ化されたアプリケーションが受信ループバック接続を受信できるようにします。
    * [CheckNetIsolation.exe](/previous-versions/windows/apps/hh780593(v=win.10)) は、パッケージアプリケーションが接続をリッスンしている間、継続的に実行されている必要があります。
    * この `-is` フラグは、Windows 10 バージョン1607で導入されました (10.0;ビルド 14393)。

> [!NOTE]
> CheckNetIsolation.exeのフラグに必要なパッケージファミリ名は、 `-n` 開発時に Visual Studio のパッケージマニフェストエディターを使用するか、Microsoft Store を通じて発行されたアプリケーションの[パートナーセンター](../publish/view-app-identity-details.md)を通じて、または既にインストールされているアプリケーションの[get-appxpackage](/powershell/module/appx/get-appxpackage?view=win10-ps) PowerShell コマンドを使用して見つけることができます。 [CheckNetIsolation.exe](/previous-versions/windows/apps/hh780593(v=win.10))

[CheckNetIsolation.exe](/previous-versions/windows/apps/hh780593(v=win.10)) は、ネットワークの [分離に関する問題をデバッグ](/previous-versions/windows/apps/hh780593(v=win.10)#debug-network-isolation-issues)する場合にも役立ちます。

## <a name="pipes"></a>パイプ

[パイプを使用する](/windows/win32/ipc/pipes) と、パイプサーバーと1つ以上のパイプクライアントとの間で簡単な通信を行うことができます。

[匿名パイプ](/windows/win32/ipc/anonymous-pipes) と [名前付きパイプ](/windows/win32/ipc/named-pipes) は、次の制約でサポートされています。

* 既定では、パッケージアプリケーションの名前付きパイプは、プロセスが完全に信頼されていない限り、同じパッケージ内のプロセス間でのみサポートされます。
* 名前付きパイプは、 [名前付きオブジェクトを共有](./sharing-named-objects.md)するためのガイドラインに従って、パッケージ間で共有できます。
* パッケージ化されたアプリケーションの名前付きパイプでは、パイプ名の構文を使用する必要があり `\\.\pipe\LOCAL\` ます。

## <a name="registry"></a>レジストリ

IPC の[レジストリ](/windows/win32/sysinfo/registry-functions)の使用は一般に推奨されていませんが、既存のコードではサポートされています。 パッケージ化されたアプリケーションは、アクセス許可のあるレジストリキーのみにアクセスできます。

[Msix としてパッケージ化](/windows/msix/desktop/desktop-to-uwp-root) されたデスクトップアプリケーションは、 [レジストリの仮想化](/windows/msix/desktop/desktop-to-uwp-behind-the-scenes#registry) を利用して、グローバルレジストリの書き込みが msix パッケージ内のプライベート hive に含まれるようにします。 これにより、グローバルレジストリへの影響を最小限に抑えながらソースコードの互換性を確保し、同じパッケージ内のプロセス間で IPC に使用できます。 レジストリを使用する必要がある場合は、このモデルを使用してグローバルレジストリを操作することをお勧めします。

## <a name="rpc"></a>RPC

パッケージアプリケーションに RPC エンドポイントの Acl と一致する適切な機能がある場合、 [rpc](/windows/win32/rpc/rpc-start-page)を使用して、パッケージ化されたアプリケーションを Win32 RPC エンドポイントに接続することができます。

カスタム機能を使用すると、Oem および Ihv は [任意の機能を定義](/windows-hardware/drivers/devapps/hardware-support-app--hsa--steps-for-driver-developers#reserving-a-custom-capability)し、 [それらの機能と共に RPC エンドポイントの ACL](/windows-hardware/drivers/devapps/hardware-support-app--hsa--steps-for-driver-developers#allowing-access-to-an-rpc-endpoint-to-a-uwp-app-using-the-custom-capability)を作成して、承認された [クライアントアプリケーションにこれらの機能を付与](/windows-hardware/drivers/devapps/hardware-support-app--hsa--steps-for-driver-developers#preparing-the-signed-custom-capability-descriptor-sccd-file)できます。 完全なサンプルアプリケーションについては、 [Customcapability ビリティ](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CustomCapability) のサンプルを参照してください。

RPC エンドポイントは、特定のパッケージアプリケーションを利用して、エンドポイントへのアクセスをそのアプリケーションだけに制限することもできます。カスタム機能の管理オーバーヘッドは必要ありません。 [DeriveAppContainerSidFromAppContainerName](/windows/win32/api/userenv/nf-userenv-deriveappcontainersidfromappcontainername) API を使用してパッケージファミリ名から sid を取得し、 [customcapability ビリティ](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/CustomCapability/Service/Server/RpcServer.cpp)サンプルに示されているように、sid を使用して RPC エンドポイントに ACL を指定することができます。

## <a name="shared-memory"></a>共有メモリ

[ファイルマッピング](/windows/win32/memory/sharing-files-and-memory) を使用すると、2つ以上のプロセス間でファイルまたはメモリを共有し、次の制約を設定できます。

* 既定では、パッケージアプリケーションのファイルマッピングは、プロセスが完全に信頼されていない限り、同じパッケージ内のプロセス間でのみサポートされます。
* ファイルマッピングは、 [名前付きオブジェクトを共有](./sharing-named-objects.md)するためのガイドラインに従って、パッケージ間で共有できます。

大量のデータを効率的に共有および操作するには、共有メモリを使用することをお勧めします。