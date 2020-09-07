---
title: アダプティブ レイアウトの作成のチュートリアル
description: XAML でアダプティブ レイアウト機能を使用して、あらゆるウィンドウ サイズで正しく表示されるアプリを作成する方法について説明します。
keywords: XAML, UWP, 概要
ms.date: 08/20/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 8e1498836772c3c279a1b9d85d76070b29593f5e
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174476"
---
# <a name="tutorial-create-adaptive-layouts"></a>チュートリアル: アダプティブ レイアウトを作成する

このチュートリアルでは、XAML のアダプティブ レイアウト機能の基本について説明します。これを使うと、どのようなサイズでも正しく表示されるアプリを作成できます。 ウィンドウ ブレークポイントの追加方法、新しい DataTemplate の作成方法、VisualStateManager クラスを使用してアプリのレイアウトをカスタマイズする方法について説明します。 これらのツールを使用して、小さめのウィンドウ サイズ用に画像編集プログラムを最適化します。

画像編集プログラムには 2 つのページがあります。 _メイン ページ_には、フォト ギャラリー ビューが各画像ファイルに関する情報と共に表示されます。

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
2. 次に、サンプルを複製またはダウンロードする必要があります。 **[Clone or download]\(クローンまたはダウンロード\)** ボタンをクリックします。 サブメニューが表示されます。
    ![PhotoLab サンプルの GitHub ページの [Clone or download]\(クローンまたはダウンロード\) メニュー](images/xaml-basics/clone-repo.png)

    **GitHub に慣れていない場合:**

    a。 **[Download ZIP]\(ZIP のダウンロード\)** をクリックし、ファイルをローカルに保存します。 これで、必要なすべてのプロジェクト ファイルを含む .zip ファイルがダウンロードされます。

    b. ファイルを展開します。 エクスプローラーを使用して、ダウンロードした .zip ファイルを参照し、ファイルを右クリックして **[すべて展開...]** を選択します。

    c. サンプルのローカル コピーを参照し、`Windows-appsample-photo-lab-master\xaml-basics-starting-points\adaptive-layout` ディレクトリに移動します。

    **GitHub に慣れている場合:**

    a。 リポジトリのマスター ブランチをローカルに複製します。

    b. `Windows-appsample-photo-lab\xaml-basics-starting-points\adaptive-layout` ディレクトリを参照します。

3. `Photolab.sln` をダブルクリックして、Visual Studio でソリューションを開きます。

## <a name="part-1-define-window-breakpoints"></a>パート 1: ウィンドウ ブレークポイントを定義する

アプリを実行します。 全画面表示では正しく表示されますが、ウィンドウを小さくしすぎると、ユーザー インターフェイス (UI) が最適ではなくなります。 UI がさまざまなウィンドウ サイズに適合したものになるように、[VisualStateManager](/uwp/api/windows.ui.xaml.visualstatemanager) クラスを使用することにより、画面表示や操作性に関するエンドユーザー エクスペリエンスを常に最適にできます。

![小さなウィンドウ: 変更前](../basics/images/xaml-basics/adaptive-layout-small-before.png)

アプリのレイアウトの詳細については、ドキュメントの「[レイアウト](../layout/index.md)」セクションを参照してください。

### <a name="add-window-breakpoints"></a>ウィンドウ ブレークポイントを追加する

最初のステップは、異なる表示状態が適用される "_ブレークポイント_" を定義することです。 小、中、大それぞれの画面のブレークポイントの詳細については、「[画面のサイズとブレークポイント](../layout/screen-sizes-and-breakpoints-for-responsive-design.md)」を参照してください。

ソリューション エクスプローラーから App.xaml を開き、`MergedDictionaries` の後の、終了タグ (`</ResourceDictionary>`) の直前に次のコードを追加します。

```xaml
    <!--  Window width adaptive breakpoints.  -->
    <x:Double x:Key="MinWindowBreakpoint">0</x:Double>
    <x:Double x:Key="MediumWindowBreakpoint">641</x:Double>
    <x:Double x:Key="LargeWindowBreakpoint">1008</x:Double>
```

これにより、3 つのブレークポイントが作成され、3 種類のウィンドウ サイズ範囲に対して新しい表示状態を作成できるようになります。

+ 小 (0 - 640 ピクセル幅)
+ 中 (641 - 1007 ピクセル幅)
+ 大 (1008 ピクセル幅以上)

この例では、"小" のウィンドウ サイズに対してのみ新しい表示を作成します。 "中" と "大" のサイズには、同じ表示を使用します。

## <a name="part-2-add-a-data-template-for-small-window-sizes"></a>パート 2: 小さいウィンドウ サイズ用のデータ テンプレートを追加する

小さいウィンドウに表示する場合でもこのアプリが正しく表示されるようにするため、ユーザーがウィンドウを縮小したときに、イメージ ギャラリー ビューのイメージの表示方法を最適化する新しいデータ テンプレートを作成できます。

### <a name="create-a-new-datatemplate"></a>新しい DataTemplate を作成する

 ソリューション エクスプローラーから MainPage.xaml を開き、`Page.Resources` タグ内に次のコードを追加します。

```xaml
<DataTemplate x:Key="ImageGridView_SmallItemTemplate"
              x:DataType="local:ImageFileInfo">

    <!-- Create image grid -->
    <Grid Height="{Binding ItemSize, ElementName=page}"
          Width="{Binding ItemSize, ElementName=page}">

        <!-- Place image in grid, stretching it to fill the pane-->
        <Image x:Name="ItemImage"
               Source="{x:Bind ImageSource, Mode=OneWay}"
               Stretch="UniformToFill">
        </Image>

    </Grid>
</DataTemplate>
```

このギャラリー テンプレートでは、画像の境界線と、各サムネイルの下にある画像のメタデータ (ファイル名、評価など) を排除することによって、画面領域を節約します。 代わりに、各サムネイルを単純な正方形で表示します。

### <a name="add-metadata-to-a-tooltip"></a>メタデータをヒントに追加する

ユーザーが各画像のメタデータにアクセスできるように、それぞれの画像項目にヒントを追加します。 先ほど作成した DataTemplate の `Image` タグ内に、次のコードを追加します。

```xaml
    <!-- Add a tooltip to the image that displays metadata -->
    <ToolTipService.ToolTip>
        <ToolTip x:Name="tooltip">

            <!-- Arrange tooltip elements vertically -->
            <StackPanel Orientation="Vertical"
                        Grid.Row="1">

                <!-- Image title -->
                <TextBlock Text="{x:Bind ImageTitle, Mode=OneWay}"
                           HorizontalAlignment="Center"
                           Style="{StaticResource SubtitleTextBlockStyle}" />

                <!-- Arrange elements horizontally -->
                <StackPanel Orientation="Horizontal"
                            HorizontalAlignment="Center">

                    <!-- Image file type -->
                    <TextBlock Text="{x:Bind ImageFileType}"
                               HorizontalAlignment="Center"
                               Style="{StaticResource CaptionTextBlockStyle}" />

                    <!-- Image dimensions -->
                    <TextBlock Text="{x:Bind ImageDimensions}"
                               HorizontalAlignment="Center"
                               Style="{StaticResource CaptionTextBlockStyle}"
                               Margin="8,0,0,0" />
                </StackPanel>
            </StackPanel>
        </ToolTip>
    </ToolTipService.ToolTip>
```

これにより、サムネイルにマウスを重ねると (または、タッチ スクリーンを長押しすると)、画像のタイトル、ファイルの種類、サイズが表示されるようになります。

## <a name="part-3-define-visual-states"></a>パート 3: 表示状態を定義する

ここまででデータの新しいレイアウトを作成できましたが、これだけでは、既定のスタイルではなくこのレイアウトをいつ使うべきかをアプリは識別できません。 これを解決するには、[VisualStateManager](/uwp/api/windows.ui.xaml.visualstatemanager) と [VisualState](/uwp/api/windows.ui.xaml.visualstate) の定義を追加する必要があります。

### <a name="add-a-visualstatemanager"></a>VisualStateManager を追加する

ページのルート要素である `RelativePanel` に次のコードを追加します。

```xaml
<VisualStateManager.VisualStateGroups>
    <VisualStateGroup>
    ...

        <!-- Large window VisualState -->
        <VisualState x:Key="LargeWindow">

        </VisualState>

        <!-- Medium window VisualState -->
        <VisualState x:Key="MediumWindow">

        </VisualState>

        <!-- Small window VisualState -->
        <VisualState x:Key="SmallWindow">

        </VisualState>

    </VisualStateGroup>
</VisualStateManager.VisualStateGroups>
```

### <a name="create-statetriggers-to-apply-the-visual-state"></a>表示状態を適用する StateTriggers を作成する

次に、各スナップ位置に対応する `StateTriggers` を作成します。 MainPage.xaml の `VisualStateManager` (パート 2 で作成) に次のコードを追加します。

```xaml
<VisualStateManager.VisualStateGroups>
    <VisualStateGroup>
    ...

        <!-- Large window VisualState -->
        <VisualState x:Key="LargeWindow">

            <!-- Large window trigger -->
            <VisualState.StateTriggers>
                <AdaptiveTrigger MinWindowWidth="{StaticResource LargeWindowSnapPoint}"/>
            </VisualState.StateTriggers>

        </VisualState>

        <!-- Medium window VisualState -->
        <VisualState x:Key="MediumWindow">

            <!-- Medium window trigger -->
            <VisualState.StateTriggers>
                <AdaptiveTrigger MinWindowWidth="{StaticResource MediumWindowSnapPoint}"/>
            </VisualState.StateTriggers>

        </VisualState>

        <!-- Small window VisualState -->
        <VisualState x:Key="SmallWindow">

            <!-- Small window trigger -->
            <VisualState.StateTriggers >
                <AdaptiveTrigger MinWindowWidth="{StaticResource MinWindowSnapPoint}"/>
            </VisualState.StateTriggers>

        </VisualState>

    </VisualStateGroup>
</VisualStateManager.VisualStateGroups>
```

各表示状態がトリガーされると、アプリは、アクティブな `VisualState` に割り当てられているレイアウト属性を使用します。

### <a name="set-properties-for-each-visual-state"></a>各表示状態のプロパティを設定する

最後に、各表示状態のプロパティを設定して、その状態がトリガーされたときに適用する属性を `VisualStateManager` に指定します。 各 setter は、特定の XAML 要素の 1 つのプロパティを対象とし、指定された値にそれを設定します。 次のコードを、先ほど作成した `SmallWindow` 表示状態の `StateTriggers` の後に追加します。

```xaml
    <!-- Small window setters -->
    <VisualState.Setters>

        <!-- Apply small template and styles -->
        <Setter Target="ImageGridView.ItemTemplate"
                Value="{StaticResource ImageGridView_SmallItemTemplate}" />
        <Setter Target="ImageGridView.ItemContainerStyle"
                Value="{StaticResource ImageGridView_SmallItemContainerStyle}" />

        <!-- Adjust the zoom slider to fit small windows-->
        <Setter Target="ZoomSlider.Minimum"
                Value="80" />
        <Setter Target="ZoomSlider.Maximum"
                Value="180" />
        <Setter Target="ZoomSlider.TickFrequency"
                Value="20" />
        <Setter Target="ZoomSlider.Value"
                Value="100" />
    </VisualState.Setters>
```

これらのセッターにより、イメージ ギャラリーの `ItemTemplate` が、前のセクションで作成した新しい `DataTemplate` に設定されます。 また、小さい画面に合わせて、ズーム スライダーの調整も行われます。

### <a name="run-the-app"></a>アプリを実行する

アプリを実行します。 アプリが読み込まれたら、ウィンドウのサイズを変更してみてください。 ウィンドウを小さなサイズに縮小すると、パート 2 で作成した小さなレイアウトにアプリが切り替わることを確認できます。

![小さなウィンドウ: 変更後](../basics/images/xaml-basics/adaptive-layout-small-after.png)

## <a name="going-further"></a>追加情報

これで、この演習は終わりです。自身でさらに試すために必要な、アダプティブ レイアウトに関する知識を身につけることができました。 さらに大きな課題としては、Surface Hub などの大きな画面サイズ用にレイアウトを最適化してみることができます。 Surface Hub のレイアウトをテストする場合は、「[Visual Studio を使った Surface Hub アプリのテスト](../../debug-test-perf/test-surface-hub-apps-using-visual-studio.md)」を参照してください。

行き詰まった場合は、「[XAML を使ったページ レイアウトの定義](../layout/layouts-with-xaml.md)」の以下のセクションで、詳しいガイダンスを参照できます。

+ [表示状態と状態トリガー](../layout/layouts-with-xaml.md#visual-states-and-state-triggers)
+ [カスタマイズされたレイアウト](../layout/layouts-with-xaml.md#tailored-layouts)

当初の写真編集アプリの作成方法を学習するには、XAML の[ユーザー インターフェイス](../basics/xaml-basics-ui.md)と[データ バインディング](../../data-binding/xaml-basics-data-binding.md)に関するチュートリアルをご覧ください。

## <a name="get-the-final-version-of-the-photolab-sample"></a>PhotoLab サンプルの最終バージョンを入手する

このチュートリアルで完全な写真編集アプリは作成されません。必ず[最終バージョン](https://github.com/Microsoft/Windows-appsample-photo-lab)でカスタム アニメーションやスタイルなど、他の機能を確認してください。