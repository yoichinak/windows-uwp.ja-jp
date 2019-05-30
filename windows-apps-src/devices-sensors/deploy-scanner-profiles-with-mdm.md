---
title: MDM によるバーコード スキャナー プロファイルの展開
description: バーコード スキャナー プロファイルは、MDM サーバーを使って展開できます。
ms.assetid: 99ED3BD8-022C-40C2-9C65-F599186548FE
ms.date: 09/26/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e92c4c715608f9ae36adb3a67beec8002083542f
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370286"
---
# <a name="deploy-barcode-scanner-profiles-with-mdm"></a>MDM によるバーコード スキャナー プロファイルの展開

**注**  この機能では Windows 10 Mobile 以降。

バーコード スキャナー プロファイルは、MDM サーバーを使って展開できます。 使用して、プロファイルを展開する*OemProfile*で、 [EnterpriseExtFileSystem CSP](https://docs.microsoft.com/windows/client-management/mdm/enterpriseextfilessystem-csp)に配置することに、\\データ\\SharedData\\OEM\\パブリック\\プロファイル フォルダー。 ドライバーの製造元では、これらのスキャナー プロファイルを使用して、API サーフェスを通じて公開されていない設定を構成できます。

Microsoft では、スキャナーのプロファイルやそれらを実装する方法の詳細を定義していません。

## <a name="related-topics"></a>関連トピック
- [EnterpriseExtFileSystem CSP](https://docs.microsoft.com/windows/client-management/mdm/enterpriseextfilessystem-csp)
- [バーコード スキャナのデバイスのサポート](https://docs.microsoft.com/en-us/windows/uwp/devices-sensors/pos-device-support#barcode-scanner)