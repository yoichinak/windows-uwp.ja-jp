---
description: アダプティブ トースト通知と対話型トースト通知を使うと、より多くのコンテンツやオプションのインライン画像を含み、オプションのユーザー操作を備えた柔軟性のあるポップアップ通知を作成できます。
title: トーストのコンテンツ
ms.assetid: 1FCE66AF-34B4-436A-9FC9-D0CF4BDA5A01
label: Toast content
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp, トースト通知, 対話型トースト, アダプティブ トースト, トーストのコンテンツ, トースト ペイロード
ms.localizationpriority: medium
ms.openlocfilehash: 92a8f53b2951d7fb3ed5bb5c0afbdf1e7e2424cc
ms.sourcegitcommit: fc7fb82121a00e552eaebafba42e5f8e1623c58a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/07/2021
ms.locfileid: "97978587"
---
# <a name="toast-content"></a>トーストのコンテンツ

アダプティブ トースト通知と対話型トースト通知を使うと、テキスト、画像、ボタンと入力を含む柔軟性のある通知を作成できます。

> **重要な API**: [UWP Community Toolkit Notifications NuGet パッケージ](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)

> [!NOTE]
> Windows 8.1 や Windows Phone 8.1 の従来のテンプレートについては、「[トースト テンプレート カタログ (Windows ランタイム アプリ)](/previous-versions/windows/apps/hh761494(v=win.10))」をご覧ください。


## <a name="getting-started"></a>作業の開始

**Notifications ライブラリをインストールします。** XML の代わりに C# を使って通知を生成する場合は、[Microsoft.Toolkit.Uwp.Notifications](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) という名前の NuGet パッケージをインストールします (「notifications uwp」を検索してください)。 この記事で示している C# のサンプルでは、NuGet パッケージの Version 1.0.0 を使っています。

**Notifications Visualizer をインストールします。** この無料の Windows アプリは、Visual Studio の [XAML エディター]/[デザイン] ビューと同様に、編集するときにトーストの瞬時プレビューを提供することで、対話型トースト通知をデザインするのに役立ちます。 詳しくは、「[Notifications Visualizer](notifications-visualizer.md)」をご覧になるか、[Notifications Visualizer を Microsoft Store からダウンロード](https://www.microsoft.com/store/apps/notifications-visualizer/9nblggh5xsl1)してください。


## <a name="sending-a-toast-notification"></a>トースト通知の送信

通知を送信する方法については、「[ローカル トースト通知の送信](send-local-toast.md)」をご覧ください。 このドキュメントでは、トースト コンテンツの作成についてのみ説明します。


## <a name="toast-notification-structure"></a>トースト通知の構造体

トースト通知は、Tag や Group (通知を識別できます) などのいくつかのデータ プロパティと *トースト コンテンツ* を組み合わせたものです。

トースト通知のコンテンツのコア コンポーネントは次のとおりです。
* **launch**: ユーザーがトーストをクリックしたときにアプリに渡される引数を定義します。これにより、トーストに表示されていた正しいコンテンツにディープ リンクできます。 詳しくは、「[ローカル トースト通知の送信](send-local-toast.md)」をご覧ください。
* **visual**: トーストの視覚的な部分 (テキストや画像が含まれている汎用バインディングなど) です。
* **actions**: トーストの対話的な部分 (入力やアクションなど) です。
* **audio**: トーストがユーザーに表示されるときに再生されるオーディオを制御します。

トースト コンテンツは生の XML で定義されますが、トースト コンテンツを作成するために、[NuGet ライブラリ](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)を使って C# (または C++) オブジェクト モデルを取得することができます。 この記事では、トースト コンテンツ内に含まれるすべてのものについて説明します。

#### <a name="builder-syntax"></a>[ビルダーの構文](#tab/builder-syntax)

```csharp
new ToastContentBuilder()
    .AddToastActivationInfo("app-defined-string", ToastActivationType.Foreground)
    .AddText("Some text")
    .AddButton("Archive", ToastActivationType.Background, "archive")
    .AddAudio(new Uri("ms-appx:///Sound.mp3"));
```

#### <a name="xml"></a>[XML](#tab/xml)

```xml
<toast launch="app-defined-string">

  <visual>
    <binding template="ToastGeneric">
      ...
    </binding>
  </visual>

  <actions>
    ...
  </actions>

  <audio src="ms-winsoundevent:Notification.Reminder"/>

</toast>
```

---

トースト コンテンツの視覚的な表示は、次のようになります。

![トースト通知の構造体](images/adaptivetoasts-structure.jpg)


## <a name="visual"></a>ビジュアル

各トーストでは visual を指定し、その中に汎用トースト バインディングを設定する必要があります。この汎用トースト バインディングには、テキストや画像などを含めることができます。 これらの要素は、デスクトップ、電話、タブレット、Xbox など、さまざまな Windows デバイスでレンダリングされます。

visual セクションとその子要素でサポートされているすべての属性については、[スキーマのドキュメント](toast-schema.md#toastvisual)をご覧ください。

トースト通知ではアプリの識別情報は、アプリ アイコンによって表示されます。 ただし、アプリ ロゴの上書きを使った場合は、テキスト行の下にアプリ名が表示されます。

| 標準のトーストでのアプリの識別情報 | appLogoOverride 使用時のアプリの識別情報 |
| -- | -- |
| <img src="images/adaptivetoasts-withoutapplogooverride.jpg" alt="notification without appLogoOverride" width="364"/> | <img alt="notification with appLogoOverride" src="images/adaptivetoasts-withapplogooverride.jpg" width="364"/> |


## <a name="text-elements"></a>テキスト要素

各トーストには少なくとも1つの text 要素が必要です。また、 [**AdaptiveText**](toast-schema.md#adaptivetext)型のテキスト要素を2つ含めることができます。

<img alt="Toast with title and description" src="images/toast-title-and-description.jpg" width="364"/>

Windows 10 Anniversary Update 以降は、そのテキストに対して **HintMaxLines** プロパティを使うことで、表示されるテキストの行数を制御できます。 既定値 (最大値) は、タイトルのテキストが最大 2 行であり、それに加えて、2 つの説明要素 (2 番目と 3 番目の **AdaptiveText**) を合計で最大 4 行まで含めることができます。

#### <a name="builder-syntax"></a>[ビルダーの構文](#tab/builder-syntax)

```csharp
new ToastContentBuilder()
    .AddText("Adaptive Tiles Meeting", hintMaxLines: 1)
    .AddText("Conf Room 2001 / Building 135")
    .AddText("10:00 AM - 10:30 AM");
```

#### <a name="xml"></a>[XML](#tab/xml)

```xml
<binding template="ToastGeneric">
    <text hint-maxLines="1">Adaptive Tiles Meeting</text>
    <text>Conf Room 2001 / Building 135</text>
    <text>10:00 AM - 10:30 AM</text>
</binding>
```

---


## <a name="app-logo-override"></a>アプリ ロゴの上書き

既定では、トーストにはアプリ ロゴが表示されます。 ただし、このロゴは独自の [**ToastGenericAppLogo**](toast-schema.md#toastgenericapplogo) 画像で上書きできます。 たとえば、これがある人からの通知である場合、その人の写真でアプリ ロゴを上書きすることをお勧めします。

<img alt="Toast with app logo override" src="images/toast-applogooverride.jpg" width="364"/>

**HintCrop** プロパティを使って、画像のトリミングを変更できます。 たとえば、 **円** は円でトリミングされたイメージになります。 その他の場合、画像は正方形です。 画像サイズは 100% のスケーリングで 48x48 ピクセルです。 一般に、各スケールファクターの各アイコン資産には、100%、125%、150%、200%、および400% のバージョンを指定することをお勧めします。 

#### <a name="builder-syntax"></a>[ビルダーの構文](#tab/builder-syntax)

```csharp
new ToastContentBuilder()
    ...
    
    .AddAppLogoOverride(new Uri("https://picsum.photos/48?image=883"), NotificationAppLogoCrop.Circle);
```

#### <a name="xml"></a>[XML](#tab/xml)

```xml
<binding template="ToastGeneric">
    ...
    <image placement="appLogoOverride" hint-crop="circle" src="https://picsum.photos/48?image=883"/>
</binding>
```

---



## <a name="hero-image"></a>ヒーロー イメージ

**Anniversary Update の新機能**: トーストには、ヒーロー イメージを表示できます。ヒーロー イメージとは、トースト バナー内や、アクション センター内にいるときに、目立つように表示されるメイン ビジュアルの [**ToastGenericHeroImage**](toast-schema.md#toastgenericheroimage) です。 画像サイズは 100% のスケーリングで 364x180 ピクセルです。

<img alt="Toast with hero image" src="images/toast-heroimage.jpg" width="364"/>

#### <a name="builder-syntax"></a>[ビルダーの構文](#tab/builder-syntax)

```csharp
new ToastContentBuilder()
    ...
    
    .AddHeroImage(new Uri("https://picsum.photos/364/180?image=1043"));
```

#### <a name="xml"></a>[XML](#tab/xml)

```xml
<binding template="ToastGeneric">
    ...
    <image placement="hero" src="https://picsum.photos/364/180?image=1043"/>
</binding>
```

---


## <a name="inline-image"></a>インライン画像

トーストを展開したときに表示される全幅のインライン画像を指定できます。

<img alt="Toast with additional image" src="images/toast-additionalimage.jpg" width="364"/>

#### <a name="builder-syntax"></a>[ビルダーの構文](#tab/builder-syntax)

```csharp
new ToastContentBuilder()
    ...
    
    .AddInlineImage(new Uri("https://picsum.photos/360/202?image=1043"));
```

#### <a name="xml"></a>[XML](#tab/xml)

```xml
<binding template="ToastGeneric">
    ...
    <image src="https://picsum.photos/360/202?image=1043" />
</binding>
```

---


## <a name="image-size-restrictions"></a>画像サイズの制限

トースト通知で使用する画像は、以下の場所から取得できます。

 - http://
 - ms-appx:///
 - ms-appdata:///

http および https のリモート Web 画像では、各画像のファイル サイズに制限があります。 Fall Creators Update (16299) では、この制限が通常の接続で 3 MB、従量制課金接続で 1 MB に拡大しました。 それより前のバージョンでは、いずれの場合も画像サイズの上限は 200 KB です。

| 通常の接続 | 従量制課金接続 | Fall Creators Update より前のバージョン |
| - | - | - |
| 3 MB | 1 MB | 200 KB |

画像がこのファイル サイズを超えている場合、画像がダウンロードできない場合、またはタイム アウトした場合は、画像が表示されず、通知の残りの部分のみが表示されます。


## <a name="attribution-text"></a>属性テキスト

**Anniversary Update の新機能**: コンテンツのソースを参照する必要がある場合、属性テキストを使うことができます。 このテキストは常に、アプリの ID または通知のタイムスタンプと共に通知の下部に表示されます。

属性テキストをサポートしていない以前のバージョンの Windows では、テキストは単に別のテキスト要素として表示されます (3 つのテキスト要素を最大限に含めていない場合)。

<img alt="Toast with attribution text" src="images/toast-attributiontext.jpg" width="364"/>

#### <a name="builder-syntax"></a>[ビルダーの構文](#tab/builder-syntax)

```csharp
new ToastContentBuilder()
    ...
    
    .AddAttributionText("Via SMS");
```

#### <a name="xml"></a>[XML](#tab/xml)

```xml
<binding template="ToastGeneric">
    ...
    <text placement="attribution">Via SMS</text>
</binding>
```

---


## <a name="custom-timestamp"></a>カスタム タイムスタンプ

**Creators Update の新機能**: システムが提供するタイムスタンプを、メッセージ、情報、コンテンツが生成された時点を正確に表す独自のタイムスタンプで上書きできるようになりました。 このタイムスタンプはアクション センターに表示されます。

<img alt="Toast with custom timestamp" src="images/toast-customtimestamp.jpg" width="396"/>

カスタム タイムスタンプの使用について詳しくは、「[トーストに表示されるカスタム タイムスタンプ](custom-timestamps-on-toasts.md)」をご覧ください。

#### <a name="builder-syntax"></a>[ビルダーの構文](#tab/builder-syntax)

```csharp
new ToastContentBuilder()
    ...
    
    .AddCustomTimeStamp(new DateTime(2017, 04, 15, 19, 45, 00, DateTimeKind.Utc));
```

#### <a name="xml"></a>[XML](#tab/xml)

```xml
<toast displayTimestamp="2017-04-15T19:45:00Z">
  ...
</toast>
```

---


## <a name="progress-bar"></a>進行状況バー

**作成者更新プログラムの新** 機能: トースト通知に進行状況バーを表示して、ダウンロードなどの操作の進行状況をユーザーに通知することができます。

<img alt="Toast with progress bar" src="images/toast-progressbar.png" width="364"/>

進行状況バーの使用について詳しくは、「[トーストの進行状況バー](toast-progress-bar.md)」をご覧ください。


## <a name="headers"></a>ヘッダー

**Creators Update の新機能**: アクション センターのヘッダーの下で、複数の通知をグループ化することができます。 たとえば、グループ チャットのグループ メッセージをヘッダーの下でグループ化したり、共通のテーマのグループ通知をヘッダーの下でグループ化したりすることができます。

<img alt="Toasts with header" src="images/toast-headers-action-center.png" width="396"/>

ヘッダーの使用について詳しくは、「[トーストのヘッダー](toast-headers.md)」をご覧ください。


## <a name="adaptive-content"></a>アダプティブ コンテンツ

**Anniversary Update の新機能**: 上記で指定したコンテンツ以外に、トーストが展開されたときに表示される追加のアダプティブ コンテンツを表示することもできます。

この追加コンテンツは Adaptive を使って指定されます。詳しくは、[アダプティブ タイルのドキュメント](create-adaptive-tiles.md)をご覧ください。

すべてのアダプティブ コンテンツは [**AdaptiveGroup**](./toast-schema.md#adaptivegroup) 内に含める必要があることに注意してください。 それ以外の場合、Adaptive を使ってレンダリングされません。


### <a name="columns-and-text-elements"></a>列とテキスト要素

列といくつかの詳細なアダプティブ テキスト要素が使われている例を次に示します。 テキスト要素は **AdaptiveGroup** 内にあるため、すべてのリッチアダプティブスタイルプロパティをサポートしています。

<img alt="Toast with additional text" src="images/toast-additionaltext.jpg" width="364"/>

#### <a name="builder-syntax"></a>[ビルダーの構文](#tab/builder-syntax)

```csharp
new ToastContentBuilder()
    ...
    
    .AddVisualChild(new AdaptiveGroup()
    {
        Children =
        {
            new AdaptiveSubgroup()
            {
                Children =
                {
                    new AdaptiveText()
                    {
                        Text = "52 attendees",
                        HintStyle = AdaptiveTextStyle.Base
                    },
                    new AdaptiveText()
                    {
                        Text = "23 minute drive",
                        HintStyle = AdaptiveTextStyle.CaptionSubtle
                    }
                }
            },
            new AdaptiveSubgroup()
            {
                Children =
                {
                    new AdaptiveText()
                    {
                        Text = "1 Microsoft Way",
                        HintStyle = AdaptiveTextStyle.CaptionSubtle,
                        HintAlign = AdaptiveTextAlign.Right
                    },
                    new AdaptiveText()
                    {
                        Text = "Bellevue, WA 98008",
                        HintStyle = AdaptiveTextStyle.CaptionSubtle,
                        HintAlign = AdaptiveTextAlign.Right
                    }
                }
            }
        }
    });
```

#### <a name="xml"></a>[XML](#tab/xml)

```xml
<binding template="ToastGeneric">
    ...
    <group>
        <subgroup>
            <text hint-style="base">52 attendees</text>
            <text hint-style="captionSubtle">23 minute drive</text>
        </subgroup>
        <subgroup>
            <text hint-style="captionSubtle" hint-align="right">1 Microsoft Way</text>
            <text hint-style="captionSubtle" hint-align="right">Bellevue, WA 98008</text>
        </subgroup>
    </group>
</binding>
```

---


## <a name="buttons"></a>ボタン

ボタンを使うとトーストを対話的にすることができ、ユーザーはトースト通知に対してクイック アクションを実行できます。その際、現在のワークフローは中断されません。 たとえば、ユーザーはトースト内からメッセージに直接返信したり、メール アプリを開かずにメールを削除したりすることができます。 ボタンは、通知の展開した部分に表示されます。

ボタンのエンドツーエンドの実装について詳しくは、「[ローカル トースト通知の送信](send-local-toast.md)」をご覧ください。

Buttons は次のようなさまざまな操作を実行できます。

-   特定のページやコンテキストへの移動に使うことができる引数を指定して、アプリをフォアグラウンドでアクティブ化します。
-   クイック返信や同様のシナリオで、アプリのバックグラウンド タスクをアクティブ化します。
-   プロトコル起動を利用して別のアプリをアクティブ化します。
-   システムアクション (事項など) を実行するか、通知を無視します。

> [!NOTE]
> 設定できるボタン (後述のコンキスト メニュー項目を含む) の数は最大 5 つです。

<img alt="notification with actions, example 1" src="images/adaptivetoasts-xmlsample02.jpg" width="364"/>

#### <a name="builder-syntax"></a>[ビルダーの構文](#tab/builder-syntax)

```csharp
new ToastContentBuilder()
    ...
    
    .AddButton("See more details", ToastActivationType.Foreground, "action=viewdetails&contentId=351")
    .AddButton("Remind me later", ToastActivationType.Background, "action=remindlater&contentId=351");
```

#### <a name="xml"></a>[XML](#tab/xml)

```xml
<toast launch="app-defined-string">

    ...

    <actions>

        <action
            content="See more details"
            arguments="action=viewdetails&amp;contentId=351"
            activationType="foreground"/>

        <action
            content="Remind me later"
            arguments="action=remindlater&amp;contentId=351"
            activationType="background"/>

    </actions>

</toast>
```

---


### <a name="buttons-with-icons"></a>アイコンの付いたボタン

ボタンにはアイコンを追加することができます。 これらのアイコンは、100% のスケーリングで 16 x 16 ピクセルの白い透明な画像であり、画像自体にパディングを含めることはできません。 トースト通知にアイコンを表示する場合、ボタンのスタイルがアイコン ボタンに変更されるため、通知内のすべてのボタンに対してアイコンを表示する必要があります。

> [!NOTE]
> アクセシビリティを確保するため、ユーザーが "ハイ コントラスト 白" モードをオンにしてもアイコンが見えるように、必ずコントラスト (白) バージョンのアイコン (白い背景に黒いアイコン) を指定します。 詳しくは、[トーストのアクセシビリティに関するページ](tile-toast-language-scale-contrast.md)をご覧ください。

<img src="images\adaptivetoasts-buttonswithicons.png" width="364" alt="Toast that has buttons with icons"/>

#### <a name="builder-syntax"></a>[ビルダーの構文](#tab/builder-syntax)

```csharp
new ToastContentBuilder()
    ...
    
    .AddButton(
        "Dismiss",
        ToastActivationType.Foreground,
        "dismiss", new Uri("Assets/NotificationButtonIcons/Dismiss.png", UriKind.Relative));
```

#### <a name="xml"></a>[XML](#tab/xml)

```xml
<action
    content="Dismiss"
    imageUri="Assets/NotificationButtonIcons/Dismiss.png"
    arguments="dismiss"
    activationType="background"/>
```

---


### <a name="buttons-with-pending-update-activation"></a>更新の保留アクティブ化機能を備えたボタン

**Fall Creators Update の新機能**: バックグラウンドのアクティブ化ボタンで、**PendingUpdate** のアクティブ化後の動作を使用して、トースト通知で複数ステップの対話を作成できます。 ユーザーがボタンをクリックすると、バックグラウンド タスクがアクティブ化し、トーストが "更新の保留中" 状態になります。トーストは、バックグラウンド タスクによって新しいトーストに置き換えられるまで、表示されたままになります。

この機能を実装する方法については、[トーストの更新の保留に関するページ](toast-pending-update.md)をご覧ください。

![更新の保留を使ったトースト](images/toast-pendingupdate.gif)


### <a name="context-menu-actions"></a>コンテキスト メニューのアクション

**Anniversary Update の新機能**: ユーザーがアクション センター内でトーストを右クリックしたときに表示される追加のコンテキスト メニュー アクションを既存のコンテキスト メニューに追加できます。 このメニューは、アクション センターで右クリックした場合にのみ表示されることに注意してください。 トーストのポップアップ バナーを右クリックしても表示されません。

> [!NOTE]
> 従来のデバイスでは、これらの追加のコンテキスト メニュー アクションが、単に通常のボタンとしてトースト上に表示されます。

追加した追加のコンテキストメニューアクション ([場所の変更] など) は、2つの既定のシステムエントリの上に表示されます。

<img alt="Toast with context menu" src="images/toast-contextmenu.png" width="444"/>

#### <a name="builder-syntax"></a>[ビルダーの構文](#tab/builder-syntax)

ビルダーの構文はコンテキストメニューのアクションをサポートしていないため、初期化子の構文を使用することをお勧めします。

```csharp
ToastContent content = new ToastContent()
{
    ...
 
    Actions = new ToastActionsCustom()
    {
        ContextMenuItems =
        {
            new ToastContextMenuItem("Change location", "action=changeLocation")
        }
    }
};
```

#### <a name="xml"></a>[XML](#tab/xml)

```xml
<toast>

    ...

    <actions>

        <action
            placement="contextMenu"
            content="Change location"
            arguments="action=changeLocation"/>

    </actions>

</toast>
```

---

> [!NOTE]
> 追加のコンテキスト メニュー項目は、トーストの合計ボタン数の上限である 5 つのボタンに含まれます。

追加のコンテキスト メニュー項目のアクティブ化は、トーストのボタンと同じ手順で処理されます。


## <a name="inputs"></a>入力

入力はトーストのトースト領域の Actions 領域内で指定します。つまり、これらはトーストが展開されたときにのみ表示されます。


### <a name="quick-reply-text-box"></a>クイック返信テキスト ボックス

クイック応答テキストボックスを有効にするには (たとえば、メッセージングアプリで)、テキスト入力とボタンを追加し、テキスト入力フィールドの ID を参照して、そのボタンが入力フィールドの横に表示されるようにします。 ボタンのアイコンは、埋め込みなし、白ピクセルが透明、および100% のスケールに設定されている32x32 ピクセルのイメージである必要があります。

<img alt="notification with text input and actions" src="images/adaptivetoasts-xmlsample05.jpg" width="364"/>

#### <a name="builder-syntax"></a>[ビルダーの構文](#tab/builder-syntax)

```csharp
new ToastContentBuilder()
    ...
    
    .AddInputTextBox("tbReply", "Type a reply")

    .AddButton(
        textBoxId: "tbReply", // To place button next to text box, reference text box's id
        content: "Reply",
        activationType: ToastActivationType.Background,
        arguments: "action=reply&convId=9318",
        imageUri: new Uri("Assets/Reply.png", UriKind.Relative));
```

#### <a name="xml"></a>[XML](#tab/xml)

```xml
<toast launch="app-defined-string">

    ...

    <actions>

        <input id="textBox" type="text" placeHolderContent="Type a reply"/>

        <action
            content="Send"
            arguments="action=reply&amp;convId=9318"
            activationType="background"
            hint-inputId="textBox"
            imageUri="Assets/Reply.png"/>

    </actions>

</toast>
```

---



### <a name="inputs-with-buttons-bar"></a>入力とボタン バー

1 つ (または複数) の入力と、その入力の下に通常のボタンが表示されるように設定することもできます。

<img alt="notification with text and input actions" src="images/adaptivetoasts-xmlsample04.jpg" width="364"/>

#### <a name="builder-syntax"></a>[ビルダーの構文](#tab/builder-syntax)

```csharp
new ToastContentBuilder()
    ...
    
    .AddInputTextBox("tbReply", "Type a reply")

    .AddButton("Reply", ToastActivationType.Background, "action=reply&threadId=9218")
    .AddButton("Video call", ToastActivationType.Foreground, "action=videocall&threadId=9218");
```

#### <a name="xml"></a>[XML](#tab/xml)

```xml
<toast launch="app-defined-string">

    ...

    <actions>

        <input id="textBox" type="text" placeHolderContent="Type a reply"/>

        <action
            content="Reply"
            arguments="action=reply&amp;threadId=9218"
            activationType="background"/>

        <action
            content="Video call"
            arguments="action=videocall&amp;threadId=9218"
            activationType="foreground"/>

    </actions>

</toast>
```

---


### <a name="selection-input"></a>選択入力

テキスト ボックスに加えて、選択メニューを使うこともできます。

<img alt="notification with selection input and actions" src="images/adaptivetoasts-xmlsample06.jpg" width="364"/>

#### <a name="builder-syntax"></a>[ビルダーの構文](#tab/builder-syntax)

```csharp
new ToastContentBuilder()
    ...
    
    .AddToastInput(new ToastSelectionBox("time")
    {
        DefaultSelectionBoxItemId = "lunch",
        Items =
        {
            new ToastSelectionBoxItem("breakfast", "Breakfast"),
            new ToastSelectionBoxItem("lunch", "Lunch"),
            new ToastSelectionBoxItem("dinner", "Dinner")
        }
    })

    .AddButton(...)
    .AddButton(...);
```

#### <a name="xml"></a>[XML](#tab/xml)

```xml
<toast launch="app-defined-string">

    ...

    <actions>

        <input id="time" type="selection" defaultInput="lunch">
            <selection id="breakfast" content="Breakfast" />
            <selection id="lunch" content="Lunch" />
            <selection id="dinner" content="Dinner" />
        </input>

        ...

    </actions>

</toast>
```

---



### <a name="snoozedismiss"></a>[一時停止する] と [無視]

選択メニューと 2 つのボタンを使って、システムの再通知操作と無視操作を利用するリマインダー通知を作成できます。 通知をリマインダーのように動作するように、必ずシナリオに Reminder を設定します。

<img alt="reminder notification" src="images/adaptivetoasts-xmlsample07.jpg" width="364"/>

ここでは、トースト ボタンで **SelectionBoxId** プロパティを使って、[一時停止] ボタンを選択メニューの入力に関連付けています。

#### <a name="builder-syntax"></a>[ビルダーの構文](#tab/builder-syntax)

```csharp
new ToastContentBuilder()
    .SetToastScenario(ToastScenario.Reminder)
    
    ...
    
    .AddToastInput(new ToastSelectionBox("snoozeTime")
    {
        DefaultSelectionBoxItemId = "15",
        Items =
        {
            new ToastSelectionBoxItem("5", "5 minutes"),
            new ToastSelectionBoxItem("15", "15 minutes"),
            new ToastSelectionBoxItem("60", "1 hour"),
            new ToastSelectionBoxItem("240", "4 hours"),
            new ToastSelectionBoxItem("1440", "1 day")
        }
    })

    .AddButton(new ToastButtonSnooze() { SelectionBoxId = "snoozeTime" })
    .AddButton(new ToastButtonDismiss());
```

#### <a name="xml"></a>[XML](#tab/xml)

```xml
<toast scenario="reminder" launch="action=viewEvent&amp;eventId=1983">
   
  ...
 
  <actions>
     
    <input id="snoozeTime" type="selection" defaultInput="15">
      <selection id="1" content="1 minute"/>
      <selection id="15" content="15 minutes"/>
      <selection id="60" content="1 hour"/>
      <selection id="240" content="4 hours"/>
      <selection id="1440" content="1 day"/>
    </input>
 
    <action activationType="system" arguments="snooze" hint-inputId="snoozeTime" content="" />
 
    <action activationType="system" arguments="dismiss" content=""/>
     
  </actions>
   
</toast>
```

---

システムの再通知操作と無視操作を使うには、次の手順を実行します。

-   **Toastbuttonsnooze 通知** または **toastbuttonbuttonを** 指定します
-   必要に応じてカスタムのコンテンツ文字列を指定する
    -   文字列を指定しない場合、"Snooze" と "Dismiss" のローカライズされた文字列が自動的に使われます。
-   必要に応じて **SelectionBoxId** を指定する
    -   再通知の間隔をユーザーが選ぶのではなく、システム定義の時間間隔 (OS 全体で一貫しています) に応じて 1 回だけ再通知を行う場合は、&lt;input&gt; を指定しないでください。
    -   再通知の間隔に関する選択項目を指定する場合:
        -   再通知操作に **SelectionBoxId** を指定する
        -   input の id に、再通知操作の **SelectionBoxId** と同じ値を指定する
        -   **ToastSelectionBoxItem** の値を、再通知の間隔を分単位で表す nonNegativeInteger になるよう指定する



## <a name="audio"></a>オーディオ

カスタム オーディオは、Mobile では常にサポートされ、Desktop では Version 1511 (ビルド 10586) 以降でサポートされます。 カスタム オーディオは次のパスで参照できます。

-   ms-appx:///
-   ms-appdata:///

または、[ms-winsoundevent の一覧に関するページ](/uwp/schemas/tiles/toastschema/element-audio#attributes-and-elements)から選ぶこともできます。これらは、常に両方のプラットフォームでサポートされます。

#### <a name="builder-syntax"></a>[ビルダーの構文](#tab/builder-syntax)

```csharp
new ToastContentBuilder()
    ...
    
    .AddAudio(new Uri("ms-appx:///Assets/NewMessage.mp3"));
```

#### <a name="xml"></a>[XML](#tab/xml)

```xml
<toast launch="app-defined-string">

    ...

    <audio src="ms-appx:///Assets/NewMessage.mp3"/>

</toast>
```

---


トースト通知でのオーディオについては、[オーディオ スキーマに関するページ](/uwp/schemas/tiles/toastschema/element-audio)をご覧ください。 カスタム オーディオを使うトーストの送信方法については、[トーストでのカスタム オーディオの使用](custom-audio-on-toasts.md)をご覧ください。


## <a name="alarms-reminders-and-incoming-calls"></a>アラーム、リマインダー、着信呼び出し

アラーム、リマインダー、着信呼び出しの通知を作成するには、単にシナリオの値が割り当てられた標準のトースト通知を使います。 シナリオはいくつかの動作を調整して、一貫性のある統一されたユーザー エクスペリエンスをもたらします。

> [!IMPORTANT]
> リマインダーやアラームを使用する場合、トースト通知に少なくとも 1 つのボタンを含める必要があります。 そうでない場合、トーストは、標準のトーストとして扱われます。

* **Reminder**: 通知は、ユーザーが通知を閉じるか、操作を実行するまで画面上に表示されたままになります。 Windows Mobile では、トーストは既に展開された状態で表示されます。 リマインダー音が再生されます。
* **Alarm**: リマインダーの動作に加えて、アラームは既定のアラーム音を使ってオーディオをループします。
* **IncomingCall**: 着信呼び出し通知は、Windows Mobile デバイスでは全画面で表示されます。 その他の点では、着信音オーディオを使うことと、ボタンのスタイルが異なることを除き、アラームと同じ動作が実行されます。

#### <a name="builder-syntax"></a>[ビルダーの構文](#tab/builder-syntax)

```csharp
new ToastContentBuilder()
    .SetToastScenario(ToastScenario.Reminder)
    ...
```

#### <a name="xml"></a>[XML](#tab/xml)

```xml
<toast scenario="reminder" launch="app-defined-string">

    ...

</toast>
```

---


## <a name="localization-and-accessibility"></a>ローカライズとアクセシビリティ

タイルやトーストには、表示言語や、表示倍率、ハイ コントラストなど、実行時のコンテキストに合わせた文字列や画像を読み込むことができます。 詳しくは、「[言語、スケール、ハイ コントラストに合わせたタイルとトースト通知のサポート](tile-toast-language-scale-contrast.md)」をご覧ください。


## <a name="handling-activation"></a>アクティブ化の処理
トーストのアクティブ化を処理する方法 (ユーザーがトーストをクリックするか、トースト上のボタンをクリックする) については、「[ローカル トースト通知の送信](send-local-toast.md)」をご覧ください。
 
## <a name="related-topics"></a>関連トピック

* [ローカル トースト通知の送信](send-local-toast.md)
* [GitHub の通知ライブラリ (UWP コミュニティ ツールキットの一部)](https://github.com/windows-toolkit/WindowsCommunityToolkit/tree/master/Microsoft.Toolkit.Uwp.Notifications)
* [言語、スケール、ハイ コントラストに合わせたタイルとトースト通知のサポート](tile-toast-language-scale-contrast.md)
