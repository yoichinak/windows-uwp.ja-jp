---
title: カメラ バーコード スキャナーの概要
description: これらのステップバイステップの手順とコードスニペットを使用して、カメラバーコードスキャナーを使い始めることができます。
ms.date: 09/02/2019
ms.topic: article
keywords: Windows 10, UWP, 店舗販売時点管理, POS
ms.localizationpriority: medium
ms.openlocfilehash: f7206250b2495ae2c905d2aece2b8cd1b85d6859
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89168476"
---
# <a name="getting-started-with-a-camera-barcode-scanner"></a>カメラ バーコード スキャナーの概要

ここで使用するスニペットは、デモンストレーションのみを目的としています。 実際のサンプルについては、 [バーコードスキャナーのサンプル](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)を参照してください。

## <a name="step-1-add-capability-declarations-to-your-app-manifest"></a>手順 1: アプリ マニフェストに機能宣言を追加する

1. Microsoft Visual Studio の**ソリューション エクスプローラー**で、**package.appxmanifest** 項目をダブルクリックしてアプリケーション マニフェストのデザイナーを開きます。
2. [ **機能** ] タブを選択します。
3. **[Web カメラ]** と **[PointOfService]** のチェック ボックスをオンにします。

>[!NOTE]
> **Web カメラ**機能は、アプリケーションからプレビューを表示するだけでなく、ソフトウェア デコーダーでカメラからデコードするフレームを受信するためにも必要です。

## <a name="step-2-add-using-directives"></a>手順 2: using ディレクティブを追加する

```Csharp
using Windows.Devices.Enumeration;
using Windows.Devices.PointOfService;
```

## <a name="step-3-define-your-device-selector"></a>手順 3: デバイス セレクターを定義する

### <a name="option-a-find-all-barcode-scanners"></a>**オプション A: すべてのバーコード スキャナーを検索する**

```Csharp
string selector = BarcodeScanner.GetDeviceSelector();
```

### <a name="option-b-scoping-device-selector-to-connection-type"></a>**オプション B: デバイス セレクターで接続の種類を制限する**

```Csharp
string selector = BarcodeScanner.GetDeviceSelector(PosConnectionTypes.Local);
DeviceInformationCollection deviceCollection = await DeviceInformation.FindAllAsync(selector);
```

## <a name="step-4-enumerate-all-barcode-scanners"></a>手順 4: すべてのバーコードスキャナーを列挙する

アプリケーションの有効期間中にデバイスの一覧が変化することが予想されない場合は、Deviceinformation を使用して1回だけスナップショットを列挙することができ [ます。](/uwp/api/windows.devices.enumeration.deviceinformation.findallasync)ただし、アプリケーションの有効期間中にバーコードスキャナーの一覧が変更される可能性がある場合は、代わりに [deviceinformation](/uwp/api/windows.devices.enumeration.devicewatcher) を使用する必要があります。  

> [!Important]
> GetDefaultAsync を使用して PointOfService デバイスを列挙すると、結果の動作が一貫しない可能性があります。GetDefaultAsync は、クラスで見つかった最初のデバイスを返すだけであり、このデバイスはセッションによって変化する可能性があるためです。

### <a name="option-a-enumerate-a-snapshot-of-barcode-scanners"></a>**オプション A: バーコード スキャナーのスナップショットを列挙する**

```Csharp
DeviceInformationCollection deviceCollection = await DeviceInformation.FindAllAsync(selector);
```

> [!TIP]
> *FindAllAsync* の使用方法の詳細については、「[*デバイスのスナップショットの列挙*](./enumerate-devices.md#enumerate-a-snapshot-of-devices)」を参照してください。

### <a name="option-b-enumerate-available-barcode-scanners-and-watch-for-changes-to-the-available-scanners"></a>**オプション B: 使用可能なバーコードスキャナーを列挙し、使用可能なスキャナーの変更を監視する**

```Csharp
DeviceWatcher deviceWatcher = DeviceInformation.CreateWatcher(selector);
watcher.Added += Watcher_Added;
watcher.Removed += Watcher_Removed;
watcher.Updated += Watcher_Updated;
watcher.Start();
```

> [!TIP]
> 詳細については、「[*デバイスの列挙と監視*](./enumerate-devices.md#enumerate-and-watch-devices)」と「[*DeviceWatcher*](/uwp/api/Windows.Devices.Enumeration.DeviceWatcher)」を参照してください。

## <a name="step-5-identify-camera-barcode-scanners"></a>手順 5: カメラ バーコード スキャナーの識別

カメラ バーコード スキャナーは、Windows によって、コンピューターに接続されているカメラとソフトウェア デコーダーがペアリングされたときに動的に作成されます。  カメラとデコーダーのペアはそれぞれ完全な機能を持つバーコード スキャナーです。

結果として得られるデバイスコレクション内のバーコードスキャナーごとに、Barコードのスキャナーと物理的なバーコードスキャナーを区別できます。これを行うには、 [*Bardeviceid*](/uwp/api/windows.devices.pointofservice.barcodescanner.videodeviceid#Windows_Devices_PointOfService_BarcodeScanner_VideoDeviceId) プロパティを調べます。  NULL でない VideoDeviceID は、デバイスコレクションのバーコードスキャナーオブジェクトがカメラバーコードスキャナーであることを示します。  複数のカメラバーコードスキャナーがある場合は、物理的なバーコードスキャナーを除外する個別のコレクションを作成することをお勧めします。

Windows に付属しているデコーダーを使用したカメラバーコードスキャナーは、次のように識別されます。

> Microsoft BarcodeScanner (*使用しているカメラの名前*)

複数のカメラがあり、コンピューターのシャーシに組み込まれている場合は、 *前面* と *背面* のカメラが区別されることがあります。

> [!NOTE]
> 今後、名前付けスキームが異なる追加のソフトウェアデコーダーがリリースされる可能性があります。

DeviceWatcher が開始されると (手順 4)、接続されている各デバイスを列挙します。 ここで、利用可能なスキャナーをバーコードスキャナーコレクションに追加し、コレクションを ListBox にバインドします。

```csharp
ObservableCollection<BarcodeScannerInfo> barcodeScanners = new ObservableCollection<BarcodeScannerInfo>();

private async void Watcher_Added(DeviceWatcher sender, DeviceInformation args)
{
    await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
    {
        barcodeScanners.Add(new BarcodeScannerInfo(args.Name, args.Id));

        // Select the first scanner by default.
        if (barcodeScanners.Count == 1)
        {
            ScannerListBox.SelectedIndex = 0;
        }
    });
}
```

リストボックスの SelectedIndex が変更された場合 (前のスニペットでは最初の項目が既定で選択されています)、デバイス情報に対してクエリを実行します。

```csharp
private async void ScannerSelection_Changed(object sender, SelectionChangedEventArgs args)
{
    var selectedScannerInfo = (BarcodeScannerInfo)args.AddedItems[0];
    var deviceId = selectedScannerInfo.DeviceId;

    await SelectScannerAsync(deviceId);
}
```

## <a name="step-6-claim-the-camera-barcode-scanner"></a>手順 6: カメラ バーコード スキャナーを要求する

[BarcodeScanner.ClaimScannerAsync](/uwp/api/windows.devices.pointofservice.barcodescanner.claimscannerasync#Windows_Devices_PointOfService_BarcodeScanner_ClaimScannerAsync) を使用して、カメラ バーコード スキャナーの排他的使用を取得します。

```csharp
private async Task SelectScannerAsync(string scannerDeviceId)
{
    selectedScanner = await BarcodeScanner.FromIdAsync(scannerDeviceId);

    if (selectedScanner != null)
    {
        claimedScanner = await selectedScanner.ClaimScannerAsync();
        if (claimedScanner != null)
        {
            await claimedScanner.EnableAsync();
        }
        else
        {
            rootPage.NotifyUser("Failed to claim the selected barcode scanner", NotifyType.ErrorMessage);
        }
    }
    else
    {
        rootPage.NotifyUser("Failed to create a barcode scanner object", NotifyType.ErrorMessage);
    }
}
```

## <a name="step-7-system-provided-preview"></a>手順 7: システムが提供するプレビュー

ユーザーがカメラを正しくバーコードに向けるには、カメラ プレビューが必要です。  Windows には、カメラバーコードスキャナーの基本的なコントロールのダイアログを起動する単純なカメラプレビューが用意されています。  ダイアログを開くときは [ClaimedBarcodeScanner.ShowVideoPreview](/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.showvideopreviewasync) を呼び出し、作業が終わってダイアログを閉じるときは [ClaimedBarcodeScanner.HideVideoPreview](/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.hidevideopreview) を呼び出すだけです。

> [!TIP]
> アプリケーションでカメラ バーコード スキャナーのプレビューをホストする方法については、「[プレビューのホスト](pos-camerabarcode-hosting-preview.md)」を参照してください。

## <a name="step-8-initiate-scan"></a>手順 8: スキャンを開始する

[**StartSoftwareTriggerAsync**](/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.startsoftwaretriggerasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_StartSoftwareTriggerAsync) を呼び出すことでスキャン プロセスを開始できます。

[**IsDisabledOnDataReceived**](/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdisabledondatareceived#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDisabledOnDataReceived)の値によっては、スキャナーが1つのバーコードだけをスキャンし、 [**StopSoftwareTriggerAsync**](/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.stopsoftwaretriggerasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_StopSoftwareTriggerAsync)を呼び出すまで継続的に停止またはスキャンすることがあります。

[**IsDisabledOnDataReceived**](/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdisabledondatareceived#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDisabledOnDataReceived) を目的の値に設定することで、バーコードがデコードされたときのスキャナー動作を制御します。

| 値 | 説明 |
| ----- | ----------- |
| True   | バーコードを 1 つだけスキャンして停止する |
| False  | 停止せずに継続的にバーコードをスキャンする |

## <a name="see-also"></a>関連項目

### <a name="samples"></a>サンプル

- [バーコード スキャナーのサンプル](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)