---
title: 顧客データベース アプリケーションの作成
description: 顧客データベースアプリケーションを作成し、基本的なエンタープライズアプリ機能を実装する方法について説明します。
keywords: enterprise、チュートリアル、顧客、データ、読み取り更新の削除、REST、認証の作成
ms.date: 05/07/2018
ms.topic: article
ms.localizationpriority: med
ms.openlocfilehash: b8cf0554464b56337e3d57b58db543092682ffa3
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258618"
---
# <a name="tutorial-create-a-customer-database-application"></a>チュートリアル: 顧客データベース アプリケーションの作成

このチュートリアルでは、顧客の一覧を管理するための簡単なアプリを作成します。 これにより、UWP におけるエンタープライズアプリの基本的な概念を選択できます。 次の方法について説明します。

* ローカルの SQL データベースに対して、作成、読み取り、更新、削除の各操作を実装します。
* データグリッドを追加して、UI で顧客データを表示および編集します。
* 基本的なフォームレイアウトで UI 要素を整列します。

このチュートリアルの開始点は、[顧客注文データベースサンプルアプリ](https://github.com/Microsoft/Windows-appsample-customers-orders-database)の簡略化されたバージョンに基づく、最小限の UI と機能を備えたシングルページアプリです。 これはと XAML C#で記述されており、これらの言語について基本的な知識を持っていることを期待しています。

![作業中のアプリのメインページ](images/customer-database-tutorial/customer-list.png)

### <a name="prerequisites"></a>前提条件

* [最新バージョンの Visual Studio と Windows 10 SDK があることを確認します。](https://developer.microsoft.com/windows/downloads/windows-10-sdk)
* [顧客データベースのチュートリアルサンプルを複製またはダウンロードする](https://github.com/microsoft/windows-tutorials-customer-database)

リポジトリを複製またはダウンロードした後、Visual Studio で**CustomerDatabaseTutorial**を開いてプロジェクトを編集できます。

> [!NOTE]
> このチュートリアルの基になっているアプリを確認するには、 [Customer Orders データベースの完全なサンプル](https://github.com/Microsoft/Windows-appsample-customers-orders-database)を参照してください。

## <a name="part-1-code-of-interest"></a>パート 1: 関心のあるコード

アプリを開いた直後に実行すると、空の画面の上部にいくつかのボタンが表示されます。 お客様には見えませんが、アプリには、いくつかのテスト顧客によってプロビジョニングされたローカルの SQLite データベースが既に含まれています。 ここから、まず、顧客を表示するための UI コントロールを実装し、次にデータベースに対する [操作の追加] に移動します。 作業を開始する前に、ここで作業を行います。

### <a name="views"></a>ビュー

**顧客 listpage .xaml**はアプリのビューで、このチュートリアルの単一ページの UI を定義します。 UI でビジュアル要素を追加または変更する必要がある場合はいつでも、このファイルを使用します。 このチュートリアルでは、次の要素を追加する手順について説明します。

* 顧客を表示および編集するための**レーダーデータグリッド**。 
* 新しい顧客の初期値を設定する**StackPanel** 。

### <a name="viewmodels"></a>ViewModels

**Viewmodel\ 顧客の listpageviewmodel, cs**は、アプリの基本的なロジックが配置されている場所です。 ビューで実行されるすべてのユーザー操作は、処理のためにこのファイルに渡されます。 このチュートリアルでは、新しいコードをいくつか追加し、次のメソッドを実装します。

* **CreateNewCustomerAsync**。新しい顧客ビューモデルオブジェクトを初期化します。
* **Deleは**、UI に表示される前に新しい顧客を削除します。
* **DeleteAndUpdateAsync**。削除ボタンのロジックを処理します。
* **Getcustomers Listasync**。データベースから顧客のリストを取得します。
* **Saveinitialchangesasync**は、新しい顧客の情報をデータベースに追加します。
* **UpdateCustomersAsync**は、追加または削除された顧客を反映するように UI を更新します。

顧客**ビューモデル**は、顧客の情報のラッパーであり、最近変更されたかどうかを追跡します。 このクラスに何も追加する必要はありませんが、他の場所に追加するコードの一部は参照できます。

サンプルの構築方法の詳細については、「[アプリの構造の概要」](../enterprise/customer-database-app-structure.md)を参照してください。

## <a name="part-2-add-the-datagrid"></a>パート 2: DataGrid の追加

顧客データの操作を開始する前に、それらの顧客を表示するための UI コントロールを追加する必要があります。 これを行うには、事前に作成されたサードパーティの**レーダー**コントロールを使用します。 **Telerik Microsoft.netcore.universalwindowsplatform** NuGet パッケージは、このプロジェクトに既に含まれています。 プロジェクトにグリッドを追加してみましょう。

1. ソリューションエクスプローラーから**Views\CustomerListPage.xaml**を開きます。 **ページ**タグ内に次のコード行を追加して、データグリッドを含む Telerik 名前空間へのマッピングを宣言します。

    ```xaml
        xmlns:telerikGrid="using:Telerik.UI.Xaml.Controls.Grid"
    ```

2. ビューのメイン**RelativePanel**内の**CommandBar**の下に、いくつかの基本的な構成オプションを使用して、**レーダー datagrid**コントロールを追加します。

    ```xaml
    <Grid
        x:Name="CustomerListRoot"
        Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <RelativePanel>
            <CommandBar
                x:Name="mainCommandBar"
                HorizontalAlignment="Stretch"
                Background="AliceBlue">
                <!--CommandBar content-->
            </CommandBar>
            <telerikGrid:RadDataGrid
                x:Name="DataGrid"
                BorderThickness="0"
                ColumnDataOperationsMode="Flyout"
                GridLinesVisibility="None"
                GroupPanelPosition="Left"
                RelativePanel.AlignLeftWithPanel="True"
                RelativePanel.AlignRightWithPanel="True"
                RelativePanel.Below="mainCommandBar" />
        </RelativePanel>
    </Grid>
    ```

3. データグリッドを追加しましたが、表示するデータが必要です。 次のコード行を追加します。

    ```xaml
    ItemsSource="{x:Bind ViewModel.Customers}"
    UserEditMode="Inline"
    ```
    これで、表示するデータのソースが定義されました。次は、UI のロジックの大部分を**ユーザーが処理**します。 ただし、プロジェクトを実行しても、表示されるデータは表示されません。 これは、ビューモデルでまだ読み込まれていないためです。

![空のアプリ (顧客なし)](images/customer-database-tutorial/blank-customer-list.png)

## <a name="part-3-read-customers"></a>パート 3: 顧客の読み取り

初期化が開始されると、 **Viewmodel\ 顧客の Listpageviewmodel.cs**が**getasync listasync**メソッドを呼び出します。 この方法では、チュートリアルに含まれている SQLite データベースからテスト顧客データを取得する必要があります。

1. **Viewmodelget-help**で、次のコードを使用して**Get顧客 listasync**メソッドを更新します。

    ```csharp
    public async Task GetCustomerListAsync()
    {
        var customers = await App.Repository.Customers.GetAsync(); 
        if (customers == null)
        {
            return;
        }
        await DispatcherHelper.ExecuteOnUIThreadAsync(() =>
        {
            Customers.Clear();
            foreach (var c in customers)
            {
                Customers.Add(new CustomerViewModel(c));
            }
        });
    }
    ```
    **Get顧客 Listasync**メソッドは、ビューモデルが読み込まれるときに呼び出されますが、この手順の前には何も実行しませんでした。 ここでは、**リポジトリ/SqlGetAsync リポジトリ**にメソッドの呼び出しを追加しました。 これにより、リポジトリに接続して、顧客オブジェクトの列挙可能なコレクションを取得できます。 次に、それらを個々のオブジェクトに解析してから、それらを内部**system.collections.objectmodel.observablecollection**に追加してから、表示および編集できるようにします。

2. アプリを実行する-顧客の一覧を表示するデータグリッドが表示されるようになります。

![最初の顧客一覧](images/customer-database-tutorial/initial-customers.png)

## <a name="part-4-edit-customers"></a>パート 4: 顧客を編集する

データグリッド内のエントリをダブルクリックすることで編集できますが、UI で行った変更は、分離コードで顧客のコレクションにも加えられていることを確認する必要があります。 これは、双方向のデータバインディングを実装する必要があることを意味します。 詳細については、「[データバインディングの概要」を](../get-started/display-customers-in-list-learning-track.md)参照してください。

1. まず、 **ViewmodelINotifyPropertyChanged**インターフェイスを実装することを宣言します。

    ```csharp
    public class CustomerListPageViewModel : INotifyPropertyChanged
    ```

2. 次に、クラスの本体内で、次のイベントとメソッドを追加します。

    ```csharp
    public event PropertyChangedEventHandler PropertyChanged;

    public void OnPropertyChanged([CallerMemberName] string propertyName = null) =>
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    ```

    **OnPropertyChanged**メソッドを使用すると、setter は、双方向のデータバインディングに必要な**PropertyChanged**イベントを簡単に生成できます。

3. 次の関数呼び出しを使用して、 **Selectedcustomer**の setter を更新します。

    ```csharp
    public CustomerViewModel SelectedCustomer
    {
        get => _selectedCustomer;
        set
        {
            if (_selectedCustomer != value)
            {
                _selectedCustomer = value;
                OnPropertyChanged();
            }
        }
    }
    ```

4. **Views\CustomerListPage.xaml**で、 **selectedcustomer**プロパティをデータグリッドに追加します。

    ```xaml
    SelectedItem="{x:Bind ViewModel.SelectedCustomer, Mode=TwoWay}"
    ```

    これにより、データグリッド内のユーザーの選択が、分離コード内の対応する Customer オブジェクトに関連付けられます。 TwoWay binding モードでは、UI に加えられた変更をそのオブジェクトに反映させることができます。

5. アプリを実行します。 グリッドに表示されている顧客を確認し、UI を通じて基になるデータに変更を加えることができます。

![データグリッドで顧客を編集する](images/customer-database-tutorial/edit-customer-inline.png)

## <a name="part-5-update-customers"></a>パート 5: 顧客を更新する

顧客を表示して編集できるようになったので、変更をデータベースにプッシュし、他のユーザーが行った更新をプルできるようにする必要があります。

1. **ViewmodelUpdateCustomersAsync**に戻り、[] メソッドに移動します (& a) 。 変更をデータベースにプッシュし、新しい情報を取得するには、次のコードで更新します。

    ```csharp
    public async Task UpdateCustomersAsync()
    {
        foreach (var modifiedCustomer in Customers
            .Where(x => x.IsModified).Select(x => x.Model))
        {
            await App.Repository.Customers.UpsertAsync(modifiedCustomer);
        }
        await GetCustomerListAsync();
    }
    ```
    このコードでは、ユーザーが変更されるたびに自動的に更新される**viewmodel\** の**ismodified**プロパティを使用します。 これにより、不要な呼び出しを回避して、更新された顧客からの変更のみをデータベースにプッシュすることができます。

## <a name="part-6-create-a-new-customer"></a>パート 6: 新しい顧客を作成する

新しい顧客を追加すると、そのプロパティの値を指定する前に、ユーザーが UI に追加された場合に空白行として表示されるため、問題が生じます。 これは問題ではありませんが、ここでは顧客の初期値を簡単に設定できるようにします。 このチュートリアルでは、簡単に折りたたみ可能なパネルを追加しますが、追加する情報が多い場合は、この目的のために別のページを作成することもできます。

### <a name="update-the-code-behind"></a>分離コードを更新する

1. 新しいプライベートフィールドとパブリックプロパティを**Viewmodel\ 顧客の Listpageviewmodel.cs**に追加します。 これは、パネルを表示するかどうかを制御するために使用されます。

    ```csharp
    private bool _addingNewCustomer = false;

    public bool AddingNewCustomer
    {
        get => _addingNewCustomer;
        set
        {
            if (_addingNewCustomer != value)
            {
                _addingNewCustomer = value;
                OnPropertyChanged();
            }
        }
    }
    ```

2. 新しいパブリックプロパティをビューモデルに**追加します。この**値は、[追加] の値の逆になります。 これは、パネルが表示されているときに通常のコマンドバーボタンを無効にするために使用されます。

    ```csharp
    public bool EnableCommandBar => !AddingNewCustomer;
    ```
    ここでは、折りたたみ可能なパネルを表示し、その中で編集する顧客を作成する方法が必要になります。 

3. 新しく作成された顧客を保持するために、新しいプライベート fiend とパブリックプロパティをビューモデルに追加します。

    ```csharp
    private CustomerViewModel _newCustomer;

    public CustomerViewModel NewCustomer
    {
        get => _newCustomer;
        set
        {
            if (_newCustomer != value)
            {
                _newCustomer = value;
                OnPropertyChanged();
            }
        }
    }
    ```

2.  **CreateNewCustomerAsync**メソッドを更新して新しい顧客を作成し、それをリポジトリに追加して、選択した顧客として設定します。

    ```csharp
    public async Task CreateNewCustomerAsync()
    {
        CustomerViewModel newCustomer = new CustomerViewModel(new Models.Customer());
        NewCustomer = newCustomer;
        await App.Repository.Customers.UpsertAsync(NewCustomer.Model);
        AddingNewCustomer = true;
    }
    ```

3. **Saveinitial変更 async**メソッドを更新して、新しく作成した顧客をリポジトリに追加し、UI を更新して、パネルを閉じます。

    ```csharp
    public async Task SaveInitialChangesAsync()
    {
        await App.Repository.Customers.UpsertAsync(NewCustomer.Model);
        await UpdateCustomersAsync();
        AddingNewCustomer = false;
    }
    ```
4. 次のコード行を、add **Newcustomer**の set アクセス操作子の最後の行として追加します。

    ```csharp
    OnPropertyChanged(nameof(EnableCommandBar));
    ```

    これにより、 **EnableCommandBar**が**変更されるたびに**、自動的に更新されるようになります。

### <a name="update-the-ui"></a>UI を更新する

1. **Views\CustomerListPage.xaml**に戻り、 **CommandBar**とデータグリッドの間に次のプロパティを持つ**StackPanel**を追加します。

    ```xaml
    <StackPanel
        x:Name="newCustomerPanel"
        Orientation="Horizontal"
        x:Load="{x:Bind ViewModel.AddingNewCustomer, Mode=OneWay}"
        RelativePanel.Below="mainCommandBar">
    </StackPanel>
    ```
    **X:Load**属性を使用すると、新しい顧客を追加するときにのみこのパネルが表示されるようになります。

2. データグリッドの位置を次のように変更して、新しいパネルが表示されたときに移動するようにします。

    ```xaml
    RelativePanel.Below="newCustomerPanel"
    ```

3. 4つの**テキストボックス**コントロールを使用して、スタックパネルを更新します。 新しい顧客の個々のプロパティにバインドし、データグリッドに追加する前にその値を編集できるようにします。

    ```xaml
    <StackPanel
        x:Name="newCustomerPanel"
        Orientation="Horizontal"
        x:Load="{x:Bind ViewModel.AddingNewCustomer, Mode=OneWay}"
        RelativePanel.Below="mainCommandBar">
        <TextBox
            Header="First name"
            PlaceholderText="First"
            Margin="8,8,16,8"
            MinWidth="120"
            Text="{x:Bind ViewModel.NewCustomer.FirstName, Mode=TwoWay, UpdateSourceTrigger=PropertyChanged}"/>
        <TextBox
            Header="Last name"
            PlaceholderText="Last"
            Margin="0,8,16,8"
            MinWidth="120"
            Text="{x:Bind ViewModel.NewCustomer.LastName, Mode=TwoWay, UpdateSourceTrigger=PropertyChanged}"/>
        <TextBox
            Header="Address"
            PlaceholderText="1234 Address St, Redmond WA 00000"
            Margin="0,8,16,8"
            MinWidth="280"
            Text="{x:Bind ViewModel.NewCustomer.Address, Mode=TwoWay, UpdateSourceTrigger=PropertyChanged}"/>
        <TextBox
            Header="Company"
            PlaceholderText="Company"
            Margin="0,8,16,8"
            MinWidth="120"
            Text="{x:Bind ViewModel.NewCustomer.Company, Mode=TwoWay, UpdateSourceTrigger=PropertyChanged}"/>
    </StackPanel>
    ```

4. 新しいスタックパネルに単純なボタンを追加して、新しく作成した顧客を保存します。

    ```xaml
    <StackPanel>
        <!--Text boxes from step 3-->
        <AppBarButton
            x:Name="SaveNewCustomer"
            Click="{x:Bind ViewModel.SaveInitialChangesAsync}"
            Icon="Save"/>
    </StackPanel>
    ```

5. **コマンドバー**を更新すると、[スタック] パネルが表示されているときに、[作成]、[削除]、および [更新] の通常のボタンが無効になります。

    ```xaml
    <CommandBar
        x:Name="mainCommandBar"
        HorizontalAlignment="Stretch"
        IsEnabled="{x:Bind ViewModel.EnableCommandBar, Mode=OneWay}"
        Background="AliceBlue">
        <!--App bar buttons-->
    </CommandBar>
    ```

6. アプリを実行します。 これで、顧客を作成し、そのデータを [スタック] パネルに入力できるようになりました。

![新しい顧客を作成する](images/customer-database-tutorial/add-new-customer.png)

## <a name="part-7-delete-a-customer"></a>パート 7: 顧客を削除する

顧客の削除は、実装する必要のある最終的な基本操作です。 データグリッド内で選択した顧客を削除する場合は、UI を更新するために**UpdateCustomersAsync**をすぐに呼び出す必要があります。 ただし、先ほど作成した顧客を削除する場合は、そのメソッドを呼び出す必要はありません。

1. **ViewmodelDeleteAndUpdateAsync**に移動し、次のようにして、メソッドを更新します。

    ```csharp
    public async void DeleteAndUpdateAsync()
    {
        if (SelectedCustomer != null)
        {
            await App.Repository.Customers.DeleteAsync(_selectedCustomer.Model.Id);
        }
        await UpdateCustomersAsync();
    }
    ```

2. **Views\CustomerListPage.xaml**で、2番目のボタンが表示されるように、新しい顧客を追加するためにスタックパネルを更新します。

    ```xaml
    <StackPanel>
        <!--Text boxes for adding a new customer-->
        <AppBarButton
            x:Name="DeleteNewCustomer"
            Click="{x:Bind ViewModel.DeleteNewCustomerAsync}"
            Icon="Cancel"/>
        <AppBarButton
            x:Name="SaveNewCustomer"
            Click="{x:Bind ViewModel.SaveInitialChangesAsync}"
            Icon="Save"/>
    </StackPanel>
    ```

3. 次のように、 **Viewmodelewcustomer listpageviewmodel, cs**で、新しい顧客を削除するように**deleを**更新します。

    ```csharp
    public async Task DeleteNewCustomerAsync()
    {
        if (NewCustomer != null)
        {
            await App.Repository.Customers.DeleteAsync(_newCustomer.Model.Id);
            AddingNewCustomer = false;
        }
    }
    ```

4. アプリを実行します。 データグリッドまたはスタックパネル内の顧客を削除できるようになりました。

![新しい顧客を削除する](images/customer-database-tutorial/delete-new-customer.png)

## <a name="conclusion"></a>まとめ

これで終了です。 すべての処理が完了すると、アプリケーションにローカルデータベース操作がすべて含まれるようになります。 UI 内で顧客を作成、読み取り、更新、削除することができます。これらの変更はデータベースに保存され、アプリのさまざまな起動にわたって保持されます。

これで完了です。次の点を考慮してください。
* アプリがビルドされている理由の詳細については、「[アプリの構造の概要」](../enterprise/customer-database-app-structure.md)を参照してください。
* このチュートリアルの基になっているアプリを確認するには、 [Customer Orders データベースの完全なサンプル](https://github.com/Microsoft/Windows-appsample-customers-orders-database)を参照してください。

または、問題がある場合は、続行することができます...

## <a name="going-further-connect-to-a-remote-database"></a>詳細: リモートデータベースへの接続

ローカルの SQLite データベースに対してこれらの呼び出しを実装する方法の手順について説明しました。 では、リモートデータベースを使用する場合はどうでしょうか。

これを試してみる場合は、独自の Azure Active Directory (AAD) アカウントと、独自のデータソースをホストする機能が必要です。

REST 呼び出しを処理するための認証と関数を追加し、と対話するためのリモートデータベースを作成する必要があります。 [Customer Orders データベース](https://github.com/Microsoft/Windows-appsample-customers-orders-database)の完全なサンプルには、必要な操作ごとに参照できるコードが用意されています。

### <a name="settings-and-configuration"></a>設定と構成

独自のリモートデータベースに接続するために必要な手順については、[サンプルの readme](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/README.md)をご覧ください。 次の手順を実行する必要があります。

* Azure アカウントのクライアント Id を[Constants.cs](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoRepository/Constants.cs)に入力します。
* リモートデータベースの url を[Constants.cs](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoRepository/Constants.cs)に指定します。
* データベースの接続文字列を[Constants.cs](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoRepository/Constants.cs)に指定します。
* アプリを Microsoft Store に関連付けます。
* [サービスプロジェクト](https://github.com/Microsoft/Windows-appsample-customers-orders-database/tree/master/ContosoService)をアプリにコピーし、Azure にデプロイします。

### <a name="authentication"></a>認証

認証シーケンスを開始するボタンを作成し、ポップアップまたは別のページを作成して、ユーザーの情報を収集する必要があります。 これを作成したら、ユーザーの情報を要求し、それを使用してアクセストークンを取得するコードを指定する必要があります。 Customer Orders データベースのサンプルでは、 **Webaccountmanager**ライブラリを使用して Microsoft Graph 呼び出しをラップしてトークンを取得し、AAD アカウントに対する認証を処理します。

* 認証ロジックは[**AuthenticationViewModel.cs**](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoApp/ViewModels/AuthenticationViewModel.cs)に実装されています。
* 認証プロセスは、カスタム[**authenticationcontrol .xaml**](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoApp/UserControls/AuthenticationControl.xaml)コントロールに表示されます。

### <a name="rest-calls"></a>REST 呼び出し

REST 呼び出しを実装するために、このチュートリアルで追加したコードを変更する必要はありません。 代わりに、次の操作を行う必要があります。

* **ICustomerRepository**インターフェイスと**ITutorialRepository**インターフェイスの新しい実装を作成し、SQLITE ではなく REST を使用して同じ関数のセットを実装します。 JSON をシリアル化および逆シリアル化する必要があります。必要に応じて、別の**Httphelper**クラスで REST 呼び出しをラップすることができます。 詳細について[は、完全なサンプル](https://github.com/Microsoft/Windows-appsample-customers-orders-database/tree/master/ContosoRepository/Rest)を参照してください。
* **App.xaml.cs**で、REST リポジトリを初期化する新しい関数を作成し、アプリの初期化時に**SqliteDatabase**の代わりに呼び出します。 ここでも、[完全なサンプル](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoApp/App.xaml.cs)を参照してください。

これら3つの手順がすべて完了したら、アプリを使用して AAD アカウントに対する認証を行うことができます。 リモートデータベースへの REST 呼び出しでは、ローカルの SQLite 呼び出しが置き換えられますが、ユーザーエクスペリエンスは同じである必要があります。 さらに大きいを感じている場合は、[設定] ページを追加して、ユーザーがこの2つを動的に切り替えることができるようにすることができます。
