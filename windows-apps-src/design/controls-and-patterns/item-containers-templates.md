---
Description: テンプレートを使って、ListView コントロールや GridView コントロールの項目の外観を変更します。
title: 項目コンテナーとテンプレート
label: Item containers and templates
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: d8eb818d-b62e-4314-a612-f29142dbd93f
pm-contact: predavid
design-contact: kimsea
dev-contact: ranjeshj
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 505e03124f345b8b32c6b3454ffa4aad32a72e29
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172756"
---
# <a name="item-containers-and-templates"></a>項目コンテナーとテンプレート

 

**ListView** コントロールと **GridView** コントロールでは、項目の配置方法 (水平、垂直、折り返しなど) や、ユーザーが項目を操作する方法を管理しますが、画面に個別の項目を表示する方法については管理しません。 項目の視覚エフェクトは、項目コンテナーによって管理されます。 リスト ビューに項目を追加すると、それらはコンテナーに自動的に配置されます。 ListView の既定の項目コンテナーは [ListViewItem](/uwp/api/Windows.UI.Xaml.Controls.ListViewItem) であり、GridView の場合は [GridViewItem](/uwp/api/Windows.UI.Xaml.Controls.GridViewItem) です。

> **重要な API**:[ListView クラス](/uwp/api/windows.ui.xaml.controls.listview)、[GridView クラス](/uwp/api/windows.ui.xaml.controls.gridview)、[ListViewItem クラス](/uwp/api/windows.ui.xaml.controls.listviewitem)、[GridViewItem クラス](/uwp/api/windows.ui.xaml.controls.gridviewitem)、[ItemTemplate プロパティ](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate)、[ItemContainerStyle プロパティ](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemcontainerstyle)


> [!NOTE]
> ListView と GridView はどちらも [ListViewBase](/uwp/api/windows.ui.xaml.controls.listviewbase) クラスから派生しているため、同じ機能を持ちますが、データの表示方法が異なります。 この記事では、特に指定がない限り、リスト ビューについての説明は ListView コントロールにも GridView コントロールにも適用されます。 ListView や ListViewItem などのクラスの説明については、プレフィックスの "*List*" を "*Grid*" に置き換えることで、対応するグリッド クラス (GridView または GridViewItem) に適用できます。 

## <a name="listview-items-and-gridview-items"></a>ListView 項目と GridView 項目
前述のように、ListView 項目は ListViewItem コンテナーに自動的に配置され、GridView 項目は GridViewItem コンテナーに配置されます。 これらの項目コンテナーは、独自の組み込みスタイルと対話機能を備えたコントロールですが、高度にカスタマイズすることもできます。 ただし、カスタマイズする前に、ListViewItem と GridViewItem の推奨されるスタイルとガイドラインを良く確認してください。

- **ListViewItems** - 主にテキストに重点をおいた項目で、細長い形状です。 テキストの左側にアイコンまたは画像が表示されることがあります。
- **GridViewItems** - 通常、項目は正方形であるか、または少なくともそれほど細長くない四角形です。 項目は画像に重点をおいており、画像の周囲またはその上にテキストが表示される場合があります。 

## <a name="introduction-to-customization"></a>カスタマイズの概要
コンテナー コントロール (ListViewItem や GridViewItem など) は、 *"データ テンプレート"* と *"コントロール テンプレート"* という 2 つの重要な部分から構成されており、これらを組み合わせることによって 1 つの項目で表示する最終的な外観が形成されます。

- **データ テンプレート** - [DataTemplate](/uwp/api/Windows.UI.Xaml.DataTemplate) をリスト ビューの [ItemTemplate](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate) プロパティに割り当てて、個別のデータ項目の表示方法を指定します。
- **コントロール テンプレート** - コントロール テンプレートは、表示状態など、フレームワークが担当する項目の視覚エフェクトの一部を提供します。 [ItemContainerStyle](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemcontainerstyle) プロパティを使って、コントロール テンプレートを変更できます。 通常では、これを使用して、ブランドに合うようにリスト ビューの色を変更したり、選択した項目の表示方法を変更したりします。

次の画像は、コントロール テンプレートとデータ テンプレートを合わせて 1 つの項目で表示する最終的なビジュアルを形成する方法を示しています。

![リスト ビュー コントロールとデータ テンプレート](images/listview-visual-parts.png)

この項目を作成する XAML を次に示します。 テンプレートについては、後で説明します。

```xaml
<ListView Width="220" SelectionMode="Multiple">
    <ListView.ItemTemplate>
        <DataTemplate x:DataType="x:String">
            <Grid Background="Yellow">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="54"/>
                    <ColumnDefinition/>
                </Grid.ColumnDefinitions>
                <Image Source="Assets/placeholder.png" Width="44" Height="44"
                       HorizontalAlignment="Left"/>
                <TextBlock Text="{x:Bind}" Foreground="Black"
                           FontSize="14" Grid.Column="1"
                           VerticalAlignment="Center"
                           Padding="0,0,54,0"/>
            </Grid>
        </DataTemplate>
    </ListView.ItemTemplate>
    <ListView.ItemContainerStyle>
        <Style TargetType="ListViewItem">
            <Setter Property="Background" Value="LightGreen"/>
        </Style>
    </ListView.ItemContainerStyle>
    <x:String>Item 1</x:String>
    <x:String>Item 2</x:String>
    <x:String>Item 3</x:String>
    <x:String>Item 4</x:String>
    <x:String>Item 5</x:String>
</ListView>
```

> [!IMPORTANT]
> データ テンプレートとコントロール テンプレートは、ListView と GridView 以外の多くのコントロールのスタイルをカスタマイズするために使用されます。 これには、FlipView などの独自の組み込みスタイルを持つコントロールや、ItemsRepeater などのカスタム作成されたコントロールが含まれます。 次の例は ListView/GridView に固有のものですが、その概念は他の多くのコントロールに適用できます。 
 
## <a name="prerequisites"></a>前提条件

- リスト ビュー コントロールの使い方を理解していることを前提としています。 詳しくは、[ListView と GridView](listview-and-gridview.md)に関する記事をご覧ください。
- また、スタイルをインラインで使用する方法や、リソースとして使用する方法を含む、コントロール スタイルやテンプレートについて理解していることも前提としています。 詳しくは、[コントロールのスタイル](xaml-styles.md)に関するページと「[コントロール テンプレート](control-templates.md)」をご覧ください。

## <a name="the-data"></a>データ

リスト ビューでデータ項目を表示する方法について詳しく調べる前に、表示されるデータについて理解する必要があります。 この例では、`NamedColor` と呼ばれるデータ型を作成します。 これは、`Name`、`Color`、`Brush` という 3 つのプロパティとして公開されている、色の名前、色の値、**SolidColorBrush** が組み合わされています。
 
次に、[Colors](/uwp/api/windows.ui.colors) クラスの名前付きの色ごとに `NamedColor` オブジェクトを使って、**List** を設定します。 このリストは、リスト ビューの [ItemsSource](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) として設定されます。

クラスを定義したり、`NamedColors` リストを設定するためのコードを次に示します。

**C#**
```csharp
using System.Collections.Generic;
using System.Linq;
using System.Reflection;
using Windows.UI;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Media;

namespace ColorsListApp
{
    public sealed partial class MainPage : Page
    {
        // The list of colors won't change after it's populated, so we use List<T>. 
        // If the data can change, we should use an ObservableCollection<T> intead.
        List<NamedColor> NamedColors = new List<NamedColor>();

        public MainPage()
        {
            this.InitializeComponent();

            // Use reflection to get all the properties of the Colors class.
            IEnumerable<PropertyInfo> propertyInfos = typeof(Colors).GetRuntimeProperties();

            // For each property, create a NamedColor with the property name (color name),
            // and property value (color value). Add it the NamedColors list.
            for (int i = 0; i < propertyInfos.Count(); i++)
            {
                NamedColors.Add(new NamedColor(propertyInfos.ElementAt(i).Name,
                                    (Color)propertyInfos.ElementAt(i).GetValue(null)));
            }

            colorsListView.ItemsSource = NamedColors;
        }
    }

    class NamedColor
    {
        public NamedColor(string colorName, Color colorValue)
        {
            Name = colorName;
            Color = colorValue;
        }

        public string Name { get; set; }

        public Color Color { get; set; }

        public SolidColorBrush Brush
        {
            get { return new SolidColorBrush(Color); }
        }
    }
}
```

## <a name="data-template"></a>データ テンプレート

データ テンプレートを指定して、リスト ビューにデータ項目の表示方法を伝えます。 

既定では、データ項目は、バインドされているデータ オブジェクトの文字列表現としてリスト ビューに表示されます。 リスト ビューに 'NamedColors' データの表示方法を伝えることなく、リスト ビューでこのデータを表示する場合、次のように、単に **ToString** メソッドによって返されたものが表示されます。

**XAML**
```xaml
<ListView x:Name="colorsListView"/>
```

![項目の文字列表現を示すリスト ビュー](images/listview-no-template.png)

データ項目の特定のプロパティに [DisplayMemberPath](/uwp/api/windows.ui.xaml.controls.itemscontrol.displaymemberpath) を設定すると、そのプロパティの文字列表現を表示できます。 ここでは、`NamedColor` 項目の `Name` プロパティに DisplayMemberPath を設定します。

**XAML**
```xaml
<ListView x:Name="colorsListView" DisplayMemberPath="Name" />
```

リスト ビューで、次のように名前で項目が表示されるようになりました。 より便利になりましたが、あまり興味を引くものではなく、多くの情報が隠されたままです。

![項目プロパティの文字列表現を示すリスト ビュー](images/listview-display-member-path.png)

通常は、よりリッチな表現でデータを表示する必要があります。 リスト ビューでの項目の表示方法を正確に指定するには、[DataTemplate](/uwp/api/Windows.UI.Xaml.DataTemplate) を作成します。 DataTemplate の XAML では、個々の項目を表示するために使うコントロールのレイアウトと外観を定義します。 レイアウト内のコントロールでは、データ オブジェクトのプロパティにバインドすることも、静的コンテンツをインラインで定義することもできます。 DataTemplate は、リスト コントロールの [ItemTemplate](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate) プロパティに割り当てます。

> [!IMPORTANT]
> **ItemTemplate** と **DisplayMemberPath** を同時に使うことはできません。 両方のプロパティが設定されていると、例外が発生します。

ここでは、色の名前や RGB 値が設定された項目の色で[四角形](/uwp/api/windows.ui.xaml.shapes.rectangle)を表示する DataTemplate を定義します。 

> [!NOTE]
> DataTemplate で [x:Bind マークアップ拡張](../../xaml-platform/x-bind-markup-extension.md)を使う場合、DataTemplate に DataType (`x:DataType`) を指定する必要があります。

**XAML**
```xaml
<ListView x:Name="colorsListView">
    <ListView.ItemTemplate>
        <DataTemplate x:DataType="local:NamedColor">
            <Grid>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition MinWidth="54"/>
                    <ColumnDefinition Width="32"/>
                    <ColumnDefinition Width="32"/>
                    <ColumnDefinition Width="32"/>
                    <ColumnDefinition/>
                </Grid.ColumnDefinitions>
                <Grid.RowDefinitions>
                    <RowDefinition/>
                    <RowDefinition/>
                </Grid.RowDefinitions>
                <Rectangle Width="44" Height="44" Fill="{x:Bind Brush}" Grid.RowSpan="2"/>
                <TextBlock Text="{x:Bind Name}" Grid.Column="1" Grid.ColumnSpan="4"/>
                <TextBlock Text="{x:Bind Color.R}" Grid.Column="1" Grid.Row="1" Foreground="Red"/>
                <TextBlock Text="{x:Bind Color.G}" Grid.Column="2" Grid.Row="1" Foreground="Green"/>
                <TextBlock Text="{x:Bind Color.B}" Grid.Column="3" Grid.Row="1" Foreground="Blue"/>
            </Grid>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

このデータ テンプレートを使ってデータ項目を表示すると、次のようになります。

![データ テンプレートを使ったリスト ビュー項目](images/listview-data-template-0.png)

> [!IMPORTANT]
> 既定では、Listviewitem の内容は左揃えになっています。つまり、[HorizontalContentAlignmentProperty ](/uwp/api/windows.ui.xaml.controls.control.horizontalcontentalignment#Windows_UI_Xaml_Controls_Control_HorizontalContentAlignment) は Left に設定されています。 水平方向に積み上げられた要素や、同じグリッド行に配置された要素など、水平方向に隣接する複数の要素が ListViewItem 内にある場合、それらはすべて左揃えになり、定義された余白で区切られます。 
<br/><br/> 要素が ListItem の本体全体に収まるようにするためには、ListView 内で [Setter](/uwp/api/windows.ui.xaml.setter) を使用して、HorizontalContentAlignmentProperty を [Stretch](/uwp/api/windows.ui.xaml.horizontalalignment) に設定する必要があります。

```xaml
<ListView.ItemContainerStyle>
    <Style TargetType="ListViewItem">
        <Setter Property="HorizontalContentAlignment" Value="Stretch"/>
    </Style>
</ListView.ItemContainerStyle>
```


GridView でデータを表示したい場合があります。 グリッド レイアウトにより適した方法でデータを表示する、別のデータ テンプレートを次に示します。 今回は、GridView 用に XAML 内ではなく、リソースとしてデータ テンプレートを定義します。


**XAML**
```xaml
<Page.Resources>
    <DataTemplate x:Key="namedColorItemGridTemplate" x:DataType="local:NamedColor">
        <Grid>
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="32"/>
                <ColumnDefinition Width="32"/>
                <ColumnDefinition Width="32"/>
            </Grid.ColumnDefinitions>
            <Grid.RowDefinitions>
                <RowDefinition Height="96"/>
                <RowDefinition/>
                <RowDefinition/>
            </Grid.RowDefinitions>
    
            <Rectangle Width="96" Height="96" Fill="{x:Bind Brush}" Grid.ColumnSpan="3" />
            <!-- Name -->
            <Border Background="#AAFFFFFF" Grid.ColumnSpan="3" Height="40" VerticalAlignment="Top">
                <TextBlock Text="{x:Bind Name}" TextWrapping="Wrap" Margin="4,0,0,0"/>
            </Border>
            <!-- RGB -->
            <Border Background="Gainsboro" Grid.Row="1" Grid.ColumnSpan="3"/>
            <TextBlock Text="{x:Bind Color.R}" Foreground="Red"
                   Grid.Column="0" Grid.Row="1" HorizontalAlignment="Center"/>
            <TextBlock Text="{x:Bind Color.G}" Foreground="Green"
                   Grid.Column="1" Grid.Row="1" HorizontalAlignment="Center"/>
            <TextBlock Text="{x:Bind Color.B}" Foreground="Blue" 
                   Grid.Column="2" Grid.Row="1" HorizontalAlignment="Center"/>
            <!-- HEX -->
            <Border Background="Gray" Grid.Row="2" Grid.ColumnSpan="3">
                <TextBlock Text="{x:Bind Color}" Foreground="White" Margin="4,0,0,0"/>
            </Border>
        </Grid>
    </DataTemplate>
</Page.Resources>

...

<GridView x:Name="colorsGridView" 
          ItemTemplate="{StaticResource namedColorItemGridTemplate}"/>
```

このデータ テンプレートを使ってグリッドにデータを表示すると、次のようになります。

![データ テンプレートを使ったグリッド ビュー項目](images/gridview-data-template.png)

### <a name="performance-considerations"></a>パフォーマンスに関する考慮事項

データ テンプレートは、リスト ビューの外観を定義する主要な方法です。 リストに多数の項目を表示した場合、パフォーマンスが大幅に低下することもあります。 

データ テンプレートのすべての XAML 要素のインスタンスが、リスト ビューの各項目用に作成されます。 たとえば、前の例のグリッド テンプレートには、10 個の XAML 要素 (1 つの Grid、1 つの Rectangle、3 つの Border、5 つの Textblock) が含まれています。 このデータ テンプレートを使って 20 個の項目を表示する GridView では、少なくとも要素を 200 個 (20*10=200) 作成します。 データ テンプレートの要素数を減らすと、リスト ビューで作成される要素の総数を大幅に減らすことができます。 詳しくは、「[ListView と GridView の UI の最適化」の項目ごとの要素数の削減](../../debug-test-perf/optimize-gridview-and-listview.md)に関する記事をご覧ください。

 このセクションのグリッド データ テンプレートについて考えてみます。 要素数を減らすための手法をいくつか確認しましょう。

**XAML**
```xaml
<!-- RGB -->
<Border Background="Gainsboro" Grid.Row="1" Grid.ColumnSpan="3"/>
<TextBlock Text="{x:Bind Color.R}" Foreground="Red"
           Grid.Column="0" Grid.Row="1" HorizontalAlignment="Center"/>
<TextBlock Text="{x:Bind Color.G}" Foreground="Green"
           Grid.Column="1" Grid.Row="1" HorizontalAlignment="Center"/>
<TextBlock Text="{x:Bind Color.B}" Foreground="Blue" 
           Grid.Column="2" Grid.Row="1" HorizontalAlignment="Center"/>
```

 - まず、このレイアウトでは 1 つのグリッドを使用します。 1 列の Grid を用意して、StackPanel にこれら 3 つの TextBlock を配置できますが、何度も作成するデータ テンプレートでは、レイアウト パネルを他のレイアウト パネル内に埋め込まないようにする方法を探す必要があります。
 - 次に、Border コントロールを使って、Border 要素内に実際に項目を配置することなく背景をレンダリングできます。 Border 要素には子要素を 1 つしか配置できないため、他のレイアウト パネルを追加して、XAML の Border 要素内で 3 つの TextBlock 要素をホストする必要があります。 TextBlock を Border 要素の子要素にしないようにすることで、パネルで TextBlock を保持する必要がなくなります。
 - 最後に、StackPanel 内に TextBlock を配置して、明示的な Border 要素を使用する代わりに、StackPanel で Border プロパティを設定できます。 しかし、Border 要素は StackPanel よりも軽量なコントロールであるため、何度もレンダリングするときのパフォーマンスへの影響は少なくなります。

### <a name="using-different-layouts-for-different-items"></a>さまざまな項目ごとに異なるレイアウトを使用する


## <a name="control-template"></a>コントロール テンプレート
項目のコントロール テンプレートには、選択、ポインター オーバー、フォーカスなどの状態を表示するビジュアルが含まれています。 これらのビジュアルは、データ テンプレートの上または下のいずれかにレンダリングされます。 ListView コントロール テンプレートによって描画される一般的な既定のビジュアルの一部を次に示します。

- ホバー – 薄い灰色の四角形がデータ テンプレートの下に描画されます。  
- 選択 – 薄い青色の四角形がデータ テンプレートの下に描画されます。 
- キーボード フォーカス - 項目テンプレートの上に描画される[視認性の高いフォーカス表示](../input/guidelines-for-visualfeedback.md#high-visibility-focus-visuals)。

![リスト ビューの状態のビジュアル](images/listview-state-visuals.png)

リスト ビューでは、データ テンプレートとコントロール テンプレートの要素を組み合わせて、画面にレンダリングする最終的なビジュアルを作成します。 ここでは、状態のビジュアルがリスト ビューのコンテキスト内で表示されています。

![異なる状態の項目を含むリスト ビュー](images/listview-states.png)

### <a name="listviewitempresenter"></a>ListViewItemPresenter

データ テンプレートについて上で説明したとおり、各項目で作成する XAML 要素の数は、リスト ビューのパフォーマンスに大きな影響を与えます。 データ テンプレートとコントロール テンプレートを組み合わせて各項目を表示するため、項目を表示するために必要な実際の要素数には、両方のテンプレートの要素が含まれます。

ListView コントロールと GridView コントロールを最適化して、項目ごとに作成する XAML 要素の数を減らします。 **ListViewItem** のビジュアルは [ListViewItemPresenter](/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter) によって作成されます。これは、多数の UIElement によるオーバーヘッドなしで、フォーカス、選択などの表示状態で複雑なビジュアルを表示する特別な XAML 要素です。
 
> [!NOTE]
> Windows 10 の UWP アプリでは、**ListViewItem** と **GridViewItem** の両方で **ListViewItemPresenter** を使います。GridViewItemPresenter は非推奨であるため、使わないでください。 ListViewItem と GridViewItem では、ListViewItemPresenter に異なるプロパティ値を設定して、異なる既定の外観を実現します。

項目コンテナーの外観を変更するには、[ItemContainerStyle](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemcontainerstyle) プロパティを使い、[TargetType](/uwp/api/windows.ui.xaml.style.targettype) に **ListViewItem** または **GridViewItem** を設定した [Style](/uwp/api/windows.ui.xaml.style) を提供します。

この例では、ListViewItem にパディングを追加して、リストの項目の間に余白を作成します。

```xaml
<ListView x:Name="colorsListView">
    <ListView.ItemTemplate>
        <!-- DataTemplate XAML shown in previous ListView example -->
    </ListView.ItemTemplate>

    <ListView.ItemContainerStyle>
        <Style TargetType="ListViewItem">
            <Setter Property="Padding" Value="0,4"/>
        </Style>
    </ListView.ItemContainerStyle>
</ListView>
```

これで、項目の間に余白が入り、次のようにリスト ビューが表示されます。

![パディングを適用したリスト ビューの項目](images/listview-data-template-1.png)

ListViewItem の既定のスタイルの場合、ListViewItemPresenter の **ContentMargin** プロパティには、ListViewItem の **Padding** プロパティへの [TemplateBinding](../../xaml-platform/templatebinding-markup-extension.md) があります (`<ListViewItemPresenter ContentMargin="{TemplateBinding Padding}"/>`)。 Padding プロパティを設定すると、この値は実際に ListViewItemPresenter の ContentMargin プロパティに渡されます。

ListViewItems プロパティにテンプレート バインドされていないその他の ListViewItemPresenter プロパティを変更するには、プロパティを変更できる新しい ListViewItemPresenter を使って ListViewItem を再テンプレート化する必要があります。 

> [!NOTE]
> ListViewItem と GridViewItem の既定のスタイルでは、ListViewItemPresenter に多くのプロパティが設定されています。 常に既定のスタイルのコピーから始めて、必要なプロパティのみ変更することをお勧めします。 そうしなければ、一部のプロパティを正しく設定されていないことが原因で、ビジュアルが期待どおりに表示されない可能性があります。

**Visual Studio で既定のテンプレートのコピーを作成するには**
 
1. [ドキュメント アウトライン] ウィンドウを開きます ( **[表示] > [その他のウィンドウ] > [ドキュメント アウトライン]** )。
2. 変更するリストまたはグリッドの要素を選びます。 この例では、`colorsGridView` 要素を変更します。
3. 右クリックして、 **[追加テンプレートの編集] > [生成されたアイテム コンテナーの編集 (ItemContainerStyle)] > [コピーして編集]** の順に選びます。
    ![Visual Studio のエディター](images/listview-itemcontainerstyle-vs.png)
4. [Style リソースの作成] ダイアログ ボックスで、スタイルの名前を入力します。 この例では、`colorsGridViewItemStyle` を使います。
    ![Visual Studio の [Style リソースの作成] ダイアログ](images/listview-style-resource-vs.png)

次の XAML で示すように、既定のスタイルのコピーをリソースとしてアプリに追加し、**GridView.ItemContainerStyle** プロパティをそのリソースに設定します。 

```xaml
<Style x:Key="colorsGridViewItemStyle" TargetType="GridViewItem">
    <Setter Property="FontFamily" Value="{ThemeResource ContentControlThemeFontFamily}"/>
    <Setter Property="FontSize" Value="{ThemeResource ControlContentThemeFontSize}" />
    <Setter Property="Background" Value="Transparent"/>
    <Setter Property="Foreground" Value="{ThemeResource SystemControlForegroundBaseHighBrush}"/>
    <Setter Property="TabNavigation" Value="Local"/>
    <Setter Property="IsHoldingEnabled" Value="True"/>
    <Setter Property="HorizontalContentAlignment" Value="Center"/>
    <Setter Property="VerticalContentAlignment" Value="Center"/>
    <Setter Property="Margin" Value="0,0,4,4"/>
    <Setter Property="MinWidth" Value="{ThemeResource GridViewItemMinWidth}"/>
    <Setter Property="MinHeight" Value="{ThemeResource GridViewItemMinHeight}"/>
    <Setter Property="Template">
        <Setter.Value>
            <ControlTemplate TargetType="GridViewItem">
                <ListViewItemPresenter 
                    CheckBrush="{ThemeResource SystemControlForegroundBaseMediumHighBrush}" 
                    ContentMargin="{TemplateBinding Padding}" 
                    CheckMode="Overlay" 
                    ContentTransitions="{TemplateBinding ContentTransitions}" 
                    CheckBoxBrush="{ThemeResource SystemControlBackgroundChromeMediumBrush}" 
                    DragForeground="{ThemeResource ListViewItemDragForegroundThemeBrush}" 
                    DragOpacity="{ThemeResource ListViewItemDragThemeOpacity}" 
                    DragBackground="{ThemeResource ListViewItemDragBackgroundThemeBrush}" 
                    DisabledOpacity="{ThemeResource ListViewItemDisabledThemeOpacity}" 
                    FocusBorderBrush="{ThemeResource SystemControlForegroundAltHighBrush}" 
                    FocusSecondaryBorderBrush="{ThemeResource SystemControlForegroundBaseHighBrush}" 
                    HorizontalContentAlignment="{TemplateBinding HorizontalContentAlignment}" 
                    PointerOverForeground="{ThemeResource SystemControlForegroundBaseHighBrush}" 
                    PressedBackground="{ThemeResource SystemControlHighlightListMediumBrush}" 
                    PlaceholderBackground="{ThemeResource ListViewItemPlaceholderBackgroundThemeBrush}" 
                    PointerOverBackground="{ThemeResource SystemControlHighlightListLowBrush}" 
                    ReorderHintOffset="{ThemeResource GridViewItemReorderHintThemeOffset}" 
                    SelectedPressedBackground="{ThemeResource SystemControlHighlightListAccentHighBrush}" 
                    SelectionCheckMarkVisualEnabled="True" 
                    SelectedForeground="{ThemeResource SystemControlForegroundBaseHighBrush}" 
                    SelectedPointerOverBackground="{ThemeResource SystemControlHighlightListAccentMediumBrush}" 
                    SelectedBackground="{ThemeResource SystemControlHighlightAccentBrush}" 
                    VerticalContentAlignment="{TemplateBinding VerticalContentAlignment}"/>
            </ControlTemplate>
        </Setter.Value>
    </Setter>
</Style>

...

<GridView x:Name="colorsGridView" ItemContainerStyle="{StaticResource colorsGridViewItemStyle}"/>
```

これで、ListViewItemPresenter のプロパティを変更して、選択チェック ボックス、項目の配置、ブラシの色の表示状態を制御できるようになりました。 

#### <a name="inline-and-overlay-selection-visuals"></a>インラインとオーバーレイの選択ビジュアル

ListView と GridView では、コントロールや [SelectionMode](/uwp/api/windows.ui.xaml.controls.listviewbase.selectionmode) に応じて、選択されている項目をさまざまな方法で示します。 リスト ビューの選択について詳しくは、[ListView と GridView](listview-and-gridview.md)に関するページをご覧ください。 

**SelectionMode** を **Multiple** に設定すると、選択チェック ボックスは項目のコントロール テンプレートの一部として表示されます。 [SelectionCheckMarkVisualEnabled](/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.selectioncheckmarkvisualenabled) プロパティを使って、Multiple 選択モードの選択チェック ボックスをオフにできます。 しかし、このプロパティは他の選択モードでは無視されるため、Extended または Single 選択モードのチェック ボックスをオンにすることはできません。

[CheckMode](/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.checkmode) プロパティを設定して、インライン スタイルまたはオーバーレイ スタイルのどちらを使ってチェック ボックスを表示するかを指定できます。

- **インライン**:このスタイルは、コンテンツの左側にチェック ボックスを表示し、項目コンテナーの背景を色付けすることで、選択された状態を示します。 これは、ListView の既定のスタイルです。
- **オーバーレイ**:このスタイルは、コンテンツの上部にチェック ボックスを表示し、項目コンテナーの境界線のみを色付けすることで、選択された状態を示します。 これは、GridView の既定のスタイルです。

この表は、選択された状態を示すために使用する既定のビジュアルを示しています。

SelectionMode:&nbsp;&nbsp; | Single、Extended | 複数
---------------|-----------------|---------
インライン | ![インラインの Single または Extended の選択](images/listview-single-selection.png) | ![インラインの Multiple の選択](images/listview-multi-selection.png)
オーバーレイ | ![オーバーレイの Single または Extended の選択](images/gridview-single-selection.png) | ![オーバーレイの Multiple の選択](images/gridview-multi-selection.png)

> [!NOTE]
> この例と次の例では、コントロール テンプレートによって提供されているビジュアルを強調するために、データ テンプレートなしでシンプルな文字列データ項目を表示しています。

また、チェック ボックスの色を変更するためのブラシ プロパティも複数用意されています。 これらについては、以下で他のブラシ プロパティと共に確認します。

#### <a name="brushes"></a>ブラシ 

多くのプロパティでは、異なる表示状態で使用するブラシを指定します。 ブランドの色に合わせて、これらを変更する必要がある場合もあります。 

次の表は、ListViewItem の一般的な表示状態と選択された表示状態、および各状態のビジュアルのレンダリングで使用するブラシを示しています。 画像は、インラインとオーバーレイの両方の選択視覚スタイルにおける、ブラシの効果を示しています。

> [!NOTE]
> この表では、ブラシで変更する色の値はハードコードされた名前付きの色です。テンプレートのどこに適用されるかをはっきりと理解できるように、この色を選択しています。 これらは、表示状態の既定の色ではありません。 アプリの既定の色を変更する場合は、既定のテンプレートで設定されているように、ブラシのリソースを使って色の値を変更する必要があります。

状態またはブラシの名前 | インライン スタイル | オーバーレイ スタイル
------------|--------------|--------------
<b>Normal</b><ul><li><b>CheckBoxBrush="Red"</b></li></ul> | ![インラインの項目の選択 (通常)](images/listview-item-normal.png) | ![オーバーレイの項目の選択 (通常)](images/gridview-item-normal.png)
<b>PointerOver</b><ul><li><b>PointerOverForeground="DarkOrange"</b></li><li><b>PointerOverBackground="MistyRose"</b></li><li>CheckBoxBrush="Red"</li></ul> | ![インラインの項目の選択 (ポインター オーバー)](images/listview-item-pointerover.png) | ![オーバーレイの項目の選択 (ポインター オーバー)](images/gridview-item-pointerover.png)
<b>Pressed</b><ul><li><b>PressedBackground="LightCyan"</b></li><li>PointerOverForeground="DarkOrange"</li><li>CheckBoxBrush="Red"</li></ul> | ![インラインの項目の選択 (押す)](images/listview-item-pressed.png) | ![オーバーレイの項目の選択 (押す)](images/gridview-item-pressed.png)
<b>Selected</b><ul><li><b>SelectedForeground="Navy"</b></li><li><b>SelectedBackground="Khaki"</b></li><li><b>CheckBrush="Green"</b></li><li>CheckBoxBrush="Red" (インラインのみ)</li></ul> | ![インラインの項目の選択 (選択)](images/listview-item-selected.png) | ![オーバーレイの項目の選択 (選択)](images/gridview-item-selected.png)
<b>PointerOverSelected</b><ul><li><b>SelectedPointerOverBackground="Lavender"</b></li><li>SelectedForeground="Navy"</li><li>SelectedBackground="Khaki" (オーバーレイのみ)</li><li>CheckBrush="Green"</li><li>CheckBoxBrush="Red" (インラインのみ)</li></ul> | ![インラインの項目の選択 (ポインター オーバー、選択)](images/listview-item-pointeroverselected.png) | ![オーバーレイの項目の選択 (ポインター オーバー、選択)](images/gridview-item-pointeroverselected.png)
<b>PressedSelected</b><ul><li><b>SelectedPressedBackground="MediumTurquoise"</b></li></li><li>SelectedForeground="Navy"</li><li>SelectedBackground="Khaki" (オーバーレイのみ)</li><li>CheckBrush="Green"</li><li>CheckBoxBrush="Red" (インラインのみ)</li></ul> | ![インラインの項目の選択 (押す、選択)](images/listview-item-pressedselected.png) | ![オーバーレイの項目の選択 (押す、選択)](images/gridview-item-pressedselected.png)
<b>Focused</b><ul><li><b>FocusBorderBrush="Crimson"</b></li><li><b>FocusSecondaryBorderBrush="Gold"</b></li><li>CheckBoxBrush="Red"</li></ul> | ![インラインの項目の選択 (フォーカス)](images/listview-item-focused.png) | ![オーバーレイの項目の選択 (フォーカス)](images/gridview-item-focused.png)

ListViewItemPresenter には、データのプレースホルダーやドラッグ状態用のブラシ プロパティが他にもあります。 リスト ビューで段階的読み込みやドラッグ アンド ドロップを使用する場合は、このような追加のブラシ プロパティを変更する必要があるかについても検討することをお勧めします。 変更できるプロパティの完全な一覧については、ListViewItemPresenter クラスをご覧ください。 

### <a name="expanded-xaml-item-templates"></a>展開時の XAML 項目テンプレート

たとえば、**ListViewItemPresenter** プロパティで許可されていない変更を行う必要がある場合や、チェック ボックスの位置を変更する必要がある場合は、*ListViewItemExpanded* テンプレートまたは *GridViewItemExpanded* テンプレートを使用できます。 これらのテンプレートは、generic.xaml の既定のスタイルに含まれています。 これらは、個別の UIElement からすべてのビジュアルをビルドする標準の XAML パターンに従います。

前述のように、項目テンプレート内の UIElement の数は、リスト ビューのパフォーマンスに大きな影響を与えます。 ListViewItemPresenter を展開時の XAML テンプレートに置き換えると、要素の数が大幅に増大するため、リスト ビューで多数の項目を表示する場合や、パフォーマンスを懸念する場合は推奨されません。

> [!NOTE]
> **ListViewItemPresenter** は、リスト ビューの [ItemsPanel](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemspanel) が [ItemsWrapGrid](/uwp/api/windows.ui.xaml.controls.itemswrapgrid) または [ItemsStackPanel](/uwp/api/windows.ui.xaml.controls.itemsstackpanel) である場合にのみサポートされます。 [VariableSizedWrapGrid](/uwp/api/windows.ui.xaml.controls.variablesizedwrapgrid)、[WrapGrid](/uwp/api/windows.ui.xaml.controls.wrapgrid)、または [StackPanel](/uwp/api/windows.ui.xaml.controls.stackpanel) を使うように ItemsPanel を変更すると、項目テンプレートは展開時の XAML テンプレートに自動的に切り替わります。 詳しくは、「[ListView と GridView の UI の最適化](../../debug-test-perf/optimize-gridview-and-listview.md)」をご覧ください。

展開時の XAML テンプレートをカスタマイズするには、アプリでコピーを作成し、コピーに **ItemContainerStyle** プロパティを設定します。

**展開時のテンプレートをコピーするには**
1. 次に示すように、ListView または GridView に ItemContainerStyle プロパティを設定します。
    ```xaml
    <ListView ItemContainerStyle="{StaticResource ListViewItemExpanded}"/>
    <GridView ItemContainerStyle="{StaticResource GridViewItemExpanded}"/>
    ```
2. Visual Studio の [プロパティ] ウィンドウで、[その他] セクションを展開し、ItemContainerStyle プロパティを見つけます。 (ListView または GridView が選択されていることを確認します)。
3. ItemContainerStyle プロパティのプロパティ マーカーをクリックします。 (TextBox の横にある小さなボックスです。 StaticResource に設定されていることを示すために緑色で表示されています)。プロパティ メニューが開きます。
4. プロパティ メニューで、 **[新しいリソースに変換]** をクリックします。 
    
    ![Visual Studio のプロパティ メニュー](images/listview-convert-resource-vs.png)
5. [Style リソースの作成] ダイアログ ボックスで、リソースの名前を入力し、[OK] をクリックします。

generic.xaml の展開時のテンプレートのコピーがアプリで作成され、必要に応じて変更できるようになります。


## <a name="related-articles"></a>関連記事

- [リスト](lists.md)
- [ListView と GridView](listview-and-gridview.md)