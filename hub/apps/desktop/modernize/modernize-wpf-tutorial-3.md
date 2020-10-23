---
description: このチュートリアルでは、タッチ対応デバイスで経費報告書の日付を簡単に選択できるようにする方法について説明します。
title: XAML Islands を使用した UWP CalendarView コントロールの追加
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: windows 10、uwp、windows フォーム、wpf、xaml islands
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 53a87f5803a6e4707b2dc6b86b32b7db003e59ef
ms.sourcegitcommit: 0c4bbaf1c119a84002748cdcf02e1449835559c3
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/16/2020
ms.locfileid: "92133045"
---
# <a name="part-3-add-a-uwp-calendarview-control-using-xaml-islands"></a>パート 3: XAML Islands を使用した UWP CalendarView コントロールの追加

これは、Contoso Expenses という名前のサンプル WPF デスクトップ アプリを現代化する方法を示すチュートリアルの 3 番目の部分です。 チュートリアルの概要、前提条件、サンプル アプリをダウンロードするための手順については、「[チュートリアル: WPF アプリの現代化](modernize-wpf-tutorial.md)」を参照してください。 この記事では、読者が[パート 2](modernize-wpf-tutorial-2.md) を既に完了していることを前提にしています。

このチュートリアルの架空のシナリオでは、Contoso 開発チームは、タッチ対応デバイスで経費報告書の日付を簡単に選択できるようにしたいと考えています。 チュートリアルのこの部分では、アプリに UWP [CalendarView](/windows/uwp/design/controls-and-patterns/calendar-view) コントロールを追加します。 これは、タスク バーにある Windows 10 の日付と時刻の機能で使用されるものと同じコントロールです。

![CalendarViewControl の画像](images/wpf-modernize-tutorial/CalendarViewControl.png)

[パート 2](modernize-wpf-tutorial-2.md) で追加した **InkCanvas** コントロールとは異なり、Windows コミュニティ ツールキットには、WPF アプリで使用できる UWP **CalendarView** のラップされたバージョンが用意されていません。 別の方法として、汎用の [WindowsXamlHost](/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) コントロールで **InkCanvas** をホストします。 このコントロールを使用すると、Windows SDK または WinUI ライブラリによって提供される任意のファーストパーティ UWP コントロールや、サードパーティによって作成された任意のカスタム UWP コントロールをホストできます。 **WindowsXamlHost** コントロールは、`Microsoft.Toolkit.Wpf.UI.XamlHost` NuGet パッケージによって提供されます。 このパッケージは、[パート 2](modernize-wpf-tutorial-2.md) でインストールした `Microsoft.Toolkit.Wpf.UI.Controls` NuGet パッケージに含まれています。

> [!NOTE]
> このチュートリアルでは、[WindowsXamlHost](/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) を使用して、Windows SDK によって提供されるファーストパーティ **CalendarView** コントロールをホストする方法のみを示します。 カスタム コントロールをホストする方法を示すチュートリアルについては、[XAML Islands を使用した WPF アプリでのカスタム UWP コントロールのホスト](host-custom-control-with-xaml-islands.md)に関するページを参照してください。

**WindowsXamlHost** コントロールを使用するには、WPF アプリのコードから直接 WinRT API を呼び出す必要があります。 `Microsoft.Windows.SDK.Contracts` NuGet パッケージには、アプリから WinRT API を呼び出せるようにするために必要な参照が含まれています。 このパッケージはまた、[パート 2](modernize-wpf-tutorial-2.md) でインストールした `Microsoft.Toolkit.Wpf.UI.Controls` NuGet パッケージにも含まれています。

## <a name="add-the-windowsxamlhost-control"></a>WindowsXamlHost コントロールを追加する

1. **ソリューション エクスプローラー**で、**ContosoExpenses.Core** プロジェクトの **Views** フォルダーを展開し、**AddNewExpense.xaml** ファイルをダブルクリックします。 これは、一覧に新しい経費を追加するために使用されるフォームです。 それが現在のバージョンのアプリでどのように表示されるかを次に示します。

    ![新しい経費を追加する](images/wpf-modernize-tutorial/AddNewExpense.png)

    WPF に含まれている日付の選択コントロールは、マウスとキーボードが接続された従来のコンピューターを対象としています。 コントロールのサイズが小さく、カレンダー内の各日の間の領域が制限されているため、タッチ スクリーンでの日付の選択は実際には不可能です。

2. **AddNewExpense.xaml** ファイルの先頭で、**Window** 要素に次の属性を追加します。

    ```xml
    xmlns:xamlhost="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost"
    ```

    この属性を追加した後、**Window** 要素は次のようになります。

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

3. **Window** 要素の **Height** 属性を 450 から 800 に変更します。 これは、UWP **CalendarView** コントロールには WPF の日付の選択より広い領域が要るために必要です。

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

4. ファイルの最後の近くにある `DatePicker` 要素を見つけ、この要素を次の XAML に置き換えます。

    ```xml
    <xamlhost:WindowsXamlHost InitialTypeName="Windows.UI.Xaml.Controls.CalendarView" Grid.Column="1" Grid.Row="6" Margin="5, 0, 0, 0" x:Name="CalendarUwp"  />
    ```

    この XAML が **WindowsXamlHost** コントロールを追加します。 **InitialTypeName** プロパティは、ホストする UWP コントロールのフルネーム (この場合は、**Windows.UI.Xaml.Controls.CalendarView**) を示します。

5. デバッガーで F5 キーを押して、アプリをビルドして実行します。 一覧から従業員を選択した後、 **[Add new expense] (新しい経費の追加)** ボタンを押します。 次のページに新しい UWP **CalendarView** コントロールがホストされていることを確認します。

    ![CalendarView ラッパー](images/wpf-modernize-tutorial/CalendarViewWrapper.png)

6. アプリを閉じます。

## <a name="interact-with-the-windowsxamlhost-control"></a>WindowsXamlHost コントロールを操作する

次に、選択された日付を処理するようにアプリを更新し、それを画面に表示してから、データベースに保存する **Expense** オブジェクトを設定します。

UWP [CalendarView](/uwp/api/Windows.UI.Xaml.Controls.CalendarView) には、このシナリオに関連する次の 2 つのメンバーが含まれています。

- **SelectedDates** プロパティには、ユーザーによって選択された日付が含まれます。
- **SelectedDatesChanged** イベントは、ユーザーが日付を選択したときに生成されます。

ただし、**WindowsXamlHost** コントロールは、*任意の*種類の UWP コントロールのための汎用のホスト コントロールです。 そのため、**SelectedDates** という名前のプロパティまたは **SelectedDatesChanged** という名前のイベントは、**CalendarView** コントロールに固有であるため公開されません。 これらのメンバーにアクセスするには、**WindowsXamlHost** 型を **CalendarView** 型にキャストするコードを記述する必要があります。 これを行うための最適な場所は、ホストされているコントロールがレンダリングされたときに生成される **WindowsXamlHost** コントロールの **ChildChanged** イベントへの応答です。

1. **AddNewExpense.xaml** ファイルで、前に追加した **WindowsXamlHost** コントロールの **ChildChanged** イベントのイベント ハンドラーを追加します。 完了すると、**WindowsXamlHost** 要素は次のようになります。

    ```xml
    <xamlhost:WindowsXamlHost InitialTypeName="Windows.UI.Xaml.Controls.CalendarView" Grid.Column="1" Grid.Row="6" Margin="5, 0, 0, 0" x:Name="CalendarUwp"  ChildChanged="CalendarUwp_ChildChanged" />
    ```

2. 同じファイルで、メイン **Grid** の **Grid.RowDefinitions** 要素を見つけます。 子要素の一覧の最後に **Height** が **Auto** に等しい **RowDefinition** 要素をもう 1 つ追加します。 完了すると、**Grid.RowDefinitions** 要素は次のようになります (**RowDefinition** 要素が 9 つになっています)。

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

4. ファイルの最後の近くにある **WindowsXamlHost** 要素の後でかつ **Button** 要素の前に次の XAML を追加します。

    ```xml
    <TextBlock Text="Selected date:" FontSize="16" FontWeight="Bold" Grid.Row="7" Grid.Column="0" />
    <TextBlock Text="{Binding Path=Date}" FontSize="16" Grid.Row="7" Grid.Column="1" />
    ```

5. ファイルの最後の近くにある **Button** 要素を見つけ、**Grid.Row** プロパティを **7** から **8** に変更します。 新しい行を追加したため、これによりグリッド内でボタンが 1 行下にシフトされます。

    ```xml
    <Button Content="Save" Grid.Row="8" Grid.Column="0" Command="{Binding Path=SaveExpenseCommand}" Margin="5, 12, 0, 0" HorizontalAlignment="Left" Width="180" />
    ```

6. **AddNewExpense.xaml.cs** コード ファイルを開きます。

7. ファイルの先頭に次のステートメントを追加します。

    ```csharp
    using Microsoft.Toolkit.Wpf.UI.XamlHost;
    ```

8. `AddNewExpense` クラスに次のイベント ハンドラーを追加します。 このコードは、XAML ファイル内で前に宣言した **WindowsXamlHost** コントロールの **ChildChanged** イベントを実装します。

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

    このコードでは、**WindowsXamlHost** コントロールの **Child** プロパティを使用して UWP **CalendarView** コントロールにアクセスします。 このコードは次に、ユーザーがカレンダーから日付を選択したときにトリガーされる **SelectedDatesChanged** イベントをサブスクライブします。 このイベント ハンドラーは、選択された日付を ViewModel に渡します。そこで新しい **Expense** オブジェクトが作成され、データベースに保存されます。 これを行うために、このコードでは、MVVM Light NuGet パッケージによって提供されるメッセージング インフラストラクチャを使用します。 このコードが **SelectedDateMessage** という名前のメッセージを ViewModel に送信すると、ViewModel がそれを受信し、**Date** プロパティを選択された値に設定します。 このシナリオでは、**CalendarView** コントロールは単一選択モードで構成されているため、コレクションには 1 つの要素しか含まれません。

    この時点では、**SelectedDateMessage** クラスの実装がないため、プロジェクトはまだコンパイルされません。 このクラスは、次の手順で実装されます。

9. **ソリューション エクスプローラー**で、**Messages** フォルダーを右クリックし、 **[追加] -> [クラス]** の順に選択します。 新しいクラスに **SelectedDateMessage** という名前を付け、 **[追加]** をクリックします。

10. **SelectedDateMessage.cs** コード ファイルの内容を次のコードに置き換えます。 このコードは、`GalaSoft.MvvmLight.Messaging` 名前空間の using ステートメントを (MVVM Light NuGet パッケージから) 追加し、**MessageBase** クラスから継承し、パブリック コンストラクター経由で初期化される **DateTime** プロパティを追加します。 完了すると、ファイルは次のようになります。

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

11. 次に、このメッセージを受信するように ViewModel を更新し、ViewModel の **Date** プロパティを設定します。 **ソリューション エクスプローラー**で、**ViewModels** フォルダーを展開し、**AddNewExpenseViewModel.cs** ファイルを開きます。

12. `AddNewExpenseViewModel` クラスのパブリック コンストラクターを見つけ、そのコンストラクターの最後に次のコードを追加します。 このコードは、**SelectedDateMessage** を受信するように登録し、選択された日付を **SelectedDate** プロパティ経由でそこから抽出します。それを使用して、ViewModel によって公開される **Date** プロパティを設定します。 このプロパティは、前に追加した **TextBlock** コントロールにバインドされているため、ユーザーによって選択された日付を確認できます。

    ```csharp
    Messenger.Default.Register<SelectedDateMessage>(this, message =>
    {
        Date = message.SelectedDate;
    });
    ```

    完了すると、`AddNewExpenseViewModel` コンストラクターは次のようになります。 

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
    > 同じコード ファイル内の `SaveExpenseCommand` メソッドは、経費をデータベースに保存する作業を行います。 このメソッドでは、**Date** プロパティを使用して、新しい **Expense** オブジェクトを作成します。 このプロパティは、今作成した `SelectedDateMessage` メッセージ経由で **CalendarView** コントロールによって設定されるようになります。

13. デバッガーで F5 キーを押して、アプリをビルドして実行します。 選択可能な従業員のいずれかを選択し、 **[Add new expense] (新しい経費の追加)** ボタンをクリックします。 フォームのすべてのフィールドに入力し、新しい **CalendarView** コントロールから日付を選択します。 選択した日付がそのコントロールの下に文字列として表示されることを確認します。

14. **[保存]** ボタンを押します。 フォームが閉じられ、新しい経費が経費の一覧の最後に追加されます。 経費の日付のある最初の列が **CalendarView** コントロールで選択した日付になっています。

## <a name="next-steps"></a>次の手順

チュートリアルのこの時点で、WPF の日付と時刻コントロールが、マウスとキーボードの入力に加えてタッチとデジタル ペンをサポートする UWP **CalendarView** コントロールに正常に置き換えられました。 Windows コミュニティ ツールキットには、WPF アプリで直接使用できる UWP **CalendarView** コントロールのラップされたバージョンが用意されていませんが、汎用の [WindowsXamlHost](/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) コントロールを使用してそのコントロールをホストすることができました。

これで、「[パート 4: Windows 10 ユーザー アクティビティと通知の追加](modernize-wpf-tutorial-4.md)」に進む準備ができました。