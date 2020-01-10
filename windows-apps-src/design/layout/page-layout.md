---
title: UWP アプリのページレイアウト
description: アプリを設計する場合、最初に考慮すべきことは、レイアウト構造です。 この記事では、基本的なページレイアウトの一般的な構造について説明します。これには、必要な UI 要素や、ページをどこに配置する必要があるかなどが含まれます。 UWP アプリでは、各ページには通常、ナビゲーション、コマンド、およびコンテンツ要素があります。
ms.date: 03/19/2018
ms.topic: article
keywords: Windows 10, UWP
localizationpriority: medium
ms.openlocfilehash: 7333cebc945715412e3ff1140ca26e1ed5368704
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/06/2020
ms.locfileid: "75684545"
---
# <a name="page-layout"></a>ページのレイアウト

UWP アプリでは、各[**ページ**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page)には通常、ナビゲーション、コマンド、およびコンテンツ要素があります。 

アプリには複数のページを含めることができます。ユーザーが UWP アプリを起動すると、アプリケーションコードは、アプリケーションの[**ウィンドウ**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window)内に配置する[**フレーム**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame)を作成します。 フレームは、アプリケーションの[**ページ**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page)インスタンス間を[移動](../basics/navigate-between-two-pages.md)できます。 

ほとんどのページは一般的なレイアウト構造に従います。この記事では、必要な UI 要素、およびページの配置場所について説明します。 

![ページ構造](images/page-components.svg)

## <a name="navigation"></a>［ナビゲーション］
アプリのレイアウトは、選択したナビゲーションモデルから開始します。これにより、ユーザーがアプリ内のページ間を移動する方法が定義されます。 この記事では、左ナビゲーションとトップナビゲーションの2つの一般的なナビゲーションパターンについて説明します。 他のナビゲーションオプションの選択に関するガイダンスについては、「 [UWP アプリのナビゲーションデザインの基礎](../basics/navigation-basics.md)」を参照してください。

![上部および左のナビゲーションパターン](images/top-left-nav.svg)

### <a name="left-nav"></a>左ナビゲーション
左ナビゲーションまたは[ナビゲーションウィンドウ](../controls-and-patterns/navigationview.md)パターンは、一般にアプリレベルのナビゲーション用に予約されており、アプリ内の最上位レベルに存在します。つまり、常に表示され、使用できる状態になります。 ナビゲーション項目が5つ以上ある場合、またはアプリに5ページを超える場合は、左ナビゲーションをお勧めします。 通常、ナビゲーションウィンドウパターンには次のものが含まれます。
- ナビゲーション項目
- アプリ設定へのエントリポイント
- アカウント設定のエントリポイント

[Navigationview](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview)コントロールは、UWP の左ナビゲーションパターンを実装します。

ナビゲーション項目を選択すると、選択した項目のページに移動します。

![展開されたナビゲーションウィンドウ](images/navview-expanded.svg)

メニューボタンを使用すると、ユーザーはナビゲーションウィンドウを展開したり折りたたんだりできます。 画面のサイズが 640 px を超える場合は、[メニュー] ボタンをクリックするとナビゲーションウィンドウがバーに折りたたまれます。

![ナビゲーションウィンドウのコンパクト](images/navview-compact.svg)

画面のサイズが 640 px より小さい場合、ナビゲーションウィンドウは完全に折りたたまれています。

![ナビゲーションウィンドウの最小](images/navview-minimal.svg)

### <a name="top-nav"></a>トップナビゲーション

トップナビゲーションは、トップレベルのナビゲーションとしても機能します。 左ナビゲーションは折りたたむことができますが、トップナビゲーションは常に表示されます。 [Navigationview](../controls-and-patterns/navigationview.md)コントロールは、UWP の上部のナビゲーションとタブのパターンを実装します。

![上部のナビゲーション](images/pivot-large.svg)

## <a name="command-bar"></a>コマンド バー

次に、ユーザーがアプリの最も一般的なタスクに簡単にアクセスできるようにすることができます。 [コマンドバー](../controls-and-patterns/app-bars.md)は、アプリレベルまたはページレベルのコマンドへのアクセスを提供し、任意のナビゲーションパターンと共に使用できます。

![コマンドバーの配置 (上) ](images/app-bar-desktop.svg)

コマンドバーは、ページの上部または下部に配置できます。いずれかのアプリに最適です。

![下部のコマンドバーの配置](images/app-bar-mobile.svg)

## <a name="content"></a>コンテンツ

最後に、コンテンツはアプリによって大きく異なるため、さまざまな方法でコンテンツを表示できます。 ここでは、アプリで使用する可能性のある一般的なページパターンについて説明します。 多くのアプリは、これらの一般的なページ パターンの一部またはすべてを使用して、さまざまな種類のコンテンツを表示します。 同様に、これらのパターンを自由に組み合わせて、アプリに合わせて最適化することもできます。

## <a name="landing"></a>ランディング

![ランディング ページ](images/hero-screen.svg)

通常、ランディング ページ (ヒーロー画面とも呼ばれる) は、アプリのエクスペリエンスのトップ レベルに表示されます。 大きいサーフェス領域は、ユーザーが参照および使用する可能性があるコンテンツを、アプリが強調表示するためのステージとして機能します。

## <a name="collections"></a>コレクション

![ギャラリー](images/gridview.svg)

コレクションを使用すると、ユーザーはコンテンツやデータのグループを参照することができます。 [グリッド ビュー](../controls-and-patterns/item-templates-gridview.md)は写真またはメディアを中心とするコンテンツに適していて、[リスト ビュー](../controls-and-patterns/item-templates-listview.md)はテキストが多いコンテンツやデータに適しています。

## <a name="masterdetail"></a>マスター/詳細

![マスター/詳細](images/master-detail.svg)

[マスター/詳細](../controls-and-patterns/master-details.md)モデルは、リスト ビュー (マスター) とコンテンツ ビュー (詳細) で構成されます。 両方のウィンドウは固定されていて、垂直方向にスクロールできます。 リストビュー内の項目が選択されている場合は、それに応じてコンテンツビューが更新されます。 

## <a name="forms"></a>フォーム
![フォーム](images/form.svg)

[フォーム](../controls-and-patterns/forms.md)は、ユーザーからデータを収集して送信するコントロールのグループです。 すべてではなくても、ほとんどのアプリが、設定ページ、ログイン ポータル、フィードバック Hub、アカウントの作成などのために、何らかのフォームを使用しています。 

## <a name="sample-apps"></a>サンプル アプリ
これらのパターンを実装する方法については、 [UWP サンプルアプリ](https://developer.microsoft.com/windows/samples)を参照してください。
- [BuildCast ビデオプレーヤー](https://github.com/Microsoft/BuildCast)
- [ランチ スケジューラ](https://github.com/Microsoft/Windows-appsample-lunch-scheduler)
- [塗り絵帳](https://github.com/Microsoft/Windows-appsample-coloringbook)
- [顧客注文データベース](https://github.com/Microsoft/Windows-appsample-customers-orders-database)