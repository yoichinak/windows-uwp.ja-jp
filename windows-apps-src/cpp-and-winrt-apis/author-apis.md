---
description: このトピックでは、直接的または間接的に **winrt::implements** 基本構造体を使用して、C++/WinRT API を作成する方法を示します。
title: C++/WinRT での API の作成
ms.date: 07/08/2019
ms.topic: article
keywords: windows 10、uwp、標準、c++、cpp、winrt、プロジェクション、プロジェクション、実装、インプリメント、ランタイム クラス、ライセンス認証
ms.localizationpriority: medium
ms.openlocfilehash: 0b5c515760d0a03e163fa663da1f97a728a6da2c
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89154596"
---
# <a name="author-apis-with-cwinrt"></a>C++/WinRT での API の作成

このトピックでは、直接的または間接的に [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements) 基本構造体を使用して、[C++/WinRT](./intro-to-using-cpp-with-winrt.md) API を作成する方法を示します。 このコンテキストで*作成者*の同義語は、*生成*、または*実装*です。 このトピックでは、C++/WinRT で API を実装するために、次のシナリオをこの順序で説明します。

> [!NOTE]
> このトピックでは Windows ランタイム コンポーネントについて説明しますが、C++/WinRT のコンテキストについてだけです。 すべての Windows ランタイム言語を対象とする Windows ランタイム コンポーネントに関するコンテンツをお探しの場合は、「[Windows ランタイム コンポーネント](../winrt-components/index.md)」をご覧ください。

- Windows ランタイム クラス (ランタイム クラス) は作成*しません*。アプリ内でのローカルの使用のために 1 つまたは複数の Windows ランタイム インターフェイスを実装するだけです。 この場合、**winrt::implements** から直接派生し、機能を実装します。
- ランタイム クラスを作成*します*。 アプリで使用するコンポーネントを作成している場合があります。 または、XAML ユーザー インターフェイス (UI) で使用する型を作成していることがあり、その場合は両方を実装して、同じのコンパイル ユニット内のランタイム クラスを使用しています。 このような場合、ツールで **winrt::implements** から派生するクラスを生成することができます。

どちらの場合も、C++/WinRT API を実装する型は、*実装型*と呼ばれます。

> [!IMPORTANT]
> 実装型の概念を投影型と区別することは重要です。 投影型については、「[C++/WinRT での API の使用](consume-apis.md)」で説明します。

## <a name="if-youre-not-authoring-a-runtime-class"></a>ランタイム クラスを作成*していない*場合

最も単純なシナリオは、ローカル使用のための Windows ランタイム インターフェイスを実装しているケースです。 ランタイム クラスは必要ありません。通常の C++ のクラスだけです。 たとえば、[**CoreApplication**](/uwp/api/windows.applicationmodel.core.coreapplication) に基づいてアプリを記述している場合があります。

> [!NOTE]
> C++/WinRT Visual Studio Extension (VSIX) と NuGet パッケージ (両者が連携してプロジェクト テンプレートとビルドをサポート) のインストールと使用については、[Visual Studio での C++/WinRT のサポート](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)に関する記事を参照してください。

Visual Studio では、**Visual C++**  > **Windows ユニバーサル** > **Core App (C++/WinRT)** プロジェクト テンプレートで **CoreApplication** パターンを示しています。 パターンは、[**Windows::ApplicationModel::Core::IFrameworkViewSource**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource) の実装を [**coreapplication::run**](/uwp/api/windows.applicationmodel.core.coreapplication.run) に渡して開始します。

```cppwinrt
using namespace Windows::ApplicationModel::Core;
int __stdcall wWinMain(HINSTANCE, HINSTANCE, PWSTR, int)
{
    IFrameworkViewSource source = ...
    CoreApplication::Run(source);
}
```

**CoreApplication** はインターフェイスを使用してアプリの最初のビューを作成します。 概念的には、**IFrameworkViewSource** は次のように見えます。

```cppwinrt
struct IFrameworkViewSource : IInspectable
{
    IFrameworkView CreateView();
};
```

さらに、概念的には、**CoreApplication::run** の実装は次のように見えます。

```cppwinrt
void Run(IFrameworkViewSource viewSource) const
{
    IFrameworkView view = viewSource.CreateView();
    ...
}
```

したがって、開発者は、**IFrameworkViewSource** インターフェイスを実装します。 C++/WinRT には、基本構造体テンプレート [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements) があり、COM スタイルのプログラミングを使用せずにインターフェイス (1 つまたは複数) を簡単に実装することができます。 **実装**から型を派生して、インターフェイスの機能を実装するだけです。 以下にその方法を示します。

```cppwinrt
// App.cpp
...
struct App : implements<App, IFrameworkViewSource>
{
    IFrameworkView CreateView()
    {
        return ...
    }
}
...
```

**IFrameworkViewSource** に対応済みです。 次に、**IFrameworkView** インターフェイスを実装するオブジェクトを返します。 **アプリ**上にそのインターフェイスを実装することも選択できます。 次のコード例は、少なくともウィンドウを起動してデスクトップ上で実行する最小限のアプリを表します。

```cppwinrt
// App.cpp
...
struct App : implements<App, IFrameworkViewSource, IFrameworkView>
{
    IFrameworkView CreateView()
    {
        return *this;
    }

    void Initialize(CoreApplicationView const &) {}

    void Load(hstring const&) {}

    void Run()
    {
        CoreWindow window = CoreWindow::GetForCurrentThread();
        window.Activate();

        CoreDispatcher dispatcher = window.Dispatcher();
        dispatcher.ProcessEvents(CoreProcessEventsOption::ProcessUntilQuit);
    }

    void SetWindow(CoreWindow const & window)
    {
        // Prepare app visuals here
    }

    void Uninitialize() {}
};
...
```

**アプリ**型*が* **IFrameworkViewSource** であるため、**Run** に渡すことだけができます。

```cppwinrt
using namespace Windows::ApplicationModel::Core;
int __stdcall wWinMain(HINSTANCE, HINSTANCE, PWSTR, int)
{
    CoreApplication::Run(winrt::make<App>());
}
```

## <a name="if-youre-authoring-a-runtime-class-in-a-windows-runtime-component"></a>Windows ランタイム コンポーネントでランタイム クラスを作成する場合

型が、アプリケーションから使用する Windows ランタイム コンポーネントにパッケージ化されている場合は、ランタイム クラスである必要があります。 ランタイム クラスは Microsoft インターフェイス定義言語 (IDL) (.idl) ファイルに宣言します (「[ランタイム クラスを Midl ファイル (.idl) にファクタリングする](#factoring-runtime-classes-into-midl-files-idl)」を参照)。

結果として各 IDL ファイルが `.winmd` ファイルになり、Visual Studio では、それらのすべてのファイルをルート名前空間と同じ名前の単一のファイルにマージします。 その最終的な `.winmd` ファイルが、コンポーネントのユーザーが参照するファイルになります。

ランタイム クラスを IDL ファイルに宣言する例を次に示します。

```idl
// MyRuntimeClass.idl
namespace MyProject
{
    runtimeclass MyRuntimeClass
    {
        // Declaring a constructor (or constructors) in the IDL causes the runtime class to be
        // activatable from outside the compilation unit.
        MyRuntimeClass();
        String Name;
    }
}
```

この IDL では、Windows ランタイム (ランタイム) クラスを宣言します。 ランタイム クラスは、通常実行可能な境界を越えて、モダン COM インターフェイス経由でアクティブ化および使用可能な型です。 プロジェクトに IDL ファイルを追加し、ビルドすると、C++/WinRT ツールチェーン (`midl.exe` および `cppwinrt.exe`) が実装型を生成します。 IDL ファイルのワークフローの動作の例については、「[XAML コントロール: C++/WinRT プロパティへのバインド](binding-property.md)」をご覧ください。

上記の IDL の例を使用すると、実装型は、`\MyProject\MyProject\Generated Files\sources\MyRuntimeClass.h` と `MyRuntimeClass.cpp` という名前のソース コード ファイルの **winrt::MyProject::implementation::MyRuntimeClass** という名前の C++ 構造体スタブです。

実装型は次のようになります。

```cppwinrt
// MyRuntimeClass.h
...
namespace winrt::MyProject::implementation
{
    struct MyRuntimeClass : MyRuntimeClassT<MyRuntimeClass>
    {
        MyRuntimeClass() = default;

        winrt::hstring Name();
        void Name(winrt::hstring const& value);
    };
}

// winrt::MyProject::factory_implementation::MyRuntimeClass is here, too.
```

使用されている F バインド ポリモーフィズム パターンに注意してください (**MyRuntimeClass** は、その基本である **MyRuntimeClassT** へのテンプレート引数としてそれ自体を使用します)。 これは、CRTP (curiously recurring template pattern (奇妙に再帰したテンプレート パターン)) とも呼ばれます。 上方向に継承チェーンをたどると、**MyRuntimeClass_base** が見つかります。

```cppwinrt
template <typename D, typename... I>
struct MyRuntimeClass_base : implements<D, MyProject::IMyRuntimeClass, I...>
```

そのため、このシナリオでは、継承のルートの階層はもう一度 [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements) 基本構造体のテンプレートです。

Windows ランタイム コンポーネントでの API の作成に関する詳細、コード、チュートリアルについては、「[C++/WinRT を使用した Windows ランタイム コンポーネント](../winrt-components/create-a-windows-runtime-component-in-cppwinrt.md)」および「[C++/WinRT でのイベントの作成](./author-events.md)」を参照してください。

## <a name="if-youre-authoring-a-runtime-class-to-be-referenced-in-your-xaml-ui"></a>XAML UI で参照されるランタイム クラスを作成する場合

型が XAML UI によって参照される場合、XAML と同じプロジェクトになっていても、ランタイム クラスである必要があります。 通常は実行可能な境界を越えてアクティブ化されますが、ランタイム クラスでは、代わりにそれを実装するコンパイル ユニット内で使用できます。

このシナリオでは、API を使用する *および* のどちらも作成します。 ランタイム クラスを実装するための手順は、Windows ランタイム コンポーネントと基本的に同じです。 このため、前のセクション「[Windows ランタイム コンポーネントでランタイム クラスを作成する場合](#if-youre-authoring-a-runtime-class-in-a-windows-runtime-component)」を参照してください。 これと唯一異なる詳細な点は、IDL から、C++/WinRT ツールチェーンが、実装型だけでなく投影型も生成することです。 このシナリオでは "**MyRuntimeClass**" というだけではあいまいなことを理解することは重要です。これは、その名前を持つさまざまな種類の複数のエンティティがあるためです。

- **MyRuntimeClass** はランタイム クラスの名前です。 ただし、これは実際には IDL で宣言され、一部のプログラミング言語で実装されたアブストラクションです。
- **MyRuntimeClass** は、C++ 構造体 **winrt::MyProject::implementation::MyRuntimeClass** の名前です。これはランタイム クラスの C++/WinRT の実装です。 すでに見たように、プロジェクトを別に実装および使用している場合、この構造体は実装しているプロジェクトにのみ存在します。 これは*実装型*、または*実装*です。 この型は、(`cppwinrt.exe` ツールによって) ファイル `\MyProject\MyProject\Generated Files\sources\MyRuntimeClass.h` と `MyRuntimeClass.cpp` で生成されます。
- **MyRuntimeClass** は C++ 構造体 **winrt::MyProject::MyRuntimeClass** の形式の投影型の名前です。 プロジェクトを別に実装および使用している場合、この構造体は使用しているプロジェクトにのみ存在します。 これは*投影型*、または*投影*です。 この型は、(`cppwinrt.exe` によって) ファイル `\MyProject\MyProject\Generated Files\winrt\impl\MyProject.2.h` で生成されます。

![投影型と実装型](images/myruntimeclass.png)

このトピックに関連する投影型の一部を次に示します。

```cppwinrt
// MyProject.2.h
...
namespace winrt::MyProject
{
    struct MyRuntimeClass : MyProject::IMyRuntimeClass
    {
        MyRuntimeClass(std::nullptr_t) noexcept {}
        MyRuntimeClass();
    };
}
```

ランタイム クラスで **INotifyPropertyChanged** インターフェイスを実装する例のチュートリアルについては、「[XAML コントロール、C++/WinRT プロパティへのバインド](binding-property.md)」を参照してください。

このシナリオでランタイム クラスを使用するための手順は、「[C++/WinRT での API の使用](consume-apis.md#if-the-api-is-implemented-in-the-consuming-project)」で説明しています。

## <a name="factoring-runtime-classes-into-midl-files-idl"></a>ランタイム クラスを Midl ファイル (.idl) にファクタリングする

Visual Studio プロジェクトと項目テンプレートでは、ランタイム クラスごとに個別の IDL ファイルが生成されます。 これにより、IDL ファイルとその生成されたソース コード ファイルとの間に論理的な通信手段が提供されます。

ただし、プロジェクトのすべてのランタイム クラスを 1 つの IDL ファイルに統合すると、ビルド時間が大幅に短縮されます。 これらの間に複雑な (またはわかりにくい) `import` 依存関係があると、統合が実際に必要になる場合があります。 また、まとまっていればランタイム クラスをより簡単に作成、レビューできることがわかります。

## <a name="runtime-class-constructors"></a>ランタイム クラスのコンストラクター

上記で見た一覧から除くためのポイントを次に示します。

- 各コンストラクターを IDL で宣言することで、コンストラクターは実装型と投影型の両方で生成されます。 IDL で宣言したコンストラクターは、*別の*コンパイル ユニットからランタイム クラスを使用するために使用されます。
- コンストラクターを IDL で宣言してもしなくても、**std::nullptr_t** を受け取るコンストラクターのオーバーロードが投影型に対して生成されます。 **std::nullptr_t** コンストラクターの呼び出しは、"*同じ*" コンパイル ユニットからのランタイム クラスの使用における "*2 つのステップのうちの最初のステップ*" です。 詳細とコード例については、「[C++/WinRT での API の使用](consume-apis.md#if-the-api-is-implemented-in-the-consuming-project)」を参照してください。
- *同じ*コンパイル ユニットからランタイム クラスを使用している場合、(`MyRuntimeClass.h` の) 実装型で非既定のコンストラクターを直接実装することもできます。

> [!NOTE]
> ランタイム クラスが別のコンパイル ユニットから使用することが予測される場合は (これは一般的です)、IDL にコンストラクター (少なくとも既定のコンストラクター) を含めます。 これを行うことで、実装型とともにファクトリの実装も取得します。
> 
> 同じコンパイル ユニット内でのみランタイム クラスを作成および使用する場合は、IDL でコンストラクターを宣言しないでください。 ファクトリの実装は不要であり、生成されません。 実装型の既定のコンストラクターが削除されますが、簡単に編集して、代わりに既定にすることができます。
> 
> 同じコンパイル ユニット内でのみランタイム クラスを作成および使用する場合は、コンストラクターのパラメーターが必要であり、実装型で直接必要なコンストラクターを作成します。

## <a name="runtime-class-methods-properties-and-events"></a>ランタイム クラスのメソッド、プロパティ、イベント

ワークフローでは IDL を使ってランタイム クラスとそのメンバーを宣言すること、そしてツールによってプロトタイプとスタブの実装が自動的に生成されることを説明しました。 ランタイム クラスのメンバーに対して自動的に生成されるそれらのプロトタイプについては、IDL で宣言されている型から別の型を渡すように編集することが "*できます*"。 ただし、それができるのは、IDL で宣言されている型が、実装バージョンで宣言されている型に転送できる場合だけです。

例をいくつか紹介します。

- パラメーターの型を緩和できます。 たとえば、IDL でメソッドが **SomeClass** を受け取る場合、実装ではそれを **IInspectable** に変更できます。 これができるのは、すべての **SomeClass** を **IInspectable** に転送できるためです (もちろん、逆はできません)。
- コピー可能なパラメーターを参照渡しではなく値で受け取ることができます。 たとえば、`SomeClass const&` を `SomeClass` に変更します。 コルーチンに参照をキャプチャしないようにする必要があるときは、それが必要です (「[パラメーターの引き渡し](./concurrency.md#parameter-passing)」をご覧ください)。
- 戻り値を緩和できます。 たとえば、**void** を [**winrt::fire_and_forget**](/uwp/cpp-ref-for-winrt/fire-and-forget) に変更できます。

最後の 2 つは、非同期イベント ハンドラーを作成する場合に非常に便利です。

## <a name="instantiating-and-returning-implementation-types-and-interfaces"></a>実装型とインターフェイスをインスタンス化して返す

このセクションでは、[**IStringable**](/uwp/api/windows.foundation.istringable) および [**IClosable**](/uwp/api/windows.foundation.iclosable) インターフェイスを実装する、**MyType** という名前の実装型を例として見ていきましょう。

[**winrt::implements**](/uwp/cpp-ref-for-winrt/implements) から直接 **MyType** を派生できます (ランタイム クラスではありません)。

```cppwinrt
#include <winrt/Windows.Foundation.h>

using namespace winrt;
using namespace Windows::Foundation;

struct MyType : implements<MyType, IStringable, IClosable>
{
    winrt::hstring ToString(){ ... }
    void Close(){}
};
```

または IDL から生成することができます (これはランタイム クラスです)。

```idl
// MyType.idl
namespace MyProject
{
    runtimeclass MyType: Windows.Foundation.IStringable, Windows.Foundation.IClosable
    {
        MyType();
    }    
}
```

実装型を直接割り当てることはできません。

```cppwinrt
MyType myimpl; // error C2259: 'MyType': cannot instantiate abstract class
```

ただし、[**winrt::make**](/uwp/cpp-ref-for-winrt/make) 関数テンプレートを呼び出すことで、**MyType** から **IStringable** または **IClosable** オブジェクトに移動し、これらのオブジェクトを使用するか、プロジェクションの一部として返すことができます。 **make** は実装型の既定のインターフェイスを返します。

```cppwinrt
IStringable istringable = winrt::make<MyType>();
```

> [!NOTE]
> ただし、XAML UI から型を参照している場合は、同じプロジェクト内に実装型と投影型の両方があります。 その場合、**make** は投影型のインスタンスを返します。 そのシナリオのコード例については、「[XAML コントロール、C++/WinRT プロパティへのバインド](binding-property.md#add-a-property-of-type-bookstoreviewmodel-to-mainpage)」を参照してください。

**IStringable** インターフェイスのメンバーを呼び出すには (上記のコード例の場合) `istringable` のみ使用できます。 ただし C++/WinRT インターフェイス (投影されたインターフェイス) は [**winrt::Windows::Foundation::IUnknown**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown) から派生します。 したがって、その上で [**IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function) (または [**IUnknown::try_as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntry_as-function)) を呼び出して他の投影された型またはインターフェイスのクエリを実行し、これを使用するか返すことができます。

```cppwinrt
istringable.ToString();
IClosable iclosable = istringable.as<IClosable>();
iclosable.Close();
```

すべての実装のメンバーにアクセスし、後でインターフェイスを呼び出し元に返す必要がある場合、[**winrt::make_self**](/uwp/cpp-ref-for-winrt/make-self) 関数テンプレートを使用します。 **make_self** は、実装型をラッピングする [**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr) を返します。 (矢印演算子を使用して) そのインターフェイスのすべてのメンバーにアクセスすることができます。これをそのまま呼び出し元に返すか、またはそこで **as** を呼び出して結果として得られるインターフェイス オブジェクトを呼び出し元に返します。

```cppwinrt
winrt::com_ptr<MyType> myimpl = winrt::make_self<MyType>();
myimpl->ToString();
myimpl->Close();
IClosable iclosable = myimpl.as<IClosable>();
iclosable.Close();
```

**MyType** クラスは、プロジェクションの一部ではなく、実装です。 ただしこの方法では、仮想関数呼び出しのオーバーヘッドがなく、その実装メソッドを直接呼び出すことができます。 上記の例では、**MyType::ToString** が **IStringable** で投影されたメソッドと同じ署名を使用するにもかかわらず、アプリケーション バイナリ インターフェイス (ABI) を交差することなく非仮想メソッドを直接呼び出しています。 **Com_ptr** は **MyType** 構造体へのポインターを保持するだけであるため、`myimpl` 変数と矢印演算子を介して、**MyType** の他の内部の詳細にもアクセスできます。

インターフェイス オブジェクトがあり、それが実装上のインターフェイスであることがわかった場合は、[**winrt::get_self**](/uwp/cpp-ref-for-winrt/get-self) 関数テンプレートを使用して実装に戻すことができます。 もう一度、これは仮想関数の呼び出しを回避し、実装で直接取得することができる手法です。

> [!NOTE]
> Windows SDK バージョン 10.0.17763.0 (Windows 10 Version 1809) 以降をインストールしていない場合は、[**winrt::get_self**](/uwp/cpp-ref-for-winrt/get-self) ではなく [**winrt::from_abi**](/uwp/cpp-ref-for-winrt/from-abi) を呼び出す必要があります。

次に例を示します。 別の例が、[**BgLabelControl** カスタム コントロール クラスの実装](xaml-cust-ctrl.md#implement-the-bglabelcontrol-custom-control-class)に関する記事にあります。

```cppwinrt
void ImplFromIClosable(IClosable const& from)
{
    MyType* myimpl = winrt::get_self<MyType>(from);
    myimpl->ToString();
    myimpl->Close();
}
```

ただし元のインターフェイス オブジェクトのみ参照に保持されます。 こ*れを*保持する場合は、[**com_ptr::copy_from**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrcopy_from-function) を呼び出すことができます。

```cppwinrt
winrt::com_ptr<MyType> impl;
impl.copy_from(winrt::get_self<MyType>(from));
// com_ptr::copy_from ensures that AddRef is called.
```

実装型自体は、**winrt::Windows::Foundation::IUnknown** から派生したものではないため、**as** 関数はありません。 それでも、上記の **ImplFromIClosable** 関数でわかるように、そのインターフェイスすべてのメンバーにアクセスできます。 ただし、そのようにする場合、未処理の実装型のインスタンスを呼び出し元に返さないでください。 代わりに、既に示した手法のいずれかを使用して、投影されるインターフェイスまたは **com_ptr** を返します。

実装型のインスタンスがある場合は、次のコード例に示すように、対応する投影型を想定している関数にこれを渡す必要があり、その後実行できます。 変換演算子は (`cppwinrt.exe` ツールによって実装型が生成されたという条件で) 実装型に存在し、これを可能にします。 実装型の値は、対応する投影型の値を想定しているメソッドに直接渡すことができます。 実装型のメンバー関数から、対応する投影型の値を想定しているメソッドに `*this` を渡すことができます。

```cppwinrt
// MyClass.idl
import "MyOtherClass.idl";
namespace MyProject
{
    runtimeclass MyClass
    {
        MyClass();
        void MemberFunction(MyOtherClass oc);
    }
}

// MyClass.h
...
namespace winrt::MyProject::implementation
{
    struct MyClass : MyClassT<MyClass>
    {
        MyClass() = default;
        void MemberFunction(MyProject::MyOtherClass const& oc) { oc.DoWork(*this); }
    };
}
...

// MyOtherClass.idl
import "MyClass.idl";
namespace MyProject
{
    runtimeclass MyOtherClass
    {
        MyOtherClass();
        void DoWork(MyClass c);
    }
}

// MyOtherClass.h
...
namespace winrt::MyProject::implementation
{
    struct MyOtherClass : MyOtherClassT<MyOtherClass>
    {
        MyOtherClass() = default;
        void DoWork(MyProject::MyClass const& c){ /* ... */ }
    };
}
...

//main.cpp
#include "pch.h"
#include <winrt/base.h>
#include "MyClass.h"
#include "MyOtherClass.h"
using namespace winrt;

// MyProject::MyClass is the projected type; the implementation type would be MyProject::implementation::MyClass.

void FreeFunction(MyProject::MyOtherClass const& oc)
{
    auto defaultInterface = winrt::make<MyProject::implementation::MyClass>();
    MyProject::implementation::MyClass* myimpl = winrt::get_self<MyProject::implementation::MyClass>(defaultInterface);
    oc.DoWork(*myimpl);
}
...
```

## <a name="deriving-from-a-type-that-has-a-non-default-constructor"></a>既定以外のコンストラクターを持つ型からの派生

[**ToggleButtonAutomationPeer::ToggleButtonAutomationPeer(ToggleButton)** ](/uwp/api/windows.ui.xaml.automation.peers.togglebuttonautomationpeer.-ctor#Windows_UI_Xaml_Automation_Peers_ToggleButtonAutomationPeer__ctor_Windows_UI_Xaml_Controls_Primitives_ToggleButton_) は既定以外のコンストラクターの例です。 既定のコンストラクターがないので、**ToggleButtonAutomationPeer** を作成するには、*オーナー* に渡す必要があります。 したがって、**ToggleButtonAutomationPeer** から派生する場合、*オーナー* を受け取りベースに渡すコンストラクターを提供する必要があります。 実際には次のようになります。

```idl
// MySpecializedToggleButton.idl
namespace MyNamespace
{
    runtimeclass MySpecializedToggleButton :
        Windows.UI.Xaml.Controls.Primitives.ToggleButton
    {
        ...
    };
}
```

```idl
// MySpecializedToggleButtonAutomationPeer.idl
namespace MyNamespace
{
    runtimeclass MySpecializedToggleButtonAutomationPeer :
        Windows.UI.Xaml.Automation.Peers.ToggleButtonAutomationPeer
    {
        MySpecializedToggleButtonAutomationPeer(MySpecializedToggleButton owner);
    };
}
```

実装型の生成されるコンストラクターは次のようになります。

```cppwinrt
// MySpecializedToggleButtonAutomationPeer.cpp
...
MySpecializedToggleButtonAutomationPeer::MySpecializedToggleButtonAutomationPeer
    (MyNamespace::MySpecializedToggleButton const& owner)
{
    ...
}
...
```

唯一のない部分は、基底クラスにコンストラクター パラメーターを渡す必要があることです。 上述の F バインド ポリモーフィズム パターンを覚えていますか。 C++/WinRT で使われるそのパターンの詳細を理解したら、基底クラスと呼ばれるものが何かを理解することができます (または実装クラスのヘッダー ファイルだけを参照することができます)。 これは、このケースで基底クラス コンストラクターを呼び出す方法です。

```cppwinrt
// MySpecializedToggleButtonAutomationPeer.cpp
...
MySpecializedToggleButtonAutomationPeer::MySpecializedToggleButtonAutomationPeer
    (MyNamespace::MySpecializedToggleButton const& owner) : 
    MySpecializedToggleButtonAutomationPeerT<MySpecializedToggleButtonAutomationPeer>(owner)
{
    ...
}
...
```

基底クラス コンストラクターは、**ToggleButton** を期待します。 **MySpecializedToggleButton** は **ToggleButton** "*です*"。

(基底クラスにコンストラクター パラメーターを渡すために) 上記で説明した編集を行うまで、コンパイラは、コンストラクターにフラグを設定し、(この場合は)  **MySpecializedToggleButtonAutomationPeer_base&lt;MySpecializedToggleButtonAutomationPeer&gt;** と呼ばれる型で利用可能な適切な既定のコンストラクターがないことを指摘します。 実際には、実装型の基底クラスの基底クラスです。

## <a name="namespaces-projected-types-implementation-types-and-factories"></a>名前空間: 投影型、実装型、ファクトリ

このトピックで前に説明したように、C++/WinRT のランタイム クラスは、複数の名前空間に複数の C++ クラスの形式で存在します。 したがって、名前 **MyRuntimeClass** の意味は、**winrt::MyProject** 名前空間内と **winrt::MyProject::implementation** 名前空間内では異なります。 現在のコンテキストがどちらの名前空間かを認識し、別の名前空間の名前が必要な場合は名前空間プレフィックスを使います。 問題になる名前空間についてさらに詳しく見てみましょう。

- **winrt::MyProject**。 この名前空間には投影型が含まれます。 投影型のオブジェクトはプロキシです。基本的に基になるオブジェクトへのスマート ポインターであり、基になるオブジェクトは自分のプロジェクト内で実装されている場合も、別のコンパイル単位内で実装されている場合もあります。
- **winrt::MyProject::implementation**。 この名前空間には、実装型が含まれます。 実装型のオブジェクトはポインターではありません。値であり、完全な C++ スタック オブジェクトです。 実装型を直接構築しないでください。代わりに、[**winrt::make**](/uwp/cpp-ref-for-winrt/make) を呼び出し、テンプレート パラメーターとして実装型を渡します。 このトピックでは前に動作する **winrt::make** の例を示しました。別の例については、「[XAML コントロール: C++/WinRT プロパティへのバインド](binding-property.md#add-a-property-of-type-bookstoreviewmodel-to-mainpage)」をご覧ください。 「[直接割当ての診断](./diag-direct-alloc.md)」もご覧ください。
- **winrt::MyProject::factory_implementation**。 この名前空間にはファクトリが含まれます。 この名前空間のオブジェクトでは、[**IActivationFactory**](/windows/win32/api/activation/nn-activation-iactivationfactory) がサポートされています。

次の表では、異なるコンテキストで使う必要がある最小限の名前空間の修飾を示します。

|コンテキスト内の名前空間|実装型を指定する場合|実装型を指定する場合|
|-|-|-|
|**winrt::MyProject**|`MyRuntimeClass`|`implementation::MyRuntimeClass`|
|**winrt::MyProject::implementation**|`MyProject::MyRuntimeClass`|`MyRuntimeClass`|

> [!IMPORTANT]
> 実装から投影型を返す必要がある場合は、`MyRuntimeClass myRuntimeClass;` を記述することにより実装型をインスタンス化しないように注意してください。 その場合の適切な手法とコードは、このトピックのセクション「[実装型とインターフェイスをインスタンス化して返す](#instantiating-and-returning-implementation-types-and-interfaces)」で示してあります。
>
> そのシナリオでの `MyRuntimeClass myRuntimeClass;` に関する問題は、スタック上に **winrt::MyProject::implementation::MyRuntimeClass** オブジェクトが作成されることです。 その (実装型の) オブジェクトは、いくつかの点で投影型のように動作します。同じ方法でそのオブジェクトのメソッドを呼び出すことができ、投影型に変換することさえできます。 ただし、スコープが終了すると、C++ の通常のルールに従って、オブジェクトは破棄されます。 そのため、そのオブジェクトへの投影型 (スマート ポインター) を返した場合、そのポインターは中ぶらりんになります。
>
> この種のメモリ破損のバグを診断するのは困難です。 したがって、デバッグ ビルドの場合は、C++/WinRT アサーションでスタック検出機能を使ってこの誤りをキャッチすると役に立ちます。 ただし、コルーチンはヒープ上に割り当てられるので、誤りがコルーチン内にある場合、この誤りに関するヘルプは得られません。 詳細については、「[直接割当ての診断](./diag-direct-alloc.md)」を参照してください。

## <a name="using-projected-types-and-implementation-types-with-various-cwinrt-features"></a>さまざまな C++/WinRT 機能での投影型と実装型の使用

以下では、型が必要な C++/WinRT のさまざまな機能と、必要な型の種類 (投影型、実装型、両方) を示します。

|機能|受け入れられる型|メモ|
|-|-|-|
|`T` (スマート ポインターを表す)|投影|誤った実装型の使用については、「[名前空間: 投影型、実装型、ファクトリ](#namespaces-projected-types-implementation-types-and-factories)」の注意をご覧ください。|
|`agile_ref<T>`|Both|実装型を使う場合は、コンストラクターの引数を `com_ptr<T>` にする必要があります。|
|`com_ptr<T>`|実装|投影型を使うと、次のエラーが発生します: `'Release' is not a member of 'T'`。|
|`default_interface<T>`|Both|実装型を使った場合、最初に実装されたインターフェイスが返されます。|
|`get_self<T>`|実装|投影型を使うと、次のエラーが発生します: `'_abi_TrustLevel': is not a member of 'T'`。|
|`guid_of<T>()`|Both|既定のインターフェイスの GUID が返されます。|
|`IWinRTTemplateInterface<T>`<br>|投影|実装型を使ってもコンパイルされますが、それは誤りです。「[名前空間: 投影型、実装型、ファクトリ](#namespaces-projected-types-implementation-types-and-factories)」の注意をご覧ください。|
|`make<T>`|実装|投影型を使うと、次のエラーが発生します: `'implements_type': is not a member of any direct or indirect base class of 'T'`|
| `make_agile(T const&amp;)`|Both|実装型を使う場合は、引数を `com_ptr<T>` にする必要があります。|
| `make_self<T>`|実装|投影型を使うと、次のエラーが発生します: `'Release': is not a member of any direct or indirect base class of 'T'`|
| `name_of<T>`|投影|実装型を使った場合、既定のインターフェイスの文字列化された GUID が返されます。|
| `weak_ref<T>`|Both|実装型を使う場合は、コンストラクターの引数を `com_ptr<T>` にする必要があります。|

## <a name="opt-in-to-uniform-construction-and-direct-implementation-access"></a>均一コンストラクションのオプトインと、実装への直接アクセス

ここでは、オプトインされる C++/WinRT 2.0 の機能について説明します。ただし、新しいプロジェクトでは既定で有効になります。 既存のプロジェクトの場合は、`cppwinrt.exe` ツールを構成することでオプトインする必要があります。 Visual Studio で、プロジェクトのプロパティ **[共通プロパティ]**  >  **[C++/WinRT]**  >  **[最適化]** を *[はい]* に設定します。 その効果として、プロジェクト ファイルに `<CppWinRTOptimized>true</CppWinRTOptimized>` が追加されます。 また、コマンド ラインから `cppwinrt.exe` を呼び出すときにスイッチを追加するのと同じ効果があります。

`-opt[imize]` スイッチを使用すると、一般に "*均一コンストラクション*" と呼ばれる機能が有効になります。 均一 (または "*統合*") コンストラクションでは、C++/WinRT 言語プロジェクション自体を使用して、実装型 (アプリケーションで使用するためにコンポーネントによって実装された型) が効率的に作成され、使用されます。ローダーの問題はありません。

この機能について説明する前に、まず均一コンストラクションが "*ない*" 状況を見てみましょう。 説明のために、このサンプル Windows ランタイム クラスから始めます。

```idl
// MyClass.idl
namespace MyProject
{
    runtimeclass MyClass
    {
        MyClass();
        void Method();
        static void StaticMethod();
    }
}
```

C++/WinRT ライブラリを使い慣れている開発者は、次のようなクラスを使用できます。

```cppwinrt
using namespace winrt::MyProject;

MyClass c;
c.Method();
MyClass::StaticMethod();
```

また、示されている使用中のコードがこのクラスを実装する同じコンポーネント内に存在しなかった場合、これは完全に合理的です。 言語プロジェクションとして、C++/WinRT では開発者が ABI (Windows ランタイムで定義される COM ベースのアプリケーション バイナリ インターフェイス) から遮蔽されます。 C++/WinRT では、実装が直接呼び出されず、ABI を経由します。

その結果、**MyClass** オブジェクト (`MyClass c;`) を構築しているコード行で、C++/WinRT プロジェクションによってクラスまたはアクティベーション ファクトリを取得するために [**RoGetActivationFactory**](/windows/win32/api/roapi/nf-roapi-rogetactivationfactory) が呼び出され、そのファクトリを使用してオブジェクトが作成されます。 最後の行でも同様にファクトリを使用して、静的なメソッド呼び出しのように見えるものが作成されます。 このすべてにおいて、クラスを登録し、モジュールで [**DllGetActivationFactory**](/previous-versions/br205771(v=vs.85)) エントリ ポイントを実装する必要があります。 C++/WinRT にはとても高速なファクトリ キャッシュがあるので、このいずれもコンポーネントを使用するアプリケーションの問題の原因にはなりません。 問題は、コンポーネント内で若干問題のある処理を行ったことだけです。

まず、C++/WinRT ファクトリ キャッシュの速度に関係なく、**RoGetActivationFactory** (またはファクトリ キャッシュを経由する後続の呼び出し) を通じた呼び出しは、実装を直接呼び出すよりも時間がかかります。 **RoGetActivationFactory** に続いて [**IActivationFactory::ActivateInstance**](/windows/win32/api/activation/nf-activation-iactivationfactory-activateinstance)、さらに [**QueryInterface**](/windows/win32/api/unknwn/nf-unknwn-iunknown-queryinterface(refiid_void)) を呼び出すことは、明らかに、ローカルに定義された型に対して C++ `new` 式を使用する場合ほど効率的ではありません。 その結果、経験豊富な C++/WinRT 開発者は、コンポーネント内にオブジェクトを作成するときの [**winrt::make**](/uwp/cpp-ref-for-winrt/make) または [**winrt::make_self**](/uwp/cpp-ref-for-winrt/make-self) ヘルパー関数の使用に慣れています。

```cppwinrt
// MyClass c;
MyProject::MyClass c{ winrt::make<implementation::MyClass>() };
```

しかし、ご覧のとおり、これはあまり便利でも簡潔でもありません。 オブジェクトを作成するにはヘルパー関数を使用する必要があります。また、実装型と投影型を明確に区別する必要もあります。

次に、プロジェクションを使用してクラスを作成することは、そのアクティベーション ファクトリがキャッシュされることを意味します。 通常、これは目的どおりですが、呼び出しを行っているのと同じモジュール (DLL) 内にファクトリが存在する場合は、事実上 DLL を固定し、アンロードされないようにしています。 多くの場合、これは問題になりませんが、一部のシステム コンポーネントではアンロードをサポートする "*必要があります*"。

ここで、"*均一コンストラクション*" という用語が登場します。 作成コードが、クラスを使用しているだけのプロジェクト内に存在するのか、それとも実際にクラスを "*実装*" しているプロジェクト内に存在するのかにかかわらず、同じ構文を自由に使用してオブジェクトを自由に作成できます。

```cppwinrt
// MyProject::MyClass c{ winrt::make<implementation::MyClass>() };
MyClass c;
```

`-opt[imize]` スイッチを使用してコンポーネント プロジェクトをビルドすると、言語プロジェクションを使用した呼び出しは、実装型を直接作成する **winrt::make** 関数の同じ効率的な呼び出しにコンパイルされます。 これにより、構文が単純で予測可能になり、ファクトリを使用した呼び出しによるパフォーマンスの低下が回避され、プロセス内のコンポーネントの固定が回避されます。 コンポーネント プロジェクトに加えて、これは XAML アプリケーションにも役立ちます。 同じアプリケーション内に実装されたクラスの **RoGetActivationFactory** をバイパスすることで、コンポーネントの外部にクラスがある場合とまったく同じ方法で (登録する必要なしに) クラスを構築できます。

均一コンストラクションは、ファクトリによって内部的に処理される "*任意*" の呼び出しに適用されます。 実際には、これは最適化がコンストラクターと静的メンバーの両方に対して機能することを意味します。 次に、元の例をもう一度示します。

```cppwinrt
MyClass c;
c.Method();
MyClass::StaticMethod();
```

`-opt[imize]` がない場合、最初のステートメントと最後のステートメントでは、ファクトリ オブジェクトを介した呼び出しが必要になります。 `-opt[imize]` が "*ある*" 場合、どちらも当てはまりません。 これらの呼び出しは、実装に対して直接コンパイルされ、インライン化される可能性もあります。 `-opt[imize]` (つまり "*直接実装*" アクセス) について話すときによく使用される他の用語にも言及しています。

言語プロジェクションは便利ですが、実装に直接アクセスできる場合は、それを利用すれば可能な範囲で最も効率的なコードを生成することができるので、そうするべきです。 C++/WinRT でこれを行うことができます。プロジェクションの安全性と生産性から離れることは強制されません。

言語プロジェクションがその実装型に到達して直接アクセスできるようにするにはコンポーネントが連携する必要があるので、これは破壊的変更です。 C++/WinRT はヘッダーのみのライブラリなので、内部を参照して何が行われているかを確認できます。 `-opt[imize]` がない場合、**MyClass** コンストラクターと **StaticMethod** メンバーは、このようなプロジェクションによって定義されます。

```cppwinrt
namespace winrt::MyProject
{
    inline MyClass::MyClass() :
        MyClass(impl::call_factory<MyClass>([](auto&& f){
            return f.template ActivateInstance<MyClass>(); }))
    {
    }
    inline void MyClass::StaticMethod()
    {
        impl::call_factory<MyClass, MyProject::IClassStatics>([&](auto&& f) {
            return f.StaticMethod(); });
    }
}
```

上記のすべてに追従する必要はありません。目的は、両方の呼び出しに **call_factory** という名前の関数の呼び出しが含まれていることを示すことです。 これは、これらの呼び出しにファクトリ キャッシュが含まれており、実装に直接アクセスしていないことを示す手掛かりです。 `-opt[imize]` が "*ある*" 場合、これらの同じ関数はまったく定義されていません。 代わりに、プロジェクションによって宣言され、その定義はコンポーネントに委ねられます。

その後、コンポーネントでは実装を直接呼び出す定義を提供できます。 これで、破壊的変更に到達しました。 これらの定義は `-component` と `-opt[imize]` の両方を使用する場合に生成され、`Type.g.cpp` という名前のファイルに表示されます。ここで、*Type* は実装されているランタイム クラスの名前です。 そのため、既存のプロジェクト内で `-opt[imize]` を初めて有効にしたときに、さまざまなリンカー エラーが発生する可能性があります。 物事をまとめるには、生成されたファイルを実装に含める必要があります。

この例では、(`-opt[imize]` が使用されているかどうかに関係なく) `MyClass.h` はこのようになります。

```cppwinrt
// MyClass.h
#pragma once
#include "MyClass.g.h"
 
namespace winrt::MyProject::implementation
{
    struct MyClass : ClassT<MyClass>
    {
        MyClass() = default;
 
        static void StaticMethod();
        void Method();
    };
}
namespace winrt::MyProject::factory_implementation
{
    struct MyClass : ClassT<MyClass, implementation::MyClass>
    {
    };
}
```

`MyClass.cpp` にすべてがまとめられています。

```cppwinrt
#include "pch.h"
#include "MyClass.h"
#include "MyClass.g.cpp" // !!It's important that you add this line!!
 
namespace winrt::MyProject::implementation
{
    void MyClass::StaticMethod()
    {
    }
 
    void MyClass::Method()
    {
    }
}
```

したがって、既存のプロジェクト内で均一コンストラクションを使用するには、各実装の `.cpp` ファイルを編集して、実装クラスの包含 (および定義) の後に `#include <Sub/Namespace/Type.g.cpp>` を含める必要があります。 このファイルでは、プロジェクションが未定義のままの関数の定義が提供されます。 `MyClass.g.cpp` ファイル内で、これらの定義は次のようになります。

```cppwinrt
namespace winrt::MyProject
{
    MyClass::MyClass() :
        MyClass(make<MyProject::implementation::MyClass>())
    {
    }
    void MyClass::StaticMethod()
    {
        return MyProject::implementation::MyClass::StaticMethod();
    }
}
```

また、実装の効率的な直接呼び出しによってプロジェクションを正常に完了して、ファクトリ キャッシュの呼び出しを回避し、リンカーに適合させます。

`-opt[imize]` によって最後に行われることは、C++/WinRT 1.0 に必要だった厳密な型の結合を排除することで増分ビルドがはるかに高速になるような方法で、プロジェクトの `module.g.cpp` (DLL の **DllGetActivationFactory** と **DllCanUnloadNow** のエクスポートを実装するのに役立つファイル) の実装を変更することです。 これは通常、"*型消去されたファクトリ*" と呼ばれます。 `-opt[imize]` がない場合、コンポーネントに対して生成される `module.g.cpp` ファイルでは、最初にご自身のすべての実装クラス (この例では `MyClass.h`) の定義をインクルードします。 その後、このような各クラス用の実装ファクトリが直接作成されます。

```cppwinrt
if (requal(name, L"MyProject.MyClass"))
{
    return winrt::detach_abi(winrt::make<winrt::MyProject::factory_implementation::MyClass>());
}
```

この場合も、すべての詳細に追従する必要はありません。 確認しておくと役立つのは、これにはコンポーネントによって実装されているすべてのクラスに完全な定義が必要であるということです。 1 つの実装に対する変更が原因で `module.g.cpp` が再コンパイルされるので、これは内部ループに大きな影響を与える可能性があります。 `-opt[imize]` では、これが適用されません。 代わりに、生成された `module.g.cpp` ファイルに対して 2 つの処理が行われます。 1 つ目は、実装クラスがインクルードされなくなったことです。 この例では、`MyClass.h` がまったくインクルードされません。 代わりに、実装についての知識なしで実装ファクトリが作成されます。

```cppwinrt
void* winrt_make_MyProject_MyClass();
 
if (requal(name, L"MyProject.MyClass"))
{
    return winrt_make_MyProject_MyClass();
}
```

当然ながら、定義を含める必要はありません。また、**winrt_make_Component_Class** 関数の定義の解決はリンカーによって行われる必要があります。 もちろん、生成された (および均一コンストラクションをサポートするために以前に追加した) `MyClass.g.cpp` ファイルでもこの関数が定義されているので、これについて考える必要はありません。 この例に対して生成される `MyClass.g.cpp` ファイルの全体を次に示します。

```cppwinrt
void* winrt_make_MyProject_MyClass()
{
    return winrt::detach_abi(winrt::make<winrt::MyProject::factory_implementation::MyClass>());
}
namespace winrt::MyProject
{
    MyClass::MyClass() :
        MyClass(make<MyProject::implementation::MyClass>())
    {
    }
    void MyClass::StaticMethod()
    {
        return MyProject::implementation::MyClass::StaticMethod();
    }
}
```

ご覧のように、**winrt_make_MyProject_MyClass** 関数では、実装のファクトリが直接作成されます。 これはすべて、特定の実装を適切に変更でき、`module.g.cpp` の再コンパイルはまったく不要であることを意味します。 `module.g.cpp` が更新されるのは Windows ランタイム クラスを追加または削除したときだけで、再コンパイルが必要です。

## <a name="overriding-base-class-virtual-methods"></a>基底クラスの仮想メソッドのオーバーライド

基底クラスと派生クラスが両方ともアプリで定義されたクラスであっても、仮想メソッドが祖父母 Windows ランタイム クラスに定義されている場合、派生クラスは仮想メソッドで問題を発生する可能性があります。 実際には、これは XAML クラスから派生した場合に発生します。 このセクションの残りの部分は、[派生クラス](./move-to-winrt-from-cx.md#derived-classes)の例から続きます。

```cppwinrt
namespace winrt::MyNamespace::implementation
{
    struct BasePage : BasePageT<BasePage>
    {
        void OnNavigatedFrom(Windows::UI::Xaml::Navigation::NavigationEventArgs const& e);
    };

    struct DerivedPage : DerivedPageT<DerivedPage>
    {
        void OnNavigatedFrom(Windows::UI::Xaml::Navigation::NavigationEventArgs const& e);
    };
}
```

階層は [**Windows::UI::Xaml::Controls::Page**](/uwp/api/windows.ui.xaml.controls.page) \<- **BasePage** \<- **DerivedPage** です。 **BasePage::OnNavigatedFrom** メソッドは [**Page::OnNavigatedFrom**](/uwp/api/windows.ui.xaml.controls.page.onnavigatedfrom) を適切にオーバーライドしますが、**DerivedPage::OnNavigatedFrom** は **BasePage::OnNavigatedFrom** をオーバーライドしません。

ここでは、**DerivedPage** は **BasePage** から **IPageOverrides** vtable を再利用します。これは、**IPageOverrides::OnNavigatedFrom** メソッドをオーバーライドできないことを意味します。 考えられる 1 つの解決策では、**BasePage** 自体をテンプレート クラスにし、その実装をヘッダー ファイルに完全に含める必要がありますが、これでは受け入れがたいほど物事が複雑になります。

回避策として、**OnNavigatedFrom** メソッドを基底クラスに明示的な仮想として宣言します。 このようにして、**DerivedPage::IPageOverrides::OnNavigatedFrom** の vtable エントリが **BasePage::IPageOverrides::OnNavigatedFrom** を呼び出すと、producer が **BasePage::OnNavigatedFrom** を呼び出し、最終的にこれが (その仮想性が原因で) **DerivedPage::OnNavigatedFrom** を呼び出します。

```cppwinrt
namespace winrt::MyNamespace::implementation
{
    struct BasePage : BasePageT<BasePage>
    {
        // Note the `virtual` keyword here.
        virtual void OnNavigatedFrom(Windows::UI::Xaml::Navigation::NavigationEventArgs const& e);
    };

    struct DerivedPage : DerivedPageT<DerivedPage>
    {
        void OnNavigatedFrom(Windows::UI::Xaml::Navigation::NavigationEventArgs const& e);
    };
}
```

これには、クラス階層のすべてのメンバーが、**OnNavigatedFrom** メソッドの戻り値とパラメーター型に同意する必要があります。 同意しない場合は、上記のバージョンを仮想メソッドとして使用し、代替をラップする必要があります。

## <a name="important-apis"></a>重要な API
* [winrt::com_ptr 構造体テンプレート](/uwp/cpp-ref-for-winrt/com-ptr)
* [winrt::com_ptr::copy_from 関数](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrcopy_from-function)
* [winrt::from_abi 関数テンプレート](/uwp/cpp-ref-for-winrt/from-abi)
* [winrt::get_self 関数テンプレート](/uwp/cpp-ref-for-winrt/get-self)
* [winrt::implements 構造体テンプレート](/uwp/cpp-ref-for-winrt/implements)
* [winrt::make 関数テンプレート](/uwp/cpp-ref-for-winrt/make)
* [winrt::make_self 関数テンプレート](/uwp/cpp-ref-for-winrt/make-self)
* [winrt::Windows::Foundation::IUnknown::as 関数](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function)

## <a name="related-topics"></a>関連トピック
* [C++/WinRT でのイベントの作成](./author-events.md)
* [C++/WinRT で API を使用する](./consume-apis.md)
* [C++/WinRT を使用した Windows ランタイム コンポーネント](../winrt-components/create-a-windows-runtime-component-in-cppwinrt.md)
* [XAML コントロール: C++/WinRT プロパティへのバインド](./binding-property.md)