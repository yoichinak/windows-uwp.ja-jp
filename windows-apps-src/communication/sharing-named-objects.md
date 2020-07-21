---
title: 名前付きオブジェクトの共有
description: このトピックでは、ユニバーサル Windows プラットフォーム (UWP) アプリケーションと Win32 アプリケーションの間で名前付きオブジェクトを共有する方法について説明します。
ms.date: 04/06/2020
ms.topic: article
keywords: windows 10, uwp
ms.openlocfilehash: 95bbecd85b1dfa6f6e12766c082f3338de549677
ms.sourcegitcommit: 2d375e1c34473158134475af401532cc55fc50f4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/08/2020
ms.locfileid: "80888574"
---
# <a name="sharing-named-objects"></a>名前付きオブジェクトの共有

このトピックでは、ユニバーサル Windows プラットフォーム (UWP) アプリケーションと Win32 アプリケーションの間で名前付きオブジェクトを共有する方法について説明します。

## <a name="named-objects-in-packaged-applications"></a>パッケージ化されたアプリケーションの名前付きオブジェクト

[名前付きオブジェクト](/windows/win32/sync/object-names)を使用すると、プロセスでオブジェクトハンドルを簡単に共有できます。 プロセスによって名前付きオブジェクトが作成された後、その名前を使用して適切な関数を呼び出し、そのオブジェクトへのハンドルを開くことができます。 名前付きオブジェクトは、[スレッドの同期](/windows/win32/sync/interprocess-synchronization)と[プロセス間の通信](/windows/uwp/communication/interprocess-communication)によく使用されます。

既定では、パッケージ化されたアプリケーションは、作成した名前付きオブジェクトにのみアクセスできます。 パッケージ化されたアプリケーションで名前付きオブジェクトを共有するには、オブジェクトの作成時にアクセス許可を設定する必要があります。また、オブジェクトを開くときには、名前を修飾する必要があります。

## <a name="creating-named-objects"></a>名前付きオブジェクトの作成

名前付きオブジェクトは、対応する `Create` API を使用して作成されます。

* [CreateEvent](/windows/win32/api/synchapi/nf-synchapi-createeventexw)
* [Createfilemapping に](/windows/win32/api/memoryapi/nf-memoryapi-createfilemappingw)
* [Createmutex に](/windows/win32/api/synchapi/nf-synchapi-createmutexexw)
* [CreateSemaphore](/windows/win32/api/synchapi/nf-synchapi-createsemaphoreexw)
* [Createwaitabletimer の](/windows/win32/api/synchapi/nf-synchapi-createwaitabletimerexw)

これらのすべての Api は `LPSECURITY_ATTRIBUTES` パラメーターを共有します。これにより、呼び出し元は、[アクセス制御リスト (acl)](/previous-versions/windows/desktop/legacy/aa379560(v=vs.85))を指定して、オブジェクトにアクセスできるプロセスを制御できます。 パッケージ化されたアプリケーションで名前付きオブジェクトを共有するには、名前付きオブジェクトを作成するときに、Acl 内でアクセス許可を付与する必要があります。

セキュリティ識別子 (Sid) は、Acl 内の id を表します。 パッケージ化されたすべてのアプリケーションには、パッケージファミリ名に基づいた独自の SID があります。 パッケージ化されたアプリケーションの SID は、パッケージファミリ名を[DeriveAppContainerSidFromAppContainerName](/windows/win32/api/userenv/nf-userenv-deriveappcontainersidfromappcontainername)に渡すことによって生成できます。

> [!NOTE]
> パッケージファミリ名は、開発時に Visual Studio のパッケージマニフェストエディターを使用するか、Microsoft Store で公開されたアプリケーションの[パートナーセンター](/windows/uwp/publish/view-app-identity-details)を通じて、または既にインストールされているアプリケーションの[get-appxpackage](/powershell/module/appx/get-appxpackage?view=win10-ps) PowerShell コマンドを使用して見つけることができます。

[このサンプル](/windows/win32/api/securityappcontainer/nf-securityappcontainer-getappcontainernamedobjectpath#examples)では、名前付きオブジェクトの ACL に必要な基本的なパターンを示します。 パッケージ化されたアプリケーションで名前付きオブジェクトを共有するには、アプリケーションごとに[EXPLICIT_ACCESS](/windows/win32/api/accctrl/ns-accctrl-explicit_access_w)構造を構築します。

* `grfAccessMode = GRANT_ACCESS`
* オブジェクトと使用目的に基づいて適切なアクセス許可を `grfAccessPermissions =` する
    * [汎用アクセス権](/windows/win32/secauthz/generic-access-rights)
    * [同期オブジェクトのセキュリティとアクセス権](/windows/win32/sync/synchronization-object-security-and-access-rights)
    * [ファイルマッピングのセキュリティとアクセス権](/windows/win32/memory/file-mapping-security-and-access-rights)
* `grfInheritance = NO_INHERITANCE`
* `Trustee.TrusteeForm = TRUSTEE_IS_SID`
* `Trustee.TrusteeType = TRUSTEE_IS_USER`
* [DeriveAppContainerSidFromAppContainerName](/windows/win32/api/userenv/nf-userenv-deriveappcontainersidfromappcontainername)から取得した SID を `Trustee.ptstrName =` します。

パッケージ化されたアプリケーションの `EXPLICIT_ACCESS` ルールを使用して `Create` 呼び出しに `LPSECURITY_ATTRIBUTES` パラメーターを設定することにより、それらのアプリケーションへのアクセス権を付与して、名前付きオブジェクトを開くことができます。

> [!NOTE]
> Win32 アプリケーションは、パッケージアプリケーションで作成されたすべての名前付きオブジェクトにアクセスできます。ただし、そのオブジェクトを[開く](#opening-named-objects)ときにオブジェクト名を修飾することはできます。 アクセス権を付与する必要はありません。

## <a name="opening-named-objects"></a>名前付きオブジェクトを開く

名前付きオブジェクトは、対応する `Open` API に名前を渡すことによって開かれます。

* [OpenEvent](/windows/win32/api/synchapi/nf-synchapi-openeventw)
* [Openfilemapping に](/windows/win32/api/memoryapi/nf-memoryapi-openfilemappingw)
* [OpenMutex](/windows/win32/api/synchapi/nf-synchapi-openmutexw)
* [OpenSemaphore](/windows/win32/api/synchapi/nf-synchapi-opensemaphorew)
* [OpenWaitableTimer](/windows/win32/api/synchapi/nf-synchapi-openwaitabletimerw)

パッケージ化されたアプリケーションによって作成された名前付きオブジェクトは、アプリケーションの名前空間内に作成されます。それ以外の場合は、名前付きオブジェクトパスと呼ばれます。 パッケージ化されたアプリケーションによって作成された名前付きオブジェクトを開く場合、オブジェクト名の先頭には、作成するアプリケーションの名前付きオブジェクトパスを付ける必要があります。

[GetAppContainerNamedObjectPath](/windows/win32/api/securityappcontainer/nf-securityappcontainer-getappcontainernamedobjectpath)は、その SID に基づいて、パッケージ化されたアプリケーションの名前付きオブジェクトパスを返します。 パッケージ化されたアプリケーションの SID は、パッケージファミリ名を[DeriveAppContainerSidFromAppContainerName](/windows/win32/api/userenv/nf-userenv-deriveappcontainersidfromappcontainername)に渡すことによって生成できます。

> [!NOTE]
> パッケージファミリ名は、開発時に Visual Studio のパッケージマニフェストエディターを使用するか、Microsoft Store で公開されたアプリケーションの[パートナーセンター](/windows/uwp/publish/view-app-identity-details)を通じて、または既にインストールされているアプリケーションの[get-appxpackage](/powershell/module/appx/get-appxpackage?view=win10-ps) PowerShell コマンドを使用して見つけることができます。

パッケージ化されたアプリケーションによって作成された名前付きオブジェクトを開く場合は、`<PATH>\<NAME>`の形式を使用します。

* `<PATH>` を作成するアプリケーションの名前付きオブジェクトのパスに置き換えます。
* `<NAME>` をオブジェクト名に置き換えます。

> [!NOTE]
> オブジェクト名のプレフィックスを `<PATH>` にする必要があるのは、パッケージ化されたアプリケーションがオブジェクトを作成した場合のみです。 Win32 アプリケーションによって作成された名前付きオブジェクトは修飾する必要はありませんが、オブジェクトの[作成](#creating-named-objects)時にアクセス権を付与する必要があります。

## <a name="remarks"></a>コメント

パッケージ化されたアプリケーションの名前付きオブジェクトは、セキュリティを維持し、中断や終了などのアプリケーションライフサイクルイベントのサポートを確保するために、既定で分離されています。 アプリケーション間で名前付きオブジェクトを共有すると、バインドとバージョン管理の制約が厳しくなり、各アプリケーションが他のアプリケーションのライフサイクルに対して回復力を持つ必要があります。 これらの理由から、同じ発行元のアプリケーション間でのみ名前付きオブジェクトを共有することをお勧めします。
