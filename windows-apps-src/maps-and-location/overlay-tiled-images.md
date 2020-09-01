---
title: 地図へのタイル画像のオーバーレイ
description: タイル ソースを使って、地図上にサード パーティ製タイルまたはカスタム タイル画像をオーバーレイします。 タイル ソースを使って、気象データ、人口データ、地質データなどの特殊な情報をオーバーレイすることや、既定の地図を完全に置き換えることができます。
ms.assetid: 066BD6E2-C22B-4F5B-AA94-5D6C86A09BDF
ms.date: 07/19/2018
ms.topic: article
keywords: Windows 10、UWP、地図、位置情報、画像、オーバーレイ
ms.localizationpriority: medium
ms.openlocfilehash: a2a93ac408232e71c2a5ec2bc79f99388e24e5df
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89171736"
---
# <a name="overlay-tiled-images-on-a-map"></a>地図へのタイル画像のオーバーレイ

タイル ソースを使って、地図上にサード パーティ製タイルまたはカスタム タイル画像をオーバーレイします。 タイル ソースを使って、気象データ、人口データ、地質データなどの特殊な情報をオーバーレイすることや、既定の地図を完全に置き換えることができます。

**ヒント** アプリでの地図の使用について詳しくは、Github で[ユニバーサル Windows プラットフォーム (UWP) の地図サンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)をダウンロードしてください。

<a id="tileintro" />

## <a name="tiled-image-overview"></a>タイル画像の概要

Nokia Maps や Bing Maps などのマップ サービスでは、迅速な取得と表示のために正方形タイルに地図を切り取ります。 こうしたタイルは 256 ピクセル x 256 ピクセル サイズであり、いくつかの詳細レベルで事前にレンダリングされます。 また、多くのサード パーティ サービスがタイルに切り取られた地図ベースのデータを提供しています。 タイル ソースを使ってサード パーティ製タイルを取得するか、独自のカスタム タイルを作成してそれを [**MapControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) に表示された地図上にオーバーレイできます。

**重要**   タイルソースを使用する場合、個々のタイルを要求または配置するコードを記述する必要はありません。 [**MapControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) が必要時にタイルを要求します。 各要求では、個々のタイルについて X 座標と Y 座標、ズーム レベルを指定します。 タイルを取得するために使う URI またはファイル名の形式を **UriFormatString** プロパティに指定します。 つまり、各タイルの X 座標、Y 座標、ズーム レベルを渡す場所を示すベース URI またはファイル名に、置き換え可能なパラメーターを挿入します。

次に、X 座標、Y 座標、ズーム レベルの置き換え可能なパラメーターを示す [**HttpMapTileDataSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.HttpMapTileDataSource) の [**UriFormatString**](/uwp/api/windows.ui.xaml.controls.maps.httpmaptiledatasource.uriformatstring) プロパティの例を示します。

```syntax
http://www.<web service name>.com/z={zoomlevel}&x={x}&y={y}
```

X 座標と Y 座標は、指定された詳細レベルで世界地図内の個々のタイルの場所を表します。 タイルの番号付けは、地図の左上端の {0, 0} から開始します。 たとえば、{1, 2} のタイルは、タイル グリッドの第 1 行の第 2 列にあります。

マッピング サービスにより使用されるタイル システムについては、[Bing Maps タイル システムに関するページ (英語) ](/bingmaps/articles/bing-maps-tile-system)をご覧ください。

### <a name="overlay-tiles-from-a-tile-source"></a>タイル ソースからのタイルのオーバーレイ

[**MapTileDataSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileDataSource) を使って、タイル ソースからのタイル画像を地図にオーバーレイします。

1.  [**MapTileDataSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileDataSource) から継承する 3 つのタイル データ ソース クラスのいずれかをインスタンス化します。

    -   [**HttpMapTileDataSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.HttpMapTileDataSource)
    -   [**LocalMapTileDataSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.LocalMapTileDataSource)
    -   [**CustomMapTileDataSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.CustomMapTileDataSource)

    ベース URI またはファイル名に置き換え可能なパラメーターを挿入することにより、タイルの要求に使う **UriFormatString** を構成します。

    次の例では、[**HttpMapTileDataSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.HttpMapTileDataSource) をインスタンス化します。 次の例では、**HttpMapTileDataSource** のコンストラクターで [**UriFormatString**](/uwp/api/windows.ui.xaml.controls.maps.httpmaptiledatasource.uriformatstring) の値を指定しています。

    ```csharp
        HttpMapTileDataSource dataSource = new HttpMapTileDataSource(
          "http://www.<web service name>.com/z={zoomlevel}&x={x}&y={y}");
    ```

2.  [**MapTileSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileSource) をインスタンス化および構成します。 以前の手順で **MapTileSource** の [**DataSource**](/uwp/api/windows.ui.xaml.controls.maps.maptilesource.datasource) として構成した [**MapTileDataSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileDataSource) を指定します。

    次の例では、[**MapTileSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileSource) のコンストラクターで [**DataSource**](/uwp/api/windows.ui.xaml.controls.maps.maptilesource.datasource) を指定しています。

    ```csharp
        MapTileSource tileSource = new MapTileSource(dataSource);
    ```

    [**MapTileSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileSource) のプロパティを使うことにより、タイルが表示される条件を制限できます。

    -   [**Bounds**](/uwp/api/windows.ui.xaml.controls.maps.maptilesource.bounds) プロパティの値を指定することにより、特定地域内にのみタイルを表示します。
    -   [**ZoomLevelRange**](/uwp/api/windows.ui.xaml.controls.maps.maptilesource.zoomlevelrange) プロパティの値を指定することにより、特定の詳細レベルでのみタイルを表示します。

    オプションで、[**Layer**](/uwp/api/windows.ui.xaml.controls.maps.maptilesource.layer)、[**AllowOverstretch**](/uwp/api/windows.ui.xaml.controls.maps.maptilesource.allowoverstretch)、[**IsRetryEnabled**](/uwp/api/windows.ui.xaml.controls.maps.maptilesource.isretryenabled)、[**IsTransparencyEnabled**](/uwp/api/windows.ui.xaml.controls.maps.maptilesource.istransparencyenabled) など、タイルの読み込みまたは表示に影響する [**MapTileSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileSource) の他のプロパティを構成します。

3.  [**MapControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) の [**TileSources**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.tilesources) コレクションに [**MapTileSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileSource) を追加します。

    ```csharp
         MapControl1.TileSources.Add(tileSource);
    ```

## <a name="overlay-tiles-from-a-web-service"></a>Web サービスからのタイルのオーバーレイ


[**HttpMapTileDataSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.HttpMapTileDataSource) を使って、Web サービスから取得したタイル画像をオーバーレイします。

1.  [**HttpMapTileDataSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.HttpMapTileDataSource) をインスタンス化します。
2.  Web サービスで [**UriFormatString**](/uwp/api/windows.ui.xaml.controls.maps.httpmaptiledatasource.uriformatstring) プロパティの値として想定される URI の形式を指定します。 この値を作成するには、ベース URI に置き換え可能なパラメーターを挿入します。 たとえば次のコード サンプルでは、**UriFormatString** の値は次のとおりです。

    ``` syntax
    http://www.<web service name>.com/z={zoomlevel}&x={x}&y={y}
    ```

    この Web サービスでは、置き換え可能なパラメーター {x}、{y}、{zoomlevel} を含む URI をサポートする必要があります。 大半の Web サービス (たとえば Nokia、Bing、Google など) で、この形式の URI がサポートされています。 [**UriFormatString**](/uwp/api/windows.ui.xaml.controls.maps.httpmaptiledatasource.uriformatstring) プロパティで使用できない追加引数が Web サービスで必要な場合は、カスタム URI を作成する必要があります。 [**UriRequested**](/uwp/api/windows.ui.xaml.controls.maps.httpmaptiledatasource.urirequested) イベントを処理することにより、カスタム URI を作成して返します。 詳しくは、このトピックで後述する「[カスタム URI の指定](#customuri)」をご覧ください。

3.  次に、「[タイル画像の概要](#tileintro)」で説明した残りの手順に従います。

次の例では、北米のマップに架空の Web サービスからのタイルをオーバーレイしています。 [**HttpMapTileDataSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.HttpMapTileDataSource) のコンストラクターで [**UriFormatString**](/uwp/api/windows.ui.xaml.controls.maps.httpmaptiledatasource.uriformatstring) の値を指定しています。 この例では、省略可能な [**Bounds**](/uwp/api/windows.ui.xaml.controls.maps.maptilesource.bounds) プロパティによって指定した地理的境界内にのみタイルが表示されます。

```csharp
private void AddHttpMapTileSource()
{
    // Create the bounding box in which the tiles are displayed.
    // This example represents North America.
    BasicGeoposition northWestCorner =
        new BasicGeoposition() { Latitude = 48.38544, Longitude = -124.667360 };
    BasicGeoposition southEastCorner =
        new BasicGeoposition() { Latitude = 25.26954, Longitude = -80.30182 };
    GeoboundingBox boundingBox = new GeoboundingBox(northWestCorner, southEastCorner);

    // Create an HTTP data source.
    // This example retrieves tiles from a fictitious web service.
    HttpMapTileDataSource dataSource = new HttpMapTileDataSource(
        "http://www.<web service name>.com/z={zoomlevel}&x={x}&y={y}");

    // Optionally, add custom HTTP headers if the web service requires them.
    dataSource.AdditionalRequestHeaders.Add("header name", "header value");

    // Create a tile source and add it to the Map control.
    MapTileSource tileSource = new MapTileSource(dataSource);
    tileSource.Bounds = boundingBox;
    MapControl1.TileSources.Add(tileSource);
}
```

```cppwinrt
...
#include <winrt/Windows.Devices.Geolocation.h>
#include <winrt/Windows.UI.Xaml.Controls.Maps.h>
...
void MainPage::AddHttpMapTileSource()
{
    Windows::Devices::Geolocation::BasicGeoposition northWest{ 48.38544, -124.667360 };
    Windows::Devices::Geolocation::BasicGeoposition southEast{ 25.26954, -80.30182 };
    Windows::Devices::Geolocation::GeoboundingBox boundingBox{ northWest, southEast };

    Windows::UI::Xaml::Controls::Maps::HttpMapTileDataSource dataSource{
        L"http://www.<web service name>.com/z={zoomlevel}&x={x}&y={y}" };

    dataSource.AdditionalRequestHeaders().Insert(L"header name", L"header value");

    Windows::UI::Xaml::Controls::Maps::MapTileSource tileSource{ dataSource };
    tileSource.Bounds(boundingBox);

    MapControl1().TileSources().Append(tileSource);
}
...
```

```cpp
void MainPage::AddHttpMapTileSource()
{
    BasicGeoposition northWest = { 48.38544, -124.667360 };
    BasicGeoposition southEast = { 25.26954, -80.30182 };
    GeoboundingBox^ boundingBox = ref new GeoboundingBox(northWest, southEast);

    auto dataSource = ref new Windows::UI::Xaml::Controls::Maps::HttpMapTileDataSource(
        "http://www.<web service name>.com/z={zoomlevel}&x={x}&y={y}");

    dataSource->AdditionalRequestHeaders->Insert("header name", "header value");

    auto tileSource = ref new Windows::UI::Xaml::Controls::Maps::MapTileSource(dataSource);
    tileSource->Bounds = boundingBox;

    this->MapControl1->TileSources->Append(tileSource);
}
```

## <a name="overlay-tiles-from-local-storage"></a>ローカル記憶域からのタイルのオーバーレイ


[**LocalMapTileDataSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.LocalMapTileDataSource) を使って、ローカル ストレージにファイルとして格納されたタイル画像をオーバーレイします。 通常、こうしたファイルはアプリと共にパッケージ化して配布します。

1.  [**LocalMapTileDataSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.LocalMapTileDataSource) をインスタンス化します。
2.  [**UriFormatString**](/uwp/api/windows.ui.xaml.controls.maps.localmaptiledatasource.uriformatstring) プロパティの値としてファイル名の形式を指定します。 この値を作成するには、ベース ファイル名に置き換え可能なパラメーターを挿入します。 たとえば、次のコードサンプルでは、 [**Uriformatstring**](/uwp/api/windows.ui.xaml.controls.maps.httpmaptiledatasource.uriformatstring) の値は次のようになります。

    ``` syntax
        Tile_{zoomlevel}_{x}_{y}.png
    ```

    [**UriFormatString**](/uwp/api/windows.ui.xaml.controls.maps.localmaptiledatasource.uriformatstring) プロパティで使用できない追加引数がファイル名の形式で必要な場合は、カスタム URI を作成する必要があります。 [**UriRequested**](/uwp/api/windows.ui.xaml.controls.maps.localmaptiledatasource.urirequested) イベントを処理することにより、カスタム URI を作成して返します。 詳しくは、このトピックで後述する「[カスタム URI の指定](#customuri)」をご覧ください。

3.  次に、「[タイル画像の概要](#tileintro)」で説明した残りの手順に従います。

ローカル ストレージからタイルを読み込むために、次のプロトコルと場所を使用できます。

| Uri | 詳細情報 |
|---------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| ms-appx:/// | アプリのインストール フォルダーのルートを参照します。 |
|  | これは、[Package.InstalledLocation](/uwp/api/windows.applicationmodel.package.installedlocation) プロパティによって参照される場所です。 |
| ms-appdata:///local | アプリのローカル ストレージのルートを参照します。 |
|  | これは、[ApplicationData.LocalFolder](/uwp/api/windows.storage.applicationdata.localfolder) プロパティによって参照される場所です。 |
| ms-appdata:///temp | アプリの一時フォルダーを参照します。 |
|  | これは、[ApplicationData.TemporaryFolder](/uwp/api/windows.storage.applicationdata.temporaryfolder) プロパティによって参照される場所です。 |

 

次の例では、`ms-appx:///` プロトコルを使って、アプリのインストール フォルダーにファイルとして格納されたタイルを読み込みます。 [**LocalMapTileDataSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.LocalMapTileDataSource) のコンストラクターで [**UriFormatString**](/uwp/api/windows.ui.xaml.controls.maps.localmaptiledatasource.uriformatstring) の値を指定しています。 この例では、省略可能な [**ZoomLevelRange**](/uwp/api/windows.ui.xaml.controls.maps.maptilesource.zoomlevelrange) プロパティによって指定された範囲内に地図のズーム レベルがある場合にのみ、タイルが表示されます。

```csharp
        void AddLocalMapTileSource()
        {
            // Specify the range of zoom levels
            // at which the overlaid tiles are displayed.
            MapZoomLevelRange range;
            range.Min = 11;
            range.Max = 20;

            // Create a local data source.
            LocalMapTileDataSource dataSource = new LocalMapTileDataSource(
                "ms-appx:///TileSourceAssets/Tile_{zoomlevel}_{x}_{y}.png");

            // Create a tile source and add it to the Map control.
            MapTileSource tileSource = new MapTileSource(dataSource);
            tileSource.ZoomLevelRange = range;
            MapControl1.TileSources.Add(tileSource);
        }
```

<a id="customuri" />

## <a name="provide-a-custom-uri"></a>カスタム URI の指定

[**HttpMapTileDataSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.HttpMapTileDataSource) の [**UriFormatString**](/uwp/api/windows.ui.xaml.controls.maps.httpmaptiledatasource.uriformatstring) プロパティまたは [**LocalMapTileDataSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.LocalMapTileDataSource) の [**UriFormatString**](/uwp/api/windows.ui.xaml.controls.maps.localmaptiledatasource.uriformatstring) プロパティにより使用できる置き換え可能なパラメーターがタイルの取得に十分でない場合は、カスタム URI を作成する必要があります。 **UriRequested** イベントのカスタム ハンドラーを指定することによりカスタム URI を作成して返します。 **UriRequested** イベントは、個々のタイルについて発生します。

1.  カスタム URI を作成するために、**UriRequested** イベントのカスタム ハンドラーで、[**MapTileUriRequestedEventArgs**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileUriRequestedEventArgs) の [**X**](/uwp/api/windows.ui.xaml.controls.maps.maptileurirequestedeventargs.x) プロパティ、[**Y**](/uwp/api/windows.ui.xaml.controls.maps.maptileurirequestedeventargs.y) プロパティ、[**ZoomLevel**](/uwp/api/windows.ui.xaml.controls.maps.maptileurirequestedeventargs.zoomlevel) プロパティにより必要なカスタム引数を組み合わせます。
2.  [**MapTileUriRequestedEventArgs**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileUriRequestedEventArgs) の [**Request**](/uwp/api/windows.ui.xaml.controls.maps.maptileurirequestedeventargs.request) プロパティに含まれている [**MapTileUriRequest**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileUriRequest) の [**Uri**](/uwp/api/windows.ui.xaml.controls.maps.maptileurirequest.uri) プロパティにカスタム URI を返します。

次の例では、**UriRequested** イベントのカスタム ハンドラーを作成することによりカスタム URI を指定する方法を示しています。 また、カスタム URI を作成するために非同期処理が必要な場合に、保留パターンを実装する方法を示しています。

```csharp
using Windows.UI.Xaml.Controls.Maps;
using System.Threading.Tasks;
...
            var httpTileDataSource = new HttpMapTileDataSource();
            // Attach a handler for the UriRequested event.
            httpTileDataSource.UriRequested += HandleUriRequestAsync;
            MapTileSource httpTileSource = new MapTileSource(httpTileDataSource);
            MapControl1.TileSources.Add(httpTileSource);
...
        // Handle the UriRequested event.
        private async void HandleUriRequestAsync(HttpMapTileDataSource sender,
            MapTileUriRequestedEventArgs args)
        {
            // Get a deferral to do something asynchronously.
            // Omit this line if you don't have to do something asynchronously.
            var deferral = args.Request.GetDeferral();

            // Get the custom Uri.
            var uri = await GetCustomUriAsync(args.X, args.Y, args.ZoomLevel);

            // Specify the Uri in the Uri property of the MapTileUriRequest.
            args.Request.Uri = uri;

            // Notify the app that the custom Uri is ready.
            // Omit this line also if you don't have to do something asynchronously.
            deferral.Complete();
        }

        // Create the custom Uri.
        private async Task<Uri> GetCustomUriAsync(int x, int y, int zoomLevel)
        {
            // Do something asynchronously to create and return the custom Uri.        }
        }
```

## <a name="overlay-tiles-from-a-custom-source"></a>カスタム ソースからのタイルのオーバーレイ

[**CustomMapTileDataSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.CustomMapTileDataSource) を使って、カスタム タイルをオーバーレイします。 プログラムによってメモリ内で随時にタイルを作成するか、または別のソースから既存のタイルを読み込むために独自のコードを記述します。

カスタム タイルを作成するか読み込むには、[**BitmapRequested**](/uwp/api/windows.ui.xaml.controls.maps.custommaptiledatasource.bitmaprequested) イベントのカスタム ハンドラーを指定します。 **BitmapRequested** イベントは、個々のタイルについて発生します。

1.  カスタム タイルを作成または取得するために、[**BitmapRequested**](/uwp/api/windows.ui.xaml.controls.maps.custommaptiledatasource.bitmaprequested) イベントのカスタム ハンドラーで、[**MapTileBitmapRequestedEventArgs**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileBitmapRequestedEventArgs) の [**X**](/uwp/api/windows.ui.xaml.controls.maps.maptilebitmaprequestedeventargs.x) プロパティ、[**Y**](/uwp/api/windows.ui.xaml.controls.maps.maptilebitmaprequestedeventargs.y) プロパティ、[**ZoomLevel**](/uwp/api/windows.ui.xaml.controls.maps.maptilebitmaprequestedeventargs.zoomlevel) プロパティにより必要なカスタム引数を組み合わせます。
2.  [**MapTileBitmapRequestedEventArgs**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileBitmapRequestedEventArgs) の [**Request**](/uwp/api/windows.ui.xaml.controls.maps.maptilebitmaprequestedeventargs.request) プロパティに含まれている [**MapTileBitmapRequest**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileBitmapRequest) の [**PixelData**](/uwp/api/windows.ui.xaml.controls.maps.maptilebitmaprequest.pixeldata) プロパティにカスタム タイルを返します。 **PixelData** プロパティの型は [**IRandomAccessStreamReference**](/uwp/api/Windows.Storage.Streams.IRandomAccessStreamReference) です。

次の例では、**BitmapRequested** イベントのカスタム ハンドラーを作成することによりカスタム タイルを指定する方法を示しています。 この例では、一部が不透明な赤の同じタイルを作成します。 この例では、[**MapTileBitmapRequestedEventArgs**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileBitmapRequestedEventArgs) の [**X**](/uwp/api/windows.ui.xaml.controls.maps.maptilebitmaprequestedeventargs.x) プロパティ、[**Y**](/uwp/api/windows.ui.xaml.controls.maps.maptilebitmaprequestedeventargs.y) プロパティ、[**ZoomLevel**](/uwp/api/windows.ui.xaml.controls.maps.maptilebitmaprequestedeventargs.zoomlevel) プロパティを無視します。 これは実際の例ではありませんが、メモリ内で随時にカスタム タイルを作成する方法を示しています。 この例ではまた、カスタム タイルを作成するために非同期処理が必要な場合に、保留パターンを実装する方法を示しています。

```csharp
using Windows.UI.Xaml.Controls.Maps;
using Windows.Storage.Streams;
using System.Threading.Tasks;
...
        CustomMapTileDataSource customDataSource = new CustomMapTileDataSource();
        // Attach a handler for the BitmapRequested event.
        customDataSource.BitmapRequested += customDataSource_BitmapRequestedAsync;
        customTileSource = new MapTileSource(customDataSource);
        MapControl1.TileSources.Add(customTileSource);
...
        // Handle the BitmapRequested event.
        private async void customDataSource_BitmapRequestedAsync(
            CustomMapTileDataSource sender,
            MapTileBitmapRequestedEventArgs args)
        {
            var deferral = args.Request.GetDeferral();
            args.Request.PixelData = await CreateBitmapAsStreamAsync();
            deferral.Complete();
        }

        // Create the custom tiles.
        // This example creates red tiles that are partially opaque.
        private async Task<RandomAccessStreamReference> CreateBitmapAsStreamAsync()
        {
            int pixelHeight = 256;
            int pixelWidth = 256;
            int bpp = 4;

            byte[] bytes = new byte[pixelHeight * pixelWidth * bpp];

            for (int y = 0; y < pixelHeight; y++)
            {
                for (int x = 0; x < pixelWidth; x++)
                {
                    int pixelIndex = y * pixelWidth + x;
                    int byteIndex = pixelIndex * bpp;

                    // Set the current pixel bytes.
                    bytes[byteIndex] = 0xff;        // Red
                    bytes[byteIndex + 1] = 0x00;    // Green
                    bytes[byteIndex + 2] = 0x00;    // Blue
                    bytes[byteIndex + 3] = 0x80;    // Alpha (0xff = fully opaque)
                }
            }

            // Create RandomAccessStream from byte array.
            InMemoryRandomAccessStream randomAccessStream =
                new InMemoryRandomAccessStream();
            IOutputStream outputStream = randomAccessStream.GetOutputStreamAt(0);
            DataWriter writer = new DataWriter(outputStream);
            writer.WriteBytes(bytes);
            await writer.StoreAsync();
            await writer.FlushAsync();
            return RandomAccessStreamReference.CreateFromStream(randomAccessStream);
        }
```

```cppwinrt
...
#include <winrt/Windows.Storage.Streams.h>
...
Windows::Foundation::IAsyncOperation<Windows::Storage::Streams::InMemoryRandomAccessStream> MainPage::CustomRandomAccessStream()
{
    constexpr int pixelHeight{ 256 };
    constexpr int pixelWidth{ 256 };
    constexpr int bpp{ 4 };

    std::array<uint8_t, pixelHeight * pixelWidth * bpp> bytes;

    for (int y = 0; y < pixelHeight; y++)
    {
        for (int x = 0; x < pixelWidth; x++)
        {
            int pixelIndex{ y * pixelWidth + x };
            int byteIndex{ pixelIndex * bpp };

            // Set the current pixel bytes.
            bytes[byteIndex] = (byte)(std::rand() % 256);        // Red
            bytes[byteIndex + 1] = (byte)(std::rand() % 256);    // Green
            bytes[byteIndex + 2] = (byte)(std::rand() % 256);    // Blue
            bytes[byteIndex + 3] = (byte)((std::rand() % 56) + 200);    // Alpha (0xff = fully opaque)
        }
    }

    // Create RandomAccessStream from byte array.
    Windows::Storage::Streams::InMemoryRandomAccessStream randomAccessStream;
    Windows::Storage::Streams::IOutputStream outputStream{ randomAccessStream.GetOutputStreamAt(0) };
    Windows::Storage::Streams::DataWriter writer{ outputStream };
    writer.WriteBytes(bytes);

    co_await writer.StoreAsync();
    co_await writer.FlushAsync();

    co_return randomAccessStream;
}
...
```

```cpp
InMemoryRandomAccessStream^ TileSources::CustomRandomAccessStream::get()
{
    int pixelHeight = 256;
    int pixelWidth = 256;
    int bpp = 4;

    Array<byte>^ bytes = ref new Array<byte>(pixelHeight * pixelWidth * bpp);

    for (int y = 0; y < pixelHeight; y++)
    {
        for (int x = 0; x < pixelWidth; x++)
        {
            int pixelIndex = y * pixelWidth + x;
            int byteIndex = pixelIndex * bpp;

            // Set the current pixel bytes.
            bytes[byteIndex] = (byte)(std::rand() % 256);        // Red
            bytes[byteIndex + 1] = (byte)(std::rand() % 256);    // Green
            bytes[byteIndex + 2] = (byte)(std::rand() % 256);    // Blue
            bytes[byteIndex + 3] = (byte)((std::rand() % 56) + 200);    // Alpha (0xff = fully opaque)
        }
    }

    // Create RandomAccessStream from byte array.
    InMemoryRandomAccessStream^ randomAccessStream = ref new InMemoryRandomAccessStream();
    IOutputStream^ outputStream = randomAccessStream->GetOutputStreamAt(0);
    DataWriter^ writer = ref new DataWriter(outputStream);
    writer->WriteBytes(bytes);

    create_task(writer->StoreAsync()).then([writer](unsigned int)
    {
        create_task(writer->FlushAsync());
    });

    return randomAccessStream;
}
```

## <a name="replace-the-default-map"></a>既定の地図の置き換え

既定の地図をサード パーティ製タイルまたはカスタム タイルに完全に置き換えるには、次の手順に従います。

-   [**MapTileSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileSource) の [**Layer**](/uwp/api/windows.ui.xaml.controls.maps.maptilesource.layer) プロパティ値として [**MapTileLayer**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileLayer).**BackgroundReplacement** を指定します。
-   [**MapControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) の [**Style**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.style) プロパティ値として [**MapStyle**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapStyle).**None** を指定します。

## <a name="related-topics"></a>関連トピック

* [Bing Maps Developer Center](https://www.bingmapsportal.com/)
* [UWP の地図のサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)
* [地図の設計ガイドライン](./display-maps.md)
* [ビルド2015ビデオ: Windows アプリでの電話、タブレット、および PC でのマップと場所の活用](https://channel9.msdn.com/Events/Build/2015/2-757)
* [UWP の交通情報アプリのサンプル](https://github.com/Microsoft/Windows-appsample-trafficapp)