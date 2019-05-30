---
description: ランタイム オブジェクト グラフの相対関係に関するバインドのソースを指定する手段を提供します。
title: RelativeSource マークアップ拡張
ms.assetid: B87DEF36-BE1F-4C16-B32E-7A896BD09272
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 716ca61fc9925846377157d215ca3326191915b7
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/29/2019
ms.locfileid: "66371145"
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

| 用語 | 説明 |
|------|-------------|
| {RelativeSource Self} | <strong>Self</strong> の [<strong>Mode</strong>](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.relativesource.mode) 値を生成します。 ターゲット要素をこのバインドのソースとして使う必要があります。 要素の 1 つのプロパティを同じ要素の別のプロパティにバインドする場合に便利です。 |
| {RelativeSource TemplatedParent} | このバインドのソースとして適用される [<strong>ControlTemplate</strong>](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate) を生成します。 ランタイム情報をテンプレート レベルでバインドに適用する場合に便利です。 | 

## <a name="remarks"></a>注釈

[  **Binding**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.Binding) では、**Binding** オブジェクト要素の属性または [{Binding} マークアップ拡張](binding-markup-extension.md)内のコンポーネントとして [**Binding.RelativeSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.relativesource) を設定できます。 これが、2 つの異なる XAML 構文が示されている理由です。

**RelativeSource** は [{Binding} マークアップ拡張](binding-markup-extension.md)に似ています。  具体的には、自分自身のインスタンスを返すことができ、基本的に引数をコンストラクターに渡す文字列ベースの構築をサポートできるマークアップ拡張機能であるという点です。 この場合、渡される引数は [**Mode**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.relativesource.mode) 値になります。

**Self** モードは、要素の 1 つのプロパティを同じ要素の別のプロパティにバインドする場合に便利です。これは、要素の名前の指定の後に自己参照を必要としない、[**ElementName**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.elementname) バインドの 1 つのバリエーションです。 要素の 1 つのプロパティを同じ要素の別のプロパティにバインドする場合、プロパティで同じプロパティの型を使うか、またはバインドに対して [**Converter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converter) を使って値を変換する必要があります。 たとえば、[**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) は変換せずに [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) のソースとして使用できますが、[**Visibility**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.isenabled) のソースとして [**IsEnabled**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Visibility) を使う場合には、コンバーターが必要です。

次に例を示します。 この [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) は [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) と [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) が常に等しく、正方形として表示されるように [{Binding} マークアップ拡張](binding-markup-extension.md) を使います。 Height のみが固定値として設定されます。 この **Rectangle** の既定の[**DataContext**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.datacontext) は**これ**ではなく **null** です。 そこで、データ コンテキストのソースをオブジェクト自体にするために (そして他のプロパティにバインドできるようにするために)、{Binding} マークアップ拡張の使用時に `RelativeSource={RelativeSource Self}` 引数を使います。

```XML
<Rectangle
  Fill="Orange" Width="200"
  Height="{Binding RelativeSource={RelativeSource Self}, Path=Width}"
/>
```

オブジェクトの [**DataContext**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.datacontext) を自分自身に設定する方法として `RelativeSource={RelativeSource Self}` を使うこともできます。  たとえば、いくつかの SDK の例では、この手法が生じる場所、 [**ページ**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page)クラスは、独自のデータ バインディングのすぐにビュー モデルを既に提供しているカスタム プロパティを使用して拡張されています例を示します。 `<common:LayoutAwarePage ... DataContext="{Binding DefaultViewModel, RelativeSource={RelativeSource Self}}">`

**注**  、XAML の使用状況の**RelativeSource**の対象となる使用量のみを示しています: の値を設定[ **Binding.RelativeSource** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.relativesource)バインディング式の一部としての XAML でします。 ただし、値が [**RelativeSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.RelativeSource) のプロパティを設定する場合は、理論的にそれ以外の使用法も可能です。

## <a name="related-topics"></a>関連トピック

* [XAML の概要](xaml-overview.md)
* [データ バインディングの詳細](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth)
* [{0} バインド} マークアップ拡張機能](binding-markup-extension.md)
* [**バインド**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.Binding)
* [**RelativeSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.RelativeSource)

