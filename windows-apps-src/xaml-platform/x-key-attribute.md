---
description: リソースとして作成されて参照される、ResourceDictionary 内に存在する要素を一意に識別します。
title: xKey 属性
ms.assetid: 141FC5AF-80EE-4401-8A1B-17CB22C2277A
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: cb42fcb17cfcad76989732b1a1482d9fbc85be5e
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372326"
---
# <a name="xkey-attribute"></a>x:Key 属性


リソースとして作成されて参照される、[**ResourceDictionary**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary) 内に存在する要素を一意に識別します。

## <a name="xaml-attribute-usage"></a>XAML 属性の使用方法

``` syntax
<ResourceDictionary>
  <object x:Key="stringKeyValue".../>
</ResourceDictionary>
```

## <a name="xaml-attribute-usage-implicit-resourcedictionary"></a>XAML 属性の使用方法 (暗黙的な **ResourceDictionary**)

``` syntax
<object.Resources>
  <object x:Key="stringKeyValue".../>
</object.Resources>
```

## <a name="xaml-values"></a>XAML 値

| 用語 | 説明 |
|------|-------------|
| オブジェクト | 共有可能な任意のオブジェクト。 「[ResourceDictionary と XAML リソースの参照](https://docs.microsoft.com/windows/uwp/controls-and-patterns/resourcedictionary-and-xaml-resource-references)」をご覧ください。 |
| stringKeyValue | _XamlName_> の文法に準拠する必要がある、キーとして使われる実際の文字列。 以下の「XamlName の文法」をご覧ください。 | 

##  <a name="xamlname-grammar"></a>XamlName の文法

ユニバーサル Windows プラットフォーム (UWP) の XAML 実装でキーとして使われる文字列の規範となる文法を次に示します。

``` syntax
XamlName ::= NameStartChar (NameChar)*
NameStartChar ::= LetterCharacter | '_'
NameChar ::= NameStartChar | DecimalDigit
LetterCharacter ::= ('a'-'z') | ('A'-'Z')
DecimalDigit ::= '0'-'9'
CombiningCharacter::= none
```

-   文字が ASCII の下の範囲はローマ字の大文字と小文字の英字、数字、およびアンダー スコアを具体的には制限されている (\_) 文字。
-   Unicode 文字範囲はサポートされていません。
-   名前の先頭を数字にすることはできません。

## <a name="remarks"></a>注釈

通常、[**ResourceDictionary**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary) の子要素は、そのディクショナリ内の一意のキー値を指定する **x:Key** 属性を含みます。 キーは、読み込み時に XAML プロセッサによって一意であることが要求されます。 **x:Key** 値が一意でない場合は、XAML の解析で例外が発生します。 [{StaticResource} マークアップ拡張](staticresource-markup-extension.md)によって要求された場合、解決されていないキーも XAML の解析での例外の原因になります。

**x:Key** と [x:Name](x-name-attribute.md) は同じ概念ではありません。 **x:Key** はリソース ディクショナリだけで使われます。 x:Name は、すべての XAML 領域で使われます。 キー値を使って [**FindName**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.findname) を呼び出した場合、キーを持つリソースは取得されません。 リソース ディクショナリで定義されているオブジェクトは、**x:Key** と **x:Name** のどちらか一方または両方を持つことができます。 キーと名前が一致する必要はありません。

ここに示す暗黙的な構文において、[**ResourceDictionary**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary) オブジェクトは XAML プロセッサが [**Resources**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.resources) コレクションを取得するための新しいオブジェクトを生成する方法を暗黙的に決定することに注意してください。

**x:Key** の指定に相当するコードは、基になる [**ResourceDictionary**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary) でキーを使う任意の操作です。 たとえば、あるリソースのマークアップで適用される **x:Key** は、リソースを **ResourceDictionary** に追加するときの **Insert** の *key* パラメーターの値と同じです。

リソース ディクショナリ内の項目は、ターゲットとなる [**Style**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Style) または [**ControlTemplate**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate) である場合、**x:Key** の値を省略できます。どちらの場合も、リソース項目の暗黙的なキーは、文字列として解釈される **TargetType** 値です。 詳しくは、「[クイック スタート: コントロールのスタイル (JavaScript と HTML を使った Windows ストア アプリ)](https://docs.microsoft.com/previous-versions/windows/apps/hh465498(v=win.10))」と「[ResourceDictionary と XAML リソースの参照](https://docs.microsoft.com/windows/uwp/controls-and-patterns/resourcedictionary-and-xaml-resource-references)」をご覧ください。

