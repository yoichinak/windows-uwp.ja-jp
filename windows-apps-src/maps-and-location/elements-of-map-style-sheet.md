---
description: マップ スタイル シートのエントリとプロパティ
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: マップ スタイル シート リファレンス
ms.date: 03/19/2017
ms.topic: article
keywords: Windows 10, UWP, マップ, マップ スタイル シート
ms.localizationpriority: medium
ms.openlocfilehash: ec30fd5d63dfa522ef721d2d8a2e4950e6e8e854
ms.sourcegitcommit: c9edb164356c0843d8a9b19ab43707d486e392e1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/25/2020
ms.locfileid: "85377933"
---
# <a name="map-style-sheet-reference"></a>マップ スタイル シート リファレンス

Microsoft のマッピングテクノロジでは、[マップスタイルシート](https://docs.microsoft.com/BingMaps/styling/map-style-sheets)を使用して、マップの外観を定義します。これには、Windows ストアアプリケーションの[mapcontrol](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol)に表示されるものも含まれます。  マップスタイルシートは、 [Mapstylesheet. ParseFromJson](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapstylesheet.parsefromjson#Windows_UI_Xaml_Controls_Maps_MapStyleSheet_ParseFromJson_System_String_)メソッドを介して JAVASCRIPT OBJECT NOTATION (JSON) を使用して定義されます。

スタイルシートは、マップの最終的な外観を変更するためにプロパティがオーバーライドされた[エントリ](https://docs.microsoft.com/BingMaps/styling/map-style-sheet-entries)で構成されます。

たとえば、次の JSON を使用して、水の領域を赤で表示し、水ラベルを緑色で表示し、領域を青で表示することができます。

```json
    {"version":"1.*",
        "settings":{"landColor":"#0000FF"},
        "elements":{"water":{"fillColor":"#FF0000","labelColor":"#00FF00"}}
    }
```

スタイルシートは、[マップスタイルシートエディター](https://www.microsoft.com/p/map-style-sheet-editor/9nbhtcjt72ft)アプリケーションを使用して対話形式で作成できます。
