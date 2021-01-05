---
title: 角の半径
description: 角の丸めの原則、デザイン方法、カスタマイズ オプションについて説明します。
ms.date: 10/08/2019
ms.topic: article
keywords: windows 10, uwp, 角の半径, 丸め
ms.openlocfilehash: 33432ac0083c0d6660d0669ea43805e0ae73f37e
ms.sourcegitcommit: 4cafc1c55511741dd1e5bfe4496d9950a9b4de1b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/04/2021
ms.locfileid: "97860196"
---
# <a name="corner-radius"></a>角の半径

[Windows UI ライブラリ](/uwp/toolkits/winui/) (WinUI) のバージョン 2.2 から、多くのコントロールで既定のスタイルが更新され、角の丸めを使用するようになりました。 これらの新しいスタイルの目的は、暖かみと安心感を与え、ユーザーにとって UI を視覚的に扱いやすくすることです。

次に示している 2 つのボタン コントロールで、1 つ目は角を丸めず、2 つ目は新しい角を丸めたスタイルを使用しています。

![角を丸めないボタンと丸めたボタン](images/rounded-corner/my-button.png)

WinUI 2.2 以降の NuGet パッケージをインストールすると、WinUI コントロールとプラットフォーム コントロールの両方で、新しい既定のスタイルがインストールされます。 アプリで WinUI 2.2 を使用すると、これらのスタイルが自動的に使用されます。新しいスタイルを使用するために、特に何かをする必要はありません。 ただし、この記事の後半では、必要に応じて角の丸めをカスタマイズする方法について説明します。

> [!IMPORTANT]
> **TreeView** や **ColorPicker** などの一部のコントロールは、プラットフォーム ([Windows.UI.Xaml.Controls](/uwp/api/windows.ui.xaml.controls)) と WinUI ([Microsoft.UI.Xaml.Controls](/uwp/api/microsoft.ui.xaml.controls?view=winui-2.2&preserve-view=true)) の両方で使用できます。 アプリで WinUI を使用するときは、コントロールの WinUI バージョンを使用することをお勧めします。 WinUI と共に使用すると、プラットフォーム バージョンでは角の丸めが一貫して適用されない場合があります。

> **重要な API**:[Control.CornerRadius プロパティ](/uwp/api/windows.ui.xaml.controls.control.cornerradius)

## <a name="default-control-designs"></a>既定のコントロール デザイン

コントロールで角の丸めスタイルが使用される 3 つの領域は、四角形要素、フライアウト要素、バー要素です。

### <a name="corners-of-rectangle-ui-elements"></a>四角形 UI 要素の角

- これらの UI 要素には、ユーザーがいつも画面上で目にするボタンなどの基本的なコントロールが含まれます。
- これらの UI 要素に使用する既定の半径値は **2 px** です。

![丸めた角を強調したボタン](images/rounded-corner/button.png)

**コントロール**

- AutoSuggestBox
- ボタン
  - ContentDialog のボタン
- CalendarDatePicker
- チェック ボックス
  - TreeView の複数選択チェック ボックス
- ComboBox
- DatePicker
- DropDownButton
- FlipView
- PasswordBox
- RichEditBox
- SplitButton
- TextBox
- TimePicker
- ToggleButton
- ToggleSplitButton

### <a name="corners-of-flyout-and-overlay-ui-elements"></a>フライアウト UI 要素とオーバーレイ UI 要素の角

- これらは、MenuFlyout のように一時的に画面に表示される一時 UI 要素の場合もあれば、TabView のタブのように他の UI に重なる要素の場合もあります。
- これらの UI 要素に使用する既定の半径値は **4 px** です。

![フライアウトの例](images/rounded-corner/flyout.png)

**コントロール**

- CommandBarFlyout
- ContentDialog
- ポップアップ
- MenuFlyout
- TabView のタブ
- TeachingTip
- ToolTip
- フライアウト部品 (開いているとき)
  - AutoSuggestBox
  - CalendarDatePicker
  - ComboBox
  - DatePicker
  - DropDownButton
  - MenuBar
  - SplitButton
  - TimePicker
  - ToggleSplitButton

### <a name="bar-elements"></a>バー要素

- これらの UI 要素は棒や線のような形をしており、ProgressBar などの例があります。
- ここで使用する既定の半径値は **2 px** です。

![進行状況バーの例](images/rounded-corner/bars.png)

**コントロール**

- NavigationView 選択インジケーター
- ピボット選択インジケーター
- ProgressBar
- ScrollBar (`IndicatorMode=TouchIndicator` のとき)
- スライダー
  - ColorPicker 色スライダー
  - MediaTransportControls シーク バー スライダー

## <a name="customization-options"></a>カスタマイズ オプション

ここで説明している既定の角の半径値は固定ではなく、いくつかの方法で角の丸め量を簡単に変更できます。 これは、必要なカスタマイズのきめ細かさに応じて、2 つのグローバル リソースを使用して、または [CornerRadius](/uwp/api/windows.ui.xaml.controls.control.cornerradius) プロパティを使用してコントロールに対して直接、実行できます。

### <a name="when-not-to-round"></a>丸めない場合

コントロールの角を丸めることが望ましくない状況があり、そのような状況では、丸めは既定では適用されません。

- SplitButton の 2 つの部品のように、コンテナーに格納されている複数の UI 要素が互いに接している場合。 接している場合、スペースがありません。

![SplitButton](images/rounded-corner/split-button-2.png)

- コントロールが別のコンテナーに格納されている場合。たとえば、ScrollBar のバーとボタンが ScrollBar コンテナーの一部であり、ScrollBar コンテナーがさらに ScrollViewer の一部である場合。

![角が丸められていない垂直スクロール バーのスクリーンショット。](images/rounded-corner/scrollbar.png)

- フライアウト UI 要素が、1 つの端で、そのフライアウトを呼び出す UI と接している場合。

![一部の角が丸められていない AutoSuggest ポップアップのスクリーンショット。](images/rounded-corner/autosuggest.png)

### <a name="keyboard-focus-rectangle-and-shadow"></a>キーボード フォーカスの四角形と影

既定のデザインでは、キーボード フォーカスの四角形の角を丸めたり、影を制御したりするための特別な処理は行われません。 角の半径値を大きくしても機能が損なわれることはありませんが、値を大きくすると意図せず表示が崩れることがあり、そのような事態を避けるため、この点に注意することをお勧めします。

次に示すのは、角の半径を大きくすると影の見た目が不自然になる例です。

![ContentDialogShadow](images/rounded-corner/larger-corner-radius.png)

### <a name="rounded-corners-and-performance"></a>角の丸めとパフォーマンス

角の丸めのレンダリングには、当然ながら、直角の角のレンダリングよりも多くの描画能力が使用されます。 角の半径の既定値を決めるにあたっては、デザインの原則を考慮するだけでなく、アプリで既定のコントロールを使用したときのパフォーマンスが良好になるように配慮しました。

この文脈でアプリのパフォーマンスを考えるとき、主に考慮する必要があるのは、ページの読み込み時間とアプリの起動時間です。 UI サーフェスが広くなるほど、角の丸めがパフォーマンスに及ぼす影響も大きくなる点を考慮してください。 全画面表示アプリの UI では、丸めた角を描画しないようにしてください。 ContentDialog のように、UI が短時間、ページの読み込み後に表示される場合、このことはあまり問題になりません。

### <a name="page-or-app-wide-cornerradius-changes"></a>ページまたはアプリ全体での CornerRadius の変更

すべてのコントロールの角の半径を制御する 2 つのアプリ リソースがあります。

- `ControlCornerRadius` - 既定値は 2 px です。
- `OverlayCornerRadius` - 既定値は 4 px です。

いずれかのスコープでこれらのリソースの値をオーバーライドすると、そのスコープ内のすべてのコントロールが変更の影響を受けます。

つまり、丸みを適用できるすべてのコントロールの丸みを変更したい場合、次のように、新しい CornerRadius の値を使用して、アプリ レベルで両方のリソースを定義することができます。

```xaml
<Application.Resources>
    <ResourceDictionary>
        <ResourceDictionary.MergedDictionaries>
            <XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls" />
            <ResourceDictionary>
                <CornerRadius x:Key="OverlayCornerRadius">0</CornerRadius>
                <CornerRadius x:Key="ControlCornerRadius">0</CornerRadius>
            </ResourceDictionary>
        </ResourceDictionary.MergedDictionaries>
    </ResourceDictionary>
</Application.Resources>
```

また、ページ レベルやコンテナー レベルなど、特定のスコープ内ですべてのコントロールの丸みを変更したい場合、同様のパターンに従うことができます。

```xaml
<Grid>
    <Grid.Resources>
        <CornerRadius x:Key="ControlCornerRadius">8</CornerRadius>
    </Grid.Resources>
    <Button Content="Button"/>
</Grid>
```

> [!NOTE]
> `OverlayCornerRadius` リソースを有効にするためには、アプリ レベルでこのリソースを定義する必要があります。
>
>なぜなら、ポップアップとフライアウトは動的であり、ビジュアル ツリーのルート要素に作成されるため、それらが使用するすべてのリソースもそこに定義する必要があるからです。 そのようにしないと、それらはスコープ外になります。

### <a name="per-control-cornerradius-changes"></a>コントロール単位での CornerRadius の変更

選択したいくつかのコントロールの丸みだけを変更したい場合、対象のコントロールの [CornerRadius](/uwp/api/windows.ui.xaml.controls.control.cornerradius) プロパティを直接変更することができます。

|既定 | 変更されたプロパティ |
|:-- |:-- |
|![DefaultCheckBox](images/rounded-corner/default-checkbox.png)| ![CustomCheckBox](images/rounded-corner/custom-checkbox.png)|
|`<CheckBox Content="Checkbox"/>` | `<CheckBox Content="Checkbox" CornerRadius="5"/> ` |

コントロールの `CornerRadius` プロパティを変更しても、すべてのコントロールの角に効果が発揮されるわけではありません。 角を丸めたいコントロールについて、`CornerRadius` プロパティの効果が実際に、また期待どおりに発揮されるようにするには、まず、`ControlCornerRadius` または `OverlayCornerRadius` グローバル リソースが対象のコントロールに影響を及ぼすことを確認します。 影響が及ばない場合、丸めたいコントロールに実際に角があることを確認してください。 コントロールの多くは実際の境界をレンダリングせず、したがって `CornerRadius` プロパティを適切に使用できません。

### <a name="basing-custom-styles-on-winui"></a>WinUI でのカスタム スタイルの基本

スタイルに適切な `BasedOn` 属性を指定することで、WinUI の角を丸めたスタイルに基づいてカスタム スタイルを作成できます。 たとえば、WinUI ボタン スタイルに基づいてカスタム ボタン スタイルを作成するには、次の手順を行います。

```xaml
<Style x:Key="MyCustomButtonStyle" BasedOn="{StaticResource DefaultButtonStyle}">
   ...
</Style>
```

通常、WinUI コントロール スタイルでは、次のような一貫した名前付け規則に従います: "DefaultXYZStyle"。この場合、"XYZ" はコントロールの名前です。 完全参照の場合は、WinUI リポジトリ内の XAML ファイルを参照できます。
