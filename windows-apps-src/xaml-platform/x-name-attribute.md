---
description: コード ビハインドまたは一般的なコードからインスタンス化されたオブジェクトにアクセスするために、オブジェクト要素を一意に識別します。
title: xName 属性
ms.assetid: 4FF1F3ED-903A-4305-B2BD-DCD29E0C9E6D
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b5fd9c2b482eb79fcdace2c068afa21bad673de2
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89161676"
---
# <a name="xname-attribute"></a>x:Name 属性


コード ビハインドまたは一般的なコードからインスタンス化されたオブジェクトにアクセスするために、オブジェクト要素を一意に識別します。 基になるプログラミング モデルに適用後の **x:Name** は、コンストラクターによって返されるオブジェクト参照を保持する変数と等価であると見なすことができます。

## <a name="xaml-attribute-usage"></a>XAML 属性の使用方法

``` syntax
<object x:Name="XAMLNameValue".../>
```

## <a name="xaml-values"></a>XAML 値

| 期間 | 説明 |
|------|-------------|
| XAMLNameValue | XamlName の文法の制限に準拠する文字列。 |

##  <a name="xamlname-grammar"></a>XamlName の文法

XAML 実装でキーとして使われる文字列の規範となる文法を次に示します。

``` syntax
XamlName ::= NameStartChar (NameChar)*
NameStartChar ::= LetterCharacter | '_'
NameChar ::= NameStartChar | DecimalDigit
LetterCharacter ::= ('a'-'z') | ('A'-'Z')
DecimalDigit ::= '0'-'9'
CombiningCharacter::= none
```

-   文字は、小文字の ASCII の範囲に制限されています。具体的には、大文字と小文字、数字、およびアンダースコア () 文字がアルファベット順に \_ なります。
-   Unicode 文字範囲はサポートされていません。
-   名前の先頭を数字にすることはできません。 一部のツール実装では、 \_ ユーザーが最初の文字として数字を入力すると、文字列にアンダースコア () が付加されます。また、ツールは、数字を含む他の値に基づいて **x:Name** 値を自動生成します。

## <a name="remarks"></a>注釈

指定した **x:Name** は、XAML が処理されるときに基になるコードで生成されるフィールドの名前となり、そのフィールドにオブジェクトへの参照が保持されます。 このフィールドの作成は、MSBuild ターゲットの手順で実行されます。この手順で、XAML ファイルの部分クラスとそのコード ビハインドも加えられます。 この動作は、必ずしも XAML 言語で指定されているわけではなく、XAML のユニバーサル Windows プラットフォーム (UWP) のプログラミング モデルやアプリケーション モデルで **x:Name** を使うために適用されている特別な実装です。

定義されている各 **x:Name** は、XAML 名前スコープ内で一意である必要があります。 XAML 名前スコープは一般に、読み込まれたページのルート要素レベルで定義され、1 つの XAML ページ内のルート要素の下にすべての要素が含まれます。 それ以外の XAML 名前スコープは、そのページで定義されているコントロール テンプレートやデータ テンプレートで定義されます。 実行時には、適用されたコントロール テンプレートから作成されるオブジェクト ツリーのルートにまた別の XAML 名前スコープが作成されます。この XAML 名前空間はほかにも、[**XamlReader.Load**](/uwp/api/windows.ui.xaml.markup.xamlreader.load) の呼び出しで作成されるオブジェクト ツリーによっても作成されます。 詳しくは、「[XAML 名前スコープ](xaml-namescopes.md)」をご覧ください。

デザイン ツールでは、要素をデザイン サーフェイスに取り込むと要素の **x:Name** の値が自動生成されることがよくあります。 自動生成方式は使っているデザイナーによって異なりますが、一般的な方式では、要素をサポートするクラス名で始まり、その後に増加していく整数が続く文字列が生成されます。 たとえば、最初の [**Button**](/uwp/api/Windows.UI.Xaml.Controls.Button) 要素をデザイナーに取り込むと、XAML ではこの要素の **x:Name** 属性の値が Button1 になります。

**x:Name** は、XAML プロパティ要素構文で設定することも、[**SetValue**](/uwp/api/windows.ui.xaml.dependencyobject.setvalue) を使うコードで設定することもできません。 **x:Name** は、要素の XAML 属性構文を使うことでのみ設定できます。

**メモ**   特に C++/CX アプリの場合、 **x:Name**参照のバッキングフィールドは、XAML ファイルまたはページのルート要素に対して作成されません。 C++ のコード ビハインドからルート オブジェクトを参照する必要がある場合は、他の API またはツリー走査を使ってください。 たとえば、既知の名前付き子要素に対して [**FindName**](/uwp/api/windows.ui.xaml.frameworkelement.findname) を呼び出した後、[**Parent**](/uwp/api/windows.ui.xaml.frameworkelement.parent) を呼び出します。

### <a name="xname-and-other-name-properties"></a>x:Name などの Name プロパティ

UWP XAML で使われる一部の型にも、**Name** という名前のプロパティがあります。 [**FrameworkElement.Name**](/uwp/api/windows.ui.xaml.frameworkelement.name)、[**TextElement.Name**](/uwp/api/windows.ui.xaml.documents.textelement.name) などです。

要素で設定可能なプロパティとして **Name** が使用できる場合、XAML では **Name** と **x:Name** のどちらも使うことができますが、両方の属性を同じ要素で指定するとエラーが発生します。 また、**Name** プロパティがあるものの、読み取り専用であるという場合もあります ([**VisualState.Name**](/uwp/api/windows.ui.xaml.visualstate.name) など)。 そのような場合には、XAML 内の要素に名前を付けるときには常に **x:Name** を使います。読み取り専用の **Name** は、それほど一般的ではないコードのシナリオのために存在します。

**注:**  [**FrameworkElement.Name**](/uwp/api/windows.ui.xaml.frameworkelement.name) は通常、**x:Name** で設定された値を変更するときには使いませんが、この原則の例外となるシナリオもあります。 一般的なシナリオでは、XAML 名前スコープの作成と定義は XAML プロセッサの操作です。 **FrameworkElement.Name** を実行時に変更すると、XAML 名前スコープとプライベート フィールドの名前付けの調整の整合性が損なわれ、コード ビハインドで追跡するのが難しくなる可能性があります。

### <a name="xname-and-xkey"></a>x:Name と x:Key

[**ResourceDictionary**](/uwp/api/Windows.UI.Xaml.ResourceDictionary) 内の要素に属性として **x:Name** を適用すると、[x:Key 属性](x-key-attribute.md)の代わりとしての役割を果たします (**ResourceDictionary** 内のどの要素にも x:Key または x:Name 属性があるというのが規則です)。これは、[ストーリーボードに設定されたアニメーション](../design/motion/storyboarded-animations.md)によく見られます。 詳しくは、「[ResourceDictionary と XAML リソースの参照](../design/controls-and-patterns/resourcedictionary-and-xaml-resource-references.md)」の各セクションをご覧ください。