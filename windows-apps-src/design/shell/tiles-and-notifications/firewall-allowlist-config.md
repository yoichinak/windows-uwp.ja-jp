---
Description: 多くの企業では、ファイアウォールを使用して不要なトラフィックをブロックしています。 このドキュメントでは、WNS トラフィックがファイアウォールを通過することを許可する方法について説明します。
title: ファイアウォール許可リストへの WNS トラフィックの追加
ms.assetid: 2125B09F-DB90-4515-9AA6-516C7E9ACCCD
template: detail.hbs
ms.date: 05/20/2019
ms.topic: article
keywords: windows 10、uwp、WNS、windows notification service、通知、windows、ファイアウォール、トラブルシューティング、IP、トラフィック、エンタープライズ、ネットワーク、IPv4、VIP、FQDN、パブリック IP アドレス
ms.localizationpriority: medium
ms.openlocfilehash: 4277b46728464630bf478b1f78008e92b4e3fe99
ms.sourcegitcommit: 41dbee78d827107c224a9136c26f90be4dfe12ad
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/21/2020
ms.locfileid: "90845531"
---
# <a name="enterprise-firewall-and-proxy-configurations-to-support-wns-traffic"></a>WNS トラフィックをサポートするためのエンタープライズファイアウォールとプロキシ構成

## <a name="background"></a>背景
多くの企業では、ファイアウォールを使用して、不要なネットワークトラフィックとポートをブロックしています。残念ながら、これは Windows Notification Service の通信などの重要な機能もブロックする可能性があります。 つまり、WNS を介して送信されるすべての通知は、特定のネットワーク構成で削除されます。 これを回避するために、ネットワーク管理者は、承認済みの WNS Fqdn または Vip の一覧を除外リストに追加して、WNS トラフィックがファイアウォールを通過できるようにすることができます。 次に、さまざまなプロキシの種類のサポートに加え、追加の方法と用途について詳しく説明します。

## <a name="proxy-support"></a>プロキシのサポート

> [!Note]
> Windows での WNS プッシュ通知は、現在、すべてのプロキシをサポートしていません。 最適な結果を得るには、WNS への接続が直接接続である必要があります。

さまざまなネットワーク構成、プロキシ、およびファイアウォールを積極的に調査しています。 このページは、一般的なエンタープライズシナリオと WNS サポートについて、さらに詳しく説明します。


## <a name="what-information-should-be-added-to-the-allowlist"></a>許可リストに追加する必要がある情報
Windows Notification Service によって使用される Fqdn、Vip、および IP アドレスの範囲を含む一覧を次に示します。 

> [!IMPORTANT]
> FQDN でリストを許可することを強くお勧めします。これは変更されないためです。 FQDN で一覧表示することを許可する場合は、IP アドレス範囲も許可する必要はありません。

> [!IMPORTANT]
> IP アドレス範囲は定期的に変更されます。このため、このページには含まれていません。 IP 範囲の一覧を表示する場合は、ダウンロードセンターからファイルをダウンロードできます。 [Windows Notification Service (WNS) の VIP と IP の範囲](https://www.microsoft.com/download/details.aspx?id=44238)。 最新の情報があることを確認するために、定期的に確認してください。 


### <a name="fqdns-vips-ips-and-ports"></a>Fqdn、Vip、Ip、ポート
以下から選択する方法に関係なく、 **ポート 443**を使用して、一覧表示されている宛先へのネットワークトラフィックを許可する必要があります。 次の XML ドキュメントに記載されている各要素については、この後の表で説明します ( [用語と表記](#terms-and-notations))。 このドキュメントから除外された IP 範囲は、Fqdn が不変であるため、Fqdn のみを使用することをお勧めします。 ただし、ダウンロードセンターから完全な一覧を含む XML ファイル [(Windows Notification Service (WNS) の VIP と IP の範囲)](https://www.microsoft.com/download/details.aspx?id=44238)をダウンロードできます。 新しい Vip または IP 範囲は **、アップロードされてから1週間後に有効**になります。

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
    <IdentityServiceDNS>
        <DNS FQDN="login.microsoftonline.com"/>
        <DNS FQDN="login.live.com"/>
    </IdentityServiceDNS>
</WNSPublicIpAddresses>

```

### <a name="terms-and-notations"></a>用語と表記
次に、上記の XML スニペットで使用される表記法と要素について説明します。

| 項目 | 説明 |
|---|---|
| **ドット小数点表記 (64.4.28.0/26)** | ドット10進数表記は、IP アドレスの範囲を記述する方法です。 たとえば、64.4.28.0/26 は、64.4.28.0 の最初の26ビットが定数であり、最後の6ビットは変数であることを意味します。  この場合、IPv4 の範囲は 64.4.28.0-64.4.28.63 です。 |
| **ClientDNS** | これらは、WNS から通知を受け取るクライアントデバイス (Windows Pc、デスクトップ) の完全修飾ドメイン名 (FQDN) フィルターです。 WNS クライアントで WNS 機能を使用するためには、ファイアウォールを通過する必要があります。  IP/VIP 範囲ではなく Fqdn によってリストを許可することをお勧めします。これは変更されないためです。 |
| **ClientIPsIPv4** | これらは、WNS から通知を受け取るクライアントデバイス (Windows Pc、デスクトップ) によってアクセスされるサーバーの IPv4 アドレスです。 |
| **CloudServiceDNS** | これらは、通知を WNS に送信するためにクラウドサービスが通信する WNS サーバーの完全修飾ドメイン名 (FQDN) フィルターです。 これらは、サービスが WNS 通知を送信するために、ファイアウォール経由で許可されている必要があります。  IP/VIP 範囲ではなく Fqdn によってリストを許可することをお勧めします。これは変更されないためです。|
| **CloudServiceIPs** | CloudServiceIPs は、クラウドサービスが WNS に通知を送信するために使用するサーバーの IPv4 アドレスです。  |


## <a name="microsoft-push-notifications-service-mpns-public-ip-ranges"></a>Microsoft プッシュ通知サービス (MPNS) のパブリック IP 範囲
従来の notification service (MPNS) を使用している場合、許可リストに追加する必要がある IP アドレス範囲は、ダウンロードセンターの「 [Microsoft プッシュ通知サービス (mpns) のパブリック ip 範囲](https://www.microsoft.com/download/details.aspx?id=44535)」から入手できます。


## <a name="related-topics"></a>関連トピック

* [クイック スタート: プッシュ通知の送信](/previous-versions/windows/apps/hh868252(v=win.10))
* [通知チャネルを要求、作成、保存する方法](/previous-versions/windows/apps/hh465412(v=win.10))
* [実行中のアプリの通知を中断する方法](/previous-versions/windows/apps/jj709907(v=win.10))
* [Windows プッシュ通知サービス (WNS) に対して認証する方法](/previous-versions/windows/apps/hh465407(v=win.10))
* [プッシュ通知サービスの要求ヘッダーと応答ヘッダー](/previous-versions/windows/apps/hh465435(v=win.10))
* [プッシュ通知のガイドラインとチェック リスト](./windows-push-notification-services--wns--overview.md)
 
