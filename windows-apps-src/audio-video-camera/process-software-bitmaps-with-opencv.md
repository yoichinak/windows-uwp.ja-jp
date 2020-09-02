---
ms.assetid: ''
description: この記事では、Open Source Computer Vision Library (OpenCV) で、SoftwareBitmap クラスを使用する方法について説明します。
title: OpenCV でのビットマップの処理
ms.date: 03/19/2018
ms.topic: article
keywords: Windows 10, UWP, OpenCV, SoftwareBitmap
ms.localizationpriority: medium
ms.openlocfilehash: a917c4efc8da8fbdabbdc753aacf23724ae17055
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363805"
---
# <a name="process-bitmaps-with-opencv"></a>OpenCV でのビットマップの処理

この記事では、各種のイメージ処理アルゴリズムを提供するオープンソースのネイティブコードライブラリである Open Source Computer Vision Library (OpenCV) を使用して、さまざまな Windows ランタイム Api でイメージを表すために使用される、 **[ソフトウェアビットマップ](/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap)** クラスの使用方法について説明します。 

この記事の例では、C# を使用して作成されたアプリを含む、UWP アプリから使用できるネイティブコード Windows ランタイムコンポーネントを作成する手順について説明します。 このヘルパー コンポーネントは、OpenCV の blur 画像処理関数を使用する 1 つのメソッド **Blur** を公開します。 このコンポーネントは、OpenCV ライブラリから直接使用できる、基になる画像データ バッファーへのポインターを取得するプライベート メソッドを実装します。これにより、ヘルパー コンポーネントを拡張して他の OpenCV 処理機能を実装することが容易になります。 

* **SoftwareBitmap** の使用方法の概要については、「[ビットマップ画像の作成、編集、保存](imaging.md)」をご覧ください。 
* OpenCV ライブラリの使用方法については、「」を参照 [https://opencv.org](https://opencv.org) してください。
* この記事で説明する OpenCV ヘルパー コンポーネントを **[MediaFrameReader](/uwp/api/windows.media.capture.frames.mediaframereader)** と共に使用して、カメラからのフレームのリアルタイムの画像処理を実現する方法については、「[OpenCV と MediaFrameReader の使用](use-opencv-with-mediaframereader.md)」をご覧ください。
* 複数の効果を実装する完全なコード例については、Windows ユニバーサル サンプル GitHub リポジトリにある[カメラ フレームと OpenCV のサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraOpenCV)をご覧ください。

> [!NOTE] 
> この記事で解説する OpenCVHelper コンポーネントで使用されている手法では、処理される画像データが GPU メモリではなく CPU メモリに格納されている必要があります。 したがって、画像のメモリ位置を要求できる API (**[MediaCapture](/uwp/api/windows.media.capture.mediacapture)** クラスなど) では、CPU メモリを指定する必要があります。

## <a name="create-a-helper-windows-runtime-component-for-opencv-interop"></a>OpenCV 相互運用機能のヘルパー Windows ランタイムコンポーネントを作成する

### <a name="1-add-a-new-native-code-windows-runtime-component-project-to-your-solution"></a>1. ソリューションに新しいネイティブコード Windows ランタイムコンポーネントプロジェクトを追加する

1. ソリューション エクスプローラーで、ソリューションを右クリックして **[追加]、[新しいプロジェクト]** の順に選択し、新しいプロジェクトを Visual Studio のソリューションに追加します。 
2. **[Visual C++]** カテゴリで、**[Windows ランタイム コンポーネント (ユニバーサル Windows)]** を選択します。 この例では、「OpenCVBridge」というプロジェクト名を入力し、**[OK]** をクリックします。 
3. **[新しい Windows ユニバーサル プロジェクト]** ダイアログ ボックスで、アプリのターゲットと最小 OS バージョンを選択して **[OK]** をクリックします。
4. ソリューション エクスプローラーで自動生成された Class1.cpp ファイルを右クリックして **[削除]** を選択し、確認のダイアログ ボックスが表示されたら **[削除]** を選択します。 次に Class1.h ヘッダー ファイルを削除します。
5. OpenCVBridge プロジェクト アイコンを右クリックし、**[追加]、[クラス]** の順に選択します。**[クラスの追加]** ダイアログ ボックスで、**[クラス名]** フィールドに「OpenCVHelper」と入力し、**[OK]** をクリックします。 コードは、後の手順で作成されるクラス ファイルに追加されます。

### <a name="2-add-the-opencv-nuget-packages-to-your-component-project"></a>2. OpenCV NuGet パッケージをコンポーネント プロジェクトに追加する

1. ソリューション エクスプローラーで、OpenCVBridge プロジェクト アイコンを右クリックし、**[NuGet パッケージの管理]** をクリックします。
2. [NuGet パッケージ マネージャー] ダイアログが開いたら、**[参照]** タブを選択し、検索ボックスに「OpenCV.Win」と入力します。
3. [OpenCV.Win.Core] を選択し、**[インストール]** をクリックします。 **[プレビュー]** ダイアログ ボックスで、**[OK]** をクリックします。
4. 同じ手順で "OpenCV.Win.ImgProc" パッケージをインストールします。

>[!NOTE]
>ImgProc は定期的に更新されず、ストアの準拠チェックにも合格しないため、これらのパッケージは実験のみを目的としています。

### <a name="3-implement-the-opencvhelper-class"></a>3. OpenCVHelper クラスを実装する

OpenCVHelper.h ヘッダー ファイルに以下のコードを貼り付けます。 このコードには、インストールした *Core* パッケージと *ImgProc* パッケージの OpenCV ヘッダー ファイルが含まれており、以下の手順で示される 3 つのメソッドを宣言しています。

:::code language="cpp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/OpenCVBridge/OpenCVHelper.h" id="SnippetOpenCVHelperHeader":::

OpenCVHelper.cpp ファイルの既存の内容を削除し、次の include ディレクティブを追加します。 

:::code language="cpp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/OpenCVBridge/OpenCVHelper.cpp" id="SnippetOpenCVHelperInclude":::

include ディレクティブの後に、以下の **using** ディレクティブを追加します。 

:::code language="cpp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/OpenCVBridge/OpenCVHelper.cpp" id="SnippetOpenCVHelperUsing":::

次に、**GetPointerToPixelData** メソッドを OpenCVHelper.cpp に追加します。 このメソッドは、**[SoftwareBitmap](/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap)** を受け取り、一連の変換を経て、ピクセル データの COM インターフェイス表現を取得します。これにより、基になるデータ バッファーへのポインターを **char** 配列として取得できます。 

最初に、ピクセル データを格納する **[BitmapBuffer](/uwp/api/windows.graphics.imaging.bitmapbuffer)** が、**[LockBuffer](/uwp/api/windows.graphics.imaging.softwarebitmap.lockbuffer)** を呼び出すことによって取得されます。LockBuffer は読み取り/書き込みバッファーを要求し、OpenCV ライブラリはそのピクセル データを変更できるようにします。  **[CreateReference](/uwp/api/windows.graphics.imaging.bitmapbuffer.CreateReference)** が呼び出され、**[IMemoryBufferReference](/uwp/api/windows.foundation.imemorybufferreference)** オブジェクトが取得されます。 次に、**IMemoryBufferByteAccess** インターフェイスが、すべての Windows ランタイム クラスの基本インターフェイスである **IInspectable** としてキャストされ、**[QueryInterface](/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_))** が呼び出されて **[IMemoryBufferByteAccess](/previous-versions/mt297505(v=vs.85))** COM インターフェイスが取得されます。これにより、ピクセル データ バッファーを **char** 配列として取得できます。 最後に、**[IMemoryBufferByteAccess::GetBuffer](/windows/desktop/WinRT/imemorybufferbyteaccess-getbuffer)** を呼び出して **char** 配列を設定します。 このメソッドの変換手順のいずれかが失敗した場合、メソッドは **false** を返し、処理が続行できないことを示します。

:::code language="cpp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/OpenCVBridge/OpenCVHelper.cpp" id="SnippetOpenCVHelperGetPointerToPixelData":::

次に、以下に示すように **TryConvert** メソッドを追加します。 このメソッドは、**SoftwareBitmap** を受け取り、**Mat** オブジェクトへの変換を試行します。このオブジェクトは、OpenCV が画像データ バッファーを表すために使用するマトリックス オブジェクトです。 このメソッドは、上で定義した **GetPointerToPixelData** メソッドを呼び出して、ピクセル データ バッファーの **char** 配列表現を取得します。 これが成功した場合、**Mat** クラスのコンストラクターが呼び出され、ソース **SoftwareBitmap** オブジェクトから取得されたピクセルの幅と高さが渡されます。 

> [!NOTE] 
> この例では、作成された **Mat** オブジェクトのピクセル形式として CV_8UC4 定数を指定します。 つまり、このメソッドに渡される **SoftwareBitmap** の **[BitmapPixelFormat](/uwp/api/windows.graphics.imaging.softwarebitmap.BitmapPixelFormat)** プロパティの値は、プリマルチプライ済みアルファを含む **[BGRA8](/uwp/api/Windows.Graphics.Imaging.BitmapPixelFormat)** (この例で使用する CV_8UC4 と同等) である必要があります。

作成された **Mat** オブジェクトのシャロー コピーがこのメソッドによって返されるため、それ以降の処理は、バッファーのコピーではなく、**SoftwareBitmap** によって参照される同じデータのピクセル データ バッファーで実行されます。

:::code language="cpp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/OpenCVBridge/OpenCVHelper.cpp" id="SnippetOpenCVHelperTryConvert":::

最後に、この例のヘルパー クラス メソッドは、1 つの画像処理メソッド **Blur** を実装します。これは、上で定義した **TryConvert** メソッドを使用して、ぼかし操作のソース ビットマップとターゲット ビットマップを表す **Mat** オブジェクトを取得し、OpenCV ImgProc ライブラリの **blur** メソッドを呼び出します。 **blur** のその他のパラメーターで、X および Y 方向のぼかし効果のサイズを指定します。

:::code language="cpp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/OpenCVBridge/OpenCVHelper.cpp" id="SnippetOpenCVHelperBlur":::


## <a name="a-simple-softwarebitmap-opencv-example-using-the-helper-component"></a>ヘルパー コンポーネントを使用するシンプルな SoftwareBitmap OpenCV の例
OpenCVBridge コンポーネントが作成されたので、OpenCV の **blur** メソッドを使用して **SoftwareBitmap** を変更するシンプルな C# アプリを作成できます。 UWP アプリから Windows ランタイムコンポーネントにアクセスするには、まずコンポーネントへの参照を追加する必要があります。 ソリューション エクスプローラーで、UWP アプリ プロジェクトの下にある **[参照設定] **ノードを右クリックし、**[参照の追加]** を選択します。[参照マネージャー] ダイアログ ボックスで、**[プロジェクト]、[ソリューション]** の順に選択します。 OpenCVBridge プロジェクトの横のボックスをオンにし、**[OK]** をクリックします。

次のコード例では、ユーザーは画像ファイルを選択し、**[BitmapDecoder](/uwp/api/windows.graphics.imaging.bitmapencoder)** を使用して画像の  **SoftwareBitmap** 表現を作成できます。 **SoftwareBitmap** の操作について詳しくは、「[ビットマップ画像の作成、編集、保存](./imaging.md)」をご覧ください。

この記事で既に説明したように、**OpenCVHelper** クラスでは、提供されるすべての **SoftwareBitmap** 画像がプリマルチプライ済みアルファ付き BGRA8 ピクセル形式でエンコードされている必要があります。そのため、画像がこの形式ではない場合、このコード例では **[Convert](/uwp/api/windows.graphics.imaging.softwarebitmap.BitmapAlphaMode)** を呼び出して画像を予期された形式に変換します。

次に、ぼかし操作のターゲットとして使用される **SoftwareBitmap** が作成されます。 一致する形式のビットマップを作成するために、入力画像のプロパティがコンストラクターの引数として使用されます。

**OpenCVHelper** の新しいインスタンスが作成されると、**Blur** メソッドが呼び出され、ソースとターゲットのビットマップが渡されます。 最後に、**SoftwareBitmapSource** が作成され、出力画像を XAML **Image** コントロールに割り当てます。

このサンプルコードでは、既定のプロジェクトテンプレートに含まれる名前空間に加えて、次の名前空間の Api を使用します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/MainPage.OpenCV.xaml.cs" id="SnippetOpenCVMainPageUsing":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/MainPage.OpenCV.xaml.cs" id="SnippetOpenCVBlur":::

## <a name="related-topics"></a>関連トピック

* [BitmapEncoder オプションのリファレンス](bitmapencoder-options-reference.md)
* [イメージのメタデータ](image-metadata.md)
 

 
