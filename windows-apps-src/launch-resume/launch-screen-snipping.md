---
title: 画面切り取りの起動
description: このトピックでは、ms screenclip および ms screenclip URI スキームについて説明します。 アプリでは、これらの URI スキームを使用して、切り取り & スケッチアプリを起動したり、新しい切り取り領域を開いたりすることができます。
ms.date: 08/09/2017
ms.topic: article
keywords: windows 10、uwp、uri、切り取り領域、スケッチ
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 2d7471f414922eb1e4923079082ee6abfd8418bd
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167816"
---
# <a name="launch-screen-snipping"></a>画面切り取りの起動

**Ms screenclip:** および**ms screenclip:** URI スキームでは、snipping または編集スクリーンショットを開始できます。

## <a name="open-a-new-snip-from-your-app"></a>アプリから新しい切り取り領域を開く

**Ms screenclip:** URI を使用すると、アプリを自動的に開いて、新しい切り取り領域を開始できます。 結果の切り取り領域はユーザーのクリップボードにコピーされますが、開いているアプリには自動的には戻されません。

**ms screenclip:** 次のパラメーターを受け取ります。

| パラメーター | Type | 必須 | 説明 |
| --- | --- | --- | --- |
| source | string | no | URI を起動したソースを示す自由形式の文字列。 |
| delayInSeconds | INT | Ｘ | 1 ~ 30 の整数値。 URI 呼び出しから snipping が開始されるまでの遅延を秒単位で指定します。 |
| 表示形式 | string | no | このパラメーターは使用できません。 |

## <a name="launching-the-snip--sketch-app"></a>切り取り & スケッチアプリを起動しています

**Ms screensketch:** URI を使用すると、プログラムによって & スケッチアプリの切り取りを開始し、そのアプリ内の特定のイメージを注釈用に開くことができます。

**ms screensketch:** 次のパラメーターを受け取ります。

| パラメーター | Type | 必須 | 説明 |
| --- | --- | --- | --- |
| sharedAccessToken | string | no | 切り取り & スケッチアプリで開くファイルを識別するトークン。 [Sharedstorageaccessmanager. AddFile](/uwp/api/windows.applicationmodel.datatransfer.sharedstorageaccessmanager.addfile)から取得されます。 このパラメーターを省略すると、ファイルを開いていない状態でアプリが起動されます。 |
| secondarySharedAccessToken | string | no | 切り取り領域に関するメタデータを含む JSON ファイルを識別する文字列。 メタデータには、x、y 座標、または[Useractivity](/uwp/api/windows.applicationmodel.useractivities.useractivity)の配列を含む**clippoints**フィールドを含めることができます。 |
| source | string | no | URI を起動したソースを示す自由形式の文字列。 |
| isTemporary | [bool] | Ｘ | True に設定すると、ファイルを開いた後に、そのファイルを削除しようとします。 |

次の例では、 [LaunchUriAsync](/uwp/api/Windows.System.Launcher#Windows_System_Launcher_LaunchUriAsync_Windows_Foundation_Uri_) メソッドを呼び出して、ユーザーのアプリから & の切り取り領域にイメージを送信します。

```csharp

bool result = await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-screensketch:edit?source=MyApp&isTemporary=false&sharedAccessToken=2C37ADDA-B054-40B5-8B38-11CED1E1A2D"));

```

次の例は、 **ms screensketch**の**secondarySharedAccessToken**パラメーターによって指定されるファイルに、次のものが含まれていることを示しています。

```json
{
  "clipPoints": [
    {
      "x": 0,
      "y": 0
    },
    {
      "x": 2080,
      "y": 0
    },
    {
      "x": 2080,
      "y": 780
    },
    {
      "x": 0,
      "y": 780
    }
  ],
  "userActivity": "{\"$schema\":\"http://activity.windows.com/user-activity.json\",\"UserActivity\":\"type\",\"1.0\":\"version\",\"cross-platform-identifiers\":[{\"platform\":\"windows_universal\",\"application\":\"Microsoft.MicrosoftEdge_8wekyb3d8bbwe!MicrosoftEdge\"},{\"platform\":\"host\",\"application\":\"edge.activity.windows.com\"}],\"activationUrl\":\"microsoft-edge:https://support.microsoft.com/help/13776/windows-use-snipping-tool-to-capture-screenshots\",\"contentUrl\":\"https://support.microsoft.com/help/13776/windows-use-snipping-tool-to-capture-screenshots\",\"visualElements\":{\"attribution\":{\"iconUrl\":\"https://www.microsoft.com/favicon.ico?v2\",\"alternateText\":\"microsoft.com\"},\"description\":\"https://support.microsoft.com/help/13776/windows-use-snipping-tool-to-capture-screenshots\",\"backgroundColor\":\"#FF0078D7\",\"displayText\":\"Use snipping tool to capture screenshots - Windows Help\",\"content\":{\"$schema\":\"http://adaptivecards.io/schemas/adaptive-card.json\",\"type\":\"AdaptiveCard\",\"version\":\"1.0\",\"body\":[{\"type\":\"Container\",\"items\":[{\"type\":\"TextBlock\",\"text\":\"Use snipping tool to capture screenshots - Windows Help\",\"weight\":\"bolder\",\"size\":\"large\",\"wrap\":true,\"maxLines\":3},{\"type\":\"TextBlock\",\"text\":\"https://support.microsoft.com/help/13776/windows-use-snipping-tool-to-capture-screenshots\",\"size\":\"normal\",\"wrap\":true,\"maxLines\":3}]}]}},\"isRoamable\":true,\"appActivityId\":\"https://support.microsoft.com/help/13776/windows-use-snipping-tool-to-capture-screenshots\"}"
}

```