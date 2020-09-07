---
title: UWP アプリでの SQL Server データベースの使用
description: UWP アプリを直接 SQL Server データベースに接続し、System.Data.SqlClient を使用してデータを保存および取得する方法について説明します。
ms.date: 06/26/2020
ms.topic: article
keywords: windows 10, UWP, SQL Server, データベース
ms.localizationpriority: medium
ms.openlocfilehash: b775f1b6f7850b1e7c0b951c0126fcb97ceec8a2
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89170186"
---
# <a name="use-a-sql-server-database-in-a-uwp-app"></a>UWP アプリでの SQL Server データベースの使用
アプリで [System.Data.SqlClient](/dotnet/api/system.data.sqlclient) 名前空間のクラスを使用して、SQL Server データベースに直接接続し、データを保存および取得することができます。

このガイドでは、それを行う方法の 1 つを示します。 [Northwind](/dotnet/framework/data/adonet/sql/linq/downloading-sample-databases) サンプル データベースを SQL Server インスタンスにインストールし、ここで示すスニペットを使用して、Northwind サンプルのデータベースから取得した製品を表示する基本的な UI を作成します。

![Northwind 製品](images/products-northwind.png)

このガイドで示すスニペットは、このもっと[完全なサンプル](https://github.com/StefanWickDev/IgniteDemos/tree/master/NorthwindDemo)に基づいています。

## <a name="first-set-up-your-solution"></a>まず、ソリューションをセットアップします。

アプリを SQL Server データベースに直接接続するために、プロジェクトの最小バージョンが Fall Creators Update を対象にしていることを確認します。  UWP プロジェクトのプロパティ ページにその情報があります。

![Windows SDK の最小バージョン](images/min-version-fall-creators.png)

マニフェスト デザイナーで、UWP プロジェクトの **Package.appxmanifest** ファイルを開きます。

Windows 認証を使用してご利用の SQL Server を認証する場合は、 **[機能]** タブで **[エンタープライズ認証]** チェックボックスを選択します。

![エンタープライズ認証機能](images/enterprise-authentication.png)

> [!IMPORTANT]
> また、Windows 認証を使用しているかどうかにかかわらず、**インターネット (クライアントとサーバー)** 、**インターネット (クライアント)** 、および**プライベート ネットワーク (クライアントとサーバー)** を選択する必要があります。

<a id="use-data" />

## <a name="add-and-retrieve-data-in-a-sql-server-database"></a>SQL Server データベースのデータの追加と取得

このセクションでは、以下のことを行います。

:1:接続文字列を追加します。

:2:製品データを保持するクラスを作成します。

:3:SQL Server データベースから製品を取得します。

:4:基本的なユーザー インターフェイスを追加します。

:5:UI に製品を追加します。

>[!NOTE]
> このセクションでは、データ アクセス コードを編成する方法の 1 つを示します。 つまり、[System.Data.SqlClient](/dotnet/api/system.data.sqlclient) を使用して、SQL Server データベースのデータを保存および取得する方法の例を示すだけです。 アプリケーションの設計に最も適した方法でコードを編成してください。

### <a name="add-a-connection-string"></a>接続文字列を追加する

**App.xaml.cs** ファイルで、``App`` クラスにプロパティを追加します。これにより、ソリューションに含まれる他のクラスが接続文字列にアクセスできるようになります。

接続文字列は、SQL Server Express のインスタンスの Northwind データベースを指しています。

```csharp
sealed partial class App : Application
{
    // Connection string for using Windows Authentication.
    private string connectionString =
        @"Data Source=YourServerName\SQLEXPRESS;Initial Catalog=NORTHWIND;Integrated Security=SSPI";

    // This is an example connection string for using SQL Server Authentication.
    // private string connectionString =
    //     @"Data Source=YourServerName\YourInstanceName;Initial Catalog=DatabaseName; User Id=XXXXX; Password=XXXXX";

    public string ConnectionString { get => connectionString; set => connectionString = value; }

    ...
}
```

### <a name="create-a-class-to-hold-product-data"></a>製品データを保持するクラスを作成する

XAML UI でこのクラスのプロパティに属性をバインドできるように、[INotifyPropertyChanged](/dotnet/api/system.componentmodel.inotifypropertychanged) イベントを実装するクラスを作成します。

```csharp
public class Product : INotifyPropertyChanged
{
    public int ProductID { get; set; }
    public string ProductCode { get { return ProductID.ToString(); } }
    public string ProductName { get; set; }
    public string QuantityPerUnit { get; set; }
    public decimal UnitPrice { get; set; }
    public string UnitPriceString { get { return UnitPrice.ToString("######.00"); } }
    public int UnitsInStock { get; set; }
    public string UnitsInStockString { get { return UnitsInStock.ToString("#####0"); } }
    public int CategoryId { get; set; }

    public event PropertyChangedEventHandler PropertyChanged;
    private void NotifyPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }

}
```

### <a name="retrieve-products-from-the-sql-server-database"></a>SQL Server データベースから製品を取得します。

Northwind サンプル データベースから製品を取得し、``Product``インスタンスの [ObservableCollection](/dotnet/api/system.collections.objectmodel.observablecollection-1) コレクションとしてそれらを返すメソッドを作成します。

```csharp
public ObservableCollection<Product> GetProducts(string connectionString)
{
    const string GetProductsQuery = "select ProductID, ProductName, QuantityPerUnit," +
       " UnitPrice, UnitsInStock, Products.CategoryID " +
       " from Products inner join Categories on Products.CategoryID = Categories.CategoryID " +
       " where Discontinued = 0";

    var products = new ObservableCollection<Product>();
    try
    {
        using (SqlConnection conn = new SqlConnection(connectionString))
        {
            conn.Open();
            if (conn.State == System.Data.ConnectionState.Open)
            {
                using (SqlCommand cmd = conn.CreateCommand())
                {
                    cmd.CommandText = GetProductsQuery;
                    using (SqlDataReader reader = cmd.ExecuteReader())
                    {
                        while (reader.Read())
                        {
                            var product = new Product();
                            product.ProductID = reader.GetInt32(0);
                            product.ProductName = reader.GetString(1);
                            product.QuantityPerUnit = reader.GetString(2);
                            product.UnitPrice = reader.GetDecimal(3);
                            product.UnitsInStock = reader.GetInt16(4);
                            product.CategoryId = reader.GetInt32(5);
                            products.Add(product);
                        }
                    }
                }
            }
        }
        return products;
    }
    catch (Exception eSql)
    {
        Debug.WriteLine("Exception: " + eSql.Message);
    }
    return null;
}
```

### <a name="add-a-basic-user-interface"></a>基本的なユーザー インターフェイスを追加する

 次の XAML を、UWP プロジェクトの **MainPage.xaml** ファイルに追加します。

 この XAML では、[ListView](/uwp/api/windows.ui.xaml.controls.listview) を作成して前のスニペットで返された各製品を表示し、[ListView](/uwp/api/windows.ui.xaml.controls.listview) 内の各行の属性を ``Product`` クラスで定義したプロパティにバインドします。

```xml
<Grid Background="{ThemeResource SystemControlAcrylicWindowBrush}">
    <RelativePanel>
        <ListView Name="InventoryList"
                  SelectionMode="Single"
                  ScrollViewer.VerticalScrollBarVisibility="Auto"
                  ScrollViewer.IsVerticalRailEnabled="True"
                  ScrollViewer.VerticalScrollMode="Enabled"
                  ScrollViewer.HorizontalScrollMode="Enabled"
                  ScrollViewer.HorizontalScrollBarVisibility="Auto"
                  ScrollViewer.IsHorizontalRailEnabled="True"
                  Margin="20">
            <ListView.HeaderTemplate>
                <DataTemplate>
                    <StackPanel Orientation="Horizontal"  >
                        <TextBlock Text="ID" Margin="8,0" Width="50" Foreground="DarkRed" />
                        <TextBlock Text="Product description" Width="300" Foreground="DarkRed" />
                        <TextBlock Text="Packaging" Width="200" Foreground="DarkRed" />
                        <TextBlock Text="Price" Width="80" Foreground="DarkRed" />
                        <TextBlock Text="In stock" Width="80" Foreground="DarkRed" />
                    </StackPanel>
                </DataTemplate>
            </ListView.HeaderTemplate>
            <ListView.ItemTemplate>
                <DataTemplate x:DataType="local:Product">
                    <StackPanel Orientation="Horizontal" >
                        <TextBlock Name="ItemId"
                                    Text="{x:Bind ProductCode}"
                                    Width="50" />
                        <TextBlock Name="ItemName"
                                    Text="{x:Bind ProductName}"
                                    Width="300" />
                        <TextBlock Text="{x:Bind QuantityPerUnit}"
                                   Width="200" />
                        <TextBlock Text="{x:Bind UnitPriceString}"
                                   Width="80" />
                        <TextBlock Text="{x:Bind UnitsInStockString}"
                                   Width="80" />
                    </StackPanel>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </RelativePanel>
</Grid>
```

### <a name="show-products-in-the-listview"></a>ListView で製品を表示する

**MainPage.xaml.cs** ファイルを開き、[ListView](/uwp/api/windows.ui.xaml.controls.listview) の **ItemSource** プロパティを ``Product`` インスタンスの [ObservableCollection](/dotnet/api/system.collections.objectmodel.observablecollection-1) に設定するコードを、``MainPage`` クラスのコンストラクターに追加します。

```csharp
public MainPage()
{
    this.InitializeComponent();
    InventoryList.ItemsSource = GetProducts((App.Current as App).ConnectionString);
}
```

プロジェクトを開始すると、Northwind サンプル データベースから取得した製品が UI に表示されます。

![Northwind 製品](images/products-northwind.png)

[System.Data.SqlClient](/dotnet/api/system.data.sqlclient) 名前空間を調べて、SQL Server データベース内のデータを使用して他に何ができるかを確認してください。

## <a name="trouble-connecting-to-your-database"></a>データベースへの接続で問題が発生した場合

ほとんどの場合、SQL Server 構成のいくつかの側面を変更する必要があります。 Windows フォームや WPF アプリケーションなどの別の種類のデスクトップ アプリケーションからデータベースに接続できる場合は、SQL Server の TCP/IP を有効にしていることを確認します。 これは、**コンピューターの管理**コンソールで行うことができます。

![コンピューターの管理](images/computer-management.png)

次に、SQL Server Browser サービスが実行されていることを確認します。

![SQL Server Browser サービス](images/sql-browser-service.png)

## <a name="next-steps"></a>次の手順

**簡易データベースを使用して、ユーザー デバイスにデータを保存する**

「[UWP アプリでの SQLite データベースの使用](sqlite-databases.md)」をご覧ください。

**異なるプラットフォームにわたる異なるアプリの間でコードを共有する**

[デスクトップと UWP 間のコード共有](../porting/desktop-to-uwp-migrate.md)に関するページをご覧ください。

**Azure SQL バックエンドでマスター/詳細ページを追加する**

[顧客注文データベースのサンプル](https://github.com/Microsoft/Windows-appsample-customers-orders-database)をご覧ください。