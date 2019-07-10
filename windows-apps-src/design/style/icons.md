---
Description: 優れたアイコンは、文字の体裁やその他のデザイン言語と調和するものです。 アイコンは比喩と混用しないようにします。優れたアイコンは、できるだけすばやくシンプルに、必要なことのみを伝えます。
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
ms.openlocfilehash: 5e464251200812e79474d05d9d0a680b49167871
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/13/2019
ms.locfileid: "64564536"
---
# <a name="icons-for-uwp-apps"></a>UWP アプリのアイコン

![アイコンのヘッダー画像](images/icons/header-icons.png)

アイコンは、アクション、概念、または製品の簡潔にした視覚表現を提供します。 シンボリック イメージに意味を凝縮することによって、アイコンは言語の壁を乗り越えることができ、非常に価値のあるリソースである画面領域を節約するのに役立ちます。 

アイコンはアプリ内、およびアプリの外部に表示されます。 

:::row:::
    :::column:::
        **Icons inside the app**

        ![icons inside the app](images/icons/inside-icons.png)
アプリ内では、テキストのコピー、設定ページへの移動などのアクションを表すためにアイコンを使用します。
    :::column-end:::
    :::column:::
**アプリ外部のアイコン**

        ![icons outside the app](images/icons/outside-icons.jpg)
アプリ外部では、Windows のスタート メニューとタスクバーにアプリを表すアイコンが使用されます。 ユーザーがアプリをスタート メニューにピン留めすることを選択すると、アプリのスタート タイルにアプリのアイコンが表示されることがあります。 アプリのアイコンはタイトル バーに表示されます。アプリのロゴ付きのスプラッシュ画面を作成することもできます。
    :::column-end:::
:::row-end:::

この記事では、アプリ内のアイコンについて説明します。 アプリの外部のアイコン (アプリ アイコン) の詳細については、「[アプリおよびタイル アイコンに関する記事](/windows/uwp/design/shell/tiles-and-notifications/app-assets)」を参照してください。

## <a name="when-to-use-icons"></a>アイコンを使う状況

アイコンは領域を節約できますが、アイコンをいつ使用する必要があるか説明します。 

:::row:::
    :::column:::
        ![do](images/do.svg)
        ![icons standard image](images/icons/icons-standard.svg)<br>

アイコンは、切り取り、コピー、貼り付け、保存などの操作、またはナビゲーション メニューのナビゲーション項目に使用します。
    :::column-end:::
    :::column:::
        ![don't](images/dont.svg)
        ![icons concept image](images/icons/icons-concept.svg)<br>

表現する概念のアイコンが既に存在する場合は、それを使用します (アイコンが存在するかどうかを確認するには、Segoe アイコン一覧を確認します)。
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![do](images/do.svg)
        ![icon shopping cart](images/icons/icon-shopping-cart.svg)<br>

アイコンが意味する内容をユーザーが理解しやすく、小さいサイズでもはっきりとわかるほどシンプルな場合は、アイコンを使用します。
    :::column-end:::
    :::column:::
        ![dont](images/dont.svg)
        ![icons concept image](images/icons/icon-bad-example.png)<br>

アイコンの意味が明確でない場合、または明確にするには複雑な図形が必要な場合は、アイコンを使用しないでください。
    :::column-end:::
:::row-end:::



## <a name="using-the-right-type-of-icon"></a>適切な種類のアイコンの使用

アイコンを作成する方法は数多くあります。 Segoe MDL2 アセットなどの記号フォントを使用できます。 独自のベクトルに基づく画像を作成できます。 ビットマップ画像も使用できますが、お勧めしません。 アプリにアイコンを追加するさまざまな方法の概要を次に示します。 

### <a name="use-a-predefined-icon"></a>定義済みのアイコンを使用します。
:::row:::
    :::column:::
Microsoft は、Segoe MDL2 アセット フォントの形式で 1,000 個を超えるアイコンを提供しています。 フォントからアイコンを取得するのは直感的ではない可能性がありますが、マイクロソフトのフォントの表示テクノロジでは、これらのアイコンが任意のディスプレイ、解像度、サイズではっきりと鮮明に表示されます。 手順については、「[Segoe MDL2 アイコン](segoe-ui-symbol-font.md)」を参照してください。
    :::column-end:::
    :::column:::
        ![pre-defined icon image](images/icons/predefined-icon.png)
    :::column-end:::
:::row-end:::

### <a name="use-a-font"></a>フォントを使用します。
:::row:::
    :::column:::
Segoe MDL2 アセット フォントを使用する必要はありません。Wingdings や Webdings など、ユーザーがシステムにインストールしている任意のフォントを使用できます。
    :::column-end:::
    :::column:::
        ![wingdings image](images/icons/wingdings.png)
    :::column-end:::
:::row-end:::

### <a name="use-a-scalable-vector-graphics-svg-file"></a>スケーラブル ベクター グラフィックス (SVG) ファイルを使用します。
:::row:::
    :::column:::
SVG リソースは、任意のサイズや解像度で常に鮮明に表示されるため、アイコンに最適です。 ほとんどの描画アプリケーションは、SVG にエクスポートできます。 手順については、[SVGImageSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.svgimagesource)に関するページを参照してください。
    :::column-end:::
    :::column:::
        ![SVG image](images/icons/icon-scale.gif)
    :::column-end:::
:::row-end:::

### <a name="use-geometry-objects"></a>ジオメトリ オブジェクトを使用します。
:::row:::
    :::column:::
SVG ファイルのように、ジオメトリはベクトルに基づくリソースであるため、常に鮮明に表示されます。 ただし、それぞれの点と曲線を個々に指定する必要があるため、ジオメトリの作成は複雑です。 実際にはアプリの実行中にアイコンを変更する必要がある場合のみ最適です (アプリをアニメーション化する場合など)。 手順については、「[ジオメトリのコマンドの移動と描画](../../xaml-platform/move-draw-commands-syntax.md)」を参照してください。 
    :::column-end:::
    :::column:::
        ![Geometry objects image](images/icons/geometry-objects.png)
    :::column-end:::
:::row-end:::

### <a name="you-can-also-use-a-bitmap-image-such-as-png-gif-or-jpeg-although-we-dont-recommend-it"></a>お勧めしませんが、PNG、GIF、JPEG などのビットマップ画像を使用することもできます。
:::row:::
    :::column:::
ビットマップ画像は特定のサイズで作成されるため、希望するアイコンの大きさと画面の解像度に応じて拡大縮小する必要があります。 画像を縮小すると、ぼやけて見えることがあります。画像を拡大すると、むらのあるピクセル化された外観になることがあります。 ビットマップ画像を使用する必要がある場合は、JPEG ではなく PNG または GIF を使用することをお勧めします。 
    :::column-end:::
    :::column:::
        ![don't](images/dont.svg)
        ![Bitmap image](images/icons/bitmap-image.png)
    :::column-end:::
:::row-end:::

## <a name="make-the-icon-do-something"></a>アイコンで何かを行う

アイコンができたら、次の手順として、それをコマンドやナビゲーション操作に関連付けて、何かを行うようにすることです。 これを行う最も良い方法は、アイコンをボタンやコマンド バーに追加することです。 

![コマンド バーのイメージ](images/icons/app-bar-desktop.svg)

## <a name="create-an-icon-button"></a>アイコン ボタンの作成

アイコンは標準的なボタンに配置することができます。 ボタンは幅広い場所で使用できるため、アクション アイコンが表示される場所に関して、柔軟性がやや高くなります。 

ボタンにアイコンを追加する方法は次のようにいくつかあります。

:::row:::
    :::column span="2":::
        <b>Step 1</b><br>
ボタンのフォント ファミリを `Segoe MDL2 Assets` に設定し、そのコンテンツ プロパティを使用するグリフの Unicode 値に設定します。
    :::column-end:::
    :::column:::
        ![Create an icon button step 1](images/icons/create-icon-step-1.svg)
    :::column-end:::
:::row-end:::

```xaml 
<Button FontFamily="Segoe MDL2 Assets" Content="&#xE102;" />
```

:::row:::
    :::column span="2":::
        <b>Step 2</b><br>
次のアイコンの要素オブジェクトのいずれかを使用します。[BitmapIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.bitmapicon)、[FontIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.fonticon)、[PathIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.pathicon)、または [SymbolIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.symbolicon)。 これにより、選択するアイコンの種類が多くなり、必要に応じて、テキストなどのその他の種類のコンテンツとアイコンを組み合わせることができるようになります。
    :::column-end:::
    :::column:::
        ![Create an icon button step 2](images/icons/icon-text-step-2.svg)
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
切り取り/コピー/貼り付けや、写真編集プログラムの一連の描画コマンドなど、組み合わされる一連のコマンドがある場合は、それらを[コマンド バー](../controls-and-patterns/app-bars.md)にまとめます。 コマンド バーは、1 つ以上のアプリ バーのボタンまたはアプリ バーのトグル ボタンを取得します。それぞれのボタンはアクションを表します。 それぞれのボタンには、表示されるアイコンを制御するために使用する[アイコン](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.appbarbutton#Windows_UI_Xaml_Controls_AppBarButton_Icon) プロパティがあります。 アイコンを指定するには、さまざまな方法があります。 
    :::column-end:::
    :::column:::
        ![Example of a command bar with icons](images/icons/create-icon-command-bar.svg)
    :::column-end:::
:::row-end:::

最も簡単な方法は、指定した定義済みのアイコンの一覧を使用することです。"戻る" または "停止" などのアイコン名を指定するだけで、システムが描画します。 

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
アイコン名の完全な一覧については、「[Symbol 列挙値](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.symbol)」を参照してください。 

コマンド バーにあるボタンにアイコンを指定する方法は他にもあります。

+ [FontIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.fonticon) - アイコンは指定されたフォント ファミリのグリフに基づきます。
+ [BitmapIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.bitmapicon) - アイコンは指定された **URI** を持つビットマップ画像ファイルに基づきます。
+ [PathIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.pathicon) - アイコンは [Path](/uwp/api/windows.ui.xaml.shapes.path) データに基づきます。

コマンド バーの詳細については、「[コマンド バーの記事](../controls-and-patterns/app-bars.md)」を参照してください。 



## <a name="related-articles"></a>関連記事

* [タイルとアイコン アセットのガイドライン](../shell/tiles-and-notifications/app-assets.md)
