---
description: カスタム リソース検索の実装から取得されたリソースの参照を評価して、任意の XAML 属性の値を提供します。 リソース検索は、CustomXamlResourceLoader クラスの実装によって実行されます。
title: CustomResource マークアップ拡張
ms.assetid: 3A59A8DE-E805-4F04-B9D9-A91E053F3642
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 094a2a957493787acef8e97b49b61778fc1ec42f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155106"
---
# <a name="customresource-markup-extension"></a>{CustomResource} マークアップ拡張


カスタム リソース検索の実装から取得されたリソースの参照を評価して、任意の XAML 属性の値を提供します。 リソース参照は、 [**Customxamlresourceloader**](/uwp/api/Windows.UI.Xaml.Resources.CustomXamlResourceLoader) クラスの実装によって実行されます。

## <a name="xaml-attribute-usage"></a>XAML 属性の使用方法

``` syntax
<object property="{CustomResource key}" .../>
```

## <a name="xaml-values"></a>XAML 値

| 期間 | 説明 |
|------|-------------|
| key | 要求されたリソースのキー。 キーが最初にどのように割り当てられるかは、現在使用が登録されている [**CustomXamlResourceLoader**](/uwp/api/Windows.UI.Xaml.Resources.CustomXamlResourceLoader) クラスの実装に固有のものです。 |

## <a name="remarks"></a>注釈

**CustomResource** は、カスタム リソース リポジトリのどこかで定義されている値を取得するための手法です。 この手法は比較的高度なものであり、Windows ランタイム アプリのほとんどのシナリオでは使われていません。

**CustomResource** がどのようにリソース辞書に解決されるかについては、[**CustomXamlResourceLoader**](/uwp/api/Windows.UI.Xaml.Resources.CustomXamlResourceLoader) の実装方法によって異なるため、このトピックでは説明しません。

[**CustomXamlResourceLoader**](/uwp/api/Windows.UI.Xaml.Resources.CustomXamlResourceLoader) 実装の [**GetResource**](/uwp/api/windows.ui.xaml.resources.customxamlresourceloader.getresource) メソッドは、Windows ランタイム XAML パーサーがマークアップ内で `{CustomResource}` の使用を検出するたびに呼び出されます。 **GetResource** に渡される *resourceId* は *key* 引数から与えられ、他の入力パラメーターはコンテキスト (たとえば、使用が適用されたプロパティ) から与えられます。

`{CustomResource}` の使用は既定では機能しません ([**GetResource**](/uwp/api/windows.ui.xaml.resources.customxamlresourceloader.getresource) の基本実装が不完全です)。 `{CustomResource}` の有効な参照を行うには、次の各手順を実行する必要があります。

1.  [**CustomXamlResourceLoader**](/uwp/api/Windows.UI.Xaml.Resources.CustomXamlResourceLoader) からカスタム クラスを派生し、[**GetResource**](/uwp/api/windows.ui.xaml.resources.customxamlresourceloader.getresource) メソッドをオーバーライドします。 実装で基本メソッドを呼び出さないでください。
2.  初期化ロジックでクラスを参照するために [**CustomXamlResourceLoader.Current**](/uwp/api/windows.ui.xaml.resources.customxamlresourceloader.current) を設定します。 これは、`{CustomResource}` 拡張の使用が含まれるページ レベルの XAML が読み込まれる前に行う必要があります。 **CustomXamlResourceLoader.Current** を設定する場所の 1 つは、App.xaml コード ビハインド テンプレートで生成される [**Application**](/uwp/api/Windows.UI.Xaml.Application) サブクラス コンストラクター内です。
3.  これで、アプリでページとして読み込む XAML 内で、XAML リソース ディクショナリ内から、`{CustomResource}` 拡張を使うことができます。

**CustomResource** はマークアップ拡張です。 一般にマークアップ拡張機能を実装するのは、属性値をリテラル値やハンドラー名以外にエスケープする要件が存在し、その要件の適用範囲がグローバルで、特定の型やプロパティに型コンバーターを適用するだけにとどまらない場合です。 XAML のすべてのマークアップ拡張 \{ は、属性構文で "" と "" の文字を使用します \} 。これは、マークアップ拡張機能が属性を処理する必要があることを xaml プロセッサが認識する規則です。

## <a name="related-topics"></a>関連トピック

* [ResourceDictionary と XAML リソースの参照](../design/controls-and-patterns/resourcedictionary-and-xaml-resource-references.md)
* [**CustomXamlResourceLoader**](/uwp/api/Windows.UI.Xaml.Resources.CustomXamlResourceLoader)
* [**GetResource**](/uwp/api/windows.ui.xaml.resources.customxamlresourceloader.getresource)