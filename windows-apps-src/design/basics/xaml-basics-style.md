---
title: カスタム スタイルを作成する
description: このチュートリアルでは、カスタム スタイルとスライダー コントロールを作成して、XAML アプリの UI をカスタマイズする方法について説明します。
keywords: XAML, UWP, 概要
ms.date: 08/20/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 6d3423e9d78e2519f2d3c9ad1fc2c0b099de0349
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89160776"
---
# <a name="tutorial-create-custom-styles"></a>チュートリアル: カスタム スタイルを作成する

このチュートリアルでは、XAML アプリの UI をカスタマイズする方法を示します。 警告: このチュートリアルにユニコーンが登場するかどうかは保証できません。 (後で登場します!)

PhotoLab サンプル アプリには 2 つのページがあります。 _メイン ページ_には、フォト ギャラリー ビューが各画像ファイルに関する情報と共に表示されます。

![MainPage](../basics/images/xaml-basics/mainpage.png)

*詳細ページ*には、選択された 1 枚の写真が表示されます。 ポップアップの編集メニューにより、写真の編集、名前変更、保存を行うことができます。

![DetailPage](../basics/images/xaml-basics/detailpage.png)

## <a name="prerequisites"></a>前提条件

+ Visual Studio 2019: [Visual Studio 2019 をダウンロードする](https://visualstudio.microsoft.com/downloads/) (Community エディションは無料)。
+ Windows 10 SDK (10.0.17763.0 以降):[最新の Windows SDK のダウンロード (無料)](https://developer.microsoft.com/windows/downloads/windows-10-sdk)
+ Windows 10 バージョン 1809 以降

## <a name="part-0-get-the-starter-code-from-github"></a>パート 0: GitHub からスタート コードを入手する

このチュートリアルでは、PhotoLab サンプルの簡易バージョンから開始します。

1. サンプルを入手するため、GitHub ページにアクセスします ([https://github.com/Microsoft/Windows-appsample-photo-lab](https://github.com/Microsoft/Windows-appsample-photo-lab))。
2. 次に、サンプルを複製またはダウンロードする必要があります。 **[Clone or download]\(クローンまたはダウンロード\)** ボタンを選択します。 サブメニューが表示されます。
    ![PhotoLab サンプルの GitHub ページの [Clone or download]\(クローンまたはダウンロード\) メニュー](images/xaml-basics/clone-repo.png)

    **GitHub に慣れていない場合:**

    a。 **[Download ZIP]\(ZIP のダウンロード\)** を選択し、ファイルをローカルに保存します。 これで、必要なすべてのプロジェクト ファイルを含む .zip ファイルがダウンロードされます。

    b. ファイルを展開します。 エクスプローラーを使用して、ダウンロードした .zip ファイルを参照し、ファイルを右クリックして **[すべて展開...]** を選択します。

    c. サンプルのローカル コピーを参照し、`Windows-appsample-photo-lab-master\xaml-basics-starting-points\style` ディレクトリに移動します。

    **GitHub に慣れている場合:**

    a。 リポジトリのマスター ブランチをローカルに複製します。

    b. `Windows-appsample-photo-lab\xaml-basics-starting-points\style` ディレクトリを参照します。

3. `Photolab.sln` をダブルクリックして、Visual Studio でソリューションを開きます。

## <a name="part-1-create-a-fancy-slider-control"></a>パート 1: 装飾的なスライダー コントロールを作成する

Windows アプリでは、アプリの外観をカスタマイズするためのさまざまな方法が提供されます。 フォントや文字体裁設定から、色やグラデーション、ぼかし効果まで、多数のオプションがあります。

チュートリアルの最初のパートでは、写真編集コントロールを装飾してみましょう。

![既定のスタイルによるシンプルなスライダー。](../basics/images/xaml-basics/slider-start.png)

このスライダーに問題はなく、スライダーに必要な機能をすべて備えています。しかし、今ひとつ装飾性に欠けます。 これを修正してみましょう。

露出スライダーは、画像の露出を調整します。左へスライドすると、画像が暗くなり、右へスライドすると画像が明るくなります。 黒から白に進む背景を指定することで、スライダーの見栄えを改善してみましょう。 スライダーの外観が良くなるだけではなく、スライダーによって提供される機能に関する視覚的な手がかりにもなります。

F5 キーを押して、アプリをコンパイルし、実行します。 最初の画面には、画像のギャラリーが表示されます。 いずれかの画像をクリックして、画像の詳細ページに移動します。 移動したら [編集] ボタンをクリックして、これから作業を行う編集コントロールを確認します。 アプリを終了し、Visual Studio に戻ります。

### <a name="customize-a-slider-control"></a>スライダー コントロールをカスタマイズする

1. [ソリューション エクスプローラー] パネルで、DetailPage.xaml をダブルクリックして開きます。

    ![Visual Studio のソリューション エクスプローラーに表示された DetailPage.xaml ファイル。](../basics/images/xaml-basics/style-detail-page-explorer.png)

1. `Polygon` 要素を使用して、露出スライダーの背景用図形を作成します。

    [Windows.UI.Xaml.Shapes 名前空間](/uwp/api/Windows.UI.Xaml.Shapes)には、7 つの図形が用意されており、ここから選ぶことができます。 楕円形、四角形のほか、パスと呼ばれるものを使うと、どのような図形でも (ユニコーンでも!) 作成できます。

    ![ユニコーン](../basics/images/xaml-basics/unicorn.png)

    > **参考情報:** 「[図形の描画](../controls-and-patterns/shapes.md)」という記事には、XAML の図形について知っておく必要があるすべての情報が含まれています。

    ここでは、ステレオのボリューム コントロールのような形をした、三角形のウィジェットを作成します。

    ![ボリューム スライダー](../basics/images/xaml-basics/style-volume-slider.png)

    `Polygon` 図形を扱う作業のようですね! 多角形を定義するには、点のセットを指定し、塗りつぶしを行います。 幅 200 ピクセル、高さ 20 ピクセルの多角形を作成し、グラデーションで塗りつぶしてみましょう。

    DetailPage.xaml で、露出スライダーのコードを見つけ、その直前に Polygon 要素を作成します。

    * 露出スライダーと同じ行に多角形を配置するために、**Grid.Row** を "2" に設定します。
    * 三角形の図形を定義するために、**Points** プロパティを "0,20 200,20 200,0" に設定します。
    * **Stretch** プロパティを "Fill" に、**HorizontalAlignment** プロパティを "Stretch" に設定します。
    * **Height** を "20"、**VerticalAlignment** を "Center" に設定します。
    * **Polygon** に、線状グラデーションによる塗りつぶしを適用します。
    * 露出スライダーでは、多角形が見えるように、**Foreground** プロパティを "Transparent" に設定します。

    **変更前**

    ```xaml
    <Slider Header="Exposure"
        Grid.Row="2"
        Value="{x:Bind item.Exposure, Mode=TwoWay}"
        Minimum="-2"
        Maximum="2" />
    ```

    **変更後**

    ```xaml
    <Polygon Grid.Row="2" Stretch="Fill"
                Points="0,20 200,20 200,0" HorizontalAlignment="Stretch"
                VerticalAlignment="Center" Height="20">
        <Polygon.Fill>
            <LinearGradientBrush StartPoint="0,0.5" EndPoint="1,0.5">
                <LinearGradientBrush.GradientStops>
                    <GradientStop Offset="0" Color="Black" />
                    <GradientStop Offset="1" Color="White" />
                </LinearGradientBrush.GradientStops>
            </LinearGradientBrush>
        </Polygon.Fill>
    </Polygon>
    <Slider Header="Exposure"
        Grid.Row="2"
        Foreground="Transparent"
        Value="{x:Bind item.Exposure, Mode=TwoWay}"
        Minimum="-2"
        Maximum="2" />
    ```

    注:
    * 周囲の XAML を見ると、これらの要素が `Grid` に含まれていることがわかります。 露出スライダーと同じ行 (`Grid.Row="2"`) に多角形を配置したため、どちらも同じ位置に表示されます。 スライダーが図形の上にレンダリングされるように、スライダーの前に多角形を配置しました。
    * 三角形が空き領域に合わせた大きさになるように、この多角形に対して `Stretch="Fill"` および `HorizontalAlignment="Stretch"` を設定しました。 スライダーの幅が縮小または拡大されると、これに合わせて多角形も縮小または拡大されます。

1. アプリをコンパイルして実行します。 これでスライダーはすばらしい外観になりました。

    ![装飾的な露出スライダー](../basics/images/xaml-basics/style-exposure-slider-done.png)

1. 次のスライダー (温度スライダー) をアップグレードしましょう。 温度スライダーでは、画像の色温度が変化します。左へスライドすると画像が青っぽくなり、右へスライドすると黄色っぽくなります。

    この背景図形用に、先ほどのものと同じ寸法の多角形をもう 1 つ使用します。ただし今回は、白黒ではなく青と黄色のグラデーションで塗りつぶします。

    **変更前**

    ```xaml
    <TextBlock Grid.Row="2"
                Grid.Column="1"
                 Margin="10,8,0,0" VerticalAlignment="Center" Padding="0"
                Text="{x:Bind item.Exposure.ToString('N', culture), Mode=OneWay}" />

    <Slider Header="Temperature"
            Grid.Row="3" Background="Transparent" Foreground="Transparent"
            Value="{x:Bind item.Temperature, Mode=TwoWay}"
            Minimum="-1"
            Maximum="1" />
    ```

    **変更後**

    ```xaml
    <TextBlock Grid.Row="2"
                Grid.Column="1"
                Margin="10,8,0,0" VerticalAlignment="Center" Padding="0"
                Text="{x:Bind item.Exposure.ToString('N', culture), Mode=OneWay}" />

    <Polygon Grid.Row="3" Stretch="Fill"
                Points="0,20 200,20 200,0" HorizontalAlignment="Stretch"
                VerticalAlignment="Center" Height="20">
        <Polygon.Fill>
            <LinearGradientBrush StartPoint="0,0.5" EndPoint="1,0.5">
                <LinearGradientBrush.GradientStops>
                    <GradientStop Offset="0" Color="Blue" />
                    <GradientStop Offset="1" Color="Yellow" />
                </LinearGradientBrush.GradientStops>
            </LinearGradientBrush>
        </Polygon.Fill>
    </Polygon>
    <Slider Header="Temperature"
            Grid.Row="3" Background="Transparent" Foreground="Transparent"
            Value="{x:Bind item.Temperature, Mode=TwoWay}"
            Minimum="-1"
            Maximum="1" />
    ```

1. アプリをコンパイルして実行します。 これで、2 つの装飾的なスライダーが作成されました。

    ![2 つの装飾的なスライダー](../basics/images/xaml-basics/style-2sliders-done.png)

1. **追加課題**

    緑から赤のグラデーションを持つ濃淡スライダーに、背景の図形を追加します。

    ![3 つの装飾的なスライダー](../basics/images/xaml-basics/style-3sliders-done.png)

これで、パート 1 は完了しました。 行き詰まった場合や最終的なソリューションを確認したい場合は、完成したコードが [https://github.com/Microsoft/Windows-appsample-photo-lab](https://github.com/Microsoft/Windows-appsample-photo-lab) にあります。

## <a name="part-2-create-basic-styles"></a>パート 2: 基本スタイルを作成する

XAML スタイルの利点の 1 つは、記述するコードの量を劇的に削減し、アプリの外観を更新する作業がずっと簡単になることです。

スタイルを定義するには、スタイルの適用対象となるコントロールが含まれる要素の [Resources](/uwp/api/windows.ui.xaml.frameworkelement.Resources) プロパティに [Style](/uwp/api/Windows.UI.Xaml.Style) 要素を追加します。  スタイルを `Page.Resources` プロパティに追加すると、ページ全体で、そのスタイルにアクセスできるようになります。 App.xaml ファイル内で `Application.Resources` プロパティにスタイルを追加すると、ページ全体で、そのスタイルにアクセスできるようになります。

名前付きスタイルと標準スタイルを作成することができます。 名前付きスタイルは、特定のコントロールに明示的に適用する必要があります。標準スタイルは、指定された `TargetType` に一致するすべてのコントロールに適用されます。

この例では、最初のスタイルには `x:Key` 属性が含まれており、ターゲットとなる型は `Button` です。 最初のボタンの `Style` プロパティはこのキーに設定されているため、このスタイルは名前付きスタイルであり、明示的に適用される必要があります。 2 番目のスタイルは、ターゲットとなる型が `Button` で、スタイルに `x:Key` 属性が含まれないため、2 番目のボタンに自動的に適用されます。

```xaml
<Page.Resources>
    <Style x:Key="PurpleStyle" TargetType="Button">
        <Setter Property="FontFamily" Value="Lucida Sans Unicode"/>
        <Setter Property="FontStyle" Value="Italic"/>
        <Setter Property="FontSize" Value="14"/>
        <Setter Property="Foreground" Value="MediumOrchid"/>
    </Style>

    <Style TargetType="Button">
        <Setter Property="Foreground" Value="Orange"/>
    </Style>
</Page.Resources>

<Grid x:Name="LayoutRoot">
    <Button Content="Button" Style="{StaticResource PurpleStyle}"/>
    <Button Content="Button" />
</Grid>
```

アプリにスタイルを追加してみましょう。 DetailsPage.xaml で、露出、温度、濃淡の各スライダーに隣接するテキスト ブロックを確認します。 これらのテキスト ブロックはそれぞれ、スライダーの値を表示しています。 露出スライダーのテキスト ブロックは次のとおりです。 `Margin`、`VerticalAlignment`、`Padding` の各プロパティが設定されていることがわかります。

```xaml
<TextBlock Grid.Row="2"
            Grid.Column="1"
            Margin="10,8,0,0" VerticalAlignment="Center" Padding="0"
            Text="{x:Bind item.Exposure.ToString('N', culture), Mode=OneWay}" />
```

他のテキスト ブロックをご覧ください。同じプロパティが同じ値に設定されています。 これらをスタイルに使うと良さそうです。

### <a name="create-a-value-text-block-style"></a>値テキスト ブロックのスタイルを作成する

1. DetailsPage.xaml を開きます。

2. **EditControlsGrid** という名前の `Grid` コントロールを見つけます。 これには、スライダーとテキスト ボックスが含まれています。 グリッドでスライダー用にスタイルが既に定義されています。

    ```xaml
    <Grid x:Name="EditControlsGrid"
            HorizontalAlignment="Stretch"
            Margin="24,48,24,24">
        <Grid.Resources>
            <Style TargetType="Slider">
                <Setter Property="Margin"
                        Value="0,0,0,0" />
                <Setter Property="Padding"
                        Value="0" />
                <Setter Property="MinWidth"
                        Value="100" />
                <Setter Property="StepFrequency"
                        Value="0.1" />
                <Setter Property="TickFrequency"
                        Value="0.1" />
            </Style>
        </Grid.Resources>
    ```

3. **TextBlock** の **Margin** を "10,8,0,0"、**VerticalAlignment** を "Center"、**Padding** を "0" に設定したスタイルを作成します。

    **変更前**

    ```xaml
        <Grid.Resources>
            <Style TargetType="Slider">
                <Setter Property="Margin"
                        Value="0,0,0,0" />
                <Setter Property="Padding"
                        Value="0" />
                <Setter Property="MinWidth"
                        Value="100" />
                <Setter Property="StepFrequency"
                        Value="0.1" />
                <Setter Property="TickFrequency"
                        Value="0.1" />
            </Style>
        </Grid.Resources>
    ```

    **変更後**

    ```xaml
        <Grid.Resources>
            <Style TargetType="Slider">
                <Setter Property="Margin"
                        Value="0,0,0,0" />
                <Setter Property="Padding"
                        Value="0" />
                <Setter Property="MinWidth"
                        Value="100" />
                <Setter Property="StepFrequency"
                        Value="0.1" />
                <Setter Property="TickFrequency"
                        Value="0.1" />
            </Style>
            <Style TargetType="TextBlock">
                <Setter Property="Margin"
                        Value="10,8,0,0" />
                <Setter Property="VerticalAlignment"
                        Value="Center" />
                <Setter Property="Padding"
                        Value="0" />
            </Style>
        </Grid.Resources>
    ```

4. どの `TextBlock` コントロールに適用するかを指定できるように、これを名前付きスタイルにしましょう。 スタイルの `x:Key` プロパティを "ValueTextBlock" に設定します。

    **変更前**

    ```xaml
            <Style TargetType="TextBlock">
                <Setter Property="Margin"
                        Value="10,8,0,0" />
                <Setter Property="VerticalAlignment"
                        Value="Center" />
                <Setter Property="Padding"
                        Value="0" />
            </Style>
    ```

    **変更後**

    ```xaml
            <Style TargetType="TextBlock"
                   x:Key="ValueTextBlock">
                <Setter Property="Margin"
                        Value="10,8,0,0" />
                <Setter Property="VerticalAlignment"
                        Value="Center" />
                <Setter Property="Padding"
                        Value="0" />
            </Style>
    ```

5. 各 `TextBlock` について、その `Margin`、`VerticalAlignment`、`Padding` プロパティを削除し、`Style` プロパティを "{StaticResource ValueTextBlock}" に設定します。

    **変更前**

    ```xaml
     <TextBlock Grid.Row="2"
                Grid.Column="1"
                Margin="10,8,0,0" VerticalAlignment="Center" Padding="0"
                Text="{x:Bind item.Exposure.ToString('N', culture), Mode=OneWay}" />
    ```

    **変更後**

    ```xaml
     <TextBlock Grid.Row="2"
                Grid.Column="1"
                Style="{StaticResource ValueTextBlock}"
                Text="{x:Bind item.Exposure.ToString('N', culture), Mode=OneWay}" />
    ```

    スライダーに関連付けられている 6 つの TextBlock コントロールすべてに対して、この変更を行います。

6. アプリをコンパイルして実行します。 同じように見えるはずです。 しかし、効率的で保守性に優れたコードを記述したことによる、すばらしい満足感と達成感を感じることができるでしょう。

これで、パート 2 は完了しました。

## <a name="part-3-use-a-control-template-to-make-a-fancy-slider"></a>パート 3: コントロール テンプレートを使用して装飾的なスライダーを作成する

パート 1 で、見栄えを良くするために、スライダーの背後に図形を追加した方法を覚えていますか?

それで作業は完了しましたが、同じ効果を得るためのもっと良い方法があります。コントロール テンプレートを作成することです。

1. [ソリューション エクスプローラー] パネルで、DetailPage.xaml をダブルクリックします。

1. 次に、開始点として、スライダー用に既定のコントロール テンプレートを使用します。 この XAML を `Page.Resources` 要素に追加します。 (`Page.Resources` 要素は、ページ先頭の近くにあります)。

    このアプリは Windows UI ライブラリ (WinUI) を使用するので、WinUI GitHub リポジトリから次の既定のテンプレートをコピーできます:[Slider_themeresources.xaml](https://github.com/microsoft/microsoft-ui-xaml/blob/master/dev/Slider/Slider_themeresources.xaml)。


    ```xaml
    <ControlTemplate x:Key="FancySliderControlTemplate" TargetType="Slider"
      xmlns:contract6Present="http://schemas.microsoft.com/winfx/2006/xaml/presentation?IsApiContractPresent(Windows.Foundation.UniversalApiContract,6)"
      xmlns:contract6NotPresent="http://schemas.microsoft.com/winfx/2006/xaml/presentation?IsApiContractNotPresent(Windows.Foundation.UniversalApiContract,6)"
      xmlns:contract7Present="http://schemas.microsoft.com/winfx/2006/xaml/presentation?IsApiContractPresent(Windows.Foundation.UniversalApiContract,7)"
      xmlns:contract7NotPresent="http://schemas.microsoft.com/winfx/2006/xaml/presentation?IsApiContractNotPresent(Windows.Foundation.UniversalApiContract,7)">
      <Grid Margin="{TemplateBinding Padding}">
        <Grid.Resources>
            <Style TargetType="Thumb" x:Key="SliderThumbStyle">
                <Setter Property="BorderThickness" Value="0" />
                <Setter Property="Background" Value="{ThemeResource SliderThumbBackground}" />
                <Setter Property="Template">
                    <Setter.Value>
                        <ControlTemplate TargetType="Thumb">
                            <Border
                                Background="{TemplateBinding Background}"
                                BorderBrush="{TemplateBinding BorderBrush}"
                                BorderThickness="{TemplateBinding BorderThickness}"
                                CornerRadius="{ThemeResource SliderThumbCornerRadius}" />
                        </ControlTemplate>
                    </Setter.Value>
                </Setter>
            </Style>
        </Grid.Resources>

        <VisualStateManager.VisualStateGroups>
            <VisualStateGroup x:Name="CommonStates">
                <VisualState x:Name="Normal" />

                <VisualState x:Name="Pressed">

                    <Storyboard>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="HorizontalTrackRect" Storyboard.TargetProperty="Fill">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTrackFillPressed}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="VerticalTrackRect" Storyboard.TargetProperty="Fill">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTrackFillPressed}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="HorizontalThumb" Storyboard.TargetProperty="Background">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderThumbBackgroundPressed}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="VerticalThumb" Storyboard.TargetProperty="Background">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderThumbBackgroundPressed}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="SliderContainer" Storyboard.TargetProperty="Background">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderContainerBackgroundPressed}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="HorizontalDecreaseRect" Storyboard.TargetProperty="Fill">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTrackValueFillPressed}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="VerticalDecreaseRect" Storyboard.TargetProperty="Fill">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTrackValueFillPressed}" />
                        </ObjectAnimationUsingKeyFrames>
                    </Storyboard>
                </VisualState>

                <VisualState x:Name="Disabled">

                    <Storyboard>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="HeaderContentPresenter" Storyboard.TargetProperty="Foreground">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderHeaderForegroundDisabled}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="HorizontalDecreaseRect" Storyboard.TargetProperty="Fill">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTrackValueFillDisabled}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="HorizontalTrackRect" Storyboard.TargetProperty="Fill">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTrackFillDisabled}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="VerticalDecreaseRect" Storyboard.TargetProperty="Fill">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTrackValueFillDisabled}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="VerticalTrackRect" Storyboard.TargetProperty="Fill">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTrackFillDisabled}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="HorizontalThumb" Storyboard.TargetProperty="Background">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderThumbBackgroundDisabled}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="VerticalThumb" Storyboard.TargetProperty="Background">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderThumbBackgroundDisabled}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="TopTickBar" Storyboard.TargetProperty="Fill">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTickBarFillDisabled}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="BottomTickBar" Storyboard.TargetProperty="Fill">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTickBarFillDisabled}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="LeftTickBar" Storyboard.TargetProperty="Fill">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTickBarFillDisabled}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="RightTickBar" Storyboard.TargetProperty="Fill">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTickBarFillDisabled}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="SliderContainer" Storyboard.TargetProperty="Background">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderContainerBackgroundDisabled}" />
                        </ObjectAnimationUsingKeyFrames>
                    </Storyboard>
                </VisualState>

                <VisualState x:Name="PointerOver">

                    <Storyboard>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="HorizontalTrackRect" Storyboard.TargetProperty="Fill">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTrackFillPointerOver}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="VerticalTrackRect" Storyboard.TargetProperty="Fill">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTrackFillPointerOver}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="HorizontalThumb" Storyboard.TargetProperty="Background">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderThumbBackgroundPointerOver}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="VerticalThumb" Storyboard.TargetProperty="Background">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderThumbBackgroundPointerOver}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="SliderContainer" Storyboard.TargetProperty="Background">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderContainerBackgroundPointerOver}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="HorizontalDecreaseRect" Storyboard.TargetProperty="Fill">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTrackValueFillPointerOver}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="VerticalDecreaseRect" Storyboard.TargetProperty="Fill">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTrackValueFillPointerOver}" />
                        </ObjectAnimationUsingKeyFrames>
                    </Storyboard>
                </VisualState>

            </VisualStateGroup>
            <VisualStateGroup x:Name="FocusEngagementStates">
                <VisualState x:Name="FocusDisengaged" />
                <VisualState x:Name="FocusEngagedHorizontal">

                    <Storyboard>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="SliderContainer" Storyboard.TargetProperty="(Control.IsTemplateFocusTarget)">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="False" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="HorizontalThumb" Storyboard.TargetProperty="(Control.IsTemplateFocusTarget)">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="True" />
                        </ObjectAnimationUsingKeyFrames>
                    </Storyboard>
                </VisualState>
                <VisualState x:Name="FocusEngagedVertical">

                    <Storyboard>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="SliderContainer" Storyboard.TargetProperty="(Control.IsTemplateFocusTarget)">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="False" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="VerticalThumb" Storyboard.TargetProperty="(Control.IsTemplateFocusTarget)">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="True" />
                        </ObjectAnimationUsingKeyFrames>
                    </Storyboard>
                </VisualState>

            </VisualStateGroup>

        </VisualStateManager.VisualStateGroups>

        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <ContentPresenter x:Name="HeaderContentPresenter"
            Grid.Row="0"
            Content="{TemplateBinding Header}"
            ContentTemplate="{TemplateBinding HeaderTemplate}"
            FontWeight="{ThemeResource SliderHeaderThemeFontWeight}"
            Foreground="{ThemeResource SliderHeaderForeground}"
            Margin="{ThemeResource SliderTopHeaderMargin}"
            TextWrapping="Wrap"
            Visibility="Collapsed"
            x:DeferLoadStrategy="Lazy"/>
        <Grid x:Name="SliderContainer"
            Grid.Row="1"
            Background="{ThemeResource SliderContainerBackground}"
            Control.IsTemplateFocusTarget="True">
            <Grid x:Name="HorizontalTemplate" MinHeight="{ThemeResource SliderHorizontalHeight}">

                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="Auto" />
                    <ColumnDefinition Width="Auto" />
                    <ColumnDefinition Width="*" />
                </Grid.ColumnDefinitions>

                <Grid.RowDefinitions>
                    <RowDefinition Height="{ThemeResource SliderPreContentMargin}" />
                    <RowDefinition Height="Auto" />
                    <RowDefinition Height="{ThemeResource SliderPostContentMargin}" />
                </Grid.RowDefinitions>

                <Rectangle x:Name="HorizontalTrackRect"
                    Fill="{TemplateBinding Background}"
                    Height="{ThemeResource SliderTrackThemeHeight}"
                    Grid.Row="1"
                    Grid.ColumnSpan="3"
                    contract7Present:RadiusX="{Binding CornerRadius, RelativeSource={RelativeSource TemplatedParent}, Converter={StaticResource TopLeftCornerRadiusDoubleValueConverter}}"
                    contract7Present:RadiusY="{Binding CornerRadius, RelativeSource={RelativeSource TemplatedParent}, Converter={StaticResource BottomRightCornerRadiusDoubleValueConverter}}"
                    contract7NotPresent:RadiusX="{Binding Source={ThemeResource ControlCornerRadius}, Converter={StaticResource TopLeftCornerRadiusDoubleValueConverter}}"
                    contract7NotPresent:RadiusY="{Binding Source={ThemeResource ControlCornerRadius}, Converter={StaticResource BottomRightCornerRadiusDoubleValueConverter}}" />
                <Rectangle x:Name="HorizontalDecreaseRect" Fill="{TemplateBinding Foreground}" Grid.Row="1"
                    contract7Present:RadiusX="{Binding CornerRadius, RelativeSource={RelativeSource TemplatedParent}, Converter={StaticResource TopLeftCornerRadiusDoubleValueConverter}}"
                    contract7Present:RadiusY="{Binding CornerRadius, RelativeSource={RelativeSource TemplatedParent}, Converter={StaticResource BottomRightCornerRadiusDoubleValueConverter}}"
                    contract7NotPresent:RadiusX="{Binding Source={ThemeResource ControlCornerRadius}, Converter={StaticResource TopLeftCornerRadiusDoubleValueConverter}}"
                    contract7NotPresent:RadiusY="{Binding Source={ThemeResource ControlCornerRadius}, Converter={StaticResource BottomRightCornerRadiusDoubleValueConverter}}" />
                <TickBar x:Name="TopTickBar"
                    Visibility="Collapsed"
                    Fill="{ThemeResource SliderTickBarFill}"
                    Height="{ThemeResource SliderOutsideTickBarThemeHeight}"
                    VerticalAlignment="Bottom"
                    Margin="0,0,0,4"
                    Grid.ColumnSpan="3" />
                <TickBar x:Name="HorizontalInlineTickBar"
                    Visibility="Collapsed"
                    Fill="{ThemeResource SliderInlineTickBarFill}"
                    Height="{ThemeResource SliderTrackThemeHeight}"
                    Grid.Row="1"
                    Grid.ColumnSpan="3" />
                <TickBar x:Name="BottomTickBar"
                    Visibility="Collapsed"
                    Fill="{ThemeResource SliderTickBarFill}"
                    Height="{ThemeResource SliderOutsideTickBarThemeHeight}"
                    VerticalAlignment="Top"
                    Margin="0,4,0,0"
                    Grid.Row="2"
                    Grid.ColumnSpan="3" />
                <Thumb x:Name="HorizontalThumb"
                    Style="{StaticResource SliderThumbStyle}"
                    DataContext="{TemplateBinding Value}"
                    Height="{ThemeResource SliderHorizontalThumbHeight}"
                    Width="{ThemeResource SliderHorizontalThumbWidth}"
                    Grid.Row="0"
                    Grid.RowSpan="3"
                    Grid.Column="1"
                    FocusVisualMargin="-14,-6,-14,-6"
                    AutomationProperties.AccessibilityView="Raw" />
            </Grid>
            <Grid x:Name="VerticalTemplate" MinWidth="{ThemeResource SliderVerticalWidth}" Visibility="Collapsed">

                <Grid.RowDefinitions>
                    <RowDefinition Height="*" />
                    <RowDefinition Height="Auto" />
                    <RowDefinition Height="Auto" />
                </Grid.RowDefinitions>

                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="{ThemeResource SliderPreContentMargin}" />
                    <ColumnDefinition Width="Auto" />
                    <ColumnDefinition Width="{ThemeResource SliderPostContentMargin}" />
                </Grid.ColumnDefinitions>

                <Rectangle x:Name="VerticalTrackRect"
                    Fill="{TemplateBinding Background}"
                    Width="{ThemeResource SliderTrackThemeHeight}"
                    Grid.Column="1"
                    Grid.RowSpan="3"
                    contract7Present:RadiusX="{Binding CornerRadius, RelativeSource={RelativeSource TemplatedParent}, Converter={StaticResource TopLeftCornerRadiusDoubleValueConverter}}"
                    contract7Present:RadiusY="{Binding CornerRadius, RelativeSource={RelativeSource TemplatedParent}, Converter={StaticResource BottomRightCornerRadiusDoubleValueConverter}}"
                    contract7NotPresent:RadiusX="{Binding Source={ThemeResource ControlCornerRadius}, Converter={StaticResource TopLeftCornerRadiusDoubleValueConverter}}"
                    contract7NotPresent:RadiusY="{Binding Source={ThemeResource ControlCornerRadius}, Converter={StaticResource BottomRightCornerRadiusDoubleValueConverter}}" />
                <Rectangle x:Name="VerticalDecreaseRect"
                    Fill="{TemplateBinding Foreground}"
                    Grid.Column="1"
                    Grid.Row="2"
                    contract7Present:RadiusX="{Binding CornerRadius, RelativeSource={RelativeSource TemplatedParent}, Converter={StaticResource TopLeftCornerRadiusDoubleValueConverter}}"
                    contract7Present:RadiusY="{Binding CornerRadius, RelativeSource={RelativeSource TemplatedParent}, Converter={StaticResource BottomRightCornerRadiusDoubleValueConverter}}"
                    contract7NotPresent:RadiusX="{Binding Source={ThemeResource ControlCornerRadius}, Converter={StaticResource TopLeftCornerRadiusDoubleValueConverter}}"
                    contract7NotPresent:RadiusY="{Binding Source={ThemeResource ControlCornerRadius}, Converter={StaticResource BottomRightCornerRadiusDoubleValueConverter}}" />
                <TickBar x:Name="LeftTickBar"
                    Visibility="Collapsed"
                    Fill="{ThemeResource SliderTickBarFill}"
                    Width="{ThemeResource SliderOutsideTickBarThemeHeight}"
                    HorizontalAlignment="Right"
                    Margin="0,0,4,0"
                    Grid.RowSpan="3" />
                <TickBar x:Name="VerticalInlineTickBar"
                    Visibility="Collapsed"
                    Fill="{ThemeResource SliderInlineTickBarFill}"
                    Width="{ThemeResource SliderTrackThemeHeight}"
                    Grid.Column="1"
                    Grid.RowSpan="3" />
                <TickBar x:Name="RightTickBar"
                    Visibility="Collapsed"
                    Fill="{ThemeResource SliderTickBarFill}"
                    Width="{ThemeResource SliderOutsideTickBarThemeHeight}"
                    HorizontalAlignment="Left"
                    Margin="4,0,0,0"
                    Grid.Column="2"
                    Grid.RowSpan="3" />
                <Thumb x:Name="VerticalThumb"
                    Style="{StaticResource SliderThumbStyle}"
                    DataContext="{TemplateBinding Value}"
                    Width="{ThemeResource SliderVerticalThumbWidth}"
                    Height="{ThemeResource SliderVerticalThumbHeight}"
                    Grid.Row="1"
                    Grid.Column="0"
                    Grid.ColumnSpan="3"
                    FocusVisualMargin="-6,-14,-6,-14"
                    AutomationProperties.AccessibilityView="Raw" />
            </Grid>
        </Grid>
      </Grid>
    </ControlTemplate>
    ```

    長い XAML ですね。 コントロール テンプレートは強力な機能ですが、かなり複雑であるため、通常は、既定のテンプレートから開始することをお勧めします。

1. 追加した `ControlTemplate` 内で、`HorizontalTemplate` という名前のグリッド コントロールを見つけます。 このグリッドで変更するテンプレートの部分を定義します。

    ```xaml
    <Grid x:Name="HorizontalTemplate" MinHeight="{ThemeResource SliderHorizontalHeight}">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>
        <Grid.RowDefinitions>
            <RowDefinition Height="{ThemeResource SliderPreContentMargin}" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="{ThemeResource SliderPostContentMargin}" />
        </Grid.RowDefinitions>
    ```

1.  パート 1 で露出スライダー用に作成した多角形と同様の多角形を作成します。 `Grid.RowDefinitions` の終了タグの後に、多角形を追加します。 `Grid.Row` を "0"、`Grid.RowSpan` を "3"、`Grid.ColumnSpan` を "3" に設定します。

    **変更前**

    ```xaml
    <Grid x:Name="HorizontalTemplate" MinHeight="44">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>
        <Grid.RowDefinitions>
            <RowDefinition Height="18" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="18" />
        </Grid.RowDefinitions>
    ```

    **変更後**

    ```xaml
    <Grid x:Name="HorizontalTemplate" MinHeight="44">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>
        <Grid.RowDefinitions>
            <RowDefinition Height="18" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="18" />
        </Grid.RowDefinitions>
        <Polygon Grid.Row="0" Grid.RowSpan="3"  Grid.ColumnSpan="3" Stretch="Fill"
                    Points="0,20 200,20 200,0" HorizontalAlignment="Stretch"
                    VerticalAlignment="Center" Height="20" >
            <Polygon.Fill>
                <LinearGradientBrush StartPoint="0,0.5" EndPoint="1,0.5">
                    <LinearGradientBrush.GradientStops>
                        <GradientStop Offset="0" Color="Black" />
                        <GradientStop Offset="1" Color="White" />
                    </LinearGradientBrush.GradientStops>
                </LinearGradientBrush>
            </Polygon.Fill>
        </Polygon>
    ```

1. `Polygon.Fill` 設定を削除します。 `Fill` を `"{TemplateBinding Background}"` に設定します。 これにより、スライダーの `Background` プロパティを設定すると、多角形の `Fill` プロパティが設定されるようになります。

    **変更前**

    ```xaml
        <Polygon Grid.Row="0" Grid.RowSpan="3"  Grid.ColumnSpan="3" Stretch="Fill"
                    Points="0,20 200,20 200,0" HorizontalAlignment="Stretch"
                    VerticalAlignment="Center" Height="20" >
            <Polygon.Fill>
                <LinearGradientBrush StartPoint="0,0.5" EndPoint="1,0.5">
                    <LinearGradientBrush.GradientStops>
                        <GradientStop Offset="0" Color="Black" />
                        <GradientStop Offset="1" Color="White" />
                    </LinearGradientBrush.GradientStops>
                </LinearGradientBrush>
            </Polygon.Fill>
        </Polygon>
    ```

    **変更後**

    ```xaml
        <Polygon Grid.Row="0" Grid.RowSpan="3"  Grid.ColumnSpan="3" Stretch="Fill"
                    Points="0,20 200,20 200,0" HorizontalAlignment="Stretch"
                    VerticalAlignment="Center" Height="20"
                    Fill="{TemplateBinding Background}">
        </Polygon>
    ```

1. 追加した多角形の直後に、`HorizontalTrackRect` という名前の四角形があります。 四角形が表示されて多角形図形を遮ることのないように、Rectangle の `Fill` 設定を削除します。 (四角形は、コントロール テンプレートでホバーなどの対話式操作用ビジュアルにも使用されるため、完全には削除しません)。

    **変更前**

    ```xaml
        <Rectangle x:Name="HorizontalTrackRect"
                    Fill="{TemplateBinding Background}"
                    Height="{ThemeResource SliderTrackThemeHeight}"
                    Grid.Row="1"
                    Grid.ColumnSpan="3" />
    ```

    **変更後**

    ```xaml
        <Rectangle x:Name="HorizontalTrackRect"
                    Height="{ThemeResource SliderTrackThemeHeight}"
                    Grid.Row="1"
                    Grid.ColumnSpan="3" />
    ```

    これで、テンプレートに対する作業が完了しました。 次は、これをスライダーに適用する必要があります。

1. 露出スライダーを更新しましょう。

    * スライダーの `Template` プロパティを `"{StaticResource FancySliderControlTemplate}"` に設定します。
    * スライダーの `Background="Transparent"` 設定を削除します。
    * スライダーの `Background` を、黒から白への線状グラデーションに設定します。
    * パート 1 で作成した背景の多角形を削除します。

    **変更前**

    ```xaml
    <Polygon Grid.Row="2" Stretch="Fill"
                Points="0,20 200,20 200,0" HorizontalAlignment="Stretch"
                VerticalAlignment="Center" Height="20">
        <Polygon.Fill>
            <LinearGradientBrush StartPoint="0,0.5" EndPoint="1,0.5">
                <LinearGradientBrush.GradientStops>
                    <GradientStop Offset="0" Color="Black" />
                    <GradientStop Offset="1" Color="White" />
                </LinearGradientBrush.GradientStops>
            </LinearGradientBrush>
        </Polygon.Fill>
    </Polygon>
    <Slider Header="Exposure"
            Grid.Row="2" Background="Transparent" Foreground="Transparent"
            Value="{x:Bind item.Exposure, Mode=TwoWay}"
            Minimum="-2"
            Maximum="2" />
    ```

    **変更後**

    ```xaml
    <Slider Header="Exposure"
            Grid.Row="2"  Foreground="Transparent"
            Value="{x:Bind item.Exposure, Mode=TwoWay}"
            Minimum="-2"
            Maximum="2"
            Template="{StaticResource FancySliderControlTemplate}">
        <Slider.Background>
            <LinearGradientBrush StartPoint="0,0.5" EndPoint="1,0.5">
                <LinearGradientBrush.GradientStops>
                    <GradientStop Offset="0" Color="Black" />
                    <GradientStop Offset="1" Color="White" />
                </LinearGradientBrush.GradientStops>
            </LinearGradientBrush>
        </Slider.Background>
    </Slider>
    ```

1. 温度スライダーにも、同じ更新を行います。

    **変更前**

    ```xaml
    <Polygon Grid.Row="3" Stretch="Fill"
                Points="0,20 200,20 200,0" HorizontalAlignment="Stretch"
                VerticalAlignment="Center" Height="20">
        <Polygon.Fill>
            <LinearGradientBrush StartPoint="0,0.5" EndPoint="1,0.5">
                <LinearGradientBrush.GradientStops>
                    <GradientStop Offset="0" Color="Blue" />
                    <GradientStop Offset="1" Color="Yellow" />
                </LinearGradientBrush.GradientStops>
            </LinearGradientBrush>
        </Polygon.Fill>
    </Polygon>
    <Slider Header="Temperature"
            Grid.Row="3" Background="Transparent" Foreground="Transparent"
            Value="{x:Bind item.Temperature, Mode=TwoWay}"
            Minimum="-1"
            Maximum="1" />
    ```

    **変更後**

    ```xaml
    <Slider Header="Temperature"
            Grid.Row="3" Foreground="Transparent"
            Value="{x:Bind item.Temperature, Mode=TwoWay}"
            Minimum="-1"
            Maximum="1"
            Template="{StaticResource FancySliderControlTemplate}">
        <Slider.Background>
            <LinearGradientBrush StartPoint="0,0.5" EndPoint="1,0.5">
                <LinearGradientBrush.GradientStops>
                    <GradientStop Offset="0" Color="Blue" />
                    <GradientStop Offset="1" Color="Yellow" />
                </LinearGradientBrush.GradientStops>
            </LinearGradientBrush>
        </Slider.Background>
    </Slider>
    ```

1. 濃淡スライダーにも、同じ更新を行います。

    **変更前**

    ```xaml
    <Polygon Grid.Row="4" Stretch="Fill"
                Points="0,20 200,20 200,0" HorizontalAlignment="Stretch"
                VerticalAlignment="Center" Height="20">
        <Polygon.Fill>
            <LinearGradientBrush StartPoint="0,0.5" EndPoint="1,0.5">
                <LinearGradientBrush.GradientStops>
                    <GradientStop Offset="0" Color="Red" />
                    <GradientStop Offset="1" Color="Green" />
                </LinearGradientBrush.GradientStops>
            </LinearGradientBrush>
        </Polygon.Fill>
    </Polygon>
    <Slider Header="Tint"
            Grid.Row="4" Background="Transparent" Foreground="Transparent"
            Value="{x:Bind item.Tint, Mode=TwoWay}"
            Minimum="-1"
            Maximum="1" />
    ```

    **変更後**

    ```xaml
    <Slider Header="Tint"
            Grid.Row="4" Foreground="Transparent"
            Value="{x:Bind item.Tint, Mode=TwoWay}"
            Minimum="-1"
            Maximum="1"
            Template="{StaticResource FancySliderControlTemplate}">
        <Slider.Background>
            <LinearGradientBrush StartPoint="0,0.5" EndPoint="1,0.5">
                <LinearGradientBrush.GradientStops>
                    <GradientStop Offset="0" Color="Red" />
                    <GradientStop Offset="1" Color="Green" />
                </LinearGradientBrush.GradientStops>
            </LinearGradientBrush>
        </Slider.Background>
    </Slider>
    ```

1. アプリをコンパイルして実行します。

    ![世界一のスライダー](../basics/images/xaml-basics/style-sliders-templates.png)

    このとおり、ここで行った更新によって、多角形の配置が改善されました。多角形の下端がスライダーのつまみの下端と揃っています。

これで、チュートリアルは完了しました。 行き詰まって最終的なソリューションを確認したい場合は、完成したサンプルが [https://github.com/Microsoft/Windows-appsample-photo-lab](https://github.com/Microsoft/Windows-appsample-photo-lab) にあります。