---
title: アプリを再開する方法 (DirectX と C++)
description: このトピックでは、ユニバーサル Windows プラットフォーム (UWP) DirectX アプリをシステムが再開するときに重要なアプリケーション データを復元する方法について説明します。
ms.assetid: 5e6bb673-6874-ace5-05eb-f88c045f2178
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, 再開, DirectX
ms.localizationpriority: medium
ms.openlocfilehash: 37bceafae39c314966a95f06a282fe5c91814738
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175286"
---
# <a name="how-to-resume-an-app-directx-and-c"></a>アプリを再開する方法 (DirectX と C++)



このトピックでは、ユニバーサル Windows プラットフォーム (UWP) DirectX アプリをシステムが再開するときに重要なアプリケーション データを復元する方法について説明します。

## <a name="register-the-resuming-event-handler"></a>Resuming イベント ハンドラーに登録する


[**CoreApplication::Resuming**](/uwp/api/windows.applicationmodel.core.coreapplication.resuming) イベントを処理するために登録します。このイベントは、ユーザーがアプリを切り替えてから、アプリに戻ったことを示します。

このコードをビュー プロバイダーの [**IFrameworkView::Initialize**](/uwp/api/windows.applicationmodel.core.iframeworkview.initialize) メソッドの実装に追加します。

```cpp
// The first method is called when the IFrameworkView is being created.
void App::Initialize(CoreApplicationView^ applicationView)
{
  //...
  
    CoreApplication::Resuming +=
        ref new EventHandler<Platform::Object^>(this, &App::OnResuming);
    
  //...

}
```

## <a name="refresh-displayed-content-after-suspension"></a>一時停止の後で表示されるコンテンツを更新する


アプリでは、Resuming イベントを処理する時点で、表示されているコンテンツを更新できます。 [**CoreApplication::Suspending**](/uwp/api/windows.applicationmodel.core.coreapplication.suspending) のハンドラーで保存した任意のアプリを復元し、処理を再開します。 ゲーム開発の場合、オーディオ エンジンを一時停止していたら、ここで再開します。

```cpp
void App::OnResuming(Platform::Object^ sender, Platform::Object^ args)
{
    // Restore any data or state that was unloaded on suspend. By default, data
    // and state are persisted when resuming from suspend. Note that this event
    // does not occur if the app was previously terminated.

    // Insert your code here.
}
```

このコールバックは、アプリの [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow). の [**CoreDispatcher**](/uwp/api/Windows.UI.Core.CoreDispatcher) によって処理されるイベント メッセージとして発生します。 このコールバックは、アプリのメイン ループ (ビュー プロバイダーの [**IFrameworkView::Run**](/uwp/api/windows.applicationmodel.core.iframeworkview.run) メソッドで実装) から [**CoreDispatcher::ProcessEvents**](/uwp/api/windows.ui.core.coredispatcher.processevents) を呼び出さない場合は呼び出されません。

``` syntax
// This method is called after the window becomes active.
void App::Run()
{
    while (!m_windowClosed)
    {
        if (m_windowVisible)
        {
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);

            m_main->Update();

            if (m_main->Render())
            {
                m_deviceResources->Present();
            }
        }
        else
        {
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
        }
    }
}
```

## <a name="remarks"></a>注釈


ユーザーが別のアプリまたはデスクトップに切り替えると、システムはアプリを中断します。 ユーザーが元のアプリに戻すと、システムはアプリを再開します。 システムがアプリを再開した時点で、変数とデータ構造の内容は、システムがアプリを一時停止する前の状態と同じです。 システムはアプリを厳密に中断前の状態に復元するので、ユーザーからはアプリがバックグラウンドで実行していたように見えます。 しかし、アプリは長時間一時停止している場合があるので、アプリが一時停止している間に変化した可能性のある表示コンテンツを更新し、レンダリングやオーディオ処理スレッドを再開する必要があります。 以前の一時中止イベント中にゲームの状態データを保存してある場合は、ここで復元します。

## <a name="related-topics"></a>関連トピック

* [アプリを一時停止する方法 (DirectX と C++)](how-to-suspend-an-app-directx-and-cpp.md)
* [アプリをアクティブ化する方法 (DirectX と C++)](how-to-activate-an-app-directx-and-cpp.md)

 

 