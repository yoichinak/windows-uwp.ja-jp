---
title: OpenGL ES 2.0 と Direct3D のシェーダー パイプラインの比較
description: 概念的には、Direct3D 11 のシェーダー パイプラインは OpenGL ES 2.0 のそれとよく似ています。
ms.assetid: 3678a264-e3f9-72d2-be91-f79cd6f7c4ca
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, ゲーム, OpenGL, Direct3D, シェーダー パイプライン
ms.localizationpriority: medium
ms.openlocfilehash: 3f7f512ce0eeb793f999ce133f8a2c32c35cd1cd
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89165376"
---
# <a name="compare-the-opengl-es-20-shader-pipeline-to-direct3d"></a>OpenGL ES 2.0 と Direct3D のシェーダー パイプラインの比較




**重要な API**

-   [入力アセンブラー ステージ](/windows/desktop/direct3d11/d3d10-graphics-programming-guide-input-assembler-stage)
-   [頂点シェーダー ステージ](/previous-versions/bb205146(v=vs.85))
-   [ピクセル シェーダー ステージ](/previous-versions/bb205146(v=vs.85))

概念的には、Direct3D 11 のシェーダー パイプラインは OpenGL ES 2.0 のそれとよく似ています。 ただし、API の設計という点では、シェーダー ステージを作成、管理するための主要コンポーネントは、[**ID3D11Device1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1) と [**ID3D11DeviceContext1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) という 2 つのプライマリ インターフェイスに含まれています。 このトピックでは、OpenGL ES 2.0 の一般的なシェーダー パイプライン API パターンが、Direct3D 11 におけるこれらのインターフェイスの何に対応するかを説明します。

## <a name="reviewing-the-direct3d-11-shader-pipeline"></a>Direct3D 11 のシェーダー パイプラインについて


シェーダー オブジェクトは [**ID3D11Device1::CreateVertexShader**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createvertexshader) や [**ID3D11Device1::CreatePixelShader**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createpixelshader) などの [**ID3D11Device1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1) インターフェイスのメソッドで作成されます。

Direct3D 11 グラフィックス パイプラインは、[**ID3D11DeviceContext1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) インターフェイスのインスタンスで管理され、次のステージが含まれます。

-   [入力アセンブラーステージ](/windows/desktop/direct3d11/d3d10-graphics-programming-guide-input-assembler-stage)。 入力アセンブラー ステージでは、パイプラインにデータ (三角形、線、点) を提供します。 このステージをサポートする [**ID3D11DeviceContext1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) メソッドには、"IA" というプレフィックスが付きます。
-   [頂点シェーダー ステージ](/previous-versions/bb205146(v=vs.85)): 頂点シェーダー ステージでは頂点を処理し、通常は、変換、スキンの適用、照明の適用などの操作を実行します。 頂点シェーダーは常に 1 つの入力頂点を受け取り、1 つの出力頂点を生成します。 このステージをサポートする [**ID3D11DeviceContext1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) メソッドには、"VS" というプレフィックスが付きます。
-   [ストリーム出力ステージ](/windows/desktop/direct3d11/d3d10-graphics-programming-guide-output-stream-stage): ストリーム出力ステージでは、パイプラインからラスタライザーへの途中でメモリにプリミティブ データをストリーミングします。 データはラスタライザーにストリーミングするか、渡すことができます。 メモリにストリーミングされたデータは、CPU からの入力データまたはリード バックとして、パイプラインに再循環させることができます。 このステージをサポートする [**ID3D11DeviceContext1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) メソッドには、"SO" というプレフィックスが付きます。
-   [ラスタライザー ステージ](/windows/desktop/direct3d11/d3d10-graphics-programming-guide-rasterizer-stage): ラスタライザーは、プリミティブをトリミングし、ピクセル シェーダー用にプリミティブを準備して、ピクセル シェーダーを呼び出す方法を決定します。 ( [**である id3d11devicecontext::P SSetShader**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-pssetshader)を使用してピクセルシェーダーのステージを NULL に設定する)、深さとステンシルのテストを無効にする ( [**D3D11 \_ depth \_ ステンシル \_ DESC**](/windows/desktop/api/d3d11/ns-d3d11-d3d11_depth_stencil_desc)では、DepthEnable と StencilEnable を FALSE に設定する) ことをパイプラインに指示することにより、ラスタライズを無効にすることができます。 無効になっている間、ラスター化関連のパイプライン カウンターは更新されません。
-   [ピクセル シェーダー ステージ](/previous-versions/bb205146(v=vs.85)): ピクセル シェーダー ステージがプリミティブの補間データを受信し、カラーなどのピクセル単位のデータを生成します。 このステージをサポートする [**ID3D11DeviceContext1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) メソッドには、"PS" というプレフィックスが付きます。
-   [出力マージャー ステージ](/windows/desktop/direct3d11/d3d10-graphics-programming-guide-output-merger-stage): 出力マージャー ステージでは、各種出力データ (ピクセル シェーダー値、深度とステンシルの情報) をレンダー ターゲットおよび深度/ステンシル バッファーのコンテンツと結合し、最終的なパイプラインの結果を生成します。 このステージをサポートする [**ID3D11DeviceContext1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) メソッドには、"OM" というプレフィックスが付きます。

(ジオメトリ シェーダー、ハル シェーダー、テッセレーター、ドメイン シェーダーのステージもありますが、OpenGL ES 2.0 には類似するものがないので、ここでは説明しません) これらのステージのメソッドの詳しい一覧については、[**ID3D11DeviceContext**](/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext) と [**ID3D11DeviceContext1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) のリファレンス ページをご覧ください。 **ID3D11DeviceContext1** は Direct3D 11 向けに **ID3D11DeviceContext** を拡張したものです。

## <a name="creating-a-shader"></a>シェーダーの作成


Direct3D では、シェーダー リソースは、コンパイルと読み込みの前に作成されません。このリソースは HLSL が読み込まれたときに作成されます。 したがって、glCreateShader に直接似た関数はありません。これにより、特定の種類 (GL \_ 頂点 \_ シェーダーや gl フラグメントシェーダーなど) の初期化されたシェーダーリソースが作成され \_ \_ ます。 Direct3D では、シェーダーは HLSL が [**ID3D11Device1::CreateVertexShader**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createvertexshader) や [**ID3D11Device1::CreatePixelShader**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createpixelshader) などの特定の関数で読み込まれた後で作成されます。これらの関数は、パラメーターとして型とコンパイルされた HLSL を受け取ります。

| OpenGL ES 2.0  | Direct3D 11                                                                                                                                                                                                                                                             |
|----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| glCreateShader | コンパイルされたシェーダー オブジェクトが正常に読み込まれた後で [**ID3D11Device1::CreateVertexShader**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createvertexshader) と [**ID3D11Device1::CreatePixelShader**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createpixelshader) を呼び出し、それらに CSO をバッファーとして渡します。 |

 

## <a name="compiling-a-shader"></a>シェーダーのコンパイル


Direct3D シェーダーは、ユニバーサル Windows プラットフォーム (UWP) アプリでコンパイル済みシェーダーオブジェクト (cso) ファイルとしてプリコンパイルされ、Windows ランタイム file Api の1つを使用して読み込む必要があります。 (デスクトップ アプリは実行時にテキスト ファイルからのシェーダーや文字列をコンパイルできます)。CSO ファイルは、Microsoft Visual Studio プロジェクトに含まれる任意の .hlsl ファイルから作成され、同じ名前が保持されます。ただし、ファイル拡張子は .cso です。 出荷時に、これらがパッケージに含まれていることを確かめてください。

| OpenGL ES 2.0                          | Direct3D 11                                                                                                                                                                   |
|----------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| glCompileShader                        | 該当なし。 シェーダーを Visual Studio の .cso ファイルにコンパイルし、パッケージに含めます。                                                                                     |
| コンパイルの状態に glGetShaderiv を使用 | 該当なし。 コンパイルでエラーが発生した場合は、Visual Studio の FX Compiler (FXC) からのコンパイル出力を調べてください。 コンパイルが成功すると、対応する CSO ファイルが作成されます。 |

 

## <a name="loading-a-shader"></a>シェーダーの読み込み


シェーダーの作成に関するセクションで触れたように、Direct3D 11 では、対応する CSO ファイルがバッファーに読み込まれ、次の表のいずれかのメソッドに渡されたときに、シェーダーを作成します。

| OpenGL ES 2.0 | Direct3D 11                                                                                                                                                                                                                           |
|---------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ShaderSource  | コンパイルされたシェーダー オブジェクトが正常に読み込まれた後で [**ID3D11Device1::CreateVertexShader**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createvertexshader) と [**ID3D11Device1::CreatePixelShader**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createpixelshader) を呼び出します。 |

 

## <a name="setting-up-the-pipeline"></a>パイプラインの設定


OpenGL ES 2.0 には、実行のための複数のシェーダーを含む "シェーダー プログラム" オブジェクトがあります。 個々のシェーダーはシェーダー プログラム オブジェクトにアタッチされます。 ただし、Direct3D 11 では、レンダリング コンテキスト ([**ID3D11DeviceContext1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1)) を直接操作して、そのコンテキストでシェーダーを作成します。

| OpenGL ES 2.0   | Direct3D 11                                                                                   |
|-----------------|-----------------------------------------------------------------------------------------------|
| glCreateProgram | 該当なし。 Direct3D 11 では、シェーダー プログラム オブジェクト アブストラクションは使われません。                          |
| glLinkProgram   | 該当なし。 Direct3D 11 では、シェーダー プログラム オブジェクト アブストラクションは使われません。                          |
| glUseProgram    | 該当なし。 Direct3D 11 では、シェーダー プログラム オブジェクト アブストラクションは使われません。                          |
| glGetProgramiv  | 作成した、[**ID3D11DeviceContext1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) への参照を使います。 |

 

静的な [**D3D11CreateDevice**](/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice) メソッドを使って [**ID3D11DeviceContext1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) と [**ID3D11Device1**](/windows/desktop/api/d3d11_2/nn-d3d11_2-id3d11device2) のインスタンスを作成します。

``` syntax
Microsoft::WRL::ComPtr<ID3D11Device1>          m_d3dDevice;
Microsoft::WRL::ComPtr<ID3D11DeviceContext1>  m_d3dContext;

// ...

D3D11CreateDevice(
  nullptr, // Specify nullptr to use the default adapter.
  D3D_DRIVER_TYPE_HARDWARE,
  nullptr,
  creationFlags, // Set set debug and Direct2D compatibility flags.
  featureLevels, // List of feature levels this app can support.
  ARRAYSIZE(featureLevels),
  D3D11_SDK_VERSION, // Always set this to D3D11_SDK_VERSION for UWP apps.
  &device, // Returns the Direct3D device created.
  &m_featureLevel, // Returns feature level of device created.
  &m_d3dContext // Returns the device's immediate context.
);
```

## <a name="setting-the-viewports"></a>ビューポートの設定


Direct3D 11 のビューポートの設定は、OpenGL ES 2.0 でのビューポートの設定方法とよく似ています。 Direct3D 11 で、構成された[**CD3D11 \_ ビューポート**](/previous-versions/windows/desktop/legacy/jj151722(v=vs.85))を使用して[**である id3d11devicecontext:: rssetviewports**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-rssetviewports)を呼び出します。

Direct3D 11: ビューポートを設定します。

``` syntax
CD3D11_VIEWPORT viewport(
        0.0f,
        0.0f,
        m_d3dRenderTargetSize.Width,
        m_d3dRenderTargetSize.Height
        );
m_d3dContext->RSSetViewports(1, &viewport);
```

| OpenGL ES 2.0 | Direct3D 11                                                                                                                                  |
|---------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| glViewport    | [**CD3D11 \_ビューポート**](/previous-versions/windows/desktop/legacy/jj151722(v=vs.85))、 [**である id3d11devicecontext:: rssetviewports**ポート](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-rssetviewports) |

 

## <a name="configuring-the-vertex-shaders"></a>頂点シェーダーの構成


Direct3D 11 の頂点シェーダーの構成は、シェーダーの読み込み時に行われます。 Uniform は [**ID3D11DeviceContext1::VSSetConstantBuffers1**](/windows/desktop/api/d3d11_1/nf-d3d11_1-id3d11devicecontext1-vssetconstantbuffers1) を使って定数バッファーとして渡されます。

| OpenGL ES 2.0                    | Direct3D 11                                                                                               |
|----------------------------------|-----------------------------------------------------------------------------------------------------------|
| glAttachShader                   | [**ID3D11Device1::CreateVertexShader**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createvertexshader)                       |
| glGetShaderiv、glGetShaderSource | [**ID3D11DeviceContext1::VSGetShader**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-vsgetshader)                       |
| glGetUniformfv、glGetUniformiv   | [**ID3D11DeviceContext1::VSGetConstantBuffers1**](/windows/desktop/api/d3d11_1/nf-d3d11_1-id3d11devicecontext1-vsgetconstantbuffers1) |

 

## <a name="configuring-the-pixel-shaders"></a>ピクセル シェーダーの構成


Direct3D 11 のピクセル シェーダーの構成は、シェーダーの読み込み時に行われます。 Uniform は [**ID3D11DeviceContext1::PSSetConstantBuffers1**](/windows/desktop/api/d3d11_1/nf-d3d11_1-id3d11devicecontext1-pssetconstantbuffers1) を使って定数バッファーとして渡されます。

| OpenGL ES 2.0                    | Direct3D 11                                                                                               |
|----------------------------------|-----------------------------------------------------------------------------------------------------------|
| glAttachShader                   | [**ID3D11Device1::CreatePixelShader**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createpixelshader)                         |
| glGetShaderiv、glGetShaderSource | [**ID3D11DeviceContext1::PSGetShader**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-psgetshader)                       |
| glGetUniformfv、glGetUniformiv   | [**ID3D11DeviceContext1::PSGetConstantBuffers1**](/windows/desktop/api/d3d11_1/nf-d3d11_1-id3d11devicecontext1-psgetconstantbuffers1) |

 

## <a name="generating-the-final-results"></a>最終結果の生成


パイプラインが完了したら、バック バッファーにシェーダー ステージの結果を描画します。 Direct3D 11 では、このとき、描画コマンドを呼び出して結果をバック バッファーのカラー マップとして出力し、そのバック バッファーをディスプレイに送信します。これは Open GL ES 2.0 と同様です。

| OpenGL ES 2.0  | Direct3D 11                                                                                                                                                                                                                                         |
|----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| glDrawElements | [**ID3D11DeviceContext1::D raw**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-draw)、 [**ID3D11DeviceContext1::D rawindexed**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-drawindexed) (または、 \* [**ID3D11DeviceContext1**](/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext)のその他の描画メソッド)。 |
| eglSwapBuffers | [**IDXGISwapChain1::Present1**](/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-present1)                                                                                                                                                                              |

 

## <a name="porting-glsl-to-hlsl"></a>HLSL への GLSL の移植


GLSL と HLSL は、複合型のサポートと全体的な構文以外はそれほど違いません。 最も簡単な移植方法は、一般的な OpenGL ES 2.0 の命令と定義を HLSL の対応する要素にエイリアシングすることです。 グラフィックス インターフェイスでサポートされる HLSL の機能セットを表現するために、Direct3D ではシェーダー モデル バージョンを使います。OpenGL には、HLSL 向けに異なるバージョン仕様があります。 次の表では、他のバージョンの観点から、Direct3D 11 と OpenGL ES 2.0 に対して定義されているシェーダー言語機能セットについて、簡単に示します。

| シェーダー言語           | GLSL 機能バージョン                                                                                                                                                                                                      | Direct3D シェーダー モデル |
|---------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------|
| Direct3D 11 HLSL          | ～ 4.30。                                                                                                                                                                                                                    | SM 5.0                |
| OpenGL ES 2.0 向けの GLSL ES | 1.40。 OpenGL ES 2.0 向けの GLSL ES の以前の実装では、1.10 から 1.30 までを使用できます。 GlGetString (GL \_ シェーディング \_ 言語 \_ バージョン) または glGetString (シェーディング言語バージョン) を使用して元のコードを確認し、 \_ それを \_ 特定します。 | ～ SM 2.0               |

 

2 つのシェーダー言語の違いと一般的な構文のマッピングについて詳しくは、「[GLSL と HLSL の対応を示すリファレンス](glsl-to-hlsl-reference.md)」をご覧ください。

## <a name="porting-the-opengl-intrinsics-to-hlsl-semantics"></a>HLSL セマンティクスへの OpenGL 組み込みメソッドの移植


Direct3D 11 HLSL セマンティクスは、uniform や属性名と同様に、アプリとシェーダー プログラムの間で渡される値を識別するために使われる文字列です。 使用できる文字列はさまざまですが、用途を示す文字列 (POSITION、COLOR など) を使うことをお勧めします。 これらのセマンティクスは、定数バッファーまたはバッファー入力レイアウトを構成する際に割り当てます。 類似する値に別のレジスタを使うために、セマンティクスに 0 ～ 7 の範囲の数値を追加できます。 COLOR0、COLOR1、COLOR2 などです。

"SV" というプレフィックスが付いたセマンティクス \_ は、シェーダープログラムによって書き込まれるシステム値のセマンティクスです。 (CPU で実行されている) アプリ自体は変更できません。 通常、これらには、グラフィックス パイプラインの別のシェーダー ステージからの入出力値のほか、GPU のみによって生成された値が含まれます。

さらに、SV のセマンティクスでは、 \_ シェーダーステージに対する入力を指定したり、出力を指定したりするときに、異なる動作が使用されます。 たとえば、SV の \_ 位置 (出力) には頂点シェーダーステージ中に変換された頂点データが含まれ、sv の \_ 位置 (入力) にはラスタライズ中に補間されたピクセル位置の値が含まれます。

次に、一般的な OpenGL ES 2.0 シェーダー組み込みメソッドのマッピングをいくつか示します。

| OpenGL のシステム値 | 使用する HLSL セマンティック                                                                                                                                                   |
|---------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| gl の \_ 位置        | 頂点バッファー データを表す POSITION(n)。 SV \_ の位置はピクセルシェーダーへのピクセル位置を提供し、アプリで書き込むことはできません。                                        |
| gl \_ 標準          | 頂点バッファーによって提供される標準データを表す NORMAL(n)。                                                                                                                 |
| gl \_ texcoord \[ n\]   | シェーダーに提供されるテクスチャ UV (一部の OpenGL のドキュメントでは ST) 座標データを表す TEXCOORD(n)。                                                                       |
| gl の中の \_ 色       | シェーダーに提供される RGBA カラー データを表す COLOR(n)。 座標データと同様に処理されることに注意してください。このセマンティクスは、単に色データであることを示すために使用します。 |
| gl の \_ データの \[ n\]   | \_ \[ \] ピクセルシェーダーからターゲットテクスチャまたはその他のピクセルバッファーに書き込むための SV ターゲット n。                                                                               |

 

セマンティクスのコーディングに使うメソッドは、OpenGL ES 2.0 での組み込みメソッドの使用と同じではありません。 OpenGL では、構成または宣言なしで組み込みメソッドの多くに直接アクセスできます。Direct3D では、特定のセマンティクスを使うために、特定の定数バッファーでフィールドを宣言するか、シェーダーの **main()** メソッドの戻り値として宣言する必要があります。

定数バッファーの定義で使われるセマンティックの例を次に示します。

```cpp
struct VertexShaderInput
{
  float3 pos : POSITION;
  float3 color : COLOR0;
};

// The position is interpolated to the pixel value by the system. The per-vertex color data is also interpolated and passed through the pixel shader. 
struct PixelShaderInput
{
  float4 pos : SV_POSITION;
  float3 color : COLOR0;
};
```

このコードは、単純な定数バッファーのペアを定義します。

フラグメント シェーダーによって返される値を定義するために使われるセマンティックの例を次に示します。

```cpp
// A pass-through for the (interpolated) color data.
float4 main(PixelShaderInput input) : SV_TARGET
{
  return float4(input.color,1.0f);
}
```

この場合、[SV \_ target] はレンダーターゲットの場所で、シェーダーの実行が完了すると、ピクセルの色 (4 つの浮動小数点値を持つベクトルとして定義される) が書き込まれます。

Direct3D でのセマンティクスの使用について詳しくは、「[HLSL セマンティクス](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-semantics)」をご覧ください。

 

 