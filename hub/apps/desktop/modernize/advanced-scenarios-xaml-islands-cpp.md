---
description: この記事では、C++ Win32 アプリでの高度な XAML Islands ホスト シナリオについて説明します。
title: C++ Win32 アプリでの XAML Islands の高度なシナリオ
ms.date: 03/23/2020
ms.topic: article
keywords: Windows 10, UWP, cpp, Win32, XAML Islands, ラップされたコントロール, 標準コントロール
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 6270f6f486c8d0a2764ea20d29dd966f142f3630
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89170686"
---
# <a name="advanced-scenarios-for-xaml-islands-in-c-win32-apps"></a>C++ Win32 アプリでの XAML Islands の高度なシナリオ

[標準 UWP コントロールのホスト](host-standard-control-with-xaml-islands-cpp.md)および[カスタム UWP コントロールのホスト](host-custom-control-with-xaml-islands-cpp.md)に関する記事では、C++ Win32 アプリで XAML Islands をホストするための手順と例が提供されています。 ただし、これらの記事のコード例では、円滑なユーザー エクスペリエンスを提供するためにデスクトップ アプリケーションで処理する必要がある多くの高度なシナリオについては扱われていません。 この記事では、これらのシナリオのいくつかに関するガイダンスを提供し、関連するコード サンプルを示します。

## <a name="keyboard-input"></a>キーボード入力

各 XAML Island に対するキーボード入力を正しく処理するには、特定のメッセージが正しく処理されるように、アプリケーションですべての Windows メッセージを UWP XAML フレームワークに渡す必要があります。 これを行うには、メッセージ ループにアクセスできるアプリケーション内の場所で、各 XAML Island の **DesktopWindowXamlSource** オブジェクトを、**IDesktopWindowXamlSourceNative2** COM インターフェイスにキャストします。 次に、このインターフェイスの **PreTranslateMessage** メソッドを呼び出して、現在のメッセージを渡します。

  * **C++ Win32:** アプリのメイン メッセージ ループ内で **PreTranslateMessage** を直接呼び出すことができます。 例については、[XamlBridge.cpp](https://github.com/microsoft/Xaml-Islands-Samples/blob/master/Samples/Win32/SampleCppApp/XamlBridge.cpp#L16) ファイルをご覧ください。

  * **WPF:** アプリでは、[ComponentDispatcher.ThreadFilterMessage](/dotnet/api/system.windows.interop.componentdispatcher.threadfiltermessage) イベントのイベント ハンドラーから **PreTranslateMessage** を呼び出すことができます。 例については、Windows Community Toolkit の [WindowsXamlHostBase.Focus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs#L177) ファイルをご覧ください。

  * **Windows フォーム:** アプリでは、[Control.PreprocessMessage](/dotnet/api/system.windows.forms.control.preprocessmessage) メソッドのオーバーライドから、**PreTranslateMessage** を呼び出すことができます。 例については、Windows Community Toolkit の [WindowsXamlHostBase.KeyboardFocus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs#L100) ファイルをご覧ください。

## <a name="keyboard-focus-navigation"></a>キーボード フォーカスのナビゲーション

ユーザーがキーボードを使用してアプリケーションの UI 要素間を移動したとき (たとえば、**Tab** キーや方向キーが押された場合)、**DesktopWindowXamlSource** オブジェクトに、またはオブジェクトから、プログラムでフォーカスを移動する必要があります。 ユーザーのキーボード ナビゲーションが **DesktopWindowXamlSource** に達したら、UI のナビゲーション順序で最初の [Windows.UI.Xaml.UIElement](/uwp/api/windows.ui.xaml.uielement) オブジェクトにフォーカスを移動して、ユーザーが要素を順番に移動するのに合わせて以降の **Windows.UI.Xaml.UIElement** にフォーカスを移動した後、フォーカスを **DesktopWindowXamlSource** の外に出して親 UI 要素に戻します。  

UWP XAML ホスティング API には、これらのタスクを実行するのに役立ついくつかの型とメンバーが用意されています。

* **DesktopWindowXamlSource** にキーボード ナビゲーションが入ると、[GotFocus](/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.gotfocus) イベントが発生します。 このイベントをプログラムで処理し、[NavigateFocus](/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.navigatefocus) メソッドを使用することによって、ホストされている最初の **Windows.UI.Xaml.UIElement** にフォーカスを移動します。

* ユーザーが **DesktopWindowXamlSource** の最後のフォーカス可能な要素にいて、**Tab** キーまたは方向キーを押すと、[TakeFocusRequested](/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.takefocusrequested) イベントが発生します。 このイベントをプログラムで処理し、ホスト アプリケーションで次にフォーカス可能な要素にフォーカスを移動します。 たとえば、**DesktopWindowXamlSource** が [System.Windows.Interop.HwndHost](/dotnet/api/system.windows.interop.hwndhost) でホストされている WPF アプリケーションでは、[MoveFocus](/dotnet/api/system.windows.frameworkelement.movefocus) メソッドを使用して、ホスト アプリケーション内の次にフォーカス可能な要素にフォーカスを移すことができます。

動作するサンプル アプリケーションのコンテキストでこれを行う方法が示されている例については、次のコード ファイルを参照してください。

  * **C++/Win32**: [XamlBridge.cpp](https://github.com/microsoft/Xaml-Islands-Samples/blob/master/Samples/Win32/SampleCppApp/XamlBridge.cpp) ファイルをご覧ください。

  * **WPF:** Windows Community Toolkit の [WindowsXamlHostBase.Focus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs) ファイルをご覧ください。  

  * **Windows フォーム:** Windows Community Toolkit の [WindowsXamlHostBase.KeyboardFocus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs) ファイルをご覧ください。

## <a name="handle-layout-changes"></a>レイアウトの変更を処理する

ユーザーが親 UI 要素のサイズを変更した場合は、必要なレイアウト変更を処理して、UWP コントロールが意図したとおりに表示されるようにする必要があります。 ここでは、考慮すべき重要なシナリオをいくつか示します。

* C++ Win32 アプリケーションで WM_SIZE メッセージを処理するときは、[SetWindowPos](/windows/desktop/api/winuser/nf-winuser-setwindowpos) 関数を使用して、ホストされている XAML Islands の位置を変更できます。 例については、[SampleApp.cpp](https://github.com/microsoft/Xaml-Islands-Samples/blob/master/Samples/Win32/SampleCppApp/SampleApp.cpp#L170) コード ファイルをご覧ください。

* **DesktopWindowXamlSource** でホストしている **Windows.UI.Xaml.UIElement** に合わせるために必要な四角形の領域のサイズを、親 UI 要素で取得する必要がある場合は、**Windows.UI.Xaml.UIElement** の [Measure](/uwp/api/windows.ui.xaml.uielement.measure) メソッドを呼び出します。 たとえば、次のように入力します。

    * WPF アプリケーションでは、**DesktopWindowXamlSource** をホストしている [HwndHost](/dotnet/api/system.windows.interop.hwndhost) の [MeasureOverride](/dotnet/api/system.windows.frameworkelement.measureoverride) メソッドから、この操作を行うことができます。 例については、Windows Community Toolkit の [WindowsXamlHostBase.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs) ファイルをご覧ください。

    * Windows フォーム アプリケーションでは、**DesktopWindowXamlSource** をホストしている [Control](/dotnet/api/system.windows.forms.control) の [GetPreferredSize](/dotnet/api/system.windows.forms.control.getpreferredsize) メソッドから、この操作を行うことができます。 例については、Windows Community Toolkit の [WindowsXamlHostBase.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs) ファイルをご覧ください。

* 親 UI 要素のサイズが変更されたときは、**DesktopWindowXamlSource** でホストしているルート **Windows.UI.Xaml.UIElement** の [Arrange](/uwp/api/windows.ui.xaml.uielement.arrange) メソッドを呼び出します。 たとえば、次のように入力します。

    * WPF アプリケーションでは、**DesktopWindowXamlSource** をホストしている [HwndHost](/dotnet/api/system.windows.interop.hwndhost) オブジェクトの [ArrangeOverride](/dotnet/api/system.windows.frameworkelement.arrangeoverride) メソッドから、この操作を行うことができます。 例については、Windows Community Toolkit の [WindowsXamlHost.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs) ファイルをご覧ください。

    * Windows フォーム アプリケーションでは、**DesktopWindowXamlSource** をホストしている [Control](/dotnet/api/system.windows.forms.control) の [SizeChanged](/dotnet/api/system.windows.forms.control.sizechanged) イベントに対するハンドラーから、この操作を行うことができます。 例については、Windows Community Toolkit の [WindowsXamlHost.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs) ファイルをご覧ください。

## <a name="handle-dpi-changes"></a>DPI の変更を処理する

UWP XAML フレームワークは、ホストされている UWP コントロールの DPI の変更が自動的に処理されます (たとえば、ユーザーが画面の DPI が異なるモニター間でウィンドウをドラッグした場合)。 最適なエクスペリエンスのためには、Windows フォーム、WPF、または C++ Win32 アプリケーションを、モニターごとの DPI に対応するように構成することをお勧めします。

モニターごとの DPI に対応するようにアプリケーションを構成するには、[サイド バイ サイド アセンブリ マニフェスト](/windows/desktop/SbsCs/application-manifests)をプロジェクトに追加し、 **\<dpiAwareness\>** 要素を **PerMonitorV2** に設定します。 この値について詳しくは、[DPI_AWARENESS_CONTEXT_PER_MONITOR_AWARE_V2](/windows/desktop/hidpi/dpi-awareness-context) の説明をご覧ください。

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

* [デスクトップ アプリで UWP XAML コントロールをホストする (XAML Islands)](xaml-islands.md)
* [C++ Win32 アプリでの UWP XAML ホスティング API の使用](using-the-xaml-hosting-api.md)
* [C++ Win32 アプリで標準 UWP コントロールをホストする](host-standard-control-with-xaml-islands-cpp.md)
* [C++ Win32 アプリでカスタム UWP コントロールをホストする](host-custom-control-with-xaml-islands-cpp.md)
* [XAML Islands コード サンプル](https://github.com/microsoft/Xaml-Islands-Samples)
* [C++ Win32 XAML Islands のサンプル](https://github.com/microsoft/Xaml-Islands-Samples/tree/master/Samples/Win32/SampleCppApp)