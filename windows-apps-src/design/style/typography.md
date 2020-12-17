---
description: コンテンツをユーザーに理解しやすくするための、アプリにおける文字体裁の使用方法について説明します。
title: Windows アプリの文字体裁
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 3660417f6e88268dd2ad814ce4dba62354029828
ms.sourcegitcommit: 05f9bb0c563e12daaacf7732d46f1c7f578dbe6a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/15/2020
ms.locfileid: "97529289"
---
# <a name="typography-in-windows-apps"></a>Windows アプリの文字体裁

![ヒーロー イメージ](images/header-typography.svg)

言語の視覚的表現である文字体裁において、何よりも重要な役割は情報を伝達することです。 スタイルによってその目的が邪魔されてはなりません。 この記事では、ユーザーが簡単かつ効率的にコンテンツを理解できるような Windows アプリの文字体裁のスタイルを決定する方法について説明します。

## <a name="font"></a>フォント

アプリの UI 全体で同じフォントを使用してください。Windows アプリの既定のフォントである **Segoe UI** に統一することをお勧めします。 このフォントは、常に最適な読みやすさが維持されるサイズとピクセル密度を備え、システムのコンテンツを清潔で軽快、かつオープンに美しく表示します。

![Segoe UI フォントのサンプル テキスト。](images/type/segoe-sample.svg)

アプリで英語以外の言語を表示する場合、または異なるフォントを選択する場合は、「[言語](#languages)」と「[フォント](#fonts)」のセクションで、弊社が Windows アプリ向けに推奨するフォントを確認してください。

:::row:::
    :::column:::
![緑色のチェック マークが付いており、"Do" と書かれている緑色のバーの最初のスクリーンショット。](images/do.svg)
UI のフォントを統一します。
    :::column-end:::
    :::column:::
![非推奨](images/dont.svg) 複数のフォントを混在させないでください。
    :::column-end:::
:::row-end:::

## <a name="size-and-scaling"></a>サイズとスケーリング

UWP アプリのフォント サイズは、すべてのデバイスで自動的に拡大縮小します。 スケーリング アルゴリズムによって、10 フィート離れた Surface Hub 上の 24 ピクセル フォントが、目の前にある 5 インチ サイズの電話の画面に表示された 24 ピクセル フォントと同じ読みやすさで表示されます。

![さまざまなデバイスの視距離。](images/type/scaling-chart.svg)

スケーリング システムのしくみを踏まえ、実際の物理ピクセルではなく、有効ピクセルに基づいてデザインしてください。異なる画面サイズや解像度に応じてフォント サイズを変更する必要はありません。

:::row:::
    :::column:::
![緑色のチェック マークが付いており、"Do" と書かれている緑色のバーの 2 番目のスクリーンショット。](images/do.svg)
Windows の[書体見本](#type-ramp)に従います。
    :::column-end:::
    :::column:::
![非推奨](images/dont.svg) 12 ピクセルよりも小さいフォント サイズを使用しないでください。
    :::column-end:::
:::row-end:::

## <a name="hierarchy"></a>階層

:::row:::
    :::column:::
ユーザーはページを斜め読みするとき、視覚的な階層を手掛かりにしています。見出しは内容を要約し、本文は詳細を説明するものと想定されます。 アプリでわかりやすい視覚的な階層を作成するためには、Windows 書体見本に従ってください。
    :::column-end:::
    :::column:::
![3 行のテキストを行ごとに少しずつ小さいフォントで表示したスクリーンショット。](images/type/type-hierarchy.svg)
    :::column-end:::
:::row-end:::

### <a name="type-ramp"></a>書体見本

Windows 書体見本は、ユーザーがコンテンツを読みやすいように、ページ上の各書体スタイル間の重要な関係を定めたものです。 すべてのサイズは有効ピクセル単位で示され、UWP アプリが動作するデバイスを問わず、常に最適に表示されるように調整されています。

![Windows 書体見本。](images/type/type-ramp.png)

### <a name="using-the-type-ramp"></a>書体見本の使用

:::row:::
    :::column:::
各レベルの書体見本は、XAML の[静的リソース](../controls-and-patterns/xaml-theme-resources.md#the-xaml-type-ramp)としてアクセスできます。 これらのスタイルは、ここに示した `*TextBlockStyle` 名前付け規則に従っています。

```XAML
<TextBlock Text="Header" Style="{StaticResource HeaderTextBlockStyle}"/>
<TextBlock Text="SubHeader" Style="{StaticResource SubheaderTextBlockStyle}"/>
<TextBlock Text="Title" Style="{StaticResource TitleTextBlockStyle}"/>
<TextBlock Text="SubTitle" Style="{StaticResource SubtitleTextBlockStyle}"/>
<TextBlock Text="Base" Style="{StaticResource BaseTextBlockStyle}"/>
<TextBlock Text="Body" Style="{StaticResource BodyTextBlockStyle}"/>
<TextBlock Text="Caption" Style="{StaticResource CaptionTextBlockStyle}"/>
```
    :::column-end:::
    :::column:::
![Header、Subheader、Title、Subtitle、Base、Body、Caption テキスト スタイルのスクリーンショット。](images/type/text-block-type-ramp.svg)
    :::column-end:::
:::row-end:::



:::row:::
    :::column:::
![緑色のチェック マークが付いており、"Do" と書かれている緑色のバーの 3 番目のスクリーンショット。](images/do.svg)
ほとんどのテキストには "Body" を使用します。

スペースに制約がある場合はタイトルに "Base" を使用します。
    :::column-end:::
    :::column:::
![非推奨](images/dont.svg) プライマリ操作や長い文字列に "Caption" を使用する。

テキストを折り返す必要がある場合に "Header" や "Subheader" を使用する。
    :::column-end:::
:::row-end:::

## <a name="alignment"></a>配置

既定の [TextAlignment](/uwp/api/windows.ui.xaml.textalignment) (行揃え) は Left (左揃え) です。ほとんどの場合、左揃え、右不揃いの形式でコンテンツを一貫してアンカー設定することで、均一なレイアウトが実現します。 RTL 言語については、[グローバリゼーションをサポートするためのレイアウトとフォントの調整に関するページ](../globalizing/adjust-layout-and-fonts--and-support-rtl.md)をご覧ください。

![左揃えテキストを示します。](images/type/alignment.svg)

```xaml
<TextBlock TextAlignment="Left">
```

## <a name="character-count"></a>文字カウント

:::row:::
    :::column:::
![緑色のチェック マークが付いており、"Do" と書かれている緑色のバーの 4 番目のスクリーンショット。](images/do.svg)
読みやすさを確保するため、1 行当たり 50 から 60 文字にします。
    :::column-end:::
    :::column:::
![非推奨](images/dont.svg) 1 行当たりの文字カウントが 20 文字を下回るか 60 文字を超えると読みにくくなります。
    :::column-end:::
:::row-end:::

## <a name="clipping-and-ellipses"></a>クリッピングと省略記号

テキストの量が利用可能なスペースを超えている場合は、テキストをクリッピングすることが推奨されます。クリッピングは、ほとんどの [UWP テキスト コントロール](../controls-and-patterns/text-controls.md) で既定の処理です。

![一部がテキスト クリッピングされたデバイス フレームを示しています。](images/type/clipping.svg)

```xaml
<TextBlock TextWrapping="WrapWholeWords" TextTrimming="Clip"/>
```

:::row:::
    :::column:::
![緑色のチェック マークが付いており、"Do" と書かれている緑色のバーの 5 番目のスクリーンショット。](images/do.svg)
テキストをクリップし、複数行を使用できる場合は、行を折り返します。
    :::column-end:::
    :::column:::
![非推奨](images/dont.svg) すっきりと表示するため、省略記号は使用しないでください。
    :::column-end:::
:::row-end:::

**注**:表示領域が不明確な場合 (領域が異なる背景色によって明確に表示されていない場合など)、または詳細テキストへのリンクがある場合は、省略記号を使用します。

## <a name="languages"></a>言語 

Segoe UI は、英語、ヨーロッパの各言語、ギリシャ語、ヘブライ語、アルメニア語、ジョージア語、アラビア語に対応した弊社のフォントです。 他の言語については、以下の推奨事項を参照してください。

### <a name="globalizinglocalizing-fonts"></a>フォントのグローバリゼーション/ローカライズ

特定言語の推奨フォント ファミリー、サイズ、太さ、スタイルにプログラムを使ってアクセスする場合は、[LanguageFont フォント マッピング API](/uwp/api/Windows.Globalization.Fonts.LanguageFont) を使ってください。 LanguageFont オブジェクトを使うと、コンテンツのさまざまなカテゴリ (UI ヘッダー、通知、本文のテキスト、ユーザー自身で編集できるドキュメント本文のフォントなど) の正しいフォント情報にアクセスできます。 詳しくは、「[レイアウトとグローバリゼーションをサポートするフォントの調整](../globalizing/adjust-layout-and-fonts--and-support-rtl.md)」をご覧ください。

### <a name="fonts-for-non-latin-languages"></a>ラテン語以外の言語用のフォント

<table>
<thead>
<tr class="header">
<th align="left">フォント ファミリー</th>
<th align="left">スタイル</th>
<th align="left">メモ</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="font-family: Embrima;">Ebrima</td>
<td align="left">標準、太字</td>
<td align="left">アフリカのスクリプト (エチオピア文字、ンコ文字、オスマニア文字、ティフィナグ文字、ヴァイ文字) 用のユーザー インターフェイス フォント。</td>
</tr>
<tr class="even">
<td style="font-family: Gadugi;">Gadugi</td>
<td align="left">標準、太字</td>
<td align="left">北アメリカ スクリプト (カナダ音節文字、チェロキー文字) 用のユーザー インターフェイス フォント。</td>
</tr>
<tr class="odd">
<td align="left" style="font-family: Leelawadee UI;">Leelawadee UI</td>
<td align="left">通常、Semilight、太字</td>
<td align="left">東南アジアのスクリプト (ブギス文字、ラオス文字、クメール文字、タイ文字) 用のユーザー インターフェイス フォント。</td>
</tr>
<tr class="odd">
<td align="left" style="font-family: Malgun Gothic;">Malgun Gothic</td>
<td align="left">標準</td>
<td align="left">韓国語用のユーザー インターフェイス フォント。</td>
</tr>
<tr class="even">
<td align="left" style="font-family: Microsoft JhengHei UI;">Microsoft JhengHei UI</td>
<td align="left">標準、太字、細字</td>
<td align="left">繁体字中国語用のユーザー インターフェイス フォント。</td>
</tr>
<tr class="odd">
<td align="left" style="font-family: Microsoft YaHei UI;">Microsoft YaHei UI</td>
<td align="left">標準、太字、細字</td>
<td align="left">簡体中国語用のユーザー インターフェイス フォント。</td>
</tr>
<tr class="odd">
<td align="left" style="font-family: Myanmar Text;">Myanmar Text</td>
<td align="left">標準</td>
<td align="left">ミャンマー文字のスクリプト用のフォールバック フォント。</td>
</tr>
<tr class="even">
<td align="left" style="font-family: Nirmala UI;">Nirmala UI</td>
<td align="left">通常、Semilight、太字</td>
<td align="left">南アジア言語のスクリプト (バングラ文字、デーバナーガリー文字、グジャラート文字、グルムキー文字、カンナダ文字、マラヤーラム文字、オディア文字、オル チキ文字、シンハラ文字、ソラング ソンペング文字、タミール文字、テルグ文字) 用のユーザー インターフェイス フォント</td>
</tr>
<tr class="odd">
<td align="left" style="font-family: SimSun;">SimSun</td>
<td align="left">標準</td>
<td align="left">中国語繁体字の UI フォント。 </td>
</tr>
<tr class="even">
<td align="left" style="font-family: Yu Gothic UI;">Yu Gothic UI</td>
<td align="left">細字、中細、標準、中太、太字</td>
<td align="left">日本語用のユーザー インターフェイス フォント。</td>
</tr>
</tbody>
</table>

## <a name="fonts"></a>フォント

### <a name="sans-serif-fonts"></a>サンセリフ フォント

サンセリフ フォントは、ヘッダーと UI 要素に適しています。 

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">フォント ファミリー</th>
<th align="left">スタイル</th>
<th align="left">メモ</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left" style="font-family: Arial;">Arial</td>
<td align="left">通常、斜体、太字、太字斜体、黒</td>
<td align="left">ヨーロッパおよび中東のスクリプト (ラテン文字、ギリシャ文字、キリル文字、アラビア文字、アルメニア文字、ヘブライ文字) をサポートしています。極太の太さがサポートされているのはヨーロッパのスクリプトだけです。</td>
</tr>
<tr class="even">
<td align="left" style="font-family: Calibri;">Calibri</td>
<td align="left">通常、斜体、太字、太字斜体、中細、中細斜体</td>
<td align="left">ヨーロッパおよび中東のスクリプト (ラテン文字、ギリシャ文字、キリル文字、アラビア語、ヘブライ語) をサポートしています。 アラビア語は縦書きでのみ利用できます。</td>
</tr>
<td style="font-family: Consolas;">Consolas</td>
<td>標準、斜体、太字、太字斜体</td>
<td>ヨーロッパのスクリプト (ラテン文字、ギリシャ文字、キリル文字) をサポートする等幅フォント。</td>
</tr>

<tr>
<td style="font-family: Segoe UI;">Segoe UI</td>
<td>標準、斜体、細字斜体、極太斜体、太字、太字斜体、細字、中細、中太、極太</td>
<td>ヨーロッパおよび中東のスクリプト (アラビア文字、アルメニア文字、キリル文字、ジョージア文字、ギリシャ文字、ヘブライ文字、ラテン文字) およびリス文字のスクリプト用のユーザー インターフェイス フォント。</td>
</tr>

<tr class="even">
<td style="font-family: Selawik;">Selawik</td>
<td align="left">標準、中細、細字、太字、中太</td>
<td align="left">他のプラットフォーム上で動作する Segoe UI をバンドルしないアプリ向けの、Segoe UI と測定上の互換性があるオープン ソース フォント。 <a href="https://github.com/Microsoft/Selawik">Selawik は、GitHub で入手できます。</a></td>
</tr>

</tbody>
</table>

### <a name="serif-fonts"></a>セリフ フォント

セリフ フォントは、大量のテキストを表示するのに適しています。 

<table>
<thead>
<tr class="header">
<th align="left">フォント ファミリー</th>
<th align="left">スタイル</th>
<th align="left">メモ</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="font-family: Cambria;">Cambria</td>
<td align="left">標準</td>
<td align="left">ヨーロッパのスクリプト (ラテン文字、ギリシャ文字、キリル文字) をサポートするセリフ フォント。</td>
</tr>
<tr class="even">
<td style="font-family: Courier New;">Courier New</td>
<td align="left">標準、斜体、太字、太字斜体</td>
<td align="left">セリフ等幅フォントは、ヨーロッパおよび中東のスクリプト (ラテン文字、ギリシャ文字、キリル文字、アラビア文字、アルメニア文字、ヘブライ文字) をサポートしています。</td>
</tr>
<tr class="odd">
<td style="font-family: Georgia;">ジョージア</td>
<td align="left">標準、斜体、太字、太字斜体</td>
<td align="left">ヨーロッパのスクリプト (ラテン文字、ギリシャ文字、キリル文字) をサポートしています。</td>
</tr>

<tr class="even">
<td style="font-family: Times New Roman;">Times New Roman</td>
<td align="left">標準、斜体、太字、太字斜体</td>
<td align="left">ヨーロッパのスクリプト (ラテン文字、ギリシャ文字、キリル文字、アラビア文字、アルメニア文字、ヘブライ文字) をサポートしている従来のフォント。</td>
</tr>

</tbody>
</table>

### <a name="symbols-and-icons"></a>シンボルとアイコン

<table>
<thead>
<tr class="header">
<th align="left">フォント ファミリー</th>
<th align="left">スタイル</th>
<th align="left">メモ</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Segoe MDL2 アセット</td>
<td align="left">標準</td>
<td align="left">アプリ アイコン用のユーザー インターフェイス フォント。 詳しくは、<a href="segoe-ui-symbol-font.md">Segoe MDL2 アセットの記事</a>をご覧ください。</td>
</tr>
<tr class="even">
<td align="left">Segoe UI Emoji</td>
<td align="left">標準</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">Segoe UI Symbol</td>
<td align="left">標準</td>
<td align="left">記号用のフォールバック フォント</td>
</tr>
</tbody>
</table>

## <a name="related-articles"></a>関連記事

* [テキスト コントロール](../controls-and-patterns/text-controls.md)
* [XAML テーマ リソース](../controls-and-patterns/xaml-theme-resources.md#the-xaml-type-ramp)
* [XAML スタイル](../controls-and-patterns/xaml-styles.md)
* [Microsoft の文字体裁](/typography/)
