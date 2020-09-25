---
description: ハイコントラストテーマがアクティブな場合に Windows アプリを使用できるようにするために必要な手順について説明します。
ms.assetid: FD7CA6F6-A8F1-47D8-AA6C-3F2EC3168C45
title: ハイ コントラスト テーマ
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b5e9b823ca335370f6cde22ef6417a6823851c18
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/24/2020
ms.locfileid: "91219875"
---
# <a name="high-contrast-themes"></a>ハイ コントラスト テーマ  

Windows では、OS やアプリでハイ コントラスト テーマがサポートされていて、必要に応じて有効にすることができます。 ハイ コントラスト テーマは、少ない数のコントラスト カラーで構成されるパレットを使い、インターフェイスを見やすくします。

![淡色テーマと黒のハイ コントラスト テーマで表示された電卓。](images/high-contrast-calculators.png)

*淡色テーマと黒のハイ コントラスト テーマで表示された電卓。*

ハイ コントラスト テーマに切り替えるには、*[設定]、[簡単操作]、[ハイ コントラスト]* の順に選択します。

> [!NOTE]
> ハイ コントラスト テーマは、淡色テーマおよび濃色テーマとは異なることに注意してください。淡色テーマと濃色テーマのカラー パレットは色の種類が豊富で、ハイ コントラストとは見なされません。 淡色テーマと濃色テーマについて詳しくは、「[色](../style/color.md)」をご覧ください。

コモン コントロールでは、ハイ コントラストが無償で完全にサポートされていますが、UI をカスタマイズする場合は注意する必要があります。 ハイ コントラストに関する最も一般的なバグは、コントロールにインラインで色をハードコーディングすることで生じます。

```xaml
<!-- Don't do this! -->
<Grid Background="#E6E6E6">

<!-- Instead, create BrandedPageBackgroundBrush and do this. -->
<Grid Background="{ThemeResource BrandedPageBackgroundBrush}">
```

最初の例で `#E6E6E6` をインラインで設定すると、グリッドはすべてのテーマでその背景色を保持します。 ユーザーが黒のハイ コントラスト テーマに切り替えたら、彼らはアプリの背景が黒で表示されることを期待します。 `#E6E6E6` はほぼ白一色であるため、ユーザーによってはアプリを操作できない場合があります。

2 番目の例では、[**ResourceDictionary**](/uwp/api/Windows.UI.Xaml.ResourceDictionary) 要素の専用プロパティである [**ThemeDictionaries**](/uwp/api/windows.ui.xaml.resourcedictionary.themedictionaries) コレクション内の色を参照するために [**{ThemeResource} マークアップ拡張**](../../xaml-platform/themeresource-markup-extension.md)を使っています。 **ThemeDictionaries** により、XAML はユーザーの現在のテーマに基づいて自動的に色を変えることができます。

## <a name="theme-dictionaries"></a>テーマ ディクショナリ

システムの既定の色を変更する必要がある場合は、アプリの ThemeDictionaries コレクションを作成します。

1. まず、適切なプラミングを作成します (プラミングがまだない場合)。 App.xaml で、 **ThemeDictionaries** コレクションを作成します。これには、少なくとも **Default** と **systeminformation.highcontrast** が含まれます。
2. **Default** では、必要な種類の [Brush](/uwp/api/Windows.UI.Xaml.Media.Brush) を作成します。通常は、**SolidColorBrush** です。 このクラスに対して、具体的な使用目的を示す *x:Key* 名を指定します。
3. 必要な **Color** を割り当てます。
4. その **ブラシ** を **systeminformation.highcontrast**にコピーします。

``` xaml
<Application.Resources>
    <ResourceDictionary>
        <ResourceDictionary.ThemeDictionaries>
            <!-- Default is a fallback if a more precise theme isn't called
            out below -->
            <ResourceDictionary x:Key="Default">
                <SolidColorBrush x:Key="BrandedPageBackgroundBrush" Color="#E6E6E6" />
            </ResourceDictionary>

            <!-- Optional, Light is used in light theme.
            If included, Default will be used for Dark theme -->
            <ResourceDictionary x:Key="Light">
                <SolidColorBrush x:Key="BrandedPageBackgroundBrush" Color="#E6E6E6" />
            </ResourceDictionary>

            <!-- HighContrast is used in all high contrast themes -->
            <ResourceDictionary x:Key="HighContrast">
                <SolidColorBrush x:Key="BrandedPageBackgroundBrush" Color="#E6E6E6" />
            </ResourceDictionary>
        </ResourceDictionary.ThemeDictionaries>
    </ResourceDictionary>
</Application.Resources>
```

最後に、ハイ コントラストで使用する色を決定します。これについては、次のセクションで説明します。

> [!NOTE]
> **HighContrast** のみが、利用可能なキー名というわけではありません。 **HighContrastBlack**、**HighContrastWhite**、**HighContrastCustom** も利用可能なキー名です。 ほとんどの場合、必要なのは **systeminformation.highcontrast** だけです。

## <a name="high-contrast-colors"></a>ハイ コントラストの色

*[設定]、[簡単操作]、[ハイ コントラスト]* の順に選択すると、既定の 4 つのハイ コントラスト テーマが表示されます。 


![ハイ コントラスト設定](images/high-contrast-settings.png)  

*ユーザーがオプションを選ぶと、ページにはプレビューが表示されます。*  

![ハイ コントラスト リソース](images/high-contrast-resources.png)  

*プレビューのすべての色見本をクリックして、その値を変更できます。すべての見本は、直接 XAML 色リソースにもマップされます。*  

各 **SystemColor*Color** リソースは、ユーザーがハイ コントラスト テーマに切り替えたときに自動的に色を更新する変数です。 各リソースをいつどこで使用するかについてのガイドラインを以下に示します。

リソース | 使用法 |
|--------|-------|
**SystemColorWindowTextColor** | 本文、見出し、一覧など、操作できないテキスト |
| **SystemColorHotlightColor** | ハイパーリンク |
| **SystemColorGrayTextColor** | 無効な UI |
| **SystemColorHighlightTextColor** | 処理中、選択されている、または現在操作されているテキストや UI の前景色 |
| **SystemColorHighlightColor** | 処理中、選択されている、または現在操作されているテキストや UI の背景色 |
| **SystemColorButtonTextColor** | ボタンなど、操作可能な UI の前景色 |
| **SystemColorButtonFaceColor** | ボタンなど、操作可能な UI の背景色 |
| **SystemColorWindowColor** | ページ、ウィンドウ、ポップアップ、およびバーの背景 |

既存のアプリ、スタート画面、またはコモン コントロールを確認すると、ハイ コントラストのデザインの参考になります。

**すべきこと**

* 可能な限り、背景と前景の組み合わせを考慮します。
* アプリの実行中に、4 つのハイ コントラスト テーマをすべてテストします。 ユーザーがテーマを切り替えたときに、アプリを再起動しなくても良いようにします。
* 一貫性を保ちます。

**やってはいけないこと**

* **SystemColor*Color** リソースを使って **HighContrast** テーマの色をハードコーディングしないようにします。
* 見栄えを良くすることを目的としてカラー リソースを選ばないようにします。 カラー リソースはテーマによって変わることに注意してください。
* **SystemColorGrayTextColor** を、セカンダリ テキストの本文やヒント目的の本文に使用しないようにします。


先ほどの例を続けるには、**BrandedPageBackgroundBrush** のリソースを選択する必要があります。 背景に使用されることを名前が示しているため、**SystemColorWindowColor** が最適です。

``` xaml
<Application.Resources>
    <ResourceDictionary>
        <ResourceDictionary.ThemeDictionaries>
            <!-- Default is a fallback if a more precise theme isn't called
            out below -->
            <ResourceDictionary x:Key="Default">
                <SolidColorBrush x:Key="BrandedPageBackgroundBrush" Color="#E6E6E6" />
            </ResourceDictionary>

            <!-- Optional, Light is used in light theme.
            If included, Default will be used for Dark theme -->
            <ResourceDictionary x:Key="Light">
                <SolidColorBrush x:Key="BrandedPageBackgroundBrush" Color="#E6E6E6" />
            </ResourceDictionary>

            <!-- HighContrast is used in all high contrast themes -->
            <ResourceDictionary x:Key="HighContrast">
                <SolidColorBrush x:Key="BrandedPageBackgroundBrush" Color="{ThemeResource SystemColorWindowColor}" />
            </ResourceDictionary>
        </ResourceDictionary.ThemeDictionaries>
    </ResourceDictionary>
</Application.Resources>
```

その後、アプリで背景を設定できます。

```xaml
<Grid Background="{ThemeResource BrandedPageBackgroundBrush}">
```

**BrandedPageBackgroundBrush**を参照するために、1回は**Systemcolorwindowcolor**を参照するために2回、もう一度**使用されるかどう \{ \} **かに注意してください。 実行時に正しいテーマを使うためには、両方の参照がアプリに必要です。 ここで、アプリでハイ コントラストの機能をテストすると良いでしょう。 ハイ コントラスト テーマに切り替えると、グリッドの背景が自動的に更新されます。 また、別のハイ コントラスト テーマに切り替えたときにも更新されます。

## <a name="when-to-use-borders"></a>境界線を使う状況

ページ、ウィンドウ、ポップアップ、およびバーでは、ハイ コントラストの背景に **SystemColorWindowColor** を使う必要があります。 UI で重要な境界を維持する必要がある場合、ハイ コントラストのみの境界線を追加します。

![ページの他の部分と区別されたナビゲーション ウィンドウ](images/high-contrast-actions-content.png)  

*ナビゲーションウィンドウとページは、どちらも同じ背景色をハイコントラストで共有します。ハイコントラストのみの境界線を分割することが重要です。*


## <a name="list-items"></a>項目の一覧表示

ハイ コントラストでは、ポイント、押下、または選択時に [ListView](/uwp/api/windows.ui.xaml.controls.listview) の項目の背景が **SystemColorHighlightColor** に設定されます。 複雑なリスト項目では、項目のポイント、押下、選択時に色の反転に失敗するというバグがよく生じ、 項目が読めなくなってしまいます。

![単色テーマと黒のハイ コントラスト テーマの簡単なリスト](images/high-contrast-list1.png)

*ライトテーマ (左) とハイコントラスト黒のテーマ (右) の単純なリスト。2番目の項目が選択されています。テキストの色がハイコントラストで反転されていることに注意してください。*


### <a name="list-items-with-colored-text"></a>テキストに色が付いているリスト項目

問題の原因の 1 つが、ListView の [DataTemplate](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate) で TextBlock.Foreground を設定することです。 TextBlock.Foreground は一般的に、視覚的な階層を確立するために設定します。 Foreground プロパティは [ListViewItem](/uwp/api/windows.ui.xaml.controls.listviewitem) で設定され、DataTemplate 内の TextBlock は、項目がポイント、押下、または選択されたときに適切な Foreground 色を継承します。 ところが、Foreground を設定すると継承が中断されてしまいます。

![淡色テーマと黒のハイ コントラスト テーマの複雑なリスト](images/high-contrast-list2.png)

*明るいテーマの複合リスト (左) とハイコントラスト黒のテーマ (右側)。ハイコントラストでは、選択した項目の2行目を反転できませんでした。*  

この問題を回避するには、**ThemeDictionaries** コレクションに含まれている Style を使って、条件付きで Foreground を設定します。 **Systeminformation.highcontrast**では、**前景色**が**secondarybodytextblockstyle**によって設定されていないため、その色は正しく反転されます。

```xaml
<!-- In App.xaml... -->
<ResourceDictionary.ThemeDictionaries>
    <ResourceDictionary x:Key="Default">
        <Style
            x:Key="SecondaryBodyTextBlockStyle"
            TargetType="TextBlock"
            BasedOn="{StaticResource BodyTextBlockStyle}">
            <Setter Property="Foreground" Value="{StaticResource SystemControlForegroundBaseMediumBrush}" />
        </Style>
    </ResourceDictionary>

    <ResourceDictionary x:Key="Light">
        <Style
            x:Key="SecondaryBodyTextBlockStyle"
            TargetType="TextBlock"
            BasedOn="{StaticResource BodyTextBlockStyle}">
            <Setter Property="Foreground" Value="{StaticResource SystemControlForegroundBaseMediumBrush}" />
        </Style>
    </ResourceDictionary>

    <ResourceDictionary x:Key="HighContrast">
        <!-- The Foreground Setter is omitted in HighContrast -->
        <Style
            x:Key="SecondaryBodyTextBlockStyle"
            TargetType="TextBlock"
            BasedOn="{StaticResource BodyTextBlockStyle}" />
    </ResourceDictionary>
</ResourceDictionary.ThemeDictionaries>

<!-- Usage in your DataTemplate... -->
<DataTemplate>
    <StackPanel>
        <TextBlock Style="{StaticResource BodyTextBlockStyle}" Text="Double line list item" />

        <!-- Note how ThemeResource is used to reference the Style -->
        <TextBlock Style="{ThemeResource SecondaryBodyTextBlockStyle}" Text="Second line of text" />
    </StackPanel>
</DataTemplate>
```


## <a name="detecting-high-contrast"></a>ハイ コントラストを検出する

[**AccessibilitySettings**](/uwp/api/Windows.UI.ViewManagement.AccessibilitySettings) クラスのメンバーを使えば、現在のテーマがハイ コントラストであるかどうかをプログラムで確認することができます。

> [!NOTE]
>  注 アプリが初期化され、既にコンテンツが表示されているスコープから **AccessibilitySettings** コンストラクターを呼び出すようにします。

## <a name="related-topics"></a>関連トピック  
* [アクセシビリティ](accessibility.md)
* [UI コントラストと設定のサンプル](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/XAML%20high%20contrast%20style%20sample%20(Windows%208))
* [XAML アクセシビリティ サンプル](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/XAML%20accessibility%20sample)
* [XAML ハイ コントラスト サンプル](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/XAML%20high%20contrast%20style%20sample%20(Windows%208))
* [**AccessibilitySettings**](/uwp/api/Windows.UI.ViewManagement.AccessibilitySettings)
