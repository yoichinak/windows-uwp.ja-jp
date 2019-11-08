---
Description: ItemsRepeater コントロールなどのコンテナーで使用するために、添付されたレイアウトを定義できます。
title: AttachedLayout
label: AttachedLayout
template: detail.hbs
ms.date: 03/13/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: dc23e86f85c5db3dd10c5cec152047be387d4513
ms.sourcegitcommit: 445320ff0ee7323d823194d4ec9cfa6e710ed85d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/11/2019
ms.locfileid: "72282291"
---
# <a name="attached-layouts"></a>添付されたレイアウト

レイアウトロジックを別のオブジェクトにデリゲートするコンテナー (パネルなど) は、添付されたレイアウトオブジェクトに依存して、子要素のレイアウト動作を提供します。  アタッチされたレイアウトモデルでは、アプリケーションが実行時に項目のレイアウトを変更する柔軟性が提供されます。また、UI のさまざまな部分 (たとえば、列内に配置されているように見えるテーブルの行の項目など) 間でレイアウトの側面を簡単に共有することができます。

このトピックでは、アタッチされたレイアウト (仮想化と非仮想化)、理解する必要がある概念とクラス、およびそれらを決定する際に考慮する必要があるトレードオフについて説明します。

| **Windows UI ライブラリを入手する** |
| - |
| このコントロールは、Windows UI ライブラリの NuGet パッケージの一部として組み込まれており、パッケージには、UWP アプリの新しいコントロールと UI 機能が含まれています。 インストール手順などの詳細については、[Windows UI ライブラリの概要](https://docs.microsoft.com/uwp/toolkits/winui/)に関するページを参照してください。 |

> **重要な API**:

> * [ScrollViewer](/uwp/api/windows.ui.xaml.controls.scrollviewer)
> * [ItemsRepeater](/windows/uwp/design/controls-and-patterns/items-repeater)
> * [レイアウト](/uwp/api/microsoft.ui.xaml.controls.layout)
>     * [非 Virtualizinglayout](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout)
>     * [VirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout)
> * [LayoutContext](/uwp/api/microsoft.ui.xaml.controls.layoutcontext)
>     * [NonVirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayoutcontext)
>     * [VirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext)
> * [LayoutPanel](/uwp/api/microsoft.ui.xaml.controls.layoutpanel) (プレビュー)

## <a name="key-concepts"></a>主要概念

レイアウトを実行するには、すべての要素に対して2つの質問に回答する必要があります。

1. この要素の***サイズ***を指定してください。

2. この要素の***位置***はどのようなものですか。

これらの質問に答える XAML のレイアウトシステムは、[カスタムパネル](/windows/uwp/design/layout/custom-panels-overview)の説明の一部として簡単に説明されています。

### <a name="containers-and-context"></a>コンテナーとコンテキスト

概念的には、XAML の[パネル](/uwp/api/windows.ui.xaml.controls.panel)には、フレームワークの2つの重要なロールがあります。

1. 子要素を含むことができ、要素のツリーに分岐が導入されます。
2. これらの子には、特定のレイアウト戦略が適用されます。

このため、XAML のパネルはレイアウトと同義であることがよくありますが、技術的に言えば、レイアウトだけではありません。

また、 [Itemsrepeater](/windows/uwp/design/controls-and-patterns/items-repeater)はパネルのように動作しますが、パネルとは異なり、プログラムで UIElement の子を追加または削除できる子プロパティを公開しません。  代わりに、子の有効期間は、データ項目のコレクションに対応するように、フレームワークによって自動的に管理されます。  パネルからは派生していませんが、動作し、パネルのようなフレームワークによって処理されます。

> [!NOTE]
> [LayoutPanel](/uwp/api/microsoft.ui.xaml.controls.layoutpanel)は、Panel から派生したコンテナーであり、関連付けられている[レイアウト](/uwp/api/microsoft.ui.xaml.controls.layoutpanel.layout)オブジェクトにロジックを委任します。  LayoutPanel は*プレビュー*段階であり、現在、WinUI パッケージの*プレリリース版*でのみ使用できます。

#### <a name="containers"></a>コンテナー

概念的には、[パネル](/uwp/api/windows.ui.xaml.controls.panel)は要素のコンテナーであり、[背景](/uwp/api/windows.ui.xaml.controls.panel.background)のピクセルをレンダリングする機能も備えています。  パネルには、一般的なレイアウトロジックを使いやすいパッケージにカプセル化する方法が用意されています。

**添付レイアウト**の概念により、コンテナーとレイアウトの2つのロールがより明確に区別されます。  コンテナーがレイアウトロジックを別のオブジェクトにデリゲートする場合は、次のスニペットに示すように、そのオブジェクトを、添付されたレイアウトで呼び出します。 LayoutPanel など、 [FrameworkElement](/uwp/api/windows.ui.xaml.frameworkelement)から継承されたコンテナーは、XAML のレイアウト処理 (たとえば、高さや幅) に入力を提供する共通プロパティを自動的に公開します。

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

レイアウト処理中、コンテナーは、アタッチされた*UniformGridLayout*に依存して子を測定し、配置します。

#### <a name="per-container-state"></a>コンテナーごとの状態

レイアウトが添付されている場合は、次のスニペットのように、レイアウトオブジェクトの1つのインスタンスを*多数*のコンテナーに関連付けることができます。そのため、ホストコンテナーに依存したり、直接参照したりすることはできません。  以下に例を示します。

```xaml
<!-- ... --->
<Page.Resources>
    <ExampleLayout x:Name="exampleLayout"/>
<Page.Resources>

<LayoutPanel x:Name="example1" Layout="{StaticResource exampleLayout}"/>
<LayoutPanel x:Name="example2" Layout="{StaticResource exampleLayout}"/>
<!-- ... --->
```

このような状況では、 *ExampleLayout*は、レイアウトの計算で使用する状態と、その状態が格納される場所を慎重に考慮して、一方のパネルの要素のレイアウトには影響を及ぼさないようにする必要があります。  MeasureOverride ロジックと ArrangeOverride ロジックが*静的*プロパティの値に依存するカスタムパネルに似ています。

#### <a name="layoutcontext"></a>LayoutContext

[Layoutcontext](/uwp/api/microsoft.ui.xaml.controls.layoutcontext)の目的は、これらの課題に対処することです。  これにより、アタッチされたレイアウトは、2つの間に直接的な依存関係を導入せずに、子要素の取得などのホストコンテナーと対話する機能を提供します。 また、コンテキストを使用すると、コンテナーの子要素に関連する可能性のある任意の状態をレイアウトで格納できます。

単純な非仮想化レイアウトでは、状態を維持する必要がなく、問題が発生しないことがよくあります。 ただし、グリッドなどのより複雑なレイアウトでは、値の再計算を避けるために、メジャーと整列呼び出しの間の状態を維持することができます。

仮想化レイアウトでは、*多くの場合*、メジャーと配置の間、および反復的なレイアウトパスの間で一定の状態を維持する必要があります。

#### <a name="initializing-and-uninitializing-per-container-state"></a>コンテナーごとの状態の初期化と初期化解除

レイアウトがコンテナーにアタッチされると、その[Initializeforcontextcore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.initializeforcontextcore)メソッドが呼び出され、状態を格納するためにオブジェクトを初期化する機会が提供されます。

同様に、レイアウトがコンテナーから削除されると、 [Uninitializeforcontextcore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.uninitializeforcontextcore)メソッドが呼び出されます。  これにより、そのコンテナーに関連付けられた状態をクリーンアップする機会がレイアウトに与えられます。

レイアウトの状態オブジェクトは、コンテキストの[Layoutstate](/uwp/api/microsoft.ui.xaml.controls.layoutcontext.layoutstate)プロパティを使用して格納し、コンテナーから取得できます。

### <a name="ui-virtualization"></a>UI の仮想化

UI の仮想化とは、_必要に_なるまで ui オブジェクトの作成を遅らせることを意味します。  これはパフォーマンスの最適化です。  スクロール以外のシナリオでは、_必要に応じ_て、アプリ固有の任意の数に基づいて決定できます。  そのような場合は、アプリで[x:Load](../../xaml-platform/x-load-attribute.md)の使用を検討する必要があります。 レイアウトに特別な処理は必要ありません。

リストなどのスクロールベースのシナリオでは、_必要に応じ_て "ユーザーに表示されるかどうか" を決定します。これは、レイアウトプロセス中の配置場所に大きく依存し、特別な考慮が必要です。  このドキュメントでは、このシナリオを中心に説明します。

> [!NOTE]
> このドキュメントでは説明しませんが、スクロールシナリオで UI 仮想化を有効にするのと同じ機能を、スクロール以外のシナリオでも適用できます。  たとえば、表示されている領域とオーバーフローメニューの間で要素をリサイクルまたは移動することによって、表示されるコマンドの有効期間を管理し、使用可能な領域の変化に応答する、データドリブンのツールバーコントロール。

## <a name="getting-started"></a>作業の開始

まず、作成する必要があるレイアウトで UI 仮想化をサポートする必要があるかどうかを決定します。

**注意すべき点がいくつかあります。**

1. 非仮想化レイアウトは、簡単に作成できます。 項目の数が常に小さい場合は、仮想化されていないレイアウトを作成することをお勧めします。
2. プラットフォームには、一般的なニーズに対応するために、 [Itemsrepeater](/windows/uwp/design/controls-and-patterns/items-repeater#change-the-layout-of-items)および[LayoutPanel](/uwp/api/microsoft.ui.xaml.controls.layoutpanel)と連携する一連の添付レイアウトが用意されています。  カスタムレイアウトを定義する必要があることを判断する前に、それらについて理解しておいてください。
3. レイアウトの仮想化では、仮想化されていないレイアウトと比較して、CPU とメモリのコスト/複雑さ/オーバーヘッドが常に増加します。  一般的な経験則として、レイアウトを管理する必要がある子が、ビューポートのサイズの3倍の領域に収まっている場合、仮想化レイアウトがあまり大きくない可能性があります。 3倍のサイズについては、このドキュメントで後ほど詳しく説明しますが、Windows でのスクロールの非同期の性質と、仮想化への影響によるものです。

> [!TIP]
> 参照のポイントとして、 [ListView](/uwp/api/windows.ui.xaml.controls.listview) (および[itemsrepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)) の既定の設定では、項目の数が現在のビューポートのサイズの3倍になるまで、リサイクルは開始されません。

**基本データ型の選択**

![アタッチされたレイアウト階層](images/xaml-attached-layout-hierarchy.png)

基本[レイアウト](/uwp/api/microsoft.ui.xaml.controls.layout)の型には、添付されたレイアウトを作成するための開始点として機能する2つの派生型があります。

1. [非 Virtualizinglayout](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout)
2. [VirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout)

## <a name="non-virtualizing-layout"></a>非仮想化レイアウト

仮想化されていないレイアウトを作成する方法は、[カスタムパネル](/windows/uwp/design/layout/custom-panels-overview)を作成したユーザーになじみのあるものにする必要があります。  同じ概念が適用されます。  主な違いは、[非 Virtualizinglayoutcontext](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayoutcontext)を使用して[子](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayoutcontext.children)コレクションにアクセスすることです。レイアウトでは、状態を保存することができます。

1. (パネルではなく) 基本型の[Nonvirtualizinglayout](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout)から派生します。
2. *(省略可能)* 変更時にレイアウトを無効にする依存関係プロパティを定義します。
3. _(**新規**/省略可能)_ [Initializeforcontextcore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.initializeforcontextcore)の一部として、レイアウトに必要な状態オブジェクトを初期化します。 コンテキストで提供される[Layoutstate](/uwp/api/microsoft.ui.xaml.controls.layoutcontext.layoutstate)を使用して、ホストコンテナーでそのファイルを一時退避します。
4. [Measureoverride](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout.measureoverride)をオーバーライドし、すべての子で[Measure](/uwp/api/windows.ui.xaml.uielement.measure)メソッドを呼び出します。
5. 並べ替え[Eoverride](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout.arrangeoverride)をオーバーライドし、すべての子の[配置](/uwp/api/windows.ui.xaml.uielement.arrange)メソッドを呼び出します。
6. *(**新規**/省略可能)* [Uninitializeforcontextcore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.uninitializeforcontextcore)の一部として保存された状態をクリーンアップします。

### <a name="example-a-simple-stack-layout-varying-sized-items"></a>例:単純なスタックレイアウト (サイズの異なる項目)

![MyStackLayout](images/xaml-attached-layout-mystacklayout.png)

さまざまなサイズの項目の仮想化されていない基本的なスタックレイアウトを次に示します。 レイアウトの動作を調整するためのプロパティがありません。 次の実装では、レイアウトがコンテナーによって提供されるコンテキストオブジェクトにどのように依存しているかを示します。

1. 子の数を取得します。
2. 各子要素にインデックスでアクセスします。

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

## <a name="virtualizing-layouts"></a>レイアウトの仮想化

仮想化以外のレイアウトと同様に、仮想化レイアウトの大まかな手順は同じです。  複雑さは、主にビューポート内でどの要素が使用されるかを決定することであり、実現する必要があります。

1. 基本型の[Virtualizinglayout](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout)から派生します。
2. Optional変更するとレイアウトが無効になる依存関係プロパティを定義します。
3. [Initializeforcontextcore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.initializeforcontextcore)の一部としてレイアウトで必要となる状態オブジェクトを初期化します。 コンテキストで提供される[Layoutstate](/uwp/api/microsoft.ui.xaml.controls.layoutcontext.layoutstate)を使用して、ホストコンテナーでそのファイルを一時退避します。
4. [Measureoverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.measureoverride)をオーバーライドし、実現する必要のある各子に対して[Measure](/uwp/api/windows.ui.xaml.uielement.measure)メソッドを呼び出します。
   1. [Getorcreateelementat](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat)メソッドは、フレームワークによって準備された UIElement (たとえば、適用されたデータバインディング) を取得するために使用されます。
5. 並べ替え[Eoverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.arrangeoverride)をオーバーライドし、実現された各子の[配置](/uwp/api/windows.ui.xaml.uielement.arrange)メソッドを呼び出します。
6. Optional[Uninitializeforcontextcore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.uninitializeforcontextcore)の一部として保存された状態をクリーンアップします。

> [!TIP]
> [Measureoverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout)によって返される値は、仮想化されたコンテンツのサイズとして使用されます。

仮想化レイアウトを作成するときは、2つの一般的な方法を検討する必要があります。  どちらを選択するかは、「要素のサイズをどのように決定するか」に大きく依存します。  データセット内の項目のインデックスを把握しておく必要がある場合、またはデータ自体が最終的なサイズを決定する場合は、**データに依存**していると考えます。  これらはより簡単に作成できます。  ただし、項目のサイズを判断する唯一の方法は、UI を作成して測定することです。次に、**コンテンツに依存**しているとします。  これらはより複雑です。

### <a name="the-layout-process"></a>レイアウトプロセス

データやコンテンツに依存するレイアウトのどちらを作成する場合でも、レイアウトプロセスと Windows の非同期スクロールの影響について理解することが重要です。

画面に表示される UI を起動するためにフレームワークによって実行される手順を次に示します。

1. マークアップを解析します。

2. 要素のツリーを生成します。

3. レイアウトパスを実行します。

4. レンダーパスを実行します。

UI の仮想化を使用すると、手順2で通常実行される要素の作成は遅延されるか、または、ビューポートを埋めるために十分なコンテンツが作成されたことを確認した後に早く終了します。 仮想化コンテナー (たとえば、ItemsRepeater) は、このプロセスを実行するために、添付されたレイアウトに従います。 これは、仮想化レイアウトに必要な追加情報を表示する[Virtualizinglayoutcontext](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext)を使用して、アタッチされたレイアウトを提供します。

**RealizationRect (ビューポート)**

Windows でのスクロールは、UI スレッドに対して非同期に行われます。 フレームワークのレイアウトによって制御されることはありません。  代わりに、システムのコンポジターで相互作用と移動が発生します。 このアプローチの利点は、パンコンテンツを常に 60 fps で実行できることです。  ただし、レイアウトに示されているような "ビューポート" は、画面に実際に表示されている内容に比べて若干古い場合があります。 ユーザーは、すばやくスクロールすると、UI スレッドの速度を追い越し始めて、新しいコンテンツや "black to black" を生成することができます。 このため、仮想化レイアウトでは、ビューポートよりも大きい領域を埋めるのに十分な準備済み要素の追加バッファーを生成する必要があります。 スクロール中に負荷が高くなると、ユーザーには引き続きコンテンツが表示されます。

![実現 rect](images/xaml-attached-layout-realizationrect.png)

要素の作成はコストがかかるため、コンテナー (たとえば、 [Itemsrepeater](/windows/uwp/design/controls-and-patterns/items-repeater)) を仮想化すると、最初に、関連付けられているレイアウトに、ビューポートに一致する[realizationrect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect)が提供されます。 アイドル状態のとき、コンテナーは、より大きな実現化を使用してレイアウトを繰り返し呼び出すことで、準備されたコンテンツのバッファーを拡張することがあります。 この動作は、高速起動時間と優れたパンエクスペリエンスのバランスを取るためのパフォーマンスの最適化です。 ItemsRepeater が生成する最大バッファーサイズは、 [VerticalCacheLength](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.verticalcachelength)プロパティと[HorizontalCacheLength](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.verticalcachelength)プロパティによって制御されます。

**要素の再利用 (リサイクル)**

レイアウトでは、実行するたびに、 [Realizationrect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect)に収まるように要素のサイズと位置を設定する必要があります。 既定では、 [Virtualizinglayout](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout)は、各レイアウトパスの最後に未使用の要素をリサイクルします。

[Measureoverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.measureoverride)および[layouteoverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.arrangeoverride)の一部としてレイアウトに渡される[virtualizinglayoutcontext](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext)は、仮想化レイアウトに必要な追加情報を提供します。 最も一般的に使用される機能のいくつかを次に示します。

1. データ内の項目数 ([ItemCount](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.itemcount)) を照会します。
2. [GetItemAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getitemat)メソッドを使用して特定の項目を取得します。
3. レイアウトが実現された要素で塗りつぶす必要があるビューポートとバッファーを表す、 [Realizationrect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect)を取得します。
4. [Getorcreateelement](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat)メソッドを使用して、特定の項目の UIElement を要求します。

特定のインデックスの要素を要求すると、その要素はレイアウトのそのパスに対して "使用中" としてマークされます。 要素がまだ存在しない場合は、その要素が認識され、自動的に使用できるように準備されます (たとえば、System.windows.datatemplate> で定義されている UI ツリーの拡大、任意のデータバインディングの処理など)。  それ以外の場合は、既存のインスタンスのプールから取得されます。

各メジャーパスの最後には、"使用中" とマークされていない既存の、実現された要素は、 [SuppressAutoRecycle](/uwp/api/microsoft.ui.xaml.controls.elementrealizationoptions)のオプションを使用して取得[された場合を除き、自動的に再利用可能と見なされます。GetOrCreateElementAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat)メソッド。 フレームワークによって自動的にリサイクルプールに移動され、使用できるようになります。 その後、別のコンテナーで使用するためにプルされる可能性があります。 要素の再親に関連するコストが発生するため、可能であればフレームワークはこれを回避しようとします。

仮想化レイアウトが各メジャーの先頭で認識している要素が、実現されていない要素である場合、その再利用を最適化できます。 フレームワークの既定の動作に依存するのではなく、 レイアウトでは、 [RecycleElement](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recycleelement)メソッドを使用して、事前にの要素をリサイクルプールに移動できます。  新しい要素を要求する前にこのメソッドを呼び出すと、既に要素に関連付けられていないインデックスに対して、後で[Getorcreateelementat](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat)要求を発行するときに、これらの既存の要素が使用可能になります。

VirtualizingLayoutContext には、レイアウト作成者がコンテンツに依存するレイアウトを作成するための2つの追加のプロパティが用意されています。 詳細については、後で詳しく説明します。

1. レイアウトへの省略可能な_入力_を提供する[RecommendedAnchorIndex](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recommendedanchorindex) 。
2. レイアウトの省略可能な_出力_である[layoutorigin](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.layoutorigin) 。

## <a name="data-dependent-virtualizing-layouts"></a>データ依存の仮想化レイアウト

表示するコンテンツを測定しなくても、すべての項目のサイズがわかると、仮想化レイアウトがより簡単になります。  このドキュメントでは、通常、データの検査が必要になるため、**データレイアウト**として仮想化レイアウトのこのカテゴリを参照します。  データに基づいて、アプリは、データの一部であるか、以前は設計によって決定されたことが原因で、既知のサイズのビジュアル表現を選択できます。

一般的なアプローチでは、次のようにレイアウトを使用します。

1. すべての項目のサイズと位置を計算します。
2. [Measureoverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.measureoverride)の一部として:
   1. ビューポート内に表示する項目を決定するには、 [Realizationrect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect)を使用します。
   2. [Getorcreateelementat](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat)メソッドを使用して、項目を表すべき UIElement を取得します。
   3. 事前に計算されたサイズで UIElement を[測定](/uwp/api/windows.ui.xaml.uielement.measure)します。
3. Arrange [Eoverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.arrangeoverride)の一部として、各認識される UIElement を事前に計算された位置に[配置](/uwp/api/windows.ui.xaml.uielement.arrange)します。

> [!NOTE]
> データレイアウトアプローチは、多くの場合、_データの仮想化_と互換性がありません。  特に、メモリに読み込まれるデータは、ユーザーに表示されるデータを入力するために必要なデータのみです。  データの仮想化は、データが常駐している場所をスクロールダウンすると、データの遅延または増分読み込みを指していません。  代わりに、スクロールして表示されなくなった項目がメモリから解放されるタイミングを参照します。  データレイアウトでデータレイアウトの一部としてすべてのデータ項目を検査すると、データの仮想化が想定どおりに動作しなくなる可能性があります。  例外とは、すべてのサイズが同じであることを前提とした UniformGridLayout のようなレイアウトです。

> [!TIP]
> さまざまな状況で他のユーザーによって使用されるコントロールライブラリのカスタムコントロールを作成する場合、データレイアウトを選択することはできません。

### <a name="example-xbox-activity-feed-layout"></a>例:Xbox アクティビティフィードレイアウト

Xbox アクティビティフィードの UI では繰り返しパターンが使用されます。このパターンでは、各行にワイドタイルが含まれ、その後に2つの細いタイルが適用されます。 このレイアウトでは、すべての項目のサイズは、データセット内の項目の位置と、タイルの既知のサイズ (幅と幅の比較) の関数です。

![Xbox アクティビティフィード](images/xaml-attached-layout-activityfeedscreenshot.png)

次のコードでは、アクティビティフィード用のカスタムの仮想化 UI について説明します。これは、**データレイアウト**に対して実行する一般的なアプローチを示すためのものです。

<table>
<td>
    <p><strong style="font-weight: semi-bold">XAML コントロールギャラリー</strong>アプリがインストールされている場合は、ここをクリックしてアプリを開き、このサンプルレイアウトで動作している<a href="xamlcontrolsgallery:/item/ItemsRepeater">itemsrepeater</a>を確認します。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML コントロール ギャラリー アプリを入手する (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">ソース コード (GitHub) を入手する</a></li>
    </ul>
</td>
</tr>
</table>

#### <a name="implementation"></a>実装

```csharp
/// <summary>
///  This is a custom layout that displays elements in two different sizes
///  wide (w) and narrow (n). There are two types of rows 
///  odd rows - narrow narrow wide
///  even rows - wide narrow narrow
///  This pattern repeats.
/// </summary>

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

### <a name="optional-managing-the-item-to-uielement-mapping"></a>Optional項目を UIElement マッピングに管理する

既定では、 [Virtualizinglayoutcontext](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext)は、認識される要素と、それが表すデータソース内のインデックスとの間のマッピングを維持します。  レイアウトでは、既定の自動リサイクル動作を妨げる[Getorcreateelementat](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat)メソッドを使用して要素を取得するときに、常に[SuppressAutoRecycle](/uwp/api/microsoft.ui.xaml.controls.elementrealizationoptions)のオプションを要求することによって、このマッピングを管理することを選択できます。  たとえば、スクロールが1方向に制限されていて、それが考慮される項目が常に連続している場合 (つまり、最初の要素と最後の要素のインデックスを知っていれば、動詞する必要があるすべての要素を把握できるようにする場合など) に、レイアウトを選択できます。lized).

#### <a name="example-xbox-activity-feed-measure"></a>例:Xbox アクティビティフィードのメジャー

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

## <a name="content-dependent-virtualizing-layouts"></a>コンテンツ依存の仮想化レイアウト

項目の UI コンテンツを測定して正確なサイズを判断する必要がある場合は、**コンテンツに依存するレイアウト**です。  また、項目のサイズを指定するレイアウトではなく、それぞれの項目のサイズを変更する必要があるレイアウトであると考えることもできます。 このカテゴリに分類されるレイアウトの仮想化は、さらに複雑になります。

> [!NOTE]
> コンテンツに依存するレイアウトでは、データの仮想化が中断されることはありません。

### <a name="estimations"></a>予測

コンテンツに依存するレイアウトでは、予測に基づいて、実現されていないコンテンツのサイズと実現コンテンツの位置の両方を推測します。 これらの推定値が変更されると、結果として得られるコンテンツはスクロール可能な領域内で定期的に移動します。 これにより、軽減されない場合に、ユーザーエクスペリエンスが非常に困難になります。 潜在的な問題と軽減策については、こちらを参照してください。

> [!NOTE]
> すべてのアイテムの正確なサイズを把握し、それらのアイテムの正確なサイズを把握しているデータレイアウトでは、これらの問題を完全に回避できます。

**スクロールアンカー**

XAML には、スクロールコントロールで[IScrollAnchorPovider](/uwp/api/windows.ui.xaml.controls.iscrollanchorprovider)インターフェイスを実装することによって[スクロール](/uwp/api/windows.ui.xaml.controls.iscrollanchorprovider)がサポートされるようにすることで、急激なビューポートの移動を軽減するメカニズムがあります。 ユーザーがコンテンツを操作すると、スクロールコントロールは、追跡対象としてオプトインされた候補のセットから要素を継続的に選択します。 アンカー要素の位置がレイアウト中にシフトする場合、スクロールコントロールはビューポートを自動的に移動してビューポートを維持します。

レイアウトに対して指定された[RecommendedAnchorIndex](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recommendedanchorindex)の値は、スクロールコントロールによって選択された現在のアンカー要素を反映する場合があります。 また、開発者が、 [Itemsrepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)で[getorcreateelement](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.getorcreateelement)メソッドを使用してインデックスの要素を認識するように明示的に要求した場合、そのインデックスは次のレイアウトパスで[RecommendedAnchorIndex](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recommendedanchorindex)として指定されます。 これにより、開発者が要素を認識し、その後、 [Startbring@ view](/uwp/api/windows.ui.xaml.uielement.startbringintoview)メソッドを使用して表示されるように要求する可能性のあるシナリオに対して、レイアウトを準備できます。

[RecommendedAnchorIndex](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recommendedanchorindex)は、コンテンツに依存するレイアウトが項目の位置を推定するときに最初に配置する必要がある、データソース内の項目のインデックスです。 他の実現項目を配置するための開始点として機能します。

**スクロールバーへの影響**

スクロールが固定されていても、コンテンツのサイズが大きく変化することが原因で、レイアウトの推定値がかなり異なる場合は、スクロールバーのつまみの位置が移動する可能性があります。  ユーザーがドラッグしているときにマウスポインターの位置を追跡できない場合は、ユーザーに対して目障りを実行できます。

レイアウトの精度が高くなるほど、ユーザーがスクロールバーのつまみを表示する確率が低くなります。

### <a name="layout-corrections"></a>レイアウトの修正

実際の見積もりを合理化するには、コンテンツに依存するレイアウトを準備する必要があります。  たとえば、ユーザーがコンテンツの一番上までスクロールし、レイアウトによって最初の要素が認識されている場合、要素の開始元の要素を基準とした要素の予想位置が、の原点 (x:0) 以外の場所になることがあります。, y:0). この場合、レイアウトで[layoutorigin](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.layoutorigin)プロパティを使用して、新しいレイアウトの原点として計算された位置を設定できます。  結果はスクロールの固定に似ており、スクロールコントロールのビューポートは、レイアウトによって報告されたコンテンツの位置を考慮して自動的に調整されます。

![LayoutOrigin の修正](images/xaml-attached-layout-origincorrection.png)

### <a name="disconnected-viewports"></a>切断されたビューポート

レイアウトの[Measureoverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.measureoverride)メソッドから返されるサイズは、コンテンツのサイズで最適な推測を表します。これは、後続の各レイアウトで変更される可能性があります。  ユーザーがスクロールすると、レイアウトは、更新された[Realizationrect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect)で継続的に再評価されます。

ユーザーが、レイアウトの観点から、ビューポートを使用できるようになったときに、その前の位置が現在の位置と重ならないように、大きなジャンプを行うようにユーザーが設定した場合。  これは、スクロールの非同期の性質が原因です。 また、レイアウトを使用するアプリでは、現在認識されていない項目に対して要素を表示するように要求したり、レイアウトによって追跡される現在の範囲の外に配置すると推定されたりすることもできます。

レイアウトで推測が正しくないことが検出された場合、または予期しないビューポートのシフトが見られる場合は、その開始位置を方向を確認する必要があります。  XAML コントロールの一部として出荷される仮想化レイアウトは、表示されるコンテンツの性質に対する制限を減らすことで、コンテンツに依存するレイアウトとして開発されます。


### <a name="example-simple-virtualizing-stack-layout-for-variable-sized-items"></a>例:可変サイズの項目の単純な仮想化スタックレイアウト

次のサンプルでは、可変サイズの項目の単純なスタックレイアウトを示しています。

* UI の仮想化をサポートします。
* 予測を使用して、実現されていない項目のサイズを推測します。
* 非連続的なビューポートのシフトを認識し、
* レイアウトの修正を適用して、それらのシフトを考慮します。

**Usage:マークアップ @ no__t-0

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

**Codebehind:メイン .cs @ no__t-0

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

**Code:VirtualizingStackLayout. cs @ no__t-0

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
