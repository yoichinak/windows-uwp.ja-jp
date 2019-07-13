---
description: C++/WinRT では、標準的な C++ データ型を使用して Windows ランタイム API を呼び出すことができます。
title: 標準的な C++ のデータ型と C++/WinRT
ms.date: 04/23/2019
ms.topic: article
keywords: Windows 10、uwp、標準、c++、cpp、winrt、プロジェクション、データ、型
ms.localizationpriority: medium
ms.openlocfilehash: 83d2c0c2c544d63d2806dc71bfc367613d34e23a
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/13/2019
ms.locfileid: "64745285"
---
# <a name="standard-c-data-types-and-cwinrt"></a>標準的な C++ のデータ型と C++/WinRT

[C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) では、標準的な C++ データ型 (いくつかの C++ 標準ライブラリのデータ型を含む) を使用して Windows ランタイム API を呼び出すことができます。 API に標準文字列を渡すことができるほか (「[C++/WinRT での文字列の処理](strings.md)」を参照)、意味的に等価なコレクションを期待する API に初期化リストと標準コンテナーを渡すこともできます。

## <a name="standard-initializer-lists"></a>標準的な初期化子リスト
初期化子リスト (**std::initializer_list**) は、C++ 標準ライブラリのコンストラクトです。 Windows ランタイムの特定のコンストラクターやメソッドを呼び出すときに初期化子リストを使用することができます。 たとえば、[**DataWriter::WriteBytes**](/uwp/api/windows.storage.streams.datawriter.writebytes) を呼び出すことができます。

```cppwinrt
#include <winrt/Windows.Storage.Streams.h>

using namespace winrt::Windows::Storage::Streams;

int main()
{
    winrt::init_apartment();

    InMemoryRandomAccessStream stream;
    DataWriter dataWriter{stream};
    dataWriter.WriteBytes({ 99, 98, 97 }); // the initializer list is converted to a winrt::array_view before being passed to WriteBytes.
}
```

この作業には 2 つの部分が含まれています。 最初に、**DataWriter::WriteBytes** メソッドは型が [**winrt::array_view**](/uwp/cpp-ref-for-winrt/array-view) であるパラメーターを受け取ります。

```cppwinrt
void WriteBytes(winrt::array_view<uint8_t const> value) const
```

**winrt::array_view** は C++/WinRT のカスタム型であり、連続した一連の値を安全に表します (C++/WinRT 基本ライブラリ、`%WindowsSdkDir%Include\<WindowsTargetPlatformVersion>\cppwinrt\winrt\base.h` で定義されています)。

2 番目に、**winrt::array_view** は初期化子リスト コンストラクターを持っています。

```cppwinrt
template <typename T> winrt::array_view(std::initializer_list<T> value) noexcept
```

多くの場合、プログラミングで **winrt::array_view** を認識するかどうかを選択できます。 認識*しない*ことを選択する場合は、対応する型が C++ 標準ライブラリに現れる場合に変更すべきコードはありません。

コレクション パラメーターを予期している Windows ランタイム API に初期化子リストを渡すことができます。 たとえば **StorageItemContentProperties::RetrievePropertiesAsync** です。

```cppwinrt
IAsyncOperation<IMap<winrt::hstring, IInspectable>> StorageItemContentProperties::RetrievePropertiesAsync(IIterable<winrt::hstring> propertiesToRetrieve) const;
```

次のような初期化子リストを使用してその API を呼び出すことができます。

```cppwinrt
IAsyncAction retrieve_properties_async(StorageFile const& storageFile)
{
    auto properties{ co_await storageFile.Properties().RetrievePropertiesAsync({ L"System.ItemUrl" }) };
}
```

ここでは、2 つの要因が作用しています。 最初に、呼び出し先が初期化子リストから **std::vector** を作成します (この呼び出し先は非同期であるため、そのオブジェクトを所有することができます。これは必須です)。 2 番目に、C++/WinRT は、**std::vector** を Windows ランタイムのコレクション パラメーターとして透過的に (およびコピーを導入せずに) バインドします。

## <a name="standard-arrays-and-vectors"></a>標準的な配列とベクトル
[**winrt::array_view**](/uwp/cpp-ref-for-winrt/array-view) は、**std::vector** および **std::array** からの変換コンストラクターも持っています。

```cppwinrt
template <typename C, size_type N> winrt::array_view(std::array<C, N>& value) noexcept
template <typename C> winrt::array_view(std::vector<C>& vectorValue) noexcept
```

したがって、代わりに **std::vector** を使用して **DataWriter::WriteBytes** を呼び出すことができます。

```cppwinrt
std::vector<byte> theVector{ 99, 98, 97 };
dataWriter.WriteBytes(theVector); // theVector is converted to a winrt::array_view before being passed to WriteBytes.
```

または、**std::array** を使用します。

```cppwinrt
std::array<byte, 3> theArray{ 99, 98, 97 };
dataWriter.WriteBytes(theArray); // theArray is converted to a winrt::array_view before being passed to WriteBytes.
```

C++/WinRT は、Windows ランタイムのコレクション パラメーターとして **std::vector** をバインドします。 したがって、**std::vector&lt;winrt::hstring&gt;** を渡すと、Windows ランタイムの適切な **winrt::hstring** のコレクションに変換されます。 呼び出し先が非同期である場合に留意する必要がある追加の詳細があります。 その場合の実装の詳細により、rvalue 値を指定する必要があるので、ベクトルのコピーまたは移動を実現する必要があります。 以下のコード例では、非同期呼び出し先によって受け入れられたパラメーター型のオブジェクトにベクトルの所有権を移動します (そして、移動後に `vecH` に再びアクセスしないように注意します)。 rvalue の詳細については、「[値のカテゴリ、およびそれらへの参照](cpp-value-categories.md)」を参照してください。

```cppwinrt
IAsyncAction retrieve_properties_async(StorageFile const storageFile, std::vector<winrt::hstring> vecH)
{
    auto properties{ co_await storageFile.Properties().RetrievePropertiesAsync(std::move(vecH)) };
}
```

ただし、Windows ランタイムのコレクションが予期されているところに **std::vector&lt;std::wstring&gt;** を渡すことはできません。 これは、Windows ランタイムの適切な **std::wstring** のコレクションに変換されたため、C++ 言語がそのコレクションの型パラメーターを強制的に変換しないことが原因です。 結果として、次のコード例はコンパイルされません (解決策は、上記のように、**std::vector&lt;winrt::hstring&gt;** を代わりに渡すことです)。

```cppwinrt
IAsyncAction retrieve_properties_async(StorageFile const& storageFile, std::vector<std::wstring> const& vecW)
{
    auto properties{ co_await storageFile.Properties().RetrievePropertiesAsync(std::move(vecW)) }; // error! Can't convert from vector of wstring to async_iterable of hstring.
}
```

## <a name="raw-arrays-and-pointer-ranges"></a>未処理配列、およびポインターの範囲
将来の C++ 標準ライブラリで対応する型が存在する可能性があることに注意して、選択する場合、または必要に応じて、**winrt::array_view** を直接操作することもできます。

**winrt::array_view** は、未加工配列およびさまざまな **T&ast;** (要素型へのポインター) からの変換コンストラクターを持っています。

```cppwinrt
using namespace winrt;
...
byte theRawArray[]{ 99, 98, 97 };
array_view<byte const> fromRawArray{ theRawArray };
dataWriter.WriteBytes(fromRawArray); // the winrt::array_view is passed to WriteBytes.

array_view<byte const> fromRange{ theArray.data(), theArray.data() + 2 }; // just the first two elements.
dataWriter.WriteBytes(fromRange); // the winrt::array_view is passed to WriteBytes.
```

## <a name="winrtarrayview-functions-and-operators"></a>winrt::array_view の関数と演算子
コンストラクター、演算子、関数、および反復子のホストが **winrt::array_view** に対して実装されています。 **winrt::array_view** は範囲であるため、範囲ベースの `for`、または **std::for_each** で使用できます。

その他の例や詳細については、[**winrt::array_view**](/uwp/cpp-ref-for-winrt/array-view) API リファレンス トピックをご覧ください。

## <a name="ivectorlttgt-and-standard-iteration-constructs"></a>**IVector&lt;T&gt;** と標準的な反復コンストラクト
[**SyndicationFeed.Items**](/uwp/api/windows.web.syndication.syndicationfeed.items) は、[**IVector&lt;T&gt;** ](/uwp/api/windows.foundation.collections.ivector_t_) 型のコレクション (**winrt::Windows::Foundation::Collections::IVector&lt;T&gt;** として C++/WinRT に投影される) を返す Windows ランタイム API の例です。 この型は、範囲ベースの `for` など、標準的な反復コンストラクトで使用できます。

```cppwinrt
// main.cpp
#include "pch.h"
#include <winrt/Windows.Web.Syndication.h>
#include <iostream>

using namespace winrt;
using namespace Windows::Web::Syndication;

void PrintFeed(SyndicationFeed const& syndicationFeed)
{
    for (SyndicationItem const& syndicationItem : syndicationFeed.Items())
    {
        std::wcout << syndicationItem.Title().Text().c_str() << std::endl;
    }
}
```

## <a name="c-coroutines-with-asynchronous-windows-runtime-apis"></a>非同期 Windows ランタイム API を使用した C++ コルーチン
非同期 Windows ランタイム API を呼び出すときには、引き続き[並列パターン ライブラリ (PPL)](/cpp/parallel/concrt/parallel-patterns-library-ppl) を使用できます。 しかし、多くの場合、C++ コルーチンは非同期オブジェクトとのやりとり用に、効率的でコーディングがより簡単なイディオムを提供します。 詳細とコード例については、「[C++/WinRT を使用した同時実行操作と非同期操作](concurrency.md)」を参照してください。

## <a name="important-apis"></a>重要な API
* [IVector&lt;T&gt; インターフェイス](/uwp/api/windows.foundation.collections.ivector_t_)
* [winrt::array_view 構造体テンプレート](/uwp/cpp-ref-for-winrt/array-view)

## <a name="related-topics"></a>関連トピック
* [C++/WinRT での文字列の処理](strings.md)
