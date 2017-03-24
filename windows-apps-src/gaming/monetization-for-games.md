---
author: joannaleecy
title: "ゲームの収益化"
description: "Windows 10 のユニバーサル Windows プラットフォーム (UWP) ゲームに、バナー広告、ビデオ スポット広告、アプリ内購入を実装します。"
ms.assetid: 79f4e177-d8e7-45d3-8a78-31d4c2fe298a
ms.author: joanlee
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, UWP, ゲーム, 収益化"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: eccff6f037890fdd375eb150520db99a67aa718d
ms.lasthandoff: 02/07/2017

---
#  <a name="monetization-for-games"></a>ゲームの収益化

ゲーム開発者にとって収益化のオプションを把握することは、ビジネスを維持し、優れたゲームの開発という素晴らしい仕事を続けるために不可欠です。 この記事では、ユニバーサル Windows プラットフォーム (UWP) ゲームの収益化とその実装方法の概要を示します。

以前は、ゲームから収益を得るには、価格を設定し、それがストアで購入されるのを待つ以外に方法はありませんでした。 しかし今は、さまざまなオプションがあります。 実店舗での販売や、オンラインでの販売 (物理メディアまたはソフト コピー) のほか、ゲームを無料で公開する代わりに広告を表示したり、無料のゲーム内で有料のアイテムを販売したりする方法もあります。 ゲーム自体も、もはや単体の製品ではありません。 メインのゲームに関連して追加のコンテンツを販売する方法が主流となっています。 

UWP ゲームの販売促進や収益化には、次の方法があります。
* Windows ストアで販売する。Windows ストアは、[世界規模の販売チャネル](#worldwide-distribution-channel)を介して製品を提供するセキュリティで保護されたオンライン ストアです。 世界中のゲーマーが、[販売者の設定する価格](#set-a-price-for-your-game)でゲームをオンラインで購入できます。
* Windows SDK の API を使って[ゲーム内購入](#in-game-purchases)を作成する。 ゲーマーはゲーム内からアイテムを購入したり、特別な機器、スキン、地図、ゲーム レベルなどの追加のコンテンツを購入したりすることができます。
* [Microsoft Store Services SDK](https://visualstudiogallery.msdn.microsoft.com/229b7858-2c6a-4073-886e-cbb79e851211) の API を使って、広告ネットワークから供給される広告を表示する。 [ゲーム内で広告を表示](#display-ads-in-your-game)し、ゲーマーがビデオ広告を見るとゲーム内のリワードがもらえるオプションを提供できます。
* [広告キャンペーンによってゲームの可能性を最大限に広げる](#maximize-your-games-potential-through-ad-campaigns)。 有料広告、コミュニティ広告 (無料)、または自社広告 (無料) キャンペーンを使ってゲームを宣伝し、ユーザー ベースを拡大します。
 
## <a name="worldwide-distribution-channel"></a>世界規模の販売チャネル

Windows ストアでは、世界中の 200 以上の国や地域のユーザーにダウンロード配信でゲームを提供し、Visa、MasterCard、PayPal などのさまざまな請求方法を通じて料金を受け取ることができます。 すべての国と地域の一覧については、「[価格設定と市場設定](https://msdn.microsoft.com/windows/uwp/publish/define-pricing-and-market-selection#markets-and-custom-prices)」をご覧ください。

## <a name="set-a-price-for-your-game"></a>ゲームの価格設定 

ストアに公開する UWP ゲームは、_有料_または_無料_に設定できます。 有料ゲームでは、ゲームを開始する前に、販売側が設定したゲームの価格がユーザーに請求されます。無料ゲームでは、料金を支払うことなくゲームをダウンロードしてプレイできます。

以下に、ストアでのゲームの価格設定に関するいくつかの重要な概念を説明します。

### <a name="base-price"></a>基本価格 

ゲームの基本価格は、提供するゲームが_有料_か_無料_かを決定する要素です。 [デベロッパー センター ダッシュ ボード](https://developer.microsoft.com/windows)を使って、国や地域ごとに基本価格を構成できます。 場合によっては、価格を決定する際に、[他国向けに販売する場合の納税義務](https://msdn.microsoft.com/windows/uwp/publish/tax-details-for-paid-apps)と[特定の市場向けに販売する場合のコストに関する考慮事項](https://msdn.microsoft.com/windows/uwp/publish/define-pricing-and-market-selection#price-considerations-for-specific-markets)を考慮する必要があります。 また[特定の市場向けにカスタム価格を設定](https://msdn.microsoft.com/windows/uwp/publish/define-pricing-and-market-selection#markets-and-custom-prices)することもできます。 詳細については、「[価格設定と市場設定](https://msdn.microsoft.com/windows/uwp/publish/define-pricing-and-market-selection)」をご覧ください。

### <a name="sale-price"></a>セール価格

ゲームの販売促進方法の 1 つに、期間を限定して価格を下げる方法があります。 セール価格を__無料__に設定して、無料でゲームのダウンロードを許可することもできます。
あらかじめセールの開始日と終了日の両方を設定することで、セール キャンペーンのスケジュールを事前に設定できます。 詳しくは、「[アプリとアドオンを販売する](https://msdn.microsoft.com/windows/uwp/publish/put-apps-and-add-ons-on-sale)」をご覧ください。

## <a name="in-game-purchases"></a>ゲーム内購入

ゲーム内購入とは、ゲームの中で購入できる製品です。 これらの製品は、一般に_アプリ内購入_とも呼ばれます。 Windows ストアでは、これらの製品を_アドオン_と呼びます。 [アドオンの公開](https://msdn.microsoft.com/windows/uwp/publish/add-on-submissions)には、Windows デベロッパー センター ダッシュボードを使います。 また、ゲームのコードでアドオンを有効にする必要があります。

### <a name="types-of-add-ons"></a>アドオンの種類

ストアでは、_永続的_と_コンシューマブル_の 2 種類のアドオンを作成できます。 永続的なアドオンは、指定された時間にわたって使うことができ、有効期限の間に一度だけ購入できる項目です。 コンシューマブルなアドオンとは、繰り返し購入して使うことができる項目です。

コンシューマブルを作成する際は、追跡方法として、それらの項目が_開発者による管理_か_ストアによる管理_か (この機能は Windows 10、バージョン 1607 以降で使用可能) を決定してください。 開発者により管理されるコンシューマブルの場合、開発者はゲーマーが持つ項目の残高を追跡する責任があります。ストアで管理されるコンシューマブルの場合、Windows ストアが開発者に代わって項目の残高を追跡します。 詳しくは、「[コンシューマブルなアドオン購入の有効化](https://msdn.microsoft.com/windows/uwp/monetize/enable-consumable-add-on-purchases#overview-of-consumable-add-ons)」をご覧ください。

### <a name="create-in-game-purchases"></a>ゲーム内購入の作成

最新のアプリ内購入およびライセンス情報 API は、Windows SDK (Windows 10 バージョン 1607 以降) の [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) 名前空間に含まれています。 1607 以降のリリースをターゲットとする新しいゲームを開発している場合は、最新のアドオンの種類をサポートし、パフォーマンスに優れた __Windows.Services.Store__ 名前空間の使用をお勧めします。
この名前空間はまた、Windows デベロッパー センターとストアで今後サポートされる種類の製品と機能に対応するように設計されています。 以前のバージョンの Windows 10 向けに開発を行う場合は、この代わりに [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) 名前空間を使います。

詳しくは、「[アプリ内購入と試用版](https://msdn.microsoft.com/windows/uwp/monetize/in-app-purchases-and-trials)」をご覧ください。

#### <a name="simplified-purchase-example"></a>単純な購入の例

このセクションでは、単純な購入の例を通じて、さまざまなメソッド呼び出しを使って購入フローを実装する方法を説明します。

|ゲーム内の操作/アクティビティ                                                | ゲームのバックグラウンド タスク                  |
|--------------------------------------------------------------------------|----------------------------------------|
|ゲーマーが、ショップにアクセスします。 ショップ メニューがポップアップ表示され、入手可能なアドオンと購入価格が示されます。 |  ゲームがアドオンの[製品情報を取得](https://msdn.microsoft.com/windows/uwp/monetize/get-product-info-for-apps-and-add-ons)し、[アドオンが適切なライセンスを持つかどうかを判定](https://msdn.microsoft.com/windows/uwp/monetize/get-license-info-for-apps-and-add-ons)して、ゲーマーが購入できるアドオンをショップのメニューに表示します。                           |
|ゲーマーが __[購入]__ をクリックして項目を購入します。             |__[購入]__ アクションが、項目の購入要求を送信し、それを取得するための支払いプロセスを開始します。 実装は、項目の種類によって異なります。 [永続的または 1 回限りの購入項目](https://msdn.microsoft.com/windows/uwp/monetize/enable-in-app-purchases-of-apps-and-add-ons)の場合、ユーザーは有効期限が切れるまで、1 つの項目のみを所有できます。 項目が[コンシューマブル](https://msdn.microsoft.com/windows/uwp/monetize/enable-consumable-add-on-purchases)の場合、お客様は 1 つ以上の項目を所有できます。 |

### <a name="test-in-game-purchases-during-game-development"></a>ゲーム開発時のゲーム内購入のテスト

アドオンはゲームと関連して作成する必要があるため、ゲームをストアで公開して提供する必要があります。 このセクションでは、まだ開発段階にあるゲームに対してアドオンを作成する手順を示します 
(完成したゲームを既にストアに公開している場合は、最初の 3 つのステップをスキップして、「[ストアでのアドオンの作成](#create-an-add-on-in-the-store)」に進むことができます)。

開発中のゲームに対してアドオンを作成するには、次の手順を実行します。 
1. [パッケージを作成する](#create-a-package)
2. [非表示としてゲームを公開する](#publish-the-game-as-hidden)
3. [Visual Studio で、ゲーム ソリューションをストアに関連付ける](#associate-your-game-solution-with-the-store)
4. [ストアでアドオンを作成する](#create-an-add-on-in-the-store)

#### <a name="create-a-package"></a>パッケージを作成する

公開されるすべてのゲームは、Windows アプリ認定の最小要件を満たす必要があります。 ゲームが Windows ストアで公開するための要件に適合していることを確認するには、Windows 10 SDK に含まれている [Windows アプリ認定キット](https://msdn.microsoft.com/windows/uwp/debug-test-perf/windows-app-certification-kit)を使ってゲームをテストすることができます。 Windows アプリ認定キットが含まれている Windows 10 SDK をまだダウンロードしていない場合は、[Windows 10 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk) にアクセスしてください。

ストアにアップロード可能なパッケージを作成するには:

1. Visual Studio でゲーム ソリューションを開きます。
2. Visual Studio 内で、__[プロジェクト]__ > __[ストア]__ > __[アプリ パッケージの作成]__ の順に選びます。
3. __[Windows ストアにアップロードするパッケージを作成しますか?]__ オプションで __[はい]__ を選びます。
4. デベロッパー センターの開発者アカウントにサインインします。 アカウントを持っていない場合は、開発者アカウントに[登録](https://developer.microsoft.com/store/register)できます。
5. アップロード パッケージを作成するアプリを選びます。 アプリの申請をまだ作成していない場合は、新しいアプリ名を指定して新しい申請を作成します。 詳しくは、「[名前の予約によるアプリの作成](https://msdn.microsoft.com/windows/uwp/publish/create-your-app-by-reserving-a-name)」をご覧ください。
6. パッケージが正常に作成されたら、__[Windows アプリ認定キットを起動する]__をクリックしてテスト プロセスを開始します。
7. エラーを修正してゲームパッケージを作成します。

#### <a name="publish-the-game-as-hidden"></a>非表示としてゲームを公開する

1. [デベロッパー センター](https://developer.microsoft.com/store)にアクセスしてサインインします。
2. __[ダッシュボード概要]__ ページまたは __[すべてのアプリ]__ ページで、目的のアプリをクリックします。 アプリの申請をまだ作成していない場合は、__[新しいアプリの作成]__ をクリックして名前を予約します。
3. __[アプリの概要]__ ページで、__[提出を開始する]__ をクリックします。
4. この新しい申請を構成します。 [申請] ページで、次の手順を実行します。 
    * __[価格と使用可能状況]__ をクリックします。 __[表示]__ セクションで __[このアプリを非表示にして取得できないようにします]__'を選び、 開発チームのみがゲームにアクセスできるように設定します。 詳しくは、「[分布と認知度](https://msdn.microsoft.com/windows/uwp/publish/set-app-pricing-and-availability#distribution-and-visibility)」をご覧ください。
    * __[プロパティ]__ をクリックします。 __[カテゴリとサブカテゴリ]__ セクションで __[ゲーム]__ を選択し、ゲームに適したサブカテゴリを選びます。
    * __[年齢区分]__ をクリックします。 質問表に正確に入力します。
    * __[パッケージ]__ をクリックします。 前の手順で作成したゲーム パッケージをアップロードします。
5. ダッシュボードに表示されるその他の申請プロンプトに従って、このゲームを一般のユーザーに対して非表示にしたまま、正常に公開します。
6. __[ストアに提出]__ をクリックします。

詳しくは、「[アプリの申請](https://msdn.microsoft.com/windows/uwp/publish/app-submissions)」をご覧ください。

ゲームをストアに提出すると、[アプリの認定プロセス](https://msdn.microsoft.com/windows/uwp/publish/the-app-certification-process)が開始します。 このプロセスでは、ゲームが表示されるまでに最大 16 時間かかることがあります。

#### <a name="associate-your-game-solution-with-the-store"></a>ゲーム ソリューションをストアに関連付ける

次の手順を実行して、Visual Studio でゲーム ソリューションを開きます。

1. __[プロジェクト]__ > __[ストア]__ > __[アプリケーションをストアと関連付ける]__ の順に選びます。
2. デベロッパー センターの開発者アカウントにサインインし、このソリューションに関連付けるアプリ名を選択します。
3. __Package.appxmanifest.xml ファイル__をダブルクリックし、__[パッケージ化]__ タブに移動してゲームが正しく関連付けられていることを確認します。

すでにライブのゲームとしてストアに掲載されている公開済みのゲームにソリューションを関連付けた場合、ソリューションにアクティブなライセンスが設定され、ゲーム用のアドオン作成が一歩前進します。 詳しくは、「[アプリのパッケージ化](https://msdn.microsoft.com/windows/uwp/packaging/index)」をご覧ください。

#### <a name="create-an-add-on-in-the-store"></a>ストアでアドオンを作成する

アドオンを作成する際は、提出した適切なゲームを確認してアドオンを関連付けます。 アドオンに関連付けるすべての多様な情報を構成する方法について詳しくは、「[アドオンの申請](https://msdn.microsoft.com/windows/uwp/publish/add-on-submissions)」をご覧ください。

1. [デベロッパー センター](https://developer.microsoft.com/store)にアクセスしてサインインします。
2. __[ダッシュボード概要]__ ページまたは __[すべてのアプリ]__ ページで、アドオンを作成するアプリをクリックします。
3. __[アプリの概要]__ ページの __[アドオン]__ セクションで、__[新しいアドオンを作成する]__ をクリックします。
4. アドオンの製品の種類として、__[開発者により管理されるコンシューマブル]__、__[ストアで管理されるコンシューマブル]__、__[永続的]__のいずれかを選択します。
5. 一意の製品 ID を入力します。この製品 ID は、このアドオンをゲーム コードに統合する際に文字列変数として使われます。 この ID は、ユーザーには表示されません。 詳しくは、「[アドオンの製品の種類と製品 ID を設定する](https://msdn.microsoft.com/windows/uwp/publish/set-your-add-on-product-id)」をご覧ください。

アドオンのその他の構成は次のとおりです。
* [プロパティ](https://msdn.microsoft.com/windows/uwp/publish/enter-add-on-properties)
* [価格と使用可能状況](https://msdn.microsoft.com/windows/uwp/publish/set-add-on-pricing-and-availability)
* [ストア登録情報](https://msdn.microsoft.com/windows/uwp/publish/create-add-on-store-listings)

ゲームに多くのアドオンがある場合は、__Windows ストア申請 API__ を使ってプログラムによってアドオンを作成できます。 詳しくは、「[Windows ストア サービスを使用した申請の作成と管理](https://msdn.microsoft.com/windows/uwp/monetize/create-and-manage-submissions-using-windows-store-services)」をご覧ください。

## <a name="display-ads-in-your-game"></a>ゲーム内での広告の表示

Microsoft Store Services SDK に含まれているライブラリやツールを使って、広告ネットワークから広告を受信するサービスをゲーム内に設定できます。 ゲーマーに対してライブ広告が表示され、ゲーマーが表示された広告を見たり操作したりしたときに、広告主から開発者に報酬が支払われます。 詳しくは、「[広告を含むアプリを作成するためのワークフロー](https://msdn.microsoft.com/windows/uwp/monetize/workflows-for-creating-apps-with-ads)」をご覧ください。

### <a name="ad-formats"></a>広告の形式

Microsoft Store Services SDK を使って表示できる広告には、次の 2 種類があります。

* バナー広告 &mdash; ゲーム画面の一部に表示される広告であり、通常、ゲーム内に表示されます。
* ビデオ スポット広告 &mdash; 全画面表示の広告であり、ゲームのレベル間で使うと非常に効果的です。 正しく実装すれば、バナー広告よりもゲームのプレイを阻害しない可能性があります。

### <a name="which-ads-are-displayed"></a>表示される広告

Microsoft Store Services SDK を使う場合、弊社のパートナー ネットワークを通じて広告が供給されます。 現在提供されている広告ネットワークについて詳しくは、「[広告を使用したアプリの収益化](https://developer.microsoft.com/store/monetize/ads-in-apps)」をご覧ください。
AdControl を使って広告を表示する場合は、ゲーム内に表示される製品広告を拡張して、[アフィリエイト広告](https://msdn.microsoft.com/windows/uwp/publish/about-affiliate-ads)を表示するようにオプトインすることができます。

### <a name="which-markets-allow-ads-to-be-displayed"></a>広告を表示できる市場

バナー広告やビデオ スポット広告は、対象の国を選択してユーザーに表示できます。 広告がサポートされるすべての国と地域の一覧については、「[Microsoft Advertising でサポートされる市場](https://msdn.microsoft.com/windows/uwp/monetize/supported-markets-for-microsoft-advertising)」をご覧ください。

### <a name="apis-for-displaying-ads"></a>広告を表示するための API

ゲーム内に広告を表示するには、Microsoft Store Services SDK の [Microsoft.Advertising.WinRT.UI](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.aspx) 名前空間に含まれる [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) クラスと [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) クラスを使います。

これにはまず、[Microsoft Store Services SDK](http://aka.ms/store-em-sdk) をダウンロードし、Visual Studio 2015 と共にインストールします。 詳しくは、「[SDK で利用可能な機能](https://msdn.microsoft.com/windows/uwp/monetize/microsoft-store-services-sdk#features-available-in-the-sdk)」をご覧ください。

#### <a name="implementation-guides"></a>実装ガイド

次のウォークスルーでは、__AdControl__ と __InterstitialAd__ を使って広告を実装する方法を説明しています。

* [XAML および .NET で AdControl クラスを使ったバナー広告の作成](https://msdn.microsoft.com/windows/uwp/monetize/adcontrol-in-xaml-and--net)
* [HTML5 および JavaScript で AdControl クラスを使ったバナー広告の作成](https://msdn.microsoft.com/windows/uwp/monetize/adcontrol-in-html-5-and-javascript)
* [InterstitialAd クラスを使ったビデオ スポット広告の作成](https://msdn.microsoft.com/windows/uwp/monetize/interstitial-ads)

開発時には、次のテスト値を使って広告がどのようにレンダリングされるかを確認できます。 上のウォークスルーでも、これらと同じ値が使われています。

|AdType             | AdUnitId  | AppId                              |
|-------------------|-----------|------------------------------------|
|バナー広告         |10865270   |3f83fe91-d6be-434d-a0ae-7351c5a997f1|
|スポット広告   |11389925   |d25517cb-12d4-4699-8bdc-52040c712cab|

設計と実装のプロセスに役立つベスト プラクティスを次に示します。

* [AdControl クラスを使ったバナー広告に関するベスト プラクティス](https://msdn.microsoft.com/windows/uwp/monetize/ui-and-user-experience-guidelines)
* [InterstitialAd クラスを使ったスポット広告に関するベスト プラクティス](https://msdn.microsoft.com/windows/uwp/monetize/ui-and-user-experience-guidelines#interstitialbestpractices10)

広告が表示されない、ブラック ボックスが点滅し、表示されなくなる、広告が更新されないなど、開発上の一般的な問題に対する解決策については、[トラブルシューティング ガイド](https://msdn.microsoft.com/windows/uwp/monetize/troubleshooting-guides)」をご覧ください。

### <a name="prepare-for-release-by-replacing-ad-unit-test-values"></a>広告ユニットのテスト値を置き換えてリリースの準備をする

ライブ テストに移行する準備または公開したゲームで広告を受信する準備ができたら、テスト用の広告ユニット値を、ゲーム用に提供された実際の値に更新する必要があります。
ゲーム用の広告ユニットを作成する方法については、「[アプリの広告ユニットをセットアップする](https://msdn.microsoft.com/windows/uwp/monetize/set-up-ad-units-in-your-app)」をご覧ください。

### <a name="other-ad-networks"></a>他の広告ネットワーク

UWP アプリおよび UWP ゲームへの広告提供をサポートするその他の広告ネットワークを次に示します。

#### <a name="vungle"></a>Vungle

Vungle SDK for Windows は、アプリやゲームに表示するビデオ広告を提供します。 この SDK をダウンロードするには [Vungle SDK](https://v.vungle.com/sdk) にアクセスしてください。

#### <a name="smaato"></a>Smaato

Smaato では、UWP アプリと UWP ゲームにバナー広告を組み込むことができます。 [SDK](https://www.smaato.com/resources/sdks/) をダウンロードし、[ドキュメント](https://wiki.smaato.com/display/SPX/Windows+Phone)で詳しい情報をご覧ください。

#### <a name="adduplex"></a>AdDuplex

AdDuplex を使うと、ゲームにバナー広告やスポット広告を実装できます。

Windows 10 XAML プロジェクトに直接 AdDuplex を統合する方法について詳しくは、次の AdDuplex の Web サイトにアクセスしてください。
* バナー広告: [Windows 10 SDK for XAML](https://adduplex.zendesk.com/hc/en-us/articles/204849031-Windows-10-SDK-for-XAML-apps-installation-and-usage) 
* スポット広告: [Windows 10 XAML AdDuplex Interstitial Ad Installation and Usage (Windows 10 XAML AdDuplex のスポット広告のインストールと使用)](https://adduplex.zendesk.com/hc/en-us/articles/204849091-Windows-10-XAML-AdDuplex-Interstitial-Ad-Installation-and-Usage)

Unity を使って作成された Windows 10 UWP ゲームに AdDuplex SDK を統合する方法については、「[Windows 10 SDK for Unity apps installation and usage (Windows 10 SDK for Unity アプリのインストールと使用)](https://adduplex.zendesk.com/hc/en-us/articles/207279435-Windows-10-SDK-for-Unity-apps-installation-and-usage)」をご覧ください。

## <a name="maximize-your-games-potential-through-ad-campaigns"></a>広告キャンペーンによってゲームの可能性を最大限に広げる

広告を利用して、ゲームの販売促進を強化しましょう。 ゲームの[広告キャンペーンを作成](https://msdn.microsoft.com/windows/uwp/publish/create-an-ad-campaign-for-your-app)すると、ゲームの販売を促進するための広告が、他の開発者のアプリやゲームに表示されます。 

ゲーマー ベースの増加に役立つキャンペーンを複数の種類から選択できます。

|キャンペーンの種類             | ゲームの広告が表示されるアプリ                                                                                                                                                                   |
|--------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|有料                      |ゲームのデバイスまたはカテゴリに一致するアプリ。                                                                                                                                                   |
|コミュニティ (無料)            |コミュニティ広告キャンペーンにオプト インしている他の開発者が公開するアプリ。 詳しくは、「[コミュニティ広告について](https://msdn.microsoft.com/windows/uwp/publish/about-community-ads)」をご覧ください。|
|自社 (無料)                |自社が公開したアプリのみ。 詳しくは、「[自社広告について](https://msdn.microsoft.com/windows/uwp/publish/about-house-ads)」をご覧ください。                                                            |

## <a name="related-links"></a>関連リンク

* [支払いの受け取り](https://msdn.microsoft.com/windows/uwp/publish/getting-paid-apps)
* [アカウントの種類、場所、料金](https://msdn.microsoft.com/windows/uwp/publish/account-types-locations-and-fees)
* [分析](https://msdn.microsoft.com/windows/uwp/publish/analytics)
* [グローバリゼーションとローカライズ](https://msdn.microsoft.com/windows/uwp/globalizing/globalizing-portal)
* [アプリの試用版の実装](https://msdn.microsoft.com/windows/uwp/monetize/implement-a-trial-version-of-your-app)
* [A/B テストを使用してアプリの試験的機能を実行する](https://msdn.microsoft.com/windows/uwp/monetize/run-app-experiments-with-a-b-testing)