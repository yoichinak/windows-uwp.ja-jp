---
title: PointOfService デバイス機能
description: PointOfService 機能は、Windows.Devices.PointOfService 名前空間の使用に必要です。
ms.date: 05/02/2018
ms.topic: article
keywords: Windows 10, UWP, 店舗販売時点管理, POS
ms.localizationpriority: medium
ms.openlocfilehash: f120e093ab65224ca4c32b64640100b6abd1a36a
ms.sourcegitcommit: 0301f794f994e604ffc131de7e40ffcede3530c9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/08/2019
ms.locfileid: "72036264"
---
# <a name="pointofservice-device-capability"></a>PointOfService デバイス機能
アプリケーション パッケージのマニフェストで機能を宣言することで、PointOfService API へのアクセスを要求します。Microsoft Visual Studio のマニフェスト デザイナーを使用することで、ほとんどの機能を宣言することができます。または、手動で追加することもできます。  

> [!Important]
> アプリケーションマニフェストで**PointOfService**機能を宣言していない場合は、PointOfService 名前空間で API を使用しようとすると、 **system.unauthorizedaccessexception**エラーが発生します。 

## <a name="declare-capability-using-manifest-designer"></a>マニフェスト デザイナーを使用した機能の宣言

1. **ソリューション エクスプローラー**で、UWP アプリケーションのプロジェクト ノードを展開します。
2. **[Package.appxmanifest]** ファイルをダブルクリックします。  
*XML コードビューでマニフェストファイルが既に開かれている場合、Visual Studio はファイルを閉じるように求めます。*
3. **[機能]** タブをクリックします。
4. 機能の一覧で **[店舗販売時点管理 (POS)]** の横にあるチェック ボックスをオンにして、POS デバイスの機能を有効にします。


## <a name="declare-capability-manually"></a>手動での機能の宣言

1. **ソリューション エクスプローラー**で、UWP アプリケーションのプロジェクト ノードを展開します。
2. **[Package.appxmanifest]** ファイルを右クリックし、 **[コードの表示]** を選択します。
3. PointOfService DeviceCapability 要素をアプリケーション マニフェストの Capabilities セクションに追加します。  

```xml
  <Capabilities>
    <DeviceCapability Name="pointOfService" />
  </Capabilities>
   ```
