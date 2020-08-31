---
description: 定義済みリソースへの参照を評価することで、任意の XAML 属性の値を提供します。 リソースは ResourceDictionary で定義されており、StaticResource の使用では ResourceDictionary 内のそのリソースのキーを参照します。
title: StaticResource マークアップ拡張
ms.assetid: D50349B5-4588-4EBD-9458-75F629CCC395
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 32d09d4366d5ee05fe9581c4c8076811cfa0436f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155006"
---
# <a name="staticresource-markup-extension"></a>{StaticResource} マークアップ拡張


定義済みリソースへの参照を評価することで、任意の XAML 属性の値を提供します。 リソースは [**ResourceDictionary**](/uwp/api/Windows.UI.Xaml.ResourceDictionary) で定義されており、**StaticResource** の使用では **ResourceDictionary** 内のそのリソースのキーを参照します。

## <a name="xaml-attribute-usage"></a>XAML 属性の使用方法

``` syntax
<object property="{StaticResource key}" .../>
```

## <a name="xaml-values"></a>XAML 値

| 期間 | 説明 |
|------|-------------|
| key | 要求されたリソースのキー。 このキーは、[**ResourceDictionary**](/uwp/api/Windows.UI.Xaml.ResourceDictionary) によって最初に割当てられます。 リソース キーとしては、XamlName の文法で定義されている任意の文字列を使うことができます。 |

## <a name="remarks"></a>注釈

**StaticResource** は、XAML リソース ディクショナリ内の別の場所で定義されている XAML 属性の値を取得するための手法です。 値がリソース ディクショナリに格納される理由としては、複数のプロパティ値で共有されることや、XAML リソース ディクショナリが XAML パッケージングまたはファクタリング手法として使われることが挙げられます。 XAML パッケージング手法の例は、コントロールのテーマ ディクショナリです。 もう 1 つの例は、リソースのフォールバックに使われるマージされたリソース ディクショナリです。

**StaticResource** は、要求されたリソースについてキーを指定する 1 個の引数を受け取ります。 リソース キーは常に、Windows ランタイム XAML の文字列です。 リソース キーを最初に指定する方法について詳しくは、「[x:Key 属性](x-key-attribute.md)」をご覧ください。

**StaticResource** がリソース ディクショナリの項目に解決される規則は、このトピックでは説明していません。 そのような規則は、参照とリソースがいずれもテンプレートに存在するかどうかや、マージされたリソース ディクショナリを使うかどうかなどによって異なります。 リソースの定義方法と [**ResourceDictionary**](/uwp/api/Windows.UI.Xaml.ResourceDictionary) の適切な使用方法 (サンプル コードを含む) について詳しくは、「[ResourceDictionary と XAML リソースの参照](../design/controls-and-patterns/resourcedictionary-and-xaml-resource-references.md)」をご覧ください。

**重要**   **StaticResource**は、XAML ファイル内でさらに構文的に定義されているリソースへの前方参照を試行することはできません。 そのようなことを試みることはサポートしていません。 前方参照が失敗しなかったとしても、そのようなことを試みるだけでパフォーマンスの低下を招きます。 最善の結果を得るには、前方参照を避けるようにリソース ディクショナリの構成を調整します。

解決できないキーに **StaticResource** を指定しようとすると、実行時に XAML の解析で例外が発生します。 デザイン ツールでも、警告やエラーが通知されることがあります。

Windows ランタイム XAML プロセッサの実装では、**StaticResource** 機能のバッキング クラス表現はありません。 **StaticResource** は、XAML マークアップでのみ使用できます。 コードで最も近いのは、[**Contains**](/uwp/api/windows.ui.xaml.resourcedictionary.contains) または [**TryGetValue**](/uwp/api/windows.ui.xaml.resourcedictionary.trygetvalue) を呼び出すなど、[**ResourceDictionary**](/uwp/api/Windows.UI.Xaml.ResourceDictionary) のコレクション API を使うことです。

[{ThemeResource} マークアップ拡張](themeresource-markup-extension.md)は、別の場所の名前付きリソースを参照する同様のマークアップ拡張です。 違いは、{ThemeResource} マークアップ拡張ではアクティブなシステム テーマに応じて異なるリソースを返すことができるという点です。 詳しくは、「[{ThemeResource} マークアップ拡張](themeresource-markup-extension.md)」をご覧ください。

**StaticResource** は、マークアップ拡張です。 一般にマークアップ拡張機能を実装するのは、属性値をリテラル値やハンドラー名以外にエスケープする要件が存在し、その要件の適用範囲がグローバルで、特定の型やプロパティに型コンバーターを適用するだけにとどまらない場合です。 XAML のすべてのマークアップ拡張 \{ は、属性構文で "" と "" の文字を使用します \} 。これは、マークアップ拡張機能が属性を処理する必要があることを xaml プロセッサが認識する規則です。

### <a name="an-example-staticresource-usage"></a>{StaticResource} の使用例

この XAML 例は、[XAML データ バインディング サンプルに関するページ](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlBind)から抜粋したものです。

```xml
<StackPanel Margin="5">
    <!-- Add converter as a resource to reference it from a Binding. --> 
    <StackPanel.Resources>
        <local:S2Formatter x:Key="GradeConverter"/>
    </StackPanel.Resources>
    <TextBlock Style="{StaticResource BasicTextStyle}" Text="Percent grade:" Margin="5" />
    <Slider x:Name="sliderValueConverter" Minimum="1" Maximum="100" Value="70" Margin="5"/>
    <TextBlock Style="{StaticResource BasicTextStyle}" Text="Letter grade:" Margin="5"/>
    <TextBox x:Name="tbValueConverterDataBound"
      Text="{Binding ElementName=sliderValueConverter, Path=Value, Mode=OneWay,  
        Converter={StaticResource GradeConverter}}" Margin="5" Width="150"/> 
</StackPanel> 
```

この例では、カスタム クラスによってサポートされるオブジェクトを、[**ResourceDictionary**](/uwp/api/Windows.UI.Xaml.ResourceDictionary) のリソースとして作成しています。 有効なリソースにするには、ほかにも、この `local:S2Formatter` 要素に **x:Key** 属性値が必要です。 属性値は "GradeConverter" に設定されます。

XAML でもう少し先に進むと、リソースが要求されています。`{StaticResource GradeConverter}` というところです。

{StaticResource} マークアップ拡張を使って、別のマークアップ拡張 [{Binding} マークアップ拡張](binding-markup-extension.md)のプロパティを設定していることに注意してください。つまり、ここではマークアップ拡張が 2 つ、入れ子構造で使われています。 内側のものが最初に評価されます。このため、リソースが取得された後、値として使われます。 これと同じ例は、「{Binding} マークアップ拡張」にも記載してあります。

## <a name="design-time-tools-support-for-the-staticresource-markup-extension"></a>設計時ツールの **{StaticResource}** マークアップ拡張のサポート

Microsoft Visual Studio 2013 では、XAML ページで **{StaticResource}** マークアップ拡張を使うときに Microsoft IntelliSense のドロップダウンに可能なキー値を含めることができます。 たとえば、"{StaticResource" と入力するとすぐに、現在の検索範囲のリソース キーが IntelliSense のドロップダウンに表示されます。 ページ レベル ([**FrameworkElement.Resources**](/uwp/api/windows.ui.xaml.frameworkelement.resources)) とアプリ レベル ([**Application.Resources**](/uwp/api/windows.ui.xaml.application.resources)) で持つ一般的なリソースに加えて、[XAML テーマ リソース](../design/controls-and-patterns/xaml-theme-resources.md)と、プロジェクトで使っている拡張機能のリソースも表示されます。

**{StaticResource}** の一部としてリソース キーが存在すると、**[定義へ移動]** (F12 キー) 機能でそのリソースを解決して、リソースが定義されているディクショナリを表示できます。 テーマ リソースの場合は、設計時の generic.xaml に移動します。

## <a name="related-topics"></a>関連トピック

* [ResourceDictionary と XAML リソースの参照](../design/controls-and-patterns/resourcedictionary-and-xaml-resource-references.md)
* [**ResourceDictionary**](/uwp/api/Windows.UI.Xaml.ResourceDictionary)
* [x:Key 属性](x-key-attribute.md)
* [{ThemeResource} マークアップ拡張](themeresource-markup-extension.md)