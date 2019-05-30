---
Description: 別のウィンドウで、アプリの複数の部分を表示します。
title: アプリの複数のビューの表示
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 097ff0bb9e2ac8d36780a692172afb0a7933fdd1
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/29/2019
ms.locfileid: "66364972"
---
# <a name="show-multiple-views-for-an-app"></a>アプリの複数のビューの表示

![複数のウィンドウでアプリを表示するワイヤーフレーム](images/multi-view.gif)

アプリの独立した部分を別々のウィンドウで表示できるようにすることは、ユーザーが生産性を高めるために役立ちます。 アプリの複数のウィンドウを作成すると、各ウィンドウは別々に動作します。 タスク バーには各ウィンドウが別々に表示されます。 ユーザーはアプリ ウィンドウの移動、サイズ変更、表示、非表示を個別に行うことができます。また、個別のアプリの場合と同じように各アプリ ウィンドウを切り替えることができます。 各ウィンドウは、独自のスレッドで動作します。

> **重要な API**:[**ApplicationViewSwitcher**](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.ApplicationViewSwitcher)、 [ **CreateNewView**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.createnewview)

## <a name="when-should-an-app-use-multiple-views"></a>アプリが複数のビューを使用する場合
複数のビューによるメリットを活用できる、さまざまなシナリオがあります。 以下は例です。
 - 受信したメッセージの一覧を表示しながら、新しいメールを作成できるメール アプリ
 - 複数の連絡先情報を並列に表示して比較できるアドレス帳アプリ
 - 再生中の曲の情報を表示しながら、その他の利用可能な曲のリストを閲覧できるミュージック プレイヤー アプリ
 - ノートの 1 つのページから別のページに情報をコピーできるメモ アプリ
 - すべてのヘッドライン概要に目を通しながら、後から読むための記事を複数開くことができる閲覧アプリ

アプリの別のインスタンスを作成するには、「[マルチインスタンスの UWP アプリの作成](../../launch-resume/multi-instance-uwp.md)」を参照してください。

## <a name="what-is-a-view"></a>ビューとは

アプリのビューは、スレッドとウィンドウが 1:1 で対応したもので、アプリがコンテンツの表示に使います。 ビューは [**Windows.ApplicationModel.Core.CoreApplicationView**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.CoreApplicationView) オブジェクトによって表現されます。

また、[**CoreApplication**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.CoreApplication) オブジェクトによって管理されます。 [  **CoreApplication.CreateNewView**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.createnewview) を呼び出して、[**CoreApplicationView**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.CoreApplicationView) オブジェクトを作成できます。 **CoreApplicationView** は [**CoreWindow**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow) と [**CoreDispatcher**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreDispatcher) ([**CoreWindow**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplicationview.corewindow) プロパティと [**Dispatcher**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplicationview.dispatcher) プロパティに格納) を関連付けます。 **CoreApplicationView** は、Windows ランタイムがコア Windows システムとのやり取りに使うオブジェクトとして考えることができます。

通常は [**CoreApplicationView**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.CoreApplicationView) を直接操作しません。 代わりに Windows ランタイムでは、[**ApplicationView**](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.ApplicationView) クラスは [**Windows.UI.ViewManagement**](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement) 名前空間にあります。 このクラスには、アプリがウィンドウ システムとのやり取りに使うプロパティ、メソッド、イベントが用意されています。 **ApplicationView** を操作するには、静的メソッド [**ApplicationView.GetForCurrentView**](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationview.getforcurrentview) を呼び出して、現在の **CoreApplicationView** のスレッドに関連付けられている **ApplicationView** インスタンスを取得します。

同様に、XAML フレーム ワークは [**CoreWindow**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow) オブジェクトを [**Windows.UI.XAML.Window**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Window) オブジェクトにラップします。 XAML アプリでは通常、**CoreWindow** を直接操作しないで、**Window** オブジェクトを操作します。

## <a name="show-a-new-view"></a>新しいビューの表示

各アプリのレイアウトはそれぞれ異なりますが、「新しいウィンドウ」ボタンを予測可能な場所に含めることを推奨します。たとえば、新しいウィンドウで開くことができるコンテンツの右上隅などです。 さらに、[新しいウィンドウで開く] ためのコンテキスト メニュー オプションを含めることを検討します。

新しいビューを作成する手順を見てみましょう。 ここでは、新しいビューがボタンのクリックに応じて起動されます。

```csharp
private async void Button_Click(object sender, RoutedEventArgs e)
{
    CoreApplicationView newView = CoreApplication.CreateNewView();
    int newViewId = 0;
    await newView.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
    {
        Frame frame = new Frame();
        frame.Navigate(typeof(SecondaryPage), null);   
        Window.Current.Content = frame;
        // You have to activate the window in order to show it later.
        Window.Current.Activate();

        newViewId = ApplicationView.GetForCurrentView().Id;
    });
    bool viewShown = await ApplicationViewSwitcher.TryShowAsStandaloneAsync(newViewId);
}
```

**新しいビューを表示するには**

1.  [  **CoreApplication.CreateNewView**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.createnewview) を呼び出して、ビュー コンテンツに使う新しいウィンドウとスレッドを作成します。

    ```csharp
    CoreApplicationView newView = CoreApplication.CreateNewView();
    ```

2.  新しいビューの [**Id**](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationview.id) を記録します。 これは後でビューの表示に使います。

    作成するビューの追跡に役立つ何らかのインフラストラクチャをアプリに構築することを検討することもできます。 例については、[MultipleViews サンプル](https://go.microsoft.com/fwlink/p/?LinkId=620574)の `ViewLifetimeControl` クラスをご覧ください。

    ```csharp
    int newViewId = 0;
    ```

3.  新しいスレッドで、ウィンドウにコンテンツを読み込みます。

    [  **CoreDispatcher.RunAsync**](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.windows) メソッドを使って、UI スレッドでの新しいビューの操作をスケジュールします。 [ラムダ式](https://go.microsoft.com/fwlink/p/?LinkId=389615)を使って、**RunAsync** メソッドの引数として関数を渡します。 ラムダ関数による操作は新しいビューのスレッドで実行されます。

    XAML では通常、[**Window**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Window) の [**Content**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window.content) プロパティに [**Frame**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame) を追加した後、**Frame** から、アプリのコンテンツを定義した XAML [**Page**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page) に移ります。 詳しくは、「[2 ページ間のピア ツー ピア ナビゲーション](../basics/navigate-between-two-pages.md)」をご覧ください。

    新しい [**Window**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Window) にコンテンツが読み込まれたら、後で **Window** を表示するには、**Window** の [**Activate**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window.activate) メソッドを呼び出す必要があります。 この操作は新しいビューのスレッドで実行されるため、新しい **Window** がアクティブになります。

    最後に、後でビューの表示に使う新しいビューの [**Id**](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationview.id) を取得します。 やはり、この操作も新しいビューのスレッドで実行されるため、[**ApplicationView.GetForCurrentView**](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationview.getforcurrentview) は新しいビューの **Id** を取得します。

    ```csharp
    await newView.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
    {
        Frame frame = new Frame();
        frame.Navigate(typeof(SecondaryPage), null);   
        Window.Current.Content = frame;
        // You have to activate the window in order to show it later.
        Window.Current.Activate();

        newViewId = ApplicationView.GetForCurrentView().Id;
    });
    ```

4.  [  **ApplicationViewSwitcher.TryShowAsStandaloneAsync**](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationviewswitcher.tryshowasstandaloneasync) を呼び出して、新しいビューを表示します。

    新しいビューを作成したら、[**ApplicationViewSwitcher.TryShowAsStandaloneAsync**](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationviewswitcher.tryshowasstandaloneasync) メソッドを呼び出して、そのビューを新しいウィンドウに表示できます。 このメソッドの *viewId* パラメーターはアプリの各ビューを一意に識別する整数です。 ビュー [**Id**](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationview.id) は、**ApplicationView.Id** プロパティまたは [**ApplicationView.GetApplicationViewIdForWindow**](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationview.getapplicationviewidforwindow) メソッドを使って取得できます。

    ```csharp
    bool viewShown = await ApplicationViewSwitcher.TryShowAsStandaloneAsync(newViewId);
    ```

## <a name="the-main-view"></a>メイン ビュー


アプリの起動時に最初に作成されるビューは、*メイン ビュー*と呼ばれます。 このビューは、[**CoreApplication.MainView**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.mainview) プロパティに格納され、その [**IsMain**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplicationview.ismain) プロパティは true です。 このビューは作成しません。アプリによって作成されます。 メイン ビューのスレッドはアプリのマネージャーとして機能し、すべてのアプリの起動イベントはこのスレッドに振り分けられます。

セカンダリ ビューが開いている場合は、ウィンドウのタイトル バーの閉じるボタン (x) をクリックするなどして、メイン ビューのウィンドウを非表示にすることができます。ただし、そのスレッドはアクティブのままになります。 メイン ビューの [**Window**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Window) で [**Close**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window.close) を呼び出すと、**InvalidOperationException** が発生します  (使用[ **Application.Exit** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.exit)アプリを閉じます)。メイン ビューのスレッドが終了した場合、アプリを閉じます。

## <a name="secondary-views"></a>セカンダリ ビュー


アプリのコードで [**CreateNewView**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.createnewview) を呼び出すことで作成するすべてのビューなど、その他のビューがセカンダリ ビューです。 メイン ビューとセカンダリ ビューの両方が [**CoreApplication.Views**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.views) コレクションに格納されます。 通常、ユーザーの操作に応じてセカンダリ ビューを作成します。 システムによってアプリのセカンダリ ビューが作成される場合もあります。

> [!NOTE]
> Windows の*割り当てられたアクセス*機能を使うと、[キオスク モード](https://technet.microsoft.com/library/mt219050.aspx)でアプリを実行できます。 この場合、システムによってロック画面に、アプリの UI を表示するセカンダリ ビューが作成されます。 アプリによるセカンダリ ビューの作成は許可されないため、キオスク モードで独自のセカンダリ ビューを表示しようとすると、例外がスローされます。

## <a name="switch-from-one-view-to-another"></a>ビュー間の切り替え

ユーザーには、セカンダリ ウィンドウから親ウィンドウに戻る方法を提供することを検討します。 そのためには、[**ApplicationViewSwitcher.SwitchAsync**](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationviewswitcher.switchasync) メソッドを使用します。 このメソッドを切り替え元のウィンドウのスレッドから呼び出し、切り替え先のウィンドウのビュー ID を渡します。

```csharp
await ApplicationViewSwitcher.SwitchAsync(viewIdToShow);
```

[  **SwitchAsync**](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationviewswitcher.switchasync) を使うときは、[**ApplicationViewSwitchingOptions**](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.ApplicationViewSwitchingOptions) の値を指定することで、最初のウィンドウを閉じてタスク バーから削除するかどうかを選べます。

## <a name="dos-and-donts"></a>推奨と非推奨

* 「新しいウィンドウを開く」のグリフを利用することにより、セカンダリ ビューへの明確なエントリ ポイントを提供します。
* セカンダリ ビューの目的をユーザーに伝えます。
* アプリが単一のビューでも完全に機能することを確認します。セカンダリ ビューは利便性のためのみに提供します。
* 通知やその他の一時的な視覚効果を提供するためにセカンダリ ビューを使用しないようにします。

## <a name="related-topics"></a>関連トピック

* [ApplicationViewSwitcher](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.ApplicationViewSwitcher)
* [CreateNewView](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.createnewview)
 
