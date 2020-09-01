---
title: バーコード データの取得と理解
description: バーコードスキャナーのデータを Barのレポートオブジェクトで取得し、その形式と内容を理解する方法について説明します。
ms.date: 08/29/2018
ms.topic: article
keywords: Windows 10, UWP, 店舗販売時点管理, POS
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 70226b45a50e0d340c3de316902033d5563106d2
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89163286"
---
# <a name="obtain-and-understand-barcode-data"></a>バーコード データの取得と理解

バーコードスキャナーを設定したら、スキャンするデータを理解する方法が必要になります。 バーコードをスキャンすると、 [Datareceived](/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.datareceived) イベントが発生します。 [ClaimedBarcodeScanner](/uwp/api/windows.devices.pointofservice.claimedbarcodescanner)はこのイベントをサブスクライブする必要があります。 **Datareceived**イベントは、バーコードデータにアクセスするために使用できる[BarcodeScannerDataReceivedEventArgs](/uwp/api/windows.devices.pointofservice.barcodescannerdatareceivedeventargs)オブジェクトを渡します。

## <a name="subscribe-to-the-datareceived-event"></a>DataReceived イベントをサブスクライブする

**ClaimedBarcodeScanner**を取得したら、 **datareceived**イベントをサブスクライブします。

```cs
claimedBarcodeScanner.DataReceived += ClaimedBarcodeScanner_DataReceived;
```

イベントハンドラーには、 **ClaimedBarcodeScanner** および **BarcodeScannerDataReceivedEventArgs** オブジェクトが渡されます。 バーコードデータには、このオブジェクトの [レポート](/uwp/api/windows.devices.pointofservice.barcodescannerdatareceivedeventargs.report#Windows_Devices_PointOfService_BarcodeScannerDataReceivedEventArgs_Report) プロパティを使用してアクセスできます。このプロパティは、 [barコード](/uwp/api/windows.devices.pointofservice.barcodescannerreport)によるレポートの種類です。

```cs
private async void ClaimedBarcodeScanner_DataReceived(ClaimedBarcodeScanner sender, BarcodeScannerDataReceivedEventArgs args)
{
    // Parse the data
}
```

## <a name="get-the-data"></a>データを取得する

**Barコードをレポート**すると、バーコードデータにアクセスして解析できるようになります。 **Barているレポート** には、次の3つのプロパティがあります。

* [Scandata](/uwp/api/windows.devices.pointofservice.barcodescannerreport.scandata): 未加工の完全なバーコードデータ。
* [ScanDataLabel](/uwp/api/windows.devices.pointofservice.barcodescannerreport.scandatalabel): デコードされたバーコードラベル。ヘッダー、チェックサムなどのその他の情報は含まれません。
* [Scandatatype](/uwp/api/windows.devices.pointofservice.barcodescannerreport.scandatatype): デコードされたバーコードの種類。 指定できる値は、 [BarcodeSymbologies](/uwp/api/windows.devices.pointofservice.barcodesymbologies) クラスで定義されています。

**ScanDataLabel**または**scandatatype**にアクセスするには、最初に[IsDecodeDataEnabled](/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdecodedataenabled#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDecodeDataEnabled)を**true**に設定する必要があります。

```cs
claimedBarcodeScanner.IsDecodeDataEnabled = true;
```

### <a name="get-the-scan-data-type"></a>スキャンデータの種類を取得する

デコードされたバーコードの種類を取得するのは非常に簡単ですが、 &mdash; **scandatatype**で[GetName](/uwp/api/windows.devices.pointofservice.barcodesymbologies.getname)を呼び出します。

```cs
private string GetSymbology(BarcodeScannerDataReceivedEventArgs args)
{
    return BarcodeSymbologies.GetName(args.Report.ScanDataType);
}
```

### <a name="get-the-scan-data-label"></a>スキャンデータラベルを取得する

デコードされたバーコードラベルを取得するには、注意が必要な点がいくつかあります。 エンコードされたテキストを含むのは特定のデータ型のみなので、まず、コードを文字列に変換できるかどうかを確認し、 **ScanDataLabel** から取得したバッファーをエンコード済みの utf-8 文字列に変換する必要があります。

```cs
private string GetDataLabel(BarcodeScannerDataReceivedEventArgs args)
{
    uint scanDataType = args.Report.ScanDataType;

    // Only certain data types contain encoded text.
    // To keep this simple, we'll just decode a few of them.
    if (args.Report.ScanDataLabel == null)
    {
        return "No data";
    }

    // This is not an exhaustive list of symbologies that can be converted to a string.
    else if (scanDataType == BarcodeSymbologies.Upca ||
        scanDataType == BarcodeSymbologies.UpcaAdd2 ||
        scanDataType == BarcodeSymbologies.UpcaAdd5 ||
        scanDataType == BarcodeSymbologies.Upce ||
        scanDataType == BarcodeSymbologies.UpceAdd2 ||
        scanDataType == BarcodeSymbologies.UpceAdd5 ||
        scanDataType == BarcodeSymbologies.Ean8 ||
        scanDataType == BarcodeSymbologies.TfStd)
    {
        // The UPC, EAN8, and 2 of 5 families encode the digits 0..9
        // which are then sent to the app in a UTF8 string (like "01234").
        return CryptographicBuffer.ConvertBinaryToString(BinaryStringEncoding.Utf8, args.Report.ScanDataLabel);
    }

    // Some other symbologies (typically 2-D symbologies) contain binary data that
    // should not be converted to text.
    else
    {
        return "Decoded data unavailable.";
    }
}
```

### <a name="get-the-raw-scan-data"></a>生のスキャンデータを取得する

バーコードから生データ全体を取得するには、 **スキャンデータ** から取得したバッファーを文字列に変換するだけです。

```cs
private string GetRawData(BarcodeScannerDataReceivedEventArgs args)
{
    // Get the full, raw barcode data.
    if (args.Report.ScanData == null)
    {
        return "No data";
    }

    // Just to show that we have the raw data, we'll print the value of the bytes.
    else
    {
        return CryptographicBuffer.ConvertBinaryToString(BinaryStringEncoding.Utf8, args.Report.ScanData);
    }
}
```

これらのデータは、通常、スキャナーから配信される形式で指定します。 ただし、メッセージヘッダーとトレーラーの情報は、アプリケーションに関する有用な情報が含まれておらず、スキャナー固有のものである可能性があるため、削除されます。

共通ヘッダー情報はプレフィックス文字 (STX 文字など) です。 一般的なトレーラー情報は、ターミネータ文字 (ETX や CR 文字など) とブロックチェック文字 (スキャナーによって生成された場合) です。

スキャナーによって返される場合は、このプロパティにシンボル文字を含める必要があります (たとえば、UPC-A の **a** )。 また、ラベルに含まれており、スキャナーによって返される場合は、チェックディジットも含まれている必要があります。 (スキャナーの構成によっては、記号と数字の両方が表示されることがあります。 スキャナーからは、存在する場合はそれらが返されますが、存在しない場合は生成または計算されません)。

補助バーコードでマークされている商品もあります。 通常、このバーコードはメインバーの右側に配置され、2文字または5文字の情報が追加されます。 スキャナーが主要なバーコードと補助バーコードの両方が含まれている商品を読み取ると、補助文字がメイン文字に追加され、結果が1つのラベルとしてアプリケーションに配信されます。 (スキャナーでは、補足コードの読み取りを有効または無効にする構成がサポートされている場合があります)。

複数のラベルでマークされている商品もあります ( *multisymbol ラベル* や *階層化ラベル*と呼ばれることもあります)。 通常、これらのバーコードは垂直方向に配置され、同じ記号または異なる記号を使用できます。 複数のラベルが含まれている商品をスキャナーが読み取った場合、各バーコードは個別のラベルとしてアプリケーションに配信されます。 これは、このようなバーコードの種類が現在標準化されていないために必要です。 1つは、個々のバーコードデータに基づいてすべてのバリエーションを判別できないことです。 そのため、返されたデータに基づいて複数のラベルのバーコードが読み取られたことをアプリケーションで確認する必要があります。 (スキャナーは複数のラベルの読み取りをサポートしている場合もあれば、サポートされない場合もあります)。

この値は、アプリケーションに対して **Datareceived** イベントが発生する前に設定されます。

[!INCLUDE [feedback](./includes/pos-feedback.md)]

## <a name="see-also"></a>関連項目
* [バーコード スキャナー](pos-barcodescanner.md)
* [ClaimedBarcodeScanner クラス](/uwp/api/windows.devices.pointofservice.barcodesymbologies.getname)
* [BarcodeScannerDataReceivedEventArgs クラス](/uwp/api/windows.devices.pointofservice.barcodescannerdatareceivedeventargs)
* [Barの場合はレポートクラス](/uwp/api/windows.devices.pointofservice.barcodescannerreport)
* [BarcodeSymbologies クラス](/uwp/api/windows.devices.pointofservice.barcodesymbologies)