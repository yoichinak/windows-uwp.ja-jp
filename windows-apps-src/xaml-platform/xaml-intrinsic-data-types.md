---
description: Windows ランタイムの XAML について、共通言語ランタイム (CLR) や、C++ のような他のプログラミング言語での特定のデータ型に対する言語レベルのサポートの一覧を示します。
title: XAML 固有のデータ型
ms.assetid: D50E6127-395D-4E27-BAA2-2FE627F4B711
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: dd8bd8f80be068af92d679795431cf9a938fd5f6
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372950"
---
# <a name="xaml-intrinsic-data-types"></a>XAML 固有のデータ型


Windows ランタイムの XAML は、共通言語ランタイム (CLR) や、C++ のような他のプログラミング言語で頻繁に使われるプリミティブである複数のデータ型に対して、言語レベルのサポートを提供します。

XAML 固有のデータ型が最も一般的に使われるのは、リソースが XAML リソース ディクショナリで定義されている場合です。 さまざまな値として使う数など、定数も定義できます。 文字列またはブール値によるストーリーボードに設定されたアニメーションも使えます。その場合は、[**ObjectAnimationUsingKeyFrames**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.ObjectAnimationUsingKeyFrames) 定義のキーフレームを設定するための文字列またはブール値を表す XAML オブジェクト要素が必要です。 Windows ランタイムの既定の XAML テンプレートでは、この両方の手法が使われます。

Windows ランタイムの XAML は、次の型に対する言語レベルのサポートを提供します。

| XAML プリミティブ | 説明 |
|-------|-------------|
| **x: ブール値**  | CLR のサポートについては、[**Boolean**](https://docs.microsoft.com/dotnet/api/system.boolean?redirectedfrom=MSDN) と同じです。 XAML は、**x:Boolean** の値の大文字と小文字を区別せずに解析します。 "x:Bool" は、承諾済みの代替プリミティブではありません。 |
| **x:String**   | CLR のサポートについては、[**String**](https://docs.microsoft.com/dotnet/api/system.string?redirectedfrom=MSDN) と同じです。 既定では、文字列型のエンコーディングは XML による全体的なエンコーディングです。 |
| **x: Double**   | CLR のサポートについては、[**Double**](https://docs.microsoft.com/dotnet/api/system.double?redirectedfrom=MSDN) と同じです。 数値に加え、**x:Double** のテキスト構文は、"NaN" トークンも許可しています。これにより、レイアウト動作の "Auto" をリソース値として格納することができます。 トークンでは、大文字と小文字が区別されます。 `1,000,000` に "1+E06" のように、科学的記数法を使えます。 |
| **x:Int32**    | CLR のサポートについては、[**Int32**](https://docs.microsoft.com/dotnet/api/system.int32?redirectedfrom=MSDN) と同じです。 **x:Int32** は符号付きとして処理され、負の整数の場合は負の符号 ("-") を含めることができます。 XAML では、テキスト構文に符号がない場合、暗黙的に正符号付きの値を示します。 |

これらの XAML 言語プリミティブは、通常、XAML で **x:** プレフィックスを使うオブジェクト要素を定義する場合のみ使われます。 他のすべての XAML 言語機能は、一般に属性形式で、またはマークアップ拡張として使われます。

**注**  x「:」プレフィックスを持つ規則により、XAML とその他のすべての XAML 言語要素の言語プリミティブが示すようにします。 これは、XAML 言語要素が実際のマークアップでいかによく使われているかを表しています。 この規則は、XAML の説明書や XAML の仕様に従っています。

## <a name="other-xaml-primitives"></a>その他の XAML プリミティブ

XAML 2009 の仕様には、**x:Uri**、**x:Single** など、他の XAML 言語レベルのプリミティブに関する情報が記載されています。 このトピックの表に示されていない場合、他の XAML ボキャブラリまたは XAML 2009 の仕様で定義されている他の XAML 言語プリミティブは、現在 Windows ランタイムの XAML ではサポートされていません。

**注**  日付と時刻 (を使用するプロパティ[ **DateTime** ](https://docs.microsoft.com/uwp/api/Windows.Foundation.DateTime)または[ **DateTimeOffset**](https://docs.microsoft.com/dotnet/api/system.datetimeoffset?redirectedfrom=MSDN)、 [**TimeSpan** ](https://docs.microsoft.com/uwp/api/Windows.Foundation.TimeSpan)または[ **System.TimeSpan**](https://docs.microsoft.com/dotnet/api/system.timespan?redirectedfrom=MSDN)) XAML プリミティブで設定可能ではありません。 これらのプロパティは一般に、XAML ではまったく設定できません。これは、Windows ランタイム XAML パーサーには、日付と時刻に対する既定の from 文字列変換の動作がないためです。 すべての日付と時刻のプロパティの初期化値には、ページまたは要素を読み込むときに実行されるコード ビハインドを使う必要があります。

## <a name="related-topics"></a>関連トピック

* [XAML の概要](xaml-overview.md)
* [XAML 構文のガイド](xaml-syntax-guide.md)
* [アニメーションの再検討](https://docs.microsoft.com/windows/uwp/graphics/storyboarded-animations)
 

