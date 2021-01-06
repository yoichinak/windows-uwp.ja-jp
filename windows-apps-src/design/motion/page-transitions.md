---
title: ページ切り替え効果
description: ユニバーサル Windows プラットフォーム (UWP) ページの切り替え効果を使用して、アプリ内のページ間の関係に関するフィードバックをユーザーに提供する方法について説明します。
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: b676cf274121ae991066f908b18f1be705d1c580
ms.sourcegitcommit: 77903451ae8ab1ba854494488f79184e674707df
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/06/2021
ms.locfileid: "97911012"
---
# <a name="page-transitions"></a>ページ切り替え効果

ページ切り替え効果により、ユーザーがアプリ内のページ間を移動するため、ページ間の関係がフィードバックされます。 ページ切り替え効果により、ナビゲーション階層の最上位にいるのか、兄弟ページ間を移動しているのか、またはページ階層をより深く移動しているのかを、ユーザーが理解しやすくなります。

アプリ内のページ間のナビゲーションについて 2 つの異なるアニメーション (*ページの更新* および *ドリル*) が提供されており、[**NavigationTransitionInfo**](/uwp/api/windows.ui.xaml.media.animation.navigationtransitioninfo) のサブクラスとして表されています。

## <a name="examples"></a>例

<table>
<th align="left">XAML コントロール ギャラリー<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML コントロールギャラリー</strong>アプリがインストールされている場合は、ここをクリックして<a href="xamlcontrolsgallery:/item/PageTransition">アプリを開き、動作中のページの切り替え効果を確認</a>します。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML コントロール ギャラリー アプリを入手する (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">ソース コード (GitHub) を入手する</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="page-refresh"></a>ページの更新

ページの更新は、受信したコンテンツのスライド アップ アニメーションとフェード イン アニメーションの組み合わせです。 ページの更新は、ユーザーがナビゲーション スタックの最上位に移動したときに使用します (タブ間や左側のナビゲーションの項目間の移動など)。

ユーザーが処理を開始したという意識を持てるようにします。

![ページの更新のアニメーション](images/page-refresh.gif)

ページ更新アニメーションは、 [**EntranceNavigationTransitionInfoClass**](/uwp/api/windows.ui.xaml.media.animation.entrancenavigationtransitioninfo)によって表されます。

```csharp
// Explicitly play the page refresh animation
myFrame.Navigate(typeof(Page2), null, new EntranceNavigationTransitionInfo());

```

**注**: [**フレーム**](/uwp/api/windows.ui.xaml.controls.frame)は自動的に [**NavigationThemeTransition**](/uwp/api/windows.ui.xaml.media.animation.navigationthemetransition) を使用して 2 つのページ間のナビゲーションをアニメーション化します。 既定では、アニメーションはページの更新です。

## <a name="drill"></a>Drill

ドリルは、項目を選択した後で詳細情報を表示するなど、ユーザーがアプリ内でより深く移動するときに使用します。

ユーザーがアプリ内をより深く移動したという意識を持てるようにします。

![ドリルのアニメーション](images/drill.gif)

ドリルのアニメーションは、[**DrillInNavigationTransitionInfo**](/uwp/api/windows.ui.xaml.media.animation.drillinnavigationtransitioninfo) クラスで表されます。

```csharp
// Play the drill in animation
myFrame.Navigate(typeof(Page2), null, new DrillInNavigationTransitionInfo());
```

## <a name="horizontal-slide"></a>横方向のスライド

横方向のスライドを使用して、兄弟ページが相互に表示されることを示します。 [Navigationview](../controls-and-patterns/navigationview.md)コントロールは、トップナビゲーション用にこのアニメーションを自動的に使用しますが、独自の水平方向のナビゲーションエクスペリエンスを構築する場合は、SlideNavigationTransitionInfo を使用して横方向のスライドを実装できます。

ユーザーが互いに隣にあるページ間を移動していることが期待できます。 

```csharp
// Navigate to the right, ie. from LeftPage to RightPage
myFrame.Navigate(typeof(RightPage), null, new SlideNavigationTransitionInfo() { Effect = SlideNavigationTransitionEffect.FromRight } );

// Navigate to the left, ie. from RightPage to LeftPage
myFrame.Navigate(typeof(LeftPage), null, new SlideNavigationTransitionInfo() { Effect = SlideNavigationTransitionEffect.FromLeft } );
```

## <a name="suppress"></a>Suppress

ナビゲーション中にアニメーションの再生を回避するには、[**SuppressNavigationTransitionInfo**](/uwp/api/windows.ui.xaml.media.animation.suppressnavigationtransitioninfo) を他の **NavigationTransitionInfo** サブタイプの代わりに使用します。

```csharp
// Suppress the default animation
myFrame.Navigate(typeof(Page2), null, new SuppressNavigationTransitionInfo());
```

アニメーションの抑制は、[接続型アニメーション](connected-animation.md)または暗黙的な表示/非表示アニメーションを使用して独自の切り替え効果を作成している場合に役立ちます。

## <a name="backwards-navigation"></a>逆方向のナビゲーション

`Frame.GoBack(NavigationTransitionInfo)` を使用して逆方向に移動するときに特定の切り替え効果を再生することができます。

これは、たとえば応答性の高いマスター/詳細シナリオなど、画面サイズに基づいて移動動作を動的に変更する場合に役立ちます。

## <a name="related-topics"></a>関連トピック

- [2 つのページ間の移動](../basics/navigate-between-two-pages.md)
- [UWP アプリのモーション](index.md)
