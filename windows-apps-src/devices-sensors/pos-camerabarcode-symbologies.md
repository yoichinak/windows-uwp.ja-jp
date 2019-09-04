---
title: カメラ バーコード スキャナーのシンボル体系
description: カメラ バーコード スキャナーでサポートされるシンボル体系
ms.date: 05/02/2018
ms.topic: article
keywords: Windows 10, UWP, 店舗販売時点管理, POS
ms.localizationpriority: medium
ms.openlocfilehash: cc2aaaf4e9779cb2be712119fb1dacdf946952c5
ms.sourcegitcommit: 0dec04de501a3db6b22dfd4a320fc09b5c4a21b5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70243324"
---
# <a name="symbologies"></a>シンボル体系
このトピックでは、Windows 10 に付属するソフトウェアバーコードデコーダーでサポートされている各 symbologies のサンプルバーコードを示します。次に例を示します。UPC/EAN、コード39、コード128、インターリーブ 2 of 5、縦棒全方向、縦棒積み上げ、QR コード、および GS1DWCode。

## <a name="1d-symbologies"></a>1D シンボル体系

### <a name="code-39"></a>Code 39
![サンプル バーコード - Code 39](images/pos/sample-barcode-code39.png)

### <a name="code-128"></a>Code 128
![サンプル バーコード - Code 128](images/pos/sample-barcode-code128.png)

### <a name="databar-omnidirectional"></a>Databar Omnidirectional
![サンプル バーコード - Databar Omnidirectional](images/pos/sample-barcode-databar-omnidirectional.png) 
### <a name="databar-stacked"></a>Databar Stacked
![サンプル バーコード - Databar Stacked](images/pos/sample-barcode-databar-stacked.png)

### <a name="ean-8"></a>EAN-8
![サンプル バーコード - EAN-8](images/pos/sample-barcode-ean8.png)

### <a name="ean-13"></a>EAN-13
![サンプル バーコード - EAN-13](images/pos/sample-barcode-ean13.png)

### <a name="interleaved-2-of-5"></a>Interleaved 2 of 5
![サンプル バーコード - Interleaved 2 of 5](images/pos/sample-barcode-interleaved-2-of-5.png)

### <a name="upc-a"></a>UPC-A
![サンプル バーコード - UPC A](images/pos/sample-barcode-upca.png)

### <a name="upc-e"></a>UPC-E
![サンプル バーコード - UPC E](images/pos/sample-barcode-upce.png)

## <a name="2d-symbologies"></a>2D シンボル体系
### <a name="qr-code"></a>QR コード
![サンプル バーコード - QR コード](images/pos/sample-barcode-qrcode.png)

## <a name="digital-watermark"></a>電子透かし
### <a name="gs1-dwcode"></a>GS1-DWCode

カメラ バーコード スキャナー アプリケーションを使用して以下のパッケージの画像をスキャンすると、GS1DWCode の動作を確認できます。  この画像は、UPCA 856107006854 でエンコードされています。  GS1DWCode 機能の詳細については、 http://www.digimarc.com を参照してください。

![サンプル バーコード - GS1DWCode](images/pos/rice-box-v7.jpg)

> [!NOTE]
> Windows 10 に付属するソフトウェア デコーダーは、[*Digimarc Corporation*](https://www.digimarc.com/) から無料で提供されています。

## <a name="see-also"></a>関連項目

### <a name="samples"></a>サンプル

- [バーコードスキャナーのサンプル](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)
