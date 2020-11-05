---
description: ApplicationView クラスを使用して、アプリのさまざまな部分を個別のウィンドウに表示します。
title: ApplicationView クラスを使用してアプリのセカンダリ ウィンドウを表示する
ms.date: 07/19/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 2cdee3c0844fc5a01d0749e4e6219d92cd8178a0
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034855"
---
# <a name="show-multiple-views-with-applicationview"></a>ApplicationView を使用して複数のビューを表示する

アプリの独立した部分を別々のウィンドウで表示できるようにすることは、ユーザーが生産性を高めるために役立ちます。 アプリの複数のウィンドウを作成すると、各ウィンドウは別々に動作します。 タスク バーには各ウィンドウが別々に表示されます。 ユーザーはアプリ ウィンドウの移動、サイズ変更、表示、非表示を個別に行うことができます。また、個別のアプリの場合と同じように各アプリ ウィンドウを切り替えることができます。 各ウィンドウは、独自のスレッドで動作します。

> **重要な API** : [**ApplicationViewSwitcher**](/uwp/api/Windows.UI.ViewManagement.ApplicationViewSwitcher)、 [**CreateNewView**](/uwp/api/windows.applicationmodel.core.coreapplication.createnewview)

## <a name="what-is-a-view"></a>ビューとは

アプリのビューは、スレッドとウィンドウが 1:1 で対応したもので、アプリがコンテンツの表示に使います。 ビューは [**Windows.ApplicationModel.Core.CoreApplicationView**](/uwp/api/Windows.ApplicationModel.Core.CoreApplicationView) オブジェクトによって表現されます。

また、 [**CoreApplication**](/uwp/api/Windows.ApplicationModel.Core.CoreApplication) オブジェクトによって管理されます。 [  **CoreApplication.CreateNewView**](/uwp/api/windows.applicationmodel.core.coreapplication.createnewview) を呼び出して、 [**CoreApplicationView**](/uwp/api/Windows.ApplicationModel.Core.CoreApplicationView) オブジェクトを作成できます。 **CoreApplicationView** は [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) と [**CoreDispatcher**](/uwp/api/Windows.UI.Core.CoreDispatcher) ( [**CoreWindow**](/uwp/api/windows.applicationmodel.core.coreapplicationview.corewindow) プロパティと [**Dispatcher**](/uwp/api/windows.applicationmodel.core.coreapplicationview.dispatcher) プロパティに格納) を関連付けます。 **CoreApplicationView** は、Windows ランタイムがコア Windows システムとのやり取りに使うオブジェクトとして考えることができます。

通常は [**CoreApplicationView**](/uwp/api/Windows.ApplicationModel.Core.CoreApplicationView) を直接操作しません。 代わりに Windows ランタイムでは、 [**ApplicationView**](/uwp/api/Windows.UI.ViewManagement.ApplicationView) クラスは [**Windows.UI.ViewManagement**](/uwp/api/Windows.UI.ViewManagement) 名前空間にあります。 このクラスには、アプリがウィンドウ システムとのやり取りに使うプロパティ、メソッド、イベントが用意されています。 **ApplicationView** を操作するには、静的メソッド [**ApplicationView.GetForCurrentView**](/uwp/api/windows.ui.viewmanagement.applicationview.getforcurrentview) を呼び出して、現在の **CoreApplicationView** のスレッドに関連付けられている **ApplicationView** インスタンスを取得します。

同様に、XAML フレーム ワークは [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) オブジェクトを [**Windows.UI.XAML.Window**](/uwp/api/Windows.UI.Xaml.Window) オブジェクトにラップします。 XAML アプリでは通常、 **CoreWindow** を直接操作しないで、 **Window** オブジェクトを操作します。

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

1.  [  **CoreApplication.CreateNewView**](/uwp/api/windows.applicationmodel.core.coreapplication.createnewview) を呼び出して、ビュー コンテンツに使う新しいウィンドウとスレッドを作成します。

    ```csharp
    CoreApplicationView newView = CoreApplication.CreateNewView();
    ```

2.  新しいビューの [**Id**](/uwp/api/windows.ui.viewmanagement.applicationview.id) を記録します。 これは後でビューの表示に使います。

    作成するビューの追跡に役立つ何らかのインフラストラクチャをアプリに構築することを検討することもできます。 例については、[MultipleViews サンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MultipleViews)の `ViewLifetimeControl` クラスをご覧ください。

    ```csharp
    int newViewId = 0;
    ```

3.  新しいスレッドで、ウィンドウにコンテンツを読み込みます。

    [  **CoreDispatcher.RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync) メソッドを使って、UI スレッドでの新しいビューの操作をスケジュールします。 [ラムダ式](/dotnet/csharp/language-reference/operators/lambda-expressions)を使って、 **RunAsync** メソッドの引数として関数を渡します。 ラムダ関数による操作は新しいビューのスレッドで実行されます。

    XAML では通常、 [**Window**](/uwp/api/Windows.UI.Xaml.Window) の [**Content**](/uwp/api/windows.ui.xaml.window.content) プロパティに [**Frame**](/uwp/api/Windows.UI.Xaml.Controls.Frame) を追加した後、 **Frame** から、アプリのコンテンツを定義した XAML [**Page**](/uwp/api/Windows.UI.Xaml.Controls.Page) に移ります。 フレームとページの詳細については、[2 ページ間でのピア ツー ピアのナビゲーション](../basics/navigate-between-two-pages.md)に関するページを参照してください。

    新しい [**Window**](/uwp/api/Windows.UI.Xaml.Window) にコンテンツが読み込まれたら、後で **Window** を表示するには、 **Window** の [**Activate**](/uwp/api/windows.ui.xaml.window.activate) メソッドを呼び出す必要があります。 この操作は新しいビューのスレッドで実行されるため、新しい **Window** がアクティブになります。

    最後に、後でビューの表示に使う新しいビューの [**Id**](/uwp/api/windows.ui.viewmanagement.applicationview.id) を取得します。 やはり、この操作も新しいビューのスレッドで実行されるため、 [**ApplicationView.GetForCurrentView**](/uwp/api/windows.ui.viewmanagement.applicationview.getforcurrentview) は新しいビューの **Id** を取得します。

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

4.  [  **ApplicationViewSwitcher.TryShowAsStandaloneAsync**](/uwp/api/windows.ui.viewmanagement.applicationviewswitcher.tryshowasstandaloneasync) を呼び出して、新しいビューを表示します。

    新しいビューを作成したら、 [**ApplicationViewSwitcher.TryShowAsStandaloneAsync**](/uwp/api/windows.ui.viewmanagement.applicationviewswitcher.tryshowasstandaloneasync) メソッドを呼び出して、そのビューを新しいウィンドウに表示できます。 このメソッドの *viewId* パラメーターはアプリの各ビューを一意に識別する整数です。 ビュー [**Id**](/uwp/api/windows.ui.viewmanagement.applicationview.id) は、 **ApplicationView.Id** プロパティまたは [**ApplicationView.GetApplicationViewIdForWindow**](/uwp/api/windows.ui.viewmanagement.applicationview.getapplicationviewidforwindow) メソッドを使って取得できます。

    ```csharp
    bool viewShown = await ApplicationViewSwitcher.TryShowAsStandaloneAsync(newViewId);
    ```

## <a name="the-main-view"></a>メイン ビュー


アプリの起動時に最初に作成されるビューは、 *メイン ビュー* と呼ばれます。 このビューは、 [**CoreApplication.MainView**](/uwp/api/windows.applicationmodel.core.coreapplication.mainview) プロパティに格納され、その [**IsMain**](/uwp/api/windows.applicationmodel.core.coreapplicationview.ismain) プロパティは true です。 このビューは作成しません。アプリによって作成されます。 メイン ビューのスレッドはアプリのマネージャーとして機能し、すべてのアプリの起動イベントはこのスレッドに振り分けられます。

セカンダリ ビューが開いている場合は、ウィンドウのタイトル バーの閉じるボタン (x) をクリックするなどして、メイン ビューのウィンドウを非表示にすることができます。ただし、そのスレッドはアクティブのままになります。 メイン ビューの [**Window**](/uwp/api/Windows.UI.Xaml.Window) で [**Close**](/uwp/api/windows.ui.xaml.window.close) を呼び出すと、 **InvalidOperationException** が発生します  ( [**Application.Exit**](/uwp/api/windows.ui.xaml.application.exit) を使用してアプリを閉じます)メイン ビューのスレッドが終了した場合、アプリは閉じられます。

## <a name="secondary-views"></a>セカンダリ ビュー


アプリのコードで [**CreateNewView**](/uwp/api/windows.applicationmodel.core.coreapplication.createnewview) を呼び出すことで作成するすべてのビューなど、その他のビューがセカンダリ ビューです。 メイン ビューとセカンダリ ビューの両方が [**CoreApplication.Views**](/uwp/api/windows.applicationmodel.core.coreapplication.views) コレクションに格納されます。 通常、ユーザーの操作に応じてセカンダリ ビューを作成します。 システムによってアプリのセカンダリ ビューが作成される場合もあります。

> [!NOTE]
> Windows の *割り当てられたアクセス* 機能を使うと、 [キオスク モード](/windows/manage/set-up-a-device-for-anyone-to-use)でアプリを実行できます。 この場合、システムによってロック画面に、アプリの UI を表示するセカンダリ ビューが作成されます。 アプリによるセカンダリ ビューの作成は許可されないため、キオスク モードで独自のセカンダリ ビューを表示しようとすると、例外がスローされます。

## <a name="switch-from-one-view-to-another"></a>ビュー間の切り替え

ユーザーには、セカンダリ ウィンドウから親ウィンドウに戻る方法を提供することを検討します。 そのためには、 [**ApplicationViewSwitcher.SwitchAsync**](/uwp/api/windows.ui.viewmanagement.applicationviewswitcher.switchasync) メソッドを使用します。 このメソッドを切り替え元のウィンドウのスレッドから呼び出し、切り替え先のウィンドウのビュー ID を渡します。

```csharp
await ApplicationViewSwitcher.SwitchAsync(viewIdToShow);
```

[  **SwitchAsync**](/uwp/api/windows.ui.viewmanagement.applicationviewswitcher.switchasync) を使うときは、 [**ApplicationViewSwitchingOptions**](/uwp/api/Windows.UI.ViewManagement.ApplicationViewSwitchingOptions) の値を指定することで、最初のウィンドウを閉じてタスク バーから削除するかどうかを選べます。

## <a name="related-topics"></a>関連トピック

- [複数のビューを表示する](show-multiple-views.md)
- [AppWindow を使用して複数のビューを表示する](app-window.md)
- [ApplicationViewSwitcher](/uwp/api/Windows.UI.ViewManagement.ApplicationViewSwitcher)
- [CreateNewView](/uwp/api/windows.applicationmodel.core.coreapplication.createnewview)
