---
description: デバイス自体とそのセンサーに統合するコードには、ユーザーに対する入力と出力が含まれます。
title: 入出力、デバイス、アプリ モデルの Windows ランタイム 8.x から UWP への移植'
ms.assetid: bb13fb8f-bdec-46f5-8640-57fb0dd2d85b
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b6d319b901791a7c6b4b6b2fd93e8c217a3caef2
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167536"
---
# <a name="porting-windows-runtime-8x-to-uwp-for-io-device-and-app-model"></a>入出力、デバイス、アプリ モデルの Windows ランタイム 8.x から UWP への移植




前のトピックは、「[XAML と UI の移植](w8x-to-uwp-porting-xaml-and-ui.md)」でした。

デバイス自体とそのセンサーに統合するコードには、ユーザーに対する入力と出力が含まれます。 また、データ処理を含むこともあります。 ただしこのコードは一般には、UI レイヤー*または*データ レイヤーのいずれにも見なされません。 このコードには、振動コントローラー、加速度計、ジャイロスコープ、マイクとスピーカー (音声認識と音声合成で使います)、地理位置情報、およびタッチ、マウス、キーボード、ペンなどの入力モダリティとの統合が含まれます。

## <a name="application-lifecycle-process-lifetime-management"></a>アプリケーションのライフサイクル (プロセス ライフタイム管理)


ユニバーサル 8.1 アプリでは、アプリが非アクティブになり、システムで中断イベントが発生するまでの間に、2 秒間の "デバウンス時間" があります。 このデバウンス時間を猶予時間として使い、中断状態にさせることは安全な方法ではありません。また、ユニバーサル Windows プラットフォーム (UWP) アプリにはデバウンス時間がありません。このため、アプリが非アクティブになるとすぐに中断イベントが発生します。

詳しくは、「[アプリのライフサイクル](../launch-resume/app-lifecycle.md)」をご覧ください。

## <a name="background-audio"></a>バックグラウンド オーディオ


[**MediaElement.AudioCategory**](/uwp/api/windows.ui.xaml.controls.mediaelement.audiocategory) プロパティについては、**ForegroundOnlyMedia** と **BackgroundCapableMedia** が Windows 10 アプリでは使われなくなりました。 代わりに Windows Phone ストア アプリ モデルを使ってください。 詳しくは、「[バックグラウンド オーディオ](../audio-video-camera/background-audio.md)」をご覧ください。

## <a name="detecting-the-platform-your-app-is-running-on"></a>アプリが実行されているプラットフォームの検出


アプリの対応に関する考え方は、Windows 10 で変わりました。 また新しい概念モデルでは、アプリはユニバーサル Windows プラットフォーム (UWP) をターゲットとし、すべての Windows デバイスで実行されます。 また、特定のデバイス ファミリ専用の機能を使うように指定することができます。 必要な場合は、アプリのターゲットを 1 つまたは複数のデバイス ファミリに限定するオプションをアプリに設定することもできます。 デバイス ファミリの説明や、ターゲットにするデバイス ファミリを決定する方法について詳しくは、「[UWP アプリのガイド](../get-started/universal-application-platform-guide.md)」をご覧ください。

ユニバーサル 8.1 アプリが実行されているオペレーティング システムを検出するコードがそのアプリに含まれている場合、ロジックによってはそのコードの変更が必要になることがあります。 アプリが値をパススルーし、その値を処理しないとき、場合によっては、オペレーティング システムに関する情報を継続して収集する必要があります。

**メモ**   機能の存在を検出するために、オペレーティングシステムまたはデバイスファミリを使用しないことをお勧めします。 通常、現在のオペレーティング システムやデバイス ファミリを識別する手法は、特定のオペレーティング システムやデバイス ファミリの機能の有無を判別する際には最適な方法ではありません。 オペレーティング システムやデバイス ファミリ (およびバージョン番号) を検出するのではなく、機能自体の存在をテストしてください (「[条件付きコンパイルとアダプティブ コード](w8x-to-uwp-porting-to-a-uwp-project.md)」をご覧ください)。 特定のオペレーティング システムやデバイス ファミリの情報が必要な場合は、その情報を、サポートされる最小バージョンとして使ってください。そのバージョン用のテストは設計しないでください。

 

さまざまなデバイスに合わせてアプリの UI を調整するには、推奨される方法がいくつかあります。 これまでと同様に、自動的にサイズ調整される要素と動的レイアウト パネルを引き続き使います。 また、XAML マークアップで、有効ピクセル (以前の表示ピクセル) 単位でサイズを引き続き使います。これにより、UI がさまざまな解像度やスケール ファクターに対応します (「[有効ピクセル、視聴距離、スケール ファクター](w8x-to-uwp-porting-xaml-and-ui.md)」をご覧ください)。 Visual State Manager のアダプティブなトリガーとセッターを使って、UI をウィンドウ サイズに対応させることもできます (「[UWP アプリのガイド](../get-started/universal-application-platform-guide.md)」をご覧ください)。

ただし、デバイス ファミリの検出が必須となるシナリオの場合は、デバイス ファミリの検出を行ってください。 次の例では、[**AnalyticsVersionInfo**](/uwp/api/Windows.System.Profile.AnalyticsVersionInfo) クラスを使い、必要に応じてモバイル デバイス ファミリ向けに調整されたページに移動し、該当しない場合は既定のページにフォール バックします。

```csharp
   if (Windows.System.Profile.AnalyticsInfo.VersionInfo.DeviceFamily == "Windows.Mobile")
        rootFrame.Navigate(typeof(MainPageMobile), e.Arguments);
    else
        rootFrame.Navigate(typeof(MainPage), e.Arguments);
```

アプリでは、リソースの選択に関する有効な要因に基づいて、アプリが実行されているデバイス ファミリを判別することもできます。 次の例は、この操作を命令を使って実行する方法を示しています。「[**ResourceContext.QualifierValues**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.qualifiervalues)」トピックには、このクラスを使い、デバイス ファミリに関する要因に基づいてデバイス ファミリ固有のリソースを読み込む場合の一般的な使用例が説明されています。

```csharp
var qualifiers = Windows.ApplicationModel.Resources.Core.ResourceContext.GetForCurrentView().QualifierValues;
string deviceFamilyName;
bool isDeviceFamilyNameKnown = qualifiers.TryGetValue("DeviceFamily", out deviceFamilyName);
```

「[条件付きコンパイルとアダプティブ コード](w8x-to-uwp-porting-to-a-uwp-project.md)」もご覧ください。

## <a name="location"></a>場所


アプリ パッケージ マニフェストで位置情報機能を宣言するアプリを Windows 10 で実行する場合、システムはエンド ユーザーに同意を求めます。 これは、アプリが Windows Phone ストア アプリの場合でも、Windows 10 アプリの場合でも同じです。 アプリが独自の同意プロンプトを表示する場合や、オン/オフ切り替えを提供する場合、エンド ユーザーの確認を 1 回のみにするためにその機能を削除できます。

 

 