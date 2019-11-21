---
Description: If your app displays ads using the Microsoft Advertising SDK, use the In-app ads page of Partner Center to manage your use of ads.
title: アプリ内広告
ms.assetid: 09970DE3-461A-4E2A-88E3-68F2399BBCC8
ms.date: 03/25/2019
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: e12641695dd72cddcfb6b51f6cd2f20fa66ddf41
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259001"
---
# <a name="in-app-ads"></a>アプリ内広告

Use the **Monetize** &gt; **In-app ads** page in [Partner Center](https://partner.microsoft.com/dashboard) to create and manage ad units for:

* [Microsoft Advertising SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK) を使用するユニバーサル Windows プラットフォーム (UWP) アプリ。
* Previously published Windows 8.x and Windows Phone 8.x apps that use the [Microsoft Advertising SDK for Windows and Windows Phone 8.x](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDKforWindowsandWindowsPhone8x).

> [!IMPORTANT]
> As of October 31, 2018, newly-created products cannot include packages targeting Windows 8.x/Windows Phone 8.x or earlier. For more info, see this [blog post](https://blogs.windows.com/windowsdeveloper/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store).

これらの SDK をアプリに統合して広告を表示する方法については、「[Microsoft Advertising SDK を使用したアプリでの広告の表示](../monetize/display-ads-in-your-app.md)」をご覧ください。

<span id="create-ad-unit" />

## <a name="create-ad-units"></a>広告ユニットを作成

アプリ内の[バナー広告](../monetize/banner-ads.md), [interstitial ad](../monetize/interstitial-ads.md)または[ネイティブ広告](../monetize/native-ads.md)用に広告ユニットを作成するには:

1.  Go to the **Monetize** &gt; **In-app ads** page in Partner Center and click **Create ad unit**.
2.  **[アプリ名]** ドロップダウンで、広告ユニットを使用するアプリを選択します。
3.  **[広告ユニット名]** フィールドに広告ユニットの名前を入力します。 レポートで広告ユニットを識別しやすくするために、任意の説明文字列を指定できます。
4.  **[広告ユニットの種類]** ドロップダウンで、広告の種類を選択します。

    * If you are showing a banner ad in your app, select **Banner**.
    * If you are showing an interstitial video ad or interstitial banner ad in your app, select **Video interstitial** or **Banner interstitial** (be sure to select the appropriate option for the type of interstitial ad you want to show).
    * If you are showing a native ad in your app, select **Native**.

5. **[デバイス ファミリ]** ドロップダウンで、広告ユニットを使うアプリがターゲットとしているデバイス ファミリを選択します。 選択できるオプションには、 **[UWP (Windows 10)]** 、 **[PC/タブレット (Windows 8.1)]** 、 **[モバイル (Windows Phone 8.x)]** があります。

6. 必要に応じて、次の追加設定を構成します。

    * 広告ユニットのターゲットに **[UWP (Windows 10)]** デバイス ファミリを選択した場合は、オプションで広告ユニットの[仲介設定](#mediation)を構成できます。
    * バナー広告ユニットのターゲットとして **[PC/タブレット (Windows 8.1)]** デバイス ファミリまたは **[モバイル (Windows Phone 8.x)]** デバイス ファミリを選択した場合は、オプションで **[アプリにコミュニティ広告を表示する]** を選択して[コミュニティ広告](about-community-ads.md)にオプトインすることができます。

7.  選択したアプリに対して COPPA 準拠を設定していない場合は、[[COPPA 準拠](#coppa)] セクションでオプションを選択します。
8.  **[広告ユニットを作成]** をクリックします。

After you create the new ad unit, it appears in the table of available ad units in the **Monetize** &gt; **In-app ads** page.

<span id="available-ad-units" />

## <a name="review-and-edit-ad-units"></a>広告ユニットの確認と編集

After you create ad units for one or more apps in your account, these ad units appear in a table at the bottom of the **Monetize** &gt; **In-app ads** page. この表には、各広告ユニットの **[アプリケーション ID]** および **[広告ユニット ID]** がその他の情報と共に表示されます。 アプリに広告を表示するには、コードでこれらの値を使う必要があります。 詳しくは、「[アプリの広告ユニットをセットアップする](../monetize/set-up-ad-units-in-your-app.md)」をご覧ください。

* アプリに[バナー広告](../monetize/banner-ads.md)を表示する場合は、これらの値を [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol) オブジェクトの [ApplicationId](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.applicationid) プロパティと [AdUnitId](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.adunitid) プロパティに割り当てる必要があります。
* アプリに[スポット広告](../monetize/interstitial-ads.md)を表示する場合は、[InterstitialAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad) オブジェクトの [RequestAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) メソッドにこれらの値を渡します。
* If your app shows [native ads](../monetize/native-ads.md), pass these values to the **NativeAdsManagerV2** constructor.
  > [!IMPORTANT]
  > 各広告ユニットは 1 つのアプリのみで使用できます。 同じ広告ユニットを複数のアプリで使うと、その広告ユニットには広告が配信されません。

  > [!NOTE]
  > 1 つのアプリに複数のバナー広告コントロール、スポット広告コントロール、ネイティブ広告コントロールを使用できます。 このシナリオでは、各コントロールに異なる広告ユニットを割り当てることをお勧めします。 各コントロールに異なる広告ユニットを使用することで、別々に[仲介の設定を構成](../publish/in-app-ads.md#mediation)して、個別の[報告データ](../publish/advertising-performance-report.md)を取得することが可能です。 また、これにより、Microsoft のサービスはアプリに提供する広告を最適化できます。

UWP 広告ユニットの[仲介設定](#mediation)または広告ユニットを使用しているアプリの [COPPA 準拠](#coppa)を編集するには、ユニット名をクリックします。

> [!NOTE]
> If an ad unit has no activity for the past six months, we will label it as **Inactive**, and eventually remove it from Partner Center. フィルターを使用して "**アクティブ**" または "**非アクティブ**" の広告ユニットのみを表示することもできます。 誤って "**非アクティブ**" がマークされていると思われる広告ユニットを見つけた場合は、[サポートにお問い合わせください](https://developer.microsoft.com/windows/support)。

<span id="mediation" />

## <a name="mediation-settings"></a>仲介設定

When you [create a new UWP ad unit](#create-ad-unit) or [edit an existing UWP ad unit](#available-ad-units), use the options in this section to configure [ad mediation](../monetize/ad-mediation-service.md) for the ad unit. 広告仲介を使うと、複数の広告ネットワークから広告を表示して、広告収益とアプリ プロモーションの機能を最大限に引き出すことができます。表示される広告には、他の有料広告ネットワークからの広告や、Microsoft のアプリ プロモーション キャンペーン用の収益が生じない広告などが含まれます。 選択した広告ネットワークからのバナー広告要求の仲介は自動的に行われます。 アプリ内のバナー広告、スポット広告、またはネイティブ広告に既に関連付けられている UWP 広告ユニットがある場合は、広告仲介を有効にするためにアプリのコードを変更する必要はありません。

> [!NOTE]
> UWP 広告ユニットで広告仲介を有効にする場合、サードパーティの広告ネットワークから広告ユニットを取得する必要はありません。 必要なサードパーティの広告ユニットは、広告仲介サービスによって自動的に作成されます。

アプリ内の UWP 広告ユニットに対して広告仲介設定を構成するには、次の手順を実行します。

1. [広告ユニットを作成](#create-ad-unit)するか、[既にある広告ユニットを選択](#available-ad-units)します。
2. On the **In-app ads** page, go to the **Mediation settings** section and configuration your settings.

    * By default, the **Let Microsoft optimize my settings** check box is selected. このオプションを使うことをお勧めします。 このオプションでは、アプリがサポートする各市場での広告収益を最大化できるように、機械学習アルゴリズムを使ってアプリの広告仲介設定を自動的に選択します。 When you use this option, you can also choose the ad networks you want to use in the configuration. Uncheck the ad networks that you don't want to be part of the configuration and our algorithm will ensure that your app only receives ads from the selected ad networks.
    * If you want to choose your own ad mediation settings, choose **Modify default settings**.

    > [!NOTE]
    > The remaining steps in this section are only applicable if you choose **Modify default settings**.

3. **[ターゲット]** ドロップダウンで **[ベースライン]** を選択して、既定の広告仲介設定を構成します。 この既定の構成は、市場固有の構成が定義されていないすべての市場に適用されます。
4. 次に、有料ネットワーク (広告表示に応じて収益が支払われるネットワーク) とその他の広告ネットワーク (広告表示に対する収益の支払いがないネットワーク) について、コントロールに表示するそれぞれの広告の割合を指定します。 これを行うには、 **[有料の広告ネットワーク]** と **[他の広告ネットワーク]** の **[重さ]** フィールドに 0 ～ 100 の値を入力します。  
5. **[有料の広告ネットワーク]** セクションで、使用する[有料ネットワーク](#paid-networks)ごとに **[有効]** 列のチェック ボックスをオンにします。次に、 **[ランク]** 列の矢印を使ってネットワークをランク順に並べ替えます (これで、コントロールによる各ネットワークの使用頻度が指定されます)。
6. **[バナー]** または **[バナー (スポット)]** 広告ユニットを選ぶと、 **[他の広告ネットワーク]** という名前のセクションも表示されます。 このセクションのネットワークでは、広告インプレッションに対する収益は生じません。 代わりに、これらのネットワークはアプリ プロモーション キャンペーンなどのソースからの広告を表示します。

    **[その他の広告ネットワーク]** セクションで、使用する[その他のネットワーク](#other-networks)ごとに **[有効]** 列のチェック ボックスをオンにします。次に、 **[ランク]** 列の矢印を使ってネットワークをランク順に並べ替えます (これで、コントロールによる各ネットワークの使用頻度が指定されます)。 現在サポートされているその他のネットワークは次のとおりです。

7. 既定の仲介構成をオーバーライドする市場ごとに、 **[ターゲット]** ドロップダウンで市場を選択し、広告ネットワークの選択とランクを更新します。
8. **[広告ユニットを作成]** (新しい広告ユニットを作成している場合) または **[保存]** (既存の広告ユニットを編集している場合) をクリックします。

<span id="paid-networks" />

### <a name="supported-paid-ad-networks"></a>サポートされている有料広告ネットワーク

次の表は、広告の種類によって現在サポートされている有料ネットワークを示しています。 ただし、[一部の市場では利用できない](#network-markets)ネットワークもあります。

|  広告ネットワーク  |  説明  |  サポートされている広告の種類  |
|--------------|---------------|---------------------|
| Oath and AppNexus |  This is a Microsoft-managed ad network that serves ads through our partner networks, Oath and AppNexus.<p/>**Note**: Oath and AppNexus is always ranked first in the **Paid ad networks** list for banner ad units, and it cannot be changed to a lower ranking for these types of ads. | バナー、ビデオ スポット広告 |
| AppNexus (直接) | Select this option to serve ads from [AppNexus](https://www.appnexus.com). | ビデオ (スポット)、ネイティブ  |
| Microsoft アプリ インストール広告 | Windows エコシステム内の他の開発者で、[各自が開発したアプリのプロモーション用広告キャンペーンを作成している](create-an-ad-campaign-for-your-app.md)開発者によって作成されたアプリ インストール広告やアプリ リエンゲージメント広告を提供するには、このオプションを選択します。  |  バナー、バナー (スポット)、ネイティブ  |
| MSN Content Recommendations |  Select this option to serve ads from MSN Content Recommendations. |  バナー、バナー (スポット)  |
| Outbrain |  [Outbrain](https://www.outbrain.com/) から広告を提供するには、このオプションを選択します。 |  バナー、バナー (スポット)  |
| Revcontent |  [Revcontent](https://www.revcontent.com/) から広告を提供するには、このオプションを選択します。 |  バナー、ネイティブ  |
| Smaato |  [Smaato](https://www.smaato.com/) から広告を提供するには、このオプションを選択します。 |  Banner  |
| smartclip |  [smartclip](http://www.smartclip.com/) から広告を提供するには、このオプションを選択します。 |  ビデオ (スポット)  |
| SpotX |  [SpotX](https://www.spotx.tv/) から広告を提供するには、このオプションを選択します。 |  ビデオ (スポット)  |
| Taboola |  [Taboola](https://www.taboola.com/) から広告を提供するには、このオプションを選択します。 |  Banner  |
| Vungle | Select this option to serve ads from [Vungle](https://vungle.com/) | ビデオ (スポット) |
| Undertone | Select this option to serve ads from [Undertone](https://www.undertone.com/). | Banner interstitial |


<span id="other-networks" />

### <a name="other-ad-networks"></a>他の広告ネットワーク

次の表は、広告の種類によって現在サポートされている他のネットワークを示しています。

|  広告ネットワーク  |  説明  |  サポートされている広告の種類  |
|--------------|---------------|---------------------|
| Microsoft コミュニティ広告 |  [自分のアプリの 1 つを宣伝するプロモーション用の広告キャンペーンを作成](create-an-ad-campaign-for-your-app.md)し、そのキャンペーンを[コミュニティ広告キャンペーン](about-community-ads.md)として構成する場合、このキャンペーンから広告を表示するには、このオプションを選択します。 | バナー、バナー (スポット) |
| Microsoft の自社広告 | [自分のアプリの 1 つを宣伝するプロモーション用の広告キャンペーンを作成](create-an-ad-campaign-for-your-app.md)し、そのキャンペーンを[自社広告キャンペーン](about-house-ads.md)として構成する場合、このキャンペーンから広告を表示するには、このオプションを選択します。 | バナー、バナー (スポット)  |


<span id="network-markets" />

### <a name="supported-markets-for-ad-networks"></a>広告ネットワークでサポートされる市場

利用可能な広告ネットワークは、[サポートされるすべての市場](define-market-selection.md#microsoft-store-consumer-markets)で広告を提供しますが、次の例外があります。

|  広告ネットワーク  |  サポート対象の市場  |
|--------------|---------------------|
| Revcontent | ブラジル、カナダ、フランス、ドイツ、イタリア、日本、スペイン、英国、米国  |
| Smaato | ブラジル、カナダ、フランス、ドイツ、イタリア、日本、スペイン、英国、米国 |
| smartclip | オーストリア、ベルギー、デンマーク、フィンランド、ドイツ、イタリア、オランダ、ノルウェー、スウェーデン、スイス  |
| Undertone | 米国 |

<span id="coppa" />

## <a name="coppa-compliance"></a>COPPA 準拠

When you [create an ad unit](#create-ad-unit) or [select an existing ad unit](#available-ad-units), the **COPPA compliance** section appears at the bottom of the page if the selected app for the ad unit has at least one submission that has reached the [in the Store](../publish/the-app-certification-process.md#in-the-store) step in the app certification process.

アプリの対象が 13 歳未満の子供である場合、児童オンライン プライバシー保護法 ("COPPA") に従って、このセクションで **[This application is directed at children under the age of 13]** (このアプリは 13 歳未満の子供を対象としています) を選ぶ必要があります。 このオプションを選んだ場合、マイクロソフトはアプリに広告を配信する際に、行動広告サービスを無効にする手順を実行します。

選んだ **COPPA 準拠**設定は、選んだアプリのすべての広告ユニットに自動的に適用されます。

> [!IMPORTANT]
> アプリの対象が 13 歳未満の子供である場合、COPPA の下で特定の義務が発生します。 義務について詳しくは、[こちらのページ](https://www.ftc.gov/enforcement/rules/rulemaking-regulatory-reform-proceedings/childrens-online-privacy-protection-rule)をご覧ください。
