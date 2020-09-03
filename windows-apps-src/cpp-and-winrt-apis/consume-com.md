---
description: このトピックでは、完全な Direct2D コードの例を使用し、C++/WinRT を使って COM クラスとインターフェイスを利用する方法を示します。
title: C++/WinRT での COM コンポーネントの使用
ms.date: 04/24/2019
ms.topic: article
keywords: windows 10、uwp、標準、c ++、cpp、winrt、COM、コンポーネント、クラス、インターフェイス
ms.localizationpriority: medium
ms.openlocfilehash: 6ccd196b2cd571cc66523b34427ca17acd73388a
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89170346"
---
# <a name="consume-com-components-with-cwinrt"></a>C++/WinRT での COM コンポーネントの使用

[C++/WinRT](./intro-to-using-cpp-with-winrt.md) ライブラリの機能を使用して、DirectX API のパフォーマンスの高い 2-D および 3-D グラフィックなどの COM コンポーネントを使用できます。 C++/WinRT は、パフォーマンスを犠牲にすることなく DirectX を使用する最も簡単な方法です。 このトピックでは、Direct2D コードの例を使用し、C++/WinRT を使って COM クラスとインターフェイスを利用する方法を示します。 当然ながら、同じ C++/WinRT プロジェクト内で COM と Windows ランタイム プログラミングを混在させることもできます。

このトピックの末尾には、最小限の Direct2D アプリケーションの完全なソース コード一覧が掲載されています。 ここでは、そのコードの抜粋を取り上げ、それを使い、C++/WinRT ライブラリのさまざまな機能を使用し、C++/WinRT を使って COM コンポーネントを利用する方法を説明します。

## <a name="com-smart-pointers-winrtcom_ptr"></a>COM スマート ポインター ([**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr))

COM を使ってプログラミングする場合は、オブジェクトではなくインターフェイスを直接使って作業します (これは、COM が進化したものである Windows ランタイム API のバックグラウンドにも当てはまります)。 たとえば、COM クラスに対して関数を呼び出すには、そのクラスをアクティブ化し、インターフェイスを取得してから、そのインターフェイスに対して関数を呼び出します。 オブジェクトの状態にアクセスするには、そのデータ メンバーに直接アクセスしないでください。代わりに、インターフェイスに対してアクセサー関数とミューテーター関数を呼び出します。

より具体的に、ここではインターフェイス "*ポインター*" との対話について説明します。 そのために、C++/WinRT に COM スマート ポインター型 ([**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr) 型) が存在することを利用します。

```cppwinrt
#include <d2d1_1.h>
...
winrt::com_ptr<ID2D1Factory1> factory;
```

上記のコードは、[**ID2D1Factory1**](/windows/desktop/api/d2d1_1/nn-d2d1_1-id2d1factory1) COM インターフェイスに対する初期化されていないスマート ポインターを宣言する方法を示しています。 スマート ポインターは初期化されていないので、実際のオブジェクトに属する **ID2D1Factory1** インターフェイスをまだ指していません (まったくインターフェイスを指していません)。 ただし、そうすることはできます。また、(スマート ポインターなので) COM 参照カウントを介して、それが指すインターフェイスの所有オブジェクトの有効期間を管理し、そのインターフェイスに対して関数を呼び出すための媒体となることができます。

## <a name="com-functions-that-return-an-interface-pointer-as-void"></a>インターフェイス ポインターを **void** として返す COM 関数

[**com_ptr::put_void**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrput_void-function) 関数を呼び出して、初期化されていないスマート ポインターの基になる生ポインターに書き込むことができます。

```cppwinrt
D2D1_FACTORY_OPTIONS options{ D2D1_DEBUG_LEVEL_NONE };
D2D1CreateFactory(
    D2D1_FACTORY_TYPE_SINGLE_THREADED,
    __uuidof(factory),
    &options,
    factory.put_void()
);
```

上記のコードでは [**D2D1CreateFactory**](/windows/desktop/api/d2d1/nf-d2d1-d2d1createfactory) 関数を呼び出すと、最後のパラメーターを介して **ID2D1Factory1** インターフェイス ポインターが返されます。これは **void\*\*** 型です。 多くの COM 関数からは **void\*\*** が返されます。 このような関数の場合は、ご覧のように [**com_ptr::put_void**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrput_void-function) を使います。

## <a name="com-functions-that-return-a-specific-interface-pointer"></a>特定のインターフェイス ポインターを返す COM 関数

[**D3D11CreateDevice**](/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice) 関数からは、最後から 3 番目のパラメーター (**ID3D11Device\*\*** 型) を介して [**ID3D11Device**](/windows/desktop/api/d3d11/nn-d3d11-id3d11device) インターフェイス ポインターが返されます。 そのような特定のインターフェイス ポインターを返す関数には、[**com_ptr::put**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptr_put-function) を使用します。

```cppwinrt
winrt::com_ptr<ID3D11Device> device;
D3D11CreateDevice(
    ...
    device.put(),
    ...);
```

前のセクションのコード例は、生の **D2D1CreateFactory** 関数を呼び出す方法を示しています。 ただし実際には、このトピックのコード例で **D2D1CreateFactory** を呼び出すと、生の API をラップするヘルパー関数テンプレートが使用されるため、このコード例には実際には [**com_ptr::put**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptr_put-function) が使用されます。

```cppwinrt
winrt::com_ptr<ID2D1Factory1> factory;
D2D1CreateFactory(
    D2D1_FACTORY_TYPE_SINGLE_THREADED,
    options,
    factory.put());
```

## <a name="com-functions-that-return-an-interface-pointer-as-iunknown"></a>インターフェイス ポインターを **IUnknown** として返す COM 関数

[**DWriteCreateFactory**](/windows/desktop/api/dwrite/nf-dwrite-dwritecreatefactory) 関数からは、[**IUnknown**](/windows/desktop/api/unknwn/nn-unknwn-iunknown) 型の最後のパラメーターを介して DirectWrite ファクトリ インターフェイス ポインターが返されます。 そのような関数には [**com_ptr::put**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptr_put-function) を使用しますが、それを **IUnknown** に再解釈してキャストします。

```cppwinrt
DWriteCreateFactory(
    DWRITE_FACTORY_TYPE_SHARED,
    __uuidof(dwriteFactory2),
    reinterpret_cast<IUnknown**>(dwriteFactory2.put()));
```

## <a name="re-seat-a-winrtcom_ptr"></a>**winrt::com_ptr** を再設定する

> [!IMPORTANT]
> [**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr) が既に設定されていて (内部の生のポインターが既にターゲットを持っていて)、別のオブジェクトを指すように再設定する場合は、次のコード例に示すように、最初に `nullptr` をそれに割り当てる必要があります。 そうしない場合、([**com_ptr::put**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptr_put-function) または [**com_ptr::put_void**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrput_void-function) を呼び出すときに) 既に設定されている **com_ptr** によって内部ポインターが null ではないとアサートされるため、問題に気づきます。

```cppwinrt
winrt::com_ptr<ID2D1SolidColorBrush> brush;
...
    brush.put()
...
brush = nullptr; // Important because we're about to re-seat
target->CreateSolidColorBrush(
    color_orange,
    D2D1::BrushProperties(0.8f),
    brush.put()));
```

## <a name="handle-hresult-error-codes"></a>HRESULT エラー コードを処理する

COM 関数から返された HRESULT の値を確認し、それがエラー コードを示す場合に例外をスローするには、[**winrt::check_hresult**](/uwp/cpp-ref-for-winrt/error-handling/check-hresult) を呼び出します。

```cppwinrt
winrt::check_hresult(D2D1CreateFactory(
    D2D1_FACTORY_TYPE_SINGLE_THREADED,
    __uuidof(factory),
    options,
    factory.put_void()));
```

## <a name="com-functions-that-take-a-specific-interface-pointer"></a>特定のインターフェイス ポインターを受け取る COM 関数

[**com_ptr::get**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrget-function) 関数を呼び出して、**com_ptr** を同じ型の特定のインターフェイス ポインターを受け取る関数に渡すことができます。

```cppwinrt
... ExampleFunction(
    winrt::com_ptr<ID2D1Factory1> const& factory,
    winrt::com_ptr<IDXGIDevice> const& dxdevice)
{
    ...
    winrt::check_hresult(factory->CreateDevice(dxdevice.get(), ...));
    ...
}
```

## <a name="com-functions-that-take-an-iunknown-interface-pointer"></a>**IUnknown** インターフェイス ポインターを受け取る COM 関数

[**com_ptr::get**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrget-function) を使用して、**com_ptr** を **IUnknown** インターフェイス ポインターを受け取る関数に渡すことができます。

[**winrt::get_unknown**](/uwp/cpp-ref-for-winrt/get-unknown) 関数を使用して、プロジェクションされた型のオブジェクトの基になる生の [IUnknown インターフェイス](/windows/win32/api/unknwn/nn-unknwn-iunknown)のアドレス (つまり、それに対するポインター) を返すことができます。 その後、そのアドレスを、**IUnknown** インターフェイス ポインターを受け取る関数に渡すことができます。

"*プロジェクションされた型*" については、「[C++/WinRT での API の使用](./consume-apis.md)」をご覧ください。

**get_unknown** のコード例については、[**winrt::get_unknown**](/uwp/cpp-ref-for-winrt/get-unknown) に関するページか、このトピックの「[最小限の Direct2D アプリケーションの完全なソース コード一覧](#full-source-code-listing-of-a-minimal-direct2d-application)」をご覧ください。

## <a name="passing-and-returning-com-smart-pointers"></a>COM スマート ポインターの受け渡し

**winrt::com_ptr** の形式で COM スマート ポインターを受け取る関数は、定数の参照または参照によって行う必要があります。

```cppwinrt
... GetDxgiFactory(winrt::com_ptr<ID3D11Device> const& device) ...

... CreateDevice(..., winrt::com_ptr<ID3D11Device>& device) ...
```

**winrt::com_ptr** を返す関数は、それを値によって行う必要があります。

```cppwinrt
winrt::com_ptr<ID2D1Factory1> CreateFactory() ...
```

## <a name="query-a-com-smart-pointer-for-a-different-interface"></a>別のインターフェイスの COM スマート ポインターのクエリ

[**com_ptr::as**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptras-function) 関数を使用して、COM スマート ポインターに別のインターフェイスのクエリを実行することができます。 クエリが成功しなかった場合、関数から例外がスローされます。

```cppwinrt
void ExampleFunction(winrt::com_ptr<ID3D11Device> const& device)
{
    ...
    winrt::com_ptr<IDXGIDevice> const dxdevice{ device.as<IDXGIDevice>() };
    ...
}
```

または、[**com_ptr::try_as**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrtry_as-function) を使用します。これで、`nullptr` と照合してクエリが成功したかどうかを確認できる値が返されます。

## <a name="full-source-code-listing-of-a-minimal-direct2d-application"></a>最小限の Direct2D アプリケーションの完全なソース コード一覧

> [!NOTE]
> &mdash;C++/WinRT Visual Studio Extension (VSIX) と NuGet パッケージ (両者が連携してプロジェクト テンプレートとビルドをサポート) のインストールと使用など、&mdash;C++/WinRT 開発用に Visual Studio を設定する方法については、[Visual Studio での C++/WinRT のサポート](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)に関する記事を参照してください。

このソースコードの例をビルドして実行する場合は、まず、最新バージョンの C++/WinRT Visual Studio 拡張機能 (VSIX) をインストール (または更新) します。上記の注を参照してください。 次に、Visual Studio で、新しい**コア アプリ (C++/WinRT)** を作成します。 `Direct2D` はプロジェクトに適した名前ですが、任意の名前を付けることができます。 Windows SDK の最新の一般公開された (プレビュー以外の) バージョンを対象とします。

### <a name="step-1-edit-pchh"></a>手順 1. `pch.h` を編集します

`pch.h` を開き、`windows.h` のインクルードの直後に `#include <unknwn.h>` を追加します。 これは、[**winrt:: get_unknown**](/uwp/cpp-ref-for-winrt/get-unknown) を使用しているためです。 **winrt::get_unknown** を使用するときは常に、そのヘッダーが別のヘッダーにインクルードされている場合でも、明示的に `#include <unknwn.h>` することをお勧めします。

> [!NOTE]
> この手順を省略すると、ビルドエラー " *'get_unknown': 識別子が見つかりません*" が表示されます。

### <a name="step-2-edit-appcpp"></a>手順 2. `App.cpp` を編集します

`App.cpp` を開き、内容全体を削除し、以下の一覧に貼り付けます。

次のコードでは、可能な場合は [winrt::com_ptr::capture 関数](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrcapture-function)を使用しています。 `WINRT_ASSERT` はマクロ定義であり、[_ASSERTE](/cpp/c-runtime-library/reference/assert-asserte-assert-expr-macros) に展開されます。

```cppwinrt
#include "pch.h"
#include <d2d1_1.h>
#include <d3d11.h>
#include <dxgi1_2.h>
#include <winrt/Windows.Graphics.Display.h>

using namespace winrt;

using namespace Windows;
using namespace Windows::ApplicationModel::Core;
using namespace Windows::UI;
using namespace Windows::UI::Core;
using namespace Windows::Graphics::Display;

namespace
{
    winrt::com_ptr<ID2D1Factory1> CreateFactory()
    {
        D2D1_FACTORY_OPTIONS options{};

#ifdef _DEBUG
        options.debugLevel = D2D1_DEBUG_LEVEL_INFORMATION;
#endif

        winrt::com_ptr<ID2D1Factory1> factory;

        winrt::check_hresult(D2D1CreateFactory(
            D2D1_FACTORY_TYPE_SINGLE_THREADED,
            options,
            factory.put()));

        return factory;
    }

    HRESULT CreateDevice(D3D_DRIVER_TYPE const type, winrt::com_ptr<ID3D11Device>& device)
    {
        WINRT_ASSERT(!device);

        return D3D11CreateDevice(
            nullptr,
            type,
            nullptr,
            D3D11_CREATE_DEVICE_BGRA_SUPPORT,
            nullptr, 0,
            D3D11_SDK_VERSION,
            device.put(),
            nullptr,
            nullptr);
    }

    winrt::com_ptr<ID3D11Device> CreateDevice()
    {
        winrt::com_ptr<ID3D11Device> device;
        HRESULT hr{ CreateDevice(D3D_DRIVER_TYPE_HARDWARE, device) };

        if (DXGI_ERROR_UNSUPPORTED == hr)
        {
            hr = CreateDevice(D3D_DRIVER_TYPE_WARP, device);
        }

        winrt::check_hresult(hr);
        return device;
    }

    winrt::com_ptr<ID2D1DeviceContext> CreateRenderTarget(
        winrt::com_ptr<ID2D1Factory1> const& factory,
        winrt::com_ptr<ID3D11Device> const& device)
    {
        WINRT_ASSERT(factory);
        WINRT_ASSERT(device);

        winrt::com_ptr<IDXGIDevice> const dxdevice{ device.as<IDXGIDevice>() };

        winrt::com_ptr<ID2D1Device> d2device;
        winrt::check_hresult(factory->CreateDevice(dxdevice.get(), d2device.put()));

        winrt::com_ptr<ID2D1DeviceContext> target;
        winrt::check_hresult(d2device->CreateDeviceContext(D2D1_DEVICE_CONTEXT_OPTIONS_NONE, target.put()));
        return target;
    }

    winrt::com_ptr<IDXGIFactory2> GetDxgiFactory(winrt::com_ptr<ID3D11Device> const& device)
    {
        WINRT_ASSERT(device);

        winrt::com_ptr<IDXGIDevice> const dxdevice{ device.as<IDXGIDevice>() };

        winrt::com_ptr<IDXGIAdapter> adapter;
        winrt::check_hresult(dxdevice->GetAdapter(adapter.put()));

        winrt::com_ptr<IDXGIFactory2> factory;
        factory.capture(adapter, &IDXGIAdapter::GetParent);
        return factory;
    }

    void CreateDeviceSwapChainBitmap(
        winrt::com_ptr<IDXGISwapChain1> const& swapchain,
        winrt::com_ptr<ID2D1DeviceContext> const& target)
    {
        WINRT_ASSERT(swapchain);
        WINRT_ASSERT(target);

        winrt::com_ptr<IDXGISurface> surface;
        surface.capture(swapchain, &IDXGISwapChain1::GetBuffer, 0);

        D2D1_BITMAP_PROPERTIES1 const props{ D2D1::BitmapProperties1(
            D2D1_BITMAP_OPTIONS_TARGET | D2D1_BITMAP_OPTIONS_CANNOT_DRAW,
            D2D1::PixelFormat(DXGI_FORMAT_B8G8R8A8_UNORM, D2D1_ALPHA_MODE_IGNORE)) };

        winrt::com_ptr<ID2D1Bitmap1> bitmap;

        winrt::check_hresult(target->CreateBitmapFromDxgiSurface(surface.get(),
            props,
            bitmap.put()));

        target->SetTarget(bitmap.get());
    }

    winrt::com_ptr<IDXGISwapChain1> CreateSwapChainForCoreWindow(winrt::com_ptr<ID3D11Device> const& device)
    {
        WINRT_ASSERT(device);

        winrt::com_ptr<IDXGIFactory2> const factory{ GetDxgiFactory(device) };

        DXGI_SWAP_CHAIN_DESC1 props{};
        props.Format = DXGI_FORMAT_B8G8R8A8_UNORM;
        props.SampleDesc.Count = 1;
        props.BufferUsage = DXGI_USAGE_RENDER_TARGET_OUTPUT;
        props.BufferCount = 2;
        props.SwapEffect = DXGI_SWAP_EFFECT_FLIP_SEQUENTIAL;

        winrt::com_ptr<IDXGISwapChain1> swapChain;

        winrt::check_hresult(factory->CreateSwapChainForCoreWindow(
            device.get(),
            winrt::get_unknown(CoreWindow::GetForCurrentThread()),
            &props,
            nullptr, // all or nothing
            swapChain.put()));

        return swapChain;
    }

    constexpr D2D1_COLOR_F color_white{ 1.0f,  1.0f,  1.0f,  1.0f };
    constexpr D2D1_COLOR_F color_orange{ 0.92f,  0.38f,  0.208f,  1.0f };
}

struct App : implements<App, IFrameworkViewSource, IFrameworkView>
{
    winrt::com_ptr<ID2D1Factory1> m_factory;
    winrt::com_ptr<ID2D1DeviceContext> m_target;
    winrt::com_ptr<IDXGISwapChain1> m_swapChain;
    winrt::com_ptr<ID2D1SolidColorBrush> m_brush;
    float m_dpi{};

    IFrameworkView CreateView()
    {
        return *this;
    }

    void Initialize(CoreApplicationView const&)
    {
    }

    void Load(hstring const&)
    {
        CoreWindow const window{ CoreWindow::GetForCurrentThread() };

        window.SizeChanged([&](auto&&...)
        {
            if (m_target)
            {
                ResizeSwapChainBitmap();
                Render();
            }
        });

        DisplayInformation const display{ DisplayInformation::GetForCurrentView() };
        m_dpi = display.LogicalDpi();

        display.DpiChanged([&](DisplayInformation const& display, IInspectable const&)
        {
            if (m_target)
            {
                m_dpi = display.LogicalDpi();
                m_target->SetDpi(m_dpi, m_dpi);
                CreateDeviceSizeResources();
                Render();
            }
        });

        m_factory = CreateFactory();
        CreateDeviceIndependentResources();
    }

    void Uninitialize()
    {
    }

    void Run()
    {
        CoreWindow const window{ CoreWindow::GetForCurrentThread() };
        window.Activate();

        Render();
        CoreDispatcher const dispatcher{ window.Dispatcher() };
        dispatcher.ProcessEvents(CoreProcessEventsOption::ProcessUntilQuit);
    }

    void SetWindow(CoreWindow const&) {}

    void Draw()
    {
        m_target->Clear(color_white);

        D2D1_SIZE_F const size{ m_target->GetSize() };
        D2D1_RECT_F const rect{ 100.0f, 100.0f, size.width - 100.0f, size.height - 100.0f };
        m_target->DrawRectangle(rect, m_brush.get(), 100.0f);

        char buffer[1024];
        (void)snprintf(buffer, sizeof(buffer), "Draw %.2f x %.2f @ %.2f\n", size.width, size.height, m_dpi);
        ::OutputDebugStringA(buffer);
    }

    void Render()
    {
        if (!m_target)
        {
            winrt::com_ptr<ID3D11Device> const device{ CreateDevice() };
            m_target = CreateRenderTarget(m_factory, device);
            m_swapChain = CreateSwapChainForCoreWindow(device);

            CreateDeviceSwapChainBitmap(m_swapChain, m_target);

            m_target->SetDpi(m_dpi, m_dpi);

            CreateDeviceResources();
            CreateDeviceSizeResources();
        }

        m_target->BeginDraw();
        Draw();
        m_target->EndDraw();

        HRESULT const hr{ m_swapChain->Present(1, 0) };

        if (S_OK != hr && DXGI_STATUS_OCCLUDED != hr)
        {
            ReleaseDevice();
        }
    }

    void ReleaseDevice()
    {
        m_target = nullptr;
        m_swapChain = nullptr;

        ReleaseDeviceResources();
    }

    void ResizeSwapChainBitmap()
    {
        WINRT_ASSERT(m_target);
        WINRT_ASSERT(m_swapChain);

        m_target->SetTarget(nullptr);

        if (S_OK == m_swapChain->ResizeBuffers(0, // all buffers
            0, 0, // client area
            DXGI_FORMAT_UNKNOWN, // preserve format
            0)) // flags
        {
            CreateDeviceSwapChainBitmap(m_swapChain, m_target);
            CreateDeviceSizeResources();
        }
        else
        {
            ReleaseDevice();
        }
    }

    void CreateDeviceIndependentResources()
    {
    }

    void CreateDeviceResources()
    {
        winrt::check_hresult(m_target->CreateSolidColorBrush(
            color_orange,
            D2D1::BrushProperties(0.8f),
            m_brush.put()));
    }

    void CreateDeviceSizeResources()
    {
    }

    void ReleaseDeviceResources()
    {
        m_brush = nullptr;
    }
};

int __stdcall wWinMain(HINSTANCE, HINSTANCE, PWSTR, int)
{
    CoreApplication::Run(winrt::make<App>());
}
```

## <a name="working-with-com-types-such-as-bstr-and-variant"></a>BSTR や VARIANT などの COM 型を使用する

ご覧のとおり、C++/WinRT は COM インターフェイスの実装と呼び出しの両方をサポートしています。 BSTR や VARIANT などの COM の種類を使用する場合は、(リソースの有効期間を管理する) **wil::unique_bstr** や **wil::unique_variant** など、[Windows 実装ライブラリ (WIL)](https://github.com/Microsoft/wil) で提供されるラッパーを使用することをお勧めします。

[WIL](https://github.com/Microsoft/wil) は、Active Template Library (ATL) や Visual C++ コンパイラの COM サポートなどのフレームワークに代わるものです。 また、独自のラッパーを作成することや、生の形式で (適切な API と共に) BSTR や VARIANT などの COM の種類を使用することをお勧めします。

## <a name="avoiding-namespace-collisions"></a>名前空間の競合を回避する

このトピックのコードで示されているように、using ディレクティブを大量に使うのは C++/WinRT ではよくあることです。 ただし、場合によっては、それにより、競合している名前がグローバル名前空間にインポートされる問題が発生する可能性があります。 次に例を示します。

C++/WinRT には [**winrt::Windows::Foundation::IUnknown**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown) という名前の型が含まれるのに対し、COM では [ **::IUnknown**](/windows/desktop/api/unknwn/nn-unknwn-iunknown) という名前の型が定義されています。 そこで、COM ヘッダーを使用する C++/WinRT プロジェクトの次のようなコードについて考えます。

```cppwinrt
using namespace winrt::Windows::Foundation;
...
void MyFunction(IUnknown*); // error C2872:  'IUnknown': ambiguous symbol
```

修飾されていない名前 *IUnknown* はグローバル名前空間内で競合するため、"*あいまいなシンボル*" のコンパイラ エラーが発生します。 代わりに、次のように、C++/WinRT バージョンの名前を、**winrt** 名前空間に分離することができます。

```cppwinrt
namespace winrt
{
    using namespace Windows::Foundation;
}
...
void MyFunctionA(IUnknown*); // Ok.
void MyFunctionB(winrt::IUnknown const&); // Ok.
```

または、`using namespace winrt` が便利な場合は、そのようにできます。 次のように、*IUnknown* のグローバル バージョンだけを修飾する必要があります。

```cppwinrt
using namespace winrt;
namespace winrt
{
    using namespace Windows::Foundation;
}
...
void MyFunctionA(::IUnknown*); // Ok.
void MyFunctionB(winrt::IUnknown const&); // Ok.
```

当然ながら、これは C++/WinRT のどの名前空間でも動作します。

```cppwinrt
namespace winrt
{
    using namespace Windows::Storage;
    using namespace Windows::System;
}
```

その場合、**winrt::Windows::Storage::StorageFile** は、たとえば **winrt::StorageFile** だけで参照できます。

## <a name="important-apis"></a>重要な API
* [winrt::check_hresult 関数](/uwp/cpp-ref-for-winrt/error-handling/check-hresult)
* [winrt::com_ptr 構造体テンプレート](/uwp/cpp-ref-for-winrt/com-ptr)
* [winrt::Windows::Foundation::IUnknown 構造体](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown)