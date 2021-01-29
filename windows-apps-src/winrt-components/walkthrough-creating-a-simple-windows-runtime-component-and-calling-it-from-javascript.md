---
title: C# または Visual Basic Windows ランタイム コンポーネントの作成と JavaScript からの呼び出しに関するチュートリアル
description: このチュートリアルでは、Visual Basic または C# で .NET を使用して独自の Windows ランタイム型を作成する方法、Windows ランタイムコンポーネントにパッケージ化する方法、および JavaScript を使用して Windows 用にビルドされた UWP アプリケーションからコンポーネントを呼び出す方法について説明します。
ms.assetid: 1565D86C-BF89-4EF3-81FE-35367DB8D671
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a1d931e98f21160badb7a8c2603a580c2adfd682
ms.sourcegitcommit: d51c3dd64d58c7fa9513ba20e736905f12df2a9a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2021
ms.locfileid: "98988744"
---
# <a name="walkthrough-of-creating-a-c-or-visual-basic-windows-runtime-component-and-calling-it-from-javascript"></a>C# または Visual Basic Windows ランタイム コンポーネントの作成と JavaScript からの呼び出しに関するチュートリアル

このチュートリアルでは、Visual Basic または C# と共に .NET を使用して、Windows ランタイムコンポーネントにパッケージ化された独自の Windows ランタイム型を作成する方法、および JavaScript ユニバーサル Windows プラットフォーム (UWP) アプリからそのコンポーネントを呼び出す方法について説明します。

Visual Studio を使用すると、C# または Visual Basic で記述された Windows ランタイムコンポーネント (WRC) プロジェクト内に独自のカスタム Windows ランタイム型を簡単に作成して配置し、JavaScript アプリケーションプロジェクトからその WRC を参照し、そのアプリケーションからカスタム型を使用することができます。

内部的には、Windows ランタイム型は、UWP アプリケーションで許可されているすべての .NET 機能を使用できます。

> [!NOTE]
> 詳細については、「 [C# および Visual Basic を使用したコンポーネントの Windows ランタイム](creating-windows-runtime-components-in-csharp-and-visual-basic.md) 」および「 [UWP アプリ用 .net の概要](/dotnet/api/index?view=dotnet-uwp-10.0)」を参照してください。

外部的には、型のメンバーは、パラメーターと戻り値に対して Windows ランタイム型のみを公開できます。 ソリューションをビルドすると、Visual Studio によって .NET WRC プロジェクトがビルドされ、Windows メタデータ (winmd) ファイルを作成するビルドステップが実行されます。 これが、Visual Studio がアプリに含める Windows ランタイム コンポーネントです。

> [!NOTE]
> .NET では、プリミティブデータ型やコレクション型などの一般的に使用される .NET 型が、同等の Windows ランタイムに自動的にマップされます。 これらの .NET 型は、Windows ランタイムコンポーネントのパブリックインターフェイスで使用できます。また、コンポーネントのユーザーには、対応する Windows ランタイムの種類として表示されます。 「 [C# および Visual Basic でのコンポーネントの Windows ランタイム」を](creating-windows-runtime-components-in-csharp-and-visual-basic.md)参照してください。

## <a name="prerequisites"></a>前提条件:

- Windows 10
- [Microsoft Visual Studio](https://visualstudio.microsoft.com/downloads/)

> [!NOTE]
> JavaScript を使用したユニバーサル Windows プラットフォーム (UWP) プロジェクトは、Visual Studio 2019 ではサポートされていません。 「 [Visual Studio 2019 での JavaScript と TypeScript](/visualstudio/javascript/javascript-in-vs-2019#projects)」を参照してください。 このトピックに従うには、Visual Studio 2017 を使用することをお勧めします。 「 [Visual Studio 2017 での JavaScript」を](/visualstudio/javascript/javascript-in-vs-2017)参照してください。

## <a name="creating-a-simple-windows-runtime-class"></a>単純な Windows ランタイム クラスの作成

このセクションでは、JavaScript UWP アプリケーションを作成し、Visual Basic または C# Windows ランタイムコンポーネントプロジェクトをソリューションに追加します。 Windows ランタイム型を定義する方法、JavaScript から型のインスタンスを作成する方法、および静的メンバーとインスタンスメンバーを呼び出す方法を示します。 コンポーネントにフォーカスを保持するために、サンプルアプリのビジュアル表示は意図的に低キーです。

1. Visual Studio で新しい JavaScript プロジェクトを作成します。メニュー バーで、**[ファイル]、[新規作成]、[プロジェクト]** の順にクリックします。 **[新しいプロジェクト]** ダイアログ ボックスの **[インストールされたテンプレート]** セクションで **[JavaScript]** を選択し、**[Windows]**、**[ユニバーサル]** の順に選択します  ([Windows] が利用できない場合は、Windows 8 以降を使っていることを確認してください)。**[新しいアプリケーション]** テンプレートを選び、プロジェクト名として「SampleApp」と入力します。
2.  コンポーネント プロジェクトを作成します。ソリューション エクスプローラーで、SampleApp ソリューションのショートカット メニューを開き、**[追加]**、**[新しいプロジェクト]** の順にクリックして、新しい C# プロジェクトまたは Visual Basic プロジェクトをソリューションに追加します。 **[新しいプロジェクトの追加]** ダイアログ ボックスの **[インストールされたテンプレート]** セクションで、**[Visual Basic]** または **[Visual C#]** を選択し、**[Windows]**、**[ユニバーサル]** の順に選択します。 **[Windows ランタイム コンポーネント]** テンプレートを選択し、プロジェクト名として「**SampleComponent**」と入力します。
3.  クラス名を「**Example**」に変更します。 既定では、クラスは **public sealed** (Visual Basic では **Public NotInheritable**) としてマークされていることに注意してください。 コンポーネントから公開するすべての Windows ランタイム クラスをシールする必要があります。
4.  **static** メソッド (Visual Basic では **Shared** メソッド) とインスタンス プロパティの 2 つの単純なメンバーをクラスに追加します。

    > [!div class="tabbedCodeSnippets"]
    > ```csharp
    > namespace SampleComponent
    > {
    >     public sealed class Example
    >     {
    >         public static string GetAnswer()
    >         {
    >             return "The answer is 42.";
    >         }
    >
    >         public int SampleProperty { get; set; }
    >     }
    > }
    > ```
    > ```vb
    > Public NotInheritable Class Example
    >     Public Shared Function GetAnswer() As String
    >         Return "The answer is 42."
    >     End Function
    >
    >     Public Property SampleProperty As Integer
    > End Class
    > ```

5.  省略可能: 新しく追加したメンバーで IntelliSense を有効にするには、ソリューション エクスプローラーで SampleComponent プロジェクトのショートカット メニューを開き、**[ビルド]** を選びます。
6.  ソリューション エクスプローラーの JavaScript プロジェクトで、**[参照]** のショートカット メニューを開き、**[参照の追加]** をクリックして **[参照マネージャー]** を開きます。 **[プロジェクト]** をクリックし、**[ソリューション]** をクリックします。 SampleComponent プロジェクトのチェック ボックスをオンにし、**[OK]** をクリックして参照を追加します。

## <a name="call-the-component-from-javascript"></a>JavaScript からコンポーネントを呼び出す

JavaScript から Windows ランタイム型を使うには、(プロジェクトの js フォルダーにある) default.js ファイルで、Visual Studio テンプレートで提供される匿名関数に次のコードを追加します。 匿名関数は、app.oncheckpoint イベント ハンドラーの後の app.start 呼び出しの前にあります。

```javascript
var ex;

function basics1() {
   document.getElementById('output').innerHTML =
        SampleComponent.Example.getAnswer();

    ex = new SampleComponent.Example();

   document.getElementById('output').innerHTML += "<br/>" +
       ex.sampleProperty;

}

function basics2() {
    ex.sampleProperty += 1;
    document.getElementById('output').innerHTML += "<br/>" +
        ex.sampleProperty;
}
```

各メンバー名の最初の文字が大文字から小文字に変更されていることに注意してください。 この変換は、Windows ランタイムの自然な使い方を可能にするために JavaScript が提供するサポートの一部です。 名前空間とクラス名は pascal 規約に従って大文字小文字が使い分けられます。 すべて小文字のイベント名を除き、メンバー名は camel 規約に従っています。 「[JavaScript での Windows ランタイムの使用](/scripting/jswinrt/using-the-windows-runtime-in-javascript)」をご覧ください。 camel 規約の規則はわかりにくい場合があります。 通常、一連の先頭の大文字は小文字で表示されますが、3 つの大文字の後に小文字が続く場合は、最初の 2 つの文字だけが小文字で表示されます。たとえば、IDStringKind という名前のメンバーは idStringKind と表示されます。 Visual Studio では、Windows ランタイム コンポーネント プロジェクトをビルドし、JavaScript プロジェクトで IntelliSense を使って正しい大文字小文字の区別を確認できます。

同様に、.NET では、マネージコードでの Windows ランタイムの自然な使用を有効にすることがサポートされています。 この点については、この記事の後続のセクションで説明します。また、「 [C# および Visual Basic を使用したコンポーネントの Windows ランタイム](creating-windows-runtime-components-in-csharp-and-visual-basic.md) 」および「 [UWP アプリと Windows ランタイムの .net サポート](/dotnet/standard/cross-platform/support-for-windows-store-apps-and-windows-runtime)」の記事をご覧ください。

## <a name="create-a-simple-user-interface"></a>単純なユーザー インターフェイスを作成する

JavaScript プロジェクトで、default.html ファイルを開き、次のコードに示すように本文を更新します。 このコードには、サンプル アプリのコントロールの完全なセットが含まれています。また、クリック イベントの関数名も指定されています。

> **注**  アプリを初めて実行するときは、Basics1 ボタンと Basics2 ボタンだけがサポートされています。

```html
<body>
            <div id="buttons">
            <button id="button1" >Basics 1</button>
            <button id="button2" >Basics 2</button>

            <button id="runtimeButton1">Runtime 1</button>
            <button id="runtimeButton2">Runtime 2</button>

            <button id="returnsButton1">Returns 1</button>
            <button id="returnsButton2">Returns 2</button>

            <button id="events1Button">Events 1</button>

            <button id="btnAsync">Async</button>
            <button id="btnCancel" disabled="disabled">Cancel Async</button>
            <progress id="primeProg" value="25" max="100" style="color: yellow;"></progress>
        </div>
        <div id="output">
        </div>
</body>
```

JavaScript プロジェクトで、css フォルダーの default.css を開きます。 body セクションを次のように変更し、ボタンのレイアウトと出力テキストの配置を制御するスタイルを追加します。

```css
body
{
    -ms-grid-columns: 1fr;
    -ms-grid-rows: 1fr 14fr;
    display: -ms-grid;
}

#buttons {
    -ms-grid-rows: 1fr;
    -ms-grid-columns: auto;
    -ms-grid-row-align: start;
}
#output {
    -ms-grid-row: 2;
    -ms-grid-column: 1;
}
```

次に、default.js で app.onactivated 内の processAll 呼び出しに then 句を追加して、イベント リスナーの登録コードを追加します。 setPromise を呼び出す既存のコード行を置き換え、次のコードに変更します。

```javascript
args.setPromise(WinJS.UI.processAll().then(function () {
    var button1 = document.getElementById("button1");
    button1.addEventListener("click", basics1, false);
    var button2 = document.getElementById("button2");
    button2.addEventListener("click", basics2, false);
}));
```

HTML コントロールにイベントを追加するときは、HTML でクリック イベント ハンドラーを直接追加するのではなく、この方法をお勧めします。 「 [Hello, World "アプリ (JS) を作成する」を](/windows/apps/get-started/)参照してください。

## <a name="build-and-run-the-app"></a>アプリのビルドと実行

ビルドする前に、お使いのコンピューターに応じて、すべてのプロジェクトのターゲット プラットフォームを ARM、x64、または x86 に変更します。

ソリューションをビルドして実行するには、F5 キーを押します  (SampleComponent が未定義であることを示す実行時エラー メッセージが表示された場合は、クラス ライブラリ プロジェクトへの参照がありません)。

Visual Studio では、まずクラス ライブラリをコンパイルし、[Winmdexp.exe (Windows ランタイム メタデータ エクスポート ツール)](/dotnet/framework/tools/winmdexp-exe-windows-runtime-metadata-export-tool) を実行する MSBuild タスクを実行して、Windows ランタイム コンポーネントを作成します。 コンポーネントは、マネージ コードと、コードを説明する Windows メタデータの両方を含む .winmd ファイルに含まれています。 Windows ランタイム コンポーネントで無効なコードを作成した場合、WinMdExp.exe によってビルド エラー メッセージが生成され、Visual Studio IDE に表示されます。 Visual Studio によって、コンポーネントが UWP アプリケーションのアプリケーションパッケージ (.appx ファイル) に追加され、適切なマニフェストが生成されます。

[Basics 1] ボタンをクリックして GetAnswer 静的メソッドからの戻り値を出力領域に割り当て、Example クラスのインスタンスを作成し、SampleProperty プロパティの値を出力領域に表示します。 出力は次のように表示されます。

``` syntax
"The answer is 42."
0
```

[Basics 2] ボタンをクリックして SampleProperty プロパティの値を増やし、新しい値を出力領域に表示します。 文字列や数値などのプリミティブ型は、パラメーターの型や戻り値の型として使用でき、マネージ コードと JavaScript の間で渡すことができます。 JavaScript の数値は倍精度浮動小数点形式で保存されるため、.NET Framework の数値型に変換されます。

> **注**  既定では、ブレークポイントは JavaScript コードでのみ設定できます。 Visual Basic または C# コードをデバッグするには、「C# および Visual Basic での Windows ランタイムコンポーネントの作成」を参照してください。

デバッグを停止してアプリを閉じるには、アプリから Visual Studio に切り替え、Shift キーを押しながら F5 キーを押します。

## <a name="using-the-windows-runtime-from-javascript-and-managed-code"></a>JavaScript とマネージ コードからの Windows ランタイムの使用

Windows ランタイムは、JavaScript またはマネージ コードから呼び出すことができます。 Windows ランタイム オブジェクトはこの 2 つの間で受け渡すことができ、イベントはどちらの側からでも処理できます。 ただし、2つの環境の Windows ランタイム型を使用する方法は、いくつかの詳細に違いがあります。これは、JavaScript と .NET では Windows ランタイムが異なる方法でサポートされるためです。 次の例では、[Windows.Foundation.Collections.PropertySet](/uwp/api/windows.foundation.collections.propertyset) クラスを使用して、これらの違いを示します。 この例では、マネージ コードで PropertySet コレクションのインスタンスを作成し、コレクションの変更を追跡するイベント ハンドラーを登録します。 次に、コレクションを取得し、独自のイベント ハンドラーを登録して、コレクションを使用する JavaScript コードを追加します。 最後に、マネージ コードからコレクションに変更を加え、マネージ例外を処理する JavaScript を示すメソッドを追加します。

> **重要:** この例では、UI スレッドでイベントを発生させます。 非同期呼び出しなどでバックグラウンド スレッドからイベントを発生させる場合は、JavaScript がそのイベントを処理できるように、追加の作業が必要になります。 詳細については、「 [Windows ランタイムコンポーネントでのイベントの発生](raising-events-in-windows-runtime-components.md)」を参照してください。

SampleComponent プロジェクトで、PropertySetStats という名前の新しい **public sealed** クラス (Visual Basic では **Public NotInheritable** クラス) を追加します。 このクラスは、PropertySet コレクションをラップし、MapChanged イベントを処理します。 イベント ハンドラーは発生した各種変更の数を追跡し、DisplayStats メソッドは HTML 形式のレポートを生成します。 追加の **using** ステートメント (Visual Basic では **Imports** ステートメント) に注意してください。このステートメントで既存の **using** ステートメントを上書きするのではなく、このステートメントを既存の using ステートメントに追加する必要があります。

> [!div class="tabbedCodeSnippets"]
> ```csharp
> using Windows.Foundation.Collections;
>
> namespace SampleComponent
> {
>     public sealed class PropertySetStats
>     {
>         private PropertySet _ps;
>         public PropertySetStats()
>         {
>             _ps = new PropertySet();
>             _ps.MapChanged += this.MapChangedHandler;
>         }
>
>         public PropertySet PropertySet { get { return _ps; } }
>
>         int[] counts = { 0, 0, 0, 0 };
>         private void MapChangedHandler(IObservableMap<string, object> sender,
>             IMapChangedEventArgs<string> args)
>         {
>             counts[(int)args.CollectionChange] += 1;
>         }
>
>         public string DisplayStats()
>         {
>             StringBuilder report = new StringBuilder("<br/>Number of changes:<ul>");
>             for (int i = 0; i < counts.Length; i++)
>             {
>                 report.Append("<li>" + (CollectionChange)i + ": " + counts[i] + "</li>");
>             }
>             return report.ToString() + "</ul>";
>         }
>     }
> }
> ```
> ```vb
> Imports System.Text
>
> Public NotInheritable Class PropertySetStats
>     Private _ps As PropertySet
>     Public Sub New()
>         _ps = New PropertySet()
>         AddHandler _ps.MapChanged, AddressOf Me.MapChangedHandler
>     End Sub
>
>     Public ReadOnly Property PropertySet As PropertySet
>         Get
>             Return _ps
>         End Get
>     End Property
>
>     Dim counts() As Integer = {0, 0, 0, 0}
>     Private Sub MapChangedHandler(ByVal sender As IObservableMap(Of String, Object),
>         ByVal args As IMapChangedEventArgs(Of String))
>
>         counts(CInt(args.CollectionChange)) += 1
>     End Sub
>
>     Public Function DisplayStats() As String
>         Dim report As New StringBuilder("<br/>Number of changes:<ul>")
>         For i As Integer = 0 To counts.Length - 1
>             report.Append("<li>" & CType(i, CollectionChange).ToString() &
>                           ": " & counts(i) & "</li>")
>         Next
>         Return report.ToString() & "</ul>"
>     End Function
> End Class
> ```

イベントハンドラーは、使い慣れた .NET Framework イベントパターンに従います。ただし、イベントの送信者 (この場合は PropertySet オブジェクト) は、IObservableMap &lt; 文字列、オブジェクト &gt; インターフェイス (Visual Basic の IObservableMap (String, object)) にキャストされます。これは、Windows ランタイムインターフェイス[IObservableMap &lt; K, &gt; V](/uwp/api/Windows.Foundation.Collections.IObservableMap_K_V_)のインスタンス化です。  (必要に応じて、送信元をその型にキャストできます)。また、イベント引数は、オブジェクトとしてではなくインターフェイスとして示されます。

default.js ファイルで、次のように runtime1 関数を追加します。 このコードでは、PropertySetStats オブジェクトを作成し、PropertySet コレクションを取得します。また、独自のイベント ハンドラーである onMapChanged 関数を追加して、MapChanged イベントを処理します。 コレクションの変更後、runtime1 は DisplayStats メソッドを呼び出して、変更の種類の概要を表示します。

```javascript
var propertysetstats;

function runtime1() {
    document.getElementById('output').innerHTML = "";

    propertysetstats = new SampleComponent.PropertySetStats();
    var propertyset = propertysetstats.propertySet;

    propertyset.addEventListener("mapchanged", onMapChanged);

    propertyset.insert("FirstProperty", "First property value");
    propertyset.insert("SuperfluousProperty", "Unnecessary property value");
    propertyset.insert("AnotherProperty", "A property value");

    propertyset.insert("SuperfluousProperty", "Altered property value")
    propertyset.remove("SuperfluousProperty");

    document.getElementById('output').innerHTML +=
        propertysetstats.displayStats();
}

function onMapChanged(change) {
    var result
    switch (change.collectionChange) {
        case Windows.Foundation.Collections.CollectionChange.reset:
            result = "All properties cleared";
            break;
        case Windows.Foundation.Collections.CollectionChange.itemInserted:
            result = "Inserted " + change.key + ": '" +
                change.target.lookup(change.key) + "'";
            break;
        case Windows.Foundation.Collections.CollectionChange.itemRemoved:
            result = "Removed " + change.key;
            break;
        case Windows.Foundation.Collections.CollectionChange.itemChanged:
            result = "Changed " + change.key + " to '" +
                change.target.lookup(change.key) + "'";
            break;
        default:
            break;
     }

     document.getElementById('output').innerHTML +=
         "<br/>" + result;
}
```

JavaScript で Windows ランタイム イベントを処理する方法は、.NET Framework コードで処理する方法とは大きく異なります。 JavaScript イベント ハンドラーは引数を 1 つだけ受け取ります。 Visual Studio デバッガーでこのオブジェクトを表示した場合、最初のプロパティが送信元です。 イベント引数インターフェイスのメンバーも、このオブジェクトに直接表示されます。

アプリを実行するには、F5 キーを押します。 クラスがシールされていない場合、"Exporting unsealed type 'SampleComponent.Example' is not currently supported. Please mark it as sealed." (unsealed 型 'SampleComponent.Example' のエクスポートは現在サポートされていません。sealed としてマークしてください。) というエラー メッセージが表示されます。

**[Runtime 1]** ボタンをクリックします。 要素が追加または変更されると、イベント ハンドラーが変更を表示し、最後に DisplayStats メソッドが呼び出されて変更の数の概要が生成されます。 デバッグを停止してアプリを閉じるには、Visual Studio に戻り、Shift キーを押しながら F5 キーを押します。

マネージ コードから PropertySet コレクションにさらに 2 つの項目を追加するには、次のコードを PropertySetStats クラスに追加します。

> [!div class="tabbedCodeSnippets"]
> ```csharp
> public void AddMore()
> {
>     _ps.Add("NewProperty", "New property value");
>     _ps.Add("AnotherProperty", "A property value");
> }
> ```
> ```vb
> Public Sub AddMore()
>     _ps.Add("NewProperty", "New property value")
>     _ps.Add("AnotherProperty", "A property value")
> End Sub
> ```

このコードは、2 つの環境での Windows ランタイム型の使用方法のもう 1 つの違いを示しています。 このコードを自分で入力すると、IntelliSense が JavaScript コードで使用した insert メソッドを表示しないことがわかります。 代わりに、.NET のコレクションでよく見られる Add メソッドを示しています。 これは、一般的に使用されるコレクションインターフェイスの名前は異なりますが、Windows ランタイムと .NET でも同様の機能を持つためです。 マネージ コードでこれらのインターフェイスを使用すると、.NET Framework の対応するインターフェイスとして表示されます。 これについ [ては、「C# および Visual Basic でのコンポーネントの Windows ランタイム](creating-windows-runtime-components-in-csharp-and-visual-basic.md)」で説明されています。 JavaScript で同じインターフェイスを使用した場合、Windows ランタイムからの変更点は、メンバー名の先頭の大文字が小文字になることだけです。

最後に、例外処理で AddMore メソッドを呼び出すには、runtime2 関数を default.js に追加します。

```javascript
function runtime2() {
   try {
      propertysetstats.addMore();
    }
   catch(ex) {
       document.getElementById('output').innerHTML +=
          "<br/><b>" + ex + "<br/>";
   }

   document.getElementById('output').innerHTML +=
       propertysetstats.displayStats();
}
```

前と同様に、イベント ハンドラーの登録コードを追加します。

```javascript
var runtimeButton1 = document.getElementById("runtimeButton1");
runtimeButton1.addEventListener("click", runtime1, false);
var runtimeButton2 = document.getElementById("runtimeButton2");
runtimeButton2.addEventListener("click", runtime2, false);
```

アプリを実行するには、F5 キーを押します。 **[Runtime 1]**、**[Runtime 2]** の順にクリックします。 JavaScript イベント ハンドラーはコレクションの最初の変更を報告します。 ただし、2 番目の変更には重複するキーがあります。 .NET Framework ディクショナリのユーザーは、Add メソッドが例外をスローすることを想定しており、実際にそのとおりになります。 JavaScript は .NET 例外を処理します。

> **注**  JavaScript コードから例外のメッセージを表示することはできません。 メッセージ テキストはスタック トレースに置き換えられます。 詳細については、「C# および Visual Basic での Windows ランタイムコンポーネントの作成」の「例外のスロー」を参照してください。

一方、JavaScript が重複するキーで insert メソッドを呼び出したときには、項目の値が変更されました。 動作の違いは、「 [C# および Visual Basic でのコンポーネントの Windows ランタイム](creating-windows-runtime-components-in-csharp-and-visual-basic.md)」で説明されているように、JavaScript と .net が Windows ランタイムをサポートするさまざまな方法です。

## <a name="returning-managed-types-from-your-component"></a>コンポーネントからマネージ型を返す

前述のように、JavaScript コードと C# または Visual Basic のコード間で、ネイティブの Windows ランタイム型を自由に受け渡すことができます。 ほとんどの場合、型名とメンバー名はどちらの場合も同じになります (JavaScript ではメンバー名が小文字で始まる点を除きます)。 ただし、前のセクションでは、マネージ コードで PropertySet クラスのメンバーが異なっていました  (たとえば、JavaScript では insert メソッドを呼び出しましたが、.NET コードでは Add メソッドを呼び出しました)。このセクションでは、これらの違いが JavaScript に渡される .NET Framework 型にどのように影響するかについて説明します。

コンポーネントで作成した Windows ランタイム型または JavaScript からコンポーネントに渡された Windows ランタイム型を返すだけでなく、マネージ コードで作成されたマネージ型を、対応する Windows ランタイム型と同様に JavaScript に返すこともできます。 ランタイム クラスの最初の簡単な例でも、メンバーのパラメーターと戻り値の型は、Visual Basic または C# のプリミティブ型 (.NET Framework の型) でした。 コレクションでこれを示すために、Example クラスに次のコードを追加して、整数でインデックス付けされた文字列のジェネリック ディクショナリを返すメソッドを作成します。

> [!div class="tabbedCodeSnippets"]
> ```csharp
> public static IDictionary<int, string> GetMapOfNames()
> {
>     Dictionary<int, string> retval = new Dictionary<int, string>();
>     retval.Add(1, "one");
>     retval.Add(2, "two");
>     retval.Add(3, "three");
>     retval.Add(42, "forty-two");
>     retval.Add(100, "one hundred");
>     return retval;
> }
> ```
> ```vb
> Public Shared Function GetMapOfNames() As IDictionary(Of Integer, String)
>     Dim retval As New Dictionary(Of Integer, String)
>     retval.Add(1, "one")
>     retval.Add(2, "two")
>     retval.Add(3, "three")
>     retval.Add(42, "forty-two")
>     retval.Add(100, "one hundred")
>     Return retval
> End Function
> ```

ディクショナリは、[Dictionary&lt;TKey, TValue&gt;](/dotnet/api/system.collections.generic.dictionary-2) によって実装され、Windows ランタイム インターフェイスにマップされるインターフェイスとして返される必要があることに注意してください。 この例では、インターフェイスは IDictionary&lt;int, string&gt; (Visual Basic では IDictionary(Of Integer, String)) です。 Windows ランタイム型の IMap&lt;int, string&gt; がマネージ コードに渡されると、IDictionary&lt;int, string&gt; として表示され、マネージ型が JavaScript に渡されると、この逆になります。

**重要:** マネージ型が複数のインターフェイスを実装している場合、JavaScript はリストの最初に表示されるインターフェイスを使用します。 たとえば、Dictionary&lt;int, string&gt; を JavaScript コードに返した場合、戻り値の型としてどのインターフェイスを指定しても、IDictionary&lt;int, string&gt; として表示されます。 つまり、後のインターフェイスにメンバーが最初のインターフェイスに含まれていない場合、そのメンバーは JavaScript では認識されません。

 

新しいメソッドをテストしてディクショナリを使用するには、returns1 関数と returns2 関数を default.js に追加します。

```javascript
var names;

function returns1() {
    names = SampleComponent.Example.getMapOfNames();
    document.getElementById('output').innerHTML = showMap(names);
}

var ct = 7;

function returns2() {
    if (!names.hasKey(17)) {
        names.insert(43, "forty-three");
        names.insert(17, "seventeen");
    }
    else {
        var err = names.insert("7", ct++);
        names.insert("forty", "forty");
    }
    document.getElementById('output').innerHTML = showMap(names);
}

function showMap(map) {
    var item = map.first();
    var retval = "<ul>";

    for (var i = 0, len = map.size; i < len; i++) {
        retval += "<li>" + item.current.key + ": " + item.current.value + "</li>";
        item.moveNext();
    }
    return retval + "</ul>";
}
```

イベント登録コードを、他のイベント登録コードと同じ then ブロックに追加します。

```javascript
var returnsButton1 = document.getElementById("returnsButton1");
returnsButton1.addEventListener("click", returns1, false);
var returnsButton2 = document.getElementById("returnsButton2");
returnsButton2.addEventListener("click", returns2, false);
```

この JavaScript コードには、注目すべき点がいくつかあります。 まず、ディクショナリの内容を HTML で表示する showMap 関数が含まれています。 showMap のコードのイテレーション パターンに注目してください。 .NET では、汎用 IDictionary インターフェイスに最初のメソッドがなく、サイズがサイズのメソッドではなく Count プロパティによって返されます。 JavaScript に対して、IDictionary&lt;int, string&gt; は Windows ランタイム型の IMap&lt;int, string&gt; として表示されます  ([IMap&lt;K,V&gt;](/uwp/api/Windows.Foundation.Collections.IMap_K_V_) インターフェイスをご覧ください)。

前の例のように、returns2 関数では、JavaScript が Insert メソッド (JavaScript では insert) を呼び出して項目をディクショナリに追加します。

アプリを実行するには、F5 キーを押します。 ディクショナリの初期内容を作成して表示するには、**[Returns 1]** ボタンをクリックします。 ディクショナリにさらに 2 つのエントリを追加するには、**[Returns 2]** ボタンをクリックします。 Dictionary&lt;TKey, TValue&gt; から想定されるように、エントリは挿入順に表示されます。 エントリを並べ替える場合は、GetMapOfNames から SortedDictionary&lt;int, string&gt; を返すことができます  (前の例で使われている PropertySet クラスの内部構成は、Dictionary&lt;TKey, TValue&gt; とは異なります)。

JavaScript は厳密に型指定された言語ではないため、厳密に型指定されたジェネリック コレクションを使うと、予期しない結果になる場合があります。 **[Returns 2]** ボタンをもう一度クリックします。 JavaScript は "7" を数値 7 に強制的に変換し、ct に格納された数値 7 を文字列に変換します。 さらに、この文字列 "forty" を強制的にゼロに変換します。 しかし、これは始まりにすぎません。 **[Returns 2]** ボタンをさらに数回クリックします。 マネージ コードでは、値が正しい型にキャストされていても、Add メソッドは重複キーの例外を生成します。 これに対して、Insert メソッドは既存のキーに関連付けられている値を更新し、新しいキーがディクショナリに追加されたかどうかを示すブール値を返します。 キー 7 に関連付けられている値が変わり続けるのはこのためです。

予期しない動作がもう 1 つあります。未割り当ての JavaScript 変数を文字列引数として渡すと、"undefined" という文字列が返されます。 つまり、.NET Framework のコレクション型を JavaScript コードに渡すときには注意が必要です。

> **注:** 大量のテキストを連結する場合は、showMap 関数に示すように、コードを .NET Framework メソッドに移動し、StringBuilder クラスを使うことで、この連結をより効率的に実行できます。

Windows ランタイム コンポーネントから独自のジェネリック型を公開することはできませんが、次のようなコードを使って Windows ランタイム クラスの .NET Framework ジェネリック コレクションを返すことができます。

> [!div class="tabbedCodeSnippets"]
> ```csharp
> public static object GetListOfThis(object obj)
> {
>     Type target = obj.GetType();
>     return Activator.CreateInstance(typeof(List<>).MakeGenericType(target));
> }
> ```
> ```vb
> Public Shared Function GetListOfThis(obj As Object) As Object
>     Dim target As Type = obj.GetType()
>     Return Activator.CreateInstance(GetType(List(Of )).MakeGenericType(target))
> End Function
> ```

List&lt;T&gt; は IList&lt;T&gt; を実装しており、JavaScript では Windows ランタイム型の IVector&lt;T&gt; として表示されます。

## <a name="declaring-events"></a>イベントの宣言


イベントを宣言するには、.NET Framework の標準のイベント パターン、または Windows ランタイムで使用される他のパターンを使うことができます。 .NET Framework は、System.EventHandler&lt;TEventArgs&gt; デリゲートと Windows ランタイムの EventHandler&lt;T&gt; デリゲート間の等価性をサポートするので、.NET Framework の標準パターンを実装するときは、EventHandler&lt;TEventArgs&gt; を使うと便利です。 この動作を確認するために、SampleComponent プロジェクトにクラスの次の組み合わせを追加します。

> [!div class="tabbedCodeSnippets"]
> ```csharp
> namespace SampleComponent
> {
>     public sealed class Eventful
>     {
>         public event EventHandler<TestEventArgs> Test;
>         public void OnTest(string msg, long number)
>         {
>             EventHandler<TestEventArgs> temp = Test;
>             if (temp != null)
>             {
>                 temp(this, new TestEventArgs()
>                 {
>                     Value1 = msg,
>                     Value2 = number
>                 });
>             }
>         }
>     }
>
>     public sealed class TestEventArgs
>     {
>         public string Value1 { get; set; }
>         public long Value2 { get; set; }
>     }
> }
> ```
> ```vb
> Public NotInheritable Class Eventful
>     Public Event Test As EventHandler(Of TestEventArgs)
>     Public Sub OnTest(ByVal msg As String, ByVal number As Long)
>         RaiseEvent Test(Me, New TestEventArgs() With {
>                             .Value1 = msg,
>                             .Value2 = number
>                             })
>     End Sub
> End Class
>
> Public NotInheritable Class TestEventArgs
>     Public Property Value1 As String
>     Public Property Value2 As Long
> End Class
> ```

Windows ランタイムでイベントを公開すると、イベント引数クラスは System.Object から継承します。 EventArgs は Windows ランタイム型ではないため、.NET の場合と同様に、system.object から継承することはありません。

イベントのカスタム イベント アクセサー (Visual Basic では **Custom** キーワード) を宣言する場合は、Windows ランタイムのイベント パターンを使用する必要があります。 「 [Windows ランタイムコンポーネント」の「カスタムイベントとイベントアクセサー](custom-events-and-event-accessors-in-windows-runtime-components.md)」を参照してください。

Test イベントを処理するには、events1 関数を default.js に追加します。 events1 関数は Test イベントのイベント ハンドラー関数を作成し、OnTest メソッドを即座に呼び出してイベントを発生させます。 イベント ハンドラーの本体にブレークポイントを配置すると、単一のパラメーターに渡されるオブジェクトにソース オブジェクトと TestEventArgs の両方のメンバーが含まれていることを確認できます。

```javascript
var ev;

function events1() {
   ev = new SampleComponent.Eventful();
   ev.addEventListener("test", function (e) {
       document.getElementById('output').innerHTML = e.value1;
       document.getElementById('output').innerHTML += "<br/>" + e.value2;
   });
   ev.onTest("Number of feet in a mile:", 5280);
}
```

イベント登録コードを、他のイベント登録コードと同じ then ブロックに追加します。

```javascript
var events1Button = document.getElementById("events1Button");
events1Button.addEventListener("click", events1, false);
```

## <a name="exposing-asynchronous-operations"></a>非同期操作の公開


.NET Framework には、Task クラスとジェネリック [Task&lt;TResult&gt;](/dotnet/api/system.threading.tasks.task-1) クラスに基づく、非同期処理と並列処理のための豊富なツール セットがあります。 Windows ランタイム コンポーネントでタスク ベースの非同期処理を公開するには、[IAsyncAction](/windows/desktop/api/windows.foundation/nn-windows-foundation-iasyncaction)、[IAsyncActionWithProgress&lt;TProgress&gt;](/previous-versions/br205784(v=vs.85))、[IAsyncOperation&lt;TResult&gt;](/previous-versions/br205802(v=vs.85))、[IAsyncOperationWithProgress&lt;TResult, TProgress&gt;](/previous-versions/br205807(v=vs.85)) の各 Windows ランタイム インターフェイスを使います  (Windows ランタイムでは、操作は結果を返しますが、アクションは結果を返しません)。

このセクションでは、進行状況を報告し、結果を返す取り消し可能な非同期操作を示します。 GetPrimesInRangeAsync メソッドは、[AsyncInfo](/dotnet/api/system.runtime.interopservices.windowsruntime) クラスを使用してタスクを生成し、取り消し機能と進行状況レポート機能を WinJS.Promise オブジェクトに関連付けます。 まず、GetPrimesInRangeAsync メソッドをサンプル クラスに追加します。

> [!div class="tabbedCodeSnippets"]
> ```csharp
> using System.Runtime.InteropServices.WindowsRuntime;
> using Windows.Foundation;
>
> public static IAsyncOperationWithProgress<IList<long>, double>
> GetPrimesInRangeAsync(long start, long count)
> {
>     if (start < 2 || count < 1) throw new ArgumentException();
>
>     return AsyncInfo.Run<IList<long>, double>((token, progress) =>
>
>         Task.Run<IList<long>>(() =>
>         {
>             List<long> primes = new List<long>();
>             double onePercent = count / 100;
>             long ctProgress = 0;
>             double nextProgress = onePercent;
>
>             for (long candidate = start; candidate < start + count; candidate++)
>             {
>                 ctProgress += 1;
>                 if (ctProgress >= nextProgress)
>                 {
>                     progress.Report(ctProgress / onePercent);
>                     nextProgress += onePercent;
>                 }
>                 bool isPrime = true;
>                 for (long i = 2, limit = (long)Math.Sqrt(candidate); i <= limit; i++)
>                 {
>                     if (candidate % i == 0)
>                     {
>                         isPrime = false;
>                         break;
>                     }
>                 }
>                 if (isPrime) primes.Add(candidate);
>
>                 token.ThrowIfCancellationRequested();
>             }
>             progress.Report(100.0);
>             return primes;
>         }, token)
>     );
> }
> ```
> ```vb
> Imports System.Runtime.InteropServices.WindowsRuntime
>
> Public Shared Function GetPrimesInRangeAsync(ByVal start As Long, ByVal count As Long)
> As IAsyncOperationWithProgress(Of IList(Of Long), Double)
>
>     If (start < 2 Or count < 1) Then Throw New ArgumentException()
>
>     Return AsyncInfo.Run(Of IList(Of Long), Double)( _
>         Function(token, prog)
>             Return Task.Run(Of IList(Of Long))( _
>                 Function()
>                     Dim primes As New List(Of Long)
>                     Dim onePercent As Long = count / 100
>                     Dim ctProgress As Long = 0
>                     Dim nextProgress As Long = onePercent
>
>                     For candidate As Long = start To start + count - 1
>                         ctProgress += 1
>
>                         If ctProgress >= nextProgress Then
>                             prog.Report(ctProgress / onePercent)
>                             nextProgress += onePercent
>                         End If
>
>                         Dim isPrime As Boolean = True
>                         For i As Long = 2 To CLng(Math.Sqrt(candidate))
>                             If (candidate Mod i) = 0 Then
>                                 isPrime = False
>                                 Exit For
>                             End If
>                         Next
>
>                         If isPrime Then primes.Add(candidate)
>
>                         token.ThrowIfCancellationRequested()
>                     Next
>                     prog.Report(100.0)
>                     Return primes
>                 End Function, token)
>         End Function)
> End Function
> ```

GetPrimesInRangeAsync は非常にシンプルな素数ファインダーであり、これは仕様です。 ここでは非同期操作の実装に重点を置いているので、シンプルであることが重要であり、キャンセルを示すときに低速の実装が利点となります。 GetPrimesInRangeAsync はブルート フォース方式で素数を見つけます。つまり、素数だけを使うのではなく、候補をその平方根以下のすべての整数で除算します。 このコードのステップ実行は次のとおりです。

-   非同期操作を開始する前に、パラメーターの検証や無効な入力に対する例外のスローなどのハウスキーピング アクティビティを実行します。
-   この実装の鍵は、 [asyncinfo です。 &lt; TResult、tprogress &gt; (Func &lt; CancellationToken、iprogress &lt; tprogress &gt; 、Task &lt; TResult &gt; ](/dotnet/api/system.runtime.interopservices.windowsruntime) &gt; ) メソッド、およびメソッドの唯一のパラメーターであるデリゲートを実行します。 デリゲートはキャンセル トークンと進行状況を報告するためのインターフェイスを受け取り、それらのパラメーターを使う開始タスクを返す必要があります。 JavaScript が GetPrimesInRangeAsync メソッドを呼び出すと、次の手順が発生します (ここで示す順序とは限りません)。

    -   [WinJS.Promise](/previous-versions/windows/apps/br211867(v=win.10)) オブジェクトは、返された結果の処理、キャンセルへの対応、進行状況レポートの処理を行う関数を提供します。
    -   AsyncInfo.Run メソッドは、キャンセル ソースと、IProgress&lt;T&gt; インターフェイスを実装するオブジェクトを作成します。 デリゲートに対して、キャンセル ソースからの [CancellationToken](/dotnet/api/system.threading.cancellationtoken) トークンと、[IProgress&lt;T&gt;](/dotnet/api/system.iprogress-1) インターフェイスの両方を渡します。

        > **注:** Promise オブジェクトが取り消し機能に対応する関数を提供しない場合、AsyncInfo.Run は引き続き取り消し可能なトークンを渡し、取り消しが引き続き発生する可能性があります。 Promise オブジェクトが進行状況の更新を処理する関数を提供しない場合、AsyncInfo.Run は IProgress&lt;T&gt; を実装するオブジェクトを引き続き提供しますが、レポートは無視されます。

    -   デリゲートは [Task.Run&lt;TResult&gt;(Func&lt;TResult&gt;, CancellationToken](/dotnet/api/system.threading.tasks.task.run#System_Threading_Tasks_Task_Run__1_System_Func___0__System_Threading_CancellationToken_)) メソッドを使って、トークンと進行状況インターフェイスを使う開始タスクを作成します。 開始タスクのデリゲートは、目的の結果を計算するラムダ関数によって提供されます。 詳細については、すぐに説明します。
    -   AsyncInfo.Run メソッドは、[IAsyncOperationWithProgress&lt;TResult, TProgress&gt;](/uwp/api/Windows.Foundation.IAsyncOperationWithProgress_TResult_TProgress_) インターフェイスを実装するオブジェクトを作成し、Windows ランタイムのキャンセル メカニズムをトークン ソースに関連付け、Promise オブジェクトの進行状況レポート関数を IProgress&lt;T&gt; インターフェイスに関連付けます。
    -   IAsyncOperationWithProgress&lt;TResult, TProgress&gt; インターフェイスが JavaScript に返されます。

-   開始タスクで表されるラムダ関数は引数を受け取りません。 ラムダ関数であるため、トークンと IProgress インターフェイスにアクセスできます。 候補の数値が評価されるたびに、ラムダ関数は以下を実行します。

    -   進行状況の次のパーセント ポイントに到達したかどうかを確認します。 到達した場合、ラムダ関数は IProgress&lt;T&gt;.Report メソッドを呼び出し、Promise オブジェクトが進行状況を報告するために指定した関数にパーセンテージが渡されます。
    -   操作が取り消された場合は、キャンセル トークンを使って例外をスローします。 [IAsyncInfo.Cancel](/uwp/api/windows.foundation.iasyncinfo.cancel) メソッド (IAsyncOperationWithProgress&lt;TResult, TProgress&gt; インターフェイスが継承するメソッド) が呼び出された場合、AsyncInfo.Run メソッドが設定した関連付けにより、キャンセル トークンに確実に通知されます。
-   ラムダ関数が素数のリストを返すと、WinJS.Promise オブジェクトが結果を処理するために指定した関数にこのリストが渡されます。

JavaScript Promise を作成し、キャンセル メカニズムを設定するには、asyncRun 関数と asyncCancel 関数を default.js に追加します。

```javascript
var resultAsync;
function asyncRun() {
    document.getElementById('output').innerHTML = "Retrieving prime numbers.";
    btnAsync.disabled = "disabled";
    btnCancel.disabled = "";

    resultAsync = SampleComponent.Example.getPrimesInRangeAsync(10000000000001, 2500).then(
        function (primes) {
            for (i = 0; i < primes.length; i++)
                document.getElementById('output').innerHTML += " " + primes[i];

            btnCancel.disabled = "disabled";
            btnAsync.disabled = "";
        },
        function () {
            document.getElementById('output').innerHTML += " -- getPrimesInRangeAsync was canceled. -- ";

            btnCancel.disabled = "disabled";
            btnAsync.disabled = "";
        },
        function (prog) {
            document.getElementById('primeProg').value = prog;
        }
    );
}

function asyncCancel() {    
    resultAsync.cancel();
}
```

これまでと同様に、イベント登録コードを忘れないでください。

```javascript
var btnAsync = document.getElementById("btnAsync");
btnAsync.addEventListener("click", asyncRun, false);
var btnCancel = document.getElementById("btnCancel");
btnCancel.addEventListener("click", asyncCancel, false);
```

非同期の GetPrimesInRangeAsync メソッドを呼び出すことで、asyncRun 関数は WinJS.Promise オブジェクトを作成します。 このオブジェクトの then メソッドは、返される結果の処理、エラーへの対応 (キャンセルを含む)、進行状況レポートの処理を行う 3 つの関数を受け取ります。 この例では、返された結果が出力領域に表示されます。 キャンセルまたは完了により、操作を開始するボタンと操作を取り消すボタンがリセットされます。 進行状況レポートにより、プログレス コントロールが更新されます。

asyncCancel 関数は、WinJS.Promise オブジェクトの cancel メソッドを呼び出すだけです。

アプリを実行するには、F5 キーを押します。 非同期操作を開始するには、**[Async]** ボタンをクリックします。 次に行われる処理は、コンピューターの速度によって異なります。 点滅時間になる前に進行状況バーが完了まで進む場合は、10 個の中の 1 つ以上の要素によって GetPrimesInRangeAsync に渡される開始番号を大きくします。 テストする数値の数の増減によって操作の時間を調整できますが、開始番号の途中にゼロを追加すると影響が大きくなります。 操作を取り消すには、**[Cancel Async]** ボタンを選びます。

## <a name="related-topics"></a>関連トピック

* [UWP アプリ用の .NET の概要](/previous-versions/windows/apps/br230302(v=vs.140))
* [UWP アプリ用 .NET](/dotnet/api/index?view=dotnet-uwp-10.0)