---
description: この記事では、XAML ホスティング API を使用してC++ 、Win32 アプリでカスタム UWP コントロールをホストする方法について説明します。
title: XAML ホスティング API を使用してC++ Win32 アプリでカスタム UWP コントロールをホストする
ms.date: 03/23/2020
ms.topic: article
keywords: windows 10、uwp、 C++、Win32、xaml アイランド、カスタムコントロール、ユーザーコントロール、ホストコントロール
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 2f34c9c56cf9db5dfcfd702b97f2d34273b86e6a
ms.sourcegitcommit: c660def841abc742600fbcf6ed98e1f4f7beb8cc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/24/2020
ms.locfileid: "80226316"
---
# <a name="host-a-custom-uwp-control-in-a-c-win32-app"></a>C++ Win32 アプリでカスタム UWP コントロールをホストする

この記事では、 [UWP xaml ホスティング API](using-the-xaml-hosting-api.md)を使用して、新しいC++ WIN32 アプリでカスタム UWP xaml コントロールをホストする方法について説明します。 既存C++の Win32 アプリプロジェクトがある場合は、プロジェクトのこれらの手順とコード例を調整できます。

カスタム UWP XAML コントロールをホストするには、このチュートリアルの一部として次のプロジェクトとコンポーネントを作成します。

* **Windows デスクトップアプリケーションプロジェクト**。 このプロジェクトは、ネイティブC++の Win32 デスクトップアプリを実装します。 UWP XAML ホスティング API を使用してカスタム UWP XAML コントロールをホストするコードをこのプロジェクトに追加します。

* **UWP アプリプロジェクト (C++/WinRT)** 。 このプロジェクトは、カスタム UWP XAML コントロールを実装します。 また、プロジェクトのカスタム UWP XAML 型のメタデータを読み込むためのルートメタデータプロバイダーも実装します。

## <a name="requirements"></a>要件

* Visual Studio 2019 バージョン16.4.3 以降。
* Windows 10 バージョン 1903 SDK (バージョン 10.0.18362) 以降。
* Visual studio と共にインストールされた visual [studio 拡張機能 (VSIX)。 C++](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264) C++/WinRT は Windows ランタイム (WinRT) API の標準的な最新の C++17 言語プロジェクションで、ヘッダー ファイル ベースのライブラリとして実装され、最新の Windows API への最上位アクセス権を提供するように設計されています。 詳細については、「 [ C++/WinRT](https://docs.microsoft.com/windows/uwp/cpp-and-winrt-apis/)」を参照してください。

## <a name="create-a-desktop-application-project"></a>デスクトップアプリケーションプロジェクトを作成する

1. Visual Studio で、 **MyDesktopWin32App**という名前の新しい**Windows デスクトップアプリケーション**プロジェクトを作成します。 このプロジェクトテンプレートは、、 **C++** **Windows**、および**デスクトップ**のプロジェクトフィルターで使用できます。

2. **ソリューションエクスプローラー**で、ソリューションノードを右クリックし、 **[ソリューション]** の再ターゲット をクリックして、 **10.0.18362.0**またはそれ以降の SDK リリースを選択し、 **[OK]** をクリックします。

3. [Microsoft. Windows. cppwinrt](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) NuGet パッケージをインストールして、プロジェクトで[ C++/WinRT](/windows/uwp/cpp-and-winrt-apis)のサポートを有効にします。

    1. **ソリューションエクスプローラー**で**MyDesktopWin32App**プロジェクトを右クリックし、 **[NuGet パッケージの管理]** を選択します。
    2. **[参照]** タブを選択し、「 [Microsoft. Windows. cppwinrt](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/)パッケージ」を検索して、このパッケージの最新バージョンをインストールします。

4. **[Nuget パッケージの管理]** ウィンドウで、次の追加の NuGet パッケージをインストールします。

    * [Microsoft. Toolkit. UI](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.SDK) (version v 6.0.0 以降)。 このパッケージには、XAML Islands をアプリで使用できるようにする、いくつかのビルドおよび実行時アセットが用意されています。
    * [Microsoft. Toolkit. UI. XamlApplication](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication) (version v 6.0.0 以降)
    * [VCRTForwarders](https://www.nuget.org/packages/Microsoft.VCRTForwarders.140)。

5. ソリューションをビルドし、正常にビルドされることを確認します。

## <a name="create-a-uwp-app-project"></a>UWP アプリプロジェクトを作成する

次に、 **UWP (C++/WinRT)** アプリプロジェクトをソリューションに追加し、このプロジェクトにいくつかの構成変更を加えます。 このチュートリアルの後の方で、このプロジェクトにコードを追加して、カスタム UWP XAML コントロールを実装し、Windows Community Toolkit によって提供される、Microsoft の Toolkit............. [UI. Xamlhost](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication)クラスのインスタンスを定義します。 このクラスは、カスタムの UWP XAML 型のメタデータを読み込むために、ルートメタデータプロバイダーとして使用されます。

1. **ソリューションエクスプローラー**で、[ソリューション] ノードを右クリックし、[ -> **新しいプロジェクト**の**追加**] を選択します。

2. 空の**アプリ (C++WinRT)** プロジェクトをソリューションに追加します。 プロジェクトに**MyUWPApp**という名前を設定し、ターゲットバージョンと最小バージョンの両方が**Windows 10 バージョン 1903**以降に設定されていることを確認します。

3. **MyUWPApp**プロジェクトに、次のように[、NuGet パッケージをインストールします](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication)。

    1. **MyUWPApp**プロジェクトを右クリックし、 **[NuGet パッケージの管理]** を選択します。
    2. **[参照]** タブを選択し、 [6.0.0 パッケージを](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication)検索して、v2.0 またはそれ以降のバージョンのパッケージをインストールします。

4. **MyUWPApp**ノードを右クリックし、 **[プロパティ]** を選択します。 [**共通プロパティ** ->  **C++/WinRT** ] ページで、次のプロパティを設定し、 **[適用]** をクリックします。

    * **詳細**度を**通常**に設定します。
    * "**最適化**" を "**いいえ**" に設定します。

    完了すると、[プロパティ] ページは次のようになります。

    ![C++/WinRT プロジェクトのプロパティ](images/xaml-islands/xaml-island-cpp-1.png)

5. プロパティ ウィンドウの **構成プロパティ** -> **全般** ページで、**構成の種類** を **ダイナミックライブラリ (.dll)** に設定し、**OK** をクリックして プロパティ ウィンドウを閉じます。

    ![一般的なプロジェクトプロパティ](images/xaml-islands/xaml-island-cpp-2.png)

6. **MyUWPApp**プロジェクトに、プレースホルダーの実行可能ファイルを追加します。 このプレースホルダーの実行可能ファイルは、Visual Studio が必要なプロジェクトファイルを生成し、プロジェクトを正しくビルドするために必要です。

    1. **ソリューションエクスプローラー**で、 **MyUWPApp**プロジェクトノードを右クリックし、[ -> **新しい項目**の**追加**] を選択します。
    2. **[新しい項目の追加]** ダイアログで、左側のページにある **[ユーティリティ]** を選択し、 **[テキストファイル (.txt)]** を選択します。 「 **Placeholder .exe** 」という名前を入力し、 **[追加]** をクリックします。
      テキストファイル](images/xaml-islands/xaml-island-cpp-3.png) を追加 ![には
    3. **ソリューションエクスプローラー**で、**プレースホルダー**ファイルを選択します。 **[プロパティ]** ウィンドウで、 **[コンテンツ]** プロパティが **[True]** に設定されていることを確認します。
    4. **ソリューションエクスプローラー**で、 **MyUWPApp**プロジェクトの**package.appxmanifest**ファイルを右クリックし、 **[ファイルを開くアプリケーション]** の選択 をクリックして **[XML (テキスト) エディター]** を選択し、 **[OK]** をクリックします。
    5. **&lt;アプリケーションの&gt;** 要素を見つけて、 **Executable**属性を `placeholder.exe`値に変更します。 完了すると、 **&lt;アプリケーションの&gt;** 要素は次のようになります。

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

    6. **Package.appxmanifest**ファイルを保存して閉じます。

7. **ソリューションエクスプローラー**で、 **[MyUWPApp]** ノードを右クリックし、 **[プロジェクトのアンロード]** を選択します。
8. **MyUWPApp**ノードを右クリックし、 **[MyUWPApp の編集]** を選択します。
9. `<Import Project="$(VCTargetsPath)\Microsoft.Cpp.Default.props" />` 要素を見つけて、次の XML に置き換えます。 この XML は、要素の直前に新しいプロパティをいくつか追加します。

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
11. **ソリューションエクスプローラー**で、 **[MyUWPApp]** ノードを右クリックし、 **[プロジェクトの再読み込み]** を選択します。

## <a name="configure-the-solution"></a>ソリューションを構成する

このセクションでは、両方のプロジェクトを含むソリューションを更新して、プロジェクトの依存関係を構成し、プロジェクトを正しくビルドするために必要なビルドプロパティを構成します。

1. **ソリューションエクスプローラー**で、ソリューションノードを右クリックし、「 **solution. props**」という名前の新しい XML ファイルを追加します。
2. 次の XML をソリューションの**props**ファイルに追加します。

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

3. **[表示]** メニューの **[プロパティマネージャー]** をクリックします (構成によっては、**他のウィンドウ** -> **表示**される場合があります)。
4. **プロパティマネージャー**ウィンドウで、 **[MyDesktopWin32App]** を右クリックし、 **[既存のプロパティシートの追加]** を選択します。 追加した**ソリューションの props**ファイルに移動し、 **[開く]** をクリックします。
5. 前の手順を繰り返して、 **MyUWPApp**プロジェクトの **[プロパティマネージャー]** ウィンドウに**ソリューション**のプロパティを追加します。
6. **[プロパティマネージャー]** ウィンドウを閉じます。
7. プロパティシートの変更が正常に保存されたことを確認します。 **ソリューションエクスプローラー**で、 **MyDesktopWin32App**プロジェクトを右クリックし、 **[プロパティ]** を選択します。 **[構成プロパティ]** をクリックし、 **[ -> ] をクリック**して、**出力ディレクトリ**と**中間ディレクトリ**のプロパティに、**ソリューションの props**ファイルに追加した値があることを確認します。 **MyUWPApp**プロジェクトでも同じことを確認できます。
    ![プロジェクトのプロパティ](images/xaml-islands/xaml-island-cpp-4.png)

8. **ソリューションエクスプローラー**で、ソリューションノードを右クリックし、 **[プロジェクトの依存関係]** を選択します。 **[プロジェクト]** ドロップダウンで、 **[MyDesktopWin32App]** が選択されていることを確認し、 **[依存]** 先 の一覧で **[MyUWPApp]** を選択します。
    プロジェクトの依存関係を ![](images/xaml-islands/xaml-island-cpp-5.png)

9. **[OK]** をクリックすると、

## <a name="add-code-to-the-uwp-app-project"></a>UWP アプリプロジェクトにコードを追加する

これで、 **MyUWPApp**プロジェクトにコードを追加して、次のタスクを実行する準備ができました。

* カスタム UWP XAML コントロールを実装します。 このチュートリアルの後半では、 **MyDesktopWin32App**プロジェクトでこのコントロールをホストするコードを追加します。
* Windows Community Toolkit の " [Microsoft. toolkit](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication) ......................................... このクラスは、カスタムの UWP XAML 型のメタデータを読み込むためのルートメタデータプロバイダーとして機能します。

### <a name="define-a-custom-uwp-xaml-control"></a>カスタム UWP XAML コントロールを定義する

1. **ソリューションエクスプローラー**で、 **[MyUWPApp]** を右クリックし、[**新しい項目**の**追加** -> ] を選択します。 左側のウィンドウで**ビジュアルC++** ノードを選択し、 **[空のユーザーC++コントロール (/WinRT)]** を選択し、 **MyUserControl**という名前を付けて、 **[追加]** をクリックします。
2. XAML エディターで、 **MyUserControl**ファイルの内容を次の xaml に置き換え、ファイルを保存します。

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

次に、MyUWPApp プロジェクトの既定の**アプリ**クラスを変更して、Windows Community Toolkit によって提供される**MyUWPApp** [クラスから](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication)派生するようにします。 このチュートリアルの後の方で、デスクトッププロジェクトを更新して、カスタムの UWP XAML 型のメタデータを読み込むためのルートメタデータプロバイダーとして、このクラスのインスタンスを作成します。

  > [!NOTE]
  > XAML アイランドを使用する各ソリューションには、`XamlApplication` オブジェクトを定義するプロジェクトを1つだけ含めることができます。 アプリ内のすべてのカスタム UWP XAML コントロールは、同じ `XamlApplication` オブジェクトを共有します。 

1. **ソリューションエクスプローラー**で、 **MyUWPApp**プロジェクトの**mainpage.xaml**ファイルを右クリックします。 **[削除]** をクリックし、 **[削除]** をクリックして、このファイル permamently をプロジェクトから削除します。
2. **MyUWPApp**プロジェクトで、 **app.xaml**ファイルを展開します。
3. **App.xaml**、App.xaml、 **App.xaml**、および**app.config**ファイルの内容を、次のコードに置き換え**ます ()** 。

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

    * **App.config**:

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

    * **アプリケーション .h**:

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

    * **アプリケーション .cpp**:

        ```cpp
        #include "pch.h"
        #include "App.h"
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

4. **MyUWPApp**プロジェクトに新しいヘッダーファイルを追加します **。**
5. 次のコードを**app.config**ファイルに追加し、ファイルを保存して閉じます。

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

6. ソリューションをビルドし、正常にビルドされることを確認します。

## <a name="configure-the-desktop-project-to-consume-custom-control-types"></a>カスタムコントロール型を使用するようにデスクトッププロジェクトを構成する

**MyDesktopWin32App**アプリでカスタム UWP xaml コントロールを xaml アイランドでホストできるようにするには、 **MyUWPApp**プロジェクトからカスタムコントロール型を使用するように構成する必要があります。 これを行うには2つの方法があります。このチュートリアルを完了するには、いずれかのオプションを選択できます。

### <a name="option-1-package-the-app-using-msix"></a>オプション 1: MSIX を使用してアプリをパッケージ化する

アプリを[Msix パッケージ](https://docs.microsoft.com/windows/msix)にパッケージ化してデプロイすることができます。 MSIX は Windows 向けの最新のアプリケーションパッケージ化テクノロジであり、MSI、.appx、App-v、ClickOnce のインストールテクノロジの組み合わせに基づいています。

1. 新しい[Windows アプリケーションパッケージプロジェクト](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)をソリューションに追加します。 プロジェクトを作成するときに、 **MyDesktopWin32Project**という名前を付け、 **Windows 10 バージョン 1903 (10.0; を選択します。ビルド 18362)** 。**ターゲットバージョン**と**最小バージョン**の両方に対応します。

2. パッケージプロジェクトで、 **[アプリケーション]** ノードを右クリックし、 **[参照の追加]** を選択します。 プロジェクトの一覧で、 **MyDesktopWin32App**プロジェクトの横にあるチェックボックスをオンにし、[ **OK]** をクリックします。
    ![参照プロジェクト](images/xaml-islands/xaml-island-cpp-6.png)

> [!NOTE]
> アプリケーションを展開用の[Msix パッケージ](https://docs.microsoft.com/windows/msix)にパッケージ化しない場合は、アプリを実行するコンピューターに[ C++ Visual Runtime](https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads)がインストールされている必要があります。

### <a name="option-2-create-an-application-manifest"></a>オプション 2: アプリケーションマニフェストを作成する

[アプリケーションマニフェスト](https://docs.microsoft.com/windows/desktop/SbsCs/application-manifests)をアプリに追加できます。

1. **MyDesktopWin32App**プロジェクトを右クリックし、[**新しい項目**の**追加** -> ] を選択します。 
2. **[新しい項目の追加]** ダイアログで、左ペインの **[Web]** をクリックし、 **[xml ファイル (.xml)]** を選択します。 
3. 新しいファイルに「 **app.xaml** 」という名前を指定し、 **[追加]** をクリックします。
4. 新しいファイルの内容を次の XML に置き換えます。 この XML は、 **MyUWPApp**プロジェクトにカスタムコントロール型を登録します。

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

## <a name="configure-additional-desktop-project-properties"></a>追加のデスクトッププロジェクトプロパティの構成

次に、 **MyDesktopWin32App**プロジェクトを更新して、追加のインクルードディレクトリのマクロを定義し、追加のプロパティを構成します。

1. **ソリューションエクスプローラー**で、 **MyDesktopWin32App**プロジェクトを右クリックし、 **[プロジェクトのアンロード]** を選択します。

2. **[MyDesktopWin32App (アンロード)]** を右クリックし、 **[MyDesktopWin32App の編集]** を選択します。

3. ファイルの末尾にある終了 `</Project>` タグの直前に、次の XML を追加します。 次に、ファイルを保存して閉じます。

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

4. **ソリューションエクスプローラー**で、 **[MyDesktopWin32App (アンロード)]** を右クリックし、 **[プロジェクトの再読み込み]** を選択します。

5. **[MyDesktopWin32App]** を右クリックし、 **[プロパティ]** を選択して、左ペインの **[C/C++ ]** ノードをクリックします。 **追加のインクルードディレクトリ**マクロが、前の手順で行ったプロジェクトファイルの変更から定義されていることを確認します。
    ![C/C++ project の設定](images/xaml-islands/xaml-island-cpp-7.png)

6. **[プロパティページ]** ダイアログボックスで、[**マニフェストツール** -> **入力および出力**] を展開します。 **Dpi 認識**プロパティをモニターの**高 DPI 対応**に設定します。 このプロパティを設定しなかった場合、特定の高 DPI シナリオでマニフェスト構成エラーが発生することがあります。
    ![C/C++ project の設定](images/xaml-islands/xaml-island-cpp-8.png)

## <a name="host-the-custom-uwp-xaml-control-in-the-desktop-project"></a>デスクトッププロジェクトでカスタム UWP XAML コントロールをホストする

最後に、 **MyDesktopWin32App**プロジェクトにコードを追加して、 **MyUWPApp**プロジェクトで前に定義したカスタム UWP XAML コントロールをホストする準備ができました。

1. **MyDesktopWin32App**プロジェクトで、**フレームワーク .h**ファイルを開き、次のコード行をコメントアウトします。 完了したら、ファイルを保存します。

    ```cpp
    #define WIN32_LEAN_AND_MEAN
    ```

2. **MyDesktopWin32App**ファイルを開き、このファイルの内容を次のコードに置き換えて、必要なC++WinRT ヘッダーファイルを参照します。 完了したら、ファイルを保存します。

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

3. **MyDesktopWin32App**ファイルを開き、次のコードを `Global Variables:` セクションに追加します。

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

7. 同じファイル内で、ファイルの末尾に次の新しい関数を追加します。

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

8. 同じファイルで、`WndProc` 関数を見つけます。 Switch ステートメントの既定の `WM_DESTROY` ハンドラーを次のコードに置き換えます。

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
10. ソリューションをビルドし、正常にビルドされることを確認します。

## <a name="test-the-app"></a>アプリをテストする

ソリューションを実行し、次のウィンドウで**MyDesktopWin32App**が開いていることを確認します。

![MyDesktopWin32App アプリ](images/xaml-islands/xaml-island-cpp-9.png)

## <a name="next-steps"></a>次のステップ:

XAML アイランドをホストする多くのデスクトップアプリケーションでは、ユーザーエクスペリエンスをスムーズにするために、追加のシナリオを処理する必要があります。 たとえば、デスクトップアプリケーションでは、XAML アイランドでのキーボード入力の処理、XAML アイランドと他の UI 要素間のフォーカスナビゲーション、およびレイアウトの変更が必要になる場合があります。

これらのシナリオおよび関連するコードサンプルへのポインターの処理の詳細については、「 [Win32 アプリでC++の XAML アイランドの高度なシナリオ](advanced-scenarios-xaml-islands-cpp.md)」を参照してください。

## <a name="related-topics"></a>関連トピック

* [デスクトップアプリでの UWP XAML コントロールのホスト (XAML アイランド)](xaml-islands.md)
* [C++ Win32 アプリで UWP XAML ホスティング API を使用する](using-the-xaml-hosting-api.md)
* [C++ Win32 アプリで標準の UWP コントロールをホストする](host-standard-control-with-xaml-islands-cpp.md)
* [Win32 アプリでC++の XAML アイランドの高度なシナリオ](advanced-scenarios-xaml-islands-cpp.md)
* [XAML アイランドコードサンプル](https://github.com/microsoft/Xaml-Islands-Samples)
