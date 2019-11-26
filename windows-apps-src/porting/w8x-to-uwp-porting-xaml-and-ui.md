---
description: 宣言型 XAML マークアップ形式での UI の定義は、ユニバーサル 8.1 アプリからユニバーサル Windows プラットフォーム (UWP) アプリに適切に変換されます。
title: Windows ランタイム 8.x の XAML と UI の UWP への移植
ms.assetid: 78b86762-7359-474f-b1e3-c2d7cf9aa907
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 19e754fd6a52880c7bc636818acaeda815f9da16
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259107"
---
# <a name="porting-windows-runtime-8x-xaml-and-ui-to-uwp"></a>Windows ランタイム 8.x の XAML と UI の UWP への移植


前のトピックは、「[トラブルシューティング](w8x-to-uwp-troubleshooting.md)」でした。

宣言型 XAML マークアップ形式での UI の定義は、ユニバーサル 8.1 アプリからユニバーサル Windows プラットフォーム (UWP) アプリに適切に変換されます。 ほとんどのマークアップには互換性がありますが、場合によっては、使っているシステムのリソース キーやカスタム テンプレートを調整する必要があります。 ビュー モデルの命令型コードについては、ほとんどあるいはまったく変更する必要はありません。 プレゼンテーション層にあるほとんどのコード (UI 要素を操作するコード) も、簡単に移植できます。

## <a name="imperative-code"></a>命令型コード

プロジェクトのビルド段階に進むだけであれば、重要でないコードのコメントアウトやスタブの挿入を行うことができます。 次に、このセクションの以降のトピック (および前のトピック「[トラブルシューティング](w8x-to-uwp-troubleshooting.md)」) を参考にして、ビルドとランタイムの問題が解決して移植が完了するまで一度に 1 つの問題について反復作業を行います。

## <a name="adaptiveresponsive-ui"></a>アダプティブ/応答性の高い UI

アプリは、画面サイズと解像度が異なるさまざまなデバイスで実行できる場合があります。このため、最小限の手順でアプリを移植するだけでなく、各デバイスで最適な外観になるように UI を調整する必要があります。 アダプティブな Visual State Manager の機能を使って、ウィンドウのサイズを動的に検出し、それに応じてレイアウトを変更できます。その方法を示す例を、Bookstore2 ケース スタディの「[アダプティブ UI](w8x-to-uwp-case-study-bookstore2.md)」に示します。

## <a name="back-button-handling"></a>"戻る" ボタンの処理

ユニバーサル8.1 アプリの場合、Windows ランタイムの 8. x アプリと Windows Phone ストアアプリには、表示する UI と、[戻る] ボタンに対して処理するイベントの異なるアプローチがあります。 ただし、Windows 10 アプリの場合は、アプリで単一のアプローチを使用できます。 モバイル デバイスでは、このボタンはデバイス上の静電容量式のボタンまたはシェル内のボタンとして提供されます。 デスクトップ デバイスでは、アプリ内で戻るナビゲーションが可能な場合には常にアプリのクロムにボタンを追加します。このボタンは、ウィンドウ表示されたアプリのタイトル バーまたはタブレット モードのタスク バーに表示されます。 "戻る" ボタンのイベントはすべてのデバイス ファミリに共通するユニバーサルな概念であり、ハードウェアまたはソフトウェアに実装されるボタンは同じ [**BackRequested**](https://docs.microsoft.com/uwp/api/windows.ui.core.systemnavigationmanager.backrequested) イベントを発生させます。

次の例は、すべてのデバイス ファミリで動作し、同じ処理をすべてのページに適用する場合や、ナビゲーションを確認する必要がない場合 (保存していない変更に関する警告を表示する場合など) に適しています。

```csharp
   // app.xaml.cs

    protected override void OnLaunched(LaunchActivatedEventArgs e)
    {
        [...]

        Windows.UI.Core.SystemNavigationManager.GetForCurrentView().BackRequested += App_BackRequested;
        rootFrame.Navigated += RootFrame_Navigated;
    }

    private void RootFrame_Navigated(object sender, NavigationEventArgs e)
    {
        Frame rootFrame = Window.Current.Content as Frame;

        // Note: On device families that have no title bar, setting AppViewBackButtonVisibility can safely execute 
        // but it will have no effect. Such device families provide back button UI for you.
        if (rootFrame.CanGoBack)
        {
            Windows.UI.Core.SystemNavigationManager.GetForCurrentView().AppViewBackButtonVisibility = 
                Windows.UI.Core.AppViewBackButtonVisibility.Visible;
        }
        else
        {
            Windows.UI.Core.SystemNavigationManager.GetForCurrentView().AppViewBackButtonVisibility = 
                Windows.UI.Core.AppViewBackButtonVisibility.Collapsed;
        }
    }

    private void App_BackRequested(object sender, Windows.UI.Core.BackRequestedEventArgs e)
    {
        Frame rootFrame = Window.Current.Content as Frame;

        if (rootFrame.CanGoBack)
        {
            rootFrame.GoBack();
        }
    }
```

プログラムを使ったアプリの終了に関しても、すべてのデバイス ファミリに対する単一のアプローチがあります。

```csharp
   Windows.UI.Xaml.Application.Current.Exit();
```

## <a name="charms"></a>チャーム

Charms と統合するコードを変更する必要はありませんが、アプリにいくつかの UI を追加して、Windows 10 シェルの一部ではない Charms バーの代わりに使用する必要があります。 Windows 10 で実行されているユニバーサル8.1 アプリには、アプリケーションのタイトルバーにシステムレンダリング chrome によって提供される独自の置換 UI があります。

## <a name="controls-and-control-styles-and-templates"></a>コントロールとコントロール スタイルおよびテンプレート

Windows 10 で実行されている Universal 8.1 アプリは、コントロールに関して8.1 の外観と動作を保持します。 ただし、そのアプリを Windows 10 アプリに移植する場合、外観と動作にはいくつかの違いがあります。 コントロールのアーキテクチャと設計は、基本的には Windows 10 アプリでは変更されません。そのため、ほとんどの変更はデザイン言語、簡略化、およびユーザビリティの向上に関するものです。

視覚的な状態に対するポインターは、Windows 10 アプリのカスタムスタイル/テンプレート、および Windows ランタイム 8. x アプリでは必要ですが、Windows Phone ストアアプリでは関連していませ**ん  。** この理由 (および Windows 10 アプリでサポートされているシステムリソースキー) では、アプリを Windows 10 に移植するときに、Windows ランタイム1.x アプリからカスタムスタイルまたはテンプレートを再利用することをお勧めします。
カスタムスタイルまたはテンプレートが最新の表示状態のセットを使用していることを確認し、既定のスタイルまたはテンプレートに加えられたパフォーマンスの向上からドラマする場合は、新しい Windows 10 の既定のテンプレートのコピーを編集し、カスタマイズを再適用します。 パフォーマンス向上の 1 つの例として、以前に **ContentPresenter** または Panel を囲んでいた **Border** が削除され、子要素が境界線を表示するようになりました。

以下に、コントロールの変更に関する具体的な例を示します。

| コントロール名 | [Change] |
|--------------|--------|
| **AppBar**   | **Appbar**コントロールを使用している場合 (代わりに[**CommandBar**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBar)を使用することをお勧めします)、Windows 10 アプリでは既定で非表示になりません。 これを制御するには、[**AppBar.ClosedDisplayMode**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbar.closeddisplaymode) プロパティを使います。 |
| **AppBar**、[**CommandBar**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBar) | Windows 10 アプリでは、 **Appbar**と[**CommandBar**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBar)に **[詳細]** ボタン (省略記号) が表示されます。 |
| [**CommandBar**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBar) | Windows ランタイム2.x アプリでは、 [**CommandBar**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBar)のセカンダリコマンドは常に表示されます。 Windows Phone ストアアプリでは、Windows 10 アプリでは、コマンドバーが開くまでは表示されません。 |
| [**CommandBar**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBar) | Windows Phone ストア アプリでは、[**CommandBar.IsSticky**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbar.issticky) の値は、バーの簡易非表示の動作に影響しません。 Windows 10 アプリの場合、 **Issticky**が true に設定されていると、 **CommandBar**は、電球を消すジェスチャを無視します。 |
| [**CommandBar**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBar) | Windows 10 アプリでは、 [**CommandBar**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBar)は[**EdgeGesture**](https://docs.microsoft.com/uwp/api/windows.ui.input.edgegesture.completed)イベントや[**RightTapped**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.righttapped)イベントを処理しません。 タップまたはスワイプにも応答しません。 ただし、これらのイベントを処理し、[**IsOpen**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbar.isopen) を設定するオプションがあります。 |
| [**DatePicker**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.DatePicker)、 [ **timepicker**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TimePicker) | [  **DatePicker**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.DatePicker) や [**TimePicker**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TimePicker) に加えられた視覚的な変化によってアプリの外観がどうなるかを確認してください。 モバイルデバイスで実行されている Windows 10 アプリの場合、これらのコントロールは選択ページに移動せず、代わりに無視できるポップアップを使用します。 |
| [**DatePicker**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.DatePicker)、 [ **timepicker**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TimePicker) | Windows 10 アプリでは、 [**DatePicker**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.DatePicker)または[**timepicker**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TimePicker)をフライアウト内に配置することはできません。これらのコントロールをポップアップ型のコントロールに表示する場合は、 [**DatePickerFlyout**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.DatePickerFlyout)と[**TimePickerFlyout**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TimePickerFlyout)を使用できます。 |
| **GridView**、**ListView** | **GridView**/**ListView** については、「[GridView と ListView の変更](#gridview-and-listview-changes)」をご覧ください。 |
| [**Hub**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Hub) | Windows Phone ストア アプリでは、[**Hub**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Hub) コントロールは最後のセクションから最初のセクションに折り返します。 Windows ランタイム 8. x アプリと Windows 10 アプリでは、ハブのセクションは折り返されません。 |
| [**Hub**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Hub) | Windows Phone ストア アプリでは、[**Hub**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Hub) コントロールの背景画像は、ハブ セクションに対する視差効果で移動します。 Windows ランタイム 8. x アプリと Windows 10 アプリでは、視差は使用されません。 |
| [**Hub**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Hub)  | ユニバーサル 8.1 アプリでは、[**HubSection.IsHeaderInteractive**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.hubsection.isheaderinteractive) プロパティにより、セクション ヘッダーとその横に表示される山形のグリフが対話型になります。 Windows 10 アプリでは、ヘッダーの横に対話型の "see more" affordance がありますが、ヘッダー自体は対話型ではありません。 **IsHeaderInteractive** により、操作で [**Hub.SectionHeaderClick**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.hub.sectionheaderclick) イベントが発生するかどうかが決まります。 |
| **Windows.ui.popups.messagedialog** | **MessageDialog** を使っている場合は、柔軟性が向上した [**ContentDialog**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentDialog) の利用を検討してください。 [XAML UI の基本](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) のサンプルに関するページもご覧ください。 |
| **ListPickerFlyout**、**PickerFlyout**  | **ListPickerFlyout**と**PickerFlyout**は、Windows 10 アプリでは非推奨とされます。 単一選択ポップアップの場合は、[**MenuFlyout**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MenuFlyout) を使います。より複雑なエクスペリエンスの場合は、[**Flyout**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Flyout) を使います。 |
| [**PasswordBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.PasswordBox) | [**IsPasswordRevealButtonEnabled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.passwordbox.ispasswordrevealbuttonenabled)プロパティは、Windows 10 アプリでは非推奨とされており、設定しても効果はありません。 代わりに[**PasswordRevealMode**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.passwordbox.passwordrevealmode)を使用します。既定では、 **Peek** (Windows ランタイム 8. x アプリのような目のグリフが表示されます) が既定で選択されます。 「[パスワード ボックスのガイドライン](https://docs.microsoft.com/windows/uwp/controls-and-patterns/password-box)」もご覧ください。 |
| [**ピボット**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot) | [  **Pivot**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot) はユニバーサル コントロールとなり、モバイル デバイスでの利用のみに限定されていた制限が排除されました。 |
| [**SearchBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SearchBox) | ユニバーサル デバイス ファミリでは [**SearchBox**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.searchbox) が実装されていますが、モバイル デバイスでは部分的に機能しません。 「[AutoSuggestBox に使用されない SearchBox](#searchbox-deprecated-in-favor-of-autosuggestbox)」をご覧ください。 |
| **SemanticZoom** | **SemanticZoom** については、「[SemanticZoom に関する変更](#semanticzoom-changes)」をご覧ください。 |
| [**ScrollViewer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer)  | [  **ScrollViewer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer) の既定のプロパティの一部が変更されています。 [**水平スクロールモード**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.horizontalscrollmode)は**auto**、[**垂直スクロールモード**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.verticalscrollmode)は**auto**、 [**ZoomMode**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.zoommode)は**無効に**なっています。 新しい既定値がアプリに対して適切でない場合は、スタイルで変更するか、コントロール自体のローカル値として変更できます。  |
| [**Wl**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox) | Windows ランタイム2.x アプリでは、[**テキストボックス**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox)のスペルチェックが既定でオフになっています。 Windows Phone ストアアプリでは、Windows 10 アプリでは既定でオンになっています。 |
| [**Wl**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox) | [  **TextBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox) の既定のフォント サイズは 11 から 15 に変更されました。 |
| [**Wl**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox) | [  **TextBox.TextReadingOrder**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.textreadingorder) の既定値は、**Default** から **DetectFromContent** に変更されました。 これが望ましくない場合は、**UseFlowDirection** を使ってください。 **Default** は使われなくなりました。 |
| Various | アクセントカラーは、Windows Phone ストアアプリと Windows 10 アプリに適用されますが、Windows ランタイム 8. x アプリには適用されません。  |

UWP アプリのコントロールについて詳しくは、「[機能別コントロール](https://docs.microsoft.com/windows/uwp/controls-and-patterns/controls-by-function)」、「[コントロールの一覧](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/)」、「[コントロールのガイドライン](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/index)」をご覧ください。

##  <a name="design-language-in-windows10"></a>Windows 10 のデザイン言語

ユニバーサル8.1 アプリと Windows 10 アプリの間では、デザイン言語に多少の違いがありますが、重要な違いがいくつかあります。 詳しくは、「[Design](https://developer.microsoft.com/en-us/windows/apps/design)」(UWP アプリの設計) をご覧ください。 デザイン言語に変更が加えられていますが、設計原則は維持されています。細部にまで注意を払いながら、簡潔さを追求しています。そのために、クロムよりもコンテンツを優先し、視覚要素を大幅に減らし、真のデジタル領域を常に意識しています。また、視覚的な階層の利用 (特に文字体裁に対して)、グリッド内でのデザイン、滑らかなアニメーションを使ったエクスペリエンスの実現も行っています。

## <a name="effective-pixels-viewing-distance-and-scale-factors"></a>有効ピクセル、視聴距離、スケール ファクター

これまで、表示ピクセルは、デバイスの実際の物理サイズと解像度から UI 要素のサイズとレイアウトを抽象化する方法でした。 現在では、表示ピクセルが有効ピクセルに変わりました。その用語の説明、有効ピクセルが何をするものなのか、および有効ピクセルで使うことができる追加の値について、以下に示します。

一般的な考えとは異なり、"解像度" という用語はピクセル密度の測定値を表しており、ピクセル数ではありません。 "有効解像度" は、画像またはグリフを構成する物理ピクセルを解決して、デバイスの視聴距離と物理ピクセル サイズでの目視による相違の度合を取得する方法です (物理ピクセル サイズの逆数であるピクセル密度)。 有効解像度は、ユーザー中心であるために、エクスペリエンスの構築に適したメトリックです。 すべての要因について理解し、UI 要素のサイズを制御することによって、ユーザーのエクスペリエンスを適切なものにすることができます。

デバイスによって、有効ピクセルの幅の値が異なります。その範囲は、320 epx (最小のデバイス) から 1024 epx (一般的なサイズのモニター)、またはそれ以上のさらに広い幅になります。 これまでと同様に、自動的にサイズ調整される要素と動的レイアウト パネルを引き続き使うことで十分に対応できます。 ただし、場合によっては、UI 要素のプロパティを XAML マークアップで固定サイズに設定することがあります。 スケール ファクターは、アプリが実行されているデバイスやユーザーが行った表示設定に応じて、アプリに自動的に適用されます。 スケール ファクターによって、さまざまな幅の画面サイズでユーザーに対してほぼ一定サイズのタッチ (または読み取り) ターゲットを提示するように、すべての UI 要素を固定サイズで維持できます。 また、動的レイアウトと共に使うと、UI は単にさまざまなデバイスで光学的なスケーリングを行うだけではありません。 利用可能な領域に対して適切な量のコンテンツを表示するために必要な処理も実行します。

すべてのディスプレイで最適なアプリのエクスペリエンスが実現できるように、一連のサイズで各ビットマップ アセットを作成し、各アセットが特定のスケール ファクターに適合するように設定することをお勧めします。 ただし、100% スケール、200% スケール、および 400% スケール (この優先順位で) でアセットを作成するほうが、多くの場合、すべての中間スケール ファクターで適切な結果を得ることができます。

理由によっては、複数のサイズでアセットを作成することができない場合**は  、** 100% スケールのアセットを作成します。 Microsoft Visual Studio では、UWP アプリの既定のプロジェクト テンプレートには 1 つのサイズのみのブランド アセット (タイル イメージとロゴ) が用意されていますが、これらは 100% のスケールではありません。 独自のアプリのアセットを作成する場合は、このセクションに示したガイドラインに従って、100%、200%、400% のサイズを用意し、アセット パックを使います。

複雑なアートワークがある場合は、さらに多くのサイズに対応したアセットが必要になることがあります。 ベクター アートを使って作業を始める場合は、どのようなスケール ファクターでも高品質なアセットを比較的簡単に生成できます。

すべてのスケールファクターをサポートすることはお勧めしませんが、Windows 10 アプリのスケールファクターの完全な一覧は、100%、125%、150%、200%、250%、300%、および400% です。 すべてのスケール ファクターのアセットを提供した場合、ストアでは、各デバイスに合った適切なサイズのアセットが選ばれ、それらのアセットのみがダウンロードされます。 ストアでは、デバイスの DPI に基づいて、ダウンロードするアセットが選ばれます。 Windows ランタイム2.x アプリのアセットは、140% や220% などの大規模な要因で再利用できますが、アプリは新しいスケールファクターの1つで実行されるため、一部のビットマップスケーリングは避けられません。 状況に合った最適な結果を得ることができるかどうかを確認するために、さまざまなデバイスでアプリをテストしてください。

Windows ランタイム 8. x アプリから XAML マークアップを再利用することができます。このアプリでは、リテラルのディメンション値がマークアップで使用されます (たとえば、文字体裁の場合など、図形や他の要素のサイズを設定する場合)。 ただし、場合によっては、ユニバーサル8.1 アプリの場合よりも、Windows 10 アプリのデバイスでより大きなスケールファクターが使用されます (たとえば、140% が実行されている場合は150% が使用され、180% がの場合は200% が使用されます)。 そのため、これらのリテラル値が Windows 10 で大きすぎることがわかった場合は、0.8 で乗算してみてください。 詳しくは、「[UWP アプリ用レスポンシブ デザイン 101](https://docs.microsoft.com/windows/uwp/layout/screen-sizes-and-breakpoints-for-responsive-design)」をご覧ください。

## <a name="gridview-and-listview-changes"></a>GridView と ListView の変更

コントロールを縦方向へスクロールするために、[**GridView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView) の既定の style setter に対していくつかの変更が行われました (横方向へのスクロールではありません。これについては既定で対応しています)。 プロジェクトに含まれている既定のスタイルのコピーを編集した場合、そのコピーにはこれらの変更が適用されていないため、手動で変更を加える必要があります。 一連の変更を以下にまとめます。

-   [  **ScrollViewer.HorizontalScrollBarVisibility**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.horizontalscrollbarvisibility) の setter は、**Auto** から **Disabled** に変更されました。
-   [  **ScrollViewer.VerticalScrollBarVisibility**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.verticalscrollbarvisibility) の setter は、**Disabled** から **Auto** に変更されました。
-   [  **ScrollViewer.HorizontalScrollMode**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.horizontalscrollmode) の setter は、**Enabled** から **Disabled** に変更されました。
-   [  **ScrollViewer.VerticalScrollMode**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.verticalscrollmode) の setter は、**Disabled** から **Enabled** に変更されました。
-   [  **ItemsPanel**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemspanel) の setter では、[**ItemsWrapGrid.Orientation**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemswrapgrid.orientation) の値が **Vertical** から **Horizontal** に変更されました。

最後の変更 (**Orientation** に対する変更) は矛盾していると考えられますが、ここでは折り返しグリッドについて説明していることに注意してください。 横方向の折り返しグリッド (新しい値) は、テキストが横方向に表示され、ページの終端で次の行に改行される書記体系と類似しています。 そのようなテキストのページでは、縦方向にスクロールします。 これに対して、縦方向の折り返しグリッド (以前の値) は、テキストが縦方向に表示される書記体系と類似しています。このため、横方向にスクロールします。

次に示すのは、変更または Windows 10 でサポートされていない[**GridView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView)と[**ListView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView)の側面です。

-   [**IsSwipeEnabled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.isswipeenabled)プロパティ (Windows ランタイム8.x アプリのみ) は、Windows 10 アプリではサポートされていません。 API は存在しますが、API を設定しても効果はありません。 これまでのすべての選択ジェスチャがサポートされていますが、下方向へのスワイプと右クリックはサポートされていません。これは、下方向へのスワイプではデータが検出不可能と示されるためです。また、右クリックはコンテキスト メニューの表示用に予約されているためです。
-   [**ReorderMode**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.reordermode)プロパティ (Windows Phone ストアアプリのみ) は、Windows 10 アプリではサポートされていません。 API は存在しますが、API を設定しても効果はありません。 代わりに、[GridView**や**ListView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.allowdrop) に対して [**AllowDrop**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.canreorderitems) とCanReorderItems を true に設定することで、ユーザーは、長押し (またはクリックしてドラッグ) のジェスチャを使って順序を変更することができます。
-   Windows 10 用に開発する場合は、 [**ListView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView)と[**GridView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView)の両方で、項目コンテナースタイルの[**GridViewItemPresenter**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.GridViewItemPresenter)ではなく[**ListViewItemPresenter**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ListViewItemPresenter)を使用します。 アイテム コンテナーの既定のスタイルをコピーして編集する場合は、適切な種類のスタイルを取得できます。
-   Windows 10 アプリの選択ビジュアルが変更されました。 [  **SelectionMode**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.selectionmode) を **Multiple** に設定すると、既定では、各項目に対してチェック ボックスが表示されます。 **ListView** 項目の既定の設定では、チェック ボックスが項目の横にインラインで配置されます。その結果、他の項目が占める領域が若干小さくなり、移動されます。 **GridView** 項目の場合、既定では、チェック ボックスが項目の上部に重なって表示されます。 ただしどちらの場合も、チェック ボックスのレイアウト (インラインまたはオーバーレイ) を制御できます ([**CheckMode**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.checkmode) プロパティを使う)。また、アイテム コンテナーのスタイル内にある [**ListViewItemPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.selectioncheckmarkvisualenabled) 要素上にチェック ボックスをすべて表示するかどうかを制御することもできます ([**SelectionCheckMarkVisualEnabled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter) プロパティを使う)。次に例を示します。
-   Windows 10 では、UI の仮想化中に、 [**ContainerContentChanging**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.containercontentchanging)イベントが項目ごとに2回発生します。1回は再利用され、もう一度使用する場合は1回です。 [  **InRecycleQueue**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.containercontentchangingeventargs.inrecyclequeue) の値が **true** で、特別な回収操作がない場合、同じ項目が再利用されるとき (**InRecycleQueue** が **false** になる場合) には再入力されることが確実であれば、すぐにイベント ハンドラーを終了することができます。

```xml
<Style x:Key="CustomItemContainerStyle" TargetType="ListViewItem|GridViewItem">
    ...
    <Setter.Value>
        <ControlTemplate TargetType="ListViewItem|GridViewItem">
            <ListViewItemPresenter CheckMode="Inline|Overlay" ... />
        </ControlTemplate>
    </Setter.Value>
    ...
</Style>
```

![インライン チェック ボックスを使った ListViewItemPresenter](images/w8x-to-uwp-case-studies/ui-listviewbase-cb-inline.jpg)

インライン チェック ボックスを使った ListViewItemPresenter

![重ねて表示されるチェック ボックスを使った ListViewItemPresenter](images/w8x-to-uwp-case-studies/ui-listviewbase-cb-overlay.jpg)

重ねて表示されるチェック ボックスを使った ListViewItemPresenter

-   選択する際の下方向へのスワイプと右クリックのジェスチャがサポートされなくなったため (理由については上記のとおり)、相互作用モデルが変更されました。その結果の 1 つとして、[**ItemClick**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.itemclick) イベントと [**SelectionChanged**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) イベントは相互に排他的ではなくなりました。 Windows 10 アプリの場合は、シナリオを確認し、"選択" または "呼び出し" の相互作用モデルを採用するかどうかを決定します。 詳しくは、「[操作モードを変更する方法](https://docs.microsoft.com/previous-versions/windows/apps/hh780625(v=win.10))」をご覧ください。
-   [  **ListViewItemPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter) のスタイルを指定する際に使うプロパティについて、変更がいくつかあります。 新しいプロパティは、[**CheckBoxBrush**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.checkboxbrush)、[**PressedBackground**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.pressedbackground)、[**SelectedPressedBackground**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.selectedpressedbackground)、[**FocusSecondaryBorderBrush**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.focussecondaryborderbrush) です。 Windows 10 アプリで無視されるプロパティは、[**パディング**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.padding)(代わりに[**contentmargin**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.contentmargin)を使用)、 [**checkhintbrush**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.checkhintbrush)、 [**CheckSelectingBrush**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.checkselectingbrush)、[**ポインター Overbackgroundmargin**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.pointeroverbackgroundmargin)、 [**ReorderHintOffset**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.reorderhintoffset)、 [**SelectedBorderThickness**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.selectedborderthickness)、 [**selectedポインタ overborderbrush**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.selectedpointeroverborderbrush)です。

次の表では、[**ListViewItem**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListViewItem) コントロール テンプレートと [**GridViewItem**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridViewItem) コントロール テンプレートでの表示状態や表示状態グループに対する変更について説明します。

| 8.1                 |                         | Windows 10        |                     |
|---------------------|-------------------------|-------------------|---------------------|
| CommonStates        |                         | CommonStates      |                     |
|                     | 標準                  |                   | 標準              |
|                     | PointerOver             |                   | PointerOver         |
|                     | Pressed                 |                   | Pressed             |
|                     | PointerOverPressed      |                   | [利用不可]       |
|                     | 無効                |                   | [利用不可]       |
|                     | [利用不可]           |                   | PointerOverSelected |
|                     | [利用不可]           |                   | Selected            |
|                     | [利用不可]           |                   | PressedSelected     |
| [利用不可]       |                         | DisabledStates    |                     |
|                     | [利用不可]           |                   | 無効            |
|                     | [利用不可]           |                   | 有効             |
| SelectionHintStates |                         | [利用不可]     |                     |
|                     | VerticalSelectionHint   |                   | [利用不可]       |
|                     | HorizontalSelectionHint |                   | [利用不可]       |
|                     | NoSelectionHint         |                   | [利用不可]       |
| [利用不可]       |                         | MultiSelectStates |                     |
|                     | [利用不可]           |                   | MultiSelectDisabled |
|                     | [利用不可]           |                   | MultiSelectEnabled  |
| SelectionStates     |                         | [利用不可]     |                     |
|                     | Unselecting             |                   | [利用不可]       |
|                     | Unselected              |                   | [利用不可]       |
|                     | UnselectedPointerOver   |                   | [利用不可]       |
|                     | UnselectedSwiping       |                   | [利用不可]       |
|                     | 選択               |                   | [利用不可]       |
|                     | Selected                |                   | [利用不可]       |
|                     | SelectedSwiping         |                   | [利用不可]       |
|                     | SelectedUnfocused       |                   | [利用不可]       |

カスタムの [**ListViewItem**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListViewItem) コントロール テンプレートまたは [**GridViewItem**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridViewItem) コントロール テンプレートを使っている場合は、上記の変更を踏まえてテンプレートを確認してください。 新しい既定のテンプレートのコピーを編集し、そのコピーにカスタマイズを再適用して、コントロール テンプレートのカスタマイズをやり直すことをお勧めします。 こうした作業をできない理由があり、既にあるテンプレートの編集が必要になる場合は、既存のテンプレートを編集する方法に関する次の一般的なガイダンスを参考にしてください。

-   新しい MultiSelectStates 表示状態グループを追加します。
-   新しい MultiSelectDisabled 表示状態を追加します。
-   新しい MultiSelectEnabled 表示状態を追加します。
-   新しい DisabledStates 表示状態グループを追加します。
-   新しい Enabled 表示状態を追加します。
-   CommonStates 表示状態グループから PointerOverPressed 表示状態を削除します。
-   Disabled 表示状態を DisabledStates 表示状態グループに移動します。
-   新しい PointerOverSelected 表示状態を追加します。
-   新しい PressedSelected 表示状態を追加します。
-   SelectedHintStates 表示状態グループを削除します。
-   SelectionStates 表示状態グループの Selected 表示状態を CommonStates 表示状態グループに移動します。
-   SelectionStates 表示状態グループ全体を削除します。

## <a name="localization-and-globalization"></a>ローカリゼーションとグローバリゼーション

UWP アプリ プロジェクトで、ユニバーサル 8.1 プロジェクトの Resources.resw ファイルを再利用できます。 このファイルをコピーしてから、プロジェクトに追加し、 **[ビルド アクション]** を **[PRIResource]** に、 **[出力ディレクトリにコピー]** を **[コピーしない]** に設定します。 「[**ResourceContext.QualifierValues**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.resources.core.resourcecontext.qualifiervalues)」トピックでは、デバイス ファミリのリソースを選ぶ要因に基づいてデバイス ファミリ固有のリソースを読み込む方法について説明しています。

## <a name="play-to"></a>リモート再生

Windows 10 アプリでは、windows[**の media. 名前空間**](https://docs.microsoft.com/uwp/api/Windows.Media.PlayTo)の api は非推奨とされて[**います。** ](https://docs.microsoft.com/uwp/api/Windows.Media.Casting)

## <a name="resource-keys-and-textblock-style-sizes"></a>リソース キー、および TextBlock スタイル サイズ

Windows 10 のデザイン言語が進化し、その結果、特定のシステムスタイルが変更されました。 場合によっては、変更されたスタイルのプロパティにビューが適合するように、ビューのビジュアル デザインに戻る必要があります。

それ以外の場合、リソース キーはサポートされなくなりました。 Visual Studio の XAML マークアップ エディターでは、解決できないリソース キーへの参照が強調表示されます。 たとえば XAML マークアップ エディターでは、スタイル キー `ListViewItemTextBlockStyle` への参照の下に赤い波線が引かれます。 これを修正しない場合、エミュレーターかデバイスに展開しようとしたときにアプリが直ちに終了します。 したがって、XAML マークアップの正確性に関する作業に着手することが重要です。 また、そのような問題を検出するために Visual Studio が優れたツールであることがわかります。

まだサポートされているキーに関して、デザイン言語の変更は、一部のスタイルによって設定されるプロパティが変更されたことを意味します。 たとえば、`TitleTextBlockStyle` は、Windows Phone ストアアプリの Windows ランタイム 8. x アプリと 18.14 px で、 **FontSize**を 14.667 px に設定します。 ただし、同じスタイルで、Windows 10 アプリでは、**フォント**サイズが非常に大きい24px に設定されています。 デザインとレイアウトを確認し、適切なスタイルを適切な場所で使ってください。 詳しくは、「[フォントのガイドライン](https://docs.microsoft.com/windows/uwp/controls-and-patterns/fonts)」と「[UWP アプリの設計](https://developer.microsoft.com/en-us/windows/apps/design)」をご覧ください。

サポートされなくなったキーの完全な一覧を次に示します。

-   CheckBoxAndRadioButtonMinWidthSize
-   CheckBoxAndRadioButtonTextPaddingThickness
-   ComboBoxFlyoutListPlaceholderTextOpacity
-   ComboBoxFlyoutListPlaceholderTextThemeMargin
-   ComboBoxHighlightedBackgroundThemeBrush
-   ComboBoxHighlightedBorderThemeBrush
-   ComboBoxHighlightedForegroundThemeBrush
-   ComboBoxInlinePlaceholderTextForegroundThemeBrush
-   ComboBoxInlinePlaceholderTextThemeFontWeight
-   ComboBoxItemDisabledThemeOpacity
-   ComboBoxItemHighContrastBackgroundThemeMargin
-   ComboBoxItemMinHeightThemeSize
-   ComboBoxPlaceholderTextBlockStyle
-   ComboBoxPlaceholderTextThemeMargin
-   CommandBarBackgroundThemeBrush
-   CommandBarForegroundThemeBrush
-   ContentDialogButton1HostPadding
-   ContentDialogButton2HostPadding
-   ContentDialogButtonsMinHeight
-   ContentDialogContentLandscapeWidth
-   ContentDialogContentMinHeight
-   ContentDialogDimmingColor
-   ContentDialogTitleMinHeight
-   ControlContextualInfoTextBlockStyle
-   ControlHeaderContentPresenterStyle
-   ControlHeaderTextBlockStyle
-   FlyoutContentPanelLandscapeThemeMargin
-   FlyoutContentPanelPortraitThemeMargin
-   GrabberMargin
-   GridViewItemMargin
-   GridViewItemPlaceholderBackgroundThemeBrush
-   GroupHeaderTextBlockStyle
-   HeaderContentPresenterStyle
-   HighContrastBlack
-   HighContrastWhite
-   HubHeaderCharacterSpacing
-   HubHeaderFontSize
-   HubHeaderMarginThickness
-   HubSectionHeaderCharacterSpacing
-   HubSectionHeaderFontSize
-   HubSectionHeaderMarginThickness
-   HubSectionMarginThickness
-   InlineWindowPlayPauseMargin
-   ItemTemplate
-   LeftFullWindowMargin
-   LeftMargin
-   ListViewEmptyStaticTextBlockStyle
-   ListViewItemContentTextBlockStyle
-   ListViewItemContentTranslateX
-   ListViewItemMargin
-   ListViewItemMultiselectCheckBoxMargin
-   ListViewItemSubheaderTextBlockStyle
-   ListViewItemTextBlockStyle
-   MediaControlPanelAudioThemeBrush
-   MediaControlPanelPhoneVideoThemeBrush
-   MediaControlPanelVideoThemeBrush
-   MediaControlPanelVideoThemeColor
-   MediaControlPlayPauseThemeBrush
-   MediaControlTimeRowThemeBrush
-   MediaControlTimeRowThemeColor
-   MediaDownloadProgressIndicatorThemeBrush
-   MediaErrorBackgroundThemeBrush
-   MediaTextThemeBrush
-   MenuFlyoutBackgroundThemeBrush
-   MenuFlyoutBorderThemeBrush
-   MenuFlyoutLandscapeThemePadding
-   MenuFlyoutLeftLandscapeBorderThemeThickness
-   MenuFlyoutPortraitBorderThemeThickness
-   MenuFlyoutPortraitThemePadding
-   MenuFlyoutRightLandscapeBorderThemeThickness
-   MessageDialogContentStyle
-   MessageDialogTitleStyle
-   MinimalWindowMargin
-   PasswordBoxCheckBoxThemeMargin
-   PhoneAccentBrush
-   PhoneBackgroundBrush
-   PhoneBackgroundColor
-   PhoneBaseBlackColor
-   PhoneBaseHighColor
-   PhoneBaseLowColor
-   PhoneBaseLowSolidColor
-   PhoneBaseMediumHighColor
-   PhoneBaseMediumMidColor
-   PhoneBaseMediumMidSolidColor
-   PhoneBaseMidColor
-   PhoneBaseWhiteColor
-   PhoneBorderThickness
-   PhoneButtonBasePressedForegroundBrush
-   PhoneButtonContentPadding
-   PhoneButtonFontWeight
-   PhoneButtonMinHeight
-   PhoneButtonMinWidth
-   PhoneChromeBrush
-   PhoneChromeColor
-   PhoneControlBackgroundColor
-   PhoneControlDisabledColor
-   PhoneControlForegroundColor
-   PhoneDisabledBrush
-   PhoneDisabledColor
-   PhoneFontFamilyLight
-   PhoneFontFamilySemiBold
-   PhoneForegroundBrush
-   PhoneForegroundColor
-   PhoneHighContrastSelectedBackgroundThemeBrush
-   PhoneHighContrastSelectedForegroundThemeBrush
-   PhoneImagePlaceholderColor
-   PhoneLowBrush
-   PhoneMidBrush
-   PhonePageBackgroundColor
-   PhonePivotLockedTranslation
-   PhonePivotUnselectedItemOpacity
-   PhoneRadioCheckBoxBorderBrush
-   PhoneRadioCheckBoxBrush
-   PhoneRadioCheckBoxCheckBrush
-   PhoneRadioCheckBoxPressedBrush
-   PhoneStrokeThickness
-   PhoneTextHighColor
-   PhoneTextLowColor
-   PhoneTextMidColor
-   PhoneTextOverAccentColor
-   PhoneTouchTargetLargeOverhang
-   PhoneTouchTargetOverhang
-   PivotHeaderItemPadding
-   PlaceholderContentPresenterStyle
-   ProgressBarHighContrastAccentBarThemeBrush
-   ProgressBarIndeterminateRectagleThemeSize
-   ProgressBarRectangleStyle
-   ProgressRingActiveBackgroundOpacity
-   ProgressRingElipseThemeMargin
-   ProgressRingElipseThemeSize
-   ProgressRingTextForegroundThemeBrush
-   ProgressRingTextThemeMargin
-   ProgressRingThemeSize
-   RichEditBoxTextThemeMargin
-   RightFullWindowMargin
-   RightMargin
-   ScrollBarMinThemeHeight
-   ScrollBarMinThemeWidth
-   ScrollBarPanningThumbThemeHeight
-   ScrollBarPanningThumbThemeWidth
-   SliderThumbDisabledBorderThemeBrush
-   SliderTrackBorderThemeBrush
-   SliderTrackDisabledBorderThemeBrush
-   TextBoxBackgroundColor
-   TextBoxBorderColor
-   TextBoxDisabledHeaderForegroundThemeBrush
-   TextBoxFocusedBackgroundThemeBrush
-   TextBoxForegroundColor
-   TextBoxPlaceholderColor
-   TextControlHeaderMarginThemeThickness
-   TextControlHeaderMinHeightSize
-   TextStyleExtraExtraLargeFontSize
-   TextStyleExtraLargePlusFontSize
-   TextStyleMediumFontSize
-   TextStyleSmallFontSize
-   TimeRemainingElementMargin

## <a name="searchbox-deprecated-in-favor-of-autosuggestbox"></a>SearchBox に代わって使われる AutoSuggestBox

ユニバーサル デバイス ファミリでは [**SearchBox**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.searchbox) が実装されていますが、モバイル デバイスでは部分的に機能しません。 ユニバーサル検索エクスペリエンスには [**AutoSuggestBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AutoSuggestBox) を使います。 **AutoSuggestBox** を使った検索エクスペリエンスの通常の実装方法を次に示します。

入力を開始すると、**UserInput** という理由で **TextChanged** イベントが発生します。 次に、候補の一覧を設定し、AutoSuggestBox[**の**ItemsSource](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AutoSuggestBox) を設定します。 ユーザーが一覧を移動すると、**SuggestionChosen** イベントが発生します (また、**TextMemberDisplayPath** を設定すると、指定されたプロパティに基づいてテキスト ボックスが自動的に入力されます)。 ユーザーが Enter キーで選択項目を送信すると、**QuerySubmitted** イベントが発生し、その時点で、選ばれた候補に対するアクションを実行できます (多くの場合、指定されたコンテンツの詳細を示す別のページに移動するというアクションが実行されます)。 **SearchBoxQuerySubmittedEventArgs** の **LinguisticDetails** プロパティと **Language** プロパティはサポートされなくなった点に注意してください (その機能をサポートする同等の API はあります)。 また、**KeyModifiers** もサポートされなくなりました。

[**AutoSuggestBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AutoSuggestBox)では、ime (input method editor) もサポートされています。 必要に応じて "検索" アイコンを表示できます (このアイコンを操作すると **QuerySubmitted** イベントが発生します)。

```xml
   <AutoSuggestBox ... >
        <AutoSuggestBox.QueryIcon>
            <SymbolIcon Symbol="Find"/>
        </AutoSuggestBox.QueryIcon>
    </AutoSuggestBox>
```

[AutoSuggestBox の移植のサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlAutoSuggestBox)に関するページもご覧ください。

## <a name="semanticzoom-changes"></a>SemanticZoom に関する変更

[  **SemanticZoom**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) での縮小表示のジェスチャは、Windows Phone モデルで収束され、グループ ヘッダーをタップまたはクリックするという動作になりました (このため、デスクトップ コンピューターでは、縮小表示するためのマイナス記号のボタンのアフォーダンスは表示されなくなります)。 これで、デバイスに関係なく同一で一貫性のある動作を実現できます。 Windows Phone モデルと外観面で異なる点の 1 つは、縮小表示ビュー (ジャンプ リスト) が、拡大表示ビューをオーバーレイするのではなく、拡大表示ビューを置き換えることです。 このため、縮小表示ビューから半透明の背景を削除することができます。

Windows Phone ストアアプリでは、縮小表示は画面のサイズに拡大されます。 Windows ランタイム2.x アプリと Windows 10 アプリでは、ズームアウトされたビューのサイズは**SemanticZoom**コントロールの境界に制限されます。

Windows Phone ストア アプリでは、縮小表示ビューで背景に透明度が設定されている場合、そのビューの (Z オーダーに基づく) 背後のコンテンツが透けて表示されます。 Windows ランタイム2.x アプリでは、Windows 10 アプリでは、縮小表示の背後には何も表示されません。

Windows ランタイム2.x アプリでは、アプリが非アクティブ化されて再アクティブ化されると、縮小表示されたビューが (表示されている場合は) 表示されなくなり、ズームインビューが代わりに表示されます。 Windows Phone ストアアプリでは、Windows 10 アプリでは、表示されていた場合でも、拡大表示されたビューは表示されたままになります。

Windows Phone ストアアプリでは、Windows 10 アプリでは、[戻る] ボタンが押されると、縮小表示が消えます。 Windows ランタイム2.x アプリの場合、組み込みの [戻る] ボタンの処理はないため、この質問は適用されません。

## <a name="settings"></a>設定

Windows ランタイム 8. x **SettingsPane**クラスは、Windows 10 には適していません。 代わりに、[設定] ページを作成し、さらに、アプリ内から [設定] ページにアクセスする方法をユーザーに提供する必要があります。 このアプリの [設定] ページはトップ レベルで表示されるようにすることをお勧めしますが、ナビゲーション ウィンドウの最後のピン留めされた項目として、ここにはオプションの完全なセットがあります。

-   ナビゲーション ウィンドウ。 設定は選択肢のナビゲーション リストの最後の項目であり、下部にピン留めしている必要があります。
-   アプリ バー/ツール バー (タブ ビューまたはピボット レイアウト内)。 [設定] はアプリ バーやツール バーのメニューのポップアップで最後の項目であることが必要です。 [設定] をナビゲーション内のトップレベルのいずれかの項目にすることはお勧めしません。
-   ハブ。 [設定] はメニューのポップアップ内に配置する必要があります (ハブ レイアウトでのアプリ バー メニューやツール バー メニューからのポップアップ内など)。

また、マスター詳細ウィンドウ内に [設定] を配置することはお勧めしません。

[設定] ページはアプリのウィンドウ全体を埋め、このページには [バージョン情報] と [フィードバック] があることも必要です。 [設定] ページのデザインに関するガイダンスについては、「[アプリ設定のガイドライン](https://docs.microsoft.com/windows/uwp/app-settings/guidelines-for-app-settings)」をご覧ください。

## <a name="text"></a>Text

テキスト (または文字体裁) は UWP アプリの重要な要素です。移植するときには、ビューの視覚的なデザインが新しいデザイン言語に適合するように、ビューの視覚的なデザインを再検討することが必要になる場合があります。 次の図を使って、ユニバーサル Windows プラットフォーム (UWP) の利用可能な  **TextBlock** システム スタイルを見つけてください。 使用した Windows Phone Silverlight スタイルに対応するものを探します。 または、独自のユニバーサルスタイルを作成し、そのプロパティを Windows Phone Silverlight システムスタイルからそれらにコピーすることもできます。

![Windows 10 アプリのシステム TextBlock スタイル](images/label-uwp10stylegallery.png) <br/>Windows 10 アプリのシステム TextBlock スタイル

Windows ランタイム2.x アプリと Windows Phone ストアアプリでは、既定のフォントファミリはグローバルユーザーインターフェイスです。 Windows 10 アプリでは、既定のフォントファミリは Segoe UI。 この結果、アプリでのフォント メトリックの表示が異なる可能性があります。 8\.1 のテキストの外観を再現する場合は、[**LineHeight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.lineheight) や [**LineStackingStrategy**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.linestackingstrategy) などのプロパティを使って、独自のメトリックを設定できます。

Windows ランタイム2.x アプリと Windows Phone ストアアプリでは、テキストの既定の言語はビルドの言語、または en-us に設定されます。 Windows 10 アプリでは、既定の言語が上位のアプリ言語 (フォントフォールバック) に設定されます。 [  **FrameworkElement.Language**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.language) を明示的に設定できますが、そのプロパティの値を設定しないと、フォントの優れたフォールバック動作を得ることができます。

詳しくは、「[フォントのガイドライン](https://docs.microsoft.com/windows/uwp/controls-and-patterns/fonts)」と「[UWP アプリの設計](https://developer.microsoft.com/)」をご覧ください。 テキスト コントロールの変更については、上記の「[コントロール](#controls-and-control-styles-and-templates)」セクションもご覧ください。

## <a name="theme-changes"></a>テーマの変更

ユニバーサル 8.1 アプリでは、既定のテーマは濃色になっています。 Windows 10 デバイスの場合、既定のテーマが変更されていますが、app.xaml で要求されたテーマを宣言することによって使用されるテーマを制御できます。 たとえば、すべてのデバイスで濃色テーマを使うには、`RequestedTheme="Dark"` をルートの Application 要素に追加します。

## <a name="tiles-and-toasts"></a>タイルとトースト

タイルと toasts の場合、現在使用しているテンプレートは引き続き Windows 10 アプリで動作します。 ただし、新しいアダプティブ テンプレートを使うことができ、それらについては「 [通知、タイル、トースト、バッジ](https://docs.microsoft.com/windows/uwp/controls-and-patterns/tiles-badges-notifications)」で説明されています。

以前、デスクトップ コンピューターでは、トースト通知は一時的なメッセージでした。 表示が消えるため、見逃したり無視した場合はもう取得できなくなりました。 Windows phone では、トースト通知が無視されたり一時的に消された場合、通知はアクション センターに移されます。 アクション センターは、モバイル デバイス ファミリに限定されたものではなくなりました。

トースト通知を送るために機能を宣言する必要はなくなりました。

## <a name="window-size"></a>ウィンドウ サイズ

ユニバーサル 8.1 アプリでは、アプリ マニフェストの要素 [**ApplicationView**](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema2013/element-applicationview) を使って、ウィンドウの最小幅が宣言されます。 UWP アプリでは、命令型コードを使って最小サイズ (幅と高さ) を指定できます。 既定の最小サイズは 500 x 320 epx で、このサイズは受け入れられる最も小さいサイズでもあります。 受け入れられる最も大きいサイズは 500 x 500 epx です。

```csharp
   Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().SetPreferredMinSize
        (new Size { Width = 500, Height = 500 });
```

次のトピックは、「[入出力、デバイス、アプリ モデルの移植](w8x-to-uwp-input-and-sensors.md)」です。

