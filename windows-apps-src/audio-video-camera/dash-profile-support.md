---
ms.assetid: 3E0FBB43-F6A4-4558-AA89-20E7760BA73F
description: この記事では、UWP アプリでサポートされる Dynamic Adaptive Streaming over HTTP (DASH) プロファイルの一覧を示します。
title: Dynamic Adaptive Streaming over HTTP (DASH) プロファイルのサポート
ms.date: 02/15/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 268020c06be57d8ac300f1202046e52b6e1d2507
ms.sourcegitcommit: 789bfe3756c5c47f7324b96f482af636d12c0ed3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/09/2019
ms.locfileid: "68867382"
---
# <a name="dynamic-adaptive-streaming-over-http-dash-profile-support"></a>Dynamic Adaptive Streaming over HTTP (DASH) プロファイルのサポート


## <a name="supported-dash-profiles"></a>サポートされている DASH プロファイル
次の表では、UWP アプリでサポートされている DASH プロファイルを示します。

|Tag | マニフェストの種類 | メモ|7 月にリリースされた Windows 10|Windows 10 バージョン 1511|Windows 10 バージョン 1607 |Windows 10 バージョン 1607 |Windows 10 バージョン 1703| Windows 10 バージョン1809
|----------------|------|-------|-----------|--------------|---------|-------|--------|--------|
|urn:mpeg&#58;dash:profile:isoff-live:2011 | スタティック |     |Supported            |  Supported              | Supported        |Supported| Supported| Supported|
|urn:mpeg&#58;dash:profile:isoff-main:2011 |        | ベスト エフォート | Supported            |  Supported              | Supported        |Supported| Supported| Supported|
|urn:mpeg&#58;dash:profile:isoff-live:2011 | 動的 | セグメント テンプレートで $Time$ はサポートされていますが、$Number$ はサポートされていません。 | サポート非対象            | サポートされない              | サポートされない        |サポートされない| Supported| Supported|
|urn:mpeg&#58;dash:profile:isoff-on-demand:2011 |        |  | サポート非対象            |  サポートされない              | サポートされない        |サポートされない| サポートされない| Supported|


## <a name="unsupported-dash-profiles"></a>サポートされていない DASH プロファイル
上の表に示されていないプロファイルはサポートされていません。これには、次のようなものが含まれますが、これらに限定されません。

* urn:mpeg&#58;dash:profile:full:2011
* urn:mpeg&#58;dash:profile:mp2t-main:2011
* urn:mpeg&#58;dash:profile:mp2t-simple:2011


## <a name="related-topics"></a>関連トピック

* [メディア再生](media-playback.md)
* [アダプティブストリーミング](adaptive-streaming.md)
 

 




