---
title: ゲームのフロー管理
description: ゲームの状態の初期化、イベントの処理、ゲームの更新ループのセットアップの方法について説明します。
ms.assetid: 6c33bf09-b46a-4bb5-8a59-ca83ce257eb3
ms.date: 06/24/2020
ms.topic: article
keywords: Windows 10, UWP, ゲーム, DirectX
ms.localizationpriority: medium
ms.openlocfilehash: 181eca743a9ccdc76ebfc1302e8bb04d85a32269
ms.sourcegitcommit: 20969781aca50738792631f4b68326f9171a3980
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/26/2020
ms.locfileid: "85409631"
---
# <a name="game-flow-management"></a>ゲームのフロー管理

> [!NOTE]
> このトピックは、「DirectX チュートリアルシリーズ[を含む simple ユニバーサル Windows プラットフォーム (UWP) ゲームの作成](tutorial--create-your-first-uwp-directx-game.md)」に含まれています。 このリンクのトピックでは、系列のコンテキストを設定します。

これで、ゲームにウィンドウが表示され、いくつかのイベントハンドラーが登録され、アセットが非同期に読み込まれました。 このトピックでは、ゲームの状態の使用方法、特定の主要なゲームの状態を管理する方法、およびゲームエンジンの更新ループを作成する方法について説明します。 次に、ユーザーインターフェイスフローについて説明し、最後に UWP ゲームに必要なイベントハンドラーについて詳しく説明します。

## <a name="game-states-used-to-manage-game-flow"></a>ゲームのフローを管理するために使用するゲームの状態

ゲームのフローを管理するには、ゲームの状態を使用します。

**Simple3DGameDX**サンプルゲームをコンピューターで初めて実行すると、ゲームが開始されていない状態になります。 その後、ゲームを実行すると、次のいずれかの状態になります。

- ゲームが開始されていないか、またはゲームがレベルの間にあります (高いスコアはゼロ)。
- ゲームループは実行中で、はレベルの中にあります。
- ゲームが完了したため、ゲームループは実行されていません (高スコアには0以外の値があります)。

ゲームは、必要なだけの状態を持つことができます。 ただし、いつでも終了できることに注意してください。 また、再開されると、ユーザーは、終了時の状態で再開することを想定しています。

## <a name="game-state-management"></a>ゲーム状態の管理

そのため、ゲームの初期化中に、ゲームのコールドスタートをサポートし、フライトを停止した後にゲームを再開する必要があります。 **Simple3DGameDX**サンプルでは、停止しないという印象を与えるために、常にゲーム状態を保存します。

中断イベントに応答して、ゲームプレイは中断されますが、ゲームのリソースはまだメモリ内にあります。 同様に、resume イベントは、サンプルゲームが中断または終了したときの状態で取得されるように処理されます。 状態に応じて、異なるオプションがプレーヤーに表示されます。 

- ゲームが途中で再開した場合は、[一時停止] と表示され、オーバーレイは続行するオプションを提供します。
- ゲームが完了した状態でゲームが再開されると、ハイスコアと新しいゲームをプレイするためのオプションが表示されます。
- 最後に、レベルが開始される前にゲームが再開した場合は、オーバーレイによってユーザーに開始オプションが表示されます。

サンプルゲームでは、ゲームがコールドスタートされているか、中断イベントが発生して初めて起動するのか、中断状態から再開しているのかを区別していません。 これは、どの UWP アプリにも適切な設計です。

このサンプルでは、ゲームの状態の初期化は[**GameMain::::**](#the-gamemaininitializegamestate-method)初期化された状態で発生します (そのメソッドの概要を次のセクションで説明します)。

フローを視覚化するためのフローチャートを次に示します。 初期化と更新ループの両方が対象となります。

- 初期化は、**Start** ノードから始まり、現在のゲームの状態をチェックします。 ゲームコードについては、次のセクションの「 [**GameMain:: 初期化**](#the-gamemaininitializegamestate-method)された状態」を参照してください。
* 更新ループについて詳しくは、「[ゲーム エンジンを更新する](#update-game-engine)」をご覧ください。 ゲームコードについては、 [**GameMain:: Update**](#the-gamemainupdate-method)にアクセスしてください。

![ゲームのメイン ステート マシン](images/simple-dx-game-flow-statemachine.png)

### <a name="the-gamemaininitializegamestate-method"></a>GameMain:: 初期化された状態メソッド

**GameMain::** **GameMain**クラスのコンストラクターを介して間接的に呼び出されます。これは、 **App:: Load**内で**GameMain**インスタンスを作成した結果です。

```cppwinrt
GameMain::GameMain(std::shared_ptr<DX::DeviceResources> const& deviceResources) : ...
{
    m_deviceResources->RegisterDeviceNotify(this);
    ...
    ConstructInBackground();
}

winrt::fire_and_forget GameMain::ConstructInBackground()
{
    ...
    m_renderer->FinalizeCreateGameDeviceResources();

    InitializeGameState();
    ...
}

void GameMain::InitializeGameState()
{
    // Set up the initial state machine for handling Game playing state.
    if (m_game->GameActive() && m_game->LevelActive())
    {
        // The last time the game terminated it was in the middle
        // of a level.
        // We are waiting for the user to continue the game.
        ...
    }
    else if (!m_game->GameActive() && (m_game->HighScore().totalHits > 0))
    {
        // The last time the game terminated the game had been completed.
        // Show the high score.
        // We are waiting for the user to acknowledge the high score and start a new game.
        // The level resources for the first level will be loaded later.
        ...
    }
    else
    {
        // This is either the first time the game has run or
        // the last time the game terminated the level was completed.
        // We are waiting for the user to begin the next level.
        ...
    }
    m_uiControl->ShowGameInfoOverlay();
}
```

## <a name="update-game-engine"></a>ゲーム エンジンを更新する

**App:: run**メソッドは**GameMain:: run**を呼び出します。 **GameMain:: Run**は、ユーザーが実行できる主要なアクションをすべて処理するための基本的なステートマシンです。 このステートマシンの最上位レベルでは、ゲームの読み込み、特定のレベルの再生、またはゲームが一時停止された後のレベル (システムまたはユーザーによる) の継続を処理します。

サンプルゲームでは、3つの主要な状態 ( **UpdateEngineState**列挙型によって表されます) がゲームに使用できます。

1. **UpdateEngineState:: WaitingForResources**。 ゲーム ループが循環していて、リソース (具体的にはグラフィックス リソース) が使用可能になるまで、移行はできません。 非同期のリソース読み込みタスクが完了したら、状態を**UpdateEngineState:: ResourcesLoaded**に更新します。 これは通常、レベルがディスク、ゲームサーバー、またはクラウドバックエンドから新しいリソースを読み込んだときに発生します。 サンプルゲームでは、この動作をシミュレートします。このサンプルでは、その時点で追加のレベルごとのリソースは必要ありません。
2. **UpdateEngineState:: WaitingForPress**。 ゲーム ループが循環していて、特定のユーザー入力を待機しています。 この入力は、ゲームの読み込み、レベルの開始、またはレベルの継続を行うプレーヤーアクションです。 このサンプルコードでは、 **Pressresultstate**列挙を使用してこれらのサブ状態を参照しています。
3. **UpdateEngineState::D ynamics**。 ゲーム ループが実行中で、ユーザーがゲームをしています。 ユーザーがプレイ中に、ゲームは移行できる 3 つの条件をチェックします。 
 - **状態:: TimeExpired 切れ**。 レベルの制限時間の有効期限。
 - お持ちの**状態:: LevelComplete**。 Player によるレベルの完了。
 - **GameComplete 状態::**。 プレーヤーによるすべてのレベルの完了。

ゲームとは、複数の小さいステートマシンを含むステートマシンのことです。 特定の状態は、非常に限定的な条件で定義する必要があります。 ある状態から別の状態への遷移は、個別のユーザー入力またはシステムアクション (グラフィックスリソースの読み込みなど) に基づいて行う必要があります。

ゲームを計画するときに、ユーザーまたはシステムが実行できるすべてのアクションに対処したことを確認するために、ゲームフロー全体を描画することを検討してください。 ゲームは非常に複雑になる可能性があるため、ステートマシンは、この複雑さを視覚化し、より管理しやすくするための強力なツールです。

Update ループのコードを見てみましょう。

### <a name="the-gamemainupdate-method"></a>GameMain:: Update メソッド

これは、ゲームエンジンを更新するために使用されるステートマシンの構造です。

```cppwinrt
void GameMain::Update()
{
    // The controller object has its own update loop.
    m_controller->Update(); 

    switch (m_updateState)
    {
    case UpdateEngineState::WaitingForResources:
        ...
        break;

    case UpdateEngineState::ResourcesLoaded:
        ...
        break;

    case UpdateEngineState::WaitingForPress:
        if (m_controller->IsPressComplete())
        {
            ...
        }
        break;

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
            case GameState::TimeExpired:
                ...
                break;

            case GameState::LevelComplete:
                ...
                break;

            case GameState::GameComplete:
                ...
                break;
            }
        }

        if (m_updateState == UpdateEngineState::WaitingForPress)
        {
            // Transitioning state, so enable waiting for the press event.
            m_controller->WaitForPress(
                m_renderer->GameInfoOverlayUpperLeft(),
                m_renderer->GameInfoOverlayLowerRight());
        }
        if (m_updateState == UpdateEngineState::WaitingForResources)
        {
            // Transitioning state, so shut down the input controller
            // until resources are loaded.
            m_controller->Active(false);
        }
        break;
    }
}
```

## <a name="update-the-user-interface"></a>ユーザーインターフェイスを更新する

プレイヤーには、システムの状態を継続的に通知して、プレイヤーの操作とゲームを定義するルールに応じて、ゲームの状態を変更できるようにする必要があります。 このサンプルゲームを含む多くのゲームでは、ユーザーインターフェイス (UI) 要素を使用して、この情報をプレーヤーに提示します。 UI には、ゲーム状態の表現と、スコア、ammo、残りの可能性の数などの再生固有の情報が含まれています。 UI は、メイングラフィックスパイプラインとは別にレンダリングされ、3D 投影の上に配置されるため、オーバーレイとも呼ばれます。

一部の UI 情報は、ヘッドアップディスプレイ (HUD) としても表示されます。これは、ユーザーがメインのゲームプレイ領域を完全にオフにせずにその情報を見ることができるようにするためです。 サンプルゲームでは、Direct2D Api を使用してこのオーバーレイを作成します。 または、このオーバーレイを XAML を使用して作成することもできます。これについては、[サンプルゲームの拡張](tutorial-resources.md)について説明します。

ユーザーインターフェイスには、2つのコンポーネントがあります。

- ゲームプレイの現在の状態に関するスコアと情報を格納している HUD。
- 一時停止ビットマップ。これは、ゲームの一時停止/中断状態中にテキストがオーバーレイされる黒の四角形です。 これがゲーム オーバーレイです。 これについては、「[ユーザー インターフェイスの追加](tutorial--adding-a-user-interface.md)」で詳しく説明します。

当然のことながら、オーバーレイにもステート マシンがあります。 オーバーレイには、レベルの開始メッセージやゲームオーバーメッセージが表示されます。 これは基本的に、ゲームが一時停止または中断しているときにプレーヤーに表示するゲームの状態に関する情報を出力できるキャンバスです。

表示されるオーバーレイは、ゲームの状態に応じて、これらの6つの画面のいずれかになります。

1. ゲーム開始時のリソース読み込みの進行状況画面。
2. ゲームプレイ統計画面。
3. レベルの開始メッセージ画面。
4. 時間をかけずにすべてのレベルを完了したときのゲーム画面。
5. 時間が経過したときのゲーム画面。
6. メニュー画面を一時停止します。

ユーザーインターフェイスをゲームのグラフィックスパイプラインから分離することで、ゲームのグラフィックスレンダリングエンジンとは関係なく作業を行い、ゲームのコードの複雑さを大幅に軽減できます。

ここでは、サンプルゲームによるオーバーレイのステートマシンの構造を示します。

```cppwinrt
void GameMain::SetGameInfoOverlay(GameInfoOverlayState state)
{
    m_gameInfoOverlayState = state;
    switch (state)
    {
    case GameInfoOverlayState::Loading:
        m_uiControl->SetGameLoading(m_loadingCount);
        break;

    case GameInfoOverlayState::GameStats:
        ...
        break;

    case GameInfoOverlayState::LevelStart:
        ...
        break;

    case GameInfoOverlayState::GameOverCompleted:
        ...
        break;

    case GameInfoOverlayState::GameOverExpired:
        ...
        break;

    case GameInfoOverlayState::Pause:
        ...
        break;
    }
}
```

## <a name="event-handling"></a>イベント処理

「[ゲームの UWP アプリフレームワークの定義](tutorial--building-the-games-uwp-app-framework.md)」で説明したように、 **app**クラスのビュープロバイダーメソッドの多くはイベントハンドラーを登録します。 これらのメソッドは、ゲーム機構を追加したり、グラフィックス開発を開始したりする前に、これらの重要なイベントを正しく処理する必要があります。

問題のイベントの適切な処理は、UWP アプリのエクスペリエンスの基礎となります。 UWP アプリはいつでもアクティブ化、非アクティブ化、サイズ変更、スナップ、スナップ解除、中断、または再開できます。そのため、ゲームは、可能な限り早くこれらのイベントを登録し、プレーヤーに対してスムーズで予測可能な方法で処理する必要があります。

これらは、このサンプルで使用されるイベントハンドラーと、それらが処理するイベントです。

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">イベント ハンドラー</th>
<th align="left">説明</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">OnActivated</td>
<td align="left"><a href="/uwp/api/windows.applicationmodel.core.coreapplicationview.activated"><strong>CoreApplicationView::Activated</strong></a> を処理します。 ゲーム アプリがフォアグラウンドに表示されているため、メイン ウィンドウがアクティブ化されます。</td>
</tr>
<tr class="even">
<td align="left">OnDpiChanged</td>
<td align="left"><a href="/uwp/api/windows.graphics.display.displayinformation#Windows_Graphics_Display_DisplayInformation_DpiChanged"><strong>Graphics::Display::DisplayInformation::DpiChanged</strong></a> を処理します。 ディスプレイの DPI が変更されていて、それに応じてゲームそのリソースを調整します。
<div class="alert">
<strong>注</strong> <a href="/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgifactory2-createswapchainforcorewindow"><strong>corewindow</strong></a>座標は、 <a href="/windows/desktop/Direct2D/direct2d-overview">Direct2D</a>のデバイスに依存しないピクセル (dip) です。 このため、2D アセットまたはプリミティブを正しく表示するには、Direct2D に DPI の変更を通知する必要があります。
</div>
<div>
</div></td>
</tr>
<tr class="odd">
<td align="left">OnOrientationChanged</td>
<td align="left"><a href="/uwp/api/windows.graphics.display.displayinformation#Windows_Graphics_Display_DisplayInformation_OrientationChanged"><strong>Graphics::Display::DisplayInformation::OrientationChanged</strong></a> を処理します。 ディスプレイの向きが変更され、レンダリングを更新する必要があります。</td>
</tr>
<tr class="even">
<td align="left">OnDisplayContentsInvalidated</td>
<td align="left"><a href="/uwp/api/windows.graphics.display.displayinformation#Windows_Graphics_Display_DisplayInformation_DisplayContentsInvalidated"><strong>Graphics::Display::DisplayInformation::DisplayContentsInvalidated</strong></a> を処理します。 ディスプレイを再描画する必要があり、ゲームをもう一度レンダリングする必要があります。</td>
</tr>
<tr class="odd">
<td align="left">OnResuming</td>
<td align="left"><a href="/uwp/api/windows.applicationmodel.core.coreapplication.resuming"><strong>CoreApplication::Resuming</strong></a> を処理します。 ゲーム アプリがゲームを中断状態から復元します。</td>
</tr>
<tr class="even">
<td align="left">OnSuspending</td>
<td align="left"><a href="/uwp/api/windows.applicationmodel.core.coreapplication.suspending"><strong>CoreApplication::Suspending</strong></a> を処理します。 ゲーム アプリがその状態をディスクに保存します。 ストレージへの状態の保存に使用できる時間は 5 秒です。</td>
</tr>
<tr class="odd">
<td align="left">OnVisibilityChanged</td>
<td align="left"><a href="/uwp/api/windows.ui.core.corewindow.visibilitychanged"><strong>CoreWindow::VisibilityChanged</strong></a> を処理します。 ゲーム アプリの表示が切り替わり、表示されるようになったか、別のアプリが表示されたために非表示になったことを示します。</td>
</tr>
<tr class="even">
<td align="left">OnWindowActivationChanged</td>
<td align="left"><a href="/uwp/api/windows.ui.core.corewindow.activated"><strong>CoreWindow::Activated</strong></a> を処理します。 ゲーム アプリのメイン ウィンドウが非アクティブ化またはアクティブ化されたため、フォーカスを動かしてゲームを一時停止するか、フォーカスを再取得する必要があります。 どちらの場合も、ゲームが一時停止されていることがオーバーレイに表示されます。</td>
</tr>
<tr class="odd">
<td align="left">OnWindowClosed</td>
<td align="left"><a href="/uwp/api/windows.ui.core.corewindow.closed"><strong>CoreWindow::Closed</strong></a> を処理します。 ゲーム アプリがメイン ウィンドウを閉じ、ゲームを中断します。</td>
</tr>
<tr class="even">
<td align="left">OnWindowSizeChanged</td>
<td align="left"><a href="/uwp/api/windows.ui.core.corewindow.sizechanged"><strong>CoreWindow::SizeChanged</strong></a> を処理します。 サイズ変更に応じてゲーム アプリがグラフィックス リソースとオーバーレイを再割り当てし、その後、レンダー ターゲットを更新します。</td>
</tr>
</tbody>
</table>

## <a name="next-steps"></a>次のステップ

このトピックでは、ゲームの状態を使用してゲームフロー全体がどのように管理されているか、およびゲームが複数の異なるステートマシンで構成されていることを説明しました。 また、UI を更新する方法や、キーアプリのイベントハンドラーを管理する方法についても説明しました。 これで、レンダリングループ、ゲーム、およびその機構を調べる準備ができました。
 
このゲームを任意の順序で文書化する残りのトピックを参照できます。

- [メイン ゲーム オブジェクトの定義](tutorial--defining-the-main-game-loop.md)
- [レンダリング フレームワーク I: レンダリングの概要](tutorial--assembling-the-rendering-pipeline.md)
- [レンダリング フレームワーク II: ゲームのレンダリング](tutorial-game-rendering.md)
- [ユーザー インターフェイスの追加](tutorial--adding-a-user-interface.md)
- [コントロールを追加する](tutorial--adding-controls.md)
- [サウンドの追加](tutorial--adding-sound.md)
