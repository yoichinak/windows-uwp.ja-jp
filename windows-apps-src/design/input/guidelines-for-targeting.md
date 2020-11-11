---
description: このトピックでは、タッチ補正のための接触形状の使用について説明し、Windows ランタイム アプリでのターゲット設定のベスト プラクティスを紹介します。
title: ターゲット
ms.assetid: 93ad2232-97f3-42f5-9e45-3fc2143ac4d2
label: Targeting
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c0f18feed166ea775f540a28fd873d7e81083cf0
ms.sourcegitcommit: 4fffc66fac18fc4c80281e2a4afa9c4f2e1f7551
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/11/2020
ms.locfileid: "94513691"
---
# <a name="guidelines-for-touch-targets"></a>タッチ ターゲットのガイドライン

Windows アプリケーションのすべての対話型 UI 要素は、デバイスの種類や入力方法に関係なく、ユーザーが正確にアクセスして使用できる大きさである必要があります。

タッチ入力をサポートする (また、タッチの連絡先領域の比較的不不正確な性質) には、対象のサイズとコントロールのレイアウトに関してさらに最適化が必要です。これは、タッチデジタイザーによって報告されるより複雑な入力データのセットを使用して、ユーザーが意図している (または最も可能性がある) ターゲットを特定する

すべての UWP コントロールは、既定のタッチターゲットのサイズとレイアウトで設計されています。これにより、快適で使いやすく、自信を持って、視覚的にバランスの取れた魅力的なアプリを作成できます。

このトピックでは、これらの既定の動作について説明します。これにより、プラットフォームコントロールとカスタムコントロール (アプリで必要になるもの) を使用して、使いやすさを最大限にするアプリを設計できます。

> **重要な API** : [**Windows.UI.Core**](/uwp/api/Windows.UI.Core)、 [**Windows.UI.Input**](/uwp/api/Windows.UI.Input)、 [**Windows.UI.Xaml.Input**](/uwp/api/Windows.UI.Xaml.Input)

## <a name="fluent-standard-sizing"></a>Fluent Standard サイズ

" *Fluent Standard サイズ* " は、情報の密度とユーザーの快適さのバランスを取るために作成されました。 実質的に、画面上のすべての項目が 40 x 40 の有効ピクセル (epx) ターゲットに揃えられ、UI 要素をグリッドに位置合わせし、システム レベルのスケーリングに基づいて適切にスケーリングできます

> [!NOTE]
> 有効ピクセルとスケーリングについて詳しくは、[Windows アプリ デザインの概要](../basics/design-and-ui-intro.md#effective-pixels-and-scaling)に関するページをご覧ください。
>
> システム レベルのスケーリングについて詳しくは、「[配置、余白、パディング](../layout/alignment-margin-padding.md)」をご覧ください。

## <a name="fluent-compact-sizing"></a>Fluent Compact サイズ

アプリケーションでは、 *Fluent Compact のサイズ変更* により、より高いレベルの情報密度を表示できます。 コンパクトなサイズ変更では、UI 要素を 32x32 window.epx.codesnippet ターゲットに揃えます。これにより、UI 要素をより厳密なグリッドに揃え、システムレベルのスケーリングに基づいて適切に拡大縮小できます。

### <a name="examples"></a>例

コンパクトなサイズ変更は、ページレベルまたはグリッドレベルで適用できます。

### <a name="page-level"></a>ページ レベル

```xaml
<Page.Resources>
    <ResourceDictionary Source="ms-appx:///Microsoft.UI.Xaml/DensityStyles/Compact.xaml" />
</Page.Resources>
```

### <a name="grid-level"></a>グリッド レベル

```xaml
<Grid>
    <Grid.Resources>
        <ResourceDictionary Source="ms-appx:///Microsoft.UI.Xaml/DensityStyles/Compact.xaml" />
    </Grid.Resources>
</Grid>
```

## <a name="target-size"></a>ターゲット サイズ

一般に、タッチターゲットサイズを 7.5 mm 二乗範囲に設定します (1.0 x スケーリング安定で 135 PPI ディスプレイの40x40 ピクセル)。 通常、UWP コントロールは 7.5 mm タッチターゲットに合わせて配置されます (これは、特定のコントロールと、一般的な使用パターンによって異なる場合があります)。 詳細については [、「コントロールのサイズと密度](../style/spacing.md) 」を参照してください。

表に示したターゲット サイズの推奨サイズは、個々のシナリオの必要に応じて調整できます。 次の点を考慮することをお勧めします。

- タッチの頻度-最小サイズよりも、繰り返しまたは頻繁に押されるターゲットを作成することを検討します。
- エラーの結果-エラーが発生した場合に重大な結果が得られるターゲットは、余白が大きくなり、コンテンツ領域の端からさらに配置される必要があります。 特に当てはまるのは頻繁にタッチされるターゲットです。
- コンテンツ領域内の位置。
- フォームファクターと画面サイズ。
- 指の体制。
- タッチの視覚エフェクト。

## <a name="related-articles"></a>関連記事

- [Windows アプリ デザインの概要](../basics/design-and-ui-intro.md)
- [コントロールのサイズと密度](../style/spacing.md)
- [配置、余白、パディング](../layout/alignment-margin-padding.md)

### <a name="samples"></a>サンプル

- [基本的な入力のサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
- [待機時間が短い入力のサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
- [ユーザー操作モードのサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode)
- [フォーカスの視覚効果のサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals)

### <a name="archive-samples"></a>サンプルのアーカイブ

- [入力: XAML ユーザー入力イベントのサンプルに関するページ](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20XAML%20user%20input%20events%20sample)
- [入力: デバイス機能のサンプルに関するページ](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Input%20Device%20capabilities%20sample%20(Windows%208))
- [入力: タッチのヒット テストのサンプルに関するページ](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20desktop%20samples/%5BC%2B%2B%5D-Windows%208%20desktop%20samples/C%2B%2B/Windows%208%20desktop%20samples/Input%20Touch%20hit%20testing%20sample)
- [XAML のスクロール、パン、ズームのサンプル](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Universal%20Windows%20app%20samples/111487-Universal%20Windows%20app%20samples/XAML%20scrolling%2C%20panning%2C%20and%20zooming%20sample)
- [入力: 簡略化されたインクのサンプル](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20Simplified%20ink%20sample)
* [入力: 操作とジェスチャのサンプル](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20Gestures%20and%20manipulations%20with%20GestureRecognizer)
- [DirectX タッチ入力のサンプル](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%2B%2B%5D-Windows%208%20app%20samples/C%2B%2B/Windows%208%20app%20samples/DirectX%20touch%20input%20sample%20(Windows%208))
