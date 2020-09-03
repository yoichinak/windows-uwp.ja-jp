---
description: この記事では、XAML ホスティング API を使用して C++ Win32 アプリで標準の UWP コントロールをホストする方法を示します。
title: XAML Islands を使用して C++ Win32 アプリで標準 UWP コントロールをホストする
ms.date: 03/23/2020
ms.topic: article
keywords: Windows 10, UWP, cpp, Win32, XAML Islands, ラップされたコントロール, 標準コントロール
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 0842046419402bbfacc24331d0521efa9510153a
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174196"
---
# <a name="host-a-standard-uwp-control-in-a-c-win32-app"></a>C++ Win32 アプリで標準 UWP コントロールをホストする

この記事では、[UWP XAML ホスティング API](using-the-xaml-hosting-api.md) を使用して、新しい C++ Win32 アプリで標準の UWP コントロール (つまり、Windows SDK によって提供されるコントロール) をホストする方法について説明します。 コードは[単純な XAML Islands のサンプル](https://github.com/microsoft/Xaml-Islands-Samples/tree/master/Standalone_Samples/CppWinRT_Basic_Win32App)に基づいています。このセクションでは、コードの最も重要ないくつかの部分について説明します。 C++ Win32 アプリ プロジェクトが既にある場合は、以下の手順とコード例をプロジェクトに合わせて調整できます。

> [!NOTE]
> この記事で説明するシナリオでは、アプリでホストされている UWP コントロールの XAML マークアップを直接編集することはサポートされていません。 このシナリオは、ホストされている UWP コントロールの外観と動作をコードで変更することに限定されています。 UWP コントロールをホストするときに XAML マークアップを直接編集できるようにする手順については、「[C++ Win32 アプリでカスタム UWP コントロールをホストする](host-custom-control-with-xaml-islands-cpp.md)」を参照してください。

## <a name="create-a-desktop-application-project"></a>デスクトップ アプリケーション プロジェクトを作成する

1. Windows 10 バージョン 1903 SDK (バージョン 10.0.18362) またはそれ以降のリリースがインストールされている Visual Studio 2019 で、新しい **Windows デスクトップ アプリケーション** プロジェクトを作成し、**MyDesktopWin32App** という名前を付けます。 このプロジェクトの種類は、**C++** 、**Windows**、および**デスクトップ**のプロジェクト フィルターで使用できます。

2. **ソリューション エクスプローラー**でソリューション ノードを右クリックして、 **[ソリューションの再ターゲット]** をクリックし、**10.0.18362.0** 以降の SDK リリースを選択してから、 **[OK]** をクリックします。

3. [Microsoft.Windows.CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) NuGet パッケージをインストールして、プロジェクトでの [C++/WinRT](/windows/uwp/cpp-and-winrt-apis) のサポートを有効にします。

    1. **ソリューション エクスプローラー**でプロジェクトを右クリックし、 **[NuGet パッケージの管理]** を選択します。
    2. **[参照]** タブを選択し、[Microsoft.Windows.CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) パッケージを探して、このパッケージの最新バージョンをインストールします。

    > [!NOTE]
    > 新しいプロジェクトの場合は、[C++/WinRT Visual Studio 拡張機能 (VSIX)](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264) をインストールし、その拡張機能に含まれているいずれかの C++/WinRT プロジェクト テンプレートを使用できます。 詳細については、[こちらの記事](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)を参照してください。

4. [Microsoft.Toolkit.Win32.UI.SDK](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.SDK) NuGet パッケージをインストールします。

    1. **[NuGet パッケージ マネージャー]** ウィンドウで、 **[プレリリースを含める]** が選択されていることを確認します。
    2. **[参照]** タブを選択し、**Microsoft.Toolkit.Win32.UI.SDK** パッケージを見つけて、このパッケージの v6.0.0 以降のバージョンをインストールします。 このパッケージでは、XAML Islands がアプリで動作できるようにする、いくつかのビルド資産と実行時資産が提供されています。

5. [アプリケーション マニフェスト](/windows/desktop/SbsCs/application-manifests)内に `maxVersionTested` 値を設定して、アプリケーションが Windows 10 バージョン 1903 以降と互換性があることを指定します。

    1. プロジェクトにアプリケーション マニフェストがまだない場合は、新しい XML ファイルをプロジェクトに追加し、**app.manifest** という名前を付けます。
    2. 次の例に示すように、**compatibility** 要素と子要素をアプリケーション マニフェストに含めます。 **maxVersionTested** 要素の **Id** 属性を、ターゲットとしている Windows 10 のバージョン番号に置き換えます (これは Windows 10 バージョン 1903 以降のリリースである必要があります)。

        ```xml
        <?xml version="1.0" encoding="UTF-8"?>
        <assembly xmlns="urn:schemas-microsoft-com:asm.v1" manifestVersion="1.0">
            <compatibility xmlns="urn:schemas-microsoft-com:compatibility.v1">
                <application>
                    <!-- Windows 10 -->
                    <maxversiontested Id="10.0.18362.0"/>
                    <supportedOS Id="{8e0f7a12-bfb3-4fe8-b9a5-48fd50a15a9a}" />
                </application>
            </compatibility>
        </assembly>
        ```

## <a name="use-the-xaml-hosting-api-to-host-a-uwp-control"></a>XAML ホスティング API を使用して UWP コントロールをホストする

XAML ホスティング API を使用して UWP コントロールをホストする基本的なプロセスでは、次の一般的な手順に従います。

1. アプリで、現在のスレッド用に UWP XAML フレームワークを初期化してから、それによってホストされる [Windows.UI.Xaml.UIElement](/uwp/api/windows.ui.xaml.uielement) オブジェクトを作成します。 これを行うには、コントロールをホストする [DesktopWindowXamlSource](/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) オブジェクトをいつ作成する予定かによって、複数の方法があります。

    * アプリケーションで **DesktopWindowXamlSource** オブジェクトを作成し、その後に、そのオブジェクトによってホストされるいずれかの **Windows.UI.Xaml.UIElement** オブジェクトを作成する場合は、**DesktopWindowXamlSource** オブジェクトをインスタンス化するときに、このフレームワークが初期化されます。 このシナリオでは、フレームワークを初期化するためにご自身のコードを追加する必要はありません。

    * ただし、アプリケーションで **Windows.UI.Xaml.UIElement** オブジェクトを先に作成し、それらのオブジェクトをホストする **DesktopWindowXamlSource** オブジェクトをその後に作成する場合は、アプリケーションで静的な [WindowsXamlManager.InitializeForCurrentThread](/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager.initializeforcurrentthread) メソッドを呼び出して、**Windows.UI.Xaml.UIElement** オブジェクトがインスタンス化される前に UWP XAML フレームワークを明示的に初期化する必要があります。 アプリケーションでは一般的に、**DesktopWindowXamlSource** をホストする親 UI 要素がインスタンス化されるときに、このメソッドを呼び出す必要があります。

    > [!NOTE]
    > このメソッドからは、UWP XAML フレームワークへの参照を含む [WindowsXamlManager](/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) オブジェクトが返されます。 任意の 1 つのスレッドで **WindowsXamlManager** オブジェクトを必要な数だけ作成できます。 ただし、各オブジェクトが UWP XAML フレームワークへの参照を保持しているため、オブジェクトを破棄して、最終的に XAML リソースが確実に解放されるようにする必要があります。

2. [DesktopWindowXamlSource](/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) オブジェクトを作成し、アプリケーション内のウィンドウ ハンドルに関連付けられている親 UI 要素にアタッチします。

    これを行うには、次の手順に従う必要があります。

    1. **DesktopWindowXamlSource** オブジェクトを作成し、それを **IDesktopWindowXamlSourceNative** または **IDesktopWindowXamlSourceNative2** COM インターフェイスにキャストします。
        > [!NOTE]
        > これらのインターフェイスは、Windows SDK の **windows.ui.xaml.hosting.desktopwindowxamlsource.h** ヘッダー ファイルで宣言されています。 既定では、このファイルは %programfiles(x86)%\Windows Kits\10\Include\\<ビルド番号\>\um にあります。

    2. **IDesktopWindowXamlSourceNative** または **IDesktopWindowXamlSourceNative2** インターフェイスの **AttachToWindow** メソッドを呼び出し、アプリケーション内の親 UI 要素のウィンドウ ハンドルを渡します。

    3. **DesktopWindowXamlSource** に含まれる内部の子ウィンドウの初期サイズを設定します。 既定では、この内部の子ウィンドウは、幅と高さが 0 に設定されています。 ウィンドウのサイズを設定しない場合、**DesktopWindowXamlSource** に追加した UWP コントロールは表示されません。 **DesktopWindowXamlSource** の内部の子ウィンドウにアクセスするには、**IDesktopWindowXamlSourceNative** または **IDesktopWindowXamlSourceNative2** インターフェイスの **WindowHandle** プロパティを使用します。

3. 最後に、ホストする対象の **Windows.UI.Xaml.UIElement** を **DesktopWindowXamlSource** オブジェクトの [Content](/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content) プロパティに割り当てます。

次の手順とコード例は、上記のプロセスを実装する方法を示しています。

1. プロジェクトの **[ソース ファイル]** フォルダーで、既定の **MyDesktopWin32App** ファイルを開きます。 ファイルの内容全体を削除し、次の `include` および `using` ステートメントを追加します。 C++ および UWP の標準のヘッダーと名前空間に加えて、これらのステートメントによって XAML Islands 固有のいくつかの項目がインクルードされます。

    ```cppwinrt
    #include <windows.h>
    #include <stdlib.h>
    #include <string.h>

    #include <winrt/Windows.Foundation.Collections.h>
    #include <winrt/Windows.system.h>
    #include <winrt/windows.ui.xaml.hosting.h>
    #include <windows.ui.xaml.hosting.desktopwindowxamlsource.h>
    #include <winrt/windows.ui.xaml.controls.h>
    #include <winrt/Windows.ui.xaml.media.h>

    using namespace winrt;
    using namespace Windows::UI;
    using namespace Windows::UI::Composition;
    using namespace Windows::UI::Xaml::Hosting;
    using namespace Windows::Foundation::Numerics;
    ```

3. 前のセクションの後に次のコードをコピーします。 このコードでは、アプリの **WinMain** 関数を定義します。 この関数によって基本的なウィンドウを初期化し、XAML ホスティング API を使用して、ウィンドウ内の単純な UWP **TextBlock** コントロールをホストします。

    ```cppwinrt
    LRESULT CALLBACK WindowProc(HWND, UINT, WPARAM, LPARAM);

    HWND _hWnd;
    HWND _childhWnd;
    HINSTANCE _hInstance;

    int CALLBACK WinMain(_In_ HINSTANCE hInstance, _In_opt_ HINSTANCE hPrevInstance, _In_ LPSTR lpCmdLine, _In_ int nCmdShow)
    {
        _hInstance = hInstance;

        // The main window class name.
        const wchar_t szWindowClass[] = L"Win32DesktopApp";
        WNDCLASSEX windowClass = { };

        windowClass.cbSize = sizeof(WNDCLASSEX);
        windowClass.lpfnWndProc = WindowProc;
        windowClass.hInstance = hInstance;
        windowClass.lpszClassName = szWindowClass;
        windowClass.hbrBackground = (HBRUSH)(COLOR_WINDOW + 1);

        windowClass.hIconSm = LoadIcon(windowClass.hInstance, IDI_APPLICATION);

        if (RegisterClassEx(&windowClass) == NULL)
        {
            MessageBox(NULL, L"Windows registration failed!", L"Error", NULL);
            return 0;
        }

        _hWnd = CreateWindow(
            szWindowClass,
            L"Windows c++ Win32 Desktop App",
            WS_OVERLAPPEDWINDOW | WS_VISIBLE,
            CW_USEDEFAULT, CW_USEDEFAULT, CW_USEDEFAULT, CW_USEDEFAULT,
            NULL,
            NULL,
            hInstance,
            NULL
        );
        if (_hWnd == NULL)
        {
            MessageBox(NULL, L"Call to CreateWindow failed!", L"Error", NULL);
            return 0;
        }


        // Begin XAML Island section.

        // The call to winrt::init_apartment initializes COM; by default, in a multithreaded apartment.
        winrt::init_apartment(apartment_type::single_threaded);

        // Initialize the XAML framework's core window for the current thread.
        WindowsXamlManager winxamlmanager = WindowsXamlManager::InitializeForCurrentThread();

        // This DesktopWindowXamlSource is the object that enables a non-UWP desktop application 
        // to host UWP controls in any UI element that is associated with a window handle (HWND).
        DesktopWindowXamlSource desktopSource;

        // Get handle to the core window.
        auto interop = desktopSource.as<IDesktopWindowXamlSourceNative>();

        // Parent the DesktopWindowXamlSource object to the current window.
        check_hresult(interop->AttachToWindow(_hWnd));

        // This HWND will be the window handler for the XAML Island: A child window that contains XAML.  
        HWND hWndXamlIsland = nullptr;

        // Get the new child window's HWND. 
        interop->get_WindowHandle(&hWndXamlIsland);

        // Update the XAML Island window size because initially it is 0,0.
        SetWindowPos(hWndXamlIsland, 0, 200, 100, 800, 200, SWP_SHOWWINDOW);

        // Create the XAML content.
        Windows::UI::Xaml::Controls::StackPanel xamlContainer;
        xamlContainer.Background(Windows::UI::Xaml::Media::SolidColorBrush{ Windows::UI::Colors::LightGray() });

        Windows::UI::Xaml::Controls::TextBlock tb;
        tb.Text(L"Hello World from Xaml Islands!");
        tb.VerticalAlignment(Windows::UI::Xaml::VerticalAlignment::Center);
        tb.HorizontalAlignment(Windows::UI::Xaml::HorizontalAlignment::Center);
        tb.FontSize(48);

        xamlContainer.Children().Append(tb);
        xamlContainer.UpdateLayout();
        desktopSource.Content(xamlContainer);

        // End XAML Island section.

        ShowWindow(_hWnd, nCmdShow);
        UpdateWindow(_hWnd);

        //Message loop:
        MSG msg = { };
        while (GetMessage(&msg, NULL, 0, 0))
        {
            TranslateMessage(&msg);
            DispatchMessage(&msg);
        }

        return 0;
    }
    ```

4. 前のセクションの後に次のコードをコピーします。 このコードでは、ウィンドウの[ウィンドウ プロシージャ](/windows/win32/learnwin32/writing-the-window-procedure)を定義します。

    ```cppwinrt
    LRESULT CALLBACK WindowProc(HWND hWnd, UINT messageCode, WPARAM wParam, LPARAM lParam)
    {
        PAINTSTRUCT ps;
        HDC hdc;
        wchar_t greeting[] = L"Hello World in Win32!";
        RECT rcClient;

        switch (messageCode)
        {
            case WM_PAINT:
                if (hWnd == _hWnd)
                {
                    hdc = BeginPaint(hWnd, &ps);
                    TextOut(hdc, 300, 5, greeting, wcslen(greeting));
                    EndPaint(hWnd, &ps);
                }
                break;
            case WM_DESTROY:
                PostQuitMessage(0);
                break;

            // Create main window
            case WM_CREATE:
                _childhWnd = CreateWindowEx(0, L"ChildWClass", NULL, WS_CHILD | WS_BORDER, 0, 0, 0, 0, hWnd, NULL, _hInstance, NULL);
                return 0;

            // Main window changed size
            case WM_SIZE:
                // Get the dimensions of the main window's client
                // area, and enumerate the child windows. Pass the
                // dimensions to the child windows during enumeration.
                GetClientRect(hWnd, &rcClient);
                MoveWindow(_childhWnd, 200, 200, 400, 500, TRUE);
                ShowWindow(_childhWnd, SW_SHOW);

                return 0;

                // Process other messages.

            default:
                return DefWindowProc(hWnd, messageCode, wParam, lParam);
                break;
        }

        return 0;
    }
    ```

5. コード ファイルを保存し、アプリをビルドして実行します。 アプリ ウィンドウに UWP **TextBlock** コントロールが表示されることを確認します。
    > [!NOTE]
    > `warning C4002:  too many arguments for function-like macro invocation 'GetCurrentTime'` や `manifest authoring warning 81010002: Unrecognized Element "maxversiontested" in namespace "urn:schemas-microsoft-com:compatibility.v1"` など、いくつかのビルド警告が表示される場合があります。 これらの警告は、現在のツールと NuGet パッケージに関する既知の問題であり、無視してかまいません。

これらのタスクを示す完全な例については、次のコード ファイルを参照してください。

* **C++ Win32:**
  * [HelloWindowsDesktop.cpp](https://github.com/microsoft/Xaml-Islands-Samples/blob/master/Standalone_Samples/CppWinRT_Basic_Win32App/Win32DesktopApp/HelloWindowsDesktop.cpp) ファイルをご覧ください。
  * [XamlBridge.cpp](https://github.com/microsoft/Xaml-Islands-Samples/blob/master/Samples/Win32/SampleCppApp/XamlBridge.cpp) ファイルをご覧ください。
* **WPF:** Windows Community Toolkit の [WindowsXamlHostBase.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.cs) ファイルと [WindowsXamlHost.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHost.cs) ファイルをご覧ください。  
* **Windows フォーム:** Windows Community Toolkit の [WindowsXamlHostBase.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.cs) ファイルと [WindowsXamlHost.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHost.cs) ファイルをご覧ください。

## <a name="package-the-app"></a>アプリをパッケージ化する

必要に応じて、配置用にアプリを [MSIX パッケージ](/windows/msix)にパッケージ化することもできます。 MSIX は、Windows 向けの最新のアプリ パッケージ化テクノロジであり、MSI、.appx、App-V、ClickOnce の各インストール テクノロジの組み合わせに基づいています。

次の手順では、Visual Studio 2019 で [Windows アプリケーション パッケージ プロジェクト](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)を使用して、ソリューションのすべてのコンポーネントを MSIX パッケージにパッケージ化する方法について説明します。 これらの手順は、アプリを MSIX パッケージにパッケージ化する場合にのみ必要です。

> [!NOTE]
> 展開用にアプリケーションを [MSIX パッケージ](/windows/msix)にパッケージ化しない場合は、アプリを実行するコンピューターに [Visual C++ ランタイム](https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads)がインストールされている必要があります。

1. ソリューションに新しい [Windows アプリケーション パッケージ プロジェクト](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)を追加します。 プロジェクトを作成するときに、 **[ターゲット バージョン]** と **[最小バージョン]** の両方に対して、**Windows 10 バージョン 1903 (10.0、ビルド 18362)** を選択します。

2. パッケージ プロジェクトで、 **[アプリケーション]** ノードを右クリックして **[参照の追加]** を選択します。 プロジェクトの一覧でソリューション内の C++/Win32 デスクトップ アプリケーション プロジェクトを選択し、 **[OK]** をクリックします。

3. パッケージ プロジェクトをビルドして実行します。 アプリが実行されて UWP コントロールが想定どおりに表示されることを確認します。

## <a name="next-steps"></a>次の手順

この記事のコード例では、C++ Win32 アプリで標準の UWP コントロールをホストする基本的なシナリオについて説明しています。 以下のセクションでは、アプリケーションでサポートすることが必要になる可能性のある追加のシナリオを紹介します。

### <a name="host-a-custom-uwp-control"></a>カスタム UWP コントロールをホストする

多くのシナリオでは、連携して動作する複数のコントロールを含むカスタム UWP XAML コントロールをホストすることが必要になる場合があります。 C++ Win32 アプリでカスタム UWP コントロール (ご自身で定義したコントロール、またはサードパーティによって提供されるコントロール) をホストするプロセスは、標準コントロールをホストするよりも複雑であり、追加のコードが必要です。

詳細なチュートリアルについては、「[XAML ホスティング API を使用して C++ Win32 アプリでカスタム UWP コントロールをホストする](host-custom-control-with-xaml-islands-cpp.md)」を参照してください。

### <a name="advanced-scenarios"></a>高度なシナリオ

XAML Islands をホストする多くのデスクトップ アプリケーションでは、スムーズなユーザー エクスペリエンスを提供するために処理する必要があるシナリオが他にもあります。 たとえば、デスクトップ アプリケーションでは、XAML Islands でのキーボード入力、XAML Islands と他の UI 要素の間でのフォーカス ナビゲーション、およびレイアウトの変更を処理することが、必要になる場合があります。

これらのシナリオの処理に関する詳細、および関連するコード サンプルへのリンクについては、「[C++ Win32 アプリでの XAML Islands の高度なシナリオ](advanced-scenarios-xaml-islands-cpp.md)」を参照してください。

## <a name="related-topics"></a>関連トピック

* [デスクトップ アプリで UWP XAML コントロールをホストする (XAML Islands)](xaml-islands.md)
* [C++ Win32 アプリでの UWP XAML ホスティング API の使用](using-the-xaml-hosting-api.md)
* [C++ Win32 アプリでカスタム UWP コントロールをホストする](host-custom-control-with-xaml-islands-cpp.md)
* [C++ Win32 アプリでの XAML Islands の高度なシナリオ](advanced-scenarios-xaml-islands-cpp.md)
* [XAML Islands コード サンプル](https://github.com/microsoft/Xaml-Islands-Samples)