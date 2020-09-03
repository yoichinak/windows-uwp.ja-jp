---
Description: ユーザー入力を使用してコレクション内の項目をフィルター処理します。
title: コレクションのフィルター処理
label: Filtering collections
template: detail.hbs
ms.date: 12/3/2019
ms.topic: article
keywords: windows 10, uwp
pm-contact: anawish
ms.openlocfilehash: b1ffa6374753343321f34d388eb994a62614cb15
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172606"
---
# <a name="filtering-collections-and-lists-through-user-input"></a>ユーザー入力によるコレクションとリストのフィルター処理
コレクションの表示項目数が多い場合や、コレクションがユーザーの対話に強く紐付いている場合、フィルター処理は実装すると便利な機能です。 この記事で説明する方法を使用したフィルター処理は、[ListView](/uwp/api/Windows.UI.Xaml.Controls.ListView)、[GridView](/uwp/api/windows.ui.xaml.controls.gridview)、[ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater?view=winui-2.2) など、ほとんどのコレクション コントロールに実装できます。 コレクションのフィルター処理には、チェック ボックス、ラジオ ボタン、スライダーなど、さまざまなユーザー入力を使用できますが、この記事では、テキスト ベースのユーザー入力を取得し、それを使用して、ユーザーの検索に従って ListView をリアルタイムで更新する方法について説明します。 

> [!NOTE]
> この記事では、ListView を使用したフィルター処理に焦点を当てます。 フィルター処理の手法は、GridView、ItemsRepeater、TreeView など、その他のコレクション コントロールにも応用できることに注意してください。

## <a name="setting-up-the-ui-and-xaml-for-filtering"></a>フィルター処理のための UI と XAML の設定
フィルター処理を実装するには、アプリに ListView を用意し、TextBox や、ユーザー入力を受け付けるその他のコントロールの近くに表示する必要があります。 ユーザーが TextBox に入力するテキストがフィルターとして使用され、テキスト入力/検索クエリを含む結果のみが表示されます。 ユーザーが TextBox に入力すると、ListView は常に、フィルター処理された結果で更新されます。具体的には、TextBox 内のテキストが 1 文字でも変更されるたびに、ListView の全項目がその条件でフィルター処理されます。

次のコードは、単純な ListView、その DataTemplate、付随する TextBox を含む UI を示しています。 この例では、ListView は Person オブジェクトのコレクションを表示します。 Person は (下記のコード サンプルには示されていない) 分離コードで定義されるクラスであり、各 Person オブジェクトには次のプロパティがあります: FirstName、LastName、Company。

ユーザーは TextBox を使用して検索/フィルター条件を入力し、Person オブジェクトのリストを姓でフィルター処理できます。 TextBox は特定の名前 (`FilterByLName`) にバインドされ、独自の TextChanged イベント (`FilteredLV_LNameChanged`) を備えていることに注意してください。 バインドされた名前によって、分離コードで TextBox の内容/テキストにアクセスできます。また、ユーザーが TextBox に入力するたびに TextChanged イベントが発生し、それを利用して、ユーザー入力を受け取った時点でフィルター操作を実行できます。 

フィルター処理が機能するためには、分離コードで操作できる `ObservableCollection<>` などのデータ ソースが ListView に必要です。 この場合、ListView の ItemsSource プロパティが分離コードの `ObservableCollection<Person>` に割り当てられます。 

```xaml
<Grid>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="1*"></ColumnDefinition>
        <ColumnDefinition Width="1*"></ColumnDefinition>
    </Grid.ColumnDefinitions>
    <Grid.RowDefinitions>
            <RowDefinition Height="400"></RowDefinition>
            <RowDefinition Height="400"></RowDefinition>
    </Grid.RowDefinitions>

    <ListView x:Name="FilteredListView"
                Grid.Column="0"
                Margin="0,0,20,0">

        <ListView.ItemTemplate>
            <DataTemplate x:DataType="local:Person">
                <StackPanel>
                    <TextBlock Style="{ThemeResource BaseTextBlockStyle}" Margin="0,5,0,5">
                        <Run Text="{x:Bind FirstName}"></Run>
                        <Run Text="{x:Bind LastName}"></Run>
                    </TextBlock>
                    <TextBlock Style="{ThemeResource BodyTextBlockStyle}" Margin="0,5,0,5" Text="{x:Bind Company}"/>
                </StackPanel>
            </DataTemplate>
        </ListView.ItemTemplate>

    </ListView>

    <TextBox x:Name="FilterByLName" Grid.Column="1" Header="Last Name" Width="200"
             HorizontalAlignment="Left" VerticalAlignment="Top" Margin="0,0,0,20"
             TextChanged="FilteredLV_LNameChanged"/>
</Grid>
```
## <a name="filtering-the-data"></a>データへのフィルター適用
[Linq](/dotnet/csharp/programming-guide/concepts/linq/introduction-to-linq-queries) クエリを使用して、コレクションの特定の項目をグループ化、順序付け、および選択できます。 リストをフィルター処理するために、`FilterByLName` TextBox にユーザーが入力した検索クエリ/フィルター条件に一致する項目のみを選択する Linq クエリを作成します。 クエリ結果は [IEnumerable<T>](/dotnet/api/system.collections.generic.ienumerable-1) コレクション オブジェクトに割り当てることができます。 このコレクションを取得したら、それを使用して元のリストと比較し、一致しない項目を削除し、一致する項目を追加し直します (バックスペースの場合)。

> [!NOTE]
> 項目を増減するとき、最も直感的に ListView をアニメーション表示するためには、フィルター処理されたオブジェクトの新しいコレクションを作成して ListView の ItemsSource プロパティに割り当てるのではなく、ListView の ItemsSource コレクション自体の項目を削除または追加することが重要です。

まず、`List<T>` や `ObservableCollection<T>` などの別のコレクションで元のデータソースを初期化する必要があります。 この例では、`People` という名前の `List<Person>` があり、ListView に示されるすべての Person オブジェクトがここに保持されます (このリストの事前設定/初期化は次のコード スニペットに示されていません)。 フィルター処理されたデータを保持するリストも必要になります。このリストは、フィルターが適用されるたびに常に変更されます。 これは `PeopleFiltered` という名前の `ObservableCollection<Person>` であり、初期化時に `People` と同じ内容になります。
 
次のコードに示すように、以下の手順でフィルター処理を実行します。
 - ListView の ItemsSource プロパティを `PeopledFiltered` に設定します。 
 - `FilterByLName` TextBox の TextChanged イベント `FilteredLV_LNameChanged()` を定義します。 この関数内で、データをフィルター処理します。
 - データをフィルター処理するには、ユーザーが入力した検索クエリ/フィルター条件に `FilterByLName.Text` を介してアクセスします。 Linq クエリを使用して、条件 `FilterByLName.Text` が姓に含まれる `People` の項目を選択し、一致した項目を `TempFiltered` という名前のコレクションに追加します。
 - 現在の `PeopleFiltered` コレクションと、新しくフィルター処理された `TempFiltered` の項目を比較し、必要に応じて `PeopleFiltered` の項目を削除および追加します。
 - 項目が `PeopleFiltered` から削除および追加されると、ListView が更新され、更新内容に応じてアニメーション表示されます。

 ```csharp
using System.Linq;

public MainPage()
{
    // Define People collection to hold all Person objects. 
    // Populate collection - i.e. add Person objects (not shown)
    IList<Person> People = new List<Person>();

    // Create PeopleFiltered collection and copy data from original People collection
    ObservableCollection<Person> PeopleFiltered = new ObservableCollection<Person>(People);

    // Set the ListView's ItemsSource property to the PeopleFiltered collection
    FilteredListView.ItemsSource = PeopleFiltered;

    // ... 
}

private void FilteredLV_LNameChanged(object sender, TextChangedEventArgs e)
{
    /* Perform a Linq query to find all Person objects (from the original People collection)
    that fit the criteria of the filter, save them in a new List called TempFiltered. */
    List<Person> TempFiltered;
    
    /* Make sure all text is case-insensitive when comparing, and make sure 
    the filtered items are in a List object */
    TempFiltered = people.Where(contact => contact.LastName.Contains(FilterByLName.Text, StringComparison.InvariantCultureIgnoreCase)).ToList();
    
    /* Go through TempFiltered and compare it with the current PeopleFiltered collection,
    adding and subtracting items as necessary: */

    // First, remove any Person objects in PeopleFiltered that are not in TempFiltered
    for (int i = PeopleFiltered.Count - 1; i >= 0; i--)
    {
        var item = PeopleFiltered[i];
        if (!TempFiltered.Contains(item))
        {
            PeopleFiltered.Remove(item);
        }
    }

    /* Next, add back any Person objects that are included in TempFiltered and may 
    not currently be in PeopleFiltered (in case of a backspace) */

    foreach (var item in TempFiltered)
    {
        if (!PeopleFiltered.Contains(item))
        {
            PeopleFiltered.Add(item);
        }
    }
}
 ```

以後、ユーザーが `FilterByLName` TextBox にフィルター条件を入力すると、ListView がすぐに更新され、フィルター条件が姓に含まれる人のみが表示されます。

## <a name="next-steps"></a>次の手順

### <a name="get-the-sample-code"></a>サンプル コードを入手する
- XAML コントロール ギャラリー</strong> アプリがインストールされている場合、[こちら](xamlcontrolsgallery:/item/ListView)をクリックしてアプリを開き、ListView ページのリストのフィルター処理の、より具体的で詳細な例を確認してください。
- [XAML コントロール ギャラリー アプリ (Microsoft Store)](https://www.microsoft.com/store/productId/9MSVH128X2ZT) を入手します。

### <a name="related-articles"></a>関連記事
- [リスト](lists.md)
- [リスト ビューとグリッド ビュー](listview-and-gridview.md)
- [コレクションのコマンド実行](collection-commanding.md)