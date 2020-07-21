---
Description: ラジオ ボタンでは、ユーザーは 2 つ以上の選択肢から 1 つのオプションを選ぶことができます。
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
ms.openlocfilehash: 6705c314d9a70f8b6282841a7f8b1df76c6ef880
ms.sourcegitcommit: 6dd6d61c912daab2cc4defe5ba0cf717339f7765
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2020
ms.locfileid: "84978404"
---
# <a name="radio-buttons"></a>ラジオ ボタン

ラジオ ボタンは、相互排他的であるものの、関連している 2 つ以上のオプションのコレクションから 1 つのオプションをユーザーが選択できるようにするボタンです。 各オプションは、1 つのラジオ ボタンによって表されます。

既定の状態では、グループ内のラジオ ボタンはどれも選択されていません。 ただし、ラジオ ボタンのオプションがユーザーによって選択されると、そのグループをどれも選択されていない状態にユーザーが復元することはできません。

ラジオ ボタン グループのこの独特の動作が、[チェック ボックス](checkbox.md)とは異なる部分です。チェック ボックスでは、複数選択や選択解除がサポートされています。

![ラジオ ボタン](images/controls/radio-button.png)

**Windows UI ライブラリを入手する**

|  |  |
| - | - |
| ![WinUI ロゴ](images/winui-logo-64x64.png) | **RadioButtons** コントロールは、Windows アプリのための新しいコントロールと UI 機能を含む NuGet パッケージである、Windows UI ライブラリの一部として含まれています。 インストール手順などについて詳しくは、「[Windows UI Library (Windows UI ライブラリ)](https://docs.microsoft.com/uwp/toolkits/winui/)」をご覧ください。 |

> **Windows UI ライブラリ API:** [RadioButtons クラス](/uwp/api/microsoft.ui.xaml.controls.radiobuttons)、[SelectionChanged イベント](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selectionchanged)、[SelectedItem プロパティ](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selecteditem)、[SelectedIndex プロパティ](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selectedindex)
>
> **プラットフォーム API:** [RadioButton クラス](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RadioButton)、[Checked イベント](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton.Checked)、[IsChecked プロパティ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton.IsChecked)

## <a name="is-this-the-right-control"></a>これは適切なコントロールですか?

ユーザーが 2 つ以上の相互排他的なオプションの中から選択できるようにするには、ラジオ ボタンを使用します。

![ラジオ ボタンのグループ](images/radiobutton_basic.png)

ユーザーが選択するすべてのオプションを表示する必要がある場合は、ラジオ ボタンを使用します。 ラジオ ボタンでは、すべてのオプションが均等に強調されるため、オプションが必要以上ないしは望ましいレベルを超えて注目される可能性があります。 どのオプションにも均等にユーザーの注意を引く必要がある場合を除き、他のコントロールを使うことを検討してください。 たとえば、ほとんどの状況でほとんどのユーザーに既定のオプションが推奨されている場合は、代わりに[コンボ ボックス](combo-box.md)を使います。

![既定のオプションを強調するために使用されるドロップダウン リスト](images/combo_box_collapsed.png)

相互排他的なオプションが 2 つだけの場合は、それらを 1 つの[チェック ボックス](checkbox.md)か[トグル スイッチ](toggles.md)にまとめます。 たとえば、"I agree" と "I don't agree" という 2 つのラジオ ボタンではなく、"I agree" のチェック ボックスを使います。

![チェックボックスは二者択一を表すのに適した代替手段](images/radiobutton_vs_checkbox.png)

ユーザーが複数のオプションを選択できる場合は、[チェックボックス](checkbox.md) を使います。

![チェックボックスでは複数選択がサポートされる](images/checkbox2.png)

オプションが固定間隔の数値 (10, 20, 30) である場合は、[スライダー](slider.md) コントロールを使用します。

![階段値を選択するために使用されるスライダー](images/controls/slider.png)

オプションが 8 個より多い場合は、[コンボ ボックスかリスト ボックス](combo-box.md)を使用します。

![複数のオプションを表示するために使用されるリスト ボックス](images/combo_box_scroll.png)

> [!NOTE]
> オプションがアプリの現在のコンテキストに基づいて表示される場合や、その他の方法で動的に変化する場合は、単一選択の [リスト ボックス](combo-box.md#list-boxes) を使います。

## <a name="radiobuttons-behavior"></a>RadioButtons の動作

[RadioButton](/uwp/api/windows.ui.xaml.controls.radiobutton?view=winrt-19041) グループでは、キーボード アクセスとナビゲーションの動作が最適化されており、アクセシビリティ ユーザーもキーボード パワー ユーザーもオプションのリスト内をすばやく簡単に移動できます。

キーボード ショートカットとアクセシビリティの改善に加えて、RadioButton グループ内の個々のラジオ ボタンの既定の表示レイアウトも、それらの向き、間隔、余白の設定が自動化されて最適化されました。 [StackPanel](../layout/layout-panels.md#stackpanel) や [Grid](../layout/layout-panels.md#grid) などのよりプリミティブなグループ化コントロールを使用する場合にはこれらのプロパティを使用する必要がありましたが、その必要がなくなりました。

### <a name="navigating-a-radiobuttons-group"></a>RadioButtons グループ内の移動

RadioButtons コントロールでは、次の 2 つの状態がサポートされています。

- どれも選択またはチェックされていない RadioButton コントロールのリスト
- 1 つが既に選択またはチェックされた RadioButton コントロールのリスト

次の 2 つのセクションで、両方の場合のラジオ ボタンのフォーカス動作について説明します。

#### <a name="item-already-selected"></a>項目が既に選択されている場合

ラジオ ボタンが既に選択されている場合、ユーザーがリスト内にタブ キーで移動すると、選択されているラジオ ボタンがフォーカスされます。

|タブ フォーカスされていないリスト | タブで初期フォーカスされているリスト |
|:--:|:--:|
| ![タブ フォーカスされていないリスト](images/radiobutton-selected-item-no-tab-focus.png) | ![タブで初期フォーカスされているリスト](images/radiobutton-selected-item-tab-focus.png)|

#### <a name="no-item-selected"></a>項目が選択されていない場合

どのラジオ ボタンも選択されていない場合、リスト内の最初のラジオ ボタンがフォーカスされます。

> [!NOTE]
> 最初のタブ ナビゲーションでタブ フォーカスされた項目は、選択またはチェックされていません。

|タブ フォーカスされていないリスト | タブで初期フォーカスされているリスト|
|:--:|:--:|
| ![タブ フォーカスされていないリスト](images/radiobutton-no-selected-item-no-tab-focus.png) | ![タブで初期フォーカスされているリスト](images/radiobutton-no-selected-item-tab-focus.png)|

### <a name="keyboard-navigation"></a>キーボード ナビゲーション

ラジオ ボタンのオプションが 1 行または 1 列になっており、1 つの項目が既にタブ フォーカスされている場合、RadioButtons コントロール内の項目間の "内部ナビゲーション" を矢印キーで実行できます。 キーボード ナビゲーションの動作の詳細については、[キーボード操作のナビゲーション](../input/keyboard-interactions.md#navigation)に関するセクションを参照してください。

RadioButtons コントロールでは、オプションのリストが垂直方向に (排他的に) 配置されている場合、上下方向キーで項目間を移動できます。左右方向キーでは何も実行されません。 ただし、水平方向に (排他的に) 配置されているリストでは、左右方向キーでも上下方向キーでも、すべて同様に項目間を移動できます。

![1 列または 1 行の RadioButton グループ内のキーボード ナビゲーションの例](images/radiobutton-keyboard-navigation-single-column-row.png)<br/>
*1 列または 1 行の RadioButton グループ内のキーボード ナビゲーションの例*

#### <a name="navigating-within-multi-columnrow-layouts"></a>複数列または複数行のレイアウト内の移動

列優先順序 (項目が上から下、左から右に入力される) では、列内の最後の項目がフォーカスされている状況で下方向キーが押されると、フォーカスは次の列の最初の項目に移動します。 逆向きにも同様に動作します。列内の最初の項目がフォーカスされている状況で上方向キーが押されると、フォーカスは前の列の最後の項目に移動します。

![複数列または複数行の RadioButton グループ内のキーボード ナビゲーションの例](images/radiobutton-keyboard-navigation-multi-column-row.png)

行優先順序 (項目が左から右、上から下に入力される) では、行内の最後の項目がフォーカスされている状況で右方向キーが押されると、フォーカスは次の行の最初の項目に移動します。 逆向きにも同様に動作します。行内の最初の項目がフォーカスされている状況で左方向キーが押されると、フォーカスは前の行の最後の項目に移動します。

##### <a name="wrapping"></a>折り返し

RadioButtons グループは折り返されません。 これは、スクリーン リーダーを使用するときに、境界や、先頭と末尾がはっきりしなくなると、視覚障碍のあるユーザーがリスト内を移動しにくくなるためです。 RadioButtons コントロールでは、適切な数の項目が含まれるようにするため、列挙もサポートされていません (「[これは適切なコントロールですか?](#is-this-the-right-control)」を参照してください)。

## <a name="selection-follows-focus"></a>フォーカスに連動した選択

キーボードを使用して RadioButtons リスト内の項目間を移動する場合 (項目が既に選択されている場合)、フォーカスが 1 つの項目から次の項目に移動すると同時に、新しくフォーカスされた項目が選択またはチェックされ、前にフォーカスされていた項目の選択またはチェックが解除されます。

|キーボード ナビゲーション前 | キーボード ナビゲーション後|
|:--|:--|
| ![キーボード ナビゲーション前のフォーカスと選択の例](images/radiobutton-two-selected-before-keyboard-navigation.png)</br>*キーボード ナビゲーション前のフォーカスと選択の例* | ![キーボード ナビゲーション後のフォーカスと選択の例](images/radiobutton-three-selected-after-keyboard-navigation.png)<br/>*キーボード ナビゲーション後のフォーカスと選択の例。下矢印キーまたは右方向キーでフォーカスが "3" RadioButton に移動した結果、"3" が選択され、"2" の選択が解除されている。*

### <a name="navigating-with-xbox-gamepad-and-remote-control"></a>Xbox ゲームパッドやリモート コントロールを使用した移動

Xbox ゲームパッドやリモート コントロールを使用して RadioButtons コントロール内を移動する場合、"フォーカスに連動した選択" の動作は無効になるため、フォーカスされているラジオ ボタンを選択するには、A ボタンを押す必要があります。

## <a name="accessibility-behavior"></a>アクセシビリティの動作

次の表に、ナレーターによるラジオ ボタン グループの処理方法と、読み上げられる内容の詳細を示します (これは、ユーザーが設定したナレーターの詳細設定によって異なります)。

| 初期フォーカス | 選択された項目にフォーカスが移動する場合 |
|:--|:--|
| "グループ名" RadioButton コレクションの、N の中の x が選択されています | RadioButton "名前" が選択されています。N の中の x です |
|"グループ名" RadioButton コレクションは、どれも選択されていません| RadioButton "名前" は選択されていません。N の中の x です <br> *(Shift キーを押しながら方向キーを使って移動する場合、フォーカスに選択が連動しないことを示します)* |

## <a name="examples"></a>例

<table>
<th align="left">XAML コントロール ギャラリー<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML コントロール ギャラリー</strong> アプリがインストールされている場合、こちらをクリックして<a href="xamlcontrolsgallery:/item/RadioButton">アプリを開き、RadioButton の動作を確認</a>してください。</p>
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

次に、オプションが 3 つある基本的な RadioButtons コントロールを宣言します。

```xaml
<RadioButtons Header="App Mode" SelectedIndex="2">
    <RadioButton>Item 1</RadioButton>
    <RadioButton>Item 2</RadioButton>
    <RadioButton>Item 3</RadioButton>
</RadioButtons>
```

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

![2 列のグループのラジオ ボタン](images/radiobutton-multi-columns.png)

### <a name="data-binding"></a>データ バインディング

RadioButtons コントロールでは、次のスニペットに示すように、その [ItemsSource](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.itemssource) プロパティを使用したデータ バインディングがサポートされています。

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

## <a name="create-your-own-radio-button-group"></a>独自のラジオ ボタン グループを作成する

> [!Important]
> RadioButton の要素をグループ化する場合は、WinUI RadioButtons コントロールを使用することをお勧めします (以前のバージョンの WinUI を使用している場合を除く)。

ラジオ ボタンは、グループで動作します。 ラジオ ボタン コントロールをグループ化する 2 つの方法があります。

- 同一の親コンテナー内にそれらを配置する
- 各ラジオ ボタンの [GroupName](/uwp/api/Windows.UI.Xaml.Controls.RadioButton.GroupName) プロパティを同じ値に設定する

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

このラジオ ボタン グループがどのようにレンダリングされるかを次の図に示します。

![ラジオ ボタンの 2 つのグループ](images/radio-button-groups.png)

## <a name="radio-button-states"></a>ラジオ ボタンの状態

ラジオ ボタンには、チェックされているかチェックが解除されているかの 2 つの状態があります。 ラジオ ボタンを選択すると、[IsChecked](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton.IsChecked) プロパティは **true** になります。 ラジオ ボタンのチェックが解除されると、**IsChecked** プロパティは **false** になります。 ラジオ ボタンは、同じグループ内の別のラジオ ボタンをクリックするとチェックを解除できますが、そのボタン自体をもう一度クリックしてもチェックを解除することはできません。 ただし、プログラムで IsChecked プロパティを **false** に設定することでラジオ ボタンのチェックを解除することはできます。

## <a name="recommendations"></a>推奨事項

- 一連のラジオ ボタンの用途と現在の状態が明確に表示されていることを確認します。
- ラジオ ボタンのテキスト コンテンツは、1 行に収まるように作成します。
- テキスト コンテンツが動的な場合、ボタンのサイズがどのように変わり、周囲の視覚効果にどのような影響が生じるかを検討してください。
- ブランドのガイドラインで別の指示がない限り、既定のフォントを使います。
- 2 つのラジオ ボタン グループを並べて配置しないようにします。 2 つのラジオ ボタン グループが並んでいると、どのボタンがどのグループに属しているかがわかりにくくなります。

### <a name="visuals-to-consider"></a>表示に関する考慮事項

RadioButton グループのラジオ ボタンをレイアウトするのに最適な方法を次の図に示します。

![一連のラジオ ボタン](images/radiobutton-layout.png)

![ラジオ ボタンの間隔ガイドライン](images/radiobutton-redline.png)

> [!NOTE]
> WinUI RadioButtons コントロールを使用している場合、スペース、余白、向きは既に最適化されています。

## <a name="get-the-sample-code"></a>サンプル コードの入手

- [XAML コントロール ギャラリー サンプル](https://github.com/Microsoft/Xaml-Controls-Gallery) - 対話形式で XAML コントロールのすべてを参照できます。

## <a name="related-topics"></a>関連トピック

### <a name="for-designers"></a>デザイナー向け

- [ボタン](buttons.md)
- [トグル スイッチ](toggles.md)
- [チェック ボックス](checkbox.md)
- [リストとコンボ ボックス](lists.md)
- [スライダー](slider.md)

### <a name="for-developers-xaml"></a>開発者向け (XAML)

- [RadioButton クラス](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.radiobutton)
