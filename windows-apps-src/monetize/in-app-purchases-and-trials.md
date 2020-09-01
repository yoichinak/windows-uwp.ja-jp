---
ms.assetid: F45E6F35-BC18-45C8-A8A5-193D528E2A4E
description: UWP アプリでアプリ内購入と試用機能を実装およびテストするために必要な基本的なタスクと概念について説明します。
title: アプリ内購入と試用版
ms.date: 05/09/2018
ms.topic: article
keywords: Windows 10, UWP, アプリ内購入, IAP, アドオン, 試用版, コンシューマブル, 永続的, サブスクリプション
ms.localizationpriority: medium
ms.openlocfilehash: 1027159d250610a7d358f536d34f9c72fde1e940
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155546"
---
# <a name="in-app-purchases-and-trials"></a>アプリ内購入と試用版

Windows SDK に用意されている API を使用して、以下の機能を実装し、ユニバーサル Windows プラットフォーム (UWP) アプリによる収益を向上させることができます。

* **アプリ内購入** &nbsp; &nbsp;アプリが無料かどうかにかかわらず、アプリ内でコンテンツまたは新しいアプリ機能 (ゲームの次のレベルのロックを解除するなど) を販売できます。

* **試用版の機能** &nbsp; &nbsp;[パートナーセンターで無料試用版としてアプリを構成](../publish/set-app-pricing-and-availability.md#free-trial)した場合、試用期間中に一部の機能を除外または制限することで、アプリの完全なバージョンを購入するよう顧客に仕向けることができます。 また、ユーザーがアプリを購入する前の試用期間中にだけバナーや透かしなどを表示する機能を有効にすることもできます。

この記事では、UWP アプリでのアプリ内購入と試用版のしくみについて、その概要を説明します。

<span id="choose-namespace" />

## <a name="choose-which-namespace-to-use"></a>使用する名前空間を選ぶ

UWP アプリにアプリ内購入機能や試用版機能を追加する際に使用できる名前空間は、2 つあります。どちらの名前空間を使用するかは、アプリのターゲットとなる Windows 10 のバージョンによって決まります。 これらの名前空間の API は同じ目的を果たしますが、その設計は大きく異なります。また、この 2 つの API 間にコードの互換性はありません。

* **[Windows. Services. ストア](/uwp/api/windows.services.store)** &nbsp; &nbsp;Windows 10 バージョン1607以降では、アプリはこの名前空間の API を使用して、アプリ内購入と試用版を実装できます。 Visual Studio でアプリ プロジェクトのターゲットが **Windows 10 Anniversary Edition (10.0、ビルド 14393)** 以降である場合は、この名前空間のメンバーを使用することをお勧めします。 この名前空間は、ストアで管理された使用可能なアドオンなどの最新のアドオンの種類をサポートし、パートナーセンターとストアでサポートされる将来の種類の製品や機能と互換性を持つように設計されています。 この名前空間について詳しくは、この記事の「[Windows.Services.Store 名前空間を使用するアプリ内購入と試用版](#api_intro)」をご覧ください。

* **[Windows. ApplicationModel. ストア](/uwp/api/windows.applicationmodel.store)** &nbsp; &nbsp;Windows 10 のすべてのバージョンは、この名前空間でのアプリ内購入と試用用の古い API もサポートしています。 **Windows.ApplicationModel.Store** 名前空間については、「[Windows.ApplicationModel.Store 名前空間を使用するアプリ内購入と試用版](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md)」をご覧ください。

> [!IMPORTANT]
> **Windows.ApplicationModel.Store** 名前空間は今後更新されず、新機能も追加されないため、アプリでは、可能であれば代わりに **Windows.Services.Store** 名前空間を使うことをお勧めします。 [デスクトップブリッジ](https://developer.microsoft.com/windows/bridges/desktop)を使用する windows デスクトップアプリケーションや、パートナーセンターの開発サンドボックスを使用するアプリやゲームでは、 **Windows の Applicationmodel. ストア**名前空間はサポートされていません (たとえば、Xbox Live と統合されているゲームの場合は、これが該当します)。

<span id="concepts" />

## <a name="basic-concepts"></a>基本的な概念

ストアで提供されるアイテムはすべて、通常、*製品*と呼ばれます。 ほとんどの開発者が扱う製品の種類は、*アプリ*と*アドオン*のみです。

アドオンとは、ユーザーがアプリのコンテキストで利用できる製品や機能のことです。たとえば、アプリやゲームで使用される通貨、ゲームの新しい地図や武器、広告を表示せずにアプリを使用できる機能などに加え、音楽やビデオなどのデジタル コンテンツを提供できるアプリなら、そのようなコンテンツのことも表します。 すべてのアプリとアドオンは、そのアプリやアドオンを使用する権利がユーザーにあるかどうかを示す関連付けられたライセンスを備えます。 アプリまたはアドオンを試用版として使用する権利がユーザーにある場合は、その試用版に関する追加情報もライセンスによって提供されます。

アプリで顧客にアドオンを提供するには、ストアが認識できるように、 [パートナーセンターでアプリのアドオンを定義](../publish/add-on-submissions.md) する必要があります。 その後で、アプリで **Windows.Services.Store** または **Windows.ApplicationModel.Store** 名前空間の API を使用して、アプリ内購入としてユーザーに販売するアドオンを提供することができます。

UWP アプリでは、次の種類のアドオンを提供できます。

| アドオンの種類 |  説明  |
|---------|-------------------|
| Durable  |  [パートナーセンターで指定](../publish/enter-add-on-properties.md)した有効期間にわたって保持されるアドオン。 <p/><p/>既定では、永続的なアドオンの有効期限が切れることはありません。この場合、アドオンは 1 回のみ購入することができます。 アドオンで特定の期間を指定している場合、期限が切れた後、ユーザーはアドオンを再購入できます。 |
| 開発者により管理されるコンシューマブル  |  購入して使用し、消費した後でもう一度購入可能なアドオンです。 アドオンが表すユーザーのアイテムの残量の追跡は開発者が行います。<p/><p/>ユーザーがアドオンに関連付けられたアイテムを消費したら、ユーザーの残量を保持し、ユーザーによってすべてのアイテムが消費されたらアドオンの購入をフルフィルメント完了として Microsoft Store に報告する義務が開発者にあります。 以前のアドオン購入をフルフィルメント完了としてアプリで報告するまで、ユーザーがそのアドオンを再購入することはできません。 <p/><p/>たとえば、アドオンがゲーム内の 100 コインを表しており、ユーザーによって 10 コインが消費されている場合、アプリまたはサービスでは、ユーザーの 90 コインの残高を保持する必要があります。 ユーザーによって 100 コインすべてが消費されたら、アプリでそのアドオンをフルフィルメント完了として報告する必要があります。その後、ユーザーは 100 コイン アドオンを再購入できます。    |
| Microsoft Store で管理されるコンシューマブル  |  購入し、使用した後にいつでも再購入できるアドオンです。 アドオンが表すユーザーのアイテムの残量は Microsoft Store で追跡します。<p/><p/>アドオンに関連付けられたアイテムがユーザーにより消費されたら、そのアイテムをフルフィルメント完了として Microsoft Store に報告する義務が開発者にあり、Microsoft Store によってユーザーの残量が更新されます。 ユーザーは、必要な回数だけアドオンを購入できます (最初にアイテムを使用する必要はありません)。 アプリでは、ユーザーのアイテムの現在の残量をいつでも照会できます。 <p/><p/> たとえば、アドオンがゲーム内の 100 のコインの初期量を表しており、ユーザーによって 50 コインが消費された場合、そのアドオンの 50 ユニットがフルフィルメント完了したことをアプリで Microsoft Store に報告すると、Microsoft Store により残高が更新されます。 ユーザーがアドオンを再購入して 100 個以上のコインを獲得した場合、合計で 150 個のコインを手にします。 <p/><p/>**Note** &nbsp; メモ &nbsp;ストアで管理された消耗品を使用するには、アプリで**Windows 10 記念版 (10.0; を対象にする必要があります。ビルド 14393)** またはそれ以降のリリースの Visual Studio では、 **windows**の名前空間ではなく **、名前空間**を使用する必要があります。  |
| サブスクリプション | アドオンを使い続けるために定期的な間隔でユーザーが継続的に課金する永続的なアドオンです。 ユーザーはいつでもサブスクリプションをキャンセルして、それ以降の課金を取り消すことができます。 <p/><p/>**Note** &nbsp; メモ &nbsp;サブスクリプションアドオンを使用するには、アプリで**Windows 10 周年 Edition (10.0;) をターゲットにする必要があります。ビルド 14393)** またはそれ以降のリリースの Visual Studio では、 **windows**の名前空間ではなく **、名前空間**を使用する必要があります。  |

<span />

> [!NOTE]
> パッケージで使用される永続的なアドオン (ダウンロード コンテンツまたは DLC とも呼ばれます) など、他の種類のアドオンが利用できるのは一部の開発者に限られており、このドキュメントでは説明しません。

<span id="api_intro" />

## <a name="in-app-purchases-and-trials-using-the-windowsservicesstore-namespace"></a>Windows.Services.Store 名前空間を使用するアプリ内購入と試用版

このセクションでは、[Windows.Services.Store](/uwp/api/windows.services.store) 名前空間の重要なタスクと概念の概要について説明します。 この名前空間は、Visual Studio で**Windows 10 Anniversary Edition (10.0、ビルド 14393)** 以降 (これは Windows 10 バージョン 1607 に対応) をターゲットとするアプリでのみ利用可能です。 可能であれば、アプリで [Windows.ApplicationModel.Store](/uwp/api/windows.applicationmodel.store) 名前空間ではなく、**Windows.Services.Store** 名前空間を使用することをお勧めします。 **Windows.ApplicationModel.Store** 名前空間については、[この記事](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md)をご覧ください。

**このセクションの内容**

* [ビデオ](#video)
* [StoreContext クラスの概要](#get-started-storecontext)
* [アプリ内購入の実装](#implement-iap)
* [試用版機能の実装](#implement-trial)
* [アプリ内購入や試用版の実装のテスト](#testing)
* [アプリ内購入の受領通知](#receipts)
* [デスクトップ ブリッジでの StoreContext クラスの使用](#desktop)
* [製品、SKU、および可用性](#products-skus)
* [ストア ID](#store-ids)

<span id="video" />

### <a name="video"></a>ビデオ

[Windows.Services.Store](/uwp/api/windows.services.store) 名前空間を使用してアプリにアプリ内購入を実装する方法の概要については、次のビデオをご覧ください。
<br/>
<br/>
> [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/Adding-In-App-Purchases-to-Your-UWP-App/player]

<span id="get-started-storecontext" />

### <a name="get-started-with-the-storecontext-class"></a>StoreContext クラスの概要

**Windows.Services.Store** 名前空間のメイン エントリ ポイントは、[StoreContext](/uwp/api/windows.services.store.storecontext) クラスです。 このクラスには、現在のアプリとその利用可能なアドオンに関する情報の取得、現在のアプリまたはそのアドオンに関するライセンス情報の取得、現在のユーザー向けのアプリまたはアドオンの購入、およびその他のタスクを行う際に使用できるメソッドが用意されています。 [StoreContext](/uwp/api/windows.services.store.storecontext) オブジェクトを取得するには、次のいずれかを実行します。

* シングル ユーザー アプリ (アプリを起動したユーザーのコンテキストのみで実行されるアプリ) では、静的な [GetDefault](/uwp/api/windows.services.store.storecontext.getdefault) メソッドを使って **StoreContext** オブジェクトを取得します。このオブジェクトをうと、ユーザーの Microsoft Store 関連のデータにアクセスできます。 ほとんどのユニバーサル Windows プラットフォーム アプリ (UWP) アプリはシングル ユーザー アプリです。

  ```csharp
  Windows.Services.Store.StoreContext context = StoreContext.GetDefault();
  ```

* [マルチユーザー アプリ](../xbox-apps/multi-user-applications.md)では、静的な [GetForUser](/uwp/api/windows.services.store.storecontext.getforuser) メソッドを使って **StoreContext** オブジェクトを取得します。このオブジェクトを使うと、アプリの使用中に Microsoft アカウントでサインインしていた特定のユーザーに関する Microsoft Store 関連のデータにアクセスできます。 次の例は、利用可能な最初のユーザーの **StoreContext** オブジェクトを取得します。

  ```csharp
  var users = await Windows.System.User.FindAllAsync();
  Windows.Services.Store.StoreContext context = StoreContext.GetForUser(users[0]);
  ```

> [!NOTE]
> [デスクトップ ブリッジ](https://developer.microsoft.com/windows/bridges/desktop) を使用する Windows デスクトップ アプリケーションでは、[StoreContext](/uwp/api/windows.services.store.storecontext) オブジェクトを使用する前に、追加の手順を実行してこのオブジェクトを構成する必要があります。 詳細については、[こちらのセクション](#desktop)をご覧ください。

[StoreContext](/uwp/api/windows.services.store.storecontext) オブジェクトを構成すると、このオブジェクトのメソッドを呼び出して、現在のアプリやアドオンに関するストア製品情報の取得、現在のアプリとそのアドオンに関するライセンス情報の取得、現在のユーザー向けのアプリやアドオンの購入、およびその他のタスクを実行できます。 このオブジェクトを使用して実行できる一般的なタスクについて詳しくは、次の記事をご覧ください。

* [アプリとアドオンの製品情報の取得](get-product-info-for-apps-and-add-ons.md)
* [アプリとアドオンのライセンス情報の取得](get-license-info-for-apps-and-add-ons.md)
* [アプリとアドオンのアプリ内購入の有効化](enable-in-app-purchases-of-apps-and-add-ons.md)
* [コンシューマブルなアドオン購入の有効化](enable-consumable-add-on-purchases.md)
* [アプリのサブスクリプション アドオンの有効化](enable-subscription-add-ons-for-your-app.md)
* [アプリの試用版の実装](implement-a-trial-version-of-your-app.md)

**StoreContext** および **Windows.Services.Store** 名前空間にある他の型を使用する方法を示すサンプル アプリについては、[ストア サンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)をご覧ください。

<span id="implement-iap" />

### <a name="implement-in-app-purchases"></a>アプリ内購入の実装

**Windows.Services.Store** 名前空間を使用して、アプリでアプリ内購入をユーザーに提供するには:

1. お客様が購入できるアドオンがアプリに提供されている場合は、 [パートナーセンターでアプリのアドオン申請を作成 ](../publish/add-on-submissions.md)します。

2. [アプリで提供されるアプリやアドオンの製品情報を取得](get-product-info-for-apps-and-add-ons.md)してから、[ライセンスがアクティブになっているかどうかを判断](get-license-info-for-apps-and-add-ons.md)する (つまり、アプリやアドオンを使用するためのライセンスをユーザーが所有しているかどうかを判断する) ようにアプリのコードを記述します。 ライセンスがアクティブになっていない場合、ユーザーに販売するためのアプリやアドオンをアプリ内購入として提供する UI を表示します。

3. ユーザーがアプリやアドオンの購入を選んだ場合、製品を購入するための適切なメソッドを使用します。

    * ユーザーがアプリや永続的なアドオンを購入する場合は、「[アプリとアドオンのアプリ内購入の有効化](enable-in-app-purchases-of-apps-and-add-ons.md)」の指示に従ってください。
    * ユーザーがコンシューマブルなアドオンを購入する場合は、「[コンシューマブルなアドオン購入の有効化](enable-consumable-add-on-purchases.md)」指示に従ってください。
    * ユーザーがサブスクリプション アドオンを購入する場合は、「[アプリのサブスクリプション アドオンの有効化](enable-subscription-add-ons-for-your-app.md)」の指示に従ってください。

4. この記事に記載されている[テスト ガイダンス](#testing)に従って、実装をテストします。

<span id="implement-trial" />

### <a name="implement-trial-functionality"></a>試用版機能の実装

**Windows.Services.Store** 名前空間を使用して、アプリの試用版の機能を除外または制限するには:

1. [パートナーセンターで無料試用版としてアプリを構成](../publish/set-app-pricing-and-availability.md#free-trial)します。

2. [アプリで提供されるアプリやアドオンの製品情報を取得](get-product-info-for-apps-and-add-ons.md)してから、[アプリに関連付けられているライセンスが試用版ライセンスになっているかどうかを判断](get-license-info-for-apps-and-add-ons.md)するようにアプリのコードを記述します。

3. 試用版である場合は、アプリの特定の機能を除外または制限します。ユーザーが完全なライセンスを購入した場合は、その機能を有効にします。 詳細とコード例については、「[アプリの試用版の実装](implement-a-trial-version-of-your-app.md)」をご覧ください。

4. この記事に記載されている[テスト ガイダンス](#testing)に従って、実装をテストします。

<span id="testing" />

### <a name="test-your-in-app-purchase-or-trial-implementation"></a>アプリ内購入や試用版の実装のテスト

アプリで **Windows.Services.Store** 名前空間の API を使用してアプリ内購入や試用版機能を実装する場合は、アプリを Store に公開してからそのアプリを開発デバイスにダウンロードし、そのライセンスを使用してテストを行う必要があります。 コードをテストするには次のプロセスに従います。

1. アプリがまだ発行されておらず、ストアで使用できない場合は、アプリが [Windows アプリ認定キット](https://developer.microsoft.com/windows/develop/app-certification-kit) の最小要件を満たしていることを確認し、パートナーセンターでアプリを [送信](../publish/app-submissions.md) し、アプリが認定プロセスに合格したことを確認します。 テスト中に[ストアでアプリを検索できないようにアプリを構成する](../publish/set-app-pricing-and-availability.md)ことも可能です。 [パッケージフライト](../publish/package-flights.md)の適切な構成に注意してください。 正しく構成されていないパッケージフライトをダウンロードできない可能性があります。

2. 次に、以下の操作が完了していることを確認します。

    * [StoreContext](/uwp/api/windows.services.store.storecontext) クラス、および **Windows.Services.Store** 名前空間にある他の関連する型を使用するアプリのコードを、[アプリ内購入](#implement-iap)や[試用版機能](#implement-trial)を実装するように記述します。
    * お客様が購入できるアドオンがアプリに提供されている場合は、 [パートナーセンターでアプリのアドオンの送信を作成](../publish/add-on-submissions.md)します。
    * アプリの試用版の一部の機能を除外または制限する場合は、 [パートナーセンターで無料試用版としてアプリを構成](../publish/set-app-pricing-and-availability.md#free-trial)します。

3. プロジェクトを Visual Studio で開き、**[プロジェクト]** メニューをクリックし、**[ストア]** をポイントして、**[アプリケーションをストアと関連付ける]** をクリックします。 ウィザードの指示に従って、アプリケーションプロジェクトを、テストに使用するパートナーセンターアカウントのアプリに関連付けます。
    > [!NOTE]
    > プロジェクトとストアのアプリを関連付けていない場合、[StoreContext](/uwp/api/windows.services.store.storecontext) メソッドにより、戻り値の **ExtendedError** プロパティがエラー コード値 0x803F6107 に設定されます。 この値は、ストアにそのアプリに関する情報がないことを示します。
4. まだ前の手順で指定したストアのアプリをインストールしていない場合は、アプリをインストールし、アプリを一度実行してから閉じます。 これにより、アプリの有効なライセンスが開発用デバイスにインストールされます。

5. Visual Studio で、プロジェクトの実行またはデバッグを開始します。 コードによって、ローカルのプロジェクトと関連付けたストア アプリからアプリおよびアドオンのデータが取得されます。 アプリを再インストールするように求められた場合は、その指示に従った後、プロジェクトを実行またはデバッグします。
    > [!NOTE]
    > 上記の手順を実行すると、ストアに新しいアプリ パッケージを提出しなくても、引き続きアプリのコードを更新し、更新されたプロジェクトを開発用コンピューターでデバッグできます。 テストで使用するローカル ライセンスを取得するため、開発用コンピューターにストア バージョンのアプリを 1 度だけダウンロードする必要があります。 テストが完了し、ユーザーがアプリのアプリ内購入や試用版に関連する機能を利用できるようにするには、ストアに新しいアプリ パッケージを申請するだけで済みます。

アプリで **Windows.ApplicationModel.Store** 名前空間を使用している場合は、アプリで [CurrentAppSimulator](/uwp/api/Windows.ApplicationModel.Store.CurrentAppSimulator) クラスを使用することで、アプリをストアに提出する前に、ライセンス情報をテスト中にシミュレートできます。 詳細については、「 [CurrentApp クラスと Currentappシミュレータークラスの概要](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md#get-started-with-the-currentapp-and-currentappsimulator-classes)」を参照してください。  

> [!NOTE]
> **Windows.Services.Store** 名前空間には、テスト中にライセンス情報をシミュレートするために使用できるクラスが用意されていません。 **Windows.Services.Store** 名前空間を使用してアプリ内購入または試用版を実装する場合は、上記のように、アプリをストアに公開してからそのアプリを開発デバイスにダウンロードし、そのライセンスを使用してテストを行う必要があります。

<span id="receipts" />

### <a name="receipts-for-in-app-purchases"></a>アプリ内購入の受領通知

**Windows.Services.Store** 名前空間には、購入が成功した場合の取引の受領通知をアプリのコードで取得する際に使用できる API が用意されていません。 これは、**Windows.ApplicationModel.Store** 名前空間を使用するアプリの場合とは異なります。このようなアプリでは、[クライアント側の API を使用して、取引の受領通知を取得する](use-receipts-to-verify-product-purchases.md)ことができます。

**Windows.Services.Store** 名前空間を使ってアプリ内購入を実装しているとき、特定のユーザーがアプリまたはアドオンを購入したかどうかを確認する必要がある場合は、[Microsoft Store コレクション REST API](view-and-grant-products-from-a-service.md) の[製品の照会メソッド](query-for-products.md)を使用できます。 このメソッドで返されるデータによって、指定されたユーザーが特定の製品に対する資格を持っているかどうかを確認し、ユーザーが購入した製品の取引に関するデータを取得することができます。 Microsoft Store コレクション API では、Azure AD Authentication を使ってこの情報を取得します。

<span id="desktop" />

### <a name="using-the-storecontext-class-with-the-desktop-bridge"></a>デスクトップ ブリッジでの StoreContext クラスの使用

[Desktop Bridge](https://developer.microsoft.com/windows/bridges/desktop) を使用するデスクトップ アプリケーションでは、[StoreContext](/uwp/api/windows.services.store.storecontext) クラスを使用してアプリ内購入と試用版を実装できます。 ただし、Win32 デスクトップ アプリケーション、またはレンダリング フレームワーク (WPF アプリケーションなど) に関連付けられているウィンドウ ハンドル (HWND) を含んだデスクトップ アプリケーションがある場合、アプリケーションでは、オブジェクトによって表示されるモーダル ダイアログのオーナー ウィンドウとなるアプリケーション ウィンドウを指定するように、**StoreContext** オブジェクトを構成する必要があります。

多くの **StoreContext** メンバー (および **StoreContext** オブジェクトを介してアクセスされる他の関連する型のメンバー) では、製品購入などのストアに関連する操作を行うために、モーダル ダイアログがユーザーに表示されます。 デスクトップ アプリケーションで、モーダル ダイアログのオーナー ウィンドウを指定するように **StoreContext** オブジェクトが構成されていない場合、このオブジェクトは不正確なデータまたはエラーを返します。

デスクトップ ブリッジを使用するデスクトップ アプリケーションで **StoreContext** オブジェクトを構成するには、次の手順を実行します。

1. 次のいずれかを実行して、アプリで [IInitializeWithWindow](/windows/desktop/api/shobjidl_core/nn-shobjidl_core-iinitializewithwindow) インターフェイスにアクセスできるようにします。

    * アプリケーションが C# や Visual Basic などのマネージ言語で記述されている場合、次の C# の例に示すように、アプリのコードで [ComImport](/dotnet/api/system.runtime.interopservices.comimportattribute) 属性を使用して、**IInitializeWithWindow** インターフェイスを宣言します。 この例では、コード ファイルに **System.Runtime.InteropServices** 名前空間の **using** ステートメントが指定されていることを前提としています。

        ```csharp
        [ComImport]
        [Guid("3E68D4BD-7135-4D10-8018-9FB6D9F33FA1")]
        [InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
        public interface IInitializeWithWindow
        {
            void Initialize(IntPtr hwnd);
        }
        ```

    * アプリケーションが C++ で記述されている場合、コードに shobjidl.h ヘッダー ファイルへの参照を追加します。 このヘッダー ファイルには、**IInitializeWithWindow** インターフェイスの宣言が含まれています。

2. この記事で既に説明したように、[GetDefault](/uwp/api/windows.services.store.storecontext.getdefault) メソッド ([マルチ ユーザー アプリ](../xbox-apps/multi-user-applications.md)の場合は [GetForUser](/uwp/api/windows.services.store.storecontext.getforuser) メソッド) を使用して、[StoreContext](/uwp/api/windows.services.store.storecontext) オブジェクトを取得します。次に、このオブジェクトを [IInitializeWithWindow](/windows/desktop/api/shobjidl_core/nn-shobjidl_core-iinitializewithwindow) オブジェクトにキャストします。 その後で、[IInitializeWithWindow.Initialize](/windows/desktop/api/shobjidl_core/nf-shobjidl_core-iinitializewithwindow-initialize) メソッドを呼び出し、**StoreContext** メソッドによって表示されるモーダル ダイアログのオーナーにするウィンドウのハンドルを渡します。 次の C# の例は、アプリのメイン ウィンドウのハンドルをメソッドに渡す方法を示しています。
    ```csharp
    StoreContext context = StoreContext.GetDefault();
    IInitializeWithWindow initWindow = (IInitializeWithWindow)(object)context;
    initWindow.Initialize(System.Diagnostics.Process.GetCurrentProcess().MainWindowHandle);
    ```

<span id="products-skus" />

### <a name="products-skus-and-availabilities"></a>製品、SKU、および可用性

ストア内のすべての製品には少なくとも 1 つの *SKU* があり、各 SKU ごとに少なくとも 1 つの*可用性*があります。 これらの概念は、パートナーセンターのほとんどの開発者から抽象化されており、ほとんどの開発者は、アプリやアドオンの Sku や可用性を定義することはありません。 ただし、**Windows.Services.Store** 名前空間のストア製品のオブジェクト モデルには SKU と可用性が含まれているため、シナリオによっては、これらの概念の基本的な知識があると役に立つことがあります。

| Object |  説明  |
|---------|-------------------|
| 製品  |  *製品*は、アプリやアドオンなど、ストアで利用できるすべての製品の種類を指します。 <p/><p/> ストアの各製品には対応する [StoreProduct](/uwp/api/windows.services.store.storeproduct) オブジェクトがあります。 このクラスには、製品のストア ID、ストア登録情報の画像と動画、価格情報などのデータにアクセスするために使用できるプロパティが用意されています。 また、製品を購入するために使用できるメソッドも提供されます。 |
| SKU |  *SKU*は、製品の特定のバージョンであり、独自の説明、価格、およびその他の固有の製品詳細が含まれています。 各アプリまたはアドオンには既定の SKU があります。 ほとんどの開発者は、アプリの通常版と試用版を公開している場合を除いて (ストア カタログでは、通常版と試用版は同じアプリの別の SKU になります)、1 つのアプリに複数の SKU を用意することはありません。 <p/><p/> 一部の発行元は、独自の SKU を定義することができます。 たとえば、大規模なゲームの発行元の場合、赤い血が許可されない市場では緑の血が表示される SKU でゲームをリリースし、それ以外のすべての市場では赤い血が表示される別の SKU でゲームをリリースすることがあります。 または、デジタル ビデオ コンテンツを販売している発行元が、1 つのビデオに対して、高解像度版の SKU と通常解像度版の SKU という、2 つの SKU を公開する場合もあります。 <p/><p/> ストアの各 SKU には対応する [StoreSku](/uwp/api/windows.services.store.storesku) オブジェクトがあります。 すべての [StoreProduct](/uwp/api/windows.services.store.storeproduct) には、製品の SKU にアクセスするために使用できる [Skus](/uwp/api/windows.services.store.storeproduct.skus) プロパティがあります。 |
| 可用性  |  *可用性*とは、固有の価格情報を持つ SKU の特定のバージョンです。 各 SKU には、既定の可用性があります。 一部の発行元は独自の可用性を定義でき、特定の SKU にさまざまな価格オプションを導入できます。 <p/><p/> ストアの各可用性には対応する [StoreAvailability](/uwp/api/windows.services.store.storeavailability) オブジェクトがあります。 すべての [StoreSku](/uwp/api/windows.services.store.storesku) には、SKU の可用性にアクセスするために使用できる [Availabilities](/uwp/api/windows.services.store.storesku.availabilities) プロパティがあります。 ほとんどの開発者の場合、SKU ごとに 1 つの既定の可用性があります。  |

<span id="store_ids" />

### <a name="store-ids"></a>ストア ID

アプリやアドオンなど、ストアのすべての製品には関連付けられた**ストア ID** があります (これは*製品ストア ID* と呼ばれることもあります)。 多くの API は、アプリまたはアドオンで操作を実行するためにストア ID を必要とします。

ストア内の製品のストア ID は 12 文字の英数字文字列です。たとえば、```9NBLGGH4R315``` のようになります。 ストア内の製品のストア ID は、次のようにさまざまな方法で取得できます。

* アプリの場合、パートナーセンターの [ [アプリ id] ページ](../publish/view-app-identity-details.md) でストア ID を取得できます。
* アドオンの場合は、パートナーセンターのアドオンの [概要] ページでストア ID を取得できます。
* 製品の場合、その製品を表す [StoreProduct](/uwp/api/windows.services.store.storeproduct) オブジェクトの [StoreId](/uwp/api/windows.services.store.storeproduct.storeid) プロパティを使用するにより、ストア ID をプログラムで取得することもできます。

製品の SKU と可用性の場合は、その SKU と可用性にも形式の異なる固有のストア ID が用意されています。

| オブジェクト |  ストア ID の形式  |
|---------|-------------------|
| SKU |  SKU の場合、ストア ID の形式は ```<product Store ID>/xxxx``` のようになります。ここで、```xxxx``` は製品の SKU を識別する 4 文字の英数字文字列です。 たとえば、「 ```9NBLGGH4R315/000N``` 」のように入力します。 この ID は、[StoreSku](/uwp/api/windows.services.store.storesku) オブジェクトの [StoreId](/uwp/api/windows.services.store.storesku.storeid) プロパティによって返され、*SKU ストア ID* と呼ばれることもあります。 |
| 可用性  |  可用性の場合、ストア ID の形式は ```<product Store ID>/xxxx/yyyyyyyyyyyy``` のようになります。ここで、```xxxx``` は製品の SKU を識別する 4 文字の英数字文字列で、```yyyyyyyyyyyy``` は SKU の可用性を識別する 12 文字の英数字文字列です。 たとえば、「 ```9NBLGGH4R315/000N/4KW6QZD2VN6X``` 」のように入力します。 この ID は、[StoreAvailability](/uwp/api/windows.services.store.storeavailability) オブジェクトの [StoreId](/uwp/api/windows.services.store.storeavailability.storeid) プロパティによって返され、*可用性ストア ID* と呼ばれることもあります。  |

<span id="product-ids" />

## <a name="how-to-use-product-ids-for-add-ons-in-your-code"></a>コードでアドオンの製品 ID を使用する方法

アプリのコンテキストで顧客がアドオンを利用できるようにするには、パートナーセンターで[アドオンの送信を作成](../publish/add-on-submissions.md)するときに、アドオンの一意の[製品 ID を入力](../publish/set-your-add-on-product-id.md#product-id)する必要があります。 この製品 ID を使用することでコード内でアドオンを参照できます。ただし、製品 ID を使用できる特定のシナリオは、アプリのアプリ内購入に使用する名前空間によって異なります。

> [!NOTE]
> アドオンのパートナーセンターで入力する製品 ID は、アドオンの [ストア ID](#store-ids)とは異なります。 ストア ID は、パートナーセンターによって生成されます。

### <a name="apps-that-use-the-windowsservicesstore-namespace"></a>Windows.Services.Store 名前空間を使うアプリ

アプリで **Windows.Services.Store** 名前空間を使用している場合、製品 ID を使用することで、アドオンを表す [StoreProduct](/uwp/api/Windows.Services.Store.StoreProduct) や、アドオンのライセンスを表す [StoreLicense](/uwp/api/windows.services.store.storelicense) を簡単に識別できます。 製品 ID は、[StoreProduct.InAppOfferToken](/uwp/api/Windows.Services.Store.StoreProduct.InAppOfferToken) プロパティおよび [StoreLicense.InAppOfferToken](/uwp/api/windows.services.store.storelicense.InAppOfferToken) プロパティにより公開されます。

> [!NOTE]
> 製品 ID はコードでアドオンを識別する便利な方法ですが、**Windows.Services.Store** 名前空間のほとんどの操作では、製品 ID の代わりにアドオンの[ストア ID](#store-ids) を使用します。 たとえば、アプリの 1 つ以上の既知のアドオンをプログラムで取得するには、アドオンの (製品 ID ではなく) ストア ID を [GetStoreProductsAsync](/uwp/api/windows.services.store.storecontext.getstoreproductsasync) メソッドに渡します。 同様に、コンシューマブルなアドオンをフルフィルメント完了として報告するには、アドオンの (製品 ID ではなく) ストア ID を [ReportConsumableFulfillmentAsync](/uwp/api/windows.services.store.storecontext.reportconsumablefulfillmentasync) メソッドに渡します。

### <a name="apps-that-use-the-windowsapplicationmodelstore-namespace"></a>Windows.ApplicationModel.Store 名前空間を使うアプリ

アプリで **Windows. ApplicationModel. Store** 名前空間を使用する場合は、ほとんどの操作で、パートナーセンターのアドオンに割り当てる製品 ID を使用する必要があります。 次に例を示します。

* アドオンを表す [ProductListing](/uwp/api/windows.applicationmodel.store.productlisting) や、アドオンのライセンスを表す [ProductLicense](/uwp/api/windows.applicationmodel.store.productlicense) を識別するには、製品 ID を使用します。 製品 ID は、[ProductListing.ProductId](/uwp/api/windows.applicationmodel.store.productlisting.ProductId) プロパティおよび [ProductLicense.ProductId](/uwp/api/windows.applicationmodel.store.productlicense.ProductId) プロパティにより公開されます。

* ユーザーのアドオンを購入する場合は [RequestProductPurchaseAsync](/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync) メソッドを使用して製品 ID を指定します。 詳しくは、「[アプリ内製品購入の有効化](enable-in-app-product-purchases.md)」をご覧ください。

* コンシューマブルなアドオンをフルフィルメント完了として報告する場合は [ReportConsumableFulfillmentAsync](/uwp/api/windows.applicationmodel.store.currentapp.reportconsumablefulfillmentasync) メソッドを使用して製品 ID を指定します。 詳しくは、「[コンシューマブルなアプリ内製品購入の有効化](enable-consumable-in-app-product-purchases.md)」をご覧ください。

## <a name="related-topics"></a>関連トピック

* [アプリとアドオンの製品情報の取得](get-product-info-for-apps-and-add-ons.md)
* [アプリとアドオンのライセンス情報の取得](get-license-info-for-apps-and-add-ons.md)
* [アプリとアドオンのアプリ内購入の有効化](enable-in-app-purchases-of-apps-and-add-ons.md)
* [コンシューマブルなアドオン購入の有効化](enable-consumable-add-on-purchases.md)
* [アプリのサブスクリプション アドオンの有効化](enable-subscription-add-ons-for-your-app.md)
* [アプリの試用版の実装](implement-a-trial-version-of-your-app.md)
* [Microsoft Store の操作のエラー コード](error-codes-for-store-operations.md)
* [Windows.ApplicationModel.Store 名前空間を使用するアプリ内購入と試用版](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md)