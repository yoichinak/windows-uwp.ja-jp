---
description: ItemsSource を階層データ ソースにバインドして展開可能なツリー ビューを作成することができます。または、TreeViewNode オブジェクトを自分で作成して管理することもできます。
title: ツリー構造
label: Tree view
template: detail.hbs
ms.date: 07/24/2019
ms.topic: article
ms.localizationpriority: medium
pm-contact: predavid
design-contact: ksulliv
dev-contact: joyate
doc-status: Published
dev_langs:
- csharp
- vb
ms.custom: RS5, 19H1
ms.openlocfilehash: 0d13572d5fabea14090c4908442355491623cbde
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173876"
---
# <a name="treeview"></a>TreeView

XAML の [TreeView](/uwp/api/windows.ui.xaml.controls.treeview) コントロールを使用すると、階層リストが有効になり、入れ子になった項目を含むノードを展開したり、折りたたんだりすることができるようになります。 フォルダー構造や入れ子になった関係を UI で視覚的に示すために使用できます。

**TreeView** API では、以下の機能がサポートされています。

- N レベルの入れ子
- 1 つまたは複数のノードの選択
- **TreeView** および [TreeViewItem](/uwp/api/windows.ui.xaml.controls.treeviewitem) での **ItemsSource** プロパティへのデータ バインディング
- **TreeView** 項目テンプレートのルートとしての **TreeViewItem**
- **TreeViewItem** 内の任意の型のコンテンツ
- ツリー ビュー間のドラッグ アンド ドロップ

**Windows UI ライブラリを入手する**

|  |  |
| - | - |
| ![WinUI ロゴ](images/winui-logo-64x64.png) | **TreeView** コントロールは、Windows UI ライブラリの NuGet パッケージの一部として組み込まれており、パッケージには、Windows アプリの新しいコントロールと UI 機能が含まれています。 インストール手順などについて詳しくは、「[Windows UI Library (Windows UI ライブラリ)](/uwp/toolkits/winui/)」をご覧ください。 |

> **Windows UI ライブラリ API:** [TreeView クラス](/uwp/api/microsoft.ui.xaml.controls.treeview)、[TreeViewNode クラス](/uwp/api/microsoft.ui.xaml.controls.treeviewnode)、[TreeView.ItemsSource プロパティ](/uwp/api/microsoft.ui.xaml.controls.treeview.itemssource)
>
> **プラットフォーム API:** [TreeView クラス](/uwp/api/windows.ui.xaml.controls.treeview)、[TreeViewNode クラス](/uwp/api/windows.ui.xaml.controls.treeviewnode)、[TreeView.ItemsSource プロパティ](/uwp/api/windows.ui.xaml.controls.treeview.itemssource)

> [!TIP]
> このドキュメントでは、XAML で **muxc** エイリアスを使って、プロジェクトに含めた Windows UI Library API を表します。 [Page](/uwp/api/windows.ui.xaml.controls.page) 要素にこれを追加しました。`xmlns:muxc="using:Microsoft.UI.Xaml.Controls"`
>
>コードビハインドでは、C# でも **muxc** エイリアスを使って、プロジェクトに含めた Windows UI Library API を表します。 この **using** ステートメントは、ファイルの先頭に追加されています。`using muxc = Microsoft.UI.Xaml.Controls;`

## <a name="is-this-the-right-control"></a>これは適切なコントロールですか?

- 項目に入れ子になった一覧項目が含まれているとき、それらの項目とピアやノードとの階層関係を視覚的に示すことが重要になる場合は、**TreeView** を使用します。

- 項目の入れ子になった関係を強調表示することが優先的な処理ではない場合は、**TreeView** を使用しないでください。 ほとんどの演習用シナリオでは、標準的なリスト ビューが適しています。

## <a name="examples"></a>例

<table>
<th align="left">XAML コントロール ギャラリー<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML コントロール ギャラリー</strong> アプリがインストールされている場合、こちらをクリックして<a href="xamlcontrolsgallery:/item/TreeView">アプリを開き、TreeView の動作を確認</a>してください。</p>
    <ul>
    <li><a href="https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt">XAML コントロール ギャラリー アプリを入手する (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">ソース コード (GitHub) を入手する</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="treeview-ui"></a>TreeView UI

ツリー ビューはインデントとアイコンを組み合わせて使用することで、親ノードと子ノードとの間の入れ子になった関係を表します。 折りたたまれているノードでは右向きの山形記号を使用し、展開されているノードでは下向きの山形記号を使用します。

![TreeView での山形記号アイコン](images/treeview-simple.png)

ツリー ビュー項目データ テンプレートにアイコンを含めてノードを表すことができます。 たとえば、ファイル システム階層を表示する場合、親ノードにフォルダー アイコンを、リーフ ノードにファイル アイコンを使用できます。

![TreeView で山形記号のアイコンとフォルダー アイコンの組み合わせ](images/treeview-icons.png)

## <a name="create-a-tree-view"></a>ツリー ビューの作成

[ItemsSource](/uwp/api/windows.ui.xaml.controls.treeview.itemssource) を階層データ ソースにバインドしてツリー ビューを作成することができます。または、**TreeViewNode** オブジェクトを自分で作成して管理することもできます。

ツリー ビューを作成するには、[TreeView](/uwp/api/windows.ui.xaml.controls.treeview) コントロールと [TreeViewNode](/uwp/api/windows.ui.xaml.controls.treeviewnode) オブジェクトの階層を使用します。 ノード階層を作成するには、**TreeView** コントロールの [RootNodes](/uwp/api/windows.ui.xaml.controls.treeview.rootnodes) コレクションに 1 つまたは複数のルート ノードを追加します。 各 **TreeViewNode** では、より多くのノードをその [Children](/uwp/api/windows.ui.xaml.controls.treeviewnode.children) コレクションに追加することができます。 ツリー ビュー ノードは、必要な任意の深さまで入れ子にすることができます。

[ListView](/uwp/api/windows.ui.xaml.controls.listview) の **ItemsSource** の場合と同様に、階層データ ソースを [ItemsSource](/uwp/api/windows.ui.xaml.controls.treeview.itemssource) プロパティにバインドしてツリー ビューのコンテンツを提供することができます。 同様に、[ItemTemplate](/uwp/api/windows.ui.xaml.controls.treeview.itemtemplate) (および省略可能な [ItemTemplateSelector](/uwp/api/windows.ui.xaml.controls.treeview.itemtemplate)) を使用して、項目をレンダリングする [DataTemplate](/uwp/api/windows.ui.xaml.datatemplate) を提供します。

> [!IMPORTANT]
> **ItemsSource** およびその関連 API には、Windows 10 Version 1809 ([SDK 17763 以降](https://developer.microsoft.com/windows/downloads/windows-10-sdk))、または [Windows UI ライブラリ](/uwp/toolkits/winui/)が必要です。
>
> **ItemsSource** は、**TreeView** コントロール内にコンテンツを配置するための、**TreeView.RootNodes** の代替手段です。 **ItemsSource** と **RootNodes** の両方を同時に設定することはできません。 **ItemsSource** を使用すると、ノードは自動的に作成され、**TreeView.RootNodes** プロパティからアクセスできます。

XAML で宣言された単純なツリー ビューの例を以下に示します。 通常は、コードにノードを追加しますが、ノードの階層を作成する方法を視覚化するうえで役立つため、XAML の階層をここに示します。

```xaml
<muxc:TreeView>
    <muxc:TreeView.RootNodes>
        <muxc:TreeViewNode Content="Flavors"
                           IsExpanded="True">
            <muxc:TreeViewNode.Children>
                <muxc:TreeViewNode Content="Vanilla"/>
                <muxc:TreeViewNode Content="Strawberry"/>
                <muxc:TreeViewNode Content="Chocolate"/>
            </muxc:TreeViewNode.Children>
        </muxc:TreeViewNode>
    </muxc:TreeView.RootNodes>
</muxc:TreeView>
```

ほとんどの場合、ツリー ビューにはデータ ソースのデータが表示されるため、通常はルート **TreeView** コントロールを XAML で宣言しますが、**TreeViewNode** オブジェクトをコードに、またはデータ バインディングを使用して追加します。

### <a name="bind-to-a-hierarchical-data-source"></a>階層データ ソースへのバインド

データ バインディングを使用してツリー ビューを作成するには、階層コレクションを **TreeView.ItemsSource** プロパティに設定します。 次に、**ItemTemplate** で子項目コレクションを **TreeViewItem.ItemsSource** プロパティに設定します。

```xaml
<muxc:TreeView ItemsSource="{x:Bind DataSource}">
    <muxc:TreeView.ItemTemplate>
        <DataTemplate x:DataType="local:Item">
            <muxc:TreeViewItem ItemsSource="{x:Bind Children}"
                               Content="{x:Bind Name}"/>
        </DataTemplate>
    </muxc:TreeView.ItemTemplate>
</muxc:TreeView>
```

コード全体を見るには、「[データ バインディングを使用したツリー ビュー](#tree-view-using-data-binding)」を参照してください。

#### <a name="items-and-item-containers"></a>項目と項目のコンテナー

**TreeView.ItemsSource** を使用する場合、コンテナーからノードまたはデータ項目を取得するために (またはその逆に)、これらの API を使用できます。

| **[TreeViewItem](/uwp/api/windows.ui.xaml.controls.treeviewitem)** | |
| - | - |
| [TreeView.ItemFromContainer](/uwp/api/windows.ui.xaml.controls.treeview.itemfromcontainer) | 指定した **TreeViewItem** コンテナーのデータ項目を取得します。 |
| [TreeView.ContainerFromItem](/uwp/api/windows.ui.xaml.controls.treeview.containerfromitem) | 指定したデータ項目の **TreeViewItem** コンテナーを取得します。 |

| **[TreeViewNode](/uwp/api/windows.ui.xaml.controls.treeviewnode)** | |
| - | - |
| [TreeView.NodeFromContainer](/uwp/api/windows.ui.xaml.controls.treeview.nodefromcontainer) | 指定した **TreeViewItem** コンテナーの **TreeViewNode** を取得します。 |
| [TreeView.ContainerFromNode](/uwp/api/windows.ui.xaml.controls.treeview.containerfromnode) | 指定した **TreeViewNode** の **TreeViewItem** コンテナーを取得します。 |

### <a name="manage-tree-view-nodes"></a>ツリー ビュー ノードの管理

このツリー ビューは、XAML で作成されたものと同じですが、ノードはコードで作成されています。

```xaml
<muxc:TreeView x:Name="sampleTreeView"/>
```

```csharp
private void InitializeTreeView()
{
    muxc.TreeViewNode rootNode = new muxc.TreeViewNode() { Content = "Flavors" };
    rootNode.IsExpanded = true;
    rootNode.Children.Add(new muxc.TreeViewNode() { Content = "Vanilla" });
    rootNode.Children.Add(new muxc.TreeViewNode() { Content = "Strawberry" });
    rootNode.Children.Add(new muxc.TreeViewNode() { Content = "Chocolate" });

    sampleTreeView.RootNodes.Add(rootNode);
}
```

```vb
Private Sub InitializeTreeView()
    Dim rootNode As New muxc.TreeViewNode With {.Content = "Flavors", .IsExpanded = True}
    With rootNode.Children
        .Add(New muxc.TreeViewNode With {.Content = "Vanilla"})
        .Add(New muxc.TreeViewNode With {.Content = "Strawberry"})
        .Add(New muxc.TreeViewNode With {.Content = "Chocolate"})
    End With
    sampleTreeView.RootNodes.Add(rootNode)
End Sub
```

これらの API は、ツリー ビューのデータの階層の管理に使用できます。

| **[TreeView](/uwp/api/windows.ui.xaml.controls.treeview)** | |
| - | - |
| [RootNodes](/uwp/api/windows.ui.xaml.controls.treeview.rootnodes) | ツリー ビューは、1 つまたは複数のルート ノードの場合があります。 **TreeViewNode** オブジェクトを **RootNodes** コレクションに追加し、ルート ノードを作成します。 ルート ノードの **Parent** は常に **null** です。 ルート ノードの**奥行き**は0です。 |

| **[TreeViewNode](/uwp/api/windows.ui.xaml.controls.treeviewnode)** | |
| - | - |
| [Children](/uwp/api/windows.ui.xaml.controls.treeviewnode.children) | **TreeViewNode** オブジェクトを親ノードの **Children** コレクションに追加し、ノード階層を作成します。 ノードは、その **Children** コレクションのすべてのノードの **Parent** です。 |
| [HasChildren](/uwp/api/windows.ui.xaml.controls.treeviewnode.haschildren) | ノードが子を実体化した場合は **true**。 **false** は空のフォルダーまたは項目を示します。 |
| [HasUnrealizedChildren](/uwp/api/windows.ui.xaml.controls.treeviewnode.hasunrealizedchildren) | ノードが展開されているときにノードに入力している場合は、このプロパティを使用します。 この記事の後半にある「[展開時にノードを入力する](#fill-a-node-when-its-expanding)」をご覧ください。 |
| [Depth](/uwp/api/windows.ui.xaml.controls.treeviewnode.depth) | 子ノードとルート ノードの距離を示します。 |
| [Parent](/uwp/api/windows.ui.xaml.controls.treeviewnode.parent) | このノードが含まれている **Children** コレクションを所有する **TreeViewNode** を取得します。 |

ツリー ビューは、**HasChildren** プロパティと **HasUnrealizedChildren** プロパティを使用して、開く/閉じるアイコンを表示するかどうかを確認します。 いずれかのプロパティが **true** である場合、アイコンが表示されます。それ以外の場合は表示されません。

## <a name="tree-view-node-content"></a>ツリー ビュー ノード コンテンツ

ツリー ビュー ノードが [Content](/uwp/api/windows.ui.xaml.controls.treeviewnode.content) プロパティで表すデータ項目を保存することができます。

前の例では、コンテンツは単純な文字列値でした。 ここでは、ツリー ビュー ノードはユーザーの **Pictures** フォルダーを表すため、ピクチャ ライブラリ [StorageFolder](/uwp/api/windows.storage.storagefolder) がノードの **Content** プロパティに割り当てられます。

```csharp
StorageFolder picturesFolder = KnownFolders.PicturesLibrary;
muxc.TreeViewNode pictureNode = new muxc.TreeViewNode();
pictureNode.Content = picturesFolder;
```

```vb
Dim picturesFolder As StorageFolder = KnownFolders.PicturesLibrary
Dim pictureNode As New muxc.TreeViewNode With {.Content = picturesFolder}
```

> [!NOTE]
> **Pictures** フォルダーにアクセスするには、アプリ マニフェストで**ピクチャ ライブラリ**機能を指定する必要があります。 詳しくは、「[アプリ機能の宣言](../../packaging/app-capability-declarations.md)」をご覧ください。

[DataTemplate](/uwp/api/windows.ui.xaml.datatemplate) を指定して、ツリー ビューでのデータ項目の表示方法を指定することができます。

> [!NOTE]
> Windows 10 バージョン 1803 では、コンテンツが文字列ではない場合は、**TreeView** コントロールを再テンプレート化して、カスタム **ItemTemplate** を指定する必要があります。 今後のバージョンでは、**ItemTemplate** プロパティを設定してください。 詳細については、[TreeView.ItemTemplate](/uwp/api/windows.ui.xaml.controls.treeview.itemtemplate) を参照してください。

### <a name="item-container-style"></a>項目コンテナーのスタイル

**ItemsSource** を使用するか、**RootNodes** を使用するかにかかわらず、各ノードの表示に使用される実際の要素 ("コンテナー" と呼ばれる) は、[TreeViewItem](/uwp/api/windows.ui.xaml.controls.treeviewitem) オブジェクトです。 **TreeViewItem** のプロパティを変更して、**TreeView** の [ItemContainerStyle](/uwp/api/windows.ui.xaml.controls.treeview.itemcontainerstyle) または [ItemContainerStyleSelector](/uwp/api/windows.ui.xaml.controls.treeview.itemcontainerstyleselector) プロパティを使用するコンテナーのスタイルを設定できます。

この例では、展開/折りたたみグリフをオレンジ色の +/- 記号に変更する方法を示します。 既定の **TreeViewItem** テンプレートでは、これらのグリフは `Segoe MDL2 Assets` フォントを使用するように設定されています。 XAML で使用される形式で Unicode 文字の値を指定することで、**Setter.Value** プロパティを設定できます (例: `Value="&#xE948;"`) 。

```xaml
<muxc:TreeView>
    <muxc:TreeView.ItemContainerStyle>
        <Style TargetType="muxc:TreeViewItem">
            <Setter Property="CollapsedGlyph" Value="&#xE948;"/>
            <Setter Property="ExpandedGlyph" Value="&#xE949;"/>
            <Setter Property="GlyphBrush" Value="DarkOrange"/>
        </Style>
    </muxc:TreeView.ItemContainerStyle>
    <muxc:TreeView.RootNodes>
        <muxc:TreeViewNode Content="Flavors"
               IsExpanded="True">
            <muxc:TreeViewNode.Children>
                <muxc:TreeViewNode Content="Vanilla"/>
                <muxc:TreeViewNode Content="Strawberry"/>
                <muxc:TreeViewNode Content="Chocolate"/>
            </muxc:TreeViewNode.Children>
        </muxc:TreeViewNode>
    </muxc:TreeView.RootNodes>
</muxc:TreeView>
```

### <a name="item-template-selectors"></a>項目テンプレート セレクター

既定では、[TreeView](/uwp/api/windows.ui.xaml.controls.treeview) には、各ノードのデータ項目の文字列表現が表示されます。 [ItemTemplate](/uwp/api/windows.ui.xaml.controls.treeview.itemtemplate) プロパティを設定して、すべてのノードで何が表示されるかを変更できます。 または、[ItemTemplateSelector](/uwp/api/windows.ui.xaml.controls.treeview.itemtemplateselector) を使用して、項目の種類または自分で指定したその他の基準に基づいて、ツリー ビュー項目用の別の [DataTemplate](/uwp/api/windows.ui.xaml.datatemplate) を選択できます。

たとえば、ファイル エクスプローラー アプリでは、フォルダー用とファイル用に別々のデータ テンプレートを使用できます。

![フォルダーとファイルに別々のデータ テンプレートを使用](images/treeview-icons.png)

項目テンプレート セレクターを作成して使用する方法の例を次に示します。  詳細については、[DataTemplateSelector](/uwp/api/windows.ui.xaml.controls.datatemplateselector) クラスを参照してください。

> [!NOTE]
> このコードは大規模な例の一部であり、単独では機能しません。 `ExplorerItem` を定義するコードが含まれる完全な例を確認するには、GitHub の [Xaml-Controls-Gallery リポジトリ](https://github.com/microsoft/Xaml-Controls-Gallery)をご覧ください。 [TreeViewPage.xaml](https://github.com/microsoft/Xaml-Controls-Gallery/blob/1ecd85c908a8a1cb9a8201e548f58db379801e69/XamlControlsGallery/ControlPages/TreeViewPage.xaml) と [TreeViewPage.xaml.cs](https://github.com/Microsoft/Xaml-Controls-Gallery/blob/1ecd85c908a8a1cb9a8201e548f58db379801e69/XamlControlsGallery/ControlPages/TreeViewPage.xaml.cs) には、関連するコードが含まれます。

```xaml
<Page.Resources>
    <DataTemplate x:Key="FolderTemplate" x:DataType="local:ExplorerItem">
        <muxc:TreeViewItem ItemsSource="{x:Bind Children}">
            <StackPanel Orientation="Horizontal">
                <Image Width="20" Source="Assets/folder.png"/>
                <TextBlock Text="{x:Bind Name}" />
            </StackPanel>
        </muxc:TreeViewItem>
    </DataTemplate>

    <DataTemplate x:Key="FileTemplate" x:DataType="local:ExplorerItem">
        <muxc:TreeViewItem>
            <StackPanel Orientation="Horizontal">
                <Image Width="20" Source="Assets/file.png"/>
                <TextBlock Text="{x:Bind Name}"/>
            </StackPanel>
        </muxc:TreeViewItem>
    </DataTemplate>

    <local:ExplorerItemTemplateSelector
            x:Key="ExplorerItemTemplateSelector"
            FolderTemplate="{StaticResource FolderTemplate}"
            FileTemplate="{StaticResource FileTemplate}" />
</Page.Resources>

<Grid>
    <muxc:TreeView
        ItemsSource="{x:Bind DataSource}"
        ItemTemplateSelector="{StaticResource ExplorerItemTemplateSelector}"/>
</Grid>
```

```csharp
public class ExplorerItemTemplateSelector : DataTemplateSelector
{
    public DataTemplate FolderTemplate { get; set; }
    public DataTemplate FileTemplate { get; set; }

    protected override DataTemplate SelectTemplateCore(object item)
    {
        var explorerItem = (ExplorerItem)item;
        if (explorerItem.Type == ExplorerItem.ExplorerItemType.Folder) return FolderTemplate;

        return FileTemplate;
    }
}
```

[SelectTemplateCore](/uwp/api/windows.ui.xaml.controls.datatemplateselector.selecttemplatecore) メソッドに渡されるオブジェクトの型は、**ItemsSource** プロパティを設定することでツリー ビューを作成しているか、**TreeViewNode** オブジェクトそのものを作成して管理しているかによって異なります。

- **ItemsSource** が設定されている場合、オブジェクトの型はデータ項目の型になります。 前の例では、オブジェクトは `ExplorerItem` だったため、`ExplorerItem`: `var explorerItem = (ExplorerItem)item;` への単純なキャストの後で、それを使用できます。
- **ItemsSource** の設定がなく、ツリー ビュー ノードを自分で管理している場合、**SelectTemplateCore** に渡されるオブジェクトは **TreeViewNode** になります。 この場合は、[TreeViewNode.Content](/uwp/api/windows.ui.xaml.controls.treeviewnode.content) プロパティからデータ項目を取得できます。

次に示すのは、後述する[画像やミュージック ライブラリのツリー ビュー](#pictures-and-music-library-tree-view)の例に含まれるデータ テンプレート セレクターです。 **SelectTemplateCore** メソッドで **TreeViewNode** が受け取られますが、その内容は [StorageFolder](/uwp/api/windows.storage.storagefolder) または [StorageFile](/uwp/api/windows.storage.storagefile) のいずれかです。 この内容に基づいて、既定のテンプレートを返すか、ミュージック フォルダー、画像フォルダー、ミュージック ファイル、または画像ファイル用の特定のテンプレートを返すことができます。

```csharp
protected override DataTemplate SelectTemplateCore(object item)
{
    var node = (TreeViewNode)item;
    if (node.Content is StorageFolder)
    {
        var content = node.Content as StorageFolder;
        if (content.DisplayName.StartsWith("Pictures")) return PictureFolderTemplate;
        if (content.DisplayName.StartsWith("Music")) return MusicFolderTemplate;
    }
    else if (node.Content is StorageFile)
    {
        var content = node.Content as StorageFile;
        if (content.ContentType.StartsWith("image")) return PictureItemTemplate;
        if (content.ContentType.StartsWith("audio")) return MusicItemTemplate;
    }
    return DefaultTemplate;
}
```

```vb
Protected Overrides Function SelectTemplateCore(ByVal item As Object) As DataTemplate
    Dim node = CType(item, muxc.TreeViewNode)

    If TypeOf node.Content Is StorageFolder Then
        Dim content = TryCast(node.Content, StorageFolder)
        If content.DisplayName.StartsWith("Pictures") Then Return PictureFolderTemplate
        If content.DisplayName.StartsWith("Music") Then Return MusicFolderTemplate
    ElseIf TypeOf node.Content Is StorageFile Then
        Dim content = TryCast(node.Content, StorageFile)
        If content.ContentType.StartsWith("image") Then Return PictureItemTemplate
        If content.ContentType.StartsWith("audio") Then Return MusicItemTemplate
    End If

    Return DefaultTemplate
End Function
```

## <a name="interacting-with-a-tree-view"></a>ツリー ビューの操作

ユーザーが操作できるようにツリー ビューを構成する方法はいくつかあります。

- ノードを展開するか折りたたむ
- 項目の単一選択または複数選択
- クリックして項目を呼び出す

### <a name="expandcollapse"></a>展開/折りたたみ

子を持つ任意のツリー ビュー ノードは常に、展開/折りたたみグリフをクリックして展開または折りたたむことができます。 ノードをプログラムにより展開するか折りたたみ、ノードの状態が変化したときに対応することもできます。

#### <a name="expandcollapse-a-node-programmatically"></a>ノードをプログラムにより展開/折りたたみ

コードでツリー ビュー ノードを展開したり、折りたたんだりする 2 つの方法があります。

- [TreeView](/uwp/api/windows.ui.xaml.controls.treeview) クラスには [Collapse](/uwp/api/windows.ui.xaml.controls.treeview.collapse) メソッドと [Expand](/uwp/api/windows.ui.xaml.controls.treeview.expand) メソッドが含まれています。 これらのメソッドを呼び出すときは、展開するか折りたたむ **TreeViewNode** を渡します。

- 各 [TreeViewNode](/uwp/api/windows.ui.xaml.controls.treeviewnode) には [IsExpanded](/uwp/api/windows.ui.xaml.controls.treeviewnode.isexpanded) プロパティが含まれています。 このプロパティを使用して、ノードの状態を確認したり、状態を変更するように設定したりできます。 このプロパティを XAML で設定し、ノードの初期状態に設定することもできます。

### <a name="fill-a-node-when-its-expanding"></a>展開時にノードを入力する

ツリー ビューで多数のノードを表示しなければならない場合があります。または含まれるノードの数が前もってわからない場合があります。 **TreeView** コントロールは仮想化されていないため、リソースを管理するには、展開時に各ノードを入力し、折りたたみ時に子ノードを削除します。

[展開](/uwp/api/windows.ui.xaml.controls.treeview.expand) イベントを処理し、[HasUnrealizedChildren](/uwp/api/windows.ui.xaml.controls.treeviewnode.hasunrealizedchildren) プロパティを使用して展開時に子をノードに追加します。 **HasUnrealizedChildren** プロパティでは、ノードを入力する必要があるかどうか、またはその **Children** コレクションが既に設定されているかどうかが示されます。 この値は **TreeViewNode** では設定されないことに留意することが重要です。この値はアプリのコードで管理する必要があります。

次の例は、使用中のこれらの API を示しています。 **FillTreeNode** の実装などのコンテキストについては、この記事の最後にある完全なコード例をご覧ください。

```csharp
private void SampleTreeView_Expanding(muxc.TreeView sender, muxc.TreeViewExpandingEventArgs args)
{
    if (args.Node.HasUnrealizedChildren)
    {
        FillTreeNode(args.Node);
    }
}
```

```vb
Private Sub SampleTreeView_Expanding(sender As muxc.TreeView, args As muxc.TreeViewExpandingEventArgs)
    If args.Node.HasUnrealizedChildren Then
        FillTreeNode(args.Node)
    End If
End Sub
```

これは必須ではありませんが、[Collapsed](/uwp/api/windows.ui.xaml.controls.treeview.collapsed) イベントを処理して、親ノードが閉じられたときに子ノードを削除することもできます。 これは、ツリー ビューに多くのノードがある場合、またはノード データが大量のリソースを使用している場合に重要です。 ノードを開くたびに入力することと、子をクローズド ノードにしたままにすることのパフォーマンスへの影響を考慮する必要があります。 最適な選択肢は、アプリによって異なります。

この例では、**Collapsed** イベントに対するハンドラーを示します。

```csharp
private void SampleTreeView_Collapsed(muxc.TreeView sender, muxc.TreeViewCollapsedEventArgs args)
{
    args.Node.Children.Clear();
    args.Node.HasUnrealizedChildren = true;
}
```

```vb
Private Sub SampleTreeView_Collapsed(sender As muxc.TreeView, args As muxc.TreeViewCollapsedEventArgs)
    args.Node.Children.Clear()
    args.Node.HasUnrealizedChildren = True
End Sub
```

### <a name="invoking-an-item"></a>項目の呼び出し

ユーザーは、項目を選択する代わりに操作 (ボタンのように項目を扱う) を呼び出すことができます。 [ItemInvoked](/uwp/api/windows.ui.xaml.controls.treeview.iteminvoked) イベントを処理してこのユーザー操作に対応します。

> [!NOTE]
> [IsItemClickEnabled](/uwp/api/windows.ui.xaml.controls.listviewbase.isitemclickenabled) プロパティが含まれる **ListView** とは異なり、項目の呼び出しはツリー ビューでは常に有効になっています。 イベントを処理するかどうか選択することができます。

**[TreeViewItemInvokedEventArgs](/uwp/api/windows.ui.xaml.controls.treeviewiteminvokedeventargs) クラス**

**ItemInvoked** イベント引数により、呼び出された項目にアクセスできます。 [InvokedItem](/uwp/api/windows.ui.xaml.controls.treeviewiteminvokedeventargs.invokeditem) プロパティには呼び出されたノードが含まれています。 それを **TreeViewNode** にキャストし、データ項目を **TreeViewNode.Content** プロパティから取得できます。

**ItemInvoked** イベント ハンドラーの例を次に示します。 データ項目は [IStorageItem](/uwp/api/windows.storage.istorageitem) です。この例では、ファイルおよびツリーについての情報だけが表示されます。 また、ノードがフォルダー ノードの場合は、ノードを同時に展開または折りたたみます。 それ以外の場合、ノードは山形マークをクリックした場合にのみ展開または折りたたまれます。

```csharp
private void SampleTreeView_ItemInvoked(muxc.TreeView sender, muxc.TreeViewItemInvokedEventArgs args)
{
    var node = args.InvokedItem as muxc.TreeViewNode;
    if (node.Content is IStorageItem item)
    {
        FileNameTextBlock.Text = item.Name;
        FilePathTextBlock.Text = item.Path;
        TreeDepthTextBlock.Text = node.Depth.ToString();

        if (node.Content is StorageFolder)
        {
            node.IsExpanded = !node.IsExpanded;
        }
    }
}
```

```vb
Private Sub SampleTreeView_ItemInvoked(sender As muxc.TreeView, args As muxc.TreeViewItemInvokedEventArgs)
    Dim node = TryCast(args.InvokedItem, muxc.TreeViewNode)
    Dim item = TryCast(node.Content, IStorageItem)
    If item IsNot Nothing Then
        FileNameTextBlock.Text = item.Name
        FilePathTextBlock.Text = item.Path
        TreeDepthTextBlock.Text = node.Depth.ToString()
        If TypeOf node.Content Is StorageFolder Then
            node.IsExpanded = Not node.IsExpanded
        End If
    End If
End Sub
```

### <a name="item-selection"></a>項目の選択

**TreeView** コントロールでは、単一選択と複数選択の両方がサポートされています。 既定では、ノードの選択はオフになっていますが、[TreeView.SelectionMode](/uwp/api/windows.ui.xaml.controls.treeview.selectionmode) プロパティを設定してノードの選択を許可することができます。 [TreeViewSelectionMode](/uwp/api/windows.ui.xaml.controls.treeviewselectionmode) 値は **None**、**Single**、および **Multiple** です。

#### <a name="multiple-selection"></a>複数選択

複数選択を有効にすると、各ツリー ビュー ノードの横にチェック ボックスが表示され、選択した項目が強調表示されます。 ユーザーは、チェック ボックスを使用して項目を選択または選択解除できます。項目は引き続きクリックして呼び出すことができます。

親ノードを選択または選択解除すると、そのノードの下にあるすべての子ノードが選択または選択解除されます。 親ノードの下にある子の全部ではなく一部を選択した場合、親ノードのチェック ボックスは不確定状態として表示されます。

![ツリー ビューでの複数選択](images/treeview-selection.png)

選択したノードは、ツリー ビューの [SelectedNodes](/uwp/api/windows.ui.xaml.controls.treeview.selectednodes) コレクションに追加されます。 [SelectAll](/uwp/api/windows.ui.xaml.controls.treeview.selectall) メソッドを呼び出して、ツリー ビューですべてのノードを選択できます。

> [!NOTE]
> **SelectAll** を呼び出した場合、**SelectionMode** に関係なく、実体化されたすべてのノードが選択されます。 一貫したユーザー エクスペリエンスを提供するため、**SelectionMode** が **Multiple** である場合にのみ、**SelectAll** を呼び出す必要があります。

#### <a name="selection-and-realizedunrealized-nodes"></a>選択と実体化された/実体化されていないノード

ツリー ビューに実体化されていないノードが含まれる場合、それらは選択肢として考慮されません。 実体化されていないノードを選択する際に留意すべき点がいくつかあります。

- ユーザーが親ノードを選択すると、その親の下で実体化されたすべての子も選択されます。 同様に、すべての子ノードが選択されている場合、親ノードも選択されます。
- **SelectAll** メソッドでは、実体化されたノードのみが **SelectedNodes** コレクションに追加されます。
- 実体化されていない子を含む親ノードが選択された場合、子は実体化されたときに選択されます。

#### <a name="selecteditemselecteditems"></a>SelectedItem/SelectedItems

WinUI 2.2 以降では、TreeView に [SelectedItem](/uwp/api/microsoft.ui.xaml.controls.treeview.selecteditem) および [SelectedItems](/uwp/api/microsoft.ui.xaml.controls.treeview.selecteditems) プロパティが用意されています。 これらのプロパティを使用すると、選択したノードのコンテンツを直接取得できます。 複数選択が有効になっている場合、SelectedItem には、SelectedItems コレクションの最初の項目が格納されます。

## <a name="code-examples"></a>コード例

次のコード例では、ツリー ビュー コントロールのさまざまな機能を示します。

### <a name="tree-view-using-xaml"></a>XAML を使用したツリー ビュー

次の例は、XAML で 1 つのツリー ビュー構造を作成する方法を示しています。 ツリー ビューは、カテゴリに配置されており、ユーザーが選択できるアイスクリームのフレーバーとトッピングを示しています。 複数選択が有効になっており、ユーザーがボタンをクリックすると、選択された項目がメイン アプリ UI に表示されます。

```xaml
<Page
    x:Class="TreeViewTest.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    xmlns:muxc="using:Microsoft.UI.Xaml.Controls"
    mc:Ignorable="d"
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}"
          Padding="100">
        <SplitView IsPaneOpen="True"
               DisplayMode="Inline"
               OpenPaneLength="296">
            <SplitView.Pane>
                <muxc:TreeView x:Name="DessertTree" SelectionMode="Multiple">
                    <muxc:TreeView.RootNodes>
                        <muxc:TreeViewNode Content="Flavors" IsExpanded="True">
                            <muxc:TreeViewNode.Children>
                                <muxc:TreeViewNode Content="Vanilla"/>
                                <muxc:TreeViewNode Content="Strawberry"/>
                                <muxc:TreeViewNode Content="Chocolate"/>
                            </muxc:TreeViewNode.Children>
                        </muxc:TreeViewNode>

                        <muxc:TreeViewNode Content="Toppings">
                            <muxc:TreeViewNode.Children>
                                <muxc:TreeViewNode Content="Candy">
                                    <muxc:TreeViewNode.Children>
                                        <muxc:TreeViewNode Content="Chocolate"/>
                                        <muxc:TreeViewNode Content="Mint"/>
                                        <muxc:TreeViewNode Content="Sprinkles"/>
                                    </muxc:TreeViewNode.Children>
                                </muxc:TreeViewNode>
                                <muxc:TreeViewNode Content="Fruits">
                                    <muxc:TreeViewNode.Children>
                                        <muxc:TreeViewNode Content="Mango"/>
                                        <muxc:TreeViewNode Content="Peach"/>
                                        <muxc:TreeViewNode Content="Kiwi"/>
                                    </muxc:TreeViewNode.Children>
                                </muxc:TreeViewNode>
                                <muxc:TreeViewNode Content="Berries">
                                    <muxc:TreeViewNode.Children>
                                        <muxc:TreeViewNode Content="Strawberry"/>
                                        <muxc:TreeViewNode Content="Blueberry"/>
                                        <muxc:TreeViewNode Content="Blackberry"/>
                                    </muxc:TreeViewNode.Children>
                                </muxc:TreeViewNode>
                            </muxc:TreeViewNode.Children>
                        </muxc:TreeViewNode>
                    </muxc:TreeView.RootNodes>
                </muxc:TreeView>
            </SplitView.Pane>

            <StackPanel Grid.Column="1" Margin="12,0">
                <Button Content="Select all" Click="SelectAllButton_Click"/>
                <Button Content="Create order" Click="OrderButton_Click" Margin="0,12"/>
                <TextBlock Text="Your flavor selections:" Style="{StaticResource CaptionTextBlockStyle}"/>
                <TextBlock x:Name="FlavorList" Margin="0,0,0,12"/>
                <TextBlock Text="Your topping selections:" Style="{StaticResource CaptionTextBlockStyle}"/>
                <TextBlock x:Name="ToppingList"/>
            </StackPanel>
        </SplitView>
    </Grid>
</Page>
```

```csharp
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using muxc = Microsoft.UI.Xaml.Controls;

namespace TreeViewTest
{
    public sealed partial class MainPage : Page
    {
        public MainPage()
        {
            this.InitializeComponent();
        }

        private void OrderButton_Click(object sender, RoutedEventArgs e)
        {
            FlavorList.Text = string.Empty;
            ToppingList.Text = string.Empty;

            foreach (muxc.TreeViewNode node in DessertTree.SelectedNodes)
            {
                if (node.Parent.Content?.ToString() == "Flavors")
                {
                    FlavorList.Text += node.Content + "; ";
                }
                else if (node.HasChildren == false)
                {
                    ToppingList.Text += node.Content + "; ";
                }
            }
        }

        private void SelectAllButton_Click(object sender, RoutedEventArgs e)
        {
            if (DessertTree.SelectionMode == muxc.TreeViewSelectionMode.Multiple)
            {
                DessertTree.SelectAll();
            }
        }
    }
}
```

```vb
Private Sub OrderButton_Click(sender As Object, e As RoutedEventArgs)
    FlavorList.Text = String.Empty
    ToppingList.Text = String.Empty
    For Each node As muxc.TreeViewNode In DessertTree.SelectedNodes
        If node.Parent.Content?.ToString() = "Flavors" Then
            FlavorList.Text += node.Content & "; "
        ElseIf node.HasChildren = False Then
            ToppingList.Text += node.Content & "; "
        End If
    Next
End Sub

Private Sub SelectAllButton_Click(sender As Object, e As RoutedEventArgs)
    If DessertTree.SelectionMode = muxc.TreeViewSelectionMode.Multiple Then
        DessertTree.SelectAll()
    End If
End Sub
```

### <a name="tree-view-using-data-binding"></a>データ バインディングを使用したツリー ビュー

この例では、前の例と同じツリー ビューを作成する方法を示します。 ただし、XAML でデータ階層を作成するのではなく、データをコードで作成し、ツリー ビューの **ItemsSource** プロパティにバインドします (前の例に示したボタン イベント ハンドラーをこの例にも適用します)。

```xaml
<Page
    x:Class="TreeViewTest.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    xmlns:muxc="using:Microsoft.UI.Xaml.Controls"
    xmlns:local="using:TreeViewTest"
    mc:Ignorable="d"
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}"
          Padding="100">
        <SplitView IsPaneOpen="True"
                   DisplayMode="Inline"
                   OpenPaneLength="296">
            <SplitView.Pane>
                <muxc:TreeView Name="DessertTree"
                                      SelectionMode="Multiple"
                                      ItemsSource="{x:Bind DataSource}">
                    <muxc:TreeView.ItemTemplate>
                        <DataTemplate x:DataType="local:Item">
                            <muxc:TreeViewItem
                                ItemsSource="{x:Bind Children}"
                                Content="{x:Bind Name}"/>
                        </DataTemplate>
                    </muxc:TreeView.ItemTemplate>
                </muxc:TreeView>
            </SplitView.Pane>

            <StackPanel Grid.Column="1" Margin="12,0">
                <Button Content="Select all"
                        Click="SelectAllButton_Click"/>
                <Button Content="Create order"
                        Click="OrderButton_Click"
                        Margin="0,12"/>
                <TextBlock Text="Your flavor selections:"
                           Style="{StaticResource CaptionTextBlockStyle}"/>
                <TextBlock x:Name="FlavorList" Margin="0,0,0,12"/>
                <TextBlock Text="Your topping selections:"
                           Style="{StaticResource CaptionTextBlockStyle}"/>
                <TextBlock x:Name="ToppingList"/>
            </StackPanel>
        </SplitView>
    </Grid>

</Page>
```

```csharp
using System.Collections.ObjectModel;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using muxc = Microsoft.UI.Xaml.Controls;

namespace TreeViewTest
{
    public sealed partial class MainPage : Page
    {
        private ObservableCollection<Item> DataSource = new ObservableCollection<Item>();

        public MainPage()
        {
            this.InitializeComponent();
            DataSource = GetDessertData();
        }

        private ObservableCollection<Item> GetDessertData()
        {
            var list = new ObservableCollection<Item>();

            Item flavorsCategory = new Item()
            {
                Name = "Flavors",
                Children =
                {
                    new Item() { Name = "Vanilla" },
                    new Item() { Name = "Strawberry" },
                    new Item() { Name = "Chocolate" }
                }
            };

            Item toppingsCategory = new Item()
            {
                Name = "Toppings",
                Children =
                {
                    new Item()
                    {
                        Name = "Candy",
                        Children =
                        {
                            new Item() { Name = "Chocolate" },
                            new Item() { Name = "Mint" },
                            new Item() { Name = "Sprinkles" }
                        }
                    },
                    new Item()
                    {
                        Name = "Fruits",
                        Children =
                        {
                            new Item() { Name = "Mango" },
                            new Item() { Name = "Peach" },
                            new Item() { Name = "Kiwi" }
                        }
                    },
                    new Item()
                    {
                        Name = "Berries",
                        Children =
                        {
                            new Item() { Name = "Strawberry" },
                            new Item() { Name = "Blueberry" },
                            new Item() { Name = "Blackberry" }
                        }
                    }
                }
            };

            list.Add(flavorsCategory);
            list.Add(toppingsCategory);
            return list;
        }

        private void OrderButton_Click(object sender, RoutedEventArgs e)
        {
            FlavorList.Text = string.Empty;
            ToppingList.Text = string.Empty;

            foreach (muxc.TreeViewNode node in DessertTree.SelectedNodes)
            {
                if (node.Parent.Content?.ToString() == "Flavors")
                {
                    FlavorList.Text += node.Content + "; ";
                }
                else if (node.HasChildren == false)
                {
                    ToppingList.Text += node.Content + "; ";
                }
            }
        }

        private void SelectAllButton_Click(object sender, RoutedEventArgs e)
        {
            if (DessertTree.SelectionMode == muxc.TreeViewSelectionMode.Multiple)
            {
                DessertTree.SelectAll();
            }
        }
    }

    public class Item
    {
        public string Name { get; set; }
        public ObservableCollection<Item> Children { get; set; } = new ObservableCollection<Item>();

        public override string ToString()
        {
            return Name;
        }
    }
}

```

### <a name="pictures-and-music-library-tree-view"></a>画像やミュージック ライブラリのツリー ビュー

この例では、ユーザーの **Pictures** ライブラリと **Music** ライブラリのコンテンツや構造を表示するツリー ビューを作成する方法を示します。 項目の数は事前に知ることができないため、各ノードは展開時に入力され、折りたたまれたときに空になります。

カスタム項目テンプレートは、型が [IStorageItem](/uwp/api/windows.storage.istorageitem) であるデータ項目を表示するために使用されます。

> [!IMPORTANT]
> この例のコードでは、**picturesLibrary** 機能と **musicLibrary** 機能が必要です。 ファイル アクセスの詳細については、[ファイル アクセス許可](../../files/file-access-permissions.md)、[ファイルとフォルダーの列挙と照会](../../files/quickstart-listing-files-and-folders.md)、および[ミュージック、画像、およびビデオ ライブラリのファイルとフォルダー](../../files/quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md) をご覧ください。

```xaml
<Page
    x:Class="TreeViewTest.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:TreeViewTest"
    xmlns:muxc="using:Microsoft.UI.Xaml.Controls"
    xmlns:storage="using:Windows.Storage"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">
    <Page.Resources>
        <DataTemplate x:Key="TreeViewItemDataTemplate" x:DataType="muxc:TreeViewNode">
            <Grid Height="44">
                <TextBlock Text="{x:Bind ((storage:IStorageItem)Content).Name}"
                           HorizontalAlignment="Left"
                           VerticalAlignment="Center"
                           Style="{ThemeResource BodyTextBlockStyle}"/>
            </Grid>
        </DataTemplate>

        <DataTemplate x:Key="MusicItemDataTemplate" x:DataType="muxc:TreeViewNode">
            <StackPanel Height="44" Orientation="Horizontal">
                <SymbolIcon Symbol="Audio" Margin="0,0,4,0"/>
                <TextBlock Text="{x:Bind ((storage:StorageFile)Content).DisplayName}"
                           HorizontalAlignment="Left"
                           VerticalAlignment="Center"
                           Style="{ThemeResource BodyTextBlockStyle}"/>
            </StackPanel>
        </DataTemplate>

        <DataTemplate x:Key="PictureItemDataTemplate" x:DataType="muxc:TreeViewNode">
            <StackPanel Height="44" Orientation="Horizontal">
                <FontIcon FontFamily="Segoe MDL2 Assets" Glyph="&#xEB9F;"
                          Margin="0,0,4,0"/>
                <TextBlock Text="{x:Bind ((storage:StorageFile)Content).DisplayName}"
                           HorizontalAlignment="Left"
                           VerticalAlignment="Center"
                           Style="{ThemeResource BodyTextBlockStyle}"/>
            </StackPanel>
        </DataTemplate>

        <DataTemplate x:Key="MusicFolderDataTemplate" x:DataType="muxc:TreeViewNode">
            <StackPanel Height="44" Orientation="Horizontal">
                <SymbolIcon Symbol="MusicInfo" Margin="0,0,4,0"/>
                <TextBlock Text="{x:Bind ((storage:StorageFolder)Content).DisplayName}"
                           HorizontalAlignment="Left"
                           VerticalAlignment="Center"
                           Style="{ThemeResource BodyTextBlockStyle}"/>
            </StackPanel>
        </DataTemplate>

        <DataTemplate x:Key="PictureFolderDataTemplate" x:DataType="muxc:TreeViewNode">
            <StackPanel Height="44" Orientation="Horizontal">
                <SymbolIcon Symbol="Pictures" Margin="0,0,4,0"/>
                <TextBlock Text="{x:Bind ((storage:StorageFolder)Content).DisplayName}"
                           HorizontalAlignment="Left"
                           VerticalAlignment="Center"
                           Style="{ThemeResource BodyTextBlockStyle}"/>
            </StackPanel>
        </DataTemplate>

        <local:ExplorerItemTemplateSelector
            x:Key="ExplorerItemTemplateSelector"
            DefaultTemplate="{StaticResource TreeViewItemDataTemplate}"
            MusicItemTemplate="{StaticResource MusicItemDataTemplate}"
            MusicFolderTemplate="{StaticResource MusicFolderDataTemplate}"
            PictureItemTemplate="{StaticResource PictureItemDataTemplate}"
            PictureFolderTemplate="{StaticResource PictureFolderDataTemplate}"/>
    </Page.Resources>

    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <SplitView IsPaneOpen="True"
                   DisplayMode="Inline"
                   OpenPaneLength="296">
            <SplitView.Pane>
                <Grid>
                    <Grid.RowDefinitions>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition/>
                    </Grid.RowDefinitions>
                    <Button Content="Refresh tree" Click="RefreshButton_Click" Margin="24,12"/>
                    <muxc:TreeView x:Name="sampleTreeView" Grid.Row="1" SelectionMode="Single"
                              ItemTemplateSelector="{StaticResource ExplorerItemTemplateSelector}"
                              Expanding="SampleTreeView_Expanding"
                              Collapsed="SampleTreeView_Collapsed"
                              ItemInvoked="SampleTreeView_ItemInvoked"/>
                </Grid>
            </SplitView.Pane>

            <StackPanel Grid.Column="1" Margin="12,72">
                <TextBlock Text="File name:" Style="{StaticResource CaptionTextBlockStyle}"/>
                <TextBlock x:Name="FileNameTextBlock" Margin="0,0,0,12"/>

                <TextBlock Text="File path:" Style="{StaticResource CaptionTextBlockStyle}"/>
                <TextBlock x:Name="FilePathTextBlock" Margin="0,0,0,12"/>

                <TextBlock Text="Tree depth:" Style="{StaticResource CaptionTextBlockStyle}"/>
                <TextBlock x:Name="TreeDepthTextBlock" Margin="0,0,0,12"/>
            </StackPanel>
        </SplitView>
    </Grid>
</Page>
```

```csharp
using System;
using System.Collections.Generic;
using Windows.Storage;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using muxc = Microsoft.UI.Xaml.Controls;

namespace TreeViewTest
{
    public sealed partial class MainPage : Page
    {
        public MainPage()
        {
            this.InitializeComponent();
            InitializeTreeView();
        }

        private void InitializeTreeView()
        {
            // A TreeView can have more than 1 root node. The Pictures library
            // and the Music library will each be a root node in the tree.
            // Get Pictures library.
            StorageFolder picturesFolder = KnownFolders.PicturesLibrary;
            muxc.TreeViewNode pictureNode = new muxc.TreeViewNode();
            pictureNode.Content = picturesFolder;
            pictureNode.IsExpanded = true;
            pictureNode.HasUnrealizedChildren = true;
            sampleTreeView.RootNodes.Add(pictureNode);
            FillTreeNode(pictureNode);

            // Get Music library.
            StorageFolder musicFolder = KnownFolders.MusicLibrary;
            muxc.TreeViewNode musicNode = new muxc.TreeViewNode();
            musicNode.Content = musicFolder;
            musicNode.IsExpanded = true;
            musicNode.HasUnrealizedChildren = true;
            sampleTreeView.RootNodes.Add(musicNode);
            FillTreeNode(musicNode);
        }

        private async void FillTreeNode(muxc.TreeViewNode node)
        {
            // Get the contents of the folder represented by the current tree node.
            // Add each item as a new child node of the node that's being expanded.

            // Only process the node if it's a folder and has unrealized children.
            StorageFolder folder = null;

            if (node.Content is StorageFolder && node.HasUnrealizedChildren == true)
            {
                folder = node.Content as StorageFolder;
            }
            else
            {
                // The node isn't a folder, or it's already been filled.
                return;
            }

            IReadOnlyList<IStorageItem> itemsList = await folder.GetItemsAsync();

            if (itemsList.Count == 0)
            {
                // The item is a folder, but it's empty. Leave HasUnrealizedChildren = true so
                // that the chevron appears, but don't try to process children that aren't there.
                return;
            }

            foreach (var item in itemsList)
            {
                var newNode = new muxc.TreeViewNode();
                newNode.Content = item;

                if (item is StorageFolder)
                {
                    // If the item is a folder, set HasUnrealizedChildren to true.
                    // This makes the collapsed chevron show up.
                    newNode.HasUnrealizedChildren = true;
                }
                else
                {
                    // Item is StorageFile. No processing needed for this scenario.
                }

                node.Children.Add(newNode);
            }

            // Children were just added to this node, so set HasUnrealizedChildren to false.
            node.HasUnrealizedChildren = false;
        }

        private void SampleTreeView_Expanding(muxc.TreeView sender, muxc.TreeViewExpandingEventArgs args)
        {
            if (args.Node.HasUnrealizedChildren)
            {
                FillTreeNode(args.Node);
            }
        }

        private void SampleTreeView_Collapsed(muxc.TreeView sender, muxc.TreeViewCollapsedEventArgs args)
        {
            args.Node.Children.Clear();
            args.Node.HasUnrealizedChildren = true;
        }

        private void SampleTreeView_ItemInvoked(muxc.TreeView sender, muxc.TreeViewItemInvokedEventArgs args)
        {
            var node = args.InvokedItem as muxc.TreeViewNode;

            if (node.Content is IStorageItem item)
            {
                FileNameTextBlock.Text = item.Name;
                FilePathTextBlock.Text = item.Path;
                TreeDepthTextBlock.Text = node.Depth.ToString();

                if (node.Content is StorageFolder)
                {
                    node.IsExpanded = !node.IsExpanded;
                }
            }
        }

        private void RefreshButton_Click(object sender, RoutedEventArgs e)
        {
            sampleTreeView.RootNodes.Clear();
            InitializeTreeView();
        }
    }

    public class ExplorerItemTemplateSelector : DataTemplateSelector
    {
        public DataTemplate DefaultTemplate { get; set; }
        public DataTemplate MusicItemTemplate { get; set; }
        public DataTemplate PictureItemTemplate { get; set; }
        public DataTemplate MusicFolderTemplate { get; set; }
        public DataTemplate PictureFolderTemplate { get; set; }

        protected override DataTemplate SelectTemplateCore(object item)
        {
            var node = (muxc.TreeViewNode)item;

            if (node.Content is StorageFolder)
            {
                var content = node.Content as StorageFolder;
                if (content.DisplayName.StartsWith("Pictures")) return PictureFolderTemplate;
                if (content.DisplayName.StartsWith("Music")) return MusicFolderTemplate;
            }
            else if (node.Content is StorageFile)
            {
                var content = node.Content as StorageFile;
                if (content.ContentType.StartsWith("image")) return PictureItemTemplate;
                if (content.ContentType.StartsWith("audio")) return MusicItemTemplate;

            }
            return DefaultTemplate;
        }
    }
}
```

```vb
Public NotInheritable Class MainPage
    Inherits Page

    Public Sub New()
        InitializeComponent()
        InitializeTreeView()
    End Sub

    Private Sub InitializeTreeView()
        ' A TreeView can have more than 1 root node. The Pictures library
        ' and the Music library will each be a root node in the tree.
        ' Get Pictures library.
        Dim picturesFolder As StorageFolder = KnownFolders.PicturesLibrary
        Dim pictureNode As New muxc.TreeViewNode With {
        .Content = picturesFolder,
        .IsExpanded = True,
        .HasUnrealizedChildren = True
    }
        sampleTreeView.RootNodes.Add(pictureNode)
        FillTreeNode(pictureNode)

        ' Get Music library.
        Dim musicFolder As StorageFolder = KnownFolders.MusicLibrary
        Dim musicNode As New muxc.TreeViewNode With {
        .Content = musicFolder,
        .IsExpanded = True,
        .HasUnrealizedChildren = True
    }
        sampleTreeView.RootNodes.Add(musicNode)
        FillTreeNode(musicNode)
    End Sub

    Private Async Sub FillTreeNode(node As muxc.TreeViewNode)
        ' Get the contents of the folder represented by the current tree node.
        ' Add each item as a new child node of the node that's being expanded.

        ' Only process the node if it's a folder and has unrealized children.
        Dim folder As StorageFolder = Nothing
        If TypeOf node.Content Is StorageFolder AndAlso node.HasUnrealizedChildren Then
            folder = TryCast(node.Content, StorageFolder)
        Else
            ' The node isn't a folder, or it's already been filled.
            Return
        End If

        Dim itemsList As IReadOnlyList(Of IStorageItem) = Await folder.GetItemsAsync()
        If itemsList.Count = 0 Then
            ' The item is a folder, but it's empty. Leave HasUnrealizedChildren = true so
            ' that the chevron appears, but don't try to process children that aren't there.
            Return
        End If

        For Each item In itemsList
            Dim newNode As New muxc.TreeViewNode With {
            .Content = item
        }
            If TypeOf item Is StorageFolder Then
                ' If the item is a folder, set HasUnrealizedChildren to True.
                ' This makes the collapsed chevron show up.
                newNode.HasUnrealizedChildren = True
            Else
                ' Item is StorageFile. No processing needed for this scenario.
            End If
            node.Children.Add(newNode)
        Next

        ' Children were just added to this node, so set HasUnrealizedChildren to False.
        node.HasUnrealizedChildren = False
    End Sub

    Private Sub SampleTreeView_Expanding(sender As muxc.TreeView, args As muxc.TreeViewExpandingEventArgs)
        If args.Node.HasUnrealizedChildren Then
            FillTreeNode(args.Node)
        End If
    End Sub

    Private Sub SampleTreeView_Collapsed(sender As muxc.TreeView, args As muxc.TreeViewCollapsedEventArgs)
        args.Node.Children.Clear()
        args.Node.HasUnrealizedChildren = True
    End Sub

    Private Sub SampleTreeView_ItemInvoked(sender As muxc.TreeView, args As muxc.TreeViewItemInvokedEventArgs)
        Dim node = TryCast(args.InvokedItem, muxc.TreeViewNode)
        Dim item = TryCast(node.Content, IStorageItem)
        If item IsNot Nothing Then
            FileNameTextBlock.Text = item.Name
            FilePathTextBlock.Text = item.Path
            TreeDepthTextBlock.Text = node.Depth.ToString()
            If TypeOf node.Content Is StorageFolder Then
                node.IsExpanded = Not node.IsExpanded
            End If
        End If
    End Sub

    Private Sub RefreshButton_Click(sender As Object, e As RoutedEventArgs)
        sampleTreeView.RootNodes.Clear()
        InitializeTreeView()
    End Sub

End Class

Public Class ExplorerItemTemplateSelector
    Inherits DataTemplateSelector

    Public Property DefaultTemplate As DataTemplate
    Public Property MusicItemTemplate As DataTemplate
    Public Property PictureItemTemplate As DataTemplate
    Public Property MusicFolderTemplate As DataTemplate
    Public Property PictureFolderTemplate As DataTemplate

    Protected Overrides Function SelectTemplateCore(ByVal item As Object) As DataTemplate
        Dim node = CType(item, muxc.TreeViewNode)

        If TypeOf node.Content Is StorageFolder Then
            Dim content = TryCast(node.Content, StorageFolder)
            If content.DisplayName.StartsWith("Pictures") Then Return PictureFolderTemplate
            If content.DisplayName.StartsWith("Music") Then Return MusicFolderTemplate
        ElseIf TypeOf node.Content Is StorageFile Then
            Dim content = TryCast(node.Content, StorageFile)
            If content.ContentType.StartsWith("image") Then Return PictureItemTemplate
            If content.ContentType.StartsWith("audio") Then Return MusicItemTemplate
        End If

        Return DefaultTemplate
    End Function
End Class
```

### <a name="drag-and-drop-items-between-tree-views"></a>ツリー ビュー間で項目をドラッグ アンド ドロップする

次の例では、それぞれの間で項目をドラッグ アンド ドロップできる 2 つのツリー ビューを作成する方法を示します。 他のツリー ビューにドラッグされた項目は、そのリストの末尾に追加されます。 ただし、ツリー ビュー内で項目の順序を変更できます。 また、この例では、ルート ノードが 1 つのツリー ビューだけが考慮されています。

```xaml
<Page
    x:Class="TreeViewTest.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">

    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <Grid.ColumnDefinitions>
            <ColumnDefinition/>
            <ColumnDefinition/>
        </Grid.ColumnDefinitions>

        <TreeView x:Name="treeView1"
                  AllowDrop="True"
                  CanDragItems="True"
                  CanReorderItems="True"
                  DragOver="TreeView_DragOver"
                  Drop="TreeView_Drop"
                  DragItemsStarting="TreeView_DragItemsStarting"
                  DragItemsCompleted="TreeView_DragItemsCompleted"/>
        <TreeView x:Name="treeView2"
                  AllowDrop="True"
                  Grid.Column="1"
                  CanDragItems="True"
                  CanReorderItems="True"
                  DragOver="TreeView_DragOver"
                  Drop="TreeView_Drop"
                  DragItemsStarting="TreeView_DragItemsStarting"
                  DragItemsCompleted="TreeView_DragItemsCompleted"/>

    </Grid>

</Page>
```

```csharp
using System;
using Windows.ApplicationModel.DataTransfer;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;

namespace TreeViewTest
{
    public sealed partial class MainPage : Page
    {
        private TreeViewNode deletedItem;
        private TreeView sourceTreeView;

        public MainPage()
        {
            this.InitializeComponent();
            InitializeTreeView();
        }

        private void InitializeTreeView()
        {
            TreeViewNode parentNode1 = new TreeViewNode() { Content = "tv1" };
            TreeViewNode parentNode2 = new TreeViewNode() { Content = "tv2" };

            parentNode1.Children.Add(new TreeViewNode() { Content = "tv1FirstChild" });
            parentNode1.Children.Add(new TreeViewNode() { Content = "tv1SecondChild" });
            parentNode1.Children.Add(new TreeViewNode() { Content = "tv1ThirdChild" });
            parentNode1.Children.Add(new TreeViewNode() { Content = "tv1FourthChild" });
            parentNode1.IsExpanded = true;
            treeView1.RootNodes.Add(parentNode1);

            parentNode2.Children.Add(new TreeViewNode() { Content = "tv2FirstChild" });
            parentNode2.Children.Add(new TreeViewNode() { Content = "tv2SecondChild" });
            parentNode2.IsExpanded = true;
            treeView2.RootNodes.Add(parentNode2);
        }

        private void TreeView_DragOver(object sender, DragEventArgs e)
        {
            if (e.DataView.Contains(StandardDataFormats.Text))
            {
                e.AcceptedOperation = DataPackageOperation.Move;
            }
        }

        private async void TreeView_Drop(object sender, DragEventArgs e)
        {
            if (e.DataView.Contains(StandardDataFormats.Text))
            {
                string text = await e.DataView.GetTextAsync();
                TreeView destinationTreeView = sender as TreeView;

                if (destinationTreeView.RootNodes != null)
                {
                    TreeViewNode newNode = new TreeViewNode() { Content = text };
                    destinationTreeView.RootNodes[0].Children.Add(newNode);
                    deletedItem = newNode;
                }
            }
        }

        private void TreeView_DragItemsStarting(TreeView sender, TreeViewDragItemsStartingEventArgs args)
        {
            if (args.Items.Count == 1)
            {
                args.Data.RequestedOperation = DataPackageOperation.Move;
                sourceTreeView = sender;

                foreach (var item in args.Items)
                {
                    args.Data.SetText(item.ToString());
                }
            }
        }

        private void TreeView_DragItemsCompleted(TreeView sender, TreeViewDragItemsCompletedEventArgs args)
        {
            var children = sourceTreeView.RootNodes[0].Children;

            if (deletedItem != null)
            {
                for (int i = 0; i < children.Count; i++)
                {
                    if (children[i].Content.ToString() == deletedItem.Content.ToString())
                    {
                        children.RemoveAt(i);
                        break;
                    }
                }
            }

            sourceTreeView = null;
            deletedItem = null;
        }
    }
}
```

## <a name="related-articles"></a>関連記事

- [TreeView クラス](/uwp/api/windows.ui.xaml.controls.treeview)
- [ListView クラス](/uwp/api/windows.ui.xaml.controls.listview)
- [ListView と GridView](listview-and-gridview.md)