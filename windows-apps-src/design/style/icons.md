---
Description: 優れたアイコンは、文字体裁やその他のデザイン言語と調和するものです。 これらは比喩と混用しないようにします。できるだけすばやくシンプルに、必要なことのみを伝えます。
title: アイコン
ms.assetid: b90ac02d-5467-4304-99bd-292d6272a014
label: Icons
template: detail.hbs
ms.date: 05/02/2018
ms.topic: article
keywords: windows 10, uwp
design-contact: Judysa
doc-status: Published
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: e35041cce7e43f6eebed06b39f3ae2dbda55a4ae
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156796"
---
# <a name="icons-for-windows-apps"></a>Windows アプリのアイコン

![ヘッダー画像のアイコン](images/icons/header-icons.png)

アイコンによって、アクション、概念、または製品を簡略化したビジュアルが提供されます。 シンボリック イメージに意味を凝縮することによって、アイコンは言語の壁を乗り越えることができ、非常に価値のあるリソースである画面領域を節約するのに役立ちます。 

アイコンはアプリ内とアプリ外部に表示されます。 

:::row:::
    :::column:::
        **アプリ内のアイコン**

        ![アプリ内のアイコン](images/icons/inside-icons.png) アプリ内では、テキストのコピー、設定ページへの移動などのアクションを表すためにアイコンを使用します。
    :::column-end:::
    :::column:::
**アプリ外部のアイコン**

        ![アプリ外部のアイコン](images/icons/outside-icons.jpg) アプリ外部では、Windows でスタート メニューとタスクバーにアプリを表すためにアイコンが使用されます。 ユーザーがアプリをスタート メニューにピン留めすることを選択すると、アプリのスタート タイルにアプリのアイコンが表示されることがあります。 アプリのアイコンはタイトル バーに表示されます。アプリのロゴ付きのスプラッシュ画面を作成することもできます。
    :::column-end:::
:::row-end:::

この記事では、アプリ内のアイコンについて説明します。 アプリ外部のアイコン (アプリ アイコン) の詳細については、[アプリおよびタイル アイコン](./app-icons-and-logos.md)に関する記事を参照してください。

## <a name="when-to-use-icons"></a>アイコンを使用するタイミング

アイコンを使用すると、領域を節約できますが、どのような場合に使用しますか? 

:::row:::
    :::column:::
        ![する](images/do.svg) ![標準イメージのアイコン](images/icons/icons-standard.svg)<br>

アイコンは、切り取り、コピー、貼り付け、保存などの操作、またはナビゲーション メニューのナビゲーション項目に使用します。
    :::column-end:::
    :::column:::
        ![しない](images/dont.svg) ![概念イメージのアイコン](images/icons/icons-concept.svg)<br>

表現する概念のアイコンが既に存在する場合は、それを使用します (アイコンが存在するかどうかを確認するには、Segoe アイコン一覧を確認します)。
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![する](images/do.svg) ![ショッピング カートのアイコン](images/icons/icon-shopping-cart.svg)<br>

アイコンが意味する内容をユーザーが理解しやすく、小さいサイズでもはっきりとわかるほどシンプルな場合は、アイコンを使用します。
    :::column-end:::
    :::column:::
        ![しない](images/dont.svg) ![概念イメージのアイコン](images/icons/icon-bad-example.png)<br>

アイコンの意味が明確でない場合、または明確にするには複雑な図形が必要な場合は、アイコンを使用しないでください。
    :::column-end:::
:::row-end:::



## <a name="using-the-right-type-of-icon"></a>適切な種類のアイコンの使用

アイコンを作成する方法は数多くあります。 Segoe MDL2 Assets などの記号フォントを使用できます。 独自のベクトルに基づく画像を作成できます。 ビットマップ画像も使用できますが、お勧めしません。 アプリにアイコンを追加するさまざまな方法の概要を次に示します。 

### <a name="use-a-predefined-icon"></a>定義済みのアイコンを使用します。
:::row:::
    :::column:::
Microsoft は、Segoe MDL2 アセット フォントの形式で 1,000 個を超えるアイコンを提供しています。 フォントからアイコンを取得するのは直感的ではない可能性がありますが、Microsoft のフォント表示テクノロジでは、これらのアイコンが任意のディスプレイ、解像度、サイズではっきりと鮮明に表示されます。 手順については、「[Segoe MDL2 アイコン](segoe-ui-symbol-font.md)」を参照してください。
    :::column-end:::
    :::column:::
        ![定義済みアイコン イメージ](images/icons/predefined-icon.png)
    :::column-end:::
:::row-end:::

### <a name="use-a-font"></a>フォントを使用します。
:::row:::
    :::column:::
Segoe MDL2 アセット フォントを使用する必要はありません。Wingdings や Webdings など、ユーザーがシステムにインストールしている任意のフォントを使用できます。
    :::column-end:::
    :::column:::
        ![Wingdings イメージ](images/icons/wingdings.png)
    :::column-end:::
:::row-end:::

### <a name="use-a-scalable-vector-graphics-svg-file"></a>スケーラブル ベクター グラフィックス (SVG) ファイルを使用します。
:::row:::
    :::column:::
SVG リソースは、任意のサイズや解像度で常に鮮明に表示されるため、アイコンに最適です。 ほとんどの描画アプリケーションは、SVG にエクスポートできます。 手順については、[SVGImageSource](/uwp/api/windows.ui.xaml.media.imaging.svgimagesource)に関するページを参照してください。
    :::column-end:::
    :::column:::
        ![SVG イメージ](images/icons/icon-scale.gif)
    :::column-end:::
:::row-end:::

### <a name="use-geometry-objects"></a>ジオメトリ オブジェクトを使用します。
:::row:::
    :::column:::
SVG ファイルのように、ジオメトリはベクトルに基づくリソースであるため、常に鮮明に表示されます。 しかし、それぞれの点と曲線を個々に指定する必要があるため、ジオメトリの作成は複雑です。 実際にはアプリの実行中にアイコンを変更する必要がある場合のみ適しています (アニメーション化する場合など)。 手順については、[ジオメトリのコマンドの移動と描画](../../xaml-platform/move-draw-commands-syntax.md)に関するページを参照してください。 
    :::column-end:::
    :::column:::
        ![ジオメトリ オブジェクトのイメージ](images/icons/geometry-objects.png)
    :::column-end:::
:::row-end:::

### <a name="you-can-also-use-a-bitmap-image-such-as-png-gif-or-jpeg-although-we-dont-recommend-it"></a>お勧めしませんが、PNG、GIF、JPEG などのビットマップ画像を使用することもできます。
:::row:::
    :::column:::
ビットマップ画像は特定のサイズで作成されるため、希望するアイコンの大きさと画面の解像度に応じて拡大縮小する必要があります。 画像を縮小すると、ぼやけて見えることがあります。拡大すると、ギザギザのピクセル化された外観になることがあります。 ビットマップ画像を使用する必要がある場合は、JPEG ではなく PNG または GIF を使用することをお勧めします。 
    :::column-end:::
    :::column:::
        ![しない](images/dont.svg) ![ビットマップ画像](images/icons/bitmap-image.png)
    :::column-end:::
:::row-end:::

## <a name="make-the-icon-do-something"></a>アイコンで何かを行う

アイコンができたら、次の手順として、それをコマンドやナビゲーション操作に関連付けて、何かを行うようにすることです。 これを行う最も良い方法は、アイコンをボタンやコマンド バーに追加することです。 

![コマンド バーのイメージ](images/icons/app-bar-desktop.svg)

## <a name="create-an-icon-button"></a>アイコン ボタンの作成

アイコンは標準ボタンに配置することができます。 ボタンはより広範な場所で使用できるため、アクション アイコンが表示される場所に関して、柔軟性がやや高くなります。 

ボタンにアイコンを追加する方法は、次のようにいくつかあります。

:::row:::
    :::column span="2":::
        <b>手順 1</b><br>
ボタンのフォント ファミリを `Segoe MDL2 Assets` に設定し、そのコンテンツ プロパティを使用するグリフの Unicode 値に設定します。
    :::column-end:::
    :::column:::
        ![アイコン ボタンの作成の手順 1](images/icons/create-icon-step-1.svg)
    :::column-end:::
:::row-end:::

```xaml 
<Button FontFamily="Segoe MDL2 Assets" Content="&#xE102;" />
```

:::row:::
    :::column span="2":::
        <b>手順 2</b><br>
次のアイコンの要素オブジェクトのいずれかを使用します。[BitmapIcon](/uwp/api/windows.ui.xaml.controls.bitmapicon)、[FontIcon](/uwp/api/windows.ui.xaml.controls.fonticon)、[PathIcon](/uwp/api/windows.ui.xaml.controls.pathicon)、または [SymbolIcon](/uwp/api/windows.ui.xaml.controls.symbolicon)。 これにより、選択するアイコンの種類が多くなり、必要に応じて、テキストなどのその他の種類のコンテンツとアイコンを組み合わせることができるようになります。
    :::column-end:::
    :::column:::
        ![アイコン ボタンの作成の手順 2](images/icons/icon-text-step-2.svg)
    :::column-end:::
:::row-end:::

```xaml 
<Button>
    <StackPanel>
        <SymbolIcon Symbol="Play" />
        <TextBlock>Play the movie</TextBlock>
    </StackPanel>
</Button>
```

## <a name="create-a-series-of-icons-in-a-command-bar"></a>コマンド バーでの一連のアイコンの作成

:::row:::
    :::column span:::
切り取り/コピー/貼り付けや、写真編集プログラムの一連の描画コマンドなど、組み合わされる一連のコマンドがある場合は、それらを[コマンド バー](../controls-and-patterns/app-bars.md)にまとめます。 コマンド バーでは、1 つまたは複数のアプリ バーのボタンまたはアプリ バーのトグル ボタンが取得されます。それぞれがアクションを表します。 それぞれのボタンには、表示されるアイコンを制御するために使用する [Icon](/uwp/api/windows.ui.xaml.controls.appbarbutton#Windows_UI_Xaml_Controls_AppBarButton_Icon) プロパティがあります。 アイコンを指定するには、さまざまな方法があります。 
    :::column-end:::
    :::column:::
        ![アイコンを含むコマンド バーの例](images/icons/create-icon-command-bar.svg)
    :::column-end:::
:::row-end:::

最も簡単な方法は、指定した定義済みのアイコンの一覧を使用することです。"戻る" または "停止" などのアイコン名を指定するだけで、システムによって描画されます。 

``` xaml
<CommandBar>
    <AppBarToggleButton Icon="Shuffle" Label="Shuffle" Click="AppBarButton_Click" />
    <AppBarToggleButton Icon="RepeatAll" Label="Repeat" Click="AppBarButton_Click"/>
    <AppBarSeparator/>
    <AppBarButton Icon="Back" Label="Back" Click="AppBarButton_Click"/>
    <AppBarButton Icon="Stop" Label="Stop" Click="AppBarButton_Click"/>
    <AppBarButton Icon="Play" Label="Play" Click="AppBarButton_Click"/>
    <AppBarButton Icon="Forward" Label="Forward" Click="AppBarButton_Click"/>
</CommandBar>

```
アイコン名の完全な一覧については、[Symbol 列挙型](/uwp/api/windows.ui.xaml.controls.symbol)に関するページを参照してください。 

コマンド バーにあるボタンにアイコンを指定する方法は他にもあります。

+ [FontIcon](/uwp/api/windows.ui.xaml.controls.fonticon) - アイコンは指定されたフォント ファミリのグリフに基づきます。
+ [BitmapIcon](/uwp/api/windows.ui.xaml.controls.bitmapicon) - アイコンは指定された **URI** を持つビットマップ画像ファイルに基づきます。
+ [PathIcon](/uwp/api/windows.ui.xaml.controls.pathicon) - アイコンは [Path](/uwp/api/windows.ui.xaml.shapes.path) データに基づきます。

コマンド バーの詳細については、「[コマンド バー](../controls-and-patterns/app-bars.md)」の記事を参照してください。 



## <a name="related-articles"></a>関連記事

* [アプリのアイコンとロゴ](app-icons-and-logos.md)