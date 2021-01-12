---
title: アカウント ユーザーの役割またはカスタムのアクセス許可の設定
description: パートナーセンターアカウントにユーザーを追加するときに、標準ロールまたはカスタムアクセス許可を設定する方法について説明します。
ms.assetid: 99f3aa18-98b4-4919-bd7b-d78356b0bf78
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, uwp, ユーザー ロール, ユーザーのアクセス許可, カスタム ロール, ユーザー アクセス, アクセス許可のカスタマイズ, 標準ロール
ms.localizationpriority: medium
ms.openlocfilehash: bdf46b681c7ae2bce18bab464ac75984f784fc7b
ms.sourcegitcommit: afc4ff2c89f148d32073ab1cc42063ccdc573a8c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/11/2021
ms.locfileid: "98104643"
---
# <a name="set-roles-or-custom-permissions-for-account-users"></a>アカウント ユーザーの役割またはカスタムのアクセス許可の設定

[パートナーセンターアカウントにユーザーを追加](add-users-groups-and-azure-ad-applications.md)する場合は、アカウント内のアクセス権を指定する必要があります。 それには、アカウント全体に適用される[標準ロール](#roles)を割り当てます。または、[ユーザーのアクセス許可をカスタマイズ](#custom)して、適切なレベルのアクセスを提供することもできます。 カスタムのアクセス許可はアカウント全体に適用されるものもあれば、特定の 1 つまたは複数の製品のみを対象にできるものもあります (必要に応じて全製品を対象にすることもできます)。

> [!NOTE] 
> ユーザー、グループ、Azure AD アプリケーションのどれを追加する場合でも、適用できるロールやアクセス許可は同じです。

適用するロールまたはアクセス許可を決めるときは、次のことに注意してください。 
-   ユーザー (グループと Azure AD アプリケーションを含む) は、割り当てられたロールに関連付けられたアクセス許可を持つパートナーセンターアカウント全体にアクセスできるようになります。ただし、 [アクセス許可をカスタマイズ](#custom) し、 [製品レベルのアクセス許可](#product-level-permissions) を割り当てて、特定のアプリやアドオンだけを使用できるようにすることはできません。
-   複数のロールを選択するか、カスタムのアクセス許可を使って必要なアクセス許可を付与することにより、ユーザー、グループ、または Azure AD アプリケーションが複数のロールの機能にアクセスできるようにすることができます。
-   特定の 1 つのロール (またはカスタムのアクセス許可のセット) を持つユーザーは、別のロール (またはアクセス許可のセット) を持つグループの一部となることも可能です。 その場合ユーザーは、グループと個人アカウントの両方に関連付けられた機能のすべてにアクセスできます。

> [!TIP]
> このトピックは、 [パートナーセンター](https://partner.microsoft.com/dashboard)の Windows アプリ開発者プログラムに固有のものです。 ハードウェア開発者プログラムでのユーザー ロールについて詳しくは、「[ユーザー ロールの管理](/windows-hardware/drivers/dashboard/managing-user-roles)」をご覧ください。 Windows デスクトップ アプリケーション プログラムでのユーザー ロールについて詳しくは、「[Windows デスクトップ アプリケーション プログラム](/windows/desktop/appxpkg/windows-desktop-application-program#add-and-manage-account-users)」をご覧ください。


<span id="roles" />

## <a name="assign-roles-to-account-users"></a>アカウント ユーザーへの役割の割り当て

既定では、ユーザー、グループ、または Azure AD アプリケーションをパートナーセンターアカウントに追加するときに、一連の標準的な役割が選択されます。 各ロールには、アカウント内で特定の機能を実行するための特定のアクセス許可セットが付与されています。 

**[アクセス許可のカスタマイズ]** を選び、[カスタムのアクセス許可](#custom)を定義していない場合は、アカウントに追加するユーザー、グループ、または Azure AD アプリケーションそれぞれに、少なくとも次の標準ロールのいずれかを割り当てる必要があります。 

> [!NOTE]
> アカウントの **オーナー** は、最初に Microsoft アカウントでそのアカウントを作成したユーザーです (Azure AD によって追加されたユーザーではありません)。 このアカウント所有者のみが、アカウントに完全にアクセスして、アプリを削除したり、すべてのアカウント ユーザーを作成および編集したり、すべての財務およびアカウント設定を変更したりできます。 


| ロール                 | 説明              |
|----------------------|--------------------------|
| Manager              | アカウントに完全にアクセスできます (税金と支払いの設定の変更を除く)。 これには、パートナーセンターでのユーザーの管理が含まれますが、Azure AD テナントでユーザーを作成および削除する機能は、Azure AD でのアカウントのアクセス許可によって異なります。 つまり、ユーザーにマネージャーロールが割り当てられていても、組織の Azure AD のグローバル管理者のアクセス許可を持っていない場合は、新しいユーザーを作成したり、ディレクトリからユーザーを削除したりすることはできません (ただし、ユーザーのパートナーセンターの役割を変更することはできます)。 <p> パートナーセンターアカウントが複数の Azure AD テナントに関連付けられている場合、管理者は、そのテナントのグローバル管理者のアクセス許可を持つアカウントを使用してユーザーと同じテナントにサインインしていない限り、ユーザーの完全な詳細 (姓、名、パスワード回復用電子メール、Azure AD グローバル管理者) を表示できません。 ただし、パートナーセンターアカウントに関連付けられているテナントにユーザーを追加したり、削除したりすることはできます。 |
| 開発者            | パッケージをアップロードし、アプリおよびアドオンを申請できます。また、[使用状況レポート](usage-report.md)で統計情報の詳細を確認できます。 [デバイス間のエクスペリエンス](https://developer.microsoft.com/windows/project-rome)機能にアクセスできます。 財務情報やアカウントの設定を表示することはできません。   |
| 経営担当者 | [[正常性]](health-report.md) レポートと [[使用状況]](usage-report.md) レポートを表示できます。 製品の作成や申請、アカウント設定の変更、財務情報の表示はできません。   |
| 財務担当者  | [支払いレポート](/partner-center/payout-statement)、財務情報、取得レポートを表示できます。 アプリ、アドオン、またはアカウント設定を変更することはできません。    |
| マーケター             | [顧客のレビューに返信](respond-to-customer-reviews.md)したり、非財務[分析レポート](analytics.md)を表示したりできます。 アプリ、アドオン、またはアカウント設定を変更することはできません。      |

以下の表は、これらの各ロール (およびアカウント所有者) が使用できる特定の機能の一部を示しています。

|                                                       |    アカウント所有者                 |    Manager                       |    Developer                     |    経営担当者    |    財務担当者    |    マーケター                      |
|-------------------------------------------------------|----------------------------------|----------------------------------|----------------------------------|----------------------------|---------------------------|----------------------------------|
|    **取得レポート (ほぼリアルタイムのデータを含む)** |    表示可能                      |    表示可能                      |    アクセス権なし                     |    アクセス権なし               |    表示可能               |    アクセス権なし                     |
|    **フィードバック レポート/応答**                          |    フィードバックの表示および送信可能    |    フィードバックの表示および送信可能    |    フィードバックの表示および送信可能    |    アクセス権なし               |    アクセス権なし              |    フィードバックの表示および送信可能    |
|    **正常性レポート (ほぼリアルタイムのデータを含む)**      |    表示可能                      |    表示可能                      |    表示可能                      |    表示可能                |    アクセス権なし              |    アクセス権なし                     |
|    **利用状況レポート**                                       |    表示可能                      |    表示可能                      |    表示可能                      |    表示可能                |    アクセス権なし              |    アクセス権なし                     |
|    **支払い受取口座**                                     |    更新可能                    |    アクセス権なし                     |    アクセス権なし                     |    アクセス権なし               |    更新可能             |    アクセス権なし                     |
|    **税プロファイル**                                        |    更新可能                    |    アクセス権なし                     |    アクセス権なし                     |    アクセス権なし               |    更新可能             |    アクセス権なし                     |
|    **支払いの概要**                                     |    表示可能                      |    アクセス権なし                     |    アクセス権なし                     |    アクセス権なし               |    表示可能               |    アクセス権なし                     |

適切な標準ロールがない場合や、特定のアプリやアドオンへのアクセスを制限する場合は、以下の説明に従って **[アクセス許可のカスタマイズ]** を選んでユーザーにカスタムのアクセス許可を付与できます。


<span id="custom" />

## <a name="assign-custom-permissions-to-account-users"></a>アカウント ユーザーへのカスタムのアクセス許可の割り当て

標準ロールではなく、カスタムのアクセス許可を割り当てるには、ユーザー アカウントを追加または編集するときに、**[ロール]** セクションの **[アクセス許可のカスタマイズ]** をクリックします。 

ユーザーのアクセス許可を有効にするには、ボックスを適切な設定に切り替えます。 

![アクセス設定のガイド](images/permission_key.png)

- **アクセス権なし**: ユーザーは指定されたアクセス許可を持っていません。
- **読み取り専用**: ユーザーは指定された領域に関連する機能を表示できますが、変更はできません。 
- **読み取り/書き込み**: ユーザーは、領域の表示と領域に関連付けられている変更を実行できます。
- **混合**: このオプションを直接選択することはできませんが、該当のアクセス許可に対して組み合わせたアクセスを許可している場合は、**[混合]** インジケーターが表示されます。 たとえば、**[すべての製品]** に対して **[価格と使用可能状況]** への **読み取り専用** アクセスを付与しているときに、ある特定の製品に対して **[価格と使用可能状況]** への **読み取り/書き込み** アクセスを付与する場合は、**[すべての製品]** の **[価格と使用可能状況]** インジケーターが [混合] として表示されます。 これは、一部の製品にアクセス許可の **アクセス権なし** が設定されているときに、他の製品に **読み取り/書き込み** と **読み取り専用** の両方またはそのいずれかのアクセスが設定されている場合も、同様に適用されます。

分析データの表示に関連するものなど、一部のアクセス許可で付与できるのは、**読み取り専用** アクセスのみです。 現在の実装では、**読み取り専用** と **読み取り/書き込み** のアクセスが区別されないアクセス許可もありますので注意してください。 **読み取り専用** アクセスまたは **読み取り/書き込み** アクセス、あるいはその両方によって付与される特定の機能を理解するには、各アクセス許可の詳細を確認してください。

それぞれのアクセス許可に関する特定の詳細については、下記の表で説明します。

## <a name="account-level-permissions"></a>アカウント レベルのアクセス許可

このセクションのアクセス許可を特定の製品に対して制限することはできません。 これらのアクセス許可のいずれかにアクセス権を付与すると、ユーザーは該当するアクセス許可をアカウント全体に対して獲得します。

<table>
    <colgroup>
    <col width="20%" />
    <col width="40%" />
    <col width="40%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">アクセス許可名</th>
    <th align="left">読み取り専用</th>
    <th align="left">読み取り/書き込み</th>
    </tr>
    </thead>
    <tbody>
<tr><td align="left">    <b>アカウントの設定</b>                    </td><td align="left">  <a href="/partner-center/partner-center-account-setup">連絡先情報</a>など、<b>[アカウント設定]</b> セクションのすべてのページを表示できます。       </td><td align="left">  <b>アカウント設定</b>セクションのすべてのページを表示できます。 <a href="/partner-center/partner-center-account-setup">連絡先情報</a>と他のページを変更できますが、支払いアカウントや税プロファイルは変更できません (アクセス許可が個別に付与されている場合を除く)。            </td></tr>
<tr><td align="left">    <b>アカウントユーザー</b>                       </td><td align="left">  <b>[ユーザー]</b> セクションでアカウントに追加されているユーザーを表示できます。          </td><td align="left">  <b>[ユーザー]</b> セクションで、ユーザーのアカウントへの追加と既存のユーザーの変更ができます。             </td></tr>
<tr><td align="left">    <b>アカウントレベルの ad パフォーマンスレポート</b> </td><td align="left">  アカウント レベルの <a href="advertising-performance-report.md">[広告パフォーマンス] レポート</a>を表示できます。      </td><td align="left">  該当なし   </td></tr>
<tr><td align="left">    <b>Ad キャンペーン</b>                        </td><td align="left">  アカウントで作成した<a href="/windows/uwp/monetize/">広告キャンペーン</a>を表示できます。      </td><td align="left">  アカウントで作成した<a href="/windows/uwp/monetize/">広告キャンペーン</a>を作成、管理、および表示できます。          </td></tr>
<tr><td align="left">    <b>Ad 仲介</b>                        </td><td align="left">  アカウント内のすべての製品の広告仲介の構成を表示できます。    </td><td align="left">  アカウント内のすべての製品の広告仲介の構成の表示と変更ができます。        </td></tr>
<tr><td align="left">    <b>Ad 仲介レポート</b>                </td><td align="left">  アカウント内のすべての製品の<a href="/windows/uwp/publish/advertising-performance-report">広告仲介レポート</a>を表示できます。    </td><td align="left">  該当なし    </td></tr>
<tr><td align="left">    <b>Ad パフォーマンスレポート</b>              </td><td align="left">  アカウント内のすべての製品の <a href="advertising-performance-report.md">[広告パフォーマンス] レポート</a>を表示できます。       </td><td align="left">  該当なし         </td></tr>
<tr><td align="left">    <b>Ad ユニット</b>                            </td><td align="left">  アカウント用に作成された<a href="in-app-ads.md">広告ユニット</a>を表示できます。    </td><td align="left">  アカウントの<a href="in-app-ads.md">広告ユニット</a>を作成、管理、および表示できます。             </td></tr>
<tr><td align="left">    <b>関連広告</b>                       </td><td align="left">  アカウント内のすべての製品で<a href="/windows/uwp/publish/in-app-ads">アフィリエイト広告</a>の利用状況を表示できます。    </td><td align="left">  アカウント内のすべての製品に対して<a href="/windows/uwp/publish/in-app-ads">アフィリエイト広告</a>の利用状況の管理と表示ができます。                </td></tr>
<tr><td align="left">    <b>関連会社のパフォーマンスレポート</b>      </td><td align="left">  アカウント内のすべての製品の<a href="/windows/uwp/publish/advertising-performance-report">アフィリエイト パフォーマンス レポート</a>を表示できます。   </td><td align="left">  該当なし   </td></tr>
<tr><td align="left">    <b>アプリインストール広告レポート</b>             </td><td align="left">  <a href="/windows/uwp/publish/ad-campaign-report">[広告キャンペーン] レポート</a>を表示できます。           </td><td align="left">  該当なし   </td></tr>
<tr><td align="left">    <b>コミュニティ広告</b>                       </td><td align="left">  アカウント内のすべての製品の無料<a href="/windows/uwp/monetize/">コミュニティ広告</a>の利用状況を表示できます。          </td><td align="left">  アカウント内のすべての製品の無料<a href="/windows/uwp/monetize/">コミュニティ広告</a>の利用状況を作成、管理、および表示できます。               </td></tr>
<tr><td align="left">    <b>連絡先情報</b>                        </td><td align="left">  [アカウント設定] セクションで<a href="/partner-center/partner-center-account-setup">連絡先情報</a>を表示できます。        </td><td align="left">  [アカウント設定] セクションで<a href="/partner-center/partner-center-account-setup">連絡先情報</a>の編集と表示ができます。            </td></tr>
<tr><td align="left">    <b>COPPA コンプライアンス</b>                    </td><td align="left">  アカウント内のすべての製品の <a href="in-app-ads.md#coppa-compliance">COPPA 準拠</a>の選択 (製品が13歳未満の子供を対象とするかどうかを示す) を表示できます。                                            </td><td align="left">  アカウント内のすべての製品の <a href="in-app-ads.md#coppa-compliance">COPPA 準拠</a>の選択 (製品が13歳未満の子供を対象とするかどうかを示す) の編集と表示ができます。         </td></tr>
<tr><td align="left">    <b>顧客グループ</b>                     </td><td align="left">  <a href="create-customer-groups.md">顧客グループ</a>(セグメントと既知のユーザーグループ) を表示できます。      </td><td align="left">  <a href="create-customer-groups.md">顧客グループ</a>(セグメントと既知のユーザーグループ) を作成、編集、および表示できます。       </td></tr>
<tr><td align="left">    <b>製品グループの管理</b>&nbsp;*                            </td><td align="left">  新しい製品グループの作成ページを表示できますが、実際に新しい製品グループを作成することはできません。    </td><td align="left">  製品グループを作成して編集できます。     </td></tr>
<tr><td align="left">    <b>新しいアプリ</b>                            </td><td align="left">  新しいアプリの作成ページを表示できますが、実際にはアカウントに新しいアプリを作成することはできません。    </td><td align="left">  新しいアプリの名前を予約することで、アカウントで<a href="create-your-app-by-reserving-a-name.md">新しいアプリを作成</a>できます。また、申請を作成してアプリをストアに提出できます。     </td></tr>
<tr><td align="left">    <b>新しいバンドル</b>&nbsp;*                       </td><td align="left">  新しいバンドルの作成ページを表示できますが、実際にはアカウントに新しいバンドルを作成することはできません。     </td><td align="left">  製品の新しいバンドルを作成できます。          </td></tr>
<tr><td align="left">    <b>パートナーサービス</b>&nbsp;*                  </td><td align="left">  XToken を取得するサービスをインストールするための証明書を表示できます。     </td><td align="left">  XToken を取得するサービスをインストールするための証明書の管理と表示ができます。       </td></tr>
<tr><td align="left">    <b>支払い勘定科目</b>                      </td><td align="left">  <b>[アカウント設定]</b> に<a href="/partner-center/set-up-your-payout-account#payout-account">支払いアカウントの情報</a>を表示できます。     </td><td align="left">  <b>[アカウント設定]</b> で <a href="/partner-center/set-up-your-payout-account#payout-account">支払いアカウントの情報</a> の編集と表示ができます。       </td></tr>
<tr><td align="left">    <b>支払いの概要</b>                      </td><td align="left">  <a href="/partner-center/payout-statement">支払いの概要</a>を表示して、支払いレポート情報にアクセスしてダウンロードできます。       </td><td align="left">  <a href="/partner-center/payout-statement">支払いの概要</a>を表示して、支払いレポート情報にアクセスしてダウンロードできます。   </td></tr>
<tr><td align="left">    <b>証明書利用者</b>&nbsp;*                   </td><td align="left">  XToken を取得する証明書利用者を表示できます。    </td><td align="left">  XToken を取得する証明書利用者の管理と表示ができます。     </td></tr>
<tr><td align="left">    <b>サンド</b>&nbsp;*                         </td><td align="left">  <b>サンドボックス</b> ページにアクセスして、アカウント内のサンドボックスとそれらのサンドボックスに適用可能なすべての構成を表示できます。 適切な製品レベルのアクセス許可が付与されている場合を除き、サンドボックスごとに製品と申請を表示することはできません。 </td><td align="left">  <b>サンドボックス</b> ページにアクセスして、サンドボックスの作成と削除、およびサンドボックスの構成の管理など、アカウントでサンドボックスを表示して管理できます。 適切な製品レベルのアクセス許可が付与されている場合を除き、サンドボックスごとに製品と申請を表示することはできません。    </td></tr>
<tr><td align="left">    <b>店舗売上イベント</b>&nbsp;*                            </td><td align="left">  該当なし    </td><td align="left">  Microsoft Store セール イベントに製品を自動的に含めるオプションを構成できます。     </td></tr>
<tr><td align="left">    <b>税プロファイル</b>                         </td><td align="left">  <b>[アカウント設定]</b> に<a href="/partner-center/set-up-your-payout-account#tax-forms">税プロファイルの情報とフォーム</a>を表示できます。     </td><td align="left">  <b>[アカウント設定]</b> で税フォームに入力して、<a href="/partner-center/set-up-your-payout-account#tax-forms">税プロファイル情報</a>を更新できます。     </td></tr>
<tr><td align="left">    <b>テストアカウント</b>&nbsp;*                     </td><td align="left">  Xbox Live の構成をテストするためのアカウントを表示できます。      </td><td align="left">  Xbox Live の構成をテストするためのアカウントを作成、管理、および表示できます。      </td></tr>
<tr><td align="left">    <b>Xbox デバイス</b>                        </td><td align="left">  <b>[アカウント設定]</b> セクションでアカウントに対して有効にされている Xbox 開発コンソールを表示できます。       </td><td align="left">  <b>[アカウント設定]</b> セクションでアカウントに対して有効にされている Xbox 開発コンソールを追加、削除、および表示できます。     </td></tr>
    </tbody>
    </table>

\* アスタリスク (*) でマークされたアクセス許可は、すべてのアカウントで使用できない機能へのアクセスを許可します。 これらの機能に対してアカウントが有効になっていない場合は、これらのアクセス許可の選択内容には一切影響はありません。   


## <a name="product-level-permissions"></a>製品レベルのアクセス許可

このセクションのアクセス許可は、アカウント内のすべての製品に付与するか、または 1 つ以上の特定の製品に対してのみアクセス許可を付与するようにカスタマイズできます。 

製品レベルのアクセス許可は 4 つのカテゴリにグループ分けされます: **分析**、**収益化**、**公開**、および **Xbox Live**。 各カテゴリを展開して、カテゴリごとに個々のアクセス許可を表示できます。 また、1 つ以上の特定の製品に対して **すべてのアクセス許可** を有効にすることもできます。

アカウント内のすべての製品にアクセス許可を付与するには、**[すべての製品]** とマークされている行でアクセス許可 を選択 (トグルして **[読み取り専用]** または **[読み取り/書き込み]** を指定) します。 
 
> [!TIP]
> **[すべての製品]** に対する選択内容は、現在アカウント内にあるすべての製品だけでなく、アカウント内で作成される将来の製品にも適用されます。 今後の製品にアクセス許可が適用されないようにするには、**[すべての製品]** を選ぶのではなく、全製品を個別に選びます。

**[すべての製品]** 行の下に、別の行に記載されているアカウントに各製品が表示されます。 特定の製品にのみアクセス許可を付与するには、該当する製品の行でアクセス許可を選択します。

各アドオンは、親製品の下で個々の行と **[すべてのアドオン]** 行に一覧表示されます。 **[すべてのアドオン]** に対する選択内容は、その製品に対するすべての現在のアドオンだけでなく、その製品用に作成される将来のアドオンにも適用されます。

アドオンに対して設定できないアクセス許可もありますので注意してください。 これは、アクセス許可がアドオン (**カスタマー フィードバック** アクセス許可など) に適用されない、または親製品レベルで付与されたアクセス許可がその製品のすべてのアドオン (**プロモーション コード** など) に適用されるためです。 ただし、アドオンに対して適用可能なすべてのアクセス許可は個別に設定する必要があることに注意してください。アドオンは親製品に対する選択内容を継承しません。 たとえば、ユーザーがアドオンの価格と使用可能状況をを選択できるようにする場合、親製品に **[価格と使用可能状況]** アクセス許可を付与しているかどうかに関係なく、アドオン (または **[すべてのアドオン]**) に対して **[価格と使用可能状況]** アクセス許可を有効にする必要があります。 


### <a name="analytics"></a>Analytics

<table>
    <thead>
    <tr class="header">
    <th align="left">アクセス許可名&nbsp;</th>
    <th align="left">読み取り専用&nbsp;</th>
    <th align="left">読み取り/書き込み</th>
    <th align="left">読み取り専用&nbsp;(アドオン)&nbsp; </th>
    <th align="left">読み取り/書き込み&nbsp;(アドオン)</th>
    </tr>
    </thead>
    <tbody>
    <tr><td align="left">    <b>買収</b> (ほぼリアルタイムのデータを含む) </td><td>    製品の <a href="acquisitions-report.md">[取得]</a> レポートと <a href="add-on-acquisitions-report.md">[アドオン取得]</a> レポートを表示できます。        </td><td>    該当なし    </td><td>    該当なし (親製品の設定には **アドオン** の取得レポートが含まれます)        </td><td>    該当なし                         </td></tr>
    <tr><td align="left">    <b>ユーセジリンク</b> </td><td>    製品の <a href="usage-report.md">[使用状況] レポート</a>を表示できます。     </td><td>    該当なし       </td><td>    該当なし     </td><td>    該当なし         </td></tr>
    <tr><td align="left">    <b>正常性</b> (ほぼリアルタイムのデータを含む) </td><td>    製品の<a href="health-report.md">正常性レポート</a>を表示できます。    </td><td>    該当なし     </td><td>    該当なし     </td><td>    該当なし         </td></tr>
    <tr><td align="left">    <b>お客様からのフィードバック</b>    </td><td>    製品の <a href="reviews-report.md">[レビュー]</a> レポートと <a href="feedback-report.md">[フィードバック]</a> レポートを表示できます。       </td><td>    該当なし (フィードバックやレビューに返信するには、<b>[顧客への連絡]</b> アクセス許可を付与する必要があります)   </td><td>    該当なし     </td><td>    該当なし         </td></tr>
    <tr><td align="left">    <b>Xbox 分析</b> </td><td>    製品の <a href="xbox-analytics-report.md">Xbox analytics レポート</a> を表示できます。    </td><td>    該当なし   </td><td>    該当なし       </td><td>    該当なし          </td></tr>
    </tbody>
    </table>

### <a name="monetization"></a>収益化

<table>
    <thead>
    <tr class="header">
    <th align="left">アクセス許可名&nbsp;</th>
    <th align="left">読み取り専用&nbsp;</th>
    <th align="left">読み取り/書き込み</th>
    <th align="left">読み取り専用&nbsp;(アドオン)&nbsp; </th>
    <th align="left">読み取り/書き込み&nbsp;(アドオン)</th>
    </tr>
    </thead>
    <tbody>
    <tr><td align="left">    <b>プロモーションコード</b>     </td><td>    製品とそのアドオンの<a href="generate-promotional-codes.md">プロモーション コード</a>の注文と利用状況の情報、また利用状況情報を表示できます。         </td><td>    製品とそのアドオンの<a href="generate-promotional-codes.md">プロモーション コード</a>の注文の表示、管理、および作成、また利用状況情報の表示ができます。          </td><td>    該当なし (すべてのアドオンに親製品の設定が適用されます)     </td><td>    該当なし (すべてのアドオンに親製品の設定が適用されます)     </td></tr>
    <tr><td align="left">    <b>対象となるプラン</b>     </td><td>    製品の<a href="use-targeted-offers-to-maximize-engagement-and-conversions.md">対象プラン</a>を表示できます。         </td><td>    製品の<a href="use-targeted-offers-to-maximize-engagement-and-conversions.md">対象プラン</a>を表示、管理、作成できます。          </td><td>    該当なし     </td><td>    該当なし      </td></tr>
    <tr><td align="left">    <b>お客様へのお問い合わせ</b>  </td><td>    <b>カスタマー フィードバック</b> アクセス許可も付与されている限り、<a href="respond-to-customer-feedback.md">カスタマー フィードバックへの返信</a>と<a href="respond-to-customer-reviews.md">カスタマー レビューへの返信</a>を表示できます。 製品に対して作成された<a href="send-push-notifications-to-your-apps-customers.md">ターゲット通知</a>も表示できます。    </td><td>    カスタマー<b>フィードバックのアクセス許可</b>も付与されている限り、顧客からの<a href="respond-to-customer-feedback.md">フィードバックに応答</a>して<a href="respond-to-customer-reviews.md">顧客のレビューに応答</a>できます。 製品に対する<a href="send-push-notifications-to-your-apps-customers.md">ターゲット通知の作成と送信</a>もできます。                   </td><td>    該当なし         </td><td>    該当なし                          </td></tr>
    <tr><td align="left">    <b>実験</b></td><td>    製品の<a href="../monetize/run-app-experiments-with-a-b-testing.md">実験 (A/Bテスト)</a>と実験データを表示できます。   </td><td>    製品の<a href="../monetize/run-app-experiments-with-a-b-testing.md">実験 (A/B テスト)</a> の作成、管理、および表示と、実験データの表示ができます。     </td><td>    該当なし  </td><td>    該当なし                 </td></tr>
    <tr><td align="left">    <b>店舗売上イベント</b>&nbsp;*</td><td>    製品のセール イベントの状態を表示できます。   </td><td>    製品をセール イベントに追加して、割引を構成できます。      </td><td>    製品のセール イベントの状態を表示できます。   </td><td>    製品をセール イベントに追加して、割引を構成できます。      </td></tr>
    </tbody>
    </table>

### <a name="publishing"></a>発行 

<table>
    <thead>
    <tr class="header">
    <th align="left">アクセス許可名&nbsp;</th>
    <th align="left">読み取り専用&nbsp;</th>
    <th align="left">読み取り/書き込み</th>
    <th align="left">読み取り専用&nbsp;(アドオン)&nbsp; </th>
    <th align="left">読み取り/書き込み&nbsp;(アドオン)</th>
    </tr>
    </thead>
    <tbody>
    <tr><td align="left">    <b>製品のセットアップ</b>  </td><td>    製品のセットアップページを表示できます。     </td><td>    製品のセットアップページを表示および編集できます。 </td><td>    アドオンの製品のセットアップページを表示できます。   </td><td>    製品のセットアップページアドオンを表示および編集できます。          </td></tr>
    <tr><td align="left">    <b>価格と可用性</b>  </td><td>    製品の <a href="set-app-pricing-and-availability.md">価格と可用性</a> のページを表示できます。     </td><td>    製品の <a href="set-app-pricing-and-availability.md">価格と可用性</a> に関するページを表示および編集できます。 </td><td>    アドオンの <a href="set-add-on-pricing-and-availability.md">[価格と可用性</a> ] ページを表示できます。   </td><td>    アドオンの [ <a href="set-add-on-pricing-and-availability.md">価格と可用性</a> ] ページを表示および編集できます。          </td></tr>
    <tr><td align="left">    <b>属性</b>   </td><td>    製品の <a href="enter-app-properties.md">プロパティ</a> ページを表示できます。      </td><td>    製品の <a href="enter-app-properties.md">プロパティ</a> ページを表示および編集できます。       </td><td>    アドオンの [ <a href="enter-add-on-properties.md">プロパティ</a> ] ページを表示できます。     </td><td>    アドオンの [ <a href="enter-add-on-properties.md">プロパティ</a> ] ページを表示および編集できます。               </td></tr>
    <tr><td align="left">    <b>年齢区分</b>    </td><td>    製品の <a href="age-ratings.md">年齢</a> 区分ページを表示できます。       </td><td>    製品の <a href="age-ratings.md">年齢</a> 区分ページを表示および編集できます。    </td><td>    アドオンの年齢区分ページを表示できます。          </td><td>     アドオンの年齢区分ページを表示および編集できます。       </td></tr>
    <tr><td align="left">    <b>パッケージ</b>        </td><td>    製品の [ <a href="upload-app-packages.md">パッケージ</a> ] ページを表示できます。  </td><td>    パッケージのアップロードなど、製品の <a href="upload-app-packages.md">パッケージ</a> ページを表示および編集できます。     </td><td>   アドオンの [ <a href="upload-app-packages.md">パッケージ</a> ] ページを表示できます (該当する場合)。   </td><td>     アドオンの [ <a href="upload-app-packages.md">パッケージ</a> ] ページを表示および編集できます (該当する場合)。             </td></tr>
    <tr><td align="left">    <b>ストアの一覧</b>  </td><td>    製品の <a href="create-app-store-listings.md">ストアリスティングページ</a> を表示できます。  </td><td>    <a href="create-app-store-listings.md">店舗の一覧ページ</a>の製品を表示および編集できます。また、さまざまな言語の新しい店舗一覧を追加できます。     </td><td>    アドオンの <a href="create-add-on-store-listings.md">ストアの一覧ページ</a> を表示できます。            </td><td>    アドオンの <a href="create-add-on-store-listings.md">ストアの一覧ページ</a> を表示および編集したり、異なる言語のストアの一覧を追加したりできます。                 </td></tr>
    <tr><td align="left">    <b>ストアの提出</b>     </td><td>    このアクセス許可が読み取り専用に設定されている場合は、アクセスは一切付与されません。           </td><td>    ストアに製品を提出して、証明レポートを表示できます。 新規および更新済みの両方の申請が含まれます。 </td><td>このアクセス許可が読み取り専用に設定されている場合は、アクセスは一切付与されません。     </td><td>    ストアにアドオンを提出して、証明レポートを表示できます。 新規および更新済みの両方の申請が含まれます。</td></tr>
    <tr><td align="left">    <b>新しい送信の作成</b>       </td><td>    このアクセス許可が読み取り専用に設定されている場合は、アクセスは一切付与されません。        </td><td>    製品の新しい<a href="app-submissions.md">申請</a>を作成できます。  </td><td>    このアクセス許可が読み取り専用に設定されている場合は、アクセスは一切付与されません。   </td><td>    アドオンの新しい<a href="add-on-submissions.md">申請</a>を作成できます。        </td></tr>
    <tr><td align="left">    <b>新しいアドオン</b>    </td><td>    このアクセス許可が読み取り専用に設定されている場合は、アクセスは一切付与されません。 </td><td>    製品の<a href="set-your-add-on-product-id.md">新しいアドオンを作成</a>できます。 </td><td>    該当なし    </td><td>    該当なし        </td></tr>
    <tr><td align="left">    <b>予約名</b>   </td><td>    製品の<a href="manage-app-names.md">アプリ名の管理</a>ページを表示できます。</td><td>    追加の名前の予約や予約済みの名前の削除など、製品の<a href="manage-app-names.md">アプリ名の管理</a>ページの表示と編集ができます。 </td><td>   アドオンの予約済みの名前を表示できます。    </td><td>   アドオンの予約済みの名前の表示と編集ができます。          </td></tr>
    <tr><td align="left">    <b>ディスクの要求</b>   </td><td>    要求ページをディスクに表示できます。 </td><td>    ディスク要求を作成できます。 </td><td>   該当なし    </td><td>   該当なし          </td></tr>
    <tr><td align="left">    <b>ディスクのロイヤリティ </b>   </td><td>    ディスクをロイヤリティページで表示できます。</td><td>    ディスクのロイヤリティを作成できます。 </td><td>   該当なし    </td><td>   該当なし          </td></tr>
    </tbody>
    </table>

### <a name="xbox-live-"></a>Xbox Live \*

<table>
    <thead>
    <tr class="header">
    <th align="left">アクセス許可名&nbsp;</th>
    <th align="left">読み取り専用&nbsp;</th>
    <th align="left">読み取り/書き込み</th>
    <th align="left">読み取り専用&nbsp;(アドオン)&nbsp; </th>
    <th align="left">読み取り/書き込み&nbsp;(アドオン)</th>
    </tr>
    </thead>
    <tbody>
    <tr><td align="left">    <b>証明書利用者</b>&nbsp;*</td><td>    アカウントの [証明書利用者] ページを表示できます。   </td><td>    アカウントの証明書利用者ページを表示および編集できます。    </td><td>    該当なし    </td><td>    該当なし                      </td></tr>
    <tr><td align="left">    <b>パートナーサービス</b>&nbsp;*</td><td>    アカウントの Web サービスページを表示できます。  </td><td>    アカウントの Web サービスページを表示および編集できます。      </td><td>    該当なし    </td><td>    該当なし                      </td></tr>
    <tr><td align="left">    <b>Xbox テストアカウント</b>&nbsp;*</td><td>    アカウントの [Xbox テストアカウント] ページを表示できます。  </td><td>    アカウントの [Xbox テストアカウント] ページを表示および編集できます。    </td><td>    該当なし    </td><td>    該当なし                      </td></tr>
    <tr><td align="left">    <b>サンドボックスごとの Xbox テストアカウント</b>&nbsp;*</td><td>    指定されたアカウントのサンドボックスに対してのみ、[Xbox テストアカウント] ページを表示できます。  </td><td>    Xbox テストを表示および編集できます。   <tr><td align="left">    <b>アカウントの指定したサンドボックスのみの [アカウント] ページ    </td><td>    該当なし    </td><td>    該当なし                      </td></tr>
    <tr><td align="left">    <b>Xbox デバイス</b>&nbsp;*</td><td>    アカウントの Xbox one 開発コンソールページを表示できます。  </td><td>    アカウントの Xbox one 開発コンソールページを表示および編集できます。    </td><td>    該当なし    </td><td>    該当なし                      </td></tr>
    <tr><td align="left">    <b>Xbox デバイス (サンドボックスあたり)</b>&nbsp;*</td><td>    指定されたアカウントのサンドボックスに対してのみ、Xbox one の開発コンソールページを表示できます。  </td><td>    アカウントの指定したサンドボックスに対してのみ、Xbox one 開発コンソールページを表示および編集できます。    </td><td>    該当なし    </td><td>    該当なし                      </td></tr>
    <tr><td align="left">    <b>アプリチャネル</b>&nbsp;*</td><td>    該当なし  </td><td>    OneGuide を通じて表示するために、Xbox コンソールにプロモーション用のビデオ チャンネルを公開できます。    </td><td>    該当なし    </td><td>    該当なし                      </td></tr>
    <tr><td align="left">    <b>サービスの構成</b>&nbsp;*</td><td>    製品の Xbox Live サービス構成ページを表示できます。  </td><td>    製品の Xbox Live サービス構成ページを表示および編集できます。    </td><td>    該当なし    </td><td>    該当なし                      </td></tr>
    <tr><td align="left">    <b>ツールへのアクセス</b>&nbsp;*</td><td>    製品で Xbox Live ツールを実行して、データのみを表示できます。  </td><td>    製品で Xbox Live ツールを実行して、データを表示および編集できます。    </td><td>    該当なし    </td><td>    該当なし                      </td></tr>
</tbody>
</table>

\* アスタリスク (*) でマークされたアクセス許可は、すべてのアカウントで使用できない機能へのアクセスを許可します。 これらの機能に対してアカウントが有効になっていない場合は、これらのアクセス許可の選択内容には一切影響はありません。