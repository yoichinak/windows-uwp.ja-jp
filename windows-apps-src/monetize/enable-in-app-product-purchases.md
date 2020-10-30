---
description: アプリが無料であるかどうかにかかわらず、コンテンツ、その他のアプリ、アプリの新機能 (ゲームの次のレベルのロック解除など) をアプリ内から直接販売できます。 ここでは、アプリ内で製品を販売できるようにする方法について説明します。
title: アプリ内製品購入の有効化
ms.assetid: D158E9EB-1907-4173-9889-66507957BD6B
keywords: UWP, アドオン, アプリ内購入, IAP, Windows.ApplicationModel.Store
ms.date: 08/25/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: b24a48034585411af5edfb0950fc4f96b189519f
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2020
ms.locfileid: "93033475"
---
# <a name="enable-in-app-product-purchases"></a>アプリ内製品購入の有効化

アプリが無料であるかどうかにかかわらず、コンテンツ、その他のアプリ、アプリの新機能 (ゲームの次のレベルのロック解除など) をアプリ内から直接販売できます。 ここでは、アプリ内で製品を販売できるようにする方法について説明します。

> [!IMPORTANT]
> この記事では、[Windows.ApplicationModel.Store](/uwp/api/windows.applicationmodel.store) 名前空間のメンバーを使って、アプリ内製品の購入を有効化する方法について説明します。 この名前空間は更新されなくなり、新機能も追加されないため、代わりに [Windows.Services.Store](/uwp/api/windows.services.store) 名前空間を使用することをお勧めします。 **Windows** では、ストアで管理されているアドオンやサブスクリプションなど、最新のアドオンの種類がサポートされており、パートナーセンターとストアでサポートされる将来の種類の製品や機能と互換性があるように設計されています。 **Windows.Services.Store** 名前空間は、Windows 10 バージョン 1607 で導入され、Visual Studio で、 **Windows 10 Anniversary Edition (10.0、ビルド 14393)** 以降のリリースをターゲットとするプロジェクトでのみ使用できます。 **Windows.Services.Store** 名前空間を使用したアプリ内製品購入の有効化について詳しくは、 [この記事](enable-in-app-purchases-of-apps-and-add-ons.md)をご覧ください。

> [!NOTE]
> アプリ内製品は、アプリの試用版では提供できません。 アプリの試用版を使用中のユーザーがアプリ内製品を購入できるのは、通常版のアプリを購入する場合のみです。

## <a name="prerequisites"></a>前提条件

-   ユーザーが購入できる機能を追加する Windows アプリ。
-   新しいアプリ内製品のコード記述やテストを初めて行うときは、[CurrentApp](/uwp/api/Windows.ApplicationModel.Store.CurrentApp) オブジェクトではなく、[CurrentAppSimulator](/uwp/api/Windows.ApplicationModel.Store.CurrentAppSimulator) オブジェクトを使う必要があります。 そうすることで、実稼働サーバーを呼び出すのではなく、ライセンス サーバーへのシミュレートされた呼び出しを使って、ライセンス ロジックを検証できます。 これを行うには、% userprofile% \\ AppData \\ ローカル \\ パッケージの \\ &lt; パッケージ名 &gt; \\ localstate \\ Microsoft \\ Windows Store \\ apidata で WindowsStoreProxy.xml という名前のファイルをカスタマイズする必要があります。 このファイルは、アプリを初めて実行するときに Microsoft Visual Studio シミュレーターによって作られます。カスタマイズされたファイルを実行時に読み込むこともできます。 詳しくは、「[CurrentAppSimulator での WindowsStoreProxy.xml ファイルの使用](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md#proxy)」をご覧ください。
-   このトピックでは、[ストア サンプル](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store)で提供されているコード例も参照します。 このサンプルを利用すると、ユニバーサル Windows プラットフォーム (UWP) アプリに提供されるさまざまな収益化オプションを体験できます。

## <a name="step-1-initialize-the-license-info-for-your-app"></a>手順 1: アプリのライセンス情報を初期化する

アプリを初期化するときに、[CurrentApp](/uwp/api/Windows.ApplicationModel.Store.CurrentApp) または [CurrentAppSimulator](/uwp/api/Windows.ApplicationModel.Store.CurrentAppSimulator) を初期化することでアプリの [LicenseInformation](/uwp/api/Windows.ApplicationModel.Store.LicenseInformation) オブジェクトを取得し、アプリ内製品の購入を有効にします。

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/InAppPurchasesAndLicenses/cs/EnableInAppPurchases.cs" id="InitializeLicenseTest":::

## <a name="step-2-add-the-in-app-offers-to-your-app"></a>手順 2: アプリにアプリ内製品の販売を追加する

アプリ内製品によって提供する機能ごとに、販売を作り、アプリに追加します。

> [!IMPORTANT]
> ストアにアプリを提出する前に、ユーザーに提供するすべてのアプリ内製品をアプリに追加する必要があります。 新しいアプリ内製品を後から追加する場合は、アプリを更新し、新しいバージョンを再提出する必要があります。

1.  **アプリ内販売トークンを作成する**

    アプリの各アプリ内製品は、トークンで識別します。 このトークンは開発者が定義する文字列であり、アプリ内とストア内で、特定のアプリ内製品を識別するために使われます。 アプリに固有のわかりやすい名前を付けて、その機能をコードの記述中に簡単に識別できるようにしてください。 たとえば、次のような名前を付けます。

    * "SpaceMissionLevel4"
    * "ContosoCloudSave"
    * "RainbowThemePack"

  > [!NOTE]
  > コードで使用するアプリ内プラントークンは、[パートナーセンターでアプリの対応するアドオンを定義](../publish/add-on-submissions.md)するときに指定した[製品 ID](../publish/set-your-add-on-product-id.md#product-id)値と一致する必要があります。

2.  **条件ブロック内に機能のコードを記述する**

    アプリ内製品の対象となる各機能のコードは、その機能を使うためのライセンスをユーザーが持っているかどうかをテストする条件ブロック内に記述する必要があります。

    次の例は、ライセンス固有の条件ブロック内に **featureName** という名前の製品機能のコードを記述する方法を示しています。 **featureName** という文字列は、アプリ内でこの製品を一意に識別するトークンであり、ストアでも識別用に使われます。

    > [!div class="tabbedCodeSnippets"]
    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/InAppPurchasesAndLicenses/cs/EnableInAppPurchases.cs" id="CodeFeature":::

3.  **この機能の購入 UI を追加する**

    アプリには、アプリ内製品で提供される製品または機能をユーザーが購入するための方法も用意する必要があります。 ユーザーは、完全なアプリを購入したときのように、ストアを通じてそれらの製品または機能を購入することはできません。

    次の例は、ユーザーが既にアプリ内製品を所有しているかどうかをテストし、所有していない場合は購入できるように購入用ダイアログを表示する方法を示しています。 "show the purchase dialog" というコメントを、購入用ダイアログの独自のコードに置き換えてください (わかりやすい [このアプリを購入]  ボタンのあるページなど)。

    > [!div class="tabbedCodeSnippets"]
    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/InAppPurchasesAndLicenses/cs/EnableInAppPurchases.cs" id="BuyFeature":::

## <a name="step-3-change-the-test-code-to-the-final-calls"></a>手順 3: テスト コードを最終的な呼び出しに変更する

この手順は簡単です。アプリのコード内の [CurrentAppSimulator](/uwp/api/Windows.ApplicationModel.Store.CurrentAppSimulator) への参照をすべて [CurrentApp](/uwp/api/Windows.ApplicationModel.Store.CurrentApp) への参照に変えます。 WindowsStoreProxy.xml ファイルは不要になるので、アプリのパスから削除します (ただし、次の手順でアプリ内販売を構成するときの参照用に保存しておくことをお勧めします)。

## <a name="step-4-configure-the-in-app-product-offer-in-the-store"></a>手順 4: ストアでアプリ内製品を構成する

パートナーセンターで、アプリに移動し、アプリ内製品プランと一致する [アドオンを作成](../publish/add-on-submissions.md) します。 アドオンの製品 ID、種類、価格などのプロパティを定義します。 テストのときに WindowsStoreProxy.xml で設定した構成と同じ構成になっていることを確認してください。

  > [!NOTE]
  > コードで使用するアプリ内プラントークンは、パートナーセンターの対応するアドオンに対して指定した [製品 ID](../publish/set-your-add-on-product-id.md#product-id) 値と一致する必要があります。

## <a name="remarks"></a>注釈

コンシューマブルなアプリ内購入オプション (購入して使い切った後、必要に応じて再購入できる項目) を顧客に提供することに関心がある場合は、「[コンシューマブルなアプリ内製品購入の有効化](enable-consumable-in-app-product-purchases.md)」をご覧ください。

ユーザーがアプリ内購入を行ったことを確認するために通知を使う必要がある場合は、「[受領通知を使った製品購入の確認](use-receipts-to-verify-product-purchases.md)」を確認してください。

## <a name="related-topics"></a>関連トピック


* [使用可能なアプリ内製品の購入を有効にする](enable-consumable-in-app-product-purchases.md)
* [アプリ内製品の大規模なカタログの管理](manage-a-large-catalog-of-in-app-products.md)
* [受領通知を使った製品購入の確認](use-receipts-to-verify-product-purchases.md)
* [ストア サンプル (試用版とアプリ内購入のデモンストレーション)](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store)
