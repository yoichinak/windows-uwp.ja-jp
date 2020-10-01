---
title: 間隔とサイズ
description: 新しい Fluent Standard および Compact コントロール スタイルを使うと、デバイスや入力方法に関係なく、快適なユーザー エクスペリエンスを確保できます。
keywords: UWP, Windows 10, コントロール, サイズ, 密度, 標準, コンパクト
ms.date: 09/24/2020
ms.topic: article
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: aa4d1fa64f23f417957b4590bb3a846f00653869
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/24/2020
ms.locfileid: "91217755"
---
# <a name="control-size-and-density"></a>コントロールのサイズと密度

コントロールのサイズと密度の組み合わせを使って、Windows アプリケーションを最適化し、アプリの機能と対話式操作の要件に最適なユーザー エクスペリエンスを提供します。

既定では、UWP アプリは低密度 (または `Standard`) のレイアウトでレンダリングされます。 ただし、WinUI 2.1 以降では、情報量の多い UI や同様の特殊なシナリオのための高密度 (または`Compact`) レイアウト オプションもサポートされます。 これは、基本的なスタイル リソースで指定できます (次の例を参照)。

機能と動作は変わらず、サイズと密度の 2 つのオプション間で一貫性が保たれていますが、既定の本文フォント サイズは、これら 2 つの密度オプションをサポートするため、すべてのコントロールで 14 ピクセルに更新されました。 このフォント サイズは領域やデバイスが異なっても機能し、アプリケーションのバランスが取れたユーザーに使いやすい状態を保ちます。

## <a name="examples"></a>例

<table>
<th align="left">XAML コントロール ギャラリー<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML コントロール ギャラリー</strong> アプリがインストールされている場合、こちらをクリックして<a href="xamlcontrolsgallery:/item/Compact Sizing">アプリを開き、Compact サイズの動作を確認</a>してください。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML コントロール ギャラリー アプリを入手する (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">ソース コード (GitHub) を入手する</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="fluent-standard-sizing"></a>Fluent Standard サイズ

"*Fluent Standard サイズ*" は、情報の密度とユーザーの快適さのバランスを取るために作成されました。 実質的に、画面上のすべての項目が 40 x 40 の有効ピクセル (epx) ターゲットに揃えられ、UI 要素をグリッドに位置合わせし、システム レベルのスケーリングに基づいて適切にスケーリングできます

**Standard サイズは、タッチ入力とポインター入力の両方に対応するように設計されています。**

> [!NOTE]
>有効ピクセルとスケーリングについて詳しくは、[Windows アプリ デザインの概要](../basics/design-and-ui-intro.md#effective-pixels-and-scaling)に関するページをご覧ください。
>
> システム レベルのスケーリングについて詳しくは、「[配置、余白、パディング](../layout/alignment-margin-padding.md)」をご覧ください。

Windows 10 October 2018 Update (バージョン 1809) では、すべての UWP コントロールに対する標準の既定サイズが、すべての使用シナリオで使いやすさが向上するように、小さくされました。

次の図では、Windows 10 October 2018 Update で導入されたコントロールのレイアウト変更の一部を示します。 具体的には、ヘッダーとコントロール上端の間の余白が 8 epx から 4 epx に減らされ、44epx のグリッドが 40 epx のグリッドに変更されました。

![Standard コントロール レイアウトの例](images/standarddensity.png)

*Standard コントロール レイアウトの例*

次の図では、Windows 10 October 2018 Update でコントロールのサイズに対して行われた変更を示します。 具体的には、40 epx グリッドへの配置です。

![Standard のコマンドの例](images/standarddensitycommanding.png)

## <a name="fluent-compact-sizing"></a>Fluent Compact サイズ

Compact サイズは、高密度で情報量の多いコントロールのグループに対応しており、次のような場合に役立ちます。

- 大量のコンテンツの閲覧。
- 1 ページに表示できるコンテンツの最大化。
- コントロールとコンテンツの移動および操作。

**Compact サイズは、主にポインター入力に対応するために設計されています。**

### <a name="examples"></a>例

Compact サイズは、アプリケーションにおいてページ レベルまたは特定のレイアウトで指定できる特殊なリソース ディクショナリによって実装されます。 リソース ディクショナリは、[WinUI](/uwp/toolkits/winui/) Nuget パッケージで使用できます。

次の例では、`Compact` スタイルをページおよび個々のグリッド コントロールに対して適用できる方法を示します。

#### <a name="page-level"></a>ページ レベル

```xaml
<Page.Resources>
    <ResourceDictionary Source="ms-appx:///Microsoft.UI.Xaml/DensityStyles/Compact.xaml" />
</Page.Resources>
```

#### <a name="grid-level"></a>グリッド レベル

```xaml
<Grid>
    <Grid.Resources>
        <ResourceDictionary Source="ms-appx:///Microsoft.UI.Xaml/DensityStyles/Compact.xaml" />
    </Grid.Resources>
</Grid>
```

## <a name="get-the-sample-code"></a>サンプル コードを入手する

- [XAML コントロール ギャラリー サンプル](https://github.com/Microsoft/Xaml-Controls-Gallery) - インタラクティブな形で XAML コントロールのすべてを参照できます。

## <a name="related-articles"></a>関連記事

- [タッチ ターゲットのガイドライン](../input/guidelines-for-targeting.md)
- [ResourceDictionary と XAML リソースの参照](../controls-and-patterns/resourcedictionary-and-xaml-resource-references.md)
- [リソース ディクショナリ](/uwp/api/windows.ui.xaml.resourcedictionary)
- [XAML スタイル](../controls-and-patterns/xaml-styles.md)
