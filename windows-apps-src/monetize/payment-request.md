---
description: 支払い要求 API は、ユーザーが支払い情報を入力して出荷方法を選択する必要があるプロセスをバイパスする UWP アプリ用の統合ソリューションを提供します。
title: 支払い要求 API で支払いを簡略化する
ms.date: 09/26/2017
ms.topic: article
keywords: windows 10、uwp、支払い要求
ms.openlocfilehash: 3e54767496d9c8d25ce42ea389f43ca2075d128d
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/06/2020
ms.locfileid: "75685045"
---
# <a name="simplify-payments-with-the-payment-request-api"></a>支払い要求 API で支払いを簡略化する
UWP アプリの支払い要求 API は、 [W3C 支払い要求 api 仕様](https://w3c.github.io/browser-payment-api/)に基づいています。これにより、UWP アプリでのチェックアウトプロセスを効率化することができます。 ユーザーは、Microsoft アカウントと共に既に保存されている支払いオプションと送付先住所を使用して、チェックアウトの速度を上げることができます。 支払情報がトークン化されるため、変換率を上げ、データ侵害のリスクを軽減することができます。 Windows 10 の作成者の更新プログラム以降、ユーザーは保存された支払いオプションを使用して、UWP アプリのエクスペリエンス間で簡単に支払いを行うことができます。

## <a name="prerequisites"></a>必要条件
支払い要求 API の使用を開始する前に、いくつかの作業を行う必要があります。

### <a name="getting-a-merchant-id"></a>マーチャント ID を取得する
支払い要求プロセスの一環として、マイクロソフトはお客様のサービスプロバイダーの代わりに支払いトークンを要求します。 そのため、API の使用を開始する前に、これらのトークンを要求するための承認が必要です。  販売者アカウントに登録し、必要な承認を提供するには、いくつかの手順に従う必要があります。 これを行うには、 [Microsoft 販売者センター](https://partner.microsoft.com/dashboard/registration/seller?accountprogram=uwp)にアクセスします。 これが完了したら、支払い要求を作成するときに、パートナーセンターから生成されたマーチャント ID をアプリにコピーします。 その後、アプリケーションが支払い要求を送信すると、支払いを送信する必要がある支払いトークンをプロセッサから受け取ります。

### <a name="geographic-restrictions-and-language-support"></a>地理的な制限と言語サポート
支払い要求 API は、米国の企業のみが米国のトランザクションを処理するために使用できます。

## <a name="using-the-payment-request-api-in-your-app-step-by-step"></a>アプリでの支払い要求 API の使用: ステップバイステップ
このセクションでは、アプリで[UWP 支払い要求 API](https://docs.microsoft.com/uwp/api/windows.applicationmodel.payments)を使用する方法について説明します。 ここでは、わかりやすくするために、この API を最も単純な形式で使用します。 これらの Api のより高度な使用例については、 [GitHub の UWP ショッピングアプリのサンプル](https://github.com/Microsoft/Windows-appsample-shopping)を参照してください。

### <a name="1-create-a-set-of-all-the-payment-options-that-you-accept"></a>1. 同意するすべての支払いオプションのセットを作成します。
> [!Note]
> 販売者**id**のテキストを、販売者センターから取得したマーチャント id に置き換えます。

[!code-csharp[SnippetEnumerate](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetEnumerate)]

### <a name="2-pull-the-payment-details-together"></a>2. 支払いの詳細をまとめて取得します。 

これらの詳細は、支払いアプリのユーザーに表示されます。 

[!code-csharp[SnippetDisplayItems](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetDisplayItems)]

### <a name="3-include-the-sales-tax"></a>3. 売上税を含めます。 

> [!Important]
> API は、商品を追加したり、売上税を計算したりすることはありません。 税務率は管轄区によって異なることに注意してください。 わかりやすくするために、仮定の9.5% の課税率を使用します。

[!code-csharp[SnippetTaxes](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetTaxes)]

### <a name="4-optional--add-discounts-or-other-modifiers-to-the-total"></a>4. (省略可能) 割引またはその他の修飾子を合計に追加します。 

特定の Contoso クレジットカードを使用して表示項目に割引を追加する例を次に示します。 (*Contoso*は架空の名前です)。

[!code-csharp[SnippetDiscountRate](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetDiscountRate)]

### <a name="5-assemble-all-the-payment-details"></a>5. すべての支払いの詳細をまとめます。

[!code-csharp[SnippetAggregate](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetAggregate)]
[!code-csharp[SnippetPaymentOptions](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetPaymentOptions)]

### <a name="6-submit-the-payment-request"></a>6. 支払い要求を送信します。 

支払い要求を送信するには、 **SubmitPaymentRequestAsync**メソッドを呼び出します。 これにより、使用可能な支払いオプションを示す支払いアプリが表示されます。

[!code-csharp[SnippetSubmit](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetSubmit)]

ユーザーは、Microsoft アカウントでサインインするように求められます。

ユーザーがサインインすると、以前に保存されていた支払いオプションと送付先住所を選択できます。

![支払い要求の UI](./images/33.png "支払い要求の UI")

アプリは、ユーザーが **[支払い]** をタップするまで待機してから、注文を完了します。

[!code-csharp[SnippetComplete](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetComplete)]

支払いが完了すると、ユーザーに**注文確認**画面が表示されます。

![注文の確認](./images/44.png "注文の確認")

## <a name="see-also"></a>「
- [Windows. ApplicationModel. 支払いのリファレンスドキュメント](https://docs.microsoft.com/uwp/api/windows.applicationmodel.payments)
- [GitHub の UWP ショッピングアプリのサンプル](https://github.com/Microsoft/Windows-appsample-shopping)
- [W3C 支払い要求 API 仕様](https://www.w3.org/TR/payment-request/)
- [支払い要求 API](https://docs.microsoft.com/microsoft-edge/dev-guide/windows-integration/payment-request-api)

