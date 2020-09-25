---
Description: フェード アニメーションは、項目を画面に表示したり、項目を画面から非表示にするときに使います。 一般的なフェード アニメーションは、フェード インとフェード アウトの 2 つです。
title: フェード アニメーション
ms.assetid: 975E5EE3-EFBE-4159-8D10-3C94143DD07F
label: Motion--fades
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 6ae8afb291a2ba6012c55c2f22290f51b7eb76f0
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/24/2020
ms.locfileid: "91220155"
---
# <a name="fade-animations"></a>フェード アニメーション



フェード アニメーションは、項目を画面に表示したり、項目を画面から非表示にするときに使います。 一般的なフェード アニメーションは、フェード インとフェード アウトの 2 つです。

> **重要な API**: [**FadeInThemeAnimation クラス**](/uwp/api/Windows.UI.Xaml.Media.Animation.FadeInThemeAnimation)、[**FadeOutThemeAnimation クラス**](/uwp/api/Windows.UI.Xaml.Media.Animation.FadeOutThemeAnimation)


## <a name="dos-and-donts"></a>推奨と非推奨


-   アプリで互いに関係のない要素や、テキストの多い要素を切り替えるときには、フェード アウトとフェード インを使います。 そうすることで、差し替え前のオブジェクトが完全に消えてから差し替え後のオブジェクトを表示させることができます。
-   差し替える 2 つの要素のサイズが一定であり、ユーザーに同じ項目を見ているような印象を与えたいときには、差し替え後の要素を差し替え前の要素の上にフェード インさせます。 フェード インが完了したら、差し替え前の項目は消すことができます。 これは、差し替え後の項目が差し替え前の項目を完全に覆い隠せる場合にのみ可能な方法です。
-   リストの項目を追加または削除する目的でフェード アニメーションを使うのは避けてください。 そのような場合には、専用に作成したリスト アニメーションを使います。
-   フェード アニメーションは、ページの全コンテンツを変化させるときには使わないでください。 そのような場合には、専用に作成したページ切り替えアニメーションを使います。
-   フェード アウトは要素を削除するための繊細な方法です。
## <a name="related-articles"></a>関連記事

* [アニメーションの概要](./xaml-animation.md)
* [フェードのアニメーション化](/previous-versions/windows/apps/jj649429(v=win.10))
* [クイック スタート: ライブラリのアニメーションを使った UI のアニメーション化](/previous-versions/windows/apps/hh452703(v=win.10))
* [**FadeInThemeAnimation クラス**](/uwp/api/Windows.UI.Xaml.Media.Animation.FadeInThemeAnimation)
* [**FadeOutThemeAnimation クラス**](/uwp/api/Windows.UI.Xaml.Media.Animation.FadeOutThemeAnimation)

 

 
