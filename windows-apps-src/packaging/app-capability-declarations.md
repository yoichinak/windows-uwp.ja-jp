---
ms.assetid: 25B18BA5-E584-4537-9F19-BB2C8C52DFE1
title: アプリ機能の宣言
description: 特定の API、画像や音楽などのリソース、カメラやマイクなどのデバイスにアクセスするには、Windows アプリのパッケージ マニフェストで機能を宣言する必要があります。
ms.date: 02/10/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 0afcb617daae11e66d59eab566e226fbf74c1a93
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89158146"
---
# <a name="app-capability-declarations"></a>アプリ機能の宣言

特定の Windows 10 API、画像や音楽などのリソース、カメラやマイクなどのデバイスにアクセスするには、Windows アプリの[パッケージ マニフェスト](/uwp/schemas/appxpackage/appx-package-manifest)で機能を宣言する必要があります。 機能は、UWP アプリだけでなく、Windows 10 の MSIX または AppX パッケージにパッケージ化されているその他の種類のデスクトップ アプリでも使用されます。

特定のリソースまたは API へのアクセスを要求するには、アプリの [パッケージ マニフェスト](/uwp/schemas/appxpackage/appx-package-manifest) で機能を宣言します。 一般的な機能は、Visual Studio の[マニフェスト デザイナー](/windows/msix/package/packaging-uwp-apps#configure-an-app-package)を使用して宣言することができます。また、パッケージ マニフェストに手動で追加することもできます。 詳しくは、「[パッケージ マニフェストで機能を指定する方法](/uwp/schemas/appxpackage/how-to-specify-capabilities-in-a-package-manifest)」をご覧ください。 ユーザーがストアからアプリを入手するときに、アプリで宣言されているすべての機能が通知されることを知っておくのは重要です。 アプリに不要な機能を宣言しないでください。

一部の機能では、アプリが*機密性の高いリソース*にアクセスできます。 ユーザーの個人データにアクセスしたり、ユーザーに課金したりできるため、これらのリソースは機密性の高いリソースと見なされます。 設定アプリで管理されるプライバシー設定で、機密性の高いリソースへのアクセスを動的に制御することができます。 したがって、機密性の高いリソースが常に利用できるとアプリで認識されないことが重要です。 機密性の高いリソースへのアクセスについて詳しくは、「[個人データにアクセスするアプリのガイドライン](../security/index.md)」をご覧ください。 アプリが "*機密性の高いリソース*" にアクセスできる機能には、機能のシナリオの横にアスタリスク (\*) で注釈が付けられています。

機能にはいくつかの種類があります。

- [汎用的な機能](#general-use-capabilities): ほとんどの一般的なアプリ シナリオに適用されます。
- [デバイス機能](#device-capabilities): アプリが周辺機器および内部デバイスにアクセスできるようにします。
- [制限付き機能](#restricted-capabilities): Microsoft Store 申請の承認を必要とする機能、および通常は Microsoft と特定のパートナーのみが利用可能な機能。
- [カスタム機能](#custom-capabilities)。

## <a name="general-use-capabilities"></a>一般的な用途の機能

汎用的な機能を指定するには、アプリケーション パッケージ マニフェストで **Capability** 要素を使用します。 この種類の機能は、最も一般的なアプリ シナリオに適用されます。

> [!NOTE]
> すべての **Capability** 要素は、パッケージ マニフェストの **Capabilities** ノードの下で、すべての [CustomCapability](#custom-capabilities) 要素および [DeviceCapability](#device-capabilities) 要素の前に指定する必要があります。

| 機能のシナリオ | 機能の使用法 |
|---------------------|------------------|
| **音楽**\* | **musicLibrary** 機能は、ユーザーの [音楽] ライブラリへのプログラムによるアクセスを提供し、ユーザーの操作がなくてもアプリでライブラリ内のすべてのファイルを列挙し、ファイルにアクセスすることができるようにします。 通常、この機能は、音楽ライブラリ全体を利用するジュークボックス アプリで使われます。<br /><br />[  **ファイル ピッカー**](/uwp/api/Windows.Storage.Pickers.FileOpenPicker)は、アプリで使うファイルをユーザーが開くことができる強力な UI メカニズムを提供します。 **musicLibrary** 機能を宣言するのは、アプリのシナリオでプログラムによるアクセスが必要であるため、**ファイル ピッカー**では実現できない場合だけにしてください。<br /><br /> アプリのパッケージ マニフェストで宣言するとき、以下に示すように、**musicLibrary** 機能に **uap** 名前空間を含める必要があります。 <br /><br />```<Capabilities><uap:Capability Name="musicLibrary"/></Capabilities>```
| **画像**\* | **picturesLibrary** 機能は、ユーザーの [画像] ライブラリへのプログラムによるアクセスを提供し、ユーザーの操作がなくてもアプリでライブラリ内のすべてのファイルを列挙し、ファイルにアクセスすることができるようにします。 通常、この機能は、画像ライブラリ全体を利用する写真再生アプリで使われます。<br /><br />[  **ファイル ピッカー**](/uwp/api/Windows.Storage.Pickers.FileOpenPicker)は、アプリで使うファイルをユーザーが開くことができる強力な UI メカニズムを提供します。 **picturesLibrary** 機能を宣言するのは、アプリのシナリオでプログラムによるアクセスが必要であるため、**ファイル ピッカー**では実現できない場合だけにしてください。<br /><br />アプリのパッケージ マニフェストで宣言するとき、以下に示すように、**picturesLibrary** 機能に **uap** 名前空間を含める必要があります。<br /><br />```<Capabilities><uap:Capability Name="picturesLibrary"/></Capabilities>```
| **ビデオ**\* | **videosLibrary** 機能は、ユーザーのビデオへのプログラムによるアクセスを提供します。これによって、ユーザーの操作なしで、ライブラリ内のすべてのファイルを列挙してそれらのファイルにアクセスできます。 通常、この機能は、ビデオ ライブラリ全体を利用するムービー再生アプリで使われます。<br /><br />[  **ファイル ピッカー**](/uwp/api/Windows.Storage.Pickers.FileOpenPicker)は、アプリで使うファイルをユーザーが開くことができる強力な UI メカニズムを提供します。 **videosLibrary** 機能を宣言するのは、アプリのシナリオでプログラムによるアクセスが必要であるため、**ファイル ピッカー**では実現できない場合だけにしてください。<br /><br />アプリのパッケージ マニフェストで宣言するとき、以下に示すように、**videosLibrary** 機能に **uap** 名前空間を含める必要があります。<br /><br />```<Capabilities><uap:Capability Name="videosLibrary"/></Capabilities>```
| **リムーバブル ストレージ** | **removableStorage** 機能は、パッケージ マニフェストで宣言されたファイルの種類の関連付けに限定された、USB キーや外部ハード ドライブなどのリムーバブル ストレージ上のファイルへのプログラムによるアクセスを提供します。 たとえば、ドキュメント リーダー アプリで .doc ファイルの種類の関連付けを宣言すると、リムーバブル ストレージ デバイス上の .doc ファイルを開くことはできますが、他の種類のファイルを開くことはできません。 ユーザーのリムーバブル ストレージ デバイスにはさまざまな情報が含まれている可能性があり、リムーバブル ストレージにプログラムでアクセスするときは宣言された種類のファイルすべてについて正当な理由が求められるため、この機能を宣言する場合は注意が必要です。<br /><br />ユーザーは、関連付けが宣言されたファイルはアプリで処理されるものと考えます。 そのため、責任を持って処理できないファイルについては、関連付けを宣言しないでください。 [  **ファイル ピッカー**](/uwp/api/Windows.Storage.Pickers.FileOpenPicker)は、アプリで使うファイルをユーザーが開くことができる強力な UI メカニズムを提供します。<br /><br />**removableStorage** 機能を宣言するのは、アプリのシナリオでプログラムによるアクセスが必要であるため、[**ファイル ピッカー**](/uwp/api/Windows.Storage.Pickers.FileOpenPicker)では実現できない場合だけにしてください。<br /><br />アプリのパッケージ マニフェストで宣言するとき、以下に示すように、**removableStorage** 機能に **uap** 名前空間を含める必要があります。<br /><br />```<Capabilities><uap:Capability Name="removableStorage"/></Capabilities>```
| **インターネットとパブリック ネットワーク**\* | インターネットとパブリック ネットワークに対するさまざまなレベルのアクセスを提供する機能は 2 つあります。<br /><br />**internetClient** 機能を使うと、アプリはインターネットから着信データを受信できます。 サーバーとして機能することはできません。 ローカル ネットワーク アクセスはありません。<br />**internetClientServer** 機能を使うと、アプリはインターネットから着信データを受信できます。 サーバーとして機能できます。 ローカル ネットワーク アクセスはありません。<br /><br />Web サービス コンポーネントを持つほとんどのアプリで **internetClient** を使います。 着信ネットワーク接続をリッスンする必要があるピア ツー ピア (P2P) シナリオを実現するアプリは **internetClientServer** を使う必要があります。 **internetClientServer** 機能には **internetClient** 機能で提供されるアクセスが含まれるため、**internetClientServer** を指定した場合は **internetClient** を指定する必要はありません。
| **ホーム ネットワークと社内ネットワーク**\* | **privateNetworkClientServer** 機能は、ファイアウォール経由でのホーム ネットワークおよび社内ネットワークへの入力方向および出力方向のアクセスを提供します。 通常、この機能は、ローカル エリア ネットワーク (LAN) 上で通信するゲーム、およびさまざまなローカル デバイスでデータを共有するアプリで使われます。 アプリで **musicLibrary**、**picturesLibrary**、または **videosLibrary** を指定している場合は、ホーム グループ内の対応するライブラリにアクセスするためにこの機能を使う必要はありません。 Windows の場合、この機能はインターネットへのアクセスを提供しません。
| **予定** | **appointments** 機能は、ユーザーの予定ストアへのアクセスを提供します。 この機能によって、同期されたネットワーク アカウントから取得された予定や、予定ストアへの書き込みを行う他のアプリに対する読み取りアクセスが可能になります。 この機能を使うと、アプリで新しいカレンダーを作り、そのカレンダーに予定を書き込むことができます。<br /><br />アプリのパッケージ マニフェストで宣言するとき、以下に示すように、**appointments** 機能に **uap** 名前空間を含める必要があります。<br /><br />```<Capabilities><uap:Capability Name="appointments"/></Capabilities>```
| **連絡先**\* | **contacts** 機能は、さまざまな連絡先ストアからの連絡先が集約されたビューへのアクセスを提供します。 この機能によって、アプリでは、さまざまなネットワークやローカルの連絡先ストアから同期された連絡先に制限付きでアクセスできます (ネットワーク許可規則が適用されます)。<br /><br />アプリのパッケージ マニフェストで宣言するとき、以下に示すように、**contacts** 機能に **uap** 名前空間を含める必要があります。<br /><br />```<Capabilities><uap:Capability Name="contacts"/></Capabilities>```
| **コード生成** | **codeGeneration** 機能を使うと、アプリは JIT が可能になる次の機能にアクセスできます。<br /><br />[**VirtualProtectFromApp**](/windows/desktop/api/memoryapi/nf-memoryapi-virtualprotectfromapp)<br />[**CreateFileMappingFromApp**](/windows/desktop/api/memoryapi/nf-memoryapi-createfilemappingfromapp)<br />[**OpenFileMappingFromApp**](/windows/desktop/api/memoryapi/nf-memoryapi-openfilemappingfromapp)<br />[**MapViewOfFileFromApp**](/windows/desktop/api/memoryapi/nf-memoryapi-mapviewoffilefromapp)
| **AllJoyn** | **allJoyn** 機能を使うと、ネットワーク上の AllJoyn 対応のアプリやデバイスは、相互に検出を行い、対話することができます。<br /><br />[  **Windows.Devices.AllJoyn**](/uwp/api/Windows.Devices.AllJoyn) 名前空間の API にアクセスするすべてのアプリは、この機能を使う必要があります。
| **通話** | **phoneCall** 機能を使うと、アプリは、デバイスのすべての電話回線にアクセスし、次の機能を実行できます。<ul><li>ユーザーに確認を求めずに、電話回線での通話を実行してシステムのダイヤラーを表示します。</li><li>電話回線に関連するメタデータへアクセスします。</li><li>電話回線に関連するトリガーへアクセスします。</li><li>ユーザーが選んだスパム フィルター アプリで、禁止一覧と呼び出し元の情報を設定し、確認することができます。</li></ul>アプリのパッケージ マニフェストで宣言するとき、以下に示すように、**phoneCall** 機能に **uap** 名前空間を含める必要があります。<br /><br />```<Capabilities><uap:Capability Name="phoneCall"/></Capabilities>```<br /><br /><b>phoneCallHistoryPublic</b> 機能を使用すると、アプリはデバイス上の電話と一部の VoIP 通話履歴を読み取ることができます。 また、VoIP 通話履歴のエントリを書き込むこともできます。 この機能は、<a href="https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Calls.PhoneCallHistoryStore"><b>PhoneCallHistoryStore</b></a> クラスのすべてのメンバーへのアクセスに必要です。
| **通話が録音されているフォルダー**\* | **recordedCallsFolder** デバイス機能を使うと、アプリは通話が録音されているフォルダーにアクセスできます。<br /><br />アプリのパッケージ マニフェストで宣言するとき、以下に示すように、**recordedCallsFolder** 機能に **mobile** 名前空間を含める必要があります。<br /><br />```<Capabilities><mobile:Capability Name="recordedCallsFolder"/></Capabilities>```
| **ユーザー アカウント情報**\* | **userAccountInformation** 機能を使うと、アプリはユーザーの名前と画像にアクセスできるようになります。<br /><br />[  **Windows.System.UserProfile**](/uwp/api/Windows.System.UserProfile) 名前空間の一部の API にアクセスする場合は、この機能が必要になります。<br /><br />アプリのパッケージ マニフェストで宣言するとき、以下に示すように、**userAccountInformation** 機能に **uap** 名前空間を含める必要があります。<br /><br />```<Capabilities><uap:Capability Name="userAccountInformation"/></Capabilities>```
| **VoIP 通話** | **voipCall** 機能を使用すると、アプリは [**Windows.ApplicationModel.Calls**](/uwp/api/Windows.ApplicationModel.Calls) 名前空間の VoIP 通話 API にアクセスできます。<br /><br />アプリのパッケージ マニフェストで宣言するとき、以下に示すように、**voipCall** 機能に **uap** 名前空間を含める必要があります。<br /><br />```<Capabilities><uap:Capability Name="voipCall"/></Capabilities>```
| **3D オブジェクト** | **objects3D** 機能を使用すると、アプリは 3D オブジェクト ファイルにプログラムでアクセスできます。 通常、この機能は、3D オブジェクト ライブラリ全体にアクセスする必要がある 3D アプリやゲームで使用されます。<br /><br />[  **Windows.Storage**](/uwp/api/Windows.Storage) 名前空間の API を使って 3D オブジェクトを含むフォルダーにアクセスする場合は、この機能が必要になります。<br /><br />アプリのパッケージ マニフェストで宣言するとき、以下に示すように、**objects3D** 機能に **uap** 名前空間を含める必要があります。<br /><br />```<Capabilities><uap:Capability Name="objects3D"/></Capabilities>```
| **ブロックされているメッセージの読み取り**\* | **blockedChatMessages** 機能を使うと、アプリはスパム フィルター アプリでブロックされた SMS メッセージや MMS メッセージを読み取ることができます。<br /><br />[  **Windows.ApplicationModel.Chat**](/uwp/api/Windows.ApplicationModel.Chat) 名前空間の API を使ってブロックされたメッセージにアクセスする場合は、この機能が必要になります。<br /><br />アプリのパッケージ マニフェストで宣言するとき、以下に示すように、**blockedChatMessages** 機能に **uap** 名前空間を含める必要があります。<br /><br />```<Capabilities><uap:Capability Name="blockedChatMessages"/></Capabilities>```
| **カスタム デバイス** | **lowLevelDevices** 機能を使用すると、多数の追加要件が満たされた場合に、アプリはカスタム デバイスにアクセスできます。 この機能を **lowLevel** デバイス機能と混同しないでください。lowLevel デバイス機能は、GPIO、I2C、SPI、PWM デバイスにアクセスできるようにするものです。<br /><br /> [デバイス インターフェイス](/windows-hardware/drivers/install/device-interface-classes)を公開するカスタム ドライバーを開発し、このデバイスへのハンドルを開いて、IOCTL を送信する場合は、次の操作を行う必要があります。 <ul><li>アプリケーション マニフェストで、**lowLevelDevices** 機能を有効にします: ```<Capabilities><iot:Capability Name="lowLevelDevices"/></Capabilities>```。</li><li>[埋め込みモード](/windows/iot-core/develop-your-app/EmbeddedMode)を有効にします。</li><li>[INF](/previous-versions/windows/desktop/deviceaccess/register-the-device-interface-class-as-privileged) 内で、またはドライバー内で [WdfDeviceAssignInterfaceProperty()](/windows-hardware/drivers/ddi/content/wdfdevice/nf-wdfdevice-wdfdeviceassigninterfaceproperty) を呼び出して、デバイス インターフェイスを[制限付き](/windows-hardware/drivers/install/devpkey-deviceinterface-restricted)としてマークします。</ul>この後、[**Windows.Devices.Custom.CustomDevice**](/uwp/api/Windows.Devices.Custom.CustomDevice) を使用して、デバイスへのハンドルを開くことができます。 詳細については、「[内部デバイス用の UWP デバイス アプリ](/windows-hardware/drivers/devapps/uwp-device-apps-for-specialized-devices)」を参照してください。
| **IoT システム管理** | **systemManagement** 機能を使うと、アプリは基本的なシステム管理者特権 (シャットダウン、再起動、ロケール、タイムゾーンなど) を持つことができます。<br /><br />[  **Windows.System**](/uwp/api/Windows.System) 名前空間の一部の API にアクセスする場合は、この機能が必要になります。<br /><br />アプリのパッケージ マニフェストで宣言するとき、以下に示すように、**systemManagement** 機能に **iot** 名前空間を含める必要があります。<br /><br />```<Capabilities><iot:Capability Name="systemManagement"/></Capabilities>```
| **バックグラウンドでのメディア再生** | **backgroundMediaPlayback** 機能は、[**MediaPlayer**](/uwp/api/windows.media.playback.mediaplayer) クラスや [**AudioGraph**](/uwp/api/windows.media.audio.audiograph) クラスなど、メディア固有の API の動作を変更して、アプリがバック グラウンドで実行されている間のメディアの再生を有効にします。 アプリがバックグラウンドに移行しても、アクティブなすべてのオーディオ ストリームはミュートせず、音声を発し続けます。 また再生が行われている間はアプリが有効に保たれるように、アプリの有効期間が自動的に延長されます。
| **リモート システム** | **remoteSystem**機能を使うと、アプリがユーザーの Microsoft アカウントに関連付けられているデバイスの一覧にアクセスできるようになります。 デバイスの一覧へのアクセスは、実行した操作を複数のデバイス間で保持するために不可欠です。 この機能は、次の項目のすべてのメンバーにアクセスするために必要です。<ul><li>[Windows.System.RemoteSystems](/uwp/api/windows.system.remotesystems) 名前空間</li><li>[Windows.System.RemoteLauncher](/uwp/api/Windows.System.RemoteLauncher) クラス</li><li>[AppServiceConnection.OpenRemoteAsync](/uwp/api/windows.applicationmodel.appservice.appserviceconnection.openremoteasync) メソッド</li></ul> |
| **空間認識** | **spatialPerception** 機能は、空間マッピング データにプログラムでアクセスし、ユーザーに近い空間内でアプリケーション指定領域のサーフェスに関する情報を Mixed Reality アプリに提供します。  spatialPerception 機能は、これらのサーフェス メッシュをアプリで明示的に使用する場合にのみ宣言してください。ユーザーの頭部姿勢に基づいて Mixed Reality アプリでホログラフィック レンダリングを実行する場合、この機能は必要ありません。 |
| **グローバル メディア コントロール** | **globalMediaControl** 機能を使用すると、アプリは、[**SystemMediaTransportControls**](/uwp/api/Windows.Media.SystemMediaTransportControls) と統合されたシステム全体の再生セッションにアクセスして、再生情報を提供し、リモート コントロールを行うことができます。 [**Windows.Media.Control**](/uwp/api/windows.media.control) 名前空間内の一部のアプリを使用する場合は、この機能が必要になります。 この機能は、[uap7:Capability](/uwp/schemas/appxpackage/uapmanifestschema/element-uap7-capability) 要素で定義されます。  |

## <a name="device-capabilities"></a>デバイスの機能

デバイス機能を使用すると、アプリは周辺機器と内部デバイスにアクセスできます。 デバイス機能を指定するには、アプリケーション パッケージ マニフェストで **DeviceCapability** 要素を使用します。 この要素は追加の子要素を必要とする場合があり、一部のデバイス機能をパッケージ マニフェストに手動で追加する必要があります。 詳しくは、「[パッケージ マニフェストでデバイス機能を指定する方法](/uwp/schemas/appxpackage/how-to-specify-device-capabilities-in-a-package-manifest)」と「[**DeviceCapability スキーマ リファレンス**](/uwp/schemas/appxpackage/uapmanifestschema/element-devicecapability)」をご覧ください。

> [!NOTE]
> パッケージ マニフェストの **Capabilities** 要素の下に複数の **DeviceCapability** 要素を指定することができます。 すべての**DeviceCapability** 要素は、すべての **Capability** 要素および [CustomCapability](#custom-capabilities) 要素の後に指定する必要があります。

| 機能のシナリオ | 機能の使用法 |
|---------------------|------------------|
| **位置情報**\* | **location** 機能は、位置情報機能へのアクセスを提供します。この情報は PC が備えている GPS センサーなどの専用ハードウェアや、利用可能なネットワーク情報から取得されます。 アプリは、ユーザーが **[設定]** チャームで位置情報サービスを無効にした場合に対応する必要があります。 |
| **マイク** | **microphone** 機能は、マイクのオーディオ フィードへのアクセスを提供します。これによって、接続されたマイクからオーディオを録音できます。 アプリは、ユーザーが **[設定]** チャームでマイクを無効にした場合に対応する必要があります。 |
| **近接通信** | **proximity** 機能を使うと、きわめて近い場所にある複数のデバイスが相互に通信できます。 通常、この機能は、カジュアルなマルチプレーヤー ゲームや情報を交換するアプリで使われます。 デバイスは、Bluetooth、Wi-Fi、インターネットを含む、最適な接続を提供する通信テクノロジを使います。 この機能は、デバイス間の通信を開始するためにのみ使われます。 |
| **Web カメラ** | **webcam** 機能は、内蔵カメラや外付け Web カメラのビデオ フィードへのアクセスを提供します。これによって、アプリで写真やビデオをキャプチャできます。 Windows の場合、アプリはユーザーが **[設定]** チャームでカメラを無効にした場合に対応する必要があります。<br/>**webcam** 機能では、ビデオ ストリームへのアクセスだけが許可されます。 オーディオ ストリームへのアクセスも許可するには、**microphone** 機能を追加する必要があります。 |
| **USB** | **usb** デバイス機能を使うと、「[USB デバイスのアプリ マニフェスト パッケージの更新](/windows-hardware/drivers/usbcon/)」で API にアクセスできます。 |
| **ヒューマン インターフェイス デバイス (HID)** | **humaninterfacedevice** デバイス機能を使うと、「[HID のデバイス機能を指定する方法](/uwp/schemas/appxpackage/how-to-specify-device-capabilities-for-hid)」の API にアクセスできます。 |
| **店舗販売時点管理 (POS)** | **pointOfService** デバイス機能を使うと、[**Windows.Devices.PointOfService**](/uwp/api/Windows.Devices.PointOfService) 名前空間の API にアクセスできます。 この名前空間により、アプリは、Point of Service (POS) バー コード スキャナーや磁気ストライプ リーダーにアクセスできます。 この名前空間は、さまざまな製造元の POS デバイスに UWP アプリからアクセスするための、ベンダーに依存しないインターフェイスを提供します。 |
| **Bluetooth** | **bluetooth** デバイス機能を使うと、アプリは Generic Attribute (GATT) または Classic Basic Rate (RFCOMM) プロトコル経由で既にペアリングされている Bluetooth デバイスと通信できます。<br/>[  **Windows.Devices.Bluetooth**](/uwp/api/Windows.Devices.Bluetooth) 名前空間の一部の API を使う場合は、この機能が必要になります。 |
| **Wi-Fi ネットワーク** | **wiFiControl** デバイス機能を使うと、アプリはスキャンを実行して、Wi-Fi ネットワークに接続することができます。<br/>[  **Windows.Devices.WiFi**](/uwp/api/Windows.Devices.WiFi) 名前空間の一部の API を使う場合は、この機能が必要になります。 |
| **無線状態** | **radios** デバイス機能を使うと、アプリは Wi-Fi 通信と Bluetooth 通信を切り替えることができます。<br/>[  **Windows.Devices.Radios**](/uwp/api/Windows.Devices.Radios) 名前空間の API を使う場合は、この機能が必要になります。  |
| **光ディスク** | **optical** デバイス機能を使うと、アプリは、CD、DVD、ブルーレイなどの光ディスク ドライブの機能にアクセスできます。<br/>[  **Windows.Devices.Custom**](/uwp/api/Windows.Devices.Custom) 名前空間の一部の API を使う場合は、この機能が必要になります。 |
| **モーション アクティビティ** | デバイス機能 **activity** を使うと、アプリはデバイスの現在の動きを検出できるようになります。<br/>[  **Windows.Devices.Sensors**](/uwp/api/Windows.Devices.Sensors) 名前空間の一部の API を使う場合は、この機能が必要になります。 |
| **シリアル通信** | **serialcommunication** デバイス機能では Windows.Devices.SerialCommunication 名前空間の API へのアクセスが提供され、Windows アプリはシリアル ポートまたはシリアル ポートのアブストラクションを公開するデバイスと通信できるようになります。 [  **Windows.Devices.SerialCommnication**](/uwp/api/windows.devices.serialcommunication) 名前空間の API を使う場合は、この機能が必要になります。 |
| **視線トラッカー** | **gazeInput** 機能を使うと、互換性のある視線追跡デバイスが接続されているときにユーザーがアプリケーション境界内で見ている場所を検出できます。 [**Windows.Devices.Input.Preview**](/uwp/api/windows.devices.input.preview) 名前空間の一部の API を使用する場合は、この機能が必要になります。 |
| **GPIO、I2C、SPI、PWM** | デバイス機能 **lowLevel** を使用すると、GPIO、I2C、SPI、PWM デバイスにアクセスできます。 次の名前空間内の API を使用する場合は、この機能が必要になります: [**Windows.Devices.Gpio**](/uwp/api/windows.devices.gpio)、[**Windows.Devices.I2c**](/uwp/api/windows.devices.i2c)、[**Windows.Devices.Spi**](/uwp/api/windows.devices.spi)、[**Windows.Devices.Pwm**](/uwp/api/windows.devices.pwm)。<br /><br />```<Capabilities><DeviceCapability Name="lowLevel"/></Capabilities>``` |


<span id="special-and-restricted-capabilities" />

## <a name="restricted-capabilities"></a>制限付き機能

制限された機能をアプリで宣言する場合、アプリを Microsoft Store に発行するための承認を得るために、[アプリ申請プロセス](../publish/app-submissions.md)で情報を提供する必要があります。 この情報を申請の [[申請オプション]](../publish/manage-submission-options.md#restricted-capabilities) ページで提供して、宣言する各制限機能をアプリでどのように使用するかを説明します。

> [!IMPORTANT]
> 制限付き機能は、非常に特殊なシナリオ向けの機能です。 これらの機能は、用途が厳格に制限されており、ストアへの登録に際して追加のポリシーやレビューが適用されます。 制限付き機能を宣言するアプリは、承認を得なくてもサイドロードできます。 承認は、これらのアプリを App Store に提出するときのみ必要です。

アプリで本当に必要とされる場合を除いて、制限付き機能を宣言しないでください。 これらの機能が必要で適しているものとしては、身元を証明するデジタル証明書をスマート カードに含めるようにユーザーに求める 2 要素認証を使ったバンキング アプリなどがあります。 また、主に企業ユーザー向けに設計されたアプリや、ユーザーのドメイン資格情報がないとアクセスできない企業リソースへのアクセスを必要とするアプリもあります。

制限付き機能を宣言するには、[アプリ パッケージ マニフェスト](/uwp/schemas/appxpackage/appx-package-manifest) ソース ファイル (`Package.appxmanifest`) を変更します。 **xmlns:rescap** XML 名前空間宣言を追加し、制限付き機能を宣言する際に、**rescap** プレフィックスを使用します。 たとえば、**appCaptureSettings** 機能を宣言する方法を次に示します。

```xml
<?xml version="1.0" encoding="utf-8"?>
<Package
    ...
    xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
    IgnorableNamespaces="... rescap">
...
<Capabilities>
    <rescap:Capability Name="appCaptureSettings"/>
</Capabilities>
</Package>
```

> [!NOTE]
> すべての制限付き機能要素は、パッケージ マニフェストの **Capabilities** ノードの下で、すべての [CustomCapability](#custom-capabilities) 要素および [DeviceCapability](#device-capabilities) 要素の前に指定する必要があります。

### <a name="restricted-capability-approval-process"></a>制限付き機能の承認プロセス

以前は、機能を使う承認を得るためにサポートに問い合わせる必要がありました。 この情報は、[申請プロセス](../publish/app-submissions.md)の一部として[パートナー センター](https://partner.microsoft.com/dashboard)で提供できるようになりました。

申請するパッケージをアップロードすると、制限付き機能が宣言されているかどうかがチェックされます。 検出された場合、製品で各機能が使用されているかどうかに関する詳しい情報を[申請オプション](../publish/manage-submission-options.md#restricted-capabilities)ページで提供する必要があります。 製品が機能を宣言する必要がある理由を理解できるように、必ずできる限り詳しく入力してください。 これにより、申請が認定プロセスを完了するまでの時間がいくらか長くなる可能性があります。

認定プロセス中、テスターが提供された情報を確認し、その申請で機能の使用を承認するかどうかを判断します。 これにより、申請が認定プロセスを完了するまでの時間がいくらか長くなる可能性があります。 機能の使用が承認された場合、アプリの認定プロセスの残りの部分が続行されます。 一般に、アプリの更新プログラムを申請するときに機能の承認プロセスを繰り返す必要はありません (追加の機能を宣言する場合を除く)。

機能の使用が承認されない場合、申請は認定されず、認定レポートでフィードバックが提供されます。 その後、新しい申請を作成して、機能を宣言しないパッケージをアップロードすることを選択できます。または、該当する場合は、機能の使用に関連する問題を解決し、新しい申請で承認を要求します。

> [!NOTE]
> 申請でパートナー センターの開発サンドボックスを使用する場合 (たとえば、Xbox Live と統合されるゲームはこれに該当します)、**申請オプション** ページで情報を提供するのではなく事前に承認を要求する必要があります。 このためには、[Windows 開発者向けサポート ページ](https://developer.microsoft.com/windows/support)にアクセスしてください。 Developer support topic\(開発者サポート トピック\) で **Dashboard issue\(ダッシュボードの問題\)** 、問題の種類 で **アプリの申請**、サブカテゴリ で **その他** を選択します。 次に、機能の使用方法とそれが製品に必要な理由を説明します。 必要情報がすべて記載されていない場合、要求が拒否されます。 また別途、追加情報の提供を求められることがあります。 このプロセスには通常 5 営業日以上かかるため、十分前もってリクエストを送信してください。
>
> また、申請を開始する前に制限付き機能の使用が承認されることを確認したい場合は、開発サンドボックスを使用するかどうかに関係なく、(申請でこの情報を提供するのではなく) この方法を使用して承認を要求することもできます。

<span id="restricted-and-special-use-capability-list" />

### <a name="restricted-capability-list"></a>制限付き機能の一覧

次の表は、制限付き機能の一覧を示します。 上記で説明したプロセスに従うことによって、Microsoft Store に提出するアプリでこれらの機能の承認をリクエストすることができます。

> [!IMPORTANT]
> かなり限定された状況を除き、一部の制限付き機能が Microsoft Store に提出されるアプリで承認されることはほとんどありません。 これらの機能は、次の表で言及されています。 Microsoft Store で配布する予定の場合、アプリでこれらの機能を宣言しないことをお勧めします。

| 機能のシナリオ | 機能の使用法 |
|---------------------|------------------|
| **エンタープライズ** | Windows ドメイン資格情報により、ユーザーはそれぞれの資格情報を使ってリモートのリソースにログインし、ユーザー名とパスワードを指定したかのように動作できます。 **enterpriseAuthentication** 機能は、通常、企業内のサーバーに接続する基幹業務アプリで使用されます。 <br /><br />インターネット上での汎用通信にはこの機能は不要です。<br /><br />**enterpriseAuthentication** 機能は、共通の基幹業務アプリをサポートすることを目的としています。 企業リソースにアクセスする必要がないアプリでは宣言しないでください。 [  **ファイル ピッカー**](/uwp/api/Windows.Storage.Pickers.FileOpenPicker) は、アプリで使うネットワーク共有上のファイルをユーザーが開くことができる強力な UI メカニズムを提供します。 **enterpriseAuthentication** 機能は、アプリのシナリオでプログラムによるアクセスが必要であり、**ファイル ピッカー**を使用してこれを実現できない場合にのみ宣言してください。<br /><br />アプリのパッケージ マニフェストで宣言するとき、以下に示すように、**enterpriseAuthentication** 機能に **uap** 名前空間を含める必要があります。<br /><br />```<Capabilities><uap:Capability Name="enterpriseAuthentication"/></Capabilities>```<br /><br />**enterpriseDataPolicy** 機能を使用すると、アプリが Windows 情報保護ポリシー (たとえば、モバイル デバイス管理システム、モバイル アプリケーション管理システムなど) で管理されている場合に、アプリはエンタープライズ データを個別にかつ安全に処理することができます。  この制限付き機能を次のように宣言します。 <br /><br />```<Capabilities><rescap:Capability Name="enterpriseDataPolicy"/></Capabilities>```<br /><br />この機能は、次のクラスのすべてのメンバーを使うために必要です。<ul><li><a href="https://docs.microsoft.com/uwp/api/Windows.Security.EnterpriseData.FileProtectionManager">FileProtectionManager</a></li><li><a href="https://docs.microsoft.com/uwp/api/Windows.Security.EnterpriseData.DataProtectionManager">DataProtectionManager</a></li><li><a href="https://docs.microsoft.com/uwp/api/Windows.Security.EnterpriseData.ProtectionPolicyManager">ProtectionPolicyManager</a></li></ul> |
| **共有ユーザー証明書** | **sharedUserCertificates** 機能を使用すると、アプリは共有ユーザー ストア内のソフトウェアベースおよびハードウェアベースの証明書 (スマート カードに格納されている証明書など) を追加したり、それらの証明書にアクセスしたりすることができます。 通常、この機能は、認証にスマート カードを必要とする財務アプリまたはエンタープライズ アプリで使われます。<br /><br />アプリのパッケージ マニフェストで宣言するとき、以下に示すように、**sharedUserCertificates** 機能に **uap** 名前空間を含める必要があります。<br /><br />```<Capabilities><uap:Capability Name="sharedUserCertificates"/></Capabilities>``` |
|**ドキュメント**\* | **documentsLibrary** 機能は、パッケージ マニフェストで宣言されたファイルの種類の関連付けでフィルター処理して、ユーザーの [ドキュメント] ライブラリへのプログラムによるアクセスを提供します。 たとえば、ワード プロセッシング アプリで .doc ファイルの種類の関連付けを宣言した場合、アプリは、ユーザーの [ドキュメント] ライブラリ内の .doc ファイルを開くことができます。 <br /><br />**documentsLibrary** の機能は、ユーザーが介入*せずに*、アプリケーションがプログラムによってドキュメント ライブラリにアクセスする場合に*のみ*必要です。 ユーザーがピッカー API を使用してドキュメント ライブラリを選択する場合、アプリケーションがそれにアクセスするのに **documentsLibrary** の機能は必要では<b>ありません</b>。 一般に、アプリでは、次のピッカー API のいずれかを使用して、ユーザーが自分のファイルの場所を選択できるようにする必要があります。 <ul><li>[FileOpenPicker](/uwp/api/Windows.Storage.Pickers.FileOpenPicker): 既存のファイルを開きます。</li><li>[FileSavePicker](/uwp/api/windows.storage.pickers.filesavepicker): 新しいファイルを保存します。</li><li>[FolderPicker](/uwp/api/windows.storage.pickers.folderpicker): 追加のファイルを開く、または保存するフォルダーを選択します。</li></ul>これらの API を使用すると、ユーザーは、クラウド同期アカウント (たとえば、OneDrive) などの最適な場所を選択できます。 ユーザーがこれらの API を使用してファイルまたはフォルダーを選択すると、アプリは [FutureAccessList](/uwp/api/windows.storage.accesscache.storageapplicationpermissions.futureaccesslist) API を使用して、その場所に継続的にアクセスできるようになります。 この API を使用すると、アプリは、その後、ファイルまたはフォルダーを選択するように再度ユーザーに要求することなく、それらにアクセスすることができます。<br /><br />既存のワークフローで、ファイルが [ドキュメント] ライブラリ内にあることが想定されている場合 (たとえば、既存のデスクトップ アプリケーションとの相互運用)、またはユーザーが場所を選択しないで済むようにしたい場合、アプリケーションの **documentsLibrary** 機能を宣言することができます。 アプリケーションに **documentsLibrary** 機能を使用する場合、ユーザーが手動で場所を選択できるようにすることもお勧めします。<br /><br />アプリのパッケージ マニフェストで宣言するとき、以下に示すように、**documentsLibrary** 機能に **uap** 名前空間を含める必要があります。<br /><br />```<Capabilities><uap:Capability Name="documentsLibrary"/></Capabilities>``` |
| **ゲーム DVR 設定** | 制限付き機能 **appCaptureSettings** を使うと、アプリはゲーム DVR のユーザー設定を制御できます。<br /><br />[  **Windows.Media.Capture**](/uwp/api/Windows.Media.Capture) 名前空間の一部の API を使う場合は、この機能が必要になります。 <br /><br />Microsoft Store に申請するアプリケーションでこの機能を宣言することはお勧めしません。 ほとんどの場合、この機能の使用は承認されません。  |
| **携帯ネットワーク** | 制限された機能 **cellularDeviceControl** を使うと、アプリは携帯デバイスを制御できます。<br /><br />**cellularDeviceIdentity** 機能を使うと、アプリは携帯デバイスの ID データにアクセスできます。<br /><br />**cellularMessaging** 機能を使うと、アプリは SMS と RCS を利用できます。<br /><br />[  **Windows.Devices.Sms**](/uwp/api/Windows.Devices.Sms) 名前空間の一部の API を使う場合は、これらの機能が必要になります。  |
| **デバイスのロック解除** | 制限された機能 **deviceUnlock** を使うと、アプリは、開発者サイドローディングのシナリオやエンタープライズ サイドローディングのシナリオ向けにデバイスをロック解除できます。<br /><br /> Microsoft Store に申請するアプリケーションでこの機能を宣言することはお勧めしません。 ほとんどの場合、この機能の使用は承認されません。 |
| **デュアル SIM タイル** | 制限された機能 **dualSimTiles** を使うと、アプリは、複数の SIM を備えたデバイスでアプリ一覧の追加のエントリを作成できます。<br /><br />[  **Windows.UI.StartScreen**](/uwp/api/Windows.UI.StartScreen) 名前空間の一部の API を使う場合は、この機能が必要になります。 |
| **エンタープライズ共有記憶域** | 制限された機能 **enterpriseDeviceLockdown** を使うと、アプリは、デバイスのロック ダウン API を利用したり、企業で共有している保存フォルダーにアクセスしたりすることができます。 <br /><br />Microsoft Store に申請するアプリケーションでこの機能を宣言することはお勧めしません。 ほとんどの場合、この機能の使用は承認されません。 |
| **システム入力の挿入** | 制限付き機能 **inputInjectionBrokered** を使用すると、アプリは、さまざまな形式の入力 (HID、タッチ、ペン、キーボード、マウスなど) をプログラムでシステムに挿入できます。 通常、この機能はシステムを制御できる共同作業アプリで使われます。<br /><br />PC の場合、この機能を使ったアプリからの入力の挿入は、同じアプリ コンテナー内のプロセスによってのみ許可されます。<br /><br />```<Capabilities><rescap:Capability Name="inputInjectionBrokered" /></Capabilities>``` |
| **入力の監視**\* | 制限された機能 **inputObservation** を使うと、アプリは、さまざまな形式の未加工入力 (HID、タッチ、ペン、キーボード、マウスなど) が、最終的な宛先に関係なく、システムによって許可されるのを監視できます。<br /><br />この機能とそれに関連する API を使用できるのは、選択された Microsoft パートナーのみに限られます。  |
| **入力の抑制** | 制限付き機能 **inputSuppression** を使うと、アプリは、さまざまな形式の未加工入力 (HID、タッチ、ペン、キーボード、マウスなど) が、システムによって許可されるのを抑制できます。<br /><br />この機能とそれに関連する API を使用できるのは、選択された Microsoft パートナーのみに限られます。 |
| **VPN アプリ** | 制限付き機能 **networkingVpnProvider** を使うと、アプリは VPN 機能 (接続の管理や VPN プラグイン機能の提供など) へのフル アクセスが可能になります。<br /><br />[  **Windows.Networking.Vpn**](/uwp/api/Windows.Networking.Vpn) 名前空間の一部の API を使う場合は、この機能が必要になります。 |
| **その他のアプリ管理** | 制限付き機能 **packageManagement** を使うと、アプリは他のアプリを直接管理できます。<br /><br />**packageQuery** デバイス機能を使うと、アプリは他のアプリに関する情報を収集できます。<br /><br />[  **PackageManager**](/uwp/api/Windows.Management.Deployment.PackageManager) クラスの一部のメソッドとプロパティにアクセスする場合は、これらの機能が必要になります。 |
| **画面の投影** | 制限付き機能 **screenDuplication** を使うと、アプリは画面を別のデバイスに表示できます。<br /><br />DirectX 名前空間の API を使う場合は、この機能が必要になります。 <br /><br />Microsoft Store に申請するアプリケーションでこの機能を宣言することはお勧めしません。 ほとんどの場合、この機能の使用は承認されません。 |
| **ユーザー プリンシパル名** | 制限付き機能 **userPrincipalName** を使用すると、アプリは、現在のユーザーのユーザー プリンシパル名 (UPN) にアクセスできます。<br /><br />[  **GetUserNameEx**](/windows/desktop/api/secext/nf-secext-getusernameexa) 関数を呼び出す場合は、この機能が必要になります。 <br /><br />Microsoft Store に申請するアプリケーションでこの機能を宣言することはお勧めしません。 ほとんどの場合、この機能の使用は承認されません。 |
| **ウォレット** | 制限付き機能 **walletSystem** を使うと、アプリは保存されたウォレット カードへのフル アクセスが可能になります。<br /><br />[  **Windows.ApplicationModel.Wallet.System**](/uwp/api/Windows.ApplicationModel.Wallet.System) 名前空間の API を使う場合は、この機能が必要になります。 <br /><br />Microsoft Store に申請するアプリケーションでこの機能を宣言することはお勧めしません。 ほとんどの場合、この機能の使用は承認されません。 |
| **位置情報の履歴** | 制限付き機能 **locationHistory** を使うと、アプリはデバイスの位置情報の履歴にアクセスできます。<br /><br />[  **Windows.Devices.Geolocation**](/uwp/api/Windows.Devices.Geolocation) 名前空間の API を使う場合は、この機能が必要になります。
| **アプリを閉じる確認** | 制限された機能 **confirmAppClose** を使うと、アプリはアプリ自体とアプリ独自のウィンドウを閉じたり、アプリを閉じることを遅延させたりすることができます。<br /><br />アプリは、Windows 10 Version 1703 (ビルド 10.0.15063) 以上でこの機能を要求できます。 以前の Windows 10 バージョンでは、この機能はプライベートであり、"このアプリケーションには、要求された機能を許可できません" というエラー メッセージでアプリのインストールが失敗になります。 |
| **通話履歴**\* | 制限付き機能 **phoneCallHistory** を使うと、アプリは通話履歴を読み取ったり、履歴のエントリを削除できます。<br /><br />[  **Windows.ApplicationModel.Chat**](/uwp/api/Windows.ApplicationModel.Chat) 名前空間の API を使う場合は、この機能が必要になります。 <br /><br />Microsoft Store に申請するアプリケーションでこの機能を宣言することはお勧めしません。 ほとんどの場合、この機能の使用は承認されません。 |
| **システム レベルの予定へのアクセス** | 制限された機能 **appointmentsSystem** を使うと、アプリはユーザーのカレンダーにあるすべての予定を読み取ったり、変更したりすることができます。<br /><br />[  **Windows.ApplicationModel.Appointment**](/uwp/api/Windows.ApplicationModel.Appointments) 名前空間の API を使う場合は、この機能が必要になります。 <br /><br />Microsoft Store に申請するアプリケーションでこの機能を宣言することはお勧めしません。 ほとんどの場合、この機能の使用は承認されません。 |
| **システム レベルのチャット メッセージへのアクセス**\* | 制限付き機能 **chatSystem** を使うと、アプリはすべての SMS メッセージと MMS メッセージの読み取りと書き込みを実行できます。<br />[  **Windows.ApplicationModel.Chat**](/uwp/api/Windows.ApplicationModel.Chat) 名前空間の API を使う場合は、この機能が必要になります。 <br /><br />Microsoft Store に申請するアプリケーションでこの機能を宣言することはお勧めしません。 ほとんどの場合、この機能の使用は承認されません。 |
| **システム レベルの連絡先へのアクセス** | 制限付き機能 **contactsSystem** を使うと、アプリは制限付きの連絡先情報や機密性の高い連絡先情報を読み取ったり、既存の連絡先情報を変更したりすることができます。<br /><br />[  **Windows.ApplicationModel.Chat**](/uwp/api/Windows.ApplicationModel.Chat) 名前空間の API を使う場合は、この機能が必要になります。 <br /><br />Microsoft Store に申請するアプリケーションでこの機能を宣言することはお勧めしません。 ほとんどの場合、この機能の使用は承認されません。 |
| **電子メールへのアクセス** | 制限付き機能 **email** を使うと、アプリはユーザーのメールの読み取り、トリアージ、送信を実行できます。<br /><br />[  **Windows.ApplicationModel.Email**](/uwp/api/Windows.ApplicationModel.Email) 名前空間の API を使う場合は、この機能が必要になります。 <br /><br />Microsoft Store に申請するアプリケーションでこの機能を宣言することはお勧めしません。 ほとんどの場合、この機能の使用は承認されません。 |
| **システム レベルのメールへのアクセス**| 制限された機能 **emailSystem** を使うと、アプリは制限つきのユーザーのメールや機密性の高いユーザーのメールの読み取り、トリアージ、送信を実行できます。 <br /><br />[  **Windows.ApplicationModel.Email**](/uwp/api/Windows.ApplicationModel.Email) 名前空間の API を使う場合は、この機能が必要になります。 <br /><br />Microsoft Store に申請するアプリケーションでこの機能を宣言することはお勧めしません。 ほとんどの場合、この機能の使用は承認されません。 |
| **システム レベルの通話履歴へのアクセス** | 制限付き機能 **phoneCallHistorySystem** を使うと、アプリは通話履歴を完全に変更できます (既存のエントリの変更や新しいエントリの作成など)。<br /><br />[  **Windows.ApplicationModel.Calls**](/uwp/api/Windows.ApplicationModel.Calls) 名前空間の API を使う場合は、この機能が必要になります。 <br /><br />Microsoft Store に申請するアプリケーションでこの機能を宣言することはお勧めしません。 ほとんどの場合、この機能の使用は承認されません。 |
| **テキスト メッセージの送信**\* | 制限された機能 **smsSend** を使うと、アプリは SMS メッセージや MMS メッセージを送信できます。<br /><br />[  **Windows.ApplicationModel.Chat**](/uwp/api/Windows.ApplicationModel.Chat) 名前空間の API を使う場合は、この機能が必要になります。 |
| **システム レベルのすべてのユーザー データへのアクセス** | 制限付き機能 **userDataSystem** を使うと、アプリはシステム データ ストアに保存されているユーザー データへのアクセスが可能になります。 <br /><br />Microsoft Store に申請するアプリケーションでこの機能を宣言することはお勧めしません。 ほとんどの場合、この機能の使用は承認されません。 |
| **ストア プレビュー機能** | 制限付き機能 **previewStore** を使うと、アプリはアプリ内製品の SKU の取得や購入ができます。<br /><br />[  **Windows.ApplicationModel.Store.Preview**](/uwp/api/Windows.ApplicationModel.Store.Preview) 名前空間の特定の API を使う場合は、この機能が必要になります。 |
| **初回サインイン時の設定** | 制限された機能 **firstSignInSettings** を使うと、アプリは、ユーザーが初めてデバイスにサインインしたときに設定されたユーザー設定にアクセスできます。 |
| **Windows チーム エクスペリエンス** | 制限付き機能 **teamEditionExperience** を使うと、アプリは、Windows チーム セッションの多くの経験的側面を制御する内部 API にアクセスできます。 Windows チーム セッションは、Microsoft Surface Hub など、チーム デバイスで実行されている可能性があります。 <br /><br />Microsoft Store に申請するアプリケーションでこの機能を宣言することはお勧めしません。 ほとんどの場合、この機能の使用は承認されません。 |
| **リモート ロック解除** | 制限付き機能 **remotePassportAuthentication** を使うと、アプリは、リモート PC のロック解除に使用される資格情報にアクセスできます。 <br /><br />Microsoft Store に申請するアプリケーションでこの機能を宣言することはお勧めしません。 ほとんどの場合、この機能の使用は承認されません。 |
| **コンポジションのプレビュー** | 制限された機能 **previewUiComposition** を使うと、アプリはユーザー インターフェイスの [**Windows.UI.Composition**](/uwp/api/Windows.UI.Composition) 名前空間をプレビューすることで、完成前に API に関するフィードバックを提供できます。 詳細については、wincomposition@microsoft.com にお問い合わせください。 |
| **安全な評価のためのロックダウン** | 制限された機能 **secureAssessment** を使うと、アプリは安全な評価のために単一アプリ モードに Windows をロックダウンできます。 <br /><br />Microsoft Store に申請するアプリケーションでこの機能を宣言することはお勧めしません。 ほとんどの場合、この機能の使用は承認されません。 |
| **接続マネージャーのプロビジョニング** | 制限付き機能 **networkConnectionManagerProvisioning** を使うと、アプリは、デバイスを WWAN および WLAN インターフェイスに接続するポリシーを定義できます。 この機能を使うアプリは、携帯電話会社が作成し、モバイル ネットワークへのデバイス接続を管理します。 |
| **データ通信プランのプロビジョニング** | 制限付き機能 **networkDataPlanProvisioning** を使うと、アプリは、デバイスのデータ プランに関する情報を収集し、ネットワーク使用状況を読み取れます。 この機能を使うアプリは、携帯電話会社が作成し、ユーザーの実際のデータ使用量を OS データ使用量の設定に統合します。 |
| **ソフトウェア ライセンス** | 制限された機能 **slapiQueryLicenseValue** を使うと、アプリは、ソフトウェア ライセンス ポリシーにクエリを実行できます。 <br /><br />Microsoft Store に申請するアプリケーションでこの機能を宣言することはお勧めしません。 ほとんどの場合、この機能の使用は承認されません。 |
| **延長実行** | 制限付き機能 **extendedBackgroundTaskTime** を使うと、バックグラウンド タスクが実行時間制限によって取り消されたり停止されたりすることがなくなります。 ただし、この場合も、他のメモリ使用制限や電力使用制限は、すべてバックグラウンド タスクに適用されます。 この機能は、バッテリーの使用やプライバシーなどのバックグラウンド アプリ設定を使用して制限できます。 ユーザーと管理者がグループ ポリシー設定を使うとバックグラウンド タスクを制御できる点に注意してください。<br /><br />制限された機能 **extendedExecutionBackgroundAudio** を使うと、アプリがフォアグラウンドにないとき、アプリはオーディオを再生できます。<br /><br />制限付き機能 **extendedExecutionCritical** を使うと、アプリは重要な延長実行セッションを開始できます。<br /><br />制限付き機能 **extendedExecutionUnconstrained** を使うと、アプリは制限のない延長実行セッションを開始できます。 <br /><br />Microsoft Store に申請するアプリケーションでこの機能を宣言することはお勧めしません。 ほとんどの場合、この機能の使用は承認されません。<br /><br />延長実行を使用してアプリが中断されるタイミングを延期する方法については、「[延長実行を使ってアプリの中断を延期する](../launch-resume/run-minimized-with-extended-execution.md)」をご覧ください。 |
| **モバイル デバイス管理** | 制限された機能 **deviceManagementDmAccount** を使うと、アプリは、携帯電話会社の Open Mobile Alliance - Device Management (MO OMA-DM) アカウントをプロビジョニング、構成できます。<br /><br />制限付き機能 **deviceManagementFoundation** を使うと、アプリは、デバイスのモバイル デバイス管理 (MDM) 構成サービス プロバイダー (CSP) インフラストラクチャへの基本的なアクセスが可能になります。 他の機能は、特定の CSP にアクセスする必要があることに注意してください。<br /><br />制限付き機能 **deviceManagementWapSecurityPolicies** を使うと、アプリは、ワイヤレス アプリケーション プロトコル (WAP) ベースのサービス (MM、Service Indication/Service Loading (SI/SL)、Open Mobile Alliance - Client Provisioning (OMA-CP) など) を構成できます。<br /><br />制限された機能 **deviceManagementEmailAccount** を使うと、携帯電話会社が作成したアプリは、ユーザーにプロビジョニングするデバイス上の電子メール アカウントを追加および管理できます。 |
| **パッケージ ポリシー制御** | 制限付き機能 **packagePolicySystem** を使うと、アプリは、デバイスにインストールされたアプリに関連するシステム ポリシーを制御できます。<br /><br />Microsoft Store に申請するアプリケーションでこの機能を宣言することはお勧めしません。 ほとんどの場合、この機能の使用は承認されません。 |
| **ゲームの一覧** | 制限付き機能 **gameList** を使うと、アプリはシステムにインストールされた既知のゲームの一覧を取得できます。<br /><br />Microsoft Store に申請するアプリケーションでこの機能を宣言することはお勧めしません。 ほとんどの場合、この機能の使用は承認されません。 |
| **Xbox アクセサリ** | 制限付き機能 **xboxAccessoryManagement** を使うと、アプリは Xbox ハードウェア仕様に準拠した Xbox デバイスを直接管理できます。<br /><br />Microsoft Store に申請するアプリケーションでこの機能を宣言することはお勧めしません。 ほとんどの場合、この機能の使用は承認されません。 |
| **アクセサリの音声認識** | 制限付き機能 **cortanaSpeechAccessory** を使うと、アプリはコマンドを呼び出して Cortana に渡すことができます。<br /><br />Microsoft Store に申請するアプリケーションでこの機能を宣言することはお勧めしません。 ほとんどの場合、この機能の使用は承認されません。 |
| **アクセサリ管理** | 制限付き機能 **accessoryManager** を使うと、アプリはアクセサリ アプリや特定のアプリ通知のオプトインとしての登録が可能になり、アクセサリに転送したり、ユーザーに表示したりできるようになります。 |
| **ドライバー アクセス** | 制限された機能 **interopServices** を使うと、アプリはデバイスと直接やり取りできます。 <br /><br />Microsoft Store に申請するアプリケーションでこの機能を宣言することはお勧めしません。 ほとんどの場合、この機能の使用は承認されません。 |
| **フォアグラウンドの監視** | 制限された機能 **inputForegroundObservation** を使うと、アプリはフォアグラウンドでキーボード入力を傍受でき、アプリ以外へのすべてのキーボード入力の処理を省くことができます。 SAS の組み合わせはこの機能により傍受することはできません。 この機能は、[**KeyboardDeliveryInterceptor**](/uwp/api/Windows.UI.Input.KeyboardDeliveryInterceptor) クラスのメンバーにアクセスするために必要です。
| **OEM および MO のパートナー アプリ** | 制限付き機能 **oemDeployment** を使うと、Microsoft パートナー製のアプリは、新しいアプリをインストールし、デバイスに現在インストールされているアプリを照会できます。<br /><br />制限された機能 **oemPublicDirectory** を使うと、Microsoft パートナー製のアプリは、共有アプリ フォルダーにアクセスできます。 Microsoft Store に申請するアプリケーションでこの機能を宣言することはお勧めしません。 ほとんどの場合、この機能の使用は承認されません。 |
| **アプリのライセンス** | 制限された機能 **appLicensing** を使うと、ライセンスの必要なくアプリを実行できます。 マニフェストにこの機能を宣言している場合、Microsoft Store にアプリを提出することはできません。 <br /><br />Microsoft Store に申請するアプリケーションでこの機能を宣言することはお勧めしません。 ほとんどの場合、この機能の使用は承認されません。 |
| **位置情報システム**| 制限付き機能 **locationSystem** を使うと、アプリは特権のある特定の場所の構成 (デバイスの既定の場所の設定など) を実行できます。 <br /><br />Microsoft Store に申請するアプリケーションでこの機能を宣言することはお勧めしません。 ほとんどの場合、この機能の使用は承認されません。 |
| **ユーザー データ アカウント プロバイダー**| 制限された機能 **userDataAccountsProvider** を使うと、アプリはメール、カレンダー、連絡先のアカウントを完全に管理できます。 |
| **ペン ワークスペース** | **previewPenWorkspace** 機能を使うと、アプリは、ペン ワークスペースの内側でアクション記憶ハンドラとしてホストされる Windows.ApplicationModel.Preview.Notes 名前空間にアクセスできます。 |
| **セカンダリ認証要素** | **secondaryAuthenticationFactor** 機能を使うと、アプリは近くにあるコンパニオン認証デバイス上のシークレット ストアを渡して、PC のロックを解除できます。 たとえば、コンパニオン フィットネス バンドを使用して、PC のロックを解除できます。 Windows.Security.Authentication.Identity.Provider 名前空間の API にアクセスするには、この機能が必要です。<br /><br />Microsoft Store に申請するアプリケーションでこの機能を宣言することはお勧めしません。 ほとんどの場合、この機能の使用は承認されません。 |
| **ストア ライセンスの管理**| **storeLicenseManagement**機能を使うと、Microsoft パートナー ハブ アプリで、デバイス上のストア ライセンスを管理できます。 Windows.ApplicationModel.Store.LicenseManagement 名前空間の API にアクセスするには、この機能が必要です。 |
| **ユーザー システム ID**| **userSystemId** 機能を使うと、アプリは、ユーザー固有のシステム識別子を取得できます。 この識別子は特定のシステムで現在のユーザーを一意に識別し、アプリ間での情報の関連付けに使うことができます。 Windows.System.Profile.SystemIdentification クラスの GetUserSpecificSystemId API にアクセスするには、この機能が必要です。 |
| **対象コンテンツ**| **targetedContent** 機能を使うと、アプリケーションは、[**Windows.Services.TargetedContent**](/uwp/api/windows.services.targetedcontent) 名前空間によって提供される対象のサブスクリプション コンテンツを取得して使用できます。<br /><br />**Windows.System.Profile.SystemIdentification** 名前空間の一部の API を使用するには、この機能が必要になります。 |
| **UI オートメーション**| **uiAutomation** 機能を使うと、ナレーターなどの UI オートメーション クライアントを UI オートメーション サーバーまたはプロバイダーに接続できます。<br /><br />**Windows.Xbox.Media.Capture.Broadcaster** 名前空間の一部の API を使う場合は、この機能が必要になります。 |
|**ゲーム バー サービス**| **gameBarServices** の使用は、ストア更新可能なファースト パーティ製 UWA に限定されます。<br /><br />[  **Windows.Media.Capture.GameBarsSrvices**](/uwp/api/windows.media.capture.gamebarservices) クラスを使う場合は、この機能が必要になります。<br /><br />Microsoft Store に申請するアプリケーションでこの機能を宣言することはお勧めしません。 ほとんどの場合、この機能の使用は承認されません。 |
|**アプリ キャプチャ サービス**| **appCaptureServices** 機能の使用は、Microsoft との契約関係がある当事者に限定されます。 契約関係は、Xbox サービスおよび bizdev の支援によって成り立っているパートナー契約に基づいて設定されます。<br /><br />[  **Windows.Media.Capture.AppCaptureServices**](/uwp/api/windows.media.capture.appcaptureservices) クラスを使う場合は、この機能が必要になります。 |
|**アプリ ブロードキャスト サービス**| **appBroadcastServices** 機能の使用は、Microsoft との契約関係がある当事者に限定されます。 契約関係は、Xbox サービスの支援によって成り立っているパートナー契約に基づいて設定されます。<br /> <br />[  **Windows.Media.capture.AppBroadcastServices**](/uwp/api/windows.media.capture.appbroadcastservices) クラスを使う場合は、この機能が必要になります。<br /><br />Microsoft Store に申請するアプリケーションでこの機能を宣言することはお勧めしません。 ほとんどの場合、この機能の使用は承認されません。 |
|**オーディオ デバイスの構成**| **audioDeviceConfiguration** 機能を使うと、アプリケーションは、オーディオ ドライバーによって公開されるオーディオ エフェクトを照会、構成、有効化、および無効化できます。 <br /> <br />[  **Windows.Media.Devices.AudioDeviceModulesManager**](/uwp/api/windows.media.devices.audiodevicemodulesmanager) クラスを使う場合は、この機能が必要になります。<br /><br />Microsoft Store に申請するアプリケーションでこの機能を宣言することはお勧めしません。 ほとんどの場合、この機能の使用は承認されません。 これは、アプリケーションで **AudioDeviceModulesManager** を使用すると、システム上のすべてのオーディオ エフェクトにアクセスできるためです。 可能性として、オーディオ エフェクトの設定によっては、デバイス上のオーディオ パフォーマンスに悪影響を与えることができます。 |
| **バックグラウンドでのメディアの記録** | **backgroundMediaRecording** 機能は、[**MediaCapture**](/uwp/api/windows.media.capture.mediacapture) クラスや [**AudioGraph**](/uwp/api/windows.media.audio.audiograph) クラスなど、メディア固有の API の動作を変更して、アプリがバック グラウンドで実行されている間のメディアの記録を有効にします。 |
|**プレビュー インク ワークスペース**| **previewInkWorkspace** 機能を使うと、アプリは、Ink ワークスペース内でホストされている Preview Ink 名前空間にアクセスできます。 一般的に、この機能は、デバイス上のホワイト ボード アプリケーションを置き換えるために、OEM によって使用されます。<br /> <br />[  **Windows.ApplicationModel.Preview.InkWorkspace**](/uwp/api/windows.applicationmodel.preview.inkworkspace) 名前空間の API を使う場合は、この機能が必要になります。 |
|**スタート画面の管理**| **startScreenManagement** 機能を使うと、アプリは、スタート画面にタイルを自動的にピン留めすることができます。 アプリは、バックグラウンドでピン留めすることもできます。 **startScreenManagement** 機能がないといずれかの API がブロックされるということではなく、**startScreenManagement** を使用すると、アプリで Pin API を使用しているときにシェルによって UI が表示されなくなります。 |
|**Cortana のアクセス許可**| **cortanaPermissions** 機能を使うと、アプリは、デバイス上でユーザーが Cortana に付与したアクセス許可を列挙できます。 また、アプリはこの機能によって、デバイス上で Cortana のアクセス許可の付与および取り消しを行うことができます。 **cortanaPermissions** を使うには、アクセス許可を付与する前にデバイスで法的なテキストを表示する必要があります。 アプリには、アクセス許可を変更した場合に生じる法的な影響をユーザーに通知する義務があります。<br /> <br /><br />レジストリ設定 **HKCU\SOFTWARE\Microsoft\Windows\CurrentVersion\Search** に対する読み取りアクセス許可を取得するには、この機能が必要になります。<br /><br />Microsoft Store に申請するアプリケーションでこの機能を宣言することはお勧めしません。 ほとんどの場合、この機能の使用は承認されません。 |
|**すべてのアプリの MOD**| **allAppMods** 機能を使うと、アプリは、すべてのアプリに対して AppMods フォルダーにアクセスできます。 Mod 管理ユーティリティでは **allAppMods** を使用し、MOD を使用するゲームやアプリの外で、その MOD を管理します。 |
|**拡張リソース**| **expandedResources** 機能を使うと、アプリは、ゲーム モードのリソースにアクセスできます。 Xbox と、ゲーム バーに対応する PC で、ゲーム モードのリソースとは、アプリ専用に予約されている、使用可能な CPU コアのサブセットを表します。 Xbox では、アプリは 4 GB 以上のメモリ パーティションも排他的に使用できます。<br /><br />前述のように CPU とメモリのリソースを排他的に使用するには、この機能が必要になります。 |
|**保護されたアプリ**| **protectedApp** 機能を使うと、ストアによって保護されているプロセスにアプリを読み込むことができます。 アプリがストアに取り込まれると、ストアによって実行可能ファイルに blob が追加されます。 ストアでは、Microsoft キーを使って実行可能ファイルへの署名も行われます。 blob には Microsoft の署名が必要であるため、プロセス ローダーは、保護されたプロセスを適用する機能ではなく、この blob をチェックします。<br /><br />Microsoft Store に申請するアプリケーションでこの機能を宣言することはお勧めしません。 ほとんどの場合、この機能の使用は承認されません。 |
|**ゲーム モニター**| **gameMonitor** 機能を使うと、アプリによるゲーム不正がシステムのアクティブ監視で検出されるようになります。<br /><br />Microsoft Store に申請するアプリケーションでこの機能を宣言することはお勧めしません。 ほとんどの場合、この機能の使用は承認されません。 |
|**アプリの診断**| **appDiagnostics** 機能を使うと、アプリは、実行中の他の UWP アプリに関する診断情報 (パッケージ情報、メモリ使用量、アカウント名など) を取得できます。 返される情報には、アプリの実行に使用されたドメイン/コンピューター アカウント名が含まれます。呼び出し元のアプリが管理者権限で起動されている場合、そのアプリは、コンピューター上のすべてのアカウントについて、実行中のすべてのアプリのリストを取得できます。 <br /><br />[  **Windows.System.AppDiagnosticInfo**](/uwp/api/windows.system.appdiagnosticinfo) クラス、**Windows.System.AppDiagnosticInfo.RequestAppDiagnosticInfoAsync** クラス、[**Windows.ApplicationModel.AppInfo**](/uwp/api/windows.applicationmodel.appinfo) クラスを使う場合は、この機能が必要になります。 |
| **デバイス ポータル プロバイダー** | **devicePortalProvider** 機能を使うと、アプリは **Windows.System.Diagnostics.DevicePortal** API を呼び出し、開発者モードでの診断ツール用 [Web サーバーとして](../debug-test-perf/device-portal-plugin.md) 機能することができます。<br /><br />Microsoft Store に申請するアプリケーションでこの機能を宣言することはお勧めしません。 ほとんどの場合、この機能の使用は承認されません。 |
| **エンタープライズ クラウド シングル サインオン** | **enterpriseCloudSSO** 機能を使用すると、アプリはホスト型 Web 表示コントロール内で Azure Active Director (AAD) リソースによってシングル サインオンを使用できます。 |
| **VoIP 通話の自動受信** | **backgroundVoIP** 機能を使用すると、電話を受けるようにユーザーに明示的に求めることなく、VoIP の着信を自動的に受信し、受け入れることができます。 この機能を利用するアプリは、カメラとマイクのフル制御が許可され、これらのリソースをバックグラウンドで使用することができます。<br /><br />Microsoft Store に申請するアプリでこの機能を宣言することはお勧めしません。 ほとんどの開発者に対して、この機能の使用は承認されません。 |
| **VoIP 通話用リソースの予約** | **oneProcessVoIP** 機能を使用すると、単一プロセス アプリケーションで VoIP 通話に必要な CPU とメモリを予約できます。<br /><br />Microsoft Store に申請するアプリでこの機能を宣言することはお勧めしません。 ほとんどの開発者に対して、この機能の使用は承認されません。 |
| **開発モード ネットワーク** | **developmentModeNetwork** 機能を使用すると、アプリは、C++/CX UWP アプリ、または Windows ランタイム コンポーネントで OpenFile Win32 API を呼び出すときに、サインインしているユーザーの資格情報を使用してネットワーク パスにアクセスできます。 <br /><br />Microsoft Store に申請するアプリケーションでこの機能を宣言することはお勧めしません。 ほとんどの場合、この機能の使用は承認されません。 |
| **ファイル システムへの幅広いアクセス** | **broadFileSystemAccess** 機能を使用すると、実行時にファイル ピッカー スタイルのプロンプトを追加使用しなくても、アプリはファイル システムに対して、アプリを実行中のユーザーと同じアクセス許可を獲得できます。 ユーザーが FilePicker または FolderPicker を使用して既に選択したファイルにアクセスする場合、この機能は必要ないことに注意してください。<br/><br/>この機能は、[Windows.Storage](/uwp/api/windows.storage) API で動作します。 ユーザーは [設定] でアクセス許可をいつでも付与または拒否できるため、アプリがこれらの変更に対する回復性を備えていることを確認する必要があります。 2018 年 4 月の更新プログラムでは、アクセス許可の既定値はオンです。 2018 の年 10 月の更新プログラムでは、既定値はオフです。 さらに、この機能では**ドキュメント**、**ピクチャ**、**ビデオ**などの特殊なフォルダー機能を宣言できない点にも注意してください。 アプリでこの機能を有効にするには、**broadFileSystemAccess** をマニフェストに追加します。 例については、記事「[ファイル アクセス許可](../files/file-access-permissions.md)」を参照してください。<br/><br/>**注:** この機能は、Xbox ではサポートされていません。 |
| **システム ファームウェアと BIOS** | **smbios** 機能を使うと、アプリは BIOS データとシステム ファームウェア データにアクセスできます。 |
| **完全な信頼のアクセス許可レベル** | 制限付き機能 **runFullTrust** を使用すると、アプリは、ユーザーのマシンで、完全な信頼のアクセス許可レベルで実行できます。 [FullTrustProcessLauncher](/uwp/api/windows.applicationmodel.fulltrustprocesslauncher) API を使用する場合は、この機能が必要になります。<br /><br />また、appx または .msix パッケージとして提供されるすべてのデスクトップ アプリケーションにも必要です ([デスクトップ ブリッジ](https://developer.microsoft.com/windows/bridges/desktop)と同様)。Desktop App Converter (DAC) または Visual Studio を使用してこれらのアプリをパッケージ化すると、これはマニフェストに自動的に表示されます。 |
| **昇格** | 制限付き機能 **allowElevation** を使用すると、Microsoft パートナーまたは企業によって作成されたアプリは、起動時またはアプリの有効期間中に自動昇格を要求する既存のデスクトップ機能を保持できます。<br/><br/>Microsoft Store に申請するアプリケーションでこの機能を宣言することはお勧めしません。 ほとんどの場合、この機能の使用は承認されません。 これは、企業がビジネス向け Microsoft Store を通じてプライベート ストアに展開した基幹業務アプリについてのみ承認されます。  |
| **Windows チームのデバイスの資格情報** | 制限付き機能 **teamEditionDeviceCredential** を使用すると、アプリは、Windows 10 のバージョン 1703 以降を実行している Surface Hub デバイスでデバイス アカウントの資格情報を要求する API にアクセスできます。<br/><br/>Microsoft Store に申請するアプリケーションでこの機能を宣言することはお勧めしません。 ほとんどの場合、この機能の使用は承認されません。 |
| **Windows チームのアプリケーション ビュー** | 制限付き機能 **teamEditionView** を使用すると、アプリは、Windows 10 のバージョン 1703 以降を実行している Surface Hub デバイスでアプリケーション ビューをホストするための API にアクセスできます。<br/><br/>Microsoft Store に申請するアプリケーションでこの機能を宣言することはお勧めしません。 ほとんどの場合、この機能の使用は承認されません。 |
| **カメラ処理拡張機能** | 制限付き機能 **cameraProcessingExtension** を使用すると、アプリは、カメラを直接制御せずに、カメラからキャプチャされた画像を処理できます。<br /><br />[Windows.Devices.PointOfService.Provider](/uwp/api/windows.devices.pointofservice.provider) 名前空間の API を呼び出す場合は、この機能が必要になります。<br /><br />この機能は、ストア申請用に誰でもアクセスを要求できます。 |
| **データ使用状況の管理** | 制限付き機能 **networkDataUsageManagement** を使用すると、アプリは、ネットワーク データの使用状況に関する情報を収集できます。<br /><br />[GetAttributedNetworkUsageAsync](/uwp/api/windows.networking.connectivity.connectionprofile.getattributednetworkusageasync) を呼び出す場合は、この機能が必要になります。<br /><br />この機能は、ストア申請用に誰でもアクセスを要求できます。 |
| **電話回線接続の管理** | **phoneLineTransportManagement** 機能を使用すると、アプリは、電話回線接続を担当するシステム デバイスを管理できます。<br /><br />[Windows.ApplicationModel.Calls](/uwp/api/windows.applicationmodel.calls) 名前空間の PhoneLineTransportDevice API を使用する場合は、この機能が必要になります。 |
| **仮想化されないリソース** | 制限付き機能 **unvirtualizedResources** を使用すると、アプリケーションは、パッケージ マニフェストで [RegistryWriteVirtualization](/uwp/schemas/appxpackage/uapmanifestschema/element-desktop6-registrywritevirtualization) 要素と [FileSystemWriteVirtualization](/uwp/schemas/appxpackage/uapmanifestschema/element-desktop6-filesystemwritevirtualization) 要素を宣言して、レジストリおよびファイル システムの仮想化を無効にすることができます。 これらの宣言はそれぞれ HKEY_CURRENT_USER またはユーザーの AppData フォルダーへのすべての書き込みがシステムによって仮想化されるのを防ぎます。 これは、アプリケーションが他のアプリケーションに対して、アプリケーションと同じレジストリまたはファイル システムのエントリを読み取る、または書き込むことを求めるシナリオで役立ちます。<br /><br />この機能は、Microsoft および Microsoft パートナーによって発行される特定の種類のデスクトップ PC ゲーム向けに設計されています。 これは、システムがクリーンにアンインストールする機能を損なう可能性があるため、他のシナリオでは使用しないでください。 |
| **変更可能なアプリ** | 制限付き機能 **modifiableApp** を使用すると、アプリケーションは、パッケージ マニフェストで [windows.mutablePackageDirectories](/uwp/schemas/appxpackage/uapmanifestschema/element-desktop6-package-extension) 拡張機能を宣言できます。 これにより、変更された、または追加されたファイルをアプリケーションで配置する必要があるフォルダーの名前を指定することができます。 OS はこのフォルダーを作成し、アプリケーションによって最初にインストールされたファイルの代わりに (またはファイルに加えて)、このフォルダー内のファイルをアプリケーションで使用できるようにします。<br /><br />この機能は、Microsoft および Microsoft パートナーによって発行される特定の種類のデスクトップ PC ゲーム向けに設計されています。 これは、署名されていないコードの実行を許可する可能性があるため、他のシナリオには使用しないでください。 |
| **パッケージ書き込みリダイレクト互換性 Shim** | 制限付き機能 **packageWriteRedirectionCompatibilityShim** は、すべての新しいファイルをユーザーごとの場所に作成するようにアプリケーションを構成します。 書き込み用に開かれた既存のファイルは、まずユーザーごとの場所にコピーされ、変更は、その場所にあるファイルに対して行われます。 この機能は、アプリケーションがそのインストール フォルダー内でファイルの作成または変更を行う場合に役立ちます。<br /><br />この機能は、Microsoft および Microsoft パートナーによって発行される特定の種類のデスクトップ PC ゲーム向けに設計されています。 ただし、場合によっては他のアプリにも適用される可能性があります。 |
| **カスタム インストール アクション** | 制限付き機能 **customInstallActions** を使用すると、アプリケーションは、パッケージ マニフェストで [windows.customInstall](/uwp/schemas/appxpackage/uapmanifestschema/element-desktop6-package-extension) 拡張機能を宣言できるため、アプリケーションで実行される 1 つ以上の追加のインストーラー ファイル (.exe または .msi) を指定できるようになります。 これにより、標準的な展開シナリオでカスタム アクション (インストール、更新、修復、またはアンインストール) を指定できるようになります。 たとえば、これは、アプリケーションがサードパーティ製の再頒布可能コンポーネントをバンドルする場合に役立ちます。<br /><br />この機能は、Microsoft および Microsoft パートナーによって発行される特定の種類のデスクトップ PC ゲーム向けに設計されています。 他のシナリオでは使用しないでください。 |
| **パッケージ サービス** | 制限付き機能 **packagedServices** を使用すると、Microsoft および企業で作成されたアプリケーションは、パッケージ マニフェストで [windows.service](/uwp/schemas/appxpackage/uapmanifestschema/element-desktop6-extension) 拡張機能を宣言できるので、アプリケーションとともに 1 つ以上のサービスをインストールできるようになります。 これらのサービスを、ローカル サービス、ネットワーク サービスまたはローカル システムのアカウントで実行されるように構成することができます。 ローカル サービスおよびネットワーク サービスには、**packagedServices** 機能のみが必要です。 ローカル システム サービスには、**packagedServices** 機能と **localSystemServices** 機能の両方が必要です。<br /><br />Microsoft Store に申請するアプリケーションでこの機能を宣言することはお勧めしません。 ほとんどの場合、この機能の使用は承認されません。  |
| **ローカル システム サービス** | 制限付き機能 **localSystemServices** を使用すると、Microsoft パートナーおよび企業で作成されたアプリケーションは、アプリケーションとともに 1 つ以上のローカル システム サービスをインストールできます (つまり、アプリケーションは、サービスの StartAccount を LocalSystem として宣言できます)。 このシナリオでは、**packagesServices** 機能も必要です。 <br /><br />Microsoft Store に申請するアプリケーションでこの機能を宣言することはお勧めしません。 ほとんどの場合、この機能の使用は承認されません。 |
| **バックグラウンドでの空間認識** | 制限付き機能 **backgroundSpatialPerception** を使用すると、アプリケーションは、バックグラウンドでアプリを実行しながら、ユーザーの頭や手、モーション コントローラー、その他の追跡オブジェクトにアクセスできます。 |

## <a name="custom-capabilities"></a>カスタム機能

カスタム機能の使用の承認を要求するには、上記の「[制限付き機能](#restricted-capabilities)」セクションで説明した機能の承認プロセスと同じプロセスを使用できます。 [埋め込み SIM](/uwp/api/windows.networking.networkoperators.esim) API は、カスタム機能を要求する API の例です。 アプリケーションを開発者モードでローカルでのみ実行する場合は、カスタム機能は必要ありません。 しかし、アプリを Microsoft Store に発行する場合、または開発者モード以外で実行する場合は、カスタム機能が必要です。

Windows テクニカル アカウント マネージャー (TAM) がいる場合、TAM と協力してアクセスを要求できます。 詳細については、「[Microsoft TAM に問い合わせてください](/windows-hardware/drivers/mobilebroadband/testing-your-desktop-cosa-apn-database-submission#contact-your-microsoft-tam)」を参照してください。

カスタム機能を宣言するには、[アプリケーション パッケージ マニフェスト](/uwp/schemas/appxpackage/appx-package-manifest) ソース ファイル (`Package.appxmanifest`) を変更します。 **xmlns:uap4** XML 名前空間宣言を追加し、カスタム機能を宣言する際に、**uap4** プレフィックスを使用します。 次に例を示します。

```xml
<?xml version="1.0" encoding="utf-8"?>
<Package
    ...
    xmlns:uap4="http://schemas.microsoft.com/appx/manifest/uap/windows10/4">
...
<Capabilities>
    <uap4:CustomCapability Name="CompanyName.customCapabilityName_PublisherID"/>
</Capabilities>
</Package>
```

> [!NOTE]
> すべての**CustomCapability** 要素は、パッケージ マニフェストの **Capabilities** ノードの下で、すべての **Capability** 要素の後、およびすべての[DeviceCapability](#device-capabilities) 要素の前に指定する必要があります。

## <a name="related-topics"></a>関連トピック

* [申請オプション](../publish/manage-submission-options.md)
* [パッケージ マニフェストで機能を指定する方法](/uwp/schemas/appxpackage/how-to-specify-capabilities-in-a-package-manifest)
* [パッケージ マニフェストでデバイス機能を指定する方法](/uwp/schemas/appxpackage/how-to-specify-device-capabilities-in-a-package-manifest)