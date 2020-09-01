---
title: 磁気ストライプ データの取得と理解
description: ユニバーサル Windows プラットフォーム (UWP) Point of Service (POS) Api を使用して、磁気ストライプリーダーからデータを取得し、解釈する方法について説明します。
ms.date: 10/04/2018
ms.topic: article
keywords: windows 10、uwp、point of service、pos、磁気ストライプリーダー
ms.localizationpriority: medium
ms.openlocfilehash: 2a405a66fbc243925c4c9bbd5b1ee499a9a7b9f8
ms.sourcegitcommit: e273e5901bfa6596dfef4cc741bb1c42614c25ab
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/01/2020
ms.locfileid: "89238277"
---
# <a name="obtain-and-understand-magnetic-stripe-data"></a>磁気ストライプ データの取得と理解

「 [サービスポイントの](pos-basics.md)概要」で説明されている手順に従って、アプリケーションに磁気ストライプリーダーを設定したら、そこからデータを取得する準備ができました。

## <a name="subscribe-to-datareceived-events"></a>* DataReceived イベントをサブスクライブする

リーダーがスワイプカードを認識するたびに、次の3つのイベントのいずれかが発生します。

* [AamvaCardDataReceived イベント](/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.aamvacarddatareceived): モーター車両カードがスワイプされたときに発生します。
* [BankCardDataReceived Event](/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.aamvacarddatareceived): 銀行カードがスワイプの場合に発生します。
* [VendorSpecificDataReceived Event](/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.vendorspecificdatareceived): ベンダー固有のカードがスワイプ場合に発生します。

アプリケーションでは、磁気ストライプリーダーでサポートされているイベントだけをサブスクライブする必要があります。 MagneticStripeReader でサポートされているカードの種類は、 [SupportedCardTypes](/uwp/api/windows.devices.pointofservice.magneticstripereader.supportedcardtypes)で確認できます。

次のコードは、3つの ***Datareceived** イベントをサブスクライブする方法を示しています。

```cs
private void SubscribeToEvents(ClaimedMagneticStripeReader claimedReader, MagneticStripeReader reader)
{
    foreach (var type in reader.SupportedCardTypes)
    {
        if (item == MagneticStripeReaderCardTypes.Aamva)
        {
            claimedReader.AamvaCardDataReceived += Reader_AamvaCardDataReceived;
        }
        else if (item == MagneticStripeReaderCardTypes.Bank)
        {
            claimedReader.BankCardDataReceived += Reader_BankCardDataReceived;
        }
        else if (item == MagneticStripeReaderCardTypes.ExtendedBase)
        {
            claimedReader.VendorSpecificDataReceived += Reader_VendorSpecificDataReceived;
        }
    }
}
```

イベントハンドラーには [ClaimedMagneticStripeReader](/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader) と *args* オブジェクトが渡され、その型はイベントによって異なります。

* **AamvaCardDataReceived** イベント: [MagneticStripeReaderAamvaCardDataReceivedEventArgs クラス](/uwp/api/windows.devices.pointofservice.magneticstripereaderaamvacarddatareceivedeventargs)
* **BankCardDataReceived** イベント: [MagneticStripeReaderBankCardDataReceivedEventArgs クラス](/uwp/api/windows.devices.pointofservice.magneticstripereaderbankcarddatareceivedeventargs)
* **VendorSpecificDataReceived** イベント: [MagneticStripeReaderVendorSpecificCardDataReceivedEventArgs クラス](/uwp/api/windows.devices.pointofservice.magneticstripereadervendorspecificcarddatareceivedeventargs)

## <a name="get-the-data"></a>データを取得する

**AamvaCardDataReceived**イベントと**BankCardDataReceived**イベントについては、 *args*オブジェクトからデータの一部を直接取得できます。 次の例では、いくつかのプロパティを取得し、それらをメンバー変数に代入する方法を示します。

```cs
private string _accountNumber;
private string _expirationDate;
private string _firstName;

private void Reader_BankCardDataReceived(
    ClaimedMagneticStripeReader sender, 
    MagneticStripeReaderBankCardDataReceivedEventArgs args)
{
    _accountNumber = args.AccountNumber;
    _expirationDate = args.ExpirationDate;
    _firstName = args.FirstName;
    // etc...
}
```

ただし、 **VendorSpecificDataReceived**イベントのすべてのデータを含む一部のデータは、 *args*パラメーターのプロパティである**Report**オブジェクトを使用して取得する必要があります。 これは [MagneticStripeReaderReport](/uwp/api/windows.devices.pointofservice.magneticstripereaderreport)型です。

[Cardtype](/uwp/api/windows.devices.pointofservice.magneticstripereaderreport.cardtype)プロパティを使用して、どの種類のカードがスワイプされているかを把握し、それを使用して、 [Track1](/uwp/api/windows.devices.pointofservice.magneticstripereaderreport.track1)、 [Track2](/uwp/api/windows.devices.pointofservice.magneticstripereaderreport.track2)、 [Track3](/uwp/api/windows.devices.pointofservice.magneticstripereaderreport.track3)、および[Track4](/uwp/api/windows.devices.pointofservice.magneticstripereaderreport.track4)のデータの解釈方法を通知できます。

各トラックのデータは、 [MagneticStripeReaderTrackData](/uwp/api/windows.devices.pointofservice.magneticstripereadertrackdata) オブジェクトとして表されます。 このクラスから、次の種類のデータを取得できます。

* [Data](/uwp/api/windows.devices.pointofservice.magneticstripereadertrackdata.data): 生データまたはデコードされたデータ。
* [DiscretionaryData](/uwp/api/windows.devices.pointofservice.magneticstripereadertrackdata.discretionarydata): 随意データ。 
* [EncryptedData](/uwp/api/windows.devices.pointofservice.magneticstripereadertrackdata.encrypteddata): 暗号化されたデータ。

次のコードスニペットは、レポートと追跡データを取得し、カードの種類を確認します。

```cs
private void GetTrackData(MagneticStripeReaderBankCardDataReceivedEventArgs args)
{
    MagneticStripeReaderReport report = args.Report;
    IBuffer track1 = report.Track1.Data;
    IBuffer track2 = report.Track2.Data;
    IBuffer track3 = report.Track3.Data;
    IBuffer track4 = report.Track4.Data;

    if (report.CardType == MagneticStripeReaderCardTypes.Aamva)
    {
        // Card type is AAMVA
    }
    else if (report.CardType == MagneticStripeReaderCardTypes.Bank)
    {
        // Card type is bank card
    }
    else if (report.CardType == MagneticStripeReaderCardTypes.ExtendedBase)
    {
        // Card type is vendor-specific
    }
    else if (report.CardType == MagneticStripeReaderCardTypes.Unknown)
    {
        // Card type is unknown
    }
}
```

[!INCLUDE [feedback](./includes/pos-feedback.md)]

## <a name="see-also"></a>関連項目

* [磁気ストライプ リーダー](pos-magnetic-stripe-reader.md)
* [ClaimedMagneticStripeReader クラス](/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader)
* [MagneticStripeReaderAamvaCardDataReceivedEventArgs クラス](/uwp/api/windows.devices.pointofservice.magneticstripereaderaamvacarddatareceivedeventargs)
* [MagneticStripeReaderBankCardDataReceivedEventArgs クラス](/uwp/api/windows.devices.pointofservice.magneticstripereaderbankcarddatareceivedeventargs)
* [MagneticStripeReaderVendorSpecificCardDataReceivedEventArgs クラス](/uwp/api/windows.devices.pointofservice.magneticstripereadervendorspecificcarddatareceivedeventargs)
* [MagneticStripeReaderReport](/uwp/api/windows.devices.pointofservice.magneticstripereaderreport)
* [MagneticStripeReaderTrackData](/uwp/api/windows.devices.pointofservice.magneticstripereadertrackdata)