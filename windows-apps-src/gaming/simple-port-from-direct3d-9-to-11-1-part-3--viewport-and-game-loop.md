---
title: ゲーム ループの移植
description: IFrameworkView を作成して、全画面表示の CoreWindow を制御する方法など、ユニバーサル Windows プラットフォーム (UWP) ゲームのウィンドウを実装する方法とゲーム ループを移植する方法について説明します。
ms.assetid: 070dd802-cb27-4672-12ba-a7f036ff495c
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, ゲーム, 移植, ゲーム ループ, Direct3D 9, DirectX 11
ms.localizationpriority: medium
ms.openlocfilehash: bd6a17b5e1684fbee21965158295dba123737bd6
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/29/2019
ms.locfileid: "66367912"
---
# <a name="port-the-game-loop"></a>ゲーム ループの移植



**概要**

-   [パート 1: Direct3D の初期化 11](simple-port-from-direct3d-9-to-11-1-part-1--initializing-direct3d.md)
-   [パート 2: レンダリングのフレームワークを変換します。](simple-port-from-direct3d-9-to-11-1-part-2--rendering.md)
-   パート 3:ゲーム ループの移植


[  **IFrameworkView**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.IFrameworkView) を作成して、全画面表示の [**CoreWindow**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow) を制御する方法など、ユニバーサル Windows プラットフォーム (UWP) ゲームのウィンドウを実装する方法とゲーム ループを移植する方法について説明します。 「[チュートリアル: DirectX 11 とユニバーサル Windows プラットフォーム (UWP) への簡単な Direct3D 9 アプリの移植](walkthrough--simple-port-from-direct3d-9-to-11-1.md)」のパート 3 です。

## <a name="create-a-window"></a>ウィンドウの作成


Direct3D 9 のビューポートを使ってデスクトップ ウィンドウを設定するためには、デスクトップ アプリ用の従来のウィンドウ フレームワークを実装する必要がありました。 また、HWND の作成、ウィンドウ サイズの設定、ウィンドウ処理コールバックの提供、ウィンドウの表示などを実行する必要もありました。

UWP 環境には、はるかに簡単なシステムが用意されています。 DirectX を使った Microsoft Store ゲームでは、従来のウィンドウを設定する代わりに、[**IFrameworkView**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.IFrameworkView) を実装します。 このインターフェイスは、アプリ コンテナー内の [**CoreWindow**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow) で直接 DirectX アプリやゲームを実行するために存在します。

> **注**   Windows がソース アプリケーション オブジェクトなどのリソースへのマネージ ポインターを提供し、 [**CoreWindow**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow)します。 参照してください **[オブジェクト演算子 (^) へのハンドル]** https://msdn.microsoft.com/library/windows/apps/yk97tc08.aspx します。

 

"Main"クラスから継承する必要は[ **IFrameworkView** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.IFrameworkView) 、5 つの実装と**IFrameworkView**メソッド。[**初期化**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.iframeworkview.initialize)、 [ **SetWindow**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.iframeworkview.setwindow)、 [**ロード**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.iframeworkview.load)、 [ **の実行**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.iframeworkview.run)、および[**初期化**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.iframeworkview.uninitialize)します。 (実質的に) ゲームが存在する場所となる **IFrameworkView** の作成に加えて、**IFrameworkView** のインスタンスを作成するファクトリ クラスを実装する必要があります。 ゲームにはこれまでどおり **main()** と呼ばれるメソッドを含む実行可能ファイルが存在しますが、main で実行できる操作は、ファクトリを使って、**IFrameworkView** のインスタンスを作成することだけです。

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

このゲームは [**IFrameworkView**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.IFrameworkView) クラス内で機能するため、ゲーム ループは (**main()** ではなく) [**IFrameworkView::Run**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.iframeworkview.run) メソッドで実行されます。

メッセージ処理フレームワークを実装し、[**PeekMessage**](https://docs.microsoft.com/windows/desktop/api/winuser/nf-winuser-peekmessagea) を呼び出す代わりに、アプリ ウィンドウの [**CoreDispatcher**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreDispatcher) に組み込まれている [**ProcessEvents**](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.processevents) メソッドを呼び出すことができます。 ゲーム ループで分岐し、メッセージを処理する必要はありません。**ProcessEvents** を呼び出し、次に進むだけです。

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

次の Microsoft Store の詳細なゲーム開発記事を参照してください。

-   [チュートリアル: 簡単な UWP ゲームを DirectX で](tutorial--create-your-first-uwp-directx-game.md)
-   [ゲームにオーディオ](working-with-audio-in-your-directx-game.md)
-   [ゲームの移動検索コントロール](tutorial--adding-move-look-controls-to-your-directx-game.md)

 

 




