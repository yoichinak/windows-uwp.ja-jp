---
description: このチュートリアルでは、UWP XAML ユーザー インターフェイスを追加し、MSIX パッケージを作成し、その他の最新のコンポーネントをお使いの WPF アプリに組み込む方法について説明します。
title: XAML Islands を使用した UWP InkCanvas コントロールの追加
ms.topic: article
ms.date: 01/24/2020
ms.author: mcleans
author: mcleanbyron
keywords: windows 10、uwp、windows フォーム、wpf、xaml islands
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 0b5250f1e01aece4f73d83dc7327f193a58f53cf
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89161496"
---
# <a name="part-2-add-a-uwp-inkcanvas-control-using-xaml-islands"></a>パート 2: XAML Islands を使用した UWP InkCanvas コントロールの追加

これは、Contoso Expenses という名前のサンプル WPF デスクトップ アプリを最新化する方法を示すチュートリアルの 2 番目の部分です。 チュートリアルの概要、前提条件、サンプル アプリをダウンロードする手順については、「[チュートリアル:WPF アプリの最新化](modernize-wpf-tutorial.md)」を参照してください。 この記事では、読者が[パート 1](modernize-wpf-tutorial-1.md) を既に完了していることを前提にしています。

このチュートリアルの架空のシナリオでは、Contoso 開発チームが Contoso の経費アプリにデジタル署名を追加したいと考えています。 このシナリオには、デジタル インクや AI を利用したテキストや図形認識機能などをサポートする UWP **InkCanvas** コントロールが適した選択肢です。 これを行うには、Windows コミュニティ ツールキットにある [InkCanvas](/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) でラップされた UWP コントロールを使用します。 このコントロールでは、WPF アプリで使用できるよう UWP **InkCanvas** コントロールのインターフェイスと機能がラップされています。 ラップされた UWP コントロールの詳細については、「[デスクトップ アプリで UWP XAML コントロールをホストする (XAML Islands)](xaml-islands.md)」を参照してください。

## <a name="configure-the-project-to-use-xaml-islands"></a>XAML Islands を使用するためのプロジェクトの構成

**InkCanvas** コントロールを Contoso の経費アプリに追加する前に、プロジェクトで UWP XAML Islands をサポートする構成をまず実行する必要があります。

1. Visual Studio 2019 の**ソリューション エクスプローラー**で **ContosoExpenses.Core** プロジェクトを右クリックし、 **[NuGet パッケージの管理]** を選択します。

    ![Visual Studio の [NuGet パッケージの管理] メニュー](images/wpf-modernize-tutorial//ManageNuGetPackages.png)

2. **[NuGet パッケージ マネージャー]** ウィンドウで、 **[参照]** をクリックします。 `Microsoft.Toolkit.Wpf.UI.Controls` パッケージを検索し、バージョン 6.0.0 以降のバージョンをインストールします。

    > [!NOTE]
    > このパッケージには、**InkCanvas** でラップされた UWP コントロールを含む、WPF アプリで UWP XAML Islands をホストするために必要なすべてのインフラストラクチャが含まれています。 Windows フォーム アプリ用には、`Microsoft.Toolkit.Forms.UI.Controls` という名前の同様のパッケージがあります。

3. **ソリューション エクスプローラー**で **ContosoExpenses.Core** プロジェクトを右クリックし、 **[追加]、[新しい項目]** の順に選択します。

4. **アプリケーション マニフェスト ファイル**を選択し、**app.manifest** という名前を付け、 **[追加]** をクリックします。 アプリケーション マニフェストの詳細については、[こちらの記事](/windows/desktop/SbsCs/application-manifests)を参照してください。

5. このマニフェスト ファイルで、次の Windows 10 の `<supportedOS>` 要素をコメント解除します。

    ```xml
    <!-- Windows 10 -->
    <supportedOS Id="{8e0f7a12-bfb3-4fe8-b9a5-48fd50a15a9a}" />
    ```

6. マニフェスト ファイルで、次のコメントが付いた `<application>` 要素を探します。

    ```xml
    <!--
    <application xmlns="urn:schemas-microsoft-com:asm.v3">
      <windowsSettings>
        <dpiAware xmlns="http://schemas.microsoft.com/SMI/2005/WindowsSettings">true</dpiAware>
      </windowsSettings>
    </application>
    -->
    ```

7. このセクションを削除し、次の XML と置き換えます。 これにより、アプリは DPI 対応になり、Windows 10 がサポートするその他のスケーリング要因より適切に処理するようになります。

    ```xml
    <application xmlns="urn:schemas-microsoft-com:asm.v3">
      <windowsSettings>
          <dpiAware xmlns="http://schemas.microsoft.com/SMI/2005/WindowsSettings">true/PM</dpiAware>
          <dpiAwareness xmlns="http://schemas.microsoft.com/SMI/2016/WindowsSettings">PerMonitorV2, PerMonitor</dpiAwareness>
      </windowsSettings>
    </application>
    ```

8. `app.manifest` ファイルを保存し、閉じます。

9. **ソリューション エクスプローラー**で **ContosoExpenses.Core** プロジェクトを右クリックし、 **[プロパティ]** を選択します。

10. **[アプリケーション]** タブの **[リソース]** セクションで、 **[マニフェスト]** ドロップダウンが **[app.manifest]** に設定されていることを確認します。

    ![.NET Core アプリ マニフェスト](images/wpf-modernize-tutorial/NetCoreAppManifest.png)

11. プロジェクト プロパティに変更を保存します。

## <a name="add-an-inkcanvas-control-to-the-app"></a>アプリへの InkCanvas コントロールの追加

これでご自分のプロジェクトで UWP XAML Islands を使用する構成が完了したので、アプリに [InkCanvas](/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) でラップされた UWP コントロールを追加できます。

1. **ソリューション エクスプローラー**で、**ContosoExpenses.Core** プロジェクトの **[ビュー]** フォルダーを展開し、**ExpenseDetail.xaml** ファイルをダブルクリックします。

2. XAML ファイル上部付近の **Window** 要素に、次の属性を追加します。 これにより、[InkCanvas](/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) でラップされた UWP コントロールの XAML 名前空間が参照されるようになります。

    ```xml
    xmlns:toolkit="clr-namespace:Microsoft.Toolkit.Wpf.UI.Controls;assembly=Microsoft.Toolkit.Wpf.UI.Controls"
    ```

    **Window** 要素は、この属性の追加後、次のようになります。

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

4. **ExpenseDetail.xaml** ファイルで、`<!-- Chart -->` の直前にある `</Grid>` 終了タグを探します。 次の XAML を、`</Grid>` 終了タグの直前に追加します。 この XAML により (前に名前空間として定義した **toolkit** キーワードで始まる) **InkCanvas** コントロールと、コントロールのヘッダーとして機能する単純な **TextBlock** が追加されます。

    ```xml
    <TextBlock Text="Signature:" FontSize="16" FontWeight="Bold" Grid.Row="5" />

    <toolkit:InkCanvas x:Name="Signature" Grid.Row="6" />
    ```

5. **ExpenseDetail.xaml** ファイルを保存します。

6. F5 キーを押して、デバッガーでアプリを実行します。

7. 一覧から従業員を選択し、使用可能な経費の 1 つを選択します。 経費詳細ページに、**InkCanvas** コントロール用の領域が用意されたことに注目してください。

    ![インク キャンバス ペンのみ](images/wpf-modernize-tutorial/InkCanvasPenOnly.png)

    デジタル ペンをサポートする Surface などのデバイスがあり、この演習を物理マシンで実行している場合は、このままそれの使用を継続してください。 画面にデジタル インクが表示されます。 ペン対応のデバイスを使用しておらず、ご自分のマウスで署名しようとしている場合は、何も起こりません。 これは、**InkCanvas** コントロールがデジタル ペンに対してのみ既定で有効になるためです。 ただし、この動作は変更できます。

8. アプリを閉じ、**ContosoExpenses.Core** プロジェクトの **[参照]** フォルダーの下の **ExpenseDetail.xaml.cs** ファイルをダブルクリックします。

9. このクラスの上部に、次の名前空間宣言を追加します。

    ```csharp
    using Microsoft.Toolkit.Win32.UI.Controls.Interop.WinRT;
    ```

10. `ExpenseDetail()` コンストラクターを探します。

11. `InitializeComponent()` メソッドの後に次のコード行を追加し、コード ファイルを保存します。

    ```csharp
    Signature.InkPresenter.InputDeviceTypes = CoreInputDeviceTypes.Mouse | CoreInputDeviceTypes.Pen;
    ```

    **InkPresenter** オブジェクトを使用して、既定のインク エクスペリエンスをカスタマイズできます。 このコードでは、**InputDeviceTypes** プロパティを使用して、マウスおよびペン入力を有効にします。

12. F5 キーを再度押して、デバッガーでアプリをリビルドして実行します。 一覧から従業員を選択し、使用可能な経費の 1 つを選択します。

13. マウスで署名スペースに何かを描画してみましょう。 今回は画面にインクが表示されます。

    ![署名](images/wpf-modernize-tutorial/Signature.png)

## <a name="next-steps"></a>次の手順

このチュートリアルのこの時点では、Contoso の経費アプリに UWP **InkCanvas** コントロールが追加されています。 これで、「[パート 3:XAML Islands を使用した UWP CalendarView コントロールの追加](modernize-wpf-tutorial-3.md)」に進むことができます。