---
Description: アプリで Microsoft Advertising SDK を使用して広告を表示する場合は、パートナーセンターのアプリ内広告ページを使用して、広告の使用を管理します。
title: アプリ内広告
ms.assetid: 09970DE3-461A-4E2A-88E3-68F2399BBCC8
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c8d01042fec7435652c819f29e3a791a5623a947
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/24/2020
ms.locfileid: "91220265"
---
# <a name="in-app-ads"></a>アプリ内広告

>[!WARNING]
> 2020年6月1日から、Microsoft Ad 収益化 platform for Windows UWP アプリがシャットダウンされます。 [詳細情報](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

**Monetize** &gt; [パートナーセンター](https://partner.microsoft.com/dashboard)の [収益化**のアプリ内広告**] ページを使用して、以下の ad ユニットを作成および管理します。

* [Microsoft Advertising SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK) を使用するユニバーサル Windows プラットフォーム (UWP) アプリ。
* 以前に発行された Windows 8.x と Windows Phone 8 .x アプリでは、 [MICROSOFT ADVERTISING SDK For windows と Windows Phone](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDKforWindowsandWindowsPhone8x)2.x が使用されています。

> [!IMPORTANT]
> Windows Phone 8. x SDK を使用してビルドされた新しい XAP パッケージをアップロードすることはできなくなりました。 既に XAP パッケージと共にストアに格納されているアプリは、引き続き Windows 10 Mobile デバイスで動作します。 詳細については、こちらの [ブログ投稿](https://blogs.windows.com/windowsdeveloper/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store)を参照してください。

これらの SDK をアプリに統合して広告を表示する方法については、「[Microsoft Advertising SDK を使用したアプリでの広告の表示](../monetize/display-ads-in-your-app.md)」をご覧ください。

<span id="create-ad-unit" />

## <a name="create-ad-units"></a>広告ユニットを作成

アプリ内の[バナー広告](../monetize/banner-ads.md), [interstitial ad](../monetize/interstitial-ads.md)または[ネイティブ広告](../monetize/native-ads.md)用に広告ユニットを作成するには:

1.  パートナーセンターの [ **収益化** &gt; **のアプリ内広告** ] ページにアクセスし、[ **ad ユニットの作成**] をクリックします。
2.  **[アプリ名]** ドロップダウンで、広告ユニットを使用するアプリを選択します。
3.  **[広告ユニット名]** フィールドに広告ユニットの名前を入力します。 レポートで広告ユニットを識別しやすくするために、任意の説明文字列を指定できます。
4.  **[広告ユニットの種類]** ドロップダウンで、広告の種類を選択します。

    * アプリにバナー広告が表示されている場合は、[ **バナー**] を選択します。
    * アプリにスポット video ad またはスポットバナー広告が表示されている場合は、[ **video スポット** ] または [ **バナースポット** ] を選択します (表示するスポット ad の種類に適したオプションを選択してください)。
    * アプリにネイティブ広告が表示されている場合は、[ **ネイティブ**] を選択します。

5. **[デバイス ファミリ]** ドロップダウンで、広告ユニットを使うアプリがターゲットとしているデバイス ファミリを選択します。 選択できるオプションには、**[UWP (Windows 10)]**、**[PC/タブレット (Windows 8.1)]**、**[モバイル (Windows Phone 8.x)]** があります。

6. 必要に応じて、次の追加設定を構成します。

    * 広告ユニットのターゲットに **[UWP (Windows 10)]** デバイス ファミリを選択した場合は、オプションで広告ユニットの[仲介設定](#mediation)を構成できます。
    * バナー広告ユニットのターゲットとして **[PC/タブレット (Windows 8.1)]** デバイス ファミリまたは **[モバイル (Windows Phone 8.x)]** デバイス ファミリを選択した場合は、オプションで **[アプリにコミュニティ広告を表示する]** を選択して[コミュニティ広告](about-community-ads.md)にオプトインすることができます。

7.  選択したアプリに対して COPPA 準拠を設定していない場合は、[[COPPA 準拠](#coppa)] セクションでオプションを選択します。
8.  **[広告ユニットを作成]** をクリックします。

作成した新しい広告ユニットは、**[収益化]** &gt; **[アプリ内広告]** ページにある利用可能な広告ユニットの表に表示されます。

<span id="available-ad-units" />

## <a name="review-and-edit-ad-units"></a>広告ユニットの確認と編集

アカウント内で 1 つ以上のアプリに対して広告ユニットを作成すると、これらの広告ユニットが **[収益化]** &gt; **[アプリ内広告]** ページの下部にある表に表示されます。 この表には、各広告ユニットの **[アプリケーション ID]** および **[広告ユニット ID]** がその他の情報と共に表示されます。 アプリに広告を表示するには、コードでこれらの値を使う必要があります。 詳しくは、「[アプリの広告ユニットをセットアップする](../monetize/set-up-ad-units-in-your-app.md)」をご覧ください。

* アプリに[バナー広告](../monetize/banner-ads.md)を表示する場合は、これらの値を [AdControl](/uwp/api/microsoft.advertising.winrt.ui.adcontrol) オブジェクトの [ApplicationId](/uwp/api/microsoft.advertising.winrt.ui.adcontrol.applicationid) プロパティと [AdUnitId](/uwp/api/microsoft.advertising.winrt.ui.adcontrol.adunitid) プロパティに割り当てる必要があります。
* アプリで[スポット広告](../monetize/interstitial-ads.md)が表示されている場合は、 [InterstitialAd](/uwp/api/microsoft.advertising.winrt.ui.interstitialad)オブジェクトの[requestad](/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad)メソッドにこれらの値を渡します。
* アプリが [ネイティブ広告](../monetize/native-ads.md)を表示する場合は、これらの値を **NativeAdsManagerV2** コンストラクターに渡します。
  > [!IMPORTANT]
  > 各広告ユニットは 1 つのアプリのみで使用できます。 同じ広告ユニットを複数のアプリで使うと、その広告ユニットには広告が配信されません。

  > [!NOTE]
  > 1 つのアプリに複数のバナー広告コントロール、スポット広告コントロール、ネイティブ広告コントロールを使用できます。 このシナリオでは、各コントロールに異なる広告ユニットを割り当てることをお勧めします。 各コントロールに異なる広告ユニットを使用することで、別々に[仲介の設定を構成](../publish/in-app-ads.md#mediation)して、個別の[報告データ](../publish/advertising-performance-report.md)を取得することが可能です。 また、これにより、Microsoft のサービスはアプリに提供する広告を最適化できます。

UWP 広告ユニットの[仲介設定](#mediation)または広告ユニットを使用しているアプリの [COPPA 準拠](#coppa)を編集するには、ユニット名をクリックします。

> [!NOTE]
> Ad ユニットに過去6か月間のアクティビティがない場合、 **非アクティブ**としてラベルが付けられ、最終的にパートナーセンターから削除されます。 フィルターを使用して "**アクティブ**" または "**非アクティブ**" の広告ユニットのみを表示することもできます。 誤って "**非アクティブ**" がマークされていると思われる広告ユニットを見つけた場合は、[サポートにお問い合わせください](https://developer.microsoft.com/windows/support)。

<span id="mediation" />

## <a name="mediation-settings"></a>仲介設定

[新しい uwp ad ユニットを作成](#create-ad-unit)するとき、または[既存の uwp ad ユニットを編集](#available-ad-units)するときに、このセクションのオプションを使用して、ad ユニットの[ad 仲介](../monetize/ad-mediation-service.md)装置を構成します。 広告仲介を使うと、複数の広告ネットワークから広告を表示して、広告収益とアプリ プロモーションの機能を最大限に引き出すことができます。表示される広告には、他の有料広告ネットワークからの広告や、Microsoft のアプリ プロモーション キャンペーン用の収益が生じない広告などが含まれます。 選択した広告ネットワークからのバナー広告要求の仲介は自動的に行われます。 アプリ内のバナー広告、スポット広告、またはネイティブ広告に既に関連付けられている UWP 広告ユニットがある場合は、広告仲介を有効にするためにアプリのコードを変更する必要はありません。

> [!NOTE]
> UWP 広告ユニットで広告仲介を有効にする場合、サードパーティの広告ネットワークから広告ユニットを取得する必要はありません。 必要なサードパーティの広告ユニットは、広告仲介サービスによって自動的に作成されます。

アプリ内の UWP 広告ユニットに対して広告仲介設定を構成するには、次の手順を実行します。

1. [広告ユニットを作成](#create-ad-unit)するか、[既にある広告ユニットを選択](#available-ad-units)します。
2. [ **アプリ内の広告** ] ページで、[ **仲介の設定** ] セクションにアクセスし、設定を構成します。

    * 既定では、 **[Microsoft** による設定の最適化を許可する] チェックボックスがオンになっています。 このオプションを使うことをお勧めします。 このオプションでは、アプリがサポートする各市場での広告収益を最大化できるように、機械学習アルゴリズムを使ってアプリの広告仲介設定を自動的に選択します。 このオプションを使用する場合は、構成で使用する ad ネットワークを選択することもできます。 構成に含めない ad ネットワークをオフにします。このアルゴリズムでは、アプリが選択された ad ネットワークからの広告のみを受信するようにします。
    * 独自の ad 仲介用設定を選択する場合は、[ **既定の設定を変更**する] を選択します。

    > [!NOTE]
    > このセクションの残りの手順は、[ **既定の設定を変更**する] を選択した場合にのみ適用されます。

3. **[ターゲット]** ドロップダウンで **[ベースライン]** を選択して、既定の広告仲介設定を構成します。 この既定の構成は、市場固有の構成が定義されていないすべての市場に適用されます。
4. 次に、有料ネットワーク (広告表示に応じて収益が支払われるネットワーク) とその他の広告ネットワーク (広告表示に対する収益の支払いがないネットワーク) について、コントロールに表示するそれぞれの広告の割合を指定します。 これを行うには、**[有料の広告ネットワーク]** と **[他の広告ネットワーク]** の **[重さ]** フィールドに 0 ～ 100 の値を入力します。  
5. [**有料 ad ネットワーク**] セクションで、使用する各[有料ネットワーク](#paid-networks)の [**アクティブ**] 列のチェックボックスをオンにし、[**順位**] 列の矢印を使用してランクでネットワークを並べ替えます (これにより、各ネットワークをコントロールで使用する頻度が指定されます)。
6. **[バナー]** または **[バナー (スポット)]** 広告ユニットを選ぶと、**[他の広告ネットワーク]** という名前のセクションも表示されます。 このセクションのネットワークでは、広告インプレッションに対する収益は生じません。 代わりに、これらのネットワークはアプリ プロモーション キャンペーンなどのソースからの広告を表示します。

    **[その他の広告ネットワーク]** セクションで、使用する[その他のネットワーク](#other-networks)ごとに **[有効]** 列のチェック ボックスをオンにします。次に、**[ランク]** 列の矢印を使ってネットワークをランク順に並べ替えます (これで、コントロールによる各ネットワークの使用頻度が指定されます)。 現在サポートされているその他のネットワークは次のとおりです。

7. 既定の仲介構成をオーバーライドする市場ごとに、**[ターゲット]** ドロップダウンで市場を選択し、広告ネットワークの選択とランクを更新します。
8. **[広告ユニットを作成]** (新しい広告ユニットを作成している場合) または **[保存]** (既存の広告ユニットを編集している場合) をクリックします。

<span id="paid-networks" />

### <a name="supported-paid-ad-networks"></a>サポートされている有料広告ネットワーク

次の表は、広告の種類によって現在サポートされている有料ネットワークを示しています。 ただし、[一部の市場では利用できない](#network-markets)ネットワークもあります。

|  広告ネットワーク  |  説明  |  サポートされている広告の種類  |
|--------------|---------------|---------------------|
| Oath と AppNexus |  これは、Microsoft が管理する ad ネットワークで、パートナーネットワーク、Oath、および AppNexus 連携を通じて広告を提供します。<p/>**注**: Oath と appnexus は常に、バナー広告ユニットの **有料 ad ネットワーク** の一覧で最初に順位付けされます。また、これらの種類の広告の低い順位に変更することはできません。 | バナー、ビデオ スポット広告 |
| AppNexus (直接) | このオプションを選択すると、 [appnexus](https://www.appnexus.com)処理から広告が提供されます。 | ビデオ (スポット)、ネイティブ  |
| Microsoft アプリ インストール広告 | Windows エコシステム内の他の開発者で、[各自が開発したアプリのプロモーション用広告キャンペーンを作成している](create-an-ad-campaign-for-your-app.md)開発者によって作成されたアプリ インストール広告やアプリ リエンゲージメント広告を提供するには、このオプションを選択します。  |  バナー、バナー (スポット)、ネイティブ  |
| MSN コンテンツに関する推奨事項 |  このオプションを選択すると、MSN コンテンツの推奨事項から広告が提供されます。 |  バナー、バナー (スポット)  |
| Outbrain |  [Outbrain](https://www.outbrain.com/) から広告を提供するには、このオプションを選択します。 |  バナー、バナー (スポット)  |
| Revcontent |  [Revcontent](https://www.revcontent.com/) から広告を提供するには、このオプションを選択します。 |  バナー、ネイティブ  |
| Smaato |  [Smaato](https://www.smaato.com/) から広告を提供するには、このオプションを選択します。 |  バナー  |
| smartclip |  [smartclip](http://www.smartclip.com/) から広告を提供するには、このオプションを選択します。 |  ビデオ (スポット)  |
| SpotX |  [SpotX](https://www.spotx.tv/) から広告を提供するには、このオプションを選択します。 |  ビデオ (スポット)  |
| Taboola |  [Taboola](https://www.taboola.com/) から広告を提供するには、このオプションを選択します。 |  バナー  |
| Vungle | [Vungle](https://vungle.com/)から広告を提供するには、このオプションを選択します | ビデオ (スポット) |
| 低音 | このオプションを選択すると、 [広告の機能](https://www.undertone.com/)が低下します。 | バナースポット |


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
| 低音 | アメリカ合衆国 |

<span id="coppa" />

## <a name="coppa-compliance"></a>COPPA 準拠

Ad ユニットを [作成](#create-ad-unit) するか、 [既存の ad ユニットを選択](#available-ad-units)すると、[ **COPPA コンプライアンス** ] セクションがページの下部に表示されます。このセクションでは、ad ユニット用に選択したアプリに、アプリ認定プロセスの [ストア](../publish/the-app-certification-process.md#in-the-store) ステップのに到達した送信が少なくとも1つある場合があります。

アプリの対象が 13 歳未満の子供である場合、児童オンライン プライバシー保護法 ("COPPA") に従って、このセクションで **[This application is directed at children under the age of 13]** (このアプリは 13 歳未満の子供を対象としています) を選ぶ必要があります。 このオプションを選んだ場合、マイクロソフトはアプリに広告を配信する際に、行動広告サービスを無効にする手順を実行します。

選んだ **COPPA 準拠**設定は、選んだアプリのすべての広告ユニットに自動的に適用されます。

> [!IMPORTANT]
> アプリの対象が 13 歳未満の子供である場合、COPPA の下で特定の義務が発生します。 義務について詳しくは、[こちらのページ](https://www.ftc.gov/enforcement/rules/rulemaking-regulatory-reform-proceedings/childrens-online-privacy-protection-rule)をご覧ください。
