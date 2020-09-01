---
title: メイン ゲーム オブジェクトの定義
description: ここでは、サンプルゲームの主要なオブジェクトの詳細と、それが実装するルールがゲームワールドとの相互作用にどのように変換されるかについて説明します。
ms.assetid: 6afeef84-39d0-cb78-aa2e-2e42aef936c9
ms.date: 06/24/2020
ms.topic: article
keywords: Windows 10, UWP, ゲーム, メイン オブジェクト
ms.localizationpriority: medium
ms.openlocfilehash: 497a1f0dc16308b4b9360aff958b94f04b6283ae
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162996"
---
# <a name="define-the-main-game-object"></a>メイン ゲーム オブジェクトの定義

> [!NOTE]
> このトピックは、「DirectX チュートリアルシリーズ [を含む simple ユニバーサル Windows プラットフォーム (UWP) ゲームの作成](tutorial--create-your-first-uwp-directx-game.md) 」に含まれています。 このリンクのトピックでは、系列のコンテキストを設定します。

サンプルゲームの基本的なフレームワークをレイアウトし、高レベルのユーザーとシステムの動作を処理するステートマシンを実装したら、サンプルゲームをゲームに変えるルールと機構を検討します。 ここでは、サンプルゲームの主要なオブジェクトの詳細と、ゲームのルールをゲームワールドとの相互作用に変換する方法について説明します。

## <a name="objectives"></a>目的

- 基本的な開発手法を適用して、UWP DirectX ゲームのゲームルールと機構を実装する方法について説明します。

## <a name="main-game-object"></a>メイン ゲーム オブジェクト

**Simple3DGameDX**サンプルゲームでは、 **Simple3DGame**は主要な game オブジェクトクラスです。 **Simple3DGame**のインスタンスは、 **App:: Load**メソッドを使用して間接的に構築されます。

**Simple3DGame**クラスの機能の一部を次に示します。

- ゲームプレイロジックの実装が含まれます。
- これらの詳細を通知するメソッドが含まれています。
  - ゲーム状態の変更は、app framework で定義されているステートマシンに変わります。
  - ゲームの状態をアプリからゲームオブジェクト自体に変更します。
  - ゲームの UI (オーバーレイとヘッドアップディスプレイ)、アニメーション、および物理 (dynamics) の更新に関する詳細。
  > [!NOTE]
  > グラフィックスの更新は **GameRenderer** クラスによって処理されます。このクラスには、ゲームで使用されるグラフィックスデバイスリソースを取得して使用するメソッドが含まれています。 詳細については、「 [レンダリングフレームワーク I: レンダリングの概要](tutorial--assembling-the-rendering-pipeline.md)」を参照してください。
- ゲームの高レベルでの定義方法に応じて、ゲームセッション、レベル、または有効期間を定義するデータのコンテナーとして機能します。 この場合、ゲームの状態データはゲームの有効期間を示し、ユーザーがゲームを起動したときに1回初期化されます。

このクラスで定義されているメソッドとデータを表示するには、以下 [の Simple3DGame クラス](#the-simple3dgame-class) を参照してください。

## <a name="initialize-and-start-the-game"></a>ゲームを初期化して開始する

プレーヤーがゲームを開始すると、ゲーム オブジェクトはその状態を初期化し、オーバーレイの作成と追加を行い、プレーヤーのパフォーマンスを追跡する変数を設定して、レベルの構築時に使うオブジェクトをインスタンス化する必要があります。 このサンプルでは、 **GameMain** インスタンスが **App:: Load**で作成されるときにこれが実行されます。

**Simple3DGame**型の game オブジェクトが**GameMain:: GameMain**コンストラクターに作成されます。 次に、 **GameMain:: GameMain**から呼び出される**GameMain::** **Simple3DGame:** ::: コルーチンの実行中に、このメソッドを使用して初期化します。

### <a name="the-simple3dgameinitialize-method"></a>Simple3DGame:: Initialize メソッド

サンプルゲームでは、game オブジェクトでこれらのコンポーネントを設定します。

- 新規のオーディオ再生オブジェクトを作成します。
- 一連のレベル プリミティブ、弾薬、障害物を含む、ゲームのグラフィック プリミティブの配列を作成します。
- ゲーム状態データを保存する場所を作成し、*Game* という名前を付け、[**ApplicationData::Current**](/uwp/api/windows.storage.applicationdata.current) で指定するアプリ データ設定ストレージの場所に格納します。
- ゲーム タイマーと初期ゲーム内オーバーレイ ビットマップを作成します。
- 具体的なビュー パラメーターとプロジェクション パラメーター セットを使って新規のカメラを作成します。
- プレーヤーがコントロール開始位置とカメラ位置の 1 対 1 の対応を確保されるように、入力デバイス (コントローラー) をカメラと同じ位置に上下と左右の開始位置を設定します。
- プレーヤー オブジェクトを作成し、アクティブに設定します。 球体オブジェクトを使用して、壁や障害物に対するプレーヤーの距離を検出し、immersion が壊れる可能性のある位置にカメラが配置されないようにします。
- ゲーム ワールド プリミティブを作成します。
- 円筒形の障害物を作成します。
- 標的 (**Face** オブジェクト) を作成し、番号を付けます。
- 弾薬の球体を作成します。
- レベルを作成します。
- ハイ スコアを読み込みます。
- 以前に保存されたゲーム状態を読み込みます。

ゲームには、 &mdash; 世界、プレーヤー、障害物、ターゲット、ammo 球のすべての主要なコンポーネントのインスタンスが含まれるようになりました。 これらの全コンポーネントの設定と個々の固有レベルに対する動作を表すレベルのインスタンスもあります。 次に、ゲームによってどのようにレベルが構築されるかを見てみましょう。

## <a name="build-and-load-game-levels"></a>ゲームレベルのビルドと読み込み

レベルの構築の多くの処理は、 `Level[N].h/.cpp` サンプルソリューションの **GameLevels** フォルダーにあるファイルで行われます。 ここでは、非常に具体的な実装に焦点を当てているため、ここでは説明しません。 重要な点は、各レベルのコードが個別の **レベル [N]** オブジェクトとして実行されることです。 ゲームを拡張する場合は、割り当てられた数をパラメーターとして受け取り、障害やターゲットをランダムに配置する **Level [N]** オブジェクトを作成できます。 または、リソースファイルまたはインターネットからレベル構成データを読み込むこともできます。

## <a name="define-the-gameplay"></a>ゲームプレイを定義する

この時点で、ゲームを開発するために必要なすべてのコンポーネントが用意されています。 これらのレベルは、プリミティブからメモリ内に構築されており、プレーヤーが操作を開始する準備ができています。

最適なゲームは、プレーヤー入力にすぐに反応し、すぐにフィードバックを提供します。 これは、ゲームのあらゆる種類に当てはまります。これは、twitch、リアルタイムのファーストユーザーの連絡先から、よく使用されるターンベースの戦略ゲームまでです。

### <a name="the-simple3dgamerungame-method"></a>Simple3DGame:: RunGame メソッド

ゲームレベルが進行中の場合、ゲームは **Dynamics** 状態になります。 

**GameMain:: update** は、次に示すように、フレームごとに1回アプリケーションの状態を更新するメインの更新ループです。 Update ループは、 **Simple3DGame:: rungame** メソッドを呼び出して、ゲームが **Dynamics** 状態にある場合に作業を処理します。

```cppwinrt
// Updates the application state once per frame.
void GameMain::Update()
{
    // The controller object has its own update loop.
    m_controller->Update();

    switch (m_updateState)
    {
    ...
    case UpdateEngineState::Dynamics:
        if (m_controller->IsPauseRequested())
        {
            ...
        }
        else
        {
            // When the player is playing, work is done by Simple3DGame::RunGame.
            GameState runState = m_game->RunGame();
            switch (runState)
            {
                ...
```
          
**Simple3DGame:: RunGame** は、ゲームループの現在のイテレーションに対するゲーム再生の現在の状態を定義するデータのセットを処理します。

**Simple3DGame:: RunGame**のゲームフローロジックを次に示します。

- メソッドは、レベルが完了するまで秒をカウントするタイマーを更新し、レベルの時間が経過したかどうかをテストします。 これは、時間が経過したときのゲームのルールの1つです &mdash; 。すべてのターゲットがショットに含まれていない場合は、ゲームを実行します。
- 時間が経過した場合、メソッドは **Timeexpired 切れ** のゲーム状態を設定し、前のコードの **Update** メソッドに戻ります。
- 時間が残っている場合は、カメラの位置の更新のために移動ルックコントローラーがポーリングされます。具体的には、カメラ平面 (プレーヤーが見ている場所) から投影された通常のビューの角度と、コントローラーが最後にポーリングされてから角度が移動した距離を更新します。
- カメラは、ムーブ/ルック コントローラーから送られる新しいデータに従って更新されます。
- ダイナミクス、つまりプレーヤーのコントロールからは独立したゲーム ワールド中のオブジェクトのアニメーションや動作が更新されます。 このサンプルゲームでは、 **Simple3DGame:: UpdateDynamics** メソッドを呼び出して、発生した ammo 球の動き、柱の障害物とターゲットの動きを更新します。 詳細については、「 [game world を更新する](#update-the-game-world)」を参照してください。
- メソッドは、レベルが正常に完了したかどうかを確認します。 その場合は、レベルのスコアを終了し、これが最後のレベル (6) であるかどうかを確認します。 最後のレベルの場合、メソッドは、" **: GameComplete** game state" を返します。それ以外の場合は、game state **:: LevelComplete** game state を返します。
- レベルが完了していない場合、メソッドはゲームの状態を " **状態: アクティブ**" に設定し、を返します。

## <a name="update-the-game-world"></a>ゲームワールドを更新する

このサンプルでは、ゲームの実行中に、game シーンでレンダリングされるオブジェクトを更新するために、 **Simple3DGame:** : UpdateDynamics メソッド ( **GameMain:: Update**から呼び出される) メソッドから**Simple3DGame::** メソッドが呼び出されます。

**UpdateDynamics**などのループは、プレーヤーの入力に関係なくゲームの世界の動きを設定するために使用される任意のメソッドを呼び出して、イマーシブゲームエクスペリエンスを作成し、そのレベルを有効にします。 これには、表示する必要があるグラフィックスと、プレーヤー入力がない場合でも、動的な世界を取り込むためのアニメーションループの実行が含まれます。 ゲームでは、swaying のような木を含むことができます。これには、線、機械の喫煙、宇宙人未開の伸縮と移動が含まれます。 また、プレーヤーの球体とワールドの間、または弾薬、障害物、標的の間に生じる衝突を含め、物体どうしの相互作用も統合されます。

ゲームが特に一時停止している場合を除き、ゲームループはゲームの世界を更新し続けます。ゲームロジックや物理アルゴリズムに基づいているかどうか、または単純にランダムであるかどうか。

このサンプルゲームでは、この原則を *dynamics*と呼びます。この原理は、柱の ammo の台頭と動き、そして動きと動きに応じて、動きと物理的な動作を示しています。

### <a name="the-simple3dgameupdatedynamics-method"></a>Simple3DGame:: UpdateDynamics メソッド 

このメソッドは、これら4つの計算のセットを扱います。

- ワールドで発砲された弾薬の球体の位置
- 柱の障害物のアニメーション
- プレーヤーとワールドの境界の交差部分
- 弾薬の球体と、障害物、標的、他の弾薬球体、ワールドとの衝突

障害物のアニメーションは、 **アニメーション** のソースコードファイルで定義されているループ内で行われます。 Ammo とコリジョンの動作は、コードで提供される単純化された物理アルゴリズムによって定義され、重力やマテリアルのプロパティなど、ゲーム世界のグローバル定数のセットによってパラメーター化されます。 これはすべて、ゲーム ワールドの座標空間で計算されます。

### <a name="review-the-flow"></a>フローを確認する

これで、シーン内のすべてのオブジェクトが更新され、競合が計算されたので、この情報を使用して、対応するビジュアルの変更を描画する必要があります。 

**GameMain:: Update**がゲームループの現在の反復処理を完了した後、サンプルではすぐに**GameRenderer:: Render**を呼び出して、更新されたオブジェクトデータを取得し、次に示すように、プレーヤーに表示する新しいシーンを生成します。

```cppwinrt
void GameMain::Run()
{
    while (!m_windowClosed)
    {
        if (m_visible)
        {
            switch (m_updateState)
            {
            case UpdateEngineState::Deactivated:
            case UpdateEngineState::TooSmall:
                ...
                // Otherwise, fall through and do normal processing to perform rendering.
            default:
                CoreWindow::GetForCurrentThread().Dispatcher().ProcessEvents(
                    CoreProcessEventsOption::ProcessAllIfPresent);
                // GameMain::Update calls Simple3DGame::RunGame. If game is in Dynamics
                // state, uses Simple3DGame::UpdateDynamics to update game world.
                Update();
                // Render is called immediately after the Update loop.
                m_renderer->Render();
                m_deviceResources->Present();
                m_renderNeeded = false;
            }
        }
        else
        {
            CoreWindow::GetForCurrentThread().Dispatcher().ProcessEvents(
                CoreProcessEventsOption::ProcessOneAndAllPending);
        }
    }
    m_game->OnSuspending();  // Exiting due to window close, so save state.
}
```

## <a name="render-the-game-worlds-graphics"></a>ゲームワールドのグラフィックスをレンダリングする

ゲームのグラフィックスを頻繁に更新することをお勧めします。これは、メインのゲームループが反復処理されるのと同じくらいの頻度です。 ループが反復処理されると、ゲームの世界の状態が更新されます。プレイヤー入力はありません。 これにより、計算されたアニメーションと動作をスムーズに表示できます。 たとえば、プレーヤーがボタンを押したときにのみ移動される、水の単純なシーンがあるとします。 これは現実的ではありません。優れたゲームは、常に滑らかで滑らかに見えます。

**GameMain:: Run**の例に示すように、サンプルゲームのループを思い出してください。 ゲームのメインウィンドウが表示されていて、スナップまたは非アクティブ化されていない場合、ゲームは引き続き更新され、その更新の結果が表示されます。 次に調べる **GameRenderer:: Render** メソッドでは、その状態の表現がレンダリングされます。 これは、前のセクションで説明したように、 **GameMain:: update**の呼び出しの直後に実行されます。これには、 **Simple3DGame:: rungame** が含まれ、状態が更新されます。

**GameRenderer:: Render** は、3d ワールドの投影を描画し、その上に Direct2D オーバーレイを描画します。 描画が完了すると、表示用に結合されたバッファーとともに最終的なスワップ チェーンが表示されます。

> [!NOTE]
> サンプルゲームの Direct2D オーバーレイの2つの状態があります。ゲームでは、[ &mdash; 一時停止] メニューのビットマップを含んだゲーム情報オーバーレイと、タッチスクリーンの移動用の四角形と共に十字線を表示するゲーム情報オーバーレイが表示されます。 両方の状態でスコア テキストが描画されます。 詳細については、「[レンダリング フレームワーク I: レンダリングの概要](tutorial--assembling-the-rendering-pipeline.md)」を参照してください。

### <a name="the-gamerendererrender-method"></a>GameRenderer:: Render メソッド

```cppwinrt
void GameRenderer::Render()
{
    bool stereoEnabled{ m_deviceResources->GetStereoState() };

    auto d3dContext{ m_deviceResources->GetD3DDeviceContext() };
    auto d2dContext{ m_deviceResources->GetD2DDeviceContext() };

    ...
        if (m_game != nullptr && m_gameResourcesLoaded && m_levelResourcesLoaded)
        {
            // This section is only used after the game state has been initialized and all device
            // resources needed for the game have been created and associated with the game objects.
            ...
            for (auto&& object : m_game->RenderObjects())
            {
                object->Render(d3dContext, m_constantBufferChangesEveryPrim.get());
            }
        }

        d3dContext->BeginEventInt(L"D2D BeginDraw", 1);
        d2dContext->BeginDraw();

        // To handle the swapchain being pre-rotated, set the D2D transformation to include it.
        d2dContext->SetTransform(m_deviceResources->GetOrientationTransform2D());

        if (m_game != nullptr && m_gameResourcesLoaded)
        {
            // This is only used after the game state has been initialized.
            m_gameHud.Render(m_game);
        }

        if (m_gameInfoOverlay.Visible())
        {
            d2dContext->DrawBitmap(
                m_gameInfoOverlay.Bitmap(),
                m_gameInfoOverlayRect
                );
        }
        ...
    }
}
```

## <a name="the-simple3dgame-class"></a>Simple3DGame クラス

これらは、 **Simple3DGame** クラスによって定義されるメソッドとデータメンバーです。

### <a name="member-functions"></a>メンバー関数

**Simple3DGame**によって定義されるパブリックメンバー関数には、以下のものが含まれます。

- を**初期化**します。 グローバル変数の開始値を設定し、game オブジェクトを初期化します。 これについては、「 [ゲームの初期化と開始](#initialize-and-start-the-game) 」セクションを参照してください。
- **LoadGame**。 新しいレベルを初期化し、読み込みを開始します。
- **LoadLevelAsync**。 レベルを初期化してから、レンダラーで別のコルーチンを呼び出してデバイス固有のレベルリソースを読み込むコルーチン。 このメソッドは独立したスレッドで実行されます。そのため、このスレッドから呼び出すことができるのは [**ID3D11Device**](/windows/desktop/api/d3d11/nn-d3d11-id3d11device) メソッドだけです ([**ID3D11DeviceContext**](/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext) メソッドは呼び出されません)。 デバイス コンテキストのメソッドは、**FinalizeLoadLevel** メソッドで呼び出されます。 非同期プログラミングに慣れていない場合は、「 [C++/WinRT を使用した同時実行と非同期操作](../cpp-and-winrt-apis/concurrency.md)」を参照してください。
- **FinalizeLoadLevel**。 メイン スレッドで実行する必要があるレベル読み込みの作業を完了します。 これには、Direct3D 11 のデバイス コンテキスト ([**ID3D11DeviceContext**](/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext)) のメソッドの呼び出しが含まれます。
- **StartLevel**。 新しいレベルのゲームプレイを開始します。
- **PauseGame**。 ゲームを一時停止します。
- **RunGame**。 ゲーム ループの反復を実行します。 ゲームの状態が **App::Update** の場合、ゲーム ループを反復するごとに **Active** から 1 回呼び出されます。
- **OnSuspending** および **OnResuming**。 ゲームのオーディオをそれぞれ中断/再開します。

ここでは、プライベートメンバー関数について説明します。

- **LoadSavedState** および **SaveState**。 ゲームの現在の状態をそれぞれ読み込んで保存します。
- **Loadhighscore** と **savehighscore**。 ゲーム間でハイスコアをそれぞれ読み込み、保存します。
- **InitializeAmmo**。 弾として使われるそれぞれの球体の状態を各ラウンドの最初に元の状態に戻します。
- **UpdateDynamics**。 これは、組み込みのアニメーションルーチン、物理、およびコントロールの入力に基づいてすべてのゲームオブジェクトを更新するため、重要な方法です。 これが、ゲームを定義するインタラクティビティの中核部分に相当します。 これについては、「 [game world の更新](#update-the-game-world) 」セクションを参照してください。

他のパブリックメソッドは、ゲームプレイやオーバーレイ固有の情報を表示するためにアプリフレームワークに返すプロパティアクセサーです。

### <a name="data-members"></a>データ メンバー

これらのオブジェクトは、ゲームループの実行に応じて更新されます。

- **Movelookcontroller** オブジェクト。 プレーヤーの入力を表します。 詳細については、「[コントロールの追加](tutorial--adding-controls.md)」を参照してください。
- **GameRenderer** オブジェクト。 すべてのデバイス固有オブジェクトとそのレンダリングを処理する Direct3D 11 レンダラーを表します。 詳細については、「 [レンダリングフレームワーク I](tutorial--assembling-the-rendering-pipeline.md)」を参照してください。
- **Audio** オブジェクト。 ゲームのオーディオ再生を制御します。 詳細については、「 [サウンドの追加](tutorial--adding-sound.md)」を参照してください。

残りのゲーム変数には、プリミティブの一覧と、それぞれのゲーム内の量、およびゲーム再生に固有のデータと制約が含まれています。

## <a name="next-steps"></a>次の手順

実際のレンダリングエンジンについてはまだ説明してい &mdash; ません。更新されたプリミティブに対する **Render** メソッドの呼び出しが画面上のピクセルにどのように表示されるかについて説明します。 これらの側面については、レンダリング &mdash; [フレームワーク I: レンダリングの概要](tutorial--assembling-the-rendering-pipeline.md)と[レンダリングフレームワーク II: ゲームレンダリング](tutorial-game-rendering.md)の概要について説明します。 プレーヤーコントロールがゲームの状態を更新する方法に興味がある場合は、「 [コントロールの追加](tutorial--adding-controls.md)」を参照してください。