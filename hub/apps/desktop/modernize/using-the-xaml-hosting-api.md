---
description: この記事では、デスクトップC++ Win32 アプリで UWP XAML UI をホストする方法について説明します。
title: UWP XAML を使用した C++ Win32 アプリでの API のホスト
ms.date: 08/20/2019
ms.topic: article
keywords: windows 10、uwp、windows フォーム、wpf、win32、xaml islands
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 6c1f45b4bd3da74ea150c05800eba7ec10568894
ms.sourcegitcommit: 6bb794c6e309ba543de6583d96627fbf1c177bef
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/20/2019
ms.locfileid: "69643401"
---
# <a name="using-the-uwp-xaml-hosting-api-in-a-c-win32-app"></a>UWP XAML を使用した C++ Win32 アプリでの API のホスト

Windows 10 バージョン1903以降、UWP 以外のデスクトップアプリ (Win32、WPF C++ 、Windows フォームアプリを含む) は、 *UWP XAML ホスティング API*を使用して、ウィンドウハンドル (HWND) に関連付けられている任意の UI 要素で uwp コントロールをホストできます。 この API によって、uwp 以外のデスクトップアプリは、UWP コントロール経由でのみ使用できる最新の Windows 10 UI 機能を使用できるようになります。 たとえば、UWP 以外のデスクトップアプリは、この API を使用して、 [Fluent デザインシステム](/windows/uwp/design/fluent-design-system/index)を使用し、 [Windows Ink](/windows/uwp/design/input/pen-and-stylus-interactions)をサポートする UWP コントロールをホストできます。

UWP XAML ホスティング API は、開発者が UWP デスクトップアプリに対して Fluent UI を取り込むことができるようにするために用意されている、より広範なコントロールセットの基盤を提供します。 この機能は呼 *XAML Islands* と呼ばれます。 この機能の概要については、「[デスクトップアプリでの UWP XAML コントロールのホスト (Xaml アイランド)](xaml-islands.md)」を参照してください。

> [!NOTE]
> XAML Islands に関するフィードバックがある場合は、 [Microsoft.Toolkit.Win32 リポジトリ](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/issues) に新しい問題を作成し、そこにコメントを残してください。 個人的にフィードバックを送信したい場合は、XamlIslandsFeedback@microsoft.com に送信できます。 あなたの洞察とシナリオは弊社にとって非常に重要です。

## <a name="should-you-use-the-uwp-xaml-hosting-api"></a>UWP XAML ホスティング API を使用する必要がありますか。

UWP XAML ホスティング API は、デスクトップアプリで UWP コントロールをホストするための低レベルのインフラストラクチャを提供します。 デスクトップアプリの種類によっては、別の便利な Api を使用してこの目標を達成するオプションがあります。  

* C++ Win32 デスクトップアプリがあり、uwp コントロールをアプリでホストする場合は、uwp XAML ホスティング API を使用する必要があります。 これらの種類のアプリに代わる方法はありません。

* WPF アプリと Windows フォームアプリの場合、UWP XAML ホスティング API を直接使用するのではなく、Windows Community Toolkit で[Xaml アイランド .net コントロール](xaml-islands.md#wpf-and-windows-forms-applications)を使用することを強くお勧めします。 これらのコントロールは UWP XAML ホスティング API を内部的に使用し、キーボードナビゲーションやレイアウトの変更など、UWP XAML ホスティング API を直接使用した場合に対処する必要がある動作をすべて実装しています。

この記事では、 C++ win32 アプリのみが UWP XAML ホスティング API を使用することをお勧めしますC++ 。この記事では、主に win32 アプリの手順と例について説明します。 ただし、WPF で UWP XAML ホスティング API を使用して、Windows フォームアプリを選択することもできます。 この記事では、WPF の[ホストコントロール](xaml-islands.md#host-controls)と Windows Community Toolkit の Windows フォームに関連するソースコードについて説明します。そのため、これらのコントロールで UWP XAML ホスティング API がどのように使用されているかを確認できます。

## <a name="prerequisites"></a>前提条件

XAML アイランドには、Windows 10 バージョン 1903 (以降) と、それに対応する Windows SDK のビルドが必要です。 C++ Win32 アプリで XAML アイランドを使用するには、最初にプロジェクトを設定する必要があります。

### <a name="support-for-cwinrt"></a>/WinRT C++のサポート

プロジェクトが[ C++/WinRT](/windows/uwp/cpp-and-winrt-apis)をサポートしていることを確認します。

* 新しいプロジェクトの場合は、 [ C++Visual Studio 拡張機能 (VSIX)](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264)をインストールし、その拡張機能にC++含まれているいずれかのプロジェクトテンプレートを使用することができます。
* 既存のプロジェクトの場合は、プロジェクトに[Microsoft. Windows. CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) NuGet パッケージをインストールできます。

これらのオプションの詳細については、こちらの[記事](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)を参照してください。

### <a name="configure-your-project-for-app-deployment"></a>アプリの配置用にプロジェクトを構成する

次のいずれかのオプションを選択して、プロジェクトの配置を準備します。

* **MSIX パッケージでアプリをパッケージ化**します。 [Msix パッケージ](https://docs.microsoft.com/windows/msix/)でアプリをパッケージ化すると、多くの展開および実行時の利点が得られます。
    1. Windows 10 バージョン 1903 SDK (バージョン 10.0.18362) 以降のリリースをインストールします。
    2. [Windows アプリケーションパッケージプロジェクト](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)をソリューションに追加し、 C++/Win32 プロジェクトへの参照を追加することによって、msix パッケージでアプリをパッケージ化します。

* **Microsoft. Toolkit. SDK パッケージをインストール**します。 アプリを MSIX パッケージにパッケージ化しない場合[は、6.0.0 (version](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.SDK) v preview7 またはそれ以降) をインストールすることができますが、 このパッケージには、XAML アイランドをアプリで使用できるようにする、いくつかのビルドおよび実行時アセットが用意されています。 このパッケージの最新のプレビューバージョンを確認できるように、 **[プレリリースを含める]** オプションが選択されていることを確認します。

> [!NOTE]
> 以前のバージョンの手順では、 `maxversiontested`プロジェクトのアプリケーションマニフェストに要素を追加しました。 上記のオプションのいずれかを使用している限り、この要素をマニフェストに追加する必要はありません。

### <a name="additional-requirements-for-custom-uwp-controls"></a>カスタム UWP コントロールの追加要件

カスタム UWP コントロール (たとえば、連携する複数の UWP コントロールで構成されるユーザーコントロール) をホストしている場合は、[このセクション](#host-a-custom-uwp-control)の追加の手順に従う必要があります。 また、アプリでコンパイルできるように、カスタムコントロールのソースコードも必要です。

## <a name="architecture-of-the-api"></a>API のアーキテクチャ

UWP XAML ホスティング API には、これらの主な Windows ランタイム型および COM インターフェイスが含まれています。

|  型またはインターフェイス | 説明 |
|--------------------|-------------|
| [WindowsXamlManager](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) | このクラスは、UWP XAML フレームワークを表します。 このクラスには、デスクトップアプリの現在のスレッドで UWP XAML フレームワークを初期化する単一の静的[Initializeforcurrentthread](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager.initializeforcurrentthread)メソッドが用意されています。 |
| [DesktopWindowXamlSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) | このクラスは、デスクトップアプリでホストする UWP XAML コンテンツのインスタンスを表します。 このクラスの最も重要なメンバーは、[コンテンツ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content)プロパティです。 このプロパティは、ホストする[Windows の UI. .xaml. UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)に割り当てます。 このクラスには、ルーティングのキーボード フォーカスのナビゲーション、および XAML Islands からの他のメンバーもあります。 |
| IDesktopWindowXamlSourceNative | この COM インターフェイスには**Attachtowindow**メソッドが用意されています。このメソッドは、アプリの XAML アイランドを親 UI 要素にアタッチするために使用します。 すべての**Desktopwindowxamlsource**オブジェクトは、このインターフェイスを実装します。 |
| IDesktopWindowXamlSourceNative2 | この COM インターフェイスには、UWP XAML フレームワークが特定の Windows メッセージを正しく処理できるようにする**PreTranslateMessage**メソッドが用意されています。 すべての**Desktopwindowxamlsource**オブジェクトは、このインターフェイスを実装します。 |

次の図は、デスクトップアプリでホストされている XAML アイランド内のオブジェクトの階層を示しています。

* 基本レベルは、XAML アイランドをホストするアプリ内の UI 要素です。 この UI 要素には、ウィンドウハンドル (HWND) が必要です。 XAML アイランドをホストできる UI 要素の例としては、Win32 アプリC++用の[ウィンドウ](https://docs.microsoft.com/windows/desktop/winmsg/about-windows)、WPF アプリの [System.Windows.Interop.HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost)、および Windows フォーム アプリ用の [System.Windows.Forms.Control](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) がありますが、このような場合です。

* 次のレベルには、 **Desktopwindowxamlsource**オブジェクトがあります。 このオブジェクトは、XAML Islandsをホストするためのインフラストラクチャを提供します。 コードは、このオブジェクトを作成し、親 UI 要素にアタッチする役割を担います。

* **Desktopwindowxamlsource**を作成すると、このオブジェクトによって、UWP コントロールをホストするネイティブの子ウィンドウが自動的に作成されます。 このネイティブの子ウィンドウは、ほとんどの場合、コードからは抽象化されていませんが、必要に応じてハンドル (HWND) にアクセスできます。

* 最後に、最上位レベルは、デスクトップアプリでホストする UWP コントロールです。 これには、Windows SDK によって提供される UWP コントロールやカスタムユーザーコントロールなど、 [Windows の UI](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)から派生する任意の uwp オブジェクトを指定できます。

![DesktopWindowXamlSource アーキテクチャ](images/xaml-islands/xaml-hosting-api-rev2.png)

## <a name="related-samples"></a>関連するサンプル

コードで UWP XAML ホスティング API を使用する方法は、アプリの種類、アプリの設計、およびその他の要因によって異なります。 この API を完全なアプリのコンテキストで使用する方法を説明するために、この記事では次のサンプルのコードを示します。

### <a name="c-win32"></a>C++32

次のサンプルは、 C++ Win32 アプリで UWP XAML ホスティング API を使用する方法を示しています。

* [単純な XAML アイランドサンプル](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_SimpleApp)。 このサンプルでは、パッケージ化されていないC++ Win32 アプリ (つまり、msix パッケージに組み込まれていないアプリ) で UWP コントロールをホストする基本的な実装を示します。

* [カスタムコントロールのサンプルを使用した XAML 島](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App)。 このサンプルでは、パッケージC++されていない Win32 アプリでカスタム UWP コントロールをホストする完全な実装と、キーボード入力やフォーカス移動などの他の動作を処理する方法を示します。 

### <a name="wpf-and-windows-forms"></a>WPF と Windows フォーム

Windows Community Toolkit の[Windowsxamlhost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)コントロールは、WPF および Windows フォームアプリで UWP ホスティング API を使用するためのリファレンスサンプルとして機能します。 ソースコードは次の場所で入手できます。

* コントロールの WPF バージョンについては、[こちらを参照](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Wpf.UI.XamlHost)してください。 WPF バージョンは[HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost)から派生します。

* コントロールの Windows フォームバージョンについては、[こちらを参照](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Forms.UI.XamlHost)してください。 Windows フォームのバージョンは、 [system.string から派生します](https://docs.microsoft.com/dotnet/api/system.windows.forms.control)。

## <a name="host-a-standard-uwp-control"></a>標準の UWP コントロールをホストする

このセクションでは、UWP XAML ホスティング API を使用して、新しいC++ Win32 アプリで標準の uwp コントロール (つまり、Windows SDK または WinUI ライブラリによって提供されるコントロール) をホストするプロセスについて説明します。 このコードは、[単純な XAML アイランドサンプル](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_SimpleApp)に基づいています。このセクションでは、コードの最も重要な部分について説明します。 既存C++の Win32 アプリプロジェクトがある場合は、プロジェクトのこれらの手順とコード例を調整できます。

### <a name="configure-the-project"></a>プロジェクトを構成する

1. Visual Studio 2019 で、Windows 10 version 1903 SDK (バージョン 10.0.18362) 以降のリリースがインストールされている場合は、新しい**Windows デスクトップアプリケーション**プロジェクトを作成します。 このプロジェクトは、、 **C++** **Windows**、および**デスクトップ**のプロジェクトフィルターで使用できます。

2. **ソリューションエクスプローラー**で、ソリューションノードを右クリックし、 **[ソリューション]** の再ターゲット をクリックして、 **10.0.18362.0**またはそれ以降の SDK リリースを選択し、 **[OK]** をクリックします。

3. [Microsoft. Windows. CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) NuGet パッケージをインストールします。

    1. **ソリューションエクスプローラー**でプロジェクトを右クリックし、 **[NuGet パッケージの管理]** を選択します。
    2. **[参照]** タブを選択し、「 [Microsoft. Windows. cppwinrt](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/)パッケージ」を検索して、このパッケージの最新バージョンをインストールします。

4. 次のようにして、 [Microsoft. Toolkit. UI](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.SDK) NuGet パッケージをインストールします。

    1. **[NuGet パッケージマネージャー]** ウィンドウで、 **[プレリリースを含める]** が選択されていることを確認します。
    2. **[参照]** タブを選択し、 [6.0.0 パッケージを](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.SDK)検索して、このパッケージのバージョン v preview7 (またはそれ以降) をインストールします。

### <a name="use-the-xaml-hosting-api-to-host-a-uwp-control"></a>XAML ホスティング API を使用して UWP コントロールをホストする

XAML ホスティング API を使用して UWP コントロールをホストする基本的なプロセスは、次の一般的な手順に従います。

1. アプリがホストする任意の[WINDOWS UI. .xaml](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)オブジェクトを作成する前に、現在のスレッドの UWP XAML フレームワークを初期化します。 この操作を行うには、コントロールをホストする[Desktopwindowxamlsource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource)オブジェクトを作成する時期に応じて、いくつかの方法があります。

    * アプリケーションで**Desktopwindowxamlsource**オブジェクトが作成される前に、そのオブジェクトがホストする任意の**Windows. UI. UIElement**オブジェクトを作成すると、このフレームワークは、 **DesktopWindowXamlSource**オブジェクト。 このシナリオでは、フレームワークを初期化するために独自のコードを追加する必要はありません。

    * ただし、アプリケーションがデスクトップをホストする**Desktopwindowxamlsource**オブジェクトを作成する前に、**そのオブジェクトを**作成する場合は、アプリケーションで static[を呼び出す必要があります。](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager.initializeforcurrentthread) **WINDOWS の UI**オブジェクトがインスタンス化される前に UWP XAML フレームワークを明示的に初期化するための windowsxamlmanager。 InitializeForCurrentThread メソッド。 通常、アプリケーションは、 **Desktopwindowxamlsource**をホストする親 UI 要素がインスタンス化されるときに、このメソッドを呼び出す必要があります。

    > [!NOTE]
    > このメソッドは、UWP XAML フレームワークへの参照を含む[Windowsxamlmanager](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager)オブジェクトを返します。 指定したスレッドには、任意の数の**Windowsxamlmanager**オブジェクトを作成できます。 ただし、各オブジェクトが UWP XAML フレームワークへの参照を保持しているため、オブジェクトを破棄して、最終的に XAML リソースが確実に解放されるようにする必要があります。

2. [Desktopwindowxamlsource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource)オブジェクトを作成し、ウィンドウハンドルに関連付けられているアプリケーションの親 UI 要素にアタッチします。

    これを行うには、次の手順に従う必要があります。

    1. **Desktopwindowxamlsource**オブジェクトを作成し、 **Idesktopwindowxamlsourcenative**または**IDesktopWindowXamlSourceNative2** COM インターフェイスにキャストします。
        > [!NOTE]
        > これらのインターフェイスは、Windows SDK 内の、 **windows の ui. .xaml. .h**ヘッダーファイルで宣言されています。 既定では、このファイルは% programfiles (x86)% \ Windows Kits\10\Include\\< ビルド番号\>\ umにあります。

    2. **Idesktopwindowxamlsourcenative**または**IDesktopWindowXamlSourceNative2**インターフェイスの**attachtowindow**メソッドを呼び出し、アプリケーションの親 UI 要素のウィンドウハンドルを渡します。

    3. **Desktopwindowxamlsource**に含まれる内部子ウィンドウの初期サイズを設定します。 既定では、この内部子ウィンドウは、幅と高さが0に設定されています。 ウィンドウのサイズを設定しないと、 **Desktopwindowxamlsource**に追加した UWP コントロールは表示されません。 **Desktopwindowxamlsource**の内部子ウィンドウにアクセスするには、 **Idesktopwindowxamlsourcenative**または**IDesktopWindowXamlSourceNative2**インターフェイスの**WindowHandle**プロパティを使用します。

3. 最後に、ホストする**Windows. UI. .xaml**を**Desktopwindowxamlsource**オブジェクトの[Content](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content)プロパティに割り当てます。

次の手順とコード例は、上記のプロセスを実装する方法を示しています。

1. プロジェクトの **[ソースファイル]** フォルダーで、既定の**windowsproject .cpp**ファイルを開きます。 ファイルの内容全体を削除し、次`include`のと`using`ステートメントを追加します。 これらのステートメントにC++は、標準および UWP のヘッダーと名前空間に加えて、XAML アイランドに固有のいくつかの項目が含まれています。

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

4. 前のセクションの後に次のコードをコピーします。 このコードは、ウィンドウの[ウィンドウプロシージャ](https://docs.microsoft.com/en-us/windows/win32/learnwin32/writing-the-window-procedure)を定義します。

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
    > `warning C4002:  too many arguments for function-like macro invocation 'GetCurrentTime'` や`manifest authoring warning 81010002: Unrecognized Element "maxversiontested" in namespace "urn:schemas-microsoft-com:compatibility.v1"`など、いくつかのビルド警告が表示される場合があります。 これらの警告は、現在のツールと NuGet パッケージに関する既知の問題であり、無視してかまいません。

これらのタスクを示す完全な例については、次のコードファイルを参照してください。

* **C++32**
  * [単純な XAML アイランドサンプル](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_SimpleApp)の[HelloWindowsDesktop](https://github.com/marb2000/XamlIslands/blob/master/1903_Samples/CppWinRT_Win32_SimpleApp/Win32DesktopApp/HelloWindowsDesktop.cpp)ファイルを参照してください。
  * [カスタムコントロールのサンプルと共に、XAML アイランド](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App)の[xamlbridge .cpp](https://github.com/marb2000/XamlIslands/blob/master/1903_Samples/CppWinRT_Win32_App/SampleCppApp/XamlBridge.cpp)ファイルを参照してください。

* **WPF**Windows Community Toolkit の[WindowsXamlHostBase.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.cs)ファイルと[WindowsXamlHost.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHost.cs)ファイルを参照してください。  

* **Windows フォーム:** Windows Community Toolkit の[WindowsXamlHostBase.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.cs)ファイルと[WindowsXamlHost.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHost.cs)ファイルを参照してください。

## <a name="host-a-custom-uwp-control"></a>カスタム UWP コントロールをホストする

C++ Win32 アプリでカスタム UWP コントロール (自分で定義したコントロールまたはサードパーティによって提供されるコントロール) をホストするプロセスは、[標準コントロールをホストする](#host-a-standard-uwp-control)のと同じ手順で開始されます。 ただし、カスタム UWP コントロールをホストするには、追加のプロジェクトと手順が必要です。

カスタム UWP コントロールをホストするには、次のプロジェクトとコンポーネントが必要です。

* **アプリのプロジェクトとソースコード**。 XAML アイランドをホストするための[前提条件](#prerequisites)を満たすようにプロジェクトが構成されていることを確認します。

* **カスタム UWP コントロール**。 アプリを使用してコンパイルできるように、ホストするカスタム UWP コントロールのソースコードが必要になります。 通常、カスタムコントロールは、 C++ Win32 プロジェクトと同じソリューションで参照する UWP クラスライブラリプロジェクトで定義されます。

* **XamlApplication オブジェクトを定義する UWP アプリプロジェクト**。 C++ Win32 プロジェクトは、Windows Community Toolkit によって提供`Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication`されるクラスのインスタンスにアクセスできる必要があります。 この型は、アプリケーションの現在のディレクトリ内のアセンブリにカスタム UWP XAML 型のメタデータを読み込むためのルートメタデータプロバイダーとして機能します。 これを行うには、 C++ Win32 プロジェクトと同じソリューションに**空のアプリ (ユニバーサル Windows)** プロジェクトを追加し、このプロジェクトの既定`App`のクラスを変更する方法をお勧めします。
  > [!NOTE]
  > ソリューションには、オブジェクトを`XamlApplication`定義するプロジェクトを1つだけ含めることができます。 アプリ内のすべてのカスタム UWP コントロールは、 `XamlApplication`同じオブジェクトを共有します。 `XamlApplication`オブジェクトを定義するプロジェクトには、XAML アイランドでホスト uwp コントロールを使用する他のすべての uwp ライブラリおよびプロジェクトへの参照が含まれている必要があります。

C++ Win32 アプリでカスタム UWP コントロールをホストするには、次の一般的な手順に従います。

1. Win32 デスクトップアプリプロジェクトが含まれているソリューションで、**空のアプリ (ユニバーサル Windows)** プロジェクトを追加し、関連する WPF チュートリアルの[このセクション](host-custom-control-with-xaml-islands.md#create-a-xamlapplication-object-in-a-uwp-app-project)の詳細な手順に従って構成します。 C++

2. 同じソリューションで、カスタム UWP XAML コントロールのソースコードを含むプロジェクトを追加します (これは通常、UWP クラスライブラリプロジェクトです)。プロジェクトをビルドします。

3. UWP アプリプロジェクトで、UWP クラスライブラリプロジェクトへの参照を追加します。

4. C++ Win32 プロジェクトで、uwp アプリプロジェクトへの参照と、ソリューション内の uwp クラスライブラリプロジェクトへの参照を追加します。

5. 「 [Xaml ホスティング API を使用して UWP コントロールをホストする](#use-the-xaml-hosting-api-to-host-a-uwp-control)」セクションで説明されている手順に従って、アプリの xaml アイランドでカスタムコントロールをホストします。 ホストするカスタムコントロールのインスタンスを、コード内の**Desktopwindowxamlsource**オブジェクトの[Content](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content)プロパティに割り当てます。

C++ Win32 アプリケーションの完全な例については、「[カスタムコントロールのサンプルを使用した XAML アイランド](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App)の次のプロジェクト」を参照してください。

* [Sampleusercontrol](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App/SampleUserControl):このプロジェクトは、という名前`MyUserControl`のカスタム UWP XAML コントロールを実装します。このコントロールには、テキストボックス、いくつかのボタン、およびコンボボックスが含まれています。
* [MyApp](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App/MyApp):これは、上記で説明した変更を含む UWP アプリプロジェクトです。
* [SampleCppApp](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App/SampleCppApp):これは、 C++ xaml アイランドでカスタム UWP XAML コントロールをホストする Win32 アプリプロジェクトです。

## <a name="handle-keyboard-layout-and-dpi"></a>キーボード、レイアウト、DPI を処理する

以下のセクションでは、キーボードとレイアウトのシナリオを処理する方法に関するガイダンスとコード例へのリンクを提供します。

* [キーボード入力](#keyboard-input)
* [キーボードフォーカスのナビゲーション](#keyboard-focus-navigation)
* [レイアウトの変更を処理する](#handle-layout-changes)
* [DPI 変更を処理する](#handle-dpi-changes)

### <a name="keyboard-input"></a>キーボード入力

各 XAML アイランドのキーボード入力を正しく処理するには、アプリケーションがすべての Windows メッセージを UWP XAML フレームワークに渡して、特定のメッセージが正しく処理されるようにする必要があります。 これを行うには、アプリケーションでメッセージループにアクセスできる場所で、各 XAML アイランドの**Desktopwindowxamlsource**オブジェクトを**IDesktopWindowXamlSourceNative2** COM インターフェイスにキャストします。 次に、このインターフェイスの**PreTranslateMessage**メソッドを呼び出し、現在のメッセージを渡します。

  * **C++ Win32:** :アプリは、メインメッセージループで**PreTranslateMessage**を直接呼び出すことができます。 例については、 [ C++ Win32 サンプル](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App)の[xamlbridge .cpp](https://github.com/marb2000/XamlIslands/blob/master/1903_Samples/CppWinRT_Win32_App/SampleCppApp/XamlBridge.cpp#L6)ファイルを参照してください。

  * **WPF**アプリは、 [Componentdispatcher. ThreadFilterMessage](https://docs.microsoft.com/dotnet/api/system.windows.interop.componentdispatcher.threadfiltermessage?view=netframework-4.7.2)イベントのイベントハンドラーから**PreTranslateMessage**を呼び出すことができます。 例については、Windows Community Toolkit の[WindowsXamlHostBase.Focus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs#L177)ファイルを参照してください。

  * **Windows フォーム:** アプリは[system.windows.forms.control.preprocessmessage](https://docs.microsoft.com/en-us/dotnet/api/system.windows.forms.control.preprocessmessage?view=netframework-4.7.2)メソッドのオーバーライドから**PreTranslateMessage**を呼び出すことができます。 例については、Windows Community Toolkit の[WindowsXamlHostBase.KeyboardFocus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs#L100)ファイルを参照してください。

### <a name="keyboard-focus-navigation"></a>キーボードフォーカスのナビゲーション

ユーザーがキーボードを使用してアプリケーションの UI 要素を移動するとき (たとえば、 **tab**キーまたは direction キーを押すなど)、プログラムを使用して**Desktopwindowxamlsource**オブジェクトにフォーカスを移動する必要があります。 ユーザーのキーボードナビゲーションが**Desktopwindowxamlsource**に到達したら、ui のナビゲーション順で最初の[Windows. ui. .xaml. UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)オブジェクトにフォーカスを移動します。引き続き次**のようにフォーカスを移動します。** ユーザーが要素を順番に移動し、 **Desktopwindowxamlsource**から親 UI 要素にフォーカスを移動するための、Windows. UI .xaml オブジェクト。  

UWP XAML ホスティング API は、これらのタスクを実行するのに役立ついくつかの型とメンバーを提供します。

* キーボードナビゲーションが**Desktopwindowxamlsource**に入ったときに、 [GotFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.gotfocus)イベントが発生します。 このイベントを処理し、プログラムを使用して、 [NavigateFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.navigatefocus)メソッドを使用して、最初にホストされる**Windows. UI. .xaml. UIElement**にフォーカスを移動します。

* ユーザーが**Desktopwindowxamlsource**でフォーカスを持つ最後の要素にあり、 **Tab**キーまたは方向キーを押すと、 [TakeFocusRequested](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.takefocusrequested)イベントが発生します。 このイベントを処理し、ホストアプリケーションの次のフォーカスがある要素にプログラムでフォーカスを移動します。 たとえば、 **Desktopwindowxamlsource**が[HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost)でホストされている WPF アプリケーションでは、 [system.windows.frameworkelement.movefocus](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.movefocus)メソッドを使用して、ホストアプリケーションの次にフォーカスがある要素にフォーカスを移すことができます。

これを実用的なサンプルアプリケーションのコンテキストで実行する方法を示す例については、次のコードファイルを参照してください。

  * **C++/Win32**:Win32 サンプルの[xamlbridge.cpp](https://github.com/marb2000/XamlIslands/blob/master/1903_Samples/CppWinRT_Win32_App/SampleCppApp/XamlBridge.cpp)ファイルを参照してください。 [ C++ ](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App)

  * **WPF**Windows Community Toolkit の[WindowsXamlHostBase.Focus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs)ファイルを参照してください。  

  * **Windows フォーム:** Windows Community Toolkit の[WindowsXamlHostBase.KeyboardFocus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs)ファイルを参照してください。

### <a name="handle-layout-changes"></a>レイアウトの変更を処理する

ユーザーが親 UI 要素のサイズを変更した場合は、必要なレイアウト変更を処理して、UWP コントロールが期待どおりに表示されるようにする必要があります。 ここでは、考慮すべき重要なシナリオをいくつか紹介します。

* C++ Win32 アプリケーションでは、アプリケーションが WM_SIZE メッセージを処理する際に、[SetWindowPos](https://docs.microsoft.com/windows/desktop/api/winuser/nf-winuser-setwindowpos) 関数を使用してホストされている XAML Island を再配置できます。 例については、[C++ Win32 サンプル](https://github.com/marb2000/XamlIslands/tree/master/19H1_Insider_Samples/CppWin32App_With_Island) の [SampleApp.cpp](https://github.com/marb2000/XamlIslands/blob/master/19H1_Insider_Samples/CppWin32App_With_Island/SampleCppApp/SampleApp.cpp#L191) コード ファイルを参照してください。

* 親 UI 要素が、 **Desktopwindowxamlsource**でホストしている、 **windows の ui. .xaml. uielement**に適合させるために必要な四角形の領域のサイズを取得する必要がある場合は、Windows. ui の[Measure](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.measure)メソッドを呼び出します。 以下に例を示します。

    * WPF アプリケーションでは、 **Desktopwindowxamlsource**をホストする[HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost)の[measureoverride](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.measureoverride)メソッドからこの操作を実行できます。 例については、Windows Community Toolkit の[WindowsXamlHostBase.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs)ファイルを参照してください。

    * Windows フォームアプリケーションでは、 **Desktopwindowxamlsource**をホストする[コントロール](https://docs.microsoft.com/dotnet/api/system.windows.forms.control)の[getpreferredsize](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.getpreferredsize)メソッドからこの操作を実行できます。 例については、Windows Community Toolkit の[WindowsXamlHostBase.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs)ファイルを参照してください。

* 親 UI 要素のサイズが変更されたときに、 **Desktopwindowxamlsource**でホストしているルートの**ui**の[配置](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.arrange)メソッドを呼び出します。 以下に例を示します。

    * WPF アプリケーションでは、 **Desktopwindowxamlsource**をホストする[HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost)オブジェクトの配置[eoverride](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.arrangeoverride)メソッドからこの操作を行うことができます。 例については、Windows Community Toolkit の[WindowsXamlHost.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs)ファイルを参照してください。

    * Windows フォームアプリケーションでは、 **Desktopwindowxamlsource**をホストする[コントロール](https://docs.microsoft.com/dotnet/api/system.windows.forms.control)の[sizechanged](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.sizechanged)イベントのハンドラーからこの操作を実行できます。 例については、Windows Community Toolkit の[WindowsXamlHost.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs)ファイルを参照してください。

### <a name="handle-dpi-changes"></a>DPI 変更を処理する

UWP XAML フレームワークは、ホストされている UWP コントロールの DPI 変更を自動的に処理します (たとえば、画面の DPI が異なるモニター間でユーザーがウィンドウをドラッグした場合など)。 最適なエクスペリエンスを得るには、Windows フォーム、WPF、またはC++ Win32 アプリケーションをモニターごとの DPI 対応に構成することをお勧めします。

モニターごとの DPI に対応するようにアプリケーションを構成するには、 [side-by-side アセンブリマニフェスト](https://docs.microsoft.com/windows/desktop/SbsCs/application-manifests)をプロジェクトに追加し、  **\<\> dpiawareness**要素を**PerMonitorV2**に設定します。 この値の詳細については、 [DPI_AWARENESS_CONTEXT_PER_MONITOR_AWARE_V2](https://docs.microsoft.com/windows/desktop/hidpi/dpi-awareness-context)の説明を参照してください。

```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<assembly xmlns="urn:schemas-microsoft-com:asm.v1" manifestVersion="1.0">
    <application xmlns="urn:schemas-microsoft-com:asm.v3">
        <windowsSettings>
            <dpiAwareness xmlns="http://schemas.microsoft.com/SMI/2016/WindowsSettings">PerMonitorV2</dpiAwareness>
        </windowsSettings>
    </application>
</assembly>
```

## <a name="troubleshooting"></a>トラブルシューティング

### <a name="error-using-uwp-xaml-hosting-api-in-a-uwp-app"></a>Uwp アプリで UWP XAML ホスティング API を使用するときのエラー

| 問題 | 解決方法 |
|-------|------------|
| アプリは、次のメッセージを含む**COMException**を受け取ります。"DesktopWindowXamlSource をアクティブにできません。 この型は UWP アプリでは使用できません。 " または、"WindowsXamlManager をアクティブにできません。 この型は UWP アプリでは使用できません。 " | このエラーは、uwp アプリで UWP XAML ホスティング API (具体的には、 [Desktopwindowxamlsource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource)または[windowsxamlmanager](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager)の型をインスタンス化しようとしている) を使用しようとしていることを示しています。 UWP XAML ホスティング API は、WPF、Windows フォーム、 C++ Win32 アプリケーションなど、uwp 以外のデスクトップアプリでのみ使用することを目的としています。 |

### <a name="error-trying-to-use-the-windowsxamlmanager-or-desktopwindowxamlsource-types"></a>WindowsXamlManager または DesktopWindowXamlSource 型を使用しようとしたときにエラーが発生しました

| 問題 | 解決方法 |
|-------|------------|
| アプリは次のメッセージで例外を受け取ります。"WindowsXamlManager と DesktopWindowXamlSource は、Windows バージョン10.0.18226.0 以降を対象とするアプリでサポートされています。 アプリケーションマニフェストまたはパッケージマニフェストを確認し、MaxTestedVersion プロパティが更新されていることを確認してください。 " | このエラーは、アプリケーションが UWP XAML ホスティング API で**Windowsxamlmanager**または**Desktopwindowxamlsource**型を使用しようとしましたが、OS は、アプリが Windows 10 バージョン1903以降を対象とするようにビルドされているかどうかを判断できないことを示しています。 UWP XAML ホスティング API は、以前のバージョンの Windows 10 では最初にプレビューとして導入されましたが、Windows 10 バージョン1903以降でのみサポートされています。</p></p>この問題を解決するには、アプリ用の MSIX パッケージを作成してパッケージから実行するか、プロジェクトに[Microsoft. Toolkit. UI. SDK](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.SDK) NuGet パッケージをインストールします。 詳しくは、[このセクション](#configure-your-project-for-app-deployment)をご覧ください。 |

### <a name="error-attaching-to-a-window-on-a-different-thread"></a>別のスレッドのウィンドウへのアタッチエラー

| 問題 | 解決方法 |
|-------|------------|
| アプリは、次のメッセージを含む**COMException**を受け取ります。"AttachToWindow メソッドは、指定された HWND が別のスレッドで作成されたため失敗しました。" | このエラーは、アプリケーションが**Idesktopwindowxamlsourcenative:: AttachToWindow**メソッドを呼び出し、別のスレッドで作成されたウィンドウの HWND に渡されたことを示します。 このメソッドは、メソッドの呼び出し元のコードと同じスレッド上で作成されたウィンドウの HWND に渡す必要があります。 |

### <a name="error-attaching-to-a-window-on-a-different-top-level-window"></a>別のトップレベルウィンドウ上のウィンドウへのアタッチエラー

| 問題 | 解決方法 |
|-------|------------|
| アプリは、次のメッセージを含む**COMException**を受け取ります。"AttachToWindow メソッドが失敗しました。指定された HWND が、以前に同じスレッドの AttachToWindow に渡された HWND とは異なるトップレベルウィンドウからのものです。" | このエラーは、アプリケーションが**Idesktopwindowxamlsourcenative:: AttachToWindow**メソッドを呼び出し、このメソッドの前の呼び出しで指定したウィンドウとは別のトップレベルウィンドウから降下したウィンドウの HWND を渡したことを示します。同じスレッドで。</p></p>アプリケーションが特定のスレッドで**Attachtowindow**を呼び出すと、同じスレッド上の他のすべての[Desktopwindowxamlsource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource)オブジェクトは、の最初の呼び出しで渡された同じトップレベルウィンドウの子孫であるウィンドウにのみアタッチできます。**Attachtowindow**。 特定のスレッドですべての**desktopwindowxamlsource**オブジェクトが閉じられると、次の**desktopwindowxamlsource**は、任意のウィンドウに再びアタッチできます。</p></p>この問題を解決するには、このスレッドの他のトップレベルウィンドウにバインドされているすべての**desktopwindowxamlsource**オブジェクトを閉じるか、またはこの**desktopwindowxamlsource**に新しいスレッドを作成します。 |

## <a name="related-topics"></a>関連トピック

* [デスクトップアプリケーションの UWP コントロール](xaml-islands.md)
* [C++Win32 XAML Islandsサンプル](https://github.com/marb2000/XamlIslands/tree/master/19H1_Insider_Samples/CppWin32App_With_Island)
