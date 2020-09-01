---
description: XAML マークアップで x:DefaultBindMode 属性を使用して、OneTime 以外の x:Bind の既定のモードを指定する方法について説明します。
title: xDefaultBindMode 属性
ms.date: 02/08/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 817b89dc8f3ab7952cdbfc53489eb1afb12024f2
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89166876"
---
# <a name="xdefaultbindmode-attribute"></a>x:DefaultBindMode 属性

XAML マークアップで、x:Bind の既定のモードを指定します。

**x:DefaultBindMode** は、Windows 10 バージョン 1607 (Anniversary Update) SDK バージョン 14393 以降で使用できます。

## <a name="xaml-attribute-usage"></a>XAML 属性の使用方法

``` syntax
<object x:DefaultBindMode="OneTime \| OneWay \| TwoWay" .../>
```

## <a name="remarks"></a>注釈

[x:Bind](x-bind-markup-extension.md) の既定のモードは **OneTime** です。 これはパフォーマンス上の理由から選ばれました。**OneWay** を使うと、接続して変更検出を処理するために生成されるコードが多くなるためです。 マークアップ ツリーの特定のセグメントで x:Bind の既定のモードを変更するには、**x:DefaultBindMode** を使用できます。 指定されたモードは、バインドの一部として明示的にモードが指定されている場合を除いて、対象の要素とその子に対するすべての x:Bind 式に適用されます。

## <a name="related-topics"></a>関連トピック

* [x:Bind のマークアップ拡張機能](x-bind-markup-extension.md)
