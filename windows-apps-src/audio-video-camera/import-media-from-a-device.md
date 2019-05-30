---
ms.assetid: dd2a1e01-c284-4d62-963e-f59f58dca61a
description: この記事では、利用可能なメディア ソースの検索、写真やサイドカー ファイルなどのファイルのインポート、ソース デバイスからインポートしたファイルの削除など、デバイスからメディアをインポートする方法について説明します。
title: メディアのインポート
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 7ec16f9810a0802fc319b3ebac679cc4cb2f8334
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/29/2019
ms.locfileid: "66361722"
---
# <a name="import-media-from-a-device"></a>デバイスからのメディアのインポート

この記事では、利用可能なメディア ソースの検索、ビデオ、写真、サイドカー ファイルなどのファイルのインポート、ソース デバイスからインポートしたファイルの削除など、デバイスからメディアをインポートする方法について説明します。

> [!NOTE] 
> この記事のコードは、[**MediaImport UWP アプリ サンプル**](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MediaImport)を基にしています。 このサンプルを[**ユニバーサル Windows アプリ サンプル Git リポジトリ**](https://github.com/Microsoft/Windows-universal-samples)から複製またはダウンロードすると、コンテキスト内のコードを確認できます。独自のアプリの出発点として使うこともできます。

## <a name="create-a-simple-media-import-ui"></a>シンプルなメディア インポート UI の作成
この記事の例では、メディア インポートのコア シナリオを実現する最小限の UI を使用します。 メディア インポート アプリ用のより強力な UI を作成する方法については、[**MediaImport サンプル**](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MediaImport)をご覧ください。 次の XAML では、次のコントロールを持つスタック パネルを作成します。
* メディアをインポートできるソースの検索を開始するための [**Button**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button)。
* 見つかったメディア インポート ソースを一覧にして選択するための [**ComboBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ComboBox)。
* 選択したインポート ソースのメディア項目を表示して選択するための [**ListView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) コントロール。
* 選択したソースからメディア項目のインポートを開始するための **Button**。
* 選択したソースからインポートされた項目の削除を開始するための **Button**。
* 非同期メディア インポート操作を取り消すための **Button**。

[!code-xml[ImportXAML](./code/PhotoImport_Win10/cs/MainPage.xaml#SnippetImportXAML)]

## <a name="set-up-your-code-behind-file"></a>分離コード ファイルの設定
*using* ディレクティブを追加して、既定のプロジェクト テンプレートにまだ含まれていない、この例で使用する名前空間を含めます。

[!code-cs[Using](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetUsing)]

## <a name="set-up-task-cancellation-for-media-import-operations"></a>メディア インポート操作のタスク取り消しの設定

メディア インポート操作には長い時間がかかる場合があるため、操作は [**IAsyncOperationWithProgress**](https://docs.microsoft.com/uwp/api/Windows.Foundation.IAsyncOperationWithProgress_TResult_TProgress_) を使用して非同期で実行します。 ユーザーがキャンセル ボタンをクリックした場合に、実行中の操作の取り消しに使用する、[**CancellationTokenSource**](https://docs.microsoft.com/dotnet/api/system.threading.cancellationtokensource?redirectedfrom=MSDN) 型のクラス メンバー変数を宣言します。

[!code-cs[DeclareCts](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetDeclareCts)]

キャンセル ボタンのハンドラーの実装 この記事の後で示す例では、**CancellationTokenSource** を操作が始まったときに初期化して、操作が完了したときに null に設定します。 このキャンセル ボタン ハンドラーでは、トークンが null かどうかを確認し、null でない場合は [**Cancel**](https://docs.microsoft.com/dotnet/api/system.threading.cancellationtokensource.cancel?redirectedfrom=MSDN#System_Threading_CancellationTokenSource_Cancel) を呼び出して操作を取り消します。

[!code-cs[OnCancel](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetOnCancel)]

## <a name="data-binding-helper-classes"></a>データ バインディング ヘルパー クラス

一般的なメディア インポート シナリオでは、インポートできるメディア項目の一覧をユーザーに提示します。選択するメディア ファイルの数は非常に多い可能性があり、通常、各メディア項目のサムネイルを表示します。 このため、この例では 3 つのヘルパー クラスを使用して、ユーザーが一覧をスクロールしていくと ListView コントロールにエントリが段階的に読み込まれるようにします。

* **IncrementalLoadingBase** クラス - [**IList**](https://docs.microsoft.com/dotnet/api/system.collections.ilist?redirectedfrom=MSDN)、[**ISupportIncrementalLoading**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.isupportincrementalloading)、および [**INotifyCollectionChanged**](https://msdn.microsoft.com/library/windows/apps/system.collections.specialized.inotifycollectionchanged(v=vs.105).aspx) を実装し、段階的な読み込みの基本動作を提供します。
* **GeneratorIncrementalLoadingClass** クラス - 段階的な読み込みの基底クラスの実装を提供します。
* **ImportableItemWrapper** クラス - [**PhotoImportItem**](https://docs.microsoft.com/uwp/api/Windows.Media.Import.PhotoImportItem) クラスのシン ラッパーで、インポートされる各項目のサムネイル イメージに対するバインディング可能な [**BitmapImage**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.BitmapImage) プロパティを追加します。

これらのクラスは [**MediaImport サンプル**](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MediaImport)に用意されており、変更なしでプロジェクトに追加できます。 ヘルパー クラスをプロジェクトに追加したら、後でこの例で使用する**GeneratorIncrementalLoadingClass** 型のクラス メンバー変数を宣言します。

[!code-cs[GeneratorIncrementalLoadingClass](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetGeneratorIncrementalLoadingClass)]


## <a name="find-available-sources-from-which-media-can-be-imported"></a>メディアをインポートできる利用可能なソースを見つける

ソース検索ボタンのクリック ハンドラーで、静的メソッド [**PhotoImportManager.FindAllSourcesAsync**](https://docs.microsoft.com/uwp/api/windows.media.import.photoimportmanager.findallsourcesasync) を呼び出し、メディアをインポート可能なデバイスのシステム検索を開始します。 操作の完了を待機した後、返される一覧内の各 [**PhotoImportSource**](https://docs.microsoft.com/uwp/api/Windows.Media.Import.PhotoImportSource) オブジェクトをループ処理し、エントリを **ComboBox** に追加します。ソース オブジェクト自体に **Tag** プロパティを設定して、ユーザーが選択するときに簡単にソース オブジェクトを取得できるようにします。

[!code-cs[FindSourcesClick](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetFindSourcesClick)]

ユーザーが選択したインポート ソースを格納するクラス メンバー変数を宣言します。

[!code-cs[DeclareImportSource](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetDeclareImportSource)]

インポート ソース **ComboBox** の [**SelectionChanged**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) ハンドラーで、クラス メンバー変数を選択したソースに設定します。その後、この記事の広範で示す **FindItems** ヘルパー メソッドを呼び出します。 

[!code-cs[SourcesSelectionChanged](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetSourcesSelectionChanged)]

## <a name="find-items-to-import"></a>インポートする項目を見つける

この後の手順で使用する [**PhotoImportSession**](https://docs.microsoft.com/uwp/api/Windows.Media.Import.PhotoImportSession) 型および [**PhotoImportFindItemsResult**](https://docs.microsoft.com/uwp/api/Windows.Media.Import.PhotoImportFindItemsResult) 型のクラス メンバー 変数を追加します。

[!code-cs[DeclareImport](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetDeclareImport)]

**FindItems** メソッドで、C**CancellationTokenSource** 変数を初期化して、必要に応じて検索操作の取り消しに使用できるようにします。 **try** ブロック内で、ユーザーが選択した [**PhotoImportSource**](https://docs.microsoft.com/uwp/api/Windows.Media.Import.PhotoImportSource) オブジェクトで [**CreateImportSession**](https://docs.microsoft.com/uwp/api/windows.media.import.photoimportsource.createimportsession) を呼び出して、新しいインポート セッションを作成します。 検索操作の進行状況を表示するためのコールバックを提供するために、新しい [**Progress**](https://docs.microsoft.com/dotnet/api/system.progress-1?redirectedfrom=MSDN) オブジェクトを作成します。 次に、 **[FindItemsAsync](https://docs.microsoft.com/uwp/api/windows.media.import.photoimportsession.finditemsasync)** を呼び出して、検索操作を開始します。 [  **PhotoImportContentTypeFilter**](https://docs.microsoft.com/uwp/api/Windows.Media.Import.PhotoImportContentTypeFilter) の値を提供して、写真、ビデオ、またはその両方を返す必要があるかどうかを指定します。 [  **PhotoImportItemSelectionMode**](https://docs.microsoft.com/uwp/api/Windows.Media.Import.PhotoImportItemSelectionMode) の値を提供して、[**IsSelected**](https://docs.microsoft.com/uwp/api/windows.media.import.photoimportitem.isselected) プロパティが true に設定されているときに、すべてのメディア項目を返すのか、メディア項目を何も返さないのか、新しいメディア項目のみを返すのかを指定します。 このプロパティは、ListBox 項目テンプレートの、各メディア項目のチェック ボックスにバインドされます。

**FindItemsAsync** は [**IAsyncOperationWithProgress**](https://docs.microsoft.com/uwp/api/Windows.Foundation.IAsyncOperationWithProgress_TResult_TProgress_) を返します。 拡張メソッド [**AsTask**](https://docs.microsoft.com/dotnet/api/system?redirectedfrom=MSDN) は、待機可能で、キャンセル トークンを使用して取り消し可能であり、提供された **Progress** オブジェクトを使用して進行状況を報告するタスクの作成に使用されます。

次に、データ バインディング ヘルパー クラス **GeneratorIncrementalLoadingClass** が初期化されます。 **FindItemsAsync** は、待機から返されると、[**PhotoImportFindItemsResult**](https://docs.microsoft.com/uwp/api/Windows.Media.Import.PhotoImportFindItemsResult) オブジェクトを返します。 このオブジェクトには、操作の成功、見つかったさまざまな種類のメディア項目の数など、検索操作の状態情報が含まれます。 [  **FoundItems**](https://docs.microsoft.com/uwp/api/windows.media.import.photoimportfinditemsresult.founditems) プロパティには、見つかったメディア項目を表す [**PhotoImportItem**](https://docs.microsoft.com/uwp/api/Windows.Media.Import.PhotoImportItem) オブジェクトの一覧が含まれます。 **GeneratorIncrementalLoadingClass** コンストラクターは、段階的に読み込まれる項目の合計数と、必要に応じて読み込まれる新規項目を生成する関数を引数として受け取ります。 この場合、指定されたラムダ式によって、**ImportableItemWrapper** の新しいインスタンスが作成されます。これは、**PhotoImportItem** をラップして各項目のサムネイルを含めます。 段階的読み込みクラスが初期化されたら、それを UI に含まれる **ListView** コントロールの [**ItemsSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) プロパティに設定します。 これで、見つかったメディア項目が段階的に読み込まれ、一覧に表示されます。

次に、検索操作の状態情報が出力されます。 一般的なアプリでは、この情報が UI でユーザーに表示されます。しかし、この例では、単純に情報をデバッグ コンソールに出力します。 最後に、操作が完了したのでキャンセル トークンを null に設定します。

[!code-cs[FindItems](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetFindItems)]

## <a name="import-media-items"></a>メディア項目のインポート

インポート操作を実装する前に、インポート操作の結果を格納する [**PhotoImportImportItemsResult**](https://docs.microsoft.com/uwp/api/Windows.Media.Import.PhotoImportImportItemsResult) オブジェクトを宣言します。 これは後で、ソースから正常にインポートされたメディア項目を削除するために使用します。

[!code-cs[DeclareImportResult](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetDeclareImportResult)]

メディアのインポート操作を開始する前に、**CancellationTokenSource** 変数を初期化し、[**ProgressBar**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ProgressBar) コントロールの値を 0 に設定します。

**ListView** コントロールで選択されている項目がない場合、インポートするものはありません。 それ以外の場合は、進行状況バー コントロールを更新する進行状況コールバックを提供するために、[**Progress**](https://docs.microsoft.com/dotnet/api/system.progress-1?redirectedfrom=MSDN) オブジェクトを初期化します。 検索操作によって返される [**PhotoImportFindItemsResult**](https://docs.microsoft.com/uwp/api/Windows.Media.Import.PhotoImportFindItemsResult) の [**ItemImported**](https://docs.microsoft.com/uwp/api/windows.media.import.photoimportfinditemsresult.itemimported) イベント用のハンドラーを登録します。 このイベントは、この例の場合、項目がインポートされ、インポートされた各ファイルの名前がデバッグ コンソールに出力されるときに発生します。

[  **ImportItemsAsync**](https://docs.microsoft.com/uwp/api/windows.media.import.photoimportfinditemsresult.importitemsasync) を呼び出して、インポート操作を開始します。 検索操作と同様に、[**AsTask**](https://docs.microsoft.com/dotnet/api/system?redirectedfrom=MSDN) 拡張メソッドは、返された操作を、待機可能で、進行状況を報告し、取り消し可能なタスクに変換するために使用されます。

インポート操作が完了したら、[**ImportItemsAsync**](https://docs.microsoft.com/uwp/api/windows.media.import.photoimportfinditemsresult.importitemsasync) によって返される [**PhotoImportImportItemsResult**](https://docs.microsoft.com/uwp/api/Windows.Media.Import.PhotoImportImportItemsResult) オブジェクトから操作の状態を取得できます。 この例では、状態情報をデバッグ コンソールに出力して、最後にキャンセル トークンを null に設定します。

[!code-cs[ImportClick](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetImportClick)]

## <a name="delete-imported-items"></a>インポートされた項目の削除
ソースから正常にインポートされた項目をインポート元から削除するには、まず、削除操作を取り消せるようにキャンセル トークンを初期化して、進行状況バーの値を 0 に設定します。 [  **ImportItemsAsync**](https://docs.microsoft.com/uwp/api/windows.media.import.photoimportfinditemsresult.importitemsasync) から返される [**PhotoImportImportItemsResult**](https://docs.microsoft.com/uwp/api/Windows.Media.Import.PhotoImportImportItemsResult) が null でないことを確認します。 そうでない場合は、削除操作の進行状況コールバックを提供するために、[**Progress**](https://docs.microsoft.com/dotnet/api/system.progress-1?redirectedfrom=MSDN) オブジェクトをもう一度作成します。 [  **DeleteImportedItemsFromSourceAsync**](https://docs.microsoft.com/uwp/api/windows.media.import.photoimportimportitemsresult.deleteimporteditemsfromsourceasync) を呼び出して、インポートされた項目の削除を開始します。 **AsTask** を使用して、進行状況の報告機能と取り消し機能を持った待機可能なタスクに結果を変換します。 待機後、返された [**PhotoImportDeleteImportedItemsFromSourceResult**](https://docs.microsoft.com/uwp/api/Windows.Media.Import.PhotoImportDeleteImportedItemsFromSourceResult) オブジェクトを使用して、削除操作に関する状態情報を取得および表示できます。

[!code-cs[DeleteClick](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetDeleteClick)]








 


