---
title: WinUI 2.4 リリース ノート
description: 新機能とバグ修正を含む WinUI 2.4 のリリース ノート。
ms.date: 07/15/2020
ms.topic: reference
ms.openlocfilehash: 22fd028ba2059a092ee2f2be47a114fb2d618ce1
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/20/2020
ms.locfileid: "86492827"
---
# <a name="windows-ui-library-24"></a>Windows UI ライブラリ 2.4

WinUI 2.4 は、Windows UI ライブラリ (WinUI) の最新の公式リリースです。

WinUI は、GitHub の [Windows UI ライブラリ リポジトリ](https://aka.ms/winui)にホストされているオープン ソース プロジェクトです。 すべてのバグ レポート、機能要求、およびコミュニティ コードの投稿をこのリポジトリに登録してください。

WinUI リリース:[GitHub リリース ページ](https://github.com/microsoft/microsoft-ui-xaml/releases)

WinUI パッケージは、NuGet パッケージ マネージャーを使用して Visual Studio プロジェクトに追加できます。 詳細については、「[Windows UI ライブラリの概要](../getting-started.md)」を参照してください。

NuGet パッケージのダウンロード:[Microsoft.UI.Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml)

## <a name="new-features"></a>新機能

### <a name="radialgradientbrush"></a>RadialGradientBrush

RadialGradientBrush は、Center、RadiusX、RadiusY プロパティによって定義される楕円内に描画されます。 グラデーションの色は楕円の中心から始まり、半径の位置で終了します。

![放射状グラデーション ブラシ](../images/radialgradientbrush.gif)<br>
*放射状グラデーション ブラシ*

[使用に関するガイドライン](/windows/uwp/design/style/brushes#radial-gradient-brushes)

[API リファレンス](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush)

### <a name="progressring"></a>ProgressRing

ProgressRing コントロールはモーダル操作向けに使われ、ProgressRing が消えるまでユーザーはブロックされます。 ある操作で、その操作が完了するまで、アプリとのほとんどのやり取りを中断する必要がある場合は、このコントロールを使用します。

![ProgressRing コントロール](../images/progressring.gif)<br>
*ProgressRing コントロール*

[使用に関するガイドライン](/windows/uwp/design/controls-and-patterns/progress-controls)

[API リファレンス](/uwp/api/microsoft.ui.xaml.controls.progressring)

### <a name="tabview-updates"></a>TabView の更新

TabView コントロールの更新により、タブの表示方法をより詳細に制御できるようになります。

選択されていないタブの幅を設定し、アイコンのみを表示して画面領域を節約できます。

![TabView コントロール タブのサイズ](..\images\tabview-sizing.gif)<br>
*TabView コントロール タブのサイズ*

また、選択されていないタブの [閉じる] ボタンを、ユーザーがタブにカーソルを合わせるまで非表示にすることもできます (以前のバージョンでは常に表示されていました)。

![TabView コントロールでホバーして閉じる](..\images\tabview-closebuttononhover.gif)<br>
*TabView コントロールでホバーして閉じる*

[使用に関するガイドライン](/windows/uwp/design/controls-and-patterns/tab-view)

[API リファレンス](/uwp/api/microsoft.ui.xaml.controls.tabview)

### <a name="dark-theme-updates-to-textbox-family-of-controls"></a>TextBox コントロール ファミリに対するダーク テーマの更新

ダーク テーマが有効になっている場合、テキストの挿入時に TextBox ファミリ コントロールの背景色が既定でダークのままになります (以前のバージョンでは、テキストの挿入時に背景色が白に変わります)。

| 変更前 | クリック後 |
| - | - |
| ![TextBox のダーク テーマの更新 (変更前)](..\images\textbox-darkthemeupdates-before1.gif)<br>*TextBox のダーク テーマの更新 (変更前)* | ![TextBox のダーク テーマの更新 (変更後)](..\images\textbox-darkthemeupdates-after1.gif)<br>*TextBox のダーク テーマの更新 (変更後)* |
| ![TextBox のダーク テーマの更新 (変更前)](..\images\textbox-darkthemeupdates-before2.gif)<br>*TextBox のダーク テーマの更新 (変更前)* | ![TextBox のダーク テーマの更新 (変更後)](..\images\textbox-darkthemeupdates-after2.gif)<br>*TextBox のダーク テーマの更新 (変更後)* |

TextBox コントロール ファミリに含まれるコントロールの一部を次に示します。

- [TextBox](/uwp/api/windows.ui.xaml.controls.textbox)
- [RichEditBox](/uwp/api/windows.ui.xaml.controls.richtextblock)
- [PasswordBox](/uwp/api/windows.ui.xaml.controls.passwordbox)
- [編集可能な ComboBox](/uwp/api/windows.ui.xaml.controls.combobox)
- [AutoSuggestBox](/uwp/api/windows.ui.xaml.controls.autosuggestbox)

### <a name="hierarchical-navigation"></a>階層型ナビゲーション

[NavigationView](/uwp/api/microsoft.ui.xaml.controls.navigationview?view=winui-2.4) コントロールは、階層型ナビゲーションをサポートするようになりました。Left、Top、LeftCompact の表示モードが含まれています。 階層構造の NavigationView は、ページのカテゴリの表示、関連する子ページを含むページの識別、またはハブ スタイルのページが他の多くのページにリンクしているアプリ内での使用に役立ちます。

![階層構造の NavigationView コントロール](..\images\HierarchicalNavView.gif)<br>*階層構造の NavigationView コントロール*

[使用に関するガイドライン](/windows/uwp/design/controls-and-patterns/navigationview#hierarchical-navigation)

[API リファレンス](/uwp/api/microsoft.ui.xaml.controls.navigationview)

## <a name="samples"></a>サンプル

説明されている各 WinUI 2.4 機能の例については、**XAML Controls Gallery** を参照してください。

XAML Controls Gallery アプリがインストールされていない場合は、[Microsoft Store](https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt) から入手してください。

[GitHub](https://github.com/Microsoft/Xaml-Controls-Gallery)で XAML Controls Gallery のソース コードを表示することもできます。
