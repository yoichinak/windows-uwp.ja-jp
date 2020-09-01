---
ms.assetid: B4A550E7-1639-4C9A-A229-31E22B1415E7
title: センサーの向き
description: Accelerometer、Gyrometer、Compass、Inclinometer、および OrientationSensor の各クラスのセンサー データは、基準軸によって定義されます。 これらの軸はデバイスの横長の向きで定義され、ユーザーがデバイスの向きを変えると、デバイスと共に回転します。
ms.date: 07/03/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 8836753778b1dd5dcbc8856b0df5ec1f11d8e753
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89159806"
---
# <a name="sensor-orientation"></a>センサーの向き

[**加速度計**](/uwp/api/Windows.Devices.Sensors.Accelerometer)、[**ジャイロ**](/uwp/api/Windows.Devices.Sensors.Gyrometer)、[**コンパス**](/uwp/api/Windows.Devices.Sensors.Compass)、 [**Inclinometer**](/uwp/api/Windows.Devices.Sensors.Inclinometer)、 [**OrientationSensor**](/uwp/api/Windows.Devices.Sensors.OrientationSensor)の各クラスのセンサーデータは、その参照軸によって定義されます。 これらの軸はデバイスの参照フレームで定義され、ユーザーがデバイスの向きを変えると、デバイスと共に回転します。 アプリが自動回転をサポートしており、ユーザーがデバイスを回転させたときに連動して向きが変わる場合、センサー データを使う前に回転に合わせて調整する必要があります。

### <a name="important-apis"></a>重要な API

- [**Windows.Devices.Sensors**](/uwp/api/Windows.Devices.Sensors)
- [**Windows. デバイス... カスタム**](/uwp/api/Windows.Devices.Sensors.Custom)

## <a name="display-orientation-vs-device-orientation"></a>表示の向きとデバイスの向き

センサーの基準軸について理解するために、画面の向きとデバイスの向きを区別する必要があります。 画面の向きはテキストの向きであり、画面上に画像が表示されます。それに対してデバイスの向きは、デバイスの実際の配置です。

> [!NOTE]
> 正の z 軸は、次の図に示すように、デバイス画面から拡張されます。
> :::image type="content" source="images/sensor-orientation-zaxis-1-small.png" alt-text="ラップトップの Z 軸":::

次の図では、デバイスと画面の向きの両方が [横向き](/uwp/api/Windows.Graphics.Display.DisplayOrientations) になっています (表示されるセンサー軸は横向きに固有です)。


この図は、表示とデバイスの向きを [横向き](/uwp/api/Windows.Graphics.Display.DisplayOrientations)で示しています。

:::image type="content" source="images/sensor-orientation-a-small.jpg" alt-text="画面とデバイスの向きが横向き":::

次の図は、 [LandscapeFlipped](/uwp/api/Windows.Graphics.Display.DisplayOrientations)の表示とデバイスの向きの両方を示しています。

:::image type="content" source="images/sensor-orientation-b-small.jpg" alt-text="画面とデバイスの向きが LandscapeFlipped":::

この最後の図は、デバイスの向きは [LandscapeFlipped](/uwp/api/Windows.Graphics.Display.DisplayOrientations)ですが、横方向の画面の向きを示しています。

:::image type="content" source="images/sensor-orientation-c-small.jpg" alt-text="画面の向きが Landscape、デバイスの向きが LandscapeFlipped です。":::

向きの値は、[**DisplayInformation**](/uwp/api/Windows.Graphics.Display.DisplayInformation) クラスの [**GetForCurrentView**](/uwp/api/windows.graphics.display.displayinformation.getforcurrentview) メソッドと [**CurrentOrientation**](/uwp/api/windows.graphics.display.displayinformation.currentorientation) プロパティを使って照会することができます。 次に、[**DisplayOrientations**](/uwp/api/Windows.Graphics.Display.DisplayOrientations) 列挙値と比較することによってロジックを作成できます。 サポートするすべての向きについて、その向きへの基準軸の変換をサポートする必要があることに注意してください。

## <a name="landscape-first-vs-portrait-first-devices"></a>横向き優先デバイスと縦向き優先デバイス

製造元は、横向き優先および縦向き優先のいずれのデバイスも製造します。 参照フレームは、横向き優先デバイス (デスクトップやノート PC など) と縦向き優先デバイス (電話や一部のタブレットなど) によって異なります。 次の表は、横向き優先デバイスと縦向き優先デバイスの両方のセンサー軸を示しています。

| 方向 | 横向き優先 | 縦向き優先 |
|-------------|-----------------|----------------|
| **横** | :::image type="content" source="images/sensor-orientation-0-small.jpg" alt-text="Landscape の向きの横向き優先デバイス"::: | :::image type="content" source="images/sensor-orientation-1-small.jpg" alt-text="Landscape の向きの縦向き優先デバイス"::: |
| **縦** | :::image type="content" source="images/sensor-orientation-2-small.jpg" alt-text="Portrait の向きの横向き優先デバイス"::: | :::image type="content" source="images/sensor-orientation-3-small.jpg" alt-text="Portrait の向きの縦向き優先デバイス"::: |
| **LandscapeFlipped** | :::image type="content" source="images/sensor-orientation-4-small.jpg" alt-text="LandscapeFlipped の向きの横向き優先デバイス"::: | :::image type="content" source="images/sensor-orientation-5-small.jpg" alt-text="LandscapeFlipped の向きの縦向き優先デバイス":::
| **PortraitFlipped** | :::image type="content" source="images/sensor-orientation-6-small.jpg" alt-text="PortraitFlipped の向きの横向き優先デバイス"::: | :::image type="content" source="images/sensor-orientation-7-small.jpg" alt-text="PortraitFlipped の向きの縦向き優先デバイス"::: |

## <a name="devices-broadcasting-display-and-headless-devices"></a>ディスプレイをブロードキャストするデバイスとヘッドレス デバイス

一部のデバイスは、別のデバイスにディスプレイをブロードキャストする機能を備えています。 たとえば、タブレットで、プロジェクターにディスプレイを横方向にブロードキャストできます。 このシナリオでは、デバイスの向きが、ディスプレイを表示しているものではなく、元のデバイスに基づいていることに留意することが重要です。 したがって、加速度計は、タブレットのデータを報告します。

さらに、一部のデバイスにはディスプレイがありません。 これらのデバイスでは、デバイスの既定の向きは縦です。

## <a name="display-orientation-and-compass-heading"></a>表示と向きとコンパスの方位

コンパスの方位は基準軸に依存するため、デバイスの向きと共に変化します。 次の表に基づいて補正します (ユーザーが北を向いていると仮定します)。

| 表示の向き | コンパスの方位の基準軸 | 北を向いている場合の API によるコンパスの方位 (横向き優先) | 北を向いている場合の API によるコンパスの方位 (縦向き優先) |コンパスの方位の補正 (横向き優先) | コンパスの方位の補正 (縦向き優先) |
|---------------------|------------------------------------|---------------------------------------------------------|--------------------------------------------------------|------------------------------------------------|-----------------------------------------------|
| 横           | -Z | 0   | 270 | [Heading] (方向)               | (方位 + 90) % 360  |
| 縦            |  ○ | 90  | 0   | (方位 + 270) % 360 |  [Heading] (方向)              |
| LandscapeFlipped    |  Z | 180 | 90  | (方位 + 180) % 360 | (方位 + 270) % 360 |
| PortraitFlipped     |  ○ | 270 | 180 | (方位 + 90) % 360  | (方位 + 180) % 360 |

正確に方位を表示するために、表に示されているようにコンパスの方位を修正します。 次のコード スニペットは、この方法を示しています。

```csharp
private void ReadingChanged(object sender, CompassReadingChangedEventArgs e)
{
    double heading = e.Reading.HeadingMagneticNorth;
    double displayOffset;

    // Calculate the compass heading offset based on
    // the current display orientation.
    DisplayInformation displayInfo = DisplayInformation.GetForCurrentView();

    switch (displayInfo.CurrentOrientation)
    {
        case DisplayOrientations.Landscape:
            displayOffset = 0;
            break;
        case DisplayOrientations.Portrait:
            displayOffset = 270;
            break;
        case DisplayOrientations.LandscapeFlipped:
            displayOffset = 180;
            break;
        case DisplayOrientations.PortraitFlipped:
            displayOffset = 90;
            break;
     }

    double displayCompensatedHeading = (heading + displayOffset) % 360;

    // Update the UI...
}
```

## <a name="display-orientation-with-the-accelerometer-and-gyrometer"></a>加速度計とジャイロメーターでの表示の向き

表示の向きに合わせた加速度計とジャイロメーターのデータの変換を、次の表に示します。

| 基準軸        |  X |  Y | Z |
|-----------------------|----|----|---|
| **横**         |  X |  Y | Z |
| **縦**          |  ○ | -X | Z |
| **LandscapeFlipped**  | -X | -y | Z |
| **PortraitFlipped**   | -y |  X | Z |

ジャイロメーターにこれらの変換を適用するコード例を次に示します。

```csharp
private void ReadingChanged(object sender, GyrometerReadingChangedEventArgs e)
{
    double x_Axis;
    double y_Axis;
    double z_Axis;

    GyrometerReading reading = e.Reading;  

    // Calculate the gyrometer axes based on
    // the current display orientation.
    DisplayInformation displayInfo = DisplayInformation.GetForCurrentView();
    switch (displayInfo.CurrentOrientation)
    {
        case DisplayOrientations.Landscape:
            x_Axis = reading.AngularVelocityX;
            y_Axis = reading.AngularVelocityY;
            z_Axis = reading.AngularVelocityZ;
            break;
        case DisplayOrientations.Portrait:
            x_Axis = reading.AngularVelocityY;
            y_Axis = -1 * reading.AngularVelocityX;
            z_Axis = reading.AngularVelocityZ;
            break;
        case DisplayOrientations.LandscapeFlipped:
            x_Axis = -1 * reading.AngularVelocityX;
            y_Axis = -1 * reading.AngularVelocityY;
            z_Axis = reading.AngularVelocityZ;
            break;
        case DisplayOrientations.PortraitFlipped:
            x_Axis = -1 * reading.AngularVelocityY;
            y_Axis = reading.AngularVelocityX;
            z_Axis = reading.AngularVelocityZ;
            break;
     }

    // Update the UI...
}
```

## <a name="display-orientation-and-device-orientation"></a>表示の向きとデバイスの向き

[**OrientationSensor**](/uwp/api/Windows.Devices.Sensors.OrientationSensor) データは別の方法で変更する必要があります。 これらのさまざまな向きを反時計回りに回転させて z 軸にするため、回転を反転してユーザーの向きを戻す必要があります。 四元数データの場合、オイラーの公式を使って、基準四元数により回転を定義できます。また、基準回転マトリックスを使うこともできます。

:::image type="content" source="images/eulers-formula.png" alt-text="オイラーの公式":::

必要な相対的な向きを得るには、基準オブジェクトと絶対オブジェクトを乗算します。 この演算は非可換であることに注意してください。

:::image type="content" source="images/orientation-formula.png" alt-text="基準オブジェクトと絶対オブジェクトの乗算":::

前の式では、センサー データによって絶対オブジェクトが返されます。

| 表示の向き  | Z 軸を中心とする反時計回りの回転 | 基準四元数 (逆回転) | 基準回転マトリックス (逆回転) |
|----------------------|------------------------------------|-----------------------------------------|----------------------------------------------|
| **横**        | 0                                  | 1 + 0i + 0j + 0k                        | \[1 0 0<br/> 0 1 0<br/> 0 0 1\]               |
| **縦**         | 90                                 | cos(-45⁰) + (i + j + k)*sin(-45⁰)       | \[0 1 0<br/>-1 0 0<br/>0 0 1]              |
| **LandscapeFlipped** | 180                                | 0 - i - j - k                           | \[1 0 0<br/> 0 1 0<br/> 0 0 1]               |
| **PortraitFlipped**  | 270                                | cos(-135⁰) + (i + j + k)*sin(-135⁰)     | \[0 -1 0<br/> 1  0 0<br/> 0  0 1]             |

## <a name="see-also"></a>関連項目

[モーション センサーと方位センサーの統合](/windows-hardware/design/whitepapers/integrating-motion-and-orientation-sensors)