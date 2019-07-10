---
description: C++/WinRT では、コレクションを実装したり、渡すときの時間と手間を大幅に減らすことができる、関数と基本クラスが提供されます。
title: C++/WinRT でのコレクション
ms.date: 04/23/2019
ms.topic: article
keywords: Windows 10, uwp, 標準, c++, cpp, winrt ,プロジェクション, コレクション
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 8417897fd44d5357f40d8414102d0cbff6d16476
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/13/2019
ms.locfileid: "64745209"
---
# <a name="collections-with-cwinrt"></a>C++/WinRT でのコレクション

Windows ランタイム コレクションには、内部的に複雑な移動パーツが多数あります。 ただし、コレクション オブジェクトを Windows ランタイム関数に渡したり、独自のコレクション プロパティやコレクション型を実装したりする場合は、[C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) に役に立つ関数と基底クラスがあります。 これらの機能を利用すると、複雑さが解消され、時間と労力の点で多くのオーバーヘッドがなくなります。

[**IVector**](/uwp/api/windows.foundation.collections.ivector_t_) は、要素の任意のランダムアクセス コレクションによって実装される Windows ランタイム インターフェイスです。 ご自分で **IVector** を実装する場合は、[**IIterable**](/uwp/api/windows.foundation.collections.iiterable_t_)、[**IVectorView**](/uwp/api/windows.foundation.collections.ivectorview_t_)、および[**IIterator**](/uwp/api/windows.foundation.collections.iiterator_t_) も実装する必要があります。 カスタム コレクション型が*必要*な場合でも、大変な作業です。 ただし、**std::vector** (または **std::map**、または **std::unordered_map**) にデータがあり、必要な操作がそれを Windows ランタイム API に渡すことのみの場合、可能であれば、そのレベルの作業を行わないようにします。 また、それを回避することは*可能です*。C++/WinRT を使用すると、コレクションを効率的に、またわずかな労力で作成できるからです。

「[XAML アイテム コントロール: C++/WinRT コレクションへのバインド](binding-collection.md)」も参照してください。

## <a name="helper-functions-for-collections"></a>コレクションのヘルパー関数

### <a name="general-purpose-collection-empty"></a>空の汎用コレクション

このセクションでは、最初は空のコレクションを作成してから、作成 "*後*" に設定するシナリオについて説明します。

汎用コレクションを実装する型の新しいオブジェクトを取得するには、[**winrt::single_threaded_vector**](/uwp/cpp-ref-for-winrt/single-threaded-vector) 関数テンプレートを呼び出します。 オブジェクトは [**IVector**](/uwp/api/windows.foundation.collections.ivector_t_) として返されます。これは、返されたオブジェクトの関数とプロパティを呼び出すためのインターフェイスです。

以下のコード例を **Windows コンソール アプリケーション (C++/WinRT)** プロジェクトのメイン ソース コード ファイルに直接コピーして貼り付ける場合は、最初にプロジェクト プロパティで **[プリコンパイル済みヘッダーを使用しない]** を設定します。

```cppwinrt
// main.cpp
#include <winrt/Windows.Foundation.Collections.h>
#include <iostream>
using namespace winrt;

int main()
{
    winrt::init_apartment();

    Windows::Foundation::Collections::IVector<int> coll{ winrt::single_threaded_vector<int>() };
    coll.Append(1);
    coll.Append(2);
    coll.Append(3);

    for (auto const& el : coll)
    {
        std::cout << el << std::endl;
    }

    Windows::Foundation::Collections::IVectorView<int> view{ coll.GetView() };
}
```

上記のコード例からわかるように、コレクションを作成した後は、要素を追加し、それらを繰り返し処理し、通常は API から受け取った Windows ランタイム コレクション オブジェクトと同じようにオブジェクトを扱います。 コレクションに対して不変のビューが必要な場合は、次に示すように [**IVector::GetView**](/uwp/api/windows.foundation.collections.ivector-1.getview) を呼び出すことができます。 上記のパターン (コレクションの作成と消費) は、API にデータを渡したり、API からデータを受け取ったりするシンプルなシナリオに適しています。 [**IIterable**](/uwp/api/windows.foundation.collections.iiterable_t_) が想定される場所であれば、**IVector** または **IVectorView** を渡すことができます。

上のコード例では、**winrt::init_apartment** を呼び出と、Windows ランタイムのスレッドが初期化されます。既定では、マルチスレッド アパートメントです。 この呼び出しによって COM も初期化されます。

### <a name="general-purpose-collection-primed-from-data"></a>データから準備された汎用コレクション

このセクションでは、同時にコレクションを作成し、それを設定するシナリオについて説明します。

前のコード例の **Append** の呼び出しによるオーバーヘッドを回避できます。 既にソース データを持っている場合や、Windows ランタイム コレクション オブジェクトを作成する前にソース データを入力する方が好ましい場合があります。 その実行方法を次に示します。

```cppwinrt
auto coll1{ winrt::single_threaded_vector<int>({ 1,2,3 }) };

std::vector<int> values{ 1,2,3 };
auto coll2{ winrt::single_threaded_vector<int>(std::move(values)) };

for (auto const& el : coll2)
{
    std::cout << el << std::endl;
}
```

上記の `coll1` と同様に、データを含む一時オブジェクトを **winrt::single_threaded_vector** に渡すことができます。 また、(再びアクセスしないと想定して) **std::vector** を関数に移動することもできます。 いずれの場合も、*rvalue* を関数に渡しています。 これにより、コンパイラは効率的になり、データのコピーを回避できます。 *rvalue* の詳細については、「[値のカテゴリ、およびそれらへの参照](cpp-value-categories.md)」を参照してください。

XAML アイテム コントロールをコレクションにバインドしたければ、そうすることができます。 ただし、[**ItemsControl.ItemsSource**](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) プロパティを正しく設定するには、**IInspectable** の型 **IVector** (または [**IBindableObservableVector**](/uwp/api/windows.ui.xaml.interop.ibindableobservablevector) などの相互運用性型) の値に設定する必要があります。

バインドに適した型のコレクションを作成し、それに要素を追加するコード例を次に示します。 このコード例のコンテキストについては、「[XAML アイテム コントロール: C++/WinRT コレクションへのバインド](binding-collection.md)」を参照してください。

```cppwinrt
auto bookSkus{ winrt::single_threaded_vector<Windows::Foundation::IInspectable>() };
bookSkus.Append(winrt::make<Bookstore::implementation::BookSku>(L"Moby Dick"));
```

何もコピーすることなく、データから Windows ランタイム コレクションを作成し、そのビューを取得して API に渡すことができます。

```cppwinrt
std::vector<float> values{ 0.1f, 0.2f, 0.3f };
Windows::Foundation::Collections::IVectorView<float> view{ winrt::single_threaded_vector(std::move(values)).GetView() };
```

上記の例では、作成したコレクションを XAML アイテム コントロールにバインド*できます*。ただし、コレクションは監視可能ではありません。

### <a name="observable-collection"></a>監視可能なコレクション

*監視可能*なコレクションを実装する型の新しいオブジェクトを取得するには、任意の要素型を持つ [**winrt::single_threaded_observable_vector**](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector) 関数テンプレートを呼び出します。 ただし、監視可能なコレクションを XAML アイテム コントロールへのバインドに適したものにするには、要素型として **IInspectable** を使用します。

オブジェクトは [**IObservableVector**](/uwp/api/windows.foundation.collections.iobservablevector_t_) として返されます。これは、返されたオブジェクトの関数とプロパティをご自身 (またはバインドされているコントロール) が呼び出すためのインターフェイスです。

```cppwinrt
auto bookSkus{ winrt::single_threaded_observable_vector<Windows::Foundation::IInspectable>() };
```

ユーザー インターフェイス (UI) コントロールを監視可能なコレクションにバインドする方法の詳細とコード例については、「[XAML アイテム コントロール: C++/WinRT コレクションへのバインド](binding-collection.md)」を参照してください。

### <a name="associative-collection-map"></a>関連コレクション (map)

これまで見てきた 2 つの関数の関連コレクション バージョンがあります。

- [**winrt::single_threaded_map**](/uwp/cpp-ref-for-winrt/single-threaded-map) 関数テンプレートでは、監視不可能な関連コレクションが [**IMap**](/uwp/api/windows.foundation.collections.imap_k_v_) として返されます。
- [**winrt::single_threaded_observable_map**](/uwp/cpp-ref-for-winrt/single-threaded-observable-map) 関数テンプレートでは、監視可能な関連コレクションが [**IObservableMap**](/uwp/api/windows.foundation.collections.iobservablemap_k_v_) として返されます。

必要に応じて、これらのコレクションにデータを追加するには、型 **std::map** または **std::unordered_map** の *rvalue* を関数に渡します。

```cppwinrt
auto coll1{
    winrt::single_threaded_map<winrt::hstring, int>(std::map<winrt::hstring, int>{
        { L"AliceBlue", 0xfff0f8ff }, { L"AntiqueWhite", 0xfffaebd7 }
    })
};

std::map<winrt::hstring, int> values{
    { L"AliceBlue", 0xfff0f8ff }, { L"AntiqueWhite", 0xfffaebd7 }
};
auto coll2{ winrt::single_threaded_map<winrt::hstring, int>(std::move(values)) };
```

### <a name="single-threaded"></a>シングル スレッド

これらの関数の名前に含まれる "シングル スレッド" は、それらに同時実行機能がないことを示しています。言い換えると、それらはスレッドセーフではありません。 これらの関数から返されるオブジェクトはすべてアジャイルなので、スレッドについての話はアパートメントとは無関係です (「[C++/WinRT でのアジャイル オブジェクト](agile-objects.md)」を参照してください)。 単に、オブジェクトがシングルスレッドであるということのみです。 また、これは、アプリケーション バイナリ インターフェイス (ABI) を介してデータを一方向または逆方向で渡すだけの場合に完全に適しています。

## <a name="base-classes-for-collections"></a>コレクションの基底クラス

完全な柔軟性のために独自のカスタム コレクションを実装する場合、困難な実行方法は避けたいものです。 たとえば、*C++/WinRT の基底クラスを利用しない*カスタム ベクトル ビューはこのようになります。

```cppwinrt
...
using namespace winrt;
using namespace Windows::Foundation::Collections;
...
struct MyVectorView :
    implements<MyVectorView, IVectorView<float>, IIterable<float>>
{
    // IVectorView
    float GetAt(uint32_t const) { ... };
    uint32_t GetMany(uint32_t, winrt::array_view<float>) const { ... };
    bool IndexOf(float, uint32_t&) { ... };
    uint32_t Size() { ... };

    // IIterable
    IIterator<float> First() const { ... };
};
...
IVectorView<float> view{ winrt::make<MyVectorView>() };
```

代わりに、[**winrt::vector_view_base**](/uwp/cpp-ref-for-winrt/vector-view-base) 構造体テンプレートからカスタム ベクトル ビューを派生させ、**get_container** 関数を実装してデータを保持するコンテナーを公開する方がはるかに簡単です。

```cppwinrt
struct MyVectorView2 :
    implements<MyVectorView2, IVectorView<float>, IIterable<float>>,
    winrt::vector_view_base<MyVectorView2, float>
{
    auto& get_container() const noexcept
    {
        return m_values;
    }

private:
    std::vector<float> m_values{ 0.1f, 0.2f, 0.3f };
};
```

**get_container** から返されるコンテナーは、**winrt::vector_view_base** が想定する **begin** および **end** インターフェイスを提供する必要があります。 上記の例に示すように、**std::vector** はそれを提供します。 ただし、独自のカスタム コンテナーを含め、同じコントラクトを満たす任意のコンテナーを返すことができます。

```cppwinrt
struct MyVectorView3 :
    implements<MyVectorView3, IVectorView<float>, IIterable<float>>,
    winrt::vector_view_base<MyVectorView3, float>
{
    auto get_container() const noexcept
    {
        struct container
        {
            float const* const first;
            float const* const last;

            auto begin() const noexcept
            {
                return first;
            }

            auto end() const noexcept
            {
                return last;
            }
        };

        return container{ m_values.data(), m_values.data() + m_values.size() };
    }

private:
    std::array<float, 3> m_values{ 0.2f, 0.3f, 0.4f };
};
```

これらは、カスタム コレクションの実装を支援するために C++/WinRT が提供する基底クラスです。

### <a name="winrtvectorviewbaseuwpcpp-ref-for-winrtvector-view-base"></a>[winrt::vector_view_base](/uwp/cpp-ref-for-winrt/vector-view-base)

上記のコード例を参照してください。

### <a name="winrtvectorbaseuwpcpp-ref-for-winrtvector-base"></a>[winrt::vector_base](/uwp/cpp-ref-for-winrt/vector-base)

```cppwinrt
struct MyVector :
    implements<MyVector, IVector<float>, IVectorView<float>, IIterable<float>>,
    winrt::vector_base<MyVector, float>
{
    auto& get_container() const noexcept
    {
        return m_values;
    }

    auto& get_container() noexcept
    {
        return m_values;
    }

private:
    std::vector<float> m_values{ 0.1f, 0.2f, 0.3f };
};
```

### <a name="winrtobservablevectorbaseuwpcpp-ref-for-winrtobservable-vector-base"></a>[winrt::observable_vector_base](/uwp/cpp-ref-for-winrt/observable-vector-base)

```cppwinrt
struct MyObservableVector :
    implements<MyObservableVector, IObservableVector<float>, IVector<float>, IVectorView<float>, IIterable<float>>,
    winrt::observable_vector_base<MyObservableVector, float>
{
    auto& get_container() const noexcept
    {
        return m_values;
    }

    auto& get_container() noexcept
    {
        return m_values;
    }

private:
    std::vector<float> m_values{ 0.1f, 0.2f, 0.3f };
};
```

### <a name="winrtmapviewbaseuwpcpp-ref-for-winrtmap-view-base"></a>[winrt::map_view_base](/uwp/cpp-ref-for-winrt/map-view-base)

```cppwinrt
struct MyMapView :
    implements<MyMapView, IMapView<winrt::hstring, int>, IIterable<IKeyValuePair<winrt::hstring, int>>>,
    winrt::map_view_base<MyMapView, winrt::hstring, int>
{
    auto& get_container() const noexcept
    {
        return m_values;
    }

private:
    std::map<winrt::hstring, int> m_values{
        { L"AliceBlue", 0xfff0f8ff }, { L"AntiqueWhite", 0xfffaebd7 }
    };
};
```

### <a name="winrtmapbaseuwpcpp-ref-for-winrtmap-base"></a>[winrt::map_base](/uwp/cpp-ref-for-winrt/map-base)

```cppwinrt
struct MyMap :
    implements<MyMap, IMap<winrt::hstring, int>, IMapView<winrt::hstring, int>, IIterable<IKeyValuePair<winrt::hstring, int>>>,
    winrt::map_base<MyMap, winrt::hstring, int>
{
    auto& get_container() const noexcept
    {
        return m_values;
    }

    auto& get_container() noexcept
    {
        return m_values;
    }

private:
    std::map<winrt::hstring, int> m_values{
        { L"AliceBlue", 0xfff0f8ff }, { L"AntiqueWhite", 0xfffaebd7 }
    };
};
```

### <a name="winrtobservablemapbaseuwpcpp-ref-for-winrtobservable-map-base"></a>[winrt::observable_map_base](/uwp/cpp-ref-for-winrt/observable-map-base)

```cppwinrt
struct MyObservableMap :
    implements<MyObservableMap, IObservableMap<winrt::hstring, int>, IMap<winrt::hstring, int>, IMapView<winrt::hstring, int>, IIterable<IKeyValuePair<winrt::hstring, int>>>,
    winrt::observable_map_base<MyObservableMap, winrt::hstring, int>
{
    auto& get_container() const noexcept
    {
        return m_values;
    }

    auto& get_container() noexcept
    {
        return m_values;
    }

private:
    std::map<winrt::hstring, int> m_values{
        { L"AliceBlue", 0xfff0f8ff }, { L"AntiqueWhite", 0xfffaebd7 }
    };
};
```

## <a name="important-apis"></a>重要な API
* [ItemsControl.ItemsSource プロパティ](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource)
* [IObservableVector インターフェイス](/uwp/api/windows.foundation.collections.iobservablevector_t_)
* [IVector インターフェイス](/uwp/api/windows.foundation.collections.ivector_t_)
* [winrt::map_base 構造体テンプレート](/uwp/cpp-ref-for-winrt/map-base)
* [winrt::map_view_base 構造体テンプレート](/uwp/cpp-ref-for-winrt/map-view-base)
* [winrt::observable_map_base 構造体テンプレート](/uwp/cpp-ref-for-winrt/observable-map-base)
* [winrt::observable_vector_base 構造体テンプレート](/uwp/cpp-ref-for-winrt/observable-vector-base)
* [winrt::single_threaded_observable_map 関数テンプレート](/uwp/cpp-ref-for-winrt/single-threaded-observable-map)
* [winrt::single_threaded_map 関数テンプレート](/uwp/cpp-ref-for-winrt/single-threaded-map)
* [winrt::single_threaded_observable_vector 関数テンプレート](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector)
* [winrt::single_threaded_vector 関数テンプレート](/uwp/cpp-ref-for-winrt/single-threaded-vector)
* [winrt::vector_base 構造体テンプレート](/uwp/cpp-ref-for-winrt/vector-base)
* [winrt::vector_view_base 構造体テンプレート](/uwp/cpp-ref-for-winrt/vector-view-base)

## <a name="related-topics"></a>関連トピック
* [値のカテゴリと、その参照](cpp-value-categories.md)
* [XAML アイテム コントロール: C++/WinRT コレクションへのバインド](binding-collection.md)
