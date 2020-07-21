---
title: レンダリングの概要
description: レンダリングパイプラインを開発してグラフィックスを表示する方法について説明します。 レンダリングの概要。
ms.assetid: 1da3670b-2067-576f-da50-5eba2f88b3e6
ms.date: 10/24/2017
ms.topic: article
keywords: Windows 10, UWP, ゲーム, レンダリング
ms.localizationpriority: medium
ms.openlocfilehash: d1abb324c5e9e16babbbf8d3650adc39cb995137
ms.sourcegitcommit: 20969781aca50738792631f4b68326f9171a3980
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/26/2020
ms.locfileid: "85409641"
---
# <a name="rendering-framework-i-intro-to-rendering"></a>レンダリング フレームワーク I: レンダリングの概要

> [!NOTE]
> このトピックは、「DirectX チュートリアルシリーズ[を含む simple ユニバーサル Windows プラットフォーム (UWP) ゲームの作成](tutorial--create-your-first-uwp-directx-game.md)」に含まれています。 このリンクのトピックでは、系列のコンテキストを設定します。

ここまでは、ユニバーサル Windows プラットフォーム (UWP) ゲームを構築する方法、およびゲームのフローを処理するステートマシンを定義する方法について説明しました。 次に、レンダリングフレームワークを開発する方法を学習します。 サンプルゲームで Direct3D 11 を使用してゲームシーンをレンダリングする方法を見てみましょう。

Direct3D 11 には、ハイパフォーマンスグラフィックハードウェアの高度な機能へのアクセスを提供する一連の Api が含まれています。この機能を使用して、ゲームなどのグラフィックス集中型アプリケーション用の3D グラフィックスを作成できます。

ゲームグラフィックスを画面にレンダリングすることは、基本的に画面上の一連のフレームをレンダリングすることを意味します。 各フレームでは、ビューに基づいて、シーンに表示されているオブジェクトをレンダリングする必要があります。

フレームをレンダリングするには、画面に表示できるように、必要なシーンの情報をハードウェアに渡す必要があります。 何かを画面に表示するには、ゲームの実行を開始すると同時にレンダリングを開始する必要があります。

## <a name="objectives"></a>目標

UWP DirectX ゲームのグラフィックス出力を表示するための基本的なレンダリングフレームワークを設定する。 この3つの手順には、疎に分割することができます。

1. グラフィックスインターフェイスへの接続を確立します。
2. グラフィックスを描画するために必要なリソースを作成します。
3. フレームをレンダリングして、グラフィックスを表示します。

このトピックでは、手順1と3について説明します。

[レンダリングフレームワーク II: ゲームのレンダリング](tutorial-game-rendering.md)では、レンダリング &mdash; フレームワークを設定する方法と、レンダリングが発生する前にデータを準備する方法について説明します。

## <a name="get-started"></a>開始

基本的なグラフィックスとレンダリングの概念を理解することをお勧めします。 Direct3D とレンダリングに慣れていない場合は、このトピックで使用するグラフィックスとレンダリングの用語の簡単な説明については、「[用語と概念](#terms-and-concepts)」を参照してください。

このゲームでは、 **GameRenderer**クラスはこのサンプルゲームのレンダラーを表します。 レンダラーは、ゲームの視覚効果を生成するために使用する、すべての Direct3D 11 オブジェクトと Direct2D オブジェクトの作成と保持を担当します。 また、表示するオブジェクトのリストを取得するために使用される**Simple3DGame**オブジェクトへの参照と、ヘッドアップディスプレイ (HUD) に対するゲームの状態も保持します。

このチュートリアルでは、ゲームの 3D オブジェクトのレンダリングに重点を置いています。

## <a name="establish-a-connection-to-the-graphics-interface"></a>グラフィックス インターフェイスへの接続を確立する

レンダリングのためにハードウェアにアクセスする方法の詳細については、「[ゲームの UWP アプリフレームワークの定義](tutorial--building-the-games-uwp-app-framework.md#the-appinitialize-method)」を参照してください。

### <a name="the-appinitialize-method"></a>App:: Initialize メソッド

次に示すように、 **std:: make_shared**関数を使用して、 **DX::D eviceresources**の**shared_ptr**を作成します。これにより、デバイスへのアクセスも提供されます。

Direct3D 11 では、[デバイス](#device)を使用して、オブジェクトの割り当てと破棄、プリミティブのレンダリング、グラフィックス ドライバー経由のグラフィックス カードとの通信を行います。

```cppwinrt
void Initialize(CoreApplicationView const& applicationView)
{
    ...

    // At this point we have access to the device. 
    // We can create the device-dependent resources.
    m_deviceResources = std::make_shared<DX::DeviceResources>();
}
```

## <a name="display-the-graphics-by-rendering-the-frame"></a>フレームをレンダリングすることによってグラフィックスを表示する

ゲームのシーンは、ゲームが起動したときにレンダリングされる必要があります。 次に示すように、 [**GameMain:: Run**](#gamemainrun-method)メソッドで開始するための説明を表示します。

単純なフローは次のようになります。

1. **Update**
2. **Render**
3. **存在**

### <a name="gamemainrun-method"></a>GameMain::Run メソッド

```cppwinrt
void GameMain::Run()
{
    while (!m_windowClosed)
    {
        if (m_visible) // if the window is visible
        {
            switch (m_updateState)
            {
            ...
            default:
                CoreWindow::GetForCurrentThread().Dispatcher().ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);
                Update();
                m_renderer->Render();
                m_deviceResources->Present();
                m_renderNeeded = false;
            }
        }
        else
        {
            CoreWindow::GetForCurrentThread().Dispatcher().ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
        }
    }
    m_game->OnSuspending();  // Exiting due to window close, so save state.
}
```

### <a name="update"></a>更新

[**GameMain:: Update**](tutorial-game-flow-management.md#the-gamemainupdate-method)メソッドでゲームの状態を更新する方法の詳細については、「 [game flow management](tutorial-game-flow-management.md) 」を参照してください。

### <a name="render"></a>レンダー

レンダリングは、 **GameMain:: Run**から[**GameRenderer:: Render**](#gamerendererrender-method)メソッドを呼び出すことによって実装されます。

[ステレオレンダリング](#stereo-rendering)が有効になっている場合は、2つのレンダリングパスが左側に1つ、右側に1つずつ表示され &mdash; ます。 各レンダリング パスで、レンダー ターゲットと深度ステンシル ビューをデバイスにバインドします。 後で深度ステンシル ビューをクリアします。

> [!NOTE]
> ステレオ レンダリングは、頂点のインスタンス化やジオメトリ シェーダーを使用する単一パス ステレオなど、他の方法で実現することもできます。 2つのレンダリングパスメソッドは、ステレオレンダリングを実現するための低速で便利な方法です。

ゲームが実行され、リソースが読み込まれたら、レンダリングパスごとに1回[射影行列](#projection-transform-matrix)を更新します。 オブジェクトは、各ビューで若干異なります。 次に、[グラフィックスレンダリングパイプライン](#rendering-pipeline)を設定します。 

> [!NOTE]
> リソースを読み込む方法については、「[DirectX グラフィックス リソースの作成と読み込み](tutorial-game-rendering.md#create-and-load-directx-graphic-resources)」を参照してください。

このサンプルゲームでは、レンダラーはすべてのオブジェクトに対して標準の頂点レイアウトを使用するように設計されています。 これにより、シェーダーの設計が単純化され、オブジェクトのジオメトリとは関係なく、シェーダー間で簡単に変更できるようになります。

#### <a name="gamerendererrender-method"></a>GameRenderer::Render メソッド

入力頂点レイアウトを使用するように Direct3D コンテキストを設定します。 入力レイアウト オブジェクトは、頂点バッファー データを[レンダリング パイプライン](#rendering-pipeline)にストリーミングする方法を記述します。 

次に、前に定義した定数バッファーを使用するように Direct3D コンテキストを設定します。これは、[頂点シェーダー](#vertex-shaders-and-pixel-shaders)パイプラインステージおよび[ピクセルシェーダー](#vertex-shaders-and-pixel-shaders)パイプラインステージで使用されます。 

> [!NOTE]
> 定数バッファーの定義の詳細については、「[レンダリング フレームワーク II: ゲームのレンダリング](tutorial-game-rendering.md)」を参照してください。

同じ入力レイアウトと一連の定数バッファーが、パイプライン内のすべてのシェーダーで使用されるため、設定はフレームごとに 1 回です。

```cppwinrt
void GameRenderer::Render()
{
    bool stereoEnabled{ m_deviceResources->GetStereoState() };

    auto d3dContext{ m_deviceResources->GetD3DDeviceContext() };
    auto d2dContext{ m_deviceResources->GetD2DDeviceContext() };

    int renderingPasses = 1;
    if (stereoEnabled)
    {
        renderingPasses = 2;
    }

    for (int i = 0; i < renderingPasses; i++)
    {
        // Iterate through the number of rendering passes to be completed.
        // 2 rendering passes if stereo is enabled.
        if (i > 0)
        {
            // Doing the Right Eye View.
            ID3D11RenderTargetView* const targets[1] = { m_deviceResources->GetBackBufferRenderTargetViewRight() };

            // Resets render targets to the screen.
            // OMSetRenderTargets binds 2 things to the device.
            // 1. Binds one render target atomically to the device.
            // 2. Binds the depth-stencil view, as returned by the GetDepthStencilView method, to the device.
            // For more info, see
            // https://docs.microsoft.com/windows/win32/api/d3d11/nf-d3d11-id3d11devicecontext-omsetrendertargets

            d3dContext->OMSetRenderTargets(1, targets, m_deviceResources->GetDepthStencilView());

            // Clears the depth stencil view.
            // A depth stencil view contains the format and buffer to hold depth and stencil info.
            // For more info about depth stencil view, go to: 
            // https://docs.microsoft.com/windows/uwp/graphics-concepts/depth-stencil-view--dsv-
            // A depth buffer is used to store depth information to control which areas of 
            // polygons are rendered rather than hidden from view. To learn more about a depth buffer,
            // go to: https://docs.microsoft.com/windows/uwp/graphics-concepts/depth-buffers
            // A stencil buffer is used to mask pixels in an image, to produce special effects. 
            // The mask determines whether a pixel is drawn or not,
            // by setting the bit to a 1 or 0. To learn more about a stencil buffer,
            // go to: https://docs.microsoft.com/windows/uwp/graphics-concepts/stencil-buffers

            d3dContext->ClearDepthStencilView(m_deviceResources->GetDepthStencilView(), D3D11_CLEAR_DEPTH, 1.0f, 0);

            // Direct2D -- discussed later
            d2dContext->SetTarget(m_deviceResources->GetD2DTargetBitmapRight());
        }
        else
        {
            // Doing the Mono or Left Eye View.
            // As compared to the right eye:
            // m_deviceResources->GetBackBufferRenderTargetView instead of GetBackBufferRenderTargetViewRight
            ID3D11RenderTargetView* const targets[1] = { m_deviceResources->GetBackBufferRenderTargetView() };

            // Same as the Right Eye View.
            d3dContext->OMSetRenderTargets(1, targets, m_deviceResources->GetDepthStencilView());
            d3dContext->ClearDepthStencilView(m_deviceResources->GetDepthStencilView(), D3D11_CLEAR_DEPTH, 1.0f, 0);

            // d2d -- Discussed later under Adding UI
            d2dContext->SetTarget(m_deviceResources->GetD2DTargetBitmap());
        }

        const float clearColor[4] = { 0.5f, 0.5f, 0.8f, 1.0f };

        // Only need to clear the background when not rendering the full 3D scene since
        // the 3D world is a fully enclosed box and the dynamics prevents the camera from
        // moving outside this space.
        if (i > 0)
        {
            // Doing the Right Eye View.
            d3dContext->ClearRenderTargetView(m_deviceResources->GetBackBufferRenderTargetViewRight(), clearColor);
        }
        else
        {
            // Doing the Mono or Left Eye View.
            d3dContext->ClearRenderTargetView(m_deviceResources->GetBackBufferRenderTargetView(), clearColor);
        }

        // Render the scene objects
        if (m_game != nullptr && m_gameResourcesLoaded && m_levelResourcesLoaded)
        {
            // This section is only used after the game state has been initialized and all device
            // resources needed for the game have been created and associated with the game objects.
            if (stereoEnabled)
            {
                // When doing stereo, it is necessary to update the projection matrix once per rendering pass.

                auto orientation = m_deviceResources->GetOrientationTransform3D();

                ConstantBufferChangeOnResize changesOnResize;
                // Apply either a left or right eye projection, which is an offset from the middle
                XMStoreFloat4x4(
                    &changesOnResize.projection,
                    XMMatrixMultiply(
                        XMMatrixTranspose(
                            i == 0 ?
                            m_game->GameCamera().LeftEyeProjection() :
                            m_game->GameCamera().RightEyeProjection()
                            ),
                        XMMatrixTranspose(XMLoadFloat4x4(&orientation))
                        )
                    );

                d3dContext->UpdateSubresource(
                    m_constantBufferChangeOnResize.get(),
                    0,
                    nullptr,
                    &changesOnResize,
                    0,
                    0
                    );
            }

            // Update variables that change once per frame.
            ConstantBufferChangesEveryFrame constantBufferChangesEveryFrameValue;
            XMStoreFloat4x4(
                &constantBufferChangesEveryFrameValue.view,
                XMMatrixTranspose(m_game->GameCamera().View())
                );
            d3dContext->UpdateSubresource(
                m_constantBufferChangesEveryFrame.get(),
                0,
                nullptr,
                &constantBufferChangesEveryFrameValue,
                0,
                0
                );

            // Set up the graphics pipeline. This sample uses the same InputLayout and set of
            // constant buffers for all shaders, so they only need to be set once per frame.
            // For more info about the graphics or rendering pipeline, see
            // https://docs.microsoft.com/windows/win32/direct3d11/overviews-direct3d-11-graphics-pipeline

            // IASetInputLayout binds an input-layout object to the input-assembler (IA) stage. 
            // Input-layout objects describe how vertex buffer data is streamed into the IA pipeline stage.
            // Set up the Direct3D context to use this vertex layout. For more info, see
            // https://docs.microsoft.com/windows/win32/api/d3d11/nf-d3d11-id3d11devicecontext-iasetinputlayout
            d3dContext->IASetInputLayout(m_vertexLayout.get());

            // VSSetConstantBuffers sets the constant buffers used by the vertex shader pipeline stage.
            // Set up the Direct3D context to use these constant buffers. For more info, see
            // https://docs.microsoft.com/windows/win32/api/d3d11/nf-d3d11-id3d11devicecontext-vssetconstantbuffers

            ID3D11Buffer* constantBufferNeverChanges{ m_constantBufferNeverChanges.get() };
            d3dContext->VSSetConstantBuffers(0, 1, &constantBufferNeverChanges);
            ID3D11Buffer* constantBufferChangeOnResize{ m_constantBufferChangeOnResize.get() };
            d3dContext->VSSetConstantBuffers(1, 1, &constantBufferChangeOnResize);
            ID3D11Buffer* constantBufferChangesEveryFrame{ m_constantBufferChangesEveryFrame.get() };
            d3dContext->VSSetConstantBuffers(2, 1, &constantBufferChangesEveryFrame);
            ID3D11Buffer* constantBufferChangesEveryPrim{ m_constantBufferChangesEveryPrim.get() };
            d3dContext->VSSetConstantBuffers(3, 1, &constantBufferChangesEveryPrim);

            // Sets the constant buffers used by the pixel shader pipeline stage. 
            // For more info, see
            // https://docs.microsoft.com/windows/win32/api/d3d11/nf-d3d11-id3d11devicecontext-pssetconstantbuffers

            d3dContext->PSSetConstantBuffers(2, 1, &constantBufferChangesEveryFrame);
            d3dContext->PSSetConstantBuffers(3, 1, &constantBufferChangesEveryPrim);
            ID3D11SamplerState* samplerLinear{ m_samplerLinear.get() };
            d3dContext->PSSetSamplers(0, 1, &samplerLinear);

            for (auto&& object : m_game->RenderObjects())
            {
                // The 3D object render method handles the rendering.
                // For more info, see Primitive rendering below.
                object->Render(d3dContext, m_constantBufferChangesEveryPrim.get());
            }
        }

        // Start of 2D rendering
        ...
    }
}
```

### <a name="primitive-rendering"></a>プリミティブのレンダリング

シーンをレンダリングする場合は、レンダリングする必要があるすべてのオブジェクトをループ処理します。 次の手順は、オブジェクト (プリミティブ) ごとに繰り返されます。

- モデルの[ワールド変換行列](#world-transform-matrix)とマテリアル情報を使用して、定数バッファー (**m_constantBufferChangesEveryPrim**) を更新します。
- **M_constantBufferChangesEveryPrim**には、各オブジェクトのパラメーターが含まれています。 これには、オブジェクトから世界への変換行列に加えて、光源の計算に使用する色や反射の指数などの素材プロパティも含まれます。
- [レンダリングパイプライン](#rendering-pipeline)の入力アセンブラー (IA) ステージにストリームされるメッシュオブジェクトデータの入力頂点レイアウトを使用するように、Direct3D コンテキストを設定します。
- IA ステージで[インデックスバッファー](#index-buffer)を使用するように Direct3D コンテキストを設定します。 プリミティブの情報 (型、データの順序) を提供します。
- インデックス付きの、インスタンス化されていないプリミティブを描画する描画呼び出しを送信します。 指定されたプリミティブに固有のデータを使用して、指定したプリミティブ[定数バッファー](#constant-buffer-or-shader-constant-buffer)を**更新します**。 これにより、各プリミティブのジオメトリを描画するコンテキストで **DrawIndexed** 呼び出しが行われます。 特に、この描画呼び出しは、定数バッファー データによってパラメーター化されたとおり、コマンドとデータをグラフィックス処理装置 (GPU) のキューに入れます。 各描画呼び出しは、頂点ごとに 1 回頂点シェーダーを実行し、次にプリミティブの各三角形のピクセルごとに 1 回[ピクセル シェーダー](#vertex-shaders-and-pixel-shaders)を実行します。 テクスチャは、ピクセル シェーダーがレンダリングの実行に使う状態の一部です。

複数の定数バッファーを使用する理由を次に示します。

- ゲームでは複数の定数バッファーを使用しますが、プリミティブごとに1回だけ更新する必要があります。 前述のように、定数バッファーは、プリミティブごとに実行されるシェーダーに対する入力のようなものです。 一部のデータは静的 (**m_constantBufferNeverChanges**) です。一部のデータは、カメラの位置など、フレーム (**m_constantBufferChangesEveryFrame**) にわたって一定です。また、データによっては、その色やテクスチャ (**m_constantBufferChangesEveryPrim**) などのプリミティブに固有のデータもあります。
- ゲーム レンダラーはこれらの入力を別個の定数バッファーに分けて、CPU や GPU が使うメモリ帯域幅を最適化します。 この方法は、GPU が追跡する必要があるデータの量を最小限に抑えるのにも役立ちます。 GPU にはコマンドの大きいキューがあり、ゲームが **Draw** を呼び出すたびに、そのコマンドは関連するデータと共にキューに入れられます。 ゲームがプリミティブ定数バッファーを更新して、次の **Draw** コマンドを発行すると、グラフィックス ドライバーはこの次のコマンドと関連するデータをキューに追加します。 ゲームで 100 のプリミティブを描画する場合、キューに定数バッファー データの 100 のコピーが存在する可能性があります。 ゲームから GPU に送るデータ量を最小限に抑えるために、ゲームでは、各プリミティブの更新情報のみを含む個別のプリミティブ定数バッファーを使用します。

#### <a name="gameobjectrender-method"></a>GameObject::Render メソッド

```cppwinrt
void GameObject::Render(
    _In_ ID3D11DeviceContext* context,
    _In_ ID3D11Buffer* primitiveConstantBuffer
    )
{
    if (!m_active || (m_mesh == nullptr) || (m_normalMaterial == nullptr))
    {
        return;
    }

    ConstantBufferChangesEveryPrim constantBuffer;

    // Put the model matrix info into a constant buffer, in world matrix.
    XMStoreFloat4x4(
        &constantBuffer.worldMatrix,
        XMMatrixTranspose(ModelMatrix())
        );

    // Check to see which material to use on the object.
    // If a collision (a hit) is detected, GameObject::Render checks the current context, which 
    // indicates whether the target has been hit by an ammo sphere. If the target has been hit, 
    // this method applies a hit material, which reverses the colors of the rings of the target to 
    // indicate a successful hit to the player. Otherwise, it applies the default material 
    // with the same method. In both cases, it sets the material by calling Material::RenderSetup, 
    // which sets the appropriate constants into the constant buffer. Then, it calls 
    // ID3D11DeviceContext::PSSetShaderResources to set the corresponding texture resource for the 
    // pixel shader, and ID3D11DeviceContext::VSSetShader and ID3D11DeviceContext::PSSetShader 
    // to set the vertex shader and pixel shader objects themselves, respectively.

    if (m_hit && m_hitMaterial != nullptr)
    {
        m_hitMaterial->RenderSetup(context, &constantBuffer);
    }
    else
    {
        m_normalMaterial->RenderSetup(context, &constantBuffer);
    }

    // Update the primitive constant buffer with the object model's info.
    context->UpdateSubresource(primitiveConstantBuffer, 0, nullptr, &constantBuffer, 0, 0);

    // Render the mesh.
    // See MeshObject::Render method below.
    m_mesh->Render(context);
}
```

#### <a name="meshobjectrender-method"></a>MeshObject:: Render メソッド

```cppwinrt
void MeshObject::Render(_In_ ID3D11DeviceContext* context)
{
    // PNTVertex is a struct. stride provides us the size required for all the mesh data
    // struct PNTVertex
    //{
    //  DirectX::XMFLOAT3 position;
    //  DirectX::XMFLOAT3 normal;
    //  DirectX::XMFLOAT2 textureCoordinate;
    //};
    uint32_t stride{ sizeof(PNTVertex) };
    uint32_t offset{ 0 };

    // Similar to the main render loop.
    // Input-layout objects describe how vertex buffer data is streamed into the IA pipeline stage.
    ID3D11Buffer* vertexBuffer{ m_vertexBuffer.get() };
    context->IASetVertexBuffers(0, 1, &vertexBuffer, &stride, &offset);

    // IASetIndexBuffer binds an index buffer to the input-assembler stage.
    // For more info, see
    // https://docs.microsoft.com/windows/win32/api/d3d11/nf-d3d11-id3d11devicecontext-iasetindexbuffer.
    context->IASetIndexBuffer(m_indexBuffer.get(), DXGI_FORMAT_R16_UINT, 0);

    // Binds information about the primitive type, and data order that describes input data for the input assembler stage.
    // For more info, see
    // https://docs.microsoft.com/windows/win32/api/d3d11/nf-d3d11-id3d11devicecontext-iasetprimitivetopology.
    context->IASetPrimitiveTopology(D3D11_PRIMITIVE_TOPOLOGY_TRIANGLELIST);

    // Draw indexed, non-instanced primitives. A draw API submits work to the rendering pipeline.
    // For more info, see
    // https://docs.microsoft.com/windows/win32/api/d3d11/nf-d3d11-id3d11devicecontext-drawindexed.
    context->DrawIndexed(m_indexCount, 0, 0);
}
```

### <a name="deviceresourcespresent-method"></a>DeviceResources::P 再送信メソッド

**DeviceResources::P**再送信メソッドを呼び出して、バッファーに配置した内容を表示します。

ユーザーにフレームを表示するために使用されるバッファーのコレクションという意味で、スワップ チェーンという用語を使用します。 アプリケーションが表示する新しいフレームを提供するたびに、スワップ チェーンの最初のバッファーが、表示されているバッファーの場所を取得します。 このプロセスは、スワップまたはフリップと呼ばれます。 詳しくは、「[スワップ チェーン](../graphics-concepts/swap-chains.md)」をご覧ください。

- **IDXGISwapChain1**インターフェイスの**Present**メソッドは、垂直同期 (垂直同期) が行われるまでブロックするように[DXGI](#dxgi)に指示します。これにより、アプリケーションは次の垂直同期までスリープ状態になります。 これにより、画面に表示されない、どのようなサイクルのレンダリングフレームも無駄になりません。
- **ID3D11DeviceContext3**インターフェイスの**DiscardView**メソッドは、[レンダーターゲット](#render-target)の内容を破棄します。 これは、既存の内容が完全に上書きされる場合にのみ有効な操作です。 Dirty または scroll rect が使用されている場合は、この呼び出しを削除する必要があります。
* 同じ **DiscardView** メソッドを使用して、[深度/ステンシル](#depth-stencil)の内容を破棄します。
* **Handledevicループ**メソッドは、削除する[デバイス](#device)のシナリオを管理するために使用されます。 切断またはドライバーのアップグレードによってデバイスが削除された場合は、すべてのデバイスリソースを再作成する必要があります。 詳細については、「[Direct3D 11 でのデバイス削除シナリオの処理](handling-device-lost-scenarios.md)」を参照してください。

> [!TIP]
> スムーズなフレームレートを実現するには、フレームをレンダリングするための作業量が、VSyncs の間隔に収まるようにする必要があります。

```cppwinrt
// Present the contents of the swap chain to the screen.
void DX::DeviceResources::Present()
{
    // The first argument instructs DXGI to block until VSync, putting the application
    // to sleep until the next VSync. This ensures we don't waste any cycles rendering
    // frames that will never be displayed to the screen.
    HRESULT hr = m_swapChain->Present(1, 0);

    // Discard the contents of the render target.
    // This is a valid operation only when the existing contents will be entirely
    // overwritten. If dirty or scroll rects are used, this call should be removed.
    m_d3dContext->DiscardView(m_d3dRenderTargetView.get());

    // Discard the contents of the depth stencil.
    m_d3dContext->DiscardView(m_d3dDepthStencilView.get());

    // If the device was removed either by a disconnection or a driver upgrade, we 
    // must recreate all device resources.
    if (hr == DXGI_ERROR_DEVICE_REMOVED || hr == DXGI_ERROR_DEVICE_RESET)
    {
        HandleDeviceLost();
    }
    else
    {
        winrt::check_hresult(hr);
    }
}
```

## <a name="next-steps"></a>次のステップ

このトピックでは、画面でのグラフィックスのレンダリング方法について説明します。また、使用されている一部のレンダリング用語について簡単な説明を示します (下記参照)。 レンダリングの詳細については、「[レンダリングフレームワーク II: ゲームレンダリング](tutorial-game-rendering.md)」を参照してください。表示する前に必要なデータを準備する方法についても説明します。

## <a name="terms-and-concepts"></a>用語と概念

### <a name="simple-game-scene"></a>シンプルなゲームのシーン

シンプルなゲームのシーンは、数個のオブジェクトといくつかの光源で構成されます。

オブジェクトのシェイプは、空間の一連の X、Y、Z 座標で定義されます。 ゲーム ワールド内の実際のレンダリングの位置は、位置の X、Y、Z 座標に変換行列を適用することで決定できます。 また、テクスチャ座標のセットと V を使用して、 &mdash; &mdash; 素材をオブジェクトに適用する方法を指定することもできます。 これにより、オブジェクトの表面プロパティが定義されます。また、オブジェクトに粗い表面 (テニスボールなど) があるか、または滑らかな光沢のある表面 (ボウリングボールなど) であるかを確認できます。

シーンとオブジェクトの情報は、レンダリングフレームワークによって使用され、フレームごとにシーンフレームが再作成され、ディスプレイモニターで動作するようになります。

### <a name="rendering-pipeline"></a>レンダリング パイプライン

レンダリングパイプラインは、3D シーン情報を画面に表示されるイメージに変換するプロセスです。 Direct3D 11 では、このパイプラインはプログラミング可能です。 レンダリングのニーズをサポートするために、ステージを適合させることができます。 一般的なシェーダー コアの機能を利用するステージは、HLSL プログラミング言語を使用することによってプログラミングできます。 これは、*グラフィックスレンダリングパイプライン*または単に*パイプライン*とも呼ばれます。

このパイプラインを作成するには、これらの詳細について理解しておく必要があります。

- [HLSL](#hlsl)。 UWP DirectX ゲームでは、HLSL シェーダー モデル 5.1 以上を使用することをお勧めします。
- [シェーダー](#shaders)。
- [頂点シェーダーとピクセルシェーダー](#vertex-shaders-and-pixel-shaders)。
- [シェーダーのステージ](#shader-stages)。
- [さまざまなシェーダーファイル形式](#various-shader-file-formats)。

詳細については、[Direct3D 11 のレンダリング パイプラインに関するページ](/windows/desktop/direct3dgetstarted/understand-the-directx-11-2-graphics-pipeline)と[グラフィックス パイプラインに関するページ](/windows/desktop/direct3d11/overviews-direct3d-11-graphics-pipeline)を参照してください。

#### <a name="hlsl"></a>HLSL

HLSL は、DirectX の高レベルシェーダー言語です。 HLSL を使用すると、Direct3D パイプライン用に C に似たプログラミング可能なシェーダーを作成できます。 詳しくは、「[HLSL](/windows/desktop/direct3dhlsl/dx-graphics-hlsl)」をご覧ください。

#### <a name="shaders"></a>シェーダー

シェーダーは、レンダリング時にオブジェクトのサーフェイスがどのように表示されるかを決定する命令のセットと考えることができます。 HLSL を使用してプログラミングされるシェーダーは、HLSL シェーダーと呼ばれます。 [HLSL]) (#hlsl) シェーダーのソースコードファイルには、 `.hlsl` ファイル拡張子があります。 これらのシェーダーは、ビルド時または実行時にコンパイルし、実行時に適切なパイプラインステージに設定できます。 コンパイル済みのシェーダーオブジェクトには、 `.cso` ファイル拡張子があります。

Direct3D 9 シェーダーは、シェーダーモデル1、シェーダーモデル2、およびシェーダーモデル3を使用して設計できます。Direct3D 10 シェーダーは、シェーダーモデル4でのみ設計できます。 Direct3D 11 シェーダーは、シェーダー モデル 5 で設計できます。 Direct3D 11.3 および Direct3D 12 シェーダーは、シェーダー モデル 5.1 で設計できます。Direct3D 12 シェーダーは、シェーダー モデル 6 でも設計できます。

#### <a name="vertex-shaders-and-pixel-shaders"></a>頂点シェーダーとピクセル シェーダー

データは、プリミティブのストリームとしてグラフィックスパイプラインに入り、頂点シェーダーやピクセルシェーダーなどのさまざまなシェーダーによって処理されます。 

頂点シェーダーは、通常、変換、スキン、照明の適用などの操作を実行することによって、頂点を処理します。 ピクセル シェーダーは、ピクセル単位の照明や後処理など、豊富なシェーディング手法を実現します。 ピクセル シェーダーは、定数変数、テクスチャ データ、補間された頂点単位の値などのデータを組み合わせて、ピクセル単位の出力を生成します。 

#### <a name="shader-stages"></a>シェーダー ステージ

このプリミティブのストリームを処理するために定義されたさまざまなシェーダーのシーケンスは、レンダリング パイプラインのシェーダー ステージと呼ばれます。 実際のステージは、Direct3D のバージョンによって異なりますが、通常は、頂点、ジオメトリ、ピクセルのステージが含まれます。 テセレーション用のハル シェーダーやドメイン シェーダー、計算シェーダーなど、他のステージもあります。 これらのステージはすべて、 [HLSL](#hlsl)を使用して完全にプログラミングできます。 詳細については、[グラフィックス パイプラインに関するページ](/windows/desktop/direct3d11/overviews-direct3d-11-graphics-pipeline)を参照してください。

#### <a name="various-shader-file-formats"></a>さまざまなシェーダー ファイル形式

ここでは、シェーダーコードファイルの拡張子を示します。

- 拡張子がのファイルは、 `.hlsl` [HLSL]) (#hlsl) のソースコードを保持します。
- 拡張子を持つファイルは、 `.cso` コンパイル済みのシェーダーオブジェクトを保持します。
- 拡張子を持つファイル `.h` はヘッダーファイルですが、シェーダーコードコンテキストでは、このヘッダーファイルはシェーダーデータを保持するバイト配列を定義します。
- 拡張子の付いたファイルには、 `.hlsli` 定数バッファーの形式が含まれています。 サンプルゲームでは、ファイルは**シェーダー**  >  **ConstantBuffers. .hlsli**です。

> [!NOTE]
> シェーダーを埋め込むに `.cso` は、実行時にファイルを読み込むか、 `.h` 実行可能コードにファイルを追加します。 しかし、同じシェーダーで両方を使用することはありません。

### <a name="deeper-understanding-of-directx"></a>DirectX に関する高度な知識

Direct3D 11 は、ゲームなどのグラフィックスを多用するアプリケーションのためにグラフィックスを作成するのに役立つ Api のセットです。これにより、大量の計算を処理するための優れたグラフィックスカードが必要になります。 このセクションでは、Direct3D 11 グラフィックス プログラミングの概念 (リソース、サブリソース、デバイス、デバイス コンテキスト) について簡単に説明します。

#### <a name="resource"></a>リソース

リソース (デバイスリソースとも呼ばれます) は、テクスチャ、位置、色など、オブジェクトのレンダリング方法に関する情報と考えることができます。 リソースはパイプラインにデータを提供し、シーン中に表示される内容を定義します。 リソースはゲームメディアから読み込むことも、実行時に動的に作成することもできます。

リソースは、実際には、Direct3D [パイプライン](#rendering-pipeline)からアクセスできるメモリ内の領域です。 パイプラインでメモリに効率的にアクセスするには、パイプラインに渡すデータ (入力ジオメトリ、シェーダー リソース、テクスチャなど) をリソースに格納する必要があります。 すべての Direct3D リソースの派生元となるリソースは 2 種類あります。バッファーとテクスチャです。 各パイプライン ステージでは最大 128 個のリソースをアクティブにできます。 詳細については、[リソース](../graphics-concepts/resources.md)に関連するページを参照してください。

#### <a name="subresource"></a>サブリソース

サブリソースという用語はリソースのサブセットを指します。 Direct3D は、リソース全体を参照することも、リソースのサブセットを参照することもできます。 詳しくは、「[サブリソース](../graphics-concepts/resource-types.md#subresources)」をご覧ください。

#### <a name="depth-stencil"></a>深度/ステンシル

深度/ステンシル リソースには、深度とステンシルの情報を保持するための形式とバッファーが含まれます。 このリソースは、テクスチャー リソースを使用して作成されます。 深度/ステンシル リソースを作成する方法について詳しくは、「[深度/ステンシル機能の構成](/windows/desktop/direct3d11/d3d10-graphics-programming-guide-depth-stencil)」を参照してください。 深度/ステンシル リソースにアクセスするには、[ID3D11DepthStencilView](/windows/desktop/api/d3d11/nn-d3d11-id3d11depthstencilview) インターフェイスを使って実装される深度/ステンシル ビュー使用します。

[深度情報] は、どの部分がどのように隠れているかを判別できるように、他の部分の背後にある領域を示します。 ステンシル情報は、マスクするピクセルを示します。 ステンシル情報は、ピクセルを描画するかどうかを決定する (ビットを 1 または 0 に設定する) ため、特殊効果を生成するために使用できます。 

詳細については、「[深度ステンシルビュー](../graphics-concepts/depth-stencil-view--dsv-.md)、[深度バッファー](../graphics-concepts/depth-buffers.md)、および[ステンシルバッファー](../graphics-concepts/stencil-buffers.md)」を参照してください。

#### <a name="render-target"></a>レンダー ターゲット

レンダー ターゲットは、レンダリング パスの最後に書き込むことができるリソースです。 通常、[ID3D11Device::CreateRenderTargetView](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createrendertargetview) メソッドで、入力パラメーターとしてスワップ チェーン バック バッファー (これもリソースです) を使用して作成されます。 

各レンダー ターゲットには、対応する深度/ステンシル ビューも必要です。これは、レンダー ターゲットを使用する前に、[OMSetRenderTargets](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-omsetrendertargets) を使用してレンダー ターゲットを設定するときに、深度/ステンシル ビューも必要となるためです。 レンダー ターゲット リソースにアクセスするには、[ID3D11RenderTargetView](/windows/desktop/api/d3d11/nn-d3d11-id3d11rendertargetview) インターフェイスを使用して実装されるレンダー ターゲット ビューを使用します。 

#### <a name="device"></a>デバイス

デバイスは、グラフィックスドライバーを使用して、オブジェクトの割り当てと破棄、プリミティブのレンダリング、グラフィックスカードとの通信を行う方法として考えることができます。 

より正確に説明すると、Direct3D デバイスは Direct3D のレンダリング コンポーネントです。 デバイスは、レンダリングの状態をカプセル化して格納します。また、変換や照明の操作、サーフェスへのイメージのラスタライズを実行します。 詳しくは、「[デバイス](../graphics-concepts/devices.md)」をご覧ください。

デバイスは、 [**ID3D11Device**](/windows/desktop/api/d3d11/nn-d3d11-id3d11device)インターフェイスによって表されます。 言い換えると、 **ID3D11Device**インターフェイスは仮想ディスプレイアダプターを表し、デバイスによって所有されているリソースを作成するために使用されます。 

ID3D11Device には、さまざまなバージョンがあります。 [**ID3D11Device5**](/windows/desktop/api/d3d11_4/nn-d3d11_4-id3d11device5)は最新バージョンで、 **ID3D11Device4**内のメソッドに新しいメソッドを追加します。 Direct3D が基になるハードウェアと通信する方法の詳細については、[Windows Device Driver Model (WDDM) アーキテクチャに関するページ](/windows-hardware/drivers/display/windows-vista-and-later-display-driver-model-architecture)を参照してください。

各アプリケーションには、少なくとも1つのデバイスが必要です。ほとんどのアプリケーションで作成できるのは1つだけです。 **D3D11CreateDevice**または**D3D11CreateDeviceAndSwapChain**を呼び出し、 **D3D_DRIVER_TYPE**フラグを使用してドライバーの種類を指定して、コンピューターにインストールされているハードウェアドライバーの1つに対してデバイスを作成します。 各デバイスでは、必要な機能に応じて、1 つまたは複数のデバイス コンテキストを使用できます。 詳細については、[D3D11CreateDevice 関数に関するページ](/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice)を参照してください。

#### <a name="device-context"></a>デバイスのコンテキスト

デバイスコンテキストは、[パイプライン](#rendering-pipeline)の状態を設定し、[デバイス](#device)が所有する[リソース](#resource)を使用してレンダリングコマンドを生成するために使用されます。 

Direct3D 11 は 2 種類のデバイス コンテキストを実装します。1 つは即時レンダリング用で、もう 1 つは遅延レンダリング用です。いずれのコンテキストも [ID3D11DeviceContext](/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext) インターフェイスを使用して表されます。 

**ID3D11DeviceContext** インターフェイスには複数のバージョンがあり、**ID3D11DeviceContext4** は **ID3D11DeviceContext3** に新しいメソッドを追加します。

**ID3D11DeviceContext4**は、Windows 10 の作成者の更新プログラムで導入され、**である id3d11devicecontext**インターフェイスの最新バージョンです。 Windows 10 の作成者を対象とするアプリケーションでは、以前のバージョンではなく、このインターフェイスを使用する必要があります。 詳しくは、[ID3D11DeviceContext4 に関するページ](/windows/desktop/api/d3d11_3/nn-d3d11_3-id3d11devicecontext4)を参照してください。

#### <a name="dxdeviceresources"></a>DX::DeviceResources

**DX::D eviceresources**クラスは**DeviceResources**ファイル内にあり、 / すべての DirectX デバイスリソースを制御し**ます。**

### <a name="buffer"></a>バッファー

バッファー リソースは完全に型指定されたデータのコレクションであり、複数の要素にグループ化されます。 バッファーを使用して、さまざまなデータを格納できます。たとえば、位置ベクトル、法線ベクトル、頂点バッファー内のテクスチャ座標、インデックス バッファー内のインデックス、デバイスの状態などのデータが格納されます。 バッファー要素には、パックされたデータ値 ( **R8G8B8A8** surface 値など)、1つの8ビット整数、または 4 32 ビット浮動小数点値を含めることができます。

使用可能なバッファーには、頂点バッファー、インデックスバッファー、および定数バッファーの3種類があります。

#### <a name="vertex-buffer"></a>頂点バッファー

ジオメトリの定義に使われる頂点データが格納されます。 頂点データには、位置座標、色データ、テクスチャ座標データ、法線データなどが格納されます。 

#### <a name="index-buffer"></a>インデックス バッファー

頂点バッファーへの整数オフセットが格納されます。このバッファーは、より効率的なプリミティブのレンダリングに使われます。 インデックス バッファーは 16 ビットまたは 32 ビットの連続するインデックスを格納します。各インデックスは頂点バッファーの頂点を識別するのに使用されます。

#### <a name="constant-buffer-or-shader-constant-buffer"></a>定数バッファー、またはシェーダー定数バッファー

シェーダー データをパイプラインに効率的に提供できます。 定数バッファーは、各プリミティブについて実行され、レンダリング パイプラインのストリーム出力ステージの結果を格納するシェーダーの入力として使用できます。 定数バッファーは、概念的には要素が 1 つの頂点バッファーに似ています。

#### <a name="design-and-implementation-of-buffers"></a>バッファーの設計と実装

データ型に基づいてバッファーをデザインできます。たとえば、サンプルゲームでは、静的なデータ用に1つのバッファーを作成し、もう1つはフレームに対して一定のデータを、もう1つはプリミティブに固有のデータを作成します。

すべてのバッファーの種類は **ID3D11Buffer** インターフェイスによってカプセル化され、**ID3D11Device::CreateBuffer** を呼び出すことによってバッファー リソースを作成できます。 ただし、バッファーにアクセスするには、その前にバッファーがパイプラインにバインドされている必要があります。 バッファーは、読み取りのために同時に複数のパイプライン ステージにバインドできます。 バッファーは、書き込み用に1つのパイプラインステージにバインドすることもできます。ただし、同じバッファーを、読み取りと書き込みの両方に同時にバインドすることはできません。

これらの方法でバッファーをバインドできます。

- **である id3d11devicecontext:: IASetVertexBuffers** 、**である id3d11devicecontext:: IASetIndexBuffer**などの**である id3d11devicecontext**メソッドを呼び出すことによって、入力アセンブラーステージに。
- **である id3d11devicecontext:: SOSetTargets**を呼び出すことにより、ストリーム出力ステージに対して実行します。
- **である id3d11devicecontext:: VSSetConstantBuffers**などのシェーダーメソッドを呼び出すことによってシェーダーステージに。

詳しくは、「[Direct3D 11 のバッファーについて](/windows/desktop/direct3d11/overviews-direct3d-11-resources-buffers-intro)」をご覧ください。

### <a name="dxgi"></a>DXGI

Microsoft DirectX Graphics Infrastructure (DXGI) は、Direct3D が必要とする低レベルのタスクの一部をカプセル化するサブシステムです。 マルチスレッドアプリケーションで DXGI を使用してデッドロックが発生しないようにするには、特別な注意が必要です。 詳細については、「[マルチスレッドと DXGI](/windows/win32/direct3darticles/dxgi-best-practices#multithreading-and-dxgi) 」を参照してください。

### <a name="feature-level"></a>機能レベル

機能レベルは、最新または既存のコンピューターで多様なビデオ カードを処理するために、Direct3D 11 で導入された概念です。 特徴レベルは、適切に定義された一連のグラフィックス処理装置 (GPU) 機能です。 

各ビデオ カードは、インストールされている GPU に応じて、特定のレベルの DirectX の機能を実装します。 以前のバージョンの Microsoft Direct3D では、ビデオ カードが実装しているバージョンを検出し、それに応じてアプリケーションをプログラミングすることができました。 

機能レベルを使用すると、デバイスを作成するときに、必要な機能レベルのデバイスを作成してみることができます。 デバイスの作成に成功した場合は、その機能レベルが存在します。失敗した場合は、ハードウェアはその機能レベルをサポートしていません。 より低い機能レベルでデバイスを再作成するか、アプリケーションを終了することを選択できます。 たとえば、120の機能レベルには、Direct3D 11.3 または Direct3D 12、およびシェーダーモデル5.1 が必要です。 詳細については、[Direct3D の各機能レベルの概要に関するページ](/windows/desktop/direct3d11/overviews-direct3d-11-devices-downlevel-intro)を参照してください。

機能レベルを使用して、Direct3D 9 や Microsoft Direct3D 10、Direct3D 11 のアプリケーションを開発し、9、10、または 11 のハードウェアで実行できます (いくつか例外があります)。 詳細については、[Direct3D の機能レベルに関するページ](/windows/desktop/direct3d11/overviews-direct3d-11-devices-downlevel-intro)を参照してください。

### <a name="stereo-rendering"></a>ステレオ レンダリング

ステレオ レンダリングは奥行き感を強化するために使用されます。 左目の視点から画像と右目の視点からの画像の 2 つの画像を使用して、ディスプレイの画面にシーンを表示します。 

数学的には、ステレオ射影行列を適用することによってこれを実現します。ステレオ射影行列は、通常のモノラル射影行列を水平方向にわずかに右および左にオフセットしたものです。

このサンプルゲームでは、2つのレンダリングパスを利用してステレオレンダリングを行いました。

- 右側のレンダー ターゲットにバインドし、右の射影を適用したのち、プリミティブ オブジェクトを描画します。
- 左側のレンダー ターゲットにバインドし、左の射影を適用したのち、プリミティブ オブジェクトを描画します。

### <a name="camera-and-coordinate-space"></a>カメラと座標空間

ゲームには、独自の座標系でワールドを更新するためのコードがあります (ワールド空間またはシーン空間と呼ばれることもあります)。 カメラを含むすべてのオブジェクトはこの空間に配置されます。 詳細については、「[座標系](../graphics-concepts/coordinate-systems.md)」を参照してください。

頂点シェーダーは、次のアルゴリズムを使って (V はベクター、M は行列を表す) モデル座標からデバイス座標への変換を行います。

`V(device) = V(model) x M(model-to-world) x M(world-to-view) x M(view-to-device)`

- `M(model-to-world)`モデル座標の変換行列です。ワールド座標はワールド[変換行列](#world-transform-matrix)とも呼ばれます。 このマトリックスは、プリミティブによって提供されます。
- `M(world-to-view)`座標を表示するワールド座標の変換行列です。これは、[ビュー変換行列](#view-transform-matrix)とも呼ばれます。
  - これは、カメラのビュー行列によって提供されます。 これは、カメラの位置とルックベクター (カメラからのシーンを直接*指すベクターと、それ*に対して垂直*方向*のベクトル) によって定義されます。
  - サンプルゲームでは、 **m_viewMatrix**はビュー変換行列であり、 **Camera:: setviewparams**を使用して計算されます。
- `M(view-to-device)`は、デバイス座標に対するビュー座標 ([射影変換行列](#projection-transform-matrix)とも呼ばれます) の変換行列です。
  - このマトリックスは、カメラのプロジェクションによって提供されます。 最終的なシーンに実際に表示される領域の量に関する情報を提供します。 ビュー (視界)、縦横比、およびクリッピング面のフィールドによって、射影変換行列が定義されます。
  - サンプルゲームでは、 **m_projectionMatrix**によって射影座標への変換が定義されます。これは、**カメラ:: setprojparams**を使用して計算されます (ステレオ投影の場合は、2つの投影行列を使用し &mdash; ます)。

のシェーダーコード `VertexShader.hlsl` は、これらのベクターと行列を定数バッファーから読み込んで、すべての頂点に対してこの変換を実行します。

### <a name="coordinate-transformation"></a>座標変換

Direct3D では、3 つの変換を使用して 3D モデル座標をピクセル座標 (スクリーン空間) に変更します。 これらの変換は、ワールド変換、ビュー変換、射影変換です。 詳細については、「[変換の概要](../graphics-concepts/transform-overview.md)」を参照してください。

#### <a name="world-transform-matrix"></a>ワールド変換行列

ワールド変換は、座標系をモデル空間からワールド空間に変更します。モデル空間では、頂点はモデルのローカル原点を基準として相対的に定義されます。ワールド空間では、頂点はシーン内のすべてのオブジェクトに共通の原点を基準として相対的に定義されます。 基本的に、ワールド変換はモデルをワールド空間に配置します。そのため、このように呼ばれます。 詳細については、「[ワールド変換](../graphics-concepts/world-transform.md)」を参照してください。

#### <a name="view-transform-matrix"></a>ビュー変換行列

ビュー変換は、ビューアーをワールド空間に配置し、頂点をカメラ空間に変換します。 カメラ空間では、カメラつまりビューアーは原点に位置し、Z 軸の正方向を向いています。 詳細については、「[ビュー変換](../graphics-concepts/view-transform.md)」を参照してください。

####  <a name="projection-transform-matrix"></a>射影変換行列

射影変換では、視錐台を立方体に変換します。 視錐台とは、ビューポートのカメラに対して相対的に配置された、シーン内の 3D ボリュームです。 ビューポートとは、3D シーンが投影される 2D の四角形です。 詳細については、「[ビューポートとクリッピング](../graphics-concepts/viewports-and-clipping.md)」を参照してください。

視錐台の近くの端は遠くの端よりも小さいので、このトランスフォームにはカメラの近くのオブジェクトを拡大するという効果があり、これによってシーンに遠近感が生まれます。 これにより、プレーヤーに近いオブジェクトが表示されます。さらに離れているオブジェクトは、より小さく表示されます。

数学的に言うと、射影変換は、通常はスケールとパースペクティブの両方の投影である行列です。 カメラのレンズと同様に機能します。 詳細については、「[射影変換](../graphics-concepts/projection-transform.md)」を参照してください。

### <a name="sampler-state"></a>サンプラーの状態

サンプラーの状態は、テクスチャのアドレス指定モード、フィルター、詳細レベルを使用して、テクスチャ データをサンプリングする方法を決定します。 テクスチャピクセル (またはテクセル) がテクスチャから読み取られるたびにサンプリングが実行されます。

テクスチャには、テクセルの配列が含まれています。 各テクセルの位置はで表されます `(u,v)` 。ここで、 `u` は幅、は高さ、は `v` テクスチャの幅と高さに基づいて0と1の間にマップされます。 結果として得られるテクスチャ座標は、テクスチャをサンプリングするときに、テクセルのアドレス指定に使用されます。

テクスチャ座標が 0 未満または 1 を超える場合、テクスチャ アドレス モードは、テクスチャ座標がテクセル位置を指定する方法を定義します。 たとえば、**TextureAddressMode.Clamp** を使用する場合、0 ～ 1 の範囲外の座標は、サンプリングの前に、最大値 1 および最小値 0 にクランプされます。

テクスチャが多角形に対して大きすぎるか小さすぎる場合、領域に合うようにテクスチャがフィルター処理されます。 拡大フィルターはテクスチャを拡大し、縮小フィルターは小さな領域に収まるようにテクスチャを縮小します。 テクスチャの拡大では、サンプル テクセルが 1 つまたは複数のアドレスに繰り返され、ぼやけた画像が生成されます。 複数のテクセル値を1つの値に結合する必要があるため、テクスチャの縮小はより複雑になります。 これによって、テクスチャ データに応じてエイリアシングやぎざぎざのエッジが発生する場合があります。 縮小の最も一般的なアプローチでは、ミップマップを使用します。 ミップマップは、複数レベルのテクスチャです。 各レベルのサイズは、前のレベルよりも小さい2の累乗で、1 x 1 のテクスチャになります。 縮小を使用すると、ゲームはレンダリング時に必要なサイズに最も近いミップマップ レベルを選択します。 

### <a name="the-basicloader-class"></a>BasicLoader クラス

**BasicLoader** は、ディスク上のファイルからのシェーダー、テクスチャ、メッシュの読み込みをサポートする、単純なローダー クラスです。 同期メソッドと非同期メソッドの両方を提供します。 このサンプルゲームでは、 `BasicLoader.h/.cpp` ファイルは**Utilities**フォルダーにあります。

詳細については、[BasicLoader](complete-code-for-basicloader.md) を参照してください。