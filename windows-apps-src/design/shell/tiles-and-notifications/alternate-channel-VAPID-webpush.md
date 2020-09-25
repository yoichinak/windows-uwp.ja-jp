---
title: UWP で VAPID を使用する代替プッシュ チャネル
description: Windows アプリから VAPID プロトコルで代替プッシュチャネルを使用する方法
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10、uwp、WinRT API、WNS
localizationpriority: medium
ms.openlocfilehash: 79ea88cb457e9a0d7ba33ef51a184e6f52ab19c4
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/24/2020
ms.locfileid: "91219275"
---
# <a name="alternate-push-channels-using-vapid-in-windows"></a>Windows で VAPID を使用する代替プッシュチャネル 
Windows アプリでは、VAPID 認証を使用してプッシュ通知を送信できるようになります。  

> [!NOTE]
> これらの Api は、他の web サイトをホストし、その代わりにチャネルを作成する web ブラウザーを対象としています。  Web アプリに web プッシュ通知を追加する場合は、サービスワーカーを作成して通知を送信するための W3C および WhatWG 標準に従うことをお勧めします。

## <a name="introduction"></a>概要
Web プッシュ標準の導入により、web サイトがアプリのように動作し、ユーザーが web サイトにいない場合でも通知を送信できるようになります。

VAPID 認証プロトコルが作成され、web サイトがベンダーに依存しない方法でプッシュサーバーで認証できるようになりました。 VAPID プロトコルを使用しているすべてのベンダーが、web サイトが実行されているブラウザーがわからなくてもプッシュ通知を送信できます。 これは、プラットフォームごとに異なるプッシュプロトコルを実装した場合よりも大幅に改善されています。 

Windows アプリは、VAPID を使用して、これらの利点も含めてプッシュ通知を送信できます。 これらのプロトコルを使用すると、新しいアプリの開発時間を短縮し、既存のアプリのクロスプラットフォームサポートを簡素化できます。 さらに、enterprise apps またはサイドロードアプリでは、Microsoft Store に登録しなくても通知を送信できるようになりました。 これにより、すべてのプラットフォームでユーザーと連携する新しい方法が開きます。  

## <a name="alternate-channels"></a>代替チャネル 
UWP では、これらの VAPID チャネルは代替チャネルと呼ばれ、web プッシュチャネルに同様の機能を提供します。 アプリのバックグラウンドタスクをトリガーして、メッセージの暗号化を有効にし、1つのアプリから複数のチャネルを許可することができます。 チャネルの種類の違いの詳細については、「 [適切なチャネルの選択](channel-types.md)」を参照してください。

アプリでプライマリチャネルを使用できない場合や、web サイトとアプリの間でコードを共有する場合は、代替チャネルを使用してプッシュ通知にアクセスすることをお勧めします。 チャネルの設定は、web プッシュ標準を使用しているユーザー、または以前に Windows プッシュ通知を使用していたユーザーにとって、簡単で使いやすいものです。

## <a name="code-example"></a>コードの例

Windows アプリの代替チャネルを設定する基本的なプロセスは、プライマリチャネルまたはセカンダリチャネルを設定することと似ています。 まず、 [WNS サーバー](windows-push-notification-services--wns--overview.md)でチャネルに登録します。 次に、バックグラウンドタスクとして実行するように登録します。 通知が送信され、バックグラウンドタスクがトリガーされた後、イベントを処理します。  

### <a name="get-a-channel"></a>チャネルを取得する 
代替チャネルを作成するには、アプリが2つの情報を提供する必要があります。サーバーの公開キーと、作成するチャネルの名前です。 サーバーキーの詳細については、web プッシュの仕様を参照してください。ただし、キーを生成するには、サーバーで標準の web プッシュライブラリを使用することをお勧めします。  

アプリでは複数の代替チャネルを作成できるため、チャネル ID は特に重要です。 各チャネルは、そのチャネルで送信されたすべての通知ペイロードに含まれる一意の ID によって識別される必要があります。  

```csharp
private async void AppCreateVAPIDChannelAsync(string appChannelId, IBuffer applicationServerKey) 
{ 
    // From the spec: applicationServer Key (p256dh):  
    //               An Elliptic curve Diffie–Hellman public key on the P-256 curve 
    //               (that is, the NIST secp256r1 elliptic curve).   
    //               The resulting key is an uncompressed point in ANSI X9.62 format             
    // ChannelId is an app provided value for it to identify the channel later.  
    // case of this app it is from the set { "Football", "News", "Baseball" } 
    PushNotificationChannel webChannel = await PushNotificationChannelManager.GetDefault().CreateRawPushNotificationChannelWithAlternateKeyForApplicationAsync(applicationServerKey, appChannelId); 
 
    //Save the channel  
    AppUpdateChannelMapping(appChannelId, webChannel); 
             
    //Tell our web service that we have a new channel to push notifications to 
    AppPassChannelToSite(webChannel.Uri); 
} 
```
アプリは、チャネルをサーバーに送信し、ローカルに保存します。 チャネル ID をローカルに保存すると、アプリはチャネルを区別し、チャネルを更新して、期限切れにならないようにすることができます。

他のすべての種類のプッシュ通知チャネルと同様に、web チャネルの有効期限が切れます。 アプリを認識せずにチャネルの期限が切れないようにするには、アプリが起動されるたびに新しいチャネルを作成します。    

### <a name="register-for-a-background-task"></a>バックグラウンドタスクに登録する 

アプリで代替チャネルを作成したら、フォアグラウンドまたはバックグラウンドで通知を受け取るように登録する必要があります。 次の例では、を登録して、1つのプロセスモデルを使用して通知をバックグラウンドで受信します。  

```csharp
var builder = new BackgroundTaskBuilder(); 
builder.Name = "Push Trigger"; 
builder.SetTrigger(new PushNotificationTrigger()); 
BackgroundTaskRegistration task = builder.Register(); 
```
### <a name="receive-the-notifications"></a>通知を受信する 

アプリが通知を受信するように登録されると、受信通知を処理できる必要があります。 1つのアプリで複数のチャネルを登録できるため、通知を処理する前にチャネル ID を必ず確認してください。  

```csharp
protected override void OnBackgroundActivated(BackgroundActivatedEventArgs args) 
{ 
    base.OnBackgroundActivated(args); 
    var raw = args.TaskInstance.TriggerDetails as RawNotification; 
    switch (raw.ChannelId) 
    { 
        case "Football": 
            AppPostFootballScore(raw.Content); 
            break; 
        case "News": 
            AppProcessNewsItem(raw.Content); 
            break; 
        case "Baseball": 
            AppProcessBaseball(raw.Content); 
            break; 
 
        default: 
            //We don't have the channelID registered, should only happen in the case of a 
            //caching issue on the application server 
            break; 
    }                           
} 
```

通知がプライマリチャネルから送信される場合、チャネル ID は設定されないことに注意してください。  

## <a name="note-on-encryption"></a>暗号化に関する注意 

アプリにとって役に立つ任意の暗号化スキームを使用できます。 場合によっては、サーバーと任意の Windows デバイス間で TLS 標準を利用するだけで十分です。 それ以外の場合は、web プッシュ暗号化スキームを使用するか、または別の設計を使用することをお勧めします。  

別の形式の暗号化を使用する場合、キーは raw を使用することになります。Headers プロパティ。 プッシュサーバーへの POST 要求に含まれていたすべての暗号化ヘッダーが含まれています。 そこから、アプリはキーを使用してメッセージを復号化できます。  

## <a name="related-topics"></a>関連トピック
- [通知チャンネルの種類](channel-types.md)
- [Windows プッシュ通知サービス (WNS)](windows-push-notification-services--wns--overview.md)
- [PushNotificationChannel クラス](/uwp/api/windows.networking.pushnotifications.pushnotificationchannel)
- [PushNotificationChannelManager クラス](/uwp/api/windows.networking.pushnotifications.pushnotificationchannelmanager)
