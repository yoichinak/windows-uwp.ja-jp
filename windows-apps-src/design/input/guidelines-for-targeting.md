---
Description: このトピックでは、タッチ補正のための接触形状の使用について説明し、Windows ランタイム アプリでのターゲット設定のベスト プラクティスを紹介します。
title: ターゲット設定
ms.assetid: 93ad2232-97f3-42f5-9e45-3fc2143ac4d2
label: Targeting
template: detail.hbs
ms.date: 03/18/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9b1cac04405f18aaf3c8f39f9bfce2b965577807
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/20/2019
ms.locfileid: "74257940"
---
# <a name="guidelines-for-touch-targets"></a>タッチターゲットのガイドライン

ユニバーサル Windows プラットフォーム (UWP) アプリケーションのすべての対話型 UI 要素は、デバイスの種類や入力方法に関係なく、ユーザーが正確にアクセスして使用するのに十分な大きさである必要があります。

タッチ入力をサポートする (また、タッチの連絡先領域の比較的不正確な性質) には、対象のサイズとコントロールのレイアウトに関してさらに最適化が必要です。これは、タッチデジタイザーによって報告されたより複雑な入力データのセットを使用して、ユーザーの意図された (または最も可能性の高い) ターゲット。

すべての UWP コントロールは、既定のタッチターゲットのサイズとレイアウトで設計されています。これにより、快適で使いやすく、自信を持って、視覚的にバランスの取れた魅力的なアプリを作成できます。

このトピックでは、これらの既定の動作について説明します。これにより、プラットフォームコントロールとカスタムコントロール (アプリで必要になるもの) を使用して、使いやすさを最大限にするアプリを設計できます。

> **重要な API**: [**Windows.UI.Core**](https://docs.microsoft.com/uwp/api/Windows.UI.Core)、[**Windows.UI.Input**](https://docs.microsoft.com/uwp/api/Windows.UI.Input)、[**Windows.UI.Xaml.Input**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input)

## <a name="fluent-standard-sizing"></a>Fluent Standard サイズ

"*Fluent Standard サイズ*" は、情報の密度とユーザーの快適さのバランスを取るために作成されました。 実質的に、画面上のすべての項目が 40 x 40 の有効ピクセル (epx) ターゲットに揃えられ、UI 要素をグリッドに位置合わせし、システム レベルのスケーリングに基づいて適切にスケーリングできます

> [!NOTE]
>有効ピクセルとスケーリングについて詳しくは、「[UWP アプリ設計の概要](../basics/design-and-ui-intro.md#effective-pixels-and-scaling)」をご覧ください
>
> システム レベルのスケーリングについて詳しくは、「[配置、余白、パディング](../layout/alignment-margin-padding.md)」をご覧ください。

## <a name="fluent-compact-sizing"></a>Fluent Compact サイズ

アプリケーションでは、 *Fluent Compact のサイズ変更*により、より高いレベルの情報密度を表示できます。 コンパクトなサイズ変更では、UI 要素を 32x32 window.epx.codesnippet ターゲットに揃えます。これにより、UI 要素をより厳密なグリッドに揃え、システムレベルのスケーリングに基づいて適切に拡大縮小できます。

### <a name="examples"></a>例

コンパクトなサイズ変更は、ページレベルまたはグリッドレベルで適用できます。

### <a name="page-level"></a>ページレベルのロック

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

## <a name="target-size"></a>ターゲットサイズ

一般に、タッチターゲットサイズを 7.5 mm 二乗範囲に設定します (1.0 x スケーリング安定で 135 PPI ディスプレイの40x40 ピクセル)。 通常、UWP コントロールは 7.5 mm タッチターゲットに合わせて配置されます (これは、特定のコントロールと、一般的な使用パターンによって異なる場合があります)。 詳細については[、「コントロールのサイズと密度](../style/spacing.md)」を参照してください。

表に示したターゲット サイズの推奨サイズは、個々のシナリオの必要に応じて調整できます。 次に、考慮すべき点をいくつか示します。

- タッチの頻度-最小サイズよりも、繰り返しまたは頻繁に押されるターゲットを作成することを検討します。
- エラーの結果-エラーが発生した場合に重大な結果が得られるターゲットは、余白が大きくなり、コンテンツ領域の端からさらに配置される必要があります。 特に当てはまるのは頻繁にタッチされるターゲットです。
- コンテンツ領域内の位置。
- フォームファクターと画面サイズ。
- 指の体制。
- タッチの視覚エフェクト。

## <a name="related-articles"></a>関連記事

- [UWP アプリ設計の概要](../basics/design-and-ui-intro.md)
- [サイズと密度の制御](../style/spacing.md)
- [配置、余白、パディング](../layout/alignment-margin-padding.md)

### <a name="samples"></a>サンプル

- [基本的な入力サンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
- [低待機時間入力サンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
- [ユーザー操作モードのサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode)
- [フォーカスの視覚効果のサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals)

### <a name="archive-samples"></a>サンプルのアーカイブ

- [入力: XAML ユーザー入力イベントのサンプル](https://code.msdn.microsoft.com/windowsapps/Input-3dff271b)
- [入力: デバイス機能のサンプル](https://code.msdn.microsoft.com/windowsapps/Input-device-capabilities-31b67745)
- [入力: タッチヒットテストのサンプル](https://code.msdn.microsoft.com/windowsapps/Touch-Hit-Testing-sample-5e35c690)
- [XAML のスクロール、パン、ズームのサンプル](https://code.msdn.microsoft.com/windowsapps/xaml-scrollviewer-pan-and-949d29e9)
- [入力: 簡略化されたインクのサンプル](https://code.msdn.microsoft.com/windowsapps/Input-simplified-ink-sample-11614bbf)
- [入力: Windows 8 のジェスチャのサンプル](https://docs.microsoft.com/samples/browse/?redirectedfrom=MSDN-samples)
- [入力: 操作とジェスチャ (C++) のサンプル](https://code.msdn.microsoft.com/windowsapps/Manipulations-and-gestures-362b6b59)
- [DirectX タッチ入力のサンプル](https://code.msdn.microsoft.com/windowsapps/Simple-Direct3D-Touch-f98db97e)
