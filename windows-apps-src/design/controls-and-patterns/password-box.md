---
Description: パスワード ボックスは、プライバシーの目的で入力文字が非表示になるテキスト入力ボックスです。
title: パスワード ボックスのガイドライン
ms.assetid: 332B04D6-4FFE-42A4-8B3D-ABE8266C7C18
dev.assetid: 4BFDECC6-9BC5-4FF5-8C63-BB36F6DDF2EF
label: Password box
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: miguelrb
design-contact: ksulliv
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 2bca2777145dd513cd19bfe1b002b5ec81d78c62
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89169736"
---
# <a name="password-box"></a>パスワード ボックス

パスワード ボックスは、プライバシーの目的で入力文字が非表示になるテキスト入力ボックスです。 パスワード ボックスは、入力されたテキストの代わりに代替文字が表示される点を除けば、テキスト ボックスに似ています。 この代替文字は、構成できます。

既定では、ユーザーは表示ボタンを押すことによってパスワード ボックスでパスワードを表示できます。 表示ボタンを無効にしたり、別の方法でパスワードを表示できるようにしたりすることもできます (チェック ボックスなど)。

**Windows UI ライブラリを入手する**

|  |  |
| - | - |
| ![WinUI ロゴ](images/winui-logo-64x64.png) | Windows UI ライブラリ 2.2 以降には、丸めた角を使用するこのコントロールの新しいテンプレートが含まれます。 詳しくは、「[角の半径](../style/rounded-corner.md)」をご覧ください。 WinUI は、Windows アプリの新しいコントロールと UI 機能が含まれる NuGet パッケージです。 インストール手順などについて詳しくは、「[Windows UI Library (Windows UI ライブラリ)](/uwp/toolkits/winui/)」をご覧ください。 |

> **プラットフォーム API**: [PasswordBox クラス](/uwp/api/Windows.UI.Xaml.Controls.PasswordBox)、[Password プロパティ](/uwp/api/windows.ui.xaml.controls.passwordbox.password)、[PasswordChar プロパティ](/uwp/api/windows.ui.xaml.controls.passwordbox.passwordchar)、[PasswordRevealMode プロパティ](/uwp/api/windows.ui.xaml.controls.passwordbox.passwordrevealmode)、[PasswordChanged イベント](/uwp/api/windows.ui.xaml.controls.passwordbox.passwordchanged)

## <a name="is-this-the-right-control"></a>これは適切なコントロールですか?

**PasswordBox** コントロールを使用して、パスワードや、社会保障番号などのプライベート データを収集できます。

適切なテキスト コントロールの選択の詳細については、「[テキスト コントロール](text-controls.md)」の記事をご覧ください。

## <a name="examples"></a>例

<table>
<th align="left">XAML コントロール ギャラリー<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML コントロール ギャラリー</strong> アプリがインストールされている場合、こちらをクリックして<a href="xamlcontrolsgallery:/item/PasswordBox">アプリを開き、PasswordBox の動作を確認</a>してください。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML コントロール ギャラリー アプリを入手する (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">ソース コード (GitHub) を入手する</a></li>
    </ul>
</td>
</tr>
</table>

パスワード ボックスには、次のようにいくつかの状態があります。

待機状態のパスワード ボックスでは、目的がユーザーにわかるように、ヒントのテキストを表示できます。

![ヒントのテキストが表示された、待機状態のパスワード ボックス](images/passwordbox-rest-hinttext.png)

ユーザーがパスワード ボックスに入力すると、既定の動作では、入力中のテキストを隠す記号が表示されます。

![テキスト入力中でフォーカス状態のパスワード ボックス](images/passwordbox-focus-typing.png)

右側にある "表示" ボタンを押すと、入力中のパスワード テキストを一時的に表示できます。

![テキストが一時的に表示されたパスワード ボックス](images/passwordbox-text-reveal.png)

## <a name="create-a-password-box"></a>パスワード ボックスの作成

PasswordBox の内容を取得または設定するには [Password](/uwp/api/windows.ui.xaml.controls.passwordbox.password) プロパティを使います。 [PasswordChanged](/uwp/api/windows.ui.xaml.controls.passwordbox.passwordchanged) イベントのハンドラーでこの操作を実行すると、ユーザーがパスワードを入力している間に検証を実行できます。 ボタンの [Click](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) などの別のイベントを使って、ユーザーが入力を終えてから検証を実行することもできます。

パスワード ボックス コントロールの XAML を次に示します。PasswordBox の既定の外観を確認してください。 ユーザーがパスワードを入力すると、リテラル値の "Password" であるかどうかが調べられます。 一致する場合、メッセージがユーザーに表示されます。

```xaml
<StackPanel>  
  <PasswordBox x:Name="passwordBox" Width="200" MaxLength="16"
             PasswordChanged="passwordBox_PasswordChanged"/>

  <TextBlock x:Name="statusText" Margin="10" HorizontalAlignment="Center" />
</StackPanel>   
```

```csharp
private void passwordBox_PasswordChanged(object sender, RoutedEventArgs e)
{
    if (passwordBox.Password == "Password")
    {
        statusText.Text = "'Password' is not allowed as a password.";
    }
    else
    {
        statusText.Text = string.Empty;
    }
}
```
このコードを実行し、ユーザーが「Password」と入力した場合に表示される結果を次に示します。

![検証メッセージを表示するパスワード ボックス](images/passwordbox-revealed-validation.png)

### <a name="password-character"></a>パスワード文字

パスワードを隠すために使う文字を変更するには、[PasswordChar](/uwp/api/windows.ui.xaml.controls.passwordbox.passwordchar) プロパティを設定します。 ここでは、既定の記号をアスタリスクに置き換えています。

```xaml
<PasswordBox x:Name="passwordBox" Width="200" PasswordChar="*"/>
```

結果は次のようになります。

![カスタムの文字が使われているパスワード ボックス](images/passwordbox-custom-char.png)

### <a name="headers-and-placeholder-text"></a>ヘッダーとプレースホルダー テキスト

[Header](/uwp/api/windows.ui.xaml.controls.passwordbox.header) プロパティと [PlaceholderText](/uwp/api/windows.ui.xaml.controls.passwordbox.placeholdertext) プロパティを使って PasswordBox のコンテキストを提示することができます。 パスワードを変更するためのフォームなど、複数のボックスがある場合に特に便利です。

```xaml
<PasswordBox x:Name="passwordBox" Width="200" Header="Password" PlaceholderText="Enter your password"/>
```

![ヒントのテキストが表示された、待機状態のパスワード ボックス](images/passwordbox-rest-hinttext.png)

### <a name="maximum-length"></a>最大長

ユーザーが入力できる文字の最大数を指定するには、[MaxLength](/uwp/api/windows.ui.xaml.controls.passwordbox.maxlength) プロパティを設定します。 長さの最小値を指定するプロパティはありませんが、アプリのコードでパスワードの長さをチェックしたりその他の検証を実行したりできます。

## <a name="password-reveal-mode"></a>パスワード表示モード

PasswordBox には、ユーザーが押すとパスワード テキストを表示できるボタンが組み込まれています。 ユーザーがこのボタンを操作した結果を次に示します。 ユーザーがボタンを離すと、パスワードは自動的に非表示になります。

![テキストが一時的に表示されたパスワード ボックス](images/passwordbox-text-reveal.png)

### <a name="peek-mode"></a>プレビュー モード

既定で表示されるパスワード表示ボタン ("プレビュー" ボタン) では、 ユーザーがパスワードを表示するにはボタンを押し続けなければならないため、高レベルのセキュリティが維持されます。

[PasswordRevealMode](/uwp/api/windows.ui.xaml.controls.passwordbox.passwordrevealmode) プロパティの値は、ユーザーにパスワード表示ボタンが表示されるかどうかを決定する唯一の要因ではありません。 その他の要因には、コントロールの表示幅が最小幅を上回っているか、PasswordBox にフォーカスがあるか、テキスト入力フィールドに文字が含まれているか、などがあります。 パスワード表示ボタンが表示されるのは、PasswordBox が初めてフォーカスを受け取り、文字が入力されたときだけです。 いったん PasswordBox からフォーカスが移動すると、その後にフォーカスが戻っても、パスワードをクリアして入力し直さない限り、パスワード表示ボタンは表示されません。

> **注意:** &nbsp;&nbsp;Windows 10 より前のバージョンでは、パスワード表示ボタンは既定で表示されませんでした。 アプリのセキュリティにより、パスワードを必ず非表示にする必要がある場合は、PasswordRevealMode を Hidden に設定してください。

### <a name="hidden-and-visible-modes"></a>非表示モードと表示モード

[PasswordRevealMode](/uwp/api/Windows.UI.Xaml.Controls.PasswordRevealMode) には、そのほかに **Hidden** と **Visible** という列挙値があります。これらの列挙値を使うと、パスワード表示ボタンを非表示にして、パスワードを非表示にするかどうかをプログラムで管理できます。

パスワードを常に非表示にするには、PasswordRevealMode を Hidden に設定します。 パスワードを常に非表示にする必要がある場合以外は、カスタム UI を用意して、ユーザーが PasswordRevealMode の Hidden と Visible を切り替えられるようにすることができます。 たとえば、次の例に示すように、パスワードを表示するかどうかをチェック ボックスで切り替えることができます。 [ToggleButton](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton) などのその他のコントロールを使ってユーザーがモードを切り替えられるようにすることもできます。

次の例は、[CheckBox](/uwp/api/Windows.UI.Xaml.Controls.CheckBox) を使ってユーザーが PasswordBox の表示モードを切り替えられるようにする方法を示しています。

```xaml
<StackPanel Width="200">
    <PasswordBox Name="passwordBox1"
                 PasswordRevealMode="Hidden"/>
    <CheckBox Name="revealModeCheckBox" Content="Show password"
              IsChecked="False"
              Checked="CheckBox_Changed" Unchecked="CheckBox_Changed"/>
</StackPanel>
```

```csharp
private void CheckBox_Changed(object sender, RoutedEventArgs e)
{
    if (revealModeCheckBox.IsChecked == true)
    {
        passwordBox1.PasswordRevealMode = PasswordRevealMode.Visible;
    }
    else
    {
        passwordBox1.PasswordRevealMode = PasswordRevealMode.Hidden;
    }
}
```

この PasswordBox は次のように表示されます。

![カスタムの表示ボタンが使われているパスワード ボックス](images/passwordbox-custom-reveal.png)

## <a name="choose-the-right-keyboard-for-your-text-control"></a>テキスト コントロールに適切なキーボードの選択

ユーザーがタッチ キーボード、つまりソフト入力パネル (SIP) でデータを入力できるように、ユーザーが入力すると予想されるデータの種類に合わせてテキスト コントロールの入力値の種類を設定できます。 PasswordBox でサポートされている入力値の種類は **Password** と **NumericPin** だけです。 それ以外の値はすべて無視されます。

入力値の種類の使い方について詳しくは、「[入力値の種類を使ったタッチ キーボードの変更](../input/use-input-scope-to-change-the-touch-keyboard.md)」をご覧ください。

## <a name="recommendations"></a>推奨事項

-   パスワード ボックスの目的がわかりにくい場合は、ラベルまたはプレース ホルダー テキストを使用します。 ラベルは、テキスト入力ボックスに値が存在するかどうかに関係なく表示されます。 プレースホルダー テキストはテキスト入力ボックスの内側に表示され、値を入力すると非表示になります。
-   パスワード ボックスは、入力できる値の範囲に適した幅になるようにします。 単語の長さは言語によって異なるため、アプリを世界対応にする場合は、ローカライズを考慮に入れて幅を調整します。
-   パスワード入力ボックスのすぐ横に、他のコントロールを配置しないようにします。 パスワード ボックスには、入力したパスワードをユーザーが確認するための、パスワード表示ボタンがあります。他のコントロールをすぐ横に配置すると、ユーザーが他のコントロールを操作しようとしたときに、誤ってパスワードが表示される可能性があります。 これを防ぐには、パスワード入力ボックスと他のコントロールの間には少し間隔をおくか、他のコントロールを次の行に配置します。
-   アカウントの作成時は、新しいパスワードの入力用および新しいパスワードの確認用として、2 つのパスワード ボックスを提示することを検討します。
-   ログイン時は 1 つのパスワード ボックスのみを表示します。
-   PIN の入力にパスワード ボックスを使う場合は、確認ボタンを使う代わりに、最後の数値が入力されたらすぐに応答を返すことを検討します。

## <a name="get-the-sample-code"></a>サンプル コードの入手

- [XAML コントロール ギャラリー サンプル](https://github.com/Microsoft/Xaml-Controls-Gallery) - インタラクティブな形で XAML コントロールのすべてを参照できます。

## <a name="related-articles"></a>関連記事

[テキスト コントロール](text-controls.md)

- [スペル チェックのガイドライン](text-controls.md)
- [検索の追加](/previous-versions/windows/apps/hh465231(v=win.10))
- [テキスト入力のガイドライン](text-controls.md)
- [TextBox クラス](/uwp/api/Windows.UI.Xaml.Controls.TextBox)
- [Windows.UI.Xaml.Controls PasswordBox クラス](/uwp/api/Windows.UI.Xaml.Controls.PasswordBox)
- [String.Length プロパティ](/dotnet/api/system.string.length)