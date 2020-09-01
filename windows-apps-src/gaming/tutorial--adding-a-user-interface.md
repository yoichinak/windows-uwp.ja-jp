---
title: ユーザー インターフェイスの追加
description: Direct2D Api を使用して、ヘッドアップ表示メニューとゲーム状態メニューを含む2D ユーザーインターフェイスオーバーレイを DirectX UWP ゲームに追加する方法について説明します。
ms.assetid: fa40173e-6cde-b71b-e307-db90f0388485
ms.date: 10/24/2017
ms.topic: article
keywords: Windows 10, UWP, ゲーム, ユーザー インターフェイス, DirectX
ms.localizationpriority: medium
ms.openlocfilehash: eca248887985fc6d33ca6d6b552a0b61a98ce428
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173106"
---
# <a name="add-a-user-interface"></a>ユーザー インターフェイスの追加

> [!NOTE]
> このトピックは、「DirectX チュートリアルシリーズ [を含む simple ユニバーサル Windows プラットフォーム (UWP) ゲームの作成](tutorial--create-your-first-uwp-directx-game.md) 」に含まれています。 このリンクのトピックでは、系列のコンテキストを設定します。

ゲームに3D ビジュアルが配置されたので、ゲームがプレーヤーにゲームの状態に関するフィードバックを提供できるように、いくつかの2D 要素を追加することに注目します。 これは、単純なメニューオプションと、3-d グラフィックスパイプライン出力の上にヘッドアップ表示コンポーネントを追加することによって実現できます。

>[!Note]
>このサンプルの最新のゲームコードをダウンロードしていない場合は、「 [Direct3D サンプルゲーム](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX)」にアクセスしてください。 このサンプルは、UWP 機能のサンプルの大規模なコレクションの一部です。 サンプルをダウンロードする手順については、「[GitHub から UWP のサンプルを取得する](../get-started/get-app-samples.md)」をご覧ください。

## <a name="objective"></a>目的

Direct2D を使用して、次のような多くのユーザーインターフェイスのグラフィックスと動作を UWP DirectX ゲームに追加します。
- ヘッドアップディスプレイ ( [移動外観コントローラー](tutorial--adding-controls.md) boundry 四角形を含む)
- ゲーム状態メニュー


## <a name="the-user-interface-overlay"></a>ユーザー インターフェイスのオーバーレイ


DirectX ゲームでテキストやユーザーインターフェイスの要素を表示する方法は多数ありますが、 [Direct2D](/windows/desktop/Direct2D/direct2d-portal)の使用に焦点を当てます。 また、テキスト要素に [DirectWrite](/windows/desktop/DirectWrite/direct-write-portal) を使用します。


Direct2D は、ピクセルベースのプリミティブと効果を描画するために使用される一連の2D 描画 Api です。 Direct2D を使用して作業を開始するときは、単純にしておくことをお勧めします。 複雑なレイアウトやインターフェイス動作には、時間と計画が必要です。 ゲームにシミュレーションや戦略ゲームで見られるような複雑なユーザーインターフェイスが必要な場合は、代わりに XAML を使用することを検討してください。

> [!NOTE]
> UWP DirectX ゲームで XAML を使用したユーザーインターフェイスの開発の詳細については、「 [サンプルゲームの拡張](tutorial-resources.md)」を参照してください。

Direct2D は、HTML や XAML などのユーザーインターフェイスやレイアウト用には特に設計されていません。 リスト、ボックス、ボタンなどのユーザーインターフェイスコンポーネントは用意されていません。 また、div、テーブル、グリッドなどのレイアウトコンポーネントも提供しません。


このサンプルゲームでは、2つの主要な UI コンポーネントがあります。
1. スコアとゲーム内のコントロールのヘッドアップディスプレイ。
2. ゲームの状態のテキストや、一時停止情報やレベルの開始オプションなどのオプションを表示するために使用されるオーバーレイ。

### <a name="using-direct2d-for-a-heads-up-display"></a>Direct2D を使ったヘッドアップ ディスプレイ

次の図は、サンプルのゲーム内のヘッドアップディスプレイを示しています。 これは単純ですっきりしたものであり、windows media player は3D ワールドの移動やターゲットの撮影に専念できます。 優れたインターフェイスまたはヘッドアップディスプレイは、プレーヤーがゲームのイベントを処理し、それに対応する能力を複雑にすることがないようにする必要があります。

![ゲーム オーバーレイのスクリーン ショット](images/simple-dx-game-ui-overlay.png)

オーバーレイは、次の基本プリミティブで構成されています。
- [**DirectWrite**](/windows/desktop/DirectWrite/direct-write-portal) のプレーヤーに通知する右上隅のテキスト 
    - 成功したヒット数
    - プレーヤーが行ったショットの数
    - レベルの残り時間
    - 現在のレベル番号 
- 十字線を形成するために使用される2つの交差する線分
- 境界の下隅に2つの四角形が [表示](tutorial--adding-controls.md) されます。 


オーバーレイのゲーム内のヘッドアップ表示の状態は、作成された[**hud**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameHud.h)クラスのゲームの[**Hud:: Render**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameHud.cpp#L234-L358)メソッドに描画されます。 このメソッド内では、UI を表す Direct2D オーバーレイが更新され、ヒット数、残存時間、およびレベル番号の変更が反映されます。

ゲームが初期化されている場合は、、 `TotalHits()` `TotalShots()` 、およびを `TimeRemaining()` [**swprintf_s**](/cpp/c-runtime-library/reference/sprintf-s-sprintf-s-l-swprintf-s-swprintf-s-l) バッファーに追加し、印刷形式を指定します。 その後、 [**DrawText**](/windows/desktop/Direct2D/id2d1rendertarget-drawtext) メソッドを使用して描画できます。 現在のレベルインジケーターについても同じことを行い、空の数値を描画して➀のような未完了のレベルを表示します。また、➊のような数値を入力して、特定のレベルが完了したことを示します。


次のコードスニペットで **は、を使用して** 、 
- [* * ID2D1RenderTarget::D rawBitmap * *](/windows/desktop/api/d2d1/nf-d2d1-id2d1rendertarget-drawbitmap(id2d1bitmap_constd2d1_rect_f__float_d2d1_bitmap_interpolation_mode_constd2d1_rect_f_))を使用してビットマップを作成する
- [ **D2D1:: RectF**を使用して UI 領域を四角形に分割する](/windows/desktop/api/dcommon/ns-dcommon-d2d_rect_f)
- **DrawText**を使用してテキスト要素を作成する

```cppwinrt
void GameHud::Render(_In_ std::shared_ptr<Simple3DGame> const& game)
{
    auto d2dContext = m_deviceResources->GetD2DDeviceContext();
    auto windowBounds = m_deviceResources->GetLogicalSize();

    if (m_showTitle)
    {
        d2dContext->DrawBitmap(
            m_logoBitmap.get(),
            D2D1::RectF(
                GameUIConstants::Margin,
                GameUIConstants::Margin,
                m_logoSize.width + GameUIConstants::Margin,
                m_logoSize.height + GameUIConstants::Margin
                )
            );
        d2dContext->DrawTextLayout(
            Point2F(m_logoSize.width + 2.0f * GameUIConstants::Margin, GameUIConstants::Margin),
            m_titleHeaderLayout.get(),
            m_textBrush.get()
            );
        d2dContext->DrawTextLayout(
            Point2F(GameUIConstants::Margin, m_titleBodyVerticalOffset),
            m_titleBodyLayout.get(),
            m_textBrush.get()
            );
    }

    // Draw text for number of hits, total shots, and time remaining
    if (game != nullptr)
    {
        // This section is only used after the game state has been initialized.
        static const int bufferLength = 256;
        static wchar_t wsbuffer[bufferLength];
        int length = swprintf_s(
            wsbuffer,
            bufferLength,
            L"Hits:\t%10d\nShots:\t%10d\nTime:\t%8.1f",
            game->TotalHits(),
            game->TotalShots(),
            game->TimeRemaining()
            );

        // Draw the upper right portion of the HUD displaying total hits, shots, and time remaining
        d2dContext->DrawText(
            wsbuffer,
            length,
            m_textFormatBody.get(),
            D2D1::RectF(
                windowBounds.Width - GameUIConstants::HudRightOffset,
                GameUIConstants::HudTopOffset,
                windowBounds.Width,
                GameUIConstants::HudTopOffset + (GameUIConstants::HudBodyPointSize + GameUIConstants::Margin) * 3
                ),
            m_textBrush.get()
            );

        // Using the unicode characters starting at 0x2780 ( ➀ ) for the consecutive levels of the game.
        // For completed levels start with 0x278A ( ➊ ) (This is 0x2780 + 10).
        uint32_t levelCharacter[6];
        for (uint32_t i = 0; i < 6; i++)
        {
            levelCharacter[i] = 0x2780 + i + ((static_cast<uint32_t>(game->LevelCompleted()) == i) ? 10 : 0);
        }
        length = swprintf_s(
            wsbuffer,
            bufferLength,
            L"%lc %lc %lc %lc %lc %lc",
            levelCharacter[0],
            levelCharacter[1],
            levelCharacter[2],
            levelCharacter[3],
            levelCharacter[4],
            levelCharacter[5]
            );
        // Create a new rectangle and draw the current level info text inside
        d2dContext->DrawText(
            wsbuffer,
            length,
            m_textFormatBodySymbol.get(),
            D2D1::RectF(
                windowBounds.Width - GameUIConstants::HudRightOffset,
                GameUIConstants::HudTopOffset + (GameUIConstants::HudBodyPointSize + GameUIConstants::Margin) * 3 + GameUIConstants::Margin,
                windowBounds.Width,
                GameUIConstants::HudTopOffset + (GameUIConstants::HudBodyPointSize + GameUIConstants::Margin) * 4
                ),
            m_textBrush.get()
            );

        if (game->IsActivePlay())
        {
            // Draw the move and fire rectangles
            ...
            // Draw the crosshairs
            ...
        }
    }
}
```

このメソッドをさらに分割することで、この[**:D ID2D1RenderTarget**](/windows/desktop/api/d2d1/nf-d2d1-id2d1rendertarget-drawrectangle(constd2d1_rect_f__id2d1brush_float_id2d1strokestyle))の[**例では**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameHud.cpp#L320-L358)、 [**ID2D1RenderTarget::D rawrectangle**](/windows/desktop/api/d2d1/nf-d2d1-id2d1rendertarget-drawline)の2つの呼び出しを使用して、移動と発射四角形を描画します。

```cppwinrt
// Check if game is playing
if (game->IsActivePlay())
{
    // Draw a rectangle for the touch input for the move control.
    d2dContext->DrawRectangle(
        D2D1::RectF(
            0.0f,
            windowBounds.Height - GameUIConstants::TouchRectangleSize,
            GameUIConstants::TouchRectangleSize,
            windowBounds.Height
            ),
        m_textBrush.get()
        );
    // Draw a rectangle for the touch input for the fire control.
    d2dContext->DrawRectangle(
        D2D1::RectF(
            windowBounds.Width - GameUIConstants::TouchRectangleSize,
            windowBounds.Height - GameUIConstants::TouchRectangleSize,
            windowBounds.Width,
            windowBounds.Height
            ),
        m_textBrush.get()
        );

    // Draw the cross hairs
    d2dContext->DrawLine(
        D2D1::Point2F(windowBounds.Width / 2.0f - GameUIConstants::CrossHairHalfSize,
            windowBounds.Height / 2.0f),
        D2D1::Point2F(windowBounds.Width / 2.0f + GameUIConstants::CrossHairHalfSize,
            windowBounds.Height / 2.0f),
        m_textBrush.get(),
        3.0f
        );
    d2dContext->DrawLine(
        D2D1::Point2F(windowBounds.Width / 2.0f, windowBounds.Height / 2.0f -
            GameUIConstants::CrossHairHalfSize),
        D2D1::Point2F(windowBounds.Width / 2.0f, windowBounds.Height / 2.0f +
            GameUIConstants::CrossHairHalfSize),
        m_textBrush.get(),
        3.0f
        );
}
```

Game **hud:: Render** メソッドでは、ゲームウィンドウの論理サイズを変数に格納し `windowBounds` ます。 これは、 [`GetLogicalSize`](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Common/DeviceResources.h#L41) **DeviceResources** クラスのメソッドを使用します。 

```cppwinrt
auto windowBounds = m_deviceResources->GetLogicalSize();
```

ゲームウィンドウのサイズを取得することは、UI プログラミングに不可欠です。 ウィンドウのサイズは Dip (デバイスに依存しないピクセル) と呼ばれる測定値で与えられます。 DIP は1/96 インチとして定義されます。 Direct2D は、描画が発生したときに描画単位を実際のピクセルにスケールします。これを行うには、[Windows のドット/インチ (DPI)] 設定を使用します。 同様に、 [**DirectWrite**](/windows/desktop/DirectWrite/direct-write-portal)を使用してテキストを描画する場合は、フォントのサイズをポイントするのではなく、dip を指定します。 DIP は、浮動小数点数として表されます。 

### <a name="displaying-game-state-info"></a>ゲーム状態情報の表示

サンプルゲームには、ヘッドアップディスプレイだけでなく、6つのゲーム状態を表すオーバーレイがあります。 すべての状態は、プレーヤーが読み取るテキストを含む大きな黒の四角形プリミティブです。 これらの状態ではアクティブではないため、移動外観コントローラーの四角形と十字線は描画されません。

オーバーレイは、game [**Infooverの**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.h) クラスを使用して作成されます。これにより、ゲームの状態に合わせて表示されるテキストを切り替えることができます。

![オーバーレイの状態とアクション](images/simple-dx-game-ui-finaloverlay.png)

オーバーレイは、 **状態** と **アクション**という2つのセクションに分かれています。 **ステータス**secton は、さらに [**タイトル**] と [**本文**] の四角形に分割されます。 **Action**セクションには、四角形が1つだけ含まれます。 各四角形の目的は異なります。

-   `titleRectangle` タイトルのテキストを格納します。
-   `bodyRectangle` 本文のテキストを格納します。
-   `actionRectangle` プレーヤーに特定のアクションを実行するように通知するテキストが含まれています。

ゲームには6つの州を設定できます。 オーバーレイの **状態** 部分を使用して伝達されたゲームの状態。 **状態**の四角形は、次の状態に対応するさまざまなメソッドを使用して更新されます。

- 読み込み
- 最初の開始/高スコアの統計
- レベルの開始
- ゲームを一時停止しました
- ゲーム オーバー
- 試合勝ち


オーバーレイの **アクション** 部分は、次のいずれかに設定されるように、指定された操作 [**のメソッドを**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L522-L564) 使用して更新されます。
- "タップしてもう一度再生する..."
- "レベルを読み込んでいます。お待ちください..."
- "タップして続行..."
- なし

> [!NOTE]
> これらの2つの方法については、「 [ゲーム状態を表す](#representing-game-state) 」セクションでさらに説明します。

ゲームで何が起こっているかによって、[ **状態** ] と [ **アクション** ] セクションのテキストフィールドが調整されます。
これら6つの状態のオーバーレイを初期化して描画する方法を見てみましょう。

### <a name="initializing-and-drawing-the-overlay"></a>オーバーレイの初期化と描画

6つの **状態** の状態にはいくつかの共通点があり、これらのリソースや方法は非常によく似ています。
    - すべてのユーザーは、画面の中央に黒い四角形を背景として使用します。
    - 表示されるテキストは、 **タイトル** または **本文** のテキストです。
    - テキストは Segoe UI フォントを使用し、背景の四角形の上に描画されます。 


サンプルゲームには、オーバーレイを作成するときに再生される4つの方法があります。
 

#### <a name="gameinfooverlaygameinfooverlay"></a>このようにしています。
このようにして、windows media player に情報を表示するために使用するビットマップ画面を維持したままオーバーレイを [**初期化します**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L30-L78) 。 コンストラクターは、渡された [**ID2D1Device**](/windows/desktop/api/d2d1_1/nn-d2d1_1-id2d1device) オブジェクトからファクトリを取得します。これは、オーバーレイオブジェクト自体が描画できる [**ID2D1DeviceContext**](/windows/desktop/api/d2d1_1/nn-d2d1_1-id2d1devicecontext) を作成するために使用されます。 [IDWriteFactory::CreateTextFormat](/windows/desktop/api/dwrite/nf-dwrite-idwritefactory-createtextformat) 


#### <a name="gameinfooverlaycreatedevicedependentresources"></a>このようにしてください。
のテキストを描画するために使用されるブラシを作成するには、[**次**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L82-L104)のような方法があります。 これを行うために、 [**ID2D1DeviceContext2**](/windows/desktop/api/d2d1_3/nn-d2d1_3-id2d1devicecontext2) オブジェクトを取得します。これにより、ジオメトリの作成と描画が可能になり、インクやグラデーションメッシュのレンダリングなどの機能も使用できるようになります。 次に、 [**ID2D1SolidColorBrush**](/windows/desktop/api/d2d1/nn-d2d1-id2d1solidcolorbrush) を使用して一連の色分けされたブラシを作成し、ユーザーの UI 要素を描画します。
- 四角形の背景の黒ブラシ
- ステータステキストの白いブラシ
- アクションテキストのオレンジ色のブラシ

#### <a name="deviceresourcessetdpi"></a>DeviceResources:: SetDpi

[**DeviceResources:: SetDpi**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Common/DeviceResources.cpp#L514-L527)メソッドは、ウィンドウの1インチあたりのドット数を設定します。 このメソッドは、DPI が変更され、ゲームウィンドウのサイズが変更されたときに発生する readjusted する必要があるときに呼び出されます。 DPI を更新した後、このメソッドは[**DeviceResources:: CreateWindowSizeDependentResources**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Common/DeviceResources.cpp#L214-L487) も呼び出して、ウィンドウのサイズが変更されるたびに必要なリソースが再作成されるようにします。

#### <a name="gameinfooverlaycreatewindowssizedependentresources"></a>"お持ちの情報 Infooverのレイアウト:: CreateWindowsSizeDependentResources"

次に、すべての描画が実行される [**よう**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L108-L225) にして、すべての描画が行われるようにします。 メソッドの手順の概要を次に示します。
- **タイトル**、**本文**、および**アクション**テキストの UI テキストのセクションに3つの四角形が作成されます。
    ```cppwinrt 
    m_titleRectangle = D2D1::RectF(
        GameInfoOverlayConstant::SideMargin,
        GameInfoOverlayConstant::TopMargin,
        overlaySize.width - GameInfoOverlayConstant::SideMargin,
        GameInfoOverlayConstant::TopMargin + GameInfoOverlayConstant::TitleHeight
        );
    m_actionRectangle = D2D1::RectF(
        GameInfoOverlayConstant::SideMargin,
        overlaySize.height - (GameInfoOverlayConstant::ActionHeight + GameInfoOverlayConstant::BottomMargin),
        overlaySize.width - GameInfoOverlayConstant::SideMargin,
        overlaySize.height - GameInfoOverlayConstant::BottomMargin
        );
    m_bodyRectangle = D2D1::RectF(
        GameInfoOverlayConstant::SideMargin,
        m_titleRectangle.bottom + GameInfoOverlayConstant::Separator,
        overlaySize.width - GameInfoOverlayConstant::SideMargin,
        m_actionRectangle.top - GameInfoOverlayConstant::Separator
        );
    ```

- という名前のビットマップが作成され `m_levelBitmap` 、現在の DPI が **createbitmap**を使用してアカウントになります。
- `m_levelBitmap` は、 [**ID2D1DeviceContext:: Settarget**](/windows/desktop/api/d2d1_1/nf-d2d1_1-id2d1devicecontext-settarget)を使用して2d レンダーターゲットとして設定されます。
- ビットマップは、 [**ID2D1RenderTarget:: Clear**](/windows/desktop/api/d2d1/nf-d2d1-id2d1rendertarget-clear)を使用して黒で作成されたすべてのピクセルで消去されます。
- [**ID2D1RenderTarget:: BeginDraw**](/windows/desktop/api/d2d1/nf-d2d1-id2d1rendertarget-begindraw) が描画を開始するために呼び出されます。 
- **DrawText** は `m_titleString` 、 `m_bodyString` 対応する `m_actionString` **ID2D1SolidColorBrush**を使用して、、、およびに格納されているテキストを ap 四角形に描画するために呼び出されます。
- [**ID2D1RenderTarget:: EndDraw**](ID2D1RenderTarget::EndDraw) は、ですべての描画操作を停止するために呼び出され `m_levelBitmap` ます。
- 別のビットマップは、フォールバックとして使用するために、という名前の **Createbitmap** を使用して作成されます。これは、 `m_tooSmallBitmap` ディスプレイ構成がゲームに対して小さすぎる場合にのみ表示されます。
- に対して描画するプロセスを繰り返し `m_levelBitmap` `m_tooSmallBitmap` ます。今回は、本文に文字列を描画するだけです `Paused` 。




ここで必要なのは、6つのオーバーレイ状態のテキストを入力する6つの方法です。

### <a name="representing-game-state"></a>ゲームの状態を表す


ゲーム内の6つのオーバーレイ状態のそれぞれ **に、対応** するメソッドがあります。 これらのメソッドは、さまざまなオーバーレイを描画して、ゲーム自体に関する明示的な情報をプレイヤーに伝えます。 この通信は、 **タイトル** と **本文** 文字列で表されます。 このサンプルでは、初期化時にこの情報のリソースとレイアウトが既に構成されています。また、この情報は、状態固有のオーバーレイ [**リソース**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L82-L104) メソッドを使用して提供する必要があります。

オーバーレイの **状態** 部分は、次のいずれかのメソッドを呼び出すことによって設定されます。

ゲームの状態 | Status set メソッド | 状態フィールド
:----- | :------- | :---------
読み込み | [このようにしています。](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L254-L306) |**タイトル**</br>読み込み (リソースを) </br>**本文**</br> "." をインクリメンタルに出力して、読み込みアクティビティを意味します。
最初の開始/高スコアの統計 | [このようにしてください。](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L310-L354) |**タイトル**</br>ハイスコア</br> **本文**</br> 完了したレベル# </br>合計ポイント数#</br>合計ショット#
レベルの開始 | [を実行しています。](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L413-L471) |**タイトル**</br>平準#</br>**本文**</br>レベル目標の説明。
ゲームを一時停止しました | [このようにしてください。](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L475-L502) |**タイトル**</br>ゲームを一時停止しました</br>**本文**</br>なし
ゲーム オーバー | [このようにしてください。](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L358-L409) |**タイトル**</br>ゲームは終わりました</br> **本文**</br> 完了したレベル# </br>合計ポイント数#</br>合計ショット#</br>完了したレベル#</br>ハイスコア#
試合勝ち | [このようにしてください。](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L358-L409) |**タイトル**</br>勝利しました。</br> **本文**</br> 完了したレベル# </br>合計ポイント数#</br>合計ショット#</br>完了したレベル#</br>ハイスコア#

このサンプルでは、を [**使用して**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L117-L134) 、このサンプルではオーバーレイの特定の領域に対応する3つの四角形領域が宣言されています。

これらの領域を念頭に置いて、状態に固有のメソッドの 1 つである **GameInfoOverlay::SetGameStats** と、オーバーレイの描画方法を確認してみましょう。

```cppwinrt
void GameInfoOverlay::SetGameStats(int maxLevel, int hitCount, int shotCount)
{
    int length;

    auto d2dContext = m_deviceResources->GetD2DDeviceContext();

    d2dContext->SetTarget(m_levelBitmap.get());
    d2dContext->BeginDraw();
    d2dContext->SetTransform(D2D1::Matrix3x2F::Identity());
    d2dContext->FillRectangle(&m_titleRectangle, m_backgroundBrush.get());
    d2dContext->FillRectangle(&m_bodyRectangle, m_backgroundBrush.get());
    m_titleString = L"High Score";

    d2dContext->DrawText(
        m_titleString.c_str(),
        m_titleString.size(),
        m_textFormatTitle.get(),
        m_titleRectangle,
        m_textBrush.get()
        );
    length = swprintf_s(
        wsbuffer,
        bufferLength,
        L"Levels Completed %d\nTotal Points %d\nTotal Shots %d",
        maxLevel,
        hitCount,
        shotCount
        );
    m_bodyString = std::wstring(wsbuffer, length);
    d2dContext->DrawText(
        m_bodyString.c_str(),
        m_bodyString.size(),
        m_textFormatBody.get(),
        m_bodyRectangle,
        m_textBrush.get()
        );

    // We ignore D2DERR_RECREATE_TARGET here. This error indicates that the device
    // is lost. It will be handled during the next call to Present.
    HRESULT hr = d2dContext->EndDraw();
    if (hr != D2DERR_RECREATE_TARGET)
    {
        // The D2DERR_RECREATE_TARGET indicates there has been a problem with the underlying
        // D3D device. All subsequent rendering will be ignored until the device is recreated.
        // This error will be propagated and the appropriate D3D error will be returned from the
        // swapchain->Present(...) call. At that point, the sample will recreate the device
        // and all associated resources. As a result, the D2DERR_RECREATE_TARGET doesn't
        // need to be handled here.
        winrt::check_hresult(hr);
    }
}
```

このメソッドは、Direct2D デバイスコンテキストを使用して、 **このオブジェクトが初期化されて** います。このメソッドは、背景ブラシを使用して、タイトルと本体の四角形を黒で塗りつぶします。 また、白のテキスト ブラシを使って、"High Score" 文字列用のテキストをタイトルの四角形に描画し、ゲームの状態の最新情報が含まれている文字列を本文の四角形に描画します。


アクションの四角形を更新するには、 **GameMain**オブジェクトの[**メソッドから、**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L522-L564)次のように、game Infooversample:: **setaction**が必要とするゲーム状態情報を提供します。これにより、プレーヤーに対する適切なメッセージ ("Tap to continue" など) を確認できます。

特定の状態のオーバーレイは、次のように [**GameMain:: Set Infoover**](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameXaml/cpp/GameMain.cpp#L606-L661) メソッドで選択されます。

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
        m_uiControl->SetGameStats(
            m_game->HighScore().levelCompleted + 1,
            m_game->HighScore().totalHits,
            m_game->HighScore().totalShots
            );
        break;

    case GameInfoOverlayState::LevelStart:
        m_uiControl->SetLevelStart(
            m_game->LevelCompleted() + 1,
            m_game->CurrentLevel()->Objective(),
            m_game->CurrentLevel()->TimeLimit(),
            m_game->BonusTime()
            );
        break;

    case GameInfoOverlayState::GameOverCompleted:
        m_uiControl->SetGameOver(
            true,
            m_game->LevelCompleted() + 1,
            m_game->TotalHits(),
            m_game->TotalShots(),
            m_game->HighScore().totalHits
            );
        break;

    case GameInfoOverlayState::GameOverExpired:
        m_uiControl->SetGameOver(
            false,
            m_game->LevelCompleted(),
            m_game->TotalHits(),
            m_game->TotalShots(),
            m_game->HighScore().totalHits
            );
        break;

    case GameInfoOverlayState::Pause:
        m_uiControl->SetPause(
            m_game->LevelCompleted() + 1,
            m_game->TotalHits(),
            m_game->TotalShots(),
            m_game->TimeRemaining()
            );
        break;
    }
}
```

ゲームでは、ゲームの状態に基づいてテキスト情報をプレーヤーに伝える方法が用意されているので、ゲーム全体に表示される内容を切り替えることができます。

### <a name="next-steps"></a>次の手順

次のトピック「 [コントロールの追加](tutorial--adding-controls.md)」では、プレーヤーがサンプルゲームと対話する方法と、入力がゲーム状態にどのように変化するかを確認します。