---
description: アカウントユーザーを追加して管理するには、最初にパートナーセンターアカウントを組織の Azure Active Directory に関連付ける必要があります。
title: Azure Active Directory をパートナーセンターアカウントに関連付ける
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, azure ad, azure テナント, aad テナント, azure ad テナント, テナント管理, テナント
ms.localizationpriority: medium
ms.openlocfilehash: 84cdaac2b98e166640f96df46dd9785d1c50fbf8
ms.sourcegitcommit: 069f5ab4be85a7d638fc2a426afaed824e5dfeae
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/21/2021
ms.locfileid: "98668725"
---
# <a name="associate-azure-active-directory-with-your-partner-center-account"></a>Azure Active Directory をパートナーセンターアカウントに関連付ける

[アカウントユーザーを追加して管理](add-users-groups-and-azure-ad-applications.md)するには、最初にパートナーセンターアカウントを組織の Azure Active Directory に関連付ける必要があります。 

[パートナーセンター](https://partner.microsoft.com/dashboard) では、マルチユーザーアカウントのアクセスと管理に Azure AD を活用します。 組織で既に Microsoft の Microsoft 365 またはその他のビジネスサービスを使用している場合は、既に Azure AD があります。 それ以外の場合は、追加料金なしで、パートナーセンター内から新しい Azure AD テナントを作成することができます。

> [!TIP]
> このトピックは、[パートナーセンター](https://partner.microsoft.com/dashboard)の windows アプリ開発者プログラムに固有のものですが、テナントの関連付けとユーザーの管理は、Windows デスクトップアプリケーションプログラムのアカウント (詳細については「 [Windows デスクトップアプリケーションプログラム](/windows/desktop/appxpkg/windows-desktop-application-program#add-and-manage-account-users)」を参照してください) および Windows ハードウェア **開発者プログラム**(詳細について **は、「** [ダッシュボードの管理](/windows-hardware/drivers/dashboard/dashboard-administration)」を参照してください)。

1つの Azure AD テナントを複数のパートナーセンターアカウントに関連付けることができます。 複数のアカウントユーザーを追加するために必要なのは、パートナーセンターアカウントに関連付けられている Azure AD テナント1つだけですが、複数の Azure AD テナントを1つのパートナーセンターアカウントに追加するオプションもあります。 パートナー センター アカウントにおける **マネージャー** ロールのユーザーが、そのアカウントに対して Azure AD テナントを追加したり削除したりすることができます。

> [!IMPORTANT]
> パートナーセンターアカウントを Azure AD テナントに関連付けると、そのテナントのアカウントユーザーを追加して管理するために、 **マネージャー** ロールを持つ同じテナントのユーザーとしてパートナーセンターにサインインする必要があります。


## <a name="associate-your-partner-center-account-with-your-organizations-existing-azure-ad-tenant"></a>パートナーセンターアカウントを組織の既存の Azure AD テナントに関連付ける

組織で既に Azure AD を使用している場合は、次の手順に従ってパートナーセンターアカウントをリンクします。

1.  [パートナーセンター](https://partner.microsoft.com/dashboard)で、歯車アイコン (ダッシュボードの右上隅の近く) を選択し、[**アカウント設定**] を選択します。 [ **設定** ] メニューの [ **テナント**] を選択します。
2.  [ **Azure AD をパートナーセンターアカウントに関連付ける**] を選択します。
3.  関連付けたいテナントの Azure AD 資格情報を入力します。
4.  Azure AD テナントの組織とドメイン名を確認します。 関連付けを完了するには、 **[Confirm]\(確定\)** を選択します。
5.  関連付けに成功した場合、パートナー センターの **[ユーザー]** セクションで、アカウントのユーザーを追加したり管理したりできるようになります。

> [!IMPORTANT]
> 新しいユーザーを作成したり、Azure AD に他の変更を加えたりするには、[グローバル管理者のアクセス許可](/azure/active-directory/users-groups-roles/directory-assign-admin-roles)を持つアカウントを使ってその Azure AD にサインインする必要があります。 ただし、テナントを関連付けるため、またはそのテナントに既に存在するユーザーをパートナーセンターアカウントに追加するために、グローバル管理者のアクセス許可は必要ありません。


## <a name="create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account"></a>パートナーセンターアカウントに関連付ける新しい Azure AD を作成する

パートナーセンターアカウントとリンクする新しい Azure AD を設定する必要がある場合は、次の手順を実行します。

1.  [パートナーセンター](https://partner.microsoft.com/dashboard)で、歯車アイコン (ダッシュボードの右上隅の近く) を選択し、[**アカウント設定**] を選択します。 [ **設定** ] メニューの [ **テナント**] を選択します。
2.  **[新しい Azure AD の作成]** を選びます。
3.  新しい Azure AD のディレクトリ情報を入力します。
    - **ドメイン名**: "onmicrosoft.com" と共に Azure AD ドメインに使用する一意の名前。 たとえば、「example」と入力した場合、Azure AD ドメインは "example.onmicrosoft.com" になります。
    - **連絡先の電子メール**: ご利用のアカウントに関して、Microsoft がパートナー様に連絡する際のメール アドレス。
    - **グローバル管理者ユーザー アカウント情報**: 新しいグローバル管理者アカウントに使用する名、姓、ユーザー名、パスワード。
4.  **[作成]** をクリックして、新しいドメインとアカウントの情報を確定します。
5.  新しい Azure AD 全体管理者のユーザー名とパスワードでサインインすると、[追加のアカウント ユーザーを追加および管理](add-users-groups-and-azure-ad-applications.md)を開始できます。


## <a name="manage-azure-ad-tenant-associations"></a>Azure AD テナントの関連付けの管理

Azure AD テナントをパートナーセンターアカウントに関連付けたら、新しいテナントを追加したり、[ **テナント** ] ページから既存のテナントを削除したりできます。


### <a name="add-multiple-azure-ad-tenants-to-your-partner-center-account"></a>パートナーセンターアカウントに複数の Azure AD テナントを追加する

パートナーセンターアカウントの **マネージャー** ロールを持つユーザーは、Azure AD テナントをそのアカウントに関連付けることができます。

新しいテナントを関連付けるには、**[別の Azure AD テナントの関連付け]** を選び、上記の手順に従います。 関連付ける Azure AD テナントの資格情報が求められる点に注意してください。


### <a name="remove-an-azure-ad-tenant-from-your-partner-center-account"></a>パートナーセンターアカウントから Azure AD テナントを削除する

パートナーセンターアカウントの **マネージャー** ロールを持つユーザーは、アカウントから Azure AD テナントを削除できます。

> [!IMPORTANT]
> テナントを削除すると、そのテナントからパートナー センター アカウントに追加されていたユーザーはだれも、そのアカウントにサインインできなくなります。 

テナントを削除するには、[ **テナント** ] ページ ([ **アカウントの設定**]) で名前を確認し、[ **削除**] を選択します。 テナントを削除してよいか確認するメッセージが表示されます。 これを行うと、そのテナント内のユーザーはだれもパートナー センター アカウントにサインインできなくなり、また、それらのユーザーに対して構成したアクセス許可も削除されます。

> [!TIP]
> パートナー センターへのサインインに使用しているテナント内のアカウントを使用して、そのテナントを削除することはできません。 テナントを削除するには、アカウントに関連付けられている別のテナントの **マネージャー** として、パートナー センターにサインインする必要があります。 アカウントに関連付けられているテナントが 1 つしかない場合、そのテナントを削除するには、そのアカウントを開いた Microsoft アカウントでサインインする必要があります。
