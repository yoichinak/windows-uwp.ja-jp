---
description: ラジオ ボタンを使用して、相互排他的であるものの、関連している 2 つ以上のオプションのコレクションから 1 つのオプションをユーザーが選択できるようにする方法について説明します。
title: ラジオ ボタンのガイドライン
ms.assetid: 41E3F928-AA55-42A2-9281-EC3907C4F898
label: Radio buttons
template: detail.hbs
ms.date: 06/10/2020
ms.topic: article
keywords: windows 10, uwp
pm-contact: kisai
design-contact: kimsea
dev-contact: mitra
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: ee933bd28594263e61e654b14b0541c6fa9ed41b
ms.sourcegitcommit: 875bd348608547e7a66fa4b460efe64b3246807e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/14/2020
ms.locfileid: "90080844"
---
# <a name="radio-buttons"></a>ラジオ ボタン

ラジオ ボタン (オプション ボタンとも呼ばれます) は、相互排他的であるものの、関連している 2 つ以上のオプションのコレクションから 1 つのオプションをユーザーが選択できるようにするボタンです。 ラジオ ボタンは常にグループで使用され、各オプションはグループ内の 1 つのラジオボタンで表されます。

既定の状態では、RadioButtons グループ内のラジオ ボタンはどれも選択されていません。 つまり、すべてのオプション ボタンがオフになっています。 ただし、ユーザーがラジオ ボタンを選択すると、そのボタンの選択を解除して、元のオフの状態にグループを戻すことはできなくなります。

RadioButtons グループのこの独特の動作は、複数選択や選択の解除 (クリア) がサポートされている[チェック ボックス](checkbox.md)とは異なる点です。

:::image type="content" source="images/controls/radio-button.png" alt-text="1 つのラジオ ボタンが選択されている RadioButtons グループの例":::

**Windows UI ライブラリを入手する**

| &nbsp; | &nbsp; |
| - | - |
| ![WinUI ロゴ](images/winui-logo-64x64.png) | RadioButtons コントロールは、Windows アプリのための新しいコントロールと UI 機能を含む NuGet パッケージである、Windows UI ライブラリの一部として含まれています。 インストール手順などについて詳しくは、「[Windows UI Library (Windows UI ライブラリ)](/uwp/toolkits/winui/)」をご覧ください。 |

> **Windows UI ライブラリ API**: [RadioButtons クラス](/uwp/api/microsoft.ui.xaml.controls.radiobuttons), [SelectedItem プロパティ](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selecteditem), [SelectedIndex プロパティ](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selectedindex), [SelectionChanged イベント](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selectionchanged)
>
> **プラットフォーム API**: [RadioButton クラス](/uwp/api/Windows.UI.Xaml.Controls.RadioButton), [IsChecked プロパティ](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton.IsChecked), [Checked イベント](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton.Checked)

> [!IMPORTANT]
> **RadioButtons と RadioButton**
>
>ラジオ ボタン グループを作成するには、次の 2 つの方法があります。
>
>- WinUI 2.3 以降では、 **[RadioButtons](/uwp/api/microsoft.ui.xaml.controls.radiobuttons)** コントロールを使用することをお勧めします。 このコントロールでは、レイアウトの簡略化、キーボード ナビゲーションとアクセシビリティの処理、データ ソースへのバインドのサポートがなされています。
>- 個々の **[RadioButton](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RadioButton)** コントロールのグループを使用できます。 アプリで WinUI 2.3 以降が使用されていない場合は、これが唯一のオプションです。

## <a name="is-this-the-right-control"></a>これは適切なコントロールですか?

ユーザーが 2 つ以上の相互排他的なオプションから選択できるようにするには、ラジオ ボタンを使用します。

:::image type="content" source="images/radiobutton_basic.png" alt-text="1 つのラジオ ボタンが選択されている RadioButtons グループ":::

選択する前にユーザーがすべてのオプションを見る必要がある場合は、ラジオ ボタンを使用します。 ラジオ ボタンでは、すべてのオプションが同じように強調されます。これは、一部のオプションが必要以上に、または望ましいレベルを超えて、注目される可能性があることを意味します。

どのオプションも均等に注意を引く場合を除き、他のコントロールを使うことを検討してください。 たとえば、ほとんどのユーザーおよびほとんどの状況に対して 1 つの最適なオプションを推奨するには、[コンボ ボックス](combo-box.md)を使用して、その最適なオプションを既定のオプションとして表示します。

:::image type="content" source="images/combo_box_collapsed.png" alt-text="既定のオプションが表示されているコンボ ボックス":::

考えられるオプションは 2 つのみで、それらを 1 つの二者択一ではっきり示すことのできる場合 ("オン/オフ" や "はい/いいえ" など) は、それらを 1 つの[チェック ボックス](checkbox.md)または[トグル スイッチ](toggles.md) コントロールにまとめます。 たとえば、"同意する" と "同意しない" という 2 つのラジオ ボタンではなく、"同意する" という 1 つのチェック ボックスを使用します。

以下のように 1 つの二者択一で 2 つのラジオ ボタンを使用することはしないでください。

:::image type="content" source="images/radiobutton-vs-checkbox-rb.png" alt-text="1 つの二者択一を表す 2 つのラジオ ボタン":::

代わりに、以下のようにチェック ボックスを使用します。

:::image type="content" source="images/radiobutton-vs-checkbox-cb.png" alt-text="チェック ボックスは二者択一を表すのに適した代替手段である":::

ユーザーが複数のオプションを選択できる場合は、[チェック ボックス](checkbox.md)を使用します。

:::image type="content" source="images/checkbox2.png" alt-text="チェック ボックスでは複数選択がサポートされる":::

ユーザーのオプションがある値の範囲に限定される場合は (例: *10、20、30、... 100*)、[スライダー](slider.md) コントロールを使用します。

:::image type="content" source="images/controls/slider.png" alt-text="ある値の範囲内の 1 つの値が表示されているスライダー コントロール":::

オプションが 8 個より多い場合は、[コンボ ボックス](combo-box.md)を使用します。

:::image type="content" source="images/combo_box_scroll.png" alt-text="複数のオプションが表示されているリスト ボックス":::

使用できるオプションが、アプリの現在のコンテキストに基づく場合、またはそれ以外の理由で非常に動的な場合は、リスト コントロールを使用します。

## <a name="examples"></a>例

<table>
<th align="left">XAML コントロール ギャラリー<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="The XAML Controls Gallery app icon"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML コントロール ギャラリー</strong>のアプリをインストールしてある場合は、<a href="xamlcontrolsgallery:/item/RadioButton">それを開いて RadioButtons コントロールの動作を確認</a>してください。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML コントロール ギャラリー アプリを入手する (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">ソース コード (GitHub) を入手する</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="winui-radiobuttons-overview"></a>WinUI RadioButtons の概要

キーボード アクセスおよびナビゲーション動作は、[RadioButtons](/uwp/api/microsoft.ui.xaml.controls.radiobuttons) コントロールで最適化されています。 これらの機能強化は、アクセシビリティと、キーボード パワー ユーザーによるオプションの一覧のすばやく簡単な移動の両方に役立ちます。

これらの機能強化に加えて、RadioButtons グループ内の個々のラジオ ボタンの既定の表示レイアウトが、自動化な向き、間隔、余白の設定によって最適化されています。 この最適化により、[StackPanel](../layout/layout-panels.md#stackpanel) や [Grid](../layout/layout-panels.md#grid) などのよりプリミティブなグループ化コントロールを使用する場合にはこれらのプロパティを使用する必要がありましたが、その必要がなくなりました。

### <a name="navigating-a-radiobuttons-group"></a>RadioButtons グループ内の移動

`RadioButtons` コントロールには、キーボード ユーザーがリストをすばやく簡単に移動するのに役立つ特別なナビゲーション動作が用意されています。

#### <a name="keyboard-focus"></a>キーボード フォーカス

`RadioButtons` コントロールでは、以下の 2 つの状態がサポートされています。

- どのラジオ ボタンも選択されていない
- 1 つのラジオ ボタンが選択されている

以下のセクションで、各状態でのコントロールのフォーカスの動作について説明します。

##### <a name="no-radio-button-is-selected"></a>どのラジオ ボタンも選択されていない

どのラジオ ボタンも選択されていない場合、リスト内の最初のラジオ ボタンがフォーカスされます。

> [!NOTE]
> 最初のタブ ナビゲーションからタブ フォーカスを受け取る項目が、選択されていません。

:::row:::
   :::column span="":::
     **_タブ フォーカスのないリスト、未選択_**

     ![タブ フォーカスされていないリストの項目未選択の状態](images/radiobutton-no-selected-item-no-tab-focus.png)
   :::column-end:::
   :::column span="":::
      **_初期タブ フォーカスのあるリスト、未選択_**

      ![タブで初期フォーカスされているリストの項目未選択の状態](images/radiobutton-no-selected-item-tab-focus.png)
   :::column-end:::
:::row-end:::

##### <a name="one-radio-button-is-selected"></a>1 つのラジオ ボタンが選択されている

ラジオ ボタンが既に選択されているリスト内にユーザーがタブ キーで移動すると、選択されているラジオ ボタンがフォーカスされます。

:::row:::
   :::column span="":::
     **_タブ フォーカスされていないリスト_**

     ![タブ フォーカスされていないリストの 1 つの項目が選択された状態](images/radiobutton-selected-item-no-tab-focus.png)
   :::column-end:::
   :::column span="":::
      **_タブで初期フォーカスされているリスト_**

      ![タブで初期フォーカスされているリストの 1 つの項目が選択された状態](images/radiobutton-selected-item-tab-focus.png)
   :::column-end:::
:::row-end:::

#### <a name="keyboard-navigation"></a>キーボード ナビゲーション

一般的なキーボード ナビゲーションの動作の詳細については、[キーボード操作のナビゲーション](../input/keyboard-interactions.md#navigation)に関するページを参照してください。

`RadioButtons` グループの項目が既にフォーカスされている場合、ユーザーは、グループ内の項目間の "内部ナビゲーション" に方向キーを使用できます。 上下の方向キーを使用すると、XAML マークアップの定義に従い、"次" または "前" の論理項目に移動します。 左右の方向キーを使用すると、空間的に移動します。

##### <a name="navigation-within-single-column-or-single-row-layouts"></a>単一列または単一行のレイアウト内のナビゲーション

単一列または単一行のレイアウトでは、キーボード ナビゲーションは以下のように動作します。

:::row:::
   :::column span="":::
     **_単一列_**

     ![単一列の RadioButtons グループ内のキーボード ナビゲーションの例](images/radiobutton-keyboard-navigation-single-column.png)

     上下の方向キーで項目間を移動できる。</br>左右の方向キーでは何も変化しない。
   :::column-end:::
   :::column span="":::
      **_単一行_**

      ![単一行の RadioButtons グループ内のキーボード ナビゲーションの例](images/radiobutton-keyboard-navigation-single-row.png)

      左か上の方向キーで前の項目に移動し、右か下の方向キーで次の項目に移動する。
   :::column-end:::
:::row-end:::

##### <a name="navigation-within-multi-column-multi-row-layouts"></a>複数列、複数行のレイアウト内のナビゲーション

複数列、複数行のグリッド レイアウトでは、キーボード ナビゲーションは以下のように動作します。

**_左右の方向キー_**

:::row:::
   :::column span="":::
      ![複数列、複数行の RadioButtons グループ内の水平方向のキーボード ナビゲーションの例](images/radiobutton-keyboard-navigation-multi-column-row-1.png)

      

      左右の方向キーで、行に含まれる項目間をフォーカスが水平方向に移動する。
   :::column-end:::
   :::column span="":::
     ![列の最後の項目がフォーカスされている水平方向のキーボード ナビゲーションの例](images/radiobutton-keyboard-navigation-multi-column-row-3.png)

      列の最後の項目がフォーカスされている状況で右または左の方向キーを押すと、フォーカスは次または前の列の最後の項目に移動します (存在する場合)。
   :::column-end:::
:::row-end:::

**_上下の方向キー_**

:::row:::
   :::column span="":::
      ![複数列、複数行の RadioButtons グループ内の垂直方向のキーボード ナビゲーションの例](images/radiobutton-keyboard-navigation-multi-column-row-2.png)

      上下の方向キーで、列に含まれる項目間をフォーカスが垂直方向に移動する。
   :::column-end:::
   :::column span="":::
     ![列の最後の項目がフォーカスされている垂直方向のキーボード ナビゲーションの例](images/radiobutton-keyboard-navigation-multi-column-row-4.png)

      列の最後の項目がフォーカスされている状況で下方向キーを押すと、フォーカスは次の列の最初の項目に移動します (存在する場合)。 列の最初の項目がフォーカスされている状況で上方向キーを押すと、フォーカスは前の列の最後の項目に移動します (存在する場合)
   :::column-end:::
:::row-end:::

詳細については、「[キーボード操作](../input/keyboard-interactions.md#wrapping-homogeneous-list-and-grid-view-items)」を参照してください。

###### <a name="wrapping"></a>折り返し

RadioButtons グループでは、最初の行または列から最後へ、あるいは最後の行または列から最初へのフォーカスの折り返しは行われません。 これは、ユーザーがスクリーン リーダーを使用していて、境界や、先頭と末尾がはっきりしなくなったときに、視覚障碍のあるユーザーがリスト内を移動しにくくなるためです。

また、`RadioButtons` コントロールでは、コントロールに適切な数の項目が含まれることが意図されているため、列挙もサポートされていません (「[これは適切なコントロールですか?](#is-this-the-right-control)」を参照してください)。

### <a name="selection-follows-focus"></a>フォーカスに連動した選択

キーボードを使用して `RadioButtons` グループ内の項目間を移動する場合、フォーカスが 1 つの項目から次に移動すると、新しくフォーカスが設定された項目はオンになり、前にフォーカスが設定されていた項目はオフになります。

:::row:::
   :::column span="":::
      **_キーボード ナビゲーション前_**

      ![キーボード ナビゲーション前のフォーカスと選択の例](images/radiobutton-two-selected-before-keyboard-navigation.png)

      キーボード ナビゲーション前のフォーカスと選択。
   :::column-end:::
   :::column span="":::
     **_キーボード ナビゲーション後_**

      ![キーボード ナビゲーション後のフォーカスと選択の例](images/radiobutton-three-selected-after-keyboard-navigation.png)

      キーボード ナビゲーション後のフォーカスと選択。下方向キーでフォーカスをラジオ ボタン 3 に移動すると、それが選択され、ラジオ ボタン 2 はオフになります。
   :::column-end:::
:::row-end:::

Ctrl キーを押しながら方向キーを押して移動すると、選択を変更せずにフォーカスを移動できます。 フォーカスの移動後に、Space キーを使用して、現在フォーカスされている項目を選択できます。

#### <a name="navigating-with-xbox-gamepad-and-remote-control"></a>Xbox ゲームパッドやリモート コントロールを使用した移動

Xbox のゲームパッドやリモート コントロールを使用してラジオ ボタン間を移動している場合、"フォーカスに連動した選択" の動作は無効になり、現在フォーカスされているラジオ ボタンを選択するには、ユーザーは "A" ボタンを押す必要があります。

## <a name="accessibility-behavior"></a>アクセシビリティの動作

以下の表で、ナレーターによる `RadioButtons` グループの処理方法と、読み上げられる内容について説明します。 この動作は、ユーザーによるナレーターの詳細設定によって異なります。

| アクション | ナレーターの読み上げ内容 |
|:--|:--|
| 選択された項目にフォーカスが移動する場合 | "<_名前_> RadioButton が選択されています。_N_ 個の中の _x_ 個目です" |
|選択されていない項目にフォーカスが移動する場合<br> " *(Ctrl キーを押しながら方向キーを押すか、Xbox ゲームパッドを使用して移動する場合、<br>選択はフォーカスに連動しないことを示します。)* " | "<_名前_> RadioButton は選択されていません。_N_ 個の中の _x_ 個目です"  |

> [!NOTE]
> 項目ごとにナレーターが読み上げる <_**名前**_> は、その項目で使用可能な場合は [AutomationProperties.Name](/uwp/api/windows.ui.xaml.automation.automationproperties.nameproperty) 添付プロパティの値です。使用可能でない場合は、項目の [ToString](/dotnet/api/system.object.tostring?view=dotnet-uwp-10.0) メソッドによって返される値です。
>
> <_**x**_> は現在の項目の番号です。 <_**N**_> はグループ内の項目の総数です。

## <a name="create-a-winui-radiobuttons-group"></a>WinUI RadioButtons グループを作成する

`RadioButtons` コントロールでは、[ItemsControl](/uwp/api/windows.ui.xaml.controls.itemscontrol) に似たコンテンツ モデルが使用されます。 そのため、以下を行うことができます。

- 項目を [Items](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.items) コレクションに直接追加するか、データをその [ItemsSource](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.itemssource) プロパティにバインドすることにより事前設定する。
- [SelectedIndex](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selectedindex) または [SelectedItem](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selecteditem) プロパティを使用して、選択されているオプションの取得および設定を行う。
- [SelectionChanged](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selectionchanged) イベントを処理して、オプションが選択されたときにアクションを実行する。

> [!TIP]
> このドキュメントでは、XAML で **muxc** エイリアスを使って、プロジェクトに含めた Windows UI Library API を表します。 [Page](/uwp/api/windows.ui.xaml.controls.page) 要素にこれを追加しました。`xmlns:muxc="using:Microsoft.UI.Xaml.Controls"`
>
>コードビハインドでは、C# でも **muxc** エイリアスを使って、プロジェクトに含めた Windows UI Library API を表します。 この **using** ステートメントは、ファイルの先頭に追加されています。`using muxc = Microsoft.UI.Xaml.Controls;`

以下では、3 つのオプションを使用して単純な `RadioButtons` コントロールを宣言します。 [Header](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.header) プロパティはグループにラベルを付けるために設定されており、`SelectedIndex` プロパティは既定のオプションを指定するために設定されています。

```xaml
<muxc:RadioButtons Header="Background color"
                   SelectedIndex="0"
                   SelectionChanged="BackgroundColor_SelectionChanged">
    <x:String>Red</x:String>
    <x:String>Green</x:String>
    <x:String>Blue</x:String>
</muxc:RadioButtons>
```

結果は次のようになります。

:::image type="content" source="images/radiobuttons-default-group.png" alt-text="3 つのラジオ ボタンからなるグループ":::

ユーザーがオプションを選択したときにアクションを実行するには、[SelectionChanged](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selectionchanged) イベントを処理します。 ここでは、"ExampleBorder" という名前が付けられた [Border](/uwp/api/windows.ui.xaml.controls.border) 要素 (`<Border x:Name="ExampleBorder" Width="100" Height="100"/>`) の背景色を変更します。

```csharp
private void BackgroundColor_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    if (ExampleBorder != null && sender is muxc.RadioButtons rb)
    {
        string colorName = rb.SelectedItem as string;
        switch (colorName)
        {
            case "Red":
                ExampleBorder.Background = new SolidColorBrush(Colors.Red);
                break;
            case "Green":
                ExampleBorder.Background = new SolidColorBrush(Colors.Green);
                break;
            case "Blue":
                ExampleBorder.Background = new SolidColorBrush(Colors.Blue);
                break;
        }
    }
}
```

> [!TIP]
> 選択されている項目は、[SelectionChangedEventArgs.AddedItems](/uwp/api/windows.ui.xaml.controls.selectionchangedeventargs.addeditems) プロパティから取得することもできます。 選択されている項目はインデックス 0 に 1 つだけ存在するので、`string colorName = e.AddedItems[0] as string;` のようにして、その選択されている項目を取得できます。

### <a name="selection-states"></a>選択の状態

ラジオ ボタンにはオンまたはオフの 2 つの状態があります。 `RadioButtons` グループ内のオプションが選択されている場合、その値は [SelectedItem](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selecteditem) プロパティから、コレクション内のそのオプションの場所は [SelectedIndex](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selectedindex) プロパティから取得できます。 ラジオ ボタンは、ユーザーが同じグループ内の別のラジオ ボタンを選択するとオフにできますが、それをもう一度選択してもオフにすることはできません。 ただし、プログラムで `SelectedItem = null` または `SelectedIndex = -1` に設定すると、ラジオ ボタン グループをオフにすることができます。 (`SelectedIndex` を `Items` コレクションの範囲外の値に設定すると、選択されていない状態になります。)

### <a name="radiobuttons-content"></a>RadioButtons のコンテンツ

前の例では、`RadioButtons` コントロールに単純な文字列を事前設定しました。 このコントロールでラジオ ボタンが提供され、その文字列がそれぞれのラベルとして使用されました。

ただし、`RadioButtons` コントロールには任意のオブジェクトを事前設定できます。 通常は、このオブジェクトでテキスト ラベルとして使用できる文字列表現が指定されるようにします。 場合によっては、テキストよりも画像の方が適していることもあります。

以下では、コントロールを事前設定するために [SymbolIcon](/uwp/api/windows.ui.xaml.controls.symbolicon) 要素が使用されています。

```xaml
<muxc:RadioButtons Header="Choose the icon without an arrow">
    <SymbolIcon Symbol="Back"/>
    <SymbolIcon Symbol="Attach"/>
    <SymbolIcon Symbol="HangUp"/>
    <SymbolIcon Symbol="FullScreen"/>
</muxc:RadioButtons>
```

:::image type="content" source="images/radiobuttons-symbolicon.png" alt-text="シンボル アイコンが指定されたグループ ラジオ ボタン":::

個々の [RadioButton](/uwp/api/Windows.UI.Xaml.Controls.RadioButton) コントロールを使用して、`RadioButtons` の項目を事前設定することもできます。 このような特殊なケースについては後で説明します。 [RadioButtons グループ内の RadioButton コントロール]()に関するセクションを参照してください。

任意のオブジェクトを使用できる利点として、`RadioButtons` コントロールをデータ モデルのカスタム型にバインドできることが挙げられます。 次のセクションでこの点について説明します。

### <a name="data-binding"></a>データ バインディング

`RadioButtons` コントロールでは、[ItemsSource](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.itemssource) プロパティへのデータ バインディングがサポートされています。 コントロールをカスタム データ ソースにバインドする方法を次の例で示します。 この例の外観と機能は前述の背景色の例と同じですが、ここでは、カラー ブラシは `SelectionChanged` イベント ハンドラーで作成されるのではなく、データ モデルに格納されています。

```xaml
 <muxc:RadioButtons Header="Background color"
                    SelectedIndex="0"
                    SelectionChanged="BackgroundColor_SelectionChanged"
                    ItemsSource="{x:Bind colorOptionItems}"/>
```

```c#
public sealed partial class MainPage : Page
{
    // Custom data item.
    public class ColorOptionDataModel
    {
        public string Label { get; set; }
        public SolidColorBrush ColorBrush { get; set; }

        public override string ToString()
        {
            return Label;
        }
    }

    List<ColorOptionDataModel> colorOptionItems;

    public MainPage1()
    {
        this.InitializeComponent();

        colorOptionItems = new List<ColorOptionDataModel>();
        colorOptionItems.Add(new ColorOptionDataModel()
            { Label = "Red", ColorBrush = new SolidColorBrush(Colors.Red) });
        colorOptionItems.Add(new ColorOptionDataModel()
            { Label = "Green", ColorBrush = new SolidColorBrush(Colors.Green) });
        colorOptionItems.Add(new ColorOptionDataModel()
            { Label = "Blue", ColorBrush = new SolidColorBrush(Colors.Blue) });
    }

    private void BackgroundColor_SelectionChanged(object sender, SelectionChangedEventArgs e)
    {
        var option = e.AddedItems[0] as ColorOptionDataModel;
        ExampleBorder.Background = option?.ColorBrush;
    }
}
```

### <a name="radiobutton-controls-in-a-radiobuttons-group"></a>RadioButtons グループ内の RadioButton コントロール

個々の [RadioButton](/uwp/api/Windows.UI.Xaml.Controls.RadioButton) コントロールを使用して、`RadioButtons` の項目を事前設定できます。 これは、`AutomationProperties.Name` などの特定のプロパティにアクセスするために使用することがあります。また、`RadioButton` コードを既に使用していて、`RadioButtons` のレイアウトとナビゲーションを利用したい場合にも使用することが考えられます。

```xaml
<muxc:RadioButtons Header="Background color">
    <RadioButton Content="Red" Tag="red" AutomationProperties.Name="red"/>
    <RadioButton Content="Green" Tag="green" AutomationProperties.Name="green"/>
    <RadioButton Content="Blue" Tag="blue" AutomationProperties.Name="blue"/>
</muxc:RadioButtons>
```

`RadioButton` コントロールを `RadioButtons` グループ内で使用する場合、`RadioButtons` コントロールは `RadioButton` の表示方法を認識しているため、選択円が 2 つ表示されることはありません。

ただし、一部の動作に注意する必要があります。 状態やイベントの処理は、競合を回避するため、個々のコントロールで行うか `RadioButtons` で行うようにし、両方では行わないようにすることをお勧めします。

両方のコントロールの関連するイベントとプロパティを以下の表に示します。

|RadioButton  |RadioButtons  |
|---------|---------|
|[Checked](/uwp/api/windows.ui.xaml.controls.primitives.togglebutton.checked)、[Unchecked](/uwp/api/windows.ui.xaml.controls.primitives.togglebutton.unchecked)、[Click](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) |    [SelectionChanged](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selectionchanged) |
|[IsChecked](/uwp/api/windows.ui.xaml.controls.primitives.togglebutton.ischecked)  | [SelectedItem](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selecteditem)、[SelectedIndex](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selectedindex) |

`Checked` や `Unchecked` などの個々の `RadioButton` のイベントを処理し、`RadioButtons.SelectionChanged` イベントも処理すると、両方のイベントが発生します。 最初に `RadioButton` イベントが発生し、次に `RadioButtons.SelectionChanged` イベントが発生するため、競合が発生する可能性があります。

`IsChecked`、`SelectedItem`、`SelectedIndex` の各プロパティは同期されています。 1 つのプロパティを変更すると、他の 2 つが更新されます。

[RadioButton.GroupName](/uwp/api/windows.ui.xaml.controls.radiobutton.groupname) プロパティは無視されます。 グループは、`RadioButtons` コントロールによって作成されます。

### <a name="defining-multiple-columns"></a>複数列の定義

既定で、`RadioButtons` コントロールでは、ラジオ ボタンは垂直方向に 1 列に配置されます。 [MaxColumns](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.maxcolumns) プロパティを設定すると、このコントロールでラジオ ボタンが複数の列に配置されるようにすることができます。 (これを行うと、それらは列優先順に配置され、項目は上から下、左から右の順に配置されます。)

```xaml
<muxc:RadioButtons Header="Options" MaxColumns="3">
    <x:String>Item 1</x:String>
    <x:String>Item 2</x:String>
    <x:String>Item 3</x:String>
    <x:String>Item 4</x:String>
    <x:String>Item 5</x:String>
    <x:String>Item 6</x:String>
</muxc:RadioButtons>
```

:::image type="content" source="images/radiobuttons-multi-column.png" alt-text="3 列のグループのラジオ ボタン":::

> [!TIP]
> 項目を水平方向に 1 行に配置するには、`MaxColumns` がグループ内の項目数と同じになるように設定します。

## <a name="create-your-own-radiobutton-group"></a>独自の RadioButton グループを作成する

> [!Important]
> 古いバージョンの WinUI を使用している場合を除き、`RadioButton` 要素をグループ化するには WinUI の `RadioButtons` コントロールを使用することをお勧めします。

ラジオ ボタンは、グループで動作します。 個々の [RadioButton](/uwp/api/Windows.UI.Xaml.Controls.RadioButton) コントロールを、以下の 2 つの方法のどちらかでグループ化できます。

- 同じ親コンテナー内に追加します。
- 各ラジオ ボタンの [GroupName](/uwp/api/Windows.UI.Xaml.Controls.RadioButton.GroupName) プロパティを同じ値に設定します。

この例では、スタック パネルを同じにすることでラジオ ボタンの最初のグループが暗黙的にグループ化されます。 2 つ目のグループは、2 つのスタック パネルの間で分割されているので、`GroupName` を使用して、それらを 1 つのグループに明示的にグループ化します。

```xaml
<StackPanel>
    <StackPanel>
        <TextBlock Text="Background" Style="{ThemeResource BaseTextBlockStyle}"/>
        <!-- Group 1 - implicit grouping -->
        <StackPanel Orientation="Horizontal">
            <RadioButton Content="Green" Tag="green" Checked="BGRadioButton_Checked"/>
            <RadioButton Content="Yellow" Tag="yellow" Checked="BGRadioButton_Checked"/>
            <RadioButton Content="Blue" Tag="blue" Checked="BGRadioButton_Checked"/>
            <RadioButton Content="White" Tag="white" Checked="BGRadioButton_Checked"
                         IsChecked="True"/>
        </StackPanel>
    </StackPanel>

    <StackPanel>
        <TextBlock Text="BorderBrush" Style="{ThemeResource BaseTextBlockStyle}"/>
        <!-- Group 2 - grouped by GroupName -->
        <StackPanel Orientation="Horizontal">
            <StackPanel>
                <RadioButton Content="Green" Tag="green" GroupName="BorderBrush"
                             Checked="BorderRadioButton_Checked"/>
                <RadioButton Content="Yellow" Tag="yellow" GroupName="BorderBrush"
                             Checked="BorderRadioButton_Checked" IsChecked="True"/>
            </StackPanel>
            <StackPanel>
                <RadioButton Content="Blue" Tag="blue" GroupName="BorderBrush"
                             Checked="BorderRadioButton_Checked"/>
                <RadioButton Content="White" Tag="white"  GroupName="BorderBrush"
                             Checked="BorderRadioButton_Checked"/>
            </StackPanel>
        </StackPanel>
    </StackPanel>
    <Border x:Name="ExampleBorder"
            BorderBrush="#FFFFD700" Background="#FFFFFFFF"
            BorderThickness="10" Height="50" Margin="0,10"/>
</StackPanel>
```

```csharp
private void BGRadioButton_Checked(object sender, RoutedEventArgs e)
{
    RadioButton rb = sender as RadioButton;

    if (rb != null && ExampleBorder != null)
    {
        string colorName = rb.Tag.ToString();
        switch (colorName)
        {
            case "yellow":
                ExampleBorder.Background = new SolidColorBrush(Colors.Yellow);
                break;
            case "green":
                ExampleBorder.Background = new SolidColorBrush(Colors.Green);
                break;
            case "blue":
                ExampleBorder.Background = new SolidColorBrush(Colors.Blue);
                break;
            case "white":
                ExampleBorder.Background = new SolidColorBrush(Colors.White);
                break;
        }
    }
}

private void BorderRadioButton_Checked(object sender, RoutedEventArgs e)
{
    RadioButton rb = sender as RadioButton;

    if (rb != null && ExampleBorder != null)
    {
        string colorName = rb.Tag.ToString();
        switch (colorName)
        {
            case "yellow":
                ExampleBorder.BorderBrush = new SolidColorBrush(Colors.Gold);
                break;
            case "green":
                ExampleBorder.BorderBrush = new SolidColorBrush(Colors.DarkGreen);
                break;
            case "blue":
                ExampleBorder.BorderBrush = new SolidColorBrush(Colors.DarkBlue);
                break;
            case "white":
                ExampleBorder.BorderBrush = new SolidColorBrush(Colors.White);
                break;
        }
    }
}
```

`RadioButton` コントロールのこれらの 2 つのグループは、以下のように表示されます。

:::image type="content" source="images/radio-button-groups.png" alt-text="ラジオ ボタンの 2 つのグループ":::

### <a name="radio-button-states"></a>ラジオ ボタンの状態

ラジオ ボタンにはオンまたはオフの 2 つの状態があります。 ラジオ ボタンが選択されている場合、[IsChecked](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton.IsChecked) プロパティは `true` です。 ラジオ ボタンが選択されていない場合、`IsChecked` プロパティは `false` です。 ラジオ ボタンは、ユーザーが同じグループ内の別のラジオ ボタンを選択するとオフにできますが、それをもう一度選択してもオフにすることはできません。 ただし、プログラムで `IsChecked` プロパティを `false` に設定すると、ラジオ ボタンをオフにすることができます。

### <a name="visuals-to-consider"></a>表示に関する考慮事項

個々の `RadioButton` コントロールの既定の間隔は、`RadioButtons` グループによって指定される間隔とは異なります。 `RadioButtons` の間隔を個々の `RadioButton` コントロールに適用するには、以下に示すように `0,0,7,3` の `Margin` 値を使用します。

```xaml
<StackPanel>
    <StackPanel.Resources>
        <Style TargetType="RadioButton">
            <Setter Property="Margin" Value="0,0,7,3"/>
        </Style>
    </StackPanel.Resources>
    <TextBlock Text="Background"/>
    <RadioButton Content="Item 1"/>
    <RadioButton Content="Item 2"/>
    <RadioButton Content="Item 3"/>
</StackPanel>
```

以下の図に、グループ内のラジオ ボタンの推奨される間隔を示します。

:::image type="content" source="images/radiobutton-layout.png" alt-text="垂直方向に整列されたラジオ ボタンのセットを示す画像":::

:::image type="content" source="images/radiobutton-redline.png" alt-text="ラジオ ボタンの間隔のガイドラインを示す画像":::

> [!NOTE]
> WinUI RadioButtons コントロールを使用している場合、スペース、余白、向きは既に最適化されています。

## <a name="recommendations"></a>推奨事項

- 一連のラジオ ボタンの用途と現在の状態が明確に示されていることを確認します。
- ラジオ ボタンのテキスト ラベルは、1 行に制限します。
- テキスト ラベルが動的な場合は、ボタンのサイズが自動的にどのように変化し、周囲の視覚化にどのように影響するかを検討してください。
- ブランドのガイドラインで別の指示がない限り、既定のフォントを使います。
- 2 つの RadioButtons グループを並べて配置しないようにします。 2 つの RadioButtons グループが並んでいると、ユーザーはどのボタンがどのグループに属しているかがわかりにくくなります。

## <a name="get-the-sample-code"></a>サンプル コードの入手

- すべての XAML コントロールを対話形式で取得するには、[XAML コントロール ギャラリー サンプル](https://github.com/Microsoft/Xaml-Controls-Gallery)を参照してください。

## <a name="related-topics"></a>関連トピック

- [ボタン](buttons.md)
- [トグル スイッチ](toggles.md)
- [チェック ボックス](checkbox.md)
- [リストとコンボ ボックス](lists.md)
- [スライダー](slider.md)
- [RadioButtons クラス](/uwp/api/microsoft.ui.xaml.controls.radiobuttons)
- [RadioButton クラス](/uwp/api/windows.ui.xaml.controls.radiobutton)
