---
title: C# および Visual Basic を使用した Windows ランタイム コンポーネント
description: .NET 4.5 以降では、マネージコードを使用して、Windows ランタイムコンポーネントにパッケージ化された独自の Windows ランタイム型を作成できます。
ms.assetid: A5672966-74DF-40AB-B01E-01E3FCD0AD7A
ms.date: 12/04/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- vb
ms.openlocfilehash: e78171fa182d44f1699bc35643265fddb87824f4
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/24/2020
ms.locfileid: "91220295"
---
# <a name="windows-runtime-components-with-c-and-visual-basic"></a>C# および Visual Basic を使用した Windows ランタイム コンポーネント

マネージコードを使用して、独自の Windows ランタイム型を作成し、Windows ランタイムコンポーネントでパッケージ化することができます。 コンポーネントは、C++、JavaScript、Visual Basic、または C# で記述されたユニバーサル Windows プラットフォーム (UWP) アプリで使用できます。 このトピックでは、コンポーネントを作成するための規則を示し、Windows ランタイム向けの .NET のサポートをいくつか説明します。 このサポートは、通常、.NET のプログラマが意識しなくても利用できるように設計されています。 ただし、JavaScript や C++ で使うコンポーネントを作成する場合は、これらの言語が Windows ランタイムをサポートする方法の違いに注意する必要があります。

Visual Basic または C# で記述された UWP アプリでのみ使用するコンポーネントを作成し、コンポーネントに UWP コントロールが含まれていない場合は、Microsoft Visual Studio で**Windows ランタイムコンポーネント**プロジェクトテンプレートの代わりに**クラスライブラリ**テンプレートを使用することを検討してください。 単純なクラス ライブラリでは、制限は少なくなります。

## <a name="declaring-types-in-windows-runtime-components"></a>Windows ランタイムコンポーネントでの型の宣言

内部的には、コンポーネント内の Windows ランタイム型は、UWP アプリで許可されているすべての .NET 機能を使用できます。 詳細については、「 [UWP アプリ用 .net](/dotnet/api/index?view=dotnet-uwp-10.0)」を参照してください。

外部的には、型のメンバーは、パラメーターと戻り値に対して Windows ランタイム型のみを公開できます。 次の一覧では、Windows ランタイムコンポーネントから公開される .NET 型の制限事項について説明します。

- コンポーネント内にあるすべてのパブリック型とメンバーのフィールド、パラメーター、戻り値は、Windows ランタイム型である必要があります。 この制限には、作成した Windows ランタイムの種類と、Windows ランタイム自体によって提供される型が含まれます。 また、多くの .NET 型も含まれています。 これらの型の追加は、マネージコードでの Windows ランタイムの自然な使用を有効にするために .NET が提供するサポートの一部です &mdash; 。基になる Windows ランタイム型の代わりに、使い慣れた .net 型を使用するようにコードが表示されます。 たとえば、 **Int32**や**Double**などの .Net プリミティブ型、 **DateTimeOffset**や**Uri**などの特定の基本型、および**ienumerable &lt; T &gt; ** (Visual Basic の ienumerable (Of T) など) および**IDictionary &lt; TKey, TValue &gt; **などの一般的に使用されるジェネリックインターフェイス型を使用できます。 これらのジェネリック型の型引数は Windows ランタイム型である必要があることに注意してください。 この点については、このトピックの後半の「 [マネージコードに Windows ランタイム型を渡す](#passing-windows-runtime-types-to-managed-code) 」と「 [マネージ型を Windows ランタイムに渡す](#passing-managed-types-to-the-windows-runtime)」セクションで説明します。

- パブリック クラスとインターフェイスには、メソッド、プロパティ、イベントを含めることができます。 イベントのデリゲートを宣言することも、 **EventHandler &lt; T &gt; **デリゲートを使用することもできます。 パブリッククラスまたはインターフェイスでは、次のことはできません。
    - ジェネリックにする。
    - Windows ランタイムインターフェイスではないインターフェイスを実装します (ただし、独自の Windows ランタイムインターフェイスを作成して実装することもできます)。
    - **System.exception**や system.servicemodel など、Windows ランタイムに含まれていない型から派生し**ます。**

- すべてのパブリック型にはアセンブリ名に一致するルート名前空間が必要になります。ただし、アセンブリ名の先頭には "Windows" を付けることはできません。

    > **ヒント**。 既定では、Visual Studio プロジェクトにはアセンブリ名に一致する名前空間名があります。 Visual Basic では、この既定の名前空間の Namespace ステートメントはコードに表示されません。

- パブリック構造体はパブリック フィールド以外のメンバーを持つことができません。また、それらのフィールドは値型または文字列であることが必要です。
- パブリック クラスは **sealed** (Visual Basic では **NotInheritable**) であることが必要です。 プログラミングモデルがポリモーフィズムを必要とする場合は、パブリックインターフェイスを作成し、ポリモーフィックである必要があるクラスにそのインターフェイスを実装することができます。

## <a name="debugging-your-component"></a>コンポーネントのデバッグ

UWP アプリとコンポーネントの両方がマネージコードでビルドされている場合は、両方を同時にデバッグできます。

C++ を使用して UWP アプリの一部としてコンポーネントをテストしている場合は、マネージコードとネイティブコードを同時にデバッグできます。 既定では、ネイティブ コードのみになります。

## <a name="to-debug-both-native-c-code-and-managed-code"></a>ネイティブ C++ コードとマネージ コードの両方をデバッグするには
1.  Visual C++ プロジェクトのショートカット メニューを開き、**[プロパティ]** をクリックします。
2.  プロパティ ページの **[構成プロパティ]** で、**[デバッグ]** を選びます。
3.  **[デバッガーの種類]** を選び、ドロップダウン リスト ボックスで、**[ネイティブのみ]** を **[混合 (マネージとネイティブ)]** に変更します。 **[OK]** を選びます。
4.  ネイティブ コードとマネージ コードのブレークポイントを設定します。

JavaScript を使用して UWP アプリの一部としてコンポーネントをテストしている場合、既定では、ソリューションは JavaScript デバッグモードになります。 Visual Studio では、JavaScript とマネージ コードを同時にデバッグすることはできません。

## <a name="to-debug-managed-code-instead-of-javascript"></a>JavaScript ではなくマネージ コードをデバッグするには
1.  JavaScript プロジェクトのショートカット メニューを開き、**[プロパティ]** を選びます。
2.  プロパティ ページの **[構成プロパティ]** で、**[デバッグ]** を選びます。
3.  **[デバッガーの種類]** を選び、ドロップダウン リスト ボックスで、**[スクリプトのみ]** を **[マネージのみ]** に変更します。 **[OK]** を選びます。
4.  マネージ コードのブレークポイントを設定し、通常どおりにデバッグします。

## <a name="passing-windows-runtime-types-to-managed-code"></a>マネージ コードへの Windows ランタイム型の引き渡し
「 [Windows ランタイムコンポーネントの宣言する型](#declaring-types-in-windows-runtime-components)」セクションで説明したように、特定の .net 型はパブリッククラスのメンバーのシグネチャに表示されることがあります。 これは、マネージコードで Windows ランタイムを自然に使用できるようにするために .NET が提供するサポートの一部です。 これには、プリミティブ型と一部のクラスやインターフェイスが含まれます。 コンポーネントが JavaScript または C++ コードから使用されている場合は、.NET 型が呼び出し元にどのように表示されるかを把握しておくことが重要です。 JavaScript を使用した例について [は、「C# または Visual Basic Windows ランタイムコンポーネントの作成」および「javascript からの呼び出し」のチュートリアル](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md) を参照してください。 このセクションでは、よく使われる型について説明します。

.NET では、 **Int32** 構造体などのプリミティブ型には、 **TryParse** メソッドなどの便利なプロパティとメソッドが多数あります。 これに対して、Windows ランタイムのプリミティブ型と構造体は、フィールドしか保持していません。 これらの型をマネージコードに渡すと、.NET 型であるように見えます。通常どおり、.NET 型のプロパティとメソッドを使用できます。 IDE で自動的に行われる置き換えの概要を次に示します。

-   Windows ランタイムプリミティブ **Int32**、 **Int64**、 **Single**、 **Double**、 **Boolean**、 **String** (Unicode 文字の変更できないコレクション)、 **Enum**、 **UInt32**、 **UInt64**、および **Guid**の場合は、System 名前空間で同じ名前の型を使用します。
-   **UInt8**の場合は、system.string**を使用します。**
-   **Char16**を使用する場合は、system.string を使用**します。**
-   **IInspectable**インターフェイスの場合は、 **system.object**を使用します。

C# または Visual Basic が、これらの型のいずれかに言語キーワードを提供している場合は、代わりに言語キーワードを使用できます。

プリミティブ型に加えて、一般的に使用されるいくつかの基本的な Windows ランタイム型は、.NET に相当するものとしてマネージコードに表示されます。 たとえば、JavaScript コードで **Windows の Foundation. Uri** クラスを使用し、それを C# または Visual Basic メソッドに渡すとします。 マネージコードの等価の型 **は .net system.string** クラスであり、これはメソッドパラメーターに使用する型です。 マネージコードを記述しているときに Visual Studio の IntelliSense によって Windows ランタイム型が非表示になり、同等の .NET 型が表示されるため、Windows ランタイム型が .NET 型として表示されるタイミングを指定できます。 (通常、2 つの型の名前は同じです。 ただし、Windows の. **datetime**構造体は、system.string としてでは**なく、system.string**としてマネージコードに表示されることに**注意してください。**

一般的に使用されるコレクション型については、Windows ランタイム型によって実装されるインターフェイスと、対応する .NET 型によって実装されるインターフェイスとの間でマッピングが行われます。 前述の型と同様に、.NET 型を使用してパラメーターの型を宣言します。 これにより、型のいくつかの違いが隠蔽され、.NET コードをより自然に記述できるようになります。

次の表は、最も一般的なジェネリック インターフェイスの型、および他の一般的なクラスやインターフェイスに関する対応付けを示しています。 .NET によってマップされる Windows ランタイムの種類の完全な一覧については、「 [Windows ランタイム型の .net マッピング](net-framework-mappings-of-windows-runtime-types.md)」を参照してください。

| Windows ランタイム                                  | .NET                                    |
|-|-|
| IIterable&lt;T&gt;                               | IEnumerable &lt; T&gt;                              |
| IVector&lt;T&gt;                                 | IList &lt; T&gt;                                    |
| IVectorView&lt;T&gt;                             | IReadOnlyList &lt; T&gt;                            |
| IMap &lt; K、V&gt;                                 | IDictionary &lt; TKey, TValue&gt;                   |
| IMapView&lt;K, V&gt;                             | Ireadonlydictionary< &lt; TKey, TValue&gt;           |
| Ikeyvaluepair<k, &lt; K、V&gt;                        | KeyValuePair &lt; TKey, TValue&gt;                  |
| IBindableIterable                                | IEnumerable                                       |
| IBindableVector                                  | IList                                             |
| Windows.UI.Xaml.Data.INotifyPropertyChanged      | System.ComponentModel.INotifyPropertyChanged      |
| Windows.UI.Xaml.Data.PropertyChangedEventHandler | System.ComponentModel.PropertyChangedEventHandler |
| Windows.UI.Xaml.Data.PropertyChangedEventArgs    | System.ComponentModel.PropertyChangedEventArgs    |

型によって複数のインターフェイスが実装される場合、メンバーのパラメーターの型または戻り値の型として実装されるインターフェイスをすべて使うことができます。 たとえば、**ディクショナリの &lt; int、string &gt; ** (Visual Basic**のディクショナリ (Of Integer, string)** ) を**IDictionary &lt; int、string &gt; **、 **ireadonlydictionary< &lt; int、string &gt; **、または**IEnumerable &lt; KeyValuePair &lt; TKey, TValue &gt; &gt; **として渡すか、返すことができます。

> [!IMPORTANT]
> JavaScript は、マネージド型が実装するインターフェイスのリストに最初に示されるインターフェイスを使用します。 たとえば、**辞書 &lt; int、 &gt; 文字列**を JavaScript コードに返す場合、戻り値の型として指定したインターフェイスに関係なく、 **IDictionary &lt; int, string &gt; **として表示されます。 つまり、後のインターフェイスにメンバーが最初のインターフェイスに含まれていない場合、そのメンバーは JavaScript では認識されません。

Windows ランタイムでは、Ikeyvaluepair<k, を使用することにより、 **IMap &lt; k、v &gt; ** 、 ** &lt; &gt; IMapView K**が反復処理されます。 これらのコードをマネージコードに渡すと、 **IDictionary &lt; Tkey, &gt; TValue** And **ireadonlydictionary< &lt; tkey, TValue &gt; **として表示されます。そのため、自然に、 **KeyValuePair &lt; tkey &gt; , TValue**を使用してそれらを列挙します。

インターフェイスがマネージド コード内に表示される方法によって、これらのインターフェイスを実装する型の表示方法が決まります。 たとえば、 **PropertySet**クラスは、マネージコードで**IDictionary &lt; TKey, TValue &gt; **として表示される**IMap &lt; K V &gt; **を実装します。 **PropertySet**は、 **IMap &lt; K &gt; , V**ではなく**IDictionary &lt; TKey, &gt; TValue**を実装しているように見えます。そのため、マネージコードでは、.Net ディクショナリの**add**メソッドのように動作する**add**メソッドがあるように見えます。 **Insert**メソッドがないように見えます。 この例については、 [C# または Visual Basic Windows ランタイムコンポーネントの作成に関するチュートリアルと、JavaScript からの呼び出し](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)に関するトピックを参照してください。

## <a name="passing-managed-types-to-the-windows-runtime"></a>Windows ランタイムへのマネージ型の引き渡し

前のセクションで説明したように、一部の Windows ランタイム型は、コンポーネントのメンバーのシグネチャに .NET 型として、または IDE で使用するときに Windows ランタイムメンバーのシグネチャに表示できます。 これらのメンバーに .NET 型を渡したり、コンポーネントのメンバーの戻り値として使用したりする場合は、対応する Windows ランタイム型として、もう一方の側のコードに表示されます。 コンポーネントが JavaScript から呼び出されたときの影響の例については、「 [C# または Visual Basic Windows ランタイムコンポーネントの作成」の](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)「コンポーネントからマネージ型を返す」セクションと「javascript からの呼び出し」を参照してください。

## <a name="overloaded-methods"></a>オーバー ロードされたメソッド

Windows ランタイムでは、メソッドはオーバーロードできます。 ただし、同じ数のパラメーターを持つ複数のオーバーロードを宣言する場合は、これらのオーバーロードのうち1つのみに [**windows.foundation.metadata.defaultoverloadattribute**](/uwp/api/windows.foundation.metadata.defaultoverloadattribute) 属性を適用する必要があります。 この属性が適用されるオーバーロードが、JavaScript から呼び出すことができる唯一のオーバーロードになります。 たとえば、次のコードでは、**int** (Visual Basic では **Integer**) を受け取るオーバーロードが既定のオーバーロードです。

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

> :JavaScript を使用すると、任意の値を **OverloadExample**に渡すことができ、パラメーターに必要な型に強制的に値が強制されます。 "42"、"42"、または42.3 を使用して **OverloadExample** を呼び出すことができますが、これらの値はすべて既定のオーバーロードに渡されます。 前の例の既定のオーバーロードでは、それぞれ0、42、および42が返されます。

コンストラクターに **DefaultOverloadAttribut**e 属性を適用することはできません。 クラスのすべてのコンストラクターは、異なる数のパラメーターを持つ必要があります。

## <a name="implementing-istringable"></a>IStringable の実装

Windows 8.1 以降、Windows ランタイムには **istringable** インターフェイスが含まれています。このインターフェイスは、単一のメソッドである **istringable**を使用して、 **オブジェクトの tostring**によって提供される基本的な書式設定のサポートを提供します。 Windows ランタイムコンポーネントにエクスポートされるパブリックマネージ型に **Istringable** を実装する場合は、次の制限が適用されます。

-   **Istringable**インターフェイスは、C# の次のコードのように、"クラスが実装する" 関係でのみ定義できます。

    ```cs
    public class NewClass : IStringable
    ```

    Visual Basic では、次のようになります。

    ```vb
    Public Class NewClass : Implements IStringable
    ```

-   インターフェイスに **Istringable** を実装することはできません。
-   パラメーターの型を **Istringable**として宣言することはできません。
-   **Istringable** は、メソッド、プロパティ、またはフィールドの戻り値の型にすることはできません。
-   次のようなメソッド定義を使用して、 **Istringable** の実装を基底クラスから隠すことはできません。

    ```cs
    public class NewClass : IStringable
    {
       public new string ToString()
       {
          return "New ToString in NewClass";
       }
    }
    ```

    代わりに、 **Istringable** の実装では、常に基底クラスの実装をオーバーライドする必要があります。 **ToString**実装は、厳密に型指定されたクラスインスタンスで呼び出すことによってのみ非表示にすることができます。

> [!NOTE]
> さまざまな条件下で、 **Istringable** を実装するマネージ型、またはその **ToString** 実装を隠すマネージ型にネイティブコードから呼び出すと、予期しない動作が生じる可能性があります。

## <a name="asynchronous-operations"></a>非同期操作

コンポーネントに非同期メソッドを実装するには、メソッド名の末尾に "Async" を追加し、非同期アクションまたは操作を表す Windows ランタイムインターフェイス ( **iasyncaction**、 **iasyncactionwithprogress &lt; tprogress &gt; **、 **IAsyncOperation &lt; &gt; tresult**、または**IAsyncOperationWithProgress &lt; tresult、tprogress &gt; **) のいずれかを返します。

.NET タスク ([**タスク**](/dotnet/api/system.threading.tasks.task)クラスと汎用[**タスク &lt; TResult &gt; **](/dotnet/api/system.threading.tasks.task-1)クラス) を使用して、非同期メソッドを実装できます。 C# または Visual Basic で記述された非同期メソッドから返されたタスクや、 [**タスクの Run**](/dotnet/api/system.threading.tasks.task.run) メソッドから返されたタスクなど、進行中の操作を表すタスクを返す必要があります。 コンストラクターを使ってタスクを作成する場合、その [Task.Start](/dotnet/api/system.threading.tasks.task.start) メソッドを呼び出してから戻す必要があります。

(Visual Basic) を使用するメソッドには `await` `Await` 、 `async` キーワード ( `Async` Visual Basic) が必要です。 このようなメソッドを Windows ランタイムコンポーネントから公開する場合は、 `async` **Run** メソッドに渡すデリゲートにキーワードを適用します。

取り消しや進行状況の報告をサポートしない非同期アクションと非同期操作では、タスクを適切なインターフェイスにラップするために、[WindowsRuntimeSystemExtensions.AsAsyncAction](/dotnet/api/system) または [AsAsyncOperation&lt;TResult&gt;](/dotnet/api/system) の拡張メソッドを使うことができます。 たとえば、次のコードでは、タスクを開始するために、 ** &lt; TResult &gt; **メソッドを使用して非同期メソッドを実装しています。 **AsAsyncOperation &lt; TResult &gt; **拡張メソッドは、タスクを Windows ランタイム非同期操作として返します。

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

次の JavaScript コードは、 [**WinJS**](/previous-versions/windows/apps/br211867(v=win.10)) オブジェクトを使用してメソッドを呼び出す方法を示しています。 then メソッドに渡される関数は、非同期呼び出しが完了したときに実行されます。 StringList パラメーターには、 **Downloadasstringasync** メソッドによって返される文字列のリストが含まれています。この関数は、必要な処理をすべて実行します。

```javascript
function asyncExample(id) {

    var result = SampleComponent.Example.downloadAsStringAsync(id).then(
        function (stringList) {
            // Place code that uses the returned list of strings here.
        });
}
```

キャンセルまたは進行状況レポートをサポートする非同期アクションおよび操作の場合は、 [**Asyncinfo**](/dotnet/api/system.runtime.interopservices.windowsruntime) クラスを使用して開始されたタスクを生成し、適切な Windows ランタイムインターフェイスのキャンセル機能と進行状況レポート機能を使用してタスクのキャンセル機能と進行状況レポート機能をフックします。 キャンセルと進行状況レポートの両方をサポートする例については、「 [C# または Visual Basic Windows ランタイムコンポーネントの作成と JavaScript からの呼び出し」のチュートリアル](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)を参照してください。

非同期メソッドがキャンセルまたは進行状況レポートをサポートしていない場合でも、 **Asyncinfo** クラスのメソッドを使用できることに注意してください。 Visual Basic のラムダ関数または C# の匿名メソッドを使用する場合は、トークンと[**Iprogress &lt; t &gt; **](/dotnet/api/system.iprogress-1)インターフェイスのパラメーターを指定しないでください。 C# のラムダ関数を使う場合は、トークンのパラメーターを指定しますが、無視されます。 前の例では、AsAsyncOperation tresult メソッドを使用して &lt; &gt; います。これは、代わりに[** &lt; tresult &gt; (Func &lt; CancellationToken, Task &lt; &gt; &gt; TResult**](/dotnet/api/system.runtime.interopservices.windowsruntime)) メソッドのオーバーロードを使用すると、次のようになります。

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

オプションでキャンセルまたは進行状況レポートをサポートする非同期メソッドを作成する場合は、キャンセルトークンまたは**Iprogress &lt; t &gt; **インターフェイスのパラメーターを持たないオーバーロードを追加することを検討してください。

## <a name="throwing-exceptions"></a>例外のスロー

Windows アプリ用 .NET に含まれている例外の型は、どれでもスローすることができます。 Windows ランタイム コンポーネントで独自のパブリック型の例外を宣言することはできませんが、非パブリック型を宣言し、スローすることはできます。

コンポーネントが例外を処理しない場合は、コンポーネントを呼び出したコードで対応する例外が発生します。 例外が呼び出し元に表示される方法は、呼び出し元の言語が Windows ランタイムをサポートする方法によって異なります。

-   JavaScript では、例外はオブジェクトとして表示され、例外メッセージがスタック トレースで置き換えられています。 Visual Studio でアプリをデバッグするとき、デバッガーの例外ダイアログ ボックスに、"WinRT 情報" として元のメッセージ テキストが表示されます。 JavaScript コードから元のメッセージ テキストにアクセスすることはできません。

    > **ヒント**。現在、スタック トレースにはマネージド型の例外が含まれますが、トレースを解析して例外の型を確認することはお勧めしません。 このセクションの後半で説明するように、代わりに HRESULT 値を使ってください。

-   C++ では、例外はプラットフォーム例外として表示されます。 マネージ例外の HResult プロパティを特定のプラットフォーム例外の HRESULT にマップできる場合は、特定の例外が使用されます。それ以外の場合は、 [**Platform:: COMException**](/cpp/cppcx/platform-comexception-class) 例外がスローされます。 マネージ例外のメッセージ テキストは、C++ コードでは利用できません。 特定のプラットフォーム例外がスローされた場合、その例外の型に関する既定のメッセージ テキストが表示されます。それ以外の場合は、メッセージ テキストは表示されません。 「[例外 (C++/CX)](/cpp/cppcx/exceptions-c-cx)」をご覧ください。
-   C# または Visual Basic では、例外は通常のマネージ例外です。

コンポーネントから例外をスローする場合、コンポーネントに固有の HResult プロパティ値を持つ非パブリック型の例外をスローすることにより、JavaScript や C++ の呼び出し元で例外を簡単に処理できるようになります。 HRESULT は、JavaScript の呼び出し元が例外オブジェクトの number プロパティを介して使用できます。また、 [**COMException:: HRESULT**](/cpp/cppcx/platform-comexception-class#hresult) プロパティを使用して C++ の呼び出し元に提供されます。

> [!NOTE]
> HRESULT には負の値を使用します。 正の値は成功と解釈されるので、JavaScript や C++ の呼び出し元で例外がスローされなくなります。

## <a name="declaring-and-raising-events"></a>イベントの宣言と発生

イベントのデータを保持する型を宣言する場合、EventArgs は Windows ランタイム型ではないので、EventArgs の代わりに Object から派生させます。 イベントの種類として[**EventHandler &lt; teventargs &gt; **](/dotnet/api/system.eventhandler-1)を使用し、イベント引数の型をジェネリック型引数として使用します。 .NET アプリケーションの場合と同じように、イベントを発生させます。

Windows ランタイム コンポーネントが JavaScript や C++ で使われる場合、イベントはそれらの言語で想定されている Windows ランタイムのイベント パターンに従います。 C# または Visual Basic のコンポーネントを使用する場合、イベントは通常の .NET イベントとして表示されます。 [C# または Visual Basic Windows ランタイムコンポーネントを作成し、JavaScript から呼び出す方法](./walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)については、「チュートリアル」を使用して説明します。

カスタム イベント アクセサーを実装する場合 (Visual Basic では **Custom** キーワードでイベントを宣言する場合) は、実装で Windows ランタイムのイベント パターンに従う必要があります。 「 [Windows ランタイムコンポーネント」の「カスタムイベントとイベントアクセサー](custom-events-and-event-accessors-in-windows-runtime-components.md)」を参照してください。 C# または Visual Basic コードからイベントを処理する場合、そのイベントは通常の .NET イベントであるように見えます。

## <a name="next-steps"></a>次の手順

ユーザーが独自に使う Windows ランタイム コンポーネントを作成した後で、そのコンポーネントにカプセル化されている機能が他の開発者の役に立つ場合があります。 他の開発者に配布するためにコンポーネントをパッケージ化する方法は 2 つあります。 「[マネージ Windows ランタイム コンポーネントの配布](/previous-versions/windows/apps/jj614475(v=vs.140))」をご覧ください。

Visual Basic と C# 言語の機能、および Windows ランタイムの .NET サポートの詳細については、「 [Visual Basic および c# 言語リファレンス](/visualstudio/welcome-to-visual-studio-2015?view=vs-2015)」を参照してください。


## <a name="troubleshooting"></a>トラブルシューティング

| 症状 | 解決方法 |
|---------|--------|
|C++/WinRT アプリで、XAML を使用する [C# Windows ランタイムコンポーネント]() を使用すると、コンパイラによって "' MyNamespace_XamlTypeInfo ' の形式のエラーが生成*されます: は ' WinRT:: MyNamespace ' のメンバーではありません。* &mdash; *MyNamespace* は Windows ランタイムコンポーネントの名前空間の名前です。 | 使用中の `pch.h` C++/WinRT アプリので、必要に応じ `#include <winrt/MyNamespace.MyNamespace_XamlTypeInfo.h>` &mdash; て*MyNamespace*を置き換えます。 |

## <a name="related-topics"></a>関連トピック
* [UWP アプリ用 .NET](/dotnet/api/index?view=dotnet-uwp-10.0)
* [C# または Visual Basic Windows ランタイム コンポーネントの作成と JavaScript からの呼び出しに関するチュートリアル](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)