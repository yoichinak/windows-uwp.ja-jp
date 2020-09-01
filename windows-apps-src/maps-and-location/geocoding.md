---
title: ジオコーディングと逆ジオコーディングの実行
description: このガイドでは、ジオコーディング名前空間の MapLocationFinder クラスのメソッドを呼び出すことによって、番地を地理的な場所 (ジオコーディング) に変換し、地理的な場所を番地 (reverse) に変換する方法について説明します。
ms.assetid: B912BE80-3E1D-43BB-918F-7A43327597D2
ms.date: 07/02/2018
ms.topic: article
keywords: Windows 10, UWP, ジオコーディング, 地図, 位置情報
ms.localizationpriority: medium
ms.openlocfilehash: 011a901e2baa9ff4b8f4a5bd8018b9b7790b1852
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162566"
---
# <a name="perform-geocoding-and-reverse-geocoding"></a>ジオコーディングと逆ジオコーディングの実行

このガイドでは、ジオコーディング名前空間の [**MapLocationFinder**](/uwp/api/Windows.Services.Maps.MapLocationFinder) クラスのメソッドを呼び出すことによっ [**て、番地**](/uwp/api/Windows.Services.Maps) を地理的な場所 (ジオコーディング) に変換し、地理的な場所を番地 (reverse) に変換する方法について説明します。

> [!TIP]
> アプリでマップを使用する方法の詳細については、GitHub の[Windows ユニバーサルサンプルリポジトリ](hhttps://github.com/Microsoft/Windows-universal-samples)から[mapcontrol](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)サンプルをダウンロードしてください。

ジオコーディングと reverse ジオコーディングに関連するクラスは、次のように構成されています。

-   [**MapLocationFinder**](/uwp/api/Windows.Services.Maps.MapLocationFinder)クラスには、ジオコーディング ([**FindLocationsAsync**](/uwp/api/windows.services.maps.maplocationfinder.findlocationsasync)) と Reverse ジオコーディング ([**findlocationsatasync**](/uwp/api/windows.services.maps.maplocationfinder.findlocationsatasync)) を処理するメソッドが含まれています。
-   これらのメソッドはどちらも、 [**MapLocationFinderResult**](/uwp/api/Windows.Services.Maps.MapLocationFinderResult) インスタンスを返します。
-   [**MapLocationFinderResult**](/uwp/api/Windows.Services.Maps.MapLocationFinderResult)の "[**場所**](/uwp/api/windows.services.maps.maplocationfinderresult.locations)" プロパティは、 [**MapLocation**](/uwp/api/Windows.Services.Maps.MapLocation)オブジェクトのコレクションを公開します。 
-   [**MapLocation**](/uwp/api/Windows.Services.Maps.MapLocation)オブジェクトには、[**住所プロパティと**](/uwp/api/windows.services.maps.maplocation.address)して、住所を表す[**mapaddress**](/uwp/api/Windows.Services.Maps.MapAddress)オブジェクトと、地理的な場所を表す[**geopoint**](/uwp/api/windows.devices.geolocation.geopoint)オブジェクトを公開する[**ポイント**](/uwp/api/windows.services.maps.maplocation.point)プロパティの両方があります。

> [!IMPORTANT]
> マップ サービスを使用する前に、マップ認証キーを指定する必要があります。 詳しくは、「[マップ認証キーの要求](authentication-key.md)」をご覧ください。

## <a name="get-a-location-geocode"></a>位置情報の取得 (ジオコーディング)

このセクションでは、住所または地名を地理的な場所 (ジオコーディング) に変換する方法について説明します。

1.  [**MapLocationFinder**](/uwp/api/Windows.Services.Maps.MapLocationFinder)クラスの[**FindLocationsAsync**](/uwp/api/windows.services.maps.maplocationfinder.findlocationsasync)メソッドのいずれかのオーバーロードを、プレース名または住所を使用して呼び出します。
2.  [**FindLocationsAsync**](/uwp/api/windows.services.maps.maplocationfinder.findlocationsasync)メソッドは、 [**MapLocationFinderResult**](/uwp/api/Windows.Services.Maps.MapLocationFinderResult)オブジェクトを返します。
3.  コレクション[**MapLocation**](/uwp/api/Windows.Services.Maps.MapLocation)オブジェクトを公開するには、 [**MapLocationFinderResult**](/uwp/api/Windows.Services.Maps.MapLocationFinderResult)の "[**場所**](/uwp/api/windows.services.maps.maplocationfinderresult.locations)" プロパティを使用します。 指定された入力に対応する複数の場所がシステムによって検出される可能性があるため、複数の [**MapLocation**](/uwp/api/Windows.Services.Maps.MapLocation) オブジェクトが存在する可能性があります。

```csharp
using Windows.Services.Maps;
using Windows.Devices.Geolocation;
...
private async void geocodeButton_Click(object sender, RoutedEventArgs e)
{
   // The address or business to geocode.
   string addressToGeocode = "Microsoft";

   // The nearby location to use as a query hint.
   BasicGeoposition queryHint = new BasicGeoposition();
   queryHint.Latitude = 47.643;
   queryHint.Longitude = -122.131;
   Geopoint hintPoint = new Geopoint(queryHint);

   // Geocode the specified address, using the specified reference point
   // as a query hint. Return no more than 3 results.
   MapLocationFinderResult result =
         await MapLocationFinder.FindLocationsAsync(
                           addressToGeocode,
                           hintPoint,
                           3);

   // If the query returns results, display the coordinates
   // of the first result.
   if (result.Status == MapLocationFinderStatus.Success)
   {
      tbOutputText.Text = "result = (" +
            result.Locations[0].Point.Position.Latitude.ToString() + "," +
            result.Locations[0].Point.Position.Longitude.ToString() + ")";
   }
}
```

このコードでは、`tbOutputText` テキスト ボックスに次の結果が表示されます。

``` syntax
result = (47.6406099647284,-122.129339994863)
```

## <a name="get-an-address-reverse-geocode"></a>住所の取得 (逆ジオコーディング)

このセクションでは、地理的な場所を住所に変換する方法 (reverse ジオコーディング) について説明します。

1.  [**MapLocationFinder**](/uwp/api/Windows.Services.Maps.MapLocationFinder) クラスの [**FindLocationsAtAsync**](/uwp/api/windows.services.maps.maplocationfinder.findlocationsatasync) メソッドを呼び出します。
2.  [**FindLocationsAtAsync**](/uwp/api/windows.services.maps.maplocationfinder.findlocationsatasync) メソッドは、一致する [**MapLocation**](/uwp/api/Windows.Services.Maps.MapLocation) オブジェクトのコレクションを含む [**MapLocationFinderResult**](/uwp/api/Windows.Services.Maps.MapLocationFinderResult) オブジェクトを返します。
3.  コレクション[**MapLocation**](/uwp/api/Windows.Services.Maps.MapLocation)オブジェクトを公開するには、 [**MapLocationFinderResult**](/uwp/api/Windows.Services.Maps.MapLocationFinderResult)の "[**場所**](/uwp/api/windows.services.maps.maplocationfinderresult.locations)" プロパティを使用します。 指定された入力に対応する複数の場所がシステムによって検出される可能性があるため、複数の [**MapLocation**](/uwp/api/Windows.Services.Maps.MapLocation) オブジェクトが存在する可能性があります。
4.  各[**MapLocation**](/uwp/api/Windows.Services.Maps.MapLocation)の[**address**](/uwp/api/windows.services.maps.maplocation.address)プロパティを使用して[**mapaddress**](/uwp/api/Windows.Services.Maps.MapAddress)オブジェクトにアクセスします。

```csharp
using Windows.Services.Maps;
using Windows.Devices.Geolocation;
...
private async void reverseGeocodeButton_Click(object sender, RoutedEventArgs e)
{
   // The location to reverse geocode.
   BasicGeoposition location = new BasicGeoposition();
   location.Latitude = 47.643;
   location.Longitude = -122.131;
   Geopoint pointToReverseGeocode = new Geopoint(location);

   // Reverse geocode the specified geographic location.
   MapLocationFinderResult result =
         await MapLocationFinder.FindLocationsAtAsync(pointToReverseGeocode);

   // If the query returns results, display the name of the town
   // contained in the address of the first result.
   if (result.Status == MapLocationFinderStatus.Success)
   {
      tbOutputText.Text = "town = " +
            result.Locations[0].Address.Town;
   }
}
```

このコードでは、`tbOutputText` テキスト ボックスに次の結果が表示されます。

``` syntax
town = Redmond
```

## <a name="related-topics"></a>関連トピック

* [UWP の地図のサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)
* [UWP の交通情報アプリのサンプル](https://github.com/Microsoft/Windows-appsample-trafficapp)
* [地図の設計ガイドライン](./display-maps.md)
* [ビデオ: Windows アプリでの電話、タブレット、および PC でのマップと場所の活用](https://channel9.msdn.com/Events/Build/2015/2-757)
* [Bing Maps Developer Center](https://www.bingmapsportal.com/)
* [**MapLocationFinder** クラス](/uwp/api/Windows.Services.Maps.MapLocationFinder)
* [**FindLocationsAsync** メソッド](/uwp/api/windows.services.maps.maplocationfinder.findlocationsasync)
* [**Findlocationsatasync** メソッド](/uwp/api/windows.services.maps.maplocationfinder.findlocationsatasync)