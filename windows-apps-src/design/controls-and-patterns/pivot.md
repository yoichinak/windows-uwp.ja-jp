---
description: Pivot コントロールを使用すると、少数のコンテンツ セクション間のタッチ スワイプが可能になります。
title: Pivot
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
pm-contact: yulikl
design-contact: kimsea
dev-contact: llongley
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: eaef3bb57eb8719ac4183f21b764ece98cae22fe
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2020
ms.locfileid: "93030855"
---
# <a name="pivot"></a>Pivot

[Pivot](/uwp/api/Windows.UI.Xaml.Controls.Pivot) コントロールを使用すると、少数のコンテンツ セクション間のタッチ スワイプが可能になります。

![既定のフォーカスでは選択されたヘッダーが下線付きで表示される](images/pivot_focus_selectedHeader.png)

**Windows UI ライブラリを入手する**

:::row:::
   :::column:::
      ![WinUI ロゴ](images/winui-logo-64x64.png)
   :::column-end:::
   :::column span="3":::
      Windows UI ライブラリ 2.2 以降には、丸めた角を使用するこのコントロールの新しいテンプレートが含まれます。 詳しくは、「[角の半径](../style/rounded-corner.md)」をご覧ください。 WinUI は、Windows アプリの新しいコントロールと UI 機能が含まれる NuGet パッケージです。 インストール手順などについて詳しくは、「[Windows UI Library (Windows UI ライブラリ)](/uwp/toolkits/winui/)」をご覧ください。
   :::column-end:::
   :::column:::

   :::column-end:::
:::row-end:::

> **プラットフォーム API** : [Pivot クラス](/uwp/api/Windows.UI.Xaml.Controls.Pivot)、 [NavigationView クラス](/uwp/api/Windows.UI.Xaml.Controls.NavigationView)

## <a name="examples"></a>例

<table>
<th align="left">XAML コントロール ギャラリー<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML コントロール ギャラリー</strong> アプリがインストールされている場合は、こちらをクリックして<a href="xamlcontrolsgallery:/item/Pivot">アプリを開き、Pivot コントロールの動作を確認</a>してください。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML コントロール ギャラリー アプリを入手する (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">ソース コード (GitHub) を入手する</a></li>
    </ul>
</td>
</tr>
</table>

Pivot コントロールは、[NavigationView](navigationview.md) と同様に、選択した項目に下線を付けます。

![既定のフォーカスでは選択されたヘッダーが下線付きで表示される](images/pivot_focus_selectedHeader.png)

## <a name="is-this-the-right-control"></a>これは適切なコントロールですか?

一般的な上部ナビゲーションとタブのパターンを達成するには、自動的にさまざまな画面サイズに適合し、より多くのカスタマイズが可能な [NavigationView](navigationview.md) を使用することをお勧めします。

ただし、ナビゲーションでタッチ スワイプが必要な場合は、Pivot を使用することをお勧めします。

NavigationView コントロールと Pivot コントロールのその他の主な違いは、既定のオーバーフロー動作とナビゲーション API です。

- Pivot では、オーバーフロー項目はカルーセル表示されますが、NavigationView では、ユーザーがすべての項目を見られるように、メニュー ドロップダウン オーバーフローが使用されます。
- Pivot ではコンテンツ セクション間のナビゲーションが処理されますが、NavigationView では、ナビゲーション動作をより厳密に制御することができます。

## <a name="use-navigationview-instead-of-pivot"></a>Pivot ではなく NavigationView を使用する

アプリの UI で Pivot コントロールが使用されている場合は、以下のコードを使用して Pivot を NavigationView に変換することができます。

この XAML は、「[ピボット コントロールの作成](#create-a-pivot-control)」の Pivot の例のように、3 つのコンテンツ セクションを持つ NavigationView を作成します。

```xaml
<NavigationView x:Name="rootNavigationView" Header="Category Title"
 ItemInvoked="NavView_ItemInvoked">
    <NavigationView.MenuItems>
        <NavigationViewItem Content="Section 1" x:Name="Section1Content" />
        <NavigationViewItem Content="Section 2" x:Name="Section2Content" />
        <NavigationViewItem Content="Section 3" x:Name="Section3Content" />
    </NavigationView.MenuItems>
</NavigationView>

<Page x:Class="AppName.Section1Page">
    <TextBlock Text="Content of section 1."/>
</Page>

<Page x:Class="AppName.Section2Page">
    <TextBlock Text="Content of section 2."/>
</Page>

<Page x:Class="AppName.Section3Page">
    <TextBlock Text="Content of section 3."/>
</Page>
```

NavigationView ではナビゲーションのカスタマイズに対してより厳密な制御が提供され、また対応する分離コードが必要になります。 上記の XAML には、次の分離コードを使用します。

```csharp
private void NavView_ItemInvoked(NavigationView sender, NavigationViewItemInvokedEventArgs args)
{
    FrameNavigationOptions navOptions = new FrameNavigationOptions();
    navOptions.TransitionInfoOverride = new SlideNavigationTransitionInfo() {
         SlideNavigationTransitionDirection=args.RecommendedNavigationTransitionInfo
    };

    navOptions.IsNavigationStackEnabled = False;

    switch (item.Name)
    {
        case "Section1Content":
            ContentFrame.NavigateToType(typeof(Section1Page), null, navOptions);
            break;

        case "Section2Content":
            ContentFrame.NavigateToType(typeof(Section2Page), null, navOptions);
            break;

        case "Section3Content":
            ContentFrame.NavigateToType(typeof(Section3Page), null, navOptions);
            break;
    }  
}
```

このコードは、Pivot コントロールの組み込みナビゲーション エクスペリエンスを模倣し、そこからコンテンツ セクション間のタッチ スワイプ エクスペリエンスを差し引きます。 ただし、ご覧のとおり、アニメーション化された遷移、ナビゲーション パラメーター、スタック機能など、いくつかの点もカスタマイズできます。

## <a name="create-a-pivot-control"></a>ピボット コントロールの作成

このコードは、3 つのコンテンツ セクションを持つ基本的な Pivot コントロールを作成します。

```xaml
<Pivot x:Name="rootPivot" Title="Category Title">
    <PivotItem Header="Section 1">
        <!--Pivot content goes here-->
        <TextBlock Text="Content of section 1."/>
    </PivotItem>
    <PivotItem Header="Section 2">
        <!--Pivot content goes here-->
        <TextBlock Text="Content of section 2."/>
    </PivotItem>
    <PivotItem Header="Section 3">
        <!--Pivot content goes here-->
        <TextBlock Text="Content of section 3."/>
    </PivotItem>
</Pivot>
```

### <a name="pivot-items"></a>ピボット項目

Pivot は [ItemsControl](/uwp/api/Windows.UI.Xaml.Controls.ItemsControl) であるため、あらゆる種類の項目のコレクションを含めることができます。 ピボットに追加する項目が明示的に [PivotItem](/uwp/api/Windows.UI.Xaml.Controls.PivotItem) ではない場合、PivotItem で暗黙的にラップされます。 ピボットは通常コンテンツのページ間を移動するために使用されるため、XAML UI 要素を使用して直接 [Items](/uwp/api/windows.ui.xaml.controls.itemscontrol.items) コレクションを設定するのが一般的です。 または、[ItemsSource](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) プロパティをデータ ソースに設定することもできます。 ItemsSource にバインドされている項目は、任意の型にすることができますが、明示的に PivotItem ではない場合は、[ItemTemplate](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate)と [HeaderTemplate](/uwp/api/windows.ui.xaml.controls.pivot.headertemplate) を定義して、項目を表示する方法を指定する必要があります。

[SelectedItem](/uwp/api/windows.ui.xaml.controls.pivot.selecteditem) プロパティを使って、ピボットのアクティブな項目を取得または設定できます。 アクティブな項目のインデックスを取得または設定するには、[SelectedIndex](/uwp/api/windows.ui.xaml.controls.pivot.selectedindex) プロパティを使います。

### <a name="pivot-headers"></a>ピボット ヘッダー

[LeftHeader](/uwp/api/windows.ui.xaml.controls.pivot.leftheader) プロパティと [RightHeader](/uwp/api/windows.ui.xaml.controls.pivot.rightheader) プロパティを使って、ピボット ヘッダーに他のコントロールを追加できます。

たとえば、[CommandBar](./app-bars.md) をピボットの RightHeader に追加できます。

```xaml
<Pivot>
    <Pivot.RightHeader>
        <CommandBar>
                <AppBarButton Icon="Add"/>
                <AppBarSeparator/>
                <AppBarButton Icon="Edit"/>
                <AppBarButton Icon="Delete"/>
                <AppBarSeparator/>
                <AppBarButton Icon="Save"/>
        </CommandBar>
    </Pivot.RightHeader>
</Pivot>
```

### <a name="pivot-interaction"></a>ピボットの操作

ピボット コントロールを使うと、次のタッチ ジェスチャ操作が可能になります。

- ピボット項目のヘッダーをタップすると、そのヘッダーのセクション コンテンツに移動します。
- ピボット項目のヘッダー上で左または右へスワイプすると、隣接するセクションに移動します。
- セクション コンテンツ上で左または右へスワイプすると、隣接するセクションに移動します。

コントロールには次の 2 つのモードがあります。

**固定**

- 許可されている領域内にすべてのピボット ヘッダーが収まる場合、ピボットは固定されます。
- ピボット ラベルをタップすると、ピボット自体は移動しませんが、対応するページに移動します。 アクティブなピボットは強調表示されます。

**カルーセル**

- 許可されている領域内にすべてのピボット ヘッダーが収まらない場合、ピボットがカルーセル表示されます。
- ピボット ラベルをタップすると対応するページに移動し、アクティブなピボット ラベルは最初の位置までカルーセル表示されます。
- カルーセル内のピボット項目は、最後のピボット セクションから最初のピボット セクションにループします。

> **注** ピボット ヘッダーを [10 フィート環境](../devices/designing-for-tv.md)でカルーセル表示しないでください。 Xbox 上でアプリを実行する場合は、 [IsHeaderItemsCarouselEnabled](/uwp/api/Windows.UI.Xaml.Controls.Pivot.IsHeaderItemsCarouselEnabled) プロパティを **false** に設定します。

## <a name="recommendations"></a>推奨事項

- カルーセル (ラウンドト リップ) モードを使う場合、5 個より多いヘッダーを使わないでください。5 個より多いヘッダーをループすると、混乱を招く可能性があります。

## <a name="get-the-sample-code"></a>サンプル コードの入手

- [XAML コントロール ギャラリー サンプル](https://github.com/Microsoft/Xaml-Controls-Gallery) - 対話形式で XAML コントロールのすべてを参照できます。

## <a name="related-topics"></a>関連トピック

- [Pivot クラス](/uwp/api/Windows.UI.Xaml.Controls.Pivot)
- [ナビゲーション デザインの基本](../basics/navigation-basics.md)
