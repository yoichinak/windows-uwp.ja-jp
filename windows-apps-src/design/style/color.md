---
description: UWP アプリでアクセント カラーとテーマを使用する方法について説明します。
title: UWP アプリでの色使い
ms.date: 04/07/2019
ms.topic: article
keywords: windows 10, uwp
design-contact: karenmui
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: e5cd8ecafd3557719e70c50890da4c3eade18f52
ms.sourcegitcommit: 139717a79af648a9231821bdfcaf69d8a1e6e894
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/10/2019
ms.locfileid: "67714135"
---
# <a name="color"></a>色

![ヒーロー イメージ](images/header-color.svg)

色は、アプリの中でユーザーに情報を伝える直感的な方法です。操作可能な要素の強調、ユーザー操作に対するフィードバックの提供、インターフェイスの連続感の演出を色によって行うことができます。

UWP アプリでの色使いは、主にアクセント カラーとテーマによって指定します。 この記事では、アプリで色を使用する方法と、アクセント カラーとテーマ リソースを使用して、任意のテーマに対応して UWP アプリを使用できるように設定する方法について説明します。

## <a name="color-principles"></a>色使いの原則

:::row:::
    :::column:::
        **Use color meaningfully.**
        When color is used sparingly to highlight important elements, it can help create a user interface that is fluid and intuitive.
    :::column-end:::
    :::column:::
        **Use color to indicate interactivity.**
        It's a good idea to choose one color to indicate elements of your application that are interactive. For example, many web pages use blue text to denote a hyperlink.
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        **Color is personal.**
        In Windows, users can choose an accent color and a light or dark theme, which are reflected throughout their experience. You can choose how to incorporate the user's accent color and theme into your application, personalizing their experience.
    :::column-end:::
    :::column:::
        **Color is cultural.**
        Consider how the colors you use will be interpreted by people from different cultures. For example, in some cultures the color blue is associated with virtue and protection, while in others it represents mourning.
    :::column-end:::
:::row-end:::

## <a name="themes"></a>テーマ

UWP アプリでは、淡色または濃色のアプリケーション テーマを使用できます。 テーマは、アプリの背景、テキスト、アイコン、[コモン コントロール](../controls-and-patterns/index.md) に反映されます。

### <a name="light-theme"></a>淡色テーマ

![淡色テーマ](images/color/light-theme.svg)

### <a name="dark-theme"></a>濃色テーマ

![濃色テーマ](images/color/dark-theme.svg)

既定では、UWP アプリのテーマは、ユーザーが Windows の設定で選択したテーマか、デバイスの既定のテーマ (XBox では黒など) に設定されます。 ただし、開発者が UWP アプリのテーマを設定することもできます。

### <a name="changing-the-theme"></a>テーマの変更

テーマを変更するには、`App.xaml` ファイルで **RequestedTheme** プロパティを変更します。

```XAML
<Application
    x:Class="App9.App"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:App9"
    RequestedTheme="Dark">
</Application>
```

**RequestedTheme** プロパティを削除すると、アプリケーションでユーザーのシステム設定が使用されるようになります。

ユーザーはまたハイ コントラスト テーマを使用することができます。これはインターフェイスを見やすくするために、コントラストの大きい、少数の色のパレットを使ったテーマです。 ハイ コントラスト テーマを使用した場合、開発者が設定した RequestedTheme はシステムによって上書きされます。

### <a name="testing-themes"></a>テーマのテスト

アプリのテーマを指定しない場合は、必ず淡色テーマと濃色テーマの両方でアプリをテストして、あらゆる条件でアプリが判読できることを確認します。

**注意**:Visual Studio では、RequestedTheme の既定値が淡色に設定されているため、両方をテストするには、RequestedTheme を変更する必要があります。

## <a name="theme-brushes"></a>テーマ ブラシ

コモン コントロールでは、[テーマ ブラシ](../controls-and-patterns/xaml-theme-resources.md#the-xaml-color-ramp-and-theme-dependent-brushes)が自動的に作動して、淡色テーマと濃色テーマのコントラストが調整されます。

たとえば、[AutoSuggestBox](../controls-and-patterns/auto-suggest-box.md) でテーマ ブラシが使用される方法を以下に示します。

![テーマ ブラシ コントロールの例](images/color/theme-brushes.svg)

テーマ ブラシは、以下の目的で使用されます。

- **Base** はテキストの設定に使用されます。
- **Alt** は、Base の反転色の設定に使用されます。
- **Chrome**  は、最上位の要素 (ナビゲーション ウィンドウやコマンド バーなど) の設定に使用されます。
- **List** は、リスト コントロールの設定に使用されます。

**Low**/**Medium**/**High** は色密度を指します。

### <a name="using-theme-brushes"></a>テーマ ブラシの使用

:::row:::
    :::column:::
        When creating templates for custom controls, use theme brushes rather than hard code color values. This way, your app can easily adapt to any theme.

        For example, these [item templates for ListView](../controls-and-patterns/item-templates-listview.md) demonstrate how to use theme brushes in a custom template.
    :::column-end:::
    :::column:::
         ![double line list item with icon example](images/color/list-view.svg)
    :::column-end:::
:::row-end:::

```xaml
<ListView ItemsSource="{x:Bind ViewModel.Recordings}">
    <ListView.ItemTemplate>
        <DataTemplate x:Name="DoubleLineDataTemplate" x:DataType="local:Recording">
            <StackPanel Orientation="Horizontal" Height="64" AutomationProperties.Name="{x:Bind CompositionName}">
                <Ellipse Height="48" Width="48" VerticalAlignment="Center">
                    <Ellipse.Fill>
                        <ImageBrush ImageSource="Placeholder.png"/>
                    </Ellipse.Fill>
                </Ellipse>
                <StackPanel Orientation="Vertical" VerticalAlignment="Center" Margin="12,0,0,0">
                    <TextBlock Text="{x:Bind CompositionName}"  Style="{ThemeResource BaseTextBlockStyle}" Foreground="{ThemeResource SystemControlPageTextBaseHighBrush}" />
                    <TextBlock Text="{x:Bind ArtistName}" Style="{ThemeResource BodyTextBlockStyle}" Foreground="{ThemeResource SystemControlPageTextBaseMediumBrush}"/>
                </StackPanel>
            </StackPanel>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

アプリでテーマ ブラシを使用する方法について詳しくは、[テーマ リソースに関するページ](../controls-and-patterns/xaml-theme-resources.md)をご覧ください。

## <a name="accent-color"></a>アクセント カラー

コモン コントロールでは、アクセント カラーを使用して、状態情報を伝達します。 アクセント カラーは、既定では、ユーザーが設定で選択した `SystemAccentColor` が使用されます。 ただし、組織のブランドを反映するように、アプリのアクセント カラーをカスタマイズすることもできます。

![Windows コントロール](images/color/windows-controls.svg)

:::row:::
    :::column:::
        ![user-selected accent header](images/color/user-accent.svg)
        ![user-selected accent color](images/color/user-selected-accent.svg)
    :::column-end:::
    :::column:::
        ![custom accent header](images/color/custom-accent.svg)
        ![custom brand accent color](images/color/brand-color.svg)
    :::column-end:::
:::row-end:::

### <a name="overriding-the-accent-color"></a>アクセント カラーの上書き

アプリのアクセント カラーを変更するには、次のコードを `app.xaml` に追加します。

```xaml
<Application.Resources>
    <ResourceDictionary>
        <Color x:Key="SystemAccentColor">#107C10</Color>
    </ResourceDictionary>
</Application.Resources>
```

### <a name="choosing-an-accent-color"></a>アクセント カラーの選択

アプリに対してカスタムのアクセント カラーを選択した場合は、アクセント カラーを使用したテキストと背景との間に十分なコントラストがあり、テキストを適切に判読できることを確認してください。 コントラストをテストするには、Windows の設定でカラー ピッカー ツールを使用するか、これらの[オンライン コントラスト ツール](https://www.w3.org/TR/WCAG20-TECHS/G18.html#G18-resources)を使用します。

![Windows の設定のアクセント カラー ピッカー](images/color/color-picker.svg)

## <a name="accent-color-palette"></a>アクセント カラー パレット

Windows シェルのアクセント カラーのアルゴリズムによって、アクセント カラーの淡色と濃色の色調が生成されます。

![アクセント カラー パレット](images/color/accent-color-palette.svg)

これらの色調には、以下の[テーマ リソース](../controls-and-patterns/xaml-theme-resources.md)としてアクセスできます。

- `SystemAccentColorLight3`
- `SystemAccentColorLight2`
- `SystemAccentColorLight1`
- `SystemAccentColorDark1`
- `SystemAccentColorDark2`
- `SystemAccentColorDark3`

<!-- check this is true -->
また [**UISettings.GetColorValue**](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.UISettings#Windows_UI_ViewManagement_UISettings_GetColorValue_Windows_UI_ViewManagement_UIColorType_) メソッドと [**UIColorType**](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.UIColorType) 列挙型を使って、プログラムによってアクセント カラー パレットにアクセスすることもできます。

アクセント カラー パレットを使用して、アプリの色のテーマを設定できます。 以下では、ボタンに対してアクセント カラー パレットを使用する方法の例を示します。

![アクセント カラー パレットのボタンへの適用](images/color/color-theme-button.svg)

```xaml
<Page.Resources>
    <ResourceDictionary>
        <ResourceDictionary.ThemeDictionaries>
            <ResourceDictionary x:Key="Light">
                <SolidColorBrush x:Key="ButtonBackground" Color="{ThemeResource SystemAccentColor}"/>
                <SolidColorBrush x:Key="ButtonBackgroundPointerOver" Color="{ThemeResource SystemAccentColorLight1}"/>
                <SolidColorBrush x:Key="ButtonBackgroundPressed" Color="{ThemeResource SystemAccentColorDark1}"/>
            </ResourceDictionary>
        </ResourceDictionary.ThemeDictionaries>
    </ResourceDictionary>
</Page.Resources>

<Button Content="Button"></Button>
```

テキスト色と背景色の両方を設定する場合は、テキストと背景の間に十分なコントラストがあることを確認します。 既定では、ハイパーリンクまたはハイパーテキストにはアクセント カラーが使用されます。 背景にアクセント カラーのバリエーションを適用する場合は、元のアクセント カラーのバリエーションを使用して、背景色とテキスト色のコントラストを最適化します。

以下の表は、さまざまな色調のアクセント カラーと、色付きの表面上での文字色の見え方の例を示します。

![色調の組み合わせ](images/color/color-on-color.png)

コントロールのスタイルについて詳しくは、「[XAML スタイル](../controls-and-patterns/xaml-styles.md)」をご覧ください。

## <a name="color-api"></a>色の API

アプリケーションに色を追加できる API は複数存在します。 まず、[**Colors**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.colors) クラスを使用すると、多数の色があらかじめ定義された一覧を実装できます。 これらは、XAML プロパティを使用して自動的にアクセスできます。 以下の例では、ボタンを作成して、**Color** クラスのメンバーに背景色プロパティと前景色プロパティを設定しています。

```xaml
<Button Background="MediumSlateBlue" Foreground="White">Button text</Button>
```

XAML で [**Color**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.color) 構造体を使用すると、RGB または 16 進数値によって独自の色を作成できます。

```xaml
<Color x:Key="LightBlue">#FF36C0FF</Color>
```

また **FromArgb** メソッドを使用すると、コード内で同じ色を作成できます。

```csharp
Color LightBlue = Color.FromArgb(255,54,192,255);
```

"Argb" という文字は、色の 4 つの構成要素であるアルファ (Alpha、不透明度)、赤 (Red)、緑 (Green)、青 (Blue) の頭文字です。 各引数の設定可能な範囲は、0 ～ 255 です。 最初の値は省略可能です。その場合、透明度が既定値の 255、つまり 100% 不透明に設定されます。

> [!Note]
> C++ を使用している場合、[**ColorHelper**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.colorhelper) クラスを使って色を作成する必要があります。

**Color** は、UI 要素を単色で塗りつぶす [**SolidColorBrush**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.media.solidcolorbrush) の引数として使用されるのが最も一般的です。 このようなブラシは、通常、[**ResourceDictionary**](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.ResourceDictionary) で定義されているため、複数の要素に再利用できます。

```xaml
<ResourceDictionary>
    <SolidColorBrush x:Key="ButtonBackgroundBrush" Color="#FFFF4F67"/>
    <SolidColorBrush x:Key="ButtonForegroundBrush" Color="White"/>
</ResourceDictionary>
```

ブラシの使用方法について詳しくは、[XAML ブラシに関するページを](brushes.md)ご覧ください。

## <a name="scoping-system-colors"></a>システムの色の範囲設定

アプリで独自の色を定義するだけでなく、**ColorPaletteResources** タグを使用して、体系化された色の範囲をアプリ全体の目的の領域に設定することもできます。 この API を使用すると、いくつかのプロパティを設定して一度に多数のコントロールを色分けし、テーマを設定できるだけでなく、独自のカスタムの色を手動で定義する方法では通常得られないシステム上のメリットも数多く受けられます。

- **ColorPaletteResources** を使用して設定された色は、ハイ コントラストに影響しません。
  * つまり、デザインや開発に追加コストをかけずに、より多くの人がアプリを利用できることを意味します。
- API に 1 つのプロパティを設定することで、両方のテーマの色を淡色、濃色、または広範囲に簡単に設定できます。
- 色を **ColorPaletteResources** に設定すると、そのシステム カラーも使用するすべての同様のコントロールにも反映されます。
  * これにより、ブランドの外観を維持しながら、アプリ全体で一貫したカラー ストーリーを実現できます。
- テンプレートを再作成することなく、すべての視覚的な状態、アニメーション、不透明度のバリエーションに影響します。

### <a name="how-to-use-colorpaletteresources"></a>ColorPaletteResources の使用方法

ColorPaletteResources は、どのリソースがどこにスコープされているかをシステムに通知する API です。 ColorPaletteResources は必ず [x:Key](https://docs.microsoft.com/windows/uwp/xaml-platform/x-key-attribute) を受け取ります。これは次の 3 つの選択肢のいずれかの可能性があります。
- Default
  * 色の変化を[淡色](https://docs.microsoft.com/windows/uwp/design/style/color#light-theme)と[濃色](https://docs.microsoft.com/windows/uwp/design/style/color#dark-theme)の両方のテーマで表示します
- 淡色
  * 色の変化を[淡色テーマ](https://docs.microsoft.com/windows/uwp/design/style/color#light-theme)でのみ示します
- 濃色
  * 色の変化を[濃色テーマ](https://docs.microsoft.com/windows/uwp/design/style/color#dark-theme)でのみ示します

x:Key を設定すると、どちらのテーマでも、異なるカスタムの外観が必要な場合に、システムまたはアプリのテーマに合わせて色が適切に変更されます。

### <a name="how-to-apply-scoped-colors"></a>範囲が指定された色を適用する方法

XAML の **ColorPaletteResources** API を使用してリソースの範囲を指定すると、[テーマ リソース](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/xaml-theme-resources) ライブラリにあるシステム カラーまたはブラシを使って、ページまたはコンテナーの範囲内で再定義できます。

たとえば、グリッドの内側に **BaseLow** と **BaseMediumLow** の 2 つのシステム カラーを定義してから、そのグリッドの内側に 1 つ、外側に 1　つの　2　つのボタンをページに配置したとします。

```xaml
<Grid x:Name="Grid_A">
    <Grid.Resources>
        <ColorPaletteResources x:Key="Default"
        BaseLow="LightGreen"
        BaseMediumLow="DarkCyan"/>
    </Grid.Resources>

    <Buton Content="Button_A"/>
</Grid>
<Buton Content="Button_B"/>
```

**Button_A** には新しい色が適用され、**Button_B** はシステムの既定ボタンのままのように見えます。

![ボタンの範囲が指定されたシステム カラー](images/color/scopedcolors_cyan_button.png)

ただし、すべてのシステム カラーは他のコントロールにも反映されるため、**BaseLow** および **BaseMediumLow** の設定はボタン以外にも影響します。 この場合、**ToggleButton**、**RadioButton**、**Slider** のようなコントロールも、このようなシステム カラーの変更の影響を受けます　(これらのコントロールが上の例のグリッドの範囲に配置されている場合)。
システム カラーの変化を "*1 つのコントロールのみ*" の範囲に指定する場合は、それを行うためにそのコントロールのリソース内で **ColorPaletteResources** を定義します。

```xaml
<Grid x:Name="Grid_A">
    <Button Content="Button_A">
        <Button.Resources>
            <ColorPaletteResources x:Key="Default"
                BaseLow="LightGreen"
                BaseMediumLow="DarkCyan"/>
        </Button.Resources>
    </Button>
</Grid>
<Button Content="Button_B"/>
```
基本的には以前とまったく同じですが、グリッドに追加される他のコントロールでは、その色の変化が選択されなくなります。 これは、それらのシステム カラーの範囲が **Button_A** のみだからです。

### <a name="nesting-scoped-resources"></a>範囲が指定されたリソースを入れ子にする

システム カラーを入れ子にとすることもできます。そのためには、アプリ レイアウトのマークアップ内で入れ子になった要素のリソースに **ColorPaletteResources** を配置します。

```xaml
<Grid x:Name="Grid_A">
    <Grid.Resources>
        <ColorPaletteResources x:Key="Default"
            BaseLow="LightGreen"
            BaseMediumLow="DarkCyan"/>
    </Grid.Resources>

    <Button Content="Button_A"/>
    <Grid x:Name="Grid_B">
        <Grid.Resources>
            <ColorPaletteResources x:Key="Default"
                BaseLow="Goldenrod"
                BaseMediumLow="DarkGoldenrod"/>
        </Grid.Resources>

        <Button Content="Nested Button"/>
    </Grid>
</Grid>
```

この例では、**Button_A** は **Grid_A** のリソースの色の定義を継承し、**入れ子になったボタン**は **Grid_B** のリソースから色を継承します。 その延長で話すと、**Grid_B** 内に配置されている他のすべてのコントロールでは、**Grid_A** のリソースがチェックまたは適用される前に、まず **Grid_B** のリソースがチェックまたは適用されます。また、ページやアプリのレベルで何も定義されていない場合は、最終的に既定の色が適用されます。

これは、リソースに色の定義がある入れ子になった要素数がいくつでも機能します。

### <a name="scoping-with-a-resourcedictionary"></a>ResourceDictionary を使った範囲指定

コンテナーやページのリソースに限定されず、ResourceDictionary でこのようなシステム カラーを定義することもできます。通常、これはディクショナリの統合と同じ方法で任意の範囲で統合できます。

#### <a name="mycustomthemexaml"></a>MyCustomTheme.xaml

まず、ResourceDictionary を作成します。 次に、**ColorPaletteResources** を ThemeDictionaries 内に配置し、希望のシステム カラーをオーバーライドします。

```xaml
<ResourceDictionary
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:TestApp">

    <ResourceDictionary.ThemeDictionaries>
        <ResourceDictionary x:Key="Default">
            <ResourceDictionary.MergedDictionaries>

                <ColorPaletteResources x:Key="Default"
                    Accent="#FF0073CF"
                    AltHigh="#FF000000"
                    AltLow="#FF000000"/>

            </ResourceDictionary>
        </ResourceDictionary.MergedDictionaries>        
    </ResourceDictionary.ThemeDictionaries>
</ResourceDictionary>
```

#### <a name="mainpagexaml"></a>MainPage.xaml

レイアウトを含むページで、目的の範囲でそのディクショナリを統合します。

```xaml
<Grid x:Name="Grid_A">
    <Grid.Resources>
            <ResourceDictionary>
                <ResourceDictionary.MergedDictionaries>
                    <ResourceDictionary Source="MyCustomTheme.xaml"/>
                </ResourceDictionary.MergedDictionaries>
            </ResourceDictionary>
    </Grid.Resources>

    <Button Content="Button_A"/>
</Grid>
```

これで、すべてのリソース、テーマ設定、およびカスタムの色を 1 つの **MyCustomTheme** リソース ディクショナリに配置し、レイアウトのマークアップを複雑にすることなく、必要に応じて範囲を指定できます。

### <a name="other-ways-to-define-color-resources"></a>色のリソースを定義するその他の方法

ColorPaletteResources では、システム カラーを配置して、インラインではなくラッパーとしてその中で直接定義することもできます。

``` xaml
<ColorPaletteResources x:Key="Dark">
    <Color x:Key="SystemBaseLowColor">Goldenrod</Color>
</ColorPaletteResources>
```

## <a name="usability"></a>ユーザビリティ

:::row:::
    :::column:::
        ![contrast illustration](images/color/illo-contrast.svg)
    :::column-end:::
    :::column span="2":::
        **Contrast**

        Make sure that elements and images have sufficient contrast to differentiate between them, regardless of the accent color or theme.

        When considering what colors to use in your application, accessibility should be a primary concern. Use the guidance below to make sure your application is accessible to as many users as possible.
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![contrast illustration](images/color/illo-lighting.svg)
    :::column-end:::
    :::column span="2":::
        **Lighting**

        Be aware that variation in ambient lighting can affect the usability of your app. For example, a page with a black background might unreadable outside due to screen glare, while a page with a white background might be painful to look at in a dark room.
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![contrast illustration](images/color/illo-colorblindness.svg)
    :::column-end:::
    :::column span="2":::
        **Colorblindness**

        Be aware of how colorblindness could affect the usability of your application. For example, a user with red-green colorblindness will have difficulty distinguishing red and green elements from each other. About **8 percent of men** and **0.5 percent of women** are red-green colorblind, so avoid using these color combinations as the sole differentiator between application elements.
    :::column-end:::
:::row-end:::

## <a name="related-articles"></a>関連記事

- [XAML スタイル](../controls-and-patterns/xaml-styles.md)
- [XAML テーマ リソース](../controls-and-patterns/xaml-theme-resources.md)
