---
title: Windows フォームでのビジュアル レイヤーの使用
description: 既存の Windows フォームのコンテンツと組み合わせてビジュアル レイヤーの Api を使用して、高度なアニメーションや効果を作成するための手法について説明します。
ms.date: 03/18/2019
ms.topic: article
keywords: windows 10, uwp
ms.author: jimwalk
author: jwmsft
ms.localizationpriority: medium
ms.openlocfilehash: 9da9dee48beef6e3c1cd38ffbe9761ed89fd940d
ms.sourcegitcommit: 93d0b2996b4742b33cd6d641e036f42672cf5238
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/23/2019
ms.locfileid: "69999639"
---
# <a name="using-the-visual-layer-with-windows-forms"></a>Windows フォームでのビジュアル レイヤーの使用

Windows ランタイムの合成 Api を使用することができます (とも呼ばれる、[ビジュアル レイヤー](/windows/uwp/composition/visual-layer)) で、Windows フォーム アプリで Windows 10 のユーザー用に光を最新のエクスペリエンスを作成します。

このチュートリアルの完全なコードについては、GitHub を参照してください。[HelloComposition サンプルを Windows フォーム](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WinForms/HelloComposition)します。

## <a name="prerequisites"></a>前提条件

UWP ホスティング API には、これらの前提条件があります。

- Windows フォームと UWP を使用したアプリ開発に関する知識があることを前提としています。 For more info, see:
  - [Windows フォームについて](/dotnet/framework/winforms/getting-started-with-windows-forms)
  - [Windows 10 アプリを使ってみる](/windows/uwp/get-started/)
  - [Windows 10 用にデスクトップアプリケーションを拡張する](/windows/uwp/porting/desktop-to-uwp-enhance)
- .NET Framework 4.7.2 以降
- Windows 10 バージョン1803以降
- Windows 10 SDK 17134 以降

## <a name="how-to-use-composition-apis-in-windows-forms"></a>Windows フォームでコンポジション Api を使用する方法

このチュートリアルでは、単純な Windows フォーム UI を作成し、アニメーション化されたコンポジション要素を追加します。 Windows フォームと合成の両方のコンポーネントが単純に保持されますが、表示される相互運用コードは、コンポーネントの複雑さに関係なく同じです。 完成したアプリは次のようになります。

![実行中のアプリの UI](images/visual-layer-interop/wf-comp-interop-app-ui.png)

## <a name="create-a-windows-forms-project"></a>Windows フォームプロジェクトを作成する

最初の手順では、アプリケーション定義と UI のメインフォームを含む Windows フォームアプリプロジェクトを作成します。

C# _HelloComposition_という名前の新しい Windows フォームアプリケーションプロジェクトを作成するには、次のようにします。

1. Visual Studio を開き、[**ファイル** > ] [**新しい** > **プロジェクト**] を選択します。<br/>**[新しいプロジェクト]** ダイアログボックスが開きます。
1. **[インストール済み]** カテゴリで、**ビジュアルC#** ノードを展開し、 **[Windows デスクトップ]** を選択します。
1. **[Windows フォームアプリ (.NET Framework)]** テンプレートを選択します。
1. 名前として _「HelloComposition」_ を入力し、[Framework **.NET Framework 4.7.2**] を選択して、[ **OK]** をクリックします。

Visual Studio によってプロジェクトが作成され、Form1.cs という名前の既定のアプリケーションウィンドウのデザイナーが開きます。

## <a name="configure-the-project-to-use-windows-runtime-apis"></a>Windows ランタイム Api を使用するようにプロジェクトを構成する

Windows フォームアプリで Windows ランタイム (WinRT) Api を使用するには、Windows ランタイムにアクセスするように Visual Studio プロジェクトを構成する必要があります。 さらに、ベクターはコンポジション Api によって広く使用されるため、ベクターを使用するために必要な参照を追加する必要があります。

NuGet パッケージは、これらの両方のニーズに対応するために使用できます。 これらのパッケージの最新バージョンをインストールして、必要な参照をプロジェクトに追加します。  

- PackageReference (既定のパッケージ管理形式は、既定ではに設定されている必要があります[)。](https://www.nuget.org/packages/Microsoft.Windows.SDK.Contracts)
- [System.Numerics.Vectors](https://www.nuget.org/packages/System.Numerics.Vectors/)

> [!NOTE]
> NuGet パッケージを使用してプロジェクトを構成することをお勧めしますが、必要な参照を手動で追加することもできます。 詳細については、「 [Windows 10 用のデスクトップアプリケーションの拡張](/windows/uwp/porting/desktop-to-uwp-enhance)」を参照してください。 次の表に、参照を追加する必要があるファイルを示します。

|ファイル|Location|
|--|--|
|System.Runtime.WindowsRuntime|C:\Windows\Microsoft.NET\Framework\v4.0.30319|
|Windows.Foundation.UniversalApiContract.winmd|C:\Program files (x86) \windows Kits\10\References\<*sdk バージョン*> \Windows.Foundation.UniversalApiContract\<*バージョン*>|
|Windows.Foundation.FoundationContract.winmd|C:\Program files (x86) \windows Kits\10\References\<*sdk バージョン*> \Windows.Foundation.FoundationContract\<*バージョン*>|
|System.string. vector. .dll|C:\WINDOWS\Microsoft.Net\assembly\GAC_MSIL\System.Numerics.Vectors\v4.0_4.0.0.0__b03f5f7f11d50a3a|
|System.string|C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETFramework\v4.7.2|

## <a name="create-a-custom-control-to-manage-interop"></a>相互運用を管理するためのカスタムコントロールを作成する

派生したカスタム コントロールを作成するビジュアル レイヤーで作成するコンテンツのホストに[コントロール](/dotnet/api/system.windows.forms.control)します。 このコントロールでは、ウィンドウにアクセスする[処理](/dotnet/api/system.windows.forms.control.handle)、ビジュアル レイヤーコンテンツのコンテナーを作成するために必要があります。

これにより、構成の大部分をホストして、合成 Api をホストすることができます。 このコントロールでは、[プラットフォーム呼び出しサービス (PInvoke)](/cpp/dotnet/calling-native-functions-from-managed-code)と[COM 相互運用機能](/dotnet/api/system.runtime.interopservices.comimportattribute)を使用して、コンポジション api を Windows フォームアプリに取り込むことができます。 PInvoke と COM 相互運用の詳細については、「[アンマネージコードとの相互運用](/dotnet/framework/interop/index)」を参照してください。

> [!TIP]
> 必要に応じて、チュートリアルの最後にある完全なコードを確認して、チュートリアルで作業するときにすべてのコードが適切な場所にあることを確認します。

1. [コントロール](/dotnet/api/system.windows.forms.control)から派生した新しいカスタムコントロールファイルをプロジェクトに追加します。
    - **ソリューションエクスプローラー**で、 _HelloComposition_プロジェクトを右クリックします。
    - コンテキストメニューで、[**新しい項目**の**追加** > ] を選択します。
    - **[新しい項目の追加]** ダイアログで、 **[カスタムコントロール]** を選択します。
    - コントロールに_CompositionHost.cs_という名前を指定し、 **[追加]** をクリックします。 デザインビューで CompositionHost.cs が開きます。

1. CompositionHost.cs のコードビューに切り替え、次のコードをクラスに追加します。

    ```csharp
    // Add
    // using Windows.UI.Composition;

    IntPtr hwndHost;
    object dispatcherQueue;
    protected ContainerVisual containerVisual;
    protected Compositor compositor;

    private ICompositionTarget compositionTarget;

    public Visual Child
    {
        set
        {
            if (compositor == null)
            {
                InitComposition(hwndHost);
            }
            compositionTarget.Root = value;
        }
    }
    ```

1. コードをコンストラクターに追加します。

    コンストラクターでは、 _InitializeCoreDispatcher_メソッドと_initcomposition_メソッドを呼び出します。 これらのメソッドは、次の手順で作成します。

    ```csharp
    public CompositionHost()
    {
        InitializeComponent();

        // Get the window handle.
        hwndHost = Handle;

        // Create dispatcher queue.
        dispatcherQueue = InitializeCoreDispatcher();

        // Build Composition tree of content.
        InitComposition(hwndHost);
    }
    ```

1. [CoreDispatcher](/uwp/api/windows.ui.core.coredispatcher)を使用してスレッドを初期化します。 コアディスパッチャーは、windows メッセージの処理と WinRT Api のイベントのディスパッチを担当します。 CoreDispatcher を持つスレッドで、**コンポジター**の新しいインスタンスを作成する必要があります。
    - _InitializeCoreDispatcher_という名前のメソッドを作成し、ディスパッチャーキューを設定するためのコードを追加します。

    ```csharp
    // Add
    // using System.Runtime.InteropServices;

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

    - ディスパッチャーキューには PInvoke 宣言が必要です。 この宣言は、クラスのコードの末尾に配置します。 (クラスコードを整理するために、このコードをリージョン内に配置します)。

    ```csharp
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

    #endregion PInvoke declarations
    ```

    これで、ディスパッチャーキューの準備が完了し、コンポジションコンテンツの初期化と作成を開始できるようになりました。

1. [コンポジター](/uwp/api/windows.ui.composition.compositor)を初期化します。 コンポジターがさまざまな型を作成するファクトリを[Windows.UI.Composition](/uwp/api/windows.ui.composition)ビジュアル レイヤー、効果のシステム、およびアニメーション システムにまたがる名前空間。 また、コンポジタークラスは、ファクトリから作成されたオブジェクトの有効期間も管理します。

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
        compositionTarget = (ICompositionTarget)rawObject;

        if (raw == null) { throw new Exception("QI Failed"); }

        containerVisual = compositor.CreateContainerVisual();
        Child = containerVisual;
    }
    ```

    - **ICompositorDesktopInterop**と**ICOMPOSITIONTARGET**には COM インポートが必要です。 このコードは、 _CompositionHost_クラスの後、名前空間宣言の内部に配置します。

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

## <a name="create-a-custom-control-to-host-composition-elements"></a>合成要素をホストするカスタムコントロールを作成する

コンポジション要素を生成して管理するコードは、CompositionHost から派生した別のコントロールに配置することをお勧めします。 これにより、CompositionHost クラスで作成した相互運用コードを再利用できます。

ここでは、CompositionHost から派生したカスタムコントロールを作成します。 このコントロールは、フォームに追加できるように、Visual Studio のツールボックスに追加されます。

1. CompositionHost から派生した新しいカスタムコントロールファイルをプロジェクトに追加します。
    - **ソリューションエクスプローラー**で、 _HelloComposition_プロジェクトを右クリックします。
    - コンテキストメニューで、[**新しい項目**の**追加** > ] を選択します。
    - **[新しい項目の追加]** ダイアログで、 **[カスタムコントロール]** を選択します。
    - コントロールに_CompositionHostControl.cs_という名前を指定し、 **[追加]** をクリックします。 デザインビューで CompositionHostControl.cs が開きます。

1. CompositionHostControl.cs デザインビューのプロパティペインで、 **BackColor**プロパティを**controllight**に設定します。

    背景色の設定は任意です。 ここでは、フォームの背景に対してカスタムコントロールを表示できるようにします。

1. CompositionHostControl.cs のコードビューに切り替えて、CompositionHost から派生するようにクラス宣言を更新します。

    ```csharp
    class CompositionHostControl : CompositionHost
    ```

1. コンストラクターを更新して、基本コンストラクターを呼び出します。

    ```csharp
    public CompositionHostControl() : base()
    {

    }
    ```

### <a name="add-composition-elements"></a>コンポジション要素の追加

インフラストラクチャが整ったら、アプリの UI にコンポジションコンテンツを追加できるようになりました。

この例では、単純な[SpriteVisual](/uwp/api/windows.ui.composition.spritevisual)を作成してアニメーション化するコードを CompositionHostControl クラスに追加します。

1. 合成要素を追加します。

    CompositionHostControl.cs で、これらのメソッドを CompositionHostControl クラスに追加します。

    ```csharp
    // Add
    // using Windows.UI.Composition;

    public void AddElement(float size, float offsetX, float offsetY)
    {
        var visual = compositor.CreateSpriteVisual();
        visual.Size = new Vector2(size, size); // Requires references
        visual.Brush = compositor.CreateColorBrush(GetRandomColor());
        visual.Offset = new Vector3(offsetX, offsetY, 0);
        containerVisual.Children.InsertAtTop(visual);

        AnimateSquare(visual, 3);
    }

    private void AnimateSquare(SpriteVisual visual, int delay)
    {
        float offsetX = (float)(visual.Offset.X);
        Vector3KeyFrameAnimation animation = compositor.CreateVector3KeyFrameAnimation();
        float bottom = Height - visual.Size.Y;
        animation.InsertKeyFrame(1f, new Vector3(offsetX, bottom, 0f));
        animation.Duration = TimeSpan.FromSeconds(2);
        animation.DelayTime = TimeSpan.FromSeconds(delay);
        visual.StartAnimation("Offset", animation);
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

## <a name="add-the-control-to-your-form"></a>フォームにコントロールを追加する

合成コンテンツをホストするカスタムコントロールを作成したので、それをアプリの UI に追加できます。 ここでは、前の手順で作成した CompositionHostControl のインスタンスを追加します。 CompositionHostControl は、Visual Studio の [ツールボックス] の [ **_プロジェクト名_コンポーネント**] に自動的に追加されます。

1. Form1.CS デザインビューで、UI にボタンを追加します。

    - ボタンを [ツールボックス] から Form1 にドラッグします。 フォームの左上隅に配置します。 (コントロールの配置を確認するには、チュートリアルの冒頭にあるイメージを参照してください)。
    - プロパティ ペインで、 **Text** プロパティを_button1_から _コンポジション要素の追加_ に変更します。
    - すべてのテキストが表示されるように、ボタンのサイズを変更します。

    (詳細については[、「方法:Windows フォーム](/dotnet/framework/winforms/controls/how-to-add-controls-to-windows-forms)にコントロールを追加します。)

1. UI に CompositionHostControl を追加します。

    - ツールボックスから Form1 に CompositionHostControl をドラッグします。 ボタンの右側に配置します。
    - CompositionHost のサイズを変更して、フォームの残りの部分を埋めるようにします。

1. ボタンのクリックイベントを処理します。

   - プロパティペインで、[稲妻] をクリックして [イベント] ビューに切り替えます。
   - [イベント] ボックスの一覧で、 **click**イベントを選択し、「 *Button_Click*」と入力して、enter キーを押します。
   - このコードは Form1.cs に追加されています。

    ```csharp
    private void Button_Click(object sender, EventArgs e)
    {

    }
    ```

1. 新しい要素を作成するには、ボタンクリックハンドラーにコードを追加します。

    - Form1.cs で、前に作成した*Button_Click*イベントハンドラーにコードを追加します。 このコードは、CompositionHostControl1 を呼び出して、ランダムに生成されたサイズとオフセットを持つ新しい要素を作成し_ます。_ (CompositionHostControl のインスタンスは、フォームにドラッグしたときに自動的に_compositionHostControl1_という名前になりました)。

    ```csharp
    // Add
    // using System;

    private void Button_Click(object sender, RoutedEventArgs e)
    {
        Random random = new Random();
        float size = random.Next(50, 150);
        float offsetX = random.Next(0, (int)(compositionHostControl1.Width - size));
        float offsetY = random.Next(0, (int)(compositionHostControl1.Height/2 - size));
        compositionHostControl1.AddElement(size, offsetX, offsetY);
    }
    ```

これで、Windows フォームアプリをビルドして実行できるようになりました。 このボタンをクリックすると、UI にアニメーション化された正方形が表示されます。

## <a name="next-steps"></a>次のステップ

同じインフラストラクチャ上に構築するより詳細な例については、 [Windows フォームのビジュアル レイヤーの統合サンプル](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WinForms/VisualLayerIntegration)GitHub でします。

## <a name="additional-resources"></a>その他の資料

- [Windows フォームでのはじめに](/dotnet/framework/winforms/getting-started-with-windows-forms).NET
- [アンマネージコードとの相互運用](/dotnet/framework/interop/).NET
- [Windows 10 アプリを使って](/windows/uwp/get-started/)みるUWP
- [Windows 10 用にデスクトップアプリケーションを拡張](/windows/uwp/porting/desktop-to-uwp-enhance)するUWP
- [Windows. UI. コンポジション名前空間](/uwp/api/windows.ui.composition)UWP

## <a name="complete-code"></a>コードを完成させる

このチュートリアルの完全なコードを次に示します。

### <a name="form1cs"></a>Form1.cs

```csharp
using System;
using System.Windows.Forms;

namespace HelloComposition
{
    public partial class Form1 : Form
    {
        public Form1()
        {
            InitializeComponent();
        }

        private void Button_Click(object sender, EventArgs e)
        {
            Random random = new Random();
            float size = random.Next(50, 150);
            float offsetX = random.Next(0, (int)(compositionHostControl1.Width - size));
            float offsetY = random.Next(0, (int)(compositionHostControl1.Height/2 - size));
            compositionHostControl1.AddElement(size, offsetX, offsetY);
        }
    }
}
```

### <a name="compositionhostcontrolcs"></a>CompositionHostControl.cs

```csharp
using System;
using System.Numerics;
using Windows.UI.Composition;

namespace HelloComposition
{
    class CompositionHostControl : CompositionHost
    {
        public CompositionHostControl() : base()
        {

        }

        public void AddElement(float size, float offsetX, float offsetY)
        {
            var visual = compositor.CreateSpriteVisual();
            visual.Size = new Vector2(size, size); // Requires references
            visual.Brush = compositor.CreateColorBrush(GetRandomColor());
            visual.Offset = new Vector3(offsetX, offsetY, 0);
            containerVisual.Children.InsertAtTop(visual);

            AnimateSquare(visual, 3);
        }

        private void AnimateSquare(SpriteVisual visual, int delay)
        {
            float offsetX = (float)(visual.Offset.X);
            Vector3KeyFrameAnimation animation = compositor.CreateVector3KeyFrameAnimation();
            float bottom = Height - visual.Size.Y;
            animation.InsertKeyFrame(1f, new Vector3(offsetX, bottom, 0f));
            animation.Duration = TimeSpan.FromSeconds(2);
            animation.DelayTime = TimeSpan.FromSeconds(delay);
            visual.StartAnimation("Offset", animation);
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
using System.Windows.Forms;
using Windows.UI.Composition;

namespace HelloComposition
{
    public partial class CompositionHost : Control
    {
        IntPtr hwndHost;
        object dispatcherQueue;
        protected ContainerVisual containerVisual;
        protected Compositor compositor;
        private ICompositionTarget compositionTarget;

        public Visual Child
        {
            set
            {
                if (compositor == null)
                {
                    InitComposition(hwndHost);
                }
                compositionTarget.Root = value;
            }
        }

        public CompositionHost()
        {
            // Get the window handle.
            hwndHost = Handle;

            // Create dispatcher queue.
            dispatcherQueue = InitializeCoreDispatcher();

            // Build Composition tree of content.
            InitComposition(hwndHost);
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

            compositor = new Compositor();
            object iunknown = compositor as object;
            interop = (ICompositorDesktopInterop)iunknown;
            IntPtr raw;
            interop.CreateDesktopWindowTarget(hwndHost, true, out raw);

            object rawObject = Marshal.GetObjectForIUnknown(raw);
            compositionTarget = (ICompositionTarget)rawObject;

            if (raw == null) { throw new Exception("QI Failed"); }

            containerVisual = compositor.CreateContainerVisual();
            Child = containerVisual;
        }

        protected override void OnPaint(PaintEventArgs pe)
        {
            base.OnPaint(pe);
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