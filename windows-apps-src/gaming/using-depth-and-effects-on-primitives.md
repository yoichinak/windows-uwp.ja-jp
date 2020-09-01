---
title: プリミティブに対する深度と各種効果の使用
description: ここでは、深度、視点、色、その他の効果をプリミティブに対して使う方法について説明します。
ms.assetid: 71ef34c5-b4a3-adae-5266-f86ba257482a
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, ゲーム, 深度, 効果, プリミティブ, DirectX
ms.localizationpriority: medium
ms.openlocfilehash: 99931ef0abef10cb5c517c4c5be04e2afe3056a2
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89159016"
---
# <a name="use-depth-and-effects-on-primitives"></a>プリミティブに対する深度と各種効果の使用



ここでは、深度、視点、色、その他の効果をプリミティブに対して使う方法について説明します。

**目標:** 3D オブジェクトを作成し、基本的な頂点の照明や色付けをオブジェクトに適用する。

## <a name="prerequisites"></a>前提条件


C++ に習熟していることを前提としています。 また、グラフィックス プログラミングの概念に対する基礎的な知識も必要となります。

加えて、「[クイック スタート: DirectX リソースの設定と画像の表示](setting-up-directx-resources.md)」と「[シェーダーの作成とプリミティブの描画](creating-shaders-and-drawing-primitives.md)」にひととおり目を通しておく必要があります。

**完了までの時間:** 20 分。

<a name="instructions"></a>Instructions
------------
### <a name="1-defining-cube-variables"></a>1. 立方体変数の定義

まず、立方体の **SimpleCubeVertex** 構造体と **ConstantBuffer** 構造体を定義する必要があります。 立方体の頂点の位置と色に加え、その見え方が、これらの構造体によって指定されます。 [**Comptr**](/cpp/windows/comptr-class)で[**ID3D11DepthStencilView**](/windows/desktop/api/d3d11/nn-d3d11-id3d11depthstencilview)と[**ID3D11Buffer**](/windows/desktop/api/d3d11/nn-d3d11-id3d11buffer)を宣言し、 **ConstantBuffer**のインスタンスを宣言します。

```cpp
struct SimpleCubeVertex
{
    DirectX::XMFLOAT3 pos;   // Position
    DirectX::XMFLOAT3 color; // Color
};

struct ConstantBuffer
{
    DirectX::XMFLOAT4X4 model;
    DirectX::XMFLOAT4X4 view;
    DirectX::XMFLOAT4X4 projection;
};

// This class defines the application as a whole.
ref class Direct3DTutorialFrameworkView : public IFrameworkView
{
private:
    Platform::Agile<CoreWindow> m_window;
    ComPtr<IDXGISwapChain1> m_swapChain;
    ComPtr<ID3D11Device1> m_d3dDevice;
    ComPtr<ID3D11DeviceContext1> m_d3dDeviceContext;
    ComPtr<ID3D11RenderTargetView> m_renderTargetView;
    ComPtr<ID3D11DepthStencilView> m_depthStencilView;
    ComPtr<ID3D11Buffer> m_constantBuffer;
    ConstantBuffer m_constantBufferData;
```

### <a name="2-creating-a-depth-stencil-view"></a>2. 深度ステンシル ビューの作成

レンダー ターゲット ビューに加え、深度ステンシル ビューも作成します。 深度/ステンシル ビューによって、カメラに近いオブジェクトをカメラから遠いオブジェクトの前にレンダリングする Direct3D の処理を効率化できます。 深度ステンシル バッファーのビューを作成する前に、深度ステンシル バッファーを作成する必要があります。 深度ステンシルバッファーを記述するように [**D3D11 \_ TEXTURE2D \_ DESC**](/windows/desktop/api/d3d11/ns-d3d11-d3d11_texture2d_desc) を設定し、 [**ID3D11Device:: CreateTexture2D**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createtexture2d) を呼び出して深度ステンシルバッファーを作成します。 深度ステンシルビューを作成するには、 [**D3D11 \_ 深度ステンシルビュー \_ \_ \_ DESC**](/windows/desktop/api/d3d11/ns-d3d11-d3d11_depth_stencil_view_desc) を設定して深度ステンシルビューを記述し、深度ステンシルビューの説明と深度ステンシルバッファーを [**ID3D11Device:: CreateDepthStencilView**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createdepthstencilview)に渡します。

```cpp
        // Once the render target view is created, create a depth stencil view.  This
        // allows Direct3D to efficiently render objects closer to the camera in front
        // of objects further from the camera.

        D3D11_TEXTURE2D_DESC backBufferDesc = {0};
        backBuffer->GetDesc(&backBufferDesc);

        D3D11_TEXTURE2D_DESC depthStencilDesc;
        depthStencilDesc.Width = backBufferDesc.Width;
        depthStencilDesc.Height = backBufferDesc.Height;
        depthStencilDesc.MipLevels = 1;
        depthStencilDesc.ArraySize = 1;
        depthStencilDesc.Format = DXGI_FORMAT_D24_UNORM_S8_UINT;
        depthStencilDesc.SampleDesc.Count = 1;
        depthStencilDesc.SampleDesc.Quality = 0;
        depthStencilDesc.Usage = D3D11_USAGE_DEFAULT;
        depthStencilDesc.BindFlags = D3D11_BIND_DEPTH_STENCIL;
        depthStencilDesc.CPUAccessFlags = 0;
        depthStencilDesc.MiscFlags = 0;
        ComPtr<ID3D11Texture2D> depthStencil;
        DX::ThrowIfFailed(
            m_d3dDevice->CreateTexture2D(
                &depthStencilDesc,
                nullptr,
                &depthStencil
                )
            );

        D3D11_DEPTH_STENCIL_VIEW_DESC depthStencilViewDesc;
        depthStencilViewDesc.Format = depthStencilDesc.Format;
        depthStencilViewDesc.ViewDimension = D3D11_DSV_DIMENSION_TEXTURE2D;
        depthStencilViewDesc.Flags = 0;
        depthStencilViewDesc.Texture2D.MipSlice = 0;
        DX::ThrowIfFailed(
            m_d3dDevice->CreateDepthStencilView(
                depthStencil.Get(),
                &depthStencilViewDesc,
                &m_depthStencilView
                )
            );
```

### <a name="3-updating-perspective-with-the-window"></a>3. ウィンドウに基づく視点の更新

ウィンドウのサイズに応じて定数バッファーの透視投影パラメーターを更新します。 パラメーターは、視野が 70°、深度の範囲が 0.01 ～ 100 に修正されています。

```cpp
        // Finally, update the constant buffer perspective projection parameters
        // to account for the size of the application window.  In this sample,
        // the parameters are fixed to a 70-degree field of view, with a depth
        // range of 0.01 to 100.  For a generalized camera class, see Lesson 5.

        float xScale = 1.42814801f;
        float yScale = 1.42814801f;
        if (backBufferDesc.Width > backBufferDesc.Height)
        {
            xScale = yScale *
                static_cast<float>(backBufferDesc.Height) /
                static_cast<float>(backBufferDesc.Width);
        }
        else
        {
            yScale = xScale *
                static_cast<float>(backBufferDesc.Width) /
                static_cast<float>(backBufferDesc.Height);
        }

        m_constantBufferData.projection = DirectX::XMFLOAT4X4(
            xScale, 0.0f,    0.0f,  0.0f,
            0.0f,   yScale,  0.0f,  0.0f,
            0.0f,   0.0f,   -1.0f, -0.01f,
            0.0f,   0.0f,   -1.0f,  0.0f
            );
```

### <a name="4-creating-vertex-and-pixel-shaders-with-color-elements"></a>4. 色要素を使った頂点シェーダーとピクセル シェーダーの作成

このアプリでは、前のチュートリアル (「[シェーダーの作成とプリミティブの描画](creating-shaders-and-drawing-primitives.md)」) で説明したものよりも複雑な頂点シェーダーとピクセル シェーダーを作成します。 このアプリの頂点シェーダーは、個々の頂点の位置を投影空間に変換し、頂点の色をピクセル シェーダーに渡します。

頂点シェーダーコードのレイアウトを記述する [**D3D11 \_ INPUT \_ 要素 \_ DESC**](/windows/desktop/api/d3d11/ns-d3d11-d3d11_input_element_desc) 構造体のアプリの配列には、2つのレイアウト要素があります。1つの要素で頂点位置を定義し、もう一方の要素で色を定義します。

周回する立方体を定義する頂点バッファー、インデックス バッファー、定数バッファーを作成します。

**周回する立方体を定義するには**

1.  まず立方体を定義します。 それぞれの頂点に、位置に加えて色を割り当てます。 これによってピクセル シェーダーが各表面に異なる色を適用できるようになり、表面が区別されます。
2.  次に、キューブ定義を使用して、頂点バッファーとインデックスバッファー ([**D3D11 \_ BUFFER \_ DESC**](/windows/desktop/api/d3d11/ns-d3d11-d3d11_buffer_desc) と [**D3D11 \_ subresource \_ DATA**](/windows/desktop/api/d3d11/ns-d3d11-d3d11_subresource_data)) について説明します。 各バッファーについて、[**ID3D11Device::CreateBuffer**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createbuffer) を 1 回呼び出します。
3.  次に、モデル、ビュー、および投影マトリックスを頂点シェーダーに渡すための定数バッファー ([**D3D11 \_ buffer \_ DESC**](/windows/desktop/api/d3d11/ns-d3d11-d3d11_buffer_desc)) を作成します。 後でこの定数バッファーを使って、立方体を回転させたり、そこに透視投影を適用したりすることができます。 定数バッファーを作成するには、[**ID3D11Device::CreateBuffer**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createbuffer) を呼び出します。
4.  次に、カメラ位置 (X = 0、Y = 1、Z = 2) に対応するビュー変換を指定します。
5.  最後に、*degree* 変数を宣言します。これは、立方体をフレームごとに回転させてアニメーション化する目的で使います。

```cpp
        
        auto loadVSTask = DX::ReadDataAsync(L"SimpleVertexShader.cso");
        auto loadPSTask = DX::ReadDataAsync(L"SimplePixelShader.cso");
        
        
        auto createVSTask = loadVSTask.then([this](const std::vector<byte>& vertexShaderBytecode) {        
          ComPtr<ID3D11VertexShader> vertexShader;
          DX::ThrowIfFailed(
              m_d3dDevice->CreateVertexShader(
                  vertexShaderBytecode->Data,
                  vertexShaderBytecode->Length,
                  nullptr,
                  &vertexShader
                  )
              );

          // Create an input layout that matches the layout defined in the vertex shader code.
          // For this lesson, this is simply a DirectX::XMFLOAT3 vector defining the vertex position, and
          // a DirectX::XMFLOAT3 vector defining the vertex color.
          const D3D11_INPUT_ELEMENT_DESC basicVertexLayoutDesc[] =
          {
              { "POSITION", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0,  0, D3D11_INPUT_PER_VERTEX_DATA, 0 },
              { "COLOR",    0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 12, D3D11_INPUT_PER_VERTEX_DATA, 0 },
          };

          ComPtr<ID3D11InputLayout> inputLayout;
          DX::ThrowIfFailed(
              m_d3dDevice->CreateInputLayout(
                  basicVertexLayoutDesc,
                  ARRAYSIZE(basicVertexLayoutDesc),
                  vertexShaderBytecode->Data,
                  vertexShaderBytecode->Length,
                  &inputLayout
                  )
              );
        });
        
        
        // Load the raw pixel shader bytecode from disk and create a pixel shader with it.
        auto createPSTask = loadPSTask.then([this](const std::vector<byte>& pixelShaderBytecode) {
          ComPtr<ID3D11PixelShader> pixelShader;
          DX::ThrowIfFailed(
              m_d3dDevice->CreatePixelShader(
                  pixelShaderBytecode->Data,
                  pixelShaderBytecode->Length,
                  nullptr,
                  &pixelShader
                  )
              );
        });
        
        
        // Create vertex and index buffers that define a simple unit cube.
        auto createCubeTask = (createPSTask && createVSTask).then([this] () {

          // In the array below, which will be used to initialize the cube vertex buffers,
          // each vertex is assigned a color in addition to a position.  This will allow
          // the pixel shader to color each face differently, enabling them to be distinguished.
          SimpleCubeVertex cubeVertices[] =
          {
              { float3(-0.5f, 0.5f, -0.5f), float3(0.0f, 1.0f, 0.0f) }, // +Y (top face)
              { float3( 0.5f, 0.5f, -0.5f), float3(1.0f, 1.0f, 0.0f) },
              { float3( 0.5f, 0.5f,  0.5f), float3(1.0f, 1.0f, 1.0f) },
              { float3(-0.5f, 0.5f,  0.5f), float3(0.0f, 1.0f, 1.0f) },

              { float3(-0.5f, -0.5f,  0.5f), float3(0.0f, 0.0f, 1.0f) }, // -Y (bottom face)
              { float3( 0.5f, -0.5f,  0.5f), float3(1.0f, 0.0f, 1.0f) },
              { float3( 0.5f, -0.5f, -0.5f), float3(1.0f, 0.0f, 0.0f) },
              { float3(-0.5f, -0.5f, -0.5f), float3(0.0f, 0.0f, 0.0f) },
          };

          unsigned short cubeIndices[] =
          {
              0, 1, 2,
              0, 2, 3,

              4, 5, 6,
              4, 6, 7,

              3, 2, 5,
              3, 5, 4,

              2, 1, 6,
              2, 6, 5,

              1, 7, 6,
              1, 0, 7,

              0, 3, 4,
              0, 4, 7
          };

          D3D11_BUFFER_DESC vertexBufferDesc = {0};
          vertexBufferDesc.ByteWidth = sizeof(SimpleCubeVertex) * ARRAYSIZE(cubeVertices);
          vertexBufferDesc.Usage = D3D11_USAGE_DEFAULT;
          vertexBufferDesc.BindFlags = D3D11_BIND_VERTEX_BUFFER;
          vertexBufferDesc.CPUAccessFlags = 0;
          vertexBufferDesc.MiscFlags = 0;
          vertexBufferDesc.StructureByteStride = 0;

          D3D11_SUBRESOURCE_DATA vertexBufferData;
          vertexBufferData.pSysMem = cubeVertices;
          vertexBufferData.SysMemPitch = 0;
          vertexBufferData.SysMemSlicePitch = 0;

          ComPtr<ID3D11Buffer> vertexBuffer;
          DX::ThrowIfFailed(
              m_d3dDevice->CreateBuffer(
                  &vertexBufferDesc,
                  &vertexBufferData,
                  &vertexBuffer
                  )
              );

          D3D11_BUFFER_DESC indexBufferDesc;
          indexBufferDesc.ByteWidth = sizeof(unsigned short) * ARRAYSIZE(cubeIndices);
          indexBufferDesc.Usage = D3D11_USAGE_DEFAULT;
          indexBufferDesc.BindFlags = D3D11_BIND_INDEX_BUFFER;
          indexBufferDesc.CPUAccessFlags = 0;
          indexBufferDesc.MiscFlags = 0;
          indexBufferDesc.StructureByteStride = 0;

          D3D11_SUBRESOURCE_DATA indexBufferData;
          indexBufferData.pSysMem = cubeIndices;
          indexBufferData.SysMemPitch = 0;
          indexBufferData.SysMemSlicePitch = 0;

          ComPtr<ID3D11Buffer> indexBuffer;
          DX::ThrowIfFailed(
              m_d3dDevice->CreateBuffer(
                  &indexBufferDesc,
                  &indexBufferData,
                  &indexBuffer
                  )
              );


          // Create a constant buffer for passing model, view, and projection matrices
          // to the vertex shader.  This will allow us to rotate the cube and apply
          // a perspective projection to it.

          D3D11_BUFFER_DESC constantBufferDesc = {0};
          constantBufferDesc.ByteWidth = sizeof(m_constantBufferData);
          constantBufferDesc.Usage = D3D11_USAGE_DEFAULT;
          constantBufferDesc.BindFlags = D3D11_BIND_CONSTANT_BUFFER;
          constantBufferDesc.CPUAccessFlags = 0;
          constantBufferDesc.MiscFlags = 0;
          constantBufferDesc.StructureByteStride = 0;
          DX::ThrowIfFailed(
              m_d3dDevice->CreateBuffer(
                  &constantBufferDesc,
                  nullptr,
                  &m_constantBuffer
                  )
              );

          // Specify the view transform corresponding to a camera position of
          // X = 0, Y = 1, Z = 2.  For a generalized camera class, see Lesson 5.

          m_constantBufferData.view = DirectX::XMFLOAT4X4(
              -1.00000000f, 0.00000000f,  0.00000000f,  0.00000000f,
               0.00000000f, 0.89442718f,  0.44721359f,  0.00000000f,
               0.00000000f, 0.44721359f, -0.89442718f, -2.23606800f,
               0.00000000f, 0.00000000f,  0.00000000f,  1.00000000f
              );

        });
        
        // This value will be used to animate the cube by rotating it every frame.
        float degree = 0.0f;
        
```

### <a name="5-rotating-and-drawing-the-cube-and-presenting-the-rendered-image"></a>5. 立方体の回転と描画およびレンダリングされた画像の表示

シーンをレンダリングして表示し続けるために、無限ループを使います。 立方体のモデル マトリックスを Y 軸を中心に回転させるための値を設定するため、**rotationY** インライン関数 (BasicMath.h) に回転量を指定して呼び出します。 さらに、[**ID3D11DeviceContext::UpdateSubresource**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-updatesubresource) を呼び出して定数バッファーを更新し、立方体モデルを回転させます。 [**ID3D11DeviceContext::OMSetRenderTargets**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-omsetrendertargets) を呼び出して、レンダー ターゲットを出力ターゲットとして指定します。 この **OMSetRenderTargets** 呼び出しでは、深度ステンシル ビューを渡します。 [**ID3D11DeviceContext::ClearRenderTargetView**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-clearrendertargetview) を呼び出してレンダー ターゲットを無地の青色にクリアし、[**ID3D11DeviceContext::ClearDepthStencilView**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-cleardepthstencilview) を呼び出して深度バッファーをクリアします。

無限ループで、立方体を青色のサーフェス上に描画します。

**立方体を描画するには**

1.  まず、頂点バッファーから入力アセンブラー ステージへのデータの流れを定義するために、[**ID3D11DeviceContext::IASetInputLayout**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetinputlayout) を呼び出します。
2.  次に、[**ID3D11DeviceContext::IASetVertexBuffers**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetvertexbuffers) と [**ID3D11DeviceContext::IASetIndexBuffer**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetindexbuffer) を呼び出して、頂点バッファーとインデックス バッファーを入力アセンブラー ステージにバインドします。
3.  次に、 [**D3D11 \_ プリミティブ \_ トポロジ \_ TRIANGLESTRIP**](/previous-versions/windows/desktop/legacy/ff476189(v=vs.85))値を指定して[**である id3d11devicecontext:: IASetPrimitiveTopology**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetprimitivetopology)を呼び出し、頂点データを三角形ストリップとして解釈するように入力アセンブラーステージを指定します。
4.  次に、[**ID3D11DeviceContext::VSSetShader**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-vssetshader) を呼び出して頂点シェーダー ステージを頂点シェーダー コードで初期化し、さらに、[**ID3D11DeviceContext::PSSetShader**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-pssetshader) を呼び出してピクセル シェーダー ステージをピクセル シェーダー コードで初期化します。
5.  次に、[**ID3D11DeviceContext::VSSetConstantBuffers**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-vssetconstantbuffers) を呼び出し、頂点シェーダーのパイプライン ステージで使われる定数バッファーを設定します。
6.  最後に、[**ID3D11DeviceContext::DrawIndexed**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-drawindexed) を呼び出して立方体を描画し、レンダリング パイプラインに送ります。

レンダリングされた画像をウィンドウに表示するために、[**IDXGISwapChain::Present**](/windows/desktop/api/dxgi/nf-dxgi-idxgiswapchain-present) を呼び出しています。

```cpp
            // Update the constant buffer to rotate the cube model.
            m_constantBufferData.model = XMMatrixRotationY(-degree);
            degree += 1.0f;

            m_d3dDeviceContext->UpdateSubresource(
                m_constantBuffer.Get(),
                0,
                nullptr,
                &m_constantBufferData,
                0,
                0
                );

            // Specify the render target and depth stencil we created as the output target.
            m_d3dDeviceContext->OMSetRenderTargets(
                1,
                m_renderTargetView.GetAddressOf(),
                m_depthStencilView.Get()
                );

            // Clear the render target to a solid color, and reset the depth stencil.
            const float clearColor[4] = { 0.071f, 0.04f, 0.561f, 1.0f };
            m_d3dDeviceContext->ClearRenderTargetView(
                m_renderTargetView.Get(),
                clearColor
                );

            m_d3dDeviceContext->ClearDepthStencilView(
                m_depthStencilView.Get(),
                D3D11_CLEAR_DEPTH,
                1.0f,
                0
                );

            m_d3dDeviceContext->IASetInputLayout(inputLayout.Get());

            // Set the vertex and index buffers, and specify the way they define geometry.
            UINT stride = sizeof(SimpleCubeVertex);
            UINT offset = 0;
            m_d3dDeviceContext->IASetVertexBuffers(
                0,
                1,
                vertexBuffer.GetAddressOf(),
                &stride,
                &offset
                );

            m_d3dDeviceContext->IASetIndexBuffer(
                indexBuffer.Get(),
                DXGI_FORMAT_R16_UINT,
                0
                );

            m_d3dDeviceContext->IASetPrimitiveTopology(D3D11_PRIMITIVE_TOPOLOGY_TRIANGLELIST);

            // Set the vertex and pixel shader stage state.
            m_d3dDeviceContext->VSSetShader(
                vertexShader.Get(),
                nullptr,
                0
                );

            m_d3dDeviceContext->VSSetConstantBuffers(
                0,
                1,
                m_constantBuffer.GetAddressOf()
                );

            m_d3dDeviceContext->PSSetShader(
                pixelShader.Get(),
                nullptr,
                0
                );

            // Draw the cube.
            m_d3dDeviceContext->DrawIndexed(
                ARRAYSIZE(cubeIndices),
                0,
                0
                );

            // Present the rendered image to the window.  Because the maximum frame latency is set to 1,
            // the render loop will generally be throttled to the screen refresh rate, typically around
            // 60 Hz, by sleeping the application on Present until the screen is refreshed.
            DX::ThrowIfFailed(
                m_swapChain->Present(1, 0)
                );
```

## <a name="summary-and-next-steps"></a>まとめと次のステップ


深度、視点、色、その他の効果をプリミティブに対して使いました。

次は、プリミティブにテクスチャを適用します。

[プリミティブへのテクスチャの適用](applying-textures-to-primitives.md)

 

 