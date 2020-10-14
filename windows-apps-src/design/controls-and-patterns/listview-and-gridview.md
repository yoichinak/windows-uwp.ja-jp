---
description: ListView コントロールと GridView コントロールを使って、イメージ ギャラリーや一連のメール メッセージなどのデータのセットを表示および操作します。
title: リスト ビューとグリッド ビュー
label: List view and grid view
template: detail.hbs
ms.date: 11/04/2019
ms.topic: article
keywords: windows 10, uwp
ms.assetid: f8532ba0-5510-4686-9fcf-87fd7c643e7b
pm-contact: predavid
design-contact: kimsea
dev-contact: ranjeshj
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 26f7e36d09857d37da4a0b4533cc8f65d2789e20
ms.sourcegitcommit: 4f032d7bb11ea98783db937feed0fa2b6f9950ef
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/08/2020
ms.locfileid: "91829645"
---
# <a name="list-view-and-grid-view"></a>リスト ビューとグリッド ビュー

ほとんどのアプリでは、イメージ ギャラリー、メール メッセージなどのデータのセットを操作および表示します。 XAML UI フレームワークでは、アプリ内でデータを簡単に表示、操作するための ListView コントロールと GridView コントロールが用意されています。  

> **重要な API**:[ListView クラス](/uwp/api/windows.ui.xaml.controls.listview)、[GridView クラス](/uwp/api/windows.ui.xaml.controls.gridview)、[ItemsSource プロパティ](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource)、[Items プロパティ](/uwp/api/windows.ui.xaml.controls.itemscontrol.items)

> [!NOTE]
> ListView と GridView はどちらも [ListViewBase](/uwp/api/windows.ui.xaml.controls.listviewbase) クラスから派生しているため、同じ機能を持ちますが、データの表示方法が異なります。 この記事では、特に指定がない限り、リスト ビューについての説明は ListView コントロールにも GridView コントロールにも適用されます。 ListView や ListViewItem などのクラスの説明については、プレフィックスの "*List*" を "*Grid*" に置き換えることで、対応するグリッド クラス (GridView または GridViewItem) に適用できます。 

ListView と GridView は、コレクションと併用すると多くのメリットがあります。 この 2 つはどちらも実装が容易で、基本的な UI、対話、スクロール処理が提供されるだけでなく、簡単にカスタマイズすることもできます。 ListView と GridView は、既存の動的データ ソース、あるいは XAML 自体またはコードビハインドで提供されるハードコーディングされたデータにバインドできます。 

これら 2 つのコントロールは多くのユース ケースで柔軟に利用できますが、最適な動作を得るには、すべての項目が同じ基本的な構造と外観を持ち、同じ対話動作をする (クリックされたときに、リンクの表示や移動など、同じアクションを実行する) コレクションと一緒に使用する必要があります。


## <a name="differences-between-listview-and-gridview"></a>ListView と GridView の違い

### <a name="listview"></a>ListView
ListView では、データを縦方向の 1 列に並べて表示します。 ListView は、焦点としてテキストが含まれる項目や、アルファベット順など、上から下へ読み取ることを意図したコレクションの場合に適しています。 ListView の一般的な使用例としては、メッセージや検索結果の一覧などがあります。 複数の列またはテーブルのような形式で表示する必要があるコレクションでは、ListView を使用しては "_ならず_"、代わりに [DataGrid](/windows/communitytoolkit/controls/datagrid) を使用する必要があります。

![グループ化されたデータを表示するリスト ビュー](images/listview-grouped-example-resized-final.png)

### <a name="gridview"></a>GridView
GridView は、縦方向にスクロールできる複数行と複数列で項目のコレクションを表示します。 データはまず横方向に並べられ、すべての列にデータが入ると、次の行に折り返して並べられます。 GridView は、焦点として画像が含まれる項目や、横方向に読み取ることのできるコレクション、または特定の順序で並び替えられていないコレクションの場合に適しています。 GridView の一般的なユース ケースは、写真や製品ギャラリーです。

![コンテンツ ライブラリの例](images/gridview-simple-example-final.png)

## <a name="which-collection-control-should-you-use-a-comparison-with-itemsrepeater"></a>どのコレクション コントロールを使用するべきか? ItemsRepeater との比較

ListView と GridView は、組み込みの UI と UX を使用して任意のコレクションを表示するためにすぐに使用できるコントロールです。 [ItemsRepeater](./items-repeater.md) コントロールも、コレクションを表示するために使用されますが、UI のニーズに的確に対応したカスタム コントロールを作成するための構成要素として作成されています。 最終的にどのコントロールを使用するかに影響を与える最も重要な相違点を次に示します。

-   ListView と GridView は、豊富な機能を備えたコントロールで、カスタマイズをほとんど必要としませんが、多くのカスタマイズも可能です。 ItemsRepeater は、独自のレイアウト コントロールを作成するための構成要素であり、同じ組み込み機能は備えていません。必要な機能や相互作用を自分で実装する必要があります。
-   ItemsRepeater は、ListView または GridView では作成できない高度なカスタム UI の場合、または項目ごとに大幅に異なる動作を必要とするデータ ソースがある場合に使用する必要があります。


ItemsRepeater の詳細については、[ガイドライン](./items-repeater.md)と [API ドキュメント](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)のページをご覧ください。

## <a name="examples"></a>例

<table>
<th align="left">XAML コントロール ギャラリー<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML コントロール ギャラリー</strong> アプリがインストールされている場合、こちらをクリックしてアプリを開き、<a href="xamlcontrolsgallery:/item/ListView">ListView</a> または <a href="xamlcontrolsgallery:/item/GridView">GridView</a> の動作を確認してください。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML コントロール ギャラリー アプリを入手する (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">ソース コード (GitHub) を入手する</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="create-a-listview-or-gridview"></a>ListView または GridView を作成する

ListView と GridView はどちらも [ItemsControl](/uwp/api/windows.ui.xaml.controls.itemscontrol) 型であるため、任意の型の項目のコレクションを含めることができます。 ListView または GridView を使って画面に項目を表示するには、あらかじめ [Items](/uwp/api/windows.ui.xaml.controls.itemscontrol.items) コレクションに項目を追加しておく必要があります。 ビューのデータを設定するには、項目を直接 [Items](/uwp/api/windows.ui.xaml.controls.itemscontrol.items) コレクションに追加するか、データ ソースに [ItemsSource](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) プロパティを設定します。 

> [!IMPORTANT]
> リストにデータを設定するときには Items または ItemsSource を使用しますが、同時に両方を使用することはできません。 ItemsSource プロパティを設定して XAML で項目を追加した場合、追加された項目は無視されます。 ItemsSource プロパティを設定してコードで Items コレクションに項目を追加した場合、例外がスローされます。

この記事の例の多くでは、説明を簡単にするために `Items` コレクションを直接設定しています。 ただし、リストの項目は、オンライン データベースの書籍一覧など、動的なソースから取得される方が一般的です。 このようなケースでは、`ItemsSource` プロパティを使用します。 

### <a name="add-items-to-a-listview-or-gridview"></a>ListView または GridView に項目を追加する

XAML またはコードを使用して ListView または GridView の [Items](/uwp/api/windows.ui.xaml.controls.itemscontrol.items) コレクションに項目を追加すると、同じ結果が得られます。 通常、項目が少数で変わらず、簡単に定義できる場合や、実行時にコードで項目を生成する場合は、XAML を使用して項目を追加します。 

<u>方法 1:Items コレクションに項目を追加する</u>
#### <a name="option-1-add-items-through-xaml"></a>オプション 1:XAML を使用した項目の追加
```xml
<!-- No corresponding C# code is needed for this example. -->

<ListView x:Name="Fruits"> 
   <x:String>Apricot</x:String> 
   <x:String>Banana</x:String> 
   <x:String>Cherry</x:String> 
   <x:String>Orange</x:String> 
   <x:String>Strawberry</x:String> 
</ListView>  
```


#### <a name="option-2-add-items-through-c"></a>オプション 2:C# を使用した項目の追加

##### <a name="c-code"></a>C# コード:
```csharp
// Create a new ListView and add content. 
ListView Fruits = new ListView(); 
Fruits.Items.Add("Apricot"); 
Fruits.Items.Add("Banana"); 
Fruits.Items.Add("Cherry"); 
Fruits.Items.Add("Orange"); 
Fruits.Items.Add("Strawberry");
 
// Add the ListView to a parent container in the visual tree (that you created in the corresponding XAML file).
FruitsPanel.Children.Add(Fruits); 
```

##### <a name="corresponding-xaml-code"></a>対応する XAML コード:
```xml
<StackPanel Name="FruitsPanel"></StackPanel>
```
上記のどちらのオプションでも、次に示すような同じ ListView が生成されます。

![果物の一覧を表示している単純なリスト ビューのスクリーンショット。](images/listview-basic-code-example2.png)
<br/>
<u>方法 2:ItemsSource を設定して項目を追加する</u>

通常、ListView または GridView では、データベースやインターネットなどのソースからデータを表示します。 データ ソースから ListView/GridView にデータを設定するには、[ItemsSource](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) プロパティをデータ項目のコレクションに設定します。 この方法は、次の例に示すように、ListView または GridView にカスタム クラスのオブジェクトを保持する場合に適しています。

#### <a name="option-1-set-itemssource-in-c"></a>オプション 1:C# で ItemsSource を設定する
以下の例では、コードでコレクションのインスタンスに直接リスト ビューの ItemsSource を設定しています。 

##### <a name="c-code"></a>C# コード:
```csharp 
// Class defintion should be provided within the namespace being used, outside of any other classes.

this.InitializeComponent();

// Instead of adding hard coded items to an ObservableCollection as shown below, 
//the data could be pulled asynchronously from a database or the internet.
ObservableCollection<Contact> Contacts = new ObservableCollection<Contact>();

// Contact objects are created by providing a first name, last name, and company for the Contact constructor.
// They are then added to the ObservableCollection Contacts.
Contacts.Add(new Contact("John", "Doe", "ABC Printers"));
Contacts.Add(new Contact("Jane", "Doe", "XYZ Refridgerators"));
Contacts.Add(new Contact("Santa", "Claus", "North Pole Toy Factory Inc."));

// Create a new ListView (or GridView) for the UI, add content by setting ItemsSource
ListView ContactsLV = new ListView();
ContactsLV.ItemsSource = Contacts;

// Add the ListView to a parent container in the visual tree (that you created in the corresponding XAML file)
ContactPanel.Children.Add(ContactsLV);
```

##### <a name="xaml-code"></a>XAML コード:
```xml
<StackPanel x:Name="ContactPanel"></StackPanel>
```

#### <a name="option-2-set-itemssource-in-xaml"></a>オプション 2:XAML で ItemsSource を設定する
ItemsSource プロパティを、XAML でコレクションにバインドすることもできます。 ここでは、ページのプライベート データ コレクション `_contacts` を公開する `Contacts` という名前のパブリック プロパティに ItemsSource をバインドします。

**XAML**
```xml
<ListView x:Name="ContactsLV" ItemsSource="{x:Bind Contacts}"/>
```

**C#**
```csharp
// Class defintion should be provided within the namespace being used, outside of any other classes.
// These two declarations belong outside of the main page class.
private ObservableCollection<Contact> _contacts = new ObservableCollection<Contact>();

public ObservableCollection<Contact> Contacts
{
    get { return this._contacts; }
}

// This method should be defined within your main page class.
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    base.OnNavigatedTo(e);

    // Instead of hard coded items, the data could be pulled 
    // asynchronously from a database or the internet.
    Contacts.Add(new Contact("John", "Doe", "ABC Printers"));
    Contacts.Add(new Contact("Jane", "Doe", "XYZ Refridgerators"));
    Contacts.Add(new Contact("Santa", "Claus", "North Pole Toy Factory Inc."));
}
```

上記のどちらのオプションでも、次に示すような同じ ListView が生成されます。 この ListView には、データ テンプレートを指定しなかったため、各項目の文字列表現のみが表示されます。

![ItemsSource で設定した単純なリスト ビュー](images/listview-basic-code-example-final.png)

> [!IMPORTANT]
> データ テンプレートを定義しない場合、カスタム クラス オブジェクトで ListView に文字列値が表示されるのは、[ToString ()](/uwp/api/windows.foundation.istringable.tostring) メソッドが定義されている場合のみです。

 次のセクションでは、ListView または GridView で、単純なカスタム クラス項目を視覚的に表現する方法について詳しく説明します。

データ バインディングについて詳しくは、「[データ バインディングの概要](../../data-binding/data-binding-quickstart.md)」をご覧ください。

> [!NOTE]
> リスト ビューにグループ化されたデータを表示する必要がある場合は、[CollectionViewSource](/uwp/api/Windows.UI.Xaml.Data.CollectionViewSource) にバインドします。 CollectionViewSource は XAML のコレクション クラスのプロキシとして機能し、グループ化サポートを有効にします。 詳しくは、[CollectionViewSource](/uwp/api/Windows.UI.Xaml.Data.CollectionViewSource) をご覧ください。

## <a name="customizing-the-look-of-items-with-a-datatemplate"></a>DataTemplate を使用して項目の外観をカスタマイズする

ListView または GridView のデータ テンプレートでは、項目やデータをどのように視覚化するかを定義します。 既定では、データ項目は、バインドされているデータ オブジェクトの文字列表現として ListView に表示されます。 データ項目の特定のプロパティに [DisplayMemberPath](/uwp/api/windows.ui.xaml.controls.itemscontrol.displaymemberpath) を設定すると、そのプロパティの文字列表現を表示できます。

しかし、通常はもっとリッチな表現でデータを表示する必要があります。 ListView/GridView で項目を表示する方法を正確に指定するには、[DataTemplate](/uwp/api/Windows.UI.Xaml.DataTemplate) を作成します。 DataTemplate の XAML では、個々の項目を表示するために使うコントロールのレイアウトと外観を定義します。 レイアウト内のコントロールでは、データ オブジェクトのプロパティにバインドすることも、静的コンテンツをインラインで定義することもできます。 

> [!NOTE]
> DataTemplate で [x:Bind マークアップ拡張](../../xaml-platform/x-bind-markup-extension.md)を使う場合、DataTemplate に DataType (`x:DataType`) を指定する必要があります。

#### <a name="simple-listview-data-template"></a>単純な ListView データ テンプレート
この例では、データ項目は単純な文字列です。 DataTemplate は ListView 定義内にインラインで定義し、文字列の左側に画像を追加し、文字列を青緑色で表示します。 これは、上記の方法 1 とオプション 1 を使用して作成したものと同じ ListView です。

**XAML**
```XML
<!--No corresponding C# code is needed for this example.-->
<ListView x:Name="FruitsList">
                <ListView.ItemTemplate>
                    <DataTemplate x:DataType="x:String">
                        <Grid>
                            <Grid.ColumnDefinitions>
                                <ColumnDefinition Width="47"/>
                                <ColumnDefinition/>
                            </Grid.ColumnDefinitions>
                            <Image Source="Assets/placeholder.png" Width="32" Height="32"
                                HorizontalAlignment="Left" VerticalAlignment="Center"/>
                            <TextBlock Text="{x:Bind}" Foreground="Teal" FontSize="14" 
                                Grid.Column="1" VerticalAlignment="Center"/>
                        </Grid>
                    </DataTemplate>
                </ListView.ItemTemplate>
                <x:String>Apricot</x:String>
                <x:String>Banana</x:String>
                <x:String>Cherry</x:String>
                <x:String>Orange</x:String>
                <x:String>Strawberry</x:String>
            </ListView>

```

このデータ テンプレートを使って ListView にデータ項目を表示すると、次のようになります。

![データ テンプレートを使った ListView 項目](images/listview-w-datatemplate1-final.png)

#### <a name="listview-data-template-for-custom-class-objects"></a>カスタム クラス オブジェクト用の ListView データ テンプレート
この例では、データ項目は Contact オブジェクトです。 DataTemplate は ListView 定義内にインラインで定義し、連絡先の名前と会社の左側に連絡先の画像を追加します。 この ListView は、前述のメソッド 2 とオプション 2 を使用して作成したものです。
```xml
<ListView x:Name="ContactsLV" ItemsSource="{x:Bind Contacts}">
    <ListView.ItemTemplate>
        <DataTemplate x:DataType="local:Contact">
            <Grid>
                <Grid.RowDefinitions>
                    <RowDefinition Height="*"/>
                    <RowDefinition Height="*"/>
                </Grid.RowDefinitions>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="Auto"/>
                    <ColumnDefinition Width="*"/>
                </Grid.ColumnDefinitions>
                <Image Grid.Column="0" Grid.RowSpan="2" Source="Assets/grey-placeholder.png" Width="32"
                    Height="32" HorizontalAlignment="Center" VerticalAlignment="Center"></Image>
                <TextBlock Grid.Column="1" Text="{x:Bind Name}" Margin="12,6,0,0" 
                    Style="{ThemeResource BaseTextBlockStyle}"/>
                <TextBlock  Grid.Column="1" Grid.Row="1" Text="{x:Bind Company}" Margin="12,0,0,6" 
                    Style="{ThemeResource BodyTextBlockStyle}"/>
            </Grid>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

このデータ テンプレートを使用して ListView にデータ項目を表示すると、次のようになります。

![データ テンプレートを使った ListView のカスタム クラス項目](images/listview-customclass-datatemplate-final.png)

データ テンプレートは、ListView の外観を定義する主要な方法です。 また、リストに多数の項目が含まれている場合、パフォーマンスが大幅に低下することがあります。  

データ テンプレートは、前述のように ListView/GridView 定義内にインラインで定義できるほか、リソース セクション内で別個に定義することもできます。 ListView/GridView 自体の外部で定義する場合、DataTemplate には [x:Key](../../xaml-platform/x-key-attribute.md) 属性を指定する必要があり、そのキーを使用して ListView または GridView の [ItemTemplate](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate) プロパティに DataTemplate を割り当てる必要があります。

データ テンプレートと項目コンテナーを使用してリストまたはグリッドの項目の外観を定義する詳しい方法とその例については、「[項目コンテナーやテンプレート](item-containers-templates.md)」をご覧ください。 

## <a name="change-the-layout-of-items"></a>項目のレイアウト変更

ListView または GridView に項目を追加すると、コントロールによって項目コンテナー内で各項目が自動的に折り返され、すべての項目コンテナーがレイアウトされます。 この項目コンテナーのレイアウト方法は、コントロールの [ItemsPanel](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemspanel) によって決まります。  
- **ListView** では既定で [ItemsStackPanel](/uwp/api/windows.ui.xaml.controls.itemsstackpanel) が使用され、次のような縦 1 列のリストが生成されます。

![項目の一覧を表示している単純なリスト ビューのスクリーンショット。](images/listview-simple.png)

- **GridView** では [ItemsWrapGrid](/uwp/api/windows.ui.xaml.controls.itemswrapgrid) が使用されます。次のように、項目が水平方向に追加され、折り返されて縦方向にスクロールします。

![単純なグリッド ビュー](images/gridview-simple.png)

項目のレイアウトを変更するには、項目パネルのプロパティを調整するか、既定のパネルを別のパネルに置き換えます。

> [!NOTE]
> ItemsPanel を変更する場合、仮想化を無効にしないようにご注意ください。 **ItemsStackPanel** と **ItemsWrapGrid** はどちらも仮想化をサポートしており、安全に使用できます。 他のパネルを使用すると、仮想化が無効になり、リスト ビューのパフォーマンスが低下する場合があります。 詳しくは、「[Performance (パフォーマンス)](../../debug-test-perf/performance-and-xaml-ui.md)」のリスト ビューに関する記事をご覧ください。 

この例では、**ItemsStackPanel** の [Orientation](/uwp/api/windows.ui.xaml.controls.itemsstackpanel.orientation) プロパティを変更することで、**ListView** に項目コンテナーを横 1 列にレイアウトする方法を示しています。
リスト ビューは既定で縦方向にスクロールするため、横方向にスクロールさせるには、リスト ビュー内部の [ScrollViewer](/uwp/api/windows.ui.xaml.controls.scrollviewer) のプロパティもいくつか調整する必要があります。
- [ScrollViewer.HorizontalScrollMode](/uwp/api/windows.ui.xaml.controls.scrollviewer.horizontalscrollmode) を **Enabled** または **Auto** に設定
- [ScrollViewer.HorizontalScrollBarVisibility](/uwp/api/windows.ui.xaml.controls.scrollviewer.horizontalscrollbarvisibility) を **Auto** に設定 
- [ScrollViewer.VerticalScrollMode](/uwp/api/windows.ui.xaml.controls.scrollviewer.verticalscrollmode) を **Disabled** に設定 
- [ScrollViewer.VerticalScrollBarVisibility](/uwp/api/windows.ui.xaml.controls.scrollviewer.verticalscrollbarvisibility) を **Hidden** に設定 

> [!IMPORTANT]
> ここに示す例ではリスト ビューの幅が規定されていないため、水平スクロール バーは表示されません。 このコードを実行する場合、ListView に `Width="180"` を設定してスクロール バーを表示することができます。

**XAML**
```xml
<ListView Height="60" 
          ScrollViewer.HorizontalScrollMode="Enabled" 
          ScrollViewer.HorizontalScrollBarVisibility="Auto"
          ScrollViewer.VerticalScrollMode="Disabled"
          ScrollViewer.VerticalScrollBarVisibility="Hidden">
    <ListView.ItemsPanel>
        <ItemsPanelTemplate>
            <ItemsStackPanel Orientation="Horizontal"/>
        </ItemsPanelTemplate>
    </ListView.ItemsPanel>
    <x:String>Apricot</x:String>
    <x:String>Banana</x:String>
    <x:String>Cherry</x:String>
    <x:String>Orange</x:String>
    <x:String>Strawberry</x:String>
</ListView>
```

このリストは次のように表示されます。

![横長のリスト ビュー](images/listview-horizontal2-final.png)

 次の例の **ListView** では、**ItemsStackPanel** ではなく **ItemsWrapGrid** を使用して、縦方向に折り返すリストで項目をレイアウトします。 
 
> [!IMPORTANT]
> コントロールでコンテナーを折り返すには、リスト ビューの高さを規定する必要があります。

**XAML**
```xml
<ListView Height="100"
          ScrollViewer.HorizontalScrollMode="Enabled" 
          ScrollViewer.HorizontalScrollBarVisibility="Auto"
          ScrollViewer.VerticalScrollMode="Disabled"
          ScrollViewer.VerticalScrollBarVisibility="Hidden">
    <ListView.ItemsPanel>
        <ItemsPanelTemplate>
            <ItemsWrapGrid/>
        </ItemsPanelTemplate>
    </ListView.ItemsPanel>
    <x:String>Apricot</x:String>
    <x:String>Banana</x:String>
    <x:String>Cherry</x:String>
    <x:String>Orange</x:String>
    <x:String>Strawberry</x:String>
</ListView>
```

このリストは次のように表示されます。

![グリッド レイアウトを使ったリスト ビュー](images/listview-itemswrapgrid2-final.png)

グループ化したデータをリスト ビューに表示する場合、ItemsPanel によって指定されるのは個々の項目のレイアウト方法ではなく、項目グループのレイアウト方法です。たとえば、前に示した横方向の ItemsStackPanel を使用して、グループ化されたデータを表示する場合、次に示すようにグループは横方向に配置されますが、各グループの項目は縦に重ねて表示されます。

![グループ化された横方向のリスト ビュー](images/listview-horizontal-groups.png)

## <a name="item-selection-and-interaction"></a>項目の選択と操作

ユーザーがリスト ビューを操作する方法は、いくつか選ぶことができます。 既定では、ユーザーは 1 つの項目を選択できます。 [SelectionMode](/uwp/api/windows.ui.xaml.controls.listviewbase.selectionmode) プロパティを変更することで、複数選択を有効にしたり、選択を無効にしたりできます。 [IsItemClickEnabled](/uwp/api/windows.ui.xaml.controls.listviewbase.isitemclickenabled) プロパティを設定することで、ユーザーが項目をクリックしたときに項目を選択するのではなく、ボタンと同じように操作を呼び出すことができます。

> **注**&nbsp;&nbsp; ListView と GridView のどちらも、SelectionMode プロパティについては [ListViewSelectionMode](/uwp/api/windows.ui.xaml.controls.listviewselectionmode) 列挙値を使用します。 IsItemClickEnabled は既定で **False** であるため、設定する必要があるのはクリック モードを有効にする場合のみです。

次の表に、ユーザーがリスト ビューを操作する方法と、その操作に対する応答方法を示します。

有効にする操作: | 使用する設定: | 処理するイベント: | 選択された項目の取得に使うプロパティ:
----------------------------|---------------------|--------------------|--------------------------------------------
操作なし | [SelectionMode](/uwp/api/windows.ui.xaml.controls.listviewbase.selectionmode) = **None**、[IsItemClickEnabled](/uwp/api/windows.ui.xaml.controls.listviewbase.isitemclickenabled) = **False** | なし | なし 
単一選択 | SelectionMode = **Single**、IsItemClickEnabled = **False** | [SelectionChanged](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) | [SelectedItem](/uwp/api/windows.ui.xaml.controls.primitives.selector.selecteditem)、[SelectedIndex](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedindex)  
複数選択 | SelectionMode = **Multiple**、IsItemClickEnabled = **False** | [SelectionChanged](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) | [SelectedItems](/uwp/api/windows.ui.xaml.controls.listviewbase.selecteditems)  
拡張選択 | SelectionMode = **Extended**、IsItemClickEnabled = **False** | [SelectionChanged](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) | [SelectedItems](/uwp/api/windows.ui.xaml.controls.listviewbase.selecteditems)  
クリック | SelectionMode = **None**、IsItemClickEnabled = **True** | [ItemClick](/uwp/api/windows.ui.xaml.controls.listviewbase.itemclick) | なし 

> **注**&nbsp;&nbsp;Windows 10 以降では、IsItemClickEnabled を有効にして ItemClick イベントを発生させる場合でも、SelectionMode を Single、Multiple、Extended のいずれにも設定できます。 その場合、ItemClick イベントが最初に発生し、次に SelectionChanged イベントが発生します。 ただし ItemClick イベント ハンドラーで別のページに移動するような場合では、SelectionChanged イベントが発生せず、項目が選択されません。

次に示すように、このプロパティを XAML またはコードで設定することができます。

**XAML**
```xaml
<ListView x:Name="myListView" SelectionMode="Multiple"/>

<GridView x:Name="myGridView" SelectionMode="None" IsItemClickEnabled="True"/> 
```

**C#**
```csharp
myListView.SelectionMode = ListViewSelectionMode.Multiple; 

myGridView.SelectionMode = ListViewSelectionMode.None;
myGridView.IsItemClickEnabled = true;
```

### <a name="read-only"></a>読み取り専用

SelectionMode プロパティを **ListViewSelectionMode.None** に設定することで、項目の選択を無効にすることができます。 これによりコントロールが読み取り専用モードになり、ユーザーの操作には使われず、データの表示のみに使われます。 コントロール自体は無効にならず、項目の選択のみが無効になります。

### <a name="single-selection"></a>単一選択

次の表では、SelectionMode が **Single** の場合のキーボード操作、マウス操作、タッチ操作について説明します。

修飾キー | 操作
-------------|------------
None | <li>ユーザーはスペース バー、マウスのクリック、タッチ操作のタップを使って 1 つの項目を選択できます。</li>
Ctrl | <li>ユーザーはスペース バー、マウスのクリック、タッチ操作のタップを使って 1 つの項目の選択を解除できます。</li><li>方向キーを使うと、選択した項目とは関係なくフォーカスを移動できます。</li>

SelectionMode が **Single** の場合、[SelectedItem](/uwp/api/windows.ui.xaml.controls.primitives.selector.selecteditem) プロパティから選択したデータ項目を取得できます。 [SelectedIndex](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedindex) プロパティを使って、選択した項目のコレクション内のインデックスを取得できます。 項目が選択されていない場合、SelectedItem は **null** になり、SelectedIndex は -1 になります。 
 
**Items** コレクションに含まれない項目を **SelectedItem** として設定しようとすると、その操作は無視され、SelectedItem が **null** になります。 ただし、リスト内の **Items** の範囲外のインデックスを **SelectedIndex** に設定しようとすると、**System.ArgumentException** 例外が発生します。 

### <a name="multiple-selection"></a>複数選択

次の表では、SelectionMode が **Multiple** の場合のキーボード操作、マウス操作、タッチ操作について説明します。

修飾キー | 操作
-------------|------------
None | <li>ユーザーは、スペース バー、マウスのクリック、タッチ操作のタップを使って、フォーカスのある項目の選択状態を切り替えることで、複数の項目を選択できます。</li><li>方向キーを使うと、選択した項目とは関係なくフォーカスを移動できます。</li>
Shift キー | <li>ユーザーは、選択する最初の項目をクリックまたはタップした後、選択する最後の項目をクリックまたはタップすることで、複数の項目を連続的に選択できます。</li><li>方向キーを使用すると、Shift キーが押されたときに選択された項目を起点として連続的に選択することができます。</li>

### <a name="extended-selection"></a>拡張選択

次の表では、SelectionMode が **Extended** の場合のキーボード操作、マウス操作、タッチ操作について説明します。

修飾キー | 操作
-------------|------------
None | <li>**Single** の選択と同じです。</li>
Ctrl | <li>ユーザーは、スペース バー、マウスのクリック、タッチ操作のタップを使って、フォーカスのある項目の選択状態を切り替えることで、複数の項目を選択できます。</li><li>方向キーを使うと、選択した項目とは関係なくフォーカスを移動できます。</li>
Shift キー | <li>ユーザーは、選択する最初の項目をクリックまたはタップした後、選択する最後の項目をクリックまたはタップすることで、複数の項目を連続的に選択できます。</li><li>方向キーを使用すると、Shift キーが押されたときに選択された項目を起点として連続的に選択することができます。</li>

SelectionMode が **Multiple** または **Extended** の場合、選択したデータ項目を [SelectedItems](/uwp/api/windows.ui.xaml.controls.listviewbase.selecteditems) プロパティから取得できます。 

**SelectedIndex**、**SelectedItem**、**SelectedItems** の各プロパティは同期されます。 たとえば、SelectedIndex を -1 に設定すると、SelectedItem は **null** に設定され、SelectedItems が空になります。SelectedItem を **null** に設定すると、SelectedIndex が -1 に設定され、SelectedItems が空になります。

複数選択モードの場合、**SelectedItem** には最初に選択された項目が含まれ、**Selectedindex** には最初に選択した項目のインデックスが含まれます。 

### <a name="respond-to-selection-changes"></a>選択範囲の変更への対応

リスト ビューにおける選択内容の変更に対応するには、[SelectionChanged](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) イベントを処理します。 イベント ハンドラーのコードでは、[SelectionChangedEventArgs.AddedItems](/uwp/api/windows.ui.xaml.controls.selectionchangedeventargs.addeditems) プロパティから選ばれた項目の一覧を取得できます。 選択が解除された項目は、[SelectionChangedEventArgs.RemovedItems](/uwp/api/windows.ui.xaml.controls.selectionchangedeventargs.removeditems) プロパティから取得できます。 ユーザーが Shift キーを押しながら一連の項目を選択しない限り、AddedItems コレクションと RemovedItems コレクションに含まれる項目の数は 1 個までになります。

次の例では、**SelectionChanged** イベントを処理してさまざまな項目コレクションにアクセスする方法を示します。

**XAML**
```xml
<StackPanel HorizontalAlignment="Right">
    <ListView x:Name="listView1" SelectionMode="Multiple" 
              SelectionChanged="ListView1_SelectionChanged">
        <x:String>Apricot</x:String>
        <x:String>Banana</x:String>
        <x:String>Cherry</x:String>
        <x:String>Orange</x:String>
        <x:String>Strawberry</x:String>
    </ListView>
    <TextBlock x:Name="selectedItem"/>
    <TextBlock x:Name="selectedIndex"/>
    <TextBlock x:Name="selectedItemCount"/>
    <TextBlock x:Name="addedItems"/>
    <TextBlock x:Name="removedItems"/>
</StackPanel> 
```

**C#**
```csharp
private void ListView1_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    if (listView1.SelectedItem != null)
    {
        selectedItem.Text = 
            "Selected item: " + listView1.SelectedItem.ToString();
    }
    else
    {
        selectedItem.Text = 
            "Selected item: null";
    }
    selectedIndex.Text = 
        "Selected index: " + listView1.SelectedIndex.ToString();
    selectedItemCount.Text = 
        "Items selected: " + listView1.SelectedItems.Count.ToString();
    addedItems.Text = 
        "Added: " + e.AddedItems.Count.ToString();
    removedItems.Text = 
        "Removed: " + e.RemovedItems.Count.ToString();
}
```

### <a name="click-mode"></a>クリック モード

項目を選択するのではなく、ボタンのように項目をクリックできるように、リスト ビューを変更することができます。 この方法は、たとえば、リストまたはグリッド内の項目をユーザーがクリックしたときに新しいページに移動するアプリで便利です。 この動作を有効にするには、次のように設定します。
- **SelectionMode** を **None** に設定します。
- **IsItemClickEnabled** を **true** に設定します。
- ユーザーが項目をクリックしたときに処理を実行する **ItemClick** イベントを設定します。

クリックできる項目を持つリスト ビューの例を次に示します。 ItemClick イベント ハンドラーのコードによって、新しいページに移動します。

**XAML**
```xml
<ListView SelectionMode="None"
          IsItemClickEnabled="True" 
          ItemClick="ListView1_ItemClick">
    <x:String>Page 1</x:String>
    <x:String>Page 2</x:String>
    <x:String>Page 3</x:String>
    <x:String>Page 4</x:String>
    <x:String>Page 5</x:String>
</ListView>
```

**C#**
```csharp
private void ListView1_ItemClick(object sender, ItemClickEventArgs e)
{
    switch (e.ClickedItem.ToString())
    {
        case "Page 1":
            this.Frame.Navigate(typeof(Page1));
            break;

        case "Page 2":
            this.Frame.Navigate(typeof(Page2));
            break;

        case "Page 3":
            this.Frame.Navigate(typeof(Page3));
            break;

        case "Page 4":
            this.Frame.Navigate(typeof(Page4));
            break;

        case "Page 5":
            this.Frame.Navigate(typeof(Page5));
            break;

        default:
            break;
    }
}
```

### <a name="select-a-range-of-items-programmatically"></a>プログラムを使った一定範囲の項目の選択

場合によっては、プログラムからリスト ビューの項目の選択を操作する必要があります。 たとえば、ユーザーがリスト内のすべての項目を選択できるように、 **[すべて選択]** ボタンを用意する場合があります。 この場合、SelectedItems コレクションの項目を 1 つずつ追加したり削除したりすることは、一般的に効率的とは言えません。 各項目を変更するたびに SelectionChanged イベントが発生し、インデックス値を操作するのではなく項目を直接操作すると、項目の仮想化が解除されます。

[SelectAll](/uwp/api/windows.ui.xaml.controls.listviewbase.selectall)、[SelectRange](/uwp/api/windows.ui.xaml.controls.listviewbase.selectrange)、[DeselectRange](/uwp/api/windows.ui.xaml.controls.listviewbase.deselectrange) の各メソッドを使用すると、SelectedItems プロパティを使用する場合よりも効率的に選択内容を変更できます。 このメソッドでは、項目インデックスの範囲を使って選択と選択解除を行います。 インデックスのみを使うため、仮想化された項目は仮想化の状態を維持します。 元の選択状態に関係なく、指定範囲のすべての項目が選択 (または選択解除) されます。 SelectionChanged イベントは、このメソッドを 1 回呼び出すたびに 1 回のみ発生します。

> [!IMPORTANT]
> このメソッドは、SelectionMode プロパティが Multiple または Extended に設定されているときにのみ呼び出してください。 SelectionMode が Single または None のときに SelectRange を呼び出すと、例外がスローされます。

インデックスの範囲を使って項目を選択する場合、[SelectedRanges](/uwp/api/windows.ui.xaml.controls.listviewbase.selectedranges) プロパティを使って、リスト内の選択範囲をすべて取得します。

ItemsSource に [IItemsRangeInfo](/uwp/api/windows.ui.xaml.data.iitemsrangeinfo) を実装し、このようなメソッドを使って選択内容を変更する場合、SelectionChangedEventArgs には **AddedItems** プロパティと **RemovedItems** プロパティが設定されません。 このプロパティを設定するには、項目オブジェクトの仮想化を解除する必要があります。 代わりに **SelectedRanges** プロパティを使って項目を取得します。

SelectAll メソッドを呼び出すと、コレクション内のすべての項目を選択できます。 ただし、すべての項目の選択を解除するメソッドはありません。 すべての項目の選択を解除するには、DeselectRange を呼び出して [ItemIndexRange](/uwp/api/windows.ui.xaml.data.itemindexrange) を渡します。このとき、[FirstIndex](/uwp/api/windows.ui.xaml.data.itemindexrange.firstindex) 値を 0 とし、[Length](/uwp/api/windows.ui.xaml.data.itemindexrange.length) 値をコレクション内の項目数と同じ値にします。 次の例では、すべての項目を選択するためのオプションが示されています。

**XAML**
```xml
<StackPanel Width="160">
    <Button Content="Select all" Click="SelectAllButton_Click"/>
    <Button Content="Deselect all" Click="DeselectAllButton_Click"/>
    <ListView x:Name="listView1" SelectionMode="Multiple">
        <x:String>Apricot</x:String>
        <x:String>Banana</x:String>
        <x:String>Cherry</x:String>
        <x:String>Orange</x:String>
        <x:String>Strawberry</x:String>
    </ListView>
</StackPanel>
```

**C#**
```csharp
private void SelectAllButton_Click(object sender, RoutedEventArgs e)
{
    if (listView1.SelectionMode == ListViewSelectionMode.Multiple ||
        listView1.SelectionMode == ListViewSelectionMode.Extended)
    {
        listView1.SelectAll();
    }
}

private void DeselectAllButton_Click(object sender, RoutedEventArgs e)
{
    if (listView1.SelectionMode == ListViewSelectionMode.Multiple ||
        listView1.SelectionMode == ListViewSelectionMode.Extended)
    {
        listView1.DeselectRange(new ItemIndexRange(0, (uint)listView1.Items.Count));
    }
}
```

選択した項目の外観を変更する方法について詳しくは、「[項目コンテナーやテンプレート](item-containers-templates.md)」をご覧ください。

### <a name="drag-and-drop"></a>ドラッグ アンド ドロップ

ListView コントロールと GridView コントロールは、項目内、項目間、および他の ListView コントロールと GridView コントロール間での項目のドラッグ アンド ドロップをサポートします。 ドラッグ アンド ドロップのパターンの実装について詳しくは、「[ドラッグ アンド ドロップ](../input/drag-and-drop.md)」をご覧ください。

## <a name="get-the-sample-code"></a>サンプル コードの入手

- [XAML ListView と GridView のサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlListView)- ListView と GridView コントロールを示しています。
- [XAML ドラッグ アンド ドロップのサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlDragAndDrop) - ListView コントロールを使用したドラッグ アンド ドロップを示します。
- [XAML コントロール ギャラリー サンプル](https://github.com/Microsoft/Xaml-Controls-Gallery) - インタラクティブな形で XAML コントロールのすべてを参照できます。

## <a name="related-articles"></a>関連記事

- [リスト](lists.md)
- [項目コンテナーやテンプレート](item-containers-templates.md)
- [ドラッグ アンド ドロップ](../input/drag-and-drop.md)
