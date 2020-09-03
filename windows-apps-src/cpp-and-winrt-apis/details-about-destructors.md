---
description: C++/WinRT 2.0 のこれらの拡張ポイントを使用すると、実装の種類の破棄を延期したり、破棄中に安全にクエリを実行したり、プロジェクションが実行された方法に対してエントリをフックしたりすることができます。
title: 実装の種類の拡張ポイント
ms.date: 09/26/2019
ms.topic: article
keywords: windows 10、uwp、標準、c++、cpp、winrt、プロジェクション、遅延破棄、安全な照会
ms.localizationpriority: medium
ms.openlocfilehash: 6b15c32bb35bec1f6a8e8d59e6aefe17ebf74b5d
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89170329"
---
# <a name="extension-points-for-your-implementation-types"></a>実装の種類の拡張ポイント

[winrt::implements 構造体テンプレート](/uwp/cpp-ref-for-winrt/implements)は、(ランタイム クラスとアクティベーション ファクトリの) 独自の [C++/WinRT](./intro-to-using-cpp-with-winrt.md) 実装の直接的または間接的な派生元です。

このトピックでは、C++/WinRT 2.0 の **winrt::implements** の拡張ポイントについて説明します。 検査可能ブジェクト([IInspectable](/windows/win32/api/inspectable/nn-inspectable-iinspectable) インターフェイスの意味での*検査可能*) の既定の動作をカスタマイズするために、これらの拡張ポイントを実装の種類に実装することを選択できます。

これらの拡張ポイントを使用すると、実装の種類の破棄を延期したり、破棄中に安全にクエリを実行したり、投影されたメソッドに対してエントリをフックおよび終了したりすることができます。 このトピックでは、それらの機能について説明し、それらをいつどのように使用するかについて詳しく説明します。

## <a name="deferred-destruction"></a>遅延破棄

「[直接割当ての診断](./diag-direct-alloc.md)」のトピックでは、実装型でプライベート デストラクターを使用できないことを説明しました。

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

これは、より確定的なガベージ コレクターと考えてください。

通常、オブジェクトは  **std::unique_ptr**  が破棄されると破棄されますが、 **std::unique_ptr::reset** を呼び出すことで破棄を早めることができます。または、 **std :: unique_ptr**  をどこかに保存して、破棄を延期することもできます。

おそらく、より実用的かつ強力に、**final_release** 関数をコルーチンに変換し、必要に応じてスレッドを中断して切り替えながら、その最終的な破棄を 1 か所で処理することができます。

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

従来の COM は、2 つの中心となる概念に基づいています。 1 つ目は参照カウントで、もう 1 つはインターフェイスの照会です。 **AddRef** と **Release** に加えて、**IUnknown**インターフェイスには [**QueryInterface**](/windows/win32/api/unknwn/nf-unknwn-iunknown-queryinterface(refiid_void)) が用意されています。 このメソッドは、XAML などの特定の UI フレームワークによって、構成可能型システムをシミュレートするときに XAML 階層を走査するために頻繁に使用されます。 単純な例について考えます。

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

## <a name="method-entry-and-exit-hooks"></a>メソッドのエントリと終了のフック

あまり一般的に使用されない拡張ポイントは、 **abi_guard** 構造体、および **abi_enter**  と  **abi_exit** 関数です。

実装の種類によって関数 **abi_enter** が定義されている場合、その関数は、投影されたインターフェイス メソッド( [IInspectable](/windows/win32/api/inspectable/nn-inspectable-iinspectable) のメソッドはカウントされない) のすべてのエントリで呼び出されます。

同様に、**abi_exit** を定義すると、そのようなすべてのメソッドの終了時に呼び出されます。ただし、**abi_enter**  が例外をスローした場合は呼び出されません。 *なお*、投影されたインターフェイス メソッド自体によって例外がスローされた場合には呼び出されます。

たとえば、**abi_enter** を使用すると、 **Shut­Down**  または  **Disconnect** メソッドの呼び出しの後など、オブジェクトが使用不可能な状態になった後にクライアントがオブジェクトを使用しようとした場合に、仮想的な  **invalid_state_error** 例外をスローできます。 C++/WinRT 反復子クラスは、この機能を使用して、基になるコレクションが変更された場合に、 **abi_enter** 関数で無効な状態例外をスローします。

単純な **abi_enter**  および  **abi_exit** 関数に加えて、 **abi_guard** という名前の入れ子になった型を定義できます。 その場合、**abi_guard** のインスタンスは、オブジェクトへの参照をコンストラクター パラメーターとして使用して、投影されたインターフェイス メソッドの各 (非 **IInspectable**) へのエントリに対して作成されます。 その後、 **abi_guard**  は、メソッドの終了時に破棄されます。 任意の追加の状態を **abi_guard** の型に設定できます。

独自の **abi_guard** を定義しない場合は、構築時に **abi_enter**  を呼び出し、破棄時に  **abi_exit** を呼び出す既定のものが使用されます。

これらのガードが使用されるのは、*投影されたインターフェイスを介して* メソッドが呼び出される場合だけです。 実装オブジェクトでメソッドを直接呼び出すと、それらの呼び出しはガードなしで実装に直接送られます。

次にコード例を示します。

```cppwinrt
struct Sample : SampleT<Sample, IClosable>
{
    void abi_enter();
    void abi_exit();

    void Close();
};

void example1()
{
    auto sampleObj1{ winrt::make<Sample>() };
    sampleObj1.Close(); // Calls abi_enter and abi_exit.
}

void example2()
{
    auto sampleObj2{ winrt::make_self<Sample>() };
    sampleObj2->Close(); // Doesn't call abi_enter nor abi_exit.
}

// A guard is used only for the duration of the method call.
// If the method is a coroutine, then the guard applies only until
// the IAsyncXxx is returned; not until the coroutine completes.

IAsyncAction CloseAsync()
{
    // Guard is active here.
    DoWork();

    // Guard becomes inactive once DoOtherWorkAsync
    // returns an IAsyncAction.
    co_await DoOtherWorkAsync();

    // Guard is not active here.
}
```