---
Description: Web ビュー コントロールでは、Microsoft Edge レンダリング エンジンを使って、Web コンテンツをレンダリングするアプリにビューが埋め込まれます。 また、Web ビュー コントロールでは、ハイパーリンクの表示と動作が可能です。
title: Web ビュー
ms.assetid: D3CFD438-F9D6-4B72-AF1D-16EF2DFC1BB1
label: Web view
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 46fc3c0eb087891de4fe622f0770bc7f1b2955d7
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89163306"
---
# <a name="web-view"></a>Web ビュー

Web ビュー コントロールでは、Microsoft Edge レンダリング エンジンを使って、Web コンテンツをレンダリングするアプリにビューが埋め込まれます。 また、Web ビュー コントロールでは、ハイパーリンクの表示と動作が可能です。

> **重要な API**: [WebView クラス](/uwp/api/Windows.UI.Xaml.Controls.WebView)

## <a name="is-this-the-right-control"></a>適切なコントロールの選択

リモート Web サーバー、動的に生成されたコード、またはアプリ パッケージのコンテンツ ファイルの、書式がリッチな HTML コンテンツを表示するには、Web ビュー コントロールを使います。 また、リッチ コンテンツは、スクリプト コードを含めることができ、さらに、スクリプトとアプリのコード間で通信を行うこともできます。

## <a name="examples"></a>例

<table>
<th align="left">XAML コントロール ギャラリー<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML コントロール ギャラリー</strong> アプリがインストールされている場合、こちらをクリックして<a href="xamlcontrolsgallery:/item/WebView">アプリを開き、WebView の動作を確認</a>してください。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML コントロール ギャラリー アプリを入手する (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">ソース コード (GitHub) を入手する</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="create-a-web-view"></a>Web ビューを作成する

**Web ビューの外観を変更する**

[WebView](/uwp/api/Windows.UI.Xaml.Controls.WebView) は、[Control](/uwp/api/Windows.UI.Xaml.Controls.Control) サブクラスではないため、コントロール テンプレートがありません。 ただし、さまざまなプロパティを設定して、Web ビューのビジュアル要素の一部を制御することはできます。
- 表示領域を制限するには、[Width](/uwp/api/windows.ui.xaml.frameworkelement.width) プロパティと [Height](/uwp/api/windows.ui.xaml.frameworkelement.height) プロパティを設定します。 
- Web ビューの変換、拡大縮小、傾斜、そして回転には、[RenderTransform](/uwp/api/windows.ui.xaml.uielement.rendertransform) プロパティを使います。
- Web ビューの不透明度を調整するには、[Opacity](/uwp/api/windows.ui.xaml.uielement.opacity) プロパティを設定します。
- HTML コンテンツが色を指定していないときに、Web ページの背景として色を指定するには、[DefaultBackgroundColor](/uwp/api/windows.ui.xaml.controls.webview.defaultbackgroundcolor) プロパティを設定します。 

**Web ページのタイトルを取得する**

[DocumentTitle](/uwp/api/windows.ui.xaml.controls.webview.documenttitle) プロパティを使うと、現在 Web ビューに表示されている HTML ドキュメントのタイトルを取得することができます。 

**入力イベントとタブ オーダー**

WebView は Control サブクラスではありませんが、キーボードの入力フォーカスを受け取って、タブ順に関与します。 ただし、[Focus](/uwp/api/windows.ui.xaml.controls.webview.focus) メソッド、そして [GotFocus](/uwp/api/windows.ui.xaml.uielement.gotfocus) イベントと [LostFocus](/uwp/api/windows.ui.xaml.uielement.lostfocus) イベントを提供しますが、タブ関連のプロパティはありません。 タブ順での位置は、XAML ドキュメントの順序での位置と同じです。 タブ順には、入力フォーカスを受け取ることができる Web ビューのコンテンツの要素がすべて含まれています。 

[WebView](/uwp/api/Windows.UI.Xaml.Controls.WebView) クラスのページの「Event」表からもわかるとおり、Web ビューは、[KeyDown](/uwp/api/windows.ui.xaml.uielement.keydown) や [KeyUp](/uwp/api/windows.ui.xaml.uielement.keyup)、[PointerPressed](/uwp/api/windows.ui.xaml.uielement.pointerpressed) といった [UIElement](/uwp/api/Windows.UI.Xaml.UIElement) から継承されたユーザー入力イベントのほとんどをサポートしていません。 その代わり、[InvokeScriptAsync](/uwp/api/windows.ui.xaml.controls.webview.invokescriptasync) を JavaScript の **eval** 関数と共に使って、HTML イベント ハンドラーを利用したり、HTML イベント ハンドラーの **window.external.notify** を通じて [WebView.ScriptNotify](/uwp/api/windows.ui.xaml.controls.webview.scriptnotify) に対応するアプリに通知したりできます。

### <a name="navigating-to-content"></a>コンテンツに移動する

Web ビューには、[GoBack](/uwp/api/windows.ui.xaml.controls.webview.goback)、[GoForward](/uwp/api/windows.ui.xaml.controls.webview.goforward)、[Stop](/uwp/api/windows.ui.xaml.controls.webview.stop)、[Refresh](/uwp/api/windows.ui.xaml.controls.webview.refresh)、[CanGoBack](/uwp/api/windows.ui.xaml.controls.webview.cangoback)、そして [CanGoForward](/uwp/api/windows.ui.xaml.controls.webview.cangoforward) という基本的な操作用の API が用意されています。 これらを使うと、一般的な Web 閲覧機能をアプリに追加できます。 

Web ビューの初期コンテンツを設定するには、XAML の [Source](/uwp/api/windows.ui.xaml.controls.webview.source) プロパティを使います。 XAML パーサーは文字列を [Uri](/uwp/api/Windows.Foundation.Uri) に自動的に変換します。 

```xaml
<!-- Source file is on the web. -->
<WebView x:Name="webView1" Source="http://www.contoso.com"/>

<!-- Source file is in local storage. -->
<WebView x:Name="webView2" Source="ms-appdata:///local/intro/welcome.html"/>

<!-- Source file is in the app package. -->
<WebView x:Name="webView3" Source="ms-appx-web:///help/about.html"/>
```

Source プロパティはコードで設定できますが、それよりも **Navigate** メソッドの 1 つを使ってコードにコンテンツを読み込むほうがよいでしょう。 

Web コンテンツを読み込むには、[Navigate](/uwp/api/windows.ui.xaml.controls.webview.navigate) メソッドを http または https スキームを使う **Uri** と共に用います。 

```csharp
webView1.Navigate("http://www.contoso.com");
```

POST 要求と HTTP ヘッダーを有する URI へと移動するには、[NavigateWithHttpRequestMessage](/uwp/api/windows.ui.xaml.controls.webview.navigatewithhttprequestmessage) メソッドを使います。 このメソッドは、[HttpRequestMessage.Method](/uwp/api/windows.web.http.httprequestmessage.method) プロパティの値として [HttpMethod.Post](/uwp/api/windows.web.http.httpmethod.post) と [HttpMethod.Get](/uwp/api/windows.web.http.httpmethod.get) のみをサポートします。 

圧縮されておらず、暗号化もされていないコンテンツをアプリの [LocalFolder](/uwp/api/windows.storage.applicationdata.localfolder) データ ストアまたは [TemporaryFolder](/uwp/api/windows.storage.applicationdata.temporaryfolder) データ ストアから読み込むには、**Navigate** メソッドを、[ms-appdata](../../app-resources/uri-schemes.md) スキームを使う **Uri** と共に用います。 このスキームを Web ビューがサポートするには、ローカル フォルダーまたは一時フォルダーの下にサブフォルダーを設け、そこにコンテンツを配置する必要があります。 これにより、「ms-appdata:///local/*フォルダー*/*ファイル*.html」や「ms-appdata:///temp/*フォルダー*/*ファイル*.html」といった URI への移動が可能になります (圧縮され、暗号化されたファイルを読み込む場合は、[NavigateToLocalStreamUri](/uwp/api/windows.ui.xaml.controls.webview.navigatetolocalstreamuri) に関するページをご覧ください)。 

これらの第 1 レベルのサブフォルダーは、他の第 1 レベルのサブフォルダー内のコンテンツから分離されます。 たとえば「ms-appdata:///temp/folder1/file.html」に移動はできますが、このファイル内のリンクに「ms-appdata:///temp/folder2/file.html」は指定できません。 ただし、**ms-appx-web** スキームを使ってアプリ パッケージの HTML コンテンツにリンクしたり、**http** または **https** の URI スキームを使って Web コンテンツにリンクしたりすることはできます。

```csharp
webView1.Navigate("ms-appdata:///local/intro/welcome.html");
```

アプリ パッケージからコンテンツを読み込むには、**Navigate** メソッドを [ms-appx-web](/previous-versions/windows/apps/jj655406(v=win.10)) スキームを使った **Uri** と共に用います。 

```csharp
webView1.Navigate("ms-appx-web:///help/about.html");
```

[NavigateToLocalStreamUri](/uwp/api/windows.ui.xaml.controls.webview.navigatetolocalstreamuri) メソッドを使えば、カスタム リゾルバーを通じてローカルのコンテンツを読み込めます。 これにより、Web ベースのコンテンツをオフライン用にダウンロードしたりキャッシュしたり、圧縮ファイルからコンテンツを抽出したりといった高度なシナリオも可能です。

### <a name="responding-to-navigation-events"></a>ナビゲーション イベントを処理する

Web ビュー コントロールでは、ナビゲーションやコンテンツの読み込みの状態に対する処理に使うことができるイベントがいくつか用意されています。 ルートとなる Web ビュー コンテンツについて、イベントは次の順番で発生します。[NavigationStarting](/uwp/api/windows.ui.xaml.controls.webview.navigationstarting)、[ContentLoading](/uwp/api/windows.ui.xaml.controls.webview.contentloading)、[DOMContentLoaded](/uwp/api/windows.ui.xaml.controls.webview.domcontentloaded)、[NavigationCompleted](/uwp/api/windows.ui.xaml.controls.webview.navigationcompleted)


**NavigationStarting** - Web ビューが新しいコンテンツに移動する前に発生します。 WebViewNavigationStartingEventArgs.Cancel プロパティを "true" に設定することで、このイベントのハンドラーで移動をキャンセルできます。 

```csharp
webView1.NavigationStarting += webView1_NavigationStarting;

private void webView1_NavigationStarting(object sender, WebViewNavigationStartingEventArgs args)
{
    // Cancel navigation if URL is not allowed. (Implemetation of IsAllowedUri not shown.)
    if (!IsAllowedUri(args.Uri))
        args.Cancel = true;
}
```

**ContentLoading** - Web ビューが新しいコンテンツの読み込みを開始すると発生します。 

```csharp
webView1.ContentLoading += webView1_ContentLoading;

private void webView1_ContentLoading(WebView sender, WebViewContentLoadingEventArgs args)
{
    // Show status.
    if (args.Uri != null)
    {
        statusTextBlock.Text = "Loading content for " + args.Uri.ToString();
    }
}
```

**DOMContentLoaded** - Web ビューが現在の HTML のコンテンツの解析を完了すると発生します。 

```csharp
webView1.DOMContentLoaded += webView1_DOMContentLoaded;

private void webView1_DOMContentLoaded(WebView sender, WebViewDOMContentLoadedEventArgs args)
{
    // Show status.
    if (args.Uri != null)
    {
        statusTextBlock.Text = "Content for " + args.Uri.ToString() + " has finished loading";
    }
}
```

**NavigationCompleted** - Web ビューが現在のコンテンツの読み込みを完了したとき、またはナビゲーションが失敗したときに発生します。 ナビゲーションが失敗したかどうかを判断するには、[WebViewNavigationCompletedEventArgs](/uwp/api/Windows.UI.Xaml.Controls.WebViewNavigationCompletedEventArgs) クラスの [IsSuccess](/uwp/api/windows.ui.xaml.controls.webviewnavigationcompletedeventargs.issuccess) プロパティと [WebErrorStatus](/uwp/api/windows.ui.xaml.controls.webviewnavigationcompletedeventargs.weberrorstatus) プロパティを確認します。 

```csharp
webView1.NavigationCompleted += webView1_NavigationCompleted;

private void webView1_NavigationCompleted(WebView sender, WebViewNavigationCompletedEventArgs args)
{
    if (args.IsSuccess == true)
    {
        statusTextBlock.Text = "Navigation to " + args.Uri.ToString() + " completed successfully.";
    }
    else
    {
        statusTextBlock.Text = "Navigation to: " + args.Uri.ToString() + 
                               " failed with error " + args.WebErrorStatus.ToString();
    }
}
```

Web ビューのコンテンツの各 **iframe** についても、同様のイベントが同じ順序で発生します。 
- [FrameNavigationStarting](/uwp/api/windows.ui.xaml.controls.webview.framenavigationstarting) - Web ビューのフレームが新しいコンテンツに移動する前に発生します。 
- [FrameContentLoading](/uwp/api/windows.ui.xaml.controls.webview.framecontentloading) - Web ビューのフレームが新しいコンテンツの読み込みを開始すると発生します。 
- [FrameDOMContentLoaded](/uwp/api/windows.ui.xaml.controls.webview.framedomcontentloaded) - Web ビューのフレームが現在の HTML のコンテンツの解析を完了すると発生します。 
- [FrameNavigationCompleted](/uwp/api/windows.ui.xaml.controls.webview.framenavigationcompleted) - Web ビューのフレームがコンテンツの読み込みを完了すると発生します。 

### <a name="responding-to-potential-problems"></a>潜在的な問題に対処する

長時間実行されているスクリプトや、Web ビューが読み込めないコンテンツ、安全でないコンテンツに関する警告など潜在的な問題があれば、それに対処することができます。 

スクリプトを実行中にアプリが応答しないような場合があったとします。 Web ビューにより JavaScript が実行され、スクリプトを中断する機会が提供される際に、[LongRunningScriptDetected](/uwp/api/windows.ui.xaml.controls.webview.longrunningscriptdetected) イベントが定期的に発生します。 スクリプトがどれくらいの時間実行されているか調べるには、[WebViewLongRunningScriptDetectedEventArgs](/uwp/api/Windows.UI.Xaml.Controls.WebViewLongRunningScriptDetectedEventArgs) の [ExecutionTime](/uwp/api/windows.ui.xaml.controls.webviewlongrunningscriptdetectedeventargs.executiontime) プロパティを確認します。 スクリプトを停止するには、イベント引数の [StopPageScriptExecution](/uwp/api/windows.ui.xaml.controls.webviewlongrunningscriptdetectedeventargs.stoppagescriptexecution) プロパティを **true** に設定します。 停止されたスクリプトは、以降の Web ビューのナビゲーションでもう一度読み込まれるまで、実行されません。 

Web ビュー コントロールは、任意のファイルの種類をホストすることができません。 Web ビューでホストできないコンテンツを読み込もうすると、[UnviewableContentIdentified](/uwp/api/windows.ui.xaml.controls.webview.unviewablecontentidentified) イベントが発生します。 このイベントを処理してユーザーに通知することも、[Launcher](/uwp/api/Windows.System.Launcher) クラスを使ってファイルを外部のブラウザーまたは別のアプリにリダイレクトすることもできます。

同様に、fbconnect:// や mailto:// といったサポートされていない URI スキームが Web コンテンツで呼び出されると、[UnsupportedUriSchemeIdentified](/uwp/api/windows.ui.xaml.controls.webview.unsupportedurischemeidentified) イベントが発生します。 既定のシステム起動プログラムに URI を起動させるのではなく、このイベントを処理してカスタム動作を定義してもよいでしょう。

Web ビューによって、SmartScreen フィルターにより安全でないと報告されているコンテンツの警告ページが表示されると、[UnsafeContentWarningDisplayingevent](/uwp/api/windows.ui.xaml.controls.webview.unsafecontentwarningdisplaying) イベントが発生します。 ユーザーがナビゲーションの続行を選んだ場合は、そのページへの移動では以降、警告が表示されたり、イベントが発されたりすることはありません。

### <a name="handling-special-cases-for-web-view-content"></a>Web ビューのコンテンツの特殊ケースを処理する

[ContainsFullScreenElement](/uwp/api/windows.ui.xaml.controls.webview.containsfullscreenelement) プロパティと [ContainsFullScreenElementChanged](/uwp/api/windows.ui.xaml.controls.webview.containsfullscreenelementchanged) イベントを使うと、全画面での動画の再生といった、全画面表示を可能にしたり、検出したり、または処理したりすることができます。 たとえば、ContainsFullScreenElementChanged イベントを使えば、Web ビューのサイズを変更して、アプリ ビュー全体を占有することができます。もしくは、次の例で示すとおり、Web の全画面表示が望ましいときは、ウィンドウ内のアプリを全画面表示にすることもできます。

```csharp
// Assume webView is defined in XAML
webView.ContainsFullScreenElementChanged += webView_ContainsFullScreenElementChanged;

private void webView_ContainsFullScreenElementChanged(WebView sender, object args)
{
    var applicationView = ApplicationView.GetForCurrentView();

    if (sender.ContainsFullScreenElement)
    {
        applicationView.TryEnterFullScreenMode();
    }
    else if (applicationView.IsFullScreenMode)
    {
        applicationView.ExitFullScreenMode();
    }
}
```

[NewWindowRequested](/uwp/api/windows.ui.xaml.controls.webview.newwindowrequested) イベントを使えば、たとえばポップアップ ウィンドウのように、ホストされている Web コンテンツによって新しいウィンドウの表示が要求されるようなケースに対処できます。 別の WebView コントロールで、要求されたウィンドウのコンテンツを表示することもできます。

特別な機能を必要とする Web 機能を有効にするには、[PermissionRequested](/uwp/api/windows.ui.xaml.controls.webview.permissionrequested) イベントを使います。 現在、これらには位置情報、IndexedDB ストレージ、ユーザーのオーディオやビデオ (たとえば、マイクまたは Web カメラの機能) があります。 アプリがユーザーの位置情報またはユーザーのメディアにアクセスする場合も、アプリのマニフェストでそうした機能を宣言する必要があります。 たとえば、位置情報を使うアプリでは少なくとも Package.appxmanifest で次の機能の宣言が必要です。

```xml
  <Capabilities>
    <Capability Name="internetClient" />
    <DeviceCapability Name="location" />
  </Capabilities>
```

アプリによる [PermissionRequested](/uwp/api/windows.ui.xaml.controls.webview.permissionrequested) イベントの処理に加えて、これらの機能を有効にするには、位置情報やメディアの機能を要求するアプリに関する、標準的なシステム ダイアログをユーザーが承認する必要があります。

次の例は、アプリによって Bing のマップで位置情報がどのように有効されるかを示しています。

```csharp
// Assume webView is defined in XAML
webView.PermissionRequested += webView_PermissionRequested;

private void webView_PermissionRequested(WebView sender, WebViewPermissionRequestedEventArgs args)
{
    if (args.PermissionRequest.PermissionType == WebViewPermissionType.Geolocation &&
        args.PermissionRequest.Uri.Host == "www.bing.com")
    {
        args.PermissionRequest.Allow();
    }
}
```

アプリが許可の要求に応答するにあたってユーザーの入力をはじめ非同期の操作を要求する場合は、[WebViewPermissionRequest](/uwp/api/Windows.UI.Xaml.Controls.WebViewPermissionRequest) の [Defer](/uwp/api/windows.ui.xaml.controls.webviewpermissionrequest.defer) メソッドを使い、後で処理できる [WebViewDeferredPermissionRequest](/uwp/api/Windows.UI.Xaml.Controls.WebViewDeferredPermissionRequest) を作成します。 [WebViewPermissionRequest.Defer](/uwp/api/windows.ui.xaml.controls.webviewpermissionrequest.defer) に関するページをご覧ください。 

Web ビューでホストされている Web サイトからユーザーが安全にログアウトしなければならない場合や、セキュリティが重要であるような場合は、静的メソッドである [ClearTemporaryWebDataAsync](/uwp/api/windows.ui.xaml.controls.webview.cleartemporarywebdataasync) を呼び出し、当該の Web ビュー セッションでローカルにキャッシュされたコンテンツをすべて消去します。 これにより、悪意あるユーザーが重要なデータにアクセスするのを防ぎます。 

### <a name="interacting-with-web-view-content"></a>Web ビューのコンテンツとインタラクトする

[InvokeScriptAsync](/uwp/api/windows.ui.xaml.controls.webview.invokescriptasync) メソッドで Web ビューのコンテンツにスクリプトを呼び出し、または挿入して、[ScriptNotify](/uwp/api/windows.ui.xaml.controls.webview.scriptnotify) イベントで Web ビューのコンテンツから情報を反対に取得することで、Web ビューのコンテンツとインタラクトできます。

Web ビューのコンテンツ内で JavaScript を呼び出すには、[InvokeScriptAsync](/uwp/api/windows.ui.xaml.controls.webview.invokescriptasync) メソッドを使います。 呼び出されたスクリプトは、文字列型の値のみを返すことができます。 

たとえば、`webView1` という名前の Web ビューのコンテンツに、3 つのパラメーターを要する `setDate` という名前の関数が含まれている場合は、次のように呼び出すことができます。 

```csharp
string[] args = {"January", "1", "2000"};
string returnValue = await webView1.InvokeScriptAsync("setDate", args);
```


**InvokeScriptAsync** を JavaScript の **eval** 関数と共に使って、Web ページにコンテンツを挿入します。

ここでは、XAML のテキスト ボックス (`nameTextBox.Text`) のテキストは、`webView1` でホストされている HTML ページの div に書き込まれます。 

```csharp
private async void Button_Click(object sender, RoutedEventArgs e)
{
    string functionString = String.Format("document.getElementById('nameDiv').innerText = 'Hello, {0}';", nameTextBox.Text);
    await webView1.InvokeScriptAsync("eval", new string[] { functionString });
}
```

Web ビューのコンテンツのスクリプトは、文字列型パラメーターの **window.external.notify** を使えば、情報をアプリに戻せます。 これらのメッセージを受け取るには、[ScriptNotify](/uwp/api/windows.ui.xaml.controls.webview.scriptnotify) イベントを処理します。 

外部の Web ページを有効にして、window.external.notify を呼び出した際に **ScriptNotify** イベントを発生させるには、当該のページの URI をアプリの宣言の **ApplicationContentUriRules** セクションに含める必要があります (これは、Package.appxmanifest デザイナーの [コンテンツ URI] タブにある Microsoft Visual Studio で可能です)。この一覧にある URI は HTTPS を使う必要があります。また、サブドメインのワイルドカード (たとえば、`https://*.microsoft.com`) を含めることはできますが、ドメインのワイルドカード (たとえば、`https://*.com` や `https://*.*`) を含めることはできません。 マニフェスト要件は、アプリ パッケージから生成されたコンテンツには適用されず、ms-local-stream:// URI を使うか、[NavigateToString](/uwp/api/windows.ui.xaml.controls.webview.navigatetostring) を使って読み込まれるかのいずれかです。 

### <a name="accessing-the-windows-runtime-in-a-web-view"></a>Web ビューの Windows ランタイムにアクセスする

[AddWebAllowedObject](/uwp/api/windows.ui.xaml.controls.webview.addweballowedobject) メソッドを使うと、Windows ランタイム コンポーネントから Web ビューの JavaScript コンテンツにネイティブ クラスのインスタンスを挿入できます。 それにより、その Web ビューの JavaScript コンテンツにあるオブジェクトの、ネイティブのメソッドやプロパティ、イベントにフルにアクセスできるようになります。 クラスは、[AllowForWeb](/uwp/api/Windows.Foundation.Metadata.AllowForWebAttribute) 属性で修飾される必要があります。 

たとえば、次のコードでは、Windows ランタイム コンポーネントからインポートされた `MyClass` のインスタンスが Web ビューに挿入されます。

```csharp
private void webView_NavigationStarting(WebView sender, WebViewNavigationStartingEventArgs args) 
{ 
    if (args.Uri.Host == "www.contoso.com")  
    { 
        webView.AddWebAllowedObject("nativeObject", new MyClass()); 
    } 
}
```

詳しくは、[WebView.AddWebAllowedObject](/uwp/api/windows.ui.xaml.controls.webview.addweballowedobject) に関するページをご覧ください。 

さらに、Web ビューの信頼できる JavaScript コンテンツでは、Windows ランタイム API に直接アクセスすることも許可されています。 これにより、Web ビューでホストされている Web アプリの強力なネイティブ機能が利用できます。 この機能を有効にするには、WindowsRuntimeAccess を "all" に設定して、信頼できるコンテンツの URI が Package.appxmanifest のアプリの ApplicationContentUriRules でホワイトリスト化される必要があります。 

この例は、アプリ マニフェストのセクションを示しています。 ここでは、ローカル URI が Windows ランタイムへのアクセスを与えられます。 

```csharp
  <Applications>
    <Application Id="App"
      ...

      <uap:ApplicationContentUriRules>
        <uap:Rule Match="ms-appx-web:///Web/App.html" WindowsRuntimeAccess="all" Type="include"/>
      </uap:ApplicationContentUriRules>
    </Application>
  </Applications>
```

### <a name="options-for-web-content-hosting"></a>Web コンテンツのホスティングのオプション

[WebView.Settings](/uwp/api/windows.ui.xaml.controls.webview.settings) プロパティ ([WebViewSettings](/uwp/api/Windows.UI.Xaml.Controls.WebViewSettings) 型のプロパティ) を使うと、JavaScript と IndexedDB のオン/ オフをコントロールできます。 たとえば、Web ビューで完全に静的なコンテンツを表示するような場合は、JavaScript を無効にすることでパフォーマンスを高められます。

### <a name="capturing-web-view-content"></a>Web ビューのコンテンツをキャプチャする

Web ビューのコンテンツを他のアプリと共有できるようにするには、[CaptureSelectedContentToDataPackageAsync](/uwp/api/windows.ui.xaml.controls.webview.captureselectedcontenttodatapackageasync) メソッドを使います。このメソッドは、[DataPackage](/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackage) として選択したコンテンツを返します。 このメソッドは非同期であるため、[DataRequested](/uwp/api/windows.applicationmodel.datatransfer.datatransfermanager.datarequested) イベント ハンドラーが、非同期呼び出しが完了する前に戻されてしまうのを防ぐために、遅延を使用する必要があります。 

Web ビューの現在のコンテンツに関するプレビュー イメージを取得するには、[CapturePreviewToStreamAsync](/uwp/api/windows.ui.xaml.controls.webview.capturepreviewtostreamasync) メソッドを使います。 このメソッドは、現在のコンテンツのイメージを作成し、指定のストリームに書き込みます。 

### <a name="threading-behavior"></a>スレッド処理の動作

既定では、Web ビューのコンテンツは、デスクトップ デバイス ファミリのデバイス上の UI スレッドにホストされており、その他のデバイス上の UI スレッドからは分離されています。 [WebView.DefaultExecutionMode](/uwp/api/windows.ui.xaml.controls.webview.defaultexecutionmode) 静的プロパティを使うと、現在のクライアントに対する既定のスレッド処理動作を照会できます。 必要であれば、[WebView(WebViewExecutionMode)](/uwp/api/windows.ui.xaml.controls.webview.-ctor#Windows_UI_Xaml_Controls_WebView__ctor_Windows_UI_Xaml_Controls_WebViewExecutionMode_) コンストラクターを使ってその動作をオーバーライドすることもできます。 

> **注**&nbsp;&nbsp;モバイル デバイスの UI スレッドでコンテンツをホストしている場合は、パフォーマンス上の問題が発生する可能性があります。DefaultExecutionMode を変更するときは、対象となるすべてのデバイスを必ずテストしてください。

UI スレッドから外れてコンテンツをホストしている Web ビューは、[FlipView](/uwp/api/Windows.UI.Xaml.Controls.FlipView)、[ScrollViewer](/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer) といった、Web ビューのコントロールから親へと伝達するジェスチャを必要とする親コントロールや、その他の関連コントロールと互換性がありません。 そうしたコントロールは、オフスレッドの Web ビューで開始されるジェスチャを受け取ることができません。 さらに、オフスレッドの Web コンテンツの出力は、直接サポートされていません。つまり、要素は [WebViewBrush](/uwp/api/Windows.UI.Xaml.Controls.WebViewBrush) フィルで代わりに出力することになります。

## <a name="recommendations"></a>推奨事項


-   読み込まれた Web サイトがデバイスに応じて正しく書式設定されており、アプリの他の部分と一貫性のある色、文字体裁、ナビゲーションが使われていることを確認します。
-   入力フィールドのサイズを適切に調整する必要があります。 テキストを入力する際にズームインできることにユーザーが気付かない場合があります。
-   Web ビューがアプリの他の部分とは異なって見える場合は、関連タスクを実行するための代替のコントロールまたは手段を検討します。 Web ビューがアプリの他の部分にマッチしていると、すべてが 1 つのシームレスなエクスペリエンスとしてユーザーに認識してもらえます。

## <a name="get-the-sample-code"></a>サンプル コードを入手する

- [XAML コントロール ギャラリー サンプル](https://github.com/Microsoft/Xaml-Controls-Gallery) - インタラクティブな形で XAML コントロールのすべてを参照できます。

## <a name="related-topics"></a>関連トピック

- [WebView クラス](/uwp/api/Windows.UI.Xaml.Controls.WebView)