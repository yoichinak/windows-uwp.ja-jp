---
title: Windows 設定アプリの起動
description: Ms 設定 URI スキームを使用して、アプリから Windows 設定アプリを起動する方法について説明します。
ms.assetid: C84D4BEE-1FEE-4648-AD7D-8321EAC70290
ms.date: 11/18/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: contperf-fy21q2
dev_langs:
- csharp
- cppwinrt
ms.openlocfilehash: 78d843ff9bdd0625b7172af0bc973581d8bd988a
ms.sourcegitcommit: 4cafc1c55511741dd1e5bfe4496d9950a9b4de1b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/04/2021
ms.locfileid: "97860416"
---
# <a name="launch-the-windows-settings-app"></a>Windows 設定アプリの起動

**重要な API**

-   [**LaunchUriAsync**](/uwp/api/windows.system.launcher.launchuriasync)
-   [**PreferredApplicationPackageFamilyName**](/uwp/api/windows.system.launcheroptions.preferredapplicationpackagefamilyname)
-   [**DesiredRemainingView**](/uwp/api/windows.system.launcheroptions.desiredremainingview)

Windows 設定アプリを起動する方法について説明します。 このトピックでは、 **ms 設定:** URI スキームについて説明します。 Windows 設定アプリを起動して特定の設定ページを表示するには、この URI スキームを使います。

設定アプリの起動は、個人データにアクセスするアプリの開発の重要な部分です。 アプリが機密性の高いリソースにアクセスできない場合、そのリソースのプライバシー設定への便利なリンクをユーザーに提供することをお勧めします。 詳しくは、「[個人データにアクセスするアプリのガイドライン](../security/index.md)」をご覧ください。

## <a name="how-to-launch-the-settings-app"></a>設定アプリを起動する方法

**設定** アプリを起動するには、次の例に示されているように `ms-settings:` URI スキームを使います。

### <a name="xaml-hyperlink-control"></a>XAML ハイパーリンクコントロール

この例では、ハイパーリンク XAML コントロールで `ms-settings:privacy-microphone` URI を使って、マイクのプライバシー設定ページを起動します。

```xml
<!--Set Visibility to Visible when access to the microphone is denied -->
<TextBlock x:Name="LocationDisabledMessage" FontStyle="Italic"
                 Visibility="Collapsed" Margin="0,15,0,0" TextWrapping="Wrap" >
          <Run Text="This app is not able to access the microphone. Go to " />
              <Hyperlink NavigateUri="ms-settings:privacy-microphone">
                  <Run Text="Settings" />
              </Hyperlink>
          <Run Text=" to check the microphone privacy settings."/>
</TextBlock>
```

### <a name="calling-launchuriasync"></a>LaunchUriAsync の呼び出し

また、アプリで [**LaunchUriAsync**](/uwp/api/windows.system.launcher.launchuriasync) メソッドを呼び出し、コードから **設定** アプリを起動することもできます。 次の例は、`ms-settings:privacy-webcam` URI を使って、カメラのプライバシー設定ページを起動する方法を示しています。

```cs
bool result = await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-settings:privacy-webcam"));
```

```cppwinrt
bool result = co_await Windows::System::Launcher::LaunchUriAsync(Windows::Foundation::Uri(L"ms-settings:privacy-webcam"));
```

上記のコードでは、カメラのプライバシー設定ページが起動されます。

:::image type="content" source="images/privacyawarenesssettingsapp.png" alt-text="カメラのプライバシー設定。":::

URI の起動について詳しくは、「[URI に応じた既定のアプリの起動](launch-default-app.md)」をご覧ください。

## <a name="ms-settings-uri-scheme-reference"></a>ms-settings: URI スキーム リファレンス

次のセクションでは、設定アプリのさまざまなページを開くために使用する ms 設定 Uri のさまざまなカテゴリについて説明します。

* [Accounts](#accounts)
* [アプリ](#apps)
* [Cortana](#cortana)
* [デバイス](#devices)
* [簡単なアクセス](#ease-of-access)
* [追加](#extras)
* [Gaming](#gaming)
* [ホーム ページ](#home-page)
* [複合現実](#mixed-reality)
* [ネットワークとインターネット](#network-and-internet)
* [個人用設定](#personalization)
* [Phone](#phone)
* [プライバシー](#privacy)
* [Surface Hub](#surface-hub)
* [システム](#system)
* [時刻と言語](#time-and-language)
* [Update and security (更新とセキュリティ)](#update-and-security)
* [ユーザー アカウント](#user-accounts)



> [!NOTE]
> 設定ページが使用可能かどうかは、Windows SKU によって異なります。 デスクトップの Windows 10 で利用可能な設定ページのすべてを Windows 10 Mobile で利用できるとは限りません。またその逆も同様です。 注の列には、ページが使用可能となるための追加の要件も記されています。

<!-- TODO: 
* ms-settings:controlcenter
* ms-settings:holographic
* ms-settings:keyboard-advanced
* ms-settings:regionlanguage-adddisplaylanguage (crashed)
* ms-settings:regionlanguage-setdisplaylanguage (crashed)
* ms-settings:signinoptions-launchpinenrollment
* ms-settings:storagecleanup
* ms-settings:update-security -->

### <a name="accounts"></a>アカウント

|[設定] ページ| URI |
|-------------|-----|
| 職場または学校にアクセスする | ms-settings:workplace |
| メール & アプリのアカウント  | ms-settings:emailandaccounts |
| 家族とその他のユーザー | ms-settings:otherusers |
| キオスクの設定 | ms-設定: assignedaccess |
| サインイン オプション | ms-settings:signinoptions<br>ms-settings:signinoptions-dynamiclock |
| 設定の同期 | ms-settings:sync |
| Windows Hello セットアップ | ms-settings:signinoptions-launchfaceenrollment<br>ms-settings:signinoptions-launchfingerprintenrollment |
| ユーザーの情報 | ms-settings:yourinfo |

### <a name="apps"></a>アプリ

|[設定] ページ| URI |
|-------------|-----|
| アプリと機能 | ms-settings:appsfeatures |
| アプリの機能 | ms-settings:appsfeatures-app (アプリのリセット、アドオンの管理およびダウンロード可能なコンテンツなど)|
| Web サイト用のアプリ | ms-settings:appsforwebsites |
| 既定のアプリ | ms-settings:defaultapps |
| オプション機能の管理 | ms-settings:optionalfeatures |
| オフライン マップ | ms-settings:maps<br/>ms 設定: maps-downloadmaps (ダウンロードマップ) |
| スタートアップ アプリ | ms-settings:startupapps |
| ビデオ再生 | ms-settings:videoplayback |

### <a name="cortana"></a>Cortana

|[設定] ページ| URI |
|-------------|-----|
| [デバイス間での Cortana] | ms-settings:cortana-notifications |
| 詳細 | ms-settings:cortana-moredetails |
| アクセス許可 & 履歴 | ms-settings:cortana-permissions |
| 検索 (ウィンドウを) | ms 設定: cortana-windowssearch |
| Cortana に話す | ms-settings:cortana-language<br/>ms 設定: cortana<br/>ms 設定: cortana-talktocortana |

> [!NOTE] 
> デスクトップのこの設定セクションは、Cortana が現在使用できないリージョンまたは Cortana が無効になっているリージョンに PC が設定されている場合に、Search と呼ばれます。 Cortana 固有のページ (デバイス間での Cortana) は、この場合は表示されません。 

### <a name="devices"></a>デバイス

|[設定] ページ| URI |
|-------------|-----|
| AutoPlay | ms-settings:autoplay |
| Bluetooth | ms-settings:bluetooth |
| 接続されたデバイス | ms-settings:connecteddevices |
| 既定のカメラ | ms 設定: カメラ (**Windows 10 バージョン1809以降では非推奨**) |
| マウスとタッチパッド | ms-settings:mousetouchpad (タッチパット設定は、タッチパッドが搭載されているデバイスでのみ利用可能) |
| ペンと Windows Ink | ms-settings:pen |
| プリンターとスキャナー | ms-settings:printers |
| Touchpad | ms-settings:devices-touchpad (タッチパッド ハードウェアが搭載されている場合のみ利用可能) |
| Typing (入力) | ms-settings:typing |
| USB | ms-settings:usb |
| Wheel | ms-settings:wheel (Dial がペアリングされている場合のみ利用可能) |
| 同期電話 | ms-settings:mobile-devices  |

### <a name="ease-of-access"></a>簡単操作

|[設定] ページ| URI |
|-------------|-----|
| オーディオ | ms-settings:easeofaccess-audio |
| クローズド キャプション | ms-settings:easeofaccess-closedcaptioning |
| 色フィルター | ms 設定: easeofaccess-colorfilter |
| カーソルとポインター サイズ | ms 設定: easeofaccess-カーソルとポインターのサイズ |
| ディスプレイ | ms-settings:easeofaccess-display |
| 視線制御 | ms-settings:easeofaccess-eyecontrol |
| フォント | ms-settings:fonts |
| ハイ コントラスト | ms-settings:easeofaccess-highcontrast |
| キーボード | ms-settings:easeofaccess-keyboard |
| 拡大鏡 | ms-settings:easeofaccess-magnifier |
| マウス | ms-settings:easeofaccess-mouse |
| Narrator | ms-settings:easeofaccess-narrator |
| その他のオプション | ms 設定: easeofaccess-otheroptions (**Windows 10 バージョン1809以降では非推奨**) |
| 音声 | ms-settings:easeofaccess-speechrecognition |

### <a name="extras"></a>追加

|[設定] ページ| URI |
|-------------|-----|
| 追加 | ms 設定: エクストラ (たとえば、サードパーティによって "設定アプリ" がインストールされている場合のみ使用可能) |

### <a name="gaming"></a>Gaming

|[設定] ページ| URI |
|-------------|-----|
| ブロードキャスト | ms-settings:gaming-broadcasting |
| ゲーム バー | ms-settings:gaming-gamebar |
| ゲーム DVR | ms-settings:gaming-gamedvr |
| ゲーム モード | ms-settings:gaming-gamemode |
| ゲームの全画面表示の再生 | ms-settings:quietmomentsgame |
| TruePlay | ms 設定: trueplay (**Windows 10 バージョン1809以降 (10.0;ビルド 17763)、この機能は Windows から削除されています**) |
| Xbox ネットワーク | ms-settings:gaming-xboxnetworking |

### <a name="home-page"></a>ホーム ページ

|[設定] ページ| URI |
|-------------|-----|
| 設定ホーム ページ | ms-settings: |

### <a name="mixed-reality"></a>複合現実

> [!NOTE]
> これらの設定は、Mixed Reality ポータルアプリがインストールされている場合にのみ使用できます。

| [設定] ページ | URI |
|---------------|-----|
| オーディオと音声認識 | ms-settings:holographic-audio |
| 環境 | ms-設定: プライバシー-holographic-環境 |
| ヘッドセット ディスプレイ | ms-設定: holographic |
| アンインストール | ms-設定: holographic |

### <a name="network-and-internet"></a>ネットワークとインターネット

|[設定] ページ| URI |
|-------------|-----|
| 機内モード | ms-settings:network-airplanemode<br/>ms-settings:proximity |
| 携帯ネットワークと SIM | ms-settings:network-cellular |
| データの使用 | ms-settings:datausage |
| ダイヤルアップ | ms-settings:network-dialup |
| DirectAccess | ms-settings:network-directaccess (DirectAccess が有効な場合にのみ利用可能) |
| イーサネット | ms-settings:network-ethernet |
| 既知のネットワークの管理 | ms-settings:network-wifisettings |
| モバイルホットスポット | ms-settings:network-mobilehotspot |
| NFC | ms-settings:nfctransactions |
| プロキシ | ms-settings:network-proxy |
| Status | ms-settings:network-status<br/>ms-設定: ネットワーク |
| VPN | ms-settings:network-vpn |
| Wi-Fi | ms-settings:network-wifi (デバイスに wifi アダプターがある場合にのみ利用可能) |
| Wi-Fi 通話 | ms-settings:network-wificalling (Wi-Fi 通話が有効な場合のみ利用可能) |

### <a name="personalization"></a>個人設定

|[設定] ページ| URI |
|-------------|-----|
| バックグラウンド | ms-settings:personalization-background |
| スタート時にどのフォルダを表示するかを選択する | ms-settings:personalization-start-places |
| 色 | ms-settings:personalization-colors<br/>ms-設定: 色 |
| 概要 | ms-設定: カスタマイズの概要 (**Windows 10 バージョン1809以降では非推奨**) |
| ロック画面 | ms-settings:lockscreen |
| ナビゲーション バー | ms 設定: 個人用設定-ナビゲーションバー (**Windows 10 バージョン1809以降では非推奨**) |
| 個人用設定 (カテゴリ) | ms-settings:personalization |
| 開始 | ms-settings:personalization-start |
| タスク バー | ms-settings:taskbar |
| テーマ | ms-settings:themes |

### <a name="phone"></a>Phone

|[設定] ページ| URI |
|-------------|-----|
| 同期電話 | ms-settings:mobile-devices<br/>ms 設定: モバイルデバイス-addphone<br/>ms 設定: モバイルデバイス-addphone-direct ( **電話** アプリを開きます) |

### <a name="privacy"></a>プライバシー

|[設定] ページ| URI |
|-------------|-----|
| アクセサリ用アプリ | ms 設定: プライバシー-accessoryapps (**Windows 10 バージョン1809以降では非推奨**) |
| アカウント情報 | ms-settings:privacy-accountinfo |
| アクティビティの履歴 | ms-settings:privacy-activityhistory |
| 広告識別子 | ms 設定: プライバシー-advertisingid (**Windows 10 バージョン1809以降では非推奨**) |
| アプリの診断 | ms-settings:privacy-appdiagnostics |
| ファイルの自動ダウンロード | ms-settings:privacy-automaticfiledownloads |
| バックグラウンド アプリ | ms-settings:privacy-backgroundapps |
| Calendar | ms-settings:privacy-calendar |
| 通話履歴 | ms-settings:privacy-callhistory |
| カメラ | ms-settings:privacy-webcam |
| 連絡先 | ms-settings:privacy-contacts |
| ドキュメント | ms-settings:privacy-documents |
| 電子メール | ms-settings:privacy-email |
| アイ トラッカー | ms-settings:privacy-eyetracker (eyetracker ハードウェアが必要) |
| フィードバックと診断 | ms-settings:privacy-feedback |
| ファイル システム | ms-settings:privacy-broadfilesystemaccess |
| 全般 | ms-設定: プライバシーまたは ms 設定: プライバシー-全般 |
| インク & 入力 |ms-settings:privacy-speechtyping |
| 場所 | ms-settings:privacy-location |
| メッセージング | ms-settings:privacy-messaging |
| マイク | ms-settings:privacy-microphone |
| モーション | ms-settings:privacy-motion |
| 通知 | ms-settings:privacy-notifications |
| その他のデバイス | ms-settings:privacy-customdevices |
| 電話 | ms-設定: プライバシー-phonecalls |
| ピクチャ | ms-settings:privacy-pictures |
| 無線 | ms-settings:privacy-radios |
| 音声 | ms-設定: プライバシー-音声 |
| タスク | ms-settings:privacy-tasks |
| ビデオ | ms-settings:privacy-videos |
| 音声のアクティブ化 | ms-設定: プライバシー-voiceactivation |

### <a name="surface-hub"></a>Surface Hub

|[設定] ページ| URI |
|-------------|-----|
| アカウント | ms-settings:surfacehub-accounts |
| セッションのクリーンアップ | ms-settings:surfacehub-sessioncleanup |
| チーム会議 | ms-settings:surfacehub-calling |
| チーム デバイス管理 | ms-settings:surfacehub-devicemanagenent |
| [ようこそ] 画面 | ms-settings:surfacehub-welcome |

### <a name="system"></a>システム

|[設定] ページ| URI |
|-------------|-----|
| 詳細 | ms-settings:about |
| ディスプレイの詳細設定 | ms-settings:display-advanced (詳細オプションをサポートするデバイスでのみ利用可能) |
| アプリのボリュームとデバイスの基本設定 | ms 設定: アプリ-ボリューム (**Windows 10 バージョン1903で追加**)|
| バッテリー節約機能 | ms-settings:batterysaver (タブレットなど、バッテリーを搭載したデバイスでのみ利用可能) |
| バッテリー節約機能の設定 | ms-settings:batterysaver-settings (タブレットなど、バッテリーを搭載したデバイスでのみ利用可能) |
| バッテリーの使用状況 | ms-settings:batterysaver-usagedetails (タブレットなど、バッテリーを搭載したデバイスでのみ利用可能) |
| クリップボードのトピック | ms-設定: クリップボード |
| ディスプレイ | ms-settings:display |
| 既定の保存場所 | ms-settings:savelocations |
| ディスプレイ | ms-settings:screenrotation |
| ディスプレイの複製 | ms-settings:quietmomentspresentation |
| これらの時間帯 | ms-settings:quietmomentsscheduled |
| 暗号化 | ms-settings:deviceencryption |
| 集中モード | ms-settings:quiethours <br> ms-settings:quietmomentshome |
| グラフィック設定 | ms-settings:display-advancedgraphics (詳細グラフィック オプションをサポートするデバイスでのみ利用可能) |
| メッセージング | ms-settings:messaging |
| マルチタスキング | ms-settings:multitasking |
| 夜間モード設定 | ms-settings:nightlight |
| Phone | ms-settings:phone-defaultapps |
| この PC へのプロジェクション | ms-settings:project |
| 共有エクスペリエンス | ms-settings:crossdevice |
| タブレット モード | ms-settings:tabletmode |
| タスク バー | ms-settings:taskbar |
| 通知とアクション | ms-settings:notifications |
| リモート デスクトップ | ms-settings:remotedesktop |
| Phone | ms 設定: phone (**Windows 10 バージョン1809以降では非推奨**) |
| 電源とスリープ | ms-settings:powersleep |
| サウンド | ms 設定: サウンド |
| ストレージ | ms-settings:storagesense |
| ストレージ センサー | ms-settings:storagepolicies |

### <a name="time-and-language"></a>時刻と言語

|[設定] ページ| URI |
|-------------|-----|
| 日時 | ms-settings:dateandtime |
| 日本 IME の設定 | ms-settings:regionlanguage-jpnime (Microsoft 日本語入力方式エディターがインストールされている場合に利用可能) |
| Region | ms 設定: regionformatting 設定 |
| Language | ms-設定: キーボード<br/>ms-settings:regionlanguage<br/>ms-設定: regionlanguage-bpmfime<br/>ms-設定: regionlanguage-cangjieime<br/>ms 設定: regionlanguage-chsime-domainlexicon<br/>ms 設定: regionlanguage-chsime-keyconfig<br/>ms 設定: regionlanguage-chsime-udp<br/>ms-設定: regionlanguage-chsime-wubi-udp<br/>ms 設定: regionlanguage-quickime |
| Pinyin IME の設定 | ms-settings:regionlanguage-chsime-pinyin (Microsoft Pinyin 入力方式エディターがインストールされている場合に利用可能) |
| 音声 | ms-settings:speech |
| Wubi IME の設定  | ms-settings:regionlanguage-chsime-wubi (Microsoft Wubi 入力方式エディターがインストールされている場合に利用可能) |

### <a name="update-and-security"></a>Update and security (更新とセキュリティ)

|[設定] ページ| URI |
|-------------|-----|
| アクティブ化 | ms-settings:activation |
| バックアップ | ms-settings:backup |
| 配信の最適化 | ms-settings:delivery-optimization |
| デバイスの検索 | ms-settings:findmydevice |
| 開発者向け | ms-settings:developers |
| Recovery | ms-settings:recovery |
| トラブルシューティング | ms-settings:troubleshoot |
| Windows セキュリティ | ms-settings:windowsdefender |
| Windows Insider Program | ms-settings:windowsinsider(ユーザーが WIP に登録されている場合にのみ存在)<br/>ms 設定: windowsinsider-optin |
| Windows Update | ms-settings:windowsupdate<br>ms-settings:windowsupdate-action |
| Windows Update - 詳細オプション | ms-settings:windowsupdate-options |
| Windows Update - 再起動オプション | ms-settings:windowsupdate-restartoptions |
| Windows Update - 更新履歴の表示 | ms-settings:windowsupdate-history |

### <a name="user-accounts"></a>ユーザー アカウント

|[設定] ページ| URI |
|-------------|-----|
| プロビジョニング | ms-settings:workplace-provisioning (企業でプロビジョニング パッケージが展開されている場合のみ利用可能) |
| プロビジョニング | ms-settings:workplace-provisioning (企業でプロビジョニング パッケージが展開されている場合にモバイルでのみ利用可能) |
| Windows Anywhere | ms-settings:windowsanywhere (デバイスが Windows Anywhere に対応している必要がある) |
