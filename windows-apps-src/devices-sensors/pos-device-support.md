---
title: POS (店舗販売時点管理) デバイスのサポート
description: この記事には、POS (店舗販売時点管理) デバイス クラスの各ハードウェアのサポートに関する情報が含まれています
ms.date: 06/13/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9e89011086e6c6d318d589226400789b41f8fe64
ms.sourcegitcommit: 39fb8c0dff1b98ededca2f12e8ea7977c2eddbce
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/06/2020
ms.locfileid: "91750349"
---
# <a name="supported-point-of-service-peripherals"></a>サポートされている POS 周辺機器

## <a name="barcode-scanner"></a>バーコード スキャナー
| 接続 | サポート |
| -------------|-------------|
| USB          | <p>Windows には、[USB.org](https://www.usb.org/hid) によって定義された HID POS Scanner Usage Table (8c) 仕様に基づく、USB 接続バーコード スキャナー用インボックス クラス ドライバーが含まれています。互換性のある既知のデバイスの一覧については、次の表をご覧ください。  **USB.HID.POS Scanner** モードでスキャナーを構成する方法については、バーコード スキャナーのマニュアルを参照するか、製造元にお問い合わせください。 </p><p>Windows では、USB.HID.POS Scanner 標準をサポートしない追加のバーコード スキャナーをサポートするために、ベンダー固有のドライバーの実装をサポートしています。 ベンダー固有のドライバーの利用可能性については、バーコード スキャナーの製造元に確認してください。</p><p>バーコード スキャナーのメーカー様は、カスタム バーコード スキャナー ドライバーの作成については、[バーコード スキャナー ドライバー設計ガイド](/windows-hardware/drivers/ddi/_pos/index)をご覧ください。</p> |
| Bluetooth    | <p>Windows では、Serial Port Protocol - Simple Serial Interface (SPP-SSI) ベースの Bluetooth バーコード スキャナーがサポートされています。 既知の互換性のあるデバイスの一覧については、次の表を参照してください。 **SPP-SSI** モードでスキャナーを構成する方法については、バーコード スキャナーのマニュアルを参照するか、製造元にお問い合わせください。</p> |
| ウェブ カメラ       | <p>Windows 10 バージョン 1803 以降では、ユニバーサル Windows アプリケーションから標準のカメラ レンズでバーコードを読み取ることができます。 オート フォーカスと 1920 x 1440 以上の解像度をサポートするカメラを使用することをお勧めします。  バーコードが十分な大きさで印刷されている場合、これより解像度の低いカメラでも標準的なバーコードを読み取ることができる可能性があります。  バーコードの要素が細い場合は、より高い解像度のカメラが必要になる可能性があります。</p>| 
|


| Manufacturer  | モデル                          | 機能 | Connection    | Type         | モード                      |
|---------------|--------------------------------|------------|--------------|--------------|---------------------------|
| コード          | リーダー™950                    | 2D         | USB          | ハンドヘルド     | HID POS スキャナー           |
| コード          | リーダー™1021                   | 2D         | USB          | ハンドヘルド     | HID POS スキャナー           |
| コード          | リーダー™1421                   | 2D         | USB          | ハンドヘルド     | HID POS スキャナー           |
| コード          | リーダー™5000                   | 2D         | USB          | プレゼンテーション | HID POS スキャナー           |
| Honeywell     | Genesis 7580g                  | 2D         | USB          | プレゼンテーション | HID POS スキャナー           |
| Honeywell     | Granit 198Xi                   | 2D         | USB          | ハンドヘルド     | HID POS スキャナー           |
| Honeywell     | Granit 191Xi                   | 2D         | USB          | ハンドヘルド     | HID POS スキャナー           |
| Honeywell     | N5680                          | 2D         | 内部     | コンポーネント    | HID POS スキャナー           |
| Honeywell     | N3680                          | 2D         | 内部     | コンポーネント    | HID POS スキャナー           |
| Honeywell     | 軌道7190g                    | 2D         | USB          | プレゼンテーション | HID POS スキャナー           |
| Honeywell     | Stratos 2700                   | 2D         | USB          | カウンター内   | HID POS スキャナー           |
| Honeywell     | Voyager 1200g                  | 1D         | USB          | ハンドヘルド     | HID POS スキャナー           |
| Honeywell     | Voyager 1202g                  | 1D         | USB          | ハンドヘルド     | HID POS スキャナー           |
| Honeywell     | Voyager 1202-bf                | 1D         | USB          | ハンドヘルド     | HID POS スキャナー           |
| Honeywell     | Voyager 145Xg                  | 1D/2D<sup>1</sup>   | USB          | ハンドヘルド     | HID POS スキャナー           |
| Honeywell     | Voyager 1602g                  | 2D         | USB          | ハンドヘルド     | HID POS スキャナー           |
| Honeywell     | Xenon 1900g                    | 2D         | USB          | ハンドヘルド     | HID POS スキャナー           |
| Honeywell     | Xenon 1902g                    | 2D         | USB          | ハンドヘルド     | HID POS スキャナー           |
| Honeywell     | Xenon 1902g-bf                 | 2D         | USB          | ハンドヘルド     | HID POS スキャナー           |
| Honeywell     | Xenon 1900h                    | 2D         | USB          | ハンドヘルド     | HID POS スキャナー           |
| Honeywell     | Xenon 1902h                    | 2D         | USB          | ハンドヘルド     | HID POS スキャナー           |
| HP            | 値バーコードスキャナー (HR2150) | 2D         | USB          | ハンドヘルド     | HID POS スキャナー           |
| Intermec      | SG20                           | 2D         | USB          | ハンドヘルド     | HID POS スキャナー           |
| Socket Mobile | CHS 7 Ci                        | 1D         | Bluetooth    | ハンドヘルド     | シリアルポートプロファイル (SPP) |
| Socket Mobile | CHS 7Di                        | 1D         | Bluetooth    | ハンドヘルド     | シリアルポートプロファイル (SPP) |
| Socket Mobile | CHS 7Mi                        | 1D         | Bluetooth    | ハンドヘルド     | シリアルポートプロファイル (SPP) |
| Socket Mobile | CHS 7Pi                        | 1D         | Bluetooth    | ハンドヘルド     | シリアルポートプロファイル (SPP) |
| Socket Mobile | CHS 8Ci                        | 1D         | Bluetooth    | ハンドヘルド     | シリアルポートプロファイル (SPP) |
| Socket Mobile | DuraScan D700                  | 1D         | Bluetooth    | ハンドヘルド     | シリアルポートプロファイル (SPP) |
| Socket Mobile | DuraScan D730                  | 1D         | Bluetooth    | ハンドヘルド     | シリアルポートプロファイル (SPP) |
| Socket Mobile | DuraScan D740                  | 2D         | Bluetooth    | ハンドヘルド     | シリアルポートプロファイル (SPP) |
| Socket Mobile | ソケットスキャン S700                | 1D         | Bluetooth    | ハンドヘルド     | シリアルポートプロファイル (SPP) |
| Socket Mobile | ソケットスキャン S730                | 1D         | Bluetooth    | ハンドヘルド     | シリアルポートプロファイル (SPP) |
| Socket Mobile | ソケットスキャン S740                | 2D         | Bluetooth    | ハンドヘルド     | シリアルポートプロファイル (SPP) |
| Socket Mobile | ソケットスキャン S800                | 1D         | Bluetooth    | ハンドヘルド     | シリアルポートプロファイル (SPP) |
| Socket Mobile | ソケットスキャン S850                | 2D         | Bluetooth    | ハンドヘルド     | シリアルポートプロファイル (SPP) |
| ゼブラ         | DS2208<sup>2</sup>                        | 2D         | USB          | ハンドヘルド     | HID POS スキャナー           |
| ゼブラ         | DS2278                         | 2D         | USB          | ハンドヘルド     | HID POS スキャナー           |
| ゼブラ         | DS8108<sup>3</sup>                        | 2D         | USB          | ハンドヘルド     | HID POS スキャナー           |
| ゼブラ         | DS8178<sup>4</sup>                         | 2D         | USB          | ハンドヘルド     | HID POS スキャナー           | 


<sup>1</sup> Honeywell を通じて2d バーコードをサポートするためにアップグレード可能 <br/>
<sup>2</sup> 最小ファームウェア 009 (2018.07.09) が必要です。 ゼブラ [123Scan](http://www.zebra.com/123scan)を使用してアップグレード可能。<br/>
<sup>3</sup> 最小ファームウェア 016 (2018.01.18) が必要です。 ゼブラ [123Scan](http://www.zebra.com/123scan)を使用してアップグレード可能。<br/> 
<sup>4</sup> 最小ファームウェア 023 (2019.03.11) が必要です。 ゼブラ [123Scan](http://www.zebra.com/123scan)を使用してアップグレード可能。<br/>

<hr>

### <a name="windows-devices-with-built-in-barcode-scanner"></a>組み込みのバーコードスキャナーがある Windows デバイス
| Manufacturer   | モデル | オペレーティング システム |
|----------------|-------|------------------|
| Innowi         | ChecOut-M | Windows 10   |

### <a name="windows-mobile-devices-with-built-in-barcode-scanner"></a>組み込みのバーコードスキャナーを備えた Windows Mobile デバイス
| Manufacturer   | モデル | オペレーティング システム |
|----------------|-------|------------------|
| Bluebird       | EF400 | Windows Mobile   |
| Bluebird       | EF500 | Windows Mobile   |
| Bluebird       | EF500R | Windows Mobile   |
| Honeywell      | CT50   | Windows Mobile   |
| Honeywell      | D75e | Windows Mobile   |
| Janam          | XT2      | Windows Mobile   |
| Panasonic      | FZ-E1 | Windows Mobile   |
| Panasonic      | FZ-F1 |Windows Mobile   |
| PointMobile    | PM80 | Windows Mobile   |
| ゼブラ          | TC700j | Windows Mobile   |
| HP             | エリート X3 ジャケット | Windows Mobile   |




## <a name="cash-drawer"></a>キャッシュ ドロワー
| 接続 | サポート |
| -------------|-------------|
| ネットワーク/Bluetooth | <p> キャッシュ ドロワー ユニットの機能に応じて、ネットワーク経由または Bluetooth によって、キャッシュ ドロワーに直接接続できます。 </p><p>APG Cash Drawer: NetPRO、BluePRO</p> |
| DK ポート | <p> ネットワーク機能や Bluetooth 機能を持たないキャッシュ ドロワーは、サポートされているレシート プリンター上の DK ポートまたは Star Micronics DK-AirCash アクセサリ経由で接続できます。 </p>
| OPOS    | <p> 製造元から提供される OPOS サービス オブジェクトを介して、すべての OPOS 互換キャッシュ ドロワーをサポートします。 デバイスの製造元のインストール手順に従って、OPOS ドライバーをインストールします。 </p> |


## <a name="customer-display-linedisplay"></a>カスタマー ディスプレイ (LineDisplay)
製造元から提供される OPOS サービス オブジェクトを介して、すべての OPOS 互換ライン ディスプレイをサポートします。 デバイスの製造元のインストール手順に従って、OPOS ドライバーをインストールします。

## <a name="magnetic-stripe-reader"></a>磁気ストライプ リーダー
Windows では、ベンダー ID と製品 ID (VID/PID) に基づいて、Magtek と IDTech の次の磁気ストライプ リーダーのサポートを提供します。

| Manufacturer |    モデル |  製品番号 |
|--------------|-----------|--------------|
| IDTech | SecureMag (VID:0ACD PID:2010) | IDRE-3x5xxxx |
| | MiniMag (VID:0ACD PID:0500) |   IDMB-3x5xxxx |
| Magtek | MagneSafe (VID:0801 PID:0011) |  210730xx |
| | Dynamag (VID:0801 PID:0002) |   210401xx |

 Windows では、追加の磁気ストライプ リーダーをサポートする追加のベンダー固有のドライバーの実装をサポートしています。 ベンダー固有のドライバーの有無については、磁気ストライプ リーダーの製造元に確認してください。 磁気ストライプ リーダーのメーカー様は、カスタム磁気ストライプ リーダー ドライバーの作成については、[磁気ストライプ リーダー設計ガイド](/windows-hardware/drivers/ddi/_pos/index)をご覧ください。

## <a name="receipt-printer-posprinter"></a>レシート プリンター (POSPrinter)
| 接続 | サポート |
| -------------|-------------|
| ネットワークと Bluetooth | <p>Windows では、Epson ESC/POS プリンター制御言語を使用して、ネットワークおよび Bluetooth で接続されているレシート プリンターをサポートしています。  以下に示すプリンターは、POSPrinter API を使用して自動的に検出されます。 ESC/POS エミュレーションを提供するその他のレシート プリンターも使用できる可能性がありますが、[帯域外ペアリング](./point-of-service.md) プロセスを使用して関連付ける必要があります。</p><p>注: この方法では、スリップ ステーションとジャーナル ステーションはサポートされません。</p> |
| OPOS    | <p> OPOS サービス オブジェクトを介して、OPOS 互換のレシート プリンターをサポートします。 デバイスの製造元のインストール手順に従って、OPOS ドライバーをインストールします。 </p> |

### <a name="stationary-receipt-printers-networkbluetooth"></a>固定型レシート プリンター (ネットワーク/Bluetooth)
| Manufacturer |    モデル |
|--------------|-----------|
| Epson |   TM T88V、TM-T70、TM-T20、TM U220 |

### <a name="mobile-receipt-printers-bluetooth"></a>モバイル レシート プリンター (Bluetooth)
| Manufacturer |    モデル |
|--------------|-----------|
| Epson |   Mobilink P20 (TM-P20)、Mobilink P60 (TM-P60)、Mobilink P80 (TM-P80) |