---
title: C# および Visual Basic を使用した Windows ランタイム コンポーネント
description: .NET 4.5 以降では、マネージコードを使用して、Windows ランタイムコンポーネントにパッケージ化された独自の Windows ランタイム型を作成できます。
ms.assetid: A5672966-74DF-40AB-B01E-01E3FCD0AD7A
ms.date: 12/04/2018
ms.topic: article
dev_langs:
- csharp
- vb
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c402b8e4ba98f55267a42c1bce1c16e6f090e80c
ms.sourcegitcommit: a20457776064c95a74804f519993f36b87df911e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/27/2019
ms.locfileid: "71340370"
---
# <a name="windows-runtime-components-with-c-and-visual-basic"></a>C# および Visual Basic を使用した Windows ランタイム コンポーネント

マネージコードを使用して、独自の Windows ランタイム型を作成し、Windows ランタイムコンポーネントでパッケージ化することができます。 コンポーネントは、、JavaScript、Visual Basic、またはC++ C#で記述されたユニバーサル Windows プラットフォーム (UWP) アプリで使用できます。 このトピックでは、コンポーネントを作成するための規則を示し、Windows ランタイム向けの .NET のサポートをいくつか説明します。 このサポートは、通常、.NET のプログラマが意識しなくても利用できるように設計されています。 ただし、JavaScript や C++ で使うコンポーネントを作成する場合は、これらの言語が Windows ランタイムをサポートする方法の違いに注意する必要があります。

Visual Basic またはC#で記述された uwp アプリでのみ使用するコンポーネントを作成し、コンポーネントに uwp コントロールが含まれていない場合は、 **Windows ランタイムコンポーネント**プロジェクトの代わりに**クラスライブラリ**テンプレートを使用します。Microsoft Visual Studio 内のテンプレート。 単純なクラス ライブラリでは、制限は少なくなります。

## <a name="declaring-types-in-windows-runtime-components"></a>Windows ランタイムコンポーネントでの型の宣言

内部的には、コンポーネント内の Windows ランタイム型は、UWP アプリで許可されているすべての .NET 機能を使用できます。 詳細については、「 [UWP アプリ用 .net](https://docs.microsoft.com/dotnet/api/index?view=dotnet-uwp-10.0)」を参照してください。

外部的には、型のメンバーは、パラメーターと戻り値に対して Windows ランタイム型のみを公開できます。 次の一覧では、Windows ランタイムコンポーネントから公開される .NET 型の制限事項について説明します。

- コンポーネント内にあるすべてのパブリック型とメンバーのフィールド、パラメーター、戻り値は、Windows ランタイム型である必要があります。 この制限には、作成した Windows ランタイムの種類と、Windows ランタイム自体によって提供される型が含まれます。 また、多くの .NET 型も含まれています。 これらの型を含めることは、マネージコードで Windows ランタイムを自然に使用できるようにするために .NET が提供するサポートの一部です。 @ no__t-0your の Windows ランタイム型ではなく、使い慣れた .NET 型を使用するようにコードを表示します。 たとえば、 **Int32**や**Double**などの .Net プリミティブ型、 **DateTimeOffset**や**Uri**などの特定の基本型、および**IEnumerable @ no__t-5t @ no__t-6 などの一般的に使用されるジェネリックインターフェイス型を使用できます。** (Visual Basic 内の IEnumerable (Of T)) と**IDictionary @ No__t-8tkey, TValue @ no__t-9**。 これらのジェネリック型の型引数は Windows ランタイム型である必要があることに注意してください。 この点については、このトピックの後半の「[マネージコードに Windows ランタイム型を渡す](#passing-windows-runtime-types-to-managed-code)」と「[マネージ型を Windows ランタイムに渡す](#passing-managed-types-to-the-windows-runtime)」セクションで説明します。

- パブリック クラスとインターフェイスには、メソッド、プロパティ、イベントを含めることができます。 イベントのデリゲートを宣言することも、 **EventHandler @ no__t-1t @ no__t**デリゲートを使用することもできます。 パブリッククラスまたはインターフェイスでは、次のことはできません。
    - ジェネリックにする。
    - Windows ランタイムインターフェイスではないインターフェイスを実装します (ただし、独自の Windows ランタイムインターフェイスを作成して実装することもできます)。
    - **System.exception**や system.servicemodel など、Windows ランタイムに含まれていない型から派生し**ます。**

- すべてのパブリック型にはアセンブリ名に一致するルート名前空間が必要になります。ただし、アセンブリ名の先頭には "Windows" を付けることはできません。

    > **ヒント**。 既定では、Visual Studio プロジェクトにはアセンブリ名に一致する名前空間名があります。 Visual Basic では、この既定の名前空間の Namespace ステートメントはコードに表示されません。

- パブリック構造体はパブリック フィールド以外のメンバーを持つことができません。また、それらのフィールドは値型または文字列であることが必要です。
- パブリック クラスは **sealed** (Visual Basic では **NotInheritable**) であることが必要です。 プログラミングモデルがポリモーフィズムを必要とする場合は、パブリックインターフェイスを作成し、ポリモーフィックである必要があるクラスにそのインターフェイスを実装することができます。

## <a name="debugging-your-component"></a>コンポーネントのデバッグ

UWP アプリとコンポーネントの両方がマネージコードでビルドされている場合は、両方を同時にデバッグできます。

を使用C++する UWP アプリの一部としてコンポーネントをテストしている場合は、マネージコードとネイティブコードを同時にデバッグできます。 既定では、ネイティブ コードのみになります。

## <a name="to-debug-both-native-c-code-and-managed-code"></a>ネイティブ C++ コードとマネージ コードの両方をデバッグするには
1.  Visual C++ プロジェクトのショートカット メニューを開き、 **[プロパティ]** をクリックします。
2.  プロパティ ページの **[構成プロパティ]** で、 **[デバッグ]** を選びます。
3.  **[デバッガーの種類]** を選び、ドロップダウン リスト ボックスで、 **[ネイティブのみ]** を **[混合 (マネージとネイティブ)]** に変更します。 **[OK]** をクリックします。
4.  ネイティブ コードとマネージ コードのブレークポイントを設定します。

JavaScript を使用して UWP アプリの一部としてコンポーネントをテストしている場合、既定では、ソリューションは JavaScript デバッグモードになります。 Visual Studio では、JavaScript とマネージ コードを同時にデバッグすることはできません。

## <a name="to-debug-managed-code-instead-of-javascript"></a>JavaScript ではなくマネージ コードをデバッグするには
1.  JavaScript プロジェクトのショートカット メニューを開き、 **[プロパティ]** を選びます。
2.  プロパティ ページの **[構成プロパティ]** で、 **[デバッグ]** を選びます。
3.  **[デバッガーの種類]** を選び、ドロップダウン リスト ボックスで、 **[スクリプトのみ]** を **[マネージのみ]** に変更します。 **[OK]** をクリックします。
4.  マネージ コードのブレークポイントを設定し、通常どおりにデバッグします。

## <a name="passing-windows-runtime-types-to-managed-code"></a>マネージ コードへの Windows ランタイム型の引き渡し
「 [Windows ランタイムコンポーネントの宣言する型](#declaring-types-in-windows-runtime-components)」セクションで説明したように、特定の .net 型はパブリッククラスのメンバーのシグネチャに表示されることがあります。 これは、マネージコードで Windows ランタイムを自然に使用できるようにするために .NET が提供するサポートの一部です。 これには、プリミティブ型と一部のクラスやインターフェイスが含まれます。 コンポーネントが JavaScript またはコードからC++使用されている場合は、.net 型が呼び出し元にどのように表示されるかを把握しておくことが重要です。 JavaScript を使用した例について[ C#は、または Visual Basic Windows ランタイムコンポーネントの作成と javascript からの呼び出し](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)に関するチュートリアルを参照してください。 このセクションでは、よく使われる型について説明します。

.NET では、 **Int32**構造体などのプリミティブ型には、 **TryParse**メソッドなどの便利なプロパティとメソッドが多数あります。 これに対して、Windows ランタイムのプリミティブ型と構造体は、フィールドしか保持していません。 これらの型をマネージコードに渡すと、.NET 型であるように見えます。通常どおり、.NET 型のプロパティとメソッドを使用できます。 IDE で自動的に行われる置き換えの概要を次に示します。

-   Windows ランタイムプリミティブ**Int32**、 **Int64**、 **Single**、 **Double**、 **Boolean**、 **String** (Unicode 文字の変更できないコレクション)、 **Enum**、 **UInt32**、 **UInt64**、および**Guid**では、System 名前空間で同じ名前の型を使用します。
-   **UInt8**の場合は、system.string**を使用します。**
-   **Char16**を使用する場合は、system.string を使用**します。**
-   **IInspectable**インターフェイスの場合は、 **system.object**を使用します。

またC#は Visual Basic がこれらの型のいずれかに言語キーワードを提供している場合は、代わりに言語キーワードを使用できます。

プリミティブ型に加えて、一般的に使用されるいくつかの基本的な Windows ランタイム型は、.NET に相当するものとしてマネージコードに表示されます。 たとえば、JavaScript コードが**Windows の Foundation. Uri**クラスを使用していて、それをC#または Visual Basic メソッドに渡すとします。 マネージコードの等価の型**は .net system.string**クラスであり、これはメソッドパラメーターに使用する型です。 マネージコードを記述しているときに Visual Studio の IntelliSense によって Windows ランタイム型が非表示になり、同等の .NET 型が表示されるため、Windows ランタイム型が .NET 型として表示されるタイミングを指定できます。 (通常、2 つの型の名前は同じです。 ただし、Windows の. **datetime**構造体は、system.string としてでは**なく、system.string**としてマネージコードに表示されることに**注意してください。**

一般的に使用されるコレクション型については、Windows ランタイム型によって実装されるインターフェイスと、対応する .NET 型によって実装されるインターフェイスとの間でマッピングが行われます。 前述の型と同様に、.NET 型を使用してパラメーターの型を宣言します。 これにより、型のいくつかの違いが隠蔽され、.NET コードをより自然に記述できるようになります。

次の表は、最も一般的なジェネリック インターフェイスの型、および他の一般的なクラスやインターフェイスに関する対応付けを示しています。 .NET によってマップされる Windows ランタイムの種類の完全な一覧については、「 [Windows ランタイム型の .net マッピング](net-framework-mappings-of-windows-runtime-types.md)」を参照してください。

| Windows ランタイム                                  | .NET                                    |
|-|-|
| IIterable @ no__t-0T @ no__t-1                               | IEnumerable @ no__t-0T @ no__t-1                              |
| IVector @ no__t-0T @ no__t-1                                 | IList @ no__t-0T @ no__t-1                                    |
| IVectorView @ no__t-0T @ no__t-1                             | IReadOnlyList @ no__t-0T @ no__t-1                            |
| IMap @ no__t-正常処理, V @ no__t-1                                 | IDictionary @ no__t-0TKey, TValue @ no__t-1                   |
| IMapView @ no__t-正常処理, V @ no__t-1                             | Ireadonlydictionary< @ no__t-0TKey, TValue @ no__t-1           |
| Ikeyvaluepair<k, @ no__t-正常処理, V @ no__t-1                        | KeyValuePair @ no__t-0TKey, TValue @ no__t-1                  |
| IBindableIterable                                | IEnumerable                                       |
| IBindableVector                                  | IList                                             |
| Windows.UI.Xaml.Data.INotifyPropertyChanged      | System.ComponentModel.INotifyPropertyChanged      |
| Windows.UI.Xaml.Data.PropertyChangedEventHandler | System.ComponentModel.PropertyChangedEventHandler |
| Windows.UI.Xaml.Data.PropertyChangedEventArgs    | System.ComponentModel.PropertyChangedEventArgs    |

型によって複数のインターフェイスが実装される場合、メンバーのパラメーターの型または戻り値の型として実装されるインターフェイスをすべて使うことができます。 たとえば、**辞書 @ no__t-1int、string @ no__t** (**辞書 (Of Integer、string Visual Basic)** as) を**IDictionary @ no__t-5int、string @ no__t-6**、 **ireadonlydictionary< @ no__t-8int、string @ no__t-9 として渡すか、返すことができます。** 、または**IEnumerable @ No__t-11system @ No__t-12tkey, TValue @ no__t-13 @ no__t-14**.

> [!IMPORTANT]
> JavaScript は、マネージ型が実装するインターフェイスのリストに最初に表示されるインターフェイスを使用します。 たとえば、**辞書 @ no__t-1int, string @ no__t-2**を JavaScript コードに返すと、戻り値の型として指定したインターフェイスに関係なく、 **IDictionary @ no__t-4int, string @ no__t-5**として表示されます。 つまり、後のインターフェイスにメンバーが最初のインターフェイスに含まれていない場合、そのメンバーは JavaScript では認識されません。

Windows ランタイムでは、 **IMap @ no__t-1k、v @ no__t-2** 、 **IMapView @ no__t、v @ no__t-5**は ikeyvaluepair<k, を使用して反復処理されます。 これらのコードをマネージコードに渡すと、それらは**IDictionary @ no__t-1TKey, TValue @ no__t-2** and **ireadonlydictionary< @ No__t-4Tkey, TValue @** no__t-5 として表示されるので、自然に使用します。 **KeyValuePair @ no__t-7tkey,TValue @ no__t-8**を列挙します。

インターフェイスがマネージ コード内に表示される方法によって、これらのインターフェイスを実装する型の表示方法が決まります。 たとえば、 **PropertySet**クラスは**IMap @ no__t-2k, V @ no__t**を実装します。これは、マネージコードでは、 **IDictionary @ No__t-5tkey, TValue @ no__t-6**として表示されます。 **PropertySet**が実装されているかのように、 **IMap @ no__t、V @ 5K**の代わりに、 **IDictionary @ No__t-2tkey, TValue @ no__t**が実装されているように見えます。マネージコードでは **、Add メソッドのよう**に動作する**add**メソッドがあるように見えます。NET 辞書。 **Insert**メソッドがないように見えます。 この例について[ C#は、「」または「Visual Basic Windows ランタイムコンポーネントの作成」のチュートリアルと、JavaScript からの呼び出し](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)に関するトピックを参照してください。

## <a name="passing-managed-types-to-the-windows-runtime"></a>Windows ランタイムへのマネージ型の引き渡し

前のセクションで説明したように、一部の Windows ランタイム型は、コンポーネントのメンバーのシグネチャに .NET 型として、または IDE で使用するときに Windows ランタイムメンバーのシグネチャに表示できます。 これらのメンバーに .NET 型を渡したり、コンポーネントのメンバーの戻り値として使用したりする場合は、対応する Windows ランタイム型として、もう一方の側のコードに表示されます。 コンポーネントが JavaScript から呼び出されたときの効果の例について[ C#は、「」の「コンポーネントからマネージ型を返す」セクションを参照してください。または Visual Basic Windows ランタイムコンポーネントの作成と、JavaScript](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)。

## <a name="overloaded-methods"></a>オーバー ロードされたメソッド

Windows ランタイムでは、メソッドはオーバーロードできます。 ただし、同じ数のパラメーターを持つ複数のオーバーロードを宣言する場合は、これらのオーバーロードのうち1つのみに[**windows.foundation.metadata.defaultoverloadattribute**](/uwp/api/windows.foundation.metadata.defaultoverloadattribute)属性を適用する必要があります。 この属性が適用されるオーバーロードが、JavaScript から呼び出すことができる唯一のオーバーロードになります。 たとえば、次のコードでは、**int** (Visual Basic では **Integer**) を受け取るオーバーロードが既定のオーバーロードです。

```csharp
public string OverloadExample(string s)
{
    return s;
}

[Windows.Foundation.Metadata.DefaultOverload()]
public int OverloadExample(int x)
{
    return x;
}
```

```vb
Public Function OverloadExample(ByVal s As String) As String
    Return s
End Function

<Windows.Foundation.Metadata.DefaultOverload> _
Public Function OverloadExample(ByVal x As Integer) As Integer
    Return x
End Function
```

> :JavaScript を使用すると、任意の値を**OverloadExample**に渡すことができ、パラメーターに必要な型に強制的に値が強制されます。 "42"、"42"、または42.3 を使用して**OverloadExample**を呼び出すことができますが、これらの値はすべて既定のオーバーロードに渡されます。 前の例の既定のオーバーロードでは、それぞれ0、42、および42が返されます。

コンストラクターに**DefaultOverloadAttribut**e 属性を適用することはできません。 クラスのすべてのコンストラクターは、異なる数のパラメーターを持つ必要があります。

## <a name="implementing-istringable"></a>IStringable の実装

Windows 8.1 以降、Windows ランタイムには**istringable**インターフェイスが含まれています。このインターフェイスは、単一のメソッドである**istringable**を使用して、**オブジェクトの tostring**によって提供される基本的な書式設定のサポートを提供します。 Windows ランタイムコンポーネントにエクスポートされるパブリックマネージ型に**Istringable**を実装する場合は、次の制限が適用されます。

-   **Istringable**インターフェイスは、のC#次のコードのように、"クラスが実装する" 関係でのみ定義できます。

    ```cs
    public class NewClass : IStringable
    ```

    Visual Basic では、次のようになります。

    ```vb
    Public Class NewClass : Implements IStringable
    ```

-   インターフェイスに**Istringable**を実装することはできません。
-   パラメーターの型を**Istringable**として宣言することはできません。
-   **Istringable**は、メソッド、プロパティ、またはフィールドの戻り値の型にすることはできません。
-   次のようなメソッド定義を使用して、 **Istringable**の実装を基底クラスから隠すことはできません。

    ```cs
    public class NewClass : IStringable
    {
       public new string ToString()
       {
          return "New ToString in NewClass";
       }
    }
    ```

    代わりに、 **Istringable**の実装では、常に基底クラスの実装をオーバーライドする必要があります。 **ToString**実装は、厳密に型指定されたクラスインスタンスで呼び出すことによってのみ非表示にすることができます。

> [!NOTE]
> さまざまな条件下で、 **Istringable**を実装するマネージ型、またはその**ToString**実装を隠すマネージ型にネイティブコードから呼び出すと、予期しない動作が生じる可能性があります。

## <a name="asynchronous-operations"></a>非同期操作

コンポーネントに非同期メソッドを実装するには、メソッド名の末尾に "Async" を追加し、非同期アクションまたは非同期操作を表す Windows ランタイムインターフェイスの1つを返します。**Iasyncaction**、 **iasyncactionwithprogress @ No__t-2tprogress @ no__t**、 **IAsyncOperation @ no__t-5tresult @ no__t-6**、または**IAsyncOperationWithProgress @ no__t-8tresult、tprogress @ no__t-9**。

.NET タスク ([**タスク**](/dotnet/api/system.threading.tasks.task)クラスと汎用[**タスク @ No__t-4tresult @ no__t-5**](/dotnet/api/system.threading.tasks.task-1)クラス) を使用して、非同期メソッドを実装できます。 または Visual Basic でC#記述された非同期メソッドから返されるタスクや、 [task. Run](/dotnet/api/system.threading.tasks.task.run)メソッドから返されたタスクなど、進行中の操作を表すタスクを返す必要があります。 コンストラクターを使ってタスクを作成する場合、その [Task.Start](/dotnet/api/system.threading.tasks.task.start) メソッドを呼び出してから戻す必要があります。

@No__t-0 (Visual Basic で `Await`) を使用するメソッドには、`async` キーワード (@no__t では-3) が必要です。 このようなメソッドを Windows ランタイムコンポーネントから公開する場合は、 **Run**メソッドに渡すデリゲートに @no__t 0 キーワードを適用します。

取り消しや進行状況の報告をサポートしない非同期アクションと非同期操作では、タスクを適切なインターフェイスにラップするために、[WindowsRuntimeSystemExtensions.AsAsyncAction](https://docs.microsoft.com/dotnet/api/system) または [AsAsyncOperation&lt;TResult&gt;](https://docs.microsoft.com/dotnet/api/system) の拡張メソッドを使うことができます。 たとえば、次のコードでは、タスクを使用して非同期メソッドを実装**しています。 Run @ no__t-1TResult @ no__t-2**メソッドを使用してタスクを開始します。 **AsAsyncOperation @ no__t-1TResult @ no__t-2**拡張メソッドは、タスクを Windows ランタイム非同期操作として返します。

```csharp
public static IAsyncOperation<IList<string>> DownloadAsStringsAsync(string id)
{
    return Task.Run<IList<string>>(async () =>
    {
        var data = await DownloadDataAsync(id);
        return ExtractStrings(data);
    }).AsAsyncOperation();
}
```

```vb
Public Shared Function DownloadAsStringsAsync(ByVal id As String) _
     As IAsyncOperation(Of IList(Of String))

    Return Task.Run(Of IList(Of String))(
        Async Function()
            Dim data = Await DownloadDataAsync(id)
            Return ExtractStrings(data)
        End Function).AsAsyncOperation()
End Function
```

次の JavaScript コードは、 [**WinJS**](https://docs.microsoft.com/previous-versions/windows/apps/br211867(v=win.10))オブジェクトを使用してメソッドを呼び出す方法を示しています。 then メソッドに渡される関数は、非同期呼び出しが完了したときに実行されます。 StringList パラメーターには、 **Downloadasstringasync**メソッドによって返される文字列のリストが含まれています。この関数は、必要な処理をすべて実行します。

```javascript
function asyncExample(id) {

    var result = SampleComponent.Example.downloadAsStringAsync(id).then(
        function (stringList) {
            // Place code that uses the returned list of strings here.
        });
}
```

キャンセルまたは進行状況レポートをサポートする非同期アクションおよび操作の場合は、 [**Asyncinfo**](/dotnet/api/system.runtime.interopservices.windowsruntime)クラスを使用して開始されたタスクを生成し、キャンセルと進行状況を使用してタスクのキャンセル機能と進行状況レポート機能をフックします。適切な Windows ランタイムインターフェイスのレポート機能。 キャンセルと進行状況レポートの両方をサポートする例について[ C#は、「」または「Visual Basic Windows ランタイムコンポーネントの作成」と「JavaScript からの呼び出し](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)」を参照してください。

非同期メソッドがキャンセルまたは進行状況レポートをサポートしていない場合でも、 **Asyncinfo**クラスのメソッドを使用できることに注意してください。 Visual Basic のラムダ関数またはC#匿名メソッドを使用する場合は、トークンのパラメーターと[iprogress @ No__t-3t @ no__t-4](https://docs.microsoft.com/dotnet/api/system.iprogress-1)インターフェイスを指定しないでください。 C# のラムダ関数を使う場合は、トークンのパラメーターを指定しますが、無視されます。 前の例では、AsAsyncOperation @ no__t-0TResult @ no__t メソッドが使用されています。この例では、 [**Asyncinfo. Run @ no__t-4TResult @ no__t-5 (Func @ no__t-6CancellationToken, Task @ no__t-7TResult @ no__t-8 @ no__t-9**](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.windowsruntime)) メソッドを使用すると、次のようになります。代わりにオーバーロードします。

```csharp
public static IAsyncOperation<IList<string>> DownloadAsStringsAsync(string id)
{
    return AsyncInfo.Run<IList<string>>(async (token) =>
    {
        var data = await DownloadDataAsync(id);
        return ExtractStrings(data);
    });
}
```

```vb
Public Shared Function DownloadAsStringsAsync(ByVal id As String) _
    As IAsyncOperation(Of IList(Of String))

    Return AsyncInfo.Run(Of IList(Of String))(
        Async Function()
            Dim data = Await DownloadDataAsync(id)
            Return ExtractStrings(data)
        End Function)
End Function
```

オプションでキャンセルまたは進行状況レポートをサポートする非同期メソッドを作成する場合は、キャンセルトークンまたは**Iprogress @ no__t-1T @ no__t**インターフェイスのパラメーターを持たないオーバーロードを追加することを検討してください。

## <a name="throwing-exceptions"></a>例外のスロー

Windows アプリ用 .NET に含まれている例外の型は、どれでもスローすることができます。 Windows ランタイム コンポーネントで独自のパブリック型の例外を宣言することはできませんが、非パブリック型を宣言し、スローすることはできます。

コンポーネントが例外を処理しない場合は、コンポーネントを呼び出したコードで対応する例外が発生します。 例外が呼び出し元に表示される方法は、呼び出し元の言語が Windows ランタイムをサポートする方法によって異なります。

-   JavaScript では、例外はオブジェクトとして表示され、例外メッセージがスタック トレースで置き換えられています。 Visual Studio でアプリをデバッグするとき、デバッガーの例外ダイアログ ボックスに、"WinRT 情報" として元のメッセージ テキストが表示されます。 JavaScript コードから元のメッセージ テキストにアクセスすることはできません。

    > **ヒント**。 現在、スタックトレースにはマネージ例外型が含まれていますが、例外の種類を識別するためにトレースを解析することはお勧めしません。 このセクションの後半で説明するように、代わりに HRESULT 値を使ってください。

-   C++ では、例外はプラットフォーム例外として表示されます。 マネージ例外の HResult プロパティを特定のプラットフォーム例外の HRESULT にマップできる場合は、特定の例外が使用されます。それ以外の場合は、 [**Platform:: COMException**](https://docs.microsoft.com/cpp/cppcx/platform-comexception-class)例外がスローされます。 マネージ例外のメッセージ テキストは、C++ コードでは利用できません。 特定のプラットフォーム例外がスローされた場合、その例外の型に関する既定のメッセージ テキストが表示されます。それ以外の場合は、メッセージ テキストは表示されません。 「[例外 (C++/CX)](https://docs.microsoft.com/cpp/cppcx/exceptions-c-cx)」をご覧ください。
-   C# または Visual Basic では、例外は通常のマネージ例外です。

コンポーネントから例外をスローする場合、コンポーネントに固有の HResult プロパティ値を持つ非パブリック型の例外をスローすることにより、JavaScript や C++ の呼び出し元で例外を簡単に処理できるようになります。 HRESULT は、JavaScript の呼び出し元が例外オブジェクトの number プロパティを介して使用できますC++ 。また、 [COMException:: HRESULT](https://docs.microsoft.com/cpp/cppcx/platform-comexception-class#hresult)プロパティを通じて呼び出し元に提供されます。

> [!NOTE]
> HRESULT には負の値を使用します。 正の値は成功と解釈されるので、JavaScript や C++ の呼び出し元で例外がスローされなくなります。

## <a name="declaring-and-raising-events"></a>イベントの宣言と発生

イベントのデータを保持する型を宣言する場合、EventArgs は Windows ランタイム型ではないので、EventArgs の代わりに Object から派生させます。 イベントの種類として[**EventHandler @ no__t-2TEventArgs @ no__t-3**](https://docs.microsoft.com/dotnet/api/system.eventhandler-1)を使用し、イベントの引数の型をジェネリック型の引数として使用します。 .NET アプリケーションの場合と同じように、イベントを発生させます。

Windows ランタイム コンポーネントが JavaScript や C++ で使われる場合、イベントはそれらの言語で想定されている Windows ランタイムのイベント パターンに従います。 または Visual Basic からC#コンポーネントを使用する場合、イベントは通常の .net イベントとして表示されます。 [または Visual Basic Windows ランタイムコンポーネントを作成し、JavaScript から呼び出す方法については、「」のチュートリアルで説明しています。 C# ](/windows/uwp/winrt-components/walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript)

カスタム イベント アクセサーを実装する場合 (Visual Basic では **Custom** キーワードでイベントを宣言する場合) は、実装で Windows ランタイムのイベント パターンに従う必要があります。 「 [Windows ランタイムコンポーネント」の「カスタムイベントとイベントアクセサー](custom-events-and-event-accessors-in-windows-runtime-components.md)」を参照してください。 または Visual Basic コードからC#イベントを処理する場合は、通常の .net イベントであるように見えます。

## <a name="next-steps"></a>次の手順

ユーザーが独自に使う Windows ランタイム コンポーネントを作成した後で、そのコンポーネントにカプセル化されている機能が他の開発者の役に立つ場合があります。 他の開発者に配布するためにコンポーネントをパッケージ化する方法は 2 つあります。 「[マネージ Windows ランタイム コンポーネントの配布](https://docs.microsoft.com/previous-versions/windows/apps/jj614475(v=vs.140))」をご覧ください。

Visual Basic とC#言語の機能、および Windows ランタイムの .net サポートの詳細については、「 [Visual Basic C#および言語リファレンス](https://docs.microsoft.com/visualstudio/welcome-to-visual-studio-2015?view=vs-2015)」を参照してください。

## <a name="related-topics"></a>関連トピック
* [UWP アプリ用 .NET](https://docs.microsoft.com/dotnet/api/index?view=dotnet-uwp-10.0)
* [C# または Visual Basic Windows ランタイム コンポーネントの作成と JavaScript からの呼び出しに関するチュートリアル](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)
