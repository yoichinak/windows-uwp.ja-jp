---
title: /users/xuid({xuid})/history/titles
assetID: 2f8eb79a-42c2-0267-cbf2-8682bb28f270
permalink: en-us/docs/xboxlive/rest/uri-titlehistoryusersxuidhistorytitlesv2.html
author: KevinAsgari
description: " /users/xuid({xuid})/history/titles"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Xbox Live, Xbox, ゲーム, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: a4780c92fc16adb697783ecee50d36523ff92998
ms.sourcegitcommit: 232543fba1fb30bb1489b053310ed6bd4b8f15d5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/25/2018
ms.locfileid: "4177508"
---
# <a name="usersxuidxuidhistorytitles"></a>/users/xuid({xuid})/history/titles
 
このユニバーサル Resource Identifier (URI) は、ユーザーのタイトルの実績に関連する履歴へのアクセスを提供します。
 
これらの Uri のドメインが`achievements.xboxlive.com`します。
 
<a id="ID4E1"></a>

 
## <a name="uri-parameters"></a>URI パラメーター
 
| パラメーター| 型| 説明| 
| --- | --- | --- | 
| xuid| 64 ビットの符号なし整数| Xbox ユーザー ID (XUID)、ユーザーがタイトル履歴にアクセスしているのです。| 
  
<a id="ID4EAC"></a>

 
## <a name="valid-methods"></a>有効なメソッド

[GET](uri-titlehistoryusersxuidhistorytitlesgetv2.md)

&nbsp;&nbsp;タイトルは、ユーザーがロックを解除またはに対するその実績の進行状況の一覧を取得します。 この API では、タイトルのプレイまたは起動のユーザーのすべての履歴は返されません。
 
<a id="ID4EKC"></a>

 
## <a name="see-also"></a>関連項目
 
<a id="ID4EMC"></a>

 
##### <a name="parent"></a>Parent 

[実績タイトル履歴 URI](atoc-reference-titlehistoryv2.md)

   