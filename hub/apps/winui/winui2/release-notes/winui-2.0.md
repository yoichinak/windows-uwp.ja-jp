---
title: WinUI 2.0 リリース ノート
description: WinUI 2.0 のリリース ノート。
ms.date: 04/15/2020
ms.topic: article
ms.openlocfilehash: 300a8eaa0077b3a59ef2ac01213cd82b0c76c343
ms.sourcegitcommit: d0f479f1955881afb62c2af249db5d0b053b63e5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/19/2020
ms.locfileid: "83580659"
---
# <a name="windows-ui-library-20"></a>Windows UI ライブラリ 2.0

WinUI 2.0 は、Windows UI ライブラリの最初のパブリック リリースです。

WinUI は、Windows 向けの優れた Fluent Design エクスペリエンスを構築するための最も簡単な方法です。

次の 2 つの NuGet パッケージが含まれています。

* [Microsoft.UI.Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml):UWP アプリのコントロールと Fluent Design。 これは、メインの WinUI パッケージです。

* [Microsoft.UI.Xaml.Core.Direct](https://www.nuget.org/packages/Microsoft.UI.Xaml.Core.Direct):ミドルウェア コンポーネントで使用するための下位 API。

WinUI パッケージをダウンロードして、NuGet パッケージ マネージャーを使用してアプリ内で使用できます。詳細については、「[Windows UI ライブラリの使用を開始する](https://docs.microsoft.com/uwp/toolkits/winui/getting-started)」を参照してください。

WinUI は、GitHub でホストされているオープン ソース プロジェクトです。 [Windows UI ライブラリ リポジトリ](https://aka.ms/winui)のバグ レポート、機能要求、およびコミュニティ コードの投稿を歓迎します。

## <a name="microsoftuixaml-20181011001"></a>Microsoft.UI.Xaml 2.0.181011001

2018 年 10 月

これは、[Microsoft.UI.Xaml NuGet パッケージ](https://www.nuget.org/packages/Microsoft.UI.Xaml)の最初のリリースです。 Windows UWP アプリ用の正式なネイティブ Fluent コントロールと機能が含まれています。

### <a name="new-features"></a>新機能

このリリースのコントロールとパターンは次のとおりです。

| 機能 | 説明 |
| --- | --- |
|[AcrylicBrush]( https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.media.acrylicbrush)| ぼかしとノイズ テクスチャを含む複数の効果を使用する半透明の素材を使用して領域を塗りつぶします。|
|[BitmapIconSource]( https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.bitmapiconsource)| ビットマップをコンテンツとして使用するアイコン ソースを表します。|
|[ColorPicker]( https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.colorpicker)| ユーザーがカラー スペクトル、スライダー、テキスト入力を使用して色を選択できるようにするコントロールを表します。|
|[CommandBarFlyout](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.commandbarflyout)|AppBarButton および関連するコマンド要素のレイアウトを提供する特殊なポップアップを表します。|
|[DropDownButton](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.dropdownbutton)|メニューを開くことを目的としたシェブロンがあるボタンを表します。|
|[FontIconSource ](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.fonticonsource)|指定されたフォントのグリフを使用するアイコン ソースを表します。|
|[MenuBar](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.menubar)|横一列 (通常はアプリ ウィンドウの上部) に複数のメニューを表示する特殊なコンテナーを表します。|
|[MenuBarItem](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.menubaritem)|MenuBar コントロールのトップレベル メニューを表します。|
|[NavigationView](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.navigationview)|アプリ コンテンツのナビゲーションを可能にするコンテナーを表します。 ヘッダー、メイン コンテンツのビュー、およびナビゲーション コマンド用のメニュー ペインがあります。|
|[ParallaxView](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.parallaxview)|前景要素 (リストなど) のスクロール位置を背景要素 (画像など) に関連付けるコンテナーを表します。 前景要素をスクロールすると、背景要素がアニメーション化され、視差効果が発生します。|
|[PersonPicture](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.personpicture)|ユーザーのアバター画像を利用できる場合はその画像を表示し、利用できない場合はユーザーの頭文字か汎用アイコンを表示するコントロールを表します。|
|[RatingControl](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.ratingcontrol)|ユーザーが星評価を入力できるようにするコントロールを表します。|
|[RefreshContainer](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.refreshcontainer)|スクロール可能なコンテンツの RefreshVisualizer および引っ張って更新機能を提供するコンテナー コントロールを表します。|
|[RefreshVisualizer](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.refreshvisualizer)|コンテンツ更新のアニメーション状態インジケーターを提供するコントロールを表します。|
|[RevealBackgroundBrush](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.media.revealbackgroundbrush)|コンポジション ブラシと光の効果を使用して、表示効果を持つコントロールの背景を塗りつぶします。|
|[RevealBorderBrush](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.media.revealborderbrush)|コンポジション ブラシと光の効果を使用して、表示効果を持つコントロールの境界線を塗りつぶします。|
|[RevealBrush](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.media.revealbrush)|表示のビジュアル デザインの処理を実装するためにコンポジション効果と光源を使用するブラシの基本クラス。|
|[SplitButton](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.splitbutton)|個別に呼び出せる 2 つのパーツを持つボタンを表します。 一方のパーツは標準のボタンのように動作し、もう一方のパーツはポップアップを呼び出します。|
|[SwipeControl](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.swipecontrol)|タッチ操作を通じてコンテキスト コマンドへのアクセスを提供するコンテナーを表します。|
|[SymbolIconSource](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.symboliconsource)|Segoe MDL2 アセット フォントのグリフをコンテンツとして使用するアイコン ソースを表します。|
|[TextCommandBarFlyout](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.textcommandbarflyout)|テキストを編集するためのコマンドを含む特殊なコマンド バーのポップアップを表します。|
|[ToggleSplitButton](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.togglesplitbutton)|個別に呼び出せる 2 つのパーツを持つボタンを表します。 一方のパーツはトグル ボタンのように動作し、もう一方のパーツはポップアップを呼び出します。|
|[TreeView](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.treeview)|入れ子になった項目を含むノードを展開したり、折りたたんだりできる階層リストを表します。|

## <a name="examples"></a>例

XAML Controls Gallery サンプル アプリには、WinUI コントロールを使用するための対話型のデモとサンプル コードが含まれています。

* [Microsoft Store](
https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt) から XAML Controls Gallery アプリをインストールします

* XAML Controls Gallery は [GitHub のオープン ソース](
https://github.com/Microsoft/Xaml-Controls-Gallery)でもあります

## <a name="documentation"></a>ドキュメント

Windows UI ライブラリ コントロールの操作方法に関する記事は、[ユニバーサル Windows プラットフォーム コントロール ドキュメント](/windows/uwp/design/controls-and-patterns/)に含まれています。

API リファレンスのドキュメントがある場所は、[Windows UI ライブラリ API](/uwp/api/overview/winui/) です。
