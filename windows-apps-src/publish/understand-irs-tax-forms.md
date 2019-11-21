---
Description: マイクロソフトが発行する税関連の書類について、どのような開発者がその書類を受け取るのか、いつ書類が利用できるようになるのかなどを説明します。
title: マイクロソフトが発行する IRS の税関連の書類について
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, 課税, irs, 米国内国歳入庁, 税, 所得税, 1099
ms.assetid: 1e475b96-f953-457c-864f-b6f4cb4c309f
ms.localizationpriority: medium
ms.openlocfilehash: 55143f109398aae1988b7ac0d060cda138e7e48e
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258966"
---
# <a name="understand-irs-tax-forms-issued-by-microsoft"></a>マイクロソフトが発行する IRS の税関連の書類について

開発者は、所在地および受け取っている売上高や支払い額に応じて、毎年マイクロソフトから 1 種類以上の税関連の書類を受け取る可能性があります。 マイクロソフトはこれらの書類を発行し、米国内国歳入庁 (IRS) に提出する必要があります。

ここでは、どのような開発者が書類を受け取り、いつ利用可能になるのかなど、これらの書類について詳しく説明します。

## <a name="types-of-tax-forms"></a>税関連の書類の種類

| IRS の税関連の書類 | 説明 | 対象 |
|--------------|-------------|--------------|
|1099-MISC、1099-K | マイクロソフトのマーケットプレースへの参加に関する販売アクティビティや開発者への支払いに関連します。 | Printed forms will be postmarked on or before **January 31**, and .pdf copies will be available in [Partner Center](https://partner.microsoft.com/dashboard) (in **Account settings > Tax profile**) at the same time |
|1042-S | 米国の源泉徴収税の対象になる開発者への支払いに関連します。 | Printed forms will be postmarked on or before **March 15**, and .pdf copies will be available in Partner Center (in **Account settings > Tax profile**) at the same time |

> [!NOTE]
> The address we use on IRS tax forms comes from the address in your [Tax profile](setting-up-your-payout-account-and-tax-forms.md#tax-forms). 住所を変更した場合は、 **[税プロファイル]** の住所も変更するようにしてください。

The tax forms will be sent to you from the following addresses:

**US Citizens:**
<table>
<tr><th>Business Group</th><th>Legal Entity</th><th>Address</th></tr>
<tr><td>Windows, Office, Azure</td><td>Microsoft Corporation</td><td>One Microsoft Way<br>Redmond WA 98052 USA</td></tr>
<tr><td>広告</td><td>Microsoft Online Inc.</td><td>6100 Neil Road<br>Reno, NV 89511 USA</td></tr>
<table> 

**Non-US Citizens:**
<table>
<tr><th>Business Group</th><th>Legal Entity</th><th>Address</th></tr>
<tr><td>Windows, Office, Azure</td><td>Microsoft Ireland Operations Limited (Payment is made by Microsoft Corporation via Microsoft Ireland acting as qualified intermediary for Microsoft Corporation)</td><td>One Microsoft Place<br>South&nbsp;County&nbsp;Business&nbsp;Park<br>Leopardstown, Dublin 18 Ireland</td></tr>
<tr><td>Advertising *</td><td>Microsoft Ireland Operations Limited (Payment is made by Microsoft Online Inc. via Microsoft Ireland acting as payout agent for Microsoft Online Inc.)</td><td>One Microsoft Place<br>South&nbsp;County&nbsp;Business&nbsp;Park<br>Leopardstown, Dublin 18 Ireland</td></tr>
<tr><td>広告</td><td>Microsoft Online Inc.</td><td>6100 Neil Road<br>Reno, NV 89511 USA</td></tr>
<tr><td colspan="3">* Citizens of the following countries earning Advertising revenue will be paid through Microsoft Ireland Operations Limited: Austria, Belgium, Bulgaria, Croatia, Cyprus, Czech Republic, Denmark, Estonia, Finland, France, Germany, Greece, Hungary, Ireland, Isle of Man, Italy, Latvia, Liechtenstein, Lithuania, Luxembourg, Malta, Monaco, Netherlands, Norway, Poland, Portugal, Romania, Slovakia, Slovenia, South Africa, Spain, Sweden, Switzerland, United Kingdom</td></tr>
</table>

## <a name="for-developers-located-in-the-united-states"></a>米国在住の開発者の場合

<table>
  <tr>
     <th>米国在住の開発者で有料アプリを販売しており、以下の条件を満たす場合 </th>
     <th> 以下の書類を受け取る</th>
  </tr>
  <tr> 
     <td valign="top">適用される税年度に、<b>アプリの販売数が 200 個を上回り</b>、アプリ販売の合計購入金額が <b>20,000 米国ドルを超えました</b> (Windows 10 の Microsoft Store 経由でブラジルおよび中国で販売された数は<b>含まれません</b>)。</td>
    <td valign="top"><b>1099-K</b> :<br>提出者: Microsoft Corporation<br>EIN: *****4442<br><br><b>Important</b>: Form 1099-K contains <b>gross purchase</b> amounts, not payments made to you.</td>
  </tr>
  <tr> 
     <td valign="top">(i) Windows 10 の Microsoft Store 経由でブラジルおよび中国で販売したアプリまたは (ii) Minecraft Marketplace マーケットプレースでの売り上げについて、<b>10 ドル以上の支払い額</b>を受け取りました。<br>
<br>
<b>OR</b><br>
<br>
I received at least $600 in payments not related to app sales from Microsoft in the applicable tax year (for example, incentive payments or payments from a contest or promotion)</td>
    <td valign="top"><b>1099-MISC</b> :<br>支払者: Microsoft Corporation<br>EIN: *****4442<br><br><b>Important</b>: Certain business entities will not receive 1099-MISC forms regardless of the payment amounts received from Microsoft.  詳細については、税務の専門家にお問い合わせください。</td>
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
     <th>If I'm a United States developer selling ads in apps and ... </th>
     <th> 以下の書類を受け取る</th>
  </tr>
  <tr> 
     <td valign="top">適用される税年度に、アプリ内広告によって <b>600 ドル以上の支払い</b>を受け取りました。</td>
    <td valign="top"><b>1099-MISC</b> :<br>支払い人: Microsoft Online Inc<br>EIN: *****0505<br><br><b>Important</b>: Certain business entities will not receive 1099-MISC forms regardless of the payment amounts received from Microsoft.  詳細については、税務の専門家にお問い合わせください。</td>
  </tr>
  <tr> 
     <td valign="top">適用される税年度に、アプリ内広告によって <b>600 ドル未満の支払い</b>を受け取りました。</td>
     <td valign="top">なし</td>
  </tr>
</table>


## <a name="for-developers-located-outside-of-the-united-states"></a>米国外に在住する開発者の場合

<table>
  <tr>
    <td valign="top"><b>I received a form 1042-S from Microsoft. What is it for?</b></td>
    <td valign="top">マイクロソフトが 1042-S フォームを送った理由は、米国の税務当局に申告義務があると見なされている、源泉徴収税の対象になった収益を、マイクロソフトが開発者に対して支払ったためです。  フォーム 1042-S は、この報告義務のために使用されます。</td>
  </tr>
  <tr>
    <td valign="top"><b>What should I do with the forms?</b></td>
    <td valign="top">一般的に、開発者側で特別な対応は必要ありません。 フォーム 1402-S は、開発者が管轄の税務当局に任意の形式の税額控除を申請する場合に役立つ可能性があります。  このトピックについて詳しくは、税金アドバイザーにお問い合わせください。</td>
  </tr>
  <tr>
    <td valign="top"><b>Why was tax withheld on my payments when I completed a W8 form?</b></td>
    <td valign="top">税金は次のいずれかの場合に源泉徴収されます。<br>
     1. You did not complete the tax treaty section of the W8 correctly, or<br>
     2. You are resident in a country that does not have a tax treaty with the United States.<br><br>You can visit Partner Center at any time to submit an updated W8 form.<br><br><b>Note</b>: Not all income is subject to tax withholding.</td>
  </tr>
  <tr>
    <td valign="top"><b>I submitted an updated W8 form with valid treaty information. Can Microsoft refund me the tax that was withheld?</b></td>
    <td valign="top">源泉徴収された税金を払い戻すことはできません。 これらの税金について地域で控除を請求できるかどうか、または IRS から払い戻しを求められるかどうかについては、税金アドバイザーにご相談ください。</td>
  </tr>
  <tr>
    <td valign="top"><b>What sales are reported on form 1042-S?</b></td>
    <td valign="top"><b>源泉徴収の対象として分類された、米国在住の購入者に対する</b>売り上げのみ報告義務があります。  その他の売り上げはすべて報告義務があるとは見なされていません。</td>
  </tr>
  <tr>
    <td valign="top"><b>Why did I get 3 copies of the same form 1042-S in one envelope?</b></td>
    <td valign="top">IRS 規則により、次の 3 つの目的で書類を 3 部提供する必要があります。
<ul>
<li>受取人の記録のため</li>
<li>米国連邦政府の所得税申告に提出するため (該当する場合)</li>
<li>米国の所得税申告に提出するため (該当する場合)</li>
</ul></td>
  </tr>
</table>


> [!NOTE]
> **IRS の税関連の書類**に関する質問や懸案事項が他にもある場合は、[サポート チケット](https://developer.microsoft.com/windows/support)を作成してください。 マイクロソフトは特定の税金の事情に関する質問にはお答えできません。これらの質問については、税務の専門家にお問い合わせください。
