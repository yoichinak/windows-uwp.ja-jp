---
Description: The first step in creating a new app in Partner Center is reserving an app name. ここでは、アプリ名を予約する方法について説明し、優れたアプリ名を選ぶための推奨事項を紹介します。
title: 名前の予約によるアプリの作成
keywords: windows 10, uwp, 名前の予約, アプリ名, アプリの名前, 名前, 製品名, 名前付け, 予約名, タイトル, 名前, 題名
ms.assetid: 6DC58A9A-DF47-4652-8D13-0AC9289F5950
ms.date: 10/31/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 79831e1e14ddf8c525f1697074e6a435513f5ea1
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259023"
---
# <a name="create-your-app-by-reserving-a-name"></a>名前の予約によるアプリの作成

The first step in creating a new app in [Partner Center](https://partner.microsoft.com/dashboard) is reserving an app name. 各予約名 (アプリの*タイトル*と呼ばれることもあります) は、Microsoft Store 全体で一意でなければなりません。

アプリの構築をまだ開始していない場合でも、アプリの名前を予約することができます。 We recommend doing so as soon as possible, so that nobody else can use the name. その名前の予約を維持sるには、3 か月以内にアプリを申請する必要がある点に注意してください。

[アプリのパッケージをアップロード](upload-app-packages.md)するには、アプリに予約した名前と [**Package/Properties/DisplayName**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-displayname) の値が一致している必要があります。 アプリのパッケージを作成するために Microsoft Visual Studio を使う場合は、この属性が自動的に入力されます。

> [!IMPORTANT]
> You can reserve additional names for an app, and you may choose to use one of those in the published version of your app instead of the one you reserve when you first create your app in Partner Center. However, be aware that the first name you enter here will be used in some of your app's [identity details](view-app-identity-details.md), such as the **Package Family Name (PFN)** . These values may be visible to some users, and cannot be changed, so make sure that the name you reserve is appropriate for this use.


## <a name="create-your-app-by-reserving-a-new-name"></a>新しい名前の予約によるアプリの作成

Reserving a name is the first step in creating an app in Partner Center. 

1.  **[概要]** ページで、 **[新しいアプリの作成]** をクリックします。
2.  使う名前をテキスト ボックスに入力して、 **[使用可能か確認]** を選びます。 名前が利用可能な場合は、緑色のチェック マークが表示されます (入力した名前が他の開発者によって既に予約または使用されている場合は、名前が利用できないことを示すエラー メッセージが表示されます)。
3.  **[製品名の予約]** をクリックします。

これにより名前が予約され、準備ができたときに[申請](app-submissions.md)を開始することができます。 

> [!NOTE]
> 名前を予約するときに、その名前のアプリが Microsoft Store の登録情報にない場合でも、予約できないことがあります。 これは多くの場合、他の開発者がアプリのために既にその名前を予約していて、そのアプリをまだ申請していないことが原因です。 自分が商標などの法的権利を持っている名前を予約できない場合や、Microsoft Store 内でその名前を使っている別のアプリを見つけた場合は、[Microsoft にお知らせください](https://www.microsoft.com/info/cpyrtInfrg.html)。

名前を予約した後は、そのアプリを 3 か月以内に申請します。 3 か月以内にアプリを申請しない場合、名前の予約は期限切れになり、他の開発者がアプリにその名前を使うことができるようになります。 期限切れの名前でアプリを申請しようとすると、エラーが発生する可能性があります。


## <a name="choosing-your-apps-name"></a>アプリ名の選択

アプリ用に適切な名前を選ぶことは、重要な作業です。 ユーザーの興味を引き、アプリについてもっと知りたくなるような名前を選んでください。 優れたアプリ名を選ぶためのヒントを次にいくつか紹介します。

-   **Keep it short.** ほとんどの場合、アプリの名前を表示する場所は限られているので、できるだけ短い名前を使うことをお勧めします。 アプリには最大 256 文字の名前を付けることができますが、長い名前は常にユーザーに対して最後まで表示されません。
    > [!NOTE]
    > さまざまな場所に実際に表示できる文字の数は、割り当てられている長さと、アプリ名に使われている文字の種類によって異なります。 たとえば、Windows で使われている Segoe UI フォントの場合、10 個の "W" が入る幅に約 30 個の "I" が入ります。 Because of this variation, be sure to test your app and verify how its name appears on its tiles (if you choose to overlay the app name), in search results, and within the app itself. さらに、アプリを提供する各言語についても考慮します。 東アジアの文字はラテン文字よりも幅広である場合が多く、表示文字数も少なくなる点に注意してください。
-   **Be original.** 既にある別のアプリと間違えられることのないよう、区別しやすいアプリ名を選ぶようにします。
-   **Don't use names trademarked by others.** 予約した名前が商標などの法的権利を侵害していないことを確認してください。 その名前が他者によって商標登録されている場合は、侵害が通報され、その名前の使用を続行できなくなることがあります。 アプリの公開後にこの問題が発生した場合は、アプリがストアから削除されます。 また、もう一度認定用に[アプリを提出](app-submissions.md)する前に、アプリ名を変更し、アプリとコンテンツ全体にわたって名前の記載や表示をすべて変更する必要があります。
-   **Avoid adding differentiating info at the end of the name.** 複数のアプリを区別するための情報が名前の末尾に追加されていると、長い名前の場合は特に、その情報をユーザーが見落としかねません。その結果、すべてのアプリが同じ名前に見える可能性があります。 If this is unavoidable, use different logos and app images to make it easier to differentiate one app from another.
-   **Don't include emojis in your name.** 絵文字やその他のサポートされていない文字が含まれる名前を予約することはできません。


## <a name="manage-additional-app-names"></a>追加のアプリ名の管理

You can add and manage additional names on the **Manage app names** page in the **App management** section for each of your apps in Partner Center.

アプリを複数の言語で提供し、言語ごとに異なる名前を使う場合などに、同じアプリに複数の名前を予約しておきたいことがあります。 アプリの名前を完全に変更する場合は、追加の名前を予約する必要があります。

このページでは、予約したが使う必要がなくなった名前を削除することもできます。

詳しくは、「[アプリ名の管理](manage-app-names.md)」をご覧ください。

 

 




