---
title: PointOfService device claim と enable model
description: サービスのデバイス信頼性情報を使用し、Api を有効にしてデバイスを要求し、i/o 操作に対して有効にします。
ms.date: 06/19/2018
ms.topic: article
keywords: Windows 10, UWP, 店舗販売時点管理, POS
ms.localizationpriority: medium
ms.openlocfilehash: 1977fd5db2f2e026ae4bbab21de9683f275e96d3
ms.sourcegitcommit: 5d34eb13c7b840c05e5394910a22fa394097dc36
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/28/2020
ms.locfileid: "89053752"
---
# <a name="point-of-service-device-claim-and-enable-model"></a>サービスを使用するデバイスの要求とモデルの有効化

## <a name="claiming-for-exclusive-use"></a>排他的使用を要求しています

PointOfService デバイス オブジェクトを正常に作成したら、入出力にデバイスを使用する前に、デバイスの種類に適切な要求方法を使用して要求する必要があります。  要求により、多くのデバイスの機能に対する排他的アクセスがアプリケーションに付与され、あるアプリケーションが別のアプリケーションによるデバイスの使用を妨げないようにします。  排他的使用のために一度に PointOfService デバイスを要求できるアプリケーションは 1 つだけです。 

> [!Note]
> 要求アクションは、デバイスに対して排他ロックを確立しますが、動作状態にはなりません。  詳細については、「 [i/o 操作用にデバイスを有効にする](#enable-device-for-io-operations) 」を参照してください。

### <a name="apis-used-to-claim--release"></a>要求/リリースに使用される Api

|Device|要求 | リリース | 
|-|:-|:-|
|BarcodeScanner | [Bar、Claimscanを非同期に](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.claimscannerasync) | [ClaimedBarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.close) |
|CashDrawer | [CashDrawer を呼び出す](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.cashdrawer.claimdrawerasync) | [ClaimedCashDrawer](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedcashdrawer.close) | 
|LineDisplay | [LineDisplay. ClaimAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.linedisplay.claimasync) |  [ClaimedineDisplay](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedlinedisplay.close) | 
|MagneticStripeReader | [MagneticStripeReader](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereader.claimreaderasync) |  [ClaimedMagneticStripeReader](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.close) | 
|PosPrinter | [PosPrinter](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter.claimprinterasync) |  [ClaimedPosPrinter](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedposprinter.close) | 
 | 

## <a name="enable-device-for-io-operations"></a>I/o 操作のためにデバイスを有効にする

要求アクションは、単純にデバイスに対する排他的な権限を確立しますが、動作状態にはなりません。  イベントを受信するか、出力操作を実行するには、 **Enableasync**を使用してデバイスを有効にする必要があります。  逆に、 **Disableasync** を呼び出して、デバイスからのイベントのリッスンを停止したり、出力を実行したりすることができます。  **IsEnabled**を使用して、デバイスの状態を確認することもできます。

### <a name="apis-used-enable--disable"></a>有効/無効に使用される Api

| Device | 有効化 | Disable | IsEnabled? |
|-|:-|:-|:-|
|ClaimedBarcodeScanner | [EnableAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.enableasync) | [DisableAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.disableasync) | [IsEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isenabled) | 
|ClaimedCashDrawer | [EnableAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedcashdrawer.enableasync) | [DisableAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedcashdrawer.disableasync) | [IsEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedcashdrawer.isenabled) |
|ClaimedLineDisplay | 該当なし¹ | 該当なし¹ | 該当なし¹ | 
|ClaimedMagneticStripeReader | [EnableAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.enableasync) | [DisableAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.disableasync) | [IsEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.isenabled) |  
|ClaimedPosPrinter | [EnableAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedposprinter.enableasync) | [DisableAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedposprinter.disableasync) | [IsEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedposprinter.isenabled) |
|

¹行の表示では、i/o 操作のためにデバイスを明示的に有効にする必要はありません。  を有効にすると、i/o を実行する PointOfService LineDisplay Api によって自動的に実行されます。

## <a name="code-sample-claim-and-enable"></a>コードサンプル: 要求と有効化

次のサンプルでは、バーコード スキャナー オブジェクトを正常に作成した後で、バーコード スキャナーのデバイスを要求する方法を示しています。

```Csharp

    BarcodeScanner barcodeScanner = await BarcodeScanner.FromIdAsync(DeviceId);

    if(barcodeScanner != null)
    {
        // after successful creation, claim the scanner for exclusive use 
        claimedBarcodeScanner = await barcodeScanner.ClaimScannerAsync();

        if(claimedBarcodeScanner != null)
        {
            // after successful claim, enable scanner for data events to fire
            await claimedBarcodeScanner.EnableAsync();
        }
        else
        {
            Debug.WriteLine("Failure to claim barcodeScanner");
        }
    }
    else
    {
        Debug.WriteLine("Failure to create barcodeScanner object");
    }
    
```

> [!Warning]
> 次のような場合に要求が失われることがあります。
> 1. 別のアプリで同じデバイスの要求がリクエストされ、アプリが **ReleaseDeviceRequested** イベントへの応答として **RetainDevice** を発行しなかった   (詳細については、以下の「[要求のネゴシエーション](#claim-negotiation)」を参照してください)。
> 2. アプリが中断され、その結果としてデバイス オブジェクトが終了し、結果的に要求が有効ではなくなった  (詳細については、「[デバイス オブジェクトのライフサイクル](pos-basics-deviceobject.md#device-object-lifecycle)」を参照してください)。


## <a name="claim-negotiation"></a>要求のネゴシエーション

Windows はマルチタスク環境であるため、同じコンピューター上の複数のアプリケーションで連携して周辺機器にアクセスする必要がある可能性があります。  PointOfService API は、コンピューターに接続されている周辺機器を複数のアプリケーションが共有できるようにするネゴシエーション モデルを提供します。

同じコンピューター上の 2 番目のアプリケーションが別のアプリケーションによって既に要求された PointOfService 周辺機器への要求をリクエストすると、**ReleaseDeviceRequested**イベント通知が発行されます。 アクティブな要求のあるアプリケーションは、そのアプリケーションが現在デバイスを使用している場合は、要求を失うことを避けるために **RetainDevice** を呼び出してイベント通知に応答する必要があります。 

アクティブな要求を持つアプリケーションが **RetainDevice** ですぐに応答しない場合は、アプリケーションが中断されたか、またはデバイスが不要であると見なされ、要求が取り消されて新しいアプリケーションに渡されます。 

最初の手順では、 **RetainDevice**で**ReleaseDeviceRequested**イベントに応答するイベントハンドラーを作成します。  

```Csharp
    /// <summary>
    /// Event handler for the ReleaseDeviceRequested event which occurs when 
    /// the claimed barcode scanner receives a Claim request from another application
    /// </summary>
    void claimedBarcodeScanner_ReleaseDeviceRequested(object sender, ClaimedBarcodeScanner myScanner)
    {
        // Retain exclusive access to the device
        myScanner.RetainDevice();
    }
```

次に、要求されたデバイスとの関連付けでイベントハンドラーを登録します。

```Csharp
    BarcodeScanner barcodeScanner = await BarcodeScanner.FromIdAsync(DeviceId);

    if(barcodeScanner != null)
    {
        // after successful creation, claim the scanner for exclusive use 
        claimedBarcodeScanner = await barcodeScanner.ClaimScannerAsync();

        if(claimedBarcodeScanner != null)
        {
            // register a release request handler to prevent loss of scanner during active use
            claimedBarcodeScanner.ReleaseDeviceRequested += claimedBarcodeScanner_ReleaseDeviceRequested;

            // after successful claim, enable scanner for data events to fire
            await claimedBarcodeScanner.EnableAsync();          
        }
        else
        {
            Debug.WriteLine("Failure to claim barcodeScanner");
        }
    }
    else
    {
        Debug.WriteLine("Failure to create barcodeScanner object");
    }
```



### <a name="apis-used-for-claim-negotiation"></a>要求のネゴシエーションに使用される API

|要求されたデバイス|リリースの通知| デバイスの保持 |
|-|:-|:-|
|ClaimedBarcodeScanner | [ReleaseDeviceRequested](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.releasedevicerequested) | [RetainDevice](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.retaindevice)
|ClaimedCashDrawer | [ReleaseDeviceRequested](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedcashdrawer.releasedevicerequested) | [RetainDevice](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedcashdrawer.retaindevice)
|ClaimedLineDisplay | [ReleaseDeviceRequested](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedlinedisplay.releasedevicerequested) | [RetainDevice](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedlinedisplay.retaindevice)
|ClaimedMagneticStripeReader | [ReleaseDeviceRequested](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.releasedevicerequested) | [RetainDevice](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedlinedisplay.retaindevice)
|ClaimedPosPrinter | [ReleaseDeviceRequested](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedposprinter.releasedevicerequested) | [RetainDevice](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedposprinter.retaindevice)
|
