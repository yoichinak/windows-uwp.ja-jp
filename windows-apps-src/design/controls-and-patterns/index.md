---
description: Windows アプリにコントロールとパターンを追加する方法についての設計ガイダンスとコーディングの手順を入手できます。 アプリで使用できる 45 種類以上の強力なコントロールを紹介します。
title: Windows のコントロールとパターン - Windows アプリの開発
keywords: UWP コントロール, ユーザー インターフェイス, アプリ コントロール, Windows コントロール
label: Controls & patterns
template: detail.hbs
ms.date: 03/23/2020
ms.topic: article
ms.assetid: ce2e611c-c419-4a14-9095-b88ac711d1b8
ms.localizationpriority: medium
ms.openlocfilehash: 08471b8a04d37c34378b549b8534979a4188d7b8
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173956"
---
# <a name="controls-for-windows-apps"></a>Windows アプリ用のコントロール

![コントロール](../images/controls-2x.png)

Windows アプリの開発では、"<i>コントロール</i>" は、コンテンツを表示したり、操作を有効にしたりする UI 要素です。 コントロールとは、ユーザー インターフェイスの構成要素です。 <i>パターン</i>とは、いくつかのコントロールを組み合わせて、新しいものを作成するためのレシピです。

単純なボタンから、グリッド ビューのような強力なデータ コントロールまで、ユーザーが使用できる 45 種類以上のコントロールが用意されています。  これらのコントロールは Fluent Design System の一部です。すべでのデバイスやあらゆる画面サイズで見栄えがよく、力強い、スケーラブルな UI を作成できます。

このセクションの記事では、Windows アプリにコントロールとパターンを追加するための設計ガイダンスとコーディングの手順を示します。

## <a name="intro"></a>はじめに

XAML と C# でコントロールを追加し、スタイルを指定するための一般的な手順とコード例を示します。

:::row:::
    :::column:::
      <p><b><a href="controls-and-events-intro.md">コントロールを追加し、イベントを処理する</a></b> <br/>
アプリにコントロールを追加するには、3 つの重要な手順があります。アプリの UI にコントロールを追加し、コントロールのプロパティを設定し、コントロールを動作させるためのコードをコントロールのイベント ハンドラーに追加します。</p>
    :::column-end:::
    :::column:::
      <p><b><a href="xaml-styles.md">コントロールのスタイル指定</a></b> <br/>
XAML フレームワークを使って、さまざまな方法でアプリの外観をカスタマイズできます。 スタイルを使うと、コントロールのプロパティに値を設定し、その設定を再利用することで、複数のコントロールの外観を統一できます。</p>
    :::column-end:::
:::row-end:::

## <a name="get-the-windows-ui-library"></a>Windows UI ライブラリを入手する

|  |  |
| - | - |
| ![WinUI ロゴ](images/winui-logo-64x64.png) | 一部のコントロールは、Windows UI ライブラリ (WinUI) (新しいコントロールと UI 機能を含む NuGet パッケージ) でのみ入手可能です。 これを入手する場合は、[Windows UI ライブラリの概要とインストール手順](/uwp/toolkits/winui/)に関するページを参照してください。<br/>WinUI 2.2 から、多くのコントロールで既定のスタイルが更新され、角の丸めを使用するようになりました。 詳しくは、「[角の半径](../style/rounded-corner.md)」をご覧ください。 |

## <a name="alphabetical-index"></a>アルファベット順インデックス

特定のコントロールとパターンに関する詳細情報を説明します。 (機能別に並べ替えた一覧については、「[機能別コントロールのインデックス](controls-by-function.md)」をご覧ください。)

:::row:::
    :::column:::

- アニメーション化された視覚的プレーヤー ([Lottie](/windows/communitytoolkit/animations/lottie)) ![WinUI ロゴ](images/winui-logo-16x16.png)
- [自動提案ボックス](auto-suggest-box.md)
- [ボタン](buttons.md)
- [カレンダーの日付の選択コントロール](calendar-date-picker.md)
- [カレンダー ビュー](calendar-view.md)
- [チェック ボックス](checkbox.md)
- [カラー ピッカー](color-picker.md) ![WinUI ロゴ](images/winui-logo-16x16.png)
- [コンボ ボックス](combo-box.md)
- [コマンド バー](app-bars.md)
- [コマンド バーのポップアップ](command-bar-flyout.md) ![WinUI ロゴ](images/winui-logo-16x16.png)
- [連絡先カード](contact-card.md)
- [コンテンツ ダイアログ](dialogs-and-flyouts/dialogs.md)
- [コンテンツ リンク](content-links.md)
- [コンテキスト メニュー](menus.md)
- [日付の選択コントロール](date-picker.md)
- [ダイアログとポップアップ](dialogs-and-flyouts/index.md)
- [ドロップ ダウン ボタン](buttons.md#create-a-drop-down-button) ![WinUI ロゴ](images/winui-logo-16x16.png)
- [FlipView](flipview.md)
- [Flyout](dialogs-and-flyouts/flyouts.md)
- [フォーム](forms.md) (パターン)
- [グリッド ビュー](listview-and-gridview.md)
- [ハイパーリンク](hyperlinks.md)
- [ハイパーリンク ボタン](hyperlinks.md#create-a-hyperlinkbutton)
- [画像とイメージ ブラシ](images-imagebrushes.md)
- [インク コントロール](inking-controls.md)
- [リスト ビュー](listview-and-gridview.md)
- [マップ コントロール](../../maps-and-location/display-maps.md)
- [マスターと詳細](master-details.md) (パターン)
- [メディア再生](media-playback.md)
- [メニュー バー](menus.md#create-a-menu-bar) ![WinUI ロゴ](images/winui-logo-16x16.png)
- [メニュー ポップアップ](menus.md)
- [ナビゲーション ビュー](navigationview.md) ![WinUI ロゴ](images/winui-logo-16x16.png)

    :::column-end:::
    :::column:::

- [数値ボックス](number-box.md) ![WinUI ロゴ](images/winui-logo-16x16.png)
- [視差ビュー](..\motion\parallax.md) ![WinUI ロゴ](images/winui-logo-16x16.png)
- [パスワード ボックス](password-box.md)
- [ユーザー画像](person-picture.md) ![WinUI ロゴ](images/winui-logo-16x16.png)
- [ピボット](pivot.md)
- [進行状況バー](progress-controls.md) ![WinUI ロゴ](images/winui-logo-16x16.png)
- [進行状況リング](progress-controls.md) ![WinUI ロゴ](images/winui-logo-16x16.png)
- [ラジオ ボタン](radio-button.md) ![WinUI ロゴ](images/winui-logo-16x16.png)
- [評価コントロール](rating.md) ![WinUI ロゴ](images/winui-logo-16x16.png)
- [繰り返しボタン](buttons.md#create-a-repeat-button)
- [リッチ エディット ボックス](rich-edit-box.md)
- [リッチ テキスト ブロック](rich-text-block.md)
- [スクロール ビューアー](scroll-controls.md)
- [検索](search.md) (パターン)
- [セマンティック ズーム](semantic-zoom.md)
- [図形](shapes.md)
- [スライダー](slider.md)
- [分割ボタン](buttons.md#create-a-split-button) ![WinUI ロゴ](images/winui-logo-16x16.png)
- [分割ビュー](split-view.md)
- [スワイプ コントロール](swipe.md) ![WinUI ロゴ](images/winui-logo-16x16.png)
- [タブ ビュー](tab-view.md) ![WinUI ロゴ](images/winui-logo-16x16.png)
- [教育のヒント](dialogs-and-flyouts/teaching-tip.md) ![WinUI ロゴ](images/winui-logo-16x16.png)
- [テキスト ブロック](text-block.md)
- [テキスト ボックス](text-box.md)
- [時刻の選択コントロール](time-picker.md)
- [トグル スイッチ](toggles.md)
- [トグル ボタン](buttons.md)
- [分割トグル ボタン](buttons.md#create-a-toggle-split-button)
- [ヒント](tooltips.md)
- [ツリー ビュー](tree-view.md) ![WinUI ロゴ](images/winui-logo-16x16.png)
- [2 つのペインからなるビュー](two-pane-view.md) ![WinUI ロゴ](images/winui-logo-16x16.png)
- [Web ビュー](web-view.md)

    :::column-end:::
:::row-end:::




## <a name="xaml-controls-gallery"></a>XAML コントロール ギャラリー

Microsoft Store から _XAML コントロール ギャラリー_ アプリを入手し、これらのコントロールおよび Fluent Design System の動作を確認します。 このアプリは、この Web サイトの対話型コンパニオンです。 このアプリがインストールされている場合、個々のコントロール ページのリンクを使用して、アプリを起動し、コントロールの動作を確認することができます。

<a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML コントロール ギャラリー アプリを入手する (Microsoft Store)</a>

<a href="https://github.com/Microsoft/Xaml-Controls-Gallery">ソース コード (GitHub) を入手する</a>

<img src="images/xaml-controls-gallery.png" alt="XAML Controls Gallery screen" />

## <a name="additional-controls"></a>その他のコントロール

Windows 開発用の追加のコントロールは、<a href="https://www.telerik.com/">Telerik</a>、<a href="https://www.syncfusion.com/uwp-ui-controls">SyncFusion</a>、<a href="https://www.devexpress.com/Products/NET/Controls/Win10Apps/">DevExpress</a>、<a href="https://www.infragistics.com/products/universal-windows-platform">Infragistics</a>、<a href="https://www.componentone.com/Studio/Platform/UWP">ComponentOne</a>、<a href="https://www.actiprosoftware.com/products/controls/universal">ActiPro</a> などの企業から入手できます。 これらのコントロールは、カスタム コントロールおよびサービスによって標準システム コントロールを補うことにより、エンタープライズおよび .NET 開発者に追加のサポートを提供します。