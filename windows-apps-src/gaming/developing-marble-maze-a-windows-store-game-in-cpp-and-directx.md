---
title: 開発 (*大理石* &mdash; )、C++ でビルドされた DirectX 用の迷路 (ユニバーサル Windows プラットフォーム) ゲーム
description: ドキュメントのこのセクションでは、DirectX と C++ を使用して3D ユニバーサル Windows プラットフォーム (UWP) ゲームを作成する方法について説明します。
ms.assetid: 43f1977a-7e1d-614c-696e-7669dd8a9cc7
ms.date: 08/10/2017
ms.topic: article
keywords: Windows 10, UWP, ゲーム, サンプル, DirectX, 3D
ms.localizationpriority: medium
ms.openlocfilehash: 155bd4bce77cd976cfab11ca4c5c8f184c562912
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89165336"
---
# <a name="developing-marble-mazemdasha-universal-windows-platform-uwp-game-built-with-c-for-directx"></a>開発 (*大理石* &mdash; )、C++ でビルドされた DirectX 用の迷路 (ユニバーサル Windows プラットフォーム) ゲーム

このトピックでは、DirectX と C++ を使用して3D ユニバーサル Windows プラットフォーム (UWP) ゲームを作成する方法について説明します。 *大理石迷路*と呼ばれるゲームは、タブレット、従来のデスクトップ pc、ラップトップ pc など、複数のフォームファクターを採用しています。

> [!NOTE]
> *大理石の迷路*ソースコードをダウンロードするには、 [GitHub のサンプル](https://github.com/microsoft/Windows-appsample-marble-maze)を参照してください。

> [!IMPORTANT]
> *大理石迷路* は、UWP ゲームを作成するためのベストプラクティスとして考慮する設計パターンを示しています。 各自のプラクティスと開発するゲームの固有の要件に適合するように、このゲームの実装の詳細を利用できます。 各自のニーズに適合する別のテクニックやライブラリがある場合はそれを自由にお使いください  (ただし、コードが [Windows アプリ認定キット](../debug-test-perf/windows-app-certification-kit.md)によるテストに合格することを常に確かめてください。) ゲームの開発を成功させるために Marble Maze の実装が不可欠であると見なされる場合は、このドキュメントでその点を強調しています。

## <a name="introducing-marble-maze"></a>*大理石迷路*の導入

これは比較的基本的なものであるため、 *大理石の迷路* を選択しましたが、ほとんどのゲームで見られる機能の幅は引き続き示しています。 Marble Maze は、グラフィックス、入力処理、オーディオの使用方法を示しています。 さらに、規則やゴールなどのゲームのメカニズムも示します。

*大理石迷路* はテーブルトップ labyrinth ゲームに似ています。通常は、穴や鉄鋼を含む箱から構成されています。 *大理石の迷路*の目標は、テーブルトップバージョンと同じです。迷路を傾けることによって、迷路の最初から最後までの時間をできるだけ少なくすることができます。また、大理石が穴のいずれかに落下することもありません。 *大理石の迷路* は、チェックポイントの概念を追加します。 大理石が穴に落ちた場合、ゲームは、大理石が通過した最後のチェックポイントの場所から再開されます。

*大理石迷路* には、ユーザーがゲームボードと対話するための複数の方法が用意されています。 タッチ対応または加速度計対応デバイスの場合は、これらのデバイスを使ってゲーム ボードを動かすことができます。 Xbox One コントローラーまたはマウスを使って、ゲームのプレイを制御することもできます。

![Marble Maze ゲームのスクリーン ショット。](images/marblemaze-2.png)

## <a name="prerequisites"></a>前提条件

-   Windows 10 Creators Update
-   [Microsoft Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)
-   C++ プログラミングの知識
-   DirectX と DirectX の用語に関する知識
-   COM に関する基本的な知識

## <a name="who-should-read-this"></a>対象読者

Windows 10 用の3D ゲームやその他のグラフィックを多用するアプリケーションの作成に関心がある場合は、この方法をお勧めします。 このドキュメントで説明する基本原則とプラクティスを活用して、各自の UWP ゲームを作成してください。 C++ と DirectX のプログラミングの知識または強い興味があれば、このドキュメントを活用するのに役に立ちます。 DirectX の経験がない場合でも、類似の 3D グラフィックスのプログラミング環境の経験があれば役に立ちます。

ドキュメント「[チュートリアル: DirectX によるシンプルな UWP ゲームの作成](tutorial--create-your-first-uwp-directx-game.md)」に、DirectX と C++ を使った基本的な 3D シューティング ゲームを実装するサンプルの説明があります。

## <a name="what-this-documentation-covers"></a>このドキュメントの内容

このドキュメントでは、以下の方法について説明します。

-   Windows ランタイム API と DirectX を使って UWP ゲームを作成する。
-   [Direct3D](/windows/desktop/direct3d11/atoc-dx-graphics-direct3d-11)と[Direct2D](/windows/desktop/Direct2D/direct2d-portal)を使用して、モデル、テクスチャ、頂点シェーダー、ピクセルシェーダー、2d オーバーレイなどのビジュアルコンテンツを操作します。
-   タッチ、加速度計、Xbox One コントローラーなどの入力メカニズムを統合する。
-   [XAudio2](/windows/desktop/xaudio2/xaudio2-apis-portal) を使って、音楽とサウンド エフェクトを組み込む。

## <a name="what-this-documentation-does-not-cover"></a>このドキュメントで扱われていない内容

このドキュメントでは、ゲーム開発の次の側面は扱いません。 これらの側面は、追加リソースで扱われます。

-   3D ゲームの設計原則。
-   C++ または DirectX プログラミングの基本。
-   テクスチャ、モデル、オーディオなどのリソースを設計する方法。
-   ゲームの動作またはパフォーマンスに関する問題をトラブルシューティングする方法。
-   海外で使用できるようにゲームを準備する方法。
-   ゲームを検証して Microsoft Store に公開する方法。

また、*大理石迷路*では、 [directxmath](/windows/desktop/dxmath/directxmath-portal)ライブラリを使用して3d ジオメトリを操作し、競合などの物理的な計算を実行します。 DirectXMath については、このセクションでは詳しく説明しません。 *大理石の迷路*で DirectXMath を使用する方法の詳細については、ソースコードを参照してください。

*大理石の迷路*には多くの再利用可能なコンポーネントが用意されていますが、完全なゲーム開発フレームワークではありません。 *大理石の迷路*コンポーネントがゲームで再利用できると考えられる場合は、ドキュメントで強調しています。

## <a name="next-steps"></a>次の手順

まず、大理石の迷路の [サンプルの基礎](marble-maze-sample-fundamentals.md) を使用して、大理石の迷路 *の構造、* および大理石の *迷路* のソースコードが従うコーディングとスタイルのガイドラインについて学習することをお勧めします。 次の表に、簡単に参照できるように、このセクションに含まれるドキュメントの概要を示します。

## <a name="in-this-section"></a>このセクションの内容

| Title                                                                                                                    | 説明                                                                                                                                                                                                                                        |
|--------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Marble Maze サンプルの基礎](marble-maze-sample-fundamentals.md)                                                   | ゲームの構造の概要と、ソース コードが従っているコーディング ガイドラインとスタイル ガイドラインの一部を示します。                                                                                                                                 |
| [Marble Maze のアプリケーション構造](marble-maze-application-structure.md)                                               | *大理石の迷路*アプリケーションコードがどのように構造化され、DirectX UWP アプリの構造が従来のデスクトップアプリケーションとどのように異なるかについて説明します。                                                                                    |
| [Marble Maze サンプルへの視覚的なコンテンツの追加](adding-visual-content-to-the-marble-maze-sample.md)                   | Direct3D と Direct2D を使うときに留意する主なプラクティスについて説明します。 また、 *大理石の迷路* がビジュアルコンテンツのこれらのプラクティスを適用する方法についても説明します。                                                                           |
| [Marble Maze サンプルへの入力と対話機能の追加](adding-input-and-interactivity-to-the-marble-maze-sample.md) | 加速度計、タッチ、Xbox の1つのコントローラー入力を使用して、ユーザーがメニューを操作してゲームボードと対話 *する方法について* 説明します。 入力を操作するときに留意するベスト プラクティスについても説明します。 |
| [Marble Maze のサンプルへのオーディオの追加](adding-audio-to-the-marble-maze-sample.md)                                     | 大理石の *迷路* がオーディオを使ってゲームエクスペリエンスに音楽やサウンドの効果を追加する方法について説明します。                                                                                                                                                  |