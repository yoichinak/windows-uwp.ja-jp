---
description: このチュートリアルでは、UWP XAML ユーザーインターフェイスを追加する方法、MSIX パッケージを作成する方法、およびその他の最新のコンポーネントを WPF アプリに組み込む方法について説明します。
title: XAML Islands を使用した UWP InkCanvas コントロールの追加
ms.topic: article
ms.date: 01/10/2010
ms.author: mcleans
author: mcleanbyron
keywords: windows 10、uwp、windows フォーム、wpf、xaml islands
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 1ed4fa54b9987ef0ab5f3dd89aa9d11a00ae695c
ms.sourcegitcommit: 85fd390b1e602707bd9342cb4b84b97ae0d8b831
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/22/2020
ms.locfileid: "76520437"
---
# <a name="part-2-add-a-uwp-inkcanvas-control-using-xaml-islands"></a>パート 2: XAML アイランドを使用した UWP System.windows.controls.inkcanvas> コントロールの追加

これは、Contoso の支出という名前の WPF デスクトップアプリのサンプルを最新化する方法を示すチュートリアルの2番目の部分です。 サンプルアプリをダウンロードするためのチュートリアル、前提条件、および手順の概要については、「[チュートリアル: WPF アプリの](modernize-wpf-tutorial.md)最新化」を参照してください。 この記事では、既に[パート 1](modernize-wpf-tutorial-1.md)を完了していることを前提としています。

このチュートリアルの架空のシナリオでは、Contoso 開発チームは、デジタル署名のサポートを Contoso の経費アプリに追加する必要があります。 UWP **system.windows.controls.inkcanvas>** コントロールは、テキストと図形を認識する機能のようなデジタルインクと AI を使用した機能をサポートするため、このシナリオに適したオプションです。 これを行うには、Windows Community Toolkit で使用可能な[system.windows.controls.inkcanvas>](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)ラップされた UWP コントロールを使用します。 このコントロールは、WPF アプリで使用する UWP **system.windows.controls.inkcanvas>** コントロールのインターフェイスと機能をラップします。 ラップされた UWP コントロールの詳細については、次を参照してください。[デスクトップ アプリ (XAML Islands) でコントロールをホスト UWP XAML](xaml-islands.md)します。

> [!NOTE]
> このチュートリアルでは、WPF アプリは Windows SDK からのファーストパーティ UWP コントロールのみをホストします。 カスタム UWP コントロールを含む他の XAML アイランドのシナリオをサポートするには、アプリプロジェクトが Windows Community Toolkit によって提供される `Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication` クラスのインスタンスにアクセスできる必要があります。 これを行うには、WPF (または Windows フォーム) プロジェクトと同じソリューションに**空のアプリ (ユニバーサル Windows)** プロジェクトを追加し、このプロジェクトの既定の `App` クラスを変更することをお勧めします。 Windows SDK からファーストパーティの UWP コントロールをホストする基本的なシナリオでは、この手順は必要ありません。このチュートリアルでは、この手順を省略します。 詳細については、[こちらの記事](host-standard-control-with-xaml-islands.md)を参照してください。

## <a name="configure-the-project-to-use-xaml-islands"></a>XAML Islandsを使用するプロジェクトを構成します。

追加する前に、 **InkCanvas** UWP XAML Islandsをサポートするためにプロジェクトを構成する最初必要である Contoso 経費アプリを制御します。

1. Visual Studio 2019 で**ソリューションエクスプローラー**で**ContosoExpenses**プロジェクトを右クリックし、 **[NuGet パッケージの管理]** を選択します。

    ![Visual Studio の [NuGet パッケージの管理] メニュー](images/wpf-modernize-tutorial//ManageNuGetPackages.png)

2. **[NuGet パッケージマネージャー]** ウィンドウで、 **[参照]** をクリックします。 `Microsoft.Toolkit.Wpf.UI.Controls` パッケージを検索し、バージョン6.0.0 以降のバージョンをインストールします。

    > [!NOTE]
    > このパッケージには、WPF アプリケーションでは、UWP XAML Islandsをホストするためのすべての必要なインフラストラクチャが含まれています。 など、 **InkCanvas** UWP コントロールをラップします。 Windows フォームアプリには、`Microsoft.Toolkit.Forms.UI.Controls` という名前の同様のパッケージを使用できます。

3. **ソリューションエクスプローラー**で **[ContosoExpenses]** プロジェクト を右クリックし、[**追加] > [新しい項目**] の順に選択します。

4. **[アプリケーションマニフェストファイル]** を選択し、「 **app.xaml**」という名前を指定して、 **[追加]** をクリックします。 アプリケーションマニフェストの詳細については、こちらの[記事](https://docs.microsoft.com/windows/desktop/SbsCs/application-manifests)を参照してください。

5. マニフェストファイルで、Windows 10 の次の `<supportedOS>` 要素のコメントを解除します。

    ```xml
    <!-- Windows 10 -->
    <supportedOS Id="{8e0f7a12-bfb3-4fe8-b9a5-48fd50a15a9a}" />
    ```

6. マニフェストファイルで、次のコメントが付いた `<application>` 要素を見つけます。

    ```xml
    <!--
    <application xmlns="urn:schemas-microsoft-com:asm.v3">
      <windowsSettings>
        <dpiAware xmlns="http://schemas.microsoft.com/SMI/2005/WindowsSettings">true</dpiAware>
      </windowsSettings>
    </application>
    -->
    ```

7. このセクションを削除し、次の XML に置き換えます。 これにより、アプリケーションが DPI 対応になり、Windows 10 でサポートされるさまざまなスケーリング要因をより適切に処理できるようになります。

    ```xml
    <application xmlns="urn:schemas-microsoft-com:asm.v3">
      <windowsSettings>
          <dpiAware xmlns="http://schemas.microsoft.com/SMI/2005/WindowsSettings">true/PM</dpiAware>
          <dpiAwareness xmlns="http://schemas.microsoft.com/SMI/2016/WindowsSettings">PerMonitorV2, PerMonitor</dpiAwareness>
      </windowsSettings>
    </application>
    ```

8. `app.manifest` ファイルを保存して閉じます。

9. **ソリューションエクスプローラー**で、 **ContosoExpenses**プロジェクトを右クリックし、 **[プロパティ]** を選択します。

10. **[アプリケーション]** タブの **[リソース]** セクションで、 **[マニフェスト]** ドロップダウンが **[app.xaml]** に設定されていることを確認します。

    ![.NET Core アプリケーションマニフェスト](images/wpf-modernize-tutorial/NetCoreAppManifest.png)

11. プロジェクトプロパティへの変更を保存します。

## <a name="add-an-inkcanvas-control-to-the-app"></a>System.windows.controls.inkcanvas> コントロールをアプリに追加する

追加する準備ができました UWP XAML Islandsを使用するプロジェクトを構成したところ、 [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)をアプリに UWP コントロールをラップします。

1. **ソリューションエクスプローラー**で、ContosoExpenses プロジェクトの**Views**フォルダーを展開し、[ **] ファイルを**ダブルクリックします。

2. XAML ファイルの先頭付近にある**ウィンドウ**要素に、次の属性を追加します。 これは、 [system.windows.controls.inkcanvas>](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)ラップされた UWP コントロールの XAML 名前空間を参照します。

    ```xml
    xmlns:toolkit="clr-namespace:Microsoft.Toolkit.Wpf.UI.Controls;assembly=Microsoft.Toolkit.Wpf.UI.Controls"
    ```

    この属性を追加すると、**ウィンドウ**要素は次のようになります。

    ```xml
    <Window x:Class="ContosoExpenses.Views.ExpenseDetail"
            xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
            xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
            xmlns:toolkit="clr-namespace:Microsoft.Toolkit.Wpf.UI.Controls;assembly=Microsoft.Toolkit.Wpf.UI.Controls"
            xmlns:converters="clr-namespace:ContosoExpenses.Converters"
            DataContext="{Binding Source={StaticResource ViewModelLocator}, Path=ExpensesDetailViewModel}"
            xmlns:local="clr-namespace:ContosoExpenses"
            mc:Ignorable="d"
            Title="Expense Detail" Height="500" Width="800"
            Background="{StaticResource HorizontalBackground}">
    ```

4. [`<!-- Chart -->`] コメントの直前にある [終了] `</Grid>` タグを見つけ**ます。** 終了 `</Grid>` タグの直前に、次の XAML を追加します。 この XAML は、 **system.windows.controls.inkcanvas>** コントロール (前に名前空間として定義した**toolkit**キーワードで始まります) と、コントロールのヘッダーとして機能する単純な**TextBlock**を追加します。

    ```xml
    <TextBlock Text="Signature:" FontSize="16" FontWeight="Bold" Grid.Row="5" />

    <toolkit:InkCanvas x:Name="Signature" Grid.Row="6" />
    ```

5. このファイル**を保存します**。

6. F5 キーを押して、デバッガーでアプリを実行します。

7. 一覧から従業員を選択し、使用可能な経費の1つを選択します。 [経費詳細] ページに、 **system.windows.controls.inkcanvas>** コントロールの領域が含まれていることに注意してください。

    ![インクキャンバスのペンのみ](images/wpf-modernize-tutorial/InkCanvasPenOnly.png)

    表面のようなデジタルペンをサポートするデバイスがあり、このラボを物理マシンで実行している場合は、「」に進んで、使用してみてください。 画面にデジタルインクが表示されます。 ただし、ペンに対応したデバイスを持っておらず、マウスでサインインしようとすると、何も起こりません。 この問題が発生するのは、既定ではデジタルペンに対してのみ**system.windows.controls.inkcanvas>** コントロールが有効になっているためです。 ただし、この動作は変更できます。

8. アプリを終了し、 **ContosoExpenses**プロジェクトの**Views**フォルダーの下にある**ExpenseDetail.xaml.cs**ファイルをダブルクリックします。

9. クラスの先頭に、次の名前空間宣言を追加します。

    ```csharp
    using Microsoft.Toolkit.Win32.UI.Controls.Interop.WinRT;
    ```

10. `ExpenseDetail()` コンストラクターを見つけます。

11. `InitializeComponent()` メソッドの後に次のコード行を追加し、コードファイルを保存します。

    ```csharp
    Signature.InkPresenter.InputDeviceTypes = CoreInputDeviceTypes.Mouse | CoreInputDeviceTypes.Pen;
    ```

    **InkPresenter**オブジェクトを使用して、既定のインク操作をカスタマイズできます。 このコードでは、 **Inputdevicetypes**プロパティを使用して、マウスおよびペン入力を有効にします。

12. F5 キーをもう一度押して、デバッガーでアプリをリビルドして実行します。 一覧から従業員を選択し、使用可能な経費の1つを選択します。

13. マウスで署名スペースに何かを描画してみましょう。 今度は、画面にインクが表示されます。

    ![Signature](images/wpf-modernize-tutorial/Signature.png)

## <a name="next-steps"></a>次の手順

このチュートリアルのこの時点で、UWP **system.windows.controls.inkcanvas>** コントロールが Contoso の経費アプリに追加されました。 これで、 [「パート 3: XAML アイランドを使用した UWP CalendarView コントロールの追加](modernize-wpf-tutorial-3.md)」の準備ができました。
