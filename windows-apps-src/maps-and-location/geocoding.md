---
title: ジオコーディングと逆ジオコーディングの実行
description: このガイドでは、住所を地理的な場所 (ジオコーディング) に変換し、Windows.Services.Maps 名前空間に MapLocationFinder クラスのメソッドを呼び出すことによって (逆ジオコーディング) の住所を地理的な場所に変換する方法を示します。
ms.assetid: B912BE80-3E1D-43BB-918F-7A43327597D2
ms.date: 07/02/2018
ms.topic: article
keywords: Windows 10, UWP, ジオコーディング, 地図, 位置情報
ms.localizationpriority: medium
ms.openlocfilehash: 88687f6e7074ff7c914927a81a08720336b3c60c
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/29/2019
ms.locfileid: "66371871"
---
# <a name="perform-geocoding-and-reverse-geocoding"></a>ジオコーディングと逆ジオコーディングの実行

このガイドは、住所を地理的な場所 (ジオコーディング) に変換しのメソッドを呼び出して、地理的な場所を住所 (逆ジオコーディング) に変換する方法を示します、 [ **MapLocationFinder**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinder)クラス、 [ **Windows.Services.Maps** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps)名前空間。

> [!TIP]
> 詳細については、アプリでマップを使用して、ダウンロード、 [MapControl](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)サンプルから、 [Windows ユニバーサル サンプル リポジトリ](h https://github.com/Microsoft/Windows-universal-samples)GitHub で。

ジオコーディングと逆ジオコーディングに関連するクラスは、次のように編成されます。

-   [ **MapLocationFinder** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinder)クラスには、ジオコーディングを処理するメソッドが含まれています ([**FindLocationsAsync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsasync)) とリバース ジオコーディング ([**FindLocationsAtAsync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsatasync))。
-   これらのメソッドは両方を返す、 [ **MapLocationFinderResult** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinderResult)インスタンス。
-   [**場所**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinderresult.locations)のプロパティ、 [ **MapLocationFinderResult** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinderResult)のコレクションを公開[ **MapLocation** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation)オブジェクト。 
-   [**MapLocation** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation)両方のオブジェクトがある、 [**アドレス**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocation.address)プロパティを公開する、 [ **MapAddress** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapAddress)、番地を表すオブジェクト、 [**ポイント**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocation.point)プロパティを公開する、 [ **Geopoint** ](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geopoint)オブジェクト地理的な場所を表すです。

> [!IMPORTANT]
> マップ サービスを使用する前に、マップの認証キーを指定する必要があります。 詳しくは、「[マップ認証キーの要求](authentication-key.md)」をご覧ください。

## <a name="get-a-location-geocode"></a>位置情報の取得 (ジオコーディング)

このセクションでは、住所や場所名を地理的な場所 (ジオコーディング) に変換する方法を示します。

1.  オーバー ロードの 1 つを呼び出して、 [ **FindLocationsAsync** ](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsasync)のメソッド、 [ **MapLocationFinder** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinder)スポット名または住所を持つクラスアドレス。
2.  [ **FindLocationsAsync** ](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsasync)メソッドを返します。 を[ **MapLocationFinderResult** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinderResult)オブジェクト。
3.  使用して、 [**場所**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinderresult.locations)のプロパティ、 [ **MapLocationFinderResult** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinderResult)コレクションを公開する[ **MapLocation** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation)オブジェクト。 複数存在する可能性があります[ **MapLocation** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation)オブジェクトをシステムが複数ありますので、指定された入力に対応する場所。

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

このセクションでは、地理的な場所を (逆ジオコーディング) アドレスに変換する方法を示します。

1.  [  **MapLocationFinder**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinder) クラスの [**FindLocationsAtAsync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsatasync) メソッドを呼び出します。
2.  [  **FindLocationsAtAsync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsatasync) メソッドは、一致する [**MapLocation**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation) オブジェクトのコレクションを含む [**MapLocationFinderResult**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinderResult) オブジェクトを返します。
3.  使用して、 [**場所**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinderresult.locations)のプロパティ、 [ **MapLocationFinderResult** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinderResult)コレクションを公開する[ **MapLocation** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation)オブジェクト。 複数存在する可能性があります[ **MapLocation** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation)オブジェクトをシステムが複数ありますので、指定された入力に対応する場所。
4.  アクセス[ **MapAddress** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapAddress)オブジェクトを通じて、 [**アドレス**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocation.address)の各プロパティ[ **MapLocation**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation).

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

* [UWP の地図のサンプル](https://go.microsoft.com/fwlink/p/?LinkId=619977)
* [UWP の交通情報アプリのサンプル](https://go.microsoft.com/fwlink/p/?LinkId=619982)
* [地図の設計ガイドライン](https://docs.microsoft.com/windows/uwp/maps-and-location/controls-map)
* [ビデオ:Windows アプリでの電話、タブレット、PC で使用できるマップと位置情報の活用](https://channel9.msdn.com/Events/Build/2015/2-757)
* [Bing Maps Developer Center](https://www.bingmapsportal.com/)
* [**MapLocationFinder**クラス](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinder)
* [**FindLocationsAsync**メソッド](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsasync)
* [**FindLocationsAtAsync**メソッド](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsatasync)
