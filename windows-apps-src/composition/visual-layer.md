---
ms.assetid: a2751e22-6842-073a-daec-425fb981bafe
title: ビジュアル レイヤー
description: Windows.UI.Composition API を使うと、フレーム ワーク層 (XAML) とグラフィック層 (DirectX) との間のコンポジション層にアクセスできます。
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: ac41d461982a39a939e460b7a81b144e5a08fdb3
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/20/2019
ms.locfileid: "74255527"
---
# <a name="visual-layer"></a>ビジュアル レイヤー

ビジュアル レイヤーは、グラフィックス、効果、アニメーション用の高パフォーマンスの保持モード API を提供し、Windows デバイス間ですべての UI の基盤となります。 You define your UI in a declarative manner and the Visual layer relies on graphics hardware acceleration to ensure your content, effects and animations are rendered in a smooth, glitch-free manner independent of the app's UI thread.

主な特長は次のとおりです。

* 使い慣れた WinRT API
* さらに動的な UI と対話式操作を実現する設計
* デザイン ツールとの一貫した概念
* 突然パフォーマンスが低下することがない直線的なスケーラビリティ

Windows UWP アプリは、いずれかの UI フレームワークを介してビジュアル レイヤーを既に使用しています。 非常にわずかな労力で、カスタム レンダリング、効果、アニメーションに直接ビジュアル レイヤーを利用することもできます。

![UI フレームワークの階層: フレームワーク レイヤー (Windows.UI.XAML) はビジュアル レイヤー (Windows.UI.Composition) を基盤とし、ビジュアル レイヤーはさらにグラフィック レイヤー (DirectX) を基盤としている](images/layers-win-ui-composition.png)

## <a name="whats-in-the-visual-layer"></a>ビジュアル レイヤーとは

ビジュアル レイヤーの主な機能は次のとおりです。

1. **コンテンツ**: カスタム描画コンテンツの軽量合成
1. **効果**: 効果をアニメーション化、チェーン化、カスタマイズできる、リアルタイム UI 効果システム
1. **アニメーション**: UI スレッドから独立して実行される、表現力豊かな、フレームワークに依存しないアニメーション

### <a name="content"></a>コンテンツ

コンテンツは、ビジュアルを使用するアニメーションおよび効果システムで使用できるように、ホスト、変換、提供されます。 クラス階層の基底クラスは [**Visual**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.Visual) クラスで、コンポジターでビジュアル状態を処理するアプリ プロセスにおける、軽量でスレッド アジャイルなプロキシです。 Sub-classes of Visual include  [**ContainerVisual**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.ContainerVisual) to allow for children to create trees of visuals and [**SpriteVisual**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.SpriteVisual) that contains content and can be painted with either solid colors, custom drawn content or visual effects. また、これらの種類のビジュアルは 2D UI 用のビジュアル ツリー構造を構成し、多くの表示される XAML FrameworkElements を強化します。

詳しくは、[コンポジションのビジュアル](composition-visual-tree.md)の概要をご覧ください。

### <a name="effects"></a>効果

ビジュアル レイヤーの効果システムでは、ビジュアルやビジュアル ツリーにフィルターや透明効果のチェーンを適用できます。 これは UI 効果システムであり、画像やメディアの効果を混同しないでください。 効果はアニメーション システムとの組み合わせで動作し、ユーザーは、UI スレッドから独立してレンダリングされる、スムーズで動的な効果プロパティのアニメーションを実現できます。 ビジュアル レイヤーの効果は、カスタマイズされたと対話型エクスペリエンスを構築するために組み合わせてアニメーション化できる、創造的な構成要素を提供します。

ビジュアル レイヤーは、アニメーション化可能な効果のチェーンに加えて、アニメーション化可能なライトに応答することにより、ビジュアルでマテリアル プロパティを模倣できるようにする照明モデルもサポートします。 ビジュアルによって、シャドウを生じさせることもできます。 照明とシャドウを組み合わせて使うことで、奥行きとリアルさを感じられるようにすることができます。

詳しくは、「[コンポジションの効果](composition-effects.md)」をご覧ください。

### <a name="animations"></a>[アニメーション]

ビジュアル レイヤーのアニメーション システムによって、移動の視覚効果、効果のアニメーション化、変換、クリップ、その他のプロパティの駆動を実現できます。  It is a framework agnostic system that has been designed from the ground up with performance in mind.  It runs independently from the UI thread to ensure smoothness and scalability.  While it lets you use familiar KeyFrame animations to drive property changes over time, it also lets you set up mathematical relationships between different properties, including user input, letting you directly craft seamless choreographed experiences.

詳しくは、「[コンポジションのアニメーション](composition-animation.md)」をご覧ください。

### <a name="working-with-your-xaml-uwp-app"></a>XAML UWP アプリの操作

[  **Windows.UI.Xaml.Hosting**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Hosting) の [**ElementCompositionPreview**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Hosting.ElementCompositionPreview) クラスを使用して、XAML フレームワークによってビジュアルを作成できるようになり、表示可能な FrameworkElement を強化することができます。 フレームワークによって作成されたビジュアルには、カスタマイズに関するいくつかの制限があることに注意してください。 これは、フレームワークがオフセット、変換、有効期間を管理するためです。 ただし、独自のビジュアルを作成し、ElementCompositionPreview によって、またはビジュアル ツリー構造内のどこかに既にある ContainerVisual に追加することにより、既存の XAML 要素にアタッチできます。

詳しくは、[XAML でのビジュアル レイヤーの使用](using-the-visual-layer-with-xaml.md)の概要をご覧ください。

### <a name="working-with-your-desktop-app"></a>Working with your desktop app

You can use the Visual layer to enhance the look, feel, and functionality of your WPF, Windows Forms, and C++ Win32 desktop apps. You can migrate islands of content to use the Visual layer and keep the rest of your UI in its existing framework. This means you can make significant updates and enhancements to your application UI without needing to make extensive changes to your existing code base.

詳しくは、「[Modernize your desktop app using the Visual layer (ビジュアル レイヤーを使用したデスクトップ アプリの現代化)](/windows/apps/desktop/modernize/visual-layer-in-desktop-apps)」をご覧ください。

## <a name="additional-resources"></a>その他の資料

* [**Full reference documentation for the API**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition)
* [WindowsUIDevLabs GitHub](https://github.com/microsoft/WindowsCompositionSamples) にある高度な UI とコンポジションのサンプル
* [Windows.UI.Composition Sample Gallery](https://www.microsoft.com/store/apps/9pp1sb5wgnww)
* [@windowsui Twitter feed ](https://twitter.com/windowsui)
* この API に関する Kenny Kerr の MSDN 記事:「[グラフィックスとアニメーション - Windows 合成が 10 歳になる](https://msdn.microsoft.com/magazine/mt590968)」
