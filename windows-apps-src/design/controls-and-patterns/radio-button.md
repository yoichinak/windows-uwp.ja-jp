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
ms.openlocfilehash: 7d09eaefff193a8283fd4bad68528b8976e0b63b
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89169786"
---
# <a name="radio-buttons"></a>ラジオ ボタン

ラジオ ボタン (オプション ボタンとも呼ばれます) は、相互排他的であるものの、関連している 2 つ以上のオプションのコレクションから 1 つのオプションをユーザーが選択できるようにするボタンです。 各オプションは、1 つのラジオ ボタンによって表されます。

既定の状態では、RadioButtons グループ内のラジオ ボタンはどれも選択されていません。 つまり、すべてのオプション ボタンがオフになっています。 ただし、ラジオ ボタンが選択された後では、グループをオフ状態に戻すことはできません。

RadioButtons グループのこの独特の動作が、複数選択や選択の解除 (クリア) がサポートされている[チェック ボックス](checkbox.md)とは異なる点です。

![1 つのラジオ ボタンが選択されている RadioButtons グループの例](images/controls/radio-button.png)

## <a name="get-the-windows-ui-library"></a>Windows UI ライブラリを入手する

| &nbsp; | &nbsp; |
| - | - |
| ![WinUI ロゴ](images/winui-logo-64x64.png) | RadioButtons コントロールは、Windows アプリのための新しいコントロールと UI 機能を含む NuGet パッケージである、Windows UI ライブラリの一部として含まれています。 インストール手順などについて詳しくは、[Windows UI ライブラリ](/uwp/toolkits/winui/)に関する記事を参照してください。 |

**Windows UI ライブラリ API**: 
* [RadioButtons クラス](/uwp/api/microsoft.ui.xaml.controls.radiobuttons)
* [SelectionChanged イベント](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selectionchanged)
* [SelectedItem プロパティ](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selecteditem)
* [SelectedIndex プロパティ](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selectedindex)

**プラットフォーム API**: 
* [RadioButton クラス](/uwp/api/Windows.UI.Xaml.Controls.RadioButton)
* [Checked イベント](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton.Checked)
* [IsChecked プロパティ](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton.IsChecked)

## <a name="is-this-the-right-control"></a>これは適切なコントロールですか?

ユーザーが 2 つ以上の相互排他的なオプションから選択できるようにするには、ラジオ ボタンを使用します。

![1 つのラジオ ボタンが選択されている RadioButtons グループ](images/radiobutton_basic.png)

選択する前にユーザーがすべてのオプションを見る必要がある場合は、ラジオ ボタンを使用します。 ラジオ ボタンでは、すべてのオプションが同じように強調されます。これは、一部のオプションが必要以上に、または望ましいレベルを超えて、注目される可能性があることを意味します。 

どのオプションも均等に注意を引く場合を除き、他のコントロールを使うことを検討してください。 たとえば、ほとんどのユーザーおよびほとんどの状況に対して 1 つの最適なオプションを推奨するには、[コンボ ボックス](combo-box.md)を使用して、その最適なオプションを既定のオプションとして表示します。

![既定のオプションが表示されているコンボ ボックス](images/combo_box_collapsed.png)

オプションが 2 つだけで、それらが相互に排他的である場合は、それらを 1 つの[チェック ボックス](checkbox.md)または[トグル スイッチ](toggles.md) コントロールにまとめます。 たとえば、"同意する" と "同意しない" という 2 つのラジオ ボタンではなく、"同意する" という 1 つのチェック ボックスを使用します。

![チェック ボックスは二者択一を表すのに適した代替手段である](images/radiobutton_vs_checkbox.png)

ユーザーが複数のオプションを選択できる場合は、[チェック ボックス](checkbox.md)を使用します。

![チェック ボックスでは複数選択がサポートされる](images/checkbox2.png)

ユーザーのオプションがある値の範囲に限定される場合は (例: *10、20、30、... 100*)、[スライダー](slider.md) コントロールを使用します。

![ある値の範囲内の 1 つの値が表示されているスライダー コントロール](images/controls/slider.png)

8 つ以上のオプションがある場合は、[コンボ ボックス](combo-box.md)を使用します。

![複数のオプションが表示されているリスト ボックス](images/combo_box_scroll.png)

> [!NOTE]
> 使用できるオプションが、アプリの現在のコンテキストに基づく場合、またはそれ以外の理由で非常に動的な場合は、リスト コントロールを使用します。

## <a name="radiobuttons-behavior"></a>RadioButtons の動作

キーボード アクセスおよびナビゲーション動作は、[RadioButton クラス](/uwp/api/windows.ui.xaml.controls.radiobutton?view=winrt-19041)で最適化されています。 これらの機能強化は、アクセシビリティと、キーボード パワー ユーザーによるオプションの一覧のすばやく簡単な移動の両方に役立ちます。

これらの機能強化に加えて、RadioButtons グループ内の個々のラジオ ボタンの既定の表示レイアウトが、自動化な向き、間隔、余白の設定によって最適化されています。 この最適化により、[StackPanel](../layout/layout-panels.md#stackpanel) や [Grid](../layout/layout-panels.md#grid) などのよりプリミティブなグループ化コントロールを使用する場合にはこれらのプロパティを使用する必要がありましたが、その必要がなくなりました。

### <a name="navigating-a-radiobuttons-group"></a>RadioButtons グループ内の移動

RadioButtons コントロールでは、次の 2 つの状態がサポートされています。

- どのラジオ ボタンも選択されていない
- 1 つのラジオ ボタンが選択されている

次の 2 つのセクションで、両方の場合のラジオ ボタンのフォーカス動作について説明します。

#### <a name="no-radio-button-is-selected"></a>どのラジオ ボタンも選択されていない

どのラジオ ボタンも選択されていない場合、リスト内の最初のラジオ ボタンがフォーカスされます。

> [!NOTE]
> 最初のタブ ナビゲーションからタブ フォーカスを受け取る項目が、選択されていません。

|タブ フォーカスされていないリスト | タブで初期フォーカスされているリスト|
|:--:|:--:|
| ![タブ フォーカスされていないリスト](images/radiobutton-no-selected-item-no-tab-focus.png) | ![タブで初期フォーカスされているリスト](images/radiobutton-no-selected-item-tab-focus.png)|

#### <a name="one-radio-button-is-selected"></a>1 つのラジオ ボタンが選択されている

ラジオ ボタンが選択されていて、ユーザーがリスト内にタブ移動すると、選択されているラジオ ボタンにフォーカスが設定されます。

|タブ フォーカスされていないリスト | タブで初期フォーカスされているリスト |
|:--:|:--:|
| ![タブ フォーカスされていないリスト](images/radiobutton-selected-item-no-tab-focus.png) | ![タブで初期フォーカスされているリスト](images/radiobutton-selected-item-tab-focus.png)|


### <a name="keyboard-navigation"></a>キーボード ナビゲーション

ラジオ ボタンのオプションが 1 行または 1 列になっており、1 つの項目に既にタブ フォーカスが設定されている場合、ユーザーは RadioButtons コントロール内の項目間の "内部ナビゲーション" に方向キーを使用できます。 キーボード ナビゲーションの動作の詳細については、[キーボード操作のナビゲーション](../input/keyboard-interactions.md#navigation)に関するセクションを参照してください。

RadioButtons コントロールで、オプションのリストが垂直方向だけに配置されている場合、上下方向キーでは項目間を移動できますが、左右方向キーでは何も行われません。 一方、水平方向だけに配置されているリストでは、左右方向キーと上下方向キーのすべてで、同じように項目間を移動できます。

![1 列または 1 行の RadioButtons グループでのキーボード ナビゲーションの例](images/radiobutton-keyboard-navigation-single-column-row.png)<br/>
*1 列または 1 行の RadioButtons グループでのキーボード ナビゲーションの例*

#### <a name="navigating-within-multi-column-or-multi-row-layouts"></a>複数列または複数行のレイアウト内の移動

列優先順序では、フォーカスは上から下、左から右に移動します。 列の最後の項目にフォーカスが設定されている状況で下方向キーを押すと、フォーカスは次の列の最初の項目に移動します。 これと同じ動作が逆の順序で発生します。フォーカスが列の最初の項目に設定されているときに上方向キーを押すと、フォーカスは前の列の最後の項目に移動します。

![複数列または複数行の RadioButtons グループ内のキーボード ナビゲーションの例](images/radiobutton-keyboard-navigation-multi-column-row.png)

行優先順序 (項目が左から右、上から下に入力される) では、行内の最後の項目がフォーカスされている状況で右方向キーが押されると、フォーカスは次の行の最初の項目に移動します。 これと同じ動作が逆の順序で発生します。フォーカスが行の最初の項目に設定されているときに左方向キーを押すと、フォーカスは前の行の最後の項目に移動します。

詳細については、「[キーボード操作](../input/keyboard-interactions.md#wrapping-homogeneous-list-and-grid-view-items)」を参照してください。

##### <a name="wrapping"></a>折り返し

RadioButtons グループは折り返されません。 これは、ユーザーがスクリーン リーダーを使用していて、境界や、先頭と末尾がはっきりしなくなったときに、視覚障碍のあるユーザーがリスト内を移動しにくくなるためです。 また、RadioButtons コントロールでは、コントロールに適切な数の項目が含まれることが意図されているため、列挙もサポートされていません (「[これは適切なコントロールですか?](#is-this-the-right-control)」を参照してください)。

## <a name="selection-follows-focus"></a>フォーカスに連動した選択

ユーザーがキーボードを使用して RadioButtons リスト内の項目間を移動する場合 (項目が既に選択されている場合)、フォーカスが 1 つの項目から次の項目に移動すると、新しくフォーカスが設定された項目はオンになり、前にフォーカスが設定されていた項目はオフになります。

|キーボード ナビゲーション前 | キーボード ナビゲーション後|
|:--|:--|
| ![キーボード ナビゲーション前のフォーカスと選択の例](images/radiobutton-two-selected-before-keyboard-navigation.png)</br>*キーボード ナビゲーション前のフォーカスと選択の例* | ![キーボード ナビゲーション後のフォーカスと選択の例](images/radiobutton-three-selected-after-keyboard-navigation.png)<br/>*キーボード ナビゲーション後のフォーカスと選択の例。下方向キーまたは右方向キーでフォーカスをラジオ ボタン 3 に移動すると、それが選択され、ラジオ ボタン 2 はオフになる* |

### <a name="navigating-with-xbox-gamepad-and-remote-control"></a>Xbox ゲームパッドやリモート コントロールを使用した移動

ユーザーが Xbox のゲームパッドやリモート コントロールを使用してラジオ ボタン間を移動している場合、"フォーカスに連動した選択" の動作は無効になり、フォーカスが設定されているラジオ ボタンを選択するには、ユーザーは "A" ボタンを押す必要があります。

## <a name="accessibility-behavior"></a>アクセシビリティの動作

次の表では、ナレーターによる RadioButtons グループの処理方法と、読み上げられる内容について説明します。 この動作は、ユーザーによるナレーターの詳細設定によって異なります。

| 初期フォーカス | 選択された項目にフォーカスが移動する場合 |
|:--|:--|
| "グループ名" RadioButton コレクションにフォーカスがあり、N 個の項目のうちの項目 x が選択されています | RadioButton "名前" が選択されている場合、項目 x にフォーカスが設定されます。 |
| "グループ名" RadioButton コレクションにフォーカスがあり、項目は選択されていません| RadioButton "名前" が選択されていない場合、項目 x にフォーカスが設定されます。 <br> ユーザーが Shift キーを押しながら方向キーを使用した場合、フォーカスに連動して選択されません。 |

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

## <a name="using-the-winui-radiobuttons-control"></a>WinUI RadioButtons コントロールの使用

[WinUI](https://github.com/microsoft/microsoft-ui-xaml) を使用している場合は、[RadioButtons](/uwp/api/microsoft.ui.xaml.controls.radiobuttons) コントロールを使用することをお勧めします。

RadioButtons コントロールは簡単にセットアップして使用でき、キーボードやナレーターで期待どおりの適切な動作を実現できます。

次のコードでは、オプションが 3 つある基本的な RadioButtons コントロールが宣言されています。

```xaml
<RadioButtons Header="App Mode" SelectedIndex="2">
    <RadioButton>Item 1</RadioButton>
    <RadioButton>Item 2</RadioButton>
    <RadioButton>Item 3</RadioButton>
</RadioButtons>
```
結果は次の図のようになります。

![ラジオ ボタンの 2 つのグループ](images/default-radiobutton-group.png)

### <a name="defining-multiple-columns"></a>複数列の定義

[MaxColumns プロパティ](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.maxcolumns)を指定すると、複数列の RadioButtons コントロールを宣言できます。

```xaml
<muxc:RadioButtons Header="App Mode" MaxColumns="3">
    <x:String>Column 1</x:String>
    <x:String>Column 2</x:String>
    <x:String>Column 3</x:String>
    <x:String>Column 1</x:String>
    <x:String>Column 2</x:String>
    <x:String>Column 3</x:String>
</muxc:RadioButtons>
```

![3 列のグループのラジオ ボタン](images/radiobutton-multi-columns.png)

### <a name="data-binding"></a>データ バインディング

RadioButtons コントロールでは、次のスニペットに示すように、その [ItemsSource](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.itemssource) プロパティを使用するデータ バインディングがサポートされています。

```xaml
<RadioButtons Header="App Mode" ItemsSource="{x:Bind radioButtonItems}" />
```

```c#
public sealed partial class MainPage : Page
{
    public class OptionDataModel
    {
        public string Label;
        public override string ToString()
        {
            return Label;
        }
    }

    List<OptionDataModel> radioButtonItems;

    public MainPage()
    {
        this.InitializeComponent();

        radioButtonItems = new List<OptionDataModel>();
        radioButtonItems.Add(new OptionDataModel() { label = "Item 1" });
        radioButtonItems.Add(new OptionDataModel() { label = "Item 2" });
        radioButtonItems.Add(new OptionDataModel() { label = "Item 3" });
    }
}
```

## <a name="create-your-own-radiobuttons-group"></a>独自の RadioButtons グループを作成する

> [!Important]
> 古いバージョンの WinUI を使用している場合を除き、RadioButton 要素をグループ化するには WinUI の RadioButtons コントロールを使用することをお勧めします。

ラジオ ボタンは、グループで動作します。 ラジオ ボタンは、次の 2 つの方法のいずれかでグループ化できます。

- 同じ親コンテナー内に追加します。
- 各ラジオ ボタンの [GroupName](/uwp/api/Windows.UI.Xaml.Controls.RadioButton.GroupName) プロパティを同じ値に設定します。

この例では、スタック パネルを同じにすることでラジオ ボタンの最初のグループが暗黙的にグループ化されます。 2 つ目のグループは、2 つのスタック パネルの間で分割されているので、GroupName で明示的にグループ化されます。

```xaml
<StackPanel>
    <StackPanel>
        <TextBlock Text="Background" Style="{ThemeResource BaseTextBlockStyle}"/>
        <StackPanel Orientation="Horizontal">
            <RadioButton Content="Green" Tag="Green" Checked="BGRadioButton_Checked"/>
            <RadioButton Content="Yellow" Tag="Yellow" Checked="BGRadioButton_Checked"/>
            <RadioButton Content="Blue" Tag="Blue" Checked="BGRadioButton_Checked"/>
            <RadioButton Content="White" Tag="White" Checked="BGRadioButton_Checked" IsChecked="True"/>
        </StackPanel>
    </StackPanel>
    <StackPanel>
        <TextBlock Text="BorderBrush" Style="{ThemeResource BaseTextBlockStyle}"/>
        <StackPanel Orientation="Horizontal">
            <StackPanel>
                <RadioButton Content="Green" GroupName="BorderBrush" Tag="Green" Checked="BorderRadioButton_Checked"/>
                <RadioButton Content="Yellow" GroupName="BorderBrush" Tag="Yellow" Checked="BorderRadioButton_Checked" IsChecked="True"/>
            </StackPanel>
            <StackPanel>
                <RadioButton Content="Blue" GroupName="BorderBrush" Tag="Blue" Checked="BorderRadioButton_Checked"/>
                <RadioButton Content="White" GroupName="BorderBrush" Tag="White"  Checked="BorderRadioButton_Checked"/>
            </StackPanel>
        </StackPanel>
    </StackPanel>
    <Border x:Name="BorderExample1" BorderThickness="10" BorderBrush="#FFFFD700" Background="#FFFFFFFF" Height="50" Margin="0,10,0,10"/>
</StackPanel>
```

```csharp
private void BGRadioButton_Checked(object sender, RoutedEventArgs e)
{
    RadioButton rb = sender as RadioButton;

    if (rb != null && BorderExample1 != null)
    {
        string colorName = rb.Tag.ToString();
        switch (colorName)
        {
            case "Yellow":
                BorderExample1.Background = new SolidColorBrush(Colors.Yellow);
                break;
            case "Green":
                BorderExample1.Background = new SolidColorBrush(Colors.Green);
                break;
            case "Blue":
                BorderExample1.Background = new SolidColorBrush(Colors.Blue);
                break;
            case "White":
                BorderExample1.Background = new SolidColorBrush(Colors.White);
                break;
        }
    }
}

private void BorderRadioButton_Checked(object sender, RoutedEventArgs e)
{
    RadioButton rb = sender as RadioButton;

    if (rb != null && BorderExample1 != null)
    {
        string colorName = rb.Tag.ToString();
        switch (colorName)
        {
            case "Yellow":
                BorderExample1.BorderBrush = new SolidColorBrush(Colors.Gold);
                break;
            case "Green":
                BorderExample1.BorderBrush = new SolidColorBrush(Colors.DarkGreen);
                break;
            case "Blue":
                BorderExample1.BorderBrush = new SolidColorBrush(Colors.DarkBlue);
                break;
            case "White":
                BorderExample1.BorderBrush = new SolidColorBrush(Colors.White);
                break;
        }
    }
}
```

この RadioButtons グループがどのようにレンダリングされるかを次の図に示します。

![ラジオ ボタンの 2 つのグループ](images/radio-button-groups.png)

## <a name="radio-button-states"></a>ラジオ ボタンの状態

ラジオ ボタンにはオンまたはオフの 2 つの状態があります。 ラジオ ボタンをオンにすると、[IsChecked プロパティ](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton.IsChecked)が `true` になります。 ラジオ ボタンをオフにすると、IsChecked プロパティは `false` になります。 ラジオ ボタンは、ユーザーが同じグループ内の別のラジオ ボタンを選択するとオフにできますが、それをもう一度選択してもオフにすることはできません。 ただし、プログラムで IsChecked プロパティを `false` に設定してラジオ ボタンをオフにすることができます。

## <a name="recommendations"></a>推奨事項

- 一連のラジオ ボタンの用途と現在の状態が明確に示されていることを確認します。
- ラジオ ボタンのテキスト ラベルは、1 行に制限します。
- テキスト ラベルが動的な場合は、ボタンのサイズが自動的にどのように変化し、周囲の視覚化にどのように影響するかを検討してください。
- ブランドのガイドラインで別の指示がない限り、既定のフォントを使います。
- 2 つの RadioButtons グループを並べて配置しないようにします。 2 つの RadioButtons グループが並んでいると、ユーザーはどのボタンがどのグループに属しているかがわかりにくくなります。

### <a name="visuals-to-consider"></a>表示に関する考慮事項

RadioButtons グループのラジオ ボタンを配置するのに最適な方法を次の図に示します。

![垂直方向に整列されたラジオ ボタンのセットを示す画像](images/radiobutton-layout.png)

![ラジオ ボタンの間隔のガイドラインを示す画像](images/radiobutton-redline.png)

> [!NOTE]
> WinUI RadioButtons コントロールを使用している場合、スペース、余白、向きは既に最適化されています。

## <a name="get-the-sample-code"></a>サンプル コードの入手

- すべての XAML コントロールを対話形式で取得するには、[XAML コントロール ギャラリー サンプル](https://github.com/Microsoft/Xaml-Controls-Gallery)を参照してください。 

## <a name="related-topics"></a>関連トピック

### <a name="for-designers"></a>デザイナー向け

- [ボタン](buttons.md)
- [トグル スイッチ](toggles.md)
- [チェック ボックス](checkbox.md)
- [リストとコンボ ボックス](lists.md)
- [スライダー](slider.md)

### <a name="for-developers-xaml"></a>開発者向け (XAML)

- [RadioButton クラス](/uwp/api/windows.ui.xaml.controls.radiobutton)