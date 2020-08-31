---
ms.assetid: ECE848C2-33DE-46B0-BAE7-647DB62779BB
title: センサーの調整
description: デバイスの磁力計 (コンパス、傾斜計、方位センサー) に基づくセンサーは、環境の要因に応じて調整が必要になることがあります。
ms.date: 03/22/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 0ef2c1cfbe949e5f850a494e4f1ed9c79faf6050
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89168576"
---
# <a name="calibrate-sensors"></a>センサーの調整


**重要な API**

-   [**Windows.Devices.Sensors**](/uwp/api/Windows.Devices.Sensors)
-   [**Windows. デバイス... カスタム**](/uwp/api/Windows.Devices.Sensors.Custom)

デバイスの磁力計 (コンパス、傾斜計、方位センサー) に基づくセンサーは、環境の要因に応じて調整が必要になることがあります。 [**MagnetometerAccuracy**](/uwp/api/Windows.Devices.Sensors.MagnetometerAccuracy) 列挙値は、デバイスの調整が必要になる場合の対応策を決めるのに役立ちます。

## <a name="when-to-calibrate-the-magnetometer"></a>磁力計を調整するタイミング

[**MagnetometerAccuracy**](/uwp/api/Windows.Devices.Sensors.MagnetometerAccuracy) 列挙値には、アプリが実行されているデバイスを調整する必要があるかどうかを判断するのに役立つ 4 つの値があります。 デバイスを調整する必要がある場合は、その旨をユーザーに知らせる必要があります。 ただし、あまりに頻繁に調整を行うようユーザーに促さないでください。 10 分に 1 回を超えないようにします。

| 値           | 説明    |
| ----------------- | ------------------- |
| **Unknown**     | センサー ドライバーは現在の正確さを報告できませんでした。 これは、必ずしもデバイスが調整されていないことを意味するわけではありません。 **Unknown** が返された場合の最適な対応策は、開発しているアプリによって決まります。 アプリがセンサーの正確な読み取り値を利用しているのであれば、ユーザーにデバイスの調整を促します。 |
| **Unreliable**  | 現在、磁力計の正確さが大きく劣っています。 この値が最初に返された時点で、アプリからユーザーに必ず調整を求めます。 |
| **概数** | 特定のアプリに対してデータは十分に正確です。 ユーザーがデバイスを上下または左右に動かしたかどうかがわかればよいだけの仮想現実アプリでは、調整なしで続行できます。 指示を出すためにどの方向に進んでいるか知る必要があるナビゲーション アプリのように、絶対的な進路を必要とするアプリでは、調整を促す必要があります。 |
| **高**        | データは正確です。 拡張現実アプリ、ナビゲーション アプリなどの絶対的な進路を知る必要があるアプリでも、調整は必要ありません。 |