---
description: このチュートリアルでは、UWP XAML ユーザー インターフェイスを追加、MSIX パッケージを作成およびその他の最新コンポーネントを WPF アプリに組み込む方法を示します。
title: XAML Islandsを使って UWP InkCanvas コントロールを追加
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: windows 10、uwp、windows フォーム、wpf、xaml islands
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 2f8cf18bce7bec880a2cb0bef298c0b565e20208
ms.sourcegitcommit: 1eec0e4fd8a5ba82803fdce6e23fcd01b9488523
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/27/2019
ms.locfileid: "67420091"
---
# <a name="part-2-add-a-uwp-inkcanvas-control-using-xaml-islands"></a>パート 2:XAML Islandsを使って UWP InkCanvas コントロールを追加

これは、Contoso の経費をという名前のサンプル WPF デスクトップ アプリの近代化する方法を説明するチュートリアルの 2 番目の部分です。 チュートリアル、前提条件、およびサンプル アプリをダウンロードする手順の概要については、次を参照してください。[チュートリアル。WPF アプリの近代化](modernize-wpf-tutorial.md)します。 この記事では、既に完了している前提としています。[パート 1](modernize-wpf-tutorial-1.md)します。

このチュートリアルの架空のシナリオでは、contoso 社の開発チームが、Contoso 経費アプリにデジタル署名のサポートを追加します。 UWP **InkCanvas**デジタル インクおよびテキストや図形を認識する機能などの AI を利用した機能をサポートしているために、コントロールがこのシナリオで便利なオプションです。 これを行うには、使用して、 [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) Windows コミュニティ Toolkit で使用可能な UWP コントロールをラップします。 このコントロールは、インターフェイスと、UWP の機能をラップ**InkCanvas** WPF アプリで使用されるコントロール。 ラップされた UWP コントロールの詳細については、次を参照してください。[デスクトップ アプリ (XAML Islands) でコントロールをホスト UWP XAML](xaml-islands.md)します。

## <a name="configure-the-project-to-use-xaml-islands"></a>XAML Islandsを使用するプロジェクトを構成します。

追加する前に、 **InkCanvas** UWP XAML Islandsをサポートするためにプロジェクトを構成する最初必要である Contoso 経費アプリを制御します。

1. Visual Studio 2019 を右クリックし、 **ContosoExpenses.Core**プロジェクト**ソリューション エクスプ ローラー**選択**NuGet パッケージの管理**します。

    ![Visual Studio のメニューの NuGet パッケージを管理します。](images/wpf-modernize-tutorial//ManageNuGetPackages.png)

2. **NuGet パッケージ マネージャー**ウィンドウで、をクリックして**参照**します。 選択、**プレリリースを含める**オプションで、検索、`Microsoft.Toolkit.Wpf.UI.Controls`パッケージ化、および結果に示すように、パッケージの最新のプレビュー リリースをインストールします。

    > [!NOTE]
    > このパッケージには、WPF アプリケーションでは、UWP XAML Islandsをホストするためのすべての必要なインフラストラクチャが含まれています。 など、 **InkCanvas** UWP コントロールをラップします。 という名前のようなパッケージ`Microsoft.Toolkit.Forms.UI.Controls`は Windows フォーム アプリで利用できます。

3. 右クリック**ContosoExpenses.Core**プロジェクト**ソリューション エクスプ ローラー**選択**追加]、[新しい項目の**します。

4. 選択**アプリケーション マニフェスト ファイル**、名前を付けます**app.manifest**、 をクリック**追加**します。

5. マニフェスト ファイルを開いたときに検索、**互換性**セクションし、次のコメントが付けられたエントリを識別します。

    ```xml
    <!-- Windows 10 -->
    <!--<supportedOS Id="{8e0f7a12-bfb3-4fe8-b9a5-48fd50a15a9a}" />-->
    ```

6. このエントリを下には、次の項目を追加します。

    ```xml
    <maxversiontested Id="10.0.18362.0"/>
    ```

7. コメントを解除、 **supportedOS** Windows 10 用のエントリ。 このセクションでは、次のようになります。

    ```xml
    <!-- Windows 10 -->
    <supportedOS Id="{8e0f7a12-bfb3-4fe8-b9a5-48fd50a15a9a}" />
    <maxversiontested Id="10.0.18362.0"/>
    ```

    > [!NOTE]
    > このエントリは、アプリが Windows 10 バージョン 1903 (ビルド 18362) が必要であるを指定しますまたはそれ以降。 これは、XAML Islandsをサポートする Windows 10 の最初のバージョンです。 アプリでは、アプリケーション マニフェストにこのエントリが、実行時に例外がスローされます。

8. マニフェストのファイルで、次のコメントを探します**アプリケーション**セクション。

    ```xml
    <!--
    <application xmlns="urn:schemas-microsoft-com:asm.v3">
      <windowsSettings>
        <dpiAware xmlns="http://schemas.microsoft.com/SMI/2005/WindowsSettings">true</dpiAware>
      </windowsSettings>
    </application>
    -->
    ```

9. このセクションを削除し、次の XML に置き換えます。 これには、DPI 対応であり、効率的ハンドルは異なるスケーリング要因 Windows 10 ではサポートするアプリが構成されます。

    ```xml
    <application xmlns="urn:schemas-microsoft-com:asm.v3">
      <windowsSettings>
          <dpiAware xmlns="http://schemas.microsoft.com/SMI/2005/WindowsSettings">true/PM</dpiAware>
          <dpiAwareness xmlns="http://schemas.microsoft.com/SMI/2016/WindowsSettings">PerMonitorV2, PerMonitor</dpiAwareness>
      </windowsSettings>
    </application>
    ```

10. 保存して閉じます、`app.manifest`ファイル。

12. **ソリューション エクスプ ローラー**を右クリックし、 **ContosoExpenses.Core**プロジェクト**プロパティ**します。

13. **リソース**のセクション、**アプリケーション** タブで、確認、**マニフェスト**ドロップダウンに設定されている**app.manifest**します。

    ![.NET core アプリのマニフェスト](images/wpf-modernize-tutorial/NetCoreAppManifest.png)

16. プロジェクト プロパティに変更を保存します。

## <a name="add-an-inkcanvas-control-to-the-app"></a>InkCanvas コントロールをアプリに追加します。

追加する準備ができました UWP XAML Islandsを使用するプロジェクトを構成したところ、 [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)をアプリに UWP コントロールをラップします。

1. **ソリューション エクスプ ローラー**、展開、**ビュー**のフォルダー、 **ContosoExpenses.Core**プロジェクトし、ダブルクリックして、 **ExpenseDetail.xaml**ファイル。

2. **ウィンドウ**XAML ファイルの先頭付近にある要素は、次の属性を追加します。 これは参照の XAML 名前空間、 [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) UWP コントロールをラップします。

    ```xml
    xmlns:toolkit="clr-namespace:Microsoft.Toolkit.Wpf.UI.Controls;assembly=Microsoft.Toolkit.Wpf.UI.Controls"
    ```

    この属性を追加した後、**ウィンドウ**要素が次のようになります。

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

4. **ExpenseDetail.xaml**ファイルの終了を探し、`</Grid>`タグの直前にある、`<!-- Chart -->`コメント。 終了直前に次の XAML を追加`</Grid>`タグ。 この XAML を追加、 **InkCanvas**コントロール (付いて、 **toolkit**キーワード名前空間として定義した) およびシンプルな**TextBlock**コントロールのヘッダーとして機能します。

    ```xml
    <TextBlock Text="Signature:" FontSize="16" FontWeight="Bold" Grid.Row="5" />

    <toolkit:InkCanvas x:Name="Signature" Grid.Row="6" />
    ```

5. 保存、 **ExpenseDetail.xaml**ファイル。

6. F5 キーを押して、デバッガーでアプリを実行します。

7. 一覧から、従業員を選択し、使用可能な経費のいずれかを選択します。 費用の詳細ページ用の領域が含まれていること、 **InkCanvas**コントロール。

    ![インク キャンバス ペンのみ](images/wpf-modernize-tutorial/InkCanvasPenOnly.png)

    など、サーフェイスのデジタル ペンをサポートするデバイスがあり、物理マシンでこのラボを実行している場合し、それを使用しようとしてください。 画面上に表示されるデジタル インクが表示されます。 ただし、ペン対応デバイスがない、マウスを使用してサインインしようとする場合は、何も起こりません。 ため、これが起こっているか、 **InkCanvas**コントロールは、既定でデジタル ペンに対してのみ有効です。 ただし、この動作を変更することができます。

8. アプリケーションを終了し、ダブルクリック、 **ExpenseDetail.xaml.cs**ファイル、**ビュー**のフォルダー、 **ContosoExpenses.Core**プロジェクト。

9. クラスの上部にある次の名前空間宣言を追加します。

    ```csharp
    using Microsoft.Toolkit.Win32.UI.Controls.Interop.WinRT;
    ```

10. 検索、`ExpenseDetail()`コンス トラクター。

11. 次のコードの直後の行を追加、`InitializeComponent()`メソッドのコード ファイルを保存します。

    ```csharp
    Signature.InkPresenter.InputDeviceTypes = CoreInputDeviceTypes.Mouse | CoreInputDeviceTypes.Pen;
    ```

    使用することができます、 **InkPresenter**既定のエクスペリエンスを手描き入力機能をカスタマイズするオブジェクト。 このコードを使用して、 **InputDeviceTypes**ペン入力とマウスを有効にするプロパティ。

12. F5 キーを押してもう一度をリビルドし、デバッガーでアプリを実行します。 一覧から、従業員を選択し、使用可能な経費のいずれかを選択します。

13. マウスを使用して署名領域に描画するには、今すぐお試しください。 今回は、画面上に表示されるインクが表示されます。

    ![署名](images/wpf-modernize-tutorial/Signature.png)

## <a name="next-steps"></a>次のステップ

この時点で、チュートリアルでは正常に追加した、UWP **InkCanvas** Contoso 経費アプリケーションを制御します。 準備が整いました[パート 3。XAML Islandsを使用して、UWP の予定表ビュー コントロールを追加](modernize-wpf-tutorial-3.md)します。
