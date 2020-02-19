---
ms.assetid: 646977ed-1705-4ea7-a3db-a6b9aac70703
description: JavaScript/HTML. を使ってスポット広告を起動する方法について説明します。
title: JavaScript を使ったスポット広告のサンプル コード
ms.date: 02/18/2020
ms.topic: article
keywords: windows 10, UWP, 広告, Advertising, スポット, JavaScript, サンプルコード
ms.localizationpriority: medium
ms.openlocfilehash: 2f08e3425b8364bf8c6beb8a8d136534aea4a9b2
ms.sourcegitcommit: 6af7ce0e3c27f8e52922118deea1b7aad0ae026e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/19/2020
ms.locfileid: "77463514"
---
# <a name="interstitial-ad-sample-code-in-javascript"></a>JavaScript を使ったスポット広告のサンプル コード

>[!WARNING]
> 2020年6月1日から、Microsoft Ad 収益化 platform for Windows UWP アプリがシャットダウンされます。 [詳細情報](https://aka.ms/ad-monetization-shutdown)

このトピックでは、スポット広告を表示する基本的な JavaScript と HTML のユニバーサル Windows プラットフォーム (UWP) アプリの完全なサンプル コードを示します。 このコードを使うためのプロジェクトの詳しい構成手順については、「[スポット広告](interstitial-ads.md)」をご覧ください。 完全なサンプル プロジェクトについては、[GitHub の広告サンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising)をご覧ください。

## <a name="code-example"></a>コードの例

このセクションでは、スポット広告を表示する基本的なアプリの HTML と JavaScript ファイルの内容を示します。 これらの例を使用するには、コードを Visual Studio の JavaScript の **WinJS アプリ (ユニバーサル Windows)** プロジェクトにコピーします。

このサンプル アプリでは 2 つのボタンを使用して、スポット広告を要求して起動します。 Visual Studio によって生成された main.js ファイルと index.html ファイルは変更されています。これらのファイルを次に示します。 次に示す script.js ファイルには、サンプルのコードの大半が含まれています。このファイルは **js** フォルダーのプロジェクトに追加します。

アプリをストアに送信する前に、```applicationId``` と ```adUnitId``` 変数の値を、パートナーセンターのライブ値に置き換えます。 詳しくは、「[アプリの広告ユニットをセットアップする](set-up-ad-units-in-your-app.md#live-ad-units)」をご覧ください。

> [!NOTE]
> ビデオ スポット広告ではなくバナー スポット広告を表示するようにこの例を変更するには、**RequestAd** メソッドの最初のパラメーターとして、[InterstitialAdType.video](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) の代わりに値 **InterstitialAdType.display** を渡します。 詳しくは、「[スポット広告](interstitial-ads.md)」をご覧ください。

### <a name="indexhtml"></a>index.html

> [!div class="tabbedCodeSnippets"]
[!code-html[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/index.html#L1-L21)]

### <a name="scriptjs"></a>script.js

> [!div class="tabbedCodeSnippets"]
[!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/script.js#script)]

### <a name="mainjs"></a>main.js

> [!div class="tabbedCodeSnippets"]
[!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/main.js#main)]

## <a name="related-topics"></a>関連トピック

* [GitHub の広告サンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising)

 
