---
title: ゲーム ループの移植
description: IFrameworkView を作成して、全画面表示の CoreWindow を制御する方法など、ユニバーサル Windows プラットフォーム (UWP) ゲームのウィンドウを実装する方法とゲーム ループを移植する方法について説明します。
ms.assetid: 070dd802-cb27-4672-12ba-a7f036ff495c
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, ゲーム, 移植, ゲーム ループ, Direct3D 9, DirectX 11
ms.localizationpriority: medium
ms.openlocfilehash: e62b7e6576ff1b39cbeba2c201f929952abd4105
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89159096"
---
# <a name="port-the-game-loop"></a>ゲーム ループの移植



**まとめ**

-   [パート 1: Direct3D 11 の初期化](simple-port-from-direct3d-9-to-11-1-part-1--initializing-direct3d.md)
-   [パート 2: レンダリング フレームワークの変換](simple-port-from-direct3d-9-to-11-1-part-2--rendering.md)
-   パート 3: ゲーム ループの移植


ユニバーサル Windows プラットフォーム (UWP) ゲーム用のウィンドウを実装する方法と、ゲームループをどのように使用するかを示します。これには、 [**IFrameworkView**](/uwp/api/Windows.ApplicationModel.Core.IFrameworkView) を作成して全画面の [**corewindow**](/uwp/api/Windows.UI.Core.CoreWindow)を制御する方法が含まれます。 「[チュートリアル: DirectX 11 とユニバーサル Windows プラットフォーム (UWP) への簡単な Direct3D 9 アプリの移植](walkthrough--simple-port-from-direct3d-9-to-11-1.md)」のパート 3 です。

## <a name="create-a-window"></a>ウィンドウの作成


Direct3D 9 のビューポートを使ってデスクトップ ウィンドウを設定するためには、デスクトップ アプリ用の従来のウィンドウ フレームワークを実装する必要がありました。 また、HWND の作成、ウィンドウ サイズの設定、ウィンドウ処理コールバックの提供、ウィンドウの表示などを実行する必要もありました。

UWP 環境には、はるかに簡単なシステムが用意されています。 DirectX を使った Microsoft Store ゲームでは、従来のウィンドウを設定する代わりに、[**IFrameworkView**](/uwp/api/Windows.ApplicationModel.Core.IFrameworkView) を実装します。 このインターフェイスは、アプリ コンテナー内の [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) で直接 DirectX アプリやゲームを実行するために存在します。

> **メモ**   Windows には、ソースアプリケーションオブジェクトや[**Corewindow**](/uwp/api/Windows.UI.Core.CoreWindow)などのリソースへのマネージポインターが用意されています。 「 [**オブジェクト演算子 (^) へのハンドル」を**](/cpp/extensions/handle-to-object-operator-hat-cpp-component-extensions)参照してください。

 

"main" クラスには、[**IFrameworkView**](/uwp/api/Windows.ApplicationModel.Core.IFrameworkView) から継承し、5 つの **IFrameworkView** メソッド ([**Initialize**](/uwp/api/windows.applicationmodel.core.iframeworkview.initialize)、[**SetWindow**](/uwp/api/windows.applicationmodel.core.iframeworkview.setwindow)、[**Load**](/uwp/api/windows.applicationmodel.core.iframeworkview.load)、[**Run**](/uwp/api/windows.applicationmodel.core.iframeworkview.run)、[**Uninitialize**](/uwp/api/windows.applicationmodel.core.iframeworkview.uninitialize)) を実装する必要があります。 (実質的に) ゲームが存在する場所となる **IFrameworkView** の作成に加えて、**IFrameworkView** のインスタンスを作成するファクトリ クラスを実装する必要があります。 ゲームにはこれまでどおり **main()** と呼ばれるメソッドを含む実行可能ファイルが存在しますが、main で実行できる操作は、ファクトリを使って、**IFrameworkView** のインスタンスを作成することだけです。

main 関数

```cpp
//-----------------------------------------------------------------------------
// Required method for a DirectX-only app.
// The main function is only used to initialize the app's IFrameworkView class.
//-----------------------------------------------------------------------------
[Platform::MTAThread]
int main(Platform::Array<Platform::String^>^)
{
    auto direct3DApplicationSource = ref new Direct3DApplicationSource();
    CoreApplication::Run(direct3DApplicationSource);
    return 0;
}
```

IFrameworkView ファクトリ

```cpp
//-----------------------------------------------------------------------------
// This class creates our IFrameworkView.
//-----------------------------------------------------------------------------
ref class Direct3DApplicationSource sealed : 
    Windows::ApplicationModel::Core::IFrameworkViewSource
{
public:
    virtual Windows::ApplicationModel::Core::IFrameworkView^ CreateView()
    {
        return ref new Cube11();
    };
};
```

## <a name="port-the-game-loop"></a>ゲーム ループの移植


次に、Direct3D 9 の実装のゲーム ループを見てみましょう。 このコードは、アプリの main 関数内に存在します。 このループの各反復では、ウィンドウ メッセージを処理するか、フレームをレンダリングします。

Direct3D 9 デスクトップ ゲームのゲーム ループ

```cpp
while(WM_QUIT != msg.message)
{
    // Process window events.
    // Use PeekMessage() so we can use idle time to render the scene. 
    bGotMsg = (PeekMessage(&msg, NULL, 0U, 0U, PM_REMOVE) != 0);

    if(bGotMsg)
    {
        // Translate and dispatch the message
        TranslateMessage(&msg);
        DispatchMessage(&msg);
    }
    else
    {
        // Render a new frame.
        // Render frames during idle time (when no messages are waiting).
        RenderFrame();
    }
}
```

このゲーム ループは、UWP バージョンのゲームと似ていますが、それよりも簡単です。

このゲームは [**IFrameworkView**](/uwp/api/Windows.ApplicationModel.Core.IFrameworkView) クラス内で機能するため、ゲーム ループは (**main()** ではなく) [**IFrameworkView::Run**](/uwp/api/windows.applicationmodel.core.iframeworkview.run) メソッドで実行されます。

メッセージ処理フレームワークを実装し、[**PeekMessage**](/windows/desktop/api/winuser/nf-winuser-peekmessagea) を呼び出す代わりに、アプリ ウィンドウの [**CoreDispatcher**](/uwp/api/Windows.UI.Core.CoreDispatcher) に組み込まれている [**ProcessEvents**](/uwp/api/windows.ui.core.coredispatcher.processevents) メソッドを呼び出すことができます。 ゲーム ループで分岐し、メッセージを処理する必要はありません。**ProcessEvents** を呼び出し、次に進むだけです。

Direct3D 11 Microsoft Store ゲームのゲーム ループ

```cpp
// UWP apps should not exit. Use app lifecycle events instead.
while (true)
{
    // Process window events.
    auto dispatcher = CoreWindow::GetForCurrentThread()->Dispatcher;
    dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);

    // Render a new frame.
    RenderFrame();
}
```

これで、DirectX 9 の例と同じように、基本的なグラフィックス インフラストラクチャをセットアップし、同じカラフルな立方体をレンダリングする UWP アプリが完成しました。

## <a name="where-do-i-go-from-here"></a>今後の進め方について


「[DirectX 11 の移植に関する FAQ](directx-porting-faq.md)」をブックマークに追加してください。

DirectX UWP テンプレートには、ゲームですぐに使うことができる堅牢な Direct3D デバイス インフラストラクチャが含まれています。 適切なテンプレートを選ぶためのガイダンスについては、「[テンプレートからの DirectX ゲーム プロジェクトの作成](user-interface.md)」をご覧ください。

次の詳細な Microsoft Store ゲーム開発に関する記事をご覧ください。

-   [チュートリアル: DirectX によるシンプルな UWP ゲームの作成](tutorial--create-your-first-uwp-directx-game.md)
-   [ゲームのオーディオ](working-with-audio-in-your-directx-game.md)
-   [ゲームのムーブ/ルック コントロール](tutorial--adding-move-look-controls-to-your-directx-game.md)

 

 