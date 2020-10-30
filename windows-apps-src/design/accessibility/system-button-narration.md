---
description: 詳細については、「スクリーンリーダーとハードウェアシステムのボタン」を参照してください。
title: スクリーンリーダーとハードウェアボタンイベント
label: Screen readers and hardware button events
template: detail.hbs
ms.date: 02/20/2020
ms.topic: article
keywords: windows 10、uwp、アクセシビリティ、ナレーター、スクリーンリーダー
ms.localizationpriority: medium
ms.openlocfilehash: d4bf712c83b39a184247a4476db5a045f62a9482
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2020
ms.locfileid: "93029855"
---
# <a name="screen-readers-and-hardware-system-buttons"></a>スクリーン リーダーとハードウェア システムのボタン

[ナレーター](https://support.microsoft.com/help/22798/windows-10-complete-guide-to-narrator)などのスクリーンリーダーは、ハードウェアシステムのボタンイベントを認識して処理し、その状態をユーザーに伝えることができる必要があります。 場合によっては、スクリーンリーダーがこれらのハードウェアボタンイベントを排他的に処理し、他のハンドラーにバブルアップさせないようにすることが必要になることがあります。

Windows 10 バージョン2004以降、UWP アプリケーションは、他のハードウェアボタンと同じ方法で、 **Fn** ハードウェアシステムボタンイベントをリッスンして処理できます。 以前は、このシステムボタンは、他のハードウェアボタンがイベントと状態を報告した方法の修飾子としてのみ使用されていました。

> [!NOTE]
> Fn ボタンのサポートは OEM 固有のものであり、対応するロックインジケーターライト (たとえば、プレスアンドホールドキーの組み合わせ) と、対応するロックインジケーターライト (視覚障害のあるユーザーにとっては役に立たないことがあります) と共に、トグル/ロックをオンまたはオフにする機能などを含めることができます。

Fn ボタンイベントは、 [Windows](/uwp/api/windows.ui.input)の新しい[Systembuttoneventcontroller クラス](/uwp/api/windows.ui.input.systembuttoneventcontroller)を介して公開されます。 SystemButtonEventController オブジェクトでは、次のイベントがサポートされています。

- [SystemFunctionButtonPressed れました](/uwp/api/windows.ui.input.systembuttoneventcontroller.systemfunctionbuttonpressed)
- [SystemFunctionButtonReleased](/uwp/api/windows.ui.input.systembuttoneventcontroller.systemfunctionbuttonreleased)
- [SystemFunctionLockChanged](/uwp/api/windows.ui.input.systembuttoneventcontroller.systemfunctionlockchanged)
- [SystemFunctionLockIndicatorChanged](/uwp/api/windows.ui.input.systembuttoneventcontroller.systemfunctionlockindicatorchanged)

> [!Important]
> SystemButtonEventController は、より優先順位の高いハンドラーによって既に処理されている場合、これらのイベントを受け取ることができません。

## <a name="examples"></a>例

次の例では、DispatcherQueue に基づいて [Systembuttoneventcontroller](/uwp/api/windows.ui.input.systembuttoneventcontroller) を作成し、このオブジェクトでサポートされている4つのイベントを処理する方法を示します。

Fn ボタンを押すと、サポートされているイベントの1つ以上が発生します。 たとえば、Surface キーボードで Fn ボタンを押すと、SystemFunctionButtonPressed、Systemfunctionbuttonpressed、および SystemFunctionLockIndicatorChanged が同時に起動します。

1. この最初のスニペットでは、必要な名前空間を含め、いくつかのグローバルオブジェクトを指定します。これには、 [Systembuttoneventcontroller](/uwp/api/windows.ui.input.systembuttoneventcontroller)スレッドを管理するための[DispatcherQueue](/uwp/api/windows.system.dispatcherqueue)および[DispatcherQueueController](/uwp/api/windows.system.dispatcherqueuecontroller)オブジェクトも含まれます。

   次に、 [Systembuttoneventcontroller](/uwp/api/windows.ui.input.systembuttoneventcontroller)イベント処理デリゲートを登録するときに返される[イベントトークン](/uwp/cpp-ref-for-winrt/event-token)を指定します。

    ```cppwinrt
    namespace winrt
    {
        using namespace Windows::System;
        using namespace Windows::UI::Input;
    }

    ...

    // Declare related members
    winrt::DispatcherQueueController _queueController;
    winrt::DispatcherQueue _queue;
    winrt::SystemButtonEventController _controller;
    winrt::event_token _fnKeyDownToken;
    winrt::event_token _fnKeyUpToken;
    winrt::event_token _fnLockToken;
    ```

2. また、 [SystemFunctionLockIndicatorChanged](/uwp/api/windows.ui.input.systembuttoneventcontroller.systemfunctionlockindicatorchanged) イベントのイベントトークンとブール値を指定して、アプリケーションが "学習モード" であるかどうかを示します (ユーザーは、関数を実行せずに単にキーボードを探索します)。

    ```cppwinrt
    winrt::event_token _fnLockIndicatorToken;
    bool _isLearningMode = false;
    ```

3. この3つ目のスニペットには、 [Systembuttoneventcontroller](/uwp/api/windows.ui.input.systembuttoneventcontroller) オブジェクトでサポートされる各イベントに対応するイベントハンドラーデリゲートが含まれています。

   各イベントハンドラーは、発生したイベントをアナウンスします。 さらに、FunctionLockIndicatorChanged ハンドラーは、アプリが "Learning" モード (= true) であるかどうかも制御します `_isLearningMode` 。これにより、イベントが他のハンドラーにバブルされるのを防ぎ、ユーザーが実際に操作を実行せずにキーボード機能を調べることができるようになります。

    ```cppwinrt
    void SetupSystemButtonEventController()
    {
        // Create dispatcher queue controller and dispatcher queue
        _queueController = winrt::DispatcherQueueController::CreateOnDedicatedThread();
        _queue = _queueController.DispatcherQueue();

        // Create controller based on new created dispatcher queue
        _controller = winrt::SystemButtonEventController::CreateForDispatcherQueue(_queue);

        // Add Event Handler for each different event
        _fnKeyDownToken = _controller->FunctionButtonPressed(
            [](const winrt::SystemButtonEventController& /*sender*/, const winrt:: FunctionButtonEventArgs& args)
            {
                // Mock function to read the sentence "Fn button is pressed"
                PronounceFunctionButtonPressedMock();
                // Set Handled as true means this event is consumed by this controller
                // no more targets will receive this event
                args.Handled(true);
            });

            _fnKeyUpToken = _controller->FunctionButtonReleased(
                [](const winrt::SystemButtonEventController& /*sender*/, const winrt:: FunctionButtonEventArgs& args)
                {
                    // Mock function to read the sentence "Fn button is up"
                    PronounceFunctionButtonReleasedMock();
                    // Set Handled as true means this event is consumed by this controller
                    // no more targets will receive this event
                    args.Handled(true);
                });

        _fnLockToken = _controller->FunctionLockChanged(
            [](const winrt::SystemButtonEventController& /*sender*/, const winrt:: FunctionLockChangedEventArgs& args)
            {
                // Mock function to read the sentence "Fn shift is locked/unlocked"
                PronounceFunctionLockMock(args.IsLocked());
                // Set Handled as true means this event is consumed by this controller
                // no more targets will receive this event
                args.Handled(true);
            });

        _fnLockIndicatorToken = _controller->FunctionLockIndicatorChanged(
            [](const winrt::SystemButtonEventController& /*sender*/, const winrt:: FunctionLockIndicatorChangedEventArgs& args)
            {
                // Mock function to read the sentence "Fn lock indicator is on/off"
                PronounceFunctionLockIndicatorMock(args.IsIndicatorOn());
                // In learning mode, the user is exploring the keyboard. They expect the program
                // to announce what the key they just pressed WOULD HAVE DONE, without actually
                // doing it. Therefore, handle the event when in learning mode so the key is ignored
                // by the system.
                args.Handled(_isLearningMode);
            });
    }
    ```

## <a name="see-also"></a>関連項目

[SystemButtonEventController クラス](/uwp/api/windows.ui.input.systembuttoneventcontroller)
