---
description: メールの作成ダイアログを起動して、ユーザーがメール メッセージを送信できるようにする方法について説明します。 ダイアログを表示する前に、メールの各フィールドにデータを設定することができます。 メッセージは、ユーザーが送信ボタンをタップするまで送信されません。
title: 電子メールの送信
ms.assetid: 74511E90-9438-430E-B2DE-24E196A111E5
keywords: 連絡先, メール, 送信
ms.date: 10/11/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 47d07fa1932aae87704f7922762a8f0b7e430444
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89154666"
---
# <a name="send-email"></a>電子メールの送信

メールの作成ダイアログを起動して、ユーザーがメール メッセージを送信できるようにする方法について説明します。 ダイアログを表示する前に、メールの各フィールドにデータを設定することができます。 メッセージは、ユーザーが送信ボタンをタップするまで送信されません。

**この記事の内容**

-   [メールの作成ダイアログの起動](#launch-the-compose-email-dialog)
-   [概要と次の手順](#summary-and-next-steps)
-   [関連トピック](#related-topics)

## <a name="launch-the-compose-email-dialog"></a>メールの作成ダイアログの起動

新しい [**EmailMessage**](/uwp/api/Windows.ApplicationModel.Email.EmailMessage) オブジェクトを作成し、メールの作成ダイアログに事前に入力するデータを設定します。 ダイアログを表示するには、[**ShowComposeNewEmailAsync**](/uwp/api/windows.applicationmodel.email.emailmanager.showcomposenewemailasync) を呼び出します。

``` cs
private async Task ComposeEmail(Windows.ApplicationModel.Contacts.Contact recipient,
    string subject, string messageBody)
{
    var emailMessage = new Windows.ApplicationModel.Email.EmailMessage();
    emailMessage.Body = messageBody;

    var email = recipient.Emails.FirstOrDefault<Windows.ApplicationModel.Contacts.ContactEmail>();
    if (email != null)
    {
        var emailRecipient = new Windows.ApplicationModel.Email.EmailRecipient(email.Address);
        emailMessage.To.Add(emailRecipient);
        emailMessage.Subject = subject;
    }

    await Windows.ApplicationModel.Email.EmailManager.ShowComposeNewEmailAsync(emailMessage);
}
```

>[!NOTE]
> [Emailattachment](/uwp/api/windows.applicationmodel.email.emailattachment)クラスを使用して電子メールに追加した添付ファイルは、メールアプリでのみ表示されます。 他のメールプログラムが既定のメールプログラムとして構成されている場合は、[作成] ウィンドウが添付ファイルなしで表示されます。 これは既知の問題です。

## <a name="summary-and-next-steps"></a>まとめと次のステップ

このトピックでは、メールの作成ダイアログの起動方法を示しました。 メール メッセージの受信者として使う連絡先を選ぶ方法については、「[連絡先の選択](selecting-contacts.md)」をご覧ください。 電子メールの添付ファイルとして使用するファイルの選択については、「[**PickSingleFileAsync**](/uwp/api/windows.storage.pickers.fileopenpicker.picksinglefileasync)」をご覧ください。

## <a name="related-topics"></a>関連トピック

* [連絡先の選択](selecting-contacts.md)
 