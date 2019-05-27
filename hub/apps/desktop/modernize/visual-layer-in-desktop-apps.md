---
title: ビジュアル層を使用して、デスクトップ アプリを最新化します。
description: ビジュアル層は、.NET や Win32 デスクトップ アプリの UI を強化するために使用します。
ms.date: 03/18/2019
ms.topic: article
keywords: windows 10, uwp
ms.author: jimwalk
author: jwmsft
ms.localizationpriority: medium
ms.openlocfilehash: 249291c59a31036fa967ac338209404557b57503
ms.sourcegitcommit: d1c3e13de3da3f7dce878b3735ee53765d0df240
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/24/2019
ms.locfileid: "66215170"
---
# <a name="using-the-visual-layer-in-desktop-apps"></a>デスクトップ アプリでのビジュアル層の使用

外観、外観と、Windows フォーム、WPF の機能を強化するために、デスクトップ アプリケーションを UWP 以外で UWP Api を使用することができますようになりましたしC++Win32 アプリケーションでのみ UWP を使用して利用できる最新の Windows 10 の UI 機能を活用してください。

使用することが多くのシナリオ[XAML 諸島](xaml-islands.md)アプリに最新の XAML コントロールを追加します。 ただし、組み込みのコントロールだけでなくカスタム エクスペリエンスを作成する必要がある場合は、ビジュアル層の Api にアクセスできます。

ビジュアル層では、高パフォーマンスのグラフィック、エフェクト、およびアニメーションの保持モード API を提供します。 Windows 10 デバイス間で UI の基盤になります。 UWP XAML コントロールは、ビジュアル層で構築され、これにより、さまざまな側面、 [Fluent Design System](/windows/uwp/design/fluent-design-system/index)光、深さ、モーション、マテリアル、およびスケールなど。

![ビジュアル層で作成されたユーザー インターフェイス](images/visual-layer-interop/pull-to-animate.gif)

> _ビジュアル層で作成されたユーザー インターフェイス_

## <a name="create-a-visually-engaging-user-interface-in-any-windows-app"></a>すべての Windows アプリでの視覚的に魅力的なユーザー インターフェイスを作成します。

ビジュアル層では、カスタムの描画コンテンツ (ビジュアル) の軽量の合成を使用して、アプリケーションでそれらのオブジェクトに対して強力なアニメーション、エフェクト、および操作を適用して魅力的なエクスペリエンスを作成できます。 ビジュアル層は、既存の UI フレームワークに代わるもの代わりに、これらのフレームワークを補足するため貴重になります。

ビジュアル層を使用して、アプリケーションの一意のルック アンド フィールを提供し、他のアプリケーションとは別に設定された id を確立できます。 ユーザーからに対するエンゲージメントの描画を使用するには、アプリケーションを簡単に設計された Fluent デザインの原則もできます。 たとえば、視覚的な手掛かりと画面上の項目間の関係を示すアニメーション遷移の作成に使用できます。

## <a name="visual-layer-features"></a>ビジュアル層の機能

### <a name="brushes"></a>ブラシ

[合成ブラシ](/windows/uwp/composition/composition-brushes)純色、グラデーション、イメージ、ビデオ、複雑な特殊効果、および詳細を UI オブジェクトを描画することができます。

![マテリアルの作成者で作成された、卵](images/visual-layer-interop/egg.gif)

> _作成された、egg、[マテリアル作成者デモ アプリ](https://github.com/Microsoft/WindowsCompositionSamples/tree/master/Demos/MaterialCreator)します。_

### <a name="effects"></a>効果

[合成効果](/windows/uwp/composition/composition-effects)ライト、シャドウ、およびフィルター効果の一覧が含まれます。 それらをアニメーション化、カスタマイズすると連結するなどしてビジュアルに直接適用。 大気、深さ、資料を作成する合成照明と組み合わせて、SceneLightingEffect 使用できます。

![ライトとマテリアル](images/visual-layer-interop/light-interop.gif)

> _ライトと素材の説明、[サンプル ギャラリーの Windows UI コンポジション](https://github.com/Microsoft/WindowsCompositionSamples/tree/master/SampleGallery)します。_

### <a name="animations"></a>アニメーション

[合成アニメーション](/windows/uwp/composition/composition-animation)コンポジター プロセスでは、UI スレッドから独立してで直接実行します。 これにより、滑らかさとスケール、多数の同時実行、明示的なアニメーションを実行できるようにします。 時間の経過と共にドライブ プロパティの変更に使い慣れたキーフレーム アニメーション、だけでなく、ユーザー入力を含む、各種プロパティ間の数学的な関係を設定する式を使用することができます。 入力の駆動型アニメーションでは、動的かつ流動的に応答する可能性が高いユーザー エンゲージメントをユーザー入力の UI を作成できます。

![ビジュアル層で作成されたユーザー インターフェイス](images/visual-layer-interop/swipe-scroller.gif)

> _示されているモーション、[サンプル ギャラリーの Windows UI コンポジション](https://github.com/Microsoft/WindowsCompositionSamples/tree/master/SampleGallery)します。_

## <a name="keep-your-existing-codebase-and-adopt-incrementally"></a>既存のコードベースを保持し、段階的に採用

既存のアプリケーションでコードが失われるしたくない多大な投資を表します。 移行することができます_諸島_ビジュアル層を使用し、その既存のフレームワークで、残りの UI へのコンテンツの。 これは、行うことができます重要な更新プログラムと機能強化、アプリケーションの UI に、既存のコードに広範な変更を加えることがなく基本ことを意味します。

## <a name="samples-and-tutorials"></a>サンプルおよびチュートリアル

サンプルを使って試してみる、アプリケーションで、ビジュアル レイヤーを使用する方法について説明します。 これらのサンプルとチュートリアル ヘルプ ビジュアル層の使用を開始し、機能のしくみを説明します。

### <a name="win32"></a>Win32

- [ビジュアル層を使用して、Win32 で](using-the-visual-layer-with-win32.md)チュートリアル
  - [こんにちはコンポジションのサンプル](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/HelloComposition)
- [こんにちはベクトルのサンプル](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/HelloVectors)
- [仮想サーフェスのサンプル](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/VirtualSurfaces)
- [画面キャプチャのサンプル](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/ScreenCaptureforHWND)

### <a name="windows-forms"></a>Windows フォーム

- [ビジュアル層を使用して Windows フォームで](using-the-visual-layer-with-windows-forms.md)チュートリアル
  - [こんにちはコンポジションのサンプル](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WinForms/HelloComposition)
- [ビジュアル層の統合サンプル](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WinForms/VisualLayerIntegration)

### <a name="wpf"></a>WPF

- [WPF のビジュアル層を使用して](using-the-visual-layer-with-wpf.md)チュートリアル
  - [こんにちはコンポジションのサンプル](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WPF/HelloComposition)
- [ビジュアル層の統合サンプル](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WPF/VisualLayerIntegration)
- [画面キャプチャのサンプル](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WPF/ScreenCapture)

## <a name="limitations"></a>制限事項

ビジュアル層の多くの機能は、UWP アプリでのように、デスクトップ アプリケーションでホストされているときでも同じ動作、一部の機能は制限があります。 ここで注意すべき制限事項の一部を示します。

- 効果のチェーンが依存[Win2D](http://microsoft.github.io/Win2D/html/Introduction.htm)効果の説明についてはします。 [Win2D NuGet パッケージ](https://www.nuget.org/packages/Win2D.uwp)から再コンパイルすることになるために、デスクトップ アプリケーションでサポートされていない、[ソース コード](https://github.com/Microsoft/Win2D)します。
- 実行のヒット テストを自分でビジュアル ツリーをウォークして境界の計算を行う必要があります。 これは、UWP でのビジュアル層と同じです。 ここでのヒット テストにバインドできます簡単に XAML 要素がない点が異なります。
- ビジュアル層では、テキストのレンダリング プリミティブはありません。
- 2 つの異なる UI テクノロジが一緒に使用される場合は各画面で、独自のピクセルの描画など、WPF とビジュアル層では、およびピクセルを共有することはできません。 その結果、ビジュアル層のコンテンツは常に、その他の UI コンテンツの上にレンダリングされます。 (これと呼ばれますが、_空域_問題です)。追加のコーディングを行う必要があり、テスト、ビジュアル層のコンテンツをホストの UI とサイズを変更して、その他のコンテンツがありません。
- デスクトップ アプリケーションでホストされているコンテンツは、自動的にサイズを変更または DPI のスケール。 追加の手順は、コンテンツは、DPI の変更を処理することを確認する必要があります。 (詳細についてはプラットフォーム固有のチュートリアルを参照してください)。

## <a name="additional-resources"></a>その他のリソース

- [ビジュアル レイヤー](/windows/uwp/composition/visual-layer)
- [合成ビジュアル](/windows/uwp/composition/composition-visual-tree)
- [合成ブラシ](/windows/uwp/composition/composition-brushes)
- [合成効果](/windows/uwp/composition/composition-effects)
- [合成アニメーション](/windows/uwp/composition/composition-animation)

API リファレンス

- [Windows.UI.Composition](/uwp/api/Windows.UI.Composition)