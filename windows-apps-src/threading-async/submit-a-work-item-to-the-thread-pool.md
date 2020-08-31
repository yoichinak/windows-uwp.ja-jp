---
ms.assetid: E2A1200C-9583-40FA-AE4D-C9E6F6C32BCF
title: スレッド プールへの作業項目の送信
description: スレッド プールに作業項目を送信することで独立したスレッドで作業を実行する方法について説明します。
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, スレッド, スレッド プール
ms.localizationpriority: medium
ms.openlocfilehash: 3576f907e4ab601013d22fe9ae7697e0ec523ce4
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155206"
---
# <a name="submit-a-work-item-to-the-thread-pool"></a>スレッド プールへの作業項目の送信

\[ Windows 10 の UWP アプリ用に更新されました。 Windows 8.x の記事については、「 [アーカイブ](/previous-versions/windows/apps/mt244353(v=win.10))」を参照してください。 \]

<b>重要な API</b>

-   [**RunAsync**](/uwp/api/windows.system.threading.threadpool.runasync)
-   [**IAsyncAction**](/uwp/api/Windows.Foundation.IAsyncAction)

スレッド プールに作業項目を送信することで独立したスレッドで作業を実行する方法について説明します。 これによって、非常に時間のかかる作業を実行しながら UI の応答性を確保でき、また複数のタスクを並行して実行することができます。

## <a name="create-and-submit-the-work-item"></a>作業項目の作成と送信

[**RunAsync**](/uwp/api/windows.system.threading.threadpool.runasync) を呼び出して作業項目を作成します。 作業を実行するデリゲートを指定します (ラムダやデリゲート関数を使うことができます)。 **RunAsync** が [**IAsyncAction**](/uwp/api/Windows.Foundation.IAsyncAction) オブジェクトを返すことに注意してください。このオブジェクトは次の手順で使うために格納しておきます。

3 つのバージョンの [**RunAsync**](/uwp/api/windows.system.threading.threadpool.runasync) を使うことができるため、必要に応じて作業項目の優先度を指定し、他の作業項目と同時に実行するかどうかを制御できます。

>[!NOTE]
>[**CoreDispatcher**](/uwp/api/windows.ui.core.coredispatcher.runasync)を使用して UI スレッドにアクセスし、作業項目の進行状況を表示します。

次の例では作業項目を作成し、作業を実行するラムダを指定します。

```csharp
// The nth prime number to find.
const uint n = 9999;

// A shared pointer to the result.
// We use a shared pointer to keep the result alive until the
// thread is done.
ulong nthPrime = 0;

// Simulates work by searching for the nth prime number. Uses a
// naive algorithm and counts 2 as the first prime number.
IAsyncAction asyncAction = Windows.System.Threading.ThreadPool.RunAsync(
    (workItem) =>
{
    uint  progress = 0; // For progress reporting.
    uint  primes = 0;   // Number of primes found so far.
    ulong i = 2;        // Number iterator.

    if ((n >= 0) && (n <= 2))
    {
        nthPrime = n;
        return;
    }

    while (primes < (n - 1))
    {
        if (workItem.Status == AsyncStatus.Canceled)
        {
            break;
        }

        // Go to the next number.
        i++;

        // Check for prime.
        bool prime = true;
        for (uint j = 2; j < i; ++j)
        {
            if ((i % j) == 0)
            {
                prime = false;
                break;
            }
        };

        if (prime)
        {
            // Found another prime number.
            primes++;

            // Report progress at every 10 percent.
            uint temp = progress;
            progress = (uint)(10.0*primes/n);

            if (progress != temp)
            {
                String updateString;
                updateString = "Progress to " + n + "th prime: "
                    + (10 * progress) + "%\n";

                // Update the UI thread with the CoreDispatcher.
                CoreApplication.MainView.CoreWindow.Dispatcher.RunAsync(
                    CoreDispatcherPriority.High,
                    new DispatchedHandler(() =>
                {
                    UpdateUI(updateString);
                }));
            }
        }
    }

    // Return the nth prime number.
    nthPrime = i;
});

// A reference to the work item is cached so that we can trigger a
// cancellation when the user presses the Cancel button.
m_workItem = asyncAction;
```

```cppwinrt
// The nth prime number to find.
const unsigned int n{ 9999 };

unsigned long nthPrime{ 0 };

// Simulates work by searching for the nth prime number. Uses a
// naive algorithm and counts 2 as the first prime number.

// A reference to the work item is cached so that we can trigger a
// cancellation when the user presses the Cancel button.
m_workItem = Windows::System::Threading::ThreadPool::RunAsync([&](Windows::Foundation::IAsyncAction const& workItem)
{
    unsigned int progress = 0; // For progress reporting.
    unsigned int primes = 0;   // Number of primes found so far.
    unsigned long int i = 2;   // Number iterator.

    if ((n >= 0) && (n <= 2))
    {
        nthPrime = n;
        return;
    }

    while (primes < (n - 1))
    {
        if (workItem.Status() == Windows::Foundation::AsyncStatus::Canceled)
        {
            break;
        }

        // Go to the next number.
        i++;

        // Check for prime.
        bool prime = true;
        for (unsigned int j = 2; j < i; ++j)
        {
            if ((i % j) == 0)
            {
                prime = false;
                break;
            }
        };

        if (prime)
        {
            // Found another prime number.
            primes++;

            // Report progress at every 10 percent.
            unsigned int temp = progress;
            progress = static_cast<unsigned int>(10.f*primes / n);

            if (progress != temp)
            {
                std::wstringstream updateString;
                updateString << L"Progress to " << n << L"th prime: " << (10 * progress) << std::endl;

                // Update the UI thread with the CoreDispatcher.
                Windows::ApplicationModel::Core::CoreApplication::MainView().CoreWindow().Dispatcher().RunAsync(
                    Windows::UI::Core::CoreDispatcherPriority::High,
                    Windows::UI::Core::DispatchedHandler([&]()
                {
                    UpdateUI(updateString.str());
                }));
            }
        }
    }
    // Return the nth prime number.
    nthPrime = i;
});
```

```cpp
// The nth prime number to find.
const unsigned int n = 9999;

// A shared pointer to the result.
// We use a shared pointer to keep the result alive until the
// thread is done.
std::shared_ptr<unsigned long> nthPrime = std::make_shared<unsigned long int>(0);

// Simulates work by searching for the nth prime number. Uses a
// naive algorithm and counts 2 as the first prime number.
auto workItem = ref new Windows::System::Threading::WorkItemHandler(
    \[this, n, nthPrime](IAsyncAction^ workItem)
{
    unsigned int progress = 0; // For progress reporting.
    unsigned int primes = 0;   // Number of primes found so far.
    unsigned long int i = 2;   // Number iterator.

    if ((n >= 0) && (n <= 2))
    {
        *nthPrime = n;
        return;
    }

    while (primes < (n - 1))
    {
        if (workItem->Status == AsyncStatus::Canceled)
       {
           break;
       }

       // Go to the next number.
       i++;

       // Check for prime.
       bool prime = true;
       for (unsigned int j = 2; j < i; ++j)
       {
           if ((i % j) == 0)
           {
               prime = false;
               break;
           }
       };

       if (prime)
       {
           // Found another prime number.
           primes++;

           // Report progress at every 10 percent.
           unsigned int temp = progress;
           progress = static_cast<unsigned int>(10.f*primes / n);

           if (progress != temp)
           {
               String^ updateString;
               updateString = "Progress to " + n + "th prime: "
                   + (10 * progress).ToString() + "%\n";

               // Update the UI thread with the CoreDispatcher.
               CoreApplication::MainView->CoreWindow->Dispatcher->RunAsync(
                   CoreDispatcherPriority::High,
                   ref new DispatchedHandler([this, updateString]()
               {
                   UpdateUI(updateString);
               }));
           }
       }
   }
   // Return the nth prime number.
   *nthPrime = i;
});

auto asyncAction = ThreadPool::RunAsync(workItem);

// A reference to the work item is cached so that we can trigger a
// cancellation when the user presses the Cancel button.
m_workItem = asyncAction;
```

[**RunAsync**](/uwp/api/windows.system.threading.threadpool.runasync) が呼び出された後に、スレッド プールで作業項目がキューに入れられ、スレッドが使用可能になったときに実行されます。 スレッド プールの作業項目は非同期に実行されます。任意の順番で実行されることがあるため、作業項目は単独で機能するようにしてください。

作業項目は [**IAsyncInfo.Status**](/uwp/api/windows.foundation.iasyncinfo.status) プロパティをチェックし、作業項目が取り消されている場合は終了することに注意してください。

## <a name="handle-work-item-completion"></a>作業項目の完了の処理

作業項目の [**IAsyncAction.Completed**](/uwp/api/windows.foundation.iasyncaction.completed) プロパティを設定することで、完了ハンドラーを指定します。 作業項目の完了を処理するデリゲートを指定します (ラムダやデリゲート関数を使うことができます)。 たとえば、UI スレッドにアクセスしたり、結果を表示したりするには、[**CoreDispatcher.RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync) を使います。

次の例では、手順 1. で送信した作業項目の結果を使って UI を更新します。

```cpp
asyncAction->Completed = ref new AsyncActionCompletedHandler(
    \[this, n, nthPrime](IAsyncAction^ asyncInfo, AsyncStatus asyncStatus)
{
    if (asyncStatus == AsyncStatus::Canceled)
    {
        return;
    }

    String^ updateString;
    updateString = "\n" + "The " + n + "th prime number is "
        + (*nthPrime).ToString() + ".\n";

    // Update the UI thread with the CoreDispatcher.
    CoreApplication::MainView->CoreWindow->Dispatcher->RunAsync(
        CoreDispatcherPriority::High,
        ref new DispatchedHandler([this, updateString]()
    {
        UpdateUI(updateString);
    }));
});
```

```cppwinrt
m_workItem.Completed([&](Windows::Foundation::IAsyncAction const& asyncInfo, Windows::Foundation::AsyncStatus const& asyncStatus)
{
    if (asyncStatus == Windows::Foundation::AsyncStatus::Canceled)
    {
        return;
    }

    std::wstringstream updateString;
    updateString << std::endl << L"The " << n << L"th prime number is " << nthPrime << std::endl;

    // Update the UI thread with the CoreDispatcher.
    Windows::ApplicationModel::Core::CoreApplication::MainView().CoreWindow().Dispatcher().RunAsync(
        Windows::UI::Core::CoreDispatcherPriority::High,
        Windows::UI::Core::DispatchedHandler([&]()
    {
        UpdateUI(updateString.str());
    }));
});
```

```c#
asyncAction.Completed = new AsyncActionCompletedHandler(
    (IAsyncAction asyncInfo, AsyncStatus asyncStatus) =>
{
    if (asyncStatus == AsyncStatus.Canceled)
    {
        return;
    }

    String updateString;
    updateString = "\n" + "The " + n + "th prime number is "
        + nthPrime + ".\n";

    // Update the UI thread with the CoreDispatcher.
    CoreApplication.MainView.CoreWindow.Dispatcher.RunAsync(
        CoreDispatcherPriority.High,
        new DispatchedHandler(()=>
    {
        UpdateUI(updateString);
    }));
});
```

完了ハンドラーは、UI 更新をディスパッチする前に作業項目が取り消されたかどうかをチェックします。

## <a name="summary-and-next-steps"></a>まとめと次のステップ

詳細については、このクイックスタートの「Windows 8.1 用に記述された [ThreadPool 作業項目サンプルの作成](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Thread%20pool%20sample) 」に記載されているコードをダウンロードし、win \_ unap Windows 10 アプリでソースコードを再利用する方法に関するページを参照してください。

## <a name="related-topics"></a>関連トピック

* [スレッド プールへの作業項目の送信](submit-a-work-item-to-the-thread-pool.md)
* [スレッド プールを使うためのベスト プラクティス](best-practices-for-using-the-thread-pool.md)
* [タイマーを使った作業項目の送信](use-a-timer-to-submit-a-work-item.md)
 