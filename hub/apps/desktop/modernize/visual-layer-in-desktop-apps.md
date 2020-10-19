---
title: ビジュアル レイヤーを使用してデスクトップ アプリを現代化する
description: ビジュアル レイヤーを使用して .NET または Win32 デスクトップ アプリの UI を強化します。
ms.date: 03/18/2019
ms.topic: article
keywords: windows 10, uwp
ms.author: jimwalk
author: jwmsft
ms.localizationpriority: medium
ms.openlocfilehash: 8fc9b3ea0f085a12be769e9733b3f92b2700dc16
ms.sourcegitcommit: 53c00939b20d4b0a294936df3d395adb0c13e231
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/09/2020
ms.locfileid: "91932943"
---
# <a name="using-the-visual-layer-in-desktop-apps"></a>デスクトップ アプリでのビジュアル レイヤーの使用

UWP 以外のデスクトップ アプリケーションで Windows ランタイム API を使用して WPF、Windows フォーム、C++ Win32 アプリケーションの外観や機能を高めたり、UWP でのみ利用可能な最新の Windows 10 UI 機能を活用したりできるようになりました。

多くのシナリオでは、[XAML Islands](xaml-islands.md) を使用して最新の XAML コントロールをアプリに追加できます。 ただし、組み込みのコントロールを超えるカスタム エクスペリエンスを作成する必要がある場合は、ビジュアル レイヤー API にアクセスできます。

ビジュアル レイヤーは、グラフィックス、エフェクト、アニメーションのための高パフォーマンスの保持モード API を提供します。 これは、Windows 10 デバイスにまたがる UI のための基礎です。 UWP XAML コントロールはビジュアル レイヤー上に構築されており、ライト、奥行き、モーション、素材、スケールなどの [Fluent Design System](/windows/uwp/design/fluent-design-system/index) の多くの側面を可能にします。

![ビジュアル レイヤーを使用して作成されたユーザー インターフェイスを示す短いビデオ。](images/visual-layer-interop/pull-to-animate.gif)

> _ビジュアル レイヤーで作成されたユーザー インターフェイス_

## <a name="create-a-visually-engaging-user-interface-in-any-windows-app"></a>任意の Windows アプリで視覚的に魅力のあるユーザー インターフェイスを作成する

ビジュアル レイヤーを使用すると、カスタム描画コンテンツの軽量合成 (ビジュアル) を使用し、アプリケーションでこれらのオブジェクトに強力なアニメーション、エフェクト、操作を適用することによって魅力的なエクスペリエンスを作成できます。 ビジュアル レイヤーは既存の UI フレームワークを置き換えるものではなく、これらのフレームワークの価値ある補完機能です。

ビジュアル レイヤーを使用すると、アプリケーションに固有の外観を与え、それを他のアプリケーションから区別するアイデンティティーを確立できます。 また、アプリケーションを使いやすくするように設計された Fluent Design の原則を有効にして、ユーザー エンゲージメントを向上させることもできます。 たとえば、これを使用して視覚的な合図や、画面上の項目間の関係を示すアニメーション化された画面の切り替えを作成できます。

## <a name="visual-layer-features"></a>ビジュアル レイヤーの機能

### <a name="brushes"></a>ブラシ

[コンポジションのブラシ](/windows/uwp/composition/composition-brushes)を使用すると、UI オブジェクトを単色、グラデーション、画像、ビデオ、複雑なエフェクトなどで塗りつぶすことができます。

![Material Creator で作成された卵](images/visual-layer-interop/egg.gif)

> _[Material Creator デモ アプリ](https://github.com/Microsoft/WindowsCompositionSamples/tree/master/Demos/MaterialCreator)で作成された卵。_

### <a name="effects"></a>効果

[コンポジション効果](/windows/uwp/composition/composition-effects)には、ライト、影、フィルター エフェクトの一覧が含まれます。 これらをアニメーション化、カスタマイズ、チェーンしてから、直接ビジュアルに適用できます。 SceneLightingEffect をコンポジション照明と組み合わせると、雰囲気、奥行き、素材を作成できます。

![ライトと素材](images/visual-layer-interop/light-interop.gif)

> _[Windows UI コンポジション サンプル ギャラリー](https://github.com/Microsoft/WindowsCompositionSamples/tree/master/SampleGallery)に示されているライトと素材。_

### <a name="animations"></a>アニメーション

[コンポジションのアニメーション](/windows/uwp/composition/composition-animation)は、UI スレッドには関係なく、コンポジター プロセスで直接実行されます。 これにより滑らかさとスケールが保証されるため、多数の同時実行の明示的なアニメーションを実行できます。 徐々にプロパティの変更を実行する使い慣れたキーフレーム アニメーションに加えて、式を使用して、ユーザー入力を含むさまざまなプロパティ間の数学的な関係を設定できます。 入力駆動型アニメーションを使用すると、ユーザー入力に動的かつ流動的に応答する UI を作成できます。これにより、ユーザー エンゲージメントが向上することがあります。

![ビジュアル レイヤーを使用して作成された別のユーザー インターフェイスを示す短いビデオ。](images/visual-layer-interop/swipe-scroller.gif)

> _[Windows UI コンポジション サンプル ギャラリー](https://github.com/Microsoft/WindowsCompositionSamples/tree/master/SampleGallery)に示されているモーション。_

## <a name="keep-your-existing-codebase-and-adopt-incrementally"></a>既存のコードベースを保持し、段階的に導入する

既存のアプリケーション内のコードは、失いたくない多大な投資を表しています。 ビジュアル レイヤーを使用するためにコンテンツの_島々_を移行し、残りの UI をその既存のフレームワーク内に保持することができます。 つまり、既存のコード ベースに広範囲の変更を加えることなく、アプリケーションの UI に対して大幅な更新や機能強化を行うことができます。

## <a name="samples-and-tutorials"></a>サンプルとチュートリアル

サンプルを試すことによりアプリケーションでビジュアル レイヤーを使用する方法について説明します。 これらのサンプルとチュートリアルは、ビジュアル レイヤーの使用開始に役立つと共に、機能がどのように動作するかを示します。

### <a name="win32"></a>Win32

- [Win32 でのビジュアル レイヤーの使用](using-the-visual-layer-with-win32.md)チュートリアル
  - [Hello コンポジションのサンプル](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/HelloComposition)
- [Hello ベクトルのサンプル](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/HelloVectors)
- [仮想サーフェスのサンプル](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/VirtualSurfaces)
- [画面キャプチャのサンプル](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/ScreenCaptureforHWND)

### <a name="windows-forms"></a>Windows フォーム

- [Windows フォームでのビジュアル レイヤーの使用](using-the-visual-layer-with-windows-forms.md)チュートリアル
  - [Hello コンポジションのサンプル](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WinForms/HelloComposition)
- [ビジュアル レイヤー統合のサンプル](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WinForms/VisualLayerIntegration)

### <a name="wpf"></a>WPF

- [WPF でのビジュアル レイヤーの使用](using-the-visual-layer-with-wpf.md)チュートリアル
  - [Hello コンポジションのサンプル](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WPF/HelloComposition)
- [ビジュアル レイヤー統合のサンプル](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WPF/VisualLayerIntegration)
- [画面キャプチャのサンプル](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WPF/ScreenCapture)

## <a name="limitations"></a>制限事項

デスクトップ アプリケーションでホストされているとき、ビジュアル レイヤーの機能の多くは UWP アプリでの場合と同様に動作しますが、一部の機能には制限があります。 注意する必要のあるいくつかの制限を次に示します。

- エフェクト チェーンは、エフェクトの説明に関して [Win2D](http://microsoft.github.io/Win2D/html/Introduction.htm) に依存します。 [Win2D NuGet パッケージ](https://www.nuget.org/packages/Win2D.uwp)は、デスクトップ アプリケーションではサポートされていないため、[ソース コード](https://github.com/Microsoft/Win2D)から再コンパイルする必要があります。
- ヒット テストを実行するには、ビジュアル ツリーを自分で調べて境界計算を実行する必要があります。 これは UWP のビジュアル レイヤーと同じですが、この場合は、ヒット テストのために容易にバインドできる XAML 要素が存在しない点が異なります。
- ビジュアル レイヤーには、テキストをレンダリングするためのプリミティブがありません。
- 2 つの異なる UI テクノロジ (WPF とビジュアル レイヤーなど) が一緒に使用されている場合、それらのテクノロジは画面へのそれぞれのピクセルの描画に責任を負い、ピクセルを共有することはできません。 その結果、ビジュアル レイヤー コンテンツは常に、他の UI コンテンツの上にレンダリングされます。 (これは_空域_の問題と呼ばれます。)ビジュアル レイヤー コンテンツがホスト UI と共にサイズを変更し、他のコンテンツを遮らないようにするために、追加のコーディングとテストが必要になることがあります。
- デスクトップ アプリケーションでホストされているコンテンツは、DPI に合わせて自動的にサイズを変更したり、スケーリングしたりしません。 コンテンツが DPI の変更に対処するようにするために、追加の手順が必要になることがあります。 (詳細については、プラットフォーム固有のチュートリアルを参照してください。)

## <a name="additional-resources"></a>その他のリソース

- [ビジュアル レイヤー](/windows/uwp/composition/visual-layer)
- [コンポジションのビジュアル](/windows/uwp/composition/composition-visual-tree)
- [コンポジションのブラシ](/windows/uwp/composition/composition-brushes)
- [コンポジション効果](/windows/uwp/composition/composition-effects)
- [コンポジションのアニメーション](/windows/uwp/composition/composition-animation)

API リファレンス

- [Windows.UI.Composition](/uwp/api/Windows.UI.Composition)