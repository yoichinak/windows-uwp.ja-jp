---
title: DirectX ユニバーサル Windows プラットフォーム (UWP) ゲームの作成
description: この一連のチュートリアルでは、DirectX と [C++/WinRT](../cpp-and-winrt-apis/index.md) を使用して、 **Simple3DGameDX**という名前の基本的なユニバーサル Windows プラットフォーム (UWP) サンプルゲームを作成する方法について説明します。
ms.assetid: 9edc5868-38cf-58cc-1fb3-8fb85a7ab2c9
keywords: DirectX sample game、sample game、ユニバーサル Windows プラットフォーム (UWP)、Direct3D 11 game
ms.date: 06/24/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 284aa821cc58a49f45bed3b0d7e28c20f9d19ba1
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89163026"
---
# <a name="create-a-simple-universal-windows-platform-uwp-game-with-directx"></a>DirectX を使った単純なユニバーサル Windows プラットフォーム (UWP) ゲームの作成

この一連のチュートリアルでは、DirectX と [C++/WinRT](../cpp-and-winrt-apis/index.md) を使用して、 **Simple3DGameDX**という名前の基本的なユニバーサル Windows プラットフォーム (UWP) サンプルゲームを作成する方法について説明します。 ゲームプレイは、単純なファーストユーザー3D 撮影ギャラリーで行われます。

> [!NOTE]
> **Simple3DGameDX**サンプルゲーム自体をダウンロードできるリンクは、 [Direct3D サンプルゲーム](/samples/microsoft/windows-universal-samples/simple3dgamedx/)です。 C++/WinRT ソースコードは、という名前のフォルダーに `cppwinrt` あります。 その他の UWP サンプルアプリの詳細については、「 [uwp アプリのサンプルの入手](../get-started/get-app-samples.md)」を参照してください。

これらのチュートリアルでは、アートやメッシュなどのアセットを読み込んだり、メインのゲームループを作成したり、単純なレンダリングパイプラインを実装したり、サウンドやコントロールを追加したりするためのプロセスなど、ゲームの主要な部分について説明します。

また、UWP ゲームの開発手法と考慮事項についても説明します。 ここでは、主要な UWP DirectX game development の概念に焦点を当て、これらの概念について、Windows ランタイム固有の考慮事項について説明します。

## <a name="objective"></a>目的

UWP DirectX ゲームの基本的な概念とコンポーネントについて説明すると共に、DirectX を使用して UWP ゲームをより快適に設計します。

## <a name="what-you-need-to-know"></a>知っておく必要がある情報

このチュートリアルでは、これらのサブジェクトについて理解している必要があります。

- [C++/WinRT](../cpp-and-winrt-apis/index.md)。 C++/winrt は、Windows ランタイム (WinRT) Api 向けの標準の最新 C++ 17 言語投影であり、ヘッダーファイルベースのライブラリとして実装されており、最新の Windows Api に対するファーストクラスのアクセスを提供するように設計されています。
- 線形代数およびニュートン物理学の基本的な概念。
- 基本的なグラフィックス プログラミング用語。
- Windows プログラミングの基本的な概念。
- [Direct2D](/windows/desktop/Direct2D/direct2d-portal) および [Direct3D 11](/windows/desktop/direct3d11/how-to-use-direct3d-11) API に関する基本的な知識。

##  <a name="direct3d-uwp-shooting-gallery-sample"></a>Direct3D UWP 撮影ギャラリーのサンプル

**Simple3DGameDX**サンプルゲームでは、単純なファースト person 3d 撮影ギャラリーが実装されています。プレーヤーは、移動先のボールを発射します。 標的に命中するたびにポイントが与えられ、プレイヤーは難度が上がっていく 6 つのレベルを進むことができます。 レベルの最後に、ポイントが集計されて、プレイヤーに最終スコアが与えられます。

このサンプルでは、これらのゲームの概念を示します。

- DirectX 11.1 と Windows ランタイムの間の相互運用
- 主観 3D 視点およびカメラ
- ステレオスコピック 3D 効果
- 衝突-3D のオブジェクト間の検出
- マウス、タッチ、Xbox コントローラーからのプレイヤーの入力の処理
- オーディオ ミキシングおよび再生
- 基本的なゲームの状態-マシン

![動作中のサンプルゲーム](images/simple-dx-game-overview.png)

|トピック|説明|
|-------|-------------|
|[ゲーム プロジェクトのセットアップ](tutorial--setting-up-the-games-infrastructure.md)|ゲームを開発するための最初の手順は、Microsoft Visual Studio でプロジェクトを設定することです。 ゲーム開発専用のプロジェクトを構成した後、後でテンプレートの一種として再利用できます。|
|[ゲームの UWP アプリ フレームワークの定義](tutorial--building-the-games-uwp-app-framework.md)|ユニバーサル Windows プラットフォーム (UWP) ゲームのコーディングの最初の手順は、アプリオブジェクトが Windows と対話できるようにするフレームワークを構築することです。|
|[ゲームのフロー管理](tutorial-game-flow-management.md)|プレイヤーとシステムとの対話を有効にする高度なステート マシンを定義します。 UI で全体的なゲームのステート マシンを操作する方法および UWP ゲーム用のイベント ハンドラーを作成する方法について説明します。|
|[メイン ゲーム オブジェクトの定義](tutorial--defining-the-main-game-loop.md)|ここでは、サンプルゲームの主要なオブジェクトの詳細と、それが実装するルールがゲームワールドとの相互作用にどのように変換されるかについて説明します。|
|[レンダリング フレームワーク I: レンダリングの概要](tutorial--assembling-the-rendering-pipeline.md)|レンダリングパイプラインを開発してグラフィックスを表示する方法について説明します。 レンダリングの概要。|
|[レンダリング フレームワーク II: ゲームのレンダリング](tutorial-game-rendering.md)|グラフィックスを表示するレンダリング パイプラインをアセンブルする方法について説明します。 ゲームのレンダリング、データのセットアップと準備。|
|[ユーザー インターフェイスの追加](tutorial--adding-a-user-interface.md)|DirectX UWP ゲームに2D ユーザーインターフェイスオーバーレイを追加する方法について説明します。|
|[コントロールの追加](tutorial--adding-controls.md)|ここでは、サンプルゲームが3d ゲームで移動外観コントロールを実装する方法と、基本的なタッチ、マウス、およびゲームコントローラーコントロールを開発する方法について説明します。|
|[サウンドの追加](tutorial--adding-sound.md)|XAudio2 Api を使用してゲームの音楽とサウンド効果を再生する単純なサウンドエンジンを開発します。|
|[サンプル ゲームを拡張する](tutorial-resources.md)|UWP DirectX ゲームの XAML オーバーレイを実装する方法について説明します。|