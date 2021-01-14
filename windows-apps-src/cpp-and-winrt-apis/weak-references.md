---
description: Windows ランタイムは参照カウント システムです。このようなシステムでは、強参照と弱参照の重要性とこれらの違いを認識することが重要です。
title: C++/WinRT の強参照と弱参照
ms.date: 05/16/2019
ms.topic: article
keywords: windows 10, uwp, 標準, c++, cpp, winrt, プロジェクション, 強, 弱, 参照
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 46b62c202d090a7760445b3e07bca073d2636c66
ms.sourcegitcommit: afc4ff2c89f148d32073ab1cc42063ccdc573a8c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/11/2021
ms.locfileid: "98104483"
---
# <a name="strong-and-weak-references-in-cwinrt"></a>C++/WinRT の強参照と弱参照

Windows ランタイムは参照カウント システムです。このようなシステムでは、強参照と弱参照 (および、暗黙的 *this* ポインターのように、いずれでもない参照) の重要性とこれらの違いを認識することが重要です。 このトピックで説明しますが、このような参照を正しく扱う方法を知ることは、円滑に動作する安定したシステムと突然クラッシュするシステムの違いを意味することがあります。 [C++/WinRT](./intro-to-using-cpp-with-winrt.md) により言語プロジェクションを広範囲で支えるヘルパー関数が提供され、複雑なシステムを簡単かつ正しく構築する作業が半分まで片付きます。

> [!NOTE]
> いくつかの例外を除き、[C++/WinRT](./index.md) で使用または作成する Windows ランタイム型では、弱参照のサポートが既定でオンになっています。 **Windows.UI.Composition** と **Windows.Devices.Input.PenDevice** は、例外の例です。つまり、これらの名前空間の型については、弱参照のサポートはオンになって "*いません*"。 「[自動取り消しのデリゲートの登録が失敗する場合](./handle-events.md#if-your-auto-revoke-delegate-fails-to-register)」も参照してください。
> 
> 型を作成している場合は、このトピックの「[C++/WinRT の弱参照](#weak-references-in-cwinrt)」セクションを参照してください。

## <a name="safely-accessing-the-this-pointer-in-a-class-member-coroutine"></a>class-member コルーチンで *this* ポインターに安全にアクセスする

コルーチンの詳細とコード例については、「[C++/WinRT を使用した同時実行操作と非同期操作](./concurrency.md)」を参照してください。

下のコード一覧では、あるクラスのメンバー関数であるコルーチンの典型的な例を確認できます。 新しい **Windows コンソール アプリケーション (C++/WinRT)** プロジェクトで、この例をコピーし、指定のファイルに貼り付けることができます。

```cppwinrt
// pch.h
#pragma once
#include <iostream>
#include <winrt/Windows.Foundation.h>

// main.cpp : Defines the entry point for the console application.
#include "pch.h"

using namespace winrt;
using namespace Windows::Foundation;
using namespace std::chrono_literals;

struct MyClass : winrt::implements<MyClass, IInspectable>
{
    winrt::hstring m_value{ L"Hello, World!" };

    IAsyncOperation<winrt::hstring> RetrieveValueAsync()
    {
        co_await 5s;
        co_return m_value;
    }
};

int main()
{
    winrt::init_apartment();

    auto myclass_instance{ winrt::make_self<MyClass>() };
    auto async{ myclass_instance->RetrieveValueAsync() };

    winrt::hstring result{ async.get() };
    std::wcout << result.c_str() << std::endl;
}
```

**MyClass::RetrieveValueAsync** はしばらくの間動作し、最終的に `MyClass::m_value` データ メンバーのコピーを返します。 **RetrieveValueAsync** を呼び出すと、非同期オブジェクトが作成されます。そのオブジェクトには、暗黙的 *this* ポインターが含まれます (これを経由し、最終的に `m_value` にアクセスします)。

コルーチンでは、最初の一時停止ポイントまで実行は同期であり、そこで呼び出し元に制御が返されることを思い出してください。 **RetrieveValueAsync** では、最初の `co_await` は最初の中断ポイントです。 コルーチンが再開されるまで (この場合は約 5 秒後)、`m_value` にアクセスするために使用している暗黙の *this* ポインターに対して何かが発生している可能性があります。

イベントは次のように進行します。

1. **main** で、**MyClass** のインスタンスが作成されます (`myclass_instance`)。
2. `async` オブジェクトが作成されます。これは (その *this* を介して) `myclass_instance` を指します。
3. **winrt::Windows::Foundation::IAsyncAction::get** 関数では、その最初の中断ポイントに達し、数秒間ブロックしてから、**RetrieveValueAsync** の結果が返されます。
4. **RetrieveValueAsync** から値 `this->m_value` が返されます。

手順 4 は、*this* が有効である間だけ安全です。

しかし、非同期操作の完了前にクラス インスタンスが破棄された場合はどうなるでしょうか。 非同期メソッドの完了前にクラス インスタンスが範囲から外れることは、あらゆる形でありえます。 しかし、クラス インスタンスを `nullptr` に設定することでそれをシミュレートできます。

```cppwinrt
int main()
{
    winrt::init_apartment();

    auto myclass_instance{ winrt::make_self<MyClass>() };
    auto async{ myclass_instance->RetrieveValueAsync() };
    myclass_instance = nullptr; // Simulate the class instance going out of scope.

    winrt::hstring result{ async.get() }; // Behavior is now undefined; crashing is likely.
    std::wcout << result.c_str() << std::endl;
}
```

クラス インスタンスを破棄する箇所を過ぎると、それを再び直接参照することはないようです。 しかし、当然、この非同期オブジェクトにはそれを指す *this* ポインターが与えられており、それを使用し、クラス インスタンス内に格納されている値をコピーしようとします。 このコルーチンはメンバー関数であり、その *this* ポインターを問題なく使用できるはずです。

このようにコードを変更することで、手順 4 の問題に遭遇します。クラス インスタンスが破棄されており、*this* が無効になっているためです。 非同期オブジェクトがクラス インスタンス内の変数にアクセスしようとすると、直後にクラッシュします (あるいは、まったく未定義の何かが起こります)。

この解決策は、&mdash;非同期操作 (コルーチン)&mdash; にクラス インスタンスの強参照を与えることです。 現時点では、コルーチンは実際にクラス インスタンスの生の *this* ポインターを保持していますが、それだけではクラス インスタンスを維持できません。

クラス インスタンスを維持するには、**RetrieveValueAsync** の実装を下のように変更します。

```cppwinrt
IAsyncOperation<winrt::hstring> RetrieveValueAsync()
{
    auto strong_this{ get_strong() }; // Keep *this* alive.
    co_await 5s;
    co_return m_value;
}
```

C++/WinRT クラスは、[**winrt::implements**](/uwp/cpp-ref-for-winrt/implements) テンプレートから直接的または間接的に派生します。 そのため、C++/WinRT オブジェクトでは、[**implements::get_strong**](/uwp/cpp-ref-for-winrt/implements#implementsget_strong-function) というその保護メンバー関数を呼び出し、その *this* ポインターの強参照を取得できます。 上のコード例では実際に `strong_this` 変数を使用する必要はないことにご留意ください。**get_strong** を呼び出すだけで、C++/WinRT オブジェクトの参照カウントがインクリメントされ、その暗黙的 *this* ポインターの有効な状態が維持されます。

> [!IMPORTANT]
> **get_strong** は **winrt::implements** 構造体テンプレートのメンバー関数であるため、C++/WinRT クラスなど、**winrt::implements** から直接的または間接的に派生するクラスからのみ呼び出すことができます。 **winrt::implements** から派生されるものに関する詳細と例については、「[C++/WinRT での API の作成](./author-apis.md)」を参照してください。

これで以前、手順 4 に進んだときに発生した問題が解決されます。 クラス インスタンスの他の参照がすべて消えても、その依存関係を安定させるという予防策がコルーチンでとられます。

強参照が適切でない場合、代わりに [**implements::get_weak**](/uwp/cpp-ref-for-winrt/implements#implementsget_weak-function) を呼び出し、*this* の弱参照を取得できます。 *this* にアクセスする前に強参照を取得できることを確認してください。 繰り返しになりますが、**get_weak** は **winrt::implements** 構造体テンプレートのメンバー関数です。

```cppwinrt
IAsyncOperation<winrt::hstring> RetrieveValueAsync()
{
    auto weak_this{ get_weak() }; // Maybe keep *this* alive.

    co_await 5s;

    if (auto strong_this{ weak_this.get() })
    {
        co_return m_value;
    }
    else
    {
        co_return L"";
    }
}
```

上の例では、強参照が残っていないとき、弱参照ではクラス インスタンスの破棄が止められません。 しかし、メンバー変数にアクセスする前に、強参照を取得できるかどうかを確認する方法が、これにより与えられます。

## <a name="safely-accessing-the-this-pointer-with-an-event-handling-delegate"></a>イベント処理デリゲートで *this* ポインターに安全にアクセスする

### <a name="the-scenario"></a>シナリオ

イベント処理に関する一般情報については、「[C++/WinRT でのデリゲートを使用したイベントの処理](handle-events.md)」を参照してください。

前のセクションでは、コルーチンと同時開催性の領域における潜在的な有効期間の問題を取り上げました。 しかし、オブジェクトのメンバー関数でイベントを処理する場合、あるいはオブジェクトのメンバー関数内にあるラムダ関数内からイベントを処理する場合、イベント受信側 (イベントを処理するオブジェクト) とイベント ソース (イベントを発生させるオブジェクト) の相対的な有効期間を考慮する必要があります。 コード例をいくつか見てみましょう。

下のコードでは、まず、簡単な **EventSource** クラスが定義されます。これから発生する汎用イベントは、それに追加されているデリゲートで処理されます。 この例のイベントではデリゲート タイプとしてたまたま [**Windows::Foundation::EventHandler**](/uwp/api/windows.foundation.eventhandler) が使用されていますが、ここで紹介する問題と解決策はあらゆるデリゲート タイプに適用されます。

次に、**EventRecipient** クラスからラムダ関数の形式で **EventSource::Event** イベントのハンドラーが与えられます。

```cppwinrt
// pch.h
#pragma once
#include <iostream>
#include <winrt/Windows.Foundation.h>

// main.cpp : Defines the entry point for the console application.
#include "pch.h"

using namespace winrt;
using namespace Windows::Foundation;

struct EventSource
{
    winrt::event<EventHandler<int>> m_event;

    void Event(EventHandler<int> const& handler)
    {
        m_event.add(handler);
    }

    void RaiseEvent()
    {
        m_event(nullptr, 0);
    }
};

struct EventRecipient : winrt::implements<EventRecipient, IInspectable>
{
    winrt::hstring m_value{ L"Hello, World!" };

    void Register(EventSource& event_source)
    {
        event_source.Event([&](auto&& ...)
        {
            std::wcout << m_value.c_str() << std::endl;
        });
    }
};

int main()
{
    winrt::init_apartment();

    EventSource event_source;
    auto event_recipient{ winrt::make_self<EventRecipient>() };
    event_recipient->Register(event_source);
    event_source.RaiseEvent();
}
```

パターンは、その *this* ポインターに依存するラムダ イベント ハンドラーがイベントの受信側に与えられるというものです。 イベントの受信側がイベント ソースより長く残るときは必ず、その依存関係よりも長く残ります。 その場合、共通して、このパターンが問題なく機能します。 UI ページでそのページ上にあるコントロールで発生したイベントを処理するときなど、明らかな場合があります。 ページの有効期間がボタンより長いため、ハンドラーの有効期間もボタンより長くなります。 これは、受信側がソースを所有する場合 (データ メンバーとしてなど)、または受信側とソースが兄弟関係にあり、他のオブジェクトによって直接所有されている場合に当てはまります。

ハンドラーの有効期間が、ハンドラーが依存する *this* より長くない場合があることが確実なときは、有効期間の強弱を気にしなくても、通常どおり *this* をキャプチャできます。

ただし、*this* がハンドラー内の自身の用途に表示されない場合などもあります (非同期アクションと非同期操作によって発生する完了イベントと進行状況イベントのハンドラーなど)。それに対処する方法を知ることは重要です。

- イベント ソースでそのイベントが *同期的* に生成される場合は、ハンドラーを取り消して、それ以上イベントを受け取ることはないという確信を持つことができます。 ただし、非同期イベントの場合は、取り消し後 (特にデストラクター内で取り消す場合) でも、破棄が開始された後に実行中のイベントがオブジェクトに到着する可能性があります。 破棄の前に登録を解除する場所を見つけることで問題を軽減できますが、引き続き堅牢なソリューションについて判断してください。
- 非同期メソッドを実装するためにコルーチンを作成する場合は可能です。
- 特定の XAML UI フレームワーク オブジェクト ([**SwapChainPanel**](/uwp/api/windows.ui.xaml.controls.swapchainpanel) など) を使用するまれなケースで、イベント ソースから登録解除しなくても、受信側が最終処理される場合は可能です。

### <a name="the-issue"></a>問題

次に紹介する **main** 関数では、イベント ソースで依然としてイベントを発生させているとき、イベントの受信側が破棄された場合 (おそらく範囲の外に出た場合) どうなるかをシミュレートします。

```cppwinrt
int main()
{
    winrt::init_apartment();

    EventSource event_source;
    auto event_recipient{ winrt::make_self<EventRecipient>() };
    event_recipient->Register(event_source);
    event_recipient = nullptr; // Simulate the event recipient going out of scope.
    event_source.RaiseEvent(); // Behavior is now undefined within the lambda event handler; crashing is likely.
}
```

イベントの受信側は破棄されますが、その中のラムダ イベント ハンドラーは依然として **Event** イベントにサブスクライブされています。 そのイベントが発生すると、ラムダは、その時点では無効になっている *this* ポインターを逆参照しようとします。 そのため、それを使用しようとするハンドラー (または、コルーチンの継続) のコードからアクセス違反が出ます。

> [!IMPORTANT]
> このような状況になった場合は、*this* オブジェクトの有効期間と、キャプチャした *this* オブジェクトがキャプチャの有効期間を超えて存続するかどうかについて考えます。 キャプチャの有効期間を超えることがなければ、下に示すように、強参照または弱参照でキャプチャします。
>
> あるいは、これがご自分のシナリオに適していて、スレッドの考慮事項にも対応する場合は、受信側でイベントが完了した後、あるいは受信側のデストラクターでハンドラーを取り消すという選択肢があります。 「[登録済みデリゲートの取り消し](handle-events.md#revoke-a-registered-delegate)」を参照してください。

ハンドラーは次のように登録します。

```cppwinrt
event_source.Event([&](auto&& ...)
{
    std::wcout << m_value.c_str() << std::endl;
});
```

このラムダでは、参照によってあらゆるローカル変数が自動的にキャプチャされます。 そのため、この例の場合、次のように記述しても同じことになります。

```cppwinrt
event_source.Event([this](auto&& ...)
{
    std::wcout << m_value.c_str() << std::endl;
});
```

いずれの場合でも、生の *this* ポインターをキャプチャするだけです。 それは参照カウントに何の影響も与えません。そのため、現在のオブジェクトの破棄を止めるものはありません。

### <a name="the-solution"></a>解決策

解決策は、強参照 (または、ここで説明するように、より適切な場合は弱参照) をキャプチャすることです。 強参照は参照カウントをインクリメント *します*。また、現在のオブジェクトの有効な状態を維持 *します*。 キャプチャ変数 (この例では `strong_this`) を宣言し、[**implements::get_strong**](/uwp/cpp-ref-for-winrt/implements#implementsget_strong-function) を呼び出すことでそれを初期化します。結果、*this* ポインターの強参照が取得されます。

> [!IMPORTANT]
> **get_strong** は **winrt::implements** 構造体テンプレートのメンバー関数であるため、C++/WinRT クラスなど、**winrt::implements** から直接的または間接的に派生するクラスからのみ呼び出すことができます。 **winrt::implements** から派生されるものに関する詳細と例については、「[C++/WinRT での API の作成](./author-apis.md)」を参照してください。

```cppwinrt
event_source.Event([this, strong_this { get_strong()}](auto&& ...)
{
    std::wcout << m_value.c_str() << std::endl;
});
```

現在のオブジェクトの自動キャプチャを省略し、暗黙的 *this* を経由する代わりに、キャプチャ変数を介してデータ メンバーにアクセスすることもできます。

```cppwinrt
event_source.Event([strong_this { get_strong()}](auto&& ...)
{
    std::wcout << strong_this->m_value.c_str() << std::endl;
});
```

強参照が適切でない場合、代わりに [**implements::get_weak**](/uwp/cpp-ref-for-winrt/implements#implementsget_weak-function) を呼び出し、*this* の弱参照を取得できます。 弱参照では、現在のオブジェクトは存続 *しません*。 そのため、メンバーにアクセスする前に、引き続き弱参照から強参照を取得できることを確認してください。

```cppwinrt
event_source.Event([weak_this{ get_weak() }](auto&& ...)
{
    if (auto strong_this{ weak_this.get() })
    {
        std::wcout << strong_this->m_value.c_str() << std::endl;
    }
});
```

生ポインターをキャプチャする場合は、参照先のオブジェクトが存続することを確認する必要があります。

### <a name="if-you-use-a-member-function-as-a-delegate"></a>デリゲートとしてメンバー関数を使用する場合

ラムダ関数と共に、以上の原則はデリゲートとしてメンバー関数を使用する場合にも適用されます。 構文は異なります。いくつかのコードを見てみましょう。 最初に紹介するのは、潜在的に安全ではないメンバー関数イベント ハンドラーです。生の *this* ポインターが使用されています。

```cppwinrt
struct EventRecipient : winrt::implements<EventRecipient, IInspectable>
{
    winrt::hstring m_value{ L"Hello, World!" };

    void Register(EventSource& event_source)
    {
        event_source.Event({ this, &EventRecipient::OnEvent });
    }

    void OnEvent(IInspectable const& /* sender */, int /* args */)
    {
        std::wcout << m_value.c_str() << std::endl;
    }
};
```

これは、あるオブジェクトとそのメンバー関数を参照する従来の標準的な方法です。 これを安全にする目的で、Windows SDK のバージョン 10.0.17763.0 (Windows 10、バージョン 1809) 以降、ハンドラーが登録される箇所で強参照または弱参照を確立できます。 その箇所で、イベントを受信するオブジェクトは有効です。

強参照の場合、生の *this* ポインターの代わりに [**get_strong**](/uwp/cpp-ref-for-winrt/implements#implementsget_strong-function) を呼び出します。 C++/WinRT によって、結果的に生成されるデリゲートでは、現在のオブジェクトの強参照が保持されます。

```cppwinrt
event_source.Event({ get_strong(), &EventRecipient::OnEvent });
```

強参照をキャプチャすることは、ハンドラーが登録解除されてすべての未処理のコールバックが返された後にのみ、オブジェクトが破棄の対象になることを意味します。 ただし、その保証はイベントが発生した時点でのみ有効です。 イベント ハンドラーが非同期の場合は、最初の中断ポイントの前に、コルーチンにクラス インスタンスへの強参照を与える必要があります (詳細とコードについては、このトピックで前述した「[class-member コルーチンで *this* ポインターに安全にアクセスする](#safely-accessing-the-this-pointer-in-a-class-member-coroutine)」を参照してください)。 ただし、これにより、イベント ソースとオブジェクトの間に循環参照が作成されるため、イベントを取り消すことによって明示的に中断する必要があります。

弱参照の場合、[**get_weak**](/uwp/cpp-ref-for-winrt/implements#implementsget_weak-function) を呼び出します。 C++/WinRT によって。結果的に生成されるデリゲートでは、弱参照が保持されます。 最後の瞬間、舞台裏では、弱参照を強参照に解決するよう、デリゲートによって試行されます。解決できた場合、そのメンバー関数のみが呼び出されます。

```cppwinrt
event_source.Event({ get_weak(), &EventRecipient::OnEvent });
```

デリゲートがメンバー関数を *呼び出す* 場合、C++/WinRT ではそのハンドラーが返されるまでオブジェクトが存続します。 ただし、ハンドラーが非同期の場合は中断ポイントで返されるため、最初の中断ポイントの前に、コルーチンにクラス インスタンスへの強参照を与える必要があります。 この場合も、詳細については、このセクションで前述した「[class-member コルーチンで *this* ポインターに安全にアクセスする](#safely-accessing-the-this-pointer-in-a-class-member-coroutine)」を参照してください。

### <a name="a-weak-reference-example-using-swapchainpanelcompositionscalechanged"></a>**SwapChainPanel::CompositionScaleChanged** を使用する弱参照の例

このコード例では、弱参照を説明するもう 1 つの方法として、[**SwapChainPanel::CompositionScaleChanged**](/uwp/api/windows.ui.xaml.controls.swapchainpanel.compositionscalechanged) イベントを使用します。 このコードでは、受信側の弱参照をキャプチャするラムダを使用し、イベント ハンドラーが登録されます。

```cppwinrt
winrt::Windows::UI::Xaml::Controls::SwapChainPanel m_swapChainPanel;
winrt::event_token m_compositionScaleChangedEventToken;

void RegisterEventHandler()
{
    m_compositionScaleChangedEventToken = m_swapChainPanel.CompositionScaleChanged([weak_this{ get_weak() }]
        (Windows::UI::Xaml::Controls::SwapChainPanel const& sender,
        Windows::Foundation::IInspectable const& object)
    {
        if (auto strong_this{ weak_this.get() })
        {
            strong_this->OnCompositionScaleChanged(sender, object);
        }
    });
}

void OnCompositionScaleChanged(Windows::UI::Xaml::Controls::SwapChainPanel const& sender,
    Windows::Foundation::IInspectable const& object)
{
    // Here, we know that the "this" object is valid.
}
```

ラムダのキャプチャ句で、一時変数を作成し、*このオブジェクト* の弱参照を表示します。 ラムダ式の本文で、*このオブジェクト* の強参照を取得する場合は、**OnCompositionScaleChanged** 関数が呼び出されます。 これにより、**OnCompositionScaleChanged** 内で *このオブジェクト* を安全に使用することができます。

## <a name="weak-references-in-cwinrt"></a>C++/WinRT の弱参照

以上、弱参照の使用を確認しました。 一般的に、弱参照は循環参照から抜けるときに最適です。 たとえば、XAML ベースの UI フレームワークのネイティブ実装の場合&mdash;フレームワーク設計の歴史的背景が理由で&mdash;C++/WinRT の弱参照メカニズムは循環参照を処理するために必要になります。 ただし、XAML 以外では、弱参照はおそらく使用する必要はありません (本質的に XAML 固有のものがあるというわけではありません)。 むしろ、通常は、循環参照や弱参照が必要とならないように独自の C++/WinRT API を設計することができるはずです。 

宣言するすべての型について、いつどこで弱参照が必要になるかが C++/WinRT に対してすぐに明白になるわけではありません。 したがって、C++/WinRT では構造体テンプレート [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements) で弱参照サポートを自動的に提供し、そこから直接的または間接的に独自の C++/WinRT の型を派生します。 利用に応じた料金制度であるため、オブジェクトが [**IWeakReferenceSource**](/windows/desktop/api/weakreference/nn-weakreference-iweakreferencesource) で実際に照会されない限り料金はかかりません。 また、[そのサポートを除外する](#opting-out-of-weak-reference-support)ことを明示的に選択することができます。

### <a name="code-examples"></a>コード例
[**winrt::weak_ref**](/uwp/cpp-ref-for-winrt/weak-ref) 構造体テンプレートは、クラス インスタンスへの弱参照を取得するための 1 つのオプションです。

```cppwinrt
Class c;
winrt::weak_ref<Class> weak{ c };
```

または、[**winrt::make_weak**](/uwp/cpp-ref-for-winrt/make-weak) ヘルパー関数を使用できます。

```cppwinrt
Class c;
auto weak = winrt::make_weak(c);
```

弱参照を作成してもオブジェクト自体の参照カウントには影響しません。制御ブロックが割り当てられるだけです。 その制御ブロックが弱参照セマンティクスの実装を処理します。 その後、弱参照から強参照への昇格を試みて、成功した場合は使用することができます。

```cppwinrt
if (Class strong = weak.get())
{
    // use strong, for example strong.DoWork();
}
```

他の強参照が存在する場合、[**weak_ref::get**](/uwp/cpp-ref-for-winrt/weak-ref#weak_refget-function) の呼び出しにより参照カウントが増分され、呼び出し元に強参照が返されます。

### <a name="opting-out-of-weak-reference-support"></a>弱参照サポートの除外
弱参照サポートは自動です。 ただし、[**winrt::no_weak_ref**](/uwp/cpp-ref-for-winrt/no-weak-ref) マーカー構造体をテンプレート引数として基底クラスに渡すことによって、そのサポートを明示的に除外することを選択できます。

**winrt::implements** から直接派生する場合。

```cppwinrt
struct MyImplementation: implements<MyImplementation, IStringable, no_weak_ref>
{
    ...
}
```

ランタイム クラスを作成している場合。

```cppwinrt
struct MyRuntimeClass: MyRuntimeClassT<MyRuntimeClass, no_weak_ref>
{
    ...
}
```

可変個引数パラメーター パックのどこにマーカー構造体が現れるかは関係ありません。 除外された型に対して弱参照を要求すると、コンパイラは "*これは弱参照サポート専用です*" というメッセージで知らせます。

## <a name="important-apis"></a>重要な API
* [implements::get_weak 関数](/uwp/cpp-ref-for-winrt/implements#implementsget_weak-function)
* [winrt::make_weak 関数テンプレート](/uwp/cpp-ref-for-winrt/make-weak)
* [winrt::no_weak_ref マーカー構造体](/uwp/cpp-ref-for-winrt/no-weak-ref)
* [winrt::weak_ref 構造体テンプレート](/uwp/cpp-ref-for-winrt/weak-ref)