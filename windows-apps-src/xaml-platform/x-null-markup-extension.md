---
description: XAML マークアップで、プロパティに null 値を指定します。
title: xNull マークアップ拡張
ms.assetid: E6A4038E-4ADA-4E82-9824-582FC16AB037
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 0589249c65d301e3e74b305b92842fe4d3f61929
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372285"
---
# <a name="xnull-markup-extension"></a>{x:Null} マークアップ拡張


XAML マークアップで、プロパティに **null** 値を指定します。

## <a name="xaml-attribute-usage"></a>XAML 属性の使用方法

``` syntax
<object property="{x:Null}" .../>
```

## <a name="remarks"></a>注釈

**null** は、C# と C++ の null 参照キーワードです。 Microsoft Visual Basic の null 参照キーワードは **Nothing** です。

初期の既定値は、依存関係プロパティごとに異なる場合があり、必ずしも **null** というわけではありません。 また、依存関係プロパティの多くは、その内部実装により、(マークアップまたはコードのいずれによる場合でも) **null** を値として受け入れません。 このような場合、 **{x:Null}** で XAML 属性値を設定すると、パーサー例外が発生することがあります。

一部の Windows ランタイム型では、null を使うことができます。 null 許容型に **null** が既定値として設定されていない場合に備え、 **{x:Null}** を使って XAML 内で **null** 値に設定することができます。 Visual C コンポーネント拡張機能を使用する場合 (C +/cli CX)、null 許容型として表されます[ **platform::ibox<T>** ](https://docs.microsoft.com/cpp/cppcx/platform-ibox-interface)します。 Microsoft .NET 言語を使う場合、null 許容型は [**Nullable<T>** ](https://docs.microsoft.com/dotnet/api/system.nullable-1?redirectedfrom=MSDN) として表されます。

## <a name="related-topics"></a>関連トピック

* [**Null 許容型<T>** ](https://docs.microsoft.com/dotnet/api/system.nullable-1?redirectedfrom=MSDN)
* [**IReference<T>** ](https://docs.microsoft.com/uwp/api/Windows.Foundation.IReference_T_)
 

