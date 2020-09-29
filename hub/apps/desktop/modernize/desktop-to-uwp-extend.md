---
title: Windows UI とコンポーネントでアプリを拡張する
description: UWP プロジェクトと Windows ランタイム コンポーネントを使用してデスクトップ アプリケーションを拡張し、最新の Windows 10 エクスペリエンスを追加します。
ms.date: 06/08/2018
ms.topic: article
keywords: windows 10, uwp
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: d0a1d1b7685ae76c26c94fa104b6c0ff6334364d
ms.sourcegitcommit: 609441402c17d92e7bfac83a6056909bb235223c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/21/2020
ms.locfileid: "90837827"
---
# <a name="extend-your-desktop-app-with-modern-uwp-components"></a>最新の UWP コンポーネントでデスクトップ アプリを拡張する

一部の Windows 10 エクスペリエンス (タッチ対応 UI ページなど) は、最新のアプリ コンテナー内で実行する必要があります。 こうしたエクスペリエンスを追加するには、UWP プロジェクトと Windows ランタイム コンポーネントを使ってデスクトップ アプリケーションを拡張します。

多くの場合、デスクトップ アプリケーションから Windows ランタイム API を直接呼び出すことができます。そのため、このガイドを確認する前に、[Windows 10 のための強化](desktop-to-uwp-enhance.md)に関する記事をご覧ください。

> [!NOTE]
> この記事で説明されている機能を使用するには、[デスクトップ アプリを MSIX パッケージにパッケージ化する](/windows/msix/desktop/desktop-to-uwp-root)か、または[スパース パッケージを使用してアプリ ID を付与する](grant-identity-to-nonpackaged-apps.md)ことで、デスクトップ アプリに[パッケージ ID ](modernize-packaged-apps.md)を付与する必要があります。

準備ができたら始めましょう。

<a id="setup"></a>

## <a name="first-setup-your-solution"></a>まず、ソリューションをセットアップする

UWP プロジェクトとランタイム コンポーネントを 1 つ以上ソリューションに追加します。

**Windows アプリケーション パッケージ プロジェクト**とデスクトップ アプリケーションへの参照が含まれるソリューションから始めます。

次の画像は、ソリューションの例を示しています。

![開始プロジェクトを拡張する](images/desktop-to-uwp/extend-start-project.png)

ソリューションにパッケージ プロジェクトが含まれていない場合は、[Visual Studio を使ったデスクトップ アプリケーションのパッケージ化](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)に関する記事を参照してください。

### <a name="configure-the-desktop-application"></a>デスクトップ アプリケーションを構成する

デスクトップ アプリケーションに Windows ランタイム API を呼び出すために必要なファイルへの参照があることを確認します。

これを行うには、「[デスクトップ アプリで Windows ランタイム API を呼び出す](desktop-to-uwp-enhance.md)」を参照してください。

### <a name="add-a-uwp-project"></a>UWP プロジェクトを追加する

ソリューションに **[空白のアプリ (ユニバーサル Windows)]** を追加します。

ここでは、最新の XAML UI をビルドするか、UWP プロセス内でのみ実行される API を使います。

![新しいプロジェクトの追加](images/desktop-to-uwp/add-uwp-project-to-solution.png)

パッケージ プロジェクトで、 **[アプリケーション]** ノードを右クリックして **[参照の追加]** をクリックします。

![参照の追加](images/desktop-to-uwp/add-uwp-project-reference.png)

次に、UWP プロジェクトに参照を追加します。

![UWP プロジェクトの選択](images/desktop-to-uwp/choose-uwp-project.png)

ソリューションは次のようになります。

![UWP プロジェクトのあるソリューション](images/desktop-to-uwp/uwp-project-reference.png)

### <a name="optional-add-a-windows-runtime-component"></a>(省略可能) Windows ランタイム コンポーネントを追加する

いくつかのシナリオを実現するには、Windows ランタイム コンポーネントにコードを追加する必要があります。

![ランタイム コンポーネントのアプリ サービス](images/desktop-to-uwp/add-runtime-component.png)

次に、UWP プロジェクトからランタイム コンポーネントに参照を追加します。 ソリューションは次のようになります。

![ランタイム コンポーネント参照](images/desktop-to-uwp/runtime-component-reference.png)

### <a name="build-your-solution"></a>ソリューションをビルドする

ソリューションをビルドして、エラーが発生していないことを確認します。 エラーが発生した場合は、**構成マネージャー**を開き、プロジェクトが確実に同じプラットフォームを対象とするようにします。

![構成マネージャー](images/desktop-to-uwp/config-manager.png)

UWP プロジェクトとランタイム コンポーネントで行うことができる操作をいくつか見てみましょう。

## <a name="show-a-modern-xaml-ui"></a>最新の XAML UI を表示する

アプリケーション フローの一環として、最新の XAML ベースのユーザー インターフェイスをデスクトップ アプリケーションに組み込むことができます。 これらのユーザー インターフェイスは、さまざまな画面サイズと解像度に適応し、タッチや手描きなどの最新の対話モデルをサポートする性質を備えています。

たとえば、少量の XAML マークアップを使用して、地図関連の強力な視覚化機能をユーザーに提供できます。

次の画像に、マップ コントロールを含む XAML ベースの最新の UI を表示している Windows フォーム アプリケーションを示しています。

![アダプティブ デザイン](images/desktop-to-uwp/extend-xaml-ui.png)

>[!NOTE]
>この例では、UWP プロジェクトをソリューションに追加して、XAML UI を表示しています。 これは、デスクトップ アプリケーションで XAML UI を表示するためにサポートされている安定した方法です。 この方法の代わりに、XAML Island を使用して UWP XAML コントロールをデスクトップ アプリケーションに直接追加することもできます。 XAML Islands は現在、開発者プレビューとして使用できます。 ご自身のプロトタイプ コードでこれらを試すことはお勧めしますが、現時点では運用コードで使用することはお勧めしません。 これらの API とコントロールは、引き続き今後の Windows リリースで完成度が高められ、安定したものになる予定です。 XAML Islands の詳細については、[デスクトップ アプリケーションでの UWP コントロール ](xaml-islands.md)に関する記事を参照してください

### <a name="the-design-pattern"></a>設計パターン

XAML ベースの UI を表示するには、以下の手順を実行します。

:1:[ソリューションをセットアップする](#solution-setup)

:2:[XAML UI を作成する](#xaml-UI)

:3:[プロトコル拡張機能を UWP プロジェクトに追加する](#add-a-protocol-extension)

:4:[デスクトップ アプリから UWP アプリを起動する](#start)

:5:[UWP プロジェクトで目的のページを表示する](#parse)

<a id="solution-setup"></a>

### <a name="setup-your-solution"></a>ソリューションをセットアップする

ソリューションのセットアップ方法に関する一般的なガイダンスについては、このガイドの冒頭の「[まず、ソリューションをセットアップする](#setup)」セクションを参照してください。

ソリューションは次のようになります。

![XAML UI ソリューション](images/desktop-to-uwp/xaml-ui-solution.png)

この例では、Windows フォーム プロジェクトは **Landmarks** という名前で、XAML UI を含む UWP プロジェクトは **MapUI** という名前です。

<a id="xaml-UI"></a>

### <a name="create-a-xaml-ui"></a>XAML UI の作成

XAML UI を UWP プロジェクトに追加します。 基本的なマップの XAML を次に示します。

```xml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}" Margin="12,20,12,14">
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="Auto"/>
        <ColumnDefinition Width="*"/>
    </Grid.ColumnDefinitions>
    <maps:MapControl x:Name="myMap" Grid.Column="0" Width="500" Height="500"
                     ZoomLevel="{Binding ElementName=zoomSlider,Path=Value, Mode=TwoWay}"
                     Heading="{Binding ElementName=headingSlider,Path=Value, Mode=TwoWay}"
                     DesiredPitch="{Binding ElementName=desiredPitchSlider,Path=Value, Mode=TwoWay}"
                     HorizontalAlignment="Left"
                     MapServiceToken="<Your Key Goes Here" />
    <Grid Grid.Column="1" Margin="12">
        <StackPanel>
            <Slider Minimum="1" Maximum="20" Header="ZoomLevel" Name="zoomSlider" Value="17.5"/>
            <Slider Minimum="0" Maximum="360" Header="Heading" Name="headingSlider" Value="0"/>
            <Slider Minimum="0" Maximum="64" Header=" DesiredPitch" Name="desiredPitchSlider" Value="32"/>
        </StackPanel>
    </Grid>
</Grid>
```

### <a name="add-a-protocol-extension"></a>プロトコル拡張機能を追加する

**ソリューション エクスプローラー**で、ソリューション内にパッケージ プロジェクトの **package.appxmanifest** ファイルを開き、この拡張機能を追加します。

```xml
<Extensions>
  <uap:Extension Category="windows.protocol" Executable="MapUI.exe" EntryPoint="MapUI.App">
    <uap:Protocol Name="xamluidemo" />
  </uap:Extension>
</Extensions>
```

プロトコルに名前を付けて、UWP プロジェクトによって生成された実行可能ファイルの名前と、エントリ ポイント クラスの名前を指定します。

デザイナーで **package.appxmanifest** 開き、 **[宣言]** タブを選んで、そこで拡張機能を追加することもできます。

![[宣言] タブ](images/desktop-to-uwp/protocol-properties.png)

> [!NOTE]
> マップ コントロールはインターネットからデータをダウンロードします。そのため、マップ コントロールを使用する場合は、"インターネット クライアント" 機能もマニフェストに追加する必要があります。

<a id="start"></a>

### <a name="start-the-uwp-app"></a>UWP アプリを起動する

まず、デスクトップ アプリケーションから、プロトコル名と UWP アプリに渡すパラメーターが含まれた [URI](/dotnet/api/system.uri) を作成します。 次に、[LaunchUriAsync](/uwp/api/windows.system.launcher.launchuriasync) メソッドを呼び出します。

```csharp

private void Statue_Of_Liberty_Click(object sender, EventArgs e)
{
    ShowMap(40.689247, -74.044502);
}

private async void ShowMap(double lat, double lon)
{
    string str = "xamluidemo://";

    Uri uri = new Uri(str + "location?lat=" +
        lat.ToString() + "&?lon=" + lon.ToString());

    var success = await Windows.System.Launcher.LaunchUriAsync(uri);

}
```

<a id="parse"></a>

### <a name="parse-parameters-and-show-a-page"></a>パラメーターを解析してページを表示する

UWP プロジェクトの **App** クラスで、**OnActivated** イベント ハンドラーをオーバーライドします。 アプリがプロトコルによってアクティブ化されている場合は、パラメーターを解析して目的のページを開きます。

```csharp
protected override void OnActivated(Windows.ApplicationModel.Activation.IActivatedEventArgs e)
{
    if (e.Kind == ActivationKind.Protocol)
    {
        ProtocolActivatedEventArgs protocolArgs = (ProtocolActivatedEventArgs)e;
        Uri uri = protocolArgs.Uri;
        if (uri.Scheme == "xamluidemo")
        {
            Frame rootFrame = new Frame();
            Window.Current.Content = rootFrame;
            rootFrame.Navigate(typeof(MainPage), uri.Query);
            Window.Current.Activate();
        }
    }
}
```

XAML ページの背後にあるコードで、ページに渡されたパラメーターを使用するように ``OnNavigatedTo`` メソッドをオーバーライドします。 この場合、このページに渡された緯度と経度を使用してマップに場所を表示します。

```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
 {
     if (e.Parameter != null)
     {
         WwwFormUrlDecoder decoder = new WwwFormUrlDecoder(e.Parameter.ToString());

         double lat = Convert.ToDouble(decoder[0].Value);
         double lon = Convert.ToDouble(decoder[1].Value);

         BasicGeoposition pos = new BasicGeoposition();

         pos.Latitude = lat;
         pos.Longitude = lon;

         myMap.Center = new Geopoint(pos);

         myMap.Style = MapStyle.Aerial3D;

     }

     base.OnNavigatedTo(e);
 }
```

## <a name="making-your-desktop-application-a-share-target"></a>デスクトップ アプリケーションを共有ターゲットにする

デスクトップ アプリケーションを共有ターゲットにすることで、共有をサポートしている他のアプリのデータ (画像など) をユーザーが簡単に共有できるようになります。

たとえば、ユーザーは、Microsoft Edge やフォト アプリから画像を共有するためにアプリケーションを選択できます。 その機能を備えた WPF サンプル アプリケーションを次に示します。

![共有ターゲット](images/desktop-to-uwp/share-target.png)。

完全なサンプルについては、[こちら](https://github.com/Microsoft/Windows-Packaging-Samples/tree/master/ShareTarget)を参照してください

### <a name="the-design-pattern"></a>設計パターン

アプリケーションを共有ターゲットにするには、以下の手順を実行します。

:1:[共有ターゲットの拡張機能を追加する](#share-extension)

:2:[OnShareTargetActivated イベント ハンドラーをオーバーライドする](#override)

:3:[UWP プロジェクトにデスクトップ拡張機能を追加する](#desktop-extensions)

:4:[完全信頼のプロセス拡張機能を追加する](#full-trust)

:5:[共有ファイルを取得するようにデスクトップ アプリケーションを変更する](#modify-desktop)

<a id="share-extension"></a>

次の手順に従います  

### <a name="add-a-share-target-extension"></a>共有ターゲットの拡張機能を追加する

**ソリューション エクスプローラー**で、ソリューション内にパッケージ プロジェクトの **package.appxmanifest** ファイルを開き、共有ターゲットの拡張機能を追加します。

```xml
<Extensions>
      <uap:Extension
          Category="windows.shareTarget"
          Executable="ShareTarget.exe"
          EntryPoint="App">
        <uap:ShareTarget>
          <uap:SupportedFileTypes>
            <uap:SupportsAnyFileType />
          </uap:SupportedFileTypes>
          <uap:DataFormat>Bitmap</uap:DataFormat>
        </uap:ShareTarget>
      </uap:Extension>
</Extensions>  
```

UWP プロジェクトによって生成された実行可能ファイルの名前と、エントリ ポイント クラスの名前を指定します。 このマークアップでは、UWP アプリの実行可能ファイルの名前が `ShareTarget.exe` であることを前提としています。

アプリとの間で共有できるようにするファイルの種類を指定することも必要です。 この例では、[WPF PhotoStoreDemo](https://github.com/Microsoft/WPF-Samples/tree/master/Sample%20Applications/PhotoStoreDemo) デスクトップ アプリケーションをビットマップ イメージの共有ターゲットとしています。そのため、サポートされているファイルの種類に `Bitmap` を指定します。

<a id="override"></a>

### <a name="override-the-onsharetargetactivated-event-handler"></a>OnShareTargetActivated イベント ハンドラーをオーバーライドする

UWP プロジェクトの **App** クラスで、**OnShareTargetActivated** イベント ハンドラーをオーバーライドします。

このイベント ハンドラーは、ユーザーがファイルを共有するためにアプリを選択するときに呼び出されます。

```csharp

protected override void OnShareTargetActivated(ShareTargetActivatedEventArgs args)
{
    shareWithDesktopApplication(args.ShareOperation);
}

private async void shareWithDesktopApplication(ShareOperation shareOperation)
{
    if (shareOperation.Data.Contains(StandardDataFormats.StorageItems))
    {
        var items = await shareOperation.Data.GetStorageItemsAsync();
        StorageFile file = items[0] as StorageFile;
        IRandomAccessStreamWithContentType stream = await file.OpenReadAsync();

        await file.CopyAsync(ApplicationData.Current.LocalFolder);
            shareOperation.ReportCompleted();

        await FullTrustProcessLauncher.LaunchFullTrustProcessForCurrentAppAsync();
    }
}
```

このコードでは、ユーザーによって共有されているイメージをアプリのローカル ストレージ フォルダーに保存します。 後で、その同じフォルダーからイメージをプルするように、デスクトップ アプリケーションを変更します。 デスクトップ アプリケーションは、UWP アプリと同じパッケージに含まれているために、これを行うことができます。

<a id="desktop-extensions"></a>

### <a name="add-desktop-extensions-to-the-uwp-project"></a>UWP プロジェクトにデスクトップ拡張機能を追加する

UWP アプリ プロジェクトに **[Windows Desktop Extensions for the UWP]** 拡張機能を追加します。

![デスクトップ拡張機能](images/desktop-to-uwp/desktop-extensions.png)

<a id="full-trust"></a>

### <a name="add-the-full-trust-process-extension"></a>完全信頼のプロセス拡張機能を追加する

**ソリューション エクスプローラー**で、ソリューション内にパッケージ プロジェクトの **package.appxmanifest** ファイルを開き、以前にこのファイルに追加している共有ターゲット拡張機能の横に、完全信頼のプロセス拡張機能を追加します。

```xml
<Extensions>
  ...
      <desktop:Extension Category="windows.fullTrustProcess" Executable="PhotoStoreDemo\PhotoStoreDemo.exe" />
  ...
</Extensions>  
```

この拡張機能によって、UWP アプリでは、ファイルを共有するデスクトップ アプリケーションを起動できるようになります。 例では、[WPF PhotoStoreDemo](https://github.com/Microsoft/WPF-Samples/tree/master/Sample%20Applications/PhotoStoreDemo) デスクトップ アプリケーションの実行可能ファイルを参照しています。

<a id="modify-desktop"></a>

### <a name="modify-the-desktop-application-to-get-the-shared-file"></a>共有ファイルを取得するようにデスクトップ アプリケーションを変更する

共有ファイルを検索して処理するように、デスクトップ アプリケーションを変更します。 この例では、UWP アプリによって、ローカル アプリ データ フォルダー内に共有ファイルが保存されました。 そのため、そのフォルダーから写真をプルするように、[WPF PhotoStoreDemo](https://github.com/Microsoft/WPF-Samples/tree/master/Sample%20Applications/PhotoStoreDemo) デスクトップ アプリケーションを変更します。

```csharp
Photos.Path = Windows.Storage.ApplicationData.Current.LocalFolder.Path;
```

ユーザーによって既に開かれているデスクトップ アプリケーションのインスタンスでは、[FileSystemWatcher](/dotnet/api/system.io.filesystemwatcher) イベントを処理して、ファイルの場所へのパスを渡すこともできます。 これにより、開かれたデスクトップ アプリケーションのインスタンスでは、共有された写真が表示されます。

```csharp
...

   FileSystemWatcher watcher = new FileSystemWatcher(Photos.Path);

...

private void Watcher_Created(object sender, FileSystemEventArgs e)
{
    // new file got created, adding it to the list
    Dispatcher.BeginInvoke(System.Windows.Threading.DispatcherPriority.Normal, new Action(() =>
    {
        if (File.Exists(e.FullPath))
        {
            ImageFile item = new ImageFile(e.FullPath);
            Photos.Insert(0, item);
            PhotoListBox.SelectedIndex = 0;
            CurrentPhoto.Source = (BitmapSource)item.Image;
        }
    }));
}

```

## <a name="create-a-background-task"></a>バックグラウンド タスクを作成する

バックグラウンド タスクを追加して、アプリが一時停止されているときでもコードを実行できます。 バックグラウンド タスクは、ユーザーの操作を必要としない小さなタスクに最適です。 たとえば、タスクはメールのダウンロード、受信チャット メッセージに関するトースト通知の表示、システムの状態の変化に対する対応を行うことができます。

バックグラウンド タスクを登録する WPF サンプル アプリケーションを以下に示します。

![バックグラウンド タスク](images/desktop-to-uwp/sample-background-task.png)

タスクは http 要求を行い、要求が応答を返すのにかかる時間を測定します。 タスクはさらに興味深いものと考えられますが、このサンプルはバックグラウンド タスクの基本的なしくみを学習するのに適しています。

完全なサンプルについては、[こちら](https://github.com/Microsoft/Windows-Packaging-Samples/tree/master/BGTask)を参照してください。

### <a name="the-design-pattern"></a>設計パターン

バックグラウンド サービスを作成するには、以下の手順を実行します。

:1:[バックグラウンド タスクの実装](#implement-task)

:2:[バックグラウンド タスクの構成](#configure-background-task)

:3:[バックグラウンド タスクの登録](#register-background-task)

<a id="implement-task"></a>

### <a name="implement-the-background-task"></a>バックグラウンド タスクの実装

Windows ランタイム コンポーネント プロジェクトにコードを追加することで、バックグラウンド タスクを実装します。

```csharp
public sealed class SiteVerifier : IBackgroundTask
{
    public async void Run(IBackgroundTaskInstance taskInstance)
    {

        taskInstance.Canceled += TaskInstance_Canceled;
        BackgroundTaskDeferral deferral = taskInstance.GetDeferral();
        var msg = await MeasureRequestTime();
        ShowToast(msg);
        deferral.Complete();
    }

    private async Task<string> MeasureRequestTime()
    {
        string msg;
        try
        {
            var url = ApplicationData.Current.LocalSettings.Values["UrlToVerify"] as string;
            var http = new HttpClient();
            Stopwatch clock = Stopwatch.StartNew();
            var response = await http.GetAsync(new Uri(url));
            response.EnsureSuccessStatusCode();
            var elapsed = clock.ElapsedMilliseconds;
            clock.Stop();
            msg = $"{url} took {elapsed.ToString()} ms";
        }
        catch (Exception ex)
        {
            msg = ex.Message;
        }
        return msg;
    }
```

<a id="configure-background-task"></a>

### <a name="configure-the-background-task"></a>バックグラウンド タスクの構成

マニフェスト デザイナーで、ソリューション内にパッケージ プロジェクトの **package.appxmanifest** ファイルを開きます。

**[宣言]** タブで、 **[バックグラウンド タスク]** 宣言を追加します。

![バックグラウンド タスクのオプション](images/desktop-to-uwp/background-task-option.png)

次に、必要なプロパティを選択します。 サンプルでは、**Timer** プロパティを使います。

![Timer プロパティ](images/desktop-to-uwp/timer-property.png)

バックグラウンド タスクを実装する Windows ランタイム コンポーネントで、クラスの完全修飾名を指定します。

![エントリ ポイントの指定](images/desktop-to-uwp/background-task-entry-point.png)

<a id="register-background-task"></a>

### <a name="register-the-background-task"></a>バックグラウンド タスクの登録

バックグラウンド タスクを登録するデスクトップ アプリケーション プロジェクトにコードを追加します。

```csharp
public void RegisterBackgroundTask(String triggerName)
{
    var current = BackgroundTaskRegistration.AllTasks
        .Where(b => b.Value.Name == triggerName).FirstOrDefault().Value;

    if (current is null)
    {
        BackgroundTaskBuilder builder = new BackgroundTaskBuilder();
        builder.Name = triggerName;
        builder.SetTrigger(new MaintenanceTrigger(15, false));
        builder.TaskEntryPoint = "HttpPing.SiteVerifier";
        builder.Register();
        System.Diagnostics.Debug.WriteLine("BGTask registered:" + triggerName);
    }
    else
    {
        System.Diagnostics.Debug.WriteLine("Task already:" + triggerName);
    }
}
```

## <a name="find-answers-to-your-questions"></a>質問に対する回答を見つける

ご質問があるでしょうか。 Stack Overflow でお問い合わせください。 Microsoft のチームでは、これらの[タグ](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)をチェックしています。 [こちら](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)から質問することもできます。