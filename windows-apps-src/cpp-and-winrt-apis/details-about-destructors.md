---
description: C++/WinRT 2.0 を使用すると、実装型の破棄を延期したり、破棄中に安全にクエリを実行したりすることができます。 このトピックでは、それらの機能についてと、それらをいつ使用するかについて説明します。
title: デストラクターに関する詳細情報
ms.date: 07/19/2019
ms.topic: article
keywords: windows 10、uwp、標準、c++、cpp、winrt、プロジェクション、遅延破棄、安全な照会
ms.localizationpriority: medium
ms.openlocfilehash: 9806ea54665b24c246f2023714a14d94ec3bcc8e
ms.sourcegitcommit: 02cc7aaa408efe280b089ff27484e8bc879adf23
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/23/2019
ms.locfileid: "68387799"
---
# <a name="details-about-destructors"></a>デストラクターに関する詳細情報

C++/WinRT 2.0 を使用すると、実装型の破棄を延期したり、破棄中に安全にクエリを実行したりすることができます。 このトピックでは、それらの機能についてと、それらをいつ使用するかについて説明します。

## <a name="deferred-destruction"></a>遅延破棄

「[直接割当ての診断](/windows/uwp/cpp-and-winrt-apis/diag-direct-alloc)」のトピックでは、実装型でプライベート デストラクターを使用できないことを説明しました。

パブリック デストラクターを使用する利点は、遅延破棄が有効になることです。これは、オブジェクト上で最後の [**IUnknown::Release**](/windows/win32/api/unknwn/nf-unknwn-iunknown-release) 呼び出しを検出し、そのオブジェクトの所有権を取得してその破棄を無期限に延期する機能です。

従来の COM オブジェクトは、本質的に参照カウントされることを思い出してください。参照カウントは [**IUnknown::AddRef**](/windows/win32/api/unknwn/nf-unknwn-iunknown-addref) 関数と **IUnknown::Release** 関数を介して管理されます。 従来の **Release** の実装では、参照カウントが 0 になると従来の COM オブジェクトの C++ デストラクターが呼び出されます。

```cppwinrt
uint32_t WINRT_CALL Release() noexcept
{
    uint32_t const remaining{ subtract_reference() };
 
    if (remaining == 0)
    {
        delete this;
    }
 
    return remaining;
}
```

`delete this;` では、オブジェクトによって占有されているメモリを解放する前に、オブジェクトのデストラクターが呼び出されます。 デストラクター内で何か興味深いことを行う必要がなければ、これは十分に機能します。

```cppwinrt
using namespace winrt::Windows::Foundation;
... 
struct Sample : implements<Sample, IStringable>
{
    winrt::hstring ToString() const;
 
    ~Sample() noexcept
    {
        // Too late to do anything interesting.
    }
};
```

"*興味深い*" こととは何でしょうか。 一例を挙げると、デストラクターは本質的に同期的です。 異なるコンテキストでスレッド固有のリソースを破棄するなどのためにスレッドを切り替えることはできません。 特定のリソースを解放するために必要になる可能性のある他のいくつかのインターフェイスに対してオブジェクトを確実に照会することはできません。 例は他にもあります。 破棄が重要な場合は、より柔軟なソリューションが必要です。 ここで、C++/WinRT の **final_release** 関数が登場します。

```cppwinrt
struct Sample : implements<Sample, IStringable>
{
    winrt::hstring ToString() const;
 
    static void final_release(std::unique_ptr<Sample> ptr) noexcept
    {
        // This is the first stop...
    }
 
    ~Sample() noexcept
    {
        // ...And this happens only when *unique_ptr* finally deletes the object.
    }
};
```

オブジェクトの参照カウントが 0 に遷移したらすぐに **final_release** が呼び出されるように、**Release** の C++/WinRT 実装を更新しました。 この状態では、オブジェクトは、それ以上未解決の参照がないことが確実で、それ自体の排他的所有権を持つことになります。 そのため、それ自体の所有権を、静的な **final_release** 関数に譲渡できます。

つまり、オブジェクトは、共有所有権をサポートするオブジェクトから、排他的に所有されるオブジェクトに変換されています。 **std::unique_ptr** にはオブジェクトの排他的所有権があるので、(それより前に別の場所に移動されていなければ) **std::unique_ptr** がスコープ外に出たときに、そのセマンティクス (したがって、パブリック デストラクターのニーズ) の一部としてオブジェクトが自然に破棄されます。 これが鍵です。 **std::unique_ptr** がオブジェクトを有効なまま保持する場合は、オブジェクトを無期限に使用できます。 オブジェクトを他の場所に移動する方法を次の図に示します。

```cppwinrt
struct Sample : implements<Sample, IStringable>
{
    winrt::hstring ToString() const;
 
    static void final_release(std::unique_ptr<Sample> ptr) noexcept
    {
        gc.push_back(std::move(ptr));
    }
};
```

これは、より確定的なガベージ コレクターと考えてください。 おそらく、より実用的かつ強力に、**final_release** 関数をコルーチンに変換し、必要に応じてスレッドを中断して切り替えながら、その最終的な破棄を 1 か所で処理することができます。

```cppwinrt
struct Sample : implements<Sample, IStringable>
{
    winrt::hstring ToString() const;
 
    static winrt::fire_and_forget final_release(std::unique_ptr<Sample> ptr) noexcept
    {
        co_await winrt::resume_background(); // Unwind the calling thread.
 
        // Safely perform complex teardown here.
    }
};
```

中断によって、(最初に **IUnknown::Release** 関数の呼び出しを開始した) 呼び出し元のスレッドが戻る原因が示されるため、保持されていたオブジェクトがそのインターフェイス ポインターを介して使用できなくなったことが呼び出し元に通知されます。 多くの場合、UI フレームワークでは、オブジェクトが最初に作成された特定の UI スレッド上でそのオブジェクトが破棄されるようにする必要があります。 この機能により、破棄がオブジェクトの解放から分離されるので、このような要件を満たすことが容易になります。

## <a name="safe-queries-during-destruction"></a>破棄中の安全な照会

遅延破棄の概念に基づくと、破棄中にインターフェイスを安全に照会できます。

従来の COM は、2 つの中心となる概念に基づいています。 1 つ目は参照カウントで、もう 1 つはインターフェイスの照会です。 **AddRef** と **Release** に加えて、**IUnknown**インターフェイスには **[QueryInterface]** (/windows/win32/api/unknwn/nf-unknwn-iunknown-queryinterface(refiid_void) が用意されています。 このメソッドは、XAML などの特定の UI フレームワークによって、構成可能型システムをシミュレートするときに XAML 階層を走査するために頻繁に使用されます。 単純な例について考えます。

```cppwinrt
struct MainPage : PageT<MainPage>
{
    ~MainPage()
    {
        DataContext(nullptr);
    }
};
```

これは無害なように "*見えます*"。 この XAML ページでは、デストラクター内のデータ コンテキストをクリアする必要があります。 しかし、[**DataContext**](/uwp/api/windows.ui.xaml.frameworkelement.datacontext) は **FrameworkElement** 基本クラスのプロパティであり、個別の **IFrameworkElement** インターフェイス上に存在します。 その結果、**DataContext** プロパティを呼び出せるようになる前に、C++/WinRT で **QueryInterface** の呼び出しを挿入して適切な vtable を検索する必要があります。 しかし、デストラクター内にいるのは、参照カウントが 0 に遷移したからです。 ここで **QueryInterface** を呼び出すと、その参照カウントが一時的に増加し、再び 0 に戻ったときにオブジェクトがもう一度破棄されます。

C++/WinRT 2.0 は、これをサポートするために強化されました。 簡略化された形式での Release の C++/WinRT 2.0 実装を次に示します。

```cppwinrt
uint32_t Release() noexcept
{
    uint32_t const remaining{ subtract_reference() };
 
    if (remaining == 0)
    {
        m_references = 1; // Debouncing!
        T::final_release(...);
    }
 
    return remaining;
}
```

予測されたように、まず参照カウントを減らし、未解決の参照がない場合にのみ機能します。 ただし、このトピックで既に説明した静的な **final_release** 関数を呼び出す前に、参照カウントを 1 に設定することにより安定させます。 これは、*デバウンス* と呼ばれています (電気工学の用語を借用)。 これは、最後の参照が解放されないようにするために重要です。 このような状況が発生すると、参照カウントが不安定になり、**QueryInterface** の呼び出しを確実にはサポートできなくなります。

参照カウントが無限に増大する可能性があるので、最後の参照が解放された後に **QueryInterface** を呼び出すことは危険です。 オブジェクトの有効期間を延長しない既知のコード パスのみを呼び出すのはユーザーの責任です。 C++/WinRT は、これらの **QueryInterface** 呼び出しを確実に行えるようにすることで、その半分を満たします。

これは、参照カウントを安定化することで行われます。 最後の参照が解放されると、実際の参照カウントは 0 か、まったく予測できない値になります。 後者は、弱参照が関係している場合に発生する可能性があります。 いずれにしても、これは **QueryInterface** の後続の呼び出しが発生した場合に持続できません。必ず参照カウントの一時的な増加の原因となるからです。そのため、デバウンスを参照します。 1 に設定すると、このオブジェクトに対して **Release** への最後の呼び出しが再び行われることはなくなります。 **std::unique_ptr** がオブジェクトを所有するようになりましたが、**QueryInterface**/**Release** のペアに対するバインドされた呼び出しは安全であるため、これはまさに望みどおりです。

もっと興味深い例を考えてみましょう。

```cppwinrt
struct MainPage : PageT<MainPage>
{
    ~MainPage()
    {
        DataContext(nullptr);
    }

    static winrt::fire_and_forget final_release(std::unique_ptr<MainPage> ptr)
    {
        co_await 5s;
        co_await winrt::resume_foreground(ptr->Dispatcher());
        ptr = nullptr;
    }
};
```

まず、**final_release** 関数が呼び出され、クリーンアップの時刻になったことが実装に通知されます。 ここでは、**final_release** はコルーチンになっています。 最初の中断点をシミュレートするために、スレッド プール上で数秒間待機することから始めます。 その後、ページのディスパッチャー スレッド上で再開します。 [**Dispatcher**](/uwp/api/windows.ui.xaml.dependencyobject.dispatcher) は **DependencyObject** 基本クラスのプロパティなので、この最後の手順ではクエリが必要になります。 最後に、**std::unique_ptr** への `nullptr` の割り当てにより、ページが実際に削除されます。 その後、ページのデストラクターが呼び出されます。

デストラクター内で、データ コンテキストをクリアします。ご存じのように、これには **FrameworkElement** 基本クラスに対するクエリが必要です。

参照カウントのデバウンス (または参照カウントの安定化) が C++/WinRT 2.0 によって提供されているため、このすべてが可能です。