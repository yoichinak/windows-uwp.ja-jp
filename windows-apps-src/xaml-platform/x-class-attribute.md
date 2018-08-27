---
author: jwmsft
description: マークアップとコード ビハインドの間で部分クラスを結合するための XAML コンパイルを設定します。 コードの部分クラスは個別のコード ファイルで定義され、マークアップ部分クラスは XAML コンパイル時のコード生成によって作成されます。
title: xClass 属性
ms.assetid: 40A7C036-133A-44DF-9D11-0D39232C948F
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.openlocfilehash: d3105e8ac345e1eb6f0d974f8ea29e741dae9f58
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.locfileid: "244536"
---
# <a name="xclass-attribute"></a>x:Class 属性

\[Windows 10 の UWP アプリ向けに更新。 Windows 8.x の記事については、[アーカイブ](http://go.microsoft.com/fwlink/p/?linkid=619132)をご覧ください。\]

マークアップとコード ビハインドの間で部分クラスを結合するための XAML コンパイルを設定します。 コードの部分クラスは、個別のコード ファイルで定義され、マークアップ部分クラスは XAML コンパイル時のコード生成によって作成されます。

## <a name="xaml-attribute-usage"></a>XAML 属性の使用方法


``` syntax
<object x:Class="namespace.classname"...>
  ...
</object>
```

## <a name="xaml-values"></a>XAML 値

| 用語 | 説明 |
|------|-------------|
| 名前空間 | 省略可能。 _classname_ で識別される部分クラスが含まれている名前空間を指定します。 _名前空間_を指定すると、_名前空間_と_クラス名_がドット (.) で区切られます。 _名前空間_を省略すると、_クラス名_には名前空間がないものと見なされます。 |
| classname | 必須。 ロードされた XAML とその XAML のコード ビハインドを結び付ける部分クラスの名前を指定します。 | 

## <a name="remarks"></a>注釈

**x:Class** は、XAML ファイル/オブジェクト ツリーのルートであり、ビルド アクションによってコンパイルされる任意の要素か、コンパイルされたアプリケーションのアプリケーション定義中の [**Application**](https://msdn.microsoft.com/library/windows/apps/br242324) ルートの属性として宣言できます。 ルート ノード以外の任意の要素に対して **x:Class** を宣言した場合、XAML ファイルが **Page** ビルド動作でコンパイルされていない任意の状況下で、コンパイル時エラーになります。

**x:Class** として使われたクラスは、ネストされたクラスにすることはできません。

**x:Class** 属性の値は、クラスの完全修飾名を指定する文字列であることが必要です。 コード ビハインドに名前空間情報がない場合に限り、名前空間情報を省略できます (クラス定義はクラス レベルで開始されます) ページまたはアプリケーション定義のコード ビハインド ファイルは、プロジェクトの一部として含まれているコード ファイルの中にあることが必要です。 コード ビハインド クラスはパブリックであることが必要です。 コード ビハインド クラスは部分的であることが必要です。

## <a name="clr-language-rules"></a>CLR 言語規則

コード ビハインド ファイルとしては C++ ファイルが使用可能ですが、XAML 構文に違いが出ないようにするため、CLR の言語形式に従う特定の構文があります。 特に、任意の **x:Class** 値の名前空間とクラス名の間の区切り文字は、XAML に関連付けられた C++ コード ファイルの名前空間とクラス名の間の区切り文字が "::" であっても、常にドット (".") になります。 **x:Class** 値の *namespace* 部分を指定する場合、C++ で入れ子になった名前空間を宣言すると、以降の入れ子になった名前空間の文字列間の区切り文字も "::" ではなく "." である必要があります。
