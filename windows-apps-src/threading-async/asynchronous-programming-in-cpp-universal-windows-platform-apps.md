---
ms.assetid: 34C00F9F-2196-46A3-A32F-0067AB48291B
description: ここでは、ppltasks.h の concurrency 名前空間で定義された task クラスを使って Visual C++ コンポーネント拡張機能 (C++/CX) の非同期メソッドを実装する際に推奨される方法について説明します。
title: C++ での非同期プログラミング
ms.date: 05/14/2018
ms.topic: article
keywords: Windows 10、UWP、スレッド、非同期、C++
ms.localizationpriority: medium
ms.openlocfilehash: 0e3810b25ac35cbf5e16f49a86affb4792089d1e
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89161796"
---
# <a name="asynchronous-programming-in-ccx"></a>C++/CX での非同期プログラミング
> [!NOTE]
> このトピックは、C++/CX アプリケーションの管理ができるようにすることを目的としています。 ただし、新しいアプリケーションには [C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) を使用することをお勧めします。 C++/WinRT は Windows ランタイム (WinRT) API の標準的な最新の C++17 言語プロジェクションで、ヘッダー ファイル ベースのライブラリとして実装され、最新の Windows API への最上位アクセス権を提供するように設計されています。

ここでは、ppltasks.h の `concurrency` 名前空間で定義された `task` クラスを使って Visual C++ コンポーネント拡張機能 (C++/CX) の非同期メソッドを実装する際に推奨される方法について説明します。

## <a name="universal-windows-platform-uwp-asynchronous-types"></a>ユニバーサル Windows プラットフォーム (UWP) の非同期型
ユニバーサル Windows プラットフォーム (UWP) には、非同期メソッドを呼び出すためのモデルが明確に定義されており、非同期メソッドを使う必要がある型があります。 UWP の非同期モデルについて詳しくない場合は、この記事の前に「[非同期プログラミング][AsyncProgramming]」をご覧ください。

C++ で非同期 Windows ランタイム Api を直接使用することもできますが、 [**タスククラス**][task-class] とそれに関連する型および関数を使用することをお勧めします。この方法は、 [**同時実行**][concurrencyNamespace] 名前空間に含まれ、で定義されてい `<ppltasks.h>` ます。 **concurrency::task** は汎用型のクラスですが、**/ZW** コンパイラ スイッチ (ユニバーサル Windows プラットフォーム (UWP) アプリとそのコンポーネントには必須) を使うと、task クラスで UWP の非同期型をカプセル化して次の処理を簡単に行うことができます。

-   複数の非同期操作や同期操作を 1 つのチェーンで連結する

-   タスク チェーンで例外を処理する

-   タスク チェーンで取り消しを実行する

-   各タスクを適切なスレッド コンテキストまたはスレッド アパートメントで実行する

ここでは、**task** クラスを UWP の非同期 API で使う方法に関する基本的なガイダンスを示しています。 **タスク**とそれに関連するメソッド (タスクの作成など) に関する詳細なドキュメントについては、「[タスクの並列化 (同時実行ランタイム)][taskParallelism]」を参照してください。 [** \_ **][createTask] 

## <a name="consuming-an-async-operation-by-using-a-task"></a>タスクを使った非同期操作の使用
次の例は、task クラスを使って、[**IAsyncOperation**][IAsyncOperation] インターフェイスを返す **async** メソッドを使う方法を示しています。この操作では値が生成されます。 基本的な手順は次のとおりです。

1.  `create_task` メソッドを呼び出し、**IAsyncOperation** オブジェクトに渡します。

2.  タスクのメンバー関数 [**task::then**][taskThen] を呼び出し、非同期操作の完了時に呼び出すラムダを指定します。

``` cpp
#include <ppltasks.h>
using namespace concurrency;
using namespace Windows::Devices::Enumeration;
...
void App::TestAsync()
{    
    //Call the *Async method that starts the operation.
    IAsyncOperation<DeviceInformationCollection^>^ deviceOp =
        DeviceInformation::FindAllAsync();

    // Explicit construction. (Not recommended)
    // Pass the IAsyncOperation to a task constructor.
    // task<DeviceInformationCollection^> deviceEnumTask(deviceOp);

    // Recommended:
    auto deviceEnumTask = create_task(deviceOp);

    // Call the task's .then member function, and provide
    // the lambda to be invoked when the async operation completes.
    deviceEnumTask.then( [this] (DeviceInformationCollection^ devices )
    {       
        for(int i = 0; i < devices->Size; i++)
        {
            DeviceInformation^ di = devices->GetAt(i);
            // Do something with di...          
        }       
    }); // end lambda
    // Continue doing work or return...
}
```

[**task::then**][taskThen] 関数で作成されて返されるタスクのことを*後続タスク*と呼びます。 ユーザーが指定するラムダの入力引数は (この例の場合)、タスク操作の完了時に生成される結果です。 この値は、**IAsyncOperation** インターフェイスを直接使う場合に [**IAsyncOperation::GetResults**](/uwp/api/Windows.Foundation.IAsyncOperation_TResult_#Windows_Foundation_IAsyncOperation_1_GetResults) を呼び出して取得する値と同じになります。

[**task::then**][taskThen] メソッドからは直ちに制御が返され、そのデリゲートは非同期作業が正常に完了するまで実行されません。 この例では、非同期操作で例外がスローされるか、取り消し要求によって非同期操作が取り消された状態で終わると、継続は実行されません。 前のタスクが取り消されるか失敗しても実行される継続を記述する方法については後で説明します。

タスクの変数はローカル スタックで宣言しますが、その有効期間は、操作が完了する前にメソッドから制御が返されても、すべての操作が完了してすべての参照がスコープ外になるまで削除されないように管理されます。

## <a name="creating-a-chain-of-tasks"></a>タスクのチェーンの作成
非同期プログラミングでは、前のタスクが完了した場合にのみ継続が実行されるように、操作のシーケンス (*タスク チェーン*) を定義するのが一般的です。 場合によっては、前のタスク (*先行タスク*) で生成された値を継続が入力として受け取ることもあります。 [**task::then**][taskThen] メソッドを使うと、直観的な方法で簡単にタスク チェーンを作成できます。このメソッドは、**task<T>** (**T** はラムダ関数の戻り値の型) を返します。 複数の継続を含めて 1 つのタスク チェーンを構成することができます。`myTask.then(…).then(…).then(…);`

タスク チェーンは、継続で新しい非同期操作を作成する場合に特に便利です。このようなタスクのことを非同期タスクと呼びます。 次の例は、2 つの継続を含むタスク チェーンを示しています。 既存のファイルへのハンドルを取得する最初のタスクの操作が完了すると、1 つ目の継続でそのファイルを削除する新しい非同期操作が始まります。 その操作が完了すると、2 つ目の継続が実行され、確認メッセージが出力されます。

``` cpp
#include <ppltasks.h>
using namespace concurrency;
...
void App::DeleteWithTasks(String^ fileName)
{    
    using namespace Windows::Storage;
    StorageFolder^ localFolder = ApplicationData::Current->LocalFolder;
    auto getFileTask = create_task(localFolder->GetFileAsync(fileName));

    getFileTask.then([](StorageFile^ storageFileSample) ->IAsyncAction^ {       
        return storageFileSample->DeleteAsync();
    }).then([](void) {
        OutputDebugString(L"File deleted.");
    });
}
```

この例で重要なポイントは次の 4 つです。

-   1 つ目の継続は、[**IAsyncAction^**][IAsyncAction] オブジェクトを **task<void>** に変換し、**task** を返します。

-   2 つ目の継続は、エラー処理を実行しないため、**task<void>** ではなく **void** を入力として受け取ります。 これは値ベースの継続です。

-   2 つ目の後続タスクは、[**DeleteAsync**][deleteAsync] 操作が完了するまで実行されません。

-   2 つ目の後続タスクは値ベースであるため、[**DeleteAsync**][deleteAsync] を呼び出して開始された操作から例外がスローされると、2 つ目の後続タスクは実行されません。

**メモ**   タスクチェーンの作成は、**タスク**クラスを使用して非同期操作を作成する方法の1つにすぎません。 Join 演算子と choice 演算子およびを使用して、操作を作成することもでき **&&** **||** ます。 詳しくは、「[タスクの並列処理 (同時実行ランタイム)][taskParallelism]」をご覧ください。

## <a name="lambda-function-return-types-and-task-return-types"></a>ラムダ関数の戻り値の型とタスクの戻り値の型
継続タスクでは、ラムダ関数の戻り値の型が **task** オブジェクトでラップされます。 ラムダが **double** を返す場合、継続タスクの型は **task<double>** になります。 ただし、タスク オブジェクトは、戻り値の型を必要以上に入れ子にしないように設計されています。 ラムダが **IAsyncOperation<SyndicationFeed^>^** を返す場合、継続は、**task<task<SyndicationFeed^>>** や **task<IAsyncOperation<SyndicationFeed^>^>^** ではなく、**task<SyndicationFeed^>** を返します。 *非同期ラップ解除*と呼ばれるこの処理により、さらに、継続内の非同期操作が完了しないと次の継続が呼び出されないようになります。

前の例では、ラムダが [**IAsyncInfo**][IAsyncInfo] オブジェクトを返しているのに、タスクは **task<void>** を返しています。 ラムダ関数とその外側のタスクの間で行われるこれらの型変換を次の表に示します。

| | |
|--------------------------------------------------------|---------------------|
| ラムダの戻り値の型                                     | `.then` の戻り値の型 |
| TResult                                                | タスク<TResult> |
| IAsyncOperation<TResult>^                        | タスク<TResult> |
| IAsyncOperationWithProgress<TResult, TProgress>^ | タスク<TResult> |
|IAsyncAction^                                           | タスク<void>    |
| IAsyncActionWithProgress<TProgress>^             |タスク<void>     |
| タスク<TResult>                                    |タスク<TResult>  |


## <a name="canceling-tasks"></a>タスクの取り消し
通常、非同期操作をユーザーが取り消せるようにすることをお勧めします。 また、場合によっては、プログラムを使ってタスク チェーンの外側から操作を取り消さなければならないこともあります。 各 \* **非同期**の戻り値の型には、 [**iasyncinfo**][IAsyncInfo]から継承する[**Cancel**][IAsyncInfoCancel]メソッドがありますが、外部のメソッドに公開するのは厄介です。 タスクチェーンでの取り消しをサポートするには、キャンセル [** \_ トークン \_ ソース**](/cpp/parallel/concrt/reference/cancellation-token-source-class) を使用して [**キャンセル \_ トークン**](/cpp/parallel/concrt/reference/cancellation-token-class)を作成し、そのトークンを初期タスクのコンストラクターに渡すことをお勧めします。 キャンセルトークンを使用して非同期タスクが作成され、キャンセル[** \_ トークン \_ ソース:: cancel**](/cpp/parallel/concrt/reference/cancellation-token-source-class?view=vs-2017)が呼び出された場合、タスクは自動的に**Iasync \* **操作に対して**cancel**を呼び出し、キャンセル要求をその継続チェーンに渡します。 この基本的な方法を示す疑似コードを次に示します。

``` cpp
//Class member:
cancellation_token_source m_fileTaskTokenSource;

// Cancel button event handler:
m_fileTaskTokenSource.cancel();

// task chain
auto getFileTask2 = create_task(documentsFolder->GetFileAsync(fileName),
                                m_fileTaskTokenSource.get_token());
//getFileTask2.then ...
```

タスクが取り消されると、タスク [**が \_ 取り消さ**][taskCanceled] れた例外がタスクチェーンの下位に伝達されます。 値ベースの後続タスクは実行されないだけですが、タスクベースの後続タスクでは、[**task::get**][taskGet] が呼び出されると例外がスローされます。 エラー処理の継続がある場合は、 **タスクの \_ キャンセル** された例外が明示的にキャッチされていることを確認してください。 (これは [**Platform::Exception**](/cpp/cppcx/platform-exception-class) から派生した例外ではありません)。

取り消しは連携して行います。 継続で、UWP メソッドを呼び出すだけでなく、時間のかかる作業を行うときは、キャンセル トークンの状態を定期的に確認し、取り消された場合は実行を中止する必要があります。 継続で割り当てられたすべてのリソースをクリーンアップした後、 [**[ \_ 現在の \_ タスクのキャンセル]**](/cpp/parallel/concrt/reference/concurrency-namespace-functions?view=vs-2017) を呼び出してそのタスクを取り消し、その後に続く値ベースの継続に取り消しを反映させます。 また、別の例として、[**FileSavePicker**](/uwp/api/Windows.Storage.Pickers.FileSavePicker) 操作の結果を表すタスク チェーンを作成するとします。 ユーザーが **[キャンセル]** ボタンをクリックした場合、[**IAsyncInfo::Cancel**][IAsyncInfoCancel] メソッドは呼び出されません。 代わりに、操作は完了しますが **nullptr** が返されます。 継続は入力パラメーターをテストし、入力が**nullptr**の場合は **[ \_ 現在の \_ タスクのキャンセル]** を呼び出すことができます。

詳しくは、「[PPL での取り消し](/cpp/parallel/concrt/cancellation-in-the-ppl)」をご覧ください。

## <a name="handling-errors-in-a-task-chain"></a>タスク チェーンでのエラーの処理
先行タスクの取り消しや例外のスローが行われても継続を実行する場合は、継続のラムダ関数への入力を **task<TResult>** または **task<void>** (先行タスクのラムダが [**IAsyncAction^**][IAsyncAction] を返す場合) として指定して、継続をタスクベースにします。

タスク チェーンでエラーや取り消しを処理するために、すべての継続をタスクベースにしたり、スローする可能性があるすべての操作を `try…catch` ブロック内に含めたりする必要はありません。 代わりに、タスクベースの継続をチェーンの最後に追加し、その継続ですべてのエラーを処理します。 すべての例外には、タスクの [** \_ 取り消し**][taskCanceled] 例外が含まれます。はタスクチェーンを下位に伝達し、値ベースの継続をバイパスして、エラー処理タスクベースの継続で処理できるようにします。 エラー処理を行うタスクベースの継続を使うように前の例を書き換えると次のようになります。

``` cpp
#include <ppltasks.h>
void App::DeleteWithTasksHandleErrors(String^ fileName)
{    
    using namespace Windows::Storage;
    using namespace concurrency;

    StorageFolder^ documentsFolder = KnownFolders::DocumentsLibrary;
    auto getFileTask = create_task(documentsFolder->GetFileAsync(fileName));

    getFileTask.then([](StorageFile^ storageFileSample)
    {       
        return storageFileSample->DeleteAsync();
    })

    .then([](task<void> t)
    {

        try
        {
            t.get();
            // .get() didn' t throw, so we succeeded.
            OutputDebugString(L"File deleted.");
        }
        catch (Platform::COMException^ e)
        {
            //Example output: The system cannot find the specified file.
            OutputDebugString(e->Message->Data());
        }

    });
}
```

タスクベースの後続タスクで、メンバー関数 [**task::get**][taskGet] を呼び出してタスクの結果を取得しています。 **task::get** は、結果を生成しない [**IAsyncAction**][IAsyncAction] 操作の場合でも呼び出す必要があります。これは、**task::get** ではタスクに送られた例外も取得するためです。 入力タスクに例外が格納されている場合、**task::get** の呼び出しでその例外がスローされます。 タスク **:: get**を呼び出さない場合、またはチェーンの最後でタスクベースの継続を使用しない場合、またはスローされた例外の種類をキャッチしない場合は、タスクへのすべての参照が削除されたときに **監視され \_ task \_ 例外** がスローされます。

キャッチする例外は処理できるものだけにしてください。 回復できないエラーがアプリで発生した場合は、不明な状態のままアプリの実行を続けるのではなく、アプリをクラッシュさせることをお勧めします。 また、一般に、 **監視され \_ タスクの \_ 例外** 自体をキャッチしません。 この例外は、主に診断を目的としたものです。 **監視され \_ task \_ 例外**がスローされた場合は、通常、コード内のバグを示します。 原因のほとんどは、処理が必要な例外があること、またはコード内の他のエラーが原因で回復できない例外があることのどちらかです。

## <a name="managing-the-thread-context"></a>スレッド コンテキストの管理
UWP アプリの UI は、シングルスレッド アパートメント (STA) で実行されます。 ラムダが [**IAsyncAction**][IAsyncAction] または [**IAsyncOperation**][IAsyncOperation] を返すタスクは、アパートメントを認識します。 タスクが STA で作成されている場合、特に指定しない限り、その継続もすべて STA で実行されます。 つまり、タスク チェーン全体が親タスクからアパートメントの認識を継承します。 この動作により、STA からしかアクセスできない UI コントロールの操作が簡単になります。

たとえば、UWP アプリの [**ListBox**](/uwp/api/Windows.UI.Xaml.Controls.ListBox) コントロールを設定するとき、XAML ページを表す任意のクラスのメンバー関数で [**task::then**][taskThen] メソッド内から設定できるため、[**Dispatcher**](/uwp/api/Windows.UI.Core.CoreDispatcher) オブジェクトを使う必要がありません。

``` cpp
#include <ppltasks.h>
void App::SetFeedText()
{    
    using namespace Windows::Web::Syndication;
    using namespace concurrency;
    String^ url = "http://windowsteamblog.com/windows_phone/b/wmdev/atom.aspx";
    SyndicationClient^ client = ref new SyndicationClient();
    auto feedOp = client->RetrieveFeedAsync(ref new Uri(url));

    create_task(feedOp).then([this]  (SyndicationFeed^ feed)
    {
        m_TextBlock1->Text = feed->Title->Text;
    });
}
```

[**IAsyncAction**][IAsyncAction] または [**IAsyncOperation**][IAsyncOperation] を返さないタスクは、アパートメントを認識しません。これらのタスクの後続タスクは、既定では、利用可能な最初のバックグラウンド スレッドで実行されます。

Task [**::**][taskThen] のオーバーロードを使用して、タスクの既定のスレッドコンテキストをオーバーライドできます。これは、タスクの [** \_ 継続 \_ コンテキスト**](/cpp/parallel/concrt/reference/task-continuation-context-class)を取得します。 たとえば、状況によっては、アパートメントを認識するタスクの継続をバックグラウンド スレッドでスケジュールする方が適している場合もあります。 このような場合は、タスクの [** \_ 継続 \_ コンテキスト:: \_ 任意を使用**][useArbitrary] して、マルチスレッドアパートメントで次に使用可能なスレッドでタスクの作業をスケジュールすることができます。 これにより、継続の作業を UI スレッドで発生する他の作業と同期する必要がないため、継続のパフォーマンスが向上します。

次の例では、 [**タスク \_ 継続 \_ コンテキスト:: \_ 任意**][useArbitrary] のオプションを指定すると便利です。また、既定の継続コンテキストがスレッドセーフでないコレクションでの同時操作の同期にどのように役立つかも示しています。 このコードでは、RSS フィードの URL の一覧をループ処理し、各 URL について、非同期操作を開始してフィード データを取得しています。 フィードを取得する順序は制御できませんが、ここでは問題ありません。 [**RetrieveFeedAsync**](/uwp/api/windows.web.syndication.isyndicationclient.retrievefeedasync) 操作が完了するたびに、1 つ目の継続が [**SyndicationFeed^**](/uwp/api/Windows.Web.Syndication.SyndicationFeed) オブジェクトを受け取り、それを使ってアプリで定義されている `FeedData^` オブジェクトを初期化します。 これらの各操作は互いに独立しているため、 **タスク \_ 継続 \_ コンテキスト:: \_ 任意** の継続コンテキストを指定することで、処理速度を上げることができます。 ただし、それぞれの `FeedData` オブジェクトを初期化した後に、そのオブジェクトを [**Vector**](/cpp/cppcx/platform-collections-vector-class) に追加する必要があり、スレッド セーフなコレクションではありません。 したがって、継続を作成し、 [**タスクの \_ 継続 \_ コンテキスト:: use \_ current**](/cpp/parallel/concrt/reference/task-continuation-context-class?view=vs-2017) を指定して、 [**追加**](/uwp/api/windows.foundation.collections.ivector_t_.append) するすべての呼び出しが同じアプリケーションのシングルスレッドアパートメント (ASTA) コンテキストで発生するようにします。 [**タスクの \_ 継続 \_ コンテキスト:: use \_ default**](/cpp/parallel/concrt/reference/task-continuation-context-class?view=vs-2017)は既定のコンテキストであるため、明示的に指定する必要はありませんが、わかりやすくするためにここで行います。

``` cpp
#include <ppltasks.h>
void App::InitDataSource(Vector<Object^>^ feedList, vector<wstring> urls)
{
                using namespace concurrency;
    SyndicationClient^ client = ref new SyndicationClient();

    std::for_each(std::begin(urls), std::end(urls), [=,this] (std::wstring url)
    {
        // Create the async operation. feedOp is an
        // IAsyncOperationWithProgress<SyndicationFeed^, RetrievalProgress>^
        // but we don't handle progress in this example.

        auto feedUri = ref new Uri(ref new String(url.c_str()));
        auto feedOp = client->RetrieveFeedAsync(feedUri);

        // Create the task object and pass it the async operation.
        // SyndicationFeed^ is the type of the return value
        // that the feedOp operation will eventually produce.

        // Then, initialize a FeedData object by using the feed info. Each
        // operation is independent and does not have to happen on the
        // UI thread. Therefore, we specify use_arbitrary.
        create_task(feedOp).then([this]  (SyndicationFeed^ feed) -> FeedData^
        {
            return GetFeedData(feed);
        }, task_continuation_context::use_arbitrary())

        // Append the initialized FeedData object to the list
        // that is the data source for the items collection.
        // This all has to happen on the same thread.
        // By using the use_default context, we can append
        // safely to the Vector without taking an explicit lock.
        .then([feedList] (FeedData^ fd)
        {
            feedList->Append(fd);
            OutputDebugString(fd->Title->Data());
        }, task_continuation_context::use_default())

        // The last continuation serves as an error handler. The
        // call to get() will surface any exceptions that were raised
        // at any point in the task chain.
        .then( [this] (task<void> t)
        {
            try
            {
                t.get();
            }
            catch(Platform::InvalidArgumentException^ e)
            {
                //TODO handle error.
                OutputDebugString(e->Message->Data());
            }
        }); //end task chain

    }); //end std::for_each
}
```

継続内で作成される入れ子になった新しいタスクは、最初のタスクのアパートメントの認識を継承しません。

## <a name="handing-progress-updates"></a>進行状況の更新の処理
[**IAsyncOperationWithProgress**](/uwp/api/Windows.Foundation.IAsyncOperationWithProgress_TResult_TProgress_) または [**IAsyncActionWithProgress**](/uwp/api/Windows.Foundation.IAsyncActionWithProgress_TProgress_) をサポートするメソッドは、実行中の操作が完了するまでの間、定期的に進行状況の更新を提供します。 この進行状況の報告は、タスクや継続とは別に独立して処理されます。 オブジェクトの [**Progress**](/uwp/api/Windows.Foundation.IAsyncOperationWithProgress_TResult_TProgress_) プロパティのデリゲートを指定するだけでかまいません。 このデリゲートの一般的な用途は、UI の進行状況バーを更新することです。

## <a name="related-topics"></a>関連トピック
* [UWP アプリ用に C++/CX で非同期操作を作成](/cpp/parallel/concrt/creating-asynchronous-operations-in-cpp-for-windows-store-apps)
* [Visual C++ 言語のリファレンス](/cpp/cppcx/visual-c-language-reference-c-cx)
* [非同期の概要][AsyncProgramming]
* [タスクの並列化 (同時実行ランタイム)][taskParallelism]
* [concurrency::task](/cpp/parallel/concrt/reference/task-class)

<!-- LINKS -->
[AsyncProgramming]: ./asynchronous-programming-universal-windows-platform-apps.md "AsyncProgramming"
[concurrencyNamespace]: /cpp/parallel/concrt/reference/concurrency-namespace "Concurrency 名前空間"
[createTask]: /cpp/parallel/concrt/reference/concurrency-namespace-functions#create_task "CreateTask"
[deleteAsync]: /uwp/api/Windows.Storage.StorageFile "DeleteAsync"
[IAsyncAction]: /uwp/api/Windows.Foundation.IAsyncAction "IAsyncAction"
[IAsyncOperation]: /uwp/api/windows.foundation.iasyncoperation-1 "IAsyncOperation"
[IAsyncInfo]: /uwp/api/Windows.Foundation.IAsyncInfo "IAsyncInfo"
[IAsyncInfoCancel]: /uwp/api/Windows.Foundation.IAsyncInfo "IAsyncInfoCancel"
[taskCanceled]: /cpp/parallel/concrt/reference/task-canceled-class "TaskCancelled"
[task-class]: /cpp/parallel/concrt/reference/task-class#get "Task クラス"
[taskGet]: /cpp/parallel/concrt/reference/task-class "TaskGet"
[taskParallelism]: /cpp/parallel/concrt/task-parallelism-concurrency-runtime "タスクの並列処理"
[taskThen]: /cpp/parallel/concrt/reference/task-class#then "TaskThen"
[useArbitrary]: /cpp/parallel/concrt/reference/task-continuation-context-class "Us、"