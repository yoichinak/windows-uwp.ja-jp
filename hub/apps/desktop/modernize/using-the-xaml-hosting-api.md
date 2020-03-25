---
description: この記事では、デスクトップC++ Win32 アプリで UWP XAML UI をホストする方法について説明します。
title: UWP XAML を使用した C++ Win32 アプリでの API のホスト
ms.date: 03/23/2020
ms.topic: article
keywords: windows 10、uwp、windows フォーム、wpf、win32、xaml islands
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 36c3aeb7a51c84e92c5bca461aee7efe50740237
ms.sourcegitcommit: c660def841abc742600fbcf6ed98e1f4f7beb8cc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/24/2020
ms.locfileid: "80218462"
---
# <a name="using-the-uwp-xaml-hosting-api-in-a-c-win32-app"></a>UWP XAML を使用した C++ Win32 アプリでの API のホスト

Windows 10 バージョン1903以降、UWP 以外のデスクトップアプリ (Win32、WPF C++ 、Windows フォームアプリを含む) は、 *UWP XAML ホスティング API*を使用して、ウィンドウハンドル (HWND) に関連付けられている任意の UI 要素で uwp コントロールをホストできます。 この API によって、uwp 以外のデスクトップアプリは、UWP コントロール経由でのみ使用できる最新の Windows 10 UI 機能を使用できるようになります。 たとえば、UWP 以外のデスクトップアプリは、この API を使用して、 [Fluent デザインシステム](/windows/uwp/design/fluent-design-system/index)を使用し、 [Windows Ink](/windows/uwp/design/input/pen-and-stylus-interactions)をサポートする UWP コントロールをホストできます。

UWP XAML ホスティング API は、開発者が UWP デスクトップアプリに対して Fluent UI を取り込むことができるようにするために用意されている、より広範なコントロールセットの基盤を提供します。 この機能は、 *XAML アイランド*と呼ばれます。 この機能の概要については、「[デスクトップアプリでの UWP XAML コントロールのホスト (Xaml アイランド)](xaml-islands.md)」を参照してください。

> [!NOTE]
> XAML Islands に関するフィードバックがある場合は、[Microsoft.Toolkit.Win32 リポジトリ](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/issues)に新しい問題を作成し、そこにコメントを残してください。 個人的にフィードバックを送信したい場合は、XamlIslandsFeedback@microsoft.com に送信できます。 あなたの洞察とシナリオは弊社にとって非常に重要です。

## <a name="is-the-uwp-xaml-hosting-api-the-right-choice-for-your-desktop-app"></a>UWP XAML ホスティング API は、デスクトップアプリに最適な選択ですか。

UWP XAML ホスティング API は、デスクトップアプリで UWP コントロールをホストするための低レベルのインフラストラクチャを提供します。 デスクトップアプリの種類によっては、別の便利な Api を使用してこの目標を達成するオプションがあります。

* C++ Win32 デスクトップアプリがあり、uwp コントロールをアプリでホストする場合は、uwp XAML ホスティング API を使用する必要があります。 これらの種類のアプリに代わる方法はありません。

* WPF アプリと Windows フォームアプリの場合、UWP XAML ホスティング API を直接使用するのではなく、Windows Community Toolkit で[Xaml アイランド .net コントロール](xaml-islands.md#wpf-and-windows-forms-applications)を使用することを強くお勧めします。 これらのコントロールは UWP XAML ホスティング API を内部的に使用し、キーボードナビゲーションやレイアウトの変更など、UWP XAML ホスティング API を直接使用した場合に対処する必要がある動作をすべて実装しています。

この記事では、 C++ win32 アプリのみが UWP XAML ホスティング API を使用することをお勧めしますC++ 。この記事では、主に win32 アプリの手順と例について説明します。 ただし、WPF で UWP XAML ホスティング API を使用して、Windows フォームアプリを選択することもできます。 この記事では、WPF の[ホストコントロール](xaml-islands.md#host-controls)と Windows Community Toolkit の Windows フォームに関連するソースコードについて説明します。そのため、これらのコントロールで UWP XAML ホスティング API がどのように使用されているかを確認できます。

## <a name="learn-how-to-use-the-xaml-hosting-api"></a>XAML ホスティング API の使用方法について説明します。

Win32 アプリでC++ XAML ホスティング API を使用するためのコード例を含む詳細な手順については、次の記事を参照してください。

* [標準の UWP コントロールをホストする](host-standard-control-with-xaml-islands-cpp.md)
* [カスタム UWP コントロールをホストする](host-custom-control-with-xaml-islands-cpp.md)
* [高度なシナリオ](advanced-scenarios-xaml-islands-cpp.md)

## <a name="samples"></a>Samples

コードで UWP XAML ホスティング API を使用する方法は、アプリの種類、アプリの設計、およびその他の要因によって異なります。 この API を完全なアプリのコンテキストで使用する方法を説明するために、この記事では次のサンプルのコードを示します。

### <a name="c-win32"></a>C++32

次のサンプルは、 C++ Win32 アプリで UWP XAML ホスティング API を使用する方法を示しています。

* [単純な XAML アイランドサンプル](https://github.com/microsoft/Xaml-Islands-Samples/tree/master/Standalone_Samples/CppWinRT_Basic_Win32App)。 このサンプルでは、パッケージ化されていないC++ Win32 アプリ (つまり、msix パッケージに組み込まれていないアプリ) で UWP コントロールをホストする基本的な実装を示します。

* [カスタムコントロールのサンプルを使用した XAML 島](https://github.com/microsoft/Xaml-Islands-Samples/tree/master/Samples/Win32)。 このサンプルでは、パッケージ化C++された Win32 アプリでカスタム UWP コントロールをホストする完全な実装と、キーボード入力やフォーカス移動などの他の動作を処理する方法を示します。

### <a name="wpf-and-windows-forms"></a>WPF と Windows フォーム

Windows Community Toolkit の[Windowsxamlhost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)コントロールは、WPF および Windows フォームアプリで UWP ホスティング API を使用するためのリファレンスサンプルとして機能します。 ソースコードは次の場所で入手できます。

* コントロールの WPF バージョンについては、[こちらを参照](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Wpf.UI.XamlHost)してください。 WPF バージョンは[HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost)から派生します。

* コントロールの Windows フォームバージョンについては、[こちらを参照](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Forms.UI.XamlHost)してください。 Windows フォームのバージョンは、 [system.string から派生します](https://docs.microsoft.com/dotnet/api/system.windows.forms.control)。

> [!NOTE]
> WPF と Windows フォームアプリで直接 UWP XAML ホスティング API を使用するのではなく、Windows Community Toolkit で[Xaml アイランド .net コントロール](xaml-islands.md#wpf-and-windows-forms-applications)を使用することを強くお勧めします。 この記事の WPF と Windows フォームのサンプルリンクは、説明を目的としたものです。

## <a name="architecture-of-the-api"></a>API のアーキテクチャ

UWP XAML ホスティング API には、これらの主な Windows ランタイム型および COM インターフェイスが含まれています。

|  型またはインターフェイス | 説明 |
|--------------------|-------------|
| [WindowsXamlManager](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) | このクラスは、UWP XAML フレームワークを表します。 このクラスには、デスクトップアプリの現在のスレッドで UWP XAML フレームワークを初期化する単一の静的[Initializeforcurrentthread](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager.initializeforcurrentthread)メソッドが用意されています。 |
| [DesktopWindowXamlSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) | このクラスは、デスクトップアプリでホストする UWP XAML コンテンツのインスタンスを表します。 このクラスの最も重要なメンバーは、[コンテンツ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content)プロパティです。 このプロパティは、ホストする[Windows の UI. .xaml. UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)に割り当てます。 このクラスには、ルーティングのキーボード フォーカスのナビゲーション、および XAML Islands からの他のメンバーもあります。 |
| IDesktopWindowXamlSourceNative | この COM インターフェイスには**Attachtowindow**メソッドが用意されています。このメソッドは、アプリの XAML アイランドを親 UI 要素にアタッチするために使用します。 すべての**Desktopwindowxamlsource**オブジェクトは、このインターフェイスを実装します。 このインターフェイスは、windows. ui. app.xaml. .h と宣言されています。 |
| IDesktopWindowXamlSourceNative2 | この COM インターフェイスには、UWP XAML フレームワークが特定の Windows メッセージを正しく処理できるようにする**PreTranslateMessage**メソッドが用意されています。 すべての**Desktopwindowxamlsource**オブジェクトは、このインターフェイスを実装します。 このインターフェイスは、windows. ui. app.xaml. .h と宣言されています。 |

次の図は、デスクトップアプリでホストされている XAML アイランド内のオブジェクトの階層を示しています。

* 基本レベルは、XAML アイランドをホストするアプリ内の UI 要素です。 この UI 要素には、ウィンドウハンドル (HWND) が必要です。 XAML アイランドをホストできる UI 要素の例としては、 [window](https://docs.microsoft.com/windows/desktop/winmsg/about-windows) Win32 アプリC++用のウィンドウ、WPF アプリの[HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) 、および Windows フォームアプリ用のがありますが、この[ような場合](https://docs.microsoft.com/dotnet/api/system.windows.forms.control)です。

* 次のレベルには、 **Desktopwindowxamlsource**オブジェクトがあります。 このオブジェクトは、XAML Islandsをホストするためのインフラストラクチャを提供します。 コードは、このオブジェクトを作成し、親 UI 要素にアタッチする役割を担います。

* **Desktopwindowxamlsource**を作成すると、このオブジェクトによって、UWP コントロールをホストするネイティブの子ウィンドウが自動的に作成されます。 このネイティブの子ウィンドウは、ほとんどの場合、コードからは抽象化されていませんが、必要に応じてハンドル (HWND) にアクセスできます。

* 最後に、最上位レベルは、デスクトップアプリでホストする UWP コントロールです。 これには、Windows SDK によって提供される UWP コントロールやカスタムユーザーコントロールなど、 [Windows の UI](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)から派生する任意の uwp オブジェクトを指定できます。

![DesktopWindowXamlSource アーキテクチャ](images/xaml-islands/xaml-hosting-api-rev2.png)

> [!NOTE]
> デスクトップ アプリで XAML Islands をホストすると、XAML コンテンツの複数のツリーを同じスレッド上で同時に実行できます。 XAML Island で XAML コンテンツのツリーのルート要素にアクセスし、それがホストされているコンテキストに関する関連情報を取得するには、[XamlRoot](https://docs.microsoft.com/uwp/api/windows.ui.xaml.xamlroot) クラスを使用します。 [Corewindow](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow)、 [applicationview](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationview)、および[Window](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window) api は、XAML アイランドに関する正しい情報を提供しません。 詳細については、[このセクション](xaml-islands.md#window-host-context-for-xaml-islands)を参照してください。

## <a name="troubleshooting"></a>トラブルシューティング

### <a name="error-using-uwp-xaml-hosting-api-in-a-uwp-app"></a>Uwp アプリで UWP XAML ホスティング API を使用するときのエラー

| 問題 | 解決方法 |
|-------|------------|
| アプリは、"DesktopWindowXamlSource をアクティブにできません" というメッセージが表示された**COMException**を受け取ります。 この型は UWP アプリでは使用できません。 " または、"WindowsXamlManager をアクティブにできません。 この型は UWP アプリでは使用できません。 " | このエラーは、uwp アプリで UWP XAML ホスティング API (具体的には、 [Desktopwindowxamlsource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource)または[windowsxamlmanager](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager)の型をインスタンス化しようとしている) を使用しようとしていることを示しています。 UWP XAML ホスティング API は、WPF、Windows フォーム、 C++ Win32 アプリケーションなど、uwp 以外のデスクトップアプリでのみ使用することを目的としています。 |

### <a name="error-trying-to-use-the-windowsxamlmanager-or-desktopwindowxamlsource-types"></a>WindowsXamlManager または DesktopWindowXamlSource 型を使用しようとしたときにエラーが発生しました

| 問題 | 解決方法 |
|-------|------------|
| アプリは次のメッセージで例外を受け取ります。 "WindowsXamlManager と DesktopWindowXamlSource は、Windows バージョン10.0.18226.0 以降を対象とするアプリでサポートされています。 アプリケーションマニフェストまたはパッケージマニフェストを確認し、MaxTestedVersion プロパティが更新されていることを確認してください。 " | このエラーは、アプリケーションが UWP XAML ホスティング API で**Windowsxamlmanager**または**Desktopwindowxamlsource**型を使用しようとしましたが、OS は、アプリが Windows 10 バージョン1903以降を対象とするようにビルドされているかどうかを判断できないことを示しています。 UWP XAML ホスティング API は、以前のバージョンの Windows 10 では最初にプレビューとして導入されましたが、Windows 10 バージョン1903以降でのみサポートされています。</p></p>この問題を解決するには、アプリ用の MSIX パッケージを作成してパッケージから実行するか、プロジェクトに[Microsoft. Toolkit. UI. SDK](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.SDK) NuGet パッケージをインストールします。  |

### <a name="error-attaching-to-a-window-on-a-different-thread"></a>別のスレッドのウィンドウへのアタッチエラー

| 問題 | 解決方法 |
|-------|------------|
| アプリは次のメッセージで**COMException**を受け取ります: "AttachToWindow メソッドは、指定された HWND が別のスレッドで作成されたため失敗しました。" | このエラーは、アプリケーションが**Idesktopwindowxamlsourcenative:: AttachToWindow**メソッドを呼び出し、別のスレッドで作成されたウィンドウの HWND に渡されたことを示します。 このメソッドは、メソッドの呼び出し元のコードと同じスレッド上で作成されたウィンドウの HWND に渡す必要があります。 |

### <a name="error-attaching-to-a-window-on-a-different-top-level-window"></a>別のトップレベルウィンドウ上のウィンドウへのアタッチエラー

| 問題 | 解決方法 |
|-------|------------|
| アプリは、"AttachToWindow メソッドが失敗しました **" というメッセージが表示**されます。これは、指定した hwnd が、以前に同じスレッドの attachtowindow に渡された hwnd とは異なるトップレベルウィンドウからのものであるためです。 | このエラーは、アプリケーションが**Idesktopwindowxamlsourcenative:: AttachToWindow**メソッドを呼び出し、同じスレッドでこのメソッドの前の呼び出しで指定されたウィンドウとは別のトップレベルウィンドウから降下するウィンドウの HWND を渡したことを示します。</p></p>アプリケーションが特定のスレッドで**Attachtowindow**を呼び出すと、同じスレッド上の他のすべての[Desktopwindowxamlsource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource)オブジェクトは、 **attachtowindow**への最初の呼び出しで渡されたのと同じトップレベルウィンドウの子孫であるウィンドウにのみアタッチできます。 特定のスレッドですべての**desktopwindowxamlsource**オブジェクトが閉じられると、次の**desktopwindowxamlsource**は、任意のウィンドウに再びアタッチできます。</p></p>この問題を解決するには、このスレッドの他のトップレベルウィンドウにバインドされているすべての**desktopwindowxamlsource**オブジェクトを閉じるか、またはこの**desktopwindowxamlsource**に新しいスレッドを作成します。 |

## <a name="related-topics"></a>関連トピック

* [デスクトップアプリでの UWP XAML コントロールのホスト (XAML アイランド)](xaml-islands.md)
* [C++ Win32 アプリで標準の UWP コントロールをホストする](host-standard-control-with-xaml-islands-cpp.md)
* [C++ Win32 アプリでカスタム UWP コントロールをホストする](host-custom-control-with-xaml-islands-cpp.md)
* [Win32 アプリでC++の XAML アイランドの高度なシナリオ](advanced-scenarios-xaml-islands-cpp.md)
* [XAML アイランドコードサンプル](https://github.com/microsoft/Xaml-Islands-Samples)
* [C++Win32 XAML アイランドサンプル](https://github.com/microsoft/Xaml-Islands-Samples/tree/master/Samples/Win32/SampleCppApp)
