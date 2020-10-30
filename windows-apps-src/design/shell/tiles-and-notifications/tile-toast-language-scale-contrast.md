---
description: タイルやトーストには、表示言語や、表示倍率、ハイ コントラストなど、実行時のコンテキストに合わせた文字列や画像を読み込むことができます。
title: 言語、スケール、ハイ コントラストに合わせたタイルとトースト通知のサポート
template: detail.hbs
ms.date: 10/12/2017
ms.topic: article
keywords: Windows 10, UWP, リソース, 画像, アセット, MRT, 修飾子
ms.localizationpriority: medium
ms.openlocfilehash: 0048da25cd1e775391c2523e37cb936243b7308c
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2020
ms.locfileid: "93033115"
---
# <a name="tile-and-toast-notification-support-for-language-scale-and-high-contrast"></a>言語、スケール、ハイ コントラストに合わせたタイルとトースト通知のサポート

タイルとトーストでは、表示言語、 [スケールファクター](../../layout/screen-sizes-and-breakpoints-for-responsive-design.md)、ハイコントラスト、およびその他のランタイムコンテキストに合わせて調整された文字列とイメージを読み込むことができます。 リソースファイルの名前に修飾子を使用する方法の背景については、「 [言語、スケール、およびその他の修飾子](../../../app-resources/tailor-resources-lang-scale-contrast.md) と [アプリのアイコンとロゴ](../../style/app-icons-and-logos.md)用にリソースを調整する」を参照してください。

アプリのローカライズの価値提案の詳細については、「[グローバリゼーションとローカライズ](../../globalizing/globalizing-portal.md)」をご覧ください。

## <a name="refer-to-a-string-resource-from-a-template"></a>テンプレートから文字列リソースを参照する

タイルまたはトースト テンプレートで、`ms-resource` という URI (Uniform Resource Identifier) スキームに単純な文字列リソース識別子を続けて指定することにより、文字列リソースを参照できます。 たとえば、"Farewell" という名前のリソース エントリが含まれた Resources.resx ファイルがあれば、"Farewell" という識別子の文字列リソースがあります。 文字列リソース識別子とリソース ファイル (.resw) について詳しくは、「[UI とアプリ パッケージ マニフェスト内の文字列をローカライズする](../../../app-resources/localize-strings-ui-manifest.md)」をご覧ください。

以下は、`ms-resource` を使用して "Farewell" 文字列リソース識別子への参照によってテンプレート コンテンツの[テキスト](/uwp/schemas/tiles/tilesschema/element-text?branch=live)本文を参照する方法を示しています。

```xml
<text id="1">ms-resource:Farewell</text>
```

`ms-resource` URI スキームを省略した場合、テキスト本文は単なる文字列リテラルとなり、  識別子への参照にはなりません。

```xml
<text id="1">Farewell</text>
```

## <a name="refer-to-an-image-resource-from-a-template"></a>テンプレートから画像リソースを参照する

タイルまたはトースト テンプレートで、`ms-appx` という URI (Uniform Resource Identifier) スキームに画像リソースの名前を続けて指定することにより、画像リソースを参照できます。 これは、XAML マークアップで画像リソースを参照する場合と同じ方法です (詳しくは、「[XAML マークアップとコードから画像やその他のアセットを参照する](../../../app-resources/images-tailored-for-scale-theme-contrast.md#reference-an-image-or-other-asset-from-xaml-markup-and-code)」をご覧ください)。

たとえば、フォルダーに次のような名前を付けたとします。

```
\Assets\Images\contrast-standard\welcome.png
\Assets\Images\contrast-high\welcome.png
```

この場合の画像リソースは 1 つで、名前 (絶対パス) は `/Assets/Images/welcome.png` です。 この名前をテンプレートで使用する方法は次のとおりです。

```xml
<image id="1" src="ms-appx:///Assets/Images/welcome.png"/>
```

この例の URI で、スキーム ("`ms-appx`") の後に "`://`" が続き、その後に絶対パスが続くことに注意してください (絶対パスは "`/`" から始まる部分です)。

## <a name="hosting-and-loading-images-in-the-cloud"></a>クラウド内の画像のホスティングと読み込み

URI スキーム `ms-resource` および `ms-appx` が自動で修飾子の照合を実行して、現在のコンテキストに最も適切なリソースを見つけます。 Web URI スキーム (`http`、`https`、`ftp` など) では、このような自動的な照合が実行されません。

代わりとして、画像の URI には、要求された修飾子の 1 つ以上の値を記述したクエリ文字列を追加します。

```xml
<image id="1" src="http://www.contoso.com/Assets/Images/welcome.png?ms-lang=en-US"/>
```

次に、画像を提供するアプリ サービスに、どの画像を返すか決定するためのクエリ文字列を調べて使用する HTTP ハンドラーを実装します。

また、 [タイル](/uwp/schemas/tiles/tilesschema/schema-root?branch=live)または [トースト](/uwp/schemas/tiles/toastschema/schema-root?branch=live)通知の XML ペイロードで [**addImageQuery**](/uwp/schemas/tiles/tilesschema/element-visual?branch=live) 属性を `true` に設定する必要があります。 **AddImageQuery** 属性は、 `visual` `binding` `image` タイルスキーマとトーストスキーマの両方の、、およびの各要素に表示されます。 要素に **addImageQuery** を明示的に設定すると、祖先に設定された値が上書きされます。 たとえば、`image` 要素の **addImageQuery** の値が `true` であれば、その親の `binding` 要素の **addImageQuery** の `false` が上書きされます。

使用できるクエリ文字列は次のとおりです。

| 修飾子 | クエリ文字列 | 例 |
| --------- | ------------ | ------- |
| スケール | ms-scale | ?ms-scale=400 |
| Language | ms-lang | ?ms-lang=en-US |
| この例を、 | ms-contrast | ?ms-contrast=high |

クエリ文字列で使用可能な修飾子の値を網羅したリファレンス テーブルについては、「[ResourceContext.QualifierValues](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues)」をご覧ください。

## <a name="important-apis"></a>重要な API

* [ResourceContext.QualifierValues](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues)

## <a name="related-topics"></a>関連トピック

* [レスポンシブ デザインの画面サイズと改行ポイント](../../layout/screen-sizes-and-breakpoints-for-responsive-design.md)
* [言語、スケール、その他の修飾子用にリソースを調整する](../../../app-resources/tailor-resources-lang-scale-contrast.md)
* [タイルとアイコン資産のガイドライン](../../style/app-icons-and-logos.md)。
* [グローバライズとローカライズ](../../globalizing/globalizing-portal.md)
* [UI とアプリ パッケージ マニフェスト内の文字列をローカライズする](../../../app-resources/localize-strings-ui-manifest.md)
* [XAML マークアップとコードから画像やその他のアセットを参照する](../../../app-resources/images-tailored-for-scale-theme-contrast.md)
* [addImageQuery](/uwp/schemas/tiles/tilesschema/element-visual?branch=live)
* [タイルのスキーマ](/uwp/schemas/tiles/tilesschema/schema-root?branch=live)
* [トースト スキーマ](/uwp/schemas/tiles/toastschema/schema-root?branch=live)
