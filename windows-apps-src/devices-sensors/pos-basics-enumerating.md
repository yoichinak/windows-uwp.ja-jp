---
title: PointOfService デバイスの列挙
description: デバイスセレクターを使用して、システムで使用可能な PointOfService デバイスを照会して列挙する4つの方法について説明します。
ms.date: 10/08/2018
ms.topic: article
keywords: Windows 10, UWP, 店舗販売時点管理, POS
ms.localizationpriority: medium
ms.openlocfilehash: 6d1767e49e14f424b7d21424fbfc02b76acd546a
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89159676"
---
# <a name="enumerating-point-of-service-devices"></a>POS デバイスの列挙
このセクションでは、システムが利用できるデバイスを照会するために使用する[デバイス セレクターを定義](./build-a-device-selector.md)し、次のいずれかの方法でこのセレクターを使用して POS デバイスを列挙する方法について説明します。

**方法 1:** [デバイスピッカーを使用](#method-1-use-a-device-picker)する
<br/>
デバイスピッカー UI を表示し、ユーザーが接続されたデバイスを選択できるようにします。 このメソッドは、デバイスがアタッチされ削除されたときのリストの更新を処理し、他の方法よりも簡単で安全です。

**方法 2:** [最初に使用可能なデバイスを取得](#method-2-get-first-available-device)する<br />特定の時点のサービスデバイスクラスで使用可能な最初のデバイスにアクセスするには、 [Getdefaultasync](/uwp/api/windows.devices.pointofservice.barcodescanner.getdefaultasync) を使用します。

**方法 3:** [デバイスのスナップショット](#method-3-snapshot-of-devices)<br />特定の時点におけるシステム上に存在するサービスの時点のデバイスのスナップショットを列挙します。 これは、独自の UI を作成する場合や、ユーザーに UI を表示せずにデバイスを列挙する場合に役立ちます。 [Findallasync](/uwp/api/windows.devices.enumeration.deviceinformation.findallasync) は、列挙全体が完了するまで結果を保持します。

**方法 4:** [列挙してウォッチ](#method-4-enumerate-and-watch)する<br />[Devicewatcher](/uwp/api/Windows.Devices.Enumeration.DeviceWatcher) は、より強力で柔軟性の高い列挙モデルであり、現在存在しているデバイスを列挙できます。また、デバイスがシステムに追加または削除されたときに通知を受け取ることができます。  これは、スナップショットが発生するのを待機するのではなく、UI で表示するためにバックグラウンドでデバイスの現在の一覧を維持する場合に役立ちます。

## <a name="define-a-device-selector"></a>デバイス セレクターの定義
デバイス セレクターにより、デバイスを列挙するときに、検索するデバイスを絞り込むことができるようになります。  これにより、関連する結果のみを取得し、目的のデバイスを列挙するためにかかる時間を短縮することができます。

検索するデバイスの種類に対して **Getdeviceselector** メソッドを使用して、その種類のデバイスセレクターを取得できます。 たとえば、PosPrinter を使用すると、システムに接続されているすべての[PosPrinters](/uwp/api/windows.devices.pointofservice.posprinter) (USB、ネットワーク、Bluetooth POS プリンターなど) を列挙するセレクターが表示され[ます。](/uwp/api/windows.devices.pointofservice.posprinter.getdeviceselector#Windows_Devices_PointOfService_PosPrinter_GetDeviceSelector)

```Csharp
using Windows.Devices.PointOfService;

string selector = POSPrinter.GetDeviceSelector();
```

さまざまな種類のデバイスの **Getdeviceselector** メソッドは次のとおりです。

* [Bardevicepath. GetDeviceSelector](/uwp/api/windows.devices.pointofservice.barcodescanner.getdeviceselector)
* [CashDrawer](/uwp/api/windows.devices.pointofservice.cashdrawer.getdeviceselector)
* [LineDisplay. GetDeviceSelector](/uwp/api/windows.devices.pointofservice.linedisplay.getdeviceselector)
* [MagneticStripeReader](/uwp/api/windows.devices.pointofservice.magneticstripereader.getdeviceselector)
* [PosPrinter](/uwp/api/windows.devices.pointofservice.posprinter.getdeviceselector)

[Posconnectiontypes](/uwp/api/windows.devices.pointofservice.posconnectiontypes)値をパラメーターとして受け取る**getdeviceselector**メソッドを使用して、ローカル、ネットワーク、または Bluetooth 接続された POS デバイスを列挙するようにセレクターを制限し、クエリの完了にかかる時間を短縮できます。  次のサンプルでは、このメソッドを使用して、ローカルに接続されている POS プリンターのみをサポートするセレクターを定義しています。

 ```Csharp
using Windows.Devices.PointOfService;

string selector = POSPrinter.GetDeviceSelector(PosConnectionTypes.Local);
```

> [!TIP]
> より高度なセレクター文字列のビルドについては、「[デバイス セレクターのビルド](./build-a-device-selector.md)」を参照してください。

## <a name="method-1-use-a-device-picker"></a>方法 1: デバイスピッカーを使用する

[Devicepicker](/uwp/api/windows.devices.enumeration.devicepicker)クラスを使用すると、ユーザーが選択できるデバイスの一覧を含む選択ポップアップを表示できます。 [フィルター](/uwp/api/windows.devices.enumeration.devicepicker.filter)プロパティを使用して、ピッカーに表示するデバイスの種類を選択できます。 このプロパティの型は [DevicePickerFilter](/uwp/api/windows.devices.enumeration.devicepickerfilter)です。 [Supporteddeviceclasses](/uwp/api/windows.devices.enumeration.devicepickerfilter.supporteddeviceclasses)または[supporteddeviceセレクタ](/uwp/api/windows.devices.enumeration.devicepickerfilter.supporteddeviceselectors)プロパティを使用して、デバイスの種類をフィルターに追加できます。

デバイスピッカーを表示する準備ができたら、 [PickSingleDeviceAsync](/uwp/api/windows.devices.enumeration.devicepicker.picksingledeviceasync) メソッドを呼び出すことができます。これにより、ピッカー UI が表示され、選択したデバイスが返されます。 ポップアップが表示される場所を決定する [Rect](/uwp/api/windows.foundation.rect) を指定する必要があります。 このメソッドは [Deviceinformation](/uwp/api/windows.devices.enumeration.deviceinformation) オブジェクトを返します。そのため、サービス Api のポイントで使用するには、必要な特定のデバイスクラスに対して **fromidasync** メソッドを使用する必要があります。 [DeviceInformation.Id](/uwp/api/windows.devices.enumeration.deviceinformation.id)プロパティをメソッドの*deviceId*パラメーターとして渡し、デバイスクラスのインスタンスを戻り値として取得します。

次のコードスニペットでは、 **Devicepicker**を作成し、バーコードスキャナーフィルターを追加して、ユーザーがデバイスを選択し、デバイス ID に基づいて **bardevicepath canの** オブジェクトを作成します。

```cs
private async Task<BarcodeScanner> GetBarcodeScanner()
{
    DevicePicker devicePicker = new DevicePicker();
    devicePicker.Filter.SupportedDeviceSelectors.Add(BarcodeScanner.GetDeviceSelector());
    Rect rect = new Rect();
    DeviceInformation deviceInformation = await devicePicker.PickSingleDeviceAsync(rect);
    BarcodeScanner barcodeScanner = await BarcodeScanner.FromIdAsync(deviceInformation.Id);
    return barcodeScanner;
}
```

## <a name="method-2-get-first-available-device"></a>方法 2: 最初に使用可能なデバイスを取得する

サービスデバイスのポイントを取得する最も簡単な方法は、 **Getdefaultasync** を使用して、サービスデバイスクラス内で使用可能な最初のデバイスを取得することです。 

次のサンプルでは、 [barコード](/uwp/api/windows.devices.pointofservice.barcodescanner)で[getdefaultasync](/uwp/api/windows.devices.pointofservice.barcodescanner.getdefaultasync#Windows_Devices_PointOfService_BarcodeScanner_GetDefaultAsync)使用方法を示しています。 コーディングパターンは、すべてのポイントサービスデバイスクラスに似ています。

```Csharp
using Windows.Devices.PointOfService;

BarcodeScanner barcodeScanner = await BarcodeScanner.GetDefaultAsync();
```

> [!CAUTION]
> **Getdefaultasync** 、セッション間で別のデバイスを返す場合があるため、注意して使用する必要があります。 多くのイベントがこの列挙に影響を与え、次のように最初に利用できるデバイスが異なる結果になる場合があります。 
> - コンピューターに接続されているカメラの変更 
> - コンピューターに接続されているサービスの時点でのデバイスの変更
> - ネットワーク上で使用可能なネットワーク接続ポイントサービスデバイスの変更
> - コンピューターの範囲内にある Bluetooth サービスデバイスの変更 
> - サービス構成のポイントに対する変更 
> - ドライバーまたは OPOS サービス オブジェクトのインストール
> - サービス拡張機能のインストール
> - Windows オペレーティング システムの更新

## <a name="method-3-snapshot-of-devices"></a>方法 3: デバイスのスナップショット

シナリオによっては、独自の UI を作成する場合や、ユーザーに UI を表示せずにデバイスを列挙する場合などが考えられます。  このような場合は、[DeviceInformation.FindAllAsync](/uwp/api/windows.devices.enumeration.deviceinformation.findallasync) を使用して現在接続されている、またはシステムとペアリングされているデバイスのスナップショットを列挙する可能性があります。  この方法では列挙がすべて完了するまで結果が表示されません。

> [!TIP]
> **Findallasync**を使用してクエリを目的の接続の種類に制限する場合は、 **Getdeviceselector**メソッドと**posconnectiontypes**パラメーターを使用することをお勧めします。  ネットワーク接続と Bluetooth 接続では、 **Findallasync** の結果が返される前に、列挙が完了する必要があるため、結果を遅延させることができます。

> [!CAUTION] 
> **Findallasync** は、デバイスの配列を返します。  この配列の順序はセッションごとに変わる可能性があるため、配列にハードコードされたインデックスを使用することで特定の順序に依存しないことをお勧めします。  [Deviceinformation](/uwp/api/windows.devices.enumeration.deviceinformation)のプロパティを使用して、結果をフィルター処理するか、ユーザーが選択するための UI を提供します。

このサンプルでは、上で定義したセレクターを使用して、 **Findallasync** を使用してデバイスのスナップショットを作成し、コレクションによって返された各項目を列挙して、デバイス名と ID をデバッグ出力に書き込みます。 

```Csharp
using Windows.Devices.Enumeration;

DeviceInformationCollection deviceCollection = await DeviceInformation.FindAllAsync(selector);

foreach (DeviceInformation devInfo in deviceCollection)
{
    Debug.WriteLine("{0} {1}", devInfo.Name, devInfo.Id);
}
```

> [!TIP] 
> [Windows.Devices.Enumeration](/uwp/api/Windows.Devices.Enumeration) API を操作する場合は、[DeviceInformation](/uwp/api/windows.devices.enumeration.deviceinformation) オブジェクトを頻繁に使用して特定のデバイスに関する情報を取得する必要があります。 たとえば、 [DeviceInformation.ID](/uwp/api/windows.devices.enumeration.deviceinformation.id) プロパティを使用して、今後のセッションで使用可能な場合は、同じデバイスを回復して再利用することができます。また、 [DeviceInformation.Name](/uwp/api/windows.devices.enumeration.deviceinformation.name) プロパティは、アプリでの表示目的に使用できます。  利用可能なその他のプロパティについては、「[DeviceInformation](/uwp/api/windows.devices.enumeration.deviceinformation)」リファレンス ページを参照してください。

## <a name="method-4-enumerate-and-watch"></a>方法 4: 列挙してウォッチする

デバイスをより強力かつ柔軟な方法で列挙するには、[DeviceWatcher](/uwp/api/Windows.Devices.Enumeration.DeviceWatcher) を作成します。  デバイス ウォッチャーはデバイスを動的に列挙します。これにより、初回の列挙が完了した後にデバイスが追加、削除、変更された場合、アプリケーションが通知を受け取ることができます。  **Devicewatcher**を使用すると、ネットワークに接続されているデバイスがオンラインになったこと、Bluetooth デバイスが範囲内にあること、およびローカルに接続されているデバイスが取り外されているかどうかを検出できます。これにより、アプリケーション内で適切な操作を行うことができます。

このサンプルでは、上で定義したセレクターを使用して **Devicewatcher** を作成すると共に、 [追加](/uwp/api/windows.devices.enumeration.devicewatcher.added)、 [削除](/uwp/api/windows.devices.enumeration.devicewatcher.removed)、および [更新](/uwp/api/windows.devices.enumeration.devicewatcher.updated) された通知のイベントハンドラーを定義します。 各通知で実行するアクションの詳細を記入する必要があります。

```Csharp
using Windows.Devices.Enumeration;

DeviceWatcher deviceWatcher = DeviceInformation.CreateWatcher(selector);
deviceWatcher.Added += DeviceWatcher_Added;
deviceWatcher.Removed += DeviceWatcher_Removed;
deviceWatcher.Updated += DeviceWatcher_Updated;

void DeviceWatcher_Added(DeviceWatcher sender, DeviceInformation args)
{
    // TODO: Add the DeviceInformation object to your collection
}

void DeviceWatcher_Removed(DeviceWatcher sender, DeviceInformationUpdate args)
{
    // TODO: Remove the item in your collection associated with DeviceInformationUpdate
}

void DeviceWatcher_Updated(DeviceWatcher sender, DeviceInformationUpdate args)
{
    // TODO: Update your collection with information from DeviceInformationUpdate
}
```

> [!TIP]
> **Devicewatcher**の使用の詳細については、「[デバイスの列挙と監視]( ./enumerate-devices.md#enumerate-and-watch-devices)」を参照してください。

## <a name="see-also"></a>関連項目
* [サービスポイントの概要](pos-basics.md)
* [DeviceInformation クラス](/uwp/api/windows.devices.enumeration.deviceinformation)
* [PosPrinter クラス](/uwp/api/windows.devices.pointofservice.posprinter)
* [PosConnectionTypes 列挙型](/uwp/api/windows.devices.pointofservice.posconnectiontypes)
* [Barているクラス](/uwp/api/windows.devices.pointofservice.barcodescanner)
* [DeviceWatcher クラス](/uwp/api/Windows.Devices.Enumeration.DeviceWatcher)

[!INCLUDE [feedback](./includes/pos-feedback.md)]