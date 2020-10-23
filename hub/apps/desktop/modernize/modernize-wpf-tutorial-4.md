---
description: このチュートリアルでは、アプリにアクティビティと通知の機能を追加する方法について説明します。
title: Windows 10 ユーザー アクティビティと通知の追加
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: windows 10、uwp、windows フォーム、wpf、xaml islands
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 3acc5638115932f6536eccb3be5e7222ef53fbb7
ms.sourcegitcommit: 0c4bbaf1c119a84002748cdcf02e1449835559c3
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/16/2020
ms.locfileid: "92133055"
---
# <a name="part-4-add-windows-10-user-activities-and-notifications"></a>パート 4: Windows 10 ユーザー アクティビティと通知の追加

これは、Contoso Expenses という名前のサンプル WPF デスクトップ アプリを現代化する方法を示すチュートリアルの 4 番目の部分です。 チュートリアルの概要、前提条件、サンプル アプリをダウンロードするための手順については、「[チュートリアル: WPF アプリの現代化](modernize-wpf-tutorial.md)」を参照してください。 この記事では、読者が[パート 3](modernize-wpf-tutorial-3.md) を既に完了していることを前提にしています。

このチュートリアルの前のパートでは、XAML Islands を使用して UWP XAML コントロールをアプリに追加しました。 この副次的効果として、アプリで任意の WinRT API を呼び出すことができるようにもなりました。 これにより、UWP XAML コントロールだけでなく、Windows 10 で提供される他の多くの機能を、アプリで使用できるようになります。

このチュートリアルの架空のシナリオの Contoso 開発チームは、2 つの新しい機能として、アクティビティと通知をアプリに追加することにしました。 チュートリアルのこのパートでは、これらの機能を実装する方法について説明します。

## <a name="add-a-user-activity"></a>ユーザー アクティビティを追加する

Windows 10 では、ファイルを開いたり、特定のページを表示するといった、ユーザーが実行したアクティビティを、アプリで追跡できます。 その後、これらのアクティビティは、Windows 10 バージョン 1803 で導入された機能であるタイムラインで利用でき、ユーザーは過去にすばやく戻って、以前に開始したアクティビティを再開できます。

![Windows タイムラインのイメージ](images/wpf-modernize-tutorial/WindowsTimeline.png)

ユーザー アクティビティは、[Microsoft Graph](https://developer.microsoft.com/graph/) を使用して追跡されます。 ただし、Windows 10 アプリを構築するときに、Microsoft Graph によって提供される REST エンドポイントを直接操作する必要はありません。 代わりに、WinRT API の便利なセットを使用できます。 Contoso Expenses アプリでこれらの WinRT API を使用して、ユーザーがアプリ内で経費を開くたびに追跡し、アダプティブ カードを使用してユーザーがアクティビティを作成できるようにします。

### <a name="introduction-to-adaptive-cards"></a>アダプティブ カードの概要

このセクションでは、[アダプティブ カード](/adaptive-cards/)の概要について簡単に説明します。 この情報が不要な場合は、このステップを省略して、[アダプティブ カードの追加](#add-an-adaptive-card)の説明にすぐ進んでもかまいません。

アダプティブ カードを使用すると、開発者は共通の一貫した方法でカードのコンテンツを交換することができます。 アダプティブ カードはコンテンツを定義する JSON ペイロードによって記述されており、テキスト、画像、アクション、その他を含むことができます。

アダプティブ カードで定義されているのはコンテンツだけであり、コンテンツの表示方法は定義されていません。 アダプティブ カードを受け取ったプラットフォームでは、最適なスタイルを使用してコンテンツを表示できます。 アダプティブ カードを設計するには、[レンダラー](/adaptive-cards/rendering-cards/getting-started)を使用します。レンダラーは、JSON ペイロードを受け取り、それをネイティブ UI に変換することができます。 たとえば、UI として、WPF アプリや UWP アプリでは XAML を、Android アプリでは AXML を、Web サイトやボット チャットでは HTML を使用できます。

簡単なアダプティブ カードのペイロードの例を次に示します。

```json
{
    "type": "AdaptiveCard",
    "body": [
        {
            "type": "Container",
            "items": [
                {
                    "type": "TextBlock",
                    "size": "Medium",
                    "weight": "Bolder",
                    "text": "Publish Adaptive Card schema"
                },
                {
                    "type": "ColumnSet",
                    "columns": [
                        {
                            "type": "Column",
                            "items": [
                                {
                                    "type": "Image",
                                    "style": "Person",
                                    "url": "https://pbs.twimg.com/profile_images/3647943215/d7f12830b3c17a5a9e4afcc370e3a37e_400x400.jpeg",
                                    "size": "Small"
                                }
                            ],
                            "width": "auto"
                        },
                        {
                            "type": "Column",
                            "items": [
                                {
                                    "type": "TextBlock",
                                    "weight": "Bolder",
                                    "text": "Matt Hidinger",
                                    "wrap": true
                                },
                                {
                                    "type": "TextBlock",
                                    "spacing": "None",
                                    "text": "Created {{DATE(2017-02-14T06:08:39Z,SHORT)}}",
                                    "isSubtle": true,
                                    "wrap": true
                                }
                            ],
                            "width": "stretch"
                        }
                    ]
                }
            ]
        }
    ],
    "actions": [
        {
            "type": "Action.ShowCard",
            "title": "Set due date",
            "card": {
                "type": "AdaptiveCard",
                "style": "emphasis",
                "body": [
                    {
                        "type": "Input.Date",
                        "id": "dueDate"
                    },
                    {
                        "type": "Input.Text",
                        "id": "comment",
                        "placeholder": "Add a comment",
                        "isMultiline": true
                    }
                ],
                "actions": [
                    {
                        "type": "Action.OpenUrl",
                        "title": "OK",
                        "url": "http://adaptivecards.io"
                    }
                ],
                "$schema": "http://adaptivecards.io/schemas/adaptive-card.json"
            }
        },
        {
            "type": "Action.OpenUrl",
            "title": "View",
            "url": "http://adaptivecards.io"
        }
    ],
    "$schema": "http://adaptivecards.io/schemas/adaptive-card.json",
    "version": "1.0"
}
```

次の図は、Teams チャネル、Cortana、Windows 通知により、この JSON がさまざまな方法でどのように表示されるかを示したものです。

![アダプティブ カードのレンダリングの画像](images/wpf-modernize-tutorial/AdaptiveCards.png)

アダプティブ カードは、Windows によってアクティビティがレンダリングされる方法であるため、タイムラインで重要な役割を果たします。 タイムライン内に表示される各サムネイルは、実際にはアダプティブ カードです。 そのため、アプリ内でユーザー アクティビティを作成するときに、それをレンダリングするためのアダプティブ カードを指定するように求められます。

> [!NOTE]
> アダプティブ カードの設計をブレーンストーミングするための優れた方法は、[オンライン デザイナー](https://adaptivecards.io/designer/)を使用することです。 構成要素 (画像、テキスト、列など) を使用してカードを設計し、対応する JSON を取得することができます。 最終的な設計が決まったら、[AdaptiveCards](https://www.nuget.org/packages/AdaptiveCards/) という名前のライブラリを使用し、デバッグやビルドが難しい場合がある、単純な JSON ではなく C# のクラスを使用してアダプティブ カードを簡単に作成できます。

### <a name="add-an-adaptive-card"></a>アダプティブ カードを追加する

1. ソリューション エクスプローラーで **ContosoExpenses.Core** プロジェクトを右クリックし、 **[NuGet パッケージの管理]** を選択します。

2. **[NuGet パッケージ マネージャー]** ウィンドウで、 **[参照]** をクリックします。 `Newtonsoft.Json` パッケージを探し、利用可能な最新バージョンをインストールします。 これは、アダプティブ カードで必要な JSON 文字列の操作に使用する一般的な JSON 操作ライブラリです。

    ![NewtonSoft.Json NuGet パッケージ](images/wpf-modernize-tutorial/JsonNetNuGet.png)

    > [!NOTE]
    > `Newtonsoft.Json` パッケージを個別にインストールしないと、AdaptiveCards ライブラリでは、.NET Core 3.0 がサポートされていない古いバージョンの `Newtonsoft.Json` パッケージが参照されます。

2. **[NuGet パッケージ マネージャー]** ウィンドウで、 **[参照]** をクリックします。 `AdaptiveCards` パッケージを探し、利用可能な最新バージョンをインストールします。

    ![AdaptiveCards NuGet パッケージ](images/wpf-modernize-tutorial/AdaptiveCardsNuGet.png)

3. **ソリューション エクスプローラー**で **ContosoExpenses.Core** プロジェクトを右クリックし、 **[追加] > [クラス]** の順に選択します。 クラスに **TimelineService.cs** という名前を指定し、 **[OK]** をクリックします。

4. **TimelineService.cs** ファイルで、次のステートメントをファイルの先頭に追加します。

    ```csharp
    using AdaptiveCards;
    using ContosoExpenses.Data.Models;
    ```

5. ファイルで宣言されている名前空間を `ContosoExpenses.Core` から `ContosoExpenses` に変更します。

5. 次のメソッドを `TimelineService` クラスに追加します。

   ```csharp
    private string BuildAdaptiveCard(Expense expense)
    {
        AdaptiveCard card = new AdaptiveCard("1.0");

        AdaptiveTextBlock title = new AdaptiveTextBlock
        {
            Text = expense.Description,
            Size = AdaptiveTextSize.Medium,
            Wrap = true
        };

        AdaptiveColumnSet columnSet = new AdaptiveColumnSet();
        AdaptiveColumn photoColumn = new AdaptiveColumn
        {
            Width = "auto"
        };

        AdaptiveImage image = new AdaptiveImage
        {
            Url = new Uri("https://appmodernizationworkshop.blob.core.windows.net/contosoexpenses/Contoso192x192.png"),
            Size = AdaptiveImageSize.Small,
            Style = AdaptiveImageStyle.Default
        };
        photoColumn.Items.Add(image);

        AdaptiveTextBlock amount = new AdaptiveTextBlock
        {
            Text = expense.Cost.ToString(),
            Weight = AdaptiveTextWeight.Bolder,
            Wrap = true
        };

        AdaptiveTextBlock date = new AdaptiveTextBlock
        {
            Text = expense.Date.Date.ToShortDateString(),
            IsSubtle = true,
            Spacing = AdaptiveSpacing.None,
            Wrap = true
        };

        AdaptiveColumn expenseColumn = new AdaptiveColumn
        {
            Width = "stretch"
        };
        expenseColumn.Items.Add(amount);
        expenseColumn.Items.Add(date);

        columnSet.Columns.Add(photoColumn);
        columnSet.Columns.Add(expenseColumn);

        card.Body.Add(title);
        card.Body.Add(columnSet);

        string json = card.ToJson();
        return json;
    }
    ```

#### <a name="about-the-code"></a>コードについて

このメソッドは、レンダリングする経費関連のすべての情報が含まれる **Expense** オブジェクトを受け取り、新しい **AdaptiveCard** オブジェクトを構築します。 このメソッドでは、次のものがカードに追加されます。

- 経費の説明を使用したタイトル。
- Contoso ロゴの画像。
- 経費の金額。
- 経費の日付。

最後の 3 つの要素は 2 つの異なる列に分かれているため、Contoso のロゴと経費に関する詳細情報を並べて配置できます。 オブジェクトが構築された後、メソッドからは、**ToJson** メソッドを利用して対応する JSON 文字列が返されます。

### <a name="define-the-user-activity"></a>ユーザー アクティビティを定義する

アダプティブ カードの定義が済んだので、それを基にしてユーザー アクティビティを作成できます。

1. 次のステートメントを **TimelineService.cs** ファイルの先頭に追加します。

    ```csharp
    using Windows.ApplicationModel.UserActivities;
    using System.Threading.Tasks;
    using Windows.UI.Shell;
    ```

    > [!NOTE]
    > これらは UWP 名前空間です。 これらは、ステップ 2 でインストールした `Microsoft.Toolkit.Wpf.UI.Controls` NuGet パッケージに `Microsoft.Windows.SDK.Contracts` パッケージへの参照が含まれているため解決され、.NET Core 3 プロジェクトであっても **ContosoExpenses.Core** プロジェクトで WinRT API を参照できるようになります。

2. 次のフィールド宣言を `TimelineService` クラスに追加します。

    ```csharp
    private UserActivityChannel _userActivityChannel;
    private UserActivity _userActivity;
    private UserActivitySession _userActivitySession;
    ```

3. 次のメソッドを `TimelineService` クラスに追加します。

    ```csharp
    public async Task AddToTimeline(Expense expense)
    {
        _userActivityChannel = UserActivityChannel.GetDefault();
        _userActivity = await _userActivityChannel.GetOrCreateUserActivityAsync($"Expense-{expense.ExpenseId}");

        _userActivity.ActivationUri = new Uri($"contosoexpenses://expense/{expense.ExpenseId}");
        _userActivity.VisualElements.DisplayText = "Contoso Expenses";

        string json = BuildAdaptiveCard(expense);

        _userActivity.VisualElements.Content = AdaptiveCardBuilder.CreateAdaptiveCardFromJson(json);

        await _userActivity.SaveAsync();
        _userActivitySession?.Dispose();
        _userActivitySession = _userActivity.CreateSession();
    }
    ```

4. 変更内容を **TimelineService.cs** に保存します。

#### <a name="about-the-code"></a>コードについて

`AddToTimeline` メソッドでは、最初に、ユーザー アクティビティを格納するために必要な **UserActivityChannel** オブジェクトが取得されます。 次に、**GetOrCreateUserActivityAsync** メソッドを使用して、新しいユーザー アクティビティが作成されます。これには、一意の識別子が必要です。 これにより、アクティビティが既に存在する場合は、アプリでそれを更新できます。そうでないと、新しいものが作成されます。 渡す識別子は、構築しているアプリケーションの種類によって異なります。

* タイムラインで最新のアクティビティのみが表示されるように、同じアクティビティを常に更新する場合は、固定の識別子 (**Expenses** など) を使用できます。
* タイムラインですべてのアクティビティが表示されるように、すべてのアクティビティを異なるものとして追跡する場合は、動的な識別子を使用できます。

このシナリオのアプリでは、開かれている各経費を異なるユーザー アクティビティとして追跡するので、コードでは、キーワード **Expense-** の後に一意の経費 ID を加えたものを使用して、各識別子を作成します。

メソッドでは、**UserActivity** オブジェクトが作成された後、次の情報がオブジェクトに設定されます。

* ユーザーがタイムラインでアクティビティをクリックしたときに呼び出される **ActivationUri**。 コードでは、後でアプリによって処理される **contosoexpenses** という名前のカスタム プロトコルが使用されます。
* **VisualElements** オブジェクト。これには、アクティビティの外観を定義する一連のプロパティが含まれています。 このコードでは、**DisplayText** (タイムラインのエントリの上部に表示されるタイトル) と **Content** が設定されます。 

ここで、前に定義したアダプティブ カードが役割を果たします。 アプリでは、前にコンテンツとして設計したアダプティブ カードがメソッドに渡されます。 ただし、Windows 10 では、`AdaptiveCards` NuGet パッケージによって使用されるものとは異なるオブジェクトを使用して、カードが表示されます。 そのため、メソッドでは、**AdaptiveCardBuilder** クラスによって公開されている **CreateAdaptiveCardFromJson** メソッドを使用してカードが再作成されます。 メソッドによってユーザー アクティビティが作成されて保存され、新しいセッションが作成されます。

ユーザーがタイムラインでアクティビティをクリックすると、**contosoexpenses://** プロトコルがアクティブになり、選択された経費をアプリで取得するために必要な情報が URL に挿入されます。 オプションのタスクとして、ユーザーがタイムラインを使用したときにアプリケーションが正しく動作するように、プロトコルのアクティブ化を実装することもできます。

### <a name="integrate-the-application-with-timeline"></a>アプリケーションをタイムラインと統合する

タイムラインと対話するクラスを作成したので、それを使用してアプリケーションのエクスペリエンスの拡張を始めることができます。 **TimelineService** クラスによって公開されている **AddToTimeline** メソッドを使用するのに最も適した場所は、ユーザーが経費の詳細ページを開いたときです。

1. **ContosoExpenses.Core** プロジェクトで、**ViewModels** フォルダーを展開して、**ExpenseDetailViewModel.cs** ファイルを開きます。 これは、経費詳細のウィンドウをサポートする ViewModel です。

2. **ExpenseDetailViewModel** クラスのパブリック コンストラクターを見つけ、そのコンストラクターの最後に次のコードを追加します。 経費ウィンドウが開かれるたびに、そのメソッドによって **AddToTimeline** メソッドが呼び出され、現在の経費が渡されます。 **TimelineService** クラスでは、この情報を使用して、経費情報を使用するユーザー アクティビティが作成されます。

    ```csharp
    TimelineService timeline = new TimelineService();
    timeline.AddToTimeline(expense);
    ```

    完了すると、コンストラクターは次のようになります。

    ```csharp
    public ExpensesDetailViewModel(IDatabaseService databaseService, IStorageService storageService)
    {
        var expense = databaseService.GetExpense(storageService.SelectedExpense);

        ExpenseType = expense.Type;
        Description = expense.Description;
        Location = expense.Address;
        Amount = expense.Cost;

        TimelineService timeline = new TimelineService();
        timeline.AddToTimeline(expense);
    }
    ```

3. デバッガーで F5 キーを押して、アプリをビルドして実行します。 一覧から従業員を選択し、経費を選択します。 詳細ページで、経費の説明、日付、金額を確認します。

4. [開始] を選択し、**Tab** キーを押して、タイムラインを開きます。

5. 現在開いているアプリケーションの一覧を下にスクロールして、 **[今日]** というセクションを表示します。 このセクションには、最新のユーザー アクティビティの一部が表示されます。 **[今日]** の隣にある **[See all activities]\(すべてのアクティビティを表示する\)** リンクをクリックします。

6. アプリケーションで選択した経費に関する情報が表示された新しいカードがあることを確認します。

    ![Contoso の経費のタイムライン](images/wpf-modernize-tutorial/ContosoExpensesTimeline.png)

7. 他の経費を開くと、ユーザー アクティビティとして追加された新しいカードが表示されます。 コードではアクティビティごとに異なる識別子が使用されているので、アプリで経費を開くたびにカードが作成されることに注意してください。

8. アプリを閉じます。

## <a name="add-a-notification"></a>通知を追加する

Contoso 開発チームが追加したい 2 番目の機能は、新しい経費がデータベースに保存されるたびにユーザーに表示される通知です。 これを行うには、Windows 10 に組み込まれている通知システムを利用します。これは、WinRT API によって開発者に公開されます。 この通知システムには多くの利点があります。

- OS の他の部分と一貫した通知になります。
- アクションを実行できます。
- アクション センターに保存されるため、後で確認できます。

通知をアプリに追加するには:

1. **ソリューション エクスプローラー**で **ContosoExpenses.Core** プロジェクトを右クリックし、 **[追加] > [クラス]** の順に選択します。 クラスに **NotificationService.cs** という名前を指定し、 **[OK]** をクリックします。

2. **NotificationService.cs** ファイルで、次のステートメントをファイルの先頭に追加します。

    ```csharp
    using Windows.Data.Xml.Dom;
    using Windows.UI.Notifications;
    ```

3. ファイルで宣言されている名前空間を `ContosoExpenses.Core` から `ContosoExpenses` に変更します。

4. 次のメソッドを `NotificationService` クラスに追加します。

    ```csharp
    public void ShowNotification(string description, double amount)
    {
        string xml = $@"<toast>
                          <visual>
                            <binding template='ToastGeneric'>
                              <text>Expense added</text>
                              <text>Description: {description} - Amount: {amount} </text>
                            </binding>
                          </visual>
                        </toast>";

        XmlDocument doc = new XmlDocument();
        doc.LoadXml(xml);

        ToastNotification toast = new ToastNotification(doc);
        ToastNotificationManager.CreateToastNotifier().Show(toast);
    }
    ```

    トースト通知は XML ペイロードによって表され、テキスト、画像、アクションなどを含むことができます。 サポートされているすべての要素は、[こちら](/windows/uwp/design/shell/tiles-and-notifications/toast-schema)で確認できます。 このコードでは、テキストがタイトルと本文の 2 行の非常にシンプルなスキーマを使用します。 コードでは、XML ペイロードが定義されて **XmlDocument** オブジェクトに読み込まれた後、XML が **ToastNotification** オブジェクト内にラップされ、**ToastNotificationManager** クラスを使用して表示されます。

5. **ContosoExpenses.Core** プロジェクトで、**ViewModels** フォルダーを展開して、**AddNewExpenseViewModel.cs** ファイルを開きます。 

6. `SaveExpenseCommand` メソッドを見つけます。このメソッドは、ユーザーがボタンをクリックして新しい経費を保存したときにトリガーされます。 次のコードを、このメソッドで `SaveExpense` メソッドを呼び出している直後に追加します。

    ```csharp
    NotificationService notificationService = new NotificationService();
    notificationService.ShowNotification(expense.Description, expense.Cost);
    ```

    完了すると、`SaveExpenseCommand` メソッドは次のようになります。

    ```csharp
    private RelayCommand _saveExpenseCommand;
    public RelayCommand SaveExpenseCommand
    {
        get
        {
            if (_saveExpenseCommand == null)
            {
                _saveExpenseCommand = new RelayCommand(() =>
                {
                    Expense expense = new Expense
                    {
                        Address = Address,
                        City = City,
                        Cost = Cost,
                        Date = Date,
                        Description = Description,
                        EmployeeId = storageService.SelectedEmployeeId,
                        Type = ExpenseType
                    };

                    databaseService.SaveExpense(expense);

                    NotificationService notificationService = new NotificationService();
                    notificationService.ShowNotification(expense.Description, expense.Cost);

                    Messenger.Default.Send<UpdateExpensesListMessage>(new UpdateExpensesListMessage());
                    Messenger.Default.Send<CloseWindowMessage>(new CloseWindowMessage());
                }, () => IsFormFilled
                );
            }

            return _saveExpenseCommand;
        }
    }
    ```

7. デバッガーで F5 キーを押して、アプリをビルドして実行します。 一覧から従業員を選択して、 **[Add new expense]\(新しい経費の追加\)** ボタンをクリックします。 フォームのすべてのフィールドを入力し、 **[Save]\(保存\)** をクリックします。

8. 次の例外が表示されます。

    ![トースト通知エラー](images/wpf-modernize-tutorial/ToastNotificationError.png)

この例外は、Contoso Expenses アプリにパッケージ ID がまだないことが原因で発生します。 Notifications API を含む一部の WinRT API では、アプリで API を使用するにはパッケージ ID が事前に必要です。 UWP アプリは、MSIX パッケージ経由でのみ配布できるため、既定でパッケージ ID を受け取ります。 WPF アプリを含む他の種類の Windows アプリは、パッケージ ID を取得するために、MSIX パッケージを使用して配置することもできます。 このチュートリアルの次のパートでは、その方法について説明します。

## <a name="next-steps"></a>次の手順

このチュートリアルでは、ここまでで、Windows タイムラインと統合するアプリにユーザー アクティビティを追加し、ユーザーが新しい経費を作成するとトリガーされる通知をアプリに追加しました。 ただし、アプリで Notifications API を使用するにはパッケージ ID が必要であるため、通知はまだ機能していません。 MSIX パッケージを構築し、アプリでパッケージ ID を取得したり他の配置の利点を利用したりする方法については、「[パート 5: MSIX によるパッケージ化と配置](modernize-wpf-tutorial-5.md)」を参照してください。