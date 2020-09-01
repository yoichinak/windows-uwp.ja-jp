---
title: PointOfService デバイス共有
description: 複数の Pc が共有周辺機器に依存している環境で、ネットワークまたは Bluetooth で接続されている周辺機器を他のコンピューターと共有する方法について説明します。
ms.date: 06/14/2018
ms.topic: article
keywords: Windows 10, UWP, 店舗販売時点管理, POS
ms.localizationpriority: medium
ms.openlocfilehash: 4fad9bc75ed0ff79be1596a3c99445c7e9f97b1f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89163266"
---
# <a name="pointofservice-device-sharing"></a>PointOfService デバイス共有

複数の Pc が各コンピューターに接続された専用の周辺機器ではなく、共有周辺機器に依存している環境で、ネットワークまたは Bluetooth で接続された周辺機器を他のコンピューターと共有する方法について説明します。

## <a name="device-sharing"></a>デバイスの共有

ネットワークと Bluetooth で接続された PointOfService 周辺機器は、通常、複数のクライアントデバイスが1日に同じ周辺機器を共有している環境で使用されます。  ビジー状態の小売または食品サービス環境では、クライアントデバイスが周辺機器に接続する機能の遅延によって、顧客とのトランザクションを終了し、次に進むことができる効率に影響が及びます。 顧客の注文の詳細を料理プリンターとして使用し、準備のために顧客の注文の詳細をキッチンに転送する、迅速なサービスのレストランのシナリオでは、顧客からの注文を受け取る複数のクライアントデバイスが存在することになります。  注文が完了すると、各クライアントデバイスは共有プリンターを要求し、直ちにキッチンの注文を印刷できるようになります。

これらの環境では、別のデバイスが同じデバイスを要求できるように、アプリケーションがデバイスオブジェクトを完全に **破棄** することが重要です。

' Using ' ブロックの最後にある PosPrinter の破棄

```Csharp 
using Windows.Devices.PointOfService;
using(PosPrinter printer = await PosPrinter.FromIdAsync("Device ID"))
{
    if (printer != null)
    {
        // Exercise the printer.
    }

    // When leaving this scope, printer.Dispose() is automatically invoked, 
    // releasing the session we have with the printer.
}
```


Dispose () を明示的に呼び出すことによる PosPrinter の破棄

```Csharp 
using Windows.Devices.PointOfService;

PosPrinter printer = await PosPrinter.FromIdAsync("Device ID");
if (printer != null)
{
    // Exercise the printer, then dispose of the printer explicitly.
    printer.Dispose();
}
```

## <a name="api-methods-used"></a>使用されている API メソッド 

+ [Barの場合](/uwp/api/windows.devices.pointofservice.barcodescanner.dispose) 
+ [CashDrawer](/uwp/api/windows.devices.pointofservice.cashdrawer.dispose) 
+ [LineDisplay。破棄](/uwp/api/windows.devices.pointofservice.linedisplay.dispose) 
+ [MagneticStripeReader](/uwp/api/windows.devices.pointofservice.magneticstripereader.dispose)  
+ [PosPrinter](/uwp/api/windows.devices.pointofservice.posprinter.dispose) 


[!INCLUDE [feedback](./includes/pos-feedback.md)]