---
ms.assetid: 646977ed-1705-4ea7-a3db-a6b9aac70703
description: JavaScript および HTML で記述されたユニバーサル Windows プラットフォーム (UWP) アプリを使用してスポット ad を起動する方法について説明します。
title: JavaScript を使ったスポット広告のサンプル コード
ms.date: 02/18/2020
ms.topic: article
keywords: windows 10, UWP, 広告, Advertising, スポット, JavaScript, サンプルコード
ms.localizationpriority: medium
ms.openlocfilehash: 07a61bfde1fc6a77f87617ee553408c71f79cbc3
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363636"
---
# <a name="interstitial-ad-sample-code-in-javascript"></a>JavaScript を使ったスポット広告のサンプル コード

>[!WARNING]
> 2020年6月1日から、Microsoft Ad 収益化 platform for Windows UWP アプリがシャットダウンされます。 [詳細情報](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

このトピックでは、スポット広告を表示する基本的な JavaScript と HTML のユニバーサル Windows プラットフォーム (UWP) アプリの完全なサンプル コードを示します。 このコードを使うためのプロジェクトの詳しい構成手順については、「[スポット広告](interstitial-ads.md)」をご覧ください。 完全なサンプル プロジェクトについては、[GitHub の広告サンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising)をご覧ください。

## <a name="code-example"></a>コードの例

このセクションでは、スポット広告を表示する基本的なアプリの HTML と JavaScript ファイルの内容を示します。 これらの例を使用するには、コードを Visual Studio の JavaScript の **WinJS アプリ (ユニバーサル Windows)** プロジェクトにコピーします。

このサンプル アプリでは 2 つのボタンを使用して、スポット広告を要求して起動します。 Visual Studio によって生成された main.js ファイルと index.html ファイルは変更されています。これらのファイルを次に示します。 次に示す script.js ファイルには、サンプルのコードの大半が含まれています。このファイルは **js** フォルダーのプロジェクトに追加します。

```applicationId``` ```adUnitId``` アプリをストアに送信する前に、変数と変数の値を、パートナーセンターのライブ値に置き換えます。 詳しくは、「[アプリの広告ユニットをセットアップする](set-up-ad-units-in-your-app.md#live-ad-units)」をご覧ください。

> [!NOTE]
> ビデオ スポット広告ではなくバナー スポット広告を表示するようにこの例を変更するには、[RequestAd](/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) メソッドの最初のパラメーターとして、**InterstitialAdType.video** の代わりに値 **InterstitialAdType.display** を渡します。 詳しくは、「[スポット広告](interstitial-ads.md)」をご覧ください。

### <a name="indexhtml"></a>index.html

> [!div class="tabbedCodeSnippets"]
:::code language="html" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/js/index.html" range="1-21":::

### <a name="scriptjs"></a>script.js

> [!div class="tabbedCodeSnippets"]
:::code language="javascript" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/js/script.js" id="script":::

### <a name="mainjs"></a>main.js

> [!div class="tabbedCodeSnippets"]
:::code language="javascript" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/js/main.js" id="main":::

## <a name="related-topics"></a>関連トピック

* [GitHub の広告サンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising)

 
