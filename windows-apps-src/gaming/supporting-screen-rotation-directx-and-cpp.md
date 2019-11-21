---
title: 画面の向きのサポート (DirectX と C++)
description: Here, we'll discuss best practices for handling screen rotation in your UWP DirectX app, so that the Windows 10 device's graphics hardware are used efficiently and effectively.
ms.assetid: f23818a6-e372-735d-912b-89cabeddb6d4
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, ゲーム, 画面の向き, DirectX
ms.localizationpriority: medium
ms.openlocfilehash: 5f6f50abeae643cccca2a23a4b3c20dc698d200e
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258516"
---
# <a name="supporting-screen-orientation-directx-and-c"></a>画面の向きのサポート (DirectX と C++)



ユニバーサル Windows プラットフォーム (UWP) アプリでは、[**DisplayInformation::OrientationChanged**](https://docs.microsoft.com/uwp/api/windows.graphics.display.displayinformation.orientationchanged) イベントを処理するとき、複数の画面の向きをサポートできます。 Here, we'll discuss best practices for handling screen rotation in your UWP DirectX app, so that the Windows 10 device's graphics hardware are used efficiently and effectively.

始める前に、グラフィックス ハードウェアは、デバイスの向きにかかわらず、常に同じ方法でピクセル データを出力するということを覚えておいてください。 Windows 10 devices can determine their current display orientation (with some sort of sensor, or with a software toggle) and allow users to change the display settings. Because of this, Windows 10 itself handles the rotation of the images to ensure they are "upright" based on the orientation of the device. 既定では、アプリは何か (たとえば、ウィンドウ サイズ) の方向が変化したという通知を受け取ります。 When this happens, Windows 10 immediately rotates the image for final display. For three of the four specific screen orientations (discussed later), Windows 10 uses additional graphic resources and computation to display the final image.

UWP DirectX アプリでは、アプリが照会できる基本的な表示の向きのデータを [**DisplayInformation**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Display.DisplayInformation) オブジェクトが提供します。 既定の向きは*横*で、表示のピクセルの幅が高さよりも大きくなります。もう 1 つの向きは*縦*で、表示はどちらかの方向に 90° 回転され、幅は高さより小さくなります。

Windows 10 defines four specific display orientation modes:

-   Landscape—the default display orientation for Windows 10, and is considered the base or identity angle for rotation (0 degrees).
-   縦: 表示は時計回りに 90° (または反時計回りに 270°) 回転します。
-   横、反転: 表示は 180° 回転されます (上下が逆になります)。
-   縦、反転: 表示は時計回りに 270° (または反時計回りに 90°) 回転します。

When the display rotates from one orientation to another, Windows 10 internally performs a rotation operation to align the drawn image with the new orientation, and the user sees an upright image on the screen.

Also, Windows 10 displays automatic transition animations to create a smooth user experience when shifting from one orientation to another. 表示の向きが変わる際に、ユーザーにはその変化が、表示されている画面の画像の固定拡大と回転アニメーションとして表示されます。 Time is allocated by Windows 10 to the app for layout in the new orientation.

画面の向きの変更を処理する一般的なプロセスは、ほぼ次のようになります。

1.  ウィンドウの境界値と表示の向きのデータの組み合わせを使って、デバイスの本来の表示の向きに合わせたスワップ チェーンを維持します。
2.  Notify Windows 10 of the orientation of the swap chain using [**IDXGISwapChain1::SetRotation**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-setrotation).
3.  レンダリング コードを変更して、デバイスのユーザーによる向きに合わせた画像を生成します。

## <a name="resizing-the-swap-chain-and-pre-rotating-its-contents"></a>スワップ チェーンのサイズ変更とその内容の事前回転


UWP DirectX アプリで基本的な表示サイズ変更とその内容の事前回転を行うには、次の手順を実装します。

1.  [  **DisplayInformation::OrientationChanged**](https://docs.microsoft.com/uwp/api/windows.graphics.display.displayinformation.orientationchanged) イベントを処理します。
2.  ウィンドウの新しいサイズにスワップ チェーンをサイズ変更します。
3.  [  **IDXGISwapChain1::SetRotation**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-setrotation) を呼び出して、スワップ チェーンの向きを設定します。
4.  ウィンドウ サイズに依存するすべてのリソース (レンダー ターゲット、その他のピクセル データ バッファーなど) を再作成します。

それでは、これらの手順をもう少し詳しく見てみましょう。

最初の手順は、[**DisplayInformation::OrientationChanged**](https://docs.microsoft.com/uwp/api/windows.graphics.display.displayinformation.orientationchanged) イベントのハンドラーを登録することです。 このイベントは、アプリで画面の向きが変更されるたびに発生します (表示が回転されたときなど)。

[  **DisplayInformation::OrientationChanged**](https://docs.microsoft.com/uwp/api/windows.graphics.display.displayinformation.orientationchanged) イベントを処理するには、必須の [**SetWindow**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.iframeworkview.setwindow) メソッドで **DisplayInformation::OrientationChanged** のハンドラーを接続します。このメソッドは、ビュー プロバイダーが実装しなければならない [**IFrameworkView**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.IFrameworkView) インターフェイスのメソッドのうちの 1 つです。

このコード例では、[**DisplayInformation::OrientationChanged**](https://docs.microsoft.com/uwp/api/windows.graphics.display.displayinformation.orientationchanged) のイベント ハンドラーは、**OnOrientationChanged** というメソッドです。 **DisplayInformation::OrientationChanged** が発生すると、**SetCurrentOrientation** というメソッドを呼び出し、このメソッドが **CreateWindowSizeDependentResources** を呼び出します。

```cpp
void App::SetWindow(CoreWindow^ window)
{
  // ... Other UI event handlers assigned here ...
  
    currentDisplayInformation->OrientationChanged +=
        ref new TypedEventHandler<DisplayInformation^, Object^>(this, &App::OnOrientationChanged);

  // ...
}
}
```

```cpp
void App::OnOrientationChanged(DisplayInformation^ sender, Object^ args)
{
    m_deviceResources->SetCurrentOrientation(sender->CurrentOrientation);
    m_main->CreateWindowSizeDependentResources();
}

// This method is called in the event handler for the OrientationChanged event.
void DX::DeviceResources::SetCurrentOrientation(DisplayOrientations currentOrientation)
{
    if (m_currentOrientation != currentOrientation)
    {
        m_currentOrientation = currentOrientation;
        CreateWindowSizeDependentResources();
    }
}
```

次に、スワップ チェーンを新しい画面の向きにサイズ変更し、レンダリングが行われるときにグラフィックス パイプラインの内容を回転させる準備をします。 この例では、**DirectXBase::CreateWindowSizeDependentResources** は、IDXGISwapChain::ResizeBuffers の呼び出し、3D および 2D 回転マトリックスの設定、SetRotation の呼び出し、リソースの再作成を処理するメソッドです。

```cpp
void DX::DeviceResources::CreateWindowSizeDependentResources() 
{
    // Clear the previous window size specific context.
    ID3D11RenderTargetView* nullViews[] = {nullptr};
    m_d3dContext->OMSetRenderTargets(ARRAYSIZE(nullViews), nullViews, nullptr);
    m_d3dRenderTargetView = nullptr;
    m_d2dContext->SetTarget(nullptr);
    m_d2dTargetBitmap = nullptr;
    m_d3dDepthStencilView = nullptr;
    m_d3dContext->Flush();

    // Calculate the necessary render target size in pixels.
    m_outputSize.Width = DX::ConvertDipsToPixels(m_logicalSize.Width, m_dpi);
    m_outputSize.Height = DX::ConvertDipsToPixels(m_logicalSize.Height, m_dpi);
    
    // Prevent zero size DirectX content from being created.
    m_outputSize.Width = max(m_outputSize.Width, 1);
    m_outputSize.Height = max(m_outputSize.Height, 1);

    // The width and height of the swap chain must be based on the window's
    // natively-oriented width and height. If the window is not in the native
    // orientation, the dimensions must be reversed.
    DXGI_MODE_ROTATION displayRotation = ComputeDisplayRotation();

    bool swapDimensions = displayRotation == DXGI_MODE_ROTATION_ROTATE90 || displayRotation == DXGI_MODE_ROTATION_ROTATE270;
    m_d3dRenderTargetSize.Width = swapDimensions ? m_outputSize.Height : m_outputSize.Width;
    m_d3dRenderTargetSize.Height = swapDimensions ? m_outputSize.Width : m_outputSize.Height;

    if (m_swapChain != nullptr)
    {
        // If the swap chain already exists, resize it.
        HRESULT hr = m_swapChain->ResizeBuffers(
            2, // Double-buffered swap chain.
            lround(m_d3dRenderTargetSize.Width),
            lround(m_d3dRenderTargetSize.Height),
            DXGI_FORMAT_B8G8R8A8_UNORM,
            0
            );

        if (hr == DXGI_ERROR_DEVICE_REMOVED || hr == DXGI_ERROR_DEVICE_RESET)
        {
            // If the device was removed for any reason, a new device and swap chain will need to be created.
            HandleDeviceLost();

            // Everything is set up now. Do not continue execution of this method. HandleDeviceLost will reenter this method 
            // and correctly set up the new device.
            return;
        }
        else
        {
            DX::ThrowIfFailed(hr);
        }
    }
    else
    {
        // Otherwise, create a new one using the same adapter as the existing Direct3D device.
        DXGI_SWAP_CHAIN_DESC1 swapChainDesc = {0};

        swapChainDesc.Width = lround(m_d3dRenderTargetSize.Width); // Match the size of the window.
        swapChainDesc.Height = lround(m_d3dRenderTargetSize.Height);
        swapChainDesc.Format = DXGI_FORMAT_B8G8R8A8_UNORM; // This is the most common swap chain format.
        swapChainDesc.Stereo = false;
        swapChainDesc.SampleDesc.Count = 1; // Don't use multi-sampling.
        swapChainDesc.SampleDesc.Quality = 0;
        swapChainDesc.BufferUsage = DXGI_USAGE_RENDER_TARGET_OUTPUT;
        swapChainDesc.BufferCount = 2; // Use double-buffering to minimize latency.
        swapChainDesc.SwapEffect = DXGI_SWAP_EFFECT_FLIP_SEQUENTIAL; // All UWP apps must use this SwapEffect.
        swapChainDesc.Flags = 0;    
        swapChainDesc.Scaling = DXGI_SCALING_NONE;
        swapChainDesc.AlphaMode = DXGI_ALPHA_MODE_IGNORE;

        // This sequence obtains the DXGI factory that was used to create the Direct3D device above.
        ComPtr<IDXGIDevice3> dxgiDevice;
        DX::ThrowIfFailed(
            m_d3dDevice.As(&dxgiDevice)
            );

        ComPtr<IDXGIAdapter> dxgiAdapter;
        DX::ThrowIfFailed(
            dxgiDevice->GetAdapter(&dxgiAdapter)
            );

        ComPtr<IDXGIFactory2> dxgiFactory;
        DX::ThrowIfFailed(
            dxgiAdapter->GetParent(IID_PPV_ARGS(&dxgiFactory))
            );

        DX::ThrowIfFailed(
            dxgiFactory->CreateSwapChainForCoreWindow(
                m_d3dDevice.Get(),
                reinterpret_cast<IUnknown*>(m_window.Get()),
                &swapChainDesc,
                nullptr,
                &m_swapChain
                )
            );

        // Ensure that DXGI does not queue more than one frame at a time. This both reduces latency and
        // ensures that the application will only render after each VSync, minimizing power consumption.
        DX::ThrowIfFailed(
            dxgiDevice->SetMaximumFrameLatency(1)
            );
    }

    // Set the proper orientation for the swap chain, and generate 2D and
    // 3D matrix transformations for rendering to the rotated swap chain.
    // Note the rotation angle for the 2D and 3D transforms are different.
    // This is due to the difference in coordinate spaces.  Additionally,
    // the 3D matrix is specified explicitly to avoid rounding errors.

    switch (displayRotation)
    {
    case DXGI_MODE_ROTATION_IDENTITY:
        m_orientationTransform2D = Matrix3x2F::Identity();
        m_orientationTransform3D = ScreenRotation::Rotation0;
        break;

    case DXGI_MODE_ROTATION_ROTATE90:
        m_orientationTransform2D = 
            Matrix3x2F::Rotation(90.0f) *
            Matrix3x2F::Translation(m_logicalSize.Height, 0.0f);
        m_orientationTransform3D = ScreenRotation::Rotation270;
        break;

    case DXGI_MODE_ROTATION_ROTATE180:
        m_orientationTransform2D = 
            Matrix3x2F::Rotation(180.0f) *
            Matrix3x2F::Translation(m_logicalSize.Width, m_logicalSize.Height);
        m_orientationTransform3D = ScreenRotation::Rotation180;
        break;

    case DXGI_MODE_ROTATION_ROTATE270:
        m_orientationTransform2D = 
            Matrix3x2F::Rotation(270.0f) *
            Matrix3x2F::Translation(0.0f, m_logicalSize.Width);
        m_orientationTransform3D = ScreenRotation::Rotation90;
        break;

    default:
        throw ref new FailureException();
    }


    //SDM: only instance of SetRotation
    DX::ThrowIfFailed(
        m_swapChain->SetRotation(displayRotation)
        );

    // Create a render target view of the swap chain back buffer.
    ComPtr<ID3D11Texture2D> backBuffer;
    DX::ThrowIfFailed(
        m_swapChain->GetBuffer(0, IID_PPV_ARGS(&backBuffer))
        );

    DX::ThrowIfFailed(
        m_d3dDevice->CreateRenderTargetView(
            backBuffer.Get(),
            nullptr,
            &m_d3dRenderTargetView
            )
        );

    // Create a depth stencil view for use with 3D rendering if needed.
    CD3D11_TEXTURE2D_DESC depthStencilDesc(
        DXGI_FORMAT_D24_UNORM_S8_UINT, 
        lround(m_d3dRenderTargetSize.Width),
        lround(m_d3dRenderTargetSize.Height),
        1, // This depth stencil view has only one texture.
        1, // Use a single mipmap level.
        D3D11_BIND_DEPTH_STENCIL
        );

    ComPtr<ID3D11Texture2D> depthStencil;
    DX::ThrowIfFailed(
        m_d3dDevice->CreateTexture2D(
            &depthStencilDesc,
            nullptr,
            &depthStencil
            )
        );

    CD3D11_DEPTH_STENCIL_VIEW_DESC depthStencilViewDesc(D3D11_DSV_DIMENSION_TEXTURE2D);
    DX::ThrowIfFailed(
        m_d3dDevice->CreateDepthStencilView(
            depthStencil.Get(),
            &depthStencilViewDesc,
            &m_d3dDepthStencilView
            )
        );
    
    // Set the 3D rendering viewport to target the entire window.
    m_screenViewport = CD3D11_VIEWPORT(
        0.0f,
        0.0f,
        m_d3dRenderTargetSize.Width,
        m_d3dRenderTargetSize.Height
        );

    m_d3dContext->RSSetViewports(1, &m_screenViewport);

    // Create a Direct2D target bitmap associated with the
    // swap chain back buffer and set it as the current target.
    D2D1_BITMAP_PROPERTIES1 bitmapProperties = 
        D2D1::BitmapProperties1(
            D2D1_BITMAP_OPTIONS_TARGET | D2D1_BITMAP_OPTIONS_CANNOT_DRAW,
            D2D1::PixelFormat(DXGI_FORMAT_B8G8R8A8_UNORM, D2D1_ALPHA_MODE_PREMULTIPLIED),
            m_dpi,
            m_dpi
            );

    ComPtr<IDXGISurface2> dxgiBackBuffer;
    DX::ThrowIfFailed(
        m_swapChain->GetBuffer(0, IID_PPV_ARGS(&dxgiBackBuffer))
        );

    DX::ThrowIfFailed(
        m_d2dContext->CreateBitmapFromDxgiSurface(
            dxgiBackBuffer.Get(),
            &bitmapProperties,
            &m_d2dTargetBitmap
            )
        );

    m_d2dContext->SetTarget(m_d2dTargetBitmap.Get());

    // Grayscale text anti-aliasing is recommended for all UWP apps.
    m_d2dContext->SetTextAntialiasMode(D2D1_TEXT_ANTIALIAS_MODE_GRAYSCALE);

}

```

このメソッドを次回に呼び出すときのためにウィンドウの現在の高さと幅の値を保存した後で、表示の境界の DIP (デバイスに依存しないピクセル) 値をピクセルに変換します。 例では、**ConvertDipsToPixels** を呼び出しています。これは、次のコードを実行する単純な関数です。

` floor((dips * dpi / 96.0f) + 0.5f);`

0\.5f を追加しているのは、最も近い整数に丸めるためです。

なお、[**CoreWindow**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow) の座標は、常に DIP で定義されます。 For Windows 10 and earlier versions of Windows, a DIP is defined as 1/96th of an inch, and aligned to the OS's definition of *up*. 表示の向きが縦モードに回転されると、アプリは **CoreWindow** の幅と高さを反転するので、レンダー ターゲットのサイズ (境界) もそれに応じて変更する必要があります。 Direct3D の座標は常に物理ピクセルであるため、スワップ チェーンをセットアップするために Direct3D に渡す **CoreWindow** の DIP 値は、事前に整数ピクセル値に変換する必要があります。

プロセスに関しては、単にスワップ チェーンをサイズ変更する場合よりも少し多くの作業を行っています。実際には、画像の Direct2D と Direct3D のコンポーネントを提示用に合成する前に回転させ、スワップ チェーンに結果を新しい向きでレンダリングしたことを伝えます。 ここでは、**DX::DeviceResources::CreateWindowSizeDependentResources** のコード例で示されているように、このプロセスについてさらに詳しく説明します。

-   表示の新しい向きを判断します。 表示が横から縦に (またはその逆に) 反転された場合は、表示境界の高さと幅の値を交換します (当然、DIP 値をピクセルに変換します)。

-   次に、スワップ チェーンが作成されたかどうかを確認します。 作成されていない場合は、[**IDXGIFactory2::CreateSwapChainForCoreWindow**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgifactory2-createswapchainforcorewindow) を呼び出して作成します。 または、[**IDXGISwapchain:ResizeBuffers**](https://docs.microsoft.com/windows/desktop/api/dxgi/nf-dxgi-idxgiswapchain-resizebuffers) を呼び出して、既にあるスワップ チェーンのバッファーのサイズを新しい表示サイズに変更します。 回転イベントのためにスワップ チェーンをサイズ変更する必要はありませんが (レンダリング パイプラインによって既に回転された内容を出力するだけであるため)、スナップ イベントやページ横幅に合わせるイベントのように、サイズ変更が必要なその他のイベントもあります。

-   その後、グラフィックス パイプラインのピクセルまたは頂点をスワップ チェーンにレンダリングするときに適用する、適切な 2-D または 3-D マトリックス変換を設定します。 可能性のある回転マトリックスは、次の 4 つです。

    -   landscape (DXGI\_MODE\_ROTATION\_IDENTITY)
    -   portrait (DXGI\_MODE\_ROTATION\_ROTATE270)
    -   landscape, flipped (DXGI\_MODE\_ROTATION\_ROTATE180)
    -   portrait, flipped (DXGI\_MODE\_ROTATION\_ROTATE90)

    The correct matrix is selected based on the data provided by Windows 10 (such as the results of [**DisplayInformation::OrientationChanged**](https://docs.microsoft.com/uwp/api/windows.graphics.display.displayinformation.orientationchanged)) for determining display orientation, and it will be multiplied by the coordinates of each pixel (Direct2D) or vertex (Direct3D) in the scene, effectively rotating them to align to the orientation of the screen. Direct2D では画面の原点が左上隅として定義されていますが、Direct3D では原点がウィンドウの論理的中央として定義されていることに注意してください。

> **Note**   For more info about the 2-D transformations used for rotation and how to define them, see [Defining matrices for screen rotation (2-D)](#appendix-a-applying-matrices-for-screen-rotation-2-d). 回転で使われる 3-D 変換について詳しくは、「[画面の回転のためのマトリックスの適用 (3-D)](#appendix-b-applying-matrices-for-screen-rotation-3-d)」をご覧ください。

 

ここからが重要な部分です。次のように、[**IDXGISwapChain1::SetRotation**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-setrotation) を呼び出して、更新した回転マトリックスを渡します。

`m_swapChain->SetRotation(rotation);`

また、選択された回転マトリックスを、レンダリング メソッドが新しいプロジェクションを計算するときに取得できる場所に格納します。 最終的な 3-D プロジェクションをレンダリングするとき、または最終的な 2-D レイアウトを合成するときに、このマトリックスを使います。 自動的に適用されることはありません。

その後、回転された 3-D ビューのための新しいレンダー ターゲットと、ビューのための新しい深度ステンシル バッファーを作成します。 [  **ID3D11DeviceContext:RSSetViewports**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-rssetviewports) を呼び出して、回転されたシーンのための 3-D レンダリング ビューポートを設定します。

最後に、回転またはレイアウトする 2-D 画像がある場合は、[**ID2D1DeviceContext::CreateBitmapFromDxgiSurface**](https://docs.microsoft.com/windows/desktop/api/d2d1_1/nf-d2d1_1-id2d1devicecontext-createbitmapfromdxgisurface(idxgisurface_constd2d1_bitmap_properties1__id2d1bitmap1)) を使って、サイズ変更されたスワップ チェーンのための書き込み可能なビットマップとして 2-D レンダー ターゲットを作成し、更新された向きのための新しいレイアウトを合成します。 レンダー ターゲットで、必要に応じて任意のプロパティ (コード例でのアンチエイリアシング モードなど) を設定します。

ここで、スワップ チェーンを表示します。

## <a name="reduce-the-rotation-delay-by-using-corewindowresizemanager"></a>CoreWindowResizeManager の使用による回転の遅延の短縮


By default, Windows 10 provides a short but noticeable window of time for any app, regardless of app model or language, to complete the rotation of the image. ただし、アプリがここで説明したいずれかの方法で回転の計算を行った場合は、この時間枠が閉じる前に処理を完了できる可能性があります。 残った時間は返して、回転アニメーションを完了するために使いたいと思うでしょう。 ここで、[**CoreWindowResizeManager**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindowResizeManager) が登場します。

[  **CoreWindowResizeManager**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindowResizeManager) の使い方は、次のようになります。[**DisplayInformation::OrientationChanged**](https://docs.microsoft.com/uwp/api/windows.graphics.display.displayinformation.orientationchanged) イベントが発生したら、イベントのハンドラー内で [**CoreWindowResizeManager::GetForCurrentView**](https://docs.microsoft.com/previous-versions/hh404170(v=vs.85)) を呼び出して、**CoreWindowResizeManager** のインスタンスを取得します。新しい向きのレイアウトが完了して提示されたら、[**NotifyLayoutCompleted**](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindowresizemanager.notifylayoutcompleted) を呼び出し、Windows に対して、回転アニメーションを完了してアプリ画面を表示できることを知らせます。

[  **DisplayInformation::OrientationChanged**](https://docs.microsoft.com/uwp/api/windows.graphics.display.displayinformation.orientationchanged) のイベント ハンドラー内のコードは、次のようになります。

```cpp
CoreWindowResizeManager^ resizeManager = Windows::UI::Core::CoreWindowResizeManager::GetForCurrentView();

// ... build the layout for the new display orientation ...

resizeManager->NotifyLayoutCompleted();
```

When a user rotates the orientation of the display, Windows 10 shows an animation independent of your app as feedback to the user. アニメーションは 3 つの部分から成り、次の順序で実行されます。

-   Windows 10 shrinks the original image.
-   Windows 10 holds the image for the time it takes to rebuild the new layout. これが、短縮したい時間枠です。アプリはおそらく、この時間のすべては必要としません。
-   レイアウトの時間枠が終了するか、レイアウトの完了通知を受け取ると、Windows は画像を回転させ、クロスフェードが新しい向きにズームします。

As suggested in the third bullet, when an app calls [**NotifyLayoutCompleted**](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindowresizemanager.notifylayoutcompleted), Windows 10 stops the timeout window, completes the rotation animation and returns control to your app, which is now drawing in the new display orientation. 全体的な効果は、アプリの動作が少し滑らかで機敏になり、効率もいくらか向上することです。

## <a name="appendix-a-applying-matrices-for-screen-rotation-2-d"></a>付録 A: 画面の向きのためのマトリックスの適用 (2-D)


[スワップ チェーンのサイズ変更とその内容の事前回転](#resizing-the-swap-chain-and-pre-rotating-its-contents)のサンプル　コード (および [DXGI スワップ チェーンの回転のサンプル](https://code.msdn.microsoft.com/windowsapps/DXGI-swap-chain-rotation-21d13d71)) で既にお気付きかもしれませんが、回転マトリックスは Direct2D 出力用と Direct3D 出力用とに分けてきました。 まずは、2-D マトリックスを見てみましょう。

Direct2D コンテンツと Direct3D コンテンツに同じ回転マトリックスを適用できない理由は、次の 2 つです。

-   1 つ目の理由は、異なるデカルト座標モデルが使われていることです。 Direct2D では右手の法則が使われており、Y 座標は原点から上へ移動すると正の値が増加します。 一方、Direct3D では左手の法則が使われており、Y 座標は原点から右に移動すると正の値が増加します。 そのため、Direct2D では画面座標の原点が左上にあり、Direct3D では画面 (プロジェクション平面) の原点が左下にあります。 詳しくは、「[3-D 座標系](https://docs.microsoft.com/previous-versions/windows/desktop/bb324490(v=vs.85))」をご覧ください。

    ![Direct3D 座標系。](images/direct3d-origin.png)![Direct2D 座標系。](images/direct2d-origin.png)

-   2 つ目の理由は、3-D 回転マトリックスは、丸めエラーを回避するために、明示的に指定する必要があることです。

スワップ チェーンは原点が左下にあると想定するため、右手による Direct2D 座標系を回転させて、スワップ チェーンで使われる左手による座標系に揃える必要があります。 具体的には、回転した座標系原点の変換マトリックスで回転マトリックスを乗算して、左手による新しい向きに画像を位置変更します。次に、画像を [**CoreWindow**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow) の座標空間からスワップ チェーンの座標空間に変換します。 Direct2D レンダー ターゲットがスワップ チェーンに接続されている場合は、アプリも一貫してこの変換を適用する必要があります。 ただし、スワップ チェーンに直接関連付けられていない中間サーフェスにアプリが描画している場合は、この座標空間変換を適用しないでください。

4 つの回転の中から適切なマトリックスを選ぶコードは、次のようになります (新しい座標系原点への変換に注意が必要です)。

```cpp
   
// Set the proper orientation for the swap chain, and generate 2D and
// 3D matrix transformations for rendering to the rotated swap chain.
// Note the rotation angle for the 2D and 3D transforms are different.
// This is due to the difference in coordinate spaces.  Additionally,
// the 3D matrix is specified explicitly to avoid rounding errors.

switch (displayRotation)
{
case DXGI_MODE_ROTATION_IDENTITY:
    m_orientationTransform2D = Matrix3x2F::Identity();
    m_orientationTransform3D = ScreenRotation::Rotation0;
    break;

case DXGI_MODE_ROTATION_ROTATE90:
    m_orientationTransform2D = 
        Matrix3x2F::Rotation(90.0f) *
        Matrix3x2F::Translation(m_logicalSize.Height, 0.0f);
    m_orientationTransform3D = ScreenRotation::Rotation270;
    break;

case DXGI_MODE_ROTATION_ROTATE180:
    m_orientationTransform2D = 
        Matrix3x2F::Rotation(180.0f) *
        Matrix3x2F::Translation(m_logicalSize.Width, m_logicalSize.Height);
    m_orientationTransform3D = ScreenRotation::Rotation180;
    break;

case DXGI_MODE_ROTATION_ROTATE270:
    m_orientationTransform2D = 
        Matrix3x2F::Rotation(270.0f) *
        Matrix3x2F::Translation(0.0f, m_logicalSize.Width);
    m_orientationTransform3D = ScreenRotation::Rotation90;
    break;

default:
    throw ref new FailureException();
}
    
```

2-D 画像の正しい変換マトリックスと原点を取得したら、[**ID2D1DeviceContext::BeginDraw**](https://docs.microsoft.com/windows/desktop/api/d2d1/nf-d2d1-id2d1rendertarget-begindraw) と [**ID2D1DeviceContext::EndDraw**](https://docs.microsoft.com/windows/desktop/api/d2d1/nf-d2d1-id2d1rendertarget-enddraw) の呼び出しの間で、[**ID2D1DeviceContext::SetTransform**](https://docs.microsoft.com/windows/desktop/Direct2D/id2d1rendertarget-settransform) を呼び出して設定します。

**Warning**   Direct2D doesn't have a transformation stack. アプリが [**ID2D1DeviceContext::SetTransform**](https://docs.microsoft.com/windows/desktop/Direct2D/id2d1rendertarget-settransform) を描画コードの一部としても使っている場合、このマトリックスは、適用した他のすべての変換で、右から乗算する必要があります。

 

```cpp
    ID2D1DeviceContext* context = m_deviceResources->GetD2DDeviceContext();
    Windows::Foundation::Size logicalSize = m_deviceResources->GetLogicalSize();

    context->SaveDrawingState(m_stateBlock.Get());
    context->BeginDraw();

    // Position on the bottom right corner.
    D2D1::Matrix3x2F screenTranslation = D2D1::Matrix3x2F::Translation(
        logicalSize.Width - m_textMetrics.layoutWidth,
        logicalSize.Height - m_textMetrics.height
        );

    context->SetTransform(screenTranslation * m_deviceResources->GetOrientationTransform2D());

    DX::ThrowIfFailed(
        m_textFormat->SetTextAlignment(DWRITE_TEXT_ALIGNMENT_TRAILING)
        );

    context->DrawTextLayout(
        D2D1::Point2F(0.f, 0.f),
        m_textLayout.Get(),
        m_whiteBrush.Get()
        );

    // Ignore D2DERR_RECREATE_TARGET here. This error indicates that the device
    // is lost. It will be handled during the next call to Present.
    HRESULT hr = context->EndDraw();
```

次にスワップ チェーンを表示するときは、2-D 画像が新しい表示の向きに合わせて回転されています。

## <a name="appendix-b-applying-matrices-for-screen-rotation-3-d"></a>付録 B: 画面の向きのためのマトリックスの適用 (3-D)


[スワップ チェーンのサイズ変更とその内容の事前回転](#resizing-the-swap-chain-and-pre-rotating-its-contents)のコード例 (および [DXGI スワップ チェーンの回転のサンプル](https://code.msdn.microsoft.com/windowsapps/DXGI-swap-chain-rotation-21d13d71)) では、画面の向きごとに特定の変換マトリックスを定義しました。 ここでは、3-D シーンを回転させるためのマトリックスを見てみましょう。 前と同じように、4 つのそれぞれの向きのために、マトリックスのセットを作成します。 丸めエラー、つまりわずかな視覚的アーティファクトを避けるために、コード内でマトリックスを明示的に宣言します。

これらの 3-D 回転マトリックスは、次のようにセットアップします。 次のコード例で示されているマトリックスは、カメラの 3-D シーン空間内の点を定義する頂点を 0°、90°、180°、または 270° 回転させる、標準的な回転マトリックスです。 Each vertex's \[x, y, z\] coordinate value in the scene is multiplied by this rotation matrix when the 2-D projection of the scene is computed.

```cpp
   
// 0-degree Z-rotation
static const XMFLOAT4X4 Rotation0( 
    1.0f, 0.0f, 0.0f, 0.0f,
    0.0f, 1.0f, 0.0f, 0.0f,
    0.0f, 0.0f, 1.0f, 0.0f,
    0.0f, 0.0f, 0.0f, 1.0f
    );

// 90-degree Z-rotation
static const XMFLOAT4X4 Rotation90(
    0.0f, 1.0f, 0.0f, 0.0f,
    -1.0f, 0.0f, 0.0f, 0.0f,
    0.0f, 0.0f, 1.0f, 0.0f,
    0.0f, 0.0f, 0.0f, 1.0f
    );

// 180-degree Z-rotation
static const XMFLOAT4X4 Rotation180(
    -1.0f, 0.0f, 0.0f, 0.0f,
    0.0f, -1.0f, 0.0f, 0.0f,
    0.0f, 0.0f, 1.0f, 0.0f,
    0.0f, 0.0f, 0.0f, 1.0f
    );

// 270-degree Z-rotation
static const XMFLOAT4X4 Rotation270( 
    0.0f, -1.0f, 0.0f, 0.0f,
    1.0f, 0.0f, 0.0f, 0.0f,
    0.0f, 0.0f, 1.0f, 0.0f,
    0.0f, 0.0f, 0.0f, 1.0f
    );            
    }
```

スワップ チェーンの回転の種類は、次のように、[**IDXGISwapChain1::SetRotation**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-setrotation) を呼び出して設定します。

`   m_swapChain->SetRotation(rotation);`

ここで、レンダリング メソッド内に、次のようなコードを実装します。

``` syntax
struct ConstantBuffer // This struct is provided for illustration.
{
    // Other constant buffer matrices and data are defined here.

    float4x4 projection; // Current matrix for projection
} ;
ConstantBuffer  m_constantBufferData;          // Constant buffer resource data

// ...

// Rotate the projection matrix as it will be used to render to the rotated swap chain.
m_constantBufferData.projection = mul(m_constantBufferData.projection, m_rotationTransform3D);
```

Now, when you call your render method, it multiplies the current rotation matrix (as specified by the class variable **m\_orientationTransform3D**) with the current projection matrix, and assigns the results of that operation as the new projection matrix for your renderer. スワップ チェーンを表示すると、更新された表示の向きでシーンを見ることができます。

 

 




