---
Description: パートナーセンターから送信するプッシュ通知を受信するように UWP アプリを登録する方法について説明します。
title: ターゲット プッシュ通知用のアプリの構成
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10、uwp、Microsoft Store Services SDK、対象のプッシュ通知、パートナーセンター
ms.assetid: 30c832b7-5fbe-4852-957f-7941df8eb85a
ms.localizationpriority: medium
ms.openlocfilehash: abb901c1b067dcf3609cbfb5c4cf3f81c9dc465c
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/02/2020
ms.locfileid: "89364135"
---
# <a name="configure-your-app-for-targeted-push-notifications"></a>ターゲット プッシュ通知用のアプリの構成

パートナーセンターの [ **プッシュ通知** ] ページを使用すると、ユニバーサル WINDOWS プラットフォーム (UWP) アプリがインストールされているデバイスに対象のプッシュ通知を送信することで、顧客と直接やり取りできます。 たとえば、ターゲット プッシュ通知を使用して、ユーザーにアプリの評価や新しい機能の試用などの行動を促すことができます。 トースト通知、タイル通知、生の XML 通知など、さまざまな種類のプッシュ通知を送信できます。 また、プッシュ通知の結果としてのアプリの起動率を追跡することもできます。 この機能について詳しくは、「[アプリのユーザーにプッシュ通知を送信する](../publish/send-push-notifications-to-your-apps-customers.md)」をご覧ください。

パートナーセンターから対象のプッシュ通知を顧客に送信するには、Microsoft Store Services SDK の [StoreServicesEngagementManager](/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager) クラスのメソッドを使用して、通知を受信するようにアプリを登録する必要があります。 このクラスの追加のメソッドを使用して、対象のプッシュ通知に応答してアプリが起動されたことをパートナーセンターに通知できます (通知から発生したアプリの起動率を追跡し、通知の受信を停止する場合)。

## <a name="configure-your-project"></a>プロジェクトを構成する

コードを記述する前に、Microsoft Store Services SDK への参照をプロジェクトに追加するには、以下の手順を実行します。

1. Microsoft Store Services SDK を開発用コンピューターにインストールしていない場合には、[Microsoft Store Services SDK をインストール](microsoft-store-services-sdk.md#install-the-sdk)します。 
2. Visual Studio でプロジェクトを開きます。
3. ソリューション エクスプローラーで、プロジェクトの **[参照設定]** ノードを右クリックし、**[参照の追加]** をクリックします。
4. **[参照マネージャー]** で、**[ユニバーサル Windows]** を展開し、**[拡張機能]** をクリックします。
5. SDK の一覧で、**[Microsoft Engagement Framework]** の横にあるチェック ボックスをオンにして、**[OK]** をクリックします。

## <a name="register-for-push-notifications"></a>プッシュ通知に登録する

パートナーセンターから対象のプッシュ通知を受信するようにアプリを登録するには:

1. プロジェクトで、起動中に実行されるコード セクションを見つけます。このセクションで、通知を受信するようにアプリケーションを登録することができます。
2. コード ファイルの先頭に、次のステートメントを追加します。

    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreSDKSamples/cs/DevCenterNotifications.cs" id="EngagementNamespace":::

3. [StoreServicesEngagementManager](/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager) オブジェクトを取得し、先ほど見つけた起動コードの [RegisterNotificationChannelAsync](/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.registernotificationchannelasync) オーバーロードの 1 つを呼び出します。 このメソッドは、アプリを起動するたびに呼び出す必要があります。

  * パートナーセンターで通知用に独自のチャネル URI を作成する場合は、 [Registernotificationchannelasync ()](/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.registernotificationchannelasync) オーバーロードを呼び出します。

      :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreSDKSamples/cs/DevCenterNotifications.cs" id="RegisterNotificationChannelAsync1":::
      > [!IMPORTANT]
      > アプリが [CreatePushNotificationChannelForApplicationAsync](/uwp/api/windows.networking.pushnotifications.pushnotificationchannelmanager.createpushnotificationchannelforapplicationasync) も呼び出して WNS の通知チャネルを作成する場合、コードが [CreatePushNotificationChannelForApplicationAsync](/uwp/api/windows.networking.pushnotifications.pushnotificationchannelmanager.createpushnotificationchannelforapplicationasync) および [RegisterNotificationChannelAsync()](/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.registernotificationchannelasync) オーバーロードを同時に呼び出さないようにしてください・ これらのメソッドの両方を呼び出す必要がある場合は、それらを順番に呼び出すようにして、もう一方のメソッドを呼び出す前に別のメソッドの戻りを待つようにします。

  * パートナーセンターからの対象となるプッシュ通知に使用するチャネル URI を指定する場合は、 [Registernotificationchannelasync (StoreServicesNotificationChannelParameters)](/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.registernotificationchannelasync) オーバーロードを呼び出します。 たとえば、アプリが既に Windows プッシュ通知サービス (WNS) を使用していて、同じチャネル URI を使用する場合は、次のようにします。 まず [StoreServicesNotificationChannelParameters](/uwp/api/microsoft.services.store.engagement.storeservicesnotificationchannelparameters) オブジェクトを作成し、[CustomNotificationChannelUri](/uwp/api/microsoft.services.store.engagement.storeservicesnotificationchannelparameters.customnotificationchanneluri) プロパティをチャネル URI に割り当てる必要があります。

      :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreSDKSamples/cs/DevCenterNotifications.cs" id="RegisterNotificationChannelAsync2":::

> [!NOTE]
> **RegisterNotificationChannelAsync** メソッドを呼び出すと、MicrosoftStoreEngagementSDKId.txt という名前のファイルが、アプリのローカル アプリ データ ストア ([ApplicationData.LocalFolder](/uwp/api/Windows.Storage.ApplicationData.LocalFolder) プロパティによって返されるフォルダー) に作成されます。 このファイルには、ターゲット プッシュ通知インフラストラクチャで使用される ID が含まれています。 アプリがこのファイルを変更または削除しないことを確認してください。 ファイルの変更や削除が行われると、通知の複数のインスタンスを受け取ったり、他の方法で通知が正しく動作しない可能性があります。

<span id="notification-customers" />

### <a name="how-targeted-push-notifications-are-routed-to-customers"></a>ターゲット プッシュ通知をユーザーにルーティングする方法

アプリで **RegisterNotificationChannelAsync** を呼び出すと、このメソッドは、現在デバイスにサインインしているユーザーの Microsoft アカウントを収集します。 その後、この顧客を含むセグメントに対象のプッシュ通知を送信すると、パートナーセンターは、この顧客の Microsoft アカウントに関連付けられているデバイスに通知を送信します。

アプリを起動したユーザーが、Microsoft アカウントでデバイスにまだサインインしているときに、そのデバイスを他のユーザーに使用させた場合、他のユーザーが元のユーザーを対象とした通知を見る可能性があります。 これにより予期しない結果が起こる可能性があります (特に、ユーザーによるサインインが必要となるサービスを提供するアプリの場合)。 このシナリオで他のユーザーがターゲット通知を見ることができなくするには、ユーザーがアプリからからサインアウトするときに、[UnregisterNotificationChannelAsync](/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.unregisternotificationchannelasync) メソッドを呼び出します。 詳しくは、この記事の後半にある「[プッシュ通知の登録解除](#unregister)」をご覧ください。

### <a name="how-your-app-responds-when-the-user-launches-your-app"></a>ユーザーがアプリを起動したときのアプリの対応方法

アプリが通知を受信するように登録され、 [パートナーセンターからアプリの顧客にプッシュ通知を送信](../publish/send-push-notifications-to-your-apps-customers.md)すると、ユーザーがプッシュ通知に応答してアプリを起動すると、アプリ内の次のエントリポイントのいずれかが呼び出されます。 ユーザーがアプリを起動したときに実行するコードがある場合は、アプリのこれらのエントリ ポイントのいずれかにコードを追加できます。

  * プッシュ通知にフォアグラウンドのアクティブ化の種類がある場合には、プロジェクトの**アプリ**クラスの [OnActivated](/uwp/api/windows.ui.xaml.application.onactivated) メソッドを上書きして、このメソッドにコードを追加します。

  * プッシュ通知にバックグラウンドのアクティブ化の種類がある場合には、[バックグラウンド タスク](../launch-resume/support-your-app-with-background-tasks.md)の [Run](/uwp/api/windows.applicationmodel.background.ibackgroundtask.run) メソッドにコードを追加します。

たとえば、有料のアドオンを購入したアプリのユーザーに、特典として無料のアドオンを提供することができます。 この例では、対象とするユーザーの[ユーザー セグメント](../publish/create-customer-segments.md)にプッシュ通知を送信できます。 次に、無料の[アプリ内購入](in-app-purchases-and-trials.md)を提供するコードを、上記のエントリ ポイントのいずれかに追加できます。

## <a name="notify-partner-center-of-your-app-launch"></a>アプリの起動のパートナーセンターに通知する

パートナーセンターで対象のプッシュ通知に対して [ **アプリ起動率の追跡** ] オプションを選択した場合は、アプリの適切なエントリポイントから [ParseArgumentsAndTrackAppLaunch](/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.parseargumentsandtrackapplaunch) メソッドを呼び出して、プッシュ通知に応答してアプリが起動されたことをパートナーセンターに通知します。

このメソッドは、アプリの元の起動引数を返します。 プッシュ通知のアプリ起動率を追跡することを選択すると、非透過の追跡 ID が起動引数に追加され、パートナーセンターでのアプリの起動を追跡できるようになります。 アプリの起動引数を [ParseArgumentsAndTrackAppLaunch](/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.parseargumentsandtrackapplaunch) メソッドに渡す必要があります。このメソッドは、追跡 Id をパートナーセンターに送信し、起動引数から追跡 id を削除して、コードに元の起動引数を返します。

このメソッドを呼び出す方法は、プッシュ通知のアクティブ化の種類によって異なります。

* プッシュ通知にフォアグラウンドのアクティブ化の種類がある場合は、このメソッドをアプリの [OnActivated](/uwp/api/windows.ui.xaml.application.onactivated) メソッド オーバーライドから呼び出し、このメソッドに渡された [ToastNotificationActivatedEventArgs](/uwp/api/Windows.ApplicationModel.Activation.ToastNotificationActivatedEventArgs) オブジェクトで使用可能な引数を渡します。 次のコード例では、コード ファイルに **Microsoft.Services.Store.Engagement** 名前空間と **Windows.ApplicationModel.Activation** 名前空間の **using** ステートメントがあることを前提としています。

  :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreSDKSamples/cs/App.xaml.cs" id="OnActivated":::

* プッシュ通知にバックグラウンドのアクティブ化の種類がある場合、[バックグラウンド タスク](../launch-resume/support-your-app-with-background-tasks.md)の [Run](/uwp/api/windows.applicationmodel.background.ibackgroundtask.run) メソッドからこのメソッドを呼び出し、このメソッドに渡された [ToastNotificationActionTriggerDetail](/uwp/api/Windows.UI.Notifications.ToastNotificationActionTriggerDetail) オブジェクトで使用可能な引数を渡します。 次のコード例では、コード ファイルに **Microsoft.Services.Store.Engagement**、**Windows.ApplicationModel.Background**、**Windows.UI.Notifications** の名前空間の **using** ステートメントがあることを前提としています。

  :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreSDKSamples/cs/DevCenterNotifications.cs" id="Run":::

<span id="unregister" />

## <a name="unregister-for-push-notifications"></a>プッシュ通知の登録解除

アプリでパートナーセンターからの対象となるプッシュ通知の受信を停止する場合は、 [Unregisternotificationchannelasync](/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.unregisternotificationchannelasync) メソッドを呼び出します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreSDKSamples/cs/DevCenterNotifications.cs" id="UnregisterNotificationChannelAsync":::

このメソッドは通知に使用されているチャネルを無効にするため、アプリは*いずれの*サービスからもプッシュ通知を受信しなくなることに注意してください。 チャネルを閉じた後、パートナーセンターからのターゲットのプッシュ通知や、WNS を使用したその他の通知など、サービスに対してチャネルを再度使用することはできません。 このアプリでプッシュ通知の送信を再開するには、アプリは新しいチャネルを要求する必要があります。

## <a name="related-topics"></a>関連トピック

* [アプリのユーザーにプッシュ通知を送信する](../publish/send-push-notifications-to-your-apps-customers.md)
* [Windows プッシュ通知サービス (WNS) の概要](../design/shell/tiles-and-notifications/windows-push-notification-services--wns--overview.md)
* [通知チャネルを要求、作成、保存する方法](/previous-versions/windows/apps/hh868221(v=win.10))
* [Microsoft Store Services SDK](./microsoft-store-services-sdk.md)
