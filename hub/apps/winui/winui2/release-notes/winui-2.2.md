---
title: WinUI 2.2 リリース ノート
description: 新機能とバグ修正を含む WinUI 2.2 のリリース ノート。
ms.date: 07/15/2020
ms.topic: article
ms.openlocfilehash: 4c200701e0d845d6c9b9f8797899d88cc72d8c1c
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89154906"
---
# <a name="windows-ui-library-22"></a>Windows UI ライブラリ 2.2

WinUI 2.2 は、Windows UI ライブラリの最新の公式リリースです。

NuGet パッケージ マネージャーを使用して WinUI パッケージをアプリに追加できます。詳細については、「[Windows UI ライブラリの使用を開始する](../getting-started.md)」を参照してください。

WinUI は、GitHub でホストされているオープン ソース プロジェクトです。 [Windows UI ライブラリ リポジトリ](https://aka.ms/winui)のバグ レポート、機能要求、およびコミュニティ コードの投稿を歓迎します。

## <a name="microsoftuixaml-22-version-history"></a>Microsoft.UI.Xaml 2.2 のバージョン履歴

### <a name="windows-ui-library-22-official-release"></a>Windows UI Library 2.2 公式リリース

2019 年 8 月

[GitHub リリース ページ](https://github.com/microsoft/microsoft-ui-xaml/releases)

[NuGet パッケージのダウンロード](https://www.nuget.org/packages/Microsoft.UI.Xaml)

### <a name="new-features"></a>新機能

#### <a name="tabview"></a>TabView

![例](../images/tabview-gif.gif)

#### <a name="description"></a>説明

TabView コントロールは、それぞれがアプリ内の新しいページまたはドキュメントを表すタブのコレクションです。 TabView は、アプリに複数のコンテンツ ページがあり、ユーザーがタブを追加、閉じ、再配置できることを期待している場合に便利です。 新しい [Windows ターミナル](https://github.com/Microsoft/Terminal)では、複数のコマンド ライン インターフェイスを表示するために TabView が使用されます。

#### <a name="documentation"></a>ドキュメント

https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.tabview?view=winui-2.2

#### <a name="navigationview-updates"></a>NavigationView の更新

##### <a name="a-navigationviews-back-button-update"></a>a) NavigationView の戻るボタンの更新

![例](../images/navigationview-back-button.gif)

##### <a name="description"></a>説明

NavigationView の最小モードでは、戻るボタンが消えなくなりました。 ウィンドウを開いたり閉じたりするときに、ユーザーはハンバーガー ボタンをクリックするためにカーソルを移動する必要がなくなります。 この機能は、既定で動作します。 これを機能させるためにコードを変更する必要はありません。

##### <a name="b-navigationview---no-auto-padding"></a>b) NavigationView - 自動埋め込みなし

![例](../images/navigationview-no-auto-padding.png)

##### <a name="description"></a>説明

アプリ開発者は、NavigationView コントロールを使用してタイトル バー領域に拡張するときに、アプリ ウィンドウ内のすべてのピクセルを再利用できるようになりました。

##### <a name="documentation"></a>ドキュメント

https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/navigationview#top-whitespace

#### <a name="visual-style-updates"></a>視覚スタイルの更新

##### <a name="a-corner-radius-update"></a>a) 角の半径の更新

![例](../images/corner-radius.png)

##### <a name="description"></a>説明

CornerRadius 属性が追加されました。 既定のコントロールは、若干丸い角を使用するように更新されました。 開発者は、角の半径を簡単にカスタマイズして、必要に応じてアプリに固有の外観を与えることができます。

##### <a name="github-spec-link"></a>GitHub 仕様リンク

https://github.com/microsoft/microsoft-ui-xaml/issues/524

##### <a name="b-border-thickness-update"></a>b) 外枠の太さの更新

![例](../images/border-thickness.png)

##### <a name="description"></a>説明

BorderThickness プロパティのカスタマイズが容易になりました。 既定のコントロールが更新され、より明確で見慣れた外観となるようにアウトラインが小さくなりました。

##### <a name="github-spec-link"></a>GitHub 仕様リンク

https://github.com/microsoft/microsoft-ui-xaml/issues/835

##### <a name="c-button-visual-update"></a>c) ボタンのビジュアルの更新

![例](../images/button-hover-visual-update.png)

##### <a name="description"></a>説明: 
既定のボタンのビジュアルが更新されて、マウス ポインターを置いたときに表示されるアウトラインが削除され、より明確な外観になりました。

##### <a name="github-spec-link"></a>GitHub 仕様リンク:  
https://github.com/microsoft/microsoft-ui-xaml/issues/953

##### <a name="d-splitbutton-visual-update"></a>c) SplitButton のビジュアルの更新

![例](../images/splitbutton-visual-update.png)

##### <a name="description"></a>説明: 
既定の SplitButton のビジュアルが更新され、DropDownButton と区別しやすくなりました。

##### <a name="github-spec-link"></a>GitHub 仕様リンク: 
https://github.com/microsoft/microsoft-ui-xaml/issues/986

##### <a name="e-toggleswitch-visual-update"></a>e) ToggleSwitch のビジュアル更新

![例](../images/toggleswitch-update.png)

##### <a name="description"></a>説明: 
既定の ToggleSwitch の幅が 44 px から 40 px に縮小され、使いやすさを維持しながら視覚的にバランスがとられています。

##### <a name="github-spec-link"></a>GitHub 仕様リンク: 
https://github.com/microsoft/microsoft-ui-xaml/issues/836

##### <a name="f-checkbox-and-radiobutton-visual-update"></a>f) CheckBox および RadioButton のビジュアルの更新

![例](../images/checkbox-radiobutton.png)

##### <a name="description"></a>説明: 
CheckBox および RadioButton のビジュアルが更新され、他の視覚的なスタイル変更と一致するようになりました。

##### <a name="github-spec-link"></a>GitHub 仕様リンク: 
https://github.com/microsoft/microsoft-ui-xaml/issues/839

## <a name="examples"></a>例

XAML Controls Gallery サンプル アプリには、WinUI コントロールを使用するための対話型のデモとサンプル コードが含まれています。

* [Microsoft Store](
https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt) から XAML Controls Gallery アプリをインストールします

* XAML Controls Gallery は [GitHub のオープン ソース](
https://github.com/Microsoft/Xaml-Controls-Gallery)でもあります

## <a name="documentation"></a>ドキュメント

Windows UI ライブラリ コントロールの操作方法に関する記事は、[ユニバーサル Windows プラットフォーム コントロール ドキュメント](/windows/uwp/design/controls-and-patterns/)に含まれています。

API リファレンスのドキュメントがある場所は、[Windows UI ライブラリ API](/uwp/api/overview/winui/) です。

## <a name="microsoftuixaml-22-version-history"></a>Microsoft.UI.Xaml 2.2 のバージョン履歴

### <a name="microsoftuixaml-22190702001-prerelease"></a>Microsoft.UI.Xaml 2.2.190702001-prerelease

2019 年 7 月

[GitHub リリース ページ](https://github.com/microsoft/microsoft-ui-xaml/releases/tag/v2.2.190702001-prerelease)

[NuGet パッケージのダウンロード](https://www.nuget.org/packages/Microsoft.UI.Xaml/2.2.190702001-prerelease)

### <a name="experimental-feature"></a>試験的な機能

* [TabView](/uwp/api/microsoft.ui.xaml.controls.tabview?view=winui-2.2)

### <a name="microsoftuixaml-2220190416001-prerelease"></a>Microsoft.UI.Xaml 2.2.20190416001-prerelease

2019 年 4 月

[GitHub リリース ページ](https://github.com/Microsoft/microsoft-ui-xaml/releases/tag/v2.2.190416008-prerelease)

[NuGet パッケージのダウンロード](https://www.nuget.org/packages/Microsoft.UI.Xaml/2.2.190416008-prerelease)

#### <a name="experimental-features"></a>実験的な機能

* [FlowLayout](/uwp/api/microsoft.ui.xaml.controls.flowlayout)

* [LayoutPanel](/uwp/api/microsoft.ui.xaml.controls.layoutpanel)

* [RadioButtons](/uwp/api/microsoft.ui.xaml.controls.radiobuttons)

* [ScrollViewer](/uwp/api/microsoft.ui.xaml.controls.scrollviewer)