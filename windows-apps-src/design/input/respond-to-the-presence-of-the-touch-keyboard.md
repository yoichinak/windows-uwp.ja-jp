---
Description: タッチ キーボードを表示または非表示にするときにアプリの UI を調整する方法について説明します。
title: タッチ キーボードの表示への応答
ms.assetid: 70C6130E-23A2-4F9D-88E7-7060062DA988
label: Respond to the presence of the touch keyboard
template: detail.hbs
keywords: キーボード, アクセシビリティ, ナビゲーション, フォーカス, テキスト, 入力, ユーザーの操作
ms.date: 07/13/2018
ms.topic: article
ms.openlocfilehash: e26bbe00bba8b3d91d7ee842cb4d9c984a941f2b
ms.sourcegitcommit: 0a319e2e69ef88b55d472b009b3061a7b82e3ab1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/21/2020
ms.locfileid: "77521363"
---
# <a name="respond-to-the-presence-of-the-touch-keyboard"></a>タッチ キーボードの表示への応答

タッチ キーボードを表示または非表示にするときにアプリの UI を調整する方法について説明します。

### <a name="important-apis"></a>重要な API

- [AutomationPeer](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer)
- [InputPane](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.InputPane)

![既定のレイアウト モードのタッチ キーボード](images/keyboard/default.png)

<sup>既定のレイアウトモードでのタッチキーボード</sup>

タッチ キーボードによって、タッチをサポートするデバイスのテキスト入力が有効になります。 ユニバーサル Windows プラットフォーム (UWP) のテキスト入力コントロールでは、ユーザーが編集可能な入力フィールドをタップしたときに、既定でタッチ キーボードが表示されます。 タッチ キーボードは、通常、ユーザーがフォーム内のコントロール間を移動している間は表示されますが、この動作はフォーム内の他のコントロールの種類に基づいて異なります。

標準のテキスト入力コントロールから派生しないカスタムテキスト入力コントロールで、対応するタッチキーボード動作をサポートするには、 <a href="https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer">AutomationPeer</a>クラスを使用して、コントロールを Microsoft ui オートメーションに公開し、適切な ui オートメーションコントロールパターンを実装する必要があります。 「[キーボードのアクセシビリティ](https://docs.microsoft.com/windows/uwp/design/accessibility/keyboard-accessibility)」と「[カスタム オートメーション ピア](https://docs.microsoft.com/windows/uwp/design/accessibility/custom-automation-peers)」をご覧ください。

このサポートがカスタム コントロールに追加されると、タッチ キーボードの表示に適切に応答できます。

**応募**

このトピックは、「[キーボード操作](keyboard-interactions.md)」に基づいています。

標準のキーボード操作、キーボード入力とイベントの処理、UI オートメーションの基本を理解している必要があります。

ユニバーサル Windows プラットフォーム (UWP) アプリを開発するのが初めての場合は、以下のトピックに目を通して、ここで説明されているテクノロジをよく理解できるようにしてください。

- [初めてのアプリの作成](https://docs.microsoft.com/windows/uwp/get-started/your-first-app)
- 「[イベントとルーティング イベントの概要](https://docs.microsoft.com/windows/uwp/xaml-platform/events-and-routed-events-overview)」に記載されているイベントの説明

**ユーザーエクスペリエンスのガイドライン:**

キーボード入力用に最適化された便利で魅力的なアプリのデザインに関するヒントについては、「[キーボード操作](https://docs.microsoft.com/windows/uwp/design/input/keyboard-interactions)」を参照してください。

## <a name="touch-keyboard-and-a-custom-ui"></a>タッチ キーボードとカスタム UI

ここでは、カスタム テキスト入力コントロールについてのいくつかの基本的な推奨事項を示します。

- フォームに対する操作が行われている間はタッチ キーボードを表示します。

- テキスト入力のコンテキストでフォーカスがテキスト入力フィールドから移動したときに、カスタムコントロールに適切な UI オートメーション [AutomationControlType](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationControlType)が割り当てられていることを確認します。 たとえば、テキスト入力シナリオの半ばでメニューを開くときに、キーボードを表示したままにするには、このメニューに Menu の **AutomationControlType** が必要です。

- UI オートメーション プロパティを操作してタッチ キーボードを制御しないでください。 UI オートメーション プロパティの正確さに依存する他のアクセシビリティ ツールがあります。

- 操作している入力フィールドをユーザーが常に見られるようにします。

    タッチ キーボードによって画面の大部分が見えなくなるため、UWP では、ユーザーがフォームのコントロール間を移動するときに、フォーカスのある入力フィールドをスクロールしてビューに表示します。これには、現在ビューに表示されていないコントロールも含まれます。

    UI をカスタマイズする場合は、 [**Inputpane**](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.InputPane)オブジェクトによって公開されるイベントの[表示](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.inputpane.showing)と[非表示](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.inputpane.hiding)を処理することで、タッチキーボードの外観に同様の動作を提供します。

    ![タッチ キーボードが表示または非表示になっているフォーム](images/touch-keyboard-pan1.png)

    場合によっては、画面にずっと表示されたままであることが必要な UI 要素もあります。 フォーム コントロールがパン領域に含まれ、重要な UI 要素が静的であるように UI を設計します。 例 :

    ![常に表示されている必要がある領域を含むフォーム](images/touch-keyboard-pan2.png)

## <a name="handling-the-showing-and-hiding-events"></a>Showing イベントと Hiding イベントの処理

タッチキーボードのイベントを[表示](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.inputpane.showing)および[非表示](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.inputpane.hiding)にするイベントハンドラーをアタッチする例を次に示します。

```csharp
using Windows.UI.ViewManagement;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Media;
using Windows.Foundation;
using Windows.UI.Xaml.Navigation;

namespace SDKTemplate
{
    /// <summary>
    /// Sample page to subscribe show/hide event of Touch Keyboard.
    /// </summary>
    public sealed partial class Scenario2_ShowHideEvents : Page
    {
        public Scenario2_ShowHideEvents()
        {
            this.InitializeComponent();
        }

        protected override void OnNavigatedTo(NavigationEventArgs e)
        {
            InputPane currentInputPane = InputPane.GetForCurrentView();
            // Subscribe to Showing/Hiding events
            currentInputPane.Showing += OnShowing;
            currentInputPane.Hiding += OnHiding;
        }

        protected override void OnNavigatedFrom(NavigationEventArgs e)
        {
            InputPane currentInputPane = InputPane.GetForCurrentView();
            // Unsubscribe from Showing/Hiding events
            currentInputPane.Showing -= OnShowing;
            currentInputPane.Hiding -= OnHiding;
        }

        void OnShowing(InputPane sender, InputPaneVisibilityEventArgs e)
        {
            LastInputPaneEventRun.Text = "Showing";
        }
        
        void OnHiding(InputPane sender, InputPaneVisibilityEventArgs e)
        {
            LastInputPaneEventRun.Text = "Hiding";
        }
    }
}
```

```cppwinrt
...
#include <winrt/Windows.UI.ViewManagement.h>
...
private:
    winrt::event_token m_showingEventToken;
    winrt::event_token m_hidingEventToken;
...
Scenario2_ShowHideEvents::Scenario2_ShowHideEvents()
{
    InitializeComponent();
}

void Scenario2_ShowHideEvents::OnNavigatedTo(Windows::UI::Xaml::Navigation::NavigationEventArgs const& e)
{
    auto inputPane{ Windows::UI::ViewManagement::InputPane::GetForCurrentView() };
    // Subscribe to Showing/Hiding events
    m_showingEventToken = inputPane.Showing({ this, &Scenario2_ShowHideEvents::OnShowing });
    m_hidingEventToken = inputPane.Hiding({ this, &Scenario2_ShowHideEvents::OnHiding });
}

void Scenario2_ShowHideEvents::OnNavigatedFrom(Windows::UI::Xaml::Navigation::NavigationEventArgs const& e)
{
    auto inputPane{ Windows::UI::ViewManagement::InputPane::GetForCurrentView() };
    // Unsubscribe from Showing/Hiding events
    inputPane.Showing(m_showingEventToken);
    inputPane.Hiding(m_hidingEventToken);
}

void Scenario2_ShowHideEvents::OnShowing(Windows::UI::ViewManagement::InputPane const& /*sender*/, Windows::UI::ViewManagement::InputPaneVisibilityEventArgs const& /*args*/)
{
    LastInputPaneEventRun().Text(L"Showing");
}

void Scenario2_ShowHideEvents::OnHiding(Windows::UI::ViewManagement::InputPane const& /*sender*/, Windows::UI::ViewManagement::InputPaneVisibilityEventArgs const& /*args*/)
{
    LastInputPaneEventRun().Text(L"Hiding");
}
```

```cpp
#include "pch.h"
#include "Scenario2_ShowHideEvents.xaml.h"

using namespace SDKTemplate;
using namespace Platform;
using namespace Windows::Foundation;
using namespace Windows::UI::ViewManagement;
using namespace Windows::UI::Xaml;
using namespace Windows::UI::Xaml::Controls;
using namespace Windows::UI::Xaml::Navigation;

Scenario2_ShowHideEvents::Scenario2_ShowHideEvents()
{
    InitializeComponent();
}

void Scenario2_ShowHideEvents::OnNavigatedTo(NavigationEventArgs^ e)
{
    auto inputPane = InputPane::GetForCurrentView();
    // Subscribe to Showing/Hiding events
    showingEventToken = inputPane->Showing +=
        ref new TypedEventHandler<InputPane^, InputPaneVisibilityEventArgs^>(this, &Scenario2_ShowHideEvents::OnShowing);
    hidingEventToken = inputPane->Hiding +=
        ref new TypedEventHandler<InputPane^, InputPaneVisibilityEventArgs^>(this, &Scenario2_ShowHideEvents::OnHiding);
}

void Scenario2_ShowHideEvents::OnNavigatedFrom(NavigationEventArgs^ e)
{
    auto inputPane = Windows::UI::ViewManagement::InputPane::GetForCurrentView();
    // Unsubscribe from Showing/Hiding events
    inputPane->Showing -= showingEventToken;
    inputPane->Hiding -= hidingEventToken;
}

void Scenario2_ShowHideEvents::OnShowing(InputPane^ /*sender*/, InputPaneVisibilityEventArgs^ /*args*/)
{
    LastInputPaneEventRun->Text = L"Showing";
}

void Scenario2_ShowHideEvents::OnHiding(InputPane^ /*sender*/, InputPaneVisibilityEventArgs ^ /*args*/)
{
    LastInputPaneEventRun->Text = L"Hiding";
}
```

## <a name="related-articles"></a>関連トピック

- [キーボード操作](keyboard-interactions.md)
- [キーボードのアクセシビリティ](https://docs.microsoft.com/windows/uwp/accessibility/keyboard-accessibility)
- [カスタム オートメーション ピア](https://docs.microsoft.com/windows/uwp/accessibility/custom-automation-peers)

**サンプル**

- [タッチキーボードのサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/TouchKeyboard)

**サンプルのアーカイブ**

- [入力: タッチキーボードのサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/TouchKeyboard)
- [スクリーンキーボードサンプルの外観への対応](https://code.msdn.microsoft.com/windowsapps/keyboard-events-sample-866ba41c)
- [XAML テキスト編集のサンプル](https://code.msdn.microsoft.com/windowsapps/XAML-text-editing-sample-fb0493ad)
- [XAML アクセシビリティのサンプル](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/XAML%20accessibility%20sample)
