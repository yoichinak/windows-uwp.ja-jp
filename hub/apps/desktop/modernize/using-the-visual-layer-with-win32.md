---
title: Win32 でのビジュアル レイヤーの使用
description: ビジュアル レイヤーとも呼ばれるユニバーサル Windows プラットフォーム (UWP) の Windows ランタイム コンポジション API を使用して、Win32 C++ アプリの UI を強化する方法について説明します。
template: detail.hbs
ms.date: 03/18/2019
ms.topic: article
keywords: UWP, レンダリング, コンポジション, win32
ms.author: jimwalk
author: jwmsft
ms.localizationpriority: medium
ms.openlocfilehash: 98811d890aa496e05c38009dedea6978386d5e4a
ms.sourcegitcommit: cb5af00af05e838621c270173e7fde1c5d2168ef
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/28/2020
ms.locfileid: "89043474"
---
# <a name="using-the-visual-layer-with-win32"></a>Win32 でのビジュアル レイヤーの使用

Win32 アプリで Windows ランタイム コンポジション API ([ビジュアル レイヤー](/windows/uwp/composition/visual-layer)とも呼ばれる) を使用して、Windows 10 ユーザーの利便性を高める最新のエクスペリエンスを作成できます。

このチュートリアルの完全なコードは、GitHub で入手できます。[Win32 HelloComposition サンプル](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/HelloComposition).

UI コンポジションを正確に制御する必要があるユニバーサル Windows アプリケーションでは、[Windows.UI.Composition](/uwp/api/windows.ui.composition) 名前空間にアクセスして、UI の構成とレンダリングの方法をきめ細かく制御できます。 ただし、このコンポジション API は UWP アプリに限定されません。 Win32 デスクトップ アプリケーションでは、UWP と Windows 10 で最新のコンポジション システムを利用できます。

## <a name="prerequisites"></a>前提条件

UWP ホスティング API には、これらの前提条件があります。

- Win32 と UWP を使用したアプリ開発に関するいくらかの知識があることを前提としています。 For more info, see:
  - [Win32 と C++ の概要](/windows/desktop/learnwin32/learn-to-program-for-windows)
  - [Windows 10 アプリの概要](/windows/uwp/get-started/)
  - [Windows 10 向けのデスクトップ アプリを強化する](/windows/uwp/porting/desktop-to-uwp-enhance)
- Windows 10 バージョン 1803 以降
- Windows 10 SDK 17134 以降

## <a name="how-to-use-composition-apis-from-a-win32-desktop-application"></a>Win32 デスクトップ アプリケーションからコンポジション API を使用する方法

このチュートリアルでは、単純な Win32 C++ アプリを作成し、UWP コンポジション要素を追加します。 Windows Composition API を使用して、プロジェクトを正しく構成し、相互運用コードを作成して、シンプルなものを描画することに焦点を合わせています。 完成したアプリは次のようになります。

![実行中のアプリの UI](images/visual-layer-interop/win32-comp-interop-app-ui.png)

## <a name="create-a-c-win32-project-in-visual-studio"></a>Visual Studio で C++ Win32 プロジェクトを作成する

最初の手順は、Visual Studio で Win32 アプリ プロジェクトを作成することです。

C++ で _HelloComposition_ という名前の新しい Win32 アプリケーションプロジェクトを作成するには:

1. Visual Studio を開き、 **[ファイル]**  >  **[新規作成]**  >  **[プロジェクト]** の順に選択します。

    **[新しいプロジェクト]** ダイアログ ボックスが開きます。
1. **[インストール済み]** カテゴリで、 **[Visual C++]** ノードを展開し、 **[Windows デスクトップ]** を選択します。
1. **[Windows デスクトップ アプリケーション]** テンプレートを選択します。
1. 名前「_HelloComposition_」を入力し、 **[OK]** をクリックします。

    Visual Studio によってプロジェクトが作成され、メイン アプリ ファイルのエディターが開きます。

## <a name="configure-the-project-to-use-windows-runtime-apis"></a>Windows ランタイム API を使用するようにプロジェクトを構成する

Win32 アプリで Windows ランタイム (WinRT) API を使用するには、C++/WinRT を使用します。 C++/WinRT サポートを追加するには、Visual Studio プロジェクトを構成する必要があります。

(詳細については、[「C++/WinRT の使用を開始する」 - 「Windows デスクトップ アプリケーション プロジェクトを変更して C++/WinRT のサポートを追加する」](/windows/uwp/cpp-and-winrt-apis/get-started.md#modify-a-windows-desktop-application-project-to-add-cwinrt-support)を参照してください)。

1. **[プロジェクト]** メニューで、プロジェクトのプロパティ (_HelloComposition Properties_) を開き、次の設定が指定した値に設定されていることを確認します。

    - **[構成]** では、 _[すべての構成]_ を選択します。 **[プラットフォーム]** では、 _[すべてのプラットフォーム]_ を選択します。
    - **[構成プロパティ]**  >  **[全般]**  >  **[Windows SDK バージョン]**  = _10.0.17763.0_ 以上

    ![SDK バージョンの設定](images/visual-layer-interop/sdk-version.png)

    - **[C/C++]**  >  **[言語]**  >  **[C++ 言語標準]**  = _ISO C++ 17 Standard (/stf:c++17)_

    ![言語標準の設定](images/visual-layer-interop/language-standard.png)

    - **[リンカー]**  >  **[入力]**  >  **[追加の依存ファイル]** に "_windowsapp.lib_" を含める必要があります。 リストに含まれていない場合は、それを追加します。

    ![リンカーの依存関係の追加](images/visual-layer-interop/linker-dependencies.png)

1. プリコンパイル済みヘッダーを更新します

    - `stdafx.h` と `stdafx.cpp` の名前をそれぞれ `pch.h` と `pch.cpp` に変更します。
    - プロジェクトのプロパティ **[C/C++]**  >  **[プリコンパイル済みヘッダー]**  >  **[プリコンパイル済みヘッダー ファイル]** を *pch.h* に設定します。
    - すべてのファイルで、`#include "stdafx.h"` を検索して `#include "pch.h"` に置換します。

        ( **[編集]**  >  **[検索と置換]**  >  **[フォルダーを指定して検索]** )

        ![stdafx.h の検索と置換](images/visual-layer-interop/replace-stdafx.png)

    - `pch.h` に、`winrt/base.h` と `unknwn.h` を含めます。

        ```cppwinrt
        // reference additional headers your program requires here
        #include <unknwn.h>
        #include <winrt/base.h>
        ```

この時点でプロジェクトをビルドして、続行する前にエラーが発生しないことを確認することをお勧めします。

## <a name="create-a-class-to-host-composition-elements"></a>コンポジション要素をホストするクラスを作成する

ビジュアル レイヤーによって作成したコンテンツをホストするには、クラス (_CompositionHost_) を作成して、相互運用を管理し、コンポジション要素を作成します。 ここでは、以下のような Composition API をホストするための構成の大半を行います。

- [Compositor](/uwp/api/windows.ui.composition.compositor) を取得します。これにより、[Windows.UI.Composition](/uwp/api/windows.ui.composition) 名前空間のオブジェクトを作成および管理します。
- [DispatcherQueueController](/uwp/api/windows.system.dispatcherqueuecontroller)/[DispatcherQueue](/uwp/api/windows.system.dispatcherqueue) を作成して、WinRT API のタスクを管理します。
- コンポジション オブジェクトを表示するために [DesktopWindowTarget](/uwp/api/windows.ui.composition.desktop.desktopwindowtarget) とコンポジション コンテナーを作成します。

スレッドの問題を回避するため、このクラスはシングルトンにします。 たとえば、スレッドごとに作成できるディスパッチャー キューは 1 つだけであるため、同じスレッドで 2 つ目の CompositionHost のインスタンスをインスタンス化すると、エラーが発生します。

> [!TIP]
> チュートリアルを進めながら、必要に応じて、チュートリアルの最後にある完全なコードを確認して、すべてのコードが適切な場所にあることを確認してください。

1. プロジェクトに新しいクラス ファイルを追加します。
    - **ソリューション エクスプローラー**で、_HelloComposition_ プロジェクトを右クリックします。
    - コンテキスト メニューで、 **[追加]**  >  **[クラス...]** を選択します。
    - **[クラスの追加]** ダイアログ ボックスで、クラスに _CompositionHost.cs_ という名前を付けて、 **[追加]** をクリックします。

1. コンポジション相互運用機能に必要なヘッダーと _using_ を含めます。
    - CompositionHost.h で、ファイルの先頭に、これらの _include_ を追加します。

    ```cppwinrt
    #pragma once
    #include <winrt/Windows.UI.Composition.Desktop.h>
    #include <windows.ui.composition.interop.h>
    #include <DispatcherQueue.h>
    ```

    - CompositionHost.cpp で、ファイルの先頭のすべての _include_ の後に、_using_ を追加します。

    ```cppwinrt
    using namespace winrt;
    using namespace Windows::System;
    using namespace Windows::UI;
    using namespace Windows::UI::Composition;
    using namespace Windows::UI::Composition::Desktop;
    using namespace Windows::Foundation::Numerics;
    ```

1. シングルトン パターンを使用するようにクラスを編集します。
    - CompositionHost.h で、コンストラクターをプライベートにします。
    - パブリック静的 _GetInstance_ メソッドを宣言します。

    ```cppwinrt
    class CompositionHost
    {
    public:
        ~CompositionHost();
        static CompositionHost* GetInstance();

    private:
        CompositionHost();
    };
    ```

    - CompositionHost.cpp で、_GetInstance_ メソッドの定義を追加します。

    ```cppwinrt
    CompositionHost* CompositionHost::GetInstance()
    {
        static CompositionHost instance;
        return &instance;
    }
    ```

1. CompositionHost.h で、Compositor、DispatcherQueueController、および DesktopWindowTarget のプライベート メンバー変数を宣言します。

    ```cppwinrt
    winrt::Windows::UI::Composition::Compositor m_compositor{ nullptr };
    winrt::Windows::System::DispatcherQueueController m_dispatcherQueueController{ nullptr };
    winrt::Windows::UI::Composition::Desktop::DesktopWindowTarget m_target{ nullptr };
    ```

1. コンポジション相互運用オブジェクトを初期化するパブリック メソッドを追加します。
    > [!NOTE]
    > _Initialize_ では、_EnsureDispatcherQueue_、_CreateDesktopWindowTarget_、および _CreateCompositionRoot_ メソッドを呼び出します。 これらのメソッドは、次の手順で作成します。

    - CompositionHost.h で、HWND を引数として受け取る _Initialize_ という名前のパブリック メソッドを宣言します。

    ```cppwinrt
    void Initialize(HWND hwnd);
    ```

    - CompositionHost.cpp で、_Initialize_ メソッドの定義を追加します。

    ```cppwinrt
    void CompositionHost::Initialize(HWND hwnd)
    {
        EnsureDispatcherQueue();
        if (m_dispatcherQueueController) m_compositor = Compositor();

        CreateDesktopWindowTarget(hwnd);
        CreateCompositionRoot();
    }
    ```

1. Windows Composition を使用するスレッドで、ディスパッチャー キューを作成します。

    Compositor は、ディスパッチャー キューがあるスレッドで作成される必要があるため、このメソッドは初期化中に最初に呼び出されます。

    - CompositionHost.h で、_EnsureDispatcherQueue_ という名前のプライベート メソッドを宣言します。

    ```cppwinrt
    void EnsureDispatcherQueue();
    ```

    - CompositionHost.cpp で、_EnsureDispatcherQueue_ メソッドの定義を追加します。

    ```cppwinrt
    void CompositionHost::EnsureDispatcherQueue()
    {
        namespace abi = ABI::Windows::System;

        if (m_dispatcherQueueController == nullptr)
        {
            DispatcherQueueOptions options
            {
                sizeof(DispatcherQueueOptions), /* dwSize */
                DQTYPE_THREAD_CURRENT,          /* threadType */
                DQTAT_COM_ASTA                  /* apartmentType */
            };

            Windows::System::DispatcherQueueController controller{ nullptr };
            check_hresult(CreateDispatcherQueueController(options, reinterpret_cast<abi::IDispatcherQueueController**>(put_abi(controller))));
            m_dispatcherQueueController = controller;
        }
    }
    ```

1. アプリのウィンドウをコンポジション ターゲットとして登録します。
    - CompositionHost.h で、引数として HWND を受け取る _CreateDesktopWindowTarget_ という名前のプライベート メソッドを宣言します。

    ```cppwinrt
    void CreateDesktopWindowTarget(HWND window);
    ```

    - CompositionHost.cpp で、_CreateDesktopWindowTarget_ メソッドの定義を追加します。

    ```cppwinrt
    void CompositionHost::CreateDesktopWindowTarget(HWND window)
    {
        namespace abi = ABI::Windows::UI::Composition::Desktop;

        auto interop = m_compositor.as<abi::ICompositorDesktopInterop>();
        DesktopWindowTarget target{ nullptr };
        check_hresult(interop->CreateDesktopWindowTarget(window, false, reinterpret_cast<abi::IDesktopWindowTarget**>(put_abi(target))));
        m_target = target;
    }
    ```

1. ビジュアル オブジェクトを保持するルート ビジュアル コンテナーを作成します。
    - CompositionHost.h で、_CreateCompositionRoot_ という名前のプライベート メソッドを宣言します。

    ```cppwinrt
    void CreateCompositionRoot();
    ```

    - CompositionHost.cpp で、_CreateCompositionRoot_ メソッドの定義を追加します。

    ```cppwinrt
    void CompositionHost::CreateCompositionRoot()
    {
        auto root = m_compositor.CreateContainerVisual();
        root.RelativeSizeAdjustment({ 1.0f, 1.0f });
        root.Offset({ 124, 12, 0 });
        m_target.Root(root);
    }
    ```

ここでプロジェクトをビルドして、エラーが発生しないことを確認します。

これらのメソッドでは、UWP ビジュアル レイヤーと Win32 API 間の相互運用に必要なコンポーネントをセットアップします。 これで、アプリにコンテンツを追加できるようになりました。

### <a name="add-composition-elements"></a>コンポジション要素を追加する

インフラストラクチャが配置されたので、表示するコンポジション コンテンツを生成できるようになりました。

この例では、短い遅延の後にドロップさせるアニメーションを使用して、ランダムに色付けされた正方形 [SpriteVisual](/uwp/api/windows.ui.composition.spritevisual) を作成するコードを追加します。

1. コンポジション要素を追加します。
    - CompositionHost.h で、3 つの **float** 値を引数として受け取る _AddElement_ という名前のパブリック メソッドを宣言します。

    ```cppwinrt
    void AddElement(float size, float x, float y);
    ```

    - CompositionHost.cpp で、_AddElement_ メソッドの定義を追加します。

    ```cppwinrt
    void CompositionHost::AddElement(float size, float x, float y)
    {
        if (m_target.Root())
        {
            auto visuals = m_target.Root().as<ContainerVisual>().Children();
            auto visual = m_compositor.CreateSpriteVisual();

            auto element = m_compositor.CreateSpriteVisual();
            uint8_t r = (double)(double)(rand() % 255);;
            uint8_t g = (double)(double)(rand() % 255);;
            uint8_t b = (double)(double)(rand() % 255);;

            element.Brush(m_compositor.CreateColorBrush({ 255, r, g, b }));
            element.Size({ size, size });
            element.Offset({ x, y, 0.0f, });

            auto animation = m_compositor.CreateVector3KeyFrameAnimation();
            auto bottom = (float)600 - element.Size().y;
            animation.InsertKeyFrame(1, { element.Offset().x, bottom, 0 });

            using timeSpan = std::chrono::duration<int, std::ratio<1, 1>>;

            std::chrono::seconds duration(2);
            std::chrono::seconds delay(3);

            animation.Duration(timeSpan(duration));
            animation.DelayTime(timeSpan(delay));
            element.StartAnimation(L"Offset", animation);
            visuals.InsertAtTop(element);

            visuals.InsertAtTop(visual);
        }
    }
    ```

## <a name="create-and-show-the-window"></a>ウィンドウを作成して表示する

これで、Win32 UI にボタンと UWP コンポジション コンテンツを追加できるようになりました。

1. HelloComposition.cpp で、ファイルの先頭に _CompositionHost.h_ をインクルードし、BTN_ADD を定義して、CompositionHost のインスタンスを取得します。

    ```cppwinrt
    #include "CompositionHost.h"

    // #define MAX_LOADSTRING 100 // This is already in the file.
    #define BTN_ADD 1000

    CompositionHost* compHost = CompositionHost::GetInstance();
    ```

1. `InitInstance` メソッドで、作成されるウィンドウのサイズを変更します。 (この行では、`CW_USEDEFAULT, 0` を `900, 672` に変更します)。

    ```cppwinrt
    HWND hWnd = CreateWindowW(szWindowClass, szTitle, WS_OVERLAPPEDWINDOW,
        CW_USEDEFAULT, 0, 900, 672, nullptr, nullptr, hInstance, nullptr);
    ```

1. WndProc 関数で、_message_ switch ブロックに `case WM_CREATE` を追加します。 この場合は、CompositionHost を初期化して、ボタンを作成します。

    ```cppwinrt
    case WM_CREATE:
    {
        compHost->Initialize(hWnd);
        srand(time(nullptr));

        CreateWindow(TEXT("button"), TEXT("Add element"),
            WS_VISIBLE | WS_CHILD | BS_PUSHBUTTON,
            12, 12, 100, 50,
            hWnd, (HMENU)BTN_ADD, nullptr, nullptr);
    }
    break;
    ```

1. また、WndProc 関数で、ボタン クリックを処理し、コンポジション要素を UI に追加します。 

    WM_COMMAND ブロック内の _wmId_ switch ブロックに `case BTN_ADD` を追加します。

    ```cppwinrt
    case BTN_ADD: // addButton click
    {
        double size = (double)(rand() % 150 + 50);
        double x = (double)(rand() % 600);
        double y = (double)(rand() % 200);
        compHost->AddElement(size, x, y);
        break;
    }
    ```

これで、アプリをビルドして実行できるようになりました。 必要に応じて、チュートリアルの最後にある完全なコードを確認して、すべてのコードが適切な場所にあることを確認してください。

アプリを実行して、ボタンをクリックすると、UI に追加されたアニメーションの正方形が表示されるはずです。

## <a name="additional-resources"></a>その他の資料

- [Win32 HelloComposition サンプル (GitHub)](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/HelloComposition)
- [Win32 と C++ の概要](/windows/desktop/learnwin32/learn-to-program-for-windows)
- [Windows 10 アプリの概要](/windows/uwp/get-started/) (UWP)
- [Windows 10 向けのデスクトップ アプリケーションの強化](/windows/uwp/porting/desktop-to-uwp-enhance) (UWP)
- [Windows.UI.Composition 名前空間](/uwp/api/windows.ui.composition) (UWP)

## <a name="complete-code"></a>コードを完成させる

CompositionHost クラスと InitInstance メソッドの完全なコードを次に示します。

### <a name="compositionhosth"></a>CompositionHost.h

```cppwinrt
#pragma once
#include <winrt/Windows.UI.Composition.Desktop.h>
#include <windows.ui.composition.interop.h>
#include <DispatcherQueue.h>

class CompositionHost
{
public:
    ~CompositionHost();
    static CompositionHost* GetInstance();

    void Initialize(HWND hwnd);
    void AddElement(float size, float x, float y);

private:
    CompositionHost();

    void CreateDesktopWindowTarget(HWND window);
    void EnsureDispatcherQueue();
    void CreateCompositionRoot();

    winrt::Windows::UI::Composition::Compositor m_compositor{ nullptr };
    winrt::Windows::UI::Composition::Desktop::DesktopWindowTarget m_target{ nullptr };
    winrt::Windows::System::DispatcherQueueController m_dispatcherQueueController{ nullptr };
};
```

### <a name="compositionhostcpp"></a>CompositionHost.cpp

```cppwinrt
#include "pch.h"
#include "CompositionHost.h"

using namespace winrt;
using namespace Windows::System;
using namespace Windows::UI;
using namespace Windows::UI::Composition;
using namespace Windows::UI::Composition::Desktop;
using namespace Windows::Foundation::Numerics;

CompositionHost::CompositionHost()
{
}

CompositionHost* CompositionHost::GetInstance()
{
    static CompositionHost instance;
    return &instance;
}

CompositionHost::~CompositionHost()
{
}

void CompositionHost::Initialize(HWND hwnd)
{
    EnsureDispatcherQueue();
    if (m_dispatcherQueueController) m_compositor = Compositor();

    if (m_compositor)
    {
        CreateDesktopWindowTarget(hwnd);
        CreateCompositionRoot();
    }
}

void CompositionHost::EnsureDispatcherQueue()
{
    namespace abi = ABI::Windows::System;

    if (m_dispatcherQueueController == nullptr)
    {
        DispatcherQueueOptions options
        {
            sizeof(DispatcherQueueOptions), /* dwSize */
            DQTYPE_THREAD_CURRENT,          /* threadType */
            DQTAT_COM_ASTA                  /* apartmentType */
        };

        Windows::System::DispatcherQueueController controller{ nullptr };
        check_hresult(CreateDispatcherQueueController(options, reinterpret_cast<abi::IDispatcherQueueController**>(put_abi(controller))));
        m_dispatcherQueueController = controller;
    }
}

void CompositionHost::CreateDesktopWindowTarget(HWND window)
{
    namespace abi = ABI::Windows::UI::Composition::Desktop;

    auto interop = m_compositor.as<abi::ICompositorDesktopInterop>();
    DesktopWindowTarget target{ nullptr };
    check_hresult(interop->CreateDesktopWindowTarget(window, false, reinterpret_cast<abi::IDesktopWindowTarget**>(put_abi(target))));
    m_target = target;
}

void CompositionHost::CreateCompositionRoot()
{
    auto root = m_compositor.CreateContainerVisual();
    root.RelativeSizeAdjustment({ 1.0f, 1.0f });
    root.Offset({ 124, 12, 0 });
    m_target.Root(root);
}

void CompositionHost::AddElement(float size, float x, float y)
{
    if (m_target.Root())
    {
        auto visuals = m_target.Root().as<ContainerVisual>().Children();
        auto visual = m_compositor.CreateSpriteVisual();

        auto element = m_compositor.CreateSpriteVisual();
        uint8_t r = (double)(double)(rand() % 255);;
        uint8_t g = (double)(double)(rand() % 255);;
        uint8_t b = (double)(double)(rand() % 255);;

        element.Brush(m_compositor.CreateColorBrush({ 255, r, g, b }));
        element.Size({ size, size });
        element.Offset({ x, y, 0.0f, });

        auto animation = m_compositor.CreateVector3KeyFrameAnimation();
        auto bottom = (float)600 - element.Size().y;
        animation.InsertKeyFrame(1, { element.Offset().x, bottom, 0 });

        using timeSpan = std::chrono::duration<int, std::ratio<1, 1>>;

        std::chrono::seconds duration(2);
        std::chrono::seconds delay(3);

        animation.Duration(timeSpan(duration));
        animation.DelayTime(timeSpan(delay));
        element.StartAnimation(L"Offset", animation);
        visuals.InsertAtTop(element);

        visuals.InsertAtTop(visual);
    }
}
```

### <a name="hellocompositioncpp-partial"></a>HelloComposition.cpp (一部)

```cppwinrt
#include "pch.h"
#include "HelloComposition.h"
#include "CompositionHost.h"

#define MAX_LOADSTRING 100
#define BTN_ADD 1000

CompositionHost* compHost = CompositionHost::GetInstance();

// Global Variables:

// ...
// ... code not shown ...
// ...

BOOL InitInstance(HINSTANCE hInstance, int nCmdShow)
{
   hInst = hInstance; // Store instance handle in our global variable

   HWND hWnd = CreateWindowW(szWindowClass, szTitle, WS_OVERLAPPEDWINDOW,
      CW_USEDEFAULT, 0, 900, 672, nullptr, nullptr, hInstance, nullptr);

// ...
// ... code not shown ...
// ...
}

// ...

LRESULT CALLBACK WndProc(HWND hWnd, UINT message, WPARAM wParam, LPARAM lParam)
{
    switch (message)
    {
// Add this...
    case WM_CREATE:
    {
        compHost->Initialize(hWnd);
        srand(time(nullptr));

        CreateWindow(TEXT("button"), TEXT("Add element"),
            WS_VISIBLE | WS_CHILD | BS_PUSHBUTTON,
            12, 12, 100, 50,
            hWnd, (HMENU)BTN_ADD, nullptr, nullptr);
    }
    break;
// ...
    case WM_COMMAND:
    {
        int wmId = LOWORD(wParam);
        // Parse the menu selections:
        switch (wmId)
        {
        case IDM_ABOUT:
            DialogBox(hInst, MAKEINTRESOURCE(IDD_ABOUTBOX), hWnd, About);
            break;
        case IDM_EXIT:
            DestroyWindow(hWnd);
            break;
// Add this...
        case BTN_ADD: // addButton click
        {
            double size = (double)(rand() % 150 + 50);
            double x = (double)(rand() % 600);
            double y = (double)(rand() % 200);
            compHost->AddElement(size, x, y);
            break;
        }
// ...
        default:
            return DefWindowProc(hWnd, message, wParam, lParam);
        }
    }
    break;
    case WM_PAINT:
    {
        PAINTSTRUCT ps;
        HDC hdc = BeginPaint(hWnd, &ps);
        // TODO: Add any drawing code that uses hdc here...
        EndPaint(hWnd, &ps);
    }
    break;
    case WM_DESTROY:
        PostQuitMessage(0);
        break;
    default:
        return DefWindowProc(hWnd, message, wParam, lParam);
    }
    return 0;
}

// ...
// ... code not shown ...
// ...
```