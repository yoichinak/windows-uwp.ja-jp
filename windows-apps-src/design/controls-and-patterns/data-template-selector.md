---
description: データ テンプレート セレクターを使用して、項目のプロパティに基づいて項目のスタイルをカスタマイズします。
title: データ テンプレートの選択
label: Data template selection
template: detail.hbs
ms.date: 10/18/2019
ms.topic: article
keywords: windows 10, uwp
pm-contact: anawish
ms.openlocfilehash: 0d9a35c3e66a4d4189016ca87d3da51da5bf5be4
ms.sourcegitcommit: 4cafc1c55511741dd1e5bfe4496d9950a9b4de1b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/04/2021
ms.locfileid: "97860391"
---
# <a name="data-template-selection-styling-items-based-on-their-properties"></a>データ テンプレートの選択:プロパティに基づいて項目のスタイルを設定する

コレクションのコントロールのカスタマイズしたデザインは、[DataTemplate](/uwp/api/windows.ui.xaml.datatemplate) によって管理されます。 データ テンプレートによって、各項目のレイアウトおよびスタイルが定義され、そのマークアップがコレクション内のすべての項目に適用されます。 この記事では、[DataTemplateSelector](/uwp/api/windows.ui.xaml.controls.datatemplateselector) を使用して、選択した特定の項目のプロパティまたは値に基づきコレクションに異なるデータ テンプレートを適用し、使用するデータ テンプレートを選択する方法について説明します。

> **重要な API**:[DataTemplateSelector](/uwp/api/windows.ui.xaml.controls.datatemplateselector)、[DataTemplate](/uwp/api/windows.ui.xaml.datatemplate)

[DataTemplateSelector](/uwp/api/windows.ui.xaml.controls.datatemplateselector) は、カスタム テンプレート選択ロジックを有効にするクラスです。 コレクション内の特定の項目に対して使用するデータ テンプレートを指定するルールを定義できます。 このロジックを実装するには、コード ビハインドで DataTemplateSelector のサブクラスを作成し、どの項目のカテゴリにどのデータ テンプレートを使用するかを決定するロジックを定義します (たとえば、特定の種類の項目や特定のプロパティ値を持つ項目など)。 このクラスのインスタンスは、使用するデータ テンプレートの定義と共に、XAML ファイルの Resources セクションで宣言します。 これらのリソースは、XAML で参照できるように `x:Key` 値で識別します。

## <a name="prerequisites"></a>前提条件

- [ListView や GridView などのコレクション コントロールを使用し、作成する](listview-and-gridview.md)方法。
- [DataTemplate を使用して項目の外観をカスタマイズする](item-containers-templates.md#data-template)方法。

## <a name="when-not-to-use-a-datatemplateselector"></a>DataTemplateSelector を使用しない場合

一般に、ListView または GridView のすべての項目に、まったく異なるレイアウト/スタイルを付与するべきではありません。これは DataTemplateSelector の良くない使い方であり、パフォーマンスに悪影響を及ぼします。

リスト項目のビジュアル表示の特定の要素は、特定のプロパティをバインドすることで、単一のデータ テンプレートだけを使用して制御できます。 たとえば、各項目は、データ テンプレートのアイコン ソース プロパティにバインドし、そのアイコン ソース プロパティの異なる値を各項目に指定することによって、それぞれ異なるアイコンを持つことができます。 これにより、DataTemplateSelector を使用するよりもパフォーマンスが向上します。

## <a name="when-to-use-a-datatemplateselector"></a>DataTemplateSelector を使用する場合

1 つのコレクション コントロールで複数のデータ テンプレートを使用する場合は、[DataTemplateSelector](/uwp/api/windows.ui.xaml.controls.datatemplateselector) を作成する必要があります。 `DataTemplateSelector` を使用すると、項目を同様のレイアウトで保持したまま、特定の項目を目立たせることができます。 DataTemplateSelector が役に立つユース ケースは多くあり、使用しているコントロールと戦略を再検討する方がよい場合があります。

通常、コレクション コントロールはすべて同じ種類の項目のコレクションにバインドされます。 ただし、項目が同じ種類であっても、特定のプロパティに対して異なる値を持つ場合や、異なる意味を表す場合があります。 また、特定の項目が他の項目よりも重要な場合や、1 つの項目が特に重要であるか、異なる場合があり、視覚的に強調する必要があることがあります。このような状況では、DataTemplateSelector が非常に役に立ちます。

また、さまざまな種類の項目を含むコレクションにバインドすることもできます。バインドされたコレクションには、文字列、整数、カスタム クラス オブジェクトなどを混在させることができます。 これにより、項目のオブジェクトの種類に基づいて異なるデータ テンプレートを割り当てることができるので、DataTemplateSelector が特に便利になります。

データ テンプレート セレクターの使用例をいくつか紹介します。

- ListView 内でさまざまなレベルの従業員を表す場合、従業員の種類/レベルごとに、異なる色の背景で簡単に区別したい場合。
- GridView を使用する製品ギャラリーで販売品目を表す場合は、セールの品目を赤の背景や別の色のフォントを使用して、標準価格の品目より目立たせたい場合。
- FlipView を使用して、フォト ギャラリーで受賞者/上位の写真を表す場合。
- ListView の負の数/正の数、または短い文字列/長い文字列を区別して表す必要がある場合。

## <a name="create-a-datatemplateselector"></a>DataTemplateSelector を作成する

データ テンプレート セレクターを作成する場合、コードでテンプレート選択ロジックを定義し、XAML でデータ テンプレートを定義します。

### <a name="code-behind-component"></a>コード ビハインドのコンポーネント

データ テンプレート セレクターを使用するには、まず、コード ビハインドで [DataTemplateSelector](/uwp/api/windows.ui.xaml.controls.datatemplateselector) のサブクラス (派生クラス) を作成します。 クラスでは、各テンプレートをクラスのプロパティとして宣言します。 次に、[SelectTemplateCore](/uwp/api/windows.ui.xaml.controls.datatemplateselector.selecttemplatecore) メソッドをオーバーライドして、独自のテンプレート選択ロジックを含めます。

`MyDataTemplateSelector` と呼ばれる単純な `DataTemplateSelector` サブクラスの例を次に示します。

```csharp
public class MyDataTemplateSelector : DataTemplateSelector
{
    public DataTemplate Normal { get; set; }
    public DataTemplate Accent { get; set; }

    protected override DataTemplate SelectTemplateCore(object item)
    {
        if ((int)item % 2 == 0)
        {
            return Normal;
        }
        else
        {
            return Accent;
        }
    }
}
```

`MyDataTemplateSelector` クラスは `DataTemplateSelector` クラスから派生し、まず、`Normal` と `Accent` という 2 つの [DataTemplate](/uwp/api/windows.ui.xaml.datatemplate) オブジェクトを定義します。 ここでは、これらは空の宣言ですが、XAML ファイル内にマークアップと共に "入力" されます。

`SelectTemplateCore` メソッドは、項目オブジェクト (つまり、各コレクション項目) を取り込み、どのような状況でどの `DataTemplate` を返すかのルールでオーバーライドします。 この場合、項目が奇数の場合は、`Accent` データ テンプレートを受け取ります。偶数の場合は、`Normal` データ テンプレートを受け取ります。

### <a name="xaml-component"></a>XAML コンポーネント

次に、XAML ファイルのリソース セクションに、この新しい `MyDataTemplateSelector` クラスのインスタンスを作成する必要があります。 すべてのリソースには `x:Key` が必要です。これは後の手順でコレクション コントロールの `ItemTemplateSelector` プロパティにバインドするために使用します。 また、`DataTemplate` オブジェクトのインスタンスを 2 つ作成し、リソースセクションでそのレイアウトを定義します。 これらのデータ テンプレートは、`MyDataTemplateSelector` クラスで宣言した `Accent` および `Normal` プロパティに割り当てます。

必要な XAML リソースとマークアップの例を次に示します。

```xaml
<Page.Resources>

<DataTemplate x:Key="NormalItemTemplate" x:DataType="x:Int32">
    <Button HorizontalAlignment="Stretch" VerticalAlignment="Stretch" Background="{ThemeResource SystemChromeLowColor}">
        <TextBlock Text="{x:Bind}" />
    </Button>
</DataTemplate>

<DataTemplate x:Key="AccentItemTemplate" x:DataType="x:Int32">
    <Button HorizontalAlignment="Stretch" VerticalAlignment="Stretch" Background="{ThemeResource SystemAccentColor}">
        <TextBlock Text="{x:Bind}" />
    </Button>
</DataTemplate>

<l:MyDataTemplateSelector x:Key="MyDataTemplateSelector"
    Normal="{StaticResource NormalItemTemplate}"
    Accent="{StaticResource AccentItemTemplate}"/>

</Page.Resources>
```

前述のように、2 つのデータ テンプレート `Normal` と `Accent` が定義されています。これらのテンプレートは両方とも項目をボタンとして表示しますが、`Accent` データ テンプレートでは背景にアクセント カラー ブラシが使用されており、`Normal` データ テンプレートではグレー カラー ブラシ (`SystemChromeLowColor`) が使用されています。 この 2 つのデータ テンプレートは、C# コード ビハインドで作成された MyDataTemplateSelector クラスの属性である `Normal` および `Accent` DataTemplate オブジェクトに割り当てられます。

最後の手順では、`DataTemplateSelector` をコレクション コントロール (この場合は ListView) の `ItemTemplateSelector` プロパティにバインドします。 これにより、`ItemTemplate` プロパティに値を割り当てる必要がなくなります。 

```xaml
<ListView x:Name = "TestListView"
          ItemsSource = "{x:Bind NumbersList}"
          ItemTemplateSelector = "{StaticResource MyDataTemplateSelector}">
</ListView>
```

コードをコンパイルすると、各コレクション項目は `MyDataTemplateSelector` のオーバーライドされた `SelectTemplateCore` メソッドを通じて実行され、適切な DataTemplate を使用してレンダリングされます。

> [!IMPORTANT]
> [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater?view=winui-2.2&preserve-view=true) で `DataTemplateSelector` を使用する場合は、`DataTemplateSelector` を `ItemTemplate` プロパティにバインドします。 `ItemsRepeater` には `ItemTemplateSelector` のプロパティがありません。

## <a name="datatemplateselector-performance-considerations"></a>DataTemplateSelector のパフォーマンスに関する考慮事項

大きなデータ コレクションで ListView または GridView を使用すると、スクロールとパンのパフォーマンスが問題になる場合があります。 大規模なコレクションのパフォーマンスを十分に維持するには、データ テンプレートのパフォーマンスを向上させるためにいくつかの手順を実行します。 これらの詳細については「[ListView と GridView の UI の最適化](../../debug-test-perf/optimize-gridview-and-listview.md)」を参照してください。

- _項目ごとの要素の削減_ - データ テンプレート内の UI 要素の数を適切な最小値に維持します。
- 異種コレクションでのコンテナー リサイクル
  - _ChoosingItemContainer_ の使用 - このイベントは異なる項目に異なるデータ テンプレートを使用するための効率的な方法です。 最適なパフォーマンスを実現するには、キャッシュを最適化し、特定のデータに応じたデータ テンプレートを選択する必要があります。
  - _項目テンプレート セレクター_ の使用 - 一部のインスタンスでは、パフォーマンスに影響を与えるために項目テンプレート セレクター (`DataTemplateSelector`) を使用しないようにする必要があります。
