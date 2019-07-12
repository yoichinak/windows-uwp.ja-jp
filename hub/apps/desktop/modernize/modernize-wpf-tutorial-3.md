---
description: このチュートリアルでは、UWP XAML ユーザー インターフェイスを追加、MSIX パッケージを作成およびその他の最新コンポーネントを WPF アプリに組み込む方法を示します。
title: XAML Islands を使用して、UWP の予定表ビュー コントロールを追加します。
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: windows 10、uwp、windows フォーム、wpf、xaml 諸島
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: fce9135267461f61515c7dc04bbaf43b1e4c04ed
ms.sourcegitcommit: 1eec0e4fd8a5ba82803fdce6e23fcd01b9488523
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/27/2019
ms.locfileid: "67420111"
---
# <a name="part-3-add-a-uwp-calendarview-control-using-xaml-islands"></a>パート 3:XAML Islands を使用して、UWP の予定表ビュー コントロールを追加します。

これは、Contoso の経費をという名前のサンプル WPF デスクトップ アプリの近代化する方法を説明するチュートリアルの第 3 部です。 チュートリアル、前提条件、およびサンプル アプリをダウンロードする手順の概要については、次を参照してください。[チュートリアル。WPF アプリの近代化](modernize-wpf-tutorial.md)します。 この記事では、既に完了している前提としています。[パート 2](modernize-wpf-tutorial-2.md)します。

このチュートリアルのシナリオでは、架空の contoso 社の開発チームは、経費報告書タッチ対応のデバイスでの日付を選択しやすくしようとします。 チュートリアルのこの部分では、UWP を追加します[予定表ビュー](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/calendar-view)アプリを制御します。 これは、タスク バーの Windows 10 の日付と時刻の機能で使用されている同じコントロールです。

![CalendarViewControl イメージ](images/wpf-modernize-tutorial/CalendarViewControl.png)

異なり、 **InkCanvas**制御で追加した[パート 2](modernize-wpf-tutorial-2.md)、Windows の Community Toolkit は、UWP のラップされたバージョンを行わない**予定表ビュー** WPF アプリで使用できます. ホストする別の方法として、 **InkCanvas**ジェネリック[WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)コントロール。 このコントロールを使用して、プラットフォームまたはサード パーティ ベンダーから提供されるすべての UWP コントロールをホストすることができます。 **WindowsXamlHost**によってコントロールが提供される、 `Microsoft.Toolkit.Wpf.UI.XamlHost` NuGet パッケージのパッケージ。 このパッケージが含まれています、`Microsoft.Toolkit.Wpf.UI.Controls`でインストールした NuGet パッケージ[パート 2](modernize-wpf-tutorial-2.md)します。

使用するには、 **WindowsXamlHost**コントロールする WPF アプリのコードから直接 WinRT Api を呼び出す必要があります。 `Microsoft.Windows.SDK.Contracts` NuGet パッケージにはアプリから WinRT Api を呼び出すことを有効にするために必要な参照が含まれています。 このパッケージに記載されても、`Microsoft.Toolkit.Wpf.UI.Controls`でインストールした NuGet パッケージ[パート 2](modernize-wpf-tutorial-2.md)します。

## <a name="add-the-windowsxamlhost-control"></a>WindowsXamlHost コントロールを追加します。

1. **ソリューション エクスプ ローラー**、展開、**ビュー**フォルダーで、 **ContosoExpenses.Core**プロジェクトし、ダブルクリックして、 **AddNewExpense.xaml**ファイル。 これは、一覧に新しい経費を追加するために使用します。 現在のバージョンのアプリでの表示方法を示します。

    ![新しい経費を追加します。](images/wpf-modernize-tutorial/AddNewExpense.png)

    WPF に含まれる日付の選択コントロールはマウス/キーボードと従来のコンピューター用のもの。 コントロールと予定表では、1 日の間の限られたスペースのサイズが小さいため、実際に不可能ではありません、タッチ スクリーンで日付を選択します。

2. 上部にある、 **AddNewExpense.xaml**ファイルで、次の属性を追加、**ウィンドウ**要素。

    ```xml
    xmlns:xamlhost="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost"
    ```

    この属性を追加した後、**ウィンドウ**要素が次のようになります。

    ```xml
    <Window x:Class="ContosoExpenses.Views.AddNewExpense"
            xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
            xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
            xmlns:xamlhost="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost"
            DataContext="{Binding Source={StaticResource ViewModelLocator},Path=AddNewExpenseViewModel}"
            xmlns:local="clr-namespace:ContosoExpenses"
            mc:Ignorable="d"
            Title="Add new expense" Height="450" Width="800"
            Background="{StaticResource AddNewExpenseBackground}">
    ```

3. 変更、**高さ**の属性、**ウィンドウ**800 に 450 からの要素。 ため、これは必要 UWP**予定表ビュー**コントロールが WPF 日付選択カレンダーよりも多い領域を取得します。

    ```xml
    <Window x:Class="ContosoExpenses.Views.AddNewExpense"
            xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
            xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
            xmlns:xamlhost="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost"
            DataContext="{Binding Source={StaticResource ViewModelLocator},Path=AddNewExpenseViewModel}"
            xmlns:local="clr-namespace:ContosoExpenses"
            mc:Ignorable="d"
            Title="Add new expense" Height="800" Width="800"
            Background="{StaticResource AddNewExpenseBackground}">
    ```

4. 検索、`DatePicker`要素は、ファイルの下部付近でと、この要素を次の XAML に置き換えます。

    ```xml
    <xamlhost:WindowsXamlHost InitialTypeName="Windows.UI.Xaml.Controls.CalendarView" Grid.Column="1" Grid.Row="6" Margin="5, 0, 0, 0" x:Name="CalendarUwp"  />
    ```

    この XAML を追加、 **WindowsXamlHost**コントロール。 **InitialTypeName**プロパティをホストする UWP コントロールの完全な名前を示します (この場合、 **Windows.UI.Xaml.Controls.CalendarView**)。

5. F5 キーを押してビルドして、デバッガーでアプリを実行します。 一覧から、従業員を選択し、キーを押します、**新しい経費の追加**ボタンをクリックします。 次のページが新しい UWP をホストしていることを確認します。**予定表ビュー**コントロール。

    ![予定表ビュー ラッパー](images/wpf-modernize-tutorial/CalendarViewWrapper.png)

6. アプリを閉じます。

## <a name="interact-with-the-windowsxamlhost-control"></a>WindowsXamlHost コントロールを操作します。

次を選択した日付を処理して、画面上に表示設定にアプリを更新、**経費**データベースに保存するオブジェクト。

UWP[予定表ビュー](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarView)このシナリオに関連する 2 つのメンバーが含まれています。

- **SelectedDates**プロパティには、ユーザーが選択した日付が含まれています。
- **SelectedDatesChanged**日付を選択すると、イベントが発生します。

ただし、 **WindowsXamlHost**コントロールは、汎用ホスト コントロールの*任意*UWP コントロールの種類。 という名前のプロパティを公開しないよう、 **SelectedDates**またはイベントが呼び出される**SelectedDatesChanged**の固有であるため、**予定表ビュー**コントロール。 これらのメンバーにアクセスするには、キャストするコードを記述する必要があります、 **WindowsXamlHost**を**予定表ビュー**型。 これを行う最適な場所への応答は、 **ChildChanged**のイベント、 **WindowsXamlHost**コントロールで、ホストされるコントロールが表示されたときに発生します。

1. **AddNewExpense.xaml**ファイル、イベント ハンドラーを追加の**ChildChanged**のイベント、 **WindowsXamlHost**前に追加します。 済んだら、 **WindowsXamlHost**要素は、次のようになります。

    ```xml
    <xamlhost:WindowsXamlHost InitialTypeName="Windows.UI.Xaml.Controls.CalendarView" Grid.Column="1" Grid.Row="6" Margin="5, 0, 0, 0" x:Name="CalendarUwp"  ChildChanged="CalendarUwp_ChildChanged" />
    ```

2. 同じファイルで探します、 **Grid.RowDefinitions**主要な要素**グリッド**します。 1 つ追加**RowDefinition**を持つ要素、**高さ**等しく**自動**子要素の一覧の最後にします。 済んだら、 **Grid.RowDefinitions**要素は、次のようになります (9 はず**RowDefinition**要素)。

    ```xml
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
    </Grid.RowDefinitions>
    ```

4. 後に次の XAML を追加、 **WindowsXamlHost**要素とする前に、**ボタン**ファイルの末尾付近の要素。

    ```xml
    <TextBlock Text="Selected date:" FontSize="16" FontWeight="Bold" Grid.Row="7" Grid.Column="0" />
    <TextBlock Text="{Binding Path=Date}" FontSize="16" Grid.Row="7" Grid.Column="1" />
    ```

5. 検索、**ボタン**ファイルと変更の最後の要素、 **Grid.Row**プロパティから**7**に**8**します。 新しい行を追加するために、グリッド内の 1 行下のボタンを移動します。

    ```xml
    <Button Content="Save" Grid.Row="8" Grid.Column="0" Command="{Binding Path=SaveExpenseCommand}" Margin="5, 12, 0, 0" HorizontalAlignment="Left" Width="180" />
    ```

6. 開く、 **AddNewExpense.xaml.cs**コード ファイル。

7. ファイルの先頭に次のステートメントを追加します。

    ```csharp
    using Microsoft.Toolkit.Wpf.UI.XamlHost;
    ```

8. 次のイベント ハンドラーを追加、`AddNewExpense`クラス。 このコードは、実装、 **ChildChanged**のイベント、 **WindowsXamlHost** XAML ファイルで宣言したコントロール。

    ```csharp
    private void CalendarUwp_ChildChanged(object sender, System.EventArgs e)
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
                    Messenger.Default.Send<SelectedDateMessage>(new SelectedDateMessage(args.AddedDates[0].DateTime));
                }
            };
        }
    }
    ```

    このコードを使用して、**子**のプロパティ、 **WindowsXamlHost** UWP へのアクセスにコントロール**予定表ビュー**コントロール。 コードをサブスクライブし、 **SelectedDatesChanged** 、カレンダーから日付を選択するとトリガーされるイベント。 このイベント ハンドラーは、ViewModel に選択した日付を渡す、新しい**経費**オブジェクトを作成し、データベースに保存します。 これを行うには、コードは、MVVM Light の NuGet パッケージで提供される、メッセージング インフラストラクチャを使用します。 コードと呼ばれるメッセージを送信します**SelectedDateMessage** 、ViewModel に受信して設定するが、**日付**選択した値を持つプロパティです。 このシナリオでは、**予定表ビュー**単一選択モードでコントロールが構成されているため、コレクションが 1 つだけの要素が格納されます。

    この時点では、プロジェクトがコンパイルされないの実装がないため、 **SelectedDateMessage**クラス。 次の手順では、このクラスを実装します。

9. **ソリューション エクスプ ローラー**を右クリックし、**メッセージ**フォルダー選択**追加]、[クラス**します。 新しいクラスの名前**SelectedDateMessage**クリック**追加**します。

10. 内容を置き換える、 **SelectedDateMessage.cs**を次のコードのコード ファイル。 使用して、このコードを追加のステートメント、`GalaSoft.MvvmLight.Messaging`名前空間 (MVVM Light の NuGet パッケージにある) からの継承、 **MessageBase**クラス、および追加**DateTime**から初期化されるプロパティ、パブリック コンス トラクターです。 完了したら、ファイルは次のようになります。

    ```csharp
    using GalaSoft.MvvmLight.Messaging;
    using System;
    
    namespace ContosoExpenses.Messages
    {
        public class SelectedDateMessage: MessageBase
        {
            public DateTime SelectedDate { get; set; }
    
            public SelectedDateMessage(DateTime selectedDate)
            {
                this.SelectedDate = selectedDate;
            }
        }
    }
    ```

11. 次に、このメッセージを受信し、設定、ビューモデルを更新、**日付**ビューモデルのプロパティ。 **ソリューション エクスプ ローラー**、展開、 **ViewModels**フォルダーとオープン、 **AddNewExpenseViewModel.cs**ファイル。

12. パブリック コンス トラクターを検索、`AddNewExpenseViewModel`クラスし、コンス トラクターの末尾に次のコードを追加します。 このコードは、受け取りを登録、 **SelectedDateMessage**、してから、選択した日付を抽出、 **SelectedDate**プロパティ、および私たちはそれを使用、**日付**プロパティViewModel で公開されます。 このプロパティにバインドされているため、 **TextBlock**コントロールを追加した以前では、ユーザーが選択されている日付を確認できます。

    ```csharp
    Messenger.Default.Register<SelectedDateMessage>(this, message =>
    {
        Date = message.SelectedDate;
    });
    ```

    済んだら、`AddNewExpenseViewModel`コンス トラクターは、次のようになります。 

    ```csharp
    public AddNewExpenseViewModel(IDatabaseService databaseService, IStorageService storageService)
    {
        this.databaseService = databaseService;
        this.storageService = storageService;

        Date = DateTime.Today;

        Messenger.Default.Register<SelectedDateMessage>(this, message =>
        {
            Date = message.SelectedDate;
        });
    }
    ```

    > [!NOTE]
    > `SaveExpenseCommand`同じコード ファイル内のメソッドは、経費をデータベースに保存します。 このメソッドを使用して、**日付**プロパティを新しい**経費**オブジェクト。 このプロパティは登録されているようになりました、**予定表ビュー**を介して制御、`SelectedDateMessage`したメッセージを作成します。

13. F5 キーを押してビルドして、デバッガーでアプリを実行します。 使用可能な従業員のいずれかを選択し、クリックして、**新しい経費の追加**ボタンをクリックします。 形式ですべてのフィールドに入力し、新しいから日付を選択**予定表ビュー**コントロール。 コントロールの下の文字列として選択した日付が表示されることを確認します。

14. キーを押して、**保存**ボタンをクリックします。 フォームが閉じられ、新しい費用は、経費のリストの末尾に追加されます。 日付の最初の列で選択した日付をする必要があります、**予定表ビュー**コントロール。

## <a name="next-steps"></a>次のステップ

この時点で、チュートリアルでは置換されました、WPF 日時コントロール uwp**予定表ビュー**コントロールで、タッチとマウスおよびキーボード入力だけでなく、デジタル ペンをサポートしています。 Windows の Community Toolkit は、UWP のラップされたバージョンを提供しませんが**予定表ビュー** WPF アプリで直接使用できるコントロール、ジェネリックを使用してコントロールをホストするできた[WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)コントロール。

準備が整いました[パート 4。Windows 10 ユーザーのアクティビティと通知の追加](modernize-wpf-tutorial-4.md)します。
