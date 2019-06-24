---
Description: デスクトップ アプリケーションおよび UWP アプリ間で共有コード
title: デスクトップ アプリケーションおよび UWP アプリ間で共有コード
ms.date: 10/03/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 344ee7060edcee3376e271fc21e104490d8724d7
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/21/2019
ms.locfileid: "67319710"
---
# <a name="move-from-a-desktop-application-to-uwp"></a>UWP へのデスクトップ アプリケーションからの移動します。

.NET Framework (WPF と Windows フォームを含む) を使用して構築された既存のデスクトップ アプリケーションがある場合またはC++Win32 Api では、Windows 10 とユニバーサル Windows プラットフォーム (UWP) に移動するためのいくつかのオプションがあります。

## <a name="package-your-desktop-application-in-an-msix-package"></a>MSIX パッケージ内のデスクトップ、アプリケーションをパッケージ化します。

Windows 10 の多数の機能へのアクセスを取得する MSIX パッケージをデスクトップ アプリケーションをパッケージ化することができます。 MSIX は、UWP、WPF、Windows フォーム、Win32 アプリを含む、あらゆる Windows アプリ用のユニバーサル パッケージ化エクスペリエンスを提供するモダンな Windows アプリ パッケージ形式です。 MSIX パッケージにデスクトップ Windows アプリをパッケージ化することで、堅牢なインストール、更新エクスペリエンス、柔軟な機能システムによる管理されたセキュリティ モデル、Microsoft Store のサポート、エンタープライズ管理、および多くのカスタム配布モデルにアクセスできます。 ソース コードがあるかどうか、またはのみ (MSI、または、APP-V のインストーラー) などの既存のインストーラー ファイルがあるかどうか、アプリケーションをパッケージ化することができます。 アプリケーションをパッケージ化したら、パッケージの拡張機能とその他の UWP コンポーネントなどの UWP 機能を統合できます。

詳細については、次を参照してください。[デスクトップ アプリケーション (デスクトップ ブリッジ) パッケージ](/windows/msix/desktop/desktop-to-uwp-root)と[パッケージ id が必要な機能](/windows/apps/desktop/modernize/modernize-packaged-apps)します。

## <a name="use-uwp-apis"></a>UWP Api を使用して、

多くの UWP API を WPF、Windows フォーム、または C++ Win32 デスクトップ アプリで直接呼び出して、Windows 10 ユーザーの利便性を高めるモダン エクスペリエンスを統合できます。 たとえば、UWP API を呼び出して、トースト通知をデスクトップ アプリに追加できます。

詳細については、「[Use UWP APIs in desktop apps (デスクトップ アプリでの UWP API の使用)](/windows/apps/desktop/modernize/desktop-to-uwp-enhance)」を参照してください。

## <a name="migrate-a-net-framework-app-to-a-uwp-app"></a>移行、.NET Framework アプリを UWP アプリ

アプリケーションを .NET Framework で実行する場合は、.NET Standard 2.0 を利用して UWP アプリに移行することができます。 .NET Standard 2.0 クラス ライブラリにことができますし、.NET Standard 2.0 ライブラリを参照する UWP アプリを作成すると、多くのコードに移動します。 

### <a name="share-code-in-a-net-standard-20-library"></a>.NET Standard 2.0 ライブラリでコードを共有する

.NET Framework でアプリケーションを実行する場合は、することができますに .NET Standard 2.0 クラス ライブラリのコードを配置します。 標準で定義されている API を使用しているコードは、UWP アプリで再利用できます。 .NET Standard 2.0 に含まれる API が大幅に増えたため、.NET Standard ライブラリでのコード共有は、従来よりずっと簡単になっています。

詳細については、通知するビデオを次に示します。

> [!VIDEO https://www.youtube-nocookie.com/embed/YI4MurjfMn8?list=PLRAdsfhKI4OWx321A_pr-7HhRNk7wOLLY&amp;ecver=1]

#### <a name="add-net-standard-libraries"></a>.NET Standard ライブラリを追加する

まず、1 つ以上の .NET Standard クラス ライブラリをソリューションに追加します。  

![.NET Standard プロジェクトの追加](images/desktop-to-uwp/dot-net-standard-project-template.png)

ソリューションに追加するライブラリの数は、計画しているコード編成によって異なります。

各クラス ライブラリのターゲットが **.NET Standard 2.0** になっていることを確認してください。

![.NET Standard 2.0 をターゲットにする](images/desktop-to-uwp/target-standard-20.png)

この設定には、クラス ライブラリ プロジェクトのプロパティ ページからアクセスできます。

デスクトップ アプリケーション プロジェクトから、クラス ライブラリ プロジェクトへの参照を追加します。

![クラス ライブラリ参照](images/desktop-to-uwp/class-library-reference.png)

次に、ツールを使用して、どの程度のコードが標準に準拠しているか調べます。 これにより、コードをライブラリに移行する前に、どの部分を再利用でき、どの部分で最小限の変更が必要になり、どの部分がアプリケーション固有にしておくのかを決定できます。

#### <a name="check-library-and-code-compatibility"></a>ライブラリとコードの互換性を確認する

まず Nuget パッケージと、サード パーティから取得したその他の dll ファイルから始めます。

アプリケーションでこれらを使用する場合は、.NET Standard 2.0 と互換性があるかどうかを確認します。 そのためには、Visual Studio 拡張機能またはコマンド ライン ユーティリティを使用できます。

これらの同じツールを使用して、コードを分析します。 ここでツール ([dotnet apiport](https://github.com/Microsoft/dotnet-apiport/releases)) をダウンロードし、使用方法に関するビデオをご覧ください。
&nbsp;
> [!VIDEO https://www.youtube-nocookie.com/embed/rzs_FGPyAlY?list=PLRAdsfhKI4OWx321A_pr-7HhRNk7wOLLY&amp;ecver=2]

コードに標準との互換性がない場合は、そのコードを実装するための他の方法を検討してください。 まず [.NET API ブラウザー](https://docs.microsoft.com/dotnet/api/?view=netstandard-2.0)を開きます。 このブラウザーを使用して、.NET Standard 2.0 に含まれている API を確認します。 一覧の範囲として .NET Standard 2.0 を指定してください。

![.NET オプション](images/desktop-to-uwp/dot-net-option.png)

コードの一部はプラットフォーム固有でありデスクトップ アプリケーション プロジェクト内に残す必要があります。

#### <a name="example-migrating-data-access-code-to-a-net-standard-20-library"></a>以下に例を示します.NET Standard 2.0 ライブラリへのデータ アクセス コードの移行

Northwind サンプル データベースから顧客を表示する非常に基本的な Windows フォーム アプリケーションがあると仮定します。

![Windows フォーム アプリ](images/desktop-to-uwp/win-forms-app.png)

このプロジェクトには、.NET Standard 2.0 クラス ライブラリおよび **Northwind** という静的クラスが含まれています。 このコードでは ``SQLConnection`` クラス、``SqlCommand`` クラス、``SqlDataReader`` クラスが使用されており、これらのクラスが .NET Standard 2.0 にないため、**Northwind** クラスに移行してもコンパイルできません。

```csharp
public static ArrayList GetCustomerNames()
{
    ArrayList customers = new ArrayList();

    using (SqlConnection conn = new SqlConnection())
    {
        conn.ConnectionString =
            @"Data Source=" +
            @"<Your Server Name>\SQLEXPRESS;Initial Catalog=NORTHWIND;Integrated Security=SSPI";

        conn.Open();

        SqlCommand command = new SqlCommand("select ContactName from customers order by ContactName asc", conn);

        using (SqlDataReader reader = command.ExecuteReader())
        {
            while (reader.Read())
            {
                customers.Add(reader[0]);
            }
        }
    }

    return customers;
}

```
ただし、[.NET API ブラウザー](https://docs.microsoft.com/dotnet/api/?view=netstandard-2.0)を使用して代わりのクラスを見つけることができます。 ``DbConnection``、``DbCommand``、``DbDataReader`` の各クラスはすべて .NET Standard 2.0 で利用可能であるため、それらを代わりに使用することができます。  

この改訂バージョンではこれらのクラスを使用して顧客の一覧を取得しますが、``DbConnection`` クラスを作成するには、クライアント アプリケーションで作成するファクトリ オブジェクトを渡す必要があります。

```csharp
public static ArrayList GetCustomerNames(DbProviderFactory factory)
{
    ArrayList customers = new ArrayList();

    using (DbConnection conn = factory.CreateConnection())
    {
        conn.ConnectionString = @"Data Source=" +
                        @"<Your Server Name>\SQLEXPRESS;Initial Catalog=NORTHWIND;Integrated Security=SSPI";

        conn.Open();

        DbCommand command = factory.CreateCommand();
        command.Connection = conn;
        command.CommandText = "select ContactName from customers order by ContactName asc";

        using (DbDataReader reader = command.ExecuteReader())
        {
            while (reader.Read())
            {
                customers.Add(reader[0]);
            }
        }
    }

    return customers;
}
```  

Windows フォームの分離コード ページでは、ファクトリ インスタンスを作成しメソッドに渡すことができます。

```csharp
public partial class Customers : Form
{
    public Customers()
    {
        InitializeComponent();

        dataGridView1.Rows.Clear();

        SqlClientFactory factory = SqlClientFactory.Instance;

        foreach (string customer in Northwind.GetCustomerNames(factory))
        {
            dataGridView1.Rows.Add(customer);
        }
    }
}
```

### <a name="create-a-uwp-app"></a>UWP アプリを作成します。

これで、ソリューションに UWP アプリを追加する準備ができました。

![Desktop to UWP Bridge のイメージ](images/desktop-to-uwp/adaptive-ui.png)

UI ページは XAML で設計し、デバイス固有またはプラットフォーム固有のコードも記述する必要がありますが、完了すると、すべての Windows 10 デバイスをターゲットにすることができます。アプリ ページはさまざまな画面サイズや解像度に合わせて調整されるモダンな使用感になります。

アプリはキーボードとマウスだけではなく、他の入力メカニズムにも応答し、さまざまなデバイスで直感的に機能や設定を使用できます。 つまり、ユーザーが操作方法を 1 回だけ学習すると、デバイスに関係なく慣れた方法で操作できます。

これらは、UWP による利点のごく一部です。 詳しくは、「[Windowsで優れたエクスペリエンスを実現](https://developer.microsoft.com/windows/why-build-for-uwp)」をご覧ください。

#### <a name="add-a-uwp-project"></a>UWP プロジェクトを追加する

まず、UWP プロジェクトをソリューションに追加します。

![UWP プロジェクト](images/desktop-to-uwp/new-uwp-app.png)

次に、UWP プロジェクトから、.NET Standard 2.0 ライブラリ プロジェクトへの参照を追加します。

![クラス ライブラリ参照](images/desktop-to-uwp/class-library-reference2.png)

#### <a name="build-your-pages"></a>ページをビルドする

XAML ページを追加し、.NET Standard 2.0 ライブラリ内のコードを呼び出します。

![UWP アプリ](images/desktop-to-uwp/uwp-app.png)

```xml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <StackPanel x:Name="customerStackPanel">
        <ListView x:Name="customerList"/>
    </StackPanel>
</Grid>
```

```csharp
public sealed partial class MainPage : Page
{
    public MainPage()
    {
        this.InitializeComponent();

        SqlClientFactory factory = SqlClientFactory.Instance;

        customerList.ItemsSource = Northwind.GetCustomerNames(factory);
    }
}
```

UWP を使い始めるには、「[UWP アプリとは](https://docs.microsoft.com/windows/uwp/get-started/universal-application-platform-guide)」をご覧ください。

### <a name="reach-ios-and-android-devices"></a>iOS および Android デバイスへのリーチ

Xamarin プロジェクトを追加することにより、Android デバイスと iOS デバイスをターゲットにすることができます。  

![Xamarin アプリ](images/desktop-to-uwp/xamarin-apps.png)

これらのプロジェクトでは、C# でプラットフォーム固有およびデバイスに固有の API へのフル アクセスを使用して、Android アプリと iOS アプリを構築できます。 これらのアプリはプラットフォーム固有のハードウェア アクセラレーションを利用し、ネイティブ パフォーマンス用にコンパイルできます。

これらのアプリでは、iBeacons や Android フラグメントなどのプラットフォーム固有機能を含めて、基盤となるプラットフォームおよびデバイスによって公開される機能に全面的にアクセスできます。標準のネイティブ ユーザー インターフェイス コントロールを使用すると、ユーザーが期待する外観と操作感の UI を構築できます。

UWP の場合と同様、.NET Standard 2.0 クラス ライブラリに用意されているビジネス ロジックを再利用できるため、Android アプリまたは iOS アプリを追加するコストが低くなります。 UI ページは XAML で設計し、デバイス固有またはプラットフォーム固有のコードを記述する必要があります。

#### <a name="add-a-xamarin-project"></a>Xamarin プロジェクトを追加する

まず、**Android**、**iOS**、または**クロス プラットフォーム**のプロジェクトをソリューションに追加します。

これらのテンプレートは、 **[新しいプロジェクトの追加]** ダイアログ ボックスの **[Visual C#]** グループにあります。

![Xamarin アプリ](images/desktop-to-uwp/xamarin-projects.png)

>[!NOTE]
>クロスプラット フォーム プロジェクトは、プラットフォーム固有の機能がほとんどないアプリに最適です。 これらを使用して、iOS、Android、および Windows で実行されるネイティブの XAML ベース UI を 1 つ構築できます。 [こちら](https://docs.microsoft.com/xamarin/xamarin-forms/)をご覧ください。

次に、Android、iOS、またはクロスプラットフォーム プロジェクトから、クラス ライブラリ プロジェクトの参照を追加します。

![クラス ライブラリ参照](images/desktop-to-uwp/class-library-reference3.png)

#### <a name="build-your-pages"></a>ページをビルドする

この例では、Android アプリに顧客の一覧が示されます。

![Android アプリ](images/desktop-to-uwp/android-app.png)

```xml
<?xml version="1.0" encoding="utf-8"?>
<TextView xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:padding="10dp" android:textSize="16sp"
    android:id="@android:id/list">
</TextView>
```

```csharp
[Activity(Label = "MyAndroidApp", MainLauncher = true)]
public class MainActivity : ListActivity
{
    protected override void OnCreate(Bundle savedInstanceState)
    {
        base.OnCreate(savedInstanceState);

        SqlClientFactory factory = SqlClientFactory.Instance;

        var customers = (string[])Northwind.GetCustomerNames(factory).ToArray(typeof(string));

        ListAdapter = new ArrayAdapter<string>(this, Resource.Layout.list_item, customers);
    }
}
```

Android プロジェクト、iOS プロジェクト、およびクロスプラットフォーム プロジェクトの概要については、[Xamarin 開発者ポータル](https://docs.microsoft.com/xamarin)をご覧ください。

## <a name="next-steps"></a>次のステップ

**質問の回答を検索**

ご質問がある場合は、 Stack Overflow でお問い合わせください。 Microsoft のチームでは、これらの[タグ](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)をチェックしています。 [こちら](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)から質問することもできます。

**ご意見や機能を提案します。**

[UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial) のページをご覧ください。
