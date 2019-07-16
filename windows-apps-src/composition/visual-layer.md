---
ms.assetid: a2751e22-6842-073a-daec-425fb981bafe
title: ビジュアル レイヤー
description: Windows.UI.Composition API を使うと、フレーム ワーク層 (XAML) とグラフィック層 (DirectX) との間のコンポジション層にアクセスできます。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: bf9cc4f97cdfcb02eb725b81163f215b22b259e4
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/21/2019
ms.locfileid: "67318055"
---
# <a name="visual-layer"></a>ビジュアル レイヤー

ビジュアル レイヤーは、グラフィックス、効果、アニメーション用の高パフォーマンスの保持モード API を提供し、Windows デバイス間ですべての UI の基盤となります。 宣言的な方法で UI を定義すると、ビジュアル レイヤーは、グラフィックス ハードウェア アクセラレータを利用し、アプリの UI スレッドから独立して、エラーのない、なめらかな方法でコンテンツ、効果、アニメーションをレンダリングします。

主な特長は次のとおりです。

* 使い慣れた WinRT API
* さらに動的な UI と対話式操作を実現する設計
* デザイン ツールとの一貫した概念
* 突然パフォーマンスが低下することがない直線的なスケーラビリティ

Windows UWP アプリは、いずれかの UI フレームワークを介してビジュアル レイヤーを既に使用しています。 非常にわずかな労力で、カスタム レンダリング、効果、アニメーションに直接ビジュアル レイヤーを利用することもできます。

![UI フレームワークの階層: フレームワーク レイヤー (Windows.UI.XAML) はビジュアル レイヤー (Windows.UI.Composition) を基盤とし、ビジュアル レイヤーはさらにグラフィック レイヤー (DirectX) を基盤としている](images/layers-win-ui-composition.png)

## <a name="whats-in-the-visual-layer"></a>ビジュアル レイヤーとは

ビジュアル レイヤーの主な機能は次のとおりです。

1. **[コンテンツ]** :カスタムの描画コンテンツの軽量の複合
1. **効果**:リアルタイム UI 効果システムが効果をアニメーション化、連鎖してカスタマイズできます。
1. **アニメーション**:UI スレッドから独立して実行されているアニメーションの表現力豊かな、フレームワークに依存しません。

### <a name="content"></a>Content

コンテンツは、ビジュアルを使用するアニメーションおよび効果システムで使用できるように、ホスト、変換、提供されます。 クラス階層の基底クラスは [**Visual**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.Visual) クラスで、コンポジターでビジュアル状態を処理するアプリ プロセスにおける、軽量でスレッド アジャイルなプロキシです。 ビジュアルのサブ クラスが含まれます  [**ContainerVisual** ](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.ContainerVisual)子のビジュアル ツリーを作成するために、 [ **SpriteVisual** ](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.SpriteVisual)ですコンテンツを含み、いずれかの純色、カスタムの描画コンテンツまたは視覚的効果を描画することができます。 また、これらの種類のビジュアルは 2D UI 用のビジュアル ツリー構造を構成し、多くの表示される XAML FrameworkElements を強化します。

詳しくは、[コンポジションのビジュアル](composition-visual-tree.md)の概要をご覧ください。

### <a name="effects"></a>効果

ビジュアル レイヤーの効果システムでは、ビジュアルやビジュアル ツリーにフィルターや透明効果のチェーンを適用できます。 これは UI 効果システムであり、画像やメディアの効果を混同しないでください。 効果はアニメーション システムとの組み合わせで動作し、ユーザーは、UI スレッドから独立してレンダリングされる、スムーズで動的な効果プロパティのアニメーションを実現できます。 ビジュアル レイヤーの効果は、カスタマイズされたと対話型エクスペリエンスを構築するために組み合わせてアニメーション化できる、創造的な構成要素を提供します。

ビジュアル レイヤーは、アニメーション化可能な効果のチェーンに加えて、アニメーション化可能なライトに応答することにより、ビジュアルでマテリアル プロパティを模倣できるようにする照明モデルもサポートします。 ビジュアルによって、シャドウを生じさせることもできます。 照明とシャドウを組み合わせて使うことで、奥行きとリアルさを感じられるようにすることができます。

詳しくは、「[コンポジションの効果](composition-effects.md)」をご覧ください。

### <a name="animations"></a>アニメーション

ビジュアル レイヤーのアニメーション システムによって、移動の視覚効果、効果のアニメーション化、変換、クリップ、その他のプロパティの駆動を実現できます。  これは、パフォーマンスを考慮して一から設計された、フレームワークに依存しないシステムです。  UI スレッドから独立して実行されるため、滑らかさとスケーラビリティが実現されます。  使い慣れたキー フレーム アニメーションを使って時間の経過に伴うプロパティの変化を駆動できる一方で、ユーザー入力を含む、さまざまなプロパティ間の数学的な関係を設定して、シームレスな演出エクスペリエンスを直接作成することもできます。

詳しくは、[コンポジションのアニメーション](composition-animation.md)の概要をご覧ください。

### <a name="working-with-your-xaml-uwp-app"></a>XAML UWP アプリの操作

[  **Windows.UI.Xaml.Hosting**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Hosting) の [**ElementCompositionPreview**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Hosting.ElementCompositionPreview) クラスを使用して、XAML フレームワークによってビジュアルを作成できるようになり、表示可能な FrameworkElement を強化することができます。 フレームワークによって作成されたビジュアルには、カスタマイズに関するいくつかの制限があることに注意してください。 これは、フレームワークがオフセット、変換、有効期間を管理するためです。 ただし、独自のビジュアルを作成し、ElementCompositionPreview によって、またはビジュアル ツリー構造内のどこかに既にある ContainerVisual に追加することにより、既存の XAML 要素にアタッチできます。

詳しくは、[XAML でのビジュアル レイヤーの使用](using-the-visual-layer-with-xaml.md)の概要をご覧ください。

### <a name="working-with-your-desktop-app"></a>デスクトップ アプリの使用

ビジュアル レイヤーを使用することで、Windows フォーム、WPF、C++ Win32 デスクトップ アプリの外観、操作性、機能を向上させることができます 。 Islands のコンテンツを移行してビジュアルレイヤーを使用し、残りのUIを既存のフレームワークに維持することができます。 これは、既存のコードに広範な変更を加えることなく、アプリケーションの UI の重要なプログラム更新や機能強化ができます。

詳しくは、「[Modernize your desktop app using the Visual layer (ビジュアル レイヤーを使用したデスクトップ アプリの現代化)](/windows/apps/desktop/modernize/visual-layer-in-desktop-apps)」をご覧ください。

## <a name="additional-resources"></a>その他の資料

* [**API の完全なリファレンス ドキュメント**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition)
* [WindowsUIDevLabs GitHub](https://github.com/microsoft/WindowsCompositionSamples) にある高度な UI とコンポジションのサンプル
* [Windows.UI.Composition サンプル ギャラリー](https://aka.ms/winuiapp)
* [@windowsui Twitter フィード ](https://twitter.com/windowsui)
* この API の Kenny Kerr の MSDN 記事をご覧ください。[グラフィックスとアニメーション - Windows 合成に 10](https://msdn.microsoft.com/magazine/mt590968)
