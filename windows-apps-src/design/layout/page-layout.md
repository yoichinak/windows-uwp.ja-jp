---
title: Windows アプリのページ レイアウト
description: アプリを設計する場合、最初に検討すべきことは、レイアウト構造です。 この記事では、必要な UI 要素およびそれらのページ上の配置場所など、基本的なページ レイアウトの一般的な構造について説明します。 Windows アプリの場合、通常、各ページは、ナビゲーション、コマンド、コンテンツの各要素で構成されます。
ms.date: 03/19/2018
ms.topic: article
keywords: windows 10, uwp
localizationpriority: medium
ms.openlocfilehash: 551d937836f0dcf0094e54a503d2a8cd80a2f28b
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89169526"
---
# <a name="page-layout"></a>ページのレイアウト

Windows アプリの場合、通常、各[**ページ**](/uwp/api/Windows.UI.Xaml.Controls.Page)は、ナビゲーション、コマンド、コンテンツの各要素で構成されます。 

アプリは複数のページで構成することができます。ユーザーが Windows アプリを起動すると、アプリケーション コードによって、[**フレーム**](/uwp/api/Windows.UI.Xaml.Controls.Frame)が作成され、アプリケーションの[**ウィンドウ**](/uwp/api/windows.ui.xaml.window)内に配置されます。 フレームは、アプリケーションの[**ページ**](/uwp/api/Windows.UI.Xaml.Controls.Page) インスタンス間を[移動](../basics/navigate-between-two-pages.md)できます。 

ほとんどのページは一般的なレイアウト構造に従うため、この記事では、必要な UI 要素、それらのページ上の配置場所について説明します。 

![ページ構造](images/page-components.svg)

## <a name="navigation"></a>［ナビゲーション］
アプリのレイアウトは、ナビゲーション モデルの選択から始まります。ナビゲーション モデルでは、ユーザーがアプリ内のページ間を移動する方法を定義します。 この記事では、2 つの一般的なナビゲーション パターン (左側のナビゲーションと上部ナビゲーション) について説明します。 他のナビゲーション オプションの選択に関するガイダンスについては、[Windows アプリのナビゲーション デザインの基本](../basics/navigation-basics.md)に関するページをご覧ください。

![上部および左側のナビゲーション パターン](images/top-left-nav.svg)

### <a name="left-nav"></a>左側のナビゲーション
左側のナビゲーション、または[ナビゲーション ウィンドウ](../controls-and-patterns/navigationview.md) パターンは、通常、アプリレベルのナビゲーション用に予約され、アプリ内で最上位のレベルにあります。つまり、常に表示され、使用可能な状態にある必要があります。 ナビゲーション項目の数が 5 個を超える場合、またはアプリのページが 5 ページを超える場合は、左側のナビゲーションをおすすめします。 通常、このナビゲーション ウィンドウ パターンには、次のものが含まれます。
- ナビゲーション項目
- アプリ設定へのエントリ ポイント
- アカウント設定へのエントリ ポイント

[NavigationView](/uwp/api/windows.ui.xaml.controls.navigationview) コントロールは、UWP の左側のナビゲーション パターンを実装します。

ナビゲーション項目が選択されると、フレームは選択された項目のページに移動する必要があります。

![展開されたナビゲーション ウィンドウ](images/navview-expanded.svg)

メニュー ボタンを使用すると、ユーザーは、ナビゲーション ウィンドウを展開したり、折りたたんだりすることができます。 画面のサイズが 640 ピクセルより大きい場合、メニュー ボタンをクリックすると、ナビゲーション ウィンドウが折りたたまれ、バーとして表示されます。

![折りたたまれたナビゲーション ウィンドウ](images/navview-compact.svg)

画面のサイズが 640 ピクセルより小さい場合、ナビゲーション ウィンドウは完全に折りたたまれます。

![最小のナビゲーション ウィンドウ](images/navview-minimal.svg)

### <a name="top-nav"></a>上部ナビゲーション

上部ナビゲーションもトップレベルのナビゲーションとして機能します。 左側のナビゲーションは折りたたむことができますが、上部ナビゲーションは常に表示されます。 [NavigationView](../controls-and-patterns/navigationview.md) コントロールは、UWP の上部ナビゲーションとタブのパターンを実装します。

![上部ナビゲーション](images/pivot-large.svg)

## <a name="command-bar"></a>コマンド バー

次に、ユーザーがアプリの最も一般的なタスクに簡単にアクセスできるようにすることが必要になる場合があります。 [コマンド バー](../controls-and-patterns/app-bars.md)を使用すると、アプリレベルまたはページレベルのコマンドにアクセスすることができます。これはどのナビゲーション パターンでも使用できます。

![上部に配置されたコマンド バー ](images/app-bar-desktop.svg)

コマンド バーは、ページの上部または下部 (アプリに最適ないずれかの位置) に配置できます。

![下部に配置されたコマンド バー](images/app-bar-mobile.svg)

## <a name="content"></a>コンテンツ

最後に、コンテンツはアプリによって大きく異なるため、さまざまな方法で表示できます。 ここでは、アプリで使用する可能性のあるいくつかの一般的なページ パターンについて説明します。 多くのアプリは、これらの一般的なページ パターンの一部またはすべてを使用して、さまざまな種類のコンテンツを表示します。 また、これらのパターンを自由に組み合わせて、アプリに合わせて最適化することもできます。

## <a name="landing"></a>ランディング

![ランディング ページ](images/hero-screen.svg)

通常、ランディング ページ (ヒーロー画面とも呼ばれる) は、アプリのエクスペリエンスのトップ レベルに表示されます。 大きいサーフェス領域は、ユーザーが参照および使用する可能性があるコンテンツを、アプリが強調表示するためのステージとして機能します。

## <a name="collections"></a>コレクション

![ギャラリー](images/gridview.svg)

コレクションを使用すると、ユーザーはコンテンツやデータのグループを参照することができます。 [グリッド ビュー](../controls-and-patterns/item-templates-gridview.md)は写真またはメディアを中心とするコンテンツに適していて、[リスト ビュー](../controls-and-patterns/item-templates-listview.md)はテキストが多いコンテンツやデータに適しています。

## <a name="masterdetail"></a>マスター/詳細

![マスター/詳細](images/master-detail.svg)

[マスター/詳細](../controls-and-patterns/master-details.md)モデルは、リスト ビュー (マスター) とコンテンツ ビュー (詳細) で構成されます。 両方のウィンドウは固定されていて、垂直方向にスクロールできます。 リスト ビュー内の項目が選択されると、それに対応して、コンテンツ ビューも更新されます。 

## <a name="forms"></a>フォーム
![フォーム](images/form.svg)

[フォーム](../controls-and-patterns/forms.md)は、ユーザーからデータを収集して送信するコントロールのグループです。 すべてではなくても、ほとんどのアプリが、設定ページ、ログイン ポータル、フィードバック Hub、アカウントの作成などのために、何らかのフォームを使用しています。 

## <a name="sample-apps"></a>サンプル アプリ
これらのパターンを実装する方法については、[Windows サンプル アプリ](https://developer.microsoft.com/windows/samples)を確認してください。
- [BuildCast ビデオ プレーヤー](https://github.com/Microsoft/BuildCast)
- [ランチ スケジューラ](https://github.com/Microsoft/Windows-appsample-lunch-scheduler)
- [塗り絵帳](https://github.com/Microsoft/Windows-appsample-coloringbook)
- [顧客注文データベース](https://github.com/Microsoft/Windows-appsample-customers-orders-database)