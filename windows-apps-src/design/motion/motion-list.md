---
description: リスト アニメーションを使うと、写真のアルバムや検索結果の一覧などのコレクションに対して任意の数の項目を挿入または削除できます。
title: 追加と削除のアニメーション
ms.assetid: A85006AE-4992-457a-B514-500B8BEF5DC8
label: Motion--add and delete animations
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: cb01cbf3dab1ae8f1cdb055ad3fc04b94b1ff510
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034205"
---
# <a name="add-and-delete-animations"></a>追加と削除のアニメーション



リスト アニメーションを使うと、写真のアルバムや検索結果の一覧などのコレクションに対して任意の数の項目を挿入または削除できます。

> **重要な API** : [**AddDeleteThemeTransition クラス**](/uwp/api/windows.ui.xaml.media.animation.adddeletethemetransition)


## <a name="dos-and-donts"></a>推奨と非推奨


-   リスト アニメーションは、既にある一連の項目に新しい項目を 1 つ追加するときに使います。 たとえば、新しい電子メールを受け取ったときや、既にあるセットに新しい写真をインポートするときに使います。
-   リスト アニメーションは、一連の項目に対して複数の項目を一度に追加するときに使います。 たとえば、一連の新しい写真を既にあるコレクションにインポートするときに使います。 複数項目の追加と削除は、個々のオブジェクトの処理に間が生じることなく同時に実行されます。
-   追加と削除のリスト アニメーションは、ペアで使います。 一方のアニメーションを使った場合は、逆の操作として対応するもう一方のアニメーションを使うようにしてください。
-   リスト アニメーションは、要素または要素のグループを一度に追加または削除できる項目リストに使います。
-   コンテナーを表示したり非表示にしたりする目的でリスト アニメーションを使うのは避けてください。 リスト アニメーションは、既に表示されているコレクションまたはセットのメンバーに対して使います。 アプリ サーフェス上に一時的なコンテナーを表示したり非表示にしたりするには、ポップアップ アニメーションを使います。 アプリ サーフェスの一部となっているコンテナーを表示したり置き換えたりするには、コンテンツ切り替えアニメーションを使います。
-   項目のセット全体に対してリスト アニメーションを使うのは避けてください。 コンテナー内のコレクション全体を追加したり削除したりするには、コンテンツ切り替えアニメーションを使います。



## <a name="related-articles"></a>関連記事

* [アニメーションの概要](./xaml-animation.md)
* [リストの追加と削除のアニメーション化](/previous-versions/windows/apps/jj649430(v=win.10))
* [クイック スタート: ライブラリのアニメーションを使った UI のアニメーション化](/previous-versions/windows/apps/hh452703(v=win.10))
* [**AddDeleteThemeTransition クラス**](/uwp/api/windows.ui.xaml.media.animation.adddeletethemetransition)

 

 
