---
description: XAML マークアップで x:Null マークアップ拡張機能を使用して、プロパティに Null 値を指定する方法について説明します。
title: xNull マークアップ拡張
ms.assetid: E6A4038E-4ADA-4E82-9824-582FC16AB037
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 46256a5456583f9e434f76a2d06bc552c2cb0d3f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89169036"
---
# <a name="xnull-markup-extension"></a>{x:Null} マークアップ拡張


XAML マークアップで、プロパティに **null** 値を指定します。

## <a name="xaml-attribute-usage"></a>XAML 属性の使用方法

``` syntax
<object property="{x:Null}" .../>
```

## <a name="remarks"></a>注釈

**null** は、C# と C++ の null 参照キーワードです。 Microsoft Visual Basic の null 参照キーワードは **Nothing** です。

初期の既定値は、依存関係プロパティごとに異なる場合があり、必ずしも **null** というわけではありません。 また、依存関係プロパティの多くは、その内部実装により、(マークアップまたはコードのいずれによる場合でも) **null** を値として受け入れません。 このような場合、**{x:Null}** で XAML 属性値を設定すると、パーサー例外が発生することがあります。

一部の Windows ランタイム型では、null を使うことができます。 null 許容型に **null** が既定値として設定されていない場合に備え、**{x:Null}** を使って XAML 内で **null** 値に設定することができます。 Visual C++ コンポーネント拡張機能 (C++/CX) を使う場合、null 許容型は [**Platform::IBox<T>**](/cpp/cppcx/platform-ibox-interface) として表されます。 Microsoft .NET 言語を使う場合、null 許容型は [**Nullable<T>**](/dotnet/api/system.nullable-1) として表されます。

## <a name="related-topics"></a>関連トピック

* [**Nullable<T>**](/dotnet/api/system.nullable-1)
* [**IReference<T>**](/uwp/api/Windows.Foundation.IReference_T_)
 