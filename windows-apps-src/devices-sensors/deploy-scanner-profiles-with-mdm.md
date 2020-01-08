---
title: MDM によるバーコード スキャナー プロファイルの展開
description: バーコード スキャナー プロファイルは、MDM サーバーを使って展開できます。
ms.assetid: 99ED3BD8-022C-40C2-9C65-F599186548FE
ms.date: 09/26/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: f537833385582678b215804cac9a16002618c7e4
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/06/2020
ms.locfileid: "75684821"
---
# <a name="deploy-barcode-scanner-profiles-with-mdm"></a>MDM によるバーコード スキャナー プロファイルの展開

この**機能  Windows** 10 Mobile 以降が必要です。

バーコード スキャナー プロファイルは、MDM サーバーを使って展開できます。 プロファイルを展開するには、 [Enterpriseextfilesystem CSP](https://docs.microsoft.com/windows/client-management/mdm/enterpriseextfilessystem-csp)の*oemprofile*を使用して、\\データ\\shareddata\\OEM\\パブリック\\プロファイルフォルダーに配置します。 ドライバーの製造元では、これらのスキャナー プロファイルを使用して、API サーフェスを通じて公開されていない設定を構成できます。

Microsoft では、スキャナーのプロファイルやそれらを実装する方法の詳細を定義していません。

## <a name="related-topics"></a>関連トピック
- [EnterpriseExtFileSystem CSP](https://docs.microsoft.com/windows/client-management/mdm/enterpriseextfilessystem-csp)
- [バーコードスキャナーデバイスのサポート](https://docs.microsoft.com/windows/uwp/devices-sensors/pos-device-support#barcode-scanner)