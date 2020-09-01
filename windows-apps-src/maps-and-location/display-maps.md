---
title: 2D、3D、Streetside ビューでの地図の表示
description: 地図を表示するには、マップ *プレースカード*と呼ばれる簡易非表示に対応したウィンドウか、フル機能を備えたマップ コントロールを使うことができます。
ms.assetid: 3839E00B-2C1E-4627-A45F-6DDA98D7077F
ms.date: 03/19/2018
ms.topic: article
keywords: Windows 10, UWP, 地図, 位置情報, マップ コントロール, マップ ビュー
ms.localizationpriority: medium
ms.openlocfilehash: 1979aa2b2e99a585a122e4835bb6920b9d342cd7
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162646"
---
# <a name="display-maps-with-2d-3d-and-streetside-views"></a>2D、3D、Streetside ビューでの地図の表示

地図を表示するには、マップ *プレースカード*と呼ばれる簡易非表示に対応したウィンドウか、フル機能を備えたマップ コントロールを使うことができます。

[地図サンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)をダウンロードして、このガイドで説明されている機能のいくつかを試してみてください。

<a id="placecard" />

## <a name="display-map-in-a-placecard"></a>プレースカードへの地図の表示
UI 要素またはユーザーがタッチするアプリの領域の上下左右に軽量なポップアップ ウィンドウを開いて、その内部に地図を表示することができます。 地図には、アプリ内の情報に関連する都市や住所を表示できます。  

次のプレースカードは、シアトルの街を表示します。

![シアトルの街を表示するプレースカード](images/placecard-city.png)

プレースカードのボタンの下にシアトルを表示するコードを次に示します。

```csharp
private void Seattle_Click(object sender, RoutedEventArgs e)
{
    Geopoint seattlePoint = new Geopoint
        (new BasicGeoposition { Latitude = 47.6062, Longitude = -122.3321 });

    PlaceInfo spaceNeedlePlace = PlaceInfo.Create(seattlePoint);

    FrameworkElement targetElement = (FrameworkElement)sender;

    GeneralTransform generalTransform =
        targetElement.TransformToVisual((FrameworkElement)targetElement.Parent);

    Rect rectangle = generalTransform.TransformBounds(new Rect(new Point
        (targetElement.Margin.Left, targetElement.Margin.Top), targetElement.RenderSize));

    spaceNeedlePlace.Show(rectangle, Windows.UI.Popups.Placement.Below);
}
```

次のプレースカードは、シアトルにある Space Needle の場所を表示します。

![Space Needle の場所を表示するプレースカード](images/placecard-needle.png)

プレースカードのボタンの下に Space Needle を表示するコードを次に示します。

```csharp
private void SpaceNeedle_Click(object sender, RoutedEventArgs e)
{
    Geopoint spaceNeedlePoint = new Geopoint
        (new BasicGeoposition { Latitude = 47.6205, Longitude = -122.3493 });

    PlaceInfoCreateOptions options = new PlaceInfoCreateOptions();

    options.DisplayAddress = "400 Broad St, Seattle, WA 98109";
    options.DisplayName = "Seattle Space Needle";

    PlaceInfo spaceNeedlePlace =  PlaceInfo.Create(spaceNeedlePoint, options);

    FrameworkElement targetElement = (FrameworkElement)sender;

    GeneralTransform generalTransform =
        targetElement.TransformToVisual((FrameworkElement)targetElement.Parent);

    Rect rectangle = generalTransform.TransformBounds(new Rect(new Point
        (targetElement.Margin.Left, targetElement.Margin.Top), targetElement.RenderSize));

    spaceNeedlePlace.Show(rectangle, Windows.UI.Popups.Placement.Below);
}
```

<a id="map-control" />

## <a name="display-map-in-a-control"></a>コントロールでの地図の表示

機能豊富でカスタマイズ可能なマップ データをアプリに表示するには、マップ コントロールを使います。 マップ コントロールでは、道路地図、航空写真、3D ビュー、ルート案内、検索結果、交通情報を表示できます。 マップ上には、現在地、ルート、関心のあるポイントを表示できます。 また、航空写真 3D ビュー、Streetside ビュー、交通情報、乗り換え情報、周辺情報を表示することもできます。

アプリ固有の地理情報または一般的な地理情報を表示できるアプリ内でマップが必要な場合に、マップ コントロールを使います。 アプリにマップ コントロールを含めておくことで、ユーザーはアプリの外部に移動することなく情報を得ることができます。

> [!NOTE]
>その情報を得るためにユーザーがアプリの外部に移動してもかまわない場合は、Windows マップ アプリを利用することも検討してください。 アプリから Windows マップ アプリを起動し、特定の地図、ルート案内、検索結果を表示することができます。 詳しくは、「[Windows マップ アプリの起動](../launch-resume/launch-maps-app.md)」をご覧ください。

### <a name="add-a-map-control-to-your-app"></a>アプリへのマップ コントロールの追加

[**MapControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) を追加することによって、XAML ページに地図を表示します。 **MapControl** を使うには、XAML ページまたはコード内に [**Windows.UI.Xaml.Controls.Maps**](/uwp/api/Windows.UI.Xaml.Controls.Maps) 名前空間の宣言が必要です。 ツールボックスからコントロールをドラッグすると、この名前空間宣言が自動的に追加されます。 XAML ページに **MapControl** を手動で追加する場合は、ページの上部に名前空間宣言を手動で追加する必要があります。

次の例では、基本的なマップ コントロールを表示し、タッチ入力を受け付けるだけでなくズーム コントロールとチルト コントロールを表示するように地図を構成しています。

```xml
<Page
    x:Class="MapsAndLocation1.DisplayMaps"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:MapsAndLocation1"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    xmlns:Maps="using:Windows.UI.Xaml.Controls.Maps"
    mc:Ignorable="d">

 <Grid x:Name="pageGrid" Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    <Maps:MapControl
       x:Name="MapControl1"            
       ZoomInteractionMode="GestureAndControl"
       TiltInteractionMode="GestureAndControl"   
       MapServiceToken="EnterYourAuthenticationKeyHere"/>

 </Grid>
</Page>
```

コードにマップ コントロールを追加する場合は、コード ファイルの先頭で手動で名前空間を宣言する必要があります。

```csharp
using Windows.UI.Xaml.Controls.Maps;
...

// Add the MapControl and the specify maps authentication key.
MapControl MapControl2 = new MapControl();
MapControl2.ZoomInteractionMode = MapInteractionMode.GestureAndControl;
MapControl2.TiltInteractionMode = MapInteractionMode.GestureAndControl;
MapControl2.MapServiceToken = "EnterYourAuthenticationKeyHere";
pageGrid.Children.Add(MapControl2);
```

### <a name="get-and-set-a-maps-authentication-key"></a>マップ認証キーの取得と設定

[**MapControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) やマップ サービスを使用する前に、マップ認証キーを [**MapServiceToken**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.mapservicetoken) プロパティの値として指定する必要があります。 前に示した例では、`EnterYourAuthenticationKeyHere` を [Bing Maps Developer Center](https://www.bingmapsportal.com/) から取得したキーで置き換えます。 マップ認証キーを指定するまでは、コントロールの下に **[警告: MapServiceToken が指定されていません]** というテキストが表示されます。 マップ認証キーを取得して設定する方法について詳しくは、「[マップ認証キーの要求](authentication-key.md)」をご覧ください。

## <a name="set-the-location-of-a-map"></a>地図の場所の設定
地図を表示するときは、特定の場所を示すように設定することも、ユーザーの現在の場所を使うこともできます。  

### <a name="set-a-starting-location-for-the-map"></a>地図の開始位置の設定

コードで [**MapControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) の [**Center**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.center) プロパティを指定するか、または XAML マークアップでプロパティをバインドすることにより、地図上の表示位置を設定します。 シアトル市を中心とした地図を表示する例を次に示します。

> [!NOTE]
> 文字列は [**Geopoint**](/uwp/api/Windows.Devices.Geolocation.Geopoint) に変換できないため、データ バインディングを使わない限り、XAML マークアップで [**Center**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.center) プロパティに対する値を指定できません。 (この制限は、[**MapControl.Location**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.setlocation) 添付プロパティにも適用されます)。

 
```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
{
   // Specify a known location.
   BasicGeoposition cityPosition = new BasicGeoposition() { Latitude = 47.604, Longitude = -122.329 };
   Geopoint cityCenter = new Geopoint(cityPosition);

   // Set the map location.
   MapControl1.Center = cityCenter;
   MapControl1.ZoomLevel = 12;
   MapControl1.LandmarksVisible = true;
}
```

![マップ コントロールの例。](images/displaymapsexample1.png)

### <a name="set-the-current-location-of-the-map"></a>地図の現在位置の設定

ユーザーの位置情報にアクセスするには、先にアプリで [**RequestAccessAsync**](/uwp/api/windows.devices.geolocation.geolocator.requestaccessasync) メソッドを呼び出す必要があります。 このときに、アプリをフォアグラウンドで実行し、**RequestAccessAsync** を UI スレッドから呼び出す必要があります。 位置情報に対するアクセス許可をユーザーがアプリに与えるまで、アプリは位置情報にアクセスできません。

[**Geolocator**](/uwp/api/Windows.Devices.Geolocation.Geolocator) クラスの [**GetGeopositionAsync**](/uwp/api/windows.devices.geolocation.geolocator.getgeopositionasync) メソッドを使って、デバイスの現在の位置情報を取得します (位置情報にアクセスできる場合)。 対応する [**Geopoint**](/uwp/api/Windows.Devices.Geolocation.Geopoint) を取得するには、geoposition オブジェクトの geocoordinate の [**Point**](/uwp/api/windows.devices.geolocation.geocoordinate.point) プロパティを使います。 詳しくは、「[現在の位置情報の取得](get-location.md)」をご覧ください。

```csharp
// Set your current location.
var accessStatus = await Geolocator.RequestAccessAsync();
switch (accessStatus)
{
   case GeolocationAccessStatus.Allowed:

      // Get the current location.
      Geolocator geolocator = new Geolocator();
      Geoposition pos = await geolocator.GetGeopositionAsync();
      Geopoint myLocation = pos.Coordinate.Point;

      // Set the map location.
      MapControl1.Center = myLocation;
      MapControl1.ZoomLevel = 12;
      MapControl1.LandmarksVisible = true;
      break;

   case GeolocationAccessStatus.Denied:
      // Handle the case  if access to location is denied.
      break;

   case GeolocationAccessStatus.Unspecified:
      // Handle the case if  an unspecified error occurs.
      break;
}
```

地図にデバイスの位置を表示するときは、位置情報の精度に基づいてグラフィックスを表示してズーム レベルを設定することを検討します。 詳細については、「 [場所を認識するアプリのガイドライン](./guidelines-and-checklist-for-detecting-location.md)」を参照してください。

### <a name="change-the-location-of-the-map"></a>地図の位置の変更

2D 地図に表示する位置を変更するには、[**TrySetViewAsync**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.trysetviewasync) メソッドのいずれかのオーバーロードを呼び出します。 そのメソッドを使って、[**Center**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.center)、[**ZoomLevel**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.zoomlevel)、[**Heading**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.heading)、[**Pitch**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.pitch) の新しい値を指定します。 また、ビューが変わるときに使うアニメーションをオプションで指定することもできます。そのためには、[**MapAnimationKind**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapAnimationKind) 列挙値の定数を指定します。

3D 地図の場所を変更するには、代わりに [**TrySetSceneAsync**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.trysetsceneasync) メソッドを使います。 詳しくは、「[航空写真 3D ビューの表示](#3Dviews)」をご覧ください。

地図上に [**GeoboundingBox**](/uwp/api/Windows.Devices.Geolocation.GeoboundingBox) の内容を表示するには、[**TrySetViewBoundsAsync**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.trysetviewboundsasync) メソッドを呼び出します。 たとえばこのメソッドを使うと、地図上にルートやルートの一部を表示することができます。 詳細については、「 [マップにルートと方向を表示する](routes-and-directions.md)」を参照してください。

## <a name="change-the-appearance-of-a-map"></a>地図の外観の変更

地図の外観をカスタマイズするには、マップ コントロールの [**StyleSheet**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.StyleSheet) プロパティを既存の [**MapStyleSheet**](/uwp/api/windows.ui.xaml.controls.maps.mapstylesheet) オブジェクトに設定します。

```csharp
myMap.StyleSheet = MapStyleSheet.RoadDark();
```

![濃色スタイルの地図](images/style-dark.png)

また、JSON を使用してカスタム スタイルを定義し、その JSON を使用して [**MapStyleSheet**](/uwp/api/windows.ui.xaml.controls.maps.mapstylesheet) オブジェクトを作成することもできます。

スタイルシートの JSON は、 [マップスタイルシートエディター](https://www.microsoft.com/p/map-style-sheet-editor/9nbhtcjt72ft) アプリケーションを使用して対話形式で作成できます。

```csharp
myMap.StyleSheet = MapStyleSheet.ParseFromJson(@"
    {
        ""version"": ""1.0"",
        ""settings"": {
            ""landColor"": ""#FFFFFF"",
            ""spaceColor"": ""#000000""
        },
        ""elements"": {
            ""mapElement"": {
                ""labelColor"": ""#000000"",
                ""labelOutlineColor"": ""#FFFFFF""
            },
            ""water"": {
                ""fillColor"": ""#DDDDDD""
            },
            ""area"": {
                ""fillColor"": ""#EEEEEE""
            },
            ""political"": {
                ""borderStrokeColor"": ""#CCCCCC"",
                ""borderOutlineColor"": ""#00000000""
            }
        }
    }
");
```

![カスタム スタイルの地図](images/style-custom.png)

完全な JSON エントリのリファレンスについては、「[マップ スタイル シート リファレンス](elements-of-map-style-sheet.md)」をご覧ください。

最初は既存のシートを使用して、次に JSON を使用して要素を上書きできます。 この例では、最初は既存のスタイルを使用し、次に JSON を使用して水域の色のみを変更しています。

```csharp
 MapStyleSheet \customSheet = MapStyleSheet.ParseFromJson(@"
    {
        ""version"": ""1.0"",
        ""elements"": {
            ""water"": {
                ""fillColor"": ""#DDDDDD""
            }
        }
    }
");

MapStyleSheet builtInSheet = MapStyleSheet.RoadDark();

myMap.StyleSheet = MapStyleSheet.Combine(new List<MapStyleSheet> { builtInSheet, customSheet });
```

![複数のスタイルの地図](images/style-combined.png)

>[!NOTE]
>2 番目のスタイル シートで定義するスタイルは、最初のスタイルを上書きします。

## <a name="set-orientation-and-perspective"></a>向きと視点の設定

マップ カメラを拡大、縮小、回転、傾けることで、求める効果をもたらす適切な角度に設定できます。 次のプロパティをお試しください。

-   地図の**中心**を地理的位置に設定するには、[**Center**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.center) プロパティを設定します。
-   [**ZoomLevel**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.zoomlevel) プロパティに 1 ～ 20 度の値を設定することにより、地図の**ズーム レベル**を設定します。
-   [**Heading**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.heading) プロパティを設定することにより地図の**回転**を設定します。ここでは 0 度または 360 度 = 北、90 度 = 東、180 度 = 南、270 度 = 西です。
-   [**DesiredPitch**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.desiredpitch) プロパティに 0 ～ 65 度の値を設定することにより、地図の**傾き**を設定します。

## <a name="show-and-hide-map-features"></a>地図機能を表示または非表示にする

道路やランドマークなどの地図機能を表示または非表示にするには、[**MapControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) の次のプロパティの値を設定します。

* 地図に**建物やランドマーク**を表示するには、[**LandmarksVisible**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.landmarksvisible) プロパティを有効または無効にします。

  > [!NOTE]
  > 建物の表示と非表示を切り替えることはできますが、3 次元表示を無効にすることはできません。  

* 地図に公共階段などの**歩行者用の施設**を表示するには、[**PedestrianFeaturesVisible**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.pedestrianfeaturesvisible) プロパティを有効または無効にします。
* 地図に**交通情報**を表示するには、[**TrafficFlowVisible**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.trafficflowvisible) プロパティを有効または無効にします。
* 地図に**透かし**を表示するかどうかを指定するには、[**WatermarkMode**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.watermarkmode) プロパティを [**MapWatermarkMode**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapWatermarkMode) 定数のいずれかに設定します。
* 地図に**自動車ルートや徒歩ルート**を表示するには、マップ コントロールの [**Routes**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.routes) コレクションに [**MapRouteView**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapRouteView) を追加します。 詳細と例については、「 [マップにルートと方向を表示](routes-and-directions.md)する」を参照してください。

[**MapControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) でプッシュピン、図形、XAML コントロールを表示する方法については、「[関心のあるポイント (POI) の地図への表示](display-poi.md)」をご覧ください。

## <a name="display-streetside-views"></a>Streetside ビューの表示


Streetside ビューは、視点がストリート レベルにある場合の場所の見え方を示すもので、マップ コントロールの上に表示されます。

![マップ コントロールの Streetside ビューの例。](images/onlystreetside-730width.png)

Streetside ビューの「内部」での操作は、マップ コントロールにもともと表示されている地図とは独立していることを考慮します。 たとえば、Streetside ビューで場所を変更しても、Streetside ビューの「下」にある地図の場所や外観は変わりません。 コントロールの右上隅にある **[X]** をクリックして Streetside ビューを閉じると、元の地図は Streetside ビューが表示される前のままです。

Streetside ビューを表示するには

1.  [**IsStreetsideSupported**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.isstreetsidesupported) を確認して、Streetside ビューがデバイスでサポートされているかどうかを調べます。
2.  Streetside ビューがサポートされている場合は、[**FindNearbyAsync**](/uwp/api/windows.ui.xaml.controls.maps.streetsidepanorama.findnearbyasync) を呼び出し、指定した位置の近くに [**StreetsidePanorama**](/uwp/api/Windows.UI.Xaml.Controls.Maps.StreetsidePanorama) を作成します。
3.  [**StreetsidePanorama**](/uwp/api/Windows.UI.Xaml.Controls.Maps.StreetsidePanorama) が null でないかどうかを確認し、近隣のパノラマ写真があるかどうかを調べます。
4.  近隣のパノラマ写真があった場合、[**StreetsideExperience**](/uwp/api/windows.ui.xaml.controls.maps.streetsideexperience) を作成し、マップ コントロールの [**CustomExperience**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.customexperience) プロパティに設定します。

次の例は、前掲の画像に似た Streetside ビューを表示する方法を示しています。

**メモ**   マップコントロールのサイズが小さすぎると、概要マップは表示されません。

 

```csharp
private async void showStreetsideView()
{
   // Check if Streetside is supported.
   if (MapControl1.IsStreetsideSupported)
   {
      // Find a panorama near Avenue Gustave Eiffel.
      BasicGeoposition cityPosition = new BasicGeoposition() { Latitude = 48.858, Longitude = 2.295};
      Geopoint cityCenter = new Geopoint(cityPosition);
      StreetsidePanorama panoramaNearCity = await StreetsidePanorama.FindNearbyAsync(cityCenter);

      // Set the Streetside view if a panorama exists.
      if (panoramaNearCity != null)
      {
         // Create the Streetside view.
         StreetsideExperience ssView = new StreetsideExperience(panoramaNearCity);
         ssView.OverviewMapVisible = true;
         MapControl1.CustomExperience = ssView;
      }
   }
   else
   {
      // If Streetside is not supported
      ContentDialog viewNotSupportedDialog = new ContentDialog()
      {
         Title = "Streetside is not supported",
         Content ="\nStreetside views are not supported on this device.",
         PrimaryButtonText = "OK"
      };
      await viewNotSupportedDialog.ShowAsync();            
   }
}
```

<a id="3Dviews" />
## <a name="display-aerial-3d-views"></a>航空写真 3D ビューの表示


[**MapScene**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapScene) クラスを使って、地図の 3D 視点を指定します。 マップ シーンは、地図に表示される 3D ビューを表します。 [**MapCamera**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapCamera) クラスは、このようなビューが表示されるカメラの位置を表します。

![マップ シーンの場所と MapCamera の場所を示す図](images/mapcontrol-techdiagram.png)

建物などの地物を地図表面に 3D 表示するには、マップ コントロールの [**Style**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.style) プロパティを [**MapStyle.Aerial3DWithRoads**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapStyle) に設定します。 **Aerial3DWithRoads** スタイルの 3D ビューの例を次に示します。

![3D 地図ビューの例。](images/only3d-730width.png)

3D ビューを表示するには

1.  [**Is3DSupported**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.is3dsupported) を確認して、3D ビューがデバイスでサポートされているかどうかを調べます。
2.  3D ビューがサポートされている場合は、マップコントロールの [**スタイル**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.style) プロパティを [**mapstyle**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapStyle)に設定します。
3.  [**Createfromlocationandradius**](/uwp/api/windows.ui.xaml.controls.maps.mapscene.createfromlocationandradius)や[**createfromcamera**](/uwp/api/windows.ui.xaml.controls.maps.mapscene.createfromcamera)など、多くの**createfrom**メソッドの1つを使用して[**mapscene**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapScene)オブジェクトを作成します。
4.  [**TrySetSceneAsync**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.trysetsceneasync) を呼び出して、3D ビューを表示します。 また、ビューが変わるときに使うアニメーションをオプションで指定することもできます。そのためには、[**MapAnimationKind**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapAnimationKind) 列挙値の定数を指定します。

次の例は、3D ビューを表示する方法を示しています。

```csharp
private async void display3DLocation()
{
   if (MapControl1.Is3DSupported)
   {
      // Set the aerial 3D view.
      MapControl1.Style = MapStyle.Aerial3DWithRoads;

      // Specify the location.
      BasicGeoposition hwGeoposition = new BasicGeoposition() { Latitude = 43.773251, Longitude = 11.255474};
      Geopoint hwPoint = new Geopoint(hwGeoposition);

      // Create the map scene.
      MapScene hwScene = MapScene.CreateFromLocationAndRadius(hwPoint,
                                                                           80, /* show this many meters around */
                                                                           0, /* looking at it to the North*/
                                                                           60 /* degrees pitch */);
      // Set the 3D view with animation.
      await MapControl1.TrySetSceneAsync(hwScene,MapAnimationKind.Bow);
   }
   else
   {
      // If 3D views are not supported, display dialog.
      ContentDialog viewNotSupportedDialog = new ContentDialog()
      {
         Title = "3D is not supported",
         Content = "\n3D views are not supported on this device.",
         PrimaryButtonText = "OK"
      };
      await viewNotSupportedDialog.ShowAsync();
   }
}
```

## <a name="get-info-about-locations"></a>位置に関する情報の取得


地図上の位置に関する情報を取得するには、[**MapControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) の次のメソッドを呼び出します。

-   [**Trygetlocationfromoffset**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.getlocationfromoffset) メソッド-マップコントロールのビューポートの指定したポイントに対応する地理的位置を取得します。
-   [**GetOffsetFromLocation**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.getoffsetfromlocation) メソッド - 指定した地理的な位置情報に対応するマップ コントロールのビューポート内のポイントを取得します。
-   [**IsLocationInView**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.islocationinview) メソッド - 指定した地理的な位置がマップ コントロールのビューポート内に現在表示されているかどうかを調べます。
-   [**FindMapElementsAtOffset**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.findmapelementsatoffset) メソッド - マップ コントロールのビューポート内の指定したポイントにある地図上の要素を取得します。

## <a name="handle-interaction-and-changes"></a>操作と変更の処理


地図上でのユーザー入力ジェスチャを処理するには、[**MapControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) の次のイベントを処理します。 地図上の地理的な位置、およびジェスチャが行われたビューポート内の実際の位置に関する情報を取得するには、[**MapInputEventArgs**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapInputEventArgs) の [**Location**](/uwp/api/windows.ui.xaml.controls.maps.mapinputeventargs.location) プロパティと [**Position**](/uwp/api/windows.ui.xaml.controls.maps.mapinputeventargs.position) プロパティの値を確認します。

-   [**MapTapped**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.maptapped)
-   [**MapDoubleTapped**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.mapdoubletapped)
-   [**MapHolding**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.mapholding)

地図が読み込まれている最中であるか、完全に読み込まれたかどうかを判断するには、コントロールの [**LoadingStatusChanged**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.loadingstatuschanged) イベントを処理します。

ユーザーやアプリによる地図の設定変更に対応するには、[**MapControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) の次のイベントを処理します。 [マップのガイドライン]()

-   [**CenterChanged**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.centerchanged)
-   [**HeadingChanged**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.headingchanged)
-   [**PitchChanged**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.pitchchanged)
-   [**ZoomLevelChanged**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.zoomlevelchanged)

## <a name="best-practice-recommendations"></a>ベスト プラクティスの推奨事項

-   ユーザーが地理情報を表示するためにパンとズームを過度に使用しなくて済むように、十分な画面領域 (または画面全体) を使用してマップを表示します。

-   静的な情報ビューの提示をするためにのみマップを使う場合、小さなマップを使う方が適している場合があります。 小さく静的なマップを使う場合は、使いやすさを考えてサイズを決めます。画面上の領域を十分節約できる程度に小さく、判読しにくくならない程度に大きくします。

-   マップ シーンに関心のあるポイントを埋め込むには、[**MapElements**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.mapelementsproperty) を使います。その他の情報も、マップ シーンのオーバーレイとして表示される一時的な UI に表示できます。

## <a name="related-topics"></a>関連トピック

* [Bing Maps Developer Center](https://www.bingmapsportal.com/)
* [UWP の地図のサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)
* [現在の位置情報の取得](get-location.md)
* [位置認識アプリの設計ガイドライン](./guidelines-and-checklist-for-detecting-location.md)
* [地図の設計ガイドライン]()
* [ビルド2015ビデオ: Windows アプリでの電話、タブレット、および PC でのマップと場所の活用](https://channel9.msdn.com/Events/Build/2015/2-757)
* [UWP の交通情報アプリのサンプル](https://github.com/Microsoft/Windows-appsample-trafficapp)
* [**MapControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl)