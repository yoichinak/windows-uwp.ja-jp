---
description: タイミング、イージング、方向性、重力など、Fluent モーションの基礎をアプリでどのように使用するかについて説明します。
title: モーションのモーション-Windows アプリでのアニメーション
label: Motion in practice
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: jeffarn
doc-status: Draft
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: cebc072a7b358aedfdd2320fa47f238712d7ee92
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/24/2020
ms.locfileid: "91220405"
---
# <a name="bringing-it-together"></a>まとめる

タイミング、イージング、方向、および重力は、連携して Fluent モーションの基礎となります。 それぞれが、互いのコンテキストで考慮され、アプリのコンテキストで適切に適用される必要があります。

アプリに Fluent モーションの基礎を適用する 3 つの方法を次に示します。

:::row:::
    :::column:::
**暗黙的なアニメーション** 標準の値を使用して非常に単純な Fluent モーションを実現するために、パラメーターの値の間の自動トゥイーンとタイミングを変更します。
    :::column-end:::
    :::column:::
**組み込みのアニメーション** 一般的なコントロールや共有モーションなどのシステムコンポーネントは、"既定では" Fluent です。 基本は、暗黙的な使用法と一貫性のある方法で適用されています。
    :::column-end:::
    :::column:::
**ガイダンス推奨事項に従うカスタムアニメーション** お客様のシナリオに対して、システムが正確なモーションソリューションを提供していない場合があります。 そのような場合は、エクスペリエンスの出発点としてベースラインの基本的な推奨事項を使用します。
    :::column-end:::
:::row-end:::

**切り替えの例**

![機能的なアニメーション](images/pageRefresh.gif)

:::row:::
    :::column:::
<b>転送方向:</b><br>
フェードアウト: 150 m;イージング: 既定 <b>の高速転送方向:</b><br>
スライドアップ 150 px: 300ms;イージング: 既定の減速
    :::column-end:::
    :::column:::
<b>逆方向:</b><br>
スライドダウン 150 px: 150 ミリ秒。イージング: 既定 <b>で後方方向に加速します。</b><br>
フェードイン: 300 ミリ秒。イージング: 既定の減速
    :::column-end:::
:::row-end:::

**オブジェクトの例**

 ![300 ミリ秒のモーション](images/control.gif)

:::row:::
    :::column:::
<b>方向の展開:</b><br>
拡張: 300ms;イージング: Standard
    :::column-end:::
    :::column:::
<b>方向のコントラクト:</b><br>
拡張: 150 ミリ秒。イージング: 既定の加速
    :::column-end:::
:::row-end:::

## <a name="examples"></a>例

<table>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML コントロールギャラリー</strong>アプリがインストールされている場合は、ここをクリックして<a href="xamlcontrolsgallery:/item/ImplicitTransition">アプリを開き、暗黙の遷移が動作</a>することを確認します。</p>
    <ul>
    <li><a href="https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt">XAML コントロール ギャラリー アプリを入手する (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">ソース コード (GitHub) を入手する</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="implicit-animations"></a>暗黙のアニメーション

> 暗黙のアニメーションには、Windows 10 バージョン 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) 以降が必要です。

暗黙のアニメーションは、パラメーターの変更時に新旧の値を自動的に補間することで、Fluent モーションを実現するための簡単な方法です。

次のプロパティに対する変更を暗黙的にアニメーション化できます。

- [UIElement](/uwp/api/windows.ui.xaml.uielement)
  - **Opacity**
  - **回転**
  - **スケール**
  - **翻訳**

- [Border](/uwp/api/windows.ui.xaml.controls.border)、 [ContentPresenter](/uwp/api/windows.ui.xaml.controls.contentpresenter)、または [Panel](/uwp/api/windows.ui.xaml.controls.panel)
  - **バックグラウンド**

暗黙的にアニメーション化された変更が可能な各プロパティには、対応する _transition_ プロパティがあります。 プロパティをアニメーション化するには、対応する _transition_ プロパティに遷移の種類を割り当てます。 次の表は、 _遷移_ プロパティと、それぞれに使用する遷移の種類を示しています。

| アニメーション化プロパティ | Transition プロパティ | 暗黙の移行の種類 |
| -- | -- | -- |
| [UIElement。不透明度](/uwp/api/windows.ui.xaml.uielement.opacity) | [OpacityTransition](/uwp/api/windows.ui.xaml.uielement.opacitytransition) | [ScalarTransition](/uwp/api/windows.ui.xaml.scalartransition) |
| [UIElement.Rotation](/uwp/api/windows.ui.xaml.uielement.rotation) | [RotationTransition](/uwp/api/windows.ui.xaml.uielement.rotationtransition) | [ScalarTransition](/uwp/api/windows.ui.xaml.scalartransition) |
| [UIElement.Scale](/uwp/api/windows.ui.xaml.uielement.scale) | [ScaleTransition](/uwp/api/windows.ui.xaml.uielement.scaletransition) | [Vector3Transition](/uwp/api/windows.ui.xaml.vector3transition) |
| [UIElement.Translation](/uwp/api/windows.ui.xaml.uielement.translation) | [TranslationTransition](/uwp/api/windows.ui.xaml.uielement.translationtransition) | [Vector3Transition](/uwp/api/windows.ui.xaml.vector3transition) |
| [罫線の背景](/uwp/api/windows.ui.xaml.controls.border.background) | [BackgroundTransition](/uwp/api/windows.ui.xaml.controls.border.backgroundtransition) | [BrushTransition](//uwp/api/windows.ui.xaml.uielement.brushtransition) |
| [ContentPresenter](/uwp/api/windows.ui.xaml.controls.contentpresenter.background) | [BackgroundTransition](/uwp/api/windows.ui.xaml.controls.contentpresenter.backgroundtransition) | [BrushTransition](//uwp/api/windows.ui.xaml.uielement.brushtransition) |
| [パネルの背景](/uwp/api/windows.ui.xaml.controls.panel.background) | [BackgroundTransition](/uwp/api/windows.ui.xaml.controls.panel.backgroundtransition)  | [BrushTransition](//uwp/api/windows.ui.xaml.uielement.brushtransition) |

この例では、不透明度プロパティと遷移を使用して、コントロールが有効になったときにボタンをフェードインし、無効になったときにフェードアウトする方法を示します。

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

- [モーションの概要](index.md)
- [タイミングとイージング](timing-and-easing.md)
- [方向性と重力](directionality-and-gravity.md)
