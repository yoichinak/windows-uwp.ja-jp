---
Description: Panel クラスからカスタム クラスを取得して、XAML レイアウトのカスタム パネルを定義できます。
MS-HAID: dev\_ctrl\_layout\_txt.xaml\_custom\_panels\_overview
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: XAML カスタム パネルの概要
ms.assetid: 0CD395CD-E2AB-429D-BB49-56A71C5CC35D
label: XAML custom panels overview (Windows apps)
template: detail.hbs
op-migration-status: ready
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f57f50cdd5a23857e4384beea5803fc668abbf2c
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89165516"
---
# <a name="xaml-custom-panels-overview"></a>XAML カスタム パネルの概要

 

*パネル*は、Extensible Application Markup Language (XAML) レイアウト システムが実行されて、アプリの UI が表示されるときに、含まれている子要素のレイアウト動作を提供するオブジェクトです。 


> **重要な API**:[**Panel**](/uwp/api/Windows.UI.Xaml.Controls.Panel)、[**ArrangeOverride**](/uwp/api/windows.ui.xaml.frameworkelement.arrangeoverride)、[**MeasureOverride**](/uwp/api/windows.ui.xaml.frameworkelement.measureoverride)

[  **Panel**](/uwp/api/Windows.UI.Xaml.Controls.Panel) クラスからカスタム クラスを派生させて、XAML レイアウトのカスタム パネルを定義できます。 パネルの動作は、[**MeasureOverride**](/uwp/api/windows.ui.xaml.frameworkelement.measureoverride) と [**ArrangeOverride**](/uwp/api/windows.ui.xaml.frameworkelement.arrangeoverride) をオーバーライドすることで子要素を評価して配置するロジックを提供して実行します。

## <a name="the-panel-base-class"></a>**Panel** 基底クラス


カスタム パネル クラスを定義するには、[**Panel**](/uwp/api/Windows.UI.Xaml.Controls.Panel) クラスから直接派生させるか、[**Grid**](/uwp/api/Windows.UI.Xaml.Controls.Grid) や [**StackPanel**](/uwp/api/Windows.UI.Xaml.Controls.StackPanel) などの、シールされていない実用的なパネル クラスの 1 つから派生させます。 より容易なのは、**Panel** から派生させることです。これは、既にレイアウト動作があるパネルの既存のレイアウト ロジックを回避することは難しい場合があるためです。 また、動作があるパネルの既存のプロパティが、パネルのレイアウト機能に関連していない場合もあります。

[  **Panel**](/uwp/api/Windows.UI.Xaml.Controls.Panel) から、カスタム パネルは次の API を継承します。

-   [  **Children**](/uwp/api/windows.ui.xaml.controls.panel.children) プロパティ
-   [  **Background**](/uwp/api/windows.ui.xaml.controls.panel.background)、[**ChildrenTransitions**](/uwp/api/windows.ui.xaml.controls.panel.childrentransitions)、[**IsItemsHost**](/uwp/api/windows.ui.xaml.controls.panel.isitemshost) プロパティと、依存関係プロパティの識別子。 これらのプロパティはどれも仮想プロパティではないため、通常は、オーバーライドしたり、置き換えたりすることはありません。 これらのプロパティは、通常、カスタム パネルのシナリオでは不要です。値の読み取りにも使用しません。
-   レイアウト オーバーライド メソッド ([**MeasureOverride**](/uwp/api/windows.ui.xaml.frameworkelement.measureoverride) と [**ArrangeOverride**](/uwp/api/windows.ui.xaml.frameworkelement.arrangeoverride))。 これらは、最初は [**FrameworkElement**](/uwp/api/Windows.UI.Xaml.FrameworkElement) で定義されていました。 [  **Panel**](/uwp/api/Windows.UI.Xaml.Controls.Panel) 既定クラスは、これらをオーバーライドしませんが、[**Grid**](/uwp/api/Windows.UI.Xaml.Controls.Grid) のような実用的なパネルには、ネイティブ コードとして実装されたオーバーライド実装があり、システムによって実行されます。 カスタム パネルを定義するために必要な作業の大部分は、**ArrangeOverride** と **MeasureOverride** に新しい (または付加的な) 実装を提供することです。
-   [  **FrameworkElement**](/uwp/api/Windows.UI.Xaml.FrameworkElement)、[**UIElement**](/uwp/api/Windows.UI.Xaml.UIElement)、および [**DependencyObject**](/uwp/api/Windows.UI.Xaml.DependencyObject) のその他のすべての API ([**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height)、[**Visibility**](/uwp/api/windows.ui.xaml.uielement.visibility) など)。 これらのプロパティの値は、レイアウト オーバーライドで参照することがありますが、仮想値ではないため、オーバーライドしたり、置き換えたりすることはありません。

ここでは、カスタム パネルがレイアウトで可能な動作および必要な動作についてのすべての可能性を考慮できるように、XAML レイアウトの概念について説明します。 すぐに作業を開始できるようにカスタム パネルの実装例を参照する場合は、「[BoxPanel、カスタム パネルの例](boxpanel-example-custom-panel.md)」を参照してください。

## <a name="the-children-property"></a>**Children** プロパティ


[  **Children**](/uwp/api/windows.ui.xaml.controls.panel.children) プロパティは、カスタム パネルに関連しています。これは、[**Panel**](/uwp/api/Windows.UI.Xaml.Controls.Panel) から派生したすべてのクラスが、コレクションに含まれている子要素を保存する場所として **Children** プロパティを使うためです。 **Children** は、**Panel** クラスの XAML コンテンツ プロパティとして指定されており、**Panel** から派生したすべてのクラスは、XAML コンテンツ プロパティの動作を継承できます。 プロパティが XAML コンテンツ プロパティを指定している場合は、その XAML マークアップが、マークアップでそのプロパティを指定するときにプロパティ要素を省略でき、直接の子マークアップ (「コンテンツ」) として値が設定されることを意味します。 たとえば、**Panel** から、**CustomPanel** という名前のクラスを派生させ、これによって新しい動作が定義されない場合は、まだ、次のマークアップを使うことができます。

```XAML
<local:CustomPanel>
  <Button Name="button1"/>
  <Button Name="button2"/>
</local:CustomPanel>
```

XAML パーサーがこのマークアップを読み取るときに、[**Children**](/uwp/api/windows.ui.xaml.controls.panel.children) は、すべての [**Panel**](/uwp/api/Windows.UI.Xaml.Controls.Panel) 派生型の XAML コンテンツ プロパティであると認識されるため、パーサーは、[**Children** プロパティの [**UIElementCollection**](/uwp/api/Windows.UI.Xaml.Controls.UIElementCollection) 値に 2 つの **Button**](/uwp/api/Windows.UI.Xaml.Controls.Button) 要素を追加します。 XAML コンテンツ プロパティにより、UI 定義の XAML マークアップで親子関係を効率化しやすくなります。 XAML コンテンツ プロパティの詳細と、XAML の解析時のコレクション プロパティの設定方法については、「[基本的な XAML 構文のガイド](../../xaml-platform/xaml-syntax-guide.md)」を参照してください。

[  **Children**](/uwp/api/windows.ui.xaml.controls.panel.children) プロパティの値を維持しているコレクション型は [**UIElementCollection**](/uwp/api/Windows.UI.Xaml.Controls.UIElementCollection) クラスです。 **UIElementCollection** は、適用された項目の型として [**UIElement**](/uwp/api/Windows.UI.Xaml.UIElement) を使う、厳密に型指定されたコレクションです。 **UIElement** は、多くの実用的な UI 要素型によって継承されている基本型であるため、ここでは、型が意図的に緩やかに適用されています。 ただし、[**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush) が [**Panel**](/uwp/api/Windows.UI.Xaml.Controls.Panel) の直接の子になることができない点は適用されます。これは一般に、UI に表示され、レイアウトに含まれると予想されている要素のみが、**Panel** の子要素となることを意味します。

通常、XAML 定義では、カスタム パネルは、[**UIElement**](/uwp/api/Windows.UI.Xaml.UIElement) プロパティの特性をそのまま使用して、あらゆる [**Children**](/uwp/api/windows.ui.xaml.controls.panel.children) 子要素を受け入れます。 高度なシナリオとしては、レイアウトのオーバーライドでコレクションの反復処理を行う場合に、子要素の型の詳細な確認をサポートできます。

パネルのロジックは、[**Children**](/uwp/api/windows.ui.xaml.controls.panel.children) コレクションでループ処理を行うだけでなく、`Children.Count` によって影響される場合もあります。 個々の項目の目的のサイズやその他の特性ではなく、少なくとも項目の数に部分的に基づいて、スペースを割り当てているロジックがある場合もあります。

## <a name="overriding-the-layout-methods"></a>レイアウト メソッドのオーバーライド


レイアウト オーバーライド メソッドの基本的なモデル ([**MeasureOverride**](/uwp/api/windows.ui.xaml.frameworkelement.measureoverride) と [**ArrangeOverride**](/uwp/api/windows.ui.xaml.frameworkelement.arrangeoverride)) は、すべての子で反復処理を行い、各子要素の特定のレイアウト メソッドを呼び出す必要があることです。 最初のレイアウトのサイクルは、XAML レイアウト システムがルート ウィンドウの視覚効果を設定すると、開始されます。 それぞれの親はその子でレイアウトを呼び出すため、これによって、レイアウトの一部となる可能性のあるすべての UI 要素に対するレイアウト メソッドへの呼び出しが伝達されます。 XAML レイアウトでは、測定と配置という 2 つの段階があります。

[**Panel**](/uwp/api/Windows.UI.Xaml.Controls.Panel) 基底クラスからの [**MeasureOverride**](/uwp/api/windows.ui.xaml.frameworkelement.measureoverride) と [**ArrangeOverride**](/uwp/api/windows.ui.xaml.frameworkelement.arrangeoverride) についての組み込みのレイアウト メソッドの動作は発生しません。 [  **Children**](/uwp/api/windows.ui.xaml.controls.panel.children) の項目は、XAML のビジュアル ツリーの一部として自動的には表示されません。 **MeasureOverride** と **ArrangeOverride** の実装内のレイアウト パスを介して **Children** で見つかる各項目でレイアウトのメソッドを呼び出すことによって項目がレイアウト プロセスに認識されるようにするかどうかはユーザーが決定します。

独自の継承がある場合を除き、レイアウトのオーバーライドの基本実装を呼び出す理由はありません。 いずれの場合も、レイアウト動作のネイティブ メソッド (存在する場合) は動作し、オーバーライドから基本実装を呼び出さなくても、ネイティブ動作が発生しなくなることはありません。

測定パスの間に、レイアウトのロジックは、子要素の [**Measure**](/uwp/api/windows.ui.xaml.uielement.measure) メソッドを呼び出して、望ましいサイズを各子要素に照会します。 **Measure** メソッドを呼び出すと、[**DesiredSize**](/uwp/api/windows.ui.xaml.uielement.desiredsize) プロパティの値が設定されます。 パネル自体の望ましいサイズは、[**MeasureOverride**](/uwp/api/windows.ui.xaml.frameworkelement.measureoverride) の戻り値です。

配置パスでは、子要素の位置とサイズが x-y スペースで決定され、レイアウト構成を表示する準備を行います。 コードは、[**Children**](/uwp/api/windows.ui.xaml.controls.panel.children) の各子要素で [**Arrange**](/uwp/api/windows.ui.xaml.uielement.arrange) を呼び出す必要があります。これによって、要素がレイアウトに含まれることがレイアウト システムで検出されます。 **Arrange** 呼び出しは、構成とレンダリングに先行して行われます。つまり、要素の配置先がレイアウト システムに通知され、表示のために構成が送信されます。

レイアウトのロジックが実行時にどのように動作するかは、多くのプロパティと値によって決まります。 レイアウト プロセスについての考え方の 1 つは、子 (一般に、UI で最も深く入れ子になっている要素) のない要素が最初に測定を完了できる要素であるということです。 このような要素には、望ましいサイズに影響する子要素の依存関係がありません。 それぞれの要素には、望ましいサイズがある場合がありますが、レイアウトが実際に発生するまでは、サイズの候補にすぎません。 次に、測定パスは、ルート要素に測定値が与えられ、すべての測定を完了できるまで、ビジュアル ツリーを上へとたどり続けます。

候補のレイアウトは、現在のアプリ ウィンドウ内に収まる必要があります。収まらない場合は、UI の一部がクリップされます。 パネルでは、クリッピング ロジックが決定されることがよくあります。 パネルのロジックは、[**MeasureOverride**](/uwp/api/windows.ui.xaml.frameworkelement.measureoverride) の実装内から利用できるサイズを特定できます。また、サイズ制限を子にも適用し、すべてが最適に収まるように複数の子の間でスペースを分割することが必要な場合もよくあります。 理想的なレイアウトの結果は、レイアウトすべての部分のさまざまなプロパティを使い、しかも、アプリ ウィンドウ内に収まることです。 これには、パネルのレイアウト ロジックを最適に実装するだけでなく、そのパネルを使って UI を構築するあらゆるアプリ コードで慎重に UI を設計することが必要です。 全体的な UI 設計に含まれる子要素が多すぎてアプリに収まらない場合は、パネル設計が適切に表示されることはありません。

レイアウト システムが機能するための要件の大部分は、[**FrameworkElement**](/uwp/api/Windows.UI.Xaml.FrameworkElement) に基づく要素のいずれかに、コンテナーで子として機能するときの固有の動作の一部が既に含まれることです。 たとえば、**FrameworkElement** のいくつかの API は、レイアウト動作を通知する API であるか、またはレイアウトが機能するための必須 API です。 具体的な内容は次のとおりです。

-   [**DesiredSize**](/uwp/api/windows.ui.xaml.uielement.desiredsize) (実際は [**UIElement**](/uwp/api/Windows.UI.Xaml.UIElement) プロパティ)
-   [**ActualHeight**](/uwp/api/windows.ui.xaml.frameworkelement.actualheight) および [**ActualWidth**](/uwp/api/windows.ui.xaml.frameworkelement.actualwidth)
-   [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) および [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width)
-   [**Margin**](/uwp/api/windows.ui.xaml.frameworkelement.margin)
-   [**LayoutUpdated**](/uwp/api/windows.ui.xaml.frameworkelement.layoutupdated) イベント
-   [**HorizontalAlignment**](/uwp/api/windows.ui.xaml.frameworkelement.horizontalalignment) および [**VerticalAlignment**](/uwp/api/windows.ui.xaml.frameworkelement.verticalalignment)
-   [**ArrangeOverride**](/uwp/api/windows.ui.xaml.frameworkelement.arrangeoverride) メソッドと [**MeasureOverride**](/uwp/api/windows.ui.xaml.frameworkelement.measureoverride) メソッド
-   [**Arrange**](/uwp/api/windows.ui.xaml.uielement.arrange) メソッドと [**Measure**](/uwp/api/windows.ui.xaml.uielement.measure) メソッド: これらには、要素レベルのレイアウト動作を処理する [**FrameworkElement**](/uwp/api/Windows.UI.Xaml.FrameworkElement) レベルで定義されたネイティブ実装があります

## <a name="measureoverride"></a>**MeasureOverride**


[**MeasureOverride**](/uwp/api/windows.ui.xaml.frameworkelement.measureoverride) メソッドには、[**Measure**](/uwp/api/windows.ui.xaml.uielement.measure) メソッドがレイアウト内のその親によってパネルで呼び出されるときに、パネル自体の開始 [**DesiredSize**](/uwp/api/windows.ui.xaml.uielement.desiredsize) としてレイアウト システムで使われる戻り値があります。 どのロジックをメソッドで選択するかは、その戻り値と同様に重要であり、多くの場合、返される値はロジックに影響されます。

すべての [**MeasureOverride**](/uwp/api/windows.ui.xaml.frameworkelement.measureoverride) 実装は、[**Children**](/uwp/api/windows.ui.xaml.controls.panel.children) でループし、各子要素で [**Measure**](/uwp/api/windows.ui.xaml.uielement.measure) メソッドを呼び出す必要があります。 **Measure** メソッドを呼び出すと、[**DesiredSize**](/uwp/api/windows.ui.xaml.uielement.desiredsize) プロパティの値が設定されます。 これにより、パネル自体に必要なスペースの大きさだけでなく、そのスペースを要素間で分割したり、特定の子要素のためにサイズ調整したりする方法がわかる場合があります。

次に示すのは、[**MeasureOverride**](/uwp/api/windows.ui.xaml.frameworkelement.measureoverride) メソッドの非常に基本的なスケルトンです。

```CSharp
protected override Size MeasureOverride(Size availableSize)
{
    Size returnSize; //TODO might return availableSize, might do something else
     
    //loop through each Child, call Measure on each
    foreach (UIElement child in Children)
    {
        child.Measure(new Size()); // TODO determine how much space the panel allots for this child, that's what you pass to Measure
        Size childDesiredSize = child.DesiredSize; //TODO determine how the returned Size is influenced by each child's DesiredSize
        //TODO, logic if passed-in Size and net DesiredSize are different, does that matter?
    }
    return returnSize;
}
```

要素は、多くの場合、レイアウトの準備ができた時点で自然なサイズになっています。 [**Measure**](/uwp/api/windows.ui.xaml.uielement.measure) について渡した *availableSize* が小さい場合は、測定パスの後に、[**DesiredSize**](/uwp/api/windows.ui.xaml.uielement.desiredsize) が自然なサイズを示す場合もあります。 自然なサイズが、**Measure** について渡した *availableSize* よりも大きい場合は、**DesiredSize** が *availableSize* に制限されます。 これは、**Measure** の内部実装の動作であり、レイアウトのオーバーライドは、この動作を考慮する必要があります。

自然なサイズのない要素もあります。このような要素には、[**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) と [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) の **Auto** 値があるためです。 これらの要素は、**Auto** 値が表すとおり、完全な *availableSize* を使用します。つまり、要素を使用可能な最大サイズに調整します。直接のレイアウトの親は、[**Measure**](/uwp/api/windows.ui.xaml.uielement.measure) と共に *availableSize* を呼び出して、このサイズを伝えます。 実際には、(トップレベル ウィンドウである場合でも) UI がサイズ設定される測定値が常に存在します。最終的に測定パスは、すべての **Auto** 値を親の制約へと解決し、すべての **Auto** 値要素に実際の測定値 (レイアウトが完了した後に [**ActualWidth**](/uwp/api/windows.ui.xaml.frameworkelement.actualwidth) と [**ActualHeight**](/uwp/api/windows.ui.xaml.frameworkelement.actualheight) をチェックして取得できます) が与えられます。

少なくとも 1 つの無限サイズを含む [**Measure**](/uwp/api/windows.ui.xaml.uielement.measure) にサイズを渡すこともできます。これは、パネルがそれ自体のサイズを、コンテンツの測定値に収まるように調整できることを示します。 測定される各子要素の [**DesiredSize**](/uwp/api/windows.ui.xaml.uielement.desiredsize) 値が、その要素の自然なサイズを使用して設定されます。 配置パスでは、通常、そのサイズを使用してパネルが配置されます。

[**TextBlock**](/uwp/api/Windows.UI.Xaml.Controls.TextBlock) などのテキスト要素には、[**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) 値と [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) 値のいずれも設定されていない場合でも、そのテキスト文字列とテキスト プロパティに基づいて計算された [**ActualWidth**](/uwp/api/windows.ui.xaml.frameworkelement.actualwidth) と [**ActualHeight**](/uwp/api/windows.ui.xaml.frameworkelement.actualheight) があります。パネルのロジックでは、これらのサイズを考慮する必要があります。 テキストのクリッピングは、特に不適切な UI 動作です。

望ましいサイズの測定値が実装で使用されない場合でも、各子要素で [**Measure**](/uwp/api/windows.ui.xaml.uielement.measure) メソッドを呼び出すことをお勧めします。これは、**Measure** によってトリガーされる内部動作とネイティブ動作が呼び出されるためです。 要素がレイアウトに関与するには、各子要素について、測定パスで **Measure** が、配置パスで [**Arrange**](/uwp/api/windows.ui.xaml.uielement.arrange) メソッドが呼び出される必要があります。 これらのメソッドを呼び出すと、オブジェクトの内部フラグが設定されます。また、ビジュアル ツリーをビルドして UI を表示するときにシステムのレイアウト ロジックに必要な値 ([**DesiredSize**](/uwp/api/windows.ui.xaml.uielement.desiredsize) プロパティなど) が入力されます。

[**MeasureOverride**](/uwp/api/windows.ui.xaml.frameworkelement.measureoverride) 戻り値は、[**DesiredSize**](/uwp/api/windows.ui.xaml.uielement.desiredsize)、または [**Measure**](/uwp/api/windows.ui.xaml.uielement.measure) が呼び出されるときの [**Children**](/uwp/api/windows.ui.xaml.controls.panel.children) の各子要素のその他のサイズの考慮事項を解釈するパネルのロジックに基づいています。 子からの **DesiredSize** 値の取り扱いと、**MeasureOverride** 戻り値でのこの値の使用方法は、ロジックの解釈によって決定されます。 通常は値を変更せずに、加算することはありません。これは、**MeasureOverride** の入力値は、パネルの親が示す使用可能な固定サイズであることが多いためです。 そのサイズを超えると、パネル自体がクリップされる可能性があります。 通常は、子の合計サイズとパネルで使用可能なサイズを比較し、必要に応じて調整します。

### <a name="tips-and-guidance"></a>ヒントとガイダンス

-   望ましいのは、カスタムのパネルが、UI の構成での最初の実際のビジュアルに適していることです。たとえば、[**Page**](/uwp/api/Windows.UI.Xaml.Controls.Page)、[**UserControl**](/uwp/api/Windows.UI.Xaml.Controls.UserControl)、または XAML ページのルートである別の要素のすぐ下のレベルにあることです。 [  **MeasureOverride**](/uwp/api/windows.ui.xaml.frameworkelement.measureoverride) の実装では、値を検証せずに入力 [**Size**](/uwp/api/Windows.Foundation.Size) を返すことを、通常の動作にしないでください。 返される **Size** に **Infinity** 値が含まれる場合は、そのためにランタイムのレイアウト ロジックで例外がスローされる場合があります。 **Infinity** 値がアプリのメイン ウィンドウにあり、このウィンドウは、スクロール可能であるために高さの最大値がない場合があります。 その他のスクロール可能なコンテンツも、同様に動作する可能性があります。
-   [  **MeasureOverride**](/uwp/api/windows.ui.xaml.frameworkelement.measureoverride) の実装のもう 1 つの一般的な間違いは、新しい既定の [**Size**](/uwp/api/Windows.Foundation.Size) (高さと幅の値が 0) を返すことです。 その値が開始値となる場合もあります。また、どの子も表示されないことがパネルで設定されているために、それが正しい値である場合があります。 ただし、既定の **Size** は、パネルでは、ホストによって適切なサイズに調整されなくなります。 UI でスペースを必要としないため、スペースが割り当てられず、表示されません。 これ以外のパネルのコードはすべて適切に機能する可能性がありますが、高さと幅がゼロで構成されている場合は、そのパネルも、そのコンテンツも表示されません。
-   オーバーライド内では、子要素を [**FrameworkElement**](/uwp/api/Windows.UI.Xaml.FrameworkElement) にキャストしないようにし、レイアウトの結果としての集計プロパティ、特に [**ActualWidth**](/uwp/api/windows.ui.xaml.frameworkelement.actualwidth) と [**ActualHeight**](/uwp/api/windows.ui.xaml.frameworkelement.actualheight) を使ってください。 ほとんどの一般的なシナリオでは、子の [**DesiredSize**](/uwp/api/windows.ui.xaml.uielement.desiredsize) 値をロジックの基礎にすることができます。こうすると、子要素のプロパティのうち、[**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) または [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) に関連するものはいずれも必要ではなくなります。 イメージ ファイルの自然なサイズなどのように、要素の型とその詳細情報がわかっている特殊なケースでは、レイアウト システムによってアクティブに変更される値ではないため、要素の特殊な情報を利用できます。 レイアウトによる集計プロパティをレイアウト ロジックの一部に含めると、意図しないレイアウト ループが定義される危険性が著しく増大します。 このようなループを定義すると、有効なレイアウトを作成できなくなるため、ループから回復できない場合は、システムが [**LayoutCycleException**](/dotnet/api/windows.ui.xaml.markup.xamlparseexception?view=dotnet-uwp-10.0) をスローする可能性があります。
-   パネルは、通常、使用可能なスペースを複数の子要素間で分割しますが、その分割方法はさまざまです。 たとえば、[**Grid**](/uwp/api/Windows.UI.Xaml.Controls.Grid) は、スター サイズ指定とピクセル値の両方をサポートし、[**RowDefinition**](/uwp/api/Windows.UI.Xaml.Controls.RowDefinition) 値と [**ColumnDefinition**](/uwp/api/Windows.UI.Xaml.Controls.ColumnDefinition) 値を使って **Grid** セルへとスペースを分割するレイアウト ロジックを実装します。 これらがピクセル値である場合は、それぞれの子に使用可能なサイズが既にわかっているため、これが、グリッド スタイルの [**Measure**](/uwp/api/windows.ui.xaml.uielement.measure) の入力サイズとして渡されます。
-   パネル自体で、予約されたスペースを項目間の余白として使用できます。 これを行う場合は、[**Margin**](/uwp/api/windows.ui.xaml.frameworkelement.margin) とあらゆる **Padding** プロパティのいずれとも異なるプロパティとして測定値を公開するようにします。
-   要素には、前のレイアウト パスに基づき、[**ActualWidth**](/uwp/api/windows.ui.xaml.frameworkelement.actualwidth) プロパティと [**ActualHeight**](/uwp/api/windows.ui.xaml.frameworkelement.actualheight) プロパティの値がある場合があります。 値が変化する場合、実行する特別なロジックがあるが、パネルのロジックがイベント処理で変化を確認する必要がなければ、アプリの UI コードで要素に [**LayoutUpdated**](/uwp/api/windows.ui.xaml.frameworkelement.layoutupdated) のハンドラーを設定できます。 レイアウト システムは既に、レイアウトを再実行するタイミングを決定しています。これは、レイアウト関連プロパティの値が変化し、適切な場合は、パネルの [**MeasureOverride**](/uwp/api/windows.ui.xaml.frameworkelement.measureoverride) または [**ArrangeOverride**](/uwp/api/windows.ui.xaml.frameworkelement.arrangeoverride) が自動的に呼び出されるためです。

## <a name="arrangeoverride"></a>**ArrangeOverride**


[  **ArrangeOverride**](/uwp/api/windows.ui.xaml.frameworkelement.arrangeoverride) メソッドには、[**Size**](/uwp/api/Windows.Foundation.Size) 戻り値があり、これは、パネル自体を表示するときにレイアウト システムで使われます。このとき、[**Arrange**](/uwp/api/windows.ui.xaml.uielement.arrange) メソッドが、レイアウト内のその親によってパネルで呼び出されます。 入力 *finalSize* と、**ArrangeOverride** によって返される **Size** は同じであるのが一般的です。 同じでない場合は、パネルがそれ自体を、レイアウトに関与する他の要素が使用可能であることを示すサイズとは異なるサイズにしようとしていることを意味します。 最終的なサイズは、パネル コードでレイアウトの測定パスを以前に実行したことに基づいているため、一般的には、異なるサイズは返されません。したがって、意図的に測定ロジックを無視しようとしていることになります。

**Infinity** コンポーネントを持つ [**Size**](/uwp/api/Windows.Foundation.Size) は返さないようにしてください。 そうした **Size** を使おうとすると、内部レイアウトから例外がスローされます。

すべての [**ArrangeOverride**](/uwp/api/windows.ui.xaml.frameworkelement.arrangeoverride) 実装は、[**Children**](/uwp/api/windows.ui.xaml.controls.panel.children) でループし、各子要素で [**Arrange**](/uwp/api/windows.ui.xaml.uielement.arrange) メソッドを呼び出す必要があります。 [  **Measure**](/uwp/api/windows.ui.xaml.uielement.measure) と同様に、**Arrange** は値を返しません。 **Measure** とは異なり、結果として集計プロパティが設定されることはありません (ただし、この要素には、通常、[**LayoutUpdated**](/uwp/api/windows.ui.xaml.frameworkelement.layoutupdated) イベントが発生します)。

次に示すのは、[**ArrangeOverride**](/uwp/api/windows.ui.xaml.frameworkelement.arrangeoverride) メソッドの非常に基本的なスケルトンです。

```CSharp
protected override Size ArrangeOverride(Size finalSize)
{
    //loop through each Child, call Arrange on each
    foreach (UIElement child in Children)
    {
        Point anchorPoint = new Point(); //TODO more logic for topleft corner placement in your panel
       // for this child, and based on finalSize or other internal state of your panel
        child.Arrange(new Rect(anchorPoint, child.DesiredSize)); //OR, set a different Size 
    }
    return finalSize; //OR, return a different Size, but that's rare
}
```

レイアウトの配置パスは、その前に測定パスが発生することなく発生する場合があります。 ただし、これが発生するのは、前の測定値に影響するようなプロパティは変更されていないとレイアウト システムが判断している場合のみです。 たとえば、配置が変更された場合、その特定の要素の [**DesiredSize**](/uwp/api/windows.ui.xaml.uielement.desiredsize) が、配置の選択が変わったときに変更されないため、この要素を測定し直す必要はありません。 一方、レイアウトのいずれかの要素で [**ActualHeight**](/uwp/api/windows.ui.xaml.frameworkelement.actualheight) が変更された場合は、新しい測定パスが必要になります。 レイアウト システムは自動的に実際の測定の変更を検出し、もう一度測定パスを呼び出した後、別の配置パスを実行します。

[  **Arrange**](/uwp/api/windows.ui.xaml.uielement.arrange) の入力は、[**Rect**](/uwp/api/Windows.Foundation.Rect) 値を受け取ります。 この **Rect** を作成する一般的な方法は、[**Point**](/uwp/api/Windows.Foundation.Point) の入力と [**Size**](/uwp/api/Windows.Foundation.Size) の入力を持つコンストラクターを使うことです。 **Point** は、要素の境界ボックスの左上隅を配置するポイントです。 **Size** は、この特定の要素を表示するために使われるサイズです。 多くの場合、この要素の [**DesiredSize**](/uwp/api/windows.ui.xaml.uielement.desiredsize) を、この **Size** 値として使います。これは、レイアウトに関与したすべての要素の **DesiredSize** を確立することが、レイアウトの測定パスの目的であったためです。 (測定パスは、反復される方法で要素のサイズ設定全体を決定します。このため、配置パスに到達した後は、レイアウト システムが要素の配置を最適化できます)。

通常、各 [**ArrangeOverride**](/uwp/api/windows.ui.xaml.frameworkelement.arrangeoverride) 実装で異なっているのは、それぞれの子の配置方法の [**Point**](/uwp/api/Windows.Foundation.Point) コンポーネントをパネルが決定するためのロジックです。 [  **Canvas**](/uwp/api/Windows.UI.Xaml.Controls.Canvas) などの絶対配置のパネルでは、[**Canvas.Left**](/dotnet/api/system.windows.controls.canvas.left) 値と [**Canvas.Top**](/dotnet/api/system.windows.controls.canvas.top) 値を介して各要素から取得する明示的な配置情報を使用します。 [  **Grid**](/uwp/api/Windows.UI.Xaml.Controls.Grid) などのスペース分割のパネルには、使用可能なスペースをセルに分割する数学演算があり、各セルには、そのコンテンツが配置され、位置調整される場所に関する x-y 値があります。 [  **StackPanel**](/uwp/api/Windows.UI.Xaml.Controls.StackPanel) などのアダプティブ パネルでは、コンテンツの向きとサイズに合わせてパネル自体を拡大する場合があります。

直接制御して [**Arrange**](/uwp/api/windows.ui.xaml.uielement.arrange) に渡すもの以外にも、レイアウトの要素の位置に影響するものがあります。 これらは、すべての [**FrameworkElement**](/uwp/api/Windows.UI.Xaml.FrameworkElement) 派生型に一般的な **Arrange** の内部ネイティブ実装によるもので、この実装は、テキスト要素などのあるその他の型によって拡張されます。 たとえば、要素には余白と配置を含めることができ、一部の要素には、パディングを含めることができます。 これらのプロパティは、多くの場合、相互に作用します。 詳しくは、「[配置、余白、およびパディング](alignment-margin-padding.md)」をご覧ください。

## <a name="panels-and-controls"></a>パネルおよびコントロール


カスタム コントロールとして作成する必要のあるカスタム パネルには、機能を含めないようにします。 パネルの役割は、パネル内の子要素コンテンツを、自動的に実行されるレイアウトの機能として表示することです。 パネルでは、コンテンツに装飾を追加 ([**Border**](/uwp/api/Windows.UI.Xaml.Controls.Border) が、表示する要素の周りに境界線を追加する場合と同様に) したり、パディングなどのレイアウト関連の調整を実行したりすることがあります。 ただし、報告や、子からの情報の使用以上にビジュアル ツリーの出力を拡張する場合は、これ以上の機能を含めないようにしてください。

ユーザーがアクセスできる対話式操作がある場合は、パネルではなく、カスタム コントロールを作る必要があります。 たとえば、クリッピングを防ぐことが目的である場合でも、パネルが、表示するコンテンツにスクロール ビューポートを追加しないようにします。スクロールバーや親指などは、対話式のコントロール パーツであるためです (最終的には、コンテンツにスクロール バーが含まれる場合がありますが、これは、子のロジックで実行されるようにする必要があります。 レイアウト処理としてスクロールを追加して強制的に実行しないでください)。コントロールのコンテンツを表示するには、コントロールを作成し、そのコントロールのビジュアル ツリーで重要な役割を果たすカスタム パネルを作ることもできます。 ただし、コントロールとパネルは個別のコード オブジェクトである必要があります。

コントロールとパネルを区別することが重要な理由の 1 つは、Microsoft UI オートメーションとアクセシビリティです。 パネルは、論理的な動作ではなく、視覚的レイアウト動作を提供します。 UI 要素が視覚的にどのように表示されるかは、通常はアクセシビリティのシナリオで重要である UI の要素ではありません。 アクセシビリティでは、UI を理解するうえで論理的に重要なアプリの構成要素を公開します。 操作が必要な場合は、コントロールが UI オートメーション インフラストラクチャに操作の可能性を公開する必要があります。 詳しくは、「[カスタム オートメーション ピア](../accessibility/custom-automation-peers.md)」をご覧ください。

## <a name="other-layout-api"></a>その他のレイアウト API


他にも、レイアウト システムの一部であるが、[**Panel**](/uwp/api/Windows.UI.Xaml.Controls.Panel) で宣言されていない API があります。 そうした API は、パネルの実装、またはパネルを使うカスタム コントロールで使うことができます。

-   [**UpdateLayout**](/uwp/api/windows.ui.xaml.uielement.updatelayout)、[**InvalidateMeasure**](/uwp/api/windows.ui.xaml.uielement.invalidatemeasure)、および [**InvalidateArrange**](/uwp/api/windows.ui.xaml.uielement.invalidatearrange) は、レイアウト パスを開始するメソッドです。 **InvalidateArrange** は、測定パスをトリガーしない場合もありますが、他の 2 つは測定パスをトリガーします。 これらのメソッドは、レイアウト メソッド オーバーライドで呼び出さないでください。呼び出すと、ほとんどの場合、レイアウトのループが発生します。 通常、制御コードも、これらを呼び出す必要はありません。 レイアウトのほとんどの機能は、[**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) などのフレームワーク定義のレイアウト プロパティへの変更を検出することによって自動的にトリガーされます。
-   [**LayoutUpdated**](/uwp/api/windows.ui.xaml.frameworkelement.layoutupdated) は、要素のレイアウトの機能が変化したときに発生するイベントです。 これは、パネルに固有のイベントではなく、[**FrameworkElement**](/uwp/api/Windows.UI.Xaml.FrameworkElement) で定義されています。
-   [**SizeChanged**](/uwp/api/windows.ui.xaml.frameworkelement.sizechanged) は、レイアウト パスが完了した後にのみ発生するイベントで、[**ActualHeight**](/uwp/api/windows.ui.xaml.frameworkelement.actualheight) または [**ActualWidth**](/uwp/api/windows.ui.xaml.frameworkelement.actualwidth) が、結果として変更されたことを示します。 これは、もう 1 つの [**FrameworkElement**](/uwp/api/Windows.UI.Xaml.FrameworkElement) イベントです。 [  **LayoutUpdated**](/uwp/api/windows.ui.xaml.frameworkelement.layoutupdated) は発生するが、**SizeChanged** は発生しない場合があります。 たとえば、内部コンテンツが再配置されたが、要素のサイズは変更されなかった場合です。


## <a name="related-topics"></a>関連トピック

**リファレンス**
* [**FrameworkElement.ArrangeOverride**](/uwp/api/windows.ui.xaml.frameworkelement.arrangeoverride)
* [**FrameworkElement.MeasureOverride**](/uwp/api/windows.ui.xaml.frameworkelement.measureoverride)
* [**Panel**](/uwp/api/Windows.UI.Xaml.Controls.Panel)

**概念**
* [配置、余白、およびパディング](alignment-margin-padding.md)