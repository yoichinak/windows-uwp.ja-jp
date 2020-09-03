---
description: このトピックでは、アプリケーション バイナリ インターフェイス (ABI) と C++/WinRT オブジェクト間の変換方法について説明します。
title: C++/WinRT と ABI 間の相互運用
ms.date: 11/30/2018
ms.topic: article
keywords: Windows 10、uwp、標準、c++、cpp、winrt、プロジェクション、ポート、移行、相互運用、ABI
ms.localizationpriority: medium
ms.openlocfilehash: 71ae6245fe217277c7408a7eb6b5150900cc45d9
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89170176"
---
# <a name="interop-between-cwinrt-and-the-abi"></a>C++/WinRT と ABI 間の相互運用

このトピックでは、SDK アプリケーション バイナリ インターフェイス (ABI) と [C++/WinRT](./intro-to-using-cpp-with-winrt.md) オブジェクト間の変換方法について説明します。 これらの手法を使用すると、Windows ランタイムでこれら 2 つの手法によるプログラミングを使用するコード間を相互運用するか、ABI から C++/WinRT にコードを徐々に移動することができます。

一般に、C++/WinRT では ABI 型は **void\*** として公開されるので、プラットフォームのヘッダー ファイルをインクルードする必要はありません。

## <a name="what-is-the-windows-runtime-abi-and-what-are-abi-types"></a>Windows ランタイム ABI および ABI の種類とは何ですか。
Windows ランタイム クラス (ランタイム クラス) は実際にはアブストラクションです。 このアブストラクションは、さまざまなプログラミング言語がオブジェクトと対話できるようにするバイナリ インターフェイス (アプリケーション バイナリ インターフェイス、または ABI) を定義します。 プログラミング言語に関係なく、クライアント コードと Windows ランタイム オブジェクトとの対話は最下位レベルで発生し、クライアントの言語の構成要素はオブジェクトの ABI への呼び出しに変換されます。

フォルダー "%WindowsSdkDir%Include\10.0.17134.0\winrt" (必要に応じて、状況に合った SDK バージョン番号に調整) 内の Windows SDK ヘッダーは、Windows ランタイム ABI ヘッダー ファイルです。 このヘッダーは MIDL コンパイラによって生成されます。 次のいずれかのヘッダーを含む例を示します。

```cpp
#include <windows.foundation.h>
```

次に、特定の SDK ヘッダー内にあるいずれかの ABI 型の例を簡単に示します。 **ABI** 名前空間である **Windows::Foundation**、およびその他すべての Windows 名前空間は、**ABI** 名前空間内で SDK ヘッダーによって宣言される点に注意してください。

```cpp
namespace ABI::Windows::Foundation
{
    IUriRuntimeClass : public IInspectable
    {
    public:
        /* [propget] */ virtual HRESULT STDMETHODCALLTYPE get_AbsoluteUri(/* [retval, out] */__RPC__deref_out_opt HSTRING * value) = 0;
        ...
    }
}
```

**IUriRuntimeClass** は COM インターフェイスです。 ただしそれ以上に、ベースは **IInspectable** であるため、**IUriRuntimeClass** は Windows ランタイム インターフェイスです。 **HRESULT** は例外よりも型を返します。 **HSTRING** ハンドルなどのアーティファクトを使用します (完了したら、このハンドルを `nullptr` に戻すように設定することをお勧めします)。 これにより、アプリケーション バイナリ レベル、つまり、COM プログラミング レベルで Windows ランタイムの内容を把握できます。

Windows ランタイムはコンポーネント オブジェクト モデル (COM) API に基づいています。 この方法で Windows ランタイムにアクセスするか、*言語プロジェクション*を介してアクセスすることができます。 プロジェクションは、COM の詳細を隠し、特定の言語により自然なプログラミング エクスペリエンスを提供します。

たとえば、フォルダー "%WindowsSdkDir%Include\10.0.17134.0\cppwinrt\winrt" 内を見ると (必要に応じて、状況に合った SDK バージョン番号に調整)、C++/WinRT 言語プロジェクション ヘッダーがあります。 Windows 名前空間ごとに 1 つの ABI ヘッダーがあるように、Windows 名前空間それぞれにヘッダーがあります。 次に、いずれかの C++/WinRT ヘッダーを含む例を示します。

```cppwinrt
#include <winrt/Windows.Foundation.h>
```

このヘッダーから、上記の ABI 型に相当する C++/WinRT を簡略化して次に示します。

```cppwinrt
namespace winrt::Windows::Foundation
{
    struct Uri : IUriRuntimeClass, ...
    {
        winrt::hstring AbsoluteUri() const { ... }
        ...
    };
}
```

このインターフェイスは最新の標準的な C++ です。 **HRESULT** は削除されています (必要に応じて、C++/WinRT では例外が発生します)。 アクセサー関数は簡単な文字列オブジェクトを返します。このオブジェクトはその範囲の最後でクリーンアップされます。

このトピックでは、アプリケーション バイナリ インターフェイス (ABI) レイヤーで機能するコードと相互運用するか、またはコードを移植する場合について説明しています。

## <a name="converting-to-and-from-abi-types-in-code"></a>コード内での ABI 型の変換
安全で分かりやすくするため、双方向の変換の場合は、[**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr)、[**com_ptr::as**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptras-function)、および [**winrt::Windows::Foundation::IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function) のみを使用します。 次に、(**Console App** プロジェクト テンプレートに基づく) コード例を示します 。このコード例では、異なる断片の名前空間のエイリアスを使用して、C++/WinRT プロジェクションと ABI 間で生じる可能性のある他の名前空間の競合を処理する方法についても説明します。

```cppwinrt
// pch.h
#pragma once
#include <windows.foundation.h>
#include <unknwn.h>
#include "winrt/Windows.Foundation.h"

// main.cpp
#include "pch.h"

namespace winrt
{
    using namespace Windows::Foundation;
}

namespace abi
{
    using namespace ABI::Windows::Foundation;
};

int main()
{
    winrt::init_apartment();

    winrt::Uri uri(L"http://aka.ms/cppwinrt");

    // Convert to an ABI type.
    winrt::com_ptr<abi::IStringable> ptr{ uri.as<abi::IStringable>() };

    // Convert from an ABI type.
    uri = ptr.as<winrt::Uri>();
    winrt::IStringable uriAsIStringable{ ptr.as<winrt::IStringable>() };
}
```

**as** 関数を実装すると、[**QueryInterface**](/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_)) が呼び出されます。 [  **AddRef**](/windows/desktop/api/unknwn/nf-unknwn-iunknown-addref) のみを呼び出す下位レベルの変換を行う場合は、[**winrt::copy_to_abi**](/uwp/cpp-ref-for-winrt/copy-to-abi) と [**winrt::copy_from_abi**](/uwp/cpp-ref-for-winrt/copy-from-abi) のヘルパー関数を使用できます。 次に、この下位レベル変換を上記のコード例に追加するコード例を示します。

> [!IMPORTANT]
> ABI 型と相互運用する場合は、使用される ABI 型が C++/WinRT オブジェクトの既定のインターフェイスに対応していることが重要です。 対応していない場合、ABI 型でメソッドを呼び出すと、既定のインターフェイスの同じ vtable スロットにあるメソッドが呼び出され、予期しない結果が発生します。 [**winrt::copy_to_abi**](/uwp/cpp-ref-for-winrt/copy-from-abi) では、すべての ABI 型に対して **void\*** が使用され、呼び出し元が一致しない型を誤って使用することはないものと想定しているため、コンパイル時にこれを防ぐことはできないことにご注意ください。 これは、ABI 型がまったく使用されない可能性があるのに、C++/WinRT ヘッダーに ABI ヘッダーを参照することを要求するのを避けるためです。

```cppwinrt
int main()
{
    // The code in main() already shown above remains here.

    // Lower-level conversions that only call AddRef.

    // Convert to an ABI type.
    ptr = nullptr;
    winrt::copy_to_abi(uriAsIStringable, *ptr.put_void());

    // Convert from an ABI type.
    uri = nullptr;
    winrt::copy_from_abi(uriAsIStringable, ptr.get());
    ptr = nullptr;
}
```

次に、他の同様の下位レベルの変換手法を示しますが、ここでは ABI インターフェイスの型 (Windows SDK ヘッダーで定義されている型) に生のポインターを使用しています。

```cppwinrt
    // The code in main() already shown above remains here.

    // Copy to an owning raw ABI pointer with copy_to_abi.
    abi::IStringable* owning{ nullptr };
    winrt::copy_to_abi(uriAsIStringable, *reinterpret_cast<void**>(&owning));

    // Copy from a raw ABI pointer.
    uri = nullptr;
    winrt::copy_from_abi(uriAsIStringable, owning);
    owning->Release();
```

下位レベルの変換の場合、アドレスをコピーするだけで、[**winrt::get_abi**](/uwp/cpp-ref-for-winrt/get-abi)、[**winrt::detach_abi**](/uwp/cpp-ref-for-winrt/detach-abi)、[**winrt::attach_abi**](/uwp/cpp-ref-for-winrt/attach-abi) の各ヘルパー関数を使用できます。

`WINRT_ASSERT` はマクロ定義であり、[_ASSERTE](/cpp/c-runtime-library/reference/assert-asserte-assert-expr-macros) に展開されます。

```cppwinrt
    // The code in main() already shown above remains here.

    // Lowest-level conversions that only copy addresses

    // Convert to a non-owning ABI object with get_abi.
    abi::IStringable* non_owning{ static_cast<abi::IStringable*>(winrt::get_abi(uriAsIStringable)) };
    WINRT_ASSERT(non_owning);

    // Avoid interlocks this way.
    owning = static_cast<abi::IStringable*>(winrt::detach_abi(uriAsIStringable));
    WINRT_ASSERT(!uriAsIStringable);
    winrt::attach_abi(uriAsIStringable, owning);
    WINRT_ASSERT(uriAsIStringable);
```

## <a name="convert_from_abi-function"></a>convert_from_abi 関数
このヘルパー関数では、最小限のオーバーヘッドで生の ABI インターフェイス ポインターを同等の C++/WinRT オブジェクトに変換します。

```cppwinrt
template <typename T>
T convert_from_abi(::IUnknown* from)
{
    T to{ nullptr }; // `T` is a projected type.

    winrt::check_hresult(from->QueryInterface(winrt::guid_of<T>(),
        winrt::put_abi(to)));

    return to;
}
```

この関数は、[**QueryInterface**](/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_)) を呼び出し、要求された C++/WinRT 型の既定のインターフェイスを照会するだけです。

既に説明したように、C++/WinRT オブジェクトから同等の ABI インターフェイス ポインターへの変換には、ヘルパー関数は必要ありません。 [  **winrt::Windows::Foundation::IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function) (または [**try_as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntry_as-function)) メンバー関数を使用して、要求されたインターフェイスを照会するだけです。 **as** 関数と **try_as** 関数は、要求された ABI 型をラップする [**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr) オブジェクトを返します。

## <a name="code-example-using-convert_from_abi"></a>convert_from_abi を使用したコード例
次に、このヘルパー関数を表す実践的なコード例を示します。

```cppwinrt
// pch.h
#pragma once
#include <windows.foundation.h>
#include <unknwn.h>
#include "winrt/Windows.Foundation.h"

// main.cpp
#include "pch.h"
#include <iostream>

using namespace winrt;
using namespace Windows::Foundation;

namespace winrt
{
    using namespace Windows::Foundation;
}

namespace abi
{
    using namespace ABI::Windows::Foundation;
};

namespace sample
{
    template <typename T>
    T convert_from_abi(::IUnknown* from)
    {
        T to{ nullptr }; // `T` is a projected type.

        winrt::check_hresult(from->QueryInterface(winrt::guid_of<T>(),
            winrt::put_abi(to)));

        return to;
    }
    inline auto put_abi(winrt::hstring& object) noexcept
    {
        return reinterpret_cast<HSTRING*>(winrt::put_abi(object));
    }
}

int main()
{
    winrt::init_apartment();

    winrt::Uri uri(L"http://aka.ms/cppwinrt");
    std::wcout << "C++/WinRT: " << uri.Domain().c_str() << std::endl;

    // Convert to an ABI type.
    winrt::com_ptr<abi::IUriRuntimeClass> ptr = uri.as<abi::IUriRuntimeClass>();
    winrt::hstring domain;
    winrt::check_hresult(ptr->get_Domain(sample::put_abi(domain)));
    std::wcout << "ABI: " << domain.c_str() << std::endl;

    // Convert from an ABI type.
    winrt::Uri uri_from_abi = sample::convert_from_abi<winrt::Uri>(ptr.get());

    WINRT_ASSERT(uri.Domain() == uri_from_abi.Domain());
    WINRT_ASSERT(uri == uri_from_abi);
}
```

## <a name="interoperating-with-abi-com-interface-pointers"></a>ABI の COM インターフェイス ポインターとの相互運用

次のヘルパー関数テンプレートでは、特定の型の ABI COM インターフェイス ポインターを、それと同等の C++/WinRT 投影スマート ポインター型にコピーする方法を示します。

```cppwinrt
template<typename To, typename From>
To to_winrt(From* ptr)
{
    To result{ nullptr };
    winrt::check_hresult(ptr->QueryInterface(winrt::guid_of<To>(), winrt::put_abi(result)));
    return result;
}
...
ID2D1Factory1* com_ptr{ ... };
auto cppwinrt_ptr {to_winrt<winrt::com_ptr<ID2D1Factory1>>(com_ptr)};
```

この次のヘルパー関数テンプレートも同等のものですが、[Windows 実装ライブラリ (WIL)](https://github.com/Microsoft/wil) のスマート ポインター型からコピーする点が異なります。

```cppwinrt
template<typename To, typename From, typename ErrorPolicy>
To to_winrt(wil::com_ptr_t<From, ErrorPolicy> const& ptr)
{
    To result{ nullptr };
    if constexpr (std::is_same_v<typename ErrorPolicy::result, void>)
    {
        ptr.query_to(winrt::guid_of<To>(), winrt::put_abi(result));
    }
    else
    {
        winrt::check_result(ptr.query_to(winrt::guid_of<To>(), winrt::put_abi(result)));
    }
    return result;
}
```

「[C++/WinRT での COM コンポーネントの使用](./consume-com.md)」も参照してください。

### <a name="unsafe-interop-with-abi-com-interface-pointers"></a>ABI の COM インターフェイス ポインターとの安全でない相互運用

この後の表では、(他の操作に加えて) 特定の型の ABI COM インターフェイス ポインターと、それに相当する C++/WinRT 投影スマート ポインター型の間での、安全でない変換を示します。 表のコードでは、次の宣言が想定されています。

```cppwinrt
winrt::Sample s;
ISample* p;

void GetSample(_Out_ ISample** pp);
```

さらに、**ISample** は **Sample** の既定のインターフェイスとします。

コンパイル時に次のコードでそれをアサートできます。

```cppwinrt
static_assert(std::is_same_v<winrt::default_interface<winrt::Sample>, winrt::ISample>);
```

| 操作 | 方法 | メモ |
|-|-|-|
| **winrt::Sample** から **ISample\*** を抽出する | `p = reinterpret_cast<ISample*>(get_abi(s));` | *s* はオブジェクトをまだ所有しています。 |
| **winrt::Sample** から **ISample\*** をデタッチする | `p = reinterpret_cast<ISample*>(detach_abi(s));` | *s* はオブジェクトを所有しなくなります。 |
| **ISample\*** を新しい **winrt::Sample** に転送する | `winrt::Sample s{ p, winrt::take_ownership_from_abi };` | *s* はオブジェクトの所有権を取得します。 |
| **ISample\*** を **winrt::Sample** に設定する | `*put_abi(s) = p;` | *s* はオブジェクトの所有権を取得します。 *s* によって前に所有されていたすべてのオブジェクトはリークされます (デバッグではアサートします)。 |
| **ISample\*** を **winrt::Sample** に受け取る | `GetSample(reinterpret_cast<ISample**>(put_abi(s)));` | *s* はオブジェクトの所有権を取得します。 *s* によって前に所有されていたすべてのオブジェクトはリークされます (デバッグではアサートします)。 |
| **winrt::Sample** 内の **ISample\*** を置き換える | `attach_abi(s, p);` | *s* はオブジェクトの所有権を取得します。 *s* によって前に所有されていたオブジェクトは解放されます。 |
| **ISample\*** を **winrt::Sample** にコピーする | `copy_from_abi(s, p);` | *s* ではオブジェクトへの新しい参照が作成されます。 *s* によって前に所有されていたオブジェクトは解放されます。 |
| **winrt::Sample** を **ISample\*** にコピーする | `copy_to_abi(s, reinterpret_cast<void*&>(p));` | *p* はオブジェクトのコピーを受け取ります。 *s* によって前に所有されていたすべてのオブジェクトはリークされます。 |

## <a name="interoperating-with-the-abis-guid-struct"></a>ABI の GUID 構造体との相互運用

[**GUID**](/previous-versions/aa373931(v%3Dvs.80)) は **winrt::guid** として投影されます。 実装する API の場合、GUID パラメーターに対して **winrt::guid** を使用する必要があります。 それ以外の場合は、C++/WinRT のヘッダーをインクルードする前に、`unknwn.h` をインクルードしている限り (<windows.h> および他の多くのヘッダー ファイルによって暗黙的にインクルードされます)、**winrt::guid** と **GUID** の間はは自動的に変換されます。

それを行わない場合は、それらの間でハード `reinterpret_cast` を行うことができます。 以下の表では、次の宣言が想定されています。

```cppwinrt
winrt::guid winrtguid;
GUID abiguid;
```

| 変換 | `#include <unknwn.h>` あり | `#include <unknwn.h>` なし |
|-|-|-|
| **winrt::guid** から **GUID** に | `abiguid = winrtguid;` | `abiguid = reinterpret_cast<GUID&>(winrtguid);` |
| **GUID** から **winrt::guid** に | `winrtguid = abiguid;` | `winrtguid = reinterpret_cast<winrt::guid&>(abiguid);` |

**winrt::guid** は、次のように構築できます。

```cppwinrt
winrt::guid myGuid{ 0xC380465D, 0x2271, 0x428C, { 0x9B, 0x83, 0xEC, 0xEA, 0x3B, 0x4A, 0x85, 0xC1} };
```

文字列から **winrt::guid** を構築する方法を示す gist については、[make_guid.cpp](https://gist.github.com/kennykerr/6c948882de395c25b3218ad8d4daf362) のページを参照してください。

## <a name="interoperating-with-the-abis-hstring"></a>ABI の HSTRING との相互運用

次の表では、**winrt::hstring** と [**HSTRING**](/windows/win32/winrt/hstring) の間の変換、およびその他の操作を示します。 表のコードでは、次の宣言が想定されています。

```cppwinrt
winrt::hstring s;
HSTRING h;

void GetString(_Out_ HSTRING* value);
```

| 操作 | 方法 | メモ |
|-|-|-|
| **hstring** から **HSTRING** を抽出する | `h = static_cast<HSTRING>(get_abi(s));` | *s* は文字列をまだ所有しています。 |
| **hstring** から **HSTRING** をデタッチする | `h = reinterpret_cast<HSTRING>(detach_abi(s));` | *s* は文字列を所有しなくなります。 |
| **HSTRING** を **hstring** に設定する | `*put_abi(s) = h;` | *s* は文字列の所有権を取得します。 *s* によって前に所有されていたすべての文字列はリークされます (デバッグではアサートします)。 |
| **HSTRING** を **hstring** に受け取る | `GetString(reinterpret_cast<HSTRING*>(put_abi(s)));` | *s* は文字列の所有権を取得します。 *s* によって前に所有されていたすべての文字列はリークされます (デバッグではアサートします)。 |
| **hstring** 内の **HSTRING** を置き換える | `attach_abi(s, h);` | *s* は文字列の所有権を取得します。 *s* によって前に所有されていた文字列は解放されます。 |
| **HSTRING** を **hstring** にコピーする | `copy_from_abi(s, h);` | *s* では文字列のプライベート コピーが作成されます。 *s* によって前に所有されていた文字列は解放されます。 |
| **hstring** を **HSTRING** にコピーする | `copy_to_abi(s, reinterpret_cast<void*&>(h));` | *h* は文字列のコピーを受け取ります。 *h* によって前に所有されていたすべての文字列はリークされます。 |

## <a name="important-apis"></a>重要な API
* [AddRef 関数](/windows/desktop/api/unknwn/nf-unknwn-iunknown-addref)
* [QueryInterface 関数](/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_))
* [winrt::attach_abi 関数](/uwp/cpp-ref-for-winrt/attach-abi)
* [winrt::com_ptr 構造体テンプレート](/uwp/cpp-ref-for-winrt/com-ptr)
* [winrt::copy_from_abi 関数](/uwp/cpp-ref-for-winrt/copy-from-abi)
* [winrt::copy_to_abi 関数](/uwp/cpp-ref-for-winrt/copy-to-abi)
* [winrt::detach_abi 関数](/uwp/cpp-ref-for-winrt/detach-abi)
* [winrt::get_abi 関数](/uwp/cpp-ref-for-winrt/get-abi)
* [winrt::Windows::Foundation::IUnknown::as メンバー関数](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function)
* [winrt::Windows::Foundation::IUnknown::try_as メンバー関数](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntry_as-function)