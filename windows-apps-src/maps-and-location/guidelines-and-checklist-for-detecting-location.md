---
Description: このトピックでは、ユーザーの位置にアクセスする必要があるアプリを構築する際のパフォーマンス ガイドラインを説明します。
title: 位置認識アプリのガイドライン
ms.assetid: 16294DD6-5D12-4062-850A-DB5837696B4D
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, 位置, 地図, 位置情報
ms.localizationpriority: medium
ms.openlocfilehash: 2178a8812a4c900c59c370e52e7e5a5b3e0a9182
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89158666"
---
# <a name="guidelines-for-location-aware-apps"></a>位置認識アプリのガイドライン




**重要な API**

-   [**位置情報**](/uwp/api/Windows.Devices.Geolocation)
-   [**Geolocator**](/uwp/api/Windows.Devices.Geolocation.Geolocator)

このトピックでは、ユーザーの位置にアクセスする必要があるアプリを構築する際のパフォーマンス ガイドラインを説明します。

## <a name="recommendations"></a>Recommendations


-   location オブジェクトは、アプリで位置データが必要になった場合にのみ使用を開始します。

     ユーザーの位置情報にアクセスする前に、[**RequestAccessAsync**](/uwp/api/windows.devices.geolocation.geolocator.requestaccessasync) を呼び出します。 このときに、アプリをフォアグラウンドで実行し、**RequestAccessAsync** を UI スレッドから呼び出す必要があります。 位置情報に対するアクセス許可をユーザーがアプリに与えるまで、アプリは位置情報にアクセスできません。

-   アプリで位置情報が必須でない場合は、位置情報を必要とするタスクをユーザーが完了することを試みるまではその情報にアクセスしないでください。 たとえば、ソーシャル ネットワーキング アプリに、[位置情報を使ってチェックイン] というボタンがある場合、アプリは、ユーザーがそのボタンをクリックするまでは位置情報にアクセスしないようにします。 アプリのメイン機能で位置情報が必要な場合は、すぐにアクセスしても問題ありません。

-   [**Geolocator**](/uwp/api/Windows.Devices.Geolocation.Geolocator) オブジェクトの初めての使用はフォアグラウンド アプリのメイン UI スレッドで行い、ユーザーから同意を得るためのプロンプトをトリガーする必要があります。 **Geolocator** の初めての使用とは、[**getGeopositionAsync**](/uwp/api/windows.devices.geolocation.geolocator.getgeopositionasync) を初めて呼び出すとき、または [**positionChanged**](/uwp/api/windows.devices.geolocation.geolocator.positionchanged) イベントのハンドラーを初めて登録するときです。

-   位置データがどのように使われるかをユーザーに知らせてください。
-   ユーザーが現在の位置を手動で更新できる UI を用意します。
-   位置データの取得中は、進行状況バーまたは進行状況リングを表示します。 <!--For info on the available progress controls and how to use them, see [**Guidelines for progress controls**](guidelines-and-checklist-for-progress-controls.md).-->
-   位置情報サービスが無効または利用不可になっている場合は、適切なエラー メッセージまたはダイアログを表示します。

    位置情報の設定でアプリからユーザーの位置情報へのアクセスを許可していない場合は、**設定**アプリ内にある**位置情報に関するプライバシー設定**への使いやすいリンクを示すことをお勧めします。 たとえば、ハイパーリンク コントロールを使うか、`ms-settings:privacy-location` URI を使用して [**LaunchUriAsync**](/uwp/api/windows.system.launcher.launchuriasync) メソッドを呼び出すことでコードから**設定**アプリを起動します。 詳しくは、「[Windows 設定アプリの起動](../launch-resume/launch-settings-app.md)」をご覧ください。

-   位置情報へのアクセスをユーザーが無効にしたときに、キャッシュされた位置データをクリアし、[**Geolocator**](/uwp/api/Windows.Devices.Geolocation.Geolocator) を解放します。

    ユーザーが設定を使って位置情報へのアクセスをオフにした場合に、[**Geolocator**](/uwp/api/Windows.Devices.Geolocation.Geolocator) オブジェクトを解放します。 その後、アプリは任意の場所の API 呼び出しに対して **アクセス \_ 拒否** の結果を受け取ります。 アプリで位置データを保存またはキャッシュしている場合は、ユーザーが位置情報へのアクセスを無効にするときにすべてのキャッシュ データをクリアします。 位置情報サービス経由で位置データを利用できないときに位置情報を手動で入力するための代替手段を用意してください。

-   位置情報サービスを再び有効にするための UI を用意します。 たとえば、 [**Geolocator**](/uwp/api/Windows.Devices.Geolocation.Geolocator) オブジェクトを再インスタンス化して位置情報を再度取得しようとする更新ボタンを提供します。

    位置情報サービスを再び有効にするための UI を提供する—

    -   ユーザーが位置情報を無効にした後に再び有効にした場合、アプリには通知されません。 [**status**](/uwp/api/windows.devices.geolocation.statuschangedeventargs.status) プロパティは変更されず、[**statusChanged**](/uwp/api/windows.devices.geolocation.geolocator.statuschanged) イベントも発生しません。 アプリで、新しい [**Geolocator**](/uwp/api/Windows.Devices.Geolocation.Geolocator) オブジェクトを作成し、[**getGeopositionAsync**](/uwp/api/windows.devices.geolocation.geolocator.getgeopositionasync) を呼び出して更新された位置情報データを取得するか、[**positionChanged**](/uwp/api/windows.devices.geolocation.geolocator.positionchanged) イベントの受信登録をもう一度行います。 位置情報が再び有効になったことを確認できたら、位置情報サービスが無効であることをユーザーに通知するために表示していた UI をクリアし、新しい状態に対して適切に対応します。
    -   アプリをアクティブ化するとき、位置情報が必要な機能をユーザーが明示的に使おうとしたときなど、状況に応じて必要と思われる任意の時点で、位置情報データを取得し直すことをお勧めします。

**パフォーマンス**

-   アプリで位置情報の更新を受け取る必要がない場合は、位置情報の要求を 1 回だけ使います。 たとえば、写真に位置情報タグを追加するアプリでは、位置情報更新イベントを受け取る必要はありません。 このようなアプリでは、[**getGeopositionAsync**](/uwp/api/windows.devices.geolocation.geolocator.getgeopositionasync) を使って位置情報を要求します。詳しくは、「[現在の位置情報の取得](./get-location.md)」をご覧ください。

    1 回限りの位置情報の要求を行う場合は、次の値を設定する必要があります。

    -   [**DesiredAccuracy**](/uwp/api/windows.devices.geolocation.geolocator.desiredaccuracy) または [**DesiredAccuracyInMeters**](/uwp/api/windows.devices.geolocation.geolocator.desiredaccuracyinmeters) を設定して、アプリから要求される精度を指定します。 これらのパラメーターを使用する場合の推奨事項については、以下をご覧ください
    -   [**GetGeopositionAsync**](/uwp/api/windows.devices.geolocation.geolocator.getgeopositionasync) の最大保存期間のパラメーターを設定して、アプリで有用な位置情報を取得できる期間を指定します。 アプリで数秒または数分前の位置を使用できる場合は、ほとんどすぐに位置を受け取って、デバイスの電力を節約することができます。
    -   [**GetGeopositionAsync**](/uwp/api/windows.devices.geolocation.geolocator.getgeopositionasync) のタイムアウト パラメーターを設定します。 これが、アプリが返される位置またはエラーを待機することができる長さです。 ユーザーへの応答性とアプリが必要とする精度のバランスを理解する必要があります。
-   頻繁に位置を更新する必要がある場合は、連続的な位置情報のセッションを使います。 特定のしきい値を超えた移動を検出する場合、または発生時に絶えず位置情報の更新を取得する場合は、[**positionChanged**](/uwp/api/windows.devices.geolocation.geolocator.positionchanged) イベントと [**statusChanged**](/uwp/api/windows.devices.geolocation.geolocator.statuschanged) イベントを使います。

    位置情報の更新を要求すると、[**DesiredAccuracy**](/uwp/api/windows.devices.geolocation.geolocator.desiredaccuracy) または [**DesiredAccuracyInMeters**](/uwp/api/windows.devices.geolocation.geolocator.desiredaccuracyinmeters) を設定して、アプリから要求される精度を指定できます。 また、[**MovementThreshold**](/uwp/api/windows.devices.geolocation.geolocator.movementthreshold)  または [**ReportInterval**](/uwp/api/windows.devices.geolocation.geolocator.reportinterval) を使って、位置情報の更新が必要な頻度を設定する必要があります。

    -   移動しきい値を指定します。 アプリによっては、ユーザーの移動距離が大きいときにだけ位置情報を更新すれば済むものがあります。 たとえば、地域のニュースや天気予報の更新情報を提供するアプリでは、ユーザーの位置が別の都市に変わらない限り位置情報を更新する必要はありません。 このような場合は、[**MovementThreshold**](/uwp/api/windows.devices.geolocation.geolocator.movementthreshold) プロパティを設定して、位置情報更新イベントの発生条件となる最小の移動距離を調整します。 このプロパティには [**PositionChanged**](/uwp/api/windows.devices.geolocation.geolocator.positionchanged) イベントをフィルター処理する効果があります。 これらのイベントは、位置の変化が移動しきい値を超えたときにのみ発生します。

    -   アプリのエクスペリエンスと整合し、システム リソースの使用が最小限に抑えられる [**reportInterval**](/uwp/api/windows.devices.geolocation.geolocator.reportinterval) を使います。 たとえば、天気予報アプリでは、15 分ごとにデータを更新するだけでよいと思われます。 リアルタイムのナビゲーション アプリを除くほとんどのアプリでは、位置情報の更新について、高い精度のストリームを常に必要とするわけではありません。 最大限の精度のデータ ストリームを必要としない場合や、頻繁に更新する必要がない場合は、**ReportInterval** プロパティを設定して、アプリで位置情報を更新する必要がある最小の頻度を指定します。 これにより、必要なときにだけ位置情報を計算することで、位置情報の提供元の電力を節約できます。

        リアルタイムのデータを必要とするアプリでは、最短の間隔を指定せずに、[**ReportInterval**](/uwp/api/windows.devices.geolocation.geolocator.reportinterval) を 0 に設定する必要があります。 既定のレポート間隔は、1 秒またはハードウェアでサポートされる最短間隔 (短い方) です。

        位置データを提供するデバイスでは、さまざまなアプリから要求されるレポート間隔を追跡し、要求された最短の間隔でデータをレポートする場合があります。 これにより、精度の要件が最も高いアプリに必要なデータを提供できます。 そのため、別のアプリで要求された更新頻度の方が高い場合は、要求した頻度よりも頻繁に更新が生成されることがあります。

        **メモ**   場所のソースが特定のレポート間隔の要求を受け入れることは保証されません。 位置情報取得機能デバイスによってはレポート間隔を追跡しないものもありますが、追跡されるものとして指定しておくことをお勧めします。

    -   電力を節約するには、[**desiredAccuracy**](/uwp/api/windows.devices.geolocation.geolocator.desiredaccuracy) プロパティを設定して、アプリで高い精度のデータが必要かどうかを位置情報プラットフォームに示します。 高い精度のデータを必要とするアプリがなければ、GPS 位置情報取得機能を無効にして電力を節約できます。

        -   GPS でデータを取得するには、[**desiredAccuracy**](/uwp/api/windows.devices.geolocation.geolocator.desiredaccuracy) を **HIGH** に設定します。
        -   ターゲティング広告のためにのみ位置情報を使うアプリは、消費電力を最小限に抑えるため、[**desiredAccuracy**](/uwp/api/windows.devices.geolocation.geolocator.desiredaccuracy) を **Default** に設定します。

        精度についてアプリに特定のニーズがある場合は、[**DesiredAccuracy**](/uwp/api/windows.devices.geolocation.geolocator.desiredaccuracy) を使う代わりに [**DesiredAccuracyInMeters**](/uwp/api/windows.devices.geolocation.geolocator.desiredaccuracyinmeters) プロパティを使うこともあります。 これは、通常、位置情報を移動体通信ビーコン、Wi-Fi ビーコンや衛星に基づいて取得できる Windows Phone に特に役立ちます。 より具体的な精度値を選ぶと、システムが位置情報を提供する際に最も低い消費電力で適切なテクノロジを識別するために役立ちます。

        次に例を示します。

        -   アプリが広告の調整、天気、ニュースなどのための位置情報を取得している場合は、一般に 5000 m の精度で十分です。
        -   アプリで近隣の取引が隣接して表示されている場合は、通常、結果を提供するには300メーターの精度が適しています。
        -   ユーザーがお勧めの近くのレストランを探している場合は、ブロック内の位置を取得する必要がありますので、100 m の精度で十分です。
        -   ユーザーが自身の位置を共有しようとしている場合は、アプリには約 10 m の精度が必要です。
    -   アプリに特定の精度の要件がある場合は [**Geocoordinate.accuracy**](/uwp/api/windows.devices.geolocation.geocoordinate.accuracy) プロパティを使います。 たとえば、ナビゲーション アプリでは、**Geocoordinate.accuracy** プロパティを使って、利用可能な位置情報データがアプリの要件を満たしているかどうかを調べます。

-   起動時の待ち時間を考慮します。 アプリで初めて位置データを要求したとき、位置情報取得機能が起動するまでに 1 ～ 2 秒の待ち時間が発生することがあります。 アプリの UI を設計するときは、この点に注意してください。 たとえば、[**GetGeopositionAsync**](/uwp/api/windows.devices.geolocation.geolocator.getgeopositionasync) の呼び出しを保留している他のタスクがブロックされないようにしてください。

-   バックグラウンドの動作を考慮します。 アプリにフォーカスがない場合、バックグラウンドで中断されている間は位置情報更新イベントを受け取りません。 位置情報の更新をログに記録して追跡する場合は、この点に注意してください。 アプリにフォーカスが戻った後は、新しいイベントだけを受け取ります。 アプリが非アクティブだったときに発生した更新は取得されません。

-   ロー センサーとフュージョン センサーを効率的に使います。 センサーには、 *未加工* と *fusion*の2種類があります。

    -   ロー センサーには、加速度計、ジャイロメーター、磁力計が含まれます。
    -   フュージョン センサーには、向き、傾斜計、コンパスが含まれます。 フュージョン センサーは、ロー センサーの組み合わせからデータを取得します。

    Windows ランタイム API は磁力計以外のすべてのセンサーにアクセスできます。 フュージョン センサーの方がロー センサーよりも正確で安定していますが、より多くの電力を使います。 用途に適したセンサーを使う必要があります。 詳細については、「 [センサー](../devices-sensors/sensors.md)」を参照してください。

**コネクト スタンバイ**
- PC がコネクト スタンバイ状態にある場合、[**Geolocator**](/uwp/api/Windows.Devices.Geolocation.Geolocator) オブジェクトはいつでもインスタンス化できます。 しかし、**Geolocator** オブジェクトは集約する対象のセンサーを見つけることができず、[**GetGeopositionAsync**](/uwp/api/windows.devices.geolocation.geolocator.getgeopositionasync) の呼び出しは 7 秒後にタイムアウトします。[**PositionChanged**](/uwp/api/windows.devices.geolocation.geolocator.positionchanged) イベント リスナーの呼び出しは行われず、[**StatusChanged**](/uwp/api/windows.devices.geolocation.geolocator.statuschanged) イベント リスナーは 1 回呼び出され、そのステータスは **NoData** となります。

## <a name="additional-usage-guidance"></a>その他の使い方のガイダンス


### <a name="detecting-changes-in-location-settings"></a>位置情報設定の変更を検出する

ユーザーは、**設定**アプリの**位置情報に関するプライバシー設定**を使って、位置情報機能を無効にすることができます。

-   ユーザーが位置情報サービスを無効にしたり再び有効にしたことを検出するには、次の操作を行います。
    -   [**StatusChanged**](/uwp/api/windows.devices.geolocation.geolocator.statuschanged) イベントを処理します。 **StatusChanged** イベントの引数である [**Status**](/uwp/api/windows.devices.geolocation.statuschangedeventargs.status) プロパティの値は、ユーザーが位置情報サービスを無効にすると **Disabled** になります。
    -   [**GetGeopositionAsync**](/uwp/api/windows.devices.geolocation.geolocator.getgeopositionasync) から返るエラー コードをチェックします。 ユーザーがロケーションサービスを無効にしている場合は、 **Getgeopositionasync** の呼び出しが **アクセス \_ 拒否** エラーで失敗し、 [**Locationstatus**](/uwp/api/windows.devices.geolocation.geolocator.locationstatus) プロパティの値が **無効**になります。
-   地図アプリのような、位置情報データが必須のアプリの場合は、必ず次の操作を実行してください。
    -   ユーザーの位置情報が変わったときに更新情報を取得できるように、[**PositionChanged**](/uwp/api/windows.devices.geolocation.geolocator.positionchanged) イベントを処理します。
    -   前の説明に従って [**StatusChanged**](/uwp/api/windows.devices.geolocation.geolocator.statuschanged) イベントを処理し、位置設定の変化を検出します。

位置情報サービスからは、データが利用可能になったときにデータが返されます。 最初に誤差の範囲が大きい位置情報を返し、より正確な情報が利用可能になったときに位置情報を更新する場合があります。 ユーザーの位置情報を表示するアプリでは、通常、より正確な情報が利用可能になったときに位置情報を更新する必要があります。

### <a name="graphical-representations-of-location"></a>位置情報のグラフィック表示

アプリでは、[**Geocoordinate.accuracy**](/uwp/api/windows.devices.geolocation.geocoordinate.accuracy) を使って、ユーザーの現在の位置情報を地図に明確に示すようにします。 精度の幅は、主に、誤差の範囲が半径約 10 m、半径約 100 m、半径 1 km 超、という 3 種類があります。 精度情報を使うことにより、アプリでは、利用可能なデータの状況に応じて位置情報を正確に表示することができるようになります。 マップコントロールの使用に関する一般的な情報については、「 [2d、3d、および Streetside ビューでのマップの表示](./display-maps.md)」を参照してください。

-   約 10 m 相当の精度 (GPS の解像度) の場合、位置情報は点またはピンで地図上に示すことができます。 この精度では、経度と緯度の座標、住所の番地も表示できます。

    ![約 10 m の GPS 精度で表示される地図の例。](images/10metererrorradius.png)

-   10 ～ 500 m (おおよそ 100 m) の精度の場合、位置情報は通常、Wi-Fi による解決で受信されています。 移動体通信から取得した位置情報は約 300 m の精度です。 この場合、アプリでは誤差を含む範囲を表示することをお勧めします。 道順を表示するアプリなど中心点が必要となる場合は、その中心点を誤差を表す範囲で囲むことができます。

    ![約 100 m の Wi-Fi 精度で表示される地図の例。](images/100metererrorradius.png)

-   戻される精度が 1 km より大きい場合は、IP レベルの解決で位置情報を受信することになります。 多くの場合、地図上に特定の地点をピンポイントで表示するためにはこのレベルの精度は低すぎます。 アプリでは、地図を市のレベルまで、または誤差の範囲に応じて適切なエリア (たとえば、地域のレベル) までズームインすることをお勧めします。

    ![約 1 km の Wi-Fi 精度で表示される地図の例。](images/1000metererrorradius.png)

位置情報の精度が別の精度に切り替わるときは、異なるグラフィック表示が適切に遷移するようにします。 このためには、次のようにします。

-   切り替え時のアニメーションをスムーズにし、切り替えを高速かつ滑らかに保ちます。
-   数回の連続的な報告があるのを待ってから、精度が変わったと判断します。これにより、不要なズームが頻繁に行われるのを防ぐことができます。

### <a name="textual-representations-of-location"></a>位置情報のテキスト表示

天気アプリや地域情報アプリなどアプリの種類によっては、さまざまな精度の位置情報をテキストで表現することが必要になります。 位置情報は、データが提供する精度レベルまでに抑えて、明確に表示するようにします。

-   精度が約 10 m 相当 (GPS の解像度) の場合、受信した位置情報データは相当に正確であるので、ごく近隣の地名のレベルで情報を伝えることができます。 市の名前、都道府県の名前、国/地域の名前も使うことができます。
-   精度が約 100 m 相当 (Wi-Fi の解像度) の場合、受信した位置情報データはある程度正確です。市の名前までの情報を表示することをお勧めします。 それより詳しい、近隣地域の地名の使用は避けてください。
-   1 km 超の精度 (IP による解決) の場合は、都道府県の名前、または国/地域の名前のみ表示します。

### <a name="privacy-considerations"></a>プライバシーに関する考慮事項

ユーザーの地理的な位置情報は、個人を特定できる情報 (PII) に当たります。 ユーザーのプライバシーの保護に関するガイダンスについては、次の Web サイトをご覧ください。

-   [Microsoft のプライバシー]( https://www.microsoft.com/privacy/dpd/default.aspx)

<!--For more info, see [Guidelines for privacy-aware apps](guidelines-for-enabling-sensitive-devices.md).-->

## <a name="related-topics"></a>関連トピック

* [ジオフェンスのセットアップ](./set-up-a-geofence.md)
* [現在の位置情報の取得](./get-location.md)
* [2D、3D、Streetside ビューでの地図の表示](./display-maps.md)
<!--* [Design guidelines for privacy-aware apps](guidelines-for-enabling-sensitive-devices.md)-->
* [UWP の位置情報サンプル (geolocation)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Geolocation)
 

 