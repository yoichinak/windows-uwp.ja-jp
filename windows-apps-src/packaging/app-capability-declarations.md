---
ms.assetid: 25B18BA5-E584-4537-9F19-BB2C8C52DFE1
title: アプリ機能の宣言
description: Windows アプリのパッケージマニフェストで機能を宣言して、特定の API や、カメラやマイクなどの画像、音楽、デバイスなどのリソースにアクセスできるようにする必要があります。
ms.date: 04/19/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 167b388130ea02cc0613dc9638e93f0cde38eb46
ms.sourcegitcommit: 860e8303da3e36624f603ce3273d68911c696e5d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/22/2019
ms.locfileid: "72777283"
---
# <a name="app-capability-declarations"></a>アプリ機能の宣言

Windows アプリの[パッケージマニフェスト](https://docs.microsoft.com/uwp/schemas/appxpackage/appx-package-manifest)で機能を宣言して、特定の Windows 10 api またはリソース (画像、音楽、カメラやマイクなどのデバイスなど) にアクセスできるようにする必要があります。 UWP アプリと、Windows 10 用の MSIX または AppX パッケージにパッケージ化されているその他の種類のデスクトップアプリでは、機能が使用されます。

特定のリソースまたは API へのアクセスを要求するには、アプリの [パッケージ マニフェスト](https://docs.microsoft.com/uwp/schemas/appxpackage/appx-package-manifest) で機能を宣言します。 一般的な機能は、Visual Studio の[マニフェストデザイナー](/windows/msix/package/packaging-uwp-apps#configure-an-app-package)を使用して宣言することも、手動で追加することもできます。 詳しくは、「[パッケージ マニフェストで機能を指定する方法](https://docs.microsoft.com/uwp/schemas/appxpackage/how-to-specify-capabilities-in-a-package-manifest)」をご覧ください。 ユーザーがストアからアプリを入手するときに、アプリで宣言されているすべての機能が通知されることを知っておくのは重要です。 アプリに不要な機能を宣言しないでください。

一部の機能では、アプリが*機密性の高いリソース*にアクセスできます。 ユーザーの個人データにアクセスしたり、ユーザーに課金したりできるため、これらのリソースは機密性の高いリソースと見なされます。 設定アプリで管理されるプライバシー設定で、機密性の高いリソースへのアクセスを動的に制御することができます。 したがって、機密性の高いリソースが常に利用できるとアプリで認識されないことが重要です。 機密性の高いリソースへのアクセスについて詳しくは、「[個人データにアクセスするアプリのガイドライン](https://docs.microsoft.com/windows/uwp/security/index)」をご覧ください。 *機密性の高いリソース*にアクセスできるアプリを提供する機能は、機能シナリオの横にアスタリスク (\*) で注釈が付けられます。

機能にはいくつかの種類があります。

- 一般的なアプリのシナリオに適用される[汎用使用機能](#general-use-capabilities)。
- [デバイスの機能](#device-capabilities)。アプリが周辺機器と内部デバイスにアクセスできるようにします。
- Microsoft Store の送信またはその両方の承認を必要とする[制限付き機能](#restricted-capabilities)は、一般に Microsoft と特定のパートナーのみが利用できます。
- [カスタム機能](#custom-capabilities)。

## <a name="general-use-capabilities"></a>一般的な用途の機能

汎用使用機能は、アプリケーションパッケージマニフェストの**機能**要素を使用して指定します。 これらの機能は、最も一般的なアプリのシナリオに適用されます。

> [!NOTE]
> すべての**機能**要素は、パッケージマニフェストの [機能] ノードの下にある[Customcapability](#custom-capabilities)および[DeviceCapability](#device-capabilities)要素の前に指定する必要があります。

| 機能のシナリオ | 機能の使用法 |
|---------------------|------------------|
| **音楽**\* | **musicLibrary** 機能は、ユーザーの音楽ライブラリへのプログラムによるアクセスを提供します。これによって、ユーザーの操作なしで、ライブラリ内のすべてのファイルを列挙してそれらのファイルにアクセスできます。 通常、この機能は、音楽ライブラリ全体を利用するジュークボックス アプリで使われます。<br /><br />[  **ファイル ピッカー**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker)は、アプリで使うファイルをユーザーが開くことができる強力な UI メカニズムを提供します。 **musicLibrary** 機能を宣言するのは、アプリのシナリオでプログラムによるアクセスが必要であるため、**ファイル ピッカー**では実現できない場合だけにしてください。<br /><br /> アプリのパッケージ マニフェストで宣言するとき、以下に示すように、**musicLibrary** 機能に **uap** 名前空間を含める必要があります。 <br /><br />```<Capabilities><uap:Capability Name="musicLibrary"/></Capabilities>```
| **画像**\* | **picturesLibrary** 機能は、ユーザーの画像へのプログラムによるアクセスを提供します。これによって、ユーザーの操作なしで、ライブラリ内のすべてのファイルを列挙してそれらのファイルにアクセスできます。 通常、この機能は、画像ライブラリ全体を利用する写真再生アプリで使われます。<br /><br />[  **ファイル ピッカー**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker)は、アプリで使うファイルをユーザーが開くことができる強力な UI メカニズムを提供します。 **picturesLibrary** 機能を宣言するのは、アプリのシナリオでプログラムによるアクセスが必要であるため、**ファイル ピッカー**では実現できない場合だけにしてください。<br /><br />アプリのパッケージ マニフェストで宣言するとき、以下に示すように、**picturesLibrary** 機能に **uap** 名前空間を含める必要があります。<br /><br />```<Capabilities><uap:Capability Name="picturesLibrary"/></Capabilities>```
| **ビデオ**\* | **videosLibrary** 機能は、ユーザーのビデオへのプログラムによるアクセスを提供します。これによって、ユーザーの操作なしで、ライブラリ内のすべてのファイルを列挙してそれらのファイルにアクセスできます。 通常、この機能は、ビデオ ライブラリ全体を利用するムービー再生アプリで使われます。<br /><br />[  **ファイル ピッカー**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker)は、アプリで使うファイルをユーザーが開くことができる強力な UI メカニズムを提供します。 **videosLibrary** 機能を宣言するのは、アプリのシナリオでプログラムによるアクセスが必要であるため、**ファイル ピッカー**では実現できない場合だけにしてください。<br /><br />アプリのパッケージ マニフェストで宣言するとき、以下に示すように、**videosLibrary** 機能に **uap** 名前空間を含める必要があります。<br /><br />```<Capabilities><uap:Capability Name="videosLibrary"/></Capabilities>```
| **リムーバブル ストレージ** | **removableStorage** 機能は、パッケージ マニフェストで宣言されたファイルの種類の関連付けに限定された、USB キーや外部ハード ドライブなどのリムーバブル ストレージ上のファイルへのプログラムによるアクセスを提供します。 たとえば、ドキュメント リーダー アプリで .doc ファイルの種類の関連付けを宣言すると、リムーバブル ストレージ デバイス上の .doc ファイルを開くことはできますが、他の種類のファイルを開くことはできません。 ユーザーのリムーバブル ストレージ デバイスにはさまざまな情報が含まれている可能性があり、リムーバブル ストレージにプログラムでアクセスするときは宣言された種類のファイルすべてについて正当な理由が求められるため、この機能を宣言する場合は注意が必要です。<br /><br />ユーザーは、関連付けが宣言されたファイルはアプリで処理されるものと考えます。 そのため、責任を持って処理できないファイルについては、関連付けを宣言しないでください。 [  **ファイル ピッカー**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker)は、アプリで使うファイルをユーザーが開くことができる強力な UI メカニズムを提供します。<br /><br />**removableStorage** 機能を宣言するのは、アプリのシナリオでプログラムによるアクセスが必要であるため、[**ファイル ピッカー**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker)では実現できない場合だけにしてください。<br /><br />アプリのパッケージ マニフェストで宣言するとき、以下に示すように、**removableStorage** 機能に **uap** 名前空間を含める必要があります。<br /><br />```<Capabilities><uap:Capability Name="removableStorage"/></Capabilities>```
| **インターネットとパブリック ネットワーク**\* | インターネットとパブリック ネットワークに対するさまざまなレベルのアクセスを提供する機能は 2 つあります。<br /><br />**internetClient** 機能を使うと、アプリはインターネットから着信データを受信できます。 サーバーとして機能することはできません。 ローカル ネットワーク アクセスはありません。<br />**internetClientServer** 機能を使うと、アプリはインターネットから着信データを受信できます。 サーバーとして機能できます。 ローカル ネットワーク アクセスはありません。<br /><br />Web サービス コンポーネントを持つほとんどのアプリで **internetClient** を使います。 着信ネットワーク接続をリッスンする必要があるピア ツー ピア (P2P) シナリオを実現するアプリは **internetClientServer** を使う必要があります。 **internetClientServer** 機能には **internetClient** 機能で提供されるアクセスが含まれるため、**internetClientServer** を指定した場合は **internetClient** を指定する必要はありません。
| **ホーム ネットワークと社内ネットワーク**\* | **privateNetworkClientServer** 機能は、ファイアウォール経由でのホーム ネットワークおよび社内ネットワークへの入力方向および出力方向のアクセスを提供します。 通常、この機能は、ローカル エリア ネットワーク (LAN) 上で通信するゲーム、およびさまざまなローカル デバイスでデータを共有するアプリで使われます。 アプリで **musicLibrary**、**picturesLibrary**、または **videosLibrary** を指定している場合は、ホーム グループ内の対応するライブラリにアクセスするためにこの機能を使う必要はありません。 Windows の場合、この機能はインターネットへのアクセスを提供しません。
| **予定** | **appointments** 機能は、ユーザーの予定ストアへのアクセスを提供します。 この機能によって、同期されたネットワーク アカウントから取得された予定や、予定ストアへの書き込みを行う他のアプリに対する読み取りアクセスが可能になります。 この機能を使うと、アプリで新しいカレンダーを作り、そのカレンダーに予定を書き込むことができます。<br /><br />アプリのパッケージ マニフェストで宣言するとき、以下に示すように、**appointments** 機能に **uap** 名前空間を含める必要があります。<br /><br />```<Capabilities><uap:Capability Name="appointments"/></Capabilities>```
| **連絡先**\* | **contacts** 機能は、さまざまな連絡先ストアからの連絡先が集約されたビューへのアクセスを提供します。 この機能によって、アプリでは、さまざまなネットワークやローカルの連絡先ストアから同期された連絡先に制限付きでアクセスできます (ネットワーク許可規則が適用されます)。<br /><br />アプリのパッケージ マニフェストで宣言するとき、以下に示すように、**contacts** 機能に **uap** 名前空間を含める必要があります。<br /><br />```<Capabilities><uap:Capability Name="contacts"/></Capabilities>```
| **コード生成** | **codeGeneration** 機能を使うと、アプリは JIT が可能になる次の機能にアクセスできます。<br /><br />[**VirtualProtectFromApp**](https://docs.microsoft.com/windows/desktop/api/memoryapi/nf-memoryapi-virtualprotectfromapp)<br />[**CreateFileMappingFromApp**](https://docs.microsoft.com/windows/desktop/api/memoryapi/nf-memoryapi-createfilemappingfromapp)<br />[**OpenFileMappingFromApp**](https://docs.microsoft.com/windows/desktop/api/memoryapi/nf-memoryapi-openfilemappingfromapp)<br />[**MapViewOfFileFromApp**](https://docs.microsoft.com/windows/desktop/api/memoryapi/nf-memoryapi-mapviewoffilefromapp)
| **AllJoyn** | **allJoyn** 機能を使うと、ネットワーク上の AllJoyn 対応のアプリやデバイスは、相互に検出を行い、対話することができます。<br /><br />[  **Windows.Devices.AllJoyn**](https://docs.microsoft.com/uwp/api/Windows.Devices.AllJoyn) 名前空間の API にアクセスするすべてのアプリは、この機能を使う必要があります。
| **通話** | **phoneCall** 機能を使うと、アプリは、デバイスのすべての電話回線にアクセスし、次の機能を実行できます。<ul><li>ユーザーに確認を求めずに、電話回線での通話を実行してシステムのダイヤラーを表示します。</li><li>電話回線に関連するメタデータへアクセスします。</li><li>電話回線に関連するトリガーへアクセスします。</li><li>ユーザーが選んだスパム フィルター アプリで、禁止一覧と呼び出し元の情報を設定し、確認することができます。</li></ul>アプリのパッケージ マニフェストで宣言するとき、以下に示すように、**phoneCall** 機能に **uap** 名前空間を含める必要があります。<br /><br />```<Capabilities><uap:Capability Name="phoneCall"/></Capabilities>```<br /><br /><b>PhoneCallHistoryPublic</b>機能を使用すると、アプリはデバイス上の携帯電話や VoIP 通話履歴情報を読み取ることができます。 この機能により、アプリは VoIP 通話履歴エントリを書き込むこともできます。 この機能は、<a href="https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Calls.PhoneCallHistoryStore"><b>PhoneCallHistoryStore</b></a> クラスのすべてのメンバーへのアクセスに必要です。
| **通話が録音されているフォルダー**\* | **recordedCallsFolder** デバイス機能を使うと、アプリは通話が録音されているフォルダーにアクセスできます。<br /><br />アプリのパッケージ マニフェストで宣言するとき、以下に示すように、**recordedCallsFolder** 機能に **mobile** 名前空間を含める必要があります。<br /><br />```<Capabilities><mobile:Capability Name="recordedCallsFolder"/></Capabilities>```
| **ユーザー アカウント情報**\* | **userAccountInformation** 機能を使うと、アプリはユーザーの名前と画像にアクセスできるようになります。<br /><br />[  **Windows.System.UserProfile**](https://docs.microsoft.com/uwp/api/Windows.System.UserProfile) 名前空間の一部の API にアクセスする場合は、この機能が必要になります。<br /><br />アプリのパッケージ マニフェストで宣言するとき、以下に示すように、**userAccountInformation** 機能に **uap** 名前空間を含める必要があります。<br /><br />```<Capabilities><uap:Capability Name="userAccountInformation"/></Capabilities>```
| **VoIP 通話** | **Voipcall**機能を使用すると、アプリケーションは、名前空間を呼び出すことによって、VoIP 呼び出し api にアクセスできます[ **。** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Calls)<br /><br />アプリのパッケージ マニフェストで宣言するとき、以下に示すように、**voipCall** 機能に **uap** 名前空間を含める必要があります。<br /><br />```<Capabilities><uap:Capability Name="voipCall"/></Capabilities>```
| **3D オブジェクト** | **objects3D** 機能を使用すると、アプリは 3D オブジェクト ファイルにプログラムでアクセスできます。 通常、この機能は、3D オブジェクト ライブラリ全体にアクセスする必要がある 3D アプリやゲームで使用されます。<br /><br />[  **Windows.Storage**](https://docs.microsoft.com/uwp/api/Windows.Storage) 名前空間の API を使って 3D オブジェクトを含むフォルダーにアクセスする場合は、この機能が必要になります。<br /><br />アプリのパッケージ マニフェストで宣言するとき、以下に示すように、**objects3D** 機能に **uap** 名前空間を含める必要があります。<br /><br />```<Capabilities><uap:Capability Name="objects3D"/></Capabilities>```
| **ブロックされているメッセージの読み取り**\* | **blockedChatMessages** 機能を使うと、アプリはスパム フィルター アプリでブロックされた SMS メッセージや MMS メッセージを読み取ることができます。<br /><br />[  **Windows.ApplicationModel.Chat**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Chat) 名前空間の API を使ってブロックされたメッセージにアクセスする場合は、この機能が必要になります。<br /><br />アプリのパッケージ マニフェストで宣言するとき、以下に示すように、**blockedChatMessages** 機能に **uap** 名前空間を含める必要があります。<br /><br />```<Capabilities><uap:Capability Name="blockedChatMessages"/></Capabilities>```
| **カスタムデバイス** | **Lowleveldevices**機能は、追加の要件が多数満たされている場合に、アプリがカスタムデバイスにアクセスできるようにします。 この機能を**lowLevel**デバイスの機能と混同しないでください。これにより、GPIO、I2C、SPI、および PWM デバイスにアクセスできるようになります。<br /><br /> [デバイスインターフェイス](https://docs.microsoft.com/windows-hardware/drivers/install/device-interface-classes)を公開するカスタムドライバーを開発し、このデバイスへのハンドルを開き、ioctl を送信する場合は、次の操作を行う必要があります。 <ul><li>アプリケーションマニフェストで**Lowleveldevices**機能を有効にします。 ```<Capabilities><iot:Capability Name="lowLevelDevices"/></Capabilities>```</li><li>[埋め込みモード](https://docs.microsoft.com/windows/iot-core/develop-your-app/EmbeddedMode)を有効にする</li><li>[INF](https://docs.microsoft.com/previous-versions/windows/desktop/deviceaccess/register-the-device-interface-class-as-privileged)で、またはドライバーで[Wdfdevice割り当て interfaceproperty ()](https://docs.microsoft.com/windows-hardware/drivers/ddi/content/wdfdevice/nf-wdfdevice-wdfdeviceassigninterfaceproperty)を呼び出して、デバイスインターフェイスを[制限付き](https://docs.microsoft.com/windows-hardware/drivers/install/devpkey-deviceinterface-restricted)としてマークします。</ul>次に、 [**Windows のカスタム**](https://docs.microsoft.com/uwp/api/Windows.Devices.Custom.CustomDevice)デバイスを使用して、デバイスへのハンドルを開くことができます。 詳細については、「[内部デバイスの UWP デバイスアプリ](https://docs.microsoft.com/windows-hardware/drivers/devapps/uwp-device-apps-for-specialized-devices)」を参照してください。
| **IoT システム管理** | **systemManagement** 機能を使うと、アプリは基本的なシステム管理者特権 (シャットダウン、再起動、ロケール、タイムゾーンなど) を持つことができます。<br /><br />[  **Windows.System**](https://docs.microsoft.com/uwp/api/Windows.System) 名前空間の一部の API にアクセスする場合は、この機能が必要になります。<br /><br />アプリのパッケージ マニフェストで宣言するとき、以下に示すように、**systemManagement** 機能に **iot** 名前空間を含める必要があります。<br /><br />```<Capabilities><iot:Capability Name="systemManagement"/></Capabilities>```
| **バックグラウンドメディア再生** | **backgroundMediaPlayback** 機能は、[**MediaPlayer**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer) クラスや [**AudioGraph**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph) クラスなど、メディア固有の API の動作を変更して、アプリがバック グラウンドで実行されている間のメディアの再生を有効にします。 アプリがバックグラウンドに移行しても、アクティブなすべてのオーディオ ストリームはミュートせず、音声を発し続けます。 また再生が行われている間はアプリが有効に保たれるように、アプリの有効期間が自動的に延長されます。
| **リモートシステム** | **remoteSystem**機能を使うと、アプリがユーザーの Microsoft アカウントに関連付けられているデバイスの一覧にアクセスできるようになります。 デバイスの一覧へのアクセスは、実行した操作を複数のデバイス間で保持するために不可欠です。 この機能は、次の項目のすべてのメンバーにアクセスするために必要です。<ul><li>[RemoteSystems](https://docs.microsoft.com/uwp/api/windows.system.remotesystems)名前空間</li><li>[RemoteLauncher](https://docs.microsoft.com/uwp/api/Windows.System.RemoteLauncher)クラス</li><li>[AppServiceConnection](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appservice.appserviceconnection.openremoteasync)メソッド</li></ul> |
| **空間認識** | **spatialPerception** 機能は、空間マッピング データにプログラムでアクセスし、ユーザーに近い空間内でアプリケーション指定領域のサーフェスに関する情報を Mixed Reality アプリに提供します。  spatialPerception 機能は、これらのサーフェス メッシュをアプリで明示的に使用する場合にのみ宣言してください。ユーザーの頭部姿勢に基づいて Mixed Reality アプリでホログラフィック レンダリングを実行する場合、この機能は必要ありません。 |
| **グローバルメディアコントロール** | **Globalmediacontrol**機能を使用すると、 [**SystemMediaTransportControls**](https://docs.microsoft.com/uwp/api/Windows.Media.SystemMediaTransportControls)と統合されたシステム全体の再生セッションにアプリがアクセスできるようになり、再生情報が提供され、リモートコントロールが許可されます。 この機能は、 [**Windows**](https://docs.microsoft.com/uwp/api/windows.media.control)の名前空間の一部の api を使用するために必要です。 この機能は、 [uap7: 機能](/uwp/schemas/appxpackage/uapmanifestschema/element-uap7-capability)要素で定義されています。  |

## <a name="device-capabilities"></a>デバイスの機能

デバイス機能を使用すると、アプリは周辺機器と内部デバイスにアクセスできます。 デバイス機能は、アプリケーションパッケージマニフェストの**DeviceCapability**要素を使用して指定します。 この要素は追加の子要素を必要とする場合があり、一部のデバイス機能をパッケージ マニフェストに手動で追加する必要があります。 詳しくは、「[パッケージ マニフェストでデバイス機能を指定する方法](https://docs.microsoft.com/uwp/schemas/appxpackage/how-to-specify-device-capabilities-in-a-package-manifest)」と「[**DeviceCapability スキーマ リファレンス**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-devicecapability)」をご覧ください。

> [!NOTE]
> パッケージマニフェストの**Capabilities**要素には、複数の**DeviceCapability**要素を含めることができます。 すべての**DeviceCapability**要素は、すべての**機能**と[customcapability ビリティ](#custom-capabilities)要素の後に指定する必要があります。

| 機能のシナリオ | 機能の使用法 |
|---------------------|------------------|
| **位置情報**\* | **location** 機能は、位置情報機能へのアクセスを提供します。この情報は PC が備えている GPS センサーなどの専用ハードウェアや、利用可能なネットワーク情報から取得されます。 アプリは、ユーザーが **[設定]** チャームで位置情報サービスを無効にした場合に対応する必要があります。 |
| **マイク** | **microphone** 機能は、マイクのオーディオ フィードへのアクセスを提供します。これによって、接続されたマイクからオーディオを録音できます。 アプリは、ユーザーが **[設定]** チャームでマイクを無効にした場合に対応する必要があります。 |
| **近く** | **proximity** 機能を使うと、きわめて近い場所にある複数のデバイスが相互に通信できます。 通常、この機能は、カジュアルなマルチプレーヤー ゲームや情報を交換するアプリで使われます。 デバイスは、Bluetooth、Wi-Fi、インターネットを含む、最適な接続を提供する通信テクノロジを使います。 この機能は、デバイス間の通信を開始するためにのみ使われます。 |
| **カメラ** | **webcam** 機能は、内蔵カメラや外付け Web カメラのビデオ フィードへのアクセスを提供します。これによって、アプリで写真やビデオをキャプチャできます。 Windows の場合、アプリはユーザーが **[設定]** チャームでカメラを無効にした場合に対応する必要があります。<br/>**webcam** 機能では、ビデオ ストリームへのアクセスだけが許可されます。 オーディオ ストリームへのアクセスも許可するには、**microphone** 機能を追加する必要があります。 |
| **USB** | **usb** デバイス機能を使うと、「[USB デバイスのアプリ マニフェスト パッケージの更新](https://go.microsoft.com/fwlink/p/?LinkId=302259)」で API にアクセスできます。 |
| **ヒューマンインターフェイスデバイス (HID)** | **humaninterfacedevice** デバイス機能を使うと、「[HID のデバイス機能を指定する方法](https://docs.microsoft.com/uwp/schemas/appxpackage/how-to-specify-device-capabilities-for-hid)」の API にアクセスできます。 |
| **サービスのポイント (POS)** | **pointOfService** デバイス機能を使うと、[**Windows.Devices.PointOfService**](https://docs.microsoft.com/uwp/api/Windows.Devices.PointOfService) 名前空間の API にアクセスできます。 この名前空間により、アプリは、Point of Service (POS) バー コード スキャナーや磁気ストライプ リーダーにアクセスできます。 この名前空間は、さまざまな製造元の POS デバイスに UWP アプリからアクセスするための、ベンダーに依存しないインターフェイスを提供します。 |
| **Bluetooth** | **bluetooth** デバイス機能を使うと、アプリは Generic Attribute (GATT) または Classic Basic Rate (RFCOMM) プロトコル経由で既にペアリングされている Bluetooth デバイスと通信できます。<br/>[  **Windows.Devices.Bluetooth**](https://docs.microsoft.com/uwp/api/Windows.Devices.Bluetooth) 名前空間の一部の API を使う場合は、この機能が必要になります。 |
| **Wi-fi ネットワーク** | **wiFiControl** デバイス機能を使うと、アプリはスキャンを実行して、Wi-Fi ネットワークに接続することができます。<br/>[  **Windows.Devices.WiFi**](https://docs.microsoft.com/uwp/api/Windows.Devices.WiFi) 名前空間の一部の API を使う場合は、この機能が必要になります。 |
| **ラジオの状態** | **radios** デバイス機能を使うと、アプリは Wi-Fi 通信と Bluetooth 通信を切り替えることができます。<br/>[  **Windows.Devices.Radios**](https://docs.microsoft.com/uwp/api/Windows.Devices.Radios) 名前空間の API を使う場合は、この機能が必要になります。  |
| **光学ディスク** | **optical** デバイス機能を使うと、アプリは、CD、DVD、ブルーレイなどの光ディスク ドライブの機能にアクセスできます。<br/>[  **Windows.Devices.Custom**](https://docs.microsoft.com/uwp/api/Windows.Devices.Custom) 名前空間の一部の API を使う場合は、この機能が必要になります。 |
| **モーションアクティビティ** | デバイス機能 **activity** を使うと、アプリはデバイスの現在の動きを検出できるようになります。<br/>[  **Windows.Devices.Sensors**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors) 名前空間の一部の API を使う場合は、この機能が必要になります。 |
| **シリアル通信** | **serialcommunication** デバイス機能では Windows.Devices.SerialCommunication 名前空間の API へのアクセスが提供され、Windows アプリはシリアル ポートまたはシリアル ポートのアブストラクションを公開するデバイスと通信できるようになります。 [  **Windows.Devices.SerialCommnication**](https://docs.microsoft.com/uwp/api/windows.devices.serialcommunication) 名前空間の API を使う場合は、この機能が必要になります。 |
| **視線トラッカー** | **gazeInput** 機能を使うと、互換性のある視線追跡デバイスが接続されているときにユーザーがアプリケーション境界内で見ている場所を検出できます。 この機能は、 [**Windows**](https://docs.microsoft.com/en-us/uwp/api/windows.devices.input.preview)の一部の api を使用する場合に必要です。 |
| **GPIO、I2C、SPI、PWM** | **LowLevel**デバイス機能を使用すると、GPIO、I2C、SPI、および PWM デバイスにアクセスできます。 この機能は、次の名前空間内の Api を使用するために必要[**です。** ](https://docs.microsoft.com/uwp/api/windows.devices.gpio)[**Pwm**](https://docs.microsoft.com/uwp/api/windows.devices.pwm)、Windows [ **. デバイス**](https://docs.microsoft.com/uwp/api/windows.devices.i2c). [**Spi**](https://docs.microsoft.com/uwp/api/windows.devices.spi)、windows...。<br /><br />```<Capabilities><DeviceCapability Name="lowLevel"/></Capabilities>``` |


<span id="special-and-restricted-capabilities" />

## <a name="restricted-capabilities"></a>制限付き機能

アプリで制限された機能を宣言する場合は、アプリを Microsoft Store に発行するための承認を受けるために、[アプリの送信プロセス](../publish/app-submissions.md)中に情報を提供する必要があります。 この情報は、送信の [[送信オプション](../publish/manage-submission-options.md#restricted-capabilities)] ページで指定します。これは、アプリが宣言する各制限機能を使用する方法を説明するためのものです。

> [!IMPORTANT]
> 制限付きの機能は、非常に具体的なシナリオを対象としています。 これらの機能は、用途が厳格に制限されており、ストアへの登録に際して追加のポリシーやレビューが適用されます。 制限付きの機能を宣言するアプリは、承認を受けなくてもサイドロードできます。 承認は、これらのアプリを App Store に提出するときのみ必要です。

アプリで本当に必要な場合を除き、これらの制限された機能を宣言しないようにしてください。 これらの機能が必要で適しているものとしては、身元を証明するデジタル証明書をスマート カードに含めるようにユーザーに求める 2 要素認証を使ったバンキング アプリなどがあります。 また、主に企業ユーザー向けに設計されたアプリや、ユーザーのドメイン資格情報がないとアクセスできな企業リソースへのアクセスを必要とするアプリもあります。

制限された機能を宣言するには、[アプリケーションパッケージのマニフェスト](https://docs.microsoft.com/uwp/schemas/appxpackage/appx-package-manifest)ソースファイル (`Package.appxmanifest`) を変更します。 **Xmlns: rescap** XML 名前空間宣言を追加し、制限された機能を宣言するときに**rescap**プレフィックスを使用します。 たとえば、**appCaptureSettings** 機能を宣言する方法を次に示します。

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
> すべての制限付き機能要素は、パッケージマニフェストの [機能] ノードの下にある[Customcapability](#custom-capabilities)要素と[DeviceCapability](#device-capabilities)要素の前に指定する必要があります。

### <a name="restricted-capability-approval-process"></a>制限付き機能の承認プロセス

以前は、機能を使う承認を得るためにサポートに問い合わせる必要がありました。 ここでは、[送信プロセス](../publish/app-submissions.md)の一環として、[パートナーセンター](https://partner.microsoft.com/dashboard)でこの情報を提供できるようになりました。

送信するパッケージをアップロードすると、制限された機能が宣言されているかどうかが検出されます。 検出された場合、製品で各機能が使用されているかどうかに関する詳しい情報を[申請オプション](../publish/manage-submission-options.md#restricted-capabilities)ページで提供する必要があります。 製品が機能を宣言する必要がある理由を理解できるように、必ずできる限り詳しく入力してください。 これにより、申請が認定プロセスを完了するまでの時間がいくらか長くなる可能性があります。

認定プロセス中、テスターが提供された情報を確認し、その申請で機能の使用を承認するかどうかを判断します。 これにより、申請が認定プロセスを完了するまでの時間がいくらか長くなる可能性があります。 機能の使用が承認された場合、アプリの認定プロセスの残りの部分が続行されます。 一般に、アプリの更新プログラムを申請するときに機能の承認プロセスを繰り返す必要はありません (追加の機能を宣言する場合を除く)。

この機能の使用が承認されない場合、送信は認定に失敗し、認定レポートにフィードバックが送信されます。 その後、新しい申請を作成して機能を宣言しないパッケージをアップロードすることを選択できます。または、該当する場合は、機能の使用に関連する問題を解決し、新しい申請で承認をリクエストします。

> [!NOTE]
> 送信でパートナーセンターの開発サンドボックスを使用している場合 (たとえば、Xbox Live と統合されるゲームの場合)、 **[送信オプション]** ページに情報を提供するのではなく、事前に承認を要求する必要があります。 このためには、[Windows 開発者向けサポート ページ](https://developer.microsoft.com/windows/support)にアクセスしてください。 開発者サポートトピックの**ダッシュボードの問題**、問題の種類のアプリの**送信**、および**その他の**サブカテゴリを選択します。 次に、機能の使用方法と、製品に必要な理由について説明します。 必要情報がすべて記載されていない場合、要求が拒否されます。 また別途、追加情報の提供を求められることがあります。 このプロセスには通常 5 営業日以上かかるため、十分前もってリクエストを送信してください。
>
> また、開発サンドボックスを使用しているかどうかにかかわらず、(送信中にこの情報を提供するのではなく) 承認を要求する方法を使用することもできます。申請.

<span id="restricted-and-special-use-capability-list" />

### <a name="restricted-capability-list"></a>制限された機能の一覧

次の表に、制限された機能の一覧を示します。 上記で説明したプロセスに従うことによって、Microsoft Store に提出するアプリでこれらの機能の承認をリクエストすることができます。

> [!IMPORTANT]
> かなり限定された状況を除き、一部の制限付き機能が Microsoft Store に提出されるアプリで承認されることはほとんどありません。 これらの機能は、次の表で言及されています。 Microsoft Store で配布する予定の場合、アプリでこれらの機能を宣言しないことをお勧めします。

| 機能のシナリオ | 機能の使用法 |
|---------------------|------------------|
| **エンタープライズ** | Windows ドメイン資格情報により、ユーザーはそれぞれの資格情報を使ってリモートのリソースにログインし、ユーザー名とパスワードを指定したかのように動作できます。 **Enterpriseauthentication**機能は、通常、企業内のサーバーに接続する基幹業務アプリで使用されます。 <br /><br />インターネット上での汎用通信にはこの機能は不要です。<br /><br />**Enterpriseauthentication**機能は、一般的な基幹業務アプリをサポートすることを目的としています。 企業リソースにアクセスする必要がないアプリでは宣言しないでください。 [  **ファイル ピッカー**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker) は、アプリで使うネットワーク共有上のファイルをユーザーが開くことができる強力な UI メカニズムを提供します。 **Enterpriseauthentication**機能を宣言するのは、アプリのシナリオでプログラムによるアクセスが必要な場合のみです。また、**ファイルピッカー**を使用してアプリケーションを認識することはできません。<br /><br />アプリのパッケージ マニフェストで宣言するとき、以下に示すように、**enterpriseAuthentication** 機能に **uap** 名前空間を含める必要があります。<br /><br />```<Capabilities><uap:Capability Name="enterpriseAuthentication"/></Capabilities>```<br /><br />**EnterpriseDataPolicy**機能を使用すると、アプリが Windows Information Protection ポリシー (たとえば、モバイルデバイス管理とモバイルアプリケーション管理システム) で管理されている場合に、アプリが企業データを個別に、または安全に処理できるようになります。  次に示すように、この制限付き機能を宣言します。 <br /><br />```<Capabilities><rescap:Capability Name="enterpriseDataPolicy"/></Capabilities>```<br /><br />この機能は、次のクラスのすべてのメンバーを使うために必要です。<ul><li><a href="https://docs.microsoft.com/uwp/api/Windows.Security.EnterpriseData.FileProtectionManager">FileProtectionManager</a></li><li><a href="https://docs.microsoft.com/uwp/api/Windows.Security.EnterpriseData.DataProtectionManager">Microsoft.systemcenter.dataprotectionmanager.2012.reporting.mp</a></li><li><a href="https://docs.microsoft.com/uwp/api/Windows.Security.EnterpriseData.ProtectionPolicyManager">ProtectionPolicyManager</a></li></ul> |
| **共有ユーザー証明書** | **Sharedusercertificates**機能を使用すると、アプリは、スマートカードに格納されている証明書など、共有ユーザーストアにソフトウェアおよびハードウェアベースの証明書を追加してアクセスできます。 通常、この機能は、認証にスマート カードを必要とする財務アプリまたはエンタープライズ アプリで使われます。<br /><br />アプリのパッケージ マニフェストで宣言するとき、以下に示すように、**sharedUserCertificates** 機能に **uap** 名前空間を含める必要があります。<br /><br />```<Capabilities><uap:Capability Name="sharedUserCertificates"/></Capabilities>``` |
|**ドキュメント**\* | ドキュメント**ライブラリ**機能を使用すると、パッケージマニフェストで宣言されたファイルの種類の関連付けにフィルターを適用して、ユーザーの Documents フォルダーにプログラムでアクセスできます。 たとえば、ワードプロセッシングアプリで .doc ファイルの種類の関連付けが宣言されている場合、ユーザーの Documents フォルダー内の .doc ファイルを開くことができます。 <br /><br />一般に、アプリでは、次のいずれかの Api を使用して、ユーザーがファイルの場所を選択できるようにする必要があります。<ul><li>[Fileopenpicker](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker)を開き、既存のファイルを開きます。</li><li>[FileSavePicker](https://docs.microsoft.com/uwp/api/windows.storage.pickers.filesavepicker)新しいファイルを保存します。</li><li>フォルダーを選択して、追加のファイルを開く、または保存するフォルダーを選択するための[Folderpicker](https://docs.microsoft.com/uwp/api/windows.storage.pickers.folderpicker) 。</li></ul>これらの Api を使用すると、クラウドに同期されたアカウント (OneDrive など) など、最適な場所をユーザーが選択できます。 ユーザーがこれらの Api を使用してファイルまたはフォルダーを選択すると、アプリは[FutureAccessList](https://docs.microsoft.com/uwp/api/windows.storage.accesscache.storageapplicationpermissions.futureaccesslist) api を使用して、その場所に継続的にアクセスできるようになります。 この API を使用すると、アプリは今後、ファイルまたはフォルダーの選択を求められることなく、ファイルまたはフォルダーにアクセスできます。<br /><br />既存のワークフローで、ファイルが Documents フォルダー (たとえば、既存のデスクトップアプリケーションとの相互運用) に含まれる場合や、ユーザーが場所を選択する必要がない場合は、次のようにドキュメント**ライブラリ**機能を宣言します。アプリケーション。 アプリケーションで**ドキュメントライブラリ**機能を使用する場合は、ユーザーが場所を手動で選択できるようにすることもお勧めします。<br /><br />アプリのパッケージ マニフェストで宣言するとき、以下に示すように、**documentsLibrary** 機能に **uap** 名前空間を含める必要があります。<br /><br />```<Capabilities><uap:Capability Name="documentsLibrary"/></Capabilities>``` |
| **ゲーム DVR 設定** | 制限付き機能 **appCaptureSettings** を使うと、アプリはゲーム DVR のユーザー設定を制御できます。<br /><br />[  **Windows.Media.Capture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture) 名前空間の一部の API を使う場合は、この機能が必要になります。 <br /><br />Microsoft Store に送信するアプリケーションでこの機能を宣言することはお勧めしません。 ほとんどの場合、この機能の使用は承認されません。  |
| **携帯ネットワーク** | 制限された機能 **cellularDeviceControl** を使うと、アプリは携帯デバイスを制御できます。<br /><br />**cellularDeviceIdentity** 機能を使うと、アプリは携帯デバイスの ID データにアクセスできます。<br /><br />**cellularMessaging** 機能を使うと、アプリは SMS と RCS を利用できます。<br /><br />[  **Windows.Devices.Sms**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sms) 名前空間の一部の API を使う場合は、これらの機能が必要になります。  |
| **デバイスのロック解除** | 制限された機能 **deviceUnlock** を使うと、アプリは、開発者サイドローディングのシナリオやエンタープライズ サイドローディングのシナリオ向けにデバイスをロック解除できます。<br /><br /> Microsoft Store に送信するアプリケーションでこの機能を宣言することはお勧めしません。 ほとんどの場合、この機能の使用は承認されません。 |
| **デュアル SIM タイル** | 制限された機能 **dualSimTiles** を使うと、アプリは、複数の SIM を備えたデバイスでアプリ一覧の追加のエントリを作成できます。<br /><br />[  **Windows.UI.StartScreen**](https://docs.microsoft.com/uwp/api/Windows.UI.StartScreen) 名前空間の一部の API を使う場合は、この機能が必要になります。 |
| **エンタープライズ共有記憶域** | 制限された機能 **enterpriseDeviceLockdown** を使うと、アプリは、デバイスのロック ダウン API を利用したり、企業で共有している保存フォルダーにアクセスしたりすることができます。 <br /><br />Microsoft Store に送信するアプリケーションでこの機能を宣言することはお勧めしません。 ほとんどの場合、この機能の使用は承認されません。 |
| **システム入力の挿入** | **InputInjectionBrokered**の制限付き機能を使用すると、アプリケーションは、HID、タッチ、ペン、キーボード、マウスなどのさまざまな形式の入力をプログラムによってシステムに挿入できます。 通常、この機能はシステムを制御できる共同作業アプリで使われます。<br /><br />PC の場合、この機能を使ったアプリからの入力の挿入は、同じアプリ コンテナー内のプロセスによってのみ許可されます。<br /><br />```<Capabilities><rescap:Capability Name="inputInjectionBrokered" /></Capabilities>``` |
| **入力の監視**\* | 制限された機能 **inputObservation** を使うと、アプリは、さまざまな形式の未加工入力 (HID、タッチ、ペン、キーボード、マウスなど) が、最終的な宛先に関係なく、システムによって許可されるのを監視できます。  |
| **入力の抑制** | 制限付き機能 **inputSuppression** を使うと、アプリは、さまざまな形式の未加工入力 (HID、タッチ、ペン、キーボード、マウスなど) が、システムによって許可されるのを抑制できます。
| **VPN アプリ** | 制限付き機能 **networkingVpnProvider** を使うと、アプリは VPN 機能 (接続の管理や VPN プラグイン機能の提供など) へのフル アクセスが可能になります。<br /><br />[  **Windows.Networking.Vpn**](https://docs.microsoft.com/uwp/api/Windows.Networking.Vpn) 名前空間の一部の API を使う場合は、この機能が必要になります。 |
| **その他のアプリ管理** | 制限された機能 **packageManagement** を使うと、アプリは他のアプリを直接管理できます。<br /><br />**packageQuery** デバイス機能を使うと、アプリは他のアプリに関する情報を収集できます。<br /><br />[  **PackageManager**](https://docs.microsoft.com/uwp/api/Windows.Management.Deployment.PackageManager) クラスの一部のメソッドとプロパティにアクセスする場合は、これらの機能が必要になります。 |
| **画面の投影** | 制限付き機能 **screenDuplication** を使うと、アプリは画面を別のデバイスに表示できます。<br /><br />DirectX 名前空間の API を使う場合は、この機能が必要になります。 <br /><br />Microsoft Store に送信するアプリケーションでこの機能を宣言することはお勧めしません。 ほとんどの場合、この機能の使用は承認されません。 |
| **ユーザー プリンシパル名** | **UserPrincipalName**の制限付き機能を使用すると、アプリは現在のユーザーのユーザープリンシパル名 (UPN) にアクセスできます。<br /><br />[  **GetUserNameEx**](https://docs.microsoft.com/windows/desktop/api/secext/nf-secext-getusernameexa) 関数を呼び出す場合は、この機能が必要になります。 <br /><br />Microsoft Store に送信するアプリケーションでこの機能を宣言することはお勧めしません。 ほとんどの場合、この機能の使用は承認されません。 |
| **ウォレット** | 制限付き機能 **walletSystem** を使うと、アプリは保存されたウォレット カードへのフル アクセスが可能になります。<br /><br />[  **Windows.ApplicationModel.Wallet.System**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Wallet.System) 名前空間の API を使う場合は、この機能が必要になります。 <br /><br />Microsoft Store に送信するアプリケーションでこの機能を宣言することはお勧めしません。 ほとんどの場合、この機能の使用は承認されません。 |
| **位置情報の履歴** | 制限付き機能 **locationHistory** を使うと、アプリはデバイスの位置情報の履歴にアクセスできます。<br /><br />[  **Windows.Devices.Geolocation**](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation) 名前空間の API を使う場合は、この機能が必要になります。
| **アプリを閉じる確認** | 制限された機能 **confirmAppClose** を使うと、アプリはアプリ自体とアプリ独自のウィンドウを閉じたり、アプリを閉じることを遅延させたりすることができます。<br /><br />アプリは、Windows 10 Version 1703 (ビルド 10.0.15063) 以上でこの機能を要求できます。 以前の Windows 10 バージョンでは、この機能はプライベートであり、"このアプリケーションには、要求された機能を許可できません" というエラー メッセージでアプリのインストールが失敗になります。 |
| **通話履歴**\* | 制限付き機能 **phoneCallHistory** を使うと、アプリは通話履歴を読み取ったり、履歴のエントリを削除できます。<br /><br />[  **Windows.ApplicationModel.Chat**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Chat) 名前空間の API を使う場合は、この機能が必要になります。 <br /><br />Microsoft Store に送信するアプリケーションでこの機能を宣言することはお勧めしません。 ほとんどの場合、この機能の使用は承認されません。 |
| **システム レベルの予定へのアクセス** | 制限された機能 **appointmentsSystem** を使うと、アプリはユーザーのカレンダーにあるすべての予定を読み取ったり、変更したりすることができます。<br /><br />[  **Windows.ApplicationModel.Appointment**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Appointments) 名前空間の API を使う場合は、この機能が必要になります。 <br /><br />Microsoft Store に送信するアプリケーションでこの機能を宣言することはお勧めしません。 ほとんどの場合、この機能の使用は承認されません。 |
| **システム レベルのチャット メッセージへのアクセス**\* | 制限付き機能 **chatSystem** を使うと、アプリはすべての SMS メッセージと MMS メッセージの読み取りと書き込みを実行できます。<br />[  **Windows.ApplicationModel.Chat**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Chat) 名前空間の API を使う場合は、この機能が必要になります。 <br /><br />Microsoft Store に送信するアプリケーションでこの機能を宣言することはお勧めしません。 ほとんどの場合、この機能の使用は承認されません。 |
| **システム レベルの連絡先へのアクセス** | 制限された機能 **contactsSystem** を使うと、アプリは制限付きの連絡先情報や機密性の高い連絡先情報を読み取ったり、既存の連絡先情報を変更したりすることができます。<br /><br />[  **Windows.ApplicationModel.Chat**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Chat) 名前空間の API を使う場合は、この機能が必要になります。 <br /><br />Microsoft Store に送信するアプリケーションでこの機能を宣言することはお勧めしません。 ほとんどの場合、この機能の使用は承認されません。 |
| **電子メールへのアクセス** | 制限付き機能 **email** を使うと、アプリはユーザーのメールの読み取り、トリアージ、送信を実行できます。<br /><br />[  **Windows.ApplicationModel.Email**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Email) 名前空間の API を使う場合は、この機能が必要になります。 <br /><br />Microsoft Store に送信するアプリケーションでこの機能を宣言することはお勧めしません。 ほとんどの場合、この機能の使用は承認されません。 |
| **システム レベルのメールへのアクセス**| 制限された機能 **emailSystem** を使うと、アプリは制限つきのユーザーのメールや機密性の高いユーザーのメールの読み取り、トリアージ、送信を実行できます。 <br /><br />[  **Windows.ApplicationModel.Email**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Email) 名前空間の API を使う場合は、この機能が必要になります。 <br /><br />Microsoft Store に送信するアプリケーションでこの機能を宣言することはお勧めしません。 ほとんどの場合、この機能の使用は承認されません。 |
| **システム レベルの通話履歴へのアクセス** | 制限付き機能 **phoneCallHistorySystem** を使うと、アプリは通話履歴を完全に変更できます (既存のエントリの変更や新しいエントリの作成など)。<br /><br />[  **Windows.ApplicationModel.Calls**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Calls) 名前空間の API を使う場合は、この機能が必要になります。 <br /><br />Microsoft Store に送信するアプリケーションでこの機能を宣言することはお勧めしません。 ほとんどの場合、この機能の使用は承認されません。 |
| **テキスト メッセージの送信**\* | 制限された機能 **smsSend** を使うと、アプリは SMS メッセージや MMS メッセージを送信できます。<br /><br />[  **Windows.ApplicationModel.Chat**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Chat) 名前空間の API を使う場合は、この機能が必要になります。 |
| **システム レベルのすべてのユーザー データへのアクセス** | 制限付き機能 **userDataSystem** を使うと、アプリはシステム データ ストアに保存されているユーザー データへのアクセスが可能になります。 <br /><br />Microsoft Store に送信するアプリケーションでこの機能を宣言することはお勧めしません。 ほとんどの場合、この機能の使用は承認されません。 |
| **ストア プレビュー機能** | 制限付き機能 **previewStore** を使うと、アプリはアプリ内製品の SKU の取得や購入ができます。<br /><br />[  **Windows.ApplicationModel.Store.Preview**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.Preview) 名前空間の特定の API を使う場合は、この機能が必要になります。 |
| **初回サインイン時の設定** | 制限された機能 **firstSignInSettings** を使うと、アプリは、ユーザーが初めてデバイスにサインインしたときに設定されたユーザー設定にアクセスできます。 |
| **Windows チーム エクスペリエンス** | 制限付き機能 **teamEditionExperience** を使うと、アプリは、Windows チーム セッションの多くの経験的側面を制御する内部 API にアクセスできます。 Windows チーム セッションは、Microsoft Surface Hub など、チーム デバイスで実行されている可能性があります。 <br /><br />Microsoft Store に送信するアプリケーションでこの機能を宣言することはお勧めしません。 ほとんどの場合、この機能の使用は承認されません。 |
| **リモート ロック解除** | 制限付き機能 **remotePassportAuthentication** を使うと、アプリは、リモート PC のロック解除に使用される資格情報にアクセスできます。 <br /><br />Microsoft Store に送信するアプリケーションでこの機能を宣言することはお勧めしません。 ほとんどの場合、この機能の使用は承認されません。 |
| **コンポジションのプレビュー** | 制限された機能 **previewUiComposition** を使うと、アプリはユーザー インターフェイスの [**Windows.UI.Composition**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition) 名前空間をプレビューすることで、完成前に API に関するフィードバックを提供できます。 詳細については、wincomposition@microsoft.com にお問い合わせください。 |
| **安全な評価のためのロックダウン** | 制限された機能 **secureAssessment** を使うと、アプリは安全な評価のために単一アプリ モードに Windows をロックダウンできます。 <br /><br />Microsoft Store に送信するアプリケーションでこの機能を宣言することはお勧めしません。 ほとんどの場合、この機能の使用は承認されません。 |
| **接続マネージャーのプロビジョニング** | 制限付き機能 **networkConnectionManagerProvisioning** を使うと、アプリは、デバイスを WWAN および WLAN インターフェイスに接続するポリシーを定義できます。 この機能を使うアプリは、携帯電話会社が作成し、モバイル ネットワークへのデバイス接続を管理します。 |
| **データ通信プランのプロビジョニング** | 制限付き機能 **networkDataPlanProvisioning** を使うと、アプリは、デバイスのデータ プランに関する情報を収集し、ネットワーク使用状況を読み取れます。 この機能を使うアプリは、携帯電話会社が作成し、ユーザーの実際のデータ使用量を OS データ使用量の設定に統合します。 |
| **ソフトウェア ライセンス** | 制限された機能 **slapiQueryLicenseValue** を使うと、アプリは、ソフトウェア ライセンス ポリシーにクエリを実行できます。 <br /><br />Microsoft Store に送信するアプリケーションでこの機能を宣言することはお勧めしません。 ほとんどの場合、この機能の使用は承認されません。 |
| **延長実行** | 制限付き機能 **extendedBackgroundTaskTime** を使うと、バックグラウンド タスクが実行時間制限によって取り消されたり停止されたりすることがなくなります。 ただし、この場合も、他のメモリ使用制限や電力使用制限は、すべてバックグラウンド タスクに適用されます。 この機能は、バッテリーの使用やプライバシーなどのバックグラウンド アプリ設定を使用して制限できます。 ユーザーと管理者がグループ ポリシー設定を使うとバックグラウンド タスクを制御できる点に注意してください。<br /><br />制限された機能 **extendedExecutionBackgroundAudio** を使うと、アプリがフォアグラウンドにないとき、アプリはオーディオを再生できます。<br /><br />制限付き機能 **extendedExecutionCritical** を使うと、アプリは重要な延長実行セッションを開始できます。<br /><br />制限付き機能 **extendedExecutionUnconstrained** を使うと、アプリは制限のない延長実行セッションを開始できます。 <br /><br />Microsoft Store に送信するアプリケーションでこの機能を宣言することはお勧めしません。 ほとんどの場合、この機能の使用は承認されません。<br /><br />延長実行を使用してアプリが中断されるタイミングを延期する方法については、「[延長実行を使ってアプリの中断を延期する](https://docs.microsoft.com/windows/uwp/launch-resume/run-minimized-with-extended-execution)」をご覧ください。 |
| **モバイル デバイス管理** | 制限された機能 **deviceManagementDmAccount** を使うと、アプリは、携帯電話会社の Open Mobile Alliance - Device Management (MO OMA-DM) アカウントをプロビジョニング、構成できます。<br /><br />制限付き機能 **deviceManagementFoundation** を使うと、アプリは、デバイスのモバイル デバイス管理 (MDM) 構成サービス プロバイダー (CSP) インフラストラクチャへの基本的なアクセスが可能になります。 他の機能は、特定の CSP にアクセスする必要があることに注意してください。<br /><br />制限付き機能 **deviceManagementWapSecurityPolicies** を使うと、アプリは、ワイヤレス アプリケーション プロトコル (WAP) ベースのサービス (MM、Service Indication/Service Loading (SI/SL)、Open Mobile Alliance - Client Provisioning (OMA-CP) など) を構成できます。<br /><br />制限された機能 **deviceManagementEmailAccount** を使うと、携帯電話会社が作成したアプリは、ユーザーにプロビジョニングするデバイス上の電子メール アカウントを追加および管理できます。 |
| **パッケージ ポリシー制御** | 制限付き機能 **packagePolicySystem** を使うと、アプリは、デバイスにインストールされたアプリに関連するシステム ポリシーを制御できます。<br /><br />Microsoft Store に送信するアプリケーションでこの機能を宣言することはお勧めしません。 ほとんどの場合、この機能の使用は承認されません。 |
| **ゲームの一覧** | 制限された機能 **gameList** を使うと、アプリはシステムにインストールされた既知のゲームの一覧を取得できます。<br /><br />Microsoft Store に送信するアプリケーションでこの機能を宣言することはお勧めしません。 ほとんどの場合、この機能の使用は承認されません。 |
| **Xbox アクセサリ** | 制限付き機能 **xboxAccessoryManagement** を使うと、アプリは Xbox ハードウェア仕様に準拠した Xbox デバイスを直接管理できます。<br /><br />Microsoft Store に送信するアプリケーションでこの機能を宣言することはお勧めしません。 ほとんどの場合、この機能の使用は承認されません。 |
| **アクセサリの音声認識** | 制限付き機能 **cortanaSpeechAccessory** を使うと、アプリはコマンドを呼び出して Cortana に渡すことができます。<br /><br />Microsoft Store に送信するアプリケーションでこの機能を宣言することはお勧めしません。 ほとんどの場合、この機能の使用は承認されません。 |
| **アクセサリ管理** | 制限付き機能 **accessoryManager** を使うと、アプリはアクセサリ アプリや特定のアプリ通知のオプトインとしての登録が可能になり、アクセサリに転送したり、ユーザーに表示したりできるようになります。 |
| **ドライバー アクセス** | 制限された機能 **interopServices** を使うと、アプリはデバイスと直接やり取りできます。 <br /><br />Microsoft Store に送信するアプリケーションでこの機能を宣言することはお勧めしません。 ほとんどの場合、この機能の使用は承認されません。 |
| **フォアグラウンドの監視** | 制限された機能 **inputForegroundObservation** を使うと、アプリはフォアグラウンドでキーボード入力を傍受でき、アプリ以外へのすべてのキーボード入力の処理を省くことができます。 SAS の組み合わせはこの機能により傍受することはできません。 この機能は、[**KeyboardDeliveryInterceptor**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.KeyboardDeliveryInterceptor) クラスのメンバーにアクセスするために必要です。
| **OEM および MO のパートナー アプリ** | 制限付き機能 **oemDeployment** を使うと、Microsoft パートナー製のアプリは、新しいアプリをインストールし、デバイスに現在インストールされているアプリを照会できます。<br /><br />制限された機能 **oemPublicDirectory** を使うと、Microsoft パートナー製のアプリは、共有アプリ フォルダーにアクセスできます。 Microsoft Store に送信するアプリケーションでこの機能を宣言することはお勧めしません。 ほとんどの場合、この機能の使用は承認されません。 |
| **アプリのライセンス** | 制限された機能 **appLicensing** を使うと、ライセンスの必要なくアプリを実行できます。 マニフェストにこの機能を宣言している場合、Microsoft Store にアプリを提出することはできません。 <br /><br />Microsoft Store に送信するアプリケーションでこの機能を宣言することはお勧めしません。 ほとんどの場合、この機能の使用は承認されません。 |
| **場所システム**| 制限付き機能 **locationSystem** を使うと、アプリは特権のある特定の場所の構成 (デバイスの既定の場所の設定など) を実行できます。 <br /><br />Microsoft Store に送信するアプリケーションでこの機能を宣言することはお勧めしません。 ほとんどの場合、この機能の使用は承認されません。 |
| **ユーザーデータアカウントプロバイダー**| 制限された機能 **userDataAccountsProvider** を使うと、アプリはメール、カレンダー、連絡先のアカウントを完全に管理できます。 |
| **ペンワークスペース** | **previewPenWorkspace** 機能を使うと、アプリは、ペン ワークスペースの内側でアクション記憶ハンドラとしてホストされる Windows.ApplicationModel.Preview.Notes 名前空間にアクセスできます。 |
| **セカンダリ認証の要素** | **secondaryAuthenticationFactor** 機能を使うと、アプリは近くにあるコンパニオン認証デバイス上のシークレット ストアを渡して、PC のロックを解除できます。 たとえば、コンパニオン フィットネス バンドを使用して、PC のロックを解除できます。 Windows.Security.Authentication.Identity.Provider 名前空間の API にアクセスするには、この機能が必要です。<br /><br />Microsoft Store に送信するアプリケーションでこの機能を宣言することはお勧めしません。 ほとんどの場合、この機能の使用は承認されません。 |
| **ライセンス管理の保存**| **storeLicenseManagement**機能を使うと、Microsoft パートナー ハブ アプリで、デバイス上のストア ライセンスを管理できます。 Windows.ApplicationModel.Store.LicenseManagement 名前空間の API にアクセスするには、この機能が必要です。 |
| **ユーザーシステム ID**| **userSystemId** 機能を使うと、アプリは、ユーザー固有のシステム識別子を取得できます。 この識別子は特定のシステムで現在のユーザーを一意に識別し、アプリ間での情報の関連付けに使うことができます。 Windows.System.Profile.SystemIdentification クラスの GetUserSpecificSystemId API にアクセスするには、この機能が必要です。 |
| **対象となるコンテンツ**| **targetedContent** 機能を使うと、アプリケーションは、[**Windows.Services.TargetedContent**](https://docs.microsoft.com/uwp/api/windows.services.targetedcontent) 名前空間によって提供される対象のサブスクリプション コンテンツを取得して使用できます。<br /><br />**Windows.System.Profile.SystemIdentification** 名前空間の一部の API を使用するには、この機能が必要になります。 |
| **UI オートメーション**| **uiAutomation** 機能を使うと、ナレーターなどの UI オートメーション クライアントを UI オートメーション サーバーまたはプロバイダーに接続できます。<br /><br />**Windows.Xbox.Media.Capture.Broadcaster** 名前空間の一部の API を使う場合は、この機能が必要になります。 |
|**ゲームバーサービス**| **gameBarServices** の使用は、ストア更新可能なファースト パーティ製 UWA に限定されます。<br /><br />[  **Windows.Media.Capture.GameBarsSrvices**](https://docs.microsoft.com/uwp/api/windows.media.capture.gamebarservices) クラスを使う場合は、この機能が必要になります。<br /><br />Microsoft Store に送信するアプリケーションでこの機能を宣言することはお勧めしません。 ほとんどの場合、この機能の使用は承認されません。 |
|**アプリキャプチャサービス**| **appCaptureServices** 機能の使用は、Microsoft との契約関係がある当事者に限定されます。 契約関係は、Xbox サービスおよび bizdev の支援によって成り立っているパートナー契約に基づいて設定されます。<br /><br />[  **Windows.Media.Capture.AppCaptureServices**](https://docs.microsoft.com/uwp/api/windows.media.capture.appcaptureservices) クラスを使う場合は、この機能が必要になります。 |
|**アプリブロードキャストサービス**| **appBroadcastServices** 機能の使用は、Microsoft との契約関係がある当事者に限定されます。 契約関係は、Xbox サービスの支援によって成り立っているパートナー契約に基づいて設定されます。<br /> <br />[  **Windows.Media.capture.AppBroadcastServices**](https://docs.microsoft.com/uwp/api/windows.media.capture.appbroadcastservices) クラスを使う場合は、この機能が必要になります。<br /><br />Microsoft Store に送信するアプリケーションでこの機能を宣言することはお勧めしません。 ほとんどの場合、この機能の使用は承認されません。 |
|**オーディオデバイスの構成**| **audioDeviceConfiguration** 機能を使うと、アプリケーションは、オーディオ ドライバーによって公開されるオーディオ エフェクトを照会、構成、有効化、および無効化できます。 <br /> <br />[  **Windows.Media.Devices.AudioDeviceModulesManager**](https://docs.microsoft.com/uwp/api/windows.media.devices.audiodevicemodulesmanager) クラスを使う場合は、この機能が必要になります。<br /><br />Microsoft Store に送信するアプリケーションでこの機能を宣言することはお勧めしません。 ほとんどの場合、この機能の使用は承認されません。 これは、アプリケーションで **AudioDeviceModulesManager** を使用すると、システム上のすべてのオーディオ エフェクトにアクセスできるためです。 可能性として、オーディオ エフェクトの設定によっては、デバイス上のオーディオ パフォーマンスに悪影響を与えることができます。 |
| **バックグラウンドメディア記録** | **Backgroundmediarecording**機能は、 [**MediaCapture**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture)や[**audiograph**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph)クラスなどのメディア固有の api の動作を変更して、アプリがバックグラウンドで実行されている間にメディア記録を有効にします。 |
|**インクワークスペースのプレビュー**| **previewInkWorkspace** 機能を使うと、アプリは、Ink ワークスペース内でホストされている Preview Ink 名前空間にアクセスできます。 一般的に、この機能は、デバイス上のホワイト ボード アプリケーションを置き換えるために、OEM によって使用されます。<br /> <br />[  **Windows.ApplicationModel.Preview.InkWorkspace**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.preview.inkworkspace) 名前空間の API を使う場合は、この機能が必要になります。 |
|**スタート画面の管理**| **startScreenManagement** 機能を使うと、アプリは、スタート画面にタイルを自動的にピン留めすることができます。 アプリは、バックグラウンドでピン留めすることもできます。 **startScreenManagement** 機能がないといずれかの API がブロックされるということではなく、**startScreenManagement** を使用すると、アプリで Pin API を使用しているときにシェルによって UI が表示されなくなります。 |
|**Cortana のアクセス許可**| **cortanaPermissions** 機能を使うと、アプリは、デバイス上でユーザーが Cortana に付与したアクセス許可を列挙できます。 また、アプリはこの機能によって、デバイス上で Cortana のアクセス許可の付与および取り消しを行うことができます。 **cortanaPermissions** を使うには、アクセス許可を付与する前にデバイスで法的なテキストを表示する必要があります。 アプリには、アクセス許可を変更した場合に生じる法的な影響をユーザーに通知する義務があります。<br /> <br /><br />この機能は、 **HKCU\SOFTWARE\Microsoft\Windows\CurrentVersion\Search**レジストリ設定に対する読み取りアクセス権を取得するために必要です。<br /><br />Microsoft Store に送信するアプリケーションでこの機能を宣言することはお勧めしません。 ほとんどの場合、この機能の使用は承認されません。 |
|**すべてのアプリの Mods**| **allAppMods** 機能を使うと、アプリは、すべてのアプリに対して AppMods フォルダーにアクセスできます。 Mod 管理ユーティリティでは **allAppMods** を使用し、MOD を使用するゲームやアプリの外で、その MOD を管理します。 |
|**拡張されたリソース**| **expandedResources** 機能を使うと、アプリは、ゲーム モードのリソースにアクセスできます。 Xbox と、ゲーム バーに対応する PC で、ゲーム モードのリソースとは、アプリ専用に予約されている、使用可能な CPU コアのサブセットを表します。 Xbox では、アプリは 4 GB 以上のメモリ パーティションも排他的に使用できます。<br /><br />前述のように CPU とメモリのリソースを排他的に使用するには、この機能が必要になります。 |
|**保護されたアプリ**| **protectedApp** 機能を使うと、ストアによって保護されているプロセスにアプリを読み込むことができます。 アプリがストアに取り込まれると、ストアによって実行可能ファイルに blob が追加されます。 ストアでは、Microsoft キーを使って実行可能ファイルへの署名も行われます。 blob には Microsoft の署名が必要であるため、プロセス ローダーは、保護されたプロセスを適用する機能ではなく、この blob をチェックします。<br /><br />Microsoft Store に送信するアプリケーションでこの機能を宣言することはお勧めしません。 ほとんどの場合、この機能の使用は承認されません。 |
|**ゲームモニター**| **gameMonitor** 機能を使うと、アプリによるゲーム不正がシステムのアクティブ監視で検出されるようになります。<br /><br />Microsoft Store に送信するアプリケーションでこの機能を宣言することはお勧めしません。 ほとんどの場合、この機能の使用は承認されません。 |
|**アプリの診断**| **appDiagnostics** 機能を使うと、アプリは、実行中の他の UWP アプリに関する診断情報 (パッケージ情報、メモリ使用量、アカウント名など) を取得できます。 返される情報には、アプリの実行に使用されたドメイン/コンピューター アカウント名が含まれます。呼び出し元のアプリが管理者権限で起動されている場合、そのアプリは、コンピューター上のすべてのアカウントについて、実行中のすべてのアプリのリストを取得できます。 <br /><br />[  **Windows.System.AppDiagnosticInfo**](https://docs.microsoft.com/uwp/api/windows.system.appdiagnosticinfo) クラス、**Windows.System.AppDiagnosticInfo.RequestAppDiagnosticInfoAsync** クラス、[**Windows.ApplicationModel.AppInfo**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appinfo) クラスを使う場合は、この機能が必要になります。 |
| **デバイスポータルプロバイダー** | **devicePortalProvider** 機能を使うと、アプリは **Windows.System.Diagnostics.DevicePortal** API を呼び出し、開発者モードでの診断ツール用 [Web サーバーとして](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-plugin) 機能することができます。<br /><br />Microsoft Store に送信するアプリケーションでこの機能を宣言することはお勧めしません。 ほとんどの場合、この機能の使用は承認されません。 |
| **エンタープライズクラウドシングルサインオン** | **enterpriseCloudSSO** 機能を使用すると、アプリはホスト型 Web 表示コントロール内で Azure Active Director (AAD) リソースによってシングル サインオンを使用できます。 |
| **自動的に VoIP 通話を受け入れる** | **Backgroundvoip**機能を使用すると、ユーザーが明示的に呼び出しを受け入れなくても、着信 voip 通話を自動的に受信して受け入れることができます。 この機能を利用するアプリは、カメラとマイクのフル制御が許可され、これらのリソースをバックグラウンドで使用することができます。<br /><br />Microsoft Store に送信されたアプリでは、この機能を宣言しないことをお勧めします。 ほとんどの開発者にとって、この機能の使用は承認されません。 |
| **VoIP 通話用にリソースを予約する** | **Oneprocessvoip**機能を使用すると、シングルプロセスアプリケーションで VoIP 呼び出しに必要な CPU リソースとメモリリソースを予約できます。<br /><br />Microsoft Store に送信されたアプリでは、この機能を宣言しないことをお勧めします。 ほとんどの開発者にとって、この機能の使用は承認されません。 |
| **開発モードネットワーク** | **DevelopmentModeNetwork**機能により、アプリは、 C++/cx UWP アプリまたはC++ Windows ランタイムコンポーネントで OpenFile Win32 API を呼び出すときに、サインインしたユーザーの資格情報を使用してネットワークパスにアクセスできます。 <br /><br />Microsoft Store に送信するアプリケーションでこの機能を宣言することはお勧めしません。 ほとんどの場合、この機能の使用は承認されません。 |
| **広範なファイルシステムアクセス** | **broadFileSystemAccess** 機能を使用すると、実行時にファイル ピッカー スタイルのプロンプトを追加使用しなくても、アプリはファイル システムに対して、アプリを実行中のユーザーと同じアクセス許可を獲得できます。 この機能は、ユーザーが FilePicker または FolderPicker を使用して既に選択したファイルにアクセスするためには必要ないことに注意してください。<br/><br/>この機能は、[Windows.Storage](https://docs.microsoft.com/uwp/api/windows.storage) API で動作します。 ユーザーは、[設定] でいつでもアクセス許可を付与または拒否できるため、これらの変更に対してアプリが回復可能であることを確認する必要があります。 2018 年 4 月の更新プログラムでは、アクセス許可の既定値はオンです。 2018 の年 10 月の更新プログラムでは、既定値はオフです。 さらに、この機能では**ドキュメント**、**ピクチャ**、**ビデオ**などの特殊なフォルダー機能を宣言できない点にも注意してください。 **BroadFileSystemAccess**をマニフェストに追加することで、アプリでこの機能を有効にすることができます。 例については、「[ファイルアクセス許可](/windows/uwp/files/file-access-permissions)」を参照してください。 |
| **システムファームウェアと BIOS** | **smbios** 機能を使うと、アプリは BIOS データとシステム ファームウェア データにアクセスできます。 |
| **完全信頼のアクセス許可レベル** | **Runfulltrust** restricted 機能を使用すると、アプリはユーザーのコンピューターの完全信頼のアクセス許可レベルで実行できます。 [Fulltrustprocesslauncher](https://docs.microsoft.com/uwp/api/windows.applicationmodel.fulltrustprocesslauncher) API を使用するには、この機能が必要です。<br /><br />この機能は、appx または .msix パッケージ ([デスクトップブリッジ](https://developer.microsoft.com/windows/bridges/desktop)と同様) として配信されるすべてのデスクトップアプリケーションでも必要となります。デスクトップアプリコンバーター (DAC) を使用してこれらのアプリをパッケージ化すると、マニフェストに自動的に表示されます。Visual Studio。 |
| **高度** | **Allowelevation**制限機能を使用すると、Microsoft パートナーや企業によって作成されたアプリは、起動時またはアプリの有効期間中に自動昇格を必要とする既存のデスクトップ機能を維持できます。<br/><br/>Microsoft Store に送信するアプリケーションでこの機能を宣言することはお勧めしません。 ほとんどの場合、この機能の使用は承認されません。 企業によってビジネス向けのプライベートストアに展開され Microsoft Store た基幹業務アプリでのみ承認されます。  |
| **Windows チームデバイスの資格情報** | **TeamEditionDeviceCredential** restricted 機能により、アプリは、Windows 10 バージョン1703以降を実行している Surface Hub デバイスでデバイスアカウントの資格情報を要求する api にアクセスできるようになります。<br/><br/>Microsoft Store に送信するアプリケーションでこの機能を宣言することはお勧めしません。 ほとんどの場合、この機能の使用は承認されません。 |
| **Windows チームアプリケーションビュー** | **TeamEditionView** restricted 機能により、アプリは、Windows 10 バージョン1703以降を実行している Surface Hub デバイスでアプリケーションビューをホストする api にアクセスできます。<br/><br/>Microsoft Store に送信するアプリケーションでこの機能を宣言することはお勧めしません。 ほとんどの場合、この機能の使用は承認されません。 |
| **カメラ処理拡張機能** | **CameraProcessingExtension** restricted 機能により、アプリはカメラからキャプチャされたイメージを直接カメラコントロールなしで処理できます。<br /><br />この機能は、 [PointOfService](/uwp/api/windows.devices.pointofservice.provider)名前空間の api を呼び出すために必要です。<br /><br />この機能は、ストア申請用に誰でもアクセスを要求できます。 |
| **データの使用状況の管理** | **NetworkDataUsageManagement**の制限付き機能を使用すると、アプリケーションはネットワークデータの使用状況に関する情報を収集できます。<br /><br />この機能は、 [GetAttributedNetworkUsageAsync](/uwp/api/windows.networking.connectivity.connectionprofile.getattributednetworkusageasync)を呼び出すために必要です。<br /><br />この機能は、ストア申請用に誰でもアクセスを要求できます。 |
| **電話回線接続の管理** | **PhoneLineTransportManagement**機能により、アプリは電話回線接続を担当するシステムデバイスを管理できます。<br /><br />この機能は、PhoneLineTransportDevice 名前空間の Api を使用するために必要です[。](https://docs.microsoft.com/uwp/api/windows.applicationmodel.calls) |
| **仮想化されていないリソース** | **Unvirtualizedresources**の制限された機能を使用すると、アプリケーションは、パッケージマニフェストで[registrywritevirtualization](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-desktop6-registrywritevirtualization)および[filesystemwritevirtualization](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-desktop6-filesystemwritevirtualization)要素を宣言して、レジストリとファイルシステム。 これらの宣言により、システムが HKEY_CURRENT_USER またはユーザーの AppData フォルダーへの書き込みをそれぞれ仮想化できなくなります。 これは、アプリケーションがアプリケーションと同じレジストリまたはファイルシステムのエントリの読み取りまたは書き込みを他のアプリケーションに要求している場合に便利です。<br /><br />この機能は、Microsoft およびパートナーによって発行された特定の種類のデスクトップ PC ゲーム向けに設計されています。 システムのアンインストールが正常に機能しなくなる可能性があるため、他のシナリオで使用するためのものではありません。 |
| **変更可能なアプリ** | **modifiableApp** の制限付き機能を使用すると、アプリケーションでパッケージマニフェスト内の [windows.mutablePackageDirectories](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-desktop6-package-extension) 拡張機能を宣言できます。 これにより、アプリケーションが変更または追加する必要があるフォルダーの名前を指定できます。 OS はこのフォルダーを作成し、アプリケーションによって最初にインストールされたファイルではなく、このフォルダー内のファイルをアプリケーションで使用できるようにします。<br /><br />この機能は、Microsoft およびパートナーによって発行された特定の種類のデスクトップ PC ゲーム向けに設計されています。 他のシナリオでは、署名されていないコードの実行を許可することができるため、これは許可されません。 |
| **パッケージ書き込みリダイレクト互換性 Shim** | **Packagewriteredirection 互換 shim**制限付き機能は、ユーザーごとの場所にすべての新しいファイルを作成するようにアプリケーションを構成します。 書き込み用に開かれた既存のファイルは、最初にユーザーごとの場所にコピーされ、その場所にあるファイルに対して変更が行われます。 この機能は、インストールフォルダー内のファイルを作成または変更するアプリケーションに役立ちます。<br /><br />この機能は、Microsoft およびパートナーによって発行された特定の種類のデスクトップ PC ゲーム向けに設計されています。 ただし、他のアプリにも適用される場合があります。 |
| **カスタムインストールアクション** | **Custominstallactions**の制限付き機能を使用すると、アプリケーションはパッケージマニフェストで[Windows の custominstall](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-desktop6-package-extension)拡張機能を宣言して、実行される追加のインストーラーファイル (.exe または .msi) を1つ以上指定できるようになります。アプリケーションを使用します。 これにより、インストール、更新、修復、またはアンインストールのいずれかの標準的な展開シナリオでカスタムアクションを指定できます。 たとえば、サードパーティの再頒布可能コンポーネントをバンドルするアプリケーションの場合に便利です。<br /><br />この機能は、Microsoft およびパートナーによって発行された特定の種類のデスクトップ PC ゲーム向けに設計されています。 他のシナリオでは許可されません。 |
| **パッケージサービス** | パッケージマニフェストでは、Microsoft パートナーや企業によって作成されたアプリケーションを**使用して**、1つまたは複数のサービスをインストールすることができ[ます。](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-desktop6-extension)アプリケーション. これらのサービスは、ローカルサービス、ネットワークサービス、またはローカルシステムアカウントで実行するように構成できます。 Local Service と Network Service サービスには、**パッケージ**の機能のみが必要です。 ローカルシステムサービスには、"**パッケージ**の提供" 機能と " **localsystemservices** " 機能の両方が必要です。<br /><br />Microsoft Store に送信するアプリケーションでこの機能を宣言することはお勧めしません。 ほとんどの場合、この機能の使用は承認されません。  |
| **ローカルシステムサービス** | **Localsystemservices**の制限された機能を使用すると、Microsoft のパートナーおよび企業が作成したアプリケーションで1つ以上のローカルシステムサービスをアプリと共にインストールできます (つまり、アプリケーションでは、サービスは LocalSystem になります)。 このシナリオでは、**パッケージサービス**機能も必要です。 <br /><br />Microsoft Store に送信するアプリケーションでこの機能を宣言することはお勧めしません。 ほとんどの場合、この機能の使用は承認されません。 |
| **背景空間の認識** | **BackgroundSpatialPerception**の制限付き機能を使用すると、アプリケーションは、バックグラウンドでアプリを実行しているときに、ユーザーのヘッド、ハンド、モーションコントローラー、およびその他の追跡対象オブジェクトの動きにアクセスできます。 |

## <a name="custom-capabilities"></a>カスタム機能

前の「[制限付き機能](#restricted-capabilities)」セクションでは、カスタム機能を使用する承認を要求するときに使用できるのと同じ機能承認プロセスについて説明しています。 [組み込みの SIM](/uwp/api/windows.networking.networkoperators.esim) api は、カスタム機能を必要とする api の例です。 アプリケーションを開発者モードでローカルに実行するだけの場合は、カスタム機能は必要ありません。 ただし、アプリを Microsoft Store に発行したり、開発者モード以外で実行したりするために必要です。

Windows テクニカルアカウントマネージャー (TAM) がある場合は、TAM と協力してアクセスを要求することができます。 詳細につい[ては、「MICROSOFT TAM に問い合わせる](/windows-hardware/drivers/mobilebroadband/testing-your-desktop-cosa-apn-database-submission#contact-your-microsoft-tam)」を参照してください。

カスタム機能を宣言するには、[アプリケーションパッケージのマニフェスト](https://docs.microsoft.com/uwp/schemas/appxpackage/appx-package-manifest)ソースファイル (`Package.appxmanifest`) を変更します。 **Xmlns: uap4** XML 名前空間宣言を追加し、カスタム機能を宣言するときに**uap4**プレフィックスを使用します。 次に例を示します。

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
> すべての**customcapability**要素は、パッケージマニフェストの **[機能]** ノードの下にあるすべての[DeviceCapability](#device-capabilities)要素の後、すべての**機能**要素の後に指定する必要があります。

## <a name="related-topics"></a>関連トピック

* [送信オプション](../publish/manage-submission-options.md)
* [パッケージマニフェストで機能を指定する方法](https://docs.microsoft.com/uwp/schemas/appxpackage/how-to-specify-capabilities-in-a-package-manifest)
* [パッケージマニフェストでデバイス機能を指定する方法](https://docs.microsoft.com/uwp/schemas/appxpackage/how-to-specify-device-capabilities-in-a-package-manifest)
