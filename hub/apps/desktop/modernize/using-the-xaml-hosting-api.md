---
description: この記事では、デスクトップアプリケーションで UWP XAML UI をホストする方法について説明します。
title: デスクトップアプリケーションでの UWP XAML ホスティング API の使用
ms.date: 07/26/2019
ms.topic: article
keywords: windows 10、uwp、windows フォーム、wpf、win32、xaml islands
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 14607dc3e3b32f39a840d623c7a887bb8b3687c5
ms.sourcegitcommit: f6af7aeb8506379a184207035c8e43288cb31453
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/29/2019
ms.locfileid: "68601530"
---
# <a name="using-the-uwp-xaml-hosting-api-in-a-desktop-application"></a>デスクトップアプリケーションでの UWP XAML ホスティング API の使用

Windows 10 バージョン1903以降では、XAML Islands と呼ばれる機能を使用して、 *UWP* 以外のデスクトップアプリケーションでUWPコントロールをホストできます。 この機能を使用すると、UWPコントロールを介してのみ利用可能な最新の Windows 10 UI機能を使用して、既存のデスクトップアプリケーションの外観、操作性、および機能性を向上させることができます。 つまり、既存の WPF、Windowsフォーム、およびC ++ Win32 アプリケーションで [Windows Ink](/windows/uwp/design/input/pen-and-stylus-interactions) などの UWP 機能と [Fluent Design System](/windows/uwp/design/fluent-design-system/index) をサポートするコントロールを使用できます。

API をホストしている UWP XAML では、開発者がデスクトップ アプリケーションを UWP 以外に Fluent UI に表示できるようにするコントロールの広範なセットの基盤を提供します。 この機能は呼 *XAML Islands* と呼ばれます。 この機能の概要については、[デスクトップ アプリケーションでの UWP コントロール](xaml-islands.md) を参照してください。

> [!NOTE]
> XAML Islands に関するフィードバックがある場合は、 [Microsoft.Toolkit.Win32 リポジトリ](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/issues)に新しい問題を作成し、そこにコメントを残してください。 個人的にフィードバックを送信したい場合は、XamlIslandsFeedback@microsoft.com に送信できます。 あなたの洞察とシナリオは弊社にとって非常に重要です。

## <a name="should-you-use-the-uwp-xaml-hosting-api"></a>UWP XAML ホスティング API を使用する必要がありますか。

UWP XAML ホスティング API は、デスクトップアプリケーションで UWP コントロールをホストするための低レベルのインフラストラクチャを提供します。 デスクトップアプリケーションの種類によっては、この目標を達成するために、別の便利な Api を使用するオプションがあります。  

* C++ Win32 デスクトップアプリケーションがあり、uwp コントロールをアプリケーションでホストする場合は、uwp XAML ホスティング API を使用する必要があります。 これらの種類のアプリケーションに代わる方法はありません。

* WPF および Windows フォームアプリケーションの場合は、UWP XAML ホスティング API を直接使用する代わりに、ラップされた[コントロール](xaml-islands.md#wrapped-controls)と[ホストコントロール](xaml-islands.md#host-controls)を Windows Community Toolkit で使用することを強くお勧めします。 これらのコントロールは、UWP XAML ホスティング API を内部的に使用し、UWP XAML ホスティング API を直接使用した場合 (キーボードナビゲーションやレイアウトの変更など)、独自に処理する必要があるすべての動作を実装します。 ただし、UWP XAML ホスティング API は、これらの種類のアプリケーションで直接使用できます (選択した場合)。

この記事では、Windows Community Toolkit によって提供されるコントロールではなく、UWP XAML ホスティング API をアプリケーションで直接使用する方法について説明します。

## <a name="prerequisites"></a>前提条件

UWP XAML ホスティング API には、次の前提条件があります。

* Windows 10 バージョン 1903 (またはそれ以降) および対応する Windows SDK のビルド。
* Windows ランタイム Api を使用し、次の XAML Islands を有効にするプロジェクトを構成する[手順](xaml-islands.md#requirements)

## <a name="architecture-of-xaml-islands"></a>XAML Islands のアーキテクチャ

UWP XAML ホスティング API には、次の主な Windows ランタイム型および COM インターフェイスが含まれています。

* [**Windowsxamlmanager**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager)。 このクラスは、UWP XAML フレームワークを表します。 このクラスには、デスクトップアプリケーションの現在のスレッドで UWP XAML フレームワークを初期化する単一の静的[**Initializeforcurrentthread**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager.initializeforcurrentthread)メソッドが用意されています。

* この [**DesktopWindowXamlSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) クラスは、お客様のデスクトップ アプリケーションでホストしている UWP XAML コンテンツのインスタンスを表します。 このクラスの最も重要なメンバーは、 [**コンテンツ**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content)プロパティです。 このプロパティは、ホストする[**Windows の UI. .xaml. UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)に割り当てます。 このクラスには、ルーティングのキーボード フォーカスのナビゲーション、および XAML Islands からの他のメンバーもあります。

* **Idesktopwindowxamlsourcenative**および**IDesktopWindowXamlSourceNative2** COM インターフェイス。 **IDesktopWindowXamlSourceNative**提供、 **AttachToWindow**メソッドは、親の UI 要素に、アプリケーションで XAML Islandsをアタッチするために使用します。 **IDesktopWindowXamlSourceNative2**には、UWP XAML フレームワークが特定の Windows メッセージを正しく処理できるようにする**PreTranslateMessage**メソッドが用意されています。
    > [!NOTE]
    > これらのインターフェイスは、Windows SDK 内の、 **windows の ui. .xaml. .h**ヘッダーファイルで宣言されています。 既定では、このファイルは% programfiles (x86)% \ Windows Kits\10\Include\\< ビルド番号\>\ umにあります。 C++ Win32 プロジェクトでは、このヘッダーファイルを直接参照できます。 WPF または Windows フォームプロジェクトでは、 [**Comimport**](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.comimportattribute)属性を使用して、アプリケーションコードでインターフェイスを宣言する必要があります。 インターフェイス宣言が、 **windows**の宣言と完全に一致していることを確認してください。

次の図は、デスクトップアプリケーションでホストされている XAML アイランド内のオブジェクトの階層を示しています。

![DesktopWindowXamlSource アーキテクチャ](images/xaml-islands/xaml-hosting-api-rev2.png)

* 基本レベルで XAML Islandsをホストするアプリケーションで UI 要素は、します。 この UI 要素には、ウィンドウハンドル (HWND) が必要です。 XAML Islandsをホストできる UI 要素の例として、 [ **System.Windows.Interop.HwndHost** ](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) WPF アプリケーション、 [ **System.Windows.Forms.Control**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) Windows フォーム アプリケーションの場合、[ウィンドウ](https://docs.microsoft.com/windows/desktop/winmsg/about-windows)のC++Win32 アプリケーション。

* 次のレベルには、 **Desktopwindowxamlsource**オブジェクトがあります。 このオブジェクトは、XAML Islandsをホストするためのインフラストラクチャを提供します。 コードは、このオブジェクトを作成し、親 UI 要素にアタッチする役割を担います。

* **Desktopwindowxamlsource**を作成すると、このオブジェクトによって、UWP コントロールをホストするネイティブの子ウィンドウが自動的に作成されます。 このネイティブの子ウィンドウは、ほとんどの場合、コードからは抽象化されていませんが、必要に応じてハンドル (HWND) にアクセスできます。

* 最後に、最上位レベルは、デスクトップアプリケーションでホストする UWP コントロールです。 これには、Windows SDK によって提供される UWP コントロールやカスタムユーザーコントロールなど、 [**Windows の UI**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)から派生する任意の uwp オブジェクトを指定できます。

## <a name="related-samples"></a>関連するサンプル

コードで UWP XAML ホスティング API を使用する方法は、アプリケーションの種類、アプリケーションの設計、およびその他の要因によって異なります。 この API を完全なアプリケーションのコンテキストで使用する方法を説明するために、この記事では次のサンプルのコードを示します。

### <a name="c-win32"></a>C++32

[ C++ Win32 サンプル](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App)。 このサンプルでは、パッケージ化されていないC++ Win32 アプリケーション (つまり、msix パッケージに組み込まれていないアプリケーション) で UWP ユーザーコントロールをホストする完全な実装を示します。

### <a name="wpf-and-windows-forms"></a>WPF と Windows フォーム

Windows Community Toolkit の[Windowsxamlhost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)コントロールは、WPF および Windows フォームアプリケーションで UWP ホスティング API を使用するためのリファレンスサンプルとして機能します。 ソースコードは次の場所で入手できます。

  * コントロールの WPF バージョンについては、[こちらを参照](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Wpf.UI.XamlHost)してください。 WPF バージョンは[**HwndHost**](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost)から派生します。

  * コントロールの Windows フォームバージョンについては、[こちらを参照](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Forms.UI.XamlHost)してください。 Windows フォームのバージョンは、 [**system.string から派生**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control)します。

## <a name="host-uwp-xaml-controls"></a>UWP XAML コントロールのホスト

UWP XAML ホスティング API を使用して、アプリケーションで UWP コントロールをホストするための主な手順を次に示します。

1. アプリケーションが[**Desktopwindowxamlsource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource)でホストする任意の[**Windows の UI. .xaml**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)オブジェクトを作成する前に、現在のスレッドの UWP XAML フレームワークを初期化します。

    * アプリケーションで**Desktopwindowxamlsource**オブジェクトを作成してから、そのオブジェクトを作成すると、 **desktopwindowxamlsource**オブジェクトをインスタンス化するときに、このフレームワークが初期化されます **。** . このシナリオでは、フレームワークを初期化するために独自のコードを追加する必要はありません。

    * ただし、アプリケーションがデスクトップをホストする**Desktopwindowxamlsource**オブジェクトを作成する前に、**そのオブジェクトを**作成する場合は、アプリケーションで static[**を呼び出す必要があります。** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager.initializeforcurrentthread) **WINDOWS の UI**オブジェクトがインスタンス化される前に UWP XAML フレームワークを明示的に初期化するための windowsxamlmanager。 InitializeForCurrentThread メソッド。 通常、アプリケーションは、 **Desktopwindowxamlsource**をホストする親 UI 要素がインスタンス化されるときに、このメソッドを呼び出す必要があります。

    ```cppwinrt
    Windows::UI::Xaml::Hosting::WindowsXamlManager windowsXamlManager =
        Windows::UI::Xaml::Hosting::WindowsXamlManager::InitializeForCurrentThread();
    ```

    ```csharp
    global::Windows.UI.Xaml.Hosting.WindowsXamlManager windowsXamlManager =
        global::Windows.UI.Xaml.Hosting.WindowsXamlManager.InitializeForCurrentThread();
    ```

    > [!NOTE]
    > このメソッドは、UWP XAML フレームワークへの参照を含む[**Windowsxamlmanager**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager)オブジェクトを返します。 指定したスレッドには、任意の数の**Windowsxamlmanager**オブジェクトを作成できます。 ただし、各オブジェクトが UWP XAML フレームワークへの参照を保持しているため、オブジェクトを破棄して、最終的に XAML リソースが確実に解放されるようにする必要があります。

2. [**Desktopwindowxamlsource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource)オブジェクトを作成し、ウィンドウハンドルに関連付けられているアプリケーションの親 UI 要素にアタッチします。

    これを行うには、次の手順に従う必要があります。

    1. **Desktopwindowxamlsource**オブジェクトを作成し、 **Idesktopwindowxamlsourcenative**または**IDesktopWindowXamlSourceNative2** COM インターフェイスにキャストします。

    2. **Idesktopwindowxamlsourcenative**または**IDesktopWindowXamlSourceNative2**インターフェイスの**attachtowindow**メソッドを呼び出し、アプリケーションの親 UI 要素のウィンドウハンドルを渡します。

    3. **Desktopwindowxamlsource**に含まれる内部子ウィンドウの初期サイズを設定します。 既定では、この内部子ウィンドウは、幅と高さが0に設定されています。 ウィンドウのサイズを設定しないと、 **Desktopwindowxamlsource**に追加した UWP コントロールは表示されません。 **Desktopwindowxamlsource**の内部子ウィンドウにアクセスするには、 **Idesktopwindowxamlsourcenative**または**IDesktopWindowXamlSourceNative2**インターフェイスの**WindowHandle**プロパティを使用します。 次の例では、 [SetWindowPos](https://docs.microsoft.com/windows/desktop/api/winuser/nf-winuser-setwindowpos)関数を使用してウィンドウのサイズを設定します。

    このプロセスを示すコード例を次に示します。

    ```cppwinrt
    // This example assumes you already have an HWND variable named 'parentHwnd' that
    // contains the handle of the parent window.
    Windows::UI::Xaml::Hosting::DesktopWindowXamlSource desktopWindowXamlSource;
    auto interop = desktopWindowXamlSource.as<IDesktopWindowXamlSourceNative>();
    check_hresult(interop->AttachToWindow(parentHwnd));

    HWND childInteropHwnd = nullptr;
    interop->get_WindowHandle(&childInteropHwnd);

    SetWindowPos(childInteropHwnd, 0, 0, 0, 300, 300, SWP_SHOWWINDOW);
    ```

    ```csharp
    // This WPF example assumes you already have an HwndHost named 'parentHwndHost'
    // that will act as the parent UI element for your XAML Island. It also assumes
    // you have used the DllImport attribute to import SetWindowPos from user32.dll
    // as a static method into a class named NativeMethods.
    Windows.UI.Xaml.Hosting.DesktopWindowXamlSource desktopWindowXamlSource =
        new Windows.UI.Xaml.Hosting.DesktopWindowXamlSource();

    IntPtr iUnknownPtr = System.Runtime.InteropServices.Marshal.GetIUnknownForObject(
        desktopWindowXamlSource);
    IDesktopWindowXamlSourceNative desktopWindowXamlSourceNative =
        System.Runtime.InteropServices.Marshal.Marshal.GetTypedObjectForIUnknown(
            iUnknownPtr, typeof(IDesktopWindowXamlSourceNative))
            as IDesktopWindowXamlSourceNative;

    desktopWindowXamlSourceNative.AttachToWindow(parentHwndHost.Handle);

    var childInteropHwnd = desktopWindowXamlSourceNative.WindowHandle;
    NativeMethods.SetWindowPos(childInteropHwnd, HWND_TOP, 0, 0, 300, 300, SWP_SHOWWINDOW);
    ```

3. ホストする**Windows. UI. .xaml. UIElement**を、 **Desktopwindowxamlsource**オブジェクトの[**Content**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content)プロパティに設定します。 次の例では ```myGrid``` という名前の [**Windows.UI.Xaml.Controls.Grid**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.grid) コントロールを**コンテンツ**プロパティに設定します。

   ```cppwinrt
   desktopWindowXamlSource.Content(myGrid);
   ```

   ```csharp
   desktopWindowXamlSource.Content = myGrid;
   ```

実用的なサンプルアプリケーションのコンテキストでこれらのタスクを示す完全な例については、次のコードファイルを参照してください。

  * **C++32**Win32 サンプルの[xamlbridge .cpp](https://github.com/marb2000/XamlIslands/blob/master/1903_Samples/CppWinRT_Win32_App/SampleCppApp/XamlBridge.cpp)ファイルを参照してください。 [ C++ ](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App)

  * **WPF**Windows Community Toolkit の[WindowsXamlHostBase.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.cs)ファイルと[WindowsXamlHost.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHost.cs)ファイルを参照してください。  

  * **Windows フォーム:** Windows Community Toolkit の[WindowsXamlHostBase.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.cs)ファイルと[WindowsXamlHost.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHost.cs)ファイルを参照してください。

## <a name="handle-keyboard-input-and-focus-navigation"></a>キーボード入力とフォーカスナビゲーションを処理する

アプリで XAML Islands をホストするときに、キーボードの入力とフォーカスのナビゲーションを正しく処理するために必要ないくつかの点があります。

### <a name="keyboard-input"></a>キーボード入力

各 XAML アイランドのキーボード入力を正しく処理するには、アプリケーションがすべての Windows メッセージを UWP XAML フレームワークに渡して、特定のメッセージが正しく処理されるようにする必要があります。 これを行うには、アプリケーションでメッセージループにアクセスできる場所で、各 XAML アイランドの**Desktopwindowxamlsource**オブジェクトを**IDesktopWindowXamlSourceNative2** COM インターフェイスにキャストします。 次に、このインターフェイスの**PreTranslateMessage**メソッドを呼び出し、現在のメッセージを渡します。

  * C++ Win32 アプリケーションは、メイン メッセージ ループ内で **PreTranslateMessage** を直接呼び出すことができます。 例については、 [ C++ Win32 サンプル](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App)の[xamlbridge .cpp](https://github.com/marb2000/XamlIslands/blob/master/1903_Samples/CppWinRT_Win32_App/SampleCppApp/XamlBridge.cpp#L6)ファイルを参照してください。

  * WPF アプリケーションは、 [**Componentdispatcher. ThreadFilterMessage**](https://docs.microsoft.com/dotnet/api/system.windows.interop.componentdispatcher.threadfiltermessage?view=netframework-4.7.2)イベントのイベントハンドラーから**PreTranslateMessage**を呼び出すことができます。 例については、Windows Community Toolkit の[WindowsXamlHostBase.Focus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs#L177)ファイルを参照してください。

  * Windows フォームアプリケーションは、 [**system.windows.forms.control.preprocessmessage**](https://docs.microsoft.com/en-us/dotnet/api/system.windows.forms.control.preprocessmessage?view=netframework-4.7.2)メソッドのオーバーライドから**PreTranslateMessage**を呼び出すことができます。 例については、Windows Community Toolkit の[WindowsXamlHostBase.KeyboardFocus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs#L100)ファイルを参照してください。

### <a name="keyboard-focus-navigation"></a>キーボードフォーカスのナビゲーション

ユーザーがキーボードを使用してアプリケーションの UI 要素を移動するとき (たとえば、 **tab**キーまたは direction キーを押すなど)、プログラムを使用して**Desktopwindowxamlsource**オブジェクトにフォーカスを移動する必要があります。 ユーザーのキーボードナビゲーションが**Desktopwindowxamlsource**に到達したら、ui のナビゲーション順で最初の[**Windows. ui. .xaml. UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)オブジェクトにフォーカスを移動します。引き続き次**のようにフォーカスを移動します。** ユーザーが要素を順番に移動し、 **Desktopwindowxamlsource**から親 UI 要素にフォーカスを移動するための、Windows. UI .xaml オブジェクト。  

UWP XAML ホスティング API は、これらのタスクを実行するのに役立ついくつかの型とメンバーを提供します。

* キーボードナビゲーションが**Desktopwindowxamlsource**に入ったときに、 [**GotFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.gotfocus)イベントが発生します。 このイベントを処理し、プログラムを使用して、 [**NavigateFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.navigatefocus)メソッドを使用して、最初にホストされる**Windows. UI. .xaml. UIElement**にフォーカスを移動します。

* ユーザーが**Desktopwindowxamlsource**でフォーカスを持つ最後の要素にあり、 **Tab**キーまたは方向キーを押すと、 [**TakeFocusRequested**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.takefocusrequested)イベントが発生します。 このイベントを処理し、ホストアプリケーションの次のフォーカスがある要素にプログラムでフォーカスを移動します。 たとえば、 **Desktopwindowxamlsource**が[**HwndHost**](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost)でホストされている WPF アプリケーションでは、 [**system.windows.frameworkelement.movefocus**](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.movefocus)メソッドを使用して、ホストアプリケーションの次にフォーカスがある要素にフォーカスを移すことができます。

これを実用的なサンプルアプリケーションのコンテキストで実行する方法を示す例については、次のコードファイルを参照してください。

  * **WPF**Windows Community Toolkit の[WindowsXamlHostBase.Focus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs)ファイルを参照してください。  

  * **Windows フォーム:** Windows Community Toolkit の[WindowsXamlHostBase.KeyboardFocus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs)ファイルを参照してください。

  * **C++/Win32**:Win32 サンプルの[xamlbridge.cpp](https://github.com/marb2000/XamlIslands/blob/master/1903_Samples/CppWinRT_Win32_App/SampleCppApp/XamlBridge.cpp)ファイルを参照してください。 [ C++ ](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App)

## <a name="handle-layout-changes"></a>レイアウトの変更を処理する

ユーザーが親 UI 要素のサイズを変更した場合は、必要なレイアウト変更を処理して、UWP コントロールが期待どおりに表示されるようにする必要があります。 ここでは、考慮すべき重要なシナリオをいくつか紹介します。

* C++ Win32 アプリケーションでは、アプリケーションが WM_SIZE メッセージを処理する際に、[SetWindowPos](https://docs.microsoft.com/windows/desktop/api/winuser/nf-winuser-setwindowpos) 関数を使用してホストされている XAML Island を再配置できます。 例については、[C++ Win32 サンプル](https://github.com/marb2000/XamlIslands/tree/master/19H1_Insider_Samples/CppWin32App_With_Island) の [SampleApp.cpp](https://github.com/marb2000/XamlIslands/blob/master/19H1_Insider_Samples/CppWin32App_With_Island/SampleCppApp/SampleApp.cpp#L191) コード ファイルを参照してください。

* 親 UI 要素が、 **Desktopwindowxamlsource**でホストしている、 **windows の ui. .xaml. uielement**に適合させるために必要な四角形の領域のサイズを取得する必要がある場合は、Windows. ui の[**Measure**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.measure)メソッドを呼び出します。. 次に、例を示します。

    * WPF アプリケーションでは、 **Desktopwindowxamlsource**をホストする[**HwndHost**](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost)の[**measureoverride**](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.measureoverride)メソッドからこの操作を実行できます。 例については、Windows Community Toolkit の[WindowsXamlHostBase.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs)ファイルを参照してください。

    * Windows フォームアプリケーションでは、 **Desktopwindowxamlsource**をホストする[**コントロール**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control)の[**getpreferredsize**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.getpreferredsize)メソッドからこの操作を実行できます。 例については、Windows Community Toolkit の[WindowsXamlHostBase.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs)ファイルを参照してください。

* 親 UI 要素のサイズが変更されたときに、 **Desktopwindowxamlsource**でホストしているルートの**ui**の[**配置**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.arrange)メソッドを呼び出します。 次に、例を示します。

    * WPF アプリケーションでは、 **Desktopwindowxamlsource**をホストする[**HwndHost**](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost)オブジェクトの配置[**eoverride**](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.arrangeoverride)メソッドからこの操作を行うことができます。 例については、Windows Community Toolkit の[WindowsXamlHost.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs)ファイルを参照してください。

    * Windows フォームアプリケーションでは、 **Desktopwindowxamlsource**をホストする[**コントロール**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control)の[**sizechanged**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.sizechanged)イベントのハンドラーからこの操作を実行できます。 例については、Windows Community Toolkit の[WindowsXamlHost.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs)ファイルを参照してください。

## <a name="handle-dpi-changes"></a>DPI 変更を処理する

UWP XAML フレームワークは、ホストされている UWP コントロールの DPI 変更を自動的に処理します (たとえば、画面の DPI が異なるモニター間でユーザーがウィンドウをドラッグした場合など)。 最適なエクスペリエンスを得るには、Windows フォーム、WPF、またはC++ Win32 アプリケーションをモニターごとの DPI 対応に構成することをお勧めします。

モニターごとの DPI に対応するようにアプリケーションを構成するには、 [side-by-side アセンブリマニフェスト](https://docs.microsoft.com/windows/desktop/SbsCs/application-manifests)をプロジェクトに追加し、要素```<dpiAwareness>```をに```PerMonitorV2```設定します。 この値の詳細については、 [**DPI_AWARENESS_CONTEXT_PER_MONITOR_AWARE_V2**](https://docs.microsoft.com/windows/desktop/hidpi/dpi-awareness-context)の説明を参照してください。

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

## <a name="host-custom-uwp-xaml-controls"></a>カスタム UWP XAML コントロールをホストする

次の一般的な手順に従って、XAML アイランドでカスタム UWP XAML コントロール (自分で定義するコントロールか、サードパーティによって提供されるコントロール) をホストします。 アプリケーションでコンパイルできるように、カスタムコントロールのソースコードが必要です。

1. ホストアプリケーションソリューションで、カスタム UWP XAML コントロールのソースコードを統合し、カスタムコントロールをビルドします。

2. ホストアプリケーションソリューションに UWP アプリケーションプロジェクトを追加し、このプロジェクトに " [Microsoft. Toolkit. UI. UI](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication) " パッケージを追加します。

3. UWP アプリケーションプロジェクトで、 `App`クラスを変更して、microsoft. toolkit. ui. [xamlapplication](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication) NuGet パッケージによって公開されている、 **microsoft. toolkit. ui. xamlapplication**クラスからクラスを変更します。 この型は、アプリケーションの現在のディレクトリ内のアセンブリにカスタム UWP XAML 型のメタデータを読み込むためのルートメタデータプロバイダーとして機能します。

4. `App`クラスのコンストラクターで、基本となる**Microsoft. Toolkit. Win32. UI. xamlapplication**クラスの**Initialize**メソッドを呼び出します。

5. ホストアプリケーションプロジェクトで、[前のセクション](#host-uwp-xaml-controls)で説明したプロセスに従って、XAML アイランドでカスタムコントロールをホストします。

C++ Win32 アプリケーションの例については、 [ C++ win32 サンプル](https://github.com/marb2000/XamlIslands/blob/master/1903_Samples/CppWinRT_Win32_App)の次のプロジェクトを参照してください。

* [Sampleusercontrol](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App/SampleUserControl):このプロジェクトは、という名前`MyUserControl`のカスタム UWP XAML コントロールを実装します。このコントロールには、テキストボックス、いくつかのボタン、およびコンボボックスが含まれています。
* [MyApp](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App/MyApp):これは、前の手順で説明した変更を含む UWP アプリケーションプロジェクトです。
* [SampleCppApp](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App/SampleCppApp):これは、 C++ xaml アイランドでカスタム UWP XAML コントロールをホストする Win32 アプリプロジェクトです。

WPF または Windows フォームアプリケーションの詳細な手順と例については、こちらの[手順](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost#add-a-custom-uwp-control)を参照してください。

## <a name="troubleshooting"></a>トラブルシューティング

### <a name="error-using-uwp-xaml-hosting-api-in-a-uwp-app"></a>Uwp アプリで UWP XAML ホスティング API を使用するときのエラー

| 問題 | 解決方法 |
|-------|------------|
| アプリは、次のメッセージを含む**COMException**を受け取ります。"DesktopWindowXamlSource をアクティブにできません。 この型は UWP アプリでは使用できません。 " または、"WindowsXamlManager をアクティブにできません。 この型は UWP アプリでは使用できません。 " | このエラーは、uwp アプリで UWP XAML ホスティング API (具体的には、 [**Desktopwindowxamlsource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource)または[**windowsxamlmanager**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager)の型をインスタンス化しようとしている) を使用しようとしていることを示しています。 UWP XAML ホスティング API は、WPF、Windows フォーム、 C++ Win32 アプリケーションなど、uwp 以外のデスクトップアプリでのみ使用することを目的としています。 |

### <a name="error-attaching-to-a-window-on-a-different-thread"></a>別のスレッドのウィンドウへのアタッチエラー

| 問題 | 解決方法 |
|-------|------------|
| アプリは、次のメッセージを含む**COMException**を受け取ります。"AttachToWindow メソッドは、指定された HWND が別のスレッドで作成されたため失敗しました。" | このエラーは、アプリケーションが**Idesktopwindowxamlsourcenative:: AttachToWindow**メソッドを呼び出し、別のスレッドで作成されたウィンドウの HWND に渡されたことを示します。 このメソッドは、メソッドの呼び出し元のコードと同じスレッド上で作成されたウィンドウの HWND に渡す必要があります。 |

### <a name="error-attaching-to-a-window-on-a-different-top-level-window"></a>別のトップレベルウィンドウ上のウィンドウへのアタッチエラー

| 問題 | 解決方法 |
|-------|------------|
| アプリは、次のメッセージを含む**COMException**を受け取ります。"AttachToWindow メソッドが失敗しました。指定された HWND が、以前に同じスレッドの AttachToWindow に渡された HWND とは異なるトップレベルウィンドウからのものです。" | このエラーは、アプリケーションが**Idesktopwindowxamlsourcenative:: AttachToWindow**メソッドを呼び出し、このメソッドの前の呼び出しで指定したウィンドウとは別のトップレベルウィンドウから降下したウィンドウの HWND を渡したことを示します。同じスレッドで。</p></p>アプリケーションが特定のスレッドで**Idesktopwindowxamlsourcenative:: AttachToWindow**を呼び出すと、同じスレッド上の他のすべての[**Desktopwindowxamlsource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource)オブジェクトは、同じ最上位レベルの子孫であるウィンドウにのみアタッチできます。**Idesktopwindowxamlsourcenative:: AttachToWindow**への最初の呼び出しで渡されたウィンドウ。 特定のスレッドですべての**desktopwindowxamlsource**オブジェクトが閉じられると、次の**desktopwindowxamlsource**は、任意のウィンドウに再びアタッチできます。</p></p>この問題を解決するには、このスレッドの他のトップレベルウィンドウにバインドされているすべての**desktopwindowxamlsource**オブジェクトを閉じるか、またはこの**desktopwindowxamlsource**に新しいスレッドを作成します。 |

## <a name="related-topics"></a>関連トピック

* [デスクトップアプリケーションの UWP コントロール](xaml-islands.md)
* [C++Win32 XAML Islandsサンプル](https://github.com/marb2000/XamlIslands/tree/master/19H1_Insider_Samples/CppWin32App_With_Island)
