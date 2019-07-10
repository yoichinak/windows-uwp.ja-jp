---
description: このトピックでは、C++ に存在する値のさまざまなカテゴリについて説明します。 lvalues と rvalues については聞いたことがあると思いますが、それ以外の種類もあります。
title: 値のカテゴリと、その参照
ms.date: 08/11/2018
ms.topic: article
keywords: windows 10, uwp, 標準, c++, cpp, winrt, プロジェクション, 移動, 転送, 値のカテゴリ, 移動セマンティクス, 完全転送, lvalue, rvalue, glvalue, prvalue, xvalue
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 1860f562233ceefa6d9ebb3741378b3265b4c3a9
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/13/2019
ms.locfileid: "63797081"
---
# <a name="value-categories-and-references-to-them"></a>値のカテゴリと、その参照
このトピックでは、C++ に存在する値のさまざまなカテゴリ (および値の参照) について説明します。 *lvalue* と *value* については聞いたことがあると思いますが、このトピックで説明するような観点でそれらを考えたことはないかもしれません。 また、値には他の種類もあります。

C++ のすべての式では、このトピックで説明するカテゴリのいずれかに属している値が生成されます。 C++ 言語、その機能、およびルールの中には、値のカテゴリおよびその参照について適切に理解していることが必要な部分があります。 たとえば、値のアドレスの取得、値のコピー、値の移動、別の関数への値の転送などです。 このトピックでは、これらの側面のすべてについて詳しく説明はしませんが、深く理解するための基本情報を提供します。

このトピックの情報は、Stroustrup の ID と移動可能性の 2 つの独立したプロパティによる値のカテゴリの分析の観点が基になっています [Stroustrup、2013 年]。

## <a name="an-lvalue-has-identity"></a>lvalue は ID を持っている
値が *ID* を持っているとはどういうことでしょうか。 値のメモリ アドレスがあり (または取得することができ)、それを安全に使用する場合、その値には ID があります。 値の内容の比較以上のことができます。ID によって比較または区別できます。

*lvalue* には ID があります。 "lvalue" の "l" は "left" の頭文字ですが (代入の左辺)、これは今では歴史的な意味しかありません。 C++ では、lvalue は代入の左辺 "*または*" 右辺のどちらでも使用できます。 "lvalues" の "l" は、実際にはそれが何であるかを理解または定義する役には立ちません。 lvalue と呼ばれるものは ID を持つ値である、と理解しておくことだけが必要です。

lvalue である式の例としては、名前付きの変数や定数、または参照を返す関数などがあります。 lvalue では "*ない*" 式の例は、一時的な式や、値を返す関数などです。

```cppwinrt
int& get_by_ref() { ... }
int get_by_val() { ... }

int main()
{
    std::vector<byte> vec{ 99, 98, 97 };
    std::vector<byte>* addr1{ &vec }; // ok: vec is an lvalue.
    int* addr2{ &get_by_ref() }; // ok: get_by_ref() is an lvalue.

    int* addr3{ &(get_by_ref() + 1) }; // Error: get_by_ref() + 1 is not an lvalue.
    int* addr4{ &get_by_val() }; // Error: get_by_val() is not an lvalue.
}
```

現在は、lvalue は ID を持っているというのが正しい表現ですが、xvalue もやはり ID を持っています。 *xvalue* については後でさらに詳しく説明します。 ここでは、glvalue ("generalized lvalue": 一般化された lvalue) と呼ばれる値のカテゴリがあることだけを憶えておいてください。 glvalue のスーパーセットには、lvalue ("*従来の lvalue*" とも呼ばれます) と xvalue の両方が含まれます。 そのため、たしかに "lvalue は ID を持っています" が、ID を持つものの完全なセットは、次の図に示すように glvalue のセットです。

![lvalue は ID を持っている](images/has-identity1.png)

## <a name="an-rvalue-is-movable-an-lvalue-is-not"></a>rvalue は移動可能であり、lvalue は移動可能ではない
ただし、glvalue ではない値があります。 したがって、メモリ アドレスを取得することが "*できない*" (または、それが有効であることに依存できない) 値があります。 上のコード例では、そのような値がいくつか示されています。 これは欠点のように聞こえます。 しかし、実際には、そのような値の利点として、値をコピーする (一般に高コスト) のではなく、値を "*移動する*" (一般的に低コスト) ことができます。 値を移行するということは、それまであった場所に存在しなくなることを意味します。 そのため、前に使われていた場所にアクセスすることは、避ける必要があります。 いつ、"*どのようにして*" 値を移動するのかについての説明は、このトピックの範囲外です。 このトピックでは、移動可能な値が *rvalue* (または "*従来の rvalue*") と呼ばれることだけを知っておく必要があります。

"rvalue" の "r" は、"right" の頭文字です (代入の右辺)。 ただし、代入の外側でも、rvalue を使ったり、rvalue を参照したりできます。 ですから、"rvalue" の "r" は重要な点ではありません。 rvalue と呼ばれるものは移動可能な値である、とだけ理解しておけば十分です。

逆に、次の図のように、lvalue は移動可能ではありません。 移動された lvalue は *lvalue* の定義に従わなくなり、lvalue に引き続きアクセスできることがごく当然に期待されるコードに対して予期しない問題になります。

![rvalue は移動可能であり、lvalue は移動可能ではない](images/is-movable.png)

lvalue を移動することはできません。 ただし、自分が何を行っているのかを理解した上でなら (移動後にアクセスしないように注意することなど) 移動できる glvalue の種類が "*あります*"。それが xvalue です。 このアイデアについては、後で値のカテゴリについて詳しく見るときに改めて説明します。

## <a name="rvalue-references-and-reference-binding-rules"></a>rvalue 参照と参照バインド ルール
このセクションでは、rvalue を参照するときの構文について説明します。 移動および転送の処理の多くについては別のトピックで説明されていますが、それらは rvalue 参照によって解決される問題です。 ただし、rvalue 参照について説明する前にまず、`T&` についてはっきりさせておく必要があります。つまり、これまで単に "参照" と呼んでいた事柄です。 それは本当は "lvalue (非定数) の参照" であり、参照のユーザーが書き込むことのできる値を参照しています。

```cppwinrt
template<typename T> T& get_by_lvalue_ref() { ... } // Get by lvalue (non-const) reference.
template<typename T> void set_by_lvalue_ref(T&) { ... } // Set by lvalue (non-const) reference.
```

lvalue の参照は lvalue にはバインドできますが、rvalue にはバインドできません。

それから、lvalue の const 参照が (`T const&`) があり、これは参照のユーザーが書き込むことの "*できない*" オブジェクトを参照します (たとえば、定数)。

```cppwinrt
template<typename T> T const& get_by_lvalue_cref() { ... } // Get by lvalue const reference.
template<typename T> void set_by_lvalue_cref(T const&) { ... } // Set by lvalue const reference.
```

lvalue の const 参照は、lvalue または rvalue にバインドできます。

`T` 型の rvalue への参照の構文は、`T&&` と記述されます。 rvalue 参照では、移動可能な値、つまり使用後に内容を保持する必要がない値が参照されます (たとえば、一時的な値)。 核心は rvalue 参照にバインドされている値を移動する (それによって変更する) ことなので、`const` および `volatile` 修飾子 (cv 修飾子とも呼ばれます) は、rvalue の参照には適用されません。

```cppwinrt
template<typename T> T&& get_by_rvalue_ref() { ... } // Get by rvalue reference.
struct A { A(A&& other) { ... } }; // A move constructor takes an rvalue reference.
```

rvalue 参照は、rvalue にバインドされます。 実際、オーバーロードの解決では、rvalue は lvalue の const 参照より rvalue 参照にバインドする方を "*優先されます*"。 ただし、前に説明したように、rvalue 参照では内容を保持する必要がないものと見なされる値 (たとえば、移動コンストラクターのパラメーター) を参照するので、rvalue 参照を lvalue にバインドすることはできません。

また、コピー構造 (または、rvalue が xvalue の場合は移動構造) を介して、値渡しの引数が期待される rvalue を渡すこともできます。

## <a name="a-glvalue-has-identity-a-prvalue-does-not"></a>glvalue には ID があるが、prvalue にはない
ここまでで、ID を持っているものがわかりました。 また、移動できるものとできないものがわかりました。 しかし、ID を持って "*いない*" 値のセットには、まだ名前を付けていません。 そのようなセットは、*prvalue* (*pure rvalue*: 純粋な rvalue) と呼ばれます。

```cppwinrt
int& get_by_ref() { ... }
int get_by_val() { ... }

int main()
{
    int* addr3{ &(get_by_ref() + 1) }; // Error: get_by_ref() + 1 is a prvalue.
    int* addr4{ &get_by_val() }; // Error: get_by_val() is a prvalue.
}
```

![lvalue には ID があるが、prvalue にはない](images/has-identity2.png)

## <a name="the-complete-picture-of-value-categories"></a>値のカテゴリの全体像
残っているのは、これまでに示した情報と図を結合して、1 つの大きな図にすることだけです。

![値のカテゴリの全体像](images/value-categories.png)

### <a name="glvalue-i"></a>glvalue (i)
glvalue (一般化された lvalue) は、ID を持っています。

### <a name="lvalue-im"></a>lvalue (i\&\!m)
lvalue (glvalue の一種) は ID を持っていますが、移動することはできません。 これらは、通常は読み取り/書き込みの値であり、参照、const 参照、またはコピーが低コストの場合は値によって、渡されます。 lvalue を rvalue 参照にバインドすることはできません。

### <a name="xvalue-im"></a>xvalue (i\&m)
xvalue (glvalue の一種だが、rvalue の一種でもある) は ID を持っており、移動することもできます。 これは、コピーが高コストであるために移動することにしたかつての lvalue である場合があり、後でアクセスしないように注意する必要があります。 lvalue を xvalue に変換する方法を次に示します。

```cppwinrt
struct A { ... };
A a; // a is an lvalue...
static_cast<A&&>(a); // ...but this expression is an xvalue.
```

上記のコード例では、まだ何も移動していません。 lvalue を名前のない rvalue 参照にキャストして xvalue を作成しただけです。 まだ lvalue の名前で識別できますが、xvalue として移動 "*できる*" ようになっています。 このようなことを行う理由、および実際の移動については、別のトピックをご覧ください。 ただし、それが役に立つなら、"xvalue" の "x" は "expert-only" (エキスパート専用) と考えることができます。 lvalue を xvalue (rvalue の一種) にキャストすることによって、値は rvalue 参照にバインドできるようになります。

次に示すのは xvalue の他の 2 つの例で、名前のない rvalue 参照を返す関数の呼び出しと、xvalue のメンバーへのアクセスです。

```cppwinrt
struct A { int m; };
A&& f();
f(); // This expression is an xvalue...
f().m; // ...and so is this.
```

### <a name="prvalue-im"></a>prvalue (\!i\&m)
prvalue (純粋な rvalue、rvalue の一種) は ID を持ちませんが、移動することはできます。 通常、これらは一時的な値、値を返す関数の呼び出しの結果、または glvalue ではない他の式の評価の結果です

### <a name="rvalue-m"></a>rvalue (m)
rvalue は移動可能です。 rvalue "*参照*" では、常に rvalue が参照されます (内容を保持する必要がないものと思われる値)。

ところで、rvalue 参照自体は rvalue なのでしょうか。 "*名前のない*" rvalue 参照 (上の xvalue コードの例にで示されているものなど) は xvalue なので、rvalue です。 移動コンストラクターなど、rvalue 参照関数パラメーターへのバインドが優先されます。 逆に (そして、おそらくは直感に反しますが)、rvalue 参照が名前を持っている場合、その名前で構成される式は lvalue です。 したがって、rvalue 参照パラメーターにバインドすることは "*できません*"。 ただし、名前のない rvalue 参照 (xvalue) に再びキャストするだけで、簡単にそのようにすることができます。

```cppwinrt
void foo(A&) { ... }
void foo(A&&) { ... }
void bar(A&& a) // a is a named rvalue reference; it's an lvalue.
{
    foo(a); // Calls foo(A&).
    foo(static_cast<A&&>(a)); // Calls foo(A&&).
}
A&& get_by_rvalue_ref() { ... } // This unnamed rvalue reference is an xvalue.
```

### <a name="im"></a>\!i\&\!m
ID を持たず、移動できない値の種類は、まだ説明していない組み合わせです。 しかし、そのカテゴリは C++ 言語で役に立つアイデアではないので、無視してかまいません。

## <a name="reference-collapsing-rules"></a>参照縮小ルール
式に含まれる複数の似た参照 (lvalue 参照に対する lvalue 参照、または rvalue 参照に対する rvalue 参照) は、相互に相殺されます。

- `A& &` は `A&` に縮小されます。
- `A&& &&` は `A&&` に縮小されます。

式に含まれる複数の似ていない参照は、lvalue 参照に縮小されます。

- `A& &&` は `A&` に縮小されます。
- `A&& &` は `A&` に縮小されます。

## <a name="forwarding-references"></a>転送参照
この最後のセクションでは、既に説明した rvalue 参照と、別の概念である "*転送参照*" を比べます。

```cppwinrt
void foo(A&& a) { ... }
```

- 前に見たように、`A&&` は rvalue 参照です。 const と volatile は、rvalue 参照には適用されません。
- `foo` は、**A** 型の rvalue のみを受け付けます。
- rvalue 参照 (`A&&` など) が存在する理由は、一時的な値 (またはその他の rvalue) が渡される場合に最適化されたオーバーロードを作成できるようにするためです。

```cppwinrt
template <typename _Ty> void bar(_Ty&& ty) { ... }
```

- `_Ty&&` は "*転送参照*" です。 `bar` に渡すものに応じて、 **_Ty** 型を、volatile/非 volatile とは無関係に、const/非 const にできます。
- `bar` では、 **_Ty** 型の任意の lvalue または rvalue が受け付けられます。
- lvalue を渡すと、転送参照は `_Ty& &&` になり、lvalue 参照 `_Ty&` に縮小されます。
- rvalue を渡すと、転送参照は `_Ty&& &&` になり、rvalue 参照 `_Ty&&` に縮小されます。
- 転送参照 (`_Ty&&` など) が存在する理由は、最適化のためでは "*なく*"、渡されたものを取得して、透過的かつ効率的に転送するためです。 おそらく、転送参照が出てくるのは、ライブラリ コードを書く (または、詳しく学習する) 場合だけです。たとえば、コンストラクターの引数に転送するファクトリ関数などです。

## <a name="sources"></a>Sources
* \[Stroustrup、2013 年\] B. Stroustrup: C++ プログラミング言語、第 4 版。 Addison-Wesley. 2013 年。
