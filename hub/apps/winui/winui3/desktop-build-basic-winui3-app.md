---
description: WinUI 3 UI を使用して .NET および C++/Win32 デスクトップ アプリをビルドします。
title: デスクトップ用の基本的な WinUI 3 アプリを構築する
ms.date: 03/19/2021
ms.topic: article
keywords: windows 10、uwp、windows フォーム、wpf、xaml islands
ms.author: kbridge
author: Karl-Bridge-Microsoft
ms.localizationpriority: high
ms.custom: 19H1
ms.openlocfilehash: e7453f620b7c82c46a44e293a36a9eb51bfb36ae
ms.sourcegitcommit: 7f2a09e8d5d37cb5860a5f2ece5351ea6907b94c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/29/2021
ms.locfileid: "105730746"
---
# <a name="build-a-basic-winui-3---project-reunion-05-desktop-app"></a>基本的な WinUI 3 - Project Reunion 0.5 デスクトップ アプリをビルドする

この記事では、基本的なマネージド C#/.NET 5 およびネイティブ C++/Win32 WinUI 3 - Project Reunion 0.5 デスクトップ アプリケーションを、WinUI 3 のコントロールと機能のみで構成されたユーザー インターフェイスを使用してビルドする方法について説明します。

## <a name="prerequisites"></a>前提条件

1. 開発環境を設定する。 「[Project Reunion の概要](../../project-reunion/get-started-with-project-reunion.md)」を参照してください。
1. C# および .NET 5 プロジェクト用の最初の WinUI 3 デスクトップ アプリを作成して、構成をテストする。 「[デスクトップ アプリ用の WinUI 3 の概要](get-started-winui3-for-desktop.md)」を参照してください。

:::image type="content" source="images/build-basic/template-app.png" alt-text="実行中の初期テンプレート アプリ。":::<br/>
*実行中の初期テンプレート アプリ。*

## <a name="initial-template-app"></a>初期テンプレート アプリ

初期テンプレート アプリケーションからビルドを開始しますが、その前に、テンプレート アプリの構造とコンテンツについて説明します。

### <a name="the-application-solution"></a>アプリケーション ソリューション

既定で、このソリューションには 2 つのプロジェクトが含まれています。アプリケーション自体と、[MSIX](/windows/msix) アプリ パッケージを作成するためのプロジェクトです。

> [!NOTE]
> 現在、MSIX アプリ パッケージは、Project Reunion 0.5 を使用するアプリを他のコンピューターに展開するために必要です。 ただし、Project Reunion の将来のリリースで、パッケージ化されていないアプリの展開がサポートされる予定です。

:::image type="content" source="images/build-basic/template-app-solution-explorer.png" alt-text="初期テンプレート アプリのファイル構造が表示されたソリューション エクスプローラー。":::<br/>
*初期テンプレート アプリのファイル構造が表示されたソリューション エクスプローラー。*

### <a name="the-application-project"></a>アプリケーション プロジェクト

ここで、アプリケーション プロジェクト ファイルをダブルクリックします (または、右クリックして [プロジェクト ファイルの編集] を選択します)。これにより、ファイルが XML テキスト エディターで開かれます。

初期テンプレート アプリのプロジェクト ファイルを次に示します。これには、注目すべき次の 2 つの項目が含まれています。

- `TargetFramework` は、今後の .NET で主に実装される [.NET 5](/dotnet/core/dotnet-five) です。
- WinUI を含むさまざまな Project Reunion 機能のための NuGet `PackageReference` 要素。

```xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>net5.0-windows10.0.19041.0</TargetFramework>
    <TargetPlatformMinVersion>10.0.17763.0</TargetPlatformMinVersion>
    <RootNamespace>WinUIApp2</RootNamespace>
    <Platforms>x86;x64;arm64</Platforms>
    <RuntimeIdentifiers>win10-x86;win10-x64;win10-arm64</RuntimeIdentifiers>
    <NoWin32Manifest>true</NoWin32Manifest>
    <ApplicationIcon />
    <StartupObject />
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.ProjectReunion" Version="0.5.0-prerelease" />
    <PackageReference Include="Microsoft.ProjectReunion.Foundation" Version="0.5.0-prerelease" />
    <PackageReference Include="Microsoft.ProjectReunion.WinUI" Version="0.5.0-prerelease" />
    <Manifest Include="$(ApplicationManifest)" />
  </ItemGroup>
</Project>
```

### <a name="the-mainwindowxaml-file"></a>MainWindow.xaml ファイル

WinUI 3 では、XAML マークアップで [Window](/windows/winui/api/microsoft.ui.xaml.window) クラスのインスタンスを作成できるようになりました。

XAML の Window クラスは、デスクトップ ウィンドウをサポートするために拡張され、UWP およびデスクトップ アプリ モデルで使用される低レベル ウィンドウの各実装を抽象化したものになりました。 具体的には、UWP では CoreWindow が使用されるのに対し、Win32 ではウィンドウ ハンドル (または HWND) および対応する Win32 API が使用されます。

次のコード例は初期テンプレート アプリの MainWindow.xaml ファイルで、アプリのルート要素として Window クラスを使用します。

```xaml
<Window
    x:Class="WinUIApp2.MainWindow"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:WinUIApp2"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">

    <StackPanel Orientation="Horizontal" HorizontalAlignment="Center" VerticalAlignment="Center">
        <Button x:Name="myButton" Click="MyButton_Click">Click Me</Button>
    </StackPanel>
</Window>
```

## <a name="basic-managed-cnet-5-app"></a>基本的なマネージド C#/.NET 5 アプリ

この最初の例では、User32.dll の [ShowWindow](/windows/win32/api/winuser/nf-winuser-showwindow) API を使用して、プログラムでウィンドウを最大化します。

1. まず、User32.dll から Win32 API を呼び出し、**PInvoke.User32** NuGet パッケージをプロジェクトに追加します (Visual Studio のメニューから、 **[ツール]、[NuGet パッケージ マネージャー]、[ソリューションの NuGet パッケージの管理...]** の順に選択)。詳細については、「[マネージド コードからのネイティブ関数の呼び出し](/cpp/dotnet/calling-native-functions-from-managed-code)」を参照してください。
1. パッケージを追加したら、MainWindow.xaml.cs 分離コード ファイルを開き、`MyButton_Click` イベント ハンドラーを次のコードに置き換えます。

    ```csharp
            private void MyButton_Click(object sender, RoutedEventArgs e)
            {
                myButton.Content = "Clicked";
    
                IntPtr hwnd = (App.Current as App).MainWindowWindowHandle;
                PInvoke.User32.ShowWindow(hwnd, PInvoke.User32.WindowShowStyle.SW_MAXIMIZE);
            }
    ```

    [PInvoke](/dotnet/standard/native-interop/pinvoke) [ShowWindow](/windows/win32/api/winuser/nf-winuser-showwindow) メソッドは、ウィンドウ ハンドル (`hwnd`) を最初のパラメーターとして使用し、2 番目のパラメーターを使用して、ウィンドウを最大化する必要があることを指定します。 

1. ウィンドウ ハンドルを取得するには、[GetActiveWindow](/windows/win32/api/winuser/nf-winuser-getactivewindow) メソッドを使用します。 このメソッドは、現在作業中のウィンドウのウィンドウ ハンドルを返します (ターゲット ウィンドウをアクティブにした後にこのメソッドを呼び出します)。

    `MainWindow` オブジェクト (MainWindow.xaml を参照) の作成、インスタンス化、アクティブ化が、App.xaml.cs 分離コード ファイルにある `OnLaunched` イベント ハンドラーで行われます。 `OnLaunched` イベントを次のコードに置き換えます。

    ```csharp
        protected override void OnLaunched(Microsoft.UI.Xaml.LaunchActivatedEventArgs args)
        {
            m_window = new MainWindow();
            m_window.Activate();
            m_windowhandle = PInvoke.User32.GetActiveWindow();
        }

        IntPtr m_windowhandle;
        public IntPtr MainWindowWindowHandle { get { return m_windowhandle; } }
    ```

1. アプリをコンパイルして実行します。
1. 最後に、[Click me] ボタンを押すと、アプリ ウィンドウが最大化されます。

## <a name="full-trust-desktop-app"></a>"完全信頼" デスクトップ アプリ

WinUI 3 には、[AppContainer](/windows/win32/secauthz/appcontainer-for-legacy-applications-) のセキュリティ サンドボックスの外部で、アプリケーションを "完全な信頼のアクセス許可" で実行する機能が用意されています。

マネージド C#/.NET 5 およびネイティブ C++/Win32 WinUI 3 - Project Reunion 0.5 デスクトップ アプリケーションは、制限なく .NET 5 API を呼び出すことができます。 次の例 (前の例で使用した初期テンプレート アプリから派生) で、現在のプロセスに対してクエリを実行し、読み込まれたすべてのモジュールの一覧を取得する方法を説明します (UWP アプリでは使用できません)。

1. MainWindow.xaml ファイルで、[ContentDialog](/windows/winui/api/microsoft.ui.xaml.controls.contentdialog) 要素を追加します。

    ```xaml
    <StackPanel 
        Orientation="Horizontal" 
        HorizontalAlignment="Center" 
        VerticalAlignment="Center">
        <Button x:Name="myButton" Click="MyButton_Click">Click Me</Button>
        <ContentDialog x:Name="contentDialog" CloseButtonText="Close">
            <StackPanel>
                <TextBlock Name="cdTextBlock"/>
            </StackPanel>
        </ContentDialog>
    </StackPanel>
    ```

1. MainWindow.xaml.cs 分離コード ファイルで、[System.Diagnostics](/dotnet/api/system.diagnostics) から .NET API を呼び出し、[現在のプロセス](/dotnet/api/system.diagnostics.process.getcurrentprocess)に読み込まれているモジュールを取得します。 この例では、[Process.Modules](/dotnet/api/system.diagnostics.process.modules) オブジェクトに含まれる各 [ProcessModule](/dotnet/api/system.diagnostics.processmodule) を反復処理しているだけです。

    ```csharp
    private async void MyButton_Click(object sender, RoutedEventArgs e)
    {
        myButton.Content = "Clicked";

        var description = new System.Text.StringBuilder();
        var process = System.Diagnostics.Process.GetCurrentProcess();
        foreach (System.Diagnostics.ProcessModule module in process.Modules)
        {
            description.AppendLine(module.FileName);
        }

        cdTextBlock.Text = description.ToString();
        await contentDialog.ShowAsync();
    }
    ```
1. アプリをコンパイルして実行します。
1. 最後に、[Click me] ボタンを押すと、プロセスの一覧を示すダイアログが表示されます。

    :::image type="content" source="images/build-basic/template-app-full-trust.png" alt-text="完全信頼アプリケーションの例。":::<br/>*完全信頼アプリケーションの例。*

## <a name="summary"></a>まとめ

このトピックでは、基になるウィンドウ実装 (この例では Win32 と HWND) へのアクセス方法、および Windows 10 の WinRT API と共にアプリで Win32 API を使用する方法について説明しました。 これで、新しい WinUI 3 デスクトップ アプリを作成するときに、既存のデスクトップ アプリケーション コードの多くを使用できます。

WinUI 3 で .NET 5 API を UI フレームワークとして使用する方法についても説明しました。

## <a name="see-also"></a>関連項目

- [Windows UI ライブラリ 3 - Project Reunion 0.5 (2021 年 3 月)](index.md)
- [Project Reunion の概要](../../project-reunion/get-started-with-project-reunion.md)
- [デスクトップ アプリ用の WinUI 3 の概要](get-started-winui3-for-desktop.md)
