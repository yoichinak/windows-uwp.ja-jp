---
title: リモート デバイスの検出
description: "\"Rome\" プロジェクトを使ってアプリからリモート デバイスを検出する方法について説明します。"
ms.assetid: 5b4231c0-5060-49e2-a577-b747e20cf633
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10、uwp、接続されているデバイス、リモートシステム、ローマ、プロジェクトローマ
ms.localizationpriority: medium
ms.openlocfilehash: a479cb20943b9c4b2df53b22751c9de2f5a8402c
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363735"
---
# <a name="discover-remote-devices"></a>リモート デバイスの検出
アプリは、ワイヤレス ネットワーク、Bluetooth、およびクラウド接続を使って、検出側デバイスと同じ Microsoft アカウントでサインインしている Windows デバイスを検出できます。 リモート デバイスを検出するために特別なソフトウェアをインストールする必要はありません。

> [!NOTE]
> このガイドでは、「[リモート アプリの起動](launch-a-remote-app.md)」の手順に従うことで、リモート システム機能へのアクセスが既に許可されていることを前提としています。

## <a name="filter-the-set-of-discoverable-devices"></a>検出可能な一連のデバイスのフィルター処理
フィルターに [**RemoteSystemWatcher**](/uwp/api/Windows.System.RemoteSystems.RemoteSystemWatcher) を使うことで、検出可能な一連のデバイスを絞り込むことができます。 フィルターは、検出の種類 (隣接ネットワーク、ローカル ネットワーク、またはクラウド接続)、デバイスの種類 (デスクトップ、モバイル デバイス、Xbox、Hub、ホログラフィック)、利用可能ステータス (デバイスがリモート システム機能を利用可能かどうかのステータス) を検出できます。

**RemoteSystemWatcher** オブジェクトを初期化する前か、その初期化中に、フィルター オブジェクトを作成する必要があります。コンストラクターにパラメーターとして渡されるためです。 次のコードでは、利用可能な各種類のフィルターを作成し、一覧に追加します。

> [!NOTE]
> この例のコードでは、ファイルに `using Windows.System.RemoteSystems` ステートメントがあることを利用としています。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/launch-resume/DiscoverDevices/cs/MainPage.xaml.cs" id="SnippetMakeFilterList":::

> [!NOTE]
> "proximal" フィルター値は、物理的な近さの度合いを保証するものではありません。 確実に物理的に近いことが求められるシナリオでは、フィルターに [**RemoteSystemDiscoveryType.SpatiallyProximal**](/uwp/api/windows.system.remotesystems.remotesystemdiscoverytype) を使用します。 現時点では、このフィルターで許容されるデバイスは Bluetooth 経由で検出されたものに限られます。 物理的に隣接していることを保証する新しい検出メカニズムとプロトコルがサポートされたら、このフィルターにも組み込まれます。  
また、検出したデバイスが実際に物理的に近い範囲内にあるかどうかを示す、[**RemoteSystem.IsAvailableBySpatialProximity**](/uwp/api/Windows.System.RemoteSystems.RemoteSystem.IsAvailableByProximity) という、[**RemoteSystem**](/uwp/api/Windows.System.RemoteSystems.RemoteSystem) クラスのプロパティもあります。

> [!NOTE]
> ローカル ネットワーク経由でデバイスを検出する場合 (検出の種類のフィルターの選択で決定されます)、ネットワークで "プライベート" または "ドメイン" プロファイルを使用する必要があります。 デバイスでは、"パブリック" ネットワーク経由で他のデバイスを検出しません。

[**IRemoteSystemFilter**](/uwp/api/Windows.System.RemoteSystems.IRemoteSystemFilter) オブジェクトの一覧を作成すると、**RemoteSystemWatcher** のコンストラクターに渡すことができます。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/launch-resume/DiscoverDevices/cs/MainPage.xaml.cs" id="SnippetCreateWatcher":::

このウォッチャーの [**Start**](/uwp/api/windows.system.remotesystems.remotesystemwatcher.start) メソッドが呼び出されると、次の条件を満たすデバイスが検出された場合のみ [**RemoteSystemAdded**](/uwp/api/windows.system.remotesystems.remotesystemwatcher.remotesystemadded) イベントが発生します。
* 近接接続によって検出可能
* デスクトップまたは携帯電話である
* 利用可能として分類されている

これ以降、イベントの処理、[**RemoteSystem**](/uwp/api/Windows.System.RemoteSystems.RemoteSystem) オブジェクトの取得、リモート デバイスへの接続の手順は「[リモート アプリの起動](launch-a-remote-app.md)」とまったく同じ手順です。 つまり、**RemoteSystem** オブジェクトは、各 **RemoteSystemAdded** イベントで渡される [**RemoteSystemAddedEventArgs**](/uwp/api/Windows.System.RemoteSystems.RemoteSystemAddedEventArgs) オブジェクトのプロパティとして格納されます。

## <a name="discover-devices-by-address-input"></a>アドレス入力によるデバイスの検出
ユーザーに関連付けられていないか、スキャンで検出することができないデバイスでも、検出側のアプリが直接アドレスを使う場合はアクセスできます。 リモート デバイスのアドレスを表すには、[**HostName**](/uwp/api/windows.networking.hostname) クラスを使います。 これは多くの場合 IP アドレスの形式で保存されますが、他のいくつかの形式も使うことができます (詳しくは、「[**HostName コンス トラクター**](/uwp/api/windows.networking.hostname.-ctor)」をご覧ください)。

**RemoteSystem** オブジェクトは、有効な **HostName** オブジェクトが指定された場合に取得されます。 アドレス データが無効な場合、`null` オブジェクト参照が返されます。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/launch-resume/DiscoverDevices/cs/MainPage.xaml.cs" id="SnippetFindByHostName":::

## <a name="querying-a-capability-on-a-remote-system"></a>リモート システムの機能の照会

検出フィルタリングからは分離されていますが、デバイスの機能を照会することが検出プロセスの重要な要素になることがあります。 [**RemoteSystem.GetCapabilitySupportedAsync**](/uwp/api/windows.system.remotesystems.remotesystem.GetCapabilitySupportedAsync) メソッドを使うと、リモート セッション接続や空間エンティティ (ホログラフィック) 共有などの特定の機能が、検出されたリモートシステムでサポートされているかどうかを照会できます。 照会可能な機能の一覧については、[**KnownRemoteSystemCapabilities**](/uwp/api/windows.system.remotesystems.knownremotesystemcapabilities) クラスをご覧ください。

```csharp
// Check to see if the given remote system can accept LaunchUri requests
bool isRemoteSystemLaunchUriCapable = remoteSystem.GetCapabilitySupportedAsync(KnownRemoteSystemCapabilities.LaunchUri);
```

## <a name="cross-user-discovery"></a>ユーザー間の検出

開発者は、同一ユーザーに登録されているデバイスだけでなく、クライアント デバイスの近くにある_すべての_デバイスを検出するように指定できます。 これは、特別な **IRemoteSystemFilter** である、[**RemoteSystemAuthorizationKindFilter**](/uwp/api/windows.system.remotesystems.remotesystemauthorizationkindfilter) を通じて実装されます。 その実装は、他の種類のフィルターと同じように行われます。

```csharp
// Construct a user type filter that includes anonymous devices
RemoteSystemAuthorizationKindFilter authorizationKindFilter = new RemoteSystemAuthorizationKindFilter(RemoteSystemAuthorizationKind.Anonymous);
// then add this filter to the RemoteSystemWatcher
```

* [**RemoteSystemAuthorizationKind**](/uwp/api/windows.system.remotesystems.remotesystemauthorizationkind) の値 **Anonymous** では、信頼されていないユーザーのデバイスも含めて、すべての近接デバイスを検出できるようになります。
* 値 **SameUser** では、クライアント デバイスと同じユーザーに登録されているデバイスのみを検出するようにフィルター処理が行われます。 これは既定の動作です。

### <a name="checking-the-cross-user-sharing-settings"></a>ユーザー間共有設定の確認

サインイン済みのデバイスで他のユーザーとエクスペリエンスを共有できるようにするには、検出アプリで上記のフィルターを指定することに加えて、クライアント デバイス自体の構成も必要になります。 このシステム設定は、次のように **RemoteSystem** クラスで静的メソッドを使用することで照会できます。

```csharp
if (!RemoteSystem.IsAuthorizationKindEnabled(RemoteSystemAuthorizationKind.Anonymous)) {
    // The system is not authorized to connect to cross-user devices. 
    // Inform the user that they can discover more devices if they
    // update the setting to "Anonymous".
}
```

この設定を変更するには、ユーザーが**設定**アプリを開く必要があります。 [ **System**  >  デバイス間でのシステム**共有エクスペリエンス**の  >  **共有**] メニューにはドロップダウンボックスがあり、ユーザーはそのシステムで共有できるデバイスを指定できます。

![[共有エクスペリエンス] 設定ページ](images/shared-experiences-settings.png)

## <a name="related-topics"></a>関連トピック
* [接続されるアプリやデバイス ("Rome" プロジェクト)](connected-apps-and-devices.md)
* [リモート アプリの起動](launch-a-remote-app.md)
* [リモート システムの API リファレンス](/uwp/api/Windows.System.RemoteSystems)
* [リモート システムのサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/RemoteSystems)
