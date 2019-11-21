---
title: ジオコーディングと逆ジオコーディングの実行
description: このガイドでは、ジオコーディング名前空間の MapLocationFinder クラスのメソッドを呼び出すことによって、番地を地理的な場所 (ジオコーディング) に変換し、地理的な場所を番地 (reverse) に変換する方法について説明します。
ms.assetid: B912BE80-3E1D-43BB-918F-7A43327597D2
ms.date: 07/02/2018
ms.topic: article
keywords: Windows 10, UWP, ジオコーディング, 地図, 位置情報
ms.localizationpriority: medium
ms.openlocfilehash: 5d7e1dda355cf87a2c8e26c11327cfff32e9d0b5
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259371"
---
# <a name="perform-geocoding-and-reverse-geocoding"></a>ジオコーディングと逆ジオコーディングの実行

このガイドでは、ジオコーディング名前空間の[**MapLocationFinder**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinder)クラスのメソッドを呼び出すことによっ[**て、番地**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps)を地理的な場所 (ジオコーディング) に変換し、地理的な場所を番地 (reverse) に変換する方法について説明します。

> [!TIP]
> アプリでマップを使用する方法の詳細については、GitHub の[Windows ユニバーサルサンプルリポジトリ](h https://github.com/Microsoft/Windows-universal-samples)から[mapcontrol](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)サンプルをダウンロードしてください。

ジオコーディングと reverse ジオコーディングに関連するクラスは、次のように構成されています。

-   [**MapLocationFinder**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinder)クラスには、ジオコーディング ([**FindLocationsAsync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsasync)) と Reverse ジオコーディング ([**findlocationsatasync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsatasync)) を処理するメソッドが含まれています。
-   これらのメソッドはどちらも、 [**MapLocationFinderResult**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinderResult)インスタンスを返します。
-   [**MapLocationFinderResult**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinderResult)の "[**場所**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinderresult.locations)" プロパティは、 [**MapLocation**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation)オブジェクトのコレクションを公開します。 
-   [**MapLocation**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation)オブジェクトには、[**住所プロパティと**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocation.address)して、住所を表す[**mapaddress**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapAddress)オブジェクトと、地理的な場所を表す[**geopoint**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geopoint)オブジェクトを公開する[**ポイント**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocation.point)プロパティの両方があります。

> [!IMPORTANT]
> マップサービスを使用する前に、マップ認証キーを指定する必要があります。 詳しくは、「[マップ認証キーの要求](authentication-key.md)」をご覧ください。

## <a name="get-a-location-geocode"></a>位置情報の取得 (ジオコーディング)

このセクションでは、住所または地名を地理的な場所 (ジオコーディング) に変換する方法について説明します。

1.  [**MapLocationFinder**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinder)クラスの[**FindLocationsAsync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsasync)メソッドのいずれかのオーバーロードを、プレース名または住所を使用して呼び出します。
2.  [**FindLocationsAsync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsasync)メソッドは、 [**MapLocationFinderResult**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinderResult)オブジェクトを返します。
3.  コレクション[**MapLocation**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation)オブジェクトを公開するには、 [**MapLocationFinderResult**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinderResult)の "[**場所**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinderresult.locations)" プロパティを使用します。 指定された入力に対応する複数の場所がシステムによって検出される可能性があるため、複数の[**MapLocation**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation)オブジェクトが存在する可能性があります。

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

1.  [  **MapLocationFinder**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsatasync) クラスの [**FindLocationsAtAsync**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinder) メソッドを呼び出します。
2.  [  **FindLocationsAtAsync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsatasync) メソッドは、一致する [**MapLocation**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinderResult) オブジェクトのコレクションを含む [**MapLocationFinderResult**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation) オブジェクトを返します。
3.  コレクション[**MapLocation**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation)オブジェクトを公開するには、 [**MapLocationFinderResult**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinderResult)の "[**場所**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinderresult.locations)" プロパティを使用します。 指定された入力に対応する複数の場所がシステムによって検出される可能性があるため、複数の[**MapLocation**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation)オブジェクトが存在する可能性があります。
4.  各[**MapLocation**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation)の[**address**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocation.address)プロパティを使用して[**mapaddress**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapAddress)オブジェクトにアクセスします。

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
* [地図の設計ガイドライン](https://docs.microsoft.com/windows/uwp/maps-and-location/controls-map)
* [ビデオ: Windows アプリでの電話、タブレット、および PC でのマップと場所の活用](https://channel9.msdn.com/Events/Build/2015/2-757)
* [Bing Maps Developer Center](https://www.bingmapsportal.com/)
* [**MapLocationFinder**クラス](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinder)
* [**FindLocationsAsync**メソッド](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsasync)
* [**Findlocationsatasync**メソッド](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsatasync)
