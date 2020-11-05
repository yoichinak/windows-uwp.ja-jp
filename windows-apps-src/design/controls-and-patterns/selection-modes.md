---
description: 選択モードでは、ユーザーは、単一の項目または複数の項目を選択して、それらの項目に対して操作を実行できます。
title: 選択モード
label: Selection mode
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: anawish
design-contact: kimsea
dev-contact: ranjeshj
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 1f3ec8d816ecd6de0cb08ca685a69cc693f4f47c
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2020
ms.locfileid: "93035195"
---
# <a name="selection-mode-overview"></a>選択モードの概要

選択モードでは、単一の項目または複数の項目を選択して、それらの項目に対して操作を実行できます。 選択モードは、コンテキスト メニュー、Crtl キーまたは Shift キーを押しながらの項目のクリック、またはギャラリー ビューでの項目に対するターゲットのロールオーバーによって起動できます。 選択モードがアクティブであるとき、各リスト項目の横にチェック ボックスを表示し、画面の上部または下部に操作を表示できます。

## <a name="different-selection-modes"></a>さまざまな選択モード
選択モードには、次の 3 つがあります。

- 単一: ユーザーは同時に 1 つの項目だけを選ぶことができます。
- 複数: ユーザーは修飾キーを使わずに複数の項目を選ぶことができます。
- 拡張: ユーザーは、Shift キーを押すなど修飾キーを使って複数の項目を選ぶことができます。

## <a name="selection-mode-examples"></a>選択モードの例
### <a name="here-is-a-listview-with-single-selection-mode-enabled"></a>次に示すのは、単一選択モードが有効になっている ListView です。
![単一選択モードのリスト ビュー](images/listview-selection-single.png)

項目 Art Venere が選択され、現在、項目 Mitsue Tollner にポインターが置かれています。

### <a name="here-is-a-listview-with-multiple-selection-mode-enabled"></a>次に示すのは、複数選択モードが有効になっている ListView です。
![複数選択モードのリスト ビュー](images/listview-selection-multiple.png)

3 つの項目が選択され、項目 Donette Foller にポインターが置かれています。

### <a name="here-is-a-gridview-with-single-selection-mode-enabled"></a>次に示すのは、単一選択モードが有効になっている GridView です。
![単一選択モードのグリッド ビュー](images/gridview-selection-single.png)

Item 7 が選択され、現在 Item 3 にポインターが置かれています。

### <a name="here-is-a-gridview-with-multiple-selection-mode-enabled"></a>次に示すのは、複数選択モードが有効になっている GridView です。
![複数選択モードのグリッド ビュー](images/gridview-selection-multiple.png)

Item 2、Item 4、および Item 5 が選択され、現在 Item 7 にポインターが置かれています。

## <a name="behavior-and-interaction"></a>選択と操作
項目の任意の場所をタップすると、項目が選ばれます。 コマンド バーの操作をタップすると、選択したすべての項目に影響します。 項目が選ばれていない場合、コマンド バーの操作は [すべて選択] を除いて非アクティブになります。

選択モードには簡易非表示モデルがありません。選択モードがアクティブなフレームの外側をタップしても、モードを取り消すことはできません。 これにより、モードが誤って非アクティブ化されることを防止できます。 戻るボタンをクリックすると、複数選択モードが終了します。

操作が選択されているときは、確認できるように視覚的に示します。 特定の操作に対して (特に破棄を伴う削除などの操作に対して)、確認ダイアログを表示することを検討します。

選択モードは、選択モードをアクティブにしたページに限定され、そのページ以外の項目に影響を与えることはできません。

選択モードへのエントリ ポイントは、そのモードが影響を与えるコンテンツに対して並置する必要があります。

コマンド バーの推奨事項については、「[コマンド バーのガイドライン](app-bars.md)」をご覧ください。

選択モードと特定のコントロールの選択イベントの処理の詳細については、そのコントロールのガイダンス ページを参照してください。
