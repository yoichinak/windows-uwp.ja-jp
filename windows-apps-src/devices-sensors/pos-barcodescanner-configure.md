---
title: バーコード スキャナーの構成
description: 目的のアプリケーションのバーコードスキャナーを構成する方法について説明します。
ms.date: 08/29/2018
ms.topic: article
keywords: Windows 10, UWP, 店舗販売時点管理, POS
ms.localizationpriority: medium
ms.openlocfilehash: 96deeb82f0aa04929ac33001b1aee0603e6474c7
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89168506"
---
# <a name="configure-a-barcode-scanner"></a>バーコード スキャナーの構成

バーコード スキャナーは、いくつかの異なるモードで構成することができます。  目的の用途に合わせてバーコード スキャナーを正しく構成することが重要です。

多くのバーコード スキャナーは、**キーボード ウェッジ** モードで構成できます。この場合、バーコード スキャナーは Windows に対してキーボードとして表示されます。  これにより、メモ帳などのバーコード スキャナーに対応していないアプリケーションにバーコードをスキャンすることができます。  このモードでバーコードをスキャンすると、キーボードを使用してデータを入力した場合と同様に、バーコード スキャナーからデコードされたデータが挿入ポイントに挿入されます。  UWP アプリからバーコード スキャナーをより細かく制御する場合は、キーボード ウェッジ以外のモードでバーコード スキャナーを構成する必要があります。

## <a name="usb-barcode-scanner"></a>USB バーコード スキャナー
USB 接続されたバーコード スキャナーは、Windows に含まれているバーコード スキャナー ドライバーで動作するように、**HID POS スキャナー** モードで構成する必要があります。 このドライバーは、 [USB-hid](https://www.usb.org/hid)に発行された**Hid の使用状況テーブル**の仕様を実装したものです。  **HID POS スキャナー** モードを有効にする手順については、バーコード スキャナーの説明書を参照するか、バーコード スキャナーの製造元にお問い合わせください。  **HID POS スキャナー**として構成すると、バーコード スキャナーはデバイス マネージャーの **POS バーコード スキャナー** ノードに **POS HID バーコード スキャナー**として表示されます。

バーコード スキャナーの製造元が、**HID POS スキャナー**以外のモードを使用して UWP バーコード スキャナー API をサポートする、ベンダー固有のドライバーを用意している可能性もあります。  UWP バーコードスキャナ Api と互換性のある製造元提供のドライバーが既にインストールされている場合は、デバイスマネージャーの [ **POS バーコードスキャナー** ] の下にベンダー固有のデバイスが表示されることがあります。

## <a name="bluetooth-barcode-scanner"></a>Bluetooth バーコード スキャナー
Bluetooth で接続されているスキャナーで UWP バーコード スキャナー API を使用するには、バーコード スキャナーを **Serial Port Protocol - Simple Serial Interface (SPP-SSI)** モードで構成する必要があります。  **SPP-SSI モード**を有効にする手順については、バーコード スキャナーの説明書を参照するか、バーコード スキャナーの製造元にお問い合わせください。

Bluetooth バーコードスキャナーを使用するには、bluetooth **またはその他のデバイスを追加 > 他のデバイス & > 設定 > デバイス**を使用する必要があります。

ペアリングプロセスは、 [Windows の Enumeration](/uwp/api/windows.devices.enumeration) 名前空間を使用して開始および制御できます。  詳しくは、「[デバイスのペアリング](./pair-devices.md)」をご覧ください。

[!INCLUDE [feedback](./includes/pos-feedback.md)]