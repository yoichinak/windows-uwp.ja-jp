---
title: アプリへの市販デモ (RDX) 機能の追加
description: 小売のデモモード用にアプリを準備し、小売店の販売フロアでアプリを紹介します。
ms.assetid: f83f950f-7fdd-4f18-8127-b92a8f400061
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10, UWP, 市販デモ アプリ
ms.localizationpriority: medium
ms.openlocfilehash: 39f1cb7439c02f215824c6c632fb2e2fc6afdb39
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89164536"
---
# <a name="add-retail-demo-rdx-features-to-your-app"></a>アプリへの市販デモ (RDX) 機能の追加

販売フロアで PC やデバイスを試しているユーザーがすぐに開始できるように、Windows アプリに小売デモ モードを含めます。

顧客が小売店にいる場合は、Pc とデバイスのデモを試すことができることを期待しています。 多くの場合、 [小売のデモエクスペリエンス (RDX)](/windows-hardware/customize/desktop/retail-demo-experience)を通じて、アプリの時間を大量に消費します。

_通常_モードまたは_リテール_モードで、さまざまなエクスペリエンスを提供するようにアプリを設定できます。 たとえば、アプリがセットアッププロセスで開始された場合は、それをリテールモードでスキップし、サンプルデータと既定の設定を使用してアプリを事前に設定して、すぐに使用できるようにすることができます。

お客様の観点から見ると、アプリは1つだけです。 2つのモードを区別できるようにするために、アプリがリテールモードのときは、タイトルバーまたは適切な場所に "小売" という語が目立つように表示されることをお勧めします。

RDX 対応アプリは、アプリの Microsoft Store 要件に加えて、RDX のセットアップ、クリーンアップ、および更新プロセスとの互換性も必要とします。これにより、小売店で常に良好なエクスペリエンスが得られるようになります。

## <a name="design-principles"></a>設計原則

* **最適なものを表示**します。 リテールデモエクスペリエンスを使用して、アプリが岩を持つ理由を紹介します。 これは、お客様が最初にアプリを表示したときに、最適なものを表示するためです。

* **素早く表示**します。 お客様に見ていただける時間は限られています。アプリの真価がすぐに実感できるように構成してください。

* **ストーリーを単純に**します。 リテールデモエクスペリエンスは、アプリの価値を示すエレベーターピッチです。

* **エクスペリエンスを重視**します。 お客様がコンテンツを理解する時間を設けましょう。 魅力的な部分をすばやく伝えることは重要ですが、適切な空白時間を設けることでエクスペリエンスがさらに向上します。

## <a name="technical-requirements"></a>技術的な要件

RDX 対応のアプリは、小売顧客にとって最も優れたアプリを示すことを目的としているため、技術的な要件を満たし、すべての小売りデモエクスペリエンスアプリに Microsoft Store が持つプライバシー規制に準拠する必要があります。

これは、検証プロセスを準備し、テストプロセスを明確にするためのチェックリストとして使用できます。 これらの要件は、検証プロセスだけでなく、アプリが市販デモ デバイスで実行される限り、市販デモ エクスペリエンス アプリのライフタイム全体にわたって満たす必要があります。

### <a name="critical-requirements"></a>重要な要件

これらの重要な要件を満たしていない RDX 対応のアプリは、できるだけ早くすべてのリテールデモデバイスから削除されます。

* **個人を特定できる情報 (PII) を要求しないで**ください。 これには、ログイン情報、Microsoft アカウント情報、または連絡先の詳細が含まれます。

* **エラー-無料エクスペリエンス**。 アプリは、エラーなしで動作する必要があります。 また市販デモ デバイスを使うお客様に、エラー ポップアップやエラー通知を表示してはなりません。 エラーは、アプリ自体、ブランド、デバイスのブランド、デバイスの manufacturer's ブランド、Microsoft のブランドに悪影響を及ぼします。

* **有料アプリには試用版モードが必要**です。 アプリは無料であるか、 [試用モード](./exclude-or-limit-features-in-a-trial-version-of-your-app.md)が含まれている必要があります。 お客様は小売店での試用に料金を支払うことは想定していません。

### <a name="high-priority-requirements"></a>優先順位の高い要件

これらの優先順位の高い要件を満たしていない RDX 対応アプリは、直ちに修正するために調査する必要があります。 直ちに修正されない場合、このアプリをすべての市販デモ デバイスから削除することがあります。

* **覚えやすいオフラインエクスペリエンス**。 アプリでは、小売店でオフラインのデバイスの50% がオフラインであるため、優れたオフラインエクスペリエンスを実証する必要があります。 この要件は、お客様がオフラインでアプリを操作する場合でも、意味のある肯定的なエクスペリエンスを保証することを目的としています。

* **コンテンツエクスペリエンスが更新されました**。 オンラインの場合、アプリは更新プログラムのプロンプトを表示しません。 更新プログラムが必要な場合は、サイレントモードで実行する必要があります。

* **匿名通信はありません**。 小売デモデバイスを使用しているお客様は、匿名ユーザーであるため、デバイスからメッセージを送受信したり、コンテンツを共有したりすることはできません。

* **クリーンアッププロセスを使用して、一貫したエクスペリエンスを提供**します。 市販デモ デバイスは、使用開始にあたってすべてのお客様に同じエクスペリエンスを提供する必要があります。 アプリは [クリーンアッププロセス](#cleanup-process) を使用して、各使用後に同じ既定の状態に戻す必要があります。 次の顧客には、最後の顧客が残っていることを確認する必要がありません。 これには、スコアボード、達成度、ロック解除が含まれます。

* **適切なコンテンツを保存**します。 すべてのアプリコンテンツには、Teen または低い評価カテゴリを割り当てる必要があります。 詳細については、「IARC と[ESRB の評価](https://www.esrb.org/ratings/ratings_guide.aspx)[によるアプリの取得](https://www.globalratings.com/for-developers.aspx)」を参照してください。

### <a name="medium-priority-requirements"></a>中程度の優先順位の要件

Windows リテール ストア チームは、これらの問題の修正方法について、直接開発者に連絡して話し合いの場を設けることがあります。

* **さまざまなデバイスで正常に実行でき**ます。 アプリは、ローエンド仕様のデバイスを含め、すべてのデバイスで適切に実行する必要があります。 最小仕様を満たしていないデバイスにアプリがインストールされている場合、アプリはこのことをユーザーに明確に通知する必要があります。 アプリが常に高いパフォーマンスで動作できるように、最小のデバイス要件を明らかにする必要があります。

* **小売店のアプリのサイズ要件を満たし**ている。 アプリのサイズは、800 MB 未満である必要があります。 RDX 対応アプリがサイズ要件を満たしていない場合、詳細については、Windows 小売店チームに直接お問い合わせください。

## <a name="retailinfo-api-preparing-your-code-for-demo-mode"></a>RetailInfo API: デモモード用のコードの準備

### <a name="isdemomodeenabled"></a>IsDemoModeEnabled
[**RetailInfo**](/uwp/api/Windows.System.Profile.RetailInfo) utility クラスの[**Isdemomodeenabled**](/uwp/api/windows.system.profile.retailinfo.isdemomodeenabled)プロパティ。これは、Windows.System の一部です[。](/uwp/api/windows.system.profile)Windows 10 SDK のプロファイル名前空間は、アプリケーションを実行するコードパスを_標準_モードまたは_リテール_モードで指定するブール値インジケーターとして使用されます。

``` csharp
using Windows.Storage;

StorageFolder folder = ApplicationData.Current.LocalFolder;

if (Windows.System.Profile.RetailInfo.IsDemoModeEnabled) 
{
    // Use the demo specific directory
    folder = await folder.GetFolderAsync("demo");
}

StorageFile file = await folder.GetFileAsync("hello.txt");
// Now read from file
```

``` cpp
using namespace Windows::Storage;

StorageFolder^ localFolder = ApplicationData::Current->LocalFolder;

if (Windows::System::Profile::RetailInfo::IsDemoModeEnabled) 
{
    // Use the demo specific directory
    create_task(localFolder->GetFolderAsync("demo").then([this](StorageFolder^ demoFolder)
    {
        return demoFolder->GetFileAsync("hello.txt");
    }).then([this](task<StorageFile^> fileTask)
    {
        StorageFile^ file = fileTask.get();
    });
    // Do something with file
}
else
{
    create_task(localFolder->GetFileAsync("hello.txt").then([this](StorageFile^ file)
    {
        // Do something with file
    });
}
```

``` javascript
if (Windows.System.Profile.retailInfo.isDemoModeEnabled) {
    console.log("Retail mode is enabled.");
} else {
    Console.log("Retail mode is not enabled.");
}
```

### <a name="retailinfoproperties"></a>RetailInfo

[**IsDemoModeEnabled**](/uwp/api/windows.system.profile.retailinfo.isdemomodeenabled) から true が返されたら、[**RetailInfo.Properties**](/uwp/api/windows.system.profile.retailinfo.properties) を使ってデバイスに関する一連のプロパティを照会することで、さらにカスタマイズした市販デモ エクスペリエンスを構築できます。 これらのプロパティには、[**ManufacturerName**](/uwp/api/windows.system.profile.knownretailinfoproperties.manufacturername)、[**Screensize**](/uwp/api/windows.system.profile.knownretailinfoproperties.screensize)、[**Memory**](/uwp/api/windows.system.profile.knownretailinfoproperties.memory) などが含まれます。

```csharp
using Windows.UI.Xaml.Controls;
using Windows.System.Profile

TextBlock priceText = new TextBlock();
priceText.Text = RetailInfo.Properties[KnownRetailInfo.Price];
// Assume infoPanel is a StackPanel declared in XAML
this.infoPanel.Children.Add(priceText);
```

```cpp
using namespace Windows::UI::Xaml::Controls;
using namespace Windows::System::Profile;

TextBlock ^manufacturerText = ref new TextBlock();
manufacturerText.set_Text(RetailInfo::Properties[KnownRetailInfoProperties::Price]);
// Assume infoPanel is a StackPanel declared in XAML
this->infoPanel->Children->Add(manufacturerText);
```

```javascript
var pro = Windows.System.Profile;
console.log(pro.retailInfo.properties[pro.KnownRetailInfoProperties.price);
```

#### <a name="idl"></a>IDL

```cpp
//  Copyright (c) Microsoft Corporation. All rights reserved.
//
//  WindowsRuntimeAPISet

import "oaidl.idl";
import "inspectable.idl";
import "Windows.Foundation.idl";
#include <sdkddkver.h>

namespace Windows.System.Profile
{
    runtimeclass RetailInfo;
    runtimeclass KnownRetailInfoProperties;

    [version(NTDDI_WINTHRESHOLD), uuid(0712C6B8-8B92-4F2A-8499-031F1798D6EF), exclusiveto(RetailInfo)]
    [version(NTDDI_WINTHRESHOLD, Platform.WindowsPhone)]
    interface IRetailInfoStatics : IInspectable
    {
        [propget] HRESULT IsDemoModeEnabled([out, retval] boolean *value);
        [propget] HRESULT Properties([out, retval, hasvariant] Windows.Foundation.Collections.IMapView<HSTRING, IInspectable *> **value);
    }

    [version(NTDDI_WINTHRESHOLD), uuid(50BA207B-33C4-4A5C-AD8A-CD39F0A9C2E9), exclusiveto(KnownRetailInfoProperties)]
    [version(NTDDI_WINTHRESHOLD, Platform.WindowsPhone)]
    interface IKnownRetailInfoPropertiesStatics : IInspectable
    {
        [propget] HRESULT RetailAccessCode([out, retval] HSTRING *value);
        [propget] HRESULT ManufacturerName([out, retval] HSTRING *value);
        [propget] HRESULT ModelName([out, retval] HSTRING *value);
        [propget] HRESULT DisplayModelName([out, retval] HSTRING *value);
        [propget] HRESULT Price([out, retval] HSTRING *value);
        [propget] HRESULT IsFeatured([out, retval] HSTRING *value);
        [propget] HRESULT FormFactor([out, retval] HSTRING *value);
        [propget] HRESULT ScreenSize([out, retval] HSTRING *value);
        [propget] HRESULT Weight([out, retval] HSTRING *value);
        [propget] HRESULT DisplayDescription([out, retval] HSTRING *value);
        [propget] HRESULT BatteryLifeDescription([out, retval] HSTRING *value);
        [propget] HRESULT ProcessorDescription([out, retval] HSTRING *value);
        [propget] HRESULT Memory([out, retval] HSTRING *value);
        [propget] HRESULT StorageDescription([out, retval] HSTRING *value);
        [propget] HRESULT GraphicsDescription([out, retval] HSTRING *value);
        [propget] HRESULT FrontCameraDescription([out, retval] HSTRING *value);
        [propget] HRESULT RearCameraDescription([out, retval] HSTRING *value);
        [propget] HRESULT HasNfc([out, retval] HSTRING *value);
        [propget] HRESULT HasSdSlot([out, retval] HSTRING *value);
        [propget] HRESULT HasOpticalDrive([out, retval] HSTRING *value);
        [propget] HRESULT IsOfficeInstalled([out, retval] HSTRING *value);
        [propget] HRESULT WindowsVersion([out, retval] HSTRING *value);
    }

    [version(NTDDI_WINTHRESHOLD), static(IRetailInfoStatics, NTDDI_WINTHRESHOLD)]
    [version(NTDDI_WINTHRESHOLD, Platform.WindowsPhone), static(IRetailInfoStatics, NTDDI_WINTHRESHOLD, Platform.WindowsPhone)]
    [threading(both)]
    [marshaling_behavior(agile)]
    runtimeclass RetailInfo
    {
    }

    [version(NTDDI_WINTHRESHOLD), static(IKnownRetailInfoPropertiesStatics, NTDDI_WINTHRESHOLD)]
    [version(NTDDI_WINTHRESHOLD, Platform.WindowsPhone), static(IKnownRetailInfoPropertiesStatics, NTDDI_WINTHRESHOLD, Platform.WindowsPhone)]
    [threading(both)]
    [marshaling_behavior(agile)]
    runtimeclass KnownRetailInfoProperties
    {
    }
}
```

## <a name="cleanup-process"></a>クリーンアッププロセス

クリーンアップは、買い物客がデバイスとの対話を停止してから2分後に開始します。 小売デモが再生され、Windows の連絡先、写真、その他のアプリのサンプルデータのリセットが開始されます。 デバイスによっては、すべてが正常にリセットされるまでに1-5 分かかることがあります。 これにより、小売店内のすべての顧客がデバイスを操作できるようになり、デバイスとの対話にも同じエクスペリエンスが得られます。

手順 1: クリーンアップ
* すべての Win32 アプリとストア アプリが終了します
* __ピクチャ__、__ビデオ__、__ミュージック__、__ドキュメント__、__保存済みの写真__、__カメラロール__、__デスクトップ__、__ダウンロード__フォルダーなどのすべての既知のフォルダーが削除されます
* 構造化されていないローミング状態と構造化されたローミング状態が削除されます
* 構造化されたローカル状態が削除されます

手順 2: セットアップ
* オフライン デバイスの場合: フォルダーは空のままです
* オンライン デバイスの場合: Microsoft Store から市販デモ アセットがデバイスにプッシュされます

### <a name="store-data-across-user-sessions"></a>ユーザーセッション間でのデータの格納

ユーザーセッション間でデータを格納するために、既定のクリーンアッププロセスでは、このフォルダー内のデータは自動的には削除されないため、情報を __Applicationdata. Current. 一時フォルダー__ に格納できます。 *Localstate*を使用して格納された情報はクリーンアッププロセス中に削除されることに注意してください。

### <a name="customize-the-cleanup-process"></a>クリーンアッププロセスをカスタマイズする

クリーンアッププロセスをカスタマイズするには、アプリ `Microsoft-RetailDemo-Cleanup` に app service を実装します。

カスタムクリーンアップロジックが必要なシナリオには、広範なセットアップの実行、データのダウンロードとキャッシュ、 *Localstate* データの削除を望んでいないなどがあります。

手順 1: アプリマニフェストで _RetailDemo-クリーンアップ_ サービスを宣言します。
``` CSharp
  <Applications>
      <Extensions>
        <uap:Extension Category="windows.appService" EntryPoint="MyCompany.MyApp.RDXCustomCleanupTask">
          <uap:AppService Name="Microsoft-RetailDemo-Cleanup" />
        </uap:Extension>
      </Extensions>
   </Application>
  </Applications>

```

手順 2: 次のサンプルテンプレートを使用して、 _AppdataCleanup_ case 関数の下にカスタムクリーンアップロジックを実装します。
``` CSharp
using System;
using System.IO;
using System.Runtime.Serialization.Json;
using System.Threading;
using System.Threading.Tasks;
using Windows.ApplicationModel.AppService;
using Windows.ApplicationModel.Background;
using Windows.Foundation.Collections;
using Windows.Storage;

namespace MyCompany.MyApp
{
    public sealed class RDXCustomCleanupTask : IBackgroundTask
    {
        BackgroundTaskCancellationReason _cancelReason = BackgroundTaskCancellationReason.Abort;
        BackgroundTaskDeferral _deferral = null;
        IBackgroundTaskInstance _taskInstance = null;
        AppServiceConnection _appServiceConnection = null;

        const string MessageCommand = "Command";

        public void Run(IBackgroundTaskInstance taskInstance)
        {
            // Get the deferral object from the task instance, and take a reference to the taskInstance;
            _deferral = taskInstance.GetDeferral();
            _taskInstance = taskInstance;
            _taskInstance.Canceled += new BackgroundTaskCanceledEventHandler(OnCanceled);

            AppServiceTriggerDetails appService = _taskInstance.TriggerDetails as AppServiceTriggerDetails;
            if ((appService != null) && (appService.Name == "Microsoft-RetailDemo-Cleanup"))
            {
                _appServiceConnection = appService.AppServiceConnection;
                _appServiceConnection.RequestReceived += _appServiceConnection_RequestReceived;
                _appServiceConnection.ServiceClosed += _appServiceConnection_ServiceClosed;
            }
            else
            {
                _deferral.Complete();
            }
        }

        void _appServiceConnection_ServiceClosed(AppServiceConnection sender, AppServiceClosedEventArgs args)
        {
        }

        async void _appServiceConnection_RequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
        {
            //Get a deferral because we will be calling async code
            AppServiceDeferral requestDeferral = args.GetDeferral();
            string command = null;
            var returnData = new ValueSet();

            try
            {
                ValueSet message = args.Request.Message;
                if (message.ContainsKey(MessageCommand))
                {
                    command = message[MessageCommand] as string;
                }

                if (command != null)
                {
                    switch (command)
                    {
                        case "AppdataCleanup":
                            {
                                // Do custom clean up logic here
                                break;
                            }
                    }
                }
            }
            catch (Exception e)
            {
            }
            finally
            {
                requestDeferral.Complete();
                // Also release the task deferral since we only process one request per instance.
                _deferral.Complete();
            }
        }

        private void OnCanceled(IBackgroundTaskInstance sender, BackgroundTaskCancellationReason reason)
        {
            _cancelReason = reason;
        }
    }
}
```

## <a name="related-links"></a>関連リンク

* [アプリ データの保存と取得](../design/app-settings/store-and-retrieve-app-data.md)
* [アプリ サービスの作成と利用の方法](../launch-resume/how-to-create-and-consume-an-app-service.md)
* [グローバリゼーションとローカライズ](../design/globalizing/globalizing-portal.md)
* [小売デモエクスペリエンス (RDX)](/windows-hardware/customize/desktop/retail-demo-experience)