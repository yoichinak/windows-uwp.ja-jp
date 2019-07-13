---
Description: アプリに画像を統合する方法について説明します。Image と ImageBrush という主要な 2 つの XAML クラスの API の使い方について取り上げています。
title: 画像とイメージ ブラシ
ms.assetid: CEA8780C-71A3-4168-A6E8-6361CDFB2FAF
label: Images and image brushes
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 61fa4f8afa0404591831be4136c16672503274f2
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/13/2019
ms.locfileid: "66362783"
---
# <a name="images-and-image-brushes"></a>画像とイメージ ブラシ

画像を表示するには、**Image** オブジェクトまたは **ImageBrush** オブジェクトを使うことができます。 Image オブジェクトは、イメージのレンダリングに使います。ImageBrush オブジェクトは、特定のイメージを使って別のオブジェクトを描画するために使います。 

> **重要な API**:[Image クラス](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image)、[Source プロパティ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image.source)、[ImageBrush クラス](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.ImageBrush)、[ImageSource プロパティ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imagebrush.imagesource)

## <a name="are-these-the-right-elements"></a>これらの要素は適切か。
**Image** 要素を使用して、アプリにスタンドアロンの画像を表示します。

**ImageBrush** を使用して、画像を別のオブジェクトに適用します。 ImageBrush の使用法には、テキストの装飾性を高める効果や、コントロールまたはレイアウト コンテナーの背景などがあります。

## <a name="examples"></a>例

<table>
<th align="left">XAML コントロール ギャラリー<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML コントロール ギャラリー</strong> アプリがインストールされている場合、こちらをクリックして<a href="xamlcontrolsgallery:/item/Image">アプリを開き、Image の動作を確認</a>してください。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML コントロール ギャラリー アプリを入手する (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">ソース コード (GitHub) を入手する</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="create-an-image"></a>画像を作成する

### <a name="image"></a>Image
[Image](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image) オブジェクトを使ってイメージを作成する方法を次の例に示します。


```XAML
<Image Width="200" Source="sunset.jpg" />
```

次に示すのは、レンダリングされた Image オブジェクトです。

![画像要素の例](images/Image_Licorice.jpg)

この例の [Source](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image.source) プロパティは、表示する画像がある場所を指定します。 ソースを設定するには、絶対 URL (http://contoso.com/myPicture.jpg) など) を指定するか、アプリのパッケージ化構造を基準とする相対 URL を指定します。 この例では、プロジェクトのルート フォルダーに "licorice.jpg" 画像ファイルを入れ、この画像ファイルをコンテンツとして含めるプロジェクト設定を宣言しています。

### <a name="imagebrush"></a>ImageBrush

[ImageBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.ImageBrush) オブジェクトを使うと、[Brush](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Brush) オブジェクトを受け付ける領域を、画像を使ってペイントできます。 たとえば、[Ellipse](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Shapes.Ellipse) の [Fill](https://docs.microsoft.com/uwp/api/windows.ui.xaml.shapes.shape.fill) プロパティまたは [Canvas](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Canvas) の [Background](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.background) プロパティの値に ImageBrush を使うことができます。

次の例は、ImageBrush を使って Ellipse をペイントする方法を示しています。

```XAML
<Ellipse Height="200" Width="300">
   <Ellipse.Fill>
     <ImageBrush ImageSource="sunset.jpg" />
   </Ellipse.Fill>
</Ellipse>
```

次に示すのは、ImageBrush でペイントされた Ellipse です。

![ImageBrush でペイントされた Ellipse (楕円)。](images/Image_ImageBrush_Ellipse.jpg)

### <a name="stretch-an-image"></a>画像を拡大する

**Image** の [Width](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.width) 値または [Height](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.height) 値を設定しないと、**Source** で指定した画像の寸法で表示されます。 **Width** と **Height** を設定すると、画像を表示する領域を囲む四角形が作成されます。 この囲まれた領域に画像を描く方法は、[Stretch](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image.stretch) プロパティを使って指定できます。 Stretch プロパティには、[Stretch](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Stretch) 列挙体で定義されている次の値を指定します。

-   **なし**:画像は拡大されず、出力領域全体に描かれません。 この Stretch の設定には注意してください。囲まれた領域よりもソース画像が大きいと、画像はクリップされます。ユーザーは意図的な [Clip](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.clip) で行うような制御をビューポートに対して行うことができないため、通常このことは望ましくありません。
-   **Uniform**: 画像は、出力領域の大きさに合わせて拡大されます。 ただし、コンテンツの縦横比は保たれます。 これが既定値です。
-   **UniformToFill**: 画像は拡大され、出力領域を完全に塗りつぶすように描かれますが、元の縦横比は保たれます。
-   **Fill**: 画像は、出力領域の大きさに合わせて拡大されます。 コンテンツの高さと幅は個々に拡大されるので、元の画像の縦横比は保たれません。 つまり、出力領域を完全に塗りつぶすために、画像がゆがむことがあります。

![ストレッチ設定の例。](images/Image_Stretch.jpg)

### <a name="crop-an-image"></a>画像をトリミングする

[Clip](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.clip) プロパティを使うと、画像出力から一定の領域をクリップできます。 Clip プロパティを [Geometry](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Geometry) に設定します。 現在、四角形以外のクリップはサポートされていません。

画像のクリップ領域として [RectangleGeometry](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.RectangleGeometry) を使う方法を次の例に示します。 この例では、高さ 200 の **Image** オブジェクトを定義します。 **RectangleGeometry** は、表示される画像の領域に長方形を定義します。 [Rect](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.rectanglegeometry.rect) プロパティは "25,25,100,150" に設定されています。これは、四角形が "25,25" の位置から始まり、幅 100、高さ 150 になることを定義しています。 この長方形の領域の内部にだけ、画像が表示されます。

```xaml
<Image Source="sunset.jpg" Height="200">
    <Image.Clip>
        <RectangleGeometry Rect="25,25,100,150" />
    </Image.Clip>
</Image>
```

黒い背景の上にクリップされた画像を次に示します。

![RectangleGeometry によってトリミングされた Image オブジェクト。](images/Image_Cropped.jpg)

### <a name="apply-an-opacity"></a>不透明度を適用する

画像が半透明でレンダリングされるように、画像に [Opacity](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.opacity) を適用することができます。 不透明度の値は、0.0 (完全に透明) ～ 1.0 (完全に不透明) です。 不透明度 0.5 を Image に適用する方法を次の例に示します。

```xaml
<Image Height="200" Source="sunset.jpg" Opacity="0.5" />
```

次に示すのは、不透明度 0.5 と黒い背景でレンダリングされ、部分的に不透明になった画像です。

![不透明度 0.5 の Image オブジェクト。](images/Image_Opacity.jpg)

### <a name="image-file-formats"></a>画像ファイルの形式

**Image** と **ImageBrush** は、次の画像ファイル形式を表示できます。

-   Joint Photographic Experts Group (JPEG)
-   ポータブル ネットワーク グラフィックス (PNG)
-   ビットマップ (BMP)
-   グラフィックス交換形式 (GIF)
-   Tagged Image File Format (TIFF)
-   JPEG XR
-   アイコン (ICO)

[Image](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image)、[BitmapImage](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.BitmapImage)、[BitmapSource](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.BitmapSource) 用の API には、メディア形式のエンコードとデコードを行う専用のメソッドは用意されていません。 エンコード操作とデコード操作はすべてビルトインであり、せいぜいエンコードまたはデコードの局面が読み込みイベント用のイベント データの一部として現れる程度です。 アプリが画像の変換または操作を行うため、画像のエンコードまたはデコードに関連する特別の作業が必要となったときは、[Windows.Graphics.Imaging](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging) 名前空間に用意されている API を使ってください。 これらの API は、Windows の Windows Imaging Component (WIC) でもサポートされます。

Windows 10 バージョン 1607 からは、**Image** 要素で、アニメーション GIF がサポートされるようになりました。 **BitmapImage** を画像の **Source** として使用する場合、BitmapImage API を利用してアニメーション GIF 画像の再生を制御できます。 詳しくは、[BitmapImage](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.BitmapImage) クラスのページの「解説」をご覧ください。

> **注**&nbsp;&nbsp;アニメーション GIF のサポートは、アプリが Windows 10 バージョン 1607 と互換性があり、バージョン 1607 (以降) で実行されている場合に有効です。 アプリがそれ以前のバージョン向けにコンパイルされているか、それらのバージョンで実行されている場合、GIF の最初のフレームは表示されますが、アニメーション効果は得られません。

アプリ リソースについて、およびアプリにイメージ ソースをパッケージ化する方法について詳しくは、「[アプリ リソースの定義](https://docs.microsoft.com/previous-versions/windows/apps/hh965321(v=win.10))」をご覧ください。

### <a name="writeablebitmap"></a>WriteableBitmap

[WriteableBitmap](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.WriteableBitmap) は、WIC からの基本的なファイルに基づくデコードを使わない、修正可能な [BitmapSource](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.BitmapSource) を提供します。 画像を動的に変更し、更新後の画像を再レンダリングできます。 **WriteableBitmap** のバッファーの内容を定義するには、[PixelBuffer](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.writeablebitmap.pixelbuffer) プロパティを使ってバッファーにアクセスし、ストリームまたは言語固有のバッファーの種類を使って入力します。 コード例については、「[WriteableBitmap](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.WriteableBitmap)」をご覧ください。

### <a name="rendertargetbitmap"></a>RenderTargetBitmap

[RenderTargetBitmap](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.RenderTargetBitmap) クラスは、実行中のアプリから XAML UI ツリーをキャプチャして、ビットマップ画像ソースを表現することができます。 キャプチャした画像ソースは、アプリ内の他の部分に適用したり、リソース (つまり、ユーザーによって作成されたアプリ データ) として保存するなど、さまざまなシナリオに利用することができます。 特にお勧めする用途は、実行時における XAML ページのサムネイルをナビゲーション手段として利用することです。たとえば、[Hub](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Hub) コントロールから画像形式のリンクを提供することができます。 キャプチャされた画像に表示されるコンテンツに関して、**RenderTargetBitmap** にはいくつか制限があります。 詳しくは、[RenderTargetBitmap](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.RenderTargetBitmap) の API リファレンス トピックをご覧ください。

### <a name="image-sources-and-scaling"></a>画像ソースとスケーリング

画像ソースは、Windows がスケールするときにアプリで適切に表示されるように、複数の推奨サイズで作る必要があります。 **Image** の **Source** を指定する際には、現在のスケーリングに対応したリソースを自動的に示す名前付け規則を利用できます。 この名前付け規則の詳細や関連情報については、「[クイック スタート: ファイルまたは画像リソースの使用](https://docs.microsoft.com/previous-versions/windows/apps/hh965325(v=win.10))」をご覧ください。

スケーリングの設計方法について詳しくは、「[レイアウトとスケーリングの UX ガイドライン](https://developer.microsoft.com/windows/design)」をご覧ください。

### <a name="image-and-imagebrush-in-code"></a>コードを使った Image と ImageBrush

コードを使うよりも、XAML を使って Image と ImageBrush 要素を指定する方が一般的です。 これは、これらの要素が XAML UI 定義の一部としてのデザイン ツールの出力結果である場合が多いためです。

コードを使って Image または ImageBrush を定義する場合は、既定のコンストラクターを使い、次に、関連するソースのプロパティ ([Image.Source](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image.source) と[ImageBrush.ImageSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imagebrush.imagesource)) を設定します。 ソースのプロパティは、コードを使って設定する場合、[BitmapImage](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.BitmapImage) (URI ではない) を必要と使用します。 ソースがストリームである場合は、[SetSourceAsync](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapsource.setsourceasync) メソッドを使って値を初期化します。 ソースが、**ms-appx** スキームまたは **ms-resource** スキームを使うアプリ内のコンテンツを含む URI である場合は、URI を受け取る [BitmapImage](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.) コンストラクターを使います。 画像ソースが使えるようになるまで代替コンテンツを表示することが必要であるなど、画像ソースの取得やデコードについてタイミングの問題がある場合は、[ImageOpened](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.imageopened) イベントを処理することも検討してください。 コードの例については、[XAML 画像のサンプル](https://go.microsoft.com/fwlink/p/?linkid=238575)をご覧ください。

> [!NOTE]
> コードを利用して画像を確立すると、自動処理を使って、現在のスケール修飾子とカルチャ修飾子で非修飾リソースにアクセスしたり、カルチャとスケールの修飾子で [ResourceManager](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceManager) と [ResourceMap](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceMap) を使って、リソースを直接取得したりできます。 詳しくは、「[リソース管理システム](https://docs.microsoft.com/previous-versions/windows/apps/jj552947(v=win.10))」をご覧ください。

## <a name="get-the-sample-code"></a>サンプル コードを入手する

- [XAML コントロール ギャラリー サンプル](https://github.com/Microsoft/Xaml-Controls-Gallery) - インタラクティブな形で XAML コントロールのすべてを参照できます。

## <a name="related-articles"></a>関連記事

-   [オーディオ、ビデオ、およびカメラ](https://docs.microsoft.com/windows/uwp/audio-video-camera/index)
-   [Image クラス](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image)
-   [ImageBrush クラス](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.ImageBrush)