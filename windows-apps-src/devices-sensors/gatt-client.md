---
title: Bluetooth GATT クライアント
description: この記事では、ユニバーサル Windows プラットフォーム (UWP) アプリ用の Bluetooth 汎用属性プロファイル (GATT) クライアントの概要と、一般的な使用事例のサンプル コードについて説明します。
ms.date: 06/26/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 339a154c3acf39c4f574d22907cf697db658552b
ms.sourcegitcommit: 74c2c878b9dbb92785b89f126359c3f069175af2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2020
ms.locfileid: "93122403"
---
# <a name="bluetooth-gatt-client"></a>Bluetooth GATT クライアント

この記事では、ユニバーサル Windows プラットフォーム (UWP) アプリ用の Bluetooth 汎用属性 (GATT) クライアント API の使用方法を、一般的な GATT クライアント タスクのサンプル コードを使って示します。

- 近くのデバイスの照会
- デバイスへの接続
- デバイスでサポートされているサービスやデバイスの特性の列挙
- 特性の読み取りと書き込み
- 特性値が変化したときの通知の受信登録

> [!Important]
> *Package.appxmanifest* で "bluetooth" 機能を宣言する必要があります。
>
> `<Capabilities> <DeviceCapability Name="bluetooth" /> </Capabilities>`

> **重要な API**
>
> - [**Windows.Devices.Bluetooth**](/uwp/api/Windows.Devices.Bluetooth)
> - [**Windows. Devices. GenericAttributeProfile**](/uwp/api/Windows.Devices.Bluetooth.GenericAttributeProfile)

## <a name="overview"></a>概要

開発者は、 [**Windows.Devices.Bluetooth.GenericAttributeProfile**](/uwp/api/Windows.Devices.Bluetooth.GenericAttributeProfile) 名前空間の API を使って Bluetooth LE デバイスにアクセスすることができます。 Bluetooth LE デバイスは、その機能をコレクションを通じて公開します。コレクションには次の情報が含まれています。

- サービス
- 特性
- 記述子

サービスは、LE デバイスの機能的なコントラクトを定義するもので、サービスを定義する特性のコレクションを含みます。 これらの特性はさらに、その特性を表す記述子を含みます。 これら 3 つの用語は、一般的に、デバイスの属性と呼ばれます。

Bluetooth LE GATT API は、生のトランスポートにアクセスするのではなく、オブジェクトと関数を公開します。 また、GATT API で Bluetooth LE デバイスと連携することによって、次のことが可能となります。

- 属性の検出の実行
- 属性値の読み取りと書き込み
- 特性の ValueChanged イベントで呼び出されるコールバックの登録

実用的なアプリケーションを作成するためには、利用する GATT のサービスと特性についての予備知識が開発者に求められます。実際に必要な特性値を処理し、API から提供されるバイナリ データを実用的なデータに変換したうえで、ユーザーに提示しなければなりません。 Bluetooth GATT API が公開するのは、Bluetooth LE デバイスとの通信に必要な基本的なプリミティブだけです。 データを解釈するためには、Bluetooth SIG の標準のプロファイルか、デバイスのベンダーが実装したカスタム プロファイルによって、アプリケーション プロファイルを定義する必要があります。 プロファイルは、交換されるデータが表す内容や、その解釈の方法に関して、アプリケーションとデバイスとの間で交わされるバインド コントラクトを形成します。

Bluetooth SIG は、利便性向上のため、[一連のプロファイル](https://www.bluetooth.com/specifications/adopted-specifications#gattspec)を一般公開しています。

## <a name="examples"></a>例

完全なサンプルについては、「 [Bluetooth 低エネルギーサンプル](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/BluetoothLE)」を参照してください。

### <a name="query-for-nearby-devices"></a>近くのデバイスの照会

近くのデバイスを照会するための主なメソッドは 2 つあります。

- Windows.Devices.Enumeration の DeviceWatcher
- Windows.Devices.Bluetooth.Advertisement の AdvertisementWatcher

2 つ目のメソッドについては、[アドバタイズ](ble-beacon.md)に関するドキュメントで詳しく説明されているため、ここでは簡単に説明します。基本的な考え方は、特定の[アドバタイズ フィルター](/uwp/api/windows.devices.bluetooth.advertisement.bluetoothleadvertisementwatcher.advertisementfilter)の条件を満たす、近くにあるデバイスの Bluetooth アドレスを検出するということです。 アドレスを検出したら、[BluetoothLEDevice.FromBluetoothAddressAsync](/uwp/api/windows.devices.bluetooth.bluetoothledevice.frombluetoothaddressasync) を呼び出して、デバイスへの参照を取得します。

DeviceWatcher メソッドの説明に戻ります。 Bluetooth LE デバイスは、Windows の他のデバイスと同じように[列挙 API](/uwp/api/Windows.Devices.Enumeration) を使って照会できます。 [DeviceWatcher](/uwp/api/windows.devices.enumeration.devicewatcher) クラスを使用して、検索するデバイスを指定するクエリ文字列を渡します。

```csharp
// Query for extra properties you want returned
string[] requestedProperties = { "System.Devices.Aep.DeviceAddress", "System.Devices.Aep.IsConnected" };

DeviceWatcher deviceWatcher =
            DeviceInformation.CreateWatcher(
                    BluetoothLEDevice.GetDeviceSelectorFromPairingState(false),
                    requestedProperties,
                    DeviceInformationKind.AssociationEndpoint);

// Register event handlers before starting the watcher.
// Added, Updated and Removed are required to get all nearby devices
deviceWatcher.Added += DeviceWatcher_Added;
deviceWatcher.Updated += DeviceWatcher_Updated;
deviceWatcher.Removed += DeviceWatcher_Removed;

// EnumerationCompleted and Stopped are optional to implement.
deviceWatcher.EnumerationCompleted += DeviceWatcher_EnumerationCompleted;
deviceWatcher.Stopped += DeviceWatcher_Stopped;

// Start the watcher.
deviceWatcher.Start();
```

DeviceWatcher を開始すると、対象のデバイスの [Added](/uwp/api/windows.devices.enumeration.devicewatcher.added) イベントのハンドラーで、クエリを満たすデバイスごとに [DeviceInformation](/uwp/api/Windows.Devices.Enumeration.DeviceInformation) を受信します。 DeviceWatcher について詳しくは、[Github](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/DeviceEnumerationAndPairing) にある完全なサンプルをご覧ください。

### <a name="connecting-to-the-device"></a>デバイスへの接続

目的のデバイスが検出されたら、[DeviceInformation.Id](/uwp/api/windows.devices.enumeration.deviceinformation.id) を使用して、対象のデバイスの Bluetooth LE デバイス オブジェクトを取得します。

```csharp
async void ConnectDevice(DeviceInformation deviceInfo)
{
    // Note: BluetoothLEDevice.FromIdAsync must be called from a UI thread because it may prompt for consent.
    BluetoothLEDevice bluetoothLeDevice = await BluetoothLEDevice.FromIdAsync(deviceInfo.Id);
    // ...
}
```

一方、デバイスの BluetoothLEDevice オブジェクトへのすべての参照を破棄すると (システム上の他のアプリがそのデバイスを参照していない場合)、短いタイムアウト期間後に自動切断がトリガーされます。

```csharp
bluetoothLeDevice.Dispose();
```

アプリが再びデバイスにアクセスする必要がある場合は、単にデバイス オブジェクトを再作成して特性にアクセスする (次のセクションで説明します) と、必要に応じて、OS による再接続がトリガーされます。 デバイスが近くにある場合は、デバイスへのアクセスが取得されます。近くにない場合は、DeviceUnreachable エラーと共に制御が戻ります。  

> [!NOTE]
> このメソッドだけを呼び出すことによって [BluetoothLEDevice](/uwp/api/windows.devices.bluetooth.bluetoothledevice) オブジェクトを作成することは、必ずしも接続を開始するわけではありません。 接続を開始するには、 [MaintainConnection](/uwp/api/windows.devices.bluetooth.genericattributeprofile.gattsession.maintainconnection)をに設定するか、BluetoothLEDevice でキャッシュされていない `true` サービスの探索方法を呼び出すか、デバイスに対して読み取り/書き込み操作を実行します。 **BluetoothLEDevice**
>
> - **MaintainConnection** が true に設定されている場合、システムは接続を無期限に待機し、デバイスが利用可能になったときに接続します。 **MaintainConnection** はプロパティであるため、アプリケーションが待機するものはありません。
> - GATT でのサービスの検出と読み取り/書き込み操作の場合、システムは有限の時間だけ待機します。 瞬時から数分で。 スタック上のトラフィックを把握する要因と、要求をキューに登録する方法を示します。 他の保留中の要求がなく、リモートデバイスに到達できない場合、システムは7秒間待機してからタイムアウトします。他の保留中の要求がある場合は、キュー内の各要求が処理に 7 (7) 秒かかることがあります。そのため、キューの後ろの方が待機時間が長くなります。
>
> 現時点では、接続プロセスを取り消すことはできません。

### <a name="enumerating-supported-services-and-characteristics"></a>サポートされているサービスと特性の列挙

BluetoothLEDevice オブジェクトが取得されたので、次の手順はデバイスが公開するデータを検出することです。 これを行うための最初の手順は、サービスの照会です。

```csharp
GattDeviceServicesResult result = await bluetoothLeDevice.GetGattServicesAsync();

if (result.Status == GattCommunicationStatus.Success)
{
    var services = result.Services;
    // ...
}
```

対象のサービスが識別されたら、次の手順は特性の照会です。

```csharp
GattCharacteristicsResult result = await service.GetCharacteristicsAsync();

if (result.Status == GattCommunicationStatus.Success)
{
    var characteristics = result.Characteristics;
    // ...
}
```

OS は、操作を実行できる対象の GattCharacteristic オブジェクトの読み取り専用のリストを返します。

### <a name="perform-readwrite-operations-on-a-characteristic"></a>特性の読み取り/書き込み操作の実行

特性は、GATT ベースの通信の基本単位です。 これには、デバイス上の各種データを表す値が含まれています。 たとえば、バッテリ レベル特性には、デバイスのバッテリ レベルを表す値が含まれます。

特性のプロパティを読み取って、どのような操作がサポートされているかを特定します。

```csharp
GattCharacteristicProperties properties = characteristic.CharacteristicProperties

if(properties.HasFlag(GattCharacteristicProperties.Read))
{
    // This characteristic supports reading from it.
}
if(properties.HasFlag(GattCharacteristicProperties.Write))
{
    // This characteristic supports writing to it.
}
if(properties.HasFlag(GattCharacteristicProperties.Notify))
{
    // This characteristic supports subscribing to notifications.
}
```

読み取りがサポートされている場合は、値を読み取ることができます。

```csharp
GattReadResult result = await selectedCharacteristic.ReadValueAsync();
if (result.Status == GattCommunicationStatus.Success)
{
    var reader = DataReader.FromBuffer(result.Value);
    byte[] input = new byte[reader.UnconsumedBufferLength];
    reader.ReadBytes(input);
    // Utilize the data as needed
}
```

特性への書き込みは、同様のパターンに従います。

```csharp
var writer = new DataWriter();
// WriteByte used for simplicity. Other common functions - WriteInt16 and WriteSingle
writer.WriteByte(0x01);

GattCommunicationStatus result = await selectedCharacteristic.WriteValueAsync(writer.DetachBuffer());
if (result == GattCommunicationStatus.Success)
{
    // Successfully wrote to device
}
```

> **ヒント** : [DataReader](/uwp/api/windows.storage.streams.datareader) と [Datawriter](/uwp/api/windows.storage.streams.datawriter) は、多くの Bluetooth api から取得した未加工のバッファーを操作するときに不可欠されます。

### <a name="subscribing-for-notifications"></a>通知の受信登録

特性が Indicate または Notify をサポートしているかどうかを確認します (確認するには特性のプロパティを調べます)。

> **余談** : Indicate は、それぞれの値変更イベントが、クライアント デバイスからの受信確認とセットになっているため、より信頼性が高いと見なされています。 多くの GATT トランザクションでは、非常に高い信頼性よりも電力の節約が重視されるため、Notify の方が一般的です。 いずれの場合も、そのすべてがコントローラー レイヤーで処理されるため、アプリは関与しません。 これらを総称して単に「通知」と呼びますが、覚えておいてください。

通知を取得する前に処理することが 2 つあります。

- Client Characteristic Configuration Descriptor (CCCD) への書き込み
- Characteristic.ValueChanged イベントの処理

CCCD への書き込みによって、特定の特性値が変化するたびに、このクライアントでその変化を把握する必要があることを、サーバー デバイスに指示します。 この操作を行うには、次の手順を実行します。

```csharp
GattCommunicationStatus status = await selectedCharacteristic.WriteClientCharacteristicConfigurationDescriptorAsync(
                        GattClientCharacteristicConfigurationDescriptorValue.Notify);
if(status == GattCommunicationStatus.Success)
{
    // Server has been informed of clients interest.
}
```

これで、リモート デバイスで値が変更されるたび、GattCharacteristic の ValueChanged イベントが呼び出されます。 あとはハンドラーを実装するだけです。

```csharp
characteristic.ValueChanged += Characteristic_ValueChanged;

...

void Characteristic_ValueChanged(GattCharacteristic sender,
                                    GattValueChangedEventArgs args)
{
    // An Indicate or Notify reported that the value has changed.
    var reader = DataReader.FromBuffer(args.CharacteristicValue)
    // Parse the data however required.
}
```

