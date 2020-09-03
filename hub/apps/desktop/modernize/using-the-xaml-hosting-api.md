---
description: この記事では、デスクトップ C++ Win32 アプリで UWP XAML UI をホストする方法について説明します。
title: UWP XAML を使用した C++ Win32 アプリでの API のホスト
ms.date: 03/23/2020
ms.topic: article
keywords: Windows 10, UWP, Windows フォーム, WPF, Win32, XAML Islands
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 8427519dd010553eb1f4f00f951dcc747a94b0c0
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174186"
---
# <a name="using-the-uwp-xaml-hosting-api-in-a-c-win32-app"></a>UWP XAML を使用した C++ Win32 アプリでの API のホスト

Windows 10 バージョン 1903 以降、UWP 以外のデスクトップ アプリ (C++ Win32、WPF、Windows フォーム アプリを含む) は *UWP XAML ホスティング API* を使用して、ウィンドウ ハンドル (HWND) に関連付けられた任意の UI 要素の UWP コントロールをホストできます。 この API により、UWP 以外のデスクトップ アプリは、UWP コントロール経由でのみ使用できる最新の Windows 10 UI 機能を使用できます。 たとえば、UWP 以外のデスクトップ アプリはこの API を使用して、[Fluent Design System](/windows/uwp/design/fluent-design-system/index) を使用し、[Windows Ink](/windows/uwp/design/input/pen-and-stylus-interactions) をサポートする UWP コントロールをホストできます。

UWP XAML ホスティング API により、開発者が UWP 以外のデスクトップ アプリに Fluent UI を導入できるように用意されているより幅広い一連のコントロールのための基礎が提供されます。 この機能は *XAML Islands* と呼ばれます。 この機能の概要については、「[デスクトップ アプリで UWP XAML コントロールをホストする (XAML Islands)](xaml-islands.md)」を参照してください。

> [!NOTE]
> XAML Islands に関するフィードバックがある場合は、[Microsoft.Toolkit.Win32 リポジトリ](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/issues)に新しい問題を作成し、そこにコメントを残してください。 個人的にフィードバックを送信したい場合は、XamlIslandsFeedback@microsoft.com に送信できます。 お客様の洞察とシナリオは弊社にとって非常に重要です。

## <a name="is-the-uwp-xaml-hosting-api-the-right-choice-for-your-desktop-app"></a>UWP XAML ホスティング API はデスクトップ アプリのための適切な選択肢ですか?

UWP XAML ホスティング API により、デスクトップ アプリで UWP コントロールをホストするための低レベルのインフラストラクチャが提供されます。 デスクトップ アプリの種類によっては、別のより便利な API を使用してこの目標を達成するオプションがある場合があります。

* C++ Win32 デスクトップ アプリがあり、そのアプリで UWP コントロールをホストしたい場合は、UWP XAML ホスティング API を使用する必要があります。 これらのアプリの種類には別の手段がありません。

* WPF および Windows フォーム アプリの場合は、直接 UWP XAML ホスティング API を使用するのではなく、Windows コミュニティ ツールキットの [XAML Island .NET コントロール](xaml-islands.md#wpf-and-windows-forms-applications)を使用することを強くお勧めします。 これらのコントロールでは、内部的に UWP XAML ホスティング API を使用し、キーボード ナビゲーションやレイアウト変更などの、直接 UWP XAML ホスティング API を使用している場合には自分で処理する必要があるすべての動作を実装しています。

UWP XAML ホスティング API は C++ Win32 アプリでのみ使用することが推奨されるため、この記事では主に、C++ Win32 アプリのための指示と例について説明します。 ただし、必要に応じて WPF および Windows フォーム アプリでも UWP XAML ホスティング API を使用できます。 この記事では、Windows コミュニティ ツールキットの WPF と Windows フォーム用の[ホスト コントロール](xaml-islands.md#host-controls)の関連するソース コードを示しているため、これらのコントロールで UWP XAML ホスティング API がどのように使用されているかを確認できます。

## <a name="learn-how-to-use-the-xaml-hosting-api"></a>XAML ホスティング API の使用方法を確認する

C++ Win32 アプリで XAML ホスティング API を使用するためのコード例の詳細な手順に従うには、次の記事を参照してください。

* [標準 UWP コントロールをホストする](host-standard-control-with-xaml-islands-cpp.md)
* [カスタム UWP コントロールをホストする](host-custom-control-with-xaml-islands-cpp.md)
* [高度なシナリオ](advanced-scenarios-xaml-islands-cpp.md)

## <a name="samples"></a>サンプル

コードで UWP XAML ホスティング API を使用する方法は、アプリの種類やアプリの設計などの要因によって異なります。 完全なアプリのコンテキストでこの API を使用する方法を示すために、この記事では次のサンプルのコードを参照します。

### <a name="c-win32"></a>C++ Win32

次のサンプルは、C++ Win32 アプリで UWP XAML ホスティング API を使用する方法を示しています。

* [単純な XAML Island のサンプル](https://github.com/microsoft/Xaml-Islands-Samples/tree/master/Standalone_Samples/CppWinRT_Basic_Win32App)。 このサンプルは、パッケージ化されていない C++ Win32 アプリ (つまり、MSIX パッケージに組み込まれていないアプリ) での UWP コントロールのホスティングの基本的な実装を示しています。

* [カスタム コントロール サンプルを含む XAML Island](https://github.com/microsoft/Xaml-Islands-Samples/tree/master/Samples/Win32)。 このサンプルは、パッケージ化された C++ Win32 アプリでのカスタム UWP コントロールのホスティングのほか、キーボード入力やフォーカス ナビゲーションなどのその他の動作の処理の完全な実装を示しています。

### <a name="wpf-and-windows-forms"></a>WPF と Windows フォーム

Windows コミュニティ ツールキットの [WindowsXamlHost](/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) コントロールは、WPF および Windows フォーム アプリで UWP ホスティング API を使用するためのリファレンス サンプルとして機能します。 このソース コードは、次の場所で入手できます。

* コントロールの WPF バージョンについては、[ここに移動](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Wpf.UI.XamlHost)してください。 WPF バージョンは、[System.Windows.Interop.HwndHost](/dotnet/api/system.windows.interop.hwndhost) から派生します。

* コントロールの Windows フォーム バージョンについては、[ここに移動](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Forms.UI.XamlHost)してください。 Windows フォーム バージョンは、[System.Windows.Forms.Control](/dotnet/api/system.windows.forms.control) から派生します。

> [!NOTE]
> WPF および Windows フォーム アプリで直接 UWP XAML ホスティング API を使用するのではなく、Windows コミュニティ ツールキットの [XAML Island .NET コントロール](xaml-islands.md#wpf-and-windows-forms-applications)を使用することを強くお勧めします。 この記事の WPF と Windows フォームのサンプル リンクは、説明のためにのみ示されています。

## <a name="architecture-of-the-api"></a>API のアーキテクチャ

UWP XAML ホスティング API には、次の主な Windows ランタイム型と COM インターフェイスが含まれています。

|  型またはインターフェイス | 説明 |
|--------------------|-------------|
| [WindowsXamlManager](/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) | このクラスは、UWP XAML フレームワークを表します。 このクラスは、デスクトップ アプリの現在のスレッドで UWP XAML フレームワークを初期化する 1 つの静的 [InitializeForCurrentThread](/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager.initializeforcurrentthread) メソッドを提供します。 |
| [DesktopWindowXamlSource](/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) | このクラスは、デスクトップ アプリでホストしている UWP XAML コンテンツのインスタンスを表します。 このクラスの最も重要なメンバーは、[Content](/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content) プロパティです。 このプロパティを、ホストする [Windows.UI.Xaml.UIElement](/uwp/api/windows.ui.xaml.uielement) に割り当てます。 このクラスにはまた、XAML Islands との間でキーボード フォーカス ナビゲーションをルーティングするための他のメンバーも含まれています。 |
| IDesktopWindowXamlSourceNative | この COM インターフェイスは、アプリ内の XAML Island を親 UI 要素にアタッチするために使用する **AttachToWindow** メソッドを提供します。 すべての **DesktopWindowXamlSource** オブジェクトがこのインターフェイスを実装しています。 このインターフェイスは、windows.ui.xaml.hosting.desktopwindowxamlsource.h で宣言されています。 |
| IDesktopWindowXamlSourceNative2 | この COM インターフェイスは、UWP XAML フレームワークが特定の Windows メッセージを適切に処理できるようにする **PreTranslateMessage** メソッドを提供します。 すべての **DesktopWindowXamlSource** オブジェクトがこのインターフェイスを実装しています。 このインターフェイスは、windows.ui.xaml.hosting.desktopwindowxamlsource.h で宣言されています。 |

次の図は、デスクトップ アプリでホストされている XAML Island 内のオブジェクトの階層を示しています。

* 基本レベルには、XAML Island をホストするアプリ内の UI 要素があります。 この UI 要素にはウィンドウ ハンドル (HWND) が必要です。 XAML Island をホストできる UI 要素の例には、C++ Win32 アプリの[ウィンドウ](/windows/desktop/winmsg/about-windows)、WPF アプリの [System.Windows.Interop.HwndHost](/dotnet/api/system.windows.interop.hwndhost)、Windows フォーム アプリの [System.Windows.Forms.Control](/dotnet/api/system.windows.forms.control) などがあります。

* 次のレベルには、**DesktopWindowXamlSource** オブジェクトがあります。 このオブジェクトは、XAML Island をホストするためのインフラストラクチャを提供します。 このオブジェクトを作成し、それを親 UI 要素にアタッチするのはユーザー コードの役割です。

* **DesktopWindowXamlSource** を作成すると、このオブジェクトにより、UWP コントロールをホストするためのネイティブ子ウィンドウが自動的に作成されます。 このネイティブ子ウィンドウはユーザー コードからほとんど抽象化されていますが、必要に応じてそのハンドル (HWND) にアクセスできます。

* 最後に、最上位レベルには、デスクトップ アプリでホストする UWP コントロールがあります。 これは、Windows SDK によって提供される UWP コントロールやカスタム ユーザー コントロールなど、[Windows.UI.Xaml.UIElement](/uwp/api/windows.ui.xaml.uielement) から派生する任意の UWP オブジェクトでかまいません。

![DesktopWindowXamlSource のアーキテクチャ](images/xaml-islands/xaml-hosting-api-rev2.png)

> [!NOTE]
> デスクトップ アプリで XAML Islands をホストすると、XAML コンテンツの複数のツリーを同じスレッド上で同時に実行できます。 XAML Island で XAML コンテンツのツリーのルート要素にアクセスし、それがホストされているコンテキストに関する関連情報を取得するには、[XamlRoot](/uwp/api/windows.ui.xaml.xamlroot) クラスを使用します。 [CoreWindow](/uwp/api/windows.ui.core.corewindow)、[ApplicationView](/uwp/api/windows.ui.viewmanagement.applicationview)、[Window](/uwp/api/windows.ui.xaml.window) の各 API では、XAML Islands に関する正しい情報が提供されません。 詳しくは、[このセクション](xaml-islands.md#window-host-context-for-xaml-islands)をご覧ください。

## <a name="troubleshooting"></a>トラブルシューティング

### <a name="error-using-uwp-xaml-hosting-api-in-a-uwp-app"></a>UWP アプリで UWP XAML ホスティング API を使用しているときのエラー

| 問題 | 解決方法 |
|-------|------------|
| アプリが、次のメッセージを含む **COMException** を受信します。"DesktopWindowXamlSource をアクティブ化できません。 この型は UWP アプリでは使用できません。" または "WindowsXamlManager をアクティブ化できません。 この型は UWP アプリでは使用できません。" | このエラーは、UWP アプリで UWP XAML ホスティング API を使用しようとしている (具体的には、[DesktopWindowXamlSource](/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) または [WindowsXamlManager](/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) 型をインスタンス化しようとしている) ことを示しています。 UWP XAML ホスティング API は、UWP 以外のデスクトップ アプリ (WPF、Windows フォーム、C++ Win32 アプリケーションなど) で使用されることのみを目的にしています。 |

### <a name="error-trying-to-use-the-windowsxamlmanager-or-desktopwindowxamlsource-types"></a>WindowsXamlManager または DesktopWindowXamlSource 型を使用しようとしているときのエラー

| 問題 | 解決方法 |
|-------|------------|
| アプリが、次のメッセージを含む例外を受信します。"WindowsXamlManager と DesktopWindowXamlSource は、Windows バージョン 10.0.18226.0 以降を対象とするアプリに対してサポートされています。 アプリケーション マニフェストまたはパッケージ マニフェストを確認し、MaxTestedVersion プロパティが更新されていることを確認してください。" | このエラーは、アプリケーションが UWP XAML ホスティング API で **WindowsXamlManager** または **DesktopWindowXamlSource** 型を使用しようとしたが、そのアプリが Windows 10 バージョン 1903 以降を対象としてビルドされたかどうかを OS が確認できないことを示しています。 UWP XAML ホスティング API は最初、以前のバージョンの Windows 10 でプレビューとして導入されましたが、Windows 10 バージョン 1903 以降でのみサポートされています。</p></p>この問題を解決するには、そのアプリの MSIX パッケージを作成し、それをパッケージから実行するか、または [Microsoft.Toolkit.Win32.UI.SDK](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.SDK) NuGet パッケージをプロジェクトにインストールします。  |

### <a name="error-attaching-to-a-window-on-a-different-thread"></a>別のスレッドのウィンドウにアタッチしているときのエラー

| 問題 | 解決方法 |
|-------|------------|
| アプリが、次のメッセージを含む **COMException** を受信します。"指定された HWND が別のスレッドで作成されたため、AttachToWindow メソッドが失敗しました。" | このエラーは、アプリケーションが **IDesktopWindowXamlSourceNative::AttachToWindow** メソッドを呼び出し、それに別のスレッドで作成されたウィンドウの HWND を渡したことを示しています。 このメソッドには、そのメソッドを呼び出しているコードと同じスレッドで作成されたウィンドウの HWND を渡す必要があります。 |

### <a name="error-attaching-to-a-window-on-a-different-top-level-window"></a>別のトップレベル ウィンドウのウィンドウにアタッチしているときのエラー

| 問題 | 解決方法 |
|-------|------------|
| アプリが、次のメッセージを含む **COMException** を受信します。"指定された HWND が、以前に同じスレッドで AttachToWindow に渡された HWND とは別のトップレベル ウィンドウから派生しているため、AttachToWindow メソッドが失敗しました。" | このエラーは、アプリケーションが **IDesktopWindowXamlSourceNative::AttachToWindow** メソッドを呼び出し、それに同じスレッドでのこのメソッドへの以前の呼び出しで指定されたウィンドウとは別のトップレベル ウィンドウから派生しているウィンドウの HWND を渡したことを示しています。</p></p>アプリケーションが特定のスレッドで **AttachToWindow** を呼び出した後、同じスレッドのその他のすべての [DesktopWindowXamlSource](/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) オブジェクトは、**AttachToWindow** への最初の呼び出しで渡されたのと同じトップレベル ウィンドウの子孫であるウィンドウにのみアタッチできます。 特定のスレッドのすべての **DesktopWindowXamlSource** オブジェクトが閉じられた後は、次の **DesktopWindowXamlSource** を再び任意のウィンドウに自由にアタッチできます。</p></p>この問題を解決するには、このスレッドの他のトップレベル ウィンドウにバインドされているすべての **DesktopWindowXamlSource** オブジェクトを閉じるか、またはこの **DesktopWindowXamlSource** のための新しいスレッドを作成します。 |

## <a name="related-topics"></a>関連トピック

* [デスクトップ アプリで UWP XAML コントロールをホストする (XAML Islands)](xaml-islands.md)
* [C++ Win32 アプリで標準 UWP コントロールをホストする](host-standard-control-with-xaml-islands-cpp.md)
* [C++ Win32 アプリでカスタム UWP コントロールをホストする](host-custom-control-with-xaml-islands-cpp.md)
* [C++ Win32 アプリでの XAML Islands の高度なシナリオ](advanced-scenarios-xaml-islands-cpp.md)
* [XAML Islands のコード サンプル](https://github.com/microsoft/Xaml-Islands-Samples)
* [C++ Win32 XAML Islands のサンプル](https://github.com/microsoft/Xaml-Islands-Samples/tree/master/Samples/Win32/SampleCppApp)