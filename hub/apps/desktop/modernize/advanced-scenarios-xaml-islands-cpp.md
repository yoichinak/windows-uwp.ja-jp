---
description: この記事では、Win32 アプリの高度C++な XAML アイランドホスティングシナリオについて説明します。
title: Win32 アプリでC++の XAML アイランドの高度なシナリオ
ms.date: 03/23/2020
ms.topic: article
keywords: windows 10、uwp、cpp、win32、xaml アイランド、ラップされたコントロール、標準コントロール
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 50ee005fc0de52a3e0217a71fb3d391445c486db
ms.sourcegitcommit: c660def841abc742600fbcf6ed98e1f4f7beb8cc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/24/2020
ms.locfileid: "80226236"
---
# <a name="advanced-scenarios-for-xaml-islands-in-c-win32-apps"></a>Win32 アプリでC++の XAML アイランドの高度なシナリオ

「[ホスト a 標準 uwp コントロール](host-standard-control-with-xaml-islands-cpp.md)」と「[カスタム uwp コントロールのホスト](host-custom-control-with-xaml-islands-cpp.md)」の記事では、 C++ Win32 アプリで XAML アイランドをホストするための手順と例を提供しています。 ただし、これらの記事のコード例では、デスクトップアプリケーションが円滑なユーザーエクスペリエンスを提供するために処理する必要がある多くの高度なシナリオについては扱いません。 この記事では、これらのシナリオのいくつかと、関連するコードサンプルへのポインターに関するガイダンスを提供します。

## <a name="keyboard-input"></a>キーボード入力

各 XAML アイランドのキーボード入力を正しく処理するには、アプリケーションがすべての Windows メッセージを UWP XAML フレームワークに渡して、特定のメッセージが正しく処理されるようにする必要があります。 これを行うには、アプリケーションでメッセージループにアクセスできる場所で、各 XAML アイランドの**Desktopwindowxamlsource**オブジェクトを**IDesktopWindowXamlSourceNative2** COM インターフェイスにキャストします。 次に、このインターフェイスの**PreTranslateMessage**メソッドを呼び出し、現在のメッセージを渡します。

  * Win32:: アプリはメインメッセージループ内で直接**PreTranslateMessage**を呼び出すことができます。 **C++** 例については、 [Xamlbridge .cpp](https://github.com/microsoft/Xaml-Islands-Samples/blob/master/Samples/Win32/SampleCppApp/XamlBridge.cpp#L16)ファイルを参照してください。

  * **WPF:** アプリは、 [Componentdispatcher. ThreadFilterMessage](https://docs.microsoft.com/dotnet/api/system.windows.interop.componentdispatcher.threadfiltermessage)イベントのイベントハンドラーから**PreTranslateMessage**を呼び出すことができます。 例については、Windows Community Toolkit の[WindowsXamlHostBase.Focus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs#L177)ファイルを参照してください。

  * **Windows フォーム:** アプリは[system.windows.forms.control.preprocessmessage](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.preprocessmessage)メソッドのオーバーライドから**PreTranslateMessage**を呼び出すことができます。 例については、Windows Community Toolkit の[WindowsXamlHostBase.KeyboardFocus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs#L100)ファイルを参照してください。

## <a name="keyboard-focus-navigation"></a>キーボードフォーカスのナビゲーション

ユーザーがキーボードを使用してアプリケーションの UI 要素を移動するとき (たとえば、 **tab**キーまたは direction キーを押すなど)、プログラムを使用して**Desktopwindowxamlsource**オブジェクトにフォーカスを移動する必要があります。 ユーザーのキーボードナビゲーションが**Desktopwindowxamlsource**に到達したら、ui のナビゲーション順で最初の[windows. ui. .xaml](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)オブジェクトにフォーカスを移動します。ユーザーが要素を順番に移動するときは、次の**ui**オブジェクトにフォーカスを移動したまま、 **desktopwindowxamlsource**から親 UI 要素にフォーカスを移動します。  

UWP XAML ホスティング API は、これらのタスクを実行するのに役立ついくつかの型とメンバーを提供します。

* キーボードナビゲーションが**Desktopwindowxamlsource**に入ったときに、 [GotFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.gotfocus)イベントが発生します。 このイベントを処理し、プログラムを使用して、 [NavigateFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.navigatefocus)メソッドを使用して、最初にホストされる**Windows. UI. .xaml. UIElement**にフォーカスを移動します。

* ユーザーが**Desktopwindowxamlsource**でフォーカスを持つ最後の要素にあり、 **Tab**キーまたは方向キーを押すと、 [TakeFocusRequested](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.takefocusrequested)イベントが発生します。 このイベントを処理し、ホストアプリケーションの次のフォーカスがある要素にプログラムでフォーカスを移動します。 たとえば、 **Desktopwindowxamlsource**が[HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost)でホストされている WPF アプリケーションでは、 [system.windows.frameworkelement.movefocus](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.movefocus)メソッドを使用して、ホストアプリケーションの次にフォーカスがある要素にフォーカスを移すことができます。

これを実用的なサンプルアプリケーションのコンテキストで実行する方法を示す例については、次のコードファイルを参照してください。

  * /Win32: [xamlbridge .cpp](https://github.com/microsoft/Xaml-Islands-Samples/blob/master/Samples/Win32/SampleCppApp/XamlBridge.cpp)ファイルを参照してください。  **C++**

  * **WPF:** Windows Community Toolkit の[WindowsXamlHostBase.Focus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs)ファイルを参照してください。  

  * **Windows フォーム:** Windows Community Toolkit の[WindowsXamlHostBase.KeyboardFocus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs)ファイルを参照してください。

## <a name="handle-layout-changes"></a>レイアウトの変更を処理する

ユーザーが親 UI 要素のサイズを変更した場合は、必要なレイアウト変更を処理して、UWP コントロールが期待どおりに表示されるようにする必要があります。 ここでは、考慮すべき重要なシナリオをいくつか紹介します。

* C++ Win32 アプリケーションでは、アプリケーションが WM_SIZE メッセージを処理するときに、 [SetWindowPos](https://docs.microsoft.com/windows/desktop/api/winuser/nf-winuser-setwindowpos)関数を使用して、ホストされている XAML アイランドの位置を変更できます。 例については、「 [Sampleapp](https://github.com/microsoft/Xaml-Islands-Samples/blob/master/Samples/Win32/SampleCppApp/SampleApp.cpp#L170)のコードファイル」を参照してください。

* 親 UI 要素が、 **Desktopwindowxamlsource**でホストしている、 **windows の ui. .xaml. uielement**に適合させるために必要な四角形の領域のサイズを取得する必要がある場合は、 **windows の ui**の[Measure](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.measure)メソッドを呼び出します。 例 :

    * WPF アプリケーションでは、 **Desktopwindowxamlsource**をホストする[HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost)の[measureoverride](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.measureoverride)メソッドからこの操作を実行できます。 例については、Windows Community Toolkit の[WindowsXamlHostBase.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs)ファイルを参照してください。

    * Windows フォームアプリケーションでは、 **Desktopwindowxamlsource**をホストする[コントロール](https://docs.microsoft.com/dotnet/api/system.windows.forms.control)の[getpreferredsize](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.getpreferredsize)メソッドからこの操作を実行できます。 例については、Windows Community Toolkit の[WindowsXamlHostBase.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs)ファイルを参照してください。

* 親 UI 要素のサイズが変更されたときに、 **Desktopwindowxamlsource**でホストしているルートの**ui**の[配置](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.arrange)メソッドを呼び出します。 例 :

    * WPF アプリケーションでは、 **Desktopwindowxamlsource**をホストする[HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost)オブジェクトの配置[eoverride](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.arrangeoverride)メソッドからこの操作を行うことができます。 例については、Windows Community Toolkit の[WindowsXamlHost.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs)ファイルを参照してください。

    * Windows フォームアプリケーションでは、 **Desktopwindowxamlsource**をホストする[コントロール](https://docs.microsoft.com/dotnet/api/system.windows.forms.control)の[sizechanged](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.sizechanged)イベントのハンドラーからこの操作を実行できます。 例については、Windows Community Toolkit の[WindowsXamlHost.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs)ファイルを参照してください。

## <a name="handle-dpi-changes"></a>DPI 変更を処理する

UWP XAML フレームワークは、ホストされている UWP コントロールの DPI 変更を自動的に処理します (たとえば、画面の DPI が異なるモニター間でユーザーがウィンドウをドラッグした場合など)。 最適なエクスペリエンスを得るには、Windows フォーム、WPF、またはC++ Win32 アプリケーションをモニターごとの DPI 対応に構成することをお勧めします。

モニターごとの DPI に対応するようにアプリケーションを構成するには、 [side-by-side アセンブリマニフェスト](https://docs.microsoft.com/windows/desktop/SbsCs/application-manifests)をプロジェクトに追加し、 **\<dpiawareness\>** 要素を**PerMonitorV2**に設定します。 この値の詳細については、 [DPI_AWARENESS_CONTEXT_PER_MONITOR_AWARE_V2](https://docs.microsoft.com/windows/desktop/hidpi/dpi-awareness-context)の説明を参照してください。

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

## <a name="related-topics"></a>関連トピック

* [デスクトップアプリでの UWP XAML コントロールのホスト (XAML アイランド)](xaml-islands.md)
* [C++ Win32 アプリで UWP XAML ホスティング API を使用する](using-the-xaml-hosting-api.md)
* [C++ Win32 アプリで標準の UWP コントロールをホストする](host-standard-control-with-xaml-islands-cpp.md)
* [C++ Win32 アプリでカスタム UWP コントロールをホストする](host-custom-control-with-xaml-islands-cpp.md)
* [XAML アイランドコードサンプル](https://github.com/microsoft/Xaml-Islands-Samples)
* [C++Win32 XAML アイランドサンプル](https://github.com/microsoft/Xaml-Islands-Samples/tree/master/Samples/Win32/SampleCppApp)
