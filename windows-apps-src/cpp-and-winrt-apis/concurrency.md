---
description: このトピックでは、C++/WinRT を使用した Windows ランタイムの非同期オブジェクトの作成方法と利用方法について説明します。
title: C++/WinRT を使用した同時実行操作と非同期操作
ms.date: 07/08/2019
ms.topic: article
keywords: Windows 10、uwp、標準、c++、cpp、winrt、プロジェクション、同時実行、非同期、非同期、非同期操作
ms.localizationpriority: medium
ms.openlocfilehash: f7db1e5810de478f1c6198860100409d79d4f5d5
ms.sourcegitcommit: d37a543cfd7b449116320ccfee46a95ece4c1887
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/16/2019
ms.locfileid: "68270142"
---
# <a name="concurrency-and-asynchronous-operations-with-cwinrt"></a>C++/WinRT を使用した同時実行操作と非同期操作

このトピックでは、[C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) を使用した Windows ランタイムの非同期オブジェクトの作成方法と利用方法について説明します。

## <a name="asynchronous-operations-and-windows-runtime-async-functions"></a>非同期操作と Windows ランタイムの "非同期" 関数

完了までの時間が 50 ミリ秒を超える可能性がある Windows ランタイム API は、非同期の関数 (末尾が "Async") として実装されます。 非同期関数を実装すると、別のスレッドの作業が開始され、非同期操作を表すオブジェクトがすぐに返されます。 非同期操作が完了すると、作業結果が含まれるオブジェクトが返されます。 **Windows::Foundation** Windows ランタイムの名前空間には 4 種類の非同期操作オブジェクトが含まれます。

- [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction)
- [**IAsyncActionWithProgress&lt;TProgress&gt;** ](/uwp/api/windows.foundation.iasyncactionwithprogress_tprogress_)
- [**IAsyncOperation&lt;TResult&gt;** ](/uwp/api/windows.foundation.iasyncoperation_tresult_)
- [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;** ](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_)

各非同期操作は、**winrt::Windows::Foundation** C++/WinRT の名前空間で対応する型に投影されます。 また、C++/WinRT には内部 await アダプター構造体も含まれます。 これを直接使用することはありませんが、この構造体により、これらの非同期操作型のいずれかを返す関数の結果を一緒に待機するように `co_await` ステートメントを記述することができます。 その後、これらの型を返す独自のコルーチンを作成できます。

たとえば、非同期の Windows 関数である [**SyndicationClient::RetrieveFeedAsync**](https://docs.microsoft.com/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync) は、[**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;** ](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_) 型の非同期操作オブジェクトを返します。 それでは、C++/WinRT を使用してそのような API を呼び出すためのいくつかの方法 (最初はブロック、次に非ブロック) を見てみましょう。

## <a name="block-the-calling-thread"></a>呼び出しスレッドのブロック

次のコード例では、**RetrieveFeedAsync** から非同期操作オブジェクトを受け取り、非同期操作の結果が利用可能になるまで呼び出しスレッドをブロックするようにこのオブジェクトで **get** を呼び出します。

この例を **Windows コンソール アプリケーション (C++/WinRT)** プロジェクトのメイン ソース コード ファイルに直接コピーして貼り付ける場合は、最初にプロジェクト プロパティで **[プリコンパイル済みヘッダーを使用しない]** を設定します。

```cppwinrt
// main.cpp
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Web.Syndication.h>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Web::Syndication;

void ProcessFeed()
{
    Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
    SyndicationClient syndicationClient;
    SyndicationFeed syndicationFeed{ syndicationClient.RetrieveFeedAsync(rssFeedUri).get() };
    // use syndicationFeed.
}

int main()
{
    winrt::init_apartment();
    ProcessFeed();
}
```

**get** を呼び出すとコーディングが簡単になり、何らかの理由でコルーチンを使用しないコンソール アプリやバックグラウンド スレッドに最適です。 ただし、同時実行でも非同期でもないため、UI スレッドには適していません (UI スレッドで使用しようとすると、最適化されないビルドでアサーションが発生します)。 OS スレッドの他の有用な作業を妨げないように、さまざまな方法が必要です。

## <a name="write-a-coroutine"></a>コルーチンの作成

C++/WinRT は C++ コルーチンをプログラミング モデルに統合し、結果を連携して待機するための自然な方法を提供します。 コルーチンを作成すると、独自の Windows ランタイム非同期操作を作成することができます。 次のコード例で、**ProcessFeedAsync** はコルーチンします。

> [!NOTE]
> **get** 関数は、C++/WinRT プロジェクション型 **winrt::Windows::Foundation::IAsyncAction** に存在するため、任意の C++/WinRT プロジェクト内からこの関数を呼び出すことができます。 **get** 関数が [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction) インターフェイスのメンバーとして一覧表示されないのは、この関数が実際の Windows ランタイム型 **IAsyncAction** のアプリケーション バイナリ インターフェイス (ABI) サーフェスの一部ではないからです。

```cppwinrt
// main.cpp
#include <iostream>
#include <winrt/Windows.Foundation.Collections.h>
#include <winrt/Windows.Web.Syndication.h>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Web::Syndication;

void PrintFeed(SyndicationFeed const& syndicationFeed)
{
    for (SyndicationItem const& syndicationItem : syndicationFeed.Items())
    {
        std::wcout << syndicationItem.Title().Text().c_str() << std::endl;
    }
}

IAsyncAction ProcessFeedAsync()
{
    Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
    SyndicationClient syndicationClient;
    SyndicationFeed syndicationFeed{ co_await syndicationClient.RetrieveFeedAsync(rssFeedUri) };
    PrintFeed(syndicationFeed);
}

int main()
{
    winrt::init_apartment();

    auto processOp{ ProcessFeedAsync() };
    // do other work while the feed is being printed.
    processOp.get(); // no more work to do; call get() so that we see the printout before the application exits.
}
```

コルーチンは中断して再開できる関数です。 上記の **ProcessFeedAsync** コルーチンでは、`co_await` ステートメントに到達すると、コルーチンは **RetrieveFeedAsync** 呼び出しを非同期的に起動してからすぐに自身を中断し、呼び出し元 (上記の例の **main**) に戻ります。 次に、フィードが取得および印刷されるまで、**main** は作業を続けます。 完了すると (**RetrieveFeedAsync** 呼び出しが完了すると)、**ProcessFeedAsync** コルーチンは次のステートメントを再開します。

コルーチンは別のコルーチンに集約することができます。 または、**get** を呼び出してブロックするか、完了するまで待機します (結果が存在する場合には取得します)。 または、Windows ランタイムをサポートする別のプログラミング言語に渡すことができます。

また、デリゲートを使用して、非同期アクションおよび操作の完了イベントと進行状況イベントを処理することもできます。 詳細とコード例については、「[非同期アクションと操作のデリゲート型](handle-events.md#delegate-types-for-asynchronous-actions-and-operations)」をご覧ください。

## <a name="asychronously-return-a-windows-runtime-type"></a>Windows ランタイム型を非同期的に返す

次の例では、特定の URI で呼び出しを **RetrieveFeedAsync** にラップして、[**SyndicationFeed**](/uwp/api/windows.web.syndication.syndicationfeed) を非同期的に返す **RetrieveBlogFeedAsync** 関数を示します。

```cppwinrt
// main.cpp
#include <iostream>
#include <winrt/Windows.Foundation.Collections.h>
#include <winrt/Windows.Web.Syndication.h>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Web::Syndication;

void PrintFeed(SyndicationFeed const& syndicationFeed)
{
    for (SyndicationItem const& syndicationItem : syndicationFeed.Items())
    {
        std::wcout << syndicationItem.Title().Text().c_str() << std::endl;
    }
}

IAsyncOperationWithProgress<SyndicationFeed, RetrievalProgress> RetrieveBlogFeedAsync()
{
    Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
    SyndicationClient syndicationClient;
    return syndicationClient.RetrieveFeedAsync(rssFeedUri);
}

int main()
{
    winrt::init_apartment();

    auto feedOp{ RetrieveBlogFeedAsync() };
    // do other work.
    PrintFeed(feedOp.get());
}
```

次の例では、**RetrieveBlogFeedAsync** は進捗状況と戻り値の両方が含まれる **IAsyncOperationWithProgress** を返します。 **RetrieveBlogFeedAsync** が自分の処理を行い、フィードを取得している間は、他の作業を行うことができます。 次に、非同期操作オブジェクトで **get** を呼び出し、ブロックするか完了まで待機して、操作結果を取得します。

Windows ランタイム型を非同期的に返す場合は、[**IAsyncOperation&lt;TResult&gt;** ](/uwp/api/windows.foundation.iasyncoperation_tresult_) または [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;** ](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_) を返す必要があります。 ファースト パーティ製またはサード パーティ製のランタイム クラス、または Windows ランタイム関数との間で受け渡しできる任意の型 (例、`int` または **winrt::hstring**) がこれに適合します。 コンパイラは、Windows ランタイム型以外でこれらの非同期操作型のいずれかを使用しようとすると表示される "*WinRT 型である必要があります*" というエラーをサポートします。

コルーチンに 1 つ以上の `co_await` ステートメントがない場合、コルーチンであると認められるために、1 つ以上の `co_return` または 1 つの `co_yield` ステートメントが必要です。 非同期操作を必要とせず、そのためにコンテキストのブロックや切り替えを行わずに、コルーチンが値を返すことができる場合があります。 値をキャッシュすることでそれを行う例を次に示します (2 回目以降は呼び出されます)。

```cppwinrt
winrt::hstring m_cache;

IAsyncOperation<winrt::hstring> ReadAsync()
{
    if (m_cache.empty())
    {
        // Asynchronously download and cache the string.
    }
    co_return m_cache;
}
``` 

## <a name="asychronously-return-a-non-windows-runtime-type"></a>Windows ランタイム型以外を非同期的に返す

Windows ランタイム型*以外*の型を非同期的に返す場合は、並列パターン ライブラリ (PPL) [**concurrency::task**](/cpp/parallel/concrt/reference/task-class) を返す必要があります。 **std::future**よりもパフォーマンスに優れているため (また、今後の互換性の向上により)、**concurrency::task** をお勧めします。

> [!TIP]
> `<pplawait.h>` を含めると、コルーチンの型として **concurrency::task** を使用できます。

```cppwinrt
// main.cpp
#include <iostream>
#include <ppltasks.h>
#include <winrt/Windows.Foundation.Collections.h>
#include <winrt/Windows.Web.Syndication.h>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Web::Syndication;

concurrency::task<std::wstring> RetrieveFirstTitleAsync()
{
    return concurrency::create_task([]
        {
            Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
            SyndicationClient syndicationClient;
            SyndicationFeed syndicationFeed{ syndicationClient.RetrieveFeedAsync(rssFeedUri).get() };
            return std::wstring{ syndicationFeed.Items().GetAt(0).Title().Text() };
        });
}

int main()
{
    winrt::init_apartment();

    auto firstTitleOp{ RetrieveFirstTitleAsync() };
    // Do other work here.
    std::wcout << firstTitleOp.get() << std::endl;
}
```

## <a name="parameter-passing"></a>パラメーターの引き渡し

同期関数では、既定で `const&` パラメーターを使用する必要があります。 これにより、コピーのオーバーヘッドが回避されます (参照カウント、つまりインタロックされたインクリメントとデクリメントが含まれます)。

```cppwinrt
// Synchronous function.
void DoWork(Param const& value);
```

ただし、コルーチンに参照パラメーターを渡した場合、問題が発生する可能性があります。

```cppwinrt
// NOT the recommended way to pass a value to a coroutine!
IASyncAction DoWorkAsync(Param const& value)
{
    // While it's ok to access value here...

    co_await DoOtherWorkAsync(); // (this is the first suspension point)...

    // ...accessing value here carries no guarantees of safety.
}
```

コルーチンでは、最初の一時停止ポイントまでは実行は同期であり、そこで呼び出し元に制御が返され、呼び出しフレームが範囲外になります。 コルーチンが再開するまで、参照パラメーターが参照するソース値に何かが発生している可能性があります。 コルーチンの観点からは、参照パラメーターには管理されていない有効期間があります。 そのため、上記の例では、`co_await` まで*値*に安全にアクセスできますが、それ以降は安全ではありません。 呼び出し元によって "*値*" が破棄された場合、その後にコルーチン内で値にアクセスしようとすると、メモリが破損する結果になります。 また、その後その関数が中断し、再開した後で*値*を使用しようとするリスクがある場合、**DoOtherWorkAsync** に安全に*値*を渡すこともできません。

中断して再開した後で、パラメーターを安全に使用できるようにするには、コルーチンでは値渡しを既定で使用し、値によってキャプチャされて有効期間の問題が回避されるようにする必要があります。 それを行うことが安全であると確信できるためにそのガイダンスから逸脱できる場合は稀です。

```cppwinrt
// Coroutine
IASyncAction DoWorkAsync(Param value); // not const&
```

値で渡すには、引数の移動またはコピーが低コストである必要があり、通常はスマート ポインターを使用します。

(値を移動しない限り) 定数値を渡すことが良い方法であるということにも議論の余地があります。 コピー元のソース値に影響はありませんが、意図が明確になり、誤ってコピーを変更した場合に役立ちます。

```cppwinrt
// coroutine with strictly unnecessary const (but arguably good practice).
IASyncAction DoWorkAsync(Param const value);
```

非同期の呼び出し先に標準ベクターを渡す方法について説明した「[標準的な配列とベクトル](std-cpp-data-types.md#standard-arrays-and-vectors)」も参照してください。

コルーチンのシグネチャを変更することはできなくても、実装は変更できる場合は、最初の `co_await` の前にローカル コピーを作成することができます。

```cppwinrt
IASyncAction DoWorkAsync(Param const& value)
{
    auto safe_value = value;
    // It's ok to access both safe_value and value here.

    co_await DoOtherWorkAsync();

    // It's ok to access only safe_value here (not value).
}
```

`Param` のコピーが高コストの場合は、最初の `co_await` の前に、必要な部分だけを抽出します。

```cppwinrt
IASyncAction DoWorkAsync(Param const& value)
{
    auto safe_data = value.data;
    // It's ok to access safe_data, value.data, and value here.

    co_await DoOtherWorkAsync();

    // It's ok to access only safe_data here (not value.data, nor value).
}
```

## <a name="safely-accessing-the-this-pointer-in-a-class-member-coroutine"></a>class-member コルーチンで *this* ポインターに安全にアクセスする

「[C++/WinRT の強参照と弱参照](/windows/uwp/cpp-and-winrt-apis/weak-references#safely-accessing-the-this-pointer-in-a-class-member-coroutine)」をご覧ください。

## <a name="offloading-work-onto-the-windows-thread-pool"></a>Windows スレッド プールへの処理のオフロード

コルーチンは、関数が呼び出し元に実行を返すまで呼び出し元がブロックされるという点で、他と同様の関数です。 コルーチンが最初に返すのは、最初の `co_await`、`co_return`、または `co_yield` です。

そのため、コルーチンで計算処理にかかる処理を行う前に、呼び出し元がブロックされないように呼び出し元に実行を返す必要があります (つまり、一時停止ポイントを導入します)。 その他の操作を `co_await` することでこれをまだ行っていない場合は、[**winrt::resume_background**](/uwp/cpp-ref-for-winrt/resume-background) 関数を `co_await` できます。 これにより、呼び出し元に制御が返され、スレッド プールのスレッドですぐに実行が再開されます。

実装で使用されているスレッド プールは低レベルの [Windows スレッド プール](https://docs.microsoft.com/windows/desktop/ProcThread/thread-pool-api)であるため、最適に効率化されます。

```cppwinrt
IAsyncOperation<uint32_t> DoWorkOnThreadPoolAsync()
{
    co_await winrt::resume_background(); // Return control; resume on thread pool.

    uint32_t result;
    for (uint32_t y = 0; y < height; ++y)
    for (uint32_t x = 0; x < width; ++x)
    {
        // Do compute-bound work here.
    }
    co_return result;
}
```

## <a name="programming-with-thread-affinity-in-mind"></a>スレッドの関係を考慮したプログラミング

このシナリオは、前のシナリオをさらに詳しく説明しています。 一部の処理をスレッド プールにオフロードするが、ユーザー インターフェイス (UI) で進行状況を表示したいとします。

```cppwinrt
IAsyncAction DoWorkAsync(TextBlock textblock)
{
    co_await winrt::resume_background();
    // Do compute-bound work here.

    textblock.Text(L"Done!"); // Error: TextBlock has thread affinity.
}
```

上のコードは、[**winrt::hresult_wrong_thread**](/uwp/cpp-ref-for-winrt/error-handling/hresult-wrong-thread) 例外をスローします。これは、**TextBlock** がそれを作成したスレッド (UI スレッド) から更新する必要があるためです。 1 つの解決方法は、コルーチンが最初に呼び出されたスレッド コンテキストをキャプチャする方法です。 これを行うには、[**winrt::apartment_context**](/uwp/cpp-ref-for-winrt/apartment-context) オブジェクトをインスタンス化し、バックグラウンド処理を実行してから、**apartment_context** を `co_await` して呼び出し元コンテキストに切り替えて戻ります。

```cppwinrt
IAsyncAction DoWorkAsync(TextBlock textblock)
{
    winrt::apartment_context ui_thread; // Capture calling context.

    co_await winrt::resume_background();
    // Do compute-bound work here.

    co_await ui_thread; // Switch back to calling context.

    textblock.Text(L"Done!"); // Ok if we really were called from the UI thread.
}
```

上のコルーチンが **TextBlock** を作成した UI スレッドから呼び出される限り、この手法は機能します。 アプリで多くの場合にそれを確信できます。

スレッドの呼び出しがよくわからない場合を対象とした、UI の更新に対するより一般的な解決策として、[**winrt::resume_foreground**](/uwp/cpp-ref-for-winrt/resume-foreground) 関数を `co_await` して特定のフォアグラウンド スレッドに切り替えることができます。 次のコード例では、([**Dispatcher**](/uwp/api/windows.ui.xaml.dependencyobject.dispatcher#Windows_UI_Xaml_DependencyObject_Dispatcher) プロパティにアクセスして) **TextBlock** に関連するディスパッチャー オブジェクトを渡すことでフォアグラウンド スレッドを指定しています。 **winrt::resume_foreground** の実装では、そのディスパッチャー オブジェクトで [**CoreDispatcher.RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync) を呼び出し、コルーチンでその後に続く処理を実行しています。

```cppwinrt
IAsyncAction DoWorkAsync(TextBlock textblock)
{
    co_await winrt::resume_background();
    // Do compute-bound work here.

    co_await winrt::resume_foreground(textblock.Dispatcher()); // Switch to the foreground thread associated with textblock.

    textblock.Text(L"Done!"); // Guaranteed to work.
}
```

## <a name="execution-contexts-resuming-and-switching-in-a-coroutine"></a>実行コンテキスト、再開、およびコルーチンでの切り替え

大まかに言えば、コルーチン内の一時停止ポイントの後に、実行の元のスレッドが消えて、いずれかのスレッドで再開が発生する場合があります (つまり、いずれかのスレッドで非同期操作のために**完了した**メソッドが呼び出されることがあります)。

ただし、4 つの Windows ランタイムの非同期操作型 (**IAsyncXxx**) のいずれかを `co_await` する場合、`co_await` するポイントで呼び出し元コンテキストが C++/WinRT によってキャプチャされます。 また、継続が再開されたときに、引き続きそのコンテキストにいることが保証されます。 C++/WinRT では、ユーザーが呼び出し元コンテキストに既にいるかどうかが確認され、いない場合はそれに切り替えられることでこれが行われます。 `co_await` の前にシングル スレッド アパートメント (STA) スレッドにいた場合は、その後も同じスレッドにいることになり、`co_await` の前にマルチ スレッド アパートメント (MTA) スレッドにいた場合は、その後もそこにいることになります。

```cppwinrt
IAsyncAction ProcessFeedAsync()
{
    Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
    SyndicationClient syndicationClient;

    // The thread context at this point is captured...
    SyndicationFeed syndicationFeed{ co_await syndicationClient.RetrieveFeedAsync(rssFeedUri) };
    // ...and is restored at this point.
}
```

この動作に依存できるのは、C++WinRT で、これらの Windows ランタイムの非同期操作型を C++ コルーチンの言語サポートに適応させるコードが提供されているからです (これらのコードは待機アダプターと呼ばれます)。 C++/WinRT でその他の待機可能な型は、単純なスレッド プール ラッパーやヘルパーなので、これらはスレッド プールで完了します。

```cppwinrt
using namespace std::chrono;
IAsyncOperation<int> return_123_after_5s()
{
    // No matter what the thread context is at this point...
    co_await 5s;
    // ...we're on the thread pool at this point.
    co_return 123;
}
```

(C++/WinRT のコルーチンの実装内であっても) 他の型を `co_await` する場合は、別のライブラリによってアダプターが提供されるので、再開とコンテキストの観点から、これらのアダプターで何が行われるかを理解する必要があります。

コンテキストの切り替えを最小限に保持するには、このトピックで既に説明した手法の一部を使用できます。 これを行う実例をいくつか見てみましょう。 次の擬似コードの例では、Windows ランタイム API を呼び出してイメージを読み込み、そのイメージをバックグラウンド スレッドにドロップして処理してから、UI スレッドに戻って UI でイメージを表示するイベント ハンドラーの概要を示します。

```cppwinrt
IAsyncAction MainPage::ClickHandler(IInspectable /* sender */, RoutedEventArgs /* args */)
{
    // We begin in the UI context.

    // Call StorageFile::OpenAsync to load an image file.

    // The call to OpenAsync occurred on a background thread, but C++/WinRT has restored us to the UI thread by this point.

    co_await winrt::resume_background();

    // We're now on a background thread.

    // Process the image.

    co_await winrt::resume_foreground(this->Dispatcher());

    // We're back on MainPage's UI thread.

    // Display the image in the UI.
}
```

このシナリオでは、**StorageFile::OpenAsync** の呼び出しが少々非効率的です。 C++/WinRT が UI スレッドのコンテキストを復元した後の再開時に、(ハンドラーが実行を呼び出し元に返せるように) バックグラウンド スレッドへの必要なコンテキスト スイッチがあります。 ただし、この場合は、UI を更新する寸前まで UI スレッド上に存在する必要はありません。 **winrt::resume_background** を呼び出す*前に*、呼び出す Windows ランタイム API の数が多くなるほど、不要な前後のコンテキスト スイッチが多く発生します。 解決策は、その前に、Windows ランタイム API を*一切*呼び出さないことです。 それらをすべて **winrt::resume_background** の後に移動します。

```cppwinrt
IAsyncAction MainPage::ClickHandler(IInspectable /* sender */, RoutedEventArgs /* args */)
{
    // We begin in the UI context.

    co_await winrt::resume_background();

    // We're now on a background thread.

    // Call StorageFile::OpenAsync to load an image file.

    // Process the image.

    co_await winrt::resume_foreground(this->Dispatcher());

    // We're back on MainPage's UI thread.

    // Display the image in the UI.
}
```

より高度なことを行いたい場合は、独自の await アダプターを記述できます。 たとえば、非同期操作が完了する (そのためコンテキスト スイッチがありません) のと同じスレッドで再開するために `co_await` が必要な場合は、次に示すような await アダプターを記述して開始することができます。

> [!NOTE]
> 次のコード例は、await アダプターのしくみを理解してもらうために、教育目的のみに提供されています。 独自のコードベースでこの手法を使用する場合は、独自の await アダプター構造体を開発してテストすることをお勧めします。 たとえば、**complete_on_any**、**complete_on_current**、**complete_on(dispatcher)** を記述できます。 また、**IAsyncXxx** 型を受け取るテンプレートをテンプレート パラメーターにすることも検討してください。

```cppwinrt
struct no_switch
{
    no_switch(Windows::Foundation::IAsyncAction const& async) : m_async(async)
    {
    }

    bool await_ready() const
    {
        return m_async.Status() == Windows::Foundation::AsyncStatus::Completed;
    }

    void await_suspend(std::experimental::coroutine_handle<> handle) const
    {
        m_async.Completed([handle](Windows::Foundation::IAsyncAction const& /* asyncInfo */, Windows::Foundation::AsyncStatus const& /* asyncStatus */)
        {
            handle();
        });
    }

    auto await_resume() const
    {
        return m_async.GetResults();
    }

private:
    Windows::Foundation::IAsyncAction const& m_async;
};
```

**no_switch** await アダプターの使用方法を理解するには、C++ コンパイラで、**await_ready**、**await_suspend**、**await_resume** と呼ばれる関数を探す `co_await` 式が検出されるタイミングをまず理解する必要があります。 C++/WinRT ライブラリでは、適切な動作が既定で得られるように、次のような関数が提供されています。

```cppwinrt
IAsyncAction async{ ProcessFeedAsync() };
co_await async;
```

**no_switch** await アダプターを使用するには、次のように、その `co_await` 式の型を **IAsyncXxx** から **no_switch** に切り替えるだけです。

```cppwinrt
IAsyncAction async{ ProcessFeedAsync() };
co_await static_cast<no_switch>(async);
```

次に、**IAsyncXxx** と一致する 3 つの **await_xxx** 関数を検索する代わりに、C++ コンパイラで **no_switch** と一致する関数が検索されます。

## <a name="canceling-an-asychronous-operation-and-cancellation-callbacks"></a>非同期操作の取り消しとキャンセル コールバック

非同期プログラミング向けの Windows ランタイムの機能を使用すると、実行中の非同期アクションまたは操作を取り消すことができます。 大きくなる可能性があるファイルのコレクションを取得する [**StorageFolder::GetFilesAsync**](/uwp/api/windows.storage.storagefolder.getfilesasync) を呼び出して、非同期操作の結果のオブジェクトをデータ メンバーに格納する例を次に示します。 ユーザーには、操作を取り消すオプションがあります。

```cppwinrt
// MainPage.xaml
...
<Button x:Name="workButton" Click="OnWork">Work</Button>
<Button x:Name="cancelButton" Click="OnCancel">Cancel</Button>
...

// MainPage.h
...
#include <winrt/Windows.Foundation.Collections.h>
#include <winrt/Windows.Storage.Search.h>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Foundation::Collections;
using namespace Windows::Storage;
using namespace Windows::Storage::Search;
using namespace Windows::UI::Xaml;
...
struct MainPage : MainPageT<MainPage>
{
    MainPage()
    {
        InitializeComponent();
    }

    IAsyncAction OnWork(IInspectable /* sender */, RoutedEventArgs /* args */)
    {
        workButton().Content(winrt::box_value(L"Working..."));

        // Enable the Pictures Library capability in the app manifest file.
        StorageFolder picturesLibrary{ KnownFolders::PicturesLibrary() };

        m_async = picturesLibrary.GetFilesAsync(CommonFileQuery::OrderByDate, 0, 1000);

        IVectorView<StorageFile> filesInFolder{ co_await m_async };

        workButton().Content(box_value(L"Done!"));

        // Process the files in some way.
    }

    void OnCancel(IInspectable const& /* sender */, RoutedEventArgs const& /* args */)
    {
        if (m_async.Status() != AsyncStatus::Completed)
        {
            m_async.Cancel();
            workButton().Content(winrt::box_value(L"Canceled"));
        }
    }

private:
    IAsyncOperation<::IVectorView<StorageFile>> m_async;
};
...
```

取り消しの実装側については、簡単な例から始めましょう。

```cppwinrt
// main.cpp
#include <iostream>
#include <winrt/Windows.Foundation.h>

using namespace winrt;
using namespace Windows::Foundation;
using namespace std::chrono_literals;

IAsyncAction ImplicitCancellationAsync()
{
    while (true)
    {
        std::cout << "ImplicitCancellationAsync: do some work for 1 second" << std::endl;
        co_await 1s;
    }
}

IAsyncAction MainCoroutineAsync()
{
    auto implicit_cancellation{ ImplicitCancellationAsync() };
    co_await 3s;
    implicit_cancellation.Cancel();
}

int main()
{
    winrt::init_apartment();
    MainCoroutineAsync().get();
}
```

上記の例を実行すると、**ImplicitCancellationAsync** によって 1 秒につき 1 つのメッセージが 3 秒間出力され、その後、取り消された結果として自動的に終了します。 これが機能するのは、`co_await` 式の検出時に、コルーチンでそれが取り消されているかどうかが確認されるからです。 取り消されている場合はすぐに出力し、されていない場合は通常どおり中断します。

当然ながら、取り消しはコルーチンが中断されている場合も発生する場合があります。 コルーチンは、再開時、または別の `co_await` がヒットしたときだけ取り消しを確認します。 問題は、取り消しへの応答で待機時間の粒度が荒すぎる可能性があることです。

そのため、もう 1 つのオプションは、コルーチン内から取り消しを明示的にポーリングすることです。 上記の例を次に示すコードで更新します。 この新しい例では、**ExplicitCancellationAsync** が [**winrt::get_cancellation_token**](/uwp/cpp-ref-for-winrt/get-cancellation-token) 関数で返されたオブジェクトを取得し、それを使用してコルーチンが取り消されているかどうかを定期的に確認します。 取り消されない限り、コルーチンは無限にループします。取り消されると、ループと関数は正常に終了します。 結果は前の例と同じですが、ここでは終了が制御され、明示的に発生します。

```cppwinrt
IAsyncAction ExplicitCancellationAsync()
{
    auto cancellation_token{ co_await winrt::get_cancellation_token() };

    while (!cancellation_token())
    {
        std::cout << "ExplicitCancellationAsync: do some work for 1 second" << std::endl;
        co_await 1s;
    }
}

IAsyncAction MainCoroutineAsync()
{
    auto explicit_cancellation{ ExplicitCancellationAsync() };
    co_await 3s;
    explicit_cancellation.Cancel();
}
...
```

**winrt::get_cancellation_token** を待機することで、コルーチンによって自動的に生成される **IAsyncAction** の情報を持つキャンセル トークンを取得します。 そのトークンに関数呼び出し演算子を使用して、キャンセル状態をクエリできます (実質的に取り消しのポーリング)。 いくつか計算処理がかかる操作を実行している場合、または大規模なコレクションを反復している場合、これは適切な手法です。

### <a name="register-a-cancellation-callback"></a>キャンセル コールバックを登録する

Windows ランタイムの取り消しは、他の非同期オブジェクトに自動的に流れません。 ただし、キャンセル コールバックを登録することができます (Windows SDK のバージョン 10.0.17763.0 (Windows 10、バージョン 1809) で導入されました)。 これは、取り消しを伝達できる先行フックで、既存の同時実行ライブラリとの統合を可能にします。

次のコード例では、**NestedCoroutineAsync** がこれを行いますが、特別な取り消しロジックはありません。 **CancellationPropagatorAsync** は、本質的に、入れ子になったコルーチンでのラッパーです。ラッパーは取り消しを先行的に転送します。

```cppwinrt
// main.cpp
#include <iostream>
#include <winrt/Windows.Foundation.h>

using namespace winrt;
using namespace Windows::Foundation;
using namespace std::chrono_literals;

IAsyncAction NestedCoroutineAsync()
{
    while (true)
    {
        std::cout << "NestedCoroutineAsync: do some work for 1 second" << std::endl;
        co_await 1s;
    }
}

IAsyncAction CancellationPropagatorAsync()
{
    auto cancellation_token{ co_await winrt::get_cancellation_token() };
    auto nested_coroutine{ NestedCoroutineAsync() };

    cancellation_token.callback([=]
    {
        nested_coroutine.Cancel();
    });

    co_await nested_coroutine;
}

IAsyncAction MainCoroutineAsync()
{
    auto cancellation_propagator{ CancellationPropagatorAsync() };
    co_await 3s;
    cancellation_propagator.Cancel();
}

int main()
{
    winrt::init_apartment();
    MainCoroutineAsync().get();
}
```

**CancellationPropagatorAsync** は、独自のキャンセル コールバックのラムダ関数を登録してから、入れ子になった作業が完了するまで待機 (中断) します。 **CancellationPropagatorAsync** は、取り消されると、取り消しを入れ子になったコルーチンに伝達します。 取り消しをポーリングする必要はありません。また取り消しは無限にブロックされません。 このメカニズムは、コルーチンまたは C++/WinRT に関する情報が何もない同時実行ライブラリとの相互運用に使用するのに十分な柔軟性があります。

## <a name="reporting-progress"></a>進行状況を報告する

コルーチンで [**IAsyncActionWithProgress**](/uwp/api/windows.foundation.iasyncactionwithprogress_tprogress_) または [**IAsyncOperationWithProgress**](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_) のいずれかを返す場合、[**winrt::get_progress_token**](/uwp/cpp-ref-for-winrt/get-progress-token) 関数によって返されるオブジェクトを取得し、それを使用して進行状況ハンドラーに進行状況を報告することができます。 次にコード例を示します。

```cppwinrt
// main.cpp
#include <iostream>
#include <winrt/Windows.Foundation.h>

using namespace winrt;
using namespace Windows::Foundation;
using namespace std::chrono_literals;

IAsyncOperationWithProgress<double, double> CalcPiTo5DPs()
{
    auto progress{ co_await winrt::get_progress_token() };

    co_await 1s;
    double pi_so_far{ 3.1 };
    progress(0.2);

    co_await 1s;
    pi_so_far += 4.e-2;
    progress(0.4);

    co_await 1s;
    pi_so_far += 1.e-3;
    progress(0.6);

    co_await 1s;
    pi_so_far += 5.e-4;
    progress(0.8);

    co_await 1s;
    pi_so_far += 9.e-5;
    progress(1.0);
    co_return pi_so_far;
}

IAsyncAction DoMath()
{
    auto async_op_with_progress{ CalcPiTo5DPs() };
    async_op_with_progress.Progress([](auto const& /* sender */, double progress)
    {
        std::wcout << L"CalcPiTo5DPs() reports progress: " << progress << std::endl;
    });
    double pi{ co_await async_op_with_progress };
    std::wcout << L"CalcPiTo5DPs() is complete !" << std::endl;
    std::wcout << L"Pi is approx.: " << pi << std::endl;
}

int main()
{
    winrt::init_apartment();
    DoMath().get();
}
```

> [!NOTE]
> 非同期アクションまたは操作に、複数の*完了ハンドラー*を実装するのは誤りです。 完了したイベントに対して 1 つのデリゲートを持つか、それを `co_await` することができます。 両方ある場合、2 つ目は失敗します。 同じ非同期オブジェクトには次の 2 種類の完了ハンドラーの両方ではなく、いずれか 1 つが適切です。

```cppwinrt
auto async_op_with_progress{ CalcPiTo5DPs() };
async_op_with_progress.Completed([](auto const& sender, AsyncStatus /* status */)
{
    double pi{ sender.GetResults() };
});
```

```cppwinrt
auto async_op_with_progress{ CalcPiTo5DPs() };
double pi{ co_await async_op_with_progress };
```

完了ハンドラーの詳細については、「[非同期アクションと非同期操作のデリゲート型](handle-events.md#delegate-types-for-asynchronous-actions-and-operations)」をご覧ください。

## <a name="fire-and-forget"></a>ファイア アンド フォーゲット

他の作業と同時に実行できるタスクがあり、そのタスクが完了するまで待機する必要も (それに依存する他の作業がない)、値が返される必要もない場合があります。 その場合、タスクを送信して忘れることができます。 これを行うには、戻り値の型が [**winrt::fire_and_forget**](/uwp/cpp-ref-for-winrt/fire-and-forget) (Windows ランタイムの非同期操作型のいずれか、または **concurrency::task** ではなく) のコルーチンを記述します。

```cppwinrt
// main.cpp
#include <winrt/Windows.Foundation.h>

using namespace winrt;
using namespace std::chrono_literals;

winrt::fire_and_forget CompleteInFiveSeconds()
{
    co_await 5s;
}

int main()
{
    winrt::init_apartment();
    CompleteInFiveSeconds();
    // Do other work here.
}
```

**winrt::fire_and_forget** は、イベント ハンドラーの中で非同期操作を実行する必要があるときのイベント ハンドラーの戻り値の型としても役に立ちます。 次に例を示します (「[C++/WinRT の強参照と弱参照](/windows/uwp/cpp-and-winrt-apis/weak-references#safely-accessing-the-this-pointer-in-a-class-member-coroutine)」もご覧ください)。

```cppwinrt
winrt::fire_and_forget MyClass::MyMediaBinder_OnBinding(MediaBinder const&, MediaBindingEventArgs args)
{
    auto lifetime{ get_strong() }; // Prevent *this* from prematurely being destructed.
    auto ensure_completion{ unique_deferral(args.GetDeferral()) }; // Take a deferral, and ensure that we complete it.

    auto file{ co_await StorageFile::GetFileFromApplicationUriAsync(Uri(L"ms-appx:///video_file.mp4")) };
    args.SetStorageFile(file);

    // The destructor of unique_deferral completes the deferral here.
}
```

最初の引数 (*sender*) は、使わないので名前を付けてありません。 そのため、参照のままにしても安全です。 ただし、*args* が値によって渡されていることに注意してください。 前の「[パラメーターの引き渡し](#parameter-passing)」セクションをご覧ください。

## <a name="awaiting-a-kernel-handle"></a>カーネル ハンドルの待機

C++/WinRT には **resume_on_signal** クラスが用意されています。このクラスを使用して、カーネル イベントが呼び出されるまで中断することができます。 `co_await resume_on_signal(h)` が返されるまではハンドルが有効なままであることを確認する必要があります。 これは **resume_on_signal** 自体で自動的に行うことはできません。最初の例のように、**resume_on_signal** が開始される前でもハンドルが失われている可能性があるためです。

```cppwinrt
IAsyncAction Async(HANDLE event)
{
    co_await DoWorkAsync();
    co_await resume_on_signal(event); // The incoming handle is not valid here.
}
```

受信 **HANDLE** は関数によって返されるまでのみ有効で、この関数 (コルーチン) によって最初の中断ポイント (この場合は最初の `co_await`) で返されます。 **DoWorkAsync** の待機中に制御が呼び出し元に返されて呼び出しフレームが範囲外になっているため、コルーチンの再開時にハンドルが有効になるかどうかがわからなくなります。

厳密に言えば、コルーチンは、必要に応じてそのパラメーターを値によって受け取ります (前述の「[パラメーターの引き渡し](#parameter-passing)」を参照)。 ただし、この場合は、そのガイダンスの*主旨* (単なる文字ではなく) に従うように、一歩先まで踏み込む必要があります。 ハンドルと共に、強参照 (つまり、所有権) を渡す必要があります。 以下にその方法を示します。

```cppwinrt
IAsyncAction Async(winrt::handle event)
{
    co_await DoWorkAsync();
    co_await resume_on_signal(event); // The incoming handle *is* not valid here.
}
```

[**winrt::handle**](/uwp/cpp-ref-for-winrt/handle) を値で渡すことで、所有権のセマンティクスが提供されます。これにより、カーネル ハンドルがコルーチンの有効期間にわたって有効なままになります。

このコルーチンを呼び出す方法を次に示します。

```cppwinrt
namespace
{
    winrt::handle duplicate(winrt::handle const& other, DWORD access)
    {
        winrt::handle result;
        if (other)
        {
            winrt::check_bool(::DuplicateHandle(::GetCurrentProcess(),
                other.get(), ::GetCurrentProcess(), result.put(), access, FALSE, 0));
        }
        return result;
    }

    winrt::handle make_manual_reset_event(bool initialState = false)
    {
        winrt::handle event{ ::CreateEvent(nullptr, true, initialState, nullptr) };
        winrt::check_bool(static_cast<bool>(event));
        return event;
    }
}

IAsyncAction SampleCaller()
{
    handle event{ make_manual_reset_event() };
    auto async{ Async(duplicate(event)) };

    ::SetEvent(event.get());
    event.close(); // Our handle is closed, but Async still has a valid handle.

    co_await async; // Will wake up when *event* is signaled.
}
```

## <a name="important-apis"></a>重要な API
* [concurrency::task クラス](/cpp/parallel/concrt/reference/task-class)
* [IAsyncAction インターフェイス](/uwp/api/windows.foundation.iasyncaction)
* [IAsyncActionWithProgress&lt;TProgress&gt; インターフェイス](/uwp/api/windows.foundation.iasyncactionwithprogress_tprogress_)
* [IAsyncOperation&lt;TResult&gt; インターフェイス](/uwp/api/windows.foundation.iasyncoperation_tresult_)
* [IAsyncOperationWithProgress&lt;TResult, TProgress&gt; インターフェイス](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_)
* [SyndicationClient::RetrieveFeedAsync メソッド](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)
* [SyndicationFeed クラス](/uwp/api/windows.web.syndication.syndicationfeed)
* [winrt::get_cancellation_token](/uwp/cpp-ref-for-winrt/get-cancellation-token)
* [winrt::get_progress_token](/uwp/cpp-ref-for-winrt/get-progress-token)
* [winrt::fire_and_forget](/uwp/cpp-ref-for-winrt/fire-and-forget)

## <a name="related-topics"></a>関連トピック
* [C++/WinRT でのデリゲートを使用したイベントの処理](handle-events.md)
* [標準的な C++ のデータ型と C++/WinRT](std-cpp-data-types.md)
