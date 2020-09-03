---
description: このトピックでは、ヘルパーの [**winrt::make**](/uwp/cpp-ref-for-winrt/make) ファミリを使用するのではなく、スタック上に実装型のオブジェクトを作成するという間違いを診断するのに役立つ C++/WinRT 2.0 機能について詳しく説明します。
title: 直接割当ての診断
ms.date: 07/19/2019
ms.topic: article
keywords: windows 10、uwp、標準、c++、cpp、winrt、プロジェクション、直接、スタック、割り当て、投影、実装
ms.localizationpriority: medium
ms.openlocfilehash: 199b62d96685e207e55e6dff7cd310752617d1d2
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89170256"
---
# <a name="diagnosing-direct-allocations"></a>直接割当ての診断

「[C++/WinRT での API の作成](./author-apis.md)」で説明されているように、実装型のオブジェクトを作成する場合は、ヘルパーの [**winrt::make**](/uwp/cpp-ref-for-winrt/make) ファミリを使用して作成する必要があります。 このトピックでは、スタック上に実装型のオブジェクトを直接割り当てるという間違いを診断するのに役立つ C++/WinRT 2.0 機能について詳しく説明します。

このような間違いによって、デバッグするのが難しく時間がかかる不可解なクラッシュや破損が発生する可能性があります。 これは重要な機能なので、背景について理解しておくことをお勧めします。

## <a name="setting-the-scene-with-mystringable"></a>**MyStringable** を使用したシーンの設定

まず、[**IStringable**](/uwp/api/windows.foundation.istringable) の単純な実装について考えてみましょう。

```cppwinrt
struct MyStringable : implements<MyStringable, IStringable>
{
    winrt::hstring ToString() const { return L"MyStringable"; }
};
```

ここでは、引数として **IStringable** を想定する関数を (実装内から) 呼び出す必要があると仮定します。

```cppwinrt
void Print(IStringable const& stringable)
{
    printf("%ls\n", stringable.ToString().c_str());
}
```

問題は、**MyStringable** 型が **IStringable** では "*ない*" ことです。

- **MyStringable** 型は、**IStringable** インターフェイスを実装したものです。
- **IStringable** 型は投影型です。

> [!IMPORTANT]
> "*実装型*" と "*投影型*" の違いを理解することが重要です。 基本的な概念と用語については、「[C++/WinRT での API の使用](consume-apis.md)」と「[C++/WinRT での API の作成](author-apis.md)」を参照してください。

実装とプロジェクションの間の距離が短くわかりにくい可能性があります。 実際には、実装がもう少しプロジェクションに近いものに感じられるように、実装によって実装対象の各投影型への暗黙の変換が提供されます。 これを簡単に行えるという意味ではありません。

```cppwinrt
struct MyStringable : implements<MyStringable, IStringable>
{
    winrt::hstring ToString() const;
 
    void Call()
    {
        Print(this);
    }
};
```

代わりに、呼び出しを解決するための候補として変換演算子を使用できるように参照を取得する必要があります。

```cppwinrt
void Call()
{
    Print(*this);
}
```

これで動作します。 暗黙の変換では、実装型から投影型への (とても効率的な) 変換が提供されます。これは、多くのシナリオでとても便利です。 その機能がなければ、多数の実装型によって作成がとても煩雑であることが証明されます。 [**winrt::make**](/uwp/cpp-ref-for-winrt/make) 関数テンプレート (または [**winrt::make_self**](/uwp/cpp-ref-for-winrt/make-self)) のみを使用して実装を割り当てる場合は、すべてうまくいきます。

```cppwinrt
IStringable stringable{ winrt::make<MyStringable>() };
```

## <a name="potential-pitfalls-with-cwinrt-10"></a>C++/WinRT 1.0 で発生する可能性のある落とし穴

それでも、暗黙的な変換によって問題が発生する可能性があります。 この役に立たないヘルパー関数について考えてみましょう。

```cppwinrt
IStringable MakeStringable()
{
    return MyStringable(); // Incorrect.
}
```

または、このように一見無害なステートメントもあります。

```cppwinrt
IStringable stringable{ MyStringable() }; // Also incorrect.
```

残念ながら、その暗黙の変換が原因で、そのようなコードは C++/WinRT 1.0 によってコンパイル "*されませんでした*"。 (とても深刻な) 問題は、バッキング メモリが一時的なスタック上にある参照カウント オブジェクトを指す投影型を返す可能性があることです。

次に、C++/WinRT 1.0 を使用してコンパイルされた他のものを示します。

```cppwinrt
MyStringable* stringable{ new MyStringable() }; // Very inadvisable.
```

生ポインターは、危険で手間がかかるバグの原因です。 不要な場合は使用しないでください。 C++/WinRT は、生ポインターの使用を強制することなくすべてのものの効率を高めようとします。 次に、C++/WinRT 1.0 を使用してコンパイルされた他のものを示します。

```cppwinrt
auto stringable{ std::make_shared<MyStringable>(); } // Also very inadvisable.
```

これは複数のレベルでの誤りです。 同じオブジェクトに対して 2 つの異なる参照カウントがあります。 Windows ランタイム (およびそれより前の従来の COM) は、**std::shared_ptr** と互換性のない組み込みの参照カウントに基づいています。 もちろん、**std::shared_ptr** には多くの有効なアプリケーションがありますが、Windows ランタイム (および従来の COM) オブジェクトを共有している場合はまったく不要です。 最後に、これは C++/WinRT 1.0 でもコンパイルされます。

```cppwinrt
auto stringable{ std::make_unique<MyStringable>() }; // Highly dubious.
```

これも問題があります。 一意の所有権は、**MyStringable** の組み込み参照カウントの共有の有効期間とは相容れません。

## <a name="the-solution-with-cwinrt-20"></a>C++/WinRT 2.0 を使用したソリューション

C++/WinRT 2.0 では、実装型を直接割り当てようとするすべての試行でコンパイラ エラーが発生します。 これは最適な種類のエラーであり、不可解なランタイム バグよりもはるかにましです。

実装を行う必要があるときは常に、上記のように単純に [**winrt::make**](/uwp/cpp-ref-for-winrt/make) または [**winrt::make_self**](/uwp/cpp-ref-for-winrt/make-self) を使用できます。 これを行うのを忘れた場合は、**use_make_function_to_create_this_object** という名前の抽象関数への参照を使用してこれを暗黙的に示すコンパイラ エラーが発生することになります。 これは、正確には `static_assert` ではなく、それに近いものです。 それでもなお、これは説明されているすべての間違いを検出する最も信頼性の高い方法です。

これは、実装にいくつかの小さな制約を配置する必要があることを意味します。 直接割り当てを検出するためのオーバーライドがないことに依存している場合、**winrt::make** 関数テンプレートでは、オーバーライドを伴う抽象仮想関数をなんらかの方法で満たす必要があります。 オーバーライドを提供する `final` クラスを使用して実装から派生させることで、これが行われます。 このプロセスに関する注意事項がいくつかあります。

第 1 に、仮想関数はデバッグ ビルド内にのみ存在します。 つまり、検出は、最適化されたビルド内の vtable のサイズに影響しません。

第 2 に、**winrt::make** で使用される派生クラスは `final` であるため、以前に実装クラスを `final` としてマークしないことを選択した場合でも、これはオプティマイザーによって推測される可能性のある脱仮想化が発生することを意味します。 そのため、これは改善点です。 その逆は、実装を `final` に "*できない*" ことです。 この場合も、これは重要ではありません。インスタンス化された型は常に `final` になるからです。

第 3 に、実装内の仮想関数を `final` として自由にマークできます。 当然ながら、C++/WinRT は、従来の COM や WRL などの実装とは大きく異なり、実装に関するすべてのものが仮想になる傾向があります。 C++/WinRT では、仮想ディスパッチはアプリケーション バイナリ インターフェイス (ABI) (常に `final`) に制限されており、実装メソッドはコンパイル時または静的ポリモーフィズムに依存します。 これにより、不要なランタイム ポリモーフィズムが回避されます。また、C++/WinRT 実装内の仮想関数に理由はほとんどないことも意味します。 これはとても優れたもので、はるかに予測可能なインライン化につながります。

第 4 に、**winrt::make** によって派生クラスが挿入されるので、実装にプライベート デストラクターを含めることはできません。 プライベート デストラクターは、これもすべてが仮想であったため従来の COM 実装でよく使用されていました。また、生ポインターを直接処理するのが一般的だったで、[**Release**](/windows/win32/api/unknwn/nf-unknwn-iunknown-release) の代わりに誤って `delete` を呼び出しやすくなっていました。 C++/WinRT では、生ポインターを直接処理することをわざわざ難しくしています。 また、ユーザーは `delete` を呼び出す可能性のある C++/WinRT 内の生ポインターの取得に "*本当に*" 尽力する必要があります。 値のセマンティクスは、値と参照を扱うことを意味します。ポインターはめったに使用しません。

そのため C++/WinRT では、従来の COM コードを記述することの意味について、あらかじめ考えられた概念に取り組んでいます。 WinRT は従来の COM ではないため、これはまったく問題ありません。 従来の COM は、Windows ランタイムのアセンブリ言語です。 毎日記述するコードにはしないでください。 代わりに C++/WinRT では、最新の C++ によく似ていて、従来の COM にはあまり似ていないコードを記述します。

## <a name="important-apis"></a>重要な API
* [winrt::make 関数テンプレート](/uwp/cpp-ref-for-winrt/make)
* [winrt::make_self 関数テンプレート](/uwp/cpp-ref-for-winrt/make-self)

## <a name="related-topics"></a>関連トピック
* [C++/WinRT で API を使用する](consume-apis.md)
* [C++/WinRT で API を作成する](./author-apis.md)