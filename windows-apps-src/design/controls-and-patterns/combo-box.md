---
Description: ユーザーが入力するときに、検索候補を表示するテキスト入力ボックスです。
title: コンボ ボックス (ドロップダウン リスト)
label: Combo box
template: detail.hbs
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: ''
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: 297b907191dfa07084e5e4ada0e3468733e47090
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/13/2019
ms.locfileid: "66363129"
---
# <a name="combo-box"></a>コンボ ボックス

ユーザーが選択できる項目の一覧を提供するには、コンボ ボックス (ドロップダウン リストとも呼ばれる) を使用します。 コンボ ボックスは、最初はコンパクトな状態で、展開すると選択可能な項目の一覧が表示されます。

コンボ ボックスを閉じると、現在の選択が表示されるか、選択された項目がない場合は空です。 ユーザーがコンボ ボックスを展開すると、選択可能な項目の一覧が表示されます。

> **重要な API**:[ComboBox クラス](/uwp/api/Windows.UI.Xaml.Controls.ComboBox)、[IsEditable プロパティ](/uwp/api/windows.ui.xaml.controls.combobox.iseditable)、[Text プロパティ](/uwp/api/Windows.UI.Xaml.Controls.ComboBox)、[TextSubmitted イベント](/uwp/api/Windows.UI.Xaml.Controls.ComboBox)

ヘッダーが表示されたコンパクトな状態のコンボ ボックス。

![コンパクトな状態のドロップダウン リストの例](images/combo_box_collapsed.png)

## <a name="is-this-the-right-control"></a>適切なコントロールの選択

- 1 行のテキストで十分に表すことができる項目のセットから、ユーザーが単一の値を選ぶことができるようにするには、ドロップダウン リストを使います。
- 複数のテキスト行や画像が含まれる項目を表示するには、コンボ ボックスではなくリスト ビューまたはグリッド ビューを使います。
- 項目が 5 個より少ない場合は、[ラジオ ボタン](radio-button.md) (1 つの項目だけを選ぶことができる場合)、または[チェック ボックス](checkbox.md) (複数の項目を選ぶことができる場合) の使用を検討します。
- 選択項目がアプリのフローにおいて二次的な重要性しか持たない場合は、コンボ ボックスを使います。 多くの状況でほとんどのユーザーに対して既定のオプションが推奨されている場合、リスト ビューを使ってすべての項目を表示すると、既定以外のオプションに必要以上の注意を引いてしまう可能性があります。 コンボ ボックスを使うことで、領域を節約し、無駄な情報を最小限にすることができます。

## <a name="examples"></a>例

<table>
<th align="left">XAML コントロール ギャラリー<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML コントロール ギャラリー</strong> アプリがインストールされている場合、こちらをクリックして<a href="xamlcontrolsgallery:/item/ComboBox">アプリを開き、ComboBox の動作を確認</a>してください。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML コントロール ギャラリー アプリを入手する (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">ソース コード (GitHub) を入手する</a></li>
    </ul>
</td>
</tr>
</table>

コンパクトな状態のコンボ ボックスには、ヘッダーを表示できます。

![コンパクトな状態のドロップダウン リストの例](images/combo_box_collapsed.png)

コンボ ボックスは、長い文字列の幅をサポートするために拡張できますが、読みづらくなるような長すぎる文字列は使わないでください。

![長い文字列のドロップダウン リストの例](images/combo_box_listitemstate.png)

コンボ ボックス内のコレクションが一定の長さに達すると、対応できるようにスクロール バーが表示されます。 リスト内の項目は論理的にグループ化します。

![ドロップダウン リストに表示されたスクロール バーの例](images/combo_box_scroll.png)

## <a name="create-a-combo-box"></a>コンボ ボックスを作成する

コンボ ボックスを設定するには、オブジェクトを直接 [Items](/uwp/api/windows.ui.xaml.controls.itemscontrol.items) コレクションに追加するか、データ ソースに [ItemsSource](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) プロパティをバインドします。 ComboBox に追加した項目は、[ComboBoxItem](/uwp/api/windows.ui.xaml.controls.comboboxitem) コンテナーにラップされます。

XAML で追加した項目を含むシンプルなコンボ ボックスを次に示します。

```xaml
<ComboBox Header="Colors" PlaceholderText="Pick a color" Width="200">
    <x:String>Blue</x:String>
    <x:String>Green</x:String>
    <x:String>Red</x:String>
    <x:String>Yellow</x:String>
</ComboBox>
```

次の例では、FontFamily オブジェクトのコレクションへのコンボ ボックスのバインディングを示します。

```xaml
<ComboBox x:Name="FontsCombo" Header="Fonts" Height="44" Width="296"
          ItemsSource="{x:Bind fonts}" DisplayMemberPath="Source"/>
```

```csharp
ObservableCollection<FontFamily> fonts = new ObservableCollection<FontFamily>();

public MainPage()
{
    this.InitializeComponent();
    fonts.Add(new FontFamily("Arial"));
    fonts.Add(new FontFamily("Courier New"));
    fonts.Add(new FontFamily("Times New Roman"));
}
```

### <a name="item-selection"></a>項目の選択

ListView や GridView のように、ComboBox は [Selector](/uwp/api/windows.ui.xaml.controls.primitives.selector) から派生するため、標準と同じ方法でユーザーの選択を取得できます。

[SelectedItem](/uwp/api/windows.ui.xaml.controls.primitives.selector.selecteditem) プロパティを使用して、コンボ ボックスの選択された項目を取得または設定することができ、[SelectedIndex](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedindex) プロパティを使用して、選択された項目のインデックスを取得または設定することができます。

選択されたデータ項目の特定のプロパティの値を取得するには、[SelectedValue](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedvalue) プロパティを使用できます。 この場合は、[SelectedValuePath](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedvaluepath) を設定して、選択された項目のどのプロパティの値を取得するかを指定します。

> [!TIP]
> 既定の選択を示すために SelectedItem または SelectedIndex を設定する場合、コンボ ボックスの Items コレクションを設定する前にプロパティを設定すると、例外が発生します。 XAML で Items を定義しない場合は、コンボ ボックスの Loaded イベントを処理し、Loaded イベント ハンドラー内に SelectedItem または SelectedIndex を設定することをお勧めします。

XAML でこれらのプロパティにバインドするか、[SelectionChanged](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) イベントを処理して、選択の変更に応答することができます。

イベント ハンドラーのコードでは、[SelectionChangedEventArgs.AddedItems](/uwp/api/windows.ui.xaml.controls.selectionchangedeventargs.addeditems) プロパティから、選択された項目を取得できます。 以前に選択されていた項目 (ある場合) は、[SelectionChangedEventArgs.RemovedItems](/uwp/api/windows.ui.xaml.controls.selectionchangedeventargs.removeditems) プロパティから取得できます。 AddedItems と RemovedItems のコレクションには、それぞれ 1 つの項目のみが含まれます (コンボ ボックスが複数選択をサポートしていないため)。

この例では、SelectionChanged イベントを処理する方法のほか、選択された項目にバインドする方法も示します。

```xaml
<StackPanel>
    <ComboBox x:Name="colorComboBox" Width="200"
              Header="Colors" PlaceholderText="Pick a color"
              SelectionChanged="ColorComboBox_SelectionChanged">
        <x:String>Blue</x:String>
        <x:String>Green</x:String>
        <x:String>Red</x:String>
        <x:String>Yellow</x:String>
    </ComboBox>

    <Rectangle x:Name="colorRectangle" Height="30" Width="100"
               Margin="0,8,0,0" HorizontalAlignment="Left"/>

    <TextBlock Text="{x:Bind colorComboBox.SelectedIndex, Mode=OneWay}"/>
    <TextBlock Text="{x:Bind colorComboBox.SelectedItem, Mode=OneWay}"/>
</StackPanel>
```

```csharp
private void ColorComboBox_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    // Add "using Windows.UI;" for Color and Colors.
    string colorName = e.AddedItems[0].ToString();
    Color color;
    switch (colorName)
    {
        case "Yellow":
            color = Colors.Yellow;
            break;
        case "Green":
            color = Colors.Green;
            break;
        case "Blue":
            color = Colors.Blue;
            break;
        case "Red":
            color = Colors.Red;
            break;
    }
    colorRectangle.Fill = new SolidColorBrush(color);
}
```

#### <a name="selectionchanged-and-keyboard-navigation"></a>SelectionChanged とキーボードのナビゲーション

既定では、SelectionChanged イベントは、ユーザーが一覧にある項目を対象にクリック、タップ、または Enter キーを押すことで選択を確定し、コンボ ボックスが閉じたときに発生します。 開いているコンボ ボックスの一覧をユーザーがキーボードの方向キーを使って移動しても、選択は変更されません。

開いている一覧をユーザーが方向キーを押して移動している間に "ライブ更新" されるコンボ ボックスを作成するには (フォント選択ドロップダウン リストなど)、[SelectionChangedTrigger](/uwp/api/windows.ui.xaml.controls.combobox.selectionchangedtrigger) を [Always](/uwp/api/windows.ui.xaml.controls.comboboxselectionchangedtrigger) に設定します。 これにより、開いている一覧にある別の項目にフォーカスが変更されたときに、SelectionChanged イベントが発生します。

#### <a name="selected-item-behavior-change"></a>選択された項目の動作の変更

Windows 10 Version 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) 以降では、編集可能なコンボ ボックスをサポートするために、選択された項目の動作が更新されています。

SDK 17763 より前は、SelectedItem プロパティ (したがって、SelectedValue と SelectedIndex) の値が、コンボ ボックスの Items コレクションに存在する必要があります。 前の例を使用して、`colorComboBox.SelectedItem = "Pink"` を設定すると、次の結果になります。

- SelectedItem = null
- SelectedValue = null
- SelectedIndex = -1

SDK 17763 以降、SelectedItem プロパティ (したがって、SelectedValue と SelectedIndex) の値が、コンボ ボックスの Items コレクションに存在する必要があります。 前の例を使用して、`colorComboBox.SelectedItem = "Pink"` を設定すると、次の結果になります。

- SelectedItem = Pink
- SelectedValue = Pink
- SelectedIndex = -1

### <a name="text-search"></a>テキスト検索

コンボ ボックスでは、コレクション内を自動的に検索できます。 開かれた状態または閉じた状態のコンボ ボックスにフォーカスがあるときに、物理的なキーボードで文字を入力すると、ユーザーが入力した文字列に一致する候補がビューに表示されます。 この機能は、長いリストを操作するときに特に役立ちます。 たとえば、州のリストが含まれているドロップダウンを操作するとき、“w” キーを押すと、“Washington” (ワシントン) がビューに表示され、すばやく選ぶことができます。 検索するときに、大文字と小文字は区別されません。

[IsTextSearchEnabled](/uwp/api/windows.ui.xaml.controls.combobox.istextsearchenabled) プロパティを **false** に設定すると、この機能を無効にすることができます。

## <a name="make-a-combo-box-editable"></a>コンボ ボックスを編集可能にする

> [!IMPORTANT]
> この機能には、Windows 10 Version 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) 以降が必要です。

既定では、コンボ ボックスを使用すると、ユーザーは事前に定義されたオプションの一覧から選択できます。 ただし、一覧に有効な値のサブセットのみが含まれていて、一覧にないその他の値をユーザーが入力できる場合があります。 これをサポートするために、コンボ ボックスを編集可能にすることができます。

コンボ ボックスを編集可能にするには、[IsEditable](/uwp/api/windows.ui.xaml.controls.combobox.iseditable) プロパティを **true** に設定します。 次に [TextSubmitted](/uwp/api/Windows.UI.Xaml.Controls.ComboBox) イベントを処理して、ユーザーが入力した値を処理します。

既定では、ユーザーがカスタム テキストを確定したときに、SelectedItem 値が更新されます。 TextSubmitted イベント引数で **Handled** を **true** に設定することで、この動作をオーバーライドできます。 イベントが処理済みとしてマークされると、コンボ ボックスによるイベント後の操作はそれ以上実行されず、コンボ ボックスは編集中の状態のままとなります。 SelectedItem は更新されません。

この例では、シンプルな編集可能コンボ ボックスを示しています。 一覧にはシンプルな文字列が含まれていて、ユーザーが入力した値は入力したとおりに使用されます。

"recently used names" (最近使った名前) のセレクターでは、ユーザーはカスタム文字列を入力できます。 'RecentlyUsedNames' の一覧にはいくつかの値が含まれていて、ユーザーはそこから選択できますが、カスタムの新しい値を追加することもできます。 'CurrentName' プロパティは、現在入力されている名前を表します。

```xaml
<ComboBox IsEditable="true"
          ItemsSource="{x:Bind RecentlyUsedNames}"
          SelectedItem="{x:Bind CurrentName, Mode=TwoWay}"/>
```

### <a name="text-submitted"></a>送信されたテキスト

[TextSubmitted](/uwp/api/Windows.UI.Xaml.Controls.ComboBox) イベントを処理して、ユーザーが入力した値を処理できます。 イベント ハンドラーでは、通常、ユーザーが入力した値が有効であることを検証してから、その値をアプリで使用します。 状況に応じて、将来使用するためにコンボ ボックスのオプションの一覧に値を追加することもできます。

TextSubmitted イベントは、これらの条件が満たされたときに発生します。

- IsEditable プロパティが **true**
- ユーザーがコンボ ボックスの一覧で既存のエントリに一致しないテキストを入力する
- ユーザーが Enter キーを押すか、コンボ ボックスからフォーカスを移動する

ユーザーがテキストを入力してから一覧を上下に移動しても、TextSubmitted イベントは発生しません。

### <a name="sample---validate-input-and-use-locally"></a>サンプル - 入力を検証してローカルに使用する

この例では、フォント サイズのセレクターには、フォント サイズの変化に対応する値のセットが含まれていますが、ユーザーは一覧にないフォント サイズを入力できます。

一覧にない値をユーザーが追加すると、フォント サイズが更新されますが、その値はフォント サイズの一覧に追加されません。

新しく入力された値が無効な場合は、SelectedValue を使用して Text プロパティを既知の最新の正しい値に戻します。

```xaml
<ComboBox x:Name="fontSizeComboBox"
          IsEditable="true"
          ItemsSource="{x:Bind ListOfFontSizes}"
          TextSubmitted="FontSizeComboBox_TextSubmitted"/>
```

```csharp
private void FontSizeComboBox_TextSubmitted(ComboBox sender, ComboBoxTextSubmittedEventArgs e)
{
    if (byte.TryParse(e.Text, out double newValue))
    {
        // Update the app’s font size.
        _fontSize = newValue;
    }
    else
    {
        // If the item is invalid, reject it and revert the text.
        // Mark the event as handled so the framework doesn’t update the selected item.
        sender.Text = sender.SelectedValue.ToString();
        e.Handled = true;
    }
}
```

### <a name="sample---validate-input-and-add-to-list"></a>サンプル - 入力を検証して一覧に追加する

ここでは、"favorite color セレクター" には、好きな色として最も一般的な色 (赤、青、緑、オレンジ色) が含まれていますが、ユーザーは一覧にない好きな色を入力できます。 ユーザーが有効な色 (ピンクなど) を追加すると、その新しく入力された色が一覧に追加され、アクティブな "favorite color" (好きな色) として設定されます。

```xaml
<ComboBox x:Name="favoriteColorComboBox"
          IsEditable="true"
          ItemsSource="{x:Bind ListOfColors}"
          TextSubmitted="FavoriteColorComboBox_TextSubmitted"/>
```

```csharp
private void FavoriteColorComboBox_TextSubmitted(ComboBox sender, ComboBoxTextSubmittedEventArgs e)
{
    if (IsValid(e.Text))
    {
        FavoriteColor newColor = new FavoriteColor()
        {
            ColorName = e.Text,
            Color = ColorFromStringConverter(e.Text)
        }
        ListOfColors.Add(newColor);
    }
    else
    {
        // If the item is invalid, reject it but do not revert the text.
        // Mark the event as handled so the framework doesn’t update the selected item.
        e.Handled = true;
    }
}

bool IsValid(string Text)
{
    // Validate that the string is: not empty; a color.
}
```

## <a name="dos-and-donts"></a>推奨と非推奨

- コンボ ボックス項目のテキストのコンテンツは、単一行に制限します。
- コンボ ボックス内の項目は、最も論理的な順序に並べ替えます。 関連するオプションをグループ化し、最も一般的なオプションを先頭に配置します。 名前はアルファベット順、数値は数値順、日付は時系列順に並べ替えます。

## <a name="get-the-sample-code"></a>サンプル コードを入手する

- [XAML コントロール ギャラリー サンプル](https://github.com/Microsoft/Xaml-Controls-Gallery) - インタラクティブな形で XAML コントロールのすべてを参照できます。
- [AutoSuggestBox サンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlAutoSuggestBox)

## <a name="related-articles"></a>関連記事

- [テキスト コントロール](text-controls.md)
- [スペル チェック](text-controls.md)
- [検索](search.md)
- [TextBox クラス](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox)
- [Windows.UI.Xaml.Controls PasswordBox クラス](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.PasswordBox)
- [String.Length プロパティ](https://docs.microsoft.com/dotnet/api/system.string.length?redirectedfrom=MSDN#System_String_Length)
