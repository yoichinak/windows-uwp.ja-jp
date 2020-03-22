---
author: knicholasa
description: Z 深度、つまり相対的な深さ、および影は、ユーザーが自然かつ効率的にフォーカスを得るために、アプリに深さを組み込む方法の2つの方法です。
title: UWP アプリの Z 深度とシャドウ
template: detail.hbs
ms.date: 04/19/2019
ms.topic: article
ms.custom: 19H1
keywords: windows 10, uwp
pm-contact: chigy
ms.localizationpriority: medium
ms.openlocfilehash: 216974ba564a192f94473469f3a7a49191ef2192
ms.sourcegitcommit: af4050f69168c15b0afaaa8eea66a5ee38b88fed
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/21/2020
ms.locfileid: "80081385"
---
# <a name="z-depth-and-shadow"></a>Z 深度とシャドウ

![斜めに並べられた4つの灰色の四角形を、もう一方に重ねて表示する gif。 Gif はアニメーション化されており、影が表示されたり消えたりします。](images/elevation-shadow/shadow.gif)

UI で要素のビジュアル階層を作成すると、UI を簡単にスキャンし、注目すべき重要なことを伝えることができます。 UI の選択要素を取り込む操作である昇格は、多くの場合、ソフトウェアでこのような階層を実現するために使用されます。 この記事では、z depth と shadow を使用して UWP アプリで昇格を作成する方法について説明します。

Z 深度は、z 軸に沿った2つのサーフェイス間の距離を示すために3D アプリ作成者の間で使用される用語です。 これは、オブジェクトをビューアーに閉じる方法を示しています。 X/y 座標については、z 方向に同様の概念と考えることをお勧めします。

## <a name="why-use-z-depth"></a>Z 深度を使用する理由

物理的な世界では、お近くのオブジェクトに注目する傾向があります。 この空間性質をデジタル UI にも適用できます。 たとえば、要素をユーザーの近くに移動すると、ユーザーは要素にフォーカスを直感的ます。 Z 軸の近くに UI 要素を移動すると、オブジェクト間の視覚的な階層を確立し、ユーザーがアプリで自然に効率的にタスクを完了できるようにすることができます。

## <a name="what-is-shadow"></a>シャドウとは

影は、ユーザーが昇格を意識する1つの方法です。 管理者特権を持つオブジェクトの上にあるライトは、次の画面に影を作成します。 オブジェクトが大きいほど、シャドウが大きくなります。 UI の昇格されたオブジェクトには、影を付ける必要はありませんが、昇格の外観を作成するのに役立ちます。

UWP アプリでは、美しい方法ではなく、特別な目的で影を使用する必要があります。 使用するシャドウの数が多すぎると、ユーザーにフォーカスするシャドウの機能が低下したり、削除されたりします。

標準コントロールを使用すると、ThemeShadow の影が UI に自動的に組み込まれます。 ただし、ThemeShadow または DropShadow Api を使用して、UI に影を手動で含めることができます。 

## <a name="themeshadow"></a>ThemeShadow

[ThemeShadow](/uwp/api/windows.ui.xaml.media.themeshadow)型を任意の XAML 要素に適用して、x、y、z 座標に基づいて影を適切に描画できます。 ThemeShadow も、その他の環境仕様に合わせて自動的に調整されます。

- 照明、ユーザーテーマ、アプリ環境、およびシェルの変更に適応します。
- Z の深さに基づいて、要素に自動的に影を適用します。 
- では、移動時と昇格時に要素の同期が維持されます。
- では、アプリケーション間でシャドウの一貫性を維持します。

ThemeShadow が MenuFlyout アウトに実装された方法を次に示します。 MenuFlyout インには、メインサーフェイスが32ピクセルに昇格し、追加のカスケードメニューが開かれているメニューの上に 8 px を超える機能が組み込まれています。

![ThemeShadow のスクリーンショットは、入れ子になった3つのメニューを含む MenuFlyout アウトに適用されます。 1つ目のメニューは、[32px] と表示されます。前のメニューから開いた各メニューは、8ピクセルを超えて背景に影が付きます。](images/elevation-shadow/themeshadow-menuflyout.png)

### <a name="themeshadow-in-common-controls"></a>コモンコントロールの ThemeShadow

次の一般的なコントロールは、ThemeShadow を自動的に使用して、特に指定されていない限り、32px 深度から影をキャストします。

- [ショートカットメニュー](../controls-and-patterns/menus.md)、[コマンドバー](../controls-and-patterns/app-bars.md)、[コマンドバーポップアップ](../controls-and-patterns/command-bar-flyout.md)、[メニューバー](../controls-and-patterns/menus.md#create-a-menu-bar)
- ダイアログ[と flyouts](../controls-and-patterns/dialogs.md) (64px)
- [NavigationView](../controls-and-patterns/navigationview.md)
- [ComboBox](../controls-and-patterns/combo-box.md)、 [Dropdownbutton、SplitButton、ToggleSplitButton](../controls-and-patterns/buttons.md)
- [TeachingTip](../controls-and-patterns/dialogs-and-flyouts/teaching-tip.md)
- [AutoSuggestBox](../controls-and-patterns/auto-suggest-box.md) 
- [カレンダー/日付/時刻の指定](../controls-and-patterns/date-and-time.md)
- [ツールヒント](../controls-and-patterns/tooltips.md)(16px)
- [Media transport control](../controls-and-patterns/media-playback.md#media-transport-controls)、 [inktoolbar](../controls-and-patterns/inking-controls.md)
- [接続型アニメーション](../motion/connected-animation.md)

注: Flyouts は、Windows 10 バージョン1903またはそれより新しい SDK に対してコンパイルされた場合にのみ ThemeShadow を適用します。

### <a name="themeshadow-in-popups"></a>ThemeShadow のポップアップ

多くの場合、ユーザーの注意とクイックアクションが必要なシナリオでは、アプリの UI でポップアップが使用されます。 これらは、アプリの UI で階層を作成するために shadow を使用する場合の優れた例です。

ThemeShadow は、[ポップアップ](/uwp/api/windows.ui.xaml.controls.primitives.popup)の XAML 要素に適用されると、自動的に影をキャストします。 その背後にあるアプリの背景コンテンツとその下にある他の開いているポップアップに影をキャストします。

ポップアップで ThemeShadow を使用するには、`Shadow` プロパティを使用して、ThemeShadow を XAML 要素に適用します。 次に、要素をその背後にある他の要素から昇格させます。たとえば、`Translation` プロパティの z コンポーネントを使用します。
ほとんどのポップアップ UI では、アプリの背景コンテンツに対して相対的な既定の昇格が32有効ピクセルになります。

次の例では、アプリの背景コンテンツとその背後にあるその他のポップアップに影をキャストするポップアップの四角形を示しています。

```xaml
<Popup>
    <Rectangle x:Name="PopupRectangle" Fill="Lavender" Height="48" Width="96">
        <Rectangle.Shadow>
            <ThemeShadow />
        </Rectangle.Shadow>
    </Rectangle>
</Popup>
```

```csharp
// Elevate the rectangle by 32px
PopupRectangle.Translation += new Vector3(0, 0, 32);
```

![影付きの単一の四角形ポップアップ。](images/elevation-shadow/PopupRectangle.png)

### <a name="disabling-default-themeshadow-on-custom-flyout-controls"></a>カスタムポップアップコントロールの既定の ThemeShadow を無効にする

[フライアウト](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.flyout)、 [DatePickerFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.datepickerflyout)、 [Menuflyout アウト](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.menuflyout)または[TimePickerFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.timepickerflyout)に基づくコントロールは、自動的に ThemeShadow を使用して影をキャストします。

既定の影がコントロールのコンテンツに対して正しく表示されない場合は、関連付けられている FlyoutPresenter の[IsDefaultShadowEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.flyoutpresenter.isdefaultshadowenabled)プロパティを `false` に設定して無効にすることができます。

```xaml
<Flyout>
    <Flyout.FlyoutPresenterStyle>
        <Style TargetType="FlyoutPresenter">
            <Setter Property="IsDefaultShadowEnabled" Value="False" />
        </Style>
    </Flyout.FlyoutPresenterStyle>
</Flyout>
```

### <a name="themeshadow-in-other-elements"></a>他の要素の ThemeShadow

一般に、影の使用について慎重に検討し、意味のある視覚的階層が導入されている場合にその使用を制限することをお勧めします。 ただし、高度なシナリオが必要になった場合に備えて、任意の UI 要素から影をキャストする方法を提供します。

ポップアップに含まれていない XAML 要素から影をキャストするには、`ThemeShadow.Receivers` コレクションでシャドウを受け取ることができる他の要素を明示的に指定する必要があります。 レシーバーをビジュアルツリー内のキャスターの先祖にすることはできません。

この例では、影を下のグリッドにキャストする2つの四角形を示します。

```xaml
<Grid>
    <Grid.Resources>
        <ThemeShadow x:Name="SharedShadow" />
    </Grid.Resources>

    <Grid x:Name="BackgroundGrid" Background="{ThemeResource ApplicationPageBackgroundThemeBrush}" />

    <Rectangle x:Name="Rectangle1" Height="100" Width="100" Fill="Turquoise" Shadow="{StaticResource SharedShadow}" />

    <Rectangle x:Name="Rectangle2" Height="100" Width="100" Fill="Turquoise" Shadow="{StaticResource SharedShadow}" />
</Grid>
```

```csharp
/// Add BackgroundGrid as a shadow receiver and elevate the casting buttons above it
SharedShadow.Receivers.Add(BackgroundGrid);

Rectangle1.Translation += new Vector3(0, 0, 16);
Rectangle2.Translation += new Vector3(120, 0, 32);
```

![2つの水色の四角形の横に影が付きます。](images/elevation-shadow/SharedShadow.png)

### <a name="performance-best-practices-for-themeshadow"></a>ThemeShadow のパフォーマンスのベストプラクティス

1. システムは、5 ~ 5 の魔法の受信者のペアの上限を設定し、これを超えた場合はシャドウをオフにします。 システムによって強制された上限である5つの受信者のペアを維持します。

2. カスタムのレシーバー要素の数を必要最小限に制限します。

3. 複数のレシーバー要素が同じ昇格にある場合は、代わりに1つの親要素をターゲットとして、それらを結合してみてください。

4. 複数の要素が同じ種類のシャドウを同じレシーバー要素にキャストする場合は、シャドウを共有リソースとして追加して再利用します。

## <a name="drop-shadow"></a>ドロップ シャドウ

DropShadow は環境に自動的に応答せず、光源を使用しません。 実装の例については、 [Dropshadow クラス](https://docs.microsoft.com/uwp/api/windows.ui.composition.dropshadow)を参照してください。

## <a name="which-shadow-should-i-use"></a>どのシャドウを使用すればよいですか。

| プロパティ | ThemeShadow | DropShadow |
| - | - | - |
| **最小 SDK** | Windows 10 バージョン1903 | 14393 |
| **適応性** | はい | いいえ |
| **Customization** | いいえ | はい |
| **光源** | 自動 (既定ではグローバルですが、アプリごとに上書きできます) | なし |
| **3D 環境でサポート** | はい | いいえ |

- Shadow の目的は、単純な視覚的な取り扱いではなく、意味のある階層を提供することに注意してください。
- 一般に、環境に自動的に適応する ThemeShadow を使用することをお勧めします。
- パフォーマンスに関する考慮事項については、影の数を制限したり、他の視覚の取り扱いを使用したり、DropShadow を使用したりします。
- ビジュアル階層を実現する高度なシナリオがある場合は、他の視覚処理 (色など) の使用を検討してください。 Shadow が必要な場合は、DropShadow を使用します。
