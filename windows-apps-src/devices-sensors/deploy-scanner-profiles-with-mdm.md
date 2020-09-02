---
title: MDM によるバーコード スキャナー プロファイルの展開
description: EnterpriseExtFileSystem 構成サービスプロバイダー (CSP) を使用して、モバイルデバイス管理 (MDM) サーバーでバーコードスキャナープロファイルを展開する方法について説明します。
ms.assetid: 99ED3BD8-022C-40C2-9C65-F599186548FE
ms.date: 09/26/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: de3a43386e37c9bb997340c35c8f16977871916d
ms.sourcegitcommit: 5481bb34def681bc60fbfa42d9779053febec468
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/02/2020
ms.locfileid: "89304724"
---
# <a name="deploy-barcode-scanner-profiles-with-mdm"></a>MDM によるバーコード スキャナー プロファイルの展開

**メモ**   この機能には、Windows 10 Mobile 以降が必要です。

バーコード スキャナー プロファイルは、MDM サーバーを使って展開できます。 プロファイルを展開するには、 [Enterpriseextfilesystem CSP](/windows/client-management/mdm/enterpriseextfilessystem-csp)の*oemprofile*を使用して、それらを \\ データ \\ shareddata \\ OEM \\ パブリックプロファイルフォルダーに配置し \\ ます。 ドライバーの製造元では、これらのスキャナー プロファイルを使用して、API サーフェスを通じて公開されていない設定を構成できます。

Microsoft では、スキャナーのプロファイルやそれらを実装する方法の詳細を定義していません。

## <a name="related-topics"></a>関連トピック
- [EnterpriseExtFileSystem CSP](/windows/client-management/mdm/enterpriseextfilessystem-csp)
- [バーコード スキャナー デバイスのサポート](./pos-device-support.md#barcode-scanner)