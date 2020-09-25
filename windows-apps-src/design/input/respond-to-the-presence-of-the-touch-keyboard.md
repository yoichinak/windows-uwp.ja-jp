---
Description: タッチ キーボードを表示または非表示にするときにアプリの UI を調整する方法について説明します。
title: タッチ キーボードの表示への応答
ms.assetid: 70C6130E-23A2-4F9D-88E7-7060062DA988
label: Respond to the presence of the touch keyboard
template: detail.hbs
keywords: キーボード, アクセシビリティ, ナビゲーション, フォーカス, テキスト, 入力, ユーザーの操作
ms.date: 09/24/2020
ms.topic: article
ms.openlocfilehash: 4af7e7533ebd985a22eedd2e11f35d8bf5f5dc8a
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/24/2020
ms.locfileid: "91216905"
---
# <a name="respond-to-the-presence-of-the-touch-keyboard"></a>タッチ キーボードの表示への応答

タッチ キーボードを表示または非表示にするときにアプリの UI を調整する方法について説明します。

### <a name="important-apis"></a>重要な API

- [AutomationPeer](/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer)
- [InputPane](/uwp/api/Windows.UI.ViewManagement.InputPane)

![既定のレイアウト モードのタッチ キーボード](images/keyboard/default.png)

<sup>既定のレイアウトモードでのタッチキーボード</sup>

タッチ キーボードによって、タッチをサポートするデバイスのテキスト入力が有効になります。 Windows アプリのテキスト入力コントロールは、ユーザーが編集可能な入力フィールドをタップしたときに、既定でタッチキーボードを呼び出します。 タッチ キーボードは、通常、ユーザーがフォーム内のコントロール間を移動している間は表示されますが、この動作はフォーム内の他のコントロールの種類に基づいて異なります。

標準のテキスト入力コントロールから派生していないカスタム テキスト入力コントロールで対応するタッチ キーボードの動作をサポートするには、<a href="/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer">AutomationPeer</a> クラスを使ってコントロールを Microsoft UI オートメーションに公開し、適切な UI オートメーション コントロール パターンを実装する必要があります。 「[キーボードのアクセシビリティ](../accessibility/keyboard-accessibility.md)」と「[カスタム オートメーション ピア](../accessibility/custom-automation-peers.md)」をご覧ください。

このサポートがカスタム コントロールに追加されると、タッチ キーボードの表示に適切に応答できます。

**前提条件:**

このトピックは、「[キーボード操作](keyboard-interactions.md)」に基づいています。

標準のキーボード操作、キーボード入力とイベントの処理、UI オートメーションの基本を理解している必要があります。

Windows アプリの開発に慣れていない場合は、以下のトピックを参照して、ここで説明するテクノロジについて理解してください。

- [最初のアプリの作成](../../get-started/your-first-app.md)
- 「[イベントとルーティング イベントの概要](../../xaml-platform/events-and-routed-events-overview.md)」に記載されているイベントの説明

**ユーザーエクスペリエンスのガイドライン:**

キーボード入力用に最適化された便利で魅力的なアプリのデザインに関するヒントについては、「 [キーボード操作](./keyboard-interactions.md) 」を参照してください。

## <a name="touch-keyboard-and-a-custom-ui"></a>タッチ キーボードとカスタム UI

ここでは、カスタム テキスト入力コントロールについてのいくつかの基本的な推奨事項を示します。

- フォームに対する操作が行われている間はタッチ キーボードを表示します。

- テキスト入力のコンテキストでフォーカスがテキスト入力フィールドから移動したときに、カスタムコントロールに適切な UI オートメーション [AutomationControlType](/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationControlType) が割り当てられていることを確認します。 たとえば、テキスト入力シナリオの半ばでメニューを開くときに、キーボードを表示したままにするには、このメニューに Menu の **AutomationControlType** が必要です。

- UI オートメーション プロパティを操作してタッチ キーボードを制御しないでください。 UI オートメーション プロパティの正確さに依存する他のアクセシビリティ ツールがあります。

- 操作している入力フィールドをユーザーが常に見られるようにします。

    タッチキーボードは画面の大部分を occludes しているため、ユーザーがフォーム上のコントロールを移動すると、フォーカスが設定された入力フィールドが表示されるようになります。現在表示されていないコントロールも含まれます。

    UI をカスタマイズする場合は、[**InputPane**](/uwp/api/Windows.UI.ViewManagement.InputPane) オブジェクトで公開される [Showing](/uwp/api/windows.ui.viewmanagement.inputpane.showing) イベントと [Hiding](/uwp/api/windows.ui.viewmanagement.inputpane.hiding) イベントを処理して、タッチ キーボードの表示について同様の動作を提供します。

    ![タッチ キーボードが表示または非表示になっているフォーム](images/touch-keyboard-pan1.png)

    場合によっては、画面にずっと表示されたままであることが必要な UI 要素もあります。 フォーム コントロールがパン領域に含まれ、重要な UI 要素が静的であるように UI を設計します。 次に例を示します。

    ![常に表示されている必要がある領域を含むフォーム](images/touch-keyboard-pan2.png)

## <a name="handling-the-showing-and-hiding-events"></a>Showing イベントと Hiding イベントの処理

タッチキーボードのイベントを [表示](/uwp/api/windows.ui.viewmanagement.inputpane.showing) および [非表示](/uwp/api/windows.ui.viewmanagement.inputpane.hiding) にするイベントハンドラーをアタッチする例を次に示します。

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

## <a name="related-articles"></a>関連記事

- [キーボード操作](keyboard-interactions.md)
- [キーボードアクセシビリティ](../accessibility/keyboard-accessibility.md)
- [カスタム オートメーション ピア](../accessibility/custom-automation-peers.md)

### <a name="samples"></a>サンプル

- [タッチ キーボードのサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/TouchKeyboard)

### <a name="archive-samples"></a>サンプルのアーカイブ

- [入力: タッチ キーボードのサンプルに関するページ](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/TouchKeyboard)
- [スクリーン キーボードを表示したときの対応のサンプルのページ](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Responding%20to%20the%20appearance%20of%20the%20on-screen%20keyboard%20sample)
- [XAML テキスト編集のサンプルに関するページ](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BVB%5D-Windows%208%20app%20samples/VB/Windows%208%20app%20samples/XAML%20text%20editing%20sample%20(Windows%208))
- [XAML アクセシビリティ サンプル](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/XAML%20accessibility%20sample)
