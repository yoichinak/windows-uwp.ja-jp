---
ms.assetid: c43d4af3-9a1a-4eae-a137-1267c293c1b5
description: この記事では、モバイル デバイス上にのみある特殊カメラの UI 機能を活用する方法を示します。
title: モバイル デバイスのカメラ UI の機能
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 4e1076c0632299ff79a8ca2fc226865d0ff3ebef
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/02/2020
ms.locfileid: "89362895"
---
# <a name="camera-ui-features-for-mobile-devices"></a>モバイル デバイスのカメラ UI の機能

この記事では、モバイル デバイス上にのみある特殊カメラの UI 機能を活用する方法を示します。 

## <a name="add-the-mobile-extension-to-your-project"></a>モバイル拡張をプロジェクトに追加する 

これらの機能を使用するには、ユニバーサル アプリ プラットフォーム用 Microsoft Mobile Extension SDK への参照をプロジェクトに追加する必要があります。

**ハードウェア カメラ ボタンのサポート用にモバイル拡張 SDK への参照を追加するには**

1.  **ソリューション エクスプローラー**で、**[参照]** を右クリックし、**[参照の追加]** を選択します。

2.  **[Windows ユニバーサル]** ノードを展開し、**[拡張機能]** を選びます。

3.  **[Microsoft Mobile Extension SDK for Universal App Platform]**(ユニバーサル アプリ プラットフォーム用 Microsoft Mobile Extension SDK) チェック ボックスをオンにします。

## <a name="hide-the-status-bar"></a>ステータス バーを非表示

モバイル デバイスには、デバイスに関する状態情報をユーザーに通知する [**StatusBar**](/uwp/api/Windows.UI.ViewManagement.StatusBar) コントロールがあります。 このコントロールが表示される領域は、画面上でメディア キャプチャ UI の表示に干渉する可能性があります。 [**HideAsync**](/uwp/api/windows.ui.viewmanagement.statusbar.hideasync) を呼び出すことでステータス バーを非表示にできますが、呼び出しは、この API が利用可能かどうかを [**ApiInformation.IsTypePresent**](/uwp/api/windows.foundation.metadata.apiinformation.istypepresent) メソッドで確認する条件ブロック内で行う必要があります。 このメソッドは、ステータス バーをサポートするモバイル デバイスでのみ true を返します。 アプリの起動時や、またはカメラからプレビューを開始するときには、ステータス バーを非表示にする必要があります。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetHideStatusBar":::

アプリをシャットダウンするときや、ユーザーがアプリのメディア キャプチャ ページから移動するときは、コントロールを再び表示できます。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetShowStatusBar":::

## <a name="use-the-hardware-camera-button"></a>ハードウェア カメラ ボタンを使う

一部のモバイル デバイスには、専用のハードウェア カメラ ボタンが用意されていることがあります。この仕様は、ユーザーによっては画面上のコントロールより好まれます。 このハードウェア カメラ ボタンが押されたときに通知を受け取るには、[**HardwareButtons.CameraPressed**](/uwp/api/windows.phone.ui.input.hardwarebuttons.camerapressed) イベントのハンドラーを登録します。 この API を利用できるのはモバイル デバイスのみであるため、この場合も **IsTypePresent** を使用して、この API が現在のデバイスでサポートされているかどうかをアクセス前に確認する必要があります。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetPhoneUsing":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetRegisterCameraButtonHandler":::

**CameraPressed** イベントのハンドラーで、写真のキャプチャを開始できます。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetCameraPressed":::

アプリをシャットダウンするときや、ユーザーがアプリのメディア キャプチャ ページから移動するときは、ハードウェア ボタンのハンドラーを登録解除します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetUnregisterCameraButtonHandler":::

## <a name="related-topics"></a>関連トピック

* [カメラ](camera.md)
* [MediaCapture を使った基本的な写真、ビデオ、およびオーディオのキャプチャ](basic-photo-video-and-audio-capture-with-MediaCapture.md)
