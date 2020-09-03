---
description: このトピックでは、[C#](/visualstudio/get-started/csharp) プロジェクト内のソース コードを [C++/WinRT](./intro-to-using-cpp-with-winrt.md) の同等のコードに移植することに関する技術的な詳細を総合的に分類します。
title: C# から C++/WinRT への移行
ms.date: 07/15/2019
ms.topic: article
keywords: windows 10, uwp, 標準, c++, cpp, winrt, プロジェクション, 移植, 移行, C#
ms.localizationpriority: medium
ms.openlocfilehash: e3c6b4213ee5edf8f9a5878b4f9a1a7095220bcd
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89157326"
---
# <a name="move-to-cwinrt-from-c"></a>C# から C++/WinRT への移行

このトピックでは、[C#](/visualstudio/get-started/csharp) プロジェクト内のソース コードを [C++/WinRT](./intro-to-using-cpp-with-winrt.md) の同等のコードに移植することに関する技術的な詳細を総合的に分類します。

ユニバーサル Windows プラットフォーム (UWP) アプリ サンプルの移植についてのケース スタディに関しては、[C# から C++/WinRT への Clipboard サンプルの移植](./clipboard-to-winrt-from-csharp.md)についての関連トピックを参照してください。 チュートリアルの手順を実行し、サンプルを自分で移植することにより、移植を練習し経験を得ることができます。

## <a name="how-to-prepare-and-what-to-expect"></a>事前の準備と、これから行うこと

[C# から C++/WinRT への Clipboard サンプルの移植](./clipboard-to-winrt-from-csharp.md)に関するケース スタディでは、プロジェクトを C++/WinRT に移植する際に行うソフトウェア設計に関する様々な決定の例が示されています。 それで、既存のコードの動作をはっきりと理解しておくことは、移植の良い準備となります。 そうすれば、アプリの機能やコードの構造に関して概要を理解できるため、行う決定によって正しい方向に進むことができます。

行うべき移植に関する変更の種類については、4 つのカテゴリに分類できます。

- [**言語プロジェクションでの移植**](#changes-that-involve-the-language-projection)。 Windows ランタイム (WinRT) では様々なプログラミング言語への "*プロジェクション*" が行われます。 これらの言語プロジェクションのそれぞれは、対応するプログラミング言語にとって自然なものになるように設計されています。 C# の場合、一部の Windows ランタイムの型は .NET の型としてプロジェクションが行われます。 そのため、たとえば [**System.Collections.Generic.IReadOnlyList\<T\>** ](/dotnet/api/system.collections.generic.ireadonlylist-1) は [**Windows.Foundation.Collections.IVectorView\<T\>** ](/uwp/api/windows.foundation.collections.ivectorview-1) に変換することになります。 また C# では、一部の Windows ランタイムの操作は便利な C# の言語機能としてプロジェクションが行われます。 一例として、C# では、`+=` の演算子構文を使用して、イベント処理デリゲートの登録を行います。 それで、そのような言語の機能は、実行されている基本的な操作 (この例ではイベント登録) に変換することになります。
- [**言語構文の移植**](#changes-that-involve-the-language-syntax)。 これらの変更の多くは単純な機械的な変換で、あるシンボルを他のシンボルに置き換えます。 たとえば、ドット (`.`) を 2 重コロン (`::`) に変更します。
- [**言語プロシージャの移植**](#changes-that-involve-procedures-within-the-language)。 これらのうちの一部は単純で、変更の繰り返しです (`myObject.MyProperty` から `myObject.MyProperty()` など)。 大きな変更が必要なものもあります (**System.Text.StringBuilder** を使用するプロシージャを、**std::wostringstream** を使用するものに移植する、など)。
- [**C++/WinRT に特有の移植関連のタスク**](#porting-related-tasks-that-are-specific-to-cwinrt) Windows ランタイムの特定の詳細は、背後で C# によって暗黙的に処理されます。 これらの詳細は、C++/WinRT で明示的に処理されます。 一例として、`.idl` ファイルを使用してランタイム クラスを定義することがあります。

このトピックの残りの部分は、この分類に従って構成されています。

## <a name="changes-that-involve-the-language-projection"></a>言語プロジェクションに関連する変更

||C#|C++/WinRT|関連項目|
|-|-|-|-|
|型指定されていないオブジェクト|`object`、または [**System.Object**](/dotnet/api/system.object)|[**Windows::Foundation::IInspectable**](/windows/win32/api/inspectable/nn-inspectable-iinspectable)|[**EnableClipboardContentChangedNotifications** メソッドの移植](./clipboard-to-winrt-from-csharp.md#enableclipboardcontentchangednotifications)|
|プロジェクション名前空間|`using System;`|`using namespace Windows::Foundation;`||
||`using System.Collections.Generic;`|`using namespace Windows::Foundation::Collections;`||
|コレクションのサイズ|`collection.Count`|`collection.Size()`|[**BuildClipboardFormatsOutputString** メソッドの移植](./clipboard-to-winrt-from-csharp.md#buildclipboardformatsoutputstring)|
|一般的なコレクション型|[**IList\<T\>** ](/dotnet/api/system.collections.generic.ilist-1)、および **Add** で要素を追加。|[**IVector\<T\>** ](/uwp/api/windows.foundation.collections.ivector-1)、および **Append** で要素を追加。 いずれかの場所で **std::vector** を使用する場合は、**push_back** で要素を追加。||
|読み取り専用のコレクション型|[**IReadOnlyList\<T\>** ](/dotnet/api/system.collections.generic.ireadonlylist-1)|[**IVectorView\<T\>** ](/uwp/api/windows.foundation.collections.ivectorview-1)|[**BuildClipboardFormatsOutputString** メソッドの移植](./clipboard-to-winrt-from-csharp.md#buildclipboardformatsoutputstring)|
|クラス メンバーとしてのイベント ハンドラー デリゲート|`myObject.EventName += Handler;`|`token = myObject.EventName({ get_weak(), &Class::Handler });`|[**EnableClipboardContentChangedNotifications** メソッドの移植](./clipboard-to-winrt-from-csharp.md#enableclipboardcontentchangednotifications)|
|イベント ハンドラー デリゲートの取り消し|`myObject.EventName -= Handler;`|`myObject.EventName(token);`|[**EnableClipboardContentChangedNotifications** メソッドの移植](./clipboard-to-winrt-from-csharp.md#enableclipboardcontentchangednotifications)|
|連想コンテナー|[**IDictionary\<K, V\>** ](/dotnet/api/system.collections.generic.idictionary-2)|[**IMap\<K, V\>** ](/uwp/api/windows.foundation.collections.imap-2)||
|ベクター メンバーのアクセス|`x = v[i];`<br>`v[i] = x;`|`x = v.GetAt(i);`<br>`v.SetAt(i, x);`||

### <a name="registerrevoke-an-event-handler"></a>イベント ハンドラーの登録または取り消し

C++/WinRT では、「[C++/WinRT でのデリゲートを使用したイベントの処理](./handle-events.md)」で説明されているように、イベント ハンドラー デリゲートの登録または取り消しを行うための構文オプションがいくつかあります。 [**EnableClipboardContentChangedNotifications** メソッドの移植](./clipboard-to-winrt-from-csharp.md#enableclipboardcontentchangednotifications)に関する記事も参照してください。

イベントの受信側 (イベントを処理するオブジェクト) が破棄されようとしているときなど、場合によってはイベント ハンドラーを取り消して、イベント ソース (イベントを発生させるオブジェクト) が破棄されたオブジェクトを呼び出さないようにしたいことがあります。 「[登録済みデリゲートの取り消し](./handle-events.md#revoke-a-registered-delegate)」を参照してください。 そのような場合は、イベント ハンドラーに **event_token** メンバー変数を作成します。 例として、[**EnableClipboardContentChangedNotifications** メソッドの移植](./clipboard-to-winrt-from-csharp.md#enableclipboardcontentchangednotifications)に関する記事を参照してください。

また、XAML マークアップでイベント ハンドラーを登録することもできます。

```xaml
<Button x:Name="OpenButton" Click="OpenButton_Click" />
```

C# では、**OpenButton_Click** メソッドはプライベートにすることができ、XAML は引き続きこれを、*OpenButton* によって発生した [**ButtonBase.Click**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) イベントに接続できます。

C++/WinRT では、"*XAML マークアップで登録したい場合*"、**OpenButton_Click** メソッドは[実装型](./author-apis.md)でパブリックである必要があります。 命令型コードでのみイベント ハンドラーを登録したい場合、イベント ハンドラーはパブリックである必要はありません。

```cppwinrt
namespace winrt::MyProject::implementation
{
    struct MyPage : MyPageT<MyPage>
    {
        void OpenButton_Click(
            winrt::Windows:Foundation::IInspectable const& sender,
            winrt::Windows::UI::Xaml::RoutedEventArgs const& args);
    }
};
```

または、登録する XAML ページを実装型の friend にし、**OpenButton_Click** をプライベートにすることもできます。

```cppwinrt
namespace winrt::MyProject::implementation
{
    struct MyPage : MyPageT<MyPage>
    {
    private:
        friend MyPageT;
        void OpenButton_Click(
            winrt::Windows:Foundation::IInspectable const& sender,
            winrt::Windows::UI::Xaml::RoutedEventArgs const& args);
    }
};
```

最後のシナリオでは、移植する C# プロジェクトをマークアップからのイベント ハンドラーに*バインド*します (そのシナリオの背景の詳細については、「[x:Bind の 関数](../data-binding/function-bindings.md)」を参照してください)。

```xaml
<Button x:Name="OpenButton" Click="{x:Bind OpenButton_Click}" />
```

このマークアップをより単純な `Click="OpenButton_Click"` に変更することもできます。 または、必要であれば、そのマークアップをそのままにしておくこともできます。 これをサポートするために必要なのは、IDL でイベント ハンドラーを宣言することだけです。

```idl
void OpenButton_Click(Object sender, Windows.UI.Xaml.RoutedEventArgs e);
```

> [!NOTE]
> 関数を[ファイア アンド フォーゲット](./concurrency-2.md#fire-and-forget)として "*実装*" する場合でも、これを `void` として宣言します。

## <a name="changes-that-involve-the-language-syntax"></a>言語構文に関連する変更

||C#|C++/WinRT|関連項目|
|-|-|-|-|
|アクセス修飾子|`public \<member\>`|`public:`<br>&nbsp;&nbsp;&nbsp;&nbsp;`\<member\>`|[**Button_Click** メソッドの移植](./clipboard-to-winrt-from-csharp.md#button_click)|
|データ メンバーへのアクセス|`this.variable`|`this->variable`||
|非同期アクション|`async Task ...`|`IAsyncAction ...`||
|非同期操作|`async Task<T> ...`|`IAsyncOperation<T> ...`||
|fire-and-forget メソッド (つまり、非同期)|`async void ...`|`winrt::fire_and_forget ...`|[**CopyButton_Click** メソッドの移植](./clipboard-to-winrt-from-csharp.md#copybutton_click)|
|列挙型定数へのアクセス|`E.Value`|`E::Value`|[**DisplayChangedFormats** メソッドの移植](./clipboard-to-winrt-from-csharp.md#displaychangedformats)|
|協調的な待機|`await ...`|`co_await ...`|[**CopyButton_Click** メソッドの移植](./clipboard-to-winrt-from-csharp.md#copybutton_click)|
|プライベート フィールドとしての投影型のコレクション|`private List<MyRuntimeClass> myRuntimeClasses = new List<MyRuntimeClass>();`|`std::vector`<br>`<MyNamespace::MyRuntimeClass>`<br>`m_myRuntimeClasses;`||
|GUID の構築|`private static readonly Guid myGuid = new Guid("C380465D-2271-428C-9B83-ECEA3B4A85C1");`|`winrt::guid myGuid{ 0xC380465D, 0x2271, 0x428C, { 0x9B, 0x83, 0xEC, 0xEA, 0x3B, 0x4A, 0x85, 0xC1} };`||
|名前空間の区切り記号|`A.B.T`|`A::B::T`||
|Null|`null`|`nullptr`|[**UpdateStatus** メソッドの移植](./clipboard-to-winrt-from-csharp.md#updatestatus)|
|型オブジェクトの取得|`typeof(MyType)`|`winrt::xaml_typename<MyType>()`|[**Scenarios** プロパティの移植](./clipboard-to-winrt-from-csharp.md#scenarios)|
|メソッドのパラメーター宣言|`MyType`|`MyType const&`|[パラメーターの引き渡し](./concurrency.md#parameter-passing)|
|非同期メソッドのパラメーター宣言|`MyType`|`MyType`|[パラメーターの引き渡し](./concurrency.md#parameter-passing)|
|静的メソッドの呼び出し|`T.Method()`|`T::Method()`||
|文字列|`string` または **System.String**|[**winrt::hstring**](/uw/cpp-ref-for-winrt/hstring)|[C++/WinRT での文字列の処理](./strings.md)|
|文字列リテラル|`"a string literal"`|`L"a string literal"`|[コンストラクター、**Current**、**FEATURE_NAME** の移植](./clipboard-to-winrt-from-csharp.md#the-constructor-current-and-feature_name)|
|推論された (または推測された) 型|`var`|`auto`|[**BuildClipboardFormatsOutputString** メソッドの移植](./clipboard-to-winrt-from-csharp.md#buildclipboardformatsoutputstring)|
|using ディレクティブ|`using A.B.C;`|`using namespace A::B::C;`|[コンストラクター、**Current**、**FEATURE_NAME** の移植](./clipboard-to-winrt-from-csharp.md#the-constructor-current-and-feature_name)|
|逐語的/未加工の文字列リテラル|`@"verbatim string literal"`|`LR"(raw string literal)"`|[**DisplayToast** メソッドの移植](./clipboard-to-winrt-from-csharp.md##displaytoast)|

> [!NOTE]
> ヘッダー ファイルに特定の名前空間の `using namespace` ディレクティブが含まれていない場合、その名前空間のすべての型名を完全修飾する必要があります。または、コンパイラーがそれらを見つけられるように十分に修飾する必要があります。 例として、[**DisplayToast** メソッドの移植](./clipboard-to-winrt-from-csharp.md##displaytoast)に関する記事を参照してください。

### <a name="porting-classes-and-members"></a>クラスとメンバーの移植

C# の各型に関して、Windows ランタイムの型に移植するか、通常の C++ のクラス、構造体、列挙型に移植するかを決める必要があります。 詳細について、およびこれらの決定方法についての詳細な例については、[C# から C++/WinRT への Clipboard サンプルの移植](./clipboard-to-winrt-from-csharp.md)に関する記事を参照してください。

C# プロパティは一般的に、アクセサー関数、ミューテーター関数、バッキング データ メンバーになります。 詳細や例については、[**IsClipboardContentChangedEnabled** プロパティの移植](./clipboard-to-winrt-from-csharp.md#isclipboardcontentchangedenabled)に関する記事を参照してください。

非静的フィールドについては、[実装型](./author-apis.md)のデータ メンバーにしてください。

C# の静的フィールドは、C++/WinRT の静的アクセサーおよびミューテーター関数になります。 詳細や例については、[コンストラクタ、**Current**、**FEATURE_NAME** の移植](./clipboard-to-winrt-from-csharp.md#the-constructor-current-and-feature_name)に関する記事を参照してください。

メンバー関数については、再度、それぞれが IDL に属しているかどうか、または実装型のパブリックもしくはプライベートのメンバー関数であるかどうかを決める必要があります。 決定方法に関する詳細と例については、「[**MainPage** 型の IDL](./clipboard-to-winrt-from-csharp.md#idl-for-the-mainpage-type)」を参照してください。

### <a name="porting-xaml-markup-and-asset-files"></a>XAML マークアップ、資産ファイルの移植

[C# から C++/WinRT への Clipboard サンプルの移植](./clipboard-to-winrt-from-csharp.md)に関する記事では、"*同じ*" XAML マークアップ (リソースを含む) と資産ファイルを、C# および C++/WinRT プロジェクト全体で使用することができました。 ある場合には、そうするためにマークアップの編集が必要になります。 「[**MainPage** の移植を完了するために必要な XAML とスタイルをコピーする](./clipboard-to-winrt-from-csharp.md#copy-the-xaml-and-styles-necessary-to-finish-up-porting-mainpage)」を参照してください。

## <a name="changes-that-involve-procedures-within-the-language"></a>言語内のプロシージャに関連する変更

||C#|C++/WinRT|関連項目|
|-|-|-|-|
|非同期メソッドでの有効期間の管理|なし|`auto lifetime{ get_strong() };`<br>`auto lifetime = get_strong();`|[**CopyButton_Click** メソッドの移植](./clipboard-to-winrt-from-csharp.md#copybutton_click)|
|破棄|`using (var t = v)`|`auto t{ v };`<br>`t.Close(); // or let wrapper destructor do the work`|[**CopyImage** メソッドの移植](./clipboard-to-winrt-from-csharp.md#copyimage)|
|オブジェクトの構築|`new MyType(args)`|`MyType{ args }`<br>`MyType(args)`|[**Scenarios** プロパティの移植](./clipboard-to-winrt-from-csharp.md#scenarios)|
|初期化されていない参照の作成|`MyType myObject;`|`MyType myObject{ nullptr };`<br>`MyType myObject = nullptr;`|[コンストラクター、**Current**、**FEATURE_NAME** の移植](./clipboard-to-winrt-from-csharp.md#the-constructor-current-and-feature_name)|
|オブジェクトを、引数を持つ変数に構築|`var myObject = new MyType(args);`|`auto myObject{ MyType{ args } };` <br>`auto myObject{ MyType(args) };` <br>`auto myObject = MyType{ args };` <br>`auto myObject = MyType(args);` <br>`MyType myObject{ args };` <br>`MyType myObject(args);`|[**Footer_Click** メソッドの移植](./clipboard-to-winrt-from-csharp.md#footer_click)|
|オブジェクトを、引数を持たない変数に構築|`var myObject = new T();`|`MyType myObject;`|[**BuildClipboardFormatsOutputString** メソッドの移植](./clipboard-to-winrt-from-csharp.md#buildclipboardformatsoutputstring)|
|オブジェクトの初期化の短縮形|`var p = new FileOpenPicker{`<br>&nbsp;&nbsp;&nbsp;&nbsp;`ViewMode = PickerViewMode.List`<br>`};`|`FileOpenPicker p;`<br>`p.ViewMode(PickerViewMode::List);`||
|一括のベクター操作|`var p = new FileOpenPicker{`<br>&nbsp;&nbsp;&nbsp;&nbsp;`FileTypeFilter = { ".png", ".jpg", ".gif" }`<br>`};`|`FileOpenPicker p;`<br>`p.FileTypeFilter().ReplaceAll({ L".png", L".jpg", L".gif" });`|[**CopyButton_Click** メソッドの移植](./clipboard-to-winrt-from-csharp.md#copybutton_click)|
|コレクションの反復処理|`foreach (var v in c)`|`for (auto&& v : c)`|[**BuildClipboardFormatsOutputString** メソッドの移植](./clipboard-to-winrt-from-csharp.md#buildclipboardformatsoutputstring)|
|例外のキャッチ|`catch (Exception ex)`|`catch (winrt::hresult_error const& ex)`|[**PasteButton_Click** メソッドの移植](./clipboard-to-winrt-from-csharp.md#pastebutton_click)|
|例外の詳細|`ex.Message`|`ex.message()`|[**PasteButton_Click** メソッドの移植](./clipboard-to-winrt-from-csharp.md#pastebutton_click)|
|プロパティ値の取得|`myObject.MyProperty`|`myObject.MyProperty()`|[**NotifyUser** メソッドの移植](./clipboard-to-winrt-from-csharp.md#notifyuser)|
|プロパティ値の設定|`myObject.MyProperty = value;`|`myObject.MyProperty(value);`||
|プロパティ値の増分|`myObject.MyProperty += v;`|`myObject.MyProperty(thing.Property() + v);`<br>文字列の場合、ビルダーに切り替え||
|ToString()|`myObject.ToString()`|`winrt::to_hstring(myObject)`|[ToString()](#tostring)|
|言語文字列を Windows ランタイム文字列に|なし|`winrt::hstring{ s }`||
|文字列の作成|`StringBuilder builder;`<br>`builder.Append(...);`|`std::wostringstream builder;`<br>`builder << ...;`|[文字列の作成](#string-building)|
|文字列補間|`$"{i++}) {s.Title}"`|[**winrt::to_hstring**](/uwp/cpp-ref-for-winrt/to-hstring) および [**winrt::hstring::operator+** ](/uwp/cpp-ref-for-winrt/hstring#operator-concatenation-operator)|[**OnNavigatedTo** メソッドの移植](./clipboard-to-winrt-from-csharp.md#onnavigatedto)|
|比較のための空の文字列|**System.String.Empty**|[**winrt::hstring::empty**](/uwp/cpp-ref-for-winrt/hstring#hstringempty-function)|[**UpdateStatus** メソッドの移植](./clipboard-to-winrt-from-csharp.md#updatestatus)|
|空の文字列の作成|`var myEmptyString = String.Empty;`|`winrt::hstring myEmptyString{ L"" };`||
|ディクショナリ操作|`map[k] = v; // replaces any existing`<br>`v = map[k]; // throws if not present`<br>`map.ContainsKey(k)`|`map.Insert(k, v); // replaces any existing`<br>`v = map.Lookup(k); // throws if not present`<br>`map.HasKey(k)`||
|型変換 (エラー発生時にスロー)|`(MyType)v`|`v.as<MyType>()`|[**Footer_Click** メソッドの移植](./clipboard-to-winrt-from-csharp.md#footer_click)|
|型変換 (エラー発生時は null)|`v as MyType`|`v.try_as<MyType>()`|[**PasteButton_Click** メソッドの移植](./clipboard-to-winrt-from-csharp.md#pastebutton_click)|
|x:Name を持つ XAML 要素はプロパティ|`MyNamedElement`|`MyNamedElement()`|[コンストラクター、**Current**、**FEATURE_NAME** の移植](./clipboard-to-winrt-from-csharp.md#the-constructor-current-and-feature_name)|
|UI スレッドへの切り替え|**CoreDispatcher.RunAsync**|**CoreDispatcher.RunAsync** または [**winrt::resume_foreground**](/uwp/cpp-ref-for-winrt/resume-foreground)|[**NotifyUser** メソッドの移植](./clipboard-to-winrt-from-csharp.md#notifyuser) および [**HistoryAndRoaming** メソッドの移植](./clipboard-to-winrt-from-csharp.md#historyandroaming)|
|XAML ページの命令型コードでの UI 要素の構築|「[UI 要素の構築](#ui-element-construction)」を参照|「[UI 要素の構築](#ui-element-construction)」を参照||

以下のセクションでは、表内のいくつかの項目について詳細を説明します。

### <a name="ui-element-construction"></a>UI 要素の構築

次のコード例で、XAML ページの命令型コードで UI 要素を構築する方法を示します。

```csharp
var myTextBlock = new TextBlock()
{
    Text = "Text",
    Style = (Windows.UI.Xaml.Style)this.Resources["MyTextBlockStyle"]
};
```

```cppwinrt
TextBlock myTextBlock;
myTextBlock.Text(L"Text");
myTextBlock.Style(
    winrt::unbox_value<Windows::UI::Xaml::Style>(
        Resources().Lookup(
            winrt::box_value(L"MyTextBlockStyle")
        )
    )
);
```

### <a name="tostring"></a>ToString()

C# 型には [Object.ToString](/dotnet/api/system.object.tostring) メソッドが用意されています。

```csharp
int i = 2;
var s = i.ToString(); // s is a System.String with value "2".
```

C++/WinRT ではこの機能は直接提供されていませんが、代替機能があります。

```cppwinrt
int i{ 2 };
auto s{ std::to_wstring(i) }; // s is a std::wstring with value L"2".
```

また、C++/WinRT では、限られた数の型のための [**winrt:: to_hstring**](/uwp/cpp-ref-for-winrt/to-hstring) もサポートしています。 stringify が必要なその他の型のオーバーロードを追加する必要があります。

| Language | Stringify int | Stringify enum |
| - | - | - |
| C# | `string result = "hello, " + intValue.ToString();`<br>`string result = $"hello, {intValue}";` | `string result = "status: " + status.ToString();`<br>`string result = $"status: {status}";` |
| C++/WinRT | `hstring result = L"hello, " + to_hstring(intValue);` | `// must define overload (see below)`<br>`hstring result = L"status: " + to_hstring(status);` |

列挙型の stringify の場合は、**winrt::to_hstring** の実装を指定する必要があります。

```cppwinrt
namespace winrt
{
    hstring to_hstring(StatusEnum status)
    {
        switch (status)
        {
        case StatusEnum::Success: return L"Success";
        case StatusEnum::AccessDenied: return L"AccessDenied";
        case StatusEnum::DisabledByPolicy: return L"DisabledByPolicy";
        default: return to_hstring(static_cast<int>(status));
        }
    }
}
```

このような文字列化は、多くの場合、データ バインディングによって暗黙的に使用されます。

```xaml
<TextBlock>
You have <Run Text="{Binding FlowerCount}"/> flowers.
</TextBlock>
<TextBlock>
Most recent status is <Run Text="{x:Bind LatestOperation.Status}"/>.
</TextBlock>
```

これらのバインディングは、バインドされたプロパティの **winrt::to_hstring** を実行します。 2 番目の例 **(statusenum**) の場合は、**winrt::to_hstring** の独自のオーバーロードを指定する必要があります。そうしないと、コンパイラ エラーが発生します。

[**Footer_Click** メソッドの移植](./clipboard-to-winrt-from-csharp.md#footer_click)に関する記事も参照してください。

### <a name="string-building"></a>文字列の作成

C# には、文字列の作成用に組み込みの [**StringBuilder**](/dotnet/api/system.text.stringbuilder) 型があります。

| | C# | C++/WinRT |
|-|-|-|
| 文字列の作成 | `StringBuilder builder;`<br>`builder.Append(...);` | `std::wostringstream builder;`<br>`builder << ...;` |
| Windows ランタイム文字列を追加し、null を保持 | `builder.Append(s);` | `builder << std::wstring_view{ s };` |
| 改行を追加 |`builder.Append(Environment.NewLine);` | `builder << std::endl;` |
| 結果にアクセス | `s = builder.ToString();` | `ws = builder.str();` |

[**BuildClipboardFormatsOutputString** メソッドの移植](./clipboard-to-winrt-from-csharp.md#buildclipboardformatsoutputstring)および [**DisplayChangedFormats** メソッドの移植](./clipboard-to-winrt-from-csharp.md#displaychangedformats) に関する記事も参照してください。

### <a name="running-code-on-the-main-ui-thread"></a>メイン UI スレッドでコードを実行する 

この例は、「[バーコード スキャナーのサンプル](/samples/microsoft/windows-universal-samples/barcodescanner/)」に基づいています。

C# プロジェクトで、メイン UI スレッドで何らかの操作を行いたい場合は、通常は次のように [**CoreDispatcher.RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync) メソッドを使用します。

```csharp
private async void Watcher_Added(DeviceWatcher sender, DeviceInformation args)
{
    await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
    {
        // Do work on the main UI thread here.
    });
}
```

C++/WinRT では、ずっとシンプルに表現できます。 最初の中断ポイント (この例では `co_await`) の後にパラメーターにアクセスすることを想定して、パラメーターを値渡しで受け取っていることに注目してください。 詳細については、「[パラメーターの引き渡し](./concurrency.md#parameter-passing)」を参照してください。

```cppwinrt
winrt::fire_and_forget Watcher_Added(DeviceWatcher sender, winrt::DeviceInformation args)
{
    co_await Dispatcher();
    // Do work on the main UI thread here.
}
```

既定以外の優先順位で操作を行う必要がある場合、[**winrt::resume_foreground**](/uwp/cpp-ref-for-winrt/resume-foreground) 関数を確認してください。これには、優先されるオーバーロードがあります。 **winrt::resume_foreground** への呼び出しを待つ方法を示すコード例については、「[スレッドの関係を考慮したプログラミング](./concurrency-2.md#programming-with-thread-affinity-in-mind)」を参照してください。

## <a name="porting-related-tasks-that-are-specific-to-cwinrt"></a>C++/WinRT に特有の移植関連のタスク

### <a name="define-your-runtime-classes-in-idl"></a>IDL でランタイム クラスを定義する

「[**MainPage** 型の IDL](./clipboard-to-winrt-from-csharp.md#idl-for-the-mainpage-type)」および「[`.idl` ファイルを統合する](./clipboard-to-winrt-from-csharp.md#consolidate-your-idl-files)」を参照してください。

### <a name="include-the-cwinrt-windows-namespace-header-files-that-you-need"></a>必要な C++/WinRT Windows 名前空間ヘッダー ファイルをインクルードする

C++/WinRT では、Windows 名前空間から型を使用する場合は、対応する C++/WinRT Windows 名前空間ヘッダー ファイルを含めます。 例については、[**NotifyUser** メソッドの移植](./clipboard-to-winrt-from-csharp.md#notifyuser)に関する記事を参照してください。

### <a name="boxing-and-unboxing"></a>ボックス化とボックス化解除

C++ では、スカラーが自動的にオブジェクト内にボックス化されます。 C++/Winrt では、[**winrt::box_value**](/uwp/cpp-ref-for-winrt/box-value) 関数を明示的に呼び出す必要があります。 どちらの言語でも、明示的にボックス化解除する必要があります。 [C++/WinRT を使用したボックス化とボックス化解除](./boxing.md)に関するページを参照してください。

次の表では、これらの定義を使用します。

| C# | C++/WinRT|
|-|-|
| `int i;` | `int i;` |
| `string s;` | `winrt::hstring s;` |
| `object o;` | `IInspectable o;`|

| 操作 | C# | C++/WinRT|
|-|-|-|
| ボックス化 | `o = 1;`<br>`o = "string";` | `o = box_value(1);`<br>`o = box_value(L"string");` |
| ボックス化解除 | `i = (int)o;`<br>`s = (string)o;` | `i = unbox_value<int>(o);`<br>`s = unbox_value<winrt::hstring>(o);` |

C++/CX と C# では、Null ポインターを値型にボックス化解除しようとすると例外が発生します。 C++/WinRT はこれをプログラミング エラーと見なし、クラッシュします。 C++/Winrt では、オブジェクトが想定した型ではないケースに対処したい場合は [**winrt:: unbox_value_or**](/uwp/cpp-ref-for-winrt/unbox-value-or) 関数を使用します。

| シナリオ | C# | C++/WinRT|
|-|-|-|
| 既知の整数をボックス化解除する |`i = (int)o;` | `i = unbox_value<int>(o);` |
| o が null の場合 | `System.NullReferenceException` | クラッシュ |
| o がボックス化された int でない場合 | `System.InvalidCastException` | クラッシュ |
| int をボックス化解除し、null の場合はフォールバックを使用する。それ以外の場合はクラッシュ | `i = o != null ? (int)o : fallback;` | `i = o ? unbox_value<int>(o) : fallback;` |
| 可能であれば int をボックス化解除する。それ以外の場合はフォールバックを使用する | `i = as int? ?? fallback;` | `i = unbox_value_or<int>(o, fallback);` |

例については、[**OnNavigatedTo** メソッドの移植](./clipboard-to-winrt-from-csharp.md#onnavigatedto)および [**Footer_Click** メソッドの移植](./clipboard-to-winrt-from-csharp.md#footer_click)に関する記事を参照してください。

#### <a name="boxing-and-unboxing-a-string"></a>文字列のボックス化とボックス化解除

文字列は、ある点では値型であり、別の点では参照型です。 C# と C++/WinRT では文字列の扱いが異なります。

ABI 型 [**HSTRING**](/windows/win32/winrt/hstring) は、参照カウント文字列へのポインターです。 しかし、[**IInspectable**](/windows/win32/api/inspectable/nn-inspectable-iinspectable) から派生したものではないため、厳密に言えば*オブジェクト*ではありません。 さらに、null の **HSTRING** は空の文字列を表します。 **IInspectable** から派生したもの以外のボックス化は、[**IReference \<T\>** ](/uwp/api/windows.foundation.ireference_t_) 内にラップすることによって行われ、Windows ランタイムでは標準の実装が [**PropertyValue**](/uwp/api/windows.foundation.propertyvalue) オブジェクトの形式で行われます (カスタム型は [**PropertyType::Othertype**](/uwp/api/windows.foundation.propertytype) として報告されます)。

C# は Windows ランタイム文字列を参照型として表しますが、C++/WinRT は文字列を値型として投影します。 つまり、ボックス化された null 文字列は、どのように表されるかによって異なる表現を持つことができます。

| 動作 | C# | C++/WinRT|
|-|-|-|
| 宣言 | `object o;`<br>`string s;` | `IInspectable o;`<br>`hstring s;` |
| 文字列型のカテゴリ | 参照型 | 値の種類 |
| null **HSTRING** による投影 | `""` | `hstring{}` |
| null と `""` が同一かどうか | いいえ | はい |
| null の有効性 | `s = null;`<br>`s.Length` により NullReferenceException が生成される | `s = hstring{};`<br>`s.size() == 0` (有効) |
| オブジェクトに null 文字列を割り当てる場合 | `o = (string)null;`<br>`o == null` | `o = box_value(hstring{});`<br>`o != nullptr` |
| オブジェクトに `""` を割り当てる場合 | `o = "";`<br>`o != null` | `o = box_value(hstring{L""});`<br>`o != nullptr` |

基本的なボックス化とボックス化解除。

| 操作 | C# | C++/WinRT|
|-|-|-|
| 文字列をボックス化する | `o = s;`<br>空の文字列は非 null オブジェクトになります。 | `o = box_value(s);`<br>空の文字列は非 null オブジェクトになります。 |
| 既知の文字列をボックス化解除する | `s = (string)o;`<br>null オブジェクトは null 文字列になります。<br>文字列でない場合は InvalidCastException。 | `s = unbox_value<hstring>(o);`<br>null オブジェクトはクラッシュします。<br>文字列でない場合はクラッシュします。 |
| 使用可能な文字列をボックス化解除する | `s = o as string;`<br>null オブジェクトまたは文字列以外は null 文字列になります。<br><br>または<br><br>`s = o as string ?? fallback;`<br>null または文字列以外はフォールバックになります。<br>空の文字列は保持されます。 | `s = unbox_value_or<hstring>(o, fallback);`<br>null または文字列以外はフォールバックになります。<br>空の文字列は保持されます。 |

### <a name="making-a-class-available-to-the-binding-markup-extension"></a>クラスを {Binding} マークアップ拡張に使用できるようにする

{Binding} マークアップ拡張を使用してデータ型にデータ バインドする場合は、「[{Binding} を使って宣言されたバインディング オブジェクト](../data-binding/data-binding-in-depth.md#binding-object-declared-using-binding)」を参照してください。

### <a name="consuming-objects-from-xaml-markup"></a>XAML マークアップからのオブジェクトの使用

C# プロジェクトでは、XAML マークアップからプライベート メンバーと名前付き要素を使用できます。 ただし、C++/WinRT では、XAML [ **{x:Bind} マークアップ拡張**](../xaml-platform/x-bind-markup-extension.md)の使用によって使用されるすべてのエンティティが、IDL で公開されている必要があります。

また、ブール値へのバインドにより C# では `true` または `false` が表示されますが、C++/WinRT では **Windows.Foundation.IReference`1\<Boolean\>** が表示されます。

詳細とコード例については、[マークアップからのオブジェクトの使用](./binding-property.md#consuming-objects-from-xaml-markup)に関するページを参照してください。

### <a name="making-a-data-source-available-to-xaml-markup"></a>データ ソースを XAML マークアップで使用できるようにする

C++/WinRT バージョン 2.0.190530.8 以降では、[**winrt::single_threaded_observable_vector**](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector) により、 **[IObservableVector](/uwp/api/windows.foundation.collections.iobservablevector_t_)\<T\>** と **IObservableVector\<IInspectable\>** の両方をサポートする監視可能なベクターが作成されます。 例については、[**Scenarios** プロパティの移植](./clipboard-to-winrt-from-csharp.md#scenarios)に関する記事を参照してください。

次のような **Midl ファイル (.idl)** を作成できます (「[ランタイム クラスを Midl ファイル (.idl) にファクタリングする](./author-apis.md#factoring-runtime-classes-into-midl-files-idl)」も参照してください)。

```idl
namespace Bookstore
{
    runtimeclass BookSku { ... }

    runtimeclass BookstoreViewModel
    {
        Windows.Foundation.Collections.IObservableVector<BookSku> BookSkus{ get; };
    }

    runtimeclass MainPage : Windows.UI.Xaml.Controls.Page
    {
        MainPage();
        BookstoreViewModel MainViewModel{ get; };
    }
}
```

さらに、次のように実装できます。

```cppwinrt
// BookstoreViewModel.h
...
struct BookstoreViewModel : BookstoreViewModelT<BookstoreViewModel>
{
    BookstoreViewModel()
    {
        m_bookSkus = winrt::single_threaded_observable_vector<Bookstore::BookSku>();
        m_bookSkus.Append(winrt::make<Bookstore::implementation::BookSku>(L"To Kill A Mockingbird"));
    }
    
    Windows::Foundation::Collections::IObservableVector<Bookstore::BookSku> BookSkus();
    {
        return m_bookSkus;
    }

private:
    Windows::Foundation::Collections::IObservableVector<Bookstore::BookSku> m_bookSkus;
};
...
```

詳細については、「[XAML アイテム コントロール: C++/WinRT コレクションへのバインド](./binding-collection.md)」と「[C++/WinRT でのコレクション](./collections.md)」を参照してください。

### <a name="making-a-data-source-available-to-xaml-markup-prior-to-cwinrt-201905308"></a>データ ソースを XAML マークアップで使用できるようにする (C++/WinRT 2.0.190530.8 より前)

XAML データ バインディングでは、項目ソースが **[IIterable](/uwp/api/windows.foundation.collections.iiterable_t_)\<IInspectable\>** と、次のインターフェイスの組み合わせのいずれかを実装する必要があります。

- **IObservableVector\<IInspectable\>**
- **IBindableVector** および **INotifyCollectionChanged**
- **IBindableVector** および **IBindableObservableVector**
- **IBindableVector** 単独 (変更には対応しません)
- **IVector\<IInspectable\>**
- **IBindableIterable** (要素を反復処理してプライベート コレクションに保存します)

**IVector\<T\>** などのジェネリック インターフェイスは、実行時に検出できません。 各 **IVector\<T\>** には別のインターフェイス識別子 (IID) があり、これが **T** の関数です。すべての開発者は **T** のセットを自由に拡張できるため、XAML バインディングのコードではクエリを実行する完全なセットを明確に理解することができません。 この制約は C# では問題ではありません。**IEnumerable\<T\>** を実装するすべての CLR オブジェクトは **IEnumerable** を自動的に実装するためです。 これは、ABI レベルでは、**IObservableVector\<T\>** を実装するすべてのオブジェクトが **IObservableVector\<IInspectable\>** を自動的に実装することを意味します。

C++/WinRT ではその保証は提供されていません。 C++/WinRT ランタイム クラスが **IObservableVector\<T\>** を実装する場合は、**IObservableVector\<IInspectable\>** の実装も何らかの形で提供されるとは想定できません。

そのため、前の例では、次のように確認する必要があります。

```idl
...
runtimeclass BookstoreViewModel
{
    // This is really an observable vector of BookSku.
    Windows.Foundation.Collections.IObservableVector<Object> BookSkus{ get; };
}
```

実装は次のとおりです。

```cppwinrt
// BookstoreViewModel.h
...
struct BookstoreViewModel : BookstoreViewModelT<BookstoreViewModel>
{
    BookstoreViewModel()
    {
        m_bookSkus = winrt::single_threaded_observable_vector<Windows::Foundation::IInspectable>();
        m_bookSkus.Append(winrt::make<Bookstore::implementation::BookSku>(L"To Kill A Mockingbird"));
    }
    
    // This is really an observable vector of BookSku.
    Windows::Foundation::Collections::IObservableVector<Windows::Foundation::IInspectable> BookSkus();
    {
        return m_bookSkus;
    }

private:
    Windows::Foundation::Collections::IObservableVector<Windows::Foundation::IInspectable> m_bookSkus;
};
...
```

*m_bookSkus* 内のオブジェクトにアクセスする必要がある場合は、再度 **Bookstore::BookSku** に QI する必要があります。

```cppwinrt
Widget MyPage::BookstoreViewModel(winrt::hstring title)
{
    for (auto&& obj : m_bookSkus)
    {
        auto bookSku = obj.as<Bookstore::BookSku>();
        if (bookSku.Title() == title) return bookSku;
    }
    return nullptr;
}
```

### <a name="derived-classes"></a>派生クラス

ランタイム クラスから派生するためには、基底クラスが*構成可能*である必要があります。 C# ではクラスを構成可能にするために特別な手順を実行する必要はありませんが、C++/WinRT では必要です。 クラスを基底クラスとして使用できるようにすることを示すには、[シールされていないキーワード](/uwp/midl-3/intro#base-classes)を使用します。

```idl
unsealed runtimeclass BasePage : Windows.UI.Xaml.Controls.Page
{
    ...
}
runtimeclass DerivedPage : BasePage
{
    ...
}
```

[実装型](./author-apis.md)のヘッダー ファイルでは、派生クラスの自動生成されたヘッダーをインクルードする前に、基底クラスのヘッダー ファイルをインクルードする必要があります。 そうしないと、"この型は式として不適切に使用されています" などのエラーが表示されます。

```cppwinrt
// DerivedPage.h
#include "BasePage.h"       // This comes first.
#include "DerivedPage.g.h"  // Otherwise this header file will produce an error.

namespace winrt::MyNamespace::implementation
{
    struct DerivedPage : DerivedPageT<DerivedPage>
    {
        ...
    }
}
```

## <a name="important-apis"></a>重要な API
* [winrt 名前空間](/uwp/cpp-ref-for-winrt/winrt)

## <a name="related-topics"></a>関連トピック
* [C# チュートリアル](/visualstudio/get-started/csharp)
* [C++/WinRT](./index.md)
* [データ バインディングの詳細](../data-binding/data-binding-in-depth.md)