---
description: この記事では、XAML アイランドを使用して WPF アプリでカスタム UWP コントロールをホストする方法について説明します。
title: XAML アイランドを使用した WPF アプリでのカスタム UWP コントロールのホスト
ms.date: 01/24/2020
ms.topic: article
keywords: windows 10、uwp、windows フォーム、wpf、xaml アイランド、カスタムコントロール、ユーザーコントロール、ホストコントロール
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: b6bd46bcdec639cee2bc867c2c4e71cccbb13cfb
ms.sourcegitcommit: a2a3c887f6da47a6638ce5286199ea31ee7780e4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/25/2020
ms.locfileid: "77606692"
---
# <a name="host-a-custom-uwp-control-in-a-wpf-app-using-xaml-islands"></a>XAML アイランドを使用した WPF アプリでのカスタム UWP コントロールのホスト

この記事では、Windows Community Toolkit の[Windowsxamlhost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)コントロールを使用して、.net Core 3 を対象とする WPF アプリでカスタム UWP コントロールをホストする方法について説明します。 カスタムコントロールには、Windows SDK からのいくつかのファーストパーティ UWP コントロールが含まれており、UWP コントロールの1つのプロパティを WPF アプリの文字列にバインドします。 この記事では、 [WinUI ライブラリ](https://docs.microsoft.com/uwp/toolkits/winui/)からファーストパーティの UWP コントロールをホストする方法についても説明します。

この記事では、WPF アプリでこれを行う方法について説明しますが、プロセスは Windows フォームアプリに似ています。 WPF と Windows フォームアプリで UWP コントロールをホストする方法の概要については、こちらの[記事](xaml-islands.md#wpf-and-windows-forms-applications)を参照してください。

## <a name="required-components"></a>必要なコンポーネント

WPF (または Windows フォーム) アプリでカスタム UWP コントロールをホストするには、ソリューションに次のコンポーネントが必要です。 この記事では、これらの各コンポーネントを作成する手順について説明します。

* **アプリのプロジェクトとソースコード**。 [Windowsxamlhost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)コントロールを使用してカスタム UWP コントロールをホストすることは、.net Core 3 を対象とするアプリでのみサポートされています。 このシナリオは、.NET Framework を対象とするアプリではサポートされていません。

* **カスタム UWP コントロール**。 アプリを使用してコンパイルできるように、ホストするカスタム UWP コントロールのソースコードが必要になります。 通常、カスタムコントロールは、WPF または Windows フォームプロジェクトと同じソリューションで参照する UWP クラスライブラリプロジェクトで定義されます。

* **Xamlapplication から派生するルートアプリケーションクラスを定義する UWP アプリプロジェクト**。 WPF または Windows フォームプロジェクトは、Windows Community Toolkit によって提供される、Microsoft の Toolkit... [UI. Xamlhost](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication)クラスのインスタンスにアクセスできる必要があります。 このオブジェクトは、アプリケーションの現在のディレクトリにあるアセンブリ内のカスタム UWP XAML 型のメタデータを読み込むためのルートメタデータプロバイダーとして機能します。

    これを行うには、WPF または Windows フォームプロジェクトと同じソリューションに**空のアプリ (ユニバーサル Windows)** プロジェクトを追加し、`XamlApplication`から派生するようにこのプロジェクトの既定の `App` クラスを変更してから、アプリのエントリポイントコードにこのオブジェクトのインスタンスを作成することをお勧めします。

    > [!NOTE]
    > ソリューションには、`XamlApplication` オブジェクトを定義するプロジェクトを1つだけ含めることができます。 アプリ内のすべてのカスタム UWP コントロールは、同じ `XamlApplication` オブジェクトを共有します。 `XamlApplication` オブジェクトを定義するプロジェクトには、XAML アイランドの UWP コントロールにホストされる他のすべての UWP ライブラリおよびプロジェクトへの参照が含まれている必要があります。

## <a name="create-a-wpf-project"></a>WPF プロジェクトを作成する

作業を開始する前に、次の手順に従って WPF プロジェクトを作成し、XAML アイランドをホストするように構成します。 既存の WPF プロジェクトがある場合は、プロジェクトのこれらの手順とコード例を調整できます。

> [!NOTE]
> .NET Framework を対象とする既存のプロジェクトがある場合は、.NET Core 3 にプロジェクトを移行する必要があります。 詳細については、[このブログシリーズ](https://devblogs.microsoft.com/dotnet/migrating-a-sample-wpf-app-to-net-core-3-part-1/)を参照してください。

1. [.Net Core 3 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0)の最新バージョンをまだインストールしていない場合は、インストールします。

2. Visual Studio 2019 で、新しい**WPF アプリ (.Net Core)** プロジェクトを作成します。

3. [パッケージ参照](https://docs.microsoft.com/nuget/consume-packages/package-references-in-project-files)が有効になっていることを確認します。

    1. Visual Studio で、ツール、 **NuGet パッケージマネージャー-> パッケージマネージャーの設定** の順にクリックし > ます。
    2. **既定のパッケージ管理形式**として**PackageReference**が選択されていることを確認します。

4. **ソリューションエクスプローラー**で WPF プロジェクトを右クリックし、 **[NuGet パッケージの管理]** を選択します。

5. **[NuGet パッケージマネージャー]** ウィンドウで、 **[プレリリースを含める]** が選択されていることを確認します。

6. **[参照]** タブを選択し、6.0.0 またはそれ[以降のバージョンのパッケージを](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.XamlHost)検索して、パッケージをインストールします。 このパッケージには、他の関連する NuGet パッケージを含め、UWP コントロールをホストするために**Windowsxamlhost**コントロールを使用するために必要なすべてのものが用意されています。
    > [!NOTE]
    > Windows フォームアプリでは、 [6.0.0 パッケージ (](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.XamlHost)バージョン v1.0 以降) を使用する必要があります。

7. X86 や x64 などの特定のプラットフォームを対象とするようにソリューションを構成します。 カスタム UWP コントロールは **、任意の CPU**を対象とするプロジェクトではサポートされていません。

    1. **ソリューションエクスプローラー**で、ソリューション ノードを右クリックし、**プロパティ** -> **構成プロパティ** -> **Configuration Manager** の順に選択します。
    2. **[アクティブソリューションプラットフォーム]** で、 **[新規]** を選択します。 
    3. **[新しいソリューションプラットフォーム]** ダイアログボックスで、 **[x64]** または **[x86]** を選択し、[ **OK]** をクリックします。 
    4. 開いているダイアログボックスを閉じます。

## <a name="define-a-xamlapplication-class-in-a-uwp-app-project"></a>UWP アプリプロジェクトでの XamlApplication クラスの定義

次に、WPF プロジェクトと同じソリューションに UWP アプリプロジェクトを追加します。 このプロジェクトの既定の `App` クラスを修正して、Windows Community Toolkit によって提供される、 [Microsoft の toolkit](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication) .........................。 このクラスの目的の詳細については、[このセクション](#required-components)を参照してください。

1. **ソリューションエクスプローラー**で、[ソリューション] ノードを右クリックし、[ -> **新しいプロジェクト**の**追加**] を選択します。
2. ソリューションに **[空白のアプリ (ユニバーサル Windows)]** プロジェクトを追加します。 ターゲットバージョンと最小バージョンの両方が**Windows 10 バージョン 1903**以降に設定されていることを確認します。
3. UWP アプリプロジェクトで、 [6.0.0 NuGet パッケージ](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication)(バージョン v1.0 以降) をインストールします。このパッケージには、
4. **App.xaml**ファイルを開き、このファイルの内容を次の xaml に置き換えます。 `MyUWPApp` を、UWP アプリプロジェクトの名前空間に置き換えます。

    ```xml
    <xaml:XamlApplication
        x:Class="MyUWPApp.App"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:xaml="using:Microsoft.Toolkit.Win32.UI.XamlHost"
        xmlns:local="using:MyUWPApp">
    </xaml:XamlApplication>
    ```

5. **App.xaml.cs**ファイルを開き、このファイルの内容を次のコードに置き換えます。 `MyUWPApp` を、UWP アプリプロジェクトの名前空間に置き換えます。

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

6. UWP アプリプロジェクトから**mainpage.xaml**ファイルを削除します。
7. UWP アプリプロジェクトをクリーンアップし、ビルドします。
8. WPF プロジェクトで、 **[依存関係]** ノードを右クリックし、UWP アプリプロジェクトへの参照を追加します。

## <a name="instantiate-the-xamlapplication-object-in-the-entry-point-of-your-wpf-app"></a>WPF アプリのエントリポイントで XamlApplication オブジェクトをインスタンス化する

次に、WPF アプリのエントリポイントにコードを追加して、UWP プロジェクトで定義した `App` クラスのインスタンスを作成します (これは、`XamlApplication`から派生したクラスです)。 このオブジェクトの目的の詳細については、[このセクション](#required-components)を参照してください。

1. WPF プロジェクトで、プロジェクトノードを右クリックし、[**新しい項目**の**追加** -> ]、 **[クラス]** の順に選択します。 クラスに「 **Program** 」という名前を指定し、 **[追加]** をクリックします。

2. 生成された `Program` クラスを次のコードに置き換えて、ファイルを保存します。 `MyUWPApp` を UWP アプリプロジェクトの名前空間に置き換え、`MyWPFApp` を WPF アプリプロジェクトの名前空間に置き換えます。

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

3. プロジェクトノードを右クリックし、 **[プロパティ]** を選択します。

4. プロパティの **[アプリケーション]** タブで、 **[スタートアップオブジェクト]** ドロップダウンをクリックし、前の手順で追加した `Program` クラスの完全修飾名を選択します。 
    > [!NOTE]
    > 既定では、WPF プロジェクトは、変更を意図していない生成されたコードファイルに `Main` エントリポイント関数を定義します。 この手順では、プロジェクトのエントリポイントを新しい `Program` クラスの `Main` メソッドに変更します。これにより、アプリのスタートアッププロセスの早い段階で実行されるコードを追加できるようになります。 

5. プロジェクトのプロパティへの変更を保存します。

## <a name="create-a-custom-uwp-control"></a>カスタム UWP コントロールを作成する

WPF アプリでカスタム UWP コントロールをホストするには、アプリでコンパイルできるように、コントロールのソースコードが必要です。 通常、カスタムコントロールは、簡単な移植性を確保するために UWP クラスライブラリプロジェクトで定義されています。

このセクションでは、新しいクラスライブラリプロジェクトで単純なカスタム UWP コントロールを定義します。 または、前のセクションで作成した UWP アプリプロジェクトでカスタム UWP コントロールを定義することもできます。 ただし、この手順では、この方法を説明するために別のクラスライブラリプロジェクトで行います。これは、通常、カスタムコントロールを移植性のために実装する方法です。

カスタムコントロールが既にある場合は、ここに表示されているコントロールの代わりに使用できます。 ただし、次の手順に示すように、コントロールを含むプロジェクトを構成する必要があります。

1. **ソリューションエクスプローラー**で、[ソリューション] ノードを右クリックし、[ -> **新しいプロジェクト**の**追加**] を選択します。
2. ソリューションに**クラスライブラリ (ユニバーサル Windows)** プロジェクトを追加します。 ターゲットバージョンと最小バージョンの両方が**Windows 10 バージョン 1903**以降に設定されていることを確認します。
3. プロジェクトファイルを右クリックし、 **[プロジェクトのアンロード]** を選択します。 プロジェクトファイルをもう一度右クリックし、 **[編集]** を選択します。
4. 終了 `</Project>` 要素の前に、次の XML を追加して、いくつかのプロパティを無効にし、プロジェクトファイルを保存します。 WPF (または Windows フォーム) アプリでカスタム UWP コントロールをホストするには、これらのプロパティを有効にする必要があります。

    ```xml
    <PropertyGroup>
      <EnableTypeInfoReflection>false</EnableTypeInfoReflection>
      <EnableXBindDiagnostics>false</EnableXBindDiagnostics>
    </PropertyGroup>
    ```

5. プロジェクトファイルを右クリックし、 **[プロジェクトの再読み込み]** をクリックします。
6. 既定の**Class1.cs**ファイルを削除し、新しい**ユーザーコントロール**項目をプロジェクトに追加します。
7. ユーザーコントロールの XAML ファイルで、次の `StackPanel` を既定の `Grid`の子として追加します。 この例では、``TextBlock`` コントロールを追加し、そのコントロールの ``Text`` 属性を ``XamlIslandMessage`` フィールドにバインドします。

    ```xml
    <StackPanel Background="LightCoral">
        <TextBlock>This is a simple custom UWP control</TextBlock>
        <Rectangle Fill="Blue" Height="100" Width="100"/>
        <TextBlock Text="{x:Bind XamlIslandMessage}" FontSize="50"></TextBlock>
    </StackPanel>
    ```

8. ユーザーコントロールの分離コードファイルで、次に示すように、ユーザーコントロールクラスに `XamlIslandMessage` フィールドを追加します。

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

9. UWP クラスライブラリプロジェクトをビルドします。
10. WPF プロジェクトで、 **[依存関係]** ノードを右クリックし、UWP クラスライブラリプロジェクトへの参照を追加します。
11. 前に構成した UWP アプリプロジェクトで、 **[参照設定]** ノードを右クリックし、uwp クラスライブラリプロジェクトへの参照を追加します。
12. ソリューション全体をリビルドし、すべてのプロジェクトが正常にビルドされることを確認します。

## <a name="host-the-custom-uwp-control-in-your-wpf-app"></a>WPF アプリでカスタム UWP コントロールをホストする

1. **ソリューションエクスプローラー**で、WPF プロジェクトを展開し、mainwindow.xaml ファイル、またはカスタムコントロールをホストするその他のウィンドウを開きます。
2. XAML ファイルで、次の名前空間宣言を `<Window>` 要素に追加します。

    ```xml
    xmlns:xaml="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost"
    ```

3. 同じファイルで、次のコントロールを `<Grid>` 要素に追加します。 `InitialTypeName` 属性を、UWP クラスライブラリプロジェクトのユーザーコントロールの完全修飾名に変更します。

    ```xml
    <xaml:WindowsXamlHost InitialTypeName="UWPClassLibrary.MyUserControl" ChildChanged="WindowsXamlHost_ChildChanged" />
    ```

4. 分離コードファイルを開き、次のコードを `Window` クラスに追加します。 このコードは、UWP カスタムコントロールの ``XamlIslandMessage`` フィールドの値を WPF アプリの `WPFMessage` フィールドの値に割り当てる `ChildChanged` イベントハンドラーを定義します。 `UWPClassLibrary.MyUserControl` を、UWP クラスライブラリプロジェクトのユーザーコントロールの完全修飾名に変更します。

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

6. アプリをビルドして実行し、UWP ユーザーコントロールが想定どおりに表示されることを確認します。

## <a name="add-a-control-from-the-winui-library-to-the-custom-control"></a>コントロールを WinUI ライブラリからカスタムコントロールに追加する

従来、UWP コントロールは Windows 10 OS の一部としてリリースされ、Windows SDK を通じて開発者が使用できるようになりました。 [WinUI ライブラリ](https://docs.microsoft.com/uwp/toolkits/winui/)は、Windows SDK からのファーストパーティ UWP コントロールの更新バージョンが Windows SDK リリースに関連付けられていない NuGet パッケージで配布される、別の方法です。 このライブラリには、Windows SDK と既定の UWP プラットフォームの一部ではない新しいコントロールも含まれています。 詳細については、「 [WinUI ライブラリのロードマップ](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md)」を参照してください。

このセクションでは、WPF アプリでこのコントロールをホストできるように、WinUI ライブラリからユーザーコントロールに UWP コントロールを追加する方法について説明します。

1. UWP アプリプロジェクトで、最新バージョンの[UI](https://www.nuget.org/packages/Microsoft.UI.Xaml) NuGet パッケージをインストールします。

2. このプロジェクトの app.xaml ファイルで、次の子要素を `<xaml:XamlApplication>` 要素に追加します。

    ```xml
    <Application.Resources>
        <XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls" />
    </Application.Resources>
    ```

    この要素を追加すると、このファイルの内容は次のようになります。

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

3. UWP クラスライブラリプロジェクトで、最新バージョンの[MICROSOFT UI](https://www.nuget.org/packages/Microsoft.UI.Xaml) (uwp アプリプロジェクトにインストールしたものと同じバージョン) の NuGet パッケージをインストールします。

4. 同じプロジェクトで、ユーザーコントロールの XAML ファイルを開き、次の名前空間宣言を `<UserControl>` 要素に追加します。

    ```xml
    xmlns:winui="using:Microsoft.UI.Xaml.Controls"
    ```

5. 同じファイル内で、`<StackPanel>`の子として `<winui:RatingControl />` 要素を追加します。 この要素は、WinUI ライブラリから[RatingControl](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.ratingcontrol?view=winui-2.2)クラスのインスタンスを追加します。 この要素を追加すると、`<StackPanel>` は次のようになります。

    ```xml
    <StackPanel Background="LightCoral">
        <TextBlock>This is a simple custom UWP control</TextBlock>
        <Rectangle Fill="Blue" Height="100" Width="100"/>
        <TextBlock Text="{x:Bind XamlIslandMessage}" FontSize="50"></TextBlock>
        <winui:RatingControl />
    </StackPanel>
    ```

6. アプリをビルドして実行し、新しい評価コントロールが想定どおりに表示されることを確認します。

## <a name="package-the-app"></a>アプリのパッケージ化

必要に応じて、WPF アプリを[Msix パッケージ](https://docs.microsoft.com/windows/msix)にパッケージ化してデプロイすることもできます。 MSIX は Windows 向けの最新のアプリケーションパッケージ化テクノロジであり、MSI、.appx、App-v、ClickOnce のインストールテクノロジの組み合わせに基づいています。

次の手順では、Visual Studio 2019 の[Windows アプリケーションパッケージプロジェクト](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)を使用して、msix パッケージのソリューションに含まれるすべてのコンポーネントをパッケージ化する方法を示します。 これらの手順は、WPF アプリを MSIX パッケージでパッケージ化する場合にのみ必要です。 これらの手順には、現在、カスタム UWP コントロールをホストするシナリオに固有のいくつかの回避策が含まれていることに注意してください。

1. 新しい[Windows アプリケーションパッケージプロジェクト](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)をソリューションに追加します。 プロジェクトを作成するときに、 **Windows 10 バージョン 1903 (10.0;ビルド 18362)** 。**ターゲットバージョン**と**最小バージョン**の両方に対応します。

2. パッケージプロジェクトで、 **[アプリケーション]** ノードを右クリックし、 **[参照の追加]** を選択します。 プロジェクトの一覧で、ソリューション内の WPF プロジェクトを選択し、[ **OK]** をクリックします。

3. WPF プロジェクトファイルを編集します。 これらの変更は、現在、カスタム UWP コントロールをホストする WPF アプリのパッケージ化に必要です。

    1. ソリューションエクスプローラーで、WPF プロジェクト ノードを右クリックし、**プロジェクトのアンロード** を選択します。
    2. WPF プロジェクトノードを右クリックし、 **[編集]** を選択します。
    3. ファイルで最後の `</PropertyGroup>` 終了タグを探し、そのタグの直後に次の XML を追加します。

        ``` xml
        <PropertyGroup>
          <AssetTargetFallback>uap10.0.18362</AssetTargetFallback>
        </PropertyGroup>
        ```

    4. プロジェクト ファイルを保存して閉じます。
    5. WPF プロジェクトノードを右クリックし、 **[プロジェクトの再読み込み]** をクリックします。

4. パッケージプロジェクトをビルドして実行します。 WPF が実行され、UWP カスタムコントロールが想定どおりに表示されることを確認します。

## <a name="related-topics"></a>関連トピック

* [デスクトップアプリでの UWP XAML コントロールのホスト (XAML アイランド)](xaml-islands.md)
* [XAML アイランドコードサンプル](https://github.com/microsoft/Xaml-Islands-Samples)
* [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)
