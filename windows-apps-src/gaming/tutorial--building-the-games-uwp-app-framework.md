---
title: ゲームの UWP アプリ フレームワークの定義
description: ユニバーサル Windows プラットフォーム (UWP) ゲームのコーディングの最初の手順は、アプリオブジェクトが Windows と対話できるようにするフレームワークを構築することです。
ms.assetid: 7beac1eb-ba3d-e15c-44a1-da2f5a79bb3b
ms.date: 06/24/2020
ms.topic: article
keywords: Windows 10, UWP, ゲーム, DirectX
ms.localizationpriority: medium
ms.openlocfilehash: 2d762aebeaa5c97c1b23a91f0765cb5c09a91b46
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89163056"
---
#  <a name="define-the-games-uwp-app-framework"></a>ゲームの UWP アプリ フレームワークの定義

> [!NOTE]
> このトピックは、「DirectX チュートリアルシリーズ [を含む simple ユニバーサル Windows プラットフォーム (UWP) ゲームの作成](tutorial--create-your-first-uwp-directx-game.md) 」に含まれています。 このリンクのトピックでは、系列のコンテキストを設定します。

ユニバーサル Windows プラットフォーム (UWP) ゲームのコーディングの最初の手順は、アプリオブジェクトが Windows と対話できるようにするフレームワークを構築することです。これには、中断-再開イベント処理、ウィンドウフォーカスの変更、スナップなどの Windows ランタイム機能が含まれます。

## <a name="objectives"></a>目的

* ユニバーサル Windows プラットフォーム (UWP) DirectX ゲーム用のフレームワークを設定し、ゲームフロー全体を定義するステートマシンを実装します。

> [!NOTE]
> このトピックに従うには、ダウンロードした [Simple3DGameDX](/samples/microsoft/windows-universal-samples/simple3dgamedx/) サンプルゲームのソースコードを参照してください。

## <a name="introduction"></a>はじめに

「 [Game プロジェクトのセットアップ](tutorial--setting-up-the-games-infrastructure.md) 」トピックでは、 **Wwinmain** 関数と [**Iframeworkviewsource**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource) および [**IFrameworkView**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource) インターフェイスを紹介しました。 **App**クラス (Simple3DGameDX プロジェクトのソースコードファイルで定義されて `App.cpp` います) は、 **Simple3DGameDX** *ビュープロバイダーファクトリ*と*ビュープロバイダー*の両方として機能することを学習しました。

このトピックでは、この記事から、 **IFrameworkView**のメソッドを実装する必要があるゲームの**App**クラスについてさらに詳しく説明します。

## <a name="the-appinitialize-method"></a>App:: Initialize メソッド

アプリケーションの起動後、Windows が呼び出す最初のメソッドは、 [**IFrameworkView:: Initialize**](/uwp/api/windows.applicationmodel.core.iframeworkview.initialize)の実装です。

実装では、これらのイベントをサブスクライブすることで、ゲームが中断 (および後で再開可能) イベントを処理できるようにするなど、UWP ゲームの最も基本的な動作を処理する必要があります。 ここではディスプレイアダプターデバイスにもアクセスできるため、デバイスに依存するグラフィックスリソースを作成できます。

```cppwinrt
void Initialize(CoreApplicationView const& applicationView)
{
    applicationView.Activated({ this, &App::OnActivated });

    CoreApplication::Suspending({ this, &App::OnSuspending });

    CoreApplication::Resuming({ this, &App::OnResuming });

    // At this point we have access to the device. 
    // We can create the device-dependent resources.
    m_deviceResources = std::make_shared<DX::DeviceResources>();
}
```

可能な場合は常に生のポインターを使用しないようにします (ほとんどの場合は可能です)。

- Windows ランタイム型の場合は、ポインターを完全に回避し、スタックに値を構築するだけでよく使用できます。 ポインターが必要な場合は、 [**winrt:: com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr) を使用します (これについては、すぐに例を示します)。
- 一意のポインターの場合は、 [**std:: unique_ptr**](/cpp/cpp/how-to-create-and-use-unique-ptr-instances) と **std:: make_unique**を使用します。
- 共有ポインターの場合は、 [**std:: shared_ptr**](/cpp/cpp/how-to-create-and-use-shared-ptr-instances) と **std:: make_shared**を使用します。

## <a name="the-appsetwindow-method"></a>App:: SetWindow メソッド

**初期化**後、Windows は[**IFrameworkView:: setwindow**](/uwp/api/windows.applicationmodel.core.iframeworkview.setwindow)の実装を呼び出し、ゲームのメインウィンドウを表す[**corewindow**](/uwp/api/windows.ui.core.corewindow)オブジェクトを渡します。

**App:: SetWindow**では、ウィンドウ関連のイベントをサブスクライブし、いくつかのウィンドウと表示動作を構成します。 たとえば、マウスとタッチの両方のコントロールで使用できるマウスポインターを ( [**CoreCursor**](/uwp/api/windows.ui.core.corecursor) クラスを使用して) 作成します。 また、ウィンドウオブジェクトをデバイスに依存するリソースオブジェクトに渡します。

イベントの処理については、 [ゲームフロー管理](tutorial-game-flow-management.md#event-handling) に関するトピックで詳しく説明します。

```cppwinrt
void SetWindow(CoreWindow const& window)
{
    //CoreWindow window = CoreWindow::GetForCurrentThread();
    window.Activate();

    window.PointerCursor(CoreCursor(CoreCursorType::Arrow, 0));

    PointerVisualizationSettings visualizationSettings{ PointerVisualizationSettings::GetForCurrentView() };
    visualizationSettings.IsContactFeedbackEnabled(false);
    visualizationSettings.IsBarrelButtonFeedbackEnabled(false);

    m_deviceResources->SetWindow(window);

    window.Activated({ this, &App::OnWindowActivationChanged });

    window.SizeChanged({ this, &App::OnWindowSizeChanged });

    window.Closed({ this, &App::OnWindowClosed });

    window.VisibilityChanged({ this, &App::OnVisibilityChanged });

    DisplayInformation currentDisplayInformation{ DisplayInformation::GetForCurrentView() };

    currentDisplayInformation.DpiChanged({ this, &App::OnDpiChanged });

    currentDisplayInformation.OrientationChanged({ this, &App::OnOrientationChanged });

    currentDisplayInformation.StereoEnabledChanged({ this, &App::OnStereoEnabledChanged });

    DisplayInformation::DisplayContentsInvalidated({ this, &App::OnDisplayContentsInvalidated });
}
```

## <a name="the-appload-method"></a>App:: Load メソッド

メインウィンドウが設定されたので、 [**IFrameworkView:: Load**](/uwp/api/windows.applicationmodel.core.iframeworkview.load) の実装が呼び出されます。 **読み込み** は、 **初期化** と **setwindow**よりもゲームデータまたはアセットを事前にフェッチするのに適しています。

```cppwinrt
void Load(winrt::hstring const& /* entryPoint */)
{
    if (!m_main)
    {
        m_main = winrt::make_self<GameMain>(m_deviceResources);
    }
}
```

ご覧のように、実際の作業は、ここで作成した **GameMain** オブジェクトのコンストラクターに委任されます。 **GameMain**クラスは、およびで定義されてい `GameMain.h` `GameMain.cpp` ます。

### <a name="the-gamemaingamemain-constructor"></a>GameMain:: GameMain コンストラクター

**GameMain**コンストラクター (およびそれが呼び出すその他のメンバー関数) は、一連の非同期読み込み操作を開始して、ゲームオブジェクトの作成、グラフィックスリソースの読み込み、およびゲームのステートマシンの初期化を行います。 また、開始状態やグローバル値の設定など、ゲームを開始する前に必要な準備を行います。

Windows では、入力の処理を開始する前にゲームが実行できる時間に制限が設けられています。 ここで説明するように asyc を使用すると、開始された作業がバックグラウンドで継続している間に、 **読み込み** をすばやく返すことができます。 読み込みに時間がかかる場合、または多くのリソースがある場合は、頻繁に更新される進行状況バーをユーザーに提供することをお勧めします。 

非同期プログラミングに慣れていない場合は、「 [C++/WinRT を使用した同時実行と非同期操作](../cpp-and-winrt-apis/concurrency.md)」を参照してください。

```cppwinrt
GameMain::GameMain(std::shared_ptr<DX::DeviceResources> const& deviceResources) :
    m_deviceResources(deviceResources),
    m_windowClosed(false),
    m_haveFocus(false),
    m_gameInfoOverlayCommand(GameInfoOverlayCommand::None),
    m_visible(true),
    m_loadingCount(0),
    m_updateState(UpdateEngineState::WaitingForResources)
{
    m_deviceResources->RegisterDeviceNotify(this);

    m_renderer = std::make_shared<GameRenderer>(m_deviceResources);
    m_game = std::make_shared<Simple3DGame>();

    m_uiControl = m_renderer->GameUIControl();

    m_controller = std::make_shared<MoveLookController>(CoreWindow::GetForCurrentThread());

    auto bounds = m_deviceResources->GetLogicalSize();

    m_controller->SetMoveRect(
        XMFLOAT2(0.0f, bounds.Height - GameUIConstants::TouchRectangleSize),
        XMFLOAT2(GameUIConstants::TouchRectangleSize, bounds.Height)
        );
    m_controller->SetFireRect(
        XMFLOAT2(bounds.Width - GameUIConstants::TouchRectangleSize, bounds.Height - GameUIConstants::TouchRectangleSize),
        XMFLOAT2(bounds.Width, bounds.Height)
        );

    SetGameInfoOverlay(GameInfoOverlayState::Loading);
    m_uiControl->SetAction(GameInfoOverlayCommand::None);
    m_uiControl->ShowGameInfoOverlay();

    // Asynchronously initialize the game class and load the renderer device resources.
    // By doing all this asynchronously, the game gets to its main loop more quickly
    // and in parallel all the necessary resources are loaded on other threads.
    ConstructInBackground();
}

winrt::fire_and_forget GameMain::ConstructInBackground()
{
    auto lifetime = get_strong();

    m_game->Initialize(m_controller, m_renderer);

    co_await m_renderer->CreateGameDeviceResourcesAsync(m_game);

    // The finalize code needs to run in the same thread context
    // as the m_renderer object was created because the D3D device context
    // can ONLY be accessed on a single thread.
    // co_await of an IAsyncAction resumes in the same thread context.
    m_renderer->FinalizeCreateGameDeviceResources();

    InitializeGameState();

    if (m_updateState == UpdateEngineState::WaitingForResources)
    {
        // In the middle of a game so spin up the async task to load the level.
        co_await m_game->LoadLevelAsync();

        // The m_game object may need to deal with D3D device context work so
        // again the finalize code needs to run in the same thread
        // context as the m_renderer object was created because the D3D
        // device context can ONLY be accessed on a single thread.
        m_game->FinalizeLoadLevel();
        m_game->SetCurrentLevelToSavedState();
        m_updateState = UpdateEngineState::ResourcesLoaded;
    }
    else
    {
        // The game is not in the middle of a level so there aren't any level
        // resources to load.
    }

    // Since Game loading is an async task, the app visual state
    // may be too small or not have focus. Put the state machine
    // into the correct state to reflect these cases.

    if (m_deviceResources->GetLogicalSize().Width < GameUIConstants::MinPlayableWidth)
    {
        m_updateStateNext = m_updateState;
        m_updateState = UpdateEngineState::TooSmall;
        m_controller->Active(false);
        m_uiControl->HideGameInfoOverlay();
        m_uiControl->ShowTooSmall();
        m_renderNeeded = true;
    }
    else if (!m_haveFocus)
    {
        m_updateStateNext = m_updateState;
        m_updateState = UpdateEngineState::Deactivated;
        m_controller->Active(false);
        m_uiControl->SetAction(GameInfoOverlayCommand::None);
        m_renderNeeded = true;
    }
}

void GameMain::InitializeGameState()
{
    // Set up the initial state machine for handling Game playing state.
    ...
}
```

コンストラクターによって開始される一連の作業の概要を次に示します。

- **GameRenderer**型のオブジェクトを作成および初期化します。 詳細については、「[レンダリング フレームワーク I: レンダリングの概要](tutorial--assembling-the-rendering-pipeline.md)」を参照してください。
- **Simple3DGame**型のオブジェクトを作成および初期化します。 詳細については、「[メイン ゲーム オブジェクトの定義](tutorial--defining-the-main-game-loop.md)」を参照してください。
- ゲーム UI コントロールオブジェクトを作成し、ゲーム情報オーバーレイを表示して、リソースファイルの読み込み時に進行状況バーを表示します。 詳細については、「[ユーザー インターフェイスの追加](tutorial--adding-a-user-interface.md)」を参照してください。
- コントローラー (タッチ、マウス、または Xbox ワイヤレスコントローラー) からの入力を読み取るコントローラーオブジェクトを作成します。 詳細については、「[コントロールの追加](tutorial--adding-controls.md)」を参照してください。
- [移動] コントロールと [カメラのタッチ] コントロールでは、画面の左下隅と右下隅にそれぞれ2つの四角形の領域を定義します。 プレーヤーは、( **SetMoveRect**の呼び出しで定義されている) 左下の四角形を仮想制御パッドとして使用して、カメラを前方および後方に移動します。 **SetFireRect**メソッドによって定義される右下の四角形は、ammo を起動するための仮想ボタンとして使用されます。
- コルーチンを使用して、リソースの読み込みを別々のステージに分割します。 Direct3D デバイスコンテキストへのアクセスは、デバイスコンテキストが作成されたスレッドに制限されます。オブジェクト作成のための Direct3D デバイスへのアクセスはフリースレッドです。 その結果、 **GameRenderer:: CreateGameDeviceResourcesAsync** コルーチンは、完了タスク (**GameRenderer:: FinalizeCreateGameDeviceResources**) とは別のスレッドで実行できます。これは元のスレッドで実行されます。
- **Simple3DGame:: LoadLevelAsync**と**Simple3DGame:: FinalizeLoadLevel**を使用して、レベルリソースの読み込みに同様のパターンを使用します。

次のトピック ([ゲームフロー管理](tutorial-game-flow-management.md)) では、 **GameMain::** 初期の状態がさらに表示されます。

## <a name="the-apponactivated-method"></a>App:: OnActivated 化メソッド

次に、 [**Coreapplicationview:: アクティブ化**](/uwp/api/windows.applicationmodel.core.coreapplicationview.activated) イベントが発生します。 そのため、( **App:: OnActivated 化**メソッドなど) 使用している**onactivated 化**されたイベントハンドラーが呼び出されます。

```cppwinrt
void OnActivated(CoreApplicationView const& /* applicationView */, IActivatedEventArgs const& /* args */)
{
    CoreWindow window = CoreWindow::GetForCurrentThread();
    window.Activate();
}
```

ここで実行する作業は、主要な [**Corewindow**](/uwp/api/windows.ui.core.corewindow)をアクティブ化することだけです。 または、 **App:: SetWindow**でも選択できます。

## <a name="the-apprun-method"></a>App:: Run メソッド

**初期化**、 **setwindow**、および **Load** によってステージが設定されました。 これでゲームが開始され、 [**IFrameworkView:: Run**](/uwp/api/windows.applicationmodel.core.iframeworkview.run) の実装が呼び出されました。

```cppwinrt
void Run()
{
    m_main->Run();
}
```

ここでも、work は **GameMain**に委任されます。

### <a name="the-gamemainrun-method"></a>GameMain:: Run メソッド

**GameMain:: Run** はゲームのメインループです。詳細については、「」を参照 `GameMain.cpp` してください。 基本的なロジックでは、ゲームのウィンドウが開いたままになっている間に、すべてのイベントをディスパッチし、タイマーを更新してから、グラフィックスパイプラインの結果をレンダリングして表示します。 また、ゲームの状態間の遷移に使用されるイベントがディスパッチされて処理されます。

このコードは、ゲームエンジンのステートマシンの2つの状態にも関係しています。

- **UpdateEngineState::D eactivated 化**されました。 これは、ゲームウィンドウが非アクティブ化される (フォーカスが失われた) か、またはスナップされることを指定します。 
- **UpdateEngineState:: TooSmall**。 これは、クライアント領域が小さすぎてゲームをレンダリングできないことを指定します。

これらのいずれの状態でも、ゲームはイベント処理を中断し、ウィンドウのフォーカス、unsnap、またはサイズの変更を待機します。

ゲームにフォーカスがある場合、メッセージ キューに到達する各イベントを処理する必要があるため、[**CoreWindowDispatch.ProcessEvents**](/uwp/api/windows.ui.core.coredispatcher.processevents) を **ProcessAllIfPresent** オプションで呼び出す必要があります。 他のオプションを使用すると、メッセージイベントの処理に遅延が発生し、ゲームが応答しなくなったり、タッチ動作が遅くなる可能性があります。

ゲームが表示されない場合、中断されている場合、またはスナップされていない場合は、到着することのないメッセージをディスパッチするためにリソースサイクルを使用しないようにします。 この場合、ゲームでは **Processoneandallpending** オプションを使用する必要があります。 このオプションは、イベントが取得されるまでブロックし、そのイベントを処理します (最初の処理中にプロセスキューに到達した他のイベントも処理されます)。 **ProcessEvents** は、キューが処理された後すぐに制御を戻します。

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
                if (m_updateStateNext == UpdateEngineState::WaitingForResources)
                {
                    WaitingForResourceLoading();
                    m_renderNeeded = true;
                }
                else if (m_updateStateNext == UpdateEngineState::ResourcesLoaded)
                {
                    // In the device lost case, we transition to the final waiting state
                    // and make sure the display is updated.
                    switch (m_pressResult)
                    {
                    case PressResultState::LoadGame:
                        SetGameInfoOverlay(GameInfoOverlayState::GameStats);
                        break;

                    case PressResultState::PlayLevel:
                        SetGameInfoOverlay(GameInfoOverlayState::LevelStart);
                        break;

                    case PressResultState::ContinueLevel:
                        SetGameInfoOverlay(GameInfoOverlayState::Pause);
                        break;
                    }
                    m_updateStateNext = UpdateEngineState::WaitingForPress;
                    m_uiControl->ShowGameInfoOverlay();
                    m_renderNeeded = true;
                }

                if (!m_renderNeeded)
                {
                    // The App is not currently the active window and not in a transient state so just wait for events.
                    CoreWindow::GetForCurrentThread().Dispatcher().ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
                    break;
                }
                // otherwise fall through and do normal processing to get the rendering handled.
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

## <a name="the-appuninitialize-method"></a>App:: 初期化解除メソッド

ゲームが終了すると、 [**IFrameworkView:: の初期化**](/uwp/api/windows.applicationmodel.core.iframeworkview.uninitialize) の実装が呼び出されます。 クリーンアップを実行するチャンスです。 アプリウィンドウを閉じると、アプリのプロセスが強制終了しません。代わりに、アプリシングルトンの状態がメモリに書き込まれます。 リソースの特別なクリーンアップを含め、システムによってこのメモリが解放されたときに特別な処理が行われる場合は、そのクリーンアップ用のコードを **初期化**解除に配置します。

ここでは、 **App:: 初期化** 解除は no op です。

```cpp
void Uninitialize()
{
}
```

## <a name="tips"></a>ヒント

独自のゲームを開発する場合は、このトピックで説明する方法を中心にスタートアップコードを設計します。 各メソッドの基本的な提案の簡単な一覧を次に示します。

- **Initialize**を使用してメインクラスを割り当て、基本イベントハンドラーを接続します。
- **Setwindow**を使用して、ウィンドウ固有のイベントをサブスクライブし、メインウィンドウをデバイス依存のリソースオブジェクトに渡して、スワップチェーンの作成時にそのウィンドウを使用できるようにします。
- 残りのセットアップを処理し、オブジェクトの非同期作成とリソースの読み込みを開始するには、 **Load** を使用します。 Procedurally によって生成されたアセットなど、一時ファイルまたはデータを作成する必要がある場合は、ここでも実行します。

## <a name="next-steps"></a>次の手順

このトピックでは、DirectX を使用する UWP ゲームの基本的な構造について説明しました。 これらのメソッドについては、後のトピックで説明します。そのため、これらのメソッドを念頭に置いておくことをお勧めします。

次のトピック「ゲームフローの管理」では、ゲーム &mdash; [Game flow management](tutorial-game-flow-management.md) &mdash; の流れを維持するためにゲームの状態とイベント処理を管理する方法について詳しく説明します。