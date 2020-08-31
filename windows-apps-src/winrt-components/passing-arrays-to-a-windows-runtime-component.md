---
title: Windows ランタイム コンポーネントに配列を渡す
description: Windows ユニバーサル プラットフォーム (UWP) では、パラメーターは入力または出力のどちらかに使用され、両方に使用されることはありません。 つまり、メソッドに渡される配列の内容および配列自体は、入力か出力のどちらかに使用されます。
ms.assetid: 8DE695AC-CEF2-438C-8F94-FB783EE18EB9
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f38538d1a77bdeb6a497eda5802f712f23002b54
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155186"
---
# <a name="passing-arrays-to-a-windows-runtime-component"></a>Windows ランタイム コンポーネントに配列を渡す




Windows ユニバーサル プラットフォーム (UWP) では、パラメーターは入力または出力のどちらかに使用され、両方に使用されることはありません。 つまり、メソッドに渡される配列の内容および配列自体は、入力か出力のどちらかに使用されます。 配列の内容が入力に使用される場合、メソッドは配列から読み取りを行いますが、書き込みはしません。 配列の内容が出力に使用される場合、メソッドは配列に書き込みを行いますが、読み取りはしません。 .NET の配列は参照型であり、配列の参照が値によって渡された場合でも配列の内容が変更可能であるため、配列パラメーターには問題があります (Visual Basic では**ByVal** )。 [Windows ランタイム メタデータのエクスポート ツール (Winmdexp.exe)](/dotnet/framework/tools/winmdexp-exe-windows-runtime-metadata-export-tool) では、コンテキストから判別できない場合、パラメーターに ReadOnlyArrayAttribute 属性または WriteOnlyArrayAttribute 属性を適用して、配列の用途を指定する必要があります。 配列の使用方法は、次のように決定されます。

-   戻り値、または出力パラメーター (Visual Basic では、[OutAttribute](/dotnet/api/system.runtime.interopservices.outattribute) 属性の **ByRef** パラメーター) の場合、配列は常に出力に使用されます。 ReadOnlyArrayAttribute 属性は適用しないでください。 出力パラメーターで WriteOnlyArrayAttribute 属性を適用することはできますが、冗長になります。

    > **注意**   Visual Basic コンパイラでは、出力のみの規則は適用されません。 出力パラメーターからの読み取りは行わないでください。**Nothing** が含まれている可能性があります。 常に新しい配列を割り当ててください。
 
-   **ref** 修飾子 (Visual Basic では **ByRef**) を持つパラメーターは使用できません。 Winmdexp.exe によりエラーが生成されます。
-   値で渡されるパラメーターの場合、[ReadOnlyArrayAttribute](/dotnet/api/system.runtime.interopservices.windowsruntime.readonlyarrayattribute) 属性または [WriteOnlyArrayAttribute](/dotnet/api/system.runtime.interopservices.windowsruntime.writeonlyarrayattribute) 属性を適用して、配列の内容が入力と出力のどちらで使用されるのかを指定する必要があります。 両方の属性を指定すると、エラーになります。

メソッドで、入力の配列を受け取り、配列の内容を変更して、呼び出し元に配列を返す必要がある場合、入力には読み取り専用のパラメーター、出力には書き込み専用のパラメーター (または戻り値) を使用します。 次のコードは、このパターンを実装する 1 つの方法を示します。

> [!div class="tabbedCodeSnippets"]
> ```csharp
> public int[] ChangeArray([ReadOnlyArray()] int[] input)
> {
>     int[] output = input.Clone();
>     // Manipulate the copy.
>     //   ...
>     return output;
> }
> ```
> ```vb
> Public Function ChangeArray(<ReadOnlyArray> input() As Integer) As Integer()
>     Dim output() As Integer = input.Clone()
>     ' Manipulate the copy.
>     '   ...
>     Return output
> End Function
> ```

すぐに入力の配列をコピーして、利用することをお勧めします。 これにより、コンポーネントが .NET コードによって呼び出されたかどうかにかかわらず、メソッドの動作が同じになります。

## <a name="using-components-from-managed-and-unmanaged-code"></a>マネージ コードおよびアンマネージ コードからコンポーネントを使用する


ReadOnlyArrayAttribute 属性または WriteOnlyArrayAttribute 属性を持つパラメーターは、呼び出し元がネイティブ コードで記述されているか、マネージ コードで記述されているかによって、動作が異なります。 呼び出し元が、ネイティブ コード (JavaScript、または Visual C++ コンポーネント拡張機能) の場合、配列の内容は次のように処理されます。

-   ReadOnlyArrayAttribute: アプリケーション バイナリ インターフェイス (ABI) の境界を越えた呼び出しの場合、配列はコピーされます。 要素は必要に応じて変換されます。 そのため、誤ってメソッドによって入力専用配列に変更が加えられたとしても、呼び出し元からは見えません。
-   WriteOnlyArrayAttribute: 呼び出されたメソッドでは、元の配列の内容を推測できません。 たとえば、メソッドが受け取る配列が初期化されていなかったり、既定の値が入っていたりする可能性があります。 メソッドには、配列内のすべての要素の値を設定することが期待されます。

呼び出し元がマネージコードの場合は、.NET のメソッド呼び出しの場合と同様に、呼び出されたメソッドで元の配列を使用できます。 配列の内容は .NET コードで変更可能であるため、メソッドが配列に対して行う変更は呼び出し元から参照できます。 これは、Windows ランタイム コンポーネント用に作成された単体テストに影響するため、注意してください。 マネージド コードでテストが作成される場合、配列の内容はテスト中に変更可能であることが示されます。

## <a name="related-topics"></a>関連トピック

* [ReadOnlyArrayAttribute](/dotnet/api/system.runtime.interopservices.windowsruntime.readonlyarrayattribute)
* [WriteOnlyArrayAttribute](/dotnet/api/system.runtime.interopservices.windowsruntime.writeonlyarrayattribute)
* [C# および Visual Basic を使用した Windows ランタイム コンポーネント](creating-windows-runtime-components-in-csharp-and-visual-basic.md)