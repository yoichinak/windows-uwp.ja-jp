---
description: パッケージ化された .NET およびネイティブ C++/Win32 アプリの特定の種類のアクティブ化情報を取得する方法について説明します。
title: パッケージ アプリのアクティブ化情報の取得
ms.date: 09/17/2020
ms.topic: article
keywords: windows 10, uwp
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: 5593b59a542f014e482c590b8d534804c836a1a7
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/24/2020
ms.locfileid: "91216725"
---
# <a name="get-activation-info-for-packaged-apps"></a>パッケージ アプリのアクティブ化情報の取得

Windows 10 Version 1809 以降、パッケージ化されたデスクトップ アプリで [AppInstance.GetActivatedEventArgs](/uwp/api/windows.applicationmodel.appinstance.getactivatedeventargs) メソッドを呼び出して、スタートアップ時にアプリの特定の種類のアクティブ化情報を取得できるようになりました。 たとえば、このメソッドを呼び出すことにより、ファイルを開く、対話型トーストをクリックする、プロトコルを使用するなどの処理から、アプリのアクティブ化に関連する情報を取得できます。 Windows 10 Version 2004 以降では、[スパース パッケージ](./grant-identity-to-nonpackaged-apps.md)が使用されているアプリでも、この機能がサポートされています。

> [!NOTE]
> この記事の説明に従って [AppInstance.GetActivatedEventArgs](/uwp/api/windows.applicationmodel.appinstance.getactivatedeventargs) メソッドを使用することにより、特定の種類のアクティブ化情報を取得できるだけでなく、COM クラスを定義することにより、バックグラウンド タスクのアクティブ化情報を取得することもできます。 詳細については、「[winmain COM バックグラウンド タスクの作成と登録](/windows/uwp/launch-resume/create-and-register-a-winmain-background-task)」を参照してください。

## <a name="code-example"></a>コードの例

次のコードの例では、Windows フォーム アプリで、**Main** 関数から [AppInstance.GetActivatedEventArgs](/uwp/api/windows.applicationmodel.appinstance.getactivatedeventargs) メソッドを呼び出す方法を示しています。 アプリによってサポートされているアクティブ化の種類ごとに、`args` の戻り値を、対応するイベント引数の型にキャストします。 このコードの例では、`Handlexxx` メソッドは、別の場所で定義されている専用のアクティブ化ハンドラー コードと見なされます。

```csharp
static void Main()
{
    Application.EnableVisualStyles();
    Application.SetCompatibleTextRenderingDefault(false);

    var args = AppInstance.GetActivatedEventArgs();
    switch (args.Kind)
    {
        case ActivationKind.Launch:
            HandleLaunch(args as LaunchActivatedEventArgs);
            break;
        case ActivationKind.ToastNotification:
            HandleToastNotification(args as ToastNotificationActivatedEventArgs);
            break;
        case ActivationKind.VoiceCommand:
            HandleVoiceCommand(args as VoiceCommandActivatedEventArgs);
            break;
        case ActivationKind.File:
            HandleFile(args as FileActivatedEventArgs);
            break;
        case ActivationKind.Protocol:
            HandleProtocol(args as ProtocolActivatedEventArgs);
            break;
        case ActivationKind.StartupTask:
            HandleStartupTask(args as StartupTaskActivatedEventArgs);
            break;
        default:
            HandleLaunch(null);
            break;
    }
```

## <a name="supported-activation-types"></a>サポートされるアクティブ化の種類

[AppInstance.GetActivatedEventArgs](/uwp/api/windows.applicationmodel.appinstance.getactivatedeventargs) メソッドを使用すると、サポートされているイベント引数オブジェクトのセット (次の表を参照) からアクティブ化情報を取得できます。 これらのアクティブ化の種類の一部では、パッケージ マニフェストでパッケージ拡張機能を使用する必要があります。

[ShareTargetActivatedEventArgs](/uwp/api/windows.applicationmodel.activation.sharetargetactivatedeventargs) アクティブ化情報は、Windows 10 Version 2004 以降でのみサポートされています。 他のすべてのアクティブ化情報の種類は、Windows 10 Version 1809 以降でサポートされています。

| イベント引数の型 | パッケージ拡張機能 | 関連ドキュメント | 
|-------------------|-----------------|-----------------------|
| [ShareTargetActivatedEventArgs](/uwp/api/windows.applicationmodel.activation.sharetargetactivatedeventargs) | [uap:ShareTarget](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-sharetarget) | [デスクトップ アプリケーションを共有ターゲットにする](./desktop-to-uwp-extend.md#making-your-desktop-application-a-share-target) |
| [ProtocolActivatedEventArgs](/uwp/api/windows.applicationmodel.activation.protocolactivatedeventargs) | [uap:Protocol](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-protocol) | [プロトコルを使用してアプリケーションを起動する](./desktop-to-uwp-extensions.md#start-your-application-by-using-a-protocol) |
| [ToastNotificationActivatedEventArgs](/uwp/api/windows.applicationmodel.activation.toastnotificationactivatedeventarg) | desktop:ToastNotificationActivation | [デスクトップ アプリからのトースト通知](/windows/uwp/design/shell/tiles-and-notifications/toast-desktop-apps) |
| [StartupTaskActivatedEventArgs](/uwp/api/windows.applicationmodel.activation.startuptaskactivatedeventargs)  | desktop:StartupTask | [ユーザーが Windows にログオンしたときに実行可能ファイルを起動する](./desktop-to-uwp-extensions.md#start-an-executable-file-when-users-log-into-windows) |
| [FileActivatedEventArgs](/uwp/api/windows.applicationmodel.activation.fileactivatedeventargs) | [uap:FileTypeAssociation](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation) | [パッケージ アプリケーションを一連のファイルの種類に関連付ける](./desktop-to-uwp-extensions.md#associate-your-packaged-application-with-a-set-of-file-types) |
| [VoiceCommandActivatedEventArgs](/uwp/api/windows.applicationmodel.activation.voicecommandactivatedeventargs) | なし | [アクティブ化の処理と音声コマンドの実行](/cortana/voice-commands/launch-a-foreground-app-with-voice-commands-in-cortana) |
| [LaunchActivatedEventArgs](/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs) | なし |  |