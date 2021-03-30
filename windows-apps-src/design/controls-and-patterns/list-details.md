---
description: リストと詳細パターンでは、項目の一覧と、現在選択されている項目の詳細が表示されます。 このパターンは、メールや連絡先リストまたはアドレス帳によく使用されます。
title: リストと詳細
ms.assetid: 45C9FE8B-ECA6-44BF-8DDE-7D12ED34A7F7
label: List/details
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 5532a0a5a5424d69c94d33db559aaa7afe786d83
ms.sourcegitcommit: 6661f4d564d45ba10e5253864ac01e43b743c560
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/23/2021
ms.locfileid: "104806092"
---
# <a name="listdetails-pattern"></a>リストと詳細パターン

リストと詳細パターンには、リスト ペイン (通常は[リスト ビュー](lists.md)を含む) とコンテンツの詳細ペインがあります。 リストの項目を選択すると、詳細ウィンドウが更新されます。 このパターンは、メールやアドレス帳によく使われます。

> **重要な API**:[ListView クラス](/uwp/api/Windows.UI.Xaml.Controls.ListView)、[SplitView クラス](/uwp/api/windows.ui.xaml.controls.splitview)

![リストと詳細パターンの例](images/list-detail-pattern.png)

> [!TIP]
> このパターンを実装する XAML コントロールを使用したい場合は、Windows Community Toolkit の [ListDetailsView XAML コントロール](/windows/communitytoolkit/controls/masterdetailsview)をお勧めします。

## <a name="is-this-the-right-pattern"></a>これは適切なパターンですか?

次の場合は、リストと詳細パターンが適しています。

- メール アプリ、アドレス帳、またはリストと詳細レイアウトに基づくアプリを構築する。
- 大きいコンテンツのコレクションを検索して優先順位を設定する。
- コンテキスト間を前後に移動しながら、リストから項目をすばやく追加および削除できるようにする。

## <a name="choose-the-right-style"></a>適切なスタイルの選択

リストと詳細パターンを実装するとき、利用可能な画面領域の量に応じて、上下に並べるスタイルまたは左右に並べるスタイルを使うことをお勧めします。

| 利用可能なウィンドウの幅 | 推奨スタイル |
|------------------------|-------------------|
| 320 epx から 640 epx        | 上下に並べて表示           |
| 641 epx 以上       | 左右に並べて表示      |

## <a name="stacked-style"></a>上下に並べて表示するスタイル

上下に並べるスタイルでは、リストまたは詳細のいずれか 1 つのペインだけが表示されます。

![上下に並べるモードでのリストと詳細](images/patterns-md-stacked.png)

ユーザーがリスト ペインで作業を始め、リストで項目を選んで詳細ペインに "ドリルダウン" します。 ユーザーから見ると、リストと詳細のビューが別々の 2 つのページに存在するかのように表示されます。

### <a name="create-a-stacked-listdetails-pattern"></a>上下に並べるリストと詳細パターンの作成

上下に並べるリストと詳細パターンを作成する方法の 1 つは、リスト ペインと詳細ペインにそれぞれ別のページを使うことです。 リスト ビューを 1 つのページに、詳細ペインを別のページに配置します。

![上下に並べるスタイルのリストと詳細のパーツ](images/patterns-ld-stacked-parts.png)

リスト ビュー ページでは、画像およびテキストが含められる可能性があるリストを表示するのに[リスト ビュー](lists.md) コントロールが適しています。

詳細ビュー ページの場合、最も意味のある[コンテンツ要素](../layout/layout-panels.md)を使います。 多くの個別フィールドがある場合は、**グリッド** レイアウトを使って要素をフォームに配置することを検討します。

ページ間の移動については、「[Windows アプリでのナビゲーション履歴と前に戻る移動](../basics/navigation-history-and-backwards-navigation.md)」をご覧ください。

## <a name="side-by-side-style"></a>左右に並べて表示するスタイル

横に並べるスタイルでは、リスト ペインと詳細ペインを同時に表示できます。

![リストと詳細のパターン](images/patterns-listdetail-400x227.png)

リスト ペインのリストは、現在選択されている項目を示すために選択ビジュアルを使用します。 リストで新しい項目を選ぶと、詳細ペインが更新されます。

### <a name="create-a-side-by-side-listdetails-pattern"></a>左右に並べるリストと詳細パターンの作成

左右に並べるリストと詳細パターンを作成する 1 つの方法として、[分割ビュー](split-view.md) コントロールを使用する方法があります。 分割ビューのペインにリスト ビューを、分割ビューのコンテンツに詳細ビューを配置します。

![リストと詳細の分割ビューのパーツ](images/patterns-ld-splitview-parts.png)

リスト ペインでは、画像およびテキストが含まれる可能性があるリストを表示するのに[リスト ビュー](lists.md) コントロールが適しています。

詳細コンテンツの場合、最も意味のある[コンテンツ要素](../layout/layout-panels.md)を使います。 多くの個別フィールドがある場合は、**グリッド** レイアウトを使って要素をフォームに配置することを検討します。

## <a name="adaptive-layout"></a>アダプティブ レイアウト

リストと詳細パターンをすべての画面サイズで実装するには、[アダプティブ レイアウト](../layout/layouts-with-xaml.md) で応答性の高い UI を作成します。

![アダプティブなリストと詳細のレイアウト](images/patterns_listdetail.png)

### <a name="create-an-adaptive-listdetails-pattern"></a>アダプティブなリストと詳細パターンの作成
アダプティブ レイアウトを作成するには、UI にさまざまな [**VisualStates**](/uwp/api/windows.ui.xaml.visualstate) を定義し、[**AdaptiveTriggers**](/uwp/api/Windows.UI.Xaml.AdaptiveTrigger) を使用してさまざまな状態のブレークポイントを宣言します。

## <a name="get-the-sample-code"></a>サンプル コードの入手

次のサンプルでは、アダプティブ レイアウトを使用してリストと詳細パターンを実装し、静的なリソース、データベース リソース、およびオンライン リソースに対するデータ バインディングを示します。 
- [マスター/詳細のサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlMasterDetail) 
- [ListView と GridView のサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlListView)
- [Windows Template Studio マスター/詳細のサンプル](https://github.com/Microsoft/WindowsTemplateStudio/tree/master/templates/Uwp/Pages/MasterDetail)
- [顧客注文データベースのサンプル](https://github.com/Microsoft/Windows-appsample-customers-orders-database)
- [RSS リーダーのサンプル](https://github.com/Microsoft/Windows-appsample-rssreader)

> [!TIP]
> このパターンを実装する XAML コントロールを使用したい場合は、Windows Community Toolkit の [ListDetailsView XAML コントロール](/windows/communitytoolkit/controls/masterdetailsview)をお勧めします。

## <a name="related-articles"></a>関連記事

- [リスト](lists.md)
- [検索](search.md)
- [アプリ バーとコマンド バー](app-bars.md)
- [ListView クラス](/uwp/api/Windows.UI.Xaml.Controls.ListView)
- [SplitView クラス](/uwp/api/windows.ui.xaml.controls.splitview)
