---
ms.assetid: 7a16b0ca-6b8e-4ade-9853-85690e06bda6
description: C# を使ってスポット広告を起動する方法について説明します。
title: C# を使ったスポット広告のサンプル コード
ms.date: 02/18/2020
ms.topic: article
keywords: windows 10, UWP, 広告, Advertising, スポット, C#, サンプルコード
ms.localizationpriority: medium
ms.openlocfilehash: 9d908151e30510977669f2bc101575754a946560
ms.sourcegitcommit: 71f9013c41fc1038a9d6c770cea4c5e481c23fbc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/20/2020
ms.locfileid: "77507046"
---
# <a name="interstitial-ad-sample-code-in-c"></a>スポットでの ad サンプルコード\# の #  

>[!WARNING]
> 2020年6月1日から、Microsoft Ad 収益化 platform for Windows UWP アプリがシャットダウンされます。 [詳細情報](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

このトピックでは、ビデオ スポット広告を表示する基本的な C# と XAML のユニバーサル Windows プラットフォーム (UWP) アプリの完全なサンプル コードを示します。 このコードを使うためのプロジェクトの詳しい構成手順については、「[スポット広告](interstitial-ads.md)」をご覧ください。 完全なサンプル プロジェクトについては、[GitHub の広告サンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising)をご覧ください。

## <a name="code-example"></a>コードの例

このセクションでは、スポット広告を表示する基本的なアプリの MainPage.xaml と MainPage.xaml.cs ファイルの内容を示します。 これらの例を使用するには、コードを Visual Studio の Visual C# **の空白のアプリ (ユニバーサル Windows)** プロジェクトにコピーします。

このサンプル アプリでは 2 つのボタンを使用して、スポット広告を要求して起動します。 アプリをストアに送信する前に、```myAppId``` フィールドと ```myAdUnitId``` フィールドの値を、パートナーセンターのライブ値に置き換えます。 詳しくは、「[アプリの広告ユニットをセットアップする](set-up-ad-units-in-your-app.md#live-ad-units)」をご覧ください。

> [!NOTE]
> ビデオ スポット広告ではなくバナー スポット広告を表示するようにこの例を変更するには、**RequestAd** メソッドの最初のパラメーターとして、[AdType.Video](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) の代わりに値 **AdType.Display** を渡します。 詳しくは、「[スポット広告](interstitial-ads.md)」をご覧ください。

### <a name="mainpagexaml"></a>MainPage.xaml

> [!div class="tabbedCodeSnippets"]
[!code-xml[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml#L1-L13)]

### <a name="mainpagexamlcs"></a>MainPage.xaml.cs

> [!div class="tabbedCodeSnippets"]
[!code-csharp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#CompleteSample)]

 
## <a name="related-topics"></a>関連トピック

* [GitHub の広告サンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising)
 
