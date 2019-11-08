---
ms.assetid: 2A454057-FF14-40D2-8ED2-CEB5F27E0226
description: Microsoft Store 送信 API でこれらのメソッドを使用して、パートナーセンターアカウントに登録されているアプリのパッケージフライトを管理します。
title: パッケージフライトの管理
ms.date: 04/16/2018
ms.topic: article
keywords: windows 10、uwp、Microsoft Store 送信 API、フライト送信
ms.localizationpriority: medium
ms.openlocfilehash: 50596fdadae2ac4a0625687e7c8acaf985ccfaa7
ms.sourcegitcommit: f561efbda5c1d47b85601d91d70d86c5332bbf8c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/21/2019
ms.locfileid: "72690372"
---
# <a name="manage-package-flight-submissions"></a>パッケージフライトの管理

Microsoft Store 送信 API には、段階的なパッケージの展開など、アプリのパッケージフライトの管理に使用できるメソッドが用意されています。 API を使用するための前提条件など、Microsoft Store 送信 API の概要については、「 [Microsoft Store services を使用した送信の作成と管理](create-and-manage-submissions-using-windows-store-services.md)」を参照してください。

> [!IMPORTANT]
> Microsoft Store 送信 API を使用してパッケージフライトの送信を作成する場合は、パートナーセンターではなく、API を使用してのみ、送信に対してさらに変更を加えるようにしてください。 API を使用して最初に作成した送信をダッシュボードを使用して変更した場合、API を使用してその送信を変更またはコミットすることはできなくなります。 場合によっては、送信がエラー状態のままになり、送信プロセスを続行できないことがあります。 この問題が発生した場合は、送信を削除して新しい送信を作成する必要があります。

<span id="methods-for-package-flight-submissions" />

## <a name="methods-for-managing-package-flight-submissions"></a>パッケージフライトの送信を管理する方法

次のメソッドを使用して、パッケージのフライト送信を取得、作成、更新、コミット、または削除します。 これらの方法を使用する前に、パッケージのフライトがパートナーセンターに既に存在している必要があります。 パッケージフライトは、[パートナーセンターで](https://docs.microsoft.com/windows/uwp/publish/package-flights)作成することも、「[パッケージフライトの管理](manage-flights.md)」で説明されている Microsoft Store 送信 API 方法を使用して作成することもできます。

<table>
<colgroup>
<col width="10%" />
<col width="30%" />
<col width="60%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">メソッド</th>
<th align="left">URI</th>
<th align="left">説明</th>
</tr>
</thead>
<tbody>
<tr>
<td align="left">GET</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}</td>
<td align="left"><a href="get-a-flight-submission.md">既存のパッケージのフライト送信を取得する</a></td>
</tr>
<tr>
<td align="left">GET</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/status</td>
<td align="left"><a href="get-status-for-a-flight-submission.md">既存のパッケージのフライト送信の状態を取得する</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions</td>
<td align="left"><a href="create-a-flight-submission.md">新しいパッケージのフライト送信を作成する</a></td>
</tr>
<tr>
<td align="left">PUT</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}</td>
<td align="left"><a href="update-a-flight-submission.md">既存のパッケージのフライト送信を更新する</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/commit</td>
<td align="left"><a href="commit-a-flight-submission.md">新規または更新されたパッケージのフライト送信をコミットする</a></td>
</tr>
<tr>
<td align="left">DELETE</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}</td>
<td align="left"><a href="delete-a-flight-submission.md">パッケージのフライト送信を削除する</a></td>
</tr>
</tbody>
</table>

<span id="create-a-package-flight-submission">

## <a name="create-a-package-flight-submission"></a>パッケージのフライト送信の作成

パッケージフライトの送信を作成するには、次の手順に従います。

1. まだ行っていない場合は、「 [Microsoft Store サービスを使用した送信の作成と管理](create-and-manage-submissions-using-windows-store-services.md)」で説明されている前提条件を完了します。たとえば、Azure AD アプリケーションをパートナーセンターアカウントに関連付け、クライアント ID とキーを取得します。 これは1回だけ行う必要があります。クライアント ID とキーを取得した後は、新しい Azure AD アクセストークンを作成する必要があるときはいつでも再利用できます。  

2. [Azure AD アクセストークンを取得](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token)します。 このアクセストークンは、Microsoft Store 送信 API のメソッドに渡す必要があります。 アクセストークンを取得した後、有効期限が切れるまでに60分かかります。 トークンの有効期限が切れると、新しいトークンを取得できます。

3. Microsoft Store 送信 API で次のメソッドを実行して、[パッケージのフライト送信を作成](create-a-flight-submission.md)します。 このメソッドは、最後に発行された送信のコピーである、進行中の新しい送信を作成します。

    ```json
    POST https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions
    ```

    応答本文[には、](#flight-submission-object)新しい送信の ID、Azure Blob storage に送信するパッケージをアップロードするための shared access SIGNATURE (SAS) URI、および新しい送信のデータ (すべてを含む) が含まれています。一覧と価格情報)。

    > [!NOTE]
    > SAS URI を使用すると、アカウントキーを必要とせずに、Azure storage 内のセキュリティで保護されたリソースにアクセスできます。 SAS Uri と Azure Blob storage での使用に関する背景情報については、「 [Shared Access signature、第1部: sas モデル](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/)と共有アクセス署名について」 [、「パート 2: Blob ストレージでの sas の作成と使用](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-2/)」を参照してください。

4. 送信用に新しいパッケージを追加する場合は、[パッケージを準備](https://docs.microsoft.com/windows/uwp/publish/app-package-requirements)し、ZIP アーカイブに追加します。

5. 新しい送信に必要な変更を行って[フライト送信](#flight-submission-object)データを修正し、次のメソッドを実行して[、パッケージのフライト送信を更新](update-a-flight-submission.md)します。

    ```json
    PUT https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}
    ```
      > [!NOTE]
      > 送信用に新しいパッケージを追加する場合は、必ず、ZIP アーカイブ内のこれらのファイルの名前と相対パスを参照するように送信データを更新してください。

4. 送信用の新しいパッケージを追加する場合は、前に呼び出した POST メソッドの応答本文に指定された SAS URI を使用して、ZIP アーカイブを[Azure Blob storage](https://docs.microsoft.com/azure/storage/storage-introduction#blob-storage)にアップロードします。 さまざまなプラットフォームでこれを実行するために使用できるさまざまな Azure ライブラリがあります。これには、次のものが含まれます。

    * [.NET 用 Azure Storage クライアント ライブラリ](https://docs.microsoft.com/azure/storage/storage-dotnet-how-to-use-blobs)
    * [Azure Storage SDK for Java](https://docs.microsoft.com/azure/storage/storage-java-how-to-use-blob-storage)
    * [Azure Storage SDK for Python](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-blob-storage)

    次C#のコード例では、.net 用 Azure Storage クライアントライブラリの[cloudblockblob](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.blob.cloudblockblob)クラスを使用して、ZIP アーカイブを Azure Blob storage にアップロードする方法を示します。 この例では、ZIP アーカイブが既にストリームオブジェクトに書き込まれていることを前提としています。

    ```csharp
    string sasUrl = "https://productingestionbin1.blob.core.windows.net/ingestion/26920f66-b592-4439-9a9d-fb0f014902ec?sv=2014-02-14&sr=b&sig=usAN0kNFNnYE2tGQBI%2BARQWejX1Guiz7hdFtRhyK%2Bog%3D&se=2016-06-17T20:45:51Z&sp=rwl";
    Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob blockBob =
        new Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob(new System.Uri(sasUrl));
    await blockBob.UploadFromStreamAsync(stream);
    ```

5. 次のメソッドを実行して[、パッケージのフライト送信をコミットし](commit-a-flight-submission.md)ます。 これにより、送信が完了したことがパートナーセンターに通知され、更新プログラムがアカウントに適用されるようになります。

    ```json
    POST https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/commit
    ```

6. 次のメソッドを実行して、コミットの状態を確認し、[パッケージのフライト送信の状態を取得](get-status-for-a-flight-submission.md)します。

    ```json
    GET https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/status
    ```

    送信ステータスを確認するには、応答本文の*状態*の値を確認します。 この値は、要求が成功**した場合**は**commitstarted** 、要求にエラーがある場合は**commitstarted**のいずれかに変更されます。 エラーが発生した場合は、 *statusdetails*フィールドにエラーの詳細が表示されます。

7. コミットが正常に完了すると、送信がストアに送信され、インジェストされます。 前の方法を使用するか、パートナーセンターにアクセスして、送信の進行状況を引き続き監視することができます。

<span/>

## <a name="code-examples"></a>コード例

次の記事では、さまざまなプログラミング言語でパッケージのフライトを作成する方法を示す詳細なコード例を提供しています。

* [C#コード例](csharp-code-examples-for-the-windows-store-submission-api.md)
* [Java コードの例](java-code-examples-for-the-windows-store-submission-api.md)
* [Python のコード例](python-code-examples-for-the-windows-store-submission-api.md)

## <a name="storebroker-powershell-module"></a>StoreBroker PowerShell モジュール

Microsoft Store 送信 API を直接呼び出す代わりに、API の上にコマンドラインインターフェイスを実装するオープンソースの PowerShell モジュールも用意しています。 このモジュールは[Storebroker](https://aka.ms/storebroker)と呼ばれます。 このモジュールを使用して、Microsoft Store 送信 API を直接呼び出すのではなく、コマンドラインからアプリ、フライト、およびアドオンの送信を管理できます。または、ソースを参照して、この API を呼び出す方法の例を参照することもできます。 StoreBroker モジュールは、多くのファーストパーティアプリケーションがストアに送信される主な方法として、Microsoft 内で積極的に使用されます。

詳細については、 [GitHub の Storebroker ページ](https://aka.ms/storebroker)を参照してください。

<span id="manage-gradual-package-rollout">

## <a name="manage-a-gradual-package-rollout-for-a-package-flight-submission"></a>パッケージのフライトの段階的なロールアウトを管理する

パッケージフライト送信の更新されたパッケージは、Windows 10 でのアプリの顧客の割合に段階的にロールアウトできます。 これにより、特定のパッケージのフィードバックと分析データを監視して、更新が確実に実行されてから、より広範にロールアウトすることができます。 新しい送信を作成しなくても、公開された送信のロールアウトの割合 (または更新の停止) を変更できます。 パートナーセンターで段階的なパッケージのロールアウトを有効化および管理する手順など、詳細については、こちらの[記事](../publish/gradual-package-rollout.md)を参照してください。

パッケージのフライト送信に対して段階的なパッケージのロールアウトをプログラムで有効にするには、Microsoft Store 送信 API のメソッドを使用して、次の手順を実行します。

  1. [パッケージのフライト送信を作成する](create-a-flight-submission.md)か[、パッケージのフライト送信を取得](get-a-flight-submission.md)します。
  2. 応答データで、 [packageRollout](#package-rollout-object)リソースを見つけて、 *isPackageRollout*フィールドを true に設定し、 *packageRolloutPercentage*フィールドに、更新されたパッケージを取得する必要があるアプリの顧客の割合を設定します。
  3. 更新されたパッケージフライト送信データを、[パッケージの更新フライト送信](update-a-flight-submission.md)方法に渡します。

パッケージのフライト送信に対して段階的なパッケージのロールアウトを有効にした後、次の方法を使用して、段階的なロールアウトをプログラムで取得、更新、停止、または終了できます。

<table>
<colgroup>
<col width="10%" />
<col width="30%" />
<col width="60%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">メソッド</th>
<th align="left">URI</th>
<th align="left">説明</th>
</tr>
</thead>
<tbody>
<tr>
<td align="left">GET</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/packagerollout</td>
<td align="left"><a href="get-package-rollout-info-for-a-flight-submission.md">パッケージのフライト送信の段階的なロールアウト情報を取得する</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/updatepackagerolloutpercentage</td>
<td align="left"><a href="update-the-package-rollout-percentage-for-a-flight-submission.md">パッケージのフライト送信の段階的なロールアウト率を更新する</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/haltpackagerollout</td>
<td align="left"><a href="halt-the-package-rollout-for-a-flight-submission.md">パッケージのフライト送信の段階的なロールアウトを停止します</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/finalizepackagerollout</td>
<td align="left"><a href="finalize-the-package-rollout-for-a-flight-submission.md">パッケージのフライト送信の段階的なロールアウトの最終処理</a></td>
</tr>
</tbody>
</table>

<span/>

## <a name="data-resources"></a>データ リソース

パッケージフライトの送信を管理するための Microsoft Store 送信 API メソッドでは、次の JSON データリソースを使用します。

<span id="flight-submission-object" />

### <a name="flight-submission-resource"></a>フライト送信リソース

このリソースでは、パッケージのフライト送信について説明します。

```json
{
  "id": "1152921504621243649",
  "flightId": "cd2e368a-0da5-4026-9f34-0e7934bc6f23",
  "status": "PendingCommit",
  "statusDetails": {
    "errors": [],
    "warnings": [],
    "certificationReports": []
  },
  "flightPackages": [
    {
      "fileName": "newPackage.appx",
      "fileStatus": "PendingUpload",
      "id": "",
      "version": "1.0.0.0",
      "languages": ["en-us"],
      "capabilities": [],
      "minimumDirectXVersion": "None",
      "minimumSystemRam": "None"
    }
  ],
  "packageDeliveryOptions": {
    "packageRollout": {
        "isPackageRollout": false,
        "packageRolloutPercentage": 0.0,
        "packageRolloutStatus": "PackageRolloutNotStarted",
        "fallbackSubmissionId": "0"
    },
    "isMandatoryUpdate": false,
    "mandatoryUpdateEffectiveDate": "1601-01-01T00:00:00.0000000Z"
  },
  "fileUploadUrl": "https://productingestionbin1.blob.core.windows.net/ingestion/8b389577-5d5e-4cbe-a744-1ff2e97a9eb8?sv=2014-02-14&sr=b&sig=wgMCQPjPDkuuxNLkeG35rfHaMToebCxBNMPw7WABdXU%3D&se=2016-06-17T21:29:44Z&sp=rwl",
  "targetPublishMode": "Immediate",
  "targetPublishDate": "",
  "notesForCertification": "No special steps are required for certification of this app."
}
```

このリソースの値は次のとおりです。

| 値      | 種類   | 説明              |
|------------|--------|------------------------------|
| id            | string  | 送信の ID。  |
| flightId           | string  |  送信が関連付けられているパッケージフライトの ID。  |  
| status           | string  | 送信のステータス。 次のいずれかの値を指定できます。 <ul><li>なし</li><li>Canceled</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>発行</li><li>公開先</li><li>PublishFailed</li><li>前処理</li><li>PreProcessingFailed</li><li>認定</li><li>CertificationFailed</li><li>Release</li><li>ReleaseFailed</li></ul>   |
| statusDetails           | object  |  [ステータスの詳細リソース](#status-details-object)。エラーに関する情報を含め、送信の状態に関する追加情報が含まれています。  |
| フライトパッケージ           | array  | 送信の各パッケージに関する詳細情報を提供する[フライトパッケージリソース](#flight-package-object)が含まれています。   |
| packageDeliveryOptions    | object  | パッケージ[配布オプションのリソース](#package-delivery-options-object)。このリソースには、パッケージの段階的なロールアウトと、送信の必須の更新設定が含まれています。   |
| fileUploadUrl           | string  | 送信用のパッケージをアップロードするための共有アクセス署名 (SAS) URI。 送信用に新しいパッケージを追加する場合は、パッケージを含む ZIP アーカイブをこの URI にアップロードします。 詳細については、「[フライトパッケージの作成](#create-a-package-flight-submission)」を参照してください。  |
| targetPublishMode           | string  | 送信の発行モード。 次のいずれかの値を指定できます。 <ul><li>即時</li><li>マニュアル</li><li>固有の日付</li></ul> |
| targetPublishDate           | string  | *Targetpublishmode*が固有の日付に設定されている場合は、ISO 8601 形式で送信される発行日。  |
| 証明書           | string  |  テストアカウントの資格情報や、機能にアクセスして検証するための手順など、認定テスト担当者向けの追加情報を提供します。 詳細については、「[認定のメモ](https://docs.microsoft.com/windows/uwp/publish/notes-for-certification)」を参照してください。 |

<span id="status-details-object" />

### <a name="status-details-resource"></a>状態の詳細リソース

このリソースには、送信の状態に関する追加情報が含まれています。 このリソースの値は次のとおりです。

| 値           | 種類    | 説明                   |
|-----------------|---------|------|
|  errors               |    object     |   送信のエラーの詳細を含む[ステータス詳細リソース](#status-detail-object)の配列。   |     
|  付               |   object      | 送信の警告の詳細を含む[ステータス詳細リソース](#status-detail-object)の配列。     |
|  certificationReports               |     object    |   送信用の証明書レポートデータへのアクセスを提供する、[証明書レポートリソース](#certification-report-object)の配列。 認定に失敗した場合は、これらのレポートで詳細を確認できます。    |  


<span id="status-detail-object" />

### <a name="status-detail-resource"></a>ステータスの詳細リソース

このリソースには、送信に関連するエラーまたは警告に関する追加情報が含まれています。 このリソースの値は次のとおりです。

| 値           | 種類    | 説明       |
|-----------------|---------|------|
|  code               |    string     |   エラーまたは警告の種類を説明する[送信ステータスコード](#submission-status-code)。 |  
|  詳細               |     string    |  問題に関する詳細情報を含むメッセージ。     |


<span id="certification-report-object" />

### <a name="certification-report-resource"></a>証明書レポートリソース

このリソースは、送信用の証明書レポートデータへのアクセスを提供します。 このリソースの値は次のとおりです。

| 値           | 種類    | 説明         |
|-----------------|---------|------|
|     date            |    string     |  レポートが生成された日付と時刻 (ISO 8601 形式)。    |
|     reportUrl            |    string     |  レポートにアクセスできる URL。    |


<span id="flight-package-object" />

### <a name="flight-package-resource"></a>フライトパッケージリソース

このリソースは、送信内のパッケージに関する詳細を提供します。

```json
{
  "flightPackages": [
    {
      "fileName": "newPackage.appx",
      "fileStatus": "PendingUpload",
      "id": "",
      "version": "1.0.0.0",
      "languages": ["en-us"],
      "capabilities": [],
      "minimumDirectXVersion": "None",
      "minimumSystemRam": "None"
    }
  ],
}
```

このリソースの値は次のとおりです。

> [!NOTE]
> [パッケージの[更新] フライト送信](update-a-flight-submission.md)方法を呼び出すとき、要求本文には、このオブジェクトの*fileName*、 *filestatus*、 *Minimumdirectxversion*、および*minimumsystemram*の値のみが必要です。 その他の値は、パートナーセンターによって設定されます。

| 値           | 種類    | 説明              |
|-----------------|---------|------|
| fileName   |   string      |  パッケージの名前です。    |  
| fileStatus    | string    |  パッケージの状態です。 次のいずれかの値を指定できます。 <ul><li>なし</li><li>PendingUpload</li><li>アップロード完了</li><li>PendingDelete</li></ul>    |  
| id    |  string   |  パッケージを一意に識別する ID。 この値は、パートナーセンターによって使用されます。   |     
| version    |  string   |  アプリケーションパッケージのバージョン。 詳細については、「[パッケージバージョン番号](https://docs.microsoft.com/windows/uwp/publish/package-version-numbering)」を参照してください。   |   
| アーキテクチャ    |  string   |  アプリケーションパッケージのアーキテクチャ (ARM など)。   |     
| languages    | array    |  アプリがサポートしている言語の言語コードの配列。 詳細については、「[サポートされる言語](https://docs.microsoft.com/windows/uwp/publish/supported-languages)」を参照してください。    |     
| capabilities    |  array   |  パッケージに必要な機能の配列です。 機能の詳細については、「[アプリ機能の宣言](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations)」を参照してください。   |     
| minimumDirectXVersion    |  string   |  アプリパッケージでサポートされている DirectX の最小バージョン。 これは、Windows 8.x を対象とするアプリに対してのみ設定できます。他のバージョンを対象とするアプリでは無視されます。 次のいずれかの値を指定できます。 <ul><li>なし</li><li>DirectX93</li><li>DirectX100</li></ul>   |     
| minimumSystemRam    | string    |  アプリパッケージで必要とされる最小 RAM。 これは、Windows 8.x を対象とするアプリに対してのみ設定できます。他のバージョンを対象とするアプリでは無視されます。 次のいずれかの値を指定できます。 <ul><li>なし</li><li>Memory2GB</li></ul>   |    


<span id="package-delivery-options-object" />

### <a name="package-delivery-options-resource"></a>パッケージ配信オプションのリソース

このリソースには、パッケージの段階的なロールアウトと、送信用の必須の更新設定が含まれています。

```json
{
  "packageDeliveryOptions": {
    "packageRollout": {
        "isPackageRollout": false,
        "packageRolloutPercentage": 0.0,
        "packageRolloutStatus": "PackageRolloutNotStarted",
        "fallbackSubmissionId": "0"
    },
    "isMandatoryUpdate": false,
    "mandatoryUpdateEffectiveDate": "1601-01-01T00:00:00.0000000Z"
  },
}
```

このリソースの値は次のとおりです。

| 値           | 種類    | 説明        |
|-----------------|---------|------|
| packageRollout   |   object      |   送信用の段階的なパッケージロールアウト設定を含む[パッケージロールアウトリソース](#package-rollout-object)。    |  
| isMandatoryUpdate    | boolean    |  この送信のパッケージを、自己インストールアプリの更新に必須として扱うかどうかを示します。 アプリの更新プログラムを自己インストールするための必須パッケージの詳細については、「[アプリのパッケージ更新プログラムのダウンロードとインストール](../packaging/self-install-package-updates.md)」を参照してください。    |  
| mandatoryUpdateEffectiveDate    |  date   |  ISO 8601 形式および UTC タイムゾーンで、この送信のパッケージが必須になる日時。   |        

<span id="package-rollout-object" />

### <a name="package-rollout-resource"></a>パッケージロールアウトリソース

このリソースには、送信用の段階的な[パッケージロールアウト設定](#manage-gradual-package-rollout)が含まれています。 このリソースの値は次のとおりです。

| 値           | 種類    | 説明        |
|-----------------|---------|------|
| isPackageRollout   |   boolean      |  送信に対して段階的なパッケージのロールアウトを有効にするかどうかを示します。    |  
| packageRolloutPercentage    | float    |  段階的なロールアウトでパッケージを受信するユーザーの割合。    |  
| packageRolloutStatus    |  string   |  段階的なパッケージのロールアウトのステータスを示す次のいずれかの文字列。 <ul><li>PackageRolloutNotStarted</li><li>PackageRolloutInProgress</li><li>PackageRolloutComplete</li><li>PackageRolloutStopped</li></ul>  |  
| Fallback提出 Id    |  string   |  段階的なロールアウトパッケージを取得しない顧客によって受信される送信の ID。   |          

> [!NOTE]
> *PackageRolloutStatus*と*fallbackの Id*値はパートナーセンターによって割り当てられ、開発者が設定するものではありません。 これらの値を要求本文に含めると、これらの値は無視されます。

<span/>

## <a name="enums"></a>列挙型

これらのメソッドは、次の列挙型を使用します。

<span id="submission-status-code" />

### <a name="submission-status-code"></a>送信状態コード

次のコードは、送信の状態を表します。

| コード           |  説明      |
|-----------------|---------------|
|  なし            |     コードが指定されていません。         |     
|      InvalidArchive        |     パッケージが含まれている ZIP アーカイブが無効であるか、認識できないアーカイブ形式です。  |
| MissingFiles | ZIP アーカイブには、送信データに一覧表示されたすべてのファイルが含まれていないか、アーカイブ内の間違った場所にあります。 |
| PackageValidationFailed | 送信内の1つ以上のパッケージを検証できませんでした。 |
| InvalidParameterValue | 要求本文のパラメーターの1つが無効です。 |
| InvalidOperation | 操作を実行しようとしましたが無効です。 |
| InvalidState | 実行しようとした操作は、パッケージフライトの現在の状態に対して無効です。 |
| ResourceNotFound | 指定されたパッケージフライトが見つかりませんでした。 |
| ServiceError | 内部サービスエラーが発生したため、要求が成功しませんでした。 要求を再試行してください。 |
| ListingOptOutWarning | 開発者が前回の送信から一覧を削除したか、パッケージでサポートされているリスティング情報が含まれていませんでした。 |
| ListingOptInWarning  | 開発者がリストを追加しました。 |
| UpdateOnlyWarning | 開発者は、更新プログラムのサポートのみを含むものを挿入しようとしています。 |
| その他  | 送信が認識されていないか、カテゴリ化されていない状態です。 |
| PackageValidationWarning | パッケージの検証プロセスで警告が発生しました。 |

<span/>

## <a name="related-topics"></a>関連トピック

* [Microsoft Store services を使用した送信の作成と管理](create-and-manage-submissions-using-windows-store-services.md)
* [Microsoft Store 送信 API を使用してパッケージフライトを管理する](manage-flights.md)
* [パッケージのフライト送信を取得する](get-a-flight-submission.md)
* [パッケージのフライト送信の作成](create-a-flight-submission.md)
* [パッケージのフライト送信を更新する](update-a-flight-submission.md)
* [パッケージのフライト送信をコミットする](commit-a-flight-submission.md)
* [パッケージのフライト送信を削除する](delete-a-flight-submission.md)
* [パッケージのフライト送信の状態を取得する](get-status-for-a-flight-submission.md)
