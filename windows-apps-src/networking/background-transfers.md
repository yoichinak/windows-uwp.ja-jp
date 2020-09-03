---
description: ネットワーク経由でファイルを確実にコピーするには、バックグラウンド転送 API を使います。
title: バックグラウンド転送
ms.assetid: 1207B089-BC16-4BF0-BBD4-FD99950C764B
ms.date: 03/23/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: faf88751f5008c5a819bb39bb461a4224180edf2
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363015"
---
# <a name="background-transfers"></a>バックグラウンド転送
ネットワーク経由でファイルを確実にコピーするには、バックグラウンド転送 API を使います。 バックグラウンド転送 API には、アプリの一時停止中はバックグラウンドで実行され、アプリの終了後も実行が続行される高度なアップロード機能とダウンロード機能があります。 この API は、ネットワークの状態を監視し、接続が失われたときに転送の中断と再開を自動的に実行します。転送ではデータ センサーとバッテリー セーバーにも対応し、ダウンロード アクティビティは現在の接続とデバイスのバッテリー状態に基づいて調整されます。 この API は、アップロード HTTP(S) を使った大きなファイルのアップロードとダウンロードに適しています。 FTP もサポートされますが、その対象はダウンロードのみです。

バックグラウンド転送はアプリの呼び出しとは別に実行され、主にビデオ、音楽、サイズの大きい画像などのリソースの長期の転送操作を目的としています。 これらのシナリオでは、アプリが一時停止状態でもダウンロードが続行されるため、バックグラウンド転送の使用が不可欠です。

すぐに完了する可能性がある小さいリソースをダウンロードする場合は、バックグラウンド転送ではなく [**HttpClient**](/uwp/api/Windows.Web.Http.HttpClient) API を使ってください。

## <a name="using-windowsnetworkingbackgroundtransfer"></a>Windows.Networking.BackgroundTransfer を使う

### <a name="how-does-the-background-transfer-feature-work"></a>バックグラウンド転送機能はどのように動作するか
アプリがバックグラウンド転送を使って転送を開始するときは、[**BackgroundDownloader**](/uwp/api/Windows.Networking.BackgroundTransfer.BackgroundDownloader) または [**BackgroundUploader**](/uwp/api/Windows.Networking.BackgroundTransfer.BackgroundUploader) クラス オブジェクトを使って要求が構成され初期化されます。 それぞれの転送操作は、呼び出し元アプリとは別にシステムによって個別に処理されます。 進行情報はアプリの UI でユーザーに状況を示す場合に利用することができ、アプリで一時停止、再開、キャンセルしたり、転送中にデータを読み取ったりすることができます。 システムによって転送が処理される方法により、スマートな電力消費が実現し、アプリの中断や終了、突然のネットワーク ステータス変化などのイベントが接続アプリで発生したときに起こる可能性のある問題を回避できます。

> [!NOTE]
> アプリごとのリソースの制約により、常にアプリに 200 を超える転送 (DownloadOperations および UploadOperations) を含めてはなりません。 この制限を超過すると、アプリの転送キューが回復不能な状態になる可能性があります。

アプリケーションの起動時に、既存のすべての [**DownloadOperation**](/uwp/api/windows.networking.backgroundtransfer.downloadoperation) および [**UploadOperation**](/uwp/api/windows.networking.backgroundtransfer.uploadoperation) オブジェクトに対して [**AttachAsync**](/uwp/api/windows.networking.backgroundtransfer.downloadoperation.AttachAsync) を呼び出す必要があります。 これを行わないと、すでに完了している転送のリークが発生し、最終的にバックグラウンド転送機能が使用できなくなります。

### <a name="performing-authenticated-file-requests-with-background-transfer"></a>バックグラウンド転送での認証されたファイル要求の実行
バックグラウンド転送では、基本サーバーとプロキシの資格情報、Cookie をサポートするメソッドが用意されており、それぞれの転送操作で ([**SetRequestHeader**](/uwp/api/windows.networking.backgroundtransfer.backgrounduploader.setrequestheader) を介して) カスタム HTTP ヘッダーを使うこともできます。

### <a name="how-does-this-feature-adapt-to-network-status-changes-or-unexpected-shutdowns"></a>この機能ではネットワーク ステータスの変化や予期しないシャットダウンにどのように対応するか
バックグラウンド転送機能により、ネットワークの状態が変化したときに各転送操作に対して一貫性のあるエクスペリエンスが保たれます。これは、[接続](/previous-versions/windows/apps/hh452990(v=win.10)) 機能によって提供される接続とキャリアのデータ プラン ステータスの情報をインテリジェントに利用しています。 さまざまなネットワーク シナリオでの動作を定義するために、アプリは、[**BackgroundTransferCostPolicy**](/uwp/api/Windows.Networking.BackgroundTransfer.BackgroundTransferCostPolicy) によって定義された値を使って、各転送操作のコスト ポリシーを設定します。

たとえば、操作に対して定義されたコスト ポリシーで、デバイスが従量制課金接続を使っているときに操作を自動的に一時停止する必要があることを示すことができます。 "制限のない" ネットワークへの接続が確立されたときには、転送が自動的に再開されます。 コストによってネットワークがどのように定義されるかについては、「[**NetworkCostType**](/uwp/api/Windows.Networking.Connectivity.NetworkCostType)」をご覧ください。

バックグラウンド転送機能にはネットワーク ステータスの変化に対応する独自のメカニズムがありますが、ネットワーク接続されたアプリには他にも一般的な接続の考慮事項があります。 詳しくは、「[利用できるネットワーク接続情報の活用](/previous-versions/windows/apps/hh452983(v=win.10))」をご覧ください。

> **注:**    モバイル デバイスで実行されるアプリでは、接続の種類、ローミング状態、ユーザーのデータ通信プランに基づいて転送されるデータの量を、ユーザーが監視および制限できる機能が用意されています。 このため、[**BackgroundTransferCostPolicy**](/uwp/api/Windows.Networking.BackgroundTransfer.BackgroundTransferCostPolicy) が転送が継続中であることを示す場合でも、電話でバックグラウンド転送が一時停止される可能性があります。

次の表に、電話の現在の状態に応じて、[**BackgroundTransferCostPolicy**](/uwp/api/Windows.Networking.BackgroundTransfer.BackgroundTransferCostPolicy) の各値に対して、電話でバックグラウンド転送が許可されるかどうかを示します。 [  **ConnectionCost**](/uwp/api/Windows.Networking.Connectivity.ConnectionCost) クラスを使って、電話の現在の状態を判断できます。

| デバイスの状態                                                                                                                      | UnrestrictedOnly | 既定 | 常に表示する |
|-----------------------------------------------------------------------------------------------------------------------------------|------------------|---------|--------|
| WiFi 接続                                                                                                                 | Allow            | Allow   | Allow  |
| 従量制課金接続、ローミング時以外、データ制限未満、制限内にとどまる見込み                                                   | 拒否             | Allow   | Allow  |
| 従量制課金接続、ローミング時以外、データ制限未満、制限を超過する見込み                                                       | 拒否             | 拒否    | Allow  |
| 従量制課金接続、ローミング時、データ制限未満                                                                                     | 拒否             | 拒否    | Allow  |
| 従量制課金接続、データ制限超過 この状態は、ユーザーが Data Sense UI で [バックグラウンドでのデータ通信を制限する] を有効にしている場合にのみ発生します。 | 拒否             | 拒否    | 拒否   |

## <a name="uploading-files"></a>ファイルのアップロード
バックグラウンド転送を使う場合、アップロードは [**UploadOperation**](/uwp/api/Windows.Networking.BackgroundTransfer.UploadOperation) として存在し、操作の再起動や取り消しに使われる多くの制御メソッドを公開します。 アプリのイベント (一時停止、終了など) や接続の変更は、**UploadOperation** を通じてシステムによって自動的に処理されます。アップロードは、アプリの一時停止中も続行され、アプリの終了以降は一時停止して保持されます。 また、[**CostPolicy**](/uwp/api/windows.networking.backgroundtransfer.backgrounddownloader.costpolicy) プロパティを設定することで、従量制課金接続がインターネット接続のために使われている間もアプリがアップロードを開始するかどうかを指定します。

以下に、基本的なアップロードを作成および初期化する例と、前のアプリ セッションから続いている操作を列挙および再び取り込む例を示します。

### <a name="uploading-a-single-file"></a>1 つのファイルのアップロード
アップロードの作成は、[**BackgroundUploader**](/uwp/api/Windows.Networking.BackgroundTransfer.BackgroundUploader) から始めます。 このクラスは、アプリで [**UploadOperation**](/uwp/api/Windows.Networking.BackgroundTransfer.UploadOperation) を作成する前に、そのアップロードを構成できるようにするメソッドを提供するために使われます。 次の例は、必要な [**Uri**](/uwp/api/Windows.Foundation.Uri) オブジェクトと [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) オブジェクトを使ってこれを行う方法を示しています。

**アップロードするファイルと送信先の特定**

[  **UploadOperation**](/uwp/api/Windows.Networking.BackgroundTransfer.UploadOperation) の作成を始める前に、アップロード先となる場所の URI とアップロードされるファイルを識別する必要があります。 次の例では、UI 入力からの文字列を使って *uriString* の値が設定され、[**PickSingleFileAsync**](/uwp/api/windows.storage.pickers.fileopenpicker.picksinglefileasync) 操作で返される [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) オブジェクトを使って *file* の値が設定されます。

:::code language="javascript" source="~/../snippets-windows/windows-uwp/networking/backgroundtransfer/upload_quickstart/js/main.js" id="Snippetupload_quickstart_B":::

**アップロード操作の作成と初期化**

前の手順では、*uriString* と *file* の値が次に示す例の UploadOp のインスタンスに渡されました。これらの値は、新しいアップロード操作を構成し開始するために使われます。 まず、*uriString* が解析されて、要求された [**Uri**](/uwp/api/Windows.Foundation.Uri) オブジェクトが作成されます。

そして、与えられた [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) (*file*) のプロパティが [**BackgroundUploader**](/uwp/api/Windows.Networking.BackgroundTransfer.BackgroundUploader) で使われて要求ヘッダーが設定され、*SourceFile* プロパティに **StorageFile** オブジェクトが設定されます。 次に、[**SetRequestHeader**](/uwp/api/windows.networking.backgroundtransfer.backgrounduploader.setrequestheader) メソッドが呼び出され、文字列として提供されたファイル名と [**StorageFile.Name**](/uwp/api/windows.storage.storagefile.name) プロパティが挿入されます。

最後に、[**BackgroundUploader**](/uwp/api/Windows.Networking.BackgroundTransfer.BackgroundUploader) によって [**UploadOperation**](/uwp/api/Windows.Networking.BackgroundTransfer.UploadOperation) (*upload*) が作成されます。

:::code language="javascript" source="~/../snippets-windows/windows-uwp/networking/backgroundtransfer/upload_quickstart/js/main.js" id="Snippetupload_quickstart_A":::

JavaScript の promise を使って定義した非同期メソッドの呼び出しに注意してください。 最後の例には次の行があります。

```javascript
promise = upload.startAsync().then(complete, error, progress);
```

非同期メソッドの後に then ステートメントが続いています。このステートメントでは、非同期メソッドの呼び出しの結果が返されたときに呼び出される、アプリで定義されたメソッドを指定しています。 このプログラミング パターンについて詳しくは、「[プロミスを使った JavaScript での非同期プログラミング](/previous-versions/windows)」をご覧ください。

### <a name="uploading-multiple-files"></a>複数のファイルのアップロード
**アップロードするファイルと送信先の特定**

単一の [**UploadOperation**](/uwp/api/Windows.Networking.BackgroundTransfer.UploadOperation) で複数のファイルを転送するシナリオでは、処理は通常どおり、最初に必要なアップロード先 URI とローカル ファイルの情報を指定することから始まります。 前のセクションの例と同様に、URI はエンド ユーザーが文字列で指定します。また、[**FileOpenPicker**](/uwp/api/Windows.Storage.Pickers.FileOpenPicker) を使って、ユーザー インターフェイスからファイルを指定する機能も提供できます。 ただし、このシナリオでは、アプリで代わりに [**PickMultipleFilesAsync**](/uwp/api/windows.storage.pickers.fileopenpicker.pickmultiplefilesasync) メソッドを呼び出して、UI から複数のファイルを選ぶことができるようにする必要があります。

```javascript
function uploadFiles() {
       var filePicker = new Windows.Storage.Pickers.FileOpenPicker();
       filePicker.fileTypeFilter.replaceAll(["*"]);

       filePicker.pickMultipleFilesAsync().then(function (files) {
          if (files === 0) {
             printLog("No file selected");
                return;
          }

          var upload = new UploadOperation();
          var uriString = document.getElementById("serverAddressField").value;
          upload.startMultipart(uriString, files);

          // Persist the upload operation in the global array.
          uploadOperations.push(upload);
       });
    }
```

**指定されたパラメーターに基づくオブジェクトの作成**

次の 2 つの例では、前の手順の最後に呼び出された単一のメソッド例 **startMultipart** に含まれているコードを使っています。 わかりやすくするために、[**BackgroundTransferContentPart**](/uwp/api/Windows.Networking.BackgroundTransfer.BackgroundTransferContentPart) オブジェクトの配列を作るメソッドのコードは、結果の [**UploadOperation**](/uwp/api/Windows.Networking.BackgroundTransfer.UploadOperation) を作るコードから分割されています。

最初に、ユーザーが指定した URI 文字列を [**Uri**](/uwp/api/Windows.Foundation.Uri) として初期化します。 次に、このメソッドに渡された [**IStorageFile**](/uwp/api/Windows.Storage.IStorageFile) オブジェクト (**files**) の配列を反復処理し、配列内の各オブジェクトを使って新しい [**BackgroundTransferContentPart**](/uwp/api/Windows.Networking.BackgroundTransfer.BackgroundTransferContentPart) オブジェクトを作り、そのオブジェクトを **contentParts** 配列に配置します。

```javascript
    upload.startMultipart = function (uriString, files) {
        try {
            var uri = new Windows.Foundation.Uri(uriString);
            var uploader = new Windows.Networking.BackgroundTransfer.BackgroundUploader();

            var contentParts = [];
            files.forEach(function (file, index) {
                var part = new Windows.Networking.BackgroundTransfer.BackgroundTransferContentPart("File" + index, file.name);
                part.setFile(file);
                contentParts.push(part);
            });
```

**マルチパート アップロード操作の作成と初期化**

contentParts 配列には、アップロード用の各 [**IStorageFile**](/uwp/api/Windows.Storage.IStorageFile) を表す [**BackgroundTransferContentPart**](/uwp/api/Windows.Networking.BackgroundTransfer.BackgroundTransferContentPart) オブジェクトがすべて含まれているため、要求の送信先を示す [**Uri**](/uwp/api/Windows.Foundation.Uri) を使って [**CreateUploadAsync**](/uwp/api/windows.networking.backgroundtransfer.backgrounduploader.createuploadasync) を呼び出すことができます。

```javascript
        // Create a new upload operation.
            uploader.createUploadAsync(uri, contentParts).then(function (uploadOperation) {

               // Start the upload and persist the promise to be able to cancel the upload.
               upload = uploadOperation;
               promise = uploadOperation.startAsync().then(complete, error, progress);
            });

         } catch (err) {
             displayError(err);
         }
     };
```

### <a name="restarting-interrupted-upload-operations"></a>中断されたアップロード操作の再開
[  **UploadOperation**](/uwp/api/Windows.Networking.BackgroundTransfer.UploadOperation) が完了するか取り消されると、関連するシステム リソースがすべて解放されます。 ただし、これらのイベントのどちらかが発生する前にアプリが終了した場合、アクティブな操作は一時停止され、それぞれに関連付けられているリソースは占有されたままになります。 これらの操作が列挙されずに次のアプリ セッションに再び取り込まれると、それらの操作は完了せず、デバイス リソースを占有したままとなります。

1.  持続している操作を列挙する関数を定義する前に、返される [**UploadOperation**](/uwp/api/Windows.Networking.BackgroundTransfer.UploadOperation) オブジェクトを格納する配列を作成する必要があります。

    :::code language="javascript" source="~/../snippets-windows/windows-uwp/networking/backgroundtransfer/upload_quickstart/js/main.js" id="Snippetupload_quickstart_C":::

1.  次に、持続している操作を列挙してそれらを配列に格納する関数を定義します。 [**UploadOperation**](/uwp/api/Windows.Networking.BackgroundTransfer.UploadOperation) に対するコールバックを再び割り当てるために呼び出される **load** メソッドは、アプリの終了後も持続する場合、このセクションでこの後定義する UploadOp クラス内にあることに注意してください。

    :::code language="javascript" source="~/../snippets-windows/windows-uwp/networking/backgroundtransfer/upload_quickstart/js/main.js" id="Snippetupload_quickstart_D":::

## <a name="downloading-files"></a>ファイルのダウンロード
バックグラウンド転送を使う場合、各ダウンロードは [**DownloadOperation**](/uwp/api/Windows.Networking.BackgroundTransfer.DownloadOperation) として存在し、操作の一時停止、再開、再起動、取り消しに使われる多くの制御メソッドを公開します。 アプリのイベント (一時停止、終了など) や接続の変更は、**DownloadOperation** を通じてシステムによって自動的に処理されます。ダウンロードは、アプリの一時停止中も続行され、アプリの終了以降は一時停止して保持されます。 モバイル ネットワーク シナリオの場合、[**CostPolicy**](/uwp/api/windows.networking.backgroundtransfer.backgrounddownloader.costpolicy) プロパティを設定することで、従量制課金接続がインターネット接続のために使われている間もアプリがダウンロードを開始または続行するかどうかを指定します。

すぐに完了する可能性がある小さいリソースをダウンロードする場合は、バックグラウンド転送ではなく [**HttpClient**](/uwp/api/Windows.Web.Http.HttpClient) API を使ってください。

以下に、基本的なダウンロードを作成および初期化する例と、前のアプリ セッションから続いている操作を列挙および再び取り込む例を示します。

### <a name="configure-and-start-a-background-transfer-file-download"></a>バックグラウンド転送によるファイルのダウンロードを構成して開始する
URI とファイル名を表す文字列を使って、[**Uri**](/uwp/api/Windows.Foundation.Uri) オブジェクトと要求されたファイルを格納する [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) とを作成する方法を次の例で示します。 この例では、新しいファイルが定義済みの場所に自動的に配置されます。 または、[**FileSavePicker**](/uwp/api/Windows.Storage.Pickers.FileSavePicker) を使ってユーザーがファイルを保存するデバイスの場所を指定できるようになります。 [**DownloadOperation**](/uwp/api/Windows.Networking.BackgroundTransfer.DownloadOperation) に対するコールバックを再び割り当てるために呼び出される **load** メソッドは、アプリの終了以降も持続する場合、このセクションでこの後定義する DownloadOp クラス内にあることに注意してください。

:::code language="javascript" source="~/../snippets-windows/windows-uwp/networking/backgroundtransfer/download_quickstart/js/main.js" id="Snippetdownload_quickstart_A":::

JavaScript の promise を使って定義した非同期メソッドの呼び出しに注意してください。 前のコード例の 17 行目には次のコードがあります。

```javascript
promise = download.startAsync().then(complete, error, progress);
```

非同期メソッドの後に then ステートメントが続いています。このステートメントでは、非同期メソッドの呼び出しの結果が返されたときに呼び出される、アプリで定義されたメソッドを指定しています。 このプログラミング パターンについて詳しくは、「[プロミスを使った JavaScript での非同期プログラミング](/previous-versions/windows)」をご覧ください。

### <a name="adding-additional-operation-control-methods"></a>その他の操作制御メソッドの追加
追加の [**DownloadOperation**](/uwp/api/Windows.Networking.BackgroundTransfer.DownloadOperation) メソッドを実装することによって、制御のレベルを高めることができます。 上の例に次のコードを追加すると、ダウンロードをキャンセルすることができるようになります。

:::code language="javascript" source="~/../snippets-windows/windows-uwp/networking/backgroundtransfer/download_quickstart/js/main.js" id="Snippetdownload_quickstart_B":::

### <a name="enumerating-persisted-operations-at-start-up"></a>持続している操作の起動時の列挙
[  **DownloadOperation**](/uwp/api/Windows.Networking.BackgroundTransfer.DownloadOperation) が完了するか取り消されると、関連するシステム リソースがすべて解放されます。 ただし、これらのイベントのどちらかが発生する前にアプリが終了した場合、ダウンロードは一時停止され、バックグラウンドで保持されます。 以下の例は、持続しているダウンロードを新しいアプリ セッションに再び取り込む方法を示しています。

1.  持続している操作を列挙する関数を定義する前に、返される [**DownloadOperation**](/uwp/api/Windows.Networking.BackgroundTransfer.DownloadOperation) オブジェクトを格納する配列を作成する必要があります。

    :::code language="javascript" source="~/../snippets-windows/windows-uwp/networking/backgroundtransfer/download_quickstart/js/main.js" id="Snippetdownload_quickstart_D":::

1.  次に、持続している操作を列挙してそれらを配列に格納する関数を定義します。 持続している [**DownloadOperation**](/uwp/api/Windows.Networking.BackgroundTransfer.DownloadOperation) に対するコールバックを再び割り当てるために呼び出される **load** メソッドは、このセクションでこの後定義する DownloadOp の例に含まれていることに注意してください。

    :::code language="javascript" source="~/../snippets-windows/windows-uwp/networking/backgroundtransfer/download_quickstart/js/main.js" id="Snippetdownload_quickstart_E":::

1.  これで、返された値の一覧を使って、保留中の操作を再開できます。

## <a name="post-processing"></a>後処理
Windows 10 の新機能は、アプリが実行されていない場合でも、バックグラウンド転送の完了時にアプリケーション コードを実行できる機能です。 たとえば、アプリが開始されるたびに新しいムービーをスキャンするのではなく、ムービーのダウンロードが完了した後で利用可能な映画の一覧を更新できます。 または、アプリでファイル転送が失敗した場合に、別のサーバーまたはポートを使ってもう一度転送し直すことができます。 後処理は成功した転送と失敗した転送の両方で呼び出されるため、これを使って、カスタム エラー処理と再試行ロジックを実装できます。

後処理では、既存のバックグラウンド タスク インフラストラクチャを使います。 転送を開始する前に、バックグラウンド タスクを作成して転送に関連付けます。 転送はバックグラウンドで実行され、完了時にバックグラウンド タスクが呼び出されて後処理が実行されます。

後処理では、[**BackgroundTransferCompletionGroup**](/uwp/api/Windows.Networking.BackgroundTransfer.BackgroundTransferCompletionGroup) という新しいクラスを使います。 このクラスは、バックグラウンド タスクをグループ化できるという点で既存の [**BackgroundTransferGroup**](/uwp/api/Windows.Networking.BackgroundTransfer.BackgroundTransferGroup) に似ていますが、**BackgroundTransferCompletionGroup** には、転送の完了時に実行するバックグラウンド タスクを指定できる機能が追加されています。

後処理があるバックグラウンド転送は、次のように開始します。

1.  [  **BackgroundTransferCompletionGroup**](/uwp/api/Windows.Networking.BackgroundTransfer.BackgroundTransferCompletionGroup) オブジェクトを作成します。 続けて、[**BackgroundTaskBuilder**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) オブジェクトを作成します。 ビルダー オブジェクトの **Trigger** プロパティを完了グループ オブジェクトに設定し、ビルダーの **TaskEntryPoint** プロパティを、転送完了時に実行する必要があるバックグラウンド タスクのエントリ ポイントに設定します。 最後に、[**BackgroundTaskBuilder.Register**](/uwp/api/windows.applicationmodel.background.backgroundtaskbuilder.register) メソッドを呼び出してバックグラウンド タスクを登録します。 複数の完了グループで 1 つのバックグラウンド タスクのエントリ ポイントを共有できますが、バックグラウンド タスクの登録では 1 つの完了グループのみ指定できることに注意してください。

```csharp
var completionGroup = new BackgroundTransferCompletionGroup();
BackgroundTaskBuilder builder = new BackgroundTaskBuilder();

builder.Name = "MyDownloadProcessingTask";
builder.SetTrigger(completionGroup.Trigger);
builder.TaskEntryPoint = "Tasks.BackgroundDownloadProcessingTask";

BackgroundTaskRegistration downloadProcessingTask = builder.Register();
```

2.  次に、バックグラウンド転送を完了グループに関連付けます。 すべての転送を作成したら、完了グループを有効にします。

```csharp
BackgroundDownloader downloader = new BackgroundDownloader(completionGroup);
DownloadOperation download = downloader.CreateDownload(uri, file);
Task<DownloadOperation> startTask = download.StartAsync().AsTask();

// App still sees the normal completion path
startTask.ContinueWith(ForegroundCompletionHandler);

// Do not enable the CompletionGroup until after all downloads are created.
downloader.CompletinGroup.Enable();
```

3.  バックグラウンド タスク内のコードで、トリガーの詳細情報から操作の一覧を抽出した後、各操作の詳細を検査し、操作ごとに適切な後処理を実行できます。

```csharp
public class BackgroundDownloadProcessingTask : IBackgroundTask
{
    public async void Run(IBackgroundTaskInstance taskInstance)
    {
    var details = (BackgroundTransferCompletionGroupTriggerDetails)taskInstance.TriggerDetails;
    IReadOnlyList<DownloadOperation> downloads = details.Downloads;

    // Do post-processing on each finished operation in the list of downloads
    }
}
```

後処理タスクは、通常のバックグラウンド タスクです。 それはすべてのバックグラウンド タスクのプールの一部であり、すべてのバックグラウンド タスクと同じリソース管理ポリシーが適用されます。

後処理はフォアグラウンド完了ハンドラーに代わるものではないことにも注意してください。 アプリにフォアグラウンド完了ハンドラーが定義されているときに、ファイル転送の完了時にアプリが実行されている場合は、フォアグラウンド完了ハンドラーとバックグラウンド完了ハンドラーの両方が呼び出されます。 フォアグラウンド タスクとバックグラウンド タスクが呼び出される順序は保証されません。 両方を定義する場合は、2 つのタスクが正常に動作し、同時に実行されても相互に干渉しないことを確認する必要があります。

## <a name="request-timeouts"></a>要求のタイムアウト
次の 2 つの主要接続タイムアウト シナリオを考慮する必要があります。

-   転送のために新しい接続を確立する場合、5 分以内に接続が確立しないと、接続要求は中止されます。

-   接続が確立された後、2 分以内で応答を受け取らなかった HTTP 要求メッセージは中止されます。

> **注:**    どちらのシナリオにおいても、バックグラウンド転送はインターネット接続があることを前提に、最高 3 回まで自動的に要求を再試行します。 インターネット接続が検出されないと、検出されるまで別の要求は待機します。

## <a name="debugging-guidance"></a>デバッグのガイダンス
Microsoft Visual Studio でデバッグ セッションを停止することは、アプリを閉じることに相当します。PUT によるアップロードは一時停止され、POST によるアップロードは終了されます。 デバッグ時であっても、アプリでは、持続しているアップロードを列挙し、再実行や取り消しを行うことができる必要があります。 たとえば、そのデバッグ セッションで以前の操作が重要ではない場合、アプリの起動時に、列挙された持続しているアップロード操作をアプリで取り消すことができます。

デバッグ セッションでアプリの起動時にダウンロードやアップロードを列挙する際、そのデバッグ セッションで以前の操作が重要ではない場合、列挙された操作をアプリで取り消すことができます。 アプリ マニフェストの変更など、Visual Studio プロジェクトの更新があり、アプリがアンインストールされ、もう一度展開された場合、[**GetCurrentUploadsAsync**](/uwp/api/windows.networking.backgroundtransfer.backgrounduploader.getcurrentuploadsasync) は、前のアプリの展開を使って作成された操作を列挙できないことに注意してください。

開発時にバックグラウンド転送を使うと、完了したアクティブな転送操作の内部キャッシュが同期しなくなる状況が発生する可能性があります。このため、新しい転送操作を開始できない場合や、既存の操作や [**BackgroundTransferGroup**](/uwp/api/Windows.Networking.BackgroundTransfer.BackgroundTransferGroup) オブジェクトを処理できない場合があります。 状況によっては、既存の操作を処理しようとすると、クラッシュの原因となる可能性があります。 これは、[**TransferBehavior**](/uwp/api/windows.networking.backgroundtransfer.backgroundtransfergroup.transferbehavior) プロパティが **Parallel** に設定されている場合に発生する可能性があります。 この問題は、開発中に特定のシナリオでのみ発生し、アプリのエンド ユーザーには適用されません。

Visual Studio を使う 4 つのシナリオで、この問題が発生する可能性があります。

-   既にあるプロジェクトと同じアプリ名を持つ新しいプロジェクトを、別の言語で作成する (C++ から C# など)。
-   既にあるプロジェクトのターゲット アーキテクチャを変更する (x86 から x64 など)。
-   既にあるプロジェクトのカルチャを変更する (ニュートラルから en-US など)。
-   既にあるプロジェクトのパッケージ マニフェストで機能を追加または削除する (**エンタープライズ認証**を追加するなど)。

機能を追加または削除するマニフェストの更新など、通常のアプリのサービスでは、アプリのエンド ユーザーに対する展開でこの問題は発生しません。
この問題を回避するには、アプリのすべてのバージョンを完全にアンインストールし、新しい言語、アーキテクチャ、カルチャ、または機能をもう一度展開します。 この操作は、**スタート**画面で行うか、PowerShell と **Remove-AppxPackage** コマンドレットを使って行うことができます。

## <a name="exceptions-in-windowsnetworkingbackgroundtransfer"></a>Windows.Networking.BackgroundTransfer の例外
Uniform Resource Identifier (URI) として無効な文字列が、[**Windows.Foundation.Uri**](/uwp/api/Windows.Foundation.Uri) オブジェクトのコンストラクターに渡されると、例外がスローされます。

**.NET:** [**Windows.Foundation.Uri**](/uwp/api/Windows.Foundation.Uri) 型は、C# や VB では [**System.Uri**](/dotnet/api/system.uri) と表示されます。

C# と Visual Basic では、.NET 4.5 の [**System.Uri**](/dotnet/api/system.uri) クラスと、いずれかの [**System.Uri.TryCreate**](/dotnet/api/system.uri.trycreate#overloads) メソッドを使って、URI が作成される前にアプリのユーザーから受け取った文字列をテストすることによって、このエラーを回避できます。

C++ では、URI として渡される文字列を試行して解析するメソッドはありません。 アプリがユーザーから [**Windows.Foundation.Uri**](/uwp/api/Windows.Foundation.Uri) の入力を取得する場合、このコンストラクターを try/catch ブロックに配置する必要があります。 例外がスローされた場合、アプリは、ユーザーに通知し、新しいホスト名を要求することができます。

[  **Windows.Networking.backgroundTransfer**](/uwp/api/Windows.Networking.BackgroundTransfer) 名前空間には便利なヘルパー メソッドがあり、[**Windows.Networking.Sockets**](/uwp/api/Windows.Networking.Sockets) 名前空間の列挙値を使ってエラーを処理します。 これは、アプリで特定のネットワーク例外を異なる方法で処理する場合に役立つことがあります。

[  **Windows.Networking.backgroundTransfer**](/uwp/api/Windows.Networking.BackgroundTransfer) 名前空間の非同期メソッドで発生したエラーは、**HRESULT** 値として返されます。 [  **BackgroundTransferError.GetStatus**](/uwp/api/windows.networking.backgroundtransfer.backgroundtransfererror.getstatus) メソッドは、バックグラウンド転送操作からのネットワーク エラーを [**WebErrorStatus**](/uwp/api/Windows.Web.WebErrorStatus) 列挙値に変換するために使われます。 **WebErrorStatus** 列挙値のほとんどは、ネイティブ HTTP または FTP クライアント操作から返されるエラーに対応しています。 アプリは特定の **WebErrorStatus** 列挙値に対するフィルター処理を行い、例外の原因に応じてアプリの動作を変更できます。

パラメーター検証エラーの場合は、例外の **HRESULT** を使って、その例外の原因となったエラーの詳細情報を確認することもできます。 使うことができる **HRESULT** 値は、*Winerror.h* ヘッダー ファイルに記載されています。 ほとんどのパラメーター検証エラーの場合、返される **HRESULT** は **E\_INVALIDARG** です。

## <a name="important-apis"></a>重要な API
* [**Windows.Networking.BackgroundTransfer**](/uwp/api/windows.networking.backgroundtransfer)
* [**Windows.Foundation.Uri**](/uwp/api/Windows.Foundation.Uri)
* [**Windows.Networking.Sockets**](/uwp/api/Windows.Networking.Sockets)
