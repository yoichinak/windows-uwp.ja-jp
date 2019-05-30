---
title: DirectX ゲームの基本的な 3D グラフィックス
description: DirectX プログラミングを使って、3D グラフィックスの基本的な概念を実装する方法について説明します。
ms.assetid: 2989c91f-7b45-7377-4e83-9daa0325e92e
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, ゲーム, DirectX, グラフィックス
ms.localizationpriority: medium
ms.openlocfilehash: 556c5c74e5c8284e56047b4b8a9b2c2bf9c0155c
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/29/2019
ms.locfileid: "66369073"
---
# <a name="basic-3d-graphics-for-directx-games"></a>DirectX ゲームの基本的な 3D グラフィックス



DirectX プログラミングを使って、3D グラフィックスの基本的な概念を実装する方法について説明します。

**目標:** 3D グラフィックス アプリケーションのプログラミングについて説明します。

## <a name="prerequisites"></a>前提条件


C++ に習熟していることを前提としています。 また、グラフィックス プログラミングの概念に対する基礎的な知識も必要となります。

**完了する時間の合計:** 30 分。

## <a name="where-to-go-from-here"></a>次の段階


ここでは、DirectX および C++ での 3D グラフィックスを開発する方法について説明\\Cx します。 この 5 つのパートから成るチュートリアルでは、[Direct3D](https://docs.microsoft.com/windows/desktop/direct3d) API と、その他の DirectX サンプルの多くでも使われる概念やコードを紹介しています。 各パートでは、UWP の C++ アプリ向けに DirectX を構成することから始まって、プリミティブのテクスチャリングや効果の追加まで、段階的に構築していきます。

> **注**  このチュートリアルでは、列ベクターを使用した右辺座標系を使用します。 多くの DirectX サンプルと DirectX アプリでは、左手による座標系と行のベクターを使います。 より完全なグラフィックス数式ソリューションと、左手による座標系と行のベクターをサポートするグラフィックス数式ソリューションが必要な場合は、[DirectXMath](https://docs.microsoft.com/windows/desktop/dxmath/directxmath-portal) を使うことを検討してください。 詳しくは、「[Direct3D との DirectXMath の使用](https://docs.microsoft.com/windows/desktop/dxmath/pg-xnamath-migration-d3dx)」をご覧ください。

 

次の方法について説明します。

-   Windows ランタイムを使っての [Direct3D](https://docs.microsoft.com/windows/desktop/direct3d) インターフェイスの初期化
-   頂点ごとのシェーダー操作の適用
-   ジオメトリの設定
-   シーンのラスタライズ (3D シーンを 2D プロジェクションにフラット化)
-   隠面のカリング

> **注:**  

 

次に、Direct3D デバイス、スワップ チェーン、レンダー ターゲット ビューを作成し、レンダリングされた画像をディスプレイに表示します。

[クイック スタート: DirectX リソースの設定とイメージを表示します。](setting-up-directx-resources.md)

## <a name="related-topics"></a>関連トピック


* [Direct3D 11 のグラフィック](https://docs.microsoft.com/windows/desktop/direct3d11/atoc-dx-graphics-direct3d-11)
* [DXGI](https://docs.microsoft.com/windows/desktop/direct3ddxgi/dx-graphics-dxgi)
* [HLSL](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl)

 

 




