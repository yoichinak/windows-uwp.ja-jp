---
Description: アプリでの基礎は一体どの Fluent モーションをについて説明します。
title: 実践的なモーション -  UWP アプリのアニメーション
label: Motion in practice
template: detail.hbs
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: jeffarn
doc-status: Draft
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 8cf010533d2d62559bb8dc0d214e04ab917e62bd
ms.sourcegitcommit: d534f81590d881a18d677a648c59913029837a84
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2019
ms.locfileid: "67535434"
---
# <a name="bringing-it-together"></a>まとめる

タイミング、イージング、方向、および重力は、連携して Fluent モーションの基礎となります。 それぞれが、互いのコンテキストで考慮され、アプリのコンテキストで適切に適用される必要があります。

アプリに Fluent モーションの基礎を適用する 3 つの方法を次に示します。

:::row:::
    :::column:::
**暗黙のアニメーション**自動トゥイーンと標準化された値を使用して、非常にシンプルな Fluent モーションを実現するためにパラメーターの変更で値の間のタイミングです。
    :::column-end:::
    :::column:::
**組み込みのアニメーション**コモン コントロールと共有のモーションなどのシステム コンポーネントが"既定では Fluent"。 暗黙的な使用状況に従った方法で基本事項が適用されました。
    :::column-end:::
    :::column:::
**ガイダンスの推奨事項に従うアニメーション**システムが提供しないときにまだモーションの正確なソリューションを自分のシナリオもあります。 その場合、お客様のエクスペリエンスの開始点としてベースラインの基本的な推奨事項を使用します。
    :::column-end:::
:::row-end:::

**移行の例**

![機能的なアニメーション](images/pageRefresh.gif)

:::row:::
    :::column:::
<b>送信メール前方方向:</b><br>
フェードアウトします。150 m です。簡略化します。既定で高速化<b>で進行方向。</b><br>
150px をスライドします。300 です。簡略化します。既定の減速させる
    :::column-end:::
    :::column:::
<b>旧バージョンと方向:</b><br>
150px 下へスライドします。150ms;簡略化します。既定で高速化<b>で旧バージョンと方向。</b><br>
フェードインします。300 です。簡略化します。既定の減速させる
    :::column-end:::
:::row-end:::

**オブジェクトの例**

 ![300 ミリ秒のモーション](images/control.gif)

:::row:::
    :::column:::
<b>方向を展開します。</b><br>
拡張します。300 です。簡略化します。Standard
    :::column-end:::
    :::column:::
<b>方向のコントラクト。</b><br>
拡張します。150ms;簡略化します。既定で高速化します。
    :::column-end:::
:::row-end:::

## <a name="examples"></a>例

<table>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>ある場合、 <strong style="font-weight: semi-bold">XAML コントロール ギャラリー</strong>アプリをインストールするには、ここをクリックして<a href="xamlcontrolsgallery:/item/ImplicitTransition">アプリを開き、操作の暗黙的な遷移を参照してください</a>します。</p>
    <ul>
    <li><a href="https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt">XAML コントロール ギャラリー アプリを入手する (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">ソース コード (GitHub) を入手する</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="implicit-animations"></a>暗黙的なアニメーション

> 暗黙的なアニメーションに必要な Windows 10、バージョンは 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) またはそれ以降。

暗黙的なアニメーションは、自動的にパラメーターの変更中に新旧の値の間を補間によって Fluent の動きを実現する簡単な方法です。

暗黙的に次のプロパティを変更をアニメーション化できます。

- [UIElement](/uwp/api/windows.ui.xaml.uielement)
  - **不透明度**
  - **回転**
  - **スケール**
  - **翻訳**

- [境界線](/uwp/api/windows.ui.xaml.controls.border)、 [ContentPresenter](/uwp/api/windows.ui.xaml.controls.contentpresenter)、または[パネル](/uwp/api/windows.ui.xaml.controls.panel)
  - **背景情報**

各プロパティをアニメーション化に暗黙的に変更を持つことができますが、対応する_遷移_プロパティ。 プロパティをアニメーション化するが移行型を割り当て、対応する_遷移_プロパティ。 このテーブルを示しています、_遷移_プロパティとそれぞれに使用する移行型。

| アニメーション化されたプロパティ | 遷移プロパティ | 暗黙的な移行の種類 |
| -- | -- | -- |
| [UIElement.Opacity](/uwp/api/windows.ui.xaml.uielement.opacity) | [OpacityTransition](/uwp/api/windows.ui.xaml.uielement.opacitytransition) | [ScalarTransition](/uwp/api/windows.ui.xaml.scalartransition) |
| [UIElement.Rotation](/uwp/api/windows.ui.xaml.uielement.rotation) | [RotationTransition](/uwp/api/windows.ui.xaml.uielement.rotationtransition) | [ScalarTransition](/uwp/api/windows.ui.xaml.scalartransition) |
| [UIElement.Scale](/uwp/api/windows.ui.xaml.uielement.scale) | [ScaleTransition](/uwp/api/windows.ui.xaml.uielement.scaletransition) | [Vector3Transition](/uwp/api/windows.ui.xaml.vector3transition) |
| [UIElement.Translation](/uwp/api/windows.ui.xaml.uielement.translation) | [TranslationTransition](/uwp/api/windows.ui.xaml.uielement.translationtransition) | [Vector3Transition](/uwp/api/windows.ui.xaml.vector3transition) |
| [Border.Background](/uwp/api/windows.ui.xaml.controls.border.background) | [BackgroundTransition](/uwp/api/windows.ui.xaml.controls.border.backgroundtransition) | [BrushTransition](//uwp/api/windows.ui.xaml.uielement.brushtransition) |
| [ContentPresenter.Background](/uwp/api/windows.ui.xaml.controls.contentpresenter.background) | [BackgroundTransition](/uwp/api/windows.ui.xaml.controls.contentpresenter.backgroundtransition) | [BrushTransition](//uwp/api/windows.ui.xaml.uielement.brushtransition) |
| [Panel.Background](/uwp/api/windows.ui.xaml.controls.panel.background) | [BackgroundTransition](/uwp/api/windows.ui.xaml.controls.panel.backgroundtransition)  | [BrushTransition](//uwp/api/windows.ui.xaml.uielement.brushtransition) |

この例では、ボタン コントロールが有効にすると、フェードインし、フェードアウトを無効にすることに、Opacity プロパティと遷移を使用する方法を示します。

```xaml
<Button x:Name="SubmitButton"
        Content="Submit"
        Opacity="{x:Bind OpaqueIfEnabled(SubmitButton.IsEnabled), Mode=OneWay}">
    <Button.OpacityTransition>
        <ScalarTransition />
    </Button.OpacityTransition>
</Button>
```

```csharp
public double OpaqueIfEnabled(bool IsEnabled)
{
    return IsEnabled ? 1.0 : 0.2;
}
```

## <a name="related-articles"></a>関連記事

- [アニメーションの概要](index.md)
- [タイミングと、簡略化](timing-and-easing.md)
- [方向性と重力](directionality-and-gravity.md)
