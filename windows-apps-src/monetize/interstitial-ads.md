---
ms.assetid: 1f970d38-2338-470e-b5ba-811402752fc4
description: Microsoft Advertising SDK を使用して Windows 10 用 UWP アプリにスポット広告を組み込む方法について説明します。
title: スポット広告
ms.date: 02/18/2020
ms.topic: article
keywords: Windows 10, UWP, 広告, 宣伝, 広告コントロール, スポット
ms.localizationpriority: medium
ms.openlocfilehash: eedb3f12bfc9da51afb2d5205122cbd42d7b31bf
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363095"
---
# <a name="interstitial-ads"></a>スポット広告

>[!WARNING]
> 2020年6月1日から、Microsoft Ad 収益化 platform for Windows UWP アプリがシャットダウンされます。 [詳細情報](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

このチュートリアルでは、Windows 10 用のユニバーサル Windows プラットフォーム (UWP) アプリとゲームにスポット広告を組み込む方法について説明します。 C# と C++ を使って JavaScript/HTML アプリと XAML アプリにスポット広告を追加する方法を示す完全なサンプル プロジェクトについては、[GitHub の広告サンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising)をご覧ください。

<span id="whatareinterstitialads10"/>

## <a name="what-are-interstitial-ads"></a>スポット広告とは

アプリやゲームの UI の一部分に表示が限定される標準のバナー広告とは異なり、スポット広告は画面全体に表示されます。 通常、ゲームでは、2 つの基本的な形式が使用されます。

* *ペイウォール*広告の場合、ユーザーは一定の間隔で広告を視聴する必要があります。 たとえば、ゲームのレベル間に表示される広告が該当します。

    ![whatisaninterstitial](images/13-ed0a333b-0fc8-4ca9-a4c8-11e8b4392831.png)

* *報酬ベース*の広告の場合、ユーザーは明示的に特定のメリット (レベルを完了するためのヒントや追加の時間など) を求めていて、アプリのユーザー インターフェイスから広告を初期化します。

アプリやゲームで使用するために、次の 2 種類のスポット広告が用意されています。**ビデオ スポット広告**と**バナー スポット広告**です。

> [!NOTE]
> スポット広告用の API は、ビデオの再生時を除き、どのようなユーザー インターフェイスも処理しません。 スポット広告をアプリに統合する方法を検討するときは、何をすべきであり何をすべきでないかに関するガイドラインとして、[スポットのベスト プラクティス](ui-and-user-experience-guidelines.md#interstitialbestpractices10)をご覧ください。

## <a name="prerequisites"></a>前提条件

* Visual Studio 2015 以降の Visual Studio のリリースと共に [Microsoft Advertising SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK) をインストールします。 インストール手順については、[この記事](install-the-microsoft-advertising-libraries.md)をご覧ください。

## <a name="integrate-an-interstitial-ad-into-your-app"></a>スポット広告をアプリに統合する

アプリでスポット広告を表示するには、次のプロジェクトの種類の指示に従います。

* [XAML/.NET](#interstitialadsxaml10)
* [HTML/JavaScript](#interstitialadshtml10)
* [C++ (DirectX Interop)](#interstitialadsdirectx10)

<span id="interstitialadsxaml10"/>

### <a name="xamlnet"></a>XAML/.NET

ここでは C# の例を紹介していますが、XAML/.NET プロジェクトでは Visual Basic と C++ もサポートされています。 完全な C# コードの例については、「[C# を使ったスポット広告のサンプル コード](interstitial-ad-sample-code-in-c.md)」をご覧ください。

1. Visual Studio でプロジェクトを開きます。
    > [!NOTE]
    > 既存のプロジェクトを使用している場合、プロジェクトの Package.appxmanifest ファイルを開き、**インターネット (クライアント)** 機能が選択されていることを確認します。 アプリでは、テスト広告やライブ広告を受信するためにこの機能が必要になります。

2. プロジェクトのターゲットが **[Any CPU]** (任意の CPU) になっている場合は、アーキテクチャ固有のビルド出力 (たとえば、**[x86]**) を使うようにプロジェクトを更新します。 プロジェクトのターゲットが **[Any CPU]** (任意の CPU) になっていると、次の手順で Microsoft Advertising ライブラリへの参照を正常に追加できません。 詳しくは、「[プロジェクトのターゲットを "Any CPU" に設定すると参照エラーが発生する](known-issues-for-the-advertising-libraries.md#reference_errors)」をご覧ください。

3. プロジェクトで Microsoft Advertising SDK への参照を追加します。

    1. **[ソリューション エクスプローラー]** ウィンドウで、**[参照設定]** を右クリックし、**[参照の追加]** を選択します。
    2.  **[参照マネージャー]** で、**[Universal Windows]** を展開し、**[拡張]** をクリックして、**[Microsoft Advertising SDK for XAML]** (バージョン 10.0) の横にあるチェック ボックスをオンにします。
    3.  **[参照マネージャー]** で、[OK] をクリックします。

3.  アプリの適切なコード ファイル (たとえば、MainPage.xaml.cs またはその他のページのコード ファイル) に、次の名前空間の参照を追加します。

    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs" id="Snippet1":::

4.  アプリの適切な場所 (たとえば、```MainPage``` またはその他のページ) で、[InterstitialAd](/uwp/api/microsoft.advertising.winrt.ui.interstitialad) オブジェクトと、スポット広告のアプリケーション ID および広告ユニット ID を表す複数の文字列フィールドを宣言します。 次のコード例では、`myAppId` フィールドと `myAdUnitId` フィールドをスポット広告の[テスト値](set-up-ad-units-in-your-app.md#test-ad-units)に割り当てています。

    > [!NOTE]
    > 各 **InterstitialAd** に、対応する*広告ユニット*があります。広告ユニットは、コントロールに広告を提供するためにサービスで使用されます。すべての広告ユニットは、*広告ユニット ID* と*アプリケーション ID* で構成されます。 ここでは、広告ユニット ID とアプリケーション ID のテスト値をコントロールに割り当てます。 これらのテスト値は、テスト バージョンのアプリでのみ使用できます。 ストアにアプリを発行する前に、 [これらのテスト値をパートナーセンターのライブ値に置き換える](#release) 必要があります。

    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs" id="Snippet2":::

5.  起動時に実行されるコード (たとえば、ページのコンストラクター) 内で、**InterstitialAd** オブジェクトをインスタンス化し、このオブジェクトのイベント用のイベント ハンドラーを関連付けます。

    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs" id="Snippet3":::

6.  *ビデオ スポット*広告を表示する場合: 広告が必要になる約 30 ～ 60 秒前に、[RequestAd](/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) メソッドを使用して事前に広告を取得しておきます。 これにより、広告を表示する前に、その広告を要求して準備するための十分な時間が与えられます。 Ad の種類には必ず **Adtype. ビデオ** を指定してください。

    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs" id="Snippet4":::

    *バナー スポット*広告を表示する場合: 広告が必要になる約 5 ～ 8 秒前に、[RequestAd](/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) メソッドを使用して事前に広告を取得しておきます。 これにより、広告を表示する前に、その広告を要求して準備するための十分な時間が与えられます。 Ad 型には必ず **Adtype** を指定してください。

    ```csharp
    myInterstitialAd.RequestAd(AdType.Display, myAppId, myAdUnitId);
    ```

6.  ビデオ スポット広告やバナー スポット広告を表示するコード内のポイントで、**InterstitialAd** を表示する準備ができていることを確認してから、[Show](/uwp/api/microsoft.advertising.winrt.ui.interstitialad.show) メソッドを使用して広告を表示します。

    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs" id="Snippet5":::

7.  **InterstitialAd** オブジェクトのイベント ハンドラーを定義します。

    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs" id="Snippet6":::

8.  アプリをビルドした後テストして、テスト広告が表示されることを確認します。

<span id="interstitialadshtml10"/>

### <a name="htmljavascript"></a>HTML/JavaScript

次の手順では、Visual Studio で JavaScript 用のユニバーサル Windows プロジェクトを作成済みであり、特定の CPU をターゲットとしているものと想定しています。 完全なコード サンプルについては、「[JavaScript を使ったスポット広告のサンプル コード](interstitial-ad-sample-code-in-javascript.md)」をご覧ください。

1. Visual Studio でプロジェクトを開きます。

2. プロジェクトのターゲットが **[Any CPU]** (任意の CPU) になっている場合は、アーキテクチャ固有のビルド出力 (たとえば、**[x86]**) を使うようにプロジェクトを更新します。 プロジェクトのターゲットが **[Any CPU]** (任意の CPU) になっていると、次の手順で Microsoft Advertising ライブラリへの参照を正常に追加できません。 詳しくは、「[プロジェクトのターゲットを "Any CPU" に設定すると参照エラーが発生する](known-issues-for-the-advertising-libraries.md#reference_errors)」をご覧ください。

3. プロジェクトで Microsoft Advertising SDK への参照を追加します。

    1. **[ソリューション エクスプローラー]** ウィンドウで、**[参照設定]** を右クリックし、**[参照の追加]** を選択します。
    2.  **[参照マネージャー]** で、**[ユニバーサル Windows]** を展開し、**[拡張]** をクリックして、**[Microsoft Advertising SDK for JavaScript]** (バージョン 10.0) の横にあるチェック ボックスをオンにします。
    3.  **[参照マネージャー]** で、[OK] をクリックします。

3.  プロジェクトの HTML ファイルの** &lt; 先頭 &gt; **セクションで、プロジェクトの JavaScript による .css および default.js の参照の後に、ad.js への参照を追加します。

    ``` HTML
    <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
    ```

4.  プロジェクト内の .js ファイルで、[InterstitialAd](/uwp/api/microsoft.advertising.winrt.ui.interstitialad) オブジェクトと、スポット広告のアプリケーション ID および広告ユニット ID を含む複数のフィールドを宣言します。 次のコード例では、`applicationId` フィールドと `adUnitId` フィールドをスポット広告の[テスト値](set-up-ad-units-in-your-app.md#test-ad-units)に割り当てています。

    > [!NOTE]
    > 各 **InterstitialAd** に、対応する*広告ユニット*があります。広告ユニットは、コントロールに広告を提供するためにサービスで使用されます。すべての広告ユニットは、*広告ユニット ID* と*アプリケーション ID* で構成されます。 ここでは、広告ユニット ID とアプリケーション ID のテスト値をコントロールに割り当てます。 これらのテスト値は、テスト バージョンのアプリでのみ使用できます。 ストアにアプリを発行する前に、 [これらのテスト値をパートナーセンターのライブ値に置き換える](#release) 必要があります。

    :::code language="javascript" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/js/script.js" id="Snippet1":::

5.  起動時に実行されるコード (たとえば、ページのコンストラクター) 内で、**InterstitialAd** オブジェクトをインスタンス化し、このオブジェクトのイベント ハンドラーを関連付けます。

    :::code language="javascript" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/js/script.js" id="Snippet2":::

5. *ビデオ スポット*広告を表示する場合: 広告が必要になる約 30 ～ 60 秒前に、[RequestAd](/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) メソッドを使用して事前に広告を取得しておきます。 これにより、広告を表示する前に、その広告を要求して準備するための十分な時間が与えられます。 広告の種類として、必ず **InterstitialAdType.video** を指定してください。

    :::code language="javascript" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/js/script.js" id="Snippet3":::

    *バナー スポット*広告を表示する場合: 広告が必要になる約 5 ～ 8 秒前に、[RequestAd](/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) メソッドを使用して事前に広告を取得しておきます。 これにより、広告を表示する前に、その広告を要求して準備するための十分な時間が与えられます。 広告の種類として、必ず **InterstitialAdType.display** を指定してください。

    ```js
    if (interstitialAd) {
        interstitialAd.requestAd(MicrosoftNSJS.Advertising.InterstitialAdType.display, applicationId, adUnitId);
    }
    ```

6.  広告を表示するコード内のポイントで、**InterstitialAd** の表示準備が整っていることを確認した後、[Show](/uwp/api/microsoft.advertising.winrt.ui.interstitialad.show) メソッドを使用して広告を表示します。

    :::code language="javascript" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/js/samples.js" id="Snippet4":::

7.  **InterstitialAd** オブジェクトのイベント ハンドラーを定義します。

    :::code language="javascript" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/js/samples.js" id="Snippet5":::

9.  アプリをビルドした後テストして、テスト広告が表示されることを確認します。

<span id="interstitialadsdirectx10"/>

### <a name="c-directx-interop"></a>C++ (DirectX Interop)

このサンプルでは、Visual Studio で C++ **DirectX および XAML アプリ (ユニバーサル Windows)** プロジェクトを作成済みであり、特定の CPU アーキテクチャをターゲットとしているものと想定しています。
 
1. Visual Studio でプロジェクトを開きます。

3. プロジェクトで Microsoft Advertising SDK への参照を追加します。

    1. **[ソリューション エクスプローラー]** ウィンドウで、**[参照設定]** を右クリックし、**[参照の追加]** を選択します。
    2.  **[参照マネージャー]** で、**[Universal Windows]** を展開し、**[拡張]** をクリックして、**[Microsoft Advertising SDK for XAML]** (バージョン 10.0) の横にあるチェック ボックスをオンにします。
    3.  **[参照マネージャー]** で、[OK] をクリックします。

2.  アプリの適切なヘッダー ファイル (例: DirectXPage.xaml.h) 内で、[InterstitialAd](/uwp/api/microsoft.advertising.winrt.ui.interstitialad) オブジェクトと関連するイベント ハンドラー メソッドを宣言します。  

    :::code language="cpp" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.h" id="Snippet1":::

3.  同じヘッダー ファイル内で、スポット広告のアプリケーション ID と広告ユニット ID を表す複数の文字列フィールドを宣言します。 次のコード例では、`myAppId` フィールドと `myAdUnitId` フィールドをスポット広告の[テスト値](set-up-ad-units-in-your-app.md#test-ad-units)に割り当てています。

    > [!NOTE]
    > 各 **InterstitialAd** に、対応する*広告ユニット*があります。広告ユニットは、コントロールに広告を提供するためにサービスで使用されます。すべての広告ユニットは、*広告ユニット ID* と*アプリケーション ID* で構成されます。 ここでは、広告ユニット ID とアプリケーション ID のテスト値をコントロールに割り当てます。 これらのテスト値は、テスト バージョンのアプリでのみ使用できます。 ストアにアプリを発行する前に、 [これらのテスト値をパートナーセンターのライブ値に置き換える](#release) 必要があります。

    :::code language="cpp" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.h" id="Snippet2":::

4.  スポット広告を表示するためのコードを追加する .cpp file に、次の名前空間の参照を追加します。 次の例では、アプリの DirectXPage.xaml.cpp ファイルにそのコードを追加しているものと想定しています。

    :::code language="cpp" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp" id="Snippet3":::

6.  起動時に実行されるコード (たとえば、ページのコンストラクター) 内で、**InterstitialAd** オブジェクトをインスタンス化し、このオブジェクトのイベント用のイベント ハンドラーを関連付けます。 次の例では、```InterstitialAdSamplesCpp``` がプロジェクトの名前空間です。コードでは、必要に応じてこの名前を変更します。

    :::code language="cpp" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp" id="Snippet4":::

7. *ビデオ スポット*広告を表示する場合: スポット広告が必要になる約 30 ～ 60 秒前に、[RequestAd](/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) メソッドを使用して事前に広告を取得しておきます。 これにより、広告を表示する前に、その広告を要求して準備するための十分な時間が与えられます。 広告の種類として、必ず **AdType::Video** を指定してください。

    :::code language="cpp" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp" id="Snippet5":::

    *バナー スポット*広告を表示する場合: 広告が必要になる約 5 ～ 8 秒前に、[RequestAd](/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) メソッドを使用して事前に広告を取得しておきます。 これにより、広告を表示する前に、その広告を要求して準備するための十分な時間が与えられます。 広告の種類として、必ず **AdType::Display** を指定してください。

    ```cpp
    m_interstitialAd->RequestAd(AdType::Display, myAppId, myAdUnitId);
    ```

7.  広告を表示するコード内のポイントで、**InterstitialAd** の表示準備が整っていることを確認した後、[Show](/uwp/api/microsoft.advertising.winrt.ui.interstitialad.show) メソッドを使用して広告を表示します。

    :::code language="cpp" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp" id="Snippet6":::

8.  **InterstitialAd** オブジェクトのイベント ハンドラーを定義します。

    :::code language="cpp" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp" id="Snippet7":::

9. アプリをビルドした後テストして、テスト広告が表示されることを確認します。

<span id="release" />

## <a name="release-your-app-with-live-ads"></a>ライブ広告を表示するアプリをリリースする

1. アプリでのスポット広告の使用方法が[スポット広告のガイドライン](ui-and-user-experience-guidelines.md#interstitialbestpractices10)に従っていることを確認します。

2.  パートナーセンターで、 [アプリ内広告](../publish/in-app-ads.md) ページにアクセスし、 [広告ユニットを作成](set-up-ad-units-in-your-app.md#live-ad-units)します。 表示するスポット広告の種類に応じて、広告ユニットの種類として、**[ビデオ (スポット)]** または **[バナー (スポット)]** を選択します。 広告ユニット ID とアプリケーション ID の両方をメモしておきます。
    > [!NOTE]
    > テスト広告ユニットとライブ UWP 広告ユニットでは、アプリケーション ID の値の形式が異なります。 テスト アプリケーション ID の値は GUID です。 パートナーセンターでライブ UWP ad ユニットを作成すると、ad ユニットの [アプリケーション ID] の値は、常にアプリのストア ID と一致します (たとえば、ストア ID 値は9NBLGGH4R315 のようになります)。

3. 必要に応じて、[[アプリ内広告]](../publish/in-app-ads.md) ページの [[仲介設定]](../publish/in-app-ads.md#mediation) セクションで設定を構成することで、**InterstitialAd** の広告仲介を有効にできます。 広告仲介を使うと、複数の広告ネットワークから広告を表示して、広告収益とアプリ プロモーションの機能を最大限に引き出すことができます。表示される広告には、Taboola や Smaato などの他の有料広告ネットワークからの広告や、Microsoft のアプリ プロモーション キャンペーン用の広告などが含まれます。

4.  コードで、テスト ad の単位の値を、パートナーセンターで生成したライブ値に置き換えます。

5.  パートナーセンターを使用して、ストアに[アプリを送信](../publish/app-submissions.md)します。

6.  パートナーセンターで、 [広告のパフォーマンスレポート](../publish/advertising-performance-report.md) を確認します。

<span id="manage" />

## <a name="manage-ad-units-for-multiple-interstitial-ad-controls-in-your-app"></a>アプリで複数のスポット広告コントロールの広告ユニットを管理します。

1 つのアプリに複数の **InterstitialAd** コントロールを使用できます。 このシナリオでは、各コントロールに異なる広告ユニットを割り当てることをお勧めします。 各コントロールに異なる広告ユニットを使用することで、別々に[仲介の設定を構成](../publish/in-app-ads.md#mediation)して、個別の[報告データ](../publish/advertising-performance-report.md)を取得することが可能です。 また、これにより、Microsoft のサービスはアプリに提供する広告を最適化できます。

> [!IMPORTANT]
> 各広告ユニットは 1 つのアプリのみで使用できます。 同じ広告ユニットを複数のアプリで使うと、その広告ユニットには広告が配信されません。

## <a name="related-topics"></a>関連トピック

* [スポット広告のガイドライン](ui-and-user-experience-guidelines.md#interstitialbestpractices10)
* [C# を使ったスポット広告のサンプル コード](interstitial-ad-sample-code-in-c.md)
* [JavaScript を使ったスポット広告のサンプル コード](interstitial-ad-sample-code-in-javascript.md)
* [GitHub の広告サンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising)
* [アプリの広告ユニットをセットアップする](set-up-ad-units-in-your-app.md)
