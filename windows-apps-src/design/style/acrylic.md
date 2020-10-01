---
description: 奥行きを加えたり視覚的な階層を確立したりするための半透明のテクスチャを作成するブラシの一種である、アクリルの使用方法について説明します。
title: アクリル素材
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
pm-contact: yulikl
design-contact: rybick
dev-contact: jevansa
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: b6fb7d07901a1ad4ab99255357470c8b90fc7681
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/24/2020
ms.locfileid: "91220105"
---
# <a name="acrylic-material"></a>アクリル素材

![ヒーロー イメージ](images/header-acrylic.svg)

アクリルは、半透明のテクスチャを作成する[ブラシ](/uwp/api/Windows.UI.Xaml.Media.Brush)の一種です。 アクリルをアプリ サーフェスに適用すると、奥行きを加えたり、視覚的な階層を確立したりすることができます。  <!-- By allowing user-selected wallpaper or colors to shine through, acrylic keeps users in touch with the OS personalization they've chosen. -->

> **重要な API**:[AcrylicBrush クラス](/uwp/api/windows.ui.xaml.media.acrylicbrush)、[Background プロパティ](/uwp/api/windows.ui.xaml.controls.control.Background)

:::row:::
    :::column:::
淡色テーマのアクリル ![淡色テーマのアクリル](images/Acrylic_LightTheme_Base.png)
    :::column-end:::
    :::column:::
濃色テーマのアクリル ![濃色テーマのアクリル](images/Acrylic_DarkTheme_Base.png)
    :::column-end:::
:::row-end:::

## <a name="acrylic-and-the-fluent-design-system"></a>アクリルと Fluent Design System

 Fluent Design System では、ライト、深度、モーション、マテリアル、スケールを取り入れた、モダンで目を引く UI を作成できます。 アクリルは、アプリに物理的なテクスチャ (マテリアル) と深度を追加する Fluent Design System コンポーネントです。 詳しくは、[Fluent Design の概要](/windows/apps/fluent-design-system)に関するページをご覧ください。

 ## <a name="video-summary"></a>ビデオの概要

> [!VIDEO https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Fall-Creators-Update/WinDev002/player]

## <a name="examples"></a>例

:::row:::
    :::column span:::
![画像](images/XAML-controls-gallery-app-icon.png)
    :::column-end:::
    :::column span="2":::
**XAML コントロール ギャラリー**<br>
XAML コントロール ギャラリー アプリがインストールされている場合、<a href="xamlcontrolsgallery:/item/Acrylic">こちら</a>をクリックしてアプリを開き、アクリルの動作を確認してください。

<a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML コントロール ギャラリー アプリを入手する (Microsoft Store)</a><br>
<a href="https://github.com/Microsoft/Xaml-Controls-Gallery">ソース コード (GitHub) を入手する</a>
    :::column-end:::
:::row-end:::

## <a name="acrylic-blend-types"></a>アクリル ブレンドの種類
アクリルの最も注目すべき特徴は、その透明度です。 アクリル ブレンドの種類は 2 つあり、素材を通して何が表示されるかを変更することができます。
 - **背景アクリル**では、デスクトップの壁紙と現在アクティブになっているアプリの背後にある他のウィンドウが表示されます。これにより、アプリケーション ウィンドウ間に奥行きが出て、ユーザーが個人的に優先する画面を明示しておくことができます。
 - **アプリ内アクリル**では、アプリ フレーム内に奥行きの感覚がもたらされ、フォーカスと階層の両方が提供されます。

 ![背景アクリル](images/BackgroundAcrylic_DarkTheme.png)

 ![アプリ内アクリル](images/AppAcrylic_DarkTheme.png)

 複数のアクリル サーフェスを重ねる場合は注意が必要です。背景アクリルを何層にも重ねると、目の錯覚の原因となることがあります。

## <a name="when-to-use-acrylic"></a>アクリルの用途

* アプリ内アクリルは、スクロール時や操作時にコンテンツと重なる可能性のあるサーフェス上などで、UI をサポートするために使用します。
* コンテキスト メニュー、ポップアップ、簡易非表示 UI など、一時的な UI 要素に対して背景アクリルを使用します。<br />一時的なシナリオでアクリルを使用すると、一時的な UI をトリガーしたコンテンツとの視覚的な関係を維持するのに役立ちます。

ナビゲーション サーフェスでアプリ内アクリルを使用する場合、アプリでのフローを向上させるために、アクリル ウィンドウの下でコンテンツを拡張することを検討してください。 NavigationView を使用すると、これが自動的に行われます。 ただし、ストライプ効果が生成されないようにするには、複数のアクリルを端と端を接して配置しないでください。これを行うと、2 つのぼやけたサーフェス間に不要な継ぎ目が作成される場合があります。 アクリルは、デザインで視覚的な調和をとるためのツールですが、正しく使用しないと、視覚的なノイズになる場合があります。

次の使用パターンを検討して、アクリルをアプリに組み込むのに最適な方法を決定してください。

### <a name="vertical-panes"></a>垂直方向のウィンドウ

アプリのコンテンツを分割するのに役立つ垂直方向のウィンドウまたはサーフェスの場合は、アクリルの代わりに不透明な背景を使用することをお勧めします。 NavigationView の **Compact** モードや **Minimal** モードのように、コンテンツの上部で垂直方向のウィンドウが開く場合、ユーザーがこのウィンドウを開いた状態にしているときにページのコンテンツを維持するために、アプリ内アクリルを使用することをお勧めします。

### <a name="transient-surfaces"></a>一時的なサーフェス

メニュー ポップアップ、非モーダル ポップアップ、または簡易非表示ウィンドウを使用するアプリの場合、背景アクリルを使用することをお勧めします。

![情報のポップアップを使用したメール アプリのパターン](images/Mail_TransientContextMenu.png)

コントロールの多くは、既定でアクリルを使用します。 [MenuFlyouts](../controls-and-patterns/menus.md)、[AutoSuggestBox](../controls-and-patterns/auto-suggest-box.md)、[ComboBox](/uwp/api/windows.ui.xaml.controls.combobox)、および簡易非表示ポップアップを使用する類似のコントロールはすべて、呼び出されたときに一時的なアクリルを使用します。

> [!Note]
> アクリル サーフェスのレンダリングでは GPU を集中的に使用するため、デバイスの電力消費が増加し、バッテリー残量が少なくなる可能性があります。 デバイスがバッテリー節約機能モードになると、アクリルの効果は自動的に無効になります。またユーザーは、必要に応じて、すべてのアプリでアクリルの効果を無効にすることができます。

## <a name="usability-and-adaptability"></a>使いやすさと適応性
アクリルの外観は、さまざまなデバイスやコンテキストに合うように自動的に対応します。

ハイ コントラスト モードでは、ユーザーが選んだ見慣れた背景色が、アクリルの代わりに引き続き表示されます。 また次の場合には、背景アクリルとアプリ内アクリルはどちらも、単色として表示されます。
 - ユーザーが [設定] > [個人用設定] > [色] で透明度をオフにした場合
 - バッテリー節約機能モードがアクティブ化されている場合
 - アプリがローエンド ハードウェアで実行される場合

また次の場合には、背景アクリルのみ、その透明度とテクスチャを単色で置き換えることができます。
 - アプリ ウィンドウがデスクトップで非アクティブ化されている場合
 - Windows アプリが、電話、Xbox、HoloLens、またはタブレット モードで実行されている場合

### <a name="legibility-considerations"></a>見やすくするための考慮事項
アプリによりユーザーに表示されるすべてのテキストでは、[コントラスト比を適切な値にする](../accessibility/accessible-text-requirements.md)ことが重要です。 ハイカラーの黒や白のテキスト、またはミディアムカラーの灰色のテキストがアクリル上で適切なコントラスト比になるように、アクリルのレシピを最適化しています。 プラットフォームによって提供されるテーマ リソースは、既定で、濃淡の色のコントラストが 80% の不透明度に設定されます。 ハイカラーの本文テキストをアクリル上に配置する場合、濃淡の不透明度を下げて、見やすさを維持することができます。 ダーク モードでは濃淡の不透明度を 70% にすることができ、ライト モードのアクリルでは 50% の不透明度でコントラスト比が満たされます。

アクリル サーフェスの上にアクセントカラーのテキストを配置することはお勧めしません。これは、これらの組み合わせが、15 ピクセルのフォント サイズでのコントラスト比の最小要件を満たさない可能性があるためです。 [ハイパーリンク](../controls-and-patterns/hyperlinks.md)はアクリル要素上には配置しないようにしてください。 また、アクリルの濃淡の色や不透明度を、テーマ リソースによって提供されるプラットフォームの既定値以外にカスタマイズする場合は、見やすさへの影響を考慮してください。

## <a name="acrylic-theme-resources"></a>アクリルのテーマ リソース
新しい XAML AcrylicBrush テーマ リソースや定義済みの AcrylicBrush テーマ リソースを使用して、アクリルをアプリのサーフェスに簡単に適用できます。 まず、アプリ内アクリルまたは背景アクリルのどちらを使用するかを決める必要があります。 この記事で推奨事項として既に説明した一般的なアプリのパターンを確認してください。

背景アクリルやアプリ内アクリルの両方を対象としたブラシのテーマ リソースのコレクションが作成されています。これらのリソースでは、アプリのテーマが重視され、必要に応じて、単色にフォール バックします。 *AcrylicWindow* という名前のリソースは背景アクリルを表し、*AcrylicElement* はアプリ内アクリルを表します。

<table>
    <tr>
        <th align="center">リソース キー</th>
        <th align="center">濃淡の不透明度</th>
        <th align="center"><a href="color.md">フォールバックの色</a> </th>
    </tr>
    <tr>
        <td> SystemControlAcrylicWindowBrush、SystemControlAcrylicElementBrush <br/> SystemControlChromeLowAcrylicWindowBrush、SystemControlChromeLowAcrylicElementBrush <br/> SystemControlBaseHighAcrylicWindowBrush、SystemControlBaseHighAcrylicElementBrush <br/> SystemControlBaseLowAcrylicWindowBrush、SystemControlBaseLowAcrylicElementBrush <br/> SystemControlAltHighAcrylicWindowBrush、SystemControlAltHighAcrylicElementBrush <br/> SystemControlAltLowAcrylicWindowBrush、SystemControlAltLowAcrylicElementBrush </td>
        <td align="center"> 80% </td>
        <td> ChromeMedium <br/> ChromeLow <br/><br/> BaseHigh <br/><br/> BaseLow <br/><br/> AltHigh <br/><br/> AltLow </td>
    </tr>
    </tr>
        <td> <b>推奨される使用法:</b>これらは、汎用的なアクリルのリソースであり、さまざまな使用方法で適切に機能します。 アプリで使用するセカンダリ テキストの色が AltMedium で、そのサイズが 18 ピクセルよりも小さい場合は、<a href="../accessibility/accessible-text-requirements.md">コントラスト比の要件を満たすように</a>、80% のアクリル リソースをテキストの背景に配置してください。 </td>
    </tr>
    <tr>
        <td> SystemControlAcrylicWindowMediumHighBrush、SystemControlAcrylicElementMediumHighBrush <br/> SystemControlBaseHighAcrylicWindowMediumHighBrush、SystemControlBaseHighAcrylicElementMediumHighBrush </td>
        <td align="center"> 70% </td>
        <td> ChromeMedium <br/><br/> BaseHigh </td>
    </tr>
    <tr>
        <td> <b>推奨される使用法:</b>アプリで使用するセカンダリ テキストの色が AltMedium で、そのサイズが 18 ピクセル以上になる場合は、これらのより透過的な 70% のアクリル リソースをテキストの背景に配置できます。 これらのリソースは、アプリの最上部にある水平方向のナビゲーション領域やコマンド実行領域で使用することをお勧めします。  </td>
    </tr>
    <tr>
        <td> SystemControlChromeHighAcrylicWindowMediumBrush、SystemControlChromeHighAcrylicElementMediumBrush <br/> SystemControlChromeMediumAcrylicWindowMediumBrush、SystemControlChromeMediumAcrylicElementMediumBrush <br/> SystemControlChromeMediumLowAcrylicWindowMediumBrush、SystemControlChromeMediumLowAcrylicElementMediumBrush <br/> SystemControlBaseHighAcrylicWindowMediumBrush、SystemControlBaseHighAcrylicElementMediumBrush <br/> SystemControlBaseMediumLowAcrylicWindowMediumBrush、SystemControlBaseMediumLowAcrylicElementMediumBrush <br/> SystemControlAltMediumLowAcrylicWindowMediumBrush、SystemControlAltMediumLowAcrylicElementMediumBrush  </td>
        <td align="center"> 60% </td>
        <td> ChromeHigh <br/><br/> ChromeMedium <br/><br/> ChromeMediumLow <br/><br/> BaseHigh <br/><br/> BaseLow <br/><br/> AltMediumLow </td>
    </tr>
    <tr>
        <td> <b>推奨される使用法:</b>プライマリ テキストの色が AltHigh で、このテキストのみをアクリルの上に配置する場合は、これらの 60% のリソースをアプリで利用できます。 アプリの<a href="../controls-and-patterns/navigationview.md">垂直方向のナビゲーション ペイン</a> (ハンバーガー メニュー) を描画するときは、60% のアクリルを使用することをお勧めします。 </td>
    </tr>
</table>

中間色のアクリルに加え、ユーザーが指定したアクセント カラーを使用してアクリルに濃淡を付けるリソースも追加しました。 色が付いたアクリルは慎重に使用してください。 提供されている dark1 と dark2 のバリアントを使用する場合、濃色テーマのテキストの色と調和するように、白または明るい色のテキストをこれらのリソース上に配置してください。
<table>
    <tr>
        <th align="center">リソース キー</th>
        <th align="center">濃淡の不透明度</th>
        <th align="center"><a href="color.md">濃淡とフォールバックの色</a> </th>
    </tr>
    <tr>
        <td> SystemControlAccentAcrylicWindowAccentMediumHighBrush、SystemControlAccentAcrylicElementAccentMediumHighBrush  </td>
        <td align="center"> 70% </td>
        <td> SystemAccentColor </td>
    </tr>
    <tr>
        <td> SystemControlAccentDark1AcrylicWindowAccentDark1Brush、SystemControlAccentDark1AcrylicElementAccentDark1Brush  </td>
        <td align="center"> 80% </td>
        <td> SystemAccentColorDark1 </td>
    </tr>
    <tr>
        <td> SystemControlAccentDark2AcrylicWindowAccentDark2MediumHighBrush、SystemControlAccentDark2AcrylicElementAccentDark2MediumHighBrush  </td>
        <td align="center"> 70% </td>
        <td> SystemAccentColorDark2 </td>
    </tr>
</table>


特定のサーフェスを塗りつぶすには、上記のテーマ リソースのいずれかを、他のブラシ リソースを適用する要素の背景に適用します。

```xaml
<Grid Background="{ThemeResource SystemControlAcrylicElementBrush}">
```

## <a name="custom-acrylic-brush"></a>カスタム アクリル ブラシ
色の濃淡をアプリのアクリルに加えて、ブランドを表示したり、ページ上にある他の要素と視覚的にバランスをとったりすることができます。 グレースケール以外の色を表示するには、次のプロパティを使って、独自のアクリル ブラシを定義する必要があります。
 - **TintColor**: 色/濃淡のオーバーレイ レイヤーです。 RGB の色の値とアルファ チャネルの不透明度の両方を指定することを検討してください。
 - **TintOpacity**: 濃淡レイヤーの不透明度です。 開始点として 80% の不透明度をお勧めします。ただし、透明度が異なると、別の色を使用したほうがより魅力的に表示される可能性もあります。
 - **TintLuminosityOpacity**: 背景からアクリル サーフェスの間で許容される彩度の量を制御します。
 - **BackgroundSource**: 背景アクリルまたはアプリ内アクリルのどちらを使用するかを指定するフラグです。
 - **FallbackColor**: バッテリー節約機能でアクリルと置き換わる単色です。 背景アクリルでは、フォールバックの色は、アプリが作業中のデスクトップ ウィンドウにない場合、またはアプリが電話や Xbox 上で実行されている場合にもアクリルと置き換わります。

![淡色テーマのアクリルの見本](images/CustomAcrylic_Swatches_LightTheme.png)

![濃色テーマのアクリルの見本](images/CustomAcrylic_Swatches_DarkTheme.png)

![明暗の不透明度と濃淡の不透明度の比較](images/LuminosityVersusTint.png)

アクリル ブラシを追加するには、濃色テーマ、淡色テーマ、ハイ コントラスト テーマの 3 つのリソースを定義します。 ハイ コントラストでは、濃色/淡色の AcrylicBrush と同じ x:Key で SolidColorBrush を使用することをお勧めします。

> [!Note]
> TintLuminosityOpacity 値を指定しない場合、その値は、TintColor および TintOpacity に基づいてシステムによって自動的に調整されます。

```xaml
<ResourceDictionary.ThemeDictionaries>
    <ResourceDictionary x:Key="Default">
        <AcrylicBrush x:Key="MyAcrylicBrush"
            BackgroundSource="HostBackdrop"
            TintColor="#FFFF0000"
            TintOpacity="0.8"
            TintLuminosityOpacity="0.5"
            FallbackColor="#FF7F0000"/>
    </ResourceDictionary>

    <ResourceDictionary x:Key="HighContrast">
        <SolidColorBrush x:Key="MyAcrylicBrush"
            Color="{ThemeResource SystemColorWindowColor}"/>
    </ResourceDictionary>

    <ResourceDictionary x:Key="Light">
        <AcrylicBrush x:Key="MyAcrylicBrush"
            BackgroundSource="HostBackdrop"
            TintColor="#FFFF0000"
            TintOpacity="0.8"
            TintLuminosityOpacity="0.5"
            FallbackColor="#FFFF7F7F"/>
    </ResourceDictionary>
</ResourceDictionary.ThemeDictionaries>
```

次のサンプルは、コードで AcrylicBrush を宣言する方法を示しています。 アプリで複数の OS ターゲットがサポートされている場合は、この API がユーザーのコンピューターで利用できることを確認してください。

```csharp
if (Windows.Foundation.Metadata.ApiInformation.IsTypePresent("Windows.UI.Xaml.Media.AcrylicBrush"))
{
    Windows.UI.Xaml.Media.AcrylicBrush myBrush = new Windows.UI.Xaml.Media.AcrylicBrush();
    myBrush.BackgroundSource = Windows.UI.Xaml.Media.AcrylicBackgroundSource.HostBackdrop;
    myBrush.TintColor = Color.FromArgb(255, 202, 24, 37);
    myBrush.FallbackColor = Color.FromArgb(255, 202, 24, 37);
    myBrush.TintOpacity = 0.6;

    grid.Fill = myBrush;
}
else
{
    SolidColorBrush myBrush = new SolidColorBrush(Color.FromArgb(255, 202, 24, 37));

    grid.Fill = myBrush;
}
```

## <a name="extend-acrylic-into-the-title-bar"></a>アクリルをタイトル バーに拡張する

アプリのウィンドウを滑らかな外観にするには、タイトル バー領域にアクリルを使います。 この例では、[ApplicationViewTitleBar](/uwp/api/Windows.UI.ViewManagement.ApplicationViewTitleBar) オブジェクトの [ButtonBackgroundColor](/uwp/api/Windows.UI.ViewManagement.ApplicationViewTitleBar.ButtonBackgroundColor) および [ButtonInactiveBackgroundColor](/uwp/api/Windows.UI.ViewManagement.ApplicationViewTitleBar.ButtonInactiveBackgroundColor) プロパティを [Colors.Transparent](/uwp/api/Windows.UI.Colors.Transparent) に設定することで、アクリルをタイトル バーに拡張します。

```csharp
private void ExtendAcrylicIntoTitleBar()
{
    CoreApplication.GetCurrentView().TitleBar.ExtendViewIntoTitleBar = true;
    ApplicationViewTitleBar titleBar = ApplicationView.GetForCurrentView().TitleBar;
    titleBar.ButtonBackgroundColor = Colors.Transparent;
    titleBar.ButtonInactiveBackgroundColor = Colors.Transparent;
}
```

このコードは、ここに示すようにアプリの [OnLaunched](/uwp/api/windows.ui.xaml.application#Windows_UI_Xaml_Application_OnLaunched_Windows_ApplicationModel_Activation_LaunchActivatedEventArgs_) メソッド (_App.xaml.cs_) 内の [Window.Activate](/uwp/api/windows.ui.xaml.window.Activate) の呼び出しの後か、アプリの最初のページに配置できます。

```csharp
// Call your extend acrylic code in the OnLaunched event, after
// calling Window.Current.Activate.
protected override void OnLaunched(LaunchActivatedEventArgs e)
{
    Frame rootFrame = Window.Current.Content as Frame;

    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active
    if (rootFrame == null)
    {
        // Create a Frame to act as the navigation context and navigate to the first page
        rootFrame = new Frame();

        rootFrame.NavigationFailed += OnNavigationFailed;

        if (e.PreviousExecutionState == ApplicationExecutionState.Terminated)
        {
            //TODO: Load state from previously suspended application
        }

        // Place the frame in the current Window
        Window.Current.Content = rootFrame;
    }

    if (e.PrelaunchActivated == false)
    {
        if (rootFrame.Content == null)
        {
            // When the navigation stack isn't restored navigate to the first page,
            // configuring the new page by passing required information as a navigation
            // parameter
            rootFrame.Navigate(typeof(MainPage), e.Arguments);
        }
        // Ensure the current window is active
        Window.Current.Activate();

        // Extend acrylic
        ExtendAcrylicIntoTitleBar();
    }
}
```

また、通常はタイトル バーに自動的に表示されるアプリのタイトルを、`CaptionTextBlockStyle` を使用した TextBlock で描画する必要もあります。 詳しくは、「[タイトル バーのカスタマイズ](../shell/title-bar.md)」をご覧ください。

## <a name="dos-and-donts"></a>推奨と非推奨
* アクリルは、ナビゲーション ペインなど、アプリのプライマリ サーフェス以外のサーフェスで背景素材として使用してください。
* シームレスなエクスペリエンスを実現するには、アプリの周囲とわずかにブレンドするようにして、アクリルをアプリの 1 つ以上の端にまで拡張してください。
* アプリの大きな背景サーフェスにデスクトップ アクリルを配置しないでください。これを行うと、一時的なサーフェスに対して主に使用されるアクリルのメンタル モデルが破損します。
* アプリ内アクリルと背景アクリルは、継ぎ目部分での視覚的なテンションを回避するために、隣接するようには配置しないでください。
* 複数のアクリル ペインを、同じ濃淡や不透明度で隣接するように配置しないでください。このようにすると、望ましくない継ぎ目が表示されます。
* アクリル サーフェスの上には、アクセントカラーのテキストを配置しないでください。

## <a name="how-we-designed-acrylic"></a>アクリルをどのように設計したか

アクリルの主要なコンポーネントを微調整して、ユニークな外観とプロパティを作成しました。 設計の開始時には透明度、ぼかし、ノイズを使い、平坦なサーフェスに視覚的な奥行きとディメンションを追加しました。 除外ブレンド モード レイヤーを追加して、アクリルの背景に配置される UI のコントラストと見やすさを確保しました。 最後に、ユーザーの個性を反映できるように、色の濃淡を追加しました。 次のレイヤーを組み合わせることで、新しくて使いやすい素材が生みだされます。

![アクリルのレシピ](images/AcrylicRecipe_Diagram.jpg)
<br/>アクリルのレシピ: 背景、ぼかし、除外ブレンド、色/濃淡のオーバーレイ、ノイズ


## <a name="get-the-sample-code"></a>サンプル コードを入手する

- [XAML コントロール ギャラリー サンプル](https://github.com/Microsoft/Xaml-Controls-Gallery) - インタラクティブな形で XAML コントロールのすべてを参照できます。

## <a name="related-articles"></a>関連記事

[**表示ハイライト**](reveal.md)
