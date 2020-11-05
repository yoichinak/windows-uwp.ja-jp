---
ms.assetid: CC1BF51D-3DAC-4198-ADCB-1770B901C2FC
description: TextBox コントロールによって、ユーザーはアプリにテキストを入力できます。
title: テキスト ボックス
label: Text box
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
pm-contact: miguelrb
design-contact: ksulliv
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 542822b27f356c9471ec8a6c6f5bec0aac2144ce
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034755"
---
# <a name="text-box"></a>テキスト ボックス

TextBox コントロールによって、ユーザーはアプリにテキストを入力できます。 通常、1 行のテキストを取得するために使用されますが、複数行のテキストを取得するように構成できます。 テキストは、シンプルで均一なプレーンテキスト形式で画面に表示されます。

TextBox には、テキスト入力を簡略化するための多くの機能があります。 テキストのコピーと貼り付けをサポートする、使い慣れた組み込みのコンテキスト メニューが付属しています。 [すべてクリア] ボタンによって、ユーザーは入力されているすべてのテキストを簡単に削除できます。 スペル チェック機能も組み込まれており、既定で有効になっています。

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

> **プラットフォーム API** : [TextBox クラス](/uwp/api/Windows.UI.Xaml.Controls.TextBox)、 [Text プロパティ](/uwp/api/windows.ui.xaml.controls.textbox.text)

## <a name="is-this-the-right-control"></a>適切なコントロールの選択

フォームなどで書式設定されていないテキストをユーザーが入力、編集できるようにするには、 **TextBox** コントロールを使用します。 TextBox 内のテキストの取得と設定には、[Text](/uwp/api/windows.ui.xaml.controls.textbox.text) プロパティを使用します。

TextBox を読み取り専用にすることはできますが、これは一時的な条件付きの状態である必要があります。 テキストを編集可能にしない場合は、代わりに [TextBlock](text-block.md) を使用することを検討してください。

[PasswordBox](password-box.md) コントロールを使用して、パスワードや、社会保障番号などのプライベート データを収集できます。 パスワード ボックスは、入力されたテキストの代わりに記号が表示される点を除けば、テキスト入力ボックスに似ています。

ユーザーが検索語句を入力できるようにしたり、入力時に選べる候補リストを表示したりするには、[AutoSuggestBox](auto-suggest-box.md) コントロールを使用します。

[RichEditBox](rich-edit-box.md) を使用して、リッチ テキスト ファイルを表示および編集します。

適切なテキスト コントロールの選択の詳細については、「[テキスト コントロール](text-controls.md)」の記事をご覧ください。

## <a name="examples"></a>例

<table>
<th align="left">XAML コントロール ギャラリー<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML コントロール ギャラリー</strong> アプリがインストールされている場合、こちらをクリックして<a href="xamlcontrolsgallery:/item/TextBox">アプリを開き、TextBox の動作を確認</a>してください。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML コントロール ギャラリー アプリを入手する (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">ソース コード (GitHub) を入手する</a></li>
    </ul>
</td>
</tr>
</table>

![テキスト ボックス](images/text-box.png)

## <a name="create-a-text-box"></a>テキスト ボックスの作成

ヘッダーとプレースホルダーのテキストを含むシンプルなテキスト ボックスの XAML を次に示します。

```xaml
<TextBox Width="500" Header="Notes" PlaceholderText="Type your notes here"/>
```

```csharp
TextBox textBox = new TextBox();
textBox.Width = 500;
textBox.Header = "Notes";
textBox.PlaceholderText = "Type your notes here";
// Add the TextBox to the visual tree.
rootGrid.Children.Add(textBox);
```

この XAML で表示されるテキスト ボックスは次のとおりです。

![シンプルなテキスト ボックス](images/text-box-ex1.png)

### <a name="use-a-text-box-for-data-input-in-a-form"></a>フォームでのデータ入力用のテキスト ボックスの使用

テキスト ボックスを使用してフォームでデータ入力を受け付け、[Text](/uwp/api/windows.ui.xaml.controls.textbox.text) プロパティを使用してテキスト ボックスから完全なテキスト文字列を取得するのが一般的です。 通常、Text プロパティにアクセスするには、送信ボタンのクリックなどのイベントを使用しますが、テキストが変化したときに処理を実行する必要がある場合は、[TextChanged](/uwp/api/windows.ui.xaml.controls.textbox.textchanged) イベントや [TextChanging](/uwp/api/windows.ui.xaml.controls.textbox.textchanging) イベントを処理することができます。

この例では、テキスト ボックスの現在のコンテンツを取得して設定する方法を示します。

```xaml
<TextBox name="SampleTextBox" Text="Sample Text"/>
```

```csharp
string sampleText = SampleTextBox.Text;
...
SampleTextBox.Text = "Sample text retrieved";
```

[Header](/uwp/api/windows.ui.xaml.controls.textbox.header) (ラベル) と [PlaceholderText](/uwp/api/windows.ui.xaml.controls.textbox.placeholdertext) (透かし) をテキスト コントロールに追加すると、ユーザーに用途を示すことができます。 ヘッダーの外観をカスタマイズするには、Header ではなく [HeaderTemplate](/uwp/api/windows.ui.xaml.controls.textbox.headertemplate) プロパティを設定します。 *設計については、「ラベルのガイドライン」を参照してください。*

[MaxLength](/uwp/api/windows.ui.xaml.controls.textbox.maxlength) プロパティを設定することによって、ユーザーが入力する文字数を制限できます。 ただし、MaxLength では、貼り付けられたテキストの長さは制限されません。 アプリでこれが重要である場合は、[Paste](/uwp/api/windows.ui.xaml.controls.textbox.paste) イベントを使用して、貼り付けられたテキストを変更します。

テキスト ボックスには、テキストがボックスに入力されたときに表示されるすべてクリア ボタン ("X") が含まれています。 ユーザーが [X] をクリックすると、テキスト ボックス内のテキストがクリアされます。 次のようになります。

![すべてクリア ボタンが表示されたテキスト ボックス](images/text-box-clear-all.png)

すべてクリア ボタンは、テキストが含まれフォーカスがある、編集可能な単一行テキスト ボックスにのみ表示されます。

すべてクリア ボタンは、次のいずれかの場合には表示されません。

- **IsReadOnly** が **true** である
- **AcceptsReturn** が **true** である
- **TextWrap** の値が **NoWrap** 以外である

この例では、テキスト ボックスの現在のコンテンツを取得して設定する方法を示します。

```xaml
<TextBox name="SampleTextBox" Text="Sample Text"/>
```

```csharp
string sampleText = SampleTextBox.Text;
...
SampleTextBox.Text = "Sample text retrieved";
```


### <a name="make-a-text-box-read-only"></a>テキスト ボックスを読み取り専用にする

[IsReadOnly](/uwp/api/windows.ui.xaml.controls.textbox.isreadonly) プロパティを **true** に設定すると、テキスト ボックスを読み取り専用にすることができます。 通常、アプリでの条件に基づいて、アプリ コードでこのプロパティを切り替えます。 テキストを常に読み取り専用にする必要がある場合は、代わりに TextBlock の使用を検討してください。

IsReadOnly プロパティを true に設定すると、TextBox を読み取り専用にすることができます。 たとえば、ユーザーがコメントを入力するための TextBox が特定の条件下でのみ有効になるとします。 条件が満たされるまでは、TextBox を読み取り専用にすることができます。 テキストの表示のみが必要である場合は、代わりに TextBlock や RichTextBlock の使用を検討してください。

読み取り専用テキスト ボックスの見た目は読み取り/書き込み可能なテキスト ボックスと同じであるため、ユーザーを混乱させる可能性があります。
ユーザーはテキストを選択してコピーできます。
IsEnabled

### <a name="enable-multi-line-input"></a>複数行入力の有効化

テキスト ボックスで複数行のテキストを表示するかどうかを制御するために使用できるプロパティが 2 つあります。 通常、両方のプロパティを設定して複数行テキスト ボックスを作成します。

- テキスト ボックスで改行文字の入力を受け付けて表示するには、 [AcceptsReturn](/uwp/api/windows.ui.xaml.controls.textbox.acceptsreturn) プロパティを **true** に設定します。
- テキストの折り返しを有効にするには、 [TextWrapping](/uwp/api/windows.ui.xaml.controls.textbox.textwrapping) プロパティを **Wrap** に設定します。 これにより、テキストがテキスト ボックスの端に達すると、行の区切り文字とは無関係に折り返されます。

> **注**&nbsp;&nbsp;TextBox および RichEditBox では、TextWrapping プロパティの **WrapWholeWords** 値をサポートしていません。 TextBox.TextWrapping または RichEditBox.TextWrapping の値として WrapWholeWords を使用しようとすると、無効な引数の例外がスローされます。

複数行のテキスト ボックスは、その [Height](/uwp/api/windows.ui.xaml.frameworkelement.height) プロパティや [MaxHeight](/uwp/api/windows.ui.xaml.frameworkelement.maxheight) プロパティ、または親コンテナーによって制限されていない場合、テキストを入力すると垂直方向に拡大し続けます。 複数行テキスト ボックスが、表示領域を超えて拡大しないことをテストし、表示領域を超える場合は拡大を制限する必要があります。 複数行テキスト ボックスの適切な高さを常に指定し、ユーザーが入力するときにテキスト ボックスの高さの拡大を許可しないことをお勧めします。

スクロール ホイールまたはタッチを使用したスクロールは、必要に応じて自動的に有効になります。 ただし、垂直スクロール バーは、既定では表示されません。 垂直スクロール バーを表示するには、次に示すように、埋め込みの ScrollViewer で [ScrollViewer.VerticalScrollBarVisibility](/uwp/api/windows.ui.xaml.controls.scrollviewer.verticalscrollbarvisibility) を **Auto** に設定します。

```xaml
<TextBox AcceptsReturn="True" TextWrapping="Wrap"
         MaxHeight="172" Width="300" Header="Description"
         ScrollViewer.VerticalScrollBarVisibility="Auto"/>
```

```csharp
TextBox textBox = new TextBox();
textBox.AcceptsReturn = true;
textBox.TextWrapping = TextWrapping.Wrap;
textBox.MaxHeight = 172;
textBox.Width = 300;
textBox.Header = "Description";
ScrollViewer.SetVerticalScrollBarVisibility(textBox, ScrollBarVisibility.Auto);
```

テキストを追加した後、テキスト ボックスがどのようになるかを次に示します。

![複数行テキスト ボックス](images/text-box-multi-line.png)

### <a name="format-the-text-display"></a>テキストの表示形式の設定

テキスト ボックス内のテキストを整列するには、[TextAlignment](/uwp/api/windows.ui.xaml.controls.textbox.textalignment) プロパティを使用します。 ページのレイアウト内のテキスト ボックスを整列するには、[HorizontalAlignment](/uwp/api/windows.ui.xaml.frameworkelement.horizontalalignment) プロパティと [VerticalAlignment](/uwp/api/windows.ui.xaml.frameworkelement.verticalalignment) プロパティを使用します。

テキスト ボックスが書式設定されていないテキストのみをサポートするときに、ブランド化と一致するようにテキスト ボックスにテキストを表示する方法をカスタマイズできます。 標準的な [Control](/uwp/api/Windows.UI.Xaml.Controls.Control) プロパティ ([FontFamily](/uwp/api/windows.ui.xaml.controls.control.fontfamily)、[FontSize](/uwp/api/windows.ui.xaml.controls.control.fontsize)、[FontStyle](/uwp/api/windows.ui.xaml.controls.control.fontstyle)、[Background](/uwp/api/windows.ui.xaml.controls.control.background)、[Foreground](/uwp/api/windows.ui.xaml.controls.control.foreground)、[CharacterSpacing](/uwp/api/windows.ui.xaml.controls.control.characterspacing) など) を設定して、テキストの外観を変更できます。 これらのプロパティが影響を与えるのは、テキスト ボックスがローカルでテキストを表示する方法だけです。したがって、テキストをコピーしてリッチ テキスト コントロールなどに貼り付けても、書式設定は適用されません。

この例では、テキストの外観をカスタマイズするためにいくつかのプロパティが設定された読み取り専用のテキスト ボックスを示します。

```xaml
<TextBox Text="Sample Text" IsReadOnly="True"
         FontFamily="Verdana" FontSize="24"
         FontWeight="Bold" FontStyle="Italic"
         CharacterSpacing="200" Width="300"
         Foreground="Blue" Background="Beige"/>
```

```csharp
TextBox textBox = new TextBox();
textBox.Text = "Sample Text";
textBox.IsReadOnly = true;
textBox.FontFamily = new FontFamily("Verdana");
textBox.FontSize = 24;
textBox.FontWeight = Windows.UI.Text.FontWeights.Bold;
textBox.FontStyle = Windows.UI.Text.FontStyle.Italic;
textBox.CharacterSpacing = 200;
textBox.Width = 300;
textBox.Background = new SolidColorBrush(Windows.UI.Colors.Beige);
textBox.Foreground = new SolidColorBrush(Windows.UI.Colors.Blue);
// Add the TextBox to the visual tree.
rootGrid.Children.Add(textBox);
```

このテキスト ボックスは次のように表示されます。

![書式設定されたテキスト ボックス](images/text-box-formatted.png)

### <a name="modify-the-context-menu"></a>コンテキスト メニューの変更

既定では、テキスト ボックスのコンテキスト メニューに表示されるコマンドは、テキスト ボックスの状態によって異なります。 たとえば、次のコマンドは、テキスト ボックスが編集可能なときに表示できます。

コマンド | 表示される状況
------- | -------------
コピー | テキストが選択されている。
［切り取り］ | テキストが選択されている。
貼り付け | クリップボードにテキストが含まれている。
すべて選択 | TextBox にテキストが含まれている。
元に戻す | テキストが変更されている。

コンテキスト メニューに表示されるコマンドを変更するには、[ContextMenuOpening](/uwp/api/windows.ui.xaml.controls.textbox.contextmenuopening) イベントを処理します。 この例については、 <a href="xamlcontrolsgallery:/item/RichEditBox">XAML コントロール ギャラリー</a>の **Customizing RichEditBox's CommandBarFlyout - adding 'Share'** (RichEditBox の CommandBarFlyout のカスタマイズ - 'Share' の追加) を参照してください。 設計については、[コンテキスト メニュー](menus.md)のガイドラインを参照してください。

### <a name="select-copy-and-paste"></a>選択、コピー、貼り付け

[SelectedText](/uwp/api/windows.ui.xaml.controls.textbox.selectedtext) プロパティを使用して、テキスト ボックス内で選択したテキストを取得または設定できます。 [SelectionStart](/uwp/api/windows.ui.xaml.controls.textbox.selectionstart) プロパティと [SelectionLength](/uwp/api/windows.ui.xaml.controls.textbox.selectionlength) プロパティ、[Select](/uwp/api/windows.ui.xaml.controls.textbox.select) メソッドと [SelectAll](/uwp/api/windows.ui.xaml.controls.textbox.selectall) メソッドを使用して、テキストの選択を操作します。 ユーザーがテキストの選択または選択解除を行ったときに操作を実行するには、[SelectionChanged](/uwp/api/windows.ui.xaml.controls.textbox.selectionchanged) イベントを処理します。 選択したテキストを強調表示するために使用する色を変更するには、[SelectionHighlightColor](/uwp/api/windows.ui.xaml.controls.textbox.selectionhighlightcolor) プロパティを設定します。

TextBox では、既定でコピーと貼り付けをサポートしています。 アプリの編集可能なテキスト コントロールで、[Paste](/uwp/api/windows.ui.xaml.controls.textbox.paste) イベントのカスタム処理を行うことができます。 たとえば、複数行からなる 1 つの住所の改行を単一行の検索ボックスに貼り付けるときに削除できます。 または、貼り付けられたテキストの長さをチェックし、データベースに保存できる最大の長さを超えている場合はユーザーに警告することができます。 詳しい説明と例については、[Paste](/uwp/api/windows.ui.xaml.controls.textbox.paste) イベントに関するトピックをご覧ください。

これらのプロパティとメソッドを使用する例を次に示します。 1 つ目のテキスト ボックス内のテキストを選択すると、選択したテキストが 2 つ目のテキスト ボックスに表示されます。2 つ目のテキスト ボックスは読み取り専用です。 [SelectionLength](/uwp/api/windows.ui.xaml.controls.textbox.selectionlength) プロパティと [SelectionStart](/uwp/api/windows.ui.xaml.controls.textbox.selectionstart) プロパティの値は、2 つのテキスト ブロックに表示されます。 この処理を実行するには、[SelectionChanged](/uwp/api/windows.ui.xaml.controls.textbox.selectionchanged) イベントを使用します。

```xaml
<StackPanel>
   <TextBox x:Name="textBox1" Height="75" Width="300" Margin="10"
         Text="The text that is selected in this TextBox will show up in the read only TextBox below."
         TextWrapping="Wrap" AcceptsReturn="True"
         SelectionChanged="TextBox1_SelectionChanged" />
   <TextBox x:Name="textBox2" Height="75" Width="300" Margin="5"
         TextWrapping="Wrap" AcceptsReturn="True" IsReadOnly="True"/>
   <TextBlock x:Name="label1" HorizontalAlignment="Center"/>
   <TextBlock x:Name="label2" HorizontalAlignment="Center"/>
</StackPanel>
```

```csharp
private void TextBox1_SelectionChanged(object sender, RoutedEventArgs e)
{
    textBox2.Text = textBox1.SelectedText;
    label1.Text = "Selection length is " + textBox1.SelectionLength.ToString();
    label2.Text = "Selection starts at " + textBox1.SelectionStart.ToString();
}
```

このコードを実行すると、次のように表示されます。

![テキスト ボックスで選択されたテキスト](images/text-box-selection.png)

## <a name="choose-the-right-keyboard-for-your-text-control"></a>テキスト コントロールに適切なキーボードの選択

ユーザーがタッチ キーボード、つまりソフト入力パネル (SIP) でデータを入力できるように、ユーザーが入力すると予想されるデータの種類に合わせてテキスト コントロールの入力値の種類を設定できます。

タッチ キーボードは、アプリがタッチ スクリーン付きのデバイスで実行されているときにテキスト入力に使用できます。 タッチ キーボードは、TextBox または RichEditBox などの編集可能な入力フィールドをユーザーがタップしたときに呼び出されます。 ユーザーが入力すると予想されるデータの種類と一致するようにテキスト コントロールの入力値の種類を設定することで、ユーザーがより速く簡単にアプリにデータを入力できるようになります。 入力値の種類は、システムに対してコントロールが予期しているテキスト入力の種類のヒントとなるため、システムはその入力の種類用の特殊なタッチ キーボード レイアウトを提供できます。

たとえば、テキスト ボックスが 4 桁の PIN の入力専用の場合は、 [InputScope](/uwp/api/windows.ui.xaml.controls.textbox.inputscope) プロパティを **Number** に設定します。 これにより、システムに数字キーパッド レイアウトの表示が指示されるため、ユーザーは簡単に PIN を入力できます。

> **重要**&nbsp;&nbsp;入力値の種類の設定によって、入力の検証が実行されるわけではありません。また、ユーザーが、ハードウェア キーボードやその他の入力デバイスから入力できなくなることもありません。 必要に応じて、コードで入力を検証する必要があります。

タッチ キーボードに影響するその他のプロパティとして、[IsSpellCheckEnabled](/uwp/api/windows.ui.xaml.controls.textbox.isspellcheckenabled)、[IsTextPredictionEnabled](/uwp/api/windows.ui.xaml.controls.textbox.istextpredictionenabled)、[PreventKeyboardDisplayOnProgrammaticFocus](/uwp/api/windows.ui.xaml.controls.textbox.preventkeyboarddisplayonprogrammaticfocus) があります (IsSpellCheckEnabled は、ハードウェア キーボードを使用する場合にも TextBox に影響します)。

詳細な情報と例については、「[入力値の種類を使ったタッチ キーボードの変更](../input/use-input-scope-to-change-the-touch-keyboard.md)」とプロパティのドキュメントをご覧ください。

## <a name="recommendations"></a>推奨事項

- テキスト ボックスの目的がわかりにくい場合は、ラベルまたはプレースホルダー テキストを使用します。 ラベルは、テキスト入力ボックスに値が存在するかどうかに関係なく表示されます。 プレースホルダー テキストはテキスト入力ボックスの内側に表示され、値を入力すると非表示になります。
- テキスト ボックスは、入力できる値の範囲に適した幅になるようにします。 単語の長さは言語によって異なるため、アプリを世界対応にする場合は、ローカライズを考慮に入れて幅を調整します。
- テキスト入力ボックスは、通常、単一行 (`TextWrap = "NoWrap"`) です。 ユーザーが長い文字列を入力または編集する必要がある場合は、テキスト入力ボックスを複数行 (`TextWrap = "Wrap"`) に設定します。
- 通常、テキスト入力ボックスは編集可能なテキストに対して使用します。 ただし、読み取り、選択、コピーはできるが編集はできないように、テキスト入力ボックスを読み取り専用に設定することもできます。
- 画面をすっきりと見せる必要がある場合は、制御するチェック ボックスがオンの場合にのみ、一連のテキスト入力ボックスを表示することを検討してください。 また、有効な状態のテキスト入力ボックスをチェック ボックスなどのコントロールにバインドすることもできます。
- 既に値が入力されているテキスト入力ボックスをユーザーがタップしたときの動作を検討します。 既定の動作では、単語の間に挿入ポイントが配置され、何も選択されません。この動作は値の置き換えよりも編集に適しています。 対象のテキスト入力ボックスが、編集よりも主に置き換えに使用される場合は、コントロールがフォーカスを受け取ったときに常にフィールドのすべてのテキストを選択し、入力によって選択内容が置き換えられるように設定できます。

### <a name="single-line-input-boxes"></a>単一行入力ボックス

- 少量のテキスト情報を多数取得する場合は、いくつかの単一行テキスト ボックスを使用します。 それらのテキスト ボックスに関連性がある場合は、グループ化します。

- 単一行テキスト ボックスの幅は、予測される最長の入力値より少し広くします。 そのようにするとコントロールの幅が広くなり過ぎる場合は、2 つのコントロールに分割します。 たとえば、1 つの住所の入力を "Address line 1" と "Address line 2" に分割できます。
- 入力できる最大文字数を設定します。 バッキング データ ソースが長い入力文字列を許可しない場合は、入力を制限し、制限に達したら検証ポップアップを使用してユーザーに知らせます。
- ユーザーから少量のテキストを収集するには、単一行テキスト入力コントロールを使用します。

    次の例は、セキュリティの質問への回答を取得する単一行テキスト ボックスを示しています。 回答には短い文字列が想定されるので、ここでは単一行テキスト ボックスが適しています。

    ![基本データ入力](images/guidelines_and_checklist_for_singleline_text_input_type_text.png)

- 特定の書式のデータを入力するには、短い固定サイズの単一行テキスト入力コントロールのセットを使用します。

    ![書式付きのデータ入力](images/textinput-example-productkey.png)

- 文字列を入力または編集するには、単一行の、制約のないテキスト入力コントロールと、ユーザーが有効な値を選択できるように補助するコマンド ボタンを組み合わせて使用します。

    ![補助付きのデータ入力](images/textinput-example-assisted.png)

### <a name="multi-line-text-input-controls"></a>複数行テキスト入力コントロール

- リッチ テキスト ボックスを作成する場合は、スタイル設定ボタンを用意し、その動作を実装します。
- アプリのスタイルに合ったフォントを使用します。
- テキスト コントロールの高さは、典型的な入力が十分に入るように設定します。
- 最大文字数または単語数を設定して長いテキストを取得する場合、プレーンテキスト ボックスを使用し、ライブ実行のカウンターを用意して、制限までの残りの文字数または単語数をユーザーに示します。 カウンターは自分で作成する必要があります。テキスト ボックスの下に配置し、ユーザーが文字や単語を入力するたびに動的に更新します。

    ![長いテキスト](images/guidelines_and_checklist_for_multiline_text_input_text_limits.png)

- ユーザーの入力中にテキスト入力コントロールの高さが増加するようにはしません。
- ユーザーが 1 行しか必要としていない場合は、複数行テキスト ボックスを使用しません。
- プレーンテキスト コントロールで十分な場合に、リッチ テキスト コントロールを使用しません。

## <a name="get-the-sample-code"></a>サンプル コードを入手する

- [XAML コントロール ギャラリー サンプル](https://github.com/Microsoft/Xaml-Controls-Gallery) - インタラクティブな形で XAML コントロールのすべてを参照できます。

## <a name="related-articles"></a>関連記事

- [テキスト コントロール](text-controls.md)
- [スペル チェックのガイドライン](text-controls.md)
- [検索の追加](/previous-versions/windows/apps/hh465231(v=win.10))
- [テキスト入力のガイドライン](text-controls.md)
- [TextBox クラス](/uwp/api/Windows.UI.Xaml.Controls.TextBox)
- [PasswordBox クラス](/uwp/api/Windows.UI.Xaml.Controls.PasswordBox)
- [String.Length プロパティ](/dotnet/api/system.string.length)
