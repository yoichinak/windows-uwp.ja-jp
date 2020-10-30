---
description: ローカルトースト通知を後で表示するようにスケジュールする方法について説明します。
title: トースト通知をスケジュールする
label: Schedule a toast notification
template: detail.hbs
ms.date: 04/09/2020
ms.topic: article
keywords: windows 10、uwp、スケジュールされたトースト通知、scheduledtoastnotification、方法、クイックスタート、作業の開始、コードサンプル、チュートリアル
ms.localizationpriority: medium
ms.openlocfilehash: 2a138458634f0246d7e6bed9d6d65c2479dac3c9
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2020
ms.locfileid: "93030695"
---
# <a name="schedule-a-toast-notification"></a>トースト通知をスケジュールする

スケジュールされたトースト通知を使用すると、アプリがその時点で実行されているかどうかに関係なく、後で通知を表示するようにスケジュールできます。 これは、通知などのフォローアップタスクをユーザーに表示する場合に便利です。通知の時刻と内容は事前にわかっています。

スケジュールされたトースト通知の配信ウィンドウは5分間であることに注意してください。 スケジュールされた配信時にコンピューターの電源がオフになっていて、5分以上経過している場合、通知はユーザーに関連付けられていない状態で "破棄" されます。 コンピューターの電源が切れた時間に関係なく通知を確実に配信する必要がある場合は、 [次のコードサンプル](https://github.com/WindowsNotifications/quickstart-snoozable-toasts-even-if-computer-is-off)に示すように、時間トリガー付きのバックグラウンドタスクを使用することをお勧めします。

> [!IMPORTANT]
> デスクトップアプリケーション (MSIX/スパースパッケージと従来のデスクトップの両方) には、通知を送信し、アクティブ化を処理するための手順が若干異なります。 次の手順に従ってください。ただし、は `ToastNotificationManager` `DesktopNotificationManagerCompat` [デスクトップアプリ](toast-desktop-apps.md) のドキュメントのクラスで置き換えてください。

> **重要な api** : [scheduledtoastnotification クラス](/uwp/api/Windows.UI.Notifications.ScheduledToastNotification)


## <a name="prerequisites"></a>前提条件

このトピックを十分に理解するには、次のものが役立ちます。

* トースト通知に関する用語と概念についての実用的知識。 詳しくは、[トーストとアクション センターの概要](/archive/blogs/tiles_and_toasts/toast-notification-and-action-center-overview-for-windows-10)に関するブログをご覧ください。
* Windows 10 のトースト通知のコンテンツに関する知識。 詳しくは、[トースト コンテンツのドキュメント](adaptive-interactive-toasts.md)をご覧ください。
* Windows 10 UWP アプリ プロジェクト


## <a name="step-1-install-nuget-package"></a>手順 1: NuGet パッケージをインストールする

[Microsoft. Toolkit. 通知 NuGet パッケージ](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)をインストールします。 このサンプルコードでは、このパッケージを使用します。 この記事の最後には、NuGet パッケージを使用しない "plain" コードスニペットが用意されています。 このパッケージを使用すると、XML を使用せずにトースト通知を作成できます。


## <a name="step-2-add-namespace-declarations"></a>手順 2: 名前空間宣言を追加する

`Windows.UI.Notifications` トースト Api が含まれています。

```csharp
using Windows.UI.Notifications;
using Microsoft.Toolkit.Uwp.Notifications; // Notifications library
```


## <a name="step-3-schedule-the-notification"></a>手順 3: 通知をスケジュールする

ここでは、今日のお客様による宿題について学生に通知する単純なテキストベースの通知を使用します。 通知を作成してスケジュールを設定します。

```csharp
// Construct the content
var content = new ToastContentBuilder()
    .AddToastActivationInfo("itemsDueToday", ToastActivationType.Foreground)
    .AddText("ASTR 170B1")
    .AddText("You have 3 items due today!");
    .GetToastContent();

    
// Create the scheduled notification
var toast = new ScheduledToastNotification(content.GetXml(), DateTime.Now.AddSeconds(5));


// Add your scheduled toast to the schedule
ToastNotificationManager.CreateToastNotifier().AddToSchedule(toast);
```


## <a name="provide-a-primary-key-for-your-toast"></a>トーストの主キーを提供する

スケジュールされた通知をプログラムによってキャンセル、削除、または置換する場合は、Tag プロパティ (および必要に応じて Group プロパティ) を使用して、通知の主キーを指定する必要があります。 その後、この主キーを使用して、通知の取り消し、削除、または置換を行うことができます。

既に配信されたトースト通知の差し替えと削除の方法について詳しくは、「[クイック スタート: アクション センターでのトースト通知の管理 (XAML)](/previous-versions/windows/apps/dn631260(v=win.10))」をご覧ください。

Tag と Group を組み合わせると、復号主キーとして機能します。 Group はより汎用的な ID で、"wallPosts"、"messages"、"friendRequests" などのグループを割り当てることができます。Tag はグループ内から通知自体を一意に識別する必要があります。 汎用グループを使うことで、[RemoveGroup API](/uwp/api/Windows.UI.Notifications.ToastNotificationHistory#Windows_UI_Notifications_ToastNotificationHistory_RemoveGroup_System_String_) を使ってそのグループからすべての通知を削除できます。

```csharp
toast.Tag = "18365";
toast.Group = "ASTR 170B1";
```


## <a name="cancel-scheduled-notifications"></a>スケジュールされた通知を取り消す

スケジュールされた通知をキャンセルするには、最初に、すべてのスケジュールされた通知の一覧を取得する必要があります。

次に、前に指定したタグ (および必要に応じてグループ) に一致するスケジュールされたトーストを見つけ、RemoveFromSchedule () を呼び出します。

```csharp
// Create the toast notifier
ToastNotifier notifier = ToastNotificationManager.CreateToastNotifier();

// Get the list of scheduled toasts that haven't appeared yet
IReadOnlyList<ScheduledToastNotification> scheduledToasts = notifier.GetScheduledToastNotifications();

// Find our scheduled toast we want to cancel
var toRemove = scheduledToasts.FirstOrDefault(i => i.Tag == "18365" && i.Group == "ASTR 170B1");
if (toRemove != null)
{
    // And remove it from the schedule
    notifier.RemoveFromSchedule(toRemove);
}
```


## <a name="activation-handling"></a>ライセンス認証の処理

アクティブ化の処理の詳細については、「 [ローカルのトーストドキュメントを送信](send-local-toast.md) する」を参照してください。 スケジュールされたトースト通知のアクティブ化は、ローカルトースト通知のアクティブ化と同様に処理されます。


## <a name="adding-actions-inputs-and-more"></a>アクションや入力などの追加

アクションや入力などの高度なトピックの詳細については、「 [ローカルのトーストドキュメントを送信](send-local-toast.md) する」を参照してください。 アクションと入力は、スケジュールされたトーストと同様にローカルのトーストでも動作します。


## <a name="resources"></a>リソース

* [トースト コンテンツのドキュメント](adaptive-interactive-toasts.md)
* [ScheduledToastNotification クラス](/uwp/api/Windows.UI.Notifications.ScheduledToastNotification)
