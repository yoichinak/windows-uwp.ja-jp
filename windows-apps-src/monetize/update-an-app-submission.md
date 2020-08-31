---
ms.assetid: E8751EBF-AE0F-4107-80A1-23C186453B1C
description: 既存のアプリの申請を更新するには、Microsoft Store 申請 API の以下のメソッドを使います。
title: アプリの申請の更新
ms.date: 04/17/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store 申請 API, アプリの申請, 更新
ms.localizationpriority: medium
ms.openlocfilehash: 393b7c48723409dfc630c6f85770c4c0f7dcd1a4
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89158366"
---
# <a name="update-an-app-submission"></a>アプリの申請の更新

既存のアプリの申請を更新するには、Microsoft Store 申請 API の以下のメソッドを使います。 このメソッドを使って申請を正常に更新した後は、インジェストと公開のために[申請をコミット](commit-an-app-submission.md)する必要があります。

このメソッドが Microsoft Store 申請 API を使ったアプリの申請の作成プロセスにどのように適合するかについては、「[アプリの申請の管理](manage-app-submissions.md)」をご覧ください。

## <a name="prerequisites"></a>前提条件

このメソッドを使うには、最初に次の作業を行う必要があります。

* Microsoft Store 申請 API に関するすべての[前提条件](create-and-manage-submissions-using-windows-store-services.md#prerequisites)を満たします (前提条件がまだ満たされていない場合)。
* このメソッドの要求ヘッダーで使う [Azure AD アクセス トークンを取得](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token)します。 アクセス トークンを取得した後、アクセス トークンを使用できるのは、その有効期限が切れるまでの 60 分間です。 トークンの有効期限が切れたら新しいトークンを取得できます。
* いずれかのアプリの送信を作成します。 これはパートナーセンターで行うことができます。または、[ [アプリの作成] 送信](create-an-app-submission.md) 方法を使用して行うこともできます。

## <a name="request"></a>要求

このメソッドの構文は次のとおりです。 ヘッダーと要求本文の使用例と説明については、次のセクションをご覧ください。

| 認証方法 | 要求 URI                                                      |
|--------|------------------------------------------------------------------|
| PUT   | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}  ``` |


### <a name="request-header"></a>要求ヘッダー

| Header        | Type   | 説明                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 承認 | string | 必須。 **Bearer** &lt;*トークン*&gt; という形式の Azure AD アクセス トークン。 |


### <a name="request-parameters"></a>要求パラメーター

| 名前        | Type   | 説明                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | string | 必須。 申請を更新するアプリのストア ID です。 ストア ID について詳しくは、「[アプリ ID の詳細の表示](../publish/view-app-identity-details.md)」をご覧ください。  |
| submissionId | string | 必須。 更新する申請の ID です。 この ID は、[アプリの申請の作成](create-an-app-submission.md)要求に対する応答データで確認できます。 パートナーセンターで作成された送信の場合、この ID はパートナーセンターの [送信] ページの URL でも利用できます。  |


### <a name="request-body"></a>要求本文

要求本文には次のパラメーターがあります。

| 値      | Type   | 説明                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| applicationCategory           | string  |   アプリの[カテゴリとサブカテゴリ](../publish/category-and-subcategory-table.md)を指定する文字列です。 カテゴリとサブカテゴリは、アンダースコア "_" で 1 つの文字列に連結します (例: **BooksAndReference_EReader**)。      |  
| 価格           |  object  | アプリの価格情報を含むオブジェクトです。 詳しくは、「[価格リソース](manage-app-submissions.md#pricing-object)」セクションをご覧ください。       |   
| 参照可能範囲           |  string  |  アプリの可視性です。 次のいずれかの値を指定できます。 <ul><li>[非表示]</li><li>パブリック</li><li>プライベート</li><li>NotSet</li></ul>       |   
| targetPublishMode           | string  | 申請の公開モードです。 次のいずれかの値を指定できます。 <ul><li>即時</li><li>マニュアル</li><li>SpecificDate</li></ul> |
| targetPublishDate           | string  | *targetPublishMode* が SpecificDate に設定されている場合、ISO 8601 形式での申請の公開日です。  |  
| listings           |   object  |  キーと値のペアのディクショナリです。各キーは国コード、各値はアプリの登録情報を含む[登録情報リソース](manage-app-submissions.md#listing-object) オブジェクトです。       |   
| hardwarePreferences           |  array  |   アプリの[ハードウェアの基本設定](../publish/enter-app-properties.md)を定義する文字列の配列です。 次のいずれかの値を指定できます。 <ul><li>Touch</li><li>キーボード</li><li>マウス</li><li>カメラ</li><li>NfcHce</li><li>Nfc</li><li>BluetoothLE</li><li>テレフォニー</li></ul>     |   
| automaticBackupEnabled           |  boolean  |   OneDrive への自動バックアップにアプリのデータを含めることができるかどうかを示します。 詳しくは、「[アプリの宣言](../publish/product-declarations.md)」をご覧ください。   |   
| canInstallOnRemovableMedia           |  boolean  |   ユーザーがアプリをリムーバブル記憶域にインストールできるかどうかを示します。 詳しくは、「[アプリの宣言](../publish/product-declarations.md)」をご覧ください。     |   
| isGameDvrEnabled           |  boolean |   アプリのゲーム録画が有効になっているかどうかを示します。    |   
| gamingOptions           |  object |   アプリのゲーム関連の設定を定義する 1 つの[ゲーム オプション リソース](manage-app-submissions.md#gaming-options-object)を格納する配列です。     |   
| hasExternalInAppProducts           |     boolean          |   ユーザーが Microsoft Store コマース システムを使わないで購入することをアプリが許可するかどうかを示します。 詳しくは、「[アプリの宣言](../publish/product-declarations.md)」をご覧ください。     |   
| meetAccessibilityGuidelines           |    boolean           |  アプリがアクセシビリティ ガイドラインを満たことをテストされているかどうかを示します。 詳しくは、「[アプリの宣言](../publish/product-declarations.md)」をご覧ください。      |   
| notesForCertification           |  string  |   アプリの[認定の注意書き](../publish/notes-for-certification.md)が含まれます。    |    
| applicationPackages           |   array  | 申請の各パッケージに関する詳細を提供するオブジェクトが含まれています。 詳しくは、「[アプリ パッケージ](manage-app-submissions.md#application-package-object)」セクションをご覧ください。 このメソッドを呼び出してアプリの申請を更新するとき、要求の本文では、これらのオブジェクトの値 *fileName*、*fileStatus*、*minimumDirectXVersion*、*minimumSystemRam* だけが必須です。 その他の値は、パートナーセンターによって設定されます。   |    
| packageDeliveryOptions    | object  | 申請の段階的なパッケージのロールアウトと必須の更新の設定が含まれています。 詳しくは、「[パッケージの配信オプション オブジェクト](manage-app-submissions.md#package-delivery-options-object)」をご覧ください。  |
| enterpriseLicensing           |  string  |  アプリのエンタープライズ ライセンス動作を示す[エンタープライズ ライセンス値](manage-app-submissions.md#enterprise-licensing)のいずれかです。  |    
| allowMicrosftDecideAppAvailabilityToFutureDeviceFamilies           |  boolean   |  [アプリを将来の Windows 10 デバイス ファミリで利用できるようにする](../publish/set-app-pricing-and-availability.md)ことを Microsoft が許可されているかどうかを示すします。    |    
| allowTargetFutureDeviceFamilies           | boolean   |  [将来の Windows 10 デバイス ファミリをターゲットにする](../publish/set-app-pricing-and-availability.md)ことをアプリが許可されているかどうかを示します。     |   
| trailers           |  array |   アプリの登録情報用のビデオ トレーラーを表す[トレーラー リソース](manage-app-submissions.md#trailer-object)を格納する配列です。格納できるトレーラー リソースの数には上限があります。   |   


### <a name="request-example"></a>要求の例

次の例は、アプリの申請を更新する方法を示しています。

```json
PUT https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/submissions/1152921504621230023 HTTP/1.1
Authorization: Bearer <your access token>
Content-Type: application/json
{
  "applicationCategory": "BooksAndReference_EReader",
  "pricing": {
    "trialPeriod": "FifteenDays",
    "marketSpecificPricings": {},
    "sales": [],
    "priceId": "Tier2"
  },
  "visibility": "Public",
  "targetPublishMode": "Manual",
  "targetPublishDate": "1601-01-01T00:00:00Z",
  "listings": {
    "en-us": {
      "baseListing": {
        "copyrightAndTrademarkInfo": "",
        "keywords": [
              "epub"
            ],
        "licenseTerms": "",
        "privacyPolicy": "",
        "supportContact": "",
        "websiteUrl": "",
        "description": "Description",
        "features": [
              "Free ebook reader"
            ],
        "releaseNotes": "",
        "images": [
          {
            "fileName": "contoso.png",
            "fileStatus": "Uploaded",
            "id": "1152921504672272757",
            "imageType": "Screenshot"
          }
        ],
        "recommendedHardware": [],
        "title": "Contoso ebook reader"
      },
      "platformOverrides": {
        "Windows81": {
          "description": "Ebook reader for Windows 8.1"
        }
      }
    }
  },
  "hardwarePreferences": [
    "Touch"
  ],
  "automaticBackupEnabled": false,
  "canInstallOnRemovableMedia": true,
  "isGameDvrEnabled": false,
  "gamingOptions": [],
  "hasExternalInAppProducts": false,
  "meetAccessibilityGuidelines": true,
  "notesForCertification": "",
  "applicationPackages": [
    {
      "fileName": "contoso_app.appx",
      "fileStatus": "PendingUpload",
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
  "enterpriseLicensing": "Online",
  "allowMicrosoftDecideAppAvailabilityToFutureDeviceFamilies": true,
  "allowTargetFutureDeviceFamilies": {
    "Desktop": false,
    "Mobile": true,
    "Holographic": true,
    "Xbox": false,
    "Team": true
  },
  "trailers": []
}
```

## <a name="response"></a>応答

次の例は、このメソッドが正常に呼び出された場合の JSON 応答本文を示しています。 応答本文には、更新された申請に関する情報が含まれています。 応答本文内の値について詳しくは、[アプリの申請のリソース](manage-app-submissions.md#app-submission-object)をご覧ください。

```json
{
  "id": "1152921504621243540",
  "applicationCategory": "BooksAndReference_EReader",
  "pricing": {
    "trialPeriod": "FifteenDays",
    "marketSpecificPricings": {},
    "sales": [],
    "priceId": "Tier2"
  },
  "visibility": "Public",
  "targetPublishMode": "Manual",
  "targetPublishDate": "1601-01-01T00:00:00Z",
  "listings": {
    "en-us": {
      "baseListing": {
        "copyrightAndTrademarkInfo": "",
        "keywords": [
           "epub"
        ],
        "licenseTerms": "",
        "privacyPolicy": "",
        "supportContact": "",
        "websiteUrl": "",
        "description": "Description",
        "features": [
          "Free ebook reader"
        ],
        "releaseNotes": "",
        "images": [
          {
            "fileName": "contoso.png",
            "fileStatus": "Uploaded",
            "id": "1152921504672272757",
            "imageType": "Screenshot"
          }
        ],
        "recommendedHardware": [],
        "title": "Contoso ebook reader"
      },
      "platformOverrides": {
        "Windows81": {
          "description": "Ebook reader for Windows 8.1",
        }
      }
    }
  },
  "hardwarePreferences": [
    "Touch"
  ],
  "automaticBackupEnabled": false,
  "canInstallOnRemovableMedia": true,
  "isGameDvrEnabled": false,
  "gamingOptions": [],
  "hasExternalInAppProducts": false,
  "meetAccessibilityGuidelines": true,
  "notesForCertification": "",
  "status": "PendingCommit",
  "statusDetails": {
    "errors": [],
    "warnings": [],
    "certificationReports": []
  },
  "fileUploadUrl": "https://productingestionbin1.blob.core.windows.net/ingestion/387a9ea8-a412-43a9-8fb3-a38d03eb483d?sv=2014-02-14&sr=b&sig=sdd12JmoaT6BhvC%2BZUrwRweA%2Fkvj%2BEBCY09C2SZZowg%3D&se=2016-06-17T18:32:26Z&sp=rwl",
  "applicationPackages": [
    {
      "fileName": "contoso_app.appx",
      "fileStatus": "PendingUpload",
      "id": "1152921504620138797",
      "version": "1.0.0.0",
      "architecture": "ARM",
      "languages": [
        "en-US"
      ],
      "capabilities": [
        "ID_RESOLUTION_HD720P",
        "ID_RESOLUTION_WVGA",
        "ID_RESOLUTION_WXGA"
      ],
      "minimumDirectXVersion": "None",
      "minimumSystemRam": "None",
      "targetDeviceFamilies": [
        "Windows.Mobile min version 10.0.10240.0"
      ]
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
  "enterpriseLicensing": "Online",
  "allowMicrosoftDecideAppAvailabilityToFutureDeviceFamilies": true,
  "allowTargetFutureDeviceFamilies": {
    "Desktop": false,
    "Mobile": true,
    "Holographic": true,
    "Xbox": false,
    "Team": true
  },
  "friendlyName": "Submission 2",
  "trailers": []
}
```

## <a name="error-codes"></a>エラー コード

要求を正常に完了できない場合、次の HTTP エラー コードのいずれかが応答に含まれます。

| エラー コード |  説明   |
|--------|------------------|
| 400  | 要求が無効なため、申請を更新できませんでした。 |
| 409  | アプリの現在の状態により、送信を更新できませんでした。または、アプリが現在 [Microsoft Store 送信 API でサポート](create-and-manage-submissions-using-windows-store-services.md#not_supported)されていないパートナーセンターの機能を使用しています。 |   


## <a name="related-topics"></a>関連トピック

* [Microsoft Store サービスを使用した申請の作成と管理](create-and-manage-submissions-using-windows-store-services.md)
* [アプリの申請の取得](get-an-app-submission.md)
* [アプリの申請の作成](create-an-app-submission.md)
* [アプリの申請のコミット](commit-an-app-submission.md)
* [アプリの申請の削除](delete-an-app-submission.md)
* [アプリの申請の状態の取得](get-status-for-an-app-submission.md)