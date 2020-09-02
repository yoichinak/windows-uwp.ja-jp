---
ms.assetid: 1B077801-0A58-4A34-887C-F1E85E9A37B0
title: 定期的な作業項目の作成
description: ユニバーサル Windows プラットフォーム (UWP) ThreadPoolTimer API の CreatePeriodicTimer メソッドを使用して定期的に繰り返す作業項目を作成する方法について説明します。
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10、UWP、定期的な作業項目、スレッド、タイマー
ms.localizationpriority: medium
ms.openlocfilehash: e1b50858b2c7e3ce4cd60f9401cedb75eb950c7d
ms.sourcegitcommit: 5481bb34def681bc60fbfa42d9779053febec468
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/02/2020
ms.locfileid: "89304444"
---
# <a name="create-a-periodic-work-item"></a>定期的な作業項目の作成


<b>重要な API</b>

-   [**CreatePeriodicTimer**](/uwp/api/windows.system.threading.threadpooltimer.createperiodictimer)
-   [**ThreadPoolTimer**](/uwp/api/Windows.System.Threading.ThreadPoolTimer)

定期的に実行される作業項目の作成方法を説明します。

## <a name="create-the-periodic-work-item"></a>定期的な作業項目の作成

定期的な作業項目を作成するには、[**CreatePeriodicTimer**](/uwp/api/windows.system.threading.threadpooltimer.createperiodictimer) メソッドを使います。 作業を実行するラムダを指定し、*period* パラメーターを使って送信の間隔を指定します。 period パラメーターは [**TimeSpan**](/uwp/api/Windows.Foundation.TimeSpan) 構造体を使って指定します。 この期間が経過するたびに作業項目が再送信されるため、作業を完了できる十分な長さを確保してください。

[**CreateTimer**](/uwp/api/windows.system.threading.threadpooltimer.createtimer) は [**ThreadPoolTimer**](/uwp/api/Windows.System.Threading.ThreadPoolTimer) オブジェクトを返します。 タイマーを取り消す必要が生じた場合は、このオブジェクトを格納します。

> **メモ**   間隔に 0 (または1ミリ秒未満の値) を指定しないでください。 この場合、定期タイマーは 1 回限りのタイマーとして動作します。

> **メモ**   [**CoreDispatcher**](/uwp/api/windows.ui.core.coredispatcher.runasync)を使用して UI にアクセスし、作業項目の進行状況を表示することができます。

次の例では、60 秒ごとに 1 回実行される作業項目を作成します。

> [!div class="tabbedCodeSnippets"]
> ```csharp
> TimeSpan period = TimeSpan.FromSeconds(60);
>
> ThreadPoolTimer PeriodicTimer = ThreadPoolTimer.CreatePeriodicTimer((source) =>
>     {
>         //
>         // TODO: Work
>         //
>         
>         //
>         // Update the UI thread by using the UI core dispatcher.
>         //
>         Dispatcher.RunAsync(CoreDispatcherPriority.High,
>             () =>
>             {
>                 //
>                 // UI components can be accessed within this scope.
>                 //
>
>             });
>
>     }, period);
> ```
> ``` cpp
> TimeSpan period;
> period.Duration = 60 * 10000000; // 10,000,000 ticks per second
>
> ThreadPoolTimer ^ PeriodicTimer = ThreadPoolTimer::CreatePeriodicTimer(
>         ref new TimerElapsedHandler([this](ThreadPoolTimer^ source)
>         {
>             //
>             // TODO: Work
>             //
>             
>             //
>             // Update the UI thread by using the UI core dispatcher.
>             //
>             Dispatcher->RunAsync(CoreDispatcherPriority::High,
>                 ref new DispatchedHandler([this]()
>                 {
>                     //
>                     // UI components can be accessed within this scope.
>                     //
>                         
>                 }));
>
>         }), period);
> ```

## <a name="handle-cancellation-of-the-periodic-work-item-optional"></a>定期的な作業項目の取り消しの処理 (オプション)

必要であれば、[**TimerDestroyedHandler**](/uwp/api/windows.system.threading.timerdestroyedhandler) を使って、定期タイマーの取り消しを処理できます。 定期的な作業項目の取り消しを処理するラムダを追加で指定するには、[**CreatePeriodicTimer**](/uwp/api/windows.system.threading.threadpooltimer.createperiodictimer) オーバーロードを使います。

次の例では、60 秒ごとに実行される定期的な作業項目を作成します。ここでは取り消しハンドラーも指定しています。

> [!div class="tabbedCodeSnippets"]
> ``` csharp
> using Windows.System.Threading;
>
>     TimeSpan period = TimeSpan.FromSeconds(60);
>
>     ThreadPoolTimer PeriodicTimer = ThreadPoolTimer.CreatePeriodicTimer((source) =>
>     {
>         //
>         // TODO: Work
>         //
>         
>         //
>         // Update the UI thread by using the UI core dispatcher.
>         //
>         Dispatcher.RunAsync(CoreDispatcherPriority.High,
>             () =>
>             {
>                 //
>                 // UI components can be accessed within this scope.
>                 //
>
>             });
>     },
>     period,
>     (source) =>
>     {
>         //
>         // TODO: Handle periodic timer cancellation.
>         //
>
>         //
>         // Update the UI thread by using the UI core dispatcher.
>         //
>         Dispatcher->RunAsync(CoreDispatcherPriority.High,
>             ()=>
>             {
>                 //
>                 // UI components can be accessed within this scope.
>                 //                 
>
>                 // Periodic timer cancelled.
>
>             }));
>     });
> ```
> ``` cpp
> using namespace Windows::System::Threading;
> using namespace Windows::UI::Core;
>
> TimeSpan period;
> period.Duration = 60 * 10000000; // 10,000,000 ticks per second
>
> ThreadPoolTimer ^ PeriodicTimer = ThreadPoolTimer::CreatePeriodicTimer(
>         ref new TimerElapsedHandler([this](ThreadPoolTimer^ source)
>         {
>             //
>             // TODO: Work
>             //
>                 
>             //
>             // Update the UI thread by using the UI core dispatcher.
>             //
>             Dispatcher->RunAsync(CoreDispatcherPriority::High,
>                 ref new DispatchedHandler([this]()
>                 {
>                     //
>                     // UI components can be accessed within this scope.
>                     //
>
>                 }));
>
>         }),
>         period,
>         ref new TimerDestroyedHandler([&](ThreadPoolTimer ^ source)
>         {
>             //
>             // TODO: Handle periodic timer cancellation.
>             //
>
>             Dispatcher->RunAsync(CoreDispatcherPriority::High,
>                 ref new DispatchedHandler([&]()
>                 {
>                     //
>                     // UI components can be accessed within this scope.
>                     //
>
>                     // Periodic timer cancelled.
>
>                 }));
>         }));
> ```

## <a name="cancel-the-timer"></a>タイマーの取り消し

必要に応じて [**Cancel**](/uwp/api/windows.system.threading.threadpooltimer.cancel) メソッドを呼び出し、定期的な作業項目の繰り返しを停止します。 定期タイマーが取り消されたときに作業項目が実行中だった場合には、完了するまで実行することができます。 定期的な作業項目のすべてのインスタンスが完了したときに、[**TimerDestroyedHandler**](/uwp/api/windows.system.threading.timerdestroyedhandler) が呼び出されます (指定していた場合)。

> [!div class="tabbedCodeSnippets"]
> ``` csharp
> PeriodicTimer.Cancel();
> ```
> ``` cpp
> PeriodicTimer->Cancel();
> ```

## <a name="remarks"></a>注釈

1 回限りのタイマーについて詳しくは、「[タイマーを使った作業項目の送信](use-a-timer-to-submit-a-work-item.md)」をご覧ください。

## <a name="related-topics"></a>関連トピック

* [スレッド プールへの作業項目の送信](submit-a-work-item-to-the-thread-pool.md)
* [スレッド プールを使うためのベスト プラクティス](best-practices-for-using-the-thread-pool.md)
* [タイマーを使った作業項目の送信](use-a-timer-to-submit-a-work-item.md)
 