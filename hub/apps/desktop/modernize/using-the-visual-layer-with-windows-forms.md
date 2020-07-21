---
title: Windows フォームでのビジュアル レイヤーの使用
description: ビジュアル レイヤー API を既存の Windows フォーム コンテンツと組み合わせて使用し、高度なアニメーションや効果を作成する方法について説明します。
ms.date: 03/18/2019
ms.topic: article
keywords: windows 10, uwp
ms.author: jimwalk
author: jwmsft
ms.localizationpriority: medium
ms.openlocfilehash: 9da9dee48beef6e3c1cd38ffbe9761ed89fd940d
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/29/2020
ms.locfileid: "69999639"
---
# <a name="using-the-visual-layer-with-windows-forms"></a>Windows フォームでのビジュアル レイヤーの使用

Windows フォーム アプリで Windows ランタイムの Composition API ([ビジュアル レイヤー](/windows/uwp/composition/visual-layer)とも呼ばれる) を使用して、Windows 10 ユーザーの利便性を高める最新のエクスペリエンスを作成できます。

このチュートリアルの完全なコードは、GitHub で入手できます。[Windows フォーム HelloComposition サンプル](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WinForms/HelloComposition)

## <a name="prerequisites"></a>前提条件

UWP ホスティング API には、次の前提条件があります。

- Windows フォームと UWP を使用したアプリ開発に関する一定の知識があることを前提としています。 For more info, see:
  - [Windows フォームについて](/dotnet/framework/winforms/getting-started-with-windows-forms)
  - [Windows 10 アプリの概要](/windows/uwp/get-started/)
  - [Windows 10 向けのデスクトップ アプリケーションの強化](/windows/uwp/porting/desktop-to-uwp-enhance)
- .NET Framework 4.7.2 以降
- Windows 10 バージョン 1803 以降
- Windows 10 SDK 17134 以降

## <a name="how-to-use-composition-apis-in-windows-forms"></a>Windows フォームで Composition API を使用する方法

このチュートリアルでは、シンプルな Windows フォーム UI を作成して、アニメーション化したコンポジション要素をそこに追加します。 Windows フォームとコンポジション要素は両方ともシンプルになっていますが、示されている相互運用コードは、コンポーネントの複雑さに関係なく同じです。 完成したアプリは次のようになります。

![実行中のアプリの UI](images/visual-layer-interop/wf-comp-interop-app-ui.png)

## <a name="create-a-windows-forms-project"></a>Windows フォーム プロジェクトを作成する

最初の手順は、Windows フォーム アプリ プロジェクトを作成することであり、アプリケーション定義と UI のメイン フォームが含まれます。

Visual C# で _HelloComposition_ という名前の新しい Windows フォーム アプリケーション プロジェクトを作成するには:

1. Visual Studio を開き、 **[ファイル]**  >  **[新規作成]**  >  **[プロジェクト]** の順に選択します。<br/>**[新しいプロジェクト]** ダイアログが開きます。
1. **[インストール済み]** カテゴリで、 **[Visual C#]** ノードを展開し、 **[Windows デスクトップ]** を選択します。
1. **[Windows フォーム アプリケーション (.NET Framework)]** テンプレートを選択します。
1. 名前「_HelloComposition_」を入力し、 **[.NET Framework 4.7.2]** フレームワークを選択して、 **[OK]** をクリックします。

Visual Studio によってプロジェクトが作成され、Form1.cs という名前の既定のアプリケーション ウィンドウのデザイナーが開きます。

## <a name="configure-the-project-to-use-windows-runtime-apis"></a>Windows ランタイム API を使用するようにプロジェクトを構成する

Windows フォーム アプリで Windows ランタイム (WinRT) API を使用するには、Windows ランタイムにアクセスするように、Visual Studio プロジェクトを構成する必要があります。 さらに、Composition API ではベクトルが広範囲に使用されるため、ベクトルを使用するために必要な参照を追加する必要があります。

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

## <a name="create-a-custom-control-to-manage-interop"></a>相互運用を管理するためのカスタム コントロールを作成する

ビジュアル レイヤーを使って作成したコンテンツをホストするには、[コントロール](/dotnet/api/system.windows.forms.control)から派生したカスタム コントロールを作成します。 このコントロールを使用して、ビジュアル レイヤー コンテンツ用のコンテナーを作成するために必要なウィンドウ [ハンドル](/dotnet/api/system.windows.forms.control.handle)にアクセスできます。

Composition API をホストするための構成のほとんどを、ここで行います。 このコントロールでは、[プラットフォーム呼び出しサービス (PInvoke)](/cpp/dotnet/calling-native-functions-from-managed-code) と [COM 相互運用](/dotnet/api/system.runtime.interopservices.comimportattribute)を使用して、Composition API を Windows フォーム アプリに取り込みます。 PInvoke と COM 相互運用の詳細については、「[アンマネージド コードとの相互運用](/dotnet/framework/interop/index)」を参照してください。

> [!TIP]
> チュートリアルを進めながら、必要に応じて、チュートリアルの最後にある完全なコードを確認して、すべてのコードが適切な場所にあることを確認してください。

1. [Control](/dotnet/api/system.windows.forms.control) から派生する新しいカスタム コントロール ファイルをプロジェクトに追加します。
    - **ソリューション エクスプローラー**で、_HelloComposition_ プロジェクトを右クリックします。
    - コンテキスト メニューで、 **[追加]**  >  **[新しい項目...]** の順に選択します。
    - **[新しい項目の追加]** ダイアログで、 **[カスタム コントロール]** を選択します。
    - コントロールに _CompositionHost.cs_ という名前を付けて、 **[追加]** をクリックします。 デザイン ビューで CompositionHost.cs が開かれます。

1. CompositionHost.cs のコード ビューに切り替えて、次のコードをクラスに追加します。

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

    コンストラクター内で _InitializeCoreDispatcher_ メソッドと _InitComposition_ メソッドを呼び出します。 これらのメソッドは、次の手順で作成します。

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

1. [CoreDispatcher](/uwp/api/windows.ui.core.coredispatcher) でスレッドを初期化します。 コア ディスパッチャーは、ウィンドウ メッセージの処理と WinRT API のイベントのディスパッチを担当します。 **Compositor** の新しいインスタンスは、CoreDispatcher があるスレッド上で作成する必要があります。
    - _InitializeCoreDispatcher_ という名前のメソッドを作成し、ディスパッチャー キューをセットアップするコードを追加します。

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

    - ディスパッチャー キューには、PInvoke 宣言が必要になります。 クラスのコードの末尾にこの宣言を配置します  (クラス コードを整理しておくために、ここでは、このコードをリージョン内に配置します)。

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

    これで、ディスパッチャー キューを準備できたので、コンポジション コンテンツの初期化と作成を開始できます。

1. [Compositor](/uwp/api/windows.ui.composition.compositor) を初期化します。 Compositor は、ビジュアル レイヤー、効果システム、およびアニメーション システムにまたがる [Windows.UI.Composition](/uwp/api/windows.ui.composition) 名前空間にさまざまな型を作成するファクトリです。 また、Compositor クラスでは、ファクトリから作成されたオブジェクトの有効期間も管理されます。

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

    - **ICompositorDesktopInterop** と **ICompositionTarget** には COM インポートが必要です。 このコードは _CompositionHost_ クラスの後に、名前空間の宣言内に配置します。

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

## <a name="create-a-custom-control-to-host-composition-elements"></a>コンポジション要素をホストするカスタム コントロールを作成する

コンポジション要素を生成して CompositionHost から派生した別のコントロール内に管理するコードを挿入することをお勧めします。 これにより、CompositionHost クラスで作成した相互運用コードを引き続き再利用できます。

ここでは、CompositionHost から派生したカスタム コントロールを作成します。 このコントロールは、フォームに追加できるように、Visual Studio のツールボックスに追加されます。

1. CompositionHost から派生した新しいカスタム コントロール ファイルをプロジェクトに追加します。
    - **ソリューション エクスプローラー**で、_HelloComposition_ プロジェクトを右クリックします。
    - コンテキスト メニューで、 **[追加]**  >  **[新しい項目...]** の順に選択します。
    - **[新しい項目の追加]** ダイアログで、 **[カスタム コントロール]** を選択します。
    - コントロールに _CompositionHostControl.cs_ という名前を付けて、 **[追加]** をクリックします。 デザイン ビューで CompositionHostControl.cs が開かれます。

1. CompositionHostControl.cs のデザイン ビューのプロパティ ペインで、**BackColor** プロパティを **ControlLight** に設定します。

    背景色の設定は省略可能です。 ここでは、フォームの背景に対してカスタム コントロールを表示できるようにします。

1. CompositionHostControl.cs のコード ビューに切り替えて、CompositionHost から派生するようにクラス宣言を更新します。

    ```csharp
    class CompositionHostControl : CompositionHost
    ```

1. コンストラクターを更新して、基本コンストラクターを呼び出します。

    ```csharp
    public CompositionHostControl() : base()
    {

    }
    ```

### <a name="add-composition-elements"></a>コンポジション要素を追加する

インフラストラクチャが配置されたので、コンポジション コンテンツをアプリの UI に追加できるようになりました。

この例では、シンプルな [SpriteVisual](/uwp/api/windows.ui.composition.spritevisual) を作成してアニメーション化するコードを CompositionHostControl クラスに追加します。

1. コンポジション要素を追加します。

    CompositionHostControl.cs で、次のメソッドを CompositionHostControl クラスに追加します。

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

## <a name="add-the-control-to-your-form"></a>コントロールをフォームに追加する

コンポジション コンテンツをホストするカスタム コントロールを用意できたので、次は、それをアプリ UI に追加します。 ここでは、前の手順で作成した CompositionHostControl のインスタンスを追加します。 CompositionHostControl は、Visual Studio ツールボックスの **[<_プロジェクト名_> コンポーネント]** の下に自動的に追加されます。

1. Form1.CS デザイン ビューで、UI にボタンを追加します。

    - ツールボックスから Form1 上にボタンをドラッグします。 フォームの左上隅に配置します  (コントロールの配置を確認するには、チュートリアルの冒頭にあるイメージを参照してください)。
    - プロパティ ペインで、**Text** プロパティを _button1_ から "_Add composition element (コンポジション要素の追加)_ " に変更します。
    - すべてのテキストが表示されるように、ボタンのサイズを変更します。

    (詳細については、[方法: Windows フォームにコントロールを追加する](/dotnet/framework/winforms/controls/how-to-add-controls-to-windows-forms)」を参照してください。)

1. UI に CompositionHostControl を追加します。

    - ツールボックスから Form1 上に CompositionHostControl をドラッグします。 それをボタンの右に配置します。
    - フォームの残りの部分を埋めるように、CompositionHost のサイズを変更します。

1. ボタンのクリック イベントを処理します。

   - プロパティ ペインで、稲妻をクリックして [イベント] ビューに切り替えます。
   - イベントの一覧で、**Click** イベントを選択して、「*Button_Click*」と入力して、Enter キーを押します。
   - 次のコードが Form1.cs に追加されます。

    ```csharp
    private void Button_Click(object sender, EventArgs e)
    {

    }
    ```

1. ボタン クリック ハンドラーにコードを追加して、新しい要素を作成します。

    - Form1.cs 上で、前に作成した*Button_Click*イベント ハンドラーにコードを追加します。 このコードでは _CompositionHostControl1.AddElement_ を呼び出して、ランダムに生成されたサイズとオフセットで新しい要素を作成します。 (CompositionHostControl のインスタンスには、フォーム上にドラッグしたときに、_compositionHostControl1_ という名前が自動的に付けられました。)

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

これで、Windows フォーム アプリをビルドして実行できるようになりました。 ボタンをクリックすると、UI に追加したアニメーション化された正方形が表示されるはずです。

## <a name="next-steps"></a>次の手順

同じインフラストラクチャ上にビルドされるより完全な例については、GitHub の「[Windows フォーム ビジュアル レイヤー統合サンプル](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WinForms/VisualLayerIntegration)」を参照してください。

## <a name="additional-resources"></a>その他の資料

- [Windows フォームについて](/dotnet/framework/winforms/getting-started-with-windows-forms) (.NET)
- [アンマネージド コードとの相互運用](/dotnet/framework/interop/) (.NET)
- [Windows 10 アプリの概要](/windows/uwp/get-started/) (UWP)
- [Windows 10 向けのデスクトップ アプリケーションの強化](/windows/uwp/porting/desktop-to-uwp-enhance) (UWP)
- [Windows.UI.Composition 名前空間](/uwp/api/windows.ui.composition) (UWP)

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