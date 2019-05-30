---
title: スマート カード
description: このトピックでは、ユニバーサル Windows プラットフォーム (UWP) アプリでスマート カードを使ってユーザーをセキュリティで保護されたネットワーク サービスに接続する方法のほか、物理スマート カード リーダーにアクセスする方法、仮想スマート カードの作成方法、スマート カードとの通信方法、ユーザーの認証方法、ユーザーの PIN のリセット方法、スマート カードの取り外しや切断の方法などについて説明します。
ms.assetid: 86524267-50A0-4567-AE17-35C4B6D24745
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, セキュリティ
ms.localizationpriority: medium
ms.openlocfilehash: 5498480e0dbe2c8be96d92df766b15676a3e6b7b
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/29/2019
ms.locfileid: "66371934"
---
# <a name="smart-cards"></a>スマート カード




このトピックでは、ユニバーサル Windows プラットフォーム (UWP) アプリでスマート カードを使ってユーザーをセキュリティで保護されたネットワーク サービスに接続する方法のほか、物理スマート カード リーダーにアクセスする方法、仮想スマート カードの作成方法、スマート カードとの通信方法、ユーザーの認証方法、ユーザーの PIN のリセット方法、スマート カードの取り外しや切断の方法などについて説明します。 

## <a name="configure-the-app-manifest"></a>アプリ マニフェストの構成


アプリでスマート カードや仮想スマート カードを使ってユーザーを認証するには、あらかじめプロジェクトの Package.appxmanifest ファイルで、**共有ユーザー証明書**機能を設定しておく必要があります。

## <a name="access-connected-card-readers-and-smart-cards"></a>接続されているカード リーダーとスマート カードへのアクセス


[  **DeviceInformation**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformation) に指定されているデバイス ID を [**SmartCardReader.FromIdAsync**](https://docs.microsoft.com/uwp/api/windows.devices.smartcards.smartcardreader.fromidasync) メソッドに渡すと、リーダーや装着されているスマート カードを照会することができます。 返されたリーダー デバイスに現在装着されているスマート カードにアクセスするには、[**SmartCardReader.FindAllCardsAsync**](https://docs.microsoft.com/uwp/api/windows.devices.smartcards.smartcardreader.findallcardsasync) を呼び出します。

```cs
string selector = SmartCardReader.GetDeviceSelector();
DeviceInformationCollection devices =
    await DeviceInformation.FindAllAsync(selector);

foreach (DeviceInformation device in devices)
{
    SmartCardReader reader =
        await SmartCardReader.FromIdAsync(device.Id);

    // For each reader, we want to find all the cards associated
    // with it.  Then we will create a SmartCardListItem for
    // each (reader, card) pair.
    IReadOnlyList<SmartCard> cards =
        await reader.FindAllCardsAsync();
}
```

また、カードが挿入されたときのアプリの動作を処理するメソッドを実装して、アプリによる [**CardAdded**](https://docs.microsoft.com/uwp/api/windows.devices.smartcards.smartcardreader.cardadded) イベントの監視を有効にする必要もあります。

```cs
private void reader_CardAdded(SmartCardReader sender, CardAddedEventArgs args)
{
  // A card has been inserted into the sender SmartCardReader.
}
```

その後、返された各 [**SmartCard**](https://docs.microsoft.com/uwp/api/Windows.Devices.SmartCards.SmartCard) オブジェクトを [**SmartCardProvisioning**](https://docs.microsoft.com/uwp/api/Windows.Devices.SmartCards.SmartCardProvisioning) に渡すことで、その構成へのアクセスやカスタマイズを行うメソッドを使えるようになります。

## <a name="create-a-virtual-smart-card"></a>仮想スマート カードの作成


アプリで [**SmartCardProvisioning**](https://docs.microsoft.com/uwp/api/Windows.Devices.SmartCards.SmartCardProvisioning) を使って仮想スマート カードを作成するには、まずフレンドリ名、管理者キー、[**SmartCardPinPolicy**](https://docs.microsoft.com/uwp/api/Windows.Devices.SmartCards.SmartCardPinPolicy) を提供する必要があります。 通常、フレンドリ名は既にアプリに用意されている可能性がありますが、アプリではさらに、管理者キーを提供し、現在の **SmartCardPinPolicy** のインスタンスを生成する必要があります。その後、これらの 3 つの値をすべて [**RequestVirtualSmartCardCreationAsync**](https://docs.microsoft.com/uwp/api/windows.devices.smartcards.smartcardprovisioning.requestvirtualsmartcardcreationasync) に渡します。

1.  [  **SmartCardPinPolicy**](https://docs.microsoft.com/uwp/api/Windows.Devices.SmartCards.SmartCardPinPolicy) の新しいインスタンスを作成します。
2.  サービスまたは管理ツールから提供された管理者キーの値に対して [**CryptographicBuffer.GenerateRandom**](https://docs.microsoft.com/uwp/api/windows.security.cryptography.cryptographicbuffer.generaterandom) を呼び出して、管理者キーの値を生成します。
3.  これらの値と *FriendlyNameText* 文字列を [**RequestVirtualSmartCardCreationAsync**](https://docs.microsoft.com/uwp/api/windows.devices.smartcards.smartcardprovisioning.requestvirtualsmartcardcreationasync) に渡します。

```cs
SmartCardPinPolicy pinPolicy = new SmartCardPinPolicy();
pinPolicy.MinLength = 6;

IBuffer adminkey = CryptographicBuffer.GenerateRandom(24);

SmartCardProvisioning provisioning = await
     SmartCardProvisioning.RequestVirtualSmartCardCreationAsync(
          "Card friendly name",
          adminkey,
          pinPolicy);
```

関連付けられた [**SmartCardProvisioning**](https://docs.microsoft.com/uwp/api/Windows.Devices.SmartCards.SmartCardProvisioning) オブジェクトが [**RequestVirtualSmartCardCreationAsync**](https://docs.microsoft.com/uwp/api/windows.devices.smartcards.smartcardprovisioning.requestvirtualsmartcardcreationasync) から返されたら、仮想スマート カードのプロビジョニングが完了し、使う準備ができたことになります。

## <a name="handle-authentication-challenges"></a>認証チャレンジの処理


スマート カードや仮想スマート カードを使って認証するには、カードに格納されている管理者キーのデータと、認証サーバーまたは管理ツールによって管理されている管理者キーのデータとの間で、チャレンジを完了する動作をアプリに実装する必要があります。

次のコードは、サービスのスマート カード認証をサポートする方法、または物理カードや仮想カードの詳細を変更する方法の例を示します。 カードの管理者キーを使って生成されたデータ ("challenge") が、サーバーまたは管理者ツールから提供された管理者キーのデータ ("adminkey") と一致すれば、認証は成功します。

```cs
static class ChallengeResponseAlgorithm
{
    public static IBuffer CalculateResponse(IBuffer challenge, IBuffer adminkey)
    {
        if (challenge == null)
            throw new ArgumentNullException("challenge");
        if (adminkey == null)
            throw new ArgumentNullException("adminkey");

        SymmetricKeyAlgorithmProvider objAlg = SymmetricKeyAlgorithmProvider.OpenAlgorithm(SymmetricAlgorithmNames.TripleDesCbc);
        var symmetricKey = objAlg.CreateSymmetricKey(adminkey);
        var buffEncrypted = CryptographicEngine.Encrypt(symmetricKey, challenge, null);
        return buffEncrypted;
    }
}
```

このコードは、このトピックの残りの部分で説明する、認証操作を完了する方法や、スマート カードまたは仮想スマート カードの情報に変更を適用する方法の中で使われます。

## <a name="verify-smart-card-or-virtual-smart-card-authentication-response"></a>スマート カードまたは仮想スマート カード認証の応答の確認


これで認証チャレンジのロジックが定義されたので、リーダーと通信してスマート カードにアクセスするか、その代わりに仮想スマート カードにアクセスして、認証を行うことができます。

1.  チャレンジを始めるには、スマート カードに関連付けられた [**SmartCardProvisioning**](https://docs.microsoft.com/uwp/api/Windows.Devices.SmartCards.SmartCardProvisioning) オブジェクトから [**GetChallengeContextAsync**](https://docs.microsoft.com/uwp/api/windows.devices.smartcards.smartcardprovisioning.getchallengecontextasync) を呼び出します。 これにより、[**SmartCardChallengeContext**](https://docs.microsoft.com/uwp/api/Windows.Devices.SmartCards.SmartCardChallengeContext) のインスタンスが生成されます。このインスタンスには、カードの [**Challenge**](https://docs.microsoft.com/uwp/api/windows.devices.smartcards.smartcardchallengecontext.challenge) 値が含まれています。

2.  次に、カードのチャレンジ値とサービスまたは管理ツールから提供された管理者キーを、前の例で定義した **ChallengeResponseAlgorithm** に渡します。

3.  [**VerifyResponseAsync** ](https://docs.microsoft.com/uwp/api/windows.devices.smartcards.smartcardchallengecontext.verifyresponseasync)戻ります**true**認証が成功した場合。

```cs
bool verifyResult = false;
SmartCard card = await rootPage.GetSmartCard();
SmartCardProvisioning provisioning =
    await SmartCardProvisioning.FromSmartCardAsync(card);

using (SmartCardChallengeContext context =
       await provisioning.GetChallengeContextAsync())
{
    IBuffer response = ChallengeResponseAlgorithm.CalculateResponse(
        context.Challenge,
        rootPage.AdminKey);

    verifyResult = await context.VerifyResponseAsync(response);
}
```

## <a name="change-or-reset-a-user-pin"></a>ユーザー PIN の変更またはリセット


スマート カードに関連付けられている PIN を変更するには、次の手順に従います。

1.  カードにアクセスし、関連付けられた [**SmartCardProvisioning**](https://docs.microsoft.com/uwp/api/Windows.Devices.SmartCards.SmartCardProvisioning) オブジェクトを生成します。
2.  [  **RequestPinChangeAsync**](https://docs.microsoft.com/uwp/api/windows.devices.smartcards.smartcardprovisioning.requestpinchangeasync) を呼び出して、この操作を完了するための UI をユーザーに表示します。
3.  PIN が正しく変更された場合は、呼び出しから **true** が返されます。

```cs
SmartCardProvisioning provisioning =
    await SmartCardProvisioning.FromSmartCardAsync(card);

bool result = await provisioning.RequestPinChangeAsync();
```

PIN のリセットを要求するには、次の手順に従います。

1.  [  **RequestPinResetAsync**](https://docs.microsoft.com/uwp/api/windows.devices.smartcards.smartcardprovisioning.requestpinresetasync) を呼び出して操作を開始します。 この呼び出しには、スマート カードと PIN のリセット要求を表す [**SmartCardPinResetHandler**](https://docs.microsoft.com/uwp/api/windows.devices.smartcards.smartcardpinresethandler) メソッドが含まれます。
2.  [**SmartCardPinResetHandler** ](https://docs.microsoft.com/uwp/api/windows.devices.smartcards.smartcardpinresethandler)情報を提供する、 **ChallengeResponseAlgorithm**でラップされた、 [ **SmartCardPinResetDeferral** ](https://docs.microsoft.com/uwp/api/Windows.Devices.SmartCards.SmartCardPinResetDeferral)を呼び出すと、カードのチャレンジの値と要求の認証にサービスや管理のツールによって提供される管理キーを比較するために使用します。

3.  チャレンジが成功すると、[**RequestPinResetAsync**](https://docs.microsoft.com/uwp/api/windows.devices.smartcards.smartcardprovisioning.requestpinresetasync) の呼び出しが完了し、PIN が正しくリセットされた場合は **true** が返されます。

```cs
SmartCardProvisioning provisioning =
    await SmartCardProvisioning.FromSmartCardAsync(card);

bool result = await provisioning.RequestPinResetAsync(
    (pinResetSender, request) =>
    {
        SmartCardPinResetDeferral deferral =
            request.GetDeferral();

        try
        {
            IBuffer response =
                ChallengeResponseAlgorithm.CalculateResponse(
                    request.Challenge,
                    rootPage.AdminKey);
            request.SetResponse(response);
        }
        finally
        {
            deferral.Complete();
        }
    });
}
```

## <a name="remove-a-smart-card-or-virtual-smart-card"></a>スマート カードまたは仮想スマート カードの取り外し


物理スマート カードが取り外されると、カードの削除時に [**CardRemoved**](https://docs.microsoft.com/uwp/api/windows.devices.smartcards.smartcardreader.cardremoved) イベントが発生します。

カード リーダーでこのイベントが発生したときのイベント ハンドラーとして、カードまたはリーダーが取り外されたときのアプリの動作を定義するメソッドを関連付けます。 この動作は、カードが取り外されたことをユーザーに通知するといった単純なものでかまいません。

```cs
reader = card.Reader;
reader.CardRemoved += HandleCardRemoved;
```

仮想スマート カードの削除はプログラムによって行います。まずカードを取得し、次に [**SmartCardProvisioning**](https://docs.microsoft.com/uwp/api/Windows.Devices.SmartCards.SmartCardProvisioning) によって返されたオブジェクトから [**RequestVirtualSmartCardDeletionAsync**](https://docs.microsoft.com/uwp/api/windows.devices.smartcards.smartcardprovisioning.requestvirtualsmartcarddeletionasync) を呼び出します。

```cs
bool result = await SmartCardProvisioning
    .RequestVirtualSmartCardDeletionAsync(card);
```