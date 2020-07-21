---
description: デバイス自体とそのセンサーに統合するコードには、ユーザーに対する入力と出力が含まれます。
title: I/o、デバイス、およびアプリモデルの UWP に Windows Phone Silverlight への移植
ms.assetid: bf9f2c03-12c1-49e4-934b-e3fa98919c53
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a62fcb4a208a52fd77be2a9913e265b12bf31f43
ms.sourcegitcommit: ca1b5c3ab905ebc6a5b597145a762e2c170a0d1c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/13/2020
ms.locfileid: "79210918"
---
#  <a name="porting-windowsphone-silverlight-to-uwp-for-io-device-and-app-model"></a>I/o、デバイス、およびアプリモデルの UWP に Windows Phone Silverlight への移植


前のトピックは、「[XAML と UI の移植](wpsl-to-uwp-porting-xaml-and-ui.md)」でした。

デバイス自体とそのセンサーに統合するコードには、ユーザーに対する入力と出力が含まれます。 また、データ処理を含むこともあります。 ただしこのコードは一般には、UI レイヤーまたはデータ レイヤーのいずれにも見なされません。 このコードには、振動コントローラー、加速度計、ジャイロスコープ、マイクとスピーカー (音声認識と音声合成で使います)、地理位置情報、およびタッチ、マウス、キーボード、ペンなどの入力モダリティとの統合が含まれます。

## <a name="application-lifecycle-process-lifetime-management"></a>アプリケーションのライフサイクル (プロセス ライフタイム管理)

Windows Phone Silverlight アプリには、廃棄と再アクティブ化をサポートするために、アプリケーションの状態と表示状態を保存および復元するコードが含まれています。 ユニバーサル Windows プラットフォーム (UWP) アプリのアプリライフサイクルは、Windows Phone Silverlight アプリのアプリケーションライフサイクルと非常に似ています。これらはどちらも、ユーザーが選択したアプリで使用できるリソースを最大化することと同じ目標を使用して設計されているためです。いつでもフォアグラウンドです。 コードは、新しいシステムに合理的な容易さで適合することがわかります。

[ハードウェアの**戻る**] ボタンを**押す   と**Windows Phone Silverlight アプリが自動的に終了します。 UWP アプリでは、モバイル デバイスのハードウェアの **[戻る]** ボタンを押しても自動的に終了*しません*。 その代わりに、アプリは一時停止します。その後、終了することができます。 ただし、そうした詳細は、アプリケーション ライフ サイクル イベントに適切に応答するアプリに対して透過です。

"デバウンス時間" は、アプリが非アクティブになり、システムで中断イベントが発生するまでの時間です。 UWP アプリにはデバウンス時間がありません。このため、アプリが非アクティブになるとすぐに中断イベントが発生します。

詳しくは、「[アプリのライフサイクル](https://docs.microsoft.com/windows/uwp/launch-resume/app-lifecycle)」をご覧ください。

## <a name="camera"></a>カメラ

Silverlight カメラキャプチャコード Windows Phone**は、** **CameraCaptureTask**クラスまた**は microsoft.** ................................... ユニバーサル Windows プラットフォーム (UWP) へのコードの移植では、[**MediaCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture) クラスを使うことができます。 コードの例については、[**CapturePhotoToStorageFileAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.capturephototostoragefileasync) のトピックをご覧ください。 このメソッドでは、ストレージ ファイルに写真をキャプチャできます。また、アプリ パッケージ マニフェストに**マイク**と **Web カメラ**の [**デバイス機能**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-devicecapability)を設定する必要があります。

もう 1 つのオプションは、[**CameraCaptureUI**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.CameraCaptureUI) クラスです。このクラスでも、**マイク**と **Web カメラ**の [**デバイス機能**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-devicecapability)が必要です。

レンズ アプリは、UWP アプリではサポートされません。

## <a name="detecting-the-platform-your-app-is-running-on"></a>アプリが実行されているプラットフォームの検出

Windows 10 でのアプリのターゲット変更について考える方法。 また新しい概念モデルでは、アプリはユニバーサル Windows プラットフォーム (UWP) をターゲットとし、すべての Windows デバイスで実行されます。 また、特定のデバイス ファミリ専用の機能を使うように指定することができます。 必要な場合は、アプリのターゲットを 1 つまたは複数のデバイス ファミリに限定するオプションをアプリに設定することもできます。 デバイス ファミリの説明や、ターゲットにするデバイス ファミリを決定する方法について詳しくは、「[UWP アプリのガイド](https://docs.microsoft.com/windows/uwp/get-started/universal-application-platform-guide)」をご覧ください。

**注**   機能の存在を検出するために、オペレーティングシステムまたはデバイスファミリを使用しないことをお勧めします。 通常、現在のオペレーティング システムやデバイス ファミリを識別する手法は、特定のオペレーティング システムやデバイス ファミリの機能の有無を判別する際には最適な方法ではありません。 オペレーティング システムやデバイス ファミリ (およびバージョン番号) を検出するのではなく、機能自体の存在をテストしてください (「[条件付きコンパイルとアダプティブ コード](wpsl-to-uwp-porting-to-a-uwp-project.md)」をご覧ください)。 特定のオペレーティング システムやデバイス ファミリの情報が必要な場合は、その情報を、サポートされる最小バージョンとして使ってください。そのバージョン用のテストは設計しないでください。

さまざまなデバイスに合わせてアプリの UI を調整するには、推奨される方法がいくつかあります。 これまでと同様に、自動的にサイズ調整される要素と動的レイアウト パネルを引き続き使います。 また、XAML マークアップで、有効ピクセル (以前の表示ピクセル) 単位のサイズを引き続き使います。これにより、UI がさまざまな解像度やスケール ファクターに対応します (「[表示/有効ピクセル、視聴距離、スケール ファクター](wpsl-to-uwp-porting-xaml-and-ui.md)」をご覧ください)。 Visual State Manager のアダプティブなトリガーとセッターを使って、UI をウィンドウ サイズに対応させることもできます (「[UWP アプリのガイド](https://docs.microsoft.com/windows/uwp/get-started/universal-application-platform-guide)」をご覧ください)。

ただし、デバイス ファミリの検出が必須となるシナリオの場合は、デバイス ファミリの検出を行ってください。 次の例では、[**AnalyticsVersionInfo**](https://docs.microsoft.com/uwp/api/Windows.System.Profile.AnalyticsVersionInfo) クラスを使い、必要に応じてモバイル デバイス ファミリ向けに調整されたページに移動し、該当しない場合は既定のページにフォール バックします。

```csharp
   if (Windows.System.Profile.AnalyticsInfo.VersionInfo.DeviceFamily == "Windows.Mobile")
        rootFrame.Navigate(typeof(MainPageMobile), e.Arguments);
    else
        rootFrame.Navigate(typeof(MainPage), e.Arguments);
```

アプリでは、リソースの選択に関する有効な要因に基づいて、アプリが実行されているデバイス ファミリを判別することもできます。 次の例は、この操作を命令を使って実行する方法を示しています。「[**ResourceContext.QualifierValues**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.resources.core.resourcecontext.qualifiervalues)」トピックには、このクラスを使い、デバイス ファミリに関する要因に基づいてデバイス ファミリ固有のリソースを読み込む場合の一般的な使用例が説明されています。

```csharp
var qualifiers = Windows.ApplicationModel.Resources.Core.ResourceContext.GetForCurrentView().QualifierValues;
string deviceFamilyName;
bool isDeviceFamilyNameKnown = qualifiers.TryGetValue("DeviceFamily", out deviceFamilyName);
```

「[条件付きコンパイルとアダプティブ コード](wpsl-to-uwp-porting-to-a-uwp-project.md)」もご覧ください。

## <a name="device-status"></a>デバイスの状態

Silverlight アプリ Windows Phone は、アプリケーションが実行されているデバイスに関する情報を取得するために、 **Microsoft. get-help. DeviceStatus**クラスを使用できます。 **Microsoft.Phone.Info** 名前空間に直接相当する UWP の要素はありませんが、ここでは **DeviceStatus** クラスのメンバーを呼び出す代わりに、UWP アプリで使うことができるプロパティとイベントがいくつかあります。

| Windows Phone Silverlight                                                               | UWP                                                                                                                                                                                                                                                                                                                                |
|-----------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **ApplicationCurrentMemoryUsage** プロパティと **ApplicationCurrentMemoryUsageLimit** プロパティ | [**Memorymanager. AppMemoryUsage**](https://docs.microsoft.com/uwp/api/windows.system.memorymanager.appmemoryusage)と[**Appmemoryusage limit**](https://docs.microsoft.com/uwp/api/windows.system.memorymanager.appmemoryusagelimit)プロパティ                                                                                                                                    |
| **ApplicationPeakMemoryUsage** プロパティ                                                 | Visual Studio のメモリ プロファイル ツールを使います。 詳しくは、「[メモリ使用率の分析](https://docs.microsoft.com/visualstudio/welcome-to-visual-studio-2015?view=vs-2015)」をご覧ください。                                                                                                                                                                          |
| **DeviceFirmwareVersion** プロパティ                                                      | [**Easclientdeviceinformation. SystemFirmwareVersion**](https://docs.microsoft.com/uwp/api/windows.security.exchangeactivesyncprovisioning.easclientdeviceinformation.systemfirmwareversion)プロパティ (デスクトップデバイスファミリのみ)                                                                                                                                                                             |
| **DeviceHardwareVersion** プロパティ                                                      | [**Easclientdeviceinformation. SystemHardwareVersion**](https://docs.microsoft.com/uwp/api/windows.security.exchangeactivesyncprovisioning.easclientdeviceinformation.systemhardwareversion)プロパティ (デスクトップデバイスファミリのみ)                                                                                                                                                                             |
| **DeviceManufacturer** プロパティ                                                         | [**Easclientdeviceinformation. SystemManufacturer**](https://docs.microsoft.com/uwp/api/windows.security.exchangeactivesyncprovisioning.easclientdeviceinformation.systemmanufacturer)プロパティ (デスクトップデバイスファミリのみ)                                                                                                                                                                                |
| **DeviceName** プロパティ                                                                 | [**Easclientdeviceinformation. SystemProductName**](https://docs.microsoft.com/uwp/api/windows.security.exchangeactivesyncprovisioning.easclientdeviceinformation.systemproductname)プロパティ (デスクトップデバイスファミリのみ)                                                                                                                                                                                 |
| **DeviceTotalMemory** プロパティ                                                          | 相当する要素なし                                                                                                                                                                                                                                                                                                                      |
| **IsKeyboardDeployed** プロパティ                                                         | 相当する要素なし。 このプロパティは、モバイル デバイスのハードウェア キーボードに関する情報を提供します。                                                                                                                                                                                                        |
| **IsKeyboardPresent** プロパティ                                                          | 相当する要素なし。 このプロパティは、モバイル デバイスのハードウェア キーボードに関する情報を提供します。                                                                                                                                                                                                        |
| **KeyboardDeployedChanged** イベント                                                       | 相当する要素なし。 このプロパティは、モバイル デバイスのハードウェア キーボードに関する情報を提供します。                                                                                                                                                                                                        |
| **PowerSource** プロパティ                                                                | 相当する要素なし                                                                                                                                                                                                                                                                                                                      |
| **PowerSourceChanged** イベント                                                            | [  **RemainingChargePercentChanged**](https://docs.microsoft.com/uwp/api/windows.phone.devices.power.battery.remainingchargepercentchanged) イベントを処理します (モバイル デバイス ファミリのみ)。 このイベントは、[**RemainingChargePercent**](https://docs.microsoft.com/uwp/api/windows.phone.devices.power.battery.remainingchargepercent) プロパティ (モバイル デバイス ファミリのみ) の値が 1% 減少すると発生します。 |

## <a name="location"></a>場所

アプリパッケージマニフェスト内の場所機能を宣言するアプリが Windows 10 で実行されると、エンドユーザーに同意を求めるメッセージが表示されます。 アプリが独自の同意プロンプトを表示する場合や、オン/オフ切り替えを提供する場合、エンド ユーザーの確認を 1 回のみにするためにその機能を削除できます。

## <a name="orientation"></a>[方向]

UWP アプリで **PhoneApplicationPage.SupportedOrientations** プロパティと **Orientation** プロパティに相当するのは、アプリ パッケージ マニフェストの [**uap:InitialRotationPreference**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-splashscreen) 要素です。 まだ選択されていない場合は、 **[アプリケーション]** タブを選択し、設定を記録するために **[サポートされる回転]** の 1 つ以上のチェック ボックスをオンにします。

ただし、デバイスの向きと画面サイズにかかわらず、UWP アプリの UI が適切に表示されるように設計することをお勧めします。 詳しくは、次の次のトピック「[フォーム ファクターとユーザー エクスペリエンスの移植](wpsl-to-uwp-form-factors-and-ux.md)」をご覧ください。

次のトピックは、「[ビジネス レイヤーとデータ レイヤーの移植](wpsl-to-uwp-business-and-data.md)」です。

