---
title: C++/CX を使用した Windows ランタイム コンポーネント
description: このトピックでは、C++/CX を使って Windows ランタイム コンポーネントを作成する方法を示します。このコンポーネントは、Windows ランタイム言語を使って構築しされたユニバーサル Windows アプリから呼び出すことができます。
ms.assetid: F7E06AA2-DCEC-427E-BD5D-9CA2A0ED2612
ms.date: 05/14/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 2965eb3196f2a19f7d5351ee422013c6c22ba88a
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174306"
---
# <a name="windows-runtime-components-with-ccx"></a>C++/CX を使用した Windows ランタイム コンポーネント

> [!NOTE]
> このトピックは、C++/CX アプリケーションの管理ができるようにすることを目的としています。 ただし、新しいアプリケーションには [C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) を使用することをお勧めします。 C++/WinRT は Windows ランタイム (WinRT) API の標準的な最新の C++17 言語プロジェクションで、ヘッダー ファイル ベースのライブラリとして実装され、最新の Windows API への最上位アクセス権を提供するように設計されています。 C++/WinRT を使用して Windows ランタイム コンポーネントを作成する方法については、「[C++/WinRT を使用した Windows Runtime コンポーネント](./create-a-windows-runtime-component-in-cppwinrt.md)」を参照してください。

このトピックでは、C++/CX を使用し &mdash; て、任意の Windows ランタイム言語 (C#、Visual Basic、C++、または Javascript) を使用して構築されたユニバーサル Windows アプリから呼び出すことができるコンポーネントを、Windows ランタイムコンポーネントとして作成する方法について説明します。

C++ で Windows ランタイムコンポーネントを構築するには、いくつかの理由があります。
- 複雑な操作または負荷の高い操作で C++ のパフォーマンス上のメリットを得る。
- 既に作成されテストされている既存のコードを再利用する。

JavaScript プロジェクトまたは .NET プロジェクト、および Windows ランタイム コンポーネント プロジェクトを含むソリューションを構築すると、JavaScript プロジェクト ファイルとコンパイル済みの DLL が 1 つのパッケージにマージされます。これを、シミュレーターを使ってローカルでデバッグしたり、テザリングされたデバイス上でリモートでデバッグしたりすることができます。 また、拡張 SDK としてコンポーネント プロジェクトだけを配布することもできます。 詳しくは、「[Creating a Software Development Kit](/visualstudio/extensibility/creating-a-software-development-kit?view=vs-2015)」(ソフトウェア開発キットの作成) をご覧ください。

一般に、C++/CX コンポーネントのコードを記述するときは、通常の C++ ライブラリと組み込み型を使用します。ただし、別の winmd パッケージ内のコードとの間でデータをやり取りする抽象バイナリインターフェイス (ABI) の境界は除きます。 ここでは、これらの型を作成および操作するために C++/CX でサポートされている Windows ランタイム型と特殊な構文を使用します。 さらに、C++/CX コードでは、デリゲートやイベントなどの型を使用して、コンポーネントから発生させ、JavaScript、Visual Basic、C++、または C# で処理できるイベントを実装します。 C++/CX 構文の詳細については、「 [Visual C++ 言語リファレンス (c++/cx)](/cpp/cppcx/visual-c-language-reference-c-cx)」を参照してください。

## <a name="casing-and-naming-rules"></a>大文字小文字の区別と名前付け規則

### <a name="javascript"></a>JavaScript
JavaScript では、大文字と小文字が区別されます。 したがって、次に示す大文字小文字の区別の規則に従う必要があります。

-   C++ の名前空間とクラスを参照する場合、C++ の側と同じ大文字小文字の区別を使います。
-   メソッドを呼び出す場合、メソッド名が C++ の側で大文字になっていても、camel 規約に従った大文字小文字の区別を使います。 たとえば、C++ のメソッド GetDate() は、JavaScript では getDate() として呼び出す必要があります。
-   アクティブ化可能なクラス名や名前空間名には、UNICODE 文字を含めることはできません。

### <a name="net"></a>.NET
.NET 言語では、各言語の通常の大文字と小文字の規則が適用されます。

## <a name="instantiating-the-object"></a>オブジェクトのインスタンス化
Windows ランタイム型のみ ABI の境界を越えて渡すことができます。 コンパイラは、コンポーネントのパブリック メソッドでの戻り値の型または戻り値パラメーターが std::wstring などの型である場合、エラーを発生させます。 Visual C++ コンポーネント拡張 (C++/CX) の組み込み型には、int や double などの通常のスカラーと、その typedef である int32、float64 などがあります。 詳しくは、「[型システム (C++/CX)](/cpp/cppcx/type-system-c-cx)」をご覧ください。

```cpp
// ref class definition in C++
public ref class SampleRefClass sealed
{
    // Class members...

    // #include <valarray>
public:
    double LogCalc(double input)
    {
        // Use C++ standard library as usual.
        return std::log(input);
    }

};
```

```javascript
//Instantiation in JavaScript (requires "Add reference > Project reference")
var nativeObject = new CppComponent.SampleRefClass();
```

```csharp
//Call a method and display result in a XAML TextBlock
var num = nativeObject.LogCalc(21.5);
ResultText.Text = num.ToString();
```

## <a name="ccx-built-in-types-library-types-and-windows-runtime-types"></a>C++/CX 組み込み型、ライブラリ型、および Windows ランタイム型
アクティブ化可能なクラス (ref クラスとも呼ばれます) は、JavaScript、C#、Visual Basic などの他の言語からインスタンス化できるクラスです。 他の言語から利用できるようにするには、コンポーネントに 1 個以上のアクティブ化可能なクラスを含める必要があります。

Windows ランタイム コンポーネントには、複数のアクティブ化可能なパブリック クラスだけでなく、コンポーネント内部でのみ認識される他のクラスも含めることができます。 [WebHostHidden](/uwp/api/windows.foundation.metadata.webhosthiddenattribute)属性を、JavaScript に表示されることを意図していない C++/cx 型に適用します。

すべてのパブリック クラスが、コンポーネントのメタデータ ファイルと同じ名前を持つ同じルート名前空間に存在する必要があります。 たとえば、A.B.C.MyClass という名前のクラスは、A.winmd または A.B.winmd または A.B.C.winmd という名前のメタデータ ファイルで定義されている場合のみインスタンス化できます。 DLL の名前が .winmd ファイル名と一致する必要はありません。

クライアント コードでは、他のクラスと同様に、**new** キーワード (Visual Basic の場合は **New**) を使って、コンポーネントのインスタンスを作成します。

アクティブ化可能なクラスは **public ref class sealed** として宣言する必要があります。 **ref class** キーワードは、Windows ランタイムと互換性のある型としてクラスを作成するようにコンパイラに指示し、sealed キーワードは、クラスが継承できないことを指定します。 現在、Windows ランタイムは汎用の継承モデルをサポートしていません。限定的な継承モデルによって、カスタム XAML コントロールの作成をサポートしています。 詳しくは、「[Ref クラスと構造体 (C++/CX)](/cpp/cppcx/ref-classes-and-structs-c-cx)」をご覧ください。

C++/CX では、すべての数値プリミティブが既定の名前空間で定義されています。 [Platform](/cpp/cppcx/platform-namespace-c-cx)名前空間には、Windows ランタイム型システムに固有の C++/cx クラスが含まれています。 このようなクラスには、[Platform::String](/cpp/cppcx/platform-string-class) クラスと [Platform::Object](/cpp/cppcx/platform-object-class) クラスがあります。 [Platform::Collections::Map](/cpp/cppcx/platform-collections-map-class) クラスや [Platform::Collections::Vector](/cpp/cppcx/platform-collections-vector-class) クラスなどの具象コレクション型は、[Platform::Collections](/cpp/cppcx/platform-collections-namespace) 名前空間で定義されます。 これらの型によって実装されるパブリック インターフェイスは、[Windows::Foundation::Collections 名前空間 (C++/CX)](/cpp/cppcx/windows-foundation-collections-namespace-c-cx) で定義されます。 JavaScript、C#、および Visual Basic で利用されるのは、この種類のインターフェイスです。 詳しくは、「[型システム (C++/CX)](/cpp/cppcx/type-system-c-cx)」をご覧ください。

## <a name="method-that-returns-a-value-of-built-in-type"></a>組み込み型の値を返すメソッド
```cpp
    // #include <valarray>
public:
    double LogCalc(double input)
    {
        // Use C++ standard library as usual.
        return std::log(input);
    }
```

```javascript
//Call a method
var nativeObject = new CppComponent.SampleRefClass;
var num = nativeObject.logCalc(21.5);
document.getElementById('P2').innerHTML = num;
```

## <a name="method-that-returns-a-custom-value-struct"></a>カスタム値の構造体を返すメソッド
```cpp
namespace CppComponent
{
    // Custom struct
    public value struct PlayerData
    {
        Platform::String^ Name;
        int Number;
        double ScoringAverage;
    };

    public ref class Player sealed
    {
    private:
        PlayerData m_player;
    public:
        property PlayerData PlayerStats
        {
            PlayerData get(){ return m_player; }
            void set(PlayerData data) {m_player = data;}
        }
    };
}
```

ABI 間でユーザー定義の値構造体を渡すには、C++/cxで定義されている値構造体と同じメンバーを持つ JavaScript オブジェクトを定義します。 その後、オブジェクトが C++/CX 型に暗黙的に変換されるように、そのオブジェクトを引数として C++/CX メソッドに渡すことができます。

```javascript
// Get and set the value struct
function GetAndSetPlayerData() {
    // Create an object to pass to C++
    var myData =
        { name: "Bob Homer", number: 12, scoringAverage: .357 };
    var nativeObject = new CppComponent.Player();
    nativeObject.playerStats = myData;

    // Retrieve C++ value struct into new JavaScript object
    var myData2 = nativeObject.playerStats;
    document.getElementById('P3').innerHTML = myData.name + " , " + myData.number + " , " + myData.scoringAverage.toPrecision(3);
}
```

もう 1 つの方法は、IPropertySet を実装するクラスを定義することです (ここでは例は示されていません)。

.NET 言語では、C++/CX コンポーネントで定義されている型の変数を作成するだけです。

```csharp
private void GetAndSetPlayerData()
{
    // Create a ref class
    var player = new CppComponent.Player();

    // Create a variable of a value struct
    // type that is defined in C++
    CppComponent.PlayerData myPlayer;
    myPlayer.Name = "Babe Ruth";
    myPlayer.Number = 12;
    myPlayer.ScoringAverage = .398;

    // Set the property
    player.PlayerStats = myPlayer;

    // Get the property and store it in a new variable
    CppComponent.PlayerData myPlayer2 = player.PlayerStats;
    ResultText.Text += myPlayer.Name + " , " + myPlayer.Number.ToString() +
        " , " + myPlayer.ScoringAverage.ToString();
}
```

## <a name="overloaded-methods"></a>オーバーロードされたメソッド
C++/CX パブリック ref クラスにはオーバーロードされたメソッドを含めることができますが、JavaScript ではオーバーロードされたメソッドを区別する機能が制限されています。 たとえば、以下のシグネチャの相違を区別できます。

```cpp
public ref class NumberClass sealed
{
public:
    int GetNumber(int i);
    int GetNumber(int i, Platform::String^ str);
    double GetNumber(int i, MyData^ d);
};
```

ただし、以下のシグネチャの相違は区別できません。

```cpp
int GetNumber(int i);
double GetNumber(double d);
```

あいまいな場合、JavaScript で特定のオーバーロードを常に呼び出すようにすることができます。そのためには、ヘッダー ファイルのメソッド シグネチャに [Windows::Foundation::Metadata::DefaultOverload](/uwp/api/windows.foundation.metadata.defaultoverloadattribute) 属性を適用します。

次の JavaScript は、属性付きオーバーロードを常に呼び出します。

```javascript
var nativeObject = new CppComponent.NumberClass();
var num = nativeObject.getNumber(9);
document.getElementById('P4').innerHTML = num;
```

## <a name="net"></a>.NET
.NET 言語では、すべての .NET クラスと同様に、C++/CX ref クラスのオーバーロードが認識されます。

## <a name="datetime"></a>DateTime
Windows ランタイムでは、[Windows::Foundation::DateTime](/uwp/api/windows.foundation.datetime) オブジェクトは 1601 年 1 月 1 日の前または後の時間の長さを 100 ナノ秒単位で表した単純な 64 ビットの符号付き整数です。 Windows:Foundation::DateTime オブジェクトには、メソッドはありません。 代わりに、各言語は、その言語にネイティブな方法で DateTime を射影します。 JavaScript では Date オブジェクト、.NET では system.string および DateTimeOffset 型です。

```cpp
public  ref class MyDateClass sealed
{
public:
    property Windows::Foundation::DateTime TimeStamp;
    void SetTime(Windows::Foundation::DateTime dt)
    {
        auto cal = ref new Windows::Globalization::Calendar();
        cal->SetDateTime(dt);
        TimeStamp = cal->GetDateTime(); // or TimeStamp = dt;
    }
};
```

C++/CX から JavaScript に DateTime 値を渡すと、JavaScript はそれを日付オブジェクトとして受け入れ、既定で長い形式の日付文字列として表示します。

```javascript
function SetAndGetDate() {
    var nativeObject = new CppComponent.MyDateClass();

    var myDate = new Date(1956, 4, 21);
    nativeObject.setTime(myDate);

    var myDate2 = nativeObject.timeStamp;

    //prints long form date string
    document.getElementById('P5').innerHTML = myDate2;

}
```

.NET 言語が system.string を C++/CX コンポーネントに渡すと、メソッドはそれを Windows:: Foundation::D ateTime として受け入れます。 コンポーネントが Windows:: Foundation::D ateTime を .NET メソッドに渡すと、フレームワークメソッドはそれを DateTimeOffset として受け入れます。

```csharp
private void DateTimeExample()
{
    // Pass a System.DateTime to a C++ method
    // that takes a Windows::Foundation::DateTime
    DateTime dt = DateTime.Now;
    var nativeObject = new CppComponent.MyDateClass();
    nativeObject.SetTime(dt);

    // Retrieve a Windows::Foundation::DateTime as a
    // System.DateTimeOffset
    DateTimeOffset myDate = nativeObject.TimeStamp;

    // Print the long-form date string
    ResultText.Text += myDate.ToString();
}
```

## <a name="collections-and-arrays"></a>コレクションと配列
コレクションは、常に、Windows::Foundation::Collections::IVector^ や Windows::Foundation::Collections::IMap^ などの Windows ランタイム型へのハンドルとして ABI の境界を越えて渡されます。 たとえば、Platform::Collections::Map にハンドルを返す場合、Windows::Foundation::Collections::IMap^ に暗黙的に変換されます。 コレクションインターフェイスは、具体的な実装を提供する C++/CX クラスとは別の名前空間で定義されています。 そのインターフェイスを JavaScript 言語と .NET 言語で利用します。 詳しくは、「[コレクション (C++/CX)](/cpp/cppcx/collections-c-cx)」と「[Array と WriteOnlyArray (C++/CX)](/cpp/cppcx/array-and-writeonlyarray-c-cx)」をご覧ください。

## <a name="passing-ivector"></a>IVector を渡す場合
```cpp
// Windows::Foundation::Collections::IVector across the ABI.
//#include <algorithm>
//#include <collection.h>
Windows::Foundation::Collections::IVector<int>^ SortVector(Windows::Foundation::Collections::IVector<int>^ vec)
{
    std::sort(begin(vec), end(vec));
    return vec;
}
```

```javascript
var nativeObject = new CppComponent.CollectionExample();
// Call the method to sort an integer array
var inVector = [14, 12, 45, 89, 23];
var outVector = nativeObject.sortVector(inVector);
var result = "Sorted vector to array:";
for (var i = 0; i < outVector.length; i++)
{
    outVector[i];
    result += outVector[i].toString() + ",";
}
document.getElementById('P6').innerHTML = result;
```

.NET 言語は IVector&lt;T&gt; を IList&lt;T&gt; として認識します。

```csharp
private void SortListItems()
{
    IList<int> myList = new List<int>();
    myList.Add(5);
    myList.Add(9);
    myList.Add(17);
    myList.Add(2);

    var nativeObject = new CppComponent.CollectionExample();
    IList<int> mySortedList = nativeObject.SortVector(myList);

    foreach (var item in mySortedList)
    {
        ResultText.Text += " " + item.ToString();
    }
}
```

## <a name="passing-imap"></a>IMap を渡す場合
```cpp
// #include <map>
//#include <collection.h>
Windows::Foundation::Collections::IMap<int, Platform::String^> ^GetMap(void)
{    
    Windows::Foundation::Collections::IMap<int, Platform::String^> ^ret =
        ref new Platform::Collections::Map<int, Platform::String^>;
    ret->Insert(1, "One ");
    ret->Insert(2, "Two ");
    ret->Insert(3, "Three ");
    ret->Insert(4, "Four ");
    ret->Insert(5, "Five ");
    return ret;
}
```

```javascript
// Call the method to get the map
var outputMap = nativeObject.getMap();
var mStr = "Map result:" + outputMap.lookup(1) + outputMap.lookup(2)
    + outputMap.lookup(3) + outputMap.lookup(4) + outputMap.lookup(5);
document.getElementById('P7').innerHTML = mStr;
```

.NET 言語は IMap を IDictionary&lt;K, V&gt; として認識します。

```csharp
private void GetDictionary()
{
    var nativeObject = new CppComponent.CollectionExample();
    IDictionary<int, string> d = nativeObject.GetMap();
    ResultText.Text += d[2].ToString();
}
```

## <a name="properties"></a>プロパティ
C++/CX コンポーネント拡張のパブリック ref クラスは、property キーワードを使用して、パブリックデータメンバーをプロパティとして公開します。 概念は .NET プロパティと同じです。 単純プロパティは機能が暗黙的であるため、データ メンバーに似ています。 非単純プロパティには、明示的な get アクセサーと set アクセサーがあり、値の "バッキング ストア" である名前付きのプライベート変数があります。 この例では、プライベートメンバー変数 \_ propertyAValue は PropertyA のバッキングストアです。 プロパティの値が変化するときにイベントを生成できます。またクライアント アプリは、そのイベントを受け取るように登録することができます。

```cpp
//Properties
public delegate void PropertyChangedHandler(Platform::Object^ sender, int arg);
public ref class PropertyExample  sealed
{
public:
    PropertyExample(){}

    // Event that is fired when PropertyA changes
    event PropertyChangedHandler^ PropertyChangedEvent;

    // Property that has custom setter/getter
    property int PropertyA
    {
        int get() { return m_propertyAValue; }
        void set(int propertyAValue)
        {
            if (propertyAValue != m_propertyAValue)
            {
                m_propertyAValue = propertyAValue;
                // Fire event. (See event example below.)
                PropertyChangedEvent(this, propertyAValue);
            }
        }
    }

    // Trivial get/set property that has a compiler-generated backing store.
    property Platform::String^ PropertyB;

private:
    // Backing store for propertyA.
    int m_propertyAValue;
};
```

```javascript
var nativeObject = new CppComponent.PropertyExample();
var propValue = nativeObject.propertyA;
document.getElementById('P8').innerHTML = propValue;

//Set the string property
nativeObject.propertyB = "What is the meaning of the universe?";
document.getElementById('P9').innerHTML += nativeObject.propertyB;
```

.NET 言語では、.NET オブジェクトの場合と同様に、ネイティブ C++/CX オブジェクトのプロパティにアクセスします。

```csharp
private void GetAProperty()
{
    // Get the value of the integer property
    // Instantiate the C++ object
    var obj = new CppComponent.PropertyExample();

    // Get an integer property
    var propValue = obj.PropertyA;
    ResultText.Text += propValue.ToString();

    // Set a string property
    obj.PropertyB = " What is the meaning of the universe?";
    ResultText.Text += obj.PropertyB;

}
```

## <a name="delegates-and-events"></a>デリゲートおよびイベント
デリゲートは、関数オブジェクトを表す Windows ランタイム型です。 デリゲートは、後で実行するアクションを指定するために、イベント、コールバック、非同期メソッド呼び出しに関連して使います。 デリゲートは、関数オブジェクトのように、関数の戻り値の型とパラメーターの型を確認するためにコンパイラを有効にすることによってタイプ セーフを提供します。 デリゲートの宣言は関数のシグネチャに似ており、実装はクラス定義に、また呼び出しは関数の呼び出しに似ています。

## <a name="adding-an-event-listener"></a>イベント リスナーの追加
指定されたデリゲート型のパブリック メンバーを宣言するために event キーワードを使うことができます。 クライアント コードは、特定の言語に用意されている標準機能を使ってイベントをサブスクライブします。

```cpp
public:
    event SomeHandler^ someEvent;
```

この例では、前のプロパティに関するセクションと同じ C++ コードを使います。

```javascript
function Button_Click() {
    var nativeObj = new CppComponent.PropertyExample();
    // Define an event handler method
    var singlecasthandler = function (ev) {
        document.getElementById('P10').innerHTML = "The button was clicked and the value is " + ev;
    };

    // Subscribe to the event
    nativeObj.onpropertychangedevent = singlecasthandler;

    // Set the value of the property and fire the event
    var propValue = 21;
    nativeObj.propertyA = 2 * propValue;

}
```

.NET 言語では、C++ コンポーネントでイベントをサブスクライブすることは、.NET クラスのイベントをサブスクライブすることと同じです。

```csharp
//Subscribe to event and call method that causes it to be fired.
private void TestMethod()
{
    var objWithEvent = new CppComponent.PropertyExample();
    objWithEvent.PropertyChangedEvent += objWithEvent_PropertyChangedEvent;

    objWithEvent.PropertyA = 42;
}

//Event handler method
private void objWithEvent_PropertyChangedEvent(object __param0, int __param1)
{
    ResultText.Text = "the event was fired and the result is " +
         __param1.ToString();
}
```

## <a name="adding-multiple-event-listeners-for-one-event"></a>1 つのイベントに複数のイベント リスナーを追加する
JavaScript には、複数のハンドラーで単一のイベントをサブスクライブできるようにする addEventListener メソッドがあります。

```cpp
public delegate void SomeHandler(Platform::String^ str);

public ref class LangSample sealed
{
public:
    event SomeHandler^ someEvent;
    property Platform::String^ PropertyA;

    // Method that fires an event
    void FireEvent(Platform::String^ str)
    {
        someEvent(Platform::String::Concat(str, PropertyA->ToString()));
    }
    //...
};
```

```javascript
// Add two event handlers
var multicast1 = function (ev) {
    document.getElementById('P11').innerHTML = "Handler 1: " + ev.target;
};
var multicast2 = function (ev) {
    document.getElementById('P12').innerHTML = "Handler 2: " + ev.target;
};

var nativeObject = new CppComponent.LangSample();
//Subscribe to the same event
nativeObject.addEventListener("someevent", multicast1);
nativeObject.addEventListener("someevent", multicast2);

nativeObject.propertyA = "42";

// This method should fire an event
nativeObject.fireEvent("The answer is ");
```

C# では、前の例で示したように += 演算子を使うことで、任意の数のイベント ハンドラーがイベントをサブスクライブできるようになります。

## <a name="enums"></a>列挙型
C++/CX の Windows ランタイム列挙型は、パブリッククラス enum を使用して宣言されています。これは、標準 C++ のスコープ列挙型に似ています。

```cpp
public enum class Direction {North, South, East, West};

public ref class EnumExampleClass sealed
{
public:
    property Direction CurrentDirection
    {
        Direction  get(){return m_direction; }
    }

private:
    Direction m_direction;
};
```

列挙値は、C++/CX と JavaScript の間で整数として渡されます。 必要に応じて、C++/CX 列挙と同じ名前付きの値を含む JavaScript オブジェクトを宣言し、次のように使用できます。

```javascript
var Direction = { 0: "North", 1: "South", 2: "East", 3: "West" };
//. . .

var nativeObject = new CppComponent.EnumExampleClass();
var curDirection = nativeObject.currentDirection;
document.getElementById('P13').innerHTML =
Direction[curDirection];
```

C# と Visual Basic のどちらの言語でも列挙型がサポートされます。 これらの言語では、.NET 列挙型と同様に、C++ パブリック列挙型クラスが表示されます。

## <a name="asynchronous-methods"></a>非同期メソッド
他の Windows ランタイム オブジェクトによって公開される非同期メソッドを利用するには、[task クラス (同時実行ランタイム)](/cpp/parallel/concrt/reference/task-class) を使います。 詳しくは、「[タスクの並列処理 (同時実行ランタイム)](/cpp/parallel/concrt/task-parallelism-concurrency-runtime)」をご覧ください。

C++/CX で非同期メソッドを実装するには、ppltasks.h で定義されている [create \_ async](/cpp/parallel/concrt/reference/concurrency-namespace-functions?view=vs-2017) 関数を使用します。 詳細については、「 [C++/cx での UWP アプリ用の非同期操作の作成](/cpp/parallel/concrt/creating-asynchronous-operations-in-cpp-for-windows-store-apps)」を参照してください。 例については、「 [C++/cx Windows ランタイムコンポーネントの作成」および「JavaScript または C# からの呼び出し](walkthrough-creating-a-basic-windows-runtime-component-in-cpp-and-calling-it-from-javascript-or-csharp.md)」を参照してください。 .NET 言語では、.NET で定義されている非同期メソッドと同様に、C++/CX 非同期メソッドが使用されます。

## <a name="exceptions"></a>例外
Windows ランタイムによって定義された任意の例外の型をスローできます。 Windows ランタイムのどの例外の型からもカスタム型は取得できません。 ただし、COMException をスローし、例外をキャッチするコードがアクセスできるカスタム HRESULT を提供できます。 COMException でカスタム メッセージを指定する方法はありません。

## <a name="debugging-tips"></a>デバッグのヒント
コンポーネント DLL を含む JavaScript ソリューションをデバッグするときは、コンポーネントでスクリプトのステップ実行またはネイティブ コードのステップ実行を有効にするようにデバッガーを設定できますが、この両方を同時に有効にすることはできません。 設定を変更するには、ソリューション エクスプローラーで JavaScript プロジェクト ノードを選んでから、[プロパティ]、[デバッグ]、[デバッガーの種類] の順に選びます。

パッケージ デザイナーで必ず適切な機能を選んでください。 たとえば、Windows ランタイム API を使ってユーザーの画像ライブラリにある画像ファイルを開く場合は、マニフェスト デザイナーの [機能] ウィンドウの [画像ライブラリ] チェック ボックスをオンにします。

JavaScript コードがコンポーネントのパブリック プロパティまたはパブリック メソッドを認識しないと考えられる場合は、JavaScript で camel 規約を使っていることを確認します。 たとえば、LogCalc C++/CX メソッドは、JavaScript で logCalc として参照する必要があります。

C++/CX Windows ランタイムコンポーネントプロジェクトをソリューションから削除する場合は、JavaScript プロジェクトからプロジェクト参照も手動で削除する必要があります。 これを行わないと、後続のデバッグまたはビルド操作が妨げられます。 その後、必要に応じてアセンブリ参照を DLL に追加できます。

## <a name="related-topics"></a>関連トピック
* [C++/CX Windows ランタイム コンポーネントの作成と JavaScript または C# からの呼び出しに関するチュートリアル](walkthrough-creating-a-basic-windows-runtime-component-in-cpp-and-calling-it-from-javascript-or-csharp.md)