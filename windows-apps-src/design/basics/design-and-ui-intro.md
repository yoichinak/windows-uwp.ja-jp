---
description: すべての UWP アプリに含まれているユニバーサル デザイン機能は、さまざまなデバイス間で美しく拡大縮小されるアプリを構築するのに役立ちます。
title: Windows アプリ デザインの概要 (Windows アプリ)
ms.assetid: 50A5605E-3A91-41DB-800A-9180717C1E86
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 1f6474170967986bfee555eb07d7ea41601e9e48
ms.sourcegitcommit: aaa72ddeb01b074266f4cd51740eec8d1905d62d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/06/2020
ms.locfileid: "94339760"
---
# <a name="introduction-to-windows-app-design"></a>Windows アプリ デザインの概要

![サンプルの照明アプリ](images/introUWP-header.jpg)

Windows アプリのデザイン ガイダンスは、美しく洗練されたアプリをデザインおよび構築するのに役立つリソースです。

これは規範的な規則の一覧ではなく、進化する [Fluent Design System](/windows/apps/fluent-design-system)、およびアプリ構築コミュニティのニーズに適応するように設計された生きたドキュメントです。

この記事では、あらゆる UWP アプリに含まれているユニバーサル デザイン機能の概要を説明します。これらの機能により、さまざまなデバイス間で美しくスケーリングされるユーザー インターフェイス (UI) を構築できます。

## <a name="effective-pixels-and-scaling"></a>有効ピクセルとスケーリング

UWP アプリは、テレビからタブレットまたは PC まで、すべての [Windows 10 デバイス](../devices/index.md)で実行されます。 それでは、さまざまなデバイスや画面サイズで見栄えの良い UI をどのように設計しますか?

![さまざまなデバイス上にある同じアプリ](images/universal-image-1.jpg)

UWP は、すべてのデバイスと画面サイズで読みやすく、操作しやすいように、UI 要素のサイズを自動的に調整するのに役立ちます。

デバイスでアプリを実行するとき、システムでは、UI 要素を画面に表示する方法を正規化するアルゴリズムを使います。 このスケーリング アルゴリズムでは、視聴距離と画面の密度 (ピクセル/インチ) を考慮して、体感的なサイズを最適化します (物理的なサイズではありません)。 スケーリング アルゴリズムによって、10 フィート離れた Surface Hub における 24 ピクセルのフォントが、数インチ離れた 5 インチ サイズの電話における 24 ピクセルのフォントと同じようにユーザーに読みやすい状態で表示されます。

![さまざまなデバイスの視聴距離](images/scaling-chart.png)

スケーリング システムのしくみのため、UWP アプリをデザインするときには、実際の物理ピクセルではなく、有効ピクセルでデザインすることになります。 有効ピクセル (epx) は仮想的な測定単位で、画面の密度とは無関係にレイアウトのサイズと間隔を表すために使用されます。 (ガイドラインでは、epx、ep、および px を区別しないで使用しています。)

設計時には、ピクセル密度と実際の画面解像度を無視できます。 その代わり、サイズ クラスの有効な解像度 (有効ピクセル単位の解像度) が向上するように設計します (詳しくは、「[画面のサイズとブレークポイント](../layout/screen-sizes-and-breakpoints-for-responsive-design.md)」をご覧ください)。

> [!TIP]
> イメージ編集プログラムで画面のモックアップを作成するときは、DPI を 72 に設定し、画像サイズを、対象のサイズ クラスで有効な解像度に設定します サイズ クラスと有効な解像度の一覧については、「[画面のサイズとブレークポイント](../layout/screen-sizes-and-breakpoints-for-responsive-design.md)」をご覧ください。

### <a name="multiples-of-four"></a>4 の倍数

:::row:::
    :::column span:::
UWP アプリでは、UI 要素のサイズ、余白、および位置は、必ず **4 epx の倍数** にする必要があります。

UWP のスケールはデバイスによって異なり、100%、125%、150%、175%、200%、225%、250%、300%、350%、および 400% のスケール プラトーがあります。 基本単位は 4 になりますが、これは、整数以外の数値によってスケーリングできる唯一の整数であるためです (例: 4*1.5 = 6)。 4 の倍数の使用によってすべての UI 要素のピクセル全体が整列し、UI 要素のエッジがすっきりとシャープになります (この要件はテキストには適用されません。テキストのサイズと位置に制限はありません)。
    :::column-end:::
    :::column:::
![グリッド](images/4epx.svg)
    :::column-end:::
:::row-end:::

## <a name="layout"></a>レイアウト

UWP アプリは、すべてのデバイスに合わせて自動的に拡大縮小されるので、すべてのデバイスの UWP アプリの設計は同じ構造になっています。 UWP アプリの UI の最初から始めましょう。

### <a name="windows-frames-and-pages"></a>Windows、フレーム、ページ

:::row:::
    :::column:::
Windows 10 デバイスで UWP アプリが起動されると、[フレーム](/uwp/api/windows.ui.xaml.controls.frame)がある[ウィンドウ](/uwp/api/windows.ui.xaml.window)で起動し、[ページ](/uwp/api/windows.ui.xaml.controls.page) インスタンス間を移動できます。
    :::column-end:::
    :::column:::
![フレームがあるウィンドウのスクリーンショット。](images/frame.svg)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
アプリの UI は、ページのコレクションとして考えることができます。 各ページに配置する項目や、ページ間の関係は、開発者が自由に決めることができます。

ページを整理する方法については、[ナビゲーションの基本](navigation-basics.md)に関する記事を参照してください。
    :::column-end:::
    :::column:::
![コレクション ページのスクリーンショット。](images/collection-pages.svg)
    :::column-end:::
:::row-end:::

### <a name="page-layout"></a>ページのレイアウト

それらのページの外観はどのようになるでしょうか。 ほとんどのページでは一貫性を保つために一般的な構造に従うため、ユーザーはアプリのページ間およびページ内を簡単に移動できます。 通常、ページには次の 3 つの種類の UI 要素が含まれています。

- [ナビゲーション](navigation-basics.md)要素は、表示するコンテンツをユーザーが選択できるようにします。
- [コマンド](commanding-basics.md)要素は、操作、保存、コンテンツの共有などの操作を開始します。
- [コンテンツ](content-basics.md)要素は、アプリのコンテンツを表示します。

![一般的なレイアウト パターン](../layout/images/page-components.svg)

UWP アプリの一般的なパターンを実装する方法の詳細については、「[ページのレイアウト](../layout/page-layout.md)」を参照してください。

Visual Studio で [Windows Template Studio](https://github.com/Microsoft/WindowsTemplateStudio) を使用してアプリのレイアウトを使い始めることもできます。

## <a name="controls"></a>コントロール

UWP の設計プラットフォームには、すべての Windows デバイスで適切に動作することが保証されている一連のコモン コントロールが用意されており、[Fluent Design System](/windows/apps/fluent-design-system) の原則に従っています。 これらのコントロールには、ボタンやテキスト要素などの単純なコントロールから、一連のデータとテンプレートからリストを生成する高度なコントロールまで、すべてのものが含まれます。

![UWP コントロール](../style/images/color/windows-controls.svg)

UWP コントロールとコントロールに基づいて作成できるパターンの全一覧については、「[コントロールとパターン](../controls-and-patterns/index.md)」セクションをご覧ください。

## <a name="style"></a>スタイル

一般的なコントロールは、システム テーマとアクセント カラーを自動的に反映し、すべての入力の種類に対応し、すべてのデバイスに合わせて拡大縮小されます。 このように、Fluent Design System を反映しているため、順応性が高く、親近感があり、優れた美しさを持っています。 一般的なコントロールでは、既定のスタイルでライト、モーション、深度を使用しているため、コントロールを使用することで、アプリに Fluent Design System を組み込んでいることになります。

一般的なコントロールは高度にカスタマイズでき、コントロールの前景色を変更することも、外観を完全にカスタマイズすることもできます。 コントロールの既定のスタイルを変更するには、[軽量なスタイル設定](../controls-and-patterns/xaml-styles.md#lightweight-styling)を使用するか、XAML で[カスタム コントロール](../controls-and-patterns/control-templates.md)を作成します。

![アクセント カラーの gif](images/intro-style.gif)

## <a name="shell"></a>Shell

:::row:::
    :::column:::
UWP アプリでは、Windows [シェル](../shell/tiles-and-notifications/creating-tiles.md)のタイルと通知を使用して、幅広い Windows エクスペリエンスとの対話が実行されます。

タイルは、[スタート] メニューとアプリの起動時に表示され、アプリで何が行われるのかを簡単に示します。 タイルのパワーは、背後にあるコンテンツ、および提供するインテリジェンスと技術によるものです。

UWP アプリには 4 つのタイル サイズ (小、中、横長、大) があり、アプリのアイコンと ID でカスタマイズできます。 UWP アプリのタイルのデザインに関するガイダンスについては、「[タイルとアイコン アセットのガイドライン](../style/app-icons-and-logos.md)」をご覧ください。
    :::column-end:::
    :::column:::
![スタート メニューのタイル](images/shell.svg)
    :::column-end:::
:::row-end:::

## <a name="inputs"></a>入力

:::row:::
    :::column:::
UWP アプリではスマート操作が使用されます。 クリックの発生元がマウスか、スタイラスか、指によるタップかを認識または定義しなくても、クリック操作に対応したデザインを行うことができます。 ただし、[特定の入力モード](../input/input-primer.md)向けにアプリを設計することもできます。
    :::column-end:::
    :::column:::
![さまざまな入力モードを指定するアイコンのスクリーンショット。](images/inputs.svg)
    :::column-end:::
:::row-end:::

## <a name="devices"></a>デバイス

![デバイス](../layout/images/size-classes.svg)

同様に、UWP では、さまざまなデバイスに合わせてアプリを自動的にスケーリングしますが、[特定のデバイス向けに UWP アプリを最適化](../devices/index.md)することもできます。

## <a name="usability"></a>使いやすさ

:::image type="content" source="https://img-prod-cms-rt-microsoft-com.akamaized.net/cms/api/am/imageFileData/REYaAb" alt-text="さまざまな能力を持つ人々を描画する、棒人間のイラストの短いビデオ。":::

最後に重要な点として、ユーザビリティの目的は、アプリのエクスペリエンスをすべてのユーザーに開かれたものにすることです。 すべての人が、本当に包括的なユーザー エクスペリエンスの恩恵を受けます。すべてのユーザーに対してアプリを使いやすくする方法については、「[UWP アプリの操作性](../usability/index.md)」をご覧ください。

国際的なユーザーを対象に設計している場合は、[グローバリゼーションとローカライズ](../globalizing/globalizing-portal.md)を確認することをお勧めします。

視覚障碍、聴覚障碍、運動障碍を持つユーザーに関して、「[アクセシビリティ機能](../accessibility/accessibility-overview.md)」も検討してください。 アクセシビリティが最初から設計に組み込まれている場合は、[アプリをアクセシビリティ対応にする](../accessibility/accessibility-in-the-store.md)ことにほとんど時間と労力がかかりません。

## <a name="tools-and-design-toolkits"></a>ツールと設計ツールキット

基本的な設計機能がわかったので、UWP アプリの設計を開始しましょう。

設計プロセスで役立つさまざまなツールが用意されています。

- [設計ツールキットのページ](../downloads/index.md)をご覧ください。XD、Illustrator、Photoshop、Framer、Sketch の各ツールキット、および追加の設計ツールやフォントのダウンロードが提供されています。

- コンピューターを設定して UWP アプリのコードを記述できるようにするには、[「はじめに」 &gt; 「準備」](/windows/apps/get-started/get-set-up)の記事をご覧ください。

- UWP の UI を実装する方法については、エンド ツー エンドの「[サンプル UWP アプリ](https://developer.microsoft.com/windows/samples)」をご覧ください。

## <a name="video-summary"></a>ビデオの概要

> [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/Designing-Universal-Windows-Platform-apps/player]

## <a name="next-fluent-design-system"></a>次へ: Fluent Design System

Fluent Design (Microsoft のデザイン システム) の背後にある原則や、UWP アプリに組み込むことができる多くの機能について確認する場合は、引き続き「[Fluent Design System](/windows/apps/fluent-design-system)」をご覧ください。

## <a name="related-articles"></a>関連記事

- [UWP アプリとは](../../get-started/universal-application-platform-guide.md)
- [Fluent Design System](/windows/apps/fluent-design-system)
- [XAML プラットフォームの概要](../../xaml-platform/index.md)