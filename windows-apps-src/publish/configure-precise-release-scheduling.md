---
Description: アプリをストアで利用可能にする正確な日付と時刻を設定することができます。この機能を使うと、市場ごとに柔軟に日付をカスタマイズできます。
title: 正確なリリース スケジュールの構成
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, スケジュール, リリース日, 日付, 公開
ms.localizationpriority: medium
ms.openlocfilehash: ec9ee00aaa350fc48185cc6328674ac2d8f62ea5
ms.sourcegitcommit: f561efbda5c1d47b85601d91d70d86c5332bbf8c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/21/2019
ms.locfileid: "72690354"
---
# <a name="configure-precise-release-scheduling"></a>正確なリリース スケジュールの構成

[[価格および使用可能状況]](set-app-pricing-and-availability.md) ページの **[スケジュール]** セクションでは、アプリをストアで利用可能にする正確な日付と時刻を設定でき、市場ごとに日付を柔軟にカスタマイズできます。

> [!NOTE]
> このトピックはアプリについて説明していますが、アドオンのリリース スケジュールにも同じプロセスを使います。

さらに、ストアでの製品の公開を終了する日付を設定することもできます。 この場合、ストアの検索や参照によって製品を見つけることはできなくなりますが、直接リンクを知っているユーザーであれば、製品のストア登録情報を表示することができます。 製品をダウンロードできるのは、その製品を既に所有しているか、[プロモーション コード](generate-promotional-codes.md)を持っていて、かつ Windows 10 デバイスを使っている場合だけです。

既定では ([[表示]](choose-visibility-options.md#discoverability) セクションで **[この製品をストアで提供しますが、検索はできないようにします]** のオプションのいずれかを選択した場合を除き)、アプリが認定に合格して公開プロセスが完了すると、そのアプリはすぐにユーザーが利用できるようになります。 他の日付を選択するには、 **[オプションの表示]** を選択してこのセクションを展開します。

[[表示]](choose-visibility-options.md#discoverability) セクションで **[この製品をストアで提供しますが、検索はできないようにします]** のオプションのいずれかを選択した場合、 **[スケジュール]** セクションで日付を構成することはできません。この場合、アプリはユーザーにリリースされず、構成するリリース日が存在しないためです。

> [!IMPORTANT]
> [スケジュール] セクションで指定した日付は、Windows 10 のユーザーにのみ適用されます。
>
>以前に発行されたアプリが以前のバージョンの OS をサポートしている場合、選択した**停止の取得**日は、これらの顧客には適用されません。引き続きアプリを取得できます ([[可視性](choose-visibility-options.md#discoverability)] セクションで新しい選択を使用して更新を送信した場合、または [アプリの**概要**] ページで [**アプリを使用できないよう**にする] を選択した場合を除く)。


## <a name="base-schedule"></a>基本スケジュール

[基本スケジュール] で選択した設定は、後で [[特定の市場向けにカスタマイズする]](#customize-the-schedule-for-specific-markets) を選択して特定の市場 (または市場グループ) 向けの日付を指定しない限り、アプリを提供するすべての市場に適用されます。

ここには、 **[リリース]** と **[購入の停止]** の 2 つのオプションがあります。 

## <a name="release"></a>リリース

**[リリース]** ドロップダウンでは、アプリをストアで利用可能にする日付を設定できます。 つまり、アプリはストアの検索や参照によって見つけることができ、ユーザーはストア登録情報を表示してアプリを入手できます。

>[!NOTE]
> アプリが公開され、ストアで利用可能になった後は、 **[リリース]** の日付を選択することはできません (既にアプリがリリースされているため)。

製品の **[リリース]** スケジュールに関して構成できるオプションを次に示します。
- **[できるだけ早く]** : 製品は、認定されて公開されるとすぐにリリースされます。 これは既定のオプションです。
- **[次の時点]** : 製品は、選択した日時にリリースされます。 次の 2 つの追加オプションがあります。
   - **[UTC]** : 選択した時刻は世界標準時 (UTC) です。アプリはすべての市場で同時にリリースされます。
   - **[ローカル]** : 選択した時刻は、市場に関連付けられている各タイム ゾーンの時刻として扱われます (複数のタイム ゾーンがある市場では、その市場のタイム ゾーンの 1 つだけが使われます。 米国では、東部標準時のタイムゾーンが使用されます。 タイムゾーンの包括的な一覧については、このページを参照してください)。
- **[スケジュールされていません]** : アプリはストアで利用可能になりません。 このオプションを選択した場合は、後からアプリをストアで利用可能にすることができます。利用可能にするには、新しい申請を作成するか、他のいずれかのオプションを選択します。


## <a name="stop-acquisition"></a>購入の停止

**[購入の停止]** ドロップダウンでは、新しいユーザーがストアからアプリを入手したり、登録情報を見つけたりできないようにする日時を設定できます。 これは、複数のアプリ間で使用可能状況を調整している場合など、新しいユーザーへのアプリの提供を終了する時期を正確に制御する場合に役立つ可能性があります。

既定では、 **[購入の停止]** は [実行しない] に設定されます。 これを変更するには、ドロップダウンの **[次の時点]** を選択し、上で説明したように日付と時刻を指定します。 選択した日時になると、ユーザーはアプリを入手できなくなります。

このオプションは、[**このアプリを探索可能**にする] を選択し、[[可視性](choose-visibility-options.md#discoverability)] セクションでは利用できず、[取得の停止] を選択した場合と同じ効果があることを理解しておくことが重要です **。直接リンクを使用している顧客は、製品のストアを表示できます。一覧に表示されますが、以前に製品を所有している場合、またはプロモーションコードを持っていて、Windows 10 デバイスを使用している場合にのみダウンロードできます。** 新しいユーザーへのアプリの提供を完全に停止するには、[アプリの概要] ページで **[アプリの提供を停止する]** をクリックします。 詳しくは、「[アプリをストアから削除する](guidance-for-app-package-management.md#removing-an-app-from-the-store)」をご覧ください。

> [!TIP]
> **[購入の停止]** で日付を選択し、後からもう一度アプリを提供することにした場合は、新しい申請を作成して、 **[購入の停止]** を **[実行しない]** に戻すことができます。 更新された申請が公開されると、アプリが再び利用可能になります。

## <a name="customize-the-schedule-for-specific-markets"></a>特定の市場向けにスケジュールをカスタマイズする 

既定では、上で選択したオプションは、アプリが提供されるすべての市場に適用されます。 特定の市場向けのスケジュールをカスタマイズするには、 **[特定の市場向けにカスタマイズする]** をクリックします。 **[市場の選択]** ポップアップ ウィンドウが開き、アプリの提供先として選択されているすべての市場の一覧が表示されます。 [[市場]](define-pricing-and-market-selection.md) セクションで除外した市場がある場合、それらの市場は表示されません。 

1 つの市場向けのスケジュールを追加する場合は、その市場を選択して **[保存]** をクリックします。 上の説明と同じ **[リリース]** と **[購入の停止]** のオプションが表示されます。ただし、ここでの選択はその市場にのみ適用されます。

複数の市場に適用するスケジュールを追加するには、"市場グループ" を作成します。 これを行うには、グループに含める市場を選択し、グループの名前を入力します (この名前は参照用に使われるだけで、ユーザーには表示されません)。たとえば、北米の市場グループを作成する場合は、 **[カナダ]** 、 **[メキシコ]** 、 **[米国]** を選択し、「**北米**」などの任意の名前を付けます。 市場グループの作成が完了したら、 **[保存]** をクリックします。 上の説明と同じ **[リリース]** と **[購入の停止]** オプションが表示されます。ただし、ここでの選択はその市場グループにのみ適用されます。

さらに他の市場または市場グループ向けにカスタム スケジュールを追加するには、もう一度 **[特定の市場向けにカスタマイズする]** をクリックし、これらの手順を繰り返します。 市場グループに含まれている市場を変更するには、その名前を選択します。 市場グループ (または個別の市場) 向けのカスタム スケジュールを削除するには、 **[削除]** をクリックします。

> [!NOTE]
> 1 つの市場を、 **[スケジュール]** セクションで使う複数の市場グループに含めることはできません。 

## <a name="global-time-zones"></a>グローバルタイムゾーン

次の表は、各市場で使用されている特定のタイムゾーンを示しています。そのため、送信時に現地時刻 (午前9時ローカルでのリリースなど) を使用する場合、各市場でリリースされる時間を確認できます。特に、複数の時間がかかる市場では、カナダのようなものです。

<a name="market--time-zone"></a>市場: タイムゾーン
==================
アフガニスタン: (UTC + 04:30) カブールアルバニア: (UTC + 01:00) サラエボ、スコピエ、ワルシャワ、ザグレブアルジェリア: (UTC + 01:00) サラエボ、スコピエ、ワルシャワ、ザグレブアメリカサモア: (utc + 13:00) サモアアンドラ: (utc + 01:00) サラエボ、スコピエ、ワルシャワ、ザグレブアンゴラ: (UTC + 01:00) 西部中央アフリカアンギラ: (UTC-04:00) 大西洋時間 (カナダ) 南極: (UTC + 12:00) オークランド、ウェリントンアンティグア・バーブーダ: (UTC-04:00) 大西洋標準時 (カナダ) アルゼンチン: (utc-03:00) ブエノスアイレスアルメニア: (UTC + 04:00) アブダビ、マスカットアルバ: (UTC-04:00) 大西洋時間 (カナダ) オーストラリア: (UTC + 10:00) キャンベラ、メルボルン、シドニーのオーストリア: (UTC + 01:00) アムステルダム、ベルリン、ベルン、ローマ、ストックホルム、ウィーンアゼルバイジャン: (utc + 04:00) バクーバハマ、: (UTC-05:00) 東部時間 (米国 & カナダ) バーレーン: (UTC+ 04:00) アブダビ、マスカットバングラデシュ: (UTC + 06:00) ダッカバルバドス: (UTC-04:00) 大西洋標準時 (カナダ) ベラルーシ: (utc + 01:00) ブリュッセル、コペンハーゲン、レアルマドリード、パリベリーズ: (UTC-06:00) 中部標準時 (米国 & カナダ) ベナン: (UTC + 01:00)西中央アフリカバミューダ: (UTC-04:00) 大西洋標準時 (カナダ) ブータン: (UTC + 06:00) ダッカベネズエラボリバル共和国: (UTC-04:00) カラカスボリビア: (utc-04:00) ジョージタウン、ラパス、マナウス、San フアンボネール、サンユースタティウス島、サバ島: (UTC-04:00)大西洋標準時 (カナダ) ボスニア・ヘルツェゴビナ: (UTC + 01:00) サラエボ、スコピエ、ワルシャワ、ザグレブボツワナ: (UTC + 01:00) 西中央アフリカブーベ島: (utc + 00:00) モンロビア、レイキャビクブラジル: (utc-03:00) ブラジリア英国インド洋区域: (UTC + 06:00)ダッカ英領バージン諸島: (UTC-04:00) 大西洋標準時 (カナダ) ブルネイ: (UTC + 08:00) イルクーツクブルガリア: (utc + 02:00) キシナウブルキナファソ: (UTC + 00:00) モンロビア、レイキャビクブルンジ: (UTC + 02:00) ハラーレ、プレトリア CÃ́te ジボワール: (UTC + 00:00) モンロビア、レイキャビクカンボジア: (UTC + 07:00) バンコク、ハノイ、ジャカルタカメルーン: (UTC + 01:00) 西中央アフリカカナダ: (UTC-05:00) 東部時間 (米国 & カナダ) カーボベルデ: (UTC-01:00) カーボベルデがです。
Cayman Islands: (UTC-05:00) Eastern Time (US & Canada) Central African Republic: (UTC+01:00) West Central Africa Chad: (UTC+01:00) West Central Africa Chile: (UTC-04:00) Santiago China: (UTC+08:00) Beijing, Chongqing, Hong Kong, Urumqi Christmas Island: (UTC+07:00) Krasnoyarsk Cocos (Keeling) Islands: (UTC+06:30) Yangon (Rangoon) Colombia: (UTC-05:00) Bogota, Lima, Quito, Rio Branco Comoros: (UTC+03:00) Nairobi Congo: (UTC+01:00) West Central Africa Congo (DRC): (UTC+01:00) West Central Africa Cook Islands: (UTC-10:00) Hawaii Costa Rica: (UTC-06:00) Central Time (US & Canada) Croatia: (UTC+01:00) Sarajevo, Skopje, Warsaw, Zagreb CuraÃ§ao: (UTC-04:00) Cuiaba Cyprus: (UTC+02:00) Chisinau Czech Republic: (UTC+01:00) Belgrade, Bratislava, Budapest, Ljubljana, Prague Denmark: (UTC+01:00) Brussels, Copenhagen, Madrid, Paris Djibouti: (UTC+03:00) Nairobi Dominica: (UTC-04:00) Atlantic Time (Canada) Dominican Republic: (UTC-04:00) Atlantic Time (Canada) Ecuador: (UTC-05:00) Bogota, Lima, Quito, Rio Branco Egypt: (UTC+02:00) Chisinau El Salvador: (UTC-06:00) Central Time (US & Canada) Equatorial Guinea: (UTC+01:00) West Central Africa Eritrea: (UTC+03:00) Nairobi Estonia: (UTC+02:00) Chisinau Ethiopia: (UTC+03:00) Nairobi Falkland Islands (Islas Malvinas): (UTC-04:00) Santiago Faroe Islands: (UTC+00:00) Dublin, Edinburgh, Lisbon, London Fiji: (UTC+12:00) Fiji Finland: (UTC+02:00) Helsinki, Kyiv, Riga, Sofia, Tallinn, Vilnius France: (UTC+01:00) Brussels, Copenhagen, Madrid, Paris French Guiana: (UTC-03:00) Cayenne, Fortaleza French Polynesia: (UTC-10:00) Hawaii French Southern and Antarctic Lands: (UTC+05:00) Ashgabat, Tashkent Gabon: (UTC+01:00) West Central Africa Gambia, The: (UTC+00:00) Monrovia, Reykjavik Georgia: (UTC-05:00) Eastern Time (US & Canada) Germany: (UTC+01:00) Amsterdam, Berlin, Bern, Rome, Stockholm, Vienna Ghana: (UTC+00:00) Monrovia, Reykjavik Gibraltar: (UTC+01:00) Sarajevo, Skopje, Warsaw, Zagreb Greece: (UTC+02:00) Athens, Bucharest Greenland: (UTC+00:00) Monrovia, Reykjavik Grenada: (UTC-04:00) Atlantic Time (Canada) Guadeloupe: (UTC-04:00) Atlantic Time (Canada) Guam: (UTC+10:00) Guam, Port Moresby Guatemala: (UTC-06:00) Central Time (US & Canada) Guernsey: (UTC+00:00) Monrovia, Reykjavik Guinea: (UTC+00:00) Monrovia, Reykjavik Guinea-Bissau: (UTC+00:00) Monrovia, Reykjavik Guyana: (UTC-04:00) Atlantic Time (Canada) Haiti: (UTC-05:00) Eastern Time (US & Canada) Heard Island and McDonald Islands: (UTC-05:00) Bogota, Lima, Quito, Rio Branco Holy See (Vatican City): (UTC+01:00) Sarajevo, Skopje, Warsaw, Zagreb Honduras: (UTC-06:00) Central Time (US & Canada) Hong Kong SAR: (UTC+08:00) Beijing, Chongqing, Hong Kong, Urumqi Hungary: (UTC+01:00) Belgrade, Bratislava, Budapest, Ljubljana, Prague Iceland: (UTC+00:00) Monrovia, Reykjavik India: (UTC+05:30) Chennai, Kolkata, Mumbai, New Delhi Indonesia: (UTC+07:00) Bangkok, Hanoi, Jakarta Iraq: (UTC+04:00) Abu Dhabi, Muscat Ireland: (UTC+00:00) Dublin, Edinburgh, Lisbon, London Israel: (UTC+02:00) Jerusalem Italy: (UTC+01:00) Amsterdam, Berlin, Bern, Rome, Stockholm, Vienna Jamaica: (UTC-05:00) Eastern Time (US & Canada) Japan: (UTC+09:00) Osaka, Sapporo, Tokyo Jersey: (UTC+00:00) Monrovia, Reykjavik Jordan: (UTC+02:00) Chisinau Kazakhstan: (UTC+05:00) Ashgabat, Tashkent Kenya: (UTC+03:00) Nairobi Kiribati: (UTC+14:00) Kiritimati Island Korea: (UTC+09:00) Seoul Kuwait: (UTC+04:00) Abu Dhabi, Muscat Kyrgyzstan: (UTC+06:00) Astana Laos: (UTC+07:00) Bangkok, Hanoi, Jakarta Latvia: (UTC+02:00) Chisinau Lebanon: (UTC+02:00) Chisinau Lesotho: (UTC+02:00) Harare, Pretoria Liberia: (UTC+00:00) Monrovia, Reykjavik Libya: (UTC+02:00) Chisinau Liechtenstein: (UTC+01:00) Sarajevo, Skopje, Warsaw, Zagreb Lithuania: (UTC+02:00) Chisinau Luxembourg: (UTC+01:00) Sarajevo, Skopje, Warsaw, Zagreb Macao SAR: (UTC+08:00) Beijing, Chongqing, Hong Kong, Urumqi Macedonia, FYROM: (UTC+01:00) Sarajevo, Skopje, Warsaw, Zagreb Madagascar: (UTC+03:00) Nairobi Malawi: (UTC+02:00) Harare, Pretoria Malaysia: (UTC+08:00) Kuala Lumpur, Singapore Maldives: (UTC+05:00) Ashgabat, Tashkent Mali: (UTC+00:00) Monrovia, Reykjavik Malta: (UTC+01:00) Sarajevo, Skopje, Warsaw, Zagreb Man, Isle of: (UTC+00:00) Dublin, Edinburgh, Lisbon, London Marshall Islands: (UTC+12:00) Petropavlovsk-Kamchatsky - Old Martinique: (UTC-04:00) Atlantic Time (Canada) Mauritania: (UTC+00:00) Monrovia, Reykjavik Mauritius: (UTC+04:00) Port Louis Mayotte: (UTC+03:00) Nairobi Mexico: (UTC-06:00) Guadalajara, Mexico City, Monterrey Micronesia: (UTC+10:00) Guam, Port Moresby Moldova: (UTC+02:00) Chisinau Monaco: (UTC+01:00) Sarajevo, Skopje, Warsaw, Zagreb Mongolia: (UTC+07:00) Krasnoyarsk Montenegro: (UTC+01:00) Sarajevo, Skopje, Warsaw, Zagreb Montserrat: (UTC-04:00) Atlantic Time (Canada) Morocco: (UTC+01:00) Casablanca Mozambique: (UTC+02:00) Harare, Pretoria Myanmar: (UTC+06:30) Yangon (Rangoon) Namibia: (UTC+01:00) Amsterdam, Berlin, Bern, Rome, Stockholm, Vienna Nauru: (UTC+12:00) Petropavlovsk-Kamchatsky - Old Nepal: (UTC+05:45) Kathmandu Netherlands: (UTC+01:00) Amsterdam, Berlin, Bern, Rome, Stockholm, Vienna New Caledonia: (UTC+11:00) Solomon Is., New Caledonia New Zealand: (UTC+12:00) Auckland, Wellington Nicaragua: (UTC-06:00) Central Time (US & Canada) Niger: (UTC+01:00) West Central Africa Nigeria: (UTC+01:00) West Central Africa Niue: (UTC+13:00) Samoa Norfolk Island: (UTC+11:00) Solomon Is., New Caledonia Northern Mariana Islands: (UTC+10:00) Guam, Port Moresby Norway: (UTC+01:00) Amsterdam, Berlin, Bern, Rome, Stockholm, Vienna Oman: (UTC+04:00) Abu Dhabi, Muscat Pakistan: (UTC+05:00) Islamabad, Karachi Palau: (UTC+09:00) Osaka, Sapporo, Tokyo Palestinian Authority: (UTC+02:00) Chisinau Panama: (UTC-05:00) Eastern Time (US & Canada) Papua New Guinea: (UTC+10:00) Vladivostok Paraguay: (UTC-04:00) Asuncion Peru: (UTC-05:00) Bogota, Lima, Quito, Rio Branco Philippines: (UTC+08:00) Kuala Lumpur, Singapore Pitcairn Islands: (UTC-08:00) Pacific Time (US & Canada) Poland: (UTC+01:00) Belgrade, Bratislava, Budapest, Ljubljana, Prague Portugal: (UTC+00:00) Dublin, Edinburgh, Lisbon, London Qatar: (UTC+04:00) Abu Dhabi, Muscat Reunion: (UTC+04:00) Port Louis Romania: (UTC+02:00) Chisinau ROW: (UTC-07:00) Mountain Time (US & Canada) Russia: (UTC+03:00) Moscow, St. Petersburg Rwanda: (UTC+02:00) Harare, Pretoria SÃ£o TomÃ© and PrÃ­ncipe: (UTC+00:00) Monrovia, Reykjavik Saint BarthÃ©lemy: (UTC+04:00) Yerevan Saint Helena, Ascension and Tristan da Cunha: (UTC+00:00) Dublin, Edinburgh, Lisbon, London Saint Kitts and Nevis: (UTC-04:00) Atlantic Time (Canada) Saint Lucia: (UTC-04:00) Atlantic Time (Canada) Saint Martin (French Part): (UTC-04:00) Atlantic Time (Canada) Saint Pierre and Miquelon: (UTC-02:00) Mid-Atlantic - Old Saint Vincent and the Grenadines: (UTC-04:00) Atlantic Time (Canada) Samoa: (UTC+13:00) Samoa San Marino: (UTC+01:00) Sarajevo, Skopje, Warsaw, Zagreb Saudi Arabia: (UTC+03:00) Kuwait, Riyadh Senegal: (UTC+00:00) Monrovia, Reykjavik Serbia: (UTC+01:00) Sarajevo, Skopje, Warsaw, Zagreb Seychelles: (UTC+04:00) Abu Dhabi, Muscat Sierra Leone: (UTC+00:00) Monrovia, Reykjavik Singapore: (UTC+08:00) Kuala Lumpur, Singapore Sint Maarten (Dutch Part): (UTC-04:00) Atlantic Time (Canada) Slovakia: (UTC+01:00) Belgrade, Bratislava, Budapest, Ljubljana, Prague Slovenia: (UTC+01:00) Sarajevo, Skopje, Warsaw, Zagreb Solomon Islands: (UTC+11:00) Solomon Is., New Caledonia Somalia: (UTC+03:00) Nairobi South Africa: (UTC+02:00) Harare, Pretoria South Georgia and the South Sandwich Islands: (UTC-02:00) Mid-Atlantic - Old Spain: (UTC+01:00) Brussels, Copenhagen, Madrid, Paris Sri Lanka: (UTC+05:30) Chennai, Kolkata, Mumbai, New Delhi Suriname: (UTC-03:00) Cayenne, Fortaleza Svalbard and Jan Mayen: (UTC+01:00) Sarajevo, Skopje, Warsaw, Zagreb Swaziland: (UTC+02:00) Harare, Pretoria Sweden: (UTC+01:00) Amsterdam, Berlin, Bern, Rome, Stockholm, Vienna Switzerland: (UTC+01:00) Amsterdam, Berlin, Bern, Rome, Stockholm, Vienna Taiwan: (UTC+08:00) Taipei Tajikistan: (UTC+05:00) Ashgabat, Tashkent Tanzania: (UTC+03:00) Nairobi Thailand: (UTC+07:00) Bangkok, Hanoi, Jakarta Timor-Leste: (UTC+09:00) Seoul Togo: (UTC+00:00) Monrovia, Reykjavik Tokelau: (UTC+13:00) Nuku'alofa Tonga: (UTC+13:00) Nuku'alofa Trinidad and Tobago: (UTC-04:00) Atlantic Time (Canada) Tunisia: (UTC+01:00) Sarajevo, Skopje, Warsaw, Zagreb Turkey: (UTC+03:00) Istanbul Turkmenistan: (UTC+05:00) Ashgabat, Tashkent Turks and Caicos Islands: (UTC-05:00) Eastern Time (US & Canada) Tuvalu: (UTC+12:00) Petropavlovsk-Kamchatsky - Old U.S. Minor Outlying Islands: (UTC+13:00) Samoa U.S. Virgin Islands: (UTC-04:00) Atlantic Time (Canada) Uganda: (UTC+03:00) Nairobi Ukraine: (UTC+02:00) Chisinau United Arab Emirates: (UTC+04:00) Abu Dhabi, Muscat United Kingdom: (UTC+00:00) Dublin, Edinburgh, Lisbon, London United States: (UTC-05:00) Eastern Time (US & Canada) Uruguay: (UTC-03:00) Brasilia Uzbekistan: (UTC+05:00) Ashgabat, Tashkent Vanuatu: (UTC+11:00) Solomon Is., New Caledonia Vietnam: (UTC+07:00) Bangkok, Hanoi, Jakarta Wallis and Futuna: (UTC+12:00) Petropavlovsk-Kamchatsky - Old Western Sahara (Disputed): (UTC+00:00) Dublin, Edinburgh, Lisbon, London Yemen: (UTC+04:00) Abu Dhabi, Muscat Zambia: (UTC+02:00) Harare, Pretoria Zimbabwe: (UTC+02:00) Harare, Pretoria
