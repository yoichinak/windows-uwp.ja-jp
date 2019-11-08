---
Description: マイクロソフトが発行する税関連の書類について、どのような開発者がその書類を受け取るのか、いつ書類が利用できるようになるのかなどを説明します。
title: マイクロソフトが発行する IRS の税関連の書類について
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, 課税, irs, 米国内国歳入庁, 税, 所得税, 1099
ms.assetid: 1e475b96-f953-457c-864f-b6f4cb4c309f
ms.localizationpriority: medium
ms.openlocfilehash: dc967961be7a96d04dfc744a229b4ca1f8f23a73
ms.sourcegitcommit: 445320ff0ee7323d823194d4ec9cfa6e710ed85d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/11/2019
ms.locfileid: "72282464"
---
# <a name="understand-irs-tax-forms-issued-by-microsoft"></a>マイクロソフトが発行する IRS の税関連の書類について

開発者は、所在地および受け取っている売上高や支払い額に応じて、毎年マイクロソフトから 1 種類以上の税関連の書類を受け取る可能性があります。 マイクロソフトはこれらの書類を発行し、米国内国歳入庁 (IRS) に提出する必要があります。

ここでは、どのような開発者が書類を受け取り、いつ利用可能になるのかなど、これらの書類について詳しく説明します。

## <a name="types-of-tax-forms"></a>税関連の書類の種類

| IRS の税関連の書類 | 説明 | 可用性 |
|--------------|-------------|--------------|
|1099-MISC、1099-K | マイクロソフトのマーケットプレースへの参加に関する販売アクティビティや開発者への支払いに関連します。 | 印刷されるフォームは postmarked**年1月 31**日以前になります。また、pdf コピーは、[パートナーセンター](https://partner.microsoft.com/dashboard) (**アカウント設定 > 税プロファイル**) で同時に利用できます。 |
|1042-S | 米国の源泉徴収税の対象になる開発者への支払いに関連します。 | 印刷されるフォームは、 **3 月 15**日以前に postmarked ます。また、pdf コピーは、パートナーセンター (**アカウント設定 > 税プロファイル**) で同時に利用できます。 |

> [!NOTE]
> IRS 税のフォームで使用する住所は、[税金プロファイル](setting-up-your-payout-account-and-tax-forms.md#tax-forms)のアドレスから取得されます。 住所を変更した場合は、 **[税プロファイル]** の住所も変更するようにしてください。

税金のフォームは、次のアドレスから送信されます。

**米国市民:**
<table>
<tr><th>ビジネスグループ</th><th>法人</th><th>Address</th></tr>
<tr><td>Windows、Office、Azure</td><td>Microsoft Corporation</td><td>One Microsoft Way<br>Redmond WA 98052 USA</td></tr>
<tr><td>広告</td><td>Microsoft Online Inc.</td><td>6100 Neil の道路<br>Reno、NV 89511 USA</td></tr>
<table> 

**米国以外の市民:**
<table>
<tr><th>ビジネスグループ</th><th>法人</th><th>Address</th></tr>
<tr><td>Windows、Office、Azure</td><td>Microsoft アイルランドの運用に限定されています (マイクロソフトは microsoft corporation によって microsoft corporation の修飾された仲介役として機能します)。</td><td>Microsoft の1か所<br>南部 @ no__t-0County @ no__t-1Business @ no__t-2Park<br>Leopardstown, ダブリン18アイルランド</td></tr>
<tr><td>掲載</td><td>Microsoft アイルランドの操作は制限されています (支払いは microsoft online inc. によって行われます)。 microsoft アイルランドは、microsoft Online Inc. の支払いエージェントとして機能します)。</td><td>Microsoft の1か所<br>南部 @ no__t-0County @ no__t-1Business @ no__t-2Park<br>Leopardstown, ダブリン18アイルランド</td></tr>
<tr><td>広告</td><td>Microsoft Online Inc.</td><td>6100 Neil の道路<br>Reno、NV 89511 USA</td></tr>
<tr><td colspan="3">* 次の国の市民による収益の提供は、Microsoft アイルランドの操作に限定されています。オーストリア、ベルギー、ブルガリア、クロアチア、キプロス、チェコ共和国、デンマーク、エストニア、フィンランド、フランス、ドイツ、ギリシャ、ハンガリー、アイルランド、マン島マン、イタリア、ラトビア、リヒテンシュタイン、リトアニア、ルクセンブルク、マルタ、モナコ、オランダ、ノルウェー、ポーランド、ポルトガル、ルーマニア、スロバキア、スロベニア、南アフリカ、スペイン、スウェーデン、スイス、英国</td></tr>
</table>

## <a name="for-developers-located-in-the-united-states"></a>米国在住の開発者の場合

<table>
  <tr>
     <th>米国在住の開発者で有料アプリを販売しており、以下の条件を満たす場合 </th>
     <th> 以下の書類を受け取る</th>
  </tr>
  <tr> 
     <td valign="top">適用される税年度に、<b>アプリの販売数が 200 個を上回り</b>、アプリ販売の合計購入金額が <b>20,000 米国ドルを超えました</b> (Windows 10 の Microsoft Store 経由でブラジルおよび中国で販売された数は<b>含まれません</b>)。</td>
    <td valign="top"><b>1099-K</b>:<br>Ip4700Microsoft Corporation<br>中: * * * * * 4442<br><br><b>重要</b>: フォーム 1099-K には、支払いではなく、<b>総購入</b>金額が含まれています。</td>
  </tr>
  <tr> 
     <td valign="top">(i) Windows 10 の Microsoft Store 経由でブラジルおよび中国で販売したアプリまたは (ii) Minecraft Marketplace マーケットプレースでの売り上げについて、<b>10 ドル以上の支払い額</b>を受け取りました。<br>
<br>
<b>OR</b><br>
<br>
適用されている課税年度 (インセンティブ支払いやコンテストからの支払いなど) において、マイクロソフトからのアプリの販売に関連しない支払いを $600 以上受け取りました。</td>
    <td valign="top"><b>1099-MISC</b>:<br>納税Microsoft Corporation<br>中: * * * * * 4442<br><br><b>重要</b>: 特定のビジネスエンティティは、Microsoft から受け取った支払い額に関係なく、1099のその他の形式を受け取ることはありません。  詳細については、税務の専門家にお問い合わせください。</td>
  </tr>
  <tr>
    <td valign="top">上のいずれの理由も該当しません。</td>
    <td valign="top">なし</td>
  </tr>
  <tr>
    <td valign="top">&nbsp;</td>
    <td valign="top">&nbsp;</td>
  </tr>
  <tr>
     <th>アプリで広告を販売している米国開発者の方は、 </th>
     <th> 以下の書類を受け取る</th>
  </tr>
  <tr> 
     <td valign="top">適用される税年度に、アプリ内広告によって <b>600 ドル以上の支払い</b>を受け取りました。</td>
    <td valign="top"><b>1099-MISC</b>:<br>納税Microsoft Online Inc.<br>中: * * * * * 0505<br><br><b>重要</b>: 特定のビジネスエンティティは、Microsoft から受け取った支払い額に関係なく、1099のその他の形式を受け取ることはありません。  詳細については、税務の専門家にお問い合わせください。</td>
  </tr>
  <tr> 
     <td valign="top">適用される税年度に、アプリ内広告によって <b>600 ドル未満の支払い</b>を受け取りました。</td>
     <td valign="top">なし</td>
  </tr>
</table>


## <a name="for-developers-located-outside-of-the-united-states"></a>米国外に在住する開発者の場合

<table>
  <tr>
    <td valign="top"><b>I は、Microsoft から 1042-S という形式のフォームを受信しました。何を目的としていますか? </b></td>
    <td valign="top">マイクロソフトが 1042-S フォームを送った理由は、米国の税務当局に申告義務があると見なされている、源泉徴収税の対象になった収益を、マイクロソフトが開発者に対して支払ったためです。  フォーム 1042-S は、この報告義務のために使用されます。</td>
  </tr>
  <tr>
    <td valign="top"><b>フォームで何を行う必要がありますか。</b></td>
    <td valign="top">一般的に、開発者側で特別な対応は必要ありません。 フォーム 1402-S は、開発者が管轄の税務当局に任意の形式の税額控除を申請する場合に役立つ可能性があります。  このトピックについて詳しくは、税金アドバイザーにお問い合わせください。</td>
  </tr>
  <tr>
    <td valign="top"><b>W8 フォームを完了したときに支払いに課税されたのはなぜですか。</b></td>
    <td valign="top">税金は次のいずれかの場合に源泉徴収されます。<br>
     1. W8 の税条約のセクションを正しく完了していません。<br>
     2. は、米国の税条約がない国に居住しています。<br><br>いつでもパートナーセンターにアクセスして、更新された W8 フォームを送信できます。<br><br><b>注意</b>:すべての収入が税金源泉徴収の対象になるわけではありません。</td>
  </tr>
  <tr>
    <td valign="top"><b>I は、有効な条約情報を含む更新された W8 フォームを送信しました。Microsoft は、源泉徴収された税金を返金できますか? </b></td>
    <td valign="top">源泉徴収された税金を払い戻すことはできません。 これらの税金について地域で控除を請求できるかどうか、または IRS から払い戻しを求められるかどうかについては、税金アドバイザーにご相談ください。</td>
  </tr>
  <tr>
    <td valign="top"><b>1042-S フォームで報告される売上</b></td>
    <td valign="top"><b>源泉徴収の対象として分類された、米国在住の購入者に対する</b>売り上げのみ報告義務があります。  その他の売り上げはすべて報告義務があるとは見なされていません。</td>
  </tr>
  <tr>
    <td valign="top"><b>1つの封筒で 1042-S という同じ形式のコピーを3つ取得したのはなぜですか。</b></td>
    <td valign="top">IRS 規則により、次の 3 つの目的で書類を 3 部提供する必要があります。
<ul>
<li>受取人の記録のため</li>
<li>米国連邦政府の所得税申告に提出するため (該当する場合)</li>
<li>米国の所得税申告に提出するため (該当する場合)</li>
</ul></td>
  </tr>
</table>


> [!NOTE]
> **IRS の税関連の書類**に関する質問や懸案事項が他にもある場合は、[サポート チケット](https://aka.ms/storesupport)を作成してください。 マイクロソフトは特定の税金の事情に関する質問にはお答えできません。これらの質問については、税務の専門家にお問い合わせください。
