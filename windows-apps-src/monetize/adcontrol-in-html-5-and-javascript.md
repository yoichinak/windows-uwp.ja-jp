---
ms.assetid: adb2fa45-e18f-4254-bd8b-a749a386e3b4
description: Windows 10 (UWP) 用の JavaScript/HTML アプリで AdControl クラスを使ってバナー広告を表示する方法について説明します。
title: HTML 5 および JavaScript の AdControl
ms.date: 02/18/2020
ms.topic: article
keywords: Windows 10, UWP, 広告, Advertising, AdControl, 広告コントロール, JavaScript, HTML
ms.localizationpriority: medium
ms.openlocfilehash: b99432c40bf2b4633e8902a5bb7b7eedab3119dc
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/02/2020
ms.locfileid: "89364055"
---
# <a name="adcontrol-in-html-5-and-javascript"></a>HTML 5 および JavaScript の AdControl

>[!WARNING]
> 2020年6月1日から、Microsoft Ad 収益化 platform for Windows UWP アプリがシャットダウンされます。 [詳細情報](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

このチュートリアルでは、Windows 10 (HTML) 用のユニバーサル Windows プラットフォーム (UWP) JavaScript/HTML アプリで [AdControl](/uwp/api/microsoft.advertising.winrt.ui.adcontrol) クラスを使ってバナー広告を表示する方法について説明します。

JavaScript/HTML アプリにバナー広告を追加する方法を示す完全なサンプル プロジェクトについては、「[GitHub の広告サンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising)」をご覧ください。

## <a name="prerequisites"></a>前提条件

* Visual Studio 2015 以降の Visual Studio のリリースと共に [Microsoft Advertising SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK) をインストールします。 インストール手順については、[この記事](install-the-microsoft-advertising-libraries.md)をご覧ください。

> [!NOTE]
> Windows 10 SDK バージョン 10.0.14393 (記念日更新) 以降のバージョンの Windows SDK がインストールされている場合は、 [WinJS](https://github.com/winjs/winjs) ライブラリもインストールする必要があります。 このライブラリは以前のバージョンの Windows SDK for Windows 10 に含まれていましたが、Windows 10 SDK バージョン 10.0.14393 (Anniversary Update) 以降ではこのライブラリを別個にインストールする必要があります。 

## <a name="integrate-a-banner-ad-into-your-app"></a>バナー広告をアプリに統合する

1. Visual Studio でプロジェクトを開くか、新しいプロジェクトを作ります。

    > [!NOTE]
    > 既存のプロジェクトを使用している場合、プロジェクトの Package.appxmanifest ファイルを開き、**インターネット (クライアント)** 機能が選択されていることを確認します。 アプリでは、テスト広告やライブ広告を受信するためにこの機能が必要になります。

2. プロジェクトのターゲットが **[Any CPU]** (任意の CPU) になっている場合は、アーキテクチャ固有のビルド出力 (たとえば、**[x86]**) を使うようにプロジェクトを更新します。 プロジェクトのターゲットが **[Any CPU]** (任意の CPU) になっていると、次の手順で Microsoft Advertising ライブラリへの参照を正常に追加できません。 詳しくは、「[プロジェクトのターゲットを "Any CPU" に設定すると参照エラーが発生する](known-issues-for-the-advertising-libraries.md#reference_errors)」をご覧ください。

3. プロジェクトで Microsoft Advertising SDK への参照を追加します。

    1. **[ソリューション エクスプローラー]** ウィンドウで、**[参照設定]** を右クリックし、**[参照の追加]** を選択します。
    2.  **[参照マネージャー]** で、**[ユニバーサル Windows]** を展開し、**[拡張]** をクリックして、**[Microsoft Advertising SDK for JavaScript]** (バージョン 10.0) の横にあるチェック ボックスをオンにします。
    3.  **[参照マネージャー]** で、[OK] をクリックします。

6.  index.html ファイル (またはプロジェクトに対応するその他の html ファイル) を開きます。

7.  ** &lt; Head &gt; **セクションで、プロジェクトの JavaScript による .css および main.js の参照の後に、ad.js への参照を追加します。

    ``` HTML
    <!-- Advertising required references -->
    <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
    ```

    > [!NOTE]
    > この行は、main.js を含めた後に** &lt; head &gt; **セクションに配置する必要があります。そうしないと、プロジェクトのビルド時にエラーが発生します。

8.  default.html ファイル (または、プロジェクトに適したその他の html ファイル) の** &lt; body &gt; **セクションを変更して、 **adcontrol**の**div**を含めます。 **AdControl** の **applicationId** プロパティと **adUnitId** プロパティに、[広告ユニットのテスト値](set-up-ad-units-in-your-app.md#test-ad-units)を割り当てます。 また、コントロールの**高さ**と**幅**を、[バナー広告でサポートされている広告サイズ](supported-ad-sizes-for-banner-ads.md)のいずれかに合わせて調整します。

    > [!NOTE]
    > 各 **AdControl** に、対応する*広告ユニット*があります。広告ユニットは、コントロールに広告を提供するためにサービスで使用されます。すべての広告ユニットは、*広告ユニット ID* と*アプリケーション ID* で構成されます。 ここでは、広告ユニット ID とアプリケーション ID のテスト値をコントロールに割り当てます。 これらのテスト値は、テスト バージョンのアプリでのみ使用できます。 ストアにアプリを発行する前に、 [これらのテスト値をパートナーセンターのライブ値に置き換える](#release) 必要があります。

    ``` HTML
    <div id="myAd" style="position: absolute; top: 50px; left: 0px; width: 300px; height: 250px; z-index: 1"
        data-win-control="MicrosoftNSJS.Advertising.AdControl"
        data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1', adUnitId: 'test'}">
    </div>
    ```

9.  アプリをコンパイルして実行し、広告が表示されることを確認します。

次の例は、シンプルなアプリの index.html 全体を示しています。

``` HTML
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <title>AdControlExampleApp</title>
    <!-- WinJS references -->
    <link href="lib/winjs-4.0.1/css/ui-light.css" rel="stylesheet" />
    <script src="lib/winjs-4.0.1/js/base.js"></script>
    <script src="lib/winjs-4.0.1/js/ui.js"></script>
    <!-- AdControlExampleApp references -->
    <link href="css/default.css" rel="stylesheet" />
    <script src="js/main.js"></script>
    <!-- Required reference for AdControl -->
    <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
</head>
<body>
    <div id="myAd" style="position: absolute; top: 50px; left: 0px; width: 300px; height: 250px; z-index: 1"
      data-win-control="MicrosoftNSJS.Advertising.AdControl"
      data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1', adUnitId: 'test'}">
    </div>
    <p>Content goes here</p>
</body>
</html>
```

### <a name="create-an-adcontrol-programmatically-in-javascript"></a>JavaScript でのプログラムによる AdControl の作成

前の手順では、HTML マークアップで **AdControl** を宣言する方法を示しています。 代わりに、JavaScript を使ってプログラムで **AdControl** を作成できます。 次の例では、HTML に **div** が既にあることを前提にしています。ID は **myAd** です。

> [!div class="tabbedCodeSnippets"]
:::code language="javascript" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/AdControlSamples/js/main.js" id="DeclareAdControl":::

この例では、**myAdError**、**myAdRefreshed**、**myAdEngagedChanged** という名前のイベント ハンドラー メソッドが既に宣言されていることを前提としています。

このコードで広告が表示されない場合は、**AdControl** を含む **div** に **position:relative** の属性を挿入してみてください。 これにより、**IFrame** の既定の設定が上書きされます。 この属性の値が原因でなければ、広告が正しく表示されるようになります。 新しい広告ユニットが利用可能になるまでに最大で 30 分かかる場合があることに注意してください。

> [!NOTE]
> この例の *applicationId* の値と *adUnitId* の値は、[テスト モードの値](set-up-ad-units-in-your-app.md#test-ad-units)です。 アプリを送信する前に、これらの値をパートナーセンターの [ライブ値に置き換える](set-up-ad-units-in-your-app.md#live-ad-units) 必要があります。

<span id="release" />

## <a name="release-your-app-with-live-ads"></a>ライブ広告を表示するアプリをリリースする

1. アプリでのバナー広告の使用方法が[バナー広告のガイドライン](ui-and-user-experience-guidelines.md#guidelines-for-banner-ads)に従っていることを確認します。

1.  パートナーセンターで、 [アプリ内広告](../publish/in-app-ads.md) ページにアクセスし、 [広告ユニットを作成](set-up-ad-units-in-your-app.md#live-ad-units)します。 広告ユニットの種類として、**[バナー]** を指定します。 広告ユニット ID とアプリケーション ID の両方をメモしておきます。
    > [!NOTE]
    > テスト広告ユニットとライブ UWP 広告ユニットでは、アプリケーション ID の値の形式が異なります。 テスト アプリケーション ID の値は GUID です。 パートナーセンターでライブ UWP ad ユニットを作成すると、ad ユニットの [アプリケーション ID] の値は、常にアプリのストア ID と一致します (たとえば、ストア ID 値は9NBLGGH4R315 のようになります)。

2. 必要に応じて、[[アプリ内広告]](../publish/in-app-ads.md) ページの [[仲介設定]](../publish/in-app-ads.md#mediation) セクションで設定を構成することで、**AdControl** の広告仲介を有効にできます。 広告仲介を使うと、複数の広告ネットワークから広告を表示して、広告収益とアプリ プロモーションの機能を最大限に引き出すことができます。表示される広告には、Taboola や Smaato などの他の有料広告ネットワークからの広告や、Microsoft のアプリ プロモーション キャンペーン用の広告などが含まれます。

3.  コードで、テスト ad の単位の値 (**applicationId** と **adUnitId**) を、パートナーセンターで生成したライブの値に置き換えます。

4.  パートナーセンターを使用して、ストアに[アプリを送信](../publish/app-submissions.md)します。

5.  パートナーセンターで、 [広告のパフォーマンスレポート](../publish/advertising-performance-report.md) を確認します。             

<span id="manage" />

## <a name="manage-ad-units-for-multiple-ad-controls-in-your-app"></a>アプリで複数の広告コントロールの広告ユニットを管理する

1 つのアプリで複数の **AdControl** オブジェクトを使うことができます (たとえば、アプリの各ページに異なる **AdControl** オブジェクトをホストできます)。 このシナリオでは、各コントロールに異なる広告ユニットを割り当てることをお勧めします。 各コントロールに異なる広告ユニットを使用することで、別々に[仲介の設定を構成](../publish/in-app-ads.md#mediation)して、個別の[報告データ](../publish/advertising-performance-report.md)を取得することが可能です。 また、これにより、Microsoft のサービスはアプリに提供する広告を最適化できます。

> [!IMPORTANT]
> 各広告ユニットは 1 つのアプリのみで使用できます。 同じ広告ユニットを複数のアプリで使うと、その広告ユニットには広告が配信されません。

## <a name="related-topics"></a>関連トピック

* [バナー広告のガイドライン](ui-and-user-experience-guidelines.md#guidelines-for-banner-ads)
* [GitHub の広告サンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising)
* [アプリの広告ユニットをセットアップする](set-up-ad-units-in-your-app.md)
* [JavaScript ウォークスルーでのエラー処理](error-handling-in-javascript-walkthrough.md)
