---
description: ストア製品向けに Windows.Services.Store 名前空間の拡張 JSON データ スキーマについて説明します。
title: Store 製品のデータ スキーマ
ms.date: 09/26/2017
ms.topic: article
keywords: Windows 10, UWP, ExtendedJsonData, Store 製品, スキーマ
ms.localizationpriority: medium
ms.openlocfilehash: e09e02d12afc436f5d22d11fad85fb0bad3507f6
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363185"
---
# <a name="data-schemas-for-store-products"></a>Store 製品のデータ スキーマ

アプリやアドオンなどの製品をストアに提出すると、製品とそのライセンスの包括的なデータ セットがストアで管理されます。 アプリのコードで [Windows.Services.Store](/uwp/api/windows.services.store) 名前空間のプロパティを使用することで、こうしたデータの一部にプログラムによってアクセスできます。 たとえば、[StoreProduct.Description](/uwp/api/windows.services.store.storeproduct.Description) プロパティと [StoreProduct.Price](/uwp/api/windows.services.store.storeproduct.Price) プロパティを使用すると、現在のアプリや、現在のアプリのアドオンに関する説明と価格を取得できます。

ただし、ストア内の製品に関する多くのデータは、[Windows.Services.Store](/uwp/api/windows.services.store) 名前空間の定義済みプロパティを使用することでは公開されません。 コードで製品の完全なデータにアクセスするには、代わりに、次の全般的なプロパティを使用できます。

* [StoreProduct.ExtendedJsonData](/uwp/api/windows.services.store.storeproduct.ExtendedJsonData)
* [StoreSku.ExtendedJsonData](/uwp/api/windows.services.store.storesku.ExtendedJsonData)
* [StoreAvailability.ExtendedJsonData](/uwp/api/windows.services.store.storeavailability.ExtendedJsonData)
*   [StoreCollectionData.ExtendedJsonData](/uwp/api/windows.services.store.storecollectiondata.ExtendedJsonData)
*   [StoreAppLicense.ExtendedJsonData](/uwp/api/windows.services.store.storeapplicense.ExtendedJsonData)
* [StoreLicense.ExtendedJsonData](/uwp/api/windows.services.store.storelicense.ExtendedJsonData)
*   [StorePurchaseProperties.ExtendedJsonData](/uwp/api/windows.services.store.storepurchaseproperties.ExtendedJsonData)

これらのプロパティは、対応するオブジェクトの完全なデータを JSON 形式の文字列として返します。 この記事では、これらのプロパティによって返されるデータの完全なスキーマを提供します。

> [!NOTE]
> ストア内の製品は、[製品](/uwp/api/windows.services.store.storeproduct)オブジェクト、[SKU](/uwp/api/windows.services.store.storesku) オブジェクト、および[可用性](/uwp/api/windows.services.store.storeavailability)オブジェクトの階層で整理されます。 詳細については、「[製品、SKU、および可用性](in-app-purchases-and-trials.md#products-skus)」をご覧ください。

## <a name="schema-for-storeproduct-storesku-storeavailability-and-storecollectiondata"></a>StoreProduct、StoreSku、StoreAvailability、および StoreCollectionData のスキーマ

次のスキーマは、[StoreProduct.ExtendedJsonData](/uwp/api/windows.services.store.storeproduct.ExtendedJsonData) によって返される JSON 形式の文字列を示しています。 [StoreSku.ExtendedJsonData](/uwp/api/windows.services.store.storesku.ExtendedJsonData) プロパティ、[StoreAvailability.ExtendedJsonData](/uwp/api/windows.services.store.storeavailability.ExtendedJsonData) プロパティ、および [StoreCollectionData.ExtendedJsonData](/uwp/api/windows.services.store.storecollectiondata.ExtendedJsonData) プロパティは、`DisplaySkuAvailabilities\Sku`、`DisplaySkuAvailabilities\Availabilities`、および `DisplaySkuAvailabilities\Sku\CollectionData` のパスの下でそれぞれ定義されているスキーマの部分だけを返します。

[StoreProduct.ExtendedJsonData](/uwp/api/windows.services.store.storeproduct.ExtendedJsonData) によって返される JSON 形式の文字列の例については、[このセクション](#product-example)をご覧ください。

:::code language="json" source="~/../snippets-windows/windows-uwp/monetize/InAppPurchasesAndLicenses_RS1/json/StoreProduct.ExtendedJsonData.json" range="1-729":::

<span id="product-example" />

### <a name="example"></a>例

次の例は、アプリの [StoreProduct.ExtendedJsonData](/uwp/api/windows.services.store.storeproduct.ExtendedJsonData) プロパティによって返される JSON 形式の文字列を示しています。

:::code language="json" source="~/../snippets-windows/windows-uwp/monetize/InAppPurchasesAndLicenses_RS1/json/StoreProduct.ExtendedJsonDataExample.json" range="1-268":::

## <a name="schema-for-storeapplicense-and-storelicense"></a>StoreAppLicense と StoreLicense のスキーマ

次のスキーマは、[StoreAppLicense.ExtendedJsonData](/uwp/api/windows.services.store.storeapplicense.ExtendedJsonData) によって返される JSON 形式の文字列を示しています。 [StoreLicense.ExtendedJsonData](/uwp/api/windows.services.store.storelicense.ExtendedJsonData) プロパティは、`productAddOns` パスの下で定義されているスキーマの部分だけを返します。

[StoreAppLicense.ExtendedJsonData](/uwp/api/windows.services.store.storeapplicense.ExtendedJsonData) によって返される JSON 形式の文字列の例については、[このセクション](#license-example)をご覧ください。

:::code language="json" source="~/../snippets-windows/windows-uwp/monetize/InAppPurchasesAndLicenses_RS1/json/StoreAppLicense.ExtendedJsonData.json" range="1-80":::

<span id="license-example" />

### <a name="example"></a>例

次の例は、アプリの [StoreAppLicense.ExtendedJsonData](/uwp/api/windows.services.store.storeapplicense.ExtendedJsonData) プロパティによって返される JSON 形式の文字列を示しています。

:::code language="json" source="~/../snippets-windows/windows-uwp/monetize/InAppPurchasesAndLicenses_RS1/json/StoreAppLicense.ExtendedJsonDataExample.json" range="1-28":::

## <a name="schema-for-storepurchaseproperties"></a>StorePurchaseProperties のスキーマ

次のスキーマは、[StorePurchaseProperties.ExtendedJsonData](/uwp/api/windows.services.store.storepurchaseproperties.ExtendedJsonData) によって返される JSON 形式の文字列を示しています。

:::code language="json" source="~/../snippets-windows/windows-uwp/monetize/InAppPurchasesAndLicenses_RS1/json/StorePurchaseProperties.ExtendedJsonData.json" range="1-12":::

## <a name="related-topics"></a>関連トピック

* [アプリ内購入と試用版](in-app-purchases-and-trials.md)
* [アプリとアドオンの製品情報の取得](get-product-info-for-apps-and-add-ons.md)
* [アプリとアドオンのライセンス情報の取得](get-license-info-for-apps-and-add-ons.md)
* [アプリとアドオンのアプリ内購入の有効化](enable-in-app-purchases-of-apps-and-add-ons.md)
* [コンシューマブルなアドオン購入の有効化](enable-consumable-add-on-purchases.md)
* [アプリの試用版の実装](implement-a-trial-version-of-your-app.md)
