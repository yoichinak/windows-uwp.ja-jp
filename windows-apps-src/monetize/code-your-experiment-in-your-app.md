---
Description: ユニバーサル Windows プラットフォーム (UWP) アプリで A/B テストを実行するには、アプリで実験用のコードを記述する必要があります。
title: アプリの実験用のコードを記述する
ms.assetid: 6A5063E1-28CD-4087-A4FA-FBB511E9CED5
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store Services SDK, A/B テスト, 実験
ms.localizationpriority: medium
ms.openlocfilehash: dbdd95ab0d4ecde5fbe5cfb8d84d2d328b4c5a24
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363665"
---
# <a name="code-your-app-for-experimentation"></a>アプリの実験用のコードを記述する

[パートナーセンターでプロジェクトを作成し、リモート変数を定義](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)したら、ユニバーサル WINDOWS プラットフォーム (UWP) アプリのコードを次のように更新することができます。
* パートナーセンターからリモート変数値を受信します。
* リモート変数を使用して、ユーザーのアプリ エクスペリエンスを構成する。
* ユーザーが実験を表示し、目的のアクション ( *変換*とも呼ばれます) を実行したことを示すイベントをパートナーセンターに記録します。

この動作をアプリに追加するには、Microsoft Store Services SDK によって提供される API を使用します。

以下のセクションでは、実験のバリエーションを取得し、イベントをパートナーセンターに記録する一般的なプロセスについて説明します。 実験のためにアプリをコーディングした後、 [パートナーセンターで実験を定義](define-your-experiment-in-the-dev-center-dashboard.md)できます。 実験の作成と実行のプロセスについて詳しく示すチュートリアルについては、「[A/B テストを使用して最初の実験を作成および実行する](create-and-run-your-first-experiment-with-a-b-testing.md)」をご覧ください。

> [!NOTE]
> Microsoft Store Services SDK の実験 Api の中には、パートナーセンターからデータを取得するために [非同期パターン](../threading-async/asynchronous-programming-universal-windows-platform-apps.md) を使用するものがあります。 これは、これらのメソッドの一部が、メソッドが呼び出された後に実行されることを意味し、アプリの UI は、操作が完了するまでの間、高い応答性を維持できます。 この記事のコード例に示すように、非同期パターンでは、アプリで API を呼び出すときに、**async** キーワードと **await** 演算子を使用する必要があります。 慣例により、非同期メソッドの末尾には **Async** が付きます。

## <a name="configure-your-project"></a>プロジェクトを構成する

最初に、Microsoft Store Services SDK を開発用コンピューターにインストールして、プロジェクトに必要な参照を追加します。

1. [Microsoft Store SERVICES SDK をインストール](microsoft-store-services-sdk.md#install-the-sdk)します。
2. Visual Studio でプロジェクトを開きます。
3. ソリューション エクスプローラーで、プロジェクト ノードを展開し、**[参照設定]** を右クリックして **[参照の追加]** をクリックします。
3. **[参照マネージャー]** で、**[ユニバーサル Windows]** を展開し、**[拡張機能]** をクリックします。
4. SDK の一覧で、**[Microsoft Engagement Framework]** の横にあるチェック ボックスをオンにして、**[OK]** をクリックします。

> [!NOTE]
> この記事のコード例では、コード ファイルに **System.Threading.Tasks** 名前空間と **Microsoft.Services.Store.Engagement** 名前空間の **using** ステートメントがあることを前提としています。

## <a name="get-variation-data-and-log-the-view-event-for-your-experiment"></a>バリエーション データを取得し、実験のビュー イベントをログに記録する

プロジェクトで、試験的機能の実行において変更する機能のコードを探します。 バリエーションのデータを取得し、このデータを使用してテスト対象の機能の動作を変更し、実験のビューイベントをパートナーセンターの A/B テストサービスに記録するコードを追加します。

必要な特定のコードは、アプリによって異なりますが、次の例は基本的なプロセスを示しています。 完全なコード例については、「[A/B テストを使用して最初の実験を作成および実行する](create-and-run-your-first-experiment-with-a-b-testing.md)」をご覧ください。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreSDKSamples/cs/ExperimentExamples.cs" id="ExperimentCodeSample":::

次の手順では、このプロセスの重要な部分について詳しく説明します。

1. 現在のバリエーション割り当てと、パートナーセンターへのビューおよび変換イベントをログに記録するために使用する[StoreServicesCustomEventLogger](/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger)オブジェクトを表す[StoreServicesExperimentVariation](/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation)オブジェクトを宣言します。

    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreSDKSamples/cs/ExperimentExamples.cs" id="Snippet1":::

2. 取得する実験の[プロジェクト ID](run-app-experiments-with-a-b-testing.md#terms) に割り当てられる文字列変数を宣言します。
    > [!NOTE]
    > [パートナーセンターでプロジェクトを作成](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)するときに、プロジェクト ID を取得します。 次に示すプロジェクト ID は例示のためのものです。

    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreSDKSamples/cs/ExperimentExamples.cs" id="Snippet2":::

3. 静的な [GetCachedVariationAsync](/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getcachedvariationasync) メソッドを呼び出して、実験に対するキャッシュされた現在のバリエーションの割り当てを取得し、実験のプロジェクトID をそのメソッドに渡します。 このメソッドは、[ExperimentVariation](/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariationresult.experimentvariation) プロパティを使用してバリエーションの割り当てへのアクセスを提供する [StoreServicesExperimentVariationResult](/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariationresult) オブジェクトを返します。

    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreSDKSamples/cs/ExperimentExamples.cs" id="Snippet3":::

4. [IsStale](/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.isstale) プロパティを確認して、キャッシュされたバリエーションの割り当てを、サーバーからリモートのバリエーションの割り当てによって更新する必要があるかどうかを判断します。 更新する必要がある場合は、静的な [GetRefreshedVariationAsync](/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getrefreshedvariationasync) メソッドを呼び出して、サーバーから更新されたバリエーションの割り当てを確認し、ローカルでキャッシュされたバリエーションを更新します。

    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreSDKSamples/cs/ExperimentExamples.cs" id="Snippet4":::

5. [StoreServicesExperimentVariation](/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation) オブジェクトの [GetBoolean](/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getboolean)、[GetDouble](/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getdouble)、[GetInt32](/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getint32)、または [GetString](/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getstring) の各メソッドを使用して、バリエーションの割り当ての値を取得します。 各方法では、最初のパラメーターは取得するバリエーションの名前です (これは、パートナーセンターで入力したバリエーションと同じ名前です)。 2番目のパラメーターは、パートナーセンターから指定された値を取得できない場合 (たとえば、ネットワークに接続されていない場合) に、メソッドが返す既定値です。そのため、キャッシュされたバージョンのバリエーションを使用できません。

    次の例では、[GetString](/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getstring) を使用して、*buttonText* という名前の変数を取得し、**Grey Button** の既定の変数値を指定します。

    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreSDKSamples/cs/ExperimentExamples.cs" id="Snippet5":::

6. コードで、変数値を使用して、テストする機能の動作を変更します。 たとえば、次のコードでは *buttonText* 値を、アプリ内のボタンのコンテンツに割り当てます。 この例では、プロジェクト内の別の場所で既にこのボタンを定義していることを前提としています。

    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreSDKSamples/cs/ExperimentExamples.cs" id="Snippet6":::

7. 最後に、パートナーセンターの A/B テストサービスに実験の [ビューイベント](run-app-experiments-with-a-b-testing.md#terms) を記録します。 ```logger``` フィールドを [StoreServicesCustomEventLogger](/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger) オブジェクトに初期化し、[LogForVariation](/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.logforvariation) メソッドを呼び出します。 現在のバリエーション割り当てを表す [StoreServicesExperimentVariation](/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation) オブジェクト (このオブジェクトは、パートナーセンターへのイベントに関するコンテキストを提供します) と実験のビューイベントの名前を渡します。 これは、パートナーセンターで実験に入力するビューイベント名と一致する必要があります。 コードでは、実験の一部であるバリエーションをユーザーが最初に表示するタイミングを示すビュー イベントをログに記録する必要があります。

    次の例では、**userViewedButton** という名前のビュー イベントをログに記録する方法を示します。 この例では、実験の目的は、ユーザーにアプリ内のボタンをクリックさせることであるため、ビュー イベントは、アプリがバリエーション データ (この場合、ボタンのテキスト) を取得し、それをボタンのコンテンツに割り当てた後にログに記録されます。

    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreSDKSamples/cs/ExperimentExamples.cs" id="Snippet7":::

## <a name="log-conversion-events-to-partner-center"></a>パートナーセンターへの変換イベントのログ記録

次に、 [変換イベント](run-app-experiments-with-a-b-testing.md#terms) をパートナーセンターの A/B テストサービスに記録するコードを追加します。 コードでは、ユーザーが実験の目標に達したときに、コンバージョン イベントのログを記録する必要があります。 必要なコードはアプリによって異なりますが、ここでは一般的な手順を示します。 完全なコード例については、「[A/B テストを使用して最初の実験を作成および実行する](create-and-run-your-first-experiment-with-a-b-testing.md)」をご覧ください。

1. 実験のいずれかのゴールの目的をユーザーが達成した場合に実行するコードで、[LogForVariation](/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.logforvariation) メソッドをもう一度呼び出して、[StoreServicesExperimentVariation](/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation) オブジェクトと実験のコンバージョン イベントの名前を渡します。 これは、パートナーセンターで実験に入力する変換イベント名のいずれかと一致する必要があります。

    次の例では、ボタンの **Click** イベント ハンドラーから **userClickedButton** という名前のコンバージョン イベントをログに記録します。 この例では、実験の目的は、ユーザーにボタンをクリックさせることです。

    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreSDKSamples/cs/ExperimentExamples.cs" id="Snippet8":::

## <a name="next-steps"></a>次のステップ

アプリの実験用のコードを記述したら、次の手順に進むことができます。
1. [パートナーセンターで実験を定義](define-your-experiment-in-the-dev-center-dashboard.md)します。 ビュー イベント、コンバージョン イベント、A/B テストの一意のバリエーションを定義する実験を作成します。
2. [パートナーセンターで実験を実行し、管理](manage-your-experiment.md)します。


## <a name="related-topics"></a>関連トピック

* [パートナーセンターでプロジェクトを作成し、リモート変数を定義する](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)
* [パートナー センターで実験を定義する](define-your-experiment-in-the-dev-center-dashboard.md)
* [パートナー センターで実験を管理する](manage-your-experiment.md)
* [A/B テストを使用して最初の試験的機能を作成および実行する](create-and-run-your-first-experiment-with-a-b-testing.md)
* [A/B テストを使用してアプリの実験を実行する](run-app-experiments-with-a-b-testing.md)
