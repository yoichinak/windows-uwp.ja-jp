---
description: この記事では、Windows アプリのための Visual Studio のプロジェクト テンプレートと項目テンプレートの概要について説明します。
title: Windows アプリ用の Visual Studio プロジェクトおよび項目テンプレート
ms.date: 07/02/2020
ms.topic: article
keywords: windows 10、uwp、windows フォーム、wpf、xaml islands
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: high
ms.openlocfilehash: 7e8500aab3c6eeaa1552dc61a95ea7404bf4d5d5
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174206"
---
# <a name="visual-studio-project-and-item-templates-for-windows-apps"></a>Windows アプリ用の Visual Studio プロジェクトおよび項目テンプレート

Visual Studio 2019 によって提供される多数のプロジェクト テンプレートと項目テンプレートは、C\# または C++ を使用して Windows 10 デバイス用のアプリをビルドするときに役立ちます。 このトピックではテンプレートについて説明しますが、これによって、シナリオに合わせてテンプレートを選ぶことができます。

* プロジェクト テンプレートには、プロジェクト ファイル、コード ファイル、アプリをビルドするように構成されているその他のアセット、またはアプリで読み込んで使用できるコンポーネントが含まれます。
* 項目テンプレートは、共通して使用されるコードと、プロジェクトに追加すると開発時間を短縮できる XAML を含むプロジェクト ファイルです。 たとえば、項目テンプレートを使用すると、新しいウィンドウ、ページ、またはコントロールをアプリに追加できます。

## <a name="winui-templates"></a>WinUI テンプレート

[Windows UI ライブラリ (WinUI)](../winui/index.md) は、デスクトップ (.NET およびネイティブ Win32) と UWP アプリ プラットフォームにおける、Windows アプリ用の最新のネイティブ ユーザー インターフェイス (UI) プラットフォームです。 [WinUI 3](../winui/winui3/index.md) (現在開発者用プレビューとして提供中) は、WinUI の最新メジャー バージョンであり、WinUI をデスクトップ Windows アプリ用の完全な UX フレームワークに変換します。

WinUI 3 には Visual Studio 2019 用の VSIX パッケージが含まれており、これによって WinUI ベースのインターフェイスを備えたアプリのビルドを開始するために役立つプロジェクト テンプレートと項目テンプレートが提供されます。 WinUI 3 VSIX パッケージと、それによって提供されるプロジェクト テンプレートの詳細については、[このセクション](../winui/winui3/index.md#install-winui-3-preview-2)を参照してください。

> [!IMPORTANT]
> WinUI 3 は、関連する Visual Studio テンプレートを含めて、現在開発者プレビューとして提供されています。これは、初期評価を目的としたものであり、開発者コミュニティからフィードバックを収集します。 この時点では実稼働アプリには使用できません。

## <a name="uwp-templates"></a>UWP テンプレート

Visual Studio には、C# または C++ を使用して UWP アプリをビルドするためのさまざまなプロジェクト テンプレートが用意されています。 これらのプロジェクト テンプレートを使用するには、Visual Studio をインストールするときに、**ユニバーサル Windows プラットフォーム開発**ワークロードを含める必要があります。 C++ プロジェクト テンプレートの場合は、**ユニバーサル Windows プラットフォーム開発**ワークロードの **C++ (v142) ユニバーサル Windows プラットフォーム ツール** オプション コンポーネントも含める必要があります。

### <a name="project-templates-for-c-and-uwp"></a>C# および UWP のプロジェクト テンプレート

Visual Studio で新しいプロジェクトを作成するときに UWP C# プロジェクト テンプレートを利用するには、言語を **[C#]** 、プラットフォームを **[Windows]** 、プロジェクトの種類を **[UWP]** としてフィルター処理します。

![UWP C# プロジェクト テンプレート](images/uwp-projects-csharp.png)

これらのプロジェクト テンプレートを使用して C# UWP アプリを作成できます。

| テンプレート | 説明 |
|----------|-------------|
| 空のアプリ (ユニバーサル Windows) | UWP アプリを作成します。 生成されるプロジェクトには、UI のビルドを開始するために使用できる [Windows.UI.Xaml.Controls.Page](/uwp/api/windows.ui.xaml.controls.page) クラスから派生した基本的なページが含まれています。 |
| 単体テスト アプリ (ユニバーサル Windows) | UWP アプリ用の C# 単体テスト プロジェクトを作成します。 詳細については、「[C# コードの単体テスト](/visualstudio/test/unit-testing-visual-csharp-code-in-a-store-app)」を参照してください。 |

これらのプロジェクト テンプレートを使用して C# UWP アプリの部分をビルドできます。

| テンプレート | 説明 |
|----------|-------------|
| クラス ライブラリ (ユニバーサル Windows) | マネージド コードで記述された他の UWP アプリが使用できる C# のマネージド クラス ライブラリ (DLL) を作成します。 |
| Windows ランタイム コンポーネント (ユニバーサル Windows) | アプリが記述されたプログラミング言語に関係なく、すべての UWP アプリが使用できる C# の [Windows ランタイム コンポーネント](/windows/uwp/winrt-components/)を作成します。 |
| 省略可能なコード パッケージ (ユニバーサル Windows) | アプリによって読み込むことができる実行可能 C# コードを含む、省略可能なパッケージを作成します。 詳細については、[実行可能コードを含む省略可能なパッケージ](/windows/msix/package/optional-packages-with-executable-code)に関するページを参照してください。  |

### <a name="project-templates-for-c-and-uwp"></a>C++ および UWP のプロジェクト テンプレート

C++ UWP アプリのビルドに使用できるテクノロジは 2 種類あります。

* 推奨されるテクノロジは [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/) です。 これは C++ 言語プロジェクションで、ヘッダー ファイルに全面的に実装され、最新の WinRT API への最上位アクセス権を提供するように設計されています。
* または、古い [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) の拡張セットを使用することもできます。 C++/CX は引き続きサポートされますが、代わりに C++/WinRT を使用することをお勧めします。

Visual Studio で新しいプロジェクトを作成するときに UWP C++ プロジェクト テンプレートを利用するには、言語を **[C++]** 、プラットフォームを **[Windows]** 、プロジェクトの種類を **[UWP]** としてフィルター処理します。 

> [!NOTE]
> 既定では、Visual Studio の**ユニバーサル Windows プラットフォーム開発**ワークロードで利用できるのは C++/CX プロジェクト テンプレートのみです。 C++/WinRT プロジェクト テンプレートを利用するには、[C++/WinRT VSIX](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package) パッケージをインストールする必要があります。

![UWP C++ プロジェクト テンプレート](images/uwp-projects-cpp.png)

これらのプロジェクト テンプレートを使用して C++ UWP アプリを作成できます。

| テンプレート | 説明 |
|----------|-------------|
| 空のアプリ (C++/WinRT) | XAML ユーザー インターフェイスを備えた C++/WinRT UWP アプリを作成します。 生成されるプロジェクトには、UI のビルドを開始するために使用できる [Windows.UI.Xaml.Controls.Page](/uwp/api/windows.ui.xaml.controls.page) クラスから派生した基本的なページが含まれています。 |
| コア アプリ (C++/WinRT) | [CoreApplication](/uwp/api/Windows.ApplicationModel.Core.CoreApplication) を使用して、XAML ユーザー インターフェイスではなくさまざまな UI フレームワークと統合する、C++/WinRT UWP アプリを作成します。 このプロジェクト テンプレートを使用した、DirectX を使用する単純なゲームの作成方法を説明するチュートリアルは、[DirectX を使用した単純な UWP ゲームの作成](/windows/uwp/gaming/tutorial--create-your-first-uwp-directx-game)に関するページを参照してください。 |
| 空のアプリ (ユニバーサル Windows - C++/CX) | XAML ユーザー インターフェイスを備えた C++/WinRT UWP アプリを作成します。 生成されるプロジェクトには、UI のビルドを開始するために使用できる、WinUI ライブラリの [Windows.UI.Xaml.Controls.Page](/uwp/api/windows.ui.xaml.controls.page) クラスから派生した基本的なページが含まれています。 |
| DirectX 11 および XAML アプリ (ユニバーサル Windows - C++/CX) | XAML UI コントロールを使用できるように DirectX 11 と **SwapChainPanel** を使用する UWP アプリを作成します。 詳細については、「[DirectX ゲーム プロジェクト テンプレート](/windows/uwp/gaming/user-interface)」を参照してください。 |
| DirectX 11 アプリ (ユニバーサル Windows - C++/CX) | DirectX 11 を使用する UWP アプリを作成します。 詳細については、「[DirectX ゲーム プロジェクト テンプレート](/windows/uwp/gaming/user-interface)」を参照してください。 |
| DirectX 12 アプリ (ユニバーサル Windows - C++/CX) | DirectX 12 を使用する UWP アプリを作成します。 詳細については、「[DirectX ゲーム プロジェクト テンプレート](/windows/uwp/gaming/user-interface)」を参照してください。 |
| 単体テスト アプリ (ユニバーサル Windows - C++/CX) | UWP アプリ用の C++ 単体テスト プロジェクトを作成します。 詳細については、「[C++ UWP DLL をテストする方法](/visualstudio/test/unit-testing-a-visual-cpp-dll-for-store-apps)」を参照してください。 |

これらのプロジェクト テンプレートを使用して C++ UWP アプリの部分をビルドできます。

| テンプレート | 説明 |
|----------|-------------|
| Windows ランタイム コンポーネント (C++/WinRT) | アプリが記述されたプログラミング言語に関係なく、すべての UWP アプリが使用できる C++/WinRT の [Windows ランタイム コンポーネント](/windows/uwp/winrt-components/)を作成します。 |
| Windows ランタイム コンポーネント (ユニバーサル Windows) | アプリが記述されたプログラミング言語に関係なく、すべての UWP アプリが使用できる C++/CX の [Windows ランタイム コンポーネント](/windows/uwp/winrt-components/)を作成します。 |
| DLL (ユニバーサル Windows) | UWP アプリで使用できる、C++/CX のダイナミックリンク ライブラリ (DLL) を作成するためのプロジェクト。 詳細については、「[DLL (C++/CX)](/cpp/cppcx/dlls-c-cx)」を参照してください。 |
| スタティック ライブラリ (ユニバーサル Windows) | UWP アプリで使用できる、C++/CX のスタティック ライブラリ (LIB) を作成するためのプロジェクト。 詳細については、「[スタティック ライブラリ (C++/CX)](/cpp/cppcx/static-libraries-c-cx)」を参照してください。 |

## <a name="cwin32-templates"></a>C++/Win32 テンプレート

Visual Studio では、ネイティブの C++ で Win32 API に直接アクセスできるデスクトップ Windows アプリをビルドするためのさまざまなプロジェクト テンプレートが提供されます。 これらのプロジェクト テンプレートを使用するには、Visual Studio をインストールするときに、**C++ によるデスクトップ開発**ワークロードを含める必要があります。 このワークロードには、デスクトップ アプリ、コンソール アプリ、およびライブラリをビルドするためのプロジェクト テンプレートが含まれています。

### <a name="project-templates-for-desktop-apps"></a>デスクトップ アプリのプロジェクト テンプレート

Visual Studio で新しいプロジェクトを作成するときに、従来のデスクトップ アプリ用の C++ プロジェクト テンプレートを利用するには、言語を **[C++]** 、プラットフォームを **[Windows]** 、プロジェクトの種類を **[デスクトップ]** としてフィルター処理します。

![ネイティブ C++ アプリ プロジェクト テンプレート](images/desktop-app-projects-cpp.png)

| テンプレート | 説明 |
|----------|----------|
| Windows デスクトップ アプリケーション | C++ を使用した従来の Windows デスクトップ アプリを作成します。 詳しくは、「[チュートリアル: 従来の Windows デスクトップ アプリケーションの作成](/cpp/windows/walkthrough-creating-windows-desktop-applications-cpp)」を参照してください。 |
| Windows デスクトップ ウィザード | 従来の Windows デスクトップ アプリ、コンソール アプリ、ダイナミックリンク ライブラリ (DLL)、スタティック ライブラリのいずれかのプロジェクトを作成するために使用できる、ステップバイステップのウィザードを提供します。 詳細については、「[Windows デスクトップ ウィザード](/cpp/windows/windows-desktop-wizard)」および「[チュートリアル:従来の Windows デスクトップ アプリケーションの作成](/cpp/windows/walkthrough-creating-windows-desktop-applications-cpp)」を参照してください。         |
| Windows アプリケーション パッケージ プロジェクト | [MSIX パッケージ](/windows/msix/overview)にデスクトップ アプリをビルドするために使用できるプロジェクトを作成します。 これは、最新のデプロイ エクスペリエンスをもたらし、パッケージ拡張機能を使用して Windows 10 の機能と統合できるなど、さまざまなメリットがあります。 詳細については、[Windows アプリケーション パッケージ プロジェクト](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)に関するページを参照してください。  |

### <a name="project-templates-for-console-apps"></a>コンソール アプリのプロジェクト テンプレート

コンソール アプリの C++ プロジェクト テンプレートを利用するには、言語を **[C++]** 、プラットフォームを **[Windows]** 、プロジェクトの種類を **[コンソール]** としてフィルター処理します。

![ネイティブ C++ コンソール プロジェクト テンプレート](images/desktop-console-projects-cpp.png)

| テンプレート | 説明 |
|----------|----------|
| Windows コンソール アプリケーション (C++/WinRT) | ユーザー インターフェイスがない [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/) コンソール アプリを作成します。 詳細については、「[C++/WinRT のクイックスタート](/windows/uwp/cpp-and-winrt-apis/get-started#a-cwinrt-quick-start)」を参照してください。 このプロジェクト テンプレートには、[C++/WinRT VSIX](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package) が必要です。  |
| コンソール アプリ | ユーザー インターフェイスがないコンソール アプリを作成します。 詳しくは、「[チュートリアル: 標準 C++ プログラムの作成](/cpp/windows/walkthrough-creating-a-standard-cpp-program-cpp)」を参照してください。 |
| 空のプロジェクト | アプリケーション、ライブラリ、または DLL を作成するための空のプロジェクト。 必要なコードまたはリソースを追加する必要があります。 |

### <a name="project-templates-for-libraries"></a>ライブラリのプロジェクト テンプレート

ライブラリの C++ プロジェクト テンプレートを利用するには、言語を **[C++]** 、プラットフォームを **[Windows]** 、プロジェクトの種類を **[ライブラリ]** としてフィルター処理します。

![ネイティブ C++ ライブラリ プロジェクト テンプレート](images/desktop-library-projects-cpp.png)

| テンプレート | 説明 |
|----------|----------|
| ダイナミックリンク ライブラリ (DLL) | ダイナミックリンク ライブラリ (DLL) を作成するためのプロジェクト。 詳しくは、「[チュートリアル: ダイナミックリンク ライブラリの作成と使用](/cpp/build/walkthrough-creating-and-using-a-dynamic-link-library-cpp)」を参照してください。 |
| スタティック ライブラリ | スタティック ライブラリ (LIB) を作成するためのプロジェクト。 詳しくは、「[チュートリアル: スタティック ライブラリの作成と使用](/cpp/build/walkthrough-creating-and-using-a-static-library-cpp)」を参照してください。 |

### <a name="item-templates-for-native-c-and-win32"></a>ネイティブ C++ および Win32 の項目テンプレート

C++ プロジェクト テンプレートには、プロジェクトへの新しいファイルやリソースの追加といったタスクを実行するために使用できる多数の項目テンプレートが含まれています。 包括的な一覧は、「[Visual C++ の新しい項目の追加テンプレートの使用方法](/cpp/build/reference/using-visual-cpp-add-new-item-templates)」を参照してください。

## <a name="net-templates"></a>.NET テンプレート

Visual Studio では、.NET と C# を使用するデスクトップ Windows アプリをビルドするためのさまざまなプロジェクト テンプレートが提供されます。 これらのプロジェクト テンプレートを使用するには、Visual Studio をインストールするときに、 **.NET デスクトップ開発**ワークロードを含める必要があります。

Visual Studio で新しいプロジェクトを作成するときにデスクトップ C# プロジェクト テンプレートを利用するには、言語を **[C#]** 、プラットフォームを **[Windows]** 、プロジェクトの種類を **[デスクトップ]** としてフィルター処理します。

![.NET C# プロジェクト テンプレート](images/dotnet-projects-csharp.png)

これらのプロジェクト テンプレートを使用して、C# と .NET を使用するアプリを作成できます。

| テンプレート | 説明 |
|----------|----------|
| WPF アプリ (.NET Core) | [.NET Core](/dotnet/core/) を対象とする [WPF](/dotnet/framework/wpf/) アプリを作成します。 このプロジェクト テンプレートのチュートリアルは、[WPF アプリケーションの作成](/visualstudio/get-started/csharp/tutorial-wpf)に関するページを参照してください。 |
| WPF アプリ (.NET Framework) | [.NET Framework](/dotnet/framework/) を対象とする [WPF](/dotnet/framework/wpf/) アプリを作成します。 このプロジェクト テンプレートのチュートリアルは、[初めての WPF アプリケーションを作成するチュートリアル](/dotnet/framework/wpf/getting-started/walkthrough-my-first-wpf-desktop-application)のページを参照してください。 |
| Windows フォーム アプリ (.NET Core) | [.NET Core](/dotnet/core/) を対象とする [Windows フォーム](/dotnet/framework/winforms/) アプリを作成します。  |
| Windows フォーム アプリ (.NET Framework) | [.NET Framework](/dotnet/framework/) を対象とする [Windows フォーム](/dotnet/framework/winforms/) アプリを作成します。 このプロジェクト テンプレートのチュートリアルについては、「[Visual Studio で C# を使用して Windows フォーム アプリを作成する](/visualstudio/ide/create-csharp-winform-visual-studio)」を参照してください。 |
| Windows アプリケーション パッケージ プロジェクト | [MSIX パッケージ](/windows/msix/overview)に WPF または Windows フォーム アプリをビルドするために使用できるプロジェクトを作成します。 これは、最新のデプロイ エクスペリエンスをもたらし、パッケージ拡張機能を使用して Windows 10 の機能と統合できるなど、さまざまなメリットがあります。 詳細については、[Windows アプリケーション パッケージ プロジェクト](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)に関するページを参照してください。 |