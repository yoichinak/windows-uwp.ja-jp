---
Description: アプリでハードウェア キーボードやソフトウェア キーボードからのキーボード操作に応答するには、キーボード イベント ハンドラーとクラス イベント ハンドラーの両方を使います。
title: キーボード イベント
ms.assetid: ac500772-d6ed-4a3a-825b-210a9c3c8f59
label: Keyboard events
template: detail.hbs
keywords: キーボード, ゲームパッド, リモート, アクセシビリティ, ナビゲーション, フォーカス, テキスト, 入力, ユーザーの操作, キーのリリース, キーの押下
ms.date: 03/29/2017
ms.topic: article
pm-contact: chigy
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 151abd02b34263cdd92b917127f306c25ebc5e0d
ms.sourcegitcommit: deb2867924ce16efcabfa011892157b7aa4fa2d2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89187839"
---
# <a name="keyboard-events"></a>キーボード イベント

## <a name="keyboard-events-and-focus"></a>キーボード イベントとフォーカス

次のキーボード イベントは、ハードウェア キーボードとタッチ キーボードの両方で発生します。

| Event                                      | 説明                    |
|--------------------------------------------|--------------------------------|
| [**KeyDown**](/uwp/api/windows.ui.xaml.uielement.keydown) | キーが押されたときに発生します。  |
| [**KeyUp**](/uwp/api/windows.ui.xaml.uielement.keyup)     | キーが離されたときに発生します。 |

> [!IMPORTANT]
> 一部の Windows ランタイム コントロールでは、入力イベントが内部で処理されます。 このような場合は、イベント リスナーに関連付けられているハンドラーが呼び出されないため、入力イベントが発生しないように見えることがあります。 通常、これらのキーのサブセットはクラス ハンドラーで処理され、基本的なキーボード アクセシビリティのビルトイン サポートが提供されます。 たとえば、[**Button**](/uwp/api/Windows.UI.Xaml.Controls.Button) クラスでは、Space キーと Enter キーの [**OnKeyDown**](/uwp/api/windows.ui.xaml.controls.control.onkeydown) イベント (および [**OnPointerPressed**](/uwp/api/windows.ui.xaml.controls.control.onpointerpressed)) がオーバーライドされ、コントロールの [**Click**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) イベントにルーティングされます。 キーの押下がコントロール クラスで処理された場合、[**KeyDown**](/uwp/api/windows.ui.xaml.uielement.keydown) イベントと [**KeyUp**](/uwp/api/windows.ui.xaml.uielement.keyup) イベントは発生しません。  
> これで、ボタンの実行に対応するキーボード操作が組み込まれ、ボタンを指でタップした場合やマウスでクリックした場合と同様の動作がサポートされます。 Space キーと Enter キー以外のキーについては、通常どおり [**KeyDown**](/uwp/api/windows.ui.xaml.uielement.keydown) と [**KeyUp**](/uwp/api/windows.ui.xaml.uielement.keyup) イベントが発生します。 クラス ベースのイベント処理の動作について詳しくは、「[イベントとルーティング イベントの概要](../../xaml-platform/events-and-routed-events-overview.md)」(特に「コントロールの入力イベント ハンドラー」セクション) をご覧ください。


UI のコントロールに入力フォーカスがあるときにだけ、キーボード イベントが生成されます。 個々のコントロールは、ユーザーがレイアウト上でコントロールを直接クリックまたはタップするか、Tab キーを使ってコンテンツ領域内のタブ順に入ると、フォーカスを取得します。

コントロールの [**Focus**](/uwp/api/windows.ui.xaml.controls.control.focus) メソッドを呼び出して、フォーカスを適用することもできます。 これは、UI が読み込まれたときに既定ではキーボード フォーカスが設定されないため、ショートカット キーを実装する場合に必要です。 詳しくは、このトピックの「**ショートカット キーの例**」をご覧ください。

コントロールが入力フォーカスを受け取るには、コントロールが有効にされ、表示されている必要があります。また、[**IsTabStop**](/uwp/api/windows.ui.xaml.controls.control.istabstop) プロパティ値と [**HitTestVisible**](/uwp/api/windows.ui.xaml.uielement.ishittestvisible) プロパティ値が **true** に設定されている必要もあります。 これは、ほとんどのコントロールの既定の状態です。 コントロールに入力フォーカスがあると、このトピックで後ほど説明するように、キーボード入力イベントを発生させ、応答することもできます。 また、[**GotFocus**](/uwp/api/windows.ui.xaml.uielement.gotfocus) イベントと [**LostFocus**](/uwp/api/windows.ui.xaml.uielement.lostfocus) イベントを処理して、フォーカスを受け取るコントロールやフォーカスを失うコントロールに応答することもできます。

既定では、コントロールのタブ順は、Extensible Application Markup Language (XAML) 内の出現順になっています。 ただし、[**TabIndex**](/uwp/api/windows.ui.xaml.controls.control.tabindex) プロパティを使って、この順序を変更できます。 詳細については、「 [キーボードアクセシビリティの実装](/previous-versions/windows/apps/hh868161(v=win.10))」を参照してください。

## <a name="keyboard-event-handlers"></a>キーボード イベント ハンドラー


入力イベント ハンドラーは、次の情報を提供するデリゲートを実装します。

-   イベントの送信元。 センダーは、イベント ハンドラーがアタッチされているオブジェクトを報告します。
-   イベント データ。 キーボード イベントの場合、イベント データは [**KeyRoutedEventArgs**](/uwp/api/Windows.UI.Xaml.Input.KeyRoutedEventArgs) のインスタンスです。 ハンドラーのデリゲートは [**KeyEventHandler**](/uwp/api/windows.ui.xaml.input.keyeventhandler) です。 ハンドラーに関するほとんどのシナリオで、最もよく使われる **KeyRoutedEventArgs** のプロパティは、[**Key**](/uwp/api/windows.ui.xaml.input.keyroutedeventargs.key) です。場合によっては、[**KeyStatus**](/uwp/api/windows.ui.xaml.input.keyroutedeventargs.keystatus) も使われます。
-   [**OriginalSource**](/uwp/api/windows.ui.xaml.routedeventargs.originalsource)。 キーボード イベントはルーティング イベントであるため、イベント データには **OriginalSource** があります。 イベントがオブジェクト ツリーをバブルアップするように意図的に設定した場合、**OriginalSource** がセンダーではなく対象のオブジェクトとなる場合があります。 ただし、この動作は設計によって異なります。 Sender ではなく **Originalsource** を使用する方法の詳細については、このトピックの「キーボードルーティングイベント」、または「 [イベントとルーティングイベントの概要](../../xaml-platform/events-and-routed-events-overview.md)」を参照してください。

### <a name="attaching-a-keyboard-event-handler"></a>キーボード イベント ハンドラーのアタッチ

イベントがメンバーとして含まれているオブジェクトに対して、キーボード イベント ハンドラー関数をアタッチできます。 [**UIElement**](/uwp/api/Windows.UI.Xaml.UIElement) 派生クラスにもアタッチできます。 次の XAML の例は、[**グリッド**](/uwp/api/Windows.UI.Xaml.Controls.Grid)の[**KeyUp**](/uwp/api/windows.ui.xaml.uielement.keyup)イベントのハンドラーをアタッチする方法を示しています。

```xaml
<Grid KeyUp="Grid_KeyUp">
  ...
</Grid>
```

コードを使ってイベント ハンドラーをアタッチすることもできます。 詳しくは、「[イベントとルーティング イベントの概要](../../xaml-platform/events-and-routed-events-overview.md)」をご覧ください。

### <a name="defining-a-keyboard-event-handler"></a>キーボード イベント ハンドラーの定義

次の例は、前の例でアタッチした [**KeyUp**](/uwp/api/windows.ui.xaml.uielement.keyup) イベント ハンドラーの定義の一部です。

```csharp
void Grid_KeyUp(object sender, KeyRoutedEventArgs e)
{
    //handling code here
}
```

```vb
Private Sub Grid_KeyUp(ByVal sender As Object, ByVal e As KeyRoutedEventArgs)
    ' handling code here
End Sub
```

```c++
void MyProject::MainPage::Grid_KeyUp(
  Platform::Object^ sender,
  Windows::UI::Xaml::Input::KeyRoutedEventArgs^ e)
  {
      //handling code here
  }
```

### <a name="using-keyroutedeventargs"></a>KeyRoutedEventArgs の使用

キーボード イベントはいずれもイベント データに [**KeyRoutedEventArgs**](/uwp/api/Windows.UI.Xaml.Input.KeyRoutedEventArgs) を使います。**KeyRoutedEventArgs** には次のプロパティがあります。

-   [**レジストリ**](/uwp/api/windows.ui.xaml.input.keyroutedeventargs.key)
-   [**KeyStatus**](/uwp/api/windows.ui.xaml.input.keyroutedeventargs.keystatus)
-   [**処理済み**](/uwp/api/windows.ui.xaml.input.keyroutedeventargs.handled)
-   [**OriginalSource**](/uwp/api/windows.ui.xaml.routedeventargs.originalsource) ([**RoutedEventArgs**](/uwp/api/Windows.UI.Xaml.RoutedEventArgs) から継承)

### <a name="key"></a>キー

キーが押されると、[**KeyDown**](/uwp/api/windows.ui.xaml.uielement.keydown) イベントが発生します。 同様に、キーが離されると、[**KeyUp**](/uwp/api/windows.ui.xaml.uielement.keyup) イベントが発生します。 通常、特定のキー値を処理するにはイベントをリッスンします。 押されたキーまたは離されたキーを特定するには、イベント データの [**Key**](/uwp/api/windows.ui.xaml.input.keyroutedeventargs.key) 値を調べます。 **Key** は [**VirtualKey**](/uwp/api/Windows.System.VirtualKey) 値を返します。 **VirtualKey** 列挙体には、サポートされているすべてのキーが含まれています。

### <a name="modifier-keys"></a>修飾キー

修飾キーは、Ctrl、Shift など、一般的に他のキーと組み合わせて押されるキーです。 アプリでは、これらのキーの組み合わせをキーボード ショートカットとして使って、アプリ コマンドを呼び出すことができます。

ショートカットキーの組み合わせは、 [**KeyDown**](/uwp/api/windows.ui.xaml.uielement.keydown) イベントハンドラーと [**KeyUp**](/uwp/api/windows.ui.xaml.uielement.keyup) イベントハンドラーで検出できます。 非修飾キーに対してキーボードイベントが発生した場合は、修飾子キーが押された状態であるかどうかを確認できます。

または、 [**corewindow**](/uwp/api/windows.ui.core.corewindow)の[**getkeystate ()**](/uwp/api/windows.ui.core.corewindow.getkeystate)関数を使用[**して、**](/uwp/api/windows.ui.core.corewindow.getforcurrentthread)非修飾キーが押されたときに、修飾子の状態を確認することもできます。

次の例では、この2番目のメソッドを実装していますが、最初の実装のスタブコードも含まれています。

> [!NOTE]
> Alt キーは **VirtualKey.Menu** 値で表されます。

### <a name="shortcut-keys-example"></a>ショートカット キーの例

ショートカット キーを実装する方法を次の例で示します。 この例では、ユーザーは [Play]、[Pause]、[Stop] の各ボタンまたは Ctrl + P、Ctrl + A、Ctrl + S の各キーボード ショートカットを使って、メディアの再生を制御できます。 ボタンの XAML では、ボタン ラベルのヒントや [**AutomationProperties**](/uwp/api/Windows.UI.Xaml.Automation.AutomationProperties) プロパティを使って、ショートカット キーを示します。 このアプリ内の説明は、アプリの操作性とアクセシビリティを向上させるために重要です。 詳しくは、「[キーボードのアクセシビリティ](../accessibility/keyboard-accessibility.md)」をご覧ください。

ページが読み込まれるときに、入力フォーカスをページそのものに設定していることにも注目してください。 この手順を実行しなければ、最初の入力フォーカスがどのコントロールにも設定されず、ユーザーが手動で入力フォーカスを設定する (Tab キーを使ってコントロールを選ぶ、コントロールをクリックするなど) までアプリで入力イベントが発生しません。

```xaml
<Grid KeyDown="Grid_KeyDown">

  <Grid.RowDefinitions>
    <RowDefinition Height="Auto" />
    <RowDefinition Height="Auto" />
  </Grid.RowDefinitions>

  <MediaElement x:Name="DemoMovie" Source="xbox.wmv"
    Width="500" Height="500" Margin="20" HorizontalAlignment="Center" />

  <StackPanel Grid.Row="1" Margin="10"
    Orientation="Horizontal" HorizontalAlignment="Center">

    <Button x:Name="PlayButton" Click="MediaButton_Click"
      ToolTipService.ToolTip="Shortcut key: Ctrl+P"
      AutomationProperties.AcceleratorKey="Control P">
      <TextBlock>Play</TextBlock>
    </Button>

    <Button x:Name="PauseButton" Click="MediaButton_Click"
      ToolTipService.ToolTip="Shortcut key: Ctrl+A"
      AutomationProperties.AcceleratorKey="Control A">
      <TextBlock>Pause</TextBlock>
    </Button>

    <Button x:Name="StopButton" Click="MediaButton_Click"
      ToolTipService.ToolTip="Shortcut key: Ctrl+S"
      AutomationProperties.AcceleratorKey="Control S">
      <TextBlock>Stop</TextBlock>
    </Button>

  </StackPanel>

</Grid>
```

```c++
//showing implementations but not header definitions
void MainPage::OnNavigatedTo(NavigationEventArgs^ e)
{
    (void) e;    // Unused parameter
    this->Loaded+=ref new RoutedEventHandler(this,&amp;MainPage::ProgrammaticFocus);
}
void MainPage::ProgrammaticFocus(Object^ sender, RoutedEventArgs^ e) 
{
    this->Focus(Windows::UI::Xaml::FocusState::Programmatic);
}

void KeyboardSupport::MainPage::MediaButton_Click(Platform::Object^ sender, Windows::UI::Xaml::RoutedEventArgs^ e)
{
    FrameworkElement^ fe = safe_cast<FrameworkElement^>(sender);
    if (fe->Name == "PlayButton") {DemoMovie->Play();}
    if (fe->Name == "PauseButton") {DemoMovie->Pause();}
    if (fe->Name == "StopButton") {DemoMovie->Stop();}
}


bool KeyboardSupport::MainPage::IsCtrlKeyPressed()
{
    auto ctrlState = CoreWindow::GetForCurrentThread()->GetKeyState(VirtualKey::Control);
    return (ctrlState & CoreVirtualKeyStates::Down) == CoreVirtualKeyStates::Down;
}

void KeyboardSupport::MainPage::Grid_KeyDown(Platform::Object^ sender, Windows::UI::Xaml::Input::KeyRoutedEventArgs^ e)
{
    if (e->Key == VirtualKey::Control) isCtrlKeyPressed = true;
}


void KeyboardSupport::MainPage::Grid_KeyUp(Platform::Object^ sender, Windows::UI::Xaml::Input::KeyRoutedEventArgs^ e)
{
    if (IsCtrlKeyPressed()) 
    {
        if (e->Key==VirtualKey::P) { DemoMovie->Play(); }
        if (e->Key==VirtualKey::A) { DemoMovie->Pause(); }
        if (e->Key==VirtualKey::S) { DemoMovie->Stop(); }
    }
}
```

```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    // Set the input focus to ensure that keyboard events are raised.
    this.Loaded += delegate { this.Focus(FocusState.Programmatic); };
}

private void MediaButton_Click(object sender, RoutedEventArgs e)
{
    switch ((sender as Button).Name)
    {
        case "PlayButton": DemoMovie.Play(); break;
        case "PauseButton": DemoMovie.Pause(); break;
        case "StopButton": DemoMovie.Stop(); break;
    }
}

private static bool IsCtrlKeyPressed()
{
    var ctrlState = CoreWindow.GetForCurrentThread().GetKeyState(VirtualKey.Control);
    return (ctrlState & CoreVirtualKeyStates.Down) == CoreVirtualKeyStates.Down;
}

private void Grid_KeyDown(object sender, KeyRoutedEventArgs e)
{
    if (IsCtrlKeyPressed())
    {
        switch (e.Key)
        {
            case VirtualKey.P: DemoMovie.Play(); break;
            case VirtualKey.A: DemoMovie.Pause(); break;
            case VirtualKey.S: DemoMovie.Stop(); break;
        }
    }
}
```

```VisualBasic
Private isCtrlKeyPressed As Boolean
Protected Overrides Sub OnNavigatedTo(e As Navigation.NavigationEventArgs)

End Sub

Private Function IsCtrlKeyPressed As Boolean
    Dim ctrlState As CoreVirtualKeyStates = CoreWindow.GetForCurrentThread().GetKeyState(VirtualKey.Control);
    Return (ctrlState & CoreVirtualKeyStates.Down) == CoreVirtualKeyStates.Down;
End Function

Private Sub Grid_KeyDown(sender As Object, e As KeyRoutedEventArgs)
    If IsCtrlKeyPressed() Then
        Select Case e.Key
            Case Windows.System.VirtualKey.P
                DemoMovie.Play()
            Case Windows.System.VirtualKey.A
                DemoMovie.Pause()
            Case Windows.System.VirtualKey.S
                DemoMovie.Stop()
        End Select
    End If
End Sub

Private Sub MediaButton_Click(sender As Object, e As RoutedEventArgs)
    Dim fe As FrameworkElement = CType(sender, FrameworkElement)
    Select Case fe.Name
        Case "PlayButton"
            DemoMovie.Play()
        Case "PauseButton"
            DemoMovie.Pause()
        Case "StopButton"
            DemoMovie.Stop()
    End Select
End Sub
```

> [!NOTE]
> XAML で [**AutomationProperties.AcceleratorKey**](/dotnet/api/system.windows.automation.automationproperties.acceleratorkey) または [**AutomationProperties.AccessKey**](/dotnet/api/system.windows.automation.automationproperties.accesskey) を設定すると、(その特定の操作を呼び出すためのショートカット キーを説明する) 文字列情報が提供されます。 この情報は、ナレーターなどの Microsoft UI オートメーション クライアントによってキャプチャされ、通常は、直接ユーザーに提供されます。
>
> **AutomationProperties.AcceleratorKey** または **AutomationProperties.AccessKey** を設定しても、それだけでは操作は実行されません。 実際にアプリにキーボード ショートカットの動作を実装するには、[**KeyDown**](/uwp/api/windows.ui.xaml.uielement.keydown) イベントまたは [**KeyUp**](/uwp/api/windows.ui.xaml.uielement.keyup) イベントのハンドラーをアタッチする必要があります。 また、アクセス キーの下線も自動的には追加されません。 UI で下線付きのテキストを表示する場合は、インラインの [**Underline**](/uwp/api/Windows.UI.Xaml.Documents.Underline) 書式設定として、ニーモニックの特定のキーのテキストに明示的に下線を表示する必要があります。

 

## <a name="keyboard-routed-events"></a>キーボード ルーティング イベント


[**KeyDown**](/uwp/api/windows.ui.xaml.uielement.keydown) や、[**KeyUp**](/uwp/api/windows.ui.xaml.uielement.keyup) などの特定のイベントがルーティング イベントです。 ルーティング イベントでは、バブル ルーティング方式が採用されています。 バブル ルーティング方式では、子オブジェクトで発生したイベントが、オブジェクト ツリー内で上位にある親オブジェクトに順にルーティング (バブルアップ) されます。 つまり、同じイベントを処理し、同じイベント データを操作する機会が増えることを意味します。

次の XAML の例では、1 つの [**Canvas**](/uwp/api/Windows.UI.Xaml.Controls.Canvas) と 2 つの [**Button**](/uwp/api/Windows.UI.Xaml.Controls.Button) オブジェクトについて、[**KeyUp**](/uwp/api/windows.ui.xaml.uielement.keyup) イベントを処理します。 この場合、どちらかの **Button** オブジェクトにフォーカスがある間にキーを離すと、**KeyUp** イベントが発生します。 次に、イベントが親 **キャンバス**に送られます。

```xaml
<StackPanel KeyUp="StackPanel_KeyUp">
  <Button Name="ButtonA" Content="Button A"/>
  <Button Name="ButtonB" Content="Button B"/>
  <TextBlock Name="statusTextBlock"/>
</StackPanel>
```

次の例は、前に示した XAML コンテンツに対応する [**KeyUp**](/uwp/api/windows.ui.xaml.uielement.keyup) イベント ハンドラーの実装方法を示しています。

```csharp
void StackPanel_KeyUp(object sender, KeyRoutedEventArgs e)
{
    statusTextBlock.Text = String.Format(
        "The key {0} was pressed while focus was on {1}",
        e.Key.ToString(), (e.OriginalSource as FrameworkElement).Name);
}
```

このハンドラーで [**OriginalSource**](/uwp/api/windows.ui.xaml.routedeventargs.originalsource) プロパティが使われていることに注意してください。 この例では、**OriginalSource** がイベントの発生元のオブジェクトを報告します。 **StackPanel** はコントロールではなく、フォーカスを受け取ることもできないため、[**StackPanel**](/uwp/api/Windows.UI.Xaml.Controls.StackPanel) がこのオブジェクトになることはありません。 **StackPanel** 内の 2 つのボタンのどちらか 1 つのみがイベントの発生元である可能性がありますが、発生元を調べるにはどうすればよいでしょうか。 親オブジェクトのイベントを処理する場合は、**OriginalSource** を使って、実際のイベント ソース オブジェクトを判別します。

### <a name="the-handled-property-in-event-data"></a>イベント データ内の Handled プロパティ

イベント処理の方針によっては、1 つのイベント ハンドラーだけがバブル イベントに応答するようにした方がよい場合もあります。 たとえば、[**Button**](/uwp/api/Windows.UI.Xaml.Controls.Button) コントロールの 1 つに、特定の [**KeyUp**](/uwp/api/windows.ui.xaml.uielement.keyup) ハンドラーをアタッチすると、最初にそのイベントを処理するのは、このハンドラーがアタッチされたコントロールになります。 このとき、親パネルではイベントが処理されないようにします。 このようなシナリオでは、イベント データ内の [**Handled**](/uwp/api/windows.ui.xaml.input.keyroutedeventargs.handled) プロパティを使います。

ルーティング イベント データ クラスの [**Handled**](/uwp/api/windows.ui.xaml.input.keyroutedeventargs.handled) プロパティは、イベント ルート上で先に登録された別のハンドラーが既に処理を実行したことを示すためのプロパティで、 ルーティング イベント システムの動作に影響します。 イベント ハンドラー内で **Handled** を **true** に設定すると、そのイベントのルーティングはそこで終了し、上位の親要素にイベントは送信されません。

### <a name="addhandler-and-already-handled-keyboard-events"></a>AddHandler イベントと処理済みキーボード イベント

特殊な方法を利用して、既に処理済みとしてマークされているイベントに対して処理を実行できるハンドラーをアタッチすることができます。 この手法では、 [**AddHandler**](/uwp/api/windows.ui.xaml.uielement.addhandler) メソッドを使用してハンドラーを登録します。これには、XAML 属性や、C の + = などのハンドラーを追加するための言語固有の構文は使用し \# ません。

一般的に、この方法には、**AddHandler** API が [**RoutedEvent**](/uwp/api/Windows.UI.Xaml.RoutedEvent) 型のパラメーターを受け取るという制限があります。このパラメーターで対象のルーティング イベントを識別します。 一部のルーティング イベントには **RoutedEvent** 識別子がないため、[**Handled**](/uwp/api/windows.ui.xaml.input.keyroutedeventargs.handled) の状態でも処理できるルーティング イベントを識別する場合に考慮する必要があります。 [**UIElement**](/uwp/api/Windows.UI.Xaml.UIElement) の [**KeyDown**](/uwp/api/windows.ui.xaml.uielement.keydown) イベントと [**KeyUp**](/uwp/api/windows.ui.xaml.uielement.keyup) イベントには、ルーティング イベント識別子 ([**KeyDownEvent**](/uwp/api/windows.ui.xaml.uielement.keydownevent) と [**KeyUpEvent**](/uwp/api/windows.ui.xaml.uielement.keyupevent)) があります。 これに対し、[**TextBox.TextChanged**](/uwp/api/windows.ui.xaml.controls.textbox.textchanged) などのイベントにはルーティング イベント識別子がないため、**AddHandler** を使う手法には利用できません。

### <a name="overriding-keyboard-events-and-behavior"></a>キーボード イベントと動作のオーバーライド

特定のコントロールのキー イベント (たとえば [**GridView**](/uwp/api/Windows.UI.Xaml.Controls.GridView) など) をオーバーライドして、キーボードとゲームパッドを含むさまざまな入力デバイスに一貫したフォーカス ナビゲーションを提供できます。

次の例では、コントロールをサブクラス化し、KeyDown 動作をオーバーライドして、矢印キーが押されたときに GridView の内容にフォーカスを移動します。

```csharp
  public class CustomGridView : GridView
  {
    protected override void OnKeyDown(KeyRoutedEventArgs e)
    {
      // Override arrow key behaviors.
      if (e.Key != Windows.System.VirtualKey.Left && e.Key !=
        Windows.System.VirtualKey.Right && e.Key !=
          Windows.System.VirtualKey.Down && e.Key !=
            Windows.System.VirtualKey.Up)
              base.OnKeyDown(e);
      else
        FocusManager.TryMoveFocus(FocusNavigationDirection.Down);
    }
  }
```

> [!NOTE]
> GridView がレイアウトのみに使用されている場合には、[**ItemsControl**](/uwp/api/Windows.UI.Xaml.Controls.ItemsControl) と [**ItemsWrapGrid**](/uwp/api/Windows.UI.Xaml.Controls.ItemsWrapGrid) などの、他のコントロールの使用を検討します。

## <a name="commanding"></a>コマンド実行

ごく一部の UI 要素では、コマンド実行が組み込みでサポートされています。 コマンド実行の基になる実装では、入力に関連するルーティング イベントを使います。 この方法では、1 つのコマンド ハンドラーを呼び出して、特定のポインター操作、特定のショートカット キーなどの関連する UI 入力を処理できます。

UI 要素でコマンド実行を使うことができる場合は、個々の入力イベントではなく、コマンド実行 API を使うことを検討してください。 詳細については、「 [**Buttonbase. コマンド**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.command)」を参照してください。

[**ICommand**](/uwp/api/Windows.UI.Xaml.Input.ICommand) を実装して、通常のイベント ハンドラーから呼び出すコマンド機能をカプセル化することもできます。 この方法では、**Command** プロパティを利用できない場合でも、コマンド実行を使うことができます。

## <a name="text-input-and-controls"></a>テキスト入力とコントロール

キーボード イベントに固有の処理で対応するコントロールもあります。 たとえば、[**TextBox**](/uwp/api/Windows.UI.Xaml.Controls.TextBox) は、キーボードを使って入力されたテキストをキャプチャし、表示するためのコントロールです。 このコントロールでは、キーボード操作をキャプチャするために、固有のロジックで [**KeyUp**](/uwp/api/windows.ui.xaml.uielement.keyup) と [**KeyDown**](/uwp/api/windows.ui.xaml.uielement.keydown) が使われます。また、テキストが実際に変化した場合には、固有の [**TextChanged**](/uwp/api/windows.ui.xaml.controls.textbox.textchanged) イベントを発生させます。

また、[**TextBox**](/uwp/api/Windows.UI.Xaml.Controls.TextBox) などのテキスト入力の処理を目的としたコントロールに、[**KeyUp**](/uwp/api/windows.ui.xaml.uielement.keyup) や [**KeyDown**](/uwp/api/windows.ui.xaml.uielement.keydown) のハンドラーを追加することも通常どおりできます。 ただし、コントロールは、その設計上、キー イベントを通じて伝達されたすべてのキー値に応答するわけではありません。 動作はコントロールによって異なります。

たとえば、[**ButtonBase**](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ButtonBase) ([**Button**](/uwp/api/Windows.UI.Xaml.Controls.Button) の基底クラス) では、Space キーや Enter キーの操作を確認するために [**KeyUp**](/uwp/api/windows.ui.xaml.uielement.keyup) が処理されます。 **ButtonBase** では **KeyUp** を、[**Click**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) イベントを発生させるマウスの左ボタンを押す操作と同等と見なします。 このイベント処理は、**ButtonBase** が仮想メソッド [**OnKeyUp**](/uwp/api/windows.ui.xaml.controls.control.onkeyup) をオーバーライドする際に実現されます。 この実装では、[**Handled**](/uwp/api/windows.ui.xaml.input.keyroutedeventargs.handled) が **true** に設定されます。 このため、あるボタンの親がキー イベント (たとえば Space バー) をリッスンしていても、既に処理されたイベントをハンドラーで受け取ることはありません。

別の例としては、[**TextBox**](/uwp/api/Windows.UI.Xaml.Controls.TextBox) があります。 方向キーなど、一部のキーはテキスト **ボックス** では考慮されず、コントロール UI の動作に固有であると見なされます。 **TextBox** は、これらのイベント ケースを処理済みとしてマークします。

カスタムコントロールは、 [**OnKeyDown**](/uwp/api/windows.ui.xaml.controls.control.onkeydown)OnKeyUp をオーバーライドすることによって、キーイベントに対して同様のオーバーライド動作を実装でき  /  [**OnKeyUp**](/uwp/api/windows.ui.xaml.controls.control.onkeyup)ます。 カスタム コントロールで特定のショートカット キーを処理する場合、または [**TextBox**](/uwp/api/Windows.UI.Xaml.Controls.TextBox) の説明で示したシナリオのようなコントロールの動作またはフォーカスの動作を使う場合、**OnKeyDown** / **OnKeyUp** のオーバーライドにこのロジックを組み込む必要があります。

## <a name="the-touch-keyboard"></a>タッチ キーボード

テキスト入力コントロールでは、タッチ キーボードが自動的にサポートされます。 ユーザーがタッチ入力を使って、テキスト コントロールに入力フォーカスを設定すると、タッチ キーボードが自動的に表示されます。 入力フォーカスがテキスト コントロールにないときには、タッチ キーボードが表示されません。

タッチ キーボードが表示されると、フォーカスがある要素をユーザーが見ることができるように、UI が自動的に再配置されます。 この場合、他の重要な UI 領域が画面の表示領域外に移動することがあります。 ただし、タッチ キーボードが表示されたときの既定の動作を無効にして、独自に UI を調整することができます。 詳しくは、[タッチ キーボードのサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/TouchKeyboard)をご覧ください。

テキスト入力を必要とするカスタム コントロールを、標準のテキスト入力コントロールからの派生コントロールとして作成しない場合は、適切な UI オートメーション コントロール パターンを実装してタッチ キーボードを追加し、サポートできます。 詳しくは、[タッチ キーボードのサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/TouchKeyboard)をご覧ください。

タッチ キーボードのキーが押されると、ハードウェア キーボードのキーが押されたとき同様に、[**KeyDown**](/uwp/api/windows.ui.xaml.uielement.keydown) イベントと [**KeyUp**](/uwp/api/windows.ui.xaml.uielement.keyup) イベントが発生します。 ただし、タッチ キーボードでは、Ctrl + A、Ctrl + Z、Ctrl + X、Ctrl + C、Ctrl + V に対応する入力イベントは発生しません。これらは、入力コントロールでテキストを操作するために予約されています。

ユーザーが入力すると予想されるデータの種類と一致するようにテキスト コントロールの入力値の種類を設定することで、ユーザーがより速く簡単にアプリにデータを入力できるようになります。 入力値の種類は、コントロールが予期しているテキスト入力の種類のヒントとなるため、システムが、その入力の種類用の特殊なタッチ キーボード レイアウトを提供できます。 たとえば、テキストボックスが4桁の PIN を入力するためだけに使用されている場合は、 [**Inputscope**](/uwp/api/windows.ui.xaml.controls.textbox.inputscope) プロパティを [**Number**](/uwp/api/Windows.UI.Xaml.Input.InputScopeNameValue)に設定します。 これにより、システムに数字キーパッド レイアウトの表示が指示されるため、ユーザーは簡単に PIN を入力できます。 詳しくは、「[入力値の種類を使ったタッチ キーボードの変更](./use-input-scope-to-change-the-touch-keyboard.md)」をご覧ください。

## <a name="related-articles"></a>関連記事

### <a name="developers"></a>開発者

- [キーボード操作](keyboard-interactions.md)
- [入力デバイスの識別](identify-input-devices.md)
- [タッチ キーボードの表示への応答](respond-to-the-presence-of-the-touch-keyboard.md)

### <a name="designers"></a>デザイナー

- [キーボードの設計ガイドライン](./keyboard-interactions.md)

### <a name="samples"></a>サンプル

- [タッチ キーボードのサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/TouchKeyboard)
- [基本的な入力のサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
- [待機時間が短い入力のサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
- [フォーカスの視覚効果のサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals)

### <a name="archive-samples"></a>サンプルのアーカイブ

- [入力サンプル](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20XAML%20user%20input%20events%20sample)
- [入力: デバイス機能のサンプルに関するページ](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Input%20Device%20capabilities%20sample%20(Windows%208))
- [入力: タッチ キーボードのサンプルに関するページ](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Input%20Touch%20keyboard%20sample%20(Windows%208))
- [スクリーン キーボードを表示したときの対応のサンプルのページ](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Responding%20to%20the%20appearance%20of%20the%20on-screen%20keyboard%20sample)
- [XAML テキスト編集のサンプルに関するページ](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BVB%5D-Windows%208%20app%20samples/VB/Windows%208%20app%20samples/XAML%20text%20editing%20sample%20(Windows%208))
