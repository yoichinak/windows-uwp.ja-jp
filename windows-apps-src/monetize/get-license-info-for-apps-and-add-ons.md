---
ms.assetid: 9630AF6D-6887-4BE3-A3CB-D058F275B58F
description: Windows.Services.Store 名前空間を使って、現在のアプリとそのアドオン ライセンス情報を取得する方法について説明します。
title: アプリとアドオンのライセンス情報の取得
ms.date: 12/04/2017
ms.topic: article
keywords: Windows 10, UWP, ライセンス, アプリ, アドオン, アプリ内購入, IAP, Windows.Services.Store
ms.localizationpriority: medium
ms.openlocfilehash: 29f2e5ceee6d7d9c779c7fb544d507e31456b3fd
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363165"
---
# <a name="get-license-info-for-apps-and-add-ons"></a>アプリとアドオンのライセンス情報の取得

この記事では、[Windows.Services.Store](/uwp/api/windows.services.store) 名前空間の [StoreContext](/uwp/api/windows.services.store.storecontext) クラスのメソッドを使って、現在のアプリとそのアドオンのライセンス情報を取得する方法について説明します。 たとえば、この情報を使って、アプリまたはそのアドオンのライセンスがアクティブになっているかどうかや、試用ライセンスであるかどうかなどを確認できます。

> [!NOTE]
> **Windows.Services.Store** 名前空間は、Windows 10 バージョン 1607 で導入され、Visual Studio で、**Windows 10 Anniversary Edition (10.0、ビルド 14393)** 以降のリリースをターゲットとするプロジェクトでのみ使用できます。 アプリが Windows 10 の以前のバージョンをターゲットする場合、**Windows.Services.Store** 名前空間の代わりに [Windows.ApplicationModel.Store](/uwp/api/windows.applicationmodel.store) 名前空間を使う必要があります。 詳細については、 [こちらの記事](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md)を参照してください。

## <a name="prerequisites"></a>前提条件

この例には、次の前提条件があります。
* **Windows 10 Anniversary Edition (10.0、ビルド 14393)** 以降のリリースをターゲットとするユニバーサル Windows プラットフォーム (UWP) アプリの Visual Studio プロジェクト。
* パートナーセンターで [アプリの送信を作成](../publish/app-submissions.md) し、このアプリをストアに公開しました。 必要に応じで、テスト中にストアでアプリを検索できないようにアプリを構成することも可能です。 詳しくは、[テスト ガイダンス](in-app-purchases-and-trials.md#testing)をご覧ください。
* アプリのアドオンのライセンス情報を取得する必要がある場合は、 [パートナーセンターでアドオンも作成](../publish/add-on-submissions.md)する必要があります。

この例のコードは、次の点を前提としています。
* コードは、```workingProgressRing``` という名前の [ProgressRing](/uwp/api/windows.ui.xaml.controls.progressring) と ```textBlock``` という名前の [TextBlock](/uwp/api/windows.ui.xaml.controls.textblock) を含む [Page](/uwp/api/windows.ui.xaml.controls.page) のコンテキストで実行されます。 これらのオブジェクトは、それぞれ非同期操作が発生していることを示するためと、出力メッセージを表示するために使用されます。
* コード ファイルには、**Windows.Services.Store** 名前空間の **using** ステートメントがあります。
* アプリは、アプリを起動したユーザーのコンテキストでのみ動作するシングル ユーザー アプリです。 詳しくは、「[アプリ内購入と試用版](in-app-purchases-and-trials.md#api_intro)」をご覧ください。

> [!NOTE]
> [デスクトップ ブリッジ](https://developer.microsoft.com/windows/bridges/desktop)を使用するデスクトップ アプリケーションがある場合、この例には示されていないコードを追加して [StoreContext ](/uwp/api/windows.services.store.storecontext) オブジェクトを構成することが必要になることがあります。 詳しくは、「[デスクトップ ブリッジを使用するデスクトップ アプリケーションでの StoreContext クラスの使用](in-app-purchases-and-trials.md#desktop)」をご覧ください。

## <a name="code-example"></a>コードの例

現在のアプリのライセンス情報を取得するには、[GetAppLicenseAsync](/uwp/api/windows.services.store.storecontext.getapplicenseasync) メソッドを使います。 これは、アプリのライセンス情報を提供する [StoreAppLicense](/uwp/api/windows.services.store.storeapplicense) オブジェクトを返す非同期メソッドです。この情報には、アプリを使用するための有効なライセンスをユーザーが現在所有しているかどうかを示すプロパティ ([IsActive](/uwp/api/windows.services.store.storeapplicense.isactive)) や、ライセンスが試用版向けであるかどうかを示すプロパティ ([IsTrial](/uwp/api/windows.services.store.storeapplicense.istrial)) などが含まれます。

ユーザーが使用する権利を持っている現在のアプリの永続的なアドオンのライセンスにアクセスするには、[StoreAppLicense](/uwp/api/windows.services.store.storeapplicense) オブジェクトの [AddOnLicenses](/uwp/api/windows.services.store.storeapplicense.addonlicenses) プロパティを使います。 このプロパティは、アドオンのライセンスを表す [StoreLicense](/uwp/api/windows.services.store.storelicense) オブジェクトのコレクションを返します。

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/InAppPurchasesAndLicenses_RS1/cs/GetLicenseInfoPage.xaml.cs" id="GetLicenseInfo":::

完全なサンプル アプリケーションについては、[ストア サンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)をご覧ください。

## <a name="related-topics"></a>関連トピック

* [アプリ内購入と試用版](in-app-purchases-and-trials.md)
* [アプリとアドオンの製品情報の取得](get-product-info-for-apps-and-add-ons.md)
* [アプリとアドオンのアプリ内購入の有効化](enable-in-app-purchases-of-apps-and-add-ons.md)
* [コンシューマブルなアドオン購入の有効化](enable-consumable-add-on-purchases.md)
* [アプリの試用版の実装](implement-a-trial-version-of-your-app.md)
* [ストア サンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)
