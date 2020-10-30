---
description: ポップアップ アニメーションを使って、ポップアップ UI やカスタム ポップアップ UI 要素の表示と非表示を切り替えます。 ポップアップ要素とは、アプリのコンテンツの上に表示されるコンテナーのことで、ユーザーがポップアップ要素の外部をタップまたはクリックすると消えます。
title: ポップアップ UI のアニメーション
ms.assetid: 4E9025CE-FC90-4d4c-9DE6-EC6B6F2AD9DF
label: Motion--Pop-up animations
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 5d09363d6ecd2eb5909c98da316c7c1395ebec03
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034125"
---
# <a name="pop-up-ui-animations"></a>ポップアップ UI のアニメーション



ポップアップ アニメーションを使って、ポップアップ UI やカスタム ポップアップ UI 要素の表示と非表示を切り替えます。 ポップアップ要素とは、アプリのコンテンツの上に表示されるコンテナーのことで、ユーザーがポップアップ要素の外部をタップまたはクリックすると消えます。

> **重要な API** : [**PopInThemeAnimation クラス**](/uwp/api/Windows.UI.Xaml.Media.Animation.PopInThemeAnimation)、 [**PopupThemeTransition クラス**](/uwp/api/Windows.UI.Xaml.Media.Animation.PopupThemeTransition)


## <a name="dos-and-donts"></a>推奨と非推奨


-   ポップアップ アニメーションは、アプリ ページ自体には含まれないカスタム ポップアップ UI 要素を表示したり非表示にしたりするために使います。 Windows で用意されているコモン コントロールには、既にこのアニメーションが組み込まれています。
-   ヒントやダイアログにポップアップ アニメーションを使わないでください。
-   アプリのメイン コンテンツ内の UI の表示と非表示を切り替えるためにポップアップ アニメーションを使わないでください。ポップアップ アニメーションは、メイン アプリ コンテンツの上に表示されるポップアップ コンテナーの表示と非表示を切り替えるときにのみ使います。

## <a name="related-articles"></a>関連記事

* [アニメーションの概要](./xaml-animation.md)
* [ポップアップ UI のアニメーション化](/previous-versions/windows/apps/jj649433(v=win.10))
* [クイック スタート: ライブラリのアニメーションを使った UI のアニメーション化](/previous-versions/windows/apps/hh452703(v=win.10))
* [**PopInThemeAnimation クラス**](/uwp/api/Windows.UI.Xaml.Media.Animation.PopInThemeAnimation)
* [**PopOutThemeAnimation クラス**](/uwp/api/Windows.UI.Xaml.Media.Animation.PopOutThemeAnimation)
* [**PopupThemeTransition クラス**](/uwp/api/Windows.UI.Xaml.Media.Animation.PopupThemeTransition)

 

 
