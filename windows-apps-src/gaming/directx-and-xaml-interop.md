---
title: DirectX と XAML の相互運用機能
description: ユニバーサル Windows プラットフォーム (UWP) ゲームで Extensible Application Markup Language (XAML) と Microsoft DirectX を組み合わせて使うことができます。
ms.assetid: 0fb2819a-61ed-129d-6564-0b67debf5c6b
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, ゲーム, DirectX, XAML の相互運用機能
ms.localizationpriority: medium
ms.openlocfilehash: a34510939b84c885bf90ac2b6b42ffa158decc8e
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/24/2020
ms.locfileid: "91216575"
---
# <a name="directx-and-xaml-interop"></a>DirectX と XAML の相互運用機能

ユニバーサル Windows プラットフォーム (UWP) ゲームまたはアプリで Extensible Application Markup Language (XAML) と Microsoft DirectX を組み合わせて使うことができます。 XAML と DirectX を組み合わせれば、DirectX でレンダリングしたコンテンツと相互運用できる柔軟なユーザー インターフェイス フレームワークを構築できます。これは、グラフィックスを多用するアプリで特に役立ちます。 ここでは、DirectX を使った UWP アプリの構造について説明し、DirectX と連携する UWP アプリを構築するときに使う重要な型を示します。

アプリが主に 2D レンダリングに重点を置いている場合、[Win2D](https://github.com/microsoft/win2d) Windows ランタイム ライブラリを使用できます。 このライブラリは Microsoft によって管理されており、コア Direct2D のテクノロジを基盤として構築されています。 2D グラフィックスを実装する使用パターンを大幅に簡略化し、このドキュメントで説明する手法の一部の便利な抽象化が含まれています。 詳しくは、プロジェクトのページをご覧ください。 このドキュメントでは、Win2D を使用*しない*ことを選択したアプリ開発者向けのガイダンスを示します。

> [!NOTE]
> DirectX Api は Windows ランタイム型として定義されていませんが、通常は [C++/WinRT](../cpp-and-winrt-apis/index.md) を使用して、directx と相互運用できる XAML UWP コンポーネントを開発できます。 また、DirectX の呼び出しを独立した Windows ランタイム メタデータ ファイルにラップすると、C# と DirectX を利用する XAML を使って UWP アプリを作成できます。

## <a name="xaml-and-directx"></a>XAML と DirectX

DirectX には、2D と 3D のグラフィックス用に、Direct2D と Microsoft Direct3D という 2 つの強力なライブラリがあります。 XAML でも基本的な 2D のプリミティブと効果はサポートされますが、モデリングやゲームなどの多くのアプリでは、より複雑なグラフィックス サポートが必要になります。 そのようなアプリでは、Direct2D と Direct3D を使ってグラフィックスの一部または全体をレンダリングし、それ以外の部分には XAML を使うことができます。

カスタム XAML と DirectX の相互運用機能を実装する場合は、次の 2 つの概念を理解する必要があります。

-   共有サーフェイス。[Windows::UI::Xaml::Media::ImageSource](/uwp/api/windows.ui.xaml.media.imagesource) 型を使って DirectX で間接的に描画を行うことができる、サイズが指定されたディスプレイの領域です。この領域は XAML で定義されます。 共有サーフェイスについては、新しいコンテンツが画面に表示される正確なタイミングを制御する必要はありません。 共有サーフェイスの更新は XAML フレームワークの更新に同期されます。
-   [スワップ チェーン](/windows/desktop/direct3d9/what-is-a-swap-chain-)。最小限の待ち時間でグラフィックスを表示するために使用されるバッファーのコレクションを表します。 通常、スワップ チェーンは、UI スレッドとは別に、1 秒あたり 60 フレームで更新されます。 ただし、スワップ チェーンは高速な更新をサポートするために、より多くのメモリと CPU リソースを使用します。また、複数のスレッドを管理する必要があるために使用することが難しくなります。

次に、DirectX を使う目的を確認します。 表示ウィンドウのサイズに収まる 1 つのコントロールを作ったりアニメーション化したりするために使うのか、 ゲームなどのようにリアルタイムでレンダリングして制御する必要がある出力をサーフェイスに表示するのかを確認します。 このような場合は、おそらくスワップ チェーンを実装する必要があります。 それ以外の場合は、共有サーフェイスを使用する方法で問題はありません。

DirectX をどのように使うかを決めたら、目的に応じて次のいずれかの Windows ランタイム型を使って、DirectX のレンダリングを UWP アプリに組み込みます。

-   静的な画像を構成する場合やイベント駆動型の複雑な画像を描画する場合は、[Windows::UI::Xaml::Media::Imaging::SurfaceImageSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource) を使って共有サーフェイスに描画します。 これは、サイズが指定された DirectX の描画サーフェイスを処理する型です。 通常は、ドキュメントや UI 要素でビットマップとして表示する画像やテクスチャを構成する場合に使います。 この型は、高パフォーマンスのゲームのようなリアルタイムのインタラクティビティには適しません。 **SurfaceImageSource** オブジェクトの更新が XAML ユーザー インターフェイスの更新に同期されるため、フレーム レートが安定しない、リアルタイムの入力に対する応答が遅いなど、ユーザーへの視覚的フィードバックに待ち時間が生じるためです。 ただし、動的なコントロールやデータ シミュレーションであれば、更新に時間はかからず問題ありません。

-   画像が画面上のスペースよりも大きく、ユーザーがパンまたはズームできる場合は、[Windows::UI::Xaml::Media::Imaging::VirtualSurfaceImageSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.VirtualSurfaceImageSource) を使います。 これは、画面よりも大きいサイズが指定された DirectX の描画サーフェイスを処理する型です。 [SurfaceImageSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource) と同様に、複雑な画像やコントロールを動的に構成する場合に使います。 また、**SurfaceImageSource** と同様に、高パフォーマンスのゲームには適しません。 **VirtualSurfaceImageSource** を使うことができる XAML 要素には、マップ コントロールや、画像が多い大きなドキュメント ビューアーなどがあります。

-   リアルタイムで更新されるグラフィックスを DirectX を使って表示する場合や、短い待ち時間で定期的に更新を行う必要がある場合は、[SwapChainPanel](/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel) クラスを使います。これにより、XAML フレームワークの更新タイマーに同期せずにグラフィックスを更新することができます。 この型を使うと、グラフィックス デバイスのスワップ チェーン ([IDXGISwapChain1](/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgiswapchain1)) に直接アクセスし、XAML をレンダー ターゲットの上に配置できます。 この型は、ゲームなどの全画面の DirectX アプリで XAML ベースのユーザー インターフェイスが必要な場合に便利です。 Microsoft DirectX Graphic Infrastructure (DXGI)、Direct2D、Direct3D も含めて、この方法を使うには、DirectX に関する知識が必要です。 詳細については、「 [Direct3D 11 のプログラミングガイド](/windows/desktop/direct3d11/dx-graphics-overviews)」を参照してください。

## <a name="surfaceimagesource"></a>SurfaceImageSource

[SurfaceImageSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource) は、DirectX で描画を行うための共有サーフェイスを提供し、ビットからアプリのコンテンツを構成します。

[SurfaceImageSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource) オブジェクトをコード ビハインドで作って更新する基本的なプロセスを次に示します。

1.  [SurfaceImageSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource) コンストラクターに高さと幅の値を渡して、共有サーフェイスのサイズを定義します。 アルファ (不透明度) のサポートが必要かどうかも指定できます。

    次に例を示します。

    `SurfaceImageSource^ surfaceImageSource = ref new SurfaceImageSource(400, 300);`

2.  [ISurfaceImageSourceNativeWithD2D](/windows/desktop/api/windows.ui.xaml.media.dxinterop/nn-windows-ui-xaml-media-dxinterop-isurfaceimagesourcenativewithd2d) へのポインターを取得します。 [SurfaceImageSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource) オブジェクトを [IInspectable](/windows/desktop/api/inspectable/nn-inspectable-iinspectable) (または **IUnknown**) としてキャストし、それに対する **QueryInterface** を呼び出して、基になる **ISurfaceImageSourceNativeWithD2D** 実装を取得します。 この実装で定義されているメソッドを使って、デバイスを設定し、描画操作を実行します。

    ```cpp
    Microsoft::WRL::ComPtr<ISurfaceImageSourceNativeWithD2D> m_sisNativeWithD2D;

    // ...

    IInspectable* sisInspectable = 
        (IInspectable*) reinterpret_cast<IInspectable*>(surfaceImageSource);

    sisInspectable->QueryInterface(
        __uuidof(ISurfaceImageSourceNativeWithD2D), 
        (void **)&m_sisNativeWithD2D);
    ```

3.  最初に [D3D11CreateDevice](/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice) と [D2D1CreateDevice](/windows/desktop/api/d2d1_1/nf-d2d1_1-d2d1createdevice) を呼び出してから、デバイスとコンテキストを [ISurfaceImageSourceNativeWithD2D::SetDevice](/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-isurfaceimagesourcenativewithd2d-setdevice) に渡して、DXGI デバイスと D2D デバイスを作成します。 

    > [!NOTE]
    > バックグラウンド スレッドから **SurfaceImageSource** に描画する場合は、DXGI デバイスでマルチスレッド アクセスも有効になっている必要があります。 この有効化は、パフォーマンス上の理由で、バック グラウンド スレッドから描画する場合にのみ行ってください。

    次に例を示します。

    ```cpp
    Microsoft::WRL::ComPtr<ID3D11Device> m_d3dDevice;
    Microsoft::WRL::ComPtr<ID3D11DeviceContext> m_d3dContext;
    Microsoft::WRL::ComPtr<ID2D1Device> m_d2dDevice;

    // Create the DXGI device
    D3D11CreateDevice(
            NULL,
            D3D_DRIVER_TYPE_HARDWARE,
            NULL,
            flags,
            featureLevels,
            ARRAYSIZE(featureLevels),
            D3D11_SDK_VERSION,
            &m_d3dDevice,
            NULL,
            &m_d3dContext);

    Microsoft::WRL::ComPtr<IDXGIDevice> dxgiDevice;
    m_d3dDevice.As(&dxgiDevice);

    // To enable multi-threaded access (optional)
    Microsoft::WRL::ComPtr<ID3D10Multithread> d3dMultiThread;

    m_d3dDevice->QueryInterface(
        __uuidof(ID3D10Multithread), 
        (void **) &d3dMultiThread);

    d3dMultiThread->SetMultithreadProtected(TRUE);

    // Create the D2D device
    D2D1CreateDevice(m_dxgiDevice.Get(), NULL, &m_d2dDevice);

    // Set the D2D device
    m_sisNativeWithD2D->SetDevice(m_d2dDevice.Get());
    ```

4.  [ID2D1DeviceContext](/windows/desktop/api/dxgi/nn-dxgi-idxgisurface) オブジェクトへのポインターを [ISurfaceImageSourceNativeWithD2D::BeginDraw](/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-isurfaceimagesourcenativewithd2d-begindraw) に渡し、返された描画コンテキストを使って、**SurfaceImageSource** 内に目的の四角形のコンテンツを描画します。 **ISurfaceImageSourceNativeWithD2D::BeginDraw** と描画のコマンドは、バック グラウンド スレッドから呼び出すことができます。 *updateRect* パラメーターで更新対象として指定した領域だけが描画されます。

    このメソッドは、更新されるターゲットの四角形の位置 (x、y) を示すオフセットを *offset* パラメーターで返します。 このオフセットを使って、更新されたコンテンツを **ID2D1DeviceContext** で描画する位置を特定できます。

    ```cpp
    Microsoft::WRL::ComPtr<ID2D1DeviceContext> drawingContext;

    HRESULT beginDrawHR = m_sisNative->BeginDraw(
        updateRect, 
        &drawingContext, 
        &offset);

    if (beginDrawHR == DXGI_ERROR_DEVICE_REMOVED 
        || beginDrawHR == DXGI_ERROR_DEVICE_RESET
        || beginDrawHR == D2DERR_RECREATE_TARGET)
    {
        // The D3D and D2D device was lost and need to be re-created.
        // Recovery steps are:
        // 1) Re-create the D3D and D2D devices
        // 2) Call ISurfaceImageSourceNativeWithD2D::SetDevice with the new D2D
        //    device
        // 3) Redraw the contents of the SurfaceImageSource
    }
    else if (beginDrawHR == E_SURFACE_CONTENTS_LOST)
    {
        // The devices were not lost but the entire contents of the surface
        // were. Recovery steps are:
        // 1) Call ISurfaceImageSourceNativeWithD2D::SetDevice with the D2D 
        //    device again
        // 2) Redraw the entire contents of the SurfaceImageSource
    }
    else 
    {
        // Draw your updated rectangle with the drawingContext
    }
    ```

5. [ISurfaceImageSourceNativeWithD2D::EndDraw](/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-isurfaceimagesourcenativewithd2d-enddraw) を呼び出してビットマップを終了します。 ビットマップは、XAML の [Image](/uwp/api/windows.ui.xaml.controls.image) または [ImageBrush](/uwp/api/Windows.UI.Xaml.Media.ImageBrush) のソースとして使用できます。 **ISurfaceImageSourceNativeWithD2D::EndDraw** は、UI スレッドからのみ呼び出す必要があります。

    ```cpp
    m_sisNative->EndDraw();

    // ...
    // The SurfaceImageSource object's underlying 
    // ISurfaceImageSourceNativeWithD2D object contains the completed bitmap.

    ImageBrush^ brush = ref new ImageBrush();
    brush->ImageSource = surfaceImageSource;
    ```

    > [!NOTE]
    > 現在、[SurfaceImageSource::SetSource](/uwp/api/windows.ui.xaml.media.imaging.bitmapsource.setsource) (**IBitmapSource::SetSource** から継承) を呼び出すと例外がスローされます。 [SurfaceImageSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource) オブジェクトから呼び出さないでください。

    > [!NOTE]
    > アプリケーションでは、関連する [Window](/uwp/api/Windows.UI.Xaml.Window) が非表示になっている間は **SurfaceImageSource** へ描画しないようにする必要があります。描画すると、**ISurfaceImageSourceNativeWithD2D** API が失敗します。 これを行うためには、[Window.VisibilityChanged](/uwp/api/Windows.UI.Xaml.Window.VisibilityChanged) イベントのイベント リスナーとして登録し、表示の変更を追跡します。

## <a name="virtualsurfaceimagesource"></a>VirtualSurfaceImageSource

[VirtualSurfaceImageSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.VirtualSurfaceImageSource) は、コンテンツが画面に収まらず、仮想化しないと最適なレンダリングにならない場合に、[SurfaceImageSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource) を拡張します。

[VirtualSurfaceImageSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.VirtualSurfaceImageSource) が [SurfaceImageSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource) と異なる点は、[IVirtualSurfaceImageSourceCallbacksNative::UpdatesNeeded](/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-ivirtualsurfaceupdatescallbacknative-updatesneeded) コールバックを使うことです。このコールバックを実装することで、サーフェイスの領域が画面に表示できるようになったときに領域が更新されます。 非表示の領域をクリアする必要はありません。この処理は XAML フレームワークで行われます。

[VirtualSurfaceImageSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.VirtualSurfaceImageSource) オブジェクトをコード ビハインドで作成および更新する基本的なプロセスを次に示します。

1.  サイズを指定して [VirtualSurfaceImageSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.VirtualSurfaceImageSource) のインスタンスを作成します。 次に例を示します。

    ```cpp
    VirtualSurfaceImageSource^ virtualSIS = 
        ref new VirtualSurfaceImageSource(2000, 2000);
    ```

2.  [IVirtualSurfaceImageSourceNative](/windows/desktop/api/windows.ui.xaml.media.dxinterop/nn-windows-ui-xaml-media-dxinterop-ivirtualsurfaceimagesourcenative) と [ISurfaceImageSourceNativeWithD2D](/windows/desktop/api/windows.ui.xaml.media.dxinterop/nn-windows-ui-xaml-media-dxinterop-isurfaceimagesourcenativewithd2d) へのポインターを取得します。 [VirtualSurfaceImageSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.VirtualSurfaceImageSource) オブジェクトを [IInspectable](/windows/desktop/api/inspectable/nn-inspectable-iinspectable) または [IUnknown](/windows/desktop/api/unknwn/nn-unknwn-iunknown) としてキャストし、それに対する [QueryInterface](/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_)) を呼び出して、基になる **IVirtualSurfaceImageSourceNative** 実装と **ISurfaceImageSourceNativeWithD2D** 実装を取得します。 これらの実装で定義されているメソッドを使って、デバイスを設定し、描画操作を実行します。

    ```cpp
    Microsoft::WRL::ComPtr<IVirtualSurfaceImageSourceNative>  m_vsisNative;
    Microsoft::WRL::ComPtr<ISurfaceImageSourceNativeWithD2D> m_sisNativeWithD2D;

    // ...

    IInspectable* vsisInspectable = 
        (IInspectable*) reinterpret_cast<IInspectable*>(virtualSIS);

    vsisInspectable->QueryInterface(
        __uuidof(IVirtualSurfaceImageSourceNative), 
        (void **) &m_vsisNative);

    vsisInspectable->QueryInterface(
        __uuidof(ISurfaceImageSourceNativeWithD2D), 
        (void **) &m_sisNativeWithD2D);
    ```

3.  最初に **D3D11CreateDevice** と **D2D1CreateDevice** を呼び出してから、D2D デバイスを **ISurfaceImageSourceNativeWithD2D::SetDevice** に渡して、DXGI デバイスと D2D デバイスを作成します。

    > [!NOTE]
    > バックグラウンド スレッドから **VirtualSurfaceImageSource** に描画する場合は、DXGI デバイスでマルチスレッド アクセスも有効になっている必要があります。 この有効化は、パフォーマンス上の理由で、バック グラウンド スレッドから描画する場合にのみ行ってください。

    次に例を示します。

    ```cpp
    Microsoft::WRL::ComPtr<ID3D11Device> m_d3dDevice;
    Microsoft::WRL::ComPtr<ID3D11DeviceContext> m_d3dContext;
    Microsoft::WRL::ComPtr<ID2D1Device> m_d2dDevice;

    // Create the DXGI device
    D3D11CreateDevice(
            NULL,
            D3D_DRIVER_TYPE_HARDWARE,
            NULL,
            flags,
            featureLevels,
            ARRAYSIZE(featureLevels),
            D3D11_SDK_VERSION,
            &m_d3dDevice,
            NULL,
            &m_d3dContext);  

    Microsoft::WRL::ComPtr<IDXGIDevice> dxgiDevice;
    m_d3dDevice.As(&dxgiDevice);
    
    // To enable multi-threaded access (optional)
    Microsoft::WRL::ComPtr<ID3D10Multithread> d3dMultiThread;

    m_d3dDevice->QueryInterface(
        __uuidof(ID3D10Multithread), 
        (void **) &d3dMultiThread);

    d3dMultiThread->SetMultithreadProtected(TRUE);

    // Create the D2D device
    D2D1CreateDevice(m_dxgiDevice.Get(), NULL, &m_d2dDevice);

    // Set the D2D device
    m_vsisNativeWithD2D->SetDevice(m_d2dDevice.Get());

    m_vsisNative->SetDevice(dxgiDevice.Get());
    ```

4.  [IVirtualSurfaceImageSourceNative:: Registerfor](/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-ivirtualsurfaceimagesourcenative-registerforupdatesneeded)を呼び出し、 [IVirtualSurfaceUpdatesCallbackNative](/windows/desktop/api/windows.ui.xaml.media.dxinterop/nn-windows-ui-xaml-media-dxinterop-ivirtualsurfaceupdatescallbacknative)の実装への参照を渡します。

    ```cpp
    class MyContentImageSource : public IVirtualSurfaceUpdatesCallbackNative
    {
    // ...
      private:
         virtual HRESULT STDMETHODCALLTYPE UpdatesNeeded() override;
    }

    // ...

    HRESULT STDMETHODCALLTYPE MyContentImageSource::UpdatesNeeded()
    {
      // .. Perform drawing here ...
    }

    void MyContentImageSource::Initialize()
    {
      // ...
      m_vsisNative->RegisterForUpdatesNeeded(this);
      // ...
    }
    ```

    [VirtualSurfaceImageSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.VirtualSurfaceImageSource) の領域を更新する必要がある場合、フレームワークで [IVirtualSurfaceUpdatesCallbackNative::UpdatesNeeded](/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-ivirtualsurfaceimagesourcenative-registerforupdatesneeded) の実装が呼び出されます。

    これが発生するのは、領域を描画する必要があるとフレームワークで特定されたとき (ユーザーがサーフェイスのビューをパンまたはズームしたときなど) と、その領域に対する [IVirtualSurfaceImageSourceNative::Invalidate](/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-ivirtualsurfaceimagesourcenative-invalidate) がアプリで呼び出されたときです。

5.  [IVirtualSurfaceImageSourceNative::UpdatesNeeded](/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-ivirtualsurfaceupdatescallbacknative-updatesneeded) で、[IVirtualSurfaceImageSourceNative::GetUpdateRectCount](/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-ivirtualsurfaceimagesourcenative-getupdaterectcount) メソッドと [IVirtualSurfaceImageSourceNative::GetUpdateRects](/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-ivirtualsurfaceimagesourcenative-getupdaterects) メソッドを使って、描画する必要があるサーフェイスの領域を特定します。

    ```cpp
    HRESULT STDMETHODCALLTYPE MyContentImageSource::UpdatesNeeded()
    {
        HRESULT hr = S_OK;

        try
        {
            ULONG drawingBoundsCount = 0;  
            m_vsisNative->GetUpdateRectCount(&drawingBoundsCount);

            std::unique_ptr<RECT[]> drawingBounds(
                new RECT[drawingBoundsCount]);

            m_vsisNative->GetUpdateRects(
                drawingBounds.get(), 
                drawingBoundsCount);
            
            for (ULONG i = 0; i < drawingBoundsCount; ++i)
            {
                // Drawing code here ...
            }
        }
        catch (Platform::Exception^ exception)
        {
            hr = exception->HResult;
        }

        return hr;
    }
    ```

6.  最後に、更新する必要がある領域ごとに以下を行います。

    1.  **ID2D1DeviceContext** オブジェクトへのポインターを **ISurfaceImageSourceNativeWithD2D::BeginDraw** に渡し、返された描画コンテキストを使って、**SurfaceImageSource** 内に目的の四角形のコンテンツを描画します。 **ISurfaceImageSourceNativeWithD2D::BeginDraw** と描画のコマンドは、バック グラウンド スレッドから呼び出すことができます。 *updateRect* パラメーターで更新対象として指定した領域だけが描画されます。

        このメソッドは、更新されるターゲットの四角形の位置 (x、y) を示すオフセットを *offset* パラメーターで返します。 このオフセットを使って、更新されたコンテンツを **ID2D1DeviceContext** で描画する位置を特定できます。

        ```cpp
        Microsoft::WRL::ComPtr<ID2D1DeviceContext> drawingContext;

        HRESULT beginDrawHR = m_sisNative->BeginDraw(
            updateRect, 
            &drawingContext, 
            &offset);

        if (beginDrawHR == DXGI_ERROR_DEVICE_REMOVED 
            || beginDrawHR == DXGI_ERROR_DEVICE_RESET 
            || beginDrawHR == D2DERR_RECREATE_TARGET)
        {
            // The D3D and D2D devices were lost and need to be re-created.
            // Recovery steps are:
            // 1) Re-create the D3D and D2D devices
            // 2) Call ISurfaceImageSourceNativeWithD2D::SetDevice with the 
            //    new D2D device
            // 3) Redraw the contents of the VirtualSurfaceImageSource
        }
        else if (beginDrawHR == E_SURFACE_CONTENTS_LOST)
        {
            // The devices were not lost but the entire contents of the 
            // surface were lost. Recovery steps are:
            // 1) Call ISurfaceImageSourceNativeWithD2D::SetDevice with the 
            //    D2D device again
            // 2) Redraw the entire contents of the VirtualSurfaceImageSource
        }
        else
        {
            // Draw your updated rectangle with the drawingContext
        }
        ```

    2.  具体的なコンテンツをその領域に描画します。ただし、パフォーマンスを高めるために描画を限られた領域に制限します。

    3.  **ISurfaceImageSourceNativeWithD2D::EndDraw** を呼び出します。 結果のビットマップが返されます。

> [!NOTE]
> アプリケーションでは、関連する [Window](/uwp/api/Windows.UI.Xaml.Window) が非表示になっている間は **SurfaceImageSource** へ描画しないようにする必要があります。描画すると、**ISurfaceImageSourceNativeWithD2D** API が失敗します。 これを行うためには、[Window.VisibilityChanged](/uwp/api/Windows.UI.Xaml.Window.VisibilityChanged) イベントのイベント リスナーとして登録し、表示の変更を追跡します。

## <a name="swapchainpanel-and-gaming"></a>SwapChainPanel とゲーム


[SwapChainPanel](/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel) は、高パフォーマンスのグラフィックスやゲームをサポートするために設計された Windows ランタイム型です。この型でスワップ チェーンを直接管理します。 この例では、独自の DirectX スワップ チェーンを作成し、レンダリングされるコンテンツの表示を管理します。

パフォーマンスを高めるために、[SwapChainPanel](/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel) 型には次のような制限事項があります。

-   [SwapChainPanel](/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel) インスタンスの数は、アプリごとに 4 つ以下です。
-   DirectX スワップチェーンの高さと幅 ( [DXGI \_ スワップ \_ チェーンの \_ DESC1](/windows/desktop/api/dxgi1_2/ns-dxgi1_2-dxgi_swap_chain_desc1)) を、スワップチェーン要素の現在の次元に設定する必要があります。 そうしないと、表示コンテンツは ( **DXGI \_ スケーリング \_ 拡張**を使用して) 拡大縮小されます。
-   DirectX スワップチェーンのスケーリングモード ( [dxgi \_ スワップ \_ チェーン \_ DESC1](/windows/desktop/api/dxgi1_2/ns-dxgi1_2-dxgi_swap_chain_desc1)) を **dxgi \_ スケーリング \_ 拡張**に設定する必要があります。
-   DirectX スワップ チェーンを作成するときは、[IDXGIFactory2::CreateSwapChainForComposition](/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgifactory2-createswapchainforcomposition) を呼び出す必要があります。

[SwapChainPanel](/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel) の更新は、XAML フレームワークの更新ではなく、アプリのニーズに基づいて行います。 **SwapChainPanel** の更新を XAML フレームワークの更新に同期する必要がある場合は、[Windows::UI::Xaml::Media::CompositionTarget::Rendering](/uwp/api/windows.ui.xaml.media.compositiontarget.rendering) イベントに登録します。 それ以外の場合は、 **SwapChainPanel**を更新するものとは異なるスレッドから XAML 要素を更新しようとすると、スレッド間の問題を考慮する必要があります。

**SwapChainPanel** に対する待機時間の短いポインター入力を受信する必要がある場合は、[SwapChainPanel::CreateCoreIndependentInputSource](/uwp/api/windows.ui.xaml.controls.swapchainpanel.createcoreindependentinputsource) を使用します。 このメソッドは、バックグラウンド スレッドで最小限の待機時間で入力イベントを受信するために使用できる [CoreIndependentInputSource](/uwp/api/windows.ui.core.coreindependentinputsource) オブジェクトを返します。 このメソッドが呼び出されると、すべての入力がバックグラウンド スレッドにリダイレクトされるため、**SwapChainPanel** について通常の XAML ポインター入力イベントは発生しません。


> **注**   一般に、DirectX アプリでは、サイズが表示ウィンドウのサイズ (通常は、ほとんどの Microsoft Store ゲームのネイティブの画面解像度) と同じである横方向のスワップ チェーンを作る必要があります。 これにより、表示される XAML オーバーレイがない場合はアプリで最適なスワップ チェーンの実装が使われます。 縦モードに回転した場合、アプリは既にあるスワップ チェーンで [IDXGISwapChain1::SetRotation](/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-setrotation) を呼び出し、必要に応じてコンテンツに変換を適用して、同じスワップ チェーンで [SetSwapChain](/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-iswapchainpanelnative-setswapchain) をもう一度呼び出す必要があります。 同様に、アプリは、[IDXGISwapChain::ResizeBuffers](/windows/desktop/api/dxgi/nf-dxgi-idxgiswapchain-resizebuffers) 呼び出しによってスワップ チェーンのサイズが変更されるたびに、同じスワップ チェーンで **SetSwapChain** をもう一度呼び出す必要があります。


 

[SwapChainPanel](/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel) オブジェクトをコード ビハインドで作って更新する基本的なプロセスを次に示します。

1.  アプリのスワップ チェーン パネルのインスタンスを取得します。 これらのインスタンスは、XAML では `<SwapChainPanel>` タグを使って示されます。

    `Windows::UI::Xaml::Controls::SwapChainPanel^ swapChainPanel;`

    `<SwapChainPanel>` タグの例を次に示します。

    ```xml
    <SwapChainPanel x:Name="swapChainPanel">
        <SwapChainPanel.ColumnDefinitions>
            <ColumnDefinition Width="300*"/>
            <ColumnDefinition Width="1069*"/>
        </SwapChainPanel.ColumnDefinitions>
    …
    ```

2.  [ISwapChainPanelNative](/windows/desktop/api/windows.ui.xaml.media.dxinterop/nn-windows-ui-xaml-media-dxinterop-iswapchainpanelnative) へのポインターを取得します。 [SwapChainPanel](/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel) オブジェクトを [IInspectable](/windows/desktop/api/inspectable/nn-inspectable-iinspectable) (または **IUnknown**) としてキャストし、それに対する **QueryInterface** を呼び出して、基になる **ISwapChainPanelNative** 実装を取得します。

    ```cpp
    Microsoft::WRL::ComPtr<ISwapChainPanelNative> m_swapChainNative;
    // ...
    IInspectable* panelInspectable = (IInspectable*) reinterpret_cast<IInspectable*>(swapChainPanel);
    panelInspectable->QueryInterface(__uuidof(ISwapChainPanelNative), (void **)&m_swapChainNative);
    ```

3.  DXGI デバイスとスワップチェーンを作成し、 [Setswapchain](/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-iswapchainpanelnative-setswapchain)に渡すことによってスワップチェーンを[ISwapChainPanelNative](/windows/desktop/api/windows.ui.xaml.media.dxinterop/nn-windows-ui-xaml-media-dxinterop-iswapchainpanelnative)に設定します。

    ```cpp
    Microsoft::WRL::ComPtr<IDXGISwapChain1>               m_swapChain;    
    // ...
    DXGI_SWAP_CHAIN_DESC1 swapChainDesc = {0};
            swapChainDesc.Width = m_bounds.Width;
            swapChainDesc.Height = m_bounds.Height;
            swapChainDesc.Format = DXGI_FORMAT_B8G8R8A8_UNORM;           // This is the most common swapchain format.
            swapChainDesc.Stereo = false; 
            swapChainDesc.SampleDesc.Count = 1;                          // Don't use multi-sampling.
            swapChainDesc.SampleDesc.Quality = 0;
            swapChainDesc.BufferUsage = DXGI_USAGE_RENDER_TARGET_OUTPUT;
            swapChainDesc.BufferCount = 2;
            swapChainDesc.Scaling = DXGI_SCALING_STRETCH;
            swapChainDesc.SwapEffect = DXGI_SWAP_EFFECT_FLIP_SEQUENTIAL; // We recommend using this swap effect for all. applications
            swapChainDesc.Flags = 0;
                    
    // QI for DXGI device
    Microsoft::WRL::ComPtr<IDXGIDevice> dxgiDevice;
    m_d3dDevice.As(&dxgiDevice);

    // Get the DXGI adapter.
    Microsoft::WRL::ComPtr<IDXGIAdapter> dxgiAdapter;
    dxgiDevice->GetAdapter(&dxgiAdapter);

    // Get the DXGI factory.
    Microsoft::WRL::ComPtr<IDXGIFactory2> dxgiFactory;
    dxgiAdapter->GetParent(__uuidof(IDXGIFactory2), &dxgiFactory);
    // Create a swap chain by calling CreateSwapChainForComposition.
    dxgiFactory->CreateSwapChainForComposition(
                m_d3dDevice.Get(),
                &swapChainDesc,
                nullptr,        // Allow on any display. 
                &m_swapChain
                );
            
    m_swapChainNative->SetSwapChain(m_swapChain.Get());
    ```

4.  DirectX スワップ チェーンに描画し、それを渡してコンテンツを表示します。

    ```cpp
    HRESULT hr = m_swapChain->Present(1, 0);
    ```

    XAML 要素は、Windows ランタイムのレイアウトやレンダリング ロジックから更新が通知されると更新されます。

## <a name="related-topics"></a>関連トピック

* [Win2D](https://microsoft.github.io/Win2D/html/Introduction.htm)
* [SurfaceImageSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource)
* [VirtualSurfaceImageSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.VirtualSurfaceImageSource)
* [SwapChainPanel](/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel)
* [ISwapChainPanelNative](/windows/desktop/api/windows.ui.xaml.media.dxinterop/nn-windows-ui-xaml-media-dxinterop-iswapchainpanelnative)
* [Direct3D 11 用プログラミング ガイド](/windows/desktop/direct3d11/dx-graphics-overviews)