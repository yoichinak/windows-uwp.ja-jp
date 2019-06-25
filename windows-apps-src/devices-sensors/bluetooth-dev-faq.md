---
title: Bluetooth に関する開発者向け FAQ
description: この記事には、UWP Bluetooth API に関連するよく寄せられる質問に対する回答が含まれています。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: e7dee32d-3756-430d-a026-32c1ee288a85
ms.localizationpriority: medium
ms.openlocfilehash: 704ce146f95ed64a5891c130fee4d78c90dff034
ms.sourcegitcommit: 58d35b89662d4ad240650933e43fee0b00e9a962
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/24/2019
ms.locfileid: "67344524"
---
# <a name="bluetooth-developer-faq"></a>Bluetooth に関する開発者向け FAQ

この記事には、UWP Bluetooth API のよく寄せられる質問に対する回答が含まれています。

## <a name="what-apis-do-i-use-bluetooth-classic-rfcomm-or-bluetooth-low-energy-gatt"></a>どの API を使用すればよいですか。 Bluetooth Classic (RFCOMM) と Bluetooth 低エネルギー (GATT) のどちらですか。
この一般的なトピックに関してはオンラインでもさまざまな議論が行われています。そこで、この回答では Windows に関しての違いについて説明します。 ここでは、一般的なガイドラインをいくつか示します。

### <a name="bluetooth-le-windowsdevicesbluetoothgenericattributeprofile"></a>Bluetooth LE (Windows.Devices.Bluetooth.GenericAttributeProfile)

Bluetooth 低エネルギーをサポートするデバイスと通信している場合は、GATT API を使用します。 ユース ケースには、低頻度、低帯域幅、または低電力が必要です、答えは Bluetooth 低エネルギーです。 この機能を含む主な名前空間は、[Windows.Devices.Bluetooth.GenericAttributeProfile](https://docs.microsoft.com/en-us/uwp/api/Windows.Devices.Bluetooth.GenericAttributeProfile) です。 

**Bluetooth LE を使用しない場合**
- 高帯域幅で使用頻度の高いシナリオ。 常に大量のデータを同期する必要がある場合は、Bluetooth Classic または WiFi の使用を検討します。 

### <a name="bluetooth-classic-windowsdevicesbluetoothrfcomm"></a>Bluetooth Classic (Windows.Devices.Bluetooth.Rfcomm)

RFCOMM Api は、双方向のシリアル ポート スタイルの通信を実行するソケットを開発者に提供します。 ソケットをした後の書き込みと読み取り用のメソッドはごく標準的なです。 この実装については、[Rfcomm チャット サンプル](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BluetoothRfcommChat)をご覧ください。 

**Bluetooth Rfcomm を使用しない場合** 
- 通知。 Bluetooth GATT プロトコルにはこのための特定のコマンドがあり、電力消費量が大幅に減少し、応答時間が高速になります。 
- 近接通信またはプレゼンスの検出の確認。 [アドバタイズ API](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.advertisement) を使用し、Bluetooth LE 経由で接続することをお勧めします。 


## <a name="why-does-my-bluetooth-le-device-stop-responding-after-a-disconnect"></a>Bluetooth LE デバイスが、切断後、応答を停止する理由を教えてください。

これが発生した最も一般的な理由、リモート デバイスがペアリングの情報を失われるためにです。 古い Bluetooth デバイスの数が多い認証は必要ありません。 設定アプリケーションから実行されたすべてのペアリング トランザクションが認証を要求には、ユーザーを保護し、一部のデバイスがこれを念頭設計されません。 

1511 の Windows 10 リリース以降、開発者は、ペアリング ハンドシェイクに制御があります。 [デバイスの列挙とペアリングのサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/DeviceEnumerationAndPairing)に関するページでは、新しいデバイスの関連付けのさまざまな側面について詳しく説明しています。

この例では、暗号化を使用していないデバイスとのペアリングを開始します。 この例が動作するのは、リモート デバイスが暗号化や認証の機能を必要としない場合のみであることに注意してください。

```csharp
// Get ceremony type and protection level selections
// You must select at least ConfirmOnly or the pairing attempt will fail
    DevicePairingKinds ceremonySelected = DevicePairingKinds.ConfirmOnly;

//  Workaround remote devices losing pairing information
    DevicePairingProtectionLevel protectionLevel = DevicePairingProtectionLevel.None

    DeviceInformationCustomPairing customPairing = deviceInfoDisp.DeviceInformation.Pairing.Custom;

// Declare an event handler - you don't need to do much in PairingRequestedHandler since the ceremony is "None"
    customPairing.PairingRequested += PairingRequestedHandler;
    DevicePairingResult result = await customPairing.PairAsync(ceremonySelected, protectionLevel);
```

## <a name="do-i-have-to-pair-bluetooth-devices-before-using-them"></a>Bluetooth デバイスを使用する前にペアリングする必要はありますか。

Bluetooth RFCOMM (クラシック) を利用する場合に使用する前にデバイス ペアにする必要はありません。 Windows 10 リリース 1607 以降では、簡単に近くにあるデバイスを照会し、そのデバイスに接続できます。 更新された[RFCOMM チャット サンプル](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BluetoothRfcommChat)で、この機能について説明しています。 

**(14393 以下)** この機能は、Bluetooth 低エネルギー (GATT クライアント) では使用できないため、これらのデバイスにアクセスするために、[設定] ページで、または [Windows.Devices.Enumeration](https://docs.microsoft.com/uwp/api/windows.devices.enumeration) API を使ってペアリングを行う必要があります。

**(15030 以上)** Bluetooth デバイスのペアリングは不要になりました。 リモート デバイスの現在の状態を照会するために、GetGattServicesAsync や GetCharacteristicsAsync のような新しい非同期 API を使います。 詳しくは、[クライアントのマニュアル](gatt-client.md)をご覧ください。 

## <a name="when-should-i-pair-with-a-device-before-communicating-with-it"></a>デバイスと通信する前にペアリングが必要になるのはどのような場合ですか。
一般に、デバイスで信頼されている、長期的な債券を必要とする場合、いずれかでそれにペアは設定ページにユーザーをリダイレクトまたは、デバイスの列挙とペアリングの Api を使用しています。 (温度センサーまたはビーコンなど) であるデバイスからの情報が一般公開の読み取りに必要な場合、接続するか、提供情報、デバイスとペアリングするすべての作業を加えずにリッスンします。 多数のデバイスがペアリングをサポートしていないために、相互運用性の問題を長い目で見れば、これにより。 

## <a name="do-all-windows-devices-support-peripheral-role"></a>すべての Windows デバイスが周辺機器の役割をサポートしていますか。

No. これは、ハードウェアに依存する機能は、メソッドは提供、BluetoothAdapter.IsPeripheralRoleSupported、かサポートされているかどうかを照会します。  現在サポートされているデバイスには、8992 以上を搭載した Windows Phone や RPi3 (Windows IoT) が含まれます。 

## <a name="can-i-access-these-apis-from-win32"></a>Win32 からこれらの API にアクセスできますか。

はい。これらすべての API が機能します。 このブログでは、[デスクトップ アプリケーションから Windows API](https://blogs.windows.com/buildingapps/2017/01/25/calling-windows-10-apis-desktop-application/) を呼び出す方法の詳細を説明しています。 
## <a name="is-this-functionality-supposed-to-exist-on--insert-sku-here-"></a>この機能は、 *[-Insert SKU here-]* (ここに SKU を挿入) にも表示されますか。

**Bluetooth LE**:はい、すべての機能は OneCore であり、機能している Bluetooth LE スタックを使用した最新のデバイスで使用可能にする必要があります。 
> 注意:周辺のロールは、ハードウェアに依存し、Bluetooth をサポートしていない Windows Server の一部のエディション。 

**Bluetooth BR/EDR (クラシック)** :いくつかのバリエーションの存在が、ほとんどの場合、プロファイル レベルのサポートとよく似ています。 ドキュメントを参照してください[RFCOMM](send-or-receive-files-with-rfcomm.md)とサポートされているこれらのドキュメントをプロファイル[PC](https://support.microsoft.com/en-us/help/10568/windows-10-supported-bluetooth-profiles)と[電話](https://support.microsoft.com/en-us/help/10569/windows-10-mobile-supported-bluetooth-profiles)
