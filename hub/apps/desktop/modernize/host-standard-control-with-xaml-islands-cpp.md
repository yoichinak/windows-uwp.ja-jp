---
description: この記事では、XAML ホスティング API を使用してC++ 、Win32 アプリで標準の UWP コントロールをホストする方法について説明します。
title: XAML アイランドを使用してC++ Win32 アプリで標準の UWP コントロールをホストする
ms.date: 03/23/2020
ms.topic: article
keywords: windows 10、uwp、cpp、win32、xaml アイランド、ラップされたコントロール、標準コントロール
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 08308c7bca3cd7f39b08c836e43d791a3fda048f
ms.sourcegitcommit: c660def841abc742600fbcf6ed98e1f4f7beb8cc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/24/2020
ms.locfileid: "80226276"
---
# <a name="host-a-standard-uwp-control-in-a-c-win32-app"></a>C++ Win32 アプリで標準の UWP コントロールをホストする

この記事では、 [UWP XAML ホスティング API](using-the-xaml-hosting-api.md)を使用して、新しいC++ Win32 アプリで標準の uwp コントロール (Windows SDK によってプロビジョニングされたコントロール) をホストする方法について説明します。 このコードは、[単純な XAML アイランドサンプル](https://github.com/microsoft/Xaml-Islands-Samples/tree/master/Standalone_Samples/CppWinRT_Basic_Win32App)に基づいています。このセクションでは、コードの最も重要な部分について説明します。 既存C++の Win32 アプリプロジェクトがある場合は、プロジェクトのこれらの手順とコード例を調整できます。

> [!NOTE]
> この記事で説明するシナリオは、アプリでホストされている UWP コントロールの XAML マークアップを直接編集することをサポートしていません。 このシナリオでは、コードを使用してホストされている UWP コントロールの外観と動作を変更することを制限します。 UWP コントロールをホストするときに XAML マークアップを直接編集できるようにする手順については、「 [ C++ Win32 アプリでのカスタム UWP コントロールのホスト](host-custom-control-with-xaml-islands-cpp.md)」を参照してください。

## <a name="create-a-desktop-application-project"></a>デスクトップアプリケーションプロジェクトを作成する

1. Windows 10 version 1903 SDK (バージョン 10.0.18362) またはそれ以降のリリースがインストールされた Visual Studio 2019 では、新しい**Windows デスクトップアプリケーション**プロジェクトを作成し、 **MyDesktopWin32App**という名前を付けます。 このプロジェクトの種類は、、 **C++** **Windows**、および**デスクトップ**のプロジェクトフィルターで使用できます。

2. **ソリューションエクスプローラー**で、ソリューションノードを右クリックし、 **[ソリューション]** の再ターゲット をクリックして、 **10.0.18362.0**またはそれ以降の SDK リリースを選択し、 **[OK]** をクリックします。

3. [Microsoft. Windows. cppwinrt](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) NuGet パッケージをインストールして、プロジェクトで[ C++/WinRT](/windows/uwp/cpp-and-winrt-apis)のサポートを有効にします。

    1. **ソリューションエクスプローラー**でプロジェクトを右クリックし、 **[NuGet パッケージの管理]** を選択します。
    2. **[参照]** タブを選択し、「 [Microsoft. Windows. cppwinrt](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/)パッケージ」を検索して、このパッケージの最新バージョンをインストールします。

    > [!NOTE]
    > 新しいプロジェクトの場合は、別の方法として、 [ C++Visual Studio 拡張機能 (VSIX)](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264)をインストールC++し、その拡張機能に含まれているいずれかのプロジェクトテンプレートを使用することもできます。 詳細については、[こちらの記事](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)を参照してください。

4. 次のようにして、 [Microsoft. Toolkit. UI](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.SDK) NuGet パッケージをインストールします。

    1. **[NuGet パッケージマネージャー]** ウィンドウで、 **[プレリリースを含める]** が選択されていることを確認します。
    2. **[参照]** タブを選択し、 **6.0.0 パッケージを**検索して、このパッケージのバージョン v (またはそれ以降) をインストールします。 このパッケージには、XAML Islands をアプリで使用できるようにする、いくつかのビルドおよび実行時アセットが用意されています。

5. アプリケーション[マニフェスト](https://docs.microsoft.com/windows/desktop/SbsCs/application-manifests)で `maxVersionTested` 値を設定して、アプリケーションが Windows 10 バージョン1903以降と互換性があることを指定します。

    1. プロジェクトにアプリケーションマニフェストがまだない場合は、新しい XML ファイルをプロジェクトに追加し、「 **app.xaml**」という名前を指定します。
    2. アプリケーションマニフェストに、次の例に示すように、**互換性**要素と子要素を含めます。 **Maxversiontested**された要素の**Id**属性を、ターゲットとしている windows 10 のバージョン番号に置き換えます (これは windows 10 バージョン1903以降のリリースである必要があります)。

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

XAML ホスティング API を使用して UWP コントロールをホストする基本的なプロセスは、次の一般的な手順に従います。

1. アプリがホストする任意の[WINDOWS UI. .xaml](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)オブジェクトを作成する前に、現在のスレッドの UWP XAML フレームワークを初期化します。 この操作を行うには、コントロールをホストする[Desktopwindowxamlsource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource)オブジェクトを作成する時期に応じて、いくつかの方法があります。

    * アプリケーションで**Desktopwindowxamlsource**オブジェクトを作成する前に、そのオブジェクトがホストする任意の**Windows. UI. UIElement**オブジェクトを作成すると、 **desktopwindowxamlsource**オブジェクトをインスタンス化するときに、このフレームワークが初期化されます。 このシナリオでは、フレームワークを初期化するために独自のコードを追加する必要はありません。

    * ただし、アプリケーション**で、これら**をホストする**Desktopwindowxamlsource**オブジェクトを作成する前にアプリケーションで作成した場合、アプリケーションでは、スタティック[Windowsxamlmanager. initializeforcurrentthread](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager.initializeforcurrentthread)メソッドを呼び出して、UWP Xaml フレームワークを明示的に初期化してから、 **windows の ui**オブジェクトがインスタンス化されるようにする必要があります。 通常、アプリケーションは、 **Desktopwindowxamlsource**をホストする親 UI 要素がインスタンス化されるときに、このメソッドを呼び出す必要があります。

    > [!NOTE]
    > このメソッドは、UWP XAML フレームワークへの参照を含む[Windowsxamlmanager](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager)オブジェクトを返します。 指定したスレッドには、任意の数の**Windowsxamlmanager**オブジェクトを作成できます。 ただし、各オブジェクトが UWP XAML フレームワークへの参照を保持しているため、オブジェクトを破棄して、最終的に XAML リソースが確実に解放されるようにする必要があります。

2. [Desktopwindowxamlsource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource)オブジェクトを作成し、ウィンドウハンドルに関連付けられているアプリケーションの親 UI 要素にアタッチします。

    これを行うには、次の手順に従う必要があります。

    1. **Desktopwindowxamlsource**オブジェクトを作成し、 **Idesktopwindowxamlsourcenative**または**IDesktopWindowXamlSourceNative2** COM インターフェイスにキャストします。
        > [!NOTE]
        > これらのインターフェイスは、Windows SDK 内の、 **windows の ui. .xaml. .h**ヘッダーファイルで宣言されています。 既定では、このファイルは% programfiles (x86)% \ Windows Kits\10\Include\\< ビルド番号\>\ um.

    2. **Idesktopwindowxamlsourcenative**または**IDesktopWindowXamlSourceNative2**インターフェイスの**attachtowindow**メソッドを呼び出し、アプリケーションの親 UI 要素のウィンドウハンドルを渡します。

    3. **Desktopwindowxamlsource**に含まれる内部子ウィンドウの初期サイズを設定します。 既定では、この内部子ウィンドウは、幅と高さが0に設定されています。 ウィンドウのサイズを設定しないと、 **Desktopwindowxamlsource**に追加した UWP コントロールは表示されません。 **Desktopwindowxamlsource**の内部子ウィンドウにアクセスするには、 **Idesktopwindowxamlsourcenative**または**IDesktopWindowXamlSourceNative2**インターフェイスの**WindowHandle**プロパティを使用します。

3. 最後に、ホストする**Windows. UI. .xaml**を**Desktopwindowxamlsource**オブジェクトの[Content](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content)プロパティに割り当てます。

次の手順とコード例は、上記のプロセスを実装する方法を示しています。

1. プロジェクトの **[ソースファイル]** フォルダーで、既定の**MyDesktopWin32App**ファイルを開きます。 ファイルの内容全体を削除し、次の `include` と `using` ステートメントを追加します。 これらのステートメントにC++は、標準および UWP のヘッダーと名前空間に加えて、XAML アイランドに固有のいくつかの項目が含まれています。

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

3. 前のセクションの後に次のコードをコピーします。 このコードは、アプリの**WinMain**関数を定義します。 この関数は、基本的なウィンドウを初期化し、XAML ホスティング API を使用してウィンドウ内の単純な UWP **TextBlock**コントロールをホストします。

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

4. 前のセクションの後に次のコードをコピーします。 このコードは、ウィンドウの[ウィンドウプロシージャ](https://docs.microsoft.com/windows/win32/learnwin32/writing-the-window-procedure)を定義します。

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

5. コードファイルを保存し、アプリをビルドして実行します。 アプリウィンドウに UWP **TextBlock**コントロールが表示されていることを確認します。
    > [!NOTE]
    > `warning C4002:  too many arguments for function-like macro invocation 'GetCurrentTime'` や `manifest authoring warning 81010002: Unrecognized Element "maxversiontested" in namespace "urn:schemas-microsoft-com:compatibility.v1"`など、いくつかのビルド警告が表示される場合があります。 これらの警告は、現在のツールと NuGet パッケージに関する既知の問題であり、無視してかまいません。

これらのタスクを示す完全な例については、次のコードファイルを参照してください。

* **C++32**
  * [HelloWindowsDesktop](https://github.com/microsoft/Xaml-Islands-Samples/blob/master/Standalone_Samples/CppWinRT_Basic_Win32App/Win32DesktopApp/HelloWindowsDesktop.cpp)ファイルを参照してください。
  * [Xamlbridge .cpp](https://github.com/microsoft/Xaml-Islands-Samples/blob/master/Samples/Win32/SampleCppApp/XamlBridge.cpp)ファイルを参照してください。
* **WPF:** Windows Community Toolkit の[WindowsXamlHostBase.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.cs)ファイルと[WindowsXamlHost.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHost.cs)ファイルを参照してください。  
* **Windows フォーム:** Windows Community Toolkit の[WindowsXamlHostBase.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.cs)ファイルと[WindowsXamlHost.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHost.cs)ファイルを参照してください。

## <a name="package-the-app"></a>アプリのパッケージ化

必要に応じて、アプリを[Msix パッケージ](https://docs.microsoft.com/windows/msix)にパッケージ化してデプロイすることもできます。 MSIX は Windows 向けの最新のアプリケーションパッケージ化テクノロジであり、MSI、.appx、App-v、ClickOnce のインストールテクノロジの組み合わせに基づいています。

次の手順では、Visual Studio 2019 の[Windows アプリケーションパッケージプロジェクト](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)を使用して、msix パッケージのソリューションに含まれるすべてのコンポーネントをパッケージ化する方法を示します。 これらの手順は、アプリを MSIX パッケージでパッケージ化する場合にのみ必要です。

> [!NOTE]
> アプリケーションを展開用の[Msix パッケージ](https://docs.microsoft.com/windows/msix)にパッケージ化しない場合は、アプリを実行するコンピューターに[ C++ Visual Runtime](https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads)がインストールされている必要があります。

1. 新しい[Windows アプリケーションパッケージプロジェクト](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)をソリューションに追加します。 プロジェクトを作成するときに、 **Windows 10 バージョン 1903 (10.0;ビルド 18362)** 。**ターゲットバージョン**と**最小バージョン**の両方に対応します。

2. パッケージプロジェクトで、 **[アプリケーション]** ノードを右クリックし、 **[参照の追加]** を選択します。 プロジェクトの一覧で、ソリューション内のC++[/Win32 デスクトップアプリケーション] プロジェクトを選択し、[ **OK]** をクリックします。

3. パッケージプロジェクトをビルドして実行します。 アプリが実行されていることを確認し、UWP コントロールが想定どおりに表示されることを確認します。

## <a name="next-steps"></a>次のステップ:

この記事のコード例では、 C++ Win32 アプリで標準の UWP コントロールをホストする基本的なシナリオについて説明します。 次のセクションでは、アプリケーションでのサポートが必要になる可能性のある追加のシナリオを紹介します。

### <a name="host-a-custom-uwp-control"></a>カスタム UWP コントロールをホストする

多くのシナリオでは、連携して動作する複数のコントロールを含むカスタム UWP XAML コントロールをホストする必要がある場合があります。 C++ Win32 アプリでカスタム UWP コントロール (自分で定義するコントロールまたはサードパーティによって提供されるコントロール) をホストするプロセスは、標準コントロールをホストするよりも複雑であり、追加のコードが必要です。

完全なチュートリアルについては、「 [XAML ホスティング API をC++使用した Win32 アプリでのカスタム UWP コントロールのホスト](host-custom-control-with-xaml-islands-cpp.md)」を参照してください。

### <a name="advanced-scenarios"></a>高度なシナリオ

XAML アイランドをホストする多くのデスクトップアプリケーションでは、ユーザーエクスペリエンスをスムーズにするために、追加のシナリオを処理する必要があります。 たとえば、デスクトップアプリケーションでは、XAML アイランドでのキーボード入力の処理、XAML アイランドと他の UI 要素間のフォーカスナビゲーション、およびレイアウトの変更が必要になる場合があります。

これらのシナリオおよび関連するコードサンプルへのポインターの処理の詳細については、「 [Win32 アプリでC++の XAML アイランドの高度なシナリオ](advanced-scenarios-xaml-islands-cpp.md)」を参照してください。

## <a name="related-topics"></a>関連トピック

* [デスクトップアプリでの UWP XAML コントロールのホスト (XAML アイランド)](xaml-islands.md)
* [C++ Win32 アプリで UWP XAML ホスティング API を使用する](using-the-xaml-hosting-api.md)
* [C++ Win32 アプリでカスタム UWP コントロールをホストする](host-custom-control-with-xaml-islands-cpp.md)
* [Win32 アプリでC++の XAML アイランドの高度なシナリオ](advanced-scenarios-xaml-islands-cpp.md)
* [XAML アイランドコードサンプル](https://github.com/microsoft/Xaml-Islands-Samples)
