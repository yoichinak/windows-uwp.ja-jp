---
description: マップ スタイル シートのエントリとプロパティ
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: マップ スタイル シート リファレンス
ms.date: 03/19/2017
ms.topic: article
keywords: Windows 10, UWP, マップ, マップ スタイル シート
ms.localizationpriority: medium
ms.openlocfilehash: b59e8c3c6d9c4c299e441964be1afb4e02051e23
ms.sourcegitcommit: 5264d7499ddbe21199a63d74a294206069f90f8b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/04/2020
ms.locfileid: "78287452"
---
# <a name="map-style-sheet-reference"></a>マップ スタイル シート リファレンス

Microsoft マッピングテクノロジでは、_マップスタイルシート_を使用してマップの外観を定義します。  マップスタイルシートは JavaScript Object Notation (JSON) を使用して定義されており、さまざまな方法で使用できます。たとえば、Windows ストアアプリケーションの[Mapcontrol](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol)で[Mapcontrol. parsefromjson](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapstylesheet.parsefromjson#Windows_UI_Xaml_Controls_Maps_MapStyleSheet_ParseFromJson_System_String_)メソッドを使用します。

スタイルシートは、[マップスタイルシートエディター](https://www.microsoft.com/p/map-style-sheet-editor/9nbhtcjt72ft)アプリケーションを使用して対話形式で作成できます。

次の JSON を使用して、水の領域を赤で表示し、水ラベルを緑色で表示し、領域を青で表示することができます。

```json
    {"version":"1.*",
        "settings":{"landColor":"#0000FF"},
        "elements":{"water":{"fillColor":"#FF0000","labelColor":"#00FF00"}}
    }
```

この JSON を使用して、マップからすべてのラベルとポイントを削除できます。

```json

    {"version":"1.*", "elements":{"mapElement":{"labelVisible":false},"point":{"visible":false}}}
```

最終的な結果を生成するために、プロパティの値が変換される場合があります。  たとえば、表示されるエンティティの種類によって vegetation fillColor の影がやや異なります。  ignoreTransform プロパティを使用することで、この動作はオフにすることができます。それにより、指定の正確な値が使用されます。

```json
    {"version":"1.*",
        "settings":{"shadedReliefVisible":false},
        "elements":{"vegetation":{"fillColor":{"value":"#999999","ignoreTransform":true}}}
    }
```

このトピックでは、地図の外観をカスタマイズするために使用できる JSON のエントリと[プロパティ](#properties)を示します。  これらのプロパティは、 [MapStyleSheetEntry](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapelement.mapstylesheetentry)プロパティを介してユーザーマップ要素に適用することもできます。

<a id="entries" />

## <a name="entries"></a>エントリ
この表では、文字 ">" を使用してエントリ階層内のレベルを表しています。  また、各エントリがどのバージョンの Windows でサポートされ、それを無視するかを示します。

| バージョン | Windows のリリース名 |
|---------|----------------------|
|  1703   | Creators Update      |
|  1709   | Fall Creators Update |
|  1803   | April 2018 Update    |
|  1809   | 2018年10月の更新  |
|  1903   | 2019年5月の更新      |

| Name                               | プロパティ グループ            | 1703 | 1709 | 1803 | 1809 | 1903 | 説明    |
|------------------------------------|---------------------------|------|------|------|------|------|----------------|
| バージョン                            | [バージョン](#version)       |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 使用するスタイル シートのバージョン。 |
| 設定                           | [設定](#settings)     |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | スタイル シート全体に適用される設定。 |
| mapElement                         | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 地図のすべてのエントリの親エントリ。 |
| > baseMapElement                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | すべての非ユーザー エントリの親エントリ。 |
| >> area                            | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 土地の使用を説明する領域。  これらは、structure エントリに含まれる物理的な建物と混同しないようにしてください。 |
| >>> airport                        | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 空港をカバーする領域。 |
| >>> areaOfInterest                 | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   |  ✔   | 企業や興味深いポイントが高度に集中しているエリアです。 |
| >>> cemetery                       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Cemeteries を含む領域。 |
| >>> continent                      | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 大陸の領域のラベル。 |
| >>> education                      | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 学校やその他の教育施設を含む領域。 |
| >>> indigenousPeoplesReserve       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Indigenous の予約を含む区分。 |
| >>> industrial                     | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   |  ✔   | 産業目的で使用される領域。 |
| >>> island                         | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 島領域のラベル。 |
| >>> medical                        | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 医療目的 (例: 病院キャンパス) に使用される領域。 |
| >>> military                       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 軍事ベースまたは軍事分野が使用されている領域。 |
| >>> nautical                       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 海里に関連する目的で使用される領域。 |
| >>> neighborhood                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 隣接領域のラベル。 |
| >>> runway                         | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 飛行機の滑走路として使用される領域。 |
| >>> sand                           | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 海辺のような砂地の領域。 |
| >>> shoppingCenter                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | モールやその他のショッピング センター用に割り当てられた土地の領域。 |
| >>> stadium                        | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | スタジアムを含む領域。 |
| >>> underground                    | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   |  ✔   | 地下のエリア (例: 地下鉄の駅の専有面積)。 |
| >>> vegetation                     | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 森林、草原領域など。 |
| >>>> forest                        | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 森林の領域。 |
| >>>> golfCourse                    | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | ゴルフコースを含む区分。 |
| >>>> park                          | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 公園を含む領域。 |
| >>>> playingField                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 野球場やテニス コートなどの競技場。 |
| >>>> reserve                       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 自然予約を含む領域。 |
| > > frozenWater                     | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | "氷河" のような固定水。 |
| >> point                           | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | ある種のアイコンで描画されるすべてのポイント機能。 |
| >>> address                        | [PointStyle](#pointstyle) |      |      |  ✔   |  ✔   |  ✔   | アドレス番号ラベル。 |
| >>> naturalPoint                   | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 自然な特徴を表すアイコン。 |
| >>>> peak                          | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 山頂を表すアイコン。 |
| >>>>> volcanicPeak                 | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 火山の山頂を表すアイコン。 |
| >>>> waterPoint                    | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 滝などの水に関連する場所を表すアイコン。 |
| >>> pointOfInterest                | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 興味深い場所を表すアイコン。 |
| >>>> business                      | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 任意の事業所を表すアイコン。 |
| > > > > > attractionPoint              | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   |  ✔   | 美術館、zoos などの tourist アトラクションを表すアイコン。 |
| > > > > > > Amusementplace ポイント         | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 専用 place アイコン。 |
| > > > > > > aquariumPoint               | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 水族館アイコン。 |
| > > > > > > artGalleryPoint             | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | アートギャラリーアイコン。 |
| > > > > > > キャンプポイント                   | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | キャンプアイコン。 |
| > > > > > > fishingPoint                | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 釣りアイコン。 |
| > > > > > > gardenPoint                 | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 庭園アイコン。 |
| > > > > > >                 | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | ハイキングアイコン。 |
| > > > > > > libraryPoint                | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | ライブラリアイコン。 |
| > > > > > > museumPoint                 | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 博物館アイコン。 |
| > > > > > > naturalPlacePoint           | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 自然な場所のアイコン。 |
| > > > > > > parkPoint                   | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | パークアイコン。 |
| > > > > > > restAreaPoint               | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Rest 領域アイコン。 |
| > > > > > > touristAttractionPoint      | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Tourist 引力アイコン。 |
| > > > > > > zooPoint                    | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Zoo アイコン。 |
| > > > > > communityPoint               | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   |  ✔   | コミュニティへの一般的な使用場所を表すアイコン。 |
| > > > > > > conventionCenterPoint       | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 規則センターのアイコン。 |
| > > > > > > financialPoint              | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 財務アイコン。 |
| > > > > > > governmentPoint             | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Government アイコン。 |
| > > > > > > Informationpoint  | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 情報技術アイコン。 |
| > > > > > > palacePoint                 | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Palace アイコン。 |
| > > > > > > pollingStationPoint         | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | ポーリングステーションのアイコン。 |
| > > > > > > researchPoint               | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 調査アイコン。 |
| > > > > > educationPoint               | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   |  ✔   | 学校およびその他の教育関連の場所を表すアイコン。 |
| > > > > > entertainmentPoint           | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   |  ✔   | シアター、cinemas などのエンターテインメント会場を表すアイコン。 |
| > > > > > > artsPoint                   | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | アートアイコン。 |
| > > > > > > bowlingPoint                | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | ボウリングアイコン。 |
| > > > > > > casinoPoint                 | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | カジノアイコン。 |
| > > > > > > golfCoursePoint             | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | ゴルフコースアイコン。 |
| > > > > > > gymPoint                    | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | ジムアイコン。 |
| > > > > > > marinaPoint                 | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Marina アイコン。 |
| > > > > > > movieTheaterPoint           | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | ムービーシアターアイコン。 |
| > > > > > > nightClubPoint              | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 夜のクラブのアイコン。 |
| > > > > > > 再署名ポイント             | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 再娯楽アイコン。 |
| > > > > > > Sk/ポイント                | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Skating アイコン。 |
| > > > > > > skiAreaPoint                | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Ski 区分アイコン。 |
| > > > > > > stadiumPoint                | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | プールアイコン。 |
| > > > > > > swimmingPoolPoint           | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | プールアイコン。 |
| > > > > > >                | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | シアターアイコン。 |
| > > > > > > wineryPoint                 | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | % のアイコン。 |
| > > > > > essentialServicePoint        | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   |  ✔   | 駐車、銀行、ガスなどの重要なサービスを表すアイコン。 |
| > > > > > > aTMPoint                    | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | ATM アイコン。 |
| > > > > > > automobileRentalPoint       | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 自動車レンタルアイコン。 |
| > > > > > > automobileRepairPoint       | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 自動車修理アイコン。 |
| > > > > > > bankPoint                   | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 銀行のアイコン。 |
| > > > > > > clinicPoint                 | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | クリニックアイコン。 |
| > > > > > > electricChargingStationPoint| [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 電気請求ステーションのアイコン。 |
| > > > > > > 焼討ステーションポイント            | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | "焼討ステーション" アイコン。 |
| > > > > > > gasStationPoint             | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | GasStation アイコン。 |
| > > > > > > groceryPoint                | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 食料のアイコン。 |
| > > > > > > hospitalPoint               | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 病院アイコン。 |
| > > > > > > laundryPoint                | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 洗濯アイコン。 |
| > > > > > > liquorAndWineStorePoint     | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Liquor とワインストアのアイコン。 |
| > > > > > メールポイント                   | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | メールアイコン。 |
| > > > > > > marketPoint                 | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 市場アイコン。 |
| > > > > > >                | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 駐車アイコン。 |
| > > > > > > petsPoint                   | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | ペットアイコン。 |
| > > > > > > pharmacyPoint               | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 薬剤アイコン。 |
| > > > > > > policePoint                 | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 警察アイコン。 |
| > > > > > > postalServicePoint          | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 郵便サービスアイコン。 |
| > > > > > > professionalPoint           | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Professional サービスアイコン。 |
| > > > > > > toiletPoint                 | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | トイレットアイコン。 |
| > > > > > > veterinarianPoint           | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Veterinarian アイコン。 |
| >>>>> foodPoint                    | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | レストラン、カフェなどを表すアイコン |
| > > > > > > barAndGrillPoint            | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Bar とグリルアイコン。 |
| > > > > > > barPoint                    | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 横棒アイコン。 |
| > > > > > > breweryPoint                | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | アイコンがあります。 |
| > > > > > > cafePoint                   | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | カフェアイコン。 |
| > > > > > > iceCreamShopPoint           | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | アイスクリームショップのアイコン。 |
| > > > > > > restaurantPoint             | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | レストランアイコン。 |
| > > > > > > teaShopPoint                | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | TeaShop アイコン。 |
| > > > > > lodgingPoint                 | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   |  ✔   | ホテルやその他の宿泊事業を表すアイコン。 |
| > > > > > > gotelPoint                  | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | ホテルアイコン。 |
| > > > > > realEstatePoint              | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   |  ✔   | 不動産企業を表すアイコン。 |
| > > > > > shoppingPoint                | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   |  ✔   | ホテルやその他の宿泊事業を表すアイコン。 |
| > > > > > > automobileDealerPoint       | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 自動車ディーラーアイコン。 |
| > > > > > > beautyAndSpaPoint           | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 美しさと spa アイコン。 |
| > > > > > > bookstorePoint              | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 書店アイコン。 |
| > > > > > > departmentStorePoint        | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 部署ストアアイコン。 |
| > > > > > > electronicsShopPoint        | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | エレクトロニクスショップのアイコン。 |
| > > > > > > golfShopPoint               | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | ゴルフショップアイコン。 |
| > > > > > > homeApplianceShopPoint      | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | ホームアプライアンスのショップアイコン。 |
| > > > > > > mallPoint                   | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | モールアイコン。 |
| > > > > > > phoneShopPoint              | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 電話ショップのアイコン。 |
| >>> populatedPlace                 | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 住民のいる場所のサイズを表すアイコン (例: 市区町村)。 |
| >>>> capital                       | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 住民のいる場所の首都を表すアイコン。 |
| >>>>> adminDistrictCapital         | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 州の州都や都道府県の県庁所在地を表すアイコン。 |
| > > > > > > majorAdminDistrictCapital   | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 都道府県の主要な首都を表すアイコン。 |
| > > > > > > minorAdminDistrictCapital   | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 都道府県の軽微な首都を表すアイコン。 |
| >>>>> countryRegionCapital         | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 国や地域の首都を表すアイコン。 |
| > > > > majorPopulatedPlace           | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 設定された主要な場所のサイズを表すアイコン。 |
| > > > > minorPopulatedPlace           | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 設定された小さな位置のサイズを表すアイコン。 |
| >>> roadShield                     | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 道路の簡略化された名前を表す記号 (例: I-5)。 settings エントリの **ImageFamily** プロパティを *Palette* の値に設定している場合は、パレット値のみを使用します。 |
| >>> roadExit                       | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 通常、通行が管理された高速道路の出口を表すアイコン。 |
| >>> transit                        | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | バスの停留所、鉄道の駅、空港などを表すアイコン。 |
| >> political                       | [BorderedMapElement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 国、地域、州などの政治的な区域。 |
| >>> countryRegion                  | [BorderedMapElement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 国の地域の境界とラベル。 |
| >>> adminDistrict                  | [BorderedMapElement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Admin1、州、県など。罫線とラベル。 |
| >>> district                       | [BorderedMapElement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Admin2 の、郡、その他、罫線とラベル。 |
| >> structure                       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 建物やその他の建物のような構造体。 |
| >>> building                       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 建物. |
| >>>> educationBuilding             | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 教育に使用される建物。 |
| >>>> medicalBuilding               | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 病院などの医療目的で使用される建物。 |
| >>>> transitBuilding               | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 空港などの転送に使用される建物。 |
| >> transportation                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 交通輸送網の一部である線 (例: 道路、鉄道、フェリー航路)。 |
| >>> road                           | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | すべての道路を表す線。 |
| >>>> controlledAccessHighway       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 大きい、制御されたアクセス幹線道路を表す線。 |
| >>>>> highSpeedRamp                | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 通常は、制御されたアクセスハイウェイに接続する高速傾斜を表す線。 |
| >>>> highway                       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 幹線道路を表す線。 |
| >>>> majorRoad                     | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 主要な道路を表す線。 |
| >>>> arterialRoad                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Arterial 道路を表す線。 |
| >>>> street                        | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 道路を表す線。 |
| >>>>> ramp                         | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 通常、ハイウェイに接続する傾斜を表す線。 |
| >>>>> unpavedStreet                | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Unpaved の道路を表す線。 |
| >>>> tollRoad                      | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 使用コストがかかる道路を表す線。 |
| >>> railway                        | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 鉄道の路線。 |
| >>> trail                          | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 公園内の遊歩道やハイキング コース。 |
| > > > ウォーク方法                        | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   |  ✔   | より高度なウォーク方法。 |
| >>> waterRoute                     | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | フェリー航路の線。 |
| >> water                           | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 水のように見えるものすべて。 これには海や河川が含まれます。 |
| >>> river                          | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 河川、小川、その他の水路。  これは線の場合も、多角形の場合もあり、線があり、河川以外の水域に接続している場合があることに注意してください。 |
| > routeMapElement                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | すべてのルーティング関連エントリ。 |
| >> routeLine                       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | ルート行に関連するエントリ。 |
| >>> drivingRoute                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 運転ルートを表す線。 |
| >>> scenicRoute                    | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   |  ✔   | 観光運転ルートを表す線。 |
| >>> walkingRoute                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | ウォーキングルートを表す線。 |
| > userMapElement                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | すべてのユーザーエントリ。 |
| >> userBillboard                   | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   |  ✔   | 既定の [MapBillboard](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard) インスタンスのスタイル。 |
| >> userLine                        | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 既定の [MapPolyline](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mappolyline) インスタンスのスタイル。 |
| >> userModel3D                     | [MapElement3D](#mapelement3d) |      |  ✔   |  ✔   |  ✔   |  ✔   | 既定の [MapModel3D](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapmodel3d) インスタンスのスタイル。  これは、主に renderAsSurface を設定するためのものです。 |
| >> userPoint                       | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 既定の [MapIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapicon) インスタンスのスタイル。 |

<a id="properties" />

## <a name="properties"></a>プロパティ

このセクションでは、エントリごとに使用できるプロパティについて説明します。

<a id="version" />

### <a name="version-properties"></a>Version のプロパティ

| プロパティ                     | 種類    | 説明                                                                                                           |
|------------------------------|---------|-----------------------------------------------------------------------------------------------------------------------|
| バージョン                      | String  | 対象のスタイル シートのバージョン。 適用性のために使用します。 既定では "1.0"、追加のマイナー機能更新では "1.*" になります。 |

<a id="settings" />

### <a name="settings-properties"></a>Settings のプロパティ

| プロパティ                     | 種類    | 1703 | 1709 | 1803 | 1809 | 1903 | 説明 |
|------------------------------|---------|------|------|------|------|------|-------------|
| atmosphereVisible            | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 大気が 3D コントロールに表示されるかどうかを示すフラグ。 |
| buildingTexturesVisible      | Bool    |      |      |  ✔   |  ✔   |  ✔   | テクスチャのあるシンボル 3D 施設にテクスチャを表示するかどうかを示すフラグ。 |
| fogColor                     | 色   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 3D コントロールに表示されるディスタンス フォグの ARGB カラー値。 |
| glowColor                    | 色   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | ラベルのグローやアイコンのグローに適用される可能性がある ARGB カラー値。 |
| imageFamily                  | String  |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | このスタイルに使用するよう設定されたイメージの名前。 実際の記号に基づいて固定色を使用する記号の場合は、この値を *Default* に設定します。 パレットで構成可能な色を使用する記号の場合は、この値を *Palette* に設定します。 |
| landColor                    | 色   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 陸地に何かを描画する前の陸地の ARGB カラー値。 |
| logosVisible                 | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | **Organization** プロパティを持つ項目に適切なロゴを描画するか、汎用のアイコンを使用するかを示すフラグ。 |
| officialColorVisible         | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 公式の色のプロパティを持っている項目 (中国での乗り換え線など) をその色を描画する必要があるかどうかを示すフラグ。 たとえば、白黒の地図ではこの値をオフにします。 |
| rasterRegionsVisible         | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | ベクター (日本と韓国) よりも表現が優れているラスター領域を描画するかどうかを示すフラグ。 |
| shadedReliefVisible          | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 地図上の高度シェーディングを描画するかどうかを示すフラグ。 |
| shadowColor                  | 色   |      |      |      |  ✔   |  ✔   | 影を使用する影の背景色。 |
| spaceColor                   | 色   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 地図の周囲の領域の ARGB カラー値。 |
| useDefaultImageColors        | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | イメージ内の色のパレットエントリを検索するのではなく、SVG の元の色を使用する必要があるかどうかを示すフラグ。 |

<a id="mapelement" />

### <a name="mapelement-properties"></a>MapElement のプロパティ

| プロパティ                     | 種類    | 1703 | 1709 | 1803 | 1809 | 1903 | 説明 |
|------------------------------|---------|------|------|------|------|------|-------------|
| backgroundScale              | 浮動小数点数   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | アイコンの背景要素を拡大縮小する量。  たとえば、既定の場合は *1* を、2 倍の大きさの場合は *2* を使用します。 |
| fillColor                    | 色   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 多角形の塗りつぶし、ポイント アイコンの背景、分割した場合の線の中心に使用される色。 |
| fontFamily                   | String  |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   |  |
| fontWeight                   | String  |      |      |      |      |  ✔   | ストロークの明るさまたは細さに関して、タイプフェイスの密度。 "**Light**"、"**Normal**"、"**SemiBold**"、および "**Bold**" を設定できます。 |
| iconColor                    | 色   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | ポイント アイコンの中央に表示されるグリフの色。 |
| iconScale                    | 浮動小数点数   |      |  ✔   |  ✔   |  ✔   |  ✔   | アイコンのグリフを拡大縮小する量。  たとえば、既定の場合は *1* を、2 倍の大きさの場合は *2* を使用します。 |
| labelColor                   | 色   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   |  |
| labelOutlineColor            | 色   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   |  |
| labelScale                   | 浮動小数点数   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 既定のラベル サイズが拡大縮小される量。 たとえば、既定の場合は *1* を、2 倍の大きさの場合は *2* を使用します。 |
| labelVisible                 | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   |  |
| overwriteColor               | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | **FillColor** のアルファ値で **StrokeColor** をブレンドするのではなく、上書きします。 |
| スケール                        | 浮動小数点数   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | ポイント全体のサイズを拡大縮小する量。 たとえば、既定の場合は *1* を、2 倍の大きさの場合は *2* を使用します。 |
| strokeColor                  | 色   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 多角形の輪郭、ポイント アイコンの輪郭、線の色に使用する色。 |
| strokeWidthScale             | 浮動小数点数   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 線の太さが拡大縮小される量。 たとえば、既定の場合は *1* を、2 倍の大きさの場合は *2* を使用します。 |
| visible                      | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   |  |

<a id="borderedmap" />

### <a name="borderedmapelement"></a>BorderedMapElement

このプロパティ グループは、[MapElement](#mapelement) プロパティ グループを継承します。

| プロパティ                     | 種類    | 1703 | 1709 | 1803 | 1809 | 1903 | 説明 |
|------------------------------|---------|------|------|------|------|------|-------------|
| borderOutlineColor           | 色   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 塗りつぶされた多角形の境界線のセカンダリまたはケーシング線の色。 |
| borderStrokeColor            | 色   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 塗りつぶされた多角形の境界線のプライマリ線の色。 |
| borderVisible                | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   |  |
| borderWidthScale             | 浮動小数点数   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 境界線の描画に使用する量。 たとえば、既定の場合は *1* を、2 倍の大きさの場合は *2* を使用します。 |

<a id="pointstyle" />

### <a name="pointstyle-properties"></a>PointStyle のプロパティ

このプロパティ グループは、[MapElement](#mapelement) プロパティ グループを継承します。

| プロパティ                     | 種類    | 1703 | 1709 | 1803 | 1809 | 1903 | 説明 |
|------------------------------|---------|------|------|------|------|------|-------------|
| shadowVisible                | Bool    |      |      |      |      |  ✔   | アイコンの影を表示するかどうかを示すフラグ |
| 図形-背景             | String  |      |      |      |      |  ✔   | アイコンの背景として使用する図形。存在する任意の図形を置換します。 |
| 図形-アイコン                   | String  |      |      |      |      |  ✔   | アイコンの前景グリフとして使用する図形。そこに存在する任意の図形を置換します。 |
| stemAnchorRadiusScale        | 浮動小数点数   |      |      |  ✔   |  ✔   |  ✔   | アイコン ステムのアンカー ポイントを拡大縮小する量。  たとえば、既定の場合は *1* を、2 倍の大きさの場合は *2* を使用します。 |
| stemColor                    | 色   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 3D モードでアイコンの下部から出ている幹の色。 |
| stemHeightScale              | 浮動小数点数   |      |      |  ✔   |  ✔   |  ✔   | アイコンのステムの長さを拡大縮小する量。  たとえば、既定の場合は *1* を、2 倍の長さの場合は *2* を使用します。 |
| stemOutlineColor             | 色   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 3D モードでアイコンの下部から出ている幹の周囲の輪郭の色。 |
| stemWidthScale               | 浮動小数点数   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | アイコンのステムの幅を拡大縮小する量。  たとえば、既定の場合は *1* を、2 倍の長さの場合は *2* を使用します。 |

<a id="mapelement3d" />

### <a name="mapelement3d"></a>MapElement3D

このプロパティ グループは、[MapElement](#mapelement) プロパティ グループを継承します。

| プロパティ                     | 種類    | 1703 | 1709 | 1803 | 1809 | 1903 | 説明 |
|------------------------------|---------|------|------|------|------|------|------------|
| renderAsSurface              | Bool    |      |      |  ✔   |  ✔   |  ✔   | 3D モデルを地面に対して深度フェーディングなしで建物のようにレンダリングする必要があることを示すフラグ。 |
