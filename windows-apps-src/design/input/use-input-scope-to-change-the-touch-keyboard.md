---
Description: ユーザーがタッチ キーボード、つまりソフト入力パネル (SIP) でデータを入力できるように、ユーザーが入力すると予想されるデータの種類に合わせてテキスト コントロールの入力値の種類を設定できます。
MS-HAID: dev\_ctrl\_layout\_txt.use\_input\_scope\_to\_change\_the\_touch\_keyboard
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: 入力値の種類を使ったタッチ キーボードの変更
ms.assetid: 6E5F55D7-24D6-47CC-B457-B6231EDE2A71
template: detail.hbs
keywords: キーボード, アクセシビリティ, ナビゲーション, フォーカス, テキスト, 入力, ユーザーの操作
ms.date: 02/08/2017
ms.topic: article
ms.openlocfilehash: e6e140a1967ca3ffe7775f427ccae7a7e07c5ca6
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89165816"
---
# <a name="use-input-scope-to-change-the-touch-keyboard"></a>入力値の種類を使ったタッチ キーボードの変更

ユーザーがタッチ キーボード、つまりソフト入力パネル (SIP) でデータを入力できるように、ユーザーが入力すると予想されるデータの種類に合わせてテキスト コントロールの入力値の種類を設定できます。

### <a name="important-apis"></a>重要な API
- [InputScope](/uwp/api/windows.ui.xaml.controls.textbox.inputscope)
- [InputScopeNameValue](/uwp/api/Windows.UI.Xaml.Input.InputScopeNameValue)


タッチ キーボードは、アプリがタッチ スクリーン付きのデバイスで実行されているときにテキスト入力に使用できます。 タッチキーボードは、ユーザーが **[テキストボックス](/uwp/api/Windows.UI.Xaml.Controls.TextBox)** や **[RichEditBox](/uwp/api/Windows.UI.Xaml.Controls.RichEditBox)** などの編集可能な入力フィールドをタップしたときに呼び出されます。 ユーザーが入力するデータの種類に合わせてテキストコントロールの *入力スコープ* を設定することにより、ユーザーがアプリにデータを入力する速度を大幅に向上させることができます。 入力値の種類は、システムに対してコントロールが予期しているテキスト入力の種類のヒントとなるため、システムはその入力の種類用の特殊なタッチ キーボード レイアウトを提供できます。

たとえば、テキストボックスが4桁の PIN を入力するためだけに使用されている場合は、 [**Inputscope**](/uwp/api/windows.ui.xaml.controls.textbox.inputscope) プロパティを **Number**に設定します。 これにより、システムに数字キーパッド レイアウトの表示が指示されるため、ユーザーは簡単に PIN を入力できます。

> [!IMPORTANT]
> - この情報は、SIP にのみ適用されます。 ハードウェア キーボードにも、Windows の簡単操作オプションで使用できるスクリーン キーボードにも適用されません。
> - 入力スコープの設定によって、入力の検証が実行されるわけではありません。また、ユーザーが、ハードウェア キーボードやその他の入力デバイスから入力できなくなることもありません。 必要に応じて、コードで入力を検証する必要があります。

## <a name="changing-the-input-scope-of-a-text-control"></a>テキスト コントロールの入力値の種類を変更する

アプリで使用可能な入力値の種類は、**[InputScopeNameValue](/uwp/api/Windows.UI.Xaml.Input.InputScopeNameValue)** 列挙体のメンバーです。 **[TextBox](/uwp/api/Windows.UI.Xaml.Controls.TextBox)** または **[RichEditBox](/uwp/api/Windows.UI.Xaml.Controls.RichEditBox)** の **InputScope** プロパティを、これらの値のいずれかに設定できます。

> [!IMPORTANT]
> **[Passwordbox](/uwp/api/Windows.UI.Xaml.Controls.PasswordBox)** の**[inputscope](/uwp/api/windows.ui.xaml.controls.passwordbox.inputscope)** プロパティでサポートされるのは、**パスワード**と**numericpin**の値のみです。 それ以外の値はすべて無視されます。

ここでは、各テキスト ボックスで予期されるデータと一致するように、いくつかのテキスト ボックスの入力値の種類を変更します。

**XAML の入力値の種類を変更するには**

1.  ページの XAML ファイルで、変更するテキスト コントロールのタグを見つけます。
2.  [**InputScope**](/uwp/api/windows.ui.xaml.controls.textbox.inputscope) 属性をタグに追加し、予期される入力に一致する [**InputScopeNameValue**](/uwp/api/Windows.UI.Xaml.Input.InputScopeNameValue) 値を指定します。

    次に示すのは、一般的な顧客の連絡先フォームに表示されるテキスト ボックスです。 [**InputScope**](/uwp/api/windows.ui.xaml.controls.textbox.inputscope) を設定すると、各テキスト ボックスに、データに適切なレイアウトのタッチ キーボードが表示されます。

    ```xaml
    <StackPanel Width="300">
        <TextBox Header="Name" InputScope="Default"/>
        <TextBox Header="Email Address" InputScope="EmailSmtpAddress"/>
        <TextBox Header="Telephone Number" InputScope="TelephoneNumber"/>
        <TextBox Header="Web site" InputScope="Url"/>
    </StackPanel>
    ```

**コードの入力値の種類を変更するには**

1.  ページの XAML ファイルで、変更するテキスト コントロールのタグを見つけます。 設定されていない場合は、[x: Name 属性](../../xaml-platform/x-name-attribute.md) を設定します。これで、コード内でコントロールを参照できます。

    ```csharp
    <TextBox Header="Telephone Number" x:Name="phoneNumberTextBox"/>
    ```

2.  新しい [**InputScope**](/uwp/api/Windows.UI.Xaml.Input.InputScope) オブジェクトをインスタンス化します。

    ```csharp
    InputScope scope = new InputScope();
    ```

3.  新しい [**InputScopeName**](/uwp/api/Windows.UI.Xaml.Input.InputScopeName) オブジェクトをインスタンス化します。
    
    ```csharp
    InputScopeName scopeName = new InputScopeName();
    ```

4.  [**InputScopeName**](/uwp/api/Windows.UI.Xaml.Input.InputScopeName) オブジェクトの [**NameValue**](/uwp/api/windows.ui.xaml.input.inputscopename.namevalue) プロパティを [**InputScopeNameValue**](/uwp/api/Windows.UI.Xaml.Input.InputScopeNameValue) 列挙体の値に設定します。

    ```csharp
    scopeName.NameValue = InputScopeNameValue.TelephoneNumber;
    ```

5.  [**InputScope**](/uwp/api/Windows.UI.Xaml.Input.InputScope) オブジェクトの [**Names**](/uwp/api/windows.ui.xaml.input.inputscope.names) コレクションに [**InputScopeName**](/uwp/api/Windows.UI.Xaml.Input.InputScopeName) オブジェクトを追加します。

    ```csharp
    scope.Names.Add(scopeName);
    ```

6.  [**InputScope**](/uwp/api/Windows.UI.Xaml.Input.InputScope) オブジェクトを、テキスト コントロールの [**InputScope**](/uwp/api/windows.ui.xaml.controls.textbox.inputscope) プロパティの値として設定します。

    ```csharp
    phoneNumberTextBox.InputScope = scope;
    ```

コード全体を次に示します。

```CSharp
InputScope scope = new InputScope();
InputScopeName scopeName = new InputScopeName();
scopeName.NameValue = InputScopeNameValue.TelephoneNumber;
scope.Names.Add(scopeName);
phoneNumberTextBox.InputScope = scope;
```

同じ手順を、次の短縮形のコードにまとめることもできます。

```CSharp
phoneNumberTextBox.InputScope = new InputScope() 
{
    Names = {new InputScopeName(InputScopeNameValue.TelephoneNumber)}
};
```

## <a name="text-prediction-spell-checking-and-auto-correction"></a>予測入力、スペル チェック、および自動修正

[**TextBox**](/uwp/api/Windows.UI.Xaml.Controls.TextBox) コントロールと [**RichEditBox**](/uwp/api/Windows.UI.Xaml.Controls.RichEditBox) コントロールには、SIP の動作に影響を与えるプロパティがいくつかあります。 ユーザーに最適なエクスペリエンスを提供するには、これらのプロパティが、タッチ操作を使用したテキスト入力に与える影響を理解しておく必要があります。

-   [**IsSpellCheckEnabled**](/uwp/api/windows.ui.xaml.controls.textbox.isspellcheckenabled): テキスト コントロールでスペル チェックが有効である場合、コントロールは、システムのスペル チェック エンジンと連携して、認識されない単語をマークします。 単語をタップすると、修正候補の一覧を表示できます。 スペル チェック オプションは既定で有効になっています。

    入力値の種類が **Default** である場合、このプロパティを使用すると、文字列を入力するときに、文の最初の単語に対する大文字の自動設定および単語の自動修正が有効になります。 これらの自動修正機能は、他の入力値の種類では無効である場合があります。 詳しくは、このトピックの後半にある表をご覧ください。

-   [**IsTextPredictionEnabled**](/uwp/api/windows.ui.xaml.controls.textbox.istextpredictionenabled): テキスト コントロールで予測入力が有効である場合は、入力開始時に予測される単語の一覧が表示されます。 一覧から選択できるため、単語全体を入力しなくても済みます。 予測入力は既定で有効になっています。

    入力値の種類が **Default** 以外のとき、[**IsTextPredictionEnabled**](/uwp/api/windows.ui.xaml.controls.textbox.istextpredictionenabled) プロパティが **true** であっても、予測入力が無効になる場合があります。 詳しくは、このトピックの後半にある表をご覧ください。

-   [**PreventKeyboardDisplayOnProgrammaticFocus**](/uwp/api/windows.ui.xaml.controls.textbox.preventkeyboarddisplayonprogrammaticfocus): このプロパティが **true** の場合は、プログラムによりフォーカスがテキスト コントロールに設定されていると、SIP が表示されなくなります。 代わりに、ユーザーがコントロールで操作するときにだけ、キーボードが表示されます。

## <a name="touch-keyboard-index-for-windows"></a>Windows のタッチキーボードインデックス

これらの表は、一般的な入力スコープ値の Windows ソフト入力パネル (SIP) のレイアウトを示しています。 **IsSpellCheckEnabled** プロパティや **IsTextPredictionEnabled** プロパティによって有効になっている機能に対する入力値の種類の影響が、入力値の種類ごとに説明されています。 これは、利用できる入力値の種類をすべて示したものではありません。

> [!Tip] 
> **&123**キーを押して数字と記号のレイアウトに変更し、 **abcd**キーを押してアルファベットレイアウトに変更することにより、アルファベットレイアウトと数字と記号の間でほとんどのタッチキーボードを切り替えることができます。

### <a name="default"></a>Default

`<TextBox InputScope="Default"/>`

既定の Windows タッチキーボード。

![既定の Windows タッチ キーボード](images/input-scopes/default.png)
- スペルチェック: IsSpellCheckEnabled が**IsSpellCheckEnabled**  =  **true**の場合は有効、 **IsSpellCheckEnabled**  =  **false**の場合は無効
- 自動修正: **IsSpellCheckEnabled**が  =  **true**の場合は enabled、 **IsSpellCheckEnabled**  =  **false**の場合は無効
- 自動大文字小文字の自動設定: **IsSpellCheckEnabled**が true の場合に有効になり  =  **true**、 **IsSpellCheckEnabled**  =  **false**の場合は無効になります
- テキスト予測: IsTextPredictionEnabled が**IsTextPredictionEnabled**  =  **true**の場合に有効、 **IsTextPredictionEnabled**  =  **false**の場合は無効

### <a name="currencyamountandsymbol"></a>CurrencyAmountAndSymbol

`<TextBox InputScope="CurrencyAmountAndSymbol"/>`

既定の数字と記号のキーボード レイアウト。

![通貨用 Windows タッチ キーボード](images/input-scopes/currencyamountandsymbol.png)

- より多くのシンボルを表示するためのページ左/右キーが含まれます
- スペル チェック: 既定では有効だが、無効にすることも可能
- 自動修正: 既定では有効だが、無効にすることも可能
- 大文字の自動設定: 常に無効
- 予測入力: 既定では有効だが、無効にすることも可能
 
### <a name="url"></a>url

`<TextBox InputScope="Url"/>`

![URL 用 Windows タッチ キーボード](images/input-scopes/url.png)

- **.com** キーと ![Go キー](images/input-scopes/kbdgokey.png) (Go) キーがあります。 .Com キーを押しながら、追加のオプション (**.org**、 **.net**、リージョン固有のサフィックス) を表示し**ます。**
- には、、、およびの各キーが含まれ**ます。** **-** **/**
- スペル チェック: 既定では無効だが、有効にすることも可能
- 自動修正: 既定では無効だが、有効にすることも可能
- 大文字の自動設定: 既定では無効だが、有効にすることも可能
- 予測入力: 既定では無効だが、有効にすることも可能


### <a name="emailsmtpaddress"></a>EmailSmtpAddress

`<TextBox InputScope="EmailSmtpAddress"/>`

![メール アドレス用の Windows タッチ キーボード](images/input-scopes/emailsmtpaddress.png)
- には、 **@** および **.com** キーが含まれています。 .Com キーを押しながら、追加のオプション (**.org**、 **.net**、リージョン固有のサフィックス) を表示し**ます。**
- **_** とキーが含まれます。 **-**
- スペル チェック: 既定では無効だが、有効にすることも可能
- 自動修正: 既定では無効だが、有効にすることも可能
- 大文字の自動設定: 既定では無効だが、有効にすることも可能
- 予測入力: 既定では無効だが、有効にすることも可能


### <a name="number"></a>Number

`<TextBox InputScope="Number"/>`

![電話番号用 Windows タッチ キーボード](images/input-scopes/number.png)
- スペル チェック: 既定では有効だが、無効にすることも可能
- 自動修正: 既定では有効だが、無効にすることも可能
- 大文字の自動設定: 常に無効
- 予測入力: 既定では有効だが、無効にすることも可能

### <a name="telephonenumber"></a>TelephoneNumber

`<TextBox InputScope="TelephoneNumber"/>`

![電話番号用 Windows タッチ キーボード](images/input-scopes/telephonenumber.png)
- スペル チェック: 既定では有効だが、無効にすることも可能
- 自動修正: 既定では有効だが、無効にすることも可能
- 大文字の自動設定: 常に無効
- 予測入力: 既定では有効だが、無効にすることも可能

### <a name="search"></a>検索

`<TextBox InputScope="Search"/>`

![検索用 Windows タッチ キーボード](images/input-scopes/search.png)
- **Enter**キーの代わりに**検索**キーを含めます。
- スペル チェック: 既定では有効だが、無効にすることも可能
- 自動修正: 既定では有効だが、無効にすることも可能
- 大文字の自動設定: 常に無効
- 予測入力: 既定では有効だが、無効にすることも可能

### <a name="searchincremental"></a>SearchIncremental

`<TextBox InputScope="SearchIncremental"/>`

![インクリメンタル検索用の Windows タッチキーボード](images/input-scopes/searchincremental.png)
- **既定**と同じレイアウト
- スペル チェック: 既定では無効だが、有効にすることも可能
- 自動修正: 常に無効
- 大文字の自動設定: 常に無効
- 予測入力: 常に無効

### <a name="formula"></a>計算式

`<TextBox InputScope="Formula"/>`

![数式用の Windows タッチキーボード](images/input-scopes/formula.png)
- キーを含む **=**
- また、、 **%** **$** 、およびの各 **+** キーも含まれます。
- スペル チェック: 既定では有効だが、無効にすることも可能
- 自動修正: 既定では有効だが、無効にすることも可能
- 大文字の自動設定: 常に無効
- 予測入力: 既定では有効だが、無効にすることも可能

### <a name="chat"></a>チャット

`<TextBox InputScope="Chat"/>`

![既定の Windows タッチ キーボード](images/input-scopes/default.png)
- **既定**と同じレイアウト
- スペル チェック: 既定では有効だが、無効にすることも可能
- 自動修正: 既定では有効だが、無効にすることも可能
- 大文字の自動設定: 既定では有効だが、無効にすることも可能
- 予測入力: 既定では有効だが、無効にすることも可能

### <a name="nameorphonenumber"></a>NameOrPhoneNumber

`<TextBox InputScope="NameOrPhoneNumber"/>`

![既定の Windows タッチ キーボード](images/input-scopes/default.png)
- **既定**と同じレイアウト
- スペル チェック: 既定では無効だが、有効にすることも可能
- 自動修正: 既定では無効だが、有効にすることも可能
- 自動大文字小文字の自動設定: 既定では、有効にすることができます (各単語の最初の文字は大文字になります)。
- 予測入力: 既定では無効だが、有効にすることも可能