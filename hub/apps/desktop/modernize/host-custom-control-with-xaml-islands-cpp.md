---
description: この記事では、XAML ホスティング API を使用して C++ Win32 アプリでカスタム UWP コントロールをホストする方法を示します。
title: XAML ホスティング API を使用して C++ Win32 アプリでカスタム UWP コントロールをホストする
ms.date: 04/07/2020
ms.topic: article
keywords: Windows 10, UWP, C++, Win32, XAML Islands, カスタム コントロール, ユーザー コントロール, コントロールのホスト
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: d61abe8b59f916ed56c1fefe0bda4b9f25b673a4
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173726"
---
# <a name="host-a-custom-uwp-control-in-a-c-win32-app"></a>C++ Win32 アプリでカスタム UWP コントロールをホストする

この記事では、[UWP XAML ホスティング API](using-the-xaml-hosting-api.md) を使用して、新しい C++ Win32 アプリでカスタム UWP XAML コントロールをホストする方法について説明します。 C++ Win32 アプリ プロジェクトが既にある場合は、以下の手順とコード例をプロジェクトに合わせて調整してかまいません。

カスタム UWP XAML コントロールをホストするため、このチュートリアルの一部として次のプロジェクトとコンポーネントを作成します。

* **Windows デスクトップ アプリケーション プロジェクト**。 このプロジェクトでは、C++ Win32 のネイティブ デスクトップ アプリを実装します。 UWP XAML ホスティング API を使用してカスタム UWP XAML コントロールをホストするコードを、このプロジェクトに追加します。

* **UWP アプリ プロジェクト (C++/WinRT)** 。 このプロジェクトでは、カスタム UWP XAML コントロールを実装します。 また、カスタム UWP XAML 型のメタデータをプロジェクトに読み込むためのルート メタデータ プロバイダーも実装します。

## <a name="requirements"></a>要件

* Visual Studio 2019 バージョン 16.4.3 以降。
* Windows 10 バージョン 1903 SDK (バージョン 10.0.18362) 以降。
* Visual Studio と共にインストールされる [C++/WinRT Visual Studio Extension (VSIX)](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264)。 C++/WinRT は Windows ランタイム (WinRT) API の標準的な最新の C++17 言語プロジェクションで、ヘッダー ファイル ベースのライブラリとして実装され、最新の Windows API への最上位アクセス権を提供するように設計されています。 詳しくは、「[C++/WinRT](/windows/uwp/cpp-and-winrt-apis/)」をご覧ください。

## <a name="create-a-desktop-application-project"></a>デスクトップ アプリケーション プロジェクトを作成する

1. Visual Studio で、**MyDesktopWin32App** という名前の新しい **Windows デスクトップ アプリケーション** プロジェクトを作成します。 このプロジェクト テンプレートは、**C++** 、**Windows**、および**デスクトップ**のプロジェクト フィルターで使用できます。

2. **ソリューション エクスプローラー**でソリューション ノードを右クリックして、 **[ソリューションの再ターゲット]** をクリックし、**10.0.18362.0** 以降の SDK リリースを選択してから、 **[OK]** をクリックします。

3. [Microsoft.Windows.CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) NuGet パッケージをインストールして、プロジェクトでの [C++/WinRT](/windows/uwp/cpp-and-winrt-apis) のサポートを有効にします。

    1. **ソリューション エクスプローラー**で **MyDesktopWin32App** プロジェクトを右クリックし、 **[NuGet パッケージの管理]** を選択します。
    2. **[参照]** タブを選択し、[Microsoft.Windows.CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) パッケージを探して、このパッケージの最新バージョンをインストールします。

4. **[NuGet パッケージの管理]** ウィンドウで、次の追加の NuGet パッケージをインストールします。

    * [Microsoft.Toolkit.Win32.UI.SDK](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.SDK) (バージョン v6.0.0 以降)。 このパッケージでは、XAML Islands がアプリで動作できるようにする、いくつかのビルド資産と実行時資産が提供されています。
    * [Microsoft.Toolkit.Win32.UI.XamlApplication](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication) (バージョン v6.0.0 以降)。 このパッケージは、このチュートリアルの後半で使用する [Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication) クラスを定義します。
    * [Microsoft.VCRTForwarders.140](https://www.nuget.org/packages/Microsoft.VCRTForwarders.140)。

5. ソリューションをビルドし、正常にビルドされたことを確認します。

## <a name="create-a-uwp-app-project"></a>UWP アプリ プロジェクトを作成する

次に、**UWP (C++/WinRT)** アプリ プロジェクトをソリューションに追加し、このプロジェクトの構成にいくつかの変更を行います。 このチュートリアルの後半では、カスタム UWP XAML コントロールを実装するためのコードをこのプロジェクトに追加し、[Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication) クラスのインスタンスを定義します。 

1. **ソリューション エクスプローラー**で、ソリューション ノードを右クリックし、 **[追加]**  ->  **[新しいプロジェクト]** を選択します。

2. ソリューションに**空のアプリ (C++/WinRT)** プロジェクトを追加します。 プロジェクトの名前を **MyUWPApp** にして、対象バージョンと最小バージョンの両方が **Windows 10 バージョン 1903** 以降に設定されていることを確認します。

3. **MyUWPApp** プロジェクトに、[Microsoft.Toolkit.Win32.UI.XamlApplication](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication) NuGet パッケージをインストールします。 このパッケージは、このチュートリアルの後半で使用する [Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication) クラスを定義します。

    1. **MyUWPApp** プロジェクトを右クリックし、 **[NuGet パッケージの管理]** を選択します。
    2. **[参照]** タブを選択し、[Microsoft.Toolkit.Win32.UI.XamlApplication](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication) パッケージを見つけて、このパッケージの 6.0.0 以降のバージョンをインストールします。

4. **MyUWPApp** ノードを右クリックし、 **[プロパティ]** を選択します。 **[共通プロパティ]**  ->  **[C++/WinRT]** ページで、 **[詳細]** プロパティを **[通常]** に設定して、 **[適用]** をクリックします。 完了すると、プロパティ ページは次のようになります。

    ![C++/WinRT プロジェクトのプロパティ](images/xaml-islands/xaml-island-cpp-1.png)

5. プロパティ ウィンドウの **[構成プロパティ]**  ->  **[全般]** ページで、 **[構成の種類]** を **[ダイナミック ライブラリ (.dll)]** に設定し、 **[OK]** をクリックしてプロパティ ウィンドウを閉じます。

    ![プロジェクトの全般プロパティ](images/xaml-islands/xaml-island-cpp-2.png)

6. **MyUWPApp** プロジェクトに、プレースホルダー実行可能ファイルを追加します。 このプレースホルダー実行可能ファイルは、Visual Studio で必要なプロジェクト ファイルが生成され、プロジェクトが正しくビルドされるために必要です。

    1. **ソリューション エクスプローラー**で、**MyUWPApp** プロジェクト ノードを右クリックし、 **[追加]**  ->  **[新しい項目]** を選択します。
    2. **[新しい項目の追加]** ダイアログで、左側のページにある **[ユーティリティ]** を選択し、 **[テキスト ファイル (.txt)]** を選択します。 名前に「**placeholder.exe**」と入力し、 **[追加]** をクリックします。
      ![テキスト ファイルを追加する](images/xaml-islands/xaml-island-cpp-3.png)
    3. **ソリューション エクスプローラー**で、**placeholder.exe** ファイルを選択します。 **[プロパティ]** ウィンドウで、 **[コンテンツ]** プロパティが **[True]** に設定されていることを確認します。
    4. **ソリューション エクスプローラー**で、**MyUWPApp** プロジェクトの **Package.appxmanifest** ファイルを右クリックし、 **[プログラムから開く]** を選択し、 **[XML (テキスト) エディター]** を選択して、 **[OK]** をクリックします。
    5. **&lt;Application&gt;** 要素を見つけて、**Executable** 属性の値を `placeholder.exe` に変更します。 完了すると、 **&lt;Application&gt;** 要素は次のようになります。

        ```xml
        <Application Id="App" Executable="placeholder.exe" EntryPoint="MyUWPApp.App">
          <uap:VisualElements DisplayName="MyUWPApp" Description="Project for a single page C++/WinRT Universal Windows Platform (UWP) app with no predefined layout"
            Square150x150Logo="Assets\Square150x150Logo.png" Square44x44Logo="Assets\Square44x44Logo.png" BackgroundColor="transparent">
            <uap:DefaultTile Wide310x150Logo="Assets\Wide310x150Logo.png">
            </uap:DefaultTile>
            <uap:SplashScreen Image="Assets\SplashScreen.png" />
          </uap:VisualElements>
        </Application>
        ```

    6. **Package.appxmanifest** ファイルを保存して閉じます。

7. **ソリューション エクスプローラー**で、**MyUWPApp** ノードを右クリックして、 **[プロジェクトのアンロード]** を選択します。
8. **MyUWPApp** ノードを右クリックして、 **[Edit MyUWPApp.vcxproj]\(MyUWPApp.vcxproj の編集\)** を選択します。
9. `<Import Project="$(VCTargetsPath)\Microsoft.Cpp.Default.props" />` 要素を見つけて、次の XML に置き換えます。 この XML では、要素の直前に新しいプロパティがいくつか追加されます。

    ```xml
    <PropertyGroup Label="Globals">
        <WindowsAppContainer>true</WindowsAppContainer>
        <AppxGeneratePriEnabled>true</AppxGeneratePriEnabled>
        <ProjectPriIndexName>App</ProjectPriIndexName>
        <AppxPackage>true</AppxPackage>
    </PropertyGroup>
    <Import Project="$(VCTargetsPath)\Microsoft.Cpp.Default.props" />
    ```

10. プロジェクト ファイルを保存して閉じます。
11. **ソリューション エクスプローラー**で、**MyUWPApp** ノードを右クリックして、 **[プロジェクトの再読み込み]** を選択します。

## <a name="configure-the-solution"></a>ソリューションを構成する

このセクションでは、両方のプロジェクトが含まれるソリューションを更新して、プロジェクトを正しくビルドするために必要なプロジェクトの依存関係とビルドのプロパティを構成します。

1. **ソリューション エクスプローラー**でソリューション ノードを右クリックし、**Solution.props** という名前の新しい XML ファイルを追加します。
2. 次の XML を **Solution.props** ファイルに追加します。

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <Project xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
      <PropertyGroup>
        <IntDir>$(SolutionDir)\obj\$(Platform)\$(Configuration)\$(MSBuildProjectName)\</IntDir>
        <OutDir>$(SolutionDir)\bin\$(Platform)\$(Configuration)\$(MSBuildProjectName)\</OutDir>
        <GeneratedFilesDir>$(IntDir)Generated Files\</GeneratedFilesDir>
      </PropertyGroup>
    </Project>
    ```

3. **[表示]** メニューの **[プロパティ マネージャー]** をクリックします (構成によっては **[表示]**  ->  **[その他のウィンドウ]** の下にある場合があります)。
4. **[プロパティ マネージャー]** ウィンドウで **MyDesktopWin32App** を右クリックし、 **[既存のプロパティ シートの追加]** を選択します。 先ほど追加した **Solution.props** ファイルに移動し、 **[開く]** をクリックします。
5. 前の手順を繰り返して、 **[プロパティ マネージャー]** ウィンドウで **MyUWPApp** プロジェクトに **Solution.props** ファイルを追加します。
6. **[プロパティ マネージャー]** ウィンドウを閉じます。
7. プロパティ シートの変更が正常に保存されたことを確認します。 **ソリューション エクスプローラー**で **MyDesktopWin32App** プロジェクトを右クリックし、 **[プロパティ]** を選択します。 **[構成プロパティ]**  ->  **[全般]** をクリックし、 **[出力ディレクトリ]** プロパティと **[中間ディレクトリ]** プロパティが、**Solution.props** ファイルに追加した値に設定されていることを確認します。 **MyUWPApp** プロジェクトでも同じことを確認できます。
    ![プロジェクトのプロパティ](images/xaml-islands/xaml-island-cpp-4.png)

8. **ソリューション エクスプローラー**でソリューション ノードを右クリックし、 **[プロジェクトの依存関係]** を選択します。 **[プロジェクト]** ドロップダウンで **MyDesktopWin32App** が選択されていることを確認し、 **[依存先]** の一覧で **MyUWPApp** を選択します。
    ![プロジェクトの依存関係](images/xaml-islands/xaml-island-cpp-5.png)

9. **[OK]** をクリックします。

## <a name="add-code-to-the-uwp-app-project"></a>UWP アプリ プロジェクトにコードを追加する

これで、**MyUWPApp** プロジェクトにコードを追加し、次のタスクを実行できるようになりました。

* カスタム UWP XAML コントロールを実装します。 このチュートリアルでこの後、**MyDesktopWin32App** プロジェクトでこのコントロールをホストするコードを追加します。
* Windows Community Toolkit の [Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication) クラスから派生する型を定義します。

### <a name="define-a-custom-uwp-xaml-control"></a>カスタム UWP XAML コントロールを定義する

1. **ソリューション エクスプローラー**で **MyUWPApp** を右クリックし、 **[追加]**  ->  **[新しい項目]** を選択します。 左側のペインで **[Visual C++]** ノードを選択して、 **[Blank User Control (C++/WinRT)]\(空のユーザー コントロール (C++/WinRT)\)** を選択し、**MyUserControl** という名前を付けて、 **[追加]** をクリックします。
2. XAML エディターで、**MyUserControl.xaml** ファイルの内容を次の XAML に置き換えて、ファイルを保存します。

    ```xml
    <UserControl
        x:Class="MyUWPApp.MyUserControl"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="using:MyUWPApp"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        mc:Ignorable="d">

        <StackPanel HorizontalAlignment="Center" Spacing="10" 
                    Padding="20" VerticalAlignment="Center">
            <TextBlock HorizontalAlignment="Center" TextWrapping="Wrap" 
                           Text="Hello from XAML Islands" FontSize="30" />
            <TextBlock HorizontalAlignment="Center" Margin="15" TextWrapping="Wrap"
                           Text="😍❤💋🌹🎉😎�🐱‍👤" FontSize="16" />
            <Button HorizontalAlignment="Center" 
                    x:Name="Button" Click="ClickHandler">Click Me</Button>
        </StackPanel>
    </UserControl>
    ```

### <a name="define-a-xamlapplication-class"></a>XamlApplication クラスを定義する

次に、**MyUWPApp** プロジェクトの既定の **App** クラスを、Windows Community Toolkit によって提供される [Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication) クラスから派生するように変更します。 このクラスは [IXamlMetadaraProvider](/uwp/api/Windows.UI.Xaml.Markup.IXamlMetadataProvider) インターフェイスをサポートしています。これにより、アプリは実行時にアプリケーションの現在のディレクトリにある、アセンブリ内のカスタム UWP XAML コントロールのメタデータを検出して読み込むことができます。 このクラスでは、現在のスレッドの UWP XAML フレームワークも初期化されます。 このチュートリアルの後の方で、デスクトップ プロジェクトを更新して、このクラスのインスタンスを作成します。

  > [!NOTE]
  > `XamlApplication` オブジェクトは、XAML Islands を使用する各ソリューション内の 1 つのプロジェクトだけで、定義されている必要があります。 アプリ内のすべてのカスタム UWP XAML コントロールで、同じ `XamlApplication` オブジェクトを共有します。 

1. **ソリューション エクスプローラー**で、**MyUWPApp** プロジェクトの **MainPage.xaml** ファイルを右クリックします。 **[削除]** をクリックした後、 **[削除]** をクリックして、このファイルをプロジェクトから完全に削除します。
2. **MyUWPApp** プロジェクトで、**App.xaml** ファイルを展開します。
3. **App.xaml**、**App.cpp**、**App.h**、**App.idl** ファイルの内容を、次のコードに置き換えます。

    * **App.xaml**:

        ```xml
        <Toolkit:XamlApplication
            x:Class="MyUWPApp.App"
            xmlns:Toolkit="using:Microsoft.Toolkit.Win32.UI.XamlHost"
            xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:local="using:MyUWPApp">
        </Toolkit:XamlApplication>
        ```

    * **App.idl**:

        ```IDL
        namespace MyUWPApp
        {
             [default_interface]
             runtimeclass App : Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication
             {
                App();
             }
        }
        ```

    * **App.h**:

        ```cpp
        #pragma once
        #include "App.g.h"
        #include "App.base.h"
        namespace winrt::MyUWPApp::implementation
        {
            class App : public AppT2<App>
            {
            public:
                App();
                ~App();
            };
        }
        namespace winrt::MyUWPApp::factory_implementation
        {
            class App : public AppT<App, implementation::App>
            {
            };
        }
        ```

    * **App.cpp**:

        ```cpp
        #include "pch.h"
        #include "App.h"
        #include "App.g.cpp"
        using namespace winrt;
        using namespace Windows::UI::Xaml;
        namespace winrt::MyUWPApp::implementation
        {
            App::App()
            {
                Initialize();
                AddRef();
                m_inner.as<::IUnknown>()->Release();
            }
            App::~App()
            {
                Close();
            }
        }
        ```

        > [!NOTE]
        > プロジェクト プロパティの **[共通プロパティ]**  ->  **[C++/WinRT]** ページで **[最適化済み]** プロパティが **[はい]** に設定されている場合は、`#include "App.g.cpp"` ステートメントが必要です、 これは、新しい C++/WinRT プロジェクトの既定値です。 **[最適化済み]** プロパティの効果の詳細については、[こちらのセクション](/windows/uwp/cpp-and-winrt-apis/author-apis#opt-in-to-uniform-construction-and-direct-implementation-access)を参照してください。

4. **app.base.h** という名前の新しいヘッダー ファイルを、**MyUWPApp** プロジェクトに追加します。
5. 次のコードを **app.base.h** ファイルに追加し、ファイルを保存して閉じます。

    ```cpp
    #pragma once
    namespace winrt::MyUWPApp::implementation
    {
        template <typename D, typename... I>
        struct App_baseWithProvider : public App_base<D, ::winrt::Windows::UI::Xaml::Markup::IXamlMetadataProvider>
        {
            using IXamlType = ::winrt::Windows::UI::Xaml::Markup::IXamlType;
            IXamlType GetXamlType(::winrt::Windows::UI::Xaml::Interop::TypeName const& type)
            {
                return AppProvider()->GetXamlType(type);
            }
            IXamlType GetXamlType(::winrt::hstring const& fullName)
            {
                return AppProvider()->GetXamlType(fullName);
            }
            ::winrt::com_array<::winrt::Windows::UI::Xaml::Markup::XmlnsDefinition> GetXmlnsDefinitions()
            {
                return AppProvider()->GetXmlnsDefinitions();
            }
        private:
            bool _contentLoaded{ false };
            std::shared_ptr<XamlMetaDataProvider> _appProvider;
            std::shared_ptr<XamlMetaDataProvider> AppProvider()
            {
                if (!_appProvider)
                {
                    _appProvider = std::make_shared<XamlMetaDataProvider>();
                }
                return _appProvider;
            }
        };
        template <typename D, typename... I>
        using AppT2 = App_baseWithProvider<D, I...>;
    }
    ```

6. ソリューションをビルドし、正常にビルドされたことを確認します。

## <a name="configure-the-desktop-project-to-consume-custom-control-types"></a>カスタム コントロール型を使用するようにデスクトップ プロジェクトを構成する

**MyDesktopWin32App** アプリの XAML Island でカスタム UWP XAML コントロールをホストできるようにするには、先に、**MyUWPApp** プロジェクトのカスタム コントロール型を使用するようにアプリを構成する必要があります。 これを行うには 2 つの方法があり、どちらかのオプションを選択して、このチュートリアルを完了できます。

### <a name="option-1-package-the-app-using-msix"></a>オプション 1:MSIX を使用してアプリをパッケージ化する

配置用にアプリを [MSIX パッケージ](/windows/msix)にパッケージ化できます。 MSIX は、Windows 向けの最新のアプリ パッケージ化テクノロジであり、MSI、.appx、App-V、ClickOnce インストールの各テクノロジの組み合わせが基になっています。

1. ソリューションに新しい [Windows アプリケーション パッケージ プロジェクト](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)を追加します。 プロジェクトを作成するときに、名前を **MyDesktopWin32Project** にして、 **[ターゲット バージョン]** と **[最小バージョン]** の両方に対して、**Windows 10 バージョン 1903 (10.0、ビルド 18362)** を選択します。

2. パッケージ プロジェクトで、 **[アプリケーション]** ノードを右クリックして **[参照の追加]** を選択します。 プロジェクトの一覧で、**MyDesktopWin32App** プロジェクトの横のチェック ボックスをオンにして、 **[OK]** をクリックします。
    ![プロジェクトを参照する](images/xaml-islands/xaml-island-cpp-6.png)

> [!NOTE]
> 配置用に [MSIX パッケージ](/windows/msix)にアプリケーションをパッケージ化しない場合は、アプリを実行するコンピューターに [Visual C++ ランタイム](https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads)がインストールされている必要があります。

### <a name="option-2-create-an-application-manifest"></a>オプション 2:アプリケーション マニフェストを作成する

[アプリケーション マニフェスト](/windows/desktop/SbsCs/application-manifests)をアプリに追加できます。

1. **MyDesktopWin32App** プロジェクトを右クリックし、 **[追加]**  ->  **[新しい項目]** を選択します。 
2. **[新しい項目の追加]** ダイアログで、左側のペインの **[Web]** をクリックし、 **[XML ファイル (.xml)]** を選択します。 
3. 新しいファイルに「**app.manifest**」という名前を指定し、 **[追加]** をクリックします。
4. 新しいファイルの内容を次の XML に置き換えます。 この XML では、**MyUWPApp** プロジェクトにカスタム コントロール型が登録されます。

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <assembly
     xmlns="urn:schemas-microsoft-com:asm.v1"
     xmlns:asmv3="urn:schemas-microsoft-com:asm.v3"
     manifestVersion="1.0">
      <asmv3:file name="MyUWPApp.dll">
        <activatableClass
            name="MyUWPApp.App"
            threadingModel="both"
            xmlns="urn:schemas-microsoft-com:winrt.v1" />
        <activatableClass
            name="MyUWPApp.XamlMetadataProvider"
            threadingModel="both"
            xmlns="urn:schemas-microsoft-com:winrt.v1" />
        <activatableClass
            name="MyUWPApp.MyUserControl"
            threadingModel="both"
            xmlns="urn:schemas-microsoft-com:winrt.v1" />
      </asmv3:file>
    </assembly>
    ```

## <a name="configure-additional-desktop-project-properties"></a>デスクトップ プロジェクトの追加のプロパティを構成する

次に、**MyDesktopWin32App** プロジェクトを更新して、追加のインクルード ディレクトリのマクロを定義し、追加のプロパティを構成します。

1. **ソリューション エクスプローラー**で **MyDesktopWin32App** プロジェクトを右クリックし、 **[プロジェクトのアンロード]** を選択します。

2. **MyDesktopWin32App (アンロード済み)** を右クリックし、 **[MyDesktopWin32App.vcxproj の編集]** を選択します。

3. ファイルの末尾にある終了タグ `</Project>` の直前に、次の XML を追加します。 ファイルを保存して閉じます。

    ```xml
      <!-- Configure these for your UWP project -->
      <PropertyGroup>
        <AppProjectName>MyUWPApp</AppProjectName>
      </PropertyGroup>
      <PropertyGroup>
        <AppIncludeDirectories>$(SolutionDir)\obj\$(Platform)\$(Configuration)\$(AppProjectName)\;$(SolutionDir)\obj\$(Platform)\$(Configuration)\$(AppProjectName)\Generated Files\;</AppIncludeDirectories>
      </PropertyGroup>
      <ItemGroup>
        <ProjectReference Include="..\$(AppProjectName)\$(AppProjectName).vcxproj" />
      </ItemGroup>
      <!-- End Section-->
    ```

4. **ソリューション エクスプローラー**で、**MyDesktopWin32App (アンロード済み)** を右クリックして、 **[プロジェクトの再読み込み]** を選択します。

5. **MyDesktopWin32App** を右クリックし、 **[プロパティ]** を選択して、左側のペインで **[C/C++]** ノードをクリックします。 **[追加のインクルード ディレクトリ]** のマクロが、前のステップで行ったプロジェクト ファイルの変更で定義されていることを確認します。

    ![C/C++ プロジェクトの設定](images/xaml-islands/xaml-island-cpp-7.png)

6. **[プロパティ ページ]** ダイアログで、 **[マニフェスト ツール]**  ->  **[入出力]** を展開します。 **[DPI 認識]** プロパティを **[モニターごとの高い DPI 認識]** に設定します。 このプロパティを設定しなかった場合、特定の高 DPI シナリオでマニフェスト構成エラーが発生することがあります。

    ![C/C++ プロジェクトの設定](images/xaml-islands/xaml-island-cpp-8.png)

## <a name="host-the-custom-uwp-xaml-control-in-the-desktop-project"></a>デスクトップ プロジェクトでカスタム UWP XAML コントロールをホストする

最後に、**MyUWPApp** プロジェクトで前に定義したカスタム UWP XAML コントロールをホストするコードを、**MyDesktopWin32App** プロジェクトに追加します。

1. **MyDesktopWin32App** プロジェクトで **framework.h** ファイルを開き、次のコード行をコメントアウトします。 終わったらファイルを保存します。

    ```cpp
    #define WIN32_LEAN_AND_MEAN
    ```

2. **MyDesktopWin32App.h** ファイルを開き、このファイルの内容を次のコードに置き換えて、必要な C++/WinRT ヘッダー ファイルを参照します。 終わったらファイルを保存します。

    ```cpp
    #pragma once

    #include "resource.h"
    #include <winrt/Windows.Foundation.Collections.h>
    #include <winrt/Windows.system.h>
    #include <winrt/windows.ui.xaml.hosting.h>
    #include <windows.ui.xaml.hosting.desktopwindowxamlsource.h>
    #include <winrt/windows.ui.xaml.controls.h>
    #include <winrt/Windows.ui.xaml.media.h>
    #include <winrt/Windows.UI.Core.h>
    #include <winrt/MyUWPApp.h>

    using namespace winrt;
    using namespace Windows::UI;
    using namespace Windows::UI::Composition;
    using namespace Windows::UI::Xaml::Hosting;
    using namespace Windows::Foundation::Numerics;
    using namespace Windows::UI::Xaml::Controls;
    ```

3. **MyDesktopWin32App.cpp** ファイルを開き、次のコードを `Global Variables:` セクションに追加します。

    ```cpp
    winrt::MyUWPApp::App hostApp{ nullptr };
    winrt::Windows::UI::Xaml::Hosting::DesktopWindowXamlSource _desktopWindowXamlSource{ nullptr };
    winrt::MyUWPApp::MyUserControl _myUserControl{ nullptr };
    ```

4. 同じファイルで、次のコードを `Forward declarations of functions included in this code module:` セクションに追加します。

    ```cpp
    void AdjustLayout(HWND);
    ```

5. 同じファイルで、`wWinMain` 関数の `TODO: Place code here.` コメントの直後に次のコードを追加します。

    ```cpp
    // TODO: Place code here.
    winrt::init_apartment(winrt::apartment_type::single_threaded);
    hostApp = winrt::MyUWPApp::App{};
    _desktopWindowXamlSource = winrt::Windows::UI::Xaml::Hosting::DesktopWindowXamlSource{};
    ```

6. 同じファイルで、既定の `InitInstance` 関数を次のコードに置き換えます。

    ```cpp
    BOOL InitInstance(HINSTANCE hInstance, int nCmdShow)
    {
        hInst = hInstance; // Store instance handle in our global variable

        HWND hWnd = CreateWindowW(szWindowClass, szTitle, WS_OVERLAPPEDWINDOW,
            CW_USEDEFAULT, 0, CW_USEDEFAULT, 0, nullptr, nullptr, hInstance, nullptr);

        if (!hWnd)
        {
            return FALSE;
        }

        // Begin XAML Islands walkthrough code.
        if (_desktopWindowXamlSource != nullptr)
        {
            auto interop = _desktopWindowXamlSource.as<IDesktopWindowXamlSourceNative>();
            check_hresult(interop->AttachToWindow(hWnd));
            HWND hWndXamlIsland = nullptr;
            interop->get_WindowHandle(&hWndXamlIsland);
            RECT windowRect;
            ::GetWindowRect(hWnd, &windowRect);
            ::SetWindowPos(hWndXamlIsland, NULL, 0, 0, windowRect.right - windowRect.left, windowRect.bottom - windowRect.top, SWP_SHOWWINDOW);
            _myUserControl = winrt::MyUWPApp::MyUserControl();
            _desktopWindowXamlSource.Content(_myUserControl);
        }
        // End XAML Islands walkthrough code.

        ShowWindow(hWnd, nCmdShow);
        UpdateWindow(hWnd);

        return TRUE;
    }
    ```

7. 同じファイルで、次の新しい関数をファイルの最後に追加します。

    ```cpp
    void AdjustLayout(HWND hWnd)
    {
        if (_desktopWindowXamlSource != nullptr)
        {
            auto interop = _desktopWindowXamlSource.as<IDesktopWindowXamlSourceNative>();
            HWND xamlHostHwnd = NULL;
            check_hresult(interop->get_WindowHandle(&xamlHostHwnd));
            RECT windowRect;
            ::GetWindowRect(hWnd, &windowRect);
            ::SetWindowPos(xamlHostHwnd, NULL, 0, 0, windowRect.right - windowRect.left, windowRect.bottom - windowRect.top, SWP_SHOWWINDOW);
        }
    }
    ```

8. 同じファイルで、`WndProc` 関数を見つけます。 switch ステートメントの既定の `WM_DESTROY` ハンドラーを、次のコードに置き換えます。

    ```cpp
    case WM_DESTROY:
        PostQuitMessage(0);
        if (_desktopWindowXamlSource != nullptr)
        {
            _desktopWindowXamlSource.Close();
            _desktopWindowXamlSource = nullptr;
        }
        break;
    case WM_SIZE:
        AdjustLayout(hWnd);
        break;
    ```

9. ファイルを保存します。
10. ソリューションをビルドし、正常にビルドされたことを確認します。

## <a name="add-a-control-from-the-winui-library-to-the-custom-control"></a>WinUI ライブラリのコントロールをカスタム コントロールに追加する

従来、UWP コントロールは Windows 10 OS の一部としてリリースされ、開発者は Windows SDK を通じてそれを使用できました。 [WinUI ライブラリ](/uwp/toolkits/winui/) はそれに代わる方法であり、Windows SDK の UWP コントロールの更新バージョンが、Windows SDK のリリースに関連付けられていない NuGet パッケージで配布されます。 また、このライブラリには、Windows SDK および既定の UWP プラットフォームの一部ではない新しいコントロールも含まれています。 詳しくは、[WinUI ライブラリのロードマップ](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md)をご覧ください。

このセクションでは、WinUI ライブラリからユーザー コントロールに UWP コントロールを追加する方法について説明します。

1. **MyUWPApp** プロジェクトで、最新のプレリリースまたはリリース バージョンの [Microsoft.UI.Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml) NuGet パッケージをインストールします。

    > [!NOTE]
    > お使いのデスクトップ アプリが [MSIX パッケージ](/windows/msix)にパッケージ化されている場合は、[Microsoft.UI.Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml) NugGet パッケージのプレリリースまたはリリース バージョンのいずれかを使用できます。 お使いのデスクトップ アプリが MSIX を使用してパッケージ化されていない場合は、プレリリース バージョンの [Microsoft.UI.Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml) NuGet パッケージをインストールする必要があります。

2. このプロジェクトの pch.h ファイルで、次の `#include` ステートメントを追加し、変更を保存します。 これらのステートメントは、一連の必要なプロジェクション ヘッダーを WinUI ライブラリからプロジェクトに取り込みます。 この手順は、WinUI ライブラリを使用する任意の C++/WinRT プロジェクトに必要です。 詳しくは、[こちらの記事](/uwp/toolkits/winui/getting-started#additional-steps-for-a-cwinrt-project)をご覧ください。

    ```cpp
    #include "winrt/Microsoft.UI.Xaml.Automation.Peers.h"
    #include "winrt/Microsoft.UI.Xaml.Controls.Primitives.h"
    #include "winrt/Microsoft.UI.Xaml.Media.h"
    #include "winrt/Microsoft.UI.Xaml.XamlTypeInfo.h"
    ```

3. 同じプロジェクトの App.xaml ファイルで、次の子要素を `<xaml:XamlApplication>` 要素に追加し、変更を保存します。

    ```xml
    <Application.Resources>
        <XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls" />
    </Application.Resources>
    ```

    この要素を追加した後、このファイルの内容は次のようになります。

    ```xml
    <Toolkit:XamlApplication
        x:Class="MyUWPApp.App"
        xmlns:Toolkit="using:Microsoft.Toolkit.Win32.UI.XamlHost"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="using:MyUWPApp">
        <Application.Resources>
            <XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls"/>
        </Application.Resources>
    </Toolkit:XamlApplication>
    ```

4. 同じプロジェクトで、MyUserControl.xaml ファイルを開き、次の名前空間宣言を `<UserControl>` 要素に追加します。

    ```xml
    xmlns:winui="using:Microsoft.UI.Xaml.Controls"
    ```

5. 同じファイルで、`<StackPanel>` の子として `<winui:RatingControl />` 要素を追加し、変更を保存します。 この要素により、WinUI ライブラリの [RatingControl](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.ratingcontrol ) クラスのインスタンスが追加されます。 この要素を追加した後の `<StackPanel>` は、次のようになります。

    ```xml
    <StackPanel HorizontalAlignment="Center" Spacing="10" 
                Padding="20" VerticalAlignment="Center">
        <TextBlock HorizontalAlignment="Center" TextWrapping="Wrap" 
                       Text="Hello from XAML Islands" FontSize="30" />
        <TextBlock HorizontalAlignment="Center" Margin="15" TextWrapping="Wrap"
                       Text="😍❤💋🌹🎉😎�🐱‍👤" FontSize="16" />
        <Button HorizontalAlignment="Center" 
                x:Name="Button" Click="ClickHandler">Click Me</Button>
        <winui:RatingControl />
    </StackPanel>
    ```

6. ソリューションをビルドし、正常にビルドされたことを確認します。

## <a name="test-the-app"></a>アプリをテストする

ソリューションを実行し、次のウィンドウで **MyDesktopWin32App** が開かれることを確認します。

![MyDesktopWin32App アプリ](images/xaml-islands/xaml-island-cpp-9.png)

## <a name="next-steps"></a>次のステップ

XAML Islands をホストする多くのデスクトップ アプリケーションでは、スムーズなユーザー エクスペリエンスを提供するために処理する必要があるシナリオが他にもあります。 たとえば、デスクトップ アプリケーションでは、XAML Islands でのキーボード入力、XAML Islands と他の UI 要素の間でのフォーカス ナビゲーション、およびレイアウトの変更を処理することが、必要になる場合があります。

これらのシナリオの処理に関する詳細、および関連するコード サンプルへのリンクについては、「[C++ Win32 アプリでの XAML Islands の高度なシナリオ](advanced-scenarios-xaml-islands-cpp.md)」を参照してください。

## <a name="related-topics"></a>関連トピック

* [デスクトップ アプリで UWP XAML コントロールをホストする (XAML Islands)](xaml-islands.md)
* [C++ Win32 アプリでの UWP XAML ホスティング API の使用](using-the-xaml-hosting-api.md)
* [C++ Win32 アプリで標準 UWP コントロールをホストする](host-standard-control-with-xaml-islands-cpp.md)
* [C++ Win32 アプリでの XAML Islands の高度なシナリオ](advanced-scenarios-xaml-islands-cpp.md)
* [XAML Islands コード サンプル](https://github.com/microsoft/Xaml-Islands-Samples)