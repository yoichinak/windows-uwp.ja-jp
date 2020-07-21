---
ms.assetid: 404783BA-8859-4BFB-86E3-3DD2042E66F5
title: Bluetooth
description: このセクションでは、RFCOMM、GATT、低エネルギー (LE) の広告を使う方法を含め、ユニバーサル Windows プラットフォーム (UWP) アプリに Bluetooth を統合する方法に関する記事を取り上げています。
ms.date: 06/26/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: aad368adf500c5968bf6374a5855a7e0dab0a044
ms.sourcegitcommit: 015291bdf2e7d67076c1c85fc025f49c840ba475
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/27/2020
ms.locfileid: "85469567"
---
# <a name="bluetooth"></a>Bluetooth
このセクションには、Bluetooth をユニバーサル Windows プラットフォーム (UWP) アプリに統合する方法に関する記事が記載されています。 アプリに実装する場合、2 種類の Bluetooth テクノロジのいずれかを選択できます。

> [!Important]
> *Package.appxmanifest*で "bluetooth" 機能を宣言する必要があります。
>
> `<Capabilities> <DeviceCapability Name="bluetooth" /> </Capabilities>`

## <a name="classic-bluetooth-rfcomm"></a>Classic Bluetooth (RFCOMM)
Bluetooth LE 以前は、デバイスが Bluetooth を使って通信する場合、通常このプロトコルを使用していました。 このプロトコルはシンプルであり、電力の節約が必要ではないデバイス間の通信に便利です。 コード サンプルも含め、このプロトコルについて詳しくは、「[Bluetooth RFCOMM](send-or-receive-files-with-rfcomm.md)」のトピックをご覧ください。

## <a name="bluetooth-low-energy-le"></a>Bluetooth 低エネルギー (LE)
Bluetooth 低エネルギー (LE) は、効率的な電力使用量の要件のあるデバイス間の検出と通信用のプロトコルを定義する仕様です。 コード サンプルなどの詳細については、「[Bluetooth 低エネルギー](bluetooth-low-energy-overview.md)」のトピックをご覧ください。

## <a name="see-also"></a>関連項目
- [Bluetooth に関する開発者向け FAQ](bluetooth-dev-faq.md)