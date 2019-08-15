---
description: このチュートリアルでは、UWP XAML ユーザー インターフェイスを追加、MSIX パッケージを作成およびその他の最新コンポーネントを WPF アプリに組み込む方法を示します。
title: Windows 10 ユーザーのアクティビティと通知を追加します。
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: windows 10、uwp、windows フォーム、wpf、xaml islands
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 8443ac25ba678986046b967a90a8899eaffb76aa
ms.sourcegitcommit: 1eec0e4fd8a5ba82803fdce6e23fcd01b9488523
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/27/2019
ms.locfileid: "67420121"
---
# <a name="part-4-add-windows-10-user-activities-and-notifications"></a>パート 4:Windows 10 ユーザーのアクティビティと通知を追加します。

これは、Contoso の経費をという名前のサンプル WPF デスクトップ アプリの近代化する方法を説明するチュートリアルの第 4 部です。 チュートリアル、前提条件、およびサンプル アプリをダウンロードする手順の概要については、次を参照してください。[チュートリアル。WPF アプリの近代化](modernize-wpf-tutorial.md)します。 この記事では、既に完了している前提としています。[パート 3](modernize-wpf-tutorial-3.md)します。

このチュートリアルの前のパートで UWP XAML コントロールを XAML Islands を使用してアプリを追加しました。 として、この製品は、有効にしても、アプリをあらゆる WinRT API を呼び出します。 これは、UWP XAML コントロールだけでなく、Windows 10 で提供されるその他の多くの機能を使用するアプリの営業案件を開きます。

このチュートリアルのシナリオでは、架空の contoso 社の開発チームをアプリに 2 つの新しい機能を追加する決定が: アクティビティと通知します。 このチュートリアルのこの部分は、これらの機能を実装する方法を示します。

## <a name="add-a-user-activity"></a>ユーザー アクティビティを追加します。

Windows 10 では、アプリがファイルを開くか、特定のページを表示するなど、ユーザーによって実行されるアクティビティを追跡できます。 これらのアクティビティはタイムライン、Windows 10 バージョン 1803 で導入された機能を迅速に、過去に戻るし、以前に開始したアクティビティを再開できますを通じて提供されます。

![Windows のタイムラインの画像](images/wpf-modernize-tutorial/WindowsTimeline.png)

使用してユーザーのアクティビティが追跡される[Microsoft Graph](https://developer.microsoft.com/graph/)します。 ただし、Windows 10 アプリを構築する場合は、Microsoft graph REST のエンドポイントと直接やり取りする必要はありません。 代わりに、便利な一連の WinRT Api を使用することができます。 ユーザーが、アプリ内の経費を開くたびに追跡するために、Contoso 経費アプリケーションでこれらの WinRT Api を使用して Adaptive Cards を使用して、アクティビティを作成するユーザーを有効になります。

### <a name="introduction-to-adaptive-cards"></a>Adaptive Cards の概要

このセクションの簡単な概要を提供する[Adaptive Cards](https://docs.microsoft.com/adaptive-cards/)します。 この情報を必要としない場合は、スキップして右に移動して、[アダプティブ カードを追加](#add-an-adaptive-card)指示します。

Adaptive Cards は、一貫性のある一般的な方法でカードのコンテンツを交換する開発者を有効にします。 アダプティブのカードは、そのコンテンツは、テキスト、イメージ、アクション、および含めることができますを定義する JSON ペイロードで表されます。

アダプティブのカードは、コンテンツだけとコンテンツのない外観を定義します。 アダプティブのカードを受信する場合、プラットフォームには、最も適切なスタイルを使用してコンテンツをレンダリングできます。 Adaptive Cards を設計する方法は、使用[レンダラー](https://docs.microsoft.com/adaptive-cards/rendering-cards/getting-started)、これは、JSON ペイロードを実行して、ネイティブ UI に変換することです。 たとえば、UI では、WPF や UWP アプリでは、AXML for Android アプリの場合、または HTML web サイトまたはチャット ボットの XAML 可能性があります。

単純なアダプティブ カード ペイロードの例を次に示します。

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

次の図は、この JSON を表示する方法によってさまざまな方法で ta Teams のチャネル、Cortana と Windows 通知します。

![アダプティブ カード レンダリング イメージ](images/wpf-modernize-tutorial/AdaptiveCards.png)

アダプティブ カードでは、Windows アクティビティの表示方法のために、タイムラインで重要な役割を果たします。 タイムライン内に表示される各サムネイルは、実際にアダプティブ カードです。 そのため、アプリ内のユーザー アクティビティを作成しようとしている、するように求められますがアダプティブのカードのレンダリングを提供します。

> [!NOTE]
> アダプティブのカードのデザインでのブレーンストーミングする優れた方法を使用して[オンライン デザイナー](https://adaptivecards.io/designer/)します。 (イメージ、テキスト、列など) のビルディング ブロックでカードを設計し、対応する JSON を取得する可能性があります。 最終的な設計のアイデアがある場合は後、と呼ばれるライブラリを使用することができます[Adaptive Cards](https://www.nuget.org/packages/AdaptiveCards/)アダプティブ カードを使用して、ビルドを容易にできるようにC#プレーンな JSON は、デバッグおよびビルドが困難ではなくクラス。

### <a name="add-an-adaptive-card"></a>アダプティブのカードを追加します。

1. 右クリックして、 **ContosoExpenses.Core**ソリューション エクスプ ローラーでプロジェクト**NuGet パッケージの管理**します。

2. **NuGet パッケージ マネージャー**ウィンドウで、をクリックして**参照**します。 検索、`Newtonsoft.Json`パッケージ化し、最新のバージョンをインストールします。 これは一般的な JSON 操作ライブラリ mainipulate アダプティブ カードが必要な JSON 文字列のために使用されます。

    ![NewtonSoft.Json NuGet パッケージ](images/wpf-modernize-tutorial/JsonNetNuGet.png)

    > [!NOTE]
    > インストールしていない場合、`Newtonsoft.Json`とは別にパッケージ化、The Adaptive Cards ライブラリは、古いバージョンの参照、`Newtonsoft.Json`パッケージを .NET Core 3.0 をサポートしていません。

2. **NuGet パッケージ マネージャー**ウィンドウで、をクリックして**参照**します。 検索、`AdaptiveCards`パッケージ化し、最新のバージョンをインストールします。

    ![アダプティブのカードの NuGet パッケージ](images/wpf-modernize-tutorial/AdaptiveCardsNuGet.png)

3. **ソリューション エクスプ ローラー**を右クリックし、 **ContosoExpenses.Core**プロジェクトで、選択**追加]、[クラス**します。 クラスの名前**TimelineService.cs**  をクリック**OK**します。

4. **TimelineService.cs**ファイルで、ファイルの先頭に次のステートメントを追加します。

    ```csharp
    using AdaptiveCards;
    using ContosoExpenses.Data.Models;
    ```

5. ファイルの名前空間が宣言されている変更`ContosoExpenses.Core`に`ContosoExpenses`します。

5. 次のメソッドを追加、`TimelineService`クラス。

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

このメソッドは受信、**経費**をレンダリングする経費とそれに関するすべての情報を持つオブジェクトは新しい**AdaptiveCard**オブジェクト。 メソッドは、カードに、次を追加します。

- 経費の説明を使用するタイトル。
- これは、Contoso ロゴ イメージです。
- 経費の量。
- 経費の日付。

最後の 3 つの要素は、Contoso ロゴおよび経費の詳細を並行して配置することができるように、2 つの異なる列に分割されます。 ヘルプで、対応する JSON 文字列を返します、オブジェクトが構築された後、 **ToJson**メソッド。

### <a name="define-the-user-activity"></a>ユーザー アクティビティを定義します。

アダプティブのカードを定義するには、それに基づくユーザー アクティビティを作成できます。

1. 先頭に次のステートメントを追加**TimelineService.cs**ファイル。

    ```csharp
    using Windows.ApplicationModel.UserActivities;
    using System.Threading.Tasks;
    using Windows.UI.Shell;
    ```

    > [!NOTE]
    > これらは、UWP の名前空間です。 これらを解決するため、`Microsoft.Toolkit.Wpf.UI.Controls`手順 2. でインストールされている NuGet パッケージにはへの参照が含まれています、`Microsoft.Windows.SDK.Contracts`をパッケージ化できる、 **ContosoExpenses.Core** .NET は、その場合でも、WinRT Api を参照するプロジェクトCore 3 のプロジェクトです。

2. 次のフィールド宣言を追加、`TimelineService`クラス。

    ```csharp
    private UserActivityChannel _userActivityChannel;
    private UserActivity _userActivity;
    private UserActivitySession _userActivitySession;
    ```

3. 次のメソッドを追加、`TimelineService`クラス。

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

4. 変更を保存**TimelineService.cs**します。

#### <a name="about-the-code"></a>コードについて

`AddToTimeline`メソッドはまず、取得、 **UserActivityChannel**ユーザー アクティビティを格納するために必要なオブジェクト。 新しいユーザー アクティビティを使用して、作成し、 **GetOrCreateUserActivityAsync**メソッドで、一意の識別子が必要です。 これにより、アクティビティが既に存在する場合、アプリを更新できます。それ以外の場合、新しくを作成されます。 識別子を渡すには、構築しているアプリケーションの種類によって異なります。

* タイムラインは最新のものだけが表示されるように、同じアクティビティを常に更新する場合は、固定の識別子を使用することができます (など**経費**)。
* として、別のすべてのアクティビティを追跡するためにするタイムラインがそれらのすべてを表示するようにする場合は、動的な識別子を使用することができます。

このシナリオで、アプリは開かれている各経費として追跡、別のユーザー アクティビティ コード、キーワードを使用して各識別子を作成するため**経費-** 後に一意の経費 id。

メソッドを作成した後、 **UserActivity**オブジェクトに、次の情報を持つオブジェクトが設定されます。

* **ActivationUri**アクティビティ タイムラインでは、ユーザーがクリックしたときに呼び出されます。 コードと呼ばれるカスタム プロトコルを使用して**contosoexpenses**後で、アプリを処理します。
* **VisualElements**アクティビティの視覚的な外観を定義するプロパティのセットを含むオブジェクトです。 このコードを設定、 **DisplayText** (タイムライン内のエントリ上に表示されるタイトルです)、**コンテンツ**します。 

これは、前に定義したアダプティブ カードが役割を果たします。 アプリのデザインを終えたら、アダプティブ カード以前コンテンツとしてメソッドを渡します。 しかし、Windows 10 が別のオブジェクトを使用して、によって使用されるものと比較してカードを表す、 `AdaptiveCards` NuGet パッケージ。 そのため、メソッドを使用して、カードを再作成されます、 **CreateAdaptiveCardFromJson**によって公開されるメソッド、 **AdaptiveCardBuilder**クラス。 メソッドでは、ユーザー アクティビティが作成された後、アクティビティを保存し、新しいセッションを作成します。

タイムラインでのアクティビティ、ユーザーがクリックしたときに、 **contosoexpenses://** プロトコルが有効にし、URL には、選択した経費明細を取得するアプリに必要な情報にが含まれます。 オプションのタスクとして、ユーザーがタイムラインを使用する場合、アプリケーションが正しくが反応するためのプロトコルのアクティブ化を実装できます。

### <a name="integrate-the-application-with-timeline"></a>タイムラインと、アプリケーションを統合します。

タイムラインと対話するクラスを作成したら、これをアプリケーションのエクスペリエンスを強化するために使用開始できます。 使用する最適な場所、 **AddToTimeline**によって公開されるメソッド、 **TimelineService**クラスは、ユーザーの経費の詳細ページを開いた場合。

1. **ContosoExpenses.Core**プロジェクトで、展開、 **ViewModels**フォルダーとオープン、 **ExpenseDetailViewModel.cs**ファイル。 これは、コスト detail のウィンドウをサポートするビューモデルです。

2. パブリック コンス トラクターを検索、 **ExpenseDetailViewModel**クラスし、コンス トラクターの末尾に次のコードを追加します。 メソッドを呼び出して、経費ウィンドウが開かれるたびに、 **AddToTimeline**メソッドを現在の経費を渡します。 **TimelineService**クラスでは、この情報を使用して、経費の情報を使用して、ユーザー アクティビティを作成します。

    ```csharp
    TimelineService timeline = new TimelineService();
    timeline.AddToTimeline(expense);
    ```

    完了したら、コンス トラクターは次のようになります。

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

3. F5 キーを押してビルドして、デバッガーでアプリを実行します。 一覧から、従業員を選択し、経費をします。 詳細 ページで、経費、日付、および量の説明に注意してください。

4. キーを押して**開始 + タブ**タイムラインを開きます。

5. 」のセクションが表示されるまで、現在開いているアプリケーションの一覧をスクロールして**本日**します。 このセクションでは、最新のユーザー アクティビティの一部を示します。 をクリックして、**すべてのアクティビティを参照してください。** リンクの横に、**本日**見出し。

6. アプリケーションで選択した経費に関する情報を新しいカードが表示されることを確認します。

    ![Contoso 経費のタイムライン](images/wpf-modernize-tutorial/ContosoExpensesTimeline.png)

7. その他の費用を開いた場合は、ユーザーのアクティビティとして追加される新しいカードが表示されます。 アクティビティごとに異なる id を使用して、アプリで開く経費ごとに、カードを作成することに注意してください。

8. アプリを閉じます。

## <a name="add-a-notification"></a>通知を追加します。

Contoso 社の開発チームが、追加しようとしています。 2 番目の機能は、新しい費用は、データベースに保存されるたびに、ユーザーに表示される通知です。 これを行うには、WinRT Api を使用して開発者に公開される Windows 10 では、組み込みの通知システムを利用できます。 この通知システムには、多くの利点があります。

- 通知は、OS の残りの部分と一致します。
- アクションにつながるです。
- アクション センターに格納するため、後で確認できます。

アプリに通知を追加します。

1. **ソリューション エクスプ ローラー**を右クリックし、 **ContosoExpenses.Core**プロジェクトで、選択**追加]、[クラス**します。 クラスの名前**NotificationService.cs**  をクリック**OK**します。

2. **NotificationService.cs**ファイルで、ファイルの先頭に次のステートメントを追加します。

    ```csharp
    using Windows.Data.Xml.Dom;
    using Windows.UI.Notifications;
    ```

3. ファイルの名前空間が宣言されている変更`ContosoExpenses.Core`に`ContosoExpenses`します。

4. 次のメソッドを追加、`NotificationService`クラス。

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

    トースト通知は、テキスト、イメージ、アクション、および含めることができる XML ペイロードで表されます。 サポートされているすべての要素を検索する[ここ](https://docs.microsoft.com/windows/uwp/design/shell/tiles-and-notifications/toast-schema)します。 このコードでは、2 つの行のテキストの非常に単純なスキーマを使用します。 タイトルと本文。 XML ペイロードを定義してでを読み込むと、コード、 **XmlDocument**オブジェクト内の XML をラップして、 **ToastNotification**オブジェクトし、それを使用して説明を**ToastNotificationManager**クラス。

5. **ContosoExpenses.Core**プロジェクトで、展開、 **ViewModels**フォルダーとオープン、 **AddNewExpenseViewModel.cs**ファイル。 

6. 検索、`SaveExpenseCommand`メソッドで、ユーザーが新しい経費を保存する ボタンを押したときに発生します。 呼び出しの直後後にこのメソッドは、次のコードを追加、`SaveExpense`メソッド。

    ```csharp
    NotificationService notificationService = new NotificationService();
    notificationService.ShowNotification(expense.Description, expense.Cost);
    ```

    済んだら、`SaveExpenseCommand`メソッドは、次のようになります。

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

7. F5 キーを押してビルドして、デバッガーでアプリを実行します。 一覧から、従業員を選択し、クリックして、**新しい経費の追加**ボタンをクリックします。 キーを押して、フォームのすべてのフィールドを完了**保存**します。

8. 次の例外が表示されます。

    ![トースト通知エラー](images/wpf-modernize-tutorial/ToastNotificationError.png)

この例外は、Contoso 経費アプリケーションは、パッケージ id にまだがないという事実は発生します。 通知 API など、一部の WinRT Api では、アプリでの使用にパッケージ id が必要です。 UWP アプリでは、MSIX パッケージ経由でのみ配布するために、既定ではパッケージ id を受け取ります。 パッケージ id を取得する MSIX パッケージを使用して他の種類の WPF アプリを含む、Windows アプリを展開することもできます。 このチュートリアルの次の部分では、これを行う方法を説明します。

## <a name="next-steps"></a>次のステップ

この時点で、チュートリアルでは正常に追加したユーザー アクティビティ、Windows のタイムラインと連携するアプリと、ユーザーが新しい経費を作成するときにトリガーされるアプリに通知も追加します。 ただし、アプリが通知 API を使用するパッケージ id が必要なため、通知は機能まだしません。 アプリをパッケージ id を取得し、その他の展開の利点、MSIX パッケージを作成する方法については、次を参照してください。[パート 5。パッケージ化し、デプロイを MSIX](modernize-wpf-tutorial-5.md)します。
