---
description: 接続型アニメーションを使用すると、2 つの異なるビューの間で要素が切り替わる様子をアニメーション化することによって、動的で魅力的なナビゲーション エクスペリエンスを作成できます。
title: 接続型アニメーション
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: conrwi
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 770f859cfb90dde4f3960492479beb6192c12c8f
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/24/2020
ms.locfileid: "91217785"
---
# <a name="connected-animation-for-windows-apps"></a>Windows アプリの接続されたアニメーション

接続型アニメーションを使用すると、2 つの異なるビューの間で要素が切り替わる様子をアニメーション化することによって、動的で魅力的なナビゲーション エクスペリエンスを作成できます。 これにより、ユーザーはコンテキストを維持して、ビューの間の継続性を実現することができます。

接続されたアニメーションでは、UI コンテンツの変更時に2つのビューの間で "continue" という要素が表示されます。これにより、ソースビュー内の場所から新しいビューのターゲットに画面が移動します。 これにより、ビュー間の一般的なコンテンツが強調され、遷移の一部として美しい効果と動的効果が作成されます。

> **重要な api**:  [connectedanimation クラス](/uwp/api/windows.ui.xaml.media.animation.connectedanimation)、 [connectedanimation service クラス](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice)


## <a name="examples"></a>例

<table>
<th align="left">XAML コントロール ギャラリー<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML コントロールギャラリー</strong>アプリがインストールされている場合は、ここをクリックして<a href="xamlcontrolsgallery:/item/ConnectedAnimation">アプリを開き、接続されているアニメーションが動作して</a>いることを確認します。</p>
    <ul>
    <li><a href="https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt">XAML コントロール ギャラリー アプリを入手する (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">ソース コード (GitHub) を入手する</a></li>
    </ul>
</td>
</tr>
</table>

この短いビデオでは、アプリは接続されたアニメーションを使用して、次のページのヘッダーの一部として "続行" される項目イメージをアニメーション化します。 この効果を利用すると、画面の切り替えでユーザー コンテキストを維持することができます。

![接続型アニメーション](images/connected-animations/example.gif)

<!-- 
<iframe width=640 height=360 src='https://microsoft.sharepoint.com/portals/hub/_layouts/15/VideoEmbedHost.aspx?chId=552c725c%2De353%2D4118%2Dbd2b%2Dc2d0584c9848&amp;vId=b2daa5ee%2Dbe15%2D4503%2Db541%2D1328a6587c36&amp;width=640&amp;height=360&amp;autoPlay=false&amp;showInfo=true' allowfullscreen></iframe>
-->

## <a name="video-summary"></a>ビデオの概要

> [!VIDEO https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Fall-Creators-Update/WinDev005/player]

## <a name="connected-animation-and-the-fluent-design-system"></a>接続型アニメーションと Fluent Design System

 Fluent Design System では、ライト、深度、モーション、マテリアル、スケールを取り入れた、モダンで目を引く UI を作成できます。 接続型アニメーションは、アプリに動きを加える Fluent Design System コンポーネントです。 詳しくは、[Fluent Design の概要](/windows/apps/fluent-design-system)に関するページをご覧ください。

## <a name="why-connected-animation"></a>接続型アニメーションを使用する理由

ページ間を移動するときには、移動後にどのような新しいコンテンツが表示されるか、その新しいコンテンツがページを移動するユーザーの目的とどのように関連しているかを、ユーザーが理解できるようにすることが重要です。 接続型アニメーションを利用すると、2 つのビューで共有されているコンテンツにユーザーを注目させることによって、2 つのビューの間の関係を強調する強力な視覚的メタファが実現されます。 また、接続型アニメーションによって、ページを移動するときに、視覚的に注目を引く効果や洗練された視覚効果を発生させることができます。このことは、アプリのモーション デザインを特徴付ける際に役立ちます。

## <a name="when-to-use-connected-animation"></a>接続型アニメーションを使用するタイミング

一般に、接続型アニメーションはページを変更するとき使用されます。ただし、UI のコンテンツを変更するときに、そのコンテンツが維持されるようにユーザーに対して表示する必要がある場合は、接続型アニメーションをどのようなエクスペリエンスにでも適用できます。 ソース ビューと切り替え先のビューの間で UI の画像や他の UI の要素が共有されている場合は、必ず、[ドリル インによるナビゲーション切り替え](/uwp/api/Windows.UI.Xaml.Media.Animation.NavigationThemeTransition)ではなく、接続型アニメーションの使用を検討してください。

## <a name="configure-connected-animation"></a>接続されたアニメーションの構成

> [!IMPORTANT]
> この機能を使用するには、アプリのターゲットバージョンが Windows 10 バージョン 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) 以降である必要があります。 構成プロパティは、以前の Sdk では使用できません。 アダプティブコードまたは条件付き XAML を使用して、SDK 17763 より低い最小バージョンを対象にすることができます。 詳細については、「 [バージョンアダプティブアプリ](../../debug-test-perf/version-adaptive-apps.md)」を参照してください。

Windows 10 バージョン1809以降では、接続されたアニメーションは、前方および後方ページナビゲーション専用に調整されたアニメーション構成を提供することで、Fluent デザインをさらに具体化します。

アニメーションの構成を指定するには、ConnectedAnimation の構成プロパティを設定します。 (例については、次のセクションで説明します)。

次の表では、使用可能な構成について説明します。 これらのアニメーションで適用されるモーション原則の詳細については、「 [方向性と重力](index.md)」を参照してください。

| [GravityConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.gravityconnectedanimationconfiguration) |
| - |
| これは既定の構成であり、前方ナビゲーションの場合に推奨されます。 |
ユーザーがアプリ (A から B) に進むと、接続されている要素が物理的に "ページの取り出し" になります。 このようにすると、要素は z 空間で前方に移動し、重力をかけた場合の効果として少しだけ削除されます。 重力の効果を克服するために、要素はベロシティを獲得し、最終的な位置に加速します。 結果は "scale and dip" アニメーションになります。 |

| [DirectConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.directconnectedanimationconfiguration) |
| - |
| ユーザーがアプリ内で後方に移動すると (B から A)、アニメーションはより直接的になります。 接続された要素は、減速の3次ベジエイージング関数を使用して、B からに線形変換します。 後方 visual affordance は、ナビゲーションフローのコンテキストを維持したまま、できるだけ高速にユーザーを前の状態に戻します。 |

| [BasicConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.basicconnectedanimationconfiguration) |
| - |
| これは、Windows 10 バージョン 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) より前のバージョンで使用されている既定の (と唯一の) アニメーションです。 |

### <a name="connectedanimationservice-configuration"></a>ConnectedAnimationService の構成

[Connectedanimationservice](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice)クラスには、サービス全体ではなく個々のアニメーションに適用される2つのプロパティがあります。

- [DefaultDuration](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.defaultduration)
- [DefaultEasingFunction](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.defaulteasingfunction)

さまざまな効果を実現するために、一部の構成では、この表で説明するように、ConnectedAnimationService でこれらのプロパティを無視し、独自の値を使用します。

| 構成 | DefaultDuration に従いますか。 | DefaultEasingFunction を尊重しますか。 |
| - | - | - |
| 重力 | はい | はい* <br/> **A から B への基本的な変換では、このイージング関数を使用しますが、"重力 dip" には独自のイージング関数があります。*  |
| 直接 | いいえ <br/> *150ミリ秒を超えてアニメーション化します。*| いいえ <br/> *減速イージング関数を使用します。* |
| Basic | はい | はい |

## <a name="how-to-implement-connected-animation"></a>接続されたアニメーションを実装する方法

接続型アニメーションのセットアップでは、次の 2 つの手順を実行します。

1. ソースページでアニメーションオブジェクトを*準備*します。これは、ソース要素が接続されたアニメーションに参加するシステムを示します。
1. コピー先のページでアニメーションを*開始*し、ターゲット要素への参照を渡します。

ソースページから移動する場合は、 [ConnectedGetForCurrentView](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.getforcurrentview) を呼び出して、connectedanimationservice のインスタンスを取得します。 アニメーションを準備するには、このインスタンスで [インスタンス [化](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.preparetoanimate) ] を呼び出し、切り替えに使用する一意のキーと UI 要素を渡します。 [一意キー] を使用すると、後で変換先ページでアニメーションを取得できます。

```csharp
ConnectedAnimationService.GetForCurrentView()
    .PrepareToAnimate("forwardAnimation", SourceImage);
```

ナビゲーションが発生したら、[変換先] ページでアニメーションを開始します。 アニメーションを開始するには、[ConnectedAnimation.TryStart](/uwp/api/windows.ui.xaml.media.animation.connectedanimation.trystart) を呼び出します。 アニメーションの作成時に指定した一意のキーを使用して [ConnectedAnimationService.GetAnimation](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.getanimation) を呼び出すことにより、適切なアニメーションのインスタンスを取得できます。

```csharp
ConnectedAnimation animation =
    ConnectedAnimationService.GetForCurrentView().GetAnimation("forwardAnimation");
if (animation != null)
{
    animation.TryStart(DestinationImage);
}
```

### <a name="forward-navigation"></a>前方移動

この例では、ConnectedAnimationService を使用して、2つのページ間の前方移動 (Page_A Page_B) の遷移を作成する方法を示します。

前方ナビゲーションの推奨されるアニメーション構成は [GravityConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.gravityconnectedanimationconfiguration)です。 これは既定値であるため、別の構成を指定する場合を除き、 [構成](/uwp/api/windows.ui.xaml.media.animation.connectedanimation.configuration) プロパティを設定する必要はありません。

ソースページでアニメーションを設定します。

```xaml
<!-- Page_A.xaml -->

<Image x:Name="SourceImage"
       HorizontalAlignment="Left" VerticalAlignment="Top"
       Width="200" Height="200"
       Stretch="Fill"
       Source="Assets/StoreLogo.png"
       PointerPressed="SourceImage_PointerPressed"/>
```

```csharp
// Page_A.xaml.cs

private void SourceImage_PointerPressed(object sender, PointerRoutedEventArgs e)
{
    // Navigate to detail page.
    // Suppress the default animation to avoid conflict with the connected animation.
    Frame.Navigate(typeof(Page_B), null, new SuppressNavigationTransitionInfo());
}

protected override void OnNavigatingFrom(NavigatingCancelEventArgs e)
{
    ConnectedAnimationService.GetForCurrentView()
        .PrepareToAnimate("forwardAnimation", SourceImage);
    // You don't need to explicitly set the Configuration property because
    // the recommended Gravity configuration is default.
    // For custom animation, use:
    // animation.Configuration = new BasicConnectedAnimationConfiguration();
}
```

[変換先] ページでアニメーションを開始します。

```xaml
<!-- Page_B.xaml -->

<Image x:Name="DestinationImage"
       Width="400" Height="400"
       Stretch="Fill"
       Source="Assets/StoreLogo.png" />
```

```csharp
// Page_B.xaml.cs

protected override void OnNavigatedTo(NavigationEventArgs e)
{
    base.OnNavigatedTo(e);

    ConnectedAnimation animation =
        ConnectedAnimationService.GetForCurrentView().GetAnimation("forwardAnimation");
    if (animation != null)
    {
        animation.TryStart(DestinationImage);
    }
}
```

### <a name="back-navigation"></a>戻るナビゲーション

戻るナビゲーション (Page_B Page_A) の場合は、同じ手順を実行しますが、コピー元とコピー先のページは逆になります。

ユーザーが戻ってくると、アプリはできるだけ早く前の状態に戻ることを期待しています。 そのため、推奨される構成は、 [Directconnectedanimationconfiguration](/uwp/api/windows.ui.xaml.media.animation.directconnectedanimationconfiguration)です。 このアニメーションは高速で、より直接的で、減速のイージングを使用します。

ソースページでアニメーションを設定します。

```csharp
// Page_B.xaml.cs

protected override void OnNavigatingFrom(NavigatingCancelEventArgs e)
{
    if (e.NavigationMode == NavigationMode.Back)
    {
        ConnectedAnimation animation = 
            ConnectedAnimationService.GetForCurrentView().PrepareToAnimate("backAnimation", DestinationImage);

        // Use the recommended configuration for back animation.
        animation.Configuration = new DirectConnectedAnimationConfiguration();
    }
}
```

[変換先] ページでアニメーションを開始します。

```csharp
// Page_A.xaml.cs

protected override void OnNavigatedTo(NavigationEventArgs e)
{
    base.OnNavigatedTo(e);

    ConnectedAnimation animation =
        ConnectedAnimationService.GetForCurrentView().GetAnimation("backAnimation");
    if (animation != null)
    {
        animation.TryStart(SourceImage);
    }
}
```

アニメーションがセットアップされて開始されると、ソース要素はアプリ内の他の UI の上に固定された状態で表示されます。 これにより、その他の遷移アニメーションを同時に実行できます。 このため、ソース要素の存在が煩雑になる可能性があるため、2つの手順の間で ~ 250 ミリ秒以上待機することはできません。 アニメーションを準備しても、アニメーションを 3 秒以内に開始しないと、システムによってアニメーションが破棄され、後続の [TryStart](/uwp/api/windows.ui.xaml.media.animation.connectedanimation.trystart) の呼び出しは失敗します。

## <a name="connected-animation-in-list-and-grid-experiences"></a>リスト エクスペリエンスとグリッド エクスペリエンスでの接続型アニメーション

多くの場合、リスト コントロールやグリッド コントロール間の切り替えで接続型アニメーションの作成が必要になります。 [ListView](/uwp/api/windows.ui.xaml.controls.listview)と[GridView](/uwp/api/windows.ui.xaml.controls.gridview)の2つのメソッドを使用し[て、この](/uwp/api/windows.ui.xaml.controls.listviewbase.prepareconnectedanimation)プロセス[TryStartConnectedAnimationAsync](/uwp/api/windows.ui.xaml.controls.listviewbase.trystartconnectedanimationasync)を簡略化することができます。

たとえば、データ テンプレート内に "PortraitEllipse" という名前の要素を含んでいる **ListView** があるとします。

```xaml
<ListView x:Name="ContactsListView" Loaded="ContactsListView_Loaded">
    <ListView.ItemTemplate>
        <DataTemplate x:DataType="vm:ContactsItem">
            <Grid>
                …
                <Ellipse x:Name="PortraitEllipse" … />
            </Grid>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

指定されたリスト項目に対応する楕円を使用して接続されたアニメーションを準備するには、一意のキー、項目、および名前 "PortraitEllipse" を使用し [て、"](/uwp/api/windows.ui.xaml.controls.listviewbase.prepareconnectedanimation) " という名前の、"" という名前の "/" を呼び出します。

```csharp
void PrepareAnimationWithItem(ContactsItem item)
{
     ContactsListView.PrepareConnectedAnimation("portrait", item, "PortraitEllipse");
}
```

詳細ビューから戻るときなど、この要素を変換先として使用するアニメーションを開始するには、 [Trystartconnectedanimation async](/uwp/api/windows.ui.xaml.controls.listviewbase.trystartconnectedanimationasync)を使用します。 ListView のデータ ソースが読み込まれると、TryStartConnectedAnimationAsync は、対応する項目コンテナーが作成されるまで、アニメーションが開始されるのを待機します。

```csharp
private async void ContactsListView_Loaded(object sender, RoutedEventArgs e)
{
    ContactsItem item = GetPersistedItem(); // Get persisted item
    if (item != null)
    {
        ContactsListView.ScrollIntoView(item);
        ConnectedAnimation animation =
            ConnectedAnimationService.GetForCurrentView().GetAnimation("portrait");
        if (animation != null)
        {
            await ContactsListView.TryStartConnectedAnimationAsync(
                animation, item, "PortraitEllipse");
        }
    }
}
```

## <a name="coordinated-animation"></a>連動型アニメーション

![連動型アニメーション](images/connected-animations/coordinated_example.gif)

<!--
<iframe width=640 height=360 src='https://microsoft.sharepoint.com/portals/hub/_layouts/15/VideoEmbedHost.aspx?chId=552c725c%2De353%2D4118%2Dbd2b%2Dc2d0584c9848&amp;vId=9066bbbe%2Dcf58%2D4ab4%2Db274%2D595616f5d0a0&amp;width=640&amp;height=360&amp;autoPlay=false&amp;showInfo=true' allowfullscreen></iframe>
-->

コーディネートされた *アニメーション* とは、接続されたアニメーションのターゲットと共に要素が表示され、接続されたアニメーションの要素が画面間を移動するときにアニメーション化される、特殊な種類の入口アニメーションです。 連動型アニメーションによって、ビューの切り替え時に視覚的にさらに注目を引く効果が発生し、ソース ビューと切り替え先のビューの間で共有されているコンテキストにユーザーを注目させることができます。 上記の画像では、連動型アニメーションを使用して、項目のキャプション UI がアニメーション化されています。

コーディネートされたアニメーションで重力構成を使用する場合、[接続されたアニメーション] 要素と [調整された要素] の両方に重力が適用されます。 調整された要素は、接続された要素と共に "一挙" されるので、要素は実際には調整されたままです。

**TryStart** の 2 つのパラメーター オーバーロードを使用して、連動型の要素を接続型アニメーションに追加します。 この例では、"カバーイメージ" という名前の接続されたアニメーション要素と共に入力する "DescriptionRoot" という名前のグリッドレイアウトの、連携したアニメーションを示します。

```xaml
<!-- DestinationPage.xaml -->
<Grid>
    <Image x:Name="CoverImage" />
    <Grid x:Name="DescriptionRoot" />
</Grid>
```

```csharp
// DestinationPage.xaml.cs
void OnNavigatedTo(NavigationEventArgs e)
{
    var animationService = ConnectedAnimationService.GetForCurrentView();
    var animation = animationService.GetAnimation("coverImage");

    if (animation != null)
    {
        // Don’t need to capture the return value as we are not scheduling any subsequent
        // animations
        animation.TryStart(CoverImage, new UIElement[] { DescriptionRoot });
     }
}
```

## <a name="dos-and-donts"></a>推奨と非推奨

- 接続型アニメーションは、ソース ページと切り替え先のページの間で要素が共有されている場合に、ページの切り替えで使用してください。
- 前方ナビゲーションには [GravityConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.gravityconnectedanimationconfiguration) を使用します。
- 戻るナビゲーションには、 [Directconnectedanimationconfiguration](/uwp/api/windows.ui.xaml.media.animation.directconnectedanimationconfiguration) を使用します。
- ネットワーク要求や、接続されたアニメーションの準備と開始の間で実行時間の長い非同期操作を待機しないでください。 早めに切り替えを実行するには、必要な情報を事前に読み込んでおく必要があります。または、高解像度の画像が切り替え先のビューに読み込まれるときに、低解像度のプレースホルダー画像を使用する必要があります。
- 接続されたアニメーションが既定のナビゲーション遷移で同時に使用されることはないため、 **Connectedanimation service**を使用している場合は、 [SuppressNavigationTransitionInfo](/uwp/api/windows.ui.xaml.media.animation.suppressnavigationtransitioninfo)を使用して**フレーム**内の遷移アニメーションを防止します。 ナビゲーション切り替えの使用方法について詳しくは、「[NavigationThemeTransition](/uwp/api/Windows.UI.Xaml.Media.Animation.NavigationThemeTransition)」をご覧ください。

## <a name="related-articles"></a>関連記事

[ConnectedAnimation](/uwp/api/windows.ui.xaml.media.animation.connectedanimation)

[ConnectedAnimationService](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice)

[NavigationThemeTransition](/uwp/api/Windows.UI.Xaml.Media.Animation.NavigationThemeTransition)
