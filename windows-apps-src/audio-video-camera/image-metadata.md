---
ms.assetid: D5D98044-7221-4C2A-9724-56E59F341AB0
description: この記事では、画像のメタデータ プロパティを読み取ったり書き込んだりする方法のほか、GeotagHelper ユーティリティ クラスを使ってファイルに位置情報タグを設定する方法について説明します。
title: イメージのメタデータ
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c020a2ca66c81bee81813402e546fc01ce77c7f3
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/02/2020
ms.locfileid: "89362565"
---
# <a name="image-metadata"></a>イメージのメタデータ



この記事では、画像のメタデータ プロパティを読み取ったり書き込んだりする方法のほか、[**GeotagHelper**](/uwp/api/Windows.Storage.FileProperties.GeotagHelper) ユーティリティ クラスを使ってファイルに位置情報タグを設定する方法について説明します。

## <a name="image-properties"></a>イメージのプロパティ

ファイルの内容に関連した情報には、[**StorageFile.Properties**](/uwp/api/windows.storage.storagefile.properties) プロパティから返される [**StorageItemContentProperties**](/uwp/api/Windows.Storage.FileProperties.StorageItemContentProperties) オブジェクトを使ってアクセスできます。 画像に固有のプロパティを取得するには、[**GetImagePropertiesAsync**](/uwp/api/windows.storage.fileproperties.storageitemcontentproperties.getimagepropertiesasync) を呼び出します。 それによって返される [**ImageProperties**](/uwp/api/Windows.Storage.FileProperties.ImageProperties) オブジェクトは、画像のタイトルやキャプチャの日付など、基本的な画像メタデータのフィールドを含んだメンバーを公開します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/MainPage.xaml.cs" id="SnippetGetImageProperties":::

さらに広範なファイル メタデータにアクセスするには、一意の文字列識別子で取得できるファイル メタデータ プロパティが集約された Windows プロパティ システムを使います。 文字列のリストを作成し、取得する必要のある各プロパティの識別子を追加してください。 [**ImageProperties.RetrievePropertiesAsync**](/uwp/api/windows.storage.fileproperties.imageproperties.retrievepropertiesasync) メソッドは、この文字列のリストを引数として受け取ってキー/値ペアのディクショナリを返します。このディクショナリのキーがプロパティ識別子で、ディクショナリの値がそのプロパティの値になります。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/MainPage.xaml.cs" id="SnippetGetWindowsProperties":::

-   各プロパティの識別子と型を含む、Windows のプロパティの完全な一覧については、「 [windows のプロパティ](/windows/desktop/properties/props)」を参照してください。

-   一部のプロパティは、特定のファイル コンテナーや特定の画像コーデックでのみサポートされます。 画像の種類ごとのサポートされるメタデータについては、「[フォト メタデータ ポリシー](/windows/desktop/wic/photo-metadata-policies)」をご覧ください。

-   サポート対象外のプロパティを取得しようとすると null 値が返される場合があります。返されたメタデータの値を使う前に必ず、null のチェックを行ってください。

## <a name="geotag-helper"></a>位置情報タグ ヘルパー

GeotagHelper は、地理データを含んだ画像へのタグ付けを支援するユーティリティ クラスです。[**Windows.Devices.Geolocation**](/uwp/api/Windows.Devices.Geolocation) API を直接使って簡単にタグを設定することができます。メタデータの形式を手動で解析したり構築したりする必要はありません。

以前に位置情報 Api またはその他のソースを使用したときに、イメージ内でタグ付けする場所を表す [**Geopoint**](/uwp/api/Windows.Devices.Geolocation.Geopoint) オブジェクトが既にある場合は、 [**Geotaghelper. SetGeotagAsync**](/uwp/api/windows.storage.fileproperties.geotaghelper.setgeotagasync) を呼び出して、 [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) と **geopoint**に渡すことで、ジオタグデータを設定できます。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/MainPage.xaml.cs" id="SnippetSetGeoDataFromPoint":::

デバイスの現在位置を使って位置情報タグ データを設定するには、[**Geolocator**](/uwp/api/Windows.Devices.Geolocation.Geolocator) オブジェクトを新たに作成し、[**GeotagHelper.SetGeotagFromGeolocatorAsync**](/uwp/api/windows.storage.fileproperties.geotaghelper.setgeotagfromgeolocatorasync) の引数に **Geolocator** とタグの設定対象となるファイルとを指定して呼び出します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/MainPage.xaml.cs" id="SnippetSetGeoDataFromGeolocator":::

-   [**SetGeotagFromGeolocatorAsync**](/uwp/api/windows.storage.fileproperties.geotaghelper.setgeotagfromgeolocatorasync) API を使うには、アプリ マニフェストに**位置情報**デバイス機能を追加する必要があります。

-   [**SetGeotagFromGeolocatorAsync**](/uwp/api/windows.storage.fileproperties.geotaghelper.setgeotagfromgeolocatorasync) を呼び出す前に [**RequestAccessAsync**](/uwp/api/windows.devices.geolocation.geolocator.requestaccessasync) を呼び出し、ユーザーの位置情報をアプリで使うための許可を得ておく必要があります。

-   位置情報 Api の詳細については、「 [マップと場所](../maps-and-location/index.md)」を参照してください。

位置情報タグで示された画像ファイルの地理的位置を表す GeoPoint を取得するには、[**GetGeotagAsync**](/uwp/api/windows.storage.fileproperties.geotaghelper.getgeotagasync) を呼び出します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/MainPage.xaml.cs" id="SnippetGetGeoData":::

## <a name="decode-and-encode-image-metadata"></a>画像メタデータのデコードとエンコード

画像データを操作する最も高度な方法は、[**BitmapDecoder**](/uwp/api/Windows.Graphics.Imaging.BitmapDecoder) または [BitmapEncoder](bitmapencoder-options-reference.md) を使って、プロパティの読み取りと書き込みをストリーム レベルで行うことです。 これらの操作では、読み取りまたは書き込みの対象データを Windows プロパティを使って指定できるほか、要求するプロパティのパスを Windows Imaging Component (WIC) のメタデータ クエリ言語を使って指定することもできます。

この方法で画像のメタデータを読み取るには、ソース画像ファイル ストリームを使って作成された [**BitmapDecoder**](/uwp/api/Windows.Graphics.Imaging.BitmapDecoder) が必要です。 この方法については、「[イメージング](imaging.md)」をご覧ください。

デコーダーを取得したら、文字列のリストを作成し、Windows プロパティの識別子文字列または WIC メタデータ クエリを使って、取得する各メタデータ プロパティの新しいエントリを追加します。 特定のプロパティを要求するには、デコーダーの [**BitmapProperties**](/uwp/api/Windows.Graphics.Imaging.BitmapProperties) メンバーの [**BitmapPropertiesView.GetPropertiesAsync**](/uwp/api/windows.graphics.imaging.bitmappropertiesview.getpropertiesasync) メソッドを呼び出します。 要求したプロパティが、プロパティ名 (またはパス) とプロパティ値を含んだキー/値ペアのディクショナリとして返されます。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/MainPage.xaml.cs" id="SnippetReadImageMetadata":::

-   WIC メタデータ クエリ言語とサポートされるプロパティについては、「[WIC ネイティブ イメージ形式メタデータのクエリ](/windows/desktop/wic/-wic-native-image-format-metadata-queries)」をご覧ください。

-   メタデータのプロパティの多くは、サポートされる画像の種類に限りがあります。 デコーダーに関連付けられている画像が、要求したプロパティのいずれかをサポートしていない場合、[**GetPropertiesAsync**](/uwp/api/windows.graphics.imaging.bitmappropertiesview.getpropertiesasync) はエラー コード 0x88982F41 で失敗します。画像がどのメタデータもサポートしていない場合は、0x88982F81 で失敗します。 これらのエラーコードに関連付けられている定数は、WINCODEC の \_ err \_ PROPERTYNOTSUPPORTED と wincodec の \_ err \_ UNSUPPORTEDOPERATION であり、winerror.h ヘッダーファイルで定義されています。
-   特定のプロパティの値が画像に存在するかどうかはわからないので、**IDictionary.ContainsKey** を使って、結果にプロパティが存在するかどうかを確かめたうえでアクセスしてください。

画像のメタデータをストリームに書き込むには、画像の出力ファイルに関連付けられている **BitmapEncoder** が必要です。

設定対象プロパティの値を保持する [**BitmapPropertySet**](/uwp/api/Windows.Graphics.Imaging.BitmapPropertySet) オブジェクトを作成します。 プロパティの値を表す [**BitmapTypedValue**](/uwp/api/Windows.Graphics.Imaging.BitmapTypedValue) オブジェクトを作成します。 このオブジェクトでは、値の型を定義する [**PropertyType**](/uwp/api/Windows.Foundation.PropertyType) 列挙型の値およびメンバーとして **object** を使います。 **BitmapTypedValue** を **BitmapPropertySet** に追加したうえで、[**BitmapProperties.SetPropertiesAsync**](/uwp/api/windows.graphics.imaging.bitmapproperties.setpropertiesasync) を呼び出すと、エンコーダーがプロパティをストリームに書き込みます。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/MainPage.xaml.cs" id="SnippetWriteImageMetadata":::

-   イメージファイルの種類でサポートされるプロパティの詳細については、「 [Windows プロパティ](/windows/desktop/properties/props)」、「 [フォトメタデータポリシー](/windows/desktop/wic/photo-metadata-policies)」、および「 [WIC イメージ形式のネイティブメタデータクエリ](/windows/desktop/wic/-wic-native-image-format-metadata-queries)」を参照してください。

-   エンコーダーに関連付けられている画像が、要求したプロパティのいずれかをサポートしていない場合、[**SetPropertiesAsync**](/uwp/api/windows.graphics.imaging.bitmapproperties.setpropertiesasync) はエラー コード 0x88982F41 で失敗します。

## <a name="related-topics"></a>関連トピック

* [イメージング](imaging.md)
 

 
