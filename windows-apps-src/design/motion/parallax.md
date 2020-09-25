---
title: 視差を利用してアプリに深さと移動を追加する
description: UWP アプリで ParallaxView コントロールを使用して、ビューアーに近い項目が背景の項目よりも速く移動する視覚効果を作成する方法について説明します。
ms.assetid: ''
label: Parallax View
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
pm-contact: abarlow
design-contact: conrwi
dev-contact: stpete
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 702d856ec15391875f369b6145a715ade3092afb
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/24/2020
ms.locfileid: "91220255"
---
# <a name="parallax"></a>視差

視差は、アプリの閲覧者の近くにある項目を背景にある項目よりも速く動かすという視覚効果です。 視差によって、奥行き、遠近感、および動きといった感覚が引き起こされます。 UWP アプリでは、ParallaxView コントロールを使用して、視差効果を作成できます。  

> **WINDOWS UI ライブラリ api:** [ParallaxView クラス](/uwp/api/Microsoft.UI.Xaml.Controls.Parallaxview)、 [垂直シフトプロパティ](/uwp/api/Microsoft.UI.Xaml.Controls.Parallaxview.VerticalShift)、 [水平シフトプロパティ](/uwp/api/Microsoft.UI.Xaml.Controls.Parallaxview.HorizontalShift)
>
> **プラットフォーム api**: [ParallaxView クラス](/uwp/api/Windows.UI.Xaml.Controls.Parallaxview)、 [垂直シフトプロパティ](/uwp/api/Windows.UI.Xaml.Controls.Parallaxview.VerticalShift)、 [水平シフトプロパティ](/uwp/api/Windows.UI.Xaml.Controls.Parallaxview.HorizontalShift)

## <a name="examples"></a>例

<table>
<th align="left">XAML コントロール ギャラリー<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML コントロール ギャラリー</strong> アプリがインストールされている場合、こちらをクリックして<a href="xamlcontrolsgallery:/item/ParallaxView">アプリを開き、ParallaxView の動作を確認</a>してください。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML コントロール ギャラリー アプリを入手する (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">ソース コード (GitHub) を入手する</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="parallax-and-the-fluent-design-system"></a>視差と Fluent Design System

 Fluent Design System では、ライト、深度、モーション、マテリアル、スケールを取り入れた、モダンで目を引く UI を作成できます。 視差は、アプリにモーション、深度、スケールを追加する Fluent Design System コンポーネントです。 詳しくは、[Fluent Design の概要](/windows/apps/fluent-design-system)に関するページをご覧ください。

## <a name="how-it-works-in-a-user-interface"></a>ユーザー インターフェイスでのしくみ

UI では、UI のスクロールやパンを行うときに、さまざまなオブジェクトをさまざまな速度で動かすことによって、視差効果を作成できます。 <!-- Parallax is an important tool in adding depth to applications along with other techniques like transition animations, perspective tilt, and layering. --> 例として、2つのコンテンツレイヤー、リストと背景画像を見てみましょう。  リストは背景画像の上に配置されており、リストがアプリの閲覧者の近くに表示されているという錯覚を既に与えています。  ここで、視差効果を実現するために、遠く離れているオブジェクトよりも、最も近いオブジェクトを "より速く" 移動させたいと考えています。  ユーザーがインターフェイスをスクロールすると、リストは背景画像よりも速い速度で動作し、奥行きがあるような錯覚を与えます。

 ![リストと背景画像を使用した視差の例](images/_Parallax_v2.gif)

 
## <a name="using-the-parallaxview-control-to-create-a-parallax-effect"></a>ParallaxView コントロールを使用して視差効果を作成する

視差効果を作成するには、[ParallaxView](/uwp/api/Windows.UI.Xaml.Controls.Parallaxview) コントロールを使用します。 このコントロールによって、前景要素 (リストなど) のスクロール位置が背景要素 (画像など) に関連付けられます。 前景要素をスクロールすると、背景要素がアニメーション化され、視差効果が発生します。 

ParallaxView コントロールを使用するには、ソース要素と背景要素を用意し、[VerticalShift](/uwp/api/Windows.UI.Xaml.Controls.Parallaxview.VerticalShift) プロパティ (垂直スクロール用) や [HorizontalShift](/uwp/api/Windows.UI.Xaml.Controls.Parallaxview.HorizontalShift) プロパティ (水平スクロール用) を 0 より大きい値に設定します。 
* Source プロパティは、前景要素への参照を受け取ります。 視差効果を発生させるには、前景が [ScrollViewer](/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer) であるか、前景が ScrollViewer を含んでいる要素 ([ListView](/uwp/api/windows.ui.xaml.controls.listview) や [RichTextBox](/uwp/api/Windows.UI.Xaml.Controls.RichEditBox) など) であることがj必要です。 

* 背景要素を設定するには、その要素を ParallaxView コントロールの子として追加します。 背景要素には、[Image](/uwp/api/Windows.UI.Xaml.Controls.Image) など、どのような [UIElement](/uwp/api/windows.ui.xaml.uielement) でも使用できます。また、追加の UI 要素を含んでいるパネルも使用できます。 

視差効果を作成するには、ParallaxView が背景要素を介して動作するようにする必要があります。 [Grid](/uwp/api/windows.ui.xaml.controls.grid) パネルや [Canvas](/uwp/api/windows.ui.xaml.controls.canvas) パネルを使用すると、項目を相互に重ねることができ、それらの項目は ParallaxView コントロールで適切に動作します。  

次の例では、リストの視差効果を作成します。
 
```xaml
<Grid>
    <ParallaxView Source="{x:Bind ForegroundElement}" VerticalShift="50"> 
    
        <!-- Background element --> 
        <Image x:Name="BackgroundImage" Source="Assets/turntable.png"
               Stretch="UniformToFill"/>
    </ParallaxView>
    
    <!-- Foreground element -->
    <ListView x:Name="ForegroundElement">
       <x:String>Item 1</x:String> 
       <x:String>Item 2</x:String> 
       <x:String>Item 3</x:String> 
       <x:String>Item 4</x:String> 
       <x:String>Item 5</x:String>     
       <x:String>Item 6</x:String> 
       <x:String>Item 7</x:String> 
       <x:String>Item 8</x:String> 
       <x:String>Item 9</x:String> 
       <x:String>Item 10</x:String>     
       <x:String>Item 11</x:String> 
       <x:String>Item 13</x:String> 
       <x:String>Item 14</x:String> 
       <x:String>Item 15</x:String> 
       <x:String>Item 16</x:String>     
       <x:String>Item 17</x:String> 
       <x:String>Item 18</x:String> 
       <x:String>Item 19</x:String> 
       <x:String>Item 20</x:String> 
       <x:String>Item 21</x:String>        
    </ListView>
</Grid>
```    

ParallaxView は、画像のサイズを自動的に調整して視差操作のために使用できるようにします。これにより、画像のスクロールが非表示になるのを心配する必要がなくなります。

## <a name="customizing-the-parallax-effect"></a>視差効果のカスタマイズ 

VerticalShift プロパティと HorizontalShift プロパティでは、視差効果の程度を制御できます。

* VerticalShift プロパティでは、視差効果の操作全体を通じて、背景をどの程度垂直方向に動かすかを指定します。 値が0の場合は、背景がまったく移動しないことを意味します。
* HorizontalShift プロパティでは、視差効果の操作全体を通じて、背景をどの程度水平方向に動かすかを指定します。 値が0の場合は、背景がまったく移動しないことを意味します。

値を大きくすると、劇的な効果が発生します。 

視差をカスタマイズする方法の完全な一覧については、ParallaxView クラスをご覧ください。 

## <a name="dos-and-donts"></a>推奨と非推奨

- 視差は、背景画像を持つリストで使用してください
- ListViewItems に画像が含まれている場合は、ListViewItems で視差を使用することを検討してください
- どこでも使用しないで、使いすぎてその影響を軽減することができます。

## <a name="related-articles"></a>関連記事

- [ParallaxView クラス](/uwp/api/Windows.UI.Xaml.Controls.Parallaxview) 
- [UWP 用 Fluent Design](/windows/apps/fluent-design-system)
- [システムの科学:Fluent Design と奥行き](https://medium.com/microsoft-design/science-in-the-system-fluent-design-and-depth-fb6d0f23a53f)
