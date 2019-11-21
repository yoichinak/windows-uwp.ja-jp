---
Description: Learn how to send notifications from Partner Center to your app to encourage groups of customers to take an action, such as rating an app or buying an add-on.
title: アプリの顧客にターゲット プッシュ通知を送信する
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, ターゲット通知, プッシュ通知, トースト, タイル
ms.assetid: 16386c81-702d-47cd-9f91-67659f5dca73
ms.localizationpriority: medium
ms.openlocfilehash: 8ac9d464588cada11cd9d41cf0eb7858f593a0f2
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259936"
---
# <a name="send-notifications-to-your-apps-customers"></a>アプリのユーザーに通知を送信する

アプリ開発者として成功するには、適切なメッセージを適切なタイミングで送信して顧客を惹きつけることが重要です。 通知を使用すると、アプリの評価、アドオンの購入、新機能の試用、別のアプリのダウンロード (場合によっては、[プロモーション コード](generate-promotional-codes.md)の提供による無料ダウンロード) などの実行をユーザーに促すことができます。

[Partner Center](https://partner.microsoft.com/dashboard) provides a data-driven customer engagement platform you can use to send notifications to all of your app's customers, or only targeted to a subset of your app's Windows 10 customers who meet the criteria you’ve defined in a [customer segment](create-customer-segments.md). You can also create a notification to be sent to customers of more than one of your apps.

> [!IMPORTANT]
> これらの通知は、UWP アプリでのみ使用できます。

通知のコンテンツを検討するときは、次の点に注意してください。
- 通知のコンテンツは、ストア [コンテンツ ポリシー](https://docs.microsoft.com/legal/windows/agreements/store-policies#content_policies)に準拠する必要があります。
- 通知のコンテンツには、機密情報や機密性が高い可能性のある情報を含めないでください。
- スケジュールに従って通知を配信するよう努めていますが、配信に影響する待ち時間の問題が発生する可能性があります。
- 通知を送信する頻度を必要以上に高くしないでください。 30 分に 1 回以上の場合、侵入と見なされる可能性があります (多くのシナリオでは、これよりも頻度を低くすることをお勧めします)。
- アプリを使用しているユーザーが (セグメント メンバーシップの決定時に自分の Microsoft アカウントでサインインしており)、後でそのデバイスを他のユーザーに使用させた場合、他のユーザーが元のユーザーを対象とした通知を見る可能性があります。 詳しくは、「[アプリのターゲット プッシュ通知を構成する](../monetize/configure-your-app-to-receive-dev-center-notifications.md#notification-customers)」をご覧ください。
- 複数のアプリのユーザーに同じの通知を送信する場合、セグメントをターゲットすることはできません。通知は、選択したアプリのすべてのユーザーに送信されます。


## <a name="getting-started-with-notifications"></a>通知の使い方の概要

通知を使用してユーザーと関わりあうには、次の 3 ステップを実行する必要があります。

1. **Register your app to receive push notifications.** You do this by adding a reference to the Microsoft Store Services SDK in your app and then adding a few lines of code that registers a notification channel between Partner Center and your app. そのチャネルを使用して、通知をユーザーに配信します。 詳しくは、「[ターゲット プッシュ通知用のアプリの構成](../monetize/configure-your-app-to-receive-dev-center-notifications.md)」をご覧ください。
2. **Decide which customers to target.** すべてのアプリ ユーザーに通知を送信することも、(単一のアプリ用に作成した通知を) *セグメント*と呼ばれるユーザー グループに送信することもできます。セグメントは、人口統計や収入の条件に基づいて定義できます。 詳しくは、「[顧客セグメントの作成](create-customer-segments.md)」をご覧ください。
3. **通知のコンテンツを作成して送信します。** たとえば、新しいユーザーにアプリの評価を促す通知の作成や、アドオンの購入を促進する特別サービスを含む通知の送信などが考えられます。


## <a name="to-create-and-send-a-notification"></a>通知を作成して送信するには

Follow these steps to create a notification in Partner Center and send it to a particular customer segment.

> [!NOTE]
> Before an app can receive notifications from Partner Center, you must first call the [RegisterNotificationChannelAsync](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.registernotificationchannelasync) method in your app to register your app to receive notifications. このメソッドは、[Microsoft Store Services SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftStoreServicesSDK) に含まれています。 このメソッドを呼び出す方法の詳細 (コード例を含む) については、「[ターゲット プッシュ通知用のアプリの構成](../monetize/configure-your-app-to-receive-dev-center-notifications.md)」をご覧ください。

1. In [Partner Center](https://partner.microsoft.com/dashboard), expand the **Engage** section, and then select **Notifications**.
2. **[通知]** ページで、 **[新しい通知]** を選びます。
3. In the **Select a template** section, choose the [type of notification](#notification-template-types) you want to send and then click **OK**.
4. 次のページで、ドロップダウン メニューを使って通知を生成する **1 つのアプリ**または**複数のアプリ**を選びます。 You can only select apps that have been [configured to receive notifications using the Microsoft Store Services SDK](../monetize/configure-your-app-to-receive-dev-center-notifications.md).
5. **[通知設定]** セクションで、通知の **[名前]** を選択し、該当する場合は、通知の送信先として **[顧客グループ]** を選択します。 (複数のアプリに送信される通知は、アプリのすべてのユーザーにのみ送信できます)。まだ作成していないセグメントを使う場合、 **[新しい顧客グループの作成]** を選びます。 通知対象の新しいセグメントを使用できるまでに 24 時間かかることに注意してください。 詳しくは、「[顧客セグメントの作成](create-customer-segments.md)」をご覧ください。
6. 通知を送信するタイミングを指定する場合は、 **[すぐに通知する]** チェック ボックスをオフにして、特定の日付と時刻を選択します (各ユーザーのローカル タイム ゾーンを使用するように指定しない限り、すべてのユーザーに対して UTC が使用されます)。
7. ある時点で通知を期限切れにする場合は、 **[通知を無期限にする]** チェック ボックスをオフにして、特定の有効期限の日付と時刻 (UTC) を選択します。
8. **単一のアプリへの通知の場合:** 受信者にフィルターを適用して、特定の言語を使うユーザーや特定のタイム ゾーンのユーザーにのみ通知が配信されるようにするには、 **[Use filters]** (フィルターを使う) チェック ボックスをオンにします。 その後、使う言語とタイム ゾーンのオプションを指定できます。
8. **複数のアプリへの通知の場合:** 各デバイス (ユーザーごと) の最後のアクティブ アプリにのみ通知を送信するか、各デバイスのすべてのアプリに通知を送信するかを指定します。
10. **[通知のコンテンツ]** セクションの **[言語]** メニューで、通知に表示する言語を選択します。 詳しくは、「[通知の翻訳](#translate-your-notifications)」をご覧ください。
11. **[オプション]** セクションで、テキストを入力し、その他の必要なオプションを構成します。 テンプレートを使用して開始した場合は、既定でこの一部が提供されますが、必要な変更を加えることができます。

    使用している通知の種類に応じて、利用可能なオプションが異なります。 オプションの一部を次に示します:

    * **アクティブ化の種類** (対話型トースト型)。 **[フォアグラウンド]** 、 **[バックグラウンド]** 、または **[プロトコル]** を選択できます。
    * **起動** (対話型トースト型)。 通知がアプリを開くか、Web サイトを開くかを選択できます。
    * **アプリの起動率の追跡** (対話型トースト型)。 各通知を通じて、効率的に集客しているかを測定する場合は、このチェック ボックスを選択します。 詳しくは、「[通知のパフォーマンスの測定](#measure-notification-performance)」をご覧ください。
    * **期間** (対話型トースト型)。 **[短い]** または **[長い]** を選択できます。
    * **シナリオ** (対話型トースト型)。 **[既定]** 、 **[アラーム]** 、 **[リマインダー]** 、または **[着信]** を選択できます。
    * **ベース URI** (対話型トースト型)。 詳しくは、「[BaseUri](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.baseuri#Windows_UI_Xaml_FrameworkElement_BaseUri)」をご覧ください。
    * **イメージ クエリの追加** (対話型トースト型)。 詳しくは、「[addImageQuery](https://docs.microsoft.com/uwp/schemas/tiles/toastschema/element-visual#attributes-and-elements)」をご覧ください。
    * **ビジュアル** 。 イメージ、ビデオ、またはサウンド。 詳しくは、「[ビジュアル](https://docs.microsoft.com/uwp/schemas/tiles/toastschema/element-visual)」をご覧ください。
    * **入力**/**アクション**/**選択** (対話型トースト型)。 ユーザーが通知と対話できるようにします。 詳しくは、「[アダプティブ トースト通知と対話型トースト通知](../design/shell/tiles-and-notifications/adaptive-interactive-toasts.md)」をご覧ください。
    * **バインド** (対話型タイル型)。 トースト テンプレート。 詳しくは、「[バインド](https://docs.microsoft.com/uwp/schemas/tiles/toastschema/element-binding)」をご覧ください。

    > [!TIP]
    > アダプティブ タイル通知と対話型トースト通知を設計してテストするには、[Notifications Visualizer](https://www.microsoft.com/store/apps/9nblggh5xsl1) アプリを試してみてください。

12. 後で通知の作業を続ける場合は **[下書きとして保存]** を、すべて完了している場合は **[送信]** を選択します。


## <a name="notification-template-types"></a>通知テンプレートの種類

さまざまな通知テンプレートから選択できます。

-   **Blank (Toast).** カスタマイズできる空のトースト通知から始めます。 トースト通知は、顧客が別のアプリ内、スタート画面、またはデスクトップ上にいるときに、アプリが顧客と対話できるようにするために画面に表示されるポップアップ UI です。
-   **Blank (Tile).** カスタマイズできる空のタイル通知から始めます。 タイルは、スタート画面でのアプリの表示です。 タイルは "ライブ" にすることができます。つまり、タイルに表示されるコンテンツは、通知に応じて変化させることができます。
-   **Ask for ratings (Toast).** 顧客にアプリの評価を求めるトースト通知。 顧客が通知を選択すると、アプリのストア評価ページが表示されます。
-   **Ask for feedback (Toast).** 顧客にアプリのフィードバックを送信するよう求めるトースト通知。 顧客が通知を選択すると、アプリのフィードバック Hub ページが表示されます。
    > [!NOTE]
    > このテンプレートの種類を選択した場合は、 **[起動]** ボックスで、必ず {PACKAGE_FAMILY_NAME} プレースホルダーの値をアプリの実際のパッケージ ファミリ名 (PFN) に置き換えてください。 アプリの PFN は、[アプリ ID](view-app-identity-details.md) ページ ( **[アプリ管理]**  >  **[アプリ ID]** ) で確認できます。

    ![フィードバックのトースト起動ボックス](images/push-notifications-feedback-toast-launch-box.png)

-   **Cross-promote (Toast).** 任意の別のアプリを販売促進するトースト通知。 顧客が通知を選択すると、他のアプリのストア登録情報が表示されます。
    > [!NOTE]
    > このテンプレートの種類を選択した場合は、 **[起動]** ボックスで、必ず **{ProductId you want to promote here}** プレースホルダーの値を相互に販売促進するアイテムの実際のストア ID に置き換えてください。 ストア ID は、[アプリ ID](view-app-identity-details.md) ページ ( **[アプリ管理]**  >  **[アプリ ID]** ) で確認できます。

    ![クロス プロモーションのトースト起動ボックス](images/push-notifications-promote-toast-launch-box.png)

-   **Promote a sale (Toast).** アプリのクーポンの通知に使用できるトースト通知。 顧客が通知を選択すると、アプリのストア登録情報が表示されます。
-   **Prompt for update (Toast).** 古いバージョンのアプリを実行している顧客に最新バージョンをインストールするよう促すトースト通知。 ユーザーが通知を選択すると、Microsoft Store アプリが起動し、 **[ダウンロードと更新]** 一覧が表示されます。 このテンプレートは単一のアプリのみで使用でき、特定のセグメントを対象にしたり、送信日時を指定することはできません。この通知は常に 24 時間以内にスケジュールされ、アプリの最新バージョンをまだ実行していないすべてのユーザーが可能な限り対象になるように送信されます。


## <a name="measure-notification-performance"></a>通知のパフォーマンスの測定

各通知を通じて、効率的に集客しているかを測定できます。


### <a name="to-measure-notification-performance"></a>通知のパフォーマンスを測定するには

1.  通知を作成するときに、 **[通知のコンテンツ]** セクションで **[アプリの起動率の追跡]** チェック ボックスを選択します。
2.  In your app, call the [ParseArgumentsAndTrackAppLaunch](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.parseargumentsandtrackapplaunch) method to notify Partner Center that your app was launched in response to a targeted notification. このメソッドは Microsoft Store Services SDK によって提供されます。 For more information about how to call this method, see [Configure your app to receive Partner Center notifications](../monetize/configure-your-app-to-receive-dev-center-notifications.md).


### <a name="to-view-notification-performance"></a>通知のパフォーマンスを表示するには

When you’ve configured the notification and your app to measure notification performance as described above, you can see how well your notifications are performing.

To review detailed data for each notification:

1.  In Partner Center, expand the **Engage** section and select **Notifications**.
2.  In the table of existing notifications, select **In progress** or **Completed**, and then look at the **Delivery rate** and **App launch rate** columns to see the high-level performance of each notification.
3.  さらに詳細なパフォーマンスを確認するには、通知名を選択します。 **[配信統計]** セクションでは、通知に関する以下の**状態**について、 **[カウント]** と **[割合]** の情報を確認できます。
    * **失敗**: 何らかの理由で、通知が配信されませんでした。 これは、Windows 通知サービスで問題が発生した場合などに発生します。
    * **Channel expiration failure**: The notification could not be delivered because the channel between the app and Partner Center has expired. これは、顧客が長期間アプリを開いていない場合などに発生します。
    * **送信中**: 通知がキューで送信待ちになっています。
    * **送信済み**: 通知が送信されました。
    * **起動**: 通知が送信され、顧客が通知をクリックして、その結果、アプリが開かれました。 これは、アプリの起動のみを追跡することに注意してください。 ストアを起動して評価をするなど、他のアクションを実行するよう顧客に促す通知は、この状態には含まれていません。
    * **不明**: この通知の状態を特定できませんでした。

To analyze user activity data for all your notifications:

1.  In Partner Center, expand the **Engage** section and select **Notifications**.
2.  On the **Notifications** page, click the **Analyze** tab. This tab displays the following data:
    * Graph views of the various user action states for your toasts and action center notifications.
    * World map views of the click-through-rates for your toasts and action center notifications.
3. ページの上部で、データを表示する期間を選択できます。 既定では [30 日間] が選択されていますが、3、6、12 か月間のデータや、指定した任意の期間のデータを表示することもできます。 You can also expand **Filters** to filter all of the data by app and market.

## <a name="translate-your-notifications"></a>通知の翻訳

通知を最大限に活用するには、顧客が希望する言語に翻訳することを検討します。 Partner Center makes it easy for you to translate your notifications automatically by leveraging the power of the [Microsoft Translator](https://www.microsoft.com/translator/home.aspx) service.

1.  通知を既定の言語で記述したら、 **[言語の追加]** ( **[通知のコンテンツ]** セクションの **[言語]** メニューの下) を選択します。
2.  **[言語の追加]** ウィンドウで、通知を表示する追加の言語を選択し、 **[更新]** を選択します。
通知は **[言語の追加]** ウィンドウで選択した言語に自動的に翻訳され、これらの言語は **[言語]** メニューに追加されます。
3.  通知の翻訳を確認するには、 **[言語]** メニューで、追加した言語を選択します。

翻訳に関する考慮事項:
 - 選択した言語の **[コンテンツ]** ボックスに別のテキストを入力すると、自動翻訳を上書きできます。
 - 自動翻訳を上書きした後に英語バージョンの通知に別のテキスト ボックスを追加した場合は、新しいテキスト ボックスは翻訳済みの通知には追加されません。 その場合、翻訳済みの通知ごとに新しいテキスト ボックスを手動で追加する必要があります。
 - 通知が翻訳された後に英語のテキストを変更した場合は、翻訳済みの通知は変更に合わせて自動的に更新されます。 ただし、初期の翻訳を上書きすることを以前に選択している場合は、この処理は適用されません。

## <a name="related-topics"></a>関連トピック
- [Tiles for UWP apps](../design/shell/tiles-and-notifications/creating-tiles.md)
- [Windows プッシュ通知サービス (WNS) の概要](../design/shell/tiles-and-notifications/windows-push-notification-services--wns--overview.md)
- [Notifications Visualizer app](https://www.microsoft.com/store/apps/9nblggh5xsl1)
- [StoreServicesEngagementManager.RegisterNotificationChannelAsync() | registerNotificationChannelAsync() method](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.registernotificationchannelasync)
