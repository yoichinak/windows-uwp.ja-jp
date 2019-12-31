---
description: このチュートリアルでは、UWP XAML ユーザーインターフェイスを追加する方法、MSIX パッケージを作成する方法、およびその他の最新のコンポーネントを WPF アプリに組み込む方法について説明します。
title: XAML Islands を使用した UWP CalendarView コントロールの追加
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: windows 10、uwp、windows フォーム、wpf、xaml islands
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 830c1cdf2e24e716d51642bc65b5b6783d0d784a
ms.sourcegitcommit: 6bb794c6e309ba543de6583d96627fbf1c177bef
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/20/2019
ms.locfileid: "69643373"
---
# <a name="part-3-add-a-uwp-calendarview-control-using-xaml-islands"></a>第 3 部:XAML Islands を使用した UWP CalendarView コントロールの追加

これは、Contoso の支出という名前の WPF デスクトップアプリのサンプルを最新化する方法を示すチュートリアルの3番目の部分です。 サンプルアプリをダウンロードするためのチュートリアル、前提条件、および手順の概要につい[ては、「チュートリアル:WPF アプリ](modernize-wpf-tutorial.md)を最新化します。 この記事では、[パート 2](modernize-wpf-tutorial-2.md)を既に完了していることを前提としています。

このチュートリアルの架空のシナリオでは、Contoso 開発チームは、タッチ対応デバイスで経費報告書の日付を簡単に選択できるようにしたいと考えています。 チュートリアルのこの部分では、UWP [CalendarView](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/calendar-view)コントロールをアプリに追加します。 これは、タスクバーの Windows 10 の日付と時刻の機能で使用されるコントロールと同じです。

![CalendarViewControl イメージ](images/wpf-modernize-tutorial/CalendarViewControl.png)

[パート 2](modernize-wpf-tutorial-2.md)で追加した**system.windows.controls.inkcanvas>** コントロールとは異なり、Windows Community TOOLKIT には、WPF アプリで使用できる UWP **CalendarView**のラップされたバージョンが用意されていません。 別の方法として、汎用[Windowsxamlhost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)コントロールで**system.windows.controls.inkcanvas>** をホストします。 このコントロールを使用して、Windows SDK または WinUI ライブラリ、またはサードパーティによって作成された任意のカスタム UWP コントロールによって提供されるファーストパーティ UWP コントロールをホストできます。 **Windowsxamlhost**コントロールは、 `Microsoft.Toolkit.Wpf.UI.XamlHost`パッケージ NuGet パッケージによって提供されます。 このパッケージは、 `Microsoft.Toolkit.Wpf.UI.Controls` [パート 2](modernize-wpf-tutorial-2.md)でインストールした NuGet パッケージに含まれています。

> [!NOTE]
> このチュートリアルでは、Windows SDK によって提供されるファーストパーティの**CalendarView**コントロールをホストするために[windowsxamlhost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)を使用する方法についてのみ説明します。 カスタムコントロールをホストする方法を示すチュートリアルについては、「[XAML Islands を使用した WPF アプリでのカスタム UWP コントロールのホスト](host-custom-control-with-xaml-islands.md)」を参照してください。

**Windowsxamlhost**コントロールを使用するには、WPF アプリのコードから WinRT api を直接呼び出す必要があります。 `Microsoft.Windows.SDK.Contracts` NuGet パッケージには、アプリから WinRT api を呼び出すことができるようにするために必要な参照が含まれています。 このパッケージは、 `Microsoft.Toolkit.Wpf.UI.Controls` [パート 2](modernize-wpf-tutorial-2.md)でインストールした NuGet パッケージにも含まれています。

## <a name="add-the-windowsxamlhost-control"></a>WindowsXamlHost コントロールを追加する

1. **ソリューションエクスプローラー**で、 **ContosoExpenses**プロジェクトの**Views**フォルダーを展開し、 **addnewexpense .xaml**ファイルをダブルクリックします。 これは、一覧に新しい費用を追加するために使用されるフォームです。 現在のバージョンのアプリでの表示方法を次に示します。

    ![新しい支出の追加](images/wpf-modernize-tutorial/AddNewExpense.png)

    WPF に含まれる日付選択コントロールは、従来のコンピューターでマウスとキーボードを使用するためのものです。 コントロールのサイズが小さく、カレンダーの各日の間にスペースが制限されているため、タッチスクリーンで日付を選択することは実際には現実的ではありません。

2. **Addnewexpense .xaml**ファイルの先頭で、次の属性を**Window**要素に追加します。

    ```xml
    xmlns:xamlhost="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost"
    ```

    この属性を追加すると、**ウィンドウ**要素は次のようになります。

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

3. **Window**要素の**Height**属性を450から800に変更します。 UWP **CalendarView**コントロールは、WPF の日付の選択よりも多くの領域を必要とするため、この操作が必要になります。

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

4. ファイルの末尾付近にある要素を見つけ、この要素を次のXAMLに置き換えます。`DatePicker`

    ```xml
    <xamlhost:WindowsXamlHost InitialTypeName="Windows.UI.Xaml.Controls.CalendarView" Grid.Column="1" Grid.Row="6" Margin="5, 0, 0, 0" x:Name="CalendarUwp"  />
    ```

    この XAML は**Windowsxamlhost**コントロールを追加します。 **Initialtypename**プロパティは、ホストする UWP コントロールの完全な名前 (この例では、 **CalendarView**) を示しています。

5. F5 キーを押して、デバッガーでアプリをビルドして実行します。 一覧から従業員を選択し、 **[新しい経費の追加]** ボタンを押します。 次のページが新しい UWP **CalendarView**コントロールをホストしていることを確認します。

    ![CalendarView ラッパー](images/wpf-modernize-tutorial/CalendarViewWrapper.png)

6. アプリを閉じます。

## <a name="interact-with-the-windowsxamlhost-control"></a>WindowsXamlHost コントロールとの対話

次に、選択した日付を処理して画面に表示し、**経費**オブジェクトを設定してデータベースに保存するようにアプリを更新します。

UWP [CalendarView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarView)には、このシナリオに関連する2つのメンバーが含まれています。

- **SelectedDates**プロパティには、ユーザーが選択した日付が格納されます。
- **Selectedchanged changed**イベントは、ユーザーが日付を選択したときに発生します。

ただし、 **Windowsxamlhost**コントロールは、*任意*の種類の UWP コントロールの汎用ホストコントロールです。 そのため、 **SelectedDates**という名前のプロパティや selected という名前のイベントは、 **CalendarView**コントロールに固有であるため、公開**さ**れません。 これらのメンバーにアクセスするには、 **Windowsxamlhost**を**CalendarView**型にキャストするコードを記述する必要があります。 これを行うには、 **Windowsxamlhost**コントロールの**childchanged**イベントに応答します。これは、ホストされているコントロールがレンダリングされたときに発生します。

1. **Addnewexpense .xaml**ファイルで、前に追加した**windowsxamlhost**コントロールの**childchanged**イベントのイベントハンドラーを追加します。 完了すると、 **Windowsxamlhost**要素は次のようになります。

    ```xml
    <xamlhost:WindowsXamlHost InitialTypeName="Windows.UI.Xaml.Controls.CalendarView" Grid.Column="1" Grid.Row="6" Margin="5, 0, 0, 0" x:Name="CalendarUwp"  ChildChanged="CalendarUwp_ChildChanged" />
    ```

2. 同じファイルで、メイン**グリッド**の **[rowdefinitions]** 要素を見つけます。 子要素のリストの末尾に、**高さ**が**Auto**に等しい**rowdefinition**要素をもう1つ追加します。 完了すると、 **RowDefinitions**要素は次のようになります (9 つの**rowdefinitions**要素があるはずです)。

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

4. **Windowsxamlhost**要素の後、ファイルの末尾付近にある**Button**要素の前に、次の XAML を追加します。

    ```xml
    <TextBlock Text="Selected date:" FontSize="16" FontWeight="Bold" Grid.Row="7" Grid.Column="0" />
    <TextBlock Text="{Binding Path=Date}" FontSize="16" Grid.Row="7" Grid.Column="1" />
    ```

5. ファイルの末尾付近にある**Button**要素を探し、 **Grid**プロパティを**7**から**8**に変更します。 これにより、新しい行が追加されたため、グリッド内の1行下にボタンが移動します。

    ```xml
    <Button Content="Save" Grid.Row="8" Grid.Column="0" Command="{Binding Path=SaveExpenseCommand}" Margin="5, 12, 0, 0" HorizontalAlignment="Left" Width="180" />
    ```

6. **AddNewExpense.xaml.cs**コードファイルを開きます。

7. ファイルの先頭に次のステートメントを追加します。

    ```csharp
    using Microsoft.Toolkit.Wpf.UI.XamlHost;
    ```

8. 次のイベント ハンドラーを `AddNewExpense` クラスに追加します。 このコードは、XAML ファイルで前に宣言した**Windowsxamlhost**コントロールの**childchanged**イベントを実装します。

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

    このコードでは、 **Windowsxamlhost**コントロールの**子**プロパティを使用して UWP **CalendarView**コントロールにアクセスします。 次に、このコードは、ユーザーがカレンダーから日付を選択したときにトリガーされる**Selectedchanged changed**イベントをサブスクライブします。 このイベントハンドラーは、新しい**経費**オブジェクトが作成されてデータベースに保存される、選択された日付をビューモデルに渡します。 これを行うために、このコードでは、MVVM Light NuGet パッケージによって提供されるメッセージングインフラストラクチャを使用します。 このコードは、 **SelectedDateMessage**という名前のメッセージをビューモデルに送信し、そのメッセージを受信して、選択された値で**日付**プロパティを設定します。 このシナリオでは、 **CalendarView**コントロールは単一選択モード用に構成されているため、コレクションに含まれる要素は1つだけになります。

    この時点では、 **SelectedDateMessage**クラスの実装がないため、プロジェクトはコンパイルされません。 次の手順では、このクラスを実装します。

9. **ソリューションエクスプローラー**で、 **Messages**フォルダーを右クリックし、 **[> クラス]** を選択します。 新しいクラスに**SelectedDateMessage**という名前を指定し、 **[追加]** をクリックします。

10. **SelectedDateMessage.cs**コードファイルの内容を次のコードに置き換えます。 このコードは、(MVVM Light NuGet `GalaSoft.MvvmLight.Messaging`パッケージからの) 名前空間の using ステートメントを追加し、 **messagebase**クラスから継承して、パブリックコンストラクターを通じて初期化される**DateTime**プロパティを追加します。 完了すると、ファイルは次のようになります。

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

11. 次に、このメッセージを受信して、ビューモデルの**Date**プロパティを設定するように、モデルビューを更新します。 **ソリューションエクスプローラー**で、 **viewmodel**フォルダーを展開し、 **AddNewExpenseViewModel.cs**ファイルを開きます。

12. `AddNewExpenseViewModel`クラスのパブリックコンストラクターを探し、次のコードをコンストラクターの末尾に追加します。 このコードは、 **SelectedDateMessage**を受信するように登録し、 **SelectedDate**プロパティを使用して選択した日付を抽出し、それを使用して、ビューモデルによって公開される**日付**プロパティを設定します。 このプロパティは、前に追加した**TextBlock**コントロールにバインドされているため、ユーザーが選択した日付を確認できます。

    ```csharp
    Messenger.Default.Register<SelectedDateMessage>(this, message =>
    {
        Date = message.SelectedDate;
    });
    ```

    完了すると、コンストラクターは`AddNewExpenseViewModel`次のようになります。 

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
    > 同じコードファイル内のメソッドは、経費をデータベースに保存する作業を行います。`SaveExpenseCommand` このメソッドは、 **Date**プロパティを使用して新しい**経費**オブジェクトを作成します。 このプロパティは、作成したメッセージに`SelectedDateMessage`よって CalendarView コントロールによって設定されるようになりました。

13. F5 キーを押して、デバッガーでアプリをビルドして実行します。 利用可能な従業員の1つを選択し、 **[新しい費用の追加]** ボタンをクリックします。 フォーム内のすべてのフィールドを入力し、新しい**CalendarView**コントロールから日付を選択します。 選択した日付が、コントロールの下に文字列として表示されていることを確認します。

14. **[保存]** ボタンを押します。 フォームが閉じられ、新しい経費が経費の一覧の最後に追加されます。 支出日を含む最初の列は、 **CalendarView**コントロールで選択した日付にする必要があります。

## <a name="next-steps"></a>次の手順

このチュートリアルのこの時点で、WPF の日付と時刻の制御は、マウスやキーボードの入力に加えてタッチおよびデジタルペンをサポートする UWP **CalendarView**コントロールに置き換えられました。 Windows Community Toolkit には、WPF アプリで直接使用できる UWP **CalendarView**コントロールのラップされたバージョンが用意されていませんが、汎用[windowsxamlhost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)コントロールを使用してコントロールをホストすることができました。

これで、パート 4 [の準備ができました。Windows 10 のユーザーアクティビティと通知](modernize-wpf-tutorial-4.md)を追加します。
