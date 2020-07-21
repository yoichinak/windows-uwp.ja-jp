---
title: カメラ バーコード スキャナーのシステム要件
description: この記事では、UWP アプリからカメラ バーコード スキャナーを使用するための要件を説明します。
ms.date: 05/02/2018
ms.topic: article
keywords: Windows 10, UWP, 店舗販売時点管理, POS
ms.localizationpriority: medium
ms.openlocfilehash: 4eed59e302bc34e93d21adef794de02427f2933e
ms.sourcegitcommit: 0dec04de501a3db6b22dfd4a320fc09b5c4a21b5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70243271"
---
# <a name="camera-barcode-scanner-system-requirements"></a>カメラ バーコード スキャナーのシステム要件
Windows 10 バージョン 1803 以降では、ユニバーサル Windows アプリケーションから標準のカメラ レンズでバーコードを読み取ることができます。

## <a name="supported-windows-editions"></a>サポートされている Windows エディション
- Windows 10 Professional (S モード)
- Windows 10 Professional
- Windows 10 Enterprise
- Windows 10 IOT Core


## <a name="webcam-requirements"></a>Web カメラの要件
| Category      | 推奨           | コメント |
| ------------- | ------------------------ | -------- |
| フォーカス         | オート フォーカス               | 固定焦点はお勧めできません。 |
| 解決方法    | 1920 x 1440 以上    | 1920 x 1440 以上の解像度に対応したカメラで最適なエクスペリエンスが得られました。  バーコードが十分な大きさで印刷されている場合、これより解像度の低いカメラでも標準的なバーコードを読み取ることができる可能性があります。 バーコードの要素が細い場合は、より高い解像度のカメラが必要になる可能性があります。 |
|

## <a name="see-also"></a>関連項目

### <a name="samples"></a>サンプル

- [バーコードスキャナーのサンプル](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)
