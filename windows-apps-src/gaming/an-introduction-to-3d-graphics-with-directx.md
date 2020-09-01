---
title: DirectX ゲームの基本的な 3D グラフィックス
description: DirectX プログラミングを使って、3D グラフィックスの基本的な概念を実装する方法について説明します。
ms.assetid: 2989c91f-7b45-7377-4e83-9daa0325e92e
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, ゲーム, DirectX, グラフィックス
ms.localizationpriority: medium
ms.openlocfilehash: 68ee694c036d4bc92f76ea75f7c5e5e530e26334
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173196"
---
# <a name="basic-3d-graphics-for-directx-games"></a>DirectX ゲームの基本的な 3D グラフィックス



DirectX プログラミングを使って、3D グラフィックスの基本的な概念を実装する方法について説明します。

**目標:** 3D グラフィックス アプリのプログラミング方法を身に付ける。

## <a name="prerequisites"></a>前提条件


C++ に習熟していることを前提としています。 また、グラフィックス プログラミングの概念に対する基礎的な知識も必要となります。

**完了までの合計時間:** 30 分。

## <a name="where-to-go-from-here"></a>次にすること


ここでは、DirectX と C++ Cx を使用して3D グラフィックスを開発する方法について説明し \\ ます。 この 5 つのパートから成るチュートリアルでは、[Direct3D](/windows/desktop/direct3d) API と、その他の DirectX サンプルの多くでも使われる概念やコードを紹介しています。 各パートでは、UWP の C++ アプリ向けに DirectX を構成することから始まって、プリミティブのテクスチャリングや効果の追加まで、段階的に構築していきます。

> **メモ**   このチュートリアルでは、列ベクターと共に右手座標系を使用します。 多くの DirectX サンプルと DirectX アプリでは、左手による座標系と行のベクターを使います。 より完全なグラフィックス数式ソリューションと、左手による座標系と行のベクターをサポートするグラフィックス数式ソリューションが必要な場合は、[DirectXMath](/windows/desktop/dxmath/directxmath-portal) を使うことを検討してください。 詳しくは、「[Direct3D との DirectXMath の使用](/windows/desktop/dxmath/pg-xnamath-migration-d3dx)」をご覧ください。

 

次の方法について説明します。

-   Windows ランタイムを使っての [Direct3D](/windows/desktop/direct3d) インターフェイスの初期化
-   頂点ごとのシェーダー操作の適用
-   ジオメトリの設定
-   シーンのラスタライズ (3D シーンを 2D プロジェクションにフラット化)
-   隠面のカリング

> **注**  

 

次に、Direct3D デバイス、スワップ チェーン、レンダー ターゲット ビューを作成し、レンダリングされた画像をディスプレイに表示します。

[クイック スタート: DirectX リソースの設定と画像の表示](setting-up-directx-resources.md)

## <a name="related-topics"></a>関連トピック


* [Direct3D 11 グラフィックス](/windows/desktop/direct3d11/atoc-dx-graphics-direct3d-11)
* [DXGI](/windows/desktop/direct3ddxgi/dx-graphics-dxgi)
* [HLSL](/windows/desktop/direct3dhlsl/dx-graphics-hlsl)

 

 