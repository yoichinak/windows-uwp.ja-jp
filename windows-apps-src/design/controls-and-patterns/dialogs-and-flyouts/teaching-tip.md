---
description: 教育のヒントは、コンテキスト情報を提供する半永続的で内容豊富なポップアップです。
title: 教育のヒント
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
pm-contact: yulikl
design-contact: kimsea
dev-contact: niallm
ms.custom: 19H1
ms.localizationpriority: medium
ms.openlocfilehash: 8d4322e5d5dcdfad768b9c87b555093e42becd7e
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2020
ms.locfileid: "93033315"
---
# <a name="teaching-tip"></a>教育のヒント

教育のヒントは、コンテキスト情報を提供する半永続的で内容豊富なポップアップです。 これはユーザーのエクスペリエンスを高める可能性がある重要および新規の機能についてユーザーに通知したり、リマインドしたり、説明したりする場合によく使用されます。

教育のヒントは、簡易非表示にすることも、閉じるために明示的な操作を必要とすることもできます。 教育のヒントはテールを使用して特定の UI 要素をターゲットにすることができますが、テールやターゲットがなくても使用できます。

**Windows UI ライブラリを入手する**

:::row:::
   :::column:::
      ![WinUI ロゴ](../images/winui-logo-64x64.png)
   :::column-end:::
   :::column span="3":::
      **TeachingTip** コントロールでは、Windows アプリのための新しいコントロールと UI 機能を含む NuGet パッケージである Windows UI ライブラリが必要になります。 インストール手順などについて詳しくは、「[Windows UI Library (Windows UI ライブラリ)](/uwp/toolkits/winui/)」をご覧ください。
   :::column-end:::
   :::column:::

   :::column-end:::
:::row-end:::

> **Windows UI ライブラリ API:** [TeachingTip クラス](/uwp/api/microsoft.ui.xaml.controls.teachingtip)

> [!TIP]
> このドキュメントでは、XAML で **muxc** エイリアスを使って、プロジェクトに含めた Windows UI Library API を表します。 [Page](/uwp/api/windows.ui.xaml.controls.page) 要素にこれを追加しました。`xmlns:muxc="using:Microsoft.UI.Xaml.Controls"`
>
>コードビハインドでは、C# でも **muxc** エイリアスを使って、プロジェクトに含めた Windows UI Library API を表します。 この **using** ステートメントは、ファイルの先頭に追加されています。`using muxc = Microsoft.UI.Xaml.Controls;`

## <a name="is-this-the-right-control"></a>これは適切なコントロールですか?

**TeachingTip** コントロールを使用して、新規または重要な更新および機能にユーザーの注意を向けたり、重要ではないがユーザーのエクスペリエンスを改善できるオプションをユーザーにリマインドしたり、タスクの完了方法をユーザーに説明したりします。

教育のヒントは一時的なものであるため、エラーまたは重要な状態変更をユーザーに知らせるためのコントロールとしては推奨されません。

## <a name="examples"></a>例

<table>
<th align="left">XAML コントロール ギャラリー<th>
<tr>
<td><img src="../images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML コントロール ギャラリー</strong> アプリがインストールされている場合、こちらをクリックして<a href="xamlcontrolsgallery:/item/TeachingTip">アプリを開き、TeachingTip の動作を確認</a>してください。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML コントロール ギャラリー アプリを入手する (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">ソース コード (GitHub) を入手する</a></li>
    </ul>
</td>
</tr>
</table>

教育のヒントには、以下の主なものを含むいくつかの構成を指定できます。

教育のヒントは、表示する情報のコンテキスト上の明確さを高めるために、そのテールを使用して特定の UI 要素をターゲットとすることができます。

![保存ボタンをターゲットに指定した教育のヒントが表示されているサンプル アプリ。 ヒントのタイトルには "Saving automatically" (自動的に保存) と記載されており、サブタイトルには "We save your changes as you go - so you never have to." (変更内容は作業中に保存されるため、保存操作は不要です) と記載されています。 教育のヒントの右上隅に、閉じるボタンがあります。](../images/teaching-tip-targeted.png)

表示される情報が特定の UI 要素に関連しない場合、テールを削除することによって、ターゲットを指定しない教育のヒントを作成できます。

![教育のヒントが右下隅に表示されているサンプル アプリ。 ヒントのタイトルには "Saving automatically" (自動的に保存) と記載されており、サブタイトルには "We save your changes as you go - so you never have to." (変更内容は作業中に保存されるため、保存操作は不要です) と記載されています。 教育のヒントの右上隅に、閉じるボタンがあります。](../images/teaching-tip-non-targeted.png)

教育のヒントでは、ユーザーが上の隅にある [X] ボタンまたは下部にある [閉じる] ボタンを使用して閉じることを必須にできます。 教育のヒントでは、簡易非表示を有効にすることもでき、この場合は閉じるボタンが表示されません。代わりに、教育のヒントは、ユーザーがスクロールしたりアプリケーションの他の要素とやり取りしたりすると閉じられます。 このような動作であるため、スクロール可能な領域にヒントを配置する必要がある場合は、簡易非表示のヒントが最適なソリューションとなります。

![簡易非表示の教育のヒントが右下隅に表示されているサンプル アプリ。 ヒントのタイトルには "Saving automatically" (自動的に保存) と記載されており、サブタイトルには "We save your changes as you go - so you never have to." (変更内容は作業中に保存されるため、保存操作は不要です) と記載されています。](../images/teaching-tip-light-dismiss.png)

### <a name="create-a-teaching-tip"></a>教育のヒントを作成する

ここでは、タイトルとサブタイトルが付いた TeachingTip の既定の外観を示す、ターゲットを指定した教育のヒント コントロールの XAML を扱います。
なお、教育のヒントは要素ツリーまたはコード ビハインドの任意の場所に表示できます。 以下の例では ResourceDictionary 内に配置されます。

```xaml
<Button x:Name="SaveButton" Content="Save">
    <Button.Resources>
        <muxc:TeachingTip x:Name="AutoSaveTip"
            Target="{x:Bind SaveButton}"
            Title="Save automatically"
            Subtitle="When you save your file to OneDrive, we save your changes as you go - so you never have to.">
        </muxc:TeachingTip>
    </Button.Resources>
</Button>
```

```csharp
public MainPage()
{
    this.InitializeComponent();

    if(!HaveExplainedAutoSave())
    {
        AutoSaveTip.IsOpen = true;
        SetHaveExplainedAutoSave();
    }
}
```

ボタンと教育のヒントを含むページが表示される場合の結果を次に示します。

![保存ボタンをターゲットに指定した教育のヒントが表示されているサンプル アプリ。 ヒントのタイトルには "Saving automatically" (自動的に保存) と記載されており、サブタイトルには "We save your changes as you go - so you never have to." (変更内容は作業中に保存されるため、保存操作は不要です) と記載されています。 教育のヒントの右上隅に、閉じるボタンがあります。](../images/teaching-tip-targeted.png)

上の例では、教育のヒントのタイトルとサブタイトルを設定するために、[Title](/uwp/api/microsoft.ui.xaml.controls.teachingtip.title) および [Subtitle](/uwp/api/microsoft.ui.xaml.controls.teachingtip.subtitle) プロパティを使用しています。 [Target](/uwp/api/microsoft.ui.xaml.controls.teachingtip.target) プロパティは、それ自体とボタンとの間の視覚的な接続を確立するために、"SaveButton" に設定されています。 教育のヒントを表示するには、その [IsOpen](/uwp/api/microsoft.ui.xaml.controls.teachingtip.isopen) プロパティを `true` に設定します。

### <a name="non-targeted-tips"></a>ターゲット非指定のヒント

すべてのヒントが画面の要素に関連しているわけではありません。 これらのシナリオではターゲットを設定しないでください。代わりに、教育のヒントは XAML ルートの端を基準として表示されます。 ただし教育のヒントでは、[TailVisibility](/uwp/api/microsoft.ui.xaml.controls.teachingtip.tailvisibility) プロパティを "Collapsed" に設定することによって、UI 要素を基準とした配置を維持しながらテールを削除することができます。 次の例は、ターゲット非指定の教育のヒントを表しています。

```xaml
<Button x:Name="SaveButton" Content="Save" />

<muxc:TeachingTip x:Name="AutoSaveTip"
    Title="Saving automatically"
    Subtitle="We save your changes as you go - so you never have to.">
</muxc:TeachingTip>
```

この例では、TeachingTip が ResourceDictionary やコード ビハインドではなく要素ツリー内にあることに注意してください。 これは動作に影響せず、TeachingTip は開いたときにのみ表示され、レイアウト スペースを占有しません。

![教育のヒントが右下隅に表示されているサンプル アプリ。 ヒントのタイトルには "Saving automatically" (自動的に保存) と記載されており、サブタイトルには "We save your changes as you go - so you never have to." (変更内容は作業中に保存されるため、保存操作は不要です) と記載されています。 教育のヒントの右上隅に、閉じるボタンがあります。](../images/teaching-tip-non-targeted.png)

### <a name="preferred-placement"></a>推奨される配置

教育のヒントは [PreferredPlacement](/uwp/api/microsoft.ui.xaml.controls.teachingtip.preferredplacement) プロパティを使用して、ポップアップの [FlyoutPlacementMode](/uwp/api/Windows.UI.Xaml.Controls.Primitives.FlyoutPlacementMode) 配置動作を複製します。 既定の配置モードでは、ターゲット指定の教育のヒントはターゲットの上に配置しようとし、ターゲット非指定の教育のヒントは XAML ルートの下部中央に配置しようとします。 ポップアップと同様に、優先配置モードでは教育のヒントを表示するスペースがない場合は、別の配置モードが自動的に選択されます。

ゲームパッド入力を予測するアプリケーションの場合は、「[ゲームパッドとリモコンの操作]( ../../input/gamepad-and-remote-interactions.md#xy-focus-navigation-and-interaction)」を参照してください。 アプリの UI で考えられるすべての構成を使用して、それぞれの教育のヒントについてのゲームパッドのアクセシビリティをテストすることをお勧めします。

PreferredPlacement が "BottomLeft" に設定されたターゲット指定の教育のヒントは、テールがターゲットの下部中央に配置され、教育のヒントの本文が左寄せされた状態で表示されます。

```xaml
<Button x:Name="SaveButton" Content="Save">
    <Button.Resources>
        <muxc:TeachingTip x:Name="AutoSaveTip"
            Target="{x:Bind SaveButton}"
            Title="Saving automatically"
            Subtitle="We save your changes as you go - so you never have to."
            PreferredPlacement="BottomLeft">
        </muxc:TeachingTip>
    </Button.Resources>
</Button>
```

![左下隅の教育のヒントによってターゲットに指定された [保存] ボタンがあるサンプル アプリ。 ヒントのタイトルには "Saving automatically" (自動的に保存) と記載されており、サブタイトルには "We save your changes as you go - so you never have to." (変更内容は作業中に保存されるため、保存操作は不要です) と記載されています。 教育のヒントの右上隅に、閉じるボタンがあります。](../images/teaching-tip-targeted-preferred-placement.png)

PreferredPlacement が "BottomLeft" に設定されたターゲット非指定の教育のヒントが、XAML ルートの左下隅に表示されます。

```xaml
<Button x:Name="SaveButton" Content="Save" />

<muxc:TeachingTip x:Name="AutoSaveTip"
    Title="Saving automatically"
    Subtitle="We save your changes as you go - so you never have to."
    PreferredPlacement="BottomLeft">
</muxc:TeachingTip>
```

![教育のヒントが左下隅に表示されているサンプル アプリ。 ヒントのタイトルには "Saving automatically" (自動的に保存) と記載されており、サブタイトルには "We save your changes as you go - so you never have to." (変更内容は作業中に保存されるため、保存操作は不要です) と記載されています。 教育のヒントの右上隅に、閉じるボタンがあります。](../images/teaching-tip-non-targeted-preferred-placement.png)

次の図は、ターゲット指定の教育のヒントに対して設定できる 13 個の PreferredPlacement モードすべての結果を示しています。
![それぞれ異なるターゲット指定の配置モードを示す、13 個の教育のヒントを含む図。 個々の教育のヒントには、それぞれが表すモードを示すラベルが付いています。  配置モードの最初の単語は、教育のヒントが中央に表示されるターゲットの面を示しています。 教育のヒントのテールはターゲットのその面の中央に常に配置され、ターゲットの方向を向きます。 配置モードに 2 番目の単語がある場合、教育のヒントの本文は中央揃えされず、指定された方向にシフトされます。 たとえば、配置モードが "TopRight" の場合、教育のヒントはターゲットの上方で右にシフトされて表示され、テールはターゲットの上端の中央で下向きに表示されます。 本文が右にシフトされているため、テールは教育のヒントの本文のほぼ左端にあり、教育のヒントはターゲットの右端を越えて延びています。 配置モード "Center" は独特で、教育のヒントのテールがターゲットの中央を指し、教育のヒントはターゲットの上半分で中央揃えされます。](../images/teaching-tip-targeted-preferred-placement-modes.png)

次の図は、ターゲット非指定の教育のヒントに対して設定できる 13 個の PreferredPlacement モードすべての結果を示しています。
![それぞれ異なるターゲット非指定の配置モードを示す、9 個の教育のヒントを含む図。 個々の教育のヒントには、それぞれが表すモードを示すラベルが付いています。  配置モードの最初の単語は、教育のヒントが中央に表示される XAML ルートの面を示しています。  配置モードに 2 番目の単語がある場合、教育のヒントは XAML ルートのその指定された隅の方向に配置されます。 たとえば、配置モードが "TopRight" の場合、教育のヒントは XAML ルートの右上隅に表示されます。 ターゲット非指定の配置モードでは、2 つの単語の順序は配置に影響しません。 TopRight は RightTop と同じです。  配置モード "Center" は独特で、教育のヒントは XAML ルートの垂直方向および水平方向の中央に表示されます。](../images/teaching-tip-non-targeted-preferred-placement-modes.png)

### <a name="add-a-placement-margin"></a>配置の余白を追加する

ターゲット指定された教育のヒントをターゲットからどの程度離すか、およびターゲット非指定の教育のヒントを XAML ルートの端からどの程度離すかを、[PlacementMargin](/uwp/api/microsoft.ui.xaml.controls.teachingtip.placementmargin) プロパティを使用して制御できます。 [Margin](/uwp/api/windows.ui.xaml.frameworkelement.margin) と同じように、PlacementMargin には left、right、top、および bottom の 4 つの値があり、関連する値のみが使用されます。 たとえば、ヒントがターゲットの左側にあるか、XAML ルートの左端にある場合は PlacementMargin.Left が適用されます。

次の例では、PlacementMargin の Left/Top/Right/Bottom がすべて 80 に設定された場合のターゲット非指定のヒントを示しています。

```xaml
<Button x:Name="SaveButton" Content="Save" />

<muxc:TeachingTip x:Name="AutoSaveTip"
    Title="Saving automatically"
    Subtitle="We save your changes as you go - so you never have to."
    PreferredPlacement="BottomLeft"
    PlacementMargin="80">
</muxc:TeachingTip>
```

![右下隅の方向 (完全に右下隅ではない) に配置されている教育のヒントを表示するサンプル アプリ。 ヒントのタイトルには "Saving automatically" (自動的に保存) と記載されており、サブタイトルには "We save your changes as you go - so you never have to." (変更内容は作業中に保存されるため、保存操作は不要です) と記載されています。 教育のヒントの右上隅に、閉じるボタンがあります。](../images/teaching-tip-placement-margin.png)


### <a name="add-content"></a>コンテンツを追加する

[Content](/uwp/api/windows.ui.xaml.controls.contentcontrol.content) プロパティを使用して、教育のヒントにコンテンツを追加できます。 教育のヒントのサイズで収納できるコンテンツよりも多くのコンテンツを表示する場合、ユーザーがコンテンツ領域をスクロールできるスクロール バーが自動的に有効になります。

```xaml
<Button x:Name="SaveButton" Content="Save">
    <Button.Resources>
        <muxc:TeachingTip x:Name="AutoSaveTip"
            Target="{x:Bind SaveButton}"
            Title="Saving automatically"
            Subtitle="We save your changes as you go - so you never have to.">
                <StackPanel>
                    <CheckBox x:Name="HideTipsCheckBox" Content="Don't show tips at start up" IsChecked="{x:Bind HidingTips, Mode=TwoWay}" />
                    <TextBlock>You can change your tip preferences in <Hyperlink NavigateUri="app:/item/SettingsPage">Settings</Hyperlink> if you change your mind.</TextBlock>
                </StackPanel>
        </muxc:TeachingTip>
    </Button.Resources>
</Button>
```

![保存ボタンをターゲットに指定した教育のヒントが表示されているサンプル アプリ。 ヒントのタイトルには "Saving automatically" (自動的に保存) と記載されており、サブタイトルには "We save your changes as you go - so you never have to." (変更内容は作業中に保存されるため、保存操作は不要です) と記載されています。 教育のヒントのコンテンツ領域には、"Don't show tips at startup" (起動時にヒントを表示しない) というラベルの付いたチェック ボックスが表示され、その下には "You can change your tip preferences in Settings if you change your mind" (ヒントの設定は [設定] からいつでも変更できます) というテキストが記載され、[設定] はアプリの設定ページへのリンクになっています。 教育のヒントの右上隅に、閉じるボタンがあります。](../images/teaching-tip-content.png)

### <a name="add-buttons"></a>ボタンを追加する

既定では、標準の閉じるボタン "X" が、教育のヒントのタイトルの横に表示されます。 閉じるボタンは [CloseButtonContent](/uwp/api/microsoft.ui.xaml.controls.teachingtip.closebuttoncontent) プロパティを使用してカスタマイズできます。その場合、ボタンは教育のヒントの下部に移動します。

**注: 簡易非表示が有効なヒントの場合、閉じるボタンは表示されません**

[ActionButtonContent](/uwp/api/microsoft.ui.xaml.controls.teachingtip.actionbuttoncontent) プロパティ (および必要に応じて、[ActionButtonCommand](/uwp/api/microsoft.ui.xaml.controls.teachingtip.actionbuttoncommand) および [ActionButtonCommandParameter](/uwp/api/microsoft.ui.xaml.controls.teachingtip.actionbuttoncommandparameter) プロパティ) を設定して、カスタム アクション ボタンを追加できます。

```xaml
<Button x:Name="SaveButton" Content="Save">
    <Button.Resources>
        <muxc:TeachingTip x:Name="AutoSaveTip"
            Target="{x:Bind SaveButton}"
            Title="Saving automatically"
            Subtitle="We save your changes as you go - so you never have to."
            ActionButtonContent="Disable"
            ActionButtonCommand="{x:Bind DisableAutoSaveCommand}"
            CloseButtonContent="Got it!">
                <StackPanel>
                    <CheckBox x:Name="HideTipsCheckBox" Content="Don't show tips at start up" IsChecked="{x:Bind HidingTips, Mode=TwoWay}" />
                    <TextBlock>You can change your tip preferences in <Hyperlink NavigateUri="app:/item/SettingsPage">Settings</Hyperlink> if you change your mind.</TextBlock>
                </StackPanel>
        </muxc:TeachingTip>
    </Button.Resources>
</Button>
```

![保存ボタンをターゲットに指定した教育のヒントが表示されているサンプル アプリ。 ヒントのタイトルには "Saving automatically" (自動的に保存) と記載されており、サブタイトルには "We save your changes as you go - so you never have to." (変更内容は作業中に保存されるため、保存操作は不要です) と記載されています。 教育のヒントのコンテンツ領域には、"Don't show tips at startup" (起動時にヒントを表示しない) というラベルの付いたチェック ボックスが表示され、その下には "You can change your tip preferences in Settings if you change your mind" (ヒントの設定は [設定] からいつでも変更できます) というテキストが記載され、[設定] はアプリの設定ページへのリンクになっています。 教育のヒントの下部には 2 つのボタンがあり、左側のグレーの方には "Disable" (無効にする)、右側の青い方には "Got it!" (了解) と記載されています。](../images/teaching-tip-buttons.png)

### <a name="hero-content"></a>ヒーロー コンテンツ

[HeroContent](/uwp/api/microsoft.ui.xaml.controls.teachingtip.herocontent) プロパティを設定することによって、端から端までのコンテンツを教育のヒントに追加できます。 ヒーロー コンテンツの場所は、[HeroContentPlacement](/uwp/api/microsoft.ui.xaml.controls.teachingtip.herocontentplacement) プロパティを設定して教育のヒントの上または下に設定できます。

```xaml
<Button x:Name="SaveButton" Content="Save">
    <Button.Resources>
        <muxc:TeachingTip x:Name="AutoSaveTip"
            Target="{x:Bind SaveButton}"
            Title="Saving automatically"
            Subtitle="We save your changes as you go - so you never have to.">
            <muxc:TeachingTip.HeroContent>
                <Image Source="Assets/cloud.png" />
            </muxc:TeachingTip.HeroContent>
        </muxc:TeachingTip>
    </Button.Resources>
</Button>
```

![保存ボタンをターゲットに指定した教育のヒントが表示されているサンプル アプリ。 ヒントのタイトルには "Saving automatically" (自動的に保存) と記載されており、サブタイトルには "We save your changes as you go - so you never have to." (変更内容は作業中に保存されるため、保存操作は不要です) と記載されています。 教育のヒントの下部には、ファイルをクラウドに入れる男性のイラストを描いた画像が枠線いっぱいに表示されています。 教育のヒントの右上隅に、閉じるボタンがあります。](../images/teaching-tip-hero-content.png)

### <a name="add-an-icon"></a>アイコンを追加する

[IconSource](/uwp/api/microsoft.ui.xaml.controls.teachingtip.iconsource) プロパティを使用して、タイトルおよびサブタイトルの横にアイコンを追加できます。 推奨されるアイコンのサイズは 16px、24px、および 32px です。

```xaml
<Button x:Name="SaveButton" Content="Save">
    <Button.Resources>
        <muxc:TeachingTip x:Name="AutoSaveTip"
            Target="{x:Bind SaveButton}"
            Title="Saving automatically"
            Subtitle="We save your changes as you go - so you never have to."
            <muxc:TeachingTip.IconSource>
                <muxc:SymbolIconSource Symbol="Save" />
            </muxc:TeachingTip.IconSource>
        </muxc:TeachingTip>
    </Button.Resources>
</Button>
```

![保存ボタンをターゲットに指定した教育のヒントが表示されているサンプル アプリ。 ヒントのタイトルには "Saving automatically" (自動的に保存) と記載されており、サブタイトルには "We save your changes as you go - so you never have to." (変更内容は作業中に保存されるため、保存操作は不要です) と記載されています。 タイトルとサブタイトルの左側にフロッピー ディスクのアイコンがあります。 教育のヒントの右上隅に、閉じるボタンがあります。](../images/teaching-tip-icon.png)

### <a name="enable-light-dismiss"></a>簡易非表示を有効にする

簡易非表示機能は既定では無効になっていますが、[IsLightDismissEnabled](/uwp/api/microsoft.ui.xaml.controls.teachingtip.islightdismissenabled) プロパティを設定してこの機能を有効にすることで、たとえばユーザーがスクロールしたり、アプリケーションの他の要素とやり取りしたりしたときに、教育のヒントが閉じるようにすることができます。 このような動作であるため、スクロール可能な領域にヒントを配置する必要がある場合は、簡易非表示のヒントが最適なソリューションとなります。

簡易非表示が有効な教育のヒントからは、閉じるボタンが自動的に削除され、簡易非表示の動作であることがユーザーに示されます。

```xaml
<Button x:Name="SaveButton" Content="Save" />

<muxc:TeachingTip x:Name="AutoSaveTip"
    Title="Saving automatically"
    Subtitle="We save your changes as you go - so you never have to."
    IsLightDismissEnabled="True">
</muxc:TeachingTip>
```

![簡易非表示の教育のヒントが右下隅に表示されているサンプル アプリ。 ヒントのタイトルには "Saving automatically" (自動的に保存) と記載されており、サブタイトルには "We save your changes as you go - so you never have to." (変更内容は作業中に保存されるため、保存操作は不要です) と記載されています。](../images/teaching-tip-light-dismiss.png)

### <a name="escaping-the-xaml-root-bounds"></a>XAML ルートの境界をエスケープする

Windows 10 バージョン 1903 (ビルド 18362) 以降では、教育のヒントで [ShouldConstrainToRootBounds](/uwp/api/microsoft.ui.xaml.controls.teachingtip.shouldconstraintorootbounds) プロパティを設定することにより、XAML ルートと画面の境界をエスケープすることができます。 このプロパティを有効にすると、教育のヒントは XAML ルートの境界の内側または画面に留まろうとせず、設定された `PreferredPlacement` モードの位置に常に配置されます。 最適なユーザー エクスペリエンスを確保するには、`IsLightDismissEnabled` プロパティを有効にして、XAML ルートの中央に最も近い `PreferredPlacement` モードを設定することをお勧めします。

以前のバージョンの Windows では、このプロパティは無視され、教育のヒントは常に XAML ルートの境界内に配置されます。

```xaml
<Button x:Name="SaveButton" Content="Save" />

<muxc:TeachingTip x:Name="AutoSaveTip"
    Title="Saving automatically"
    Subtitle="We save your changes as you go - so you never have to."
    PreferredPlacement="BottomRight"
    PlacementMargin="-80,-50,0,0"
    ShouldConstrainToRootBounds="False">
</muxc:TeachingTip>
```

![教育のヒントがアプリの右下隅の外に出ているサンプル アプリ。 ヒントのタイトルには "Saving automatically" (自動的に保存) と記載されており、サブタイトルには "We save your changes as you go - so you never have to." (変更内容は作業中に保存されるため、保存操作は不要です) と記載されています。 教育のヒントの右上隅に、閉じるボタンがあります。](../images/teaching-tip-escape-xaml-root.png)

### <a name="canceling-and-deferring-close"></a>クローズのキャンセルおよび延期

[Closing](/uwp/api/microsoft.ui.xaml.controls.teachingtip.closing) イベントを使用して、教育のヒントのクローズをキャンセルしたり延期したりすることができます。 これは教育のヒントを開いたままにしたり、アクションの実行またはカスタム アニメーションの表示の時間を確保するために使用できます。 教育のヒントのクローズをキャンセルすると、IsOpen は true に戻りますが、延期の間は false のままになります。 プログラムによるクローズもキャンセルできます。

> [!NOTE]
> 教育のヒントの完全な表示を許可する配置オプションが存在しない場合、教育のヒントは、使用可能な閉じるボタンのない状態で表示されるのではなく、閉じることを強制するためにイベントのライフ サイクルを通じて繰り返されます。 アプリが Closing イベントをキャンセルした場合、教育のヒントは使用可能な閉じるボタンがない状態で開いたままになることがあります。

```xaml
<muxc:TeachingTip x:Name="EnableNewSettingsTip"
    Title="New ways to protect your privacy!"
    Subtitle="Please close this tip and review our updated privacy policy and privacy settings."
    Closing="OnTipClosing">
</muxc:TeachingTip>
```

```csharp
private void OnTipClosing(muxc.TeachingTip sender, muxc.TeachingTipClosingEventArgs args)
{
    if (args.Reason == muxc.TeachingTipCloseReason.CloseButton)
    {
        using(args.GetDeferral())
        {
            bool success = UpdateUserSettings(User thisUsersID);
            if(!success)
            {
                // We were not able to update the settings!
                // Don't close the tip and display the reason why.
                args.Cancel = true;
                ShowLastErrorMessage();
            }
        }
    }
}
```

## <a name="recommendations"></a>推奨事項

* ヒントは一時的なものであるため、アプリケーションのエクスペリエンスにとって重要な情報やオプションを含めるべきではありません。
* 教育のヒントを過剰な頻度で表示しないようにしてください。 教育のヒントは、長いセッションまたは複数のセッションで時間をずらして表示された場合に、個人の注意を惹く可能性が最も高くなります。
* ヒントは簡潔にし、トピックを明確にしてください。 調査によると、ユーザーはヒントを活用するかどうかを決める前に、平均して 3 つから 5 つの単語しか読まず、2 つから 3 つの単語しか理解していません。
* 教育のヒントのゲームパッド アクセシビリティは保証されません。 ゲームパッド入力を予測するアプリケーションの場合は、「[ゲームパッドとリモコンの操作]( ../../input/gamepad-and-remote-interactions.md#xy-focus-navigation-and-interaction)」を参照してください。 アプリの UI で考えられるすべての構成を使用して、それぞれの教育のヒントについてのゲームパッドのアクセシビリティをテストすることをお勧めします。
* 教育のヒントが XAML ルートをエスケープすることを有効にするときは、IsLightDismissEnabled プロパティも有効にして、XAML ルートの中央に最も近い PreferredPlacement モードを設定することをお勧めします。

## <a name="reconfiguring-an-open-teaching-tip"></a>開いている教育のヒントを再構成する

一部のコンテンツおよびプロパティは、教育のヒントが開いているときに再構成でき、すぐに有効になります。 アイコン プロパティ、アクション ボタン、閉じるボタン、および簡易非表示と明示的非表示との間での再構成など、その他のコンテンツおよびプロパティの場合はいずれも、これらのプロパティの変更を有効にするには、教育のヒントを閉じて再び開く必要があります。 教育のヒントが開いているときに手動非表示から簡易非表示に非表示動作を変更すると、簡易非表示の動作が有効になる前に、教育のヒントから閉じるボタンが削除され、ヒントが画面に表示されたままになることがあるため注意してください。

## <a name="related-articles"></a>関連記事

* [ダイアログとポップアップ](./index.md)
