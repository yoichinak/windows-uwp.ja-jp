---
description: ユニバーサル Windows プラットフォーム (UWP) での Fluent Design System と、アプリにそれを取り込む方法について説明します。
title: Windows 用の Fluent Design System
keywords: UWP アプリのレイアウト, ユニバーサル Windows プラットフォーム, アプリの設計, インターフェイス, Fluent Design System
ms.date: 03/07/2018
ms.topic: article
ms.localizationpriority: medium
ms.custom: RS5
ms.author: mcleans
author: mcleanbyron
ms.openlocfilehash: a46aae591767ac6ded935d3b76d60bb8fbfa2746
ms.sourcegitcommit: 45dec3dc0f14934b8ecf1ee276070b553f48074d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/29/2020
ms.locfileid: "89094529"
---
# <a name="the-fluent-design-system-for-windows-app-creators"></a>Windows アプリ作成者用の Fluent Design System

![Fluent Design のヘッダー](images/fluent/fluentdesign-app-header.png)

## <a name="introduction"></a>はじめに

Fluent Design System は、順応性が高く、親近感があり、美しいユーザー インターフェイスを作成するためのシステムです。

## <a name="principles"></a>原則

**順応性:各デバイスで自然な Fluent エクスペリエンスを得られる**

Fluent エクスペリエンスは環境に適応します。 タブレット、デスクトップ PC、Xbox で軽快な Fluent エクスペリエンスを得られます。Mixed Reality ヘッドセットでの動作も優れています。 PC の追加モニターなど、多くのハードウェアを追加しても、Fluent エクスペリエンスでそれらを活用できます。

**親近感:Fluent エクスペリエンスは直感的で、強力である**

Fluent エクスペリエンスは動作と意図を読み取ります。すなわち、必要なものを把握および予想します。 ユーザーとアイデアが物理的に離れているかどうかに関係なく、それらを統合します。

**美しさ:Fluent エクスペリエンスは魅力的で、臨場感がある**

現実世界の要素を組み込むことで、Fluent エクスペリエンスでは基本的な機能を活用できます。 直感的かつ本能的に情報を整理できるように、ライト、影、モーション、深度、テクスチャを使用します。


## <a name="applying-fluent-design-to-your-app-with-uwp"></a>UWP によるアプリへの Fluent Design の適用

![Fluent Design のロゴ](images/fluent/fluentdesign_header.png)

設計ガイドラインでは、アプリに Fluent Design の原則を適用する方法について説明します。 どのような種類のアプリでしょうか。 ガイドラインの多くはあらゆるプラットフォームに適用できますが、Fluent Design をサポートするための UWP (ユニバーサル Windows プラットフォーム) を作成しました。

Fluent Design 機能は UWP に組み込まれています。 これらの機能のいくつか (有効ピクセルやユニバーサル入力システムなど) は、自動的に取り込まれます。 これらの機能を利用するために追加のコードを記述する必要はありません。 他の機能 (アクリル効果など) はオプションであり、それらの機能をアプリに取り込むには、機能を追加するためのコードを記述します。

> Fluent Design 機能を使用して既存の WPF または Windows アプリケーションの外観や機能を高めることができるように、UWP コントロールをデスクトップに追加しています。 詳細については、[WPF および Windows フォーム アプリケーションでの UWP コントロールのホスト](/windows/uwp/xaml-platform/xaml-host-controls)に関するページを参照してください。

<!-- To apply Fluent Design to your app, follow our guidelines and use UWP (Universal Windows Platform) you can use UWP UI features combined with best practices for creating apps that perform beautifully on all types of Windows-powered devices. -->

設計ガイダンスに加え、Fluent Design の記事では、設計を実現させるコードの記述方法も示されています。 UWP では、マークアップベースの言語である XAML が使用されます。これにより、ユーザー インターフェイスが作成しやすくなります。 次に例を示します。

```xaml
<Grid BorderBrush="Blue" BorderThickness="4">
    <TextBox Text="Design with XAML" Margin="20" Padding="16,24"/>
</Grid>
```

![XAML の例](images/fluent/xaml-example.png)


> UWP を初めて開発する場合は、[UWP の概要に関するページ](/windows/uwp/get-started)を参照してください。

## <a name="find-a-natural-fit"></a>最適なデザインを見つける

さまざまなデバイスでアプリを自然に操作してもらうにはどのようにすればいいですか? それぞれのデバイスに合わせて設計されたように感じさせることです。 無駄な領域がなく (密集しない)、異なる画面サイズに対応する UI レイアウトでは、使用するデバイス用に設計されたかのような自然な使用感を得られます。

:::row:::
    :::column:::
        ![fpo の画像](images/fluent/thumbnail-size-classes.jpg)
    :::column-end:::
    :::column span="2":::
**適切なブレークポイントの設計**

それぞれの画面サイズに合わせて設計する代わりに、いくつかの主要な幅 ("ブレークポイント" とも呼ばれる) に注目すると、設計とコードが非常に簡単になり、大小の画面に対応する魅力的なアプリを作成できます。

[画面のサイズとブレークポイントの詳細](/windows/uwp/design/layout/screen-sizes-and-breakpoints-for-responsive-design)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![fpo の画像](images/fluent/rspd-resize.gif)
    :::column-end:::
    :::column span="2":::
**レスポンシブ レイアウトの作成**

アプリが自然に感じられるようにするには、そのレイアウトをさまざまな画面サイズやデバイスに適合させる必要があります。 応答性の高い UI を作成するために、自動サイズ変更、レイアウト パネル、表示状態、さらには XAML での独立した UI 定義を使用することができます。

[応答性の高い設計の詳細](/windows/uwp/design/layout/responsive-design)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![fpo の画像](images/fluent/devices.jpg)
    :::column-end:::
    :::column span="2":::
**多様なデバイスの設計**

UWP アプリはさまざまな Windows ベースのデバイスで実行できます。 これは、利用可能なデバイス、デバイスの利用目的、デバイスの操作方法を理解するのに役立ちます。

[UWP デバイスの詳細](/windows/uwp/design/devices/)
:::row-end:::

:::row:::
    :::column:::
        ![fpo の画像](images/fluent/keyboard-shortcuts.jpg)
    :::column-end:::
    :::column span="2":::
**入力方法に合わせた最適化**

UWP アプリは、一般的なマウス、キーボード、ペンを自動的にサポートします。 余分な操作をする必要がありません。 ただし、必要に応じて、ペンや Surface Dial など、特定の入力方法に合わせてサポートを最適化することで、アプリの機能を強化することができます。

[入力方法と操作方法の詳細](/windows/uwp/design/input/input-primer)
:::row-end:::

## <a name="make-it-intuitive"></a>直感的なものにする

ユーザーの想像のとおりに動作するため、直観的に操作できます。 アクセシビリティとグローバリゼーションを実現するためにコントロールとパターンを確立し、プラットフォーム サポートを活用することで、操作の手間が省け、生産性が向上します。

共感を得るには、適切なタイミングで適切な処理を行います。

Fluent エクスペリエンスは一貫性のあるコントロールとパターンを使用するため、ユーザーが学習したとおりに動作します。 Fluent エクスペリエンスでは幅広い物理的な機能を使用してユーザーにアクセスします。グローバリゼーション機能が組み込まれているため、世界中のユーザーが使用することができます。

:::row:::
    :::column:::
        ![fpo の画像](images/fluent/thumbnail-navview.png)
    :::column-end:::
    :::column span="2":::
**適切なナビゲーションを提供する**

適切なアプリ構造とナビゲーション コンポーネントを使用して、使いやすさを向上させます。

[ナビゲーションの詳細](/windows/uwp/design/basics/navigation-basics/)
:::row-end:::

:::row:::
    :::column:::
        ![fpo の画像](images/fluent/thumbnail-commanding.png)
    :::column-end:::
    :::column span="2":::
**対話性を高める**

ユーザーは、ボタン、コマンド バー、キーボード ショートカット、コンテキスト メニューを使用してアプリと対話できます。これらは、静的なエクスペリエンスを動的なものに変えるツールです。

[コマンドの詳細](/windows/uwp/design/basics/commanding-basics/)
:::row-end:::

:::row:::
    :::column:::
        ![fpo の画像](images/fluent/thumbnail-controls-2.jpg)
    :::column-end:::
    :::column span="2":::
**ジョブに最適なコントロールの使用**

コントロールとはユーザー インターフェイスの構成要素です。最適なコントロールを使用すると、ユーザーの想像どおりに動作するユーザー インターフェイスを作成できます。 UWP には、単純なボタンから強力なデータ コントロールまで、45 を超えるコントロールがあります。

[UWP コントロールの詳細](/windows/uwp/design/controls-and-patterns/)
:::row-end:::

:::row:::
    :::column:::
        ![インクルーシブの画像](images/fluent/thumbnail-inclusive.png)
    :::column-end:::
    :::column span="2":::
**インクルーシブに** 適切に設計されたアプリは、障碍のある方も利用できます。 いくつかのコーディングを追加すると、世界中のユーザーとアプリを共有できます。

[ユーザビリティの詳細](/windows/uwp/design/usability/)
:::row-end:::

## <a name="be-engaging-and-immersive"></a>魅力と臨場感を実現する

Fluent Design に派手な効果はありません。 脳で効率的に処理するようにプログラムされたエクスペリエンスをエミュレートするため、ユーザー エクスペリエンスを拡張する物理的な効果が組み込まれています。

## <a name="use-light"></a>ライトの使用

ライトは、関心を集めるための方法です。 ライトによって、使用する場所の雰囲気や特徴が作り出されます。ライトは、情報に焦点を当てるための実用的なツールです。

UWP アプリにライトを追加する

:::row:::
    :::column:::
        ![fpo の画像](images/fluent/Nav_Reveal_Animation.gif)
    :::column-end:::
    :::column span="2":::
**表示ハイライト**

[表示ハイライト](/windows/uwp/design/style/reveal)は、ライトを使用して対話型の要素を目立たせます。ライトはユーザーが対話できる要素を照らし、隠れた境界線を表示します。 表示は、リスト ビューやグリッド ビューなど、いくつかのコントロールで自動的に有効になります。 定義済みの表示ハイライト スタイルを適用すると、他のコントロールでも表示を有効にできます。
:::row-end:::

:::row:::
    :::column:::
        ![fpo の画像](images/fluent/traveling-focus-fullscreen-light-rf.gif)
    :::column-end:::
    :::column span="2":::
**表示フォーカス**

[表示フォーカス](/windows/uwp/design/style/reveal-focus) はライトを使用して、入力フォーカスのある要素に注意を向けます。
:::row-end:::

## <a name="create-a-sense-of-depth"></a>奥行きの作成

私たちは、3 次元の世界で暮らしています。 奥行きを UI に意図的に組み込むと、視覚的な階層が作成され、フラットな 2-D インターフェイスを、より効果的に情報と概念を提示するインターフェイスに変換することができます。 奥行きは、階層化された物理環境では、物事が相互にどのように関連するかを再構成します。

UWP アプリに奥行きを追加する

:::row:::
    :::column:::
        ![fpo の画像](images/fluent/_parallax_v2.gif)
    :::column-end:::
    :::column span="2":::
**視差**

"[視差](/windows/uwp/design/motion/parallax)" は、前景にあるアイテムを背景にあるアイテムよりも速く動くように見せて、奥行き感を作り出します。
:::row-end:::

## <a name="incorporate-motion"></a>モーションの組み込み

モーションのデザインは映画のようなものと考えてください。 シームレスな切り替えによって、ユーザーをストーリーに注目させておき、優れたエクスペリエンスを実現することができます。 モーションのデザインにそのような感覚を取り込むことで、あるタスクから別のタスクへ映画のようにスムーズにユーザーを移行させることができます。

UWP アプリにモーションを追加する

:::row:::
    :::column:::
        ![継続性 gif](images/fluent/continuityXbox.gif)
    :::column-end:::
    :::column span="2":::
**接続型アニメーション**

"[接続型アニメーション](/windows/uwp/design/motion/connected-animation)" を使用すると、ユーザーはシームレスなシーンの切り替えを作成することでコンテキストを維持することができます。
:::row-end:::

## <a name="build-it-with-the-right-material"></a>適切な素材を使用して作成する

現実の世界で私たちの周囲にあるものは、ある種の感覚と刺激を与えます。 そういったものは、折れ曲がったり、伸びたり、弾んだり、砕かれたり、滑らかに動いたりします。 こうした素材の質感をデジタル環境に取り込むことで、ユーザーが利用したいと思うようなデザインを実現できます。

UWP アプリに素材を追加する

:::row:::
    :::column:::
        ![fpo の画像](images/fluent/acrylic_lighttheme_base.png)
    :::column-end:::
    :::column span="2":::
**Acrylic**

"[アクリル](/windows/uwp/design/style/acrylic)" は、ユーザーに対してコンテンツのレイヤーを表示する半透明な素材です。UI 要素の階層を確立します。
:::row-end:::

## <a name="design-toolkits-and-code-samples"></a>設計ツールキットとコード サンプル

Fluent Design で独自アプリの作成を始めてみませんか。 Adobe XD、Adobe Illustrator、Adobe Photoshop、Framer、Sketch 用のツールキットを使用すると、設計をすぐに始められます。また、サンプルを使用すると、コーディングの時間が短縮されます。

:::row:::
    :::column:::
        ![fpo の画像](images/fluent/thumbnail-toolkits.jpg)
    :::column-end:::
    :::column span="2":::
**設計ツールキットとサンプルのページ**

[設計ツールキットとサンプルのページ](/windows/uwp/design/downloads/)を確認してください。
:::row-end:::









