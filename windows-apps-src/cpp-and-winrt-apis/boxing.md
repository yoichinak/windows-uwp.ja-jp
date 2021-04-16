---
description: スカラーまたは配列値は、**IInspectable** を想定している関数に渡す前に、参照クラス オブジェクト内にラップする必要があります。 このラッピング プロセスは、値の *ボックス化* と呼ばれます。
title: C++/WinRT を使用した IInspectable への値のボックス化とボックス化解除
ms.date: 04/23/2019
ms.topic: article
keywords: Windows 10、uwp、標準、c++、cpp、winrt、プロジェクション、XAML、コントロール、ボックス化、スカラー、値
ms.localizationpriority: medium
ms.openlocfilehash: 08c36c735b319bb1658b2d4ce745ae0fdb7bdeee
ms.sourcegitcommit: b89d3bc42713fbe4c0ada99d6f514f1304821221
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/14/2021
ms.locfileid: "107466422"
---
# <a name="boxing-and-unboxing-values-to-iinspectable-with-cwinrt"></a>C++/WinRT を使用した IInspectable への値のボックス化とボックス化解除

> [!NOTE]
> [**winrt::box_value**](/uwp/cpp-ref-for-winrt/box-value) および [**winrt::unbox_value**](/uwp/cpp-ref-for-winrt/unbox-value) 関数を使用すると、スカラー値だけでなく、ほとんどの種類の配列 (列挙体の配列を除く) をボックス化またはボックス化解除できます。 スカラー値のみをボックス化解除するには、[**winrt::unbox_value_or**](/uwp/cpp-ref-for-winrt/unbox-value-or) 関数を使用します。

[**IInspectable インターフェイス**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable) は、Windows ランタイム (WinRT) のすべてのランタイム クラスのルート インターフェイスです。 これは、すべての COM インターフェイスとクラスのルートである [**IUnknown**](/windows/desktop/api/unknwn/nn-unknwn-iunknown) や、すべての [共通型システム](/dotnet/standard/base-types/common-type-system) クラスのルートである **System.Object** と似た概念です。

つまり、**IInspectable** を想定している関数は、任意のランタイム クラスのインスタンスに渡すことができます。 ただし、そのような関数に、スカラー値 (数値、テキスト値など) や配列を直接渡すことはできません。 代わりに、スカラーまたは配列値を参照クラス オブジェクト内にラップする必要があります。 このラッピング プロセスは、値の *ボックス化* と呼ばれます。

> [!IMPORTANT]
> Windows ランタイム API に渡すことができる型はどれでもボックス化とボックス化解除を行うことができます。 つまり、Windows ランタイム型です。 上記の例には、数値とテキスト値 (文字列)、配列があります。 別の例として、IDL で定義する `struct` があります。 通常の C++ `struct` (IDL で定義されていないもの) をボックス化しようとすると、ボックス化できるのは Windows ランタイム型のみであることがコンパイラから通知されます。 ランタイム クラスは Windows ランタイム型ですが、もちろん、ランタイム クラスはボックス化せずに Windows ランタイム API に渡すことができます。

[C++/WinRT](./intro-to-using-cpp-with-winrt.md) には、スカラーまたは配列値を取得し、ボックス化した値を **IInspectable** へ返す [**winrt::box_value**](/uwp/cpp-ref-for-winrt/box-value) 関数が用意されています。 **IInspectable** をボックス化解除してスカラーまたは配列値に戻すには、[**winrt::unbox_value**](/uwp/cpp-ref-for-winrt/unbox-value) 関数があります。 また、**IInspectable** をボックス化解除してスカラー値に戻すには、[**winrt::unbox_value_or**](/uwp/cpp-ref-for-winrt/unbox-value-or) 関数があります。

## <a name="examples-of-boxing-a-value"></a>値をボックス化する例
[**LaunchActivatedEventArgs::Arguments**](/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs.Arguments) アクセサー関数は、スカラー値である [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) を返します。 その **hstring** 値をボックス化し、次のように **IInspectable** を想定している関数に渡します。

```cppwinrt
void App::OnLaunched(LaunchActivatedEventArgs const& e)
{
    ...
    rootFrame.Navigate(winrt::xaml_typename<BlankApp1::MainPage>(), winrt::box_value(e.Arguments()));
    ...
}
```

XAML [**Button**](/uwp/api/windows.ui.xaml.controls.button) のコンテンツ プロパティを設定するには、[**Button::Content**](/uwp/api/windows.ui.xaml.controls.contentcontrol.content?) ミューテーター関数を呼び出します。 コンテンツのプロパティを文字列値に設定するには、このコードを使用できます。

```cppwinrt
Button().Content(winrt::box_value(L"Clicked"));
```

まず、[**hstring**](/uwp/cpp-ref-for-winrt/hstring) 変換コンストラクターが文字列リテラルを **hstring** に変換します。 次に **hstring** を受け取る **winrt::box_value** のオーバーロードが呼び出されます。

## <a name="examples-of-unboxing-an-iinspectable"></a>IInspectable をボックス化解除する例
**IInspectable** を想定する独自の関数では、[**winrt::unbox_value**](/uwp/cpp-ref-for-winrt/unbox-value) を使用してボックス化解除することができます。また [**winrt::unbox_value_or**](/uwp/cpp-ref-for-winrt/unbox-value-or) を使用して既定値でボックス化解除することができます。

```cppwinrt
void Unbox(winrt::Windows::Foundation::IInspectable const& object)
{
    hstring hstringValue = unbox_value<hstring>(object); // Throws if object is not a boxed string.
    hstringValue = unbox_value_or<hstring>(object, L"Default"); // Returns L"Default" if object is not a boxed string.
    float floatValue = unbox_value_or<float>(object, 0.f); // Returns 0.0 if object is not a boxed float.
}
```

## <a name="determine-the-type-of-a-boxed-value"></a>ボックス化された値の型の判別
ボックス化された値を受け取って、その値に含まれる型が不明な場合は (型はボックス化解除するために知っておく必要があります)、その [**IPropertyValue**](/uwp/api/windows.foundation.ipropertyvalue) でボックス化された値を照会し、そこで **Type** を呼び出すことができます。 次にコード例を示します。

`WINRT_ASSERT` はマクロ定義であり、[_ASSERTE](/cpp/c-runtime-library/reference/assert-asserte-assert-expr-macros) に展開されます。

```cppwinrt
float pi = 3.14f;
auto piInspectable = winrt::box_value(pi);
auto piPropertyValue = piInspectable.as<winrt::Windows::Foundation::IPropertyValue>();
WINRT_ASSERT(piPropertyValue.Type() == winrt::Windows::Foundation::PropertyType::Single);
```

## <a name="important-apis"></a>重要な API
* [IInspectable インターフェイス](/windows/desktop/api/inspectable/nn-inspectable-iinspectable)
* [winrt::box_value 関数テンプレート](/uwp/cpp-ref-for-winrt/box-value)
* [winrt::hstring 構造体](/uwp/cpp-ref-for-winrt/hstring)
* [winrt::unbox_value 関数テンプレート](/uwp/cpp-ref-for-winrt/unbox-value)
* [winrt::unbox_value_or 関数テンプレート](/uwp/cpp-ref-for-winrt/unbox-value-or)
