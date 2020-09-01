---
title: シェーダーの作成とプリミティブの描画
description: ここでは、HLSL ソース ファイルを使い、シェーダーをコンパイルして作成する方法について説明します。作成したシェーダーを使って、ディスプレイ上にプリミティブを描画することができます。
ms.assetid: 91113bbe-96c9-4ef9-6482-39f1ff1a70f4
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10、UWP、ゲーム、シェーダー、プリミティブ、DirectX
ms.localizationpriority: medium
ms.openlocfilehash: e900767b594b195ac5e1dd1d5777cfab16c2ece4
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175366"
---
# <a name="create-shaders-and-drawing-primitives"></a>シェーダーの作成とプリミティブの描画



ここでは、HLSL ソース ファイルを使い、シェーダーをコンパイルして作成する方法について説明します。作成したシェーダーを使って、ディスプレイ上にプリミティブを描画することができます。

頂点シェーダーとピクセル シェーダーを使って、黄色の三角形を作成し描画します。 Direct3D デバイス、スワップ チェーン、レンダー ターゲット ビューを作成した後、ディスク上のバイナリ シェーダー オブジェクト ファイルからデータを読み取ります。

**目標:** シェーダーを作成し、プリミティブを描画する。

## <a name="prerequisites"></a>前提条件


C++ に習熟していることを前提としています。 また、グラフィックス プログラミングの概念に対する基礎的な知識も必要となります。

また、「[クイック スタート: DirectX リソースの設定と画像の表示](setting-up-directx-resources.md)」にひととおり目を通しておく必要があります。

**完了までの時間:** 20 分。

## <a name="instructions"></a>Instructions

### <a name="1-compiling-hlsl-source-files"></a>1. HLSL ソース ファイルのコンパイル

Microsoft Visual Studio は [fxc.exe](/windows/desktop/direct3dtools/fxc) HLSL コード コンパイラを使って .hlsl ソース ファイル (SimpleVertexShader.hlsl と SimplePixelShader.hlsl) を .cso バイナリ シェーダー オブジェクト ファイル (SimpleVertexShader.cso と SimplePixelShader.cso) にコンパイルします。 HLSL コード コンパイラについて詳しくは、「エフェクト コンパイラ ツール」をご覧ください。 シェーダー コードのコンパイルについて詳しくは、「[シェーダーのコンパイル](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-part1)」をご覧ください。

以下に示したのは、SimpleVertexShader.hlsl のコードです。

```hlsl
struct VertexShaderInput
{
    DirectX::XMFLOAT2 pos : POSITION;
};

struct PixelShaderInput
{
    float4 pos : SV_POSITION;
};

PixelShaderInput SimpleVertexShader(VertexShaderInput input)
{
    PixelShaderInput vertexShaderOutput;

    // For this lesson, set the vertex depth value to 0.5, so it is guaranteed to be drawn.
    vertexShaderOutput.pos = float4(input.pos, 0.5f, 1.0f);

    return vertexShaderOutput;
}
```

以下に示したのは、SimplePixelShader.hlsl のコードです。

```hlsl
struct PixelShaderInput
{
    float4 pos : SV_POSITION;
};

float4 SimplePixelShader(PixelShaderInput input) : SV_TARGET
{
    // Draw the entire triangle yellow.
    return float4(1.0f, 1.0f, 0.0f, 1.0f);
}
```

### <a name="2-reading-data-from-disk"></a>2. ディスクからのデータの読み取り

DirectX 11 アプリ (ユニバーサル Windows) テンプレート内の DirectXHelper.h から DX::ReadDataAsync 関数を使って、ディスク上のファイルからデータを非同期的に読み取ります。

### <a name="3-creating-vertex-and-pixel-shaders"></a>3. 頂点シェーダーとピクセル シェーダーの作成

SimpleVertexShader.cso ファイルからデータを読み取り、そのデータを *vertexShaderBytecode* バイト配列に割り当てます。 このバイト配列を使って [**ID3D11Device::CreateVertexShader**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createvertexshader) を呼び出し、頂点シェーダー ([**ID3D11VertexShader**](/windows/desktop/api/d3d11/nn-d3d11-id3d11vertexshader)) を作成します。 三角形が確実に描画されるように、SimpleVertexShader.hlsl ソースで頂点の深度値を 0.5 に設定します。 [**D3D11 \_ INPUT \_ 要素の \_ DESC**](/windows/desktop/api/d3d11/ns-d3d11-d3d11_input_element_desc)構造体の配列に頂点シェーダーコードのレイアウトを記述し、 [**ID3D11Device:: createinputlayout**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createinputlayout)を呼び出してレイアウトを作成します。 配列には頂点の位置を定義するレイアウト要素が 1 つあります。 SimplePixelShader.cso ファイルからデータを読み取り、そのデータを *pixelShaderBytecode* バイト配列に割り当てます。 このバイト配列を使って [**ID3D11Device::CreatePixelShader**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createpixelshader) を呼び出し、ピクセル シェーダー ([**ID3D11PixelShader**](/windows/desktop/api/d3d11/nn-d3d11-id3d11pixelshader)) を作成します。 三角形を黄色にするために、SimplePixelShader.hlsl ソースでピクセル値を (1,1,1,1) に設定します。 この値を変更することで色を変えることができます。

単純な三角形を定義する頂点バッファーとインデックス バッファーを作成します。 これを行うには、まず三角形を定義し、次に三角形定義を使用して頂点バッファーとインデックスバッファー ([**D3D11 \_ BUFFER \_ DESC**](/windows/desktop/api/d3d11/ns-d3d11-d3d11_buffer_desc) と [**D3D11 \_ subresource \_ DATA**](/windows/desktop/api/d3d11/ns-d3d11-d3d11_subresource_data)) について説明し、最後に、各バッファーに対して [**ID3D11Device:: createbuffer**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createbuffer) を1回呼び出します。

```cpp
        auto loadVSTask = DX::ReadDataAsync(L"SimpleVertexShader.cso");
        auto loadPSTask = DX::ReadDataAsync(L"SimplePixelShader.cso");
        
        // Load the raw vertex shader bytecode from disk and create a vertex shader with it.
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
          // For this lesson, this is simply a DirectX::XMFLOAT2 vector defining the vertex position.
          const D3D11_INPUT_ELEMENT_DESC basicVertexLayoutDesc[] =
          {
              { "POSITION", 0, DXGI_FORMAT_R32G32_FLOAT, 0, 0, D3D11_INPUT_PER_VERTEX_DATA, 0 },
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

        // Create vertex and index buffers that define a simple triangle.
        auto createTriangleTask = (createPSTask && createVSTask).then([this] () {

          DirectX::XMFLOAT2 triangleVertices[] =
          {
              float2(-0.5f, -0.5f),
              float2( 0.0f,  0.5f),
              float2( 0.5f, -0.5f),
          };

          unsigned short triangleIndices[] =
          {
              0, 1, 2,
          };

          D3D11_BUFFER_DESC vertexBufferDesc = {0};
          vertexBufferDesc.ByteWidth = sizeof(float2) * ARRAYSIZE(triangleVertices);
          vertexBufferDesc.Usage = D3D11_USAGE_DEFAULT;
          vertexBufferDesc.BindFlags = D3D11_BIND_VERTEX_BUFFER;
          vertexBufferDesc.CPUAccessFlags = 0;
          vertexBufferDesc.MiscFlags = 0;
          vertexBufferDesc.StructureByteStride = 0;

          D3D11_SUBRESOURCE_DATA vertexBufferData;
          vertexBufferData.pSysMem = triangleVertices;
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
          indexBufferDesc.ByteWidth = sizeof(unsigned short) * ARRAYSIZE(triangleIndices);
          indexBufferDesc.Usage = D3D11_USAGE_DEFAULT;
          indexBufferDesc.BindFlags = D3D11_BIND_INDEX_BUFFER;
          indexBufferDesc.CPUAccessFlags = 0;
          indexBufferDesc.MiscFlags = 0;
          indexBufferDesc.StructureByteStride = 0;

          D3D11_SUBRESOURCE_DATA indexBufferData;
          indexBufferData.pSysMem = triangleIndices;
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
        });
```

頂点シェーダー、ピクセル シェーダー、頂点シェーダー レイアウト、頂点バッファー、インデックス バッファーを使って、黄色の三角形を描画します。

### <a name="4-drawing-the-triangle-and-presenting-the-rendered-image"></a>4. 三角形の描画とレンダリングされた画像の表示

シーンをレンダリングして表示し続けるために、無限ループを使います。 [**ID3D11DeviceContext::OMSetRenderTargets**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-omsetrendertargets) を呼び出して、レンダー ターゲットを出力ターゲットとして指定します。 [**ID3D11DeviceContext::ClearRenderTargetView**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-clearrendertargetview) の呼び出しに { 0.071f, 0.04f, 0.561f, 1.0f } を渡して、レンダー ターゲットを無地の青色にクリアします。

無限ループで、黄色の三角形を青色のサーフェス上に描画します。

**黄色の三角形を描画するには**

1.  まず、頂点バッファーから入力アセンブラー ステージへのデータの流れを定義するために、[**ID3D11DeviceContext::IASetInputLayout**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetinputlayout) を呼び出します。
2.  次に、[**ID3D11DeviceContext::IASetVertexBuffers**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetvertexbuffers) と [**ID3D11DeviceContext::IASetIndexBuffer**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetindexbuffer) を呼び出して、頂点バッファーとインデックス バッファーを入力アセンブラー ステージにバインドします。
3.  次に、 [**D3D11 \_ プリミティブ \_ トポロジ \_ TRIANGLESTRIP**](/previous-versions/windows/desktop/legacy/ff476189(v=vs.85))値を指定して[**である id3d11devicecontext:: IASetPrimitiveTopology**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetprimitivetopology)を呼び出し、頂点データを三角形ストリップとして解釈するように入力アセンブラーステージを指定します。
4.  次に、[**ID3D11DeviceContext::VSSetShader**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-vssetshader) を呼び出して頂点シェーダー ステージを頂点シェーダー コードで初期化し、さらに、[**ID3D11DeviceContext::PSSetShader**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-pssetshader) を呼び出してピクセル シェーダー ステージをピクセル シェーダー コードで初期化します。
5.  最後に、[**ID3D11DeviceContext::DrawIndexed**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-drawindexed) を呼び出して三角形を描画し、レンダリング パイプラインに送ります。

レンダリングされた画像をウィンドウに表示するために、[**IDXGISwapChain::Present**](/windows/desktop/api/dxgi/nf-dxgi-idxgiswapchain-present) を呼び出しています。

```cpp
            // Specify the render target we created as the output target.
            m_d3dDeviceContext->OMSetRenderTargets(
                1,
                m_renderTargetView.GetAddressOf(),
                nullptr // Use no depth stencil.
                );

            // Clear the render target to a solid color.
            const float clearColor[4] = { 0.071f, 0.04f, 0.561f, 1.0f };
            m_d3dDeviceContext->ClearRenderTargetView(
                m_renderTargetView.Get(),
                clearColor
                );

            m_d3dDeviceContext->IASetInputLayout(inputLayout.Get());

            // Set the vertex and index buffers, and specify the way they define geometry.
            UINT stride = sizeof(float2);
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

            m_d3dDeviceContext->PSSetShader(
                pixelShader.Get(),
                nullptr,
                0
                );

            // Draw the cube.
            m_d3dDeviceContext->DrawIndexed(
                ARRAYSIZE(triangleIndices),
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


ここでは、頂点シェーダーとピクセル シェーダーを使って、黄色の三角形を作成し描画しました。

次に、周回する 3D 立方体を作成し、そこに照明効果を適用します。

[プリミティブに対する深度と各種効果の使用](using-depth-and-effects-on-primitives.md)

 

 