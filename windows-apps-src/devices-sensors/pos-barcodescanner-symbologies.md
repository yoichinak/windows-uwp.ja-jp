---
title: バーコード スキャナーのシンボル体系の使用
description: UWP バーコードスキャナー Api を使用して、手動でスキャナーを構成せずにバーコード symbologies を処理します。
ms.date: 08/29/2018
ms.topic: article
keywords: Windows 10, UWP, 店舗販売時点管理, POS
ms.localizationpriority: medium
ms.openlocfilehash: a556178adcb7c2188787c04318a1eed4a3c35df4
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89159706"
---
# <a name="working-with-symbologies"></a>シンボル体系の操作
[バーコード シンボル体系](/uwp/api/windows.devices.pointofservice.barcodesymbologies)は、データと特定のバーコード形式のマッピングです。 いくつかの一般的な symbologies には、UPC、コード128、QR コードなどが含まれています。  ユニバーサル Windows プラットフォームバーコードスキャナ Api を使用すると、スキャナーを手動で構成することなく、スキャナーがこれらの symbologies をどのように処理するかをアプリケーションで制御できます。 

## <a name="determine-which-symbologies-are-supported"></a>サポートされているシンボル体系を判断する 
複数の製造元から提供されるさまざまなバーコード スキャナーのモデルでアプリケーションを使用できるようにするために、スキャナーに対してクエリを実行して、サポートされているシンボル体系のリストを特定することができます。  これは、アプリケーションがすべてのスキャナーでサポートされていない可能性がある特定のシンボル体系を必要とする場合や、スキャナーで手動またはプログラムによって無効になっているシンボル体系を有効にする必要がある場合に便利です。

[BarcodeScanner.FromIdAsync](/uwp/api/windows.devices.pointofservice.barcodescanner.fromidasync) を使用して [BarcodeScanner](/uwp/api/windows.devices.pointofservice.barcodescanner) オブジェクトを取得した後で、[GetSupportedSymbologiesAsync](/uwp/api/windows.devices.pointofservice.barcodescanner.getsupportedsymbologiesasync#Windows_Devices_PointOfService_BarcodeScanner_GetSupportedSymbologiesAsync) を呼び出して、デバイスでサポートされているシンボル体系のリストを取得します。

次の例では、バーコードスキャナーのサポートされている symbologies の一覧を取得し、テキストブロックに表示します。

```cs
private void DisplaySupportedSymbologies(BarcodeScanner barcodeScanner, TextBlock textBlock) 
{
    var supportedSymbologies = await barcodeScanner.GetSupportedSymbologiesAsync();

    foreach (uint item in supportedSymbologies)
    {
        string symbology = BarcodeSymbologies.GetName(item);
        textBlock.Text += (symbology + "\n");
    }
}
```

## <a name="determine-if-a-specific-symbology-is-supported"></a>特定のシンボル体系がサポートされているかどうかを判断する
スキャナーが特定のシンボルをサポートしているかどうかを判断するには、 [IsSymbologySupportedAsync](/uwp/api/windows.devices.pointofservice.barcodescanner.issymbologysupportedasync#Windows_Devices_PointOfService_BarcodeScanner_IsSymbologySupportedAsync_System_UInt32_)を呼び出すことができます。

次の例では、バーコードスキャナーで **Code32** シンボルがサポートされているかどうかを確認します。

```cs
bool symbologySupported = await barcodeScanner.IsSymbologySupportedAsync(BarcodeSymbologies.Code32);
```

## <a name="change-which-symbologies-are-recognized"></a>認識される symbologies を変更する
場合によっては、バーコード スキャナーがサポートするシンボル体系のサブセットを使用することもできます。  これは、アプリケーションで使用する予定のないシンボル体系をブロックする場合に特に便利です。 たとえば、ユーザーが適切なバーコードをスキャンできるように、アイテムの SKU を取得するときにはスキャンを UPC または EAN に制限し、シリアル番号を取得するときにはスキャンを Code 128 に制限することができます。

スキャナーがサポートするシンボル体系がわかったら、スキャナーで認識するシンボル体系を設定できます。  これは、 [ClaimscanClaimedBarcodeScanner](/uwp/api/windows.devices.pointofservice.barcodescanner.claimscannerasync#Windows_Devices_PointOfService_BarcodeScanner_ClaimScannerAsync)を使用し[ClaimedBarcodeScanner](/uwp/api/windows.devices.pointofservice.claimedbarcodescanner)て、オブジェクトを確立した後に行うことができます。 [SetActiveSymbologiesAsync](/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.setactivesymbologiesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_SetActiveSymbologiesAsync_Windows_Foundation_Collections_IIterable_System_UInt32__) を呼び出して特定のシンボル体系のセットを有効にすることができます。リストから省略したシンボル体系は無効になります。

次の例では、要求されたバーコードスキャナーのアクティブな symbologies を [Code39](/uwp/api/windows.devices.pointofservice.barcodesymbologies.code39#Windows_Devices_PointOfService_BarcodeSymbologies_Code39) と [Code39Ex](/uwp/api/windows.devices.pointofservice.barcodesymbologies.code39ex)に設定します。

```cs
private async void SetSymbologies(ClaimedBarcodeScanner claimedBarcodeScanner) 
{
    var symbologies = new List<uint>{ BarcodeSymbologies.Code39, BarcodeSymbologies.Code39Ex };
    await claimedBarcodeScanner.SetActiveSymbologiesAsync(symbologies);
}
```

## <a name="barcode-symbology-attributes"></a>バーコードのシンボル属性
さまざまなバーコード symbologies には、複数のデコード長のサポート、生データの一部としてのホストへのチェックディジットの送信、およびチェックディジット検証など、さまざまな属性を設定できます。 [BarClaimedBarcodeScanner Ymbologyattributes](/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes)クラスを使用すると、特定の[ClaimedBarcodeScanner](/uwp/api/windows.devices.pointofservice.claimedbarcodescanner)およびバーコードシンボルに対してこれらの属性を取得および設定できます。

[GetSymbologyAttributesAsync](/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.getsymbologyattributesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_GetSymbologyAttributesAsync_System_UInt32_)を使用して、指定したシンボルの属性を取得できます。 次のコードスニペットは、 **ClaimedBarcodeScanner**の upca シンボルの属性を取得します。

```cs
BarcodeSymbologyAttributes barcodeSymbologyAttributes = 
    await claimedBarcodeScanner.GetSymbologyAttributesAsync(BarcodeSymbologies.Upca);
```

属性の変更が完了し、設定する準備ができたら、 [SetSymbologyAttributesAsync](/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.setsymbologyattributesasync)を呼び出すことができます。 このメソッドは **ブール**値を返します。これは、属性が正常に設定された場合に **true** になります。

```cs
bool success = await claimedBarcodeScanner.SetSymbologyAttributesAsync(
    BarcodeSymbologies.Upca, barcodeSymbologyAttributes);
```

### <a name="restrict-scan-data-by-data-length"></a>データの長さでスキャンデータを制限する
一部のシンボル体系 (Code 39 や Code 128 など) は可変長です。  これらの symbologies のバーコードは、特定の長さの異なるデータを含む互いに近くに配置できます。 必要なデータの特定の長さを設定することで、無効なスキャンを防止できます。

デコードの長さを設定する前に、バーコードシンボルが [IsDecodeLengthSupported](/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.isdecodelengthsupported#Windows_Devices_PointOfService_BarcodeSymbologyAttributes_IsDecodeLengthSupported)で複数の長さをサポートしているかどうかを確認します。 サポートされていることがわかったら、 [BarcodeSymbologyDecodeLengthKind](/uwp/api/windows.devices.pointofservice.barcodesymbologydecodelengthkind)型の[DecodeLengthKind](/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.decodelengthkind#Windows_Devices_PointOfService_BarcodeSymbologyAttributes_DecodeLengthKind)を設定できます。 このプロパティには、次のいずれかの値を指定できます。

* **Anylength**: 任意の数値の長さをデコードします。
* **不連続**: [DecodeLength1](/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.decodelength1) または [DecodeLength2](/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.decodelength2) の1バイト文字の長さをデコードします。
* **Range**: **DecodeLength1** と **DecodeLength2** の1バイト文字間の長さをデコードします。 **DecodeLength1**と**DecodeLength2**の順序は関係ありません (どちらも上位または下位のいずれかになります)。

最後に、 **DecodeLength1** と **DecodeLength2** の値を設定して、必要なデータの長さを制御できます。

次のコードスニペットは、デコードの長さを設定する方法を示しています。

```cs
private async Task<bool> SetDecodeLength(
    ClaimedBarcodeScanner scanner,
    uint symbology, 
    BarcodeSymbologyDecodeLengthKind kind, 
    uint decodeLength1, 
    uint decodeLength2)
{
    bool success = false;
    BarcodeSymbologyAttributes attributes = await scanner.GetSymbologyAttributesAsync(symbology);

    if (attributes.IsDecodeLengthSupported)
    {
        attributes.DecodeLengthKind = kind;
        attributes.DecodeLength1 = decodeLength1;
        attributes.DecodeLength2 = decodeLength2;
        success = await scanner.SetSymbologyAttributesAsync(symbology, attributes);
    }

    return success;
}
```

### <a name="check-digit-transmission"></a>チェックディジット転送

シンボルに対して設定できるもう1つの属性は、チェックディジットが生データの一部としてホストに送信されるかどうかです。 これを設定する前に、コードが [IsCheckDigitTransmissionSupported](/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigittransmissionsupported)を使用したチェックディジットの送信をサポートしていることを確認してください。 次に、 [IsCheckDigitTransmissionEnabled](/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigittransmissionenabled)でチェックディジット転送を有効にするかどうかを設定します。

次のコードスニペットは、チェックディジット転送を設定する方法を示しています。

```cs
private async Task<bool> SetCheckDigitTransmission(ClaimedBarcodeScanner scanner, uint symbology, bool isEnabled)
{
    bool success = false;
    BarcodeSymbologyAttributes attributes = await scanner.GetSymbologyAttributesAsync(symbology);

    if (attributes.IsCheckDigitTransmissionSupported)
    {
        attributes.IsCheckDigitTransmissionEnabled = isEnabled;
        success = await scanner.SetSymbologyAttributesAsync(symbology, attributes);
    }

    return success;
}
```

### <a name="check-digit-validation"></a>チェックディジット検証

バーコードのチェックディジットを検証するかどうかを設定することもできます。 これを設定する前に、コードが [IsCheckDigitValidationSupported](/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigitvalidationsupported)を使用してチェックディジット検証をサポートしていることを確認してください。 次に、 [IsCheckDigitValidationEnabled](/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigitvalidationenabled)でチェックディジット検証を有効にするかどうかを設定します。

次のコードスニペットは、チェックディジット検証の設定を示しています。

```cs
private async Task<bool> SetCheckDigitValidation(ClaimedBarcodeScanner scanner, uint symbology, bool isEnabled)
{
    bool success = false;
    BarcodeSymbologyAttributes attributes = await scanner.GetSymbologyAttributesAsync(symbology);

    if (attributes.IsCheckDigitValidationSupported)
    {
        attributes.IsCheckDigitValidationEnabled = isEnabled;
        success = await scanner.SetSymbologyAttributesAsync(symbology, attributes);
    }

    return success;
}
```

[!INCLUDE [feedback](./includes/pos-feedback.md)]

## <a name="see-also"></a>関連項目

* [バーコード スキャナー](pos-barcodescanner.md)
* [BarcodeSymbologies クラス](/uwp/api/windows.devices.pointofservice.barcodesymbologies)
* [Barているクラス](/uwp/api/windows.devices.pointofservice.barcodescanner)
* [ClaimedBarcodeScanner クラス](/uwp/api/windows.devices.pointofservice.claimedbarcodescanner)
* [Barのシンボル Ymbologyattributes クラス](/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes)
* [BarcodeSymbologyDecodeLengthKind 列挙型](/uwp/api/windows.devices.pointofservice.barcodesymbologydecodelengthkind)