---
title: WinUI 2.3 リリース ノート
description: 新機能とバグ修正を含む WinUI 2.3 のリリース ノート。
ms.date: 07/15/2020
ms.topic: article
ms.openlocfilehash: e3f6aca344fb86725e6addc7758110aca9eaf01b
ms.sourcegitcommit: b99fe39126fbb457c3690312641f57d22ba7c8b6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/04/2020
ms.locfileid: "96603849"
---
# <a name="windows-ui-library-23"></a>Windows UI ライブラリ 2.3

WinUI 2.3 は、Windows UI ライブラリ (WinUI) の 2020 年 1 月のリリースです。

WinUI は、GitHub の [Windows UI ライブラリ リポジトリ](https://aka.ms/winui)にホストされているオープン ソース プロジェクトです。 すべてのバグ レポート、機能要求、およびコミュニティ コードの投稿をこのリポジトリに登録してください。

WinUI リリース:[GitHub リリース ページ](https://github.com/microsoft/microsoft-ui-xaml/releases)

WinUI パッケージは、NuGet パッケージ マネージャーを使用して Visual Studio プロジェクトに追加できます。 詳細については、「[Windows UI ライブラリの概要](../getting-started.md)」を参照してください。

NuGet パッケージのダウンロード:[Microsoft.UI.Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml)

## <a name="new-features"></a>新機能

### <a name="progress-bar-visual-refresh"></a>進行状況バーのビジュアル更新

**ProgressBar** には、2 つの異なるビジュアル表現があります。

#### <a name="indeterminate-progress-bar"></a>進行状況不定バー

タスクが進行中であるが、ユーザーの対話式操作をブロックしないことを示します。

![進行状況不定バー](../images/IndeterminateProgressBar.gif)

#### <a name="determinate-progress-bar"></a>確定的な進行状況バー

既知の作業量に対してどの程度進行しているかを示します。 

![確定的な進行状況バー](../images/DeterminateProgressBar.gif)

[ドキュメント リンク](/windows/uwp/design/controls-and-patterns/progress-controls)

[サンプル リンク](/windows/uwp/design/controls-and-patterns/progress-controls#examples)

### <a name="numberbox"></a>NumberBox

**NumberBox** は、数値の表示と編集に使用できるコントロールを表します。 検証、増分ステップ、乗算、除算、加算、減算などの基本的な式のインライン計算をサポートしています。

![NumberBox](../images/NumberBoxGif.gif)

[ドキュメントとサンプル リンク](/windows/uwp/design/controls-and-patterns/number-box)

### <a name="radiobuttons"></a>RadioButtons

**Radiobutton** は、RadioButton 要素の関連グループを簡単に作成できる新しいコンテナー コントロールです。また、キーボード入力機能やナレーター/スクリーン リーダー機能を適切にサポートします

![3 つのラジオ ボタンのスクリーンショット (3 つ目が選択されている)。](../images/RadioButtons.png)

[ドキュメントとサンプル リンク](https://github.com/microsoft/microsoft-ui-xaml-specs/blob/c8d3d3668af546091656dfc37436b13cd062f52d/active/radiobuttons/RadioButtons_Spec.md)

## <a name="examples"></a>例

XAML Controls Gallery サンプル アプリには、WinUI コントロールを使用するための対話型のデモとサンプル コードが含まれています。

* [Microsoft Store](
https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt) から XAML Controls Gallery アプリをインストールします

* XAML Controls Gallery は [GitHub のオープン ソース](https://github.com/Microsoft/Xaml-Controls-Gallery)でもあります

## <a name="documentation"></a>ドキュメント

Windows UI ライブラリ コントロールの操作方法に関する記事は、[ユニバーサル Windows プラットフォーム コントロール ドキュメント](/windows/uwp/design/controls-and-patterns/)に含まれています。

API リファレンスのドキュメントがある場所は、[Windows UI ライブラリ API](/windows/winui/api/) です。