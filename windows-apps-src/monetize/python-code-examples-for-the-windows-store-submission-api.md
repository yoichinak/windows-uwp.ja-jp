---
ms.assetid: 8AC56AAF-8D8C-4193-A6B3-BB5D0669D994
description: このセクションの Python コード例を使用して、Microsoft Store 申請 API を使用する方法をご確認ください。
title: アプリ、アドオン、フライトを送信するための Python コード
ms.date: 07/10/2017
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store 申請 API, コード例, Python
ms.localizationpriority: medium
ms.openlocfilehash: 3fd29164dd91199b1557496985577ea956893250
ms.sourcegitcommit: 7e8dfd83b181fe720b4074cb42adc908e1ba5e44
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/26/2021
ms.locfileid: "98811260"
---
# <a name="python-sample-submissions-for-apps-add-ons-and-flights"></a>Python のコード例: アプリ、アドオン、およびフライトの申請

この記事では、次のタスクで [Microsoft Store 申請 API](create-and-manage-submissions-using-windows-store-services.md) を使用する方法を示す Python コード例を提供します。

* [Azure AD アクセストークンを取得する](#token)
* [アドオンの作成](#create-add-on)
* [パッケージ フライトの作成](#create-package-flight)
* [アプリの申請の作成](#create-app-submission)
* [アドオンの申請の作成](#create-add-on-submission)
* [パッケージ フライトの申請の作成](#create-flight-submission)

<span id="token" />

## <a name="obtain-an-azure-ad-access-token"></a>Azure AD アクセス トークンの取得

次の例は、Microsoft Store 申請 API のメソッドを呼び出すために使用できる [Azure AD アクセス トークンを取得](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token)する方法を示しています。 トークンを取得した後、Microsoft Store 申請 API の呼び出しでこのトークンを使用できるのは、その有効期限が切れるまでの 60 分間です。 トークンの有効期限が切れた後は、新しいトークンを生成できます。

:::code language="python" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/python/Examples.py" range="1-20":::

<span id="create-add-on" />

## <a name="create-an-add-on"></a>アドオンの作成

次の例は、アドオンを[作成](create-an-add-on.md)してから[削除](delete-an-add-on.md)する方法を示しています。

:::code language="python" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/python/Examples.py" range="26-52":::

<span id="create-package-flight" />

## <a name="create-a-package-flight"></a>パッケージ フライトの作成

次の例は、パッケージ フライトを[作成](create-a-flight.md)してから[削除](delete-a-flight.md)する方法を示しています。

:::code language="python" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/python/Examples.py" range="58-87":::

<span id="create-app-submission" />

## <a name="create-an-app-submission"></a>アプリの申請の作成

次の例は、Microsoft Store 申請 API のいくつかのメソッドを使用して、アプリの申請を作成する方法を示しています。 これを行うために、コードは最後に発行された送信の複製として新しい送信を作成し、複製された送信を更新してパートナーセンターにコミットします。 具体的には、この例では次のタスクを実行します。

1. まず、例では[指定されたアプリのデータを取得](get-an-app.md)します。
2. 次に、[アプリの保留中の申請を削除](delete-an-app-submission.md)します (存在する場合)。
3. その後、[アプリの新しい申請を作成](create-an-app-submission.md)します (新しい申請は、最後に公開された申請のコピーです)。
4. 新しい送信の一部の詳細を変更し、Azure Blob Storage に送信するための新しいパッケージをアップロードします。
5. 次に、パートナーセンターに新しい送信を [更新](update-an-app-submission.md) して [コミット](commit-an-app-submission.md) します。
6. 最後に、申請が正常にコミットされるまで、定期的に[新しい申請の状態をチェック](get-status-for-an-app-submission.md)します。

:::code language="python" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/python/Examples.py" range="93-166":::

<span id="create-add-on-submission" />

## <a name="create-an-add-on-submission"></a>アドオンの申請の作成

次の例は、Microsoft Store 申請 API のいくつかのメソッドを使用して、アドオンの申請を作成する方法を示しています。 これを行うために、コードは最後に発行された送信の複製として新しい送信を作成し、複製された送信を更新してパートナーセンターにコミットします。 具体的には、この例では次のタスクを実行します。

1. まず、例では[指定されたアドオンのデータを取得](get-an-add-on.md)します。
2. 次に、[アドオンの保留中の申請を削除](delete-an-add-on-submission.md)します (存在する場合)。
3. その後、[アドオンの新しい申請を作成](create-an-add-on-submission.md)します (新しい申請は、最後に公開された申請のコピーです)。
4. Azure Blob Storage に送信するためのアイコンを含む ZIP アーカイブをアップロードします。 詳細については、「 [アドオンの作成](manage-add-on-submissions.md#create-an-add-on-submission)」の AZURE BLOB STORAGE に ZIP アーカイブをアップロードする方法に関する関連手順を参照してください。
5. 次に、パートナーセンターに新しい送信を [更新](update-an-add-on-submission.md) して [コミット](commit-an-add-on-submission.md) します。
6. 最後に、申請が正常にコミットされるまで、定期的に[新しい申請の状態をチェック](get-status-for-an-add-on-submission.md)します。

:::code language="python" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/python/Examples.py" range="172-245":::

<span id="create-flight-submission" />

## <a name="create-a-package-flight-submission"></a>パッケージ フライトの申請の作成

次の例は、Microsoft Store 申請 API のいくつかのメソッドを使用して、パッケージ フライトの申請を作成する方法を示しています。 これを行うために、コードは最後に発行された送信の複製として新しい送信を作成し、複製された送信を更新してパートナーセンターにコミットします。 具体的には、この例では次のタスクを実行します。

1. まず、例では[指定されたパッケージ フライトのデータを取得](get-a-flight.md)します。
2. 次に、[パッケージ フライトの保留中の申請を削除](delete-a-flight-submission.md)します (存在する場合)。
3. その後、[パッケージ フライトの新しい申請を作成](create-a-flight-submission.md)します (新しい申請は、最後に公開された申請のコピーです)。
4. Azure Blob Storage に送信するための新しいパッケージをアップロードします。 詳細については、「 [パッケージの作成フライト送信](manage-flight-submissions.md#create-a-package-flight-submission)」の AZURE BLOB STORAGE に ZIP アーカイブをアップロードする方法に関する関連手順を参照してください。
5. 次に、パートナーセンターに新しい送信を [更新](update-a-flight-submission.md) して [コミット](commit-a-flight-submission.md) します。
6. 最後に、申請が正常にコミットされるまで、定期的に[新しい申請の状態をチェック](get-status-for-a-flight-submission.md)します。

:::code language="python" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/python/Examples.py" range="251-325":::

## <a name="related-topics"></a>関連トピック

* [Microsoft Store サービスを使用した申請の作成と管理](create-and-manage-submissions-using-windows-store-services.md)
