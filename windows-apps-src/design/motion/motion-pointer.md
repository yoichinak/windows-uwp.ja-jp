---
Description: ポインター アニメーションを使って、ユーザーが項目をタップまたはクリックしたときに視覚的なフィードバックをユーザーに提供します。
title: ポインター クリックのアニメーション
ms.assetid: EEB10A2C-629A-4705-8468-4D019D74DDFF
ms.date: 08/09/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 32c3460b1af226adfa969d02a695f1d6436390be
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970547"
---
# <a name="pointer-click-animations"></a>ポインター クリックのアニメーション



ポインター アニメーションを使って、ユーザーが項目をタップまたはクリックしたときに視覚的なフィードバックをユーザーに提供します。 ポインター ダウン アニメーション (押された項目を若干縮小して傾ける) は、項目が最初にタップされたときに再生されます。 ポインター アップ アニメーション (項目を元の位置に復元する) は、ユーザーがポインターから指を離したときに再生されます。


> **重要な API**: [**PointerUpThemeAnimation クラス**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PointerUpThemeAnimation)、[**PointerDownThemeAnimation クラス**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PointerDownThemeAnimation)


## <a name="dos-and-donts"></a>推奨と非推奨

-   ポインター アップ アニメーションを使うときには、ユーザーが指を離した直後にアニメーションを開始するようにします。 これにより、タップによってトリガーされたアクション (新しいページへの移動など) の応答が遅れたとしても、ユーザーの操作が認識されたというフィードバックを即座に返すことができます。

## <a name="related-articles"></a>関連記事

* [アニメーションの概要](https://docs.microsoft.com/windows/uwp/graphics/animations-overview)
* [ポインター クリックのアニメーション化](https://docs.microsoft.com/previous-versions/windows/apps/jj649432(v=win.10))
* [クイック スタート: ライブラリのアニメーションを使った UI のアニメーション化](https://docs.microsoft.com/previous-versions/windows/apps/hh452703(v=win.10))
* [**PointerUpThemeAnimation クラス**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PointerUpThemeAnimation)
* [**PointerDownThemeAnimation クラス**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PointerDownThemeAnimation)

 

 




