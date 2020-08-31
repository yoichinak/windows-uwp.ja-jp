---
description: XAML での添付プロパティの概念を説明し、例をいくつか紹介します。
title: 添付プロパティの概要
ms.assetid: 098C1DE0-D640-48B1-9961-D0ADF33266E2
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- vb
- cpp
ms.openlocfilehash: cc1a879944054ce8fd2b2ea8a2f53aba4d202358
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155116"
---
# <a name="attached-properties-overview"></a>添付プロパティの概要

*添付プロパティ*は、XAML の概念です。 添付プロパティは、追加のプロパティ/値ペアをオブジェクトに設定できますが、このプロパティは元のオブジェクト定義の一部ではありません。 添付プロパティは、一般に、所有者型のオブジェクト モデルで従来のプロパティ ラッパーを持たない特殊な形式の依存関係プロパティとして定義されます。

## <a name="prerequisites"></a>前提条件

依存関係プロパティの基本的な概念を理解し、「[依存関係プロパティの概要](dependency-properties-overview.md)」を読んでいることを前提としています。

## <a name="attached-properties-in-xaml"></a>XAML での添付プロパティ

XAML では、 _AttachedPropertyProvider_構文を使用して添付プロパティを設定します。 XAML で [**Canvas.Left**](/dotnet/api/system.windows.controls.canvas.left) を設定する例を次に示します。

```xaml
<Canvas>
  <Button Canvas.Left="50">Hello</Button>
</Canvas>
```

> [!NOTE]
> ここでは、使用する理由を完全に説明せずに、添付プロパティの例として [**Canvas**](/dotnet/api/system.windows.controls.canvas.left) を使用しています。 **Canvas.Left** の用途や、[**Canvas**](/uwp/api/Windows.UI.Xaml.Controls.Canvas) がそのレイアウトの子を処理する方法について詳しくは、[**Canvas**](/uwp/api/Windows.UI.Xaml.Controls.Canvas) のリファレンス トピックまたは「[XAML を使ったレイアウトの定義](../design/layout/layouts-with-xaml.md)」をご覧ください。

## <a name="why-use-attached-properties"></a>添付プロパティを使う理由

添付プロパティは、リレーションシップ内の別々のオブジェクトが実行時に情報をやり取りするのを防ぐようなコーディング規則を回避する方法の 1 つです。 共通の基底クラスにプロパティを設定し、各オブジェクトがそのプロパティを取得、設定できるようにすることも可能です。 ただ、このようにする可能性のあるシナリオの数はきわめて多く、共有できるプロパティによって基底クラスが大きくなるおそれがあります。 何百もの子孫のうちわずか 2 つしか使わないプロパティが存在するなどという事態が発生することも考えられます。 これでは、優れたクラス設計にはなりません。 この問題に対処するため、添付プロパティの概念では、自らのクラス構造では定義されないプロパティに対してオブジェクトが値を割り当てられるようになっています。 定義クラスでは、オブジェクト ツリーで各種オブジェクトが作成された後、実行時に子オブジェクトから値を読み取ります。

たとえば、子要素では添付プロパティを使用して、子要素がどのように UI に表示されるかを親要素に通知できます。 [**Canvas.Left**](/dotnet/api/system.windows.controls.canvas.left) 添付プロパティの場合がこれに該当します。 **Canvas.Left** が添付プロパティとして作成されるのは、このプロパティが **Canvas** 自体ではなく [**Canvas**](/uwp/api/Windows.UI.Xaml.Controls.Canvas) 要素に含まれる要素に対して設定されるためです。 子要素は、**Canvas.Left** と [**Canvas.Top**](/dotnet/api/system.windows.controls.canvas.top) を使って、レイアウト オフセットを親である **Canvas** レイアウト コンテナー内で指定します。 基本の要素オブジェクト モデルに多数のプロパティがあり、それぞれのプロパティが多数のレイアウト コンテナーの 1 つのみに適用される場合でも、添付プロパティを使えば、そのオブジェクト モデルを煩雑な状態にすることなくこれを実現できます。 代わりに、レイアウト コンテナーの多くは独自の添付プロパティ セットを実装します。

添付プロパティを実装するには、[**Canvas**](/uwp/api/Windows.UI.Xaml.Controls.Canvas) クラスは、[**Canvas.LeftProperty**](/uwp/api/windows.ui.xaml.controls.canvas.leftproperty) という名前の静的な [**DependencyProperty**](/uwp/api/Windows.UI.Xaml.DependencyProperty) フィールドを定義します。 次に **Canvas** では、[**SetLeft**](/uwp/api/windows.ui.xaml.controls.canvas.setleft) メソッドと [**GetLeft**](/uwp/api/windows.ui.xaml.controls.canvas.getleft) メソッドを添付プロパティのパブリック アクセサーとして提供して、XAML の設定とランタイム値のアクセスの両方を可能にします。 XAML と依存関係プロパティ システムに対しては、この API のセットは添付プロパティ用の特定の XAML 構文を使い、依存関係プロパティ ストアに値を格納するパターンを実現できます。

## <a name="how-the-owning-type-uses-attached-properties"></a>所有する型による添付プロパティの使用方法

添付プロパティは任意の XAML 要素 (または基になる [**DependencyObject**](/uwp/api/Windows.UI.Xaml.DependencyObject)) に設定できますが、プロパティを設定することによって具体的な結果が生成されたり、値がアクセスされたりするわけではありません。 添付プロパティを定義する型は、一般に次のいずれかのシナリオに従います。

- 添付プロパティを定義する型が、他のオブジェクトのリレーションシップで親になっている。 子オブジェクトは、添付プロパティの値を設定します。 添付プロパティの所有者型には、オブジェクトの有効期間内のある時点に子要素を反復処理し、値を取得し、その値を処理するという動作が元からいくつか備わっています (レイアウト操作 [**SizeChanged**](/uwp/api/windows.ui.xaml.frameworkelement.sizechanged) など)。
- 添付プロパティを定義する型は、さまざまな親要素とコンテンツ モデルの子要素として使われますが、この情報はレイアウト情報である必要はありません。
- 添付プロパティは、別の UI 要素ではなくサービスに情報を報告します。

これらのシナリオや所有する型について詳しくは、「[カスタム添付プロパティ](custom-attached-properties.md)」の「Canvas.Left の詳細」セクションをご覧ください。

## <a name="attached-properties-in-code"></a>コードでの添付プロパティ

添付プロパティには、他の依存関係プロパティのような、取得および設定アクセスを容易にする標準的なプロパティ ラッパーがありません。 これは、添付プロパティが設定されているインスタンスのコード中心のオブジェクト モデルに、その添付プロパティが組み込まれているとは限らないからです。 (他の型で設定できると同時に、所有する型では従来の方法で使われる添付プロパティを定義することもできますが、決して一般的ではありません。)

コードでは 2 つの方法で添付プロパティを設定できます。1 つはプロパティ システムの API を使う方法、もう 1 つは XAML パターン アクセサーを使う方法です。 これらの方法は最終的な結果という点ではほぼ同じため、どちらを使うかは主にコーディング スタイルによって決まります。

### <a name="using-the-property-system"></a>プロパティ システムを使う場合

Windows ランタイムの添付プロパティは依存関係プロパティとして実装されるため、プロパティ システムを使って共有依存関係プロパティ ストアに値を格納できます。 したがって、添付プロパティは所有するクラスで依存関係プロパティ ID を公開します。

コードで添付プロパティを設定するには、[**SetValue**](/uwp/api/windows.ui.xaml.dependencyobject.setvalue) メソッドを呼び出し、その添付プロパティの ID となる [**DependencyProperty**](/uwp/api/Windows.UI.Xaml.DependencyProperty) フィールドを渡します。 (設定する値も渡します)。

コードで添付プロパティの値を取得するには、[**GetValue**](/uwp/api/windows.ui.xaml.dependencyobject.getvalue) メソッドを呼び出し、同じく ID となる [**DependencyProperty**](/uwp/api/Windows.UI.Xaml.DependencyProperty) フィールドを渡します。

### <a name="using-the-xaml-accessor-pattern"></a>XAML アクセサー パターンを使う場合

XAML をオブジェクト ツリーに解析するときは、XAML プロセッサが添付プロパティの値を設定できる必要があります。 添付プロパティの所有者型は、**Get**_PropertyName_ と **Set**_PropertyName_ の形式で名前を付けた専用のアクセサー メソッドを実装する必要があります。 この専用のアクセサー メソッドは、コードで添付プロパティを取得または設定する方法の 1 つでもあります。 コードの観点からすると、添付プロパティは、プロパティ アクセサーではなくメソッド アクセサーを持つバッキング フィールドに似ています。このバッキング フィールドは、どのオブジェクトにも存在する可能性があり、具体的に定義する必要はありません。

次の例は、コードで XAML アクセサー API を使って添付プロパティを設定する方法を示しています。 この例の `myCheckBox` は、[**CheckBox**](/uwp/api/Windows.UI.Xaml.Controls.CheckBox) クラスのインスタンスです。 実際に値を設定するコードは最後の行です。それまでの行では、インスタンスとその親子関係を設定しています。 コメント解除された最後の行は、プロパティ システムを使う場合の構文です。 コメント アウトされた最後の行は、XAML アクセサー パターンを使う場合の構文です。

```csharp
    Canvas myC = new Canvas();
    CheckBox myCheckBox = new CheckBox();
    myCheckBox.Content = "Hello";
    myC.Children.Add(myCheckBox);
    myCheckBox.SetValue(Canvas.TopProperty,75);
    //Canvas.SetTop(myCheckBox, 75);
```

```vb
    Dim myC As Canvas = New Canvas()
    Dim myCheckBox As CheckBox= New CheckBox()
    myCheckBox.Content = "Hello"
    myC.Children.Add(myCheckBox)
    myCheckBox.SetValue(Canvas.TopProperty,75)
    ' Canvas.SetTop(myCheckBox, 75)
```

```cppwinrt
Canvas myC;
CheckBox myCheckBox;
myCheckBox.Content(winrt::box_value(L"Hello"));
myC.Children().Append(myCheckBox);
myCheckBox.SetValue(Canvas::TopProperty(), winrt::box_value(75));
// Canvas::SetTop(myCheckBox, 75);
```

```cpp
    Canvas^ myC = ref new Canvas();
    CheckBox^ myCheckBox = ref new CheckBox();
    myCheckBox->Content="Hello";
    myC->Children->Append(myCheckBox);
    myCheckBox->SetValue(Canvas::TopProperty,75);
    // Canvas::SetTop(myCheckBox, 75);
```

## <a name="custom-attached-properties"></a>カスタム添付プロパティ

カスタム添付プロパティの定義方法に関するコード例と添付プロパティを使うシナリオに関する詳しい情報については、「[カスタム添付プロパティ](custom-attached-properties.md)」をご覧ください。

## <a name="special-syntax-for-attached-property-references"></a>添付プロパティ参照の特別な構文

添付プロパティ名に含まれるドットは、識別パターンの重要な部分です。 ドットが別の意味に解釈される構文または状況では、あいまいさが生じる場合があります。 たとえば、バインディング パスではドットがオブジェクト モデル トラバーサルと見なされます。 あいまいさに関してほとんどの場合、添付プロパティ用の特別な構文によって、内部のドットは添付プロパティの _owner_**.**_property_ 区切り文字として解析されています。

- 添付プロパティをアニメーション用ターゲット パスの一部として指定する場合は、添付プロパティ名をかっこ "()" で囲みます。たとえば、"(Canvas.Left)" のようにします。 詳しくは、「[Property-path 構文](property-path-syntax.md)」をご覧ください。

> [!WARNING]
> Windows ランタイムの XAML 実装の既存の制限事項として、カスタム添付プロパティをアニメーション化することはできません。

- リソースファイルから**x:Uid**へのリソース参照のターゲットプロパティとして添付プロパティを指定するには、コードスタイルを挿入する特別な構文を使用します。これには、かっこ ("") 内に、完全に修飾されたコード形式の宣言を使用し**て**、 \[ \] 意図的なスコープの区切りを作成します。 たとえば、要素が存在すると仮定すると、 `<TextBlock x:Uid="Title" />` そのインスタンスのリソースファイル内のリソース**Canvas.Top**キーが "Title \[ " になります。using: Windows. UI. .Xaml. \] Top. リソース ファイルと XAML について詳しくは、「[クイック スタート: UI リソースの翻訳](/previous-versions/windows/apps/hh965329(v=win.10))」をご覧ください。

## <a name="related-topics"></a>関連トピック

- [カスタム添付プロパティ](custom-attached-properties.md)
- [依存関係プロパティの概要](dependency-properties-overview.md)
- [XAML を使用したレイアウトの定義](../design/layout/layouts-with-xaml.md)
- [クイック スタート: UI リソースの翻訳](/previous-versions/windows/apps/hh943060(v=win.10))
- [**値**](/uwp/api/windows.ui.xaml.dependencyobject.setvalue)
- [**GetValue**](/uwp/api/windows.ui.xaml.dependencyobject.getvalue)