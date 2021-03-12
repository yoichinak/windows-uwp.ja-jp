---
description: 他の種類のパッケージではないアプリからローカルのトースト通知を送信し、ユーザーがトーストをクリックするのを処理する方法について説明します。
title: アプリケーションの他の種類のパッケージからローカルトースト通知を送信する
ms.assetid: E9AB7156-A29E-4ED7-B286-DA4A6E683638
label: Send a local toast notification from other types of unpackaged apps
template: detail.hbs
ms.date: 11/17/2020
ms.topic: article
keywords: windows 10、トースト通知の送信、通知、通知の送信、トースト通知、方法、クイックスタート、作業の開始、コードサンプル、チュートリアル、その他の種類のアプリ、アンパッケージ
ms.localizationpriority: medium
ms.openlocfilehash: 71c0facc0cd77383ac6682f4934334a7356eb0e8
ms.sourcegitcommit: 5e718720d1032a7089dea46a7c5aefa6cda3385f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/12/2021
ms.locfileid: "103231549"
---
# <a name="send-a-local-toast-notification-from-other-types-of-unpackaged-apps"></a>アプリケーションの他の種類のパッケージからローカルトースト通知を送信する

MSIX/UWP またはスパース署名されたパッケージを使用しないアプリを開発していて、C# または C++ ではない場合は、このページが表示されます。

トースト通知は、ユーザーが現在アプリ内にいないときに、アプリが作成してユーザーに配信できるメッセージです。 このクイックスタートでは、Windows 10 トースト通知を作成、配信、および表示する手順について説明します。 これらのクイックスタートでは、実装するための最も簡単な通知であるローカル通知を使用します。

> [!IMPORTANT]
> C# アプリを作成する場合は、 [C# のドキュメント](send-local-toast.md)を参照してください。 C++ アプリを作成している場合は、 [C++ UWP](send-local-toast-cpp-uwp.md) または [c++ wrl](send-local-toast-desktop-cpp-wrl.md) のドキュメントを参照してください。



## <a name="step-1-register-your-app-in-the-registry"></a>手順 1: レジストリにアプリを登録する

まず、アプリを識別する一意の AUMID、アプリの表示名、アイコン、および COM アクティベーターの GUID を含む、アプリの情報をレジストリに登録する必要があります。

```xml
<registryKey keyName="HKEY_LOCAL_MACHINE\Software\Classes\AppUserModelId\<YOUR_AUMID>">
    <registryValue
        name="DisplayName"
        value="My App"
        valueType="REG_EXPAND_SZ" />
    <registryValue
        name="IconUri"
        value="C:\icon.png"
        valueType="REG_EXPAND_SZ" />
    <registryValue
        name="IconBackgroundColor"
        value="AARRGGBB"
        valueType="REG_SZ" />
    <registryValue
        name="CustomActivator"
        value="{YOUR COM ACTIVATOR GUID HERE}"
        valueType="REG_SZ" />
</registryKey>
```

## <a name="step-2-set-up-your-com-activator"></a>手順 2: COM アクティベーターを設定する

通知は、アプリが実行されていない場合でも、いつでもクリックできます。 したがって、通知のアクティブ化は COM アクティベーターを通じて処理されます。 COM クラスは、インターフェイスを実装する必要があり `INotificationActivationCallback` ます。 COM クラスの GUID は、registry CustomActivator 値で指定した GUID と一致している必要があります。

```cpp
struct callback : winrt::implements<callback, INotificationActivationCallback>
{
    HRESULT __stdcall Activate(
        LPCWSTR app,
        LPCWSTR args,
        [[maybe_unused]] NOTIFICATION_USER_INPUT_DATA const* data,
        [[maybe_unused]] ULONG count) noexcept final
    {
        try
        {
            std::wcout << this_app_name << L" has been called back from a notification." << std::endl;
            std::wcout << L"Value of the 'app' parameter is '" << app << L"'." << std::endl;
            std::wcout << L"Value of the 'args' parameter is '" << args << L"'." << std::endl;
            return S_OK;
        }
        catch (...)
        {
            return winrt::to_hresult();
        }
    }
};
```



## <a name="step-3-send-a-toast"></a>手順 3: トーストを送信する

Windows 10 では、トースト通知のコンテンツは、通知の外観にすばらしい柔軟性を持たせることができるアダプティブ言語を使って表されます。 詳しくは、[トースト コンテンツのドキュメント](adaptive-interactive-toasts.md)をご覧ください。

単純なテキストベースの通知から始めます。 通知ライブラリを使用して通知コンテンツを作成し、通知を表示します。

> [!IMPORTANT]
> 通知を送信する場合は、前の手順で AUMID を使用して、通知がアプリから表示されるようにする必要があります。

<img alt="Simple text notification" src="images/send-toast-01.png" width="364"/>

```cpp
// Construct the toast template
XmlDocument doc;
doc.LoadXml(L"<toast>\
    <visual>\
        <binding template=\"ToastGeneric\">\
            <text></text>\
            <text></text>\
        </binding>\
    </visual>\
</toast>");

// Populate with text and values
doc.SelectSingleNode(L"//text[1]").InnerText(L"Andrew sent you a picture");
doc.SelectSingleNode(L"//text[2]").InnerText(L"Check this out, The Enchantments in Washington!");

// Construct the notification
ToastNotification notif{ doc };

// And send it! Use the AUMID you specified earlier.
ToastNotificationManager::CreateToastNotifier(L"MyPublisher.MyApp").Show(notif);
```


## <a name="step-4-handling-activation"></a>手順 4: アクティブ化の処理

通知がクリックされると、COM アクティベーターがアクティブになります。


## <a name="more-details"></a>詳細

### <a name="aumid-restrictions"></a>AUMID の制限

AUMID の長さは最大129文字にする必要があります。 AUMID の長さが129文字を超える場合、スケジュールされたトースト通知は機能しません。スケジュールされた通知を追加すると、次の例外が発生します。 *システム呼び出しに渡されたデータ領域が小さすぎます。(0x8007007A)*。