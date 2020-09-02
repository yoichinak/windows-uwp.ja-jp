---
ms.assetid: 3FD2AA71-EF67-47B2-9332-3FFA5D3703EA
description: この記事では、BitmapDecoder と BitmapEncoder を使って画像ファイルを読み込んだり保存したりする方法のほか、SoftwareBitmap オブジェクトを使ってビットマップ画像を表現する方法について説明します。
title: ビットマップ画像の作成、編集、保存
ms.date: 03/22/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 22053ebe8940053094f704d52b19be2ed3f7bb97
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/02/2020
ms.locfileid: "89362615"
---
# <a name="create-edit-and-save-bitmap-images"></a>ビットマップ画像の作成、編集、保存



この記事では、 [**Bitmapdecoder**](/uwp/api/Windows.Graphics.Imaging.BitmapDecoder) と [**bitmapdecoder**](/uwp/api/Windows.Graphics.Imaging.BitmapEncoder) を使用してイメージファイルを読み込んで保存する方法と、 [**ソフトウェアビットマップ**](/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) オブジェクトを使用してビットマップイメージを表す方法について説明します。

**SoftwareBitmap** クラスは、さまざまなソースから作成できる多用途の API です。画像ファイルや [**WriteableBitmap**](/uwp/api/Windows.UI.Xaml.Media.Imaging.WriteableBitmap) オブジェクト、Direct3D サーフェスから作成できるほか、コードから作成することもできます。 **SoftwareBitmap** を使うと、異なるピクセル形式間やアルファ モード間の変換、ピクセル データへの低レベル アクセスを簡単に行うことができます。 Windows のさまざまな機能のインターフェイスとしても、**SoftwareBitmap** は広く使われています。その例を以下に挙げます。

-   [**CapturedFrame**](/uwp/api/Windows.Media.Capture.CapturedFrame) では、カメラによってキャプチャされたフレームを **SoftwareBitmap** として取得できます。

-   [**VideoFrame**](/uwp/api/Windows.Media.VideoFrame) では、**VideoFrame** の **SoftwareBitmap**表現を取得することができます。

-   [**FaceDetector**](/uwp/api/Windows.Media.FaceAnalysis.FaceDetector) では、**SoftwareBitmap** のフェイスを検出することができます。

この記事のサンプル コードには、以下の名前空間の API が使われています。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/MainPage.xaml.cs" id="SnippetNamespaces":::

## <a name="create-a-softwarebitmap-from-an-image-file-with-bitmapdecoder"></a>BitmapDecoder で画像ファイルから SoftwareBitmap を作成する

[**SoftwareBitmap**](/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) をファイルから作成するには、画像データを含んだ [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) のインスタンスを取得します。 この例では、[**FileOpenPicker**](/uwp/api/Windows.Storage.Pickers.FileOpenPicker) を使って、画像ファイルをユーザーが選択できるようにしています。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/MainPage.xaml.cs" id="SnippetPickInputFile":::

**StorageFile** オブジェクトの [**OpenAsync**](/uwp/api/windows.storage.istoragefile.openasync) メソッドを呼び出して、画像データを含んだランダム アクセス ストリームを取得します。 静的メソッド [**BitmapDecoder.CreateAsync**](/uwp/api/windows.graphics.imaging.bitmapdecoder.createasync) を呼び出して、指定したストリームの [**BitmapDecoder**](/uwp/api/Windows.Graphics.Imaging.BitmapDecoder) クラスのインスタンスを取得します。 [**GetSoftwareBitmapAsync**](/uwp/api/windows.graphics.imaging.bitmapdecoder.getsoftwarebitmapasync) を呼び出して、画像が格納されている [**SoftwareBitmap**](/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) オブジェクトを取得します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/MainPage.xaml.cs" id="SnippetCreateSoftwareBitmapFromFile":::

## <a name="save-a-softwarebitmap-to-a-file-with-bitmapencoder"></a>BitmapEncoder で SoftwareBitmap をファイルに保存する

**SoftwareBitmap** をファイルに保存するには、画像の保存先となる **StorageFile** のインスタンスを取得します。 この例では、[**FileSavePicker**](/uwp/api/Windows.Storage.Pickers.FileSavePicker) を使って、ユーザーが出力ファイルを選択できるピッカーを表示しています。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/MainPage.xaml.cs" id="SnippetPickOutputFile":::

**StorageFile** オブジェクトの [**OpenAsync**](/uwp/api/windows.storage.istoragefile.openasync) メソッドを呼び出して、画像の書き込み先となるランダム アクセス ストリームを取得します。 静的メソッド [**BitmapEncoder.CreateAsync**](/uwp/api/windows.graphics.imaging.bitmapencoder.createasync) を呼び出して、指定したストリームの [**BitmapEncoder**](/uwp/api/Windows.Graphics.Imaging.BitmapEncoder) クラスのインスタンスを取得します。 **CreateAsync** の第 1 パラメーターは、画像のエンコードに使うコーデックの GUID です。 エンコーダーがサポートしている各コーデックについて、この ID を保持するプロパティが、**BitmapEncoder** クラスによって公開されています ([**JpegEncoderId**](/uwp/api/windows.graphics.imaging.bitmapencoder.jpegencoderid) など)。

エンコードの対象となる画像は、[**SetSoftwareBitmap**](/uwp/api/windows.graphics.imaging.bitmapencoder.setsoftwarebitmap) メソッドを使って設定します。 [**BitmapTransform**](/uwp/api/Windows.Graphics.Imaging.BitmapTransform) プロパティの値を設定することで、画像のエンコード中に基本的な変換を適用することができます。 エンコーダーで縮小表示が生成されるかどうかは、[**IsThumbnailGenerated**](/uwp/api/windows.graphics.imaging.bitmapencoder.isthumbnailgenerated) プロパティによって決まります。 ファイル形式によっては縮小表示がサポートされない場合があるので注意してください。この機能を使う場合、縮小表示がサポートされない場合にスローされるエラー (サポート外操作エラー) をキャッチする必要があります。

[**FlushAsync**](/uwp/api/windows.graphics.imaging.bitmapencoder.flushasync) を呼び出すと、指定されたファイルへの画像データの書き込みをエンコーダーが開始します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/MainPage.xaml.cs" id="SnippetSaveSoftwareBitmapToFile":::

その他のエンコード オプションは、**BitmapEncoder** を作成するときに、新しい [**BitmapPropertySet**](/uwp/api/Windows.Graphics.Imaging.BitmapPropertySet) オブジェクトを作成し、そこにエンコーダーの設定を表す [**BitmapTypedValue**](/uwp/api/Windows.Graphics.Imaging.BitmapTypedValue) オブジェクトを渡すことによって指定できます。 サポートされているエンコーダー オプションの一覧については、「[BitmapEncoder オプション リファレンス](bitmapencoder-options-reference.md)」をご覧ください。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/MainPage.xaml.cs" id="SnippetUseEncodingOptions":::

## <a name="use-softwarebitmap-with-a-xaml-image-control"></a>SoftwareBitmap と XAML Image コントロールを使う

[**Image**](/uwp/api/Windows.UI.Xaml.Controls.Image) コントロールを使って XAML ページ内に画像を表示するには、まず XAML ページで **Image** コントロールを定義します。

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/MainPage.xaml" id="SnippetImageControl":::

現時点では、**Image** コントロールは、BGRA8 エンコードを使用し、プリマルチプライ処理済みまたはアルファ チャネルなしの画像のみをサポートします。 画像を表示する前に、画像の形式が正しいことをテストしてください。形式が不適切な場合は、**SoftwareBitmap** の [**Convert**](/uwp/api/windows.graphics.imaging.softwarebitmap.convert) メソッドを使用して、サポートされる形式に画像を変換してください。

新しい [**SoftwareBitmapSource**](/uwp/api/Windows.UI.Xaml.Media.Imaging.SoftwareBitmapSource) オブジェクトを作ります。 [**SetBitmapAsync**](/uwp/api/windows.ui.xaml.media.imaging.softwarebitmapsource.setbitmapasync) を呼び出し、**SoftwareBitmap** で渡して、ソース オブジェクトの内容を設定します。 その新しく作成した **SoftwareBitmapSource** を、**Image** コントロールの [**Source**](/uwp/api/windows.ui.xaml.controls.image.source) プロパティに設定します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/MainPage.xaml.cs" id="SnippetSoftwareBitmapToWriteableBitmap":::

**SoftwareBitmapSource** を [**ImageBrush**](/uwp/api/Windows.UI.Xaml.Media.ImageBrush) の [**ImageSource**](/uwp/api/windows.ui.xaml.media.imagebrush.imagesource) として使用して **SoftwareBitmap** を設定することもできます。

## <a name="create-a-softwarebitmap-from-a-writeablebitmap"></a>WriteableBitmap から SoftwareBitmap を作成する

[**SoftwareBitmap.CreateCopyFromBuffer**](/uwp/api/windows.graphics.imaging.softwarebitmap.createcopyfrombuffer) を呼び出して、**WriteableBitmap** の **PixelBuffer** プロパティを指定することで、既存の **WriteableBitmap** から **SoftwareBitmap** を作成し、ピクセル データを設定することができます。 新しく作成する **WriteableBitmap** のピクセル形式は第 2 引数で指定できます。 新しい画像のサイズは、**WriteableBitmap** の [**PixelWidth**](/uwp/api/windows.ui.xaml.media.imaging.bitmapsource.pixelwidth) プロパティと [**PixelHeight**](/uwp/api/windows.ui.xaml.media.imaging.bitmapsource.pixelheight) プロパティを使って指定してください。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/MainPage.xaml.cs" id="SnippetWriteableBitmapToSoftwareBitmap":::

## <a name="create-or-edit-a-softwarebitmap-programmatically"></a>SoftwareBitmap をプログラムから作成または編集する

ここまでは、画像ファイルを使った方法を紹介してきました。 新しい **SoftwareBitmap** をプログラム コードから作成し、同じ手法を用いて **SoftwareBitmap** のピクセル データにアクセスし、変更を加えることもできます。

**SoftwareBitmap** では、ピクセル データを含んだ RAW バッファーが、COM 相互運用機能を使って公開されます。

COM 相互運用機能を使うには、**System.Runtime.InteropServices** 名前空間の参照をプロジェクトに追加する必要があります。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/MainPage.xaml.cs" id="SnippetInteropNamespace":::

COM インターフェイス [**IMemoryBufferByteAccess**](/previous-versions/mt297505(v=vs.85)) を初期化するには、対象の名前空間に以下のコードを追加します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/MainPage.xaml.cs" id="SnippetCOMImport":::

必要なピクセル形式とサイズを指定して新しい **SoftwareBitmap** を作成します。 既にある **SoftwareBitmap** のピクセル データを編集する必要がある場合は、その SoftwareBitmap を使ってもかまいません。 [**SoftwareBitmap.LockBuffer**](/uwp/api/windows.graphics.imaging.softwarebitmap.lockbuffer) を呼び出して、ピクセル データ バッファーを表す [**BitmapBuffer**](/uwp/api/Windows.Graphics.Imaging.BitmapBuffer) クラスのインスタンスを取得します。 **BitmapBuffer** を COM インターフェイス **IMemoryBufferByteAccess** にキャストしたうえで [**IMemoryBufferByteAccess.GetBuffer**](/windows/desktop/WinRT/imemorybufferbyteaccess-getbuffer) を呼び出し、バイト配列にデータを設定します。 ピクセルごとにバッファーのオフセットを計算しやすいよう、[**BitmapBuffer.GetPlaneDescription**](/uwp/api/windows.graphics.imaging.bitmapbuffer.getplanedescription) メソッドを使って [**BitmapPlaneDescription**](/uwp/api/Windows.Graphics.Imaging.BitmapPlaneDescription) オブジェクトを取得します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/MainPage.xaml.cs" id="SnippetCreateNewSoftwareBitmap":::

このメソッドは、Windows ランタイム型よりも低いレベルの RAW バッファーにアクセスするため、**unsafe** キーワードを使って宣言する必要があります。 また、Microsoft Visual Studio でアンセーフ コードのコンパイルを許可するようにプロジェクトを構成する必要があります。プロジェクトの **[プロパティ]** ページを開き、**[ビルド]** プロパティ ページをクリックして、**[アンセーフ コードの許可]** チェック ボックスをオンにしてください。

## <a name="create-a-softwarebitmap-from-a-direct3d-surface"></a>Direct3D サーフェスから SoftwareBitmap を作成する

Direct3D サーフェスから **SoftwareBitmap** オブジェクトを作成するには、プロジェクトで [**Windows.Graphics.DirectX.Direct3D11**](/uwp/api/Windows.Graphics.DirectX.Direct3D11) 名前空間を追加する必要があります。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/MainPage.xaml.cs" id="SnippetDirect3DNamespace":::

サーフェスから新しい **SoftwareBitmap** を作成するには、[**CreateCopyFromSurfaceAsync**](/uwp/api/windows.graphics.imaging.softwarebitmap.createcopyfromsurfaceasync) を呼び出します。 この名前を見るとわかるように、新しい **SoftwareBitmap** には、画像データのコピーが別に存在します。 **SoftwareBitmap** に変更を加えても、Direct3D サーフェスには一切影響しません。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/MainPage.xaml.cs" id="SnippetCreateSoftwareBitmapFromSurface":::

## <a name="convert-a-softwarebitmap-to-a-different-pixel-format"></a>SoftwareBitmap を異なるピクセル形式に変換する

**SoftwareBitmap** クラスの静的メソッド [**Convert**](/uwp/api/windows.graphics.imaging.softwarebitmap.convert) を使うと、既にある **SoftwareBitmap** から、指定したピクセル形式とアルファ モードを使った新しい **SoftwareBitmap** を簡単に作成することができます。 新しく作成されたビットマップには、画像データのコピーが別に存在します。 新しいビットマップに変更を加えても、元のビットマップには一切影響しません。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/MainPage.xaml.cs" id="SnippetConvert":::

## <a name="transcode-an-image-file"></a>画像ファイルのトランスコード

画像ファイルを [**BitmapDecoder**](/uwp/api/Windows.Graphics.Imaging.BitmapDecoder) から [**BitmapEncoder**](/uwp/api/Windows.Graphics.Imaging.BitmapEncoder) に直接トランスコードすることができます。 トランスコードの対象となるファイルから [**IRandomAccessStream**](/uwp/api/Windows.Storage.Streams.IRandomAccessStream) を作成します。 入力ストリームから新しい **BitmapDecoder** を作成します。 エンコーダーの書き込み先となる新しい [**InMemoryRandomAccessStream**](/uwp/api/Windows.Storage.Streams.InMemoryRandomAccessStream) を作成し、[**BitmapEncoder.CreateForTranscodingAsync**](/uwp/api/windows.graphics.imaging.bitmapencoder.createfortranscodingasync) を呼び出します。このとき、引数にインメモリ ストリームとデコーダー オブジェクトを渡します。 トランスコードではエンコード オプションはサポートされません。オプションを指定する場合は、代わりに [**CreateAsync**](/uwp/api/windows.graphics.imaging.bitmapencoder.createasync) を使う必要があります。 入力画像ファイルのプロパティのうち、エンコーダーに対して明示的に指定しなかったプロパティはすべて、元のまま出力ファイルに書き込まれます。 [**FlushAsync**](/uwp/api/windows.graphics.imaging.bitmapencoder.flushasync) を呼び出すと、インメモリ ストリームへのエンコードをエンコーダーが開始します。 最後に、ファイル ストリームとインメモリ ストリームを先頭までシークし、[**CopyAsync**](/uwp/api/windows.storage.streams.randomaccessstream.copyasync) を呼び出してインメモリ ストリームをファイル ストリームに書き込みます。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/MainPage.xaml.cs" id="SnippetTranscodeImageFile":::

## <a name="related-topics"></a>関連トピック

* [BitmapEncoder オプションのリファレンス](bitmapencoder-options-reference.md)
* [イメージのメタデータ](image-metadata.md)
 

 
