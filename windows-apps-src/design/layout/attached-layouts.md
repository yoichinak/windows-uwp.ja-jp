---
description: ItemsRepeater コントロールなどのコンテナーで使用するために、接続されているレイアウトを定義できます。
title: AttachedLayout
label: AttachedLayout
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 62ecc21d3ed9835ae7360d0c0dfdfa0b09cbdced
ms.sourcegitcommit: 4df27104a9e346d6b9fb43184812441fe5ea3437
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/26/2020
ms.locfileid: "93034865"
---
# <a name="attached-layouts"></a>接続されているレイアウト

レイアウト ロジックを別のオブジェクトにデリゲートするコンテナー (パネルなど) は、接続されたレイアウト オブジェクトに依存して、子要素のレイアウト動作を提供します。  接続されたレイアウト モデルにより、アプリケーションで実行時に項目のレイアウトを変更する柔軟性が提供され、または、UI のさまざまな部分間で、レイアウトの側面を簡単に共有できます (たとえば、列内で位置合わせされているように見える表の行内の項目など)。

このトピックでは、接続されているレイアウト (仮想化と非仮想化) の作成に必要なもの、理解する必要がある概念とクラス、およびそれらを決定する際に考慮する必要があるトレードオフについて説明します。

| **Windows UI ライブラリを入手する** |
| - |
| このコントロールは、Windows アプリのための新しいコントロールと UI 機能を含む NuGet パッケージである Windows UI ライブラリの一部として含まれています。 インストール手順などの詳細については、[Windows UI ライブラリの概要](/uwp/toolkits/winui/)に関するページを参照してください。 |

> **重要な API**:

> * [ScrollViewer](/uwp/api/windows.ui.xaml.controls.scrollviewer)
> * [ItemsRepeater](../controls-and-patterns/items-repeater.md)
> * [レイアウト](/uwp/api/microsoft.ui.xaml.controls.layout)
>     * [NonVirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout)
>     * [VirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout)
> * [LayoutContext](/uwp/api/microsoft.ui.xaml.controls.layoutcontext)
>     * [NonVirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayoutcontext)
>     * [VirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext)
> * [LayoutPanel](/uwp/api/microsoft.ui.xaml.controls.layoutpanel) (プレビュー)

## <a name="key-concepts"></a>主要概念

レイアウトの実行には、すべての要素に対して 2 つの質問に答える必要があります。

1. この要素の "***サイズ** _" はどのくらいにしますか?

2. この要素の "_*_位置_*_" はどうしますか?

これらの質問に答える XAML のレイアウト システムについては、[カスタム パネル](./custom-panels-overview.md)の説明の一部として簡単に取り上げています。

### <a name="containers-and-context"></a>コンテナーとコンテキスト

概念上、XAML の [パネル](/uwp/api/windows.ui.xaml.controls.panel) は、フレームワークにおける 2 つの重要な役割を占めます。

1. それには、子要素を含めることができ、要素のツリーに分岐を導入します。
2. それらの子には、特定のレイアウト戦略が適用されます。

このため、XAML のパネルはレイアウトと同義にされることがよくありますが、技術的にいえば、レイアウトより多くのことを実行します。

[ItemsRepeater](../controls-and-patterns/items-repeater.md) もパネルのように動作しますが、パネルと異なり、プログラムによって UIElement の子を追加または削除できる Children プロパティを公開していません。  代わりに、その子の有効期間は、フレームワークによって、データ項目のコレクションに対応するように自動的に管理されます。  それはパネルから派生していませんが、パネルのように動作し、フレームワークから処理されます。

> [!NOTE]
> [LayoutPanel](/uwp/api/microsoft.ui.xaml.controls.layoutpanel) は、パネルから派生したコンテナーであり、接続されている [Layout](/uwp/api/microsoft.ui.xaml.controls.layoutpanel.layout) オブジェクトにそのロジックをデリゲートします。  LayoutPanel は "プレビュー" 段階にあり、現在、WinUI パッケージの "*プレリリース*" でのみ使用できます。

#### <a name="containers"></a>コンテナー

概念上、[パネル](/uwp/api/windows.ui.xaml.controls.panel)は、[バックグラウンド](/uwp/api/windows.ui.xaml.controls.panel.background)のピクセルをレンダリングする機能も備えている要素のコンテナーです。  パネルは、一般的なレイアウト ロジックを、使いやすいパッケージにカプセル化する方法を提供します。

**接続されているレイアウト** の概念により、コンテナーとレイアウトの 2 つの役割の区別が明確になります。  次のスニペットに示すように、コンテナーがそのレイアウト ロジックを別のオブジェクトにデリゲートする場合、そのオブジェクトを、接続されているレイアウトと呼びます。 LayoutPanel などの [FrameworkElement](/uwp/api/windows.ui.xaml.frameworkelement) から継承されたコンテナーは、XAML のレイアウト プロセスに入力を提供する共通プロパティ (たとえば、高さや幅) を自動的に公開します。

```xaml
<LayoutPanel>
    <LayoutPanel.Layout>
        <UniformGridLayout/>
    </LayoutPanel.Layout>
    <Button Content="1"/>
    <Button Content="2"/>
    <Button Content="3"/>
</LayoutPanel>
```

レイアウト プロセス時に、コンテナーは、接続されている *UniformGridLayout* に依存して、その子を測定し、配置します。

#### <a name="per-container-state"></a>コンテナーごとの状態

接続されているレイアウトによって、次のスニペットのように、レイアウト オブジェクトの 1 つのインスタンスが *多数の* コンテナーに関連付けられることがあります。そのため、ホスト コンテナーに依存したり、直接参照したりしないでください。  たとえば、次のように入力します。

```xaml
<!-- ... --->
<Page.Resources>
    <ExampleLayout x:Name="exampleLayout"/>
<Page.Resources>

<LayoutPanel x:Name="example1" Layout="{StaticResource exampleLayout}"/>
<LayoutPanel x:Name="example2" Layout="{StaticResource exampleLayout}"/>
<!-- ... --->
```

この状況の場合、*ExampleLayout* では、レイアウトの計算で使用する状態と、その状態が格納される場所を慎重に検討して、1 つのパネルの要素のレイアウトが他のパネルに影響を及ぼさないようにする必要があります。  MeasureOverride ロジックと ArrangeOverride ロジックがその *静的* プロパティの値に依存するカスタム パネルに似ています。

#### <a name="layoutcontext"></a>LayoutContext

[LayoutContext](/uwp/api/microsoft.ui.xaml.controls.layoutcontext) の目的は、それらの課題に対処することです。  これにより、接続されているレイアウトに、双方の間に直接の依存関係を導入しなくても、子要素の取得などのホスト コンテナーと対話する機能が得られます。 そのコンテキストにより、レイアウトで、コンテナーの子要素に関連する可能性のある、必要な任意の状態を格納することもできます。

シンプルな非仮想化レイアウトでは、多くの場合に状態を維持する必要はなく、問題になりません。 ただし、グリッドなどのより複雑なレイアウトでは、値の再計算を避けるために、測定と整列の呼び出しの間で状態を維持するように選択することがあります。

レイアウトを仮想化することは、*多くの場合に*、測定と整列の両方の間に加えて、反復的レイアウト パス間で状態を維持する必要があります。

#### <a name="initializing-and-uninitializing-per-container-state"></a>コンテナーごとの状態の初期化と初期化解除

レイアウトがコンテナーに接続されると、その [InitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.initializeforcontextcore) メソッドが呼び出され、状態を格納するためにオブジェクトを初期化する機会が得られます。

同様に、レイアウトがコンテナーから削除されると、[UninitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.uninitializeforcontextcore) メソッドが呼び出されます。  これにより、レイアウトには、そのコンテナーに関連付けられた状態をクリーンアップする機会が与えられます。

レイアウトの状態オブジェクトは、コンテキストの [LayoutState](/uwp/api/microsoft.ui.xaml.controls.layoutcontext.layoutstate) プロパティによって、コンテナーに格納し、コンテナーから取得できます。

### <a name="ui-virtualization"></a>UI の仮想化

UI の仮想化とは、_必要になるタイミング_ まで、UI オブジェクトの作成を遅らせることを意味します。  これはパフォーマンス最適化です。  非スクロール シナリオでは、_必要になるタイミング_ の決定は、アプリ固有の任意の数のものに基づくことがあります。  このような場合、アプリでは [x:Load](../../xaml-platform/x-load-attribute.md) の使用を検討する必要があります。 これにより、レイアウトに特別な処理は必要ありません。

リストなどのスクロールベースのシナリオでは、_必要になるタイミング_ の判断は、多くの場合に "ユーザーに表示されるか" に基づきます。これは、レイアウト プロセス時にそれが配置された場所に大きく依存し、特別な考慮が必要です。  このドキュメントでは、このシナリオを中心に説明します。

> [!NOTE]
> このドキュメントでは取り上げませんが、スクロール シナリオで UI 仮想化を有効にする同じ機能を、非スクロール シナリオでも適用できます。  たとえば、それに表示されるコマンドの有効期間を管理し、表示される領域とオーバーフロー メニューの間で要素をリサイクルまたは移動することによって、使用可能な領域の変更に対応するデータドリブンのツールバー コントロールがあります。

## <a name="getting-started"></a>はじめに

まず、作成する必要があるレイアウトで UI 仮想化をサポートする必要があるかどうかを決定します。

**注意すべき点がいくつかあります。**

1. 非仮想化レイアウトの方が簡単に作成できます。 項目の数が常に少ない場合は、非仮想化レイアウトを作成することをお勧めします。
2. プラットフォームには、一般的なニーズに対応するために、[ItemsRepeater](../controls-and-patterns/items-repeater.md#change-the-layout-of-items) と [LayoutPanel](/uwp/api/microsoft.ui.xaml.controls.layoutpanel) と連携する、一連の接続されているレイアウトが用意されています。  カスタム レイアウトを定義する必要があると決定する前に、それらについて理解してください。
3. 仮想化レイアウトでは、非仮想化レイアウトと比較して、CPU とメモリのコスト/複雑さ/オーバーヘッドが常にいくらか増加します。  一般的な経験則として、レイアウトで管理する必要がある子が、ビューポートのサイズの 3 倍の領域に収まる場合、仮想化レイアウトによる利益はあまり大きくない可能性があります。 3 倍のサイズについては、このドキュメントの後で詳しく説明しますが、Windows でのスクロールの非同期的性質と、仮想化へのその影響によるものです。

> [!TIP]
> 評価基準として、[ListView](/uwp/api/windows.ui.xaml.controls.listview) (および [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)) の既定の設定は、項目の数が現在のビューポートの 3 倍のサイズを満たすまで、リサイクルが開始されないことです。

**基本型の選択**

![接続されているレイアウト階層](images/xaml-attached-layout-hierarchy.png)

基本 [Layout](/uwp/api/microsoft.ui.xaml.controls.layout) 型には、接続されているレイアウトを作成するための開始点として使用できる 2 つの派生型があります。

1. [NonVirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout)
2. [VirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout)

## <a name="non-virtualizing-layout"></a>非仮想化レイアウト

非仮想化レイアウトを作成する方法は、[カスタム パネル](./custom-panels-overview.md)を作成したことがある誰でもなじみがあると感じるはずです。  同じ概念が適用されます。  主な違いは、[NonVirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayoutcontext) を使用して、[Children](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayoutcontext.children) コレクションにアクセスすることと、レイアウトで状態を格納するように選択できることです。

1. 基本型 [NonVirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout) (パネルではなく) から派生します。
2. *(省略可能)* 変更されるとレイアウトを無効にする依存関係プロパティを定義します。
3. _(**新規**/省略可能)_ レイアウトに必要な状態オブジェクトを [InitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.initializeforcontextcore) の一部として初期化します。 コンテキストによって提供される [LayoutState](/uwp/api/microsoft.ui.xaml.controls.layoutcontext.layoutstate) を使用して、ホスト コンテナーでそれを一時退避します。
4. すべての子で [MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout.measureoverride) をオーバーライドし、[Measure](/uwp/api/windows.ui.xaml.uielement.measure) メソッドを呼び出します。
5. すべての子で [ArrangeOverride](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout.arrangeoverride) をオーバーライドし、[Arrange](/uwp/api/windows.ui.xaml.uielement.arrange) メソッドを呼び出します。
6. *(**新規**/省略可能)* 保存された状態を [UninitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.uninitializeforcontextcore) の一部としてクリーンアップします。

### <a name="example-a-simple-stack-layout-varying-sized-items"></a>例:シンプルなスタック レイアウト (さまざまなサイズの項目)

![MyStackLayout](images/xaml-attached-layout-mystacklayout.png)

次に、さまざまなサイズの項目のきわめて基本的な非仮想化スタック レイアウトを示します。 これには、レイアウトの動作を調整するためのプロパティがありません。 以下の実装では、レイアウトが、次の操作のために、コンテナーによって提供されるコンテキスト オブジェクトにどのように依存しているかを示しています。

1. 子の数を取得する、および
2. インデックスによって各子要素にアクセスする。

```csharp
public class MyStackLayout : NonVirtualizingLayout
{
    protected override Size MeasureOverride(NonVirtualizingLayoutContext context, Size availableSize)
    {
        double extentHeight = 0.0;
        foreach (var element in context.Children)
        {
            element.Measure(availableSize);
            extentHeight += element.DesiredSize.Height;
        }

        return new Size(availableSize.Width, extentHeight);
    }

    protected override Size ArrangeOverride(NonVirtualizingLayoutContext context, Size finalSize)
    {
        double offset = 0.0;
        foreach (var element in context.Children)
        {
            element.Arrange(
                new Rect(0, offset, finalSize.Width, element.DesiredSize.Height));
            offset += element.DesiredSize.Height;
        }

        return finalSize;
    }
}
```

```xaml
 <LayoutPanel MaxWidth="196">
    <LayoutPanel.Layout>
        <local:MyStackLayout/>
    </LayoutPanel.Layout>

    <Button HorizontalAlignment="Stretch">1</Button>
    <Button HorizontalAlignment="Right">2</Button>
    <Button HorizontalAlignment="Center">3</Button>
    <Button>4</Button>

</LayoutPanel>
```

## <a name="virtualizing-layouts"></a>仮想化レイアウト

非仮想化レイアウトと同様に、仮想化レイアウトの大まかな手順は同じです。  複雑さは主に、どの要素がビューポート内に収まり、実現する必要があるかを判断することにあります。

1. 基本型 [VirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout) から派生します。
2. (省略可能) 変更されるとレイアウトを無効にする依存関係プロパティを定義します。
3. レイアウトに必要とされる状態オブジェクトを [InitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.initializeforcontextcore) の一部として初期化します。 コンテキストによって提供される [LayoutState](/uwp/api/microsoft.ui.xaml.controls.layoutcontext.layoutstate) を使用して、ホスト コンテナーでそれを一時退避します。
4. 実現する必要がある各子について、[MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.measureoverride) をオーバーライドし、[Measure](/uwp/api/windows.ui.xaml.uielement.measure) メソッドを呼び出します。
   1. [GetOrCreateElementAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat) メソッドを使用して、フレームワークによって準備された UIElement (適用されるデータ バインディングなど) を取得します。
5. 実現される各子について、[ArrangeOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.arrangeoverride) をオーバーライドし、[Arrange](/uwp/api/windows.ui.xaml.uielement.arrange) メソッドを呼び出します。
6. (省略可能) 保存された状態を [UninitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.uninitializeforcontextcore) の一部としてクリーンアップします。

> [!TIP]
> [MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout) によって返される値は、仮想化されたコンテンツのサイズとして使用されます。

仮想化レイアウトを作成する場合に、2 つの一般的な方法を検討する必要があります。  どちらを選択するかは、"要素のサイズをどれくらいに決定するか" に大きく依存します。  データ セット内の項目のインデックスがわかれば十分な場合、またはデータ自体によって、その最終的なサイズを決定する場合、それを **データ依存** とみなします。  これらはより簡単に作成できます。  ただし、項目のサイズを判断する唯一の方法が、UI を作成して測定することである場合、それを **コンテンツ依存** と呼ぶことになります。  これらはより複雑です。

### <a name="the-layout-process"></a>レイアウト プロセス

データ依存またはコンテンツ依存のどちらのレイアウトを作成する場合でも、レイアウト プロセスと Windows の非同期スクロールの影響を理解することが重要です。

起動から、画面に UI を表示するまで、フレームワークによって実行される手順をきわめて簡単に説明すると、

1. マークアップを解析します。

2. 要素のツリーを生成します。

3. レイアウト パスを実行します。

4. レンダー パスを実行します。

UI 仮想化では、手順 2 で通常行われる要素の作成は、後に回すか、またはビューポートを埋めるために十分なコンテンツが作成されたと判断されたら、早期に終了させます。 仮想化コンテナー (たとえば、ItemsRepeater) では、その接続されているレイアウトに従って、このプロセスを進めます。 これは、接続されているレイアウトに、仮想化レイアウトに必要な追加情報を表示する [VirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext) を提供します。

**RealizationRect (つまりビューポート)**

Windows でのスクロールは、UI スレッドに非同期で行われます。 フレームワークのレイアウトによって制御されません。  代わりに、システムのコンポジターで相互作用と移動が行われます。 このアプローチの利点は、コンテンツのパンが常に 60 fps で実行できることです。  ただし、レイアウトからわかるように "ビューポート" は、画面に実際に表示されているものと比べて若干古い場合があります。 ユーザーがすばやくスクロールすると、UI スレッドの速度を超えてしまい、新しいコンテンツや "真っ暗へのパン" が発生することがあります。 このため、仮想化レイアウトでは、多くの場合に、ビューポートよりも大きい領域を埋めるのに十分な準備済み要素の追加バッファーを生成する必要があります。 スクロール中に負荷が大きくなっても、ユーザーには引き続きコンテンツが表示されます。

![realization rect](images/xaml-attached-layout-realizationrect.png)

要素の作成にはコストがかかるため、仮想化コンテナー (たとえば、[ItemsRepeater](../controls-and-patterns/items-repeater.md)) では、最初に、接続されているレイアウトに、ビューポートに一致する [RealizationRect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect) が提供されます。 アイドル時に、コンテナーでは、徐々に大きい realization rect を使用して、レイアウトを繰り返し呼び出すことで、準備済みコンテンツのバッファーを大きくしていくことができます。 この動作は、高速の起動時間と優れたパン エクスペリエンスのバランスを取るように試みるパフォーマンス最適化です。 ItemsRepeater が生成する最大バッファー サイズは、その [VerticalCacheLength](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.verticalcachelength) プロパティと [HorizontalCacheLength](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.verticalcachelength) プロパティによって制御されます。

**要素の再利用 (リサイクル)**

レイアウトは、実行されるたびに [RealizationRect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect) を満たすように、要素のサイズと位置を設定することが期待されます。 既定で、[VirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout) によって、各レイアウト パスの最後で未使用の要素がリサイクルされます。

[MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.measureoverride) と [ArrangeOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.arrangeoverride) の一部として、レイアウトに渡される [VirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext) によって、仮想化レイアウトに必要な追加情報が提供されます。 それが提供する最も一般的に使用されるもののいくつかは、次を実行する機能です。

1. データの項目数を照会する ([ItemCount](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.itemcount))。
2. [GetItemAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getitemat) メソッドを使用して、特定の項目を取得する。
3. レイアウトが実現される要素で満たす必要があるビューポートとバッファーを表す [RealizationRect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect) を取得する。
4. [GetOrCreateElement](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat) メソッドによって、特定の項目の UIElement を要求する。

特定のインデックスの要素を要求すると、その要素は、レイアウトのそのパスに "使用中" とマークされます。 要素がまだ存在していない場合、それが実現され、自動的に使用の準備が行われます (たとえば、DataTemplate で定義されている UI ツリーの拡張、任意のデータ バインディングの処理など)。  そうでない場合、既存のインスタンスのプールから取得されます。

[GetOrCreateElementAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat) メソッドを使用して要素を取得したときに、[SuppressAutoRecycle](/uwp/api/microsoft.ui.xaml.controls.elementrealizationoptions) を実行するオプションが使用されていない限り、各メジャー パスの最後で、"使用中" とマークされていない既存の実現された要素は、再利用可能と見なされます。 フレームワークによって、それが自動的にリサイクル プールに移動され、使用できるようになります。 その後、別のコンテナーで使用するためにプルできます。 要素を再度ペアレンティングすることに関連するコストがあるため、フレームワークでは可能な限りこれを回避しようとします。

仮想化レイアウトで、各測定の開始時に、realization rect 内に収まらなくなる要素がわかっている場合、その再利用を最適化できます。 フレームワークの既定の動作に依存しません。 レイアウトでは、[RecycleElement](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recycleelement) メソッドを使用して、事前に要素をリサイクル プールに移動できます。  新しい要素を要求する前に、このメソッドを呼び出すと、後でレイアウトがまだ要素に関連付けられていないインデックスに対して、[GetOrCreateElementAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat) 要求を発行したときに、それらの既存の要素が使用できるようになります。

VirtualizingLayoutContext には、コンテンツ依存レイアウトを作成するレイアウト作成者向けに 2 つの追加のプロパティが用意されています。 それらについては、後で詳しく説明します。

1. レイアウトに省略可能な _入力_ を提供する [RecommendedAnchorIndex](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recommendedanchorindex)。
2. レイアウトの省略可能な _出力_ である [LayoutOrigin](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.layoutorigin)。

## <a name="data-dependent-virtualizing-layouts"></a>データ依存仮想化レイアウト

表示するコンテンツを測定しなくても、すべての項目のサイズがわかっている場合、仮想化レイアウトがより簡単になります。  この仮想化レイアウトのカテゴリには、通常データの検査が含まれるため、このドキュメントでは単に **データ レイアウト** と呼びます。  データに基づいて、アプリでは既知のサイズ (おそらくデータの一部であるか、以前に設計によって決定されているために) のビジュアル表現を選択できます。

一般的な方法として、レイアウトで次を実行します。

1. すべての項目のサイズと位置を計算します。
2. [MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.measureoverride) の一部として:
   1. [RealizationRect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect) を使用して、ビューポート内に表示する項目を決定します。
   2. [GetOrCreateElementAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat) メソッドを使用して、項目を表現する必要がある UIElement を取得します。
   3. 事前に計算されたサイズで UIElemen を[Measure (測定)](/uwp/api/windows.ui.xaml.uielement.measure) します。
3. [ArrangeOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.arrangeoverride) の一部として、事前に計算された位置で、実現される各 UIElement を[Arrange (配置)](/uwp/api/windows.ui.xaml.uielement.arrange) します。

> [!NOTE]
> データ レイアウト アプローチは、多くの場合に、_データ仮想化_ と互換性がありません。  特に、メモリに読み込まれるデータだけが、ユーザーに表示されるものを満たすために必要なデータである場合です。  データ仮想化は、そのデータが存在したままになっている場所をユーザーが下にスクロールしたときのデータの遅延または増分読み込みを指しているのではありません。  代わりに、スクロールして表示されなくなったときに、項目がメモリから解放されるタイミングを示します。  データ レイアウトの一部としてすべてのデータ項目を検査するデータ レイアウトを使用すると、データ仮想化が想定どおりに動作しなくなることがあります。  例外は、すべてのもののサイズが同じであると仮定する UniformGridLayout のようなレイアウトです。

> [!TIP]
> 多様な状況で他のユーザーによって使用されるコントロール ライブラリのカスタム コントロールを作成する場合、データ レイアウトは選択肢とならない可能性があります。

### <a name="example-xbox-activity-feed-layout"></a>例:Xbox アクティビティ フィード レイアウト

Xbox アクティビティ フィードの UI では繰り返しパターンが使用され、各行にワイド タイルが含まれ、その後に、後続の行で反転させられる 2 つのナロー タイルが続きます。 このレイアウトでは、すべての項目のサイズは、データ セット内の項目の位置と、タイルの既知のサイズ (ワイドとナロー) の関数になります。

![Xbox アクティビティ フィード](images/xaml-attached-layout-activityfeedscreenshot.png)

次のコードでは、アクティビティ フィードのカスタム仮想化 UI とはどのようなものかを説明し、**データ レイアウト** に採用できる一般的なアプローチを示します。

<table>
<td>
    <p><strong style="font-weight: semi-bold">XAML コントロール ギャラリー</strong> アプリがインストールされている場合は、ここをクリックしてアプリを開き、このサンプル レイアウトでの <a href="xamlcontrolsgallery:/item/ItemsRepeater">ItemsRepeater</a> の動作を参照してください。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML コントロール ギャラリー アプリを入手する (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">ソース コード (GitHub) を入手する</a></li>
    </ul>
</td>
</tr>
</table>

#### <a name="implementation"></a>実装

```csharp
/// <summary>
///  This is a custom layout that displays elements in two different sizes
///  wide (w) and narrow (n). There are two types of rows 
///  odd rows - narrow narrow wide
///  even rows - wide narrow narrow
///  This pattern repeats.
/// </summary>

public class ActivityFeedLayout : VirtualizingLayout // STEP #1 Inherit from base attached layout
{
    // STEP #2 - Parameterize the layout
    #region Layout parameters

    // We'll cache copies of the dependency properties to avoid calling GetValue during layout since that
    // can be quite expensive due to the number of times we'd end up calling these.
    private double _rowSpacing;
    private double _colSpacing;
    private Size _minItemSize = Size.Empty;

    /// <summary>
    /// Gets or sets the size of the whitespace gutter to include between rows
    /// </summary>
    public double RowSpacing
    {
        get { return _rowSpacing; }
        set { SetValue(RowSpacingProperty, value); }
    }

    /// <summary>
    /// Gets or sets the size of the whitespace gutter to include between items on the same row
    /// </summary>
    public double ColumnSpacing
    {
        get { return _colSpacing; }
        set { SetValue(ColumnSpacingProperty, value); }
    }

    public Size MinItemSize
    {
        get { return _minItemSize; }
        set { SetValue(MinItemSizeProperty, value); }
    }

    public static readonly DependencyProperty RowSpacingProperty =
        DependencyProperty.Register(
            nameof(RowSpacing),
            typeof(double),
            typeof(ActivityFeedLayout),
            new PropertyMetadata(0, OnPropertyChanged));

    public static readonly DependencyProperty ColumnSpacingProperty =
        DependencyProperty.Register(
            nameof(ColumnSpacing),
            typeof(double),
            typeof(ActivityFeedLayout),
            new PropertyMetadata(0, OnPropertyChanged));

    public static readonly DependencyProperty MinItemSizeProperty =
        DependencyProperty.Register(
            nameof(MinItemSize),
            typeof(Size),
            typeof(ActivityFeedLayout),
            new PropertyMetadata(Size.Empty, OnPropertyChanged));

    private static void OnPropertyChanged(DependencyObject obj, DependencyPropertyChangedEventArgs args)
    {
        var layout = obj as ActivityFeedLayout;
        if (args.Property == RowSpacingProperty)
        {
            layout._rowSpacing = (double)args.NewValue;
        }
        else if (args.Property == ColumnSpacingProperty)
        {
            layout._colSpacing = (double)args.NewValue;
        }
        else if (args.Property == MinItemSizeProperty)
        {
            layout._minItemSize = (Size)args.NewValue;
        }
        else
        {
            throw new InvalidOperationException("Don't know what you are talking about!");
        }

        layout.InvalidateMeasure();
    }

    #endregion

    #region Setup / teardown // STEP #3: Initialize state

    protected override void InitializeForContextCore(VirtualizingLayoutContext context)
    {
        base.InitializeForContextCore(context);

        var state = context.LayoutState as ActivityFeedLayoutState;
        if (state == null)
        {
            // Store any state we might need since (in theory) the layout could be in use by multiple
            // elements simultaneously
            // In reality for the Xbox Activity Feed there's probably only a single instance.
            context.LayoutState = new ActivityFeedLayoutState();
        }
    }

    protected override void UninitializeForContextCore(VirtualizingLayoutContext context)
    {
        base.UninitializeForContextCore(context);

        // clear any state
        context.LayoutState = null;
    }

    #endregion

    #region Layout // STEP #4,5 - Measure and Arrange

    protected override Size MeasureOverride(VirtualizingLayoutContext context, Size availableSize)
    {
        if (this.MinItemSize == Size.Empty)
        {
            var firstElement = context.GetOrCreateElementAt(0);
            firstElement.Measure(new Size(double.PositiveInfinity, double.PositiveInfinity));

            // setting the member value directly to skip invalidating layout
            this._minItemSize = firstElement.DesiredSize;
        }

        // Determine which rows need to be realized.  We know every row will have the same height and
        // only contain 3 items.  Use that to determine the index for the first and last item that
        // will be within that realization rect.
        var firstRowIndex = Math.Max(
            (int)(context.RealizationRect.Y / (this.MinItemSize.Height + this.RowSpacing)) - 1,
            0);
        var lastRowIndex = Math.Min(
            (int)(context.RealizationRect.Bottom / (this.MinItemSize.Height + this.RowSpacing)) + 1,
            (int)(context.ItemCount / 3));

        // Determine which items will appear on those rows and what the rect will be for each item
        var state = context.LayoutState as ActivityFeedLayoutState;
        state.LayoutRects.Clear();

        // Save the index of the first realized item.  We'll use it as a starting point during arrange.
        state.FirstRealizedIndex = firstRowIndex * 3;

        // ideal item width that will expand/shrink to fill available space
        double desiredItemWidth = Math.Max(this.MinItemSize.Width, (availableSize.Width - this.ColumnSpacing * 3) / 4);

        // Foreach item between the first and last index,
        //     Call GetElementOrCreateElementAt which causes an element to either be realized or retrieved
        //       from a recycle pool
        //     Measure the element using an appropriate size
        //
        // Any element that was previously realized which we don't retrieve in this pass (via a call to
        // GetElementOrCreateAt) will be automatically cleared and set aside for later re-use.
        // Note: While this work fine, it does mean that more elements than are required may be
        // created because it isn't until after our MeasureOverride completes that the unused elements
        // will be recycled and available to use.  We could avoid this by choosing to track the first/last
        // index from the previous layout pass.  The diff between the previous range and current range
        // would represent the elements that we can pre-emptively make available for re-use by calling
        // context.RecycleElement(element).
        for (int rowIndex = firstRowIndex; rowIndex < lastRowIndex; rowIndex++)
        {
            int firstItemIndex = rowIndex * 3;
            var boundsForCurrentRow = CalculateLayoutBoundsForRow(rowIndex, desiredItemWidth);

            for (int columnIndex = 0; columnIndex < 3; columnIndex++)
            {
                var index = firstItemIndex + columnIndex;
                var rect = boundsForCurrentRow[index % 3];
                var container = context.GetOrCreateElementAt(index);

                container.Measure(
                    new Size(boundsForCurrentRow[columnIndex].Width, boundsForCurrentRow[columnIndex].Height));

                state.LayoutRects.Add(boundsForCurrentRow[columnIndex]);
            }
        }

        // Calculate and return the size of all the content (realized or not) by figuring out
        // what the bottom/right position of the last item would be.
        var extentHeight = ((int)(context.ItemCount / 3) - 1) * (this.MinItemSize.Height + this.RowSpacing) + this.MinItemSize.Height;

        // Report this as the desired size for the layout
        return new Size(desiredItemWidth * 4 + this.ColumnSpacing * 2, extentHeight);
    }

    protected override Size ArrangeOverride(VirtualizingLayoutContext context, Size finalSize)
    {
        // walk through the cache of containers and arrange
        var state = context.LayoutState as ActivityFeedLayoutState;
        var virtualContext = context as VirtualizingLayoutContext;
        int currentIndex = state.FirstRealizedIndex;

        foreach (var arrangeRect in state.LayoutRects)
        {
            var container = virtualContext.GetOrCreateElementAt(currentIndex);
            container.Arrange(arrangeRect);
            currentIndex++;
        }

        return finalSize;
    }

    #endregion
    #region Helper methods

    private Rect[] CalculateLayoutBoundsForRow(int rowIndex, double desiredItemWidth)
    {
        var boundsForRow = new Rect[3];

        var yoffset = rowIndex * (this.MinItemSize.Height + this.RowSpacing);
        boundsForRow[0].Y = boundsForRow[1].Y = boundsForRow[2].Y = yoffset;
        boundsForRow[0].Height = boundsForRow[1].Height = boundsForRow[2].Height = this.MinItemSize.Height;

        if (rowIndex % 2 == 0)
        {
            // Left tile (narrow)
            boundsForRow[0].X = 0;
            boundsForRow[0].Width = desiredItemWidth;
            // Middle tile (narrow)
            boundsForRow[1].X = boundsForRow[0].Right + this.ColumnSpacing;
            boundsForRow[1].Width = desiredItemWidth;
            // Right tile (wide)
            boundsForRow[2].X = boundsForRow[1].Right + this.ColumnSpacing;
            boundsForRow[2].Width = desiredItemWidth * 2 + this.ColumnSpacing;
        }
        else
        {
            // Left tile (wide)
            boundsForRow[0].X = 0;
            boundsForRow[0].Width = (desiredItemWidth * 2 + this.ColumnSpacing);
            // Middle tile (narrow)
            boundsForRow[1].X = boundsForRow[0].Right + this.ColumnSpacing;
            boundsForRow[1].Width = desiredItemWidth;
            // Right tile (narrow)
            boundsForRow[2].X = boundsForRow[1].Right + this.ColumnSpacing;
            boundsForRow[2].Width = desiredItemWidth;
        }

        return boundsForRow;
    }

    #endregion
}

internal class ActivityFeedLayoutState
{
    public int FirstRealizedIndex { get; set; }

    /// <summary>
    /// List of layout bounds for items starting with the
    /// FirstRealizedIndex.
    /// </summary>
    public List<Rect> LayoutRects
    {
        get
        {
            if (_layoutRects == null)
            {
                _layoutRects = new List<Rect>();
            }

            return _layoutRects;
        }
    }

    private List<Rect> _layoutRects;
}
```

### <a name="optional-managing-the-item-to-uielement-mapping"></a>(省略可能) 項目と UIElement のマッピングの管理

既定で、[VirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext) は、実現される要素と、それらが表すデータ ソース内のインデックスとの間のマッピングを維持します。  レイアウトでは、既定の自動リサイクル動作を妨げる [GetOrCreateElementAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat) メソッドを使用して要素を取得する際に、[SuppressAutoRecycle](/uwp/api/microsoft.ui.xaml.controls.elementrealizationoptions) を実行するオプションを常に要求することによって、このマッピング自体を管理できます。  たとえば、スクロールが 1 方向に制限されていて、考慮される項目が常に連続している (つまり、最初の要素と最後の要素のインデックスを知っていれば、実現すべきすべての要素を知るのに十分である) ときにのみ使用される場合は、レイアウトでこれを行うように選択できます。

#### <a name="example-xbox-activity-feed-measure"></a>例:Xbox アクティビティ フィードの測定

次のスニペットは、マッピングを管理するために、前のサンプルの MeasureOverride に追加できる追加のロジックを示しています。

```csharp
    protected override Size MeasureOverride(VirtualizingLayoutContext context, Size availableSize)
    {
        //...

        // Determine which items will appear on those rows and what the rect will be for each item
        var state = context.LayoutState as ActivityFeedLayoutState;
        state.LayoutRects.Clear();

         // Recycle previously realized elements that we know we won't need so that they can be used to
        // fill in gaps without requiring us to realize additional elements.
        var newFirstRealizedIndex = firstRowIndex * 3;
        var newLastRealizedIndex = lastRowIndex * 3 + 3;
        for (int i = state.FirstRealizedIndex; i < newFirstRealizedIndex; i++)
        {
            context.RecycleElement(state.IndexToElementMap.Get(i));
            state.IndexToElementMap.Clear(i);
        }

        for (int i = state.LastRealizedIndex; i < newLastRealizedIndex; i++)
        {
            context.RecycleElement(context.IndexElementMap.Get(i));
            state.IndexToElementMap.Clear(i);
        }

        // ...

        // Foreach item between the first and last index,
        //     Call GetElementOrCreateElementAt which causes an element to either be realized or retrieved
        //       from a recycle pool
        //     Measure the element using an appropriate size
        //
        for (int rowIndex = firstRowIndex; rowIndex < lastRowIndex; rowIndex++)
        {
            int firstItemIndex = rowIndex * 3;
            var boundsForCurrentRow = CalculateLayoutBoundsForRow(rowIndex, desiredItemWidth);

            for (int columnIndex = 0; columnIndex < 3; columnIndex++)
            {
                var index = firstItemIndex + columnIndex;
                var rect = boundsForCurrentRow[index % 3];
                UIElement container = null;
                if (state.IndexToElementMap.Contains(index))
                {
                    container = state.IndexToElementMap.Get(index);
                }
                else
                {
                    container = context = context.GetOrCreateElementAt(index, ElementRealizationOptions.ForceCreate | ElementRealizationOptions.SuppressAutoRecycle);
                    state.IndexToElementMap.Add(index, container);
                }

                container.Measure(
                    new Size(boundsForCurrentRow[columnIndex].Width, boundsForCurrentRow[columnIndex].Height));

                state.LayoutRects.Add(boundsForCurrentRow[columnIndex]);
            }
        }

        // ...
   }

internal class ActivityFeedLayoutState
{
    // ...
    Dictionary<int, UIElement> IndexToElementMap { get; set; }
    // ...
}
```

## <a name="content-dependent-virtualizing-layouts"></a>コンテンツ依存仮想化レイアウト

最初に項目の UI コンテンツを測定して、その正確なサイズを算出する必要がある場合、それは **コンテンツ依存レイアウト** です。  また、レイアウトが項目にそのサイズを伝えるのではなく、各項目がそれ自体のサイズを測定する必要があるレイアウトと考えることもできます。 このカテゴリに分類される仮想化レイアウトは、さらに複雑になります。

> [!NOTE]
> コンテンツ依存レイアウトでは、データ仮想化が解除されることはありません (解除されないはずです)。

### <a name="estimations"></a>推定

コンテンツ依存レイアウトでは、推定に基づいて、実現されないコンテンツのサイズと実現されるコンテンツの位置の両方が推測されます。 これらの推定が変更されるたびに、実現されるコンテンツはスクロール可能な領域内で定期的に移動させられます。 これが軽減されない場合、きわめてイライラする不快なユーザー エクスペリエンスになります。 この潜在的な問題と軽減策について、ここで説明します。

> [!NOTE]
> すべての項目を考慮し、すべての項目の正確なサイズ、実現されるかどうか、それらの位置を把握しているデータ レイアウトでは、これらの問題を完全に回避できます。

**スクロール アンカー設定**

XAML には、[IScrollAnchorPovider](/uwp/api/windows.ui.xaml.controls.iscrollanchorprovider) インターフェイスを実装することによって、[スクロール アンカー設定](/uwp/api/windows.ui.xaml.controls.iscrollanchorprovider)をサポートするスクロール コントロールを使用して、急激なビューポートの移動を軽減するメカニズムがあります。 ユーザーがコンテンツを操作するたびに、スクロール コントロールによって、オプトインされた一連の候補から、追跡対象とされる要素が絶えず選択されます。 レイアウト中にアンカー要素の位置が移動した場合、スクロール コントロールはそのビューポートを自動的に移動して、ビューポートを維持します。

レイアウトに指定された [RecommendedAnchorIndex](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recommendedanchorindex) の値は、スクロール コントロールによって選択された、現在選択されているアンカー要素を反映している場合があります。 または、開発者が、[ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) で [GetOrCreateElement](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.getorcreateelement) メソッドを使用して、インデックスに対して要素が実現されるように明示的に要求した場合、そのインデックスは、次のレイアウト パスで [RecommendedAnchorIndex](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recommendedanchorindex) として指定されます。 これにより、開発者が要素を実現し、その後 [StartBringIntoView](/uwp/api/windows.ui.xaml.uielement.startbringintoview) メソッドによって、それがビューに表示されるように要求するという可能性のあるシナリオのために、レイアウトを準備できます。

[RecommendedAnchorIndex](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recommendedanchorindex) は、コンテンツ依存レイアウトで、その項目の位置を推定するときに最初に配置する必要がある、データソース内の項目のインデックスです。 それは、他の実現される項目を配置するための開始点として機能するはずです。

**スクロールバーへの影響**

スクロール アンカー設定を使用しても、コンテンツのサイズが大きく変化することなどの原因で、レイアウトの推定が著しく変化する場合、スクロールバーのつまみの位置が跳び回るように見えることがあります。  ユーザーがマウス ポインターをドラッグしているときに、つまみがポインターの位置を追跡しているように見えなければ、ユーザーが不快に感じる可能性があります。

レイアウトの推定の精度が高くなるほど、スクロールバーのつまみが跳び回って見える可能性が低くなります。

### <a name="layout-corrections"></a>レイアウトの修正

事実によってその推定を正当化するように、コンテンツ依存レイアウトを準備する必要があります。  たとえば、ユーザーがコンテンツの一番上までスクロールし、レイアウトが最初の要素を実現する場合、要素が始まっている要素に相対的な、要素の予想される位置によって、要素が (x:0, y:0) の原点以外の場所に表示されることがあります。 これが発生する場合、レイアウトでは [LayoutOrigin](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.layoutorigin) プロパティを使用して、新しいレイアウトの原点として計算した位置を設定できます。  最終結果はスクロール アンカー設定に似ており、スクロール コントロールのビューポートが、レイアウトによって報告されたコンテンツの位置を考慮して、自動的に調整されます。

![LayoutOrigin の修正](images/xaml-attached-layout-origincorrection.png)

### <a name="disconnected-viewports"></a>切断されたビューポート

レイアウトの [MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.measureoverride) メソッドから返されるサイズは、後続の各レイアウトで変更される可能性があるコンテンツのサイズについての最適な推測を表します。  ユーザーがスクロールするたびに、更新された [RealizationRect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect) によって、レイアウトが絶えず再評価されます。

ユーザーがきわめてすばやくドラッグすると、レイアウトの観点から、前の位置が現在の位置と重ならず、ビューポートが大きくジャンプするように見える可能性があります。  これは、スクロールの非同期性に原因があります。 また、レイアウトを使用しているアプリでは、現在実現されておらず、レイアウトによって追跡される現在の範囲外に配置されていると推定される項目に対して、要素を表示するように要求する可能性もあります。

レイアウトでその推測が正しくないことが検出された場合、または予期しないビューポートの移動が確認された場合は、その開始位置を再設定する必要があります。  XAML コントロールの一部として出荷される仮想化レイアウトは、表示されるコンテンツの性質に対する制限が少ないため、コンテンツ依存レイアウトとして開発されます。


### <a name="example-simple-virtualizing-stack-layout-for-variable-sized-items"></a>例:可変サイズの項目のシンプルな仮想化スタック レイアウト

次のサンプルでは、以下のような可変サイズの項目のシンプルなスタック レイアウトを示しています。

* UI の仮想化をサポートする、
* 推定を使用して、実現されない項目のサイズを推測する、
* 非連続的なビューポートの移動の可能性に対応する、および
* それらの移動を考慮して、レイアウトの修正を適用する。

**使用方法:マークアップ**

```xaml
<ScrollViewer>

  <ItemsRepeater x:Name="repeater" >
    <ItemsRepeater.Layout>

      <local:VirtualizingStackLayout />

    </ItemsRepeater.Layout>
    <ItemsRepeater.ItemTemplate>
      <DataTemplate x:Key="item">
        <UserControl IsTabStop="True" UseSystemFocusVisuals="True" Margin="5">
          <StackPanel BorderThickness="1" Background="LightGray" Margin="5">
            <Image x:Name="recipeImage" Source="{Binding ImageUri}"  Width="100" Height="100"/>
              <TextBlock x:Name="recipeDescription"
                         Text="{Binding Description}"
                         TextWrapping="Wrap"
                         Margin="10" />
          </StackPanel>
        </UserControl>
      </DataTemplate>
    </ItemsRepeater.ItemTemplate>
  </ItemsRepeater>

</ScrollViewer>
```

**分離コード:Main.cs**

```csharp
string _lorem = @"Lorem ipsum dolor sit amet, consectetur adipiscing elit. Etiam laoreet erat vel massa rutrum, eget mollis massa vulputate. Vivamus semper augue leo, eget faucibus nulla mattis nec. Donec scelerisque lacus at dui ultricies, eget auctor ipsum placerat. Integer aliquet libero sed nisi eleifend, nec rutrum arcu lacinia. Sed a sem et ante gravida congue sit amet ut augue. Donec quis pellentesque urna, non finibus metus. Proin sed ornare tellus. Lorem ipsum dolor sit amet, consectetur adipiscing elit. Etiam laoreet erat vel massa rutrum, eget mollis massa vulputate. Vivamus semper augue leo, eget faucibus nulla mattis nec. Donec scelerisque lacus at dui ultricies, eget auctor ipsum placerat. Integer aliquet libero sed nisi eleifend, nec rutrum arcu lacinia. Sed a sem et ante gravida congue sit amet ut augue. Donec quis pellentesque urna, non finibus metus. Proin sed ornare tellus.";

var rnd = new Random();
var data = new ObservableCollection<Recipe>(Enumerable.Range(0, 300).Select(k =>
               new Recipe
               {
                   ImageUri = new Uri(string.Format("ms-appx:///Images/recipe{0}.png", k % 8 + 1)),
                   Description = k + " - " + _lorem.Substring(0, rnd.Next(50, 350))
               }));

repeater.ItemsSource = data;
```

**コード:VirtualizingStackLayout.cs**

```csharp
// This is a sample layout that stacks elements one after
// the other where each item can be of variable height. This is
// also a virtualizing layout - we measure and arrange only elements
// that are in the viewport. Not measuring/arranging all elements means
// that we do not have the complete picture and need to estimate sometimes.
// For example the size of the layout (extent) is an estimation based on the
// average heights we have seen so far. Also, if you drag the mouse thumb
// and yank it quickly, then we estimate what goes in the new viewport.

// The layout caches the bounds of everything that are in the current viewport.
// During measure, we might get a suggested anchor (or start index), we use that
// index to start and layout the rest of the items in the viewport relative to that
// index. Note that since we are estimating, we can end up with negative origin when
// the viewport is somewhere in the middle of the extent. This is achieved by setting the
// LayoutOrigin property on the context. Once this is set, future viewport will account
// for the origin.
public class VirtualizingStackLayout : VirtualizingLayout
{
    // Estimation state
    List<double> m_estimationBuffer = Enumerable.Repeat(0d, 100).ToList();
    int m_numItemsUsedForEstimation = 0;
    double m_totalHeightForEstimation = 0;

    // State to keep track of realized bounds
    int m_firstRealizedDataIndex = 0;
    List<Rect> m_realizedElementBounds = new List<Rect>();

    Rect m_lastExtent = new Rect();

    protected override Size MeasureOverride(VirtualizingLayoutContext context, Size availableSize)
    {
        var viewport = context.RealizationRect;
        DebugTrace("MeasureOverride: Viewport " + viewport);

        // Remove bounds for elements that are now outside the viewport.
        // Proactive recycling elements means we can reuse it during this measure pass again.
        RemoveCachedBoundsOutsideViewport(viewport);

        // Find the index of the element to start laying out from - the anchor
        int startIndex = GetStartIndex(context, availableSize);

        // Measure and layout elements starting from the start index, forward and backward.
        Generate(context, availableSize, startIndex, forward:true);
        Generate(context, availableSize, startIndex, forward:false);

        // Estimate the extent size. Note that this can have a non 0 origin.
        m_lastExtent = EstimateExtent(context, availableSize);
        context.LayoutOrigin = new Point(m_lastExtent.X, m_lastExtent.Y);
        return new Size(m_lastExtent.Width, m_lastExtent.Height);
    }

    protected override Size ArrangeOverride(VirtualizingLayoutContext context, Size finalSize)
    {
        DebugTrace("ArrangeOverride: Viewport" + context.RealizationRect);
        for (int realizationIndex = 0; realizationIndex < m_realizedElementBounds.Count; realizationIndex++)
        {
            int currentDataIndex = m_firstRealizedDataIndex + realizationIndex;
            DebugTrace("Arranging " + currentDataIndex);

            // Arrange the child. If any alignment needs to be done, it
            // can be done here.
            var child = context.GetOrCreateElementAt(currentDataIndex);
            var arrangeBounds = m_realizedElementBounds[realizationIndex];
            arrangeBounds.X -= m_lastExtent.X;
            arrangeBounds.Y -= m_lastExtent.Y;
            child.Arrange(arrangeBounds);
        }

        return finalSize;
    }

    // The data collection has changed, since we are maintaining the bounds of elements
    // in the viewport, we will update the list to account for the collection change.
    protected override void OnItemsChangedCore(VirtualizingLayoutContext context, object source, NotifyCollectionChangedEventArgs args)
    {
        InvalidateMeasure();
        if (m_realizedElementBounds.Count > 0)
        {
            switch (args.Action)
            {
                case NotifyCollectionChangedAction.Add:
                    OnItemsAdded(args.NewStartingIndex, args.NewItems.Count);
                    break;
                case NotifyCollectionChangedAction.Replace:
                    OnItemsRemoved(args.OldStartingIndex, args.OldItems.Count);
                    OnItemsAdded(args.NewStartingIndex, args.NewItems.Count);
                    break;
                case NotifyCollectionChangedAction.Remove:
                    OnItemsRemoved(args.OldStartingIndex, args.OldItems.Count);
                    break;
                case NotifyCollectionChangedAction.Reset:
                    m_realizedElementBounds.Clear();
                    m_firstRealizedDataIndex = 0;
                    break;
                default:
                    throw new NotImplementedException();
            }
        }
    }

    // Figure out which index to use as the anchor and start laying out around it.
    private int GetStartIndex(VirtualizingLayoutContext context, Size availableSize)
    {
        int startDataIndex = -1;
        var recommendedAnchorIndex = context.RecommendedAnchorIndex;
        bool isSuggestedAnchorValid = recommendedAnchorIndex != -1;

        if (isSuggestedAnchorValid)
        {
            if (IsRealized(recommendedAnchorIndex))
            {
                startDataIndex = recommendedAnchorIndex;
            }
            else
            {
                ClearRealizedRange();
                startDataIndex = recommendedAnchorIndex;
            }
        }
        else
        {
            // Find the first realized element that is visible in the viewport.
            startDataIndex = GetFirstRealizedDataIndexInViewport(context.RealizationRect);
            if (startDataIndex < 0)
            {
                startDataIndex = EstimateIndexForViewport(context.RealizationRect, context.ItemCount);
                ClearRealizedRange();
            }
        }

        // We have an anchorIndex, realize and measure it and
        // figure out its bounds.
        if (startDataIndex != -1 & context.ItemCount > 0)
        {
            if (m_realizedElementBounds.Count == 0)
            {
                m_firstRealizedDataIndex = startDataIndex;
            }

            var newAnchor = EnsureRealized(startDataIndex);
            DebugTrace("Measuring start index " + startDataIndex);
            var desiredSize = MeasureElement(context, startDataIndex, availableSize);

            var bounds = new Rect(
                0,
                newAnchor ?
                    (m_totalHeightForEstimation / m_numItemsUsedForEstimation) * startDataIndex : GetCachedBoundsForDataIndex(startDataIndex).Y,
                availableSize.Width,
                desiredSize.Height);
            SetCachedBoundsForDataIndex(startDataIndex, bounds);
        }

        return startDataIndex;
    }


    private void Generate(VirtualizingLayoutContext context, Size availableSize, int anchorDataIndex, bool forward)
    {
        // Generate forward or backward from anchorIndex until we hit the end of the viewport
        int step = forward ? 1 : -1;
        int previousDataIndex = anchorDataIndex;
        int currentDataIndex = previousDataIndex + step;
        var viewport = context.RealizationRect;
        while (IsDataIndexValid(currentDataIndex, context.ItemCount) &&
            ShouldContinueFillingUpSpace(previousDataIndex, forward, viewport))
        {
            EnsureRealized(currentDataIndex);
            DebugTrace("Measuring " + currentDataIndex);
            var desiredSize = MeasureElement(context, currentDataIndex, availableSize);
            var previousBounds = GetCachedBoundsForDataIndex(previousDataIndex);
            Rect currentBounds = new Rect(0,
                                          forward ? previousBounds.Y + previousBounds.Height : previousBounds.Y - desiredSize.Height,
                                          availableSize.Width,
                                          desiredSize.Height);
            SetCachedBoundsForDataIndex(currentDataIndex, currentBounds);
            previousDataIndex = currentDataIndex;
            currentDataIndex += step;
        }
    }

    // Remove bounds that are outside the viewport, leaving one extra since our
    // generate stops after generating one extra to know that we are outside the
    // viewport.
    private void RemoveCachedBoundsOutsideViewport(Rect viewport)
    {
        int firstRealizedIndexInViewport = 0;
        while (firstRealizedIndexInViewport < m_realizedElementBounds.Count &&
               !Intersects(m_realizedElementBounds[firstRealizedIndexInViewport], viewport))
        {
            firstRealizedIndexInViewport++;
        }

        int lastRealizedIndexInViewport = m_realizedElementBounds.Count - 1;
        while (lastRealizedIndexInViewport >= 0 &&
            !Intersects(m_realizedElementBounds[lastRealizedIndexInViewport], viewport))
        {
            lastRealizedIndexInViewport--;
        }

        if (firstRealizedIndexInViewport > 0)
        {
            m_firstRealizedDataIndex += firstRealizedIndexInViewport;
            m_realizedElementBounds.RemoveRange(0, firstRealizedIndexInViewport);
        }

        if (lastRealizedIndexInViewport >= 0 && lastRealizedIndexInViewport < m_realizedElementBounds.Count - 2)
        {
            m_realizedElementBounds.RemoveRange(lastRealizedIndexInViewport + 2, m_realizedElementBounds.Count - lastRealizedIndexInViewport - 3);
        }
    }

    private bool Intersects(Rect bounds, Rect viewport)
    {
        return !(bounds.Bottom < viewport.Top ||
            bounds.Top > viewport.Bottom);
    }

    private bool ShouldContinueFillingUpSpace(int dataIndex, bool forward, Rect viewport)
    {
        var bounds = GetCachedBoundsForDataIndex(dataIndex);
        return forward ?
            bounds.Y < viewport.Bottom :
            bounds.Y > viewport.Top;
    }

    private bool IsDataIndexValid(int currentDataIndex, int itemCount)
    {
        return currentDataIndex >= 0 && currentDataIndex < itemCount;
    }

    private int EstimateIndexForViewport(Rect viewport, int dataCount)
    {
        double averageHeight = m_totalHeightForEstimation / m_numItemsUsedForEstimation;
        int estimatedIndex = (int)(viewport.Top / averageHeight);
        // clamp to an index within the collection
        estimatedIndex = Math.Max(0, Math.Min(estimatedIndex, dataCount));
        return estimatedIndex;
    }

    private int GetFirstRealizedDataIndexInViewport(Rect viewport)
    {
        int index = -1;
        if (m_realizedElementBounds.Count > 0)
        {
            for (int i = 0; i < m_realizedElementBounds.Count; i++)
            {
                if (m_realizedElementBounds[i].Y < viewport.Bottom &&
                   m_realizedElementBounds[i].Bottom > viewport.Top)
                {
                    index = m_firstRealizedDataIndex + i;
                    break;
                }
            }
        }

        return index;
    }

    private Size MeasureElement(VirtualizingLayoutContext context, int index, Size availableSize)
    {
        var child = context.GetOrCreateElementAt(index);
        child.Measure(availableSize);

        int estimationBufferIndex = index % m_estimationBuffer.Count;
        bool alreadyMeasured = m_estimationBuffer[estimationBufferIndex] != 0;
        if (!alreadyMeasured)
        {
            m_numItemsUsedForEstimation++;
        }

        m_totalHeightForEstimation -= m_estimationBuffer[estimationBufferIndex];
        m_totalHeightForEstimation += child.DesiredSize.Height;
        m_estimationBuffer[estimationBufferIndex] = child.DesiredSize.Height;

        return child.DesiredSize;
    }

    private bool EnsureRealized(int dataIndex)
    {
        if (!IsRealized(dataIndex))
        {
            int realizationIndex = RealizationIndex(dataIndex);
            Debug.Assert(dataIndex == m_firstRealizedDataIndex - 1 ||
                dataIndex == m_firstRealizedDataIndex + m_realizedElementBounds.Count ||
                m_realizedElementBounds.Count == 0);

            if (realizationIndex == -1)
            {
                m_realizedElementBounds.Insert(0, new Rect());
            }
            else
            {
                m_realizedElementBounds.Add(new Rect());
            }

            if (m_firstRealizedDataIndex > dataIndex)
            {
                m_firstRealizedDataIndex = dataIndex;
            }

            return true;
        }

        return false;
    }

    // Figure out the extent of the layout by getting the number of items remaining
    // above and below the realized elements and getting an estimation based on
    // average item heights seen so far.
    private Rect EstimateExtent(VirtualizingLayoutContext context, Size availableSize)
    {
        double averageHeight = m_totalHeightForEstimation / m_numItemsUsedForEstimation;

        Rect extent = new Rect(0, 0, availableSize.Width, context.ItemCount * averageHeight);

        if (context.ItemCount > 0 && m_realizedElementBounds.Count > 0)
        {
            extent.Y = m_firstRealizedDataIndex == 0 ?
                            m_realizedElementBounds[0].Y :
                            m_realizedElementBounds[0].Y - (m_firstRealizedDataIndex - 1) * averageHeight;

            int lastRealizedIndex = m_firstRealizedDataIndex + m_realizedElementBounds.Count;
            if (lastRealizedIndex == context.ItemCount - 1)
            {
                var lastBounds = m_realizedElementBounds[m_realizedElementBounds.Count - 1];
                extent.Y = lastBounds.Bottom;
            }
            else
            {
                var lastBounds = m_realizedElementBounds[m_realizedElementBounds.Count - 1];
                int lastRealizedDataIndex = m_firstRealizedDataIndex + m_realizedElementBounds.Count;
                int numItemsAfterLastRealizedIndex = context.ItemCount - lastRealizedDataIndex;
                extent.Height = lastBounds.Bottom + numItemsAfterLastRealizedIndex * averageHeight - extent.Y;
            }
        }

        DebugTrace("Extent " + extent + " with average height " + averageHeight);
        return extent;
    }

    private bool IsRealized(int dataIndex)
    {
        int realizationIndex = dataIndex - m_firstRealizedDataIndex;
        return realizationIndex >= 0 && realizationIndex < m_realizedElementBounds.Count;
    }

    // Index in the m_realizedElementBounds collection
    private int RealizationIndex(int dataIndex)
    {
        return dataIndex - m_firstRealizedDataIndex;
    }

    private void OnItemsAdded(int index, int count)
    {
        // Using the old indexes here (before it was updated by the collection change)
        // if the insert data index is between the first and last realized data index, we need
        // to insert items.
        int lastRealizedDataIndex = m_firstRealizedDataIndex + m_realizedElementBounds.Count - 1;
        int newStartingIndex = index;
        if (newStartingIndex > m_firstRealizedDataIndex &&
            newStartingIndex <= lastRealizedDataIndex)
        {
            // Inserted within the realized range
            int insertRangeStartIndex = newStartingIndex - m_firstRealizedDataIndex;
            for (int i = 0; i < count; i++)
            {
                // Insert null (sentinel) here instead of an element, that way we do not
                // end up creating a lot of elements only to be thrown out in the next layout.
                int insertRangeIndex = insertRangeStartIndex + i;
                int dataIndex = newStartingIndex + i;
                // This is to keep the contiguousness of the mapping
                m_realizedElementBounds.Insert(insertRangeIndex, new Rect());
            }
        }
        else if (index <= m_firstRealizedDataIndex)
        {
            // Items were inserted before the realized range.
            // We need to update m_firstRealizedDataIndex;
            m_firstRealizedDataIndex += count;
        }
    }

    private void OnItemsRemoved(int index, int count)
    {
        int lastRealizedDataIndex = m_firstRealizedDataIndex + m_realizedElementBounds.Count - 1;
        int startIndex = Math.Max(m_firstRealizedDataIndex, index);
        int endIndex = Math.Min(lastRealizedDataIndex, index + count - 1);
        bool removeAffectsFirstRealizedDataIndex = (index <= m_firstRealizedDataIndex);

        if (endIndex >= startIndex)
        {
            ClearRealizedRange(RealizationIndex(startIndex), endIndex - startIndex + 1);
        }

        if (removeAffectsFirstRealizedDataIndex &&
            m_firstRealizedDataIndex != -1)
        {
            m_firstRealizedDataIndex -= count;
        }
    }

    private void ClearRealizedRange(int startRealizedIndex, int count)
    {
        m_realizedElementBounds.RemoveRange(startRealizedIndex, count);
        if (startRealizedIndex == 0)
        {
            m_firstRealizedDataIndex = m_realizedElementBounds.Count == 0 ? 0 : m_firstRealizedDataIndex + count;
        }
    }

    private void ClearRealizedRange()
    {
        m_realizedElementBounds.Clear();
        m_firstRealizedDataIndex = 0;
    }

    private Rect GetCachedBoundsForDataIndex(int dataIndex)
    {
        return m_realizedElementBounds[RealizationIndex(dataIndex)];
    }

    private void SetCachedBoundsForDataIndex(int dataIndex, Rect bounds)
    {
        m_realizedElementBounds[RealizationIndex(dataIndex)] = bounds;
    }

    private Rect GetCachedBoundsForRealizationIndex(int relativeIndex)
    {
        return m_realizedElementBounds[relativeIndex];
    }

    void DebugTrace(string message, params object[] args)
    {
        Debug.WriteLine(message, args);
    }
}
```

## <a name="related-articles"></a>関連記事

- [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)
- [ScrollViewer](/uwp/api/windows.ui.xaml.controls.scrollviewer)
