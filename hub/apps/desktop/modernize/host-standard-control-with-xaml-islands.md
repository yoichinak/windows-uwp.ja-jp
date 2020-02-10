---
description: この記事では、XAML アイランドを使用して WPF アプリで標準の UWP コントロールをホストする方法について説明します。
title: XAML アイランドを使用して WPF アプリで標準の UWP コントロールをホストする
ms.date: 01/24/2020
ms.topic: article
keywords: windows 10、uwp、windows フォーム、wpf、xaml islands、ラップされたコントロール、標準コントロール、System.windows.controls.inkcanvas>、InkToolbar
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 6000773e6ac25835552ea76d220c953de5bf5475
ms.sourcegitcommit: 3e7a4f7605dfb4e87bac2d10b6d64f8b35229546
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/08/2020
ms.locfileid: "77089358"
---
# <a name="host-a-standard-uwp-control-in-a-wpf-app-using-xaml-islands"></a>XAML アイランドを使用して WPF アプリで標準の UWP コントロールをホストする

この記事では、 [XAML アイランド](xaml-islands.md)を使用して WPF アプリで標準の uwp コントロール (つまり、Windows SDK または WinUI ライブラリによって提供されるファーストパーティ uwp コントロール) をホストする2つの方法について説明します。

* この例では、Windows Community Toolkit でラップされた[コントロール](xaml-islands.md#wrapped-controls)を使用して UWP [System.windows.controls.inkcanvas>](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)コントロールと[inktoolbar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar)コントロールをホストする方法を示します。 これらのコントロールは、少数の便利な UWP コントロールのインターフェイスと機能をラップします。 WPF または Windows フォームプロジェクトのデザイン画面に直接追加し、デザイナーの他の WPF や Windows フォームコントロールと同様に使用できます。

* また、Windows Community Toolkit の[Windowsxamlhost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)コントロールを使用して UWP [CalendarView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarView)コントロールをホストする方法についても説明します。 ラップされたコントロールとして使用できるのは、一部の UWP コントロールだけなので、 [Windowsxamlhost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)を使用して、その他の標準 uwp コントロールをホストできます。

この記事では、WPF アプリで UWP コントロールをホストする方法について説明しますが、プロセスは Windows フォームアプリに似ています。

## <a name="required-components"></a>必要なコンポーネント

WPF (または Windows フォーム) アプリで UWP コントロールをホストするには、ソリューションに次のコンポーネントが必要です。 この記事では、これらの各コンポーネントを作成する手順について説明します。

* **アプリのプロジェクトとソースコード**。 [Windowsxamlhost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)コントロールを使用して標準のファーストパーティ UWP コントロールをホストすることは、.NET Framework または .net Core 3 を対象とするアプリでサポートされています。

* **Xamlapplication から派生するルートアプリケーションクラスを定義する UWP アプリプロジェクト**。 WPF または Windows フォームプロジェクトは、Windows Community Toolkit によって提供される、Microsoft の Toolkit... [UI. Xamlhost](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication)クラスのインスタンスにアクセスできる必要があります。 このオブジェクトは、アプリケーションの現在のディレクトリにあるアセンブリ内のカスタム UWP XAML 型のメタデータを読み込むためのルートメタデータプロバイダーとして機能します。

    これを行うには、WPF または Windows フォームプロジェクトと同じソリューションに**空のアプリ (ユニバーサル Windows)** プロジェクトを追加し、`XamlApplication`から派生するようにこのプロジェクトの既定の `App` クラスを変更してから、アプリのエントリポイントコードにこのオブジェクトのインスタンスを作成することをお勧めします。

    > [!NOTE]
    > このコンポーネントは、ファーストパーティ UWP コントロールのホストなど、単純な XAML アイランドのシナリオには必要ありませんが、アプリでは、カスタム UWP コントロールをホストするなど、XAML アイランドのあらゆるシナリオをサポートするために、この `XamlApplication` オブジェクトが必要です。 したがって、XAML アイランドを使用するソリューションでは、常に `XamlApplication` オブジェクトを定義することをお勧めします。

    > [!NOTE]
    > ソリューションには、`XamlApplication` オブジェクトを定義するプロジェクトを1つだけ含めることができます。 アプリ内のすべてのカスタム UWP コントロールは、同じ `XamlApplication` オブジェクトを共有します。 `XamlApplication` オブジェクトを定義するプロジェクトには、XAML アイランドで UWP コントロールをホストするために使用される他のすべての UWP ライブラリおよびプロジェクトへの参照が含まれている必要があります。

## <a name="create-a-wpf-project"></a>WPF プロジェクトを作成する

作業を開始する前に、次の手順に従って WPF プロジェクトを作成し、XAML アイランドをホストするように構成します。 既存の WPF プロジェクトがある場合は、プロジェクトのこれらの手順とコード例を調整できます。

1. Visual Studio 2019 で、新しい**Wpf アプリ (.NET Framework)** プロジェクトまたは**wpf アプリ (.net Core)** プロジェクトを作成します。 **WPF アプリ (.Net core)** プロジェクトを作成する場合は、最初に[.NET core 3 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0)の最新バージョンをインストールする必要があります。

2. [パッケージ参照](https://docs.microsoft.com/nuget/consume-packages/package-references-in-project-files)が有効になっていることを確認します。

    1. Visual Studio で、ツール、 **NuGet パッケージマネージャー-> パッケージマネージャーの設定** の順にクリックし > ます。
    2. **既定のパッケージ管理形式**として**PackageReference**が選択されていることを確認します。

3. **ソリューションエクスプローラー**で WPF プロジェクトを右クリックし、 **[NuGet パッケージの管理]** を選択します。

4. **[NuGet パッケージマネージャー]** ウィンドウで、 **[プレリリースを含める]** が選択されていることを確認します。

5. **[参照]** タブを選択[し、6.0.0 パッケージ (](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls)バージョン v1.0 以降) を検索して、パッケージをインストールします。 このパッケージには、WPF のラップされた UWP コントロールを使用するために必要なすべてのものが用意されています ( [system.windows.controls.inkcanvas>](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) 、 [Inktoolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) 、 [windowsxamlhost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)コントロールなど)。
    > [!NOTE]
    > Windows フォームアプリでは、6.0.0 パッケージ (version v 以降[) を使用](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls)する必要があります。

6. X86 や x64 などの特定のプラットフォームを対象とするようにソリューションを構成します。 ほとんどの XAML アイランドのシナリオは **、任意の CPU**を対象とするプロジェクトではサポートされていません。

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
        sealed partial class App : Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication
        {
            public App()
            {
                this.Initialize();
            }
        }
    }
    ```

6. UWP アプリプロジェクトから**mainpage.xaml**ファイルを削除します。
7. UWP アプリプロジェクトをビルドします。
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

## <a name="host-an-inkcanvas-and-inktoolbar-by-using-wrapped-controls"></a>ラップされたコントロールを使用して System.windows.controls.inkcanvas> および InkToolbar をホストする

UWP XAML アイランドを使用するようにプロジェクトを構成したので、次は[system.windows.controls.inkcanvas>](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)と[inktoolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar)のラップされた uwp コントロールをアプリに追加する準備ができました。

1. **ソリューションエクスプローラー**で、 **mainwindow.xaml**ファイルを開きます。

2. XAML ファイルの先頭付近にある**ウィンドウ**要素に、次の属性を追加します。 これは、 [system.windows.controls.inkcanvas>](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)および[inktoolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar)ラップされた UWP コントロールの XAML 名前空間を参照します。

    ```xml
    xmlns:Controls="clr-namespace:Microsoft.Toolkit.Wpf.UI.Controls;assembly=Microsoft.Toolkit.Wpf.UI.Controls"
    ```

    この属性を追加すると、**ウィンドウ**要素は次のようになります。

    ```xml
    <Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
            xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
            xmlns:local="clr-namespace:WpfApp"
            xmlns:Controls="clr-namespace:Microsoft.Toolkit.Wpf.UI.Controls;assembly=Microsoft.Toolkit.Wpf.UI.Controls"
            x:Class="WpfApp.MainWindow"
            mc:Ignorable="d"
            Title="MainWindow" Height="800" Width="800">
    ```

3. **Mainwindow.xaml**ファイルで、既存の `<Grid>` 要素を次の xaml に置き換えます。 この XAML は、 [system.windows.controls.inkcanvas>](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)および[inktoolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar)コントロール (前に名前空間として定義した**コントロール**キーワードが先頭に付いている) を `<Grid>`に追加します。

    ```xml
    <Grid Margin="10,50,10,10">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="Auto"/>
        </Grid.RowDefinitions>
        <Controls:InkToolbar x:Name="myInkToolbar" TargetInkCanvas="{x:Reference myInkCanvas}" Grid.Row="0" Width="300"
            Height="50" Margin="10,10,10,10" HorizontalAlignment="Left" VerticalAlignment="Top" />
        <Controls:InkCanvas x:Name="myInkCanvas" Grid.Row="1" HorizontalAlignment="Left" Width="600" Height="400"
            Margin="10,10,10,10" VerticalAlignment="Top" />
    </Grid>
    ```

    > [!NOTE]
    > また、これらのコントロールやその他のラップされたコントロールを、**ツールボックス**の **[Windows Community Toolkit]** セクションからデザイナーにドラッグして、ウィンドウに追加することもできます。

4. **Mainwindow.xaml**ファイルを保存します。

    画面のようなデジタルペンをサポートするデバイスがあり、このラボを物理マシンで実行している場合は、アプリをビルドして実行し、ペンを使用して画面にデジタルインクを描画できます。 ただし、ペンに対応したデバイスを持っておらず、マウスでサインインしようとすると、何も起こりません。 この問題が発生するのは、既定ではデジタルペンに対してのみ**system.windows.controls.inkcanvas>** コントロールが有効になっているためです。 ただし、この動作は変更できます。

5. **MainWindow.xaml.cs**ファイルを開きます。

6. ファイルの先頭に次の名前空間宣言を追加します。

    ```csharp
    using Microsoft.Toolkit.Win32.UI.Controls.Interop.WinRT;
    ```

7. `MainWindow()` コンストラクターを見つけます。 `InitializeComponent()` メソッドの後に次のコード行を追加し、コードファイルを保存します。

    ```csharp
    this.myInkCanvas.InkPresenter.InputDeviceTypes = CoreInputDeviceTypes.Mouse | CoreInputDeviceTypes.Pen;
    ```

    **InkPresenter**オブジェクトを使用して、既定のインク操作をカスタマイズできます。 このコードでは、 **Inputdevicetypes**プロパティを使用して、マウスおよびペン入力を有効にします。

8. F5 キーをもう一度押して、デバッガーでアプリをリビルドして実行します。 マウスでコンピューターを使用している場合は、インクキャンバススペースにマウスを使用して何かを描画できることを確認します。

## <a name="host-a-calendarview-by-using-the-host-control"></a>ホストコントロールを使用して CalendarView をホストする

これで、 [system.windows.controls.inkcanvas>](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)と[inktoolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar)がラップされた UWP コントロールがアプリに追加されたので、 [windowsxamlhost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)コントロールを使用して、 [CalendarView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarView)をアプリに追加する準備ができました。

> [!NOTE]
> [Windowsxamlhost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)コントロールは、 [Microsoft の Toolkit. ui> ホスト](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.XamlHost)パッケージによって提供されます。 このパッケージは、前の手順でインストールし[たパッケージに](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls)含まれています。

1. **ソリューションエクスプローラー**で、 **mainwindow.xaml**ファイルを開きます。

2. XAML ファイルの先頭付近にある**ウィンドウ**要素に、次の属性を追加します。 これは、 [Windowsxamlhost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)コントロールの XAML 名前空間を参照します。

    ```xml
    xmlns:xamlhost="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost"
    ```

    この属性を追加すると、**ウィンドウ**要素は次のようになります。

    ```xml
    <Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
            xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
            xmlns:local="clr-namespace:WpfApp"
            xmlns:Controls="clr-namespace:Microsoft.Toolkit.Wpf.UI.Controls;assembly=Microsoft.Toolkit.Wpf.UI.Controls"
            xmlns:xamlhost="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost"
            x:Class="WpfApp.MainWindow"
            mc:Ignorable="d"
            Title="MainWindow" Height="800" Width="800">
    ```

4. **Mainwindow.xaml**ファイルで、既存の `<Grid>` 要素を次の xaml に置き換えます。 この XAML は、グリッドに行を追加し、最後の行に[Windowsxamlhost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)オブジェクトを追加します。 UWP [CalendarView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarView)コントロールをホストするために、この XAML は `InitialTypeName` プロパティをコントロールの完全修飾名に設定します。 この XAML は、ホストされるコントロールがレンダリングされたときに発生する `ChildChanged` イベントのイベントハンドラーも定義します。

    ```xml
    <Grid Margin="10,50,10,10">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="Auto"/>
        </Grid.RowDefinitions>
        <Controls:InkToolbar x:Name="myInkToolbar" TargetInkCanvas="{x:Reference myInkCanvas}" Grid.Row="0" Width="300"
            Height="50" Margin="10,10,10,10" HorizontalAlignment="Left" VerticalAlignment="Top" />
        <Controls:InkCanvas x:Name="myInkCanvas" Grid.Row="1" HorizontalAlignment="Left" Width="600" Height="400" 
            Margin="10,10,10,10" VerticalAlignment="Top" />
        <xamlhost:WindowsXamlHost x:Name="myCalendar" InitialTypeName="Windows.UI.Xaml.Controls.CalendarView" Grid.Row="2" 
              Margin="10,10,10,10" Width="600" Height="300" ChildChanged="MyCalendar_ChildChanged"  />
    </Grid>
    ```

5. **Mainwindow.xaml**ファイルを保存し、 **MainWindow.xaml.cs**ファイルを開きます。

7. ファイルの先頭に次の名前空間宣言を追加します。

    ```csharp
    using Microsoft.Toolkit.Wpf.UI.XamlHost;
    ```

10. 次の `ChildChanged` イベントハンドラーメソッドを `MainWindow` クラスに追加し、コードファイルを保存します。 ホストコントロールがレンダリングされると、このイベントハンドラーが実行され、カレンダーコントロールの `SelectedDatesChanged` イベントの単純なイベントハンドラーが作成されます。

    ```csharp
    private void MyCalendar_ChildChanged(object sender, EventArgs e)
    {
        WindowsXamlHost windowsXamlHost = (WindowsXamlHost)sender;

        Windows.UI.Xaml.Controls.CalendarView calendarView =
            (Windows.UI.Xaml.Controls.CalendarView)windowsXamlHost.Child;

        if (calendarView != null)
        {
            calendarView.SelectedDatesChanged += (obj, args) =>
            {
                if (args.AddedDates.Count > 0)
                {
                    MessageBox.Show("The user selected a new date: " + 
                        args.AddedDates[0].DateTime.ToString());
                }
            };
        }
    }
    ```

11. F5 キーをもう一度押して、デバッガーでアプリをリビルドして実行します。 カレンダーコントロールがウィンドウの下部に表示されることを確認します。

## <a name="package-the-app"></a>アプリのパッケージ化

必要に応じて、WPF アプリを[Msix パッケージ](https://docs.microsoft.com/windows/msix)にパッケージ化してデプロイすることもできます。 MSIX は Windows 向けの最新のアプリケーションパッケージ化テクノロジであり、MSI、.appx、App-v、ClickOnce のインストールテクノロジの組み合わせに基づいています。

次の手順では、Visual Studio 2019 の[Windows アプリケーションパッケージプロジェクト](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)を使用して、msix パッケージのソリューションに含まれるすべてのコンポーネントをパッケージ化する方法を示します。 これらの手順は、WPF アプリを MSIX パッケージでパッケージ化する場合にのみ必要です。

1. 新しい[Windows アプリケーションパッケージプロジェクト](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)をソリューションに追加します。 プロジェクトを作成するときに、 **Windows 10 バージョン 1903 (10.0;ビルド 18362)** 。**ターゲットバージョン**と**最小バージョン**の両方に対応します。

2. パッケージプロジェクトで、 **[アプリケーション]** ノードを右クリックし、 **[参照の追加]** を選択します。 プロジェクトの一覧で、ソリューション内の WPF プロジェクトを選択し、[ **OK]** をクリックします。

3. X86 や x64 などの特定のプラットフォームを対象とするようにソリューションを構成します。 これは、Windows アプリケーションパッケージプロジェクトを使用して、MSIX パッケージに WPF アプリをビルドするために必要です。

    1. **ソリューションエクスプローラー**で、ソリューション ノードを右クリックし、**プロパティ** -> **構成プロパティ** -> **Configuration Manager** の順に選択します。
    2. **[アクティブソリューションプラットフォーム]** で、 **[x64]** または **[x86]** を選択します。
    3. WPF プロジェクトの行の **[プラットフォーム]** 列で **[新規]** を選択します。
    4. **[新しいソリューションプラットフォーム]** ダイアログボックスで、 **[x64]** または **[x86]** (**アクティブソリューションプラットフォーム**用に選択したものと同じプラットフォーム) を選択し、 **[OK]** をクリックします。
    5. 開いているダイアログボックスを閉じます。

5. パッケージプロジェクトをビルドして実行します。 WPF が実行され、UWP カスタムコントロールが想定どおりに表示されることを確認します。

## <a name="related-topics"></a>関連トピック

* [デスクトップアプリでの UWP XAML コントロールのホスト (XAML アイランド)](xaml-islands.md)
* [XAML アイランドコードサンプル](https://github.com/microsoft/Xaml-Islands-Samples)
* [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)
* [InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar)
* [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)
