---
description: この記事では、XAML Islands を使用して WPF アプリでカスタム WinRT XAML コントロールをホストする方法を示します。
title: XAML Islands を使用して WPF アプリでカスタム WinRT XAML コントロールをホストする
ms.date: 10/02/2020
ms.topic: article
keywords: Windows 10, UWP, Windows フォーム, WPF, XAML Islands, カスタム コントロール, ユーザー コントロール, コントロールのホスト
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 714573c91b8352bb52bb895347867862bd0642b3
ms.sourcegitcommit: 41251e85b2168704e33b9d3ee92606ab84c24769
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/02/2021
ms.locfileid: "99478527"
---
# <a name="host-a-custom-winrt-xaml-control-in-a-wpf-app-using-xaml-islands"></a>XAML Islands を使用して WPF アプリでカスタム WinRT XAML コントロールをホストする

この記事では、Windows Community Toolkit の [WindowsXamlHost](/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) コントロールを使用して、.NET Core 3.1 を対象とする WPF アプリでカスタム WinRT XAML コントロールをホストする方法について説明します。 このカスタム コントロールには Windows SDK のいくつかのファーストパーティ コントロールが含まれており、1 つの WinRT XAML コントロールのプロパティが WPF アプリの文字列にバインドされています。 また、この記事では、[WinUI ライブラリ](/uwp/toolkits/winui/)のコントロールをホストする方法についても説明します。

この記事では、WPF アプリでこれを行う方法を示しますが、Windows フォーム アプリでもプロセスはほぼ同じです。 WPF および Windows フォーム アプリで WinRT XAML コントロールをホストする方法の概要については、[こちらの記事](xaml-islands.md#wpf-and-windows-forms-applications)をご覧ください。

> [!NOTE]
> WPF および Windows フォーム アプリでの XAML Islands を使用した WinRT XAML コントロールのホストは、現在、.NET Core 3.x をターゲットとするアプリでのみサポートされています。 XAML Islands は、.NET 5 をターゲットとするアプリ、または .NET Framework のすべてのバージョンのアプリでは、まだサポートされていません。

## <a name="required-components"></a>必要なコンポーネント

WPF (または Windows フォーム) アプリでカスタム WinRT XAML コントロールをホストするには、ソリューションに次のコンポーネントが必要です。 この記事では、これらの各コンポーネントを作成する手順について説明します。

* **アプリのプロジェクトとソース コード**。 [WindowsXamlHost](/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) コントロールを使用したカスタム コントロールのホストは、.NET Core 3.x をターゲットとするアプリでのみサポートされています。

* **カスタム WinRT XAML コントロール**。 アプリと共にコンパイルできるように、ホストするカスタム コントロールのソース コードを用意する必要があります。 通常、カスタム コントロールは、WPF プロジェクトまたは Windows フォーム プロジェクトと同じソリューションで参照されている UWP クラス ライブラリ プロジェクトで定義します。

* **XamlApplication から派生するルート Application クラスが定義されている UWP アプリ プロジェクト**。 WPF プロジェクトまたは Windows フォーム プロジェクトでは、カスタムの UWP XAML コントロールを検出て読み込めるように、Windows Community Toolkit によって提供される [Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication) クラスのインスタンスにアクセスできる必要があります。 これを行うには、WPF アプリまたは Windows フォーム アプリのソリューションの一部である別の UWP アプリ プロジェクト内でこのオブジェクトを定義することをお勧めします。 

    > [!NOTE]
    > `XamlApplication` オブジェクトは、ソリューション内の 1 つのプロジェクトだけで定義されている必要があります。 アプリ内のすべてのカスタム WinRT XAML コントロールで、同じ `XamlApplication` オブジェクトを共有します。 `XamlApplication` オブジェクトが定義されているプロジェクトには、XAML Island でコントロールをホストするために使用される他のすべての WinRT ライブラリとプロジェクトへの参照が含まれている必要があります。

## <a name="create-a-wpf-project"></a>WPF プロジェクトを作成する

作業を始める前に、次の手順に従って WPF プロジェクトを作成し、XAML Islands をホストするように構成します。 WPF プロジェクトが既にある場合は、以下の手順とコード例をプロジェクトに合わせて調整してかまいません。

> [!NOTE]
> .NET Framework を対象とする既存のプロジェクトがある場合は、.NET Core 3.1 にプロジェクトを移行する必要があります。 詳しくは、[こちらのブログ シリーズ](https://devblogs.microsoft.com/dotnet/migrating-a-sample-wpf-app-to-net-core-3-part-1/)をご覧ください。

1. まだ行っていない場合は、最新バージョンの [.NET Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet/current) をインストールします。

2. Visual Studio 2019 で、新しい **WPF アプリ (.NET Core)** プロジェクトを作成します。

3. [パッケージ参照](/nuget/consume-packages/package-references-in-project-files)が有効になっていることを確認します。

    1. Visual Studio で、 **[ツール] -> [NuGet パッケージ マネージャー] -> [パッケージ マネージャー設定]** の順にクリックします。
    2. **[既定のパッケージ管理形式]** で **[PackageReference]** が選択されていることを確認します。

4. **ソリューション エクスプローラー** で WPF プロジェクトを右クリックし、 **[NuGet パッケージの管理]** を選択します。

5. **[参照]** タブを選択し、[Microsoft.Toolkit.Wpf.UI.XamlHost](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.XamlHost) パッケージを見つけて、最新の安定バージョンをインストールします。 このパッケージでは、他の関連する NuGet パッケージなど、**WindowsXamlHost** コントロールを使用して WinRT XAML コントロールをホストするために必要なすべてのものが提供されます。
    > [!NOTE]
    > Windows フォーム アプリでは、[Microsoft.Toolkit.Forms.UI.XamlHost](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.XamlHost) パッケージを使用する必要があります。

6. x86 や x64 などの特定のプラットフォームを対象とするようにソリューションを構成します。 **[Any CPU]** を対象とするプロジェクトでは、カスタム WinRT XAML コントロールはサポートされていません。

    1. **ソリューション エクスプローラー** で、ソリューション ノードを右クリックし、 **[プロパティ]**  ->  **[構成プロパティ]**  ->  **[構成マネージャー]** を選択します。
    2. **[アクティブ ソリューション プラットフォーム]** で、 **[新規作成]** を選択します。 
    3. **[新しいソリューション プラットフォーム]** ダイアログで、 **[x64]** または **[x86]** を選択して、 **[OK]** をクリックします。 
    4. 開いているダイアログ ボックスを閉じます。

## <a name="define-a-xamlapplication-class-in-a-uwp-app-project"></a>UWP アプリ プロジェクトで XamlApplication クラスを定義する

次に、UWP アプリ プロジェクトをソリューションに追加し、このプロジェクトの既定の `App` クラスを、Windows Community Toolkit によって提供される [Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication) クラスから派生するように変更します。 このクラスでは、[IXamlMetadataProvider](/uwp/api/Windows.UI.Xaml.Markup.IXamlMetadataProvider) インターフェイスがサポートされています。これにより、アプリの実行時に、アプリケーションの現在のディレクトリにある、アセンブリ内のカスタム UWP XAML コントロールのメタデータを検出して読み込むことができるようになります。 このクラスでは、現在のスレッドの UWP XAML フレームワークも初期化されます。 

1. **ソリューション エクスプローラー** で、ソリューション ノードを右クリックし、 **[追加]**  ->  **[新しいプロジェクト]** を選択します。
2. ソリューションに **[空白のアプリ (ユニバーサル Windows)]** プロジェクトを追加します。 対象バージョンと最小バージョンの両方が **Windows 10 バージョン 1903 (ビルド 18362)** またはそれ以降のリリースに設定されていることを確認します。
3. UWP アプリ プロジェクトで、[Microsoft.Toolkit.Win32.UI.XamlApplication](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication) NuGet パッケージ (最新の安定バージョン) をインストールします。
4. **App.xaml** ファイルを開き、このファイルの内容を次の XAML に置き換えます。 `MyUWPApp` を、UWP アプリ プロジェクトの名前空間に置き換えます。

    ```xml
    <xaml:XamlApplication
        x:Class="MyUWPApp.App"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:xaml="using:Microsoft.Toolkit.Win32.UI.XamlHost"
        xmlns:local="using:MyUWPApp">
    </xaml:XamlApplication>
    ```

5. **App.xaml.cs** ファイルを開き、このファイルの内容を次のコードに置き換えます。 `MyUWPApp` を、UWP アプリ プロジェクトの名前空間に置き換えます。

    ```csharp
    namespace MyUWPApp
    {
        public sealed partial class App : Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication
        {
            public App()
            {
                this.Initialize();
            }
        }
    }
    ```

6. UWP アプリ プロジェクトから **MainPage.xaml** ファイルを削除します。
7. UWP アプリ プロジェクトをクリーンアップしてからビルドします。

## <a name="add-a-reference-to-the-uwp-project-in-your-wpf-project"></a>WPF プロジェクトで、UWP プロジェクトへの参照を追加します

1. WPF プロジェクト ファイルで、互換性のあるフレームワーク バージョンを指定します。 

    1. **ソリューション エクスプローラー** で、WPF プロジェクト ノードをダブルクリックして、エディターでプロジェクト ファイルを開きます。
    2. 最初の **[PropertyGroup]** 要素に、次の子要素を追加します。 必要に応じて値の `19041` という部分を変更し、UWP プロジェクトのターゲットおよび最小 OS ビルドに一致させます。

        ```xml
        <AssetTargetFallback>uap10.0.19041</AssetTargetFallback>
        ```

        これを完了すると、 **[PropertyGroup]** 要素は以下の例のようになります。

        ```xml
        <PropertyGroup>
            <OutputType>WinExe</OutputType>
            <TargetFramework>netcoreapp3.1</TargetFramework>
            <UseWPF>true</UseWPF>
            <Platforms>AnyCPU;x64</Platforms>
            <AssetTargetFallback>uap10.0.19041</AssetTargetFallback>
        </PropertyGroup>
        ```

2. **ソリューション エクスプローラー** で、WPF プロジェクトの下の **[依存関係]** ノードを右クリックし、UWP アプリ プロジェクトへの参照を追加します。

## <a name="instantiate-the-xamlapplication-object-in-the-entry-point-of-your-wpf-app"></a>WPF アプリのエントリ ポイントで XamlApplication オブジェクトをインスタンス化する

次に、先に UWP プロジェクトで定義した `App` クラス (これは、`XamlApplication` から派生するようになったクラスです) のインスタンスを作成するためのコードを、WPF アプリのエントリ ポイントに追加します。

1. WPF プロジェクトでプロジェクト ノードを右クリックし、 **[追加]**  ->  **[新しい項目]** を選択して、 **[クラス]** を選択します。 クラスに **Program** という名前を指定し、 **[追加]** をクリックします。

2. 生成された `Program` クラスを次のコードに置き換えて、ファイルを保存します。 `MyUWPApp` を UWP アプリ プロジェクトの名前空間に置き換え、`MyWPFApp` を WPF アプリ プロジェクトの名前空間に置き換えます。

    ```csharp
    public class Program
    {
        [System.STAThreadAttribute()]
        public static void Main()
        {
            using (new MyUWPApp.App())
            {
                MyWPFApp.App app = new MyWPFApp.App();
                app.InitializeComponent();
                app.Run();
            }
        }
    }
    ```

3. プロジェクト ノードを右クリックし、 **[プロパティ]** を選択します。

4. プロパティの **[アプリケーション]** タブで、 **[スタートアップ オブジェクト]** ドロップダウンをクリックし、前のステップで追加した `Program` クラスの完全修飾名を選択します。 
    > [!NOTE]
    > 既定の WPF プロジェクトで生成されるコード ファイルでは、変更を意図されていない `Main` エントリ ポイント関数が定義されています。 このステップでは、プロジェクトのエントリ ポイントを新しい `Program` クラスの `Main` メソッドに変更します。これにより、アプリのスタートアップ プロセスの可能な限り早い段階で実行されるコードを追加できるようになります。 

5. プロジェクトのプロパティへの変更を保存します。

## <a name="create-a-custom-winrt-xaml-control"></a>カスタム WinRT XAML コントロールの作成

WPF アプリでカスタム WinRT XAML コントロールをホストするには、アプリと共にコンパイルできるように、コントロールのソース コードを用意する必要があります。 通常、カスタム コントロールは、簡単に移植できるよう、UWP クラス ライブラリ プロジェクトで定義します。

このセクションでは、新しいクラス ライブラリ プロジェクトで簡単なカスタム コントロールを定義します。 代わりに、前のセクションで作成した UWP アプリ プロジェクトでカスタム コントロールを定義することもできます。 ただし、以下の手順では、方法を説明するために別のクラス ライブラリ プロジェクトでこれを行います。これが、移植性の高いカスタム コントロールを実装する一般的な方法です。

カスタム コントロールが既にある場合は、ここで示されているコントロールの代わりに使用してかまいません。 ただし、その場合でも、次の手順で示すように、コントロールを含むプロジェクトを構成する必要があります。

1. **ソリューション エクスプローラー** で、ソリューション ノードを右クリックし、 **[追加]**  ->  **[新しいプロジェクト]** を選択します。
2. ソリューションに **クラス ライブラリ (ユニバーサル Windows)** プロジェクトを追加します。 ターゲット バージョンと最小バージョンが両方とも、UWP プロジェクトと同じターゲットおよび最小 OS ビルドに設定されていることを確認します。
3. プロジェクト ファイルを右クリックし、 **[プロジェクトのアンロード]** を選択します。 プロジェクト ファイルをもう一度右クリックして、 **[編集]** を選択します。
4. 終了要素 `</Project>` の前に、いくつかのプロパティを無効にする次の XML を追加して、プロジェクト ファイルを保存します。 WPF (または Windows フォーム) アプリでカスタム コントロールをホストするには、これらのプロパティを有効にする必要があります。

    ```xml
    <PropertyGroup>
      <EnableTypeInfoReflection>false</EnableTypeInfoReflection>
      <EnableXBindDiagnostics>false</EnableXBindDiagnostics>
    </PropertyGroup>
    ```

5. プロジェクト ファイルを右クリックし、 **[プロジェクトの再読み込み]** を選択します。
6. 既定の **Class1.cs** ファイルを削除し、新しい **ユーザー コントロール** 項目をプロジェクトに追加します。
7. ユーザー コントロールの XAML ファイルで、次の `StackPanel` を既定の `Grid` の子として追加します。 この例では、``TextBlock`` コントロールを追加した後、そのコントロールの ``Text`` 属性を ``XamlIslandMessage`` フィールドにバインドします。

    ```xml
    <StackPanel Background="LightCoral">
        <TextBlock>This is a simple custom WinRT XAML control</TextBlock>
        <Rectangle Fill="Blue" Height="100" Width="100"/>
        <TextBlock Text="{x:Bind XamlIslandMessage}" FontSize="50"></TextBlock>
    </StackPanel>
    ```

8. ユーザー コントロールの分離コード ファイルで、次に示すように、ユーザー コントロール クラスに `XamlIslandMessage` フィールドを追加します。

    ```csharp
    public sealed partial class MyUserControl : UserControl
    {
        public string XamlIslandMessage { get; set; }

        public MyUserControl()
        {
            this.InitializeComponent();
        }
    }
    ```

9. UWP クラス ライブラリ プロジェクトをビルドします。
10. WPF プロジェクトで、 **[依存関係]** ノードを右クリックし、UWP クラス ライブラリ プロジェクトへの参照を追加します。
11. 前に構成した WPF アプリ プロジェクトで、 **[参照]** ノードを右クリックし、UWP クラス ライブラリ プロジェクトへの参照を追加します。
12. ソリューション全体をリビルドし、すべてのプロジェクトが正常にビルドされることを確認します。

## <a name="host-the-custom-winrt-xaml-control-in-your-wpf-app"></a>WPF アプリでカスタム WinRT XAML コントロールをホストする

1. **ソリューション エクスプローラー** で、WPF プロジェクトを展開し、カスタム コントロールをホストする MainWindow.xaml ファイルまたは他のウィンドウを開きます。
2. その XAML ファイルで、次の名前空間宣言を `<Window>` 要素に追加します。

    ```xml
    xmlns:xaml="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost"
    ```

3. 同じファイルで、次のコントロールを `<Grid>` 要素に追加します。 `InitialTypeName` 属性を、UWP クラス ライブラリ プロジェクトでのユーザー コントロールの完全修飾名に変更します。

    ```xml
    <xaml:WindowsXamlHost InitialTypeName="UWPClassLibrary.MyUserControl" ChildChanged="WindowsXamlHost_ChildChanged" />
    ```

4. 分離コード ファイルを開き、次のコードを `Window` クラスに追加します。 このコードは、UWP カスタム コントロールの ``XamlIslandMessage`` フィールドの値を WPF アプリの `WPFMessage` フィールドの値に代入する `ChildChanged` イベント ハンドラーが定義されています。 `UWPClassLibrary.MyUserControl` を、UWP クラス ライブラリ プロジェクトでのユーザー コントロールの完全修飾名に変更します。

    ```csharp
    private void WindowsXamlHost_ChildChanged(object sender, EventArgs e)
    {
        // Hook up x:Bind source.
        global::Microsoft.Toolkit.Wpf.UI.XamlHost.WindowsXamlHost windowsXamlHost =
            sender as global::Microsoft.Toolkit.Wpf.UI.XamlHost.WindowsXamlHost;
        global::UWPClassLibrary.MyUserControl userControl =
            windowsXamlHost.GetUwpInternalObject() as global::UWPClassLibrary.MyUserControl;

        if (userControl != null)
        {
            userControl.XamlIslandMessage = this.WPFMessage;
        }
    }

    public string WPFMessage
    {
        get
        {
            return "Binding from WPF to UWP XAML";
        }
    }
    ```

6. アプリをビルドして実行し、UWP ユーザー コントロールが想定どおりに表示されることを確認します。

## <a name="add-a-control-from-the-winui-2x-library-to-the-custom-control"></a>WinUI 2.x ライブラリのコントロールをカスタム コントロールに追加する

従来、WinRT XAML コントロールは Windows 10 OS の一部としてリリースされ、開発者は Windows SDK を通じてそれを使用できました。 [WinUI ライブラリ](/uwp/toolkits/winui/) はそれに代わる方法であり、Windows SDK の WinRT XAML コントロールの更新バージョンが、Windows SDK のリリースに関連付けられていない NuGet パッケージで配布されます。 また、このライブラリには、Windows SDK および既定の UWP プラットフォームの一部ではない新しいコントロールも含まれています。 詳しくは、[WinUI ライブラリのロードマップ](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md)をご覧ください。

このセクションでは、WinUI 2.x ライブラリからユーザー コントロールに WinRT XAML コントロールを追加する方法について説明します。

> [!NOTE]
> 現在、XAML Islands では、WinUI 2.x ライブラリのコントロールのホストのみがサポートされています。 WinUI 3 ライブラリのコントロールのホストは、今後のリリースでサポートされる予定です。

1. UWP アプリ プロジェクトで、最新のリリース バージョンまたはプレリリース バージョンの [Microsoft.UI.Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml) NuGet パッケージをインストールします。

    > [!NOTE]
    > お使いのデスクトップ アプリが [MSIX パッケージ](/windows/msix)にパッケージ化されている場合は、[Microsoft.UI.Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml) NugGet パッケージのプレリリース バージョンまたはリリース バージョンのいずれかを使用できます。 お使いのデスクトップ アプリが MSIX を使用してパッケージ化されていない場合は、プレリリース バージョンの [Microsoft.UI.Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml) NuGet パッケージをインストールする必要があります。

2. このプロジェクトの App.xaml ファイルで、次の子要素を `<xaml:XamlApplication>` 要素に追加します。

    ```xml
    <Application.Resources>
        <XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls" />
    </Application.Resources>
    ```

    この要素を追加した後、このファイルの内容は次のようになります。

    ```xml
    <xaml:XamlApplication
        x:Class="MyUWPApp.App"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:xaml="using:Microsoft.Toolkit.Win32.UI.XamlHost"
        xmlns:local="using:MyUWPApp">
        <Application.Resources>
            <XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls" />
        </Application.Resources>
    </xaml:XamlApplication>
    ```

3. UWP クラス ライブラリ プロジェクトで、最新バージョンの [Microsoft.UI.Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml) NuGet パッケージ (UWP アプリ プロジェクトにインストールしたものと同じバージョン) をインストールします。

4. 同じプロジェクトで、ユーザー コントロールの XAML ファイルを開き、次の名前空間宣言を `<UserControl>` 要素に追加します。

    ```xml
    xmlns:winui="using:Microsoft.UI.Xaml.Controls"
    ```

5. 同じファイルで、`<StackPanel>` の子として `<winui:RatingControl />` 要素を追加します。 この要素により、WinUI ライブラリの [RatingControl](/uwp/api/microsoft.ui.xaml.controls.ratingcontrol) クラスのインスタンスが追加されます。 この要素を追加した後の `<StackPanel>` は、次のようになります。

    ```xml
    <StackPanel Background="LightCoral">
        <TextBlock>This is a simple custom WinRT XAML control</TextBlock>
        <Rectangle Fill="Blue" Height="100" Width="100"/>
        <TextBlock Text="{x:Bind XamlIslandMessage}" FontSize="50"></TextBlock>
        <winui:RatingControl />
    </StackPanel>
    ```

6. アプリをビルドして実行し、新しい評価コントロールが想定どおりに表示されることを確認します。

## <a name="package-the-app"></a>アプリをパッケージ化する

必要に応じて、配置用に WPF アプリを [MSIX パッケージ](/windows/msix)にパッケージ化することもできます。 MSIX は、Windows 向けの最新のアプリ パッケージ化テクノロジであり、MSI、.appx、App-V、ClickOnce インストールの各テクノロジの組み合わせが基になっています。

次の手順では、Visual Studio 2019 で [Windows アプリケーション パッケージ プロジェクト](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)を使用して、ソリューションのすべてのコンポーネントを MSIX パッケージにパッケージ化する方法について説明します。 これらの手順は、WPF アプリを MSIX パッケージにパッケージ化する場合にのみ必要です。 

> [!NOTE]
> 配置用に [MSIX パッケージ](/windows/msix)にアプリケーションをパッケージ化しない場合は、アプリを実行するコンピューターに [Visual C++ ランタイム](https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads)がインストールされている必要があります。

1. ソリューションに新しい [Windows アプリケーション パッケージ プロジェクト](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)を追加します。 プロジェクトを作成するときに、UWP プロジェクトに選択したのと同じ **ターゲット バージョン** と **最小バージョン** を選択します。

2. パッケージ プロジェクトで、 **[アプリケーション]** ノードを右クリックして **[参照の追加]** を選択します。 プロジェクトの一覧でソリューション内の WPF プロジェクトを選択し、 **[OK]** をクリックします。

    > [!NOTE]
    > Microsoft Store でアプリを公開したい場合は、パッケージ プロジェクトに UWP プロジェクトへの参照を追加する必要があります。

3. x86 や x64 などの特定のプラットフォームを対象とするようにソリューションを構成します。 Windows アプリケーション パッケージ プロジェクトを使用して MSIX パッケージに WPF アプリをビルドするには、このようにする必要があります。

    1. **ソリューション エクスプローラー** で、ソリューション ノードを右クリックし、 **[プロパティ]**  ->  **[構成プロパティ]**  ->  **[構成マネージャー]** を選択します。
    2. **[アクティブ ソリューション プラットフォーム]** で、 **[x64]** または **[x86]** を選択します。
    3. WPF プロジェクトの行の **[プラットフォーム]** 列で、 **[新規]** を選択します。
    4. **[新しいソリューション プラットフォーム]** ダイアログで、 **[x64]** または **[x86]** ( **[アクティブ ソリューション プラットフォーム]** に対して選択したものと同じプラットフォーム) を選択し、 **[OK]** をクリックします。
    5. 開いているダイアログ ボックスを閉じます。

4. パッケージ プロジェクトをビルドして実行します。 WPF が実行され、UWP カスタム コントロールが想定どおりに表示されることを確認します。

## <a name="related-topics"></a>関連トピック

* [デスクトップ アプリで UWP XAML コントロールをホストする (XAML Islands)](xaml-islands.md)
* [XAML Islands コード サンプル](https://github.com/microsoft/Xaml-Islands-Samples)
* [WindowsXamlHost](/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)
