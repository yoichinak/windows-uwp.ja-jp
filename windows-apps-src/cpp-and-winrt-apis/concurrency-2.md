---
description: C++/WinRT での同時実行操作と非同期操作を使用したより高度なシナリオを示します。
title: C++/WinRT でのより高度な同時実行操作と非同期操作
ms.date: 07/23/2019
ms.topic: article
keywords: Windows 10、uwp、標準、c++、cpp、winrt、プロジェクション、同時実行、非同期、非同期、非同期操作
ms.localizationpriority: medium
ms.openlocfilehash: e916465d664b5658eeb155874dfa00795a772622
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89170396"
---
# <a name="more-advanced-concurrency-and-asynchrony-with-cwinrt"></a>C++/WinRT でのより高度な同時実行操作と非同期操作

このトピックでは、C++/WinRT での同時実行操作と非同期操作を使用したより高度なシナリオについて説明します。

この主題の概要について、[同時実行操作と非同期操作](concurrency.md)に関する記事を先にお読みください。

## <a name="offloading-work-onto-the-windows-thread-pool"></a>Windows スレッド プールへの処理のオフロード

コルーチンは、関数が呼び出し元に実行を返すまで呼び出し元がブロックされるという点で、他と同様の関数です。 コルーチンが最初に返すのは、最初の `co_await`、`co_return`、または `co_yield` です。

そのため、コルーチンで計算処理にかかる処理を行う前に、呼び出し元がブロックされないように呼び出し元に実行を返す必要があります (つまり、一時停止ポイントを導入します)。 その他の操作を `co_await` することでこれをまだ行っていない場合は、[**winrt::resume_background**](/uwp/cpp-ref-for-winrt/resume-background) 関数を `co_await` できます。 これにより、呼び出し元に制御が返され、スレッド プールのスレッドですぐに実行が再開されます。

実装で使用されているスレッド プールは低レベルの [Windows スレッド プール](/windows/desktop/ProcThread/thread-pool-api)であるため、最適に効率化されます。

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

    // Switch to the foreground thread associated with textblock.
    co_await winrt::resume_foreground(textblock.Dispatcher());

    textblock.Text(L"Done!"); // Guaranteed to work.
}
```

**winrt::resume_foreground** 関数は、オプションの priority パラメーターを受け取ります。 このパラメーターを使用する場合、上記のパターンは適切です。 使用しない場合、`co_await winrt::resume_foreground(someDispatcherObject);` を簡略化して `co_await someDispatcherObject;` にすることができます。

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
using namespace std::chrono_literals;
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

## <a name="a-deeper-dive-into-winrtresume_foreground"></a>**winrt::resume_foreground** の詳細

[C++/WinRT 2.0](/windows/uwp/cpp-and-winrt-apis/newsnews#news-and-changes-in-cwinrt-20) では、[**winrt::resume_foreground**](/uwp/cpp-ref-for-winrt/resume-foreground) 関数は、ディスパッチャー スレッドから呼び出された場合でも中断されます (前のバージョンでは、既にディスパッチャー スレッド上に存在していない場合のみ中断されるため、一部のシナリオではデッドロックが発生する可能性がありました)。

現在の動作は、スタックのアンワインドと再キュー処理が発生することに依存できることを意味します。これは、システムの安定性を確保するために (特に低レベルのシステム コードでは) 重要です。 前の「[スレッドの関係を考慮したプログラミング](#programming-with-thread-affinity-in-mind)」セクションに示されている最後のコードでは、バックグラウンド スレッドでの複雑な計算の実行と、その後のユーザー インターフェイス (UI) を更新するための適切な UI スレッドへの切り替えが示されています。

**winrt::resume_foreground** が内部的にどのようになっているかを次に示します。

```cppwinrt
auto resume_foreground(...) noexcept
{
    struct awaitable
    {
        bool await_ready() const
        {
            return false; // Queue without waiting.
            // return m_dispatcher.HasThreadAccess(); // The C++/WinRT 1.0 implementation.
        }
        void await_resume() const {}
        void await_suspend(coroutine_handle<> handle) const { ... }
    };
    return awaitable{ ... };
};
```

この現在の動作と前の動作は、Win32 アプリケーション開発での [**PostMessage**](/windows/win32/api/winuser/nf-winuser-postmessagew) と [**SendMessage**](/windows/win32/api/winuser/nf-winuser-sendmessage) 間の違いに似ています。 **PostMessage** では、作業がキューに配置された後、作業が完了するのを待たずにスタックがアンワインドされます。 スタックのアンワインドが不可欠である可能性があります。

また、**winrt::resume_foreground** 関数では、最初は ([**CoreWindow**](/uwp/api/windows.ui.core.corewindow) に関連付けられた) [**CoreDispatcher**](/uwp/api/windows.ui.core.coredispatcher) のみがサポートされていましたが、これは Windows 10 より前に導入されています。 その後、もっと柔軟で効率的なディスパッチャーである [**DispatcherQueue**](/uwp/api/windows.system.dispatcherqueue) が導入されています。 自分の目的に合う **DispatcherQueue** を作成できます。 次の単純なコンソール アプリケーションを検討します。

```cppwinrt
using namespace Windows::System;

winrt::fire_and_forget RunAsync(DispatcherQueue queue);
 
int main()
{
    auto controller{ DispatcherQueueController::CreateOnDedicatedThread() };
    RunAsync(controller.DispatcherQueue());
    getchar();
}
```

上の例では、プライベート スレッドに (コントローラー内に含まれる) キューが作成された後、コントローラーがコルーチンに渡されます。 コルーチンでは、キューを使用して、プライベート スレッドで待機 (中断と再開) できます。 **DispatcherQueue** の他の一般的な用途は、従来のデスクトップまたは Win32 アプリの現在の UI スレッドにキューを作成することです。

```cppwinrt
DispatcherQueueController CreateDispatcherQueueController()
{
    DispatcherQueueOptions options
    {
        sizeof(DispatcherQueueOptions),
        DQTYPE_THREAD_CURRENT,
        DQTAT_COM_STA
    };
 
    ABI::Windows::System::IDispatcherQueueController* ptr{};
    winrt::check_hresult(CreateDispatcherQueueController(options, &ptr));
    return { ptr, take_ownership_from_abi };
}
```

これは、Win32 関数を呼び出して C++/WinRT プロジェクトに組み込む方法を示しています。ここで実行されているのは、Win32 スタイルの [**CreateDispatcherQueueController**](/windows/win32/api/dispatcherqueue/nf-dispatcherqueue-createdispatcherqueuecontroller) 関数を呼び出してコントローラーを作成した後、作成されたキュー コントローラーの所有権を WinRT オブジェクトとして呼び出し元に転送することだけです。 これは、まさに既存の Petzold スタイルの Win32 デスクトップ アプリケーションで効率的でシームレスなキュー処理をサポートできる方法でもあります。

```cppwinrt
winrt::fire_and_forget RunAsync(DispatcherQueue queue);
 
int main()
{
    Window window;
    auto controller{ CreateDispatcherQueueController() };
    RunAsync(controller.DispatcherQueue());
    MSG message;
 
    while (GetMessage(&message, nullptr, 0, 0))
    {
        DispatchMessage(&message);
    }
}
```

上記では、単純な **main** 関数によるウィンドウの作成から始まります。 これにより、ウィンドウ クラスが登録され、[**CreateWindow**](/windows/win32/api/winuser/nf-winuser-createwindoww) が呼び出されて最上位のデスクトップ ウィンドウが作成されることを想像できます。 その後、**CreateDispatcherQueueController** 関数が呼び出されてキュー コントローラーが作成され、このコントローラーによって所有されるディスパッチャー キューを使用するコルーチンが呼び出されます。 次に、このスレッドでコルーチンの再開が自然に発生する従来のメッセージ ポンプに入ります。 この実行によって、アプリケーション内で、非同期またはメッセージベースのワークフローのための洗練されたコルーチンの世界に戻ることができます。

```cppwinrt
winrt::fire_and_forget RunAsync(DispatcherQueue queue)
{
    ... // Begin on the calling thread...
 
    co_await winrt::resume_foreground(queue);
 
    ... // ...resume on the dispatcher thread.
}
```

**winrt::resume_foreground** の呼び出しにより常に*キュー処理*され、その後、スタックがアンワインドされます。 必要に応じて、再開の優先順位を設定することもできます。

```cppwinrt
winrt::fire_and_forget RunAsync(DispatcherQueue queue)
{
    ...
 
    co_await winrt::resume_foreground(queue, DispatcherQueuePriority::High);
 
    ...
}
```

または、既定のキュー処理の順序を使用します。

```cppwinrt
winrt::fire_and_forget RunAsync(DispatcherQueue queue)
{
    ...
 
    co_await queue;
 
    ...
}
```

または、次のようにキューのシャットダウンを検出して、適切に処理します。

```cppwinrt
winrt::fire_and_forget RunAsync(DispatcherQueue queue)
{
    ...
 
    if (co_await queue)
    {
        ... // Resume on dispatcher thread.
    }
    else
    {
        ... // Still on calling thread.
    }
}
```

`co_await` 式では、ディスパッチャー スレッドで再開が発生することを示す `true` が返されます。 つまり、このキュー処理は成功しました。 逆に、キューのコントローラーがシャットダウンされ、キュー要求を処理しなくなっているために、実行が呼び出し元のスレッドにとどまっている場合は、そのことを示す `false` が返されます。

このように、C++/WinRT とコルーチンを組み合わせると、多大なパワーを簡単に手に入れられ、これは古い Petzold スタイルのデスクトップ アプリケーション開発を行うときに特に役立ちます。

## <a name="canceling-an-asynchronous-operation-and-cancellation-callbacks"></a>非同期操作の取り消しとキャンセル コールバック

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

Windows ランタイムの取り消しは、他の非同期オブジェクトに自動的に流れません。 ただし、キャンセル コールバックを登録することができます (Windows SDK のバージョン 10.0.17763.0 (Windows 10、バージョン 1809) で導入されました)。 これは、取り消しを伝達できる先行フックで、既存の同時開催ライブラリとの統合を可能にします。

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

**CancellationPropagatorAsync** は、独自のキャンセル コールバックのラムダ関数を登録してから、入れ子になった作業が完了するまで待機 (中断) します。 **CancellationPropagatorAsync** は、取り消されると、取り消しを入れ子になったコルーチンに伝達します。 取り消しをポーリングする必要はありません。また取り消しは無限にブロックされません。 このメカニズムは、コルーチンまたは C++/WinRT に関する情報が何もない同時開催ライブラリとの相互運用に使用するのに十分な柔軟性があります。

## <a name="reporting-progress"></a>進行状況を報告する

コルーチンで [**IAsyncActionWithProgress**](/uwp/api/windows.foundation.iasyncactionwithprogress-1) または [**IAsyncOperationWithProgress**](/uwp/api/windows.foundation.iasyncoperationwithprogress-2) のいずれかを返す場合、[**winrt::get_progress_token**](/uwp/cpp-ref-for-winrt/get-progress-token) 関数によって返されるオブジェクトを取得し、それを使用して進行状況ハンドラーに進行状況を報告することができます。 次にコード例を示します。

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

**winrt::fire_and_forget** は、イベント ハンドラーの中で非同期操作を実行する必要があるときのイベント ハンドラーの戻り値の型としても役に立ちます。 次に例を示します (「[C++/WinRT の強参照と弱参照](./weak-references.md#safely-accessing-the-this-pointer-in-a-class-member-coroutine)」もご覧ください)。

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

最初の引数 (*sender*) は、使わないので名前を付けてありません。 そのため、参照のままにしても安全です。 ただし、*args* が値によって渡されていることに注意してください。 前の「[パラメーターの引き渡し](concurrency.md#parameter-passing)」セクションをご覧ください。

## <a name="awaiting-a-kernel-handle"></a>カーネル ハンドルの待機

C++/WinRT には [**winrt::resume_on_signal**](/uwp/cpp-ref-for-winrt/resume-on-signal) 関数が用意されています。これを使用して、カーネル イベントが通知されるまで中断することができます。 `co_await resume_on_signal(h)` が返されるまではハンドルが有効なままであることを確認する必要があります。 これは **resume_on_signal** 自体で自動的に行うことはできません。最初の例のように、**resume_on_signal** が開始される前でもハンドルが失われている可能性があるためです。

```cppwinrt
IAsyncAction Async(HANDLE event)
{
    co_await DoWorkAsync();
    co_await resume_on_signal(event); // The incoming handle is not valid here.
}
```

受信 **HANDLE** は関数によって返されるまでのみ有効で、この関数 (コルーチン) によって最初の中断ポイント (この場合は最初の `co_await`) で返されます。 **DoWorkAsync** の待機中に制御が呼び出し元に返されて呼び出しフレームが範囲外になっているため、コルーチンの再開時にハンドルが有効になるかどうかがわからなくなります。

厳密に言えば、コルーチンは、必要に応じてそのパラメーターを値によって受け取ります (前述の「[パラメーターの引き渡し](concurrency.md#parameter-passing)」を参照)。 ただし、この場合は、そのガイダンスの*主旨* (単なる文字ではなく) に従うように、一歩先まで踏み込む必要があります。 ハンドルと共に、強参照 (つまり、所有権) を渡す必要があります。 以下にその方法を示します。

```cppwinrt
IAsyncAction Async(winrt::handle event)
{
    co_await DoWorkAsync();
    co_await resume_on_signal(event); // The incoming handle *is* valid here.
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

この例のように、**resume_on_signal** にタイムアウト値を渡すことができます。

```cppwinrt
winrt::handle event = ...

if (co_await winrt::resume_on_signal(event.get(), std::literals::2s))
{
    puts("signaled");
}
else
{
    puts("timed out");
}
```

## <a name="asynchronous-timeouts-made-easy"></a>非同期タイムアウトを簡単に

C++/WinRT では、C++ コルーチンに対応するために多くの労力が費やされています。 同時実行コードの記述に対するそれらの効果は革新的です。 このセクションでは、非同期性の詳細は重要ではなく、必要なのはその場ですぐに結果を取得することだけであるケースについて説明します。 そのため、C++/WinRT での [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction) Windows ランタイムの非同期操作インターフェイスの実装には、**std::future** で提供されているものに似た **get** 関数があります。

```cppwinrt
using namespace winrt::Windows::Foundation;
int main()
{
    IAsyncAction async = ...
    async.get();
    puts("Done!");
}
```

**get** 関数は無制限でブロックしますが、非同期オブジェクトは完了します。 非同期オブジェクトの有効期間は非常に短くなる傾向があるため、多くの場合これが必要なもののすべてです。

ただし、それだけでは不十分であり、多少の時間が経過した後で待機を破棄しなければならない場合があります。 そのコードは、Windows ランタイムによって提供されている構成要素を使用して常に記述できます。 ただし、現在は、C++/WinRT によって提供されている **wait_for** 関数によって、作業ははるかに簡単になっています。 それは **IAsyncAction** でも実装され、これも **std::future** によって提供されるものに似ています。

```cppwinrt
using namespace std::chrono_literals;
int main()
{
    IAsyncAction async = ...
 
    if (async.wait_for(5s) == AsyncStatus::Completed)
    {
        puts("done");
    }
}
```

> [!NOTE]
> **wait_for** ではインターフェイスで **std::chrono::duration** が使用されますが、それは **std::chrono::duration** で提供する値 (約 49.7 日) より小さい範囲に制限されています。

次の例の **wait_for** では、約 5 秒間待機した後、完了をチェックします。 比較が好ましければ、非同期オブジェクトが正常に完了したことがわかり、操作は完了します。 何らかの結果を待っている場合は、結果を取得する **get** 関数の呼び出しを続けて実行できます。

```cppwinrt
int main()
{
    IAsyncOperation<int> async = ...
 
    if (async.wait_for(5s) == AsyncStatus::Completed)
    {
        printf("result %d\n", async.get());
    }
}
```

非同期オブジェクトはその時点までに完了しているため、**get** ですぐに結果が返され、それ以上待機する必要はありません。 ご覧のとおり、**wait_for** では、非同期オブジェクトの状態が返されます。 したがって、それを使用して、次のような細かい制御を実行できます。

```cppwinrt
switch (async.wait_for(5s))
{
case AsyncStatus::Completed:
    printf("result %d\n", async.get());
    break;
case AsyncStatus::Canceled:
    puts("canceled");
    break;
case AsyncStatus::Error:
    puts("failed");
    break;
case AsyncStatus::Started:
    puts("still running");
    break;
}
```

- **AsyncStatus::Completed** は非同期オブジェクトが正常に完了したことを意味し、**get** 関数を呼び出して結果を取得できることを覚えておいてください。
- **AsyncStatus::Canceled** は、非同期オブジェクトが取り消されたことを意味します。 取り消しは、通常は呼び出し元によって要求されるため、この状態を処理することはめったにありません。 通常、取り消された非同期オブジェクトは破棄されるだけです。
- **Asyncstatus:: Error** は、非同期オブジェクトが何らかの方法で失敗したことを意味します。 必要な場合は、例外を再スローするために **get** できます。
- **AsyncStatus::Started** は、非同期オブジェクトがまだ実行されていることを意味します。 Windows ランタイムの非同期パターンでは、複数の待機も待機処理も許可されません。 これは、ループ内で **wait_for** を呼び出せないことを意味します。 待機が有効にタイムアウトしている場合は、いくつかの選択肢があります。 オブジェクトを破棄するか、**get** を呼び出して結果を取得する前にその状態をポーリングできます。 ただし、最善なのは、この時点で単にオブジェクトを破棄することです。

## <a name="returning-an-array-asynchronously"></a>配列を非同期的に返す

次に示すのは、*エラー MIDL2025: [msg]syntax error [context]: expecting > or, near "["* を生成する [MIDL 3.0](/uwp/midl-3/) の例です。

```idl
Windows.Foundation.IAsyncOperation<Int32[]> RetrieveArrayAsync();
```

これは、パラメーター化されたインターフェイスのパラメーター型引数として配列を使用することが無効であるためです。 そのため、ランタイム クラスのメソッドから非同期的に配列を渡すことを目的とする明確さの低い方法が必要になります。 

配列は、[PropertyValue](/uwp/api/windows.foundation.propertyvalue) オブジェクトにボックス化して返すことができます。 その後、呼び出し元のコードでボックス化を解除します。 コード例を次に示します。これを試すには、**Windows ランタイム コンポーネント (C++/WinRT)** プロジェクトに **SampleComponent** ランタイム クラスを追加し、それを (たとえば) **Core アプリ (C++/WinRT)** プロジェクトから利用します。

```cppwinrt
// SampleComponent.idl
namespace MyComponentProject
{
    runtimeclass SampleComponent
    {
        Windows.Foundation.IAsyncOperation<IInspectable> RetrieveCollectionAsync();
    };
}

// SampleComponent.h
...
struct SampleComponent : SampleComponentT<SampleComponent>
{
    ...
    Windows::Foundation::IAsyncOperation<Windows::Foundation::IInspectable> RetrieveCollectionAsync()
    {
        co_return Windows::Foundation::PropertyValue::CreateInt32Array({ 99, 101 }); // Box an array into a PropertyValue.
    }
}
...

// SampleCoreApp.cpp
...
MyComponentProject::SampleComponent m_sample_component;
...
auto boxed_array{ co_await m_sample_component.RetrieveCollectionAsync() };
auto property_value{ boxed_array.as<winrt::Windows::Foundation::IPropertyValue>() };
winrt::com_array<int32_t> my_array;
property_value.GetInt32Array(my_array); // Unbox back into an array.
...
```

## <a name="important-apis"></a>重要な API
* [IAsyncAction インターフェイス](/uwp/api/windows.foundation.iasyncaction)
* [IAsyncActionWithProgress&lt;TProgress&gt; インターフェイス](/uwp/api/windows.foundation.iasyncactionwithprogress-1)
* [IAsyncOperation&lt;TResult&gt; インターフェイス](/uwp/api/windows.foundation.iasyncoperation-1)
* [IAsyncOperationWithProgress&lt;TResult, TProgress&gt; インターフェイス](/uwp/api/windows.foundation.iasyncoperationwithprogress-2)
* [SyndicationClient::RetrieveFeedAsync メソッド](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)
* [winrt::fire_and_forget](/uwp/cpp-ref-for-winrt/fire-and-forget)
* [winrt::get_cancellation_token](/uwp/cpp-ref-for-winrt/get-cancellation-token)
* [winrt::get_progress_token](/uwp/cpp-ref-for-winrt/get-progress-token)
* [winrt::resume_foreground](/uwp/cpp-ref-for-winrt/resume-foreground)

## <a name="related-topics"></a>関連トピック
* [同時開催操作と非同期操作](concurrency.md)
* [C++/WinRT でのデリゲートを使用したイベントの処理](handle-events.md)