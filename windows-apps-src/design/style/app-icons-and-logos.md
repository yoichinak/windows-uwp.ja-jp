---
description: スタート メニュー、アプリ タイル、タスク バー、Microsoft Store などでアプリを表すアプリ アイコン/ロゴを作成する方法。
title: アプリのアイコンとロゴ
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
design-contact: Judysa
doc-status: Published
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 4e908cbad1fb0b70fe96af50917e8b895fdda90d
ms.sourcegitcommit: d786d084dafee5da0268ebb51cead1d8acb9b13e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/09/2020
ms.locfileid: "91860127"
---
# <a name="app-icons-and-logos"></a>アプリのアイコンとロゴ 

すべてのアプリにはそのアプリを表すアイコンやロゴがあり、Windows シェルの複数の場所でそのアイコンが表示されます。 

:::row:::
    :::column:::
        * スタート メニューのアプリ一覧
        * タスク バーとタスク マネージャー
        * アプリのタイル
        * アプリのスプラッシュ スクリーン
        * Microsoft Store 内
    :::column-end:::
    :::column:::
        ![Windows 10 のスタート画面とタイル](images/assetguidance01.jpg)
    :::column-end:::
:::row-end:::

この記事では、アプリ アイコンの作成の基本、およびアプリ アイコンを Visual Studio で管理する方法と手動で管理する方法 (必要な場合) について説明します。
 
(この記事は、特にアプリ自体を表すアイコンを対象としています。一般的なアイコンに関するガイダンスについては、[アイコンに関する記事](icons.md)をご覧ください。)

## <a name="icon-types-locations-and-scale-factors"></a>アイコンの種類、場所、および倍率

Visual Studio では、既定でアイコン アセットがアセット サブディレクトリに格納されます。 以下に示すのは、各種のアイコン、それが表示される場所、およびその名前の一覧です。 

| アイコン名 | 表示される場所 | アセット ファイル名 |
| ---      | ---        | --- |
| 小さいタイル | スタート メニュー |  SmallTile.png  |
| 普通サイズのタイル |スタート メニュー、Microsoft Store 登録情報\*  |  Square150x150Logo.png |
| ワイド タイル  | スタート メニュー   | Wide310x150Logo.png |
| 大きいタイル   | スタート メニュー、Microsoft Store 登録情報\* |  LargeTile.png  |
| アプリのアイコン | スタート メニューのアプリ一覧、タスク バー、タスク マネージャー | Square44x44Logo.png |
| スプラッシュ スクリーン | アプリのスプラッシュ スクリーン | SplashScreen.png  |
| バッジ ロゴ | アプリのタイル | BadgeLogo.png  |
| パッケージ ロゴ/Microsoft Store ロゴ | アプリ インストーラー、パートナー センター、Microsoft Store の [Report an app]\(アプリを報告)\ オプション、Microsoft Store の [Write a review]\(レビューを書く)\ オプション | StoreLogo.png  |

\*[アップロードした画像のみを Microsoft Store で表示](../../publish/app-screenshots-and-images.md#display-only-uploaded-logo-images-in-the-store)することを選択しなかった場合に使用されます。 

これらのアイコンがどの画面でも鮮明に表示されるようにするには、同じアイコンに、異なる表示倍率に対応する複数のバージョンを作成します。 

倍率によって、テキストなどの UI 要素のサイズが決まります。 倍率の範囲は 100% から 400% までです。 値が大きいほど UI 要素も大きくなり、高解像度ディスプレイで見やすくなります。 

:::row:::
   :::column:::
      Windows では、ディスプレイの DPI (1 インチあたりのドット数) と、デバイスの視聴距離に基づいて各ディスプレイの倍率が自動的に設定されます 
      (ユーザーは、 **[設定] &gt; [ディスプレイ] &gt; [拡大縮小とレイアウト]** のページに移動して既定値を上書きできます)。
   :::column-end:::
   :::column:::
      ![[設定] の [ディスプレイ] ページのスクリーンショット。](images/icons/display-settings-screen.png)
   :::column-end:::
:::row-end:::  


アプリ アイコン アセットはビットマップであり、ビットマップは適切に拡大されないため、アイコン アセットごとに次の各倍率に対応するバージョンを作成することをお勧めします: 100%、125%、150%、200%、および 400%。 これを行うとアイコンの数が増えてしまいます。 Visual Studio には、これらのアイコンを簡単に生成して更新するためのツールが用意されています。 

## <a name="microsoft-store-listing-image"></a>Microsoft Store 登録情報の画像

"Microsoft Store で自分のアプリの登録情報の画像を指定するにはどうしたらいいでしょうか。"

このページの上部の表で説明しているように、Microsoft Store では既定でパッケージ内の画像が使用されます ([申請プロセスで指定したその他の画像](../../publish/app-screenshots-and-images.md)とともに)。 ただし、Windows 10 (Xbox を含む) ユーザーに Microsoft Store 登録情報が表示されるときに、アプリのパッケージのロゴ画像ではなく、アップロードした画像のみが使用されるようにすることもできます。 これにより、ストアのさまざまな画面でアプリがどのように表示されるかをさらに細かく制御できます。 (製品が以前の OS バージョンをサポートしている場合は、このオプションを使用してもパッケージの画像が引き続きユーザーに表示される場合があるので注意してください。)これは、申請プロセスの **[Store 登録情報]** の手順の **[Microstore Store ロゴ]** セクションで行うことができます。

![アプリ申請プロセスでの Microsoft Store ロゴの指定](images/app-icons/storelogodisplay.png)

このチェック ボックスをオンにすると、 **[Microsoft Store 表示用の画像]** という新しいセクションが表示されます。 ここでは、Microsoft Store でアプリのパッケージのロゴ画像に代わって使用される次の 3 つの画像サイズをアップロードできます: 300 x 300、150 x 150、71 x 71 ピクセル。 必須なのは 300 x 300 サイズだけですが、3 つのサイズをすべて指定することをお勧めします。

詳細については、[アップロードしたロゴ画像のみを Microsoft Store で表示する方法に関する記事](../../publish/app-screenshots-and-images.md#display-only-uploaded-logo-images-in-the-store)をご覧ください。

<!-- ### Fallback images for the Store

The simplest way to control the Store listing image is to specify it during the app submission process. If you don't provide these images during the app submission process, the Store will use a tile image:

1. Large tile
2. Medium tile

If these images aren't provided, the Store will search all matching images of the same image type with a square aspect ratio, preferable with a height greated than the scaled requested height (scaled height is the machine's resolution scale factor * display height). If none of the images meet this criteria, the Store will ignore the scale factor and select an image based on height.  -->

<!-- You can provide screenshots, logos, and other art assets (such as trailers and promotional images to include in your app's Microsoft Store listing. Some of these are required, and some are optional (although some of the optional images are important to include for the best Store display).

The Store may also use your app's tile and other images that you include in your app's package. 

For more information, see [App screenshots, images, and trailers in the Microsoft Store](../../publish/app-screenshots-and-images.md). -->


## <a name="managing-app-icons-with-the-visual-studio-manifest-designer"></a>Visual Studio マニフェスト デザイナーでのアプリ アイコンの管理

Visual Studio には、**マニフェスト デザイナー**と呼ばれるアプリ アイコンを管理するための便利なツールがあります。 

> まだ Visual Studio 2019 を使用していない場合は、無料バージョン (Visual Studio 2019 Community Edition) を含むいくつかのバージョンを利用できます。また、それ以外のバージョンでも無料試用版が提供されています。 いずれも次の場所からダウンロードできます。[https://developer.microsoft.com/windows/downloads](https://developer.microsoft.com/windows/downloads)


マニフェスト デザイナーを起動するには、次の手順を実行します。
<!-- 1. Use Visual Studio to open a UWP project.
2. In the **Solution Explorer**, double-click the package.appmanifest file. 

    ![The Visual Studio 2019 Solution Explorer](images/icons/vs-solution-explorer.png)

    Visual Studio displays the manifest designer.

    ![The Visual Studio 2019 manifest designer](images/icons/vs-manfiest-designer.png)
3. Click the **Visual Assets** tab.

    ![The Visual Assets tab](images/icons/vs-manfiest-designer-visual-assets.png) -->


:::row:::
    :::column:::
        1. Visual Studio で UWP プロジェクトを開きます。
    :::column-end:::
    :::column:::
        
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        2. **[ソリューション エクスプローラー]** で Package.appxmanifest ファイルをダブルクリックします。
    :::column-end:::
    :::column:::
        ![Visual Studio 2019 マニフェスト デザイナー](images/icons/vs-solution-explorer.png)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
            Visual Studio によってマニフェスト デザイナーが表示されます。
    :::column-end:::
    :::column:::
            ![マニフェスト デザイナーで [アプリケーション] タブが表示されているスクリーンショット。](images/icons/vs-manfiest-designer.png)
    :::column-end:::
:::row-end:::    
:::row:::
    :::column:::
        3. **[ビジュアル資産]** タブをクリックします。
    :::column-end:::
    :::column:::
        ![マニフェスト デザイナーで [ビジュアル資産] タブが表示されているスクリーンショット。](images/icons/vs-manfiest-designer-visual-assets.png)
    :::column-end:::
:::row-end:::        

## <a name="generating-all-assets-at-once"></a>すべてのアセットの一括生成

**[ビジュアル資産]** タブの最初のメニュー項目 **[すべてのビジュアル資産]** では、まさにその名前が示すように、ボタンのクリックによってアプリに必要なすべてのビジュアル アセットが生成されます。

![Visual Studio ですべてのビジュアル アセットを生成](images/app-icons/all-visual-assets.png)

画像を 1 つ指定するだけで、小さいタイル、普通サイズのタイル、大きいタイル、ワイド タイル、アプリ アイコン、スプラッシュ スクリーン、パッケージ ロゴの各アセットのすべての倍率に対応するバージョンが、Visual Studio によって生成されます。

すべてのアセットを一括で生成するには、次の手順を実行します。
1. **[ソース]** フィールドの横の **[...]** をクリックし、使用する画像を選択します。 ビットマップ画像を使用する場合は、鮮明な画像になるように、400 × 400 ピクセル以上を指定してください。 ベクトル ベースの画像が最適です。Visual Studio では AI (Adobe Illustrator) ファイルや PDF ファイルを使用できます。 
2. (省略可能) **[設定値を表示]** セクションで、以下のオプションを設定します。

    a。  **短い名前**:アプリの短い名前を指定します。

    b.  **名前の表示**:普通サイズのタイル、ワイド タイル、または大きいタイルに短い名前を表示するかどうかを指定します。 

    c. **タイルの背景**:タイルの背景色に対応する 16 進値または色名を指定します。 たとえば、`#464646` のように指定します。 既定値は `transparent` です。

    d. **スプラッシュ スクリーンの背景**:スプラッシュ スクリーンの背景色に対応する 16 進値または色名を指定します。 

3. **[生成]** をクリックします。 

Visual Studio で画像ファイルが生成され、プロジェクトに追加されます。 アセットを変更する場合は、このプロセスを繰り返します。 

拡大されたアイコン アセットのファイルには、次の規則に従って名前が付けられます。

*filename*-scale-*scale factor*.png

たとえば、

Square150x150Logo-scale-100.png、Square150x150Logo-scale-200.png、Square150x150Logo-scale-400.png

Visual Studio では既定でバッジ ロゴが生成されません。 これは、バッジ ロゴが独特であり、他のアプリ アイコンと一致していることがまずないためです。 詳しくは、「[Windows アプリ向けのバッジ通知](../shell/tiles-and-notifications/badges.md)」をご覧ください。 


## <a name="more-about-app-icon-assets"></a>アプリ アイコン アセットの詳細
Visual Studio では、プロジェクトで必要なすべてのアプリ アイコン アセットが生成されますが、それらのカスタマイズが必要となった場合に、他のアプリ アセットとの違いを確認できるようになっています。 

アプリ アイコン アセットは、Windows タスク バー、タスク ビュー、Alt + Tab キー、スタート タイルの右下など、さまざまな場所に表示されます。 多くの場所に表示されることから、アプリ アイコン アセットには、他のアセットにはない、サイズとプレートを設定するオプションがあります ("ターゲット サイズ" のアセットと "プレートなし" のアセット)。 

### <a name="target-size-app-icon-assets"></a>ターゲット サイズのアプリ アイコン アセット
標準の倍率サイズ ("Square44x44Logo.scale 400.png") に加えて、"ターゲット サイズ" のアセットを作成することをお勧めします。 このアセットは、特定の倍率 (400 など) ではなく特定のサイズ (16 ピクセルなど) をターゲットとしていることから、ターゲット サイズと呼ばれています。 ターゲット サイズのアセットは、表示スケール プラトー システムを使用しないサーフェスに使用されます。

* スタート画面のジャンプ リスト (デスクトップ)
* スタート画面のタイルの下隅 (デスクトップ)
* ショートカット (デスクトップ)
* コントロール パネル (デスクトップ)

ターゲット サイズのアセットの一覧を次に示します。


| アセットのサイズ | ファイル名の例                  |
|------------|------------------------------------|
| 16 x 16\*    | Square44x44Logo.targetsize-16.png  |
| 24 x 24\*    | Square44x44Logo.targetsize-24.png  |
| 32 x 32\*    | Square44x44Logo.targetsize-32.png  |
| 48 x 48\*    | Square44x44Logo.targetsize-48.png  |
| 256 x 256\*  | Square44x44Logo.targetsize-256.png |
| 20 x 20      | Square44x44Logo.targetsize-20.png  |
| 30 x 30      | Square44x44Logo.targetsize-30.png  |
| 36 x 36      | Square44x44Logo.targetsize-36.png  |
| 40 x 40      | Square44x44Logo.targetsize-40.png  |
| 60 x 60      | Square44x44Logo.targetsize-60.png  |
| 64 x 64      | Square44x44Logo.targetsize-64.png  |
| 72 x 72      | Square44x44Logo.targetsize-72.png  |
| 80 x 80      | Square44x44Logo.targetsize-80.png  |
| 96 x 96      | Square44x44Logo.targetsize-96.png  |

\* 少なくとも上記のサイズを設定することをお勧めします。 

これらのアセットにパディングを追加する必要はありません。パディングは、必要に応じて Windows によって追加されます。 これらのアセットは、16 ピクセルの最小面積を占めている必要があります。 

Windows タスク バーのアイコンに表示される、このようなアセットの例を以下に示します。

![Windows タスク バーのアセット](images/assetguidance21.png)

### <a name="unplated-assets"></a>プレートなしのアセット
Windows では、ターゲット ベースのアセットが既定で色付きのバックプレートとともに使用されます。 必要に応じて、ターゲット ベースのアセットをプレートなしにすることができます。 "プレートなし" とは、アセットが透明な背景の上に表示されることを意味します。 この場合、アセットがさまざまな背景色の上に表示されることに留意してください。 

![プレートなしのアセットとプレート付きのアセット](images/assetguidance22.png)

次に示すサーフェスでは、プレートなしのアプリ アイコン アセットが使用されています。
* タスク バーとタスク バー サムネイル (デスクトップ)
* タスク バーのジャンプリスト
* タスク ビュー
* Alt + Tab

### <a name="unplated-assets-and-themes"></a>プレートなしのアセットとテーマ

ユーザーが選択したテーマによってタスク バーの色が決まります。 プレートなしのアセットが現在のテーマに厳密に対応していない場合は、システムでアセットのコントラストがチェックされます。 タスク バーでのコントラストが十分であれば、そのまま使用されます。 そうでない場合、システムはそのアセットのハイコントラスト バージョンを探します。 これが見つからなかった場合は、アセットがプレート付きの形式で描画されます。 


### <a name="target-and-unplated-sizing"></a>ターゲットおよびプレートなしのサイズ調整

次に示すのは、100% の倍率でのターゲット ベースのアセットのサイズに関する推奨事項です。

![100% の倍率でのターゲット ベースのアセットのサイズ調整](images/assetguidance23.png)


## <a name="more-about-splash-screen-assets"></a>スプラッシュ スクリーン アセットの詳細
スプラッシュ スクリーンについて詳しくは、[Windows アプリのスプラッシュ スクリーン](../../launch-resume/splash-screens.md)に関する記事をご覧ください。

## <a name="more-about-badge-logo-assets"></a>バッジ ロゴ アセットの詳細

資産ジェネレーターを使用して必要なアセットをすべて生成する場合、バッジ ロゴは既定で生成されません。これは、バッジ ロゴが他のアプリ アセットと大きく異なっているからです。 バッジ ロゴは、通知やアプリのタイルに表示される状態の画像です。 

詳しくは、「[Windows アプリ向けのバッジ通知](../shell/tiles-and-notifications/badges.md)」をご覧ください。


## <a name="customizing-asset-padding"></a>アセットの埋め込みのカスタマイズ

Visual Studio の資産ジェネレーターでは、どの画像にも既定で推奨の埋め込みが適用されます。 画像にすでに埋め込みが含まれている場合、またはタイルの端まで拡大されたフルブリード画像が必要である場合は、 **[推奨の埋め込みを適用します]** チェック ボックスをオフにしてこの機能をオフにすることができます。 

### <a name="tile-padding-recommendations"></a>タイルの埋め込みに関する推奨事項
タイルに独自の埋め込みを適用する場合の推奨事項を以下に示します。 

タイル サイズには、小 (71 x 71)、普通サイズ (150 x 150)、ワイド (310 x 150)、大 (310 x 310) の 4 種類があります。 

各タイル アセットは、配置されるタイルと同じサイズです。

![フルブリードのタイル](images/app-icons/tile-assets1.png)

アイコンをタイルの端まで拡大しない場合は、アセットに透明のピクセルで埋め込みを作ることができます。 

![タイルとバック プレート](images/assetguidance05.png)

小さいタイルでは、アイコンの幅と高さをタイル サイズの 66% に制限します。

![小さいタイルのサイズの比率](images/assetguidance09.png)

普通サイズのタイルでは、アイコンの幅をタイル サイズの 66% に、高さをタイル サイズの 50% に制限します。 これによって、ブランド バー内の要素と重ならないようにします。

![普通サイズのタイルのサイズの比率](images/assetguidance10.png)

ワイド タイルでは、アイコンの幅をタイル サイズの 66% に、高さをタイル サイズの 50% に制限します。 これによって、ブランド バー内の要素と重ならないようにします。

![ワイド タイルのサイズの比率](images/assetguidance11.png)

大きいタイルでは、アイコンの幅をタイル サイズの 66% に、高さをタイル サイズの 50% に制限します。

![大きいタイルのサイズの比率](images/assetguidance12.png)

水平方向または垂直方向にデザインされたアイコンがある一方で、ターゲット サイズの正方形に収まらない、より複雑な形状のアイコンもあります。 中央に配置されるアイコンの一方の辺に重みを付けることができます。 この場合、アイコンの視覚的な重みが正方形に収まるアイコンと同じであれば、アイコンの一部が推奨される面積の外側にはみ出していてもかまいません。

![中央に配置された 3 つのアイコン](images/assetguidance13.png)

フルブリード アセットでは、要素が余白およびタイルの端の内側に接するように考慮します。 タイルの高さまたは幅の 16% 以上の余白を維持します。 この割合は、最小タイル サイズでの余白の幅の 2 倍を表しています。

![余白のあるフルブリード タイル](images/assetguidance14.png)

次の例では、余白が狭すぎます。

![余白が小さすぎるフルブリード タイル](images/assetguidance15.png)


## <a name="optimizing-for-specific-themes-languages-and-other-conditions"></a>特定のテーマ、言語、およびその他の条件に合わせた最適化 

この記事では、特定の倍率に対応したアセットを作成する方法について説明しましたが、各種の条件やさまざまな組み合わせの条件に合わせてアセットを作成することもできます。 たとえば、ハイ コントラスト表示や、ライト テーマとダーク テーマに対応したアイコンを作成できます。 特定の言語に対応したアセットを作成することもできます。

手順については、「[言語、スケール、ハイコントラスト、その他の修飾子用にリソースを調整する](../../app-resources/tailor-resources-lang-scale-contrast.md)」をご覧ください。
