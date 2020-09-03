---
author: knicholasa
description: Z 深度 (相対的な深さ) とシャドウは、ユーザーが自然かつ効率的に注目できるように、アプリに深度を組み入れるための 2 つの方法です。
title: Windows アプリの Z 深度とシャドウ
template: detail.hbs
ms.date: 04/19/2019
ms.topic: article
ms.custom: 19H1
keywords: windows 10, uwp
pm-contact: chigy
ms.localizationpriority: medium
ms.openlocfilehash: fc2adb295df97cf1af49608d15c135b9f56b4594
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89165566"
---
# <a name="z-depth-and-shadow"></a>Z 深度とシャドウ

![斜めに並べられた 4 つの灰色の四角形が、互いに重ね合って表示された gif があります。 gif は、影が表示されたり消えたりするように、アニメーション化されています。](images/elevation-shadow/shadow.gif)

UI に要素の視覚的な階層を作成すると、その UI では注目すべき重要な部分を簡単にスキャンして伝えることができるようになります。 昇格は、UI の選択要素を前面に移動する操作であり、多くの場合、ソフトウェア内でこのような階層を実現するために使用されます。 この記事では、Z 深度とシャドウを使用して Windows アプリ内で昇格を作成する方法について説明します。

Z 深度とは、z 軸に沿った 2 つのサーフェイス間の距離を示すために、3D アプリ作成者の間で使用されている用語です。 オブジェクトが閲覧者にどの程度近いかを示します。 x/y 座標とほぼ同じ概念ですが、それが z 方向になると考えてください。

## <a name="why-use-z-depth"></a>Z 深度を使用する理由

物理的な世界では、自分により近接しているオブジェクトに注目する傾向があります。 この空間上の性質はデジタル UI にも適用できます。 たとえば、要素をユーザーのより近くに移動すると、ユーザーは直感的にその要素に注目します。 z 軸上で UI 要素をより近くに移動することで、ユーザーがアプリ内で自然かつ効率的にタスクを完了できるように、オブジェクト間に視覚的な階層を確立することができます。

## <a name="what-is-shadow"></a>シャドウとは

シャドウは、ユーザーが昇格を知覚するための 1 つの方法です。 昇格されたオブジェクトの上の照明によって、下のサーフェイスに影を作成します。 オブジェクトの位置が高いほど、影はより大きく柔らかくなります。 UI 上で昇格されたオブジェクトに必ずシャドウが必要なわけではありませんが、昇格の外観を作り出すのに役立ちます。

Windows アプリでは、見た目を良くするためではなく、目的指向でシャドウを使用する必要があります。 使用するシャドウの数が多すぎると、ユーザーの注目を促すシャドウの働きが低下または除去されてしまいます。

標準コントロールを使用すると、ThemeShadow のシャドウが UI に自動的に組み込まれます。 ただし、ThemeShadow API または DropShadow API のどちらかを使用して、UI に手動でシャドウを組み入れることもできます。 

## <a name="themeshadow"></a>ThemeShadow

[ThemeShadow](/uwp/api/windows.ui.xaml.media.themeshadow) の種類は任意の XAML 要素に適用され、x、y、z 座標に基づいてシャドウを適切に描画できます。 また、ThemeShadow は、その他の環境仕様に合わせて自動的に調整されます。

- 照明、ユーザー テーマ、アプリ環境、およびシェルの変更に適応します。
- Z 深度に基づいて、要素に自動的にシャドウを適用します。 
- 移動時および昇格の変更時には、要素の同期が維持されます。
- シャドウの一貫性は、アプリケーション全体やアプリケーション間で維持されます。

ThemeShadow が MenuFlyout 上にどのように実装されているかを、次に示します。 MenuFlyout では、メイン サーフェイスが 32 px に昇格され、その他の各カスケード メニューは開かれた元のメニューより +8 px 上に開かれるように、エクスペリエンスが組み込まれています。

![MenuFlyout に ThemeShadow が適用され、3 つのメニューが入れ子になって開かれているスクリーンショット。 1 つ目のメニューは 32 px 昇格され、それに続いて前のメニューから開かれる各メニューは、背景にはっきりとシャドウが残るようにさらに 8 px 昇格されています。](images/elevation-shadow/themeshadow-menuflyout.png)

### <a name="themeshadow-in-common-controls"></a>コモン コントロールでの ThemeShadow

次のコモン コントロールでは、特に指定されていない限り、ThemeShadow が自動的に使用されて 32 px の深度からシャドウが投影されます。

- [コンテキスト メニュー](../controls-and-patterns/menus.md)、[コマンド バー](../controls-and-patterns/app-bars.md)、[コマンド バー ポップアップ](../controls-and-patterns/command-bar-flyout.md)、[MenuBar](../controls-and-patterns/menus.md#create-a-menu-bar)
- [ダイアログとポップアップ](../controls-and-patterns/dialogs-and-flyouts/index.md) (64 px でのダイアログ)
- [NavigationView](../controls-and-patterns/navigationview.md)
- [ComboBox](../controls-and-patterns/combo-box.md)、[DropDownButton、SplitButton、ToggleSplitButton](../controls-and-patterns/buttons.md)
- [TeachingTip](../controls-and-patterns/dialogs-and-flyouts/teaching-tip.md)
- [AutoSuggestBox](../controls-and-patterns/auto-suggest-box.md) 
- [カレンダー/日付/時刻の選択](../controls-and-patterns/date-and-time.md)
- [Tooltip](../controls-and-patterns/tooltips.md) (16px)
- [メディア トランスポート コントロール](../controls-and-patterns/media-playback.md#media-transport-controls)、[InkToolbar](../controls-and-patterns/inking-controls.md)
- [接続型アニメーション](../motion/connected-animation.md)

注: ポップアップには、Windows 10 バージョン 1903 以上の新しい SDK に対してコンパイルされた場合にのみ、ThemeShadow が適用されます。

### <a name="themeshadow-in-popups"></a>ポップアップでの ThemeShadow

ユーザーの注目と迅速なアクションが必要なシナリオでは、多くの場合、アプリの UI にポップアップが使用されます。 これらは、シャドウを使用してアプリの UI に階層を作成することがふさわしい例です。

ThemeShadow は、[Popup](/uwp/api/windows.ui.xaml.controls.primitives.popup) 内の任意の XAML 要素に適用されると、自動的にシャドウを投影します。 背後にあるアプリの背景コンテンツと、下にあるそれ以外の開かれたポップアップに対して、シャドウが投影されます。

ポップアップに ThemeShadow を使用するには、`Shadow` プロパティを使用して ThemeShadow を XAML 要素に適用します。 次に、たとえば `Translation` プロパティの z コンポーネントを使用して、要素をその背後にある他の要素から昇格させます。
ほとんどのポップアップ UI では、アプリの背景コンテンツに対して推奨される相対的な既定の昇格は 32 有効ピクセルになります。

この例では、ポップアップの 1 つの四角形が、アプリの背景コンテンツとその背後にあるその他のポップアップに対して、シャドウを投影している状態を示しています。

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

![1 つの四角形のシャドウ付きポップアップ。](images/elevation-shadow/PopupRectangle.png)

### <a name="disabling-default-themeshadow-on-custom-flyout-controls"></a>カスタム ポップアップ コントロール上の既定の ThemeShadow を無効にする

[Flyout](/uwp/api/Windows.UI.Xaml.Controls.flyout)、[DatePickerFlyout](/uwp/api/windows.ui.xaml.controls.datepickerflyout)、[MenuFlyout](/uwp/api/Windows.UI.Xaml.Controls.menuflyout) または [TimePickerFlyout](/uwp/api/windows.ui.xaml.controls.timepickerflyout) に基づくコントロールでは、自動的に ThemeShadow が使用されてシャドウが投影されます。

既定のシャドウがコントロールのコンテンツに対して正しく表示されない場合は、関連付けられている FlyoutPresenter 上で [IsDefaultShadowEnabled](/uwp/api/windows.ui.xaml.controls.flyoutpresenter.isdefaultshadowenabled) プロパティを `false` に設定することで、無効にすることができます。

```xaml
<Flyout>
    <Flyout.FlyoutPresenterStyle>
        <Style TargetType="FlyoutPresenter">
            <Setter Property="IsDefaultShadowEnabled" Value="False" />
        </Style>
    </Flyout.FlyoutPresenterStyle>
</Flyout>
```

### <a name="themeshadow-in-other-elements"></a>他の要素内の ThemeShadow

一般に、シャドウの使用については慎重に検討して、意味のある視覚的な階層が導入されるケースにその使用を限定することをお勧めします。 ただし、シャドウが必要になる高度なシナリオが発生した場合に備えて、任意の UI 要素からシャドウを投影する方法が提供されています。

ポップアップに含まれていない XAML 要素からシャドウを投影するには、`ThemeShadow.Receivers` コレクションでシャドウを受け取ることができる他の要素を明示的に指定する必要があります。 ビジュアル ツリー内で、投影先を投影元の先祖にすることはできません。

この例では、背後にあるグリッドにシャドウを投影する 2 つの四角形を示します。

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

![2 つの水色の四角形が横に並べられ、どちらにもシャドウがあります。](images/elevation-shadow/SharedShadow.png)

### <a name="performance-best-practices-for-themeshadow"></a>ThemeShadow のパフォーマンスのベスト プラクティス

1. システムでは、投影元と投影先のペアに 5 組までの制限を設定しており、これを超過すると、シャドウがオフになります。 投影元と投影先のペアは、必ず、システムによって適用される上限の 5 組までにしてください。

2. カスタムの投影先要素の数は、必要最小限に抑えてください。

3. 複数の投影先要素が同じ昇格にある場合は、代わりに 1 つの親要素をターゲットにして、それらの結合を試みてください。

4. 複数の要素によって同じ種類のシャドウが同じ投影先要素に投影される場合は、そのシャドウを共有リソースとして追加して、再利用してください。

## <a name="drop-shadow"></a>ドロップ シャドウ

DropShadow では自動的な環境への応答は行われず、光源を使用しません。 実装の例については、「[DropShadow クラス](/uwp/api/windows.ui.composition.dropshadow)」を参照してください。

## <a name="which-shadow-should-i-use"></a>どのシャドウを使用すればよいか

| プロパティ | ThemeShadow | DropShadow |
| - | - | - |
| **最小の SDK** | Windows 10 バージョン 1903 | 14393 |
| **適応性** | はい | いいえ |
| **カスタマイズ** | いいえ | はい |
| **光源** | 自動 (既定ではグローバルだが、アプリごとにオーバーライドできる) | None |
| **3D 環境でのサポートの状況** | はい | いいえ |

- シャドウの目的は、単純な視覚的処理ではなく、意味のある階層の提供であることを念頭に置いてください。
- 通常は、環境に自動的に適応する ThemeShadow を使用することをお勧めします。
- パフォーマンスに関する懸念事項に対応するには、シャドウ数の制限、他の視覚処理の使用、または DropShadow の使用を行います。
- より高度なシナリオで視覚的な階層を実現する場合は、他の視覚処理 (色など) の使用を検討してください。 シャドウが必要な場合は、DropShadow を使用します。