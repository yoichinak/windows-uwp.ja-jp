---
description: コントロール テンプレート内のプロパティの値を、template 宣言されたコントロールのその他の公開されているプロパティの値にリンクします。 XAML では、TemplateBinding は ControlTemplate 定義内でのみ使用できます。
title: TemplateBinding マークアップ拡張
ms.assetid: FDE71086-9D42-4287-89ED-8FBFCDF169DC
ms.date: 10/29/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 3b368ca2f429d52674ba1cb3493012d54dc0848a
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89154926"
---
# <a name="templatebinding-markup-extension"></a>{TemplateBinding} マークアップ拡張

コントロール テンプレート内のプロパティの値を、template 宣言されたコントロールのその他の公開されているプロパティの値にリンクします。 **TemplateBinding** は、XAML の [**ControlTemplate**](/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate) 定義内でのみ使用できます。

## <a name="xaml-attribute-usage"></a>XAML 属性の使用方法

``` syntax
<object propertyName="{TemplateBinding sourceProperty}" .../>
```

## <a name="xaml-attribute-usage-for-setter-property-in-template-or-style"></a>XAML 属性の使用方法 (テンプレートまたはスタイルの Setter プロパティの場合)

``` syntax
<Setter Property="propertyName" Value="{TemplateBinding sourceProperty}" .../>
```

## <a name="xaml-values"></a>XAML 値

| 期間 | 説明 |
|------|-------------|
| propertyName | setter 構文で設定されるプロパティの名前。 これは依存関係プロパティであることが必要です。 |
| sourceProperty | template 宣言された型に存在する、別の依存関係プロパティの名前。 |

## <a name="remarks"></a>注釈

カスタム コントロールの作成者である場合でも、コントロール テンプレートを今あるコントロールに置き換える場合でも、コントロール テンプレートを定義するうえでは **TemplateBinding** を使うことが欠かせません。 詳しくは、「[クイック スタート: コントロール テンプレート](/previous-versions/windows/apps/hh465374(v=win.10))」をご覧ください。

*propertyName* と *targetProperty* では同じプロパティ名を使うことが一般的です。 この場合、コントロール自体でプロパティを定義し、プロパティを、そのいずれかのコンポーネントの直感的な名前を持つ既にあるプロパティに転送します。 たとえば、コントロールの独自の**Text**プロパティを表示するために使用される[**TextBlock**](/uwp/api/Windows.UI.Xaml.Controls.TextBlock)を複合に組み込むコントロールには、この XAML をコントロールテンプレートに含めることができます。`<TextBlock Text="{TemplateBinding Text}" .... />`

ソース プロパティとターゲット プロパティの値として使う型は一致する必要があります。 **TemplateBinding** を使うとコンバーターを導入する機会がありません。 値が一致しないと、XAML を解析したときにエラーが発生します。 コンバーターを必要とする場合は、次のようなテンプレート バインドの冗長な形式の構文を使うことができます。`{Binding RelativeSource={RelativeSource TemplatedParent}, Converter="..." ...}`

XAML の [**ControlTemplate**](/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate) 定義の外側で **TemplateBinding** を使うと、パーサー エラーが発生します。

親のテンプレート値も別のバインドとして延期される場合は、**TemplateBinding** を使用できます。 **TemplateBinding** の評価は、必要な実行時バインドに値が設定されるまで待機することができます。

**TemplateBinding** は常に一方向バインドです。 関連する両方のプロパティは、依存関係プロパティである必要があります。

**TemplateBinding** はマークアップ拡張です。 一般にマークアップ拡張機能を実装するのは、属性値をリテラル値やハンドラー名以外にエスケープする要件が存在し、その要件の適用範囲がグローバルで、特定の型やプロパティに型コンバーターを適用するだけにとどまらない場合です。 XAML のすべてのマークアップ拡張では、それぞれの属性構文で "{" と "}" の文字を使います。これは規約であり、これに従って XAML プロセッサは、マークアップ拡張で属性を処理する必要があることを認識します。

**メモ**   Windows ランタイム XAML プロセッサの実装では、 **TemplateBinding**のバッキングクラス表現はありません。 **TemplateBinding** は、XAML マークアップでのみ使用できます。 コードの動作を再現する方法には単純なものがありません。

### <a name="xbind-in-controltemplate"></a>ControlTemplate の x:Bind

> [!NOTE]
> ControlTemplate で x:Bind を使用するには、Windows 10 バージョン 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) 以降が必要です。 ターゲット バージョンについて詳しくは、「[バージョン アダプティブ コード](../debug-test-perf/version-adaptive-code.md)」をご覧ください。

Windows 10 バージョン1809以降では、 [**ControlTemplate**](/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate)で**TemplateBinding**を使用する任意の場所で、 **x:Bind**マークアップ拡張機能を使用できます。 

**X:Bind**を使用する場合、 [ControlTemplate](/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate)には[TargetType](/uwp/api/windows.ui.xaml.controls.controltemplate.targettype)プロパティが必要です (省略可能ではありません)。

**X:Bind**のサポートにより、 [ControlTemplate](/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate)では、[関数バインド](../data-binding/function-bindings.md)と双方向のバインディングの両方を使用できます。

この例では、 **TextBlock** プロパティは、".. **コンテンツ. ToString**" に評価されます。 ControlTemplate の TargetType はデータソースとして機能し、親の TemplateBinding と同じ結果を達成します。

```xaml
<ControlTemplate TargetType="Button">
    <Grid>
        <TextBlock Text="{x:Bind Content, Mode=OneWay}"/>
    </Grid>
</ControlTemplate>
```

## <a name="related-topics"></a>関連トピック

* [クイック スタート: コントロール テンプレート](/previous-versions/windows/apps/hh465374(v=win.10))
* [データ バインディングの詳細](../data-binding/data-binding-in-depth.md)
* [**ControlTemplate**](/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate)
* [XAML の概要](xaml-overview.md)
* [依存関係プロパティの概要](dependency-properties-overview.md)
 