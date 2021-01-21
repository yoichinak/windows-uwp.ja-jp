---
title: WinUI 2.5 リリース ノート
description: 新機能とバグ修正を含む WinUI 2.5 のリリース ノート。
ms.date: 12/01/2020
ms.topic: article
ms.openlocfilehash: 63cd87caff4425d3a5e29852d84c33e09c68ab84
ms.sourcegitcommit: 617344ae1a1f5b580c938b61e910d99120b73626
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/21/2021
ms.locfileid: "98620845"
---
# <a name="windows-ui-library-25"></a>Windows UI ライブラリ 2.5

WinUI 2.5 は、Windows UI ライブラリ (WinUI) の最新の公式リリースです。

WinUI は、GitHub の [Windows UI ライブラリ リポジトリ](https://aka.ms/winui)にホストされているオープン ソース プロジェクトです。 すべてのバグ レポート、機能要求、およびコミュニティ コードの投稿をこのリポジトリに登録してください。

WinUI リリース:[GitHub リリース ページ](https://github.com/microsoft/microsoft-ui-xaml/releases)

WinUI パッケージは、NuGet パッケージ マネージャーを使用して Visual Studio プロジェクトに追加できます。 詳細については、「[Windows UI ライブラリの概要](../getting-started.md)」を参照してください。

NuGet パッケージのダウンロード:[Microsoft.UI.Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml)

## <a name="new-features"></a>新機能

### <a name="infobar"></a>InfoBar

[InfoBar](/windows/uwp/design/controls-and-patterns/infobar) コントロールは、アプリ全体の状態メッセージを、ユーザーにとって非常に見やすく、それでいて邪魔にならないように表示するために使用されます。 このコントロールには、表示されるメッセージの種類を示す Severity プロパティと、アクションまたはハイパーリンク ボタンの独自の呼び出しを指定するためのオプションがあります。 InfoBar は他の UI コンテンツとインラインで表示されるため、コントロールを常に表示するかどうか、またはユーザーがそれを閉じることができるかどうかを指定することもできます。

この例では、閉じるボタンとメッセージが表示された既定の状態の InfoBar を示します。

:::image type="content" source="../images/infobar-default-title-message.png" alt-text="閉じるボタンとメッセージが表示された、既定の状態の InfoBar の例。":::

このアニメーション化された例では、さまざまな重大度状態とカスタム メッセージが表示された InfoBar を示します。

:::image type="content" source="../images/infobar-severity-animated.gif" alt-text="InfoBar の重大度状態とカスタム メッセージのアニメーション化された例。":::

[使用に関するガイドライン](/windows/uwp/design/controls-and-patterns/infobar)

[API リファレンス](/windows/winui/api/microsoft.ui.xaml.controls.infobar)

### <a name="determinate-progressring"></a>確定的な ProgressRing

[ProgressRing](/windows/uwp/design/controls-and-patterns/progress-controls) の確定状態は、タスクが完了しているパーセンテージを示します。 これは、期間がわかっていて、操作の進行によりユーザーとアプリのやり取りをブロックしてはならない操作の間に使用する必要があります。

次のアニメーション化された画像では、確定的な ProgressRing コントロールを示します。

:::image type="content" source="../images/progressring-determinate-mode-animated.gif" alt-text="確定的な ProgressRing コントロールのアニメーション化された例。":::<br>

[使用に関するガイドライン](/windows/uwp/design/controls-and-patterns/progress-controls#progress-controls-best-practices)

[API リファレンス](/windows/winui/api/microsoft.ui.xaml.controls.progressring)


### <a name="navigationview-footermenuitems"></a>NavigationView の FooterMenuItems

ナビゲーション ペインの最後にナビゲーション項目を配置するには、NavigationView コントロールの FooterMenuItems プロパティを使用します (これに対し、MenuItems プロパティを使用すると、項目はペインの先頭に配置されます)。

次の画像では、フッター メニューに *[アカウント]* 、 *[カート]* 、 *[ヘルプ]* の各ナビゲーション項目が含まれる NavigationView を示します。

:::image type="content" source="../images/navigationview-footermenuitems.png" alt-text="フッター メニューに [アカウント]、[カート]、[ヘルプ] ナビゲーション項目が含まれる NavigationView の例。":::

[使用に関するガイドライン](/windows/uwp/design/controls-and-patterns/navigationview?#footer-menu-items)

[API リファレンス](/windows/winui/api/microsoft.ui.xaml.controls.navigationview.footermenuitems)

## <a name="samples"></a>サンプル

**XAML コントロール ギャラリー** のサンプル アプリには、WinUI のこれらの各機能とコントロールの例が含まれています。

**XAML コントロール ギャラリー** アプリがインストールされ、最新バージョンに更新されている場合:

- [InfoBar](xamlcontrolsgallery:/item/InfoBar) の動作を確認する。
- [ProgressRing](xamlcontrolsgallery:/item/ProgressRing) の動作を確認する。
- [NavigationView](xamlcontrolsgallery:/item/NavigationView) の動作を確認する。

XAML Controls Gallery アプリがインストールされていない場合は、[Microsoft Store](https://aka.ms/xamlgalleryapp) から入手してください。

[GitHub](https://github.com/Microsoft/Xaml-Controls-Gallery) から XAML コントロール ギャラリーのソース コードを表示、クローン、ビルドすることもできます。

## <a name="other-updates"></a>他の更新プログラム

このリリースで対処されている多くの GitHub の問題については、[重要な変更点](https://github.com/microsoft/microsoft-ui-xaml/releases/tag/v2.5.0)の一覧を参照してください。
