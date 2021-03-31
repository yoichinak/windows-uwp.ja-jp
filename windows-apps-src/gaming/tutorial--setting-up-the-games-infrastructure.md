---
title: ゲーム プロジェクトのセットアップ
description: ゲームを開発するための最初の手順は、Microsoft Visual Studio でプロジェクトを設定することです。 ゲーム開発専用のプロジェクトを構成した後、後でテンプレートの一種として再利用できます。
ms.assetid: 9fde90b3-bf79-bcb3-03b6-d38ab85803f2
ms.date: 06/24/2020
ms.topic: article
keywords: Windows 10, UWP, ゲーム, セットアップ, DirectX
ms.localizationpriority: medium
ms.openlocfilehash: 553069897cc71b45c34159749b7c16bd0af73c7b
ms.sourcegitcommit: 249100d990cd5cf2854c59fa66803b7f83d5db96
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/30/2021
ms.locfileid: "105939097"
---
# <a name="set-up-the-game-project"></a>ゲーム プロジェクトのセットアップ

> [!NOTE]
> このトピックは、「DirectX チュートリアルシリーズ [を含む simple ユニバーサル Windows プラットフォーム (UWP) ゲームの作成](tutorial--create-your-first-uwp-directx-game.md) 」に含まれています。 このリンクのトピックでは、系列のコンテキストを設定します。

ゲームを開発するための最初の手順は、Microsoft Visual Studio でプロジェクトを作成することです。 ゲーム開発専用のプロジェクトを構成した後、後でテンプレートの一種として再利用できます。

## <a name="objectives"></a>目標

* プロジェクトテンプレートを使用して、Visual Studio で新しいプロジェクトを作成します。
* **アプリ** クラスのソースファイルを調べることで、ゲームのエントリポイントと初期化について理解します。
* ゲームループを確認します。
* プロジェクトの **package.appxmanifest** ファイルを確認します。

## <a name="create-a-new-project-in-visual-studio"></a>Visual Studio での新しいプロジェクトの作成

> [!NOTE]
> &mdash;C++/WinRT Visual Studio Extension (VSIX) と NuGet パッケージ (両者が連携してプロジェクト テンプレートとビルドをサポート) のインストールと使用など、&mdash;C++/WinRT 開発用に Visual Studio を設定する方法については、[Visual Studio での C++/WinRT のサポート](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)に関する記事を参照してください。

最初に、最新バージョンの C++/WinRT Visual Studio 拡張機能 (VSIX) をインストール (または更新) します。上のメモを参照してください。 次に、Visual Studio で、 **コアアプリ (C++/WinRT)** プロジェクトテンプレートに基づいて新しいプロジェクトを作成します。 Windows SDK の最新の一般公開された (プレビュー以外の) バージョンを対象とします。

## <a name="review-the-app-class-to-understand-iframeworkviewsource-and-iframeworkview"></a>**Iframeworkviewsource** と **IFrameworkView** を理解するために **App** クラスを確認する

コアアプリプロジェクトで、ソースコードファイルを開き `App.cpp` ます。 には、アプリとそのライフサイクルを表す **app** クラスの実装があります。 この場合、もちろん、アプリがゲームであることがわかります。 しかし、ユニバーサル Windows プラットフォーム (UWP) アプリの初期化方法については、 *アプリ* と呼ばれています。

### <a name="the-wwinmain-function"></a>WWinMain 関数

**Wwinmain** 関数は、アプリのエントリポイントです。 **Wwinmain** は次のように `App.cpp` なります (から)。

```cppwinrt
int __stdcall wWinMain(HINSTANCE, HINSTANCE, PWSTR, int)
{
    CoreApplication::Run(winrt::make<App>());
}
```

**App** クラスのインスタンスを作成します (これは、作成される **アプリ** の唯一のインスタンスであり、それを静的 [**CoreApplication**](/uwp/api/windows.applicationmodel.core.coreapplication.run)メソッドに渡します)。 **CoreApplication** には [**Iframeworkviewsource**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource)インターフェイスが必要であることに注意してください。 そのため、 **App** クラスはそのインターフェイスを実装する必要があります。

このトピックの次の2つのセクションでは、 [**Iframeworkviewsource**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource) インターフェイスと [**IFrameworkView**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource) インターフェイスについて説明します。 これらのインターフェイス (および **CoreApplication**) は、アプリが *ビュープロバイダー* で Windows を提供する方法を表します。 Windows は、アプリケーションライフサイクルイベントを処理できるように、そのビュープロバイダーを使用してアプリを Windows シェルに接続します。

### <a name="the-iframeworkviewsource-interface"></a>IFrameworkViewSource インターフェイス

**App** クラスは、次の一覧に示すように、実際に [**Iframeworkviewsource**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource)を実装します。

```cppwinrt
struct App : winrt::implements<App, IFrameworkViewSource, IFrameworkView>
{
    ...
    IFrameworkView CreateView()
    {
        return *this;
    }
    ...
}
```

**Iframeworkviewsource** を実装するオブジェクトは、*ビュープロバイダーファクトリ* オブジェクトです。 このオブジェクトのジョブは、 *ビュープロバイダー* オブジェクトを製造して返すことです。

**Iframeworkviewsource** には、 [**Iframeworkviewsource:: CreateView**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource.createview)という1つのメソッドがあります。 Windows は、 **CoreApplication** に渡すオブジェクトに対して関数を呼び出します。 前述のように、そのメソッドの **App:: CreateView** 実装はを返し `*this` ます。 つまり、 **アプリ** オブジェクトはそれ自体を返します。 **Iframeworkviewsource:: CreateView** の戻り値の型は [**IFrameworkView**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource)であるため、 **App** クラスでも *その* インターフェイスを実装する必要があることに従います。 上記の一覧に表示されていることを確認できます。

### <a name="the-iframeworkview-interface"></a>IFrameworkView インターフェイス

[**IFrameworkView**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource)を実装するオブジェクトは、*ビュープロバイダー* オブジェクトです。 これで、そのビュープロバイダーで Windows が提供されました。 これは、 **Wwinmain** で作成した **アプリ** オブジェクトと同じです。 そのため、 **App** クラスは、 *ビュープロバイダーファクトリ* と *ビュープロバイダー* の両方として機能します。

Windows は、 **IFrameworkView** のメソッドの **App** クラスの実装を呼び出すことができるようになりました。 これらのメソッドの実装では、アプリは初期化などのタスクを実行し、必要なリソースの読み込みを開始し、適切なイベントハンドラーを接続して、アプリが出力を表示するために使用する [**Corewindow**](/uwp/api/windows.ui.core.corewindow) を受け取る機会を与えます。

**IFrameworkView** のメソッドの実装は、次に示す順序で呼び出されます。

- [**初期化する**](/uwp/api/windows.applicationmodel.core.iframeworkview.initialize)
- [**SetWindow**](/uwp/api/windows.applicationmodel.core.iframeworkview.setwindow)
- [**読み込み**](/uwp/api/windows.applicationmodel.core.iframeworkview.load)
- [**Coreapplicationview:: アクティブ化**](/uwp/api/windows.applicationmodel.core.coreapplicationview.activated)イベントが発生します。 そのため、そのイベントを処理するために (必要に応じて) 登録した場合は、この時点で **Onactivated 化** ハンドラーが呼び出されます。
- [**ラン**](/uwp/api/windows.applicationmodel.core.iframeworkview.run)
- [**解除**](/uwp/api/windows.applicationmodel.core.iframeworkview.uninitialize)

ここで、 **App** クラスのスケルトン (では) が、 `App.cpp` これらのメソッドのシグネチャを示しています。

```cppwinrt
struct App : winrt::implements<App, IFrameworkViewSource, IFrameworkView>
{
    ...
    void Initialize(Windows::ApplicationModel::Core::CoreApplicationView const& applicationView) { ... }
    void SetWindow(Windows::UI::Core::CoreWindow const& window) { ... }
    void Load(winrt::hstring const& entryPoint) { ... }
    void OnActivated(
        Windows::ApplicationModel::Core::CoreApplicationView const& applicationView,
        Windows::ApplicationModel::Activation::IActivatedEventArgs const& args) { ... }
    void Run() { ... }
    void Uninitialize() { ... }
    ...
}
```

ここでは、 **IFrameworkView** の概要について説明しました。 これらのメソッドの詳細と実装方法については、「 [ゲームの UWP アプリフレームワークを定義](tutorial--building-the-games-uwp-app-framework.md)する」を参照してください。

### <a name="tidy-up-the-project"></a>プロジェクトを整理する

プロジェクトテンプレートから作成したコアアプリプロジェクトには、この時点で整理する必要がある機能が含まれています。 その後、プロジェクトを使用して、プレイギャラリーゲーム (**Simple3DGameDX**) を再作成できます。 で、 **App** クラスに次の変更を行い `App.cpp` ます。

- そのデータメンバーを削除します。
- **Onポインタ押さ** れた、 **onポインターが移動** された、および **addvisual** を削除します。
- **Setwindow** からコードを削除します。

プロジェクトがビルドされて実行されますが、クライアント領域には純色のみが表示されます。

## <a name="the-game-loop"></a>ゲーム ループ

ゲームループがどのようなものかを理解するには、ダウンロードした [Simple3DGameDX](/samples/microsoft/windows-universal-samples/simple3dgamedx/) サンプルゲームのソースコードを確認します。

**App** クラスには、 **GameMain** 型のデータメンバー *m_main* という名前が付けられています。 このメンバーは、 **App::** のように実行されます。

```cppwinrt
void Run()
{
    m_main->Run();
}
```

**GameMain:: Run** in を見つけることができ `GameMain.cpp` ます。 これはゲームのメインループであり、最も重要な機能を示しています。

```cppwinrt
void GameMain::Run()
{
    while (!m_windowClosed)
    {
        if (m_visible)
        {
            CoreWindow::GetForCurrentThread().Dispatcher().ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);
            Update();
            m_renderer->Render();
            m_deviceResources->Present();
        }
        else
        {
            CoreWindow::GetForCurrentThread().Dispatcher().ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
        }
    }
}
```

ここでは、この主なゲームループの機能について簡単に説明します。

ゲームのウィンドウが閉じていない場合は、すべてのイベントをディスパッチし、タイマーを更新してから、グラフィックスパイプラインの結果を表示して表示します。 これらの問題についてはさらに詳しく説明します。これらのトピックでは、 [ゲームの UWP アプリフレームワークの定義](tutorial--building-the-games-uwp-app-framework.md)、 [レンダリングフレームワーク I: レンダリングの概要](tutorial--assembling-the-rendering-pipeline.md)、 [レンダリングフレームワーク II: ゲームレンダリング](tutorial-game-rendering.md)について説明します。 ただし、これは UWP DirectX ゲームの基本的なコード構造です。

## <a name="review-and-update-the-packageappxmanifest-file"></a>package.appxmanifest ファイルを確認して更新する

**Package.appxmanifest** ファイルには、UWP プロジェクトに関するメタデータが含まれています。 これらのメタデータは、ゲームのパッケージ化と起動、および Microsoft Store への送信に使用されます。 また、このファイルには、ゲームの実行に必要なシステムリソースへのアクセスを提供するために、windows media player のシステムで使用される重要な情報も含まれています。

**ソリューションエクスプローラー** で **package.appxmanifest** ファイルをダブルクリックして、**マニフェストデザイナー** を起動します。

![package.appxmanifest エディターのスクリーンショット。](images/simple-dx-game-setup-app-manifest.png)

**package.appxmanifest** ファイルとパッケージ化について詳しくは、[マニフェスト デザイナー](/previous-versions/br230259(v=vs.140))に関するページをご覧ください。 ここでは、[ **機能** ] タブを見て、表示されるオプションを確認します。

![Direct3D アプリの既定の機能のスクリーンショット。](images/simple-dx-game-setup-capabilities.png)

ゲームで使用する機能 (グローバルハイスコアボードの **インターネット** へのアクセスなど) を選択しない場合、対応するリソースや機能にアクセスすることはできません。 新しいゲームを作成するときは、ゲームが呼び出す Api に必要な機能を必ず選択してください。

次に、 **DirectX 11 アプリ (ユニバーサル Windows)** テンプレートに付属しているファイルの残りの部分を見てみましょう。

## <a name="review-other-important-libraries-and-source-code-files"></a>その他の重要なライブラリとソースコードファイルを確認する

自分用のゲームプロジェクトテンプレートを作成して、それを今後のプロジェクトの開始点として再利用できるようにする場合は、 `GameMain.h` ダウンロードした Simple3DGameDX プロジェクトをコピーして、 `GameMain.cpp` 新しい[](/samples/microsoft/windows-universal-samples/simple3dgamedx/)コアアプリプロジェクトに追加することをお勧めします。 これらのファイルを調査し、その内容を学習し、 **Simple3DGameDX** に固有のものをすべて削除します。 また、まだコピーしていないコードに依存するものもコメント化します。 一例として、は `GameMain.h` に依存 `GameRenderer.h` します。 **Simple3DGameDX** からさらにファイルをコピーすると、コメントを解除できるようになります。

ここでは、テンプレートに含めると便利な **Simple3DGameDX** のファイルの一部について簡単に説明します (作成している場合)。 どのような場合でも、 **Simple3DGameDX** 自体がどのように機能するかを理解するには、これらは同様に重要です。

|ソース ファイル|ファイル フォルダー|説明|
|------------------------------|------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|DeviceResources.h/.cpp|Utilities|すべての DirectX [デバイスリソース](tutorial--assembling-the-rendering-pipeline.md#resource)を制御する **DeviceResources** クラスを定義します。 また、は、グラフィックスアダプターデバイスが紛失または再作成されたことをアプリケーションに通知するために使用される **IDeviceNotify** インターフェイスも定義します。|
|DirectXSample.h|Utilities|**ConvertDipsToPixels** などのヘルパー関数を実装します。 **ConvertDipsToPixels** は、デバイスに依存しないピクセル (DIP) 単位の長さを物理ピクセル単位の長さに変換します。|
|このようにしてください。 h/.cpp|Utilities|ゲーム アプリまたは対話型レンダリング アプリで役に立つ、高分解能タイマーを定義します。|
|GameRenderer.h/.cpp|表示|基本的なレンダリングパイプラインを実装する **GameRenderer** クラスを定義します。|
|"お持ちの Hud. h/.cpp"|表示|Direct2D と DirectWrite を使用して、ゲームのヘッドアップディスプレイ (HUD) をレンダリングするクラスを定義します。|
|VertexShader. hlsl および VertexShaderFlat|シェーダー|基本的な頂点シェーダーの高レベルシェーダー言語 (HLSL) コードを格納します。|
|PixelShader および PixelShaderFlat|シェーダー|基本ピクセルシェーダーの高レベルシェーダー言語 (HLSL) コードを格納します。|
|ConstantBuffers .hlsli|シェーダー|モデルビュープロジェクション (MVP) 行列と頂点ごとのデータを頂点シェーダーに渡すために使用される、定数バッファーおよびシェーダー構造体のデータ構造の定義を格納します。|
|pch.h/.cpp|該当なし|共通の C++/WinRT、Windows、および DirectX のインクルードが含まれています。|

### <a name="next-steps"></a>次の手順

この時点で、DirectX ゲーム用の新しい UWP プロジェクトを作成し、その中のいくつかの部分を確認し、そのプロジェクトをゲーム用に再利用可能なテンプレートに変換する方法を検討しました。 また、 **Simple3DGameDX** サンプルゲームの重要な部分についても見てきました。

次のセクションでは [、ゲームの UWP アプリフレームワークを定義](tutorial--building-the-games-uwp-app-framework.md)します。 ここでは、 **Simple3DGameDX** のしくみについて詳しく見ていきます。