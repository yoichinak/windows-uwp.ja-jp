---
title: Marble Maze サンプルへの視覚的なコンテンツの追加
description: このドキュメントでは、Marble Maze ゲームがユニバーサル Windows プラットフォーム (UWP) アプリ環境で Direct3D と Direct2D をどのように使うかについて説明します。パターンを学習することにより、独自のゲーム コンテンツの開発に活用できます。
ms.assetid: 6e43422e-e1a1-b79e-2c4b-7d5b4fa88647
ms.date: 09/08/2017
ms.topic: article
keywords: Windows 10、UWP、ゲーム、サンプル、DirectX、グラフィックス
ms.localizationpriority: medium
ms.openlocfilehash: 8e00842a03eecb91e22cedf987830b28e960efd0
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258550"
---
# <a name="adding-visual-content-to-the-marble-maze-sample"></a>Marble Maze サンプルへの視覚的なコンテンツの追加




このドキュメントでは、Marble Maze ゲームがユニバーサル Windows プラットフォーム (UWP) アプリ環境で Direct3D と Direct2D をどのように使うかについて説明します。パターンを学習することにより、独自のゲーム コンテンツの開発に活用できます。 Marble Maze のアプリケーション構造全体で視覚的なゲーム コンポーネントがどのように使われているかについては、「[Marble Maze のアプリケーション構造](marble-maze-application-structure.md)」をご覧ください。

Marble Maze の視覚的側面は、次のような基本の手順に従って開発しました。

1.  Direct3D 環境と Direct2D 環境を初期化する基本フレームワークを作ります。
2.  Use image and model editing programs to design the 2D and 3D assets that appear in the game.
3.  Ensure that 2D and 3D assets properly load and appear in the game.
4.  ゲーム アセットの画質を高める頂点シェーダーとピクセル シェーダーを統合します。
5.  アニメーション、ユーザー入力などのゲーム ロジックを統合します。

We also focused first on adding 3D assets and then on 2D assets. たとえば、メニュー システムとタイマーを追加する前に、コア ゲーム ロジックに重点的に取り組みました。

また、開発プロセスでは、これらの手順の一部を何度も繰り返す必要がありました。 For example, as we made changes to the mesh and marble models, we had to also change some of the shader code that supports those models.

> [!NOTE]
> このドキュメントに対応するサンプル コードは、[DirectX Marble Maze ゲームのサンプルに関するページ](https://github.com/microsoft/Windows-appsample-marble-maze)にあります。

 
ここでは、DirectX や視覚的なゲーム コンテンツを扱うとき、つまり DirectX グラフィックス ライブラリの初期化、シーンのリソースの読み込み、シーンの更新やレンダリングを行う際の次の重要事項について説明します。

-   ゲーム コンテンツの追加には、通常、多くの手順が必要です。 多くの場合、これらの手順を繰り返すことも必要です。 Game developers often focus first on adding 3D game content and then on adding 2D content.
-   より多くの顧客に製品を届けて優れたユーザー エクスペリエンスを提供できるように、できるだけ広範囲のグラフィックス ハードウェアをサポートするようにします。
-   設計時と実行時の形式は明確に区別します。 設計時のアセットは、柔軟性を最大限に高め、コンテンツに対する迅速な繰り返し処理ができるように構造化します。 アセットは、実行時にできるだけ効率的に読み込みとレンダリングを行うことができるように、形式を設定して圧縮します。
-   Direct3D デバイスと Direct2D デバイスの UWP アプリでの作成は、従来の Windows デスクトップ アプリでの作成とほぼ同じです。 1 つの大きな違いは、スワップ チェーンと出力ウィンドウとの関連付けの方法です。
-   ゲームを設計するときは、選択したメッシュ形式が主要なシナリオをサポートすることを確かめます。 たとえば、ゲームで衝突が必要な場合は、メッシュから衝突データを取得できることを確かめます。
-   まず、すべてのシーン オブジェクトを更新してからレンダリングすることによって、ゲーム ロジックとレンダリング ロジックを切り離します。
-   You typically draw your 3D scene objects, and then any 2D objects that appear in front of the scene.
-   描画と垂直ブランクを同期して、実際にディスプレイに表示されないフレームの描画にゲームが時間をかけないようにします。 A *vertical blank* is the time between when one frame finishes drawing to the monitor and the next frame begins.

## <a name="getting-started-with-directx-graphics"></a>DirectX グラフィックスの概要


When we planned the Marble Maze Universal Windows Platform (UWP) game, we chose C++ and Direct3D 11.1 because they are excellent choices for creating 3D games that require maximum control over rendering and high performance. DirectX 11.1 は DirectX 9 から DirectX 11 までのハードウェアをサポートするため、より多くの顧客に効率よく製品を届けることができます。以前の各 DirectX バージョン用にコードを書き直す必要がないためです。

Marble Maze uses Direct3D 11.1 to render the 3D game assets, namely the marble and the maze. Marble Maze also uses Direct2D, DirectWrite, and Windows Imaging Component (WIC) to draw the 2D game assets, such as the menus and the timer.

ゲームを開発するには計画が必要です。 If you are new to DirectX graphics, we recommend that you read [DirectX: Getting started](directx-getting-started.md) to familiarize yourself with the basic concepts of creating a UWP DirectX game. As you read this document and work through the Marble Maze source code, you can refer to the following resources for more in-depth information about DirectX graphics:

-   [Direct3D 11 Graphics](https://docs.microsoft.com/windows/desktop/direct3d11/atoc-dx-graphics-direct3d-11): Describes Direct3D 11, a powerful, hardware-accelerated 3D graphics API for rendering 3D geometry on the Windows platform.
-   [Direct2D](https://docs.microsoft.com/windows/desktop/Direct2D/direct2d-portal): Describes Direct2D, a hardware-accelerated, 2D graphics API that provides high performance and high-quality rendering for 2D geometry, bitmaps, and text.
-   [DirectWrite](https://docs.microsoft.com/windows/desktop/DirectWrite/direct-write-portal): Describes DirectWrite, which supports high-quality text rendering.
-   [Windows Imaging Component](https://docs.microsoft.com/windows/desktop/wic/-wic-lh): Describes WIC, an extensible platform that provides low-level API for digital images.

### <a name="feature-levels"></a>機能レベル

Direct3D 11 introduces a paradigm named *feature levels*. 機能レベルは、明確に定義された GPU 機能のセットです。 機能レベルを使って、Direct3D ハードウェアの以前のバージョンで実行できるようにゲームのターゲットを設定します。 Marble Maze では機能レベル 9.1 がサポートされます。高いレベルの高度な機能は必要ないためです。 所有するコンピューターがハイエンドかローエンドかにかかわらず、すべての顧客に対して優れたユーザー エクスペリエンスを実現できるように、できるだけ幅広いハードウェアをサポートし、ゲーム コンテンツをそれに合わせることをお勧めします。 機能レベルについて詳しくは、「[下位レベル ハードウェアでの Direct3D 11](https://docs.microsoft.com/windows/desktop/direct3d11/overviews-direct3d-11-devices-downlevel)」をご覧ください。

## <a name="initializing-direct3d-and-direct2d"></a>Direct3D と Direct2D の初期化


デバイスはディスプレイ アダプターを表します。 Direct3D デバイスと Direct2D デバイスの UWP アプリでの作成は、従来の Windows デスクトップ アプリでの作成とほぼ同じです。 主な違いは、Direct3D スワップ チェーンをウィンドウ システムに関連付ける方法です。

**DeviceResources** クラスは、Direct3D と Direct2D を管理する基盤です。 This class handles general infrastructure, not game-specific assets. Marble Maze defines the **MarbleMazeMain** class to handle game-specific assets, which has a reference to a **DeviceResources** object to give it access to Direct3D and Direct2D.

During initialization, the **DeviceResources** constructor creates device-independent resources and the Direct3D and Direct2D devices.

```cpp
// Initialize the Direct3D resources required to run. 
DX::DeviceResources::DeviceResources() :
    m_screenViewport(),
    m_d3dFeatureLevel(D3D_FEATURE_LEVEL_9_1),
    m_d3dRenderTargetSize(),
    m_outputSize(),
    m_logicalSize(),
    m_nativeOrientation(DisplayOrientations::None),
    m_currentOrientation(DisplayOrientations::None),
    m_dpi(-1.0f),
    m_deviceNotify(nullptr)
{
    CreateDeviceIndependentResources();
    CreateDeviceResources();
}
```

**DeviceResources** クラスは、この機能を切り離して、環境が変更されたときに簡単に応答できるようにします。 たとえば、ウィンドウ サイズが変更されると **CreateWindowSizeDependentResources** メソッドを呼び出します。

###  <a name="initializing-the-direct2d-directwrite-and-wic-factories"></a>Direct2D、DirectWrite、WIC ファクトリの初期化

**DeviceResources::CreateDeviceIndependentResources** メソッドは、Direct2D、DirectWrite、WIC のファクトリを作成します。 DirectX グラフィックスにおいて、ファクトリは、グラフィックス リソースを作成するための開始点です。 Marble Maze specifies **D2D1\_FACTORY\_TYPE\_SINGLE\_THREADED** because it performs all drawing on the main thread.

```cpp
// These are the resources required independent of hardware. 
void DX::DeviceResources::CreateDeviceIndependentResources()
{
    // Initialize Direct2D resources.
    D2D1_FACTORY_OPTIONS options;
    ZeroMemory(&options, sizeof(D2D1_FACTORY_OPTIONS));

#if defined(_DEBUG)
    // If the project is in a debug build, enable Direct2D debugging via SDK Layers.
    options.debugLevel = D2D1_DEBUG_LEVEL_INFORMATION;
#endif

    // Initialize the Direct2D Factory.
    DX::ThrowIfFailed(
        D2D1CreateFactory(
            D2D1_FACTORY_TYPE_SINGLE_THREADED,
            __uuidof(ID2D1Factory2),
            &options,
            &m_d2dFactory
            )
        );

    // Initialize the DirectWrite Factory.
    DX::ThrowIfFailed(
        DWriteCreateFactory(
            DWRITE_FACTORY_TYPE_SHARED,
            __uuidof(IDWriteFactory2),
            &m_dwriteFactory
            )
        );

    // Initialize the Windows Imaging Component (WIC) Factory.
    DX::ThrowIfFailed(
        CoCreateInstance(
            CLSID_WICImagingFactory2,
            nullptr,
            CLSCTX_INPROC_SERVER,
            IID_PPV_ARGS(&m_wicFactory)
            )
        );
}
```

###  <a name="creating-the-direct3d-and-direct2d-devices"></a>Direct3D デバイスと Direct2D デバイスの作成

The **DeviceResources::CreateDeviceResources** method calls [D3D11CreateDevice](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice) to create the device object that represents the Direct3D display adapter. Because Marble Maze supports feature level 9.1 and above, the **DeviceResources::CreateDeviceResources** method specifies levels 9.1 through 11.1 in the **featureLevels** array. Direct3D はリストを順に確かめ、使用可能な最初の機能レベルをアプリに提供します。 Therefore the **D3D\_FEATURE\_LEVEL** array entries are listed from highest to lowest so that the app will get the highest feature level available. **DeviceResources::CreateDeviceResources** メソッドは、**D3D11CreateDevice** から返される Direct3D 11 デバイスを照会することによって Direct3D 11.1 デバイスを取得します。

```cpp
// This flag adds support for surfaces with a different color channel ordering
// than the API default. It is required for compatibility with Direct2D.
UINT creationFlags = D3D11_CREATE_DEVICE_BGRA_SUPPORT;

#if defined(_DEBUG)
    if (DX::SdkLayersAvailable())
    {
        // If the project is in a debug build, enable debugging via SDK Layers 
        // with this flag.
        creationFlags |= D3D11_CREATE_DEVICE_DEBUG;
    }
#endif

// This array defines the set of DirectX hardware feature levels this app will support.
// Note the ordering should be preserved.
// Don't forget to declare your application's minimum required feature level in its
// description.  All applications are assumed to support 9.1 unless otherwise stated.
D3D_FEATURE_LEVEL featureLevels[] =
{
    D3D_FEATURE_LEVEL_11_1,
    D3D_FEATURE_LEVEL_11_0,
    D3D_FEATURE_LEVEL_10_1,
    D3D_FEATURE_LEVEL_10_0,
    D3D_FEATURE_LEVEL_9_3,
    D3D_FEATURE_LEVEL_9_2,
    D3D_FEATURE_LEVEL_9_1
};

// Create the Direct3D 11 API device object and a corresponding context.
ComPtr<ID3D11Device> device;
ComPtr<ID3D11DeviceContext> context;

HRESULT hr = D3D11CreateDevice(
    nullptr,                    // Specify nullptr to use the default adapter.
    D3D_DRIVER_TYPE_HARDWARE,   // Create a device using the hardware graphics driver.
    0,                          // Should be 0 unless the driver is D3D_DRIVER_TYPE_SOFTWARE.
    creationFlags,              // Set debug and Direct2D compatibility flags.
    featureLevels,              // List of feature levels this app can support.
    ARRAYSIZE(featureLevels),   // Size of the list above.
    D3D11_SDK_VERSION,          // Always set this to D3D11_SDK_VERSION for UWP apps.
    &device,                    // Returns the Direct3D device created.
    &m_d3dFeatureLevel,         // Returns feature level of device created.
    &context                    // Returns the device immediate context.
    );

if (FAILED(hr))
{
    // If the initialization fails, fall back to the WARP device.
    // For more information on WARP, see:
    // https://go.microsoft.com/fwlink/?LinkId=286690
    DX::ThrowIfFailed(
        D3D11CreateDevice(
            nullptr,
            D3D_DRIVER_TYPE_WARP, // Create a WARP device instead of a hardware device.
            0,
            creationFlags,
            featureLevels,
            ARRAYSIZE(featureLevels),
            D3D11_SDK_VERSION,
            &device,
            &m_d3dFeatureLevel,
            &context
            )
        );
}

// Store pointers to the Direct3D 11.1 API device and immediate context.
DX::ThrowIfFailed(
    device.As(&m_d3dDevice)
    );

DX::ThrowIfFailed(
    context.As(&m_d3dContext)
    );
```

次に、**DeviceResources::CreateDeviceResources** メソッドが Direct2D デバイスを作成します。 Direct2D は、Direct3D との相互運用に Microsoft DirectX Graphic Infrastructure (DXGI) を使います。 DXGI によって、ビデオ メモリ サーフェスをグラフィックス ランタイム間で共有できるようになります。 Marble Maze は、Direct3D デバイスから基になる DXGI デバイスを使って、Direct2D ファクトリから Direct2D デバイスを作成します。

```cpp
// Create the Direct2D device object and a corresponding context.
ComPtr<IDXGIDevice3> dxgiDevice;
DX::ThrowIfFailed(
    m_d3dDevice.As(&dxgiDevice)
    );

DX::ThrowIfFailed(
    m_d2dFactory->CreateDevice(dxgiDevice.Get(), &m_d2dDevice)
    );

DX::ThrowIfFailed(
    m_d2dDevice->CreateDeviceContext(
        D2D1_DEVICE_CONTEXT_OPTIONS_NONE,
        &m_d2dContext
        )
    );
```

DXGI や Direct2D と Direct3D の相互運用について詳しくは、「[DXGI の概要](https://docs.microsoft.com/windows/desktop/direct3ddxgi/d3d10-graphics-programming-guide-dxgi)」と「[Direct2D と Direct3D の相互運用性の概要](https://docs.microsoft.com/windows/desktop/Direct2D/direct2d-and-direct3d-interoperation-overview)」をご覧ください。

### <a name="associating-direct3d-with-the-view"></a>Direct3D とビューの関連付け

**DeviceResources::CreateWindowSizeDependentResources** メソッドは、スワップ チェーン、Direct3D と Direct2D のレンダー ターゲットなど、所定のウィンドウ サイズによって異なるグラフィックス リソースを作成します。 DirectX UWP アプリとデスクトップ アプリとの大きな違いは、スワップ チェーンが出力ウィンドウと関連付けられる方法です。 スワップ チェーンは、デバイスがモニターにレンダリングするバッファーを表示します。 [Marble Maze application structure](marble-maze-application-structure.md) describes how the windowing system for a UWP app differs from a desktop app. Because a UWP app does not work with [HWND](https://docs.microsoft.com/windows/desktop/WinProg/windows-data-types) objects, Marble Maze must use the [IDXGIFactory2::CreateSwapChainForCoreWindow](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgifactory2-createswapchainforcorewindow) method to associate the device output to the view. 次の例に、スワップ チェーンを作成する **DeviceResources::CreateWindowSizeDependentResources** メソッドの一部を示します。

```cpp
// Obtain the final swap chain for this window from the DXGI factory.
DX::ThrowIfFailed(
    dxgiFactory->CreateSwapChainForCoreWindow(
        m_d3dDevice.Get(),
        reinterpret_cast<IUnknown*>(m_window.Get()),
        &swapChainDesc,
        nullptr,
        &m_swapChain
        )
    );
```

To minimize power consumption, which is important to do on battery-powered devices such as laptops and tablets, the **DeviceResources::CreateWindowSizeDependentResources** method calls the [IDXGIDevice1::SetMaximumFrameLatency](https://docs.microsoft.com/windows/desktop/api/dxgi/nf-dxgi-idxgidevice1-setmaximumframelatency) method to ensure that the game is rendered only after the vertical blank. Synchronizing with the vertical blank is described in greater detail in the section [Presenting the scene](#presenting-the-scene) in this document.

```cpp
// Ensure that DXGI does not queue more than one frame at a time. This both 
// reduces latency and ensures that the application will only render after each
// VSync, minimizing power consumption.
DX::ThrowIfFailed(
    dxgiDevice->SetMaximumFrameLatency(1)
    );
```

**DeviceResources::CreateWindowSizeDependentResources** メソッドは、多くのゲームに対応する方法でグラフィックス リソースを初期化します。

> [!NOTE]
> The term *view* has a different meaning in the Windows Runtime than it has in Direct3D. Windows ランタイムでは、ビューは、アプリのユーザー インターフェイス設定のコレクション (表示領域、入力動作、処理に使うスレッドなどを含む) を指します。 ビューを作成するときは、必要な構成と設定を指定します。 アプリのビューを設定するプロセスについては、「[Marble Maze のアプリケーション構造](marble-maze-application-structure.md)」で説明します。
> Direct3D では、ビューという用語には複数の意味があります。 A resource view defines the subresources that a resource can access. たとえば、テクスチャ オブジェクトがシェーダー リソース ビューに関連付けられている場合、そのシェーダーは後でテクスチャにアクセスできます。 リソース ビューの 1 つの長所は、レンダリング パイプラインの段階ごとに異なる方法でデータを解釈できることです。 For more information about resource views, see [Resource Views](https://docs.microsoft.com/windows/desktop/direct3d11/overviews-direct3d-11-resources-intro).
> ビュー変換またはビュー変換マトリックスのコンテキストで使われた場合、ビューは、カメラの位置と向きを表します。 ビュー変換は、カメラの位置と向きを基準として、ワールド内でオブジェクトを再配置します。 ビュー変換について詳しくは、「[ビュー変換 (Direct3D 9)](https://docs.microsoft.com/windows/desktop/direct3d9/view-transform)」をご覧ください。 Marble Maze でリソース ビューやマトリックス ビューをどのように使っているかについて、このトピックで詳しく説明しています。

 

## <a name="loading-scene-resources"></a>シーン リソースの読み込み


Marble Maze uses the **BasicLoader** class, which is declared in **BasicLoader.h**, to load textures and shaders. Marble Maze uses the **SDKMesh** class to load the 3D meshes for the maze and the marble.

Marble Maze では、応答性を保持するために、非同期的に (つまりバックグラウンドで) シーン リソースを読み込みます。 バックグラウンドでのアセットの読み込み中、ゲームはウィンドウ イベントに応答できます。 このプロセスについては、このガイドの「[バックグラウンドでのゲーム アセットの読み込み](marble-maze-application-structure.md#loading-game-assets-in-the-background)」で詳しく説明しています。

###  <a name="loading-the-2d-overlay-and-user-interface"></a>Loading the 2D overlay and user interface

Marble Maze では、オーバーレイは、画面の一番上に表示される画像です。 オーバーレイは、常にシーンの前面に表示されます。 In Marble Maze, the overlay contains the Windows logo and the text string **DirectX Marble Maze game sample**. The management of the overlay is performed by the **SampleOverlay** class, which is defined in **SampleOverlay.h**. Direct3D サンプルの一部としてオーバーレイを使いますが、このコードを利用すると、シーンの前面に任意の画像を表示することができます。

One important aspect of the overlay is that, because its contents do not change, the **SampleOverlay** class draws, or caches, its contents to an [ID2D1Bitmap1](https://docs.microsoft.com/windows/desktop/api/d2d1_1/nn-d2d1_1-id2d1bitmap1) object during initialization. 描画時に **SampleOverlay** クラスが行う必要があるのは、画面にビットマップを描画することだけです。 このように、テキスト描画などコストの高いルーチンを、すべてのフレームで実行する必要はありません。

The user interface (UI) consists of 2D components, such as menus and heads-up displays (HUDs), which appear in front of your scene. Marble Maze では、次の UI 要素を定義しています。

-   ユーザーがゲームを開始したりハイ スコアを表示できるようにするためのメニュー項目。
-   プレイ開始までの 3 秒をカウント ダウンするタイマー。
-   経過したプレイ時間を追跡するタイマー。
-   最速記録を表示する表。
-   Text that reads **Paused** when the game is paused.

Marble Maze defines game-specific UI elements in **UserInterface.h**. Marble Maze では、**ElementBase** クラスをすべての UI 要素の基本型として定義しています。 **ElementBase** クラスは、UI 要素のサイズ、位置、配置、可視性などの属性を定義します。 さらに、要素の更新方法やレンダリング方法も制御します。

```cpp
class ElementBase
{
public:
    virtual void Initialize() { }
    virtual void Update(float timeTotal, float timeDelta) { }
    virtual void Render() { }

    void SetAlignment(AlignType horizontal, AlignType vertical);
    virtual void SetContainer(const D2D1_RECT_F& container);
    void SetVisible(bool visible);

    D2D1_RECT_F GetBounds();

    bool IsVisible() const { return m_visible; }

protected:
    ElementBase();

    virtual void CalculateSize() { }

    Alignment       m_alignment;
    D2D1_RECT_F     m_container;
    D2D1_SIZE_F     m_size;
    bool            m_visible;
};
```

UI 要素の共通基底クラスを提供することで、ユーザー インターフェイスを管理する **UserInterface** クラスが行う必要があるのは、**ElementBase** オブジェクトのコレクションの保持のみになります。これにより、UI の管理が簡略化され、再利用可能なユーザー インターフェイス マネージャーが提供されます。 Marble Maze では、ゲーム固有の動作を実装する型を **ElementBase** から派生させて定義します。 たとえば、**HighScoreTable** は、ハイ スコア表の動作を定義します。 これらの型について詳しくは、ソース コードをご覧ください。

> [!NOTE]
> Because XAML enables you to more easily create complex user interfaces, like those found in simulation and strategy games, consider whether to use XAML to define your UI. For info about how to develop a user interface in XAML in a DirectX UWP game, see [Extend the game sample](tutorial-resources.md), which refers to the DirectX 3D shooting game sample.

 

###  <a name="loading-shaders"></a>シェーダーの読み込み

Marble Maze では、**BasicLoader::LoadShader** メソッドを使ってファイルからシェーダーを読み込みます。

シェーダーは、現在のゲームの GPU プログラミングの基本単位です。 Nearly all 3D graphics processing is driven through shaders, whether it is model transformation and scene lighting, or more complex geometry processing, from character skinning to tessellation. シェーダーのプログラミング モデルについて詳しくは、「[HLSL](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl)」をご覧ください。

Marble Maze では、頂点シェーダーとピクセル シェーダーを使っています。 頂点シェーダーは、常に、入力された 1 つの頂点を処理し、出力として 1 つの頂点を生成します。 ピクセル シェーダーは、数値、テクスチャ データ、補間された頂点単位の値、その他のデータを受け取り、出力としてピクセル色を生成します。 1 つのシェーダーは一度に 1 つの要素を変換するため、複数のシェーダー パイプラインを提供するグラフィックス ハードウェアは要素のセットを並列処理できます。 GPU で使用できる並列パイプラインの数は、CPU で使用可能な数を大きく上回る可能性があります。 したがって、基本的なシェーダーでさえスループットを大幅に向上することができます。

The **MarbleMazeMain::LoadDeferredResources** method loads one vertex shader and one pixel shader after it loads the overlay. The design-time versions of these shaders are defined in **BasicVertexShader.hlsl** and **BasicPixelShader.hlsl**, respectively. Marble Maze では、レンダリング フェーズでこれらのシェーダーをボールと迷路の両方に適用します。

Marble Maze プロジェクトには、シェーダー ファイルの .hlsl バージョン (設計時の形式) と .cso バージョン (実行時形式) の両方が含まれています。 ビルド時に Visual Studio が fxc.exe エフェクト コンパイラを使って .hlsl ソース ファイルを .cso バイナリ シェーダーにコンパイルします。 エフェクト コンパイラ ツールについて詳しくは、「[エフェクト コンパイラ ツール](https://docs.microsoft.com/windows/desktop/direct3dtools/fxc)」をご覧ください。

頂点シェーダーは、指定されたモデル マトリックス、ビュー マトリックス、プロジェクション マトリックスを使って、入力ジオメトリを変換します。 入力ジオメトリの位置データは変換されて、2 回出力されます。まずレンダリングのために必要な画面空間内に出力され、ピクセル シェーダーが照明計算を実行できるように再びワールド空間内に出力されます。 サーフェスの標準ベクターはワールド空間に変換されます。これも、これもピクセル シェーダーが照明のために使います。 テクスチャ座標は、変更されずにピクセル シェーダーに渡されます。

```hlsl
sPSInput main(sVSInput input)
{
    sPSInput output;
    float4 temp = float4(input.pos, 1.0f);
    temp = mul(temp, model);
    output.worldPos = temp.xyz / temp.w;
    temp = mul(temp, view);
    temp = mul(temp, projection);
    output.pos = temp;
    output.tex = input.tex;
    output.norm = mul(float4(input.norm, 0.0f), model).xyz;
    return output;
}
```

ピクセル シェーダーは、頂点シェーダーの出力を入力として受け取ります。 このシェーダーは照明計算を実行して、輪郭がぼやけたスポットライトを模倣します。このライトは、大理石の位置に合わせて迷路上を動きます。 照明は、光に直面するサーフェスで最も強くなります。 サーフェス法線が光に対して直角になるにつれて、拡散コンポーネントは減少してゼロになります。また、法線の向きが光から離れるにつれて、環境光が減少します。 大理石に近い (つまりスポットライトの中央に近い) ほど照明が強くなります。 ただし、大理石の下では柔らかい影をシミュレートするために、照明が調整されます。 実際の環境では、白い大理石のようなオブジェクトは、シーンの他のオブジェクトに拡散的にスポットライトを反射します。 これは、大理石の明るい側の半球のビューにあるサーフェスについて概算されます。 照明のその他の係数は、大理石に対する相対的な角度と距離です。 生成されるピクセル色は、サンプリングされたテクスチャと照明計算の結果を合成したものになります。

```hlsl
float4 main(sPSInput input) : SV_TARGET
{
    float3 lightDirection = float3(0, 0, -1);
    float3 ambientColor = float3(0.43, 0.31, 0.24);
    float3 lightColor = 1 - ambientColor;
    float spotRadius = 50;

    // Basic ambient (Ka) and diffuse (Kd) lighting from above.
    float3 N = normalize(input.norm);
    float NdotL = dot(N, lightDirection);
    float Ka = saturate(NdotL + 1);
    float Kd = saturate(NdotL);

    // Spotlight.
    float3 vec = input.worldPos - marblePosition;
    float dist2D = sqrt(dot(vec.xy, vec.xy));
    Kd = Kd * saturate(spotRadius / dist2D);

    // Shadowing from ball.
    if (input.worldPos.z > marblePosition.z)
        Kd = Kd * saturate(dist2D / (marbleRadius * 1.5));

    // Diffuse reflection of light off ball.
    float dist3D = sqrt(dot(vec, vec));
    float3 V = normalize(vec);
    Kd += saturate(dot(-V, N)) * saturate(dot(V, lightDirection))
        * saturate(marbleRadius / dist3D);

    // Final composite.
    float4 diffuseTexture = Texture.Sample(Sampler, input.tex);
    float3 color = diffuseTexture.rgb * ((ambientColor * Ka) + (lightColor * Kd));
    return float4(color * lightStrength, diffuseTexture.a);
}
```

> [!WARNING]
> The compiled pixel shader contains 32 arithmetic instructions and 1 texture instruction. このシェーダーは、デスクトップ コンピューターとハイエンドのタブレットで正常に動作する必要があります。 ただし、ローエンドのコンピューターでは、このシェーダーを処理することができず、対話型のフレーム レートが提供される場合があります。 対象ユーザーの標準的なハードウェアを検討し、そのハードウェアの性能に合わせてシェーダーを設計します。

 

The **MarbleMazeMain::LoadDeferredResources** method uses the **BasicLoader::LoadShader** method to load the shaders. 次の例では、頂点シェーダーを読み込みます。 The run-time format for this shader is **BasicVertexShader.cso**. The **m\_vertexShader** member variable is an [ID3D11VertexShader](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11vertexshader) object.

```cpp
BasicLoader^ loader = ref new BasicLoader(m_deviceResources->GetD3DDevice());

D3D11_INPUT_ELEMENT_DESC layoutDesc [] =
{
    { "POSITION", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 0, D3D11_INPUT_PER_VERTEX_DATA, 0 },
    { "NORMAL", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 12, D3D11_INPUT_PER_VERTEX_DATA, 0 },
    { "TEXCOORD", 0, DXGI_FORMAT_R32G32_FLOAT, 0, 24, D3D11_INPUT_PER_VERTEX_DATA, 0 },
    { "TANGENT", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 32, D3D11_INPUT_PER_VERTEX_DATA, 0 },
};
m_vertexStride = 44; // must set this to match the size of layoutDesc above

Platform::String^ vertexShaderName = L"BasicVertexShader.cso";
loader->LoadShader(
    vertexShaderName,
    layoutDesc,
    ARRAYSIZE(layoutDesc),
    &m_vertexShader,
    &m_inputLayout
    );
```

The **m\_inputLayout** member variable is an [ID3D11InputLayout](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11inputlayout) object. 入力レイアウト オブジェクトは、入力アセンブラー (IA) ステージの入力状態をカプセル化します。 IA ステージの 1 つの仕事は、シェーダーの効率を向上することです。システム生成値 (*セマンティクス*) を使って、まだ処理されていないプリミティブまたは頂点のみを処理します。

Use the [ID3D11Device::CreateInputLayout](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createinputlayout) method to create an input-layout from an array of input-element descriptions. 配列には 1 つまたは複数の入力要素が含まれます。各入力要素は、1 つの頂点バッファーの 1 つの頂点データ要素を記述します。 入力要素の記述のセット全体によって、IA ステージにバインドされているすべての頂点バッファーのすべての頂点データ要素を記述します。 

**layoutDesc** in the above code snippet shows the layout description that Marble Maze uses. このレイアウトの記述には、4 つの頂点データ要素を含む頂点バッファーが記述されています。 配列の各エントリの重要な部分は、セマンティック名、データ形式、バイト オフセットです。 たとえば、**POSITION** 要素は、オブジェクト空間での頂点の位置を指定します。 これは、バイト オフセット 0 で開始し、3 つの浮動小数点コンポーネントを含みます (合計 12 バイト)。 **NORMAL** 要素は、標準ベクターを指定します。 これはバイト オフセット 12 で開始します。レイアウトで、12 バイトを必要とする **POSITION** の後にあるためです。 **NORMAL** 要素は、4 つの要素で構成される 32 ビット符号なし整数を含みます。

次の例に示すように、頂点シェーダーによって定義される **sVSInput** 構造体と入力レイアウトを比較します。 **sVSInput** 構造体は、**POSITION**、**NORMAL**、**TEXCOORD0** の各要素を定義します。 DirectX ランタイムは、シェーダーによって定義された入力構造体にレイアウトの各要素をマップします。

```hlsl
struct sVSInput
{
    float3 pos : POSITION;
    float3 norm : NORMAL;
    float2 tex : TEXCOORD0;
};

struct sPSInput
{
    float4 pos : SV_POSITION;
    float3 norm : NORMAL;
    float2 tex : TEXCOORD0;
    float3 worldPos : TEXCOORD1;
};

sPSInput main(sVSInput input)
{
    sPSInput output;
    float4 temp = float4(input.pos, 1.0f);
    temp = mul(temp, model);
    output.worldPos = temp.xyz / temp.w;
    temp = mul(temp, view);
    temp = mul(temp, projection);
    output.pos = temp;
    output.tex = input.tex;
    output.norm = mul(float4(input.norm, 0.0f), model).xyz;
    return output;
}
```

「[セマンティクス](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-semantics)」では、使用できるセマンティクスのそれぞれについてさらに詳しく説明しています。

> [!NOTE]
> In a layout, you can specify additional components that are not used to enable multiple shaders to share the same layout. たとえば、**TANGENT** 要素はシェーダーでは使われません。 法線マッピングなどの手法を使う場合は、**TANGENT** 要素を使うことができます。 法線マッピング (バンプ マッピング) を使うと、オブジェクトのサーフェスに対してバンプ エフェクトを作成できます。 バンプ マッピングについて詳しくは、「[バンプ マッピング (Direct3D 9)](https://docs.microsoft.com/windows/desktop/direct3d9/bump-mapping)」をご覧ください。

 

For more information about the input assembly stage, see [Input-Assembler Stage](https://docs.microsoft.com/windows/desktop/direct3d11/d3d10-graphics-programming-guide-input-assembler-stage) and [Getting Started with the Input-Assembler Stage](https://docs.microsoft.com/windows/desktop/direct3d11/d3d10-graphics-programming-guide-input-assembler-stage-getting-started).

頂点シェーダーとピクセル シェーダーを使ってシーンをレンダリングするプロセスについては、このドキュメントの「[シーンのレンダリング](#rendering-the-scene)」で説明しています。

### <a name="creating-the-constant-buffer"></a>定数バッファーの作成

Direct3D バッファーは、データのコレクションをグループ化します。 定数バッファーは、シェーダーにデータを渡すために使うことができるバッファーの種類の 1 つです。 Marble Maze では、定数バッファーを使って、モデル (またはワールド) ビューと、アクティブなシーン オブジェクトのためのプロジェクション マトリックスを保持します。

The following example shows how the **MarbleMazeMain::LoadDeferredResources** method creates a constant buffer that will later hold matrix data. The example creates a **D3D11\_BUFFER\_DESC** structure that uses the **D3D11\_BIND\_CONSTANT\_BUFFER** flag to specify usage as a constant buffer. This example then passes that structure to the [ID3D11Device::CreateBuffer](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createbuffer) method. The **m\_constantBuffer** variable is an [ID3D11Buffer](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11buffer) object.

```cpp
// Create the constant buffer for updating model and camera data.
D3D11_BUFFER_DESC constantBufferDesc = {0};

// Multiple of 16 bytes
constantBufferDesc.ByteWidth = ((sizeof(ConstantBuffer) + 15) / 16) * 16;

constantBufferDesc.Usage               = D3D11_USAGE_DEFAULT;
constantBufferDesc.BindFlags           = D3D11_BIND_CONSTANT_BUFFER;
constantBufferDesc.CPUAccessFlags      = 0;
constantBufferDesc.MiscFlags           = 0;

// This will not be used as a structured buffer, so this parameter is ignored.
constantBufferDesc.StructureByteStride = 0;

DX::ThrowIfFailed(
    m_deviceResources->GetD3DDevice()->CreateBuffer(
        &constantBufferDesc,
        nullptr,    // leave the buffer uninitialized
        &m_constantBuffer
        )
    );
```

The **MarbleMazeMain::Update** method later updates **ConstantBuffer** objects, one for the maze and one for the marble. The **MarbleMazeMain::Render** method then binds each **ConstantBuffer** object to the constant buffer before each object is rendered. The following example shows the **ConstantBuffer** structure, which is in **MarbleMazeMain.h**.

```cpp
// Describes the constant buffer that draws the meshes.
struct ConstantBuffer
{
    XMFLOAT4X4 model;
    XMFLOAT4X4 view;
    XMFLOAT4X4 projection;

    XMFLOAT3 marblePosition;
    float marbleRadius;
    float lightStrength;
};
```

To better understand how constant buffers map to shader code, compare the **ConstantBuffer** structure in **MarbleMazeMain.h** to the **ConstantBuffer** constant buffer that is defined by the vertex shader in **BasicVertexShader.hlsl**:

```hlsl
cbuffer ConstantBuffer : register(b0)
{
    matrix model;
    matrix view;
    matrix projection;
    float3 marblePosition;
    float marbleRadius;
    float lightStrength;
};
```

**ConstantBuffer** 構造体のレイアウトは、**cbuffer** オブジェクトと一致します。 **cbuffer** 変数は、レジスタ b0 を指定します。つまり、定数バッファー データがレジスタ 0 に格納されます。 The **MarbleMazeMain::Render** method specifies register 0 when it activates the constant buffer. このプロセスについては、このドキュメントの後の方で詳しく説明します。

定数バッファーについて詳しくは、「[Direct3D 11 のバッファーについて](https://docs.microsoft.com/windows/desktop/direct3d11/overviews-direct3d-11-resources-buffers-intro)」をご覧ください。 For more information about the register keyword, see [register](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-variable-register).

###  <a name="loading-meshes"></a>メッシュの読み込み

Marble Maze では、実行時の形式として SDK メッシュを使います。この形式では、サンプル アプリケーションのメッシュ データを読み込む基本的な方法が提供されるためです。 本番で使うには、ゲーム固有の要件を満たすメッシュ形式を使う必要があります。

The **MarbleMazeMain::LoadDeferredResources** method loads mesh data after it loads the vertex and pixel shaders. メッシュは頂点データのコレクションです。多くの場合、位置、法線データ、色、素材、テクスチャ座標などの情報が含まれます。 Meshes are typically created in 3D authoring software and maintained in files that are separate from application code. 大理石と迷路は、このゲームに使われているメッシュの 2 つの例です。

Marble Maze では、**SDKMesh** クラスを使ってメッシュを管理します。 This class is declared in **SDKMesh.h**. **SDKMesh** は、メッシュ データを読み込み、レンダリングし、破棄するためのメソッドを提供します。

> [!IMPORTANT]
> Marble Maze uses the SDK-Mesh format and provides the **SDKMesh** class for illustration only. SDK メッシュ形式は学習用やプロトタイプの作成用に役立ちますが、ごく基本的な形式であるため、多くのゲーム開発の要件を満たさない可能性があります。 ゲーム固有の要件を満たすメッシュ形式を使うことをお勧めします。

 

The following example shows how the **MarbleMazeMain::LoadDeferredResources** method uses the **SDKMesh::Create** method to load mesh data for the maze and for the ball.

```cpp
// Load the meshes.
DX::ThrowIfFailed(
    m_mazeMesh.Create(
        m_deviceResources->GetD3DDevice(),
        L"Media\\Models\\maze1.sdkmesh",
        false
        )
    );

DX::ThrowIfFailed(
    m_marbleMesh.Create(
        m_deviceResources->GetD3DDevice(),
        L"Media\\Models\\marble2.sdkmesh",
        false
        )
    );
```

###  <a name="loading-collision-data"></a>衝突データの読み込み

ここでは、Marble Maze で大理石と迷路の間の物理シミュレーションを実装する方法を特に説明しませんが、メッシュが読み込まれるときに物理システムのメッシュ ジオメトリが読み取られる点に注目してください。

```cpp
// Extract mesh geometry for the physics system.
DX::ThrowIfFailed(
    ExtractTrianglesFromMesh(
        m_mazeMesh,
        "Mesh_walls",
        m_collision.m_wallTriList
        )
    );

DX::ThrowIfFailed(
    ExtractTrianglesFromMesh(
        m_mazeMesh,
        "Mesh_Floor",
        m_collision.m_groundTriList
        )
    );

DX::ThrowIfFailed(
    ExtractTrianglesFromMesh(
        m_mazeMesh,
        "Mesh_floorSides",
        m_collision.m_floorTriList
        )
    );

m_physics.SetCollision(&m_collision);
float radius = m_marbleMesh.GetMeshBoundingBoxExtents(0).x / 2;
m_physics.SetRadius(radius);
```

The way that you load collision data largely depends on the run-time format that you use. For more information about how Marble Maze loads the collision geometry from an SDK-Mesh file, see the **MarbleMazeMain::ExtractTrianglesFromMesh** method in the source code.

## <a name="updating-game-state"></a>ゲームの状態の更新


Marble Maze では、まずすべてのシーン オブジェクトを更新してからレンダリングすることによって、ゲーム ロジックとレンダリング ロジックを分離しています。

[Marble Maze application structure](marble-maze-application-structure.md) describes the main game loop. ゲーム ループの一部であるシーンの更新は、Windows イベントの後の入力が処理された後で、シーンがレンダリングされる前に行います。 The **MarbleMazeMain::Update** method handles the update of the UI and the game.

### <a name="updating-the-user-interface"></a>ユーザー インターフェイスの更新

The **MarbleMazeMain::Update** method calls the **UserInterface::Update** method to update the state of the UI.

```cpp
UserInterface::GetInstance().Update(
    static_cast<float>(m_timer.GetTotalSeconds()), 
    static_cast<float>(m_timer.GetElapsedSeconds()));
```

**UserInterface::Update** メソッドは、UI コレクション内の各要素を更新します。

```cpp
void UserInterface::Update(float timeTotal, float timeDelta)
{
    for (auto iter = m_elements.begin(); iter != m_elements.end(); ++iter)
    {
        (*iter)->Update(timeTotal, timeDelta);
    }
}
```

Classes that derive from **ElementBase** (defined in **UserInterface.h**) implement the **Update** method to perform specific behaviors. たとえば、**StopwatchTimer::Update** メソッドは、指定された時間で経過時間を更新し、後で表示されるテキストを更新します。

```cpp
void StopwatchTimer::Update(float timeTotal, float timeDelta)
{
    if (m_active)
    {
        m_elapsedTime += timeDelta;

        WCHAR buffer[16];
        GetFormattedTime(buffer);
        SetText(buffer);
    }

    TextElement::Update(timeTotal, timeDelta);
}
```

###  <a name="updating-the-scene"></a>シーンの更新

The **MarbleMazeMain::Update** method updates the game based on the current state of the state machine (the **GameState**, stored in **m_gameState**). When the game is in the active state (**GameState::InGameActive**), Marble Maze updates the camera to follow the marble, updates the view matrix part of the constant buffers, and updates the physics simulation.

The following example shows how the **MarbleMazeMain::Update** method updates the position of the camera. Marble Maze uses the **m\_resetCamera** variable to flag that the camera must be reset to be located directly above the marble. カメラがリセットされるのは、ゲームが開始するときか大理石が迷路を通り抜けたときです。 メイン メニューまたはハイスコア表示画面がアクティブな場合、カメラは定位置に設定されます。 それ以外の場合、Marble Maze では、*timeDelta* パラメーターを使って、カメラの位置を現在位置と目標位置の間で補間します。 目標位置は大理石の斜め前方です。 経過フレーム時間を使うと、カメラが大理石を少しずつ追いかける、つまり追跡することができます。

```cpp
static float eyeDistance = 200.0f;
static XMFLOAT3A eyePosition = XMFLOAT3A(0, 0, 0);

// Gradually move the camera above the marble.
XMFLOAT3A targetEyePosition;
XMStoreFloat3A(
    &targetEyePosition, 
    XMLoadFloat3A(&marblePosition) - (XMLoadFloat3A(&g) * eyeDistance));

if (m_resetCamera)
{
    eyePosition = targetEyePosition;
    m_resetCamera = false;
}
else
{
    XMStoreFloat3A(
        &eyePosition, 
        XMLoadFloat3A(&eyePosition) 
            + ((XMLoadFloat3A(&targetEyePosition) - XMLoadFloat3A(&eyePosition)) 
                * min(1, static_cast<float>(m_timer.GetElapsedSeconds()) * 8)
            )
    );
}

// Look at the marble. 
if ((m_gameState == GameState::MainMenu) || (m_gameState == GameState::HighScoreDisplay))
{
    // Override camera position for menus.
    XMStoreFloat3A(
        &eyePosition, 
        XMLoadFloat3A(&marblePosition) + XMVectorSet(75.0f, -150.0f, -75.0f, 0.0f));

    m_camera->SetViewParameters(
        eyePosition, 
        marblePosition, 
        XMFLOAT3(0.0f, 0.0f, -1.0f));
}
else
{
    m_camera->SetViewParameters(eyePosition, marblePosition, XMFLOAT3(0.0f, 1.0f, 0.0f));
}
```

The following example shows how the **MarbleMazeMain::Update** method updates the constant buffers for the marble and the maze. 迷路のモデル (ワールド) マトリックスは、常に単位マトリックスです。 要素がすべて 1 のメイン対角線を除き、単位マトリックスは、0 で構成される正方形マトリックスです。 大理石のモデル マトリックスは、位置マトリックスと回転マトリックスを掛けた値に基づいています。

```cpp
// Update the model matrices based on the simulation.
XMStoreFloat4x4(&m_mazeConstantBufferData.model, XMMatrixIdentity());

XMStoreFloat4x4(
    &m_marbleConstantBufferData.model, 
    XMMatrixTranspose(
        XMMatrixMultiply(
            marbleRotationMatrix, 
            XMMatrixTranslationFromVector(XMLoadFloat3A(&marblePosition))
        )
    )
);

// Update the view matrix based on the camera.
XMFLOAT4X4 view;
m_camera->GetViewMatrix(&view);
m_mazeConstantBufferData.view = view;
m_marbleConstantBufferData.view = view;
```

For information about how the **MarbleMazeMain::Update** method reads user input and simulates the motion of the marble, see [Adding input and interactivity to the Marble Maze sample](adding-input-and-interactivity-to-the-marble-maze-sample.md).

## <a name="rendering-the-scene"></a>シーンのレンダリング


シーンがレンダリングされるとき、通常は次の手順が含まれます。

1.  現在のレンダー ターゲットの深度ステンシル バッファーを設定します。
2.  レンダーおよびステンシル ビューをクリアします。
3.  描画のために頂点シェーダーとピクセル シェーダーを準備します。
4.  Render the 3D objects in the scene.
5.  Render any 2D object that you want to appear in front of the scene.
6.  レンダリングした画像をモニターに表示します。

The **MarbleMazeMain::Render** method binds the render target and depth stencil views, clears those views, draws the scene, and then draws the overlay.

###  <a name="preparing-the-render-targets"></a>レンダー ターゲットの準備

シーンをレンダリングする前に、現在のレンダー ターゲットの深度ステンシル バッファーを設定する必要があります。 シーンが画面のすべてのピクセルに描画される保証がない場合は、レンダー ビューとステンシル ビューもクリアします。 Marble Maze では、すべてのフレームのレンダー ビューとステンシル ビューをクリアして、前のフレームに由来して表示されるアーティファクトがないことを確かめます。

The following example shows how the **MarbleMazeMain::Render** method calls the [ID3D11DeviceContext::OMSetRenderTargets](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-omsetrendertargets) method to set the render target and the depth-stencil buffer as the current ones.

```cpp
auto context = m_deviceResources->GetD3DDeviceContext();

// Reset the viewport to target the whole screen.
auto viewport = m_deviceResources->GetScreenViewport();
context->RSSetViewports(1, &viewport);

// Reset render targets to the screen.
ID3D11RenderTargetView *const targets[1] = 
    { m_deviceResources->GetBackBufferRenderTargetView() };

context->OMSetRenderTargets(1, targets, m_deviceResources->GetDepthStencilView());

// Clear the back buffer and depth stencil view.
context->ClearRenderTargetView(
    m_deviceResources->GetBackBufferRenderTargetView(), 
    DirectX::Colors::Black);

context->ClearDepthStencilView(
    m_deviceResources->GetDepthStencilView(), 
    D3D11_CLEAR_DEPTH | D3D11_CLEAR_STENCIL, 
    1.0f, 
    0);
```

The [ID3D11RenderTargetView](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11rendertargetview) and [ID3D11DepthStencilView](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11depthstencilview) interfaces support the texture view mechanism that is provided by Direct3D 10 and later. テクスチャ ビューについて詳しくは、「[テクスチャ ビュー (Direct3D 10)](https://docs.microsoft.com/windows/desktop/direct3d10/d3d10-graphics-programming-guide-resources-access-views)」をご覧ください。 The [OMSetRenderTargets](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-omsetrendertargets) method prepares the output-merger stage of the Direct3D pipeline. 出力マージャー ステージについて詳しくは、「[出力マージャー ステージ](https://docs.microsoft.com/windows/desktop/direct3d11/d3d10-graphics-programming-guide-output-merger-stage)」をご覧ください。

### <a name="preparing-the-vertex-and-pixel-shaders"></a>頂点シェーダーとピクセル シェーダーの準備

シーン オブジェクトをレンダリングする前に、次の手順を実行して、描画用のために頂点シェーダーとピクセル シェーダーを準備します。

1.  現在のレイアウトとしてシェーダー入力レイアウトを設定します。
2.  現在のシェーダーとして頂点シェーダーとピクセル シェーダーを設定します。
3.  シェーダーに渡す必要があるデータで定数バッファーを更新します。

> [!IMPORTANT]
> Marble Maze uses one pair of vertex and pixel shaders for all 3D objects. ゲームでシェーダーのペアを複数使う場合は、別のシェーダーを使うオブジェクトを描画するたびに、これらの手順を実行する必要があります。 シェーダーの状態の変更に伴うオーバーヘッドを減らすには、同じシェーダーを使うすべてのオブジェクトごとにレンダー呼び出しをグループ化することをお勧めします。

 

このドキュメントの「[シェーダーの読み込み](#loading-shaders)」では、頂点シェーダーが作成されるときに入力レイアウトがどのように作成されるかについて説明しています。 The following example shows how the **MarbleMazeMain::Render** method uses the [ID3D11DeviceContext::IASetInputLayout](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetinputlayout) method to set this layout as the current layout.

```cpp
m_deviceResources->GetD3DDeviceContext()->IASetInputLayout(m_inputLayout.Get());
```

The following example shows how the **MarbleMazeMain::Render** method uses the [ID3D11DeviceContext::VSSetShader](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-vssetshader) and [ID3D11DeviceContext::PSSetShader](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-pssetshader) methods to set the vertex and pixel shaders as the current shaders, respectively.

```cpp
// Set the vertex shader stage state.
m_deviceResources->GetD3DDeviceContext()->VSSetShader(
    m_vertexShader.Get(),   // use this vertex shader
    nullptr,                // don't use shader linkage
    0);                     // don't use shader linkage

m_deviceResources->GetD3DDeviceContext()->PSSetShader(
    m_pixelShader.Get(),    // use this pixel shader
    nullptr,                // don't use shader linkage
    0);                     // don't use shader linkage

m_deviceResources->GetD3DDeviceContext()->PSSetSamplers(
    0,                          // starting at the first sampler slot
    1,                          // set one sampler binding
    m_sampler.GetAddressOf());  // to use this sampler
```

After **MarbleMazeMain::Render** sets the shaders and their input layout, it uses the [ID3D11DeviceContext::UpdateSubresource](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-updatesubresource) method to update the constant buffer with the model, view, and projection matrices for the maze. **UpdateSubresource** メソッドは、CPU メモリから GPU メモリにマトリックス データをコピーします。 Recall that the model and view components of the **ConstantBuffer** structure are updated in the **MarbleMazeMain::Update** method. The **MarbleMazeMain::Render** method then calls the [ID3D11DeviceContext::VSSetConstantBuffers](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-vssetconstantbuffers) and [ID3D11DeviceContext::PSSetConstantBuffers](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-pssetconstantbuffers) methods to set this constant buffer as the current one.

```cpp
// Update the constant buffer with the new data.
m_deviceResources->GetD3DDeviceContext()->UpdateSubresource(
    m_constantBuffer.Get(),
    0,
    nullptr,
    &m_mazeConstantBufferData,
    0,
    0);

m_deviceResources->GetD3DDeviceContext()->VSSetConstantBuffers(
    0,                                  // starting at the first constant buffer slot
    1,                                  // set one constant buffer binding
    m_constantBuffer.GetAddressOf());   // to use this buffer

m_deviceResources->GetD3DDeviceContext()->PSSetConstantBuffers(
    0,                                  // starting at the first constant buffer slot
    1,                                  // set one constant buffer binding
    m_constantBuffer.GetAddressOf());   // to use this buffer
```

The **MarbleMazeMain::Render** method performs similar steps to prepare the marble to be rendered.

### <a name="rendering-the-maze-and-the-marble"></a>迷路と大理石のレンダリング

現在のシェーダーをアクティブにした後、シーン オブジェクトを描画できます。 The **MarbleMazeMain::Render** method calls the **SDKMesh::Render** method to render the maze mesh.

```cpp
m_mazeMesh.Render(
    m_deviceResources->GetD3DDeviceContext(), 
    0, 
    INVALID_SAMPLER_SLOT, 
    INVALID_SAMPLER_SLOT);
```

The **MarbleMazeMain::Render** method performs similar steps to render the marble.

このドキュメントで前に説明したように、**SDKMesh** クラスはデモンストレーション用に提供していますが、本番品質のゲームでの使用はお勧めしません。 However, notice that the **SDKMesh::RenderMesh** method, which is called by **SDKMesh::Render**, uses the [ID3D11DeviceContext::IASetVertexBuffers](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetvertexbuffers) and [ID3D11DeviceContext::IASetIndexBuffer](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetindexbuffer) methods to set the current vertex and index buffers that define the mesh, and the [ID3D11DeviceContext::DrawIndexed](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-drawindexedinstanced) method to draw the buffers. 頂点バッファーとインデックス バッファーの操作方法について詳しくは、「[Direct3D 11 のバッファーについて](https://docs.microsoft.com/windows/desktop/direct3d11/overviews-direct3d-11-resources-buffers-intro)」をご覧ください。

### <a name="drawing-the-user-interface-and-overlay"></a>ユーザー インターフェイスとオーバーレイの描画

After drawing 3D scene objects, Marble Maze draws the 2D UI elements that appear in front of the scene.

The **MarbleMazeMain::Render** method ends by drawing the user interface and the overlay.

```cpp
// Draw the user interface and the overlay.
UserInterface::GetInstance().Render(m_deviceResources->GetOrientationTransform2D());

m_deviceResources->GetD3DDeviceContext()->BeginEventInt(L"Render Overlay", 0);
m_sampleOverlay->Render();
m_deviceResources->GetD3DDeviceContext()->EndEvent();
```

The **UserInterface::Render** method uses an [ID2D1DeviceContext](https://docs.microsoft.com/windows/desktop/api/d2d1_1/nn-d2d1_1-id2d1devicecontext) object to draw the UI elements. このメソッドは、描画の状態を設定し、すべてのアクティブな UI 要素を描画してから、前の描画の状態を復元します。

```cpp
void UserInterface::Render(D2D1::Matrix3x2F orientation2D)
{
    m_d2dContext->SaveDrawingState(m_stateBlock.Get());
    m_d2dContext->BeginDraw();
    m_d2dContext->SetTransform(orientation2D);

    m_d2dContext->SetTextAntialiasMode(D2D1_TEXT_ANTIALIAS_MODE_GRAYSCALE);

    for (auto iter = m_elements.begin(); iter != m_elements.end(); ++iter)
    {
        if ((*iter)->IsVisible())
            (*iter)->Render();
    }

    // We ignore D2DERR_RECREATE_TARGET here. This error indicates that the device
    // is lost. It will be handled during the next call to Present.
    HRESULT hr = m_d2dContext->EndDraw();
    if (hr != D2DERR_RECREATE_TARGET)
    {
        DX::ThrowIfFailed(hr);
    }

    m_d2dContext->RestoreDrawingState(m_stateBlock.Get());
}
```

**SampleOverlay::Render** メソッドは、同様の手法を使ってオーバーレイ ビットマップを描画します。

###  <a name="presenting-the-scene"></a>シーンの表示

After drawing all 2D and 3D scene objects, Marble Maze presents the rendered image to the monitor. 描画を垂直ブランクに同期して、実際にディスプレイに表示されないフレームの描画に時間が費やされないようにします。 Marble Maze では、シーンを表示するときにデバイスの変更も処理します。

After the **MarbleMazeMain::Render** method returns, the game loop calls the **DX::DeviceResources::Present** method to send the rendered image to the monitor or display. The **DX::DeviceResources::Present** method calls [IDXGISwapChain::Present](https://docs.microsoft.com/windows/desktop/api/dxgi/nf-dxgi-idxgiswapchain-present) to perform the present operation, as shown in the following example:

```cpp
// The first argument instructs DXGI to block until VSync, putting the application
// to sleep until the next VSync. This ensures we don't waste any cycles rendering
// frames that will never be displayed to the screen.
HRESULT hr = m_swapChain->Present(1, 0);
```

In this example, **m\_swapChain** is an [IDXGISwapChain1](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgiswapchain1) object. このオブジェクトの初期化については、このドキュメントの「[Direct3D と Direct2D の初期化](#initializing-direct3d-and-direct2d)」で説明しています。

The first parameter to [IDXGISwapChain::Present](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-present1), *SyncInterval*, specifies the number of vertical blanks to wait before presenting the frame. Marble Maze では 1 を指定して、次の垂直ブランクまで待機することを指定しています。

The [IDXGISwapChain::Present](https://docs.microsoft.com/windows/desktop/api/dxgi/nf-dxgi-idxgiswapchain-present) method returns an error code that indicates that the device was removed or otherwise failed. この場合、Marble Maze は、デバイスを再初期化します。

```cpp
// If the device was removed either by a disconnection or a driver upgrade, we
// must recreate all device resources.
if (hr == DXGI_ERROR_DEVICE_REMOVED)
{
    HandleDeviceLost();
}
else
{
    DX::ThrowIfFailed(hr);
}
```

## <a name="next-steps"></a>次のステップ


入力デバイスを操作する際の重要な手順については、「[Marble Maze サンプルへの入力と対話機能の追加](adding-input-and-interactivity-to-the-marble-maze-sample.md)」をご覧ください。 This document discusses how Marble Maze supports touch, accelerometer, Xbox controllers, and mouse input.

## <a name="related-topics"></a>関連トピック


* [Adding input and interactivity to the Marble Maze sample](adding-input-and-interactivity-to-the-marble-maze-sample.md)
* [Marble Maze application structure](marble-maze-application-structure.md)
* [Developing Marble Maze, a UWP game in C++ and DirectX](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md)

 

 




