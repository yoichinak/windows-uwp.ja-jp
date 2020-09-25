---
description: セカンダリタイルをタスクバーにピン留めして、ユーザーがアプリ内のコンテンツにすばやくアクセスできるようにする方法について説明します。
title: タスクバーへのセカンダリ タイルのピン留め
label: Pin secondary tiles to taskbar
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10、uwp、タスクバーへのピン留め、セカンダリタイル、タスクバーへのセカンダリタイルのピン留め、ショートカット
ms.localizationpriority: medium
ms.openlocfilehash: 22f49fba21e4d3f997efee1a59123ab453e555ea
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/24/2020
ms.locfileid: "91220225"
---
# <a name="pin-secondary-tiles-to-taskbar"></a>タスクバーへのセカンダリ タイルのピン留め

セカンダリタイルのピン留めと同じように、セカンダリタイルをタスクバーにピン留めして、ユーザーがアプリ内のコンテンツにすばやくアクセスできるようにすることができます。

<img alt="Taskbar pinning" src="../images/taskbar/pin-secondary-ui.png" width="972"/>

> [!IMPORTANT]
> **制限付きアクセス api**: この api はアクセスが制限されています。 この API を使用するには、にお問い合わせください [taskbarsecondarytile@microsoft.com](mailto:taskbarsecondarytile@microsoft.com?Subject=Limited%20Access%20permission%20to%20use%20secondary%20tiles%20on%20taskbar) 。

> **2018 年10月の更新プログラムが必要**: SDK 17763 をターゲットにし、タスクバーにピン留めするためにビルド17763以降を実行する必要があります。


## <a name="guidance"></a>ガイダンス

セカンダリタイルは、アプリ内の特定の領域にユーザーが直接アクセスするための一貫した効率的な方法を提供します。 ユーザーは、セカンダリタイルをタスクバーに "ピン留めする" かどうかを選択しますが、アプリの pinnable 領域は開発者によって決定されます。 詳細については、「 [セカンダリタイルのガイダンス](secondary-tiles-guidance.md)」を参照してください。


## <a name="1-determine-if-api-exists-and-unlock-limited-access"></a>1. API が存在するかどうかを確認し、制限付きアクセスをロック解除する

古いデバイスでは、タスクバーに Api がピン留めされていません (以前のバージョンの Windows 10 を対象としている場合)。 そのため、ピン留めできないデバイスにはピンボタンを表示しないようにしてください。

また、この機能は制限付きアクセスでロックされています。 アクセスするには、Microsoft にお問い合わせください。 **[Taskbarmanager. RequestPinSecondaryTileAsync](/uwp/api/windows.ui.shell.taskbarmanager.requestpinsecondarytileasync#Windows_UI_Shell_TaskbarManager_RequestPinSecondaryTileAsync_Windows_UI_StartScreen_SecondaryTile_)**、 **[Taskbarmanager. IsSecondaryTilePinnedAsync](/uwp/api/windows.ui.shell.taskbarmanager.issecondarytilepinnedasync)**、および**[taskbarmanager](/uwp/api/windows.ui.shell.taskbarmanager.tryunpinsecondarytileasync)** の API 呼び出しは、アクセス拒否例外で失敗します。 アプリでは、アクセス許可なしでこの API を使用することはできません。また、API 定義はいつでも変更される可能性があります。

Api が存在するかどうかを判断するには、 [Apiinformation. IsMethodPresent](/uwp/api/windows.foundation.metadata.apiinformation.ismethodpresent#Windows_Foundation_Metadata_ApiInformation_IsMethodPresent_System_String_System_String_) メソッドを使用します。 次に、 **[LimitedAccessFeatures](/uwp/api/windows.applicationmodel.limitedaccessfeatures)** api を使用して api のロックを解除します。

```csharp
if (ApiInformation.IsMethodPresent("Windows.UI.Shell.TaskbarManager", "RequestPinSecondaryTileAsync"))
{
    // API present!
    // Unlock the pin to taskbar feature
    var result = LimitedAccessFeatures.TryUnlockFeature(
        "com.microsoft.windows.secondarytilemanagement",
        "<tokenFromMicrosoft>",
        "<publisher> has registered their use of com.microsoft.windows.secondarytilemanagement with Microsoft and agrees to the terms of use.");

    // If unlock succeeded
    if ((result.Status == LimitedAccessFeatureStatus.Available) ||
        (result.Status == LimitedAccessFeatureStatus.AvailableWithoutToken))
    {
        // Continue
    }
    else
    {
        // Don't show pin to taskbar button or call any of the below APIs
    }
}

else
{
    // Don't show pin to taskbar button or call any of the below APIs
}
```


## <a name="2-get-the-taskbarmanager-instance"></a>2. TaskbarManager インスタンスを取得する

Windows アプリは、さまざまなデバイスで実行できます。すべてのユーザーがタスクバーをサポートしているわけではありません。 現時点では、デスクトップ デバイスのみがタスク バーをサポートしています。 また、タスクバーが表示される場合もあります。 タスクバーが現在存在するかどうかを確認するには、 **[Taskbarmanager. GetDefault](/uwp/api/windows.ui.shell.taskbarmanager.getdefault)** メソッドを呼び出し、返されたインスタンスが null でないことを確認します。 タスクバーが存在しない場合は、pin ボタンを表示しません。

1回の操作 (ピン留めなど) の実行中にインスタンスを保持し、次に別の操作を行うときに新しいインスタンスを取得することをお勧めします。

```csharp
TaskbarManager taskbarManager = TaskbarManager.GetDefault();

if (taskbarManager != null)
{
    // Continue
}
else
{
    // Taskbar not present, don't display a pin button
}
```


## <a name="3-check-whether-your-tile-is-currently-pinned-to-the-taskbar"></a>3. 現在のタイルがタスクバーにピン留めされているかどうかを確認する

タイルが既にピン留めされている場合は、代わりに [ピン留め] ボタンを表示する必要があります。 **[IsSecondaryTilePinnedAsync](/uwp/api/windows.ui.shell.taskbarmanager.issecondarytilepinnedasync)** メソッドを使用して、タイルが現在ピン留めされているかどうかを確認できます (ユーザーはいつでもピン留めを外すことができます)。 このメソッドでは、確認するタイルのタイル **id** をピン留めして渡します。

```csharp
if (await taskbarManager.IsSecondaryTilePinnedAsync("myTileId"))
{
    // The tile is already pinned. Display the unpin button.
}

else 
{
    // The tile is not pinned. Display the pin button.
}
```


## <a name="4-check-whether-pinning-is-allowed"></a>4. ピン留めが許可されているかどうかを確認する

タスクバーへのピン留めは、グループポリシーによって無効にすることができます。 [IsPinningAllowed](/uwp/api/windows.ui.shell.taskbarmanager.ispinningallowed)プロパティを使用すると、ピン留めが許可されているかどうかを確認できます。

ユーザーが pin ボタンをクリックすると、このプロパティを確認する必要があります。 false の場合は、このコンピューターでピン留めが許可されていないことを知らせるメッセージダイアログが表示されます。

```csharp
TaskbarManager taskbarManager = TaskbarManager.GetDefault();
if (taskbarManager == null)
{
    // Display message dialog informing user that taskbar is no longer present, and then hide the button
}

else if (taskbarManager.IsPinningAllowed == false)
{
    // Display message dialog informing user pinning is not allowed on this machine
}

else
{
    // Continue pinning
}
```


## <a name="5-construct-and-pin-your-tile"></a>5. タイルを構築してピン留めする

ユーザーが [pin] ボタンをクリックし、Api が存在し、タスクバーが存在し、ピン留めが許可されていることを確認しました。ピン留めする時間

最初に、スタートにピン留めする場合と同じように、セカンダリタイルを作成します。 セカンダリタイルのプロパティの詳細については、「[ [セカンダリタイルのピン留め] を開始する](secondary-tiles-pinning.md)」を参照してください。 ただし、タスクバーにピン留めする場合は、以前に必要なプロパティに加えて、Square44x44Logo (タスクバーで使用されるロゴ) も必要です。 この操作を行わない場合、例外がスローされます。

次に、このタイルを **[Requestpinsecondaryタイル async](/uwp/api/windows.ui.shell.taskbarmanager.requestpinsecondarytileasync)** メソッドに渡します。 これはアクセスが制限されているため、確認ダイアログが表示されず、UI スレッドは必要ありません。 ただし、これが制限付きアクセス以外で開かれている場合、制限付きアクセスを使用していない呼び出し元にはダイアログが表示され、UI スレッドを使用する必要があります。

```csharp
// Initialize the tile (all properties below are required)
SecondaryTile tile = new SecondaryTile("myTileId");
tile.DisplayName = "PowerPoint 2016 (Remote)";
tile.Arguments = "app=powerpoint";
tile.VisualElements.Square44x44Logo = new Uri("ms-appdata:///AppIcons/PowerPoint_Square44x44Logo.png");
tile.VisualElements.Square150x150Logo = new Uri("ms-appdata:///AppIcons/PowerPoint_Square150x150Logo.png");

// Pin it to the taskbar
bool isPinned = await taskbarManager.RequestPinSecondaryTileAsync(tile);
```

このメソッドは、タイルがタスクバーにピン留めされているかどうかを示すブール値を返します。 タイルが既にピン留めされている場合、メソッドは既存のタイルを更新し、true を返します。 ピン留めが許可されていないか、タスクバーがサポートされていない場合、メソッドは false を返します。


## <a name="enumerate-tiles"></a>タイルの列挙

作成したすべてのタイルを表示するには、[開始]、[タスクバー]、またはその両方にピン留めされているすべてのタイルを表示するには、 **[Findallasync](/uwp/api/windows.ui.startscreen.secondarytile.findallasync)** を使用します。 その後、これらのタイルがタスクバーにピン留めされているか、開始されているかを確認できます。 サーフェイスがサポートされていない場合、これらのメソッドは false を返します。

```csharp
var taskbarManager = TaskbarManager.GetDefault();
var startScreenManager = StartScreenManager.GetDefault();

// Look through all tiles
foreach (SecondaryTile tile in await SecondaryTile.FindAllAsync())
{
    if (taskbarManager != null && await taskbarManager.IsSecondaryTilePinnedAsync(tile.TileId))
    {
        // Tile is pinned to the taskbar
    }

    if (startScreenManager != null && await startScreenManager.ContainsSecondaryTileAsync(tile.TileId))
    {
        // Tile is pinned to Start
    }
}
```


## <a name="update-a-tile"></a>タイルを更新する

既にピン留めされているタイルを更新するには、「[セカンダリタイルの更新](secondary-tiles-pinning.md#updating-a-secondary-tile)」の説明に従って、 [**UpdateAsync**](/uwp/api/windows.ui.startscreen.secondarytile.updateasync)メソッドを使用します。


## <a name="unpin-a-tile"></a>タイルのピン留めを外す

タイルが現在ピン留めされている場合、アプリはピン留めしないボタンを提供する必要があります。 タイルの固定を解除するには、 **[Tryunpinsecondarytileasync](/uwp/api/windows.ui.shell.taskbarmanager.tryunpinsecondarytileasync)** を呼び出して、固定を解除するセカンダリタイルのタイル **id** を渡します。

このメソッドは、タイルがタスクバーにピン留めされていないかどうかを示すブール値を返します。 タイルが最初の位置にピン留めされていない場合は、true も返されます。 ピン留めが許可されていない場合、false を返します。

タイルがタスクバーだけにピン留めされている場合は、どこにもピン留めされていないため、タイルが削除されます。

```csharp
var taskbarManager = TaskbarManager.GetDefault();
if (taskbarManager != null)
{
    bool isUnpinned = await taskbarManager.TryUnpinSecondaryTileAsync("myTileId");
}
```


## <a name="delete-a-tile"></a>タイルを削除する

すべての場所 (開始、タスクバー) からタイルのピン留めを解除する場合は、 **[Requestdeleteasync](/uwp/api/windows.ui.startscreen.secondarytile.requestdeleteasync)** メソッドを使用します。

これは、ユーザーがピン留めしたコンテンツが適用されなくなった場合に適しています。 たとえば、アプリでノートブックをスタート画面とタスクバーにピン留めした後、ユーザーがノートブックを削除した場合、ノートブックに関連付けられているタイルを削除するだけで済みます。

```csharp
// Initialize a secondary tile with the same tile ID you want removed.
// Or, locate it with FindAllAsync()
SecondaryTile toBeDeleted = new SecondaryTile(tileId);

// And then delete the tile
await toBeDeleted.RequestDeleteAsync();
```


## <a name="unpin-only-from-start"></a>開始時にのみピン留めを外す

セカンダリタイルをタスクバーに残したまま [開始] から固定解除するだけの場合は、 **[TryRemoveSecondaryTileAsync](/uwp/api/windows.ui.startscreen.startscreenmanager.tryremovesecondarytileasync)** メソッドを呼び出すことができます。 これにより、他のサーフェイスにピン留めされていない場合でも、タイルが削除されます。

このメソッドは、タイルが開始にピン留めされていないかどうかを示すブール値を返します。 タイルが最初の位置にピン留めされていない場合は、true も返されます。 ピン留めが許可されていない場合、または Start がサポートされていない場合、false を返します。

```csharp
await StartScreenManager.GetDefault().TryRemoveSecondaryTileAsync("myTileId");
```


## <a name="resources"></a>リソース

* [TaskbarManager クラス](/uwp/api/windows.ui.shell.taskbarmanager)
* [スタート画面へのセカンダリ タイルのピン留め](secondary-tiles-pinning.md)
