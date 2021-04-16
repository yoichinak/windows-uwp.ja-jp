---
ms.openlocfilehash: e4f09efa1bd6d6884e9a32b46aff1bf586a16e60
ms.sourcegitcommit: 6cd970686d1ea7176b7e6651f349a14551709820
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/16/2021
ms.locfileid: "107559389"
---
> [!IMPORTANT]
> packages.config を依然として使用する .NET Framework デスクトップアプリは、PackageReference に移行する必要があります。そうしないと、Windows 10 Sdk が正しく参照されません。 プロジェクトで、[参照] を右クリックし、[packages.config を PackageReference に移行する] をクリックします。
> 
> .NET Core 3.0 WPF アプリは .NET Core 3.1 に更新する必要があります。そうしないと、Api は存在しません。
> 
> .NET 5 アプリでは、 [Windows 10 TFMs の1つを使用](https://docs.microsoft.com/dotnet/standard/frameworks#how-to-specify-a-target-framework)する必要があります。そうしないと、のようなトースト送信および管理 api が `Show()` 失われます。 TFM を以上に設定 `net5.0-windows10.0.17763.0` します。
