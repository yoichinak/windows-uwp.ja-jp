---
description: パートナーセンターで各アプリに関連する詳細を管理および表示し、A/B テストやマップなどのサービスを構成します。
title: アプリの管理とサービス
ms.assetid: 99DA2BC1-9B5D-4746-8BC0-EC723D516EEF
ms.date: 03/21/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e973e9b3a8f3c9ba63a091f4e542e36a84c26128
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2020
ms.locfileid: "93032755"
---
# <a name="app-management-and-services"></a>アプリの管理とサービス

[パートナーセンター](https://partner.microsoft.com/dashboard)で各アプリに関連する詳細を管理および表示したり、通知、A/B テスト、マップなどのサービスを構成したりすることができます。

パートナーセンターでアプリを使用する場合、 **サービス** と **アプリ管理** の左側のナビゲーションメニューにセクションが表示されます。 これらのセクションを展開すると、次の機能にアクセスできます。

## <a name="services"></a>サービス

**[サービス]** セクションでは、アプリのいくつかの異なるサービスを管理できます。

## <a name="xbox-live"></a>Xbox Live

ゲームを発行する場合は、このページで [Xbox Live](https://www.xbox.com/developers/creators-program) 作成者プログラムを有効にすることができます。 これにより、Xbox Live の機能の構成とテストを開始し、最終的に Xbox Live クリエータープログラムゲームを発行できます。

詳細については、「 [Xbox Live 作成者プログラムの概要](/gaming/xbox-live/get-started-with-creators/get-started-with-xbox-live-creators) 」を参照して、 [新しい xbox Live 作成者プログラムのタイトルを作成し、テスト環境に発行](/gaming/xbox-live/get-started-with-creators/create-and-test-a-new-creators-title)してください。

## <a name="experimentation"></a>実験

**[Experimentation]** ページを使うと、ユニバーサル Windows プラットフォーム (UWP) アプリの試験的機能を作成し、A/B テストを実行できます。 アプリの機能の変更をすべてのユーザー向けに有効にする前に、一部のユーザーに対して変更 (またはバリエーション) の有効性を A/B テストで測定します。

詳しくは、「[A/B テストを使用してアプリの試験的機能を実行する](../monetize/run-app-experiments-with-a-b-testing.md)」をご覧ください。

## <a name="maps"></a>Maps

Windows 10 または Windows 8.x を対象としたアプリでマップ サービスを使うには、[Bing 地図デベロッパー センター](https://www.bingmapsportal.com/)にアクセスしてください。 Bing Maps デベロッパーセンターからマップ認証キーを要求してアプリに追加する方法の詳細については、「 [request a maps authentication key](../maps-and-location/authentication-key.md) 」を参照してください。 

[ **マップ** ] ページは、以前に発行された Windows Phone 8.1 以前のアプリに対してのみ使用してください。 これらのアプリでマップサービスを使用するには、アプリのコードに含めるマップサービスアプリケーション ID とトークンを要求する必要があります。 [トークンの **取得** ] をクリックすると、マップサービスアプリケーション ID ( **ApplicationID** ) が生成され、アプリのサービス認証トークン ( **authenticationtoken** ) がマップされます。 アプリをパッケージ化して送信する前に、これらの値をコードに追加してください。 詳細については、「[Windows Phone 8 でマップ コントロールをページに追加する方法](/previous-versions/windows/apps/jj207033(v=vs.105))」を参照してください。

## <a name="product-collections-and-purchases"></a>製品のコレクションと購入

Microsoft Store collection API と Microsoft Store purchase API を使用してアプリとアドオンの所有権情報にアクセスするには、関連付けられた Azure AD クライアント Id をここに入力する必要があります。 これらの変更が有効になるまで最大で 16 時間かかることに注意してください。

詳しくは、「[サービスから製品の権利を管理する](../monetize/view-and-grant-products-from-a-service.md)」をご覧ください。

## <a name="administrator-consent"></a>管理者の同意

製品が Azure AD と統合され、 [アプリケーションのアクセス許可または](/graph/permissions-reference) 管理者の同意が必要な委任されたアクセス許可を要求する api を呼び出す場合は、ここに AZURE AD クライアント ID を入力します。 これにより、組織のアプリを取得する管理者は、テナント内のすべてのユーザーの代理として、製品の同意を与えることができます。

詳しくは、「[テナント全体の同意を要求する](/azure/active-directory/develop/v2-permissions-and-consent#requesting-consent-for-an-entire-tenant)」をご覧ください。

## <a name="app-management"></a>アプリの管理

**[アプリ管理]** セクションでは、ID とパッケージの詳細を確認したり、アプリの名前を管理したりできます。

## <a name="app-identity"></a>アプリ ID

このページには、アプリの内容へのリンクの URL など、ストア内のアプリの一意の ID に関連する詳細情報が表示されます。

詳しくは、「[アプリ ID の詳細の表示](view-app-identity-details.md)」をご覧ください。

## <a name="manage-app-names"></a>アプリ名の管理

ここでは、アプリのために予約したすべての名前を確認できます。 追加の名前の予約や、使わなくなったの名前の削除は、ここで行うことができます。

詳しくは、「[アプリ名の管理](manage-app-names.md)」をご覧ください。

## <a name="current-packages"></a>現在のパッケージ

このページでは、公開されたすべてのパッケージに関連する詳しい情報を確認することができます。

> [!NOTE]
> ここには、アプリが公開されるまで、情報は表示されません。

各パッケージの名前、バージョン、およびアーキテクチャが表示されます。 **[詳細]** をクリックすると、サポートされる言語、アプリの機能、ファイル サイズなどの詳しい情報が表示されます。 パッケージごとに表示される情報は、対象となるオペレーティング システムとその他の要因によって異なることがあります。 

OEM アクセス許可を持つ開発者は、 **[現在のパッケージ]** ページから [プレインストール パッケージを生成](generate-preinstall-packages-for-oems.md) することもできます。

## <a name="wnsmpns"></a>WNS/MPNS

**WNS/MPNS** セクションには、アプリの顧客に通知を作成して送信するためのオプションが用意されています。 

> [!TIP]
> UWP アプリの場合は、パートナーセンターの **通知** 機能を使用することをお勧めします。 この機能を使用すると、お客様のすべてのお客様に通知を送信することができます。また、お客様の [セグメント](create-customer-segments.md)で定義した条件を満たす Windows 10 のお客様の対象となるサブセットに通知を送信することもできます。 詳しくは、「[アプリのユーザーに通知を送信する](send-push-notifications-to-your-apps-customers.md)」をご覧ください。

アプリのパッケージの種類とその具体的な要件に応じて、次のいずれかのオプションを使用することもできます。 

-   **Windows プッシュ通知サービス (WNS)** を使うと、独自のクラウド サービスからトースト更新、タイル更新、バッジ更新、直接更新を送ることができます。 詳しくは、「[Windows プッシュ通知サービスの概要](../design/shell/tiles-and-notifications/windows-push-notification-services--wns--overview.md)」をご覧ください。

-   **Microsoft Azure Mobile Apps** を使うと、プッシュ通知の送信や、アプリ ユーザーの認証や管理、クラウドでのアプリ データの保存をすることができます。 詳しくは、[モバイル アプリに関するドキュメント](/azure/app-service-mobile/)をご覧ください。

-   **Microsoft プッシュ通知サービス (MPNS)** は、以前に公開された .xap パッケージと共に Windows Phone に使用できます。 ここで構成を行わなくても、認証されていない通知を限られた数送信することができますが、制限が減らないように認証済みの通知を使うことをお勧めします。 MPNS を使用している場合は、[ **WNS/MPNS** ] ページで指定したフィールドに証明書をアップロードする必要があります。 詳しくは、「[Windows Phone 8 のプッシュ通知を送信するように認証済み Web サービスを設定する](/previous-versions/windows/apps/ff941099(v=vs.105))」をご覧ください。
 

 
