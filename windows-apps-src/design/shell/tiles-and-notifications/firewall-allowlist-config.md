---
author: mijacobs
Description: 多くの企業では、不要なトラフィックをブロックするのにファイアウォールを使用します。 このドキュメントでは、ファイアウォールを通過する WNS トラフィックを許可する方法について説明します。
title: WNS のトラフィックをファイアウォールの許可リストに追加します。
ms.assetid: 2125B09F-DB90-4515-9AA6-516C7E9ACCCD
template: detail.hbs
ms.author: mijacobs
ms.date: 05/20/2019
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10、uwp、WNS の場合、windows 通知サービス、通知、windows ファイアウォール、トラブルシューティング、IP、トラフィック、enterprise、ネットワーク、パブリック IP アドレス、IPv4、VIP、FQDN
ms.localizationpriority: medium
ms.openlocfilehash: 466463bfc984707af4cb30618f9cbfa47d78703c
ms.sourcegitcommit: fd7d358aad3a5b7112f5a587bb6ea86805dc8a4d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/21/2019
ms.locfileid: "65976252"
---
# <a name="allowing-windows-notification-traffic-through-enterprise-firewalls"></a>エンタープライズ ファイアウォールを介した Windows の通知のトラフィックを許可します。

## <a name="background"></a>背景
多くの企業では、ファイアウォールを使用して、不要なネットワーク トラフィックをブロックするには残念ながら、Windows 通知サービスの通信などの重要なブロックこれこともできます。 つまり、WNS から送信されるすべての通知は破棄されます。 これを回避するには、ネットワーク管理者は、ファイアウォールを通過する WNS トラフィックを許可するには、その除外リストに承認済みの WNS チャネルの一覧を追加できます。 方法の詳細および追加するものを次に示します。 


## <a name="what-information-should-be-added-to-the-allowlist"></a>どのような情報は、許可リストに追加する必要があります。
以下は、Fqdn、Vip、および IP を含むリスト アドレス範囲、Windows 通知サービスで使用します。 

> [!IMPORTANT]
> 強くお勧め fqdn を一覧できるようにすることため、これらは変更されません。 Fqdn ボックスの一覧を許可する場合も IP アドレスの範囲を許可する必要はありません。

> [!IMPORTANT]
> IP アドレスの範囲が定期的に変更されます。このため、このページは含まれません。 IP 範囲の一覧を表示する場合は、ダウンロード センターからファイルをダウンロードできます。[Windows 通知サービス (WNS) の VIP と IP の範囲は](https://www.microsoft.com/download/details.aspx?id=44238)します。 確認してください、最新の情報があるかどうかを確認するには、定期的にバックアップします。 


### <a name="fqdns-vips-and-ips"></a>Fqdn、Vip、および ip アドレス
それに続くテーブルで各次の XML ドキュメント内の要素の説明 (で[用語と表記](#terms-and-notations)します。 IP の範囲は、Fqdn を一定に保たれます Fqdn のみを使用することを促すには、このドキュメントから意図的にありました。 ただし、ダウンロード センターから完全な一覧を含む XML ファイルをダウンロードすることができます。[Windows 通知サービス (WNS) の VIP と IP の範囲は](https://www.microsoft.com/download/details.aspx?id=44238)します。 新しい Vip または IP 範囲がある**1 つの日の週はアップロード後**します。

```XML
<?xml version="1.0" encoding="UTF-8"?>
<WNSPublicIpAddresses xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
    <!-- This file contains the FQDNs, VIPs, and IP address ranges used by the Windows Notification Service. A new text file will be uploaded every time a new VIP or IP range is released in production.  Please copy the below information and perform the necessary changes on your site. Endpoints in CloudService nodes are used for cloud services to send notifications to WNS. Endpoints in Client nodes are used by devices to receive notifications from WNS. --> 
    <CloudServiceDNS>
    <DNS FQDN="*.notify.windows.com"/>
    </CloudServiceDNS>
    <ClientDNS>
        <DNS FQDN="*.wns.windows.com"/>
        <DNS FQDN="*.notify.live.net"/>
    </ClientDNS>
    <CloudServiceIPs>
        <IpRange Subnet=""/>
        <!-- See the file in Download Center for the complete list of IP ranges -->
    </CloudServiceIPs>
    <ClientIPsIPv4>
        <IpRange Subnet=""/>
        <!-- See the file in Download Center for the complete list of IP ranges -->
    </ClientIPsIPv4>
</WNSPublicIpAddresses>

```

### <a name="terms-and-notations"></a>用語との表記
表記と上記の XML スニペットで使用される要素の説明を次に示します。

| 用語 | 説明 |
|---|---|
| **ドット数値記法 (つまり 64.4.28.0/26)** | ドット数値記法は、範囲の IP アドレスを記述する方法です。 たとえば、64.4.28.0/26 64.4.28.0 の最初の 26 のビットは定数、最後の 6 ビットは変数を意味します。  この場合は、IPv4 の範囲は、64.4.28.0 - 64.4.28.63 します。 |
| **ClientDNS** | これらのクライアント デバイス (Windows Pc、デスクトップ) の完全修飾ドメイン名 (FQDN) フィルターは、WNS から通知を受信します。 これらは、WNS クライアント WNS 機能を使用するためにファイアウォールで許可する必要があります。  お勧め許可一覧に IP/VIP の範囲ではなく Fqdn によってこれらが変更されないためです。 |
| **ClientIPsIPv4** | これらは、クライアント デバイス (Windows Pc、デスクトップ) からアクセスされるサーバーの IPv4 アドレス WNS から通知を受信します。 |
| **CloudServiceDNS** | これらは、WNS に notificatios を送信する、クラウド サービスと対話する WNS サーバーの完全修飾ドメイン名 (FQDN) のフィルターです。 これらは、WNS 通知を送信するサービスのためにファイアウォールで許可する必要があります。  お勧め許可一覧に IP/VIP の範囲ではなく Fqdn によってこれらが変更されないためです。|
| **CloudServiceIPs** | CloudServiceIPs は cloud services に WNS 通知を送信するために使用するサーバーの IPv4 アドレスです。  |


## <a name="microsoft-push-notifications-service-mpns-public-ip-ranges"></a>Microsoft プッシュ通知サービス (MPNS) パブリック IP 範囲
MPNS、従来の通知サービスを使用している場合は、許可一覧に追加する必要があります IP アドレスの範囲は使用可能なダウンロード センターから。[Microsoft プッシュ通知サービス (MPNS) のパブリック IP の範囲は](https://www.microsoft.com/download/details.aspx?id=44535)します。


## <a name="related-topics"></a>関連トピック

* [クイック スタート:プッシュ通知を送信します。](https://msdn.microsoft.com/library/windows/apps/xaml/hh868252)
* [要求、作成、および通知チャネルを保存する方法](https://msdn.microsoft.com/library/windows/apps/hh465412)
* [アプリケーションを実行するための通知をインターセプトする方法](https://msdn.microsoft.com/library/windows/apps/xaml/jj709907.aspx)
* [Windows プッシュ通知サービス (WNS) とを認証する方法](https://msdn.microsoft.com/library/windows/apps/hh465407)
* [プッシュ通知サービスの要求および応答ヘッダー](https://msdn.microsoft.com/library/windows/apps/hh465435)
* [プッシュ通知のガイドラインとチェックリスト](https://msdn.microsoft.com/library/windows/apps/hh761462)
 
