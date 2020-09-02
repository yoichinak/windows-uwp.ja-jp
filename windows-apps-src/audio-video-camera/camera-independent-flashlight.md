---
ms.assetid: D20C8E01-4E78-4115-A2E8-07BB3E67DDDC
description: この記事では、デバイスのライトにアクセスして使う方法を説明します (存在する場合)。 ライト機能は、デバイスのカメラやカメラのフラッシュ機能とは別に管理されます。
title: カメラに依存しない懐中電灯
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 281cede94ee587cc86509a9f32ed34857a5ae620
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/02/2020
ms.locfileid: "89362925"
---
# <a name="camera-independent-flashlight"></a>カメラに依存しない懐中電灯



この記事では、デバイスのライトにアクセスして使う方法を説明します (存在する場合)。 ライト機能は、デバイスのカメラやカメラのフラッシュ機能とは別に管理されます。 この記事では、ライトへの参照の取得および設定の調整に加えて、使用されていないときにライトのリソースを正しく解放する方法と、ライトが別のアプリで使用されている場合に利用状況の変化を検出する方法も説明します。

## <a name="get-the-devices-default-lamp"></a>デバイスの既定のライトを取得する

デバイスの既定のライトを取得するには、[**Lamp.GetDefaultAsync**](/uwp/api/windows.devices.lights.lamp.getdefaultasync) を呼び出します。 ライト関連 API は、[**Windows.Devices.Lights**](/uwp/api/Windows.Devices.Lights) 名前空間にあります。 これらの API にアクセスするには、この名前空間の using ディレクティブをあらかじめ追加しておく必要があります。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Lamp/cs/MainPage.xaml.cs" id="SnippetLightsNamespace":::


:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Lamp/cs/MainPage.xaml.cs" id="SnippetDeclareLamp":::


:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Lamp/cs/MainPage.xaml.cs" id="SnippetGetDefaultLamp":::

返されたオブジェクトが **null** の場合、そのデバイスでは **Lamp** API がサポートされていません。 一部のデバイスでは、ライトが物理的には存在していても、**Lamp** API がサポートされていないことがあります。

## <a name="get-a-specific-lamp-using-the-lamp-selector-string"></a>ライト セレクター文字列を使って特定のランプを取得する

デバイスによっては、複数のライトが組み込まれている場合があります。 デバイスで利用可能なライトの一覧を取得するには、[**GetDeviceSelector**](/uwp/api/windows.devices.lights.lamp.getdeviceselector) を呼び出すことによってデバイスのセレクター文字列を取得します。 このセレクター文字列は、[**DeviceInformation.FindAllAsync**](/uwp/api/windows.devices.enumeration.deviceinformation.findallasync) に渡すことができます。 これは、さまざまな種類の多数のデバイスを列挙するために使うメソッドです。セレクター文字列はこのメソッドに対し、ライト デバイスのみを返すように伝えます。 **FindAllAsync** から返される [**DeviceInformationCollection**](/uwp/api/Windows.Devices.Enumeration.DeviceInformationCollection) オブジェクトは、デバイスで利用可能なライトを表す [**DeviceInformation**](/uwp/api/Windows.Devices.Enumeration.DeviceInformation) オブジェクトのコレクションになります。 一覧からいずれかのオブジェクトを選択し、[**Id**](/uwp/api/windows.devices.enumeration.deviceinformation.id) プロパティを [**Lamp.FromIdAsync**](/uwp/api/windows.devices.lights.lamp.fromidasync) に渡すと、要求されたライトへの参照を取得できます。 この例では、**System.Linq** 名前空間の **GetFirstOrDefault** 拡張メソッドを使って、[**EnclosureLocation.Panel**](/uwp/api/windows.devices.enumeration.enclosurelocation.panel) プロパティの値が **Back** である **DeviceInformation** オブジェクトを選択しています。これにより、デバイス エンクロージャの背面にあるライトが選択されます (存在する場合)。

[**DeviceInformation**](/uwp/api/Windows.Devices.Enumeration.DeviceInformation) API は [**Windows.Devices.Enumeration**](/uwp/api/Windows.Devices.Enumeration) 名前空間にあります。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Lamp/cs/MainPage.xaml.cs" id="SnippetEnumerationNamespace":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Lamp/cs/MainPage.xaml.cs" id="SnippetGetLampWithSelectionString":::

## <a name="adjust-lamp-settings"></a>ライトの設定を調整する

[**Lamp**](/uwp/api/Windows.Devices.Lights.Lamp) クラスのインスタンスを作成した後、[**IsEnabled**](/uwp/api/windows.devices.lights.lamp.isenabled) プロパティを **true** に設定することで、ライトをオンにします。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Lamp/cs/MainPage.xaml.cs" id="SnippetLampSettingsOn":::

ライトをオフにするには、[**IsEnabled**](/uwp/api/windows.devices.lights.lamp.isenabled) プロパティを **false** に設定します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Lamp/cs/MainPage.xaml.cs" id="SnippetLampSettingsOff":::

デバイスによっては、ライトで色の値がサポートされていることがあります。 ライトで色がサポートされているかどうかを確認するには、[**IsColorSettable**](/uwp/api/windows.devices.lights.lamp.iscolorsettable) プロパティをチェックします。 この値が **true** であれば、[**Color**](/uwp/api/windows.devices.lights.lamp.color) プロパティを使ってライトの色を設定できます。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Lamp/cs/MainPage.xaml.cs" id="SnippetLampSettingsColor":::

## <a name="register-to-be-notified-if-the-lamp-availability-changes"></a>ライトの利用状況が変化したら通知されるよう登録する

ライトへのアクセス権は、アクセスを要求した最新のアプリに付与されます。 このため、別のアプリが起動され、現在のアプリで使用中のライト リソースが要求された場合は、別のアプリからリソースが解放されるまで、現在のアプリではライトを制御できなくなります。 ライトの利用状況が変化したときに通知を受け取るには、[**Lamp.AvailabilityChanged**](/uwp/api/windows.devices.lights.lamp.availabilitychanged) イベントに対するハンドラーを登録します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Lamp/cs/MainPage.xaml.cs" id="SnippetAvailabilityChanged":::

このイベントのハンドラーでは、[**LampAvailabilityChanged.IsAvailable**](/uwp/api/windows.devices.lights.lampavailabilitychangedeventargs.isavailable) プロパティをチェックして、ライトを使用できるかどうかを確認します。 この例では、ライトの利用可能性に基づいて、ライトをオンまたはオフにするトグル スイッチが有効または無効になります。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Lamp/cs/MainPage.xaml.cs" id="SnippetAvailabilityChangedHandler":::

## <a name="properly-dispose-of-the-lamp-resource-when-not-in-use"></a>使用していないライト リソースを適切に破棄する

ライトの使用が終わったら、ライトを無効にして [**Lamp.Close**](/uwp/api/windows.devices.lights.lamp.close) を呼び出すことにより、他のアプリがライトにアクセスできるようリソースを解放する必要があります。 C# を使用している場合、このプロパティは **Dispose** メソッドにマップされています。 [**AvailabilityChanged**](/uwp/api/windows.devices.lights.lamp.availabilitychanged) に登録した場合は、ライト リソースを破棄するときにハンドラーの登録を解除する必要があります。 ライト リソースを破棄するコードの適切な場所は、アプリによって異なります。 ライト アクセスのスコープを単一のページに限定するには、リソースを [**OnNavigatingFrom**](/uwp/api/windows.ui.xaml.controls.page.onnavigatingfrom) イベントで解放します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Lamp/cs/MainPage.xaml.cs" id="SnippetDisposeLamp":::

## <a name="related-topics"></a>関連トピック
- [メディア再生](media-playback.md)

 
