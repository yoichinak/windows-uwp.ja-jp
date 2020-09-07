---
description: まとめて表示される複数の関連データ項目の表現としてのコレクションとリストについて説明します。 
title: コレクションとリスト
ms.assetid: C73125E8-3768-46A5-B078-FDDF42AB1077
label: Collections and Lists
template: detail.hbs
ms.date: 10/08/2019
ms.topic: article
keywords: windows 10, uwp
pm-contact: anawish
design-contact: kimsea
dev-contact: ranjeshj
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: ede68414d86f333b516be81cbae83ea58dc83ba0
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89169916"
---
# <a name="collections-and-lists"></a>コレクションとリスト

コレクションとリストは、どちらも、まとめて表示される複数の関連データ項目の表現を指します。 コレクションは、さまざまなコレクション コントロール (コレクション ビューと呼ばれる場合もあります) によって、複数の方法で表現できます。 コレクション コントロールを使用して、連絡先リスト、日付リスト、画像コレクションなど、コレクションベースのコンテンツを表示して操作できるようにします。

> **重要な API**:[ListView クラス](/uwp/api/Windows.UI.Xaml.Controls.ListView)、[GridView クラス](/uwp/api/Windows.UI.Xaml.Controls.GridView)、[FlipView クラス](/uwp/api/windows.ui.xaml.controls.flipview)、[TreeView クラス](/uwp/api/windows.ui.xaml.controls.treeview)、[ItemsRepeater クラス](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater?view=winui-2.2)

この記事で説明するコントロールには、以下が含まれます。

- リスト ビュー: 主に、テキストの多いコンテンツのコレクションを表示するために使います。
- グリッド ビュー: 主に、画像の多いコンテンツのコレクションを表示するために使います。
- フリップ ビュー: 主に、一度に 1 つの項目のみに注目する必要がある画像の多いコンテンツのコレクションを表示するために使います。
- ツリー ビュー: 主に、テキストの多いコンテンツのコレクションを特定の階層で表示するために使います。
- ItemsRepeater: これは、カスタム コレクション コントロールを作成するためのカスタマイズ可能な構成要素です。

この後、各コントロールについて、設計のガイドライン、特徴、および例を示します。

これらの (ItemsRepeater を除く) 各コントロールには、組み込みのスタイル設定と操作が用意されています。 ただし、コレクション ビューとその中の項目の視覚的な外観をカスタマイズするには、[DataTemplate](/uwp/api/Windows.UI.Xaml.DataTemplate) を使用します。 データ テンプレートの詳細とコレクション ビューの外観のカスタマイズについては、「[項目コンテナーやテンプレート](./item-containers-templates.md)」を参照してください。

これらの (ItemsRepeater を除く) 各コントロールには、1 つまたは複数の項目を選択できるようにするための組み込みの動作も含まれています。 詳細については、「[選択モードの概要](selection-modes.md)」を参照してください。

この記事で説明されていないシナリオの 1 つは、コレクションをテーブルまたは複数の列で表示することです。 この形式でコレクションを表示する場合は、[Windows Community Toolkit](/windows/communitytoolkit/) の [DataGrid コントロール](/windows/communitytoolkit/controls/datagrid)の使用を検討してください。 

> **Windows 10 Fall Creators Update - 動作の変更** 既定では、Windows アプリでのアクティブなペンは、選択の実行ではなく、リストのスクロールやパンを (タッチ、タッチパッド、パッシブなペンなどと同様に) するようになりました。
> アプリが以前の動作に依存している場合は、ペン スクロールを上書きして、以前の動作に戻すことができます。 詳しくは、[ScrollViewer クラス](/uwp/api/windows.ui.xaml.controls.scrollviewer)の API リファレンス トピックをご覧ください。

## <a name="examples"></a>例

<table>
<th align="left">XAML コントロール ギャラリー<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML コントロール ギャラリー</strong> アプリがインストールされている場合は、<a href="xamlcontrolsgallery:/item/ListView">ListView</a>、<a href="xamlcontrolsgallery:/item/GridView">GridView</a>、<a href="xamlcontrolsgallery:/item/FlipView">FlipView</a>、<a href="xamlcontrolsgallery:/item/TreeView">TreeView</a>、および <a href="xamlcontrolsgalley:/item/ItemsRepeater">ItemsRepeater</a> の動作を確認してください。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML コントロール ギャラリー アプリを入手する (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">ソース コード (GitHub) を入手する</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="list-views"></a>リスト ビュー

リスト ビューは、通常、単一列の垂直方向に並べられたレイアウトで、テキストの多い項目を表現します。 それらを使用して、項目の分類、グループ ヘッダーの割り当て、項目のドラッグ アンド ドロップ、コンテンツの管理、および項目の順序変更を行うことができます。

### <a name="is-this-the-right-control"></a>これは適切なコントロールですか?

リスト ビューは、次の用途で使います。

- 主にテキストベースの項目で構成され、すべての項目のビジュアルと対話動作を同じにする必要があるコレクションを表示する。
- コンテンツの単一のコレクションまたはカテゴリ別のコレクションを表現する。
- 次の一般的なケースを含むさまざまなユース ケースに対応する。
    - メッセージまたはメッセージ ログの一覧を作成する。
    - 連絡先リストを作成する。
    - [マスター/詳細パターン](master-details.md)のマスター ウィンドウを作成する。 マスター/詳細パターンは、メール アプリによく使われます。このパターンでは、選択できる項目の一覧を一方のウィンドウ (マスター) に表示し、選択された項目の詳細ビューをもう一方のウィンドウ (詳細) に表示します。
    

### <a name="examples"></a>例

連絡先リストを表示する単純なリスト ビューを次に示します。データ項目はアルファベット順にグループ化されています。 グループ ヘッダー (この例ではアルファベットの各文字) をカスタマイズして、ListView のスクロール中に常に上部に表示されるように "固定" することもできます。

![グループ化されたデータを表示するリスト ビュー](images/listview-grouped-example-resized-final.png)

これは、メッセージのログを表示するための反転 ListView であり、最新のメッセージが最後に表示されます。 反転 ListView では、項目は、組み込みのアニメーションを使用して画面の下部に表示されます。

![反転リスト ビュー](images/listview-inverted-2.png)

### <a name="related-articles"></a>関連記事
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">トピック</th>
<th align="left">説明</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="listview-and-gridview.md">リスト ビューとグリッド ビュー</a></p></td>
<td align="left"><p>アプリでリスト ビューやグリッド ビューを使用するための基本情報を提供します。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="item-containers-templates.md">項目コンテナーやテンプレート</a></p></td>
<td align="left"><p>リスト ビューまたはグリッド ビューに表示する項目は、アプリの全体的な見た目を左右する要素になる可能性があります。 コントロール テンプレートとデータ テンプレートを変更してコレクション項目の外観をカスタマイズすることで、アプリの見栄えをよくすることができます。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="item-templates-listview.md">リスト ビューの項目テンプレート</a></p></td>
<td align="left"><p>これらの ListView 用のサンプル項目テンプレートを使って、一般的な種類のアプリの外観を設定できます。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="inverted-lists.md">反転リスト</a></p></td>
<td align="left"><p>反転リストでは、チャット アプリのように、新しい項目が下部に追加されます。 アプリで反転リストを使用する場合は、この記事のガイダンスに従ってください。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="pull-to-refresh.md">引っ張って更新</a></p></td>
<td align="left"><p>引っ張って更新メカニズムを使うと、ユーザーは、タッチ操作でデータのリストを下に引っ張ることで、より多くのデータを取得できるようになります。 リスト ビューで引っ張って更新を実装する場合は、この記事を使用してください。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="nested-ui.md">入れ子になった UI</a></p></td>
<td align="left"><p>入れ子になった UI は、ユーザーも操作が可能なコンテナー内部に囲まれた、操作できるコントロールを公開するユーザー インターフェイス (UI) です。 たとえば、ボタンを含むリスト ビュー項目があるとします。ユーザーはそのリスト項目を選択することも、項目内に入れ子になっているボタンを押すこともできます。 以下のベスト プラクティスに従って、ユーザーにとって最適な入れ子になった UI のエクスペリエンスを提供してください。</p></td>
</tr>
</tbody>
</table>

## <a name="grid-views"></a>グリッド ビュー

グリッド ビューは、画像ベースのコンテンツのコレクションを配置および閲覧する場合に適しています。 グリッド ビュー レイアウトでは、スクロールが垂直方向、パンが水平方向で行われます。 項目はラップされたレイアウトで表示され、左から右、上から下への読み取り順序で表示されます。

### <a name="is-this-the-right-control"></a>これは適切なコントロールですか?

リスト ビューは、次の用途で使います。

- 各項目の焦点が画像であり、各項目のビジュアルと対話動作が同じである必要があるコンテンツ コレクションを表示する。
- コンテンツ ライブラリを表示する。
- [セマンティック ズーム](semantic-zoom.md)に関連付けられた 2 つのコンテンツ ビューの形式を設定する。
- 次の一般的なケースを含むさまざまなユース ケースに対応する。
    - ネットショップ型のユーザー インターフェイス (アプリ、曲、製品を閲覧する)
    - 対話型フォト ライブラリ

### <a name="examples"></a>例

ここでは、アプリの参照用を例として、標準的なグリッド ビューのレイアウトを示します。 グリッド ビュー項目のメタデータは通常、数行のテキストと項目の評価に制限されます。

![グリッド ビューのレイアウトの例](images/controls_gridview_example02.png)

グリッド ビューは、写真やビデオなどのメディアを表示するためによく使用される、コンテンツ ライブラリに最適なソリューションです。 コンテンツ ライブラリでは、ユーザーが項目をタップして動作を開始します。

![コンテンツ ライブラリの例](images/gridview-simple-example-final.png)

### <a name="related-articles"></a>関連記事
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">トピック</th>
<th align="left">説明</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="listview-and-gridview.md">リスト ビューとグリッド ビュー</a></p></td>
<td align="left"><p>アプリでリスト ビューやグリッド ビューを使用するための基本情報を提供します。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="item-containers-templates.md">項目コンテナーやテンプレート</a></p></td>
<td align="left"><p>リスト ビューまたはグリッド ビューに表示する項目は、アプリの全体的な見た目を左右する要素になる可能性があります。 コントロール テンプレートとデータ テンプレートを変更してコレクション項目の外観をカスタマイズすることで、アプリの見栄えをよくすることができます。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="item-templates-gridview.md">グリッド ビューの項目テンプレート</a></p></td>
<td align="left"><p>これらの GridView 用のサンプル項目テンプレートを使って、一般的な種類のアプリの外観を設定できます。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="nested-ui.md">入れ子になった UI</a></p></td>
<td align="left"><p>入れ子になった UI は、ユーザーも操作が可能なコンテナー内部に囲まれた、操作できるコントロールを公開するユーザー インターフェイス (UI) です。 たとえば、ボタンを含むグリッド ビュー項目があるとします。ユーザーはグリッド項目を選択することも、項目内の入れ子になっているボタンを押すこともできます。 以下のベスト プラクティスに従って、ユーザーにとって最適な入れ子になった UI のエクスペリエンスを提供してください。</p></td>
</tr>
</tbody>
</table>

## <a name="flip-views"></a>フリップ ビュー

フリップ ビューは、画像ベースのコンテンツ コレクションの閲覧に適しています。特に、一度に 1 つの画像のみを表示することを目的とするエクスペリエンスに適しています。 フリップ ビューでは、ユーザーがコレクション項目を (垂直方向または水平方向に) 移動させて "ページをめくるように表示" することができ、ユーザーの操作後に各項目が 1 つずつ表示されます。

### <a name="is-this-the-right-control"></a>これは適切なコントロールですか?

フリップ ビューは、次の用途で使います。

- メタデータをほとんど含まない画像で構成される小規模から中規模 (25 項目未満) のコレクションを表示する。
- 項目を一度に 1 つずつ表示し、エンドユーザーが自分のペースで項目を表示できるようにする。
- 次の一般的なケースを含むさまざまなユース ケースに対応する。
    - フォト ギャラリー
    - 製品ギャラリーやショーケース

### <a name="examples"></a>例

次の 2 つの例は、それぞれが水平方向と垂直方向に反転する FlipView を示しています。

![水平方向のフリップ ビュー](images/controls_flipview_horizonal.jpg)

![垂直方向のフリップ ビュー](images/controls_flipview_vertical.jpg)

### <a name="related-articles"></a>関連記事
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">トピック</th>
<th align="left">説明</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="flipview.md">FlipView</a></p></td>
<td align="left"><p>アプリでフリップ ビューを使用する際の要点と、フリップ ビュー内の項目の外観をカスタマイズする方法について説明します。</p></td>
</tr>
</tbody>
</table>

## <a name="tree-views"></a>ツリー ビュー

ツリー ビューは、はっきりと示す必要がある重要な階層が存在するテキストベースのコレクションを表示する場合に適しています。 ツリー ビューの項目は折りたたみ/展開可能であるビジュアル階層で表示されます。アイコンを使用して補完でき、ツリー ビュー間でドラッグ アンド ドロップできます。 ツリー ビューでは、N レベルの入れ子が可能です。

### <a name="is-this-the-right-control"></a>これは適切なコントロールですか?

ツリー ビューは、次の用途で使います。

- コンテキストと意味が階層または特定の組織チェーンに依存している入れ子になった項目のコレクションを表示する。
- 次の一般的なケースを含むさまざまなユース ケースに対応する。
    - ファイル ブラウザー
    - 会社の組織図

### <a name="examples"></a>例

ファイル エクスプローラーを表現するツリー ビューの例を次に示します。アイコンによって補完された複数の入れ子になった項目が示されています。

![アイコン付きのツリー ビュー](images/treeview-icons.png)

### <a name="related-articles"></a>関連記事
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">トピック</th>
<th align="left">説明</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="tree-view.md">ツリー ビュー</a></p></td>
<td align="left"><p>アプリでツリー ビューを使用する際の要点と、ツリー ビュー内の項目の外観をカスタマイズする方法について説明します。</p></td>
</tr>
</tbody>
</table>

## <a name="itemsrepeater"></a>ItemsRepeater

ItemsRepeater は、このページに示された他のコレクション コントロールとは異なり、プロパティを定義せずに単にページに配置するだけでスタイルや対話機能が提供されることはありません。 ItemsRepeater は、どちらかといえば、開発者が独自のカスタム コレクション コントロールを作成するために使用できる構成要素であり、特にこの記事の他のコントロールの使用では実現できないコントロールを作成するために使用できます。 ItemsRepeater は、ニーズに合わせて調整できるデータ主導のハイパフォーマンス パネルです。

### <a name="is-this-the-right-control"></a>これは適切なコントロールですか?

ItemsRepeater は、次に該当する場合に使用します。

- 既存のコレクション コントロールの使用では作成できない固有のユーザー インターフェイスとユーザー エクスペリエンスを考えている。
- 項目用の既存のデータ ソース (インターネット、データベース、分離コード内の既存のコレクションから引き出されるデータなど) がある。

### <a name="examples"></a>例

次の 3 つの例は、すべてが同じデータ ソース (数値のコレクション) にバインドされている ItemsRepeater コントロールです。 数値のコレクションが、3 つの方法で表現されています。それぞれの ItemsRepeater で、異なるカスタム [Layout](/uwp/api/microsoft.ui.xaml.controls.layout) と異なるカスタム [ItemTemplate](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.itemtemplate?view=winui-2.2) が使用されています。

![水平バーを使用した ItemsRepeater](images/itemsrepeater-1.png)
![垂直バーを使用した ItemsRepeater](images/itemsrepeater-2.png)
![円表現を使用した ItemsRepeater](images/itemsrepeater-3.png)

### <a name="related-articles"></a>関連記事
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">トピック</th>
<th align="left">説明</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="items-repeater.md">ItemsRepeater</a></p></td>
<td align="left"><p>アプリで ItemsRepeater を使用する際の要点と、コレクション ビューで必要なすべての操作とビジュアル コンポーネントを実装する方法について説明します。</p></td>
</tr>
</tbody>
</table>


## <a name="globalization-and-localization-checklist"></a>グローバリゼーションとローカライズのチェックリスト

<table>
<tr>
<th>折り返し</th><td>一覧のラベルを 2 行にできます。</td>
</tr>
<tr>
<th>水平方向の拡張</th><td>フィールドでテキストの伸張とスクロールに対応できるようにします。</td>
</tr>
<tr>
<th>垂直方向の間隔</th><td>垂直方向の間隔に非ラテン文字を使用し、非ラテン文字が適切に表示されるようにします。</td>
</tr>
</table>

## <a name="get-the-sample-code"></a>サンプル コードの入手

- [XAML コントロール ギャラリー サンプル](https://github.com/Microsoft/Xaml-Controls-Gallery) - インタラクティブな形で XAML コントロールのすべてを参照できます。

## <a name="related-articles"></a>関連記事

**設計と UX のガイドライン**
- [マスター/詳細](master-details.md)
- [ナビゲーション ウィンドウ](navigationview.md)
- [セマンティック ズーム](semantic-zoom.md)
- [ドラッグ アンド ドロップ](https://docs.microsoft.com/windows/uwp/app-to-app/drag-and-drop)
- [サムネイル画像](../../files/thumbnails.md)

**API リファレンス**
- [ListView クラス](/uwp/api/Windows.UI.Xaml.Controls.ListView)
- [GridView クラス](/uwp/api/Windows.UI.Xaml.Controls.GridView)
- [ComboBox クラス](/uwp/api/Windows.UI.Xaml.Controls.ComboBox)
- [ListBox クラス](/uwp/api/Windows.UI.Xaml.Controls.ListBox)