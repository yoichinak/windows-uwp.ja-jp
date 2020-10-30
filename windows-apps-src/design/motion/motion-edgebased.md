---
description: エッジに基づく UI アニメーションでは、画面の端を起点とする UI の表示と非表示を切り替えられます。
title: エッジに基づく UI アニメーション
ms.assetid: 5A8F73B1-F4F6-424b-9EDF-A9766C5DEAE8
label: Motion--edge-based UI
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a49277a82a3bfffffb478496ab01d353c169463f
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034235"
---
# <a name="edge-based-ui-animations"></a>エッジに基づく UI アニメーション





エッジに基づく UI アニメーションでは、画面の端を起点とする UI の表示と非表示を切り替えられます。 この表示と非表示のアクションは、ユーザーが開始することも、アプリから開始することもできます。 UI は、アプリの手前に表示するか、メイン アプリ サーフェスの一部として表示することができます。 UI をアプリ サーフェスの一部として表示する場合は、UI を表示できるようにアプリの残りの部分のサイズを調整する必要があります。

> **重要な API** : [**EdgeUIThemeTransition クラス**](/uwp/api/Windows.UI.Xaml.Media.Animation.EdgeUIThemeTransition)


## <a name="dos-and-donts"></a>推奨と非推奨


-   画面領域をあまり占有しないカスタム メッセージ バーやエラー バーを表示または非表示にするには、エッジ (端) UI アニメーションを使います。
-   作業ウィンドウやカスタム ソフト キーボードなど、画面内側にスライドして領域を大きく確保する UI を表示するには、パネル アニメーションを使います。
-   UI を開くには、それが関連付けられている端から画面内側にスライドします。
-   UI を閉じるには、画面内側から、開いたときと同じ端に向かってスライドします。
-   UI のスライド操作に応じてアプリのコンテンツ サイズを変更する必要がある場合は、フェード アニメーションを使ってサイズを変更します。
    -   UI を画面内側に向かってスライドする場合は、エッジ (端) UI アニメーションまたはパネル アニメーションの後にフェード アニメーションを使います。
    -   UI を画面外側に向かってスライドする場合は、エッジ (端) UI アニメーションまたはパネル アニメーションと同時にフェード アニメーションを使います。
-   通知には、このアニメーションを適用しないでください。 エッジに基づく UI に通知を格納することはお勧めしません。
-   画面のエッジ (端) にない UI コンテナーやコントロールには、エッジ (端) UI アニメーションとパネル アニメーションを適用しないでください。 このアニメーションは、画面のエッジ (端) にある UI の開閉とサイズ変更にのみ使います。 他のタイプの UI を移動するには、位置変更アニメーションを使います。

    ![エッジ (端) UI アニメーションまたはパネル アニメーションを使うケースと位置変更を使うケースの図](images/edgevsreposition.png)

## <a name="related-articles"></a>関連記事


**開発者向け**
* [アニメーションの概要](./xaml-animation.md)
* [エッジに基づく UI のアニメーション化](/previous-versions/windows/apps/jj649428(v=win.10))
* [クイック スタート: ライブラリのアニメーションを使った UI のアニメーション化](/previous-versions/windows/apps/hh452703(v=win.10))
* [**EdgeUIThemeTransition クラス**](/uwp/api/Windows.UI.Xaml.Media.Animation.EdgeUIThemeTransition)
* [**PaneThemeTransition クラス**](/uwp/api/Windows.UI.Xaml.Media.Animation.PaneThemeTransition)
* [フェードのアニメーション化](/previous-versions/windows/apps/jj649429(v=win.10))
* [位置変更のアニメーション化](/previous-versions/windows/apps/jj649434(v=win.10))

 

 
