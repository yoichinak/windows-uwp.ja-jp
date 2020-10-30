---
description: UWP アプリからカスタムイベントをログに記録し、パートナーセンターの使用状況レポートでこれらのイベントを確認することができます。
title: パートナー センターのカスタム イベントをログに記録する
ms.date: 06/01/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store Services SDK, イベントをログ記録
ms.assetid: 4aa591e0-c22a-4c90-b316-0b5d0410af19
ms.localizationpriority: medium
ms.openlocfilehash: 3cc89272984ffdda47c488e7bb2f24b4f37d3a41
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2020
ms.locfileid: "93033455"
---
# <a name="log-custom-events-for-partner-center"></a>パートナー センターのカスタム イベントをログに記録する

パートナーセンターの [使用状況レポート](../publish/usage-report.md) では、ユニバーサル WINDOWS プラットフォーム (UWP) アプリで定義したカスタムイベントに関する情報を取得できます。 カスタムイベントは、アプリ内のイベントやアクティビティを表す任意の文字列です。 たとえば、ゲームで *firstLevelPassed* 、 *secondLevelPassed* という名前のカスタム イベントを定義して、ユーザーがゲームの各レベルをクリアしたときに記録されるようにできます。

アプリからのカスタム イベントをログに記録するには、カスタム イベントの文字列を Microsoft Store Services SDK で提供されている [Log](/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log) メソッドに渡します。 カスタムイベントの合計発生回数は、パートナーセンターの [使用状況レポート](../publish/usage-report.md)の [ **カスタムイベント** ] セクションで確認できます。

> [!NOTE]
> パートナーセンターにログを記録するカスタムイベントは、 [Windows イベント](/windows/desktop/Events/windows-events)とは無関係であり、 **イベントビューアー** には表示されません。

## <a name="prerequisites"></a>前提条件

パートナーセンターでアプリの **使用状況レポート** のカスタムログイベントを確認する前に、アプリをストアで公開する必要があります。

## <a name="how-to-log-custom-events"></a>カスタム イベントをログに記録する方法

1. Microsoft Store Services SDK を開発用コンピューターにインストールしていない場合には、[Microsoft Store Services SDK をインストール](microsoft-store-services-sdk.md#install-the-sdk)します。

2. Visual Studio でプロジェクトを開きます。

3. ソリューション エクスプローラーで、プロジェクトの **[参照設定]** ノードを右クリックし、 **[参照の追加]** をクリックします。

4. **[参照マネージャー]** で、 **[ユニバーサル Windows]** を展開し、 **[拡張機能]** をクリックします。

5. SDK の一覧で、 **[Microsoft Engagement Framework]** の横にあるチェック ボックスをオンにして、 **[OK]** をクリックします。

6. カスタム イベントを記録する各コード ファイルの先頭に、次のステートメントを追加します。
    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreSDKSamples/cs/LogEvents.cs" id="EngagementNamespace":::

7. カスタム イベントのログを記録するコードの各セクションで、[StoreServicesCustomEventLogger](/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log) オブジェクトを取得し、[Log](/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log) メソッドを呼び出します。 カスタム イベント文字列をメソッドに渡します。
    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreSDKSamples/cs/LogEvents.cs" id="Log":::

    > [!NOTE]
    > アプリで長い名前を持つ多くのカスタム イベントをログに記録する場合は、[[使用状況] レポート](../publish/usage-report.md)の読み込みに時間がかかることもあります。 カスタム イベントには簡単な名前を使用することをお勧めします。 

## <a name="related-topics"></a>関連トピック

* [利用状況レポート](../publish/usage-report.md)
* [Log メソッド](/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log)
* [Microsoft Store Services SDK](./microsoft-store-services-sdk.md)
