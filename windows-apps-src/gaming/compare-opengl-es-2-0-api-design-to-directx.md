---
title: OpenGL ES 2.0 から Direct3D への移植の計画
description: iOS または Android プラットフォームからゲームを移植している場合、OpenGL ES 2.0 に多大な投資を行ってこられたものと思われます。
ms.assetid: a31b8c5a-5577-4142-fc60-53217302ec3a
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10、UWP、ゲーム、OpenGL、Direct3D
ms.localizationpriority: medium
ms.openlocfilehash: dddbf3a1009ccbaeb7fd0695d995cd17ba6182d3
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89165346"
---
# <a name="plan-your-port-from-opengl-es-20-to-direct3d"></a>OpenGL ES 2.0 から Direct3D への移植の計画




**重要な API**

-   [Direct3D 11](/windows/desktop/direct3d11/atoc-dx-graphics-direct3d-11)
-   [Visual C++](/previous-versions/60k1461a(v=vs.140))

iOS または Android プラットフォームからゲームを移植している場合、OpenGL ES 2.0 に多大な投資を行ってこられたものと思われます。 グラフィックス パイプラインのコードベースを Direct3D 11 と Windows ランタイムに移す準備をしているときは、開始する前に何点か注意してください。

ほとんどの移植作業では、通常、最初にコードベースを調べて、2 つのモデル間で共通する API とパターンをマップします。 このトピックを読んで内容を確かめれば、このプロセスが少し楽になるはずです。

OpenGL ES 2.0 から Direct3D 11 にグラフィックスを移植する際に注意する必要のある事柄を次に示します。

## <a name="notes-on-specific-opengl-es-20-providers"></a>特定の OpenGL ES 2.0 プロバイダーに関する注意事項


このセクションの移植に関するトピックでは、Khronos Group によって作成された OpenGL ES 2.0 仕様の Windows 実装を参照します。 OpenGL ES 2.0 コード サンプルは、いずれも Visual Studio 2012 と基本的な Windows C 構文を使って開発されたものです。 Objective-C (iOS) または Java (Android) コードベースから移植する場合は、用意されている OpenGL ES 2.0 コード サンプルで、類似の API 呼び出し構文またはパラメーターが使われていない可能性があることに注意してください。 このガイダンスでは、できるだけプラットフォームにとらわれずに説明します。

このドキュメントでは、OpenGL ES のコードと参照に 2.0 仕様の API のみを使います。 OpenGL ES 1.1 または 3.0 から移植する場合もこのコンテンツは有用ですが、OpenGL ES 2.0 のコード例とコンテキストの一部に馴染みがない可能性があります。

これらのトピックの Direct3D 11 のサンプルでは、Microsoft Windows C++ とコンポーネント拡張 (CX) を使います。 このバージョンの C++ 構文の詳細については、「[ランタイムプラットフォーム用のコンポーネント拡張](/cpp/windows/component-extensions-for-runtime-platforms)」および「[クイックリファレンス (c++ \\ CX)](/cpp/cppcx/quick-reference-c-cx)」を参照してください[Visual C++](/previous-versions/60k1461a(v=vs.140))。

## <a name="understand-your-hardware-requirements-and-resources"></a>ハードウェア要件とリソースについて


OpenGL ES 2.0 でサポートされる一連のグラフィックス処理機能は、Direct3D 9.1 で提供される機能にほぼ対応しています。 Direct3D 11 で提供される高度な機能を利用する場合は、移植の計画段階では [Direct3D 11](/windows/desktop/direct3d11/atoc-dx-graphics-direct3d-11) のドキュメントを、最初の作業を終えた段階では「[DirectX 9 からユニバーサル Windows プラットフォーム (UWP) への移植](porting-your-directx-9-game-to-windows-store.md)」のトピックをご覧ください。

初期移植作業を簡単にするには、Visual Studio の Direct3D テンプレートを利用してください。 基本的なレンダラーが既に構成されており、ウィンドウの変更と Direct3D 機能レベルに関するリソースの再作成などの UWP アプリ機能がサポートされています。

## <a name="understand-direct3d-feature-levels"></a>Direct3D の機能レベルについて


Direct3D 11 では、 \_ 11 1 (direct3d 9.1) のハードウェアの "機能レベル" がサポートされて \_ います。 これらの機能レベルは、特定のグラフィックス機能とリソースの可用性を示します。 通常、ほとんどの OpenGL ES 2.0 プラットフォームは、Direct3D 9.1 (機能レベル 9 \_ 1) の機能セットをサポートしています。

## <a name="review-directx-graphics-features-and-apis"></a>DirectX のグラフィックス機能と API の確認


| API ファミリ                                                | 説明                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|-----------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [DXGI](/windows/desktop/direct3ddxgi/dx-graphics-dxgi)                     | DirectX Graphic Infrastructure (DXGI) は、グラフィックス ハードウェアと Direct3D の間のインターフェイスを提供します。 これは [**IDXGIAdapter**](/windows/desktop/api/dxgi/nn-dxgi-idxgiadapter) と [**IDXGIDevice1**](/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgidevice2) の COM インターフェイスを使ってデバイス アダプターとハードウェア構成を設定します。 バッファーなどのウィンドウ リソースを作成および構成する場合に使います。 特に、[**IDXGIFactory2**](/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgifactory2) ファクトリ パターンは、スワップ チェーン (フレーム バッファーのセット) などのグラフィックス リソースを取得するために使われます。 DXGI がスワップ チェーンを所有するため、画面にフレームを表示するために [**IDXGISwapChain1**](/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgiswapchain1) インターフェイスが使われます。 |
| [Direct3D](/windows/desktop/direct3d11/atoc-dx-graphics-direct3d-11)       | Direct3D は API のセットであり、グラフィックス インターフェイスの仮想表示を提供し、それを使ってグラフィックスを描画できるようにします。 バージョン 11 は、機能的に OpenGL 4.3 とほぼ同等です  (一方、OpenGL ES 2.0 は機能的に DirectX9 および OpenGL 2.0 と似ていますが、OpenGL 3.0 の統合シェーダー パイプラインがあります)。重要な作業の大半は、個々のリソースとサブリソースへのアクセスを提供する ID3D11Device1 インターフェイスと、レンダリング コンテキストを提供する ID3D11DeviceContext1 インターフェイスで行われます。                                                                                                                                          |
| [Direct2D](/windows/desktop/Direct2D/direct2d-portal)                      | Direct2D は、GPU アクセラレーションが活用された 2D レンダリング用の API のセットです。 用途は OpenVG と似ています。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| [DirectWrite](/windows/desktop/DirectWrite/direct-write-portal)            | DirectWrite は、GPU アクセラレーションが活用された高品質なフォント レンダリング用の API のセットです。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| [DirectXMath](/windows/desktop/dxmath/directxmath-portal)                  | DirectXMath は、一般的な線形代数と三角法の種類、値、関数を処理するための API とマクロのセットを提供します。 これらの型と関数は、Direct3D とそのシェーダーの操作に対応しています。                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| [DirectX HLSL](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-common-core) | Direct3D シェーダーで使われる現在の HLSL 構文です。 Direct3D Shader Model 5.0 を実装します。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |

 

## <a name="review-the-windows-runtime-apis-and-template-library"></a>Windows ランタイム API とテンプレート ライブラリの確認


Windows ランタイム API は、UWP アプリの全体的なインフラストラクチャを提供します。 詳しくは、[ここ](/uwp/api/) をご覧ください。

グラフィックス パイプラインの移植で使われる主要な Windows ランタイム API は次のとおりです。

-   [**Windows:: UI:: Core:: CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow)
-   [**Windows:: UI:: Core:: CoreDispatcher**](/uwp/api/Windows.UI.Core.CoreDispatcher)
-   [**Windows::ApplicationModel::Core::IFrameworkView**](/uwp/api/Windows.ApplicationModel.Core.IFrameworkView)
-   [**Windows::ApplicationModel::Core::CoreApplicationView**](/uwp/api/Windows.ApplicationModel.Core.CoreApplicationView)

また、Windows ランタイム C++ テンプレート ライブラリ (WRL) は、Windows ランタイム コンポーネントを作成して使うための下位レベルの方法を提供するテンプレート ライブラリです。 UWP アプリ用の Direct3D 11 API は、スマート ポインター ([ComPtr](/cpp/windows/comptr-class)) など、このライブラリ内のインターフェイスおよび型と組み合わせたときに最適に動作します。 WRL について詳しくは、「[Windows ランタイム C++ テンプレート ライブラリ (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl)」をご覧ください。

## <a name="change-your-coordinate-system"></a>座標系の変更


ときとして初期移植作業に混乱を招く違いは、OpenGL の従来の右手による座標系から Direct3D の既定の左手による座標系への変更です。 座標モデルにおけるこの変更は、頂点バッファーの設定と構成から、マトリックスの数学関数の多くに至るまで、ゲームの多くの部分に影響します。 2 つの最も重要な変更は次のとおりです。

-   三角形の頂点の順序を逆にし、Direct3D が前から時計回りにそれらをスキャンするようにします。 たとえば、頂点のインデックスが OpenGL パイプラインで 0、1、2 のとき、Direct3D に 0、2、1 として渡します。
-   効果的に z 軸座標を反転させるために、ビュー マトリックスを使って z 方向に -1.0f だけワールド空間を拡大します。 そのためには、ビュー マトリックスの位置 M31、M32、M33 にある値の符号を逆にします ([**Matrix**](/windows/desktop/direct3d9/matrix4x4) 型に移植するとき)。 M34 が 0 でない場合は、その符号を同様に逆にします。

ただし、Direct3D は右手による座標系をサポートできます。 DirectXMath は、左手による座標系と右手による座標系を操作するさまざまな関数を提供します。 元のメッシュ データおよびマトリックスの処理の一部を維持するために使うことができます。 それには以下が含まれます。

| DirectXMath のマトリックス関数                                                   | 説明                                                                                                                 |
|-------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------|
| [**XMMatrixLookAtLH**](/windows/desktop/api/directxmath/nf-directxmath-xmmatrixlookatlh)                               | カメラの位置、上方向、焦点を使って、左手による座標系のビュー マトリックスを作成します。       |
| [**XMMatrixLookAtRH**](/windows/desktop/api/directxmath/nf-directxmath-xmmatrixlookatrh)                               | カメラの位置、上方向、焦点を使って、右手による座標系のビュー マトリックスを作成します。      |
| [**XMMatrixLookToLH**](/windows/desktop/api/directxmath/nf-directxmath-xmmatrixlooktolh)                               | カメラの位置、上方向、カメラの向きを使って、左手による座標系のビュー マトリックスを作成します。  |
| [**XMMatrixLookToRH**](/windows/desktop/api/directxmath/nf-directxmath-xmmatrixlooktorh)                               | カメラの位置、上方向、カメラの向きを使って、右手による座標系のビュー マトリックスを作成します。 |
| [**XMMatrixOrthographicLH**](/windows/desktop/api/directxmath/nf-directxmath-xmmatrixorthographiclh)                   | 左手による座標系の正投影マトリックスを作成します。                                                 |
| [**XMMatrixOrthographicOffCenterLH**](/windows/desktop/api/directxmath/nf-directxmath-xmmatrixorthographicoffcenterlh) | 左手による座標系のカスタム正投影マトリックスを作成します。                                           |
| [**XMMatrixOrthographicOffCenterRH**](/windows/desktop/api/directxmath/nf-directxmath-xmmatrixorthographicoffcenterrh) | 右手による座標系のカスタム正投影マトリックスを作成します。                                          |
| [**XMMatrixOrthographicRH**](/windows/desktop/api/directxmath/nf-directxmath-xmmatrixorthographicrh)                   | 右手による座標系の正投影マトリックスを作成します。                                                |
| [**XMMatrixPerspectiveFovLH**](/windows/desktop/api/directxmath/nf-directxmath-xmmatrixperspectivefovlh)               | 視野に基づいて左手による遠近投影マトリックスを作成します。                                                |
| [**XMMatrixPerspectiveFovRH**](/windows/desktop/api/directxmath/nf-directxmath-xmmatrixperspectivefovrh)               | 視野に基づいて右手による遠近投影マトリックスを作成します。                                               |
| [**XMMatrixPerspectiveLH**](/windows/desktop/api/directxmath/nf-directxmath-xmmatrixperspectivelh)                     | 左手による遠近投影マトリックスを作成します。                                                                         |
| [**XMMatrixPerspectiveOffCenterLH**](/windows/desktop/api/directxmath/nf-directxmath-xmmatrixperspectiveoffcenterlh)   | 左手による遠近投影マトリックスのカスタム バージョンを作成します。                                                     |
| [**XMMatrixPerspectiveOffCenterRH**](/windows/desktop/api/directxmath/nf-directxmath-xmmatrixperspectiveoffcenterrh)   | 右手による遠近投影マトリックスのカスタム バージョンを作成します。                                                    |
| [**XMMatrixPerspectiveRH**](/windows/desktop/api/directxmath/nf-directxmath-xmmatrixperspectiverh)                     | 右手による遠近投影マトリックスを作成します。                                                                        |

 

## <a name="opengl-es20-to-direct3d-11-porting-frequently-asked-questions"></a>OpenGL ES2.0 から Direct3D 11 への移植についてよく寄せられる質問


-   質問: "通常、自分の OpenGL コードで特定の文字列やパターンを検索し、それを対応する Direct3D の要素と置き換えることはできますか"
-   回答: いいえ。 OpenGL ES 2.0 と Direct3D 11 は、グラフィックス パイプライン モデルの異なる世代に基づいています。 レンダリング コンテキストやシェーダーのインスタンス化など、概念と API の間に表面的な類似点はありますが、このガイダンスと Direct3D 11 のリファレンスを調べて、1 対 1 のマッピングを試みる代わりに、パイプラインを再作成するときに最適な選択ができるようにしてください。 ただし、GLSL から HLSL に移植する場合、GLSL の変数、組み込みメソッド、関数の一連の共通エイリアスを作成すると、移植が容易になるだけでなく、1 ペアのシェーダー コード ファイルだけを維持することができます。

 

 