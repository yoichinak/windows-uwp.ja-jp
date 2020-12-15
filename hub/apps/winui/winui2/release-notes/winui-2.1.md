---
title: WinUI 2.1 リリース ノート
description: 新機能とバグ修正を含む WinUI 2.1 のリリース ノート。
ms.date: 07/15/2020
ms.topic: article
ms.openlocfilehash: eb9d01c49dffd8017b2a70557f9408ceb758ae1e
ms.sourcegitcommit: b99fe39126fbb457c3690312641f57d22ba7c8b6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/04/2020
ms.locfileid: "96603859"
---
# <a name="windows-ui-library-21"></a>Windows UI ライブラリ 2.1

Windows UI ライブラリの最初のオープンソース バージョン – WinUI 2.1 (2019 年 4 月リリース)。

WinUI では、最新の Fluent コントロールやスタイルなど、最新の Windows UX プラットフォーム機能の多くがすぐに使用できる方法で提供され、Windows 10 Anniversary Update (14393) と互換性があります。 [XAML コントロール ギャラリー](/windows/uwp/design/controls-and-patterns/#xaml-controls-gallery)には、ライブラリに追加されたすべてのクールな新機能を調べるためのサンプルが用意されています。

[WinUI 2.1 NuGet パッケージ](https://www.nuget.org/packages/Microsoft.UI.Xaml/2.1.190405004)をダウンロードします

NuGet パッケージ マネージャーを使用してアプリ内で WinUI パッケージを使用することを選択できます。詳細については、「[Windows UI ライブラリの使用を開始する](/uwp/toolkits/winui/getting-started)」を参照してください。

WinUI は、GitHub でホストされているオープン ソース プロジェクトです。 [Windows UI ライブラリ リポジトリ](https://aka.ms/winui)のバグ レポート、機能要求、およびコミュニティ コードの投稿を歓迎します。

## <a name="whats-new-in-this-release"></a>このリリースの新機能

### <a name="itemsrepeater"></a>ItemsRepeater

柔軟なレイアウト システム、カスタム ビュー、および仮想化を使ってカスタム コレクション エクスペリエンスを作成するには、ItemsRepeater を使用します。
ListView とは異なり、ItemsRepeater では包括的なエンドユーザー エクスペリエンスが提供されません。既定の UI はなく、フォーカス、選択、ユーザーの操作に関するポリシーは提供されません。 その代わり、構成要素を使用することで、独自の一意のコレクション ベースのエクスペリエンスおよびカスタム コントロールを作成することができます。 充実し、パフォーマンスに優れたエクスペリエンスの構築をサポートしています。

![Items Repeater コントロールの動作を示す短い動画。](../images/ItemsRepeater%20-%20MSN%20News.gif)

[ドキュメント](/windows/uwp/design/controls-and-patterns/items-repeater)

### <a name="animatedvisualplayer"></a>AnimatedVisualPlayer

AnimatedVisualPlayer では、アニメーション化されたビジュアルがホストされ、その再生が制御されます。これにより、アプリにハイ パフォーマンスのカスタム モーション グラフィックスを追加できます。 たとえば、AnimatedVisualPlayer を使用して Lottie アニメーションを表示および制御します。

![Animated Visual Player コントロールの動作を示す短い動画。](../images/AnimatedVisualPlayerUpdated.gif)

[ドキュメント](/windows/communitytoolkit/animations/lottie)

### <a name="teachingtip"></a>TeachingTip

TeachingTip は、アプリケーションが非侵入型でコンテンツ豊富なヒントを使用してユーザーにガイドと情報を提供できるようにする魅力的な Fluent 方式を提供します。 TeachingTip は、新しいまたは重要な機能にフォーカスを移し、タスクを実行する方法をユーザーに示すことができます。また、目前のタスクに対して文脈上関連する情報を提供することで、ワークフローを強化することができます。

![Teaching Tip コントロールの動作を示す短い動画。](../images/TeachingTipUpdated.gif)

[ドキュメント](/windows/uwp/design/controls-and-patterns/dialogs-and-flyouts/teaching-tip)

### <a name="radiomenuflyoutitem"></a>RadioMenuFlyoutItem

MenuBar に 'ラジオ ボタン' スタイル オプションを設定する機能が含まれています。 これにより、ラジオ ボタン グループのように関連付けられた箇条書きを含むオプションのグループが有効になります。 ロジックは開発者に対して処理されます。

![Radio Menu Fly out Item コントロールの動作を示すスクリーンショット。](../images/RadioMenuFlyoutItem1.png)

[ドキュメント](/windows/uwp/design/controls-and-patterns/menus#create-a-menu-flyout-or-a-context-menu)

### <a name="compactdensity"></a>CompactDensity

コンパクト モードでは、開発者は任意の数のシナリオ向けの快適なエクスペリエンスを作成できます。 リソース ディクショナリを追加するだけで、アプリケーションは平均で最大 33% 多い UI に収まります。

![Compact Density コントロールの動作を示すスクリーンショット。](../images/CompactDensityUpdated.png)

[ドキュメント](/windows/uwp/design/style/spacing)

### <a name="shadows"></a>シャドウ

![例](../images/shadow.gif)

UI 内で要素のビジュアル階層を作成すると、注目すべき重要なことを UI で簡単にスキャンし、伝達できます。 UI の選択要素を前面に移動する操作である昇格は、多くの場合、ソフトウェア内でこのような階層を実現するために使用されます。 

Windows 10 May 2019 Update では、一般的なコントロールの多くによって、z 深度とシャドウを使用して昇格が既定で追加されます。 Windows 10 May 2019 Update がインストールされている OS 上で実行する場合、WinUI 2.1 の NavigationView および TeachingTip コントロールにも既定のシャドウがあります。 Windows 10 May 2019 Update がリリースされ、リンクがここに投稿されると、既定のシャドウを持つコントロールの完全な一覧と追加の API の使用方法を確認できます。

## <a name="examples"></a>例

XAML Controls Gallery サンプル アプリには、WinUI コントロールを使用するための対話型のデモとサンプル コードが含まれています。

* [Microsoft Store](
https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt) から XAML Controls Gallery アプリをインストールします

* XAML Controls Gallery は [GitHub のオープン ソース](
https://github.com/Microsoft/Xaml-Controls-Gallery)でもあります

## <a name="documentation"></a>ドキュメント

Windows UI ライブラリ コントロールの操作方法に関する記事は、[ユニバーサル Windows プラットフォーム コントロール ドキュメント](/windows/uwp/design/controls-and-patterns/)に含まれています。

API リファレンスのドキュメントがある場所は、[Windows UI ライブラリ API](/windows/winui/api/) です。

## <a name="microsoftuixaml-21-version-history"></a>Microsoft.UI.Xaml 2.1 のバージョン履歴

### <a name="microsoftuixaml-21-official-release"></a>Microsoft. UI. Xaml 2.1 の公式リリース

2019 年 4 月

[GitHub リリース ページ](https://github.com/Microsoft/microsoft-ui-xaml/releases)

[NuGet パッケージのダウンロード](https://www.nuget.org/packages/Microsoft.UI.Xaml/2.1.190405004)

#### <a name="new-feature-not-included-in-earlier-pre-releases"></a>新機能 (以前のプレリリース版には含まれていません)

* [CompactDensity](/windows/uwp/design/style/spacing):コンパクト モードでは、開発者は任意の数のシナリオ向けの快適なエクスペリエンスを作成できます。 リソース ディクショナリを追加するだけで、アプリケーションは平均で最大 33% 多い UI に収まります。

* シャドウ:UI 内で要素のビジュアル階層を作成すると、注目すべき重要なことを UI で簡単にスキャンし、伝達できます。 UI の選択要素を前面に移動する操作である昇格は、多くの場合、ソフトウェア内でこのような階層を実現するために使用されます。 一般的なコントロールの多くによって、z 深度とシャドウを使用して昇格が既定で追加されます。  

### <a name="microsoftuixaml-21190218001-prerelease"></a>Microsoft.UI.Xaml 2.1.190218001-prerelease

2019 年 2 月

[GitHub リリース ページ](https://github.com/Microsoft/microsoft-ui-xaml/releases/tag/v2.1.190219001-prerelease)

[NuGet パッケージのダウンロード](https://www.nuget.org/packages/Microsoft.UI.Xaml/2.1.190218001-prerelease)

試験的な新機能:

* [TeachingTip コントロール](https://github.com/Microsoft/microsoft-ui-xaml/issues/21)  
  この新しいコントロールでは、非侵入型でコンテンツ豊富な通知を使用してアプリケーション内でユーザーにガイドと情報を提供する方法がアプリに提供されます。 TeachingTip を使用して、新しいまたは重要な機能にフォーカスを移し、タスクを実行する方法をユーザーに示すことができます。また、目前のタスクに対して文脈上関連する情報を提供することで、ユーザー ワークフローを強化することもできます。

### <a name="microsoftuixaml-21190131001-prerelease"></a>Microsoft.UI.Xaml 2.1.190131001-prerelease

2019 年 2 月

[GitHub リリース ページ](https://github.com/Microsoft/microsoft-ui-xaml/releases/tag/v2.1.190131001-prerelease)

[NuGet パッケージのダウンロード](https://www.nuget.org/packages/Microsoft.UI.Xaml/2.1.190131001-prerelease)

試験的な新機能:

* [AnimatedVisualPlayer](/uwp/api/microsoft.ui.xaml.controls.animatedvisualplayer)  
  この新しいコントロールにより、[Lottie-Windows](/windows/communitytoolkit/animations/lottie) を使用して作成された [Lottie](https://github.com/airbnb/lottie) アニメーションなど、複雑なハイ パフォーマンスのベクター アニメーションを再生できます。

### <a name="microsoftuixaml-21181217001-prerelease"></a>Microsoft.UI.Xaml 2.1.181217001-prerelease

2018 年 12 月

[GitHub リリース ページ](https://github.com/Microsoft/microsoft-ui-xaml/releases/tag/v2.1.181217001-prerelease)

[NuGet パッケージのダウンロード](https://www.nuget.org/packages/Microsoft.UI.Xaml/2.1.181217001-prerelease)

試験的な新機能:

* [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)

* [RadioButtons](/uwp/api/microsoft.ui.xaml.controls.radiobuttons)

* [RadioMenuFlyoutItem](/uwp/api/microsoft.ui.xaml.controls.radiomenuflyoutitem)