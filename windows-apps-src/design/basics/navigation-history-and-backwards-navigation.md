---
description: Windows アプリ内でのユーザーのナビゲーション履歴を横断して実行される、前に戻る移動を実装する方法について説明します。
title: ナビゲーション履歴と前に戻る移動
template: detail.hbs
op-migration-status: ready
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
ms.openlocfilehash: 738efe43afaf5c278425d5636bddc72cecadf47a
ms.sourcegitcommit: d51c3dd64d58c7fa9513ba20e736905f12df2a9a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2021
ms.locfileid: "98988764"
---
# <a name="navigation-history-and-backwards-navigation-for-windows-apps"></a>Windows アプリでのナビゲーション履歴と前に戻る移動

> **重要な API**:[BackRequested イベント](/uwp/api/Windows.UI.Core.SystemNavigationManager.BackRequested)、[SystemNavigationManager クラス](/uwp/api/Windows.UI.Core.SystemNavigationManager)、[OnNavigatedTo](/uwp/api/windows.ui.xaml.controls.page.onnavigatedto)

Windows アプリでは、アプリ内でのユーザーのナビゲーション履歴の横断や、デバイスによってはアプリ間の移動が可能な、一貫性のある "戻る" ナビゲーション システムが提供されています。

アプリに前に戻る移動を実装するには、アプリの UI の左上隅に戻るボタンを配置します。 ユーザーは、戻るボタンによって、アプリのナビゲーション履歴における前の場所に戻ることを想定しています。 ナビゲーション履歴に追加するナビゲーション操作の種類、および戻るボタンが押されたときの応答方法は、自由に決めることができます。

複数ページのほとんどのアプリでは、[NavigationView](../controls-and-patterns/navigationview.md) コントロールを使用して、アプリのナビゲーション フレームワークを提供することをお勧めします。 これは、さまざまな画面サイズに適応し、_上部_ のナビゲーション スタイルと _左側_ のナビゲーション スタイルの両方をサポートしています。 アプリで `NavigationView` コントロールを使用する場合は、[NavigationView の組み込みの戻るボタン](../controls-and-patterns/navigationview.md#backwards-navigation)を使用できます。

> [!NOTE]
> この記事のガイドラインと例は、`NavigationView` コントロールを使用しないナビゲーションを実装する場合に使用する必要があります。 `NavigationView` を使用する場合は、この情報は背景知識としては役に立ちますが、具体的なガイダンスと例については [NavigationView](../controls-and-patterns/navigationview.md) に関する記事のものを使用する必要があります

## <a name="back-button"></a>[戻る] ボタン

戻るボタンを作成するには、`NavigationBackButtonNormalStyle` スタイルの [Button](../controls-and-patterns/buttons.md) コントロールを使用し、アプリの UI の左上隅にボタンを配置します (詳しくは、後の XAML コードの例をご覧ください)。

![アプリの UI の左上隅にある [戻る] ボタン](images/back-nav/BackEnabled.png)

```xaml
<Page>
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>

        <Button x:Name="BackButton"
                Style="{StaticResource NavigationBackButtonNormalStyle}"
                IsEnabled="{x:Bind Frame.CanGoBack, Mode=OneWay}" 
                ToolTipService.ToolTip="Back"/>

    </Grid>
</Page>
```

アプリに上部 [CommandBar](../controls-and-patterns/app-bars.md) がある場合、高さ 44px の Button コントロールは 48px の AppBarButtons とはぴったり合いません。 不整合を避けるために、Button コントロールの最上部を 48px の境界内部に合わせてください。

![上部のコマンド バーの [戻る] ボタン](images/back-nav/CommandBar.png)

```xaml
<Page>
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        
        <CommandBar>
            <CommandBar.Content>
                <Button x:Name="BackButton"
                        Style="{StaticResource NavigationBackButtonNormalStyle}"
                        IsEnabled="{x:Bind Frame.CanGoBack, Mode=OneWay}" 
                        ToolTipService.ToolTip="Back" 
                        VerticalAlignment="Top"/>
            </CommandBar.Content>
        
            <AppBarButton Icon="Delete" Label="Delete"/>
            <AppBarButton Icon="Save" Label="Save"/>
        </CommandBar>
    </Grid>
</Page>
```

アプリ内を移動できる UI 要素を最小化するため、バックスタックに何もないときに、無効になった戻るボタンを表示します (`IsEnabled="{x:Bind Frame.CanGoBack, Mode=OneWay}"`)。 ただし、アプリにバックスタックがないことが予想される場合は、戻るボタンを表示する必要はありません。

![[戻る] ボタンの状態](images/back-nav/BackDisabled.png)

## <a name="optimize-for-different-devices-and-inputs"></a>さまざまなデバイスと入力に合わせて最適化する

この後方ナビゲーション設計ガイダンスは、すべてのデバイスに適用できますが、異なるフォーム ファクターや入力方法に合わせて最適化すると、ユーザーにメリットがあります。

UI を最適化するには:

- **デスクトップ/ハブ**: アプリの UI の左上隅にアプリ内の戻るボタンを描画します。
- **[タブレット モード](https://support.microsoft.com/windows/use-your-pc-like-a-tablet-4fbfcca5-f058-814a-4f80-a12e703d7c34)** : ハードウェアまたはソフトウェアの戻るボタンがタブレットに表示される場合がありますが、わかりやすくするため、アプリ内で戻るボタンを描画することをお勧めします。
- **Xbox/テレビ**: 不要な UI 要素が追加されるため、戻るボタンは描画しません。 代わりに、ゲームパッドの B ボタンで前に戻ります。

アプリが Xbox で実行される場合は、[Xbox 用の視覚的なカスタム トリガーを作成](../devices/designing-for-tv.md#custom-visual-state-trigger-for-xbox)して、ボタンを表示するかどうかを切り替えます。 [NavigationView](../controls-and-patterns/navigationview.md) コントロールを使用している場合、アプリが Xbox で実行されているときは、戻るボタンの表示が自動的に切り替わります。

戻るナビゲーションの最も一般的な入力をサポートするため、(戻るボタンのクリックに加えて) 次のイベントを処理することをお勧めします。

| イベント | 入力 |
| --- | --- |
| [CoreDispatcher.AcceleratorKeyActivated](/uwp/api/windows.ui.core.coredispatcher.acceleratorkeyactivated) | Alt + 左方向キー、<br/>VirtualKey.GoBack |
| [SystemNavigationManager.BackRequested](/api/windows.ui.core.systemnavigationmanager.backrequested) | Windows + Backspace、<br/>ゲームパッドの B ボタン、<br/>タブレット モードの戻るボタン、<br/>ハードウェアの戻るボタン |
| [CoreWindow.PointerPressed](/uwp/api/windows.ui.core.corewindow.pointerpressed) | VirtualKey.XButton1<br/>(一部のマウスにある戻るボタンなど。) |

## <a name="code-examples"></a>コード例

このセクションでは、さまざまな入力を使用して戻るナビゲーションを処理する方法を示します。

### <a name="back-button-and-back-navigation"></a>戻るボタンと戻るナビゲーション

少なくとも、戻るボタンの `Click` イベントを処理し、戻るナビゲーションを実行するためのコードを提供する必要があります。 また、バックスタックが空のときは、戻るボタンを無効にする必要があります。

この例のコードでは、戻るボタンでの後方ナビゲーション動作を実装する方法を示します。 このコードは、Button の [Click](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.Click) イベントに応答して移動します。 戻るボタンは、新しいページに移動するときに呼び出される [OnNavigatedTo](/uwp/api/windows.ui.xaml.controls.page.onnavigatedto) メソッドで有効または無効にされます。

示されているコードは `MainPage` のものですが、戻るナビゲーションをサポートするページごとにこのコードを追加します。 重複を避けるには、`App.xaml.*` 分離コード ページの `App` クラスに、ナビゲーション関連のコードを配置できます。

```xaml
<!-- MainPage.xaml -->
<Page x:Class="AppName.MainPage">
...
        <Button x:Name="BackButton" Click="BackButton_Click"
                Style="{StaticResource NavigationBackButtonNormalStyle}"
                IsEnabled="{x:Bind Frame.CanGoBack, Mode=OneWay}" 
                ToolTipService.ToolTip="Back"/>
...
<Page/>
```

分離コード:

```csharp
// MainPage.xaml.cs
private void BackButton_Click(object sender, RoutedEventArgs e)
{
    App.TryGoBack();
}

// App.xaml.cs
//
// Add this method to the App class.
public static bool TryGoBack()
{
    Frame rootFrame = Window.Current.Content as Frame;
    if (rootFrame.CanGoBack)
    {
        rootFrame.GoBack();
        return true;
    }
    return false;
}
```

```cppwinrt
// MainPage.h
namespace winrt::AppName::implementation
{
    struct MainPage : MainPageT<MainPage>
    {
        MainPage();
 
        void MainPage::BackButton_Click(IInspectable const&, RoutedEventArgs const&)
        {
            App::TryGoBack();
        }
    };
}

// App.h
#include "winrt/Windows.UI.Core.h"
#include "winrt/Windows.System.h"
#include "winrt/Windows.UI.Input.h"
#include "winrt/Windows.UI.Xaml.Input.h"
 
using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::UI::Core;
using namespace Windows::UI::Input;
using namespace Windows::UI::Xaml;
using namespace Windows::UI::Xaml::Controls;

struct App : AppT<App>
{
    App();

    // ...

    // Perform back navigation if possible.
    static bool TryGoBack()
    {
        Frame rootFrame{ nullptr };
        auto content = Window::Current().Content();
        if (content)
        {
            rootFrame = content.try_as<Frame>();
            if (rootFrame.CanGoBack())
            {
                rootFrame.GoBack();
                return true;
            }
        }
        return false;
    }
};
```

### <a name="support-access-keys"></a>アクセス キーをサポートする

さまざまなスキル、能力、期待を持ったユーザーにとってアプリケーションが十分に機能するために、キーボードのサポートは重要です。 アクセラレータ キーを使用するユーザーは前方移動と後方移動の両方があるものと考えるので、両方をサポートすることをお勧めします。 詳細については、「[キーボード操作](..\input\keyboard-interactions.md)」と「[キーボード アクセラレータ](..\input\keyboard-accelerators.md)」を参照してください。

前方移動と後方移動のための一般的なアクセラレータ キーは、Alt + 右方向キー (前方) と Alt + 左方向キー (後方) です。 これらのキーをナビゲーション用にサポートするには、[CoreDispatcher.AcceleratorKeyActivated](/uwp/api/windows.ui.core.coredispatcher.acceleratorkeyactivated) イベントを処理します。 フォーカスがある要素に関係なくアプリがアクセラレータ キーに応答するよう、(ページ上の要素ではなく) ウィンドウで直接イベントを処理します。

アクセラレータ キーと前方ナビゲーションをサポートするには、次に示すように、`App` クラスにコードを追加します。 (戻るボタンをサポートする前記のコードは既に追加されているものとします。) すべてをまとめた `App` コードは、「コード例」セクションの最後で確認できます。

```csharp
// App.xaml.cs
// Add event handler in OnLaunced.
protected override void OnLaunched(LaunchActivatedEventArgs e)
{
    // ...
    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active
    if (rootFrame == null)
    {
        // ...
        // rootFrame.NavigationFailed += OnNavigationFailed;

        // Add support for accelerator keys. 
        // Listen to the window directly so the app responds
        // to accelerator keys regardless of which element has focus.
        Window.Current.CoreWindow.Dispatcher.AcceleratorKeyActivated +=
            CoreDispatcher_AcceleratorKeyActivated;

        // ...

    }
}

// ...

// Add this code after the TryGoBack method added previously.
// Perform forward navigation if possible.
private bool TryGoForward()
{
    Frame rootFrame = Window.Current.Content as Frame;
    if (rootFrame.CanGoForward)
    {
        rootFrame.GoForward();
        return true;
    }
    return false;
}

// Invoked on every keystroke, including system keys such as Alt key combinations.
// Used to detect keyboard navigation between pages even when the page itself
// doesn't have focus.
private void CoreDispatcher_AcceleratorKeyActivated(CoreDispatcher sender, AcceleratorKeyEventArgs e)
{
    // When Alt+Left are pressed navigate back.
    // When Alt+Right are pressed navigate forward.
    if (e.EventType == CoreAcceleratorKeyEventType.SystemKeyDown
        && (e.VirtualKey == VirtualKey.Left || e.VirtualKey == VirtualKey.Right)
        && e.KeyStatus.IsMenuKeyDown == true
        && !e.Handled)
    {
        if (e.VirtualKey == VirtualKey.Left)
        {
            e.Handled = TryGoBack();
        }
        else if (e.VirtualKey == VirtualKey.Right)
        {
            e.Handled = TryGoForward();
        }
    }
}
```

```cppwinrt
// App.cpp
void App::OnLaunched(LaunchActivatedEventArgs const& e)
{
    // ...
    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active
    if (rootFrame == nullptr)
    {
        // ...
        // rootFrame.NavigationFailed({ this, &App::OnNavigationFailed });

        // Add support for accelerator keys. 
        // Listen to the window directly so the app responds
        // to accelerator keys regardless of which element has focus.
        Window::Current().CoreWindow().Dispatcher().
            AcceleratorKeyActivated({ this, &App::CoreDispatcher_AcceleratorKeyActivated });

        // ...
    }
}

// App.h
struct App : AppT<App>
{
    App();

    // ...
    // Add this code after the TryGoBack method added previously.

private:
    // Perform forward navigation if possible.
    bool TryGoForward()
    {
        Frame rootFrame{ nullptr };
        auto content = Window::Current().Content();
        if (content)
        {
            rootFrame = content.try_as<Frame>();
            if (rootFrame.CanGoForward())
            {
                rootFrame.GoForward();
                return true;
            }
        }
        return false;
    }
 
 
    // Invoked on every keystroke, including system keys such as Alt key combinations.
    // Used to detect keyboard navigation between pages even when the page itself
    // doesn't have focus.
    void CoreDispatcher_AcceleratorKeyActivated(CoreDispatcher const& /* sender */, AcceleratorKeyEventArgs const& e)
    {
        // When Alt+Left are pressed navigate back.
        // When Alt+Right are pressed navigate forward.
        if (e.EventType() == CoreAcceleratorKeyEventType::SystemKeyDown
            && (e.VirtualKey() == Windows::System::VirtualKey::Left || e.VirtualKey() == Windows::System::VirtualKey::Right)
            && e.KeyStatus().IsMenuKeyDown
            && !e.Handled())
        {
            if (e.VirtualKey() == Windows::System::VirtualKey::Left)
            {
                e.Handled(TryGoBack());
            }
            else if (e.VirtualKey() == Windows::System::VirtualKey::Right)
            {
                e.Handled(TryGoForward());
            }
        }
    }
};
```

### <a name="handle-system-back-requests"></a>システムの戻る要求を処理する

Windows デバイスには、システムで戻るナビゲーション要求をアプリに渡すためのさまざまな方法が用意されています。 一般的な方法としては、ゲームパッドの B ボタン、Windows キー + Backspace キーのショートカット、タブレット モードでのシステムの戻るボタンなどがあります。使用可能なオプションは、デバイスによって異なります。

[SystemNavigationManager.BackRequested](/api/windows.ui.core.systemnavigationmanager.backrequested) イベントにリスナーを登録することにより、ハードウェアおよびソフトウェア システムの戻るキーから発生する、システム提供の戻る要求をサポートできます。

システムで提供される戻る要求をサポートするには、次に示すコードを `App` クラスに追加します。 (戻るボタンをサポートする前記のコードは既に追加されているものとします。) すべてをまとめた `App` コードは、「コード例」セクションの最後で確認できます。

```csharp
// App.xaml.cs
// Add event handler in OnLaunced.
protected override void OnLaunched(LaunchActivatedEventArgs e)
{
    // ...
    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active
    if (rootFrame == null)
    {
        // ...
        // Add support for accelerator keys. 
        // ... (Previously added code.)

        // Add support for system back requests. 
        SystemNavigationManager.GetForCurrentView().BackRequested 
            += System_BackRequested;

        // ...

    }
}

// ...
// Handle system back requests.
private void System_BackRequested(object sender, BackRequestedEventArgs e)
{
    if (!e.Handled)
    {
        e.Handled = TryGoBack();
    }
}
```

```cppwinrt
// App.cpp
void App::OnLaunched(LaunchActivatedEventArgs const& e)
{
    // ...
    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active
    if (rootFrame == nullptr)
    {
        // ...
        // Add support for accelerator keys. 
        // ... (Previously added code.)

        // Add support for system back requests. 
        SystemNavigationManager::GetForCurrentView().
            BackRequested({ this, &App::System_BackRequested });

        // ...
    }
}

// App.h
struct App : AppT<App>
{
    App();

    // ...

private:
    // ...

    // Handle system back requests.
    void System_BackRequested(IInspectable const& /* sender */, BackRequestedEventArgs const& e)
    {
        if (!e.Handled())
        {
            e.Handled(TryGoBack());
        }
    }
};
```

#### <a name="system-back-behavior-for-backward-compatibility"></a>下位互換性のためのシステムの戻る動作

以前の UWP アプリでは、後方ナビゲーション用のシステム提供の戻るボタンの表示と非表示を切り替えるために、[SystemNavigationManager.AppViewBackButtonVisibility](/uwp/api/windows.ui.core.systemnavigationmanager.appviewbackbuttonvisibility) が使用されていました。 (このボタンにより [SystemNavigationManager.BackRequested](/api/windows.ui.core.systemnavigationmanager.backrequested) イベントが発生します。) この API は下位互換性を確保するために引き続きサポートされますが、`AppViewBackButtonVisibility` によって公開される戻るボタンの使用は推奨されなくなっています。 代わりに、この記事で説明されているように、アプリ内で独自の戻るボタンを提供する必要があります。

[AppViewBackButtonVisibility](/uwp/api/windows.ui.core.appviewbackbuttonvisibility) を使い続けた場合、システム UI によりタイトル バーの内部に戻るボタンがレンダリングされます。 (戻るボタンの外観とユーザー操作は以前のビルドと変わりありません。)

![タイトル バーの戻るボタン](images/nav-back-pc.png)

### <a name="handle-mouse-navigation-buttons"></a>マウスのナビゲーション ボタンを処理する

一部のマウスには、前方と後方のナビゲーション用にハードウェア ナビゲーション ボタンが用意されています。 これらのマウス ボタンをサポートするには、[CoreWindow.PointerPressed](/uwp/api/windows.ui.core.corewindow.pointerpressed) イベントを処理し、[IsXButton1Pressed](/uwp/api/windows.ui.input.pointerpointproperties.isxbutton1pressed) (後方) または [IsXButton2Pressed](/uwp/api/windows.ui.input.pointerpointproperties.isxbutton2pressed) (前方) をチェックします。

マウス ボタンのナビゲーションをサポートするために `App` クラスに追加するコードを次に示します。 (戻るボタンをサポートする前記のコードは既に追加されているものとします。) すべてをまとめた `App` コードは、「コード例」セクションの最後で確認できます。

```csharp
// App.xaml.cs
// Add event handler in OnLaunced.
protected override void OnLaunched(LaunchActivatedEventArgs e)
{
    // ...
    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active
    if (rootFrame == null)
    {
        // ...
        // Add support for system back requests. 
        // ... (Previously added code.)

        // Add support for mouse navigation buttons. 
        Window.Current.CoreWindow.PointerPressed += CoreWindow_PointerPressed;

        // ...

    }
}

// ...

// Handle mouse back button.
private void CoreWindow_PointerPressed(CoreWindow sender, PointerEventArgs e)
{
    // For this event, e.Handled arrives as 'true', so invert the value.
    if (e.CurrentPoint.Properties.IsXButton1Pressed
        && e.Handled)
    {
        e.Handled = !TryGoBack();
    }
    else if (e.CurrentPoint.Properties.IsXButton2Pressed
            && e.Handled)
    {
        e.Handled = !TryGoForward();
    }
}
```

```cppwinrt
// App.cpp
void App::OnLaunched(LaunchActivatedEventArgs const& e)
{
    // ...
    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active
    if (rootFrame == nullptr)
    {
        // ...
        // Add support for system back requests. 
        // ... (Previously added code.)

        // Add support for mouse navigation buttons. 
        Window::Current().CoreWindow().
            PointerPressed({ this, &App::CoreWindow_PointerPressed });

        // ...
    }
}

// App.h
struct App : AppT<App>
{
    App();

    // ...

private:
    // ...

    // Handle mouse forward and back buttons.
    void CoreWindow_PointerPressed(CoreWindow const& /* sender */, PointerEventArgs const& e)
    {
        // For this event, e.Handled arrives as 'true', so invert the value. 
        if (e.CurrentPoint().Properties().IsXButton1Pressed()
            && e.Handled())
        {
            e.Handled(!TryGoBack());
        }
        else if (e.CurrentPoint().Properties().IsXButton2Pressed()
            && e.Handled())
        {
            e.Handled(!TryGoForward());
        }
    }
};
```

### <a name="all-code-added-to-app-class"></a>App クラスに追加されたすべてのコード
 
```csharp
// App.xaml.cs
//
// (Add event handlers in OnLaunched override.)
protected override void OnLaunched(LaunchActivatedEventArgs e)
{
    // ...
    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active
    if (rootFrame == null)
    {
        // ...
        // rootFrame.NavigationFailed += OnNavigationFailed;

        // Add support for accelerator keys. 
        // Listen to the window directly so the app responds
        // to accelerator keys regardless of which element has focus.
        Window.Current.CoreWindow.Dispatcher.AcceleratorKeyActivated +=
            CoreDispatcher_AcceleratorKeyActivated;

        // Add support for system back requests. 
        SystemNavigationManager.GetForCurrentView().BackRequested 
            += System_BackRequested;

        // Add support for mouse navigation buttons. 
        Window.Current.CoreWindow.PointerPressed += CoreWindow_PointerPressed;

        // ...

    }
}

// ...

// (Add these methods to the App class.)
public static bool TryGoBack()
{
    Frame rootFrame = Window.Current.Content as Frame;
    if (rootFrame.CanGoBack)
    {
        rootFrame.GoBack();
        return true;
    }
    return false;
}

// Perform forward navigation if possible.
private bool TryGoForward()
{
    Frame rootFrame = Window.Current.Content as Frame;
    if (rootFrame.CanGoForward)
    {
        rootFrame.GoForward();
        return true;
    }
    return false;
}

// Invoked on every keystroke, including system keys such as Alt key combinations.
// Used to detect keyboard navigation between pages even when the page itself
// doesn't have focus.
private void CoreDispatcher_AcceleratorKeyActivated(CoreDispatcher sender, AcceleratorKeyEventArgs e)
{
    // When Alt+Left are pressed navigate back.
    // When Alt+Right are pressed navigate forward.
    if (e.EventType == CoreAcceleratorKeyEventType.SystemKeyDown
        && (e.VirtualKey == VirtualKey.Left || e.VirtualKey == VirtualKey.Right)
        && e.KeyStatus.IsMenuKeyDown == true
        && !e.Handled)
    {
        if (e.VirtualKey == VirtualKey.Left)
        {
            e.Handled = TryGoBack();
        }
        else if (e.VirtualKey == VirtualKey.Right)
        {
            e.Handled = TryGoForward();
        }
    }
}

// Handle system back requests.
private void System_BackRequested(object sender, BackRequestedEventArgs e)
{
    if (!e.Handled)
    {
        e.Handled = TryGoBack();
    }
}

// Handle mouse back button.
private void CoreWindow_PointerPressed(CoreWindow sender, PointerEventArgs e)
{
    // For this event, e.Handled arrives as 'true', so invert the value.
    if (e.CurrentPoint.Properties.IsXButton1Pressed
        && e.Handled)
    {
        e.Handled = !TryGoBack();
    }
    else if (e.CurrentPoint.Properties.IsXButton2Pressed
            && e.Handled)
    {
        e.Handled = !TryGoForward();
    }
}


```

```cppwinrt
// App.cpp
void App::OnLaunched(LaunchActivatedEventArgs const& e)
{
    // ...
    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active
    if (rootFrame == nullptr)
    {
        // ...
        // rootFrame.NavigationFailed({ this, &App::OnNavigationFailed });

        // Add support for accelerator keys. 
        // Listen to the window directly so the app responds
        // to accelerator keys regardless of which element has focus.
        Window::Current().CoreWindow().Dispatcher().
            AcceleratorKeyActivated({ this, &App::CoreDispatcher_AcceleratorKeyActivated });

        // Add support for system back requests. 
        SystemNavigationManager::GetForCurrentView().
            BackRequested({ this, &App::System_BackRequested });

        // Add support for mouse navigation buttons. 
        Window::Current().CoreWindow().
            PointerPressed({ this, &App::CoreWindow_PointerPressed });

        // ...
    }
}

// App.h
#include "winrt/Windows.UI.Core.h"
#include "winrt/Windows.System.h"
#include "winrt/Windows.UI.Input.h"
#include "winrt/Windows.UI.Xaml.Input.h"
 
using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::UI::Core;
using namespace Windows::UI::Input;
using namespace Windows::UI::Xaml;
using namespace Windows::UI::Xaml::Controls;

struct App : AppT<App>
{
    App();

    // ...

    // Perform back navigation if possible.
    static bool TryGoBack()
    {
        Frame rootFrame{ nullptr };
        auto content = Window::Current().Content();
        if (content)
        {
            rootFrame = content.try_as<Frame>();
            if (rootFrame.CanGoBack())
            {
                rootFrame.GoBack();
                return true;
            }
        }
        return false;
    }
private:
    // Perform forward navigation if possible.
    bool TryGoForward()
    {
        Frame rootFrame{ nullptr };
        auto content = Window::Current().Content();
        if (content)
        {
            rootFrame = content.try_as<Frame>();
            if (rootFrame.CanGoForward())
            {
                rootFrame.GoForward();
                return true;
            }
        }
        return false;
    }
  
    // Invoked on every keystroke, including system keys such as Alt key combinations.
    // Used to detect keyboard navigation between pages even when the page itself
    // doesn't have focus.
    void CoreDispatcher_AcceleratorKeyActivated(CoreDispatcher const& /* sender */, AcceleratorKeyEventArgs const& e)
    {
        // When Alt+Left are pressed navigate back.
        // When Alt+Right are pressed navigate forward.
        if (e.EventType() == CoreAcceleratorKeyEventType::SystemKeyDown
            && (e.VirtualKey() == Windows::System::VirtualKey::Left || e.VirtualKey() == Windows::System::VirtualKey::Right)
            && e.KeyStatus().IsMenuKeyDown
            && !e.Handled())
        {
            if (e.VirtualKey() == Windows::System::VirtualKey::Left)
            {
                e.Handled(TryGoBack());
            }
            else if (e.VirtualKey() == Windows::System::VirtualKey::Right)
            {
                e.Handled(TryGoForward());
            }
        }
    }

    // Handle system back requests.
    void System_BackRequested(IInspectable const& /* sender */, BackRequestedEventArgs const& e)
    {
        if (!e.Handled())
        {
            e.Handled(TryGoBack());
        }
    }

    // Handle mouse forward and back buttons.
    void CoreWindow_PointerPressed(CoreWindow const& /* sender */, PointerEventArgs const& e)
    {
        // For this event, e.Handled arrives as 'true', so invert the value. 
        if (e.CurrentPoint().Properties().IsXButton1Pressed()
            && e.Handled())
        {
            e.Handled(!TryGoBack());
        }
        else if (e.CurrentPoint().Properties().IsXButton2Pressed()
            && e.Handled())
        {
            e.Handled(!TryGoForward());
        }
    }
};
```

## <a name="guidelines-for-custom-back-navigation-behavior"></a>カスタムの "戻る" ナビゲーションの動作のガイドライン

独自のバック スタック ナビゲーションを提供する場合、エクスペリエンスが他のアプリと一貫している必要があります。 ナビゲーション操作では、次のパターンに従うことをお勧めします。

<div class="mx-responsive-img">
<table>
<thead>
<tr class="header">
<th align="left">ナビゲーション操作</th>
<th align="left">ナビゲーション履歴への追加</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="vertical-align:top;"><strong>ページ間、異なるピア グループ</strong></td>
<td style="vertical-align:top;"><strong>はい</strong>
<p>この図では、ユーザーはピア グループを横断して、アプリのレベル 1 からレベル 2 に移動します。そのため、このナビゲーションはナビゲーション履歴に追加されます。</p>
<p><img src="images/back-nav/nav-pagetopage-diffpeers-imageonly1.png" alt="Diagram of navigation across peer groups showing the user navigating from group one to group two and the back to group one." /></p>
<p>次の図では、ユーザーは同じレベルにある 2 つのピア グループの間を移動し、この場合もピア グループを横断します。そのため、このナビゲーションはナビゲーション履歴に追加されます。</p>
<p><img src="images/back-nav/nav-pagetopage-diffpeers-imageonly2.png" alt="Diagram of navigation across peer groups showing the user navigating from group one to group two then on to group three and back to group two." /></p></td>
</tr>
<tr class="even">
<td style="vertical-align:top;"><strong>ページ間、同じピア グループ、ナビゲーション要素は画面上に表示されない</strong>
<p>ユーザーは、同じピア グループでページ間を移動します。 両方のページに直接的なナビゲーションを提供する画面上のナビゲーション要素 (<a href="/windows/uwp/design/controls-and-patterns/navigationview">NavigationView</a> など) はありません。</p></td>
<td style="vertical-align:top;"><strong>はい</strong>
<p>次の図では、ユーザーは同じピア グループ内の 2 つのページ間を移動し、ナビゲーションはナビゲーション履歴に追加する必要があります。</p>
<p><img src="images/back-nav/nav-pagetopage-samepeer-noosnavelement.png" alt="Navigation within a peer group" /></p></td>
</tr>
<tr class="odd">
<td style="vertical-align:top;"><strong>ページ間、同じピア グループ、画面上に表示されるナビゲーション要素を使う</strong>
<p>ユーザーは、同じピア グループ内のページ間を移動します。 両方のページは同じナビゲーション要素に表示されます (<a href="/windows/uwp/design/controls-and-patterns/navigationview">NavigationView</a> など)。</p></td>
<td style="vertical-align:top;"><strong>場合によって異なります</strong>
<p>はい、ナビゲーション履歴に追加しますが、2 つの注目すべき例外があります。 アプリのユーザーがピア グループ内でページ間を頻繁に切り替えることが予想される場合、またはナビゲーション階層を保持する場合は、ナビゲーション履歴に追加しないでください。 この場合、ユーザーが戻るボタンを押すと、現在のピア グループに移動する前に表示していた最後のページに戻ります。 </p>
<p><img src="images/back-nav/nav-pagetopage-samepeer-yesosnavelement.png" alt="Navigation across peer groups when a navigation element is present" /></p></td>
</tr>
<tr class="even">
<td style="vertical-align:top;"><strong>一時的な UI の表示</strong>
<p>アプリは、ダイアログ、スプラッシュ画面、スクリーン キーボードなどのポップアップ ウィンドウや子ウィンドウを表示します。または、アプリが複数選択モードなどの特別なモードに移行します。</p></td>
<td style="vertical-align:top;"><strong>いいえ</strong>
<p>ユーザーが戻るボタンを押すと、一時的な UI が閉じられ (スクリーン キーボードが非表示になる、ダイアログがキャンセルされるなど)、一時的な UI を生成したページに戻ります。</p>
<p><img src="images/back-nav/back-transui.png" alt="Showing a transient UI" /></p></td>
</tr>
<tr class="odd">
<td style="vertical-align:top;"><strong>項目の列挙</strong>
<p>アプリが、マスター/詳細リストで選んだ項目の詳細など、画面上の項目のコンテンツを表示します。</p></td>
<td style="vertical-align:top;"><strong>いいえ</strong>
<p>項目の列挙は、ピア グループ内の移動に似ています。 ユーザーが戻るボタンを押すと、項目の列挙が表示されている現在のページの前のページに移動されます。</p>
<p><img src="images/back-nav/nav-enumerate.png" alt="Iterm enumeration" /></p></td>
</tr>
</tbody>
</table>
</div>

### <a name="resuming"></a>Resuming

ユーザーが別のアプリに切り替えた後で、元のアプリに戻った場合は、ナビゲーション履歴にある最後のページに戻すことをお勧めします。

## <a name="related-articles"></a>関連記事

- [ナビゲーションの基本](navigation-basics.md)
