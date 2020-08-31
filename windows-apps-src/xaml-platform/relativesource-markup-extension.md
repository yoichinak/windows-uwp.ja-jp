---
description: ランタイム オブジェクト グラフの相対関係に関するバインドのソースを指定する手段を提供します。
title: RelativeSource マークアップ拡張
ms.assetid: B87DEF36-BE1F-4C16-B32E-7A896BD09272
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 383e7b0576b5462879f903b684c332b35ee7d756
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155026"
---
# <a name="relativesource-markup-extension"></a>{RelativeSource} マークアップ拡張


ランタイム オブジェクト グラフの相対関係に関するバインドのソースを指定する手段を提供します。

## <a name="xaml-attribute-usage-self-mode"></a>XAML 属性の使用方法 (Self モード)

``` syntax
<Binding RelativeSource="{RelativeSource Self}" .../>
-or-
<object property="{Binding RelativeSource={RelativeSource Self} ...}" .../>
```

## <a name="xaml-attribute-usage-templatedparent-mode"></a>XAML 属性の使用方法 (TemplatedParent モード)

``` syntax
<Binding RelativeSource="{RelativeSource TemplatedParent}" .../>
-or-
<object property="{Binding RelativeSource={RelativeSource TemplatedParent} ...}" .../>
```

## <a name="xaml-values"></a>XAML 値

| 期間 | 説明 |
|------|-------------|
| {RelativeSource Self} | <strong>Self</strong> の [<strong>Mode</strong>](/uwp/api/windows.ui.xaml.data.relativesource.mode) 値を生成します。 ターゲット要素をこのバインドのソースとして使う必要があります。 要素の 1 つのプロパティを同じ要素の別のプロパティにバインドする場合に便利です。 |
| {RelativeSource TemplatedParent} | このバインドのソースとして適用される [<strong>ControlTemplate</strong>](/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate) を生成します。 ランタイム情報をテンプレート レベルでバインドに適用する場合に便利です。 | 

## <a name="remarks"></a>注釈

[**Binding**](/uwp/api/Windows.UI.Xaml.Data.Binding) では、**Binding** オブジェクト要素の属性または [{Binding} マークアップ拡張](binding-markup-extension.md)内のコンポーネントとして [**Binding.RelativeSource**](/uwp/api/windows.ui.xaml.data.binding.relativesource) を設定できます。 これが、2 つの異なる XAML 構文が示されている理由です。

**RelativeSource** は [{Binding} マークアップ拡張](binding-markup-extension.md)に似ています。  具体的には、自分自身のインスタンスを返すことができ、基本的に引数をコンストラクターに渡す文字列ベースの構築をサポートできるマークアップ拡張機能であるという点です。 この場合、渡される引数は [**Mode**](/uwp/api/windows.ui.xaml.data.relativesource.mode) 値になります。

**Self** モードは、要素の 1 つのプロパティを同じ要素の別のプロパティにバインドする場合に便利です。これは、要素の名前の指定の後に自己参照を必要としない、[**ElementName**](/uwp/api/windows.ui.xaml.data.binding.elementname) バインドの 1 つのバリエーションです。 要素の 1 つのプロパティを同じ要素の別のプロパティにバインドする場合、プロパティで同じプロパティの型を使うか、またはバインドに対して [**Converter**](/uwp/api/windows.ui.xaml.data.binding.converter) を使って値を変換する必要があります。 たとえば、[**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) は変換せずに [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) のソースとして使用できますが、[**Visibility**](/uwp/api/windows.ui.xaml.controls.control.isenabled) のソースとして [**IsEnabled**](/uwp/api/Windows.UI.Xaml.Visibility) を使う場合には、コンバーターが必要です。

次に例を示します。 この [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) は [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) と [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) が常に等しく、正方形として表示されるように [{Binding} マークアップ拡張](binding-markup-extension.md) を使います。 Height のみが固定値として設定されます。 この **Rectangle** の既定の[**DataContext**](/uwp/api/windows.ui.xaml.frameworkelement.datacontext) は**これ**ではなく **null** です。 そこで、データ コンテキストのソースをオブジェクト自体にするために (そして他のプロパティにバインドできるようにするために)、{Binding} マークアップ拡張の使用時に `RelativeSource={RelativeSource Self}` 引数を使います。

```XML
<Rectangle
  Fill="Orange" Width="200"
  Height="{Binding RelativeSource={RelativeSource Self}, Path=Width}"
/>
```

オブジェクトの [**DataContext**](/uwp/api/windows.ui.xaml.frameworkelement.datacontext) を自分自身に設定する方法として `RelativeSource={RelativeSource Self}` を使うこともできます。  たとえば、次のような独自のデータバインディングのための準備完了のビューモデルを既に提供しているカスタムプロパティを使用して、 [**ページ**](/uwp/api/Windows.UI.Xaml.Controls.Page) クラスが拡張されている SDK の例では、この手法を使用できます。 `<common:LayoutAwarePage ... DataContext="{Binding DefaultViewModel, RelativeSource={RelativeSource Self}}">`

**メモ**   **RelativeSource**の xaml の使用方法では、バインド式の一部として Xaml の[**RelativeSource**](/uwp/api/windows.ui.xaml.data.binding.relativesource)に値を設定することによって、意図した用途のみが表示されます。 ただし、値が [**RelativeSource**](/uwp/api/Windows.UI.Xaml.Data.RelativeSource) のプロパティを設定する場合は、理論的にそれ以外の使用法も可能です。

## <a name="related-topics"></a>関連トピック

* [XAML の概要](xaml-overview.md)
* [データ バインディングの詳細](../data-binding/data-binding-in-depth.md)
* [{Binding} マークアップ拡張](binding-markup-extension.md)
* [**バインド**](/uwp/api/Windows.UI.Xaml.Data.Binding)
* [**RelativeSource**](/uwp/api/Windows.UI.Xaml.Data.RelativeSource)