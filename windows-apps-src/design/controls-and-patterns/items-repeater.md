---
Description: ItemsRepeater は、項目のコレクションを生成して表示するための軽量なコントロールです。
title: ItemsRepeater
label: ItemsRepeater
template: detail.hbs
ms.date: 02/01/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 20aeda53af3b4b11c1562d2ed22b099a3377d3c7
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172636"
---
# <a name="itemsrepeater"></a>ItemsRepeater

柔軟なレイアウト システム、カスタム ビュー、および仮想化を使ってカスタム コレクション エクスペリエンスを作成するには、[ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) を使用します。

[ListView](/uwp/api/windows.ui.xaml.controls.listview) とは異なり、[ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) では包括的なエンド ユーザー エクスペリエンスが提供されません。既定の UI はなく、フォーカス、選択、ユーザーの操作に関するポリシーは提供されません。 その代わり、構成要素を使用することで、独自の一意のコレクション ベースのエクスペリエンスおよびカスタム コントロールを作成することができます。 組み込みのポリシーはありませんが、必要なエクスペリエンスを構築するためのポリシーをアタッチすることができます。 たとえば、使用するレイアウト、キーボード操作ポリシー、選択ポリシーなどを定義できます。

[ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) は概念的に、ListView などの完全なコントロールとしてではなく、データドリブン パネルと考えることができます。 表示されるデータ項目のコレクション、各データ項目の UI 要素を生成する項目テンプレート、要素のサイズと位置を設定する方法を決定するレイアウトを指定します。 その後、ItemsRepeater でデータ ソースに基づいた子要素が生成され、項目テンプレートとレイアウトで指定されたとおりに表示されます。 表示される項目が同種である必要はありません。これは、データ テンプレート セレクターで指定する条件に基づいてデータ項目を表すために、ItemsRepeater でコンテンツを読み込むことができるためです。

**Windows UI ライブラリを入手する**

|  |  |
| - | - |
| ![WinUI ロゴ](images/winui-logo-64x64.png) | **ItemsRepeater** コントロールは、Windows アプリのための新しいコントロールと UI 機能を含む NuGet パッケージである Windows UI ライブラリの一部として含まれています。 インストール手順などについて詳しくは、「[Windows UI Library (Windows UI ライブラリ)](/uwp/toolkits/winui/)」をご覧ください。 |

> **Windows UI ライブラリ API:** [ItemsRepeater クラス](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)
>
> **プラットフォーム API:** [ScrollViewer クラス](/uwp/api/windows.ui.xaml.controls.scrollviewer)

## <a name="is-this-the-right-control"></a>これは適切なコントロールですか?

データ コレクション用のカスタム表示を作成するには、[ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) を使用します。 これは基本的な一連の項目を表示するために使用できますが、多くの場合、カスタム コントロールのテンプレートで表示要素として使用します。

最小限にカスタマイズされたグリッドまたはリストにデータを表示するためにすぐに使えるコントロールが必要な場合は、[ListView](/uwp/api/windows.ui.xaml.controls.listview) または [GridView](/uwp/api/windows.ui.xaml.controls.gridview) の使用を検討してください。

ItemsRepeater には組み込みの項目コレクションはありません。 別のデータ ソースにバインドするのではなく、項目コレクションを直接提供しなければならない場合は、より高いポリシー エクスペリエンスが必要になる可能性があり、[ListView](/uwp/api/windows.ui.xaml.controls.listview) または [GridView](/uwp/api/windows.ui.xaml.controls.gridview) を使用する必要があります。

[ItemsControl](/uwp/api/windows.ui.xaml.controls.itemscontrol) と ItemsRepeater の両方でカスタマイズ可能なコレクション エクスペリエンスを有効にすることはできますが、ItemsRepeater では UI レイアウトの仮想化がサポートされるのに対して ItemsControl ではサポートされません。 単にデータのいくつかの項目を表示するためであるか、カスタム コレクション コントロールをビルドするためであるかに関係なく、ItemsControl ではなく ItemsRepeater を使用することをお勧めします。

> [!NOTE]
> ItemsControl ではニーズが満たされ、ItemsRepeater では満たされていないと思われる場合は、[Windows UI ライブラリの GitHub プロジェクト](https://github.com/Microsoft/microsoft-ui-xaml/issues)でフィードバックを送信してお知らせください。

## <a name="examples"></a>例

<table>
<th align="left">XAML コントロール ギャラリー<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML コントロール ギャラリー</strong> アプリがインストールされている場合は、こちらをクリックしてアプリを開き、<a href="xamlcontrolsgallery:/item/ItemsRepeater">ItemsRepeater</a> の動作を確認してください。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML コントロール ギャラリー アプリを入手する (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">ソース コード (GitHub) を入手する</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="scrolling-with-itemsrepeater"></a>ItemsRepeater でのスクロール

[**ItemsRepeater**](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) は [**Control**](/uwp/api/windows.ui.xaml.controls.control) から派生しないため、コントロール テンプレートはありません。 したがって、ListView やその他のコレクション コントロールのようにスクロールの操作は組み込まれていません。

**ItemsRepeater** を使用する場合は、[**ScrollViewer**](/uwp/api/windows.ui.xaml.controls.scrollviewer) コントロールでラップし、スクロール機能を提供する必要があります。

> [!NOTE]
> 以前のバージョンの Windows (Windows 10 バージョン 1809 *より前* にリリースされたもの) でアプリを実行する場合は、[**ItemsRepeaterScrollHost**](/uwp/api/microsoft.ui.xaml.controls.itemsrepeaterscrollhost) 内で **ScrollViewer** をホストする必要もあります。 
> ```xaml
> <muxc:ItemsRepeaterScrollHost>
>     <ScrollViewer>
>         <muxc:ItemsRepeater ... />
>     </ScrollViewer>
> </muxc:ItemsRepeaterScrollHost>
> ```
> 最新バージョン (バージョン 1809 以降) の Windows 10 でのみアプリを実行する場合は、[**ItemsRepeaterScrollHost**](/uwp/api/microsoft.ui.xaml.controls.itemsrepeaterscrollhost) を使用する必要はありません。
>
> Windows 10 バージョン 1809 より前の場合、**ScrollViewer** では、**ItemsRepeater** に必要な [ **IScrollAnchorProvider**](/uwp/api/windows.ui.xaml.controls.iscrollanchorprovider) インターフェイスが実装されませんでした。  **ItemsRepeaterScrollHost** では、**ItemsRepeater** が以前のリリースの **ScrollViewer** と連動し、ユーザーに表示される項目の見える場所が適切に保持されるようにします。  それ以外の場合、リスト内の項目が変更されたり、アプリのサイズが変更されたりした場合に、項目が突然移動されたり、消されたりするように表示されることがあります。

## <a name="create-an-itemsrepeater"></a>ItemsRepeater を作成する

[**ItemsRepeater**](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) を使用するには、**ItemsSource** プロパティを設定し、表示するデータを指定する必要があります。 その後、[**ItemTemplate**](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.itemtemplate) プロパティを設定し、項目の表示方法を指示します。

### <a name="itemssource"></a>ItemsSource

ビューを設定するには、[**ItemsSource**](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.itemssource) プロパティをデータ項目のコレクションに設定します。 ここでは、**ItemsSource** がコードでコレクションのインスタンスに直接設定されています。

```csharp
ObservableCollection<string> Items = new ObservableCollection<string>();

ItemsRepeater itemsRepeater1 = new ItemsRepeater();
itemsRepeater1.ItemsSource = Items;
```

**ItemsSource** プロパティを、XAML でコレクションにバインドすることもできます。 データ バインディングについて詳しくは、「[データ バインディングの概要](../../data-binding/data-binding-quickstart.md)」をご覧ください。


```xaml
<ItemsRepeater ItemsSource="{x:Bind Items}"/>
```

### <a name="itemtemplate"></a>ItemTemplate
データ項目を視覚化する方法を指定するには、定義した [**DataTemplate**](/uwp/api/windows.ui.xaml.datatemplate) または [**DataTemplateSelector**](/uwp/api/windows.ui.xaml.controls.datatemplateselector) に [**ItemTemplate**](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.itemtemplate) プロパティを設定します。 データ テンプレートで、データがどのように視覚化されるかが定義されます。 既定では、項目は、データ オブジェクトの文字列表現を使用する **TextBlock** でビューに表示されます。

しかし、通常は、個々の項目を表示するために使用する 1 つまたは複数のコントロールのレイアウトと外観を定義するテンプレートを使用して、より豊富な表現でデータを表示する必要があります。 テンプレートで使用するコントロールを、データ オブジェクトのプロパティにバインドすることも、その静的コンテンツをインラインで定義することもできます。

#### <a name="datatemplate"></a>DataTemplate
この例では、データ オブジェクトはシンプルな文字列です。 **DataTemplate** にはテキストの左側にイメージが含まれており、青緑色で文字列を表示するために **TextBlock** がスタイル設定されています。

> [!NOTE]
> **DataTemplate** で [x:Bind markup extension](../../xaml-platform/x-bind-markup-extension.md) を使う場合、DataTemplate に DataType (`x:DataType`) を指定する必要があります。

```xaml
<DataTemplate x:DataType="x:String">
    <Grid>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="47"/>
            <ColumnDefinition/>
        </Grid.ColumnDefinitions>
        <Image Source="Assets/placeholder.png" Width="32" Height="32"
               HorizontalAlignment="Left"/>
        <TextBlock Text="{x:Bind}" Foreground="Teal"
                   FontSize="15" Grid.Column="1"/>
    </Grid>
</DataTemplate>
```

この **DataTemplate** で項目を表示した場合、次のようになります。

![データ テンプレートを使って表示された項目](images/listview-itemstemplate.png)

ビューで多くの項目が表示される場合、項目の **DataTemplate** で使用される要素の数がパフォーマンスに大きく影響する可能性があります。 **DataTemplate** を使用してリスト項目の外観を定義する詳しい方法のその例については、「[項目のコンテナーとテンプレート](item-containers-templates.md)」を参照してください。

> [!TIP]
> 便宜上、静的リソースとして参照するのではなく、テンプレートをインラインで宣言する必要がある場合は、**ItemsRepeater** の直接の子として、**DataTemplate** または **DataTemplateSelector** を指定することができます。  これは、**ItemTemplate** プロパティの値として割り当てられます。 たとえば、これが有効です。
> ```xaml
> <ItemsRepeater ItemsSource="{x:Bind Items}">
>     <DataTemplate>
>         <!-- ... -->
>     </DataTemplate>
> </ItemsRepeater>
> ```

> [!TIP]
> **ListView** やその他のコレクション コントロールとは異なり、**ItemsRepeater** では、余白、パディング、選択ビジュアル、ビジュアル状態のポインターなど、既定のポリシーを含む追加の項目コンテナーがある **DataTemplate** の要素は折り返されません。 代わりに、**ItemsRepeater** では、**DataTemplate** で定義された内容のみが表示されます。 項目をリスト ビュー項目と同じ外観にする必要がある場合は、データ テンプレートで、**ListViewItem** などの、コンテナーを明示的に含めることができます。 **ItemsRepeater** では  **ListViewItem** ビジュアルが表示されますが、選択や複数選択チェックボックスの表示など、その他の機能は自動的に利用されません。
>
> 同様に、データ コレクションが、**Button** (`List<Button>`) などの、実際のコントロールのコレクションである場合は、**DataTemplate** に  **ContentPresenter** を配置してコントロールを表示することができます。

#### <a name="datatemplateselector"></a>DataTemplateSelector

ビューで表示する項目が、同じ種類である必要はありません。 [**DataTemplateSelector**](/uwp/api/windows.ui.xaml.controls.datatemplateselector) を使用して [**ItemTemplate**](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.itemtemplate) プロパティを指定し、指定した条件に基づいて異なる **DataTemplate** を選択することができます。

この例では、Large と Small の項目を表すために、2 つの異なる **DataTemplate** のうちどちらかを決める **DataTemplateSelector** が定義されていると仮定します。

```xaml
<ItemsRepeater ...>
    <ItemsRepeater.ItemTemplate>
        <local:VariableSizeTemplateSelector Large="{StaticResource LargeItemTemplate}" 
                                            Small="{StaticResource SmallItemTemplate}"/>
    </ItemsRepeater.ItemTemplate>
</ItemsRepeater>
```

**ItemsRepeater** で使用する **DataTemplateSelector** を定義する場合、[**SelectTemplateCore(Object)** ](/uwp/api/windows.ui.xaml.controls.datatemplateselector.selecttemplatecore#Windows_UI_Xaml_Controls_DataTemplateSelector_SelectTemplateCore_System_Object_) メソッドのオーバーライドを実装するだけで済みます。 詳しい説明と例については、[**DataTemplateSelector**](/uwp/api/windows.ui.xaml.controls.datatemplateselector) に関するページを参照してください。

> [!NOTE]
> より高度なシナリオで要素を作成する方法を管理する **DataTemplate** の代わりに、独自の [**Windows.UI.Xaml.Controls.IElementFactory**](/uwp/api/windows.ui.xaml.controls.ielementfactory) を実装し、**ItemTemplate** として使用することができます。  これには、要求されたときにコンテンツを生成する役割があります。

## <a name="configure-the-data-source"></a>データ ソースを構成する

項目のコンテンツを生成するために使用するコレクションを指定するには、[ItemsSource](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.itemssource) プロパティを使用します。 **IEnumerable** を実装する任意の種類に ItemsSource を設定することができます。 データ ソースによって実装される追加のコレクション インターフェイスで、データを操作するために ItemsRepeater で使用できる機能が決定されます。

このリストには使用可能なインターフェイスと、それぞれの使用を検討するタイミングが示されています。

- [IEnumerable](/dotnet/api/system.collections.generic.ienumerable-1)(.NET) / [IIterable](/uwp/api/windows.foundation.collections.iiterable_t_)

  - 小規模の静的なデータ セットで使用できます。

    データ ソースでは、少なくとも IEnumerable / IIterable インターフェイスを実装する必要があります。 サポートされているのがこれだけである場合、コントロールで一度すべてのものが反復処理されると、インデックス値を使用して項目にアクセスするために使用できるコピーが作成されます。

- [IReadonlyList](/dotnet/api/system.collections.generic.ireadonlylist-1)(.NET) / [IVectorView](/uwp/api/windows.foundation.collections.ivectorview_t_)

  - 静的な読み取り専用のデータ セットで使用できます。

    インデックスを使用してコントロールで項目にアクセスできるようにし、内部の冗長コピーの回避を可能にします。

- [IList](/dotnet/api/system.collections.generic.ilist-1)(.NET) / [IVector](/uwp/api/windows.foundation.collections.ivector_t_)

  - 静的なデータ セットで使用できます。

    インデックスを使用してコントロールで項目にアクセスできるようにし、内部の冗長コピーの回避を可能にします。

    **警告**: [INotifyCollectionChanged](/dotnet/api/system.collections.specialized.inotifycollectionchanged) を実装せずに list/vector を変更した場合、UI では反映されません。

- [INotifyCollectionChanged](/dotnet/api/system.collections.specialized.inotifycollectionchanged)(.NET)

  - 変更通知をサポートすることをお勧めします。

    コントロールで、データ ソース内の変更を監視して対応できるようにし、これらの変更を UI に反映できるようにします。

- [IObservableVector](/uwp/api/windows.foundation.collections.iobservablevector_t_)

  - 変更通知がサポートされます

    **INotifyCollectionChanged** インターフェイスと同様、これにより、コントロールでデータ ソース内の変更を監視し、対応できるようになります。

    **警告**: Windows.Foundation.IObservableVector\<T> では '移動' アクションはサポートされません。 そのため、UI で項目の表示状態が失われる可能性があります。  たとえば、現在選択されているか、'削除' の後、'追加' によって移動が行われた場所にフォーカスがあるか、あるいはその両方の状態の項目のフォーカスが失われ、選択できなくなります。

    Platform.Collections.Vector\<T> では IObservableVector\<T> が使用され、これと同じ制限があります。 '移動' アクションのサポートが必要な場合は、**INotifyCollectionChanged** インターフェイスを使用します。  .NET ObservableCollection\<T> クラスでは **INotifyCollectionChanged** が使用されます。

- [IKeyIndexMapping](/uwp/api/microsoft.ui.xaml.controls.ikeyindexmapping)

  - 一意識別子を各項目に関連付けることができる場合。  コレクション変更アクションとして 'リセット' を使用する場合にお勧めします。

    コントロールで、**INotifyCollectionChanged** または **IObservableVector** イベントの一部としてハード 'リセット' アクションを受信した後、既存の UI を非常に効率的に回復できるようにします。 リセットを受信した後、コントロールでは、既に作成されている要素に現在のデータを関連付けるために、指定された一意の ID が使用されます。 キーをインデックスにマップしない場合、コントロールでは、データの UI の作成を最初からやり直す必要があると見なす必要があります。

IKeyIndexMapping 以外の、上記のインターフェイスでは、ItemsRepeater で、ListView や GridView の場合と同じ動作が提供されます。


ItemsSource の次のインターフェイスでは、ListView および GridView コントロールで特別な機能が有効になりますが、現在、ItemsRepeater で影響はありません。

- [ISupportIncrementalLoading](/uwp/api/windows.ui.xaml.data.isupportincrementalloading)
- [IItemsRangeInfo](/uwp/api/windows.ui.xaml.data.iitemsrangeinfo)
- [ISelectionInfo](/uwp/api/windows.ui.xaml.data.iselectioninfo)

> [!TIP]
> ご意見をお寄せください [Windows UI ライブラリ GitHub プロジェクト](https://github.com/Microsoft/microsoft-ui-xaml/issues)でご意見をお知らせください。 次の [#374](https://github.com/Microsoft/microsoft-ui-xaml/issues/374) などの既存の提案に対する意見の追加をご検討ください:ItemsRepeater の段階的な読み込みのサポートを追加する。

ユーザーが上下にスクロールしたときに、データを段階的に読み込む別の方法は、ScrollViewer のビューポートの位置を監視し、ビューポートがエクステントに近づいたときにさらにデータを読み込むことです。

```xaml
<ScrollViewer ViewChanged="ScrollViewer_ViewChanged">
    <ItemsRepeater ItemsSource="{x:Bind MyItemsSource}" .../>
</ScrollViewer>
```

```csharp
private async void ScrollViewer_ViewChanged(object sender, ScrollViewerViewChangedEventArgs e)
{
    if (!e.IsIntermediate)
    {
        var scroller = (ScrollViewer)sender;
        var distanceToEnd = scroller.ExtentHeight - (scroller.VerticalOffset + scroller.ViewportHeight);

        // trigger if within 2 viewports of the end
        if (distanceToEnd <= 2.0 * scroller.ViewportHeight
                && MyItemsSource.HasMore && !itemsSource.Busy)
        {
            // show an indeterminate progress UI
            myLoadingIndicator.Visibility = Visibility.Visible;

            await MyItemsSource.LoadMoreItemsAsync(/*DataFetchSize*/);

            loadingIndicator.Visibility = Visibility.Collapsed;
        }
    }
}
```

## <a name="change-the-layout-of-items"></a>項目のレイアウト変更

[ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) によって表示される項目は、その子要素のサイズ設定と配置を管理する [Layout](/uwp/api/microsoft.ui.xaml.controls.layout) オブジェクトによって配置されます。 ItemsRepeater と共に使用する場合、Layout オブジェクトによって UI の仮想化が有効になります。 指定されているレイアウトは、[StackLayout](/uwp/api/microsoft.ui.xaml.controls.stacklayout) と [UniformGridLayout](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout) です。 既定では、ItemsRepeater で垂直方向の StackLayout が使用されます。

### <a name="stacklayout"></a>StackLayout

[StackLayout](/uwp/api/microsoft.ui.xaml.controls.stacklayout) では 1 行に要素が配置され、これを水平方向または垂直方向に設定することができます。

[Spacing](/uwp/api/microsoft.ui.xaml.controls.stacklayout.spacing) プロパティを設定することで、項目間のスペースの量を調整できます。 Spacing は、レイアウトの [Orientation](/uwp/api/microsoft.ui.xaml.controls.stacklayout.orientation) の方向に適用されます。

![スタック レイアウトの間隔](images/stack-layout.png)

この例では、ItemsRepeater.Layout プロパティの StackLayout を水平方向に、間隔を 8 ピクセルに設定します。

```xaml
<!-- xmlns:muxc="using:Microsoft.UI.Xaml.Controls" -->
<muxc:ItemsRepeater ItemsSource="{x:Bind Items}" ItemTemplate="{StaticResource MyTemplate}">
    <muxc:ItemsRepeater.Layout>
        <muxc:StackLayout Orientation="Horizontal" Spacing="8"/>
    </muxc:ItemsRepeater.Layout>
</muxc:ItemsRepeater>
```

### <a name="uniformgridlayout"></a>UniformGridLayout

[UniformGridLayout](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout) では、折り返しレイアウトで順に要素を配置します。 [Orientation](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.orientation) が **Horizontal** の場合、項目は左から右の順にレイアウトされ、Orientation が **Vertical** の場合は、上から下にレイアウトされます。 すべての項目のサイズが同じように設定されます。

![均一なグリッド レイアウトの間隔](images/uniform-grid-layout.png)

水平レイアウトの各行の項目数は、項目の最小の幅の影響を受けます。 垂直レイアウトの各列の項目数は、項目の最小の高さの影響を受けます。

- 使用する最小サイズは、[MinItemHeight](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.minitemheight) および [MinItemWidth](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.minitemwidth) プロパティを設定することで、明示的に指定できます。
- 最小サイズを指定しない場合、最初の項目の測定されたサイズが項目ごとの最小サイズと見なされます。

また、行と列の間に含めるレイアウトの最小間隔は、[MinColumnSpacing](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.mincolumnspacing) と [MinRowSpacing](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.minrowspacing) プロパティを設定することで、設定できます。

![均一なグリッドのサイズと間隔の設定](images/uniform-grid-sizing-spacing.png)

行または列内の項目の数が項目の最小サイズと間隔に基づいて決定された後、(前の図で示したとおり) 行または列の最後の項目の後に未使用のスペースが残る可能性があります。 余分なスペースについては、無視するか、各項目のサイズを増やすために使用するか、項目間に追加のスペースを作成するために使用するかを指定できます。 これは [ItemsStretch](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.itemsstretch) および [ItemsJustification](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.itemsjustification) プロパティで制御されます。

[ItemsStretch](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.itemsstretch) プロパティを使用すれば、未使用のスペースを埋めるために項目サイズを増やす方法を指定することができます。

このリストには使用可能な値が示されています。 定義では、既定の **Orientation** が **Horizontal** であることが前提となります。

- **None**: 余分なスペースが行の末尾に未使用のまま残されます。 これは既定です。
- **Fill**:使用可能なスペースを使い切るように、項目に追加の幅 (垂直の場合は高さ) が指定されます。
- **Uniform**:使用可能なスペースを使い切るように、項目に追加の幅が指定され、縦横比を維持するために追加の高さが指定されます (垂直の場合は、高さと幅が切り替わります)。

この図は、水平レイアウトでの **ItemsStretch** 値の効果を示しています。

![均一なグリッド項目のストレッチ](images/uniform-grid-item-stretch.png)

**ItemsStretch** が **None** の場合は、[ItemsJustification](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.itemsjustification) プロパティを設定し、余分なスペースを使用して項目を揃える方法を指定できます。

このリストには使用可能な値が示されています。 定義では、既定の **Orientation** が **Horizontal** であることが前提となります。

- **Start**:項目は行の先頭に揃えられます。 余分なスペースが行の末尾に未使用のまま残されます。 これは既定です。
- **Center**:項目は行の中央に揃えられます。 余分なスペースは、行の先頭と末尾に均等に分割されます。
- **End**:項目は行の末尾に揃えられます。 余分なスペースが行の先頭に未使用のまま残されます。
- **SpaceAround**:項目は均等に分散されます。 各項目の前後に同量のスペースが追加されます。
- **SpaceBetween**:項目は均等に分散されます。 各項目の間に同量のスペースが追加されます。 行の先頭と末尾にはスペースは追加されません。
- **SpaceEvenly**:項目は、各項目間および行の先頭と末尾の両方に、等しいスペースで均等に分散されます。

この図には、垂直レイアウトでの **ItemsStretch** 値の効果が示されています (行ではなく、列に適用)。

![均一なグリッド項目の位置揃え](images/uniform-grid-item-justification.png)

> [!TIP]
> **ItemsStretch** プロパティは、レイアウトの_測定_ パスに影響します。 **ItemsJustification** プロパティは、レイアウトの_配置_ パスに影響します。

この例では、**ItemsRepeater.Layout** プロパティを **UniformGridLayout** に設定する方法を示します。

```xaml
<!-- xmlns:muxc="using:Microsoft.UI.Xaml.Controls" -->
<muxc:ItemsRepeater ItemsSource="{x:Bind Items}"
                    ItemTemplate="{StaticResource MyTemplate}">
    <muxc:ItemsRepeater.Layout>
        <muxc:UniformGridLayout MinItemWidth="200"
                                MinColumnSpacing="28"
                                ItemsJustification="SpaceAround"/>
    </muxc:ItemsRepeater.Layout>
</muxc:ItemsRepeater>
```

## <a name="lifecycle-events"></a>ライフサイクル イベント

[ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) で項目をホストするときに、コンテンツの非同期ダウンロードの開始、選択を追跡するメカニズムと要素との関連付け、あるいはバックグラウンド タスクの停止など、項目が表示されたとき、または表示が中止されたときに何らかのアクションが必要な場合があります。

仮想化コントロールでは、Loaded/Unloaded イベントに依存することはできません。これは、要素がリサイクル時にライブ ビジュアル ツリーから削除されない場合があるためです。 代わりに、その他のイベントが、要素のライフサイクルを管理するために提供されます。 この図では、ItemsRepeater での要素のライフサイクルと、関連イベントがいつ発生するかが示されています。

![ライフ サイクル イベントの図](images/items-repeater-lifecycle.png)

- [**ElementPrepared**](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.elementprepared) は、要素が使用できる状態になるたびに発生します。 これは、新しく作成された要素と、既に存在していてリサイクル キューから再利用されている要素の両方で発生します。
- [**ElementClearing**](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.elementclearing) は、認識された項目の範囲から外れた場合など、要素がリサイクル キューに送信されるたびにすぐに発生します。
- [**ElementIndexChanged**](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.elementindexchanged
) は、表される項目のインデックスが変更されている、認識済みの各 UIElement で発生します。 たとえば、別の項目がデータ ソースで追加または削除された場合、次の順番にある項目のインデックスでこのイベントが受信されます。

この例では、これらのイベントを使用してカスタム選択サービスをアタッチし、項目を表示するために ItemsRepeater を使用するカスタム コントロールで項目の選択を追跡する方法を示します。

```xaml
<!-- xmlns:muxc="using:Microsoft.UI.Xaml.Controls" -->
<UserControl ...>
    ...
    <ScrollViewer>
        <muxc:ItemsRepeater ItemsSource="{x:Bind Items}"
                            ItemTemplate="{StaticResource MyTemplate}"
                            ElementPrepared="OnElementPrepared"
                            ElementIndexChanged="OnElementIndexChanged"
                            ElementClearing="OnElementClearing">
        </muxc:ItemsRepeater>
    </ScrollViewer>
    ...
</UserControl>
```

```csharp
interface ISelectable
{
    int SelectionIndex { get; set; }
    void UnregisterSelectionModel(SelectionModel selectionModel);
    void RegisterSelectionModel(SelectionModel selectionModel);
}

private void OnElementPrepared(ItemsRepeater sender, ElementPreparedEventArgs args)
{
    var selectable = args.Element as ISelectable;
    if (selectable != null)
    {
        // Wire up this item to recognize a 'select' and listen for programmatic
        // changes to the selection model to know when to update its visual state.
        selectable.SelectionIndex = args.Index;
        selectable.RegisterSelectionModel(this.SelectionModel);
    }
}

private void OnElementIndexChanged(ItemsRepeater sender, ElementIndexChangedEventArgs args)
{
    var selectable = args.Element as ISelectable;
    if (selectable != null)
    {
        // Sync the ID we use to notify the selection model when the item
        // we represent has changed location in the data source.
        selectable.SelectionIndex = args.NewIndex;
    }
}

private void OnElementClearing(ItemsRepeater sender, ElementClearingEventArgs args)
{
    var selectable = args.Element as ISelectable;
    if (selectable != null)
    {
        // Disconnect handlers to recognize a 'select' and stop
        // listening for programmatic changes to the selection model.
        selectable.UnregisterSelectionModel(this.SelectionModel);
        selectable.SelectionIndex = -1;
    }
}
```

## <a name="sorting-filtering-and-resetting-the-data"></a>データの並べ替え、フィルター処理、リセット

データ セットのフィルター処理や並べ替えなどのアクションを実行するときに、従来は前のデータ セットと新しいデータを比較してから、[INotifyCollectionChanged](/uwp/api/windows.ui.xaml.interop.inotifycollectionchanged) を使用して詳細な変更通知を発行していたかもしれません。 しかし、多くの場合、古いデータを新しいデータに完全に置き換え、代わりに[リセット](/uwp/api/windows.ui.xaml.interop.notifycollectionchangedaction) アクションを使用してコレクション変更通知をトリガーするほうが簡単です。

通常、リセットにより、コントロールで既存の子要素が解放され、スクロール位置 0 で最初から UI のビルドをやり直すことになります。これは、リセット時にデータの変更方法が正確に認識されないためです。

しかし、ItemsSource として割り当てられているコレクションで、[IKeyIndexMapping](/uwp/api/microsoft.ui.xaml.controls.ikeyindexmapping) インターフェイスを実装することで一意識別子がサポートされる場合、ItemsRepeater で以下をすばやく識別できます。

- データの再利用可能な UIElement のうち、リセットの前と後の両方に存在していたもの
- 以前表示されていた項目のうち、削除されたもの
- 新しく追加された項目のうち、表示されるもの

これにより、ItemsRepeater でスクロール位置 0 からのやり直しが回避されます。 また、リセットで変更されなかったデータの UIElement をすばやく復元でき、その結果、パフォーマンスが向上します。

この例では、_MyItemsSource_ が項目の基になるリストを折り返すカスタム データ ソースである、垂直スタックで項目のリストを表示する方法を示します。 _Data_ プロパティが公開され、これを使用して新しいリストを再度割り当て、項目ソースとして使用し、その後、リセットをトリガーすることができます。

```xaml
<ScrollViewer x:Name="sv">
    <ItemsRepeater x:Name="repeater"
                ItemsSource="{x:Bind MyItemsSource}"
                ItemTemplate="{StaticResource MyTemplate}">
       <ItemsRepeater.Layout>
           <StackLayout ItemSpacing="8"/>
       </ItemsRepeater.Layout>
   </ItemsRepeater>
</ScrollViewer>
```

```csharp
public MainPage()
{
    this.InitializeComponent();

    // Similar to an ItemsControl, a developer sets the ItemsRepeater's ItemsSource.
    // Here we provide our custom source that supports unique IDs which enables
    // ItemsRepeater to be smart about handling resets from the data.
    // Unique IDs also make it easy to do things apply sorting/filtering
    // without impacting any state (i.e. selection).
    MyItemsSource myItemsSource = new MyItemsSource(data);

    repeater.ItemsSource = myItemsSource;

    // ...

    // We can sort/filter the data using whatever mechanism makes the
    // most sense (LINQ, database query, etc.) and then reassign
    // it, which in our implementation triggers a reset.
    myItemsSource.Data = someNewData;
}

// ...


public class MyItemsSource : IReadOnlyList<ItemBase>, IKeyIndexMapping, INotifyCollectionChanged
{
    private IList<ItemBase> _data;

    public MyItemsSource(IEnumerable<ItemBase> data)
    {
        if (data == null) throw new ArgumentNullException();

        this._data = data.ToList();
    }

    public IList<ItemBase> Data
    {
        get { return _data; }
        set
        {
            _data = value;

            // Instead of tossing out existing elements and re-creating them,
            // ItemsRepeater will reuse the existing elements and match them up
            // with the data again.
            this.CollectionChanged?.Invoke(
                this,
                new NotifyCollectionChangedEventArgs(NotifyCollectionChangedAction.Reset));
        }
    }

    #region IReadOnlyList<T>

    public ItemBase this[int index] => this.Data != null
        ? this.Data[index]
        : throw new IndexOutOfRangeException();

    public int Count => this.Data != null ? this.Data.Count : 0;
    public IEnumerator<ItemBase> GetEnumerator() => this.Data.GetEnumerator();
    IEnumerator IEnumerable.GetEnumerator() => this.GetEnumerator();

    #endregion

    #region INotifyCollectionChanged

    public event NotifyCollectionChangedEventHandler CollectionChanged;

    #endregion

    #region IKeyIndexMapping

    private int lastRequestedIndex = IndexNotFound;
    private const int IndexNotFound = -1;

    // When UniqueIDs are supported, the ItemsRepeater caches the unique ID for each item
    // with the matching UIElement that represents the item.  When a reset occurs the
    // ItemsRepeater pairs up the already generated UIElements with items in the data
    // source.
    // ItemsRepeater uses IndexForUniqueId after a reset to probe the data and identify
    // the new index of an item to use as the anchor.  If that item no
    // longer exists in the data source it may try using another cached unique ID until
    // either a match is found or it determines that all the previously visible items
    // no longer exist.
    public int IndexForUniqueId(string uniqueId)
    {
        // We'll try to increase our odds of finding a match sooner by starting from the
        // position that we know was last requested and search forward.
        var start = lastRequestedIndex;
        for (int i = start; i < this.Count; i++)
        {
            if (this[i].PrimaryKey.Equals(uniqueId))
                return i;
        }

        // Then try searching backward.
        start = Math.Min(this.Count - 1, lastRequestedIndex);
        for (int i = start; i >= 0; i--)
        {
            if (this[i].PrimaryKey.Equals(uniqueId))
                return i;
        }

        return IndexNotFound;
    }

    public string UniqueIdForIndex(int index)
    {
        var key = this[index].PrimaryKey;
        lastRequestedIndex = index;
        return key;
    }

    #endregion
}

```

## <a name="create-a-custom-collection-control"></a>カスタム コレクション コントロールを作成する

[ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) を使用すれば、各項目を表示する独自の種類のコントロールを備えたカスタム コレクション コントロールを作成することができます。

> [!NOTE]
> これは **ItemsControl** を使用する場合と似ていますが、**ItemsControl** から派生させ、コントロール テンプレートに **ItemsPresenter** を配置するのではなく、**Control** から派生させ、コントロール テンプレートに **ItemsRepeater** を挿入します。 **ItemsRepeater** はカスタム コレクション コントロールに "含まれ" ているのに対して、**ItemsControl** はカスタム コレクション コントロール "である" ことになります。 これは、サポートしない継承済みのプロパティではなく、公開するプロパティを明示的に選ぶ必要もあることを意味します。

この例では、_MediaCollectionView_ という名前のカスタム コントロールのテンプレートで [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) を配置し、そのプロパティを公開する方法を示します。

```xaml
<!-- xmlns:muxc="using:Microsoft.UI.Xaml.Controls" -->
<Style TargetType="local:MediaCollectionView">
    <Setter Property="Template">
        <Setter.Value>
            <ControlTemplate TargetType="local:MediaCollectionView">
                <Border
                    Background="{TemplateBinding Background}"
                    BorderBrush="{TemplateBinding BorderBrush}"
                    BorderThickness="{TemplateBinding BorderThickness}">
                    <ScrollViewer x:Name="ScrollViewer">
                        <muxc:ItemsRepeater x:Name="ItemsRepeater"
                                            ItemsSource="{TemplateBinding ItemsSource}"
                                            ItemTemplate="{TemplateBinding ItemTemplate}"
                                            Layout="{TemplateBinding Layout}"
                                            TabFocusNavigation="{TemplateBinding TabFocusNavigation}"/>
                    </ScrollViewer>
                </Border>
            </ControlTemplate>
        </Setter.Value>
    </Setter>
</Style>
```

```csharp
public sealed class MediaCollectionView : Control
{
    public object ItemsSource
    {
        get { return (object)GetValue(ItemsSourceProperty); }
        set { SetValue(ItemsSourceProperty, value); }
    }

    // Using a DependencyProperty as the backing store for ItemsSource.  This enables animation, styling, binding, etc...
    public static readonly DependencyProperty ItemsSourceProperty =
        DependencyProperty.Register(nameof(ItemsSource), typeof(object), typeof(MediaCollectionView), new PropertyMetadata(0));

    public DataTemplate ItemTemplate
    {
        get { return (DataTemplate)GetValue(ItemTemplateProperty); }
        set { SetValue(ItemTemplateProperty, value); }
    }

    // Using a DependencyProperty as the backing store for ItemTemplate.  This enables animation, styling, binding, etc...
    public static readonly DependencyProperty ItemTemplateProperty =
        DependencyProperty.Register(nameof(ItemTemplate), typeof(DataTemplate), typeof(MediaCollectionView), new PropertyMetadata(0));

    public Layout Layout
    {
        get { return (Layout)GetValue(LayoutProperty); }
        set { SetValue(LayoutProperty, value); }
    }

    // Using a DependencyProperty as the backing store for Layout.  This enables animation, styling, binding, etc...
    public static readonly DependencyProperty LayoutProperty =
        DependencyProperty.Register(nameof(Layout), typeof(Layout), typeof(MediaCollectionView), new PropertyMetadata(0));

    public MediaCollectionView()
    {
        this.DefaultStyleKey = typeof(MediaCollectionView);
    }
}
```

## <a name="display-grouped-items"></a>グループ化された項目を表示する

別の ItemsRepeater の [ItemTemplate](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.itemtemplate) で [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) を入れ子にすることで、入れ子になった仮想化レイアウトを作成できます。 フレームワークでは、表示されていないか、現在のビューポートの近くにない要素の不要な認識を最小限に抑えることで、リソースが効率的に利用されます。

この例では、垂直スタックでグループ化された項目のリストを表示する方法を示します。 外部の ItemsRepeater で各グループが生成されます。 各グループのテンプレートでは、別の ItemsRepeater で項目が生成されます。

```xaml
<!-- xmlns:muxc="using:Microsoft.UI.Xaml.Controls" -->

<Page.Resources>
    <muxc:StackLayout x:Key="MyGroupLayout"/>
    <muxc:StackLayout x:Key="MyItemLayout" Orientation="Horizontal"/>
</Page.Resources>

<ScrollViewer>
  <muxc:ItemsRepeater ItemsSource="{x:Bind AppNotifications}"
                      Layout="{StaticResource MyGroupLayout}">
    <muxc:ItemsRepeater.ItemTemplate>
      <DataTemplate x:DataType="ExampleApp:AppNotifications">
        <!-- Group -->
        <StackPanel>
          <!-- Header -->
          <TextBlock Text="{x:Bind AppTitle}"/>
          <!-- Items -->
          <muxc:ItemsRepeater ItemsSource="{x:Bind Notifications}"
                              Layout="{StaticResource MyItemLayout}"
                              ItemTemplate="{StaticResource MyTemplate}"/>
          <!-- Footer -->
          <Button Content="{x:Bind FooterText}"/>
        </StackPanel>
      </DataTemplate>
    </muxc:ItemsRepeater.ItemTemplate>
  </muxc:ItemsRepeater>
</ScrollViewer>
```
以下の図は、上のサンプルをガイドラインとして使用して作成された基本的なレイアウトを示しています。

![items repeater を使用して入れ子にされたレイアウト](images/items-repeater-nested-layout.png)

次の例では、ユーザー設定で変更でき、水平スクロール リストとして表示される、さまざまなカテゴリを含むアプリのレイアウトを示します。 この例のレイアウトは、上の図によっても示されています。

```xaml
<!-- xmlns:muxc="using:Microsoft.UI.Xaml.Controls" -->
<!-- Include the <muxc:ItemsRepeaterScrollHost> if targeting Windows 10 versions earlier than 1809. -->
<ScrollViewer>
  <muxc:ItemsRepeater ItemsSource="{x:Bind Categories}"
                      Background="LightGreen">
    <muxc:ItemsRepeater.ItemTemplate>
      <DataTemplate x:DataType="local:Category">
        <StackPanel Margin="12,0">
          <TextBlock Text="{x:Bind Name}" Style="{ThemeResource TitleTextBlockStyle}"/>
          <!-- Include the <muxc:ItemsRepeaterScrollHost> if targeting Windows 10 versions earlier than 1809. -->
          <ScrollViewer HorizontalScrollMode="Enabled"
                                          VerticalScrollMode="Disabled"
                                          HorizontalScrollBarVisibility="Auto" >
            <muxc:ItemsRepeater ItemsSource="{x:Bind Items}"
                                Background="Orange">
              <muxc:ItemsRepeater.ItemTemplate>
                <DataTemplate x:DataType="local:CategoryItem">
                  <Grid Margin="10"
                        Height="60" Width="120"
                        Background="LightBlue">
                    <TextBlock Text="{x:Bind Name}"
                               Style="{StaticResource SubtitleTextBlockStyle}"
                               Margin="4"/>
                  </Grid>
                </DataTemplate>
              </muxc:ItemsRepeater.ItemTemplate>
              <muxc:ItemsRepeater.Layout>
                <muxc:StackLayout Orientation="Horizontal"/>
              </muxc:ItemsRepeater.Layout>
            </muxc:ItemsRepeater>
          </ScrollViewer>
        </StackPanel>
      </DataTemplate>
    </muxc:ItemsRepeater.ItemTemplate>
  </muxc:ItemsRepeater>
</ScrollViewer>
```

## <a name="bringing-an-element-into-view"></a>要素をビューで表示する

XAML フレームワークでは既に、1) キーボードのフォーカスを受け取ったとき、または 2) ナレーターのフォーカスを受け取ったときにビューでの FrameworkElement の表示が処理されています。 要素を明示的にビューで表示する必要があるケースが他にもある場合があります。 たとえば、ユーザー アクションに応答する場合や、ページ ナビゲーション後に UI の状態を復元する場合です。

仮想化された要素をビューで表示するには、以下の操作が必要です。
1. 項目の UIElement を認識する
2. レイアウトを実行し、確実に要素に有効な位置が指定されるようにする
3. 認識された要素を表示する要求を開始する

以下の例では、ページ ナビゲーション後のフラットな垂直リストでの項目のスクロール位置の復元の一環として、これらの手順を示します。 入れ子になった ItemsRepeater を使用する階層データの場合、方法は同じですが、階層レベルごとに実行する必要があります。

```xaml
<ScrollViewer x:Name="scrollviewer">
  <ItemsRepeater x:Name="repeater" .../>
</ScrollViewer>
```

```csharp
public class MyPage : Page
{
    // ...

     protected override void OnNavigatedTo(NavigationEventArgs e)
    {
        base.OnNavigatedTo(e);

        // retrieve saved offset + index(es) of the tracked element and then bring it into view.
        // ... 
        
        var element = repeater.GetOrCreateElement(index);

        // ensure the item is given a valid position
        element.UpdateLayout();

        element.StartBringIntoView(new BringIntoViewOptions()
        {
            VerticalOffset = relativeVerticalOffset
        });
    }

    protected override void OnNavigatingFrom(NavigatingCancelEventArgs e)
    {
        base.OnNavigatingFrom(e);

        // retrieve and save the relative offset and index(es) of the scrollviewer's current anchor element ...
        var anchor = this.scrollviewer.CurrentAnchor;
        var index = this.repeater.GetElementIndex(anchor);
        var anchorBounds = anchor.TransformToVisual(this.scrollviewer).TransformBounds(new Rect(0, 0, anchor.ActualSize.X, anchor.ActualSize.Y));
        relativeVerticalOffset = this.scrollviewer.VerticalOffset - anchorBounds.Top;
    }
}

```

## <a name="enable-accessibility"></a>アクセシビリティを有効にする

[ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) では、既定のアクセシビリティ エクスペリエンスは提供されません。 [Windows アプリのユーザビリティ](../usability/index.md)に関するドキュメントでは、確実にアプリで包括的なユーザー エクスペリエンスを提供できるようにするために役立つ情報が豊富に示されています。 ItemsRepeater を使ってカスタム コントロールを作成する場合は、必ず、[カスタム オートメーション ピア](../accessibility/custom-automation-peers.md)に関するドキュメントを参照してください。

### <a name="keyboarding"></a>キーボード操作
ItemsRepeater で提供されるフォーカス移動のためのキーボード操作の最小限のサポートは、XAML の[キーボード操作の 2D 方向ナビゲーション](../input/focus-navigation.md#2d-directional-navigation-for-keyboard)に関する記述に基づいています。

![方向ナビゲーション](/windows/uwp/design/input/images/keyboard/directional-navigation.png)

ItemsRepeater の [XYFocusKeyboardNavigation モード](/uwp/api/windows.ui.xaml.input.xyfocuskeyboardnavigationmode)は、既定で _Enabled_ になっています。 目的のエクスペリエンスに応じて、Home、End、PageUp、PageDown などの一般的な[キーボード操作](../input/keyboard-interactions.md)のサポートの追加を検討してください。

ItemsRepeater では、項目の既定のタブ順序が (仮想化されているかどうかに関わらず)、データ内に項目が指定されているのと同じ順序に従っていることが自動的に確保されます。 ItemsRepeater では、既定でその [TabFocusNavigation](/uwp/api/windows.ui.xaml.uielement.tabfocusnavigation) プロパティが、一般的な既定値である _Local_ ではなく、[Once](/uwp/api/windows.ui.xaml.input.keyboardnavigationmode) に設定されます。

> [!NOTE]
> ItemsRepeater では、最後にフォーカスが置かれた項目が自動的に記憶されません。  これは、ユーザーが Shift + Tab キーを使用したときに、最後に認識された項目に移動される可能性があることを意味します。

### <a name="announcing-item-_x_-of-_y_-in-screen-readers"></a>スクリーン リーダーでの "_Y_ の _X_ 項目" の読み上げ

**PositionInSet** や **SizeOfSet** の値などの、適切なオートメーション プロパティの設定を管理する必要があり、項目の追加、移動、削除などが行われた場合、確実に最新の状態が保たれているようにする必要があります。

一部のカスタム レイアウトでは、表示順序が明白でない場合があります。  ユーザーは最低でも、スクリーン リーダーによって使用される PositionInSet および SizeOfSet プロパティの値が、データでの項目の表示順序と一致することを期待します (0 ベースではなく、自然計数と一致させるために 1 単位のオフセット)。

これを実現する最善の方法は、項目コントロールのオートメーション ピアで [GetPositionInSetCore](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpositioninsetcore) および [GetSizeOfSetCore](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getsizeofsetcore) メソッドを実装し、コントロールによって表されるデータ セットの項目の位置をレポートすることです。 支援技術によるアクセスの実行時にのみ、値が計算され、それを最新の状態に保つことが大変なことではなくなります。 値はデータの順序と一致します。

この例では、_CardControl_ と呼ばれるカスタム コントロールを表示するときに、これをどのように実行するかを示します。

```xaml
<ScrollViewer >
    <ItemsRepeater x:Name="repeater" ItemsSource="{x:Bind MyItemsSource}">
       <ItemsRepeater.ItemTemplate>
           <DataTemplate x:DataType="local:CardViewModel">
               <local:CardControl Item="{x:Bind}"/>
           </DataTemplate>
       </ItemsRepeater.ItemTemplate>
   </ItemsRepeater>
</ScrollViewer>
```

```csharp
internal sealed class CardControl : CardControlBase
{
    protected override AutomationPeer OnCreateAutomationPeer() => new CardControlAutomationPeer(this);

    private sealed class CardControlAutomationPeer : FrameworkElementAutomationPeer
    {
        private readonly CardControl owner;

        public CardControlAutomationPeer(CardControl owner) : base(owner) => this.owner = owner;

        protected override int GetPositionInSetCore()
          => ((ItemsRepeater)owner.Parent)?.GetElementIndex(this.owner) + 1 ?? base.GetPositionInSetCore();

        protected override int GetSizeOfSetCore()
          => ((ItemsRepeater)owner.Parent)?.ItemsSourceView?.Count ?? base.GetSizeOfSetCore();
    }
}
```

## <a name="related-articles"></a>関連記事

- [リスト](lists.md)
- [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)
- [ScrollViewer](/uwp/api/windows.ui.xaml.controls.scrollviewer)