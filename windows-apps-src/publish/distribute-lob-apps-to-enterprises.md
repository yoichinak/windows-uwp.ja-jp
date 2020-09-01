---
Description: ビジネス向け Microsoft ストアまたは教育機関向け Microsoft ストアでは、基幹業務 (LOB) アプリを企業に直接公開して、ボリューム取得を可能にすることができます。アプリをストアで一般公開する必要はありません。
title: LOB アプリの企業への配布
ms.assetid: 2050126E-CE49-4DE3-AC2B-A572AC895158
ms.date: 01/16/2020
ms.topic: article
keywords: Windows 10, UWP, LOB, 基幹業務, エンタープライズ アプリ, ビジネス向け Store, 教育機関向け Store, 企業
ms.localizationpriority: medium
ms.openlocfilehash: 9fccf3cab82724f12789131b8450795201e0fba6
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89161946"
---
# <a name="distribute-lob-apps-to-enterprises"></a>LOB アプリの企業への配布

[Msix パッケージ](/windows/msix/)を使用して組織のユーザーに基幹業務 (LOB) アプリを配布するためのオプションがいくつかあります。アプリを一般公開されていなくても使用できます。 デバイス管理ツールを使用したり、アプリのインストーラーベースの展開を構成したり、アプリを直接サイドロードしたり、Microsoft Store の Microsoft Store にアプリを発行したりすることができます。

## <a name="microsoft-endpoint-configuration-manager-and-microsoft-intune"></a>Microsoft エンドポイント Configuration Manager および Microsoft Intune

組織でデバイスを管理するために Microsoft エンドポイント Configuration Manager または Microsoft Intune を使用している場合は、これらのツールを使用して LOB アプリを展開できます。 詳細については、次の記事を参照してください。

* [Configuration Manager でのアプリケーション管理の概要](/configmgr/apps/understand/introduction-to-application-management)
* [Microsoft Intune のアプリ ライフサイクルの概要](/intune/apps/app-lifecycle)

## <a name="app-installer"></a>アプリ インストーラー

アプリインストーラーを使用すると、MSIX アプリケーションパッケージを直接ダブルクリックするか、web サーバーからアプリケーションパッケージをインストールする appinstaller ファイルをダブルクリックすることで、Windows 10 アプリをインストールできます。 これは、ユーザーが LOB アプリをインストールするために PowerShell やその他の開発者ツールを使用する必要がないことを意味します。 アプリインストーラーでは、オプションのパッケージと関連するセットを含むアプリパッケージをインストールすることもできます。

アプリ インストーラーは、エンタープライズ内でオフラインで使用する目的で、ビジネス向け Microsoft Store の [Web ポータル](https://businessstore.microsoft.com/store/details/app-installer/9NBLGGH4NNS1)からダウンロードすることができます。 アプリインストーラーの詳細については、「 [アプリインストーラーを使用した Windows 10 アプリのインストール](/windows/msix/app-installer/app-installer-root)」を参照してください。

## <a name="sideloading"></a>サイドローディング

組織内のユーザーに LOB アプリを直接配布するためのもう1つのオプションはサイドローディングです。 このオプションは、ユーザーが MSIX アプリケーションパッケージを直接インストールできるという点で、アプリのインストールベースの展開に似ています。 Windows 10 バージョン2004以降では、サイドローディングが既定で有効になり、ユーザーは署名済みの MSIX アプリパッケージをダブルクリックしてアプリをインストールできるようになりました。 Windows 10 バージョン1909以前では、サイドローディングには追加の構成と PowerShell スクリプトの使用が必要です。 詳しくは、「[Windows 10 での LOB アプリのサイドローディング](/windows/application-management/sideload-apps-in-windows-10)」をご覧ください。

## <a name="microsoft-store-for-business-or-microsoft-store-for-education"></a>教育のためのビジネスまたは Microsoft Store の Microsoft Store

ビジネス向け Microsoft ストアまたは教育機関向け Microsoft ストアでは、基幹業務 (LOB) アプリを企業に直接公開して、ボリューム取得を可能にすることができます。アプリをストアで一般公開する必要はありません。 このオプションを使用する場合、アプリはストアによって署名され、標準のストアポリシーに準拠している必要があります。

> [!NOTE]
> 現在、ビジネス向け Microsoft ストアまたは教育機関向け Microsoft ストアを通じて企業に排他的に配布できるのは無料アプリだけです。 有料アプリを LOB として申請しても、企業が利用できるようにはなりません。 

> [!IMPORTANT]
> [Microsoft Store 申請 API](../monetize/create-and-manage-submissions-using-windows-store-services.md) を使って、LOB アプリを直接企業に公開することはできません。 LOB アプリのすべての申請は、パートナーセンターを通じて公開する必要があります。

### <a name="set-up-the-enterprise-association"></a>企業の関連付けの設定

LOB アプリを専用アプリとして企業に公開するには、最初に、お使いのアカウントと企業のプライベート ストアを関連付けます。

> [!IMPORTANT]
> この関連付けプロセスは、企業が開始する必要があります。また、開発者アカウントの作成に使った Microsoft アカウントに関連付けられているメール アドレスを使う必要があります。 詳しくは、「[基幹業務アプリの操作](/microsoft-store/working-with-line-of-business-apps)」をご覧ください。

企業は、専用アプリを公開してほしいと思う開発者に、関連付けを確認するためのリンクを含むメールを送信します。 **[アカウント設定]** の **[企業団体]** セクションに移動することで、これらの関連付けを確認することもできます (開発者アカウントを開くために使った Microsoft アカウントでサインインしている場合に限ります)。

関連付けを確認するには、**[承諾]** をクリックします。 お使いのアカウントが、その企業専用のアプリを公開できるようになります。

### <a name="submit-lob-apps"></a>LOB アプリの提出

企業専用のアプリを公開する準備ができたら、プロセスは、アプリの申請プロセスとほぼ同じです。 アプリは同じ[認定プロセス](the-app-certification-process.md)で処理され、すべての [Microsoft Store ポリシー](store-policies.md)に準拠している必要があります。 異なる点はわずかです。

#### <a name="visibility"></a>視程

企業の関連付けを設定したら、アプリを申請するたびに、その申請の **[価格と使用可能状況]** ページの **[表示]** セクションにドロップダウン ボックスが表示されます。 既定では、これは **[小売配布]** に設定されています。 アプリを企業専用にするには、**[基幹業務 (LOB) 配布]** を選択する必要があります。

**[基幹業務 (LOB) 配布]** を選択すると、通常の **[表示]** オプションが専用アプリの公開先として選択できる企業の一覧に変わります。 選択した企業の外部のユーザーは、アプリを表示することもダウンロードすることもできません。

基幹業務アプリとして公開するには、少なくとも 1 つの企業を選択する必要があります。

<span id="organizational" />

#### <a name="organizational-licensing"></a>組織のライセンス

既定では、アプリを申請するとき、**[ストアで管理される (オンライン) ボリューム ライセンスと配布]** のチェック ボックスはオンになっています。 LOB アプリを公開するとき、このチェック ボックスは、企業がアプリをボリューム取得できるようにオンにしておく必要があります。 このように設定しても、**[分布と認知度]** セクションで選択した企業以外のユーザーが、このアプリを利用できるようになるわけではありません。

未接続状態 (オフライン) のライセンスによって企業がアプリを利用できるようにするには、**[組織で管理される (オフライン) ライセンスと配布]** チェック ボックスもオンにします。

詳しくは、「[組織のライセンス オプション](organizational-licensing.md)」をご覧ください。

#### <a name="age-ratings"></a>年齢区分

LOB アプリでの申請のプロセスの[年齢区分](age-ratings.md)の手順は、販売アプリと同様ですが、アンケートへの回答や既存の IARC 評価 ID のインポートを行わずに、ストアの年齢区分を手動で指定できる追加のオプションもあります。 この手動の区分は、基幹業務 (LOB) 配布のみで使うことができます。そのため、アプリの **[表示]** の設定を **[小売配布] ** に変更する場合には、申請を行う前に、年齢区分のアンケートへの回答を行う必要があります。

### <a name="enterprise-deployment-of-lob-apps"></a>LOB アプリの企業展開

**[ストアに提出]** をクリックすると、アプリの認定プロセスが開始します。 準備ができたら、企業の管理者が、そのアプリをビジネス向け Microsoft ストアまたは教育機関向け Microsoft ストア ポータルのプライベート ストアに追加する必要があります。 その後、企業はそのアプリをユーザーに展開できます。

> [!NOTE]
> LOB アプリを取得するには、組織が[サポート対象の市場](/windows/whats-new/windows-store-for-business-overview#supported-markets)に含まれている必要があります。また、アプリを提出する際に、その[市場を除外](./define-market-selection.md)することはできません。 

詳しくは、[基幹業務アプリの操作](/microsoft-store/working-with-line-of-business-apps)に関するページ、および[プライベート ストアを使用したアプリの配布](/microsoft-store/distribute-apps-from-your-private-store)に関するページをご覧ください。

### <a name="update-lob-apps"></a>LOB アプリの更新

LOB として既に公開したアプリに更新プログラムを公開するには、新しい申請を作成するだけです。 新しいパッケージをアップロードするか、変更を加えて、**[ストアに提出]** をクリックし、更新されたバージョンを利用できるようにします。 **[表示]** で選択されている企業はそのままにしてください (アプリを取得できるようにする企業を追加する、前にアプリを配布していた企業を削除するなど、意図的に変更する場合を除く)。

前に基幹業務として公開したアプリの提供を終了して、これ以上取得できないようにするには、新しい申請を作成する必要があります。 最初に、**[表示]** セクションを **[基幹業務 (LOB) 配布]** から **[小売配布]** に変更します。 次に [[Discoverability]](choose-visibility-options.md#discoverability) (見つけやすさ) セクションで **[この製品を Microsoft Store で提供しますが、検索はできないようにします]** と **[購入の停止]** オプションを選びます。

申請の認定プロセスが完了すると、以降そのアプリは取得できなくなります (アプリを既に所有しているユーザーは引き続き使用できます)。

> [!NOTE]
> アプリを **[小売配布]** に変更する場合には、[年齢区分のアンケート](age-ratings.md)に回答していない場合、回答する必要があります (アプリが新規取得用に提供されない場合でも同様です)。