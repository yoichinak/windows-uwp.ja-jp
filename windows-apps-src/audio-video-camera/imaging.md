---
ms.assetid: 3FD2AA71-EF67-47B2-9332-3FFA5D3703EA
description: この記事では、BitmapDecoder と BitmapEncoder を使って画像ファイルを読み込んだり保存したりする方法のほか、SoftwareBitmap オブジェクトを使ってビットマップ画像を表現する方法について説明します。
title: ビットマップ画像の作成、編集、保存
ms.date: 03/22/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 7c61a35f0ad35cf85afcba564eb676aa171b0243
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/29/2019
ms.locfileid: "66360843"
---
# <a name="create-edit-and-save-bitmap-images"></a>ビットマップ画像の作成、編集、保存



この記事では、[**BitmapDecoder**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapDecoder) と [**BitmapEncoder**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapEncoder) を使って画像ファイルを読み込んだり保存したりする方法のほか、[**SoftwareBitmap**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) オブジェクトを使ってビットマップ画像を表現する方法について説明します。

**SoftwareBitmap** クラスは、さまざまなソースから作成できる多用途の API です。画像ファイルや [**WriteableBitmap**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.WriteableBitmap) オブジェクト、Direct3D サーフェスから作成できるほか、コードから作成することもできます。 **SoftwareBitmap** を使うと、異なるピクセル形式間やアルファ モード間の変換、ピクセル データへの低レベル アクセスを簡単に行うことができます。 Windows のさまざまな機能のインターフェイスとしても、**SoftwareBitmap** は広く使われています。その例を以下に挙げます。

-   [**CapturedFrame** ](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.CapturedFrame)としてカメラによってキャプチャされたフレームを取得することができます、 **SoftwareBitmap**します。

-   [**VideoFrame** ](https://docs.microsoft.com/uwp/api/Windows.Media.VideoFrame)を取得することができます、 **SoftwareBitmap**の表現、 **VideoFrame**します。

-   [**FaceDetector** ](https://docs.microsoft.com/uwp/api/Windows.Media.FaceAnalysis.FaceDetector)で顔を検出することができます、 **SoftwareBitmap**します。

この記事のサンプル コードには、以下の名前空間の API が使われています。

[!code-cs[Namespaces](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetNamespaces)]

## <a name="create-a-softwarebitmap-from-an-image-file-with-bitmapdecoder"></a>BitmapDecoder で画像ファイルから SoftwareBitmap を作成する

[  **SoftwareBitmap**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) をファイルから作成するには、画像データを含んだ [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) のインスタンスを取得します。 この例では、[**FileOpenPicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker) を使って、画像ファイルをユーザーが選択できるようにしています。

[!code-cs[PickInputFile](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetPickInputFile)]

**StorageFile** オブジェクトの [**OpenAsync**](https://docs.microsoft.com/uwp/api/windows.storage.istoragefile.openasync) メソッドを呼び出して、画像データを含んだランダム アクセス ストリームを取得します。 静的メソッド [**BitmapDecoder.CreateAsync**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapdecoder.createasync) を呼び出して、指定したストリームの [**BitmapDecoder**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapDecoder) クラスのインスタンスを取得します。 [  **GetSoftwareBitmapAsync**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapdecoder.getsoftwarebitmapasync) を呼び出して、画像が格納されている [**SoftwareBitmap**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) オブジェクトを取得します。

[!code-cs[CreateSoftwareBitmapFromFile](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetCreateSoftwareBitmapFromFile)]

## <a name="save-a-softwarebitmap-to-a-file-with-bitmapencoder"></a>BitmapEncoder で SoftwareBitmap をファイルに保存する

**SoftwareBitmap** をファイルに保存するには、画像の保存先となる **StorageFile** のインスタンスを取得します。 この例では、[**FileSavePicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileSavePicker) を使って、ユーザーが出力ファイルを選択できるピッカーを表示しています。

[!code-cs[PickOutputFile](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetPickOutputFile)]

**StorageFile** オブジェクトの [**OpenAsync**](https://docs.microsoft.com/uwp/api/windows.storage.istoragefile.openasync) メソッドを呼び出して、画像の書き込み先となるランダム アクセス ストリームを取得します。 静的メソッド [**BitmapEncoder.CreateAsync**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapencoder.createasync) を呼び出して、指定したストリームの [**BitmapEncoder**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapEncoder) クラスのインスタンスを取得します。 **CreateAsync** の第 1 パラメーターは、画像のエンコードに使うコーデックの GUID です。 エンコーダーがサポートしている各コーデックについて、この ID を保持するプロパティが、**BitmapEncoder** クラスによって公開されています ([**JpegEncoderId**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapencoder.jpegencoderid) など)。

エンコードの対象となる画像は、[**SetSoftwareBitmap**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapencoder.setsoftwarebitmap) メソッドを使って設定します。 [  **BitmapTransform**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapTransform) プロパティの値を設定することで、画像のエンコード中に基本的な変換を適用することができます。 エンコーダーで縮小表示が生成されるかどうかは、[**IsThumbnailGenerated**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapencoder.isthumbnailgenerated) プロパティによって決まります。 ファイル形式によっては縮小表示がサポートされない場合があるので注意してください。この機能を使う場合、縮小表示がサポートされない場合にスローされるエラー (サポート外操作エラー) をキャッチする必要があります。

[  **FlushAsync**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapencoder.flushasync) を呼び出すと、指定されたファイルへの画像データの書き込みをエンコーダーが開始します。

[!code-cs[SaveSoftwareBitmapToFile](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetSaveSoftwareBitmapToFile)]

その他のエンコード オプションは、**BitmapEncoder** を作成するときに、新しい [**BitmapPropertySet**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapPropertySet) オブジェクトを作成し、そこにエンコーダーの設定を表す [**BitmapTypedValue**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapTypedValue) オブジェクトを渡すことによって指定できます。 サポートされているエンコーダー オプションの一覧については、「[BitmapEncoder オプション リファレンス](bitmapencoder-options-reference.md)」をご覧ください。

[!code-cs[UseEncodingOptions](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetUseEncodingOptions)]

## <a name="use-softwarebitmap-with-a-xaml-image-control"></a>SoftwareBitmap と XAML Image コントロールを使う

[  **Image**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image) コントロールを使って XAML ページ内に画像を表示するには、まず XAML ページで **Image** コントロールを定義します。

[!code-xml[ImageControl](./code/ImagingWin10/cs/MainPage.xaml#SnippetImageControl)]

現時点では、**Image** コントロールは、BGRA8 エンコードを使用し、プリマルチプライ処理済みまたはアルファ チャネルなしの画像のみをサポートします。 画像を表示する前に、画像の形式が正しいことをテストしてください。形式が不適切な場合は、**SoftwareBitmap** の [**Convert**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.softwarebitmap.windows) メソッドを使用して、サポートされる形式に画像を変換してください。

新しい [**SoftwareBitmapSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.SoftwareBitmapSource) オブジェクトを作ります。 [  **SetBitmapAsync**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.softwarebitmapsource.setbitmapasync) を呼び出し、**SoftwareBitmap** で渡して、ソース オブジェクトの内容を設定します。 その新しく作成した **SoftwareBitmapSource** を、**Image** コントロールの [**Source**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image.source) プロパティに設定します。

[!code-cs[SoftwareBitmapToWriteableBitmap](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetSoftwareBitmapToWriteableBitmap)]

**SoftwareBitmapSource** を [**ImageBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.ImageBrush) の [**ImageSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imagebrush.imagesource) として使用して **SoftwareBitmap** を設定することもできます。

## <a name="create-a-softwarebitmap-from-a-writeablebitmap"></a>WriteableBitmap から SoftwareBitmap を作成する

[  **SoftwareBitmap.CreateCopyFromBuffer**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.softwarebitmap.createcopyfrombuffer) を呼び出して、**WriteableBitmap** の **PixelBuffer** プロパティを指定することで、既存の **WriteableBitmap** から **SoftwareBitmap** を作成し、ピクセル データを設定することができます。 新しく作成する **WriteableBitmap** のピクセル形式は第 2 引数で指定できます。 新しい画像のサイズは、**WriteableBitmap** の [**PixelWidth**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapsource.pixelwidth) プロパティと [**PixelHeight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapsource.pixelheight) プロパティを使って指定してください。

[!code-cs[WriteableBitmapToSoftwareBitmap](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetWriteableBitmapToSoftwareBitmap)]

## <a name="create-or-edit-a-softwarebitmap-programmatically"></a>SoftwareBitmap をプログラムから作成または編集する

ここまでは、画像ファイルを使った方法を紹介してきました。 新しい **SoftwareBitmap** をプログラム コードから作成し、同じ手法を用いて **SoftwareBitmap** のピクセル データにアクセスし、変更を加えることもできます。

**SoftwareBitmap** では、ピクセル データを含んだ RAW バッファーが、COM 相互運用機能を使って公開されます。

COM 相互運用機能を使うには、**System.Runtime.InteropServices** 名前空間の参照をプロジェクトに追加する必要があります。

[!code-cs[InteropNamespace](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetInteropNamespace)]

COM インターフェイス [**IMemoryBufferByteAccess**](https://docs.microsoft.com/previous-versions//mt297505(v=vs.85)) を初期化するには、対象の名前空間に以下のコードを追加します。

[!code-cs[COMImport](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetCOMImport)]

必要なピクセル形式とサイズを指定して新しい **SoftwareBitmap** を作成します。 既にある **SoftwareBitmap** のピクセル データを編集する必要がある場合は、その SoftwareBitmap を使ってもかまいません。 [  **SoftwareBitmap.LockBuffer**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.softwarebitmap.lockbuffer) を呼び出して、ピクセル データ バッファーを表す [**BitmapBuffer**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapBuffer) クラスのインスタンスを取得します。 **BitmapBuffer** を COM インターフェイス **IMemoryBufferByteAccess** にキャストしたうえで [**IMemoryBufferByteAccess.GetBuffer**](https://docs.microsoft.com/windows/desktop/WinRT/imemorybufferbyteaccess-getbuffer) を呼び出し、バイト配列にデータを設定します。 ピクセルごとにバッファーのオフセットを計算しやすいよう、[**BitmapBuffer.GetPlaneDescription**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapbuffer.getplanedescription) メソッドを使って [**BitmapPlaneDescription**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapPlaneDescription) オブジェクトを取得します。

[!code-cs[CreateNewSoftwareBitmap](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetCreateNewSoftwareBitmap)]

このメソッドは、Windows ランタイム型よりも低いレベルの RAW バッファーにアクセスするため、**unsafe** キーワードを使って宣言する必要があります。 また、Microsoft Visual Studio でアンセーフ コードのコンパイルを許可するようにプロジェクトを構成する必要があります。プロジェクトの **[プロパティ]** ページを開き、 **[ビルド]** プロパティ ページをクリックして、 **[アンセーフ コードの許可]** チェック ボックスをオンにしてください。

## <a name="create-a-softwarebitmap-from-a-direct3d-surface"></a>Direct3D サーフェスから SoftwareBitmap を作成する

Direct3D サーフェスから **SoftwareBitmap** オブジェクトを作成するには、プロジェクトで [**Windows.Graphics.DirectX.Direct3D11**](https://docs.microsoft.com/uwp/api/Windows.Graphics.DirectX.Direct3D11) 名前空間を追加する必要があります。

[!code-cs[Direct3DNamespace](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetDirect3DNamespace)]

サーフェスから新しい **SoftwareBitmap** を作成するには、[**CreateCopyFromSurfaceAsync**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.softwarebitmap.createcopyfromsurfaceasync) を呼び出します。 この名前を見るとわかるように、新しい **SoftwareBitmap** には、画像データのコピーが別に存在します。 **SoftwareBitmap** に変更を加えても、Direct3D サーフェスには一切影響しません。

[!code-cs[CreateSoftwareBitmapFromSurface](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetCreateSoftwareBitmapFromSurface)]

## <a name="convert-a-softwarebitmap-to-a-different-pixel-format"></a>SoftwareBitmap を異なるピクセル形式に変換する

**SoftwareBitmap** クラスの静的メソッド [**Convert**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.softwarebitmap.windows) を使うと、既にある **SoftwareBitmap** から、指定したピクセル形式とアルファ モードを使った新しい **SoftwareBitmap** を簡単に作成することができます。 新しく作成されたビットマップには、画像データのコピーが別に存在します。 新しいビットマップに変更を加えても、元のビットマップには一切影響しません。

[!code-cs[Convert](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetConvert)]

## <a name="transcode-an-image-file"></a>画像ファイルのトランスコード

画像ファイルを [**BitmapDecoder**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapDecoder) から [**BitmapEncoder**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapEncoder) に直接トランスコードすることができます。 トランスコードの対象となるファイルから [**IRandomAccessStream**](https://docs.microsoft.com/uwp/api/Windows.Storage.Streams.IRandomAccessStream) を作成します。 入力ストリームから新しい **BitmapDecoder** を作成します。 エンコーダーの書き込み先となる新しい [**InMemoryRandomAccessStream**](https://docs.microsoft.com/uwp/api/Windows.Storage.Streams.InMemoryRandomAccessStream) を作成し、[**BitmapEncoder.CreateForTranscodingAsync**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapencoder.createfortranscodingasync) を呼び出します。このとき、引数にインメモリ ストリームとデコーダー オブジェクトを渡します。 トランスコードではエンコード オプションはサポートされません。オプションを指定する場合は、代わりに [**CreateAsync**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapencoder.createasync) を使う必要があります。 入力画像ファイルのプロパティのうち、エンコーダーに対して明示的に指定しなかったプロパティはすべて、元のまま出力ファイルに書き込まれます。 [  **FlushAsync**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapencoder.flushasync) を呼び出すと、インメモリ ストリームへのエンコードをエンコーダーが開始します。 最後に、ファイル ストリームとインメモリ ストリームを先頭までシークし、[**CopyAsync**](https://docs.microsoft.com/uwp/api/windows.storage.streams.randomaccessstream.copyasync) を呼び出してインメモリ ストリームをファイル ストリームに書き込みます。

[!code-cs[TranscodeImageFile](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetTranscodeImageFile)]

## <a name="related-topics"></a>関連トピック

* [BitmapEncoder オプションのリファレンス](bitmapencoder-options-reference.md)
* [イメージのメタデータ](image-metadata.md)
 

 




