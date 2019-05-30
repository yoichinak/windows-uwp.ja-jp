---
description: Template settings (テンプレート設定) クラス
title: Template settings (テンプレート設定) クラス
ms.assetid: CAE933C6-EF13-465A-9831-AB003AF23907
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b0d68b31a1574d23dd66de37e2f708d8d77fc052
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/29/2019
ms.locfileid: "66371120"
---
# <a name="template-settings-classes"></a>Template settings (テンプレート設定) クラス


## <a name="prerequisites"></a>前提条件

UI にコントロールを追加し、コントロールのプロパティを設定して、イベント ハンドラーをアタッチできることを前提としています。 アプリにコントロールを追加する方法については、「[コントロールの追加とイベントの処理](https://docs.microsoft.com/windows/uwp/controls-and-patterns/controls-and-events-intro)」をご覧ください。 読者が既定のテンプレートのコピーを作成、編集して、コントロール用のカスタム テンプレートを定義する方法の基本を知っていることも前提にしています。 この詳細については、次を参照してください。[クイック スタート。コントロール テンプレート](https://docs.microsoft.com/previous-versions/windows/apps/hh465374(v=win.10))します。

## <a name="the-scenario-for-templatesettings-classes"></a>**TemplateSettings** クラスのシナリオ

**TemplateSettings** クラスは、コントロールの新しいコントロール テンプレートを定義する際に使用されるプロパティのセットを提供します。 プロパティには、UI 要素の特定部分のサイズのピクセル値などの値があります。 これらの値は、通常は上書きまたはアクセスすることが簡単ではないコントロール ロジックから計算した値であることもあります。 一部のプロパティには、部分の切り替えとアニメーションを制御する **From** および **To** 値としての目的があるため、関連する **TemplateSettings** プロパティがペアになっています。

いくつかの **TemplateSettings** クラスがあります。 それらすべては [**Windows.UI.Xaml.Controls.Primitives**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives) 名前空間に含まれています。 ここでは、クラスの一覧、および関連コントロールの **TemplateSettings** プロパティへのリンクを示します。 この **TemplateSettings** プロパティは、コントロールの **TemplateSettings** 値へのアクセス方法であり、プロパティへのテンプレート バインドを確立することができます。

-   [**ComboBoxTemplateSettings**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ComboBoxTemplateSettings): の値[ **ComboBox.TemplateSettings**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.combobox.templatesettings)
-   [**GridViewItemTemplateSettings**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.GridViewItemTemplateSettings): の値[ **GridViewItem.TemplateSettings**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.gridviewitem.templatesettings)
-   [**ListViewItemTemplateSettings**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ListViewItemTemplateSettings): の値[ **ListViewItem.TemplateSettings**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewitem.templatesettings)
-   [**ProgressBarTemplateSettings**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ProgressBarTemplateSettings): の値[ **ProgressBar.TemplateSettings**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.progressbar.templatesettings)
-   [**ProgressRingTemplateSettings**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ProgressRingTemplateSettings): の値[ **ProgressRing.TemplateSettings**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.progressring.templatesettings)
-   [**SettingsFlyoutTemplateSettings**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.SettingsFlyoutTemplateSettings): の値[ **SettingsFlyout.TemplateSettings**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.settingsflyout.templatesettings)
-   [**ToggleSwitchTemplateSettings**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleSwitchTemplateSettings): の値[ **ToggleSwitch.TemplateSettings**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch.templatesettings)
-   [**ToolTipTemplateSettings**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToolTipTemplateSettings): の値[ **ToolTip.TemplateSettings**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.tooltip.templatesettings)

**TemplateSettings** プロパティは常に、コードではなく XAML で使うことを想定しています。 親コントロールの読み取り専用 **TemplateSettings** プロパティの読み取り専用サブプロパティです。 [  **Control**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control) ベースの新しいクラスを作成し、そのためコントロール ロジックに影響を与える可能性があるカスタム コントロールの高度なシナリオについては、コントロールから再度テンプレートを作成するユーザーに役立つ情報を伝えるために、コントロールにカスタムの **TemplateSettings** プロパティを定義することをお勧めします。 そのプロパティの読み取り専用の値として、テンプレートの測定値、アニメーションの配置などに関係する情報項目ごとに、読み取り専用プロパティを持つコントロールに関連した新しい **TemplateSettings** クラスを定義し、コントロール ロジックを使って初期化されたそのクラスのランタイム インスタンスを呼び出し元に渡します。 **TemplateSettings** クラスは [**DependencyObject**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyObject) から派生し、プロパティでプロパティ変更コールバックに依存関係プロパティ システムを使用できるようにします。 ただし、プロパティの依存関係プロパティの識別子はパブリック API として公開されないため、**TemplateSettings** プロパティは、呼び出し元に対して読み取り専用にすることを意図したものです。

## <a name="how-to-use-templatesettings-in-a-control-template"></a>コントロール テンプレートで **TemplateSettings** を使う方法

次に示す例は、基盤になる既定の XAML コントロール テンプレートに含まれています。 この特定の例は、[**ProgressRing**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ProgressRing) の既定のテンプレートを基にしています。

```xml
<Ellipse
    x:Name="E1"
    Style="{StaticResource ProgressRingEllipseStyle}"
    Width="{Binding RelativeSource={RelativeSource TemplatedParent}, 
        Path=TemplateSettings.EllipseDiameter}"
    Height="{Binding RelativeSource={RelativeSource TemplatedParent}, 
        Path=TemplateSettings.EllipseDiameter}"
    Margin="{Binding RelativeSource={RelativeSource TemplatedParent}, 
        Path=TemplateSettings.EllipseOffset}"
    Fill="{TemplateBinding Foreground}"/>
```

[  **ProgressRing**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ProgressRing) テンプレートの完全な XAML は数百行あり、これは一部のみの抜粋です。 次の XAML は、進行状況不定の回転アニメーションを示す 6 つの [**Ellipse**](/uwp/api/Windows.UI.Xaml.Shapes.Ellipse) 要素の 1 つであるコントロール部を定義しています。 開発者は、アニメーションの進行状況を表す円が気に入らない場合は、別のグラフィック プリミティブや別の基本図形を使うことができます。 たとえば、正方形に配置した一連の [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) 要素を使った **ProgressRing** を作成できます。 その場合、新しいテンプレートの個別の各 **Rectangle** コンポーネントは次のようになります。

```xml
<Rectangle
    x:Name="R1"
    Width="{Binding RelativeSource={RelativeSource TemplatedParent}, 
        Path=TemplateSettings.EllipseDiameter}"
    Height="{Binding RelativeSource={RelativeSource TemplatedParent}, 
        Path=TemplateSettings.EllipseDiameter}"
    Margin="{Binding RelativeSource={RelativeSource TemplatedParent}, 
        Path=TemplateSettings.EllipseOffset}"
    Fill="{TemplateBinding Foreground}"/>
```

**TemplateSettings** プロパティがここで有用な理由は、[**ProgressRing**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ProgressRing) の基本的なコントロールのロジックに基づく計算値であることです。 計算では、**ProgressRing** の [**ActualWidth**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.actualwidth) および [**ActualHeight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.actualheight) 全体を分割し、テンプレート内のアニメーションの各要素に計算された測定値を割り当てて、テンプレート パーツをコンテンツに合わせてサイズ変更できるようにします。

既定の XAML コントロール テンプレートから抜粋したもう 1 つの使用例を次に示します。ここでは、アニメーションの **From** と **To** であるプロパティ セットのいずれかを示します。 これは、[**ComboBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ComboBox) 既定テンプレートからの抜粋です。

```xml
<VisualStateGroup x:Name="DropDownStates">
    <VisualState x:Name="Opened">
        <Storyboard>
            <SplitOpenThemeAnimation
               OpenedTargetName="PopupBorder"
               ContentTargetName="ScrollViewer"
               ClosedTargetName="ContentPresenter"
               ContentTranslationOffset="0"
               OffsetFromCenter="{Binding RelativeSource={RelativeSource TemplatedParent}, 
                 Path=TemplateSettings.DropDownOffset}"
               OpenedLength="{Binding RelativeSource={RelativeSource TemplatedParent}, 
                 Path=TemplateSettings.DropDownOpenedHeight}"
               ClosedLength="{Binding RelativeSource={RelativeSource TemplatedParent},
                 Path=TemplateSettings.DropDownClosedHeight}" />
        </Storyboard>
   </VisualState>
...
</VisualStateGroup>
```

ここでも、テンプレートの XAML は量が多いため、一部のみを抜粋して示しています。 これは、それぞれ同じ [**ComboBoxTemplateSettings**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ComboBoxTemplateSettings) プロパティを使用する、複数ある状態とテーマのアニメーションのうちの 1 つにすぎません。 [  **ComboBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ComboBox) では、バインドを利用して **ComboBoxTemplateSettings** 値を使うと、テンプレート内の関連アニメーションが共有値に基づく位置で停止および開始し、スムーズに遷移します。

**注**  使用するときに**TemplateSettings**コントロール テンプレートの一部として値を値の型に一致するプロパティを設定していることを確認します。 そうしないと、バインドのターゲットの型を **TemplateSettings** 値の異なるソース型から変換できるように、バインドの値のコンバーターを作成することが必要になる場合があります。 詳しくは、「[**IValueConverter**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.IValueConverter)」をご覧ください。

## <a name="related-topics"></a>関連トピック

* [クイック スタート:コントロール テンプレート](https://docs.microsoft.com/previous-versions/windows/apps/hh465374(v=win.10))

