---
title: ユーザー インターフェイスの作成のチュートリアル
description: このチュートリアルでは、Visual Studio で XAML ツールを使用して、イメージ編集プログラムのための基本的な UI を作成する方法について説明します。
keywords: XAML, UWP, 概要
ms.date: 08/20/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: e4c2c8d52069bf074897ec09fa44f550066b28b5
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89160756"
---
# <a name="tutorial-create-a-user-interface"></a>チュートリアル: ユーザー インターフェイスの作成

このチュートリアルでは、イメージ編集プログラムの基本的な UI を作成する方法について説明します。

+ Visual Studio の XAML ツール (XAML デザイナー、ツールボックス、XAML エディター、 [プロパティ] パネル、ドキュメント アウトラインなど) を使用してコントロールやコンテンツを UI に追加する。
+ 最も一般的な XAML レイアウト パネル (`RelativePanel`、`Grid`、`StackPanel` など) を利用する。

画像編集プログラムには 2 つのページがあります。 _メイン ページ_には、フォト ギャラリー ビューが各画像ファイルに関する情報と共に表示されます。

![メイン ページ](images/xaml-basics/mainpage.png)

*詳細ページ*には、選択された 1 枚の写真が表示されます。 ポップアップの編集メニューにより、写真の編集、名前変更、保存を行うことができます。

![詳細ページ](images/xaml-basics/detailpage.png)

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

    c. サンプルのローカル コピーを参照し、`Windows-appsample-photo-lab-master\xaml-basics-starting-points\user-interface` ディレクトリに移動します。

    **GitHub に慣れている場合:**

    a。 リポジトリのマスター ブランチをローカルに複製します。

    b. `Windows-appsample-photo-lab\xaml-basics-starting-points\user-interface` ディレクトリを参照します。

3. `Photolab.sln` をダブルクリックして、Visual Studio でソリューションを開きます。

## <a name="part-1-add-a-textblock-control-by-using-xaml-designer"></a>パート 1: XAML デザイナーを使用して TextBlock コントロールを追加する

Visual Studio には、XAML UI を簡単に作成するためのツールがいくつか用意されています。

+ コントロールを "_ツールボックス_" から "_XAML デザイナー_" のデザイン サーフェイスにドラッグすると、アプリを実行する前に表示を確認できます。
+ "_プロパティ パネル_" では、デザイナーでアクティブになっているコントロールのすべてのプロパティを表示および設定できます。
+ "_ドキュメント アウトライン_" には、UI の XAML ビジュアル ツリーの親子構造が表示されます。
+ "_XAML エディター_" では、XAML マークアップを直接入力および変更できます。

次の図は、Visual Studio の UI の名前を示したものです。

![Visual Studio のレイアウト](images/xaml-basics/visual-studio-tools.png)

各ツールを使用すると UI の作成が容易になるため、このチュートリアルではこれらをすべて使います。 最初に、XAML デザイナーを使用してコントロールを追加しましょう。

XAML デザイナーを使用してコントロールを追加するには:

1. ソリューション エクスプローラーで、**MainPage.xaml** をダブルクリックして開きます。 この手順により、UI 要素が追加されていない状態で、アプリのメイン ページが表示されます。

2. 先へ進む前に、Visual Studio を調整する必要があります。

    + ソリューション プラットフォームを必ず x86 または x64 (ARM は不可) に設定します。
    + XAML デザイナーのメイン ページで、13.3 インチ デスクトップのプレビューが表示されるように設定します。

    次のように、どちらの設定もウィンドウの最上部近辺にあります。

    ![Visual Studio の設定](images/xaml-basics/layout-vs-settings.png)

    これで、アプリを実行できますが、ほとんど何も表示されません。 このままではつまらないので、いくつか UI 要素を追加してみましょう。

3. [ツールボックス] で **[コモン XAML コントロール]** を展開し、[TextBlock](/uwp/api/windows.ui.xaml.controls.textblock) コントロールを見つけます。 `TextBlock` コントロールをデザイン サーフェイスにドラッグし、ページの左上隅近辺に配置します。

    `TextBlock` コントロールがページに追加され、レイアウトに基づいて最も適していると判断されたいくつかのプロパティが設定されます。 現在アクティブなオブジェクトであることを示すために、`TextBlock` コントロールの周りが青色で強調表示されます。 余白やその他の設定がデザイナーによって追加されます。

    XAML は次のようになります。 このとおりの書式でなくても心配は不要です。 読みやすくするために、ここでは省略しています。

    ```xaml
    <TextBlock HorizontalAlignment="Left"
               Margin="351,44,0,0"
               TextWrapping="Wrap"
               Text="TextBlock"
               VerticalAlignment="Top"/>
    ```

    次の手順では、これらの値を更新します。

4. **[プロパティ]** パネルで、`TextBlock` コントロールの **[名前]** の値を **TitleTextBlock** に変更します。 (まだ `TextBlock` コントロールがアクティブなオブジェクトであることを確認してください)。

5. **[共通]** の下で、**Text** 値を **Collection** に変更します。

    ![TextBlock のプロパティ](images/xaml-basics/text-block-properties.png)

    XAML エディターに、次のような XAML が表示されます。

    ```xaml
    <TextBlock x:Name="TitleTextBlock"
               HorizontalAlignment="Left"
               Margin="351,44,0,0"
               TextWrapping="Wrap"
               Text="Collection"
               VerticalAlignment="Top"/>
    ```

6. `TextBlock` コントロールを配置するには、まず Visual Studio によって追加されたプロパティ値を削除する必要があります。 [ドキュメント アウトライン] で **[TitleTextBlock]** を右クリックして、 **[レイアウト]**  >  **[すべてリセット]** を選択します。

    ![ドキュメント アウトライン](images/xaml-basics/doc-outline-reset.png)

7. **[プロパティ]** パネルの検索ボックスに「`Margin`」と入力します。これにより、`Margin` プロパティを簡単に見つけることができます。 左と下の余白を 24 に設定します。

    ![TextBlock の余白](images/xaml-basics/margins.png)

    余白は、ページ上の要素の配置に関する最も基本的なプロパティです。 レイアウトを微調整するには便利ですが、Visual Studio によって追加されるような大きな余白の値は使用しないことをお勧めします。 さまざまな画面サイズに合わせて UI を調整することが困難になります。

    詳しくは、「[配置、余白、パディング](../layout/alignment-margin-padding.md)」をご覧ください。

8. [ドキュメント アウトライン] で、**TitleTextBlock** を右クリックし、 **[スタイルの編集]**  >  **[リソースの適用]**  >  **[TitleTextBlockStyle]** を選択します。 この手順により、システム定義のスタイルがタイトル テキストに適用されます。

    ```xaml
    <TextBlock x:Name="TitleTextBlock"
               TextWrapping="Wrap"
               Text="Collection"
               Margin="24,0,0,24"
               Style="{StaticResource TitleTextBlockStyle}"/>
    ```

9. **[プロパティ]** パネルの検索ボックスに「**textwrapping**」と入力して、`TextWrapping` プロパティを見つけます。 `TextWrapping` プロパティの_プロパティ マーカー_を選択して、メニューを開きます (プロパティ マーカーは、各プロパティ値の右側に表示される小さな四角形のシンボルです。 プロパティ マーカーは、プロパティが既定値以外に設定されていることを示す黒色になっています)。 **[プロパティ]** メニューの **[リセット]** を選択して `TextWrapping` プロパティをリセットします。

    Visual Studio によってこのプロパティが追加されますが、適用したスタイルに既に設定されているため、ここでは必要ありません。

これで、UI の最初の部分をアプリに追加できました。 では、アプリを実行して結果を確認しましょう。

> [!NOTE]
> チュートリアルのこの部分では、ドラッグしてコントロールを追加しました。 コントロールは、[ツールボックス] でそのコントロールをダブルクリックして追加することもできます。 この方法を試し、Visual Studio によって生成される XAML で違いを確認してください。

## <a name="part-2-add-a-gridview-control-by-using-the-xaml-editor"></a>パート 2: XAML エディターを使用して GridView コントロールを追加する

パート 1 では、XAML デザイナーと Visual Studio から提供される他のいくつかのツールについて使用方法の概要を説明しました。 ここでは、XAML エディターを使用して、XAML マークアップを直接操作します。 XAML に慣れると、この方法の方が効率的であると感じられるかもしれません。

まず、ルート レイアウトである [Grid](/uwp/api/windows.ui.xaml.controls.grid) を [RelativePanel](/uwp/api/windows.ui.xaml.controls.relativepanel) に置き換えます。 `RelativePanel` を使うと、パネルや他の UI に対して UI を簡単に再配置できます。 その便利さは、[XAML アダプティブ レイアウト](xaml-basics-adaptive-layout.md)のチュートリアルで確認できます。

次に、データを表示するための [GridView](/uwp/api/windows.ui.xaml.controls.gridview) コントロールを追加します。

XAML エディターを使用してコントロールを追加するには

1. XAML エディターで、ルートの `Grid` を `RelativePanel` に変更します。

    **変更前**

    ```xaml
    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
          <TextBlock x:Name="TitleTextBlock"
                     Text="Collection"
                     Margin="24,0,0,24"
                     Style="{StaticResource TitleTextBlockStyle}"/>
    </Grid>
    ```

    **変更後**

    ```xaml
    <RelativePanel Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <TextBlock x:Name="TitleTextBlock"
                   Text="Collection"
                   Margin="24,0,0,24"
                   Style="{StaticResource TitleTextBlockStyle}"/>
    </RelativePanel>
    ```

    `RelativePanel` を使用したレイアウトの詳細については、「[レイアウト パネル](../layout/layout-panels.md#relativepanel)」を参照してください。

2. `TextBlock` 要素の下に、**ImageGridView** という名前の `GridView` コントロールを追加します。 `RelativePanel` の "_添付プロパティ_" を設定して、コントロールをタイトル テキストの下に配置し、画面の横幅一杯に表示します。

    **追加する XAML**

    ```xaml
    <GridView x:Name="ImageGridView"
              Margin="0,0,0,8"
              RelativePanel.AlignLeftWithPanel="True"
              RelativePanel.AlignRightWithPanel="True"
              RelativePanel.Below="TitleTextBlock"/>
    ```

    **TextBlock の後**

    ```xaml
    <RelativePanel Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <TextBlock x:Name="TitleTextBlock"
                   Text="Collection"
                   Margin="24,0,0,24"
                   Style="{StaticResource TitleTextBlockStyle}"/>

        <!-- Add the GridView control here. -->

    </RelativePanel>
    ```

    panel 添付プロパティの詳細については、「[レイアウト パネル](../layout/layout-panels.md)」を参照してください。

3. `GridView` コントロールに何かを表示するには、表示するデータのコレクションを追加する必要があります。 **MainPage.xaml.cs** を開き、`GetItemsAsync` メソッドを見つけます。 このメソッドでは、**MainPage** に追加したプロパティである、**Images** と呼ばれるコレクションが設定されます。

    `GetItemsAsync` メソッドの最後に、次のコード行を追加します。

    ```csharp
    ImageGridView.ItemsSource = Images;
    ```

    これにより、`GridView` コントロールの [ItemsSource](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) プロパティにアプリの **Images** コレクションが設定されます。 また、`GridView` コントロールに表示する項目も提供します。

ここでアプリを実行して、すべてが適切に動作しているかどうかを確認しましょう。 結果は次のようになるはずです。

![アプリの UI チェックポイント 1](images/xaml-basics/layout-0.png)

アプリにはイメージがまだ表示されていません。 既定では、コレクション内にあるデータ型の `ToString` 値が表示されます。 次に、データ テンプレートを作成して、データの表示方法を定義します。

> [!NOTE]
> `RelativePanel` を使用したレイアウトの詳細については、「[レイアウト パネル](../layout/layout-panels.md#relativepanel)」を参照してください。 その後で、`TextBlock` と `GridView` の `RelativePanel` 添付プロパティを設定して、さまざまなレイアウトを試してください。

## <a name="part-3-add-a-datatemplate-object-to-display-your-data"></a>パート 3: データを表示する DataTemplate オブジェクトを追加する

ここでは、データの表示方法を `GridView` コントロールに伝える [DataTemplate](/uwp/api/windows.ui.xaml.datatemplate) オブジェクトを作成します。 データ テンプレートについて詳しくは、「[項目コンテナーやテンプレート](../controls-and-patterns/item-containers-templates.md)」をご覧ください。

ここでは、希望のレイアウトを作成するためのプレースホルダーのみを追加します。 [XAML データ バインディング](../../data-binding/xaml-basics-data-binding.md)に関するチュートリアルでは、これらのプレースホルダーを `ImageFileInfo` クラスの実際のデータに置き換えます。 データ オブジェクトがどのようになっているか確認したい場合は、ここで **ImageFileInfo.cs** ファイルを開くこともできます。

データ テンプレートをグリッド ビューに追加するには:

1. **MainPage.xaml** を開きます。

2. 評価を表示するには、[Windows UI ライブラリ](/windows/apps/winui/) (WinUI) NuGet パッケージの [RatingControl](/uwp/api/microsoft.ui.xaml.controls.ratingcontrol) を使用します。 WinUI コントロールの名前空間を指定する XAML 名前空間の参照を追加します。 これを `Page` の開始タグ内 (他の `xmlns:` エントリの直後) に配置します。

    **追加する XAML**

    ```xaml
    xmlns:muxc="using:Microsoft.UI.Xaml.Controls"
    ```

    **最後の `xmlns:` エントリの後**

    ```xaml
    <Page x:Name="page"
      x:Class="PhotoLab.MainPage"
      xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
      xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
      xmlns:local="using:PhotoLab"
      xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
      xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
      xmlns:muxc="using:Microsoft.UI.Xaml.Controls"
      mc:Ignorable="d"
      NavigationCacheMode="Enabled">
    ```

    XAML 名前空間について詳しくは、「[XAML 名前空間と名前空間マッピング](../../xaml-platform/xaml-namespaces-and-namespace-mapping.md)」をご覧ください。

3. ドキュメント アウトラインで、**ImageGridView** を右クリックします。 ショートカット メニューで、 **[追加テンプレートの編集]**  >  **[生成されたアイテムの編集 (ItemTemplate)]**  >  **[空アイテムの作成]** を選択します。 **[リソースの作成]** ダイアログ ボックスが開きます。

4. ダイアログ ボックスで、 **[名前 (キー)]** の値を **ImageGridView_DefaultItemTemplate** に変更し、 **[OK]** を選択します。

    **[OK]** を選択すると、いくつかの処理が行われます。

    + **MainPage.xaml** の `Page.Resources` セクションに `DataTemplate` オブジェクトが追加されます。

        ```xaml
        <Page.Resources>
            <DataTemplate x:Key="ImageGridView_DefaultItemTemplate">
                <Grid/>
            </DataTemplate>
        </Page.Resources>
        ```

    + `GridView` コントロールの [ItemTemplate](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate) プロパティが `DataTemplate` リソースに設定されます。

       ```xaml
           <GridView x:Name="ImageGridView"
                     Margin="0,0,0,8"
                     RelativePanel.AlignLeftWithPanel="True"
                     RelativePanel.AlignRightWithPanel="True"
                     RelativePanel.Below="TitleTextBlock"
                     ItemTemplate="{StaticResource ImageGridView_DefaultItemTemplate}"/>
       ```

5. **ImageGridView_DefaultItemTemplate** リソースで、ルートの `Grid` の Height と Width に **300**、Margin に **8** を割り当てます。 次に、2 つの行を追加し、2 番目の行の Height を **Auto** に設定します。

    **変更前**

    ```xaml
    <Grid/>
    ```

    **変更後**

    ```xaml
    <Grid Height="300"
          Width="300"
          Margin="8">
        <Grid.RowDefinitions>
            <RowDefinition />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>
    </Grid>
    ```

    `Grid` レイアウトの詳細については、「[レイアウト パネル](../layout/layout-panels.md#grid)」を参照してください。

6. `Grid` レイアウトにコントロールを追加します。

    a. [Image](/uwp/api/windows.ui.xaml.controls.image) コントロールをグリッドに追加します。既定では、最初のグリッド行 (行 0) に配置されます。 ここに画像が表示されます。 ただし、ここではアプリのストア ロゴをプレースホルダーとして使用します。

    b. イメージの名前、ファイルの種類、サイズを表示する `TextBlock` コントロールを追加します。 これには、テキスト ブロックを配置するための `StackPanel` コントロールを使用します。 `Grid.Row` 添付プロパティを使用して、最も外側の `StackPanel` を 2 行目 (行 1) に配置します。

    `StackPanel` レイアウトの詳細については、「[レイアウト パネル](../layout/layout-panels.md#stackpanel)」を参照してください。

    c. 外側 (垂直方向) の `StackPanel` コントロールに `RatingControl` を追加します。 これは内側 (水平方向) の `StackPanel` コントロールの後に配置します。

    **最終的なテンプレート**

    ```xaml
    <Grid Height="300"
          Width="300"
          Margin="8">
        <Grid.RowDefinitions>
            <RowDefinition />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <Image x:Name="ItemImage"
               Source="Assets/StoreLogo.png"
               Stretch="Uniform" />

        <StackPanel Orientation="Vertical"
                    Grid.Row="1">
            <TextBlock Text="ImageTitle"
                       HorizontalAlignment="Center"
                       Style="{StaticResource SubtitleTextBlockStyle}" />
            <StackPanel Orientation="Horizontal"
                        HorizontalAlignment="Center">
                <TextBlock Text="ImageFileType"
                           HorizontalAlignment="Center"
                           Style="{StaticResource CaptionTextBlockStyle}" />
                <TextBlock Text="ImageDimensions"
                           HorizontalAlignment="Center"
                           Style="{StaticResource CaptionTextBlockStyle}"
                           Margin="8,0,0,0" />
            </StackPanel>

            <muxc:RatingControl Value="3" IsReadOnly="True"/>
        </StackPanel>
    </Grid>
    ```

ここでアプリを実行し、作成した項目テンプレートで `GridView` コントロールを確認します。 次に、背景色を変更し、グリッド項目の間にスペースを追加します。

![アプリの UI チェックポイント 3](images/xaml-basics/layout-1.png)

## <a name="part-4-modify-the-item-container-style"></a>パート 4: 項目コンテナーのスタイルを変更する

項目のコントロール テンプレートには、選択、ポインター オーバー、フォーカスなどの状態を表示するビジュアルが含まれています。 これらのビジュアルは、データ テンプレートの上または下のいずれかにレンダリングされます。 ここでは、コントロール テンプレートの `Background` プロパティおよび `Margin` プロパティを変更して、`GridView` 項目の背景色を灰色にします。

項目コンテナーを変更するには:

1. ドキュメント アウトラインで、**ImageGridView** を右クリックします。 ショートカット メニューで、 **[追加テンプレートの編集]**  >  **[生成されたアイテム コンテナーの編集 (ItemContainerStyle)]**  >  **[コピーして編集]** を選択します。 **[リソースの作成]** ダイアログ ボックスが開きます。

2. ダイアログ ボックスで、 **[名前 (キー)]** の値を **ImageGridView_DefaultItemContainerStyle** に変更し、 **[OK]** を選択します。

    既定スタイルのコピーが XAML の **Page.Resources** セクションに追加されます。

    ```xaml
    <Style x:Key="ImageGridView_DefaultItemContainerStyle" TargetType="GridViewItem">
        <Setter Property="FontFamily" Value="{ThemeResource ContentControlThemeFontFamily}"/>
        <Setter Property="FontSize" Value="{ThemeResource ControlContentThemeFontSize}"/>
        <Setter Property="Background" Value="{ThemeResource GridViewItemBackground}"/>
        <Setter Property="Foreground" Value="{ThemeResource GridViewItemForeground}"/>
        <Setter Property="TabNavigation" Value="Local"/>
        <Setter Property="IsHoldingEnabled" Value="True"/>
        <Setter Property="HorizontalContentAlignment" Value="Center"/>
        <Setter Property="VerticalContentAlignment" Value="Center"/>
        <Setter Property="Margin" Value="0,0,4,4"/>
        <Setter Property="MinWidth" Value="{ThemeResource GridViewItemMinWidth}"/>
        <Setter Property="MinHeight" Value="{ThemeResource GridViewItemMinHeight}"/>
        <Setter Property="AllowDrop" Value="False"/>
        <Setter Property="UseSystemFocusVisuals" Value="{StaticResource UseSystemFocusVisuals}"/>
        <Setter Property="FocusVisualMargin" Value="-2"/>
        <Setter Property="Template">
            <Setter.Value>
                <ControlTemplate TargetType="GridViewItem">
                <!-- XAML removed for clarity
                    <ListViewItemPresenter ... />
                -->   
                </ControlTemplate>
            </Setter.Value>
        </Setter>
    </Style>
    ```

    `GridViewItem` 既定スタイルでは、多数のプロパティが設定されます。 ただし、テンプレートのコピーでは、変更する必要があるプロパティを保持するだけで十分です。

    前の手順と同様に、**GridView** コントロールの **ItemContainerStyle** プロパティが新しい **Style** リソースに設定されます。

    ```xaml
        <GridView x:Name="ImageGridView"
                  Margin="0,0,0,8"
                  RelativePanel.AlignLeftWithPanel="True"
                  RelativePanel.AlignRightWithPanel="True"
                  RelativePanel.Below="TitleTextBlock"
                  ItemTemplate="{StaticResource ImageGridView_DefaultItemTemplate}"
                  ItemContainerStyle="{StaticResource ImageGridView_DefaultItemContainerStyle}"/>
    ```

3. `Background` と `Margin` 以外のすべての `Setter` 要素を削除します。

4. `Background` プロパティの値を `Gray` に変更します。

    **変更前**

    ```xaml
        <Setter Property="Background" Value="{ThemeResource GridViewItemBackground}"/>
    ```

    **変更後**

    ```xaml
        <Setter Property="Background" Value="Gray"/>
    ```

5. `Margin` プロパティの値を `8` に変更します。

    **変更前**

    ```xaml
        <Setter Property="Margin" Value="0,0,4,4"/>
    ```

    **変更後**

    ```xaml
        <Setter Property="Margin" Value="8"/>
    ```

アプリを実行して、この時点での結果を確認します。 アプリ ウィンドウのサイズを変更します。 画像の再配置は `GridView` コントロールによって自動的に処理されますが、幅によっては、アプリ ウィンドウの右余白が大きくなりすぎることがあります。 このような場合は、イメージを中央に配置した方が見栄えが良くなります。 これについては、次で説明します。

![アプリの UI チェックポイント 3](images/xaml-basics/layout-2.png)

> [!Note]
> 時間に余裕があれば、`Background` プロパティと `Margin` プロパティをさまざまな値に設定して、その結果がどうなるか試してみてください。

## <a name="part-5-apply-some-final-adjustments-to-the-layout"></a>パート 5: レイアウトに最終調整を適用する

画像をページの中央に配置するには、ページ内で `Grid` コントロールの配置を調整する必要があります。 それとも、`GridView` 内で画像の配置を調整する必要があるでしょうか。 どちらでも同じでしょうか? 見てみましょう。

配置について詳しくは、「[配置、余白、パディング](../layout/alignment-margin-padding.md)」をご覧ください。

(この手順で、`GridView` の `Background` プロパティを好きな色に設定することもできます。 その方が、レイアウトの変化がわかりやすくなります。)

画像の配置を変更するには:

1. `GridView` で、[HorizontalAlignment](/uwp/api/windows.ui.xaml.frameworkelement.horizontalalignment) プロパティを `Center` に設定します。

    **変更前**

    ```xaml
        <GridView x:Name="ImageGridView"
                  Margin="0,0,0,8"
                  RelativePanel.AlignLeftWithPanel="True"
                  RelativePanel.AlignRightWithPanel="True"
                  RelativePanel.Below="TitleTextBlock"
                  ItemTemplate="{StaticResource ImageGridView_DefaultItemTemplate}"/>
    ```

    **変更後**

    ```xaml
        <GridView x:Name="ImageGridView"
                  Margin="0,0,0,8"
                  RelativePanel.AlignLeftWithPanel="True"
                  RelativePanel.AlignRightWithPanel="True"
                  RelativePanel.Below="TitleTextBlock"
                  ItemTemplate="{StaticResource ImageGridView_DefaultItemTemplate}"
                  HorizontalAlignment="Center"/>
    ```

2. アプリを実行し、ウィンドウのサイズを変更します。 下へスクロールして、さらに多くのイメージを表示します。

    イメージは中央揃えで表示され、見栄えが良くなっています。 ただし、スクロール バーがウィンドウの端ではなく `GridView` コントロールの端に位置合わせされています。 この問題を解決するには、ページ上で `GridView` を中央揃えにするのではなく、`GridView` 内で画像を中央揃えにします。 少し手間がかかりますが、最終的な見栄えは良くなります。

3. 前の手順で使用した `HorizontalAlignment` 設定を削除します。

4. ドキュメント アウトラインで、**ImageGridView** を右クリックします。 ショートカット メニューで、 **[追加テンプレートの編集]**  >  **[アイテムのレイアウトの編集 (ItemsPanel)]**  >  **[コピーして編集]** を選択します。 **[リソースの作成]** ダイアログ ボックスが開きます。

5. ダイアログ ボックスで、 **[名前 (キー)]** の値を **ImageGridView_ItemsPanelTemplate** に変更し、 **[OK]** を選択します。

    既定の **ItemsPanelTemplate** のコピーが XAML の **Page.Resources** セクションに追加されます (今回も、このリソースを参照するように `GridView` が更新されます)。

    ```xaml
    <ItemsPanelTemplate x:Key="ImageGridView_ItemsPanelTemplate">
        <ItemsWrapGrid Orientation="Horizontal" />
    </ItemsPanelTemplate>
    ```

    アプリ内の各種コントロールをレイアウトするためにさまざまなパネルを使用してきましたが、`GridView` にも、項目のレイアウトを管理する内部パネルがあります。 このパネル ([ItemsWrapGrid](/uwp/api/windows.ui.xaml.controls.itemswrapgrid)) にアクセスできるようになったため、パネルのプロパティを変更して、`GridView` コントロール内にある項目のレイアウトを変更します。

6. `ItemsWrapGrid` で、`HorizontalAlignment` プロパティを `Center` に設定します。

    **変更前**

    ```xaml
    <ItemsPanelTemplate x:Key="ImageGridView_ItemsPanelTemplate">
        <ItemsWrapGrid Orientation="Horizontal" />
    </ItemsPanelTemplate>
    ```

    **変更後**

    ```xaml
    <ItemsPanelTemplate x:Key="ImageGridView_ItemsPanelTemplate">
        <ItemsWrapGrid Orientation="Horizontal"
                       HorizontalAlignment="Center"/>
    </ItemsPanelTemplate>
    ```

7. アプリを実行し、ウィンドウのサイズをもう一度変更します。 下へスクロールして、さらに多くのイメージを表示します。

![アプリの UI チェックポイント 4](images/xaml-basics/layout-3.png)

これで、スクロール バーがウィンドウの端に位置合わせされました。 おめでとうございます! アプリの基本的な UI を作成できました。

## <a name="go-further"></a>次の手順

基本的な UI を作成したので、これらの他のチュートリアルを参照してください。 これらは PhotoLab サンプルにも基づいています。

* 「[XAML データ バインディングのチュートリアル](../../data-binding/xaml-basics-data-binding.md)」では、実際のイメージとデータを追加します。
* 「[XAML アダプティブ レイアウトのチュートリアル](xaml-basics-adaptive-layout.md)」では、さまざまな画面サイズに合わせて UI を調整します。


## <a name="get-the-final-version-of-the-photolab-sample"></a>PhotoLab サンプルの最終バージョンを入手する

このチュートリアルは、写真編集アプリについての完全な説明ではありません。 そのため、カスタム アニメーションやアダプティブ レイアウトなどのその他の機能については、[最終バージョン](https://github.com/Microsoft/Windows-appsample-photo-lab)を確認してください。