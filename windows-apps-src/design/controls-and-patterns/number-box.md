---
description: 数値ボックスは、数値の表示と編集に使用できるコントロールです。
title: 数値ボックス
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 4bba2f90d7240b454c4dbd5331251e306f946af7
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2020
ms.locfileid: "93030915"
---
# <a name="number-box"></a>数値ボックス

数値の表示と編集に使用できるコントロールを表します。 検証、増分ステップ、乗算、除算、加算、減算などの基本的な式のインライン計算をサポートしています。

**Windows UI ライブラリを入手する**

:::row:::
   :::column:::
      ![WinUI ロゴ](images/winui-logo-64x64.png)
   :::column-end:::
   :::column span="3":::
      **NumberBox** コントロールでは、Windows アプリのための新しいコントロールと UI 機能を含む NuGet パッケージである Windows UI ライブラリが必要になります。 インストール手順などの詳細については、[Windows UI ライブラリの概要](/uwp/toolkits/winui/)に関するページを参照してください。
   :::column-end:::
   :::column:::

   :::column-end:::
:::row-end:::

**Windows UI ライブラリ API:** [NumberBox クラス](/uwp/api/microsoft.ui.xaml.controls.NumberBox)

> [!TIP]
> このドキュメントでは、XAML で **muxc** エイリアスを使って、プロジェクトに含めた Windows UI Library API を表します。 [Page](/uwp/api/windows.ui.xaml.controls.page) 要素にこれを追加しました。`xmlns:muxc="using:Microsoft.UI.Xaml.Controls"`
>
>コードビハインドでは、C# でも **muxc** エイリアスを使って、プロジェクトに含めた Windows UI Library API を表します。 この **using** ステートメントは、ファイルの先頭に追加されています。`using muxc = Microsoft.UI.Xaml.Controls;`

## <a name="is-this-the-right-control"></a>これは適切なコントロールですか?

NumberBox コントロールを使用して、数学的な入力を捕捉および表示することができます。 数値以外も受け付ける編集可能なテキスト ボックスが必要な場合は、[TextBox](/uwp/api/Windows.UI.Xaml.Controls.TextBox) コントロールを使用します。 パスワードやその他の機密情報の入力を受け付ける編集可能なテキスト ボックスが必要な場合は、[PasswordBox](/uwp/api/windows.ui.xaml.controls.passwordbox) を参照してください。 検索語句を入力するためのテキスト ボックスが必要な場合は、[AutoSuggestBox](/uwp/api/windows.ui.xaml.controls.autosuggestbox) を参照してください。 書式を適用したテキストを入力または編集する必要がある場合は、[RichEditBox](/uwp/api/windows.ui.xaml.controls.richeditbox) を参照してください。

## <a name="examples"></a>例

<table>
<th align="left">XAML コントロール ギャラリー<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML コントロール ギャラリー</strong> アプリがインストールされている場合、こちらをクリックして<a href="xamlcontrolsgallery:/item/TextBox">アプリを開き、NumberBox の動作を確認</a>してください。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML コントロール ギャラリー アプリを入手する (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">ソース コード (GitHub) を入手する</a></li>
    </ul>
</td>
</tr>
</table>

### <a name="create-a-simple-numberbox"></a>単純な NumberBox の作成

次に示すのは、既定の外観を示す基本的な NumberBox の XAML です。 ユーザーに表示されるデータと、アプリに保存されているデータの同期が保たれていることを保証するには、[x:Bind](../../xaml-platform/x-bind-markup-extension.md#property-path) を使用します。

```xaml
<muxc:NumberBox Value="{x:Bind Path=ViewModel.NumberBoxValue, Mode=TwoWay}" />
```

![フォーカスがある入力フィールド。0 を示している。](images/numberbox-basic.png)

### <a name="labeling-numberbox"></a>NumberBox のラベル付け

NumberBox の目的が明確でない場合は、`Header` または `PlaceholderText` を使用します。 `Header` は、NumberBox に値が存在するかどうかに関係なく表示されます。

```xaml
<muxc:NumberBox Header="Enter a number:"
    Value="{x:Bind Path=ViewModel.NumberBoxValue, Mode=TwoWay}" />
```

![NumberBox の上の "Enter expression:" (式を入力してください) という見出し。](images/numberbox-header.png)

`PlaceholderText` は NumberBox 内に表示され、`Value` が NaN に設定されているとき、または入力がユーザーによってクリアされたときにのみ表示されます。

```xaml
<muxc:NumberBox PlaceholderText="1+2^2"
    Value="{x:Bind Path=ViewModel.NumberBoxValue, Mode=TwoWay}" />
```

!["A + B" というプレースホルダー テキストを含む NumberBox。](images/numberbox-placeholder-text.png)

### <a name="enable-calculation-support"></a>計算のサポートの有効化

`AcceptsExpression` プロパティを true に設定すると、NumberBox で、標準の演算順序を使用して、乗算、除算、加算、減算などの基本的なインライン式を評価できるようになります。 評価は、フォーカスが失われたとき、またはユーザーが "Enter" キーを押したときにトリガーされます。 式が評価された後、式の元の形式は保持されません。

```xaml
<muxc:NumberBox Value="{x:Bind Path=ViewModel.NumberBoxValue, Mode=TwoWay}"
    AcceptsExpression="True" />
```

### <a name="increment-and-decrement-stepping"></a>増分および減分ステップ

NumberBox にフォーカスがあり、ユーザーが次の操作を行ったときに NumberBox 内の値がどれだけ変動するかを構成するには、`SmallChange` プロパティを使用します。

- スクロールする
- 上方向キーを押す
- 下方向キーを押す

NumberBox にフォーカスがあり、ユーザーが PageUp または PageDown キーを押したときに NumberBox 内の値がどれだけ変動するかを構成するには、`LargeChange` プロパティを使用します。

クリックすると `SmallChange` プロパティで指定された量だけ NumberBox の値を増減できるボタンを有効にするには、`SpinButtonPlacementMode` プロパティを使用します。 次のステップで最大値または最小値を超える場合、これらのボタンは無効になります。

コントロールの横にボタンを表示するには、`SpinButtonPlacementMode` を `Inline` に設定します。

```xaml
<muxc:NumberBox Value="{x:Bind Path=ViewModel.NumberBoxValue, Mode=TwoWay}"
    SmallChange="10"
    LargeChange="100"
    SpinButtonPlacementMode="Inline" />
```

![下方向ボタンと上方向ボタンが横に付いた NumberBox。](images/numberbox-spinbutton-inline.png)

NumberBox にフォーカスがあるときにのみフライアウト形式でボタンを表示するには、`SpinButtonPlacementMode` を `Compact` に設定します。

```xaml
<muxc:NumberBox Value="{x:Bind Path=ViewModel.NumberBoxValue, Mode=TwoWay}"
    SmallChange="10"
    LargeChange="100"
    SpinButtonPlacementMode="Compact" />
```

![内部の小さなアイコンに上向きと下向きの矢印を表示した NumberBox。](images/numberbox-spinbutton-compact-non-visible.png)

![フローティング表示の上方向ボタンと下方向ボタンを手前のレイヤーに併置した NumberBox。](images/numberbox-spinbutton-compact-visible.png)

### <a name="enabling-input-validation"></a>入力検証の有効化

`ValidationMode` を `InvalidInputOverwritten` に設定すると、NumberBox がフォーカスを失うか "Enter" キーが押されるかして評価がトリガーされたときに、数値でないか式が正しくないため無効である入力を最後の有効な値で上書きすることができます。

```xaml
<muxc:NumberBox Header="Quantity"
    Value="{x:Bind Path=ViewModel.NumberBoxValue, Mode=TwoWay}"
    ValidationMode="InvalidInputOverwritten" />
```

`ValidationMode` を `Disabled` に設定すると、カスタム入力検証を構成できます。

小数点とコンマに関しては、ユーザーが使用する書式設定は、NumberBox 用に構成された書式によって置き換えられます。 入力検証エラーはトリガーされません。

### <a name="formatting-input"></a>入力の書式設定

[数値の書式](/uwp/api/windows.globalization.numberformatting)を使用すると、書式クラスのインスタンスを構成して `NumberFormatter` プロパティに割り当てることによって、数値ボックスの値を書式設定することができます。 10 進数、通貨、パーセント、有効数字は、使用できる数値書式クラスの例です。 丸めも数値の書式プロパティによって定義されることに注意してください。

次の例では、DecimalFormatter を使用して、NumberBox の値を整数部 1 桁と小数部 2 桁に書式設定し、最も近い 0.25 の倍数に丸めます。

```xaml
<muxc:NumberBox  x:Name="FormattedNumberBox"
    Value="{x:Bind Path=ViewModel.NumberBoxValue, Mode=TwoWay}" />
```

```csharp
private void SetNumberBoxNumberFormatter()
{
    IncrementNumberRounder rounder = new IncrementNumberRounder();
    rounder.Increment = 0.25;
    rounder.RoundingAlgorithm = RoundingAlgorithm.RoundUp;

    DecimalFormatter formatter = new DecimalFormatter();
    formatter.IntegerDigits = 1;
    formatter.FractionDigits = 2;
    formatter.NumberRounder = rounder;
    FormattedNumberBox.NumberFormatter = formatter;
}
```

![NumberBox の値は 0.00 です。](images/numberbox-formatted.png)

小数点とコンマに関しては、ユーザーが使用する書式設定は、NumberBox 用に構成された書式によって置き換えられます。 入力検証エラーはトリガーされません。

## <a name="remarks"></a>コメント

### <a name="input-scope"></a>入力スコープ

`Number` は[入力スコープ](/uwp/api/Windows.UI.Xaml.Input.InputScopeNameValue)に使用されます。 この入力スコープは 0 ～ 9 の数字を操作するためのものです。 これは上書きできますが、代替の InputScope 型は明示的にサポートされません。

### <a name="not-a-number"></a>数値がない

NumberBox の入力がクリアされると、数値が存在しないことを示すために `Value` が `NaN` に設定されます。

### <a name="expression-evaluation"></a>式の評価

NumberBox では、中置記法を使用して式を評価します。 使用できる演算子は、優先順位の順に次のとおりです。

* ^
* */
* +-

かっこを使用して優先順位規則をオーバーライドできることに注意してください。

## <a name="recommendations"></a>推奨事項

* `Text` および `Value` を使用すると、NumberBox の値を String または Double として簡単に捕捉でき、値の型変換は必要ありません。 NumberBox の値をプログラムで変更するときは、`Value` プロパティを使用して変更することをお勧めします。 `Value` は初期設定で `Text` を上書きします。 初期設定後、一方への変更はもう一方に反映されますが、プログラムによる変更には常に `Value` を使用するようにすれば、NumberBox が `Text` 経由で数字以外の文字を受け付けるという概念的な誤解を避けることができます。
* `Header` または `PlaceholderText` を使用して、NumberBox が数字のみを入力として受け付けることをユーザーに知らせます。 "one" のようにつづられた数字表現は、許容値に解決されません。
