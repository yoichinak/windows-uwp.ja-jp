---
Description: 入金状況には、アプリやアドオンによる売り上げの詳細が表示されます。 支払いを受ける時期と支払われる金額も示されます。
title: 支払いの概要
ms.assetid: F0D070BE-8267-4CC9-B0D2-085EBA74AC98
ms.date: 08/02/2019
ms.topic: article
keywords: windows 10, uwp, 入金状況, ステートメントの, 支払い額, 売り上げ, 入金い, 支払い, 収益
ms.localizationpriority: medium
ms.openlocfilehash: d609af268cfe304b34797cea4bf91e36d1475c29
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259005"
---
# <a name="payout-summary"></a>支払いの概要

The **Payout summary** shows you details about the money you’ve earned with Microsoft. 支払いを受ける時期と支払われる金額も示されます。

Azure Marketplace で製品を販売する場合、 **[入金状況]** に正常に行われた支払いに関する情報も表示されます。 Azure Marketplace での支払いについて詳しくは、[Microsoft Azure Marketplace への参加ポリシーに関するページ](https://docs.microsoft.com/legal/marketplace/participation-policy)と [Microsoft Azure Marketplace の発行元契約に関するページ](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE3ypvt)をご覧ください。

> [!NOTE]
> To be eligible for payout, your proceeds must reach the [payment threshold](payment-thresholds-methods-and-timeframes.md) of $50. For details about the payment threshold see this page and review the app developer agreement.

## <a name="access-the-payout-summary-pages"></a>Access the payout summary pages

To open one of the payout summary pages:

1. Select the Money icon in the upper-right corner.
2. Select Payments, Transaction history, or Export data.

## <a name="payments-page"></a>Payments page

The totals on this page represent all of the programs you participate in. You can filter by Participant ID, Program, Payment ID, and Earning type. Amounts are given in US dollars. The paid value is also displayed in pay to currency.

| エリア                   | 説明                                                                                  |
|------------------------|----------------------------------------------------------------------------------------------|
| Total paid this year   | The combined total paid out to you this year, in US dollars, for all of your programs.       |
| Next estimated payment | The single next payment coming to you (even if there are others coming soon), in US dollars. |
| Last payment           | The amount (in US dollars), program name, and program of your most recent payment.           |
| Payments by source     | Amount of payments, in US dollars, represented by program over the last 12 months.           |
| 支払い               | Select Paid or Pending and then sort as you like. For additional details of a specific payment select View. To download a copy of the payment remittance statement, select Download. Note that transaction history data may take up to 24 hours to appear, so you may not see associated earnings right away. |

To export any of the data on this page, select Export and then follow directions on the Export data page.

## <a name="transaction-history-page"></a>Transaction history page

This page displays all of your individual earnings, including the date, type, and earning for each. You can select a time period to view, and you can also filter by Enrollment ID, Program, Payment ID, Earning type, Lever, and Status. Data is available for the current fiscal year (July 1 – June 30) and the previous two fiscal years.

To see more details about an earning, select the down arrow at the right-hand side of the page. This will display the lever, revenue amount, and product. If for some reason any of this data is unavailable, but you need access to it, contact [support](https://developer.microsoft.com/en-us/windows/support)]. If the earning is the result of an adjustment, and not a transaction, the product fields will not be displayed.

To export any of the transaction data on this page, select Export and then follow directions on the Export data page. Files exported from the Transaction History page show data in transaction currency, earnings in both transaction currency and US dollars,and the paid value in pay to currency.

## <a name="payment-status"></a>入金状況

| Earning status           | 原因                                                                                                                                      | Partner action required?                                   |
|--------------------------|---------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------|
| Unprocessed              | The earning is eligible for payment. It stays in this state for a cooling period as defined in the program guide for the Incentive program. | 必須ではない                                                         |
| Upcoming                 | Payment order generated pending internal reviews before payment is processed.                                                               | 必須ではない                                                         |
| Pending tax invoice      | Your tax invoice is incomplete or invalid.                                                                                                  | You need to update your tax invoice before you can be paid |
| Rejected during review   | The payment was rejected during review.                                                                                                     | Contact [Microsoft support](https://developer.microsoft.com/en-us/windows/support) for details                      |
| Failed                   | The payment failed due to a Microsoft system error.                                                                                         | Contact [Microsoft support](https://developer.microsoft.com/en-us/windows/support)  for details                      |
| 進行中              | The payment is in progress.                                                                                                                 | 必須ではない                                                         |
| Incorrect payment        | The payment recouping is in progress.                                                                                                       | 必須ではない                                                         |
| Sent                     | The payment has been sent to your bank.                                                                                                     | 必須ではない                                                         |
| Reprocessing             | The payment encountered a Microsoft system error and is being reprocessed.                                                                  | 必須ではない                                                         |
| 反転                 | The payment was reversed by your bank and will be sent again in the next payment cycle.                                                     | 必須ではない                                                         |
| Tax invoice rejected     | Your tax invoice was rejected during review. All pending payments will be on hold until the tax invoice review is complete.                 | Contact [Microsoft support](https://developer.microsoft.com/en-us/windows/support)  for details                      |
| Tax invoice under review | Your tax invoice is being reviewed. Your payment will be released once the tax invoice has been approved.                                   | 必須ではない                                                         |
| Rejected                 | The payment was rejected by your bank.                                                                                                      | Contact your bank for details.                             |

## <a name="export-data-page"></a>Export data page

Follow the instructions on this page to export the data you want.

注:

- The Export data page does not refresh on its own. You may need to refresh the page manually to see the most recent data.
- Your filter may result in a No data available error. This probably means you’ve left the default time period selected at three months, and then selected a Payment ID from an earning that’s outside of that period. Expand your time period and try again.

## <a name="payment-download-export"></a>Payment download export

This option provides a download of the payments you received in your bank for a given program, the associated tax, and aggregated earning amount. This report is used for many Partner Center programs, so some columns may be inapplicable to your report. Those columns are marked below.

| 列名              | 説明                                                                                                                               |
|--------------------------|-----------------------------------------------------------------------------------------------------------------------------------------  |
| participantID            | The primary identity of the partner earning under the program                                                                             |
| participantIDType        | Usually program id for Incentive programs and Seller ID for Store programs                                                                |
| participantName          | Name of the earning partner                                                                                                               |
| programName              | Incentive/store program name                                                                                                              |
| earned                   | Amount earned in the Pay To currency for that program/participantID                                                                       |
| earnedUSD                | Amount earned for the program/participant ID, in USD                                                                                      |
| withheldTax              | Amount of tax withheld in the Pay To currency for the program/participantID                                                               |
| salesTax                 | Total amount of sales tax in the Pay To currency for the program/participantID (applicable for incentive programs only)                   |
| serviceFeeTax            | Total amount of serviceFeeTax in Pay To currency for the program/participantID (applicable for store programs and Azure Marketplace only) |
| totalPayment             | Total payment in local currency excluding the withholding tax and including the sales tax (if applicable) for the program/participantID   |
| currencyCode             | Pay To currency code                                                                                                                      |
| paymentMethod            | The method used to pay the partner for example, electronic bank transfer, credit note                                                             |
| paymentID                | Unique identifier for the payment. This number is usually visible in your bank statement. (applicable for SAP payments only)              |
| paymentStatus            | 入金状況                                                                                                                            |
| paymentStatusDescription | Friendly description of payment status                                                                                                    |
| paymentDate              | Date payment was sent from Microsoft                                                                                                      |

## <a name="transaction-history-download-export"></a>Transaction history download export

This option provides a download of each earning line item you see in the Transaction history page, earning type, date, associated transaction amount, customer, product, and other transactional details applicable to your programs.

| 列名                    | 説明                                                                                                                              | Applicability for Incentives/Store/Azure Marketplace           |
|--------------------------------|------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------|
| earningId                      | Unique identifier for each earning                                                                                                       | すべての                                                            |
| participantId                  | The primary identity of the partner earning under the program                                                                            | すべての                                                            |
| participantIdType              | Mostly program ID for Incentive programs and Seller IF for Store programs and Azure Marketplace                                          | すべての                                                            |
| participantName                | Name of the earning partner                                                                                                              | すべての                                                            |
| partnerCountryCode             | Location/country of the earning partner                                                                                                  | すべての                                                            |
| programName                    | Incentive/store program name                                                                                                             | すべての                                                            |
| transactionId                  | Unique identifier for the transaction                                                                                                    | すべての                                                            |
| transactionCurrency            | Currency in which the original customer transaction occurred (this is not partner location currency)                                     | すべての                                                            |
| transactionDate                | Date of the transaction. Useful for programs where many transactions contribute to one earning                                           | すべての                                                            |
| transactionExchangeRate        | Exchange rate date used to show the corresponding transaction USD amount                                                                 | すべての                                                            |
| transactionAmount              | Transaction amount in the original transaction currency based on which earning is generated                                              | すべての                                                            |
| transactionAmountUSD           | Transaction amount in USD                                                                                                                | すべての                                                            |
| lever                          | Indicates business rule for the earning                                                                                                  | すべての                                                            |
| earningRate                    | Incentive rate applied on transaction amount to generate an earning                                                                      | すべての                                                            |
| quantity                       | Varies based on program. Indicates billed quantity for transactional programs                                                            | すべての                                                            |
| quantityType                   | Indicates type of quantity for example, Billed quantity, MAU                                                                                     | すべての                                                            |
| earningType                    | Indicates if it is fee, rebate, coop, sell etc.                                                                                          | すべての                                                            |
| earningAmount                  | Earning Amount in the original transaction currency                                                                                      | すべての                                                            |
| earningAmountUSD               | Earning Amount in USD                                                                                                                    | すべての                                                            |
| earningDate                    | Date of the earning                                                                                                                      | すべての                                                            |
| calculationDate                | Date the earning was calculated in the system                                                                                            | すべての                                                            |
| earningExchangeRate            | Exchange rate used to show the corresponding USD amount                                                                                  | すべての                                                            |
| exchangeRateDate               | Exchange rate date used to calculate EarningAmount USD                                                                                   | すべての                                                            |
| paymentAmountWOTax             | Earning amount (without tax) in Pay To currency for “Sent” payments only                                                                 | すべての                                                            |
| paymentCurrency                | Pay to currency chosen by partner in the Payment profile. Shown only for sent payments                                                   | すべての                                                            |
| paymentExchangeRate            | Exchange rate used to calculate paymentAmountWOTax in payment currency using ExchangeRateDate                                            | すべての                                                            |
| claimId                        | Unique identifier for claim                                                                                                              | Incentives - some programs only                                |
| planId                         | Unique identifier for plan                                                                                                               | Incentives - some programs only                                |
| paymentId                      | Unique identifier for the payment. This number is usually visible in your bank statement                                                 | SAP payments only                                              |
| paymentStatus                  | 入金状況                                                                                                                           | すべての                                                            |
| paymentStatusDescription       | Friendly description of payment status                                                                                                   | すべての                                                            |
| customerId                     | Will always be blank                                                                                                                     | Incentive programs only (exception: OEM) and Azure Marketplace |
| customerName                   | Will always be blank                                                                                                                     | Incentive programs only (exception: OEM) and Azure Marketplace |
| partNumber                     | Will always be blank                                                                                                                     | Some Incentive and Store programs and Azure Marketplace        |
| productName                    | Product name linked to transaction                                                                                                       | すべての                                                            |
| productId                      | 一意の製品識別子                                                                                                                | Store and Azure Marketplace                                    |
| parentProductId                | 一意の親製品識別子。 注意: トランザクションの親製品がない場合、親製品 ID = 製品 ID です。 | Store and Azure Marketplace                                    |
| parentProductName              | 親製品の名前。 注意: トランザクションの親製品がない場合、親製品名 = 製品名です。   | Store and Azure Marketplace                                    |
| productType                    | 製品の種類 (アプリ、アドオン、ゲームなど)                                                                                        | Store and Azure Marketplace                                    |
| invoiceNumber                  | Invoice number (applicable for EA only)                                                                                                  | Incentive and Azure Marketplace - some programs only           |
| subscriptionId                 | Subscription identifier associated with customer                                                                                         | Incentive - some programs only                                 |
| subscriptionStartDate          | サブスクリプション開始日                                                                                                                  | Incentive - some programs only                                 |
| subscriptionEndDate            | サブスクリプション終了日                                                                                                                    | Incentive - some programs only                                 |
| resellerId                     | Reseller identifier                                                                                                                      | Incentive - some programs only                                 |
| resellerName                   | 販売店名                                                                                                                            | Incentive - some programs only                                 |
| distributorId                  | Distributor identifier                                                                                                                   | Incentive - some programs only                                 |
| distributorName                | Distributor name                                                                                                                         | Incentive - some programs only                                 |
| agreementNumber                | 契約番号                                                                                                                         | Incentive - some programs only                                 |
| agreementStartDate             | 契約開始日                                                                                                                     | Incentive - some programs only                                 |
| agreementEndDate               | 契約終了日                                                                                                                       | Incentive - some programs only                                 |
| workload                       | ワークロード                                                                                                                                 | Incentive - some programs only                                 |
| transactionType                | 取引の種類 (購入、払戻し、取り消し、支払取り消しなど)                                                               | Store and Azure Marketplace                                    |
| localProviderSeller            | 登録のあるローカル プロバイダーまたは販売元                                                                                                          | Store only                                                     |
| taxRemitted                    | 徴収された税額 (売上税、使用税、または VAT/GST 税)。                                                                                   | Store and Azure Marketplace                                    |
| taxRemitModel                  | 徴収される税金の種類 (売上税、使用税、または VAT/GST 税)。                                                                    | Store only                                                     |
| storeFee                       | The amount retained by Microsoft as a fee for making the app or add-on available in the Store.                                           | Store only                                                     |
| transactionPaymentMethod       | 取引にユーザーが使用した支払い方法 (クレジット カード、携帯電話会社による課金、PayPal など)                                | Store and Azure Marketplace                                    |
| tpan                           | Indicates the third-party ad network                                                                                                     | Store - Ads only                                               |
| customerCountry                | Customer country                                                                                                                         | Store and Azure Marketplace                                    |
| customerCity                   | Customer city                                                                                                                            | Store and Azure Marketplace                                    |
| customerState                  | Customer state                                                                                                                           | Store and Azure Marketplace                                    |
| customerZip                    | Customer zip/postal code                                                                                                                 | Store and Azure Marketplace                                    |
| purchaseTypeCode               | Will always be blank                                                                                                                     | Incentive program - CRI                                        |
| purchaseOrderType              | Will always be blank                                                                                                                     | Incentive program - CRI                                        |
| purchaseOrderCoverageStartDate | Will always be blank                                                                                                                     | Incentive program - CRI                                        |
| purchaseOrderCoverageEndDate   | Will always be blank                                                                                                                     | Incentive program - CRI                                        |
| programOfferingLevel           |                                                                                                                                          | Incentive program - CRI                                        |
| TenantID                       |                                                                                                                                          | Incentive programs                                             |
| externalReferenceId            | Unique identifier for the program                                                                                                        | Direct Pay programs (Incentive and Store)                      |
| externalReferenceIdLabel       | Unique identifier label                                                                                                                  | Direct Pay programs (Incentive and Store)                      |
