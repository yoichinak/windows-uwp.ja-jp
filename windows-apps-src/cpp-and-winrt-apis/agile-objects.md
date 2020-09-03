---
description: アジャイル オブジェクトは、いずれかのスレッドからアクセスできます。 お使いの C++/WinRT 型は既定ではアジャイルですが、オプトアウトできます。
title: C++/WinRT でのアジャイル オブジェクト
ms.date: 04/24/2019
ms.topic: article
keywords: Windows 10、uwp、標準、c++、cpp、winrt、プロジェクション、アジャイル、オブジェクト、アジリティ、IAgileObject
ms.localizationpriority: medium
ms.openlocfilehash: 71800de1d209a0164ab5a7e90bbc191c0f9bebe9
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89154636"
---
# <a name="agile-objects-in-cwinrt"></a>C++/WinRT でのアジャイル オブジェクト

ほとんどの場合、Windows ランタイム クラスのインスタンスには、任意のスレッドからアクセスできます (ほとんどの標準 C++ オブジェクトと同様)。 このような Windows ランタイム クラスが "*アジャイル*" です。 Windows に組み込まれている Windows ランタイム クラスのうち少数はアジャイル以外ですが、それらを使用するときに、スレッド モデルおよびマーシャリング動作を考慮する必要があります (マーシャリングでは、アパートメント境界を越えてデータが渡されます)。 すべての Windows ランタイム オブジェクトにおいて、アジャイルであることは既定値として適切なため、ユーザー独自の [C++/WinRT](./intro-to-using-cpp-with-winrt.md) 型は既定でアジャイルになります。

ただし、オプトアウトできます。たとえば、特定のシングルスレッド アパートメントなど、特別な理由で特定の型のオブジェクトを存在させることが必要な場合があります。 これは通常、再入の要件で行う必要があります。 それでもますます、ユーザー インターフェイス (UI) API ではアジャイル オブジェクトを提供するようになっています。 一般に、アジリティは最も単純で最もパフォーマンスの高いオプションです。 また、アクティベーション ファクトリを実装する際は、対応するランタイム クラスがアジャイルではない場合でもアジャイルにする必要があります。

> [!NOTE]
> Windows ランタイムは COM に基づいています。 COM の用語では、アジャイル クラスは `ThreadingModel` = *両方*に登録されています。 COM スレッド モデルとアパートメントの詳細については、「[COM スレッド モデルの理解と使用](/previous-versions/ms809971(v=msdn.10))」をご覧ください。

## <a name="code-examples"></a>コード例

ランタイム クラスの実装例を使用して、C++/WinRT でアジリティがどのようにサポートされるか実証してみましょう。

```cppwinrt
#include <winrt/Windows.Foundation.h>

using namespace winrt;
using namespace Windows::Foundation;

struct MyType : winrt::implements<MyType, IStringable>
{
    winrt::hstring ToString(){ ... }
};
```

オプトアウトしていないため、この実装はアジャイルです。 [  **Winrt::implements**](/uwp/cpp-ref-for-winrt/implements) 基本構造体は [**IAgileObject**](/windows/desktop/api/objidl/nn-objidl-iagileobject) と [**IMarshal**](/windows/desktop/api/objidl/nn-objidl-imarshal) を実装します。 **IMarshal** 実装は、**IAgileObject** について知らないレガシー コードで適切な処理を行うために **CoCreateFreeThreadedMarshaler** を使用します。

このコードでは、オブジェクトのアジリティを確認します。 `myimpl` がアジャイルではない場合に [**IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function) の呼び出しで例外がスローされます。

```cppwinrt
winrt::com_ptr<MyType> myimpl{ winrt::make_self<MyType>() };
winrt::com_ptr<IAgileObject> iagileobject{ myimpl.as<IAgileObject>() };
```

例外を処理する代わりに、[**IUnknown::try_as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntry_as-function) を呼び出すことができます。

```cppwinrt
winrt::com_ptr<IAgileObject> iagileobject{ myimpl.try_as<IAgileObject>() };
if (iagileobject) { /* myimpl is agile. */ }
```

**IAgileObject** には独自のメソッドがないため、これを使用してできることは多くありません。 この次のバリアントはより一般的です。

```cppwinrt
if (myimpl.try_as<IAgileObject>()) { /* myimpl is agile. */ }
```

**IAgileObject** は、*マーカー インターフェイス*です。 **IAgileObject** へのクエリの単なる成功または失敗が、それから得られる情報とユーティリティの範囲です。

## <a name="opting-out-of-agile-object-support"></a>アジャイル オブジェクトのサポートのオプトアウト

[  **winrt::non_agile**](/uwp/cpp-ref-for-winrt/non-agile) マーカー構造体をテンプレート引数として基底クラスに渡すことによって、アジャイル オブジェクトのサポートを明示的にオプトアウトすることを選択することができます。

**winrt::implements** から直接派生する場合。

```cppwinrt
struct MyImplementation: implements<MyImplementation, IStringable, winrt::non_agile>
{
    ...
}
```

ランタイム クラスを作成している場合。

```cppwinrt
struct MyRuntimeClass: MyRuntimeClassT<MyRuntimeClass, winrt::non_agile>
{
    ...
}
```

可変個引数パラメーター パックのどこにマーカー構造体が現れるかは関係ありません。

アジリティをオプトアウトするかどうかに関係なく、**IMarshal** を自分で実装できます。 たとえば、**winrt::non_agile** マーカーを使用して既定のアジリティの実装を回避し、自分で **IMarshal** を実装できます&mdash;marshal-by-value (値渡しのマーシャリング) セマンティクスをサポートするためなど。

## <a name="agile-references-winrtagile_ref"></a>アジャイル リファレンス (winrt::agile_ref)

アジャイルではないオブジェクトを使用していて、ただしいくつかの可能性のあるアジャイルのコンテキストでそれを渡す必要がある場合、1 つのオプションは、[**winrt::agile_ref**](/uwp/cpp-ref-for-winrt/agile-ref) 構造体のテンプレートを使用して、非アジャイル型のインスタンス、または非アジャイル オブジェクトのインターフェイスへのアジャイルのリファレンスを取得することです。

```cppwinrt
NonAgileType nonagile_obj;
winrt::agile_ref<NonAgileType> agile{ nonagile_obj };
```

または、[**winrt::make_agile**](/uwp/cpp-ref-for-winrt/make-agile) ヘルパー関数を使用できます。

```cppwinrt
NonAgileType nonagile_obj;
auto agile{ winrt::make_agile(nonagile_obj) };
```

どちらの場合でも、`agile` を異なるアパートメント内のスレッドに自由に渡して、そこで使用できるようになりました。

```cppwinrt
co_await resume_background();
NonAgileType nonagile_obj_again{ agile.get() };
winrt::hstring message{ nonagile_obj_again.Message() };
```

[  **Agile_ref::get**](/uwp/cpp-ref-for-winrt/agile-ref#agile_refget-function) の呼び出しでは、**get** が呼びだされたスレッド コンテキスト内で安全に使用できるプロキシを返します。

## <a name="important-apis"></a>重要な API

* [IAgileObject インターフェイス](/windows/desktop/api/objidl/nn-objidl-iagileobject)
* [IMarshal インターフェイス](/windows/desktop/api/objidl/nn-objidl-imarshal)
* [winrt::agile_ref 構造体テンプレート](/uwp/cpp-ref-for-winrt/agile-ref)
* [winrt::implements 構造体テンプレート](/uwp/cpp-ref-for-winrt/implements)
* [winrt::make_agile 関数テンプレート](/uwp/cpp-ref-for-winrt/make-agile)
* [winrt::non_agile マーカー構造体](/uwp/cpp-ref-for-winrt/non-agile)
* [winrt::Windows::Foundation::IUnknown::as 関数](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function)
* [winrt::Windows::Foundation::IUnknown::try_as 関数](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntry_as-function)

## <a name="related-topics"></a>関連トピック

* [COM スレッド モデルの理解と使用](/previous-versions/ms809971(v=msdn.10))