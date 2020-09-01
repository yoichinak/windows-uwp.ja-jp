---
ms.assetid: E2B73380-D673-48C6-9026-96976D745017
title: コモン コントロールの概要
description: Common ユニバーサル Windows プラットフォーム (UWP) コントロールとそれに対応する iOS コントロールに関するトピックへのリンクの一覧を表示します。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 50678f0bd5eb3cb48c6bcd915fd88bdfb460118b
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174876"
---
# <a name="getting-started-common-controls"></a>はじめに: コモン コントロール


## <a name="common-controls-list"></a>コモン コントロールの一覧

前のセクションで扱ったコントロールは、ボタンとテキストブロックの 2 つのみでした。 もちろん、他にも多くのコントロールが用意されています。 ここでは、アプリやそれに対応する iOS アプリで使用するいくつかのコモン コントロールを紹介します。 ここでは、iOS コントロールをアルファベット順に並べ、その横に最も似ているユニバーサル Windows プラットフォーム (UWP) コントロールを示しています。

UWP コントロールが優れている点は、実行されているデバイスの種類を検出して、それに応じて外観と機能を変更できることです。 たとえば、プロジェクトが [**DatePicker**](/previous-versions/windows/apps/br211681(v=win.10)) コントロールを使用している場合、たとえば、電話と比較して、デスクトップ コンピューターで異なる外観と動作に自動的に最適化します。 何もする必要はありません。コントロールが実行時に自動的に調整します。

| iOS のコントロール (クラス/プロトコル) | 同等の UWP コントロール |
|------------------------------|--------------------------------------|
| アクティビティ インジケーター (**UIActivityIndicatorView**) | [**ProgressRing**](/uwp/api/Windows.UI.Xaml.Controls.ProgressRing) <br/> 「[クイック スタート: プログレス コントロールの追加](/previous-versions/windows/apps/hh780651(v=win.10))」もご覧ください。 |
| 広告バナー ビュー (**ADBannerView**) と広告バナー ビュー デリゲート (**ADBannerViewDelegate**) | [AdControl](/uwp/api/microsoft.advertising.winrt.ui.adcontrol) <br/> 「[アプリでの広告の表示](../monetize/display-ads-in-your-app.md)」もご覧ください。 |
| ボタン (UIButton) | [Button](/uwp/api/Windows.UI.Xaml.Controls.Button) <br/> 「[クイック スタート: ボタン コントロールの追加](/previous-versions/windows/apps/jj153346(v=win.10))」もご覧ください。 |
| 日付の選択 (UIDatePicker) | [DatePicker](/previous-versions/windows/apps/br211681(v=win.10)) |
| 画像ビュー (UIImageView) | [Image](/uwp/api/Windows.UI.Xaml.Controls.Image) <br/> 「[Image と ImageBrush](../design/controls-and-patterns/images-imagebrushes.md)」もご覧ください。 |
| ラベル (UILabel) | [TextBlock](/uwp/api/Windows.UI.Xaml.Controls.TextBlock) <br/> 「[クイック スタート: テキストの表示](/previous-versions/windows/apps/hh700392(v=win.10))」もご覧ください。 |
| 地図ビュー (MKMapView) と地図ビュー デリゲート (MKMapViewDelegate) | 「 [Bing Maps FOR UWP apps」を](/previous-versions/windows/apps/dn642089(v=win.10))参照してください。 |
| ナビゲーション コント ローラー (UINavigationController) とナビゲーション コント ローラー デリゲート (UINavigationControllerDelegate) | [Frame](/uwp/api/Windows.UI.Xaml.Controls.Frame) <br/> 「[ナビゲーション](../design/basics/navigation-basics.md)」もご覧ください。 |
| ページ コントロール (UIPageControl) | [ページ](/uwp/api/Windows.UI.Xaml.Controls.Page) <br/> 「[ナビゲーション](../design/basics/navigation-basics.md)」もご覧ください。 |
| ピッカー ビュー (UIPickerView) とピッカー ビュー デリゲート (UIPickerViewDelegate) | [ComboBox](/uwp/api/Windows.UI.Xaml.Controls.ComboBox) <br/> 「[コンボ ボックスとリスト ボックスの追加](/previous-versions/windows/apps/hh780616(v=win.10))」もご覧ください。 |
| 進行状況バー (UIProgressView) | [ProgressBar](/uwp/api/Windows.UI.Xaml.Controls.ProgressBar) <br/> 「[クイック スタート: プログレス コントロールの追加](/previous-versions/windows/apps/hh780651(v=win.10))」もご覧ください。 |
| スクロール ビュー (UIScrollView) とスクロール ビュー デリゲート (UIScrollViewDelegate) | [ScrollViewer](/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer) <br/>  「[Extensible Application Markup Language (XAML) のスクロール、パン、ズームのサンプル](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/XAML%20scrolling%2C%20panning%2C%20and%20zooming%20sample%20(Windows%208))」もご覧ください。 |
| 検索バー (UISearchBar) と検索バー デリゲート (UISearchBarDelegate) | 「[アプリへの検索の追加](/previous-versions/windows/apps/jj130767(v=win.10))」をご覧ください。 <br/>  「[クイック スタート: アプリへの検索の追加](/previous-versions/windows/apps/hh868180(v=win.10))」もご覧ください。 |
| セグメント化されたコントロール (UISegmentedControl) | なし |
| スライダー (UISlider) | [Slider](/uwp/api/Windows.UI.Xaml.Controls.Slider) <br/>  「[スライダーを追加する方法](/previous-versions/windows/apps/hh868197(v=win.10))」もご覧ください。 |
| 分割ビュー コントローラー (UISplitViewController) と分割ビュー コントローラー デリゲート (UISplitViewControllerDelegate) | なし |
| スイッチ (UISwitch) | [ToggleSwitch](/uwp/api/Windows.UI.Xaml.Controls.ToggleSwitch) <br/>  「[トグル スイッチを追加する方法](/previous-versions/windows/apps/hh868198(v=win.10))」もご覧ください。 |
| タブ バー コントローラー (UITabBarController) とタブ バー コントローラー デリゲート (UITabBarControllerDelegate) | なし |
| テーブル ビュー コントローラー (UITableViewController)、テーブル ビュー (UITableView)、テーブル ビュー デリゲート (UITableViewDelegate)、および表のセル (UITableViewCell) | [ListView](/uwp/api/Windows.UI.Xaml.Controls.ListView) <br/>  「[クイック スタート: ListView コントロールと GridView コントロールの追加](/previous-versions/windows/apps/hh780650(v=win.10))」もご覧ください。 |
| テキスト フィールド (UITextField) とテキスト フィールド デリゲート (UITextFieldDelegate) | [TextBox](/uwp/api/Windows.UI.Xaml.Controls.TextBox) <br/>  「[テキストの表示と編集](../design/controls-and-patterns/text-controls.md)」もご覧ください。 |
| テキスト ビュー (UITextView) とテキスト ビュー デリゲート (UITextViewDelegate) | [TextBlock](/uwp/api/Windows.UI.Xaml.Controls.TextBlock) <br/>  「[クイック スタート: テキストの表示](/previous-versions/windows/apps/hh700392(v=win.10))」もご覧ください。 |
| ビュー (UIView) とビュー コントローラー (UIViewController) | [ページ](/uwp/api/Windows.UI.Xaml.Controls.Page) <br/>  「[ナビゲーション](../design/basics/navigation-basics.md)」もご覧ください。 |
| Web ビュー (UIWebView) と Web ビュー デリゲート (UIWebViewDelegate) | [WebView](/uwp/api/Windows.UI.Xaml.Controls.WebView) <br/>  「[XAML WebView コントロールのサンプル](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/XAML%20WebView%20control%20sample%20(Windows%208))」もご覧ください。 |
| ウィンドウ (UIWindow) | [Frame](/uwp/api/Windows.UI.Xaml.Controls.Frame) <br/>  「[ナビゲーション](../design/basics/navigation-basics.md)」もご覧ください。 |

その他のコントロールについては、「[コントロールの一覧](../design/controls-and-patterns/index.md)」をご覧ください。

**メモ**   JavaScript と HTML を使用した UWP アプリのコントロールの一覧については、「[コントロールリスト](/previous-versions/windows/apps/hh465453(v=win.10))」を参照してください。

### <a name="next-step"></a>次のステップ

[はじめに: ナビゲーション](getting-started-navigation.md)

## <a name="related-topics"></a>関連トピック

* [Build 2014: XAML UI とコントロールの説明](https://channel9.msdn.com/Events/Build/2014/2-516)
* [Build 2014: 共通の XAML UI フレームワークを使ったアプリの開発](https://channel9.msdn.com/Events/Build/2014/2-507)
* [Build 2014: Visual Studio を使った XAML 集約型アプリの構築に関するページ](https://channel9.msdn.com/Events/Build/2014/3-591)