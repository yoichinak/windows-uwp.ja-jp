---
description: 作成したコンポーネントの C#/WinRT 診断エラーメッセージ
title: C#/WinRT コンポーネントエラーの診断
ms.date: 01/27/2021
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 8d1e5eda34a8cbce70edc812ba27437090393ca0
ms.sourcegitcommit: 457239a6907198bc2948322a07c484dd8a170b33
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/01/2021
ms.locfileid: "99230020"
---
# <a name="diagnose-cwinrt-component-errors"></a>C#/WinRT コンポーネントエラーの診断

この記事では、C#/Winrtで記述された Windows ランタイムコンポーネントの制限事項に関する追加情報を提供します。 作成者がコンポーネントをビルドするときに、C#/WinRT からのエラーメッセージで提供される情報を拡張します。 既存の UWP .NET Native マネージコンポーネントでは、.NET ツール [Winmdexp.exe](/dotnet/framework/tools/winmdexp-exe-windows-runtime-metadata-export-tool)を使用して C# WinRT コンポーネントのメタデータが生成されます。 Windows ランタイムサポートが .NET から切り離されたので、C#/WinRT はコンポーネントから winmd ファイルを生成するツールを提供します。 Windows ランタイムのコードには C# クラスライブラリよりも多くの制約があり、c#/WinRT 診断スキャナーは、winmd ファイルを生成する前にそれらのことを警告します。

この記事では、C#/Winrtからのビルドで報告されたエラーについて説明します。 この記事は、Winmdexp.exe ツールを使用する [既存の UWP .NET Native マネージコンポーネント](../winrt-components/creating-windows-runtime-components-in-csharp-and-visual-basic.md) の制限事項に関する情報の更新バージョンとして機能します。

エラーメッセージのテキスト (プレースホルダーの特定の値を省略します) またはメッセージ番号を検索します。 ここで必要な情報が見つからない場合は、この記事の最後にある [フィードバック] ボタンを使用して、ドキュメントの品質向上にご協力ください。 フィードバックには、エラーメッセージを含めてください。 または、 [C#/WinRT リポジトリ](https://github.com/microsoft/CsWinRT)でバグを報告することもできます。

この記事では、シナリオごとにエラーメッセージを整理します。

## <a name="implementing-interfaces-that-arent-valid-windows-runtime-interfaces"></a>Windows ランタイムインターフェイスではないインターフェイスの実装

C#/WinRT コンポーネントでは、非同期アクションまたは操作を表す Windows ランタイムインターフェイス (**iasyncaction**、 **\<TProgress\> iasyncactionwithprogress**、 **IAsyncOperation \<TResult\>**、または **IAsyncOperationWithProgress \<TResult,TProgress\>**) など、特定の Windows ランタイムインターフェイスを実装することはできません。 代わりに、Windows ランタイムコンポーネントで非同期操作を生成するために、 **Asyncinfo** クラスを使用します。 注: これらは無効なインターフェイスではありません。たとえば、クラスが **system.exception** を実装することはできません。

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th><p>エラー番号</p></th>
<th><p>[メッセージ テキスト]</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>CsWinRT1008</p></td>
<td><p>{0} {1} インターフェイスが有効な Windows ランタイムインターフェイスではないため、Windows ランタイムコンポーネントの型はインターフェイスを実装できません</p></td>
</tr>
</tbody>
</table>

## <a name="attribute-related-errors"></a>属性関連のエラー

Windows ランタイムでは、オーバーロードされたメソッドは、1つのオーバーロードが既定のオーバーロードとして指定されている場合にのみ、同じ数のパラメーターを持つことができます。 属性 **DefaultOverload** (CsWinRT1015, 1016) を使用します。

配列を関数またはプロパティの入力または出力として使用する場合は、読み取り専用または書き込み専用 (CsWinRT 1025) のいずれかである必要があります。 属性 **WindowsRuntime と InteropServices** は、使用するために用意されています。 ReadOnlyArray と InteropServices. WindowsRuntime.  。 指定された属性は、配列型 (CsWinRT1026) のパラメーターでのみ使用され、パラメーター (CsWinRT1023) ごとに適用する必要があるのは1つだけです。

マーク **アウト** された配列パラメーターには書き込み専用と見なされるため、属性を適用する必要はありません。 この場合、読み取り専用として装飾すると、エラーメッセージが表示されます (CsWinRT1024)。

属性 **InteropServices. InAttribute** と **InteropServices** は、任意の型 (CsWinRT1021, 1022) のパラメーターでは使用できませんでした。

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th><p>エラー番号</p></th>
<th><p>[メッセージ テキスト]</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>CsWinRT1015</p></td>
<td><p>クラス {2} : {0} ' ' の複数パラメーターのオーバーロード {1} は、windows.foundation.metadata.defaultoverloadattribute で修飾されています。 属性は、メソッドの1つのオーバーロードにのみ適用できます。</p></td>
</tr>
<tr class="even">
<td><p>CsWinRT1016</p></td>
<td><p>クラスの {2} 場合: {0} のパラメーターのオーバーロードには、 {1} windows.foundation.metadata.defaultoverloadattribute 属性で修飾することによって、既定のオーバーロードとして指定されたメソッドが1つだけ必要です。</p></td>
</tr>
<tr class="odd">
<td><p>CsWinRT1021</p></td>
<td><p>メソッド ' {0} ' には、配列であるパラメーター ' ' が含まれています。このパラメーターには、 {1} InteropServices. InAttribute または InteropServices が含まれています。 Windows ランタイムでは、配列パラメーターに ReadOnlyArray または WriteOnlyArray のいずれかを指定する必要があります。 これらの属性を削除するか、必要に応じて、適切な Windows ランタイム属性と置き換えてください。</p></td>
</tr>
<tr class="even">
<td><p>CsWinRT1022</p></td>
<td><p>メソッド ' {0} ' には {1} 、InAttribute または InteropServices を持つパラメーター ' ' が指定されていますが、InteropServices または InAttribute でのパラメーターのマーキングはサポートしていません。 Windows ランタイム InteropServices。 System.Runtime.InteropServices.InAttribute を削除して、System.Runtime.InteropServices.OutAttribute を 'out' 修飾子と置き換えることを検討してください。</p></td>
</tr>
<tr class="odd">
<td><p>CsWinRT1023</p></td>
<td><p>メソッド ' {0} ' には、 {1} 配列で、ReadOnlyArray と writeonlyarray の両方を持つパラメーター ' ' が指定されています。 Windows ランタイムでは、配列パラメーターの内容は、読み取り可能または書き込み可能である必要があります。 属性の 1 つを '{1}' から削除してください。</p></td>
</tr>
<tr class="even">
<td><p>CsWinRT1024</p></td>
<td><p>メソッド ' {0} ' には、 {1} 配列である、ReadOnlyArray 属性を持つ出力パラメーター ' ' が指定されています。 Windows ランタイムでは、出力配列の内容は書き込み可能です。  '{1}' から属性を削除してください。</p></td>
</tr>
<tr class="even">
<td><p>CsWinRT1025</p></td>
<td><p>メソッド ' {0} ' には {1} 、配列であるパラメーター ' ' が指定されています。 Windows ランタイムでは、配列パラメーターの内容が読み取り可能または書き込み可能である必要があります。 ReadOnlyArray または WriteOnlyArray を ' ' に適用してください {1} 。</p></td>
</tr>
<tr class="even">
<td><p>CsWinRT1026</p></td>
<td><p>メソッド ' {0} ' には、 {1} 配列ではなく、ReadOnlyArray attribute または WriteOnlyArray 属性を持つパラメーター ' ' が指定されています。 Windows ランタイムは、ReadOnlyArray または WriteOnlyArray で配列以外のパラメーターをマークすることをサポートしていません。 "</p></td>
</tr>
</tbody>
</table>

## <a name="namespace-errors-and-invalid-names-for-the-output-file"></a>出力ファイルの名前空間のエラーと無効な名前

Windows ランタイムでは、Windows メタデータ (winmd) ファイル内のすべてのパブリック型は、winmd ファイル名を共有する名前空間、またはファイル名のサブ名前空間にある必要があります。 たとえば、Visual Studio プロジェクトに A. B という名前が付けられている場合 (つまり、Windows ランタイムコンポーネントが. B. winmd)、パブリッククラス A. Class3 と D.class4 を含めることはできますが、.......

> [!NOTE]
> これらの制限はパブリック型だけに適用され、実装で使用されるプライベート型には適用されません。

Class3 の場合は、Class3 を別の名前空間に移動するか、または Windows ランタイムコンポーネントの名前を winmd に変更することができます。 前の例では、A.B.winmd を呼び出すコードは A.Class3 を特定することはできません。

D.class4 の場合、ファイル名に D.class4 とクラスの両方を含めることはできません。そのため、Windows ランタイムコンポーネントの名前を変更することはできません。 D.class4 を別の名前空間に移動するか、別の Windows ランタイムコンポーネントに配置することができます。

ファイルシステムは大文字と小文字を区別できないため、大文字と小文字が異なる名前空間は許可されません (CsWinRT1002)。

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th><p>エラー番号</p></th>
<th><p>[メッセージ テキスト]</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>CsWinRT1001</p></td>
<td><p>パブリック型には、 {1} 他の名前空間 (' ') と共通プレフィックスを共有しない名前空間 (' ') があり {0} ます。 Windows メタデータ ファイル内のすべての型は、ファイル名で指定される名前空間のサブ名前空間に存在する必要があります。</p></td>
</tr>
<tr class="even">
<td><p>CsWinRT1002</p></td>
<td><p>' ' という名前の複数の名前空間が見つかりました。 {0} Windows ランタイムでは、名前空間の名前のみが異なる場合があります。</p></td>
</tr>
</tbody>
</table>

## <a name="exporting-types-that-arent-valid-windows-runtime-types"></a>無効な Windows ランタイム型である型をエクスポートする

コンポーネントのパブリックインターフェイスは、Windows ランタイム型のみを公開する必要があります。 ただし、.net では、.NET と Windows ランタイムで若干異なる、よく使用されるさまざまな型のマッピングが用意されています。 これにより、.NET 開発者は、新しい型を学習するのではなく、使い慣れた型を使用できるようになります。 マップされた .NET Framework 型は、コンポーネントのパブリック インターフェイスで使うことができます。 詳細については、「 [Windows ランタイムコンポーネントでの型の宣言](../winrt-components/creating-windows-runtime-components-in-csharp-and-visual-basic.md#declaring-types-in-windows-runtime-components)」、「 [Windows ランタイム型をマネージコードに渡す](../winrt-components/creating-windows-runtime-components-in-csharp-and-visual-basic.md#passing-windows-runtime-types-to-managed-code)」、および「 [Windows ランタイム型の .net マッピング](../winrt-components/net-framework-mappings-of-windows-runtime-types.md)」を参照してください。

これらのマッピングの多くはインターフェイスです。 たとえば、IList は \<T\> Windows ランタイムインターフェイス IVector にマップさ \<T\> れます。 \<string\>パラメーターの型として IList ではなく list を使用すると \<string\> 、C#/WinRT では、リストによって実装されたすべてのマップされたインターフェイスを含む代替の一覧が提供され \<T\> ます。 List などの入れ子になったジェネリック型を使用する場合 \<Dictionary\<int, string\> \> 、C#/WinRT は入れ子のレベルごとに選択肢を提供します。 これらのリストはかなり長くなる場合があります。

一般に、最適なのは型に最も近いインターフェイスです。 たとえば、辞書の場合、 \<int, string\> 最も適しているのは IDictionary \<int, string\> です。

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th><p>エラー番号</p></th>
<th><p>[メッセージ テキスト]</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>CsWinRT1006</p></td>
<td><p>メンバー ' {0} ' のシグネチャに型 ' ' が含まれてい {1} ます。 型 ' {1} ' は有効な Windows ランタイム型ではありません。 ただし、型 (またはそのジェネリックパラメーター) は、有効な Windows ランタイム型のインターフェイスを実装します。 メンバーシグネチャの型を、system.string の {1} 次のいずれかの型に変更することを検討して {2} ください。</p></td>
</tr>
</tbody>
</table>

Windows ランタイムでは、メンバーシグネチャ内の配列は、下限が 0 (ゼロ) の1次元である必要があります。 `myArray[][]`(CsWinRT1017) や (CsWinRT1018) などの入れ子になった配列型 `myArray[,]` は使用できません。

> [!NOTE]
> この制限は、実装で内部的に使用する配列には適用されません。

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th><p>エラー番号</p></th>
<th><p>[メッセージ テキスト]</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>CsWinRT1017</p></td>
<td><p>メソッド {0} のシグネチャに、入れ子になった型の配列が含まれてい {1} ます。 Windows ランタイムメソッドシグネチャ内の配列を入れ子にすることはできません。</p></td>
</tr>
<tr class="even">
<td><p>CsWinRT1018</p></td>
<td><p>メソッド ' {0} ' のシグネチャには、型 ' ' の多次元配列が含まれてい {1} ます。 Windows ランタイム メソッドのシグネチャ内の配列は 1 次元配列にする必要があります。</p></td>
</tr>
</tbody>
</table>

## <a name="structures-that-contain-fields-of-disallowed-types"></a>使うことができない型のフィールドを含む構造体

Windows ランタイムでは、構造体にはフィールドのみを含めることができ、フィールドは構造体にのみ含めることができます。 これらのフィールドはパブリック型である必要があります。 有効なフィールド型には、列挙体、構造体、およびプリミティブ型があります。

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th><p>エラー番号</p></th>
<th><p>[メッセージ テキスト]</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>CsWinRT1007</p></td>
<td><p>構造体に {0} パブリックフィールドが含まれていません。 Windows ランタイム構造体には、少なくとも1つのパブリックフィールドを含める必要があります。</p></td>
</tr>
<tr class="even">
<td><p>CsWinRT1011</p></td>
<td><p>構造体 {0} にパブリックでないフィールドがあります。 Windows ランタイム構造体では、すべてのフィールドがパブリックである必要があります。</p></td>
</tr>
<tr class="odd">
<td><p>CsWinRT1012</p></td>
<td><p>構造体 {0} に const フィールドがあります。 定数は Windows ランタイム列挙型でのみ使用できます。</p></td>
</tr>
<tr class="even">
<td><p>CsWinRT1013</p></td>
<td><p>構造体 {0} に型のフィールドがあります {1} 。 {1} は有効な Windows ランタイムフィールド型ではありません。 Windows ランタイムの構造体に含まれる各フィールドに指定できるのは、UInt8、Int16、UInt16、Int32、UInt32、Int64、UInt64、Single、Double、Boolean、String、Enum、または構造体自体のみです。</p></td>
</tr>
</tbody>
</table>

## <a name="parameter-name-conflicts-with-generated-code"></a>パラメーター名が生成されたコードと競合しています

Windows ランタイムでは、戻り値は出力パラメーターと見なされ、パラメーターの名前は一意である必要があります。 既定では、C#/WinRT は戻り値の名前を指定し `__retval` ます。 メソッドにという名前のパラメーターがある場合は `__retval` 、エラー CsWinRT1010 が返されます。 これを修正するには、パラメーターに以外の名前を付け `__retvalue` ます。

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th><p>エラー番号</p></th>
<th><p>[メッセージ テキスト]</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>CsWinRT1010</p></td>
<td><p>{1}メソッドのパラメーター名 {0} は、生成された C#/WinRT 相互運用機能で使用される戻り値パラメーター名と同じです。別のパラメーター名を使用してください。</p></td>
</tr>
</tbody>
</table>

## <a name="miscellaneous"></a>その他

C#/WinRT で作成されたコンポーネントには、次のような制限があります。

- パブリック型に対してオーバーロードされた演算子を公開することはできません。
- クラスとインターフェイスをジェネリックにすることはできません。
- クラスはシールされている必要があります。
- **Ref** キーワードなどを使用して、パラメーターを参照渡しすることはできません。
- プロパティには、パブリックな get メソッドが必要です。
- コンポーネントの名前空間には、少なくとも1つのパブリック型 (クラスまたはインターフェイス) が必要です。

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th><p>エラー番号</p></th>
<th><p>[メッセージ テキスト]</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>CsWinRT1014</p></td>
<td><p>' {0} ' は演算子のオーバーロードです。 マネージ型は、Windows ランタイムで演算子オーバーロードを公開できません。</p></td>
</tr>
<tr class="even">
<td><p>CsWinRT1004</p></td>
<td><p>種類 {0} は generic です。 Windows ランタイム型をジェネリックにすることはできません。</p></td>
</tr>
<tr class="odd">
<td><p>CsWinRT1005</p></td>
<td><p>非封印型のエクスポートは、CsWinRT ではサポートされていません。 型をシールとしてマークしてください {0} 。</p></td>
</tr>
<tr class="even">
<td><p>CsWinRT1020</p></td>
<td><p>メソッド ' {0} ' にパラメーター ' {1} ' が設定されてい `ref` ます。 参照パラメーターは Windows ランタイムでは許可されていません。</p></td>
</tr>
<tr class="odd">
<td><p>CsWinRT1000</p></td>
<td><p>プロパティ ' {0} ' にパブリック getter メソッドがありません。 Windows ランタイムは setter のみのプロパティをサポートしていません。</p></td>
</tr>
<tr class="even">
<td><p>CsWinRT1003</p></td>
<td><p>Windows ランタイムコンポーネントには、少なくとも1つのパブリック型が必要です</p></td>
</tr>
</tbody>
</table>

Windows ランタイムでは、クラスは、指定された数のパラメーターを持つコンストラクターを1つだけ持つことができます。 たとえば、 **文字列** 型の1つのパラメーターと **int** 型の1つのパラメーターを持つコンストラクターを1つ持つことはできません。唯一の回避策は、コンストラクターごとに異なる数のパラメーターを使用することです。

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th><p>エラー番号</p></th>
<th><p>[メッセージ テキスト]</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>CsWinRT1009</p></td>
<td><p>クラスには、Windows ランタイム内の同じアリティの複数のコンストラクターを含めることはできません。クラスに {0} は複数 {1} のアリティコンストラクターがあります。</p></td>
</tr>
</tbody>
</table>


