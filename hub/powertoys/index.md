---
title: Microsoft PowerToys
description: Microsoft PowerToys は、Windows 10 をカスタマイズするための一連のユーティリティです。 ユーティリティには、ColorPicker (任意の場所をクリックして色の値を取得)、FancyZones (ウィンドウをグリッド レイアウトに配置するためのショートカット)、エクスプローラーのアドオン (SVG または Markdown ファイルをプレビュー)、Image Resizer (1 つまたは複数の画像を右クリックで簡単にサイズ変更)、Keyboard Manager (キーの再割り当てまたは独自のショートカットを作成)、PowerRename (検索と置換を使用した一括名前変更)、PowerToys Run (Alt + Space でアプリを再起動)、ショートカット ガイドなどが含まれています。
ms.date: 12/02/2020
ms.topic: overview
ms.localizationpriority: medium
ms.openlocfilehash: 5e7e88e8ff179ebbb63aa7369c22149b645c9838
ms.sourcegitcommit: afc4ff2c89f148d32073ab1cc42063ccdc573a8c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/11/2021
ms.locfileid: "98104583"
---
# <a name="microsoft-powertoys-utilities-to-customize-windows-10"></a>Microsoft PowerToys:Windows 10 をカスタマイズするためのユーティリティ

Microsoft PowerToys は、パワー ユーザーが Windows 10 エクスペリエンスを調整および合理化して生産性を向上させるためのユーティリティ セットです。

> [!div class="nextstepaction"]
> [PowerToys をインストールする](install.md)

## <a name="processor-support"></a>プロセッサのサポート

- **x64**:サポート対象
- **x86**:開発中 ([問題 #602](https://github.com/microsoft/PowerToys/issues/602) を参照してください)
- **ARM**:開発中 ([問題 #490](https://github.com/microsoft/PowerToys/issues/490) を参照してください)

## <a name="current-powertoy-utilities"></a>最新の PowerToy ユーティリティ

現在利用可能なユーティリティは、次のとおりです。

### <a name="color-picker"></a>カラー ピッカー

:::row:::
    :::column:::
        [![ColorPicker スクリーンショット](../images/pt-color-picker.png)](color-picker.md)
    :::column-end:::
    :::column span="2":::
        [ColorPicker](color-picker.md) は、<kbd>Win</kbd>+<kbd>Shift</kbd>+<kbd>C</kbd> でアクティブ化されるシステム全体の色選択ユーティリティです。 現在実行中のアプリケーションから色を選択すると、ピッカーによって構成可能な形式でクリップボードに色が自動的にコピーされます。 Color Picker には、以前に選択した色の履歴を表示するエディターも用意されており、選択した色を微調整したり、別の文字列形式をコピーしたりすることができます。 このコードは、[Martin Chrzan の Color Picker](https://github.com/martinchrzan/ColorPicker) に基づいています。
    :::column-end:::
:::row-end:::

### <a name="fancy-zones"></a>Fancy Zones

:::row:::
    :::column:::
        [![FancyZones のスクリーンショット](../images/pt-fancy-zones.png)](fancyzones.md)
    :::column-end:::
    :::column span="2":::
        [FancyZones](fancyzones.md) は、複雑なウィンドウ レイアウトを簡単に作成し、ウィンドウをこれらのレイアウトにすばやく配置できるウィンドウ マネージャーです。
    :::column-end:::
:::row-end:::

### <a name="file-explorer-add-ons"></a>エクスプローラーのアドオン

:::row:::
    :::column:::
        [![エクスプローラーのスクリーンショット](../images/pt-file-explorer.png)](file-explorer.md)
    :::column-end:::
    :::column span="2":::
        [エクスプローラー](file-explorer.md)のアドオンでは、エクスプローラーでプレビュー ウィンドウの表示を有効にして、SVG アイコン (.svg) および Markdown (.md) ファイルのプレビューを表示できます。 プレビュー ウィンドウを有効にするには、エクスプローラーで [表示] タブを選択し、[プレビューウィンドウ] を選択します。
    :::column-end:::
:::row-end:::

### <a name="image-resizer"></a>Image Resizer

:::row:::
    :::column:::
        [![Image Resizer のスクリーンショット](../images/pt-image-resizer.png)](image-resizer.md)
    :::column-end:::
    :::column span="2":::
        [Image Resizer](image-resizer.md) は、画像のサイズをすばやく変更するための Windows シェル拡張機能です。  エクスプローラーから単純に右クリックして、1 つまたは複数の画像のサイズを瞬時に変更します。 このコードは、[Brice Lambson の Image Resizer](https://github.com/bricelam/ImageResizer) に基づいています。
    :::column-end:::
:::row-end:::

### <a name="keyboard-manager"></a>Keyboard Manager

:::row:::
    :::column:::
        [![Keyboard Manager のスクリーンショット](../images/pt-keyboard-manager.png)](keyboard-manager.md)
    :::column-end:::
    :::column span="2":::
        [Keyboard Manager](keyboard-manager.md) を使用すると、キーを再割り当てしたり、独自のキーボード ショートカットを作成したりして、キーをカスタマイズして生産性を向上させることができます。 この PowerToy には、Windows 10 1903 (ビルド 18362) 以降が必要です。
    :::column-end:::
:::row-end:::

### <a name="powerrename"></a>PowerRename

:::row:::
    :::column:::
        [![PowerRename のスクリーンショット](../images/pt-rename.png)](powerrename.md)
    :::column-end:::
    :::column span="2":::
        [PowerRename](powerrename.md) では、ファイル名の一括変更、検索、置換を実行できます。 これには、正規表現の使用、特定のファイルの種類のターゲット設定、予想される結果のプレビュー、変更を元に戻す機能など、高度な機能が用意されています。 このコードは、[Chris Davis の SmartRename](https://github.com/chrdavis/SmartRename) に基づいています。
    :::column-end:::
:::row-end:::

### <a name="powertoys-run"></a>PowerToys Run

:::row:::
    :::column:::
        [![PowerToys Run のスクリーンショット](../images/pt-run.png)](run.md)
    :::column-end:::
    :::column span="2":::
        [PowerToys Run](run.md) を使用すると、アプリをすぐに検索して起動できます。ショートカット <kbd>Alt</kbd>+<kbd>Space</kbd> を入力して、入力を開始するだけです。 これはオープン ソースであり、追加のプラグイン用のモジュラーです。 Window Walker も含まれるようになりました。 この PowerToy には、Windows 10 1903 (ビルド 18362) 以降が必要です。
    :::column-end:::
:::row-end:::

### <a name="shortcut-guide"></a>Shortcut Guide

:::row:::
    :::column:::
        [![Shortcut Guide のスクリーンショット](../images/pt-shortcut-guide.png)](shortcut-guide.md)
    :::column-end:::
    :::column span="2":::
        [Windows キー ショートカット ガイド](shortcut-guide.md)は、ユーザーが 1 秒以上 Windows キーを押したままにすると表示され、デスクトップの現在の状態で使用可能なショートカットが表示されます。
    :::column-end:::
:::row-end:::

## <a name="powertoys-video-walk-through"></a>PowerToys ビデオ チュートリアル

このビデオでは、Clint Rutkas (PowerToys 担当 PM) が、さまざまなユーティリティのインストールおよび使用方法のほか、いくつかのヒント、投稿方法に関する情報を紹介します。

> [!VIDEO https://channel9.msdn.com/Shows/Tabs-vs-Spaces/PowerToys-Utilities-to-customize-Windows-10/player?format=ny]

## <a name="future-powertoy-utilities"></a>今後の PowerToy ユーティリティ

### <a name="experimental-powertoys"></a>試験的な PowerToys

プレリリース版の PowerToys 試験版をインストールして、次のような最新の実験的なユーティリティをお試しください。

#### <a name="video-conference-mute-experimental"></a>Video Conference Mute (試験段階)

:::row:::
    :::column:::
        [![Video Conference Mute のスクリーンショット](../images/pt-video-conference-mute.png)](video-conference-mute.md)
    :::column-end:::
    :::column span="2":::
        [Video Conference Mute](video-conference-mute.md) を使用すると、現在フォーカスがあるアプリケーションに関係なく、電話会議で <kbd>⊞ Win</kbd>+<kbd>N</kbd> を使用して、マイクとカメラの両方をグローバルに簡単に "ミュート" できます。 これは、[PowerToys のプレリリース/試験版](https://github.com/microsoft/PowerToys/releases/)にのみ含まれており、Windows 10 1903 (ビルド 18362) 以降が必要です。
    :::column-end:::
:::row-end:::

## <a name="known-issues"></a>既知の問題

GitHub の PowerToys リポジトリの[イシュー](https://github.com/microsoft/PowerToys/issues) タブで、既知の問題を検索したり、新しい問題を報告したりできます。

## <a name="contribute-to-powertoys-open-source"></a>PowerToys (オープン ソース) に投稿する

PowerToys に関する投稿をお寄せください。 PowerToys 開発チームは、パワー ユーザー コミュニティと提携して、ユーザーが Windows を最大限に活用するのに役立つツールを構築することを楽しみにしています。 投稿にはさまざまな方法があります。

- [技術仕様](https://codeburst.io/on-writing-tech-specs-6404c9791159)を書く
- [設計の概念または推奨事項](https://www.microsoft.com/design/inclusive/)を送信する
- [ドキュメントへの投稿](/contribute/)
- [ソース コード](https://github.com/microsoft/PowerToys/tree/master/src)内のバグを特定して修正する
- [新機能と PowerToy ユーティリティのコードを記述する](https://github.com/microsoft/PowerToys/tree/master/doc/devdocs)

投稿する機能について作業を開始する前に、 **[投稿者向けガイド](https://github.com/microsoft/PowerToys/blob/master/CONTRIBUTING.md)をお読みください**。 PowerToys チームがお客様と協力して最適なアプローチを特定し、機能の開発全体のガイダンスを提供して指導するため、作業の無駄や重複を回避できます。

## <a name="powertoys-release-notes"></a>PowerToys のリリース ノート

PowerToys の[リリース ノート](https://github.com/microsoft/PowerToys/releases/)は、GitHub リポジトリのインストール ページに表示されています。 参考までに、PowerToys wiki の[リリース チェックリスト](https://github.com/microsoft/PowerToys/wiki/Release-check-list)も参照できます。

## <a name="powertoys-history"></a>PowerToys の履歴

[Windows 95 時代の PowerToys プロジェクト](https://en.wikipedia.org/wiki/Microsoft_PowerToys)に着想を得て、このリブートによりパワー ユーザーはさまざまな方法で Windows 10 シェルの効率を向上させ、個々のワークフローに合わせてこれをカスタマイズすることができます。  Windows 95 PowerToys の概要については、[こちら](https://socket3.wordpress.com/2016/10/22/using-windows-95-powertoys/)を参照してください。

## <a name="powertoys-roadmap"></a>PowerToys のロードマップ

PowerToys は、パワー ユーザーに Windows 10 シェルの効率を高め、個々のワークフローに合わせてこれをカスタマイズするためのさまざまな方法を提供することを目的とした急速養成のオープン ソース チームです。 作業の優先順位は、ユーザーの生産性を向上させることを目的として、一貫して調査、再評価、調整されます。

- [PowerToys の新しい仕様](https://github.com/microsoft/PowerToys/wiki/Specs)
- [バックログ優先順位一覧](https://github.com/microsoft/PowerToys/wiki/Roadmap#backlog-priority-list-in-order)
- [バージョン1.0 戦略仕様](https://github.com/microsoft/PowerToys/wiki/Version-1.0-Strategy)、2020 年 2 月