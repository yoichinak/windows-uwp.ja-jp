---
title: WPF でのビジュアル レイヤーの使用
description: ビジュアル レイヤー API を既存の WPF コンテンツと組み合わせて使用し、高度なアニメーションや効果を作成する方法について説明します。
ms.date: 03/18/2019
ms.topic: article
keywords: windows 10, uwp
ms.author: jimwalk
author: jwmsft
ms.localizationpriority: medium
ms.openlocfilehash: a2f30ba67acc12d622acd09f9fae872ee2058a2f
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/29/2020
ms.locfileid: "66215150"
---
# <a name="using-the-visual-layer-with-wpf"></a>WPF でのビジュアル レイヤーの使用

Windows Presentation Foundation (WPF) アプリで Windows ランタイム コンポジション API ([ビジュアル レイヤー](/windows/uwp/composition/visual-layer)とも呼ばれる) を使用して、Windows 10 ユーザーの利便性を高める最新のエクスペリエンスを作成できます。

このチュートリアルの完全なコードは、GitHub で入手できます。[WPF HelloComposition サンプル](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WPF/HelloComposition).

## <a name="prerequisites"></a>前提条件

UWP XAML ホスティング API には、これらの前提条件があります。

- WPF と UWP を使用したアプリ開発に関するいくらかの知識があることを前提としています。 For more info, see:
  - [概要 (WPF)](/dotnet/framework/wpf/getting-started/)
  - [Windows 10 アプリの概要](/windows/uwp/get-started/)
  - [Windows 10 向けのデスクトップ アプリケーションの強化](/windows/uwp/porting/desktop-to-uwp-enhance)
- .NET Framework 4.7.2 以降
- Windows 10 バージョン 1803 以降
- Windows 10 SDK 17134 以降

## <a name="how-to-use-composition-apis-in-wpf"></a>WPF で Composition API を使用する方法

このチュートリアルでは、シンプルな WPF アプリ の UI を作成し、アニメーション化したコンポジション要素を追加します。 WPF コンポーネントとコンポジション コンポーネントは両方ともシンプルにしますが、示されている相互運用コードは、コンポーネントの複雑さに関係なく同じものです。 完成したアプリは次のようになります。

![実行中のアプリの UI](images/visual-layer-interop/wpf-comp-interop-app-ui.png)

## <a name="create-a-wpf-project"></a>WPF プロジェクトの作成

最初の手順は、WPF アプリ プロジェクトを作成することであり、これにはアプリケーションの定義と UI の XAML ページが含まれます。

Visual C# で _HelloComposition_ という名前の新しい WPF アプリケーション プロジェクトを作成するには:

1. Visual Studio を開き、 **[ファイル]**  >  **[新規作成]**  >  **[プロジェクト]** の順に選択します。

    **[新しいプロジェクト]** ダイアログが開きます。
1. **[インストール済み]** カテゴリで、 **[Visual C#]** ノードを展開し、 **[Windows デスクトップ]** を選択します。
1. **[WPF アプリ (.NET Framework)]** テンプレートを選択します。
1. 名前「_HelloComposition_」を入力し、 **[.NET Framework 4.7.2]** Framework を選択して、 **[OK]** をクリックします。

    Visual Studio によって、プロジェクトが作成され、MainWindow.xaml という名前の既定のアプリケーション ウィンドウのデザイナーが開きます。

## <a name="configure-the-project-to-use-windows-runtime-apis"></a>Windows ランタイム API を使用するようにプロジェクトを構成する

WPF アプリで Windows ランタイム (WinRT) API を使用するには、Windows ランタイムにアクセスするように、Visual Studio プロジェクトを構成する必要があります。 さらに、Composition API ではベクトルが広範囲に使用されるため、ベクトルを使用するために必要な参照を追加する必要があります。

NuGet パッケージは、これらの両方のニーズに対処するために使用できます。 これらのパッケージの最新バージョンをインストールして、プロジェクトに必要な参照を追加します。  

- [Microsoft.Windows.SDK.Contracts](https://www.nuget.org/packages/Microsoft.Windows.SDK.Contracts) (既定のパッケージ管理形式を PackageReference に設定する必要があります。)
- [System.Numerics.Vectors](https://www.nuget.org/packages/System.Numerics.Vectors/)

> [!NOTE]
> NuGet パッケージを使用してプロジェクトを構成することをお勧めしますが、必要な参照を手動で追加できます。 詳細については、[Windows 10 向けのデスクトップ アプリの強化](/windows/uwp/porting/desktop-to-uwp-enhance)に関する記事を参照してください。 次の表に、参照を追加する必要があるファイルを示します。

|ファイル|インストール先|
|--|--|
|System.Runtime.WindowsRuntime|C:\Windows\Microsoft.NET\Framework\v4.0.30319|
|Windows.Foundation.UniversalApiContract.winmd|C:\Program Files (x86)\Windows Kits\10\References\<*SDK バージョン*>\Windows.Foundation.UniversalApiContract\<*バージョン*>|
|Windows.Foundation.FoundationContract.winmd|C:\Program Files (x86)\Windows Kits\10\References\<*SDK バージョン*>\Windows.Foundation.FoundationContract\<*バージョン*>|
|System.Numerics.Vectors.dll|C:\WINDOWS\Microsoft.Net\assembly\GAC_MSIL\System.Numerics.Vectors\v4.0_4.0.0.0__b03f5f7f11d50a3a|
|System.Numerics.dll|C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETFramework\v4.7.2|

## <a name="configure-the-project-to-be-per-monitor-dpi-aware"></a>モニターごとの DPI 対応になるように、プロジェクトを構成する

アプリに追加するビジュアル レイヤーのコンテンツは、それが表示される画面の DPI 設定に合わせて自動的に拡大縮小されません。 アプリのモニターごとの DPI 対応を有効にして、ビジュアル レイヤーのコンテンツを作成するために使用するコードで、アプリの実行時に現在の DPI スケールが考慮されることを確認する必要があります。 ここでは、モニターごとの DPI 対応になるように、プロジェクトを構成します。 後のシナリオで、ビジュアル レイヤー コンテンツを拡大縮小するための DPI 情報の使用方法について説明します。

WPF アプリは、既定でシステム DPI 対応ですが、app.manifest ファイルで、モニターごとの DPI 対応になるように、それ自体を宣言する必要があります。 アプリ マニフェスト ファイルで、Windows レベルのモニターごとの DPI 対応を有効にするには:

1. **ソリューション エクスプローラー**で、_HelloComposition_ プロジェクトを右クリックします。
1. コンテキスト メニューで、 **[追加]**  >  **[新しい項目...]** を選択します。
1. **[新しい項目の追加]** ダイアログで、[アプリケーション マニフェスト ファイル] を選択し、 **[追加]** をクリックします。 (既定の名前はそのままにしておいてかまいません)。
1. app.manifest ファイルでこの xml を見つけて、そのコメントを解除します。

    ```xaml
    <application xmlns="urn:schemas-microsoft-com:asm.v3">
        <windowsSettings>
          <dpiAware xmlns="http://schemas.microsoft.com/SMI/2005/WindowsSettings">true</dpiAware>
        </windowsSettings>
      </application>
    ```

1. 開始の `<windowsSettings>` タグの後に次の設定を追加します。

    ```xaml
          <dpiAwareness xmlns="http://schemas.microsoft.com/SMI/2016/WindowsSettings">PerMonitor</dpiAwareness>
    ```

1. App.config ファイルに、**DoNotScaleForDpiChanges** 設定を指定する必要もあります。

    App.config ファイルを開き、`<configuration>` 要素内に次の xml を追加します。

    ```xml
    <runtime>
      <AppContextSwitchOverrides value="Switch.System.Windows.DoNotScaleForDpiChanges=false"/>
    </runtime>
    ```

> [!NOTE]
> **AppContextSwitchOverrides** を設定できるのは 1 回のみです。 アプリケーションに既に 1 セットある場合は、値属性内でこの switch をセミコロンで区切る必要があります。

(詳しくは、GitHub の[モニターごとの DPI 開発者ガイドとサンプル](https://github.com/Microsoft/WPF-Samples/tree/master/PerMonitorDPI)を参照してください)。

## <a name="create-an-hwndhost-derived-class-to-host-composition-elements"></a>コンポジション要素をホストする HwndHost 派生クラスを作成する

ビジュアル レイヤーによって作成したコンテンツをホストするには、[HwndHost](/dotnet/api/system.windows.interop.hwndhost) から派生するクラスを作成する必要があります。 ここでは、Composition API をホストするための構成の大半を行います。 このクラスでは、[プラットフォーム呼び出しサービス (PInvoke)](/cpp/dotnet/calling-native-functions-from-managed-code) と [COM 相互運用](/dotnet/api/system.runtime.interopservices.comimportattribute)を使用して、Composition API を WPF アプリに取り込みます。 PInvoke と COM 相互運用の詳細については、「[アンマネージ コードとの相互運用](/dotnet/framework/interop/index)」を参照してください。

> [!TIP]
> チュートリアルを進めながら、必要に応じて、チュートリアルの最後にある完全なコードを確認して、すべてのコードが適切な場所にあることを確認してください。

1. プロジェクトに、[HwndHost](/dotnet/api/system.windows.interop.hwndhost) から派生する新しいクラス ファイルを追加します。
    - **ソリューション エクスプローラー**で、_HelloComposition_ プロジェクトを右クリックします。
    - コンテキスト メニューで、 **[追加]**  >  **[クラス...]** を選択します。
    - **[新しい項目の追加]** ダイアログ ボックスで、クラスに _CompositionHost.cs_ という名前を付けて、 **[追加]** をクリックします。
1. CompositionHost.cs で、クラス定義を **HwndHost** から派生するように編集します。

    ```csharp
    // Add
    // using System.Windows.Interop;

    namespace HelloComposition
    {
        class CompositionHost : HwndHost
        {
        }
    }
    ```

1. クラスに次のコードとコンストラクターを追加します。

    ```csharp
    // Add
    // using Windows.UI.Composition;

    IntPtr hwndHost;
    int hostHeight, hostWidth;
    object dispatcherQueue;
    ICompositionTarget compositionTarget;

    public Compositor Compositor { get; private set; }

    public Visual Child
    {
        set
        {
            if (Compositor == null)
            {
                InitComposition(hwndHost);
            }
            compositionTarget.Root = value;
        }
    }

    internal const int
      WS_CHILD = 0x40000000,
      WS_VISIBLE = 0x10000000,
      LBS_NOTIFY = 0x00000001,
      HOST_ID = 0x00000002,
      LISTBOX_ID = 0x00000001,
      WS_VSCROLL = 0x00200000,
      WS_BORDER = 0x00800000;

    public CompositionHost(double height, double width)
    {
        hostHeight = (int)height;
        hostWidth = (int)width;
    }
    ```

1. **BuildWindowCore** メソッドと **DestroyWindowCore** メソッドをオーバーライドします。
    > [!NOTE]
    > BuildWindowCore で _InitializeCoreDispatcher_ メソッドと _InitComposition_ メソッドを呼び出します。 これらのメソッドは、次の手順で作成します。

    ```csharp
    // Add
    // using System.Runtime.InteropServices;

    protected override HandleRef BuildWindowCore(HandleRef hwndParent)
    {
        // Create Window
        hwndHost = IntPtr.Zero;
        hwndHost = CreateWindowEx(0, "static", "",
                                  WS_CHILD | WS_VISIBLE,
                                  0, 0,
                                  hostWidth, hostHeight,
                                  hwndParent.Handle,
                                  (IntPtr)HOST_ID,
                                  IntPtr.Zero,
                                  0);

        // Create Dispatcher Queue
        dispatcherQueue = InitializeCoreDispatcher();

        // Build Composition tree of content
        InitComposition(hwndHost);

        return new HandleRef(this, hwndHost);
    }

    protected override void DestroyWindowCore(HandleRef hwnd)
    {
        if (compositionTarget.Root != null)
        {
            compositionTarget.Root.Dispose();
        }
        DestroyWindow(hwnd.Handle);
    }
    ```

    - [CreateWindowEx](/windows/desktop/api/winuser/nf-winuser-createwindowexa) および [DestroyWindow](/windows/desktop/api/winuser/nf-winuser-destroywindow) には、PInvoke 宣言が必要です。 この宣言は、クラスのコードの末尾に配置します。

    ```csharp
    #region PInvoke declarations

    [DllImport("user32.dll", EntryPoint = "CreateWindowEx", CharSet = CharSet.Unicode)]
    internal static extern IntPtr CreateWindowEx(int dwExStyle,
                                                  string lpszClassName,
                                                  string lpszWindowName,
                                                  int style,
                                                  int x, int y,
                                                  int width, int height,
                                                  IntPtr hwndParent,
                                                  IntPtr hMenu,
                                                  IntPtr hInst,
                                                  [MarshalAs(UnmanagedType.AsAny)] object pvParam);

    [DllImport("user32.dll", EntryPoint = "DestroyWindow", CharSet = CharSet.Unicode)]
    internal static extern bool DestroyWindow(IntPtr hwnd);

    #endregion PInvoke declarations
    ```

1. [CoreDispatcher](/uwp/api/windows.ui.core.coredispatcher) でスレッドを初期化します。 コア ディスパッチャーは、ウィンドウ メッセージの処理と WinRT API のイベントのディスパッチを担当します。 **CoreDispatcher** の新しいインスタンスは、CoreDispatcher があるスレッドで作成する必要があります。
    - _InitializeCoreDispatcher_ という名前のメソッドを作成し、ディスパッチャー キューをセットアップするコードを追加します。

    ```csharp
    private object InitializeCoreDispatcher()
    {
        DispatcherQueueOptions options = new DispatcherQueueOptions();
        options.apartmentType = DISPATCHERQUEUE_THREAD_APARTMENTTYPE.DQTAT_COM_STA;
        options.threadType = DISPATCHERQUEUE_THREAD_TYPE.DQTYPE_THREAD_CURRENT;
        options.dwSize = Marshal.SizeOf(typeof(DispatcherQueueOptions));

        object queue = null;
        CreateDispatcherQueueController(options, out queue);
        return queue;
    }
    ```

    - ディスパッチャー キューには、PInvoke 宣言も必要です。 前の手順で作成した _PInvoke 宣言_領域内に、この宣言を配置します。

    ```csharp
    //typedef enum DISPATCHERQUEUE_THREAD_APARTMENTTYPE
    //{
    //    DQTAT_COM_NONE,
    //    DQTAT_COM_ASTA,
    //    DQTAT_COM_STA
    //};
    internal enum DISPATCHERQUEUE_THREAD_APARTMENTTYPE
    {
        DQTAT_COM_NONE = 0,
        DQTAT_COM_ASTA = 1,
        DQTAT_COM_STA = 2
    };

    //typedef enum DISPATCHERQUEUE_THREAD_TYPE
    //{
    //    DQTYPE_THREAD_DEDICATED,
    //    DQTYPE_THREAD_CURRENT
    //};
    internal enum DISPATCHERQUEUE_THREAD_TYPE
    {
        DQTYPE_THREAD_DEDICATED = 1,
        DQTYPE_THREAD_CURRENT = 2,
    };

    //struct DispatcherQueueOptions
    //{
    //    DWORD dwSize;
    //    DISPATCHERQUEUE_THREAD_TYPE threadType;
    //    DISPATCHERQUEUE_THREAD_APARTMENTTYPE apartmentType;
    //};
    [StructLayout(LayoutKind.Sequential)]
    internal struct DispatcherQueueOptions
    {
        public int dwSize;

        [MarshalAs(UnmanagedType.I4)]
        public DISPATCHERQUEUE_THREAD_TYPE threadType;

        [MarshalAs(UnmanagedType.I4)]
        public DISPATCHERQUEUE_THREAD_APARTMENTTYPE apartmentType;
    };

    //HRESULT CreateDispatcherQueueController(
    //  DispatcherQueueOptions options,
    //  ABI::Windows::System::IDispatcherQueueController** dispatcherQueueController
    //);
    [DllImport("coremessaging.dll", EntryPoint = "CreateDispatcherQueueController", CharSet = CharSet.Unicode)]
    internal static extern IntPtr CreateDispatcherQueueController(DispatcherQueueOptions options,
                                            [MarshalAs(UnmanagedType.IUnknown)]
                                            out object dispatcherQueueController);
    ```

    これでディスパッチャー キューが準備され、コンポジション コンテンツの初期化と作成を開始できます。

1. [Compositor](/uwp/api/windows.ui.composition.compositor) を初期化します。 Compositor は、ビジュアル、効果システム、アニメーション システムにまたがる [Windows.UI.Composition](/uwp/api/windows.ui.composition) 名前空間にさまざまな型を作成するファクトリです。 Compositor クラスでは、ファクトリから作成されたオブジェクトの有効期間も管理します。

    ```csharp
    private void InitComposition(IntPtr hwndHost)
    {
        ICompositorDesktopInterop interop;

        compositor = new Compositor();
        object iunknown = compositor as object;
        interop = (ICompositorDesktopInterop)iunknown;
        IntPtr raw;
        interop.CreateDesktopWindowTarget(hwndHost, true, out raw);

        object rawObject = Marshal.GetObjectForIUnknown(raw);
        ICompositionTarget target = (ICompositionTarget)rawObject;

        if (raw == null) { throw new Exception("QI Failed"); }
    }
    ```

    - **ICompositorDesktopInterop** と **ICompositionTarget** には COM インポートが必要です。 このコードは _CompositionHost_ クラスの後で、名前空間宣言内に配置します。

    ```csharp
    #region COM Interop

    /*
    #undef INTERFACE
    #define INTERFACE ICompositorDesktopInterop
        DECLARE_INTERFACE_IID_(ICompositorDesktopInterop, IUnknown, "29E691FA-4567-4DCA-B319-D0F207EB6807")
        {
            IFACEMETHOD(CreateDesktopWindowTarget)(
                _In_ HWND hwndTarget,
                _In_ BOOL isTopmost,
                _COM_Outptr_ IDesktopWindowTarget * *result
                ) PURE;
        };
    */
    [ComImport]
    [Guid("29E691FA-4567-4DCA-B319-D0F207EB6807")]
    [InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
    public interface ICompositorDesktopInterop
    {
        void CreateDesktopWindowTarget(IntPtr hwndTarget, bool isTopmost, out IntPtr test);
    }

    //[contract(Windows.Foundation.UniversalApiContract, 2.0)]
    //[exclusiveto(Windows.UI.Composition.CompositionTarget)]
    //[uuid(A1BEA8BA - D726 - 4663 - 8129 - 6B5E7927FFA6)]
    //interface ICompositionTarget : IInspectable
    //{
    //    [propget] HRESULT Root([out] [retval] Windows.UI.Composition.Visual** value);
    //    [propput] HRESULT Root([in] Windows.UI.Composition.Visual* value);
    //}

    [ComImport]
    [Guid("A1BEA8BA-D726-4663-8129-6B5E7927FFA6")]
    [InterfaceType(ComInterfaceType.InterfaceIsIInspectable)]
    public interface ICompositionTarget
    {
        Windows.UI.Composition.Visual Root
        {
            get;
            set;
        }
    }

    #endregion COM Interop
    ```

## <a name="create-a-usercontrol-to-add-your-content-to-the-wpf-visual-tree"></a>WPF ビジュアル ツリーにコンテンツを追加するための UserControl を作成する

コンポジション コンテンツをホストするために必要なインフラストラクチャをセットアップする最後の手順は、WPF ビジュアルツリーに HwndHost を追加することです。

### <a name="create-a-usercontrol"></a>UserControl の作成

UserControl は、コンポジション コンテンツを作成して管理するコードをパッケージ化し、XAML にコンテンツを簡単に追加する便利な方法です。

1. 新しいユーザー コントロール ファイルをプロジェクトに追加します。
    - **ソリューション エクスプローラー**で、_HelloComposition_ プロジェクトを右クリックします。
    - コンテキスト メニューで、 **[追加]**  >  **[ユーザー コントロール...]** を選択します。
    - **[新しい項目の追加]** ダイアログ ボックスで、ユーザー コントロールに _CompositionHostControl.xaml_ という名前を付けて、 **[追加]** をクリックします。

    CompositionHostControl.xaml ファイルと CompositionHostControl.xaml.cs ファイルの両方が作成され、プロジェクトに追加されます。
1. CompositionHostControl.xaml で、`<Grid> </Grid>` タグをこの **Border** 要素に置き換えます。これは、HwndHost が入る XAML コンテナーです。

    ```xaml
    <Border Name="CompositionHostElement"/>
    ```

ユーザー コントロールのコードで、前の手順で作成した CompositionHost クラスのインスタンスを作成し、XAML ページに作成した Border である _CompositionHostElement_ の子要素として、それを追加します。

1. CompositionHostControl.xaml.cs で、コンポジション コードで使用するオブジェクトのプライベート変数を追加します。 これらはクラス定義の後に追加します。

    ```csharp
    CompositionHost compositionHost;
    Compositor compositor;
    Windows.UI.Composition.ContainerVisual containerVisual;
    DpiScale currentDpi;
    ```

1. ユーザー コントロールの **Loaded** イベントのハンドラーを追加します。 ここで、CompositionHost インスタンスをセットアップします。

    - コンストラクターで、ここに示すようにイベントハンドラーをフックします (`Loaded += CompositionHostControl_Loaded;`)。

    ```csharp
    public CompositionHostControl()
    {
        InitializeComponent();
        Loaded += CompositionHostControl_Loaded;
    }
    ```

    - *CompositionHostControl_Loaded* という名前のイベント ハンドラー メソッドを追加します。
    ```csharp
    private void CompositionHostControl_Loaded(object sender, RoutedEventArgs e)
    {
        // If the user changes the DPI scale setting for the screen the app is on,
        // the CompositionHostControl is reloaded. Don't redo this set up if it's
        // already been done.
        if (compositionHost is null)
        {
            currentDpi = VisualTreeHelper.GetDpi(this);

            compositionHost =
                new CompositionHost(ControlHostElement.ActualHeight, ControlHostElement.ActualWidth);
            ControlHostElement.Child = compositionHost;
            compositor = compositionHost.Compositor;
            containerVisual = compositor.CreateContainerVisual();
            compositionHost.Child = containerVisual;
        }
    }
    ```

    このメソッドで、コンポジション コードで使用するオブジェクトをセットアップします。 何が起こるかを簡単に説明します。

    - まず、CompositionHost のインスタンスが既に存在するかどうかを確認することによって、セットアップが 1 回だけ行われるようにします。

    ```csharp
    // If the user changes the DPI scale setting for the screen the app is on,
    // the CompositionHostControl is reloaded. Don't redo this set up if it's
    // already been done.
    if (compositionHost is null)
    {

    }
    ```

    - 現在の DPI を取得します。 これは、コンポジション要素を適切に拡大縮小するために使用されます。

    ```csharp
    currentDpi = VisualTreeHelper.GetDpi(this);
    ```

    - CompositionHost のインスタンスを作成し、Border _CompositionHostElement_ の子としてそれを割り当てます。

    ```csharp
    compositionHost =
        new CompositionHost(ControlHostElement.ActualHeight, ControlHostElement.ActualWidth);
    ControlHostElement.Child = compositionHost;
    ```

    - CompositionHost からコンポジターを取得します。

    ```csharp
    compositor = compositionHost.Compositor;
    ```

    - Compositor を使用して、コンテナー ビジュアルを作成します。 これは、コンポジション要素を追加するコンポジション コンテナーです。

    ```csharp
    containerVisual = compositor.CreateContainerVisual();
    compositionHost.Child = containerVisual;
    ```

### <a name="add-composition-elements"></a>コンポジション要素を追加する

インフラストラクチャが配置されたので、表示するコンポジション コンテンツを生成できるようになりました。

この例では、シンプルな正方形 [SpriteVisual](/uwp/api/windows.ui.composition.spritevisual) を作成してアニメーション化するコードを追加します。

1. コンポジション要素を追加します。 CompositionHostControl.xaml.cs で、これらのメソッドを CompositionHostControl クラスに追加します。

    ```csharp
    // Add
    // using System.Numerics;

    public void AddElement(float size, float offsetX, float offsetY)
    {
        var visual = compositor.CreateSpriteVisual();
        visual.Size = new Vector2(size, size);
        visual.Scale = new Vector3((float)currentDpi.DpiScaleX, (float)currentDpi.DpiScaleY, 1);
        visual.Brush = compositor.CreateColorBrush(GetRandomColor());
        visual.Offset = new Vector3(offsetX * (float)currentDpi.DpiScaleX, offsetY * (float)currentDpi.DpiScaleY, 0);

        containerVisual.Children.InsertAtTop(visual);

        AnimateSquare(visual, 3);
    }

    private void AnimateSquare(SpriteVisual visual, int delay)
    {
        float offsetX = (float)(visual.Offset.X); // Already adjusted for DPI.

        // Adjust values for DPI scale, then find the Y offset that aligns the bottom of the square
        // with the bottom of the host container. This is the value to animate to.
        var hostHeightAdj = CompositionHostElement.ActualHeight * currentDpi.DpiScaleY;
        var squareSizeAdj = visual.Size.Y * currentDpi.DpiScaleY;
        float bottom = (float)(hostHeightAdj - squareSizeAdj);

        // Create the animation only if it's needed.
        if (visual.Offset.Y != bottom)
        {
            Vector3KeyFrameAnimation animation = compositor.CreateVector3KeyFrameAnimation();
            animation.InsertKeyFrame(1f, new Vector3(offsetX, bottom, 0f));
            animation.Duration = TimeSpan.FromSeconds(2);
            animation.DelayTime = TimeSpan.FromSeconds(delay);
            visual.StartAnimation("Offset", animation);
        }
    }

    private Windows.UI.Color GetRandomColor()
    {
        Random random = new Random();
        byte r = (byte)random.Next(0, 255);
        byte g = (byte)random.Next(0, 255);
        byte b = (byte)random.Next(0, 255);
        return Windows.UI.Color.FromArgb(255, r, g, b);
    }
    ```

### <a name="handle-dpi-changes"></a>DPI の変更を処理する

要素を追加して、アニメーション化するコードでは、要素の作成時に現在の DPI スケールが考慮されますが、アプリの実行中にも DPI の変更を考慮する必要があります。 変更が通知されるように [HwndHost.DpiChanged](/dotnet/api/system.windows.interop.hwndhost.dpichanged) イベントを処理し、新しい DPI に基づいて計算を調整できます。

1. CompositionHostControl_Loaded メソッドで、最後の行の後に、DpiChanged イベントハンドラーをフックするために、これを追加します。

    ```csharp
    compositionHost.DpiChanged += CompositionHost_DpiChanged;
    ```

1. _CompositionHostDpiChanged_ という名前のイベント ハンドラー メソッドを追加します。 このコードは、各要素のスケールとオフセットを調整し、完了していないすべてのアニメーションを再計算します。

    ```csharp
    private void CompositionHost_DpiChanged(object sender, DpiChangedEventArgs e)
    {
        currentDpi = e.NewDpi;
        Vector3 newScale = new Vector3((float)e.NewDpi.DpiScaleX, (float)e.NewDpi.DpiScaleY, 1);

        foreach (SpriteVisual child in containerVisual.Children)
        {
            child.Scale = newScale;
            var newOffsetX = child.Offset.X * ((float)e.NewDpi.DpiScaleX / (float)e.OldDpi.DpiScaleX);
            var newOffsetY = child.Offset.Y * ((float)e.NewDpi.DpiScaleY / (float)e.OldDpi.DpiScaleY);
            child.Offset = new Vector3(newOffsetX, newOffsetY, 1);

            // Adjust animations for DPI change.
            AnimateSquare(child, 0);
        }
    }
    ```

## <a name="add-the-user-control-to-your-xaml-page"></a>XAML ページにユーザー コントロールを追加する

これで、XAML UI にユーザー コントロールを追加できます。

1. Mainwindow.xaml で、ウィンドウの高さを 600 に設定し、幅を 840 に設定します。
1. UI の XAML を追加します。 MainWindow.xaml で、この XAML をルート `<Grid> </Grid>` タグの間に追加します。

    ```xaml
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="210"/>
        <ColumnDefinition Width="600"/>
        <ColumnDefinition/>
    </Grid.ColumnDefinitions>
    <Grid.RowDefinitions>
        <RowDefinition Height="46"/>
        <RowDefinition/>
    </Grid.RowDefinitions>
    <Button Content="Add composition element" Click="Button_Click"
            Grid.Row="1" Margin="12,0"
            VerticalAlignment="Top" Height="40"/>
    <TextBlock Text="Composition content" FontSize="20"
               Grid.Column="1" Margin="0,12,0,4"
               HorizontalAlignment="Center"/>
    <local:CompositionHostControl x:Name="CompositionHostControl1"
                                  Grid.Row="1" Grid.Column="1"
                                  VerticalAlignment="Top"
                                  Width="600" Height="500"
                                  BorderBrush="LightGray"
                                  BorderThickness="3"/>
    ```

1. 新しい要素を作成するには、ボタン クリックを処理します。 (クリック イベントは XAML で既にフックされています)。

    MainWindow.xaml.cs で、この *Button_Click* イベント ハンドラー メソッドを追加します。 このコードは _CompositionHost.AddElement_ を呼び出して、ランダムに生成されたサイズとオフセットで新しい要素を作成します。

    ```csharp
    // Add
    // using System;

    private void Button_Click(object sender, RoutedEventArgs e)
    {
        Random random = new Random();
        float size = random.Next(50, 150);
        float offsetX = random.Next(0, (int)(CompositionHostControl1.ActualWidth - size));
        float offsetY = random.Next(0, (int)(CompositionHostControl1.ActualHeight/2 - size));
        CompositionHostControl1.AddElement(size, offsetX, offsetY);
    }
    ```

これで、WPF アプリをビルドして実行できるようになりました。 必要に応じて、チュートリアルの最後にある完全なコードを確認して、すべてのコードが適切な場所にあることを確認してください。

アプリを実行して、ボタンをクリックすると、UI に追加されたアニメーションの正方形が表示されるはずです。

## <a name="next-steps"></a>次の手順

同じインフラストラクチャにビルドする、より完全な例については、GitHub の [WPF ビジュアル レイヤー統合サンプル](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WPF/VisualLayerIntegration)を参照してください。

## <a name="additional-resources"></a>その他の資料

- [概要 (WPF)](/dotnet/framework/wpf/getting-started/) (.NET)
- [アンマネージ コードとの相互運用](/dotnet/framework/interop/) (.NET)
- [Windows 10 アプリの概要](/windows/uwp/get-started/) (UWP)
- [Windows 10 向けのデスクトップ アプリケーションの強化](/windows/uwp/porting/desktop-to-uwp-enhance) (UWP)
- [Windows.UI.Composition 名前空間](/uwp/api/windows.ui.composition) (UWP)

## <a name="complete-code"></a>コードを完成させる

このチュートリアルの完全なコードを次に示します。

### <a name="mainwindowxaml"></a>MainWindow.xaml

```xaml
<Window x:Class="HelloComposition.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:HelloComposition"
        mc:Ignorable="d"
        Title="MainWindow" Height="600" Width="840">
    <Grid>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="210"/>
            <ColumnDefinition Width="600"/>
            <ColumnDefinition/>
        </Grid.ColumnDefinitions>
        <Grid.RowDefinitions>
            <RowDefinition Height="46"/>
            <RowDefinition/>
        </Grid.RowDefinitions>
        <Button Content="Add composition element" Click="Button_Click"
                Grid.Row="1" Margin="12,0"
                VerticalAlignment="Top" Height="40"/>
        <TextBlock Text="Composition content" FontSize="20"
                   Grid.Column="1" Margin="0,12,0,4"
                   HorizontalAlignment="Center"/>
        <local:CompositionHostControl x:Name="CompositionHostControl1"
                                      Grid.Row="1" Grid.Column="1"
                                      VerticalAlignment="Top"
                                      Width="600" Height="500"
                                      BorderBrush="LightGray" BorderThickness="3"/>
    </Grid>
</Window>
```

### <a name="mainwindowxamlcs"></a>MainWindow.xaml.cs

```csharp
using System;
using System.Windows;

namespace HelloComposition
{
    /// <summary>
    /// Interaction logic for MainWindow.xaml
    /// </summary>
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
        }

        private void Button_Click(object sender, RoutedEventArgs e)
        {
            Random random = new Random();
            float size = random.Next(50, 150);
            float offsetX = random.Next(0, (int)(CompositionHostControl1.ActualWidth - size));
            float offsetY = random.Next(0, (int)(CompositionHostControl1.ActualHeight/2 - size));
            CompositionHostControl1.AddElement(size, offsetX, offsetY);
        }
    }
}
```

### <a name="compositionhostcontrolxaml"></a>CompositionHostControl.xaml

```xaml
<UserControl x:Class="HelloComposition.CompositionHostControl"
             xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
             xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
             xmlns:local="clr-namespace:HelloComposition"
             mc:Ignorable="d"
             d:DesignHeight="450" d:DesignWidth="800">
    <Border Name="CompositionHostElement"/>
</UserControl>
```

### <a name="compositionhostcontrolxamlcs"></a>CompositionHostControl.xaml.cs

```csharp
using System;
using System.Numerics;
using System.Windows;
using System.Windows.Controls;
using System.Windows.Media;
using Windows.UI.Composition;

namespace HelloComposition
{
    /// <summary>
    /// Interaction logic for CompositionHostControl.xaml
    /// </summary>
    public partial class CompositionHostControl : UserControl
    {
        CompositionHost compositionHost;
        Compositor compositor;
        Windows.UI.Composition.ContainerVisual containerVisual;
        DpiScale currentDpi;

        public CompositionHostControl()
        {
            InitializeComponent();
            Loaded += CompositionHostControl_Loaded;
        }

        private void CompositionHostControl_Loaded(object sender, RoutedEventArgs e)
        {
            // If the user changes the DPI scale setting for the screen the app is on,
            // the CompositionHostControl is reloaded. Don't redo this set up if it's
            // already been done.
            if (compositionHost is null)
            {
                currentDpi = VisualTreeHelper.GetDpi(this);

                compositionHost = new CompositionHost(CompositionHostElement.ActualHeight, CompositionHostElement.ActualWidth);
                CompositionHostElement.Child = compositionHost;
                compositor = compositionHost.Compositor;
                containerVisual = compositor.CreateContainerVisual();
                compositionHost.Child = containerVisual;
            }
        }

        protected override void OnDpiChanged(DpiScale oldDpi, DpiScale newDpi)
        {
            base.OnDpiChanged(oldDpi, newDpi);
            currentDpi = newDpi;
            Vector3 newScale = new Vector3((float)newDpi.DpiScaleX, (float)newDpi.DpiScaleY, 1);

            foreach (SpriteVisual child in containerVisual.Children)
            {
                child.Scale = newScale;
                var newOffsetX = child.Offset.X * ((float)newDpi.DpiScaleX / (float)oldDpi.DpiScaleX);
                var newOffsetY = child.Offset.Y * ((float)newDpi.DpiScaleY / (float)oldDpi.DpiScaleY);
                child.Offset = new Vector3(newOffsetX, newOffsetY, 1);

                // Adjust animations for DPI change.
                AnimateSquare(child, 0);
            }
        }

        public void AddElement(float size, float offsetX, float offsetY)
        {
            var visual = compositor.CreateSpriteVisual();
            visual.Size = new Vector2(size, size);
            visual.Scale = new Vector3((float)currentDpi.DpiScaleX, (float)currentDpi.DpiScaleY, 1);
            visual.Brush = compositor.CreateColorBrush(GetRandomColor());
            visual.Offset = new Vector3(offsetX * (float)currentDpi.DpiScaleX, offsetY * (float)currentDpi.DpiScaleY, 0);

            containerVisual.Children.InsertAtTop(visual);

            AnimateSquare(visual, 3);
        }

        private void AnimateSquare(SpriteVisual visual, int delay)
        {
            float offsetX = (float)(visual.Offset.X); // Already adjusted for DPI.

            // Adjust values for DPI scale, then find the Y offset that aligns the bottom of the square
            // with the bottom of the host container. This is the value to animate to.
            var hostHeightAdj = CompositionHostElement.ActualHeight * currentDpi.DpiScaleY;
            var squareSizeAdj = visual.Size.Y * currentDpi.DpiScaleY;
            float bottom = (float)(hostHeightAdj - squareSizeAdj);

            // Create the animation only if it's needed.
            if (visual.Offset.Y != bottom)
            {
                Vector3KeyFrameAnimation animation = compositor.CreateVector3KeyFrameAnimation();
                animation.InsertKeyFrame(1f, new Vector3(offsetX, bottom, 0f));
                animation.Duration = TimeSpan.FromSeconds(2);
                animation.DelayTime = TimeSpan.FromSeconds(delay);
                visual.StartAnimation("Offset", animation);
            }
        }

        private Windows.UI.Color GetRandomColor()
        {
            Random random = new Random();
            byte r = (byte)random.Next(0, 255);
            byte g = (byte)random.Next(0, 255);
            byte b = (byte)random.Next(0, 255);
            return Windows.UI.Color.FromArgb(255, r, g, b);
        }
    }
}

```

### <a name="compositionhostcs"></a>CompositionHost.cs

```csharp
using System;
using System.Runtime.InteropServices;
using System.Windows.Interop;
using Windows.UI.Composition;

namespace HelloComposition
{
    class CompositionHost : HwndHost
    {
        IntPtr hwndHost;
        int hostHeight, hostWidth;
        object dispatcherQueue;
        ICompositionTarget compositionTarget;

        public Compositor Compositor { get; private set; }

        public Visual Child
        {
            set
            {
                if (Compositor == null)
                {
                    InitComposition(hwndHost);
                }
                compositionTarget.Root = value;
            }
        }

        internal const int
          WS_CHILD = 0x40000000,
          WS_VISIBLE = 0x10000000,
          LBS_NOTIFY = 0x00000001,
          HOST_ID = 0x00000002,
          LISTBOX_ID = 0x00000001,
          WS_VSCROLL = 0x00200000,
          WS_BORDER = 0x00800000;

        public CompositionHost(double height, double width)
        {
            hostHeight = (int)height;
            hostWidth = (int)width;
        }

        protected override HandleRef BuildWindowCore(HandleRef hwndParent)
        {
            // Create Window
            hwndHost = IntPtr.Zero;
            hwndHost = CreateWindowEx(0, "static", "",
                                      WS_CHILD | WS_VISIBLE,
                                      0, 0,
                                      hostWidth, hostHeight,
                                      hwndParent.Handle,
                                      (IntPtr)HOST_ID,
                                      IntPtr.Zero,
                                      0);

            // Create Dispatcher Queue
            dispatcherQueue = InitializeCoreDispatcher();

            // Build Composition Tree of content
            InitComposition(hwndHost);

            return new HandleRef(this, hwndHost);
        }

        protected override void DestroyWindowCore(HandleRef hwnd)
        {
            if (compositionTarget.Root != null)
            {
                compositionTarget.Root.Dispose();
            }
            DestroyWindow(hwnd.Handle);
        }

        private object InitializeCoreDispatcher()
        {
            DispatcherQueueOptions options = new DispatcherQueueOptions();
            options.apartmentType = DISPATCHERQUEUE_THREAD_APARTMENTTYPE.DQTAT_COM_STA;
            options.threadType = DISPATCHERQUEUE_THREAD_TYPE.DQTYPE_THREAD_CURRENT;
            options.dwSize = Marshal.SizeOf(typeof(DispatcherQueueOptions));

            object queue = null;
            CreateDispatcherQueueController(options, out queue);
            return queue;
        }

        private void InitComposition(IntPtr hwndHost)
        {
            ICompositorDesktopInterop interop;

            Compositor = new Compositor();
            object iunknown = Compositor as object;
            interop = (ICompositorDesktopInterop)iunknown;
            IntPtr raw;
            interop.CreateDesktopWindowTarget(hwndHost, true, out raw);

            object rawObject = Marshal.GetObjectForIUnknown(raw);
            compositionTarget = (ICompositionTarget)rawObject;

            if (raw == null) { throw new Exception("QI Failed"); }
        }

        #region PInvoke declarations

        //typedef enum DISPATCHERQUEUE_THREAD_APARTMENTTYPE
        //{
        //    DQTAT_COM_NONE,
        //    DQTAT_COM_ASTA,
        //    DQTAT_COM_STA
        //};
        internal enum DISPATCHERQUEUE_THREAD_APARTMENTTYPE
        {
            DQTAT_COM_NONE = 0,
            DQTAT_COM_ASTA = 1,
            DQTAT_COM_STA = 2
        };

        //typedef enum DISPATCHERQUEUE_THREAD_TYPE
        //{
        //    DQTYPE_THREAD_DEDICATED,
        //    DQTYPE_THREAD_CURRENT
        //};
        internal enum DISPATCHERQUEUE_THREAD_TYPE
        {
            DQTYPE_THREAD_DEDICATED = 1,
            DQTYPE_THREAD_CURRENT = 2,
        };

        //struct DispatcherQueueOptions
        //{
        //    DWORD dwSize;
        //    DISPATCHERQUEUE_THREAD_TYPE threadType;
        //    DISPATCHERQUEUE_THREAD_APARTMENTTYPE apartmentType;
        //};
        [StructLayout(LayoutKind.Sequential)]
        internal struct DispatcherQueueOptions
        {
            public int dwSize;

            [MarshalAs(UnmanagedType.I4)]
            public DISPATCHERQUEUE_THREAD_TYPE threadType;

            [MarshalAs(UnmanagedType.I4)]
            public DISPATCHERQUEUE_THREAD_APARTMENTTYPE apartmentType;
        };

        //HRESULT CreateDispatcherQueueController(
        //  DispatcherQueueOptions options,
        //  ABI::Windows::System::IDispatcherQueueController** dispatcherQueueController
        //);
        [DllImport("coremessaging.dll", EntryPoint = "CreateDispatcherQueueController", CharSet = CharSet.Unicode)]
        internal static extern IntPtr CreateDispatcherQueueController(DispatcherQueueOptions options,
                                                [MarshalAs(UnmanagedType.IUnknown)]
                                               out object dispatcherQueueController);


        [DllImport("user32.dll", EntryPoint = "CreateWindowEx", CharSet = CharSet.Unicode)]
        internal static extern IntPtr CreateWindowEx(int dwExStyle,
                                                      string lpszClassName,
                                                      string lpszWindowName,
                                                      int style,
                                                      int x, int y,
                                                      int width, int height,
                                                      IntPtr hwndParent,
                                                      IntPtr hMenu,
                                                      IntPtr hInst,
                                                      [MarshalAs(UnmanagedType.AsAny)] object pvParam);

        [DllImport("user32.dll", EntryPoint = "DestroyWindow", CharSet = CharSet.Unicode)]
        internal static extern bool DestroyWindow(IntPtr hwnd);


        #endregion PInvoke declarations

    }
    #region COM Interop

    /*
    #undef INTERFACE
    #define INTERFACE ICompositorDesktopInterop
        DECLARE_INTERFACE_IID_(ICompositorDesktopInterop, IUnknown, "29E691FA-4567-4DCA-B319-D0F207EB6807")
        {
            IFACEMETHOD(CreateDesktopWindowTarget)(
                _In_ HWND hwndTarget,
                _In_ BOOL isTopmost,
                _COM_Outptr_ IDesktopWindowTarget * *result
                ) PURE;
        };
    */
    [ComImport]
    [Guid("29E691FA-4567-4DCA-B319-D0F207EB6807")]
    [InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
    public interface ICompositorDesktopInterop
    {
        void CreateDesktopWindowTarget(IntPtr hwndTarget, bool isTopmost, out IntPtr test);
    }

    //[contract(Windows.Foundation.UniversalApiContract, 2.0)]
    //[exclusiveto(Windows.UI.Composition.CompositionTarget)]
    //[uuid(A1BEA8BA - D726 - 4663 - 8129 - 6B5E7927FFA6)]
    //interface ICompositionTarget : IInspectable
    //{
    //    [propget] HRESULT Root([out] [retval] Windows.UI.Composition.Visual** value);
    //    [propput] HRESULT Root([in] Windows.UI.Composition.Visual* value);
    //}

    [ComImport]
    [Guid("A1BEA8BA-D726-4663-8129-6B5E7927FFA6")]
    [InterfaceType(ComInterfaceType.InterfaceIsIInspectable)]
    public interface ICompositionTarget
    {
        Windows.UI.Composition.Visual Root
        {
            get;
            set;
        }
    }

    #endregion COM Interop
}
```